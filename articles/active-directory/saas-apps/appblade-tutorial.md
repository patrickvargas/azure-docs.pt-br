---
title: 'Tutorial: Integração do Azure Active Directory com o AppBlade | Microsoft Docs'
description: Saiba como configurar o logon único entre o Active Directory do Azure e o AppBlade.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 3360d7aa-6518-4f99-88bd-b7f7258183e8
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: 66c893a89138d7daf7d8118d8e2b1d8389d40ea4
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36229291"
---
# <a name="tutorial-azure-active-directory-integration-with-appblade"></a>Tutorial: Integração do Active Directory do Azure com o AppBlade

Neste tutorial, você aprende a integrar o AppBlade ao Azure AD (Azure Active Directory).

A integração do AppBlade ao Azure AD oferece os seguintes benefícios:

- É possível controlar no Azure AD quem tem acesso ao AppBlade
- É possível permitir que seus usuários façam logon automaticamente no AppBlade (logon único) com suas contas do Azure AD
- Você pode gerenciar suas contas em um única localização: o Portal do Azure

Para conhecer mais detalhadamente a integração de aplicativos de SaaS ao Azure AD, consulte [o que é o acesso a aplicativos e logon único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>pré-requisitos

Para configurar a integração do Azure AD com o AppBlade, você precisa dos seguintes itens:

- Uma assinatura do AD do Azure
- Uma assinatura habilitada para logon único do AppBlade

> [!NOTE]
> Para testar as etapas deste tutorial, nós não recomendamos o uso de um ambiente de produção.

Para testar as etapas deste tutorial, você deve seguir estas recomendações:

- Não use o ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do AD do Azure, você pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, você testará o logon único do Azure AD em um ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adição do AppBlade da galeria
2. configurar e testar o logon único do AD do Azure

## <a name="adding-appblade-from-the-gallery"></a>Adição do AppBlade da galeria
Para configurar a integração do AppBlade com o Azure AD, você precisa adicionar o AppBlade à sua lista de aplicativos SaaS gerenciados a partir da galeria.

**Para adicionar o AppBlade a partir da galeria, execute as seguintes etapas:**

1. No **[Portal do Azure](https://portal.azure.com)**, no painel navegação à esquerda, clique no ícone **Azure Active Directory**. 

    ![Active Directory][1]

2. Navegue até **aplicativos empresariais**. Em seguida, vá para **todos os aplicativos**.

    ![APLICATIVOS][2]
    
3. Clique no botão **Novo aplicativo** na parte superior da caixa de diálogo para adicionar o novo aplicativo.

    ![APLICATIVOS][3]

4. Na caixa de pesquisa, digite **AppBlade**.

    ![Criação de um usuário de teste do AD do Azure](./media/appblade-tutorial/tutorial_appblade_search.png)

5. No painel de resultados, selecione **AppBlade** e, depois, clique no botão **Adicionar** para adicionar o aplicativo.

    ![Criação de um usuário de teste do AD do Azure](./media/appblade-tutorial/tutorial_appblade_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>configurar e testar o logon único do AD do Azure
Nesta seção, você configura e testa o logon único do Azure AD com o AppBlade, com base em um usuário de teste chamado “Brenda Fernandes”.

Para que o logon único funcione, o Azure AD precisa saber qual usuário do AppBlade é equivalente a um usuário do Azure AD. Em outras palavras, é necessário estabelecer uma relação de vínculo entre um usuário do Azure AD e o usuário relacionado no AppBlade.

No AppBlade, atribua o valor do **nome de usuário** no Azure AD como o valor do **Nome de usuário** para estabelecer a relação de vínculo.

Para configurar e testar o logon único do Azure AD com o AppBlade, você precisa concluir os seguintes blocos de construção:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - para habilitar seus usuários a usar esse recurso.
2. **[Criação de um usuário de teste do AD do Azure](#creating-an-azure-ad-test-user)** : para testar o logon único do Azure AD com Brenda Fernandes.
3. **[Criando um usuário de teste do AppBlade](#creating-an-appblade-test-user)** – para ter um equivalente de Brenda Fernandes no AppBlade que esteja vinculado à representação de usuário do Azure AD.
4. **[Atribuição do usuário de teste do AD do Azure](#assigning-the-azure-ad-test-user)** – para permitir que Brenda Fernandes use o logon único do AD do Azure.
5. **[Teste do logon único](#testing-single-sign-on)** : para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do logon único do Azure AD

Nesta seção, você habilita o logon único do Azure AD no portal do Azure e configura o logon único no aplicativo AppBlade.

**Para configurar o logon único do Azure AD com o AppBlade, execute as seguintes etapas:**

1. No portal do Azure, na página de integração do aplicativo do **AppBlade**, clique em **Logon único**.

    ![Configurar o logon único][4]

2. Na caixa de diálogo **Logon único**, selecione **Modo** como **Logon baseado em SAML** para habilitar o logon único.
 
    ![Configurar o logon único](./media/appblade-tutorial/tutorial_appblade_samlbase.png)

3. Na seção **Domínio e URLs do AppBlade**, realize as seguintes etapas:

    ![Configurar o logon único](./media/appblade-tutorial/tutorial_appblade_url.png)

    Na caixa de texto **URL de Logon**, digite uma URL usando o seguinte padrão: `https://<companyname>.appblade.com/saml/<tenantid>`

    > [!NOTE] 
    > Esse valor não é real. Atualize o valor com a URL de Logon real. Contate a [equipe de suporte ao Cliente do AppBlade](mailto:support@appblade.com) para obter o valor. 
 
4. Na seção **Certificado de Autenticação SAML**, clique em **Metadados XML** e, em seguida, salve o arquivo de metadados em seu computador.

    ![Configurar o logon único](./media/appblade-tutorial/tutorial_appblade_certificate.png) 

5. Clique no botão **Salvar** .

    ![Configurar o logon único](./media/appblade-tutorial/tutorial_general_400.png)

6. Para configurar o logon único no lado do **AppBlade**, é necessário enviar o **XML de Metadados** baixado para a [equipe de suporte do AppBlade](mailto:support@appblade.com). Além disso, solicite a eles que configurem a **URL do Emissor de SSO** como `https://appblade.com/saml`. Essa configuração é necessária para que o logon único funcione.


> [!TIP]
> É possível ler uma versão concisa dessas instruções no [Portal do Azure](https://portal.azure.com), enquanto você estiver configurando o aplicativo!  Depois de adicionar esse aplicativo da seção **Active Directory > Aplicativos Empresariais**, basta clicar na guia **Logon Único** e acessar a documentação inserida por meio da seção **Configuração** na parte inferior. Saiba mais sobre a funcionalidade de documentação inserida aqui: [Documentação inserida do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
 
### <a name="creating-an-azure-ad-test-user"></a>Criação de um usuário de teste do AD do Azure
O objetivo desta seção é criar um usuário de teste no Portal do Azure chamado Brenda Fernandes.

![Criar um usuário do AD do Azure][100]

**Para criar um usuário de teste no AD do Azure, execute as seguintes etapas:**

1. No **Portal do Azure**, no painel de navegação esquerdo, clique no ícone **Azure Active Directory**.

    ![Criação de um usuário de teste do AD do Azure](./media/appblade-tutorial/create_aaduser_01.png) 

2. Vá para **Usuários e grupos** e clique em **Todos os usuários** para exibir a lista de usuários.
    
    ![Criação de um usuário de teste do AD do Azure](./media/appblade-tutorial/create_aaduser_02.png) 

3. Para abrir a caixa de diálogo **Usuário**, clique em **Adicionar** na parte superior da caixa de diálogo.
 
    ![Criação de um usuário de teste do AD do Azure](./media/appblade-tutorial/create_aaduser_03.png) 

4. Na página do diálogo **Usuário**, execute as seguintes etapas:
 
    ![Criação de um usuário de teste do AD do Azure](./media/appblade-tutorial/create_aaduser_04.png) 

    a. Na caixa de texto **Nome**, digite **Brenda Fernandes**.

    b. Na caixa de texto **Nome de usuário**, digite o **endereço de email** da conta de Brenda Fernandes.

    c. Selecione **Mostrar senha** e anote o valor de **senha**.

    d. Clique em **Criar**.
 
### <a name="creating-an-appblade-test-user"></a>Criando um usuário de teste do AppBlade

O objetivo desta seção é criar um usuário chamado Brenda Fernandes no AppBlade. O AppBlade dá suporte ao provisionamento just-in-time, que está habilitado por padrão. **Verifique se o nome de domínio está configurado no AppBlade para o provisionamento de usuário. Depois disso, somente o provisionamento de usuário Just-In-Time funciona.**

Se o usuário tiver um endereço de email que termina com o domínio configurado pelo AppBlade para sua conta, o usuário unirá automaticamente na conta como membro, com o nível de permissão especificado por você, que pode ser “Básico” (um usuário básico que só pode instalar aplicativos), “Membro da Equipe” (um usuário que pode carregar novas versões do aplicativo e gerenciar projetos) ou “Administrador” (privilégios totais de administrador na conta). O normal seria escolher Básico e promover os usuários manualmente por meio de um logon de administrador (o AppBlade precisa configurar antecipadamente um logon de administração baseado em email ou promover um usuário em nome do cliente após o logon).

Não há itens de ação para você nesta seção. Um novo usuário será criado durante uma tentativa de acessar o AppBlade, caso ele ainda não exista. 

> [!NOTE]
> Se precisar criar um usuário manualmente, será necessário contatar a [equipe de suporte do AppBlade](mailto:support@appblade.com).

### <a name="assigning-the-azure-ad-test-user"></a>Atribuição do usuário de teste do AD do Azure

Nesta seção, você permite que Brenda Fernandes use o logon único do Azure concedendo acesso ao AppBlade.

![Atribuir usuário][200] 

**Para atribuir Brenda Fernandes ao AppBlade, execute as seguintes etapas:**

1. No Portal do Azure, abra a exibição de aplicativos e, em seguida, navegue até a exibição de diretório e vá para **Aplicativos Empresariais** e clique em **Todos os aplicativos**.

    ![Atribuir usuário][201] 

2. Na lista de aplicativos, escolha **AppBlade**.

    ![Configurar o logon único](./media/appblade-tutorial/tutorial_appblade_app.png) 

3. No menu à esquerda, clique em **usuários e grupos**.

    ![Atribuir usuário][202] 

4. Clique no botão **Adicionar**. Em seguida, selecione **usuários e grupos** na **Adicionar atribuição** caixa de diálogo.

    ![Atribuir usuário][203]

5. Em **usuários e grupos** caixa de diálogo, selecione **Britta Simon** na lista de usuários.

6. Clique em **selecione** botão **usuários e grupos** caixa de diálogo.

7. Clique em **atribuir** botão **Adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste do logon único

O objetivo desta seção é testar sua configuração de logon único do Azure AD usando o Painel de Acesso.  
Ao clicar no bloco do AppBlade no Painel de Acesso, você deverá ser conectado automaticamente ao seu aplicativo do AppBlade. 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure](tutorial-list.md)
* [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/appblade-tutorial/tutorial_general_01.png
[2]: ./media/appblade-tutorial/tutorial_general_02.png
[3]: ./media/appblade-tutorial/tutorial_general_03.png
[4]: ./media/appblade-tutorial/tutorial_general_04.png

[100]: ./media/appblade-tutorial/tutorial_general_100.png

[200]: ./media/appblade-tutorial/tutorial_general_200.png
[201]: ./media/appblade-tutorial/tutorial_general_201.png
[202]: ./media/appblade-tutorial/tutorial_general_202.png
[203]: ./media/appblade-tutorial/tutorial_general_203.png

