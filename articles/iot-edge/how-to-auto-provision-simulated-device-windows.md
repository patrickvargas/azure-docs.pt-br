---
title: Dispositivo com Azure IoT Edge de auto-provisionamento com DPS - Windows | Microsoft Docs
description: Usar um dispositivo simulado em seu computador Windows para testar o provisionamento automático de dispositivos para o Azure IoT Edge com o serviço de provisionamento de dispositivos
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 08/06/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 9be790d9b512dc9338cf183240430ad0ada3bef4
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/12/2018
ms.locfileid: "51565098"
---
# <a name="create-and-provision-a-simulated-tpm-edge-device-on-windows"></a>Crie e provisione um dispositivo de borda do TPM simulado no Windows

Dispositivos do Azure IoT Edge podem ser autoprovisionados usando o [Serviço de provisionamento de dispositivo](../iot-dps/index.yml) assim como os dispositivos que não são habilitados de borda. Se você não estiver familiarizado com o processo de provisionamento automático, analise os [Conceitos de provisionamento automático](../iot-dps/concepts-auto-provisioning.md) antes de continuar. 

Este artigo mostra como testar o provisionamento automático em um dispositivo de borda simulado com as seguintes etapas: 

* Criar uma nova instância para o Serviço de Provisionamento de Dispositivos (DPS) no Hub IoT.
* Crie um dispositivo simulado em seu computador Windows com um simulado Trusted Platform Module (TPM) para segurança de hardware.
* Crie um registro individual para o dispositivo.
* Instale o tempo de execução do IoT Edge e conecte o dispositivo ao Hub IoT.

## <a name="prerequisites"></a>Pré-requisitos

* Computador de desenvolvimento do Windows. Este artigo usa Windows 10. 
* Um Hub IoT ativo. 

## <a name="set-up-the-iot-hub-device-provisioning-service"></a>Configurar o Serviço de Provisionamento de Dispositivos no Hub IoT

Criar uma nova instância do Serviço de Provisionamento de Dispositivos no Hub IoT no Microsoft Azure e vincular ao seu hub IoT. Você pode seguir as instruções em [Configurar o DPS do Hub IoT](../iot-dps/quick-setup-auto-provision.md).

Depois de executar o Serviço de Provisionamento de Dispositivo, copie o valor do **Escopo de ID** da página de visão geral. Você usa esse valor ao configurar o tempo de execução do Azure IoT Edge. 

## <a name="simulate-a-tpm-device"></a>Simular um dispositivo TPM

Crie um dispositivo TPM simulado em sua máquina de desenvolvimento do Windows. Recupere a **ID de registro** e **Chave de endosso** para o seu dispositivo e use-os para criar uma entrada de inscrição individual no DPS. 

Ao criar uma inscrição no DPS, tem a oportunidade de declarar um **Estado inicial do dispositivo duplo**. No dispositivo gêmeo, você pode definir tags para agrupar dispositivos por qualquer métrica que precisar em sua solução, como região, ambiente, local ou tipo de dispositivo. Essas marcas são usadas para criar [implantações automáticas](how-to-deploy-monitor.md). 

Escolha o idioma SDK que você deseja usar para criar o dispositivo simulado e siga as etapas até que você crie o registro individual. 

Ao criar a inscrição individual, selecione **Ativar** para declarar que essa máquina virtual é um **dispositivo IoT Edge**.

Dispositivo simulado e guias de inscrição individuais: 
* [C](../iot-dps/quick-create-simulated-device.md)
* [Java](../iot-dps/quick-create-simulated-device-tpm-java.md)
* [C#](../iot-dps/quick-create-simulated-device-tpm-csharp.md)
* [Node.js](../iot-dps/quick-create-simulated-device-tpm-node.md)
* [Python](../iot-dps/quick-create-simulated-device-tpm-python.md)

Depois de criar o registro individual, salve o valor do **ID de registro**. Você usa esse valor ao configurar o tempo de execução do Azure IoT Edge. 

## <a name="install-the-iot-edge-runtime"></a>Instalar o tempo de execução do Azure IoT Edge

Depois de concluir a seção anterior, você deve ver o novo dispositivo listado como um dispositivo IoT Edge em seu Hub IoT. Agora, você precisa instalar o tempo de execução do IoT Edge no dispositivo. 

O tempo de execução do IoT Edge é implantado em todos os dispositivos IoT Edge. Seus componentes são executados em contêineres e permitem implantar contêineres adicionais no dispositivo para que você possa executar o código na borda. Em dispositivos que executam o Windows, você pode optar por usar contêineres do Windows ou contêineres do Linux. Escolha o tipo de contêineres que você deseja usar e siga as etapas. Certifique-se de configurar o tempo de execução do IoT Edge para provisionamento automático, não manual. 

Siga as instruções para instalar o tempo de execução do IoT Edge no dispositivo que está executando o TPM simulado da seção anterior. 

Saiba seu DPS **Escopo da ID** e do dispositivo **ID de registro** antes de começar estes artigos. 

* [Contêineres do Windows](how-to-install-iot-edge-windows-with-windows.md)
* [Contêineres do Linux](how-to-install-iot-edge-windows-with-linux.md)

## <a name="verify-successful-installation"></a>Verifique se a instalação bem-sucedida

Se o tempo de execução foi iniciado com êxito, você pode entrar em seu Hub IoT e iniciar a implantação de módulos do IoT Edge em seu dispositivo. Use os seguintes comandos em seu dispositivo para verificar o tempo de execução instalado e iniciado com êxito.  

Verifique o status do serviço do IoT Edge.

```powershell
Get-Service iotedge
```

Examine os logs de serviço pelos últimos 5 minutos usando.

```powershell
# Displays logs from last 5 min, newest at the bottom.

Get-WinEvent -ea SilentlyContinue `
  -FilterHashtable @{ProviderName= "iotedged";
    LogName = "application"; StartTime = [datetime]::Now.AddMinutes(-5)} |
  select TimeCreated, Message |
  sort-object @{Expression="TimeCreated";Descending=$false} |
  format-table -autosize -wrap
```

Módulos de execução da lista.

```powershell
iotedge list
```

## <a name="next-steps"></a>Próximas etapas

O processo de registro do serviço de provisionamento de dispositivo permite definir a ID do dispositivo e as marcas do dispositivo gêmeo ao mesmo tempo, como provisionar o novo dispositivo. Você pode usar esses valores para dispositivos individuais ou grupos de dispositivos usando o gerenciamento automático de dispositivo de destino. Saiba como [Implantar e monitorar os módulos de IoT Edge em escala usando o portal do Azure](how-to-deploy-monitor.md) ou [usando a CLI do Azure](how-to-deploy-monitor-cli.md)
