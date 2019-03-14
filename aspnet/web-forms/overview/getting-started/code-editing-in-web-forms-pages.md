---
uid: web-forms/overview/getting-started/code-editing-in-web-forms-pages
title: Edição de código Web Forms do ASP.NET no Visual Studio 2013 | Microsoft Docs
author: Erikre
description: ''
ms.author: riande
ms.date: 03/03/2014
ms.assetid: 5344b74e-b888-479a-92bc-601a33bd61a2
msc.legacyurl: /web-forms/overview/getting-started/code-editing-in-web-forms-pages
msc.type: authoredcontent
ms.openlocfilehash: 670f81ca1ef9923575cb2fee1747f06f426963d8
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57029703"
---
<a name="code-editing-aspnet-web-forms-in-visual-studio-2013"></a><span data-ttu-id="3810a-102">Edição de código de Web Forms do ASP.NET no Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="3810a-102">Code Editing ASP.NET Web Forms in Visual Studio 2013</span></span>
====================
<span data-ttu-id="3810a-103">by [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="3810a-103">by [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="3810a-104">Em muitas páginas de Web Form do ASP.NET, você escreve código em Visual Basic, c# ou outra linguagem.</span><span class="sxs-lookup"><span data-stu-id="3810a-104">In many ASP.NET Web Form pages, you write code in Visual Basic, C#, or another language.</span></span> <span data-ttu-id="3810a-105">O editor de código no Visual Studio pode ajudá-lo a escrever código rapidamente, enquanto ajuda a evitar erros.</span><span class="sxs-lookup"><span data-stu-id="3810a-105">The code editor in Visual Studio can help you write code quickly while helping you avoid errors.</span></span> <span data-ttu-id="3810a-106">Além disso, o editor fornece maneiras para você criar códigos reutilizáveis para ajudar a reduzir a quantidade de trabalho que você precisa fazer.</span><span class="sxs-lookup"><span data-stu-id="3810a-106">In addition, the editor provides ways for you to create reusable code to help reduce the amount of work you need to do.</span></span>

<span data-ttu-id="3810a-107">Este passo a passo ilustra vários recursos do editor de código do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3810a-107">This walkthrough illustrates various features of the Visual Studio code editor.</span></span>

<span data-ttu-id="3810a-108">Durante este passo a passo, você aprenderá a:</span><span class="sxs-lookup"><span data-stu-id="3810a-108">During this walkthrough, you will learn how to:</span></span>

- <span data-ttu-id="3810a-109">Corrija os erros de codificação embutida.</span><span class="sxs-lookup"><span data-stu-id="3810a-109">Correct inline coding errors.</span></span>
- <span data-ttu-id="3810a-110">Refatorar e renomear o código.</span><span class="sxs-lookup"><span data-stu-id="3810a-110">Refactor and rename code.</span></span>
- <span data-ttu-id="3810a-111">Renomear variáveis e objetos.</span><span class="sxs-lookup"><span data-stu-id="3810a-111">Rename variables and objects.</span></span>
- <span data-ttu-id="3810a-112">Inserir trechos de código.</span><span class="sxs-lookup"><span data-stu-id="3810a-112">Insert code snippets.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3810a-113">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="3810a-113">Prerequisites</span></span>


<span data-ttu-id="3810a-114">Para concluir este passo a passo, você precisará de:</span><span class="sxs-lookup"><span data-stu-id="3810a-114">In order to complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="3810a-115">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) ou [Microsoft Visual Studio Express 2013 para Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span><span class="sxs-lookup"><span data-stu-id="3810a-115">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) or [Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span> <span data-ttu-id="3810a-116">O .NET Framework é instalado automaticamente.</span><span class="sxs-lookup"><span data-stu-id="3810a-116">The .NET Framework is installed automatically.</span></span> 

    > [!NOTE] 
    > 
    > <span data-ttu-id="3810a-117">Microsoft Visual Studio 2013 e Microsoft Visual Studio Express 2013 para Web serão ser conhecido como Visual Studio em toda esta série de tutoriais.</span><span class="sxs-lookup"><span data-stu-id="3810a-117">Microsoft Visual Studio 2013 and Microsoft Visual Studio Express 2013 for Web will often be referred to as Visual Studio throughout this tutorial series.</span></span>  
    >   
    > <span data-ttu-id="3810a-118">Se você estiver usando o Visual Studio, este passo a passo pressupõe que você selecionou a **desenvolvimento Web** coleção de configurações na primeira vez que você iniciou o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3810a-118">If you are using Visual Studio, this walkthrough assumes that you selected the **Web Development** collection of settings the first time that you started Visual Studio.</span></span> <span data-ttu-id="3810a-119">Para obter mais informações, confira [Como: Selecione as configurações de ambiente de desenvolvimento da Web](https://msdn.microsoft.com/library/ff521558.aspx).</span><span class="sxs-lookup"><span data-stu-id="3810a-119">For more information, see [How to: Select Web Development Environment Settings](https://msdn.microsoft.com/library/ff521558.aspx).</span></span>

## <a name="creating-a-web-application-project-and-a-page"></a><span data-ttu-id="3810a-120">Criando um projeto de aplicativo Web e uma página</span><span class="sxs-lookup"><span data-stu-id="3810a-120">Creating a Web application project and a Page</span></span>

<a id="sectionToggle0"></a>

<span data-ttu-id="3810a-121">Nesta parte do passo a passo, você criará um projeto de aplicativo Web e adicionar uma nova página a ele.</span><span class="sxs-lookup"><span data-stu-id="3810a-121">In this part of the walkthrough, you will create a Web application project and add a new page to it.</span></span>

### <a name="to-create-a-web-application-project"></a><span data-ttu-id="3810a-122">Para criar um projeto de aplicativo Web</span><span class="sxs-lookup"><span data-stu-id="3810a-122">To create a Web application project</span></span>

1. <span data-ttu-id="3810a-123">Abra o Microsoft Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3810a-123">Open Microsoft Visual Studio.</span></span>
2. <span data-ttu-id="3810a-124">No menu **Arquivo**, selecione **Novo Projeto**.</span><span class="sxs-lookup"><span data-stu-id="3810a-124">On the **File** menu, select **New Project**.</span></span>  
    <span data-ttu-id="3810a-125">![Menu arquivo](code-editing-in-web-forms-pages/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="3810a-125">![File Menu](code-editing-in-web-forms-pages/_static/image1.png)</span></span>

    <span data-ttu-id="3810a-126">A caixa de diálogo **Novo Projeto** é exibida.</span><span class="sxs-lookup"><span data-stu-id="3810a-126">The **New Project** dialog box appears.</span></span>
3. <span data-ttu-id="3810a-127">Selecione o **modelos**  - &gt; **Visual c#**  - &gt; **Web** grupo de modelos à esquerda.</span><span class="sxs-lookup"><span data-stu-id="3810a-127">Select the **Templates** -&gt; **Visual C#** -&gt; **Web** templates group on the left.</span></span>
4. <span data-ttu-id="3810a-128">Escolha o **aplicativo Web ASP.NET** modelo na coluna central.</span><span class="sxs-lookup"><span data-stu-id="3810a-128">Choose the **ASP.NET Web Application** template in the center column.</span></span>
5. <span data-ttu-id="3810a-129">Nomeie o projeto ***BasicWebApp*** e clique no **Okey** botão.</span><span class="sxs-lookup"><span data-stu-id="3810a-129">Name your project ***BasicWebApp*** and click the **OK** button.</span></span>   
<span data-ttu-id="3810a-130">![Caixa de diálogo Novo projeto](code-editing-in-web-forms-pages/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="3810a-130">![New Project dialog box](code-editing-in-web-forms-pages/_static/image2.png)</span></span>
6. <span data-ttu-id="3810a-131">Em seguida, selecione a **Web Forms** modelo e clique no **Okey** botão para criar o projeto.</span><span class="sxs-lookup"><span data-stu-id="3810a-131">Next, select the **Web Forms** template and click the **OK** button to create the project.</span></span>  
<span data-ttu-id="3810a-132">![Caixa de diálogo Novo projeto ASP.NET](code-editing-in-web-forms-pages/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="3810a-132">![New ASP.NET Project dialog box](code-editing-in-web-forms-pages/_static/image3.png)</span></span>  

    <span data-ttu-id="3810a-133">Visual Studio cria um novo projeto que inclui a funcionalidade predefinidas com base no modelo de formulários da Web.</span><span class="sxs-lookup"><span data-stu-id="3810a-133">Visual Studio creates a new project that includes prebuilt functionality based on the Web Forms template.</span></span>


## <a name="creating-a-new-aspnet-web-forms-page"></a><span data-ttu-id="3810a-134">Criando uma nova página Web Forms do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="3810a-134">Creating a new ASP.NET Web Forms Page</span></span>


<span data-ttu-id="3810a-135">Quando você cria um novo aplicativo do Web Forms usando o **aplicativo Web ASP.NET** modelo de projeto, o Visual Studio adiciona uma página do ASP.NET (página de Web Forms) chamada *default. aspx*, bem como vários outros arquivos e pastas.</span><span class="sxs-lookup"><span data-stu-id="3810a-135">When you create a new Web Forms application using the **ASP.NET Web Application** project template, Visual Studio adds an ASP.NET page (Web Forms page) named *Default.aspx*, as well as several other files and folders.</span></span> <span data-ttu-id="3810a-136">Você pode usar o *default. aspx* página como a home page do seu aplicativo Web.</span><span class="sxs-lookup"><span data-stu-id="3810a-136">You can use the *Default.aspx* page as the home page for your Web application.</span></span> <span data-ttu-id="3810a-137">No entanto, para este passo a passo, você irá criar e trabalhar com uma nova página.</span><span class="sxs-lookup"><span data-stu-id="3810a-137">However, for this walkthrough, you will create and work with a new page.</span></span>

### <a name="to-add-a-page-to-the-web-application"></a><span data-ttu-id="3810a-138">Para adicionar uma página ao aplicativo Web</span><span class="sxs-lookup"><span data-stu-id="3810a-138">To add a page to the Web application</span></span>


1. <span data-ttu-id="3810a-139">Na **Gerenciador de soluções**, clique no nome do aplicativo Web (neste tutorial é o nome do aplicativo **BasicWebSite**) e, em seguida, clique em **Add**  - &gt; **Novo Item**.</span><span class="sxs-lookup"><span data-stu-id="3810a-139">In **Solution Explorer**, right-click the Web application name (in this tutorial the application name is **BasicWebSite**), and then click **Add** -&gt; **New Item**.</span></span>   
<span data-ttu-id="3810a-140">A caixa de diálogo **Adicionar Novo Item** é exibida.</span><span class="sxs-lookup"><span data-stu-id="3810a-140">The **Add New Item** dialog box is displayed.</span></span>
2. <span data-ttu-id="3810a-141">Selecione o **Visual c#**  - &gt; **Web** grupo de modelos à esquerda.</span><span class="sxs-lookup"><span data-stu-id="3810a-141">Select the **Visual C#** -&gt; **Web** templates group on the left.</span></span> <span data-ttu-id="3810a-142">Em seguida, selecione **Web Form** do meio lista e nomeie-o *FirstWebPage*.</span><span class="sxs-lookup"><span data-stu-id="3810a-142">Then, select **Web Form** from the middle list and name it *FirstWebPage.aspx*.</span></span>   
    <span data-ttu-id="3810a-143">![Adicionar caixa de diálogo Novo Item](code-editing-in-web-forms-pages/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="3810a-143">![Add New Item dialog box](code-editing-in-web-forms-pages/_static/image4.png)</span></span>
3. <span data-ttu-id="3810a-144">Clique em **adicionar** para adicionar a página de Web Forms ao seu projeto.</span><span class="sxs-lookup"><span data-stu-id="3810a-144">Click **Add** to add the Web Forms page to your project.</span></span>  
 <span data-ttu-id="3810a-145">Visual Studio cria a nova página e ele é aberto.</span><span class="sxs-lookup"><span data-stu-id="3810a-145">Visual Studio creates the new page and opens it.</span></span>
4. <span data-ttu-id="3810a-146">Em seguida, defina essa nova página como a página de inicialização padrão.</span><span class="sxs-lookup"><span data-stu-id="3810a-146">Next, set this new page as the default startup page.</span></span> <span data-ttu-id="3810a-147">Na **Gerenciador de soluções**, clique com botão direito a nova página denominada *FirstWebPage* e selecione **definir como página inicial**.</span><span class="sxs-lookup"><span data-stu-id="3810a-147">In **Solution Explorer**, right-click the new page named *FirstWebPage.aspx* and select **Set As Start Page**.</span></span> <span data-ttu-id="3810a-148">Na próxima vez que você executar esse aplicativo para testar nosso progresso, você será automaticamente consulte essa nova página no navegador.</span><span class="sxs-lookup"><span data-stu-id="3810a-148">The next time you run this application to test our progress, you will automatically see this new page in the browser.</span></span>


## <a name="correcting-inline-coding-errors"></a><span data-ttu-id="3810a-149">Corrigindo embutido erros de codificação</span><span class="sxs-lookup"><span data-stu-id="3810a-149">Correcting Inline Coding Errors</span></span>


<span data-ttu-id="3810a-150">O editor de código no Visual Studio ajuda você a evitar erros conforme você escreve o código e, se você tiver cometido um erro, o editor de código ajuda você a corrigir o erro.</span><span class="sxs-lookup"><span data-stu-id="3810a-150">The code editor in Visual Studio helps you to avoid errors as you write code, and if you have made an error, the code editor helps you to correct the error.</span></span> <span data-ttu-id="3810a-151">Nesta parte do passo a passo, você irá escrever uma linha de código que ilustram os recursos de correção de erro no editor.</span><span class="sxs-lookup"><span data-stu-id="3810a-151">In this part of the walkthrough, you will write a line of code that illustrate the error correction features in the editor.</span></span>

### <a name="to-correct-simple-coding-errors-in-visual-studio"></a><span data-ttu-id="3810a-152">Para corrigir erros de codificação simples no Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3810a-152">To correct simple coding errors in Visual Studio</span></span>


1. <span data-ttu-id="3810a-153">Na **Design** exibir, clique duas vezes na página em branco para criar um manipulador para o **carga** evento para a página.</span><span class="sxs-lookup"><span data-stu-id="3810a-153">In **Design** view, double-click the blank page to create a handler for the **Load** event for the page.</span></span>   
   <span data-ttu-id="3810a-154">Você está usando o manipulador de eventos somente como um lugar para escrever um código.</span><span class="sxs-lookup"><span data-stu-id="3810a-154">You are using the event handler only as a place to write some code.</span></span>
2. <span data-ttu-id="3810a-155">Dentro do manipulador, digite a seguinte linha que contém um erro e pressione **ENTER**:</span><span class="sxs-lookup"><span data-stu-id="3810a-155">Inside the handler, type the following line that contains an error and press **ENTER**:</span></span>

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample1.cs)]

   <span data-ttu-id="3810a-156">Quando você pressiona **ENTER**, o editor de códigos coloca sublinhados verdes e vermelhos (normalmente chamam &quot;ondulada&quot; linhas) em áreas do código que apresentam problemas.</span><span class="sxs-lookup"><span data-stu-id="3810a-156">When you press **ENTER**, the code editor places green and red underlines (commonly call &quot;squiggly&quot; lines) under areas of the code that have issues.</span></span> <span data-ttu-id="3810a-157">Um sublinhado verde indica um aviso.</span><span class="sxs-lookup"><span data-stu-id="3810a-157">A green underline indicates a warning.</span></span> <span data-ttu-id="3810a-158">Um sublinhado vermelho indica um erro que você precisa corrigir.</span><span class="sxs-lookup"><span data-stu-id="3810a-158">A red underline indicates an error that you must fix.</span></span> 

    <span data-ttu-id="3810a-159">Mantenha o ponteiro do mouse sobre `myStr` para ver uma dica de ferramenta que informa você sobre o aviso.</span><span class="sxs-lookup"><span data-stu-id="3810a-159">Hold the mouse pointer over `myStr` to see a tooltip that tells you about the warning.</span></span> <span data-ttu-id="3810a-160">Além disso, mantenha o ponteiro do mouse sobre o sublinhado vermelho para ver a mensagem de erro.</span><span class="sxs-lookup"><span data-stu-id="3810a-160">Also, hold your mouse pointer over the red underline to see the error message.</span></span>

    <span data-ttu-id="3810a-161">A imagem a seguir mostra o código com os sublinhados.</span><span class="sxs-lookup"><span data-stu-id="3810a-161">The following image shows the code with the underlines.</span></span>

    <span data-ttu-id="3810a-162">![Texto de boas vindas no modo de exibição de Design](code-editing-in-web-forms-pages/_static/image5.png "texto de boas vindas no modo de exibição de Design")</span><span class="sxs-lookup"><span data-stu-id="3810a-162">![Welcome text in Design view](code-editing-in-web-forms-pages/_static/image5.png "Welcome text in Design view")</span></span>  
   <span data-ttu-id="3810a-163">O erro deve ser corrigido pela adição de um ponto e vírgula `;` até o final da linha.</span><span class="sxs-lookup"><span data-stu-id="3810a-163">The error must be fixed by adding a semicolon `;` to the end of the line.</span></span> <span data-ttu-id="3810a-164">O aviso simplesmente notifica que você não usou o `myStr` variável ainda.</span><span class="sxs-lookup"><span data-stu-id="3810a-164">The warning simply notifies you that you haven't used the `myStr` variable yet.</span></span>  

    > [!NOTE] 
    > 
    > <span data-ttu-id="3810a-165">Visualizar seu código atual formatação configurações no Visual Studio, selecionando **ferramentas**  - &gt; **opções**  - &gt; **fontes e Cores**.</span><span class="sxs-lookup"><span data-stu-id="3810a-165">You view your current code formatting settings in Visual Studio by selecting **Tools** -&gt; **Options** -&gt; **Fonts and Colors**.</span></span>


## <a name="refactoring-and-renaming"></a><span data-ttu-id="3810a-166">Refatoração e renomear</span><span class="sxs-lookup"><span data-stu-id="3810a-166">Refactoring and Renaming</span></span>

<span data-ttu-id="3810a-167">Refatoração é uma metodologia de software que envolve a reestruturação do código para torná-lo mais fácil de entender e manter, preservando sua funcionalidade.</span><span class="sxs-lookup"><span data-stu-id="3810a-167">Refactoring is a software methodology that involves restructuring your code to make it easier to understand and to maintain, while preserving its functionality.</span></span> <span data-ttu-id="3810a-168">Um exemplo simples pode ser que você escreva código em um manipulador de eventos para obter dados de um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="3810a-168">A simple example might be that you write code in an event handler to get data from a database.</span></span> <span data-ttu-id="3810a-169">À medida que você desenvolve sua página, você descobre que precisa acessar os dados de vários manipuladores diferentes.</span><span class="sxs-lookup"><span data-stu-id="3810a-169">As you develop your page, you discover that you need to access the data from several different handlers.</span></span> <span data-ttu-id="3810a-170">Portanto, você refatora o código da página Criando um método de acesso a dados na página e inserindo chamadas para o método nos manipuladores.</span><span class="sxs-lookup"><span data-stu-id="3810a-170">Therefore, you refactor the page's code by creating a data-access method in the page and inserting calls to the method in the handlers.</span></span>

<span data-ttu-id="3810a-171">O editor de código inclui ferramentas para ajudá-lo a realizar várias tarefas de refatoração.</span><span class="sxs-lookup"><span data-stu-id="3810a-171">The code editor includes tools to help you perform various refactoring tasks.</span></span> <span data-ttu-id="3810a-172">Neste passo a passo, você trabalhará com duas técnicas de refatoração: Renomeando variáveis e métodos de extração.</span><span class="sxs-lookup"><span data-stu-id="3810a-172">In this walkthrough, you will work with two refactoring techniques: renaming variables and extracting methods.</span></span> <span data-ttu-id="3810a-173">Outras opções de refatoração incluem encapsular campos, promovendo as variáveis locais para parâmetros de método e gerenciar os parâmetros de método.</span><span class="sxs-lookup"><span data-stu-id="3810a-173">Other refactoring options include encapsulating fields, promoting local variables to method parameters, and managing method parameters.</span></span> <span data-ttu-id="3810a-174">A disponibilidade dessas opções de refatoração depende do local no código.</span><span class="sxs-lookup"><span data-stu-id="3810a-174">The availability of these refactoring options depends on the location in the code.</span></span>

### <a name="refactoring-code"></a><span data-ttu-id="3810a-175">Código de refatoração</span><span class="sxs-lookup"><span data-stu-id="3810a-175">Refactoring Code</span></span>

<span data-ttu-id="3810a-176">Um cenário de refatoração comum é criar (Extrair) um método do código que está dentro de outro membro, como um método.</span><span class="sxs-lookup"><span data-stu-id="3810a-176">A common refactoring scenario is to create (extract) a method from code that is inside another member, such as a method.</span></span> <span data-ttu-id="3810a-177">Isso reduz o tamanho do membro original e torna o código extraído reutilizável.</span><span class="sxs-lookup"><span data-stu-id="3810a-177">This reduces the size of the original member and makes the extracted code reusable.</span></span>

<span data-ttu-id="3810a-178">Nesta parte do passo a passo, você escrever um código simples e, em seguida, extrair um método dele.</span><span class="sxs-lookup"><span data-stu-id="3810a-178">In this part of the walkthrough, you will write some simple code, and then extract a method from it.</span></span> <span data-ttu-id="3810a-179">Refatoração é compatível com c#, portanto, você criará uma página que usa c# como sua linguagem de programação.</span><span class="sxs-lookup"><span data-stu-id="3810a-179">Refactoring is supported for C#, so you will create a page that uses C# as its programming language.</span></span>

### <a name="to-extract-a-method-in-a-c-page"></a><span data-ttu-id="3810a-180">Para extrair um método em uma página do c#</span><span class="sxs-lookup"><span data-stu-id="3810a-180">To extract a method in a C# page</span></span>

1. <span data-ttu-id="3810a-181">Alterne para **Design** modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="3810a-181">Switch to **Design** view.</span></span>
2. <span data-ttu-id="3810a-182">No **caixa de ferramentas**, da **padrão** guia, arraste uma [botão](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) até a página.</span><span class="sxs-lookup"><span data-stu-id="3810a-182">In the **Toolbox**, from the **Standard** tab, drag a [Button](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) control onto the page.</span></span>
3. <span data-ttu-id="3810a-183">Clique duas vezes o **botão** controle para criar um manipulador para seu [clique](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) evento e, em seguida, adicione o seguinte código realçado:</span><span class="sxs-lookup"><span data-stu-id="3810a-183">Double-click the **Button** control to create a handler for its [Click](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) event, and then add the following highlighted code:</span></span>

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample2.cs?highlight=3-16)]

   <span data-ttu-id="3810a-184">O código cria um **ArrayList** objeto, usa um loop para carregá-lo com valores e, em seguida, usa outro loop para exibir o conteúdo da **ArrayList** objeto.</span><span class="sxs-lookup"><span data-stu-id="3810a-184">The code creates an **ArrayList** object, uses a loop to load it with values, and then uses another loop to display the contents of the **ArrayList** object.</span></span>
4. <span data-ttu-id="3810a-185">Pressione **CTRL+F5** para executar a página e, em seguida, clique no **botão** para certificar-se de que você vê a seguinte saída:</span><span class="sxs-lookup"><span data-stu-id="3810a-185">Press **CTRL+F5** to run the page, and then click the **button** to make sure that you see the following output:</span></span>   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample3.html)]
5. <span data-ttu-id="3810a-186">Retornar ao editor de códigos e, em seguida, selecione as linhas a seguir no manipulador de eventos.</span><span class="sxs-lookup"><span data-stu-id="3810a-186">Return to the code editor, and then select the following lines in the event handler.</span></span>   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample4.html)]
6. <span data-ttu-id="3810a-187">A seleção com o botão direito, clique em **refatorar**e, em seguida, escolha **extrair método**.</span><span class="sxs-lookup"><span data-stu-id="3810a-187">Right-click the selection, click **Refactor**, and then choose **Extract Method**.</span></span> 

    <span data-ttu-id="3810a-188">O **extrair método** caixa de diálogo é exibida.</span><span class="sxs-lookup"><span data-stu-id="3810a-188">The **Extract Method** dialog box appears.</span></span>
