---
title: Copiar dados de fontes OData usando o Azure Data Factory | Microsoft Docs
description: Saiba como copiar dados de fontes OData para armazenamentos de dados de coletor com suporte usando uma atividade de cópia em um pipeline do Azure Data Factory.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/22/2018
ms.author: jingwang
ms.openlocfilehash: c8bee6902fb74cb77c34395fd05c1c861b4f630e
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/12/2018
ms.locfileid: "49166127"
---
# <a name="copy-data-from-an-odata-source-by-using-azure-data-factory"></a>Copiar dados de uma fonte OData usando o Azure Data Factory

> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versão 1](v1/data-factory-odata-connector.md)
> * [Versão atual](connector-odata.md)

Este artigo descreve como usar a Atividade de Cópia no Azure Data Factory para copiar dados de uma fonte OData. O artigo baseia-se em [Atividade de Cópia no Azure Data Factory](copy-activity-overview.md), que apresenta uma visão geral da Atividade de Cópia.

## <a name="supported-capabilities"></a>Funcionalidades com suporte

Você pode copiar dados de uma origem OData para qualquer repositório de dados de coletor com suporte. Para obter uma lista de armazenamentos de dados que o Copy Activity suporta como fontes e coletores, consulte [Armazenamentos de dados e formatos compatíveis](copy-activity-overview.md#supported-data-stores-and-formats).

Especificamente, este conector OData dá suporte:

- OData versão 3.0 e 4.0.
- Cópia de dados usando uma das seguintes autenticações: **Anônima**, **Básica** ou **Windows**.

## <a name="get-started"></a>Introdução

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

As seções a seguir fornecem detalhes sobre propriedades que você pode usar para definir entidades do Data Factory específicas do conector OData.

## <a name="linked-service-properties"></a>Propriedades do serviço vinculado

As propriedades a seguir são compatíveis com o serviço vinculado do OData:

| Propriedade | DESCRIÇÃO | Obrigatório |
|:--- |:--- |:--- |
| Tipo | A propriedade **type** precisa ser definida como **OData**. |SIM |
| url | A URL raiz do serviço OData. |SIM |
| authenticationType | O tipo de autenticação usado para se conectar à fonte OData. Os valores permitidos são **Anônima**, **Básica** e **Windows**. OAuth não é compatível. | SIM |
| userName | Especifique o **userName** se estiver usando a autenticação Básica ou do Windows. | Não  |
| Senha | Especifique a **senha** da conta de usuário que você especificou para **userName**. Marque esse campo como um tipo **SecureString** para armazená-lo com segurança no Data Factory. Você também pode [referenciar um segredo armazenado no Azure Key Vault](store-credentials-in-key-vault.md). | Não  |
| connectVia | O [Tempo de Integração](concepts-integration-runtime.md) a ser usado para se conectar ao armazenamento de dados. Você pode escolher o Azure Integration Runtime ou o Integration Runtime auto-hospedado (se o armazenamento de dados estiver localizado em uma rede privada). Se não especificado, o Tempo de Execução de Integração do Azure padrão será usado. |Não  |

**Exemplo 1: usando a autenticação Anônima**

```json
{
    "name": "ODataLinkedService",
    "properties": {
        "type": "OData",
        "typeProperties": {
            "url": "http://services.odata.org/OData/OData.svc",
            "authenticationType": "Anonymous"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Exemplo 2: usando a autenticação Básica**

```json
{
    "name": "ODataLinkedService",
    "properties": {
        "type": "OData",
        "typeProperties": {
            "url": "<endpoint of OData source>",
            "authenticationType": "Basic",
            "userName": "<user name>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Exemplo 3: usando a autenticação do Windows**

```json
{
    "name": "ODataLinkedService",
    "properties": {
        "type": "OData",
        "typeProperties": {
            "url": "<endpoint of on-premises OData source>",
            "authenticationType": "Windows",
            "userName": "<domain>\\<user>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Propriedades do conjunto de dados

Esta seção fornece uma lista de propriedades compatíveis com o conjunto de dados OData.

Para obter uma lista completa de seções e propriedades disponíveis para definição de conjuntos de dados, consulte [Conjuntos de dados e serviços vinculados](concepts-datasets-linked-services.md). 

Para copiar dados do OData, defina a propriedade **type** do conjunto de dados como **ODataResource**. Há suporte para as seguintes propriedades:

| Propriedade | DESCRIÇÃO | Obrigatório |
|:--- |:--- |:--- |
| Tipo | A propriedade **type** do conjunto de dados precisa ser definida como **ODataResource**. | SIM |
| caminho | O caminho para o recurso OData. | SIM |

**Exemplo**

```json
{
    "name": "ODataDataset",
    "properties":
    {
        "type": "ODataResource",
        "linkedServiceName": {
            "referenceName": "<OData linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties":
        {
            "path": "Products"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Propriedades da Atividade de Cópia

Esta seção fornece uma lista de propriedades compatíveis com a fonte OData.

Para obter uma lista completa de seções e propriedades que estão disponíveis para definir atividades, consulte [Pipelines](concepts-pipelines-activities.md). 

### <a name="odata-as-source"></a>OData como fonte

Para copiar dados do OData, defina o tipo de **origem** na Atividade de Cópia como **RelationalSource**. As seguintes propriedades são suportadas na seção **source** da atividade de cópia:

| Propriedade | DESCRIÇÃO | Obrigatório |
|:--- |:--- |:--- |
| Tipo | A propriedade **type** da fonte da Atividade de Cópia precisa ser definida como **RelationalSource**. | SIM |
| query | Opções de consulta OData para filtrar dados. Exemplo: `"?$select=Name,Description&$top=5"`.<br/><br/>**Observação**: o conector do OData copia os dados da URL combinada: `[URL specified in linked service]/[path specified in dataset][query specified in copy activity source]`. Para saber mais, confira as [Componentes da URL do OData](http://www.odata.org/documentation/odata-version-3-0/url-conventions/). | Não  |

**Exemplo**

```json
"activities":[
    {
        "name": "CopyFromOData",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<OData input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "RelationalSource",
                "query": "?$select=Name,Description&$top=5"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="data-type-mapping-for-odata"></a>Mapeamento de tipo de dados para o OData

Ao copiar dados do OData, os seguintes mapeamentos são usados entre os tipos de dados OData e os tipos de dados provisórios do Azure Data Factory. Para saber como a Atividade de Cópia mapeia o esquema de origem e o tipo de dados para o coletor, confira [Esquema e mapeamentos de tipo de dados](copy-activity-schema-and-type-mapping.md).

| Tipo de dados OData | Tipo de dados provisório do Data Factory |
|:--- |:--- |
| Edm.Binary | Byte[] |
| Edm.Boolean | Bool |
| Edm.Byte | Byte[] |
| Edm.DateTime | Datetime |
| Edm.Decimal | Decimal |
| Edm.Double | Duplo |
| Edm.Single | Single |
| Edm.Guid | Guid |
| Edm.Int16 | Int16 |
| Edm.Int32 | Int32 |
| Edm.Int64 | Int64 |
| Edm.SByte | Int16 |
| Edm.String | Cadeia de caracteres |
| Edm.Time | timespan |
| Edm.DateTimeOffset | DateTimeOffset |

> [!NOTE]
> Tipos de dados complexos do OData (como **Objeto**) não têm suporte.


## <a name="next-steps"></a>Próximas etapas

Para obter uma lista de armazenamentos de dados que o Copy Activity suporta como fontes e coletores no Azure Data Factory, consulte [Armazenamentos e formatos de dados compatíveis](copy-activity-overview.md##supported-data-stores-and-formats).