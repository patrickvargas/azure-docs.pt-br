---
title: Tipos de aplicativos que podem ser usados no Azure Active Directory B2C| Microsoft Docs
description: Saiba mais sobre os tipos de aplicativos que podem ser usados no Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/01/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 7671a0a99e12463fcce5ff33fbcba7e8677dde05
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51006187"
---
# <a name="applications-types-that-can-be-used-in-active-directory-b2c"></a>Tipos de aplicativos que podem ser usados no Active Directory B2C

O Azure AD (Azure Active Directory) B2C dá suporte à autenticação para uma variedade de arquiteturas de aplicativos modernos. Todas elas se baseiam nos protocolos padrão da indústria, [OAuth 2.0](active-directory-b2c-reference-protocols.md) ou [OpenID Connect](active-directory-b2c-reference-protocols.md). Este documento descreve os tipos de aplicativos que você pode compilar, independentemente da linguagem ou plataforma preferida. Além disso, ajuda a reconhecer cenários de alto nível antes de começar a compilar aplicativos.

Cada aplicativo que usa o Azure AD B2C deve estar registrado no [locatário do Azure AD B2C](active-directory-b2c-get-started.md), usando o [portal do Azure](https://portal.azure.com/). O processo de registro do aplicativo coleta e atribui valores, como:

* Uma **ID do aplicativo** que identifica exclusivamente o aplicativo.
* Um **URL de resposta** que pode ser usado para direcionar as respostas de volta ao seu aplicativo.

Cada solicitação enviada para o AD B2C do Azure especifica uma **política**. Uma política controla o comportamento do AD do Azure. Você também pode usar esses pontos de extremidade para criar um conjunto altamente personalizável de experiências do usuário. Algumas políticas comuns incluem inscrição, entrada e edição de perfil. Se você não estiver familiarizado com as políticas, leia sobre a [estrutura de política extensível](active-directory-b2c-reference-policies.md) do Azure AD B2C antes de continuar.

A interação de cada aplicativo segue um padrão semelhante de alto nível:

1. O aplicativo direciona o usuário para o ponto de extremidade v2.0 para executar uma [política](active-directory-b2c-reference-policies.md).
2. O usuário conclui a política de acordo com a definição de política.
3. O aplicativo recebe um token de segurança do ponto de extremidade v2.0.
4. O aplicativo usa o token de segurança para acessar informações protegidas ou um recurso protegido.
5. O servidor de recurso valida o token de segurança para verificar se o acesso pode ser concedido.
6. O aplicativo atualiza periodicamente o token de segurança.

Essas etapas podem variar um pouco com base no tipo de aplicativo que você está compilando.

## <a name="web-applications"></a>Aplicativos Web

Para os aplicativos Web (incluindo .NET, PHP, Java, Ruby, Python e Node.js) hospedados em um servidor e acessados por meio de um navegador, o Azure AD B2C dá suporte ao [OpenID Connect](active-directory-b2c-reference-protocols.md) para todas as experiências de usuário. Isso inclui gerenciamento de entrada, de inscrição e de perfil. Na implementação do OpenID Connect do Azure AD B2C, o aplicativo Web inicia essas experiências de usuário emitindo solicitações de autenticação para o Azure AD. O resultado da solicitação é um `id_token`. Esse token de segurança representa a identidade do usuário. Ele também fornece informações sobre o usuário na forma de declarações:

```
// Partial raw id_token
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6ImtyaU1QZG1Cd...

// Partial content of a decoded id_token
{
    "name": "John Smith",
    "email": "john.smith@gmail.com",
    "oid": "d9674823-dffc-4e3f-a6eb-62fe4bd48a58"
    ...
}
```

Saiba mais sobre os tipos de tokens e declarações disponíveis para um aplicativo na [referência de token do Azure AD B2C](active-directory-b2c-reference-tokens.md).

Em aplicativos Web, cada execução de uma [política](active-directory-b2c-reference-policies.md) usa estas etapas de alto nível:

1. O usuário navega até o aplicativo web.
2. O aplicativo web redireciona o usuário para o Azure Active Directory B2C que indica a política para executar.
3. O usuário conclui a política.
4. O Azure Active Directory B2C retorna um `id_token` no navegador.
5. O `id_token` é postado para o URI de redirecionamento.
6. O `id_token` é validado e um cookie de sessão é definido.
7. A página de segurança é retornada para o usuário.

Validação do `id_token` usando uma chave de assinatura pública recebida do Azure AD é suficiente para verificar a identidade do usuário. Isso também define um cookie de sessão que pode ser usado para identificar o usuário nas solicitações de página subsequentes.

Para ver esse cenário em ação, experimente um destes exemplos de código de entrada de aplicativo Web em nossa seção [Introdução](active-directory-b2c-overview.md).

Além de facilitar a entrada simples, um aplicativo de servidor Web talvez precise acessar algum serviço Web back-end. Nesse caso, o aplicativo Web pode executar o [fluxo do OpenID Connect](active-directory-b2c-reference-oidc.md) um pouco diferente, e adquirir tokens usando códigos de autorização e tokens de atualização. Este cenário é descrito na [seção de APIs Web](#web-apis)a seguir.

## <a name="web-apis"></a>APIs da Web

Você pode usar o Azure AD B2C para proteger serviços Web, tais como a API Web RESTful do aplicativo. APIs Web podem usar o OAuth 2.0 para proteger seus dados, autenticando solicitações HTTP recebidas usando tokens. O chamador de uma API Web acrescenta um token no cabeçalho de autorização de uma solicitação HTTP:

```
GET /api/items HTTP/1.1
Host: www.mywebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6...
Accept: application/json
...
```

A API Web pode usar o token para verificar a identidade do chamador da API e extrair informações sobre o chamador por meio de declarações codificadas no token. Saiba mais sobre os tipos de tokens e declarações disponíveis para um aplicativo na [referência de token do Azure AD B2C](active-directory-b2c-reference-tokens.md).

Uma API Web pode receber tokens de muitos tipos de clientes, incluindo aplicativos Web, aplicativos móveis e da área de trabalho, aplicativos de página única, daemons do lado do servidor e até outras APIs Web. Veja um exemplo do fluxo completo de um aplicativo Web que chama uma API Web:

1. O aplicativo web executa uma política e o usuário conclui a experiência do usuário.
2. Azure Active Directory B2C retorna um `access_token` e um código de autorização para o navegador.
3. As postagens de navegador a `access_token` e código de autorização ao URI de redirecionamento.
4. O servidor web valida o `access token` e define um cookie de sessão.
5. O `access_token` é fornecido para o Azure AD B2C com o código de autorização, ID de cliente do aplicativo e as credenciais.
6. O `access_token` e `refresh_token` são retornados para o servidor web.
7. A API da web é chamada com o `access_token` em um cabeçalho de autorização.
8. A API da Web valida o token.
9. Os dados de segurança são retornados para o navegador web.

Para saber mais sobre códigos de atualização, tokens de atualização e etapas para a obtenção de tokens, leia sobre o [Protocolo OAuth 2.0](active-directory-b2c-reference-oauth-code.md).

Para saber como proteger uma API Web com o Azure AD B2C, confira os tutoriais da API Web em nossa [seção Introdução](active-directory-b2c-overview.md).

## <a name="mobile-and-native-applications"></a>Aplicativos nativos e móveis

Os aplicativos instalados em dispositivos, como aplicativos móveis e da área de trabalho, geralmente precisam acessar serviços de back-end ou APIs Web em nome de usuários. Você pode adicionar experiências de gerenciamento de identidade personalizadas aos aplicativos nativos e chamar com segurança serviços back-end usando o Azure AD B2C e o [fluxo de código de autorização do OAuth 2.0](active-directory-b2c-reference-oauth-code.md).  

Nesse fluxo, o aplicativo executa [políticas](active-directory-b2c-reference-policies.md) e recebe um `authorization_code` do Azure AD depois que o usuário conclui a política. O `authorization_code` representa a permissão do aplicativo para chamar serviços back-end em nome do usuário conectado no momento. O aplicativo pode trocar o `authorization_code` em segundo plano por um `id_token` e um `refresh_token`.  O aplicativo pode usar o `id_token` para autenticar em uma API Web back-end em solicitações HTTP. Ele também pode usar o `refresh_token` para obter um novo `id_token` quando o antigo expira.

## <a name="current-limitations"></a>Limitações atuais

### <a name="application-not-supported"></a>Aplicativo não tem suportado 

#### <a name="daemonsserver-side-applications"></a>Aplicativos daemons/do lado do servidor

Os aplicativos que contêm processos de longa duração ou que operam sem a presença de um usuário também precisam encontrar uma maneira de acessar os recursos protegidos, tais como APIs Web. Esses aplicativos podem autenticar e obter tokens usando a identidade do aplicativo (em vez da identidade delegada de um usuário) e usando o fluxo de credenciais do cliente OAuth 2.0. O fluxo de credenciais do cliente não é o mesmo que o fluxo em nome e o fluxo em nome não deve ser usado para autenticação de servidor para servidor.

Embora o fluxo de credenciais do cliente atualmente não tenha suporte pelo Azure AD B2C, é possível configurar o fluxo de credencial do cliente usando o Azure AD. Um locatário do Azure AD B2C compartilha algumas funcionalidades com os locatários corporativos do Azure AD.  O fluxo de credencial do cliente tem suporte usando a funcionalidade do Azure AD do locatário do Azure AD B2C. 

Para configurar o fluxo de credencial do cliente, consulte [Azure Active Directory v2.0 e o fluxo de credencial do cliente OAuth 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-protocols-oauth-client-creds). Uma autenticação com êxito resulta no recebimento de um token formatado para poder ser usado pelo Azure AD, conforme descrito na [referência de token do Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims).

#### <a name="web-api-chains-on-behalf-of-flow"></a>Cadeias de API Web (fluxo Em nome de)

Muitas arquiteturas incluem uma API Web que precisa chamar outra API Web downstream, ambas protegidas pelo AD B2C do Azure. Esse cenário é comum em clientes nativos que têm um back-end de API Web. Isso chama um serviço online da Microsoft como a API do Graph do AD do Azure.

Esse cenário de API Web encadeado pode ter suporte usando a concessão credencial de portador JWT do OAuth 2.0, também conhecida como fluxo Em nome de.  No entanto, o fluxo em nome de não está implementado atualmente no Azure AD B2C.

### <a name="reply-url-values"></a>Valores da URL de resposta

No momento, os aplicativos registrados no Azure AD B2C estão restritos a um conjunto limitado de valores para URLs de resposta. O URL de resposta para serviços e aplicativos Web deve começar com o esquema `https`, e todos os valores de URL de resposta devem compartilhar um único domínio DNS. Por exemplo, você não pode registrar um aplicativo Web que tenha uma destas URLs de resposta:

`https://login-east.contoso.com`

`https://login-west.contoso.com`

O sistema de registro compara o nome DNS completo da URL de resposta existente ao nome DNS da URL de resposta que você está adicionando. A solicitação para adicionar o nome DNS falhará se alguma das condições abaixo for verdadeira:

- Se o nome DNS completo da URL de resposta nova não coincidir com o nome DNS da URL de resposta existente.
- Se o nome DNS completo da URL de resposta nova não for um subdomínio da URL de resposta existente.

Por exemplo, se o aplicativo tiver esta URL de resposta:

`https://login.contoso.com`

Você pode adicionar a ele, desta forma:

`https://login.contoso.com/new`

Nesse caso, os nome DNS corresponde exatamente. Ou você pode fazer isto:

`https://new.login.contoso.com`

Nesse caso, você está se referindo a um subdomínio DNS logon.contoso.com. Se você quiser ter um aplicativo com login-east.contoso.com e login-west.contoso.com como URLs de resposta, deverá adicionar as seguintes URLs de resposta nesta ordem:

`https://contoso.com`

`https://login-east.contoso.com`

`https://login-west.contoso.com`

Os dois últimos podem ser adicionados porque eles são subdomínios da primeira URL de resposta, contoso.com. 

Quando você cria aplicativos móveis/nativo, você define uma **URI de redirecionamento** em vez de uma **URL de reprodução**. Há duas considerações importantes ao escolher um URI de redirecionamento:

- **Exclusivo**: o esquema do URI de redirecionamento deve ser exclusivo para cada aplicativo. No exemplo `com.onmicrosoft.contoso.appname://redirect/path`, `com.onmicrosoft.contoso.appname` é o esquema. Esse padrão deve ser seguido. Se dois aplicativos compartilharem o mesmo esquema, o usuário verá a caixa de diálogo **escolher aplicativo**. Se o usuário fizer uma escolha incorreta, o logon falhará.
- **Completo**: o URI de redirecionamento deve ter um esquema e um caminho. O caminho deve conter pelo menos uma barra após o domínio. Por exemplo, `//contoso/` funciona e `//contoso` falha. Certifique-se de que não haja caracteres especiais, como sublinhados, no URI de redirecionamento.

### <a name="faulted-apps"></a>Aplicativos com falha

Aplicativos do Azure AD B2C NÃO devem ser editados:

- Em outros portais de gerenciamento de aplicativos, como o  [Portal de Registro de Aplicativos](https://apps.dev.microsoft.com/).
- Usando a API do Graph ou o PowerShell.

Se você editar o aplicativo do Azure AD B2C fora do portal do Azure, ele se tornará um aplicativo com falha e não poderá mais ser usado com o Azure AD B2C. Você precisa excluir o aplicativo e criá-lo novamente.

Para excluir o aplicativo, acesse o [Portal de Registro de Aplicativos](https://apps.dev.microsoft.com/) e exclua o aplicativo lá. Para que o aplicativo fique visível, você precisa ser o proprietário do aplicativo (e não apenas um administrador do locatário).

