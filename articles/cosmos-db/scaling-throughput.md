---
title: Dimensionar a taxa de transferência no Azure Cosmos DB
description: Este artigo descreve como o Azure Cosmos DB dimensiona a taxa de transferência de maneira elástica
services: cosmos-db
author: dharmas
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/15/2018
ms.author: dharmas
ms.reviewer: sngun
ms.openlocfilehash: d29f01c7f953ed211b429e41b844a01c67e41054
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/08/2018
ms.locfileid: "51282366"
---
# <a name="scaling-throughput-in-azure-cosmos-db"></a>Dimensionar a taxa de transferência no Azure Cosmos DB

No Azure Cosmos DB, a taxa de transferência provisionada é representada como unidades de solicitação/segundo (RU/s ou, no plural, RUs). As RUs medem o custo das operações de leitura e de gravação em seu contêiner do Cosmos como mostrado nesta imagem:

![Unidades de solicitação](./media/scale-throughput/figure1.png)

Você pode provisionar RUs em um contêiner ou em um banco de dados do Cosmos. RUs provisionadas em um contêiner ficam disponíveis exclusivamente para operações executadas no contêiner. RUs provisionadas em um banco de dados são compartilhadas entre todos os contêineres do banco de dados (exceto por contêineres com RUs atribuídas de maneira exclusiva).

Para dimensionar a taxa de transferência de maneira elástica, você pode aumentar ou diminuir as RU/s provisionadas a qualquer momento. Para saber mais, confira [Como provisionar a taxa de transferência](set-throughput.md) e dimensionar elasticamente bancos de dados e contêineres do Cosmos. Para dimensionar a taxa de transferência globalmente, você pode adicionar ou remover regiões de sua conta do Cosmos a qualquer momento. Para saber mais, confira [Como adicionar ou remover regiões de sua conta do Cosmos](how-to-manage-database-account.md#addremove-regions-from-your-database-account). Em muitos cenários, é importante associar várias regiões a uma conta do Cosmos para conseguir baixa latência e [alta disponibilidade](high-availability.md) no mundo inteiro.

## <a name="how-provisioned-throughput-is-distributed-across-regions"></a>como a taxa de transferência provisionada é distribuída entre regiões

Se você provisionar RUs de ‘R’ em um contêiner (ou banco de dados) do Cosmos, o Cosmos DB garantirá que as RUs de R estejam disponíveis em *cada* região associada à sua conta do Cosmos. Sempre que você adicionar uma nova região à sua conta, o Cosmos DB provisionará automaticamente RUs de ‘R’ na região recém-adicionada. As operações executadas em seu contêiner do Cosmos são garantidas para obter o RUs 'R' em cada região. Você não pode atribuir seletivamente RUs a uma região específica. As RUs provisionadas para um contêiner (ou banco de dados) do Cosmos são provisionadas para todas as regiões associadas à sua conta do Cosmos.

Supondo que um contêiner do Cosmos esteja configurado com RUs de ‘R’ e que haja ‘N’ regiões associadas à conta do Cosmos, então:

- Se a conta do Cosmos estiver configurada com apenas uma região de gravação, o total de RUs disponíveis globalmente no contêiner será R x N.

- Se a conta do Cosmos estiver configurada com várias regiões de gravação, o total de RUs disponíveis globalmente no contêiner será R x (N + 1). As RUs de R adicionais serão provisionadas automaticamente para processar conflitos de atualização e tráfego antientropia nas regiões.

Sua opção de [modelo de consistência](consistency-levels.md) também afeta a taxa de transferência. Você pode obter uma taxa de transferência de leitura aproximadamente duas vezes maior por sessão, prefixo consistente e coerência eventual em comparação com a desatualização limitada ou a coerência forte.

## <a name="next-steps"></a>Próximas etapas

Em seguida, você pode aprender a configurar a taxa de transferência com ajuda do seguinte artigo:

* [Obter e definir a taxa de transferência para contêineres e bancos de dados](set-throughput.md) 

