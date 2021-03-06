---
title: Suporte para Java Enterprise para o Serviço de Aplicativo do Azure no Linux | Microsoft Docs
description: Guia do desenvolvedor para implantar aplicativos Java Enterprise usando o Wildfly com o Serviço de Aplicativo do Azure no Linux.
keywords: serviço de aplicativo do azure, aplicativo Web, linux, oss, java, wildfly, enterprise
services: app-service
author: rloutlaw
manager: angerobe
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 08/29/2018
ms.author: routlaw
ms.openlocfilehash: 6613def8891109e3a0ddf818111898a893a8035d
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/14/2018
ms.locfileid: "51628272"
---
# <a name="java-enterprise-guide-for-app-service-on-linux"></a>Guia do Java Enterprise para o Serviço de Aplicativo no Linux

O Serviço de Aplicativo do Azure no Linux permite que os desenvolvedores de Java criem, implantem e escalonem aplicativos JEE (Java Enterprise) em um serviço totalmente gerenciado baseado em Linux.  O ambiente de tempo de execução do Java Enterprise subjacente é o servidor de aplicativos de software livre [Wildfly](http://wildfly.org/).

Este guia fornece os principais conceitos e instruções para desenvolvedores de Java Enterprise que usam o Serviço de Aplicativo para Linux. Se você nunca implantou aplicativos Java com o Serviço de Aplicativo do Azure para Linux, você deve concluir o [início rápido do Java](quickstart-java.md) primeiro. Dúvidas sobre o Serviço de Aplicativo para Linux que não são específicas do Java Enterprise são respondidas no [guia do desenvolvedor de Java](app-service-linux-java.md) e nas [Perguntas frequentes do Serviço de Aplicativo no Linux](app-service-linux-faq.md).

## <a name="scale-with-app-service"></a>Escalonar com o Serviço de Aplicativo 

O servidor de aplicativos WildFly em execução no Serviço de Aplicativo no Linux é executado em modo autônomo, não em uma configuração de domínio. 

 Dimensione seu aplicativo vertical ou horizontalmente com [regras de dimensionamento](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-autoscale-get-started?toc=%2Fazure%2Fapp-service%2Fcontainers%2Ftoc.json) e [aumentando sua contagem de instâncias](https://docs.microsoft.com/azure/app-service/web-sites-scale?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json).

## <a name="customize-application-server-configuration"></a>Personalizar a configuração do servidor de aplicativos

Os desenvolvedores podem escrever um script Bash de inicialização para executar uma configuração adicional necessária para seu aplicativo como:

- Configurar fontes de dados
- Configurar provedores de mensagens
- Adicionar outros módulos e dependências à configuração do servidor Wildfly.

 O script é executado quando o Wildfly está em execução, mas antes que o aplicativo seja iniciado. O script deve usar a [CLI JBOSS](https://docs.jboss.org/author/display/WFLY/Command+Line+Interface) chamada de `/opt/jboss/wildfly/bin/jboss-cli.sh` para configurar o servidor de aplicativos com qualquer configuração ou com alterações necessárias após o servidor ser iniciado. 

Não use o modo interativo da CLI para configurar o Wildfly. Em vez disso, é possível fornecer um script de comandos da CLI JBoss usando o comando `--file`, por exemplo:

```bash
/opt/jboss/wildfly/bin/jboss-cli.sh -c --file=/path/to/your/jboss_commands.cli
```

Faça upload do script de inicialização para `/home/site/deployments/tools` na instância do Serviço de Aplicativo. Confira [este documento](/azure/app-service/app-service-deployment-credentials#userscope) para obter instruções sobre como obter as credenciais de FTP. 

Defina o campo **Script de inicialização** no portal do Azure como a localização do seu script de shell de inicialização, por exemplo `/home/site/deployments/tools/your-startup-script.sh`.

Use as [configurações de aplicativo](/azure/app-service/web-sites-configure#application-settings) para definir as variáveis de ambiente para uso no script. Essas configurações são disponibilizadas para o ambiente de script de inicialização e mantêm as cadeias de conexão e outros segredos fora do controle de versão.

## <a name="modules-and-dependencies"></a>Módulos e dependências

Para instalar módulos e suas dependências no classpath do Wildfly por meio da CLI JBoss, será necessário criar os seguintes arquivos em seu próprio diretório.  Alguns módulos e dependências talvez precisem de configuração adicional, como nomenclatura JNDI ou outra configuração específica de API; portanto, essa lista é um conjunto mínimo do que será necessário para configurar uma dependência na maioria dos casos.

- Um [descritor do módulo XML](https://jboss-modules.github.io/jboss-modules/manual/#descriptors). Esse arquivo XML define o nome, os atributos e as dependências do seu módulo. Esse [arquivo module.xml de exemplo](https://access.redhat.com/documentation/en-us/jboss_enterprise_application_platform/6/html/administration_and_configuration_guide/example_postgresql_xa_datasource) define um módulo Postgres, sua dependência do JDBC de arquivo JAR e outras dependências de módulo necessárias.
- Quaisquer dependências de arquivo JAR necessárias para seu módulo.
- Um script com seus comandos da CLI JBoss para configurar o novo módulo. Esse arquivo conterá seus comandos para serem executados pela CLI JBoss para configurar o servidor para usar a dependência. Para obter documentação sobre os comandos para adicionar módulos e provedores de mensagens, veja [este documento](https://access.redhat.com/documentation/red_hat_jboss_enterprise_application_platform/7.0/html-single/management_cli_guide/#how_to_cli).
-  Um script de inicialização de Bash para chamar a CLI JBoss e executar o script na etapa anterior. Esse arquivo será executado quando a instância do Serviço de Aplicativo for reiniciada ou quando novas instâncias forem provisionadas durante uma expansão.  Nesse script de inicialização, você pode executar quaisquer outras configurações para seu aplicativo conforme os comandos JBoss são passados para a CLI JBoss. No mínimo, esse arquivo pode ser um único comando para passar seu script de comando da CLI JBoss para a CLI JBoss: 
   
```bash
`/opt/jboss/wildfly/bin/jboss-cli.sh -c --file=/path/to/your/jboss_commands.cli` 
``` 

Depois que você tiver os arquivos e o conteúdo do seu módulo, siga as etapas abaixo para adicionar o módulo ao servidor de aplicativos Wildfly. 

1. Carregue seus arquivos por FTP para o `/home/site/deployments/tools` na sua instância do Serviço de Aplicativo. Confira este documento para obter instruções sobre como obter as credenciais de FTP. 
2. Na folha Configurações do Aplicativo do portal do Azure, defina o campo "Script de inicialização" como a localização do seu script de shell de inicialização, por exemplo `/home/site/deployments/tools/your-startup-script.sh`.
3. Reinicie sua instância do Serviço de Aplicativo pressionando o botão **Reiniciar** na seção **Visão geral** do portal ou usando a CLI do Azure.

## <a name="data-sources"></a>Fontes de dados

Para configurar o Wildfly para uma conexão de fonte de dados, siga o mesmo procedimento descrito acima na seção Instalando módulos e dependências. É possível seguir as mesmas etapas para qualquer serviço de Banco de dados do Azure.

1. Baixe o driver JDBC para seu tipo de banco de dados. Para sua conveniência, confira os drivers do [Postgres](https://jdbc.postgresql.org/download.html) e do [MySQL](https://dev.mysql.com/downloads/connector/j/). Desempacote o download para obter o arquivo .jar.
2. Siga as etapas descritas em "Módulos e dependências" para criar e fazer upload do descritor de módulo XML, script da CLI JBoss, script de inicialização e dependência .jar do JDBC.


Estão disponíveis mais informações sobre como configurar o Wildfly com [PostgreSQL](https://developer.jboss.org/blogs/amartin-blog/2012/02/08/how-to-set-up-a-postgresql-jdbc-driver-on-jboss-7), [MySQL](https://dev.mysql.com/doc/connector-j/5.1/connector-j-usagenotes-jboss.html) e [Banco de Dados SQL](https://docs.jboss.org/jbossas/docs/Installation_And_Getting_Started_Guide/5/html/Using_other_Databases.html#d0e3898). É possível usar essas instruções personalizadas juntamente com a abordagem generalizada acima para adicionar definições de fonte de dados ao seu servidor.

## <a name="messaging-providers"></a>Provedores de mensagens

Para habilitar Beans controlado por mensagens usando o Barramento de Serviço como o mecanismo de mensagens:

1. Use a [biblioteca de mensagens do Apache QPId JMS](https://qpid.apache.org/proton/index.html). Inclua essa dependência em seu pom.xml (ou outro arquivo de build) para o aplicativo.

2.  Crie os [recursos do Barramento de Serviço](/azure/service-bus-messaging/service-bus-java-how-to-use-jms-api-amqp). Crie um namespace do Barramento de Serviço do Azure, uma fila dentro desse namespace e uma Política de acesso compartilhada com funcionalidades de envio e recebimento.

3. Passe a chave da política de acesso compartilhada para seu código por meio da codificação de URL da chave primária de sua política ou [Use o SDK do Barramento de Serviço](/azure/service-bus-messaging/service-bus-java-how-to-use-jms-api-amqp#setup-jndi-context-and-configure-the-connectionfactory).

4. Siga as etapas descritas na seção Instalando Módulos e Dependências com seu descritor XML do módulo, dependências .jar, comandos da CLI JBoss e script de inicialização do provedor JMS. Além dos quatro arquivos, também será necessário criar um arquivo XML que define o nome JNDI para a fila JMS e para o tópico. Confira [este repositório](https://github.com/JasonFreeberg/widlfly-server-configs/tree/master/appconfig) para arquivos de configuração de referência.


## <a name="configure-session-management-caching"></a>Configurar o cache de gerenciamento de sessão

Por padrão, o Serviço de Aplicativo no Linux usará cookies de afinidade de sessão para garantir que as solicitações de cliente com sessões existentes sejam roteadas para a mesma instância do seu aplicativo. Esse comportamento padrão não requer nenhuma configuração, mas tem algumas limitações:

- Se uma instância do aplicativo for reiniciada ou reduzida verticalmente, o estado de sessão do usuário no servidor de aplicativos será perdido.
- Se os aplicativos tiverem configurações de tempo limite de sessão longo ou um número fixo de usuários, poderá levar algum tempo para que novas instâncias com dimensionamento automático recebam carga, uma vez que apenas novas sessões serão roteadas a instâncias recém-iniciadas.

É possível configurar o Wildfly para usar um armazenamento de sessão externa como o [Cache Redis](/azure/redis-cache/). Será necessário [desabilitar a configuração de afinidade de instância do ARR existente](https://azure.microsoft.com/blog/disabling-arrs-instance-affinity-in-windows-azure-web-sites/) para desligar o roteamento baseado em cookie de sessão e permitir que o armazenamento de sessão do Wildfly configurado opere sem interferência.

## <a name="enable-web-sockets"></a>Habilitar soquetes Web

Por padrão, os soquetes Web são habilitados no Serviço de Aplicativo. Para começar a usar o WebSockets em seu aplicativo, veja [este início rápido](https://github.com/wildfly/quickstart/tree/master/websocket-hello).

## <a name="logs-and-troubleshooting"></a>Logs e solução de problemas

O Serviço de Aplicativo oferece ferramentas para ajudar a solucionar problemas com seu aplicativo.

-   Ative o log clicando em **Logs de diagnóstico** no painel de navegação à esquerda. Clique em **Sistema de Arquivos** para definir o período de retenção e a cota de armazenamento e salve suas alterações. É possível encontrar esses logs em `/home/LogFiles/`.
-   [Use o SSH para conectar-se à instância de aplicativo](/app-service-linux-ssh-support) para exibir logs para aplicativos em execução.
-   Verifique os logs de diagnóstico no painel **Logs de diagnóstico** do Portal ou usando o comando da CLI do Azure: ` az webapp log tail --name <your-app-name> --resource-group <your-apps-resource-group> `
