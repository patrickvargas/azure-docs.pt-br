---
title: A inicialização da VM está parada em "Windows Preparando. Não desligue o computador." no Azure | Microsoft Docs
description: Apresenta as etapas para solucionar o problema no qual a inicialização da VM fica paralisada em "Preparando o Windows. Não desligue o computador".
services: virtual-machines-windows
documentationcenter: ''
author: Deland-Han
manager: willchen
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 09/18/2018
ms.author: delhan
ms.openlocfilehash: 2bcdb2b458327a5e87dc36e4a5f50f0ac46bf96a
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/14/2018
ms.locfileid: "51621014"
---
# <a name="vm-startup-is-stuck-on-getting-windows-ready-dont-turn-off-your-computer-in-azure"></a>A inicialização da VM está parada em "Windows Preparando. Não desligue o computador." no Azure

Este artigo ajuda você a resolver o problema quando a VM (máquina virtual) fica paralisada no estágio "Preparando o Windows. Não desligue o computador." durante a inicialização.

## <a name="symptoms"></a>Sintomas

Ao usar o **Diagnóstico de inicialização** para obter a captura de tela de uma VM, você descobre que o sistema operacional não foi inicializado completamente. Além disso, a VM exibe uma mensagem **"Preparando o Windows. Não desligue o computador."** mensagem.

![Exemplo de mensagem](./media/troubleshoot-vm-configure-update-boot/message1.png)

![Exemplo de mensagem](./media/troubleshoot-vm-configure-update-boot/message2.png)

## <a name="cause"></a>Causa

Normalmente, esse problema ocorre quando o servidor está fazendo a reinicialização final depois que a configuração foi alterada. A alteração na configuração pode ter sido iniciada por atualizações do Windows ou alterações nos recursos ou nas funções do servidor. Para o Windows Update, quando o tamanho das atualizações é grande, o sistema operacional precisa de mais tempo para reconfigurar as alterações.

## <a name="back-up-the-os-disk"></a>Fazer backup do disco do sistema operacional

Antes de tentar corrigir o problema, faça backup do disco do sistema operacional:

### <a name="for-vms-with-an-encrypted-disk-you-must-unlock-the-disks-first"></a>Para VMs com disco criptografado, você precisa desbloquear os discos primeiro

Valide se a VM é uma VM criptografada. Para fazer isso, siga estas etapas:

1. No portal, abra a VM e, em seguida, navegue até os discos.

2. Você verá uma coluna chamada "Criptografia", que informa se a criptografia está habilitada.

Se o disco do sistema operacional estiver criptografado, desbloqueie o disco criptografado. Para fazer isso, siga estas etapas:

1. Crie uma VM de recuperação localizada no mesmo grupo de recursos, conta de armazenamento e local que a VM afetada.

2. No portal do Azure, exclua a VM afetada e mantenha o disco.

3. Execute o PowerShell como administrador.

4. Execute o seguinte cmdlet para obter o nome do segredo.

    ```Powershell
    Login-AzureRmAccount
 
    $vmName = “VirtualMachineName”
    $vault = “AzureKeyVaultName”
 
    # Get the Secret for the C drive from Azure Key Vault
    Get-AzureKeyVaultSecret -VaultName $vault | where {($_.Tags.MachineName -eq $vmName) -and ($_.Tags.VolumeLetter -eq “C:\”) -and ($_.ContentType -eq ‘BEK‘)}

    # OR Use the below command to get BEK keys for all the Volumes
    Get-AzureKeyVaultSecret -VaultName $vault | where {($_.Tags.MachineName -eq   $vmName) -and ($_.ContentType -eq ‘BEK’)}
    ```

5. Depois de obter o nome do segredo, execute os seguintes comandos no PowerShell:

    ```Powershell
    $secretName = 'SecretName'
    $keyVaultSecret = Get-AzureKeyVaultSecret -VaultName $vault -Name $secretname
    $bekSecretBase64 = $keyVaultSecret.SecretValueText
    ```

6. Converta o valor codificado em Base64 para bytes e grave a saída em um arquivo. 

    > [!Note]
    > O nome do arquivo BEK precisará corresponder ao GUID BEK original se você usar a opção de desbloqueio de USB. Além disso, você precisará criar uma pasta na unidade C, chamada "BEK" para que as etapas a seguir funcionem.
    
    ```Powershell
    New-Item -ItemType directory -Path C:\BEK
    $bekFileBytes = [Convert]::FromBase64String($bekSecretbase64)
    $path = “c:\BEK\$secretName.BEK”
    [System.IO.File]::WriteAllBytes($path,$bekFileBytes)
    ```

7. Depois que o arquivo BEK for criado no computador, copie o arquivo para a VM de recuperação à qual o disco do sistema operacional bloqueado está anexado. Execute o seguinte usando o local do arquivo BEK:

    ```Powershell
    manage-bde -status F:
    manage-bde -unlock F: -rk C:\BEKFILENAME.BEK
    ```
    **Opcional** em alguns cenários talvez seja necessário também descriptografar o disco com esse comando.
   
    ```Powershell
    manage-bde -off F:
    ```

    > [!Note]
    > Estamos considerando que o disco a ser criptografado está na letra F:.

