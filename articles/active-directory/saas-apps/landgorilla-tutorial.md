---
title: 'Tutorial: integração do Azure Active Directory com o Land Gorilla Client | Microsoft Docs'
description: Saiba como configurar o logon único entre o Azure Active Directory e o Land Gorilla.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/13/2017
ms.author: jeedes
ms.openlocfilehash: e93c4721f34b06fec853d876543e9939220efd9f
ms.sourcegitcommit: 4eddd89f8f2406f9605d1a46796caf188c458f64
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/11/2018
ms.locfileid: "49116429"
---
# <a name="tutorial-azure-active-directory-integration-with-land-gorilla-client"></a>Tutorial: integração do Azure Active Directory com o Land Gorilla Client

Neste tutorial, você aprenderá a integrar o Land Gorilla Client ao Azure AD (Azure Active Directory).

A integração do Land Gorilla Client ao Azure AD oferece os seguintes benefícios:

- É possível controlar, no Azure AD, quem tem acesso ao Land Gorilla Client
- É possível permitir que seus usuários façam logon automaticamente no Land Gorilla Client (Logon Único) com suas contas do Azure AD
- Você pode gerenciar suas contas em um único local - o portal de Gerenciamento do Azure

Para conhecer mais detalhadamente a integração de aplicativos de SaaS ao AD do Azure, consulte [O que é o acesso a aplicativos e logon único com o Active Directory do Azure](../manage-apps/what-is-single-sign-on.md).


## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o Land Gorilla Client, são necessários os seguintes itens:

- Uma assinatura do AD do Azure
- Uma assinatura do Land Gorilla Client habilitada para logon único


> [!NOTE]
> Para testar as etapas deste tutorial, nós não recomendamos o uso de um ambiente de produção.


Para testar as etapas deste tutorial, você deve seguir estas recomendações:

- Não use o ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do AD do Azure, você pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, você testará o logon único do Azure AD em um ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Land Gorilla Client da galeria
1. configurar e testar o logon único do AD do Azure


## <a name="adding-land-gorilla-client-from-the-gallery"></a>Adicionando Land Gorilla Client da galeria
Para configurar a integração do Land Gorilla Client com o Azure AD, é necessário adicionar o Land Gorilla Client da galeria à sua lista de aplicativos SaaS gerenciados.

**Para adicionar o Land Gorilla Client da galeria, siga as etapas abaixo:**

