---
title: Enviar telemetria ao Hub IoT do Azure (C) | Microsoft Docs
description: Neste início rápido, você executará dois aplicativos C de exemplo para enviar telemetria simulada a um Hub IoT e ler a telemetria do Hub IoT para processamento na nuvem.
author: wesmc7777
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.devlang: c
ms.topic: quickstart
ms.custom: mvc
ms.date: 08/27/2018
ms.author: wesmc
ms.openlocfilehash: 1bb21f6decc725c47f135c9842a2ba6d8989f693
ms.sourcegitcommit: 5a1d601f01444be7d9f405df18c57be0316a1c79
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2018
ms.locfileid: "51515158"
---
# <a name="quickstart-send-telemetry-from-a-device-to-an-iot-hub-and-read-the-telemetry-from-the-hub-with-a-back-end-application-c"></a>Início rápido: enviar telemetria de um dispositivo para um Hub IoT e ler a telemetria do hub com um aplicativo de back-end (C)

[!INCLUDE [iot-hub-quickstarts-1-selector](../../includes/iot-hub-quickstarts-1-selector.md)]

O Hub IoT é um serviço do Azure que permite a ingestão de grandes volumes de telemetria de seus dispositivos IoT pela nuvem para processamento ou armazenamento. Neste início rápido, você envia telemetria de um aplicativo de dispositivo simulado, por meio do Hub IoT, para um aplicativo de back-end para processamento.

