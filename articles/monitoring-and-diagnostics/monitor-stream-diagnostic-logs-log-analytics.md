---
title: Transmitir logs de diagnóstico do Azure para o Log Analytics
description: Saiba como transmitir logs de diagnóstico do Azure para um workspace do Log Analytics.
author: johnkemnetz
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 04/04/2018
ms.author: johnkem
ms.component: logs
ms.openlocfilehash: fe1557a6f9e5fd4e463af254fa1dd52726e73024
ms.sourcegitcommit: a4e4e0236197544569a0a7e34c1c20d071774dd6
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51713037"
---
# <a name="stream-azure-diagnostic-logs-to-log-analytics"></a>Transmitir logs de diagnóstico do Azure para o Log Analytics

**[Os Logs de diagnósticos do Azure](monitoring-overview-of-diagnostic-logs.md)** podem ser transmitidos quase em tempo real para o Azure Log Analytics usando o portal, cmdlets do PowerShell ou a CLI do Azure.

## <a name="what-you-can-do-with-diagnostics-logs-in-log-analytics"></a>O que você pode fazer com os logs de diagnóstico no Log Analytics

O Azure Log Analytics é uma ferramenta de análise e pesquisa de logs flexível que permite que você obtenha informações sobre os dados brutos de log gerados de recursos do Azure. Algumas funcionalidades incluem:

* **Pesquisa de logs** -gravar de consultas avançadas sobre os dados de log, correlacionar logs de várias origens e até mesmo gerar gráficos que podem ser fixados no painel do Azure.
* **Alerta** -detectar quando um ou mais eventos corresponderem a uma consulta específica e ser notificados com uma chamada de webhook ou email.
* **Soluções** -usar exibições predefinidas e painéis que fornecem ideias imediatas sobre seus dados de log.
* **Análise avançada** – aplicar o aprendizado de máquina e algoritmos de correspondência de padrão para identificar possíveis problemas revelados por seus logs.

## <a name="enable-streaming-of-diagnostic-logs-to-log-analytics"></a>Habilitar o streaming de logs de diagnóstico para o Log Analytics

Você pode habilitar programaticamente o streaming de logs de diagnóstico por meio do portal ou usando a [API REST do Azure Monitor](https://docs.microsoft.com/rest/api/monitor/diagnosticsettings). De qualquer forma, você cria uma configuração de diagnóstico na qual especifica um workspace do Log Analytics e as categorias de log e as métricas que deseja enviar para esse workspace. Uma **categoria de log** de diagnóstico é um tipo de log que um recurso pode fornecer.

O workspace do Log Analytics não precisa estar na mesma assinatura que o recurso que emite os logs, contanto que o usuário que define a configuração tenha acesso RBAC apropriado a ambas as assinaturas.

> [!NOTE]
> Atualmente, não há suporte para o envio da métrica multidimensional por meio das configurações de diagnóstico. As métricas com dimensões são exportadas como métricas dimensionais simples, agregadas nos valores da dimensão.
>
> *Por exemplo*: a métrica “Mensagens de Entrada” em um Hub de Eventos pode ser explorada e mapeada por nível da fila. No entanto, quando exportada por meio das configurações de diagnóstico, a métrica será representada como todas as mensagens de entrada em todas as filas no Hub de Eventos.
>
>

## <a name="stream-diagnostic-logs-using-the-portal"></a>Transmitir logs de diagnóstico usando o portal
1. No portal, navegue até o Azure Monitor e clique em **Configurações de Diagnóstico**

    ![Seção de monitoramento do Azure Monitor](media/monitor-stream-diagnostic-logs-log-analytics/diagnostic-settings-blade.png)

2. Se desejar filtrar a lista por tipo de recurso ou grupo de recursos, clique no recurso para o qual você deseja definir uma configuração de diagnóstica.

3. Se nenhuma configuração existir no recurso que você selecionou, será solicitada a criação de uma configuração. Clique em “Ativar diagnóstico”.

   ![Adicionar configuração de diagnóstico - nenhuma configuração existente](media/monitor-stream-diagnostic-logs-log-analytics/diagnostic-settings-none.png)

   Se houver configurações existentes no recurso, você verá uma lista de configurações já definidas nesse recurso. Clique em "Adicionar configuração de diagnóstico".

   ![Adicionar configuração de diagnóstico - configurações existentes](media/monitor-stream-diagnostic-logs-log-analytics/diagnostic-settings-multiple.png)

3. Dê um nome à sua configuração e marque a caixa **Enviar para o Log Analytics**, em seguida, selecione um workspace do Log Analytics.

   ![Adicionar configuração de diagnóstico - configurações existentes](media/monitor-stream-diagnostic-logs-log-analytics/diagnostic-settings-configure.png)

4. Clique em **Salvar**.

Após alguns instantes, a nova configuração aparece na lista de configurações para esse recurso e os logs de diagnóstico são transmitidos para esse workspace assim que os novos dados de evento são gerados. Observe que pode haver até quinze minutos entre quando um evento é emitido e quando ele é exibido no Log Analytics.

### <a name="via-powershell-cmdlets"></a>Via Cmdlets do PowerShell
Para habilitar o streaming por meio de [Cmdlets do Azure PowerShell](insights-powershell-samples.md), use o cmdlet `Set-AzureRmDiagnosticSetting` com estes parâmetros:

```powershell
Set-AzureRmDiagnosticSetting -ResourceId [your resource ID] -WorkspaceID [resource ID of the Log Analytics workspace] -Categories [list of log categories] -Enabled $true
```

Observe que a propriedade workspaceID usa a ID de recurso completa do Azure do workspace, não a ID/chave do workspace mostrada no portal do Log Analytics.

### <a name="via-azure-cli"></a>Via CLI do Azure

Para habilitar o streaming por meio da [CLI do Azure](insights-cli-samples.md), você pode usar o comando [az monitor diagnostic-settings create](/cli/azure/monitor/diagnostic-settings#az-monitor-diagnostic-settings-create).

```azurecli
az monitor diagnostic-settings create --name <diagnostic name> \
    --workspace <log analytics name or object ID> \
    --resource <target resource object ID> \
    --resource-group <log analytics workspace resource group> \
    --logs '[
    {
        "category": <category name>,
        "enabled": true
    }
    ]'
```

É possível adicionar categorias adicionais ao log de diagnóstico, adicionando dicionários à matriz JSON transmitida como o parâmetro `--logs`.

O argumento `--resource-group` somente será necessário se `--workspace` não for uma ID de objeto.

## <a name="how-do-i-query-the-data-in-log-analytics"></a>Como faço para consultar os dados do Log Analytics?

Na folha de Pesquisa de Logs no portal ou na experiência do Advanced Analytics como parte do Log Analytics, você pode consultar os logs de diagnóstico como parte da solução de Gerenciamento de Log na tabela do AzureDiagnostics. Também há [várias soluções para recursos do Azure](../azure-monitor/insights/solutions.md) que podem ser instaladas para obter informações imediatas sobre os dados de log que você está enviando para o Log Analytics.

## <a name="next-steps"></a>Próximas etapas

* [Saiba mais sobre os Logs de Diagnóstico do Azure](monitoring-overview-of-diagnostic-logs.md)
