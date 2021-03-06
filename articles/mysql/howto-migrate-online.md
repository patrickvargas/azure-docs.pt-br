---
title: Migração com tempo de inatividade mínimo para o Banco de Dados do Azure para MySQL
description: Este artigo descreve como realizar uma migração de tempo de inatividade mínimo de um banco de dados MySQL para o Banco de Dados do Azure para MySQL usando o Serviço de Migração de Banco de Dados do Azure.
services: mysql
author: HJToland3
ms.author: jtoland
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.topic: article
ms.date: 10/11/2018
ms.openlocfilehash: cb2e1a34748875de9c7f8dd59be27c408ec82818
ms.sourcegitcommit: 4047b262cf2a1441a7ae82f8ac7a80ec148c40c4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/11/2018
ms.locfileid: "49090324"
---
# <a name="minimal-downtime-migration-to-azure-database-for-mysql"></a>Migração com tempo de inatividade mínimo para o Banco de Dados do Azure para MySQL
É possível realizar migrações do MySQL para o Banco de Dados do Azure para MySQL com tempo de inatividade mínimo usando o **recurso de sincronização contínua** introduzido recentemente para [DMS](https://aka.ms/get-dms) (Serviço de Migração de Banco de Dados do Azure). Essa funcionalidade limita o tempo de inatividade incorrido pelo aplicativo.

## <a name="overview"></a>Visão geral
O DMS executa uma carga inicial do local no Banco de Dados do Azure para MySQL e, em seguida, sincroniza continuamente quaisquer novas transações no Azure enquanto o aplicativo permanece em execução. Depois que os dados são atualizados no lado do Azure de destino, você interrompe o aplicativo por um breve momento (tempo de inatividade mínimo), aguarde o último lote de dados (desde o momento em que você para o aplicativo até que o aplicativo esteja efetivamente indisponível para receber qualquer novo tráfego) para atualizar no destino e, em seguida, atualize a cadeia de conexão para apontar para o Azure. Quando você terminar, o aplicativo estará ativo no Azure!

![Sincronização contínua com o Serviço de Migração de Banco de Dados do Azure](./media/howto-migrate-online/ContinuousSync.png)

- A migração de fontes MySQL do DMS atualmente está em versão prévia pública. Se você quiser experimentar o serviço para migrar suas cargas de trabalho do MySQL, vá em frente no portal. Seu comentários são imprescindíveis para ajudar a melhorar ainda mais o serviço.

## <a name="next-steps"></a>Próximas etapas
- Exibir o vídeo [Migrar facilmente aplicativos MySQL/PostgreSQL para o serviço gerenciado do Azure](https://medius.studios.ms/Embed/Video/THR2201?sid=THR2201), que contém uma demonstração mostrando como migrar aplicativos MySQL para o Banco de Dados do Azure para MySQL.
- Confira o tutorial [Migrar o MySQL para o Banco de Dados do Azure para MySQL online usando o DMS](https://docs.microsoft.com/azure/dms/tutorial-mysql-azure-mysql-online).
