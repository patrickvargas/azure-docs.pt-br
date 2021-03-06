---
title: Gerenciar o ciclo de vida do Armazenamento do Azure
description: Aprenda a criar regras de política de ciclo de vida para fazer a transição dos dados antigos para os níveis Hot to Cool e Archive.
services: storage
author: yzheng-msft
ms.service: storage
ms.topic: article
ms.date: 11/01/2018
ms.author: yzheng
ms.component: common
ms.openlocfilehash: c6647ff97b078ca5afa5c66833a1617f6b3ec0f1
ms.sourcegitcommit: 1fc949dab883453ac960e02d882e613806fabe6f
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/03/2018
ms.locfileid: "50978801"
---
# <a name="managing-the-azure-blob-storage-lifecycle-preview"></a>Gerenciar ciclo de vida do Armazenamento de Blobs do Azure (Versão Prévia)

Conjuntos de dados têm ciclos de vida exclusivos. Alguns dados são acessados frequentemente cedo no ciclo de vida, mas a necessidade de acesso cai drasticamente conforme os dados se tornam mais antigos. Alguns dados permanecem ociosos na nuvem e raramente são acessados após serem armazenados. Alguns dados expiram dias ou meses após a criação, enquanto outros conjuntos de dados são lidos e modificados ativamente durante seu tempo de vida. O gerenciamento do ciclo de vida de armazenamento do Azure Blob (Preview) oferece uma política rica e baseada em regras que você pode usar nas contas de armazenamento do GPV2 e do Blob para fazer a transição dos dados para as camadas de acesso apropriadas ou expirar no final do ciclo de vida.

A política de gerenciamento de ciclo de vida ajuda você a:

- Fazer a transição dos blobs de uma camada de armazenamento mais fria (frequente para esporádico, frequente para de arquivos ou esporádico para de arquivos) para otimizar o desempenho e os custos
- Excluir os blobs no final dos respectivos ciclos de vida
- Definir regras a serem executadas uma vez por dia no nível da conta de armazenamento
- Aplicar regras a contêineres ou a um subconjunto de blobs (usando prefixos como filtros)

Considere a possibilidade de um conjunto de dados que são acessados frequentemente durante a fase inicial do ciclo de vida, são necessários apenas ocasionalmente depois de duas semanas e raramente são acessados após um mês e além. Nesse cenário, o armazenamento frequente é melhor durante as fases iniciais, o armazenamento esporádico é mais apropriado para acesso ocasional e o armazenamento de arquivos é a melhor opção de camada após os dados envelhecerem por um mês. Ajustando as camadas de armazenamento em relação à idade dos dados, você pode criar as opções de armazenamento menos dispendiosas para suas necessidades. Para conseguir essa transição, as regras de política de gerenciamento do ciclo de vida estão disponíveis para mover os dados antigos para camadas mais frias.

## <a name="storage-account-support"></a>Suporte da conta de armazenamento

A política de gerenciamento do ciclo de vida está disponível tanto com a conta GPv2 (de uso geral v2) quanto com a conta de Armazenamento de Blobs. Você pode atualizar uma conta de Uso geral (GPv1) existente para uma conta GPv2 por meio de um processo simples de um clique no portal do Azure sem tempo de inatividade. Para saber mais sobre as contas de armazenamento, confira [Visão geral da conta de Armazenamento do Azure](../common/storage-account-overview.md) para saber mais.  

## <a name="pricing"></a>Preços 

