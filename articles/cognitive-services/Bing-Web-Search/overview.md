---
title: O que é a Pesquisa na Web do Bing?
titleSuffix: Azure Cognitive Services
description: A API de Pesquisa na Web do Bing é um serviço RESTful que fornece respostas instantâneas às consultas de usuário. Os resultados da pesquisa são facilmente definidos para incluir as páginas da Web, imagens, vídeos, notícias, traduções e muito mais. Os resultados são fornecidos como JSON e são baseados na relevância de pesquisa e em suas assinaturas de pesquisa na Web do Bing.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: overview
ms.date: 08/14/2018
ms.author: aahi
ms.openlocfilehash: 738941afadbad318d383a119967e3f98c098a9c1
ms.sourcegitcommit: ebf2f2fab4441c3065559201faf8b0a81d575743
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2018
ms.locfileid: "52160708"
---
# <a name="what-is-bing-web-search"></a>O que é a Pesquisa na Web do Bing?

A API de Pesquisa na Web do Bing é um serviço RESTful que fornece respostas instantâneas às consultas de usuário. Os resultados da pesquisa são facilmente definidos para incluir as páginas da Web, imagens, vídeos, notícias, traduções e muito mais. Os resultados são fornecidos como JSON e são baseados na relevância de pesquisa e em suas assinaturas de pesquisa na Web do Bing.

Essa API é ideal para aplicativos que precisam de acesso a todo o conteúdo relevante para a consulta de pesquisa do usuário. Se você estiver criando um aplicativo que exige apenas um tipo específico de resultados, considere usar a [API de Pesquisa de Imagem do Bing](../Bing-Image-Search/overview.md), [API de Pesquisa de Vídeo do Bing](../Bing-Video-Search/search-the-web.md), ou [API de Pesquisa de Notícias do Bing](../Bing-News-Search/search-the-web.md). Ver [API de Serviços Cognitivos](https://docs.microsoft.com/azure/cognitive-services#cognitive-services-apis) para obter uma lista completa das APIs de Pesquisa do Bing.

Quer ver como funciona? Experimente a [demonstração da API de Pesquisa na Web do Bing ](https://azure.microsoft.com/services/cognitive-services/bing-web-search-api/).

## <a name="features"></a>Recursos  

Além das respostas instantâneas, a Pesquisa na Web do Bing fornece recursos adicionais e funcionalidades que permitem que você personalize os resultados da pesquisa para seus usuários.

| Recurso | DESCRIÇÃO |
|---------|-------------|
| [Sugerir os termos de pesquisa em tempo real](../bing-autosuggest/get-suggested-search-terms.md) | Aprimore sua experiência com aplicativos usando a API de Sugestão Automática do Bing para exibir os termos de pesquisa sugeridos à medida que eles são digitados. |
| [Filtrar e restringir os resultados por tipo de conteúdo](filter-answers.md) | Personalize e refine os resultados da pesquisa com filtros e parâmetros de consulta para páginas da Web, imagens, vídeos, pesquisa segura e muito mais. |
| [Realce de ocorrências para caracteres unicode](hit-highlighting.md) | Identifique e remova caracteres unicode indesejados nos resultados da pesquisa antes de exibi-los para os usuários com realce de ocorrências. |
| [Localizar resultados da pesquisa por país, região e/ou mercado](supported-countries-markets.md) | A Pesquisa na Web do Bing dá suporte a mais de três dúzias de países ou regiões. Use esse recurso para refinar os resultados da pesquisa para um país/região ou mercado específico. |
| [Analisar métricas de pesquisa com Estatísticas do Bing](bing-web-stats.md) | As Estatísticas do Bing são uma assinatura paga que fornece análise sobre o volume de chamadas, as principais cadeias de caracteres de consulta, distribuição geográfica e muito mais. |

## <a name="workflow"></a>Fluxo de trabalho

A API de Pesquisa na Web do Bing é fácil de ser chamada a partir de qualquer linguagem de programação que pode fazer solicitações HTTP e analisar as respostas JSON. O serviço é acessado usando a [API REST](quickstarts/python.md) ou os [SDKs de Pesquisa na Web do Bing](web-sdk-python-quickstart.md).  

1. Crie uma [Conta da API dos Serviços Cognitivos](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) com acesso às APIs de Pesquisa do Bing. Caso não tenha uma assinatura do Azure, é possível criar uma [conta gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api).  
2. Envie uma [solicitação à API de Pesquisa na Web do Bing](quickstarts/python.md).
3. Analise a resposta JSON.

## <a name="next-steps"></a>Próximas etapas

* Use nosso [início rápido do Python](quickstarts/python.md) para fazer sua primeira chamada à API de Pesquisa na Web do Bing.  
* [Compilar um aplicativo Web de página única](tutorial-bing-web-search-single-page-app.md).
* Revisar a documentação de [referência da API de Pesquisa na Web v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference).  
* Saiba mais sobre [usar e exibir requisitos](UseAndDisplayRequirements.md) para Pesquisa na Web do Bing.  