7. <span data-ttu-id="3810a-189">No **nome do novo método** , digite **DisplayArray**e, em seguida, clique em **Okey**.</span><span class="sxs-lookup"><span data-stu-id="3810a-189">In the **New Method Name** box, type **DisplayArray**, and then click **OK**.</span></span> 

    <span data-ttu-id="3810a-190">O editor de código cria um novo método chamado `DisplayArray`e coloca uma chamada para o novo método em de **clique em** manipulador em que o loop foi originalmente.</span><span class="sxs-lookup"><span data-stu-id="3810a-190">The code editor creates a new method named `DisplayArray`, and puts a call to the new method in the **Click** handler where the loop was originally.</span></span>

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample5.cs?highlight=12)]
8. <span data-ttu-id="3810a-191">Pressione **CTRL+F5** para executar a página novamente e, em seguida, clique no **botão**.</span><span class="sxs-lookup"><span data-stu-id="3810a-191">Press **CTRL+F5** to run the page again, and click the **button**.</span></span>

    <span data-ttu-id="3810a-192">A página funciona da mesma como fazia antes.</span><span class="sxs-lookup"><span data-stu-id="3810a-192">The page functions the same as it did before.</span></span> <span data-ttu-id="3810a-193">O `DisplayArray` método agora pode ser chamada em qualquer lugar na classe de página.</span><span class="sxs-lookup"><span data-stu-id="3810a-193">The `DisplayArray` method can now be call from anywhere in the page class.</span></span>