O início rápido usa um aplicativo de exemplo do C do [SDK do dispositivo IoT do Azure para C](iot-hub-device-sdk-c-intro.md) para enviar telemetria a um hub IoT. Os SDKs do dispositivo IoT do Azure são escritos em [ANSI C (C99)](https://wikipedia.org/wiki/C99) para portabilidade e compatibilidade geral com a plataforma. Antes de executar o código de exemplo, você criará um hub IoT e registrará o dispositivo simulado nesse hub.

Este artigo foi escrito para Windows, mas você pode concluir este início rápido no Linux também.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

* Instale o [Visual Studio 2017](https://www.visualstudio.com/vs/) com a carga de trabalho ["Desenvolvimento de área de trabalho com C++"](https://www.visualstudio.com/vs/support/selecting-workloads-visual-studio-2017/) habilitada.
* Instale a versão mais recente do [Git](https://git-scm.com/download/).

## <a name="prepare-the-development-environment"></a>Preparar o ambiente de desenvolvimento

Neste início rápido, você usará o [SDK do dispositivo IoT do Azure para C](iot-hub-device-sdk-c-intro.md). 

É possível usar o SDK instalando os pacotes e bibliotecas para os seguintes ambientes:

* **Linux**: pacotes apt-get estão disponíveis para Ubuntu 16.04 e 18.04 que usam as seguintes arquiteturas de CPU: amd64, arm64, armhf e i386. Para saber mais, confira [Usar o apt-get para criar um projeto cliente para dispositivo C no Ubuntu](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/ubuntu_apt-get_sample_setup.md).

* **mbed**: para os desenvolvedores que criam aplicativos de dispositivos na plataforma mbed, publicamos uma biblioteca e amostras que ajudarão a começar em questão de minutos com o Hub IoT do Azure. Para saber mais, confira [Usar a biblioteca mbed](https://github.com/Azure/azure-iot-sdk-c/blob/master/iothub_client/readme.md#mbed).

* **Arduino**: se você estiver desenvolvendo no Arduino, poderá aproveitar a biblioteca IoT do Azure disponível no gerente de biblioteca do IDE do Arduino. Para saber mais, confira [A biblioteca do Hub IoT do Azure para Arduino](https://github.com/azure/azure-iot-arduino).

* **iOS**: o SDK de Dispositivo do Hub IoT está disponível como CocoaPods para desenvolvimento de dispositivo Mac e iOS. Para saber mais, confira os [Exemplos do iOS para IoT do Microsoft Azure](https://cocoapods.org/pods/AzureIoTHubClient).

No entanto, neste início rápido, você preparará um ambiente de desenvolvimento usado para clonar e criar o [SDK de C do IoT do Azure](https://github.com/Azure/azure-iot-sdk-c) no GitHub. O SDK no GitHub inclui o código de exemplo usado neste início rápido. 


1. Baixe a versão 3.11.4 do [sistema de build CMake](https://cmake.org/download/). Verifique o binário baixado usando o valor de hash criptográfico correspondente. O exemplo a seguir usou o Windows PowerShell para verificar o hash criptográfico da versão 3.11.4 da distribuição MSI x64:

    ```PowerShell
    PS C:\Downloads> $hash = get-filehash .\cmake-3.11.4-win64-x64.msi
    PS C:\Downloads> $hash.Hash -eq "56e3605b8e49cd446f3487da88fcc38cb9c3e9e99a20f5d4bd63e54b7a35f869"
    True
    ```
    
    Os valores de hash a seguir para a versão 3.11.4 foram listados no site do CMake no momento da redação deste material:

    ```
    6dab016a6b82082b8bcd0f4d1e53418d6372015dd983d29367b9153f1a376435  cmake-3.11.4-Linux-x86_64.tar.gz
    72b3b82b6d2c2f3a375c0d2799c01819df8669dc55694c8b8daaf6232e873725  cmake-3.11.4-win32-x86.msi
    56e3605b8e49cd446f3487da88fcc38cb9c3e9e99a20f5d4bd63e54b7a35f869  cmake-3.11.4-win64-x64.msi
    ```

    É importante que os pré-requisitos do Visual Studio (Visual Studio e a carga de trabalho de "Desenvolvimento para Desktop com C++") estejam instalados em seu computador, **antes** da instalação de `CMake`. Após a instalação dos pré-requisitos e verificação do download, instale o sistema de compilação CMake.

2. Abra um prompt de comando ou o shell Bash do Git. Execute o seguinte comando para clonar o repositório do GitHub [SDK de C do IoT do Azure](https://github.com/Azure/azure-iot-sdk-c):
    
    ```cmd/sh
    git clone https://github.com/Azure/azure-iot-sdk-c.git --recursive
    ```
    Atualmente, o tamanho desse repositório está em torno de 220 MB. Essa operação deve demorar alguns minutos.


3. Crie um subdiretório `cmake` no diretório raiz do repositório git e navegue até essa pasta. 

    ```cmd/sh
    cd azure-iot-sdk-c
    mkdir cmake
    cd cmake
    ```

4. Execute o comando a seguir, que cria uma versão do SDK específica para a plataforma cliente de desenvolvimento. Uma solução do Visual Studio para o dispositivo simulado será gerada no diretório `cmake`. 

    ```cmd
    cmake ..
    ```
    
    Se `cmake` não encontrar o compilador do C++, você poderá obter erros de build ao executar o comando acima. Se isso acontecer, tente executar esse comando no [prompt de comando do Visual Studio](https://docs.microsoft.com/dotnet/framework/tools/developer-command-prompt-for-vs). 

    Após o sucesso da compilação, as últimas linhas de saída serão semelhantes à seguinte saída:

    ```cmd/sh
    $ cmake ..
    -- Building for: Visual Studio 15 2017
    -- Selecting Windows SDK version 10.0.16299.0 to target Windows 10.0.17134.
    -- The C compiler identification is MSVC 19.12.25835.0
    -- The CXX compiler identification is MSVC 19.12.25835.0

    ...

    -- Configuring done
    -- Generating done
    -- Build files have been written to: E:/IoT Testing/azure-iot-sdk-c/cmake
    ```


## <a name="create-an-iot-hub"></a>Crie um hub IoT

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-device"></a>Registrar um dispositivo

Um dispositivo deve ser registrado no hub IoT antes de poder se conectar. Nesta seção, você usará o Azure Cloud Shell com a [extensão de IoT](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot?view=azure-cli-latest) para registrar um dispositivo simulado.

1. Execute os seguintes comandos no Azure Cloud Shell para adicionar a extensão da CLI do Hub IoT e criar a identidade do dispositivo. 

   **YourIoTHubName**: substitua o espaço reservado abaixo pelo nome escolhido para o Hub IoT.

   **MyCDdevice**: esse é o nome fornecido para o dispositivo registrado. Use myCDevice conforme mostrado. Se você escolher um nome diferente para seu dispositivo, você também precisará usar esse nome ao longo deste artigo e atualizar o nome de dispositivo nos aplicativos de exemplo antes de executá-los.

    ```azurecli-interactive
    az extension add --name azure-cli-iot-ext
    az iot hub device-identity create --hub-name YourIoTHubName --device-id MyCDevice
    ```

2. Execute os seguintes comandos no Azure Cloud Shell para obter a _cadeia de conexão de dispositivo_ referente ao dispositivo que você acabou de registrar:

   **YourIoTHubName**: substitua o espaço reservado abaixo pelo nome escolhido para o Hub IoT.

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name YourIoTHubName --device-id MyCDevice --output table
    ```

    Tome nota da cadeia de conexão do dispositivo, que se parece com:

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyNodeDevice;SharedAccessKey={YourSharedAccessKey}`

    Você usará esse valor posteriormente no início rápido.

## <a name="send-simulated-telemetry"></a>Enviar telemetria simulada

O aplicativo de dispositivo simulado se conecta a um ponto de extremidade específico do dispositivo em seu hub IoT e envia uma cadeia de caracteres como telemetria simulada.

1. Em um editor de texto, abra o arquivo de origem iothub_convenience_sample.c e revise o código de exemplo para enviar a telemetria. O arquivo está localizado no seguinte local:

    ```
    \azure-iot-sdk-c\iothub_client\samples\iothub_convenience_sample\iothub_convenience_sample.c
    ```

2. Localize a declaração da constante `connectionString`:

    ```C
    /* Paste in your device connection string  */
    static const char* connectionString = "[device connection string]";
    ```
    Substitua o valor da constante `connectionString` pela cadeia de conexão do dispositivo que você anotou anteriormente. Em seguida, salve suas alterações em **iothub_convenience_sample.c**.

3. Em uma janela de terminal local, navegue até o diretório do projeto *iothub_convenience_sample* no diretório do CMake que você criou no SDK de C do IoT do Azure.

    ```
    cd /azure-iot-sdk-c/cmake/iothub_client/samples/iothub_convenience_sample
    ```

4. Execute CMake na janela de terminal local para compilar o exemplo com o valor `connectionString` atualizado:

    ```cmd/sh
    cmake --build . --target iothub_convenience_sample --config Debug
    ```

5. Na janela de terminal local, execute o seguinte comando para executar o aplicativo de dispositivo simulado:

    ```cmd/sh
    Debug\iothub_convenience_sample.exe
    ```

    A captura de tela a seguir mostra o resultado à medida que o aplicativo de dispositivo simulado envia telemetria para o hub IoT:

    ![Executar o dispositivo simulado](media/quickstart-send-telemetry-c/simulated-device-app.png)

## <a name="read-the-telemetry-from-your-hub"></a>Ler a telemetria do hub


Nesta seção, você usará o Azure Cloud Shell com a [extensão de IoT](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot?view=azure-cli-latest) para monitorar as mensagens de dispositivo enviadas pelo dispositivo simulado.

1. Usando o Azure Cloud Shell, execute o seguinte comando para se conectar e ler mensagens do hub IoT:

   **YourIoTHubName**: substitua o espaço reservado abaixo pelo nome escolhido para o Hub IoT.

    ```azurecli-interactive
    az iot hub monitor-events --hub-name YourIoTHubName --output table
    ```

    ![Leia as mensagens do dispositivo usando a CLI do Azure](media/quickstart-send-telemetry-c/read-device-to-cloud-messages-app.png)

    

## <a name="clean-up-resources"></a>Limpar recursos

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources.md)]

## <a name="next-steps"></a>Próximas etapas

Neste início rápido, você configurou um hub IoT, registrou um dispositivo, enviou telemetria simulada para o hub usando um aplicativo C e leu a telemetria do hub usando o Azure Cloud Shell.

Para saber mais sobre o desenvolvimento com o SDK de C do Hub IoT do Azure, acesse o guia de instruções a seguir:

> [!div class="nextstepaction"]
> [Desenvolver usando o SDK de C do Hub IoT do Azure](iot-hub-devguide-develop-for-constrained-devices.md)