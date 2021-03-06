---
title: Automatizar a validação de pilha do Azure com o PowerShell | Microsoft Docs
description: Você pode automatizar a validação de pilha do Azure com o PowerShell.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 11/26/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: 3c3e064ea229db97c59cc5b49107b568a2fdaa98
ms.sourcegitcommit: 922f7a8b75e9e15a17e904cc941bdfb0f32dc153
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/27/2018
ms.locfileid: "52334441"
---
# <a name="automate-azure-stack-validation-with-powershell"></a>Automatizar a validação de pilha do Azure com o PowerShell

Validação como um serviço (VaaS) fornece a capacidade de automatizar a inicialização de testes usando o **LaunchVaaSTests.ps1** script.

Você pode usar o PowerShell para o fluxo de trabalho a seguir:

- Aprovação do teste

Neste tutorial, você aprenderá a criar um script que:

> [!div class="checklist"]
> * Instala pré-requisitos
> * Instala e inicia o agente local
> * Inicia uma categoria de testes, como a integração, funcional, confiabilidade
> * Relatórios de resultados de teste

## <a name="launch-the-test-pass-workflow"></a>Iniciar o fluxo de trabalho de aprovação do teste

1. Abra um prompt elevado do PowerShell.

2. Execute o script a seguir para baixar o script de automação:

    ```PowerShell
    New-Item -ItemType Directory -Path <VaaSLaunchDirectory>
    Set-Location <VaaSLaunchDirectory>
    Invoke-WebRequest -Uri https://storage.azurestackvalidation.com/packages/Microsoft.VaaS.Scripts.latest.nupkg -OutFile "LaunchVaaS.zip"
    Expand-Archive -Path ".\LaunchVaaS.zip" -DestinationPath .\ -Force
    ```

3. Execute o script a seguir com os valores de parâmetro apropriado:

    ```PowerShell
    $VaaSAccountCreds = New-Object System.Management.Automation.PSCredential "<VaaSUserId>", (ConvertTo-SecureString "<VaaSUserPassword>" -AsPlainText -Force)
    .\LaunchVaaSTests.ps1 -VaaSAccountCreds $VaaSAccountCreds `
                          -VaaSAccountTenantId <VaaSAccountTenantId> `
                          -VaaSSolutionName <VaaSSolutionName> `
                          -VaaSTestPassName <VaaSTestPassName> `
                          -VaaSTestCategories Integration,Functional `
                          -MaxScriptWaitTimeInHours 12 `
                          -ServiceAdminUserName <AzSServiceAdminUser> `
                          -ServiceAdminUserPassword <AzSServiceAdminPassword> `
                          -TenantAdminUserName <AzSTenantAdminUser> `
                          -TenantAdminUserPassword <AzSTenantAdminPassword> `
                          -CloudAdminUserName <AzSCloudAdminUser> `
                          -CloudAdminUserPassword <AzSCloudAdminPassword>
    ```

    **Parâmetros**

    | Parâmetro | DESCRIÇÃO |
    | --- | --- |
    | VaaSUserld | Sua ID de usuário VaaS. |
    | VaaSUserPassword | Sua senha VaaS. |
    | VaaSAccountTenantId | O GUID de locatário VaaS. |
    | VaaSSolutionName | O nome da solução VaaS sob a qual passar o teste será executado. |
    | VaaSTestPassName | O nome do teste VaaS passar um fluxo de trabalho para criar. |
    | VaaSTestCategories | `Integration`, `Functional`, ou. `Reliability`. Se você usar vários valores, separe cada valor por uma vírgula.  |
    | ServiceAdminUserName | Sua conta de administrador de serviço do Azure Stack.  |
    | ServiceAdminPassword | Sua senha de serviço do Azure Stack.  |
    | TenantAdminUserName | O administrador para o locatário principal.  |
    | TenantAdminPassword | A senha para o locatário principal.  |
    | CloudAdminUserName | O nome de usuário administrador de nuvem.  |
    | CloudAdminPassword | A senha do administrador da nuvem.  |

4. Examine os resultados do teste. Para obter outras opções, consulte [monitorar e gerenciar testes no portal do VaaS](azure-stack-vaas-monitor-test.md).

## <a name="next-steps"></a>Próximas etapas

Para saber mais sobre o PowerShell no Azure Stack, examine os módulos mais recente.

> [!div class="nextstepaction"]
> [Módulo do Azure Stack](https://docs.microsoft.com/powershell/azure/azure-stack/overview?view=azurestackps-1.5.0)
