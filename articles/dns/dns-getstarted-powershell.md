---
title: Início Rápido - Criar uma zona DNS do Azure e o registro usando o Azure PowerShell
description: Saiba como criar uma zona e registro DNS no DNS do Azure. Este é um início rápido passo a passo para criar e gerenciar sua primeira zona e registro DNS usando o Azure PowerShell.
services: dns
author: vhorne
ms.service: dns
ms.topic: quickstart
ms.date: 07/16/2018
ms.author: victorh
ms.openlocfilehash: e5801e9ed512a32d793f7b4b71be86174f656ab0
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39089971"
---
# <a name="quickstart-create-an-azure-dns-zone-and-record-using-azure-powershell"></a>Início Rápido: Criar uma zona DNS do Azure e o registro usando o Azure PowerShell

Neste início rápido, você pode criar sua primeira zona e registro DNS usando o Azure PowerShell. Você também pode executar essas etapas usando o [portal do Azure](dns-getstarted-portal.md) ou a [CLI do Azure](dns-getstarted-cli.md). 

Uma zona DNS é usada para hospedar os registros DNS para um domínio específico. Para iniciar a hospedagem do seu domínio no DNS do Azure, você precisará criar uma zona DNS para esse nome de domínio. Cada registro DNS para seu domínio é criado dentro dessa zona DNS. Por fim, para publicar sua zona DNS na Internet, você precisa configurar os servidores de nome para o domínio. Cada uma dessas etapas é descrita abaixo.

O DNS do Azure também oferece suporte à criação de domínios privados. Para obter instruções passo a passo sobre como criar sua primeira zona DNS privada e o registro, confira [Introdução às zonas privadas do Azure DNS usando o PowerShell](private-dns-getstarted-powershell.md).

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="create-the-resource-group"></a>Criar o grupo de recursos

Antes de criar a zona DNS, crie um grupo de recursos para conter a zona DNS:

```powershell
New-AzureRMResourceGroup -name MyResourceGroup -location "eastus"
```

## <a name="create-a-dns-zone"></a>Criar uma zona DNS

Uma zona DNS é criada usando o cmdlet `New-AzureRmDnsZone` . O exemplo a seguir cria uma zona DNS chamada *contoso.com* no grupo de recursos chamado *MyResourceGroup*. Use o exemplo para criar uma zona DNS, substituindo os valores pelos seus próprios.

```powershell
New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyResourceGroup
```

## <a name="create-a-dns-record"></a>Criar um registro DNS

Você cria conjuntos de registros usando o cmdlet `New-AzureRmDnsRecordSet`. O exemplo a seguir cria um registro com o nome relativo "www" na Zona DNS "contoso.com", no grupo de recursos "MyResourceGroup". O nome totalmente qualificado do conjunto de registros é "www.contoso.com". O tipo de registro é "A", com o endereço IP "1.2.3.4" e a TTL de 3600 segundos.

```powershell
New-AzureRmDnsRecordSet -Name www -RecordType A -ZoneName contoso.com -ResourceGroupName MyResourceGroup -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -IPv4Address "1.2.3.4")
```

## <a name="view-records"></a>Exibir registros

Para listar os registros DNS em sua zona, use:

```powershell
Get-AzureRmDnsRecordSet -ZoneName contoso.com -ResourceGroupName MyResourceGroup
```


## <a name="update-name-servers"></a>Atualizar servidores de nome

Quando você estiver satisfeito com a configuração de sua zona e registros DNS, configure seu nome de domínio para usar os servidores de nome DNS do Azure. Isso permite que outros usuários na Internet encontrem os registros DNS.

Os servidores de nomes de sua zona são fornecidos pelo cmdlet `Get-AzureRmDnsZone`:

```powershell
Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyResourceGroup

Name                  : contoso.com
ResourceGroupName     : myresourcegroup
Etag                  : 00000003-0000-0000-b40d-0996b97ed101
Tags                  : {}
NameServers           : {ns1-01.azure-dns.com., ns2-01.azure-dns.net., ns3-01.azure-dns.org., ns4-01.azure-dns.info.}
NumberOfRecordSets    : 3
MaxNumberOfRecordSets : 5000
```

Esses servidores de nome devem ser configurados com o registrador de nome de domínio (onde você adquiriu o nome de domínio). Seu registrador oferecerá a opção de configurar os servidores de nome do domínio. Para saber mais, confira [Delegar um domínio ao DNS do Azure](dns-domain-delegation.md).

## <a name="delete-all-resources"></a>Excluir todos os recursos

Quando não forem mais necessários, você poderá excluir todos os recursos criados neste início rápido ao excluir o grupo de recursos:

```powershell
Remove-AzureRMResourceGroup -Name MyResourceGroup
```

## <a name="next-steps"></a>Próximas etapas

Agora que você criou sua primeira zona e registro DNS usando o Azure PowerShell, pode criar registros para um aplicativo Web em um domínio personalizado.

> [!div class="nextstepaction"]
> [Criar registros DNS para um aplicativo Web em um domínio personalizado](./dns-web-sites-custom-domain.md)

