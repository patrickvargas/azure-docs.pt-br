---
title: Como procurar um endereço usando o serviço de pesquisa de Mapas do Azure | Microsoft Docs
description: Saiba como procurar um endereço usando o serviço de pesquisa de Mapas do Azure
author: dsk-2015
ms.author: dkshir
ms.date: 09/11/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: d25142cecd62fc34a08939d5f1915ba9b2cee749
ms.sourcegitcommit: f20e43e436bfeafd333da75754cd32d405903b07
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2018
ms.locfileid: "49387422"
---
# <a name="find-an-address-using-the-azure-maps-search-service"></a>Encontrar um endereço usando o serviço de pesquisa do Azure Maps

O serviço de pesquisa do Maps é um conjunto de APIs RESTful projetadas para desenvolvedores pesquisarem endereços, locais, pontos de interesse, listagens de empresas e outras informações geográficas. O serviço atribui um valor de latitude/longitude para um endereço específico, cruzamento, recurso geográfico ou POI (ponto de interesse). Os valores de latitude e longitude retornados pela pesquisa podem ser usados como parâmetros em outros serviços de mapas, como rota e fluxo de tráfego.

## <a name="prerequisites"></a>Pré-requisitos

Para fazer todas as chamadas para as APIs do serviço de mapas, você precisa de uma conta de mapas e chave. Para obter mais informações sobre como criar uma conta e recuperar uma chave, consulte [Como gerenciar as chaves e a conta dos Mapas do Azure](how-to-manage-account-keys.md).

