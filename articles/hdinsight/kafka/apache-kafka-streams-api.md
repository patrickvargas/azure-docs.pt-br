---
title: 'Tutorial: Usar a API de Streams do Apache Kafka – Azure HDInsight '
description: Saiba como usar a API de Streams do Apache Kafka com o Kafka no HDInsight. Essa API permite realizar processamento de fluxo entre tópicos no Kafka.
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: tutorial
ms.date: 11/06/2018
ms.openlocfilehash: b22a701d9e876ca011381810e330fed60b7177d4
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/08/2018
ms.locfileid: "51278694"
---
# <a name="tutorial-apache-kafka-streams-api"></a>Tutorial: API de streams do Apache Kafka

Saiba como criar um aplicativo que usa a API de Streams do Kafka e executá-lo com o Kafka no HDInsight. 

O aplicativo usado neste tutorial é uma contagem de palavras de streaming. Ele lê dados de texto de um tópico Kafka, extrai palavras individuais e, em seguida, armazena a palavra e a contagem em outro tópico Kafka.

> [!NOTE]
> O processamento de fluxo do Kafka normalmente é feito usando o Apache Spark ou Storm. A versão Kafka 0.10.0 (no HDInsight 3.5 e 3.6) introduziu a API de Streams do Kafka. Essa API permite transformar fluxos de dados entre tópicos de entrada e saída. Em alguns casos, isso pode ser uma alternativa para criar uma solução de streaming Spark ou Storm. 
>
> Para obter mais informações sobre Streams do Kafka, consulte a documentação[Introdução a Streams](https://kafka.apache.org/10/documentation/streams/) em Apache.org.

Neste tutorial, você aprenderá como:

> [!div class="checklist"]
> * Configurar seu ambiente de desenvolvimento
> * Compreender o código
> * Compilar e implantar o aplicativo
> * Configurar tópicos Kafka
> * Executar o código

## <a name="prerequisites"></a>Pré-requisitos

* Um cluster Kafka no HDInsight 3.6. Para saber como criar um Kafka no Cluster HDInsight, consulte o documento [Iniciar com Kafka no HDInsight](apache-kafka-get-started.md).

* Conclua as etapas no documento [API de produtor e consumidor do Kafka](apache-kafka-producer-consumer-api.md). As etapas neste documento usam o aplicativo de exemplo e os tópicos criados neste tutorial.

## <a name="set-up-your-development-environment"></a>Configurar seu ambiente de desenvolvimento

Você deve ter os seguintes componentes instalados no ambiente de desenvolvimento:

* [Java JDK 8](https://aka.ms/azure-jdks) ou equivalente, como OpenJDK.

* [Apache Maven](http://maven.apache.org/)

* Um cliente SSH e o comando `scp`. Para saber mais, consulte o documento [Usar SSH com HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="understand-the-code"></a>Compreender o código

O aplicativo de exemplo está localizado em [https://github.com/Azure-Samples/hdinsight-kafka-java-get-started](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started), além do subdiretório `Streaming`. O aplicativo consiste de dois arquivos:

* `pom.xml`: Esse arquivo define as dependências do projeto, versão do Java e os métodos de empacotamento.
* `Stream.java`: Esse arquivo implementa a lógica de streaming.

### <a name="pomxml"></a>Pom.xml

As coisas importantes para entender no arquivo `pom.xml` são:

* Dependências: Este projeto depende da API do Kafka Streams, que é fornecida pelo pacote `kafka-clients`. O seguinte código XML define essa dependência:

    ```xml
    <!-- Kafka client for producer/consumer operations -->
    <dependency>
      <groupId>org.apache.kafka</groupId>
      <artifactId>kafka-clients</artifactId>
      <version>${kafka.version}</version>
    </dependency>
    ```

    > [!NOTE]
    > A entrada `${kafka.version}` é declarada na seção `<properties>..</properties>` de `pom.xml`, e está configurada para a versão Kafka do cluster HDInsight.

* Plug-ins: os plug-ins do Maven fornecem vários recursos. Neste projeto, são usados os seguintes plug-ins:

    * `maven-compiler-plugin`: Usado para definir a versão do Java usada pelo projeto para 8. Java 8 é exigido pelo HDInsight 3.6.
    * `maven-shade-plugin`: Usado para gerar um jar grande que contém esse aplicativo, bem como todas as dependências. Ele também é usado para definir o ponto de entrada do aplicativo, para que você possa executar diretamente o arquivo Jar sem a necessidade de especificar a classe principal.

### <a name="streamjava"></a>Stream.java

O arquivo [Stream.java](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started/blob/master/Streaming/src/main/java/com/microsoft/example/Stream.java) usa a API do Streams para implementar um aplicativo de contagem de palavras. Ele lê dados de um tópico Kafka chamado `test` e grava as contagens de palavras em um tópico chamado `wordcounts`.

O código a seguir define o aplicativo de contagem de palavras:

```java
package com.microsoft.example;

import org.apache.kafka.common.serialization.Serde;
import org.apache.kafka.common.serialization.Serdes;
import org.apache.kafka.streams.KafkaStreams;
import org.apache.kafka.streams.KeyValue;
import org.apache.kafka.streams.StreamsConfig;
import org.apache.kafka.streams.kstream.KStream;
import org.apache.kafka.streams.kstream.KStreamBuilder;

import java.util.Arrays;
import java.util.Properties;

public class Stream
{
    public static void main( String[] args ) {
        Properties streamsConfig = new Properties();
        // The name must be unique on the Kafka cluster
        streamsConfig.put(StreamsConfig.APPLICATION_ID_CONFIG, "wordcount-example");
        // Brokers
        streamsConfig.put(StreamsConfig.BOOTSTRAP_SERVERS_CONFIG, args[0]);
        // SerDes for key and values
        streamsConfig.put(StreamsConfig.KEY_SERDE_CLASS_CONFIG, Serdes.String().getClass().getName());
        streamsConfig.put(StreamsConfig.VALUE_SERDE_CLASS_CONFIG, Serdes.String().getClass().getName());

        // Serdes for the word and count
        Serde<String> stringSerde = Serdes.String();
        Serde<Long> longSerde = Serdes.Long();

        KStreamBuilder builder = new KStreamBuilder();
        KStream<String, String> sentences = builder.stream(stringSerde, stringSerde, "test");
        KStream<String, Long> wordCounts = sentences
                .flatMapValues(value -> Arrays.asList(value.toLowerCase().split("\\W+")))
                .map((key, word) -> new KeyValue<>(word, word))
                .countByKey("Counts")
                .toStream();
        wordCounts.to(stringSerde, longSerde, "wordcounts");

        KafkaStreams streams = new KafkaStreams(builder, streamsConfig);
        streams.start();

        Runtime.getRuntime().addShutdownHook(new Thread(streams::close));
    }
}
```


## <a name="build-and-deploy-the-example"></a>Compilar e implantar o exemplo

Para criar e implantar o projeto para o Kafka no Cluster HDInsight, utilize as seguintes etapas:

1. Baixe os exemplo de [https://github.com/Azure-Samples/hdinsight-kafka-java-get-started](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started).

2. Altere os diretórios para o diretório `Streaming` e utilize o seguinte comando para criar um pacote Jar:

    ```bash
    mvn clean package
    ```

    Este comando cria o pacote em `target/kafka-streaming-1.0-SNAPSHOT.jar`.

3. Utilize o seguinte comando para copiar o arquivo `kafka-streaming-1.0-SNAPSHOT.jar` para o Cluster HDInsight:
   
    ```bash
    scp ./target/kafka-streaming-1.0-SNAPSHOT.jar sshuser@clustername-ssh.azurehdinsight.net:kafka-streaming.jar
    ```
   
    Substitua `sshuser` pelo usuário do SSH do cluster e substitua `clustername` pelo nome do cluster. Se solicitado, insira a senha para a conta de usuário SSH. Para saber mais sobre como usar o `scp` com HDInsight, confira [Usar SSH com HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="create-kafka-topics"></a>Criar tópicos Kafka

1. Para abrir uma conexão SSH para o cluster, use o seguinte comando:

    ```bash
    ssh sshuser@clustername-ssh.azurehdinsight.net
    ```

    Substitua `sshuser` pelo usuário do SSH do cluster e substitua `clustername` pelo nome do cluster. Se solicitado, insira a senha para a conta de usuário SSH. Para saber mais sobre como usar o `scp` com HDInsight, confira [Usar SSH com HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

2. Para salvar o nome do cluster a uma variável e instalar um utilitário de análise de JSON (`jq`), use os seguintes comandos. Ao receber a solicitação, insira o nome do cluster Kafka:

    ```bash
    sudo apt -y install jq
    read -p 'Enter your Kafka cluster name:' CLUSTERNAME
    ```

3. Para obter os hosts de broker Kafka e os hosts Zookeeper, use os comandos a seguir. Quando solicitado, insira a senha para a conta de logon do cluster (admin). Você receberá uma solicitação de senha duas vezes.

    ```bash
    export KAFKAZKHOSTS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`; \
    export KAFKABROKERS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`; \
    ```

4. Para criar os tópicos usados pela operação de streaming, use os seguintes comandos:

    > [!NOTE]
    > Você pode receber um erro que o tópico `test` já existe. Não há problema nisso, pois ele pode ter sido criado no tutorial de API de produtor e consumidor.

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic test --zookeeper $KAFKAZKHOSTS

    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic wordcounts --zookeeper $KAFKAZKHOSTS

    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic RekeyedIntermediateTopic --zookeeper $KAFKAZKHOSTS

    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic wordcount-example-Counts-changelog --zookeeper $KAFKAZKHOSTS
    ```

    Os tópicos são usados para as seguintes finalidades:

    * `test`: Este tópico é onde os registros são recebidos. O aplicativo de streaming faz a leitura daqui.
    * `wordcounts`: Este tópico é onde o aplicativo de transmissão armazena sua saída.
    * `RekeyedIntermediateTopic`: Este tópico é usado para reparticionar dados como a contagem é atualizada pelo operador `countByKey`.
    * `wordcount-example-Counts-changelog`: Este tópico é um armazenamento de estado usado pela operação `countByKey`

    > [!IMPORTANT]
    > O Kafka no HDInsight também pode ser configurado para criar tópicos automaticamente. Para obter mais informações, consulte o documento [Configurar a criação automática de tópicos](apache-kafka-auto-create-topics.md).

## <a name="run-the-code"></a>Executar o código

1. Para iniciar o aplicativo de streaming como processo em segundo plano, use o seguinte comando:

    ```bash
    java -jar kafka-streaming.jar $KAFKABROKERS $KAFKAZKHOSTS &
    ```

    > [!NOTE]
    > Você pode receber um aviso sobre log4j. Você pode ignorar esse aviso.

2. Para enviar registros para o tópico `test`, use o seguinte comando para iniciar o aplicativo produtor:

    ```bash
    java -jar kafka-producer-consumer.jar producer test $KAFKABROKERS
    ```

3. Após a conclusão do produtor, use o seguinte comando para exibir as informações armazenadas no tópico `wordcounts`:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --bootstrap-server $KAFKABROKERS --topic wordcounts --formatter kafka.tools.DefaultMessageFormatter --property print.key=true --property key.deserializer=org.apache.kafka.common.serialization.StringDeserializer --property value.deserializer=org.apache.kafka.common.serialization.LongDeserializer --from-beginning
    ```

    > [!NOTE]
    > Os parâmetros `--property` informam ao consumidor do console para imprimir a chave (palavra) juntamente com a contagem (valor). Esses parâmetros também configuram o desserializador a ser usado ao fazer a leitura desses valores do Kafka.

    A saída é semelhante ao texto a seguir:
   
        dwarfs  13635
        ago     13664
        snow    13636
        dwarfs  13636
        ago     13665
        a       13803
        ago     13666
        a       13804
        ago     13667
        ago     13668
        jumped  13640
        jumped  13641
   
    > [!NOTE]
    > O parâmetro `--from-beginning` configura o consumidor para começar do início dos registros armazenados no tópico. A contagem aumenta sempre que uma palavra é encontrada, logo o tópico contém várias entradas para cada palavra, com uma contagem crescente.

7. Use __Ctrl + C__ para sair do produtor. Continue usando __Ctrl + C__ para sair do aplicativo e do consumidor.

## <a name="next-steps"></a>Próximas etapas

Neste documento, você aprendeu a usar a API de Streams do Kafka com Kafka no HDInsight. Confira o seguinte para obter mais informações sobre como trabalhar com o Kafka:

* [Analisar logs do Kafka](apache-kafka-log-analytics-operations-management.md)
* [Replicar dados entre clusters de Kafka](apache-kafka-mirroring.md)
