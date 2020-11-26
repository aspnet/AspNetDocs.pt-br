---
uid: web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
title: Host ASP.NET Web API 2 em uma função de trabalho do Azure-ASP.NET 4. x
author: MikeWasson
description: 'Tutorial: host ASP.NET Web API em uma função de trabalho do Azure, usando OWIN para hospedar automaticamente a estrutura da API Web.'
ms.author: riande
ms.date: 04/02/2014
ms.custom: seoapril2019
ms.assetid: 6980ee2e-d6b0-4a08-8fb6-ab96362dd0e3
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: 06bd33a7d30ab38d851d929d72ae8c539e16873c
ms.sourcegitcommit: 4b78855427f1397df0a7be3559e04ec94a78c308
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96151900"
---
# <a name="host-aspnet-web-api-2-in-an-azure-worker-role"></a><span data-ttu-id="02238-103">Host ASP.NET Web API 2 em uma função de trabalho do Azure</span><span class="sxs-lookup"><span data-stu-id="02238-103">Host ASP.NET Web API 2 in an Azure Worker Role</span></span>

<span data-ttu-id="02238-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="02238-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="02238-105">Este tutorial mostra como hospedar ASP.NET Web API em uma função de trabalho do Azure, usando OWIN para hospedar internamente a estrutura da API Web.</span><span class="sxs-lookup"><span data-stu-id="02238-105">This tutorial shows how to host ASP.NET Web API in an Azure Worker Role, using OWIN to self-host the Web API framework.</span></span>
>
> <span data-ttu-id="02238-106">O [Open Web interface for .net](http://owin.org/) (OWIN) define uma abstração entre servidores Web e aplicativos Web do .net.</span><span class="sxs-lookup"><span data-stu-id="02238-106">[Open Web Interface for .NET](http://owin.org/) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="02238-107">OWIN dissocia o aplicativo Web do servidor, o que torna o OWIN ideal para hospedar internamente um aplicativo Web em seu próprio processo, fora do IIS – por exemplo, dentro de uma função de trabalho do Azure.</span><span class="sxs-lookup"><span data-stu-id="02238-107">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS–for example, inside an Azure worker role.</span></span>
>
> <span data-ttu-id="02238-108">Neste tutorial, você usará o pacote Microsoft. Owin. host. HttpListener, que fornece um servidor HTTP que será usado para hospedar aplicativos OWIN de hospedagem interna.</span><span class="sxs-lookup"><span data-stu-id="02238-108">In this tutorial, you'll use the Microsoft.Owin.Host.HttpListener package, which provides an HTTP server that be used to self-host OWIN applications.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="02238-109">Versões de software usadas no tutorial</span><span class="sxs-lookup"><span data-stu-id="02238-109">Software versions used in the tutorial</span></span>
>
>
> - [<span data-ttu-id="02238-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="02238-110">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="02238-111">API Web 2</span><span class="sxs-lookup"><span data-stu-id="02238-111">Web API 2</span></span>
> - [<span data-ttu-id="02238-112">SDK do Azure para .NET 2.3</span><span class="sxs-lookup"><span data-stu-id="02238-112">Azure SDK for .NET 2.3</span></span>](https://azure.microsoft.com/downloads/)

## <a name="create-a-microsoft-azure-project"></a><span data-ttu-id="02238-113">Criar um projeto Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="02238-113">Create a Microsoft Azure Project</span></span>

<span data-ttu-id="02238-114">Inicie o Visual Studio com privilégios de administrador.</span><span class="sxs-lookup"><span data-stu-id="02238-114">Start Visual Studio with administrator privileges.</span></span> <span data-ttu-id="02238-115">São necessários privilégios de administrador para depurar o aplicativo localmente, usando o emulador de computação do Azure.</span><span class="sxs-lookup"><span data-stu-id="02238-115">Administrator privileges are needed to debug the application locally, using the Azure Compute Emulator.</span></span>

<span data-ttu-id="02238-116">No menu **arquivo** , clique em **novo** e em **projeto**.</span><span class="sxs-lookup"><span data-stu-id="02238-116">On the **File** menu, click **New**, then click **Project**.</span></span> <span data-ttu-id="02238-117">Nos **modelos instalados**, em Visual C#, clique em **nuvem** e, em seguida, clique em **serviço de nuvem do Windows Azure**.</span><span class="sxs-lookup"><span data-stu-id="02238-117">From **Installed Templates**, under Visual C#, click **Cloud** and then click **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="02238-118">Nomeie o projeto "AzureApp" e clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="02238-118">Name the project "AzureApp" and click **OK**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image2.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image1.png)

<span data-ttu-id="02238-119">Na caixa de diálogo **novo serviço de nuvem do Windows Azure** , clique duas vezes em **função de trabalho**.</span><span class="sxs-lookup"><span data-stu-id="02238-119">In the **New Windows Azure Cloud Service** dialog, double-click **Worker Role**.</span></span> <span data-ttu-id="02238-120">Deixe o nome padrão ("WorkerRole1").</span><span class="sxs-lookup"><span data-stu-id="02238-120">Leave the default name ("WorkerRole1").</span></span> <span data-ttu-id="02238-121">Esta etapa adiciona uma função de trabalho à solução.</span><span class="sxs-lookup"><span data-stu-id="02238-121">This step adds a worker role to the solution.</span></span> <span data-ttu-id="02238-122">Clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="02238-122">Click **OK**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image4.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image3.png)

<span data-ttu-id="02238-123">A solução do Visual Studio criada contém dois projetos:</span><span class="sxs-lookup"><span data-stu-id="02238-123">The Visual Studio solution that is created contains two projects:</span></span>

- <span data-ttu-id="02238-124">&quot;AzureApp &quot; define as funções e a configuração para o aplicativo do Azure.</span><span class="sxs-lookup"><span data-stu-id="02238-124">&quot;AzureApp&quot; defines the roles and configuration for the Azure application.</span></span>
- <span data-ttu-id="02238-125">&quot;WorkerRole1 &quot; contém o código para a função de trabalho.</span><span class="sxs-lookup"><span data-stu-id="02238-125">&quot;WorkerRole1&quot; contains the code for the worker role.</span></span>

<span data-ttu-id="02238-126">Em geral, um aplicativo do Azure pode conter várias funções, embora este tutorial use uma única função.</span><span class="sxs-lookup"><span data-stu-id="02238-126">In general, an Azure application can contain multiple roles, although this tutorial uses a single role.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-web-api-and-owin-packages"></a><span data-ttu-id="02238-127">Adicionar a API Web e os pacotes OWIN</span><span class="sxs-lookup"><span data-stu-id="02238-127">Add the Web API and OWIN Packages</span></span>

<span data-ttu-id="02238-128">No menu **ferramentas** , clique em **Gerenciador de pacotes NuGet** e em **console do Gerenciador de pacotes**.</span><span class="sxs-lookup"><span data-stu-id="02238-128">From the **Tools** menu, click **NuGet Package Manager**, then click **Package Manager Console**.</span></span>

<span data-ttu-id="02238-129">Na janela Console do Gerenciador de Pacotes, digite o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="02238-129">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a><span data-ttu-id="02238-130">Adicionar um ponto de extremidade HTTP</span><span class="sxs-lookup"><span data-stu-id="02238-130">Add an HTTP Endpoint</span></span>

<span data-ttu-id="02238-131">Em Gerenciador de Soluções, expanda o projeto AzureApp.</span><span class="sxs-lookup"><span data-stu-id="02238-131">In Solution Explorer, expand the AzureApp project.</span></span> <span data-ttu-id="02238-132">Expanda o nó funções, clique com o botão direito do mouse em WorkerRole1 e selecione **Propriedades**.</span><span class="sxs-lookup"><span data-stu-id="02238-132">Expand the Roles node, right-click WorkerRole1, and select **Properties**.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image6.png)

<span data-ttu-id="02238-133">Clique em **Pontos de Extremidade** e clique em **Adicionar Ponto de Extremidade**.</span><span class="sxs-lookup"><span data-stu-id="02238-133">Click **Endpoints**, and then click **Add Endpoint**.</span></span>

<span data-ttu-id="02238-134">Na lista suspensa **protocolo** , selecione "http".</span><span class="sxs-lookup"><span data-stu-id="02238-134">In the **Protocol** dropdown list, select "http".</span></span> <span data-ttu-id="02238-135">Em porta **pública** e **porta privada**, digite 80.</span><span class="sxs-lookup"><span data-stu-id="02238-135">In **Public Port** and **Private Port**, type 80.</span></span> <span data-ttu-id="02238-136">O número de porta pode ser diferente.</span><span class="sxs-lookup"><span data-stu-id="02238-136">These port numbers can be different.</span></span> <span data-ttu-id="02238-137">A porta pública é o que os clientes usam quando enviam uma solicitação para a função.</span><span class="sxs-lookup"><span data-stu-id="02238-137">The public port is what clients use when they send a request to the role.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image8.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image7.png)

