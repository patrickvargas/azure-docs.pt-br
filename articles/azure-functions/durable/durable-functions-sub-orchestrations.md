---
title: Suborquestrações para Funções Duráveis – Azure
description: Como chamar orquestrações de orquestrações na extensão de Funções Duráveis do Azure Functions.
services: functions
author: kashimiz
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 10/23/2018
ms.author: azfuncdf
ms.openlocfilehash: 32f8872737fdf6dd766ae8df8ef3ed47692e2c9c
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/30/2018
ms.locfileid: "52636961"
---
# <a name="sub-orchestrations-in-durable-functions-azure-functions"></a>Suborquestrações em Funções Duráveis (Azure Functions)

Além de chamar funções de atividade, as funções de orquestrador podem chamar outras funções de orquestrador. Por exemplo, você pode criar uma orquestração maior usando uma biblioteca de funções de orquestrador. Ou pode executar várias instâncias de uma função de orquestrador em paralelo.

Uma função de orquestrador pode chamar outra função de orquestrador chamando os métodos [CallSubOrchestratorAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CallSubOrchestratorAsync_) ou [CallSubOrchestratorWithRetryAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CallSubOrchestratorWithRetryAsync_). O artigo [Tratamento de Erro e Compensação](durable-functions-error-handling.md#automatic-retry-on-failure) fornece mais informações sobre a repetição automática.

Da perspectiva do chamador, as funções de suborquestrador se comportam como funções de atividade. Elas podem retornar um valor, lançar uma exceção e podem ser aguardadas pela função de orquestrador pai.

> [!NOTE]
> Os métodos `CallSubOrchestratorAsync` e `CallSubOrchestratorWithRetryAsync` ainda não estão disponíveis em JavaScript.

## <a name="example"></a>Exemplo

O exemplo a seguir ilustra um cenário de IoT ("Internet das Coisas") em que vários dispositivos precisam ser provisionados. Há uma orquestração específica que deve acontecer para cada um dos dispositivos, que pode ser algo semelhante ao seguinte:

#### <a name="c"></a>C#

```csharp
public static async Task DeviceProvisioningOrchestration(
    [OrchestrationTrigger] DurableOrchestrationContext ctx)
{
    string deviceId = ctx.GetInput<string>();

    // Step 1: Create an installation package in blob storage and return a SAS URL.
    Uri sasUrl = await ctx.CallActivityAsync<Uri>("CreateInstallationPackage", deviceId);

    // Step 2: Notify the device that the installation package is ready.
    await ctx.CallActivityAsync("SendPackageUrlToDevice", Tuple.Create(deviceId, sasUrl));

    // Step 3: Wait for the device to acknowledge that it has downloaded the new package.
    await ctx.WaitForExternalEvent<bool>("DownloadCompletedAck");

    // Step 4: ...
}
```

#### <a name="javascript-functions-v2-only"></a>JavaScript (apenas Functions v2)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const deviceId = context.df.getInput();

    // Step 1: Create an installation package in blob storage and return a SAS URL.
    const sasUrl = yield context.df.callActivity("CreateInstallationPackage", deviceId);

    // Step 2: Notify the device that the installation package is ready.
    yield context.df.callActivity("SendPackageUrlToDevice", { id: deviceId, url: sasUrl });

    // Step 3: Wait for the device to acknowledge that it has downloaded the new package.
    yield context.df.waitForExternalEvent("DownloadCompletedAck");

    // Step 4: ...
});
```

Essa função de orquestrador pode ser usada da forma como está para o provisionamento de dispositivos individuais ou pode fazer parte de uma orquestração maior. Nesse último caso, a função de orquestrador pai pode agendar instâncias de `DeviceProvisioningOrchestration` usando a API `CallSubOrchestratorAsync` (C#) ou `callSubOrchestrator` (JS).

Este é um exemplo que mostra como executar várias funções de orquestrador em paralelo.

#### <a name="c"></a>C#

```csharp
[FunctionName("ProvisionNewDevices")]
public static async Task ProvisionNewDevices(
    [OrchestrationTrigger] DurableOrchestrationContext ctx)
{
    string[] deviceIds = await ctx.CallActivityAsync<string[]>("GetNewDeviceIds");

    // Run multiple device provisioning flows in parallel
    var provisioningTasks = new List<Task>();
    foreach (string deviceId in deviceIds)
    {
        Task provisionTask = ctx.CallSubOrchestratorAsync("DeviceProvisioningOrchestration", deviceId);
        provisioningTasks.Add(provisionTask);
    }

    await Task.WhenAll(provisioningTasks);

    // ...
}
```

#### <a name="javascript-functions-v2-only"></a>JavaScript (apenas Functions v2)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const deviceIds = yield context.df.callActivity("GetNewDeviceIds");

    // Run multiple device provisioning flows in parallel
    const provisioningTasks = [];
    for (const deviceId of deviceIds) {
        const provisionTask = context.df.callSubOrchestrator("DeviceProvisioningOrchestration", deviceId);
        provisioningTasks.push(provisionTask);
    }

    yield context.df.Task.all(provisioningTasks);

    // ...
});
```

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Saiba o que são hubs de tarefas e como configurá-los](durable-functions-task-hubs.md)
