---
title: Um início rápido para criar um aplicativo Web Java no Serviço de Aplicativo do Azure no Linux
description: Neste início rápido, você implanta seu primeiro Olá, Mundo em Java no Serviço de Aplicativo do Azure no Linux em minutos.
services: app-service\web
documentationcenter: ''
author: msangapu
manager: cfowler
editor: ''
ms.assetid: 582bb3c2-164b-42f5-b081-95bfcb7a502a
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: quickstart
ms.date: 03/07/2018
ms.author: msangapu
ms.custom: mvc
ms.openlocfilehash: e286942f092d2e8c22824a18f5a6503d04a1be0c
ms.sourcegitcommit: 1d3353b95e0de04d4aec2d0d6f84ec45deaaf6ae
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/30/2018
ms.locfileid: "50247548"
---
# <a name="quickstart-create-a-java-web-app-in-app-service-on-linux"></a>Início Rápido: criar um aplicativo Web Java no Serviço de Aplicativo do Azure no Linux

O [Serviço de Aplicativo no Linux](app-service-linux-intro.md) fornece um serviço de hospedagem na Web altamente escalonável e com aplicação automática de patches usando o sistema operacional Linux. Este início rápido mostra como usar a [CLI do Azure](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) com o [Plug-in do Maven para aplicativos Web do Azure (versão prévia)](https://github.com/Microsoft/azure-maven-plugins/tree/develop/azure-webapp-maven-plugin) para implantar um arquivo WAR (arquivo Web) de aplicativo Web Java.

![Aplicativo de exemplo em execução no Azure](media/quickstart-java/java-hello-world-in-browser.png)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-a-java-app"></a>Criar um aplicativo Java

Execute o seguinte comando do Maven no prompt do Cloud Shell para criar um novo aplicativo Web chamado `helloworld`:

```bash
mvn archetype:generate -DgroupId=example.demo -DartifactId=helloworld -DarchetypeArtifactId=maven-archetype-webapp
```

## <a name="configure-the-maven-plugin"></a>Configurar o plug-in do Maven

Para implantar do Maven, use o editor de códigos no Cloud Shell para abrir arquivo de projeto `pom.xml` no diretório `helloworld`. 

```bash
code pom.xml
```

Em seguida, adicione a seguinte definição de plug-in ao elemento `<build>` do arquivo `pom.xml`.

```xml
<plugins>
    <!--*************************************************-->
    <!-- Deploy to Tomcat in App Service Linux           -->
    <!--*************************************************-->
      
    <plugin>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-webapp-maven-plugin</artifactId>
        <version>1.4.0</version>
        <configuration>
   
            <!-- Web App information -->
            <resourceGroup>${RESOURCEGROUP_NAME}</resourceGroup>
            <appName>${WEBAPP_NAME}</appName>
            <region>${REGION}</region>
   
            <!-- Java Runtime Stack for Web App on Linux-->
            <linuxRuntime>tomcat 8.5-jre8</linuxRuntime>
   
        </configuration>
    </plugin>
</plugins>
```    


> [!NOTE] 
> Neste artigo, estamos trabalhando apenas com aplicativos Java empacotados em arquivos WAR. O plug-in também oferece suporte a aplicativos Web JAR, visite [Implantar um arquivo JAR do Java SE para o Serviço de Aplicativo no Linux](https://docs.microsoft.com/java/azure/spring-framework/deploy-spring-boot-java-app-with-maven-plugin?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json) para experimentá-lo.


Atualize os seguintes espaços reservados na configuração do plug-in:

| Placeholder | DESCRIÇÃO |
| ----------- | ----------- |
| `RESOURCEGROUP_NAME` | Nome do novo grupo de recursos no qual criar o aplicativo Web. Ao colocar todos os recursos para um aplicativo em um grupo, você pode gerenciá-los juntos. Por exemplo, excluir o grupo de recursos excluiria todos os recursos associados ao aplicativo. Atualize esse valor com um novo nome de grupo de recursos exclusivo, por exemplo, *TestResources*. Você usará esse nome de grupo de recursos para limpar todos os recursos do Azure em uma seção posterior. |
| `WEBAPP_NAME` | O nome do aplicativo será parte do nome do host do aplicativo Web quando implantado no Azure (NOME_APLICATIVO_WEB.azurewebsites.net). Atualize esse valor com um nome exclusivo para o novo aplicativo Web do Azure que irá hospedar seu aplicativo Java, por exemplo, *contoso*. |
| `REGION` | Uma região do Azure onde o aplicativo Web está hospedado, por exemplo, `westus2`. Você pode obter uma lista de regiões do Cloud Shell ou da CLI usando o comando `az account list-locations`. |

## <a name="deploy-the-app"></a>Implantar o aplicativo

Implante seu aplicativo Java no Azure usando o seguinte comando:

```bash
mvn package azure-webapp:deploy
```

Após a conclusão da implantação, navegue até o aplicativo implantado usando a URL a seguir no navegador da Web, por exemplo, `http://<webapp>.azurewebsites.net/helloworld`. 

![Aplicativo de exemplo em execução no Azure](media/quickstart-java/java-hello-world-in-browser-curl.png)

**Parabéns!** Você implantou seu primeiro aplicativo Java no Serviço de Aplicativo no Linux.


[!INCLUDE [cli-samples-clean-up](../../../includes/cli-samples-clean-up.md)]


## <a name="next-steps"></a>Próximas etapas

Neste início rápido, você usou o Maven para criar um aplicativo Web Java, configurou o [Plug-in do Maven para aplicativos Web do Azure](https://github.com/Microsoft/azure-maven-plugins/tree/develop/azure-webapp-maven-plugin) e, em seguida, implantou um aplicativo Java empacotado como arquivo Web no Serviço de Aplicativo no Linux. Para saber como se conectar bancos de dados, configurar o registro em log e o monitoramento, configurar a segurança e definir opções de tempo de execução, prossiga para o Guia do desenvolvedor de Java para o Serviço de Aplicativo no Linux.

> [!div class="nextstepaction"]
> [Guia do desenvolvedor do Serviço de Aplicativo no Java do Linux](app-service-linux-java.md)

