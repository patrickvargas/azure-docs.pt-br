---
title: Enunciados em aplicativos do LUIS
titleSuffix: Azure Cognitive Services
description: Enunciados são entradas do usuário que seu aplicativo precisa interpretar. Colete frases que você acredita que os usuários vão inserir. Inclua enunciados que tenham o mesmo significado, mas sejam construídos de modo diferente em termos de comprimento e posicionamento de palavras.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: conceptual
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 2b21102a5416c1fd7e2abb35be677c48ffc63263
ms.sourcegitcommit: 17633e545a3d03018d3a218ae6a3e4338a92450d
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/22/2018
ms.locfileid: "49638062"
---
# <a name="utterances-in-luis"></a>Enunciados em LUIS

**Enunciados** são entrada do usuário que seu aplicativo precisa interpretar. Para treinar LUIS para extrair intenções e entidades deles, é importante capturar uma variedade de diferentes entradas para cada intenção. Aprendizado ativo, ou o processo de continuar a treinar em novos enunciados, é essencial para a inteligência de aprendizado de máquina que o LUIS fornece.

Colete frases que você acredita que os usuários vão inserir. Inclua enunciados que tenham o mesmo significado, mas sejam construídos de modo diferente em termos de comprimento e posicionamento de palavras. 

## <a name="how-to-choose-varied-utterances"></a>Como escolher enunciados variados
Quando você começa [adicionando enunciados de exemplo](luis-how-to-add-example-utterances.md) ao modelo do LUIS, aqui estão alguns princípios a serem lembrados.

### <a name="utterances-arent-always-well-formed"></a>Os enunciados nem sempre são bem formados
Pode ser uma frase como "Reserve para mim uma passagem para Paris" ou um fragmento de uma frase, como "Reservando" ou "Voo para Paris."  Os usuários costumam cometer erros de ortografia. Ao planejar seu aplicativo, considere se você fará ou não a verificação ortográfica da entrada do usuário antes de passá-la para o LUIS. A [API de Verificação Ortográfica do Bing][BingSpellCheck] integra-se ao LUIS. Você poderá associar seu aplicativo de LUIS com uma chave externa para a API de Verificação Ortográfica do Bing quando o publicar. Se você não realizar a verificação ortográfica de enunciados do usuário, deverá treinar o LUIS em enunciados que incluam erros de digitação e erros de ortografia.

### <a name="use-the-representative-language-of-the-user"></a>Usar o idioma representante do usuário
Ao escolher enunciados, lembre-se de que o que você considera um termo ou frase comum pode não o ser para o usuário típico do seu aplicativo cliente. Eles não podem ter a experiência de domínio. Tenha cuidado ao usar termos ou frases que um usuário somente diria se fosse um especialista.

### <a name="choose-varied-terminology-as-well-as-phrasing"></a>Escolha a terminologia e frases variadas
Você descobrirá que, mesmo que você se esforce para criar padrões de frases variados, ainda repetirá algum vocabulário.

Pegue estes enunciados de exemplo:

|Exemplo de enunciados|
|--|
|Como obtenho um computador?|
|Onde obtenho um computador?|
|Quero obter um computador, como faço isso?|
|Quando posso ter um computador?| 

O termo principal aqui, "computador", não é variado. As pessoas poderiam dizer computador desktop, laptop, estação de trabalho ou até mesmo apenas computador. O LUIS infere de modo inteligente os sinônimos com base no contexto, mas quando você cria enunciados para treinamento, ainda é melhor variá-los.

## <a name="example-utterances-in-each-intent"></a>Enunciados de exemplo em cada intenção
Cada intenção deve ter enunciados de exemplo, pelo menos de 10 a 15. Se você tiver uma intenção que não tenha nenhum enunciado de exemplo, não poderá treinar o LUIS. Se você tiver uma intenção com um ou vários enunciados de exemplo, o LUIS não preverá mais com precisão a intenção. 

## <a name="add-small-groups-of-10-15-utterances-for-each-authoring-iteration"></a>Adicionar pequenos grupos de 10 a 15 enunciados para cada iteração de criação
Em cada iteração do modelo, não adicione uma grande quantidade de enunciados. Adicione enunciados em quantidades de dezenas. [Treine](luis-how-to-train.md), [publique](luis-how-to-publish-app.md) e [teste](luis-interactive-test.md) novamente.  

O LUIS cria modelos eficazes com enunciados selecionados com cuidado. Adicionar enunciados demais não tem valor porque leva a confusão.  

É melhor começar com alguns enunciados e, em seguida, [examinar enunciados de ponto de extremidade](luis-how-to-review-endoint-utt.md) para a extração de entidade e previsão de intenção corretas.

## <a name="ignoring-words-and-punctuation"></a>Ignorando palavras e pontuação
Se você quiser ignorar palavras específicas ou pontuação no enunciado de exemplo, use um [padrão](luis-concept-patterns.md#pattern-syntax) com a sintaxe _ignore_. 

## <a name="training-utterances"></a>Treinando enunciados
O treinamento é não determinístico: a previsão de enunciado pode variar levemente entre versões ou aplicativos.

## <a name="testing-utterances"></a>Testando enunciados 

Os desenvolvedores devem começar a testar seus aplicativos de LUIS com tráfego real enviando enunciados ao ponto de extremidade. Esses enunciados são usados para melhorar o desempenho de intenções e entidades com [Examinar enunciados](luis-how-to-review-endoint-utt.md). Testes enviadas com o painel de teste de site do LUIS não são enviados por meio do ponto de extremidade e, portanto, não contribuem para aprendizado ativo. 

## <a name="review-utterances"></a>Examinar enunciados
Depois que seu modelo estiver treinado, publicado e recebendo consultas de [ponto de extremidade](luis-glossary.md#endpoint), [examine os enunciados](luis-how-to-review-endoint-utt.md) sugeridos pelo LUIS. O LUIS seleciona enunciados de ponto de extremidade que têm pontuações baixas para intenção ou entidade. 

## <a name="best-practices"></a>Práticas recomendadas
Examine as [melhores práticas](luis-concept-best-practices.md) para saber mais.

## <a name="next-steps"></a>Próximas etapas
Consulte [Adicionar enunciados de exemplo](luis-how-to-add-example-utterances.md) para obter informações sobre o treinamento de um aplicativo de LUIS para entender enunciados do usuário.

[BingSpellCheck]: https://docs.microsoft.com/azure/cognitive-services/bing-spell-check/proof-text