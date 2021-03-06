---
title: Desabilitar a verificação de email durante a inscrição do consumidor no Azure Active Directory B2C | Microsoft Docs
description: Um tópico que demonstra como desabilitar a verificação de email durante a inscrição do consumidor no Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 2/06/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: e36dd19aa020b8cb2a623cda904cf7fa8a0b26da
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51004577"
---
# <a name="disable-email-verification-during-consumer-sign-up-in-azure-active-directory-b2c"></a>Desabilitar a verificação de e-mail durante a inscrição do consumidor no B2C do Azure Active Directory B2C 
Quando está habilitado, o Azure Active Directory (Azure AD) B2C fornece a um consumidor a capacidade de se inscrever em aplicativos fornecendo um endereço de email e criando uma conta local. O Azure AD B2C verifica os endereços de email válidos exigindo que os consumidores os confirmem durante o processo de inscrição. Ele também impede que um processo automatizado mal-intencionado gere contas falsas para os aplicativos.

Alguns desenvolvedores de aplicativo preferem ignorar a verificação de email durante o processo de inscrição e, em vez disso, pedem para os consumidores verificarem o endereço de email mais tarde. Para dar suporte a isso, o Azure AD B2C pode ser configurado para desabilitar a verificação de email. Isso cria um processo de inscrição mais tranquilo e oferece aos desenvolvedores a flexibilidade para diferenciar os consumidores que confirmaram seu endereço de email daqueles que não o fizeram.

Por padrão, as políticas de inscrição têm a verificação de email ativada. Use as etapas a seguir para desativá-la:

1. Clique em **Políticas de inscrição** ou em **Políticas de inscrição ou entrada** dependendo do que você configurou para inscrição.
2. Clique em sua política (por exemplo, "B2C_1_SiUp") para abri-la. 
3. Clique em **Editar** na parte superior da folha.
4. Clique em **Personalização da interface do usuário da página**.
5. Clique em **Página de inscrição da conta local**.
6. Clique em **Endereço de Email** na coluna **Nome** na seção **Atributos de inscrição**.
7. Alterne a opção **Exigir verificação** para **Não**.
8. Clique em **OK** na parte inferior até chegar à folha **Editar política**.
9. Clique em **Salvar** na parte superior da folha. Pronto!

> [!NOTE]
> Desabilitar a verificação de email no processo de inscrição pode gerar spam. Se você desabilitar o padrão, recomendamos adicionar seu próprio sistema de verificação.
> 
>