---
title: Criar políticas por meio de programação e exibir dados de conformidade com o Azure Policy
description: Este artigo orienta você na criação e gerenciamento por meio de programação de políticas no Azure Policy.
services: azure-policy
author: DCtheGeek
ms.author: dacoulte
ms.date: 10/30/2018
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.openlocfilehash: d72c9c1747bb697f66fa53489636b1726053060c
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/30/2018
ms.locfileid: "50242614"
---
# <a name="programmatically-create-policies-and-view-compliance-data"></a>Criar políticas por meio de programação e exibir dados de conformidade

Este artigo orienta você na criação e gerenciamento por meio de programação de políticas. As definições de políticas impõem diferentes regras e efeitos em seus recursos. A imposição garante que os recursos permaneçam em conformidade com seus padrões empresariais e contratos de nível de serviço.

Para obter informações sobre a conformidade, veja [como obter dados de conformidade](getting-compliance-data.md).

## <a name="prerequisites"></a>Pré-requisitos

Antes de começar, verifique se os seguintes pré-requisitos são cumpridos:

1. Caso ainda não tenha feito, instale o [ARMClient](https://github.com/projectkudu/ARMClient). É uma ferramenta que envia solicitações HTTP para APIs baseadas no Azure Resource Manager.

1. Atualize seu módulo do PowerShell do AzureRM para a versão mais recente. Para obter mais informações sobre a versão mais recente, consulte o [Azure PowerShell](https://github.com/Azure/azure-powershell/releases).

1. Registre o provedor de recursos de Informações de Política usando Azure PowerShell para garantir que sua assinatura funcione com o provedor de recursos. Para registrar um provedor de recursos, você deve ter permissão para executar a operação de ação de registro para o provedor de recursos. Esta operação está incluída nas funções de Colaborador e de Proprietário. Execute o seguinte comando para registrar o provedor de recursos:

   ```azurepowershell-interactive
   Register-AzureRmResourceProvider -ProviderNamespace 'Microsoft.PolicyInsights'
   ```

   Para saber mais sobre como registrar e exibir provedores de recursos, consulte [Provedores de recursos e tipos](../../../azure-resource-manager/resource-manager-supported-services.md).

1. Se ainda não fez, instale a CLI do Azure. Você pode obter a versão mais recente em [Instalar a CLI do Azure no Windows](/cli/azure/install-azure-cli-windows).

## <a name="create-and-assign-a-policy-definition"></a>Criar e atribuir uma definição de política

A primeira etapa para obter melhor visibilidade de seus recursos é criar e atribuir políticas sobre seus recursos. A próxima etapa é aprender a criar e atribuir uma política por meio de programação. O exemplo de política realiza auditoria nas contas de armazenamento que estão abertas para todas as redes públicas usando PowerShell, CLI do Azure e solicitações HTTP.

### <a name="create-and-assign-a-policy-definition-with-powershell"></a>Criar e atribuir uma definição de política com o PowerShell

1. Use o snippet JSON a seguir para criar um arquivo JSON com o nome AuditStorageAccounts.json.

   ```json
   {
       "if": {
           "allOf": [{
                   "field": "type",
                   "equals": "Microsoft.Storage/storageAccounts"
               },
               {
                   "field": "Microsoft.Storage/storageAccounts/networkAcls.defaultAction",
                   "equals": "Allow"
               }
           ]
       },
       "then": {
           "effect": "audit"
       }
   }
   ```

   Para obter mais informações sobre a criação de uma definição de política, consulte [Estrutura da definição do Azure Policy](../concepts/definition-structure.md).

1. Execute o seguinte comando para criar uma definição de política usando os arquivos AuditStorageAccounts.json.

   ```azurepowershell-interactive
   New-AzureRmPolicyDefinition -Name 'AuditStorageAccounts' -DisplayName 'Audit Storage Accounts Open to Public Networks' -Policy 'AuditStorageAccounts.json'
   ```

   O comando cria uma definição de política denominada _Auditar Contas de Armazenamento Abertas para Redes Públicas_.
   Para obter mais informações sobre outros parâmetros que você pode usar, consulte [New-AzureRmPolicyDefinition](/powershell/module/azurerm.resources/new-azurermpolicydefinition).

   Quando chamado sem parâmetros de local, `New-AzureRmPolicyDefinition` é padronizado para salvar a definição de política na assinatura selecionada do contexto de sessões. Para salvar a definição para um local diferente, use os seguintes parâmetros:

   - **SubscriptionId** – salvar em uma assinatura diferente. Exige um valor de _GUID_.
   - **ManagementGroupName** – salvar em um grupo de gerenciamento. Exige um valor de _cadeia de caracteres_.

1. Depois de criar sua definição de política, você pode criar uma atribuição de política executando os seguintes comandos:

   ```azurepowershell-interactive
   $rg = Get-AzureRmResourceGroup -Name 'ContosoRG'
   $Policy = Get-AzureRmPolicyDefinition -Name 'AuditStorageAccounts'
   New-AzureRmPolicyAssignment -Name 'AuditStorageAccounts' -PolicyDefinition $Policy -Scope $rg.ResourceId
   ```

   Substitua _ContosoRG_ pelo nome do grupo de recursos desejado.

   O parâmetro **Escopo** em `New-AzureRmPolicyAssignment` também funciona com as assinaturas e os grupos de gerenciamento. O parâmetro usa um caminho de recurso completo, que a propriedade **ResourceId** em `Get-AzureRmResourceGroup` retorna. O padrão para **Escopo** para cada contêiner é como segue.
   Substitua `{rgName}`, `{subId}` e `{mgName}` pelo nome do seu grupo de recursos, ID da assinatura e nome do grupo de gerenciamento, respectivamente.

   - Grupo de recursos – `/subscriptions/{subId}/resourceGroups/{rgName}`
   - Assinatura – `/subscriptions/{subId}/`
   - Grupo de gerenciamento – `/providers/Microsoft.Management/managementGroups/{mgName}`

Para obter mais informações sobre como gerenciar políticas de recursos usando o módulo PowerShell do Azure Resource Manager, consulte [AzureRM.Resources](/powershell/module/azurerm.resources/#policies).

### <a name="create-and-assign-a-policy-definition-using-armclient"></a>Criar e atribuir uma definição de política usando ARMClient

Use o procedimento a seguir para criar uma definição de política.

1. Copie o snippet JSON a seguir para criar um arquivo JSON. Você chamará o arquivo na próxima etapa.

   ```json
   "properties": {
       "displayName": "Audit Storage Accounts Open to Public Networks",
       "policyType": "Custom",
       "mode": "Indexed",
       "description": "This policy ensures that storage accounts with exposure to Public Networks are audited.",
       "parameters": {},
       "policyRule": {
           "if": {
               "allOf": [{
                       "field": "type",
                       "equals": "Microsoft.Storage/storageAccounts"
                   },
                   {
                       "field": "Microsoft.Storage/storageAccounts/networkAcls.defaultAction",
                       "equals": "Allow"
                   }
               ]
           },
           "then": {
               "effect": "audit"
           }
       }
   }
   ```

1. Crie a definição de política usando uma das seguintes chamadas:

   ```
   # For defining a policy in a subscription
   armclient PUT "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyDefinitions/AuditStorageAccounts?api-version=2016-12-01" @<path to policy definition JSON file>

   # For defining a policy in a management group
   armclient PUT "/providers/Microsoft.Management/managementgroups/{managementGroupId}/providers/Microsoft.Authorization/policyDefinitions/AuditStorageAccounts?api-version=2016-12-01" @<path to policy definition JSON file>
   ```

   Substitua o {subscriptionId} anterior pela ID da sua assinatura ou {managementGroupId} pela ID do seu [grupo de gerenciamento](../../management-groups/overview.md).

   Para obter mais informações sobre a estrutura da consulta, confira [Definições de políticas – criar ou atualizar](/rest/api/resources/policydefinitions/createorupdate) e [Definições de políticas – criar ou atualizar em grupo de gerenciamento](/rest/api/resources/policydefinitions/createorupdateatmanagementgroup)

Use o procedimento a seguir para criar uma atribuição de política e atribuir a definição de política no nível do grupo de recursos.

1. Copie o snippet JSON a seguir para criar um arquivo de atribuição de política JSON. Substitua as informações de exemplo nos símbolos &lt;&gt; pelos seus próprios valores.

   ```json
   {
       "properties": {
           "description": "This policy assignment makes sure that storage accounts with exposure to Public Networks are audited.",
           "displayName": "Audit Storage Accounts Open to Public Networks Assignment",
           "parameters": {},
           "policyDefinitionId": "/subscriptions/<subscriptionId>/providers/Microsoft.Authorization/policyDefinitions/Audit Storage Accounts Open to Public Networks",
           "scope": "/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>"
       }
   }
   ```

1. Crie a atribuição de política usando a seguinte chamada:

   ```
   armclient PUT "/subscriptions/<subscriptionID>/resourceGroups/<resourceGroupName>/providers/Microsoft.Authorization/policyAssignments/Audit Storage Accounts Open to Public Networks?api-version=2017-06-01-preview" @<path to Assignment JSON file>
   ```

   Substitua as informações de exemplo nos símbolos &lt;&gt; pelos seus próprios valores.

   Para obter mais informações sobre como fazer chamadas HTTP para a API REST, consulte [Recursos da API REST do Azure](/rest/api/resources/).

### <a name="create-and-assign-a-policy-definition-with-azure-cli"></a>Criar e atribuir uma definição de política com a CLI do Azure

Para criar uma definição de política, use o procedimento a seguir:

1. Copie o snippet JSON a seguir para criar um arquivo de atribuição de política JSON.

  ```json
  {
      "if": {
          "allOf": [{
                  "field": "type",
                  "equals": "Microsoft.Storage/storageAccounts"
              },
              {
                  "field": "Microsoft.Storage/storageAccounts/networkAcls.defaultAction",
                  "equals": "Allow"
              }
          ]
      },
      "then": {
          "effect": "audit"
      }
  }
  ```

1. Execute o seguinte comando para criar uma definição de política:

   ```azurecli-interactive
   az policy definition create --name 'audit-storage-accounts-open-to-public-networks' --display-name 'Audit Storage Accounts Open to Public Networks' --description 'This policy ensures that storage accounts with exposures to public networks are audited.' --rules '<path to json file>' --mode All
   ```

1. Use o comando a seguir para criar uma atribuição de política. Substitua as informações de exemplo nos símbolos &lt;&gt; pelos seus próprios valores.

   ```azurecli-interactive
   az policy assignment create --name '<name>' --scope '<scope>' --policy '<policy definition ID>'
   ```

Você pode obter a ID de definição da política usando o PowerShell com o seguinte comando:

```azurecli-interactive
az policy definition show --name 'Audit Storage Accounts with Open Public Networks'
```

A ID de definição de política para a definição de política que você criou deve se assemelhar ao exemplo a seguir:

```
"/subscription/<subscriptionId>/providers/Microsoft.Authorization/policyDefinitions/Audit Storage Accounts Open to Public Networks"
```

Para obter mais informações sobre como gerenciar políticas de recursos com a CLI do Azure, consulte [Políticas de Recursos da CLI do Azure](/cli/azure/policy?view=azure-cli-latest).

## <a name="next-steps"></a>Próximas etapas

Examine os seguintes artigos para obter mais informações sobre os comandos e consultas neste artigo.

- [Recursos da API REST do Azure](/rest/api/resources/)
- [Módulos do PowerShell do Azure RM](/powershell/module/azurerm.resources/#policies)
- [Comandos de Política da CLI do Azure](/cli/azure/policy?view=azure-cli-latest)
- [Referência da API REST do provedor de recursos de Informações de Política](/rest/api/policy-insights)
- [Organizar seus recursos com grupos de gerenciamento do Azure](../../management-groups/overview.md)