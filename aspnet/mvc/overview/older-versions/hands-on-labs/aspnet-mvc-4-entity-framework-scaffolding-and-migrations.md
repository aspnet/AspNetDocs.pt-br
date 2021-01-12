---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
title: ASP.NET MVC 4 Entity Framework scaffolding e migrações | Microsoft Docs
author: rick-anderson
description: Se você estiver familiarizado com os métodos do controlador ASP.NET MVC 4 ou tiver concluído os &quot; auxiliares, formulários e validação &quot; Hands-On laboratório, você deve estar ciente...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 093c1362-f10b-407c-a708-be370f4b62b0
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
msc.type: authoredcontent
ms.openlocfilehash: 58aafe862f56ab13bf5a6bf8908a3baf582a08ff
ms.sourcegitcommit: d4e2a07eeb2cdf19f0bfbfab4a469970bc7e1c99
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/12/2021
ms.locfileid: "98105267"
---
# <a name="aspnet-mvc-4-entity-framework-scaffolding-and-migrations"></a><span data-ttu-id="1c86e-103">Migrações e scaffolding do Entity Framework do ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="1c86e-103">ASP.NET MVC 4 Entity Framework Scaffolding and Migrations</span></span>

<span data-ttu-id="1c86e-104">Por [equipe de acampamentos da Web](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="1c86e-104">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="1c86e-105">Baixe o kit de treinamento do Web acampamentos</span><span class="sxs-lookup"><span data-stu-id="1c86e-105">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="1c86e-106">Se você estiver familiarizado com os métodos do controlador ASP.NET MVC 4 ou tiver concluído os &quot; auxiliares, formulários e validação &quot; Hands-On laboratório, você deve estar ciente de que grande parte da lógica para criar, atualizar, listar e remover qualquer entidade de dados é repetida em todo o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1c86e-106">If you are familiar with ASP.NET MVC 4 controller methods, or have completed the &quot;Helpers, Forms and Validation&quot; Hands-On lab, you should be aware that much of the logic to create, update, list and remove any data entity is repeated throughout the application.</span></span> <span data-ttu-id="1c86e-107">Para não mencionar que, se seu modelo tiver várias classes a serem manipuladas, você provavelmente gastará um tempo considerável escrevendo os métodos POST e GET de ação para cada operação de entidade, bem como cada uma das exibições.</span><span class="sxs-lookup"><span data-stu-id="1c86e-107">Not to mention that, if your model has several classes to manipulate, you will be likely to spend a considerable time writing the POST and GET action methods for each entity operation, as well as each of the views.</span></span>

<span data-ttu-id="1c86e-108">Neste laboratório, você aprenderá a usar o ASP.NET MVC 4 scaffolding para gerar automaticamente a linha de base do CRUD do seu aplicativo (criar, ler, atualizar e excluir).</span><span class="sxs-lookup"><span data-stu-id="1c86e-108">In this lab you will learn how to use the ASP.NET MVC 4 scaffolding to automatically generate the baseline of your application's CRUD (Create, Read, Update and Delete).</span></span> <span data-ttu-id="1c86e-109">A partir de uma classe de modelo simples e, sem escrever uma única linha de código, você criará um controlador que conterá todas as operações CRUD, bem como todas as exibições necessárias.</span><span class="sxs-lookup"><span data-stu-id="1c86e-109">Starting from a simple model class, and, without writing a single line of code, you will create a controller that will contain all the CRUD operations, as well as the all the necessary views.</span></span> <span data-ttu-id="1c86e-110">Depois de criar e executar a solução simples, você terá o banco de dados do aplicativo gerado, junto com a lógica MVC e exibições para manipulação de dados.</span><span class="sxs-lookup"><span data-stu-id="1c86e-110">After building and running the simple solution, you will have the application database generated, together with the MVC logic and views for data manipulation.</span></span>

<span data-ttu-id="1c86e-111">Além disso, você aprenderá como é fácil usar Entity Framework migrações para executar atualizações de modelo em todo o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1c86e-111">In addition, you will learn how easy it is to use Entity Framework Migrations to perform model updates throughout your entire application.</span></span> <span data-ttu-id="1c86e-112">Entity Framework migrações permitirão que você modifique seu banco de dados depois que o modelo for alterado com etapas simples.</span><span class="sxs-lookup"><span data-stu-id="1c86e-112">Entity Framework Migrations will let you modify your database after the model has changed with simple steps.</span></span> <span data-ttu-id="1c86e-113">Com tudo isso em mente, você poderá criar e manter aplicativos Web com mais eficiência, aproveitando os recursos mais recentes do ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="1c86e-113">With all these in mind, you will be able to build and maintain web applications more efficiently, taking advantage of the latest features of ASP.NET MVC 4.</span></span>

> [!NOTE]
> <span data-ttu-id="1c86e-114">Todos os códigos de exemplo e trechos de código estão incluídos no Web acampamentos Training Kit, disponível em [Microsoft-Web/WebCampTrainingKit releases](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="1c86e-114">All sample code and snippets are included in the Web Camps Training Kit, available from at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="1c86e-115">O projeto específico deste laboratório está disponível em [ASP.NET MVC 4 Entity Framework scaffolding e migrações](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations).</span><span class="sxs-lookup"><span data-stu-id="1c86e-115">The project specific to this lab is available at [ASP.NET MVC 4 Entity Framework Scaffolding and Migrations](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="1c86e-116">Objetivos</span><span class="sxs-lookup"><span data-stu-id="1c86e-116">Objectives</span></span>

<span data-ttu-id="1c86e-117">Neste Hands-On laboratório, você aprenderá a:</span><span class="sxs-lookup"><span data-stu-id="1c86e-117">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="1c86e-118">Use ASP.NET scaffolding para operações CRUD em controladores.</span><span class="sxs-lookup"><span data-stu-id="1c86e-118">Use ASP.NET scaffolding for CRUD operations in controllers.</span></span>
- <span data-ttu-id="1c86e-119">Altere o modelo de banco de dados usando migrações Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="1c86e-119">Change the database model using Entity Framework Migrations.</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="1c86e-120">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="1c86e-120">Prerequisites</span></span>

<span data-ttu-id="1c86e-121">Você deve ter os seguintes itens para concluir este laboratório:</span><span class="sxs-lookup"><span data-stu-id="1c86e-121">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="1c86e-122">[Microsoft Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) ou superior (leia [o apêndice a](#AppendixA) para obter instruções sobre como instalá-lo).</span><span class="sxs-lookup"><span data-stu-id="1c86e-122">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="1c86e-123">Instalação</span><span class="sxs-lookup"><span data-stu-id="1c86e-123">Setup</span></span>

<span data-ttu-id="1c86e-124">**Instalando trechos de código**</span><span class="sxs-lookup"><span data-stu-id="1c86e-124">**Installing Code Snippets**</span></span>

<span data-ttu-id="1c86e-125">Para sua conveniência, grande parte do código que você gerenciará ao longo deste laboratório está disponível como trechos de código do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1c86e-125">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="1c86e-126">Para instalar os trechos de código, execute o arquivo **.\Source\Setup\CodeSnippets.VSI** .</span><span class="sxs-lookup"><span data-stu-id="1c86e-126">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="1c86e-127">Se você não estiver familiarizado com os trechos de Visual Studio Code e quiser saber como usá-los, consulte o apêndice deste documento &quot; [Apêndice B: usando trechos de código](#AppendixB) &quot; .</span><span class="sxs-lookup"><span data-stu-id="1c86e-127">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

---

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="1c86e-128">Exercícios</span><span class="sxs-lookup"><span data-stu-id="1c86e-128">Exercises</span></span>

<span data-ttu-id="1c86e-129">O exercício a seguir compõe este Hands-On Lab:</span><span class="sxs-lookup"><span data-stu-id="1c86e-129">The following exercise make up this Hands-On Lab:</span></span>

1. [<span data-ttu-id="1c86e-130">Usando o ASP.NET MVC 4 scaffolding com migrações Entity Framework</span><span class="sxs-lookup"><span data-stu-id="1c86e-130">Using ASP.NET MVC 4 Scaffolding with Entity Framework Migrations</span></span>](#Exercise1)

> [!NOTE]
> <span data-ttu-id="1c86e-131">Este exercício é acompanhado por uma pasta **final** que contém a solução resultante que você deve obter depois de concluir o exercício.</span><span class="sxs-lookup"><span data-stu-id="1c86e-131">This exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercise.</span></span> <span data-ttu-id="1c86e-132">Você pode usar essa solução como um guia se precisar de ajuda adicional para trabalhar com o exercício.</span><span class="sxs-lookup"><span data-stu-id="1c86e-132">You can use this solution as a guide if you need additional help working through the exercise.</span></span>

<span data-ttu-id="1c86e-133">Tempo estimado para concluir este laboratório: **30 minutos**</span><span class="sxs-lookup"><span data-stu-id="1c86e-133">Estimated time to complete this lab: **30 minutes**</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Using_ASPNET_MVC_4_Scaffolding_with_Entity_Framework_Migrations"></a>
### <a name="exercise-1-using-aspnet-mvc-4-scaffolding-with-entity-framework-migrations"></a><span data-ttu-id="1c86e-134">Exercício 1: usando o ASP.NET MVC 4 scaffolding com migrações Entity Framework</span><span class="sxs-lookup"><span data-stu-id="1c86e-134">Exercise 1: Using ASP.NET MVC 4 Scaffolding with Entity Framework Migrations</span></span>

<span data-ttu-id="1c86e-135">O ASP.NET MVC scaffolding fornece uma maneira rápida de gerar as operações CRUD de forma padronizada, criando a lógica necessária que permite que o aplicativo interaja com a camada de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="1c86e-135">ASP.NET MVC scaffolding provides a quick way to generate the CRUD operations in a standardized way, creating the necessary logic that lets your application interact with the database layer.</span></span>

<span data-ttu-id="1c86e-136">Neste exercício, você aprenderá a usar o ASP.NET MVC 4 scaffolding com o Code First para criar os métodos CRUD.</span><span class="sxs-lookup"><span data-stu-id="1c86e-136">In this exercise, you will learn how to use ASP.NET MVC 4 scaffolding with code first to create the CRUD methods.</span></span> <span data-ttu-id="1c86e-137">Em seguida, você aprenderá a atualizar seu modelo aplicando as alterações no banco de dados usando Entity Framework migrações.</span><span class="sxs-lookup"><span data-stu-id="1c86e-137">Then, you will learn how to update your model applying the changes in the database by using Entity Framework Migrations.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1-_Creating_a_new_ASPNET_MVC_4_project_using_Scaffolding"></a>
#### <a name="task-1--creating-a-new-aspnet-mvc-4-project-using-scaffolding"></a><span data-ttu-id="1c86e-138">Tarefa 1-Criando um novo projeto do MVC 4 do ASP.NET usando scaffolding</span><span class="sxs-lookup"><span data-stu-id="1c86e-138">Task 1- Creating a new ASP.NET MVC 4 project using Scaffolding</span></span>

1. <span data-ttu-id="1c86e-139">Se ainda não estiver aberto, inicie o **Visual Studio 2012**.</span><span class="sxs-lookup"><span data-stu-id="1c86e-139">If not already open, start **Visual Studio 2012**.</span></span>
2. <span data-ttu-id="1c86e-140">Selecionar **arquivo | Novo projeto**.</span><span class="sxs-lookup"><span data-stu-id="1c86e-140">Select **File | New Project**.</span></span> <span data-ttu-id="1c86e-141">Na caixa de diálogo novo projeto, no **Visual C# |** , Selecione **aplicativo web do ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="1c86e-141">In the New Project dialog, under the **Visual C# | Web** section, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="1c86e-142">Nomeie o projeto para **MVC4andEFMigrations** e defina o local como **Source\Ex1-UsingMVC4ScaffoldingEFMigrations** pasta deste laboratório.</span><span class="sxs-lookup"><span data-stu-id="1c86e-142">Name the project to **MVC4andEFMigrations** and set the location to **Source\Ex1-UsingMVC4ScaffoldingEFMigrations** folder of this lab.</span></span> <span data-ttu-id="1c86e-143">Defina o **nome da solução** como **Iniciar** e verifique se a opção **criar diretório para solução** está marcada.</span><span class="sxs-lookup"><span data-stu-id="1c86e-143">Set the **Solution name** to **Begin** and ensure **Create directory for solution** is checked.</span></span> <span data-ttu-id="1c86e-144">Clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="1c86e-144">Click **OK**.</span></span>

    <span data-ttu-id="1c86e-145">![Caixa de diálogo novo projeto do ASP.NET MVC 4](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "Caixa de diálogo novo projeto do ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="1c86e-145">![New ASP.NET MVC 4 Project Dialog Box](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "New ASP.NET MVC 4 Project Dialog Box")</span></span>

    <span data-ttu-id="1c86e-146">*Caixa de diálogo novo projeto do ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="1c86e-146">*New ASP.NET MVC 4 Project Dialog Box*</span></span>
3. <span data-ttu-id="1c86e-147">Na caixa de diálogo **novo projeto do ASP.NET MVC 4** , selecione o modelo de **aplicativo da Internet** e verifique se o **Razor** é o **mecanismo de exibição** selecionado.</span><span class="sxs-lookup"><span data-stu-id="1c86e-147">In the **New ASP.NET MVC 4 Project** dialog box select the **Internet Application** template, and make sure that **Razor** is the selected **View engine**.</span></span> <span data-ttu-id="1c86e-148">Clique em **OK** para criar o projeto.</span><span class="sxs-lookup"><span data-stu-id="1c86e-148">Click **OK** to create the project.</span></span>

    <span data-ttu-id="1c86e-149">![Novo aplicativo de Internet ASP.NET MVC 4](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "Novo aplicativo de Internet ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="1c86e-149">![New ASP.NET MVC 4 Internet Application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "New ASP.NET MVC 4 Internet Application")</span></span>

    <span data-ttu-id="1c86e-150">*Novo aplicativo de Internet ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="1c86e-150">*New ASP.NET MVC 4 Internet Application*</span></span>
4. <span data-ttu-id="1c86e-151">Na Gerenciador de Soluções, clique com o botão direito do mouse em **modelos** e selecione **Adicionar | Classe** para criar uma pessoa de classe simples (POCO).</span><span class="sxs-lookup"><span data-stu-id="1c86e-151">In the Solution Explorer, right-click **Models** and select **Add | Class** to create a simple class person (POCO).</span></span> <span data-ttu-id="1c86e-152">Nomeie-a como **pessoa** e clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="1c86e-152">Name it **Person** and click **OK**.</span></span>
5. <span data-ttu-id="1c86e-153">Abra a classe Person e insira as propriedades a seguir.</span><span class="sxs-lookup"><span data-stu-id="1c86e-153">Open the Person class and insert the following properties.</span></span>

    <span data-ttu-id="1c86e-154">(Trecho de código- *ASP.NET MVC 4 e Entity Framework migrações-EX1 Propriedades da pessoa*)</span><span class="sxs-lookup"><span data-stu-id="1c86e-154">(Code Snippet - *ASP.NET MVC 4 and Entity Framework Migrations - Ex1 Person Properties*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample1.cs)]
6. <span data-ttu-id="1c86e-155">Clique em **Compilar | Compile a solução** para salvar as alterações e compilar o projeto.</span><span class="sxs-lookup"><span data-stu-id="1c86e-155">Click **Build | Build Solution** to save the changes and build the project.</span></span>

    <span data-ttu-id="1c86e-156">![Compilando o aplicativo](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "Compilando o aplicativo")</span><span class="sxs-lookup"><span data-stu-id="1c86e-156">![Building the application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "Building the application")</span></span>

    <span data-ttu-id="1c86e-157">*Compilando o aplicativo*</span><span class="sxs-lookup"><span data-stu-id="1c86e-157">*Building the Application*</span></span>
7. <span data-ttu-id="1c86e-158">Na Gerenciador de Soluções, clique com o botão direito do mouse na pasta controladores e selecione **Adicionar | Controlador**.</span><span class="sxs-lookup"><span data-stu-id="1c86e-158">In the Solution Explorer, right-click the controllers folder and select **Add | Controller**.</span></span>
8. <span data-ttu-id="1c86e-159">Nomeie o controlador *PersonController* e conclua as **Opções de scaffolding** com os valores a seguir.</span><span class="sxs-lookup"><span data-stu-id="1c86e-159">Name the controller *PersonController* and complete the **Scaffolding options** with the following values.</span></span>

   1. <span data-ttu-id="1c86e-160">Na lista suspensa **modelo** , selecione o **controlador MVC com ações de leitura/gravação e exibições, usando Entity Framework** opção.</span><span class="sxs-lookup"><span data-stu-id="1c86e-160">In the **Template** drop-down list, select the **MVC controller with read/write actions and views, using Entity Framework** option.</span></span>
   2. <span data-ttu-id="1c86e-161">Na lista suspensa **classe de modelo** , selecione a classe **Person** .</span><span class="sxs-lookup"><span data-stu-id="1c86e-161">In the **Model class** drop-down list, select the **Person** class.</span></span>
   3. <span data-ttu-id="1c86e-162">Na lista **classe de contexto de dados** , selecione **&lt; novo contexto de dados. &gt; ..**.</span><span class="sxs-lookup"><span data-stu-id="1c86e-162">In the **Data Context class** list, select **&lt;New data context...&gt;**.</span></span> <span data-ttu-id="1c86e-163">Escolha qualquer nome e clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="1c86e-163">Choose any name and click **OK**.</span></span>
   4. <span data-ttu-id="1c86e-164">Na lista suspensa **exibições** , verifique se o **Razor** está selecionado.</span><span class="sxs-lookup"><span data-stu-id="1c86e-164">In the **Views** drop-down list, make sure that **Razor** is selected.</span></span>

      <span data-ttu-id="1c86e-165">![Adicionando o controlador Person com scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "Adicionando o controlador Person com scaffolding")</span><span class="sxs-lookup"><span data-stu-id="1c86e-165">![Adding the Person controller with scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "Adding the Person controller with scaffolding")</span></span>

      <span data-ttu-id="1c86e-166">*Adicionando o controlador Person com scaffolding*</span><span class="sxs-lookup"><span data-stu-id="1c86e-166">*Adding the Person controller with scaffolding*</span></span>
9. <span data-ttu-id="1c86e-167">Clique em **Adicionar** para criar o novo controlador para pessoa com scaffolding.</span><span class="sxs-lookup"><span data-stu-id="1c86e-167">Click **Add** to create the new controller for Person with scaffolding.</span></span> <span data-ttu-id="1c86e-168">Agora, você gerou as ações do controlador, bem como as exibições.</span><span class="sxs-lookup"><span data-stu-id="1c86e-168">You have now generated the controller actions as well as the views.</span></span>

    <span data-ttu-id="1c86e-169">![Depois de criar o controlador Person com scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "Depois de criar o controlador Person com scaffolding")</span><span class="sxs-lookup"><span data-stu-id="1c86e-169">![After creating the Person controller with scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "After creating the Person controller with scaffolding")</span></span>

    <span data-ttu-id="1c86e-170">*Depois de criar o controlador Person com scaffolding*</span><span class="sxs-lookup"><span data-stu-id="1c86e-170">*After creating the Person controller with scaffolding*</span></span>
10. <span data-ttu-id="1c86e-171">Abra a classe **PersonController** .</span><span class="sxs-lookup"><span data-stu-id="1c86e-171">Open **PersonController** class.</span></span> <span data-ttu-id="1c86e-172">Observe que os métodos de ação CRUD completos foram gerados automaticamente.</span><span class="sxs-lookup"><span data-stu-id="1c86e-172">Notice that the full CRUD action methods have been generated automatically.</span></span>

   <span data-ttu-id="1c86e-173">![Dentro do controlador Person](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "Dentro do controlador Person")</span><span class="sxs-lookup"><span data-stu-id="1c86e-173">![Inside the Person controller](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "Inside the Person controller")</span></span>

   <span data-ttu-id="1c86e-174">*Dentro do controlador Person*</span><span class="sxs-lookup"><span data-stu-id="1c86e-174">*Inside the Person controller*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2-_Running_the_application"></a>
#### <a name="task-2--running-the-application"></a><span data-ttu-id="1c86e-175">Tarefa 2-executando o aplicativo</span><span class="sxs-lookup"><span data-stu-id="1c86e-175">Task 2- Running the application</span></span>

<span data-ttu-id="1c86e-176">Neste ponto, o banco de dados ainda não foi criado.</span><span class="sxs-lookup"><span data-stu-id="1c86e-176">At this point, the database is not yet created.</span></span> <span data-ttu-id="1c86e-177">Nesta tarefa, você executará o aplicativo pela primeira vez e testará as operações CRUD.</span><span class="sxs-lookup"><span data-stu-id="1c86e-177">In this task, you will run the application for the first time and test the CRUD operations.</span></span> <span data-ttu-id="1c86e-178">O banco de dados será criado instantaneamente com Code First.</span><span class="sxs-lookup"><span data-stu-id="1c86e-178">The database will be created on the fly with Code First.</span></span>

1. <span data-ttu-id="1c86e-179">Pressione **F5** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1c86e-179">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="1c86e-180">No navegador, adicione **/Person** à URL para abrir a página pessoa.</span><span class="sxs-lookup"><span data-stu-id="1c86e-180">In the browser, add **/Person** to the URL to open the Person page.</span></span>

    <span data-ttu-id="1c86e-181">![Execução do aplicativo pela primeira vez](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "Execução do aplicativo pela primeira vez")</span><span class="sxs-lookup"><span data-stu-id="1c86e-181">![Application first run](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "Application first run")</span></span>

    <span data-ttu-id="1c86e-182">*Aplicativo: primeira execução*</span><span class="sxs-lookup"><span data-stu-id="1c86e-182">*Application: first run*</span></span>
3. <span data-ttu-id="1c86e-183">Agora, você explorará as páginas da pessoa e testará as operações CRUD.</span><span class="sxs-lookup"><span data-stu-id="1c86e-183">You will now explore the Person pages and test the CRUD operations.</span></span>

    1. <span data-ttu-id="1c86e-184">Clique em **criar novo** para adicionar uma nova pessoa.</span><span class="sxs-lookup"><span data-stu-id="1c86e-184">Click **Create New** to add a new person.</span></span> <span data-ttu-id="1c86e-185">Insira um nome e sobrenome e clique em **criar**.</span><span class="sxs-lookup"><span data-stu-id="1c86e-185">Enter a first name and a last name and click **Create**.</span></span>

        <span data-ttu-id="1c86e-186">![Adicionando uma nova pessoa](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "Adicionando uma nova pessoa")</span><span class="sxs-lookup"><span data-stu-id="1c86e-186">![Adding a new person](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "Adding a new person")</span></span>

        <span data-ttu-id="1c86e-187">*Adicionando uma nova pessoa*</span><span class="sxs-lookup"><span data-stu-id="1c86e-187">*Adding a new person*</span></span>
    2. <span data-ttu-id="1c86e-188">Na lista da pessoa, você pode excluir, editar ou adicionar itens.</span><span class="sxs-lookup"><span data-stu-id="1c86e-188">In the person's list, you can delete, edit or add items.</span></span>

        <span data-ttu-id="1c86e-189">![lista de pessoas](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "lista de pessoas")</span><span class="sxs-lookup"><span data-stu-id="1c86e-189">![person list](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "person list")</span></span>

        <span data-ttu-id="1c86e-190">*Lista de pessoas*</span><span class="sxs-lookup"><span data-stu-id="1c86e-190">*Person list*</span></span>
    3. <span data-ttu-id="1c86e-191">Clique em **detalhes** para abrir os detalhes da pessoa.</span><span class="sxs-lookup"><span data-stu-id="1c86e-191">Click **Details** to open the person's details.</span></span>

        <span data-ttu-id="1c86e-192">![Detalhes da pessoa](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "Detalhes da pessoa")</span><span class="sxs-lookup"><span data-stu-id="1c86e-192">![Person's details](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "Person's details")</span></span>

        <span data-ttu-id="1c86e-193">*Detalhes da pessoa*</span><span class="sxs-lookup"><span data-stu-id="1c86e-193">*Person's details*</span></span>
4. <span data-ttu-id="1c86e-194">Feche o navegador e retorne ao Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1c86e-194">Close the browser and return to Visual Studio.</span></span> <span data-ttu-id="1c86e-195">Observe que você criou todo o CRUD para a entidade Person em todo o seu aplicativo – do modelo para as exibições, sem precisar escrever uma única linha de código!</span><span class="sxs-lookup"><span data-stu-id="1c86e-195">Notice that you have created the whole CRUD for the person entity throughout your application -from the model to the views- without having to write a single line of code!</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3-_Updating_the_database_using_Entity_Framework_Migrations"></a>
#### <a name="task-3--updating-the-database-using-entity-framework-migrations"></a><span data-ttu-id="1c86e-196">Tarefa 3-atualizando o banco de dados usando migrações Entity Framework</span><span class="sxs-lookup"><span data-stu-id="1c86e-196">Task 3- Updating the database using Entity Framework Migrations</span></span>

<span data-ttu-id="1c86e-197">Nesta tarefa, você atualizará o banco de dados usando Entity Framework migrações.</span><span class="sxs-lookup"><span data-stu-id="1c86e-197">In this task you will update the database using Entity Framework Migrations.</span></span> <span data-ttu-id="1c86e-198">Você descobrirá como é fácil alterar o modelo e refletir as alterações em seus bancos de dados usando o recurso de migrações Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="1c86e-198">You will discover how easy it is to change the model and reflect the changes in your databases by using the Entity Framework Migrations feature.</span></span>

1. <span data-ttu-id="1c86e-199">Abra o Console do Gerenciador de Pacotes.</span><span class="sxs-lookup"><span data-stu-id="1c86e-199">Open the Package Manager Console.</span></span> <span data-ttu-id="1c86e-200">Selecione **Ferramentas** > **Gerenciador de Pacotes NuGet** > **Console do Gerenciador de Pacotes**.</span><span class="sxs-lookup"><span data-stu-id="1c86e-200">Select **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>
2. <span data-ttu-id="1c86e-201">No Console do Gerenciador de Pacotes, digite o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="1c86e-201">In the Package Manager Console, enter the following command:</span></span>

    <span data-ttu-id="1c86e-202">PMC</span><span class="sxs-lookup"><span data-stu-id="1c86e-202">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample2.ps1)]

    <span data-ttu-id="1c86e-203">![Habilitando as migrações](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "Habilitando as migrações")</span><span class="sxs-lookup"><span data-stu-id="1c86e-203">![Enabling Migrations](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "Enabling Migrations")</span></span>

    <span data-ttu-id="1c86e-204">*Habilitando migrações*</span><span class="sxs-lookup"><span data-stu-id="1c86e-204">*Enabling migrations*</span></span>

    <span data-ttu-id="1c86e-205">O comando Enable-Migration cria a pasta **migrações** , que contém um script para inicializar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="1c86e-205">The Enable-Migration command creates the **Migrations** folder, which contains a script to initialize the database.</span></span>

    <span data-ttu-id="1c86e-206">![Pasta de migrações](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "Pasta de migrações")</span><span class="sxs-lookup"><span data-stu-id="1c86e-206">![Migrations folder](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "Migrations folder")</span></span>

    <span data-ttu-id="1c86e-207">*Pasta de migrações*</span><span class="sxs-lookup"><span data-stu-id="1c86e-207">*Migrations folder*</span></span>
3. <span data-ttu-id="1c86e-208">Abra o arquivo **Configuration.cs** na pasta Migrations.</span><span class="sxs-lookup"><span data-stu-id="1c86e-208">Open the **Configuration.cs** file in the Migrations folder.</span></span> <span data-ttu-id="1c86e-209">Localize o construtor da classe e altere o valor **AutomaticMigrationsEnabled** para *true*.</span><span class="sxs-lookup"><span data-stu-id="1c86e-209">Locate the class constructor and change the **AutomaticMigrationsEnabled** value to *true*.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample3.cs)]
4. <span data-ttu-id="1c86e-210">Abra a classe Person e adicione um atributo para o nome do meio da pessoa.</span><span class="sxs-lookup"><span data-stu-id="1c86e-210">Open the Person class and add an attribute for the person's middle name.</span></span> <span data-ttu-id="1c86e-211">Com esse novo atributo, você está alterando o modelo.</span><span class="sxs-lookup"><span data-stu-id="1c86e-211">With this new attribute, you are changing the model.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample4.cs)]
5. <span data-ttu-id="1c86e-212">Selecionar **compilação | Compile a solução** no menu para compilar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1c86e-212">Select **Build | Build Solution** on the menu to build the application.</span></span>

    <span data-ttu-id="1c86e-213">![Compilando o aplicativo](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "Compilando o aplicativo")</span><span class="sxs-lookup"><span data-stu-id="1c86e-213">![Building the application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "Building the application")</span></span>

    <span data-ttu-id="1c86e-214">*Compilando o aplicativo*</span><span class="sxs-lookup"><span data-stu-id="1c86e-214">*Building the application*</span></span>
6. <span data-ttu-id="1c86e-215">No Console do Gerenciador de Pacotes, digite o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="1c86e-215">In the Package Manager Console, enter the following command:</span></span>

    <span data-ttu-id="1c86e-216">PMC</span><span class="sxs-lookup"><span data-stu-id="1c86e-216">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample5.ps1)]

    <span data-ttu-id="1c86e-217">Esse comando procurará alterações nos objetos de dados e, em seguida, adicionará os comandos necessários para modificar o banco de dado de acordo.</span><span class="sxs-lookup"><span data-stu-id="1c86e-217">This command will look for changes in the data objects, and then, it will add the necessary commands to modify the database accordingly.</span></span>

    <span data-ttu-id="1c86e-218">![Adicionando um nome do meio](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "Adicionando um nome do meio")</span><span class="sxs-lookup"><span data-stu-id="1c86e-218">![Adding a middle name](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "Adding a middle name")</span></span>

    <span data-ttu-id="1c86e-219">*Adicionando um nome do meio*</span><span class="sxs-lookup"><span data-stu-id="1c86e-219">*Adding a middle name*</span></span>
7. <span data-ttu-id="1c86e-220">Adicional Você pode executar o comando a seguir para gerar um script SQL com a atualização diferencial.</span><span class="sxs-lookup"><span data-stu-id="1c86e-220">(Optional) You can run the following command to generate a SQL script with the differential update.</span></span> <span data-ttu-id="1c86e-221">Isso permitirá que você atualize o banco de dados manualmente (neste caso, ele não é necessário) ou aplique as alterações em outros bancos:</span><span class="sxs-lookup"><span data-stu-id="1c86e-221">This will let you update the database manually (In this case it's not necessary), or apply the changes in other databases:</span></span>

    <span data-ttu-id="1c86e-222">PMC</span><span class="sxs-lookup"><span data-stu-id="1c86e-222">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample6.ps1)]

    <span data-ttu-id="1c86e-223">![Gerando um script SQL](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "Gerando um script SQL")</span><span class="sxs-lookup"><span data-stu-id="1c86e-223">![Generating a SQL script](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "Generating a SQL script")</span></span>

    <span data-ttu-id="1c86e-224">*Gerando um script SQL*</span><span class="sxs-lookup"><span data-stu-id="1c86e-224">*Generating a SQL script*</span></span>

    <span data-ttu-id="1c86e-225">![Atualização de script SQL](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "Atualização de script SQL")</span><span class="sxs-lookup"><span data-stu-id="1c86e-225">![SQL Script update](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "SQL Script update")</span></span>

    <span data-ttu-id="1c86e-226">*Atualização de script SQL*</span><span class="sxs-lookup"><span data-stu-id="1c86e-226">*SQL Script update*</span></span>
8. <span data-ttu-id="1c86e-227">No console do Gerenciador de pacotes, digite o seguinte comando para atualizar o banco de dados:</span><span class="sxs-lookup"><span data-stu-id="1c86e-227">In the Package Manager Console, enter the following command to update the database:</span></span>

    <span data-ttu-id="1c86e-228">PMC</span><span class="sxs-lookup"><span data-stu-id="1c86e-228">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample7.ps1)]

    <span data-ttu-id="1c86e-229">![Atualizar o banco de dados](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "Atualizar o banco de dados")</span><span class="sxs-lookup"><span data-stu-id="1c86e-229">![Updating the Database](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "Updating the Database")</span></span>

    <span data-ttu-id="1c86e-230">*Atualizar o banco de dados*</span><span class="sxs-lookup"><span data-stu-id="1c86e-230">*Updating the Database*</span></span>

    <span data-ttu-id="1c86e-231">Isso adicionará a coluna **MiddleName** na tabela **People** para corresponder à definição atual da classe **Person** .</span><span class="sxs-lookup"><span data-stu-id="1c86e-231">This will add the **MiddleName** column in the **People** table to match the current definition of the **Person** class.</span></span>
