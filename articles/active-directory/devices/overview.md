---
title: O que é gerenciamento de dispositivos no Azure Active Directory? | Microsoft Docs
description: Saiba como o gerenciamento de dispositivos pode ajudar você a controlar os dispositivos que estão acessando os recursos do seu ambiente.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.component: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 08/25/2018
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: f239b3ef6881f9ea1be043b7d27f061e015ae3be
ms.sourcegitcommit: f0c2758fb8ccfaba76ce0b17833ca019a8a09d46
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/06/2018
ms.locfileid: "51037354"
---
# <a name="what-is-device-management-in-azure-active-directory"></a>O que é gerenciamento de dispositivos no Azure Active Directory?

Em um mundo que prioriza os dispositivos móveis e a nuvem, o Azure AD (Azure Active Directory) permite o logon único em dispositivos, aplicativos e serviços de qualquer lugar. Com a proliferação de dispositivos, incluindo a abordagem BYOD (traga seu próprio dispositivo), os profissionais de TI se deparam com duas metas conflitantes:

- Capacitar os usuários finais para serem produtivos sempre e em qualquer lugar
- Proteger os ativos corporativos a qualquer momento

Por meio de dispositivos, os usuários têm acesso aos ativos corporativos. Para proteger os ativos corporativos, como um administrador de TI, você quer ter controle sobre esses dispositivos. Desse modo, você tem a certeza de que os usuários estão acessando os recursos de dispositivos que atendem aos padrões de segurança e conformidade. 

O gerenciamento de dispositivo também é a base para [acesso condicional com base no dispositivo](../conditional-access/require-managed-devices.md). Com o acesso condicional com base no dispositivo, você pode garantir que o acesso a recursos em seu ambiente somente seja possível com dispositivos gerenciados.   

Este artigo explica como funciona o gerenciamento de dispositivo no Azure Active Directory.

## <a name="getting-devices-under-the-control-of-azure-ad"></a>Colocando dispositivos sob controle do Azure AD

Para colocar um dispositivo sob controle do Azure AD, você tem duas opções:

- Registro 
- Adição

O **registro** de um dispositivo no Azure AD permite que você gerencie a identidade do dispositivo. Quando um dispositivo é registrado, o registro de dispositivo do Azure AD fornece ao dispositivo uma identidade, que é usada para autenticar o dispositivo quando o usuário entra no Azure AD. Você pode usar a identidade para habilitar ou desabilitar um dispositivo.

