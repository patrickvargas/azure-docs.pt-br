---
title: Perguntas frequentes sobre Análise de Tráfego do Azure | Microsoft Docs
description: Obtenha respostas para algumas perguntas frequentes sobre a Análise de Tráfego.
services: network-watcher
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/08/2018
ms.author: jdial
ms.openlocfilehash: 69d2d80e40400cc7fa40aeb5a163dce5036905ab
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/18/2018
ms.locfileid: "49402753"
---
# <a name="traffic-analytics-frequently-asked-questions"></a>Perguntas frequentes sobre Análise de Tráfego

Este artigo coleta em um só lugar muitas das perguntas mais freqüentes sobre análise de tráfego no Observador da Rede do Azure.

## <a name="what-are-the-prerequisites-to-use-traffic-analytics"></a>Quais são os pré-requisitos para usar a análise de tráfego?

A análise de tráfego requer os seguintes pré-requisitos:

- Uma assinatura ativada do Watcher de Rede.
- Os logs de fluxo do Network Security Group (NSG) habilitados para os NSGs que você deseja monitorar.
- Uma conta de Armazenamento do Azure, para armazenar logs de flog brutos.
- Um workspace do Azure Log Analytics, com acesso de leitura e gravação.

Sua conta deve atender a uma das seguintes opções para ativar a análise de tráfego:

- Sua conta deve ser atribuída a uma das seguintes funções no nível da assinatura: administrador da conta, administrador do serviço ou co-administrador.
- Sua conta deve ter qualquer uma das seguintes funções de RBAC (controle de acesso baseado em função) no escopo da assinatura: proprietário, colaborador, leitor ou colaborador de rede.
- Se sua conta não estiver atribuída a uma das funções listadas anteriormente, ela deverá ser atribuída a uma função personalizada à qual são atribuídas as ações a seguir, no nível da assinatura.
            
    - Microsoft.Network/applicationGateways/read
    - Microsoft.Network/connections/read
    - Microsoft.Network/loadBalancers/read 
    - Microsoft.Network/localNetworkGateways/read 
    - Microsoft.Network/networkInterfaces/read 
    - Microsoft.Network/networkSecurityGroups/read 
    - Microsoft.Network/publicIPAddresses/read
    - Microsoft.Network/routeTables/read
    - Microsoft.Network/virtualNetworkGateways/read 
    - Microsoft.Network/virtualNetworks/read
        
Para verificar funções atribuídas a um usuário para uma inscrição:

1. Entre no Azure usando **Login-AzureRmAccount**. 

2. Selecione a assinatura necessária usando **Select-AzureRmSubscription**. 

3. Para listar todas as funções atribuídas a um usuário especificado, use **Get-AzureRmRoleAssignment -SignInName [email do usuário] -IncludeClassicAdministrators**. 

Se você não estiver vendo nenhuma saída, entre em contato com o administrador da assinatura respectiva para obter acesso para executar os comandos. Para obter mais detalhes, consulte [ Gerenciar controle de acesso baseado em função com o Azure PowerShell ](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-powershell).


## <a name="in-which-azure-regions-are-traffic-analytics-available"></a>Em quais regiões do Azure estão disponíveis análises de tráfego?

Você pode usar análises de tráfego para NSGs em qualquer uma das seguintes regiões suportadas: centro-oeste dos EUA, leste dos EUA, leste dos EUA 2, centro-norte dos EUA, centro-sul dos EUA, centro dos EUA, oeste dos EUA, oeste dos EUA 2, Europa Ocidental, norte da Europa Reino Unido Oeste, Reino Unido Sul, Austrália Leste, Austrália Sudeste e Sudeste Asiático. O workspace do Log Analytics deve existir na região Oeste Central dos EUA, Leste dos EUA, Europa Ocidental, Sul do Reino Unido, Sudeste da Austrália ou região do Sudeste Asiático.

