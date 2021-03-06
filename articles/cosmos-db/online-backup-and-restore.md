---
title: Backup e restauração online com o Azure Cosmos DB | Microsoft Docs
description: Saiba como executar o backup e a restauração automáticos em um banco de dados do Azure Cosmos DB.
keywords: backup e restauração, backup online
services: cosmos-db
author: kanshiG
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 11/15/2017
ms.author: govindk
ms.openlocfilehash: 657b75e5e3bb5c35bb23221235e62298fc797046
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48902664"
---
# <a name="automatic-online-backup-and-restore-with-azure-cosmos-db"></a>Backup e restauração online automáticos com o Azure Cosmos DB
O Azure Cosmos DB faz backup automaticamente de todos os seus dados em intervalos regulares. Os backups automáticos são feitos sem afetar o desempenho nem a disponibilidade das operações de banco de dados. Todos os backups são armazenados separadamente em outro serviço de armazenamento, e esses backups são replicados globalmente para resiliência contra desastres regionais. Os backups automáticos são destinados a cenários em que acidentalmente o contêiner do Cosmos DB é excluído e, posteriormente, requer recuperação de dados.  

Este artigo começa com uma recapitulação rápida da redundância e disponibilidade de dados no Cosmos DB e, em seguida, aborda os backups. 

