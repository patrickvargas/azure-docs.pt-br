---
title: Tutorial – Criar e gerenciar orçamentos do Azure | Microsoft Docs
description: Este tutorial ajuda a planejar e contabilizar os custos de serviços do Azure que você consome.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 11/02/2018
ms.topic: conceptual
ms.service: cost-management
manager: dougeby
ms.custom: ''
ms.openlocfilehash: 49341b320df98bb08ee4f5c4ee061a51bec29ff2
ms.sourcegitcommit: db2cb1c4add355074c384f403c8d9fcd03d12b0c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51686153"
---
# <a name="tutorial-create-and-manage-azure-budgets"></a>Tutorial: Criar e gerenciar orçamentos do Azure

Orçamentos no Gerenciamento de Custos ajudam você a planejar e promover responsabilidade organizacional. Com orçamentos, você pode considerar os serviços do Azure que consome ou assina durante um período específico. Eles ajudam você a informar outras pessoas sobre seus gastos para gerenciar proativamente os custos e monitorar como os gastos evoluem ao longo do tempo. Quando os limites de orçamento que você criou são excedidos, apenas notificações são disparadas. Nenhum de seus recursos é afetado e seu consumo não é interrompido. Você pode usar os orçamentos para comparar e controlar como analisar os custos de gastos.

Os orçamentos são redefinidos automaticamente no final de um período (mensal, trimestral ou anual) para o mesmo valor de orçamento quando você seleciona uma data de expiração no futuro. Uma vez que redefinidos com o mesmo valor de orçamento, você precisará criar orçamentos separados quando os valores monetários orçados forem diferentes para períodos futuros.

Os exemplos deste tutorial ajudam você a criar e editar um orçamento para uma assinatura do Azure Enterprise Agreement (EA).

Neste tutorial, você aprenderá como:

> [!div class="checklist"]
> * Criar um orçamento no portal do Azure
> * Editar um orçamento

## <a name="prerequisites"></a>Pré-requisitos

Orçamentos estão disponíveis a todos os clientes de EA do Azure. Você deve ter acesso de leitura para uma assinatura de EA do Azure para criar e gerenciar orçamentos. Você pode criar orçamentos individuais para grupos de recursos e assinaturas de EA. No entanto, não é possível criar os orçamentos para contas de cobrança de EA.

Há suporte para as seguintes permissões do Azure por assinatura para orçamentos por usuário e grupo:

- Proprietário – pode criar, modificar ou excluir os orçamentos para uma assinatura.
- Colaborador – pode criar, modificar ou excluir seus próprios orçamentos. Pode modificar o valor do orçamento para orçamentos criados por outras pessoas.
- Leitor – pode exibir os orçamentos para os quais ele têm permissão.

## <a name="sign-in-to-azure"></a>Entrar no Azure

- Entre no Portal do Azure em http://portal.azure.com.

## <a name="create-a-budget-in-the-azure-portal"></a>Criar um orçamento no portal do Azure

Você pode criar um orçamento de assinatura do Azure para um período mensal, trimestral ou anual. Seu conteúdo de navegação no portal do Azure determina se você cria um orçamento para uma assinatura ou para um grupo de recursos.

No portal do Azure, navegue até **Gerenciamento de Custos + Cobrança** &gt; **Assinaturas** &gt; Selecione uma assinatura &gt; **Orçamentos**. Neste exemplo, o orçamento que você cria é para a assinatura selecionada.

Depois de criar os orçamentos, eles mostram uma exibição simples de seus gastos atual em relação a eles.

Clique em **Adicionar**.

![Orçamentos de Gerenciamento de Custos](./media/tutorial-acm-create-budgets/budgets01.png)

Na janela **Criar orçamento**, insira um nome de orçamento e o valor do orçamento. Em seguida, escolha um período de duração mensal, trimestral ou anual. Em seguida, selecione uma data de término. Orçamentos exigem pelo menos um limite de custo (% do orçamento) e um endereço de email correspondente. Opcionalmente, você pode incluir até cinco limites e cinco endereços de email em um único orçamento. Quando um limite de orçamento é atingido, as notificações por email são recebidas normalmente em menos de oito horas.

Aqui está um exemplo de criação de um orçamento mensal de US$ 4.500. Um alerta por email é gerado quando 90% do orçamento é atingido.

![Exemplo de orçamento mensal](./media/tutorial-acm-create-budgets/monthly-budget01.png)

Quando você cria um orçamento trimestral, ele funciona da mesma maneira que um orçamento mensal. A diferença é que o valor do orçamento para o trimestre é dividido entre os três meses do trimestre. Como você poderia esperar, um valor do orçamento anual é dividido uniformemente entre todos os 12 meses do ano civil.

Os gastos atuais com relação aos orçamentos são atualizados sempre que o Gerenciamento de Custos recebe dados de cobrança atualizados. Isso costuma ocorrer diariamente.

![Gastos atuais com relação a orçamentos](./media/tutorial-acm-create-budgets/budgets-current-spending.png)

Depois de criar um orçamento, ele é mostrado na análise de custo. Exibir seu orçamento em relação à sua tendência de gastos é uma das primeiras etapas quando você começa a [analisar seus custos e gastos](quick-acm-cost-analysis.md).

![Orçamento mostrado na análise de custo](./media/tutorial-acm-create-budgets/cost-analysis.png)

No exemplo anterior, você criou um orçamento para uma assinatura. No entanto, você também pode criar um orçamento para um grupo de recursos. Se você quiser criar um orçamento para um grupo de recursos, navegue até **Gerenciamento de Custos + Cobrança** &gt; **Assinaturas** &gt; selecione uma assinatura > **Grupo de recursos** > selecione um grupo de recursos > **Orçamentos** > e, em seguida escolha **Adicionar** um orçamento.

## <a name="edit-a-budget"></a>Editar um orçamento

Dependendo do nível de acesso que você tem, é possível editar um orçamento para alterar suas propriedades. No exemplo a seguir, algumas das propriedades são somente leitura porque o usuário tem apenas permissão de Contribuinte para a assinatura. No momento, a **Data do término** está desabilitada e não pode ser modificada depois de definida.

![Editar o orçamento – permissão de colaborador](./media/tutorial-acm-create-budgets/edit-budget.png)


## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você aprendeu como:

> [!div class="checklist"]
> * Criar um orçamento no portal do Azure
> * Editar um orçamento

Avance para o próximo tutorial para criar uma exportação recorrente para os dados de gerenciamento de custos.

> [!div class="nextstepaction"]
> [Criar e gerenciar dados exportados](tutorial-export-acm-data.md)
