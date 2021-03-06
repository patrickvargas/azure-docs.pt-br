---
author: mrbullwinkle
ms.service: application-insights
ms.topic: include
ms.date: 11/09/2018
ms.author: mbullwin
ms.openlocfilehash: 3986b77cfad167134bf8ada1e3cbb73ad64dd3ca
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/12/2018
ms.locfileid: "51571755"
---
Coleção de medidas personalizadas. Use esta coleção para relatar a medida nomeada associada ao item de telemetria. Os casos de uso típicos são:
- o tamanho da carga de Telemetria de Dependência
- o número de itens da fila processados por Telemetria de Solicitação
- tempo que o cliente demorou para concluir a etapa no assistente de Telemetria de Evento.

Você pode consultar [medidas personalizadas](https://analytics.applicationinsights.io/demo?q=H4sIAAAAAAAAA2WLOw6DMAyGd07hZoLeoRPqyMaGGAL8aiPhGCV2kKoeHsHK%2Bj1myyr8LoiaqfrT%2FkUCzRft4LMl8OUeL3LuLLIx%2BxR%2BIF8%2BtcoiNq2o78vgWuFthQaJ1AeGGxt6UlBwKxa1qQ6EpLhAfQAAAA%3D%3D&timespan=PT24H) na Análise do Aplicativo:

```
customEvents
| where customMeasurements != ""
| summarize avg(todouble(customMeasurements["Completion Time"]) * itemCount)
```

 > [!NOTE]
 > As medidas personalizadas são associadas ao item de telemetria ao qual estas pertencem. Elas são sujeitas à amostragem com o item de telemetria que contém essas medidas. Para controlar a medida que possui valor independente de outros tipos de telemetria, use [Telemetria metrica](../articles/application-insights/app-insights-api-custom-events-metrics.md).

Comprimento de chave máximo: 150