1. No **[Portal de Gerenciamento do Azure](https://portal.azure.com)**, no painel navegação à esquerda, clique no ícone **Azure Active Directory**. 

    ![Active Directory][1]

1. Navegue até **aplicativos empresariais**. Em seguida, vá para **todos os aplicativos**.

    ![APLICATIVOS][2]
    
1. Clique em **adicionar** botão na parte superior da caixa de diálogo.

    ![APLICATIVOS][3]

1. Na caixa de pesquisa, digite **Land Gorilla Client**.

    ![Criação de um usuário de teste do AD do Azure](./media/landgorilla-tutorial/tutorial_landgorilla_search.png)

1. No painel de resultados, selecione **Land Gorilla Client** e, em seguida, clique no botão **Adicionar** para adicionar o aplicativo.

    ![Criação de um usuário de teste do AD do Azure](./media/landgorilla-tutorial/tutorial_landgorilla_addfromgallery.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>configurar e testar o logon único do AD do Azure
Nesta seção, você configurará e testará o logon único do Azure AD com o Land Gorilla Client, com base em um usuário de teste chamado "Brenda Fernandes".

Para que o logon único funcione, o Azure AD precisa saber qual usuário do Land Gorilla Client é equivalente a um usuário do Azure AD. Em outras palavras, é necessário estabelecer uma relação de vínculo entre um usuário do Azure AD e o usuário relacionado no Land Gorilla Client.

Essa relação de vínculo é estabelecida por meio da atribuição do valor do **nome de usuário** no Azure AD como o valor de **Nome de Usuário** no Land Gorilla Client.

Para configurar e testar o logon único do Azure AD com o Land Gorilla Client, é necessário concluir os seguintes blocos de construção:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - para habilitar seus usuários a usar esse recurso.
1. **[Criando um usuário de teste do Azure AD](#creating-an-azure-ad-test-user)** – para testar o logon único do Azure AD com o grupo limitado.
1. **[Criando um usuário de teste do Land Gorilla](#creating-a-land-gorilla-test-user)** – para testar o logon único do Azure AD com Brenda Fernandes.
1. **[Atribuição do usuário de teste do AD do Azure](#assigning-the-azure-ad-test-user)** – para permitir que Brenda Fernandes use o logon único do AD do Azure.
1. **[Teste do logon único](#testing-single-sign-on)** : para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do logon único do Azure AD

Nesta seção, você habilitará o logon único do Azure AD no Portal de Gerenciamento do Azure e configurará o logon único em seu aplicativo Land Gorilla Client.

**Para configurar o logon único do Azure AD com o Land Gorilla Client, siga as etapas abaixo:**

1. No Portal de Gerenciamento do Azure, na página de integração de aplicativos do **Land Gorilla Client**, clique em **Logon único**.

    ![Configurar o logon único][4]

1. Na caixa de diálogo **Logon único**, como **Modo**, selecione **Logon baseado em SAML** para habilitar o logon único.
 
    ![Configurar o logon único](./media/landgorilla-tutorial/tutorial_landgorilla_samlbase.png)

1. Na seção **URLs e Domínio do Land Gorilla Client**, siga as etapas abaixo:

    ![Configurar o logon único](./media/landgorilla-tutorial/tutorial_landgorilla_url_02.png)

    a. Na caixa de texto **Identificador**, digite um valor usando o seguinte padrão: 
    
    `https://<customer domain>.landgorilla.com/` 
    
    `https://www.<customer domain>.landgorilla.com`

    b. Na caixa de texto **URL de Resposta**, digite uma URL usando um dos seguintes padrões:

    `https://<customer domain>.landgorilla.com/simplesaml/module.php/core/authenticate.php`

    `https://www.<customer domain>.landgorilla.com/simplesaml/module.php/core/authenticate.php`

    `https://<customer domain>.landgorilla.com/simplesaml/module.php/saml/sp/saml2-acs.php/default-sp`
    
    `https://www.<customer domain>.landgorilla.com/simplesaml/module.php/saml/sp/saml2-acs.php/default-sp`

    > [!NOTE] 
    > Observe que esses não são os valores reais. Você precisa atualizar esses valores com o Identificador e a URL de Resposta reais. Aqui, sugerimos que você use o valor exclusivo de cadeia de caracteres no Identificador. Contate a [equipe Land Gorilla Client](https://www.landgorilla.com/support/) para obter esses valores. 

1. Na seção **Certificado de Autenticação SAML**, clique em **Metadados XML** e, em seguida, salve o arquivo XML em seu computador.

    ![Configurar o logon único](./media/landgorilla-tutorial/tutorial_landgorilla_certificate.png) 

1. Clique no botão **Salvar** .

    ![Configurar o logon único](./media/landgorilla-tutorial/tutorial_general_400.png) 

1. Para concluir a configuração de SSO para seu aplicativo na extremidade do Land Gorilla, contate a [equipe de suporte Land Gorilla Client](https://www.landgorilla.com/support/) e forneça-as com o arquivo **XML de metadados** baixado.


### <a name="creating-an-azure-ad-test-user"></a>Criação de um usuário de teste do AD do Azure
O objetivo desta seção é criar um usuário de teste no Portal de Gerenciamento do Azure chamado Britta Simon.

![Criar um usuário do AD do Azure][100]

**Para criar um usuário de teste no AD do Azure, execute as seguintes etapas:**

1. No **portal de Gerenciamento do Azure**, no painel navegação à esquerda, clique em **Azure Active Directory**.

    ![Criação de um usuário de teste do AD do Azure](./media/landgorilla-tutorial/create_aaduser_01.png) 

1. Vá para **usuários e grupos** e clique em **todos os usuários** para exibir a lista de usuários.
    
    ![Criação de um usuário de teste do AD do Azure](./media/landgorilla-tutorial/create_aaduser_02.png) 

1. Na parte superior da caixa de diálogo clique **adicionar** para abrir o **usuário** caixa de diálogo.
 
    ![Criação de um usuário de teste do AD do Azure](./media/landgorilla-tutorial/create_aaduser_03.png) 

1. Na página do diálogo **Usuário**, execute as seguintes etapas:
 
    ![Criação de um usuário de teste do AD do Azure](./media/landgorilla-tutorial/create_aaduser_04.png) 

    a. Na caixa de texto **Nome**, digite **Brenda Fernandes**.

    b. Na caixa de texto **Nome de usuário**, digite o **endereço de email** da conta de Brenda Fernandes.

    c. Selecione **Mostrar senha** e anote o valor de **senha**.

    d. Clique em **Criar**. 

### <a name="creating-a-land-gorilla-test-user"></a>Criar um usuário de teste do Land Gorilla

Trabalhe com a [equipe de suporte Land Gorilla](https://www.landgorilla.com/support/) para adicionar os usuários na plataforma do Land Gorilla.
    
### <a name="assigning-the-azure-ad-test-user"></a>Atribuição do usuário de teste do AD do Azure

Nesta seção, você permitirá que Brenda Fernandes use o logon único do Azure, concedendo-lhe acesso ao Land Gorilla Client.

![Atribuir usuário][200] 

**Para atribuir Brenda Fernandes ao Land Gorilla Client, siga as etapas abaixo:**

1. No Portal de Gerenciamento do Azure, abra a exibição de aplicativos e, em seguida, navegue até o modo de exibição de diretório e vá para **Aplicativos empresariais**, depois clique em **Todos os aplicativos**.

    ![Atribuir usuário][201] 

1. Na lista de aplicativos, selecione **Land Gorilla Client**.

    ![Configurar o logon único](./media/landgorilla-tutorial/tutorial_landgorilla_app.png) 

1. No menu à esquerda, clique em **usuários e grupos**.

    ![Atribuir usuário][202] 

1. Clique no botão **Adicionar**. Em seguida, selecione **usuários e grupos** na **Adicionar atribuição** caixa de diálogo.

    ![Atribuir usuário][203]

1. Em **usuários e grupos** caixa de diálogo, selecione **Britta Simon** na lista de usuários.

1. Clique em **selecione** botão **usuários e grupos** caixa de diálogo.

1. Clique em **atribuir** botão **Adicionar atribuição** caixa de diálogo.
    


### <a name="testing-single-sign-on"></a>Teste do logon único

Nesta seção, você testará sua configuração de logon único do Azure AD usando o Painel de Acesso.

Ao clicar no bloco Land Gorilla Client no Painel de Acesso, seu logon deverá ser feito automaticamente no aplicativo Land Gorilla Client.


## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure](tutorial-list.md)
* [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/landgorilla-tutorial/tutorial_general_01.png
[2]: ./media/landgorilla-tutorial/tutorial_general_02.png
[3]: ./media/landgorilla-tutorial/tutorial_general_03.png
[4]: ./media/landgorilla-tutorial/tutorial_general_04.png

[100]: ./media/landgorilla-tutorial/tutorial_general_100.png
[200]: ./media/landgorilla-tutorial/tutorial_general_200.png
[201]: ./media/landgorilla-tutorial/tutorial_general_201.png
[202]: ./media/landgorilla-tutorial/tutorial_general_202.png
[203]: ./media/landgorilla-tutorial/tutorial_general_203.png
