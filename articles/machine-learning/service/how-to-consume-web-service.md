---
title: Como consumir implantações de serviços da Web - Aprendizado de Máquina do Azure
description: Aprenda a consumir um serviço da Web criado pela implantação de um modelo do Azure Machine Learning. A implantação de um modelo do Azure Machine Learning cria um serviço da Web que expõe uma API REST. Você pode criar clientes para esta API usando a linguagem de programação de sua escolha. Neste documento, saiba como acessar a API usando Python e C#.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: raymondl
author: raymondlaghaeian
ms.reviewer: larryfr
ms.date: 10/30/2018
ms.openlocfilehash: 58c1b53a4b97aad7b916e593fd4d6b52b51b7a52
ms.sourcegitcommit: fa758779501c8a11d98f8cacb15a3cc76e9d38ae
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2018
ms.locfileid: "52262886"
---
# <a name="consume-an-azure-machine-learning-model-deployed-as-a-web-service"></a>Consumir um modelo de Azure Machine Learning implantado como um serviço web

A implantação de um modelo do Azure Machine Learning como um serviço da Web cria uma API REST. Você pode enviar dados para essa API e receber a previsão retornada pelo modelo. Neste documento, saiba como criar clientes para o serviço da Web usando C #, Go, Java e Python.

Um serviço da Web é criado quando você implanta uma imagem em uma Instância do Contêiner do Azure, no Serviço do Azure Kubernetes ou no Project Brainwave (matrizes de gate programáveis em campo). Imagens são criadas a partir de modelos registrados e arquivos de pontuação. O URI usado para acessar um serviço da Web pode ser recuperado usando o [SDK do Azure Machine Learning](https://aka.ms/aml-sdk). Se a autenticação estiver ativada, você também poderá usar o SDK para obter as chaves de autenticação.

O fluxo de trabalho geral ao criar um cliente que usa um serviço da Web ML é:

1. Use o SDK para obter as informações de conexão
1. Determinar o tipo de dados de solicitação usados pelo modelo
1. Crie um aplicativo que chame o serviço da web

## <a name="connection-information"></a>informações de conexão

> [!NOTE]
> O SDK de aprendizado de máquina do Azure é usado para obter as informações do serviço da Web. Esse é um SDK de Python. Enquanto ele é usado para recuperar informações sobre os serviços da Web, você pode usar qualquer idioma para criar um cliente para o serviço.

As informações de conexão do serviço da Web podem ser recuperadas usando o SDK do Aprendizado de Máquina do Azure. A classe [azureml.core.Webservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py) fornece as informações necessárias para criar um cliente. As seguintes propriedades `Webservice` que são úteis ao criar um aplicativo cliente:

* `auth_enabled` - Se a autenticação estiver ativada, `True`; caso contrário, `False`.
* `scoring_uri` -O endereço da API REST.

Existem três maneiras de recuperar essas informações para serviços da Web implementados:

* Quando você implanta um modelo, um objeto `Webservice` é retornado com informações sobre o serviço:

    ```python
    service = Webservice.deploy_from_model(name='myservice',
                                           deployment_config=myconfig,
                                           models=[model],
                                           image_config=image_config,
                                           workspace=ws)
    print(service.scoring_uri)
    ```

* Você pode usar `Webservice.list` para recuperar uma lista de serviços da Web implementados para modelos em sua área de trabalho. Você pode adicionar filtros para restringir a lista de informações retornadas. Para obter mais informações sobre o que pode ser filtrado, consulte a documentação de referência do [Webservice.list](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.webservice.webservice?view=azure-ml-py#list).

    ```python
    services = Webservice.list(ws)
    print(services[0].scoring_uri)
    ```

* Se você souber o nome do serviço implantado, você pode criar uma nova instância da `Webservice` e forneça o nome do espaço de trabalho e o serviço como parâmetros. O novo objeto contém informações sobre o serviço implantado.

    ```python
    service = Webservice(workspace=ws, name='myservice')
    print(service.scoring_uri)
    ```

### <a name="authentication-key"></a>Chave de autenticação

As chaves de autenticação são criadas automaticamente quando a autenticação é ativada para uma implantação.

* A autenticação é __ativada por padrão__ ao implantar no __Serviço do Azure Kubernetes__.
* A autenticação é __desabilitada por padrão__ durante a implantação na __instâncias de contêiner do Azure__.

Para controlar a autenticação, use o parâmetro `auth_enabled` ao criar ou atualizar uma implantação.

Se a autenticação estiver ativada, você poderá usar o método `get_keys` para recuperar uma chave de autenticação primária e secundária:

```python
primary, secondary = service.get_keys()
print(primary)
```

> [!IMPORTANT]
> Se você precisar regenerar uma chave, use [`service.regen_key`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py#regen-key).

## <a name="request-data"></a>Dados de solicitação

A API REST espera que o corpo da solicitação seja um documento JSON com a seguinte estrutura:

```json
{
    "data":
        [
            <model-specific-data-structure>
        ]
}
```

> [!IMPORTANT]
> A estrutura dos dados precisa corresponder ao esperado pelo script e modelo de pontuação no serviço. O script de pontuação pode modificar os dados antes de transmiti-lo ao modelo.

Por exemplo, o modelo na [Train dentro do bloco de anotações](https://github.com/Azure/MachineLearningNotebooks/tree/master/01.getting-started/01.train-within-notebook) exemplo espera uma matriz de números de 10. O script de pontuação deste exemplo cria uma matriz Numpy da solicitação e a transmite para o modelo. O exemplo a seguir mostra os dados espera que esse serviço:

```json
{
    "data": 
        [
            [
                0.0199132141783263, 
                0.0506801187398187, 
                0.104808689473925, 
                0.0700725447072635, 
                -0.0359677812752396, 
                -0.0266789028311707, 
                -0.0249926566315915, 
                -0.00259226199818282, 
                0.00371173823343597, 
                0.0403433716478807
            ]
        ]
}
``` 

O serviço da web pode aceitar vários conjuntos de dados em uma solicitação. Ele retorna um documento JSON contendo uma matriz de respostas.

## <a name="call-the-service-c"></a>Chamar o serviço (C#)

Este exemplo demonstra como usar o C # para chamar o serviço da Web criado a partir do exemplo [Treinar no caderno](https://github.com/Azure/MachineLearningNotebooks/tree/master/01.getting-started/01.train-within-notebook):

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Net.Http;
using System.Net.Http.Headers;
using Newtonsoft.Json;

namespace MLWebServiceClient
{
    // The data structure expected by the service
    internal class InputData
    {
        [JsonProperty("data")]
        // The service used by this example expects an array containing
        //   one or more arrays of doubles
        internal double[,] data;
    }
    class Program
    {
        static void Main(string[] args)
        {
            // Set the scoring URI and authentication key
            string scoringUri = "<your web service URI>";
            string authKey = "<your key>";

            // Set the data to be sent to the service.
            // In this case, we are sending two sets of data to be scored.
            InputData payload = new InputData();
            payload.data = new double[,] {
                {
                    0.0199132141783263,
                    0.0506801187398187,
                    0.104808689473925,
                    0.0700725447072635,
                    -0.0359677812752396,
                    -0.0266789028311707,
                    -0.0249926566315915,
                    -0.00259226199818282,
                    0.00371173823343597,
                    0.0403433716478807
                },
                {
                    -0.0127796318808497, 
                    -0.044641636506989, 
                    0.0606183944448076, 
                    0.0528581912385822, 
                    0.0479653430750293, 
                    0.0293746718291555, 
                    -0.0176293810234174, 
                    0.0343088588777263, 
                    0.0702112981933102, 
                    0.00720651632920303
                }
            };

            // Create the HTTP client
            HttpClient client = new HttpClient();
            // Set the auth header. Only needed if the web service requires authentication.
            client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", authKey);

            // Make the request
            try {
                var request = new HttpRequestMessage(HttpMethod.Post, new Uri(scoringUri));
                request.Content = new StringContent(JsonConvert.SerializeObject(payload));
                request.Content.Headers.ContentType = new MediaTypeHeaderValue("application/json");
                var response = client.SendAsync(request).Result;
                // Display the response from the web service
                Console.WriteLine(response.Content.ReadAsStringAsync().Result);
            }
            catch (Exception e)
            {
                Console.Out.WriteLine(e.Message);
            }
        }
    }
}
```

Os resultados retornados são semelhantes ao seguinte documento JSON:

```json
[217.67978776218715, 224.78937091757172]
```

## <a name="call-the-service-go"></a>Chamar o serviço (Ir)

Este exemplo demonstra como usar o recurso Go para chamar o serviço da Web criado a partir do exemplo [Train dentro do notebook](https://github.com/Azure/MachineLearningNotebooks/tree/master/01.getting-started/01.train-within-notebook):

```go
package main

import (
    "bytes"
    "encoding/json"
    "fmt"
    "io/ioutil"
    "net/http"
)

// Features for this model are an array of decimal values
type Features []float64

// The web service input can accept multiple sets of values for scoring
type InputData struct {
    Data []Features `json:"data",omitempty`
}

// Define some example data
var exampleData = []Features{
    []float64{
        0.0199132141783263, 
        0.0506801187398187, 
        0.104808689473925, 
        0.0700725447072635, 
        -0.0359677812752396, 
        -0.0266789028311707, 
        -0.0249926566315915, 
        -0.00259226199818282, 
        0.00371173823343597, 
        0.0403433716478807,
    },
    []float64{
        -0.0127796318808497, 
        -0.044641636506989, 
        0.0606183944448076, 
        0.0528581912385822, 
        0.0479653430750293, 
        0.0293746718291555, 
        -0.0176293810234174, 
        0.0343088588777263, 
        0.0702112981933102, 
        0.00720651632920303,
    },
}

// Set to the URI for your service
var serviceUri string = "<your web service URI>"
// Set to the authentication key (if any) for your service
var authKey string = "<your key>"

func main() {
    // Create the input data from example data
    jsonData := InputData{
        Data: exampleData,
    }
    // Create JSON from it and create the body for the HTTP request
    jsonValue, _ := json.Marshal(jsonData)
    body := bytes.NewBuffer(jsonValue)

    // Create the HTTP request
    client := &http.Client{}
    request, err := http.NewRequest("POST", serviceUri, body)
    request.Header.Add("Content-Type", "application/json")

    // These next two are only needed if using an authentication key
    bearer := fmt.Sprintf("Bearer %v", authKey)
    request.Header.Add("Authorization", bearer)

    // Send the request to the web service
    resp, err := client.Do(request)
    if err != nil {
        fmt.Println("Failure: ", err)
    }

    // Display the response received
    respBody, _ := ioutil.ReadAll(resp.Body)
    fmt.Println(string(respBody))
}
```

Os resultados retornados são semelhantes ao seguinte documento JSON:

```json
[217.67978776218715, 224.78937091757172]
```

## <a name="call-the-service-java"></a>Ligue para o serviço (Java)

Este exemplo demonstra como usar o Java para chamar o serviço da Web criado a partir do exemplo [Trainar no notebook](https://github.com/Azure/MachineLearningNotebooks/tree/master/01.getting-started/01.train-within-notebook):

```java
import java.io.IOException;
import org.apache.http.client.fluent.*;
import org.apache.http.entity.ContentType;
import org.json.simple.JSONArray;
import org.json.simple.JSONObject;

public class App {
    // Handle making the request
    public static void sendRequest(String data) {
        // Replace with the scoring_uri of your service
        String uri = "<your web service URI>";
        // If using authentication, replace with the auth key
        String key = "<your key>";
        try {
            // Create the request
            Content content = Request.Post(uri)
            .addHeader("Content-Type", "application/json")
            // Only needed if using authentication
            .addHeader("Authorization", "Bearer " + key)
            // Set the JSON data as the body
            .bodyString(data, ContentType.APPLICATION_JSON)
            // Make the request and display the response.
            .execute().returnContent();
            System.out.println(content);
        }
        catch (IOException e) {
            System.out.println(e);
        }
    }
    public static void main(String[] args) {
        // Create the data to send to the service
        JSONObject obj = new JSONObject();
        // In this case, it's an array of arrays
        JSONArray dataItems = new JSONArray();
        // Inner array has 10 elements
        JSONArray item1 = new JSONArray();
        item1.add(0.0199132141783263);
        item1.add(0.0506801187398187);
        item1.add(0.104808689473925);
        item1.add(0.0700725447072635);
        item1.add(-0.0359677812752396);
        item1.add(-0.0266789028311707);
        item1.add(-0.0249926566315915);
        item1.add(-0.00259226199818282);
        item1.add(0.00371173823343597);
        item1.add(0.0403433716478807);
        // Add the first set of data to be scored
        dataItems.add(item1);
        // Create and add the second set
        JSONArray item2 = new JSONArray();
        item2.add(-0.0127796318808497);
        item2.add(-0.044641636506989);
        item2.add(0.0606183944448076);
        item2.add(0.0528581912385822);
        item2.add(0.0479653430750293);
        item2.add(0.0293746718291555);
        item2.add(-0.0176293810234174);
        item2.add(0.0343088588777263);
        item2.add(0.0702112981933102);
        item2.add(0.00720651632920303);
        dataItems.add(item2);
        obj.put("data", dataItems);

        // Make the request using the JSON document string
        sendRequest(obj.toJSONString());
    }
}
```

Os resultados retornados são semelhantes ao seguinte documento JSON:

```json
[217.67978776218715, 224.78937091757172]
```

## <a name="call-the-service-python"></a>Chamar o serviço (Python)

Este exemplo demonstra como usar o Python para chamar o serviço da Web criado a partir do exemplo [Traino no notebook](https://github.com/Azure/MachineLearningNotebooks/tree/master/01.getting-started/01.train-within-notebook):

```python
import requests
import requests
import json

# URL for the web service
scoring_uri = '<your web service URI>'
# If the service is authenticated, set the key
key = '<your key>'

# Two sets of data to score, so we get two results back
data = {"data": 
            [
                [
                    0.0199132141783263, 
                    0.0506801187398187, 
                    0.104808689473925, 
                    0.0700725447072635, 
                    -0.0359677812752396, 
                    -0.0266789028311707, 
                    -0.0249926566315915, 
                    -0.00259226199818282, 
                    0.00371173823343597, 
                    0.0403433716478807
                ],
                [
                    -0.0127796318808497, 
                    -0.044641636506989, 
                    0.0606183944448076, 
                    0.0528581912385822, 
                    0.0479653430750293, 
                    0.0293746718291555, 
                    -0.0176293810234174, 
                    0.0343088588777263, 
                    0.0702112981933102, 
                    0.00720651632920303]
            ]
        }
# Convert to JSON string
input_data = json.dumps(data)

# Set the content type
headers = { 'Content-Type':'application/json' }
# If authentication is enabled, set the authorization header
headers['Authorization']=f'Bearer {key}'

# Make the request and display the response
resp = requests.post(scoring_uri, input_data, headers = headers)
print(resp.text)

```

Os resultados retornados são semelhantes ao seguinte documento JSON:

```JSON
[217.67978776218715, 224.78937091757172]
```

## <a name="next-steps"></a>Próximas etapas

Agora que você aprendeu como criar um cliente para um modelo implantado, saiba como [Implantar um modelo em um dispositivo IoT Edge](how-to-deploy-to-iot.md).
