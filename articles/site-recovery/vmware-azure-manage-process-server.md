---
title: Gerenciar um servidor de processo para recuperação de desastre de VMs e servidores físicos do VMware no Azure usando o Azure Site Recovery | Microsoft Docs
description: Este artigo descreve como gerenciar um servidor de processo configurado para recuperação de desastre de VMs e servidor físico do VMware no Azure usando o Azure Site Recovery.
author: Rajeswari-Mamilla
ms.service: site-recovery
ms.topic: conceptual
ms.date: 10/29/2018
ms.author: ramamill
ms.openlocfilehash: d99b5d1fdca39466d5e09ca077329b7ffa8622bc
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/12/2018
ms.locfileid: "51568845"
---
# <a name="manage-process-servers"></a>Gerenciar servidores de processo

Por padrão, o servidor em processo usado quando você replica servidores físicos ou VMs do VMware no Azure é instalado no computador do servidor de configuração local. Há duas instâncias nas quais você precisa configurar um servidor em processo separado:

- Para implantações grandes, você pode precisar de servidores em processo locais adicionais para dimensionar a capacidade.
- Para o failback, você precisa de um servidor em processo temporário configurado no Azure. Você pode excluir essa VM após o failback. 

Este artigo resume as tarefas típicas de gerenciamento para esses servidores em processo adicionais.

## <a name="upgrade-a-process-server"></a>Atualizar um servidor em processo

Atualize um servidor em processo executado localmente ou no Azure (para fins de failback), da seguinte maneira:

[!INCLUDE [site-recovery-vmware-upgrade -process-server](../../includes/site-recovery-vmware-upgrade-process-server-internal.md)]

> [!NOTE]
  Normalmente, quando você usa a Imagem da Galeria do Azure para criar um servidor em processo no Azure para fins de failback, ele executa a última versão disponível. As equipes do Site Recovery liberam correções e melhorias regularmente, e recomendamos que você mantenha os servidores em processo atualizados.



## <a name="reregister-a-process-server"></a>Registrar um servidor em processo novamente

Caso precise registrar novamente um servidor em processo executado localmente ou no Azure, com o servidor de configuração, faça o seguinte:

[!INCLUDE [site-recovery-vmware-register-process-server](../../includes/site-recovery-vmware-register-process-server.md)]

Depois de salvar as configurações, faça o seguinte:

1. No servidor em processo, abra um prompt de comando do administrador.
2. Navegue para a pasta **%PROGRAMDATA%\ASR\Agent** e execute o comando:

    ```
    cdpcli.exe --registermt
    net stop obengine
    net start obengine
    ```

## <a name="modify-proxy-settings-for-an-on-premises-process-server"></a>Modificar as configurações de proxy de um servidor em processo local

Se o servidor em processo usa um proxy para se conectar ao Site Recovery no Azure, use este procedimento caso precise modificar as configurações de proxy existentes.

1. Faça logon no computador do servidor em processo. 
2. Abra uma janela de comando do PowerShell de administrador e execute o comando a seguir:
  ```powershell
  $pwd = ConvertTo-SecureString -String MyProxyUserPassword
  Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber –ProxyUserName domain\username -ProxyPassword $pwd
  net stop obengine
  net start obengine
  ```
2. Navegue para a pasta **%PROGRAMDATA%\ASR\Agent** e execute o seguinte comando:
  ```
  cmd
  cdpcli.exe --registermt

  net stop obengine

  net start obengine

  exit
  ```


## <a name="remove-a-process-server"></a>Remover um servidor em processo

[!INCLUDE [site-recovery-vmware-unregister-process-server](../../includes/site-recovery-vmware-unregister-process-server.md)]

## <a name="manage-anti-virus-software-on-process-servers"></a>Gerenciar software antivírus em servidores de processo

Se o software antivírus estiver ativo em um servidor de processos independente ou servidor de destino mestre, exclua as seguintes pastas das operações de antivírus:


- C:\Arquivos de Programas\Microsoft Azure Recovery Services Agent
- C:\ProgramData\ASR
- C:\ProgramData\ASRLogs
- C:\ProgramData\ASRSetupLogs
- C:\ProgramData\LogUploadServiceLogs
- C:\ProgramData\Microsoft Azure Site Recovery
- Diretório de instalação de servidor de processo, exemplo: C:\Program Files (x86) \Microsoft Azure Site Recovery

