---
title: Configurar a inscrição e entrada com a conta da Amazon usando o Azure Active Directory B2C | Microsoft Docs
description: Forneça a inscrição e entrada aos consumidores com contas da Amazon em seus aplicativos usando o Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/21/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 5bc1e0faf01125f498eb339bba841b96f6de9d82
ms.sourcegitcommit: 5b8d9dc7c50a26d8f085a10c7281683ea2da9c10
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/26/2018
ms.locfileid: "47181797"
---
# <a name="set-up-sign-up-and-sign-in-with-an-amazon-account-using-azure-active-directory-b2c"></a>Configurar a inscrição e a entrada com a conta da Amazon usando o Azure Active Directory B2C

## <a name="create-an-amazon-application"></a>Criar um aplicativo da Amazon

Para usar uma conta da Amazon como um provedor de identidade no Azure AD (Azure Active Directory) B2C, você precisará criar um aplicativo no locatário que o representa. Se ainda não tiver uma conta da Amazon, consiga uma conta em [http://www.amazon.com/](http://www.amazon.com/).

1. Entre no [Centro de Desenvolvedores da Amazon](https://login.amazon.com/) com suas credenciais de conta da Amazon.
2. Se você ainda não tiver feito isso, clique em **Inscrever-se**, siga as etapas de registro do desenvolvedor e aceite a política.
3. Selecione **Registrar novo aplicativo**.
4. Insira o **Nome**, a **Descrição** e a **URL do Aviso de Privacidade** e clique em **Salvar**. O aviso de privacidade é uma página que você gerencia e que fornece informações de privacidade aos usuários.
5. Na seção **Configurações da Web**, copie os valores de **ID do cliente**. Selecione **Mostrar segredo** para obter o segredo do cliente e copie-o. Você precisará de ambos para configurar uma conta da Amazon como um provedor de identidade em seu locatário. **Segredo do Cliente** é uma credencial de segurança importante.
6. Na seção **Configurações da Web**, selecione **Editar** e insira `https://your-tenant-name.b2clogin.com` em **JavaScript Origins permitido** e `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` em **URLs de retorno permitidas**. Substitua `your-tenant-name` pelo nome do seu locatário. Você precisará usar todas as letras minúsculas, ao inserir o nome do locatário, mesmo se o locatário estiver definido com letras maiúsculas no Azure AD B2C.
7. Clique em **Salvar**.

## <a name="configure-an-amazon-account-as-an-identity-provider"></a>Configurar uma conta da Amazon como um provedor de identidade

1. Entre no [portal do Azure](https://portal.azure.com/) como administrador global do locatário Azure AD B2C.
2. Verifique se você está usando o diretório que contém o locatário do Azure AD B2C clicando no **filtro Diretório e assinatura** no menu superior e escolhendo o diretório que contém seu locatário.
3. Escolha **Todos os serviços** no canto superior esquerdo do portal do Azure, procure e selecione **Azure AD B2C**.
4. Escolha **Provedores de identidade** e escolha **Adicionar**.
5. Insira um **Nome**. Por exemplo, insira *Amazon*.
6. Selecione **Tipo de provedor de identidade**, selecione **Amazon** e clique em **OK**.
7. Selecione **Configurar este provedor de identidade**. Insira a ID do cliente que você anotou anteriormente como a **ID do cliente** e insira o segredo do cliente que você registrou como o **Segredo do cliente** do aplicativo da Amazon que criou anteriormente.
8. Clique em **OK** e em **Criar** para salvar sua configuração da Amazon.

