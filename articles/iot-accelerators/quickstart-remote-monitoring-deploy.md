---
title: Experimentar uma solução de monitoramento remoto de IoT baseada em nuvem | Microsoft Docs
description: Neste início rápido, você implanta o acelerador de solução de Monitoramento Remoto do Azure IoT e faz logon para usar o painel da solução.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: quickstart
ms.custom: mvc
ms.date: 11/08/2018
ms.author: dobett
ms.openlocfilehash: 4071770a74d205570cee082d9af0c0fb7c77e203
ms.sourcegitcommit: 8899e76afb51f0d507c4f786f28eb46ada060b8d
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/16/2018
ms.locfileid: "51824771"
---
# <a name="quickstart-try-a-cloud-based-remote-monitoring-solution"></a>Início Rápido: Experimentar uma solução de monitoramento remoto baseado em nuvem

Este início rápido mostra como implantar o acelerador de solução de monitoramento remoto do Azure IoT. Depois que você implantar a solução baseada em nuvem, poderá usar a página **Painel** da solução para visualizar os dispositivos simulados em um mapa e a página **Manutenção** para responder a um alerta de pressão de um dispositivo resfriador simulado. Você pode usar esse acelerador de solução como o ponto de partida para sua própria implementação ou como uma ferramenta de aprendizado.

A implantação inicial configura o acelerador de solução para uma empresa chamada Contoso. Como operador da Contoso, você gerencia um conjunto de vários tipos de dispositivos, como resfriadores, implantados em ambientes físicos diferentes. Um dispositivo resfriador envia telemetria de temperatura, umidade e pressão para o acelerador de solução de Monitoramento Remoto.

Para concluir este início rápido, você precisará de uma assinatura do Azure ativa.

Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="deploy-the-solution"></a>Implantar a solução

Ao implantar o acelerador de solução em sua assinatura do Azure, você precisa definir algumas opções de configuração.

