---
title: Validar pacotes do fabricante original do equipamento (OEM) na validação de pilha do Azure como um serviço | Microsoft Docs
description: Saiba como validar os pacotes do fabricante original do equipamento (OEM) com a validação como um serviço.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 11/19/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: 8268a6b04d7ddbb35821999142d3a33bdd2bedcc
ms.sourcegitcommit: fa758779501c8a11d98f8cacb15a3cc76e9d38ae
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2018
ms.locfileid: "52261795"
---
# <a name="validate-oem-packages"></a>Validar pacotes de OEM

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Você pode testar um novo pacote de OEM quando houve uma alteração no firmware ou drivers para uma validação de solução completa. Quando seu pacote tiver passado o teste, ele é assinado pela Microsoft. O teste deve conter o pacote de extensão de OEM atualizado com os drivers e firmware que passaram logotipo do Windows Server e testes PCS.

[!INCLUDE [azure-stack-vaas-workflow-validation-completion](includes/azure-stack-vaas-workflow-validation-completion.md)]

> [!IMPORTANT]
> Antes de carregar ou envio de pacotes, examine [criar um pacote de OEM](azure-stack-vaas-create-oem-package.md) para o formato de pacote esperado e o conteúdo.

## <a name="managing-packages-for-validation"></a>Gerenciamento de pacotes para validação

Ao usar o **validação do pacote** fluxo de trabalho para validar um pacote, você precisará fornecer uma URL para um **Azure Storage blob**. Esse blob é o pacote de OEM que foi instalado na solução no momento da implantação. Criar o blob usando a conta de armazenamento do Azure criada durante a instalação (consulte [configurar a validação como um serviço de recursos](azure-stack-vaas-set-up-resources.md)).

### <a name="prerequisite-provision-a-storage-container"></a>Pré-requisito: Provisionar um contêiner de armazenamento

Crie um contêiner na conta de armazenamento de blobs do pacote. Esse contêiner pode ser usado para todos os seus validação do pacote é executado.

1. No [Portal do Azure](https://portal.azure.com), vá para a conta de armazenamento criada na [configurar sua validação como um serviço de recursos](azure-stack-vaas-set-up-resources.md).
2. Na folha à esquerda sob **serviço Blob**, selecione na **contêineres**.
3. Selecione **+ contêiner** no menu da barra e forneça um nome para o contêiner, por exemplo, `vaaspackages`.

### <a name="upload-package-to-storage-account"></a>Carregar pacote para a conta de armazenamento

1. Prepare o pacote que você deseja validar. Se o seu pacote tiver vários arquivos, compactá-lo em um `.zip` arquivo.
2. No [Portal do Azure](https://portal.azure.com), selecione o contêiner de pacote e carregar o pacote selecionando na **carregar** na barra de menus.
3. Selecione o pacote `.zip` arquivo a ser carregado. Mantenha os padrões para **tipo de Blob** (ou seja, **Blob de blocos**) e **tamanho do bloco**.

> [!NOTE]
> Verifique se o `.zip` conteúdo é colocado na raiz do `.zip` arquivo. Não deve haver nenhuma subpasta no pacote.

### <a name="generate-package-blob-url-for-vaas"></a>Gerar a URL do blob de pacote para VaaS

Ao criar uma **validação de pacote** fluxo de trabalho no portal do VaaS, você precisará fornecer uma URL para o blob de armazenamento do Azure que contém o pacote.

#### <a name="option-1-generating-an-account-sas-url"></a>Opção 1: Gerar uma URL de SAS de conta

1. No [portal do Azure](https://portal.azure.com/), vá para sua conta de armazenamento e navegue até o arquivo. zip que contém o pacote

2. Selecione **gerar SAS** no menu de contexto

3. Selecione **leitura** de **permissões**

4. Definir **hora de início** para a hora atual, e **hora de término** pelo menos de 48 horas da **hora de início**. Se você for executar outros testes com o mesmo pacote, considere aumentar **hora de término** para a duração do teste. Todos os testes agendados pelo VaaS após **hora de término** falham e uma nova SAS será precisará ser gerado.

5. Selecione **gerar a URL e o token SAS do blob**

Use **URL de SAS de Blob** ao iniciar uma nova **validação de pacote** fluxo de trabalho no portal do VaaS.

#### <a name="option-2-using-public-read-container"></a>Opção 2: Usar o contêiner público de leitura

> [!CAUTION]
> Essa opção abre o contêiner para acesso anônimo de somente leitura.

1. Grant **acesso somente para blobs de leitura público** ao contêiner de pacote, seguindo as instruções na seção [conceder permissões a usuários anônimos a contêineres e blobs](https://docs.microsoft.com/azure/storage/storage-manage-access-to-resources#grant-anonymous-users-permissions-to-containers-and-blobs).

2. No contêiner do pacote, selecione no blob no contêiner de pacote para abrir o painel Propriedades.

3. Cópia de **URL**. Use esse valor ao iniciar uma nova **validação de pacote** fluxo de trabalho no portal do VaaS.

## <a name="apply-monthly-update"></a>Aplicar atualização mensal

[!INCLUDE [azure-stack-vaas-workflow-section_update-azs](includes/azure-stack-vaas-workflow-section_update-azs.md)]

## <a name="create-a-package-validation-workflow"></a>Criar um fluxo de trabalho de validação do pacote

1. Entrar para o [VaaS portal](https://azurestackvalidation.com).

2. [!INCLUDE [azure-stack-vaas-workflow-step_select-solution](includes/azure-stack-vaas-workflow-step_select-solution.md)]

3. Selecione **iniciar** sobre o **validação de pacote** lado a lado.

    ![Bloco de fluxo de trabalho de validações de pacote](media/tile_validation-package.png)

4. [!INCLUDE [azure-stack-vaas-workflow-step_naming](includes/azure-stack-vaas-workflow-step_naming.md)]

5. Insira a URL do blob de armazenamento do Azure para o pacote de OEM foi instalado na solução no momento da implantação. Para obter instruções, consulte [gerar a URL do blob de pacote para VaaS](#generate-package-blob-url-for-vaas).

6. [!INCLUDE [azure-stack-vaas-workflow-step_upload-stampinfo](includes/azure-stack-vaas-workflow-step_upload-stampinfo.md)]

7. [!INCLUDE [azure-stack-vaas-workflow-step_test-params](includes/azure-stack-vaas-workflow-step_test-params.md)]

    > [!NOTE]
    > Parâmetros de ambiente não podem ser modificados depois de criar um fluxo de trabalho.

8. [!INCLUDE [azure-stack-vaas-workflow-step_tags](includes/azure-stack-vaas-workflow-step_tags.md)]

9. [!INCLUDE [azure-stack-vaas-workflow-step_submit](includes/azure-stack-vaas-workflow-step_submit.md)]
    Você será redirecionado à página de resumo de testes.

## <a name="run-package-validation-tests"></a>Executar testes de validação de pacote

No **resumo de testes de validação de pacote** página, você verá uma lista dos testes são necessárias para concluir a validação. Executar testes nesse fluxo de trabalho por aproximadamente 24 horas.

[!INCLUDE [azure-stack-vaas-workflow-validation-section_schedule](includes/azure-stack-vaas-workflow-validation-section_schedule.md)]

Quando todos os testes foi concluído com êxito, envie o nome da sua solução VaaS e validação de pacote para [ vaashelp@microsoft.com ](mailto:vaashelp@microsoft.com) para solicitar a assinatura do pacote.

## <a name="next-steps"></a>Próximas etapas

- [Monitorar e gerenciar testes no portal VaaS](azure-stack-vaas-monitor-test.md)