---
title: Instalação e configuração do Gerenciamento de Modelos do Azure Machine Learning | Microsoft Docs
description: Este documento descreve as etapas e os conceitos envolvidos na instalação e na configuração do Gerenciamento de Modelos no Azure Machine Learning.
services: machine-learning
author: aashishb
ms.author: aashishb
manager: hjerez
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 12/6/2017
ROBOTS: NOINDEX
ms.openlocfilehash: 84068376f35bc4df6672cffcc0ac3438b12edbb5
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46979668"
---
# <a name="model-management-setup"></a>Configuração do gerenciamento de modelos

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)]


Este documento mostra como usar o Gerenciamento de Modelos do Azure ML para implantar e gerenciar seus modelos de Machine Learning como serviços Web. 

Usando o gerenciamento de modelos de ML do Azure, você pode implantar e gerenciar com eficiência modelos de Machine Learning criados usando várias estruturas, incluindo SparkML, Keras, TensorFlow, Kit de Ferramentas Cognitivas da Microsoft ou Python. 

Ao final deste documento, seu ambiente de gerenciamento de modelos deverá estar configurado e pronto para implantar seus modelos do Machine Learning.

## <a name="what-you-need-to-get-started"></a>Para começar, você precisa do seguinte:
Para aproveitar este guia ao máximo, você deve ter acesso de Colaborador a uma assinatura do Azure ou um grupo de recursos onde você possa implantar seus modelos.
A CLI vem pré-instalada no Azure Machine Learning Workbench e nas [DSVMs do Azure](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-virtual-machine-overview).

## <a name="using-the-cli"></a>Usando a CLI
Para usar as CLIs (interfaces de linha de comando) do Workbench, clique em **Arquivo** -> **Abrir o Prompt de Comando**. 

Em uma Máquina Virtual de Ciência de Dados, conecte-se e abra o prompt de comando. Digite `az ml -h` para ver as opções. Para obter mais detalhes sobre os comandos, use o sinalizador --help.

Em todos os outros sistemas, você precisa instalar as CLIs.

>[!NOTE]
> Em um Jupyter Notebook em uma DSVM do Linux, é possível acessar a CLI do Azure e CLI do Azure ML com o formato de comando abaixo.  **Isso é para um Jupyter Notebook em uma DSVM do Linux, especificamente**.  Esses comandos acessam o kernel atual do Python no notebook (por exemplo, o ambiente `py35` do conda)
>```
>import sys
>! {sys.executable} -m azure.cli login
>! {sys.executable} -m azure.cli ml -h
>```

### <a name="installing-or-updating-on-windows"></a>Instalando (ou atualizando) no Windows

Instale o Python a partir de https://www.python.org/. Verifique se você selecionou a instalação do pip.

Abra um prompt de comando usando Executar como administrador e execute os seguintes comandos:

```cmd
pip install -r https://aka.ms/az-ml-o16n-cli-requirements-file
```

### <a name="installing-or-updating-on-linux"></a>Instalando (ou atualizando) no Linux
Execute o seguinte comando na linha de comando e siga os prompts:

```bash
sudo -i
pip install -r https://aka.ms/az-ml-o16n-cli-requirements-file
```

