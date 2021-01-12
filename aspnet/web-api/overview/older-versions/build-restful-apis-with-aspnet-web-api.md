---
uid: web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
title: Criar APIs RESTful com ASP.NET Web API-ASP.NET 4. x
author: rick-anderson
description: 'Laboratório prático: Use a API da Web no ASP.NET 4. x para criar uma API REST simples para um aplicativo do Gerenciador de contatos.'
ms.author: riande
ms.date: 02/18/2013
ms.custom: seoapril2019
ms.assetid: 87daa99f-3810-407e-b969-dd28a192959d
msc.legacyurl: /web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 72ce313c380547f74d5dc17d1aefbf7cf2da4bea
ms.sourcegitcommit: d4e2a07eeb2cdf19f0bfbfab4a469970bc7e1c99
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/12/2021
ms.locfileid: "98105280"
---
# <a name="build-restful-apis-with-aspnet-web-api"></a>Criar APIs RESTful com ASP.NET Web API

por [equipe de acampamentos da Web](https://twitter.com/webcamps)

> Laboratório prático: Use a API da Web no ASP.NET 4. x para criar uma API REST simples para um aplicativo do Gerenciador de contatos. Você também criará um cliente para consumir a API.

Nos últimos anos, ficou claro que HTTP não serve apenas para servir páginas HTML. Também é uma plataforma poderosa para a criação de APIs da Web, usando alguns verbos (GET, POST e assim por diante), além de alguns conceitos simples, como *URIs* e *cabeçalhos*. ASP.NET Web API é um conjunto de componentes que simplificam a programação HTTP. Como ele é criado sobre o tempo de execução MVC do ASP.NET, a API da Web manipula automaticamente os detalhes de transporte de nível inferior do HTTP. Ao mesmo tempo, a API Web expõe naturalmente o modelo de programação HTTP. Na verdade, uma meta da API Web é *não* abstrair a realidade do http. Como resultado, a API da Web é flexível e fácil de estender.  O estilo de arquitetura REST provou ser uma maneira eficaz de aproveitar HTTP, embora, certamente, não seja a única abordagem válida para HTTP. O Gerenciador de contatos irá expor o RESTful para listagem, adição e remoção de contatos, entre outros. 

Este laboratório requer uma compreensão básica do HTTP, do REST e pressupõe que você tenha um conhecimento funcional básico de HTML, JavaScript e jQuery.
> 
> > [!NOTE]
> > O site da ASP.NET tem uma área dedicada à estrutura de ASP.NET Web API em [https://asp.net/web-api](https://asp.net/web-api) . Este site continuará a fornecer informações mais recentes, amostras e notícias relacionadas à API da Web, portanto, verifique com frequência se você gostaria de se aprofundar na arte de criar APIs Web personalizadas disponíveis para praticamente qualquer estrutura de dispositivo ou de desenvolvimento.
> > 
> > ASP.NET Web API, semelhante ao ASP.NET MVC 4, tem grande flexibilidade em termos de separar a camada de serviço dos controladores, permitindo que você use várias das estruturas de injeção de dependência disponíveis razoavelmente fáceis. 
> 
> 
> Todos os códigos de exemplo e trechos de código estão incluídos no Web acampamentos Training Kit, disponível em [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409) .

<a id="Objectives"></a>
### <a name="objectives"></a>Objetivos

Neste laboratório prático, você aprenderá a:

- Implementar uma API Web RESTful
- Chamar a API de um cliente HTML

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Pré-requisitos

O seguinte é necessário para concluir este laboratório prático:

- [Microsoft Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) ou superior (leia o [Apêndice B](#AppendixB) para obter instruções sobre como instalá-lo).

<a id="Setup"></a>
### <a name="setup"></a>Instalação

**Instalando trechos de código**

Para sua conveniência, grande parte do código que você gerenciará ao longo deste laboratório está disponível como trechos de código do Visual Studio. Para instalar os trechos de código, execute o arquivo **.\Source\Setup\CodeSnippets.VSI** .

Se você não estiver familiarizado com os trechos de Visual Studio Code e quiser saber como usá-los, consulte o apêndice deste documento &quot; [Apêndice a: usando trechos de código](#AppendixA) &quot; .

<a id="Exercises"></a>
## <a name="exercises"></a>Exercícios

Este laboratório prático inclui o seguinte exercício:

1. [Exercício 1: criar uma API Web do Read-Only](#Exercise1)
2. [Exercício 2: criar uma API da Web de leitura/gravação](#Exercise2)
3. [Exercício 3: consumir a API Web de um cliente HTML](#Exercise3)

> [!NOTE]
> Cada exercício é acompanhado por uma pasta **final** que contém a solução resultante que você deve obter depois de concluir os exercícios. Você pode usar essa solução como um guia se precisar de ajuda adicional para trabalhar com os exercícios.

Tempo estimado para concluir este laboratório: **60 minutos**.

<a id="Exercise1"></a>

<a id="Exercise_1_Create_a_Read-Only_Web_API"></a>
### <a name="exercise-1-create-a-read-only-web-api"></a>Exercício 1: criar uma API Web do Read-Only

Neste exercício, você vai implementar os métodos GET somente leitura para o Gerenciador de contatos.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_API_Project"></a>
#### <a name="task-1---creating-the-api-project"></a>Tarefa 1-criando o projeto de API

Nesta tarefa, você usará os novos modelos de projeto Web ASP.NET para criar um aplicativo Web da API Web.

1. Execute o **Visual Studio 2012 Express para Web**, para fazer isso, vá para **iniciar** e digite **vs Express para Web** pressione **Enter**.
2. No menu **arquivo** , selecione **novo projeto**. Selecione o **Visual C# |** Tipo de projeto Web no modo de exibição de árvore do tipo de projeto e, em seguida, selecione o tipo de projeto de **aplicativo Web ASP.NET MVC 4** . Defina o **nome** do projeto como *ContactManager* e o **nome da solução** como *begin* e clique em **OK**.

    ![Criando um novo projeto de aplicativo Web do ASP.NET MVC 4,0](build-restful-apis-with-aspnet-web-api/_static/image1.png "Criando um novo projeto de aplicativo Web do ASP.NET MVC 4,0")

    *Criando um novo projeto de aplicativo Web do ASP.NET MVC 4,0*
3. Na caixa de diálogo tipo de projeto do ASP.NET MVC 4, selecione o tipo de projeto de **API Web** . Clique em **OK**.

    ![Especificando o tipo de projeto de API Web](build-restful-apis-with-aspnet-web-api/_static/image2.png "Especificando o tipo de projeto de API Web")

    *Especificando o tipo de projeto de API Web*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_the_Contact_Manager_API_Controllers"></a>
#### <a name="task-2---creating-the-contact-manager-api-controllers"></a>Tarefa 2-Criando os controladores de API do Gerenciador de contatos

Nesta tarefa, você criará as classes de controlador nas quais os métodos de API residirão.

1. Exclua o arquivo chamado **ValuesController.cs** na pasta **controladores** do projeto.
2. Clique com o botão direito do mouse na pasta **controladores** no projeto e selecione **Adicionar | Controlador** no menu de contexto.

    ![Adicionando um novo controlador ao projeto](build-restful-apis-with-aspnet-web-api/_static/image3.png "Adicionando um novo controlador ao projeto")

    *Adicionando um novo controlador ao projeto*
3. Na caixa de diálogo **Adicionar controlador** exibida, selecione **controlador de API vazio** no menu modelo. Nomeie a classe de controlador **ContactController**. Em seguida, clique em **Adicionar.**

    ![Usando a caixa de diálogo Adicionar controlador para criar um novo controlador de API Web](build-restful-apis-with-aspnet-web-api/_static/image4.png "Usando a caixa de diálogo Adicionar controlador para criar um novo controlador de API Web")

    *Usando a caixa de diálogo Adicionar controlador para criar um novo controlador de API Web*
4. Adicione o código a seguir ao **ContactController**.

    (Trecho de código- *laboratório de API Web-Ex01-método de API Get*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample1.cs)]
5. Pressione **F5** para depurar o aplicativo. O home page padrão para um projeto de API Web deve aparecer.

    ![O home page padrão de um aplicativo ASP.NET Web API](build-restful-apis-with-aspnet-web-api/_static/image5.png "O home page padrão de um aplicativo ASP.NET Web API")

    *O home page padrão de um aplicativo ASP.NET Web API*
6. Na janela do Internet Explorer, pressione a tecla **F12** para abrir a janela **ferramentas para desenvolvedores** . Clique na guia **rede** e, em seguida, clique no botão **Iniciar captura** para começar a capturar o tráfego de rede na janela.

    ![Abrindo a guia rede e iniciando a captura de rede](build-restful-apis-with-aspnet-web-api/_static/image6.png "Abrindo a guia rede e iniciando a captura de rede")

    *Abrindo a guia rede e iniciando a captura de rede*
7. Acrescente a URL na barra de endereços do navegador com **/API/Contact** e pressione Enter. Os detalhes da transmissão aparecerão na janela captura de rede. Observe que o tipo MIME da resposta é **Application/JSON**. Isso demonstra como o formato de saída padrão é JSON.

    ![Exibindo a saída da solicitação da API Web na exibição de rede](build-restful-apis-with-aspnet-web-api/_static/image7.png "Exibindo a saída da solicitação da API Web na exibição de rede")

    *Exibindo a saída da solicitação da API Web na exibição de rede*

    > [!NOTE]
    > O comportamento padrão do Internet Explorer 10 neste momento será perguntar se o usuário deseja salvar ou abrir o fluxo resultante da chamada à API Web. A saída será um arquivo de texto que contém o resultado JSON da chamada de URL da API Web. Não cancele a caixa de diálogo para poder assistir ao conteúdo da resposta por meio da janela de ferramentas de desenvolvedores.
8. Clique no botão **ir para exibição detalhada** para ver mais detalhes sobre a resposta dessa chamada à API.

    ![Alternar para exibição detalhada](build-restful-apis-with-aspnet-web-api/_static/image8.png "Alternar para exibição de detalhes")

    *Alternar para exibição detalhada*
9. Clique na guia **corpo da resposta** para exibir o texto real da resposta JSON.

    ![Exibindo o texto de saída JSON no monitor de rede](build-restful-apis-with-aspnet-web-api/_static/image9.png "Exibindo o texto de saída JSON no monitor de rede")

    *Exibindo o texto de saída JSON no monitor de rede*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Creating_the_Contact_Models_and_Augment_the_Contact_Controller"></a>
#### <a name="task-3---creating-the-contact-models-and-augment-the-contact-controller"></a>Tarefa 3-criando os modelos de contato e aumentando o controlador de contato

Nesta tarefa, você criará as classes de controlador nas quais os métodos de API residirão.

1. Clique com o botão direito do mouse na pasta **modelos** e selecione **Adicionar | Classe...** no menu de contexto.

    ![Adicionando um novo modelo ao aplicativo Web](build-restful-apis-with-aspnet-web-api/_static/image10.png "Adicionando um novo modelo ao aplicativo Web")

    *Adicionando um novo modelo ao aplicativo Web*
2. Na caixa de diálogo **Adicionar novo item** , nomeie o novo arquivo **Contact.cs** e clique em **Adicionar.**

    ![Criando o novo arquivo de classe de contato](build-restful-apis-with-aspnet-web-api/_static/image11.png "Criando o novo arquivo de classe de contato")

    *Criando o novo arquivo de classe de contato*
3. Adicione o seguinte código realçado à classe **Contact** .

    (Trecho de código- *laboratório da API Web-Ex01-classe de contato*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample2.cs)]
4. Na classe **ContactController** , selecione a palavra **cadeia de caracteres** na definição de método do método **Get** e digite a palavra *contato*. Depois que a palavra for digitada, um indicador será exibido no início da palavra **contato**. Mantenha pressionada a tecla **Ctrl** e pressione a tecla period (.) ou clique no ícone usando o mouse para abrir a caixa de diálogo assistência no editor de códigos, para preencher automaticamente a diretiva **using** para o namespace Models.

    ![Usando a assistência do IntelliSense para declarações de namespace](build-restful-apis-with-aspnet-web-api/_static/image12.png)

    *Usando a assistência do IntelliSense para declarações de namespace*
5. Modifique o código para o método **Get** para que ele retorne uma matriz de instâncias de modelo de contato.

    (Trecho de código- *laboratório da API Web-Ex01-retornando uma lista de contatos*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample3.cs)]
6. Pressione **F5** para depurar o aplicativo Web no navegador. Para exibir as alterações feitas na saída de resposta da API, execute as etapas a seguir.

   1. Depois que o navegador for aberto, pressione **F12** se as ferramentas de desenvolvedor ainda não estiverem abertas.
   2. Clique na guia **rede** .
   3. Pressione o botão **Iniciar captura** .
   4. Adicione o sufixo de URL **/API/Contact** à URL na barra de endereços e pressione a tecla **Enter** .
   5. Pressione o botão **ir para exibição detalhada** .
   6. Selecione a guia **corpo da resposta** . Você deve ver uma cadeia de caracteres JSON que representa a forma serializada de uma matriz de instâncias de contato.

      ![Saída serializada JSON de uma chamada de método de API Web complexa](build-restful-apis-with-aspnet-web-api/_static/image13.png "Saída serializada JSON de uma chamada de método de API Web complexa")

      *Saída serializada JSON de uma chamada de método de API Web complexa*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Extracting_Functionality_into_a_Service_Layer"></a>
#### <a name="task-4---extracting-functionality-into-a-service-layer"></a>Tarefa 4-extraindo funcionalidade em uma camada de serviço

Essa tarefa demonstrará como extrair a funcionalidade em uma camada de serviço para facilitar para os desenvolvedores a separação da funcionalidade de serviço da camada do controlador, permitindo, assim, a reutilização dos serviços que realmente fazem o trabalho.

1. Crie uma nova pasta na raiz da solução e nomeie-a como **Serviços** de ti. Para fazer isso, clique com o botão direito do mouse em **ContactManager** Project, selecione **Adicionar**  |  **nova pasta**, nomeie os *Serviços* de ti.

    ![Criando pasta de serviços](build-restful-apis-with-aspnet-web-api/_static/image14.png "Criando pasta de serviços")

    *Criando pasta de serviços*
2. Clique com o botão direito do mouse na pasta **Serviços** e selecione **Adicionar | Classe...** no menu de contexto.

    ![Adicionando uma nova classe à pasta serviços](build-restful-apis-with-aspnet-web-api/_static/image15.png "Adicionando uma nova classe à pasta serviços")

    *Adicionando uma nova classe à pasta serviços*
3. Quando a caixa de diálogo **Adicionar novo item** for exibida, nomeie a nova classe **ContactRepository** e clique em **Adicionar**.

    ![Criando um arquivo de classe para conter o código para a camada de serviço do repositório de contatos](build-restful-apis-with-aspnet-web-api/_static/image16.png "Criando um arquivo de classe para conter o código para a camada de serviço do repositório de contatos")

    *Criando um arquivo de classe para conter o código para a camada de serviço do repositório de contatos*
4. Adicione uma diretiva using ao arquivo **ContactRepository.cs** para incluir o namespace de modelos.

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample4.cs)]
5. Adicione o seguinte código realçado ao arquivo **ContactRepository.cs** para implementar o método GetAllContacts.

    (Trecho de código- *laboratório da API Web-Ex01-repositório de contatos*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample5.cs)]
6. Abra o arquivo **ContactController.cs** se ele ainda não estiver aberto.
7. Adicione a seguinte instrução using à seção de declaração de namespace do arquivo.

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample6.cs)]
8. Adicione o seguinte código realçado à classe **ContactController.cs** para adicionar um campo particular para representar a instância do repositório, para que o restante dos membros da classe possa fazer uso da implementação do serviço.

    (Trecho de código- *laboratório da API Web-Ex01-controlador de contato*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample7.cs)]
