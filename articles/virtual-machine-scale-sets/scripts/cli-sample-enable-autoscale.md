---
title: Exemplos da CLI do Azure – habilitar o dimensionamento automático baseado em host | Microsoft Docs
description: Exemplos de CLI do Azure
services: virtual-machine-scale-sets
documentationcenter: ''
author: zr-msft
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/27/2018
ms.author: zarhoads
ms.custom: mvc
ms.openlocfilehash: 1172d9a3a2c36d41bc081454936ef8b570d99aba
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/19/2018
ms.locfileid: "49465877"
---
# <a name="automatically-scale-a-virtual-machine-scale-set-with-the-azure-cli"></a>Dimensionamento automático de um conjunto de dimensionamento de máquinas virtuais com a CLI do Azure
Esse script cria um conjunto de dimensionamento de máquinas virtuais executando o Ubuntu e usa métricas baseadas em host para dimensionar automaticamente conforme a carga da CPU muda.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Script de exemplo
[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine-scale-sets/auto-scale-host-metrics/auto-scale-host-metrics.sh "Automatically scale a virtual machine scale set")]

## <a name="clean-up-deployment"></a>Limpar a implantação
Execute o comando a seguir para remover o grupo de recursos, o conjunto de dimensionamento e todos os recursos relacionados.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Explicação sobre o script
Este script usa os comandos a seguir para criar um grupo de recursos, um conjunto de dimensionamento de máquinas virtuais e todos os recursos relacionados. Cada comando da tabela é vinculado à documentação específica do comando.

| Comando | Observações |
|---|---|
| [az group create](/cli/azure/ad/group#az_ad_group_create) | Cria um grupo de recursos no qual todos os recursos são armazenados. |
| [az vmss create](/cli/azure/vmss#az_vmss_create) | Cria um conjunto de dimensionamento de máquinas virtuais e a conecta à rede virtual, à sub-rede e ao grupo de segurança de rede. Um balanceador de carga também é criado para distribuir o tráfego para as diversas instâncias de VM. Esse comando também especifica a imagem da VM a ser usada e as credenciais administrativas.  |
| [az monitor autoscale-settings create](/cli/azure/monitor/autoscale-settings#az_monitor_autoscale_settings_create) | Cria e aplica as regras de dimensionamento automático para um conjunto de dimensionamento de máquinas virtuais. |
| [az group delete](/cli/azure/ad/group#delete) | Exclui um grupo de recursos, incluindo todos os recursos aninhados. |

## <a name="next-steps"></a>Próximas etapas
Para saber mais sobre a CLI do Azure, veja a [documentação da CLI do Azure](https://docs.microsoft.com/cli/azure/overview).

Outros exemplos de script da CLI do Azure de conjunto de dimensionamento de máquinas virtuais podem ser encontrados na [documentação do conjunto de dimensionamento de máquinas virtuais do Azure](../cli-samples.md).