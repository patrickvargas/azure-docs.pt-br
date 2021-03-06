---
title: Mover recursos do Azure para uma nova assinatura ou um novo grupo de recursos | Microsoft Docs
description: Use o Azure Resource Manager para mover recursos para um novo grupo de recursos ou uma nova assinatura.
services: azure-resource-manager
documentationcenter: ''
author: tfitzmac
ms.assetid: ab7d42bd-8434-4026-a892-df4a97b60a9b
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/08/2018
ms.author: tomfitz
ms.openlocfilehash: c65f5364ccd4943d1d3e703ed27099408d3a2a27
ms.sourcegitcommit: 96527c150e33a1d630836e72561a5f7d529521b7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/09/2018
ms.locfileid: "51346585"
---
# <a name="move-resources-to-new-resource-group-or-subscription"></a>Mover recursos para um novo grupo de recursos ou uma nova assinatura

Este artigo mostra como mover recursos para uma nova assinatura ou um novo grupo de recursos na mesma assinatura. Você pode usar o portal, PowerShell, CLI do Azure ou a API REST para mover recursos. As operações de movimentação neste artigo estão disponíveis para você sem nenhuma assistência do suporte do Azure.

O grupo de origem e o grupo de destino ficam bloqueados durante a operação de movimentação de recursos. As operações de gravação e exclusão são bloqueadas nos grupos de recursos até que a migração seja concluída. Esse bloqueio significa que você não pode adicionar, atualizar nem excluir os recursos nos grupos de recursos, mas isso não significa que os recursos estão congelados. Por exemplo, se você mover um SQL Server e seu banco de dados para um novo grupo de recursos, um aplicativo que usa o banco de dados não terá nenhuma inatividade. Ele ainda poderá ler e gravar no banco de dados.

Você não pode alterar o local do recurso. Mover um recurso só o move para um novo grupo de recursos. O novo grupo de recursos pode ter um local diferente, mas que não altere o local do recurso.

