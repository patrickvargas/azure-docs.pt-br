---
title: Uma visão geral da validação como um serviço para o Azure Stack | Microsoft Docs
description: Uma visão geral da validação de pilha do Azure como um serviço.
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
ms.reviewer: johnhas
ms.openlocfilehash: cb61b1ef1caa39f31331d8e9dc5e0da207959e89
ms.sourcegitcommit: 922f7a8b75e9e15a17e904cc941bdfb0f32dc153
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/27/2018
ms.locfileid: "52334917"
---
# <a name="what-is-validation-as-a-service-for-azure-stack"></a>O que é validação como um serviço para o Azure Stack?

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Validação como um serviço (VaaS) é um serviço do Azure nativo, projetado para os parceiros de solução que são de engenharia conjunta ofertas do Azure Stack com a Microsoft. Parceiros de solução podem usar o serviço para verificar se suas soluções de atender aos requisitos da Microsoft e funcionam conforme o esperado com o Azure Stack.

Os usos principais para VaaS são:

- Validando as novas soluções do Azure Stack
- Validando alterações para o software do Azure Stack
- Assinar digitalmente usados durante a implantação de pacotes de solução do parceiro
- Visualização VaaS Acessórios de teste

## <a name="validate-a-new-azure-stack-solution"></a>Validar uma nova solução do Azure Stack

Use a parceiros a **validação de solução** fluxo de trabalho para validar a novas soluções do Azure Stack. A solução deve passar os testes necessários de componentes de Hardware Lab Kit (HLK) do Azure Stack. Para certificar uma variedade de configurações de hardware, o fluxo de trabalho deve ser executado duas vezes para cada nova solução: uma vez a cada para as configurações de mínimas e máxima.

Para obter mais informações, consulte [validar uma nova solução do Azure Stack](azure-stack-vaas-validate-solution-new.md).

## <a name="validate-changes-to-the-azure-stack-software"></a>Validar alterações no software do Azure Stack

Use a parceiros a **validação de pacote** fluxo de trabalho para verificar se sua solução funciona com as atualizações de software mais recentes do Azure Stack. O fluxo de trabalho de validação do pacote deve ser executado em um ambiente de hardware recomendadas pela Microsoft em que o patch e atualização (P & U) foi usado para aplicar a atualização. É recomendável executar também o fluxo de trabalho na compilação da linha de base.

Para obter mais informações, consulte [validar as atualizações de software da Microsoft](azure-stack-vaas-validate-microsoft-updates.md).

## <a name="get-digitally-signed-solution-partner-packages"></a>Obter solução assinada digitalmente os pacotes de parceiro

Além de validar as atualizações do Azure Stack, a parceiros de usar o **validação de pacote** fluxo de trabalho para validar as atualizações para pacotes de personalização de OEM, que incluem os drivers de específicas de parceiro do Azure Stack, firmware e outros softwares usado durante a implantação do software do Azure Stack. Implante o pacote que você está validando na versão atual do software do Azure Stack usando pelo menos a solução em tamanho mínimo que terão suporte. O pacote é enviado para VaaS antes de executar testes. Se os testes forem bem-sucedidas, notificar [ vaashelp@microsoft.com ](mailto:vaashelp@microsoft.com) que o pacote tiver concluído o teste e deve ser digitalmente assinados com a assinatura digital do Azure Stack. A Microsoft assina o pacote e notifica o parceiro do Azure Stack que o pacote está disponível para download no portal do VaaS.

Para obter mais informações, consulte [pacotes de OEM validar](azure-stack-vaas-validate-oem-package.md).

## <a name="preview-vaas-test-collateral"></a>Visualização VaaS material de apoio de teste

Microsoft torna regularmente novos recursos disponíveis no Azure Stack. Como parte do processo de desenvolvimento para fornecer esses recursos de colocação no mercado, novos materiais adicionais de teste foi disponibilizado na **aprovação do teste** fluxo de trabalho. O fluxo de trabalho de aprovação de teste inclui material de apoio de teste dos outros fluxos de trabalho para permitir a execução de teste não oficial. Não use o fluxo de trabalho de aprovação do teste para enviar os resultados para aprovação. Use os fluxos de trabalho de validação de solução e a validação de pacote para obter aprovação oficial para sua solução.

Para obter mais informações, consulte [guia de início rápido: usar a validação como um portal de serviço para agendar seu primeiro teste](azure-stack-vaas-schedule-test-pass.md).

## <a name="next-steps"></a>Próximas etapas

- [Configurar sua validação como um serviço de recursos](azure-stack-vaas-set-up-resources.md)
- Saiba mais sobre [validação como um conceitos principais do serviço](azure-stack-vaas-key-concepts.md)
