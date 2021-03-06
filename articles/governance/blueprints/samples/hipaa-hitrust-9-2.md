---
title: HIPAA HITRUST 9.2 蓝图示例概述
description: HIPAA HITRUST 9.2 蓝图示例概述。 此蓝图示例有助于客户评估特定的 HIPAA HITRUST 9.2 控制要求。
ms.date: 09/04/2020
ms.topic: sample
ms.openlocfilehash: 4df6f05019976b3de1172cc5127c27ac00fe3c44
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2020
ms.locfileid: "89493185"
---
# <a name="hipaa-hitrust-92-blueprint-sample"></a>HIPAA HITRUST 9.2 蓝图示例

HIPAA HITRUST 9.2 蓝图示例提供了监管防护措施，其中使用 [Azure Policy](../../policy/overview.md) 来帮助评估特定 HIPAA HITRUST 9.2 控制要求。 对于 Azure 部署的任何必须实现 HIPAA HITRUST 9.2 控制要求的体系结构，此蓝图可帮助客户为其部署一组核心策略。

## <a name="control-mapping"></a>控制映射

[Azure Policy 控制映射](../../policy/samples/hipaa-hitrust-9-2.md)提供了有关此蓝图中包含的策略定义的详细信息，以及这些策略定义与 HIPAA HITRUST 9.2 中的合规性域和控制要求的映射关系 。 在将资源分配给体系结构时，Azure Policy 会评估这些资源是否不符合已分配的策略定义。 有关详细信息，请参阅 [Azure Policy](../../policy/overview.md)。

## <a name="deploy"></a>部署

若要部署 Azure 蓝图 HIPAA HITRUST 9.2 蓝图示例，必须执行以下步骤：

> [!div class="checklist"]
> - 基于示例创建新的蓝图
> - 将示例副本标记为“已发布”
> - 将蓝图副本分配到现有的订阅

