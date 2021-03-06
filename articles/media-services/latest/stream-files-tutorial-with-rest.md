---
title: Carregar, codificar e transmitir usando serviços de Mídia do Azure - REST | Microsoft Docs
description: Siga as etapas deste tutorial para carregar um arquivo, codificar o vídeo e transmitir por streaming o conteúdo com Serviços de Mídia do Azure usando REST.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: tutorial
ms.custom: mvc
ms.date: 11/11/2018
ms.author: juliako
ms.openlocfilehash: 67a0b6ced771519bd97934f8914ba420ee3119ce
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/13/2018
ms.locfileid: "51615765"
---
# <a name="tutorial-upload-encode-and-stream-videos-with-rest"></a>Tutorial: Carregar, codificar e transmitir por streaming vídeos usando REST

Os Serviços de Mídia do Azure permitem codificar os arquivos de mídia em formatos que podem ser reproduzidos em uma ampla variedade de navegadores e dispositivos. Por exemplo, talvez você queira transmitir por streaming o conteúdo nos formatos MPEG-DASH ou HLS da Apple. Antes do streaming, será necessário codificar o arquivo de mídia digital de alta qualidade. Para obter diretrizes de codificação, consulte [Conceito de codificação](encoding-concept.md).

Este tutorial mostra como carregar, codificar e transmitir arquivos de vídeo com os Serviços de Mídia do Azure usando REST. 

![Reproduzir o vídeo](./media/stream-files-tutorial-with-api/final-video.png)

Este tutorial mostra como:    

> [!div class="checklist"]
> * Criar uma conta de Serviços de Mídia
> * Instalar a API de Serviços de Mídia
> * Baixar arquivos Postman
> * Configurar Postman
> * Enviar solicitações usando Postman
> * Testar a URL de streaming
> * Limpar recursos

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Pré-requisitos