9. <span data-ttu-id="1c86e-232">Depois que o banco de dados for atualizado, clique com o botão direito do mouse na pasta controlador e selecione **Adicionar | Controlador** para adicionar o controlador Person novamente (completo com os mesmos valores).</span><span class="sxs-lookup"><span data-stu-id="1c86e-232">Once the database is updated, right-click the Controller folder and select **Add | Controller** to add the Person controller again (Complete with the same values).</span></span> <span data-ttu-id="1c86e-233">Isso atualizará os métodos e as exibições existentes adicionando o novo atributo.</span><span class="sxs-lookup"><span data-stu-id="1c86e-233">This will update the existing methods and views adding the new attribute.</span></span>

    <span data-ttu-id="1c86e-234">![Adicionando uma atualização de controlador](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "Adicionando uma atualização de controlador")</span><span class="sxs-lookup"><span data-stu-id="1c86e-234">![Adding a controller update](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "Adding a controller update")</span></span>

    <span data-ttu-id="1c86e-235">*Atualizando o controlador*</span><span class="sxs-lookup"><span data-stu-id="1c86e-235">*Updating the controller*</span></span>
10. <span data-ttu-id="1c86e-236">Clique em **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="1c86e-236">Click **Add**.</span></span> <span data-ttu-id="1c86e-237">Em seguida, selecione os valores **substituir PersonController.cs** e as **exibições de substituição associadas** e clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="1c86e-237">Then, select the values **Overwrite PersonController.cs** and the **Overwrite associated views** and click **OK**.</span></span>

   ![Adicionando uma substituição de controlador](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image19.png)

   <span data-ttu-id="1c86e-239">*Atualizando o controlador*</span><span class="sxs-lookup"><span data-stu-id="1c86e-239">*Updating the controller*</span></span>