O recurso de gerenciamento do ciclo de vida é gratuito na versão prévia. O custo de operação normal para as chamadas à API [Listar Blobs](https://docs.microsoft.com/rest/api/storageservices/list-blobs) e [Definir Camada de Blob](https://docs.microsoft.com/rest/api/storageservices/set-blob-tier) é cobrado dos clientes. Para saber mais sobre os preços, consulte [Preço de Blob de Blocos](https://azure.microsoft.com/pricing/details/storage/blobs/).

## <a name="register-for-preview"></a>Registrar-se para a versão prévia 
Para registrar-se para a versão prévia pública, você precisará enviar uma solicitação para registrar esse recurso em sua assinatura. Depois que a solicitação for aprovada (dentro de alguns dias), qualquer conta GPv2 ou conta de Armazenamento de Blobs nova ou existente no Oeste dos EUA 2, no Centro-oeste dos EUA 2 e na Europa Ocidental terá o recurso habilitado. Durante a versão prévia, há suporte apenas para o blob de blocos. Assim como acontece com a maioria das versões prévias, esse recurso não deve ser usado para cargas de trabalho de produção até passar para disponibilidade geral.

Para enviar uma solicitação, execute os seguintes comandos da CLI ou do PowerShell.

### <a name="powershell"></a>PowerShell

Para enviar uma solicitação:

```powershell
Register-AzureRmProviderFeature -FeatureName DLM -ProviderNamespace Microsoft.Storage 
```
É possível verificar o status de aprovação do registro com o seguinte comando:
```powershell
Get-AzureRmProviderFeature -FeatureName DLM -ProviderNamespace Microsoft.Storage
```
Se o recurso for aprovado e registrado corretamente, você deverá receber o estado "Registrado".

### <a name="azure-cli"></a>CLI do Azure

Para enviar uma solicitação: 
```cli
az feature register --namespace Microsoft.Storage --name DLM
```
É possível verificar o status de aprovação do registro com o seguinte comando:
```cli
az feature show --namespace Microsoft.Storage --name DLM
```
Se o recurso for aprovado e registrado corretamente, você deverá receber o estado "Registrado". 


## <a name="add-or-remove-a-policy"></a>Adicionar ou remover uma política 

Você pode adicionar, editar ou remover uma política usando o Portal do Azure, [PowerShell](https://www.powershellgallery.com/packages/AzureRM.Storage/5.0.3-preview), [APIs REST](https://docs.microsoft.com/rest/api/storagerp/managementpolicies/createorupdate) ou ferramentas cliente nos seguintes idiomas: [.NET](https://www.nuget.org/packages/Microsoft.Azure.Management.Storage/8.0.0-preview), [Python](https://pypi.org/project/azure-mgmt-storage/2.0.0rc3/), [Node.js]( https://www.npmjs.com/package/azure-arm-storage/v/5.0.0), [Ruby](  https://rubygems.org/gems/azure_mgmt_storage/versions/0.16.2). 

### <a name="azure-portal"></a>Portal do Azure

1. Entre no [Portal do Azure](https://portal.azure.com).

2. Para navegar até sua conta de armazenamento, selecione Todos os Recursos e sua conta de armazenamento.

3. No campo Configurações, clique em **Gerenciamento do ciclo de vida** agrupado em Serviço de Blob para visualizar e/ou alterar sua política.

### <a name="powershell"></a>PowerShell

```powershell
$rules = '{ ... }' 

Set-AzureRmStorageAccountManagementPolicy -ResourceGroupName [resourceGroupName] -StorageAccountName [storageAccountName] -Policy $rules 

Get-AzureRmStorageAccountManagementPolicy -ResourceGroupName [resourceGroupName] -StorageAccountName [storageAccountName]
```

> [!NOTE]
Se você habilitar as regras de firewall para sua conta de armazenamento, as solicitações de gerenciamento do ciclo de vida poderão ser bloqueadas. Você pode desbloqueá-las fornecendo exceções. Para saber mais, consulte a seção Exceções em [Configurar firewalls e redes virtuais](https://docs.microsoft.com/azure/storage/common/storage-network-security#exceptions).

## <a name="policy"></a>Política

Uma política de gerenciamento do ciclo de vida é uma coleção de regras em um documento JSON:

```json
{
  "version": "0.5",
  "rules": [
    {
      "name": "rule1",
      "type": "Lifecycle",
      "definition": {...}
    },
    {
      "name": "rule2",
      "type": "Lifecycle",
      "definition": {...}
    }
  ]
}
```


Dentro de uma política, dois parâmetros são necessários:

| Nome do parâmetro | Tipo de parâmetro | Observações |
|----------------|----------------|-------|
| version        | Uma cadeia de caracteres expressada como `x.x` | O número de versão da versão prévia é 0.5 |
| regras          | Uma matriz de objetos de regra | Pelo menos uma regra é necessária em cada política. Durante a versão prévia, você pode especificar até quatro regras por política. |

Dentro de cada regra, são necessários três parâmetros:

| Nome do parâmetro | Tipo de parâmetro | Observações |
|----------------|----------------|-------|
| NOME           | Cadeia de caracteres | Um nome de regra pode conter qualquer combinação de caracteres alfanuméricos. A regra de nome diferencia maiúsculas de minúsculas. Ela deve ser exclusiva em uma política. |
| Tipo           | Um valor de enumeração | O valor válido para a versão prévia é `Lifecycle` |
| definição     | Um objeto que define a regra de ciclo de vida | Cada definição é composta por um conjunto de filtros e um conjunto de ações. |

## <a name="rules"></a>Regras

Cada definição de regra inclui um conjunto de filtros e um conjunto de ações. O [conjunto de filtros](#rule-filters) é usado para limitar ações de regras a um determinado conjunto de objetos em um contêiner ou a nomes de objetos. O [conjunto de ações](#rule-actions) aplica-se a camada ou excluir ações para o conjunto filtrado de objetos.

### <a name="sample-rule"></a>Exemplo de regra
A regra de amostra a seguir filtra a conta para executar as ações somente em `container1/foo`. Para todos os objetos que existem dentro de `container1` **E** começa com `foo`, as seguintes ações são executadas: 

- Blob de camada para camada fria 30 dias após a última modificação
- Blob de camada para camada 90 dias após a última modificação do arquivo
- Excluir o blob 2.555 dias (7 anos) após da última modificação
- Excluir instantâneos de blob 90 dias após a criação do instantâneo

```json
{
  "version": "0.5",
  "rules": [ 
    {
      "name": "ruleFoo", 
      "type": "Lifecycle", 
      "definition": {
        "filters": {
          "blobTypes": [ "blockBlob" ],
          "prefixMatch": [ "container1/foo" ]
        },
        "actions": {
          "baseBlob": {
            "tierToCool": { "daysAfterModificationGreaterThan": 30 },
            "tierToArchive": { "daysAfterModificationGreaterThan": 90 },
            "delete": { "daysAfterModificationGreaterThan": 2555 }
          },
          "snapshot": {
            "delete": { "daysAfterCreationGreaterThan": 90 }
          }
        }
      }
    }
  ]
}

```

### <a name="rule-filters"></a>Filtros de regra

Os filtros limitam as ações de regra a um subconjunto de blobs na conta de armazenamento. Se vários filtros são definidos, uma lógica `AND` é executada em todos os filtros.

Durante a versão prévia, os filtros válidos incluem:

| Nome do filtro | Tipo do filtro | Observações | Obrigatório |
|-------------|-------------|-------|-------------|
| blobTypes   | Uma matriz de valores de enumeração predefinidos. | Para a versão prévia, há suporte apenas para `blockBlob`. | SIM |
| prefixMatch | Uma matriz de cadeias de caracteres para prefixos a serem correspondidos. Uma cadeia de caracteres de prefixo deve começar com um nome de contêiner. Por exemplo, se todos os blobs estiverem em "https://myaccount.blob.core.windows.net/container1/foo/..". deve ser correspondido por uma regra, o prefixMatch seria "container1 / foo". | Se prefixMatch não for definido, as regras serão aplicadas a todos os blobs na conta. | Não  |

### <a name="rule-actions"></a>Ações de regra

Ações são aplicadas aos blobs filtrados quando a condição de execução é atendida.

Na versão prévia, o gerenciamento de ciclo de vida oferece suporte à exclusão de blobs e à colocação de blobs em camadas, bem como à exclusão de instantâneos de blob. Cada regra deve ter pelo menos uma ação definida em blobs ou instantâneos de blob.

| Ação        | Blob base                                   | Instantâneo      |
|---------------|---------------------------------------------|---------------|
| tierToCool    | Dá suporte a blobs atualmente presentes na camada de acesso frequente         | Sem suporte |
| tierToArchive | Dá suporte a blobs atualmente presentes na camada de acesso frequente ou esporádico | Sem suporte |
| excluir        | Com suporte                                   | Com suporte     |

>[!NOTE] 
Se mais de uma ação é definida no mesmo blob, o gerenciamento de ciclo de vida aplica a ação mais barata ao blob. (por exemplo, a ação `delete` é mais barata do que a ação `tierToArchive`. A ação `tierToArchive` é mais barata do que a ação `tierToCool`.)

Na versão prévia, as condições de execução da ação são com base na idade. Para acompanhar a idade do blob base, usa a hora da última modificação, enquanto para acompanhar a idade a dos instantâneos de blob, usa-se a hora de criação do instantâneo.

| Condição de execução da ação | Valor de condição | DESCRIÇÃO |
|----------------------------|-----------------|-------------|
| daysAfterModificationGreaterThan | Valor inteiro que indica a idade em dias | Condição válida para ações de blob de base |
| daysAfterCreationGreaterThan     | Valor inteiro que indica a idade em dias | Condição válida para ações de instantâneo de blob | 

## <a name="examples"></a>Exemplos
Os exemplos a seguir demonstram como tratar cenários comuns com as regras de política de ciclo de vida.

### <a name="move-aging-data-to-a-cooler-tier"></a>Mover dados antigos para uma camada mais fria

O exemplo a seguir demonstra como fazer a transição de blobs de bloco prefixados com `container1/foo` ou `container2/bar`. A política faz a transição de blobs que não foram modificados há mais de 30 dias para o armazenamento esporádico e de blobs não modificados há mais de 90 dias para a camada de arquivos:

```json
{
  "version": "0.5",
  "rules": [ 
    {
      "name": "agingRule", 
      "type": "Lifecycle", 
      "definition": 
        {
          "filters": {
            "blobTypes": [ "blockBlob" ],
            "prefixMatch": [ "container1/foo", "container2/bar" ]
          },
          "actions": {
            "baseBlob": {
              "tierToCool": { "daysAfterModificationGreaterThan": 30 },
              "tierToArchive": { "daysAfterModificationGreaterThan": 90 }
            }
          }
        }      
    }
  ]
}
```

### <a name="archive-data-at-ingest"></a>Arquivar dados em ingestão 

Alguns dados permanecem ociosos na nuvem e raramente, ou nunca, são acessados após serem armazenados. É melhor arquivar esses dados imediatamente assim que eles são ingeridos. A política de ciclo de vida a seguir é configurada para arquivar dados no ato da ingestão. Este exemplo faz a transição de blocos de blocos na conta de armazenamento do contêiner `archivecontainer` imediatamente para uma camada de arquivamento. A transição imediata é realizada agindo sobre os blobs 0 dias após a hora da última modificação:

```json
{
  "version": "0.5",
  "rules": [ 
    {
      "name": "archiveRule", 
      "type": "Lifecycle", 
      "definition": 
        {
          "filters": {
            "blobTypes": [ "blockBlob" ],
            "prefixMatch": [ "archivecontainer" ]
          },
          "actions": {
            "baseBlob": { 
                "tierToArchive": { "daysAfterModificationGreaterThan": 0 }
            }
          }
        }      
    }
  ]
}

```

### <a name="expire-data-based-on-age"></a>Expirar os dados com base na idade

Espera-se que alguns dados expirem dias ou meses após a criação para reduzir custos ou para estar em conformidade com as normas governamentais. Uma política de gerenciamento do ciclo de vida pode ser configurada para a data de expiração por exclusão com base na idade dos dados. O exemplo a seguir mostra uma política que exclui todos os blobs de bloco (sem nenhum prefixo especificado) mais antigos do que 365 dias.

```json
{
  "version": "0.5",
  "rules": [ 
    {
      "name": "expirationRule", 
      "type": "Lifecycle", 
      "definition": 
        {
          "filters": {
            "blobTypes": [ "blockBlob" ]
          },
          "actions": {
            "baseBlob": {
              "delete": { "daysAfterModificationGreaterThan": 365 }
            }
          }
        }      
    }
  ]
}
```

### <a name="delete-old-snapshots"></a>Excluir instantâneos antigos

Para dados que são modificados e acessados regularmente durante seu ciclo de vida, instantâneos geralmente são usados para controlar versões mais antigas dos dados. Você pode criar uma política que exclui os instantâneos antigos com base na idade do instantâneo. A idade de instantâneo é determinada avaliando-se a hora de criação do instantâneo. Esta regra de política exclui instantâneos blobs de blocos no contêiner `activedata` com 90 dias ou mais após a criação do instantâneo.

```json
{
  "version": "0.5",
  "rules": [ 
    {
      "name": "snapshotRule", 
      "type": "Lifecycle", 
      "definition": 
        {
          "filters": {
            "blobTypes": [ "blockBlob" ],
            "prefixMatch": [ "activedata" ]
          },
          "actions": {            
            "snapshot": {
              "delete": { "daysAfterCreationGreaterThan": 90 }
            }
          }
        }      
    }
  ]
}
```
## <a name="faq"></a>Perguntas frequentes
### <a name="i-created-a-new-policy-why-are-the-actions-specified-not-executed-immediately"></a>Eu criei uma nova política, por que as ações especificadas não são executadas imediatamente? 

A política de ciclo de vida é executada uma vez por dia pela plataforma. Depois que uma nova política é definida, ela pode levar até 24 horas para ações, como a disposição em camadas ou a exclusão para ser iniciada e executada.  

## <a name="next-steps"></a>Próximas etapas

Saiba como recuperar dados após uma exclusão acidental:

- [Exclusão reversível para blobs do Armazenamento do Azure ](../blobs/storage-blob-soft-delete.md)