## <a name="configure-web-api-for-self-host"></a><span data-ttu-id="02238-138">Configurar API Web para Self-Host</span><span class="sxs-lookup"><span data-stu-id="02238-138">Configure Web API for Self-Host</span></span>

<span data-ttu-id="02238-139">Em Gerenciador de soluções, clique com o botão direito do mouse no projeto WorkerRole1 e selecione **Adicionar**  /  **classe** para adicionar uma nova classe.</span><span class="sxs-lookup"><span data-stu-id="02238-139">In Solution Explorer, right click the WorkerRole1 project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="02238-140">Nome da classe `Startup`.</span><span class="sxs-lookup"><span data-stu-id="02238-140">Name the class `Startup`.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image9.png)

<span data-ttu-id="02238-141">Substitua todo o código clichê deste arquivo pelo seguinte:</span><span class="sxs-lookup"><span data-stu-id="02238-141">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample2.cs)]

## <a name="add-a-web-api-controller"></a><span data-ttu-id="02238-142">Adicionar um controlador de API Web</span><span class="sxs-lookup"><span data-stu-id="02238-142">Add a Web API Controller</span></span>

<span data-ttu-id="02238-143">Em seguida, adicione uma classe de controlador da API Web.</span><span class="sxs-lookup"><span data-stu-id="02238-143">Next, add a Web API controller class.</span></span> <span data-ttu-id="02238-144">Clique com o botão direito do mouse no projeto WorkerRole1 e selecione **Adicionar**  /  **classe**.</span><span class="sxs-lookup"><span data-stu-id="02238-144">Right-click the WorkerRole1 project and select **Add** / **Class**.</span></span> <span data-ttu-id="02238-145">Nomeie a classe TestController.</span><span class="sxs-lookup"><span data-stu-id="02238-145">Name the class TestController.</span></span> <span data-ttu-id="02238-146">Substitua todo o código clichê deste arquivo pelo seguinte:</span><span class="sxs-lookup"><span data-stu-id="02238-146">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample3.cs)]

