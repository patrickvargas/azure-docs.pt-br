---
title: Referência entidades de número de telefone predefinidas de LUIS – Azure | Microsoft Docs
titleSuffix: Azure
description: Este artigo contém informações sobre a entidade predefinida de número de telefone de LUIS (Serviço Inteligente de Reconhecimento Vocal).
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 11/27/2018
ms.author: diberry
ms.openlocfilehash: 9a8fcbaf946f936d7a6d6d883a0416fce9d0c158
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2018
ms.locfileid: "52441700"
---
# <a name="phonenumber-entity"></a>Entidade de número de telefone
A entidade `phonenumber` extrai uma variedade de números de telefone, incluindo o código do país. Uma vez que essa entidade já está treinada, não é necessário adicionar enunciados de exemplo ao aplicativo. A entidade `phonenumber` é compatível somente com a cultura `en-us`. 

## <a name="types-of-phonenumber"></a>Tipos de número de telefone
O número de telefone é gerenciado por meio do repositório do GitHub [Recognizers-text](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/Base-PhoneNumbers.yaml)

## <a name="resolution-for-prebuilt-phonenumber-entity"></a>Resolução de entidade número de telefone predefinida
O exemplo a seguir mostra a resolução da entidade **builtin.phonenumber**.

```JSON
{
  "query": "my mobile is 00 44 161 1234567",
  "topScoringIntent": {
    "intent": "None",
    "score": 0.8448457
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.8448457
    }
  ],
  "entities": [
    {
      "entity": "00 44 161 1234567",
      "type": "builtin.phonenumber",
      "startIndex": 13,
      "endIndex": 29,
      "resolution": {
        "value": "00 44 161 1234567"
      }
    }
  ]
}
```


## <a name="next-steps"></a>Próximas etapas

Saiba mais sobre as entidades [percentual](luis-reference-prebuilt-percentage.md), [número](luis-reference-prebuilt-number.md) e [temperatura](luis-reference-prebuilt-temperature.md). 