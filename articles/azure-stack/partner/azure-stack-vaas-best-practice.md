---
title: Práticas recomendadas de validação de pilha do Azure. | Microsoft Docs
description: Este artigo apresenta as práticas recomendadas para a validação como um serviço.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/26/2018
ms.author: mabrigg
ms.reviewer: John.Haskin
ms.openlocfilehash: d83eb501bbe685890decb3acbb8116164f4199db
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/27/2018
ms.locfileid: "52422653"
---
# <a name="best-practices-for-validation-as-a-service"></a>Práticas recomendadas para a validação como um serviço

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Este artigo aborda as práticas recomendadas para gerenciamento de recursos de validação como um serviço (VaaS). Para uma visão geral dos recursos de VaaS, consulte [validação como um conceitos principais do serviço](azure-stack-vaas-key-concepts.md).

## <a name="solution-management"></a>Gerenciamento de solução

### <a name="naming-convention-for-vaas-solutions"></a>Convenção de nomenclatura para soluções de VaaS

Use uma convenção de nomenclatura consistente para todas as soluções são registrados no VaaS. Por exemplo, o nome da solução pode ser construído da seguinte maneira das propriedades de hardware abaixo:

|Nome do Produto | Elemento de Hardware exclusivo 1 | Elemento de Hardware exclusivo 2 | Nome da solução
|---|---|---|---|
Minha solução XYZ |  Todos os Flash | Meu Switch X01 | MySolutionXYZ_AllFlash_MySwitchX01

### <a name="when-to-create-a-new-vaas-solution"></a>Quando criar uma nova solução VaaS

Use a mesma solução VaaS quando executam fluxos de trabalho no mesmo hardware de SKU. Uma nova solução VaaS deve ser criada somente quando há uma alteração para o SKU de hardware.

## <a name="workflow-management"></a>Gerenciamento de fluxo de trabalho

### <a name="naming-convention-for-vaas-workflows"></a>Convenção de nomenclatura para fluxos de trabalho VaaS

Use uma convenção de nomenclatura consistente para todas as execuções de fluxo de trabalho VaaS. Por exemplo, a construção de um fluxo de trabalho nome das propriedades de compilação abaixo da seguinte maneira:

|(Principal) do número de build | Data | Tamanho de solução | Nome do Fluxo de Trabalho
|---|---|---| ---|
1808 | 081518 | 4NODE | 1808_081518_4NODE

## <a name="next-steps"></a>Próximas etapas

- Saiba mais sobre [validação como um conceitos principais do serviço](azure-stack-vaas-key-concepts.md)
