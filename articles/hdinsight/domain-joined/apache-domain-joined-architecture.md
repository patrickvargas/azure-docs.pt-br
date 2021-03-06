---
title: Arquitetura do Azure HDInsight com o pacote de segurança da empresa
description: Saiba como planejar a segurança do HDInsight com o pacote de segurança Enterprise.
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: omidm
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 35768aed3509d6ec5b73c64c7a8a6b132957ace1
ms.sourcegitcommit: 0b7fc82f23f0aa105afb1c5fadb74aecf9a7015b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/14/2018
ms.locfileid: "51632723"
---
# <a name="use-enterprise-security-package-in-hdinsight"></a>Usar o pacote de segurança Enterprise no HDInsight

O cluster HDInsight do Azure padrão é um cluster de usuário único. É adequado para a maioria das empresas que têm equipes de aplicativos menores compilando grandes cargas de trabalho de dados. Cada usuário pode criar um cluster dedicado sob demanda e destruí-lo quando não for mais necessário. 

Muitas empresas adotaram um modelo no qual os clusters são gerenciados por equipes de TI e várias equipes de aplicativos compartilham clusters. Essas empresas maiores precisam de acesso multiusuário para cada cluster no Azure HDInsight.

O HDInsight conta com um provedor de identidade popular -- o Active Directory -- de maneira gerenciada. Ao integrar o HDInsight com [Azure AD DS (Azure Active Directory Domain Services)](../../active-directory-domain-services/active-directory-ds-overview.md), será possível acessar os clusters usando as credenciais de domínio. 

As VMs (máquinas virtuais) no HDInsight são ingressadas no domínio para o domínio fornecido. Portanto, todos os serviços em execução no HDInsight (Apache Ambari, servidor Apache Hive, Apache Ranger, servidor thrift do Apache Spark e outros) funcionam perfeitamente para o usuário autenticado. Os administradores podem, então, criar políticas de autorização fortes usando o Apache Ranger para fornecer controle de acesso baseado em função para recursos no cluster.

## <a name="integrate-hdinsight-with-active-directory"></a>Integrar o HDInsight ao Active Directory

O código-fonte aberto do Apache Hadoop depende do Kerberos para autenticação e segurança. Portanto, os nós de cluster do HDInsight com o Enterprise Security Package (ESP) são unidos a um domínio gerenciado pelo Azure AD DS. A segurança do Kerberos é configurada para os componentes do Hadoop no cluster. 

As seguintes coisas são criadas automaticamente:
- uma entidade de serviço para cada componente do Hadoop 
- uma entidade de segurança do computador para cada computador que tenha ingressado no domínio
- uma UO (unidade organizacional) para cada cluster para armazenar essas entidades de serviço e de máquina 

Para resumir, você precisa configurar um ambiente com:

- Um domínio do Active Directory (gerenciado pelo Azure AD DS).
- LDAP Seguro (LDAPS) habilitado no Azure AD DS.
- Conectividade de rede adequada da rede virtual do HDInsight para a rede virtual do Azure AD DS, se você escolher redes virtuais separadas para elas. Uma VM dentro da rede virtual do HDInsight deve ter uma linha de visão para o Azure AD DS por meio de emparelhamento de rede virtual. Se o HDInsight e o Azure AD DS forem implantados na mesma rede virtual, a conectividade será fornecida automaticamente e nenhuma outra ação será necessária.

## <a name="set-up-different-domain-controllers"></a>Configurar controladores de domínio diferentes
O HDInsight atualmente dá suporte somente ao Azure AD DS como o controlador de domínio principal que o cluster usa para comunicação de Kerberos. Mas outras configurações complexas do Active Directory são possíveis, desde que essa configuração leve à habilitação do Azure AD DS para acesso ao HDInsight.

### <a name="azure-active-directory-domain-services"></a>Azure Active Directory Domain Services
O [Azure AD DS](../../active-directory-domain-services/active-directory-ds-overview.md) fornece um domínio gerenciado totalmente compatível com o Windows Server Active Directory. A Microsoft cuida do gerenciamento, aplicação de patches e monitoramento do domínio em uma configuração HA (altamente disponível). Você pode implantar o cluster sem se preocupar em manter os controladores de domínio. 

Usuários, grupos e senhas são sincronizados do Azure AD (Azure Active Directory). A sincronização unidirecional da instância do Azure AD para Azure AD DS permite que os usuários entrem no cluster usando as mesmas credenciais corporativas. 

Para obter mais informações, consulte [Configurar os clusters do HDInsight com o ESP usando o Azure AD DS](./apache-domain-joined-configure-using-azure-adds.md).

### <a name="on-premises-active-directory-or-active-directory-on-iaas-vms"></a>Active Directory ou Active Directory local em VMs de IaaS

Se você tiver uma instância do Active Directory local ou configurações mais complexas do Active Directory para o domínio, poderá sincronizá-las com o Azure AD usando o Azure AD Connect. É possível habilitar o Azure AD DS nesse locatário do Active Directory. 

Como o Kerberos depende de hashes de senha, será necessário [habilitar a sincronização de hash de senha no Azure AD DS](../../active-directory-domain-services/active-directory-ds-getting-started-password-sync.md). Se estiver usando federação com AD FS (Serviços de Federação do Active Directory), será possível configurar opcionalmente a sincronização de hash de senha como um backup, caso a infraestrutura do AD FS falhe. Para obter mais informações, consulte [Habilitar sincronização de hash de senha com a sincronização do Azure AD Connect](../../active-directory/hybrid/how-to-connect-password-hash-synchronization.md). 

O uso do Active Directory local ou do Active Directory somente em VMs IaaS, sem o Azure AD e o Azure AD DS, não é uma configuração com suporte para clusters HDInsight com ESP.

## <a name="next-steps"></a>Próximas etapas

* [Configurar clusters do HDInsight com ESP](apache-domain-joined-configure-using-azure-adds.md)
* [Configurar políticas do Hive para clusters do HDInsight com ESP](apache-domain-joined-run-hive.md)
* [Gerenciar clusters HDInsight com ESP](apache-domain-joined-manage.md) 
