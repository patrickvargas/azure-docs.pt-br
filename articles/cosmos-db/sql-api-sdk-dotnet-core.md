---
title: 'Azure Cosmos DB: API, SDK e recursos do .NET Core do SQL | Microsoft Docs'
description: Saiba tudo sobre o SDK e a API do .NET Core do SQL, incluindo as datas de lançamento, as datas de desativação e as alterações feitas entre cada versão do SDK do .NET Core para Azure Cosmos DB.
services: cosmos-db
author: rnagpal
manager: kfile
editor: cgronlun
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: dotnet
ms.topic: reference
ms.date: 03/22/2018
ms.author: rnagpal
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d5216a4a21fbbded4a10429c658f4842db225657
ms.sourcegitcommit: ebf2f2fab4441c3065559201faf8b0a81d575743
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2018
ms.locfileid: "52161711"
---
# <a name="azure-cosmos-db-net-core-sdk-for-sql-api-release-notes-and-resources"></a>SDK do .NET Core do Azure Cosmos DB para a API do SQL: notas de versão e recursos
> [!div class="op_single_selector"]
> * [.NET](sql-api-sdk-dotnet.md)
> * [Feed de alterações do .NET](sql-api-sdk-dotnet-changefeed.md)
> * [.NET Core](sql-api-sdk-dotnet-core.md)
> * [Node.js](sql-api-sdk-node.md)
> * [Async Java](sql-api-sdk-async-java.md)
> * [Java](sql-api-sdk-java.md)
> * [Python](sql-api-sdk-python.md)
> * [REST](https://docs.microsoft.com/rest/api/cosmos-db/)
> * [Provedor de recursos REST](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/)
> * [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)
> * [BulkExecutor - .NET](sql-api-sdk-bulk-executor-dot-net.md)
> * [BulkExecutor - Java](sql-api-sdk-bulk-executor-java.md)

<table>

<tr><td>**Baixe o SDK**</td><td>[NuGet](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core/)</td></tr>

<tr><td>**Documentação da API**</td><td>[Documentação de referência de API .NET](/dotnet/api/overview/azure/cosmosdb?view=azure-dotnet)</td></tr>

<tr><td>**Exemplos**</td><td>[Exemplos de código .NET](sql-api-dotnet-samples.md)</td></tr>

<tr><td>**Introdução**</td><td>[Introdução ao SDK do .NET Core do Azure Cosmos DB](sql-api-dotnetcore-get-started.md)</td></tr>

<tr><td>**Tutorial do aplicativo Web**</td><td>[Desenvolvimento de aplicativos Web com o Azure Cosmos DB](sql-api-dotnet-application.md)</td></tr>

<tr><td>**Framework atualmente com suporte**</td><td>[.NET Standard 1.6 e .NET Standard 1.5](https://www.nuget.org/packages/NETStandard.Library)</td></tr>
</table></br>

## <a name="release-notes"></a>Notas de versão

O SDK do .NET Core do Azure Cosmos DB tem paridade de recurso com a versão mais recente do [SDK do .NET do Azure Cosmos DB](sql-api-sdk-dotnet.md).

### <a name="a-name213213"></a><a name="2.1.3"/>2.1.3

* Atualizado System.Net.Security para 4.3.2.

### <a name="a-name212212"></a><a name="2.1.2"/>2.1.2

* Aprimoramentos de rastreamento de diagnóstico.

### <a name="a-name211211"></a><a name="2.1.1"/>2.1.1

* Adição de maior resiliência a falhas transitórias de solicitação de várias regiões.

### <a name="a-name210210"></a><a name="2.1.0"/>2.1.0

* Adição de compatibilidade à gravação em várias regiões.
* Melhorias de desempenho de consultar entre partições com TOP e MaxBufferedItemCount.

### <a name="a-name200200"></a><a name="2.0.0"/>2.0.0

* Adicionado suporte para cancelamento de solicitações.
* Adicionado SetCurrentLocation para ConnectionPolicy, que preenche automaticamente as localizações preferenciais com base na região.
* Corrigido o Bug em Consultas de Partição Cruzada com Mín/Máx e um filtro que não corresponde a nenhum documento em uma partição individual.
* Os métodos DocumentClient agora têm paridade com IDocumentClient.
* Pilha de transporte de TCP direto atualizada para reduzir o número de conexões estabelecidas.
* Adicionado suporte para TCP de modo direto para clientes não Windows.

### <a name="a-name200-preview2200-preview2"></a><a name="2.0.0-preview2"/>2.0.0-preview2

* Adicionado suporte para cancelamento de solicitações.
* Adicionado SetCurrentLocation para ConnectionPolicy, que preenche automaticamente as localizações preferenciais com base na região.
* Corrigido o Bug em Consultas de Partição Cruzada com Mín/Máx e um filtro que não corresponde a nenhum documento em uma partição individual.

### <a name="a-name200-preview200-preview"></a><a name="2.0.0-preview"/>2.0.0-preview

* Os métodos DocumentClient agora têm paridade com IDocumentClient.
* Pilha de transporte de TCP direto atualizada para reduzir o número de conexões estabelecidas.
* Adicionado suporte para TCP de modo direto para clientes não Windows.

### <a name="a-name11001100"></a><a name="1.10.0"/>1.10.0

* Propriedade ConsistencyLevel adicionada a FeedOptions.
* JsonSerializerSettings adicionado a RequestOptions e FeedOptions.
* EnableReadRequestsFallback adicionado a ConnectionPolicy.

### <a name="a-name191191"></a><a name="1.9.1"/>1.9.1

* Corrigido KeyNotFoundException para consultas order by entre partições em situações extremas.
* Correção do bug no qual o atributo JsonPropery na cláusula select para consultas LINQ não estava sendo cumprido.

### <a name="a-name182182"></a><a name="1.8.2"/>1.8.2

* Correção do bug que ocorria em determinadas condições de corrida, que resultava em erros intermitentes “Microsoft.Azure.Documents.NotFoundException: a sessão de leitura não está disponível para o token de sessão de entrada” ao usar o nível de consistência de sessão.

### <a name="a-name181181"></a><a name="1.8.1"/>1.8.1

* Regressão fixa em que FeedOptions.MaxItemCount = -1 gerou uma System.ArithmeticException: o tamanho da página é negativo.
* Uma nova função ToString() foi adicionada a QueryMetrics.
* Estatísticas de partição foram expostas em coleções de leitura.
* A propriedade PartitionKey foi adicionada a ChangeFeedOptions.
* Correções de bug secundárias.

### <a name="a-name171171"></a><a name="1.7.1"/>1.7.1
 
 * Adiciona a capacidade de especificar índices exclusivos para os documentos usando a propriedade UniqueKeyPolicy na DocumentCollection.
 * Correção de um bug no qual as configurações personalizadas JsonSerializer não estavam sendo cumpridas por algumas consultas e pela execução de procedimento armazenado.

### <a name="a-name170170"></a><a name="1.7.0"/>1.7.0
 
 * Alteração da identidade visual do DocumentDB do Azure para o Azure Cosmos DB na referência de API, documentação, informações de metadados em assemblies e o pacote NuGet. 
 * Expor informações de diagnóstico e a latência da resposta de solicitações enviadas com o modo de conectividade direta. Os nomes de propriedade são RequestDiagnosticsString e RequestLatency na classe ResourceResponse.
 * Esta versão do SDK requer a versão mais recente do Emulador do Azure Cosmos DB disponível para fazer o download em https://aka.ms/cosmosdb-emulator.
 
### <a name="a-name160160"></a><a name="1.6.0"/>1.6.0

* Foram adicionadas vários correções e melhorias de confiabilidade.

### <a name="a-name151151"></a><a name="1.5.1"/>1.5.1 

* Alterações internas relacionadas ao suporte a [Microsoft.Azure.Graphs](https://docs.microsoft.com/azure/cosmos-db/graph-sdk-dotnet)

### <a name="a-name150150"></a><a name="1.5.0"/>1.5.0 

* Adição de suporte a PartitionKeyRangeId como FeedOption de modo a definir o escopo dos resultados da consulta para um valor específico do intervalo de chaves de partição. 
* Adição de suporte a StartTime como ChangeFeedOption para começar a procurar as alterações após esse horário. 

### <a name="a-name141141"></a><a name="1.4.1"/>1.4.1

*   Foi corrigido um problema na classe JsonSerializable que podia gerar uma exceção de excedente de pilha.

### <a name="a-name140140"></a><a name="1.4.0"/>1.4.0

*   Adicionado suporte para a especificação de JsonSerializerSettings personalizadas ao instanciar uma instância [DocumentClient](/dotnet/api/microsoft.azure.documents.client.documentclient?view=azure-dotnet).

### <a name="a-name132132"></a><a name="1.3.2"/>1.3.2

*   Suporte ao .NET Standard 1.5 como uma das estruturas de destino.

### <a name="a-name131131"></a><a name="1.3.1"/>1.3.1

*   Corrigido um problema que afetava computadores x64 que não davam suporte à instrução SSE4 e que geravam SEHException ao executarem consultas do Azure Cosmos DB.

### <a name="a-name130130"></a><a name="1.3.0"/>1.3.0

*   Adição de suporte a um novo nível de consistência chamado ConsistentPrefix.
*   Adição de suporte a métricas de consulta em partições individuais.
*   Adição de suporte para limitar o tamanho do token de continuação em consultas.
*   Adição de suporte para um rastreamento mais detalhado das solicitações com falha.
*   Melhorias de desempenho no SDK.

### <a name="a-name122122"></a><a name="1.2.2"/>1.2.2

* Correção de um problema no qual o valor de PartitionKey fornecido em FeedOptions para consultas de agregação é ignorado.
* Correção de um problema na manipulação transparente do gerenciamento de partição durante a metade da execução da consulta Order By entre partições.

### <a name="a-name121121"></a><a name="1.2.1"/>1.2.1

* Correção de um problema que causou deadlocks em algumas APIs assíncronas quando usado dentro do contexto do ASP.NET.

### <a name="a-name120120"></a><a name="1.2.0"/>1.2.0

* Correções para tornar o SDK mais resilientes ao failover automático em determinadas condições.

### <a name="a-name112112"></a><a name="1.1.2"/>1.1.2

* Correção de um problema que eventualmente causa uma WebException: O nome remoto não pôde ser resolvido.
* Adição do suporte para ler um documento digitado diretamente com a adição de novas sobrecargas à API ReadDocumentAsync.

### <a name="a-name111111"></a><a name="1.1.1"/>1.1.1

* Adicionado suporte ao LINQ para consultas de agregação (CONT.NÚM, MÍNIMO, MÁXIMO, SOMA e MÉDIA).
* Correção de um problema de vazamento de memória do objeto ConnectionPolicy causado pelo uso do manipulador de eventos.
* Correção de um problema no qual UpsertAttachmentAsync não estava funcionando quando ETag era usada.
* Correção de um problema no qual a continuação da consulta order-by entre partições não estava funcionando ao classificar no campo de cadeia de caracteres.

### <a name="a-name110110"></a><a name="1.1.0"/>1.1.0

* Suporte adicionado para consultas de agregação (COUNT, MIN, MAX, SUM e AVG). Veja [Suporte de agregação](how-to-sql-query.md#Aggregates).
* Taxa de transferência mínima reduzida em coleções particionadas de 10.100 RU/s a 2500 RU/s.

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0

O SDK do .NET Core para Azure Cosmos DB permite criar aplicativos [ASP.NET Core](https://www.asp.net/core) e [.NET Core](https://www.microsoft.com/net/core#windows) rápidos de plataforma cruzada para execução em Windows, Mac e Linux. A versão mais recente do SDK do .NET Core do Azure Cosmos DB é totalmente compatível com o [Xamarin](https://www.xamarin.com) e pode ser usada para criar aplicativos destinados a iOS, Android e Mono (Linux).  

### <a name="a-name010-preview010-preview"></a><a name="0.1.0-preview"/>0.1.0-preview

O SDK de Versão Prévia do .NET Core para Azure Cosmos DB permite criar aplicativos [ASP.NET Core](https://www.asp.net/core) e [.NET Core](https://www.microsoft.com/net/core#windows) rápidos de plataforma cruzada para execução em Windows, Mac e Linux.

O SDK de Versão Prévia do .NET Core do Azure Cosmos DB tem paridade de recurso com a versão mais recente do [SDK do .NET do Azure Cosmos DB](sql-api-sdk-dotnet.md) e dá suporte para o seguinte:
* Todos os [modos de conexão](performance-tips.md#networking): Modo de gateway, TCP Direto e HTTPs Direto. 
* Todos os [níveis de consistência](consistency-levels.md): Forte, Sessão, Desatualização Limitada e Eventual.
* [Coleções particionadas](partition-data.md). 
* [Contas e replicação geográfica de banco de dados de várias regiões](distribute-data-globally.md).

Se você tiver dúvidas relacionadas a esse SDK, poste no [StackOverflow](http://stackoverflow.com/questions/tagged/azure-documentdb) ou registre um problema no [repositório do github](https://github.com/Azure/azure-documentdb-dotnet/issues). 

## <a name="release--retirement-dates"></a>Datas de lançamento e desativação

| Versão | Data do lançamento | Data de desativação |
| --- | --- | --- |
| [2.1.3](#2.1.3) |15 de outubro de 2018 |--- |
| [2.1.2](#2.1.2) |04 de outubro de 2018 |--- |
| [2.1.1](#2.1.1) |27 de setembro de 2018 |--- |
| [2.1.0](#2.1.0) |21 de setembro de 2018 |--- |
| [2.0.0](#2.0.0) |07 de setembro de 2018 |--- |
| [1.9.1](#1.9.1) |09 de março de 2018 |--- |
| [1.8.2](#1.8.2) |21 de fevereiro de 2018 |--- |
| [1.8.1](#1.8.1) |5 de fevereiro de 2018 |--- |
| [1.7.1](#1.7.1) |16 de novembro de 2017 |--- |
| [1.7.0](#1.7.0) |10 de novembro, 2017 |--- |
| [1.6.0](#1.6.0) |17 de outubro de 2017 |--- |
| [1.5.1](#1.5.1) |2 de outubro de 2017 |--- |
| [1.5.0](#1.5.0) |10 de agosto de 2017 |--- | 
| [1.4.1](#1.4.1) |7 de agosto de 2017 |--- |
| [1.4.0](#1.4.0) |2 de agosto de 2017 |--- |
| [1.3.2](#1.3.2) |12 de junho de 2017 |--- |
| [1.3.1](#1.3.1) |23 de maio de 2017 |--- |
| [1.3.0](#1.3.0) |10 de maio de 2017 |--- |
| [1.2.2](#1.2.2) |19 de abril de 2017 |--- |
| [1.2.1](#1.2.1) |29 de março de 2017 |--- |
| [1.2.0](#1.2.0) |25 de março de 2017 |--- |
| [1.1.2](#1.1.2) |20 de março de 2017 |--- |
| [1.1.1](#1.1.1) |14 de março de 2017 |--- |
| [1.1.0](#1.1.0) |16 de fevereiro de 2017 |--- |
| [1.0.0](#1.0.0) |21 de dezembro de 2016 |--- |
| [0.1.0-preview](#0.1.0-preview) |15 de novembro de 2016 |31 de dezembro de 2016 |

## <a name="see-also"></a>Veja também
Para saber mais sobre o Cosmos DB, consulte a página de serviço do [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/). 