## <a name="can-the-nsgs-i-enable-flow-logs-for-be-in-different-regions-than-my-workspace"></a>Os NSGs para os quais eu habilito logs de fluxo podem estar em regiões diferentes do meu workspace?

Sim, esses NSGs podem estar em regiões diferentes do seu workspace do Log Analytics.

## <a name="can-multiple-nsgs-be-configured-within-a-single-workspace"></a>Vários NSGs podem ser configurados dentro de um único workspace?

Sim.

## <a name="can-i-use-an-existing-workspace"></a>Posso usar um workspace existente?

Sim. Se você selecionar um workspace existente, verifique se ele foi migrado para o novo idioma de consulta. Se você não quiser atualizar o workspace, precisará criar um novo. Para obter mais informações sobre a nova linguagem de consulta, consulte [Atualização do Azure Log Analytics para a nova pesquisa de logs ](../log-analytics/log-analytics-log-search-upgrade.md).

## <a name="can-my-azure-storage-account-be-in-one-subscription-and-my-log-analytics-workspace-be-in-a-different-subscription"></a>A minha Conta de Armazenamento do Microsoft Azure pode estar em uma assinatura e meu espaço de trabalho do Log Analytics está em uma assinatura diferente?

Sim, sua conta de Armazenamento do Microsoft Azure pode estar em uma assinatura e meu espaço de trabalho do Log Analytics estar em uma assinatura diferente.

## <a name="can-i-store-raw-logs-in-a-different-subscription"></a>Posso armazenar logs brutos em uma assinatura diferente?

Não. Você pode armazenar logs brutos em qualquer conta de armazenamento em que um NSG está habilitado para logs de fluxo. No entanto, a conta de armazenamento e os logs brutos devem estar na mesma assinatura e região.

## <a name="what-if-i-cant-configure-an-nsg-for-traffic-analytics-due-to-a-not-found-error"></a>E se eu não conseguir configurar um NSG para análise de tráfego devido a um erro "Não encontrado"?

Selecione uma região suportada. Se você selecionar uma região sem suporte, receberá um erro "Não encontrado". As regiões suportadas são listadas anteriormente neste artigo.

## <a name="what-if-i-am-getting-the-status-failed-to-load-under-the-nsg-flow-logs-page"></a>E se eu estiver recebendo o status "Falha ao carregar" na página de logs de fluxo do NSG?

O provedor Microsoft.Insights deve estar registrado para que o log de fluxo funcione corretamente. Se você não tiver certeza se o provedor Microsoft.Insights está registrado para sua assinatura, substitua *xxxxx-xxxxx-xxxxxx-xxxx* no seguinte comando e execute os seguintes comandos do PowerShell:

```powershell-interactive
**Select-AzureRmSubscription** -SubscriptionId xxxxx-xxxxx-xxxxxx-xxxx
**Register-AzureRmResourceProvider** -ProviderNamespace Microsoft.Insights
```

## <a name="i-have-configured-the-solution-why-am-i-not-seeing-anything-on-the-dashboard"></a>Configurei a solução. Por que não visualizo nada no painel?

O painel pode levar até 30 minutos para aparecer pela primeira vez. A solução deve primeiro agregar dados suficientes para obter insights significativos. Então gera relatórios. 

## <a name="what-if-i-get-this-message-we-could-not-find-any-data-in-this-workspace-for-selected-time-interval-try-changing-the-time-interval-or-select-a-different-workspace"></a>E se eu receber esta mensagem: “Não conseguimos encontrar nenhum dado neste workspace para o intervalo de tempo selecionado. Tente alterar o intervalo de tempo ou selecione um workspace diferente. ”?

Experimente as opções a seguir:
- Altere o intervalo de tempo na barra superior.
- Selecione um workspace do Log Analytics diferente na barra superior.
- Tente acessar a análise de tráfego após 30 minutos, se ela foi ativada recentemente.
    
