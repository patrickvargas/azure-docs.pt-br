---
title: Visão geral da conta de armazenamento do Azure | Microsoft Docs
description: Entenda as opções para criar e usar uma conta do Armazenamento do Azure.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 09/13/2018
ms.author: tamram
ms.component: common
ms.openlocfilehash: 227ef61ee4809d376c6ac5e8e8c1a7f9c364b7fc
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51255755"
---
# <a name="azure-storage-account-overview"></a>Visão geral da conta de armazenamento do Azure

Uma conta de armazenamento do Azure contém todos os seus objetos de dados do Armazenamento do Azure: blobs, arquivos, filas, tabelas e discos. Os dados na conta de armazenamento do Azure são duráveis e altamente disponíveis, seguros, enormemente escalonáveis e acessíveis de qualquer lugar no mundo por meio de HTTP ou HTTPS. 

Para saber como criar uma conta de armazenamento do Azure, confira [Criar uma conta de armazenamento](storage-quickstart-create-account.md).

## <a name="types-of-storage-accounts"></a>Tipos de contas de armazenamento

O Armazenamento do Azure oferece três tipos de conta de armazenamento. Cada tipo é compatível com recursos diferentes e tem um modelo de preços próprio. Considere essas diferenças antes de criar uma conta de armazenamento para determinar o melhor tipo de conta para seus aplicativos. Os tipos de conta de armazenamento são:

