---
title: Visualizar dados da consulta interativa do Hive com o Power BI no Azure HDInsight
description: Usar o Microsoft Power BI para visualizar dados da consulta interativa do Hive do Azure HDInsight
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/06/2018
ms.openlocfilehash: 46b5b3436a33c3f83d85cfb02028759d53d214af
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51245266"
---
# <a name="visualize-interactive-query-hive-data-with-microsoft-power-bi-using-direct-query-in-azure-hdinsight"></a>Visualizar dados da consulta interativa do Hive com o Microsoft Power BI usando a consulta direta no Azure HDInsight

Este artigo descreve como conectar o Microsoft Power BI a clusters da consulta interativa do Azure HDInsight e visualizar os dados do Apache Hive usando a consulta direta. O exemplo fornecido carrega os dados de uma tabela de Hive hivesampletable no Power BI. A tabela de Hive hivesampletable contém alguns dados de uso de telefone celular. Em seguida, você cria gráficos com os dados de uso em um mapa mundial:

![Relatório de mapa de Power BI do HDInsight](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-power-bi-visualization.png)

Você pode aproveitar o [driver ODBC Hive](../hadoop/apache-hadoop-connect-hive-power-bi.md) a ser importado por meio do conector ODBC genérico no Power BI Desktop. No entanto, não é recomendável para cargas de trabalho de BI considerando a natureza não interativo do mecanismo de consulta do Hive. [Conector de consulta interativa HDInsight](./apache-hadoop-connect-hive-power-bi-directquery.md) e [conector do HDInsight Spark](https://docs.microsoft.com/power-bi/spark-on-hdinsight-with-direct-connect) são as melhores opções para o desempenho.

## <a name="prerequisites"></a>Pré-requisitos
Antes de prosseguir com este artigo, você deve ter os seguintes itens:

* **Cluster HDInsight**. O cluster pode ser um cluster do HDInsight com Hive ou um cluster da Consulta Interativa recém-lançado. Para a criação de clusters, consulte [Criar cluster](../hadoop/apache-hadoop-linux-tutorial-get-started.md#create-cluster).
* **[Microsoft Power BI Desktop](https://powerbi.microsoft.com/desktop/)**. Você pode baixar uma cópia do [Centro de Download da Microsoft](https://www.microsoft.com/download/details.aspx?id=45331).

## <a name="load-data-from-hdinsight"></a>Carregar dados do HDInsight

A tabela de Hive hivesampletable acompanha todos os clusters HDInsight.

1. Entre no Power BI Desktop.

2. Clique no ícone **Início**, clique em **Obter Dados** na faixa de opções **Dados externos** e, em seguida, selecione **Mais...**.

    ![Abrir dados do Power BI do HDInsight](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-power-bi-open-odbc.png)
    
3. No painel **Obter Dados**, digite **hdinsight** na caixa de pesquisa. Se não vir **Consulta Interativa do HDInsight (Beta)**, você precisará atualizar seu Power BI Desktop para a versão mais recente.

4. Selecione **Consulta Interativa do HDInsight (Beta)** e, em seguida, selecione **Conectar**.

5. Selecione **Continuar** para fechar a caixa de diálogo de aviso **Visualizar Conector**.

6. Em **Consulta Interativa do HDInsight**, selecione ou insira as seguintes informações:

    - **Servidor**: insira o nome do cluster da Consulta Interativa, por exemplo *myiqcluster.azurehdinsight.net*.

    - **Banco de Dados**: para este tutorial, insira **padrão**.
    
    - **Modo de Conectividade de Dados**: para este tutorial, selecione **DirectQuery**.

    ![Consulta interativa do HDInsight, conexão com o DirectQuery do Power BI](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-interactive-query-power-bi-connect.png)

7. Clique em **OK**.

8. Insira as credenciais do usuário HTTP e clique em **OK**. O nome de usuário padrão é **admin**

9. No painel esquerdo, selecione **hivesampletale** e, em seguida, clique em **Carregar**.

    ![Consulta interativa do HDInsight, hivesampletable do Power BI](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-interactive-query-power-bi-hivesampletable.png)

## <a name="visualize-data-on-a-map"></a>Visualizar dados em um mapa

Continue do último procedimento.

1. No painel Visualizações, selecione **Mapa**.  Ele é um ícone de globo.

    ![O Power BI do HDInsight personaliza o relatório](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-power-bi-customize.png)
    
2. No painel Campos, selecione **país** e **devicemake**. Você pode ver os dados no gráfico no mapa.

3. Expanda o mapa.

## <a name="next-steps"></a>Próximas etapas
Neste artigo, você aprendeu como visualizar os dados de HDInsight usando o Power BI.  Para obter mais informações sobre visualização de dados, consulte os seguintes artigos:

* [Visualizar dados de Hive com o Microsoft Power BI usando ODBC no Azure HDInsight](../hadoop/apache-hadoop-connect-hive-power-bi.md). 
* [Usar o Zeppelin para executar consultas do Hive no Azure HDInsight ](./../hdinsight-connect-hive-zeppelin.md).
* [Conectar o Excel ao HDInsight com o Driver ODBC do Microsoft Hive](../hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md).
* [Conectar o Excel ao Hadoop usando Power Query](../hadoop/apache-hadoop-connect-excel-power-query.md).
* [Conectar-se ao Azure HDInsight e executar consultas Hive usando Ferramentas do Data Lake para Visual Studio](../hadoop/apache-hadoop-visual-studio-tools-get-started.md).
* [Use a Ferramenta do Azure HDInsight para Visual Studio Code](../hdinsight-for-vscode.md).
* [Carregue os Dados no HDInsight](./../hdinsight-upload-data.md).
