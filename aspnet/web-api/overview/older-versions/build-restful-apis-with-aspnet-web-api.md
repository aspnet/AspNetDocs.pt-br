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
# <a name="build-restful-apis-with-aspnet-web-api"></a><span data-ttu-id="5d944-103">Criar APIs RESTful com ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="5d944-103">Build RESTful APIs with ASP.NET Web API</span></span>

<span data-ttu-id="5d944-104">por [equipe de acampamentos da Web](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="5d944-104">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> <span data-ttu-id="5d944-105">Laboratório prático: Use a API da Web no ASP.NET 4. x para criar uma API REST simples para um aplicativo do Gerenciador de contatos.</span><span class="sxs-lookup"><span data-stu-id="5d944-105">Hands on lab: Use Web API in ASP.NET 4.x to build a simple REST API for a contact manager application.</span></span> <span data-ttu-id="5d944-106">Você também criará um cliente para consumir a API.</span><span class="sxs-lookup"><span data-stu-id="5d944-106">You will also build a client to consume the API.</span></span>

<span data-ttu-id="5d944-107">Nos últimos anos, ficou claro que HTTP não serve apenas para servir páginas HTML.</span><span class="sxs-lookup"><span data-stu-id="5d944-107">In recent years, it has become clear that HTTP is not just for serving up HTML pages.</span></span> <span data-ttu-id="5d944-108">Também é uma plataforma poderosa para a criação de APIs da Web, usando alguns verbos (GET, POST e assim por diante), além de alguns conceitos simples, como *URIs* e *cabeçalhos*.</span><span class="sxs-lookup"><span data-stu-id="5d944-108">It is also a powerful platform for building Web APIs, using a handful of verbs (GET, POST, and so forth) plus a few simple concepts such as *URIs* and *headers*.</span></span> <span data-ttu-id="5d944-109">ASP.NET Web API é um conjunto de componentes que simplificam a programação HTTP.</span><span class="sxs-lookup"><span data-stu-id="5d944-109">ASP.NET Web API is a set of components that simplify HTTP programming.</span></span> <span data-ttu-id="5d944-110">Como ele é criado sobre o tempo de execução MVC do ASP.NET, a API da Web manipula automaticamente os detalhes de transporte de nível inferior do HTTP.</span><span class="sxs-lookup"><span data-stu-id="5d944-110">Because it is built on top of the ASP.NET MVC runtime, Web API automatically handles the low-level transport details of HTTP.</span></span> <span data-ttu-id="5d944-111">Ao mesmo tempo, a API Web expõe naturalmente o modelo de programação HTTP.</span><span class="sxs-lookup"><span data-stu-id="5d944-111">At the same time, Web API naturally exposes the HTTP programming model.</span></span> <span data-ttu-id="5d944-112">Na verdade, uma meta da API Web é *não* abstrair a realidade do http.</span><span class="sxs-lookup"><span data-stu-id="5d944-112">In fact, one goal of Web API is to *not* abstract away the reality of HTTP.</span></span> <span data-ttu-id="5d944-113">Como resultado, a API da Web é flexível e fácil de estender.</span><span class="sxs-lookup"><span data-stu-id="5d944-113">As a result, Web API is both flexible and easy to extend.</span></span>  <span data-ttu-id="5d944-114">O estilo de arquitetura REST provou ser uma maneira eficaz de aproveitar HTTP, embora, certamente, não seja a única abordagem válida para HTTP.</span><span class="sxs-lookup"><span data-stu-id="5d944-114">The REST architectural style has proven to be an effective way to leverage HTTP - although it is certainly not the only valid approach to HTTP.</span></span> <span data-ttu-id="5d944-115">O Gerenciador de contatos irá expor o RESTful para listagem, adição e remoção de contatos, entre outros.</span><span class="sxs-lookup"><span data-stu-id="5d944-115">The contact manager will expose the RESTful for listing, adding and removing contacts, among others.</span></span> 

<span data-ttu-id="5d944-116">Este laboratório requer uma compreensão básica do HTTP, do REST e pressupõe que você tenha um conhecimento funcional básico de HTML, JavaScript e jQuery.</span><span class="sxs-lookup"><span data-stu-id="5d944-116">This lab requires a basic understanding of HTTP, REST, and assumes you have a basic working knowledge of HTML, JavaScript, and jQuery.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="5d944-117">O site da ASP.NET tem uma área dedicada à estrutura de ASP.NET Web API em [https://asp.net/web-api](https://asp.net/web-api) .</span><span class="sxs-lookup"><span data-stu-id="5d944-117">The ASP.NET Web site has an area dedicated to the ASP.NET Web API framework at [https://asp.net/web-api](https://asp.net/web-api).</span></span> <span data-ttu-id="5d944-118">Este site continuará a fornecer informações mais recentes, amostras e notícias relacionadas à API da Web, portanto, verifique com frequência se você gostaria de se aprofundar na arte de criar APIs Web personalizadas disponíveis para praticamente qualquer estrutura de dispositivo ou de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="5d944-118">This site will continue to provide late-breaking information, samples, and news related to Web API, so check it frequently if you'd like to delve deeper into the art of creating custom Web APIs available to virtually any device or development framework.</span></span>
> > 
> > <span data-ttu-id="5d944-119">ASP.NET Web API, semelhante ao ASP.NET MVC 4, tem grande flexibilidade em termos de separar a camada de serviço dos controladores, permitindo que você use várias das estruturas de injeção de dependência disponíveis razoavelmente fáceis.</span><span class="sxs-lookup"><span data-stu-id="5d944-119">ASP.NET Web API, similar to ASP.NET MVC 4, has great flexibility in terms of separating the service layer from the controllers allowing you to use several of the available Dependency Injection frameworks fairly easy.</span></span> 
> 
> 
> <span data-ttu-id="5d944-120">Todos os códigos de exemplo e trechos de código estão incluídos no Web acampamentos Training Kit, disponível em [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409) .</span><span class="sxs-lookup"><span data-stu-id="5d944-120">All sample code and snippets are included in the Web Camps Training Kit, available at [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="5d944-121">Objetivos</span><span class="sxs-lookup"><span data-stu-id="5d944-121">Objectives</span></span>

<span data-ttu-id="5d944-122">Neste laboratório prático, você aprenderá a:</span><span class="sxs-lookup"><span data-stu-id="5d944-122">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="5d944-123">Implementar uma API Web RESTful</span><span class="sxs-lookup"><span data-stu-id="5d944-123">Implement a RESTful Web API</span></span>
- <span data-ttu-id="5d944-124">Chamar a API de um cliente HTML</span><span class="sxs-lookup"><span data-stu-id="5d944-124">Call the API from an HTML client</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="5d944-125">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="5d944-125">Prerequisites</span></span>

<span data-ttu-id="5d944-126">O seguinte é necessário para concluir este laboratório prático:</span><span class="sxs-lookup"><span data-stu-id="5d944-126">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="5d944-127">[Microsoft Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) ou superior (leia o [Apêndice B](#AppendixB) para obter instruções sobre como instalá-lo).</span><span class="sxs-lookup"><span data-stu-id="5d944-127">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix B](#AppendixB) for instructions on how to install it).</span></span>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="5d944-128">Instalação</span><span class="sxs-lookup"><span data-stu-id="5d944-128">Setup</span></span>

<span data-ttu-id="5d944-129">**Instalando trechos de código**</span><span class="sxs-lookup"><span data-stu-id="5d944-129">**Installing Code Snippets**</span></span>

<span data-ttu-id="5d944-130">Para sua conveniência, grande parte do código que você gerenciará ao longo deste laboratório está disponível como trechos de código do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5d944-130">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="5d944-131">Para instalar os trechos de código, execute o arquivo **.\Source\Setup\CodeSnippets.VSI** .</span><span class="sxs-lookup"><span data-stu-id="5d944-131">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="5d944-132">Se você não estiver familiarizado com os trechos de Visual Studio Code e quiser saber como usá-los, consulte o apêndice deste documento &quot; [Apêndice a: usando trechos de código](#AppendixA) &quot; .</span><span class="sxs-lookup"><span data-stu-id="5d944-132">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix A: Using Code Snippets](#AppendixA)&quot;.</span></span>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="5d944-133">Exercícios</span><span class="sxs-lookup"><span data-stu-id="5d944-133">Exercises</span></span>

<span data-ttu-id="5d944-134">Este laboratório prático inclui o seguinte exercício:</span><span class="sxs-lookup"><span data-stu-id="5d944-134">This hands-on lab includes the following exercise:</span></span>

1. [<span data-ttu-id="5d944-135">Exercício 1: criar uma API Web do Read-Only</span><span class="sxs-lookup"><span data-stu-id="5d944-135">Exercise 1: Create a Read-Only Web API</span></span>](#Exercise1)
2. [<span data-ttu-id="5d944-136">Exercício 2: criar uma API da Web de leitura/gravação</span><span class="sxs-lookup"><span data-stu-id="5d944-136">Exercise 2: Create a Read/Write Web API</span></span>](#Exercise2)
3. [<span data-ttu-id="5d944-137">Exercício 3: consumir a API Web de um cliente HTML</span><span class="sxs-lookup"><span data-stu-id="5d944-137">Exercise 3: Consume the Web API from an HTML Client</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="5d944-138">Cada exercício é acompanhado por uma pasta **final** que contém a solução resultante que você deve obter depois de concluir os exercícios.</span><span class="sxs-lookup"><span data-stu-id="5d944-138">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="5d944-139">Você pode usar essa solução como um guia se precisar de ajuda adicional para trabalhar com os exercícios.</span><span class="sxs-lookup"><span data-stu-id="5d944-139">You can use this solution as a guide if you need additional help working through the exercises.</span></span>

<span data-ttu-id="5d944-140">Tempo estimado para concluir este laboratório: **60 minutos**.</span><span class="sxs-lookup"><span data-stu-id="5d944-140">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Create_a_Read-Only_Web_API"></a>
### <a name="exercise-1-create-a-read-only-web-api"></a><span data-ttu-id="5d944-141">Exercício 1: criar uma API Web do Read-Only</span><span class="sxs-lookup"><span data-stu-id="5d944-141">Exercise 1: Create a Read-Only Web API</span></span>

<span data-ttu-id="5d944-142">Neste exercício, você vai implementar os métodos GET somente leitura para o Gerenciador de contatos.</span><span class="sxs-lookup"><span data-stu-id="5d944-142">In this exercise, you will implement the read-only GET methods for the contact manager.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_API_Project"></a>
#### <a name="task-1---creating-the-api-project"></a><span data-ttu-id="5d944-143">Tarefa 1-criando o projeto de API</span><span class="sxs-lookup"><span data-stu-id="5d944-143">Task 1 - Creating the API Project</span></span>

<span data-ttu-id="5d944-144">Nesta tarefa, você usará os novos modelos de projeto Web ASP.NET para criar um aplicativo Web da API Web.</span><span class="sxs-lookup"><span data-stu-id="5d944-144">In this task, you will use the new ASP.NET web project templates to create a Web API web application.</span></span>

1. <span data-ttu-id="5d944-145">Execute o **Visual Studio 2012 Express para Web**, para fazer isso, vá para **iniciar** e digite **vs Express para Web** pressione **Enter**.</span><span class="sxs-lookup"><span data-stu-id="5d944-145">Run **Visual Studio 2012 Express for Web**, to do this go to **Start** and type **VS Express for Web** then press **Enter**.</span></span>
2. <span data-ttu-id="5d944-146">No menu **arquivo** , selecione **novo projeto**.</span><span class="sxs-lookup"><span data-stu-id="5d944-146">From the **File** menu, select **New Project**.</span></span> <span data-ttu-id="5d944-147">Selecione o **Visual C# |** Tipo de projeto Web no modo de exibição de árvore do tipo de projeto e, em seguida, selecione o tipo de projeto de **aplicativo Web ASP.NET MVC 4** .</span><span class="sxs-lookup"><span data-stu-id="5d944-147">Select the **Visual C# | Web** project type from the project type tree view, then select the **ASP.NET MVC 4 Web Application** project type.</span></span> <span data-ttu-id="5d944-148">Defina o **nome** do projeto como *ContactManager* e o **nome da solução** como *begin* e clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="5d944-148">Set the project's **Name** to *ContactManager* and the **Solution name** to *Begin*, then click **OK**.</span></span>

    <span data-ttu-id="5d944-149">![Criando um novo projeto de aplicativo Web do ASP.NET MVC 4,0](build-restful-apis-with-aspnet-web-api/_static/image1.png "Criando um novo projeto de aplicativo Web do ASP.NET MVC 4,0")</span><span class="sxs-lookup"><span data-stu-id="5d944-149">![Creating a new ASP.NET MVC 4.0 Web Application Project](build-restful-apis-with-aspnet-web-api/_static/image1.png "Creating a new ASP.NET MVC 4.0 Web Application Project")</span></span>

    <span data-ttu-id="5d944-150">*Criando um novo projeto de aplicativo Web do ASP.NET MVC 4,0*</span><span class="sxs-lookup"><span data-stu-id="5d944-150">*Creating a new ASP.NET MVC 4.0 Web Application Project*</span></span>
3. <span data-ttu-id="5d944-151">Na caixa de diálogo tipo de projeto do ASP.NET MVC 4, selecione o tipo de projeto de **API Web** .</span><span class="sxs-lookup"><span data-stu-id="5d944-151">In the ASP.NET MVC 4 project type dialog, select the **Web API** project type.</span></span> <span data-ttu-id="5d944-152">Clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="5d944-152">Click **OK**.</span></span>

    <span data-ttu-id="5d944-153">![Especificando o tipo de projeto de API Web](build-restful-apis-with-aspnet-web-api/_static/image2.png "Especificando o tipo de projeto de API Web")</span><span class="sxs-lookup"><span data-stu-id="5d944-153">![Specifying the Web API project type](build-restful-apis-with-aspnet-web-api/_static/image2.png "Specifying the Web API project type")</span></span>

    <span data-ttu-id="5d944-154">*Especificando o tipo de projeto de API Web*</span><span class="sxs-lookup"><span data-stu-id="5d944-154">*Specifying the Web API project type*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_the_Contact_Manager_API_Controllers"></a>
#### <a name="task-2---creating-the-contact-manager-api-controllers"></a><span data-ttu-id="5d944-155">Tarefa 2-Criando os controladores de API do Gerenciador de contatos</span><span class="sxs-lookup"><span data-stu-id="5d944-155">Task 2 - Creating the Contact Manager API Controllers</span></span>

<span data-ttu-id="5d944-156">Nesta tarefa, você criará as classes de controlador nas quais os métodos de API residirão.</span><span class="sxs-lookup"><span data-stu-id="5d944-156">In this task, you will create the controller classes in which API methods will reside.</span></span>

1. <span data-ttu-id="5d944-157">Exclua o arquivo chamado **ValuesController.cs** na pasta **controladores** do projeto.</span><span class="sxs-lookup"><span data-stu-id="5d944-157">Delete the file named **ValuesController.cs** within **Controllers** folder from the project.</span></span>
2. <span data-ttu-id="5d944-158">Clique com o botão direito do mouse na pasta **controladores** no projeto e selecione **Adicionar | Controlador** no menu de contexto.</span><span class="sxs-lookup"><span data-stu-id="5d944-158">Right-click the **Controllers** folder in the project and select **Add | Controller** from the context menu.</span></span>

    <span data-ttu-id="5d944-159">![Adicionando um novo controlador ao projeto](build-restful-apis-with-aspnet-web-api/_static/image3.png "Adicionando um novo controlador ao projeto")</span><span class="sxs-lookup"><span data-stu-id="5d944-159">![Adding a new controller to the project](build-restful-apis-with-aspnet-web-api/_static/image3.png "Adding a new controller to the project")</span></span>

    <span data-ttu-id="5d944-160">*Adicionando um novo controlador ao projeto*</span><span class="sxs-lookup"><span data-stu-id="5d944-160">*Adding a new controller to the project*</span></span>
3. <span data-ttu-id="5d944-161">Na caixa de diálogo **Adicionar controlador** exibida, selecione **controlador de API vazio** no menu modelo.</span><span class="sxs-lookup"><span data-stu-id="5d944-161">In the **Add Controller** dialog that appears, select **Empty API Controller** from the Template menu.</span></span> <span data-ttu-id="5d944-162">Nomeie a classe de controlador **ContactController**.</span><span class="sxs-lookup"><span data-stu-id="5d944-162">Name the controller class **ContactController**.</span></span> <span data-ttu-id="5d944-163">Em seguida, clique em **Adicionar.**</span><span class="sxs-lookup"><span data-stu-id="5d944-163">Then, click **Add.**</span></span>

    <span data-ttu-id="5d944-164">![Usando a caixa de diálogo Adicionar controlador para criar um novo controlador de API Web](build-restful-apis-with-aspnet-web-api/_static/image4.png "Usando a caixa de diálogo Adicionar controlador para criar um novo controlador de API Web")</span><span class="sxs-lookup"><span data-stu-id="5d944-164">![Using the Add Controller dialog to create a new Web API controller](build-restful-apis-with-aspnet-web-api/_static/image4.png "Using the Add Controller dialog to create a new Web API controller")</span></span>

    <span data-ttu-id="5d944-165">*Usando a caixa de diálogo Adicionar controlador para criar um novo controlador de API Web*</span><span class="sxs-lookup"><span data-stu-id="5d944-165">*Using the Add Controller dialog to create a new Web API controller*</span></span>
4. <span data-ttu-id="5d944-166">Adicione o código a seguir ao **ContactController**.</span><span class="sxs-lookup"><span data-stu-id="5d944-166">Add the following code to the **ContactController**.</span></span>

    <span data-ttu-id="5d944-167">(Trecho de código- *laboratório de API Web-Ex01-método de API Get*)</span><span class="sxs-lookup"><span data-stu-id="5d944-167">(Code Snippet - *Web API Lab - Ex01 - Get API Method*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample1.cs)]
5. <span data-ttu-id="5d944-168">Pressione **F5** para depurar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="5d944-168">Press **F5** to debug the application.</span></span> <span data-ttu-id="5d944-169">O home page padrão para um projeto de API Web deve aparecer.</span><span class="sxs-lookup"><span data-stu-id="5d944-169">The default home page for a Web API project should appear.</span></span>

    <span data-ttu-id="5d944-170">![O home page padrão de um aplicativo ASP.NET Web API](build-restful-apis-with-aspnet-web-api/_static/image5.png "O home page padrão de um aplicativo ASP.NET Web API")</span><span class="sxs-lookup"><span data-stu-id="5d944-170">![The default home page of an ASP.NET Web API application](build-restful-apis-with-aspnet-web-api/_static/image5.png "The default home page of an ASP.NET Web API application")</span></span>

    <span data-ttu-id="5d944-171">*O home page padrão de um aplicativo ASP.NET Web API*</span><span class="sxs-lookup"><span data-stu-id="5d944-171">*The default home page of an ASP.NET Web API application*</span></span>
6. <span data-ttu-id="5d944-172">Na janela do Internet Explorer, pressione a tecla **F12** para abrir a janela **ferramentas para desenvolvedores** .</span><span class="sxs-lookup"><span data-stu-id="5d944-172">In the Internet Explorer window, press the **F12** key to open the **Developer Tools** window.</span></span> <span data-ttu-id="5d944-173">Clique na guia **rede** e, em seguida, clique no botão **Iniciar captura** para começar a capturar o tráfego de rede na janela.</span><span class="sxs-lookup"><span data-stu-id="5d944-173">Click the **Network** tab, and then click the **Start Capturing** button to begin capturing network traffic into the window.</span></span>

    <span data-ttu-id="5d944-174">![Abrindo a guia rede e iniciando a captura de rede](build-restful-apis-with-aspnet-web-api/_static/image6.png "Abrindo a guia rede e iniciando a captura de rede")</span><span class="sxs-lookup"><span data-stu-id="5d944-174">![Opening the network tab and initiating network capture](build-restful-apis-with-aspnet-web-api/_static/image6.png "Opening the network tab and initiating network capture")</span></span>

    <span data-ttu-id="5d944-175">*Abrindo a guia rede e iniciando a captura de rede*</span><span class="sxs-lookup"><span data-stu-id="5d944-175">*Opening the network tab and initiating network capture*</span></span>
7. <span data-ttu-id="5d944-176">Acrescente a URL na barra de endereços do navegador com **/API/Contact** e pressione Enter.</span><span class="sxs-lookup"><span data-stu-id="5d944-176">Append the URL in the browser's address bar with **/api/contact** and press enter.</span></span> <span data-ttu-id="5d944-177">Os detalhes da transmissão aparecerão na janela captura de rede.</span><span class="sxs-lookup"><span data-stu-id="5d944-177">The transmission details will appear in the network capture window.</span></span> <span data-ttu-id="5d944-178">Observe que o tipo MIME da resposta é **Application/JSON**.</span><span class="sxs-lookup"><span data-stu-id="5d944-178">Note that the response's MIME type is **application/json**.</span></span> <span data-ttu-id="5d944-179">Isso demonstra como o formato de saída padrão é JSON.</span><span class="sxs-lookup"><span data-stu-id="5d944-179">This demonstrates how the default output format is JSON.</span></span>

    <span data-ttu-id="5d944-180">![Exibindo a saída da solicitação da API Web na exibição de rede](build-restful-apis-with-aspnet-web-api/_static/image7.png "Exibindo a saída da solicitação da API Web na exibição de rede")</span><span class="sxs-lookup"><span data-stu-id="5d944-180">![Viewing the output of the Web API request in the Network view](build-restful-apis-with-aspnet-web-api/_static/image7.png "Viewing the output of the Web API request in the Network view")</span></span>

    <span data-ttu-id="5d944-181">*Exibindo a saída da solicitação da API Web na exibição de rede*</span><span class="sxs-lookup"><span data-stu-id="5d944-181">*Viewing the output of the Web API request in the Network view*</span></span>

    > [!NOTE]
    > <span data-ttu-id="5d944-182">O comportamento padrão do Internet Explorer 10 neste momento será perguntar se o usuário deseja salvar ou abrir o fluxo resultante da chamada à API Web.</span><span class="sxs-lookup"><span data-stu-id="5d944-182">Internet Explorer 10's default behavior at this point will be to ask if the user would like to save or open the stream resulting from the Web API call.</span></span> <span data-ttu-id="5d944-183">A saída será um arquivo de texto que contém o resultado JSON da chamada de URL da API Web.</span><span class="sxs-lookup"><span data-stu-id="5d944-183">The output will be a text file containing the JSON result of the Web API URL call.</span></span> <span data-ttu-id="5d944-184">Não cancele a caixa de diálogo para poder assistir ao conteúdo da resposta por meio da janela de ferramentas de desenvolvedores.</span><span class="sxs-lookup"><span data-stu-id="5d944-184">Do not cancel the dialog in order to be able to watch the response's content through Developers Tool window.</span></span>
8. <span data-ttu-id="5d944-185">Clique no botão **ir para exibição detalhada** para ver mais detalhes sobre a resposta dessa chamada à API.</span><span class="sxs-lookup"><span data-stu-id="5d944-185">Click the **Go to detailed view** button to see more details about the response of this API call.</span></span>

    <span data-ttu-id="5d944-186">![Alternar para exibição detalhada](build-restful-apis-with-aspnet-web-api/_static/image8.png "Alternar para exibição de detalhes")</span><span class="sxs-lookup"><span data-stu-id="5d944-186">![Switch to Detailed View](build-restful-apis-with-aspnet-web-api/_static/image8.png "Switch to Details View")</span></span>

    <span data-ttu-id="5d944-187">*Alternar para exibição detalhada*</span><span class="sxs-lookup"><span data-stu-id="5d944-187">*Switch to Detailed View*</span></span>
9. <span data-ttu-id="5d944-188">Clique na guia **corpo da resposta** para exibir o texto real da resposta JSON.</span><span class="sxs-lookup"><span data-stu-id="5d944-188">Click the **Response body** tab to view the actual JSON response text.</span></span>

    <span data-ttu-id="5d944-189">![Exibindo o texto de saída JSON no monitor de rede](build-restful-apis-with-aspnet-web-api/_static/image9.png "Exibindo o texto de saída JSON no monitor de rede")</span><span class="sxs-lookup"><span data-stu-id="5d944-189">![Viewing the JSON output text in the network monitor](build-restful-apis-with-aspnet-web-api/_static/image9.png "Viewing the JSON output text in the network monitor")</span></span>

    <span data-ttu-id="5d944-190">*Exibindo o texto de saída JSON no monitor de rede*</span><span class="sxs-lookup"><span data-stu-id="5d944-190">*Viewing the JSON output text in the network monitor*</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Creating_the_Contact_Models_and_Augment_the_Contact_Controller"></a>
#### <a name="task-3---creating-the-contact-models-and-augment-the-contact-controller"></a><span data-ttu-id="5d944-191">Tarefa 3-criando os modelos de contato e aumentando o controlador de contato</span><span class="sxs-lookup"><span data-stu-id="5d944-191">Task 3 - Creating the Contact Models and Augment the Contact Controller</span></span>

<span data-ttu-id="5d944-192">Nesta tarefa, você criará as classes de controlador nas quais os métodos de API residirão.</span><span class="sxs-lookup"><span data-stu-id="5d944-192">In this task, you will create the controller classes in which API methods will reside.</span></span>

1. <span data-ttu-id="5d944-193">Clique com o botão direito do mouse na pasta **modelos** e selecione **Adicionar | Classe...** no menu de contexto.</span><span class="sxs-lookup"><span data-stu-id="5d944-193">Right-click the **Models** folder and select **Add | Class...** from the context menu.</span></span>

    <span data-ttu-id="5d944-194">![Adicionando um novo modelo ao aplicativo Web](build-restful-apis-with-aspnet-web-api/_static/image10.png "Adicionando um novo modelo ao aplicativo Web")</span><span class="sxs-lookup"><span data-stu-id="5d944-194">![Adding a new model to the web application](build-restful-apis-with-aspnet-web-api/_static/image10.png "Adding a new model to the web application")</span></span>

    <span data-ttu-id="5d944-195">*Adicionando um novo modelo ao aplicativo Web*</span><span class="sxs-lookup"><span data-stu-id="5d944-195">*Adding a new model to the web application*</span></span>
2. <span data-ttu-id="5d944-196">Na caixa de diálogo **Adicionar novo item** , nomeie o novo arquivo **Contact.cs** e clique em **Adicionar.**</span><span class="sxs-lookup"><span data-stu-id="5d944-196">In the **Add New Item** dialog, name the new file **Contact.cs** and click **Add.**</span></span>

    <span data-ttu-id="5d944-197">![Criando o novo arquivo de classe de contato](build-restful-apis-with-aspnet-web-api/_static/image11.png "Criando o novo arquivo de classe de contato")</span><span class="sxs-lookup"><span data-stu-id="5d944-197">![Creating the new Contact class file](build-restful-apis-with-aspnet-web-api/_static/image11.png "Creating the new Contact class file")</span></span>

    <span data-ttu-id="5d944-198">*Criando o novo arquivo de classe de contato*</span><span class="sxs-lookup"><span data-stu-id="5d944-198">*Creating the new Contact class file*</span></span>
3. <span data-ttu-id="5d944-199">Adicione o seguinte código realçado à classe **Contact** .</span><span class="sxs-lookup"><span data-stu-id="5d944-199">Add the following highlighted code to the **Contact** class.</span></span>

    <span data-ttu-id="5d944-200">(Trecho de código- *laboratório da API Web-Ex01-classe de contato*)</span><span class="sxs-lookup"><span data-stu-id="5d944-200">(Code Snippet - *Web API Lab - Ex01 - Contact Class*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample2.cs)]
4. <span data-ttu-id="5d944-201">Na classe **ContactController** , selecione a palavra **cadeia de caracteres** na definição de método do método **Get** e digite a palavra *contato*.</span><span class="sxs-lookup"><span data-stu-id="5d944-201">In the **ContactController** class, select the word **string** in method definition of the **Get** method, and type the word *Contact*.</span></span> <span data-ttu-id="5d944-202">Depois que a palavra for digitada, um indicador será exibido no início da palavra **contato**.</span><span class="sxs-lookup"><span data-stu-id="5d944-202">Once the word is typed in, an indicator will appear at the beginning of the word **Contact**.</span></span> <span data-ttu-id="5d944-203">Mantenha pressionada a tecla **Ctrl** e pressione a tecla period (.) ou clique no ícone usando o mouse para abrir a caixa de diálogo assistência no editor de códigos, para preencher automaticamente a diretiva **using** para o namespace Models.</span><span class="sxs-lookup"><span data-stu-id="5d944-203">Either hold down the **Ctrl** key and press the period (.) key or click the icon using your mouse to open up the assistance dialog in the code editor, to automatically fill in the **using** directive for the Models namespace.</span></span>

    ![Usando a assistência do IntelliSense para declarações de namespace](build-restful-apis-with-aspnet-web-api/_static/image12.png)

    <span data-ttu-id="5d944-205">*Usando a assistência do IntelliSense para declarações de namespace*</span><span class="sxs-lookup"><span data-stu-id="5d944-205">*Using Intellisense assistance for namespace declarations*</span></span>
5. <span data-ttu-id="5d944-206">Modifique o código para o método **Get** para que ele retorne uma matriz de instâncias de modelo de contato.</span><span class="sxs-lookup"><span data-stu-id="5d944-206">Modify the code for the **Get** method so that it returns an array of Contact model instances.</span></span>

    <span data-ttu-id="5d944-207">(Trecho de código- *laboratório da API Web-Ex01-retornando uma lista de contatos*)</span><span class="sxs-lookup"><span data-stu-id="5d944-207">(Code Snippet - *Web API Lab - Ex01 - Returning a list of contacts*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample3.cs)]
6. <span data-ttu-id="5d944-208">Pressione **F5** para depurar o aplicativo Web no navegador.</span><span class="sxs-lookup"><span data-stu-id="5d944-208">Press **F5** to debug the web application in the browser.</span></span> <span data-ttu-id="5d944-209">Para exibir as alterações feitas na saída de resposta da API, execute as etapas a seguir.</span><span class="sxs-lookup"><span data-stu-id="5d944-209">To view the changes made to the response output of the API, perform the following steps.</span></span>

   1. <span data-ttu-id="5d944-210">Depois que o navegador for aberto, pressione **F12** se as ferramentas de desenvolvedor ainda não estiverem abertas.</span><span class="sxs-lookup"><span data-stu-id="5d944-210">Once the browser opens, press **F12** if the developer tools are not open yet.</span></span>
   2. <span data-ttu-id="5d944-211">Clique na guia **rede** .</span><span class="sxs-lookup"><span data-stu-id="5d944-211">Click the **Network** tab.</span></span>
   3. <span data-ttu-id="5d944-212">Pressione o botão **Iniciar captura** .</span><span class="sxs-lookup"><span data-stu-id="5d944-212">Press the **Start Capturing** button.</span></span>
   4. <span data-ttu-id="5d944-213">Adicione o sufixo de URL **/API/Contact** à URL na barra de endereços e pressione a tecla **Enter** .</span><span class="sxs-lookup"><span data-stu-id="5d944-213">Add the URL suffix **/api/contact** to the URL in the address bar and press the **Enter** key.</span></span>
   5. <span data-ttu-id="5d944-214">Pressione o botão **ir para exibição detalhada** .</span><span class="sxs-lookup"><span data-stu-id="5d944-214">Press the **Go to detailed view** button.</span></span>
   6. <span data-ttu-id="5d944-215">Selecione a guia **corpo da resposta** . Você deve ver uma cadeia de caracteres JSON que representa a forma serializada de uma matriz de instâncias de contato.</span><span class="sxs-lookup"><span data-stu-id="5d944-215">Select the **Response body** tab. You should see a JSON string representing the serialized form of an array of Contact instances.</span></span>

      <span data-ttu-id="5d944-216">![Saída serializada JSON de uma chamada de método de API Web complexa](build-restful-apis-with-aspnet-web-api/_static/image13.png "Saída serializada JSON de uma chamada de método de API Web complexa")</span><span class="sxs-lookup"><span data-stu-id="5d944-216">![JSON serialized output of a complex Web API method call](build-restful-apis-with-aspnet-web-api/_static/image13.png "JSON serialized output of a complex Web API method call")</span></span>

      <span data-ttu-id="5d944-217">*Saída serializada JSON de uma chamada de método de API Web complexa*</span><span class="sxs-lookup"><span data-stu-id="5d944-217">*JSON serialized output of a complex Web API method call*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Extracting_Functionality_into_a_Service_Layer"></a>
#### <a name="task-4---extracting-functionality-into-a-service-layer"></a><span data-ttu-id="5d944-218">Tarefa 4-extraindo funcionalidade em uma camada de serviço</span><span class="sxs-lookup"><span data-stu-id="5d944-218">Task 4 - Extracting Functionality into a Service Layer</span></span>

<span data-ttu-id="5d944-219">Essa tarefa demonstrará como extrair a funcionalidade em uma camada de serviço para facilitar para os desenvolvedores a separação da funcionalidade de serviço da camada do controlador, permitindo, assim, a reutilização dos serviços que realmente fazem o trabalho.</span><span class="sxs-lookup"><span data-stu-id="5d944-219">This task will demonstrate how to extract functionality into a Service layer to make it easy for developers to separate their service functionality from the controller layer, thereby allowing reusability of the services that actually do the work.</span></span>

1. <span data-ttu-id="5d944-220">Crie uma nova pasta na raiz da solução e nomeie-a como **Serviços** de ti.</span><span class="sxs-lookup"><span data-stu-id="5d944-220">Create a new folder in the solution root and name it **Services**.</span></span> <span data-ttu-id="5d944-221">Para fazer isso, clique com o botão direito do mouse em **ContactManager** Project, selecione **Adicionar**  |  **nova pasta**, nomeie os *Serviços* de ti.</span><span class="sxs-lookup"><span data-stu-id="5d944-221">To do this, right-click **ContactManager** project, select **Add** | **New Folder**, name it *Services*.</span></span>

    <span data-ttu-id="5d944-222">![Criando pasta de serviços](build-restful-apis-with-aspnet-web-api/_static/image14.png "Criando pasta de serviços")</span><span class="sxs-lookup"><span data-stu-id="5d944-222">![Creating Services folder](build-restful-apis-with-aspnet-web-api/_static/image14.png "Creating Services folder")</span></span>

    <span data-ttu-id="5d944-223">*Criando pasta de serviços*</span><span class="sxs-lookup"><span data-stu-id="5d944-223">*Creating Services folder*</span></span>
2. <span data-ttu-id="5d944-224">Clique com o botão direito do mouse na pasta **Serviços** e selecione **Adicionar | Classe...** no menu de contexto.</span><span class="sxs-lookup"><span data-stu-id="5d944-224">Right-click the **Services** folder and select **Add | Class...** from the context menu.</span></span>

    <span data-ttu-id="5d944-225">![Adicionando uma nova classe à pasta serviços](build-restful-apis-with-aspnet-web-api/_static/image15.png "Adicionando uma nova classe à pasta serviços")</span><span class="sxs-lookup"><span data-stu-id="5d944-225">![Adding a new class to the Services folder](build-restful-apis-with-aspnet-web-api/_static/image15.png "Adding a new class to the Services folder")</span></span>

    <span data-ttu-id="5d944-226">*Adicionando uma nova classe à pasta serviços*</span><span class="sxs-lookup"><span data-stu-id="5d944-226">*Adding a new class to the Services folder*</span></span>
3. <span data-ttu-id="5d944-227">Quando a caixa de diálogo **Adicionar novo item** for exibida, nomeie a nova classe **ContactRepository** e clique em **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="5d944-227">When the **Add New Item** dialog appears, name the new class **ContactRepository** and click **Add**.</span></span>

    <span data-ttu-id="5d944-228">![Criando um arquivo de classe para conter o código para a camada de serviço do repositório de contatos](build-restful-apis-with-aspnet-web-api/_static/image16.png "Criando um arquivo de classe para conter o código para a camada de serviço do repositório de contatos")</span><span class="sxs-lookup"><span data-stu-id="5d944-228">![Creating a class file to contain the code for the Contact Repository service layer](build-restful-apis-with-aspnet-web-api/_static/image16.png "Creating a class file to contain the code for the Contact Repository service layer")</span></span>

    <span data-ttu-id="5d944-229">*Criando um arquivo de classe para conter o código para a camada de serviço do repositório de contatos*</span><span class="sxs-lookup"><span data-stu-id="5d944-229">*Creating a class file to contain the code for the Contact Repository service layer*</span></span>
4. <span data-ttu-id="5d944-230">Adicione uma diretiva using ao arquivo **ContactRepository.cs** para incluir o namespace de modelos.</span><span class="sxs-lookup"><span data-stu-id="5d944-230">Add a using directive to the **ContactRepository.cs** file to include the models namespace.</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample4.cs)]
5. <span data-ttu-id="5d944-231">Adicione o seguinte código realçado ao arquivo **ContactRepository.cs** para implementar o método GetAllContacts.</span><span class="sxs-lookup"><span data-stu-id="5d944-231">Add the following highlighted code to the **ContactRepository.cs** file to implement GetAllContacts method.</span></span>

    <span data-ttu-id="5d944-232">(Trecho de código- *laboratório da API Web-Ex01-repositório de contatos*)</span><span class="sxs-lookup"><span data-stu-id="5d944-232">(Code Snippet - *Web API Lab - Ex01 - Contact Repository*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample5.cs)]
6. <span data-ttu-id="5d944-233">Abra o arquivo **ContactController.cs** se ele ainda não estiver aberto.</span><span class="sxs-lookup"><span data-stu-id="5d944-233">Open the **ContactController.cs** file if it is not already open.</span></span>
7. <span data-ttu-id="5d944-234">Adicione a seguinte instrução using à seção de declaração de namespace do arquivo.</span><span class="sxs-lookup"><span data-stu-id="5d944-234">Add the following using statement to the namespace declaration section of the file.</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample6.cs)]
8. <span data-ttu-id="5d944-235">Adicione o seguinte código realçado à classe **ContactController.cs** para adicionar um campo particular para representar a instância do repositório, para que o restante dos membros da classe possa fazer uso da implementação do serviço.</span><span class="sxs-lookup"><span data-stu-id="5d944-235">Add the following highlighted code to the **ContactController.cs** class to add a private field to represent the instance of the repository, so that the rest of the class members can make use of the service implementation.</span></span>

    <span data-ttu-id="5d944-236">(Trecho de código- *laboratório da API Web-Ex01-controlador de contato*)</span><span class="sxs-lookup"><span data-stu-id="5d944-236">(Code Snippet - *Web API Lab - Ex01 - Contact Controller*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample7.cs)]
