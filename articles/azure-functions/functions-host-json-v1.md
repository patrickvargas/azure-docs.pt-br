---
title: Referência de host.json para as funções do Azure 1.x
description: Documentação de referência para o arquivo host.json do Azure Functions com o runtime v1.
services: functions
author: ggailey777
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 10/19/2018
ms.author: glenga
ms.openlocfilehash: ee82aab37973117b0c1960d8b75a29bfad38b7c7
ms.sourcegitcommit: 1d3353b95e0de04d4aec2d0d6f84ec45deaaf6ae
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/30/2018
ms.locfileid: "50252068"
---
# <a name="hostjson-reference-for-azure-functions-1x"></a>Referência de host.json para as funções do Azure 1.x

> [!div class="op_single_selector" title1="Select the version of the Azure Functions runtime you are using: "]
> * [Versão 1](functions-host-json-v1.md)
> * [Versão 2](functions-host-json.md)

O arquivo de metadados *host.json* contém opções de configuração global que afetam todas as funções de um aplicativo de funções. Este artigo lista as configurações disponíveis para o tempo de execução v1. O esquema JSON está em http://json.schemastore.org/host.

> [!NOTE]
> Este artigo serve para o Azure Functions 1.x.  Para obter uma referência de host.json em Funções 2.x, consulte a referência [host.json para as Funções do Azure 2.x](functions-host-json.md).

Outras opções de configuração de aplicativo de funções são gerenciadas nas [configurações de aplicativo](functions-app-settings.md).