Se os problemas persistirem, aumente as preocupações no [Fórum de voz do usuário](https://feedback.azure.com/forums/217313-networking?category_id=195844).

## <a name="what-if-i-get-this-message-analyzing-your-nsg-flow-logs-for-the-first-time-this-process-may-take-20-30-minutes-to-complete-check-back-after-some-time-2-if-the-above-step-doesnt-work-and-your-workspace-is-under-the-free-sku-then-check-your-workspace-usage-here-to-validate-over-quota-else-refer-to-faqs-for-further-information"></a>E se eu receber esta mensagem: “Analisando seus logs de fluxo NSG pela primeira vez. Esse processo pode demorar de 20 a 30 minutos para ser concluído. Verifique novamente após algum tempo. 2) Se a etapa acima não funcionar e seu workspace estiver sob o SKU gratuito, verifique aqui o uso do seu workspace para validar a cota, caso contrário, consulte as Perguntas frequentes para obter mais informações.”

Você pode ver esta mensagem porque:
- A análise de tráfego foi ativada recentemente e pode ainda não ter agregado dados suficientes para obter insights significativos.
- Você está usando a versão gratuita do espaço de trabalho do Log Analytics e excedeu os limites de cota. Pode ser necessário usar um workspace com uma capacidade maior.
    
Se os problemas persistirem, aumente as preocupações no [Fórum de voz do usuário](https://feedback.azure.com/forums/217313-networking?category_id=195844).
    
## <a name="what-if-i-get-this-message-looks-like-we-have-resources-data-topology-and-no-flows-information-meanwhile-click-here-to-see-resources-data-and-refer-to-faqs-for-further-information"></a>E se eu receber esta mensagem: “Parece que temos dados de recursos (Topologia) e nenhuma informação de fluxos. Enquanto isso, clique aqui para ver os dados de recursos e consulte as perguntas frequentes para obter mais informações ”?

Você está vendo a informação de recursos no painel, no entanto, não há estatísticas relacionadas ao fluxo presentes. Os dados podem não estar presentes porque não há fluxos de comunicação entre os recursos. Aguarde 60 minutos e volte a verificar o estado. Se o problema persistir e você tiver certeza de que existem fluxos de comunicação entre recursos, aumente as preocupações no [Fórum de voz do usuário](https://feedback.azure.com/forums/217313-networking?category_id=195844).

## <a name="can-i-configure-traffic-analytics-using-powershell-or-an-azure-resource-manager-template-or-client"></a>Posso configurar a análise de tráfego usando o PowerShell ou um modelo ou cliente do Azure Resource Manager?

Você pode configurar a análise de tráfego usando o Windows PowerShell a partir da versão 6.2.1. Para configurar o log de fluxo e a análise de tráfego para um NSG específico usando o cmdlet Set, consulte [Set-AzureRmNetworkWatcherConfigFlowLog](https://docs.microsoft.com/powershell/module/azurerm.network/set-azurermnetworkwatcherconfigflowlog?view=azurermps-6.3.0). Para obter o log de fluxo e o status de análise de tráfego para um NSG específico, consulte [Get-AzureRmNetworkWatcherFlowLogStatus](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermnetworkwatcherflowlogstatus?view=azurermps-6.3.0).

Atualmente, você não pode usar um modelo do Azure Resource Manager para configurar a análise de tráfego.

Para configurar a análise de tráfego usando um cliente do Azure Resource Manager, consulte os exemplos a seguir.

**Definir exemplo de cmdlet:**
```
#Requestbody parameters
$TAtargetUri ="/subscriptions/<NSG subscription id>/resourceGroups/<NSG resource group name>/providers/Microsoft.Network/networkSecurityGroups/<name of NSG>"
$TAstorageId = "/subscriptions/<storage subscription id>/resourcegroups/<storage resource group name> /providers/microsoft.storage/storageaccounts/<storage account name>"
$networkWatcherResourceGroupName = "<network watcher resource group name>"
$networkWatcherName = "<network watcher name>"

$requestBody = 
@"
{
    'targetResourceId': '${TAtargetUri}',
    'properties': 
    {
        'storageId': '${TAstorageId}',
        'enabled': '<true to enable flow log or false to disable flow log>',
        'retentionPolicy' : 
        {
            days: <enter number of days like to retail flow logs in storage account>,
            enabled: <true to enable retention or false to disable retention>
        }
    },
    'flowAnalyticsConfiguration':
    {
                'networkWatcherFlowAnalyticsConfiguration':
      {
        'enabled':,<true to enable traffic analytics or false to disable traffic analytics>
        'workspaceId':'bbbbbbbb-bbbb-bbbb-bbbb-bbbbbbbbbbbb',
        'workspaceRegion':'<workspace region>',
        'workspaceResourceId':'/subscriptions/<workspace subscription id>/resourcegroups/<workspace resource group name>/providers/microsoft.operationalinsights/workspaces/<workspace name>'
        
      }

    }
}
"@
$apiversion = "2016-09-01"

armclient login
armclient post "https://management.azure.com/subscriptions/<NSG subscription id>/resourceGroups/<network watcher resource group name>/providers/Microsoft.Network/networkWatchers/<network watcher name>/configureFlowlog?api-version=${apiversion}" $requestBody
```
**Obtenha um exemplo de cmdlet:**
```
#Requestbody parameters
$TAtargetUri ="/subscriptions/<NSG subscription id>/resourceGroups/<NSG resource group name>/providers/Microsoft.Network/networkSecurityGroups/<NSG name>"


$requestBody = 
@"
{
    'targetResourceId': '${TAtargetUri}'
}
“@


armclient login
armclient post "https://management.azure.com/subscriptions/<NSG subscription id>/resourceGroups/<network watcher resource group name>/providers/Microsoft.Network/networkWatchers/<network watcher name>/queryFlowLogStatus?api-version=${apiversion}" $requestBody
```



## <a name="how-is-traffic-analytics-priced"></a>Como a Análise de Tráfego é precificada?

A análise de tráfego é medida. A medição é baseada no processamento de dados de log de fluxo pelo serviço e no armazenamento dos logs aprimorados resultantes em um workspace do Log Analytics. 

Por exemplo, de acordo o [plano de preços](https://azure.microsoft.com/pricing/details/network-watcher/), considerando a região Central Oeste dos EUA, se os logs de fluxo de dados armazenados em uma conta de armazenamento processada pela Análise de Tráfego tiver 10 GB e os logs aprimorados ingeridos no workspace do Log Analytics tiver 1 GB, os encargos aplicáveis serão: 10 x 2,3$ + 1 x 2,76$ = 25,76$

## <a name="how-can-i-navigate-by-using-the-keyboard-in-the-geo-map-view"></a>Como posso navegar usando o teclado na vista de mapa geográfico?

A página do mapa geográfico contém duas seções principais:
    
- **Banner**: o banner na parte superior do mapa geográfico fornece botões para selecionar filtros de distribuição de tráfego (por exemplo, Implantação, Tráfego de países e Malicioso). Quando você seleciona um botão, o respectivo filtro é aplicado no mapa. Por exemplo, se você selecionar o botão Ativo, o mapa destacará os datacenters ativos em sua implantação.
- **Mapa**: abaixo do banner, a seção do mapa mostra a distribuição de tráfego entre os datacenters e países do Azure.
    
### <a name="keyboard-navigation-on-the-banner"></a>Navegação do teclado no banner
    
- Por padrão, a seleção na página de mapa geográfico do banner é o filtro "Azure DCs".
- Para ir para outro filtro, use a tecla `Tab` ou `Right arrow`. Para retroceder, use a tecla `Shift+Tab` ou a `Left arrow`tecla. A navegação para frente é da esquerda para a direita, seguida de cima para baixo.
- Pressione `Enter` ou a `Down` tecla de seta para aplicar o filtro selecionado. Com base na seleção e na implantação do filtro, um ou vários nós na seção do mapa são realçados.
- Para alternar entre banner e mapa, pressione `Ctrl+F6`.
        
### <a name="keyboard-navigation-on-the-map"></a>Navegação do teclado no mapa
    
- Depois de selecionar qualquer filtro no banner e pressionar `Ctrl+F6`, o foco será movido para um dos nós destacados (**datacenter do Azure** ou **País / Região**) na exibição do mapa.
- Para mover para outros nós destacados no mapa, use `Tab` ou a tecla `Right arrow` para o movimento de avanço. Use `Shift+Tab` ou a tecla `Left arrow` para movimento para trás.
- Para selecionar qualquer nó destacado no mapa, utilize a tecla `Enter` ou `Down arrow`.
- Na seleção de quaisquer desses nós, o foco move-se para a **Caixa de Ferramentas de Informações** do nó. Por padrão, o foco move-se para o botão próximo à **Caixa de Ferramentas de Informações**. Para mover-se ainda mais dentro da visão **Box**, use as teclas `Right arrow` e `Left arrow` para avançar e voltar, respectivamente. Pressionar `Enter` tem o mesmo efeito que selecionar o botão com foco na **Caixa de Ferramentas de Informações**.
- Quando você pressiona `Tab` enquanto o foco está na  **Caixa de Ferramentas de Informações**, o foco se move para os pontos finais no mesmo continente que o nó selecionado. Use as teclas `Right arrow` e `Left arrow` para percorrer esses endpoints.
- Para mover para outros pontos finais de fluxo ou clusters continentais, use `Tab` para o movimento de avanço e `Shift+Tab` para o movimento de retrocesso.
- Quando o foco estiver em **Continent clusters**, use as teclas de seta `Enter` ou `Down` para realçar os pontos de extremidade dentro do cluster do continente. Para percorrer os endpoints e o botão fechar na caixa de informações do cluster do continente, use a tecla `Right arrow` ou `Left arrow` para o movimento de avanço e retrocesso, respectivamente. Em qualquer ponto de extremidade você poderá utilizar `Shift+L` para alternar para a linha de conexão do nó selecionado para o ponto de extremidade. Você pode pressionar `Shift+L` novamente para ir para o terminal selecionado.
        
### <a name="keyboard-navigation-at-any-stage"></a>Navegação do teclado em qualquer estágio
    
- `Esc` recolhe a seleção expandida.
- A tecla `Up arrow` realiza a mesma ação que `Esc`. A tecla `Down arrow` realiza a mesma ação que `Enter`.
- Utilize `Shift+Plus` para ampliar e `Shift+Minus` para reduzir.

## <a name="how-can-i-navigate-by-using-the-keyboard-in-the-virtual-network-topology-view"></a>Como posso navegar usando o teclado na exibição de topologia de rede virtual?

A página de topologia de redes virtuais contém duas seções principais:
    
- **Banner**: o banner na parte superior da topologia de redes virtuais fornece botões para selecionar filtros de distribuição de tráfego (por exemplo, redes virtuais conectadas, redes virtuais desconectadas e IPs públicos). Quando você seleciona um botão, o respectivo filtro é aplicado na topologia. Por exemplo, se você selecionar o botão Ativo, a topologia destacará as redes virtuais ativas em sua implantação.
- **Topologia**: abaixo do banner, a seção de topologia mostra a distribuição de tráfego entre as redes virtuais.
    
### <a name="keyboard-navigation-on-the-banner"></a>Navegação do teclado no banner
    
- Por padrão, a seleção na página de topologia de redes virtuais para o banner é o filtro “Conectado VNets”.
- Para ir para outro filtro, use a tecla `Tab` para avançar. Para retroceder, use a tecla `Shift+Tab`. A navegação para frente é da esquerda para a direita, seguida de cima para baixo.
- Pressione `Enter` para aplicar o filtro selecionado. Com base na seleção e na implantação do filtro, um ou vários nós (rede virtual) na seção de topologia são realçados.
- Para alternar entre o banner e a topologia, pressione `Ctrl+F6`.
        
### <a name="keyboard-navigation-on-the-topology"></a>Navegação do teclado na topologia
    
- Depois de selecionar qualquer filtro no banner e pressionar `Ctrl+F6`, o foco é movido para um dos nós destacados (**VNet**) na visualização de topologia.
- Para mover para outros nós destacados na visualização de topologia, use a tecla `Shift+Right arrow` para o movimento de avanço. 
- Em nós realçados, o foco é movido para a **Caixa de Ferramentas de Informações** para o nó. Por padrão, o foco é movido para o botão **Mais detalhes** na **Caixa de ferramentas de informações**. Para mover-se ainda mais dentro da visão **Box**, use as teclas `Right arrow` e `Left arrow` para mover para frente e para trás, respectivamente. Pressionar `Enter` tem o mesmo efeito que selecionar o botão com foco na **Caixa de Ferramentas de Informações**.
- Na seleção de qualquer desses nós, você pode visitar todas as suas conexões, uma a uma, pressionando a tecla `Shift+Left arrow`. O foco se move para a **Caixa de ferramentas de informações** dessa conexão. A qualquer momento, o foco pode ser deslocado de volta para o nó pressionando `Shift+Right arrow` novamente.
    

## <a name="how-can-i-navigate-by-using-the-keyboard-in-the-subnet-topology-view"></a>Como posso navegar usando o teclado na exibição de topologia de sub-rede?

A página de topologia de sub-redes virtuais contém duas seções principais:
    
- **Banner**: o banner na parte superior da topologia de sub-redes virtuais fornece botões para selecionar filtros de distribuição de tráfego (por exemplo, sub-redes Ativas, Médias e de Gateway). Quando você seleciona um botão, o respectivo filtro é aplicado na topologia. Por exemplo, se você selecionar o botão Ativo, a topologia destacará a sub-rede virtual ativa em sua implantação.
- **Topologia**: abaixo do banner, a seção de topologia mostra a distribuição de tráfego entre as sub-redes virtuais.
    
### <a name="keyboard-navigation-on-the-banner"></a>Navegação do teclado no banner
    
- Por padrão, a seleção na página de topologia das sub-redes virtuais para o banner é o filtro "Sub-redes".
- Para ir para outro filtro, use a tecla `Tab` para avançar. Para retroceder, use a tecla `Shift+Tab`. A navegação para frente é da esquerda para a direita, seguida de cima para baixo.
- Pressione `Enter` para aplicar o filtro selecionado. Com base na seleção e na implantação de filtros, um ou vários nós (Sub-rede) na seção de topologia são realçados.
- Para alternar entre o banner e a topologia, pressione `Ctrl+F6`.
        
### <a name="keyboard-navigation-on-the-topology"></a>Navegação do teclado na topologia
    
- Depois de selecionar qualquer filtro no banner e pressionar `Ctrl+F6`, o foco se moverá para um dos nós destacados (**Sub-rede**) na visualização de topologia.
- Para mover para outros nós destacados na visualização de topologia, use a tecla `Shift+Right arrow` para o movimento de avanço. 
- Em nós realçados, o foco é movido para a **Caixa de Ferramentas de Informações** para o nó. Por padrão, o foco é movido para o botão **Mais detalhes** na **Caixa de ferramentas de informações**. Para mover-se ainda mais dentro da visão **Box**, use as teclas `Right arrow` e `Left arrow` para avançar e voltar, respectivamente. Pressionar `Enter` tem o mesmo efeito que selecionar o botão com foco na **Caixa de Ferramentas de Informações**.
- Na seleção de tais nós, você pode visitar todas as suas conexões, uma a uma, pressionando a tecla `Shift+Left arrow`. O foco se move para a **Caixa de ferramentas de informações** dessa conexão. A qualquer momento, o foco pode ser deslocado de volta para o nó pressionando `Shift+Right arrow` novamente.    

