---
title: Entenda como o desconto de reserva é aplicado ao Azure Cosmos DB | Microsoft Docs
description: Saiba como o desconto de reserva é aplicado à taxa de transferência provisionada (RU/s) no Azure Cosmos DB.
services: cosmos-db
author: rimman
manager: kfile
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: cwatson
ms.reviewer: sngun
ms.openlocfilehash: 5b15b5f8188f2077b3e9cb17ab3794e881a4deb3
ms.sourcegitcommit: 8e06d67ea248340a83341f920881092fd2a4163c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/16/2018
ms.locfileid: "49353418"
---
# <a name="understand-how-the-reservation-discount-is-applied-to-azure-cosmos-db"></a>Entenda como o desconto de reserva é aplicado ao Azure Cosmos DB

Depois de adquirir uma capacidade reservada do Azure Cosmos DB, o desconto de reserva é aplicado automaticamente aos recursos do Banco de Dados do Azure Cosmos que correspondem aos atributos e à quantidade da reserva. Uma reserva cobre a taxa de transferência provisionada para recursos do Azure Cosmos DB. Ela não cobre software, rede, armazenamento nem encargos de contêiner predefinidos.

## <a name="reservation-discount-applied-to-azure-cosmos-db-accounts"></a>Desconto de reserva aplicado às contas do Azure Cosmos DB

Um desconto de reserva é aplicado a [taxa de transferência provisionada](../cosmos-db/request-units.md) em termos de unidades de solicitação por segundo (RU/s) em uma base de hora a hora. Para recursos do BD do Azure Cosmos que não são executados durante toda a hora, o desconto de reserva é aplicado automaticamente a outros recursos do BD Cosmos que correspondem aos atributos de reserva. O desconto pode se aplicar aos recursos do BD do Azure Cosmos que estão sendo executados simultaneamente. Se você não tiver recursos do BD Cosmos que sejam executados durante a hora inteira e que correspondam aos atributos da reserva, você não terá o benefício total do desconto da reserva para essa hora.

Os descontos são organizados em camadas. As reservas com as unidades de solicitação mais altas oferecem descontos maiores. 

