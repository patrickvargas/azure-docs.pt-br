---
title: Criar um aplicativo Web Python no Serviço de Aplicativo do Azure no Linux | Microsoft Docs
description: Implante seu primeiro aplicativo Python Olá, Mundo no Serviço de Aplicativo do Azure no Linux em minutos.
services: app-service\web
documentationcenter: ''
author: cephalin
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 10/09/2018
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 5f9d694d47c0bd0765af7871d7e535a4174c30ff
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/18/2018
ms.locfileid: "49404703"
---
# <a name="create-a-python-web-app-in-azure-app-service-on-linux-preview"></a>Criar um aplicativo Web Python no Serviço de Aplicativo do Azure no Linux (versão prévia)

O [Serviço de Aplicativo no Linux](app-service-linux-intro.md) fornece um serviço de hospedagem na Web altamente escalonável e com aplicação automática de patches usando o sistema operacional Linux. Este início rápido mostra como implantar um aplicativo Python com base na imagem Python interna (versão prévia) no Serviço de Aplicativo no Linux usando a [CLI do Azure](/cli/azure/install-azure-cli).

Você pode seguir as etapas deste artigo usando um computador Mac, Windows ou Linux.

![Aplicativo de exemplo em execução no Azure](media/quickstart-python/hello-world-in-browser.png)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este guia de início rápido:

* <a href="https://www.python.org/downloads/" target="_blank">Instale o Python 3.7</a>
* <a href="https://git-scm.com/" target="_blank">Instalar o Git</a>

## <a name="download-the-sample"></a>Baixar o exemplo

Em uma janela do terminal, execute os comandos a seguir para clonar o aplicativo de exemplo no computador local e navegue para o diretório que contém o código de exemplo.

```bash
git clone https://github.com/Azure-Samples/python-docs-hello-world
cd python-docs-hello-world
```

## <a name="run-the-app-locally"></a>Executar o aplicativo localmente

Execute o aplicativo no local para ver como ele deve ficar quando o implantar no Azure. Abra uma janela do terminal e use os comandos a seguir para instalar as dependências necessárias e iniciar o servidor de desenvolvimento interno. 

```bash
# In Bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
FLASK_APP=application.py flask run

# In PowerShell
py -3 -m venv env
env\scripts\activate
pip install -r requirements.txt
Set-Item Env:FLASK_APP ".\application.py"
flask run
```

Abra um navegador da Web e navegue até o aplicativo de exemplo em `http://localhost:5000/`.

Você verá o **Olá, Mundo!** mensagem do aplicativo de exemplo exibida na página.

![Aplicativo de exemplo em execução local](media/quickstart-python/hello-world-in-browser.png)

Na janela do terminal, pressione **Ctrl+C** para sair do servidor Web.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [Configure deployment user](../../../includes/configure-deployment-user.md)]

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group-linux.md)]

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux.md)]

## <a name="create-a-web-app"></a>Criar um aplicativo Web

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-web-app-python-linux-no-h.md)]

Navegue até o site para ver seu aplicativo Web recém-criado com imagem interna. Substitua _&lt;nome do aplicativo>_ por um nome exclusivo do aplicativo web.

```bash
http://<app_name>.azurewebsites.net
```

Seu novo aplicativo Web deve ficar assim:

![Página de aplicativo Web vazia](media/quickstart-php/app-service-web-service-created.png)

[!INCLUDE [Push to Azure](../../../includes/app-service-web-git-push-to-azure.md)] 

```bash
Counting objects: 42, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (39/39), done.
Writing objects: 100% (42/42), 9.43 KiB | 0 bytes/s, done.
Total 42 (delta 15), reused 0 (delta 0)
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id 'c40efbb40e'.
remote: Generating deployment script.
remote: Generating deployment script for python Web Site
.
.
.
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Deployment successful.
remote: App container will begin restart within 10 seconds.
To https://user2234@cephalin-python.scm.azurewebsites.net/cephalin-python.git
 * [new branch]      master -> master
 ```

## <a name="browse-to-the-app"></a>Navegar até o aplicativo

Navegue até o aplicativo implantado usando o navegador da Web.

```bash
http://<app_name>.azurewebsites.net
```

O código Python de exemplo está em execução em um aplicativo Web com a imagem interna.

![Aplicativo de exemplo em execução no Azure](media/quickstart-python/hello-world-in-browser.png)

**Parabéns!** Você implantou seu primeiro aplicativo Python no Serviço de Aplicativo no Linux.

## <a name="update-locally-and-redeploy-the-code"></a>Atualizar localmente e reimplantar o código

No repositório local, abra o arquivo `application.py` e faça uma pequena alteração no texto na última linha:

```python
return "Hello Azure!"
```

Confirme suas alterações no Git e, em seguida, envie as alterações de código por push para o Azure.

```bash
git commit -am "updated output"
git push azure master
```

Depois que a implantação for concluída, troque para a janela do navegador aberta na etapa **Navegar até o aplicativo** e atualize a página.

![Aplicativo de exemplo atualizado em execução no Azure](media/quickstart-python/hello-azure-in-browser.png)

## <a name="manage-your-new-azure-web-app"></a>Gerenciar seu novo aplicativo Web do Azure

Vá para o <a href="https://portal.azure.com" target="_blank">portal do Azure</a> para gerenciar o aplicativo Web que você criou.

No menu à esquerda, clique em **Serviços de Aplicativos** e então clique no nome do seu aplicativo Web do Azure.

![Navegação do portal para o aplicativo Web do Azure](./media/quickstart-python/app-service-list.png)

A página Visão Geral do seu aplicativo Web é exibida. Aqui você pode executar tarefas básicas de gerenciamento como procurar, parar, iniciar, reiniciar e excluir.

![Página Serviço de Aplicativo no portal do Azure](media/quickstart-python/app-service-detail.png)

O menu à esquerda fornece páginas diferentes para configurar seu aplicativo. 

[!INCLUDE [cli-samples-clean-up](../../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Próximas etapas

A imagem interna do Python no Serviço de Aplicativo no Linux está atualmente em versão prévia, sendo possível personalizar o comando usado para iniciar o aplicativo. Nesse caso, também é possível criar aplicativos Python de produção usando um contêiner personalizado.

> [!div class="nextstepaction"]
> [Python com PostgreSQL](tutorial-python-postgresql-app.md)

> [!div class="nextstepaction"]
> [Configurar um comando de inicialização personalizado](how-to-configure-python.md#custom-startup-command)

> [!div class="nextstepaction"]
> [Solução de problemas](how-to-configure-python.md#troubleshooting)

> [!div class="nextstepaction"]
> [Usar imagens personalizadas](tutorial-custom-docker-image.md)
