---
title: Sobre os dispositivos de fala do SDK
titleSuffix: Azure Cognitive Services
description: Obter introdução ao SDK de Dispositivos de Fala.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 05/07/2018
ms.author: erhopf
ms.openlocfilehash: eac3542059f1bc5d32a91ef871e5185fad1d2798
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/19/2018
ms.locfileid: "49464090"
---
# <a name="about-the-speech-devices-sdk-preview"></a>Sobre o SDK de Dispositivos de Fala (Versão prévia)

O [serviço de fala](overview.md) funciona com uma ampla variedade de dispositivos e fontes de áudio. Agora, você pode levar os aplicativos de fala para o próximo nível com hardware e software correspondentes. O SDK de Dispositivos de Fala é uma biblioteca pré-ajustada emparelhada com kits de desenvolvimento de matriz de microfone criados para um fim específico. 

O SDK de dispositivos de fala pode ajudá-lo:
* Teste rapidamente novos cenários de voz.
* Mais fácil integre o serviço de fala baseado em nuvem em seu dispositivo.
* Crie uma experiência de usuário excepcional para seus clientes. 

O SDK de dispositivos de fala consome os [Speech SDK](speech-sdk.md). Usa o SDK de fala para enviar áudio que é processado pelo nosso algoritmo de processamento de áudio avançada de matriz de microfone do dispositivo para o [Serviço de fala](overview.md). Usa o áudio multicanal para fornecer um [reconhecimento de fala em campo distante](speech-to-text.md) mais preciso através da supressão de ruído, cancelamento de eco, beamforming e remoção da reverberação.

Você também pode usar o SDK de dispositivos de fala para criar o ambientes dispositivos que têm sua própria [palavra de ativação personalizada](speech-devices-sdk-create-kws.md)— portanto, a indicação que inicia uma interação do usuário é exclusiva para sua marca. 

O SDK de dispositivos de fala facilita a uma variedade de cenários habilitado para voz, como sistemas de pedido drive-thru, assistentes na loja ou em casa e alto-falantes inteligentes. Você pode responder aos usuários com texto, falar com eles com uma [voz padrão](how-to-customize-voice-font.md) ou personalizada, apresentar resultados de pesquisa, [traduzir](speech-translation.md) para outros idiomas e muito mais. Estamos ansiosos para ver suas criações.

## <a name="development-kit-providers"></a>Provedores de kit de desenvolvimento

Atualmente, esses designs de referência completa e de ponta a ponta do sistema estão disponíveis: 

|||
|-|-|
|[![Logotipo da ROOBO](media/speech-devices-sdk/roobo-logo.png)](http://ddk.roobo.com/)|ROOBO fornece inteligência completa soluções do sistema (AI) para os aparelhos domésticos elétricos, automóveis, robôs, brinquedos e outros setores. Os designs de referência da ROOBO reduzem bastante o tempo de desenvolvimento por meio da integração com o serviço de Fala da Microsoft. [Visite ROOBO](http://ddk.roobo.com/).|

## <a name="next-steps"></a>Próximas etapas

Para começar, obtenha uma [Conta gratuita do Azure](https://azure.microsoft.com/free/ai/) e inscreva-se para o SDK de Dispositivos de Fala.

> [!div class="nextstepaction"]
> [Inscreva-se para o SDK de Dispositivos de Fala](get-speech-devices-sdk.md)

