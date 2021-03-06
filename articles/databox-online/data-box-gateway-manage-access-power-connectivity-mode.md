---
title: O acesso ao dispositivo Gateway do Microsoft Azure Data Box, energia e acesso ao modo conectividade | Microsoft Docs
description: Descreve como gerenciar o acesso, a potência e o modo de conectividade para o dispositivo de Gateway do Azure Data Box que ajuda a transferir dados para o Azure
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: article
ms.date: 11/09/2018
ms.author: alkohli
ms.openlocfilehash: 8f9172418f15b129a71242038efd4cdb7683bbf7
ms.sourcegitcommit: 5a1d601f01444be7d9f405df18c57be0316a1c79
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2018
ms.locfileid: "51516373"
---
# <a name="manage-access-power-and-connectivity-mode-for-your-azure-data-box-gateway-preview"></a>Gerenciar o acesso, energia e modo de conectividade para o Gateway do Azure Data Box (visualização)

Este artigo descreve como gerenciar o modo de acesso, energia e conectividade para o Gateway do Azure Data Box. Essas operações são executadas por meio da interface do usuário da web local ou o portal do Azure.

Neste artigo, você aprenderá a:

> [!div class="checklist"]
> * Gerenciar o acesso de dispositivo
> * Gerenciar o modo de conectividade
> * Gerenciar potência

> [!IMPORTANT]
> O Gateway do Data Box está em versão prévia. Examine os [termos de serviço do Azure para a versão prévia](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) antes de solicitar e implantar essa solução.

## <a name="manage-device-access"></a>Gerenciar o acesso de dispositivo

O acesso ao seu dispositivo de Gateway do Azure Data Box é controlado pelo uso de uma senha de administrador do dispositivo. Você pode alterar a senha do administrador por meio da interface do usuário da web local. Você também pode redefinir a senha do administrador do dispositivo no portal do Azure.

### <a name="change-device-administrator-password"></a>Alterar senha de administrador do dispositivo

Se você esqueceu sua senha, então você pode mudar a senha. Siga estas etapas na interface do usuário local para alterar a senha do administrador do dispositivo.

1. Na interface do usuário de web local, vá para **manutenção > alteração de senha**.
2. Digite a senha atual e, em seguida, a nova senha. A senha fornecida deve ter entre 8 e 16 caracteres. A senha deve ter 3 dos seguintes caracteres: maiúscula, minúscula, numérica e caracteres especiais. Confirme a nova senha.

    ![Alterar senha](media/data-box-gateway-manage-access-power-connectivity-mode/change-password-1.png)

3. Clique em **alterar a senha**.
 
### <a name="reset-device-administrator-password"></a>Redefinir senha do administrador do dispositivo

O fluxo de trabalho de redefinição não exige que o usuário recupere a senha antiga e é útil quando a senha é perdida. Esse fluxo de trabalho é executado no portal do Azure.

1. No portal do Azure, acesse **visão geral > Redefinir senha de administrador**.

    ![Redefinir senha](media/data-box-gateway-manage-access-power-connectivity-mode/reset-password-1.png)

 
2. Digite a nova senha e confirme-a. A senha fornecida deve ter entre 8 e 16 caracteres. A senha deve ter 3 dos seguintes caracteres: maiúscula, minúscula, numérica e caracteres especiais. Clique em **redefinir**.

    ![Redefinir senha](media/data-box-gateway-manage-access-power-connectivity-mode/reset-password-2.png)

## <a name="manage-connectivity-mode"></a>Gerenciar o modo de conectividade

Além do modo normal padrão, o dispositivo também pode ser executado no modo parcialmente desconectado ou desconectado. Cada um desses modos é descrita como abaixo:

- **Parcialmente desconectado** - Nesse modo, o dispositivo não pode carregar dados para os compartilhamentos, mas pode ser gerenciado pelo portal do Azure.

    Esse modo é normalmente usado quando em uma rede de satélite medida e o objetivo é minimizar o consumo de largura de banda da rede. Consumo de rede mínima ainda poderão ocorrer para monitoramento de operações de dispositivo.

- **Disconnected** - Neste modo, o dispositivo é totalmente desconectado da nuvem e os uploads e downloads na nuvem são desativados. O dispositivo só pode ser gerenciado pela interface da web local.

    Esse modo é normalmente usado quando você deseja colocar o dispositivo offline.

Para alterar o modo de dispositivo, siga estas etapas:

1. Na interface do usuário da web local do seu dispositivo, acesse **Configuração> Configurações da nuvem**.
2. Desabilitar a **upload e download de nuvem**.
3. Para executar o dispositivo no modo desconectado parcialmente, habilite **gerenciamento do portal do Azure**.

    ![Modo de conectividade](media/data-box-gateway-manage-access-power-connectivity-mode/connectivity-mode-1.png)
 
4. Para executar o dispositivo no modo desconectado, desabilite **gerenciamento do portal do Azure**. Agora, o dispositivo só pode ser gerenciado por meio da interface do usuário da web local.

    ![Modo de conectividade](media/data-box-gateway-manage-access-power-connectivity-mode/connectivity-mode-2.png)

## <a name="manage-power"></a>Gerenciar potência

Você pode desligar ou reiniciar seu dispositivo físico e virtual usando a interface da Web local. Nós recomendamos que antes de reiniciar, você coloque os compartilhamentos offline no host e, em seguida, no dispositivo. Essa ação minimiza a possibilidade de corrupção de dados.

1. Na interface do usuário de web local, vá para **manutenção > configurações de energia**.
2. Clique em **desligamento** ou **reiniciar** dependendo do que você pretende fazer.

    ![Configurações de energia](media/data-box-gateway-manage-access-power-connectivity-mode/shut-down-restart-1.png)

3. Quando solicitado a confirmar, clique em **Sim** para continuar.

> [!NOTE]
> Se você desligar o dispositivo virtual, você precisará iniciar o dispositivo por meio do gerenciamento de hipervisor.