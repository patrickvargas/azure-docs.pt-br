---
title: PowerShell - Remover um protetor de TDE - Banco de Dados SQL do Microsoft Azure| Microsoft Docs
description: Guia de instruções para responder a um protetor de TDE potencialmente comprometido de um Data Warehouse ou Banco de Dados SQL do Azure usando TDE com o suporte BYOK (Bring Your Own Key).
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: aliceku
ms.author: aliceku
ms.reviewer: vanto
manager: craigg
ms.date: 08/07/2017
ms.openlocfilehash: 6675a68222e09be9a092ad21ee318a53a0a39ca5
ms.sourcegitcommit: 3a02e0e8759ab3835d7c58479a05d7907a719d9c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/13/2018
ms.locfileid: "49311296"
---
# <a name="remove-a-transparent-data-encryption-tde-protector-using-powershell"></a>Remover um protetor de TDE (Transparent Data Encryption) usando PowerShell
## <a name="prerequisites"></a>Pré-requisitos
- É necessário ter uma assinatura do Azure e ser um administrador nessa assinatura
- É necessário ter o Azure PowerShell versão 4.2.0 ou mais recente instalado e em execução. 
- Este guia de instruções assume que você já esteja usando uma chave do Azure Key Vault como protetor de TDE para um Data Warehouse ou Banco de Dados SQL do Azure. Consulte [Transparent Data Encryption com suporte de BYOK](transparent-data-encryption-byok-azure-sql.md) para saber mais.

## <a name="overview"></a>Visão geral
Este guia de instruções descreve como responder a um protetor de TDE potencialmente comprometido para um Data Warehouse ou Banco de Dados SQL do Azure que esteja usando o TDE com suporte BYOKY (Bring Your Own Key). Para saber mais sobre o suporte BYOK para TDE, consulte a [página de visão geral](transparent-data-encryption-byok-azure-sql.md). 

Os procedimentos a seguir devem ser feitos apenas em casos extremos ou em ambientes de teste. Revise cuidadosamente o guia de instruções, pois a exclusão de protetores de TDE ativamente usados do Azure Key Vault pode resultar na **perda de dados**. 

Se houver a suspeita de que uma chave está comprometida, de modo que um serviço ou usuário tenha acesso não autorizado à chave, é melhor excluir a chave.

Lembre-se de que, depois que o protetor de TDE for excluído no Key Vault, **todas as conexões com os bancos de dados criptografados no servidor serão bloqueadas e esses bancos de dados ficarão offline e serão removidos em 24 horas**. Backups antigos criptografados com a chave comprometida não serão mais acessíveis.

Este guia de instruções abrange duas abordagens, dependendo do resultado desejado após a resposta ao incidente:
- Para manter os bancos de dados SQL/Data Warehouses do Azure **acessíveis**
- Para tornar os bancos de dados SQL/Data Warehouses do Azure **inacessíveis**

## <a name="to-keep-the-encrypted-resources-accessible"></a>Para manter os recursos criptografados acessíveis
1. Crie uma [nova chave no Key Vault](https://docs.microsoft.com/powershell/module/azurerm.keyvault/add-azurekeyvaultkey?view=azurermps-4.1.0). Certifique-se de que essa nova chave seja criada em um cofre de chaves separado do protetor de TDE potencialmente comprometido, já que o controle de acesso é provisionado em um nível de segurança. 
2. Adicione a nova chave ao servidor usando os cmdlets [Add-AzureRmSqlServerKeyVaultKey](/powershell/module/azurerm.sql/add-azurermsqlserverkeyvaultkey) e [Set-AzureRmSqlServerTransparentDataEncryptionProtector](/powershell/module/azurerm.sql/set-azurermsqlservertransparentdataencryptionprotector) e atualize-o como o novo protetor de TDE do servidor.

   ```powershell
   # Add the key from Key Vault to the server  
   Add-AzureRmSqlServerKeyVaultKey `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> `
   -KeyId <KeyVaultKeyId>
   
   # Set the key as the TDE protector for all resources under the server
   Set-AzureRmSqlServerTransparentDataEncryptionProtector `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> `
   -Type AzureKeyVault -KeyId <KeyVaultKeyId> 
   ```

3. Certifique-se de que o servidor e todas as réplicas foram atualizadas para o novo protetor TDE usando o cmdlet [Get-AzureRmSqlServerTransparentDataEncryptionProtector](/powershell/module/azurerm.sql/get-azurermsqlservertransparentdataencryptionprotector). 

   >[!NOTE]
   > Pode demorar alguns minutos para o novo protetor de TDE se propagar em todos os bancos de dados e bancos de dados secundários no servidor.
   >

   ```powershell
   Get-AzureRmSqlServerTransparentDataEncryptionProtector `
   -ServerName <LogicalServerName> `
   -ResourceGroupName <SQLDatabaseResourceGroupName>
   ```

4. Faça um [backup da nova chave](/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey) no Key Vault.

   ```powershell
   <# -OutputFile parameter is optional; 
   if removed, a file name is automatically generated. #>
   Backup-AzureKeyVaultKey `
   -VaultName <KeyVaultName> `
   -Name <KeyVaultKeyName> `
   -OutputFile <DesiredBackupFilePath>
   ```
 
5. Exclua a chave comprometida do Key Vault usando o cmdlet [Remove-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/remove-azurekeyvaultkey). 

   ```powershell
   Remove-AzureKeyVaultKey `
   -VaultName <KeyVaultName> `
   -Name <KeyVaultKeyName>
   ```
 
6. Para restaurar uma chave no Key Vault futuramente usando o cmdlet [Restore-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey):
   ```powershell
   Restore-AzureKeyVaultKey `
   -VaultName <KeyVaultName> `
   -InputFile <BackupFilePath>
   ```
 
## <a name="to-make-the-encrypted-resources-inaccessible"></a>Para tornar os recursos criptografados inacessíveis
1. Remova os bancos de dados que estão sendo criptografados pela chave potencialmente comprometida.
O backup do banco de dados e dos arquivos de log é feito automaticamente, portanto, uma recuperação pontual do banco de dados poderá ser realizada a qualquer momento (contanto que você forneça a chave). Os bancos de dados deverão ser removidos antes da exclusão de um protetor de TDE ativo para evitar uma possível perda de dados de até 10 minutos das transações mais recentes. 
2. Faça backup do material da chave do protetor de TDE no Key Vault.
3. Remover a chave potencialmente comprometida do Key Vault

## <a name="next-steps"></a>Próximas etapas

- Saiba como girar o protetor de TDE de um servidor para atender aos requisitos de segurança: [Girar o Protetor de TDE usando PowerShell](transparent-data-encryption-byok-azure-sql-key-rotation.md)

- Introdução ao suporte Bring Your Own Key para TDE: [Ativar o TDE com sua própria chave do Key Vault usando PowerShell](transparent-data-encryption-byok-azure-sql-configure.md)
