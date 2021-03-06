---
title: Comparação da colaboração B2B e B2C no Azure Active Directory | Microsoft Docs
description: Qual é a diferença entre colaboração B2B e a B2C do Azure Active Directory?
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: overview
ms.date: 03/15/2017
ms.author: mimart
author: msmimart
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 42fbb8b08a2dc24ced436c4a6104f03ae3bca1e9
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/18/2018
ms.locfileid: "45982803"
---
# <a name="compare-b2b-collaboration-and-b2c-in-azure-active-directory"></a>Comparação da colaboração B2B e B2C no Azure Active Directory

As duas colaborações B2B e B2C do Azure Active Directory (AD do Azure) permitem que você trabalhe com usuários externos no AD do Azure. Mas como eles se comparam?

O **Azure AD B2B** é para empresas que querem compartilhar com segurança arquivos e recursos com usuários externos para que eles possam colaborar. Um administrador do Azure configura o B2B no portal do Azure e o Azure AD cuida da federação entre o seu negócio e seus parceiros externos. Os usuários entram nos recursos compartilhados usando um processo simples de convite e resgate com a conta corporativa ou de estudante ou qualquer conta de email.
 
O **Azure AD B2C** é usado principalmente para as empresas e os desenvolvedores que criam aplicativos voltados para o cliente. Com o Azure AD B2C, os desenvolvedores podem usar o Azure AD como o sistema de identidade completo para seu aplicativo, enquanto permite que os clientes entrem com uma identidade já estabelecida (como Facebook ou Gmail).

A tabela a seguir fornece uma comparação detalhada.


Recursos de colaboração B2B |     Oferta autônoma do B2C do AD do Azure
-------- | --------
Destinado a: organizações que desejam poder autenticar usuários de uma organização de parceiros, independentemente do provedor de identidade. | Destinado a: convidar clientes dos aplicativos Web e móveis, sejam indivíduos, clientes institucionais ou organizacionais, para o Azure AD.
Identidades suportadas: funcionários com contas corporativas ou escolares, parceiros com contas corporativas ou escolares ou qualquer email. Em breve com suporte à federação direta.  | Identidades suportadas: os usuários do consumidor com contas de aplicativo local (qualquer nome de usuário ou email) ou identidades sociais suportadas com federação direta.
Qual diretório são os usuários do parceiro encontram-se: usuários de parceiros da organização externa são gerenciados no mesmo diretório que os funcionários, mas anotados especialmente. Eles podem ser gerenciados da mesma forma que os funcionários, podem ser adicionados aos mesmos grupos e assim por diante  | Qual diretório as entidades de usuário do cliente estão: no diretório do aplicativo. Gerenciado separadamente do diretório de parceiros e funcionários da organização (se houver).
Há suporte para SSO (logon único) em todos os aplicativos conectados ao Azure AD. Por exemplo, é possível fornecer acesso ao Office 365 ou a aplicativos locais, além de outros aplicativos SaaS, como o Salesforce ou Workday.  |  Há suporte para SSO para aplicativos de clientes dentro dos locatários do B2C do AD do Azure. Não há suporte para SSO para Office 365 nem para outros aplicativos SaaS da Microsoft SaaS que não sejam da Microsoft.
Ciclo de vida do parceiro: gerenciado pela organização convidada/host.  | Ciclo de vida do cliente: autoatendido ou gerenciado pelo aplicativo.
Política de segurança e conformidade: Gerenciado pela organização host / convidativa (por exemplo, com [políticas de acesso condicional](https://docs.microsoft.com/azure/active-directory/b2b/conditional-access)).  | Política de segurança e conformidade: gerenciada pelo aplicativo.
Identidade visual: uso da marca da organização convidada/host.  |    Identidade visual: gerenciada pelo aplicativo. Geralmente, tende a ser da marca do produto com o esmaecimento da organização na tela de fundo.
Mais informações em: [Postagem de Blog](https://blogs.technet.microsoft.com/enterprisemobility/2017/02/01/azure-ad-b2b-new-updates-make-cross-business-collab-easy/), [Documentação](what-is-b2b.md)  | Mais informações em: [Página](https://azure.microsoft.com/services/active-directory-b2c/), [Documentação](https://docs.microsoft.com/azure/active-directory-b2c/)


### <a name="next-steps"></a>Próximas etapas

- [O que é a colaboração B2B do AD do Azure?](what-is-b2b.md)
- [Propriedades de usuário de colaboração B2B](user-properties.md)

