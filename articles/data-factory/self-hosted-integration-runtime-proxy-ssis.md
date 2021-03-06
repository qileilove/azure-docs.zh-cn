---
title: 将自承载集成运行时配置为 SSIS 的代理
description: 了解如何将自承载集成运行时配置为 Azure-SSIS Integration Runtime 的代理。
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: mflasko
ms.custom: seo-lt-2019
ms.date: 11/19/2020
ms.openlocfilehash: a79055a77ec73ce2b267bb4f16fa91f37e22ea75
ms.sourcegitcommit: f6236e0fa28343cf0e478ab630d43e3fd78b9596
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/19/2020
ms.locfileid: "94916774"
---
# <a name="configure-a-self-hosted-ir-as-a-proxy-for-an-azure-ssis-ir-in-azure-data-factory"></a>自承载 IR 配置为 Azure 数据工厂中 Azure-SSIS IR 的代理

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

本文介绍如何在将某个自承载集成运行时（自承载 IR）配置为代理的情况下，在 Azure 数据工厂中的 Azure-SSIS Integration Runtime (Azure-SSIS IR) 上运行 SQL Server Integration Services (SSIS) 包。 

使用此功能可在本地访问数据，而无需[将 Azure-SSIS IR 加入虚拟网络](./join-azure-ssis-integration-runtime-virtual-network.md)。 当企业网络的配置过于复杂，或者采用过于严格的策略，以致你很难在此网络中注入 Azure-SSIS IR 时，此功能将很有用。

此功能可将 SSIS 数据流任务分解为两个暂存任务（如果适用）： 
* **本地暂存任务**：此任务运行连接到自承载 IR 上本地数据存储的数据流组件。 它在本地数据存储与 Azure Blob 存储中的暂存区域之间来回移动数据。
* **云暂存任务**：此任务运行无法连接到 Azure-SSIS IR 上本地数据存储的数据流组件。 它在 Azure Blob 存储中的暂存区域与云数据存储之间来回移动数据。

如果数据流任务将数据从本地移动到云，则第一个和第二个暂存任务将分别为本地暂存任务和云暂存任务。 如果数据流任务将数据从云移动到本地，则第一个和第二个暂存任务将分别为云暂存任务和本地暂存任务。 如果数据流任务将数据从本地移动到本地，则第一个和第二个暂存任务将皆为本地暂存任务。 如果数据流任务将数据从云移动到云，则此功能不适用。

举例而言，此功能的其他优势和功能使你能够在 Azure-SSIS IR 尚不支持的区域中设置自承载 IR，并在数据源的防火墙中允许自承载 IR 的公共静态 IP 地址。

## <a name="prepare-the-self-hosted-ir"></a>准备自承载 IR

若要使用此功能，请先创建一个数据工厂，然后在其中设置 Azure-SSIS IR。 如果尚未执行此操作，请按照[设置 Azure-SSIS IR](./tutorial-deploy-ssis-packages-azure.md) 中的说明操作。

然后，在设置了 Azure-SSIS IR 的同一数据工厂中设置自承载 IR。 为此，请参阅[创建自承载 IR](./create-self-hosted-integration-runtime.md)。