<span data-ttu-id="02238-147">Para simplificar, esse controlador define apenas dois métodos GET que retornam texto sem formatação.</span><span class="sxs-lookup"><span data-stu-id="02238-147">For simplicity, this controller just defines two GET methods that return plain text.</span></span>

## <a name="start-the-owin-host"></a><span data-ttu-id="02238-148">Iniciar o host OWIN</span><span class="sxs-lookup"><span data-stu-id="02238-148">Start the OWIN Host</span></span>

<span data-ttu-id="02238-149">Abra o arquivo WorkerRole.cs.</span><span class="sxs-lookup"><span data-stu-id="02238-149">Open the WorkerRole.cs file.</span></span> <span data-ttu-id="02238-150">Essa classe define o código que é executado quando a função de trabalho é iniciada e interrompida.</span><span class="sxs-lookup"><span data-stu-id="02238-150">This class defines the code that runs when the worker role is started and stopped.</span></span>

<span data-ttu-id="02238-151">Adicione a seguinte instrução usando:</span><span class="sxs-lookup"><span data-stu-id="02238-151">Add the following using statement:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample4.cs)]

<span data-ttu-id="02238-152">Adicione um membro **IDisposable** à `WorkerRole` classe:</span><span class="sxs-lookup"><span data-stu-id="02238-152">Add an **IDisposable** member to the `WorkerRole` class:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample5.cs)]

<span data-ttu-id="02238-153">No `OnStart` método, adicione o seguinte código para iniciar o host:</span><span class="sxs-lookup"><span data-stu-id="02238-153">In the `OnStart` method, add the following code to start the host:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample6.cs?highlight=5)]

