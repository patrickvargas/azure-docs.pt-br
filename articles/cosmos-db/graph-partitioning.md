---
title: Particionamento na API do Gremlin do Azure Cosmos DB | Microsoft Docs
description: Saiba mais sobre como você pode usar um gráfico particionado no Azure Cosmos DB.
services: cosmos-db
author: luisbosquez
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-graph
ms.devlang: na
ms.topic: conceptual
ms.date: 02/28/2018
ms.author: lbosq
ms.openlocfilehash: 7e9de68866b5e5849d0e48ad5073fc7b89fbb1ca
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51239281"
---
# <a name="using-a-partitioned-graph-in-azure-cosmos-db"></a>Usando um gráfico particionado no Azure Cosmos DB

Um dos principais recursos da API do Gremlin no Azure Cosmos DB é a capacidade de lidar com gráficos em larga escala por meio de escalabilidade horizontal. Esse processo é obtido por meio das [funcionalidades de particionamento no Azure Cosmos DB](partition-data.md), que usa contêineres que podem ser dimensionados de forma independente em termos de armazenamento e produtividade. O Azure Cosmos DB oferece suporte aos seguintes tipos de contêineres em todas as APIs:

- **Contêiner fixo**: esses contêineres podem armazenar um banco de dados de grafo de até 10 GB com no máximo 10.000 unidades de solicitação por segundo alocadas a ele. Para criar um contêiner fixo não é necessário especificar uma propriedade de chave de partição nos dados.

- **Contêiner ilimitado**: esses contêineres podem ser dimensionado automaticamente para armazenar um grafo além do limite de 10 GB por meio do particionamento horizontal. Cada partição armazenará 10 GB e os dados serão balanceados automaticamente com base na **chave de partição especificada**, que será um parâmetro obrigatório ao usar um contêiner ilimitado. Esse tipo de contêiner pode armazenar um tamanho de dados praticamente ilimitado e pode permitir até 100.000 unidades de solicitação por segundo, ou mais [se entrar em contato com o suporte](https://aka.ms/cosmosdbfeedback?subject=Cosmos%20DB%20More%20Throughput%20Request).

Neste documento, as informações específicas sobre como os bancos de dados do gráfico são particionados serão descritas junto com suas implicações para os vértices (ou nós) e bordas.

## <a name="requirements-for-partitioned-graph"></a>Requisitos para o gráfico particionado

A seguir estão os detalhes que precisam ser compreendidos ao se criar um contêiner de gráfico particionado:

- **A configuração do particionamento será necessária** se o contêiner precisar ter mais de 10 GB e/ou se for necessário alocar mais de 10.000 unidades de solicitação por segundo (RU/s).

- **Ambos os vértices e as bordas são armazenados como documentos JSON** no back-end de um contêiner de API do Gremlin do Azure Cosmos DB.

- **Vértices exigem uma chave de partição**. Essa chave determina em qual partição o vértice será armazenado por meio de um algoritmo de hash. O nome dessa chave de partição é uma cadeia de caracteres de uma única palavra sem espaços nem caracteres especiais e ela é definida durante a criação de um novo contêiner usando o formato `/partitioning-key-name` no portal.

- **Bordas serão armazenadas com o vértice de origem**. Em outras palavras, para cada vértice sua chave de partição definirá onde ele será armazenado junto com suas bordas de saída. Isso é feito para evitar consultas entre partições ao usar a cardinalidade de `out()` em consultas de gráfico.

- **Consultas de gráfico precisam especificar uma chave de partição**. Para aproveitar ao máximo o particionamento horizontal no Azure Cosmos DB, a chave de partição deve ser especificada quando um único vértice for selecionado, sempre que possível. A seguir estão as consultas para selecionar os vários vértices em um gráfico particionado:

    - `/id` e `/label` não têm suporte como chaves de partição para um contêiner na API do Gremlin.


    - Selecionar um vértice por ID, em seguida, **usar a `.has()` etapa para especificar a propriedade de chave de partição**: 
    
        ```
        g.V('vertex_id').has('partitionKey', 'partitionKey_value')
        ```
    
    - Selecionar um vértice por **especificação de uma tupla, incluindo o valor da chave de partição e a ID**: 
    
        ```
        g.V(['partitionKey_value', 'vertex_id'])
        ```
        
    - Especificar uma **matriz de tuplas de valores de chave de partição e IDs**:
    
        ```
        g.V(['partitionKey_value0', 'verted_id0'], ['partitionKey_value1', 'vertex_id1'], ...)
        ```
        
    - Selecionar um conjunto de vértices e **especificar uma lista de valores de chave de partição**: 
    
        ```
        g.V('vertex_id0', 'vertex_id1', 'vertex_id2', …).has('partitionKey', within('partitionKey_value0', 'partitionKey_value01', 'partitionKey_value02', …)
        ```

## <a name="best-practices-when-using-a-partitioned-graph"></a>Melhores práticas ao usar um gráfico particionado

A seguir estão as diretrizes que devem ser seguidas para garantir a maior eficiência em desempenho e escalabilidade ao usar grafos particionados em contêineres ilimitados:

- **Sempre especifique o valor de chave de partição ao consultar um vértice**. Obter um vértice de uma partição conhecida é a maneira mais eficiente em termos de desempenho.

- **Use a direção de saída ao consultar as bordas sempre que possível**. Conforme mencionado acima, as bordas são armazenadas com seus vértices de origem na direção de saída. Isso significa que as chances de reclassificação para consultas entre partições são minimizadas quando os dados e as consultas são criadas com esse padrão em mente.

- **Escolha uma chave de partição que distribuirá uniformemente dados entre as partições**. Essa decisão depende muito do modelo de dados da solução. Leia mais sobre a criação de uma chave de partição apropriada em [Particionamento e escala no Azure Cosmos DB](partition-data.md).

- **Otimize as consultas para obter dados dentro dos limites de uma partição quando possível**. Uma estratégia de particionamento ideal seria alinhada com os padrões de consulta. Consultas que obtêm dados de uma única partição fornecem o melhor desempenho possível.

## <a name="next-steps"></a>Próximas etapas
Neste artigo, uma visão geral dos conceitos e práticas recomendadas para o particionamento com uma API do Gremlin do Azure Cosmos DB foi fornecida. 

* Saiba [Como particionar e dimensionar no Azure Cosmos DB](partition-data.md).
* Saiba mais sobre o [Suporte do Gremlin na API do Gremlin](gremlin-support.md).
* Saiba mais sobre [Introdução à API do Gremlin](graph-introduction.md).