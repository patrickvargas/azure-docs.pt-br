---
title: Índices na Azure Search | Microsoft Docs
description: Saiba mais sobre conceitos de índice no Azure Search e como os índices são usados.
author: brjohnstmsft
manager: jlembicz
ms.author: brjohnst
services: search
ms.service: search
ms.topic: conceptual
ms.date: 11/08/2017
ms.openlocfilehash: cc2c56d4463203aa2561e820540acdfff651d80a
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/23/2018
ms.locfileid: "31793395"
---
# <a name="indexes-in-azure-search"></a>Índices no Azure Search
> [!div class="op_single_selector"]
> * [Visão geral](search-what-is-an-index.md)
> * [Portal](search-create-index-portal.md)
> * [.NET](search-create-index-dotnet.md)
> * [REST](search-create-index-rest-api.md)
> 
> 

No Azure Search, um *índice* é o repositório persistente de *documentos* e outras construções usados por um serviço do Azure Search. Um documento é um dado pesquisável da unidade no índice. Por exemplo, uma loja de comércio eletrônico pode ter um documento para cada item vendido, uma agência de notícias pode ter um documento para cada artigo e assim por diante. Mapeando esses conceitos para equivalentes de banco de dados mais conhecidos: um *índice* é conceitualmente semelhante a uma *tabela* e *documentos* são equivalentes a *linhas* em uma tabela.

Quando você adiciona/carrega documentos e envia consultas de pesquisa para o Azure Search, envia suas solicitações para um índice específico em seu serviço de pesquisa.

## <a name="field-types-and-attributes-in-an-azure-search-index"></a>Tipos de campo e atributos em um índice do Azure Search
Quando você define o esquema, deve especificar o nome, tipo e atributos de cada campo no índice. O tipo de campo classifica os dados armazenados nesse campo. Os atributos são definidos em campos individuais para especificar como o campo será usado. A tabela a seguir enumera os tipos e atributos que você pode especificar.

### <a name="field-types"></a>Tipos de campo
| type | DESCRIÇÃO |
| --- | --- |
| *Edm.String* |O texto que opcionalmente pode ser indexado para a pesquisa de texto completo (separação de palavras, derivação etc). |
| *Collection(Edm.String)* |Uma lista de cadeias de caracteres que opcionalmente podem ser indexadas para a pesquisa de texto completo. Não há nenhum limite teórico superior no número de itens em uma coleção, mas o limite superior de 16 MB no tamanho da carga se aplica às coleções. |
| *Edm.Boolean* |Contém valores true/false. |
| *Edm.Int32* |Valores inteiros de 32 bits. |
| *Edm.Int64* |Valores inteiros de 64 bits. |
| *Edm.Double* |Dados numéricos de dupla precisão. |
| *Edm.DateTimeOffset* |Valores de data e hora representados no formato OData V4 (por exemplo, `yyyy-MM-ddTHH:mm:ss.fffZ` ou `yyyy-MM-ddTHH:mm:ss.fff[+/-]HH:mm`). |
| *Edm.GeographyPoint* |Um ponto que representa uma localização geográfica em todo o mundo. |

Você pode encontrar informações mais detalhadas sobre os [tipos de dados com suporte](https://docs.microsoft.com/rest/api/searchservice/Supported-data-types) do Azure Search.

### <a name="field-attributes"></a>Atributos do campo
| Atributo | DESCRIÇÃO |
| --- | --- |
| *Chave* |Uma cadeia de caracteres que fornece a ID exclusiva de cada documento, usada para pesquisar documentos. Todos os índices devem ter uma chave. Somente um campo pode ser a chave e seu tipo deve ser definido para Edm.String. |
| *Recuperável* |Especifica se um campo pode ser retornado em um resultado da pesquisa. |
| *Filtrável* |Permite que o campo seja usado nas consultas de filtro. |
| *Classificável* |Permite que uma consulta classifique os resultados da pesquisa usando esse campo. |
| *Com faceta* |Permite que um campo seja usado em uma estrutura de [navegação mista](search-faceted-navigation.md) para a filtragem autodirecionada do usuário. Normalmente, os campos que contêm valores repetidos que você pode usar para agrupar vários documentos (por exemplo, vários documentos que estejam em uma única categoria de marca ou de serviço) funcionam melhor como facetas. |
| *Pesquisável* |Marca o campo como texto completo pesquisável. |

Você pode encontrar informações mais detalhadas sobre os [atributos de índice](https://docs.microsoft.com/rest/api/searchservice/Create-Index) do Azure Search.

## <a name="guidance-for-defining-an-index-schema"></a>Diretrizes para definir um esquema de índice
Ao projetar o índice, reserve algum tempo na fase de planejamento para pensar em cada decisão. É importante ter em mente suas necessidades de negócios e experiência de usuário de pesquisa ao projetar o índice, pois cada campo deve ter os [atributos apropriados](https://docs.microsoft.com/rest/api/searchservice/Create-Index). A alteração de um índice após sua implantação envolve a recriação e o recarregamento dos dados.

Se os requisitos do armazenamento de dados mudarem com o tempo, você poderá aumentar ou diminuir a capacidade adicionando ou removendo as partições. Para obter detalhes, confira [Gerenciar o serviço Search no Azure](search-manage.md) ou [Limites de serviço](search-limits-quotas-capacity.md).