Este artigo usa o [aplicativo Postman](https://www.getpostman.com/apps) para criar chamadas REST. Você pode usar qualquer ambiente de desenvolvimento de API que você preferir.

## <a name="using-fuzzy-search"></a>Usando a Pesquisa Difusa

A API padrão do serviço de pesquisa é [pesquisa difusa](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy) e é útil quando você não sabe quais são as entradas do usuário para uma consulta de pesquisa. A API combina pesquisa e geo codificação de POI em uma 'pesquisa de linha única' canônica. Por exemplo, a API pode manipular entradas de qualquer combinação de endereço ou token POI. Também pode ser ponderado com uma posição contextual (lat./lon. par), totalmente restrito por uma coordenada e raio, ou executado de forma mais geral sem qualquer ponto de ancoragem de polarização geográfica.

A maioria das consultas de pesquisa é padronizada para `maxFuzzyLevel=1` para obter desempenho e reduzir resultados incomuns. Esse padrão pode ser substituído, conforme necessário, por solicitação, passando o parâmetro de consulta `maxFuzzyLevel=2` ou `3`.

### <a name="search-for-an-address-using-fuzzy-search"></a>Procurar um endereço usando a Pesquisa Difusa

1. Abra o aplicativo Postman, clique em Novo | Criar Novo e selecione **Solicitação GET**. Insira um nome de solicitação de **Pesquisa difusa**, selecione uma coleção ou uma pasta onde salvá-la e clique em **Salvar**.

2. Na guia Construtor, selecione o método HTTP **GET** e insira a URL de solicitação para o ponto de extremidade de API.

    ![Pesquisa Difusa ](./media/how-to-search-for-address/fuzzy_search_url.png)

    | Parâmetro | Valor sugerido |
    |---------------|------------------------------------------------|
    | Método HTTP | GET |
    | URL de Solicitação | [https://atlas.microsoft.com/search/fuzzy/json?](https://atlas.microsoft.com/search/fuzzy/json?) |
    | Autorização | Sem autenticação |

    O atributo **json** no caminho da URL determina o formato da resposta. Você está usando json neste artigo pela facilidade de uso e leitura. Você pode encontrar os formatos de resposta disponíveis na **obter pesquisa difusa** definição da [referência da API de mapas funcionais](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy).

3. Clique em **Params** e digite os seguinte pares Chave/Valor para uso como parâmetros de consulta ou de caminho na URL da solicitação:

    ![Pesquisa Difusa ](./media/how-to-search-for-address/fuzzy_search_params.png)

    | Chave | Valor |
    |------------------|-------------------------|
    | api-version | 1.0 |
    | subscription-key | \<sua chave de mapas do Azure\> |
    | query | pizza |

4. Clique em **Enviar** e analise o corpo da resposta.

    A string de consulta ambígua de "pizza" retornou 10 [resultados de resultado de ponto de interesse](https://docs.microsoft.com/rest/api/maps/search/getsearchpoi#searchpoiresponse) (POI) com categorias que se enquadram em "pizza" e "restaurante". Cada resultado retorna um endereço de rua, valores de latitude/longitude, exibições de porta e pontos de entrada para o local.
  
    Os resultados variam para essa consulta, não associados a nenhum local de referência específico. Você pode usar o parâmetro **countrySet** para especificar apenas os países nos quais seu aplicativo precisa de cobertura, já que o comportamento padrão é pesquisar no mundo inteiro, retornando possivelmente resultados desnecessários.

5. Adicione a seguinte par de Chave/Valor para a seção **Params** e clique em **Enviar**:

    | Chave | Valor |
    |------------------|-------------------------|
    | countrySet | EUA |
  
    Os resultados agora são limitados pelo código do país e a consulta retorna as pizzarias no Brasil.
  
    Para fornecer resultados para um local, você pode consultar um ponto de interesse e usar os valores de latitude e longitude retornados em sua chamada para o serviço de pesquisa difusa. Nesse caso, você usou o Serviço de pesquisa para retornar a localização do Corcovado e usou os valores de lat. /long. para orientar a pesquisa.
  
6. Em Params, insira os seguintes pares Chave/Valor e clique em **Enviar**:

    ![Pesquisa Difusa ](./media/how-to-search-for-address/fuzzy_search_latlon.png)
  
    | Chave | Valor |
    |-----|------------|
    | lat | 47.620525 |
    | long | -122.349274 |

## <a name="search-for-address-properties-and-coordinates"></a>Pesquisar propriedades e coordenadas de endereço

Você pode transmitir um endereço de rua completo ou parcial para a API de pesquisa de endereço e receber uma resposta que inclui as propriedades detalhadas do endereço, como o município ou o bairro, bem como valores de posição de latitude e longitude.

1. No Postman, clique em **Nova Solicitação** | **Solicitação GET** e nomeie-a **Pesquisa de Endereço**.
2. Na guia Criador, selecione o método HTTP **GET**, insira a URL de solicitação para o ponto de extremidade de API e selecione um protocolo de autorização, se houver.

    ![Pesquisa de Endereço ](./media/how-to-search-for-address/address_search_url.png)
  
    | Parâmetro | Valor sugerido |
    |---------------|------------------------------------------------|
    | Método HTTP | GET |
    | URL de Solicitação | [https://atlas.microsoft.com/search/address/json?](https://atlas.microsoft.com/search/address/json?) |
    | Autorização | Sem autenticação |

3. Clique em **Params** e digite os seguinte pares Chave/Valor para uso como parâmetros de consulta ou de caminho na URL da solicitação:
  
    ![Pesquisa de Endereço ](./media/how-to-search-for-address/address_search_params.png)
  
    | Chave | Valor |
    |------------------|-------------------------|
    | api-version | 1.0 |
    | subscription-key | \<sua chave de mapas do Azure\> |
    | query | Rua Cosme Velho, 513, Rio de Janeiro, 22241-090 |
  
4. Clique em **Enviar** e analise o corpo da resposta.
  
    Nesse caso, você especificou uma consulta de endereço completo e recebe um único resultado no corpo da resposta.
  
5. Em Params, edite a cadeia de consulta com o seguinte valor:
    ```plaintext
        400 Broad, Seattle
    ```

6. Adicione a seguinte par de Chave/Valor para a seção **Params** e clique em **Enviar**:

    | Chave | Valor |
    |-----|------------|
    | typeahead | verdadeiro |

    O sinalizador **typeahead** informa à API de Pesquisa de Endereço para tratar a consulta como uma entrada parcial e retornar uma matriz de valores de previsão.

## <a name="search-for-a-street-address-using-reverse-address-search"></a>Procurar um endereço usando a Pesquisa Invertida de Endereço

1. No Postman, clique em **Nova Solicitação** | **Solicitação GET** e nomeie-a **Pesquisa Invertida de Endereço**.

2. Na guia Construtor, selecione o método HTTP **GET** e insira a URL de solicitação para o ponto de extremidade de API.
  
    ![URL de Pesquisa Invertida de Endereço ](./media/how-to-search-for-address/reverse_address_search_url.png)
  
    | Parâmetro | Valor sugerido |
    |---------------|------------------------------------------------|
    | Método HTTP | GET |
    | URL de Solicitação | [https://atlas.microsoft.com/search/address/reverse/json?](https://atlas.microsoft.com/search/address/reverse/json?) |
    | Autorização | Sem autenticação |
  
3. Clique em **Params** e digite os seguinte pares Chave/Valor para uso como parâmetros de consulta ou de caminho na URL da solicitação:
  
    ![Parâmetros de Pesquisa Invertida de Endereço ](./media/how-to-search-for-address/reverse_address_search_params.png)
  
    | Chave | Valor |
    |------------------|-------------------------|
    | api-version | 1.0 |
    | subscription-key | \<sua chave de mapas do Azure\> |
    | query | 47.591180,-122.332700 |
  
4. Clique em **Enviar** e analise o corpo da resposta.

    A resposta inclui informações importantes sobre o endereço do Safeco Field.
  
5. Adicione a seguinte par de Chave/Valor para a seção **Params** e clique em **Enviar**:

    | Chave | Valor |
    |-----|------------|
    | número | verdadeiro |

    Se o parâmetro de consulta [number](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse#search_getsearchaddressreverse_uri_parameters) é enviado com a solicitação, a resposta pode incluir o lado da rua (esquerda/direita) e também uma posição ajustada para o número.
  
6. Adicione a seguinte par de Chave/Valor para a seção **Params** e clique em **Enviar**:

    | Chave | Valor |
    |-----|------------|
    | returnSpeedLimit | verdadeiro |
  
    Quando o parâmetro de consulta [returnSpeedLimit](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse#search_getsearchaddressreverse_uri_parameters) é definido, a resposta retorna o limite de velocidade indicado.

7. Adicione a seguinte par de Chave/Valor para a seção **Params** e clique em **Enviar**:

    | Chave | Valor |
    |-----|------------|
    | returnRoadUse | verdadeiro |

    Quando o [returnRoadUse](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse#search_getsearchaddressreverse_uri_parameters) parâmetro de consulta for definido, a resposta retorna a matriz de uso de estrada para códigos geográficos reversos no nível da rua.

8. Adicione a seguinte par de Chave/Valor para a seção **Params** e clique em **Enviar**:

    | Chave | Valor |
    |-----|------------|
    | roadUse | verdadeiro |

    Você pode restringir a consulta invertida de geocodificação para um tipo específico de estrada usando o parâmetro de consulta [roadUse](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse#search_getsearchaddressreverse_uri_parameters).
  
## <a name="search-for-the-cross-street-using-reverse-address-cross-street-search"></a>Pesquise o cruzamento usando Pesquisa Invertida de Endereço de Cruzamento

1. No Postman, clique em **Nova Solicitação** | **Solicitação GET** e nomeie-a **Pesquisa Invertida de Endereço de Cruzamento**.

2. Na guia Construtor, selecione o método HTTP **GET** e insira a URL de solicitação para o ponto de extremidade de API.
  
    ![Pesquisa Invertida de Endereço de Cruzamento ](./media/how-to-search-for-address/reverse_address_search_url.png)
  
    | Parâmetro | Valor sugerido |
    |---------------|------------------------------------------------|
    | Método HTTP | GET |
    | URL de Solicitação | [https://atlas.microsoft.com/search/address/reverse/crossstreet/json?](https://atlas.microsoft.com/search/address/reverse/crossstreet/json?) |
    | Autorização | Sem autenticação |
  
3. Clique em **Params** e digite os seguinte pares Chave/Valor para uso como parâmetros de consulta ou de caminho na URL da solicitação:
  
    | Chave | Valor |
    |------------------|-------------------------|
    | api-version | 1.0 |
    | subscription-key | \<sua chave de mapas do Azure\> |
    | query | 47.591180,-122.332700 |
  
4. Clique em **Enviar** e analise o corpo da resposta.

## <a name="next-steps"></a>Próximas etapas

- Explore a documentação da [API do serviço de pesquisa do Azure Maps](https://docs.microsoft.com/rest/api/maps/search).