- Instalar e usar a CLI localmente, este artigo requer a versão 2.0 ou posterior da CLI do Azure. Execute `az --version` descobrir a versão que você tem. Se você precisar instalar ou atualizar, confira [Instalar a CLI do Azure](/cli/azure/install-azure-cli). 

    Atualmente, nem todos os comandos da [CLI V3 dos Serviços de Mídia](https://aka.ms/ams-v3-cli-ref) funcionam no Azure Cloud Shell. É recomendável usar a CLI localmente.

- [Crie uma conta de Serviços de Mídia](create-account-cli-how-to.md).

    Lembre-se dos valores que você usou para o nome do grupo de recursos e o nome da conta de Serviços de Mídia

- Instale o cliente REST do [Postman](https://www.getpostman.com/) para executar as APIs de REST mostradas em alguns dos tutoriais do REST AMS. 

    Estamos usando o **Postman** mas qualquer ferramenta REST seria adequada. Outras alternativas são: **Visual Studio Code** com o plug-in do REST ou **Telerik Fiddler**. 

## <a name="download-postman-files"></a>Baixar arquivos Postman

Clone um repositório GitHub que contenha os arquivos do ambiente e coleção Postman.

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-rest-postman.git
 ```

[!INCLUDE [media-services-v3-cli-access-api-include](../../../includes/media-services-v3-cli-access-api-include.md)]

## <a name="configure-postman"></a>Configurar Postman

Esta seção configura o Postman.

### <a name="configure-the-environment"></a>Configure o ambiente 

1. Abra o aplicativo **Postman**.
2. À direita da tela, selecione a opção **Gerenciar ambiente**.

    ![Gerenciar ambiente](./media/develop-with-postman/postman-import-env.png)
4. Na caixa de diálogo **Gerenciar ambiente**, clique em **Importar**.
2. Navegue até o arquivo `Azure Media Service v3 Environment.postman_environment.json` que foi baixado quando você clonou `https://github.com/Azure-Samples/media-services-v3-rest-postman.git`.
6. O ambiente do **Ambiente do Serviço de Mídia do Azure v3** é adicionado.

    > [!Note]
    > Atualize as variáveis de acesso com valores obtidos a partir da seção **Acessar a API de Serviços de Mídia do Azure** acima.

7. Clique duas vezes no arquivo selecionado e digite os valores que você obteve seguindo as etapas de [acesso a API](#access-the-media-services-api).
8. Feche a caixa de diálogo.
9. Selecione o ambiente, **Ambiente do Serviço de Mídia do Azure v3**, no menu suspenso.

    ![Escolher ambiente](./media/develop-with-postman/choose-env.png)
   
### <a name="configure-the-collection"></a>Configurar a coleção

1. Clique em **Importar** para importar o arquivo de coleção.
1. Navegue até o arquivo `Media Services v3.postman_collection.json` que foi baixado quando você clonou `https://github.com/Azure-Samples/media-services-v3-rest-postman.git`
3. Escolha o arquivo **Serviços de Mídia v3.postman_collection.json**.

    ![Importar um arquivo](./media/develop-with-postman/postman-import-collection.png)

## <a name="send-requests-using-postman"></a>Enviar solicitações usando Postman

Nesta seção, enviamos solicitações relevantes para codificar e criar URLs para que seja possível transmitir o arquivo. Especificamente, as solicitações a seguir são enviadas:

1. Obter token do Microsoft Azure AD para autenticação de entidade de serviço
2. Criar um ativo de saída
3. Criar uma transformação
4. Criar um trabalho 
5. Criar um localizador de streaming
6. Listar caminhos do localizador de streaming

> [!Note]
>  Este tutorial pressupõe que você está criando todos os recursos com nomes exclusivos.  

### <a name="get-azure-ad-token"></a>Obter token do Microsoft Azure AD 

1. Na janela esquerda do Postman, selecione "Etapa 1: Obter token de autenticação do AAD".
2. Em seguida, selecione "Obter token do Microsoft Azure AD para autenticação de entidade de serviço".
3. Pressione **Enviar**.

    A operação **POST** a seguir é enviada.

    ```
    https://login.microsoftonline.com/:tenantId/oauth2/token
    ```

4. A resposta retorna com o token e define a variável de ambiente "AccessToken" como o valor de token. Para ver o código que define "AccessToken", clique na guia **Testes**. 

    ![Obter token do AAD](./media/develop-with-postman/postman-get-aad-auth-token.png)

### <a name="create-an-output-asset"></a>Criar um ativo de saída

O [Ativo](https://docs.microsoft.com/rest/api/media/assets) de saída armazena o resultado do trabalho de codificação. 

1. Na janela esquerda do Postman, selecione "Ativos".
2. Em seguida, selecione "Criar ou atualizar um ativo".
3. Pressione **Enviar**.

    * A operação **PUT** a seguir é enviada:

        ```
        https://management.azure.com/subscriptions/:subscriptionId/resourceGroups/:resourceGroupName/providers/Microsoft.Media/mediaServices/:accountName/assets/:assetName?api-version={{api-version}}
        ```
    * A operação tem o seguinte corpo:

        ```json
        {
        "properties": {
            "description": "My Asset",
            "alternateId" : "some GUID"
         }
        }
        ```

### <a name="create-a-transform"></a>Criar uma transformação

Ao codificar ou processar o conteúdo nos Serviços de Mídia do Microsoft Azure, é um padrão comum para configurar as configurações de codificação como uma receita. Em seguida, você pode enviar um **Trabalho** para aplicar essa fórmula a um vídeo. Ao enviar novos trabalhos para cada novo vídeo, você está aplicando essa fórmula em todos os vídeos na biblioteca. Uma receita nos Serviços de Mídia do Microsoft Azure é chamada de **Transformação**. Para obter mais informações, consulte [Transformações e Trabalhos](transform-concept.md). O exemplo descrito neste tutorial define uma receita que codifica o vídeo para transmitir para uma variedade de dispositivos de iOS e Android. 

Ao criar um novo exemplo de [Transformação](https://docs.microsoft.com/rest/api/media/transforms), você precisa especificar o que deseja produzir como uma saída. O parâmetro necessário é um objeto **TransformOutput**. Cada **TransformOutput** contém um **Predefinição**. **Predefinição** descreve as instruções passo a passo das operações de processamento de áudio e/ou vídeo que serão usadas para gerar o **TransformOutput** desejado. O exemplo descrito neste artigo usa uma predefinição chamada **AdaptiveStreaming**. A predefinição codifica o vídeo de entrada em uma escada de taxa de bits gerada automaticamente (pares de resolução de taxa de bits) com base na taxa de bits e resolução de entrada e produz arquivos ISO MP4 com vídeo H. 264 e correspondente a cada par de resolução de taxa de bits de áudio AAC. Para obter informações sobre essa predefinição, consulte [escada de bitrate de geração automática](autogen-bitrate-ladder.md).

Você pode usar um EncoderNamedPreset interno ou usar as predefinições personalizadas. 

> [!Note]
> Ao criar uma [Transformação](https://docs.microsoft.com/rest/api/media/transforms), será necessário primeiro verificar se já existe alguma usando o método **Get**. Este tutorial pressupõe que você está criando a transformação com um nome exclusivo.

1. Na janela esquerda do Postman, selecione "Codificação e Análise".
2. Em seguida, selecione "Criar Transformação".
3. Pressione **Enviar**.

    * A operação **PUT** a seguir é enviada.

        ```
        https://management.azure.com/subscriptions/:subscriptionId/resourceGroups/:resourceGroupName/providers/Microsoft.Media/mediaServices/:accountName/transforms/:transformName?api-version={{api-version}}
        ```
    * A operação tem o seguinte corpo:

        ```json
        {
            "properties": {
                "description": "Basic Transform using an Adaptive Streaming encoding preset from the libray of built-in Standard Encoder presets",
                "outputs": [
                    {
                    "onError": "StopProcessingJob",
                "relativePriority": "Normal",
                    "preset": {
                        "@odata.type": "#Microsoft.Media.BuiltInStandardEncoderPreset",
                        "presetName": "AdaptiveStreaming"
                    }
                    }
                ]
            }
        }
        ```

### <a name="create-a-job"></a>Criar um trabalho

Um [Trabalho](https://docs.microsoft.com/rest/api/media/jobs) é a solicitação real aos Serviços de Mídia para aplicar a **Transformação** criada em um determinado vídeo de entrada ou conteúdo de áudio. O **Trabalho** especifica informações como o local da entrada vídeo e o local da saída.

Neste exemplo, a entrada do trabalho é baseada em uma URL HTTPS ("https://nimbuscdn-nimbuspm.streaming.mediaservices.windows.net/2b533311-b215-4409-80af-529c3e853622/").

1. Na janela esquerda do Postman, selecione "Codificação e Análise".
2. Em seguida, selecione "Criar ou Atualizar Trabalho".
3. Pressione **Enviar**.

    * A operação **PUT** a seguir é enviada.

        ```
        https://management.azure.com/subscriptions/:subscriptionId/resourceGroups/:resourceGroupName/providers/Microsoft.Media/mediaServices/:accountName/transforms/:transformName/jobs/:jobName?api-version={{api-version}}
        ```
    * A operação tem o seguinte corpo:

        ```json
        {
        "properties": {
            "input": {
            "@odata.type": "#Microsoft.Media.JobInputHttp",
            "baseUri": "https://nimbuscdn-nimbuspm.streaming.mediaservices.windows.net/2b533311-b215-4409-80af-529c3e853622/",
            "files": [
                    "Ignite-short.mp4"
                ]
            },
            "outputs": [
            {
                "@odata.type": "#Microsoft.Media.JobOutputAsset",
                "assetName": "testAsset1"
            }
            ]
        }
        }
        ```

Este trabalho levará algum tempo para concluir e quando você desejar ser notificado. Para ver o progresso do trabalho, é recomendável usar a Grade de Eventos. Ela é projetada para alta disponibilidade, desempenho consistente e escala dinâmica. Com a Grade de Eventos, seus aplicativos podem escutar e reagir a eventos de praticamente todos os serviços do Azure, bem como de origens personalizadas. A manipulação de eventos simples e reativa baseada em HTTP ajuda você a criar soluções eficientes por meio da filtragem e do roteamento de eventos.  Consulte [Rotear eventos para um ponto de extremidade personalizado de web](job-state-events-cli-how-to.md).

O **Trabalho** normalmente passa pelos seguintes estados: **Agendado**, **Em fila**, **Processamento**, **Concluído** (o estado final). Se o trabalho encontrou um erro, você receberá o estado do **Erro**. Se o trabalho está no processo de ser cancelado, você obterá **Cancelando** e **Cancelado** quando estiver pronto.

### <a name="create-a-streaming-locator"></a>Criar um localizador de streaming

Após a conclusão da tarefa de codificação, a próxima etapa é disponibilizar o vídeo no Ativo de saída aos clientes para reprodução. Você pode fazer isso em duas etapas: primeiro, crie um [StreamingLocator](https://docs.microsoft.com/rest/api/media/streaminglocators)e em seguida, crie as URLs de streaming que os clientes podem usar. 

O processo de criação de um **StreamingLocator** é chamado de publicação. Por padrão, o **StreamingLocator** é válido imediatamente após você fazer as chamadas de API e dura até ser excluído, a menos que você configure os horários de início e término opcionais. 

Ao criar um [StreamingLocator](https://docs.microsoft.com/rest/api/media/streaminglocators), você precisará especificar os detalhes desejados **StreamingPolicyName**. Neste exemplo, você transmitirá conteúdo em criptografado (ou não criptografado), portanto, a política de streaming predefinida clara **PredefinedStreamingPolicy.ClearStreamingOnly** é utilizada.

> [!IMPORTANT]
> Ao usar a [StreamingPolicy](https://docs.microsoft.com/rest/api/media/streamingpolicies) personalizada, você deve criar um conjunto limitado de tais políticas para sua conta de serviço de mídia e reutilizá-los para o seu StreamingLocators, sempre que a mesmo opção de criptografia e protocolos sejam necessários. 

Sua conta de Serviço de Mídia tem uma cota para o número de entradas de StreamingPolicy. Você não deve criar um novo StreamingPolicy para cada StreamingLocator.

1. Na janela esquerda do Postman, selecione "Políticas de Streaming".
2. Em seguida, selecione "Criar um Localizador de Streaming".
3. Pressione **Enviar**.

    * A operação **PUT** a seguir é enviada.

        ```
        https://management.azure.com/subscriptions/:subscriptionId/resourceGroups/:resourceGroupName/providers/Microsoft.Media/mediaServices/:accountName/streamingPolicies/:streamingPolicyName?api-version={{api-version}}
        ```
    * A operação tem o seguinte corpo:

        ```json
        {
            "properties":{
            "assetName": "{{assetName}}",
            "streamingPolicyName": "{{streamingPolicyName}}"
            }
        }
        ```

### <a name="list-paths-and-build-streaming-urls"></a>Listar caminhos e criar URLs de streaming

#### <a name="list-paths"></a>Listar caminhos

Agora que o [StreamingLocator](https://docs.microsoft.com/rest/api/media/streaminglocators) foi criado, é possível obter as URLs de streaming

1. Na janela esquerda do Postman, selecione "Políticas de Streaming".
2. Em seguida, selecione "Listar Caminhos".
3. Pressione **Enviar**.

    * A operação **POST** a seguir é enviada.

        ```
        https://management.azure.com/subscriptions/:subscriptionId/resourceGroups/:resourceGroupName/providers/Microsoft.Media/mediaServices/:accountName/streamingLocators/:streamingLocatorName/listPaths?api-version={{api-version}}
        ```
        
    * A operação não tem corpo:
        
4. Observe um dos caminhos que você pretende usar para streaming será utilizado na próxima seção. Nesse caso, os caminhos a seguir foram retornados:
    
    ```
    "streamingPaths": [
        {
            "streamingProtocol": "Hls",
            "encryptionScheme": "NoEncryption",
            "paths": [
                "/cdb80234-1d94-42a9-b056-0eefa78e5c63/Ignite-short.ism/manifest(format=m3u8-aapl)"
            ]
        },
        {
            "streamingProtocol": "Dash",
            "encryptionScheme": "NoEncryption",
            "paths": [
                "/cdb80234-1d94-42a9-b056-0eefa78e5c63/Ignite-short.ism/manifest(format=mpd-time-csf)"
            ]
        },
        {
            "streamingProtocol": "SmoothStreaming",
            "encryptionScheme": "NoEncryption",
            "paths": [
                "/cdb80234-1d94-42a9-b056-0eefa78e5c63/Ignite-short.ism/manifest"
            ]
        }
    ]
    ```

#### <a name="build-the-streaming-urls"></a>Compilar as URLs de streaming

Nesta seção, vamos compilar uma URL de streaming HLS. URLs consistem nos valores a seguir:

1. O protocolo pelo qual os dados são enviados. Neste caso, "https".

    > [!NOTE]
    > Se um player estiver hospedado em um site https, atualize a URL para "https".

2. Nome do host do StreamingEndpoint. Nesse caso, o nome é "amsaccount-usw22.streaming.media.azure.net".

    Para obter o nome do host, você pode usar a seguinte operação:
    
    ```
    https://management.azure.com/subscriptions/00000000-0000-0000-0000-0000000000000/resourceGroups/amsResourceGroup/providers/Microsoft.Media/mediaservices/amsaccount/streamingEndpoints/default?api-version={{api-version}}
    ```
    
3. Um caminho obtido na seção anterior (Listar caminhos).  

Como resultado, a URL HLS a seguir foi compilada

```
https://amsaccount-usw22.streaming.media.azure.net/cdb80234-1d94-42a9-b056-0eefa78e5c63/Ignite-short.ism/manifest(format=m3u8-aapl)
```

## <a name="test-the-streaming-url"></a>Testar a URL de streaming


> [!NOTE]
> Verifique se o ponto de extremidade de streaming do qual você deseja transmitir está em execução.

Para testar o streaming, este artigo usa o Player de Mídia do Azure. 

1. Abra um navegador da Web e navegue até [https://aka.ms/azuremediaplayer/](https://aka.ms/azuremediaplayer/).
2. Na caixa **URL:**, cole a URL que você compilou. 
3. Pressione **Atualizar Player**.

O Player de Mídia do Azure pode ser usado para testes, mas não deve ser usado em um ambiente de produção. 

## <a name="clean-up-resources-in-your-media-services-account"></a>Limpar os recursos em sua conta de Serviços de Mídia

Em geral, você deve limpar tudo, exceto os objetos que planeja reutilizar (normalmente, você reutilizará transformações e persistirá StreamingLocators, etc.). Se desejar para sua conta para ser limpa depois de testar, você deve excluir os recursos que não planeja reutilizar.  

Para excluir um recurso, selecione a operação "Excluir ..." em qualquer recurso que você queira excluir.

## <a name="clean-up-resources"></a>Limpar recursos

Se você não precisar mais de qualquer um dos recursos em seu grupo de recursos, incluindo as contas dos Serviços de Mídia e de armazenamento que você criou neste tutorial, exclua o grupo de recursos criados anteriormente.  

Execute este comando da CLI:

```azurecli
az group delete --name amsResourceGroup
```

## <a name="next-steps"></a>Próximas etapas

Agora que você sabe como carregar, codificar e transmitir seu vídeo, consulte o seguinte artigo: 

> [!div class="nextstepaction"]
> [Analisar vídeos](analyze-videos-tutorial-with-api.md)
