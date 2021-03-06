---
title: Mover uma VM do Linux no Azure | Microsoft Docs
description: Mova uma VM Linux para outro grupo de recursos ou outra assinatura do Azure no modelo de implantação do Resource Manager.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: d635f0a5-4458-4b95-a5f8-eed4f41eb4d4
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: article
ms.date: 09/12/2018
ms.author: cynthn
ms.openlocfilehash: b521c66581b4b77e5c49c963530b0c81f842f6f0
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/14/2018
ms.locfileid: "45573834"
---
# <a name="move-a-linux-vm-to-another-subscription-or-resource-group"></a>Mover uma VM Linux para outro grupo de recursos ou outra assinatura
Este artigo explica como mover uma máquina virtual (VM) do Linux entre grupos de recursos ou assinaturas. Mover uma VM entre assinaturas poderá ser útil se você tiver criado uma VM em uma assinatura pessoal e agora quiser movê-la para a assinatura da sua empresa.

> [!IMPORTANT]
>Você não pode mover os discos gerenciados do Azure no momento. 
>
>Novas IDs de recurso são criadas como parte da mudança. Depois que a VM for movida, você precisará atualizar suas ferramentas e scripts para usar os novos IDs de recursos. 
> 
> 

## <a name="use-the-azure-cli-to-move-a-vm"></a>Usar a CLI do Azure para mover uma VM


Antes de mover sua VM usando a CLI do Azure, você precisa verificar se as assinaturas de origem e destino existem no mesmo inquilino. Para verificar se as duas assinaturas têm a mesma ID de locatário, use [az account show](/cli/azure/account#az_account_show).

```azurecli-interactive
az account show --subscription mySourceSubscription --query tenantId
az account show --subscription myDestinationSubscription --query tenantId
```
Se as IDs do locatário para as assinaturas de origem e de destino não forem iguais, entre em contato com o [suporte](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) para mover os recursos para um novo locatário.

Para mover uma VM com êxito, você precisa mover a VM e todos os seus recursos de suporte. Use o comando [az resource list](/cli/azure/resource#az_resource_list) para listar todos os recursos em um grupo de recursos e suas IDs. Ele ajuda a direcionar a saída desse comando para um arquivo de modo que seja possível copiar e colar as IDs em comandos posteriores.

```azurecli-interactive
az resource list --resource-group "mySourceResourceGroup" --query "[].{Id:id}" --output table
```

Para mover uma VM e seus recursos para outro grupo de recursos, use [az resource move](/cli/azure/resource#az_resource_move). O exemplo a seguir mostra como mover uma VM e os recursos mais comuns que ela exige. Use o parâmetro **-ids** e passe-o em uma lista de IDs separadas por vírgulas (sem espaços) para os recursos a serem movidos.

```azurecli-interactive
vm=/subscriptions/mySourceSubscriptionID/resourceGroups/mySourceResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM
nic=/subscriptions/mySourceSubscriptionID/resourceGroups/mySourceResourceGroup/providers/Microsoft.Network/networkInterfaces/myNIC
nsg=/subscriptions/mySourceSubscriptionID/resourceGroups/mySourceResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNSG
pip=/subscriptions/mySourceSubscriptionID/resourceGroups/mySourceResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIPAddress
vnet=/subscriptions/mySourceSubscriptionID/resourceGroups/mySourceResourceGroup/providers/Microsoft.Network/virtualNetworks/myVNet
diag=/subscriptions/mySourceSubscriptionID/resourceGroups/mySourceResourceGroup/providers/Microsoft.Storage/storageAccounts/mydiagnosticstorageaccount
storage=/subscriptions/mySourceSubscriptionID/resourceGroups/mySourceResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageacountname    

az resource move \
    --ids $vm,$nic,$nsg,$pip,$vnet,$storage,$diag \
    --destination-group "myDestinationResourceGroup"
```

Se quiser mover a VM e seus recursos para outra assinatura, adicione o parâmetro **--destination-subscriptionId** para especificar a assinatura de destino.

Quando for solicitado que você confirme que deseja mover os recursos especificados, insira **Y** para confirmar.

[!INCLUDE [virtual-machines-common-move-vm](../../../includes/virtual-machines-common-move-vm.md)]

## <a name="next-steps"></a>Próximas etapas
Você pode mover vários tipos diferentes de recursos entre grupos de recursos e assinaturas. Para obter mais informações, consulte [Mover recursos para um novo grupo de recursos ou assinatura](../../resource-group-move-resources.md).    

