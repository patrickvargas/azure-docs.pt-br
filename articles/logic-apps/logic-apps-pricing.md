---
title: Preço e cobrança - Aplicativos Lógicos do Azure | Microsoft Docs
description: Saiba como os preços e a cobrança funcionam para Aplicativos Lógicos do Azure
services: logic-apps
ms.service: logic-apps
ms.suite: logic-apps
author: kevinlam1
ms.author: klam
ms.reviewer: estfan, LADocs
ms.assetid: f8f528f5-51c5-4006-b571-54ef74532f32
ms.topic: article
ms.date: 09/24/2018
ms.openlocfilehash: 5f9147035c07bbe4fb3f38b74025015e70dd87b3
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/25/2018
ms.locfileid: "47159544"
---
# <a name="pricing-model-for-azure-logic-apps"></a>Modelo de preços para Aplicativo Lógico do Azure

Você pode criar e executar fluxos de trabalho automatizados integração escalonável na nuvem com os Aplicativos Lógicos do Azure. Aqui estão os detalhes sobre como cobrança e preços funcionam para Aplicativos Lógicos. 

<a name="consumption-pricing"></a>

## <a name="consumption-pricing-model"></a>Modelo de preço por consumo

Para novos aplicativos lógicos que você cria usando o serviço de aplicativos lógicos público ou "global", você paga apenas pelo que usa. Esses aplicativos lógicos usam um plano baseado em consumo e um modelo de preços, o que significa que todas as execuções de ação executadas por um aplicativo lógico são medidas. Cada etapa em uma definição de aplicativo lógico é uma ação que inclui gatilhos, etapas de fluxo de controle, como condições, escopos, loops for each e loops do until, chamadas a conectores. Para obter mais informações, consulte a [Preços de Aplicativos Lógicos](https://azure.microsoft.com/pricing/details/logic-apps).

<a name="fixed-pricing"></a>

## <a name="fixed-pricing-model"></a>Modelo de preços fixo

> [!NOTE]
> O ambiente do serviço de integração está em *versão prévia privada*. Para solicitar acesso, [crie sua solicitação para ingressar aqui](https://aka.ms/iseprivatepreview).

Para novos aplicativos lógicos que você cria com um [*ISE* (ambiente de serviço de integração)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md), que é uma instância privada isolada de Aplicativos Lógicos que usa recursos dedicados, você paga um preço mensal fixo para ações internas e conectores padrão rotulados por ISE. O ISE inclui um conector Empresarial sem custo, enquanto os conectores Empresariais adicionais são cobrados com base no preço de consumo da empresa. Para obter mais informações, consulte a [Preços de Aplicativos Lógicos](https://azure.microsoft.com/pricing/details/logic-apps).

<a name="triggers"></a>

## <a name="triggers"></a>Gatilhos

Os gatilhos são ações especiais que criam uma instância de aplicativo lógico quando ocorre um evento específico. Dispara ações de diferentes formas, o que afeta o modo como o aplicativo lógico é monitorado.

* **Gatilho de sondagem** – esse gatilho verifica continuamente um ponto de extremidade para mensagens que satisfaçam os critérios para a criação de uma instância de aplicativo lógico e começar o fluxo de trabalho. Cada solicitação de sondagem conta como uma execução e é medida, mesmo quando nenhuma instância do aplicativo lógico é criada. Para especificar o intervalo de sondagem, configure o gatilho no Designer de Aplicativos Lógicos.

  [!INCLUDE [logic-apps-polling-trigger-non-standard-metering](../../includes/logic-apps-polling-trigger-non-standard-metering.md)]

* **Gatilho de webhook** – esse disparador espera o envio de uma solicitação por parte do cliente em um ponto de extremidade específico. Cada solicitação enviada ao ponto de extremidade do webhook conta como uma execução da ação. Por exemplo, a Solicitação e o gatilho Webhook HTTP são gatilhos de webhook.

* **Gatilho de recorrência** – esse gatilho cria uma nova instância do aplicativo lógico com base no intervalo de recorrência configurado no gatilho. Por exemplo, você pode definir um gatilho de recorrência que é executado a cada três dias ou em um agendamento mais complexo.

Você pode encontrar as execuções do gatilho no painel de visão geral do aplicativo lógico na seção de histórico de gatilho.

## <a name="actions"></a>Ações

As ações internas, como as ações que chamam HTTP, Azure Functions ou gerenciamento de API, além das etapas do fluxo de controle, são medidas como ações nativas, que têm seus respectivos tipos. Ações que chamam [conectores](https://docs.microsoft.com/connectors) têm o tipo de "ApiConnection". Esses conectores são classificados como conectores standard ou enterprise, que são medidos com base em seus respectivos [preços][pricing]. Conectores empresariais na *Versão Prévia* são cobrados como conectores Standard.

Todas as ações de execução com êxito e sem êxito são contadas e monitoradas como execuções de ação. Entretanto, as ações que foram ignoradas devido a uma condição não atendida, ou ações que não foram executadas porque o aplicativo lógico foi encerrado antes da conclusão, não são contadas como execuções de ação. Aplicativos lógicos desabilitados não podem instanciar novas instâncias, para que eles não sejam cobrados enquanto eles estão desabilitados.

> [!NOTE]
> Depois de desativar um aplicativo lógico, qualquer instância em execução no momento pode demorar algum tempo antes de parar completamente.

Ações que são executadas em loops são contadas por cada ciclo no loop. Por exemplo, uma única ação em um loop "for each" que processa uma lista de itens de 10 é contada pela multiplicação do número de itens de lista (10), o número de ações no loop (1) mais um para iniciar o loop. Portanto, neste exemplo, o cálculo é (10 * 1) + 1, o que resulta em 11 execuções de ação.

## <a name="integration-account-usage"></a>Uso da conta de integração

Incluído no consumo com base em uso está uma [conta de integração](logic-apps-enterprise-integration-create-integration-account.md) para exploração, desenvolvimento e testes, permitindo que você use os recursos [B2B/EDI](logic-apps-enterprise-integration-b2b.md) e [processamento XML](logic-apps-enterprise-integration-xml.md) de Aplicativos Lógicos sem custo adicional. Você pode ter uma conta de integração por região e armazenar até [números de artefatos](../logic-apps/logic-apps-limits-and-config.md) específicos, como parceiros comerciais de EDI e contratos, mapas, esquemas, assemblies, certificados e configurações de lote.

Aplicativos lógicos também oferecem contas de integração básico e padrão com suporte à SLA de Aplicativos Lógicos. Quando você quiser usar apenas a manipulação de mensagens ou atuar como um parceiro de pequenas empresas que tenha uma relação de parceiro comercial com uma entidade de negócios maior, você pode usar contas de integração básica. Contas de integração Standard dão suporte a relações de B2B mais complexas e aumentam o número de entidades que você pode gerenciar. Para saber mais, veja [Preços do Azure](https://azure.microsoft.com/pricing/details/logic-apps).

## <a name="next-steps"></a>Próximas etapas

* [Saiba mais sobre os Aplicativos Lógicos][whatis]
* [Criar seu primeiro aplicativo lógico][create]

[pricing]: https://azure.microsoft.com/pricing/details/logic-apps/
[whatis]: logic-apps-overview.md
[create]: quickstart-create-first-logic-app-workflow.md

