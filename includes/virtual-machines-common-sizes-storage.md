---
title: Arquivo de inclusão
description: Arquivo de inclusão
services: virtual-machines
author: jonbeck7
ms.service: virtual-machines
ms.topic: include
ms.date: 07/06/2018
ms.author: azcspmt;jonbeck;cynthn
ms.custom: include file
ms.openlocfilehash: 961f82cd4970abfdd11a30b2847a14f8ff1880b0
ms.sourcegitcommit: f31bfb398430ed7d66a85c7ca1f1cc9943656678
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/28/2018
ms.locfileid: "47454557"
---
Os tamanhos de VM otimizados para armazenamento oferecem taxa de transferência de disco alta e de E/S e são ideais para bancos de dados Big Data, SQL e NoSQL. Este artigo fornece informações sobre o número de vCPUs, discos de dados e NICs, bem como a taxa de transferência de armazenamento e largura de banda de rede para cada tamanho neste agrupamento. 

A série Ls oferece até 32 vCPUs, usando a [família de processadores Intel® Xeon® E5 v3](http://www.intel.com/content/www/us/en/processors/xeon/xeon-e5-solutions.html). A série Ls obtém o mesmo desempenho de CPU da série G/GS e vem com 8 GiB de memória por vCPU.  As VMs da série Ls são ideais para aplicativos que exigem baixa latência, alta taxa de transferência e um amplo armazenamento em disco local. 

Exemplos de casos de uso: bancos de dados NoSQL, tais como Cassandra, MongoDB, Cloudera e Redis, armazenamento de dados e grandes bancos de dados transacionais.

> [!NOTE]
> A série Ls é otimizada para uso do disco temporário conectado ao computador da VM em vez do uso de discos de dados duráveis. A alta taxa de transferência e o IOPS do disco temporário torna a série Ls ideal para repositórios NoSQL, como Apache Cassandra e MongoDB, que replicam dados entre várias VMs para obter persistência no caso de falha de uma única VM. A série Ls não é compatível com a criação de um cache local para aumentar o IOPS que pode ser obtido por discos de dados duráveis.

## <a name="ls-series"></a>Série Ls

ACU: 180-240

Armazenamento Premium: com suporte

Cache de Armazenamento Premium: sem suporte
 
| Tamanho          | vCPU | Memória: GiB | Armazenamento temporário (SSD) GiB | Discos de dados máximos | Taxa de transferência máxima de armazenamento temporário: IOPS / MBps | Taxa de transferência máxima do disco não armazenado em cache: IOPS / MBps | Máximo de NICs/Largura de banda de rede esperado (Mbps) | 
|---------------|-----------|-------------|--------------------------|----------------|-------------------------------------------------------------|-------------------------------------------|------------------------------| 
| Standard_L4s   | 4    | 32   | 678   | 16    | 20.000 / 200   | 5.000 / 125        | 2 / 4.000  | 
| Standard_L8s   | 8    | 64   | 1.388 | 32   | 40.000 / 400   | 10.000 / 250       | 4 / 8.000  | 
| Standard_L16s  | 16   | 128  | 2.807 | 64   | 80,000 / 800   | 20.000 / 500       | 8 / 16.000 | 
| Standard_L32s <sup>1</sup> | 32   | 256  | 5.630 | 64   | 160.000 / 1.600   | 40.000 / 1.000     | 8 / 20.000 | 
 

A taxa de transferência máxima possível do disco com VMs da série Ls pode ser limitada pelo número, tamanho e divisão dos discos anexados. Para obter detalhes, confira [Armazenamento Premium: armazenamento de alto desempenho para cargas de trabalho das máquinas virtuais do Azure](../articles/virtual-machines/windows/premium-storage.md).

<sup>1</sup> A instância é isolada em hardware dedicado a um único cliente.

## <a name="size-table-definitions"></a>Definições da tabela de tamanhos

- A capacidade de armazenamento é mostrada em unidades de GiB ou de 1024^3 bytes. Ao comparar os discos medidos em GB (1000^3 bytes) com os discos medidos em GiB (1024^3), lembre-se de que os números de capacidade fornecidos em GiB poderão parecer menores. Por exemplo, 1023 GiB = 1098,4 GB
- A taxa de transferência do disco é medida em IOPS (operações de entrada/saída por segundo) e em MBps, em que MBps = 10^6 bytes/s.
- Se você quiser obter o melhor desempenho para suas VMs, será necessário limitar o número de discos de dados a 2 discos por vCPU.
- **Largura de banda de rede esperada** é a [largura de banda agregada máxima alocada por tipo de VM](../articles/virtual-network/virtual-machine-network-throughput.md) em todas as NICs para todos os destinos. Os limites superiores não são garantidos, mas devem fornecer diretrizes para selecionar o tipo de VM correto para o aplicativo desejado. O desempenho real da rede dependerá de uma variedade de fatores, incluindo cargas de rede e aplicativos, bem como configurações de rede. Para saber mais sobre como otimizar a taxa de transferência de rede, veja [Otimização da taxa de transferência de rede para Windows e Linux](../articles/virtual-network/virtual-network-optimize-network-bandwidth.md). Para obter o desempenho de rede esperado no Linux ou no Windows, poderá necessário selecionar uma versão específica ou otimizar sua VM. Para saber mais, veja [Como testar a taxa de transferência da máquina virtual de forma confiável](../articles/virtual-network/virtual-network-bandwidth-testing.md).