Algumas configurações host.json são usadas apenas quando executadas localmente no arquivo [local.settings.json](functions-run-local.md#local-settings-file).

## <a name="sample-hostjson-file"></a>Arquivo host.json de exemplo

Os seguintes arquivos *host.json* de exemplo têm todas as opções possíveis especificadas.


```json
{
    "aggregator": {
        "batchSize": 1000,
        "flushTimeout": "00:00:30"
    },
    "applicationInsights": {
        "sampling": {
          "isEnabled": true,
          "maxTelemetryItemsPerSecond" : 5
        }
    },
    "eventHub": {
      "maxBatchSize": 64,
      "prefetchCount": 256,
      "batchCheckpointFrequency": 1
    },
    "functions": [ "QueueProcessor", "GitHubWebHook" ],
    "functionTimeout": "00:05:00",
    "healthMonitor": {
        "enabled": true,
        "healthCheckInterval": "00:00:10",
        "healthCheckWindow": "00:02:00",
        "healthCheckThreshold": 6,
        "counterThreshold": 0.80
    },
    "http": {
        "routePrefix": "api",
        "maxOutstandingRequests": 20,
        "maxConcurrentRequests": 10,
        "dynamicThrottlesEnabled": false
    },
    "id": "9f4ea53c5136457d883d685e57164f08",
    "logger": {
        "categoryFilter": {
            "defaultLevel": "Information",
            "categoryLevels": {
                "Host": "Error",
                "Function": "Error",
                "Host.Aggregator": "Information"
            }
        }
    },
    "queues": {
      "maxPollingInterval": 2000,
      "visibilityTimeout" : "00:00:30",
      "batchSize": 16,
      "maxDequeueCount": 5,
      "newBatchThreshold": 8
    },
    "serviceBus": {
      "maxConcurrentCalls": 16,
      "prefetchCount": 100,
      "autoRenewTimeout": "00:05:00"
    },
    "singleton": {
      "lockPeriod": "00:00:15",
      "listenerLockPeriod": "00:01:00",
      "listenerLockRecoveryPollingInterval": "00:01:00",
      "lockAcquisitionTimeout": "00:01:00",
      "lockAcquisitionPollingInterval": "00:00:03"
    },
    "tracing": {
      "consoleLevel": "verbose",
      "fileLoggingMode": "debugOnly"
    },
    "watchDirectories": [ "Shared" ],
}
```

As seções seguintes deste artigo explicam cada propriedade de nível superior. Todas são opcionais, a menos que seja indicado o contrário.

## <a name="aggregator"></a>agregador

[!INCLUDE [aggregator](../../includes/functions-host-json-aggregator.md)]

## <a name="applicationinsights"></a>applicationInsights

[!INCLUDE [applicationInsights](../../includes/functions-host-json-applicationinsights.md)]

## <a name="durabletask"></a>durableTask

[!INCLUDE [durabletask](../../includes/functions-host-json-durabletask.md)]

## <a name="eventhub"></a>eventHub

Definições de configuração para [gatilhos e associações de Hub de Eventos](functions-bindings-event-hubs.md).

[!INCLUDE [functions-host-json-event-hubs](../../includes/functions-host-json-event-hubs.md)]

## <a name="functions"></a>funções

Uma lista de funções que o host de trabalho executa. Uma matriz vazia significa que todas as funções serão executadas. Para uso somente quando [em execução localmente](functions-run-local.md). Em aplicativos de funções no Azure, você deve seguir as etapas em [Como desabilitar funções no Azure Functions](disable-function.md) para desabilitar funções específicas em vez de usar essa configuração.

```json
{
    "functions": [ "QueueProcessor", "GitHubWebHook" ]
}
```

## <a name="functiontimeout"></a>functionTimeout

Indica a duração do tempo limite para todas as funções. Em um plano de Consumo sem servidor, o intervalo válido é de 1 segundo a 10 minutos e o valor padrão é 5 minutos. Em um Plano do Serviço de Aplicativo, não há limite geral e o padrão depende da versão do tempo de execução.

```json
{
    "functionTimeout": "00:05:00"
}
```

## <a name="healthmonitor"></a>healthMonitor

Definições de configuração para [monitor de integridade de Host](https://github.com/Azure/azure-webjobs-sdk-script/wiki/Host-Health-Monitor).

```
{
    "healthMonitor": {
        "enabled": true,
        "healthCheckInterval": "00:00:10",
        "healthCheckWindow": "00:02:00",
        "healthCheckThreshold": 6,
        "counterThreshold": 0.80
    }
}
```

|Propriedade  |Padrão | DESCRIÇÃO |
|---------|---------|---------| 
|Habilitado|verdadeiro|Especifica se o recurso está habilitado. | 
|healthCheckInterval|10 segundos|O intervalo de tempo entre as verificações de integridade em segundo plano. | 
|healthCheckWindow|2 minutos|Uma janela de tempo deslizante usada em conjunto com a configuração `healthCheckThreshold`.| 
|healthCheckThreshold|6|Número máximo de vezes que a verificação de integridade pode falhar antes de uma reciclagem de host ser iniciada.| 
|counterThreshold|0.80|O limite no qual um contador de desempenho será considerado não íntegro.| 

## <a name="http"></a>http

Parâmetros de configuração para [gatilhos e associações http](functions-bindings-http-webhook.md).

[!INCLUDE [functions-host-json-http](../../includes/functions-host-json-http.md)]

## <a name="id"></a>ID

*Versão 1.x, somente.*

A ID exclusiva do host de trabalho. Pode ser uma GUID em letras minúsculas, sem traços. Obrigatório ao executar localmente. Ao executar no Azure, é recomendável que você não defina um valor de ID. Uma ID é gerada automaticamente no Azure quando `id` está omitido. 

Se você compartilhar uma conta de Armazenamento em vários aplicativos de funções, verifique se cada aplicativo de função tem um `id` diferente. É possível omitir a propriedade `id` ou definir manualmente cada aplicativo de funções `id` para um valor diferente. O gatilho de temporizador usa um bloqueio de armazenamento para garantir que haverá apenas uma instância de temporizador quando um aplicativo de funções escalar horizontalmente para várias instâncias. Se dois aplicativos de funções compartilharem o mesmo `id` e cada um usar um gatilho de temporizador, somente um temporizador irá executar.

```json
{
    "id": "9f4ea53c5136457d883d685e57164f08"
}
```

## <a name="logger"></a>agente

Controles de filtragem de logs gravados por um [objeto ILogger](functions-monitoring.md#write-logs-in-c-functions) ou por [context.log](functions-monitoring.md#write-logs-in-javascript-functions).

```json
{
    "logger": {
        "categoryFilter": {
            "defaultLevel": "Information",
            "categoryLevels": {
                "Host": "Error",
                "Function": "Error",
                "Host.Aggregator": "Information"
            }
        }
    }
}
```

|Propriedade  |Padrão | DESCRIÇÃO |
|---------|---------|---------| 
|categoryFilter|n/d|Especifica a filtragem por categoria| 
|defaultLevel|Informações|Para as categorias não especificadas na matriz `categoryLevels`, envie logs nesse nível e acima para o Application Insights.| 
|categoryLevels|n/d|Uma matriz de categorias que especifica o nível mínimo de logs que será enviado ao Application Insights para cada categoria. A categoria especificada aqui controla todas as categorias que começam com o mesmo valor, com precedência para os valores maiores. No arquivo de exemplo *host.json* anterior, todas as categorias que começam com o log "Host.Aggregator" no nível `Information`. Todas as outras categorias que começam com o log "Host", como "Host.Executor", no nível `Error`.| 

## <a name="queues"></a>filas

Parâmetros de configuração para [gatilhos e associações de Armazenamento](functions-bindings-storage-queue.md).

[!INCLUDE [functions-host-json-queues](../../includes/functions-host-json-queues.md)]

## <a name="servicebus"></a>serviceBus

Parâmetro de configuração para [gatilhos e associações do Barramento de Serviço](functions-bindings-service-bus.md).

[!INCLUDE [functions-host-json-service-bus](../../includes/functions-host-json-service-bus.md)]

## <a name="singleton"></a>singleton

Parâmetro de configuração para o comportamento de bloqueio de Singleton. Para obter mais informações, consulte [Problema com o GitHub referente ao suporte de singleton](https://github.com/Azure/azure-webjobs-sdk-script/issues/912).

```json
{
    "singleton": {
      "lockPeriod": "00:00:15",
      "listenerLockPeriod": "00:01:00",
      "listenerLockRecoveryPollingInterval": "00:01:00",
      "lockAcquisitionTimeout": "00:01:00",
      "lockAcquisitionPollingInterval": "00:00:03"
    }
}
```

|Propriedade  |Padrão | DESCRIÇÃO |
|---------|---------|---------| 
|lockPeriod|00:00:15|O período em que ocorrem os bloqueios de nível de função. Os bloqueios têm renovação automática.| 
|listenerLockPeriod|00:01:00|O período em que ocorrem os bloqueios de ouvinte.| 
|listenerLockRecoveryPollingInterval|00:01:00|O intervalo de tempo usado para a recuperação do bloqueio de ouvinte caso não tenha sido possível adquirir um bloqueio de ouvinte durante a inicialização.| 
|lockAcquisitionTimeout|00:01:00|A quantidade máxima de tempo em que o tempo de execução tenta adquirir um bloqueio.| 
|lockAcquisitionPollingInterval|n/d|O intervalo entre as tentativas de aquisição de bloqueio.| 

## <a name="tracing"></a>rastreamento

*Versão 1.x*

Parâmetros de configuração para logs que você cria usando um objeto `TraceWriter`. Consulte [Registro de logs em C#](functions-reference-csharp.md#logging) e [Registro de logs em Node.js](functions-reference-node.md#writing-trace-output-to-the-console).

```json
{
    "tracing": {
      "consoleLevel": "verbose",
      "fileLoggingMode": "debugOnly"
    }
}
```

|Propriedade  |Padrão | DESCRIÇÃO |
|---------|---------|---------| 
|consoleLevel|informações|O nível de rastreamento para o registro em log no console. As opções são: `off`, `error`, `warning`, `info` e `verbose`.|
|fileLoggingMode|debugOnly|O nível de rastreamento para registros em log de arquivo. As opções são: `never`, `always` e `debugOnly`.| 

## <a name="watchdirectories"></a>watchDirectories

Um conjunto de [diretórios de código compartilhado](functions-reference-csharp.md#watched-directories) que devem ser monitorados quanto a alterações.  Garante que, quando o código nesses diretórios é alterado, as alterações sejam coletadas pelas funções.

```json
{
    "watchDirectories": [ "Shared" ]
}
```

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Saiba como atualizar o arquivo host.json](functions-reference.md#fileupdate)

> [!div class="nextstepaction"]
> [Consulte as configurações globais em variáveis de ambiente](functions-app-settings.md)