9. Altere o método **Get** para que ele faça uso do serviço de repositório de contatos.

    (Trecho de código- *laboratório da API Web-Ex01-retornando uma lista de contatos por meio do repositório*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample8.cs)]
10. Coloque um ponto de interrupção na definição do método **Get** de **ContactController**.

   ![Adicionando pontos de interrupção ao controlador de contato](build-restful-apis-with-aspnet-web-api/_static/image17.png "Adicionando pontos de interrupção ao controlador de contato")

   *Adicionando pontos de interrupção ao controlador de contato*
11. Pressione **F5** para executar o aplicativo.
12. Quando o navegador for aberto, pressione **F12** para abrir as ferramentas de desenvolvedor.
13. Clique na guia **rede** .
14. Clique no botão **Iniciar captura** .
15. Acrescente a URL na barra de endereços com o sufixo **/API/Contact** e pressione **Enter** para carregar o controlador de API.
16. O Visual Studio 2012 deve interromper uma vez que o método **Get** comece a execução.

   ![Quebrando dentro do método Get](build-restful-apis-with-aspnet-web-api/_static/image18.png "Quebrando dentro do método Get")

   *Quebrando dentro do método Get*
17. Pressione **F5** para continuar.
18. Volte para o Internet Explorer se ele ainda não estiver em foco. Observe a janela de captura de rede.

    ![Exibição de rede no Internet Explorer mostrando os resultados da chamada à API Web](build-restful-apis-with-aspnet-web-api/_static/image19.png "Exibição de rede no Internet Explorer mostrando os resultados da chamada à API Web")

    *Exibição de rede no Internet Explorer mostrando os resultados da chamada à API Web*
