---
author: IEvangelist
ms.author: dapine
ms.date: 06/25/2019
ms.service: cognitive-services
ms.topic: include
ms.openlocfilehash: 046c5c5e32c71364e1bf54551989e19eec4937e4
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2020
ms.locfileid: "67712571"
---
此 `ApplicationInsights` 设置允许向容器添加 [Azure Application Insights](https://docs.microsoft.com/azure/application-insights) 遥测支持。 Application Insights 可深入监视容器。 可以轻松监视容器的可用性、性能和使用情况。 还可以快速识别和诊断容器中的错误。

下表描述了 `ApplicationInsights` 节支持的配置设置。

|必选| 名称 | 数据类型 | 说明 |
|--|------|-----------|-------------|
|否| `InstrumentationKey` | String | 容器遥测数据要发送到的 Application Insights 实例的检测密钥。 有关详细信息，请参阅[适用于 ASP.NET Core 的 Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net-core)。 <br><br>示例：<br>`InstrumentationKey=123456789`|