Quando combinada com uma solução MDM (gerenciamento de dispositivo móvel), como o Microsoft Intune, os atributos do dispositivo no Azure AD são atualizados com informações adicionais sobre o dispositivo. Isso permite que você crie regras de acesso condicional que imponham que o acesso dos dispositivos atendam aos padrões de segurança e conformidade. Para saber mais sobre o registro de dispositivos no Microsoft Intune, confira [Registrar dispositivos para gerenciamento no Intune](https://docs.microsoft.com/intune/device-enrollment#supported-device-platforms).

A **adição** de um dispositivo é uma extensão do registro de um dispositivo. Isto é, além de todos os benefícios recebidos ao registrar um dispositivo, o estado local de um dispositivo é alterado. A alteração do estado local permite que os usuários entrem em um dispositivo usando uma conta organizacional corporativa ou de estudante em vez de uma conta pessoal.

## <a name="azure-ad-registered-devices"></a>Dispositivos registrados no Azure AD   

A meta dos dispositivos registrados no Azure AD é fornecer suporte ao cenário **BYOD (traga seu próprio dispositivo)**. Nesse cenário, um usuário pode acessar os recursos controlados pelo Azure Active Directory da sua organização usando um dispositivo pessoal.  

![Dispositivos registrados no Azure AD](./media/overview/03.png)

O acesso se baseia em uma conta corporativa ou de estudante que foi digitada no dispositivo.  
Por exemplo, o Windows 10 permite que os usuários adicionem uma conta corporativa ou de estudante a um computador, tablet ou telefone pessoal.  
Quando um usuário tiver adicionado uma conta corporativa ou de estudante, o dispositivo será registrado no Azure AD e, opcionalmente, inscrito no sistema MDM (gerenciamento de dispositivo móvel) configurado pela organização. Os usuários da organização podem adicionar uma conta corporativa ou de estudante a um dispositivo pessoal de forma conveniente:

- Ao acessar um aplicativo de trabalho pela primeira vez
- Manualmente por meio do menu **Configurações** no caso do Windows 10 

Você pode configurar dispositivos registrados no Azure AD para Windows 10, iOS, Android e macOS.

## <a name="azure-ad-joined-devices"></a>Dispositivos adicionados ao Azure AD

A meta dos dispositivos adicionados ao Azure AD é simplificar:

- As implantações de dispositivos Windows que pertencem à organização 
- O acesso a aplicativos e recursos organizacionais de qualquer dispositivo Windows
- Gerenciamento baseado em nuvem de dispositivos empresariais

![Dispositivos registrados no Azure AD](./media/overview/02.png)

O Ingresso no Azure AD pode ser implantado usando um destes métodos: 
 - [Windows Autopilot](https://docs.microsoft.com/windows/deployment/windows-autopilot/windows-10-autopilot)
 - [Implantação em massa](https://docs.microsoft.com/intune/windows-bulk-enroll)
 - [Experiência de autoatendimento](azuread-joined-devices-frx.md) 

O **Ingresso no Azure AD** foi desenvolvido para organizações que priorizam a nuvem (ou seja, que usam principalmente serviços de nuvem, com a meta de reduzir o uso de uma infraestrutura local) ou somente em nuvem (sem infraestrutura local). Não há restrições de tamanho ou tipo de organizações que podem implantar o Ingresso no Azure AD. O Ingresso no Azure AD funciona bem até mesmo em um ambiente híbrido, permitindo acesso aos recursos e aplicativos locais e de nuvem.

A implementação de dispositivos adicionados ao Azure AD proporciona os seguintes benefícios:

- **SSO (Logon Único)** para serviços e aplicativos SaaS gerenciados pelo Azure. Os usuários não visualizam solicitações adicionais de autenticação ao acessar recursos de trabalho. A funcionalidade de SSO é consistente mesmo quando eles não estão conectados à rede de domínio disponível.

- **Roaming em conformidade corporativa** das configurações de usuário entre dispositivos adicionados. Os usuários não precisam se conectar a uma conta da Microsoft (por exemplo, Hotmail) para ver as configurações entre dispositivos.

- **Acesso à Windows Store para Empresas** usando uma conta do Azure AD. Os usuários podem escolher dentre um inventário de aplicativos pré-selecionados pela organização.

- Suporte ao **Windows Hello** para acesso conveniente e seguro aos recursos de trabalho.

- **Restrição de acesso** a aplicativos somente de dispositivos que atendem à política de conformidade.

- **Acesso direto aos recursos locais** quando o dispositivo tem a linha de visão para o controlador de domínio local. 


Embora a adição ao Azure AD se destine basicamente às organizações que não têm uma infraestrutura do Active Directory local para Windows Server, você certamente pode usá-la em cenários em que:

- Você deseja fazer a transição para infraestrutura baseada em nuvem usando o Azure AD e um MDM como o Intune.

- Não é possível usar um ingresso no domínio local, por exemplo, caso seja necessário colocar dispositivos móveis, como tablets e telefones, sob controle.

- Os usuários precisam basicamente acessar o Office 365 ou outros aplicativos SaaS integrados ao Azure AD.

- Você quer gerenciar um grupo de usuários no Azure AD, e não no Active Directory. Isso pode se aplicar, por exemplo, a funcionários temporários, prestadores de serviços ou alunos.

- Você deseja fornecer recursos de ingresso para trabalhadores em escritórios remotos com infraestrutura local limitada.

Você pode configurar dispositivos adicionados ao Azure AD para dispositivos com Windows 10.


## <a name="hybrid-azure-ad-joined-devices"></a>Dispositivos adicionados ao Azure AD híbrido

Por mais de uma década, muitas organizações têm usado o ingresso no domínio para o respectivo Active Directory local a fim de permitir que:

- Os departamentos de TI gerenciem dispositivos pertencentes à empresa de um local central.

- Os usuários entrem em seus dispositivos com as respectivas contas corporativas ou de estudante do Active Directory. 

Normalmente, as organizações com um espaço local dependem de métodos de geração de imagens para provisionar dispositivos e, muitas vezes, usam o **SCCM (System Center Configuration Manager)** ou a **GP (Política de Grupo)** para gerenciá-los.

Se seu ambiente tiver um espaço local do AD e você também quiser se beneficiar dos recursos fornecidos pelo Azure Active Directory, será possível implementar dispositivos adicionados ao Azure AD híbrido. Esses dispositivos são ingressados no Active Directory local e registrados no Azure Active Directory.

![Dispositivos registrados no Azure AD](./media/overview/01.png)


Você deverá usar dispositivos adicionados ao Azure AD híbrido se:

- Você tem aplicativos Win32 implantados nesses dispositivos que utilizam a autenticação de computador do Active Directory.

- Você precisa do GP para gerenciar dispositivos.

- Você deseja continuar a usar soluções de geração de imagens para configurar dispositivos de seus funcionários.

É possível configurar dispositivos adicionados ao Azure AD para dispositivos com Windows 10 e de nível inferior, como Windows 8 e Windows 7.

## <a name="summary"></a>Resumo

Com o gerenciamento de dispositivo no Azure AD, é possível: 

- Simplificar o processo de colocar dispositivos sob controle do Azure AD

- Fornecer aos usuários um acesso fácil de usar a recursos baseados em nuvem da sua organização

Como uma regra prática, você deve usar:

- Dispositivos registrados no Azure Active Directory:

    - Para dispositivos pessoais 

    - Para registrar manualmente os dispositivos com o Azure Active Directory

- Dispositivos vinculados ao Azure Active Directory: 

    - Para os dispositivos que pertencerem à sua organização

    - Para os dispositivos que **não** tiverem sido vinculados a um AD local

    - Para registrar manualmente os dispositivos com o Azure Active Directory

    - Para alterar o estado de local de um dispositivo

- Dispositivos adicionados ao Azure AD híbrido para dispositivos que foram adicionados a um AD local     

    - Para os dispositivos que pertencerem à sua organização

    - Para os dispositivos que tiverem sido vinculados a um AD local

    - Para registrar altomaticamente os dispositivos com o Azure Active Directory

    - Para alterar o estado de local de um dispositivo



## <a name="next-steps"></a>Próximas etapas

- Para obter uma visão geral de como gerenciar dispositivos no portal do Azure, consulte [Gerenciar dispositivos usando o portal do Azure](device-management-azure-portal.md)

- Para saber mais sobre o acesso condicional baseado em dispositivo, confira [Configurar as políticas de acesso condicional com base em dispositivo do Azure Active Directory](../conditional-access/require-managed-devices.md).

- Para configurar:
    - Dispositivos Windows 10 registrados Azure Active Directory, consulte [Como configurar dispositivos Windows 10 registrados no Azure Active Directory](../user-help/device-management-azuread-registered-devices-windows10-setup.md)
    - Dispositivos adicionados ao Azure Active Directory, consulte [Como configurar dispositivos adicionados ao Azure Active Directory](../user-help/device-management-azuread-joined-devices-setup.md)
    - Para dispositivos associados híbridos do Azure AD, consulte [Como planejar sua implementação de junção híbrida do Active Directory do Azure](hybrid-azuread-join-plan.md).


