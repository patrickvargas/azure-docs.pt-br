---
title: Faça o perfil dos serviços em nuvem do Azure com o Application Insights | Microsoft Docs
description: Habilite o Profiler do Application Insights para o Cloud Services.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.reviewer: cawa
ms.date: 08/06/2018
ms.author: mbullwin
ms.openlocfilehash: 445e607b6b0a21f840ab633b3a5a3779f49fdd98
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/26/2018
ms.locfileid: "50142640"
---
# <a name="profile-live-azure-cloud-services-with-application-insights"></a>Faça o perfil dos serviços em nuvem do Azure com o Application Insights

Você também pode implantar o Profiler do Application Insights nesses serviços:
* [Aplicativos Web do Azure](app-insights-profiler.md?toc=/azure/azure-monitor/toc.json)
* [Aplicativos do Service Fabric](app-insights-profiler-servicefabric.md?toc=/azure/azure-monitor/toc.json)
* [Máquinas virtuais](app-insights-profiler-vm.md?toc=/azure/azure-monitor/toc.json)

O Application Insights Profiler é instalado com a extensão WAD (Windows Azure Diagnostics). Você só precisa configurar o WAD para instalar o criador de perfil e enviar perfis para o recurso Application Insights.

## <a name="enable-profiler-for-your-azure-cloud-service"></a>Ativar o criador de perfil para seu Serviço de Nuvem do Azure
1. Verifique que você usar [.NET Framework 4.6.1](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed) ou posterior.  É suficiente confirmar que os arquivos *ServiceConfiguration.\*.cscfg* têm um valor `osFamily` de "5" ou posterior.
1. Adicione o [Application Insights SDK ao serviço em nuvem](app-insights-cloudservices.md?toc=/azure/azure-monitor/toc.json).
1. Acompanhe as solicitações com o Application Insights:

    Para as funções da Web do ASP.Net, o Application Insights pode rastrear as solicitações automaticamente.

    Funções de trabalho, [adicionar código para acompanhar as solicitações.](app-insights-profiler-trackrequests.md?toc=/azure/azure-monitor/toc.json)

    

1. Configure o Windows Azure extensão WAD (Diagnóstico) para habilitar o criador de perfil.



    1. Localize o arquivo [diagnostics.wadcfgx](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics) do *Diagnóstico do Azure* para sua função de aplicativo, como mostrado aqui:  

       ![Local do arquivo de configuração de diagnóstico](./media/enable-profiler-compute/cloudservice-solutionexplorer.png)  

        Se você não encontrar o arquivo, para saber como habilitar a extensão de Diagnóstico em seu projeto de Serviços de Nuvem do Azure, consulte [Configurando o diagnóstico para os Serviços de Nuvem do Azure e máquinas virtuais](https://docs.microsoft.com/azure/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines#enable-diagnostics-in-cloud-service-projects-before-deploying-them).

    1. Adicione a seguinte seção `SinksConfig` como um elemento filho do `WadCfg`:  

      ```xml
      <WadCfg>
        <DiagnosticMonitorConfiguration>...</DiagnosticMonitorConfiguration>
        <SinksConfig>
          <Sink name="MyApplicationInsightsProfiler">
            <!-- Replace with your own Application Insights instrumentation key. -->
            <ApplicationInsightsProfiler>00000000-0000-0000-0000-000000000000</ApplicationInsightsProfiler>
          </Sink>
        </SinksConfig>
      </WadCfg>
      ```

    >   **OBSERVAÇÃO:** Se o arquivo *diagnostics.wadcfgx* também contiver outro coletor do tipo `ApplicationInsights`, todas as três chaves de instrumentação a seguir deverão corresponder:  
    >  * A chave que é usada pelo seu aplicativo.  
    >  * A chave que é usada pelo coletor `ApplicationInsights`.  
    >  * A chave que é usada pelo coletor `ApplicationInsightsProfiler`.  
    >
    > Você pode encontrar o valor real da chave de instrumentação que é usado pelo coletor `ApplicationInsights` nos arquivos *ServiceConfiguration.\*.cscfg*.  
    > Após o lançamento do Visual Studio 15.5 do Azure SDK, somente as chaves de instrumentação usadas pelo aplicativo e o coletor `ApplicationInsightsProfiler` precisam corresponder um ao outro.
1. Implante seu serviço com a nova configuração de diagnóstico e o Application Insights Profiler será configurado para ser executado em seu serviço.
 
## <a name="next-steps"></a>Próximas etapas

- Gere tráfego para seu aplicativo (por exemplo, inicie um [teste de disponibilidade](https://docs.microsoft.com/azure/application-insights/app-insights-monitor-web-app-availability)). Em seguida, espere de 10 a 15 minutos para que os rastreamentos comecem a ser enviados à instância do Application Insights.
- Consulte [Rastreamentos do criador de perfil](https://docs.microsoft.com/azure/application-insights/app-insights-profiler-overview?toc=/azure/azure-monitor/toc.json) no portal do Azure.
- Obtenha ajuda para a solução de problemas do criador de perfil em [Solução de problemas do criador de perfil](app-insights-profiler-troubleshooting.md?toc=/azure/azure-monitor/toc.json).