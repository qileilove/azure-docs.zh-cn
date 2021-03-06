---
title: 在 Azure 实验室服务 (UI) 中启用嵌套虚拟化Microsoft Docs
description: 了解如何创建包含多个 VM 的模板 VM。  也就是说，在 Azure 实验室服务中对模板 VM 启用嵌套虚拟化。
author: emaher
ms.topic: article
ms.date: 06/26/2020
ms.author: enewman
ms.openlocfilehash: f8135e11fb7b7ddb588ab3a8ed01227712072fd2
ms.sourcegitcommit: 8e7316bd4c4991de62ea485adca30065e5b86c67
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/17/2020
ms.locfileid: "94647913"
---
# <a name="enable-nested-virtualization-on-a-template-virtual-machine-in-azure-lab-services-manually"></a>在 Azure 实验室服务中的模板虚拟机上手动启用嵌套虚拟化

使用嵌套虚拟化，可以在实验室的模板虚拟机内创建多 VM 环境。 发布模板会为实验室中的每个用户提供一个虚拟机，其中包含多个 VM。  有关嵌套虚拟化和 Azure 实验室服务的详细信息，请参阅 [在 Azure 实验室服务中的模板虚拟机上启用嵌套虚拟化](how-to-enable-nested-virtualization-template-vm.md)。

本文介绍如何使用 Windows 角色和工具直接在 Azure 实验室服务中的模板计算机上设置嵌套虚拟化。  启用类以使用嵌套虚拟化需要几个事项。  以下步骤将介绍如何使用 Hyper-v 手动设置实验室服务计算机模板。  步骤适用于 Windows Server 2016 或 Windows Server 2019。  

>[!IMPORTANT]
>创建实验室时，选择“大型(嵌套虚拟化)”或“中等(嵌套虚拟化)”作为虚拟机大小。  否则，嵌套虚拟化将不起作用。  

## <a name="enable-hyper-v-role"></a>启用 Hyper-v 角色

以下步骤描述了使用服务器管理器在 Windows Server 上启用 Hyper-v 所需的操作。  安装成功后，将可以使用 Hyper-v 管理器添加、修改和删除客户端虚拟机。

