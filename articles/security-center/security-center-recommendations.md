---
title: Gerenciando recomendações de segurança na Central de Segurança do Azure | Microsoft Docs
description: Este documento mostra como as recomendações na Central de Segurança do Azure ajudam a proteger os recursos do Azure e a cumprir as políticas de segurança.
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: 86c50c9f-eb6b-4d97-acb3-6d599c06133e
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/18/2018
ms.author: rkarlin
ms.openlocfilehash: 3e8333b521832579a942d3fffb06103ad0431acc
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/26/2018
ms.locfileid: "52316610"
---
# <a name="managing-security-recommendations-in-azure-security-center"></a>Gerenciando recomendações de segurança na Central de Segurança do Azure
Este documento mostra como usar as recomendações na Central de Segurança do Azure para ajudar a proteger os recursos do Azure.

> [!NOTE]
> Este documento apresenta o serviço usando uma implantação de exemplo.  Este documento não é um guia passo a passo.
>
>

## <a name="what-are-security-recommendations"></a>O que são recomendações de segurança?
A Central de Segurança analisa periodicamente o estado de segurança de seus recursos do Azure. Quando a Central de Segurança identifica possíveis vulnerabilidades de segurança, cria recomendações. As recomendações o orientam ao longo do processo de configuração dos controles necessários.

## <a name="implementing-security-recommendations"></a>Implementando recomendações de segurança
### <a name="set-recommendations"></a>Definir recomendações
Em [Configurando políticas de segurança na Central de Segurança do Azure](security-center-azure-policy.md), você aprenderá a:

* Configurar políticas de segurança.
* Habilitar a coleta de dados.
* Escolher quais recomendações ver como parte da política de segurança.

As recomendações de política atuais giram em torno de atualizações do sistema, regras de linha de base, programas antimalware, [grupos de segurança de rede](../virtual-network/security-overview.md) em sub-redes e em interfaces de rede, auditoria do banco de dados SQL, Transparent Data Encryption do banco de dados SQL e firewalls do aplicativo Web.  [Configurando políticas de segurança](security-center-azure-policy.md) fornece uma descrição de cada opção de recomendação.

### <a name="monitor-recommendations"></a>Monitorar as recomendações
Depois de definir uma política de segurança, a Central de Segurança analisa o estado de segurança de seus recursos para identificar possíveis vulnerabilidades. O bloco **Recomendações** na folha **Visão geral** permite saber o número total de recomendações identificadas pela Central de Segurança.

![Bloco Recomendações][1]

Para ver os detalhes de cada recomendação, selecione o **bloco Recomendações** em **Visão geral**. **Recomendações** se abre.

![Recomendações de filtro][2]

Você pode filtrar as recomendações. Para filtrar as recomendações, selecione **Filtro** na folha **Recomendações**. A folha **Filtro** é aberta e você seleciona os valores de gravidade e de estado que deseja ver.

As recomendações são mostradas em um formato de tabela, em que cada linha representa uma recomendação específica. As colunas da tabela são:

* **DESCRIÇÃO**: explica a recomendação e o que precisa ser feito para resolvê-la.
* **RECURSO**: lista os recursos aos quais essa recomendação se aplica.
* **ESTADO**: descreve o estado atual da recomendação:
  * **Aberta**: a recomendação ainda não foi resolvida.
  * **Em Andamento**: a recomendação está sendo aplicada atualmente aos recursos, e não é necessário que você realize nenhuma ação.
  * **Resolvida**: a recomendação já foi concluída (nesse caso, a linha fica esmaecida).
* **GRAVIDADE**: descreve a gravidade dessa recomendação específica:
  * **Alta**: existe uma vulnerabilidade em um recurso significativo (como um aplicativo, uma VM ou um grupo de segurança de rede) e ela requer atenção.
  * **Média**: existe uma vulnerabilidade, e etapas não críticas ou adicionais são necessárias para eliminá-la ou para concluir um processo.
  * **Baixa**: existe uma vulnerabilidade que deve ser resolvida, mas não exige atenção imediata. (Por padrão, não são apresentadas recomendações baixas, mas você pode filtrar as recomendações baixas caso deseje vê-las.)

Use a tabela a seguir como referência para ajudar a entender as recomendações disponíveis e o que cada uma delas fará se você as aplicar.