<a id="Ex1Task4"></a>

<a id="Task4-_Running_the_application"></a>
#### <a name="task4--running-the-application"></a><span data-ttu-id="1c86e-240">Task4-executando o aplicativo</span><span class="sxs-lookup"><span data-stu-id="1c86e-240">Task4- Running the application</span></span>

1. <span data-ttu-id="1c86e-241">Pressione **F5** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1c86e-241">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="1c86e-242">Abra **/Person**.</span><span class="sxs-lookup"><span data-stu-id="1c86e-242">Open **/Person**.</span></span> <span data-ttu-id="1c86e-243">Observe que os dados foram preservados, enquanto a coluna de nome do meio foi adicionada.</span><span class="sxs-lookup"><span data-stu-id="1c86e-243">Notice that the data was preserved, while the middle name column was added.</span></span>

    <span data-ttu-id="1c86e-244">![Nome do meio adicionado](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "Nome do meio adicionado")</span><span class="sxs-lookup"><span data-stu-id="1c86e-244">![Middle Name added](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "Middle Name added")</span></span>

    <span data-ttu-id="1c86e-245">*Nome do meio adicionado*</span><span class="sxs-lookup"><span data-stu-id="1c86e-245">*Middle Name added*</span></span>
3. <span data-ttu-id="1c86e-246">Se você clicar em **Editar**, poderá adicionar um segundo nome à pessoa atual.</span><span class="sxs-lookup"><span data-stu-id="1c86e-246">If you click **Edit**, you will be able to add a middle name to the current person.</span></span>

    <span data-ttu-id="1c86e-247">![Segundo nome edição](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "Segundo nome edição")</span><span class="sxs-lookup"><span data-stu-id="1c86e-247">![Middle Name edition](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "Middle Name edition")</span></span>

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="1c86e-248">Resumo</span><span class="sxs-lookup"><span data-stu-id="1c86e-248">Summary</span></span>