<span data-ttu-id="02238-154">O método **webapp. Start** inicia o host OWIN.</span><span class="sxs-lookup"><span data-stu-id="02238-154">The **WebApp.Start** method starts the OWIN host.</span></span> <span data-ttu-id="02238-155">O nome da `Startup` classe é um parâmetro de tipo para o método.</span><span class="sxs-lookup"><span data-stu-id="02238-155">The name of the `Startup` class is a type parameter to the method.</span></span> <span data-ttu-id="02238-156">Por convenção, o host chamará o `Configure` método dessa classe.</span><span class="sxs-lookup"><span data-stu-id="02238-156">By convention, the host will call the `Configure` method of this class.</span></span>

<span data-ttu-id="02238-157">Substitua o `OnStop` para descartar da instância do *\_ aplicativo* :</span><span class="sxs-lookup"><span data-stu-id="02238-157">Override the `OnStop` to dispose of the *\_app* instance:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample7.cs)]

<span data-ttu-id="02238-158">Este é o código completo para WorkerRole.cs:</span><span class="sxs-lookup"><span data-stu-id="02238-158">Here is the complete code for WorkerRole.cs:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample8.cs)]

<span data-ttu-id="02238-159">Compile a solução e pressione F5 para executar o aplicativo localmente no emulador de computação do Azure.</span><span class="sxs-lookup"><span data-stu-id="02238-159">Build the solution, and press F5 to run the application locally in the Azure Compute Emulator.</span></span> <span data-ttu-id="02238-160">Dependendo das configurações do firewall, talvez seja necessário permitir o emulador por meio do firewall.</span><span class="sxs-lookup"><span data-stu-id="02238-160">Depending on your firewall settings, you might need to allow the emulator through your firewall.</span></span>

> [!NOTE]
> <span data-ttu-id="02238-161">Se você receber uma exceção semelhante à seguinte, consulte [esta postagem de blog](https://blogs.msdn.com/b/praburaj/archive/2013/11/20/fileloadexception-on-microsoft-owin-when-running-on-worker-role.aspx) para obter uma solução alternativa.</span><span class="sxs-lookup"><span data-stu-id="02238-161">If you get an exception like the following, please see [this blog post](https://blogs.msdn.com/b/praburaj/archive/2013/11/20/fileloadexception-on-microsoft-owin-when-running-on-worker-role.aspx) for a workaround.</span></span> <span data-ttu-id="02238-162">"Não foi possível carregar o arquivo ou o assembly ' Microsoft. Owin, Version = 2.0.2.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35 ' ou uma de suas dependências.</span><span class="sxs-lookup"><span data-stu-id="02238-162">"Could not load file or assembly 'Microsoft.Owin, Version=2.0.2.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35' or one of its dependencies.</span></span> <span data-ttu-id="02238-163">A definição do manifesto do assembly localizado não corresponde à referência do assembly.</span><span class="sxs-lookup"><span data-stu-id="02238-163">The located assembly's manifest definition does not match the assembly reference.</span></span> <span data-ttu-id="02238-164">(Exceção de HRESULT: 0x80131040) "</span><span class="sxs-lookup"><span data-stu-id="02238-164">(Exception from HRESULT: 0x80131040)"</span></span>

<span data-ttu-id="02238-165">O emulador de computação atribui um endereço IP local ao ponto de extremidade.</span><span class="sxs-lookup"><span data-stu-id="02238-165">The compute emulator assigns a local IP address to the endpoint.</span></span> <span data-ttu-id="02238-166">Você pode encontrar o endereço IP exibindo a interface do usuário do emulador de computação.</span><span class="sxs-lookup"><span data-stu-id="02238-166">You can find the IP address by viewing the Compute Emulator UI.</span></span> <span data-ttu-id="02238-167">Clique com o botão direito no ícone do emulador na área de notificação da barra de tarefas e selecione **Mostrar interface do usuário do emulador de computação**.</span><span class="sxs-lookup"><span data-stu-id="02238-167">Right-click the emulator icon in the task bar notification area, and select **Show Compute Emulator UI**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image11.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image10.png)