## <a name="renaming-variables"></a><span data-ttu-id="3810a-194">Renomeando variáveis</span><span class="sxs-lookup"><span data-stu-id="3810a-194">Renaming Variables</span></span>

<span data-ttu-id="3810a-195">Quando você trabalha com variáveis, bem como objetos, você talvez queira renomeá-los depois que eles já estão referenciados em seu código.</span><span class="sxs-lookup"><span data-stu-id="3810a-195">When you work with variables, as well as objects, you might want to rename them after they are already referenced in your code.</span></span> <span data-ttu-id="3810a-196">No entanto, a renomeação de variáveis e objetos pode causar quebra se você esquecer de renomear uma das referências do código.</span><span class="sxs-lookup"><span data-stu-id="3810a-196">However, renaming variables and objects can cause the code to break if you miss renaming one of the references.</span></span> <span data-ttu-id="3810a-197">Portanto, você pode usar a refatoração para executar a renomeação.</span><span class="sxs-lookup"><span data-stu-id="3810a-197">Therefore, you can use refactoring to perform the renaming.</span></span>

### <a name="to-use-refactoring-to-rename-a-variable"></a><span data-ttu-id="3810a-198">Para usar a refatoração para renomear uma variável</span><span class="sxs-lookup"><span data-stu-id="3810a-198">To use refactoring to rename a variable</span></span>


