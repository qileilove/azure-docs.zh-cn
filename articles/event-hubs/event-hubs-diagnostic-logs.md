---
title: 设置诊断日志 - Azure 事件中心 | Microsoft Docs
description: 了解如何为 Azure 中的事件中心设置活动日志和诊断日志。
ms.topic: article
ms.date: 10/27/2020
ms.openlocfilehash: a7230746dc4225b04b0507c872416368aa14442b
ms.sourcegitcommit: d76108b476259fe3f5f20a91ed2c237c1577df14
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2020
ms.locfileid: "92912593"
---
# <a name="set-up-diagnostic-logs-for-an-azure-event-hub"></a>设置 Azure 事件中心的诊断日志

可以查看两种类型的 Azure 事件中心日志：

* **[活动日志](../azure-monitor/platform/platform-logs-overview.md)** ：此类日志提供对作业执行的操作的相关信息。 这些日志始终启用。 要查看活动日志条目，可以在 Azure 门户的事件中心命名空间的左窗格中选择“活动日志”。 例如：“创建或更新命名空间”、“创建或更新事件中心”。

    ![事件中心命名空间的活动日志](./media/event-hubs-diagnostic-logs/activity-log.png)
* **[诊断日志](../azure-monitor/platform/platform-logs-overview.md)** ：通过使用 API 或语言 SDK 上的管理客户端，诊断日志可提供有关对命名空间所执行操作的更丰富的信息。 
    
    下面的部分演示如何对事件中心命名空间启用诊断日志。

## <a name="enable-diagnostic-logs"></a>启用诊断日志
诊断日志默认已禁用。 若要启用诊断日志，请执行以下步骤：