Entre no [azureiotsolutions.com](https://www.azureiotsolutions.com/Accelerators) usando suas credenciais de conta do Azure.

Clique no bloco **Monitoramento Remoto**. Na página **Monitoramento Remoto**, clique no bloco **Experimentar Agora**:

![Escolha Monitoramento Remoto](./media/quickstart-remote-monitoring-deploy/remotemonitoring.png)

Na página **Criar solução de Monitoramento Remoto**, selecione uma implantação **Básica**. Se você estiver implantando o acelerador de solução para saber como ele funciona ou para executar uma demonstração, escolha a opção **Básico** para minimizar os custos.

Escolha **.NET** como linguagem. As implementações de Java e .NET têm os mesmos recursos.

Insira um **Nome de solução** exclusivo para o acelerador de solução de Monitoramento Remoto. Para este início rápido, estamos chamando nosso **contoso-rm**.

Selecione a **Assinatura** e a **Região** que você deseja usar para o acelerador de solução. Normalmente a região escolhida é a mais próxima de você. Para este início rápido, usamos **Leste dos EUA**.
Escolha **Visual Studio Enterprise**, mas você deve ser [administrador global ou usuário](iot-accelerators-permissions.md) para fazer isso.

Para iniciar a implantação, clique em **Criar Solução**. Esse processo leva pelo menos cinco minutos para ser executado:

![Detalhes da solução de Monitoramento Remoto](./media/quickstart-remote-monitoring-deploy/createform.png)

## <a name="sign-in-to-the-solution"></a>Entrar na solução

Quando a implantação para sua assinatura do Azure for concluída, você verá uma marca de seleção verde e **Pronto** no bloco da solução. Agora você pode entrar em seu painel de acelerador de solução Monitoramento Remoto.

Na página **Soluções provisionadas**, clique em seu novo acelerador de solução de Monitoramento Remoto:

![Escolher a nova solução](./media/quickstart-remote-monitoring-deploy/choosenew.png)

Você pode exibir informações sobre o acelerador de solução de Monitoramento Remoto no painel exibido. Escolha **Painel da solução** para exibir o acelerador de solução de Monitoramento Remoto:

![Painel de solução](./media/quickstart-remote-monitoring-deploy/solutionpanel.png)

Clique em **Aceitar** para aceitar a solicitação de permissões. O painel de solução de Monitoramento Remoto é exibido no navegador:

[![Painel da solução](./media/quickstart-remote-monitoring-deploy/solutiondashboard-inline.png)](./media/quickstart-remote-monitoring-deploy/solutiondashboard-expanded.png#lightbox)

## <a name="view-your-devices"></a>Exibir dispositivos

O painel da solução mostra as seguintes informações sobre os dispositivos simulados da Contoso:

* O painel **Estatísticas de dispositivo** mostra informações resumidas sobre alertas e o número total de dispositivos. Na implantação padrão, a Contoso tem 10 dispositivos simulados de tipos diferentes.

* O painel **Locais do dispositivo** mostra onde os dispositivos estão localizados fisicamente. A cor do pino mostra quando há alertas do dispositivo.

* O painel **Alertas** mostra detalhes de alertas dos dispositivos.

* O painel **Telemetria** mostra a telemetria dos dispositivos. Você pode exibir fluxos de telemetria diferentes clicando nos tipos de telemetria na parte superior.

* O painel **Análise** mostra informações combinadas sobre os alertas dos dispositivos.

## <a name="respond-to-an-alert"></a>Responder a um alerta

Como um operador da Contoso, você pode monitorar os dispositivos no painel da solução. O painel **Estatísticas do dispositivo** mostra que houve um número de alertas críticos e o painel **Alertas** mostra que a maioria deles é proveniente de um dispositivo resfriador. Para dispositivos resfriadores da Contoso, uma pressão interna acima de 250 PSI indica que o dispositivo não está funcionando corretamente.

### <a name="identify-the-issue"></a>Identificar o problema

Na página **Painel**, no painel **Alertas**, você pode ver o alerta **Pressão do resfriador muito alta**. O resfriador tem um pino vermelho no mapa (você talvez precise ver a panorâmica e ampliar o mapa):

[![O painel mostra o alerta de pressão e o dispositivo no mapa](./media/quickstart-remote-monitoring-deploy/dashboardalarm-inline.png)](./media/quickstart-remote-monitoring-deploy/dashboardalarm-expanded.png#lightbox)

No painel **Alertas**, clique em **...** na coluna **Explorar** ao lado da regra **Pressão do resfriador muito alta**. Essa ação leva você para a página **Manutenção**, onde você pode exibir os detalhes da regra que disparou o alerta.

A página de manutenção **Pressão do resfriador muito alta** mostra detalhes da regra que disparou o alerta. A página também lista quando o alerta ocorreu e qual dispositivo o disparou:

[![A página Manutenção mostra a lista de alertas que foram disparados](./media/quickstart-remote-monitoring-deploy/maintenancealarmlist-inline.png)](./media/quickstart-remote-monitoring-deploy/maintenancealarmlist-expanded.png#lightbox)

Você identificou o problema que disparou o alerta e o dispositivo associado. Como operador, suas próximas etapas serão para confirmar o alerta e corrigir o problema.

### <a name="fix-the-issue"></a>Corrigir o problema

Para indicar a outros operadores que você está trabalhando no alerta, selecione-o e altere o **status de alerta** para **Confirmado**:

[![Selecionar e confirmar o alerta](./media/quickstart-remote-monitoring-deploy/maintenanceacknowledge-inline.png)](./media/quickstart-remote-monitoring-deploy/maintenanceacknowledge-expanded.png#lightbox)

O valor na coluna de status muda para **Confirmado**.

Para agir no resfriador, role para baixo até **Informações relacionadas**, selecione o dispositivo resfriador na lista **Dispositivos com alerta** e escolha **Trabalhos**:

[![Selecionar o dispositivo e agendar uma ação](./media/quickstart-remote-monitoring-deploy/maintenanceschedule-inline.png)](./media/quickstart-remote-monitoring-deploy/maintenanceschedule-expanded.png#lightbox)

No painel **Trabalhos**, escolha **Executar método** e então o método **EmergencyValveRelease**. Adicione o nome do trabalho **ChillerPressureRelease** e clique em **Aplicar**. Essas configurações criam um trabalho para você que é executado imediatamente.

Para exibir o status do trabalho, retorne à página **Manutenção** e exiba a lista de trabalhos no modo de exibição **Trabalhos**. Talvez seja necessário aguardar alguns segundos para poder ver se o trabalho foi executado para liberar a pressão da válvula no resfriador:

[![O status dos trabalhos no modo de exibição Trabalhos](./media/quickstart-remote-monitoring-deploy/maintenancerunningjob-inline.png)](./media/quickstart-remote-monitoring-deploy/maintenancerunningjob-expanded.png#lightbox)

### <a name="check-the-pressure-is-back-to-normal"></a>Verificar se a pressão voltou ao normal

Para exibir a telemetria de pressão para o resfriador, navegue até a página **Painel**, selecione **Pressão** no painel telemetria e confirme se a pressão de **chiller-02.0** voltou ao normal:

[![Pressão de volta ao normal](./media/quickstart-remote-monitoring-deploy/pressurenormal-inline.png)](./media/quickstart-remote-monitoring-deploy/pressurenormal-expanded.png#lightbox)

Para fechar o incidente, navegue até a página **Manutenção**, selecione o alerta e defina o status como **Fechado**:

[![Selecionar e fechar o alerta](./media/quickstart-remote-monitoring-deploy/maintenanceclose-inline.png)](./media/quickstart-remote-monitoring-deploy/maintenanceclose-expanded.png#lightbox)

O valor na coluna de status muda para **Fechado**.

## <a name="clean-up-resources"></a>Limpar recursos

Se você planeja passar para outros tutoriais, deixe o acelerador de solução de Monitoramento Remoto implantado.

Caso não precise mais do acelerador de solução, exclua-o da página [Soluções provisionadas](https://www.azureiotsolutions.com/Accelerators#dashboard) selecionando-o e clicando em **Excluir solução**:

![Excluir solução](media/quickstart-remote-monitoring-deploy/deletesolution.png)

## <a name="next-steps"></a>Próximas etapas

Neste início rápido, você implantou o acelerador de solução de Monitoramento Remoto e concluiu uma tarefa de monitoramento usando os dispositivos simulados na implantação padrão da Contoso.

Para saber mais sobre o acelerador de soluções usando dispositivos simulados, prossiga para o tutorial a seguir.

> [!div class="nextstepaction"]
> [Tutorial: Monitorar seus dispositivos de IoT](iot-accelerators-remote-monitoring-monitor.md)