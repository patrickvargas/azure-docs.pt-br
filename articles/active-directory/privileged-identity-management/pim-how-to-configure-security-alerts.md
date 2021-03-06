---
title: Configurar alertas de segurança para funções de diretório do Azure AD no PIM | Microsoft Docs
description: Saiba como configurar alertas de segurança para funções do diretório do Azure AD no Azure AD PIM (Privileged Identity Management).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: pim
ms.date: 11/01/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: e7204c223681b9a33c439b0d9fc653167422384a
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51011690"
---
# <a name="configure-security-alerts-for-azure-ad-directory-roles-in-pim"></a>Configurar alertas de segurança para funções do diretório do Azure AD no PIM

Azure Active Directory Privileged Identity Management (PIM) do Azure AD gera alertas quando há atividades suspeitas ou inseguras em seu ambiente. Quando um alerta é disparado, ele aparece no painel PIM. Selecione o alerta para ver um relatório que lista os usuários ou as funções que dispararam o alerta.

![Alertas de segurança do PIM - captura de tela](./media/pim-how-to-configure-security-alerts/pim-directory-alerts.png)

## <a name="security-alerts"></a>Alertas de segurança

Esta seção lista todos os alertas de segurança para funções de diretório, além de como corrigir e prevenir. Severidade tem o seguinte significado:

* **Alta**: exige ação imediata devido a uma violação da política.
* **Média**: não exige ação imediata, mas sinaliza uma possível violação da política.
* **Baixa**: não requer ação imediata, mas sugere uma alteração de política preferencial.

### <a name="roles-are-being-assigned-outside-of-pim"></a>As funções estão sendo atribuídas fora do PIM

| | |
| --- | --- |
| **Severidade** | Alto |
| **Por que recebo este alerta?** | Atribuições de funções privilegiadas feitas fora do PIM não são monitoradas adequadamente e podem indicar um ataque ativo. |
| **Como corrigir?** | Revise os usuários na lista e remova-os das funções privilegiadas designadas fora do PIM. |
| **Prevenção** | Investigue onde os usuários estão sendo atribuídos a funções privilegiadas fora do PIM e proíba atribuições futuras de lá. |
| **Ação de mitigação no portal** | Remove a conta da sua função privilegiada. |

### <a name="potential-stale-accounts-in-a-privileged-role"></a>Contas obsoletas possíveis em uma função com privilégios

| | |
| --- | --- |
| **Severidade** | Média |
| **Por que recebo este alerta?** | As contas que não alteraram sua senha recentemente podem ser contas de serviço ou compartilhadas que não estão sendo atualizadas. Essas contas em funções privilegiadas são vulneráveis a invasores. |
| **Como corrigir?** | Examine as contas na lista. Se eles não precisarem mais de acesso, remova-os de suas funções privilegiadas. |
| **Prevenção** | Certifique-se de que as contas compartilhadas estejam girando senhas fortes quando houver uma alteração nos usuários que conhecem a senha. </br>Revise regularmente as contas com funções privilegiadas usando revisões de acesso e remova as atribuições de funções que não são mais necessárias. |
| **Ação de mitigação no portal** | Remove a conta da sua função privilegiada. |

### <a name="users-arent-using-their-privileged-roles"></a>Os usuários não estão utilizando suas funções privilegiadas

| | |
| --- | --- |
| **Severidade** | Baixo |
| **Por que recebo este alerta?** | Os usuários que receberam papéis privilegiados que não precisam aumentam a chance de um ataque. Também é mais fácil para os invasores permanecerem despercebidos nas contas que não estão sendo ativamente usadas. |
| **Como corrigir?** | Revise os usuários na lista e remova-os das funções privilegiadas de que eles não precisam. |
| **Prevenção** | Somente atribua funções privilegiadas a usuários com justificativa comercial. </br>Agende revisões de acesso regulares para verificar se os usuários ainda precisam de acesso. |
| **Ação de mitigação no portal** | Remove a conta da sua função privilegiada. |
| **Gatilho** | Acionado se um usuário passar um certo tempo sem ativar uma função. |
| **Número de dias** | Essa configuração especifica o número de dias, de 0 a 100, que um usuário pode acessar sem ativar uma função.|

### <a name="there-are-too-many-global-administrators"></a>Há muitos administradores globais

