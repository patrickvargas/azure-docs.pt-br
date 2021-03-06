---
author: PatAltimore
ms.service: active-directory-b2c
ms.topic: include
ms.date: 11/03/2016
ms.author: patricka
ms.openlocfilehash: 19e7c919345c0f56b274737840f8150f7d710501
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50133324"
---
Se quiser habilitar somente a entrada no aplicativo, use uma política de **entrada**. Essa política descreve as experiências pelas quais os clientes passarão durante a entrada e o conteúdo dos tokens que o aplicativo receberá em entradas com êxito.

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](active-directory-b2c-portal-navigate-b2c-service.md)]
Clique em **Políticas de entrada**.

Clique em **+Adicionar** , na parte superior da folha.

O **Nome** determina o nome da política de entrada usado pelo seu aplicativo. Por exemplo, insira **SiIn**.

Clique em **Provedores de identidade** e selecione **Entrada na Conta Local**. Opcionalmente, você também pode selecionar provedores de identidade social, se já configurado. Clique em **OK**.

Clique em **Declarações do aplicativo**. Escolha aqui as declarações que você deseja que sejam retornadas dos tokens enviados ao aplicativo após uma experiência de entrada bem-sucedida. Por exemplo, selecione **Nome de Exibição**, **Provedor de Identidade**, **CEP** e **ID de Objeto do Usuário**. Clique em **OK**.

Clique em **Criar**. Observe que a política recém-criada aparece como **B2C_1_SiIn** (o fragmento **B2C\_1\_** torna-se automaticamente pré-pendente) na folha **Políticas de entrada**.

Abra a política clicando em **B2C_1_SiIn**.

Selecione **Aplicativo Contoso B2C** na lista suspensa **Aplicativos** e `https://localhost:44321/` na lista suspensa **URL de Resposta/URI de Redirecionamento**.

Clique em **Executar agora**. Uma nova guia do navegador se abre e você poderá percorrer a experiência do consumidor de entrada no aplicativo.

> [!NOTE]
> Leva até um minuto para a criação de políticas e as atualizações entrem em vigor.
>