> [!NOTE]
> Este artigo descreve como mover recursos dentro de uma oferta de conta existente do Azure. Se você quiser alterar sua conta do Azure, oferecendo (por exemplo, a atualização do gratuita para pré-pago), na verdade, você precisará converter sua assinatura.
> * Para atualizar uma avaliação gratuita, consulte [Faça o upgrade da sua avaliação gratuita ou da assinatura do Microsoft Imagine Azure para o Pague conforme o uso](..//billing/billing-upgrade-azure-subscription.md).
> * Para alterar uma conta de pagamento conforme o uso, consulte [Alterar sua assinatura do Azure Pay-As-You-Go para uma oferta diferente](../billing/billing-how-to-switch-azure-offer.md).
> * Se você não conseguir converter a assinatura, [crie uma solicitação de suporte do Azure](../azure-supportability/how-to-create-azure-support-request.md). Selecione **Subscription Management** para o tipo de problema.

## <a name="checklist-before-moving-resources"></a>Lista de verificação antes de mover recursos

Há algumas etapas importantes a serem realizadas antes de mover um recurso. Ao verificar essas condições, é possível evitar erros.

1. As assinaturas de origem e de destino devem existir no mesmo [locatário do Azure Active Directory](../active-directory/develop/quickstart-create-new-tenant.md). Para verificar se as duas assinaturas têm a mesma ID de locatário, use o Azure PowerShell ou a CLI do Azure.

  Para o Azure PowerShell, use:

  ```azurepowershell-interactive
  (Get-AzureRmSubscription -SubscriptionName <your-source-subscription>).TenantId
  (Get-AzureRmSubscription -SubscriptionName <your-destination-subscription>).TenantId
  ```

  Para a CLI do Azure, use:

  ```azurecli-interactive
  az account show --subscription <your-source-subscription> --query tenantId
  az account show --subscription <your-destination-subscription> --query tenantId
  ```

  Se as IDs de locatário para as assinaturas de origem e de destino não forem iguais, use os métodos a seguir para reconciliá-las:

  * [Transferir a propriedade de uma assinatura do Azure para outra conta](../billing/billing-subscription-transfer.md)
  * [Como associar ou adicionar uma assinatura do Azure ao Azure Active Directory](../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md)

1. A assinatura de destino deve estar registrada para que o provedor de recursos do recurso seja movido. Se não estiver, você receberá um erro afirmando que a **assinatura não está registrada para um tipo de recurso**. Você pode encontrar esse problema ao mover um recurso para uma nova assinatura que nunca tenha sido usada com esse tipo de recurso.

  Para o PowerShell, use os seguintes comandos para obter o status do registro:

  ```azurepowershell-interactive
  Set-AzureRmContext -Subscription <destination-subscription-name-or-id>
  Get-AzureRmResourceProvider -ListAvailable | Select-Object ProviderNamespace, RegistrationState
  ```

  Para registrar um provedor de recursos, use:

  ```azurepowershell-interactive
  Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
  ```

  Para a CLI do Azure, use os seguintes comandos para obter o status do registro:

  ```azurecli-interactive
  az account set -s <destination-subscription-name-or-id>
  az provider list --query "[].{Provider:namespace, Status:registrationState}" --out table
  ```

  Para registrar um provedor de recursos, use:

  ```azurecli-interactive
  az provider register --namespace Microsoft.Batch
  ```

1. A conta de movimentação de recursos deve ter pelo menos as seguintes permissões:

   * **Microsoft.Resources/subscriptions/resourceGroups/moveResources/action** no grupo de recursos de origem.
   * **Microsoft.Resources/subscriptions/resourceGroups/write** no grupo de recursos de destino.

1. Antes de mover os recursos, verifique as cotas de assinatura da assinatura para a qual você está movendo os recursos. Se mover os recursos significa que a assinatura excederá seus limites, será necessário verificar se você pode solicitar um aumento na cota. Para obter uma lista de limites e como solicitar um aumento, consulte [Limites, cotas e restrições em assinaturas e serviços do Azure](../azure-subscription-service-limits.md).

1. Quando possível, quebre grandes movimentações em operações de movimentação separadas. O Gerenciador de Recursos falha imediatamente ao tentar mover mais de 800 recursos em uma única operação. No entanto, mover menos de 800 recursos também pode falhar por tempo limite.

1. O serviço deve permitir a movimentação de recursos. Para determinar se a jogada será bem-sucedida, [valide sua solicitação de movimentação](#validate-move). Consulte as seções abaixo neste artigo [serviços permitem mover os recursos](#services-that-can-be-moved) e quais [services não permitem mover os recursos](#services-that-cannot-be-moved).

## <a name="when-to-call-support"></a>Quando telefonar para o suporte

Você pode mover a maioria dos recursos por meio de operações de autoatendimento mostradas neste artigo. Use as operações de autoatendimento para:

* Mova os recursos do Resource Manager.
* Mover recursos clássicos de acordo com as [Limitações da implantação clássica](#classic-deployment-limitations).

Entre em contato com o [suporte](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) quando você precisar:

* Mover os recursos para uma nova conta do Azure (e o locatário do Azure Active Directory) e precisar de ajuda com as instruções na seção anterior.
* Mover recursos clássicos, mas está tendo problemas com as limitações.

## <a name="validate-move"></a>Validar movimentação

A operação [validate move](/rest/api/resources/resources/validatemoveresources) permite que você teste seu cenário de movimentação sem realmente mover os recursos. Use essa operação para determinar se a movimentação será bem-sucedida. Para executar essa operação, você precisa de:

* nome do grupo de recursos de origem
* ID do recurso do grupo de recursos de destino
* ID do recurso de cada recurso para mover
* o [token de acesso](/rest/api/azure/#acquire-an-access-token) para sua conta

Envie a seguinte solicitação:

```
POST https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<source-group>/validateMoveResources?api-version=2018-02-01
Authorization: Bearer <access-token>
Content-type: application/json
```

Com um corpo de solicitação:

```json
{
 "resources": ['<resource-id-1>', '<resource-id-2>'],
 "targetResourceGroup": "/subscriptions/<subscription-id>/resourceGroups/<target-group>"
}
```

Se a solicitação estiver formatada corretamente, a operação retornará:

```
Response Code: 202
cache-control: no-cache
pragma: no-cache
expires: -1
location: https://management.azure.com/subscriptions/<subscription-id>/operationresults/<operation-id>?api-version=2018-02-01
retry-after: 15
...
```

O código de status 202 indica que a solicitação de validação foi aceita, mas ainda não determinou se a operação de movimentação será bem-sucedida. O valor `location` contém um URL que você usa para verificar o status da operação de longa duração.  

Para verificar o status, envie a seguinte solicitação:

```
GET <location-url>
Authorization: Bearer <access-token>
```

Enquanto a operação ainda está em execução, você continua recebendo o código de status 202. Aguarde o número de segundos indicado no valor `retry-after` antes de tentar novamente. Se a operação de movimentação é validado com êxito, você receberá o código de 204 status. Se a validação da movimentação falhar, você receberá uma mensagem de erro, como:

```json
{"error":{"code":"ResourceMoveProviderValidationFailed","message":"<message>"...}}
```

## <a name="services-that-can-be-moved"></a>Serviços que podem ser movidos

A lista a seguir fornece um resumo geral dos serviços do Azure que podem ser movidos para um novo grupo de recursos e uma nova assinatura. Para obter mais detalhes, confira [Move operation support for resources](move-support-resources.md) (Suporte para a operação de movimentação de recursos).

* Serviços de análise
* Gerenciamento de API
* Aplicativos do Serviço de Aplicativo (aplicativos Web) - consulte [Limitações do Serviço de Aplicativo](#app-service-limitations)
* Certificados do Serviço de Aplicativo
* Application Insights
* Automação
* Azure Active Directory B2C
* Azure Cosmos DB
* Banco de Dados do Azure para MySQL
* Banco de Dados do Azure para PostgreSQL
* Azure DevOps – as organizações do Azure DevOps com compras de extensão que não são da Microsoft precisam [cancelar suas compras](https://go.microsoft.com/fwlink/?linkid=871160) para que possam mover a conta entre assinaturas.
* Mapas do Azure
* Retransmissão do Azure
* Azure Stack - registros
* Lote
* Serviços do BizTalk
* Serviço de Bot
* CDN
* Serviços de Nuvem - veja [Limitações da implantação clássica](#classic-deployment-limitations)
* Serviços Cognitivos
* Registro de Contêiner
* Content Moderator
* Gerenciamento de Custos
* Customer Insights
* Catálogo de Dados
* Data Factory
* Data Lake Analytics
* Data Lake Store
* DNS
* Grade de Eventos
* Hubs de Eventos
* Front Door
* Clusters HDInsight – veja [Limitações do HDInsight](#hdinsight-limitations)
* Central de IoT
* Hubs IoT
* Key Vault
* Load Balancers - consulte [Limitações do Load Balancer](#lb-limitations)
* Log Analytics
* Aplicativos Lógicos
* Aprendizado de Máquina - os serviços Web do Machine Learning Studio podem ser movidos para um grupo de recursos na mesma assinatura, mas não uma assinatura diferente. Outros recursos de Microsoft Machine Learning podem ser movidos entre assinaturas.
* Managed Disks – veja [Limitações das máquinas virtuais para restrições](#virtual-machines-limitations)
* Identidade gerenciada - atribuída pelo usuário
* Serviços de mídia
* Hubs de Notificação
* Insights Operacionais
* Gerenciamento de Operações
* Painéis do portal do Azure
* Power BI - tanto o Power BI inserido Embedded como a coleção de workspaces do BI
* IP público - consulte [Limitações de IP público](#pip-limitations)
* Cache Redis – se a instância do Cache Redis estiver configurada com uma rede virtual, ela não poderá ser movida para uma assinatura diferente. Confira [Limitações de redes virtuais](#virtual-networks-limitations).
* Agendador
* Search
* Barramento de Serviço
* Service Fabric
* Malha do Service Fabric
* Serviço SignalR
* Armazenamento – as contas de armazenamento em regiões diferentes não podem ser movidas na mesma operação. Nesse caso, use operações separadas para cada região.
* Armazenamento (clássico) - consulte [Limitações da implantação clássica](#classic-deployment-limitations)
* Stream Analytics – os trabalhos do Stream Analytics não podem ser movidos durante o estado de execução.
* Servidor de Banco de Dados SQL – o banco de dados e o servidor devem residir no mesmo grupo de recursos. Quando você move um SQL Server, todos os seus bancos de dados também são movidos. Este comportamento se aplica ao Banco de Dados SQL do Azure e ao banco de dados SQL Data Warehouse do Azure.
* Time Series Insights
* Gerenciador de Tráfego
* Máquinas virtuais – para VMs com discos gerenciados, confira [Limitações das máquinas virtuais](#virtual-machines-limitations)
* Máquinas virtuais (clássicas) - consulte [Limitações da implantação clássica](#classic-deployment-limitations)
* Conjuntos de Dimensionamento de Máquinas Virtuais – consulte [Limitações de máquinas virtuais](#virtual-machines-limitations)
* Redes virtuais – consulte [Limitações de redes virtuais](#virtual-networks-limitations)
* Gateway de VPN

## <a name="services-that-cannot-be-moved"></a>Serviços que não podem ser movidos

A lista a seguir fornece um resumo geral dos serviços do Azure que não podem ser movidos para um novo grupo de recursos e uma nova assinatura. Para obter mais detalhes, confira [Move operation support for resources](move-support-resources.md) (Suporte para a operação de movimentação de recursos).

* AD Domain Services
* Serviço de Integridade Híbrida do AD
* Gateway de Aplicativo
* Migração de banco de dados do Azure
* Azure Databricks
* Migrações para Azure
* Lote AI
* Certificados - Os certificados do Serviço de Aplicativo podem ser movidos, mas os certificados carregados têm [limitações](#app-service-limitations).
* Instâncias de Contêiner
* Serviço de Contêiner
* Data Box
* Espaços de Desenvolvimento
* Dynamics LCS
* ExpressRoute
* Serviço do Kubernetes
* Serviços de Laboratórios – mover para um novo grupo de recursos na mesma assinatura está habilitado, mas a troca entre assinaturas está desabilitado.
* Load Balancers - consulte [Limitações do Load Balancer](#lb-limitations)
* Aplicativos gerenciados
* Microsoft Genomics
* NetApp
* IP público - consulte [Limitações de IP público](#pip-limitations)
* Cofre de Serviços de Recuperação – não mova os recursos de Computação, Rede e Armazenamento associados ao cofre dos Serviços de Recuperação. Consulte [Limitações dos Serviços de Recuperação](#recovery-services-limitations).
* SAP HANA no Azure
* Segurança
* Site Recovery
* Gerenciador de Dispositivos StorSimple
* Redes Virtuais (clássicas) - consulte [Limitações da implantação clássica](#classic-deployment-limitations)

## <a name="virtual-machines-limitations"></a>Limitações das máquinas virtuais

Há suporte para mover discos gerenciados desde 24 de setembro de 2018. 

1. Registra a assinatura de origem, esse recurso.

  ```azurepowershell-interactive
  Register-AzureRmProviderFeature -FeatureName ManagedResourcesMove -ProviderNamespace Microsoft.Compute
  ```

  ```azurecli-interactive
  az feature register --namespace Microsoft.Compute --name ManagedResourcesMove
  ```

1. A solicitação de registro retorna inicialmente o estado de `Registering`. Você pode verificar o status atual com:

  ```azurepowershell-interactive
  Get-AzureRmProviderFeature -FeatureName ManagedResourcesMove -ProviderNamespace Microsoft.Compute
  ```

  ```azurecli-interactive
  az feature show --namespace Microsoft.Compute --name ManagedResourcesMove
  ```

1. Aguarde alguns minutos para que o status mude para `Registered`.

1. Depois que o recurso estiver registrado, registre o provedor de recursos `Microsoft.Compute`. Execute esta etapa, mesmo se o provedor de recursos já tiver sido registrado.

  ```azurepowershell-interactive
  Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Compute
  ```

  ```azurecli-interactive
  az provider register --namespace Microsoft.Compute
  ```

Esse suporte significa que você também pode mover:

* Máquinas virtuais com Managed Disks
* Imagens gerenciadas
* Instantâneos gerenciados
* Conjuntos de disponibilidade com Máquinas Virtuais com o Managed Disks

Aqui estão as restrições que ainda estão sem suporte:

* Máquinas Virtuais com certificado armazenado no Key Vault podem ser movidas para um novo grupo de recursos na mesma assinatura, mas não entre assinaturas.
* Máquinas virtuais configuradas com o Backup do Azure. Use a solução alternativa abaixo para mover essas máquinas virtuais
  * Encontre o local de sua máquina virtual.
  * Localize um grupo de recursos com o seguinte padrão de nomenclatura: `AzureBackupRG_<location of your VM>_1` por exemplo, AzureBackupRG_westus2_1
  * Se estiver no portal do Azure, marque "Mostrar tipos ocultos"
  * Se estiver no PowerShell, use o cmdlet `Get-AzureRmResource -ResourceGroupName AzureBackupRG_<location of your VM>_1`
  * Se estiver na CLI, use o `az resource list -g AzureBackupRG_<location of your VM>_1`
  * Agora, localize o recurso com o tipo `Microsoft.Compute/restorePointCollections` que tem o padrão de nomenclatura `AzureBackup_<name of your VM that you're trying to move>_###########`
  * Excluir este recurso
  * Após a conclusão da exclusão, você poderá mover sua Máquina Virtual
* Os Conjuntos de Dimensionamento de Máquinas Virtuais com o Load Balancer do SKU Standard ou o IP público do SKU Standard não podem ser movidos
* As máquinas virtuais criadas a partir dos recursos do Marketplace com os planos anexados não podem ser movidas entre grupos de recursos ou assinaturas. Desprovisione a máquina virtual na assinatura atual e implante-a novamente na nova assinatura.

## <a name="virtual-networks-limitations"></a>Limitações das redes virtuais

Ao mover uma rede virtual, você também deve mover os recursos dependentes. Para Gateways de VPN, você deve mover endereços IP, gateways de rede virtual e todos os recursos de conexão associados. Os gateways de rede local podem estar em um grupo de recursos diferentes.

Para mover uma rede virtual emparelhada, primeiro é necessário desabilitar o emparelhamento de rede virtual. Quando desabilitado, você pode mover a rede virtual. Após a movimentação, reabilite o emparelhamento de rede virtual.

Você não pode mover uma rede virtual para uma assinatura diferente caso a rede virtual contenha uma sub-rede com links de navegação de recurso. Por exemplo, se um recurso de Cache Redis estiver implantado em uma sub-rede, essa sub-rede terá um link de navegação do recurso.

## <a name="app-service-limitations"></a>Limitações do Serviço de Aplicativo

As limitações para mover recursos do Serviço de Aplicativo diferem se você está movendo os recursos dentro de uma assinatura ou para uma nova assinatura.

As limitações descritas nesta seção se aplicam a certificados carregados, não a Certificados do Serviço de Aplicativo. Você pode mover Certificados do Serviço de Aplicativo para um novo grupo de recursos ou assinatura sem limitações. Se você tiver vários aplicativos Web que usam o mesmo Certificado do Serviço de Aplicativo, primeiro mova todos os aplicativos Web e depois mova o certificado.

### <a name="moving-within-the-same-subscription"></a>Movendo dentro da mesma assinatura

Ao mover um aplicativo Web _dentro da mesma assinatura_, você não pode mover os certificados SSL carregados. No entanto, você pode mover um aplicativo Web para o novo grupo de recursos sem mover o respectivo certificado SSL carregado, mantendo a funcionalidade SSL do aplicativo.

Se você deseja mover o certificado SSL com o aplicativo Web, siga estas etapas:

1. Exclua o certificado carregado do aplicativo Web.
2. Mova o aplicativo Web.
3. Carregue o certificado no aplicativo Web movido.

### <a name="moving-across-subscriptions"></a>Movendo entre assinaturas

Ao mover um aplicativo Web _entre assinaturas_, as seguintes limitações se aplicam:

- O grupo de recursos de destino não pode ter nenhum recurso de Serviço de Aplicativo existente. Os recursos de Serviço de Aplicativo incluem:
    - Aplicativos Web
    - Planos do Serviço de Aplicativo
    - Certificados SSL importados ou carregados
    - Ambientes de Serviço de Aplicativo
- Todos os recursos de Serviço de Aplicativo no grupo de recursos devem ser movidos juntos.
- Recursos do Serviço de Aplicativo podem ser movidos apenas no grupo de recursos no qual eles foram originalmente criados. Se um recurso do Serviço de Aplicativo não estiver mais em seu grupo de recursos original, ele deverá ser movido de volta para esse grupo de recursos original primeiro e, em seguida, poderá ser movido entre assinaturas.

## <a name="classic-deployment-limitations"></a>Limitações da implantação clássica

As limitações para mover recursos do Serviço de Aplicativo diferem se você está movendo os recursos dentro de uma assinatura ou para uma nova assinatura.

### <a name="same-subscription"></a>Mesma assinatura

Ao mover recursos de um grupo de recursos para outro na mesma assinatura, as seguintes restrições se aplicarão:

* Redes virtuais (clássicas) não podem ser movidas.
* Máquinas virtuais (clássicas) devem ser movidas com o serviço de nuvem.
* Um serviço de nuvem pode ser movido apenas quando a movimentação incluir todas as suas máquinas virtuais.
* Apenas um serviço de nuvem pode ser movido por vez.
* Apenas uma conta de armazenamento (clássica) pode ser movida por vez.
* Uma conta de armazenamento (clássica) não pode ser movida na mesma operação com uma máquina virtual ou um serviço de nuvem.

Para mover recursos clássicos para um novo grupo de recursos dentro da mesma assinatura, use o [portal](#use-portal), o [Azure PowerShell](#use-powershell), a [CLI do Azure](#use-azure-cli) ou a [API REST](#use-rest-api). Use as mesmas operações como você usa para mover os recursos do Resource Manager.

### <a name="new-subscription"></a>Nova assinatura

Ao mover recursos para uma nova assinatura, as seguintes restrições se aplicarão:

* Todos os recursos clássicos na assinatura devem ser movidos na mesma operação.
* A assinatura de destino não deve conter nenhum outro recurso clássico.
* A movimentação pode ser solicitada apenas por meio de uma API REST separada para movimentações clássicas. Os comandos de movimentação padrão do Gerenciador de Recursos não funcionam quando há uma movimentação dos recursos clássicos para uma nova assinatura.

Para mover recursos clássicos para uma nova assinatura, use operações REST específicas para recursos clássicos. Para usar REST, execute as seguintes etapas:

1. Verifique se a assinatura de origem pode participar de uma movimentação entre assinaturas. Use a operação a seguir:

  ```HTTP
  POST https://management.azure.com/subscriptions/{sourceSubscriptionId}/providers/Microsoft.ClassicCompute/validateSubscriptionMoveAvailability?api-version=2016-04-01
  ```

     No corpo da solicitação, inclua:

  ```json
  {
    "role": "source"
  }
  ```

     A resposta para a operação de validação é no seguinte formato:

  ```json
  {
    "status": "{status}",
    "reasons": [
      "reason1",
      "reason2"
    ]
  }
  ```

2. Verifique se a assinatura de destino pode participar de uma movimentação entre assinaturas. Use a operação a seguir:

  ```HTTP
  POST https://management.azure.com/subscriptions/{destinationSubscriptionId}/providers/Microsoft.ClassicCompute/validateSubscriptionMoveAvailability?api-version=2016-04-01
  ```

     No corpo da solicitação, inclua:

  ```json
  {
    "role": "target"
  }
  ```

     A resposta está no mesmo formato que a validação de assinatura de origem.
3. Se ambas as assinaturas forem aprovadas na validação, mova todos os recursos clássicos de uma assinatura para outra, use a seguinte operação:

  ```HTTP
  POST https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.ClassicCompute/moveSubscriptionResources?api-version=2016-04-01
  ```

    No corpo da solicitação, inclua:

  ```json
  {
    "target": "/subscriptions/{target-subscription-id}"
  }
  ```

A operação pode executar por vários minutos.

## <a name="recovery-services-limitations"></a>Limitações dos Serviços de Recuperação

A movimentação dos recursos de Armazenamento, Rede ou Computação usados para configurar a recuperação de desastres com o Azure Site Recovery não está habilitada.

Por exemplo, suponha que você configurou a replicação das máquinas locais para uma conta de armazenamento (Storage1) e queira que a máquina protegida venha, após o failover, para o Azure como uma máquina virtual (VM1) conectada a uma rede virtual (Network1). Você não pode mover nenhum desses recursos do Microsoft Azure – Storage1, VM1 e Network1 – entre grupos de recursos dentro da mesma assinatura ou em assinaturas diferentes.

Como mover uma VM registrada no **Backup do Azure** entre grupos de recursos:
 1. Pare temporariamente o backup e mantenha os dados de backup
 2. Mova a VM para o grupo de recursos de destino
 3. Proteja-o novamente sob o mesmo/novo cofre. Os usuários poderão restaurar a partir dos pontos de restauração disponíveis criados antes da operação de movimentação.
Se o usuário mover a VM com backup nas assinaturas, as etapas 1 e 2 permanecerão as mesmas. Na etapa 3, o usuário precisa proteger a VM em um novo cofre presente/criado na assinatura de destino. O cofre dos Serviços de Recuperação não oferece suporte a backups de assinatura cruzada.

## <a name="hdinsight-limitations"></a>Limitações do HDInsight

Você pode mover os clusters HDInsight para uma nova assinatura ou grupo de recursos. No entanto, não é possível mover os recursos de rede vinculados ao cluster HDInsight (por exemplo, a rede virtual, NIC ou balanceador de carga) entre assinaturas. Além disso, não é possível mover uma para um novo grupo de recursos uma NIC que está conectada a uma máquina virtual para o cluster.

Ao mover um cluster HDInsight para uma nova assinatura, mova primeiro os outros recursos (como a conta de armazenamento). Em seguida, mova apenas o cluster HDInsight.

## <a name="search-limitations"></a>Pesquisar limitações

Não é possível mover simultaneamente vários recursos de pesquisa colocados em regiões diferentes.
Nesse caso, você precisa movê-los separadamente.

## <a name="lb-limitations"></a> Limitações do Load Balancer

O Load Balancer de SKU básica pode ser movido.
O balanceador de carga de SKU padrão não pode ser movido.

## <a name="pip-limitations"></a> Limitações de IP públicos

O IP público de SKU básica pode ser movido.
O IP Público SKU padrão não pode ser movido.

## <a name="use-portal"></a>Usar o portal

Para mover recursos, selecione o grupo de recursos que os contém e, em seguida, o botão **Mover**.

![Mover recursos](./media/resource-group-move-resources/select-move.png)

Selecione se você está movendo os recursos para um novo grupo de recursos ou uma nova assinatura.

Selecione os recursos a serem movidos e o grupo de recursos de destino. Confirme que você precisa atualizar scripts para esses recursos e selecione **OK**. Se tiver selecionado o ícone de edição da assinatura na etapa anterior, você também precisará selecionar a assinatura de destino.

![selecionar destino](./media/resource-group-move-resources/select-destination.png)

Em **Notificações**, você verá que a operação de movimentação está em execução.

![mostrar status da movimentação](./media/resource-group-move-resources/show-status.png)

Quando for concluída, você será notificado sobre o resultado.

![mostrar resultado da movimentação](./media/resource-group-move-resources/show-result.png)

## <a name="use-powershell"></a>Usar o PowerShell

Para mover os recursos existentes para outro grupo de recursos ou assinatura, use o comando [Move-AzureRmResource](/powershell/module/azurerm.resources/move-azurermresource) . O exemplo a seguir mostra como mover vários recursos para um novo grupo de recursos.

```azurepowershell-interactive
$webapp = Get-AzureRmResource -ResourceGroupName OldRG -ResourceName ExampleSite
$plan = Get-AzureRmResource -ResourceGroupName OldRG -ResourceName ExamplePlan
Move-AzureRmResource -DestinationResourceGroupName NewRG -ResourceId $webapp.ResourceId, $plan.ResourceId
```

Para mover para uma nova assinatura, inclua um valor para o parâmetro `DestinationSubscriptionId`.

## <a name="use-azure-cli"></a>Usar a CLI do Azure

Para mover os recursos existentes para outro grupo de recursos ou assinatura, use o comando [az resource move](/cli/azure/resource?view=azure-cli-latest#az-resource-move). Forneça as IDs dos recursos a mover. O exemplo a seguir mostra como mover vários recursos para um novo grupo de recursos. No parâmetro `--ids`, forneça uma lista separada por espaços das IDs do recurso que deseja mover.

```azurecli
webapp=$(az resource show -g OldRG -n ExampleSite --resource-type "Microsoft.Web/sites" --query id --output tsv)
plan=$(az resource show -g OldRG -n ExamplePlan --resource-type "Microsoft.Web/serverfarms" --query id --output tsv)
az resource move --destination-group newgroup --ids $webapp $plan
```

Para mover para uma nova assinatura, forneça o parâmetro `--destination-subscription-id`.

## <a name="use-rest-api"></a>Usar a API REST

Para mover recursos existentes para outro grupo de recursos ou outra assinatura, execute:

```HTTP
POST https://management.azure.com/subscriptions/{source-subscription-id}/resourcegroups/{source-resource-group-name}/moveResources?api-version={api-version}
```

No corpo da solicitação, especifique o grupo de recursos de destino e os recursos para mover. Para obter mais informações sobre a operação de movimentação REST, consulte [Mover recursos](/rest/api/resources/Resources/MoveResources).

## <a name="next-steps"></a>Próximas etapas

* Para saber mais sobre os cmdlets do PowerShell para gerenciar sua assinatura, veja [Como usar o Azure PowerShell com o Resource Manager](powershell-azure-resource-manager.md).
* Para saber mais sobre os comandos da CLI do Azure para gerenciar sua assinatura, veja [Como usar a CLI do Azure com o Resource Manager](xplat-cli-azure-resource-manager.md).
* Para saber mais sobre os recursos do portal para gerenciar sua assinatura, veja [Usar o Portal do Azure para gerenciar recursos](resource-group-portal.md).
* Para saber mais sobre como aplicar uma organização lógica aos seus recursos, veja [Usando marcações para organizar seus recursos](resource-group-using-tags.md).