1. <span data-ttu-id="3810a-199">No **clique** manipulador de eventos, localize a seguinte linha:</span><span class="sxs-lookup"><span data-stu-id="3810a-199">In the **Click** event handler, locate the following line:</span></span>

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample6.cs)]
2. <span data-ttu-id="3810a-200">O nome da variável com o botão direito `alist`, escolha **refatorar**e, em seguida, escolha **Renomear**.</span><span class="sxs-lookup"><span data-stu-id="3810a-200">Right-click the variable name `alist`, choose **Refactor**, and then choose **Rename**.</span></span>

    <span data-ttu-id="3810a-201">O **Renomear** caixa de diálogo é exibida.</span><span class="sxs-lookup"><span data-stu-id="3810a-201">The **Rename** dialog box appears.</span></span>
3. <span data-ttu-id="3810a-202">No **novo nome** , digite **ArrayList1** e verifique se o **visualizar alterações de referência** caixa de seleção tiver sido selecionada.</span><span class="sxs-lookup"><span data-stu-id="3810a-202">In the **New name** box, type **ArrayList1** and make sure the **Preview reference changes** checkbox has been selected.</span></span> <span data-ttu-id="3810a-203">Clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="3810a-203">Then click **OK**.</span></span>

    <span data-ttu-id="3810a-204">O **visualizar alterações** caixa de diálogo aparece e exibe uma árvore que contém todas as referências para a variável que você está renomeando.</span><span class="sxs-lookup"><span data-stu-id="3810a-204">The **Preview Changes** dialog box appears, and displays a tree that contains all references to the variable that you are renaming.</span></span>
