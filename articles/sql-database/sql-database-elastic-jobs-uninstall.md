---
title: Veja como desinstalar a ferramenta de trabalho de banco de dados elástico
description: Saiba como desinstalar os componentes de trabalhos do banco de dados Elástico usando o portal do Azure do PowerShell.
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 06/14/2018
ms.openlocfilehash: 15313bf0d2d152a8295fb831e62452f8ad63bbeb
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/30/2018
ms.locfileid: "50242442"
---
# <a name="uninstall-elastic-database-jobs-components"></a>Desinstalar componentes de trabalhos de banco de dados elástico


[!INCLUDE [elastic-database-jobs-deprecation](../../includes/sql-database-elastic-jobs-deprecate.md)]


Componentes de **trabalhos do Banco de Dados Elástico** podem ser desinstalados usando o portal do Azure ou o PowerShell.

## <a name="uninstall-elastic-database-jobs-components-using-the-azure-portal"></a>Desinstalar componentes de trabalhos de banco de dados elástico usando o Portal do Azure
1. Abra o [Portal do Azure](https://portal.azure.com/).
2. Navegue até a assinatura que contém os componentes de **trabalhos do banco de dados elástico** , ou seja, a assinatura na qual os componentes do banco de dados elástico foram instalados.
3. Clique em **Procurar** e clique em **Grupos de recursos**.
4. Selecione o grupo de recursos chamado "__ElasticDatabaseJob".
5. Exclua o grupo de recursos.

## <a name="uninstall--elastic-database-jobs-components-using-powershell"></a>Desinstalar componentes de trabalhos de banco de dados elástico usando o PowerShell
1. Inicie uma janela de comando do Microsoft Azure PowerShell e navegue até o subdiretório ferramentas na pasta Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x: digite **cd tools**.
   
     PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*>cd tools
2. Execute o script do PowerShell .\UninstallElasticDatabaseJobs.ps1.
   
     PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>Unblock-File .\UninstallElasticDatabaseJobs.ps1   PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>.\UninstallElasticDatabaseJobs.ps1

Ou simplesmente execute o script a seguir, presumindo valores padrão em que os valores são usados na instalação dos componentes:

        $ResourceGroupName = "__ElasticDatabaseJob"
        Switch-AzureMode AzureResourceManager

        $resourceGroup = Get-AzureResourceGroup -Name $ResourceGroupName
        if(!$resourceGroup)
        {
            Write-Host "The Azure Resource Group: $ResourceGroupName has already been deleted.  Elastic database job components are uninstalled."
            return
        }

        Write-Host "Removing the Azure Resource Group: $ResourceGroupName.  This may take a few minutes.”
        Remove-AzureResourceGroup -Name $ResourceGroupName -Force
        Write-Host "Completed removing the Azure Resource Group: $ResourceGroupName.  Elastic database job compoennts are now uninstalled."

## <a name="next-steps"></a>Próximas etapas
Para reinstalar trabalhos de banco de dados elástico, confira [Instalando o serviço de trabalho de banco de dados elástico](sql-database-elastic-jobs-service-installation.md)

Para uma visão geral de trabalhos de banco de dados elástico, consulte [Visão geral de trabalhos de banco de dados elástico](sql-database-elastic-jobs-overview.md).

<!--Image references-->


