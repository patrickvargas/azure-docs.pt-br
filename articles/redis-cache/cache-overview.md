---
title: O que é o Cache Redis do Azure? | Microsoft Docs
description: Saiba o que é o Cache Redis do Azure e como ele é normalmente usado.
services: redis-cache
documentationcenter: ''
author: wesmc7777
manager: cfowler
editor: ''
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: overview
ms.date: 03/26/2018
ms.author: wesmc
ms.custom: mvc
ms.openlocfilehash: 585dcd120c42562b1520d4454f9d04e445553101
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37096177"
---
# <a name="what-is-azure-redis-cache"></a>O que é o Cache Redis do Azure

O Cache Redis do Azure é baseado no popular [Cache Redis](https://redis.io/)de software livre. Normalmente, ele é usado como um cache para melhorar o desempenho e a escalabilidade de sistemas que dependem fortemente de armazenamentos de dados de back-end. O desempenho é aprimorado com a cópia temporária dos dados acessados com frequência para um armazenamento rápido localizado próximo ao aplicativo. Com o [cache Redis](https://redis.io/), esse armazenamento rápido está localizado na memória com o Cache Redis, em vez de ser carregado do disco por um banco de dados.

O Cache Redis do Azure também pode ser usado como um repositório de estruturas de dados na memória, um banco de dados não relacional distribuído e um agente de mensagens. O desempenho do aplicativo é aprimorado com o uso da baixa latência, do desempenho de alta taxa de transferência do mecanismo Redis.

O Cache Redis do Azure oferece acesso a um cache Redis seguro e dedicado, gerenciado pela Microsoft, hospedado no Azure e acessível a qualquer aplicativo dentro ou fora do Azure.

## <a name="why-use-azure-redis-cache"></a>Por que usar o Cache Redis do Azure

Há muitos padrões comuns nos quais o Cache Redis é usado para dar suporte à arquitetura do aplicativo ou para melhorar seu desempenho. Alguns dos mais comuns incluem o seguinte:

| Padrão      | DESCRIÇÃO                                        |
| ------------ | -------------------------------------------------- |
| [Cache-Aside](cache-web-app-cache-aside-leaderboard.md) | Como um banco de dados pode ser grande, o carregamento de um banco de dados inteiro em um cache não é uma abordagem recomendada. É comum usar o padrão [cache-aside](https://docs.microsoft.com/azure/architecture/patterns/cache-aside) para carregar itens de dados no cache, somente conforme necessário. Quando o sistema faz alterações nos dados de back-end, nesse momento, ele também pode atualizar o cache, que é distribuído com outros clientes. Além disso, o sistema pode definir uma expiração em itens de dados ou usar uma política de remoção para fazer com que as atualizações de dados sejam recarregadas no cache.|
| [Cache de conteúdo](cache-aspnet-output-cache-provider.md) | A maioria das páginas da Web é gerada com base em modelos com cabeçalhos, rodapés, barras de ferramentas, menus, etc. Na verdade, eles não são alterados com frequência e não devem ser gerados dinamicamente. O uso de um cache na memória, como o Cache Redis do Azure, fornecerá o acesso rápido aos servidores Web a esse tipo de conteúdo estático comparado aos armazenamentos de dados de back-end. Esse padrão reduz o tempo de processamento e a carga do servidor necessários para gerar o conteúdo dinamicamente. Isso permite que os servidores Web sejam mais dinâmicos e pode permitir que você reduza o número de servidores necessários para manipular as cargas. O Cache Redis do Azure fornece o Provedor de Cache de Saída do Redis para ajudar a dar suporte a esse padrão com o ASP.NET.|
| [Cache de sessão de usuário](cache-aspnet-session-state-provider.md) | Esse padrão é geralmente usado com carrinhos de compras e outras informações de tipo de histórico do usuário que um aplicativo Web pode desejar associar aos cookies do usuário. O armazenamento de muitas informações em um cookie pode ter um impacto negativo no desempenho conforme o tamanho do cookie aumenta e é passado e validado com cada solicitação. Uma solução típica é usar o cookie como uma chave para consultar os dados em um banco de dados de back-end. O uso de um cache na memória, como o Cache Redis do Azure, para associar as informações a um usuário é muito mais rápido que a interação com um banco de dados relacional completo. |
| Enfileiramento de mensagens e trabalhos | Quando os aplicativos recebem solicitações, geralmente, as operações associadas à solicitação levam mais tempo para serem executadas. É um padrão comum adiar operações de execução mais longa adicionando-os a uma fila, que é processada posteriormente, e possivelmente por outro servidor. Esse método de adiamento do trabalho é chamado de enfileiramento de tarefas. Há muitos componentes de software projetados para dar suporte a filas de tarefas. O Cache Redis também cumpre essa finalidade, bem como uma fila distribuída.|
| Transações distribuídas | É um requisito comum que aplicativos possam executar uma série de comandos em um armazenamento de dados de back-end como uma única operação (atômica). Todos os comandos devem ter êxito ou ser revertidos para o estado inicial. O Cache Redis dá suporte à execução de um lote de comandos como uma única operação na forma de [Transações](https://redis.io/topics/transactions). |

## <a name="azure-redis-cache-offerings"></a>Ofertas do Cache Redis do Azure

O Cache Redis do Azure está disponível nas seguintes camadas:

| Camada | DESCRIÇÃO |
|---|---|
Basic | Um cache de nó único. Essa camada dá suporte a vários tamanhos de memória (250 MB – 53 GB). Esta é uma camada ideal de desenvolvimento/teste e cargas de trabalho não críticas. A camada Básica não tem nenhum SLA (Contrato de Nível de Serviço) |
| Standard | Um cache replicado em uma configuração primária/secundária de dois nós gerenciado pela Microsoft, com um SLA de alta disponibilidade (99,9%) |
| Premium | A camada Premium é a camada pronta para Enterprise. Os Caches da camada Premium dão suporte a mais recursos e têm uma maior taxa de transferência com latências menores. Os caches na camada Premium são implantados em um hardware mais potente, fornecendo um melhor desempenho em comparação com a camada Básica ou Standard. Essa vantagem significa que a taxa de transferência para um cache do mesmo tamanho será maior na camada Premium comparado à camada Standard |

> [!TIP]
> Para obter mais informações sobre tamanho, taxa de transferência e largura de banda com caches premium, consulte [Perguntas frequentes sobre o Cache Redis do Azure](cache-faq.md#what-redis-cache-offering-and-size-should-i-use).
>

Você pode dimensionar seu cache até uma camada superior depois de criá-lo. Não há suporte para a redução para uma camada inferior. Para obter instruções sobre dimensionamento passo a passo, consulte [Como dimensionar o Cache Redis do Azure](cache-how-to-scale.md) e [Como automatizar uma operação de dimensionamento](cache-how-to-scale.md#how-to-automate-a-scaling-operation).

### <a name="feature-comparision"></a>Comparação de recursos

A página [Preço do Cache Redis](https://azure.microsoft.com/pricing/details/cache/) fornece uma comparação detalhada de cada camada. A seguinte tabela ajuda a descrever alguns dos recursos compatíveis por camada:

| Descrição do recurso | Premium | Standard | Basic |
| ------------------- | :-----: | :------: | :---: |
| [Contrato de nível de serviço (SLA)](https://azure.microsoft.com/support/legal/sla/cache/v1_0/) |✔|✔|-|
| [Persistência de dados do Redis](cache-how-to-premium-persistence.md) |✔|-|-|
| [Cluster Redis](cache-how-to-premium-clustering.md) |✔|-|-|
| [Segurança por meio de regras de Firewall](cache-configure.md#firewall) |✔|✔|✔|
| [Isolamento e segurança aprimorada com a VNET](cache-how-to-premium-vnet.md) |✔|-|-|
| [Importar/exportar](cache-how-to-import-export-data.md) |✔|-|-|
| [Agendar atualizações](cache-administration.md#schedule-updates) |✔|-|-|
| [Replicação geográfica](cache-how-to-geo-replication.md) |✔|-|-|
| [Reboot](cache-administration.md#reboot) |✔|✔|✔|

## <a name="next-steps"></a>Próximas etapas

* [Início Rápido de aplicativo Web ASP.NET](cache-web-app-howto.md) Crie um aplicativo Web ASP.NET simples que usa um Cache Redis do Azure.
* [Início Rápido do .NET](cache-dotnet-how-to-use-azure-redis-cache.md) Criar um aplicativo .NET que usa um Cache Redis do Azure.
* [Início Rápido do .NET Core](cache-dotnet-core-quickstart.md) Crie um aplicativo .NET que usa um Cache Redis do Azure.
* [Início Rápido do Node.js](cache-nodejs-get-started.md) Crie um aplicativo simples do Node.js que usa um Cache Redis do Azure.
* [Início Rápido do Java](cache-java-get-started.md) Crie um aplicativo Java simples que usa um Cache Redis do Azure.
* [Início Rápido do Python](cache-python-get-started.md) Crie um aplicativo simples do Python que usa um Cache Redis do Azure.