1.  在 [Azure 门户](https://portal.azure.com)中，导航到你的事件中心命名空间。 
2. 在左窗格中，选择“监视”下的“诊断设置”，然后选择“+ 添加诊断设置”  。 

    ![诊断设置页面 - 添加诊断设置](./media/event-hubs-diagnostic-logs/diagnostic-settings-page.png)
4. 在“类别详细信息”部分中，选择要启用的诊断日志类型 。 本文的稍后部分将介绍有关这些类别的详细信息。 
5. 在“目标位置详细信息”部分中，设置所需的存档目标（目标位置）；例如，存储帐户、事件中心或 Log Analytics 工作区。

    ![“添加诊断设置”页面](./media/event-hubs-diagnostic-logs/aDD-diagnostic-settings-page.png)
6.  选择工具栏上的“保存”以保存诊断设置。

    新设置在大约 10 分钟后生效。 在此之后，日志将出现在“诊断日志”窗格上配置的存档目标中。

    有关配置诊断的详细信息，请参阅 [Azure 诊断日志概述](../azure-monitor/platform/platform-logs-overview.md)。

## <a name="diagnostic-logs-categories"></a>诊断日志类别

事件中心会捕获以下类别的诊断日志：

| 类别 | 说明 | 
| -------- | ----------- | 
| 存档日志 | 捕获有关[事件中心捕获](event-hubs-capture-overview.md)操作的信息，特别是与捕获错误相关的日志。 |
| 操作日志 | 捕获在 Azure 事件中心命名空间中执行的所有管理操作。 由于在 Azure 事件中心进行的数据操作量较大，因此不会捕获数据操作。 |
| 自动缩放日志 | 捕获在事件中心命名空间中执行的自动膨胀操作。 |
| Kafka 协调器日志 | 捕获与事件中心相关的 Kafka 协调器操作。 |
| Kafka 用户错误日志 | 捕获事件中心中调用的 Kafka API 的相关信息。 |
| 事件中心虚拟网络 (VNet) 连接事件 | 捕获向事件中心发送流量的 IP 地址和虚拟网络的相关信息。 |
| 客户管理的密钥用户日志 | 捕获与客户管理的密钥相关的操作。 |


所有日志均以 JavaScript 对象表示法 (JSON) 格式存储。 每个条目均包含字符串字段，这些字段采用以下部分所述的格式。

## <a name="archive-logs-schema"></a>存档日志架构

存档日志 JSON 字符串包括下表列出的元素：

名称 | 说明
------- | -------
`TaskName` | 描述失败的任务
`ActivityId` | 用于跟踪的内部 ID
`trackingId` | 用于跟踪的内部 ID
`resourceId` | Azure 资源管理器资源 ID
`eventHub` | 事件中心的完整名称（包括命名空间名称）
`partitionId` | 要写入到的事件中心分区
`archiveStep` | 可能值：ArchiveFlushWriter、DestinationInit
`startTime` | 失败开始时间
`failures` | 发生失败的次数
`durationInSeconds` | 失败持续时间
`message` | 错误消息
`category` | ArchiveLogs

以下代码是存档日志 JSON 字符串的示例：

```json
{
   "TaskName": "EventHubArchiveUserError",
   "ActivityId": "000000000-0000-0000-0000-0000000000000",
   "trackingId": "0000000-0000-0000-0000-00000000000000000",
   "resourceId": "/SUBSCRIPTIONS/000000000-0000-0000-0000-0000000000000/RESOURCEGROUPS/<Resource Group Name>/PROVIDERS/MICROSOFT.EVENTHUB/NAMESPACES/<Event Hubs Namespace Name>",
   "eventHub": "<Event Hub full name>",
   "partitionId": "1",
   "archiveStep": "ArchiveFlushWriter",
   "startTime": "9/22/2016 5:11:21 AM",
   "failures": 3,
   "durationInSeconds": 360,
   "message": "Microsoft.WindowsAzure.Storage.StorageException: The remote server returned an error: (404) Not Found. ---> System.Net.WebException: The remote server returned an error: (404) Not Found.\r\n   at Microsoft.WindowsAzure.Storage.Shared.Protocol.HttpResponseParsers.ProcessExpectedStatusCodeNoException[T](HttpStatusCode expectedStatusCode, HttpStatusCode actualStatusCode, T retVal, StorageCommandBase`1 cmd, Exception ex)\r\n   at Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob.<PutBlockImpl>b__3e(RESTCommand`1 cmd, HttpWebResponse resp, Exception ex, OperationContext ctx)\r\n   at Microsoft.WindowsAzure.Storage.Core.Executor.Executor.EndGetResponse[T](IAsyncResult getResponseResult)\r\n   --- End of inner exception stack trace ---\r\n   at Microsoft.WindowsAzure.Storage.Core.Util.StorageAsyncResult`1.End()\r\n   at Microsoft.WindowsAzure.Storage.Core.Util.AsyncExtensions.<>c__DisplayClass4.<CreateCallbackVoid>b__3(IAsyncResult ar)\r\n--- End of stack trace from previous location where exception was thrown ---\r\n   at System.",
   "category": "ArchiveLogs"
}
```

## <a name="operational-logs-schema"></a>操作日志架构

运行日志 JSON 字符串包括下表列出的元素：

名称 | 说明
------- | -------
`ActivityId` | 内部 ID，用于跟踪目的 |
`EventName` | 操作名称 |
`resourceId` | Azure 资源管理器资源 ID |
`SubscriptionId` | 订阅 ID |
`EventTimeString` | 操作时间 |
`EventProperties` | 操作属性 |
`Status` | 操作状态 |
`Caller` | 操作的调用方（Azure 门户或管理客户端） |
`Category` | OperationalLogs |

以下代码是运行日志 JSON 字符串的示例：

```json
Example:
{
   "ActivityId": "00000000-0000-0000-0000-00000000000000",
   "EventName": "Create EventHub",
   "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-0000000000000/RESOURCEGROUPS/<Resource Group Name>/PROVIDERS/MICROSOFT.EVENTHUB/NAMESPACES/<Event Hubs namespace name>",
   "SubscriptionId": "000000000-0000-0000-0000-000000000000",
   "EventTimeString": "9/28/2016 8:40:06 PM +00:00",
   "EventProperties": "{\"SubscriptionId\":\"0000000000-0000-0000-0000-000000000000\",\"Namespace\":\"<Namespace Name>\",\"Via\":\"https://<Namespace Name>.servicebus.windows.net/f8096791adb448579ee83d30e006a13e/?api-version=2016-07\",\"TrackingId\":\"5ee74c9e-72b5-4e98-97c4-08a62e56e221_G1\"}",
   "Status": "Succeeded",
   "Caller": "ServiceBus Client",
   "category": "OperationalLogs"
}
```

## <a name="autoscale-logs-schema"></a>自动缩放日志架构
自动缩放日志 JSON 包括下表列出的元素：

| 名称 | 说明 |
| ---- | ----------- | 
| `TrackingId` | 内部 ID，用于跟踪目的 |
| `ResourceId` | Azure 资源管理器资源 ID。 |
| `Message` | 信息性消息，提供有关自动膨胀操作的详细信息。 此消息包含给定命名空间的吞吐量单位的以前值和当前值，以及触发了 TU 膨胀的因素。 |

下面是一个示例自动缩放事件： 

```json
{
    "TrackingId": "fb1b3676-bb2d-4b17-85b7-be1c7aa1967e",
    "Message": "Scaled-up EventHub TUs (UpdateStartTimeUTC: 5/13/2020 7:48:36 AM, PreviousValue: 1, UpdatedThroughputUnitValue: 2, AutoScaleReason: 'IncomingMessagesPerSecond reached 2170')",
    "ResourceId": "/subscriptions/0000000-0000-0000-0000-000000000000/resourcegroups/testrg/providers/microsoft.eventhub/namespaces/namespace-name"
}
```

## <a name="kafka-coordinator-logs-schema"></a>Kafka 协调器日志架构
Kafka 协调器日志 JSON 包括下表列出的元素：

| 名称 | 说明 |
| ---- | ----------- | 
| `RequestId` | 请求 ID，用于跟踪目的 |
| `ResourceId` | Azure 资源管理器资源 ID |
| `Operation` | 组协调期间执行的操作的名称 |
| `ClientId` | 客户端 ID |
| `NamespaceName` | 命名空间名称 | 
| `SubscriptionId` | Azure 订阅 ID |
| `Message` | 信息性或警告消息，提供有关组协调期间执行的操作的详细信息。 |

### <a name="example"></a>示例

```json
{
    "RequestId": "FE01001A89E30B020000000304620E2A_KafkaExampleConsumer#0",
    "Operation": "Join.Start",
    "ClientId": "KafkaExampleConsumer#0",
    "Message": "Start join group for new member namespace-name:c:$default:I:KafkaExampleConsumer#0-cc40856f7f3c4607915a571efe994e82, current group size: 0, API version: 2, session timeout: 10000ms, rebalance timeout: 300000ms.",
    "SubscriptionId": "0000000-0000-0000-0000-000000000000",
    "NamespaceName": "namespace-name",
    "ResourceId": "/subscriptions/0000000-0000-0000-0000-000000000000/resourcegroups/testrg/providers/microsoft.eventhub/namespaces/namespace-name",
    "Category": "KafkaCoordinatorLogs"
}
```

## <a name="kafka-user-error-logs-schema"></a>Kafka 用户错误日志架构
Kafka 用户错误日志 JSON 包括下表列出的元素：

| 名称 | 说明 |
| ---- | ----------- |
| `TrackingId` | 跟踪 ID，用于跟踪目的。 |
| `NamespaceName` | 命名空间名称 |
| `Eventhub` | 事件中心名称 |
| `PartitionId` | 分区 ID |
| `GroupId` | 组 ID |
| `ClientId` | 客户端 ID |
| `ResourceId` | Azure 资源管理器资源 ID。 |
| `Message` | 信息性消息，提供有关错误的详细信息 |

## <a name="event-hubs-virtual-network-connection-event-schema"></a>事件中心虚拟网络连接事件架构
事件中心虚拟网络 (VNet) 连接事件 JSON 包含下表列出的元素：

| 名称 | 说明 |
| ---  | ----------- | 
| `SubscriptionId` | Azure 订阅 ID |
| `NamespaceName` | 命名空间名称 |
| `IPAddress` | 连接到事件中心服务的客户端的 IP 地址 |
| `Action` | 评估连接请求时事件中心服务执行的操作。 支持的操作为“接受连接”和“拒绝连接” 。 |
| `Reason` | 提供执行操作的原因 |
| `Count` | 给定操作的发生次数 |
| `ResourceId` | Azure 资源管理器资源 ID。 |

仅当命名空间允许从 **所选网络** 或从 **特定 ip 地址** 进行访问时，才会生成虚拟网络日志)  (ip 筛选器规则。 如果不想使用这些功能限制对命名空间的访问，但仍希望获取虚拟网络日志来跟踪连接到事件中心命名空间的客户端的 IP 地址，可以使用以下解决方法。 启用 IP 筛选，并将 "可寻址 IPv4 范围" (1.0.0.0/1-255.0.0.0/1) 中添加。 事件中心不支持 IPv6 范围。 

### <a name="example"></a>示例

```json
{
    "SubscriptionId": "0000000-0000-0000-0000-000000000000",
    "NamespaceName": "namespace-name",
    "IPAddress": "1.2.3.4",
    "Action": "Deny Connection",
    "Reason": "IPAddress doesn't belong to a subnet with Service Endpoint enabled.",
    "Count": "65",
    "ResourceId": "/subscriptions/0000000-0000-0000-0000-000000000000/resourcegroups/testrg/providers/microsoft.eventhub/namespaces/namespace-name",
    "Category": "EventHubVNetConnectionEvent"
}
```

## <a name="customer-managed-key-user-logs"></a>客户管理的密钥用户日志
客户管理的密钥用户日志 JSON 包括下表列出的元素：

| 名称 | 说明 |
| ---- | ----------- | 
| `Category` | 消息类别的类型。 以下值之一：“错误”和“信息”  |
| `ResourceId` | 内部资源 ID，包括 Azure 订阅 ID 和命名空间名称 |
| `KeyVault` | Key Vault 资源的名称 |
| `Key` | Key Vault 密钥的名称。 |
| `Version` | Key Vault 密钥的版本 |
| `Operation` | 对服务器请求执行的操作的名称 |
| `Code` | 状态代码 |
| `Message` | 消息，提供有关错误或信息性消息的详细信息 |



## <a name="next-steps"></a>后续步骤
- [事件中心简介](./event-hubs-about.md)
- [事件中心示例](sdks.md)
- 事件中心入门
    - [.NET Core](event-hubs-dotnet-standard-getstarted-send.md)
    - [Java](event-hubs-java-get-started-send.md)
    - [Python](event-hubs-python-get-started-send.md)
    - [JavaScript](event-hubs-java-get-started-send.md)
