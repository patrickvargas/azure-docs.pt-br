---
title: Criar um gateway de rede virtual com redundância de zona em Zonas de Disponibilidade do Azure | Microsoft Docs
description: Implantar Gateway de VPN e gateways ExpressRoute em Zonas de Disponibilidade
services: vpn-gateway
author: cherylmc
Customer intent: As someone with a basic network background, I want to understand how to create zone-redundant gateways.
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 09/21/2018
ms.author: cherylmc
ms.openlocfilehash: 400ac3232b7e7dcd594734d818e3cafef09188fd
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/13/2018
ms.locfileid: "51614541"
---
# <a name="create-a-zone-redundant-virtual-network-gateway-in-azure-availability-zones"></a>Criar um gateway de rede virtual com redundância de zona em Zonas de Disponibilidade do Azure

Você pode implantar gateways VPN e ExpressRoute nas zonas de disponibilidade do Azure. Isso traz resiliência, escalabilidade e maior disponibilidade para gateways de rede virtual. A implantação de gateways em Zonas de Disponibilidade do Azure separa de forma física e lógica os gateways em uma região, enquanto protege a conectividade de rede local com o Azure de falhas no nível na zona. Para obter informações, veja [Sobre gateways de rede virtual com redundância de zona](about-zone-redundant-vnet-gateways.md) e [Sobre Zonas de Disponibilidade do Azure](../availability-zones/az-overview.md).

## <a name="before-you-begin"></a>Antes de começar

Você pode usar o PowerShell instalado localmente em seu computador ou o Azure Cloud Shell. Se você optar por instalar e usar o PowerShell localmente, esse recurso exigirá a versão mais recente do módulo do PowerShell.

[!INCLUDE [Cloud shell](../../includes/vpn-gateway-cloud-shell-powershell.md)]

### <a name="to-use-powershell-locally"></a>Para usar o PowerShell localmente

Se você estiver usando o PowerShell localmente no seu computador em vez de usar o Cloud Shell, será necessário instalar o módulo do PowerShell 6.1.1 ou superior. Para verificar a versão do PowerShell que você instalou, use o comando a seguir:

```azurepowershell
Get-Module AzureRM -ListAvailable | Select-Object -Property Name,Version,Path
```

Se você precisa atualizar, consulte [Instalar o módulo do Azure PowerShell](/powershell/azure/install-azurerm-ps).

[!INCLUDE [PowerShell login](../../includes/vpn-gateway-ps-login-include.md)]

## <a name="variables"></a>1. Declare as variáveis