| | |
| --- | --- |
| **Severidade** | Baixo |
| **Por que recebo este alerta?** | Administrador Global é o maior papel privilegiado. Se um Administrador Global for comprometido, o invasor terá acesso a todas as suas permissões, o que coloca todo o seu sistema em risco. |
| **Como corrigir?** | Revise os usuários na lista e remova qualquer um que não precise absolutamente da função Administrador Global. </br>Atribua esses usuários as funções menos privilegiadas. |
| **Prevenção** | Designe aos usuários a função menos privilegiada de que precisam. |
| **Ação de mitigação no portal** | Remove a conta da sua função privilegiada. |
| **Gatilho** | Acionado se dois critérios diferentes forem atendidos e você puder configurar ambos. Primeiro, você precisa atingir um certo limite de Administradores Globais. Segundo, uma determinada porcentagem de suas atribuições totais de funções deve ser Administradores Globais. Se você atender apenas a uma dessas medidas, o alerta não será exibido. |
| **Número mínimo de administradores globais** | Essa configuração especifica o número de Administradores Globais, de 2 a 100, que você considera uma quantia insegura. |
| **Percentual de administradores globais** | Essa configuração especifica a porcentagem mínima de administradores que são Administradores Globais, de 0% a 100%, que não são seguros em seu ambiente. |

### <a name="roles-are-being-activated-too-frequently"></a>As funções estão sendo ativadas com muita frequência

| | |
| --- | --- |
| **Severidade** | Baixo |
| **Por que recebo este alerta?** | Múltiplas ativações para o mesmo papel privilegiado pelo mesmo usuário é um sinal de um ataque. |
| **Como corrigir?** | Revise os usuários na lista e assegure-se de que a [duração da ativação](pim-how-to-change-default-settings.md) para sua função privilegiada esteja definida por tempo suficiente para que eles executem suas tarefas. |
| **Prevenção** | Assegure-se de que a [duração da ativação](pim-how-to-change-default-settings.md) para funções privilegiadas esteja definida por tempo suficiente para que os usuários executem suas tarefas.</br>[Exija o MFA](pim-how-to-change-default-settings.md) para funções privilegiadas que tenham contas compartilhadas por vários administradores. |
| **Ação de mitigação no portal** | N/D |
| **Gatilho** | Disparado se um usuário ativar a mesma função privilegiada várias vezes dentro de um período especificado. Você pode configurar o período e o número de ativações. |
| **Período de tempo de renovação de ativação** | Essa configuração especifica em dias, horas, minutos e segundos o período de tempo que você deseja usar para rastrear renovações suspeitas. |
| **Número de renovações de ativação** | Esta configuração especifica o número de ativações, de 2 a 100, que você considera merecedor de alerta, dentro do período de tempo escolhido. Você pode mudar essa configuração movendo o controle deslizante ou digitando um número na caixa de texto. |

### <a name="roles-dont-require-mfa-for-activation"></a>Funções de não exigem MFA para ativação

| | |
| --- | --- |
| **Severidade** | Baixo |
| **Por que recebo este alerta?** | Sem o MFA, os usuários comprometidos podem ativar funções privilegiadas. |
| **Como corrigir?** | Revise a lista de funções e [exija o MFA](pim-how-to-change-default-settings.md) para cada função. |
| **Prevenção** | [Exigir MFA](pim-how-to-change-default-settings.md) para cada função.  |
| **Ação de mitigação no portal** | Torna o MFA necessário para a ativação da função privilegiada. |

## <a name="configure-security-alert-settings"></a>Definir configurações de alerta de segurança

Você pode personalizar os alertas de segurança no PIM para trabalhar com seu ambiente e objetivos de segurança. Siga estas etapas para abrir as configurações de alerta de segurança:

1. Abra o **Azure AD Privileged Identity Management**.

1. Clique em **funções do Microsoft Azure Active Directory**.

1. Clique em **as configurações** e, em seguida **alertas**.

    ![Navegar até as configurações de alertas de segurança](./media/pim-how-to-configure-security-alerts/settings-alerts.png)

1. Clique em um nome de alerta para definir a configuração desse alerta.

    ![Configurações de alerta de segurança](./media/pim-how-to-configure-security-alerts/security-alert-settings.png)

## <a name="next-steps"></a>Próximas etapas

- [Definir configurações de função do diretório do Azure AD no PIM](pim-how-to-change-default-settings.md)
