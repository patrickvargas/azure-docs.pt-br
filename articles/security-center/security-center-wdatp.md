---
title: Proteção Avançada contra Ameaças do Windows Defender com a Central de Segurança do Azure
description: Este documento apresenta a integração entre a Central de Segurança do Azure e a Proteção Avançada contra Ameaças do Windows Defender.
services: security-center
documentationcenter: na
author: barclayn
manager: MBaldwin
editor: ''
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/20/2018
ms.author: barclayn
ms.openlocfilehash: e7bb7f0c4cb63390b91012e071589611991a2866
ms.sourcegitcommit: 7b0778a1488e8fd70ee57e55bde783a69521c912
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/10/2018
ms.locfileid: "49068311"
---
# <a name="windows-defender-advanced-threat-protection-with-azure-security-center"></a>Proteção Avançada contra Ameaças do Windows Defender com a Central de Segurança do Azure

A Central de Segurança do Azure está estendendo sua oferta de plataformas de proteção de cargas de trabalho de nuvem, integrando-se com a ATP ([Proteção Avançada contra Ameaças do Windows Defender](https://www.microsoft.com/en-us/WindowsForBusiness/windows-atp)).
Essa alteração traz recursos abrangentes de EDR (detecção e resposta de ponto de extremidade). Com a integração ao Windows Defender ATP, você pode detectar anormalidades. Você também pode detectar e responder a ataques avançados em pontos de extremidade de servidor monitorados pela Central de Segurança do Azure.

Os clientes da Central de Segurança do Azure agora podem usar recursos do Windows Defender ATP:

- **Sensores de detecção de violação de postagem de última geração**: os sensores do Windows Defender ATP para servidores Windows coletam uma grande variedade de sinais comportamentais.

- **Detecção de violação de postagem com base em análise e com o poder da nuvem**: o Windows Defender ATP adapta-se rapidamente a ameaças que mudam constantemente. Ele usa análise avançada e Big Data. O Windows Defender ATP é amplificado pela potência do Gráfico de Segurança Inteligente com sinais pelo Windows, o Azure e o Office para detectar ameaças desconhecidas. Ele fornece alertas acionáveis e permite que você responda rapidamente.

- **Inteligência contra ameaças**: o Windows Defender ATP identifica ferramentas, técnicas e procedimentos de invasores. Quando os detecta, ele gera alertas. Ele usa dados gerados pelos caçadores de ameaças da Microsoft e pelas equipes de segurança, incrementados por inteligência oferecida por parceiros.

Essas funcionalidades agora estão disponíveis na Central de Segurança do Azure:

- **Integração automatizada**: o sensor do Windows Defender ATP é habilitado automaticamente para os servidores Windows que são integrados à Central de Segurança do Azure.

- **Painel de controle único**: o console da Central de Segurança do Azure mostra os alertas do Windows Defender ATP.

- **Investigação de computador detalhada**: os clientes da Central de Segurança do Azure podem acessar o console do Windows Defender ATP para realizar uma investigação detalhada a fim de revelar o escopo de uma violação.

![Central de Segurança do Azure, exibindo uma lista de alertas e informações gerais sobre cada alerta](media/security-center-wdatp/image1.png)

Você pode [investigar o alerta](security-center-investigation.md) na Central de Segurança do Azure:

![O painel de investigação de alertas na Central de Segurança do Azure](media/security-center-wdatp/image2.png)

Você pode investigar o alerta repassando-o ao Windows Defender ATP. Lá você encontrará informações adicionais, como a árvore de processos do alerta e o gráfico de incidentes. Você também pode ver uma linha do tempo detalhada do computador, mostrando cada comportamento por um período de histórico de até seis meses.

![Página do Windows Defender ATP com informações detalhadas sobre um alerta](media/security-center-wdatp/image3.png)

## <a name="platform-support"></a>Suporte à plataforma

Esse recurso dá suporte à detecção no Windows Server 2012 R2 e no Windows Server 2016.

Apenas os servidores em assinaturas para a camada de serviço Standard são compatíveis.

## <a name="onboarding-instructions"></a>Instruções de integração

Para exibir se a integração ao Windows Defender ATP está habilitada, selecione **Central de Segurança** > **política de segurança** > **Assinatura** > **Editar configurações**.

  ![Gerenciamento de Política da Central de Segurança do Azure](media/security-center-wdatp/policy-management.png)

Aqui você pode ver as integrações habilitadas no momento.

  ![Página de configurações de detecção de ameaças da Central de Segurança do Azure com a integração ao Windows Defender ATP habilitada](media/security-center-wdatp/enable-integrations.png)

- Se você já integrou os servidores à camada Standard da Central de Segurança do Azure, nenhuma ação adicional é necessária. A Central de Segurança do Azure integrará automaticamente os servidores ao Windows Defender ATP. Esse processo pode levar até 24 horas.

- Se você nunca integrou os servidores à camada Standard da Central de Segurança do Azure, integre-os à Central de Segurança do Azure como de costume.

- Se você integrou os servidores por meio do Windows Defender ATP:
  - Veja a documentação para obter diretrizes sobre [como remover computadores de servidor](https://go.microsoft.com/fwlink/p/?linkid=852906).
  - Integre esses servidores à Central de Segurança do Azure.

## <a name="access-to-the-windows-defender-atp-portal"></a>Acesso ao portal do Windows Defender ATP

Siga as instruções em [Atribuir acesso de usuário ao portal](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-atp/assign-portal-access-windows-defender-advanced-threat-protection).

## <a name="set-the-firewall-configuration"></a>Definir a configuração do firewall

Quando um sensor do Windows Defender ATP estiver se conectando do contexto do sistema, caso você tenha um proxy ou firewall bloqueando o tráfego anônimo, verifique se o tráfego anônimo é permitido. Siga as instruções em [Permitir o acesso a URLs do serviço Windows Defender ATP no servidor proxy](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-atp/configure-proxy-internet-windows-defender-advanced-threat-protection#enable-access-to-windows-defender-atp-service-urls-in-the-proxy-server).

## <a name="test-the-feature"></a>Testar o recurso

Para gerar um alerta de teste benigno do Windows Defender ATP:

1. Use a Área de Trabalho Remota para acessar uma VM do Windows Server 2012 R2 ou uma VM do Windows Server 2016.  Abra uma janela de Prompt de Comando.

2. No prompt, copie e execute o comando a seguir. A janela do prompt de comando será fechada automaticamente.

    ```
    powershell.exe -NoExit -ExecutionPolicy Bypass -WindowStyle Hidden (New-Object System.Net.WebClient).DownloadFile('http://127.0.0.1/1.exe', 'C:\\test-WDATP-test\\invoice.exe'); Start-Process 'C:\\test-WDATP-test\\invoice.exe'
    ```

   ![Uma janela do prompt de comando com o comando acima](media/security-center-wdatp/image4.jpeg)

3. Se o comando for bem-sucedido, você verá um novo alerta no painel da Central de Segurança do Azure e no portal do Windows Defender ATP. Esse alerta pode levar alguns minutos para aparecer.

4. Para examinar o alerta na Central de Segurança, acesse **Alertas de segurança** >  **Linha de Comando do PowerShell Suspeita**.

5. Na janela de investigação, selecione o link para ir para o portal do Windows Defender ATP.

## <a name="next-steps"></a>Próximas etapas

- [Como configurar políticas de segurança na Central de Segurança do Azure](security-center-policies.md): saiba como configurar políticas de segurança para suas assinaturas e grupos de recursos do Azure.
- [Gerenciar as recomendações de segurança na Central de Segurança do Azure](security-center-recommendations.md): saiba como as recomendações ajudam a proteger os recursos do Azure.
- [Monitoramento da integridade de segurança na Central de Segurança do Azure](security-center-monitoring.md): saiba como monitorar a integridade dos recursos do Azure.