9. <span data-ttu-id="5d944-237">Altere o método **Get** para que ele faça uso do serviço de repositório de contatos.</span><span class="sxs-lookup"><span data-stu-id="5d944-237">Change the **Get** method so that it makes use of the contact repository service.</span></span>

    <span data-ttu-id="5d944-238">(Trecho de código- *laboratório da API Web-Ex01-retornando uma lista de contatos por meio do repositório*)</span><span class="sxs-lookup"><span data-stu-id="5d944-238">(Code Snippet - *Web API Lab - Ex01 - Returning a list of contacts via the repository*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample8.cs)]
10. <span data-ttu-id="5d944-239">Coloque um ponto de interrupção na definição do método **Get** de **ContactController**.</span><span class="sxs-lookup"><span data-stu-id="5d944-239">Put a breakpoint on the **ContactController**'s **Get** method definition.</span></span>

   <span data-ttu-id="5d944-240">![Adicionando pontos de interrupção ao controlador de contato](build-restful-apis-with-aspnet-web-api/_static/image17.png "Adicionando pontos de interrupção ao controlador de contato")</span><span class="sxs-lookup"><span data-stu-id="5d944-240">![Adding breakpoints to the contact controller](build-restful-apis-with-aspnet-web-api/_static/image17.png "Adding breakpoints to the contact controller")</span></span>

   <span data-ttu-id="5d944-241">*Adicionando pontos de interrupção ao controlador de contato*</span><span class="sxs-lookup"><span data-stu-id="5d944-241">*Adding breakpoints to the contact controller*</span></span>
