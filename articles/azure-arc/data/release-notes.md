---
title: 启用了 Azure Arc 的数据服务-发行说明
description: 最新发行说明
author: twright-msft
ms.author: twright
ms.reviewer: mikeray
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
ms.date: 10/29/2020
ms.topic: conceptual
ms.openlocfilehash: 94074c2c5e11187252084832e5a20a197f6723fd
ms.sourcegitcommit: 6a902230296a78da21fbc68c365698709c579093
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/05/2020
ms.locfileid: "93359810"
---
# <a name="release-notes---azure-arc-enabled-data-services-preview"></a>发行说明-启用了 Azure Arc 的数据服务 (预览) 

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="october-2020"></a>2020 年 10 月 

Azure 数据 CLI (`azdata`) 版本号：20.2.3。 下载位置 [https://aka.ms/azdata](https://aka.ms/azdata) 。

### <a name="breaking-changes"></a>中断性变更

此版本引入了以下重大更改： 

* 在 PostgreSQL 自定义资源定义 (.CRD) 中，术语 `shards` 被重命名为 `workers` 。 此术语 (`workers`) 与命令行参数名称匹配。

* `azdata arc postgres server delete` 删除 postgres 实例之前提示确认。  使用 `--force` 跳过提示。

### <a name="additional-changes"></a>其他更改

* 添加了一个名为的新可选参数 `azdata arc postgres server create` `--volume-claim mounts` 。 该值是以逗号分隔的卷声明装载列表。 卷声明装载是一对卷类型和 PVC 名称。 目前唯一支持的卷类型是 `backup` 。  在 PostgreSQL 中，当卷类型为时 `backup` ，PVC 将装载到 `/mnt/db-backups` 。  这样就可以在 PostgresSQL 实例之间共享备份，以便在另一个实例中还原一个 PostgresSQL 实例的备份。

* 新的 PostgresSQL 自定义资源定义的短名称： 

  * `pg11` 

  * `pg12`

* 通过使用以下任一方法为用户提供遥测数据：

   * 上载到 Azure 的点数

     or 

   * 如果没有数据加载到 Azure，则会提示再次尝试。

* `azdata arc dc debug copy-logs` 接下来，还会从 `/var/opt/controller/log` 文件夹读取并收集 Linux 上的 PostgreSQL 引擎日志。

*   当使用 PostgreSQL 超大规模创建和还原备份时，显示工作指示器。

* `azdata arc postrgres backup list` 现在包含备份大小信息。

* 已将 SQL 托管实例 admin name 属性添加到 Azure 门户中 "概述" 边栏选项卡的右侧列。

* Azure Data Studio 支持配置 PostgreSQL 超大规模的辅助角色节点、vCore 和内存设置的数目。 

* 预览版支持 Postgres 版本11和12的备份/还原。

## <a name="september-2020"></a>2020 年 9 月

已为公共预览版发布启用了 Azure Arc 的数据服务。 启用 Arc 的数据服务允许你在任何位置管理数据服务。

- SQL 托管实例
- PostgreSQL 超大规模

有关说明，请参阅 [什么是启用了 Azure Arc 的数据服务？](overview.md)

## <a name="known-limitations-and-issues"></a>已知限制和问题

- 实例名称不能超过13个字符
- Azure Arc 数据控制器或数据库实例没有就地升级。
- 启用了 Arc 的数据服务容器映像未签名。  你可能需要将 Kubernetes 节点配置为允许拉取未签名的容器映像。  例如，如果使用 Docker 作为容器运行时，则可以设置 DOCKER_CONTENT_TRUST = 0 环境变量并重新启动。  其他容器运行时具有类似的选项，例如在 [OpenShift](https://docs.openshift.com/container-platform/4.5/openshift_images/image-configuration.html#images-configuration-file_image-configuration) 中就是如此。
- 无法从 Azure 门户创建启用了 Azure Arc 的 SQL 托管实例或 PostgreSQL 超大规模服务器组。
- 现在，如果使用的是 NFS，则需要 `allowRunAsRoot` `true` 在创建 Azure Arc 数据控制器之前在部署配置文件中将设置为。
- 仅限 SQL 和 PostgreSQL 登录身份验证。  不支持 Azure Active Directory 或 Active Directory。
- 在 OpenShift 上创建数据控制器需要严格的安全约束。  有关详细信息，请参阅文档。
- 如果使用 azure Kubernetes 服务引擎 (使用 Azure Arc 数据控制器和数据库实例的 Azure Stack 集线器上的 AKS Engine) ，则不支持升级到较新的 Kubernetes 版本。 请先卸载 Azure Arc 数据控制器和所有数据库实例，然后再升级 Kubernetes 群集。
- Azure Kubernetes Service (AKS) ，支持支持 Azure Arc 的数据服务目前不支持跨 [多个可用性区域](../../aks/availability-zones.md) 的群集。 若要避免此问题，在 Azure 门户中创建 AKS 群集时，如果选择区域可用的区域，请从选择控件中清除所有区域。 参看下图：

   :::image type="content" source="media/release-notes/aks-zone-selector.png" alt-text="清除每个区域的复选框以指定 &quot;无&quot;。":::


### <a name="known-issues-for-azure-arc-enabled-postgresql-hyperscale"></a>启用了 Azure Arc 的已知问题 PostgreSQL 超大规模   

- 预览不支持 PostgreSQL 版本11引擎的备份/还原。 它仅支持 PostgreSQL 版本12的备份/还原。
- `azdata arc dc debug copy-logs` 节点不收集 Windows 上的 PostgreSQL engine 日志。
- 使用刚删除的服务器组的名称重新创建服务器组可能会失败或停止响应。 
   - **解决方法** 重新创建服务器组或等待之前删除的服务器组的负载均衡器/外部服务时，请不要重复使用相同的名称。 假设你删除的服务器组的名称为， `postgres01` 并且该名称承载于一个命名空间中 `arc` ，则在重新创建同名的服务器组之前，请等待，直到 `postgres01-external-svc` 未显示在 kubectl 命令的输出中 `kubectl get svc -n arc` 。
 - 在 Azure Data Studio 中加载 "概述" 页和 "计算 + 存储" 配置页面速度缓慢。 



## <a name="next-steps"></a>后续步骤
  
> 想尝试一下吗？  
> 在 Azure Kubernetes 服务 (AKS)、AWS Elastic Kubernetes 服务 (EKS)、Google Cloud Kubernetes Engine (GKE) 或 Azure VM 中，通过 [Azure Arc 快速入门](https://github.com/microsoft/azure_arc#azure-arc-enabled-data-services)快速开始操作。

[安装客户端工具](install-client-tools.md)

[创建 Azure Arc 数据控制器](create-data-controller.md)（首先需要安装客户端工具）

[在 Azure Arc 上创建 Azure SQL 托管实例](create-sql-managed-instance.md)（首先需要创建 Azure Arc 数据控制器）

[在 Azure Arc 上创建 Azure Database for PostgreSQL 超大规模服务器组](create-postgresql-hyperscale-server-group.md)（首先需要创建 Azure Arc 数据控制器）
