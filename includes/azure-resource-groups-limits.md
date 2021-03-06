---
author: rothja
ms.service: billing
ms.topic: include
ms.date: 11/09/2018
ms.author: jroth
ms.openlocfilehash: 5c2c7b621512be7b81d14b99069d52f4f3aa3f33
ms.sourcegitcommit: 8d88a025090e5087b9d0ab390b1207977ef4ff7c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/21/2018
ms.locfileid: "52279413"
---
| Recurso | Limite padrão | Limite máximo |
| --- | --- | --- |
| Recursos por [grupo de recursos](../articles/azure-resource-manager/resource-group-overview.md#resource-groups) (por tipo de recurso) |800 |Varia por tipo de recurso |
| Implantações por grupo de recursos no histórico de implantações |800 |800 |
| Recursos por implantação |800 |800 |
| Bloqueios de Gerenciamento (por escopo exclusivo) |20 |20 |
| Número de marcas (por recurso ou grupo de recursos) |15 |15 |
| Comprimento da chave de marca |512 |512 |
| Comprimento do valor da marca |256 |256 |


#### <a name="template-limits"></a>Limites de modelo

| Valor | Limite padrão | Limite máximo |
| --- | --- | --- |
| parâmetros |256 |256 |
| variáveis |256 |256 |
| Recursos (incluindo a contagem de cópias) |800 |800 |
| outputs |64 |64 |
| Expressão de modelo |24.576 caracteres |24.576 caracteres |
| Recursos em modelos exportados |200 |200 | 
| Tamanho do modelo |1 MB |1 MB |
| Tamanho do arquivo de parâmetro |64 KB |64 KB |

Você pode exceder alguns limites de modelo usando um modelo aninhado. Para saber mais, confira [Uso de modelos vinculados ao implantar recursos do Azure](../articles/azure-resource-manager/resource-group-linked-templates.md). Para reduzir o número de parâmetros, variáveis ou saídas, você pode combinar vários valores em um objeto. Para saber mais, veja [Objetos como parâmetros](../articles/azure-resource-manager/resource-manager-objects-as-parameters.md).

Caso você atinja o limite de 800 implantações por grupo de recursos, exclua do histórico as implantações que não são mais necessárias. Exclua entradas do histórico com [az group deployment delete](/cli/azure/group/deployment#az_group_deployment_delete) para a CLI do Azure ou [Remove-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/remove-azurermresourcegroupdeployment) no PowerShell. A exclusão de uma entrada do histórico de implantações não afeta os recursos de implantação. 