* **[Contas para uso geral v2](#general-purpose-v2-accounts)**  (recomendado na maioria dos cenários)
* **[Contas para uso geral v1](#general-purpose-v1-accounts)**
* **[Contas de armazenamento](#blob-storage-accounts) de Blobs** 

A tabela a seguir descreve os tipos de conta de armazenamento e suas funcionalidades:

| Tipo de conta de armazenamento | Serviços com suporte                       | Níveis de desempenho compatíveis | Camadas de acesso compatíveis               | Opções de replicação                                                | Modelo de implantação<sup>1</sup>  | Criptografia<sup>2</sup> |
|----------------------|------------------------------------------|-----------------------------|--------------------------------------|--------------------------------------------------------------------|-------------------|------------|
| Uso geral V2   | Blob, arquivo, fila, tabela e disco       | Standard, Premium           | Quente, frio, de arquivos<sup>3</sup> | LRS, ZRS<sup>4</sup>, GRS, RA-GRS | Gerenciador de Recursos | Criptografado  |
| Uso geral V1   | Blob, arquivo, fila, tabela e disco       | Standard, Premium           | N/D                                  | LRS, GRS, RA-GRS                                                   | Resource Manager, Clássico  | Criptografado  |
| Armazenamento de blob         | Blob (somente blobs de blocos e blobs de acréscimo) | Standard                    | Quente, frio, de arquivos<sup>3</sup>                            | LRS, GRS, RA-GRS                                                   | Gerenciador de Recursos  | Criptografado  |

<sup>1</sup>É recomendável usar o modelo de implantação do Azure Resource Manager. Ainda é possível criar contas de armazenamento que usam o modelo de implantação clássico em alguns locais, e as contas clássicas existentes ainda têm suporte. Para saber mais, confira [Azure Resource Manager vs. Implantação clássica: compreenda os modelos de implantação e o estado dos seus recursos](../../azure-resource-manager/resource-manager-deployment-model.md).

<sup>2</sup>Todas as contas de armazenamento são criptografadas usando a SSE (Criptografia do Serviço de Armazenamento) para dados em repouso. Para obter mais informações, consulte [Criptografia de serviço do Armazenamento do Azure para dados em repouso](storage-service-encryption.md).

<sup>3</sup>A camada de acesso aos arquivos está disponível somente no nível de blob individual, e não no nível da conta de armazenamento. Somente os blobs de bloco e de acréscimo podem ser arquivados. Para saber mais, confira [Armazenamento de Blobs do Azure: camadas de armazenamento quente, frio e de arquivos](../blobs/storage-blob-storage-tiers.md).

<sup>4</sup>O ZRS (armazenamento com redundância de zona) só está disponível para as contas de armazenamento padrão para uso geral v2. Para saber mais sobre o ZRS, confira [ZRS (armazenamento com redundância de zona): aplicativos do Armazenamento do Azure altamente disponíveis](storage-redundancy-zrs.md). Para saber mais sobre outras opções de replicação, confira [Replicação do Armazenamento do Azure](storage-redundancy.md).

### <a name="general-purpose-v2-accounts"></a>Contas para uso geral v2

As contas de armazenamento para uso geral v2 são compatíveis com os recursos mais recentes do Armazenamento do Azure e incorporam todas as funcionalidades das contas de armazenamento de blobs e para uso geral v1. As contas de armazenamento para uso geral v2 têm os menores preços de capacidade por gigabyte para o Armazenamento do Azure, bem como os preços de transação competitivos no setor. As contas de armazenamento para uso geral v2 são compatíveis com estes serviços do Armazenamento do Azure:

- BLOBs (todos os tipos: bloco, acrescentar, página)
- Arquivos
- Discos
- Filas
- Tabelas

> [!NOTE]
> A Microsoft recomenda o uso de contas de armazenamento para uso geral v2 na maioria dos cenários. É possível atualizar facilmente de uma conta de armazenamento de blobs ou para uso geral v1 para uma v2 sem tempo de inatividade e sem a necessidade de copiar dados.
>
> Para saber mais sobre o upgrade para uma conta para uso geral v2, confira [Atualizar para uma conta de armazenamento de para geral v2](storage-account-upgrade.md). 

As contas de armazenamento para uso geral v2 oferecem várias camadas de acesso para armazenamento de dados com base nos seus padrões de uso. Para saber mais, confira [Camadas de acesso para dados de blob de blocos](#access-tiers-for-block-blob-data).

### <a name="general-purpose-v1-accounts"></a>Contas para uso geral v1

As contas para uso geral v1 permitem acesso a todos os serviços de Armazenamento do Azure, mas talvez não tenham os recursos mais recentes ou o menor preço por gigabyte. As contas de armazenamento para uso geral v1 são compatíveis com estes serviços do Armazenamento do Azure:

- Blobs (todos os tipos)
- Arquivos
- Discos
- Filas
- Tabelas

Embora as contas para uso geral v2 sejam recomendadas na maioria dos casos, as contas para uso geral v1 são mais adequadas para estes cenários: 

* O modelo de implantação clássico do Azure é obrigatório para os aplicativos. As contas para uso geral v2 e de Armazenamento de Blobs só são compatíveis com o modelo de implantação do Azure Resource Manager.

* Os aplicativos fazem uso intensivo de transações ou de largura de banda de replicação geográfica, mas não exigem grande capacidade. Nesse caso, o uso geral v1 pode ser a opção mais econômica.

* Você usa uma versão da [API REST dos Serviços de Armazenamento](https://msdn.microsoft.com/library/azure/dd894041.aspx) que é anterior a 2014-02-14 ou uma biblioteca de cliente com uma versão inferior a 4.x e não pode atualizar o aplicativo.

### <a name="blob-storage-accounts"></a>Contas de armazenamento de Blobs

Uma conta de armazenamento de blobs é uma conta de armazenamento especializada para armazenar dados de objetos não estruturados, como blobs de blocos. As contas de armazenamento de blobs oferecem a mesma durabilidade, disponibilidade, escalabilidade e recursos de desempenho disponíveis nas contas de armazenamento para uso geral v2. As contas de armazenamento de blobs são compatíveis com os blobs de bloco e os blobs de acréscimo, e não com os blobs de página.

As contas de armazenamento de blobs oferecem várias camadas de acesso para armazenamento de dados com base nos seus padrões de uso. Para saber mais, confira [Camadas de acesso para dados de blob de blocos](#access-tiers-for-block-blob-data).

## <a name="naming-storage-accounts"></a>Nomear contas de armazenamento

Ao nomear sua conta de armazenamento, lembre-se dessas regras:

- Os nomes da conta de armazenamento devem ter entre 3 e 24 caracteres e podem conter apenas números e letras minúsculas.
- O nome da sua conta de armazenamento deve ser exclusivo no Azure. Duas contas de armazenamento não podem ter o mesmo nome.

## <a name="performance-tiers"></a>Níveis de desempenho

As contas de armazenamento para uso geral podem ser configuradas para qualquer um dos seguintes níveis de desempenho:

* Um nível de desempenho padrão para armazenamento de blobs, arquivos, tabelas, filas e discos da máquina virtual do Azure.
* Um nível de desempenho premium somente para armazenamento de discos de máquina virtual do Azure. Confira [Armazenamento Premium: Armazenamento de Alto Desempenho para as Cargas de Trabalho da Máquina Virtual do Azure](../../virtual-machines/windows/premium-storage.md) para ter uma visão geral detalhada do Armazenamento Premium.

## <a name="access-tiers-for-block-blob-data"></a>Camadas de acesso para dados de blob de blocos

O Armazenamento do Azure oferece diferentes opções para acessar dados de blob de blocos baseados em padrões de uso. Cada camada de acesso no Armazenamento do Azure é otimizada para um determinado padrão de uso de dados. Ao selecionar a camada de acesso correta para suas necessidades, é possível armazenar seus dados de blob de blocos da maneira mais econômica.

As camadas de acesso disponíveis são:

> [!NOTE]
> A [camada de acesso Premium](../blobs/storage-blob-storage-tiers.md#premium-access-tier) está disponível em versão prévia limitada como uma conta de LRS (armazenamento com redundância local) nas regiões da Europa Setentrional, Leste dos EUA 2, Centro dos EUA e Oeste dos EUA. Para saber como se registrar para a versão prévia, confira [Introdução ao Armazenamento de Blobs Premium do Azure](https://aka.ms/premiumblob).

* A camada de acesso **quente** é otimizada para acesso frequente a objetos na conta de armazenamento. Acessar dados na camada de acesso quente é mais econômico, pois os custos de armazenamento são um pouco mais altos. Por padrão, as novas contas de armazenamento são criadas na camada de acesso quente.
* A camada de acesso **frio** é otimizada para armazenar grandes quantidades de dados acessados com menos frequência e armazenados por pelo menos 30 dias. Armazenar dados na camada de acesso frio é mais econômico, mas acessá-los pode ser um pouco mais caro que acessar os dados da camada de acesso quente.
* A camada de acesso aos **Arquivos** só está disponível para blobs de blocos individuais. Essa camada é otimizada para dados que podem tolerar várias horas de latência de recuperação e permanecerão na camada de acesso aos arquivos por pelo menos 180 dias. A camada de acesso aos arquivos é a opção mais econômica para armazenar dados, mas acessá-los é mais caro que acessar os dados das camadas de acesso quente ou frio. 


Se houver uma alteração no padrão de uso dos dados, você poderá alternar entre as camadas de acesso a qualquer momento. Para saber mais sobre as camadas de acesso, confira [Armazenamento de Blobs do Azure: camadas de armazenamento quente, frio, de arquivos e Premium (versão prévia)](../blobs/storage-blob-storage-tiers.md).

> [!IMPORTANT]
> Alterar a camada de acesso para uma conta de armazenamento existente ou um blob pode resultar em cobranças adicionais. Para obter mais informações, consulte a [Seção de cobrança de conta de armazenamento](#storage-account-billing).



## <a name="replication"></a>Replicação

[!INCLUDE [storage-common-redundancy-options](../../../includes/storage-common-redundancy-options.md)]

Para saber mais sobre a replicação de armazenamento, confira [Replicação do Armazenamento do Azure](storage-redundancy.md).

## <a name="encryption"></a>Criptografia

Todos os dados da sua conta de armazenamento são criptografados no lado do serviço. Para saber mais sobre a criptografia, confira [Criptografia do Serviço de Armazenamento do Azure para dados em repouso](storage-service-encryption.md).

## <a name="storage-account-endpoints"></a>Pontos de extremidade da conta de armazenamento

Uma conta de armazenamento fornece um namespace exclusivo no Azure para seus dados. Todos os objetos armazenados no Armazenamento do Azure têm um endereço que inclui o nome exclusivo da conta. A combinação do nome da conta e do ponto de extremidade de serviço do Armazenamento do Azure forma os pontos de extremidade da sua conta de armazenamento.

Por exemplo, se a conta de armazenamento para uso geral chamar *mystorageaccount*, os pontos de extremidade padrão da conta serão:

* Armazenamento de blobs: http://*mystorageaccount*.blob.core.windows.net
* Armazenamento de tabelas: http://*mystorageaccount*.table.core.windows.net
* Armazenamento de filas: http://*mystorageaccount*.queue.core.windows.net
* Arquivos do Azure: http://*mystorageaccount*.file.core.windows.net

> [!NOTE]
> Uma conta de armazenamento de blobs expõe apenas o ponto de extremidade do serviço Blob.

A URL para acessar um objeto em uma conta de armazenamento é criada acrescentando o local do objeto na conta de armazenamento ao ponto de extremidade. Por exemplo, um endereço de blob pode ter este formato: http://*mystorageaccount*.blob.core.windows.net/*mycontainer*/*myblob*.

Também é possível configurar um domínio personalizado para blobs para usar com a conta de armazenamento. Para saber mais, confira [Configurar um nome de domínio personalizado para a conta do Armazenamento do Azure](../blobs/storage-custom-domain-name.md).  

## <a name="control-access-to-account-data"></a>Controlar o acesso aos dados da conta

Por padrão, os dados em sua conta estão disponíveis somente para você, o proprietário da conta. Você controla quem pode acessar os dados e quais permissões essas pessoas têm.

Todas as solicitações feitas na conta de armazenamento precisam ser autorizadas. No nível do serviço, a solicitação precisa incluir um cabeçalho de *autorização* válido, que inclui todas as informações necessárias para o serviço validar a solicitação antes de executá-la.

É possível permitir acesso aos dados da conta de armazenamento usando uma das seguintes abordagens:

- **Azure Active Directory:** use as credenciais do Azure AD (Azure Active Directory) para autenticar um usuário, grupo ou outra identidade para acesso aos dados de blob e fila (versão prévia). Se a autenticação de uma identidade for bem-sucedida, o Azure AD retornará um token para usar ao autorizar a solicitação de acesso ao armazenamento de blobs ou de filas do Azure. Para saber mais, confira [Autenticar o acesso ao Armazenamento do Azure usando o Azure Active Directory (versão prévia)](storage-auth-aad.md).
- **Autorização de chave compartilhada:** use a chave de acesso da conta de armazenamento para criar uma cadeia de conexão para o aplicativo usar em tempo de execução para acessar o Armazenamento do Azure. Os valores na cadeia de conexão são usados para criar o cabeçalho de *autorização* passado para o Armazenamento do Azure. Para saber mais, confira [Configurar cadeias de conexão do Armazenamento do Azure](storage-configure-connection-string.md).
- **Assinatura de acesso compartilhado:** use a assinatura de acesso compartilhado para delegar acesso a recursos na conta de armazenamento, caso não esteja usando a autenticação do Azure AD. Uma assinatura de acesso compartilhado é um token que encapsula todas as informações necessárias para autorizar uma solicitação para o Armazenamento do Azure na URL. É possível especificar o recurso de armazenamento, as permissões concedidas e o intervalo em que as permissões serão válidas como parte da assinatura de acesso compartilhado. Para saber mais, confira [Usar SAS (Assinaturas de Acesso Compartilhado)](storage-dotnet-shared-access-signature-part-1.md) para saber mais.

> [!NOTE]
> Autenticar usuários ou aplicativos usando as credenciais do Azure AD oferece mais segurança e facilidade ao usar os meios de autorização. Enquanto você pode continuar a usar a autorização de chave compartilhada com seus aplicativos, usando o AD do Azure evita a necessidade de armazenar sua chave de acesso da conta com o seu código. Também é possível continuar a usar assinaturas de acesso compartilhado (SAS) para conceder acesso refinado a recursos em sua conta de armazenamento, mas o Azure AD oferece recursos semelhantes sem a necessidade de gerenciar tokens SAS ou se preocupar sobre revogar uma SAS comprometida. 
>
> A Microsoft recomenda usar autenticação do Azure AD para os aplicativos de blobs e filas do Armazenamento do Azure quando possível.

## <a name="copying-data-into-a-storage-account"></a>Copiar dados para uma conta de armazenamento

A Microsoft oferece utilitários e bibliotecas para importar dados de dispositivos de armazenamento local ou provedores de armazenamento em nuvem de terceiros. A solução que você usa depende da quantidade de dados que você transfere. 

Ao atualizar uma conta para uso geral v1 para uma v2 ou de Armazenamento de Blobs, os dados são automaticamente migrados. A Microsoft recomenda seguir esse caminho para fazer upgrade da sua conta. No entanto, se você decidir mover seus dados de uma conta para uso geral v1 para uma de armazenamento de blobs, precisará migrar os dados manualmente usando as ferramentas e bibliotecas descritas abaixo. 

### <a name="azcopy"></a>AzCopy

O AzCopy é um utilitário de linha de comando do Windows desenvolvido para cópia de dados de alto desempenho para dentro e para fora do Armazenamento do Azure. É possível usar o AzCopy para copiar dados para uma conta de armazenamento de blobs de uma conta de armazenamento para uso geral existente ou carregar dados de dispositivos de armazenamento local. Para obter mais informações, consulte [Transferir dados com o Utilitário de Linha de Comando AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

### <a name="data-movement-library"></a>Biblioteca de movimentação de dados

A biblioteca de movimentação de dados do Armazenamento do Azure para .NET baseia-se no framework de movimentação de dados de núcleo que habilita o AzCopy. A biblioteca foi projetada para operações de transferência de dados de alto desempenho, confiáveis e fáceis, de forma semelhante ao AzCopy. Isso permite que você aproveite todos os benefícios dos recursos fornecidos pelo AzCopy em seu aplicativo de forma nativa, sem a necessidade de lidar com a execução e o monitoramento de instâncias externas do AzCopy. Para obter mais detalhes, veja [Biblioteca de Movimentação dos Dados do Armazenamento do Azure para .Net](https://github.com/Azure/azure-storage-net-data-movement)

### <a name="rest-api-or-client-library"></a>API REST ou biblioteca de cliente

Você pode criar um aplicativo personalizado para migrar os dados para uma conta de armazenamento de Blobs usando uma das bibliotecas de cliente do Azure ou a API REST dos serviços de armazenamento do Azure. O Armazenamento do Azure fornece bibliotecas de cliente avançadas para várias linguagens e plataformas, como .NET, Java, C++, Node.JS, PHP, Ruby e Python. As bibliotecas de cliente oferecem recursos avançados, como lógica de recuperação, registro em log e carregamentos paralelos. Você também pode desenvolver diretamente na API REST, que pode ser chamada por qualquer linguagem que faça solicitações HTTP/HTTPS.

Para saber mais sobre a API REST do Armazenamento do Azure, confira a [Referência da API REST dos Serviços do Armazenamento do Azure](https://docs.microsoft.com/rest/api/storageservices/). 

> [!IMPORTANT]
> Blobs criptografados usando metadados relacionados à criptografia de armazenamento no lado do cliente com o blob. Se você copiar um blob que é criptografado com criptografia do lado do cliente, verifique se que a operação de cópia preserva os metadados de blob e especialmente os metadados relacionados à criptografia. Se você copiar um blob sem os metadados de criptografia, o conteúdo do blob não poderá ser recuperado novamente. Para obter mais detalhes sobre os metadados relacionados à criptografia, confira [Criptografia no Lado do Cliente do Armazenamento do Azure](../common/storage-client-side-encryption.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

### <a name="azure-importexport-service"></a>Serviço de Importação/exportação do Azure

Se você tiver uma grande quantidade de dados a serem importados para sua conta de armazenamento, considere o serviço de Importação/Exportação do Azure. O serviço de Importação/Exportação do Azure permite importar com segurança grandes quantidades de dados para o armazenamento de blobs do Azure e Arquivos do Azure por meio do envio de unidades de disco rígido para um data center do Azure. 

Esse serviço também pode ser usado para transferir dados do armazenamento de Blobs do Azure para as unidades de disco e enviar para seus sites locais. Os dados de uma ou mais unidades de disco podem ser importados para o armazenamento de Blobs do Azure ou para os Arquivos do Azure. Para saber mais, confira [O que é o serviço de Importação/Exportação do Azure?](https://docs.microsoft.com/azure/storage/common/storage-import-export-service).

## <a name="storage-account-billing"></a>Cobrança de conta de armazenamento

[!INCLUDE [storage-account-billing-include](../../../includes/storage-account-billing-include.md)]

## <a name="next-steps"></a>Próximas etapas

* Para saber como criar uma conta de armazenamento do Azure, confira [Criar uma conta de armazenamento](storage-quickstart-create-account.md).
* Para gerenciar ou excluir uma conta de armazenamento existente, confira [Gerenciar contas de armazenamento do Azure](storage-account-manage.md).