8. Se você precisar coletar logs, navegue até o caminho **LETRA DA UNIDADE: \Windows\System32\winevt\Logs**.

9. Desanexe a unidade da máquina de recuperação.

### <a name="create-a-snapshot"></a>Criar um instantâneo

Para criar um instantâneo, siga as etapas em [Criar instantâneo de um disco](..\windows\snapshot-copy-managed-disk.md).

## <a name="collect-an-os-memory-dump"></a>Coletar um despejo de memória do sistema operacional

Siga as etapas na seção [Coletar despejo do sistema operacional](troubleshoot-common-blue-screen-error.md#collect-memory-dump-file) para coletar um despejo do sistema operacional quando a VM está paralisada na configuração.

## <a name="contact-microsoft-support"></a>Contatar Suporte da Microsoft

Depois de coletar o arquivo de despejo, entre em contato com [o suporte da Microsoft](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para executar a análise de causa raiz.


## <a name="rebuild-the-vm-using-powershell"></a>Recompilar a VM usando o PowerShell

Depois de coletar o arquivo de despejo de memória, siga as etapas a seguir para recompilar a VM.

**Para discos não gerenciados**

```PowerShell
# To login to Azure Resource Manager
Login-AzureRmAccount

# To view all subscriptions for your account
Get-AzureRmSubscription

# To select a default subscription for your current session
Get-AzureRmSubscription –SubscriptionID “SubscriptionID” | Select-AzureRmSubscription

$rgname = "RGname"
$loc = "Location"
$vmsize = "VmSize"
$vmname = "VmName"
$vm = New-AzureRmVMConfig -VMName $vmname -VMSize $vmsize;

$nic = Get-AzureRmNetworkInterface -Name ("NicName") -ResourceGroupName $rgname;
$nicId = $nic.Id;

$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nicId;

$osDiskName = "OSdiskName"
$osDiskVhdUri = "OSdiskURI"

$vm = Set-AzureRmVMOSDisk -VM $vm -VhdUri $osDiskVhdUri -name $osDiskName -CreateOption attach -Windows

New-AzureRmVM -ResourceGroupName $rgname -Location $loc -VM $vm -Verbose
```

**Para discos gerenciados**

```PowerShell
# To login to Azure Resource Manager
Login-AzureRmAccount

# To view all subscriptions for your account
Get-AzureRmSubscription

# To select a default subscription for your current session
Get-AzureRmSubscription –SubscriptionID "SubscriptionID" | Select-AzureRmSubscription

#Fill in all variables
$subid = "SubscriptionID"
$rgName = "ResourceGroupName";
$loc = "Location";
$vmSize = "VmSize";
$vmName = "VmName";
$nic1Name = "FirstNetworkInterfaceName";
#$nic2Name = "SecondNetworkInterfaceName";
$avName = "AvailabilitySetName";
$osDiskName = "OsDiskName";
$DataDiskName = "DataDiskName"

#This can be found by selecting the Managed Disks you wish you use in the Azure Portal if the format below doesn't match
$osDiskResourceId = "/subscriptions/$subid/resourceGroups/$rgname/providers/Microsoft.Compute/disks/$osDiskName";
$dataDiskResourceId = "/subscriptions/$subid/resourceGroups/$rgname/providers/Microsoft.Compute/disks/$DataDiskName";

$vm = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize;

#Uncomment to add Availability Set
#$avSet = Get-AzureRmAvailabilitySet –Name $avName –ResourceGroupName $rgName;
#$vm = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize -AvailabilitySetId $avSet.Id;

#Get NIC Resource Id and add
$nic1 = Get-AzureRmNetworkInterface -Name $nic1Name -ResourceGroupName $rgName;
$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic1.Id -Primary;

#Uncomment to add a secondary NIC
#$nic2 = Get-AzureRmNetworkInterface -Name $nic2Name -ResourceGroupName $rgName;
#$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic2.Id;

#Windows VM
$vm = Set-AzureRmVMOSDisk -VM $vm -ManagedDiskId $osDiskResourceId -name $osDiskName -CreateOption Attach -Windows;

#Linux VM
#$vm = Set-AzureRmVMOSDisk -VM $vm -ManagedDiskId $osDiskResourceId -name $osDiskName -CreateOption Attach -Linux;

#Uncomment to add additional Data Disk
#Add-AzureRmVMDataDisk -VM $vm -ManagedDiskId $dataDiskResourceId -Name $dataDiskName -Caching None -DiskSizeInGB 1024 -Lun 0 -CreateOption Attach;

New-AzureRmVM -ResourceGroupName $rgName -Location $loc -VM $vm;
```