如果没有 Azure 订阅，请在开始之前创建一个[免费帐户](https://azure.microsoft.com/free)。

### <a name="create-blueprint-from-sample"></a>基于示例创建蓝图

首先，通过使用示例作为起点在环境中创建新的蓝图，来实现蓝图示例。

1. 在左侧窗格中，选择“所有服务”  。 搜索并选择“蓝图”。

1. 在左侧的“开始”页中，选择“创建蓝图”下的“创建”按钮。 

1. 在“其他示例”下找到“HIPAA HITRUST”蓝图示例，然后选择“使用此示例”。

1. 输入该蓝图示例的“基本信息”：

   - **蓝图名称**：提供 HIPAA HITRUST 9.2 蓝图示例副本的名称。
   - **定义位置**：使用省略号并选择要将示例副本保存到的管理组。

1. 选择页面顶部的“项目”选项卡，或页面底部的“下一步:项目”。

1. 查看构成蓝图示例的项目列表。 许多项目包含稍后我们将要定义的参数。 查看完蓝图示例后，选择“保存草稿”。

### <a name="publish-the-sample-copy"></a>发布示例副本

现已在环境中创建蓝图示例的副本。 该副本在创建后处于“草稿”模式，必须先将其**发布**，然后才能分配和部署它。 可根据环境和需求自定义蓝图示例的副本，但这种修改可能不符合 HIPAA HITRUST 9.2 控制要求。

1. 在左侧窗格中，选择“所有服务”  。 搜索并选择“蓝图”。

1. 在左侧选择“蓝图定义”页。 使用筛选器找到蓝图示例的副本，然后选择它。

1. 选择页面顶部的“发布蓝图”。 在右侧的新窗格中，提供蓝图示例副本的**版本**。 以后做出修改时，此属性非常有用。 提供“更改注释”，例如，“基于 HIPAA HITRUST 9.2 蓝图示例发布的第一个版本”。 然后选择页面底部的“发布”。

### <a name="assign-the-sample-copy"></a>分配示例副本

成功**发布**蓝图示例的副本后，可将它分配到它所在的管理组中的某个订阅。 在此步骤中，需提供参数来使蓝图示例副本的每个部署保持唯一。

1. 在左侧窗格中，选择“所有服务”  。 搜索并选择“蓝图”。

1. 在左侧选择“蓝图定义”页。 使用筛选器找到蓝图示例的副本，然后选择它。

1. 选择蓝图定义页面顶部的“分配蓝图”。

1. 提供蓝图分配的参数值：

   - 基础

     - **订阅**：在蓝图示例副本所保存到的管理组中选择一个或多个订阅。 如果选择多个订阅，将使用输入的参数为每个订阅创建一个分配。
     - **分配名称**：系统会根据蓝图的名称预先填充该名称。
       请根据需要更改该名称，或保留原样。
     - 位置：选择要在其中创建托管标识的区域。 Azure 蓝图使用此托管标识在分配的蓝图中部署所有项目。 若要了解详细信息，请参阅 [Azure 资源的托管标识](../../../active-directory/managed-identities-azure-resources/overview.md)。
     - **蓝图定义版本**：选择蓝图示例副本的**已发布**版本。

   - 锁分配

     选择环境的蓝图锁定设置。 有关更多信息，请参阅[蓝图资源锁定](../concepts/resource-locking.md)。

   - 托管标识

     保留默认的系统分配的托管标识选项。

   - 项目参数

     在本部分定义的参数将应用到定义了这些参数的项目。 这些参数属于[动态参数](../concepts/parameters.md#dynamic-parameters) ，因为它们是在分配蓝图期间定义的。 有关完整列表或项目参数及其说明，请参阅[项目参数表](#artifact-parameters-table) 。

1. 输入所有参数后，选择页面底部的“分配”。 随后将创建蓝图分配，并开始部署项目。 部署过程大约需要一小时。 若要检查部署状态，请打开蓝图分配。

> [!WARNING]
> Azure 蓝图服务和内置蓝图示例是**免费的**。 Azure 资源[按产品定价](https://azure.microsoft.com/pricing/) 。 使用[定价计算器](https://azure.microsoft.com/pricing/calculator/) 可以估算运行此蓝图示例部署的资源所需的成本。

### <a name="artifact-parameters-table"></a>项目参数表

下表提供了蓝图项目参数的列表：

|项目名称 |参数名称 |说明 |
|---|---|---|
|审核 HITRUST/HIPAA 控制措施，并部署特定 VM 扩展以支持审核要求 |应该限制通过面向 Internet 的终结点进行访问 |启用或禁用对过于宽松的入站 NSG 规则的监视 |
|审核 HITRUST/HIPAA 控制措施，并部署特定 VM 扩展以支持审核要求 |帐户：来宾帐户状态 |指定是否禁用本地来宾帐户。 |
|审核 HITRUST/HIPAA 控制措施，并部署特定 VM 扩展以支持审核要求 |应在虚拟机上启用自适应应用程序控制 |启用或禁用对 Azure 安全中心中应用程序允许列表的监视 |
|审核 HITRUST/HIPAA 控制措施，并部署特定 VM 扩展以支持审核要求 |允许同时连接到 Internet 或 Windows 域 |指定是否阻止计算机同时连接到基于域的网络和基于非域的网络。 值 0 允许同时进行连接，值 1 则阻止它们。 |
|审核 HITRUST/HIPAA 控制措施，并部署特定 VM 扩展以支持审核要求 |只能通过 HTTPS V2 访问 API 应用 |启用或禁用对 API 应用 V2 中 HTTPS 使用情况的监视 |
|审核 HITRUST/HIPAA 控制措施，并部署特定 VM 扩展以支持审核要求 |应用程序名称（支持通配符） |一个分号分隔的列表，其中包含应安装的应用程序的名称。 例如，“Microsoft SQL Server 2014 (64-位); Microsoft Visual Studio Code”或“Microsoft SQL Server 2014*”(后者表示与以 "Microsoft SQL Server 2014" 开头的任意应用程序相匹配) |
|审核 HITRUST/HIPAA 控制措施，并部署特定 VM 扩展以支持审核要求 |审核进程终止 |指定在进程退出时是否生成审核事件。 建议对关键进程的终止进行监视。 |
|审核 HITRUST/HIPAA 控制措施，并部署特定 VM 扩展以支持审核要求 |审核对存储帐户的不受限的网络访问 |启用或禁用存储帐户网络访问监视 |
|审核 HITRUST/HIPAA 控制措施，并部署特定 VM 扩展以支持审核要求 |审核：如果无法记录安全审核则立即关闭系统 |审核在无法记录安全事件时系统是否将关闭。 |
|审核 HITRUST/HIPAA 控制措施，并部署特定 VM 扩展以支持审核要求 |证书指纹 |受信任的根证书存储 (Cert:\LocalMachine\Root) 中应存在以分号分隔的证书指纹列表。 例如 THUMBPRINT1;THUMBPRINT2;THUMBPRINT3 |
|审核 HITRUST/HIPAA 控制措施，并部署特定 VM 扩展以支持审核要求 |应启用 Batch 帐户中的诊断日志 |启用或禁用批处理帐户中的诊断日志监视 |
|审核 HITRUST/HIPAA 控制措施，并部署特定 VM 扩展以支持审核要求 |应启用事件中心内的诊断日志 |启用或禁用事件中心帐户中的诊断日志监视 |
|审核 HITRUST/HIPAA 控制措施，并部署特定 VM 扩展以支持审核要求 |应启用搜索服务的诊断日志 |启用或禁用 Azure 搜索服务中的诊断日志监视 |
|审核 HITRUST/HIPAA 控制措施，并部署特定 VM 扩展以支持审核要求 |应启用虚拟机规模集中的诊断日志 |启用或禁用 Service Fabric 中的诊断日志监视 |
|审核 HITRUST/HIPAA 控制措施，并部署特定 VM 扩展以支持审核要求 |应在虚拟机上启用磁盘加密 |启用或禁用 VM 磁盘加密监视 |
|审核 HITRUST/HIPAA 控制措施，并部署特定 VM 扩展以支持审核要求 |启用不安全的来宾登录 |指定 SMB 客户端是否允许不安全的来宾登录到 SMB 服务器。 |
|审核 HITRUST/HIPAA 控制措施，并部署特定 VM 扩展以支持审核要求 |应在虚拟机上应用实时网络访问控制 |启用或禁用网络即时访问监视 |
|审核 HITRUST/HIPAA 控制措施，并部署特定 VM 扩展以支持审核要求 |应关闭虚拟机上的管理端口 |启用或禁用虚拟机上开放管理端口的监视 |
|审核 HITRUST/HIPAA 控制措施，并部署特定 VM 扩展以支持审核要求 |应对订阅中拥有写入权限的帐户启用 MFA |启用或禁用对订阅中具有写入权限的帐户的 MFA 监视 |
|审核 HITRUST/HIPAA 控制措施，并部署特定 VM 扩展以支持审核要求 |应在对订阅拥有所有者权限的帐户上启用 MFA |启用或禁用对订阅中具有所有者权限的帐户的 MFA 监视 |
|审核 HITRUST/HIPAA 控制措施，并部署特定 VM 扩展以支持审核要求 |网络访问：可远程访问的注册表路径 |指定可通过网络访问的注册表路径，而不管 `winreg` 注册表项的访问控制列表 (ACL) 中列出的用户或组。 |
|审核 HITRUST/HIPAA 控制措施，并部署特定 VM 扩展以支持审核要求 |网络访问：可远程访问的注册表路径和子路径 |指定可通过网络访问的注册表路径和子路径，而不管 `winreg` 注册表项的访问控制列表 (ACL) 中列出的用户或组。 |
|审核 HITRUST/HIPAA 控制措施，并部署特定 VM 扩展以支持审核要求 |网络访问：可匿名访问的共享 |指定匿名用户可以访问的网络共享。 此策略设置的默认配置影响不大，因为必须先对所有用户进行身份验证，然后才能访问服务器上的共享资源。 |
|审核 HITRUST/HIPAA 控制措施，并部署特定 VM 扩展以支持审核要求 |恢复控制台：允许软盘复制并访问所有驱动器和所有文件夹 |指定是否使恢复控制台 SET 命令可用，从而允许设置恢复控制台环境变量。 |
|审核 HITRUST/HIPAA 控制措施，并部署特定 VM 扩展以支持审核要求 |应为 API 应用禁用远程调试 |启用或禁用对 API 应用远程调试的监视 |
|审核 HITRUST/HIPAA 控制措施，并部署特定 VM 扩展以支持审核要求 |应禁用 Web 应用程序的远程调试 |启用或禁用对 Web 应用远程调试的监视 |
|审核 HITRUST/HIPAA 控制措施，并部署特定 VM 扩展以支持审核要求 |批处理帐户中日志的所需保留期（以天为单位） |诊断日志的所需保留期（以天为单位） |
|审核 HITRUST/HIPAA 控制措施，并部署特定 VM 扩展以支持审核要求 |Azure 搜索服务中日志的所需保留期（以天为单位） |诊断日志的所需保留期（以天为单位） |
|审核 HITRUST/HIPAA 控制措施，并部署特定 VM 扩展以支持审核要求 |事件中心帐户中日志的所需保留期（以天为单位） |诊断日志的所需保留期（以天为单位） |
|审核 HITRUST/HIPAA 控制措施，并部署特定 VM 扩展以支持审核要求 |存储帐户的资源组名称（必须存在），用于为网络安全组部署诊断设置 |将在其中创建存储帐户的资源组。 此资源组必须已存在。 |
|审核 HITRUST/HIPAA 控制措施，并部署特定 VM 扩展以支持审核要求 |应在 Kubernetes 服务中使用基于角色的访问控制 (RBAC) |启用或禁用对未启用 RBAC 的 Kubernetes 服务的监视 |
|审核 HITRUST/HIPAA 控制措施，并部署特定 VM 扩展以支持审核要求 |应使用自己的密钥加密 SQL 托管实例的 TDE 保护器 |启用或禁用对支持自带密钥的透明数据加密 (TDE) 的监视。 支持自带密钥的 TDE 增加了透明度和对 TDE 保护器的控制，增强了由 HSM 提供支持的外部服务的安全性，并促进了职责划分。 |
|审核 HITRUST/HIPAA 控制措施，并部署特定 VM 扩展以支持审核要求 |应使用自己的密钥加密 SQL 服务器的 TDE 保护器 |启用或禁用对支持自带密钥的透明数据加密 (TDE) 的监视。 支持自带密钥的 TDE 增加了透明度和对 TDE 保护器的控制，增强了由 HSM 提供支持的外部服务的安全性，并促进了职责划分。 |
|审核 HITRUST/HIPAA 控制措施，并部署特定 VM 扩展以支持审核要求 |区域存储帐户的存储帐户前缀，用于为网络安全组部署诊断设置 |此前缀将与网络安全组位置结合使用，一起构成已创建的存储帐户的名称。 |
|审核 HITRUST/HIPAA 控制措施，并部署特定 VM 扩展以支持审核要求 |应在虚拟机规模集上安装系统更新 |启用或禁用虚拟机规模集的系统更新报告 |
|审核 HITRUST/HIPAA 控制措施，并部署特定 VM 扩展以支持审核要求 |应在虚拟机规模集上安装系统更新 |启用或禁用虚拟机规模集的系统更新报告 |
|审核 HITRUST/HIPAA 控制措施，并部署特定 VM 扩展以支持审核要求 |关闭多播名称解析 |指定是否启用 LLMNR（通过单个子网上的本地子网链接使用多播传输的辅助名称解析协议）。 |
|审核 HITRUST/HIPAA 控制措施，并部署特定 VM 扩展以支持审核要求 |应将虚拟机迁移到新的 Azure 资源管理器资源 |启用或禁用经典计算 VM 监视 |
|审核 HITRUST/HIPAA 控制措施，并部署特定 VM 扩展以支持审核要求 |应该修复虚拟机规模集上安全配置中的漏洞 |启用或禁用虚拟机规模集的 OS 漏洞监视 |
|审核 HITRUST/HIPAA 控制措施，并部署特定 VM 扩展以支持审核要求 |应该通过漏洞评估解决方案修复漏洞 |启用或禁用漏洞评估解决方案对 VM 漏洞的检测 |
|审核 HITRUST/HIPAA 控制措施，并部署特定 VM 扩展以支持审核要求 |应对 SQL 托管实例启用漏洞评估 |审核未启用定期漏洞评估扫描的 SQL 托管实例。 漏洞评估可发现、跟踪和帮助你修正潜在数据库漏洞。 |
|审核 HITRUST/HIPAA 控制措施，并部署特定 VM 扩展以支持审核要求 |Windows 防火墙（域）：应用本地防火墙规则 |指定是否允许本地管理员创建本地防火墙规则，这些规则与域配置文件的组策略配置的防火墙规则一起应用。 |
|审核 HITRUST/HIPAA 控制措施，并部署特定 VM 扩展以支持审核要求 |Windows 防火墙（域）：针对出站连接的行为 |指定与出站防火墙规则不匹配的域网络出站连接行为。 默认值 0 表示允许连接，值 1 则表示阻止连接。 |
|审核 HITRUST/HIPAA 控制措施，并部署特定 VM 扩展以支持审核要求 |Windows 防火墙（域）：针对出站连接的行为 |指定与出站防火墙规则不匹配的域网络出站连接行为。 默认值 0 表示允许连接，值 1 则表示阻止连接。 |
|审核 HITRUST/HIPAA 控制措施，并部署特定 VM 扩展以支持审核要求 |Windows 防火墙（域）：显示通知 |指定当阻止某个程序接收入站连接时，具有高级安全性的 Windows 防火墙是否向用户显示通知，用于域配置文件。 |
|审核 HITRUST/HIPAA 控制措施，并部署特定 VM 扩展以支持审核要求 |Windows 防火墙（域）：使用配置文件设置 |指定具有高级安全性的 Windows 防火墙是否使用域配置文件的设置来筛选网络通信。 如果选择“关闭”，具有高级安全性的 Windows 防火墙将不使用任何防火墙规则或此配置文件的连接安全规则。 |
|审核 HITRUST/HIPAA 控制措施，并部署特定 VM 扩展以支持审核要求 |Windows 防火墙（专用）：应用本地连接安全规则 |指定是否允许本地管理员创建连接安全规则，这些规则与专用配置文件的组策略配置的连接安全规则一起应用。 |
|审核 HITRUST/HIPAA 控制措施，并部署特定 VM 扩展以支持审核要求 |Windows 防火墙（专用）：应用本地防火墙规则 |指定是否允许本地管理员创建本地防火墙规则，这些规则与专用配置文件的组策略配置的防火墙规则一起应用。 |
|审核 HITRUST/HIPAA 控制措施，并部署特定 VM 扩展以支持审核要求 |Windows 防火墙（专用）：针对出站连接的行为 |为专用配置文件指定与出站防火墙规则不匹配的出站连接行为。 默认值 0 表示允许连接，值 1 则表示阻止连接。 |
|审核 HITRUST/HIPAA 控制措施，并部署特定 VM 扩展以支持审核要求 |Windows 防火墙（专用）：显示通知 |指定当阻止某个程序接收入站连接时，具有高级安全性的 Windows 防火墙是否向用户显示通知，用于专用配置文件。 |
|审核 HITRUST/HIPAA 控制措施，并部署特定 VM 扩展以支持审核要求 |Windows 防火墙（专用）：使用配置文件设置 |指定具有高级安全性的 Windows 防火墙是否使用专用配置文件的设置来筛选网络通信。 如果选择“关闭”，具有高级安全性的 Windows 防火墙将不使用任何防火墙规则或此配置文件的连接安全规则。 |
|审核 HITRUST/HIPAA 控制措施，并部署特定 VM 扩展以支持审核要求 |Windows 防火墙（公用）：应用本地连接安全规则 |指定是否允许本地管理员创建连接安全规则，这些规则与公用配置文件的组策略配置的连接安全规则一起应用。 |
|审核 HITRUST/HIPAA 控制措施，并部署特定 VM 扩展以支持审核要求 |Windows 防火墙（公用）：应用本地防火墙规则 |指定是否允许本地管理员创建本地防火墙规则，这些规则与公用配置文件的组策略配置的防火墙规则一起应用。 |
|审核 HITRUST/HIPAA 控制措施，并部署特定 VM 扩展以支持审核要求 |Windows 防火墙（公用）：针对出站连接的行为 |为公用配置文件指定与出站防火墙规则不匹配的出站连接行为。 默认值 0 表示允许连接，值 1 则表示阻止连接。 |
|审核 HITRUST/HIPAA 控制措施，并部署特定 VM 扩展以支持审核要求 |Windows 防火墙（公用）：显示通知 |指定当阻止某个程序接收入站连接时，具有高级安全性的 Windows 防火墙是否向用户显示通知，用于公用配置文件。 |
|审核 HITRUST/HIPAA 控制措施，并部署特定 VM 扩展以支持审核要求 |Windows 防火墙（公用）：使用配置文件设置 |指定具有高级安全性的 Windows 防火墙是否使用公用配置文件的设置来筛选网络通信。 如果选择“关闭”，具有高级安全性的 Windows 防火墙将不使用任何防火墙规则或此配置文件的连接安全规则。 |
|审核 HITRUST/HIPAA 控制措施，并部署特定 VM 扩展以支持审核要求 |Windows 防火墙：域：允许单播响应 |指定具有高级安全性的 Windows 防火墙是否允许本地计算机接收对其传出多播或广播消息的单播响应；用于域配置文件。 |
|审核 HITRUST/HIPAA 控制措施，并部署特定 VM 扩展以支持审核要求 |Windows 防火墙：专用：允许单播响应 |指定具有高级安全性的 Windows 防火墙是否允许本地计算机接收对其传出多播或广播消息的单播响应；用于专用配置文件。 |
|审核 HITRUST/HIPAA 控制措施，并部署特定 VM 扩展以支持审核要求 |Windows 防火墙：公共：允许单播响应 |指定具有高级安全性的 Windows 防火墙是否允许本地计算机接收对其传出多播或广播消息的单播响应；用于公用配置文件。 |

## <a name="next-steps"></a>后续步骤

有关蓝图及其使用方式的更多文章：

- 了解[蓝图生命周期](../concepts/lifecycle.md)。
- 了解如何使用[静态和动态参数](../concepts/parameters.md)。
- 了解如何自定义[蓝图排序顺序](../concepts/sequencing-order.md)。
- 了解如何利用[蓝图资源锁定](../concepts/resource-locking.md)。
- 了解如何[更新现有分配](../how-to/update-existing-assignments.md)。