> [!NOTE]
> Você deve compreender os [modelos de implantação clássica e do Gerenciador de Recursos](../azure-classic-rm.md) para recursos do Azure.
>
>

| Recomendações | DESCRIÇÃO |
| --- | --- |
| [Habilitar coleta de dados para assinaturas](security-center-enable-data-collection.md) |Recomenda que você ative a coleta de dados na política de segurança para cada uma de suas assinaturas e todas as VMs (máquinas virtuais) do Azure e computadores não Azure. |
| [Corrigir as configurações de segurança](security-center-remediate-os-vulnerabilities.md) |Recomenda que você alinhe as configurações do sistema operacional com as regras de configuração de segurança recomendadas, por exemplo, não permitir o armazenamento de senhas. |
| [Aplicar atualizações do sistema](security-center-apply-system-updates.md) |Recomenda que você implante a atualizações críticas e de segurança do sistema ausentes para computadores e VMs Linux e do Windows. |
| [Aplicar um controle de acesso à rede Just-In-Time](security-center-just-in-time.md) | Recomenda que você aplique acesso à VM just in time. O recurso Just-In-Time está disponível na camada Standard da Central de Segurança. Confira os [Preços](security-center-pricing.md) para saber mais sobre os tipos de preço da Central de Segurança. |
| [Reinicializar após as atualizações do sistema](security-center-apply-system-updates.md#reboot-after-system-updates) |Recomenda que você reinicialize uma VM para concluir o processo de aplicação de atualizações de sistema. |
| [Adicione um firewall do aplicativo Web](security-center-add-web-application-firewall.md) |Recomenda que você implante um WAF (firewall do aplicativo Web) para pontos de extremidade da Web. Uma recomendação WAF é mostrada para qualquer IP voltado para uso público (IP de nível de instância ou IP de balanceamento de carga) que tem um grupo de segurança de rede associado com portas de entrada da Web abertas (80,443). </br>A Central de Segurança recomenda que você provisione um WAF para ajudar a proteger contra ataques direcionados a seus aplicativos Web em máquinas virtuais e no Ambiente do Serviço de Aplicativo. Um ASE (Ambiente do Serviço de Aplicativo) é uma opção do plano de serviço [Premium](https://azure.microsoft.com/pricing/details/app-service/) do Serviço de Aplicativo do Azure que fornece um ambiente totalmente isolado e dedicado a executar com segurança os aplicativos do Serviço de Aplicativo do Azure. Para saber mais sobre o ASE, consulte a [Documentação do Ambiente do Serviço de Aplicativo](../app-service/environment/intro.md).</br>Você pode proteger vários aplicativos Web na Central de segurança adicionando-os às suas implantações do WAF existentes. |
| [Finalizar a proteção do aplicativo](security-center-add-web-application-firewall.md#finalize-application-protection) |Para concluir a configuração de um WAF, o tráfego deve ser roteado para o dispositivo do WAF. Se essa recomendação for seguida, as alterações de configuração necessárias serão concluídas. |
| [Adicionar um Firewall de Última Geração](security-center-add-next-generation-firewall.md) |Recomenda que você adicione um NGFW (Firewall de Última Geração) de um parceiro da Microsoft para aumentar suas proteções de segurança. |
| [Rotear o tráfego apenas através do NGFW](security-center-add-next-generation-firewall.md#route-traffic-through-ngfw-only) |Recomenda que você configure regras para o grupo de segurança de rede (NSG) que forcem o tráfego de entrada em sua VM a passar pelo NGFW. |
| [Instalar proteção do ponto de extremidade](security-center-install-endpoint-protection.md) |Recomenda que você provisione programas antimalware para máquinas virtuais (somente VMs do Windows). |
| [Habilitar Grupos de Segurança de Rede em sub-redes ou máquinas virtuais](security-center-enable-network-security-groups.md) |Recomenda que você habilite NSGs em sub-redes ou VMs. |
| [Restringir o acesso por meio de ponto de extremidade para a Internet](security-center-restrict-access-through-internet-facing-endpoints.md) |Recomenda que você configure regras de tráfego de entrada para NSGs. |
| [Habilitar a detecção de ameaças e auditoria em servidores SQL](security-center-enable-auditing-on-sql-servers.md) |Recomenda que você ative a detecção de ameaças e a auditoria para servidores SQL do Azure. (Apenas o serviço do SQL Azure. Não inclui o SQL em execução nas suas máquinas virtuais.) |
| [Habilitar a detecção de ameaças e auditoria em bancos de dados SQL](security-center-enable-auditing-on-sql-databases.md) |Recomenda que você ative a detecção de ameaças e a auditoria para Bancos de Dados SQL do Azure. (Apenas o serviço do SQL Azure. Não inclui o SQL em execução nas suas máquinas virtuais.) |
| [Habilitar Transparent Data Encryption em bancos de dados SQL](security-center-enable-transparent-data-encryption.md) |Recomenda que você habilite a criptografia para bancos de dados SQL. (Apenas o serviço do Azure SQL.) |
| [Habilitar o Agente de VM](security-center-enable-vm-agent.md) |Permite que você veja quais máquinas virtuais exigem o Agente de VM. O Agente de VM deve ser instalado em VMs para provisionar verificação de linha de base, verificação de patch e programas antimalware. O agente de VM está instalado por padrão nas VMs implantadas do Azure Marketplace. O artigo [Agente de VM e extensões – parte 2](https://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/) fornece informações sobre como instalar o Agente de VM. |
| [Aplicar a criptografia de disco](security-center-apply-disk-encryption.md) |Recomenda de que você criptografe os discos de VM usando o Azure Disk Encryption (VMs do Windows e do Linux). A criptografia é recomendada para volumes de dados e do sistema operacional em sua VM. |
| [Fornecer detalhes de contato de segurança](security-center-provide-security-contact-details.md) |Recomenda que você forneça informações de contato de segurança para cada uma das suas assinaturas. Informações de contato são um número de telefone e um endereço de email. As informações serão usadas para contatá-lo se nossa equipe de segurança acreditar que os recursos estão comprometidos. |
| [Atualizar a versão do sistema operacional](security-center-update-os-version.md) |Recomenda que você atualize a versão do SO (sistema operacional) de seu Serviço de Nuvem para a versão mais recente disponível para a família do SO.  Para saber mais sobre os Serviços de Nuvem, confira a [Visão geral dos serviços de nuvem](../cloud-services/cloud-services-choose-me.md). |
| [Avaliação de vulnerabilidade não instalada](security-center-vulnerability-assessment-recommendations.md) |Recomenda que você instale uma solução de avaliação de vulnerabilidade na VM. |
| [Corrigir vulnerabilidades](security-center-vulnerability-assessment-recommendations.md#review-the-recommendation) |Permite que você veja as vulnerabilidades do sistema e dos aplicativos detectadas pela solução de avaliação de vulnerabilidade instalada na VM. |
| [Habilitar a criptografia para a Conta de Armazenamento do Azure](security-center-enable-encryption-for-storage-account.md) | Recomenda a habilitação da Criptografia do Serviço de Armazenamento do Azure para dados em repouso. A SSE (Criptografia do Serviço de Armazenamento) funciona criptografando os dados quando eles são gravados no armazenamento do Azure e descriptografando-os antes da recuperação. Atualmente, a SSE está disponível somente para o serviço Blob do Azure e pode ser usada para blobs de blocos, blobs de páginas e blobs de acréscimo. Para saber mais, confira [Criptografia do Serviço de Armazenamento para dados em repouso](../storage/common/storage-service-encryption.md).</br>A SSE tem suporte apenas nas contas de armazenamento do Resource Manager. |
| [Habilitar controles de aplicativos adaptáveis](security-center-adaptive-application.md) | Recomenda-se de que você aplique controles de aplicativo adaptáveis em suas VMs do Windows. Esse recurso está disponível na camada Standard da Central de Segurança. Confira os [Preços](security-center-pricing.md) para saber mais sobre os tipos de preço da Central de Segurança. |
| O Serviço de Aplicativo deve ser acessível apenas por HTTPS | É recomendável que você limite o acesso do Serviço de Aplicativo apenas por HTTPS. |
| Os soquetes da Web devem ser desabilitados para o aplicativo Web| É recomendável que você examine cuidadosamente o uso de soquetes da Web em aplicativos Web.  O protocolo de soquetes da Web é vulnerável a diferentes tipos de ameaças de segurança. |
| Usar domínios personalizados para seu aplicativo Web | É recomendável o uso de domínios personalizados para proteger um aplicativo Web de ataques comuns como phishing e outros ataques relacionados a DNS. |
| Configurar restrições de IP para o aplicativo Web | É recomendável que você defina uma lista de endereços IP que tem autorização para acessar seu aplicativo.  O uso de restrições de IP protege um aplicativo Web de ataques comuns. |
| Não permita que todos os recursos ('*') acessem o aplicativo | É recomendável que você não defina o parâmetro WEBSITE_LOAD_CERTIFICATES como ‘*’. Definir o parâmetro como '*' significa que todos os certificados serão carregados para seu repositório de certificados pessoal de aplicativos Web.  Isso pode levar a abuso do princípio de privilégio mínimo porque é improvável que o site precise de acesso a todos os certificados em tempo de execução. |
| O CORS não deve permitir o acesso a todos os recursos ao seu aplicativo | É recomendável que você permita que apenas os domínios necessários interajam com seu aplicativo Web. O CORS (compartilhamento de recurso de origem cruzada) não deve permitir que todos os domínios acessem seu aplicativo Web. |
| Use o .NET Framework compatível mais recente para o aplicativo Web | É recomendável que você use a versão mais recente do .NET Framework para as classes de segurança mais recentes. Usar classes e tipos mais antigos pode tornar seu aplicativo vulnerável. |
| Use a versão mais recente compatível do Java para o aplicativo Web | É recomendável que você use a versão mais recente do Java para as classes de segurança mais recentes. Usar classes e tipos mais antigos pode tornar seu aplicativo vulnerável. |
| Use a versão mais recente compatível do PHP para o aplicativo Web | É recomendável que você use a versão mais recente do PHP para as classes de segurança mais recentes. Usar classes e tipos mais antigos pode tornar seu aplicativo vulnerável. |
| [Adicione um firewall do aplicativo Web](security-center-add-web-application-firewall.md) |Recomenda que você implante um WAF (firewall do aplicativo Web) para pontos de extremidade da Web. Uma recomendação WAF é mostrada para qualquer IP voltado para uso público (IP de nível de instância ou IP de balanceamento de carga) que tem um grupo de segurança de rede associado com portas de entrada da Web abertas (80,443).</br></br>A Central de Segurança recomenda que você provisione um WAF para ajudar a proteger contra ataques direcionados a seus aplicativos Web em máquinas virtuais e no Ambiente do Serviço de Aplicativo. Um ASE (Ambiente do Serviço de Aplicativo) é uma opção do plano de serviço [Premium](https://azure.microsoft.com/pricing/details/app-service/) do Serviço de Aplicativo do Azure que fornece um ambiente totalmente isolado e dedicado a executar com segurança os aplicativos do Serviço de Aplicativo do Azure. Para saber mais sobre o ASE, consulte a [Documentação do Ambiente do Serviço de Aplicativo](../app-service/environment/intro.md).</br></br>Você pode proteger vários aplicativos Web na Central de segurança adicionando-os às suas implantações do WAF existentes. |
| [Finalizar a proteção do aplicativo](security-center-add-web-application-firewall.md#finalize-application-protection) |Para concluir a configuração de um WAF, o tráfego deve ser roteado para o dispositivo do WAF. Se essa recomendação for seguida, as alterações de configuração necessárias serão concluídas. |
| Use a versão mais recente compatível do Node.js para o aplicativo Web | É recomendável que você use a versão mais recente do Node.js para as classes de segurança mais recentes. Usar classes e tipos mais antigos pode tornar seu aplicativo vulnerável. |
| O CORS não deve permitir o acesso a todos os recursos ao seu aplicativo de funções | É recomendável que você permita que apenas os domínios necessários interajam com seu aplicativo Web. O CORS (compartilhamento de recurso de origem cruzada) não deve permitir que todos os domínios acessem seu aplicativo de funções. |
| Use domínios personalizados para o aplicativo de funções | É recomendável o uso de domínios personalizados para proteger um aplicativo de funções de ataques comuns como phishing e outros ataques relacionados a DNS. |
| Configure restrições de IP para o aplicativo de funções | É recomendável que você defina uma lista de endereços IP que tem autorização para acessar seu aplicativo. O uso de restrições de IP protege um aplicativo de funções de ataques comuns. |
| O aplicativo de funções deve ser acessível apenas por HTTPS | É recomendável que você limite o acesso de aplicativos de função apenas por HTTPS. |
| A depuração remota deve ser desativada para o aplicativo de funções | É recomendável que você desative a depuração para o aplicativo de funções caso não precise mais usá-la. A depuração remota exige que as portas de entrada estejam abertas em um aplicativo de funções. |
| Os soquetes da Web devem ser desabilitados para o aplicativo de funções | É recomendável que você examine cuidadosamente o uso de soquetes da Web em aplicativos de funções. O protocolo de soquetes da Web é vulnerável a diferentes tipos de ameaças de segurança. |
| Designe mais de um proprietário na sua assinatura | É recomendável designar mais de um proprietário da assinatura para poder ter redundância de acesso de administrador. |
| Designe até três proprietários em sua assinatura | É recomendável designar menos de três proprietários de assinaturas para reduzir o potencial de violação por um proprietário comprometido. |
| Habilite a MFA para contas com permissões de proprietário em sua assinatura | É recomendável que você habilite a MFA (Autenticação Multifator) para todas as contas de assinatura com privilégios de administrador para evitar uma violação de conta ou recursos. |
| Habilite a MFA para contas com permissões de gravação em sua assinatura | É recomendável que você habilite a MFA (Autenticação Multifator) para todas as contas de assinatura com privilégios de gravação para evitar uma violação de conta ou recursos. |
| Habilite a MFA para contas com permissões de leitura em sua assinatura | É recomendável que você habilite a MFA (Autenticação Multifator) para todas as contas de assinatura com privilégios de leitura para evitar uma violação de conta ou recursos. |
| Remova contas externas com permissões de leitura da sua assinatura | É recomendável que você remova contas externas com privilégios de leitura da sua assinatura para impedir o acesso não monitorado. |
| Remova contas externas com permissões de gravação da sua assinatura | É recomendável que você remova contas externas com privilégios de gravação da sua assinatura para impedir o acesso não monitorado. |
| Remova contas externas com permissões de proprietário da sua assinatura | É recomendável que você remova contas externas com permissões de proprietário da sua assinatura para impedir o acesso não monitorado. |
| Remova contas preteridas da assinatura | É recomendável remover contas preteridas das suas assinaturas. |
| Remova contas preteridas com permissões de proprietário da assinatura | É recomendável que você remova as contas preteridas com permissões de proprietário das suas assinaturas. |

### <a name="apply-recommendations"></a>Aplicar recomendações
Depois de examinar todas as recomendações, decida qual delas aplicar primeiro. É recomendável usar a classificação de gravidade como o parâmetro principal para avaliar quais recomendações devem ser aplicadas primeiro.

Na tabela de recomendações acima, selecione uma recomendação e execute suas etapas como um exemplo de como aplicar uma recomendação.

## <a name="next-steps"></a>Próximas etapas
Neste documento, você foi apresentado às recomendações de segurança da Central de Segurança. Para saber mais sobre a Central de Segurança, confira o seguinte:

* [Configurando políticas de segurança na Central de Segurança do Azure](security-center-azure-policy.md) : saiba como configurar políticas de segurança para suas assinaturas e grupos de recursos do Azure.
* [Monitoramento da integridade de segurança na Central de Segurança do Azure](security-center-monitoring.md) : saiba como monitorar a integridade dos recursos do Azure.
* [Gerenciando e respondendo aos alertas de segurança na Central de Segurança do Azure](security-center-managing-and-responding-alerts.md) : aprenda a gerenciar e responder aos alertas de segurança.
* [Monitorando as soluções de parceiros com a Central de Segurança do Azure](security-center-partner-solutions.md) : saiba como monitorar o status de integridade de suas soluções de parceiros.
* [Perguntas frequentes da Central de Segurança do Azure](security-center-faq.md) : encontre as perguntas frequentes sobre como usar o serviço.
* [Blog de Segurança do Azure](https://blogs.msdn.com/b/azuresecurity/) : encontre postagens no blog sobre conformidade e segurança do Azure.

<!--Image references-->
[1]: ./media/security-center-recommendations/recommendations-tile.png
[2]: ./media/security-center-recommendations/filter-recommendations.png