## <a name="high-availability-with-cosmos-db---a-recap"></a>Alta disponibilidade com o Cosmos DB – recapitulação
O Cosmos DB foi desenvolvido para ser [distribuído globalmente](distribute-data-globally.md) – ele permite que você dimensione a produtividade em várias regiões do Azure, juntamente com failover controlado por política e APIs transparentes de hospedagem múltipla. O Azure Cosmos DB oferece [SLAs de disponibilidade de 99,99%](https://azure.microsoft.com/support/legal/sla/cosmos-db) para todas as contas de única região e todas as contas de várias regiões com consistência amena e 99,999% de disponibilidade de leitura em todas as contas de banco de dados de várias regiões. Todas as gravações no Azure Cosmos DB são permanentemente confirmadas em discos locais por um quorum de réplicas em um data center local antes de serem confirmadas no cliente. A alta disponibilidade do Cosmos DB se baseia no armazenamento local e não depende de nenhuma tecnologia de armazenamento externo. Além disso, se sua conta de banco de dados está associada a mais de uma região do Azure, suas gravações são replicadas em outras regiões também. Para dimensionar seus dados de produtividade e acesso em latências menores, você pode ter quantas regiões de leitura associadas à sua conta de banco de dados quantas quiser. Em cada região de leitura, os dados (replicados) são persistidos em um conjunto de réplicas.  

Conforme ilustrado no diagrama a seguir, um único contêiner do Cosmos DB é [particionado horizontalmente](partition-data.md). Uma "partição" é indicada por um círculo no diagrama a seguir e cada partição é altamente disponibilizada por meio de um conjunto de réplicas. Essa é a distribuição local dentro de uma única região do Azure (indicada pelo eixo X). Além disso, cada partição (com seu conjunto de réplicas correspondente) é distribuída globalmente entre várias regiões associadas à sua conta de banco de dados (por exemplo, neste caso, três regiões: Leste dos EUA, Oeste dos EUA e Índia Central). O "conjunto de partição" é uma entidade distribuída globalmente que consiste em várias cópias de seus dados em cada região (indicado pelo eixo Y). Você pode atribuir prioridade às regiões associadas à conta de banco de dados e o Cosmos DB fará failover de forma transparente para a próxima região em caso de desastre. Você pode simular o failover manualmente para testar a disponibilidade de ponta a ponta de seu aplicativo.  

A imagem a seguir ilustra o alto grau de redundância com o Cosmos DB.

![Alto grau de redundância com o Cosmos DB](./media/online-backup-and-restore/redundancy.png)

![Alto grau de redundância com o Cosmos DB](./media/online-backup-and-restore/global-distribution.png)

## <a name="full-automatic-online-backups"></a>Backups online completos, automáticos
Opa, excluí meu contêiner ou banco de dados! Com o Cosmos DB, não apenas os dados, mas também os backups dos dados ficam altamente redundantes e resilientes em desastres regionais. Esses backups automatizados são feitos atualmente a cada quatro horas, aproximadamente, e os últimos dois backups são armazenados o tempo todo. Se os dados forem acidentalmente descartados ou estiverem corrompidos, [entre em contato com o suporte do Azure](https://azure.microsoft.com/support/options/) em até oito horas. 

Os backups são feitos sem afetar o desempenho ou a disponibilidade de suas operações de banco de dados. O Cosmos DB faz o backup em segundo plano, sem consumir as RUs provisionadas nem afetar o desempenho e sem afetar a disponibilidade do banco de dados. 

Ao contrário dos dados que são armazenados no Cosmos DB, os backups automáticos são armazenados no serviço Armazenamento de Blobs do Azure. Para garantir um upload eficiente e a baixa latência, o instantâneo do backup é carregado em uma instância do Armazenamento de blobs do Azure na mesma região da região de gravação atual de sua conta de banco de dados do Cosmos DB. Para resiliência contra desastres regionais, cada instantâneo dos seus dados de backup no Armazenamento de Blobs do Azure é replicado novamente via GRS (armazenamento com redundância geográfica) para outra região. O diagrama a seguir mostra que o contêiner inteiro do Cosmos DB (com todas as três partições primárias no Oeste dos EUA, neste exemplo) é copiado em backup para uma conta remota de Armazenamento de Blobs do Azure no Oeste dos EUA e, então, replicadas por GRS no Leste dos EUA. 

A imagem a seguir ilustra os backups completos periódicos de todas as entidades do Cosmos DB no Armazenamento do Azure GRS.

![Backups completos periódicos de todas as entidades do Cosmos DB no Armazenamento do Azure GRS](./media/online-backup-and-restore/automatic-backup.png)

## <a name="backup-retention-period"></a>Período de retenção do backup
Conforme descrito acima, o Azure Cosmos DB usa instantâneos dos seus dados a cada quatro horas no nível da partição. A qualquer momento, somente os últimos dois instantâneos são mantidos. No entanto, se o contêiner ou o banco de dados for excluído, o Azure Cosmos DB reterá os instantâneos existentes de todas as partições excluídas dentro desse contêiner ou banco de dados por 30 dias.

Para API de SQL, se você quiser manter seus próprios instantâneos poderá fazer isso usando as seguintes opções:

* Use a opção exportar para JSON na [ferramenta de Migração de Dados](import-data.md#export-to-json-file) do Azure Cosmos DB para agendar backups adicionais.

* Use o [Azure Data Factory](../data-factory/connector-azure-cosmos-db.md) para mover dados periodicamente.

* Use o [feed de alterações](change-feed.md) do Azure Cosmos DB para ler os dados periodicamente para backup completo e separadamente para alterações incrementais e mover para o destino do blob. 

* Para gerenciar backups passivos é possível ler periodicamente os dados do feed de alterações e atrasar a gravação para outra coleção. Isso garantirá que não será necessário restaurar os dados e você poderá examinar imediatamente os dados para verificar se não há problemas. 

> [!NOTE]
> Se você "Provisionar taxa de transferência para um conjunto de contêineres em um nível de Banco de Dados" – Lembre-se que a restauração ocorre no nível de conta de Banco de Dados completo. Se o contêiner for excluído acidentalmente, procure a equipe de suporte dentro de um período de 8 horas. Os dados não podem ser restaurados se você não contatar a equipe de suporte dentro de 8 horas.

## <a name="restoring-a-database-from-an-online-backup"></a>Restauração de um banco de dados de um backup online

Se você excluir o banco de dados ou o contêiner acidentalmente, será possível [criar um tíquete de suporte](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) ou [ligar para o suporte do Azure](https://azure.microsoft.com/support/options/) solicitando a restauração dos dados do último backup automático. O suporte do Azure está disponível para planos selecionados somente como Standard, Developer, o suporte não está disponível para o plano Básico. Para saber mais sobre os diferentes planos de suporte, consulte a página de [planos com suporte do Azure](https://azure.microsoft.com/support/plans/). 

Se você precisar restaurar o banco de dados devido a um problema de dados corrompidos (inclusive casos em que os documentos em um contêiner são excluídos), confira [Tratando dados corrompidos](#handling-data-corruption) para executar etapas adicionais para impedir que os dados corrompidos substituam os backups existentes. Para que um instantâneo específico do backup seja restaurado, o Cosmos DB exige que os dados estejam disponíveis durante o ciclo de backup do instantâneo.

> [!NOTE]
> Coleções ou bancos de dados podem ser restaurados somente em solicitações explícitas do cliente. É de responsabilidade do cliente excluir o contêiner ou o banco de dados imediatamente após a reconciliação dos dados. Se você não excluir os bancos de dados ou coleções restaurados, eles incorrerão em custos para unidades de solicitação, armazenamento e saída.

## <a name="handling-data-corruption"></a>Manipulação de dados corrompidos

O Azure Cosmos DB retém os últimos dois backups de todas as partições na conta do banco de dados. Esse modelo funciona bem quando um contêiner (coleção de documentos, grafo, tabela) ou um banco de dados forem excluído acidentalmente desde que uma das últimas versões pode ser restaurada. No entanto, caso os usuários introduzam um problema de corrupção de dados, o Azure Cosmos DB pode não estar ciente da corrupção de dados, e é possível que a corrupção possa ter substituído os backups existentes. 

Assim que a corrupção for detectada, o usuário deverá excluir o contêiner corrompido (coleção/grafo/tabela) para que os backups sejam protegidos contra a substituição por dados corrompidos. E o mais importante, contate o Suporte da Microsoft e solicite um tíquete com solicitação específica de Gravidade 2. 

A imagem a seguir ilustra a criação de solicitação de suporte para recuperação de contêiner (coleção/gráfico/tabela) via portal do Azure para atualização ou apagamento acidental de dados dentro de um contêiner

![Restaurar um contêiner para uma atualização ou exclusão de dados incorreta no Cosmos DB](./media/online-backup-and-restore/backup-restore-support.png)

Quando a restauração é concluída para esse tipo de cenário, os dados são restaurados para outra conta (com o sufixo “-restaurado”) e contêiner. Esta restauração não é feita no lugar para fornecer uma chance do cliente fazer a validação de dados e mover os dados como quiser. O contêiner restaurado fica na mesma região com as mesmas RUs e políticas de indexação. O usuário que é administrador de assinatura ou coadministrador pode ver essa conta restaurada.


> [!NOTE]
> Se você restaurar os dados para corrigir o problema de corrupção ou apenas para testes, planeje removê-los assim que a tarefa for concluída, pois os contêineres restaurados ou um banco de dados terão custos adicionais - com base na taxa de transferência provisionada. 
## <a name="next-steps"></a>Próximas etapas

Para replicar o banco de dados em vários data centers, consulte [Distribuir os dados globalmente com o Cosmos DB](distribute-data-globally.md). 

Para entrar em contato com o Suporte do Azure, [crie um tíquete no portal do Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