1. 在 **服务器管理器** 的 "仪表板" 页上，单击 " **添加角色和功能**"。
2. 在“开始之前”  页上，单击“下一步”  。
3. 在 " **选择安装类型** " 页上，保留默认选择 "基于角色或基于功能的安装"，然后单击 " **下一步**"。
4. 在 " **选择目标服务器** " 页上，选择 "从服务器池中选择服务器"。   当前服务器已被选中。  单击“下一步”。
5. 在“选择服务器角色”页上，选择 Hyper-V。  
6. 将显示 " **添加角色和功能向导** " 弹出窗口。  如果适用) ，请选择 " **包括管理工具" (**。  单击 " **添加功能** " 按钮。
7. 在“选择服务器角色”页面上，单击“下一步”。
8. 在 " **选择功能" 页** 上，单击 " **下一步**"。
9. 在“Hyper-V”页上，单击“下一步”。
10. 在 " **创建虚拟交换机** " 页上，接受默认值，然后单击 " **下一步**"。
11. 在 " **虚拟机迁移** " 页上，接受默认值，然后单击 " **下一步**"。
12. 在 " **默认存储** " 页上，接受默认值，然后单击 " **下一步**"。
13. 在 " **确认安装选择** " 页上，选择 " **如果需要，自动重新启动目标服务器"**。
14. 出现 " **添加角色和功能向导** " 弹出窗口时，单击 **"是"**。
15. 单击“安装” 。
16. 等待 " **安装进度** " 页指示 hyper-v 角色已完成。  计算机在安装过程中可能会重新启动。
17. 单击“关闭”  。

## <a name="enable-dhcp-role"></a>启用 DHCP 角色

创建的任何 Hyper-v 客户端虚拟机都需要 NAT 网络中的 IP 地址。  稍后我们将创建 NAT 网络。  分配 IP 地址的一种方法是将该主机设置为 "实验室" 虚拟机模板，作为 DHCP 服务器。  下面是启用 DHCP 角色所需的步骤。

1. 在 **服务器管理器** 的 " **仪表板** " 页上，单击 " **添加角色和功能**"。
2. 在“开始之前”  页上，单击“下一步”  。
3. 在“选择安装类型”页上，选择“基于角色或功能的安装”，然后单击“下一步”。
4. 在 " **选择目标服务器** " 页上，从服务器池中选择当前服务器，然后单击 " **下一步**"。
5. 在 " **选择服务器角色** " 页上，选择 " **DHCP 服务器**"。  
6. 将显示 " **添加角色和功能向导** " 弹出窗口。  如果适用) ，请选择 " **包括管理工具" (**。  单击 **“添加功能”**。

    >[!NOTE]
    >你可能会看到一个验证错误，指出找不到静态 IP 地址。  对于我们的方案，可以忽略此警告。

7. 在“选择服务器角色”页面上，单击“下一步”。
8. 在“选择功能”页上，单击“下一步”。
9. 在 " **DHCP 服务器** " 页上，单击 " **下一步**"。
10. 在 **“确认安装选择”** 页上，单击 **“安装”**。
11. 等待 " **安装进度" 页** 指示 DHCP 角色已完成。
12. 单击“关闭”。

## <a name="enable-routing-and-remote-access-role"></a>启用路由和远程访问角色

1. 在 **服务器管理器** 的 " **仪表板** " 页上，单击 " **添加角色和功能**"。
2. 在“开始之前”  页上，单击“下一步”  。
3. 在“选择安装类型”页上，选择“基于角色或功能的安装”，然后单击“下一步”。
4. 在 " **选择目标服务器** " 页上，从服务器池中选择当前服务器，然后单击 " **下一步**"。
5. 在 " **选择服务器角色** " 页上，选择 " **远程访问**"。 单击“确定”。
6. 在“选择功能”页上，单击“下一步”。
7. 在 " **远程访问** " 页上，单击 " **下一步**"。
8. 在 " **角色服务** " 页上，选择 " **路由**"。
9. 将显示 " **添加角色和功能向导** " 弹出窗口。  如果适用) ，请选择 " **包括管理工具" (**。  单击 **“添加功能”**。
10. 单击“下一步”  。
11. 在“Web 服务器角色 (IIS)”页面上，单击“下一步”。
12. 在 **“选择角色服务”** 页上，单击 **“下一步”**。
13. 在 **“确认安装选择”** 页上，单击 **“安装”**。
14. 等待 " **安装进度** " 页指示远程访问角色已完成。  
15. 单击“关闭”  。

## <a name="create-virtual-nat-network"></a>创建虚拟 NAT 网络

由于已安装了所有必要的角色，接下来可以创建 NAT 网络。  创建过程涉及到创建交换机和 NAT 网络本身。  NAT (网络地址转换) 网络会将公共 IP 地址分配给专用网络上的一组 Vm，以允许连接到 internet。  在我们的示例中，专用 Vm 组将成为嵌套 Vm。  NAT 网络将允许嵌套式 Vm 彼此通信。 交换机是处理网络中流量的接收和路由的网络设备。

### <a name="create-a-new-virtual-switch"></a>创建新的虚拟交换机

1. 从 "Windows 管理工具" 中打开 " **Hyper-v 管理器** "。
2. 选择左侧导航菜单中的 "当前服务器"。
3. 单击 "**虚拟交换机管理器 ...** " 从 **Hyper-v 管理器** 右侧的 "**操作**" 菜单。
4. 在 " **虚拟交换机管理器** " 弹出窗口中，选择 " **内部** " 作为要创建的开关类型。  单击“创建虚拟交换机”。
5. 对于新创建的虚拟交换机，将名称设置为可记忆的名称。  在此示例中，我们将使用 "LabServicesSwitch"。  单击“确定”。
6. 将创建新的网络适配器。  该名称将类似于 "vEthernet (LabServicesSwitch) "。  若要验证是否打开 **控制面板**，请单击 " **网络和 Internet**"，然后单击 " **查看网络状态和任务**"。  在左侧，单击 " **更改适配器设置**"。

### <a name="create-a-nat-network"></a>创建 NAT 网络

1. 从 "Windows 管理工具" 中打开 " **路由和远程访问** " 工具。
2. 在左侧导航页中选择本地服务器。
3. 选择 "**操作**" "  ->  **配置并启用路由和远程访问**"。
4. 出现 " **路由和远程访问服务器安装向导** " 时，单击 " **下一步**"。
5. 在 " **配置** " 页上，选择 " **网络地址转换" (NAT)** 配置。  单击“下一步”  。

    >[!WARNING]
    >不要选择 "虚拟专用网络 (VPN) 访问和 NAT" 选项。

6. 在 " **NAT Internet 连接** " 页上，选择 "以太网"。  不要选择在 Hyper-v 管理器中创建的 "vEthernet (LabServicesSwitch) " 连接。 单击“下一步”  。
7. 在向导的最后一页上单击 " **完成** "。
8. 当 " **启动服务** " 对话框出现时，单击 " **启动服务**"。
9. 等待服务启动。

## <a name="update-network-adapter-settings"></a>更新网络适配器设置

网络适配器将与以前创建的 NAT 网络的默认网关 IP 使用的 IP 相关联。  在此示例中，我们将创建一个 IP 地址192.168.0.1，子网掩码为255.255.255.0。  我们将使用之前创建的虚拟交换机。

1. 打开 " **控制面板**"，单击 " **网络和 Internet**"，然后单击 " **查看网络状态和任务**"。
2. 在左侧，单击 " **更改适配器设置**"。  
3. 在 " **网络连接** " 窗口中，双击 "VEthernet (LabServicesSwitch) " 以显示 **vEthernet (LabServicesSwitch) 状态** 详细信息 "对话框。
4. 单击 " **属性** " 按钮。
5. 选择 " **Internet 协议版本 4 (" TCP/IPv4)** 项 "，然后单击" **属性** "按钮。
6. 在 " **Internet 协议版本 4 (" tcp/ip) 属性** "对话框中，选择 **" 使用下面的 IP 地址 "**。  对于 "ip 地址"，请输入192.168.0.1。 对于 "子网掩码"，请输入255.255.255.0。  将默认网关留空。  同时将 DNS 服务器保留为空。

    >[!NOTE]
    > NAT 网络的范围将是 CIDR 表示法中的 192.168.0.0/24。  这会创建一个范围从192.168.0.1 到192.168.0.254 的可用 IP 地址。  按照约定，网关具有子网范围内的第一个 IP 地址。