<span data-ttu-id="1c86e-249">Neste Hands-On laboratório, você aprendeu etapas simples para criar operações CRUD com ASP.NET MVC 4 scaffolding usando qualquer classe de modelo.</span><span class="sxs-lookup"><span data-stu-id="1c86e-249">In this Hands-On lab, you have learned simple steps to create CRUD operations with ASP.NET MVC 4 Scaffolding using any model class.</span></span> <span data-ttu-id="1c86e-250">Em seguida, você aprendeu como executar uma atualização de ponta a ponta em seu aplicativo – do banco de dados para as exibições, usando Entity Framework migrações.</span><span class="sxs-lookup"><span data-stu-id="1c86e-250">Then, you have learned how to perform an end to end update in your application -from the database to the views- by using Entity Framework Migrations.</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="1c86e-251">Apêndice A: Instalando o Visual Studio Express 2012 para Web</span><span class="sxs-lookup"><span data-stu-id="1c86e-251">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="1c86e-252">Você pode instalar o **Microsoft Visual Studio Express 2012 para Web** ou &quot; outra &quot; versão Express usando o **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="1c86e-252">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="1c86e-253">As instruções a seguir orientam você pelas etapas necessárias para instalar o *Visual Studio Express 2012 para Web* usando *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="1c86e-253">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="1c86e-254">Acesse [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="1c86e-254">Go to [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="1c86e-255">Como alternativa, se você já tiver instalado o Web Platform Installer, poderá abri-lo e procurar o produto &quot; <em>Visual Studio Express 2012 para Web com o SDK do Windows Azure</em> &quot; .</span><span class="sxs-lookup"><span data-stu-id="1c86e-255">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="1c86e-256">Clique em **instalar agora**.</span><span class="sxs-lookup"><span data-stu-id="1c86e-256">Click on **Install Now**.</span></span> <span data-ttu-id="1c86e-257">Se você não tiver **Web Platform Installer** você será redirecionado para baixar e instalá-lo primeiro.</span><span class="sxs-lookup"><span data-stu-id="1c86e-257">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="1c86e-258">Quando **Web Platform Installer** estiver aberto, clique em **instalar** para iniciar a instalação.</span><span class="sxs-lookup"><span data-stu-id="1c86e-258">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="1c86e-259">![Instalar Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "Instalar Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="1c86e-259">![Install Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="1c86e-260">*Instalar Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="1c86e-260">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="1c86e-261">Leia todos os termos e licenças de produtos e clique em **aceito** para continuar.</span><span class="sxs-lookup"><span data-stu-id="1c86e-261">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Aceitando os termos de licença](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image23.png)

    <span data-ttu-id="1c86e-263">*Aceitando os termos de licença*</span><span class="sxs-lookup"><span data-stu-id="1c86e-263">*Accepting the license terms*</span></span>
5. <span data-ttu-id="1c86e-264">Aguarde até que o processo de download e instalação seja concluído.</span><span class="sxs-lookup"><span data-stu-id="1c86e-264">Wait until the downloading and installation process completes.</span></span>

    ![Progresso da instalação](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image24.png)

    <span data-ttu-id="1c86e-266">*Progresso da instalação*</span><span class="sxs-lookup"><span data-stu-id="1c86e-266">*Installation progress*</span></span>
6. <span data-ttu-id="1c86e-267">Quando a instalação for concluída, clique em **concluir**.</span><span class="sxs-lookup"><span data-stu-id="1c86e-267">When the installation completes, click **Finish**.</span></span>

    ![Instalação concluída](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image25.png)

    <span data-ttu-id="1c86e-269">*Instalação concluída*</span><span class="sxs-lookup"><span data-stu-id="1c86e-269">*Installation completed*</span></span>
7. <span data-ttu-id="1c86e-270">Clique em **sair** para fechar Web Platform Installer.</span><span class="sxs-lookup"><span data-stu-id="1c86e-270">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="1c86e-271">Para abrir o Visual Studio Express para Web, vá para a tela **Iniciar** e comece a escrever &quot; **vs Express** e &quot; , em seguida, clique no bloco **vs Express para Web** .</span><span class="sxs-lookup"><span data-stu-id="1c86e-271">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![Bloco VS Express para Web](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image26.png)

    <span data-ttu-id="1c86e-273">*Bloco VS Express para Web*</span><span class="sxs-lookup"><span data-stu-id="1c86e-273">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="1c86e-274">Apêndice B: usando trechos de código</span><span class="sxs-lookup"><span data-stu-id="1c86e-274">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="1c86e-275">Com trechos de código, você tem todo o código de que precisa ao seu alcance.</span><span class="sxs-lookup"><span data-stu-id="1c86e-275">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="1c86e-276">O documento de laboratório informará exatamente quando você pode usá-los, conforme mostrado na figura a seguir.</span><span class="sxs-lookup"><span data-stu-id="1c86e-276">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="1c86e-277">![Usando trechos de código do Visual Studio para inserir código em seu projeto](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "Usando trechos de código do Visual Studio para inserir código em seu projeto")</span><span class="sxs-lookup"><span data-stu-id="1c86e-277">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="1c86e-278">*Usando trechos de código do Visual Studio para inserir código em seu projeto*</span><span class="sxs-lookup"><span data-stu-id="1c86e-278">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="1c86e-279">\***Para adicionar um trecho de código usando o teclado (C# somente)** _</span><span class="sxs-lookup"><span data-stu-id="1c86e-279">\***To add a code snippet using the keyboard (C# only)** _</span></span>

1. <span data-ttu-id="1c86e-280">Coloque o cursor onde você deseja inserir o código.</span><span class="sxs-lookup"><span data-stu-id="1c86e-280">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="1c86e-281">Comece digitando o nome do trecho de código (sem espaços ou hifens).</span><span class="sxs-lookup"><span data-stu-id="1c86e-281">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="1c86e-282">Observe como o IntelliSense exibe nomes de trechos de código correspondentes.</span><span class="sxs-lookup"><span data-stu-id="1c86e-282">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="1c86e-283">Selecione o trecho correto (ou continue digitando até que o nome do trecho inteiro seja selecionado).</span><span class="sxs-lookup"><span data-stu-id="1c86e-283">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="1c86e-284">Pressione a tecla TAB duas vezes para inserir o trecho de código no local do cursor.</span><span class="sxs-lookup"><span data-stu-id="1c86e-284">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="1c86e-285">![Comece a digitar o nome do trecho](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "Comece a digitar o nome do trecho")</span><span class="sxs-lookup"><span data-stu-id="1c86e-285">![Start typing the snippet name](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "Start typing the snippet name")</span></span>

<span data-ttu-id="1c86e-286">_Start digitando o nome do trecho de código \*</span><span class="sxs-lookup"><span data-stu-id="1c86e-286">_Start typing the snippet name\*</span></span>

<span data-ttu-id="1c86e-287">![Pressione Tab para selecionar o trecho realçado](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "Pressione Tab para selecionar o trecho realçado")</span><span class="sxs-lookup"><span data-stu-id="1c86e-287">![Press Tab to select the highlighted snippet](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="1c86e-288">*Pressione Tab para selecionar o trecho realçado*</span><span class="sxs-lookup"><span data-stu-id="1c86e-288">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="1c86e-289">![Pressione Tab novamente e o trecho será expandido](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "Pressione Tab novamente e o trecho será expandido")</span><span class="sxs-lookup"><span data-stu-id="1c86e-289">![Press Tab again and the snippet will expand](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="1c86e-290">*Pressione Tab novamente e o trecho será expandido*</span><span class="sxs-lookup"><span data-stu-id="1c86e-290">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="1c86e-291">\***Para adicionar um trecho de código usando o mouse (C#, Visual Basic e XML)** _ 1.</span><span class="sxs-lookup"><span data-stu-id="1c86e-291">\***To add a code snippet using the mouse (C#, Visual Basic and XML)** _ 1.</span></span> <span data-ttu-id="1c86e-292">Clique com o botão direito do mouse no local em que você deseja inserir o trecho de código.</span><span class="sxs-lookup"><span data-stu-id="1c86e-292">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="1c86e-293">Selecione _ *Inserir trecho*\* seguido por **meus trechos de código**.</span><span class="sxs-lookup"><span data-stu-id="1c86e-293">Select _ *Insert Snippet*\* followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="1c86e-294">Selecione o trecho relevante na lista clicando nele.</span><span class="sxs-lookup"><span data-stu-id="1c86e-294">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="1c86e-295">![Clique com o botão direito do mouse em onde você deseja inserir o trecho de código e selecione Inserir trecho](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "Clique com o botão direito do mouse em onde você deseja inserir o trecho de código e selecione Inserir trecho")</span><span class="sxs-lookup"><span data-stu-id="1c86e-295">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="1c86e-296">*Clique com o botão direito do mouse em onde você deseja inserir o trecho de código e selecione Inserir trecho*</span><span class="sxs-lookup"><span data-stu-id="1c86e-296">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="1c86e-297">![Selecione o trecho relevante na lista clicando nele](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "Selecione o trecho relevante na lista clicando nele")</span><span class="sxs-lookup"><span data-stu-id="1c86e-297">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="1c86e-298">*Selecione o trecho relevante na lista clicando nele*</span><span class="sxs-lookup"><span data-stu-id="1c86e-298">*Pick the relevant snippet from the list, by clicking on it*</span></span>
