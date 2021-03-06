---
title: Início rápido - Envie uma consulta para a API de pesquisa do Bing Local Business em Python | Microsoft Docs
titleSuffix: Azure Cognitive Services
description: Use este artigo para começar a usar a API de Pesquisa de Empresa Local do Bing em Python.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-local-business
ms.topic: article
ms.date: 11/01/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: 3123de58ea3dc4ab2c426f1393f8eca19542bfbf
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/02/2018
ms.locfileid: "50957094"
---
# <a name="quickstart-send-a-query-to-the-bing-local-business-search-api-in-python"></a>Início rápido: envie uma consulta para a API de pesquisa do Bing Local Business em Python

Use este início rápido para começar a enviar solicitações para a API de Pesquisa do Bing Local Business, que é um Serviço Cognitivo do Azure. Embora esse aplicativo simples seja escrito em Python, a API é um serviço da Web RESTful compatível com qualquer linguagem de programação capaz de fazer solicitações HTTP e analisar JSON.

Este aplicativo de exemplo obtém dados de resposta local da API para a consulta de pesquisa `hotel in Bellevue`.

## <a name="prerequisites"></a>Pré-requisitos

* [Python](https://www.python.org/) 2.x ou 3.x
 
Você deve ter uma [Conta da API de Serviços Cognitivos](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) com as APIs do Bing. A [avaliação gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) é suficiente para esse início rápido. Use a chave de acesso fornecida pela avaliação gratuita.

## <a name="run-the-complete-application"></a>Execute o aplicativo concluído

O código a seguir obtém resultados localizados. Ele é implementado nas etapas a seguir:
1. Declare variáveis para especificar o ponto de extremidade por host e caminho.
2. Especifique o parâmetro de consulta. 
3. Defina a função Search que cria a solicitação e adiciona o cabeçalho Ocp-Apim-Subscription-Key.
4. Defina o cabeçalho Ocp-Apim-Subscription-Key. 
5. Faça a conexão e envie a solicitação.
6. Imprima os resultados JSON.

O código completo para esta demonstração é:

````
import http.client, urllib.parse
import json

# Replace the subscriptionKey string value with your valid subscription key.
subscriptionKey = 'YOUR-SUBSCRIPTION-KEY'

host = 'api.cognitive.microsoft.com/bing'
path = '/v7.0/localbusinesses/search'

query = 'restaurant in Bellevue'

params = '?q=' + urllib.parse.quote (query) + '&appid=' + subscriptionKey + '&traffictype=Internal_monitor&mkt=en-us'

def get_local():
    headers = {'Ocp-Apim-Subscription-Key': subscriptionKey}
    conn = http.client.HTTPSConnection (host)
    conn.request ("GET", path + params, None, headers)
    response = conn.getresponse ()
    return response.read ()

result = get_local()
print (json.dumps(json.loads(result), indent=4))

````

## <a name="next-steps"></a>Próximas etapas
- [Início Rápido do Java pesquisa de local de negócios](local-search-java-quickstart.md)
- [Início Rápido do C# de Pesquisa de negócios local](local-quickstart.md)
- [Início Rápido de nó de pesquisa local de negócios](local-search-node-quickstart.md)