---
title: Criar informações de vídeo de vídeos existentes
titlesuffix: Azure Media Services
description: Este tópico mostra como criar e publicar insights de vídeo com base em arquivos de vídeo existentes.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.topic: article
ms.date: 11/19/2018
ms.author: juliako
ms.openlocfilehash: 7ba3fab514729b7b5645254fa9d34dd42b6718d5
ms.sourcegitcommit: beb4fa5b36e1529408829603f3844e433bea46fe
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/22/2018
ms.locfileid: "52291658"
---
# <a name="tutorial-create-highlights-from-existing-videos"></a>Tutorial: Criar destaques com base em vídeos existentes

Este tópico mostra como criar e publicar insights de vídeo com base em algum outro vídeo.

1. Navegue até o site do [Video Indexer](https://www.videoindexer.ai/) e entre.
2. Encontre um vídeo do qual você deseja criar seus insights de vídeo.
3. Pressione **Reproduzir**.

    A página mostra os insights resumidos do vídeo. 

    ![Insights](./media/video-indexer-create-new/video-indexer-summarized-insights.png)

3. Pressione o botão **Editar**.

    Esta página mostra a análise completa de um vídeo. A análise é dividida em blocos. Os blocos estão aqui para tornar mais fácil percorrer os dados. Por exemplo, o bloco pode ser dividido com base em quando os locutores mudam ou quando há uma longa pausa. É possível criar sua própria playlist que contém apenas as linhas desejadas. Para mostrar apenas partes específicas do vídeo de origem, é possível filtrar por topics/keywords (tópicos/palavras-chave), sentiments (sentimentos), people (pessoas), speakers (locutores). É possível optar por exibir apenas a transcrição ou o OCR do vídeo.    

    ![Insights](./media/video-indexer-create-new/video-indexer-create-new-playlist.png)

4. Crie sua playlist.

    Para adicionar ou remover linhas à/da sua playlist, pressione **+**/**-**.

5. Visualize sua playlist.

    Depois de terminar de criar a playlist, pressione **Visualizar**.
6. Publique a playlist.

    Depois de visualizar a playlist, será possível publicá-la.

    Depois de publicar a playlist, ela será adicionada à lista dos seus insights de vídeo.


## <a name="next-steps"></a>Próximas etapas 

Depois de criar a nova playlist, será possível continuar processando-a, conforme descrito em um destes tópicos: 

- [Processar conteúdo com a API REST do Video Indexer](video-indexer-use-apis.md)
- [Inserir widgets visuais em seu aplicativo](video-indexer-embed-widgets.md)

## <a name="see-also"></a>Consulte também

[Visão geral do indexador vídeo](video-indexer-overview.md) 