11. <span data-ttu-id="5d944-242">Pressione **F5** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="5d944-242">Press **F5** to run the application.</span></span>
12. <span data-ttu-id="5d944-243">Quando o navegador for aberto, pressione **F12** para abrir as ferramentas de desenvolvedor.</span><span class="sxs-lookup"><span data-stu-id="5d944-243">When the browser opens, press **F12** to open the developer tools.</span></span>
13. <span data-ttu-id="5d944-244">Clique na guia **rede** .</span><span class="sxs-lookup"><span data-stu-id="5d944-244">Click the **Network** tab.</span></span>
14. <span data-ttu-id="5d944-245">Clique no botão **Iniciar captura** .</span><span class="sxs-lookup"><span data-stu-id="5d944-245">Click the **Start Capturing** button.</span></span>
15. <span data-ttu-id="5d944-246">Acrescente a URL na barra de endereços com o sufixo **/API/Contact** e pressione **Enter** para carregar o controlador de API.</span><span class="sxs-lookup"><span data-stu-id="5d944-246">Append the URL in the address bar with the suffix **/api/contact** and press **Enter** to load the API controller.</span></span>
16. <span data-ttu-id="5d944-247">O Visual Studio 2012 deve interromper uma vez que o método **Get** comece a execução.</span><span class="sxs-lookup"><span data-stu-id="5d944-247">Visual Studio 2012 should break once **Get** method begins execution.</span></span>

   <span data-ttu-id="5d944-248">![Quebrando dentro do método Get](build-restful-apis-with-aspnet-web-api/_static/image18.png "Quebrando dentro do método Get")</span><span class="sxs-lookup"><span data-stu-id="5d944-248">![Breaking within the Get method](build-restful-apis-with-aspnet-web-api/_static/image18.png "Breaking within the Get method")</span></span>

   <span data-ttu-id="5d944-249">*Quebrando dentro do método Get*</span><span class="sxs-lookup"><span data-stu-id="5d944-249">*Breaking within the Get method*</span></span>
