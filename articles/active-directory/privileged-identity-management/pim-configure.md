---
title: O que é o Azure AD Privileged Identity Management? | Microsoft Docs
description: Fornece uma visão geral do PIM (Privileged Identity Management) do Azure Active Directory.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: pim
ms.topic: overview
ms.date: 03/07/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: af93ade2a7031aeda5b4108649c59a8d6c1393ce
ms.sourcegitcommit: 06724c499837ba342c81f4d349ec0ce4f2dfd6d6
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/19/2018
ms.locfileid: "46465853"
---
# <a name="what-is-azure-ad-privileged-identity-management"></a>O que é o Azure AD Privileged Identity Management?

Com o Azure AD (Azure Active Directory) Privileged Identity Management, você pode gerenciar, controlar e monitorar o acesso em sua organização. Isso inclui o acesso a recursos no Azure AD, recursos do Azure e outros Microsoft Online Services, como o Office 365 ou o Microsoft Intune.

> [!NOTE]
> Quando você habilita o Privileged Identity Management para seu locatário, uma licença válida, paga ou de avaliação, do Azure AD Premium P2 ou do Enterprise Mobility + Security E5 é exigida para cada usuário que interage com ou recebe um benefício do serviço. Entre os exemplos estão usuários/usuários em um grupo que:
>
>- Recebem a função Administrador de função com privilégios 
>- São atribuídos como elegíveis a outras funções de diretório, gerenciáveis por meio de PIM 
>- Capaz de aprovar/rejeitar solicitações de PIM 
>- São atribuídos a uma função de recursos do Azure com atribuições Just in time ou Direct (com base na hora)  
>- São atribuídos a uma análise de acesso
>
>Para obter mais informações, consulte [Edições do Active Directory do Azure](../fundamentals/active-directory-whatis.md).

As empresas desejam minimizar o número de pessoas que têm acesso a informações seguras ou recursos, porque isso reduz a chance de um usuário mal-intencionado obter esse tipo de acesso, ou um usuário autorizado afetar acidentalmente um recurso confidencial.  No entanto, os usuários ainda precisam executar operações privilegiadas em aplicativos do Azure AD, Azure, Office 365 ou SaaS. As organizações podem proporcionar aos usuários acesso privilegiado aos recursos do Azure como Assinaturas e Azure AD. Não há necessidade de supervisão da atividade do usuário com seus privilégios de administrador. O Azure AD Privileged Identity Management ajuda a reduzir o risco de direitos de acesso excessivo, desnecessários ou mal utilizados.

O Azure AD Privileged Identity Management ajuda sua organização a:

- Ver quais usuários receberam funções com privilégios para gerenciar recursos do Azure, bem como quais usuários receberam funções administrativas no Azure AD
- Habilitar acesso administrativo "just-in-time" sob demanda para Microsoft Online Services, como Office 365 e Intune, e recursos do Azure de assinaturas, grupos de recursos e recursos individuais, como Máquinas Virtuais 
- Ver um histórico de ativações do administrador, incluindo as mudanças feitas por administradores em recursos do Azure
- Receber alertas sobre alterações em atribuições do administrador
- Exigir aprovação para ativar as funções de administrador com privilégios do Azure AD
- Examinar a associação de funções administrativas e exigir que os usuários forneçam uma justificativa para a continuação da associação

No Azure AD, o Azure AD Privileged Identity Management pode gerenciar os usuários atribuídos às funções organizacionais internas do Azure AD, como Administrador Global. No Azure, o Azure AD Privileged Identity Management pode gerenciar usuários e grupos atribuídos por meio de funções de RBAC do Azure, incluindo Proprietário ou Colaborador.

## <a name="just-in-time-administrator-access"></a>Administrador de acesso just in time

