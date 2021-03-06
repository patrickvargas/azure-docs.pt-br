---
title: Backup e restauração de VMs criptografadas usando o Backup do Azure
description: Este artigo aborda a experiência de backup e restauração para VMs criptografadas usando o Azure Disk Encryption.
services: backup
author: sogup
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 7/10/2018
ms.author: sogup
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b269b8db59c4aeecf182b6ea11b92a3980a2cd6d
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/12/2018
ms.locfileid: "51567410"
---
# <a name="back-up-and-restore-encrypted-virtual-machines-with-azure-backup"></a>Backup e restauração de máquinas virtuais criptografadas usando o Backup do Azure
Este artigo descreve as etapas para fazer backup e restaurar VMs (máquinas virtuais) usando o Backup do Azure. Ele também oferece detalhes sobre os cenários com suporte, os pré-requisitos e as etapas de solução de problemas para casos de erro.

## <a name="supported-scenarios"></a>Cenários com suporte

 O backup e a restauração de VMs criptografadas têm suporte somente para VMs que usam o modelo de implantação do Azure Resource Manager. Não há suporte para VMs que usam o modelo de implantação clássico. O backup e a restauração de VMs criptografadas são compatíveis com VMs do Windows e do Linux que usam o Azure Disk Encryption. O Disk Encryption usa o BitLocker, um recurso do Windows padrão da indústria e o recurso dm-crypt do Linux para fornecer a criptografia de discos. A tabela a seguir mostra o tipo de criptografia e o suporte para VMs.

   |  | VMs BEK + KEK | VMs somente com BEK |
   | --- | --- | --- |
   | **VMs não gerenciadas**  | SIM | SIM  |
   | **VMs gerenciadas**  | SIM | SIM  |

## <a name="prerequisites"></a>Pré-requisitos
* A VM foi criptografada usando o [Azure Disk Encryption](../security/azure-security-disk-encryption.md).

* O cofre dos Serviços de Recuperação foi criado e a replicação de armazenamento foi definida seguindo as etapas em [Preparar seu ambiente para backup](backup-azure-arm-vms-prepare.md).