Os valores usados para as etapas de exemplo estão listados abaixo. Além disso, alguns dos exemplos usam variáveis declaradas nas etapas. Se você estiver usando estas etapas em seu próprio ambiente, certifique-se de substituir esses valores pelos seus. Ao especificar a localização, verifique se a região especificada é compatível. Consulte mais informações em [Perguntas Frequentes](#faq).

```azurepowershell-interactive
$RG1         = "TestRG1"
$VNet1       = "VNet1"
$Location1   = "CentralUS"
$FESubnet1   = "FrontEnd"
$BESubnet1   = "Backend"
$GwSubnet1   = "GatewaySubnet"
$VNet1Prefix = "10.1.0.0/16"
$FEPrefix1   = "10.1.0.0/24"
$BEPrefix1   = "10.1.1.0/24"
$GwPrefix1   = "10.1.255.0/27"
$Gw1         = "VNet1GW"
$GwIP1       = "VNet1GWIP"
$GwIPConf1   = "gwipconf1"
```

## <a name="configure"></a>2. Criar a rede virtual

Crie um grupos de recursos.

```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName $RG1 -Location $Location1
```

Crie uma rede virtual.

```azurepowershell-interactive
$fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubnet1 -AddressPrefix $FEPrefix1
$besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubnet1 -AddressPrefix $BEPrefix1
$vnet = New-AzureRmVirtualNetwork -Name $VNet1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNet1Prefix -Subnet $fesub1,$besub1
```

## <a name="gwsub"></a>3. Adicionar a sub-rede de gateway

A sub-rede de gateway contém os endereços IP reservados que usam os serviços de gateway de rede virtual. Use os exemplos a seguir para adicionar e configurar uma sub-rede de gateway:

Adicione a sub-rede de gateway.

```azurepowershell-interactive
$getvnet = Get-AzureRmVirtualNetwork -ResourceGroupName $RG1 -Name VNet1
Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.1.255.0/27 -VirtualNetwork $getvnet
```

Defina a configuração de sub-rede do gateway para a rede virtual.

```azurepowershell-interactive
$getvnet | Set-AzureRmVirtualNetwork
```
## <a name="publicip"></a>4. Solicitar um endereço IP público
 
Nesta etapa, escolha as instruções que se aplicam ao gateway que você deseja criar. A seleção de zonas para implantar os gateways depende das zonas especificadas para o endereço IP público.

### <a name="ipzoneredundant"></a>Para gateways com redundância de zona

Solicite um endereço IP público com a SKU PublicIpaddress **Standard** e não especifique nenhuma zona. Nesse caso, o endereço IP público Standard criado será um IP público com redundância de zona.   

```azurepowershell-interactive
$pip1 = New-AzureRmPublicIpAddress -ResourceGroup $RG1 -Location $Location1 -Name $GwIP1 -AllocationMethod Static -Sku Standard
```

### <a name="ipzonalgw"></a>Para gateways em zona

Solicite um endereço IP público com a SKU PublicIpaddress **Standard**. Especifique a zona (1, 2 ou 3). Todas as instâncias de gateway serão implantadas nesta zona.

```azurepowershell-interactive
$pip1 = New-AzureRmPublicIpAddress -ResourceGroup $RG1 -Location $Location1 -Name $GwIP1 -AllocationMethod Static -Sku Standard -Zone 1
```

### <a name="ipregionalgw"></a>Para gateways regionais

Solicite um endereço IP público com a SKU PublicIpaddress **Básica**. Nesse caso, o gateway é implantado como um gateway regional e não tem nenhuma redundância de zona integrada no gateway. As instâncias de gateway são criadas em quaisquer as zonas, respectivamente.

```azurepowershell-interactive
$pip1 = New-AzureRmPublicIpAddress -ResourceGroup $RG1 -Location $Location1 -Name $GwIP1 -AllocationMethod Dynamic -Sku Basic
```
## <a name="gwipconfig"></a>5. Criar a configuração de IP

```azurepowershell-interactive
$getvnet = Get-AzureRmVirtualNetwork -ResourceGroupName $RG1 -Name $VNet1
$subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name $GwSubnet1 -VirtualNetwork $getvnet
$gwipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GwIPConf1 -Subnet $subnet -PublicIpAddress $pip1
```

## <a name="gwconfig"></a>6. Criar o gateway

Crie o gateway de rede virtual.

### <a name="for-expressroute"></a>Para ExpressRoute

```azurepowershell-interactive
New-AzureRmVirtualNetworkGateway -ResourceGroup $RG1 -Location $Location1 -Name $Gw1 -IpConfigurations $GwIPConf1 -GatewayType ExpressRoute
```

### <a name="for-vpn-gateway"></a>Para Gateway de VPN

```azurepowershell-interactive
New-AzureRmVirtualNetworkGateway -ResourceGroup $RG1 -Location $Location1 -Name $Gw1 -IpConfigurations $GwIPConf1 -GatewayType Vpn -VpnType RouteBased
```

## <a name="faq"></a>Perguntas frequentes

### <a name="what-will-change-when-i-deploy-these-new-skus"></a>O que mudará quando eu implantar essas novas SKUs?

Da sua perspectiva, você poderá implantar seus gateways com redundância de zona. Isso significa que todas as instâncias dos gateways serão implantadas em Zonas de Disponibilidade do Azure e cada Zona de Disponibilidade será um domínio de atualização e falha diferente. Isso torna seus gateways mais confiáveis, disponíveis e resilientes a falhas de zona.

### <a name="can-i-use-the-azure-portal"></a>Posso usar o portal do Azure?

Sim, você pode usar o portal do Azure para implantar as novas SKUs. No entanto, você verá essas novas SKUs somente em regiões do Azure com Zonas de Disponibilidade do Azure.

### <a name="what-regions-are-available-for-me-to-use-the-new-skus"></a>Quais regiões estão disponíveis para eu usar as novas SKUs?

As novas SKUs estão disponíveis nas regiões do Azure que têm Zonas de Disponibilidade do Azure – EUA Central, França Central e Europa Ocidental. No futuro, disponibilizaremos os gateways com redundância de zona em outras Regiões Públicas do Azure.

### <a name="can-i-changemigrateupgrade-my-existing-virtual-network-gateways-to-zone-redundant-or-zonal-gateways"></a>Posso alterar/migrar/atualizar meus gateways de rede virtual existentes para gateways com redundância de zona ou em zona?

No momento, a migração de gateways de rede virtual existentes para gateways com redundância de zona ou em zona não é compatível. No entanto, você pode excluir o gateway existente e recriar um gateway com redundância de zona ou em zona.

### <a name="can-i-deploy-both-vpn-and-express-route-gateways-in-same-virtual-network"></a>Posso implantar gateways de VPN e ExpressRoute na mesma rede virtual?

A coexistência de gateways de VPN e do ExpressRoute na mesma rede virtual é compatível. No entanto, reserve um intervalo de endereços IP /27 para a sub-rede de gateway.
