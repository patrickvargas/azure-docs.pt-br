---
title: Administradores gerenciam usuários e dispositivos - Azure MFA | Microsoft Docs
description: Isso descreve como alterar as configurações de usuário, como forçar os usuários a fazer o processo de verificação novamente.
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: michmcla
ms.openlocfilehash: cf470ad14c27fd36cf60bb540e823b15fc58eded
ms.sourcegitcommit: 345b96d564256bcd3115910e93220c4e4cf827b3
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2018
ms.locfileid: "52496908"
---
# <a name="manage-user-settings-with-azure-multi-factor-authentication-in-the-cloud"></a>Gerenciar configurações de usuário com a Autenticação Multifator do Azure na nuvem

Como administrador, você pode gerenciar as seguintes configurações de usuário e dispositivo:

* Exigir que os usuários forneçam métodos de contato novamente
* Excluir senhas de aplicativo
* Exigir a MFA em todos os dispositivos confiáveis 

## <a name="require-users-to-provide-contact-methods-again"></a>Exigir que os usuários forneçam métodos de contato novamente
Essa configuração força o usuário a concluir o processo de registro novamente. Aplicativos sem navegador continuam funcionando se o usuário tiver senhas de aplicativos para eles.  Você pode excluir as senhas de aplicativos de usuários selecionando também **Excluir todas as senhas de aplicativo existentes geradas pelos usuários selecionados**.

### <a name="how-to-require-users-to-provide-contact-methods-again"></a>Como exigir que os usuários selecionados forneçam métodos de contato novamente
1. Entre no [Portal do Azure](https://portal.azure.com).
2. À esquerda, selecione **Azure Active Directory** > **Usuários e grupos** > **Todos os usuários**.
3. Selecione **Autenticação Multifator**. Abre a página de autenticação multifator. 
4. Marque a caixa ao lado do usuário ou usuários que você deseja gerenciar. Uma lista de opções de etapa rápida aparecem à direita. 
5. Selecione **Gerenciar configurações de usuário**.
6. Marque a caixa **Exigir que os usuários selecionados forneçam métodos de contato novamente**.
   ![Fornecer métodos de contato](./media/howto-mfa-userdevicesettings/reproofup.png)
7. Clique em **Salvar**.
8. Clique em **Fechar**.

## <a name="delete-users-existing-app-passwords"></a>Excluir senhas de aplicativo de usuários existentes
Essa configuração exclui todas as senhas de aplicativo que um usuário criou. Aplicativos sem navegador que estavam associados a essas senhas de aplicativo deixam de funcionar até que uma nova senha de aplicativo seja criada.

### <a name="how-to-delete-users-existing-app-passwords"></a>Como excluir senhas de aplicativo de usuários existentes
1. Entre no [Portal do Azure](https://portal.azure.com).
2. À esquerda, selecione **Azure Active Directory** > **Usuários e grupos** > **Todos os usuários**.
3. Selecione **Autenticação Multifator**. Abre a página de autenticação multifator. 
6. Marque a caixa ao lado do usuário ou usuários que você deseja gerenciar. Uma lista de opções de etapa rápida aparecem à direita. 
7. Selecione **Gerenciar configurações de usuário**.
8. Marque a caixa **Excluir todas as senhas de aplicativo existentes geradas pelos usuários selecionados**.
   ![Excluir senhas de aplicativo](./media/howto-mfa-userdevicesettings/deleteapppasswords.png)
9. Clique em **Salvar**.
10. Clique em **Fechar**.

## <a name="restore-mfa-on-all-remembered-devices-for-a-user"></a>Restaurar o MFA em todos os dispositivos lembrados de um usuário
Um dos recursos configuráveis da Autenticação Multifator do Azure está dando aos usuários a opção de marcar dispositivos como confiáveis. Para saber mais, confira [Definir as configurações da Autenticação Multifator do Azure](howto-mfa-mfasettings.md#remember-multi-factor-authentication).

Os usuários podem ignorar a verificação em duas etapas durante um período configurável em seus dispositivos regulares. Se uma conta for comprometida ou um dispositivo confiável for perdido, você precisa conseguir remover o status de confiável e exigir novamente a verificação em duas etapas.

A configuração **Restaurar a autenticação multifator em todos os dispositivos lembrados** significa que o usuário será desafiado a executar uma verificação de duas etapas na próxima vez que entrar, independentemente de ter optado por marcar seu dispositivo como confiável. 

### <a name="how-to-restore-mfa-on-all-suspended-devices-for-a-user"></a>Como Restaurar a MFA em todos os dispositivos suspensos para um usuário
1. Entre no [Portal do Azure](https://portal.azure.com).
2. À esquerda, selecione **Azure Active Directory** > **Usuários e grupos** > **Todos os usuários**.
3. Selecione **Autenticação Multifator**. Abre a página de autenticação multifator. 
6. Marque a caixa ao lado do usuário ou usuários que você deseja gerenciar. Uma lista de opções de etapa rápida aparecem à direita. 
7. Selecione **Gerenciar configurações de usuário**.
8. Marque a caixa **Restaurar a autenticação multifator em todos os dispositivos lembrados**
   ![Excluir senhas do aplicativo](./media/howto-mfa-userdevicesettings/rememberdevices.png)
9. Clique em **Salvar**.
10. Clique em **Fechar**.

## <a name="next-steps"></a>Próximas etapas

- Para saber mais sobre como [Definir as configurações da Autenticação Multifator do Azure](howto-mfa-mfasettings.md)

- Se os usuários precisarem de ajuda, direcione-os para [Guia do usuário para verificação em duas etapas](../user-help/multi-factor-authentication-end-user.md)