<span data-ttu-id="02238-168">Localize o endereço IP em implantações de serviço, implantação [ID], detalhes do serviço.</span><span class="sxs-lookup"><span data-stu-id="02238-168">Find the IP address under Service Deployments, deployment [id], Service Details.</span></span> <span data-ttu-id="02238-169">Abra um navegador da Web e navegue até http://<em>Address</em>/Test/1, em que <em>Address</em> é o endereço IP atribuído pelo emulador de computação; por exemplo, `http://127.0.0.1:80/test/1` .</span><span class="sxs-lookup"><span data-stu-id="02238-169">Open a web browser and navigate to http://<em>address</em>/test/1, where <em>address</em> is the IP address assigned by the compute emulator; for example, `http://127.0.0.1:80/test/1`.</span></span> <span data-ttu-id="02238-170">Você deve ver a resposta do controlador da API Web:</span><span class="sxs-lookup"><span data-stu-id="02238-170">You should see the response from the Web API controller:</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image12.png)

## <a name="deploy-to-azure"></a><span data-ttu-id="02238-171">Implantar no Azure</span><span class="sxs-lookup"><span data-stu-id="02238-171">Deploy to Azure</span></span>

<span data-ttu-id="02238-172">Para esta etapa, você deve ter uma conta do Azure.</span><span class="sxs-lookup"><span data-stu-id="02238-172">For this step, you must have an Azure account.</span></span> <span data-ttu-id="02238-173">Se você ainda não tiver uma, poderá criar uma conta de avaliação gratuita em apenas alguns minutos.</span><span class="sxs-lookup"><span data-stu-id="02238-173">If you don't already have one, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="02238-174">Para obter detalhes, consulte [Microsoft Azure avaliação gratuita](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="02238-174">For details, see [Microsoft Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>

<span data-ttu-id="02238-175">Em Gerenciador de Soluções, clique com o botão direito do mouse no projeto AzureApp.</span><span class="sxs-lookup"><span data-stu-id="02238-175">In Solution Explorer, right-click the AzureApp project.</span></span> <span data-ttu-id="02238-176">Selecione **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="02238-176">Select **Publish**.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image13.png)

<span data-ttu-id="02238-177">Se você não estiver conectado à sua conta do Azure, clique em **entrar**.</span><span class="sxs-lookup"><span data-stu-id="02238-177">If you are not signed in to your Azure account, click **Sign In**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image15.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image14.png)

<span data-ttu-id="02238-178">Depois de entrar, escolha uma assinatura e clique em **Avançar**.</span><span class="sxs-lookup"><span data-stu-id="02238-178">After you are signed in, choose a subscription and click **Next**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image17.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image16.png)

<span data-ttu-id="02238-179">Insira um nome para o serviço de nuvem e escolha uma região.</span><span class="sxs-lookup"><span data-stu-id="02238-179">Enter a name for the cloud service and choose a region.</span></span> <span data-ttu-id="02238-180">Clique em **Criar**.</span><span class="sxs-lookup"><span data-stu-id="02238-180">Click **Create**.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image18.png)

<span data-ttu-id="02238-181">Clique em **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="02238-181">Click **Publish**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image20.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image19.png)

<span data-ttu-id="02238-182">A janela log de atividades do Azure mostra o progresso da implantação.</span><span class="sxs-lookup"><span data-stu-id="02238-182">The Azure Activity Log window shows the progress of the deployment.</span></span> <span data-ttu-id="02238-183">Quando o aplicativo for implantado, navegue até http://appname.cloudapp.net/test/1 .</span><span class="sxs-lookup"><span data-stu-id="02238-183">When the app is deployed, browse to http://appname.cloudapp.net/test/1.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image21.png)

## <a name="additional-resources"></a><span data-ttu-id="02238-184">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="02238-184">Additional Resources</span></span>

- [<span data-ttu-id="02238-185">Uma visão geral do projeto Katana</span><span class="sxs-lookup"><span data-stu-id="02238-185">An Overview of Project Katana</span></span>](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)
- [<span data-ttu-id="02238-186">Projeto Katana no GitHub</span><span class="sxs-lookup"><span data-stu-id="02238-186">Katana Project on GitHub</span></span>](https://github.com/aspnet/AspNetKatana)
