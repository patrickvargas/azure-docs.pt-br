---
title: Arquivo de inclusão
description: Arquivo de inclusão
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 02/16/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: b4b06119b9d46781b967fc8d98808c60d2b41ccb
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38753625"
---
Você deseja certificar-se de que os usuários em sua organização têm o nível certo de acesso a esses recursos. Você não deseja conceder acesso ilimitado a usuários, mas também precisa certificar-se de que eles podem fazer o trabalho deles. O controle de acesso baseado em função (RBAC) permite que você gerencie quais usuários têm permissão para executar ações específicas em um escopo. Uma função define um conjunto de ações permitidas. Atribua a função a um escopo e especifique os usuários que pertencem a essa função para o escopo.

Ao planejar sua estratégia de controle de acesso, conceda a usuários o menor privilégio para realizarem seus trabalhos. A imagem a seguir mostra um padrão sugerido para a atribuição de RBAC.

![Escopo](./media/resource-manager-governance-rbac/role-examples.png)

Há três funcções que se aplicam a todos os recurso: Proprietário, Colaborador e Leitor. As contas atribuídas à função de Proprietário devem ser rigidamente controladas e raramente usadas. A função de leitor deve ser concedida a usuários que só precisem observar o estado de soluções.

À maioria dos usuários são concedidas [funções específicas do recurso](../articles/role-based-access-control/built-in-roles.md) ou [funções personalizadas](../articles/role-based-access-control/custom-roles.md) no nível do grupo de recurso ou da assinatura. Essas funções definem rigorosamente as ações permitidas. Ao atribuir usuários a essas funções, você pode conceder o acesso necessário a usuários sem permitir muito controle. Você pode atribuir uma conta a mais de uma função e fazer com que o usuário obtenha as permissões combinadas das funções. A concessão de acesso no nível do recurso geralmente é restritiva demais para os usuários, mas pode funcionar para um processo automatizado projetado para uma tarefa específica.

### <a name="who-can-assign-roles"></a>Quem pode atribuir funções

Para criar e remover as atribuições de função, os usuários devem ter `Microsoft.Authorization/roleAssignments/*` acesso. Esse acesso deve ser concedido pelas funções Proprietário ou Administrador de Acesso do Usuário.