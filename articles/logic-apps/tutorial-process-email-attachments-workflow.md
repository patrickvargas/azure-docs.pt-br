---
title: Criar fluxos de trabalho para processar emails e anexos – Aplicativo Lógico do Azure | Microsoft Docs
description: Este tutorial mostra como criar fluxos de trabalho automatizados para que você possa processar emails e anexos com o Aplicativo Lógico do Azure, o Armazenamento do Azure e o Azure Functions
services: logic-apps
ms.service: logic-apps
author: ecfan
ms.author: estfan
manager: jeconnoc
ms.topic: tutorial
ms.custom: mvc
ms.date: 07/20/2018
ms.reviewer: klam, LADocs
ms.openlocfilehash: 3d4e91465e2f9986ec1029b304e1c026e39f45b6
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/30/2018
ms.locfileid: "50231961"
---
# <a name="process-emails-and-attachments-with-azure-logic-apps"></a>Processar emails e anexos com o Aplicativo Lógico do Azure

Os Aplicativos Lógicos do Azure ajudam a automatizar fluxos de trabalho e a integrar os dados entre os serviços do Azure, os serviços da Microsoft, outros aplicativos SaaS (software como serviço) e sistemas locais. Este tutorial mostra como você pode criar um [aplicativo lógico](../logic-apps/logic-apps-overview.md) que trata a emails recebidos e anexos. Esse aplicativo lógico processa esse conteúdo, salva-o no armazenamento do Azure e envia notificações para revisão do conteúdo. 

Neste tutorial, você aprenderá como:

> [!div class="checklist"]
> * Configurar o [Armazenamento do Azure](../storage/common/storage-introduction.md) e o Gerenciador de Armazenamento para verificar emails e anexos.
> * Criar uma [função do Azure](../azure-functions/functions-overview.md) que remova o HTML dos emails. Este tutorial inclui o código que você pode usar para essa função.
> * Criar um aplicativo lógico em branco.
> * Adicionar um gatilho que monitora anexos nos emails.
> * Adicionar uma condição que verifica se os emails têm anexos.
> * Adicionar uma ação que chama a função do Azure quando um email tem anexos.
> * Adicionar uma ação que cria blobs de armazenamento para emails e anexos.
> * Adicionar uma ação que envia notificações por email.

Quando terminar, o aplicativo lógico ficará parecido com este fluxo de trabalho em alto nível:

![Aplicativo lógico concluído em alto nível](./media/tutorial-process-email-attachments-workflow/overview.png)

Caso você não tenha uma assinatura do Azure, <a href="https://azure.microsoft.com/free/" target="_blank">crie uma conta gratuita do Azure</a> antes de começar. 

## <a name="prerequisites"></a>Pré-requisitos

