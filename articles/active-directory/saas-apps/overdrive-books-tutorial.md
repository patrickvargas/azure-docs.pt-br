---
title: 'Tutorial: Integração do Azure Active Directory com o Overdrive | Microsoft Docs'
description: Saiba como configurar o logon único entre o Azure Active Directory e o Overdrive.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: e68cede7-1130-4813-bd55-22a9a6fc6bf5
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: 3d4df5db47cb8007599ca2a09608b23f382e3fbb
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39420542"
---
# <a name="tutorial-azure-active-directory-integration-with-overdrive"></a>Tutorial: Integração do Azure Active Directory com o Overdrive 

Neste tutorial, você aprende a integrar o Overdrive ao Azure AD (Azure Active Directory).

A integração do Overdrive ao Azure AD oferece os seguintes benefícios:

- Você pode controlar no Azure AD quem tem acesso ao Overdrive 
- Você pode permitir que seus usuários façam logon automaticamente no Overdrive (logon único) com as contas do Azure AD deles
- Você pode gerenciar suas contas em um única localização: o Portal do Azure

Para conhecer mais detalhadamente a integração de aplicativos de SaaS ao Azure AD, consulte [o que é o acesso a aplicativos e logon único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o Overdrive, você precisará dos seguintes itens:

- Uma assinatura do AD do Azure
- Uma assinatura habilitada para logon único do Overdrive

> [!NOTE]
> Para testar as etapas deste tutorial, nós não recomendamos o uso de um ambiente de produção.

Para testar as etapas deste tutorial, você deve seguir estas recomendações:

- Não use o ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do AD do Azure, você pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, você testará o logon único do Azure AD em um ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionar o Overdrive da galeria
1. configurar e testar o logon único do AD do Azure

## <a name="adding-overdrive-from-the-gallery"></a>Adicionar o Overdrive da galeria
Para configurar a integração do Overdrive ao Azure AD, você precisa adicionar o Overdrive na galeria à sua lista de aplicativos SaaS gerenciados.

**Para adicionar o Overdrive por meio da galeria, execute as seguintes etapas:**

1. No **[Portal do Azure](https://portal.azure.com)**, no painel navegação à esquerda, clique no ícone **Azure Active Directory**. 

    ![Active Directory][1]

1. Navegue até **aplicativos empresariais**. Em seguida, vá para **todos os aplicativos**.

    ![APLICATIVOS][2]
    
1. Clique no botão **Novo aplicativo** na parte superior da caixa de diálogo para adicionar o novo aplicativo.

    ![APLICATIVOS][3]

1. Na caixa de pesquisa, digite **Overdrive**.

    ![Criação de um usuário de teste do AD do Azure](./media/overdrive-books-tutorial/tutorial_overdrivebooks_search.png)

1. No painel de resultados, selecione **Overdrive** e, em seguida, clique no botão **Adicionar** para adicionar o aplicativo.

    ![Criação de um usuário de teste do AD do Azure](./media/overdrive-books-tutorial/tutorial_overdrivebooks_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>configurar e testar o logon único do AD do Azure
Nesta seção, você configurará e testará o logon único do Azure AD com o Overdrive com base em um usuário de teste chamado “Brenda Fernandes”.

Para que o logon único funcione, o Azure AD precisa saber qual usuário do Overdrive é equivalente a um usuário do Azure AD. Em outras palavras, é necessário estabelecer uma relação de vínculo entre um usuário do Azure AD e o usuário relacionado do Overdrive.

No Overdrive, atribua o valor do **nome de usuário** no Azure AD como o valor do **Nome de usuário** para estabelecer a relação de vínculo.

Para configurar e testar o logon único do Azure AD com o Overdrive, você precisa concluir os seguintes blocos de construção:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - para habilitar seus usuários a usar esse recurso.
1. **[Criação de um usuário de teste do AD do Azure](#creating-an-azure-ad-test-user)** – para testar o logon único do AD do Azure com Brenda Fernandes.
1. **[Criação de um usuário de teste do Overdrive](#creating-an-overdrive-test-user)** – para ter um equivalente de Brenda Fernandes no Overdrive que esteja vinculado à representação de usuário do Azure AD.
1. **[Atribuição do usuário de teste do AD do Azure](#assigning-the-azure-ad-test-user)** – para permitir que Brenda Fernandes use o logon único do AD do Azure.
1. **[Teste do logon único](#testing-single-sign-on)** : para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do logon único do Azure AD

Nesta seção, você habilitará o logon único do Azure AD no Portal do Azure e configurará o logon único em seu aplicativo Overdrive.

**Para configurar o logon único do Azure AD com o Overdrive, execute as seguintes etapas:**

1. No Portal do Azure, na página de integração de aplicativos do **Overdrive**, clique em **Logon único**.

    ![Configurar o logon único][4]

1. Na caixa de diálogo **Logon único**, selecione **Modo** como **Logon baseado em SAML** para habilitar o logon único.
 
    ![Configurar o logon único](./media/overdrive-books-tutorial/tutorial_overdrivebooks_samlbase.png)

1. Na seção **URLs e Domínio do Overdrive**, execute as seguintes etapas:

    ![Configurar o logon único](./media/overdrive-books-tutorial/tutorial_overdrivebooks_url.png)

    Na caixa de texto **URL de Logon**, digite uma URL usando o seguinte padrão: `http://<subdomain>.libraryreserve.com`

    > [!NOTE] 
    > O valor não é real. Atualize o valor com a URL de Logon real. Entre em contato com a [equipe de suporte ao cliente do Overdrive](https://help.overdrive.com/) para obter o valor. 
 
1. Na seção **Certificado de Autenticação SAML**, clique em **Metadados XML** e, em seguida, salve o arquivo de metadados em seu computador.

    ![Configurar o logon único](./media/overdrive-books-tutorial/tutorial_overdrivebooks_certificate.png) 

1. Clique no botão **Salvar** .

    ![Configurar o logon único](./media/overdrive-books-tutorial/tutorial_general_400.png)

1. Para configurar o logon único no lado do **Overdrive**, é necessário enviar o **XML de Metadados** baixado para a [equipe de suporte do Overdrive](https://help.overdrive.com/). Eles definem essa configuração para ter a conexão de SSO de SAML definida corretamente em ambos os lados.

> [!TIP]
> É possível ler uma versão concisa dessas instruções no [Portal do Azure](https://portal.azure.com), enquanto você estiver configurando o aplicativo!  Depois de adicionar esse aplicativo da seção **Active Directory > Aplicativos Empresariais**, basta clicar na guia **Logon Único** e acessar a documentação inserida por meio da seção **Configuração** na parte inferior. Saiba mais sobre a funcionalidade de documentação inserida aqui: [Documentação inserida do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Criação de um usuário de teste do AD do Azure
O objetivo desta seção é criar um usuário de teste no Portal do Azure chamado Brenda Fernandes.

![Criar um usuário do AD do Azure][100]

**Para criar um usuário de teste no AD do Azure, execute as seguintes etapas:**

1. No **Portal do Azure**, no painel de navegação esquerdo, clique no ícone **Azure Active Directory**.

    ![Criação de um usuário de teste do AD do Azure](./media/overdrive-books-tutorial/create_aaduser_01.png) 

1. Vá para **Usuários e grupos** e clique em **Todos os usuários** para exibir a lista de usuários.
    
    ![Criação de um usuário de teste do AD do Azure](./media/overdrive-books-tutorial/create_aaduser_02.png) 

1. Para abrir a caixa de diálogo **Usuário**, clique em **Adicionar** na parte superior da caixa de diálogo.
 
    ![Criação de um usuário de teste do AD do Azure](./media/overdrive-books-tutorial/create_aaduser_03.png) 

1. Na página do diálogo **Usuário**, execute as seguintes etapas:
 
    ![Criação de um usuário de teste do AD do Azure](./media/overdrive-books-tutorial/create_aaduser_04.png) 

    a. Na caixa de texto **Nome**, digite **Brenda Fernandes**.

    b. Na caixa de texto **Nome de usuário**, digite o **endereço de email** da conta de Brenda Fernandes.

    c. Selecione **Mostrar senha** e anote o valor de **senha**.

    d. Clique em **Criar**.
 
### <a name="creating-an-overdrive-test-user"></a>Criação de um usuário de teste do Overdrive

Não há nenhum item de ação para a configuração de provisionamento de usuário para o OverDrive.  

Quando um usuário atribuído tentar fazer logon no OverDrive, uma conta do OverDrive será automaticamente criada, se necessário.

>[!NOTE]
>É possível usar qualquer outra ferramenta de criação da conta de usuário do OverDrive ou as APIs fornecidas pelo OverDrive para provisionar as contas de usuário do AAD.
>

### <a name="assigning-the-azure-ad-test-user"></a>Atribuição do usuário de teste do AD do Azure

Nesta seção, você permitirá que Brenda Fernandes use o logon único do Azure concedendo acesso ao Overdrive.

![Atribuir usuário][200] 

**Para atribuir Brenda Fernandes ao Overdrive, execute as seguintes etapas:**

1. No Portal do Azure, abra a exibição de aplicativos e, em seguida, navegue até a exibição de diretório e vá para **Aplicativos Empresariais** e clique em **Todos os aplicativos**.

    ![Atribuir usuário][201] 

1. Na lista de aplicativos, selecione **Overdrive**.

    ![Configurar o logon único](./media/overdrive-books-tutorial/tutorial_overdrivebooks_app.png) 

1. No menu à esquerda, clique em **usuários e grupos**.

    ![Atribuir usuário][202] 

1. Clique no botão **Adicionar**. Em seguida, selecione **usuários e grupos** na **Adicionar atribuição** caixa de diálogo.

    ![Atribuir usuário][203]

1. Em **usuários e grupos** caixa de diálogo, selecione **Britta Simon** na lista de usuários.

1. Clique em **selecione** botão **usuários e grupos** caixa de diálogo.

1. Clique em **atribuir** botão **Adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste do logon único

O objetivo desta seção é testar sua configuração de logon único do Azure AD usando o Painel de Acesso.

Ao clicar no bloco do Overdrive no Painel de Acesso, você deverá ser conectado automaticamente ao seu aplicativo Overdrive.

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure](tutorial-list.md)
* [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/overdrive-books-tutorial/tutorial_general_01.png
[2]: ./media/overdrive-books-tutorial/tutorial_general_02.png
[3]: ./media/overdrive-books-tutorial/tutorial_general_03.png
[4]: ./media/overdrive-books-tutorial/tutorial_general_04.png

[100]: ./media/overdrive-books-tutorial/tutorial_general_100.png

[200]: ./media/overdrive-books-tutorial/tutorial_general_200.png
[201]: ./media/overdrive-books-tutorial/tutorial_general_201.png
[202]: ./media/overdrive-books-tutorial/tutorial_general_202.png
[203]: ./media/overdrive-books-tutorial/tutorial_general_203.png