* O backup recebeu [permissões para acessar o cofre de chaves](#provide-permissions-to-backup) que contém chaves e segredos para VMs criptografadas.

## <a name="backup-encrypted-vm"></a>Backup de VM criptografada
Use as etapas a seguir para definir a meta de backup, definir uma política, configurar itens e disparar um backup.

### <a name="configure-backup"></a>Configurar o backup
1. Se já tiver um cofre dos Serviços de Recuperação aberto, vá para a próxima etapa. Se você não tiver um cofre de Serviços de Recuperação aberto, mas estiver no Portal do Azure, selecione **Todos os serviços**.

    a. Na lista de recursos, digite **Serviços de Recuperação**.

   b. Quando você começa a digitar, a lista é filtrada com base em sua entrada. Quando você vir a opção **cofres de Serviços de Recuperação**, selecione-a.

      ![Cofre dos Serviços de Recuperação](./media/backup-azure-vms-encryption/browse-to-rs-vaults.png) <br/>

    c. A lista de cofres de Serviços de Recuperação aparecerá. Selecione um cofre na lista.

     O painel de cofres selecionados será aberto.
1. Na lista de itens que aparece no cofre, selecione **Backup** para começar a fazer backup da VM criptografada.

      ![Folha Backup](./media/backup-azure-vms-encryption/select-backup.png)
1. No bloco **Backup**, selecione **Meta de backup**.

      ![Folha Cenário](./media/backup-azure-vms-encryption/select-backup-goal-one.png)
1. Em **Onde sua carga de trabalho é executada?**, selecione **Azure**. Em **Do que você deseja fazer backup?**, selecione **Máquina virtual**. Depois, selecione **OK**.

   ![Abrir a folha Cenário](./media/backup-azure-vms-encryption/select-backup-goal-two.png)
1. Em **Escolher política de backup**, selecione a política de backup que você deseja aplicar ao cofre. Depois, selecione **OK**.

      ![Selecionar a política de backup](./media/backup-azure-vms-encryption/setting-rs-backup-policy-new.png)

    Os detalhes da política padrão estão listados. Se quiser criar uma política, selecione **Criar Nova** na lista suspensa. Após você selecionar **OK**, a política de backup será associada ao cofre.

1. Escolha as VMs criptografadas para associar à política especificada e selecione **OK**.

      ![Selecionar VMs criptografadas](./media/backup-azure-vms-encryption/selected-encrypted-vms.png)
1. Essa página mostra uma mensagem sobre os cofres de chaves associados às VMs criptografadas selecionadas. O backup requer acesso somente leitura às chaves e segredos no cofre de chaves. Ele usa essas permissões para fazer backup das chaves e segredos, junto com as VMs associadas.<br>
Se você for um **Usuário membro**, o processo Habilitar Backup vai adquirir acesso diretamente ao cofre de chaves para fazer backup de VMs criptografadas sem a necessidade de qualquer intervenção do usuário.

   ![Mensagem de VMs criptografada](./media/backup-azure-vms-encryption/member-user-encrypted-vm-warning-message.png)

   Para um **Usuário convidado**, você deve fornecer permissões para que o serviço de backup acesse o cofre de chaves para que os backups funcionem. Você pode fornecer essas permissões seguindo as [etapas mencionadas na seção a seguir](#provide-permissions-to-backup)

   ![Mensagem de VMs criptografada](./media/backup-azure-vms-encryption/guest-user-encrypted-vm-warning-message.png)

    Agora que você definiu todas as configurações para o cofre, selecione **Habilitar Backup** na parte inferior da página. **Habilitar Backup** implanta a política ao cofre e às VMs.

1. A próxima fase de preparação será instalar o Agente de VM ou verificar se ele está instalado. Para fazer o mesmo, siga as etapas em [Preparar o ambiente para backup](backup-azure-arm-vms-prepare.md).

### <a name="trigger-a-backup-job"></a>Disparar um trabalho de backup
Siga as etapas em [Fazer backup das VMs do Azure no cofre dos Serviços de Recuperação](backup-azure-arm-vms.md) para disparar um trabalho de backup.

### <a name="continue-backups-of-already-backed-up-vms-with-encryption-enabled"></a>Continuar os backups de VMs cujo backup já foi realizado com a criptografia habilitada  
Se você tiver VMs cujo backup já estiver sendo feito em um cofre dos Serviços de Recuperação e que foram habilitadas para criptografia posteriormente, você deverá conceder permissões para Backup acessar o cofre de chaves para que os backups continuem. Você pode fornecer essas permissões seguindo as [etapas mencionadas na seção abaixo](#provide-permissions-to-azure-backup). Ou pode seguir as etapas do PowerShell na seção "Habilitar backup" da [Documentação do PowerShell](backup-azure-vms-automation.md).

## <a name="provide-permissions-to-azure-backup"></a>Fornecer permissões para o Backup
Use as etapas a seguir para fornecer as permissões relevantes para o Backup acessar o cofre de chaves e fazer backup de VMs criptografadas.
1. Selecione **Todos os serviços** e pesquise **Cofres de chaves**.

    ![Cofres de chaves](./media/backup-azure-vms-encryption/search-key-vault.png)

1. Na lista de cofres de chaves, selecione o cofre de chaves associado à VM criptografada cujo backup precisa ser feito.

     ![Seleção de cofre de chaves](./media/backup-azure-vms-encryption/select-key-vault.png)

1. Selecione **Políticas de acesso** e selecione **Adicionar nova**.

    ![Adicionar nova](./media/backup-azure-vms-encryption/select-key-vault-access-policy.png)

1. Selecione **Selecionar entidade** e digite **Serviço de Gerenciamento de Backup** na caixa de pesquisa.

    ![Pesquisa de serviço de backup](./media/backup-azure-vms-encryption/search-backup-service.png)

1. Selecione **Serviço de Gerenciamento de Backup** e clique em **Selecionar**.

    ![Seleção de serviço de backup](./media/backup-azure-vms-encryption/select-backup-service.png)

1. Selecione **Backup do Azure** em **Configurar usando o modelo (opcional)**. As permissões necessárias para **Permissões de chave** e **Permissões de segredo** são preenchidas. Se sua VM for criptografada usando **somente BEK**, serão necessárias apenas as permissões para segredos, portanto você deve remover a seleção de **Permissões de chave**.

    ![Seleção de backup do Azure](./media/backup-azure-vms-encryption/select-backup-template.png)

1. Selecione **OK**. Observe que o **Serviço de Gerenciamento de Backup** é adicionado em **Políticas de acesso**.

    ![Políticas de acesso](./media/backup-azure-vms-encryption/backup-service-access-policy.png)

1. Selecione **Salvar** para conceder as permissões necessárias ao Backup.

    ![Política de acesso de backup](./media/backup-azure-vms-encryption/save-access-policy.png)

Após as permissões terem sido concedidas com êxito, você poderá continuar com a habilitação do backup para VMs criptografadas.

## <a name="restore-an-encrypted-vm"></a>Restaurar uma VM criptografada
Para restaurar uma VM criptografada, primeiro restaure os discos seguindo as etapas na seção "Restaurar discos com backup" em [Escolha uma configuração de restauração de VM](backup-azure-arm-restore-vms.md#choose-a-vm-restore-configuration). Depois disso, você pode usar uma das seguintes opções:

* Use as etapas do PowerShell em [Criar uma VM de discos restaurados](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) para criar uma VM completa de discos restaurados.
* Ou, [use modelos para personalizar uma VM restaurada](backup-azure-arm-restore-vms.md#use-templates-to-customize-a-restored-vm) para criar VMs de discos restaurados. Modelos podem ser usados somente para pontos de recuperação criados após 26 de abril de 2017.

## <a name="troubleshooting-errors"></a>Solucionar erros
| Operação | Detalhes do erro | Resolução |
| --- | --- | --- |
|Backup | O backup não tem permissões suficientes para o cofre de chaves para fazer backup de VMs criptografadas. | O backup deve receber essas permissões seguindo [as etapas na seção anterior](#provide-permissions-to-azure-backup). Ou você pode seguir as etapas na seção "Habilitar proteção" do artigo, do PowerShell [usar o PowerShell para fazer backup e restaurar máquinas virtuais](backup-azure-vms-automation.md#enable-protection). |  
| Restaurar |Você não poderá restaurar essa VM criptografada pois o cofre de chaves associado a ela não existe. |Crie o cofre de chaves usando [Introdução ao Azure Key Vault](../key-vault/key-vault-get-started.md). Consulte [Restaurar chave e segredo do cofre de chaves usando o Backup do Azure](backup-azure-restore-key-secret.md) para restaurar a chave e o segredo, se eles não estiverem presentes. |
| Restaurar |Você não poderá restaurar essa VM criptografada pois a chave e o segredo associado a ela não existem. |Consulte [Restaurar chave e segredo do cofre de chaves usando o Backup do Azure](backup-azure-restore-key-secret.md) para restaurar a chave e o segredo, se eles não estiverem presentes. |
| Restaurar |O backup não tem autorização para acessar recursos em sua assinatura. |Conforme mencionado anteriormente, primeiro restaure os discos seguindo as etapas na seção "Restaurar discos com backup" em [Escolha uma configuração de restauração de VM](backup-azure-arm-restore-vms.md#choose-a-vm-restore-configuration). Depois disso, use o PowerShell para [criar uma VM de discos restaurados](backup-azure-vms-automation.md#create-a-vm-from-restored-disks). |