* Uma conta de email de qualquer provedor de email compatível com os Aplicativos Lógicos, como o Outlook do Office 365, o Outlook.com ou o Gmail. Para outros provedores, [revise a lista de conectores aqui](https://docs.microsoft.com/connectors/).

  Esso aplicativo lógico usa uma conta do Outlook do Office 365. 
  Se você usar uma conta de email diferente, as etapas gerais serão as mesmas, mas a interface do usuário poderá parecer um pouco diferente.

* Baixe e instale o <a href="https://storageexplorer.com/" target="_blank">Gerenciador de Armazenamento do Microsoft Azure</a> gratuito. Essa ferramenta ajuda a verificar se o contêiner de armazenamento está configurado corretamente.

## <a name="sign-in-to-azure-portal"></a>Entre no portal do Azure

Entre no <a href="https://portal.azure.com" target="_blank">portal do Azure</a> com suas credenciais da conta do Azure.

## <a name="set-up-storage-to-save-attachments"></a>Configurar armazenamento para salvar anexos

Você pode salvar emails recebidos e anexos como blobs em um [contêiner de armazenamento do Azure](../storage/common/storage-introduction.md). 

1. Antes de criar um contêiner de armazenamento, [Crie uma conta de armazenamento](../storage/common/storage-quickstart-create-account.md) com estas configurações:

   | Configuração | Valor | DESCRIÇÃO | 
   |---------|-------|-------------| 
   | **Nome** | attachmentstorageacct | O nome da conta de armazenamento | 
   | **Modelo de implantação** | Gerenciador de recursos | O [modelo de implantação](../azure-resource-manager/resource-manager-deployment-model.md) para gerenciar a implantação de recursos | 
   | **Tipo de conta** | Propósito geral | O [tipo de conta de armazenamento](../storage/common/storage-introduction.md#types-of-storage-accounts) | 
   | **Localidade** | Oeste dos EUA | A região na qual armazenar informações sobre sua conta de armazenamento | 
   | **Replicação** | Armazenamento com redundância local (LRS) | Essa configuração especifica como os dados são copiados, armazenados, gerenciados e sincronizados. Confira [Replicação](../storage/common/storage-introduction.md#replication). | 
   | **Desempenho** | Standard | Essa configuração especifica os tipos de dados com suporte e a mídia para armazenar dados. Confira os [Tipos de contas de armazenamento](../storage/common/storage-introduction.md#types-of-storage-accounts). | 
   | **Transferência segura é necessária** | Desabilitado | Essa configuração especifica a segurança necessária para solicitações de conexões. Confira [Requer transferência segura](../storage/common/storage-require-secure-transfer.md). | 
   | **Assinatura** | <*nome-da-sua-assinatura-do-Azure*> | O nome e a ID da assinatura do Azure | 
   | **Grupo de recursos** | LA-Tutorial-RG | O nome do [grupo de recursos do Azure](../azure-resource-manager/resource-group-overview.md) usado para organizar e gerenciar os recursos relacionados. <p>**Observação:** um grupo de recursos reside dentro de uma região específica. Embora os itens neste tutorial possam não estar disponíveis em todas as regiões, tente usar a mesma região sempre que possível. | 
   | **Configurar redes virtuais** | Desabilitado | Para este tutorial, mantenha a configuração **Desabilitada**. | 
   |||| 

   Para criar sua conta de armazenamento, você também pode usar o [Azure PowerShell](../storage/common/storage-quickstart-create-storage-account-powershell.md) ou a [CLI do Azure](../storage/common/storage-quickstart-create-storage-account-cli.md).

2. Depois que o Azure implanta sua conta de armazenamento, obtenha a chave de acesso da sua conta de armazenamento:

   1. No menu de sua conta de armazenamento, em **Configurações**, selecione **Chaves de acesso**. 

   2. Copie o nome da conta de armazenamento e a **key1**; salve esses valores em um lugar seguro.

      ![Copie e salve a chave e o nome da conta de armazenamento](./media/tutorial-process-email-attachments-workflow/copy-save-storage-name-key.png)

   Para obter a chave de acesso da conta de armazenamento, você também pode usar o [Azure PowerShell](https://docs.microsoft.com/powershell/module/azurerm.storage/get-azurermstorageaccountkey) ou a [CLI do Azure](https://docs.microsoft.com/cli/azure/storage/account/keys?view=azure-cli-latest.md#az-storage-account-keys-list). 

3. Crie um contêiner de armazenamento de blobs para os anexos de email.
   
   1. No menu da conta de armazenamento, selecione **Visão Geral**. 
   Em **Serviços**, selecione **Blobs**.

      ![Adicionar contêiner do armazenamento de blobs](./media/tutorial-process-email-attachments-workflow/create-storage-container.png)

   2. Quando a página **Contêineres** for aberta, selecione **Contêiner** na barra de ferramentas. 

   3. Em **Novo contêiner**, insira "attachments" como nome do contêiner. 
   Em **Nível de acesso público**, selecione **Contêiner (acesso de leitura anônimo para contêineres e blobs)** e escolha **OK**.

      Quando estiver pronto, você pode encontrar seu contêiner de armazenamento na conta de armazenamento no portal do Azure:

      ![Contêiner de armazenamento concluído](./media/tutorial-process-email-attachments-workflow/created-storage-container.png)

   Para criar um contêiner de armazenamento, você também pode usar o [Azure PowerShell](https://docs.microsoft.com/powershell/module/azure.storage/new-azurestoragecontainer) ou a [CLI do Azure](https://docs.microsoft.com/cli/azure/storage/container?view=azure-cli-latest#az-storage-container-create). 

Em seguida, conecte o Gerenciador de Armazenamento à conta de armazenamento.

## <a name="set-up-storage-explorer"></a>Configurar o Gerenciador de Armazenamento

Agora, conecte o Gerenciador de Armazenamento à conta de armazenamento para que você possa confirmar que seu aplicativo lógico pode salvar anexos como blobs corretamente no contêiner de armazenamento.

1. Abra o Gerenciador de Armazenamento do Microsoft Azure. 

   O Gerenciador de Armazenamento solicita uma conexão com sua conta de armazenamento. 

2. No painel **Conectar-se ao Armazenamento do Azure**, selecione **Usar uma chave e nome de conta de armazenamento** e escolha **Avançar**. 

   ![Gerenciador de Armazenamento – conectar-se à conta de armazenamento](./media/tutorial-process-email-attachments-workflow/storage-explorer-choose-storage-account.png)

   > [!TIP]
   > Se nenhum aviso for exibido, escolha **Adicionar conta** na barra de ferramentas Gerenciador de Armazenamento.

3. Em **Nome da conta**, forneça o nome de sua conta de armazenamento. Em **Chave de conta**, forneça a chave de acesso que você salvou anteriormente. Selecione **Avançar**.

4. Confirme as informações de conexão e escolha **Conectar**. 

   O Gerenciador de Armazenamento cria a conexão e mostra sua conta de armazenamento na janela do Gerenciador em sob **(Local e Anexado)** > **Contas de Armazenamento**. 

5. Para encontrar seu contêiner de armazenamento de blobs, em **Contas de Armazenamento**, expanda sua conta de armazenamento, que aqui é **attachmentstorageacct** e expanda **Contêineres de Blob**, onde você encontra o contêiner **attachments**, por exemplo: 

   ![Gerenciador de Armazenamento – encontrar contêiner de armazenamento](./media/tutorial-process-email-attachments-workflow/storage-explorer-check-contianer.png)

Em seguida, crie uma [função do Azure](../azure-functions/functions-overview.md) que remova HTML do email de entrada.

## <a name="create-function-to-clean-html"></a>Criar função para limpar HTML

Agora, use o snippet de código fornecido por estas etapas para criar uma função do Azure que remova HTML de cada email recebido. Dessa forma, o conteúdo do email fica mais limpo e fácil de processar. Em seguida, você pode chamar essa função do seu aplicativo lógico.

1. Antes de criar uma função, [crie um aplicativo de funções](../azure-functions/functions-create-function-app-portal.md) com estas configurações:

   | Configuração | Valor | DESCRIÇÃO | 
   | ------- | ----- | ----------- | 
   | **Nome do aplicativo** | CleanTextFunctionApp | Um nome globalmente exclusivo e descritivo para o aplicativo de funções | 
   | **Assinatura** | <*nome-da-sua-assinatura-do-Azure*> | A mesma assinatura do Azure que você usou anteriormente | 
   | **Grupo de recursos** | LA-Tutorial-RG | O mesmo grupo de recursos do Azure que você usou anteriormente | 
   | **Plano de hospedagem** | Plano de consumo | Essa configuração determina como alocar e dimensionar recursos, como capacidade de computação, para executar seu aplicativo de funções. Confira [Comparação entre planos de hospedagem](../azure-functions/functions-scale.md). | 
   | **Localidade** | Oeste dos EUA | A mesma região que você usou anteriormente | 
   | **Armazenamento** | cleantextfunctionstorageacct | Crie uma conta de armazenamento para seu aplicativo de funções. Use apenas letras minúsculas e números. <p>**Observação:** a conta de armazenamento contém seus aplicativos de funções e é diferente da sua conta de armazenamento criada anteriormente para anexos de email. | 
   | **Application Insights** | Desativar | Ativa o monitoramento de aplicativos com o [Application Insights](../application-insights/app-insights-overview.md), mas, para este tutorial, escolha a configuração **Desativado**. | 
   |||| 

   Se o seu aplicativo de funções não abrir automaticamente após a implantação, localize o aplicativo no <a href="https://portal.azure.com" target="_blank">portal do Azure</a>. No menu principal do Azure, selecione **Aplicativo de Funções** e selecione seu aplicativo de funções. 

   ![Selecionar aplicativo de funções](./media/tutorial-process-email-attachments-workflow/select-function-app.png)

   Se o **Aplicativo de funções** não aparecer no menu do Azure, vá para **Todos os serviços**. Na caixa Pesquisar, localize e selecione **Aplicativos de Funções**. Para saber mais, confira [Criar sua função](../azure-functions/functions-create-first-azure-function.md).

   Caso contrário, o Azure abre seu aplicativo de funções automaticamente, como mostrado aqui:

   ![Aplicativo de funções criado](./media/tutorial-process-email-attachments-workflow/function-app-created.png)

   Você também pode usar a [CLI do Azure](../azure-functions/functions-create-first-azure-function-azure-cli.md) ou os [modelos do PowerShell e do Resource Manager](../azure-resource-manager/resource-group-template-deploy.md).

2. Em **Aplicativos de Funções**, expanda **CleanTextFunctionApp**e selecione **Funções**. Na barra de ferramentas de funções, selecione **Nova função**.

   ![Criar nova função](./media/tutorial-process-email-attachments-workflow/function-app-new-function.png)

3. Em **Escolher um modelo abaixo ou ir para o início rápido**, abra a lista **Cenário** e selecione **Core**. No modelo **Gatilho HTTP**, selecione **C#**.

   ![Selecionar o modelo de função](./media/tutorial-process-email-attachments-workflow/function-select-httptrigger-csharp-function-template.png)

   > [!NOTE]
   > Este exemplo fornece o código de exemplo em C# para que você possa acompanhar o exemplo sem precisar dominar C#.

4. No painel **Nova Função**, em **Nome**, insira ```RemoveHTMLFunction```. Mantenha o **Nível de Autorização** definido como **Função**e escolha **Criar**.

   ![Nomear sua função](./media/tutorial-process-email-attachments-workflow/function-provide-name.png)

5. Depois de abrir o editor, substitua o código do modelo por esse código de exemplo, que remove o HTML e retorna resultados para o chamador:

   ``` CSharp
   using System.Net;
   using System.Text.RegularExpressions;

   public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
   {
      log.Info($"HttpWebhook triggered");

      // Parse query parameter
      string emailBodyContent = await req.Content.ReadAsStringAsync();

      // Replace HTML with other characters
      string updatedBody = Regex.Replace(emailBodyContent, "<.*?>", string.Empty);
      updatedBody = updatedBody.Replace("\\r\\n", " ");
      updatedBody = updatedBody.Replace(@"&nbsp;", " ");

      // Return cleaned text
      return req.CreateResponse(HttpStatusCode.OK, new { updatedBody });
   }
   ```

6. Quando terminar, escolha **Salvar**. Para testar sua função, escolha **Testar** no ícone de seta (**<**) na extremidade direita do editor. 

   ![Abra o painel “Testar”](./media/tutorial-process-email-attachments-workflow/function-choose-test.png)

7. No painel **Testar**, em **Corpo da solicitação**, digite essa linha e escolha **Executar**.

   ```json
   {"name": "<p><p>Testing my function</br></p></p>"}
   ```

   ![Testar a função](./media/tutorial-process-email-attachments-workflow/function-run-test.png)

   A janela **Saída** mostra o resultado da função:

   ```json
   {"updatedBody":"{\"name\": \"Testing my function\"}"}
   ```

Depois de verificar que sua função funciona, crie o aplicativo lógico. Embora este tutorial mostre como criar uma função que remove o HTML de emails, os Aplicativos Lógicos também fornecem um conector **HTML para Texto**.

## <a name="create-your-logic-app"></a>Criar seu aplicativo lógico

1. No menu principal do Azure, selecione **Criar um recurso** > 
**Integração** > **Aplicativo Lógico**.

   ![Criar aplicativo lógico](./media/tutorial-process-email-attachments-workflow/create-logic-app.png)

2. Em **Criar aplicativo lógico**, forneça essas informações sobre seu aplicativo lógico, como mostrado e descrito. Quando tiver concluído, escolha **Fixar no painel** > **Criar**.

   ![Fornecer informações de aplicativo lógico](./media/tutorial-process-email-attachments-workflow/create-logic-app-settings.png)

   | Configuração | Valor | DESCRIÇÃO | 
   | ------- | ----- | ----------- | 
   | **Nome** | LA-ProcessAttachment | O nome do seu aplicativo lógico | 
   | **Assinatura** | <*nome-da-sua-assinatura-do-Azure*> | A mesma assinatura do Azure que você usou anteriormente | 
   | **Grupo de recursos** | LA-Tutorial-RG | O mesmo grupo de recursos do Azure que você usou anteriormente |
   | **Localidade** | Oeste dos EUA | A mesma região que você usou anteriormente | 
   | **Log Analytics** | Desativar | Para este tutorial, mantenha a configuração **Desativado**. | 
   |||| 

3. Depois que o Azure implanta o aplicativo, o Designer de Aplicativos Lógicos é exibido e mostra uma página com um vídeo introdutório e modelos de padrões de aplicativo lógico comuns. Em **Modelos**, escolha **Aplicativo lógico em branco**.

   ![Escolha o modelo de aplicativo lógico em branco](./media/tutorial-process-email-attachments-workflow/choose-logic-app-template.png)

Em seguida, adicione um [gatilho](../logic-apps/logic-apps-overview.md#logic-app-concepts) que escuta emails recebidos com anexos. Cada aplicativo lógico deve começar com um gatilho, que é disparado quando um evento específico ocorre ou quando novos dados atendem a uma condição específica. Para saber mais, confira [Criar seu primeiro aplicativo lógico](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="monitor-incoming-email"></a>Monitorar o recebimento de emails

1. No designer na caixa de pesquisa, digite "quando o novo email chega" como filtro. Selecione este disparador para seu provedor de email: **Quando um novo email é recebido – <*seu-provedor-de-email*>**

   Por exemplo: 

   ![Selecionar este disparador para o provedor de email: "Quando um novo email é recebido"](./media/tutorial-process-email-attachments-workflow/add-trigger-when-email-arrives.png)

   * Para as contas corporativas ou de estudante do Azure, selecione o Outlook do Office 365. 
   * Para contas pessoais da Microsoft, selecione Outlook.com. 

2. Se forem solicitadas credenciais, entre na sua conta de email para que os Aplicativos Lógicos possam se conectar à sua conta de email.

3. Agora forneça os critérios que o gatilho usa para filtrar novos emails.

   1. Especifique a pasta, o intervalo e a frequência de verificação de emails.

      ![Especificar a pasta, intervalo e frequência de verificação de emails](./media/tutorial-process-email-attachments-workflow/set-up-email-trigger.png)

      | Configuração | Valor | DESCRIÇÃO | 
      | ------- | ----- | ----------- | 
      | **Pasta** | Caixa de Entrada | A pasta de email a ser verificada | 
      | **Intervalo** | 1 | O número de intervalos de espera entre as verificações | 
      | **Frequência** | Minuto | A unidade de tempo para cada intervalo entre verificações | 
      |  |  |  | 
  
   2. Escolha **Mostrar opções avançadas** e especificar estas configurações:

      | Configuração | Valor | DESCRIÇÃO | 
      | ------- | ----- | ----------- | 
      | **Tem Anexo** | SIM | Obter somente emails com anexos. <p>**Observação:** o gatilho não remove emails da sua conta; ele verifica apenas novas mensagens e processa somente os emails que correspondem ao filtro de assunto. | 
      | **Incluir Anexos** | SIM | Obter os anexos como entrada para o fluxo de trabalho, em vez de simplesmente procurar anexos. | 
      | **Filtro de Assunto** | ```Business Analyst 2 #423501``` | O texto a ser localizado no assunto do email | 
      |  |  |  | 

4. Por enquanto, para ocultar os detalhes do gatilho, clique na barra de título do gatilho.

   ![Recolher forma para ocultar detalhes](./media/tutorial-process-email-attachments-workflow/collapse-trigger-shape.png)

5. Salve seu aplicativo lógico. Clique em **Salvar** na barra de ferramentas do designer.

   Seu aplicativo lógico agora está ativo, mas ele não faz nada além de verificar seus emails. 
   Em seguida, adicione uma condição que especifica os critérios para continuar o fluxo de trabalho.

## <a name="check-for-attachments"></a>Verificar anexos

Agora, adicione uma condição que seleciona somente emails com anexos.

1. No gatilho, escolha **Nova etapa** > **Adicionar uma condição**.

   ![“Nova etapa”, “Adicionar uma condição”](./media/tutorial-process-email-attachments-workflow/add-condition-under-trigger.png)

2. Renomeie a condição com uma descrição melhor.

   1. Na barra de título da condição, escolha o botão de **reticências** (**...** ) > **Renomear**.

      ![Renomear condição](./media/tutorial-process-email-attachments-workflow/condition-rename.png)

   2. Renomeie sua condição com esta descrição:```If email has attachments and key subject phrase```

3. Crie uma condição que verifica emails com anexos. 

   1. Na primeira linha em **E**, clique na caixa à esquerda. 
   Na lista de conteúdo dinâmico exibida, selecione a propriedade **Tem Anexo**.

      ![Condição da compilação](./media/tutorial-process-email-attachments-workflow/build-condition.png)

   2. Na caixa do meio, mantenha o operador **é igual a**.

   3. Na caixa à direita, insira **True** como o valor a ser comparado com o valor **Tem Anexo** da propriedade do gatilho.

      ![Condição da compilação](./media/tutorial-process-email-attachments-workflow/finished-condition.png)

      Se os dois valores forem iguais, o email tem pelo menos um anexo, a condição é atendida e o fluxo de trabalho continua.

   Na definição de aplicativo lógico subjacente, que você pode ver na janela do editor de código, a condição se parece com este exemplo:

   ```json
   "Condition": {
      "actions": { <actions-to-run-when-condition-passes> },
      "expression": {
         "and": [ {
            "equals": [
               "@triggerBody()?['HasAttachment']",
                 "True"
            ]
         } ]
      },
      "runAfter": {},
      "type": "If"
   }
   ```

4. Salve seu aplicativo lógico. Clique em **Salvar** na barra de ferramentas do designer.

### <a name="test-your-condition"></a>Testar a condição

Agora, teste se a condição funciona corretamente:

1. Se seu aplicativo lógico já não estiver em execução, escolha **Executar** na barra de ferramentas do designer.

   Esta etapa inicia o aplicativo lógico manualmente sem ter de esperar o intervalo especificado. 
   No entanto, nada acontecerá até que o email de teste chegue na caixa de entrada. 

2. Envie um email que atenda a estes critérios:

   * O assunto do email tem o texto que você especificou no **Filtro de assunto** do gatilho:```Business Analyst 2 #423501```

   * Seu email tem um anexo. 
   Por enquanto, basta criar um arquivo de texto vazio e anexar o arquivo ao seu email.

   Quando o email chega, seu aplicativo lógico verifica anexos e o texto de assunto especificado.
   Se a condição for atendida, o gatilho será acionado e fará com que o mecanismo de aplicativos lógicos crie uma instância do aplicativo lógico e inicie o fluxo de trabalho. 

3. Para verificar se o gatilho foi disparado e se o aplicativo lógico foi executado com êxito, no menu do aplicativo lógico, escolha **Visão Geral**.

   ![Verifique o histórico de execução e gatilhos](./media/tutorial-process-email-attachments-workflow/checkpoint-run-history.png)

   Se seu aplicativo lógico não dispara ou é executado apesar do êxito de um gatilho, confira [Solucionar problemas do seu aplicativo lógico](../logic-apps/logic-apps-diagnosing-failures.md).

Em seguida, defina as ações a serem executadas na ramificação **If true**. Para salvar o email junto com os anexos, remova o HTML do corpo do email e crie blobs no contêiner de armazenamento para email e anexos.

> [!NOTE]
> Sua lógica de aplicativo não precisa fazer nada na ramificação **If false** quando um email não tem anexos. Como um exercício extra depois de concluir este tutorial, você pode adicionar uma ação apropriada que deseja executar para a ramificação **If false**.

## <a name="call-removehtmlfunction"></a>Chamar RemoveHTMLFunction

Esta etapa adiciona a função do Azure criada anteriormente ao seu aplicativo lógico e passa o conteúdo de corpo do email do gatilho de email para sua função.

1. No menu de aplicativo lógico, selecione **Designer do Aplicativo Lógico**. Na ramificação **If true**, escolha **Adicionar uma ação**.

   ![Dentro de "If true", adicionar ação](./media/tutorial-process-email-attachments-workflow/if-true-add-action.png)

2. Na caixa de pesquisa, procure "azure functions" e selecione esta ação: **Escolher uma função do Azure – Azure Functions**

   ![Selecionar ação para "Escolher uma função do Azure"](./media/tutorial-process-email-attachments-workflow/add-action-azure-function.png)

3. Selecione o aplicativo de funções criado anteriormente: **CleanTextFunctionApp**

   ![Selecione o aplicativo de funções do Azure](./media/tutorial-process-email-attachments-workflow/add-action-select-azure-function-app.png)

4. Agora, selecione a função: **RemoveHTMLFunction**

   ![Selecionar a função do Azure](./media/tutorial-process-email-attachments-workflow/add-action-select-azure-function.png)

5. Renomeie a forma da função com esta descrição:```Call RemoveHTMLFunction to clean email body```

6. Agora, especifique a entrada que sua função processará. 

   1. Em **Corpo da Solicitação**, insira este texto com um espaço à direita: 
   
      ```{ "emailBody": ``` 

      Enquanto você trabalhar nessa entrada nas próximas etapas, aparecerá um erro sobre JSON inválido até que a entrada seja formatada corretamente como JSON.
      Quando você testou essa função anteriormente, a entrada especificada para a função usava JSON (JavaScript Object Notation). 
      Portanto, o corpo da solicitação deve usar o mesmo formato.

      Além disso, quando o cursor estiver dentro da caixa **Corpo da solicitação**, a lista de conteúdo dinâmico é exibida para que você possa selecionar valores de propriedade disponíveis de ações anteriores. 
      
   2. Na lista de conteúdo dinâmico, em **Quando um novo email é recebido**, selecione a propriedade **Corpo**. Após essa propriedade, lembre-se de adicionar a chave de fechamento: ```}```

      ![Especifique o corpo da solicitação para passar para a função](./media/tutorial-process-email-attachments-workflow/add-email-body-for-function-processing.png)

   Quando terminar, a entrada de sua função se parecerá com este exemplo:

   ![Corpo da solicitação concluído que será passado para sua função](./media/tutorial-process-email-attachments-workflow/add-email-body-for-function-processing-2.png)

7. Salve seu aplicativo lógico.

Em seguida, adicione uma ação que cria um blob em seu contêiner de armazenamento para você poder salvar o corpo do email.

## <a name="create-blob-for-email-body"></a>Criar blob para o corpo do email

1. No bloco **If true** e em sua função do Azure, escolha **Adicionar uma ação**. 

2. Na caixa de pesquisa, digite "criar blob" como filtro e selecione a ação: **Criar blob – Armazenamento de Blobs do Azure**

   ![Adicionar ação para criar um blob para o corpo do email](./media/tutorial-process-email-attachments-workflow/create-blob-action-for-email-body.png)

3. Crie uma conexão com a conta de armazenamento com essas configurações, como mostrado e descrito aqui. Quando terminar, escolha **Criar**.

   ![Criar conexão com a conta de armazenamento](./media/tutorial-process-email-attachments-workflow/create-storage-account-connection-first.png)

   | Configuração | Valor | DESCRIÇÃO | 
   | ------- | ----- | ----------- | 
   | **Nome da Conexão** | AttachmentStorageConnection | Um nome descritivo para a conexão | 
   | **Conta de armazenamento** | attachmentstorageacct | O nome da conta de armazenamento que você criou anteriormente para salvar os anexos | 
   |||| 

4. Renomeie a ação **Criar blob** com esta descrição:```Create blob for email body```

5. Na ação **Criar blob**, forneça estas informações e selecione estes campos para criar o blob, como mostrado e descrito:

   ![Fornecer informações de blob para o corpo do email](./media/tutorial-process-email-attachments-workflow/create-blob-for-email-body.png)

   | Configuração | Valor | DESCRIÇÃO | 
   | ------- | ----- | ----------- | 
   | **Caminho da pasta** | /attachments | O nome do contêiner que você criou anteriormente e o caminho até ele. Neste exemplo, clique no ícone de pasta e selecione o contêiner "/attachments". | 
   | **Nome de blob** | Campo **De** | Neste exemplo, use o nome do remetente como nome do blob. Clique nessa caixa para que apareça na lista de conteúdo dinâmico e selecione o campo **De** na ação **Quando um novo email é recebido**. | 
   | **Conteúdo do blob** | Campo **Conteúdo** | Neste exemplo, use o corpo do email sem HTML como conteúdo do blob. Clique nessa caixa para que apareça a lista de conteúdo dinâmico e selecione **Corpo** na ação **Chamar RemoveHTMLFunction para limpar o corpo do email**. |
   |||| 

   Após a conclusão, a ação será semelhante a este exemplo:

   ![Ação "Criar blob" concluída](./media/tutorial-process-email-attachments-workflow/create-blob-for-email-body-done.png)

6. Salve seu aplicativo lógico. 

### <a name="check-attachment-handling"></a>Verifique o tratamento de anexos

Agora teste se seu aplicativo lógico trata os emails da maneira especificada:

1. Se seu aplicativo lógico já não estiver em execução, escolha **Executar** na barra de ferramentas do designer.

2. Envie um email que atenda a estes critérios:

   * O assunto do email tem o texto que você especificou no **Filtro de assunto** do gatilho:```Business Analyst 2 #423501```

   * Seu email tem pelo menos um anexo. 
   Por enquanto, basta criar um arquivo de texto vazio e anexar o arquivo ao seu email.

   * Seu email tem conteúdo de teste no corpo, por exemplo: 

     ```
     Testing my logic app
     ```

   Se seu aplicativo lógico não dispara ou é executado apesar do êxito de um gatilho, confira [Solucionar problemas do seu aplicativo lógico](../logic-apps/logic-apps-diagnosing-failures.md).

3. Verifique se seu aplicativo lógico salvou o email no contêiner de armazenamento correto. 

   1. No Gerenciador de Armazenamento, expanda as **(Local e Anexado)** > 
    **Contas de Armazenamento** > **attachmentstorageacct (Externa)** > 
    **Contêineres de Blob** > **anexos**.

   2. Verifique o email no contêiner **anexos**. 

      Neste momento, somente o email aparece no contêiner porque o aplicativo lógico ainda não processa os anexos.

      ![Verificar o email salvo no Gerenciador de Armazenamento](./media/tutorial-process-email-attachments-workflow/storage-explorer-saved-email.png)

   3. Quando você terminar, exclua o email no Gerenciador de Armazenamento.

4. Opcionalmente, para testar a ramificação **If false**, que não faz nada no momento, você pode enviar um email que não atende aos critérios.

Em seguida, adicione um loop para processar todos os anexos de email.

## <a name="process-attachments"></a>Processar anexos

Para processar cada anexo no email, adicione um loop **For each** ao fluxo de trabalho do aplicativo lógico.

1. Na forma **Criar blob para o corpo do email**, selecione **Mais** > **Adicionar um for each**.

   ![Adicionar loop "for each"](./media/tutorial-process-email-attachments-workflow/add-for-each-loop.png)

2. Renomeie o loop com esta descrição:```For each email attachment```

3. Agora, especifique os dados para o loop processar. Clique na caixa **Selecionar uma saída das etapas anteriores** para que a lista de conteúdo dinâmico seja aberta e selecione **Anexos**. 

   ![Selecionar "Anexos"](./media/tutorial-process-email-attachments-workflow/select-attachments.png)

   O campo **Anexos** passa uma matriz que contém todos os anexos incluídos em um email. 
   O loop **for each** repete ações em cada item que é transmitido com a matriz.

4. Salve seu aplicativo lógico.

Em seguida, adicione a ação que salva cada anexo como um blob no contêiner de armazenamento **anexos**.

## <a name="create-blob-for-each-attachment"></a>Criar blob para cada anexo

1. No loop **for each email attachment**, escolha **Adicionar uma ação** para que você possa especificar a tarefa a ser executada em cada anexo encontrado.

   ![Adicionar ação para fazer loop](./media/tutorial-process-email-attachments-workflow/for-each-add-action.png)

2. Na caixa de pesquisa, digite "criar blob" como filtro e selecione a ação: **Criar blob – Armazenamento de Blobs do Azure**

   ![Adicionar ação para criar um blob](./media/tutorial-process-email-attachments-workflow/create-blob-action-for-attachments.png)

3. Renomeie a ação **Criar blob 2** com esta descrição:```Create blob for each email attachment```

4. Na ação **Criar blob para cada anexo de email**, forneça essas informações e selecione as propriedades para cada blob que você deseja criar, como mostrado e descrito:

   ![Fornecer informações de blob](./media/tutorial-process-email-attachments-workflow/create-blob-per-attachment.png)

   | Configuração | Valor | DESCRIÇÃO | 
   | ------- | ----- | ----------- | 
   | **Caminho da pasta** | /attachments | O nome do contêiner que você criou anteriormente e o caminho até ele. Neste exemplo, clique no ícone de pasta e selecione o contêiner "/attachments". | 
   | **Nome de blob** | Campo **Nome** | Neste exemplo, use o nome do anexo como nome do blob. Clique nessa caixa para que apareça na lista de conteúdo dinâmico e selecione o campo **Nome** na ação **Quando um novo email é recebido**. | 
   | **Conteúdo do blob** | Campo **Conteúdo** | Para este exemplo, use o campo **Conteúdo** como o conteúdo do blob. Clique nessa caixa para que apareça na lista de conteúdo dinâmico e selecione **Conteúdo** na ação **Quando um novo email é recebido**. |
   |||| 

   Após a conclusão, a ação será semelhante a este exemplo:

   ![Ação "Criar blob" concluída](./media/tutorial-process-email-attachments-workflow/create-blob-per-attachment-done.png)

5. Salve seu aplicativo lógico. 

### <a name="check-attachment-handling"></a>Verifique o tratamento de anexos

Em seguida, teste se seu aplicativo lógico trata os anexos da maneira especificada:

1. Se seu aplicativo lógico já não estiver em execução, escolha **Executar** na barra de ferramentas do designer.

2. Envie um email que atenda a estes critérios:

   * O assunto do email tem o texto que você especificou no **Filtro de assunto** do gatilho:```Business Analyst 2 #423501```

   * Seu email tem pelo menos dois anexos. 
   Por enquanto, basta criar dois arquivos de texto vazios e anexar os arquivos ao email.

   Se seu aplicativo lógico não dispara ou é executado apesar do êxito de um gatilho, confira [Solucionar problemas do seu aplicativo lógico](../logic-apps/logic-apps-diagnosing-failures.md).

3. Verifique se seu aplicativo lógico salvou o email e os anexos no contêiner de armazenamento correto. 

   1. No Gerenciador de Armazenamento, expanda as **(Local e Anexado)** > 
    **Contas de Armazenamento** > **attachmentstorageacct (Externa)** > 
    **Contêineres de Blob** > **anexos**.

   2. Verifique o email e os anexos no contêiner **anexos**.

      ![Verificar email e anexos salvos](./media/tutorial-process-email-attachments-workflow/storage-explorer-saved-attachments.png)

   3. Quando você terminar, exclua o email e os anexos no Gerenciador de Armazenamento.

Em seguida, adicione uma ação para que seu aplicativo lógico envie email para revisar os anexos.

## <a name="send-email-notifications"></a>Enviar notificações por email

1. Na ramificação **if true**, no loop **Para cada anexo de email**, escolha **Adicionar uma ação**. 

   ![Adicionar ação no loop "for each"](./media/tutorial-process-email-attachments-workflow/add-action-send-email.png)

2. Na caixa de pesquisa, insira "enviar email" como filtro e selecione a ação "enviar email" para seu provedor de email. 

   Para filtrar a lista de ações para um serviço específico, você pode selecionar primeiro o conector.

   ![Selecionar a ação "enviar email" para o seu provedor de email](./media/tutorial-process-email-attachments-workflow/add-action-select-send-email.png)

   * Para as contas corporativas ou de estudante do Azure, selecione o Outlook do Office 365. 
   * Para contas pessoais da Microsoft, selecione Outlook.com. 

3. Se forem solicitadas credenciais, entre na sua conta de email para que os Aplicativos Lógicos criem uma conexão à sua conta de email.

4. Renomeie a ação **Enviar um email** com esta descrição:```Send email for review```

5. Forneça as informações para a ação e selecione os campos que você deseja incluir no email, como mostrado e descrito. Para adicionar linhas em branco em uma caixa de edição, pressione Shift + Enter.  

   ![Enviar notificação por email](./media/tutorial-process-email-attachments-workflow/send-email-notification.png)

   Se você não encontrar um campo esperado na lista de conteúdo dinâmico, escolha **Ver mais** lado **Quando um novo email é recebido**. 

   | Configuração | Valor | Observações | 
   | ------- | ----- | ----- | 
   | **Corpo** | ```Please review new applicant:``` <p>```Applicant name: ``` **De** <p>```Application file location: ``` **Caminho** <p>```Application email content: ``` **Corpo** | O conteúdo do corpo do email. Clique nessa caixa, digite o texto de exemplo e, da lista de conteúdo dinâmico, selecione estes campos: <p>- O campo **De** em **Quando um novo email é recebido** </br>- O campo **Caminho** em **Criar blob para o corpo do email** </br>- O campo **Corpo** em **Chamar RemoveHTMLFunction para limpar o corpo do email** | 
   | **Assunto**  | ```ASAP - Review applicant for position: ``` **Assunto** | O assunto do email que você deseja incluir. Clique dentro dessa caixa, digite o texto de exemplo e, da lista de conteúdo dinâmico, selecione o campo **Assunto** em **Quando um novo email é recebido**. | 
   | **Para** | <*recipient-email-address*> | Para fins de teste, você pode usar seu próprio endereço de email. | 
   |||| 

   > [!NOTE] 
   > Caso você selecione um campo que tenha uma matriz, como o campo **Content**, que é uma matriz que contém anexos, o designer adicionará automaticamente um loop “For each” em torno da ação que faz referência a esse campo. Dessa forma, seu aplicativo lógico pode executar essa ação em cada item da matriz. Para remover o loop, remova o campo da matriz, mova a ação de referência para fora do loop, escolha as reticências (**...** ) na barra de título do loop e escolha **Excluir**.
     
6. Salve seu aplicativo lógico. 

Agora teste seu aplicativo lógico, que se parecerá com este exemplo:

![Aplicativo lógico concluído](./media/tutorial-process-email-attachments-workflow/complete.png)

## <a name="run-your-logic-app"></a>Executar seu aplicativo lógico

1. Envie um email que atenda a estes critérios:

   * O assunto do email tem o texto que você especificou no **Filtro de assunto** do gatilho:```Business Analyst 2 #423501```

   * Seu email tem em um ou mais anexos. 
   Você pode reutilizar um arquivo de texto vazio do teste anterior. 
   Para um cenário mais realista, anexe um arquivo de currículo.

   * O corpo do email tem este texto, que você pode copiar e colar:

     ```
     Name: Jamal Hartnett   
     
     Street address: 12345 Anywhere Road   
     
     City: Any Town   
     
     State or Country: Any State   
     
     Postal code: 00000   
     
     Email address: jamhartnett@outlook.com   
     
     Phone number: 000-000-0000   
     
     Position: Business Analyst 2 #423501   

     Technical skills: Dynamics CRM, MySQL, Microsoft SQL Server, JavaScript, Perl, Power BI, Tableau, Microsoft Office: Excel, Visio, Word, PowerPoint, SharePoint, and Outlook   

     Professional skills: Data, process, workflow, statistics, risk analysis, modeling; technical writing, expert communicator and presenter, logical and analytical thinker, team builder, mediator, negotiator, self-starter, self-managing  
     
     Certifications: Six Sigma Green Belt, Lean Project Management   
     
     Language skills: English, Mandarin, Spanish   
     
     Education: Master of Business Administration   
     ```

2. Execute seu aplicativo lógico. Se for bem-sucedido, o aplicativo lógico envia um email que se parece com este exemplo:

   ![Notificação de email enviada pelo aplicativo lógico](./media/tutorial-process-email-attachments-workflow/email-notification.png)

   Se você não receber nenhum email, verifique a pasta de Lixo eletrônico. 
   O filtro de lixo de email pode redirecionar esses tipos de mensagens. 
   Caso contrário, se você não tiver certeza de que seu aplicativo lógico foi executado corretamente, confira [Solução de problemas do aplicativo lógico](../logic-apps/logic-apps-diagnosing-failures.md).

Parabéns, você agora criou e executou um aplicativo lógico que automatiza tarefas em diferentes serviços do Azure e chama um código personalizado.

## <a name="clean-up-resources"></a>Limpar recursos

Quando não for mais necessário, exclua o grupo de recursos que contém o aplicativo lógico e os recursos relacionados. No menu principal do Azure, vá para **Grupos de recursos** e selecione o grupo de recursos de seu aplicativo lógico. Selecione **Excluir grupo de recursos**. Insira o nome do grupo de recursos como confirmação e escolha **Excluir**.

![Excluir grupo de recursos do aplicativo lógico](./media/tutorial-process-email-attachments-workflow/delete-resource-group.png)

## <a name="get-support"></a>Obtenha suporte

* Em caso de dúvidas, visite o [Fórum dos Aplicativos Lógicos do Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Para enviar ou votar em ideias de recurso, visite o [site de comentários do usuário de Aplicativos Lógicos](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você criou um aplicativo lógico que processa e armazena anexos de email integrando serviços do Azure, como o Armazenamento do Azure e o Azure Functions. Agora, saiba mais sobre outros conectores que você pode usar para criar aplicativos lógicos.

> [!div class="nextstepaction"]
> [Saiba mais sobre conectores de Aplicativos Lógicos](../connectors/apis-list.md)
