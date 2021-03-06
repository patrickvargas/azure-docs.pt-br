---
title: Orientação para desenvolvimento do Azure Functions | Microsoft Docs
description: Aprenda os conceitos e técnicas do Azure Functions que você precisa para desenvolver funções no Azure, em todas as linguagens de programação e associações.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
keywords: guia do desenvolvedor, azure functions, funções, processamento de eventos, webhooks, computação dinâmica, arquitetura sem servidor
ms.assetid: d8efe41a-bef8-4167-ba97-f3e016fcd39e
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 10/12/2017
ms.author: glenga
ms.openlocfilehash: 38d73f38a5e04a42ee15c9206ce760936e3e10c9
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46980297"
---
# <a name="azure-functions-developers-guide"></a>Guia do desenvolvedor do Azure Functions
No Azure Functions, funções específicas compartilham alguns componentes e conceitos técnicos, independentemente da linguagem ou da associação usada. Antes de aprender detalhes específicos de uma determinada linguagem ou binding, leia esta visão geral que se aplica a todos eles.

Este artigo pressupõe que você já tenha lido a [Visão geral do Azure Functions](functions-overview.md) e está familiarizado com [conceitos do SDK de WebJobs como gatilhos, associações e tempo de execução do JobHost](https://github.com/Azure/azure-webjobs-sdk/wiki). O Azure Functions é baseado no SDK de WebJobs. 

## <a name="function-code"></a>Código de função
Uma *função* é o principal conceito no Azure Functions. Você escreve o código para uma função em uma linguagem de sua escolha e salva os arquivos de código e configuração na mesma pasta. A configuração é denominada `function.json`, que contém dados de configuração JSON. Várias linguagens são compatíveis, e cada um tem uma experiência ligeiramente diferente otimizada para funcionar melhor para a linguagem em questão. 

O arquivo function.json define as associações de função e outras definições de configuração. O tempo de execução usa esse arquivo para determinar os eventos a serem monitorados, bem como para passar e retornar dados da execução da função. Veja a seguir um arquivo function.json de exemplo.

```json
{
    "disabled":false,
    "bindings":[
        // ... bindings here
        {
            "type": "bindingType",
            "direction": "in",
            "name": "myParamName",
            // ... more depending on binding
        }
    ]
}
```

Defina a propriedade `disabled` como `true` para impedir que a função seja executada.

A propriedade `bindings` é onde você configura gatilhos e associações. Cada associação compartilha algumas configurações comuns e outras que são específicas para um determinado tipo de associação. Todas as associações exigem as seguintes configurações:

| Propriedade | Valores/Tipos | Comentários |
| --- | --- | --- |
| `type` |string |Tipo de binding. Por exemplo: `queueTrigger`. |
| `direction` |'in', 'out' |Indica se a associação é para receber dados na função ou enviar dados a partir da função. |
| `name` |string |O nome que é usado para os dados associados na função. Em C#, esse é um nome de um argumento. Em JavaScript, é a chave em uma lista de chaves/valores. |

## <a name="function-app"></a>Aplicativo de funções
O aplicativo de funções fornece um contexto de execução no Azure no qual suas funções são executadas. Um aplicativo de funções é composto por uma ou mais funções individuais que são gerenciadas em conjunto pelo Serviço de Aplicativo do Azure. Todas as funções em um aplicativo de funções compartilham o mesmo plano de preços, a implantação contínua e a versão de tempo de execução. Pense em um aplicativo de funções como uma forma de organizar e gerenciar coletivamente suas funções. 

> [!NOTE]
> Começando pela [versão 2.x](functions-versions.md) do Azure Functions Runtime, todas as funções em um aplicativo de funções devem ser criadas na mesma linguagem.

## <a name="runtime"></a>Tempo de execução
O Azure Functions Runtime, ou host de script, é o host subjacente que escuta eventos, coleta e envia dados e, por fim, executa seu código. Esse mesmo host é usado pelo SDK do WebJobs.

Há também um host Web que manipula as solicitações de gatilho HTTP para o tempo de execução. Ter dois hosts ajuda a isolar o tempo de execução do tráfego de front-end gerenciado pelo host Web.

## <a name="folder-structure"></a>Estrutura de pastas
[!INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

Ao configurar um projeto para implantar funções em um aplicativo de funções no Azure, você poderá tratar essa estrutura de pastas como o código do site. É recomendável usar a [implantação de pacote](deployment-zip-push.md) para implantar seu projeto em seu aplicativo de funções no Azure. Você também pode usar ferramentas existentes, como [integração contínua e implantação](functions-continuous-deployment.md) e Azure DevOps.

> [!NOTE]
> Não se esqueça de implantar o arquivo `host.json` e as pastas de função diretamente na pasta `wwwroot`. Não inclua a pasta `wwwroot` nas implantações. Caso contrário, você acabará com pastas `wwwroot\wwwroot`.

## <a id="fileupdate"></a> Como atualizar os arquivos de aplicativo de funções
O editor de funções interno do portal do Azure permite que você atualize o arquivo *function.json* e o arquivo de código de uma função. Para carregar ou atualizar outros arquivos, como *package.json* ou *project.json*, ou dependências, você precisa usar outros métodos de implantação.

Os aplicativos de funções baseiam-se no Serviço de Aplicativo; portanto, todas as [opções de implantação disponíveis para aplicativos Web padrão](../app-service/app-service-deploy-local-git.md) também estão disponíveis para aplicativos de funções. Aqui estão alguns métodos que você pode usar para carregar ou atualizar os arquivos de aplicativos de função. 

#### <a name="use-local-tools-and-publishing"></a>Usar ferramentas locais e publicação
Aplicativos de funções podem ser criados e publicados usando várias ferramentas, incluindo [Visual Studio](./functions-develop-vs.md), [Visual Studio Code](functions-create-first-function-vs-code.md), [IntelliJ](./functions-create-maven-intellij.md), [Eclipse](./functions-create-maven-eclipse.md) e [Azure Functions Core Tools](./functions-develop-local.md). Para mais informações, confira [Codificar e testar o Azure Functions localmente](./functions-develop-local.md).

<!--NOTE: I've removed documentation on FTP, because it does not sync triggers on the consumption plan --glenga -->

#### <a name="continuous-deployment"></a>Implantação contínua
Siga as instruções no tópico [Implantação contínua para funções do Azure](functions-continuous-deployment.md).

## <a name="parallel-execution"></a>Execução paralela
Quando vários eventos de gatilho ocorrem mais rápido do que um tempo de execução single-threaded de função pode processar, o tempo de execução pode invocar a função várias vezes em paralelo.  Se um aplicativo de funções estiver usando o [Plano de hospedagem de consumo](functions-scale.md#how-the-consumption-plan-works), ele poderá escalar horizontalmente de maneira automática.  Cada instância do aplicativo de funções, quer seja executada no Plano de hospedagem de consumo, quer em um [Plano de hospedagem do Serviço de Aplicativo](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) comum, pode processar invocações de função simultâneas em paralelo usando vários threads.  O número máximo de invocações de função simultâneas em cada instância do aplicativo de funções varia com base no tipo de gatilho que está sendo usado, bem como nos recursos usados por outras funções no aplicativo de funções.

## <a name="functions-runtime-versioning"></a>Controle de versão de tempo de execução de funções

Você pode configurar a versão do tempo de execução de Funções usando a configuração de aplicativo `FUNCTIONS_EXTENSION_VERSION`. Por exemplo, o valor "~2" indica que seu Aplicativo de Funções usará 2.x como sua versão principal. Aplicativos de funções são atualizados para cada nova versão secundária à medida que elas são lançadas. Para saber mais, incluindo como exibir a versão exata do aplicativo de funções, consulte [Como direcionar versões de tempo de execução do Azure Functions](set-runtime-version.md).

## <a name="repositories"></a>Repositórios
O código para o Azure Functions é software livre e é armazenado em repositórios do GitHub:

* [Tempo de execução do Azure Functions](https://github.com/Azure/azure-webjobs-sdk-script/)
* [Portal do Azure Functions](https://github.com/projectkudu/AzureFunctionsPortal)
* [Modelos do Azure Functions](https://github.com/Azure/azure-webjobs-sdk-templates/)
* [SDK WebJobs do Azure](https://github.com/Azure/azure-webjobs-sdk/)
* [Extensões do SDK WebJobs do Azure](https://github.com/Azure/azure-webjobs-sdk-extensions/)

## <a name="bindings"></a>Associações
Veja uma tabela de todas as associações com suporte.

[!INCLUDE [dynamic compute](../../includes/functions-bindings.md)]

Está tendo problemas com erros provenientes de associações? Examine a documentação de [códigos de erro de associação do Azure Functions](functions-bindings-error-pages.md).

## <a name="reporting-issues"></a>Problemas de relatórios
[!INCLUDE [Reporting Issues](../../includes/functions-reporting-issues.md)]

## <a name="next-steps"></a>Próximas etapas
Para saber mais, consulte os recursos a seguir:

* [Práticas recomendadas para o Azure Functions](functions-best-practices.md)
* [Referência do desenvolvedor de C# do Azure Functions](functions-reference-csharp.md)
* [Referência do desenvolvedor em F# do Azure Functions](functions-reference-fsharp.md)
* [Referência do desenvolvedor de NodeJS do Azure Functions](functions-reference-node.md)
* [Gatilhos e associações de Azure Functions](functions-triggers-bindings.md)
* [Azure Functions: The Journey](https://blogs.msdn.microsoft.com/appserviceteam/2016/04/27/azure-functions-the-journey/) (Azure Functions: a jornada) no blog da equipe do Serviço de Aplicativo do Azure. Um histórico de como o Azure Functions foi desenvolvido.