4. <span data-ttu-id="3810a-205">Clique em **Apply** para fechar o **visualizar alterações** caixa de diálogo.</span><span class="sxs-lookup"><span data-stu-id="3810a-205">Click **Apply** to close the **Preview Changes** dialog box.</span></span>

    <span data-ttu-id="3810a-206">As variáveis que se refiram especificamente a instância que você selecionou são renomeadas.</span><span class="sxs-lookup"><span data-stu-id="3810a-206">The variables that refer specifically to the instance that you selected are renamed.</span></span> <span data-ttu-id="3810a-207">No entanto, observe que a variável `alist` na linha a seguir não é renomeada.</span><span class="sxs-lookup"><span data-stu-id="3810a-207">Note, however, that the variable `alist` in the following line is not renamed.</span></span>

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample7.cs)]

    <span data-ttu-id="3810a-208">A variável `alist` essa linha não é renomeada porque ele não representa o mesmo valor que a variável `alist` que você renomeou.</span><span class="sxs-lookup"><span data-stu-id="3810a-208">The variable `alist` in this line is not renamed because it does not represent the same value as the variable `alist` that you renamed.</span></span> <span data-ttu-id="3810a-209">A variável `alist` no `DisplayArray` declaração é uma variável local para o método.</span><span class="sxs-lookup"><span data-stu-id="3810a-209">The variable `alist` in the `DisplayArray` declaration is a local variable for that method.</span></span> <span data-ttu-id="3810a-210">Isso ilustra que, usando a refatoração para renomear variáveis é diferente de simplesmente executar uma ação de localizar e substituir no editor; Refatoração renomeia variáveis com conhecimento da semântica da variável que ele está funcionando com.</span><span class="sxs-lookup"><span data-stu-id="3810a-210">This illustrates that using refactoring to rename variables is different than simply performing a find-and-replace action in the editor; refactoring renames variables with knowledge of the semantics of the variable that it is working with.</span></span>


