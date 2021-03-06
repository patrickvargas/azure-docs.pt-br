---
title: Perguntas frequentes sobre o serviço de API de Conversão de Texto em Fala no Azure
titleSuffix: Azure Cognitive Services
description: Obtenha respostas às perguntas mais populares sobre o serviço de API de Conversão de Texto em Fala.
services: cognitive-services
author: PanosPeriorellis
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 06/11/2018
ms.author: panosper
ms.openlocfilehash: f8d80ab189d8ed1f4b153e81963ef31cc5f685b8
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/19/2018
ms.locfileid: "49470040"
---
# <a name="text-to-speech-frequently-asked-questions"></a>Perguntas Frequentes sobre a Conversão de Texto em Fala

Se você não encontrar respostas para suas perguntas nas perguntas frequentes, verifique[outras opções de suporte](support.md).

## <a name="general"></a>Geral

**Pergunta: Qual é a diferença entre um modelo de voz padrão e um modelo de voz personalizado?**

**Resposta**: O modelo de voz padrão (também chamado de *fonte de voz*) foi treinado usando dados de propriedade da Microsoft e já está implantado na nuvem. Os modelos de voz personalizados permitem que o usuário adapte um modelo médio e transfira o timbre e a forma de expressão de acordo com o estilo de voz do falante ou treine um modelo totalmente novo com base nos dados de treinamento preparados pelo usuário. Atualmente, cada vez mais clientes querem ter uma voz exclusiva e de marca para seus bots. A plataforma de compilação de voz personalizada é a escolha certa para isso.

**Pergunta: Por onde começar, se eu quiser usar um modelo de voz padrão?**

**Resposta**: Mais de 80 modelos de voz padrão em mais de 45 idiomas estão disponíveis através de solicitações HTTP. Primeiro, obtenha uma [chave de assinatura](https://docs.microsoft.com/azure/cognitive-services/speech-service/get-started). Para fazer chamadas REST para os modelos de voz pré-implantados, consulte a [API REST](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis#text-to-speech).

**Pergunta**: Se eu quiser usar um modelo de voz personalizado, a API será a mesma das vozes padrão?

**Resposta**: quando um modelo de voz personalizada é criado e implantado, você obter um ponto de extremidade exclusivo para seu modelo. Para usar a voz para falar em seus aplicativos, você deve especificar o ponto de extremidade nas suas solicitações HTTP. A mesma funcionalidade que está disponível na API REST para o serviço de Conversão de Texto em Fala está disponível para seu ponto de extremidade personalizado. Saiba como [criar e usar o ponto de extremidade personalizado](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-customize-voice-font#create-and-use-a-custom-endpoint).

**Pergunto: preciso preparar os dados de treinamento para criar modelos de voz personalizada por conta própria?**

**Resposta**: Sim, você deve preparar os dados de treinamento por conta própria para um modelo de voz personalizada.

Uma coleção de dados de fala é necessária para criar um modelo de voz personalizado. Esta coleção consiste em um conjunto de arquivos de áudio de gravações de fala e um arquivo de texto da transcrição de cada arquivo de áudio. O resultado da sua voz digital depende intensamente da qualidade dos seus dados de treinamento. Para produzir uma voz de texto em fala bom, é importante que as gravações sejam feitas em uma sala silenciosa com um microfone de alta qualidade. Volume consistente, velocidade de fala, densidade fala e até a consistência nos maneirismos expressivos da fala são essenciais para compilação de uma excelente voz digital. É altamente recomendável gravar as vozes em um estúdio de gravação.

Atualmente, não damos suporte para gravação online nem recomendações sobre estúdios de gravação. Para o requisito de formato, consulte [como preparar gravações e transcrições](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-customize-voice-font#prepare-recordings-and-transcripts).

**Pergunta: quais scripts devo usar para registrar os dados de fala para treinamento de voz personalizada?**

**Resposta**: Nós não limitamos os scripts para gravação de voz. Você pode usar seus próprios scripts para gravar a fala. Apenas garanta que você tenha cobertura fonética suficiente nos seus dados de fala. Para treinar uma voz personalizada, você pode começar com um pequeno volume de dados de fala, que podem ter 50 frases diferentes (cerca de 3 a 5 minutos de fala). Quanto mais dados você fornecer, mais natural será a voz. Você pode começar a treinar uma fonte de voz completa ao fornecer gravações de mais de 2,000 frases (cerca de 3 a 4 horas de fala). Para obter uma voz completa de alta qualidade, será necessário preparar gravações de mais de 6,000 frases (cerca de 8 a 10 horas de fala).

Nós fornecemos serviços adicionais para ajudá-lo a preparar scripts para gravação. Contate o [atendimento ao cliente de Voz Personalizada](mailto:customvoice@microsoft.com?subject=Inquiries%20about%20scripts%20generation%20for%20Custom%20Voice%20creation) para consultas.

**Pergunta: E se eu precisar de maior simultaneidade que o valor padrão ou do que é oferecido no portal?**

**Resposta**: você pode dimensionar o modelo em incrementos de 20 solicitações simultâneas. Contate o [atendimento ao cliente de Voz Personalizada](mailto:customvoice@microsoft.com?subject=Inquiries%20about%20scripts%20generation%20for%20Custom%20Voice%20creation) para consultas sobre maior dimensionamento.

**Pergunta: Posso baixar meu modelo e executá-lo localmente?**

**Resposta**: modelos não podem ser baixados e executados localmente.

**Pergunta: são minhas solicitações limitadas?**

**Resposta**: A API REST limita as solicitações para 25 por 5 segundos. Detalhes podem ser encontrados em nossas páginas para [Conversão de Texto em Fala](text-to-speech.md). 

## <a name="next-steps"></a>Próximas etapas

* [Solução de problemas](troubleshooting.md)
* [Notas de versão](releasenotes.md)