17. <span data-ttu-id="5d944-250">Pressione **F5** para continuar.</span><span class="sxs-lookup"><span data-stu-id="5d944-250">Press **F5** to continue.</span></span>
18. <span data-ttu-id="5d944-251">Volte para o Internet Explorer se ele ainda não estiver em foco.</span><span class="sxs-lookup"><span data-stu-id="5d944-251">Go back to Internet Explorer if it is not already in focus.</span></span> <span data-ttu-id="5d944-252">Observe a janela de captura de rede.</span><span class="sxs-lookup"><span data-stu-id="5d944-252">Note the network capture window.</span></span>

    <span data-ttu-id="5d944-253">![Exibição de rede no Internet Explorer mostrando os resultados da chamada à API Web](build-restful-apis-with-aspnet-web-api/_static/image19.png "Exibição de rede no Internet Explorer mostrando os resultados da chamada à API Web")</span><span class="sxs-lookup"><span data-stu-id="5d944-253">![Network view in Internet Explorer showing results of the Web API call](build-restful-apis-with-aspnet-web-api/_static/image19.png "Network view in Internet Explorer showing results of the Web API call")</span></span>

    <span data-ttu-id="5d944-254">*Exibição de rede no Internet Explorer mostrando os resultados da chamada à API Web*</span><span class="sxs-lookup"><span data-stu-id="5d944-254">*Network view in Internet Explorer showing results of the Web API call*</span></span>
19. <span data-ttu-id="5d944-255">Clique no botão **ir para exibição detalhada** .</span><span class="sxs-lookup"><span data-stu-id="5d944-255">Click the **Go to detailed view** button.</span></span>
20. <span data-ttu-id="5d944-256">Clique na guia **corpo da resposta** . Observe a saída JSON da chamada à API e como ela representa os dois contatos recuperados pela camada de serviço.</span><span class="sxs-lookup"><span data-stu-id="5d944-256">Click the **Response body** tab. Note the JSON output of the API call, and how it represents the two contacts retrieved by the service layer.</span></span>

    <span data-ttu-id="5d944-257">![Exibindo a saída JSON da API Web na janela ferramentas de desenvolvedor](build-restful-apis-with-aspnet-web-api/_static/image20.png "Exibindo a saída JSON da API Web na janela ferramentas de desenvolvedor")</span><span class="sxs-lookup"><span data-stu-id="5d944-257">![Viewing the JSON output from the Web API in the developer tools window](build-restful-apis-with-aspnet-web-api/_static/image20.png "Viewing the JSON output from the Web API in the developer tools window")</span></span>

    <span data-ttu-id="5d944-258">*Exibindo a saída JSON da API Web na janela ferramentas de desenvolvedor*</span><span class="sxs-lookup"><span data-stu-id="5d944-258">*Viewing the JSON output from the Web API in the developer tools window*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Create_a_ReadWrite_Web_API"></a>
### <a name="exercise-2-create-a-readwrite-web-api"></a><span data-ttu-id="5d944-259">Exercício 2: criar uma API da Web de leitura/gravação</span><span class="sxs-lookup"><span data-stu-id="5d944-259">Exercise 2: Create a Read/Write Web API</span></span>

<span data-ttu-id="5d944-260">Neste exercício, você implementará os métodos POST e PUT para o Gerenciador de contatos para habilitá-lo com recursos de edição de dados.</span><span class="sxs-lookup"><span data-stu-id="5d944-260">In this exercise, you will implement POST and PUT methods for the contact manager to enable it with data-editing features.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Opening_the_Web_API_Project"></a>
#### <a name="task-1---opening-the-web-api-project"></a><span data-ttu-id="5d944-261">Tarefa 1-abrindo o projeto de API Web</span><span class="sxs-lookup"><span data-stu-id="5d944-261">Task 1 - Opening the Web API Project</span></span>

<span data-ttu-id="5d944-262">Nesta tarefa, você irá se preparar para aprimorar o projeto de API Web criado no exercício 1 para que ele possa aceitar a entrada do usuário.</span><span class="sxs-lookup"><span data-stu-id="5d944-262">In this task, you will prepare to enhance the Web API project created in Exercise 1 so that it can accept user input.</span></span>

