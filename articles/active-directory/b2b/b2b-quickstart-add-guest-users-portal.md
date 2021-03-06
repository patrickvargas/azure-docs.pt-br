---
title: 'Início rápido: adicionar usuários convidados no portal do Azure'
description: Use este guia de início rápido para saber como os administradores do Azure AD podem adicionar usuários convidados B2B no portal do Azure e veja um passo a passo do fluxo de trabalho de convite B2B.
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: quickstart
ms.date: 07/02/2018
ms.author: mimart
author: msmimart
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: ed49eed6dbfce9a71f13770e3ddcaec6557cf875
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/18/2018
ms.locfileid: "45986538"
---
# <a name="quickstart-add-guest-users-to-your-directory-in-the-azure-portal"></a>Início rápido: adicionar usuários convidados ao diretório no portal do Azure

É possível convidar qualquer pessoa para colaborar com sua organização. Basta adicioná-la ao seu diretório como usuário convidado. Em seguida, você pode enviar um email de convite que contém um link de resgate ou enviar um link direto para um aplicativo que você deseja compartilhar. Os usuários convidados podem entrar com as próprias identidades empresariais, estudantis ou sociais.

Neste início rápido, você adicionará um novo usuário convidado ao Azure AD, enviará um convite e acompanhará o processo de resgate de convites dos usuários convidados.

Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir o cenário deste tutorial, você precisa de:

 - Uma função que permita criar usuários no diretório do locatário, como a função de administrador global ou qualquer uma das funções de administração limitada do diretório.
 - Uma conta de email válida que pode ser adicionada ao diretório do locatário e usada para receber o email de convite de teste.

## <a name="add-a-new-guest-user-in-azure-ad"></a>Adicionar um novo usuário convidado no Azure AD

1. Entre no [Portal do Azure](https://portal.azure.com/) como administrador do Microsoft Azure AD.
2. No painel esquerdo, selecione **Azure Active Directory**.
3.  Em **Gerenciar**, selecione **Usuários**.

    ![Selecione Azure Active Directory](media/quickstart-add-users-portal/quickstart-users-portal-user.png)

4.  Selecione **Novo usuário convidado**.

    ![Selecione Azure Active Directory](media/quickstart-add-users-portal/quickstart-users-portal-user-3.png)

5.  Em **Nome de usuário**, insira o endereço de email do usuário externo. Em **Incluir uma mensagem pessoal com o convite**, digite uma mensagem de boas-vindas. 

    ![Selecione Azure Active Directory](media/quickstart-add-users-portal/quickstart-users-portal-user-4.png)

6. Selecione **Convidar** para enviar automaticamente o convite ao usuário convidado. Uma notificação será exibida no canto superior direito com a mensagem **Usuário convidado com êxito**. 
7.  Depois de enviar o convite, a conta de usuário é automaticamente adicionada ao diretório como convidado.

## <a name="assign-an-app-to-the-guest-user"></a>Atribuir um aplicativo ao usuário convidado
Adicione o aplicativo do Salesforce ao locatário de teste e atribua o usuário convidado de teste ao aplicativo.
1.  Entre no portal do Azure como administrador do Azure AD.
2.  No painel esquerdo, selecione **Aplicativos Empresariais**.
3.  Selecione **Novo aplicativo**.
4. Em **Adicionar por meio da galeria**, pesquise **Salesforce** e, em seguida, selecione-o.

    ![Selecione Azure Active Directory](media/quickstart-add-users-portal/quickstart-users-portal-select-salesforce.png)
5. Selecione **Adicionar**.
6. Em **Gerenciar**, selecione **Logon único**. Em **Modo de Logon Único**, selecione **Logon baseado em Senha** e clique em **Salvar**.
7. Em **Gerenciar**, selecione **Usuários e grupos** > **Adicionar usuário** > **Usuários e grupos**.
8. Use a caixa de pesquisa para pesquisar o usuário de teste (se necessário) e selecione o usuário de teste na lista. Em seguida, clique em **Selecionar**.
9. Selecione **Atribuir**. 

## <a name="accept-the-invitation"></a>Aceitar o convite
Agora, entre como usuário convidado para ver o convite.
1.  Entre na conta de email do usuário convidado de teste.
2.  Na caixa de entrada, encontre o email de "Você foi convidado".

    ![Email de convite de B2B](media/quickstart-add-users-portal/quickstart-users-portal-email-small.png)

3.  No corpo do email, selecione **Começar**. Uma página **Analisar as permissões** será aberta no navegador. 

    ![Página de convite para aceitar B2B](media/quickstart-add-users-portal/quickstart-users-portal-accept.png)

4. Selecione **Aceitar**. O Painel de Acesso é aberto e lista os aplicativos que o usuário convidado pode acessar.

## <a name="clean-up-resources"></a>Limpar recursos
Exclua o usuário convidado de teste e o aplicativo de teste quando não forem mais necessários.
1.  Entre no portal do Azure como administrador do Azure AD.
2.  No painel esquerdo, selecione **Azure Active Directory**.
3.  Em **Gerenciar**, selecione **Aplicativos empresariais**.
4.  Abra o aplicativo **Salesforce** e, em seguida, selecione **Excluir**.
5.  No painel esquerdo, selecione **Azure Active Directory**.
6.  Em **Gerenciar**, selecione **Usuários**.
7.  Selecione o usuário de teste e, em seguida, selecione **Excluir usuário**.

## <a name="next-steps"></a>Próximas etapas
Neste tutorial, você criou um usuário convidado no portal do Azure e enviou um convite para compartilhar aplicativos. Em seguida, você exibiu o processo de resgate da perspectiva do usuário convidado e conferiu se o aplicativo apareceu no Painel de Acesso do usuário convidado. Para saber mais sobre como adicionar usuários convidados para colaboração, confira [Adicionar usuários de colaboração do Azure Active Directory B2B no portal do Azure](add-users-administrator.md).