最后，按如下所述，在本地计算机或 Azure 虚拟机 (VM) 上下载并安装最新版本的自承载 IR 以及其他驱动程序和运行时：
- 下载并安装最新版本的[自承载 IR](https://www.microsoft.com/download/details.aspx?id=39717)。
- 如果 (OLEDB) 使用对象链接和嵌入数据库，请在包中打开数据库连接 (ODBC) 或 ADO.NET 连接器，在安装了自承载 IR 的同一台计算机上下载并安装相关的驱动程序（如果尚未这样做）。  

  如果使用早期版本的用于 SQL Server 的 OLEDB 驱动程序 (SQL Server Native Client [SQLNCLI])，请[下载 64 位版本](https://www.microsoft.com/download/details.aspx?id=50402)。  

  如果使用最新版本的用于 SQL Server 的 OLEDB 驱动程序 (MSOLEDBSQL)，请[下载 64 位版本](https://www.microsoft.com/download/details.aspx?id=56730)。  
  
  如果使用 OLEDB/ODBC/ADO .NET 驱动程序，则可从其网站下载64位版本的其他数据库系统，如 PostgreSQL、MySQL、Oracle 等。
- 如果尚未这样做，请在安装了自承载 IR 的同一台计算机上[下载并安装 64 位版本的 Visual C++ (VC) 运行时](https://www.microsoft.com/download/details.aspx?id=40784)。

### <a name="enable-windows-authentication-for-on-premises-staging-tasks"></a>为本地临时任务启用 Windows 身份验证

如果自承载 IR 上的本地暂存任务需要 Windows 身份验证，请[将 SSIS 包配置为使用相同的 Windows 身份验证](/sql/integration-services/lift-shift/ssis-azure-connect-with-windows-auth?view=sql-server-ver15)。 

将使用自承载 IR 服务帐户（默认为 NT SERVICE\DIAHostService）调用本地暂存任务，将使用 Windows 身份验证帐户访问数据存储。 需要向这两个帐户分配特定的安全策略。 在自承载 IR 计算机上，转到“本地安全策略” > “本地策略” > “用户权限分配”，然后执行以下操作：   

1. 向自承载 IR 服务帐户分配“调整进程的内存配额”和“替换进程级令牌”策略。   使用默认服务帐户安装自承载 IR 时，系统应会自动进行此分配。 如果系统没有自动分配，请手动分配这些策略。 如果使用其他服务帐户，则为其分配相同的策略。

1. 向 Windows 身份验证帐户分配“作为服务登录”策略。 

## <a name="prepare-the-azure-blob-storage-linked-service-for-staging"></a>为暂存准备 Azure Blob 存储链接服务

如果尚未执行此操作，请在设置了 Azure-SSIS IR 的同一数据工厂中创建 Azure Blob 存储链接服务。 为此，请参阅[创建 Azure 数据工厂链接服务](./quickstart-create-data-factory-portal.md#create-a-linked-service)。 确保执行以下操作：
- 对于“数据存储”，请选择“Azure Blob 存储”。    
- 对于“通过集成运行时连接”  ，请选择“AutoResolveIntegrationRuntime”  （而非你的 Azure-SSIS IR 或自承载 IR），因为我们使用默认的 Azure IR 来获取 Azure Blob 存储的访问凭据。
- 对于 **身份验证方法**，请选择 **帐户密钥**、 **SAS URI**、 **服务主体** 或 **托管标识**。  

>[!TIP]
>如果选择 **服务主体** 方法，请至少向服务主体授予 " *存储 Blob 数据参与者* " 角色。 有关详细信息，请参阅 [Azure Blob 存储连接器](connector-azure-blob-storage.md#linked-service-properties)。 如果选择 **托管标识** 方法，请向 ADF 托管标识授予适当的角色以访问 Azure Blob 存储。 有关详细信息，请参阅 [使用 Azure Active Directory 身份验证和 ADF 托管标识访问 Azure Blob 存储](https://docs.microsoft.com/sql/integration-services/connection-manager/azure-storage-connection-manager?view=sql-server-ver15#managed-identities-for-azure-resources-authentication)。

![准备用于暂存的 Azure Blob 存储链接服务](media/self-hosted-integration-runtime-proxy-ssis/shir-azure-blob-storage-linked-service.png)

## <a name="configure-an-azure-ssis-ir-with-your-self-hosted-ir-as-a-proxy"></a>配置使用自承载 IR 作为代理的 Azure-SSIS IR 配置

准备好用于暂存的自承载 IR 和 Azure Blob 存储链接服务后，接下来可以在数据工厂门户或应用中，配置使用自承载 IR 作为代理的新的或现有 Azure-SSIS IR。 不过，在执行此操作之前，如果现有的 Azure-SSIS IR 已运行，请停止再重启它。

1. 在“集成运行时设置”窗格中，选择“下一步”跳过“常规设置”和“SQL 设置”部分。     

1. 在“高级设置”部分执行以下操作： 

   1. 选中“将自承载集成运行时设置为 Azure-SSIS Integration Runtime 的代理”复选框。  

   1. 在“自承载集成运行时”下拉列表中，选择现有的自承载 IR 作为 Azure-SSIS IR 的代理。 

   1. 在“暂存存储链接服务”下拉列表中，选择现有的 Azure Blob 存储链接服务，或创建新的服务用于暂存。 

   1. 在“暂存路径”框中，指定所选 Azure Blob 存储帐户中的某个 Blob 容器，或将其留空以使用默认容器进行暂存。 

   1. 选择“继续”。 

   ![自承载 IR 的高级设置](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings-shir.png)

还可以使用 PowerShell 来配置使用自承载 IR 作为代理的新的或现有 Azure-SSIS IR。

```powershell
$ResourceGroupName = "[your Azure resource group name]"
$DataFactoryName = "[your data factory name]"
$AzureSSISName = "[your Azure-SSIS IR name]"
# Self-hosted integration runtime info - This can be configured as a proxy for on-premises data access 
$DataProxyIntegrationRuntimeName = "" # OPTIONAL to configure a proxy for on-premises data access 
$DataProxyStagingLinkedServiceName = "" # OPTIONAL to configure a proxy for on-premises data access 
$DataProxyStagingPath = "" # OPTIONAL to configure a proxy for on-premises data access 

# Add self-hosted integration runtime parameters if you configure a proxy for on-premises data accesss
if(![string]::IsNullOrEmpty($DataProxyIntegrationRuntimeName) -and ![string]::IsNullOrEmpty($DataProxyStagingLinkedServiceName))
{
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
        -DataFactoryName $DataFactoryName `
        -Name $AzureSSISName `
        -DataProxyIntegrationRuntimeName $DataProxyIntegrationRuntimeName `
        -DataProxyStagingLinkedServiceName $DataProxyStagingLinkedServiceName

    if(![string]::IsNullOrEmpty($DataProxyStagingPath))
    {
        Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
            -DataFactoryName $DataFactoryName `
            -Name $AzureSSISName `
            -DataProxyStagingPath $DataProxyStagingPath
    }
}
Start-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
    -DataFactoryName $DataFactoryName `
    -Name $AzureSSISName `
    -Force
```

## <a name="enable-ssis-packages-to-connect-by-proxy"></a>使 SSIS 包能够通过代理进行连接

使用最新 SSDT 作为 Visual Studio 的 SSIS 项目扩展或独立安装程序，可以发现支持的数据流组件的连接管理器中已添加一个新的 `ConnectByProxy` 属性。
* [下载适用于 Visual Studio 的 SSIS 项目扩展](https://marketplace.visualstudio.com/items?itemName=SSIS.SqlServerIntegrationServicesProjects)
* [下载独立安装程序](/sql/ssdt/download-sql-server-data-tools-ssdt?view=sql-server-2017#ssdt-for-vs-2017-standalone-installer)   

使用可在本地访问数据的组件设计包含数据流任务的新包时，可以通过在相关连接管理器的“属性”窗格中，将此属性设置为 True 来启用此属性。

![启用 ConnectByProxy 属性](media/self-hosted-integration-runtime-proxy-ssis/shir-connection-manager-properties.png)

还可以在运行现有包时启用此属性，而无需逐个手动更改其设置。  有两个选项：
- **选项 A**：打开、重新生成并重新部署包含这些包的项目。这些包中包含要在 Azure-SSIS IR 上运行的最新 SSDT。 然后，可以针对相关的连接管理器，通过将此属性设置为 *True* 来启用此属性。 从 SSMS 运行包时，这些连接管理器将显示在“执行包”弹出窗口的“连接管理器”选项卡上。 

  ![启用 ConnectByProxy 属性 2](media/self-hosted-integration-runtime-proxy-ssis/shir-connection-managers-tab-ssms.png)

  在数据工厂管道中运行包时，对于[执行 SSIS 包活动](./how-to-invoke-ssis-package-ssis-activity.md)的“连接管理器”选项卡上显示的相关连接管理器，还可以通过将此属性设置为 True 来启用此属性。
  
  ![启用 ConnectByProxy 属性 3](media/self-hosted-integration-runtime-proxy-ssis/shir-connection-managers-tab-ssis-activity.png)

- **选择 B**：重新部署包含这些包的项目，以在 SSIS IR 上运行。 然后，可以在从 SSMS 运行包时，通过在“执行包”弹出窗口的“高级”选项卡上提供此属性的属性路径 `\Package.Connections[YourConnectionManagerName].Properties[ConnectByProxy]`，并将其设置为 *True* 作为属性重写，来启用此属性。  

  ![启用 ConnectByProxy 属性 4](media/self-hosted-integration-runtime-proxy-ssis/shir-advanced-tab-ssms.png)

  在数据工厂管道中运行包时，还可以通过在“[执行 SSIS 包活动](./how-to-invoke-ssis-package-ssis-activity.md)”的“属性重写”选项卡上提供此属性的属性路径 `\Package.Connections[YourConnectionManagerName].Properties[ConnectByProxy]`，并将其设置为 True 作为属性重写，来启用此属性。  
  
  ![启用 ConnectByProxy 属性 5](media/self-hosted-integration-runtime-proxy-ssis/shir-property-overrides-tab-ssis-activity.png)

## <a name="debug-the-on-premises-and-cloud-staging-tasks"></a>调试本地和云暂存任务

在自承载 IR 上，可以在 C:\ProgramData\SSISTelemetry 文件夹中找到运行时日志，并在 C:\ProgramData\SSISTelemetry\ExecutionLog 文件夹中找到本地暂存任务的执行日志 。  你可以在 SSISDB、指定的日志文件路径或 Azure Monitor 中找到云暂存任务的执行日志，具体取决于你是在 SSISDB 中存储包、启用 [Azure Monitor 集成](https://docs.microsoft.com/azure/data-factory/monitor-using-azure-monitor#monitor-ssis-operations-with-azure-monitor)等。你还可以在云暂存任务的执行日志中找到本地临时任务的唯一 Id。 

![第一个暂存任务的唯一 ID](media/self-hosted-integration-runtime-proxy-ssis/shir-first-staging-task-guid.png)

如果你提出了客户支持票证，则可以在自承载 IR 上安装的 **Microsoft Integration Runtime Configuration Manager** 的 "**诊断**" 选项卡上选择 "**发送日志**" 按钮，以便发送最近的操作/执行日志供我们调查。

## <a name="billing-for-the-on-premises-and-cloud-staging-tasks"></a>本地暂存任务和云暂存任务的计费

在自承载 IR 上运行的本地暂存任务将单独计费，就像自承载 IR 上运行的任何数据移动活动的计费方式一样。 这是在 [Azure 数据工厂数据管道定价](https://azure.microsoft.com/pricing/details/data-factory/data-pipeline/)一文中指定的。

Azure-SSIS IR 上运行的云暂存任务不单独计费，但是运行中的 Azure-SSIS IR 会计费，如 [Azure-SSIS IR 定价](https://azure.microsoft.com/pricing/details/data-factory/ssis/)一文中所述。

## <a name="enable-custom3rd-party-components"></a>启用自定义/第三方组件 

若要使自定义/第三方组件可以使用自承载 IR 作为 Azure-SSIS IR 的代理来访问本地数据，请按照以下说明进行操作：

1. 通过 [标准/快速自定义](https://docs.microsoft.com/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup)安装，在 Azure-SSIS IR 上安装面向 SQL Server 2017 的自定义/第三方组件。

1. 在自承载 IR 上创建以下 DTSPath 注册表项（如果它们尚不存在）：
   1. 将 `Computer\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\140\SSIS\Setup\DTSPath` 设置为 `C:\Program Files\Microsoft SQL Server\140\DTS\`
   1. 将 `Computer\HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\Microsoft SQL Server\140\SSIS\Setup\DTSPath` 设置为 `C:\Program Files (x86)\Microsoft SQL Server\140\DTS\`
   
1. 在上述 DTSPath 下，在自承载 IR 上安装面向 SQL Server 2017 的自定义/第三方组件，并确保安装过程：

   1. `<DTSPath>`如果尚未存在，则创建、、 `<DTSPath>/Connections` `<DTSPath>/PipelineComponents` 和 `<DTSPath>/UpgradeMappings` 文件夹。
   
   1. 在文件夹中创建你自己的用于扩展映射的 XML 文件 `<DTSPath>/UpgradeMappings` 。
   
   1. 在全局程序集缓存 (GAC) 中安装自定义/第三方组件程序集所引用的所有程序集。

下面是我们的合作伙伴、 [Theobald Software](https://kb.theobald-software.com/xtract-is/XIS-for-Azure-SHIR) 和 [Aecorsoft](https://www.aecorsoft.com/blog/2020/11/8/using-azure-data-factory-to-bring-sap-data-to-azure-via-self-hosted-ir-and-ssis-ir)的示例，这些示例已改编其组件，以使用我们的快速自定义安装和自承载 IR 作为 Azure-SSIS IR 的代理。

## <a name="enforce-tls-12"></a>执行 TLS 1.2

如果需要在自承载 IR 上使用强加密/更安全的网络协议 (TLS 1.2) 并禁用较旧的 SSL/TLS 版本，则可下载并运行 main.cmd  脚本，该脚本可以在公共预览版容器的 CustomSetupScript/UserScenarios/TLS 1.2  文件夹中找到。  使用 [Azure 存储资源管理器](https://storageexplorer.com/)，可以通过输入以下 SAS URI 连接到公共预览版容器：

`https://ssisazurefileshare.blob.core.windows.net/publicpreview?sp=rl&st=2020-03-25T04:00:00Z&se=2025-03-25T04:00:00Z&sv=2019-02-02&sr=c&sig=WAD3DATezJjhBCO3ezrQ7TUZ8syEUxZZtGIhhP6Pt4I%3D`

## <a name="current-limitations"></a>当前限制

- 目前仅支持内置/预安装在 Azure-SSIS IR Standard Edition 上的数据流组件，Hadoop/HDFS/DQS 组件除外，请参阅 [Azure-SSIS IR 上的所有内置/预安装组件](https://docs.microsoft.com/azure/data-factory/built-in-preinstalled-components-ssis-integration-runtime)。
- 目前仅支持用托管代码编写的自定义/第三方数据流组件 ( .NET Framework) ，当前不支持用本机代码编写的)  (代码。
- 当前不支持在本地和云过渡任务中更改变量值。
- 在本地过渡任务中更改 object 类型的变量值不会反映在其他任务中。
- 目前不支持 OLEDB 源中的 ParameterMapping。 作为变通方法，请使用“变量中的 SQL 命令”作为“访问模式”，并使用“表达式”在 SQL 命令中插入变量/参数。    有关说明，请参阅 ParameterMappingSample.dtsx  包，它位于公共预览版容器的 SelfHostedIRProxy/Limitations  文件夹中。 使用 Azure 存储资源管理器，可以通过输入以上 SAS URI 连接到公共预览版容器。

## <a name="next-steps"></a>后续步骤

将自承载 IR 配置为 Azure-SSIS IR 的代理后，可以部署并运行包，以便在数据工厂管道中以“执行 SSIS 包”活动的形式访问本地数据。 有关操作方法，请参阅[在数据工厂管道中以“执行 SSIS 包”活动的形式运行 SSIS 包](./how-to-invoke-ssis-package-ssis-activity.md)。