7. 单击“确定”。

## <a name="create-dhcp-scope"></a>创建 DHCP 作用域

以下步骤说明如何添加 DHCP 作用域。  在本文中，我们的 NAT 网络是 CIDR 表示法中的 192.168.0.0/24。  这会创建一个范围从192.168.0.1 到192.168.0.254 的可用 IP 地址。  创建的作用域必须在此可用地址范围内，不包括之前创建的 IP 地址。

1. 打开 " **管理工具** " 并打开 " **DHCP** 管理工具"。
2. 在 **DHCP** 工具中，展开当前服务器的节点，然后选择 " **IPv4**"。
3. 从 "操作" 菜单中，选择 "**新建作用域 ...** "
4. 出现 "**新建作用域" 向导** 时，单击 **欢迎** 页上的 "**下一步**"。
5. 在 " **作用域名称** " 页上，输入 "LabServicesDhcpScope" 或其他便于记忆的名称。  单击“下一步”  。
6. 在 " **IP 地址范围** " 页上，输入以下值。

    - 起始 IP 地址的192.168.0.100
    - 结束 IP 地址的192.168.0.200
    - 长度为24
    - 255.255.255.0，子网掩码

7. 单击“下一步”  。
8. 在 " **添加排除和延迟** " 页上，单击 " **下一步**"。
9. 在 " **租用持续时间** " 页上，单击 " **下一步**"。
10. 在 " **配置 DHCP 选项** " 页上，选择 **"是，我想立即配置这些选项"**。 单击“下一步”  。
11. 在 **路由器上 ("默认网关")**
12. 如果尚未执行此操作，请添加192.168.0.1。 单击“下一步”  。
13. 在 " **域名和 Dns 服务器** " 页上，将 "168.63.129.16" 添加为 DNS 服务器 IP 地址（如果尚未这样做）。  168.63.129.16 是 Azure 静态 DNS 服务器的 IP 地址。 单击“下一步”  。
14. 在 " **WINS 服务器** " 页上，单击 " **下一步**"。
15. 在 " **激活作用域** " 页上，选择 **"是，我想现在激活此作用域**"。  单击“下一步”  。
16. 在 " **正在完成新建作用域向导** " 页上，单击 " **完成**"。

## <a name="conclusion"></a>结束语

现在，你的模板计算机已准备好创建 Hyper-v 虚拟机。   有关如何创建 Hyper-v 虚拟机的说明，请参阅 [在 hyper-v 中创建虚拟机](/windows-server/virtualization/hyper-v/get-started/create-a-virtual-machine-in-hyper-v) 。  另请参阅 [Microsoft 评估中心](https://www.microsoft.com/evalcenter/) ，查看可用的操作系统和软件。

## <a name="next-steps"></a>后续步骤

下一步是设置任何实验室的常见步骤。

- [添加用户](tutorial-setup-classroom-lab.md#add-users-to-the-lab)
- [设置配额](how-to-configure-student-usage.md#set-quotas-for-users)
- [设置日程安排](tutorial-setup-classroom-lab.md#set-a-schedule-for-the-lab)
- [向学生发送电子邮件注册链接](how-to-configure-student-usage.md#send-invitations-to-users)