---
title: Trabalhando com cadeias de caracteres em consultas de análise de Log do Azure | Microsoft Docs
description: Descreve as funções de agregação em consultas de análise de Log que oferecem maneiras úteis para analisar seus dados.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/16/2018
ms.author: bwren
ms.component: na
ms.openlocfilehash: 764c43a382442096a5d130334e54afdc135ba419
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46966666"
---
# <a name="aggregations-in-log-analytics-queries"></a>Operadores úteis em consultas do Log Analytics

> [!NOTE]
> Você deve concluir [Primeiros passos com o portal do Google Analytics](get-started-analytics-portal.md) e [Primeiros passos com as consultas](get-started-queries.md) antes de concluir esta lição.

[!INCLUDE [log-analytics-demo-environment](../../../includes/log-analytics-demo-environment.md)]

Descreve as funções de agregação em consultas de análise de Log que oferecem maneiras úteis para analisar seus dados. These functions all work with the `summarize` operator that produces a  table with aggregated results of the input table.

## <a name="counts"></a>Counts

### <a name="count"></a>count
Conte o número de linhas no conjunto de resultados depois que os filtros forem aplicados. O exemplo a seguir retorna o número total de linhas na tabela _Perf_ dos últimos 30 minutos. O resultado é retornado em uma coluna chamada *count_*, a menos que você atribua um nome específico a ele:


```Kusto
Perf
| where TimeGenerated > ago(30m) 
| summarize count()
```

```Kusto
Perf
| where TimeGenerated > ago(30m) 
| summarize num_of_records=count() 
```

Uma visualização de cronograma pode ser útil para ver uma tendência ao longo do tempo:

```Kusto
Perf 
| where TimeGenerated > ago(30m) 
| summarize count() by bin(TimeGenerated, 5m)
| render timechart
```

A saída deste exemplo mostra a linha de tendência de contagem de registros perf em intervalos de 5 minutos:

![Tendência de contagem](media/aggregations/count-trend.png)


### <a name="dcount-dcountif"></a>DCount, dcountif
Use `dcount`e`dcountif` para contar valores distintos em uma coluna específica. A consulta a seguir avalia quantos computadores distintos enviam pulsações na última hora:

```Kusto
Heartbeat 
| where TimeGenerated > ago(1h) 
| summarize dcount(Computer)
```

Para contar apenas os computadores Linux que enviaram heartbeats, use `dcountif`:

```Kusto
Heartbeat 
| where TimeGenerated > ago(1h) 
| summarize dcountif(Computer, OSType=="Linux")
```

### <a name="evaluating-subgroups"></a>Avaliando subgrupos
Para executar uma contagem ou outras agregações em subgrupos em seus dados, use a `by` palavra-chave. Por exemplo, para contar o número de computadores Linux distintos que enviaram pulsações em cada país:

```Kusto
Heartbeat 
| where TimeGenerated > ago(1h) 
| summarize distinct_computers=dcountif(Computer, OSType=="Linux") by RemoteIPCountry
```

|RemoteIPCountry  | distinct_computers  |
------------------|---------------------|
|Estados Unidos    | 19                  |
|Canadá           | 3                   |
|Irlanda          | 0                   |
|Reino Unido   | 0                   |
|Países Baixos      | 2                   |


Para analisar subgrupos ainda menores de seus dados, adicione nomes de coluna adicionais à seção`by`. Por exemplo, você pode querer contar os computadores distintos de cada país por OSType:

```Kusto
Heartbeat 
| where TimeGenerated > ago(1h) 
| summarize distinct_computers=dcountif(Computer, OSType=="Linux") by RemoteIPCountry, OSType
```

## <a name="percentiles-and-variance"></a>Percentis e Variação
Ao avaliar valores numéricos, uma prática comum é calculá-los usando `summarize avg(expression)`. As médias são afetadas por valores extremos que caracterizam apenas alguns casos. Para resolver esse problema, você pode usar funções menos sensíveis, como `median` ou `variance`.

### <a name="percentile"></a>Percentil
Para encontrar o valor mediano, use a função `percentile` com um valor para especificar o percentil:

```Kusto
Perf
| where TimeGenerated > ago(30m) 
| where CounterName == "% Processor Time" and InstanceName == "_Total" 
| summarize percentiles(CounterValue, 50) by Computer
```

Você também pode especificar diferentes percentis para obter um resultado agregado para cada um:

```Kusto
Perf
| where TimeGenerated > ago(30m) 
| where CounterName == "% Processor Time" and InstanceName == "_Total" 
| summarize percentiles(CounterValue, 25, 50, 75, 90) by Computer
```

Isso pode mostrar que algumas CPUs de computadores têm valores medianos semelhantes, mas enquanto algumas estão em torno da mediana, outros computadores relataram valores de CPU muito menores e mais altos, o que significa que experimentaram picos.

### <a name="variance"></a>Variação
Para avaliar diretamente a variância de um valor, use os métodos de desvio padrão e variância:

```Kusto
Perf
| where TimeGenerated > ago(30m) 
| where CounterName == "% Processor Time" and InstanceName == "_Total" 
| summarize stdev(CounterValue), variance(CounterValue) by Computer
```

Uma boa maneira de analisar a estabilidade do uso da CPU é combinar stdev com o cálculo mediano:

```Kusto
Perf
| where TimeGenerated > ago(130m) 
| where CounterName == "% Processor Time" and InstanceName == "_Total" 
| summarize stdev(CounterValue), percentiles(CounterValue, 50) by Computer
```

Consulte outras lições para usar a linguagem de consulta do Log Analytics:

- [Operações de cadeia de caracteres](string-operations.md)
- [Operações de data e hora](datetime-operations.md)
- [Agregações avançadas](advanced-aggregations.md)
- [JSON e estruturas de dados](json-data-structures.md)
- [Gravação de consulta avançada](advanced-query-writing.md)
- [Junções](joins.md)
- [Gráficos](charts.md)
