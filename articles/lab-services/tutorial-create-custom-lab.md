---
title: Criar um laboratório usando o Azure DevTest Labs | Microsoft Docs
description: Neste início rápido, você criará um laboratório usando o Azure DevTest Labs.
services: devtest-lab, lab-services, virtual-machines
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 05/17/2018
ms.author: spelluru
ms.openlocfilehash: c3eecfdf4bc2e6bf6798a6b3845cdc2e2e243341
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/19/2018
ms.locfileid: "49467422"
---
# <a name="tutorial-set-up-a-lab-by-using-azure-devtest-labs"></a>Tutorial: Configurar um laboratório usando o Azure DevTest Labs
Neste tutorial, você criará um laboratório usando o Portal do Azure. Um administrador de laboratório define um laboratório em uma organização, cria máquinas virtuais no laboratório e configura políticas. Os usuários de laboratório (por exemplo: desenvolvedores e testadores) solicitam máquinas virtuais no laboratório, as conectam e as usam. 

Neste tutorial, você executa as seguintes ações:

> [!div class="checklist"]
> * Criar um laboratório
> * Adicionar máquinas virtuais (VM) ao laboratório
> * Adicionar um usuário à função de usuário de laboratório

Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="create-a-lab"></a>Criar um laboratório
As etapas a seguir ilustram como usar o portal do Azure para criar um laboratório no Azure DevTest Labs. 

1. Entre no [Portal do Azure](https://portal.azure.com).
2. No menu principal no lado esquerdo, selecione **Criar um recurso** (na parte superior da lista), aponte para **Ferramentas de desenvolvedor**e clique em **DevTest Labs**. 

    ![Menu novo DevTest Lab](./media/tutorial-create-custom-lab/new-custom-lab-menu.png)
1. Na janela **Criar um DevTest Lab**, execute as seguintes ações: 
    1. Para **Nome do laboratório**, insira um nome para o laboratório. 
    2. Para **Assinatura**, selecione a assinatura na qual você deseja criar o laboratório. 
    3. Para **Grupo de Recursos**, selecione **Criar novo** e digite um nome para o grupo de recursos. 
    4. Para **Local**, selecione o local/região em que você deseja que o laboratório seja criado. 
    5. Selecione **Criar**. 
    6. Selecione **Fixar no painel**. Depois de criar o laboratório, o laboratório aparece no painel. 

        ![Criar uma seção de laboratório do DevTest Labs](./media/tutorial-create-custom-lab/create-custom-lab-blade.png)

## <a name="add-a-vm-to-the-lab"></a>Adicionar uma VM ao laboratório

1. Na página **DevTest Lab**, selecione **+ Adicionar** na barra de ferramentas. 

    ![Botão Adicionar](./media/tutorial-create-custom-lab/add-vm-to-lab-button.png)
1. Na página **Escolher uma base**, pesquisa com a palavra-chave (por exemplo: Windows, Ubuntu) e selecione uma das imagens de base na lista. 
1. Na página **Máquina virtual**, execute as seguintes ações: 
    1. Para **Nome da máquina virtual**, insira um nome para a máquina virtual. 
    2. Para **Nome de usuário**, insira um nome para o usuário que tem acesso à máquina virtual. 
    3. Para **Digite um valor**, insira a senha para o usuário. 
    4. Selecione **Configurações avançadas**.
    5. Para **Tornar essa máquina solicitável**, selecione **Sim**.
    6. Confirme se a **contagem de instâncias** está definida como **1**. Se você a definiu como **2**, são criadas 2 VMs com nomes: `<base image name>00' and <base image name>01`. Por exemplo: `win10vm00` e `win10vm01`. 
    7. Para fechar a página **Avançado**, clique em **OK**. 
    8. Selecione **Criar**. 

        ![Escolher uma base](./media/tutorial-create-custom-lab/new-virtual-machine.png)
    9. Você consulta o status da VM na lista de **máquinas de virtuais solicitáveis**. A criação da máquina virtual pode levar aproximadamente 25 minutos. A VM é criada em um grupo de recursos separado do Azure, cujo nome começa com o nome do grupo de recursos atual que tem o laboratório. Por exemplo, se o laboratório está em `labrg`, a VM pode ser criada no grupo de recursos `labrg3988722144002`. 

        ![Status de criação da VM](./media/tutorial-create-custom-lab/vm-creation-status.png)
1. Depois que a VM é criada, você a vê na lista de **máquinas de virtuais solicitáveis** na lista. 

    > [!NOTE] 
    > Ao adicionar uma VM Linux a um laboratório, você pode habilitar o acesso a SSH e RDP à VM. Se não habilitar o acesso ao criar a VM, você pode adicionar regras manualmente no grupo de segurança de rede associado à VM para abrir portas para o SSH e RDP.

## <a name="add-a-user-to-the-lab-user-role"></a>Adicionar um usuário à função de usuário de laboratório

1. Selecione **Configuração e políticas** no menu à esquerda. 

    ![Configuração e políticas](./media/tutorial-create-custom-lab/configuration-and-policies-menu.png)
1. Selecione **Controle de acesso (IAM)** no menu e selecione **+ Adicionar** na barra de ferramentas. 

    ![Controle de acesso - Botão adicionar usuário](./media/tutorial-create-custom-lab/access-control-add.png)
1. Na página **Adicionar permissões**, execute as ações a seguir:
    1. Para **Função**, selecione **Usuário do DevTest Labs**. 
    2. Selecione o **usuário** que deseja adicionar. 
    3. Clique em **Salvar**.

        ![Adicionar permissões](./media/tutorial-create-custom-lab/add-lab-user.png)
4. Para fechar **Configuração e políticas - Controle de acesso (IAM)**, selecione **X** no canto direito. 

## <a name="cleanup-resources"></a>Recursos de limpeza
O seguinte tutorial mostra como um usuário de laboratório pode solicitar e se conectar a uma máquina virtual no laboratório. Se você não quiser fazer esse tutorial e limpar os recursos criados como parte deste tutorial, siga estas etapas: 

1. No portal do Azure, selecione **Grupos de recursos** no menu. 
2. Selecione o grupo de recursos em que você criou o laboratório. 
3. Selecione **Excluir grupo de recursos** na barra de ferramentas. Excluir um grupo de recursos excluirá todos os recursos no grupo, incluindo o laboratório. 
4. Repita essas etapas para excluir o grupo de recurso adicional criado para você com o nome `<your resource group name><random numbers>`. Por exemplo: `splab3988722144001`. As VMs são criadas no grupo de recursos, em vez de no grupo de recursos no qual existe o laboratório. 

## <a name="next-steps"></a>Próximas etapas
Neste tutorial, você criou um laboratório com uma VM e forneceu um acesso de usuário ao laboratório. Para saber mais sobre como acessar o laboratório como um usuário de laboratório, vá para o seguinte tutorial:

> [!div class="nextstepaction"]
> [Tutorial: Acessar o laboratório](tutorial-use-custom-lab.md)