1. <span data-ttu-id="5d944-263">Execute o **Visual Studio 2012 Express para Web**, para fazer isso, vá para **iniciar** e digite **vs Express para Web** pressione **Enter**.</span><span class="sxs-lookup"><span data-stu-id="5d944-263">Run **Visual Studio 2012 Express for Web**, to do this go to **Start** and type **VS Express for Web** then press **Enter**.</span></span>
2. <span data-ttu-id="5d944-264">Abra a solução **inicial** localizada na **origem/Ex02-ReadWriteWebAPI/início/** pasta.</span><span class="sxs-lookup"><span data-stu-id="5d944-264">Open the **Begin** solution located at **Source/Ex02-ReadWriteWebAPI/Begin/** folder.</span></span> <span data-ttu-id="5d944-265">Caso contrário, você pode continuar usando a solução **final** obtida concluindo o exercício anterior.</span><span class="sxs-lookup"><span data-stu-id="5d944-265">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="5d944-266">Se você tiver aberto a solução **inicial** fornecida, será necessário baixar alguns pacotes NuGet ausentes antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="5d944-266">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="5d944-267">Para fazer isso, clique no menu **projeto** e selecione **gerenciar pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="5d944-267">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="5d944-268">Na caixa de diálogo **gerenciar pacotes NuGet** , clique em **restaurar** para baixar os pacotes ausentes.</span><span class="sxs-lookup"><span data-stu-id="5d944-268">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="5d944-269">Por fim, Compile a solução clicando em **criar**  |  **solução de compilação**.</span><span class="sxs-lookup"><span data-stu-id="5d944-269">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="5d944-270">Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto.</span><span class="sxs-lookup"><span data-stu-id="5d944-270">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="5d944-271">Com o NuGet Power Tools, especificando as versões do pacote no arquivo de Packages.config, você poderá baixar todas as bibliotecas necessárias na primeira vez em que executar o projeto.</span><span class="sxs-lookup"><span data-stu-id="5d944-271">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="5d944-272">É por isso que você precisará executar essas etapas depois de abrir uma solução existente deste laboratório.</span><span class="sxs-lookup"><span data-stu-id="5d944-272">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="5d944-273">Abra o arquivo **Services/ContactRepository. cs** .</span><span class="sxs-lookup"><span data-stu-id="5d944-273">Open the **Services/ContactRepository.cs** file.</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_-_Adding_Data-Persistence_Features_to_the_Contact_Repository_Implementation"></a>
#### <a name="task-2---adding-data-persistence-features-to-the-contact-repository-implementation"></a><span data-ttu-id="5d944-274">Tarefa 2-adicionando recursos de Data-Persistence à implementação do repositório de contatos</span><span class="sxs-lookup"><span data-stu-id="5d944-274">Task 2 - Adding Data-Persistence Features to the Contact Repository Implementation</span></span>

<span data-ttu-id="5d944-275">Nesta tarefa, você aumentará a classe ContactRepository do projeto de API Web criado no exercício 1 para que ele possa persistir e aceitar as novas instâncias de contato e entrada do usuário.</span><span class="sxs-lookup"><span data-stu-id="5d944-275">In this task, you will augment the ContactRepository class of the Web API project created in Exercise 1 so that it can persist and accept user input and new Contact instances.</span></span>

1. <span data-ttu-id="5d944-276">Adicione a seguinte constante à classe **ContactRepository** para representar o nome do nome da chave do item de cache do servidor Web posteriormente neste exercício.</span><span class="sxs-lookup"><span data-stu-id="5d944-276">Add the following constant to the **ContactRepository** class to represent the name of the web server cache item key name later in this exercise.</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample9.cs)]
2. <span data-ttu-id="5d944-277">Adicione um construtor ao **ContactRepository** que contém o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="5d944-277">Add a constructor to the **ContactRepository** containing the following code.</span></span>

    <span data-ttu-id="5d944-278">(Trecho de código- *Ex02-Lab de API Web-Construtor de repositório de contatos*)</span><span class="sxs-lookup"><span data-stu-id="5d944-278">(Code Snippet - *Web API Lab - Ex02 - Contact Repository Constructor*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample10.cs)]
3. <span data-ttu-id="5d944-279">Modifique o código para o método **GetAllContacts** , conforme demonstrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="5d944-279">Modify the code for the **GetAllContacts** method as demonstrated below.</span></span>

    <span data-ttu-id="5d944-280">(Trecho de código- *laboratório da API Web-Ex02-obter todos os contatos*)</span><span class="sxs-lookup"><span data-stu-id="5d944-280">(Code Snippet - *Web API Lab - Ex02 - Get All Contacts*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample11.cs)]

    > [!NOTE]
    > <span data-ttu-id="5d944-281">Este exemplo é para fins de demonstração e usará o cache do servidor Web como meio de armazenamento, de modo que os valores estarão disponíveis para vários clientes simultaneamente, em vez de usar um mecanismo de armazenamento de sessão ou um tempo de vida de armazenamento de solicitação.</span><span class="sxs-lookup"><span data-stu-id="5d944-281">This example is for demonstration purposes and will use the web server's cache as a storage medium, so that the values will be available to multiple clients simultaneously, rather than use a Session storage mechanism or a Request storage lifetime.</span></span> <span data-ttu-id="5d944-282">É possível usar Entity Framework, o armazenamento XML ou qualquer outra variedade no lugar do cache do servidor Web.</span><span class="sxs-lookup"><span data-stu-id="5d944-282">One could use Entity Framework, XML storage, or any other variety in place of the web server cache.</span></span>
4. <span data-ttu-id="5d944-283">Implemente um novo método chamado **SaveContact** para a classe **ContactRepository** para fazer o trabalho de salvar um contato.</span><span class="sxs-lookup"><span data-stu-id="5d944-283">Implement a new method named **SaveContact** to the **ContactRepository** class to do the work of saving a contact.</span></span> <span data-ttu-id="5d944-284">O método **SaveContact** deve usar um único parâmetro de **contato** e retornar um valor booliano indicando êxito ou falha.</span><span class="sxs-lookup"><span data-stu-id="5d944-284">The **SaveContact** method should take a single **Contact** parameter and return a Boolean value indicating success or failure.</span></span>

    <span data-ttu-id="5d944-285">(Trecho de código- *laboratório da API Web-Ex02-implementando o método SaveContact*)</span><span class="sxs-lookup"><span data-stu-id="5d944-285">(Code Snippet - *Web API Lab - Ex02 - Implementing the SaveContact Method*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample12.cs)]

<a id="Exercise3"></a>

<a id="Exercise_3_Consume_the_Web_API_from_an_HTML_Client"></a>
### <a name="exercise-3-consume-the-web-api-from-an-html-client"></a><span data-ttu-id="5d944-286">Exercício 3: consumir a API Web de um cliente HTML</span><span class="sxs-lookup"><span data-stu-id="5d944-286">Exercise 3: Consume the Web API from an HTML Client</span></span>

<span data-ttu-id="5d944-287">Neste exercício, você criará um cliente HTML para chamar a API da Web.</span><span class="sxs-lookup"><span data-stu-id="5d944-287">In this exercise, you will create an HTML client to call the Web API.</span></span> <span data-ttu-id="5d944-288">Esse cliente facilitará a troca de dados com a API Web usando JavaScript e exibirá os resultados em um navegador da Web usando marcação HTML.</span><span class="sxs-lookup"><span data-stu-id="5d944-288">This client will facilitate data exchange with the Web API using JavaScript and will display the results in a web browser using HTML markup.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Displaying_Contacts"></a>
#### <a name="task-1---modifying-the-index-view-to-provide-a-gui-for-displaying-contacts"></a><span data-ttu-id="5d944-289">Tarefa 1-modificando a exibição de índice para fornecer uma GUI para exibir contatos</span><span class="sxs-lookup"><span data-stu-id="5d944-289">Task 1 - Modifying the Index View to Provide a GUI for Displaying Contacts</span></span>

<span data-ttu-id="5d944-290">Nesta tarefa, você modificará a exibição de índice padrão do aplicativo Web para dar suporte ao requisito de exibir a lista de contatos existentes em um navegador HTML.</span><span class="sxs-lookup"><span data-stu-id="5d944-290">In this task, you will modify the default Index view of the web application to support the requirement of displaying the list of existing contacts in an HTML browser.</span></span>

1. <span data-ttu-id="5d944-291">Abra o **Visual Studio 2012 Express para Web** se ele ainda não estiver aberto.</span><span class="sxs-lookup"><span data-stu-id="5d944-291">Open **Visual Studio 2012 Express for Web** if it is not already open.</span></span>
2. <span data-ttu-id="5d944-292">Abra a solução **inicial** localizada na **origem/Ex03-ConsumingWebAPI/início/** pasta.</span><span class="sxs-lookup"><span data-stu-id="5d944-292">Open the **Begin** solution located at **Source/Ex03-ConsumingWebAPI/Begin/** folder.</span></span> <span data-ttu-id="5d944-293">Caso contrário, você pode continuar usando a solução **final** obtida concluindo o exercício anterior.</span><span class="sxs-lookup"><span data-stu-id="5d944-293">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="5d944-294">Se você tiver aberto a solução **inicial** fornecida, será necessário baixar alguns pacotes NuGet ausentes antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="5d944-294">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="5d944-295">Para fazer isso, clique no menu **projeto** e selecione **gerenciar pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="5d944-295">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="5d944-296">Na caixa de diálogo **gerenciar pacotes NuGet** , clique em **restaurar** para baixar os pacotes ausentes.</span><span class="sxs-lookup"><span data-stu-id="5d944-296">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="5d944-297">Por fim, Compile a solução clicando em **criar**  |  **solução de compilação**.</span><span class="sxs-lookup"><span data-stu-id="5d944-297">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="5d944-298">Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto.</span><span class="sxs-lookup"><span data-stu-id="5d944-298">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="5d944-299">Com o NuGet Power Tools, especificando as versões do pacote no arquivo de Packages.config, você poderá baixar todas as bibliotecas necessárias na primeira vez em que executar o projeto.</span><span class="sxs-lookup"><span data-stu-id="5d944-299">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="5d944-300">É por isso que você precisará executar essas etapas depois de abrir uma solução existente deste laboratório.</span><span class="sxs-lookup"><span data-stu-id="5d944-300">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="5d944-301">Abra o arquivo **index. cshtml** localizado em **exibições/pasta base** .</span><span class="sxs-lookup"><span data-stu-id="5d944-301">Open the **Index.cshtml** file located at **Views/Home** folder.</span></span>
4. <span data-ttu-id="5d944-302">Substitua o código HTML dentro do elemento div por um **corpo** de ID para que ele se pareça com o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="5d944-302">Replace the HTML code within the div element with id **body** so that it looks like the following code.</span></span>

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample13.html)]
5. <span data-ttu-id="5d944-303">Adicione o seguinte código JavaScript na parte inferior do arquivo para executar a solicitação HTTP para a API da Web.</span><span class="sxs-lookup"><span data-stu-id="5d944-303">Add the following Javascript code at the bottom of the file to perform the HTTP request to the Web API.</span></span>

    [!code-cshtml[Main](build-restful-apis-with-aspnet-web-api/samples/sample14.cshtml)]
6. <span data-ttu-id="5d944-304">Abra o arquivo **ContactController.cs** se ele ainda não estiver aberto.</span><span class="sxs-lookup"><span data-stu-id="5d944-304">Open the **ContactController.cs** file if it is not already open.</span></span>
7. <span data-ttu-id="5d944-305">Coloque um ponto de interrupção no método **Get** da classe **ContactController** .</span><span class="sxs-lookup"><span data-stu-id="5d944-305">Place a breakpoint on the **Get** method of the **ContactController** class.</span></span>

    <span data-ttu-id="5d944-306">![Colocando um ponto de interrupção no método Get do controlador de API](build-restful-apis-with-aspnet-web-api/_static/image21.png "Colocando um ponto de interrupção no método Get do controlador de API")</span><span class="sxs-lookup"><span data-stu-id="5d944-306">![Placing a breakpoint on the Get method of the API controller](build-restful-apis-with-aspnet-web-api/_static/image21.png "Placing a breakpoint on the Get method of the API controller")</span></span>

    <span data-ttu-id="5d944-307">*Colocando um ponto de interrupção no método Get do controlador de API*</span><span class="sxs-lookup"><span data-stu-id="5d944-307">*Placing a breakpoint on the Get method of the API controller*</span></span>
8. <span data-ttu-id="5d944-308">Pressione **F5** para executar o projeto.</span><span class="sxs-lookup"><span data-stu-id="5d944-308">Press **F5** to run the project.</span></span> <span data-ttu-id="5d944-309">O navegador carregará o documento HTML.</span><span class="sxs-lookup"><span data-stu-id="5d944-309">The browser will load the HTML document.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5d944-310">Certifique-se de que você está navegando para a URL raiz do seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="5d944-310">Ensure that you are browsing to the root URL of your application.</span></span>
9. <span data-ttu-id="5d944-311">Depois que a página for carregada e o JavaScript for executado, o ponto de interrupção será atingido e a execução do código será pausada no controlador.</span><span class="sxs-lookup"><span data-stu-id="5d944-311">Once the page loads and the JavaScript executes, the breakpoint will be hit and the code execution will pause in the controller.</span></span>

    <span data-ttu-id="5d944-312">![Depuração nas chamadas à API Web usando VS Express para Web](build-restful-apis-with-aspnet-web-api/_static/image22.png "Depuração nas chamadas à API Web usando VS Express para Web")</span><span class="sxs-lookup"><span data-stu-id="5d944-312">![Debugging into the Web API calls using VS Express for Web](build-restful-apis-with-aspnet-web-api/_static/image22.png "Debugging into the Web API calls using VS Express for Web")</span></span>

    <span data-ttu-id="5d944-313">*Depuração na chamada à API Web usando o Visual Studio 2012 Express para Web*</span><span class="sxs-lookup"><span data-stu-id="5d944-313">*Debugging into the Web API call using Visual Studio 2012 Express for Web*</span></span>
10. <span data-ttu-id="5d944-314">Remova o ponto de interrupção e pressione **F5** ou o botão **continuar** da barra de ferramentas de depuração para continuar carregando o modo de exibição no navegador.</span><span class="sxs-lookup"><span data-stu-id="5d944-314">Remove the breakpoint and press **F5** or the debugging toolbar's **Continue** button to continue loading the view in the browser.</span></span> <span data-ttu-id="5d944-315">Depois que a chamada da API Web for concluída, você deverá ver os contatos retornados da chamada da API Web exibida como itens de lista no navegador.</span><span class="sxs-lookup"><span data-stu-id="5d944-315">Once the Web API call completes you should see the contacts returned from the Web API call displayed as list items in the browser.</span></span>

    <span data-ttu-id="5d944-316">![Resultados da chamada à API exibida no navegador como itens de lista](build-restful-apis-with-aspnet-web-api/_static/image23.png "Resultados da chamada à API exibida no navegador como itens de lista")</span><span class="sxs-lookup"><span data-stu-id="5d944-316">![Results of the API call displayed in the browser as list items](build-restful-apis-with-aspnet-web-api/_static/image23.png "Results of the API call displayed in the browser as list items")</span></span>

    <span data-ttu-id="5d944-317">*Resultados da chamada à API exibida no navegador como itens de lista*</span><span class="sxs-lookup"><span data-stu-id="5d944-317">*Results of the API call displayed in the browser as list items*</span></span>
11. <span data-ttu-id="5d944-318">Interrompa a depuração.</span><span class="sxs-lookup"><span data-stu-id="5d944-318">Stop debugging.</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Creating_Contacts"></a>
#### <a name="task-2---modifying-the-index-view-to-provide-a-gui-for-creating-contacts"></a><span data-ttu-id="5d944-319">Tarefa 2-modificando a exibição de índice para fornecer uma GUI para criar contatos</span><span class="sxs-lookup"><span data-stu-id="5d944-319">Task 2 - Modifying the Index View to Provide a GUI for Creating Contacts</span></span>

<span data-ttu-id="5d944-320">Nesta tarefa, você continuará a modificar a exibição de índice do aplicativo MVC.</span><span class="sxs-lookup"><span data-stu-id="5d944-320">In this task, you will continue to modify the Index view of the MVC application.</span></span> <span data-ttu-id="5d944-321">Um formulário será adicionado à página HTML que capturará a entrada do usuário e o enviará para a API da Web para criar um novo contato, e um novo método do controlador da API Web será criado para coletar a data da GUI.</span><span class="sxs-lookup"><span data-stu-id="5d944-321">A form will be added to the HTML page that will capture user input and send it to the Web API to create a new Contact, and a new Web API controller method will be created to collect date from the GUI.</span></span>

1. <span data-ttu-id="5d944-322">Abra o arquivo **ContactController.cs** .</span><span class="sxs-lookup"><span data-stu-id="5d944-322">Open the **ContactController.cs** file.</span></span>
2. <span data-ttu-id="5d944-323">Adicione um novo método à classe de controlador chamada **post** , conforme mostrado no código a seguir.</span><span class="sxs-lookup"><span data-stu-id="5d944-323">Add a new method to the controller class named **Post** as shown in the following code.</span></span>

    <span data-ttu-id="5d944-324">(Trecho de código- *laboratório da API Web-método Ex03-post*)</span><span class="sxs-lookup"><span data-stu-id="5d944-324">(Code Snippet - *Web API Lab - Ex03 - Post Method*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample15.cs)]
3. <span data-ttu-id="5d944-325">Abra o arquivo **index. cshtml** no Visual Studio se ele ainda não estiver aberto.</span><span class="sxs-lookup"><span data-stu-id="5d944-325">Open the **Index.cshtml** file in Visual Studio if it is not already open.</span></span>
4. <span data-ttu-id="5d944-326">Adicione o código HTML abaixo ao arquivo logo após a lista não ordenada que você adicionou na tarefa anterior.</span><span class="sxs-lookup"><span data-stu-id="5d944-326">Add the HTML code below to the file just after the unordered list you added in the previous task.</span></span>

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample16.html)]
5. <span data-ttu-id="5d944-327">No elemento script na parte inferior do documento, adicione o seguinte código realçado para manipular eventos de clique de botão, que irão postar os dados para a API Web usando uma chamada HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="5d944-327">Within the script element at the bottom of the document, add the following highlighted code to handle button-click events, which will post the data to the Web API using an HTTP POST call.</span></span>

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample17.html)]
6. <span data-ttu-id="5d944-328">Em **ContactController.cs**, coloque um ponto de interrupção no método **post** .</span><span class="sxs-lookup"><span data-stu-id="5d944-328">In **ContactController.cs**, place a breakpoint on the **Post** method.</span></span>
7. <span data-ttu-id="5d944-329">Pressione **F5** para executar o aplicativo no navegador.</span><span class="sxs-lookup"><span data-stu-id="5d944-329">Press **F5** to run the application in the browser.</span></span>
8. <span data-ttu-id="5d944-330">Depois que a página for carregada no navegador, digite um novo nome e ID de contato e clique no botão **salvar** .</span><span class="sxs-lookup"><span data-stu-id="5d944-330">Once the page is loaded in the browser, type in a new contact name and Id and click the **Save** button.</span></span>

    <span data-ttu-id="5d944-331">![O documento HTML do cliente carregado no navegador](build-restful-apis-with-aspnet-web-api/_static/image24.png "O documento HTML do cliente carregado no navegador")</span><span class="sxs-lookup"><span data-stu-id="5d944-331">![The client HTML document loaded in the browser](build-restful-apis-with-aspnet-web-api/_static/image24.png "The client HTML document loaded in the browser")</span></span>

    <span data-ttu-id="5d944-332">*O documento HTML do cliente carregado no navegador*</span><span class="sxs-lookup"><span data-stu-id="5d944-332">*The client HTML document loaded in the browser*</span></span>
9. <span data-ttu-id="5d944-333">Quando a janela do depurador for interrompida no método **post** , dê uma olhada nas propriedades do parâmetro **Contact** .</span><span class="sxs-lookup"><span data-stu-id="5d944-333">When the debugger window breaks in the **Post** method, take a look at the properties of the **contact** parameter.</span></span> <span data-ttu-id="5d944-334">Os valores devem corresponder aos dados inseridos no formulário.</span><span class="sxs-lookup"><span data-stu-id="5d944-334">The values should match the data you entered in the form.</span></span>

    <span data-ttu-id="5d944-335">![O objeto de contato que está sendo enviado para a API Web do cliente](build-restful-apis-with-aspnet-web-api/_static/image25.png "O objeto de contato que está sendo enviado para a API Web do cliente")</span><span class="sxs-lookup"><span data-stu-id="5d944-335">![The Contact object being sent to the Web API from the client](build-restful-apis-with-aspnet-web-api/_static/image25.png "The Contact object being sent to the Web API from the client")</span></span>

    <span data-ttu-id="5d944-336">*O objeto de contato que está sendo enviado para a API Web do cliente*</span><span class="sxs-lookup"><span data-stu-id="5d944-336">*The Contact object being sent to the Web API from the client*</span></span>
10. <span data-ttu-id="5d944-337">Percorra o método no depurador até que a variável de **resposta** tenha sido criada.</span><span class="sxs-lookup"><span data-stu-id="5d944-337">Step through the method in the debugger until the **response** variable has been created.</span></span> <span data-ttu-id="5d944-338">Após a inspeção na janela **locais** no depurador, você verá que todas as propriedades foram definidas.</span><span class="sxs-lookup"><span data-stu-id="5d944-338">Upon inspection in the **Locals** window in the debugger, you'll see that all the properties have been set.</span></span>

   <span data-ttu-id="5d944-339">![A resposta após a criação no depurador](build-restful-apis-with-aspnet-web-api/_static/image26.png "A resposta após a criação no depurador")</span><span class="sxs-lookup"><span data-stu-id="5d944-339">![The response following creation in the debugger](build-restful-apis-with-aspnet-web-api/_static/image26.png "The response following creation in the debugger")</span></span>

   <span data-ttu-id="5d944-340">*A resposta após a criação no depurador*</span><span class="sxs-lookup"><span data-stu-id="5d944-340">*The response following creation in the debugger*</span></span>
11. <span data-ttu-id="5d944-341">Se você pressionar **F5** ou clicar em **continuar** no depurador, a solicitação será concluída.</span><span class="sxs-lookup"><span data-stu-id="5d944-341">If you press **F5** or click **Continue** in the debugger the request will complete.</span></span> <span data-ttu-id="5d944-342">Depois de voltar para o navegador, o novo contato foi adicionado à lista de contatos armazenados pela implementação do **ContactRepository** .</span><span class="sxs-lookup"><span data-stu-id="5d944-342">Once you switch back to the browser, the new contact has been added to the list of contacts stored by the **ContactRepository** implementation.</span></span>

   <span data-ttu-id="5d944-343">![O navegador reflete a criação bem-sucedida da nova instância de contato](build-restful-apis-with-aspnet-web-api/_static/image27.png "O navegador reflete a criação bem-sucedida da nova instância de contato")</span><span class="sxs-lookup"><span data-stu-id="5d944-343">![The browser reflects successful creation of the new contact instance](build-restful-apis-with-aspnet-web-api/_static/image27.png "The browser reflects successful creation of the new contact instance")</span></span>

   <span data-ttu-id="5d944-344">*O navegador reflete a criação bem-sucedida da nova instância de contato*</span><span class="sxs-lookup"><span data-stu-id="5d944-344">*The browser reflects successful creation of the new contact instance*</span></span>

> [!NOTE]
> <span data-ttu-id="5d944-345">Além disso, você pode implantar esse aplicativo no Azure após o [Apêndice C: publicando um aplicativo ASP.NET MVC 4 usando implantação da Web](#AppendixC).</span><span class="sxs-lookup"><span data-stu-id="5d944-345">Additionally, you can deploy this application to Azure following [Appendix C: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixC).</span></span>

---

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="5d944-346">Resumo</span><span class="sxs-lookup"><span data-stu-id="5d944-346">Summary</span></span>

<span data-ttu-id="5d944-347">Este laboratório apresentou a você o novo ASP.NET Web API Framework e a implementação de APIs Web RESTful usando a estrutura.</span><span class="sxs-lookup"><span data-stu-id="5d944-347">This lab has introduced you to the new ASP.NET Web API framework and to the implementation of RESTful Web APIs using the framework.</span></span> <span data-ttu-id="5d944-348">A partir daqui, você pode criar um novo repositório que facilite a persistência de dados usando qualquer número de mecanismos e conecte esse serviço em vez do simples fornecido como um exemplo neste laboratório.</span><span class="sxs-lookup"><span data-stu-id="5d944-348">From here, you could create a new repository that facilitates data persistence using any number of mechanisms and wire that service up rather than the simple one provided as an example in this lab.</span></span> <span data-ttu-id="5d944-349">A API Web dá suporte a vários recursos adicionais, como a habilitação da comunicação de clientes não HTML escritos em qualquer linguagem compatível com HTTP e JSON ou XML.</span><span class="sxs-lookup"><span data-stu-id="5d944-349">Web API supports a number of additional features, such as enabling communication from non-HTML clients written in any language that supports HTTP and JSON or XML.</span></span> <span data-ttu-id="5d944-350">A capacidade de hospedar uma API da Web fora de um aplicativo Web típico também é possível, bem como a capacidade de criar seus próprios formatos de serialização.</span><span class="sxs-lookup"><span data-stu-id="5d944-350">The ability to host a Web API outside of a typical web application is also possible, as well as is the ability to create your own serialization formats.</span></span>

<span data-ttu-id="5d944-351">O site da ASP.NET tem uma área dedicada à estrutura de ASP.NET Web API em [[https://asp.net/web-api](https://asp.net/web-api)](https://asp.net/web-api) .</span><span class="sxs-lookup"><span data-stu-id="5d944-351">The ASP.NET Web site has an area dedicated to the ASP.NET Web API framework at [[https://asp.net/web-api](https://asp.net/web-api)](https://asp.net/web-api).</span></span> <span data-ttu-id="5d944-352">Este site continuará a fornecer informações mais recentes, amostras e notícias relacionadas à API da Web, portanto, verifique com frequência se você gostaria de se aprofundar na arte de criar APIs Web personalizadas disponíveis para praticamente qualquer estrutura de dispositivo ou de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="5d944-352">This site will continue to provide late-breaking information, samples, and news related to Web API, so check it frequently if you'd like to delve deeper into the art of creating custom Web APIs available to virtually any device or development framework.</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a><span data-ttu-id="5d944-353">Apêndice A: usando trechos de código</span><span class="sxs-lookup"><span data-stu-id="5d944-353">Appendix A: Using Code Snippets</span></span>

<span data-ttu-id="5d944-354">Com trechos de código, você tem todo o código de que precisa ao seu alcance.</span><span class="sxs-lookup"><span data-stu-id="5d944-354">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="5d944-355">O documento de laboratório informará exatamente quando você pode usá-los, conforme mostrado na figura a seguir.</span><span class="sxs-lookup"><span data-stu-id="5d944-355">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="5d944-356">![Usando trechos de código do Visual Studio para inserir código em seu projeto](build-restful-apis-with-aspnet-web-api/_static/image28.png "Usando trechos de código do Visual Studio para inserir código em seu projeto")</span><span class="sxs-lookup"><span data-stu-id="5d944-356">![Using Visual Studio code snippets to insert code into your project](build-restful-apis-with-aspnet-web-api/_static/image28.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="5d944-357">*Usando trechos de código do Visual Studio para inserir código em seu projeto*</span><span class="sxs-lookup"><span data-stu-id="5d944-357">*Using Visual Studio code snippets to insert code into your project*</span></span>

<a id="CodeSnippetUsingKeyBoard"></a>

<a id="To_add_a_code_snippet_using_the_keyboard_C_only"></a>
### <a name="to-add-a-code-snippet-using-the-keyboard-c-only"></a><span data-ttu-id="5d944-358">Para adicionar um trecho de código usando o teclado (C# somente)</span><span class="sxs-lookup"><span data-stu-id="5d944-358">To add a code snippet using the keyboard (C# only)</span></span>

1. <span data-ttu-id="5d944-359">Coloque o cursor onde você deseja inserir o código.</span><span class="sxs-lookup"><span data-stu-id="5d944-359">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="5d944-360">Comece digitando o nome do trecho de código (sem espaços ou hifens).</span><span class="sxs-lookup"><span data-stu-id="5d944-360">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="5d944-361">Observe como o IntelliSense exibe nomes de trechos de código correspondentes.</span><span class="sxs-lookup"><span data-stu-id="5d944-361">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="5d944-362">Selecione o trecho correto (ou continue digitando até que o nome do trecho inteiro seja selecionado).</span><span class="sxs-lookup"><span data-stu-id="5d944-362">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="5d944-363">Pressione a tecla TAB duas vezes para inserir o trecho de código no local do cursor.</span><span class="sxs-lookup"><span data-stu-id="5d944-363">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

    <span data-ttu-id="5d944-364">![Comece a digitar o nome do trecho](build-restful-apis-with-aspnet-web-api/_static/image29.png "Comece a digitar o nome do trecho")</span><span class="sxs-lookup"><span data-stu-id="5d944-364">![Start typing the snippet name](build-restful-apis-with-aspnet-web-api/_static/image29.png "Start typing the snippet name")</span></span>

    <span data-ttu-id="5d944-365">*Comece a digitar o nome do trecho*</span><span class="sxs-lookup"><span data-stu-id="5d944-365">*Start typing the snippet name*</span></span>

    <span data-ttu-id="5d944-366">![Pressione Tab para selecionar o trecho realçado](build-restful-apis-with-aspnet-web-api/_static/image30.png "Pressione Tab para selecionar o trecho realçado")</span><span class="sxs-lookup"><span data-stu-id="5d944-366">![Press Tab to select the highlighted snippet](build-restful-apis-with-aspnet-web-api/_static/image30.png "Press Tab to select the highlighted snippet")</span></span>

    <span data-ttu-id="5d944-367">*Pressione Tab para selecionar o trecho realçado*</span><span class="sxs-lookup"><span data-stu-id="5d944-367">*Press Tab to select the highlighted snippet*</span></span>

    <span data-ttu-id="5d944-368">![Pressione Tab novamente e o trecho será expandido](build-restful-apis-with-aspnet-web-api/_static/image31.png "Pressione Tab novamente e o trecho será expandido")</span><span class="sxs-lookup"><span data-stu-id="5d944-368">![Press Tab again and the snippet will expand](build-restful-apis-with-aspnet-web-api/_static/image31.png "Press Tab again and the snippet will expand")</span></span>

    <span data-ttu-id="5d944-369">*Pressione Tab novamente e o trecho será expandido*</span><span class="sxs-lookup"><span data-stu-id="5d944-369">*Press Tab again and the snippet will expand*</span></span>

<a id="CodeSnippetUsingMouse"></a>

<a id="To_add_a_code_snippet_using_the_mouse_C_Visual_Basic_and_XML"></a>
### <a name="to-add-a-code-snippet-using-the-mouse-c-visual-basic-and-xml"></a><span data-ttu-id="5d944-370">Para adicionar um trecho de código usando o mouse (C#, Visual Basic e XML)</span><span class="sxs-lookup"><span data-stu-id="5d944-370">To add a code snippet using the mouse (C#, Visual Basic and XML)</span></span>

1. <span data-ttu-id="5d944-371">Clique com o botão direito do mouse no local em que você deseja inserir o trecho de código.</span><span class="sxs-lookup"><span data-stu-id="5d944-371">Right-click where you want to insert the code snippet.</span></span>
2. <span data-ttu-id="5d944-372">Selecione **Inserir trecho** seguido por **meus trechos de código**.</span><span class="sxs-lookup"><span data-stu-id="5d944-372">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
3. <span data-ttu-id="5d944-373">Selecione o trecho relevante na lista clicando nele.</span><span class="sxs-lookup"><span data-stu-id="5d944-373">Pick the relevant snippet from the list, by clicking on it.</span></span>

    <span data-ttu-id="5d944-374">![Clique com o botão direito do mouse em onde você deseja inserir o trecho de código e selecione Inserir trecho](build-restful-apis-with-aspnet-web-api/_static/image32.png "Clique com o botão direito do mouse em onde você deseja inserir o trecho de código e selecione Inserir trecho")</span><span class="sxs-lookup"><span data-stu-id="5d944-374">![Right-click where you want to insert the code snippet and select Insert Snippet](build-restful-apis-with-aspnet-web-api/_static/image32.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

    <span data-ttu-id="5d944-375">*Clique com o botão direito do mouse em onde você deseja inserir o trecho de código e selecione Inserir trecho*</span><span class="sxs-lookup"><span data-stu-id="5d944-375">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

    <span data-ttu-id="5d944-376">![Selecione o trecho relevante na lista clicando nele](build-restful-apis-with-aspnet-web-api/_static/image33.png "Selecione o trecho relevante na lista clicando nele")</span><span class="sxs-lookup"><span data-stu-id="5d944-376">![Pick the relevant snippet from the list, by clicking on it](build-restful-apis-with-aspnet-web-api/_static/image33.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

    <span data-ttu-id="5d944-377">*Selecione o trecho relevante na lista clicando nele*</span><span class="sxs-lookup"><span data-stu-id="5d944-377">*Pick the relevant snippet from the list, by clicking on it*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="5d944-378">Apêndice B: Instalando o Visual Studio Express 2012 para Web</span><span class="sxs-lookup"><span data-stu-id="5d944-378">Appendix B: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="5d944-379">Você pode instalar o **Microsoft Visual Studio Express 2012 para Web** ou &quot; outra &quot; versão Express usando o **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="5d944-379">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="5d944-380">As instruções a seguir orientam você pelas etapas necessárias para instalar o *Visual Studio Express 2012 para Web* usando *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="5d944-380">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="5d944-381">Vá para [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169) .</span><span class="sxs-lookup"><span data-stu-id="5d944-381">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="5d944-382">Como alternativa, se você já tiver instalado o Web Platform Installer, poderá abri-lo e procurar o produto &quot; <em>Visual Studio Express 2012 para Web com o SDK do Azure</em> &quot; .</span><span class="sxs-lookup"><span data-stu-id="5d944-382">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="5d944-383">Clique em **instalar agora**.</span><span class="sxs-lookup"><span data-stu-id="5d944-383">Click on **Install Now**.</span></span> <span data-ttu-id="5d944-384">Se você não tiver **Web Platform Installer** você será redirecionado para baixar e instalá-lo primeiro.</span><span class="sxs-lookup"><span data-stu-id="5d944-384">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="5d944-385">Quando **Web Platform Installer** estiver aberto, clique em **instalar** para iniciar a instalação.</span><span class="sxs-lookup"><span data-stu-id="5d944-385">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="5d944-386">![Instalar Visual Studio Express](build-restful-apis-with-aspnet-web-api/_static/image34.png "Instalar Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="5d944-386">![Install Visual Studio Express](build-restful-apis-with-aspnet-web-api/_static/image34.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="5d944-387">*Instalar Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="5d944-387">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="5d944-388">Leia todos os termos e licenças de produtos e clique em **aceito** para continuar.</span><span class="sxs-lookup"><span data-stu-id="5d944-388">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Aceitando os termos de licença](build-restful-apis-with-aspnet-web-api/_static/image35.png)

    <span data-ttu-id="5d944-390">*Aceitando os termos de licença*</span><span class="sxs-lookup"><span data-stu-id="5d944-390">*Accepting the license terms*</span></span>
5. <span data-ttu-id="5d944-391">Aguarde até que o processo de download e instalação seja concluído.</span><span class="sxs-lookup"><span data-stu-id="5d944-391">Wait until the downloading and installation process completes.</span></span>

    ![Progresso da instalação](build-restful-apis-with-aspnet-web-api/_static/image36.png)

    <span data-ttu-id="5d944-393">*Progresso da instalação*</span><span class="sxs-lookup"><span data-stu-id="5d944-393">*Installation progress*</span></span>
6. <span data-ttu-id="5d944-394">Quando a instalação for concluída, clique em **concluir**.</span><span class="sxs-lookup"><span data-stu-id="5d944-394">When the installation completes, click **Finish**.</span></span>

    ![Instalação concluída](build-restful-apis-with-aspnet-web-api/_static/image37.png)

    <span data-ttu-id="5d944-396">*Instalação concluída*</span><span class="sxs-lookup"><span data-stu-id="5d944-396">*Installation completed*</span></span>
7. <span data-ttu-id="5d944-397">Clique em **sair** para fechar Web Platform Installer.</span><span class="sxs-lookup"><span data-stu-id="5d944-397">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="5d944-398">Para abrir o Visual Studio Express para Web, vá para a tela **Iniciar** e comece a escrever &quot; **vs Express** e &quot; , em seguida, clique no bloco **vs Express para Web** .</span><span class="sxs-lookup"><span data-stu-id="5d944-398">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![Bloco VS Express para Web](build-restful-apis-with-aspnet-web-api/_static/image38.png)

    <span data-ttu-id="5d944-400">*Bloco VS Express para Web*</span><span class="sxs-lookup"><span data-stu-id="5d944-400">*VS Express for Web tile*</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-c-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="5d944-401">Apêndice C: publicando um aplicativo ASP.NET MVC 4 usando Implantação da Web</span><span class="sxs-lookup"><span data-stu-id="5d944-401">Appendix C: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="5d944-402">Este apêndice mostrará como criar um novo site no portal do Azure e publicar o aplicativo obtido seguindo o laboratório, aproveitando o recurso de publicação de Implantação da Web fornecido pelo Azure.</span><span class="sxs-lookup"><span data-stu-id="5d944-402">This appendix will show you how to create a new web site from the Azure Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Azure.</span></span>

<a id="ApxCTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a><span data-ttu-id="5d944-403">Tarefa 1-Criando um novo site no portal do Azure</span><span class="sxs-lookup"><span data-stu-id="5d944-403">Task 1 - Creating a New Web Site from the Azure Portal</span></span>

1. <span data-ttu-id="5d944-404">Vá para o [portal de gerenciamento do Azure](https://manage.windowsazure.com/) e entre usando as credenciais da Microsoft associadas à sua assinatura.</span><span class="sxs-lookup"><span data-stu-id="5d944-404">Go to the [Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5d944-405">Com o Azure, você pode hospedar 10 sites da Web de ASP.NET gratuitamente e, em seguida, dimensionar conforme o tráfego cresce.</span><span class="sxs-lookup"><span data-stu-id="5d944-405">With Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="5d944-406">Você pode se inscrever [aqui](https://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="5d944-406">You can sign up [here](https://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="5d944-407">![Fazer logon no Windows portal do Azure](build-restful-apis-with-aspnet-web-api/_static/image39.png "Fazer logon no Windows portal do Azure")</span><span class="sxs-lookup"><span data-stu-id="5d944-407">![Log on to Windows Azure portal](build-restful-apis-with-aspnet-web-api/_static/image39.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="5d944-408">*Fazer logon no portal*</span><span class="sxs-lookup"><span data-stu-id="5d944-408">*Log on to Portal*</span></span>
2. <span data-ttu-id="5d944-409">Clique em **novo** na barra de comandos.</span><span class="sxs-lookup"><span data-stu-id="5d944-409">Click **New** on the command bar.</span></span>

    <span data-ttu-id="5d944-410">![Criando um novo site](build-restful-apis-with-aspnet-web-api/_static/image40.png "Criando um novo site")</span><span class="sxs-lookup"><span data-stu-id="5d944-410">![Creating a new Web Site](build-restful-apis-with-aspnet-web-api/_static/image40.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="5d944-411">*Criando um novo site*</span><span class="sxs-lookup"><span data-stu-id="5d944-411">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="5d944-412">Clique em **computação**  |  **Web site**.</span><span class="sxs-lookup"><span data-stu-id="5d944-412">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="5d944-413">Em seguida, selecione a opção **criação rápida** .</span><span class="sxs-lookup"><span data-stu-id="5d944-413">Then select **Quick Create** option.</span></span> <span data-ttu-id="5d944-414">Forneça uma URL disponível para o novo site e clique em **criar site**.</span><span class="sxs-lookup"><span data-stu-id="5d944-414">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5d944-415">O Azure é o host para um aplicativo Web em execução na nuvem que você pode controlar e gerenciar.</span><span class="sxs-lookup"><span data-stu-id="5d944-415">Azure is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="5d944-416">A opção criação rápida permite que você implante um aplicativo Web completo no Azure de fora do Portal.</span><span class="sxs-lookup"><span data-stu-id="5d944-416">The Quick Create option allows you to deploy a completed web application to the Azure from outside the portal.</span></span> <span data-ttu-id="5d944-417">Ele não inclui etapas para configurar um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="5d944-417">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="5d944-418">![Criando um novo site usando a criação rápida](build-restful-apis-with-aspnet-web-api/_static/image41.png "Criando um novo site usando a criação rápida")</span><span class="sxs-lookup"><span data-stu-id="5d944-418">![Creating a new Web Site using Quick Create](build-restful-apis-with-aspnet-web-api/_static/image41.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="5d944-419">*Criando um novo site usando a criação rápida*</span><span class="sxs-lookup"><span data-stu-id="5d944-419">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="5d944-420">Aguarde até que o novo **site** seja criado.</span><span class="sxs-lookup"><span data-stu-id="5d944-420">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="5d944-421">Depois que o site for criado, clique no link sob a coluna **URL** .</span><span class="sxs-lookup"><span data-stu-id="5d944-421">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="5d944-422">Verifique se o novo site está funcionando.</span><span class="sxs-lookup"><span data-stu-id="5d944-422">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="5d944-423">![Navegando até o novo site](build-restful-apis-with-aspnet-web-api/_static/image42.png "Navegando até o novo site")</span><span class="sxs-lookup"><span data-stu-id="5d944-423">![Browsing to the new web site](build-restful-apis-with-aspnet-web-api/_static/image42.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="5d944-424">*Navegando até o novo site*</span><span class="sxs-lookup"><span data-stu-id="5d944-424">*Browsing to the new web site*</span></span>

    <span data-ttu-id="5d944-425">![Site em execução](build-restful-apis-with-aspnet-web-api/_static/image43.png "Site em execução")</span><span class="sxs-lookup"><span data-stu-id="5d944-425">![Web site running](build-restful-apis-with-aspnet-web-api/_static/image43.png "Web site running")</span></span>

    <span data-ttu-id="5d944-426">*Site em execução*</span><span class="sxs-lookup"><span data-stu-id="5d944-426">*Web site running*</span></span>
6. <span data-ttu-id="5d944-427">Volte para o portal e clique no nome do site na coluna **nome** para exibir as páginas de gerenciamento.</span><span class="sxs-lookup"><span data-stu-id="5d944-427">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="5d944-428">![Abrindo as páginas de gerenciamento de site](build-restful-apis-with-aspnet-web-api/_static/image44.png "Abrindo as páginas de gerenciamento de site")</span><span class="sxs-lookup"><span data-stu-id="5d944-428">![Opening the web site management pages](build-restful-apis-with-aspnet-web-api/_static/image44.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="5d944-429">*Abrindo as páginas de gerenciamento de site*</span><span class="sxs-lookup"><span data-stu-id="5d944-429">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="5d944-430">Na página **painel** , na seção **visão rápida** , clique no link **baixar perfil de publicação** .</span><span class="sxs-lookup"><span data-stu-id="5d944-430">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5d944-431">O *perfil de publicação* contém todas as informações necessárias para publicar um aplicativo Web em um Azure para cada método de publicação habilitado.</span><span class="sxs-lookup"><span data-stu-id="5d944-431">The *publish profile* contains all of the information required to publish a web application to a Azure for each enabled publication method.</span></span> <span data-ttu-id="5d944-432">O perfil de publicação contém as URLs, as credenciais de usuário e as cadeias de conexão de banco de dados necessárias para conectar-se e autenticar cada um dos pontos de extremidade para os quais um método de publicação é habilitado.</span><span class="sxs-lookup"><span data-stu-id="5d944-432">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="5d944-433">**O Microsoft WebMatrix 2**, **Microsoft Visual Studio Express para Web** e **Microsoft Visual Studio 2012** dão suporte à leitura de perfis de publicação para automatizar a configuração desses programas para a publicação de aplicativos Web no Azure.</span><span class="sxs-lookup"><span data-stu-id="5d944-433">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Azure.</span></span>

    <span data-ttu-id="5d944-434">![Baixando o perfil de publicação do site](build-restful-apis-with-aspnet-web-api/_static/image45.png "Baixando o perfil de publicação do site")</span><span class="sxs-lookup"><span data-stu-id="5d944-434">![Downloading the web site publish profile](build-restful-apis-with-aspnet-web-api/_static/image45.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="5d944-435">*Baixando o perfil de publicação do site*</span><span class="sxs-lookup"><span data-stu-id="5d944-435">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="5d944-436">Baixe o arquivo de perfil de publicação em um local conhecido.</span><span class="sxs-lookup"><span data-stu-id="5d944-436">Download the publish profile file to a known location.</span></span> <span data-ttu-id="5d944-437">Neste exercício, você verá como usar esse arquivo para publicar um aplicativo Web no Azure por meio do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5d944-437">Further in this exercise you will see how to use this file to publish a web application to Azure from Visual Studio.</span></span>

    <span data-ttu-id="5d944-438">![Salvando o arquivo de perfil de publicação](build-restful-apis-with-aspnet-web-api/_static/image46.png "Salvando o perfil de publicação")</span><span class="sxs-lookup"><span data-stu-id="5d944-438">![Saving the publish profile file](build-restful-apis-with-aspnet-web-api/_static/image46.png "Saving the publish profile")</span></span>

    <span data-ttu-id="5d944-439">*Salvando o arquivo de perfil de publicação*</span><span class="sxs-lookup"><span data-stu-id="5d944-439">*Saving the publish profile file*</span></span>

<a id="ApxCTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="5d944-440">Tarefa 2-Configurando o servidor de banco de dados</span><span class="sxs-lookup"><span data-stu-id="5d944-440">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="5d944-441">Se seu aplicativo utiliza bancos de dados SQL Server, você precisará criar um servidor de banco de dados SQL.</span><span class="sxs-lookup"><span data-stu-id="5d944-441">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="5d944-442">Se você quiser implantar um aplicativo simples que não usa SQL Server você pode ignorar essa tarefa.</span><span class="sxs-lookup"><span data-stu-id="5d944-442">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="5d944-443">Você precisará de um servidor do banco de dados SQL para armazenar o banco de dados do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="5d944-443">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="5d944-444">Você pode exibir os servidores do banco de dados SQL de sua assinatura no portal de gerenciamento do Azure no  |    |  **painel do servidor** de servidores de bancos de dados SQL.</span><span class="sxs-lookup"><span data-stu-id="5d944-444">You can view the SQL Database servers from your subscription in the Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="5d944-445">Se você não tiver um servidor criado, poderá criar um usando o botão **Adicionar** na barra de comandos.</span><span class="sxs-lookup"><span data-stu-id="5d944-445">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="5d944-446">Anote o nome do **servidor e a URL, o nome de logon e a senha do administrador**, pois você irá usá-los nas próximas tarefas.</span><span class="sxs-lookup"><span data-stu-id="5d944-446">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="5d944-447">Não crie o banco de dados ainda, pois ele será criado em um estágio posterior.</span><span class="sxs-lookup"><span data-stu-id="5d944-447">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="5d944-448">![Painel do servidor do banco de dados SQL](build-restful-apis-with-aspnet-web-api/_static/image47.png "Painel do servidor do banco de dados SQL")</span><span class="sxs-lookup"><span data-stu-id="5d944-448">![SQL Database Server Dashboard](build-restful-apis-with-aspnet-web-api/_static/image47.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="5d944-449">*Painel do servidor do banco de dados SQL*</span><span class="sxs-lookup"><span data-stu-id="5d944-449">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="5d944-450">Na próxima tarefa, você testará a conexão de banco de dados do Visual Studio, por esse motivo, será necessário incluir o endereço IP local na lista de **endereços IP permitidos** do servidor.</span><span class="sxs-lookup"><span data-stu-id="5d944-450">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="5d944-451">Para fazer isso, clique em **Configurar**, selecione o endereço IP do **endereço IP do cliente atual** e cole-o nas caixas de texto endereço IP **inicial** e **endereço IP final** e clique no ![ botão Adicionar-cliente-IP-endereço-OK-botão ](build-restful-apis-with-aspnet-web-api/_static/image48.png) .</span><span class="sxs-lookup"><span data-stu-id="5d944-451">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](build-restful-apis-with-aspnet-web-api/_static/image48.png) button.</span></span>

    ![Adicionando endereço IP do cliente](build-restful-apis-with-aspnet-web-api/_static/image49.png)

    <span data-ttu-id="5d944-453">*Adicionando endereço IP do cliente*</span><span class="sxs-lookup"><span data-stu-id="5d944-453">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="5d944-454">Depois que o **endereço IP do cliente** for adicionado à lista endereços IP permitidos, clique em **salvar** para confirmar as alterações.</span><span class="sxs-lookup"><span data-stu-id="5d944-454">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Confirmar alterações](build-restful-apis-with-aspnet-web-api/_static/image50.png)

    <span data-ttu-id="5d944-456">*Confirmar alterações*</span><span class="sxs-lookup"><span data-stu-id="5d944-456">*Confirm Changes*</span></span>

<a id="ApxCTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="5d944-457">Tarefa 3-publicando um aplicativo ASP.NET MVC 4 usando Implantação da Web</span><span class="sxs-lookup"><span data-stu-id="5d944-457">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="5d944-458">Volte para a solução ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="5d944-458">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="5d944-459">Na **Gerenciador de soluções**, clique com o botão direito do mouse no projeto de site e selecione **publicar**.</span><span class="sxs-lookup"><span data-stu-id="5d944-459">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="5d944-460">![Publicando o aplicativo](build-restful-apis-with-aspnet-web-api/_static/image51.png "Publicando o aplicativo")</span><span class="sxs-lookup"><span data-stu-id="5d944-460">![Publishing the Application](build-restful-apis-with-aspnet-web-api/_static/image51.png "Publishing the Application")</span></span>

    <span data-ttu-id="5d944-461">*Publicando o site*</span><span class="sxs-lookup"><span data-stu-id="5d944-461">*Publishing the web site*</span></span>
2. <span data-ttu-id="5d944-462">Importe o perfil de publicação salvo na primeira tarefa.</span><span class="sxs-lookup"><span data-stu-id="5d944-462">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="5d944-463">![Importando o perfil de publicação](build-restful-apis-with-aspnet-web-api/_static/image52.png "Importando o perfil de publicação")</span><span class="sxs-lookup"><span data-stu-id="5d944-463">![Importing the publish profile](build-restful-apis-with-aspnet-web-api/_static/image52.png "Importing the publish profile")</span></span>

    <span data-ttu-id="5d944-464">*Importando perfil de publicação*</span><span class="sxs-lookup"><span data-stu-id="5d944-464">*Importing publish profile*</span></span>
3. <span data-ttu-id="5d944-465">Clique em **validar conexão**.</span><span class="sxs-lookup"><span data-stu-id="5d944-465">Click **Validate Connection**.</span></span> <span data-ttu-id="5d944-466">Depois que a validação for concluída, clique em **Avançar**.</span><span class="sxs-lookup"><span data-stu-id="5d944-466">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5d944-467">A validação será concluída quando você vir uma marca de seleção verde ao lado do botão Validar conexão.</span><span class="sxs-lookup"><span data-stu-id="5d944-467">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="5d944-468">![Validando conexão](build-restful-apis-with-aspnet-web-api/_static/image53.png "Validando conexão")</span><span class="sxs-lookup"><span data-stu-id="5d944-468">![Validating connection](build-restful-apis-with-aspnet-web-api/_static/image53.png "Validating connection")</span></span>

    <span data-ttu-id="5d944-469">*Validando conexão*</span><span class="sxs-lookup"><span data-stu-id="5d944-469">*Validating connection*</span></span>
4. <span data-ttu-id="5d944-470">Na página **configurações** , na seção **bancos** de dados, clique no botão ao lado da caixa de texto da sua conexão de banco (ou seja, **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="5d944-470">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="5d944-471">![Configuração de implantação da Web](build-restful-apis-with-aspnet-web-api/_static/image54.png "Configuração de implantação da Web")</span><span class="sxs-lookup"><span data-stu-id="5d944-471">![Web deploy configuration](build-restful-apis-with-aspnet-web-api/_static/image54.png "Web deploy configuration")</span></span>

    <span data-ttu-id="5d944-472">*Configuração de implantação da Web*</span><span class="sxs-lookup"><span data-stu-id="5d944-472">*Web deploy configuration*</span></span>
5. <span data-ttu-id="5d944-473">Configure a conexão de banco de dados da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="5d944-473">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="5d944-474">No **nome do servidor** , digite a URL do servidor do banco de dados SQL usando o prefixo *TCP:* .</span><span class="sxs-lookup"><span data-stu-id="5d944-474">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="5d944-475">Em **nome de usuário** , digite o nome de logon do administrador do servidor.</span><span class="sxs-lookup"><span data-stu-id="5d944-475">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="5d944-476">Em **senha** , digite a senha de logon do administrador do servidor.</span><span class="sxs-lookup"><span data-stu-id="5d944-476">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="5d944-477">Digite um novo nome de banco de dados, por exemplo: *MVC4SampleDB*.</span><span class="sxs-lookup"><span data-stu-id="5d944-477">Type a new database name, for example: *MVC4SampleDB*.</span></span>

     <span data-ttu-id="5d944-478">![Configurando a cadeia de conexão de destino](build-restful-apis-with-aspnet-web-api/_static/image55.png "Configurando a cadeia de conexão de destino")</span><span class="sxs-lookup"><span data-stu-id="5d944-478">![Configuring destination connection string](build-restful-apis-with-aspnet-web-api/_static/image55.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="5d944-479">*Configurando a cadeia de conexão de destino*</span><span class="sxs-lookup"><span data-stu-id="5d944-479">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="5d944-480">Em seguida, clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="5d944-480">Then click **OK**.</span></span> <span data-ttu-id="5d944-481">Quando for solicitado a criar o banco de dados, clique em **Sim**.</span><span class="sxs-lookup"><span data-stu-id="5d944-481">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="5d944-482">![Criando o banco de dados](build-restful-apis-with-aspnet-web-api/_static/image56.png "Criando a cadeia de caracteres do banco de dados")</span><span class="sxs-lookup"><span data-stu-id="5d944-482">![Creating the database](build-restful-apis-with-aspnet-web-api/_static/image56.png "Creating the database string")</span></span>

    <span data-ttu-id="5d944-483">*Criando o banco de dados*</span><span class="sxs-lookup"><span data-stu-id="5d944-483">*Creating the database*</span></span>
7. <span data-ttu-id="5d944-484">A cadeia de conexão que você usará para se conectar ao banco de dados SQL no Windows Azure é mostrada na caixa de texto conexão padrão.</span><span class="sxs-lookup"><span data-stu-id="5d944-484">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="5d944-485">Em seguida, clique em **Próximo**.</span><span class="sxs-lookup"><span data-stu-id="5d944-485">Then click **Next**.</span></span>

    <span data-ttu-id="5d944-486">![Cadeia de conexão apontando para o banco de dados SQL](build-restful-apis-with-aspnet-web-api/_static/image57.png "Cadeia de conexão apontando para o banco de dados SQL")</span><span class="sxs-lookup"><span data-stu-id="5d944-486">![Connection string pointing to SQL Database](build-restful-apis-with-aspnet-web-api/_static/image57.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="5d944-487">*Cadeia de conexão apontando para o banco de dados SQL*</span><span class="sxs-lookup"><span data-stu-id="5d944-487">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="5d944-488">Na página **Visualização** , clique em **publicar**.</span><span class="sxs-lookup"><span data-stu-id="5d944-488">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="5d944-489">![Publicando o aplicativo Web](build-restful-apis-with-aspnet-web-api/_static/image58.png "Publicando o aplicativo Web")</span><span class="sxs-lookup"><span data-stu-id="5d944-489">![Publishing the web application](build-restful-apis-with-aspnet-web-api/_static/image58.png "Publishing the web application")</span></span>

    <span data-ttu-id="5d944-490">*Publicando o aplicativo Web*</span><span class="sxs-lookup"><span data-stu-id="5d944-490">*Publishing the web application*</span></span>
9. <span data-ttu-id="5d944-491">Quando o processo de publicação for concluído, o navegador padrão abrirá o site publicado.</span><span class="sxs-lookup"><span data-stu-id="5d944-491">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="5d944-492">![Aplicativo publicado no Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image59.png "Aplicativo publicado no Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="5d944-492">![Application published to Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image59.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="5d944-493">*Aplicativo publicado no Azure*</span><span class="sxs-lookup"><span data-stu-id="5d944-493">*Application published to Azure*</span></span>
