---
title: 'Início Rápido: criar um workspace do serviço do Machine Learning no portal do Azure – Azure Machine Learning'
description: Use o portal do Azure para criar um workspace do Azure Machine Learning. Este espaço de trabalho é o bloco fundamental na nuvem para experimentação, treinamento e implantação de modelos de aprendizado de máquina com o serviço do Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: quickstart
ms.reviewer: sgilley
author: rastala
ms.author: roastala
ms.date: 09/24/2018
ms.openlocfilehash: 7ed45b5e8a8c3cab26c0998260055ffd7a0f0c5d
ms.sourcegitcommit: a4e4e0236197544569a0a7e34c1c20d071774dd6
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51710249"
---
# <a name="quickstart-use-the-azure-portal-to-get-started-with-azure-machine-learning"></a>Início Rápido: usar o portal do Azure para começar a usar o Azure Machine Learning

Neste início rápido, você usa o portal do Azure para criar um espaço de trabalho do Azure Machine Learning. Esse workspace é o bloco fundamental na nuvem para experimentação, treinamento e implantação de modelos de aprendizado de máquina com o serviço do Machine Learning. Este início rápido usa recursos de nuvem e não exige nenhuma instalação. Para configurar seu próprio servidor de notebook Jupyter, veja [Início Rápido: Usar o Python para uma introdução ao Azure Machine Learning](quickstart-create-workspace-with-python.md).

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE2F9Ad]

Neste início rápido, você:

* Criar um workspace na assinatura do Azure.
* Experimente com o Python em um Azure Notebook e registre em log valores em várias iterações.
* Exiba os valores registrados em log no seu espaço de trabalho.

Os seguintes recursos do Azure serão adicionados automaticamente ao workspace quando estiverem disponíveis regionalmente:

  - [Registro de Contêiner do Azure](https://azure.microsoft.com/services/container-registry/)
  - [Armazenamento do Azure](https://azure.microsoft.com/services/storage/)
  - [Azure Application Insights](https://azure.microsoft.com/services/application-insights/) 
  - [Cofre da Chave do Azure](https://azure.microsoft.com/services/key-vault/)

Os recursos que você cria podem ser usados como pré-requisitos em outros tutoriais e artigos de instruções do serviço do Machine Learning. Como com outros serviços do Azure, há limites em determinados recursos associados ao Machine Learning. Um exemplo é o tamanho do cluster de IA do Lote do Azure. Para obter informações sobre limites padrão e sobre como aumentar sua cota, consulte [este artigo](how-to-manage-quotas.md).

Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://aka.ms/AMLfree) antes de começar.


## <a name="create-a-workspace"></a>Criar um workspace 

[!INCLUDE [aml-create-portal](../../../includes/aml-create-in-portal.md)]

Na página do espaço de trabalho, selecione `Explore your Azure Machine Learning service workspace`.

 ![Explorar espaço de trabalho](./media/quickstart-get-started/explore_aml.png)


## <a name="use-the-workspace"></a>Usar o workspace

Agora veja como um workspace ajuda você a gerenciar seus scripts de aprendizado de máquina. Nesta seção, você:

* Abra um notebook no Azure Notebooks.
* Execute o código que cria alguns valores registrados em log.
* Exiba os valores registrados em log no seu espaço de trabalho.

Este exemplo mostra como o espaço de trabalho pode ajudar a manter o controle das informações geradas em um script. 

### <a name="open-a-notebook"></a>Abra um notebook 

O Azure Notebooks fornece uma plataforma de nuvem gratuita para Jupyter Notebooks que estão previamente configurados com tudo o que você precisa para executar o Machine Learning.  

Selecione `Open Azure Notebooks` para tentar seu primeiro experimento.

 ![Abrir Azure Notebooks](./media/quickstart-get-started/explore_ws.png)

Sua organização poderá exigir [consentimento do administrador](https://notebooks.azure.com/help/signing-up/work-or-school-account/admin-consent) antes de poder entrar.

Depois de entrar, uma nova guia é aberta e um prompt `Clone Library` é exibido. Selecione `Clone`


### <a name="run-the-notebook"></a>Executar o notebook

Junto com dois notebooks, você verá um arquivo `config.json`. Este arquivo de configuração contém informações sobre o espaço de trabalho que você criou.  

Selecione `01.run-experiment.ipynb` para abrir o notebook.

Para executar as células uma de cada vez, use `Shift`+`Enter`. Ou selecione `Cells` > `Run All` para executar o notebook inteiro. Quando você vir um asterisco [*] ao lado de uma célula, ele está em execução. Após o código para a célula ser concluído, um número é exibido. 

Depois de concluir a execução de todas as células no bloco de anotações, você poderá exibir os valores registrados em seu espaço de trabalho.

## <a name="view-logged-values"></a>Exibir valores registrados em log

Depois de executar todas as células no notebook, volte à página do portal.  

Selecione `View Experiments`.

![Exibir experimentos](./media/quickstart-get-started/view_exp.png)

Feche o pop-up `Reports`.

Selecione `my-first-experiment`.

Veja informações sobre a execução que você acabou de executar. Role a página para baixo para encontrar a tabela de execuções. Selecione o link de número de execução.

 ![Link do histórico de execução](./media/quickstart-get-started/report.png)

Você verá gráficos criados automaticamente dos valores registrados. Sempre que registra diversos valores com o mesmo parâmetro de nome, um gráfico é gerado automaticamente para você.

   ![Exibir histórico](./media/quickstart-get-started/plots.png)

Uma vez que o código para aproximar o pi usa valores aleatórios, seus gráficos mostrarão valores diferentes.  

## <a name="clean-up-resources"></a>Limpar recursos 

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

Você também pode manter o grupo de recursos, mas excluir um único espaço de trabalho. Exiba as propriedades do espaço de trabalho e, em seguida, selecione **Excluir**.

## <a name="next-steps"></a>Próximas etapas

Você criou os recursos necessários para experimentar e implantar modelos. Você também executou código em um notebook. E explorou o histórico de execução desse código em seu espaço de trabalho na nuvem.

Para obter uma experiência de fluxo de trabalho detalhado, siga os tutoriais do Machine Learning para treinar e implantar um modelo.  

> [!div class="nextstepaction"]
> [Tutorial: Treinar um modelo de classificação de imagem](tutorial-train-models-with-aml.md)