19. Clique no botão **ir para exibição detalhada** .
20. Clique na guia **corpo da resposta** . Observe a saída JSON da chamada à API e como ela representa os dois contatos recuperados pela camada de serviço.

    ![Exibindo a saída JSON da API Web na janela ferramentas de desenvolvedor](build-restful-apis-with-aspnet-web-api/_static/image20.png "Exibindo a saída JSON da API Web na janela ferramentas de desenvolvedor")

    *Exibindo a saída JSON da API Web na janela ferramentas de desenvolvedor*

<a id="Exercise2"></a>

<a id="Exercise_2_Create_a_ReadWrite_Web_API"></a>
### <a name="exercise-2-create-a-readwrite-web-api"></a>Exercício 2: criar uma API da Web de leitura/gravação

Neste exercício, você implementará os métodos POST e PUT para o Gerenciador de contatos para habilitá-lo com recursos de edição de dados.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Opening_the_Web_API_Project"></a>
#### <a name="task-1---opening-the-web-api-project"></a>Tarefa 1-abrindo o projeto de API Web

Nesta tarefa, você irá se preparar para aprimorar o projeto de API Web criado no exercício 1 para que ele possa aceitar a entrada do usuário.

1. Execute o **Visual Studio 2012 Express para Web**, para fazer isso, vá para **iniciar** e digite **vs Express para Web** pressione **Enter**.
2. Abra a solução **inicial** localizada na **origem/Ex02-ReadWriteWebAPI/início/** pasta. Caso contrário, você pode continuar usando a solução **final** obtida concluindo o exercício anterior.

   1. Se você tiver aberto a solução **inicial** fornecida, será necessário baixar alguns pacotes NuGet ausentes antes de continuar. Para fazer isso, clique no menu **projeto** e selecione **gerenciar pacotes NuGet**.
   2. Na caixa de diálogo **gerenciar pacotes NuGet** , clique em **restaurar** para baixar os pacotes ausentes.
   3. Por fim, Compile a solução clicando em **criar**  |  **solução de compilação**.

      > [!NOTE]
      > Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto. Com o NuGet Power Tools, especificando as versões do pacote no arquivo de Packages.config, você poderá baixar todas as bibliotecas necessárias na primeira vez em que executar o projeto. É por isso que você precisará executar essas etapas depois de abrir uma solução existente deste laboratório.
3. Abra o arquivo **Services/ContactRepository. cs** .

<a id="Ex2Task2"></a>

<a id="Task_2_-_Adding_Data-Persistence_Features_to_the_Contact_Repository_Implementation"></a>
#### <a name="task-2---adding-data-persistence-features-to-the-contact-repository-implementation"></a>Tarefa 2-adicionando recursos de Data-Persistence à implementação do repositório de contatos

Nesta tarefa, você aumentará a classe ContactRepository do projeto de API Web criado no exercício 1 para que ele possa persistir e aceitar as novas instâncias de contato e entrada do usuário.

1. Adicione a seguinte constante à classe **ContactRepository** para representar o nome do nome da chave do item de cache do servidor Web posteriormente neste exercício.

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample9.cs)]
2. Adicione um construtor ao **ContactRepository** que contém o código a seguir.

    (Trecho de código- *Ex02-Lab de API Web-Construtor de repositório de contatos*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample10.cs)]
3. Modifique o código para o método **GetAllContacts** , conforme demonstrado abaixo.

    (Trecho de código- *laboratório da API Web-Ex02-obter todos os contatos*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample11.cs)]

    > [!NOTE]
    > Este exemplo é para fins de demonstração e usará o cache do servidor Web como meio de armazenamento, de modo que os valores estarão disponíveis para vários clientes simultaneamente, em vez de usar um mecanismo de armazenamento de sessão ou um tempo de vida de armazenamento de solicitação. É possível usar Entity Framework, o armazenamento XML ou qualquer outra variedade no lugar do cache do servidor Web.
4. Implemente um novo método chamado **SaveContact** para a classe **ContactRepository** para fazer o trabalho de salvar um contato. O método **SaveContact** deve usar um único parâmetro de **contato** e retornar um valor booliano indicando êxito ou falha.

    (Trecho de código- *laboratório da API Web-Ex02-implementando o método SaveContact*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample12.cs)]

<a id="Exercise3"></a>

<a id="Exercise_3_Consume_the_Web_API_from_an_HTML_Client"></a>
### <a name="exercise-3-consume-the-web-api-from-an-html-client"></a>Exercício 3: consumir a API Web de um cliente HTML

Neste exercício, você criará um cliente HTML para chamar a API da Web. Esse cliente facilitará a troca de dados com a API Web usando JavaScript e exibirá os resultados em um navegador da Web usando marcação HTML.

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Displaying_Contacts"></a>
#### <a name="task-1---modifying-the-index-view-to-provide-a-gui-for-displaying-contacts"></a>Tarefa 1-modificando a exibição de índice para fornecer uma GUI para exibir contatos

Nesta tarefa, você modificará a exibição de índice padrão do aplicativo Web para dar suporte ao requisito de exibir a lista de contatos existentes em um navegador HTML.

1. Abra o **Visual Studio 2012 Express para Web** se ele ainda não estiver aberto.
2. Abra a solução **inicial** localizada na **origem/Ex03-ConsumingWebAPI/início/** pasta. Caso contrário, você pode continuar usando a solução **final** obtida concluindo o exercício anterior.

   1. Se você tiver aberto a solução **inicial** fornecida, será necessário baixar alguns pacotes NuGet ausentes antes de continuar. Para fazer isso, clique no menu **projeto** e selecione **gerenciar pacotes NuGet**.
   2. Na caixa de diálogo **gerenciar pacotes NuGet** , clique em **restaurar** para baixar os pacotes ausentes.
   3. Por fim, Compile a solução clicando em **criar**  |  **solução de compilação**.

      > [!NOTE]
      > Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto. Com o NuGet Power Tools, especificando as versões do pacote no arquivo de Packages.config, você poderá baixar todas as bibliotecas necessárias na primeira vez em que executar o projeto. É por isso que você precisará executar essas etapas depois de abrir uma solução existente deste laboratório.
3. Abra o arquivo **index. cshtml** localizado em **exibições/pasta base** .
4. Substitua o código HTML dentro do elemento div por um **corpo** de ID para que ele se pareça com o código a seguir.

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample13.html)]
5. Adicione o seguinte código JavaScript na parte inferior do arquivo para executar a solicitação HTTP para a API da Web.

    [!code-cshtml[Main](build-restful-apis-with-aspnet-web-api/samples/sample14.cshtml)]
6. Abra o arquivo **ContactController.cs** se ele ainda não estiver aberto.
7. Coloque um ponto de interrupção no método **Get** da classe **ContactController** .

    ![Colocando um ponto de interrupção no método Get do controlador de API](build-restful-apis-with-aspnet-web-api/_static/image21.png "Colocando um ponto de interrupção no método Get do controlador de API")

    *Colocando um ponto de interrupção no método Get do controlador de API*
8. Pressione **F5** para executar o projeto. O navegador carregará o documento HTML.

    > [!NOTE]
    > Certifique-se de que você está navegando para a URL raiz do seu aplicativo.
9. Depois que a página for carregada e o JavaScript for executado, o ponto de interrupção será atingido e a execução do código será pausada no controlador.

    ![Depuração nas chamadas à API Web usando VS Express para Web](build-restful-apis-with-aspnet-web-api/_static/image22.png "Depuração nas chamadas à API Web usando VS Express para Web")

    *Depuração na chamada à API Web usando o Visual Studio 2012 Express para Web*
10. Remova o ponto de interrupção e pressione **F5** ou o botão **continuar** da barra de ferramentas de depuração para continuar carregando o modo de exibição no navegador. Depois que a chamada da API Web for concluída, você deverá ver os contatos retornados da chamada da API Web exibida como itens de lista no navegador.

    ![Resultados da chamada à API exibida no navegador como itens de lista](build-restful-apis-with-aspnet-web-api/_static/image23.png "Resultados da chamada à API exibida no navegador como itens de lista")

    *Resultados da chamada à API exibida no navegador como itens de lista*
11. Interrompa a depuração.

<a id="Ex3Task2"></a>

<a id="Task_2_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Creating_Contacts"></a>
#### <a name="task-2---modifying-the-index-view-to-provide-a-gui-for-creating-contacts"></a>Tarefa 2-modificando a exibição de índice para fornecer uma GUI para criar contatos

Nesta tarefa, você continuará a modificar a exibição de índice do aplicativo MVC. Um formulário será adicionado à página HTML que capturará a entrada do usuário e o enviará para a API da Web para criar um novo contato, e um novo método do controlador da API Web será criado para coletar a data da GUI.

1. Abra o arquivo **ContactController.cs** .
2. Adicione um novo método à classe de controlador chamada **post** , conforme mostrado no código a seguir.

    (Trecho de código- *laboratório da API Web-método Ex03-post*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample15.cs)]
3. Abra o arquivo **index. cshtml** no Visual Studio se ele ainda não estiver aberto.
4. Adicione o código HTML abaixo ao arquivo logo após a lista não ordenada que você adicionou na tarefa anterior.

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample16.html)]
5. No elemento script na parte inferior do documento, adicione o seguinte código realçado para manipular eventos de clique de botão, que irão postar os dados para a API Web usando uma chamada HTTP POST.

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample17.html)]
6. Em **ContactController.cs**, coloque um ponto de interrupção no método **post** .
7. Pressione **F5** para executar o aplicativo no navegador.
8. Depois que a página for carregada no navegador, digite um novo nome e ID de contato e clique no botão **salvar** .

    ![O documento HTML do cliente carregado no navegador](build-restful-apis-with-aspnet-web-api/_static/image24.png "O documento HTML do cliente carregado no navegador")

    *O documento HTML do cliente carregado no navegador*
9. Quando a janela do depurador for interrompida no método **post** , dê uma olhada nas propriedades do parâmetro **Contact** . Os valores devem corresponder aos dados inseridos no formulário.

    ![O objeto de contato que está sendo enviado para a API Web do cliente](build-restful-apis-with-aspnet-web-api/_static/image25.png "O objeto de contato que está sendo enviado para a API Web do cliente")

    *O objeto de contato que está sendo enviado para a API Web do cliente*
10. Percorra o método no depurador até que a variável de **resposta** tenha sido criada. Após a inspeção na janela **locais** no depurador, você verá que todas as propriedades foram definidas.

   ![A resposta após a criação no depurador](build-restful-apis-with-aspnet-web-api/_static/image26.png "A resposta após a criação no depurador")

   *A resposta após a criação no depurador*
11. Se você pressionar **F5** ou clicar em **continuar** no depurador, a solicitação será concluída. Depois de voltar para o navegador, o novo contato foi adicionado à lista de contatos armazenados pela implementação do **ContactRepository** .

   ![O navegador reflete a criação bem-sucedida da nova instância de contato](build-restful-apis-with-aspnet-web-api/_static/image27.png "O navegador reflete a criação bem-sucedida da nova instância de contato")

   *O navegador reflete a criação bem-sucedida da nova instância de contato*

> [!NOTE]
> Além disso, você pode implantar esse aplicativo no Azure após o [Apêndice C: publicando um aplicativo ASP.NET MVC 4 usando implantação da Web](#AppendixC).

---

<a id="Summary"></a>
## <a name="summary"></a>Resumo

Este laboratório apresentou a você o novo ASP.NET Web API Framework e a implementação de APIs Web RESTful usando a estrutura. A partir daqui, você pode criar um novo repositório que facilite a persistência de dados usando qualquer número de mecanismos e conecte esse serviço em vez do simples fornecido como um exemplo neste laboratório. A API Web dá suporte a vários recursos adicionais, como a habilitação da comunicação de clientes não HTML escritos em qualquer linguagem compatível com HTTP e JSON ou XML. A capacidade de hospedar uma API da Web fora de um aplicativo Web típico também é possível, bem como a capacidade de criar seus próprios formatos de serialização.

O site da ASP.NET tem uma área dedicada à estrutura de ASP.NET Web API em [[https://asp.net/web-api](https://asp.net/web-api)](https://asp.net/web-api) . Este site continuará a fornecer informações mais recentes, amostras e notícias relacionadas à API da Web, portanto, verifique com frequência se você gostaria de se aprofundar na arte de criar APIs Web personalizadas disponíveis para praticamente qualquer estrutura de dispositivo ou de desenvolvimento.

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a>Apêndice A: usando trechos de código

Com trechos de código, você tem todo o código de que precisa ao seu alcance. O documento de laboratório informará exatamente quando você pode usá-los, conforme mostrado na figura a seguir.

![Usando trechos de código do Visual Studio para inserir código em seu projeto](build-restful-apis-with-aspnet-web-api/_static/image28.png "Usando trechos de código do Visual Studio para inserir código em seu projeto")

*Usando trechos de código do Visual Studio para inserir código em seu projeto*

<a id="CodeSnippetUsingKeyBoard"></a>

<a id="To_add_a_code_snippet_using_the_keyboard_C_only"></a>
### <a name="to-add-a-code-snippet-using-the-keyboard-c-only"></a>Para adicionar um trecho de código usando o teclado (C# somente)

1. Coloque o cursor onde você deseja inserir o código.
2. Comece digitando o nome do trecho de código (sem espaços ou hifens).
3. Observe como o IntelliSense exibe nomes de trechos de código correspondentes.
4. Selecione o trecho correto (ou continue digitando até que o nome do trecho inteiro seja selecionado).
5. Pressione a tecla TAB duas vezes para inserir o trecho de código no local do cursor.

    ![Comece a digitar o nome do trecho](build-restful-apis-with-aspnet-web-api/_static/image29.png "Comece a digitar o nome do trecho")

    *Comece a digitar o nome do trecho*

    ![Pressione Tab para selecionar o trecho realçado](build-restful-apis-with-aspnet-web-api/_static/image30.png "Pressione Tab para selecionar o trecho realçado")

    *Pressione Tab para selecionar o trecho realçado*

    ![Pressione Tab novamente e o trecho será expandido](build-restful-apis-with-aspnet-web-api/_static/image31.png "Pressione Tab novamente e o trecho será expandido")

    *Pressione Tab novamente e o trecho será expandido*

<a id="CodeSnippetUsingMouse"></a>

<a id="To_add_a_code_snippet_using_the_mouse_C_Visual_Basic_and_XML"></a>
### <a name="to-add-a-code-snippet-using-the-mouse-c-visual-basic-and-xml"></a>Para adicionar um trecho de código usando o mouse (C#, Visual Basic e XML)

1. Clique com o botão direito do mouse no local em que você deseja inserir o trecho de código.
2. Selecione **Inserir trecho** seguido por **meus trechos de código**.
3. Selecione o trecho relevante na lista clicando nele.

    ![Clique com o botão direito do mouse em onde você deseja inserir o trecho de código e selecione Inserir trecho](build-restful-apis-with-aspnet-web-api/_static/image32.png "Clique com o botão direito do mouse em onde você deseja inserir o trecho de código e selecione Inserir trecho")

    *Clique com o botão direito do mouse em onde você deseja inserir o trecho de código e selecione Inserir trecho*

    ![Selecione o trecho relevante na lista clicando nele](build-restful-apis-with-aspnet-web-api/_static/image33.png "Selecione o trecho relevante na lista clicando nele")

    *Selecione o trecho relevante na lista clicando nele*

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a>Apêndice B: Instalando o Visual Studio Express 2012 para Web

Você pode instalar o **Microsoft Visual Studio Express 2012 para Web** ou &quot; outra &quot; versão Express usando o **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. As instruções a seguir orientam você pelas etapas necessárias para instalar o *Visual Studio Express 2012 para Web* usando *Microsoft Web Platform Installer*.

1. Vá para [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169) . Como alternativa, se você já tiver instalado o Web Platform Installer, poderá abri-lo e procurar o produto &quot; <em>Visual Studio Express 2012 para Web com o SDK do Azure</em> &quot; .
2. Clique em **instalar agora**. Se você não tiver **Web Platform Installer** você será redirecionado para baixar e instalá-lo primeiro.
3. Quando **Web Platform Installer** estiver aberto, clique em **instalar** para iniciar a instalação.

    ![Instalar Visual Studio Express](build-restful-apis-with-aspnet-web-api/_static/image34.png "Instalar Visual Studio Express")

    *Instalar Visual Studio Express*
4. Leia todos os termos e licenças de produtos e clique em **aceito** para continuar.

    ![Aceitando os termos de licença](build-restful-apis-with-aspnet-web-api/_static/image35.png)

    *Aceitando os termos de licença*
5. Aguarde até que o processo de download e instalação seja concluído.

    ![Progresso da instalação](build-restful-apis-with-aspnet-web-api/_static/image36.png)

    *Progresso da instalação*
6. Quando a instalação for concluída, clique em **concluir**.

    ![Instalação concluída](build-restful-apis-with-aspnet-web-api/_static/image37.png)

    *Instalação concluída*
7. Clique em **sair** para fechar Web Platform Installer.
8. Para abrir o Visual Studio Express para Web, vá para a tela **Iniciar** e comece a escrever &quot; **vs Express** e &quot; , em seguida, clique no bloco **vs Express para Web** .

    ![Bloco VS Express para Web](build-restful-apis-with-aspnet-web-api/_static/image38.png)

    *Bloco VS Express para Web*

<a id="AppendixC"></a>

<a id="Appendix_C_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-c-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Apêndice C: publicando um aplicativo ASP.NET MVC 4 usando Implantação da Web

Este apêndice mostrará como criar um novo site no portal do Azure e publicar o aplicativo obtido seguindo o laboratório, aproveitando o recurso de publicação de Implantação da Web fornecido pelo Azure.

<a id="ApxCTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a>Tarefa 1-Criando um novo site no portal do Azure

1. Vá para o [portal de gerenciamento do Azure](https://manage.windowsazure.com/) e entre usando as credenciais da Microsoft associadas à sua assinatura.

    > [!NOTE]
    > Com o Azure, você pode hospedar 10 sites da Web de ASP.NET gratuitamente e, em seguida, dimensionar conforme o tráfego cresce. Você pode se inscrever [aqui](https://aka.ms/aspnet-hol-azure).

    ![Fazer logon no Windows portal do Azure](build-restful-apis-with-aspnet-web-api/_static/image39.png "Fazer logon no Windows portal do Azure")

    *Fazer logon no portal*
2. Clique em **novo** na barra de comandos.

    ![Criando um novo site](build-restful-apis-with-aspnet-web-api/_static/image40.png "Criando um novo site")

    *Criando um novo site*
3. Clique em **computação**  |  **Web site**. Em seguida, selecione a opção **criação rápida** . Forneça uma URL disponível para o novo site e clique em **criar site**.

    > [!NOTE]
    > O Azure é o host para um aplicativo Web em execução na nuvem que você pode controlar e gerenciar. A opção criação rápida permite que você implante um aplicativo Web completo no Azure de fora do Portal. Ele não inclui etapas para configurar um banco de dados.

    ![Criando um novo site usando a criação rápida](build-restful-apis-with-aspnet-web-api/_static/image41.png "Criando um novo site usando a criação rápida")

    *Criando um novo site usando a criação rápida*
4. Aguarde até que o novo **site** seja criado.
5. Depois que o site for criado, clique no link sob a coluna **URL** . Verifique se o novo site está funcionando.

    ![Navegando até o novo site](build-restful-apis-with-aspnet-web-api/_static/image42.png "Navegando até o novo site")

    *Navegando até o novo site*

    ![Site em execução](build-restful-apis-with-aspnet-web-api/_static/image43.png "Site em execução")

    *Site em execução*
6. Volte para o portal e clique no nome do site na coluna **nome** para exibir as páginas de gerenciamento.

    ![Abrindo as páginas de gerenciamento de site](build-restful-apis-with-aspnet-web-api/_static/image44.png "Abrindo as páginas de gerenciamento de site")

    *Abrindo as páginas de gerenciamento de site*
7. Na página **painel** , na seção **visão rápida** , clique no link **baixar perfil de publicação** .

    > [!NOTE]
    > O *perfil de publicação* contém todas as informações necessárias para publicar um aplicativo Web em um Azure para cada método de publicação habilitado. O perfil de publicação contém as URLs, as credenciais de usuário e as cadeias de conexão de banco de dados necessárias para conectar-se e autenticar cada um dos pontos de extremidade para os quais um método de publicação é habilitado. **O Microsoft WebMatrix 2**, **Microsoft Visual Studio Express para Web** e **Microsoft Visual Studio 2012** dão suporte à leitura de perfis de publicação para automatizar a configuração desses programas para a publicação de aplicativos Web no Azure.

    ![Baixando o perfil de publicação do site](build-restful-apis-with-aspnet-web-api/_static/image45.png "Baixando o perfil de publicação do site")

    *Baixando o perfil de publicação do site*
8. Baixe o arquivo de perfil de publicação em um local conhecido. Neste exercício, você verá como usar esse arquivo para publicar um aplicativo Web no Azure por meio do Visual Studio.

    ![Salvando o arquivo de perfil de publicação](build-restful-apis-with-aspnet-web-api/_static/image46.png "Salvando o perfil de publicação")

    *Salvando o arquivo de perfil de publicação*

<a id="ApxCTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Tarefa 2-Configurando o servidor de banco de dados

Se seu aplicativo utiliza bancos de dados SQL Server, você precisará criar um servidor de banco de dados SQL. Se você quiser implantar um aplicativo simples que não usa SQL Server você pode ignorar essa tarefa.

1. Você precisará de um servidor do banco de dados SQL para armazenar o banco de dados do aplicativo. Você pode exibir os servidores do banco de dados SQL de sua assinatura no portal de gerenciamento do Azure no  |    |  **painel do servidor** de servidores de bancos de dados SQL. Se você não tiver um servidor criado, poderá criar um usando o botão **Adicionar** na barra de comandos. Anote o nome do **servidor e a URL, o nome de logon e a senha do administrador**, pois você irá usá-los nas próximas tarefas. Não crie o banco de dados ainda, pois ele será criado em um estágio posterior.

    ![Painel do servidor do banco de dados SQL](build-restful-apis-with-aspnet-web-api/_static/image47.png "Painel do servidor do banco de dados SQL")

    *Painel do servidor do banco de dados SQL*
2. Na próxima tarefa, você testará a conexão de banco de dados do Visual Studio, por esse motivo, será necessário incluir o endereço IP local na lista de **endereços IP permitidos** do servidor. Para fazer isso, clique em **Configurar**, selecione o endereço IP do **endereço IP do cliente atual** e cole-o nas caixas de texto endereço IP **inicial** e **endereço IP final** e clique no ![ botão Adicionar-cliente-IP-endereço-OK-botão ](build-restful-apis-with-aspnet-web-api/_static/image48.png) .

    ![Adicionando endereço IP do cliente](build-restful-apis-with-aspnet-web-api/_static/image49.png)

    *Adicionando endereço IP do cliente*
3. Depois que o **endereço IP do cliente** for adicionado à lista endereços IP permitidos, clique em **salvar** para confirmar as alterações.

    ![Confirmar alterações](build-restful-apis-with-aspnet-web-api/_static/image50.png)

    *Confirmar alterações*

<a id="ApxCTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Tarefa 3-publicando um aplicativo ASP.NET MVC 4 usando Implantação da Web

1. Volte para a solução ASP.NET MVC 4. Na **Gerenciador de soluções**, clique com o botão direito do mouse no projeto de site e selecione **publicar**.

    ![Publicando o aplicativo](build-restful-apis-with-aspnet-web-api/_static/image51.png "Publicando o aplicativo")

    *Publicando o site*
2. Importe o perfil de publicação salvo na primeira tarefa.

    ![Importando o perfil de publicação](build-restful-apis-with-aspnet-web-api/_static/image52.png "Importando o perfil de publicação")

    *Importando perfil de publicação*
3. Clique em **validar conexão**. Depois que a validação for concluída, clique em **Avançar**.

    > [!NOTE]
    > A validação será concluída quando você vir uma marca de seleção verde ao lado do botão Validar conexão.

    ![Validando conexão](build-restful-apis-with-aspnet-web-api/_static/image53.png "Validando conexão")

    *Validando conexão*
4. Na página **configurações** , na seção **bancos** de dados, clique no botão ao lado da caixa de texto da sua conexão de banco (ou seja, **DefaultConnection**).

    ![Configuração de implantação da Web](build-restful-apis-with-aspnet-web-api/_static/image54.png "Configuração de implantação da Web")

    *Configuração de implantação da Web*
5. Configure a conexão de banco de dados da seguinte maneira:

   - No **nome do servidor** , digite a URL do servidor do banco de dados SQL usando o prefixo *TCP:* .
   - Em **nome de usuário** , digite o nome de logon do administrador do servidor.
   - Em **senha** , digite a senha de logon do administrador do servidor.
   - Digite um novo nome de banco de dados, por exemplo: *MVC4SampleDB*.

     ![Configurando a cadeia de conexão de destino](build-restful-apis-with-aspnet-web-api/_static/image55.png "Configurando a cadeia de conexão de destino")

     *Configurando a cadeia de conexão de destino*
6. Em seguida, clique em **OK**. Quando for solicitado a criar o banco de dados, clique em **Sim**.

    ![Criando o banco de dados](build-restful-apis-with-aspnet-web-api/_static/image56.png "Criando a cadeia de caracteres do banco de dados")

    *Criando o banco de dados*
7. A cadeia de conexão que você usará para se conectar ao banco de dados SQL no Windows Azure é mostrada na caixa de texto conexão padrão. Em seguida, clique em **Próximo**.

    ![Cadeia de conexão apontando para o banco de dados SQL](build-restful-apis-with-aspnet-web-api/_static/image57.png "Cadeia de conexão apontando para o banco de dados SQL")

    *Cadeia de conexão apontando para o banco de dados SQL*
8. Na página **Visualização** , clique em **publicar**.

    ![Publicando o aplicativo Web](build-restful-apis-with-aspnet-web-api/_static/image58.png "Publicando o aplicativo Web")

    *Publicando o aplicativo Web*
9. Quando o processo de publicação for concluído, o navegador padrão abrirá o site publicado.

    ![Aplicativo publicado no Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image59.png "Aplicativo publicado no Windows Azure")

    *Aplicativo publicado no Azure*