Historicamente, você podia atribuir a um usuário uma função de administrador por meio do Portal do Azure, outros portais do Microsoft Online Services ou cmdlets do Azure AD no Windows PowerShell. Como resultado, esse usuário se torna **administrador permanente**, sempre ativo na função a ele atribuída. O Azure AD Privileged Identity Management introduz o conceito de um **administrador elegíveis**. Administradores elegíveis devem ser usuários que precisam de acesso privilegiado às vezes, mas não o dia todo, todos os dias. A função fica inativa até que o usuário precise de acesso, então ele conclui um processo de ativação e torna-se um administrador ativo por um tempo predeterminado. Cada vez mais empresas estão escolhendo usar essa abordagem para reduzir ou eliminar "acesso de administrador permanente" a funções privilegiadas.


## <a name="terminology"></a>Terminologia

*Usuário de função qualificada*: um usuário de função qualificada é um usuário em sua organização que foi atribuído a uma função do Azure AD como qualificado (a função requer ativação).

*Aprovador delegado*: um aprovador delegado é um ou vários indivíduos ou grupos no Azure AD responsáveis por aprovar solicitações para ativar funções.

## <a name="scenarios"></a>Cenários

O PIM dá suporte aos seguintes cenários:

**Como PRA (Administrador da Função com Privilégios), você pode:**

- Habilitar a aprovação para funções específicas
- Especificar usuários e/ou grupos aprovadores para aprovar solicitações
- Exibir o histórico de solicitações e aprovações de todas as funções com privilégios

**Como um aprovador designado, você pode:**

- Exibir as aprovações pendentes (solicitações)
- Aprovar ou rejeitar solicitações de elevação de função (única e/ou em massa)
- Fornecer uma justificativa para minha aprovação/rejeição 

**Como usuário de função qualificada, você pode:**

- Solicitar a ativação de uma função que exige aprovação
- Exibir o status de sua solicitação a ser ativada
- Concluir a tarefa no Azure AD caso a ativação tenha sido aprovada

## <a name="enable-privileged-identity-management-for-your-directory"></a>Habilitar o Privileged Identity Management para seu diretório

