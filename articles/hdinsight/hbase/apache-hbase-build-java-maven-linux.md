---
title: Cliente HBase Java – Azure HDInsight
description: Saiba como usar o Apache Maven para compilar um aplicativo do Apache HBase baseado em Java e depois implantá-lo no HBase no HDInsight do Azure.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 04/30/2018
ms.author: hrasheed
ms.openlocfilehash: 677714487aac6e25a0505cce978792c76bb1cee4
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51016073"
---
# <a name="build-java-applications-for-apache-hbase"></a>Compilar aplicativos Java para Apache HBase

Saiba como criar um aplicativo [Apache HBase](http://hbase.apache.org/) em Java. Depois, use o aplicativo com o HBase no Azure HDInsight.

As etapas deste documentam usam [Maven](http://maven.apache.org/) para criar e compilar o projeto. Maven é uma ferramenta de software para compreensão e gerenciamento de projetos que permite a você compilar software, documentação e relatórios para projetos Java.

> [!NOTE]
> As etapas neste documento foram testadas mais recentemente com o HDInsight 3.6.

> [!IMPORTANT]
> As etapas deste documento exigem um cluster HDInsight que usa Linux. O Linux é o único sistema operacional usado no HDInsight versão 3.4 ou superior. Para obter mais informações, confira [baixa do HDInsight no Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="requirements"></a>Requisitos

* [Plataforma Java JDK](https://aka.ms/azure-jdks) 8 ou posterior.

    > [!NOTE]
    > O HDInsight 3.5 e posterior usa Java 8. Versões anteriores do HDInsight requerem Java 7.

* [Maven](http://maven.apache.org/)

* [Um cluster HDInsight do Azure baseado em Linux com HBase](apache-hbase-tutorial-get-started-linux.md#create-hbase-cluster)

## <a name="create-the-project"></a>Criar o projeto

1. Por meio da linha de comando em seu ambiente de desenvolvimento, mude os diretórios para o local em que você deseja criar o projeto, por exemplo, `cd code\hbase`.

2. Use o comando **mvn** , que é instalado com o Maven, para gerar o scaffolding para o projeto.

    ```bash
    mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=hbaseapp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

    > [!NOTE]
    > Se você estiver usando o PowerShell, coloque os parâmetros `-D` entre aspas duplas.
    >
    > `mvn archetype:generate "-DgroupId=com.microsoft.examples" "-DartifactId=hbaseapp" "-DarchetypeArtifactId=maven-archetype-quickstart" "-DinteractiveMode=false"`

    Esse comando cria um novo diretório com o mesmo nome que o parâmetro **artifactID** (**hbaseapp** neste exemplo). Esse diretório contém os seguintes itens:

   * **pom.xml**: o[POM](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html)(Modelo de Objeto de Projeto) contém informações e detalhes de configuração usados para compilar o projeto.
   * **src**: o diretório que contém **main\java\com\microsoft\examples**, no qual você criará seu aplicativo.

3. Exclua o arquivo `src/test/java/com/microsoft/examples/apptest.java`. Ele não será usado neste exemplo.

## <a name="update-the-project-object-model"></a>Atualize o modelo do objeto do projeto

1. Edite o arquivo `pom.xml` e adicione o seguinte código dentro da seção `<dependencies>`:

   ```xml
    <dependency>
        <groupId>org.apache.hbase</groupId>
        <artifactId>hbase-client</artifactId>
        <version>1.1.2</version>
    </dependency>
    <dependency>
        <groupId>org.apache.phoenix</groupId>
        <artifactId>phoenix-core</artifactId>
        <version>4.4.0-HBase-1.1</version>
    </dependency>
   ```

    Esta seção indica que o projeto precisa dos componentes **hbase-client** e **phoenix-core**. Em tempo de compilação, essa dependência será baixada do repositório padrão do Maven. Você pode usar a [Pesquisa de repositório central do Maven](http://search.maven.org/#artifactdetails%7Corg.apache.hbase%7Chbase-client%7C0.98.4-hadoop2%7Cjar) para saber mais sobre essa dependência.

   > [!IMPORTANT]
   > O número da versão do cliente hbase deve corresponder à versão do HBase fornecida com o cluster HDInsight. Use a tabela a seguir para localizar o número da versão correta.

   | Versão do cluster HDInsight | Versão do HBase a ser usada |
   | --- | --- |
   | 3.2 |0.98.4-hadoop2 |
   | 3.3, 3.4, 3.5 e 3.6 |1.1.2 |

    Para saber mais sobre as versões e os componentes do HDInsight, confira [Quais são os diferentes componentes do Hadoop disponíveis com o HDInsight?](../hdinsight-component-versioning.md).

3. Adicione o código a seguir ao arquivo **pom.xml** . Esse texto deve estar dentro das marcas `<project>...</project>` no arquivo, por exemplo, entre `</dependencies>` e `</project>`.

   ```xml
    <build>
        <sourceDirectory>src</sourceDirectory>
        <resources>
        <resource>
            <directory>${basedir}/conf</directory>
            <filtering>false</filtering>
            <includes>
            <include>hbase-site.xml</include>
            </includes>
        </resource>
        </resources>
        <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
                    <version>3.3</version>
            <configuration>
                <source>1.8</source>
                <target>1.8</target>
            </configuration>
            </plugin>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-shade-plugin</artifactId>
            <version>2.3</version>
            <configuration>
            <transformers>
                <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer">
                </transformer>
            </transformers>
            </configuration>
            <executions>
            <execution>
                <phase>package</phase>
                <goals>
                <goal>shade</goal>
                </goals>
            </execution>
            </executions>
        </plugin>
        </plugins>
    </build>
   ```

    Esta seção configura um recurso (`conf/hbase-site.xml`), que contém informações de configuração para o HBase.

   > [!NOTE]
   > Você também pode definir valores de configuração via código. Veja os comentários no exemplo `CreateTable`.

    Esta seção também configura os plug-ins [Maven Compiler](http://maven.apache.org/plugins/maven-compiler-plugin/) e [Maven Shade](http://maven.apache.org/plugins/maven-shade-plugin/). O plug-in compilador é usado para compilar a topologia. O plug-in de tonalidade é usado para evitar a duplicação de licenças no pacote JAR que é criado pelo Maven. O plug-in serve para evitar o erro de arquivos de licença duplicados no momento da execução no cluster do HDInsight. Utilize o plug-in de tonalidade do Maven com a implementação `ApacheLicenseResourceTransformer` para isso.

    O plug-in de tonalidade do Maven também produzirá um uber jar, que contém todas as dependências exigidas pelo aplicativo.

4. Salve o arquivo `pom.xml`.

5. Crie um diretório chamado `conf` no diretório `hbaseapp`. Esse diretório será usado para armazenar informações de configuração para se conectar ao HBase.

6. Use o seguinte comando para copiar a configuração HBase do cluster HBase para o diretório `conf`. Substitua `USERNAME` pelo nome de seu logon SSH. Substitua o `CLUSTERNAME` pelo nome do seu cluster HDInsight:

    ```bash
    scp USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:/etc/hbase/conf/hbase-site.xml ./conf/hbase-site.xml
    ```

   Para saber mais sobre como usar `ssh` e `scp`, confira [Usar SSH com HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="create-the-application"></a>Criar o aplicativo

1. Acesse o diretório `hbaseapp/src/main/java/com/microsoft/examples` e renomeie o arquivo app.java como `CreateTable.java`.

2. Abra o arquivo `CreateTable.java` e substitua o conteúdo existente pelo texto exibido a seguir:

   ```java
    package com.microsoft.examples;
    import java.io.IOException;

    import org.apache.hadoop.conf.Configuration;
    import org.apache.hadoop.hbase.HBaseConfiguration;
    import org.apache.hadoop.hbase.client.HBaseAdmin;
    import org.apache.hadoop.hbase.HTableDescriptor;
    import org.apache.hadoop.hbase.TableName;
    import org.apache.hadoop.hbase.HColumnDescriptor;
    import org.apache.hadoop.hbase.client.HTable;
    import org.apache.hadoop.hbase.client.Put;
    import org.apache.hadoop.hbase.util.Bytes;

    public class CreateTable {
        public static void main(String[] args) throws IOException {
        Configuration config = HBaseConfiguration.create();

        // Example of setting zookeeper values for HDInsight
        // in code instead of an hbase-site.xml file
        //
        // config.set("hbase.zookeeper.quorum",
        //            "zookeepernode0,zookeepernode1,zookeepernode2");
        //config.set("hbase.zookeeper.property.clientPort", "2181");
        //config.set("hbase.cluster.distributed", "true");
        //
        //NOTE: Actual zookeeper host names can be found using Ambari:
        //curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/hosts"

        //Linux-based HDInsight clusters use /hbase-unsecure as the znode parent
        config.set("zookeeper.znode.parent","/hbase-unsecure");

        // create an admin object using the config
        HBaseAdmin admin = new HBaseAdmin(config);

        // create the table...
        HTableDescriptor tableDescriptor = new HTableDescriptor(TableName.valueOf("people"));
        // ... with two column families
        tableDescriptor.addFamily(new HColumnDescriptor("name"));
        tableDescriptor.addFamily(new HColumnDescriptor("contactinfo"));
        admin.createTable(tableDescriptor);

        // define some people
        String[][] people = {
            { "1", "Marcel", "Haddad", "marcel@fabrikam.com"},
            { "2", "Franklin", "Holtz", "franklin@contoso.com" },
            { "3", "Dwayne", "McKee", "dwayne@fabrikam.com" },
            { "4", "Rae", "Schroeder", "rae@contoso.com" },
            { "5", "Rosalie", "burton", "rosalie@fabrikam.com"},
            { "6", "Gabriela", "Ingram", "gabriela@contoso.com"} };

        HTable table = new HTable(config, "people");

        // Add each person to the table
        //   Use the `name` column family for the name
        //   Use the `contactinfo` column family for the email
        for (int i = 0; i< people.length; i++) {
            Put person = new Put(Bytes.toBytes(people[i][0]));
            person.add(Bytes.toBytes("name"), Bytes.toBytes("first"), Bytes.toBytes(people[i][1]));
            person.add(Bytes.toBytes("name"), Bytes.toBytes("last"), Bytes.toBytes(people[i][2]));
            person.add(Bytes.toBytes("contactinfo"), Bytes.toBytes("email"), Bytes.toBytes(people[i][3]));
            table.put(person);
        }
        // flush commits and close the table
        table.flushCommits();
        table.close();
        }
    }
   ```

    Esse código é a classe **CreateTable**, que cria uma tabela chamada **pessoas** e a preenche com alguns usuários predefinidos.

3. Salve o arquivo `CreateTable.java`.

4. No diretório `hbaseapp/src/main/java/com/microsoft/examples`, crie um novo arquivo chamado `SearchByEmail.java`. Use o seguinte texto como o conteúdo deste arquivo:

   ```java
    package com.microsoft.examples;
    import java.io.IOException;

    import org.apache.hadoop.conf.Configuration;
    import org.apache.hadoop.hbase.HBaseConfiguration;
    import org.apache.hadoop.hbase.client.HTable;
    import org.apache.hadoop.hbase.client.Scan;
    import org.apache.hadoop.hbase.client.ResultScanner;
    import org.apache.hadoop.hbase.client.Result;
    import org.apache.hadoop.hbase.filter.RegexStringComparator;
    import org.apache.hadoop.hbase.filter.SingleColumnValueFilter;
    import org.apache.hadoop.hbase.filter.CompareFilter.CompareOp;
    import org.apache.hadoop.hbase.util.Bytes;
    import org.apache.hadoop.util.GenericOptionsParser;

    public class SearchByEmail {
        public static void main(String[] args) throws IOException {
        Configuration config = HBaseConfiguration.create();

        // Use GenericOptionsParser to get only the parameters to the class
        // and not all the parameters passed (when using WebHCat for example)
        String[] otherArgs = new GenericOptionsParser(config, args).getRemainingArgs();
        if (otherArgs.length != 1) {
            System.out.println("usage: [regular expression]");
            System.exit(-1);
        }

        // Open the table
        HTable table = new HTable(config, "people");

        // Define the family and qualifiers to be used
        byte[] contactFamily = Bytes.toBytes("contactinfo");
        byte[] emailQualifier = Bytes.toBytes("email");
        byte[] nameFamily = Bytes.toBytes("name");
        byte[] firstNameQualifier = Bytes.toBytes("first");
        byte[] lastNameQualifier = Bytes.toBytes("last");

        // Create a regex filter
        RegexStringComparator emailFilter = new RegexStringComparator(otherArgs[0]);
        // Attach the regex filter to a filter
        //   for the email column
        SingleColumnValueFilter filter = new SingleColumnValueFilter(
            contactFamily,
            emailQualifier,
            CompareOp.EQUAL,
            emailFilter
        );

        // Create a scan and set the filter
        Scan scan = new Scan();
        scan.setFilter(filter);

        // Get the results
        ResultScanner results = table.getScanner(scan);
        // Iterate over results and print  values
        for (Result result : results ) {
            String id = new String(result.getRow());
            byte[] firstNameObj = result.getValue(nameFamily, firstNameQualifier);
            String firstName = new String(firstNameObj);
            byte[] lastNameObj = result.getValue(nameFamily, lastNameQualifier);
            String lastName = new String(lastNameObj);
            System.out.println(firstName + " " + lastName + " - ID: " + id);
            byte[] emailObj = result.getValue(contactFamily, emailQualifier);
            String email = new String(emailObj);
            System.out.println(firstName + " " + lastName + " - " + email + " - ID: " + id);
        }
        results.close();
        table.close();
        }
    }
   ```

    A classe **SearchByEmail** pode ser usada para consultar por linhas, por endereço de email. Já que ele utiliza um filtro de expressão comum, você pode fornecer uma cadeia de caracteres ou então uma expressão comum ao utilizar a classe.

5. Salve o arquivo `SearchByEmail.java`.

6. No diretório `hbaseapp/src/main/hava/com/microsoft/examples`, crie um novo arquivo chamado `DeleteTable.java`. Use o seguinte texto como o conteúdo deste arquivo:

   ```java
    package com.microsoft.examples;
    import java.io.IOException;

    import org.apache.hadoop.conf.Configuration;
    import org.apache.hadoop.hbase.HBaseConfiguration;
    import org.apache.hadoop.hbase.client.HBaseAdmin;

    public class DeleteTable {
        public static void main(String[] args) throws IOException {
        Configuration config = HBaseConfiguration.create();

        // Create an admin object using the config
        HBaseAdmin admin = new HBaseAdmin(config);

        // Disable, and then delete the table
        admin.disableTable("people");
        admin.deleteTable("people");
        }
    }
   ```

    Essa classe limpa as tabelas HBase criadas nesse exemplo desabilitando e eliminando a tabela criada pela classe `CreateTable`.

7. Salve o arquivo `DeleteTable.java`.

## <a name="build-and-package-the-application"></a>Compilar e criar o pacote do aplicativo

1. Use o diretório `hbaseapp`, use o comando a seguir para compilar um arquivo JAR que contém o aplicativo:

    ```bash
    mvn clean package
    ```

    Este comando compila e empacota o aplicativo em um arquivo .jar.

2. Após a conclusão do comando, o diretório `hbaseapp/target` conterá um arquivo chamado `hbaseapp-1.0-SNAPSHOT.jar`.

   > [!NOTE]
   > O arquivo `hbaseapp-1.0-SNAPSHOT.jar` é um uber jar. Ele contém todas as dependências exigidas para executar o aplicativo.


## <a name="upload-the-jar-and-run-jobs-ssh"></a>Carregue o arquivo JAR e execute trabalhos (SSH)

As etapas a seguir usam `scp` para copiar o arquivo JAR no nó principal de seu HBase no cluster HDInsight. Então, o comando `ssh` será utilizado para se conectar ao cluster e executar o exemplo diretamente no nó principal.

1. Use o comando a seguir para carregar o jar no cluster:

    ```bash
    scp ./target/hbaseapp-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:hbaseapp-1.0-SNAPSHOT.jar
    ```

    Substitua `USERNAME` pelo nome de seu logon SSH. Substitua o `CLUSTERNAME` pelo nome do seu cluster HDInsight.

2. Use o comando a seguir para se conectar ao cluster HBase:

    ```bash
    ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
    ```

    Substitua `USERNAME` pelo nome de seu logon SSH. Substitua o `CLUSTERNAME` pelo nome do seu cluster HDInsight.

3. Use o seguinte comando para criar uma tabela do HBase usando o aplicativo Java:

    ```bash
    yarn jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.CreateTable
    ```

    Esse comando cria uma nova tabela do HBase chamada **people** e a preenche com dados.

4. Use o seguinte comando para procurar endereços de email armazenados na tabela:

    ```bash
    yarn jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.SearchByEmail contoso.com
    ```

    Você receberá as resultados a seguir:

        Franklin Holtz - ID: 2
        Franklin Holtz - franklin@contoso.com - ID: 2
        Rae Schroeder - ID: 4
        Rae Schroeder - rae@contoso.com - ID: 4
        Gabriela Ingram - ID: 6
        Gabriela Ingram - gabriela@contoso.com - ID: 6

5. Para excluir a tabela, use o seguinte comando:

    ```bash
    yarn jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.DeleteTable
    ```

## <a name="upload-the-jar-and-run-jobs-powershell"></a>Carregue o arquivo JAR e execute trabalhos (PowerShell)

As etapas a seguir usam o Azure PowerShell para carregar o arquivo JAR para o armazenamento padrão do cluster HBase. Os cmdlets do HDInsight serão usados para executar os exemplos remotamente.

1. Após instalar e configurar o Azure PowerShell, crie um arquivo chamado `hbase-runner.psm1`. Use o seguinte texto como o conteúdo deste arquivo:

   ```powershell
    <#
    .SYNOPSIS
    Copies a file to the primary storage of an HDInsight cluster.
    .DESCRIPTION
    Copies a file from a local directory to the blob container for
    the HDInsight cluster.
    .EXAMPLE
    Start-HBaseExample -className "com.microsoft.examples.CreateTable"
    -clusterName "MyHDInsightCluster"

    .EXAMPLE
    Start-HBaseExample -className "com.microsoft.examples.SearchByEmail"
    -clusterName "MyHDInsightCluster"
    -emailRegex "contoso.com"

    .EXAMPLE
    Start-HBaseExample -className "com.microsoft.examples.SearchByEmail"
    -clusterName "MyHDInsightCluster"
    -emailRegex "^r" -showErr
    #>

    function Start-HBaseExample {
    [CmdletBinding(SupportsShouldProcess = $true)]
    param(
    #The class to run
    [Parameter(Mandatory = $true)]
    [String]$className,

    #The name of the HDInsight cluster
    [Parameter(Mandatory = $true)]
    [String]$clusterName,

    #Only used when using SearchByEmail
    [Parameter(Mandatory = $false)]
    [String]$emailRegex,

    #Use if you want to see stderr output
    [Parameter(Mandatory = $false)]
    [Switch]$showErr
    )

    Set-StrictMode -Version 3

    # Is the Azure module installed?
    FindAzure

    # Get the login for the HDInsight cluster
    $creds=Get-Credential -Message "Enter the login for the cluster" -UserName "admin"

    # The JAR
    $jarFile = "wasb:///example/jars/hbaseapp-1.0-SNAPSHOT.jar"

    # The job definition
    $jobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
        -JarFile $jarFile `
        -ClassName $className `
        -Arguments $emailRegex

    # Get the job output
    $job = Start-AzureRmHDInsightJob `
        -ClusterName $clusterName `
        -JobDefinition $jobDefinition `
        -HttpCredential $creds
    Write-Host "Wait for the job to complete ..." -ForegroundColor Green
    Wait-AzureRmHDInsightJob `
        -ClusterName $clusterName `
        -JobId $job.JobId `
        -HttpCredential $creds
    if($showErr)
    {
    Write-Host "STDERR"
    Get-AzureRmHDInsightJobOutput `
                -Clustername $clusterName `
                -JobId $job.JobId `
                -HttpCredential $creds `
                -DisplayOutputType StandardError
    }
    Write-Host "Display the standard output ..." -ForegroundColor Green
    Get-AzureRmHDInsightJobOutput `
                -Clustername $clusterName `
                -JobId $job.JobId `
                -HttpCredential $creds
    }

    <#
    .SYNOPSIS
    Copies a file to the primary storage of an HDInsight cluster.
    .DESCRIPTION
    Copies a file from a local directory to the blob container for
    the HDInsight cluster.
    .EXAMPLE
    Add-HDInsightFile -localPath "C:\temp\data.txt"
    -destinationPath "example/data/data.txt"
    -ClusterName "MyHDInsightCluster"
    .EXAMPLE
    Add-HDInsightFile -localPath "C:\temp\data.txt"
    -destinationPath "example/data/data.txt"
    -ClusterName "MyHDInsightCluster"
    -Container "MyContainer"
    #>

    function Add-HDInsightFile {
        [CmdletBinding(SupportsShouldProcess = $true)]
        param(
            #The path to the local file.
            [Parameter(Mandatory = $true)]
            [String]$localPath,

            #The destination path and file name, relative to the root of the container.
            [Parameter(Mandatory = $true)]
            [String]$destinationPath,

            #The name of the HDInsight cluster
            [Parameter(Mandatory = $true)]
            [String]$clusterName,

            #If specified, overwrites existing files without prompting
            [Parameter(Mandatory = $false)]
            [Switch]$force
        )

        Set-StrictMode -Version 3

        # Is the Azure module installed?
        FindAzure

        # Get authentication for the cluster
        $creds=Get-Credential

        # Does the local path exist?
        if (-not (Test-Path $localPath))
        {
            throw "Source path '$localPath' does not exist."
        }

        # Get the primary storage container
        $storage = GetStorage -clusterName $clusterName

        # Upload file to storage, overwriting existing files if -force was used.
        Set-AzureStorageBlobContent -File $localPath `
            -Blob $destinationPath `
            -force:$force `
            -Container $storage.container `
            -Context $storage.context
    }

    function FindAzure {
        # Is there an active Azure subscription?
        $sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
        if(-not($sub))
        {
            throw "No active Azure subscription found! If you have a subscription, use the Connect-AzureRmAccount cmdlet to login to your subscription."
        }
    }

    function GetStorage {
        param(
            [Parameter(Mandatory = $true)]
            [String]$clusterName
        )
        $hdi = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        # Does the cluster exist?
        if (!$hdi)
        {
            throw "HDInsight cluster '$clusterName' does not exist."
        }
        # Create a return object for context & container
        $return = @{}
        $storageAccounts = @{}

        # Get storage information
        $resourceGroup = $hdi.ResourceGroup
        $storageAccountName=$hdi.DefaultStorageAccount.split('.')[0]
        $container=$hdi.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
        # Get the resource group, in case we need that
        $return.resourceGroup = $resourceGroup
        # Get the storage context, as we can't depend
        # on using the default storage context
        $return.context = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        # Get the container, so we know where to
        # find/store blobs
        $return.container = $container
        # Return storage accounts to support finding all accounts for
        # a cluster
        $return.storageAccount = $storageAccountName
        $return.storageAccountKey = $storageAccountKey

        return $return
    }
    # Only export the verb-phrase things
    export-modulemember *-*
   ```

    Esse arquivo contém dois módulos:

   * **Add-HDInsightFile** – utilizado para carregar arquivos no cluster
   * **Start-HBaseExample** - utilizado para executar as classes criadas anteriormente

2. Salve o arquivo `hbase-runner.psm1`.

3. Abra uma nova janela do Azure PowerShell, altere os diretórios para o diretório `hbaseapp` e execute o comando a seguir:

    ```powershell
    PS C:\ Import-Module c:\path\to\hbase-runner.psm1
    ```

    Altere o caminho até o local do arquivo `hbase-runner.psm1` criado anteriormente. Esse comando registra o módulo com o Azure PowerShell.

4. Use o comando a seguir para carregar o `hbaseapp-1.0-SNAPSHOT.jar` em seu cluster.

    ```powershell
    Add-HDInsightFile -localPath target\hbaseapp-1.0-SNAPSHOT.jar -destinationPath example/jars/hbaseapp-1.0-SNAPSHOT.jar -clusterName hdinsightclustername
    ```

    Substitua `hdinsightclustername` pelo nome do cluster. Quando solicitado, insira o nome e a senha do logon do cluster (admin). O comando carrega o `hbaseapp-1.0-SNAPSHOT.jar` no local `example/jars` no armazenamento primário do cluster.

5. Para criar uma tabela usando `hbaseapp`, use o seguinte comando:

    ```powershell
    Start-HBaseExample -className com.microsoft.examples.CreateTable -clusterName hdinsightclustername
    ```

    Substitua `hdinsightclustername` pelo nome do cluster. Quando solicitado, insira o nome e a senha do logon do cluster (admin).

    Esse comando cria uma tabela chamada **people** no HBase em seu cluster HDInsight. Esse comando não mostra nenhuma saída na janela do console.

6. Para procurar por entradas na tabela, use o comando a seguir:

    ```powershell
    Start-HBaseExample -className com.microsoft.examples.SearchByEmail -clusterName hdinsightclustername -emailRegex contoso.com
    ```

    Substitua `hdinsightclustername` pelo nome do cluster. Quando solicitado, insira o nome e a senha do logon do cluster (admin).

    Esse comando usa a classe `SearchByEmail` para procurar por quaisquer linhas nas quais a família de colunas `contactinformation` e a coluna `email` contenham a cadeia de caracteres `contoso.com`. Você deve receber os seguintes resultados:

          Franklin Holtz - ID: 2
          Franklin Holtz - franklin@contoso.com - ID: 2
          Rae Schroeder - ID: 4
          Rae Schroeder - rae@contoso.com - ID: 4
          Gabriela Ingram - ID: 6
          Gabriela Ingram - gabriela@contoso.com - ID: 6

    Usar **fabrikam.com** como o valor de `-emailRegex` retornará os usuários que tenham **fabrikam.com** no campo de email. Você também pode usar expressões regulares como o termo de pesquisa. Por exemplo, **^ r** retorna endereços que começam com a letra 'r'.

### <a name="no-results-or-unexpected-results-when-using-start-hbaseexample"></a>Nenhum resultado ou resultados inesperados ao usar o Start-HBaseExample

Utilize o parâmetro `-showErr` para exibir o erro padrão (STDERR) produzido durante a execução do trabalho.

## <a name="delete-the-table"></a>Excluir a tabela

Após ter terminado o exemplo, use o seguinte para excluir a tabela **pessoas** utilizada neste exemplo:

__De uma sessão `ssh`__:

`yarn jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.DeleteTable`

__Do Azure PowerShell__:

`Start-HBaseExample -className com.microsoft.examples.DeleteTable -clusterName hdinsightclustername`

## <a name="next-steps"></a>Próximas etapas

[Saiba como usar o SQuirreL SQL com o HBase](apache-hbase-phoenix-squirrel-linux.md)