### <a name="configuring-docker-on-linux"></a>Configurar o Docker no Linux
Para configurar o Docker no Linux para uso por usuários não raiz, siga as instruções aqui: [Etapas de pós-instalação para Linux](https://docs.docker.com/engine/installation/linux/linux-postinstall/)

>[!NOTE]
> Em uma DSVM Linux, você pode executar o script a seguir para configurar o Docker corretamente. **Lembre-se de fazer logoff e logon novamente após a execução do script.**
>```
>sudo /opt/microsoft/azureml/initial_setup.sh
>```

## <a name="deploying-your-model"></a>Implantando seu modelo
Use a CLI para implantar modelos como serviços Web. Os serviços Web podem ser implantados localmente ou em um cluster.

Comece com uma implantação local, valide que o modelo e o código funcionam e, em seguida, implante em um cluster para uso em escala de produção.

Para começar, você precisa configurar seu ambiente de implantação. A configuração do ambiente é uma tarefa de ocorrência única. Quando a instalação estiver concluída, você poderá reutilizar o ambiente para as próximas implantações. Consulte a seção a seguir para obter mais detalhes.

Ao concluir a configuração do ambiente:
- Será solicitado que você entre no Azure. Para entrar, utilize um navegador da Web para abrir a página https://aka.ms/devicelogin e insira o código fornecido para autenticar.
- Durante o processo de autenticação, será solicitado que você se autentique com uma conta. Importante: Selecione uma conta que tenha uma assinatura do Azure válida e permissões suficientes para criar recursos na conta. Quando o log-in é concluído, as suas informações de assinatura são apresentadas e você é perguntado se deseja continuar com a conta selecionada.

### <a name="environment-setup"></a>Configuração do ambiente
Para iniciar o processo de instalação, você precisa registrar alguns provedores de ambiente inserindo os seguintes comandos:

```azurecli
az provider register -n Microsoft.MachineLearningCompute
az provider register -n Microsoft.ContainerRegistry
az provider register -n Microsoft.ContainerService
```
#### <a name="local-deployment"></a>Implantação local
Para implantar e testar o serviço Web no computador local, configure um ambiente local usando o comando a seguir. O nome do grupo de recursos é opcional.

```azurecli
az ml env setup -l [Azure Region, e.g. eastus2] -n [your environment name] [-g [existing resource group]]
```
>[!NOTE] 
>A implantação do serviço Web local requer que você instale o Docker no computador local. 
>

O comando de instalação do ambiente local cria os seguintes recursos na sua assinatura:
- Um grupo de recursos (se não for fornecido ou se o nome fornecido não existir)
- Uma conta de armazenamento
- Um ACR (Registro de Contêiner do Azure)
- Uma conta do Application Insights

Após a configuração ser concluída com êxito, defina o ambiente a ser usado com o seguinte comando:

```azurecli
az ml env set -n [environment name] -g [resource group]
```

#### <a name="cluster-deployment"></a>Implantação de cluster
Use a implantação de Cluster para cenários de produção de grande escala. Ele configura um cluster do ACS com o Kubernetes como o orquestrador. O cluster do ACS pode ser expandido para lidar com a maior taxa de transferência para suas chamadas de serviço Web.

Para implantar o serviço Web em um ambiente de produção, primeiro configure o ambiente usando o seguinte comando:

```azurecli
az ml env setup --cluster -n [your environment name] -l [Azure region e.g. eastus2] [-g [resource group]]
```

O comando de instalação do ambiente de cluster cria os seguintes recursos na sua assinatura:
- Um grupo de recursos (se não for fornecido ou se o nome fornecido não existir)
- Uma conta de armazenamento
- Um ACR (Registro de Contêiner do Azure)
- Uma implantação do Kubernetes em um cluster do ACS (Serviço de Contêiner do Azure)
- Uma conta do Application Insights

>[!IMPORTANT]
> Para criar com êxito um ambiente de cluster, você precisará ter acesso de Colaborador na assinatura do Azure ou o grupo de recursos.

O grupo de recursos, a conta de armazenamento e o ACR são criados rapidamente. A implantação do ACS pode levar até 20 minutos. 

Para verificar o status de um provisionamento de cluster em andamento, use o seguinte comando:

```azurecli
az ml env show -n [environment name] -g [resource group]
```

Após a conclusão da instalação, você precisa definir o ambiente a ser usado para essa implantação. Use o seguinte comando:

```azurecli
az ml env set -n [environment name] -g [resource group]
```

>[!NOTE] 
> Depois que o ambiente for criado, para implantações subsequentes, você somente precisa usar o comando definido acima para reutilizá-lo.
>

### <a name="create-a-model-management-account"></a>Criar uma Conta de Gerenciamento de Modelos
Uma conta de gerenciamento de modelos é necessária para a implantação de modelos. Você precisa fazer isso uma vez por assinatura e pode reutilizar a mesma conta em várias implantações.

Para criar uma nova conta, use o comando a seguir:

```azurecli
az ml account modelmanagement create -l [Azure region, e.g. eastus2] -n [your account name] -g [resource group name] --sku-instances [number of instances, e.g. 1] --sku-name [Pricing tier for example S1]
```

Para usar uma conta existente, use o seguinte comando:
```azurecli
az ml account modelmanagement set -n [your account name] -g [resource group it was created in]
```

Como resultado desse processo, o ambiente está preparado e a conta de gerenciamento de modelo foi criada para fornecer os recursos necessários para gerenciar e implantar modelos de Machine Learning (consulte [Gerenciamento de Modelos do Machine Learning do Azure](model-management-overview.md) para ter um visão geral).

## <a name="next-steps"></a>Próximas etapas

* Para obter instruções sobre como implantar serviços da web para execução em um computador local ou em um cluster, continue em [Implantação de um modelo de Machine Learning como serviço Web](model-management-service-deploy.md).
* Tente uma das muitas amostras na Galeria.