Você pode começar a usar o Azure AD Privileged Identity Management acessando o [Portal do Azure](https://portal.azure.com/).

> [!NOTE]
> Você deve ser um administrador global com uma conta organizacional (por exemplo, @yourdomain.com) e não uma conta da Microsoft (por exemplo, @outlook.com) para habilitar o Azure AD Privileged Identity Management para um diretório.

1. Entre no [portal do Azure](https://portal.azure.com/) como um administrador global do seu diretório.
2. Se sua organização tiver mais de um diretório, selecione seu nome de usuário no canto superior direito do portal do Azure. Selecione o diretório em que você usará o Privileged Identity Management do Azure AD.
3. Selecione **Todos os serviços** e use a caixa de texto Filtrar para pesquisar o **Azure AD Privileged Identity Management**.
4. Marque **Fixar no painel** e então clique em **Criar**. O aplicativo Privileged Identity Management é aberto.

Se você for a primeira pessoa a usar o Azure AD Privileged Identity Management em seu diretório e navegar até as funções do diretório do Azure AD, um [assistente de segurança](pim-security-wizard.md) o guiará pela experiência de atribuição inicial. Depois disso, você se tornará automaticamente o primeiro **Administrador de segurança** e o **Administrador com privilégios de função** do diretório.

Para funções do Azure AD, apenas um usuário que está na função de Administrador de Função Privilegiada pode gerenciar atribuições para outros administradores no Azure AD PIM. Você pode [conceder a outros usuários a capacidade de gerenciar funções de diretório no PIM](pim-how-to-give-access-to-pim.md). Os Administradores Globais, Administradores de Segurança e Leitores de Segurança podem exibir atribuições às funções do Azure AD no Azure AD PIM.
Para funções de RBAC do Azure, somente um administrador de assinatura, um proprietário de recurso ou um administrador de acesso de usuário de recursos pode gerenciar atribuições para outros administradores no Azure AD PIM.  Por padrão, o usuários que são Administradores de Funções com Privilégio, Administradores de Segurança ou Leitores de Segurança não têm acesso para exibir as atribuições a funções de RBAC do Azure no Azure AD PIM.

## <a name="privileged-identity-management-overview-entry-point"></a>Visão geral do Privileged Identity Management (ponto de entrada)

O Azure AD Privileged Identity Management dá suporte à administração de funções de diretório do Azure AD e funções para recursos do Azure. A atribuição das funções para recursos do Azure difere das funções administrativas no Azure AD. As funções de recurso do Azure fornecem permissões granulares para o recurso no qual serão atribuídas, e a todos os recursos subordinados na hierarquia de recursos (conhecido como herança). [Saiba mais sobre RBAC, hierarquia de recursos e herança](../../role-based-access-control/role-assignments-portal.md). O PIM para as funções de diretório do Azure AD e os recursos do Azure pode ser administrado acessando o link apropriado na seção Gerenciar do menu de navegação à esquerda, ponto de entrada Visão geral de PIM.

O PIM fornece acesso conveniente para ativar funções, exibir ativações/solicitações pendentes, aprovações pendentes (para funções de diretório do Azure AD), e às revisões com resposta pendente na seção Tarefas do menu de navegação esquerdo.

Ao acessar qualquer um dos itens de menu Tarefas no ponto de entrada Visão geral, a exibição resultante conterá os resultados de funções de diretório do Azure AD e de funções dos recursos do Azure.

![Início rápido](./media/pim-configure/quick-start.png)

Minhas funções contêm uma lista de atribuições de função ativas e qualificadas para funções de diretório do Azure AD, e funções dos recursos do Azure. [Saiba mais sobre como ativar atribuições de função qualificadas](pim-how-to-activate-role.md).

A ativação de funções para recursos do Azure apresenta uma nova experiência que permite aos membros qualificados de uma função agendar a ativação para uma data/hora futura e selecionar uma duração de ativação específica dentro do máximo permitido pelos administradores.

![](./media/pim-configure/activations.png)

Caso uma ativação agendada não seja mais necessária, os usuários podem cancelar a solicitação pendente navegando até as solicitações pendentes no menu de navegação esquerdo, e clicando no botão Cancelar alinhado a essa solicitação.

![Solicitações pendentes](./media/pim-configure/pending-requests.png)

## <a name="privileged-identity-management-admin-dashboard"></a>Painel de administração do Privileged Identity Management

O Privileged Identity Manager do Azure AD oferece um painel de administração que fornece informações importantes, como:

* Alertas que indicam oportunidades para melhorar a segurança
* O número de usuários atribuídos a cada função com privilégios  
* O número de administradores elegíveis e permanentes
* Um grafo de ativações de funções com privilégios em seu diretório
* O número de atribuições Just-In-Time, Com limite de tempo e Permanente para funções de recurso do Azure
* Usuários e grupos com novas atribuições de função nos últimos 30 dias (funções de recursos do Azure)


![painel PIM - captura de tela](./media/pim-configure/PIM_Admin_Overview.png)

## <a name="privileged-role-management"></a>Gerenciamento de funções com privilégios

Com o Azure AD Privileged Identity Management, você pode gerenciar os administradores adicionando ou removendo os administradores permanentes ou elegíveis para cada função de diretório do Azure AD. Com o PIM para recursos do Azure, Proprietários, Administradores de Acesso do Usuário e Administradores Globais que habilitam o gerenciamento de Assinaturas em seu locatário podem atribuir a usuários ou grupos as funções de recurso do Azure como elegíveis (acesso Just-In-Time), acesso Com limite de tempo (ativação não necessária) com data/hora de início e de término ou Permanentes (caso isso esteja habilitado nas configurações da função).

![adicionar/remover administradores no PIM - captura de tela](./media/pim-configure/PIM_AddRemove.png)

## <a name="configure-the-role-activation-settings"></a>Definir as configurações de ativação de função

Usando as [configurações de função](pim-how-to-change-default-settings.md) , você pode configurar as propriedades de ativação elegíveis das funções de diretório do Azure AD, incluindo:

* A duração do período de ativação de função
* A notificação de ativação de função
* As informações que um usuário precisa fornecer durante o processo de ativação de função
* Tíquete de serviço ou número do incidente
* [Requisitos do fluxo de trabalho de aprovação](./azure-ad-pim-approval-workflow.md)

![configurações do PIM - ativação do administrador - captura de tela](./media/pim-configure/PIM_Settings_w_Approval_Disabled.png)

Observe que na imagem, os botões para **Autenticação Multifator** estão desabilitados. Certamente, funções com altos privilégios exigirão MFA para maior proteção.

As configurações de função para funções de recursos do Azure permitem que os administradores definam as configurações de atribuição Just-In-Time e Direta, incluindo:

- A capacidade de atribuir a usuários ou grupos funções sem uma data/hora de término (atribuição permanente)
- A duração padrão de uma atribuição (quando não for permanente)
- A duração máxima de ativação (quando um membro de função qualificado ativar)
- As informações que um usuário precisa fornecer durante a ativação da função (atribuições Just-In-Time) ou o processo de atribuição (atribuições diretas)

![](./media/pim-configure/role-settings-details.png)

## <a name="role-activation"></a>Ativação de função

Para [ativar uma função](pim-how-to-activate-role.md), um administrador qualificado solicita uma "ativação" com limite de tempo para a função. A ativação pode ser solicitada usando a opção **Ativar minha função** no Gerenciamento de identidades com privilégios do AD do Azure.

Um administrador que deseja ativar uma função precisa inicializar o Gerenciamento de identidades com privilégios do AD do Azure no Portal do Azure.

A ativação de função é personalizável. Nas configurações do PIM, você pode determinar o comprimento de ativação e as informações que o administrador precisa fornecer para ativar a função.

![ativação da função de solicitação do administrador do PIM - captura de tela](./media/pim-configure/PIM_RequestActivation.png)

## <a name="review-role-activity"></a>Examinar atividade de função

Há duas maneiras de controlar como seus funcionários e os administradores estão usando funções com privilégios. A primeira opção é usar o [Histórico de auditoria das Funções do diretório](pim-how-to-use-audit-log.md). Os logs de histórico de auditoria controlam as alterações em atribuições de função privilegiada, histórico de ativação de função e alterações nas configurações de funções dos recursos do Azure. 

![histórico da ativação do PIM - captura de tela](./media/pim-configure/PIM_ActivationHistory.png)

A segunda opção é configurar [revisões de acesso](pim-how-to-start-security-review.md)regulares. Essas revisões de acesso podem ser executadas pelo revisor (como um gerente de equipe) e atribuídas por ele, ou os funcionários podem examinar por conta própria. Essa é a melhor maneira de monitorar quem ainda precisa ter acesso e quem não precisa mais.

## <a name="azure-ad-pim-at-subscription-expiration"></a>Azure AD PIM na expiração da assinatura

Um locatário deve ter uma assinatura de avaliação ou paga do Azure AD Premium P2 (ou EMS E5) em seu locatário antes de usar o Azure AD PIM.  Além disso, as licenças devem ser atribuídas aos administradores do locatário.  Especificamente, as licenças devem ser atribuídas a administradores em funções do Azure AD gerenciadas por meio do Azure AD PIM, a administradores em funções de RBAC do Azure gerenciadas por meio do Azure AD PIM e quaisquer usuários não administradores que executarem revisões de acesso.
Se sua organização não renovar o Azure AD Premium P2 ou sua versão de avaliação expirar, os recursos do Azure AD PIM não estarão mais disponíveis em seu locatário, as atribuições de função qualificadas serão removidas e os usuários não poderão mais ativar funções. Você pode ler mais nos [requisitos de assinatura do Azure AD PIM](./subscription-requirements.md)

## <a name="next-steps"></a>Próximas etapas

- [Requisitos de assinatura para usar o PIM](subscription-requirements.md)
- [Funções de diretório do Azure AD que você pode gerenciar no PIM](pim-roles.md)
- [Protegendo o acesso privilegiado para implantações de nuvem e híbridos no Azure AD](../users-groups-roles/directory-admin-roles-secure.md?toc=%2fazure%2factive-directory%2fprivileged-identity-management%2ftoc.json)