A compra da reserva aplicará descontos a todas as regiões na proporção equivalente ao preço sob demanda regional. Para obter as taxas de desconto de reserva em cada região, confira a seção [Desconto de reserva por região](#reservation-discount-per-region) deste artigo.

## <a name="reservation-discount-per-region"></a>Desconto de reserva por região
O desconto de reserva é aplicado aos custos de taxa de transferência do Azure Cosmos DB hora a hora. Ele é aplicado ao escopo da assinatura única ou da conta/registrado. O desconto de reserva se aplica ao uso do medidor em diferentes regiões nas seguintes proporções:

|Descrição de medidor  |Região |Proporção  |
|---------|---------|---------|
|Azure Cosmos DB - 100 RU / s / hora - AP Sudeste  |  Sudeste do Pacífico Asiático    |   1      |
|Azure Cosmos DB – 100 RU/s/hora – Leste do Pacífico Asiático |   Leste do Pacífico Asiático   |    1     |
|Centro-Norte da Europa de - 100 RU/s/hora – Azure Cosmos DB|  Norte da Europa       |    1     |
|Cos do Azure DB - 100 RU / s / Hora - KR Sul|    Sul da Coreia     |     1    |
|Azure Cosmos DB – 100 RU/s/hora – da Europa Ocidental|    Oeste da Europa     |      1   |
|Azure Cosmos DB – 100 RU/s/hora – Coreia Central|   Coreia Central    |       1  |
|Azure Cosmos DB – 100 RU/s/hora - UK Sul|   Sul do Reino Unido      |     1    |
|Oeste do Reino Unido 100 RU/s/hora - do BD Cosmos do Azure|   Oeste do Reino Unido      |    1     |
|Norte do Reino Unido 100 RU/s/hora - do BD Cosmos do Azure |   Norte do Reino Unido    |     1    |
|Azure Cosmos DB – 100 RU/s/hora - UK Sul 2|   Sul do Reino Unido 2      |     1    |
|Azure Cosmos DB – 100 RU/s/hora – Leste dos EUA 2|  Leste dos EUA 2     |     1    |
|Azure Cosmos DB – 100 RU/s/hora – Centro-Norte dos EUA|   Centro-Norte dos EUA      |     1    |
|Azure Cosmos DB – 100 RU/s/hora - Oeste dos EUA|   Oeste dos EUA      |     1    |
|Azure Cosmos DB – 100 RU/s/hora – centro dos EUA| Centro dos EUA        |     1    |
|Azure Cosmos DB – 100 RU/s/hora - Oeste dos EUA 2|   Oeste dos EUA 2      |      1   |
|Azure Cosmos DB – 100 RU/s/hora - Oeste dos EUA|   Centro-Oeste dos EUA      |       1  |
|Azure Cosmos DB – 100 RU/s/hora – Leste dos EUA|   Leste dos EUA      |  1       |
|Norte do Azure Cosmos DB – 100 RU/s/hora - SA|     Norte do SA    |   1      |
|Azure Cosmos DB – 100 RU/s/hora - SA Oeste |    Oeste do SA      |    1     |
|Azure Cosmos DB – 100 RU/s/hora – Sul da Índia|    Sul da Índia     |    1.0375    |
|Azure Cosmos DB – 100 RU/s/hora - AC Leste|   Leste do Canadá      |    1,1      |
|Cos do Azure DB - 100 RU / s / Hora - JA Leste|   Leste do Japão      |    1.125     |
|Azure Cosmos DB - 100 RU / s / Hora - JA Oeste|     Oeste do Japão    |   1.125       |
|Azure Cosmos DB – 100 RU/s/hora – Índia Ocidental|     Índia Ocidental    |    1.1375     |
|Azure Cosmos DB – 100 RU/s/hora – Índia Central|    Índia Central     |  1.1375       |
|Azure Cosmos DB – 100 RU/s/hora - AU Leste|     Leste da Austrália    |   1,15       |
|Azure Cosmos DB - 100 RU/s/Hour - CA Central|  CA Central       |   1.2       |
|Azure Cosmos DB – 100 RU/s/hora - França Central|   França Central      |    1,25      |
|Azure Cosmos DB – 100 RU/s/hora – Sul do Brasil|  Sul do Brasil       |   1.5      |
|Azure Cosmos DB – 100 RU/s/hora – Austrália Central|   Austrália Central      |   1.5      |
|Azure Cosmos DB – 100 RU/s/hora – Austrália Central 2| Austrália Central 2        |    1.5     |
|Sul da França de - 100 RU/s/hora – Azure Cosmos DB|    Sul da França     |    1.625     |

## <a name="scenarios-that-show-how-the-reservation-discount-is-applied"></a>Cenários que mostram como o desconto de reserva é aplicado

Considere os seguintes requisitos para uma reserva:

* Taxa de transferência necessária: 50.000 RU/s  
* Regiões usadas: 2 

Neste caso, o total de encargos sob demanda é para uma quantidade de 500 do medidor de 100 RU/s nessas duas regiões. O consumo de RU/s total é de 100.000 a cada hora. 

**Cenário 1**

Por exemplo, suponha que você precise de implantações do Azure Cosmos DB nas regiões Centro-Norte dos EUA e Oeste dos EUA. Cada região tem um consumo de taxa de transferência de 50.000 RU/s. Uma compra de reserva de 100.000 RU/s equilibrará completamente os encargos de sob demanda.

O desconto coberto por uma reserva é calculado como: consumo de taxa de transferência * taxa_de_desconto_de_reserva_da_região. Para as regiões Centro-Norte dos EUA e Oeste dos EUA, a taxa de desconto da reserva é de 1. Portanto, o total de RU/s com desconto é de 100.000. Esse valor é computado como: 50.000 * 1 + 50.000 * 1 = 100.000 RU/s. Você não precisa pagar nenhum encargo adicional com as tarifas pagas conforme o uso normais. 

|Descrição de medidor | Região |Consumo de taxa de transferência (RU/s) |Desconto de reserva aplicado a RU/s |
|---------|---------|---------|---------|
|Azure Cosmos DB – 100 RU/s/hora – Centro-Norte dos EUA  |   Centro-Norte dos EUA  | 50.000  | 50.000  |
|Azure Cosmos DB – 100 RU/s/hora - Oeste dos EUA  |  Oeste dos EUA   |  50.000  |  50.000 |

**Cenário 2**

Por exemplo, suponha que você precise de implantações do Azure Cosmos DB nas regiões da Austrália Central 2 e do Sul da França. Cada região tem um consumo de taxa de transferência de 50.000 RU/s. Uma compra de reserva de 100.000 RU / s seria aplicável da seguinte forma (assumindo que o uso do AU Central 2 foi descontado em primeiro lugar):

|Descrição de medidor | Região |Consumo de taxa de transferência (RU/s) |Desconto de reserva aplicado a RU/s |
|---------|---------|---------|---------|
|Azure Cosmos DB – 100 RU/s/hora – Austrália Central 2  |  Austrália Central 2   |  50.000  |  50.000   |
|Sul da França de - 100 RU/s/hora – Azure Cosmos DB  |  Sul da França   |  50.000 |  15,384  |

Um uso de 50.000 unidades na região "Austrália Central 2" corresponde a 75.000 RU/s de uso faturável (ou uso normalizado). Esse valor é computado como: consumo de taxa de transferência * taxa_de_desconto_de_reserva_da_região. O cálculo é igual a 75.000 RU/s de uso faturável ou normalizado. Esse valor é computado como: 50.000 * 1,5 + 75.000 RU/s.

A compra de reserva de 100.000 RU/s compensaria 75.000 RU/s da Austrália Central 2. Isso deixa 25.000 RU/s para a região do Sul da França. Das restantes 25.000 RU/s restantes, um desconto de reserva de 15.384 RU/s é aplicado à região do Sul da França. O valor de desconto é computado como: 25.000 / 1,625 = 15.384 RU/s. As restantes 34.616 RU/s na região "Sul da França" são cobradas às taxas normais de pagamento conforme o uso. 

O sistema de faturamento do Azure atribuirá o benefício de faturamento de reserva à primeira instância processada que corresponda à configuração de reserva. Por exemplo, é Austrália Central 2 neste caso.

Para entender e visualizar o aplicativo de suas reservas do Azure nos relatórios de uso de faturamento, consulte [Entender o uso de reserva do Azure](../billing/billing-understand-reserved-instance-usage-ea.md).

## <a name="next-steps"></a>Próximas etapas

Para saber mais sobre as reservas do Azure, consulte os seguintes artigos:

* [Quais são as reservas do Azure?](../billing/billing-save-compute-costs-reservations.md)  
* [Pagar antecipadamente por recursos do Azure Cosmos DB com capacidade reservada do Azure Cosmos DB](../cosmos-db/cosmos-db-reserved-capacity.md)  
* [Pagar antecipadamente por recursos de computação de banco de dados SQL com capacidade reservada do Azure SQL Database](../sql-database/sql-database-reserved-capacity.md)  
* [Gerenciar reservas do Azure](../billing/billing-manage-reserved-vm-instance.md)  
* [Entenda o uso de instâncias reservadas para sua assinatura de Pagamento Conforme o Uso](../billing/billing-understand-reserved-instance-usage.md)  
* [Entenda o uso de reservas para o seu registro Enterprise](../billing/billing-understand-reserved-instance-usage-ea.md)  
* [Entender o uso de reserva para assinaturas de CSP](https://docs.microsoft.com/partner-center/azure-reservations)

## <a name="need-help-contact-support"></a>Precisa de ajuda? Contate o suporte

Se ainda tiver dúvidas, [entre em contato com o suporte](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para resolver seu problema rapidamente.