## <a name="inserting-snippets"></a><span data-ttu-id="3810a-211">Inserir trechos de código</span><span class="sxs-lookup"><span data-stu-id="3810a-211">Inserting Snippets</span></span>

<span data-ttu-id="3810a-212">Como há muitas tarefas de codificação que os desenvolvedores de Web Forms com frequência precisam executar, o editor de código fornece uma biblioteca de trechos de código, ou blocos de código pré-escritos.</span><span class="sxs-lookup"><span data-stu-id="3810a-212">Because there are many coding tasks that Web Forms developers frequently need to perform, the code editor provides a library of snippets, or blocks of prewritten code.</span></span> <span data-ttu-id="3810a-213">Você pode inserir esses trechos de código em sua página.</span><span class="sxs-lookup"><span data-stu-id="3810a-213">You can insert these snippets into your page.</span></span>

<span data-ttu-id="3810a-214">Cada linguagem que você usa no Visual Studio tem pequenas diferenças na maneira como inserir trechos de código.</span><span class="sxs-lookup"><span data-stu-id="3810a-214">Each language that you use in Visual Studio has slight differences in the way you insert code snippets.</span></span> <span data-ttu-id="3810a-215">Para obter informações sobre como inserir trechos de código, consulte [trechos de código do Visual Basic IntelliSense](https://msdn.microsoft.com/library/18yz4be4.aspx).</span><span class="sxs-lookup"><span data-stu-id="3810a-215">For information about inserting snippets, see [Visual Basic IntelliSense Code Snippets](https://msdn.microsoft.com/library/18yz4be4.aspx).</span></span> <span data-ttu-id="3810a-216">Para obter informações sobre como inserir trechos de código no Visual c#, consulte [trechos de código do Visual c#](https://msdn.microsoft.com/library/z41h7fat.aspx).</span><span class="sxs-lookup"><span data-stu-id="3810a-216">For information about inserting snippets in Visual C#, see [Visual C# Code Snippets](https://msdn.microsoft.com/library/z41h7fat.aspx).</span></span>

## <a name="next-steps"></a><span data-ttu-id="3810a-217">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="3810a-217">Next Steps</span></span>

<span data-ttu-id="3810a-218">Este passo a passo ilustra os recursos básicos do editor de código do Visual Studio 2010 para corrigir erros em seu código, refatoração de código, renomeando variáveis e inserindo trechos de código em seu código.</span><span class="sxs-lookup"><span data-stu-id="3810a-218">This walkthrough has illustrated the basic features of the Visual Studio 2010 code editor for correcting errors in your code, refactoring code, renaming variables, and inserting code snippets into your code.</span></span> <span data-ttu-id="3810a-219">Recursos adicionais no editor podem tornar o desenvolvimento de aplicativo rápido e fácil.</span><span class="sxs-lookup"><span data-stu-id="3810a-219">Additional features in the editor can make application development fast and easy.</span></span> <span data-ttu-id="3810a-220">Por exemplo, é possível:</span><span class="sxs-lookup"><span data-stu-id="3810a-220">For example, you might want to:</span></span>

- <span data-ttu-id="3810a-221">Saiba mais sobre os recursos do IntelliSense, como modificar opções do IntelliSense, gerenciar trechos de código e pesquisando trechos de código online.</span><span class="sxs-lookup"><span data-stu-id="3810a-221">Learn more about the features of IntelliSense, such as modifying IntelliSense options, managing code snippets, and searching for code snippets online.</span></span> <span data-ttu-id="3810a-222">Para obter mais informações, veja [Usando o IntelliSense](https://msdn.microsoft.com/library/hcw1s69b.aspx).</span><span class="sxs-lookup"><span data-stu-id="3810a-222">For more information, see [Using IntelliSense](https://msdn.microsoft.com/library/hcw1s69b.aspx).</span></span>
- <span data-ttu-id="3810a-223">Aprenda a criar seus próprios trechos de código.</span><span class="sxs-lookup"><span data-stu-id="3810a-223">Learn how to create your own code snippets.</span></span> <span data-ttu-id="3810a-224">Para obter mais informações, consulte [criando e usando trechos de código do IntelliSense](https://msdn.microsoft.com/library/ms165392.aspx)</span><span class="sxs-lookup"><span data-stu-id="3810a-224">For more information, see [Creating and Using IntelliSense Code Snippets](https://msdn.microsoft.com/library/ms165392.aspx)</span></span>
- <span data-ttu-id="3810a-225">Saiba mais sobre os recursos específicos do Visual Basic de trechos de código do IntelliSense, como personalizar os trechos de código e solução de problemas.</span><span class="sxs-lookup"><span data-stu-id="3810a-225">Learn more about the Visual Basic-specific features of IntelliSense code snippets, such as customizing the snippets and troubleshooting.</span></span> <span data-ttu-id="3810a-226">Para obter mais informações, consulte [trechos de código do Visual Basic IntelliSense](https://msdn.microsoft.com/library/18yz4be4.aspx)</span><span class="sxs-lookup"><span data-stu-id="3810a-226">For more information, see [Visual Basic IntelliSense Code Snippets](https://msdn.microsoft.com/library/18yz4be4.aspx)</span></span>
- <span data-ttu-id="3810a-227">Saiba mais sobre o c#-recursos específicos do IntelliSense, como trechos de código e refatoração.</span><span class="sxs-lookup"><span data-stu-id="3810a-227">Learn more about the C#-specific features of IntelliSense, such as refactoring and code snippets.</span></span> <span data-ttu-id="3810a-228">Para obter mais informações, consulte [Visual c# IntelliSense](https://msdn.microsoft.com/library/43f44291.aspx).</span><span class="sxs-lookup"><span data-stu-id="3810a-228">For more information, see [Visual C# IntelliSense](https://msdn.microsoft.com/library/43f44291.aspx).</span></span>