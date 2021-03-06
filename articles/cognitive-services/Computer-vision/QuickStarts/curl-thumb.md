---
title: 'Início Rápido: gerar uma miniatura – REST, cURL – Pesquisa Visual Computacional'
titleSuffix: Azure Cognitive Services
description: Neste início rápido, você gerará uma miniatura de uma imagem usando a API da Pesquisa Visual Computacional com cURL.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: quickstart
ms.date: 08/28/2018
ms.author: pafarley
ms.openlocfilehash: 51c6a8e5693602cdc839de80f268891c247c63a9
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/16/2018
ms.locfileid: "49344057"
---
# <a name="quickstart-generate-a-thumbnail-using-the-rest-api-and-curl-in-computer-vision"></a>Início Rápido: gerar uma miniatura usando a API REST e cURL na Pesquisa Visual Computacional

Neste início rápido, você gerará uma miniatura de uma imagem usando a API REST da Pesquisa Visual Computacional. Com o método [Get Thumbnail](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fb), é possível gerar uma miniatura de uma imagem. Você especifica a altura e largura, que podem ser diferentes da proporção da imagem de entrada. A Pesquisa Visual Computacional usa o corte inteligente para identificar a região de interesse de modo inteligência e gerar as coordenadas de corte com base nessa região.

Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/ai/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=cognitive-services) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

Para usar a Pesquisa Visual Computacional, você precisa de uma chave de assinatura. Veja [Obter chaves de assinatura](../Vision-API-How-to-Topics/HowToSubscribe.md).

## <a name="get-thumbnail-request"></a>Solicitação Obter Miniatura

Com o [método Obter Miniatura](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fb), você pode gerar uma miniatura de uma imagem. Você especifica a altura e largura, que podem ser diferentes da proporção da imagem de entrada. A Pesquisa Visual Computacional usa o corte inteligente para identificar a região de interesse de modo inteligência e gerar as coordenadas de corte com base nessa região.

Para executar a amostra, siga estas etapas:

1. Copie o seguinte código para um editor.
1. Substitua `<Subscription Key>` pela sua chave de assinatura válida.
1. Substitua `<File>` pelo caminho e nome de arquivo para salvar a miniatura.
1. Altere a URL da Solicitação (`https://westcentralus.api.cognitive.microsoft.com/vision/v2.0`) para usar o local do qual você obteve suas chaves de assinatura, se necessário.
1. Opcionalmente, altere a imagem (`{\"url\":\"...`) a analisar.
1. Abra uma janela Comando em um computador com a cURL instalada.
1. Cole o código na janela e execute o comando.

>[!NOTE]
>Você deve usar o mesmo local em sua chamada REST que é usado para obter as chaves de assinatura. Por exemplo, se você adquiriu suas chaves de assinatura de westus, substitua "westcentralus" na URL abaixo por "westus".

## <a name="prerequisites"></a>Pré-requisitos

- É necessário ter [cURL](https://curl.haxx.se/windows).
- Você precisa ter uma chave de assinatura para a Pesquisa Visual Computacional. Para obter uma chave de assinatura, confira [Obter chaves de assinatura](../Vision-API-How-to-Topics/HowToSubscribe.md).

## <a name="create-and-run-the-sample-command"></a>Criar e executar o comando de exemplo

Para criar e executar o exemplo, siga estas etapas:

1. Copie o seguinte comando em um editor de texto.
1. Faça as alterações a seguir no comando quando necessário:
    1. Substitua o valor de `<subscriptionKey>` pela sua chave de assinatura.
    1. Substitua o valor de `<thumbnailFile>` pelo caminho e o nome do arquivo no qual salvar a miniatura.
    1. Substitua a URL de solicitação (`https://westcentralus.api.cognitive.microsoft.com/vision/v2.0/generateThumbnail`) pela URL do ponto de extremidade para o método [Get Thumbnail](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fb) da região do Azure em que você adquiriu suas chaves de assinatura, se necessário.
    1. Opcionalmente, altere a URL da imagem no corpo da solicitação (`https://upload.wikimedia.org/wikipedia/commons/thumb/5/56/Shorkie_Poo_Puppy.jpg/1280px-Shorkie_Poo_Puppy.jpg\`) para a URL de uma imagem diferente da qual você deseja gerar uma miniatura.
1. Abra una janela de prompt de comando.
1. Cole o comando do editor de texto na janela do prompt de comando e, em seguida, execute-o.

```console
curl -H "Ocp-Apim-Subscription-Key: <subscriptionKey>" -o <thumbnailFile> -H "Content-Type: application/json" "https://westcentralus.api.cognitive.microsoft.com/vision/v2.0/generateThumbnail?width=100&height=100&smartCropping=true" -d "{\"url\":\"https://upload.wikimedia.org/wikipedia/commons/thumb/5/56/Shorkie_Poo_Puppy.jpg/1280px-Shorkie_Poo_Puppy.jpg\"}"
```

## <a name="examine-the-response"></a>Examinar a resposta

Uma resposta bem-sucedida grava a imagem em miniatura para o arquivo especificado em `<thumbnailFile>`. Se a solicitação falhar, a resposta conterá um código de erro e uma mensagem para ajudar a determinar o que deu errado.

## <a name="clean-up-resources"></a>Limpar recursos

Quando não for mais necessário, feche a janela do prompt de comando e o editor de texto.

## <a name="next-steps"></a>Próximas etapas

Explore a API da Pesquisa Visual Computacional usada para analisar uma imagem, detectar celebridades e pontos de referência, criar uma miniatura e extrair textos manuscritos e impressos. Para testar rapidamente a API da Pesquisa Visual Computacional, experimente o [Abrir o console de teste de API](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console).

> [!div class="nextstepaction"]
> [Explorar a API da Pesquisa Visual Computacional](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)
