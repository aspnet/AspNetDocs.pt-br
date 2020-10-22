---
uid: signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
title: 'Tutorial: transmissão de servidor com Signalr 2 | Microsoft Docs'
author: tdykstra
description: Este tutorial mostra como criar um aplicativo Web que usa o Signalr ASP.NET 2 para fornecer a funcionalidade de difusão do servidor.
ms.author: bradyg
ms.date: 01/02/2019
ms.topic: tutorial
ms.assetid: 1568247f-60b5-4eca-96e0-e661fbb2b273
msc.legacyurl: /signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 14924109fff8db3e537e6bc08b6dc868792ee660
ms.sourcegitcommit: c62ec20b453cee3249eb894ecd75013b57d078f0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/21/2020
ms.locfileid: "92345598"
---
# <a name="tutorial-server-broadcast-with-signalr-2"></a><span data-ttu-id="e2d5c-103">Tutorial: transmissão de servidor com Signalr 2</span><span class="sxs-lookup"><span data-stu-id="e2d5c-103">Tutorial: Server broadcast with SignalR 2</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

<span data-ttu-id="e2d5c-104">Este tutorial mostra como criar um aplicativo Web que usa o Signalr ASP.NET 2 para fornecer a funcionalidade de difusão do servidor.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-104">This tutorial shows how to create a web application that uses ASP.NET SignalR 2 to provide server broadcast functionality.</span></span> <span data-ttu-id="e2d5c-105">A difusão do servidor significa que o servidor inicia as comunicações enviadas aos clientes.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-105">Server broadcast means that the server starts the communications sent to clients.</span></span>

<span data-ttu-id="e2d5c-106">O aplicativo que você criará neste tutorial simula uma cotação de ações, um cenário típico para a funcionalidade de difusão do servidor.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-106">The application that you'll create in this tutorial simulates a stock ticker, a typical scenario for server broadcast functionality.</span></span> <span data-ttu-id="e2d5c-107">Periodicamente, o servidor atualiza de forma aleatória os preços de ações e transmite as atualizações para todos os clientes conectados.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-107">Periodically, the server randomly updates stock prices and broadcast the updates to all connected clients.</span></span> <span data-ttu-id="e2d5c-108">No navegador, os números e símbolos na **alteração** e nas **%** colunas são alterados dinamicamente em resposta a notificações do servidor.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-108">In the browser, the numbers and symbols in the **Change** and **%** columns dynamically change in response to notifications from the server.</span></span> <span data-ttu-id="e2d5c-109">Se você abrir navegadores adicionais para a mesma URL, todos eles mostrarão os mesmos dados e as mesmas alterações aos dados simultaneamente.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-109">If you open additional browsers to the same URL, they all show the same data and the same changes to the data simultaneously.</span></span>

![Criar Web](tutorial-server-broadcast-with-signalr/_static/image1.png)

<span data-ttu-id="e2d5c-111">Neste tutorial, você:</span><span class="sxs-lookup"><span data-stu-id="e2d5c-111">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e2d5c-112">Criar o projeto</span><span class="sxs-lookup"><span data-stu-id="e2d5c-112">Create the project</span></span>
> * <span data-ttu-id="e2d5c-113">Configurar o código de servidor</span><span class="sxs-lookup"><span data-stu-id="e2d5c-113">Set up the server code</span></span>
> * <span data-ttu-id="e2d5c-114">Examinar o código do servidor</span><span class="sxs-lookup"><span data-stu-id="e2d5c-114">Examine the server code</span></span>
> * <span data-ttu-id="e2d5c-115">Configurar o código do cliente</span><span class="sxs-lookup"><span data-stu-id="e2d5c-115">Set up the client code</span></span>
> * <span data-ttu-id="e2d5c-116">Examinar o código do cliente</span><span class="sxs-lookup"><span data-stu-id="e2d5c-116">Examine the client code</span></span>
> * <span data-ttu-id="e2d5c-117">Testar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="e2d5c-117">Test the application</span></span>
> * <span data-ttu-id="e2d5c-118">Habilitar o registro em log</span><span class="sxs-lookup"><span data-stu-id="e2d5c-118">Enable logging</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e2d5c-119">Se você não quiser trabalhar com as etapas de criação do aplicativo, poderá instalar o pacote Signalr. Sample em um novo projeto de aplicativo Web ASP.NET vazio.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-119">If you don't want to work through the steps of building the application, you can install the SignalR.Sample package in a new Empty ASP.NET Web Application project.</span></span> <span data-ttu-id="e2d5c-120">Se você instalar o pacote NuGet sem executar as etapas neste tutorial, deverá seguir as instruções no arquivo *readme.txt* .</span><span class="sxs-lookup"><span data-stu-id="e2d5c-120">If you install the NuGet package without performing the steps in this tutorial, you must follow the instructions in the *readme.txt* file.</span></span> <span data-ttu-id="e2d5c-121">Para executar o pacote, você precisa adicionar uma classe de inicialização OWIN que chama o `ConfigureSignalR` método no pacote instalado.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-121">To run the package you need to add an OWIN startup class which calls the `ConfigureSignalR` method in the installed package.</span></span> <span data-ttu-id="e2d5c-122">Você receberá um erro se não adicionar a classe de inicialização OWIN.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-122">You will receive an error if you do not add the OWIN startup class.</span></span> <span data-ttu-id="e2d5c-123">Consulte a seção [instalar o exemplo de StockTicker](#install-the-stockticker-sample) deste artigo.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-123">See the [Install the StockTicker sample](#install-the-stockticker-sample) section of this article.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e2d5c-124">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="e2d5c-124">Prerequisites</span></span>

* <span data-ttu-id="e2d5c-125">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) com a carga de trabalho **ASP.NET e desenvolvimento para a Web**.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-125">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** workload.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="e2d5c-126">Criar o projeto</span><span class="sxs-lookup"><span data-stu-id="e2d5c-126">Create the project</span></span>

<span data-ttu-id="e2d5c-127">Esta seção mostra como usar o Visual Studio 2017 para criar um aplicativo Web ASP.NET vazio.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-127">This section shows how to use Visual Studio 2017 to create an empty ASP.NET Web Application.</span></span>

1. <span data-ttu-id="e2d5c-128">No Visual Studio, crie um aplicativo Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-128">In Visual Studio, create an ASP.NET Web Application.</span></span>

    ![Criar Web](tutorial-server-broadcast-with-signalr/_static/image2.png)

1. <span data-ttu-id="e2d5c-130">Na janela **novo aplicativo Web ASP.net-signalr. StockTicker** , deixe a seleção **vazia** e selecione **OK**.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-130">In the **New ASP.NET Web Application - SignalR.StockTicker** window, leave **Empty** selected and select **OK**.</span></span>

## <a name="set-up-the-server-code"></a><span data-ttu-id="e2d5c-131">Configurar o código de servidor</span><span class="sxs-lookup"><span data-stu-id="e2d5c-131">Set up the server code</span></span>

<span data-ttu-id="e2d5c-132">Nesta seção, você configura o código que é executado no servidor.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-132">In this section, you set up the code that runs on the server.</span></span>

### <a name="create-the-stock-class"></a><span data-ttu-id="e2d5c-133">Criar a classe Stock</span><span class="sxs-lookup"><span data-stu-id="e2d5c-133">Create the Stock class</span></span>

<span data-ttu-id="e2d5c-134">Comece criando a classe de modelo de *estoque* que você usará para armazenar e transmitir informações sobre um estoque.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-134">You begin by creating the *Stock* model class that you'll use to store and transmit information about a stock.</span></span>

1. <span data-ttu-id="e2d5c-135">Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto e selecione **Adicionar**  >  **classe**.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-135">In **Solution Explorer**, right-click the project and select **Add** > **Class**.</span></span>

1. <span data-ttu-id="e2d5c-136">Nomeie a classe *Stock* e adicione-a ao projeto.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-136">Name the class *Stock* and add it to the project.</span></span>

1. <span data-ttu-id="e2d5c-137">Substitua o código no arquivo *stock.cs* por este código:</span><span class="sxs-lookup"><span data-stu-id="e2d5c-137">Replace the code in the *Stock.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample1.cs)]

    <span data-ttu-id="e2d5c-138">As duas propriedades que você definirá quando criar ações serão `Symbol` (por exemplo, MSFT para Microsoft) e `Price` .</span><span class="sxs-lookup"><span data-stu-id="e2d5c-138">The two properties that you'll set when you create stocks are `Symbol` (for example, MSFT for Microsoft) and `Price`.</span></span> <span data-ttu-id="e2d5c-139">As outras propriedades dependem de como e quando você define `Price` .</span><span class="sxs-lookup"><span data-stu-id="e2d5c-139">The other properties depend on how and when you set `Price`.</span></span> <span data-ttu-id="e2d5c-140">Na primeira vez que você definir `Price` , o valor será propagado para `DayOpen` .</span><span class="sxs-lookup"><span data-stu-id="e2d5c-140">The first time you set `Price`, the value gets propagated to `DayOpen`.</span></span> <span data-ttu-id="e2d5c-141">Depois disso, quando você definir `Price` , o aplicativo calculará os `Change` `PercentChange` valores de propriedade e com base na diferença entre `Price` e `DayOpen` .</span><span class="sxs-lookup"><span data-stu-id="e2d5c-141">After that, when you set `Price`, the app calculates the `Change` and `PercentChange` property values based on the difference between `Price` and `DayOpen`.</span></span>

### <a name="create-the-stocktickerhub-and-stockticker-classes"></a><span data-ttu-id="e2d5c-142">Criar as classes StockTickerHub e StockTicker</span><span class="sxs-lookup"><span data-stu-id="e2d5c-142">Create the StockTickerHub and StockTicker classes</span></span>

<span data-ttu-id="e2d5c-143">Você usará a API do Hub do Signalr para lidar com a interação de servidor para cliente.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-143">You'll use the SignalR Hub API to handle server-to-client interaction.</span></span> <span data-ttu-id="e2d5c-144">Uma `StockTickerHub` classe que deriva da classe signalr `Hub` tratará conexões de recebimento e chamadas de método de clientes.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-144">A `StockTickerHub` class that derives from the SignalR `Hub` class will handle receiving connections and method calls from clients.</span></span> <span data-ttu-id="e2d5c-145">Você também precisa manter os dados de estoque e executar um `Timer` objeto.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-145">You also need to maintain stock data and run a `Timer` object.</span></span> <span data-ttu-id="e2d5c-146">O `Timer` objeto irá disparar periodicamente atualizações de preço independentemente das conexões de cliente.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-146">The `Timer` object will periodically trigger price updates independent of client connections.</span></span> <span data-ttu-id="e2d5c-147">Você não pode colocar essas funções em uma `Hub` classe, pois os hubs são transitórios.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-147">You can't put these functions in a `Hub` class, because Hubs are transient.</span></span> <span data-ttu-id="e2d5c-148">O aplicativo cria uma `Hub` instância de classe para cada tarefa no Hub, como conexões e chamadas do cliente para o servidor.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-148">The app creates a `Hub` class instance for each task on the hub, like connections and calls from the client to the server.</span></span> <span data-ttu-id="e2d5c-149">Portanto, o mecanismo que mantém dados de estoque, atualiza os preços e transmite que as atualizações de preço têm que ser executadas em uma classe separada.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-149">So the mechanism that keeps stock data, updates prices, and broadcasts the price updates has to run in a separate class.</span></span> <span data-ttu-id="e2d5c-150">Você nomeará a classe `StockTicker` .</span><span class="sxs-lookup"><span data-stu-id="e2d5c-150">You'll name the class `StockTicker`.</span></span>

![Transmitindo do StockTicker](tutorial-server-broadcast-with-signalr/_static/image3.png)

<span data-ttu-id="e2d5c-152">Você deseja que apenas uma instância da `StockTicker` classe seja executada no servidor, portanto, você precisará configurar uma referência de cada `StockTickerHub` instância para a `StockTicker` instância singleton.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-152">You only want one instance of the `StockTicker` class to run on the server, so you'll need to set up a reference from each `StockTickerHub` instance to the singleton `StockTicker` instance.</span></span> <span data-ttu-id="e2d5c-153">A `StockTicker` classe deve difundir para os clientes porque ele tem os dados de ações e dispara atualizações, mas `StockTicker` não é uma `Hub` classe.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-153">The `StockTicker` class has to broadcast to clients because it has the stock data and triggers updates, but `StockTicker` isn't a `Hub` class.</span></span> <span data-ttu-id="e2d5c-154">A `StockTicker` classe deve obter uma referência para o objeto de contexto de conexão do Hub signalr.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-154">The `StockTicker` class has to get a reference to the SignalR Hub connection context object.</span></span> <span data-ttu-id="e2d5c-155">Em seguida, ele pode usar o objeto de contexto de conexão do Signalr para transmitir para os clientes.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-155">It can then use the SignalR connection context object to broadcast to clients.</span></span>

#### <a name="create-stocktickerhubcs"></a><span data-ttu-id="e2d5c-156">Criar StockTickerHub.cs</span><span class="sxs-lookup"><span data-stu-id="e2d5c-156">Create StockTickerHub.cs</span></span>

1. <span data-ttu-id="e2d5c-157">Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto e selecione **Adicionar**  >  **novo item**.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-157">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="e2d5c-158">Em **Adicionar novo item-signalr. StockTicker**, selecione **instalar**o  >  **Visual C#**  >  **Web**  >  **signalr** e selecione **classe de Hub do signalr (v2)**.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-158">In **Add New Item - SignalR.StockTicker**, select **Installed** > **Visual C#** > **Web** > **SignalR**  and then select **SignalR Hub Class (v2)**.</span></span>

1. <span data-ttu-id="e2d5c-159">Nomeie a classe *StockTickerHub* e adicione-a ao projeto.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-159">Name the class *StockTickerHub* and add it to the project.</span></span>

    <span data-ttu-id="e2d5c-160">Esta etapa cria o arquivo de classe *StockTickerHub.cs* .</span><span class="sxs-lookup"><span data-stu-id="e2d5c-160">This step creates the *StockTickerHub.cs* class file.</span></span> <span data-ttu-id="e2d5c-161">Simultaneamente, ele adiciona um conjunto de arquivos de script e referências de assembly que dão suporte a Signalr ao projeto.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-161">Simultaneously, it adds a set of script files and assembly references that supports SignalR to the project.</span></span>

1. <span data-ttu-id="e2d5c-162">Substitua o código no arquivo *StockTickerHub.cs* por este código:</span><span class="sxs-lookup"><span data-stu-id="e2d5c-162">Replace the code in the *StockTickerHub.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample2.cs)]

1. <span data-ttu-id="e2d5c-163">Salve o arquivo.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-163">Save the file.</span></span>

<span data-ttu-id="e2d5c-164">O aplicativo usa a classe [Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx) para definir métodos que os clientes podem chamar no servidor.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-164">The app uses the [Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx) class to define methods the clients can call on the server.</span></span> <span data-ttu-id="e2d5c-165">Você está definindo um método: `GetAllStocks()` .</span><span class="sxs-lookup"><span data-stu-id="e2d5c-165">You're defining one method: `GetAllStocks()`.</span></span> <span data-ttu-id="e2d5c-166">Quando um cliente se conecta inicialmente ao servidor, ele chamará esse método para obter uma lista de todas as ações com seus preços atuais.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-166">When a client initially connects to the server, it will call this method to get a list of all of the stocks with their current prices.</span></span> <span data-ttu-id="e2d5c-167">O método pode ser executado de forma síncrona e retornar `IEnumerable<Stock>` porque está retornando dados da memória.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-167">The method can run synchronously and return `IEnumerable<Stock>` because it's returning data from memory.</span></span>

<span data-ttu-id="e2d5c-168">Se o método tivesse que obter os dados fazendo algo que envolvesse esperar, como uma pesquisa de banco de dado ou uma chamada de serviço Web, você especificaria `Task<IEnumerable<Stock>>` como o valor de retorno para habilitar o processamento assíncrono.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-168">If the method had to get the data by doing something that would involve waiting, like a database lookup or a web service call, you would specify `Task<IEnumerable<Stock>>` as the return value to enable asynchronous processing.</span></span> <span data-ttu-id="e2d5c-169">Para obter mais informações, consulte [guia da API de hubs do ASP.net signalr-Server-quando executar de forma assíncrona](../guide-to-the-api/hubs-api-guide-server.md#asyncmethods).</span><span class="sxs-lookup"><span data-stu-id="e2d5c-169">For more information, see [ASP.NET SignalR Hubs API Guide - Server - When to execute asynchronously](../guide-to-the-api/hubs-api-guide-server.md#asyncmethods).</span></span>

<span data-ttu-id="e2d5c-170">O `HubName` atributo especifica como o aplicativo fará referência ao Hub no código JavaScript no cliente.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-170">The `HubName` attribute specifies how the app will reference the Hub in JavaScript code on the client.</span></span> <span data-ttu-id="e2d5c-171">O nome padrão no cliente se você não usar esse atributo, será uma versão camelCase do nome da classe, que nesse caso seria `stockTickerHub` .</span><span class="sxs-lookup"><span data-stu-id="e2d5c-171">The default name on the client if you don't use this attribute, is a camelCase version of the class name, which in this case would be `stockTickerHub`.</span></span>

<span data-ttu-id="e2d5c-172">Como você verá posteriormente ao criar a `StockTicker` classe, o aplicativo cria uma instância singleton dessa classe em sua `Instance` propriedade estática.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-172">As you'll see later when you create the `StockTicker` class, the app creates a singleton instance of that class in its static `Instance` property.</span></span> <span data-ttu-id="e2d5c-173">Essa instância singleton do `StockTicker` está na memória, independentemente de quantos clientes se conectam ou se desconectam.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-173">That singleton instance of `StockTicker` is in memory no matter how many clients connect or disconnect.</span></span> <span data-ttu-id="e2d5c-174">Essa instância é o que o `GetAllStocks()` método usa para retornar informações de ações atuais.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-174">That instance is what the `GetAllStocks()` method uses to return current stock information.</span></span>

#### <a name="create-stocktickercs"></a><span data-ttu-id="e2d5c-175">Criar StockTicker.cs</span><span class="sxs-lookup"><span data-stu-id="e2d5c-175">Create StockTicker.cs</span></span>

1. <span data-ttu-id="e2d5c-176">Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto e selecione **Adicionar**  >  **classe**.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-176">In **Solution Explorer**, right-click the project and select **Add** > **Class**.</span></span>

1. <span data-ttu-id="e2d5c-177">Nomeie a classe *StockTicker* e adicione-a ao projeto.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-177">Name the class *StockTicker* and add it to the project.</span></span>

1. <span data-ttu-id="e2d5c-178">Substitua o código no arquivo *StockTicker.cs* por este código:</span><span class="sxs-lookup"><span data-stu-id="e2d5c-178">Replace the code in the *StockTicker.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample3.cs)]

<span data-ttu-id="e2d5c-179">Como todos os threads executarão a mesma instância do código StockTicker, a classe StockTicker deve ser thread-safe.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-179">Since all threads will be running the same instance of StockTicker code, the StockTicker class has to be thread-safe.</span></span>

### <a name="examine-the-server-code"></a><span data-ttu-id="e2d5c-180">Examinar o código do servidor</span><span class="sxs-lookup"><span data-stu-id="e2d5c-180">Examine the server code</span></span>

<span data-ttu-id="e2d5c-181">Se você examinar o código do servidor, ele o ajudará a entender como o aplicativo funciona.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-181">If you examine the server code, it will help you understand how the app works.</span></span>

#### <a name="storing-the-singleton-instance-in-a-static-field"></a><span data-ttu-id="e2d5c-182">Armazenando a instância singleton em um campo estático</span><span class="sxs-lookup"><span data-stu-id="e2d5c-182">Storing the singleton instance in a static field</span></span>

<span data-ttu-id="e2d5c-183">O código inicializa o `_instance` campo estático que faz o backup da `Instance` propriedade com uma instância da classe.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-183">The code initializes the static `_instance` field that backs the `Instance` property with an instance of the class.</span></span> <span data-ttu-id="e2d5c-184">Como o construtor é privado, é a única instância da classe que o aplicativo pode criar.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-184">Because the constructor is private, it's the only instance of the class that the app can create.</span></span> <span data-ttu-id="e2d5c-185">O aplicativo usa a [inicialização lenta](/dotnet/framework/performance/lazy-initialization) para o `_instance` campo.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-185">The app uses [Lazy initialization](/dotnet/framework/performance/lazy-initialization) for the `_instance` field.</span></span> <span data-ttu-id="e2d5c-186">Isso não é por motivos de desempenho.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-186">It's not for performance reasons.</span></span> <span data-ttu-id="e2d5c-187">É para garantir que a criação da instância seja thread-safe.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-187">It's to make sure the instance creation is thread-safe.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample4.cs)]

<span data-ttu-id="e2d5c-188">Cada vez que um cliente se conecta ao servidor, uma nova instância da classe StockTickerHub em execução em um thread separado Obtém a instância singleton StockTicker da `StockTicker.Instance` propriedade estática, como vimos anteriormente na `StockTickerHub` classe.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-188">Each time a client connects to the server, a new instance of the StockTickerHub class running in a separate thread gets the StockTicker singleton instance from the `StockTicker.Instance` static property, as you saw earlier in the `StockTickerHub` class.</span></span>

#### <a name="storing-stock-data-in-a-concurrentdictionary"></a><span data-ttu-id="e2d5c-189">Armazenando dados de ações em um ConcurrentDictionary</span><span class="sxs-lookup"><span data-stu-id="e2d5c-189">Storing stock data in a ConcurrentDictionary</span></span>

<span data-ttu-id="e2d5c-190">O construtor inicializa a `_stocks` coleção com alguns dados de estoque de exemplo e `GetAllStocks` retorna as ações.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-190">The constructor initializes the `_stocks` collection with some sample stock data, and `GetAllStocks` returns the stocks.</span></span> <span data-ttu-id="e2d5c-191">Como vimos anteriormente, essa coleção de ações é retornada por `StockTickerHub.GetAllStocks` , que é um método de servidor na `Hub` classe que os clientes podem chamar.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-191">As you saw earlier, this collection of stocks is returned by `StockTickerHub.GetAllStocks`, which is a server method in the `Hub` class that clients can call.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample5.cs)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample6.cs)]

<span data-ttu-id="e2d5c-192">A coleção de ações é definida como um tipo de [ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx) para segurança de thread.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-192">The stocks collection is defined as a [ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx) type for thread safety.</span></span> <span data-ttu-id="e2d5c-193">Como alternativa, você pode usar um objeto [Dictionary](https://msdn.microsoft.com/library/xfhwa508.aspx) e bloquear explicitamente o dicionário quando fizer alterações nele.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-193">As an alternative, you could use a [Dictionary](https://msdn.microsoft.com/library/xfhwa508.aspx) object and explicitly lock the dictionary when you make changes to it.</span></span>

<span data-ttu-id="e2d5c-194">Para este aplicativo de exemplo, não há problema em armazenar dados de aplicativo na memória e perder os dados quando o aplicativo descarta a  `StockTicker` instância.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-194">For this sample application, it's OK to store application data in memory and to lose the data when the app disposes of the  `StockTicker` instance.</span></span> <span data-ttu-id="e2d5c-195">Em um aplicativo real, você trabalharia com um armazenamento de dados de back-end como um banco de dado.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-195">In a real application, you would work with a back-end data store like a database.</span></span>

#### <a name="periodically-updating-stock-prices"></a><span data-ttu-id="e2d5c-196">Atualizando periodicamente os preços de ações</span><span class="sxs-lookup"><span data-stu-id="e2d5c-196">Periodically updating stock prices</span></span>

<span data-ttu-id="e2d5c-197">O Construtor inicia um `Timer` objeto que chama periodicamente os métodos que atualizam os preços de ações em uma base aleatória.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-197">The constructor starts up a `Timer` object that periodically calls methods that update stock prices on a random basis.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample7.cs)]

 <span data-ttu-id="e2d5c-198">`Timer` chamadas `UpdateStockPrices` , que passa em NULL no parâmetro State.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-198">`Timer` calls `UpdateStockPrices`, which passes in null in the state parameter.</span></span> <span data-ttu-id="e2d5c-199">Antes de atualizar os preços, o aplicativo assume um bloqueio no `_updateStockPricesLock` objeto.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-199">Before updating prices, the app takes a lock on the `_updateStockPricesLock` object.</span></span> <span data-ttu-id="e2d5c-200">O código verifica se outro thread já está atualizando os preços e, em seguida, chama `TryUpdateStockPrice` em cada estoque na lista.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-200">The code checks if another thread is already updating prices, and then it calls `TryUpdateStockPrice` on each stock in the list.</span></span> <span data-ttu-id="e2d5c-201">O `TryUpdateStockPrice` método decide se o preço da ação deve ser alterado e o quanto alterar.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-201">The `TryUpdateStockPrice` method decides whether to change the stock price, and how much to change it.</span></span> <span data-ttu-id="e2d5c-202">Se o preço de ações for alterado, o aplicativo chamará `BroadcastStockPrice` para transmitir a alteração de preço de estoque para todos os clientes conectados.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-202">If the stock price changes, the app calls `BroadcastStockPrice` to broadcast the stock price change to all connected clients.</span></span>

<span data-ttu-id="e2d5c-203">O `_updatingStockPrices` sinalizador designado como [volátil](https://msdn.microsoft.com/library/x13ttww7.aspx) para garantir que ele seja thread-safe.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-203">The `_updatingStockPrices` flag designated [volatile](https://msdn.microsoft.com/library/x13ttww7.aspx) to make sure it is thread-safe.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample8.cs)]

<span data-ttu-id="e2d5c-204">Em um aplicativo real, o `TryUpdateStockPrice` método chamaria um serviço Web para pesquisar o preço.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-204">In a real application, the `TryUpdateStockPrice` method would call a web service to look up the price.</span></span> <span data-ttu-id="e2d5c-205">Nesse código, o aplicativo usa um gerador de números aleatórios para fazer alterações aleatoriamente.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-205">In this code, the app uses a random number generator to make changes randomly.</span></span>

#### <a name="getting-the-signalr-context-so-that-the-stockticker-class-can-broadcast-to-clients"></a><span data-ttu-id="e2d5c-206">Obter o contexto do Signalr para que a classe StockTicker possa difundir para os clientes</span><span class="sxs-lookup"><span data-stu-id="e2d5c-206">Getting the SignalR context so that the StockTicker class can broadcast to clients</span></span>

<span data-ttu-id="e2d5c-207">Como as alterações de preço são originadas aqui no `StockTicker` objeto, é o objeto que precisa chamar um `updateStockPrice` método em todos os clientes conectados.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-207">Because the price changes originate here in the `StockTicker` object, it's the object that needs to call an `updateStockPrice` method on all connected clients.</span></span> <span data-ttu-id="e2d5c-208">Em uma `Hub` classe, você tem uma API para chamar métodos de cliente, mas `StockTicker` não deriva da `Hub` classe e não tem uma referência a nenhum `Hub` objeto.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-208">In a `Hub` class, you have an API for calling client methods, but `StockTicker` doesn't derive from the `Hub` class and doesn't have a reference to any `Hub` object.</span></span> <span data-ttu-id="e2d5c-209">Para transmitir para clientes conectados, a `StockTicker` classe deve obter a instância de contexto do signalr para a `StockTickerHub` classe e usá-la para chamar métodos em clientes.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-209">To broadcast to connected clients, the `StockTicker` class has to get the SignalR context instance for the `StockTickerHub` class and use that to call methods on clients.</span></span>

<span data-ttu-id="e2d5c-210">O código obtém uma referência ao contexto do Signalr quando ele cria a instância da classe singleton, passa essa referência para o construtor e o Construtor a coloca na `Clients` propriedade.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-210">The code gets a reference to the SignalR context when it creates the singleton class instance, passes that reference to the constructor, and the constructor puts it in the `Clients` property.</span></span>

<span data-ttu-id="e2d5c-211">Há dois motivos pelos quais você deseja obter o contexto apenas uma vez: obter o contexto é uma tarefa cara e obtê-lo uma vez garante que o aplicativo preserva a ordem pretendida de mensagens enviadas aos clientes.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-211">There are two reasons why you want to get the context only once: getting the context is an expensive task, and getting it once makes sure the app preserves the intended order of messages sent to the clients.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample9.cs)]

<span data-ttu-id="e2d5c-212">Obter a `Clients` Propriedade do contexto e colocá-la na `StockTickerClient` propriedade permite que você escreva o código para chamar métodos de cliente que têm a mesma aparência em uma `Hub` classe.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-212">Getting the `Clients` property of the context and putting it in the `StockTickerClient` property lets you write code to call client methods that looks the same as it would in a `Hub` class.</span></span> <span data-ttu-id="e2d5c-213">Por exemplo, para difundir para todos os clientes que você pode escrever `Clients.All.updateStockPrice(stock)` .</span><span class="sxs-lookup"><span data-stu-id="e2d5c-213">For instance, to broadcast to all clients you can write `Clients.All.updateStockPrice(stock)`.</span></span>

<span data-ttu-id="e2d5c-214">O `updateStockPrice` método que você está chamando `BroadcastStockPrice` ainda não existe.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-214">The `updateStockPrice` method that you're calling in `BroadcastStockPrice` doesn't exist yet.</span></span> <span data-ttu-id="e2d5c-215">Você o adicionará posteriormente quando escrever o código que é executado no cliente.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-215">You'll add it later when you write code that runs on the client.</span></span> <span data-ttu-id="e2d5c-216">Você pode consultar `updateStockPrice` aqui porque `Clients.All` é dinâmico, o que significa que o aplicativo avaliará a expressão em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-216">You can refer to `updateStockPrice` here because `Clients.All` is dynamic, which means the app will evaluate the expression at runtime.</span></span> <span data-ttu-id="e2d5c-217">Quando essa chamada de método for executada, o Signalr enviará o nome do método e o valor do parâmetro para o cliente e, se o cliente tiver um método chamado `updateStockPrice` , o aplicativo chamará esse método e passará o valor do parâmetro para ele.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-217">When this method call executes, SignalR will send the method name and the parameter value to the client, and if the client has a method named `updateStockPrice`, the app will call that method and pass the parameter value to it.</span></span>

<span data-ttu-id="e2d5c-218">`Clients.All` significa enviar para todos os clientes.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-218">`Clients.All` means send to all clients.</span></span> <span data-ttu-id="e2d5c-219">O signalr oferece outras opções para especificar a quais clientes ou grupos de clientes enviar.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-219">SignalR gives you other options to specify which clients or groups of clients to send to.</span></span> <span data-ttu-id="e2d5c-220">Para obter mais informações, consulte [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="e2d5c-220">For more information, see [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx).</span></span>

### <a name="register-the-signalr-route"></a><span data-ttu-id="e2d5c-221">Registrar a rota do Signalr</span><span class="sxs-lookup"><span data-stu-id="e2d5c-221">Register the SignalR route</span></span>

<span data-ttu-id="e2d5c-222">O servidor precisa saber qual URL interceptar e direcionar para o Signalr.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-222">The server needs to know which URL to intercept and direct to SignalR.</span></span> <span data-ttu-id="e2d5c-223">Para fazer isso, adicione uma classe de inicialização OWIN:</span><span class="sxs-lookup"><span data-stu-id="e2d5c-223">To do that, add an OWIN startup class:</span></span>

1. <span data-ttu-id="e2d5c-224">Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto e selecione **Adicionar**  >  **novo item**.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-224">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="e2d5c-225">Em **Adicionar novo item-signalr. StockTicker** , selecione **instalar**  >  **Visual C#**  >  **Web** e, em seguida, selecione **classe de inicialização OWIN**.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-225">In **Add New Item - SignalR.StockTicker** select **Installed** > **Visual C#** > **Web** and then select **OWIN Startup Class**.</span></span>

1. <span data-ttu-id="e2d5c-226">Nomeie a classe como *Startup* e selecione **OK**.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-226">Name the class *Startup* and select **OK**.</span></span>

1. <span data-ttu-id="e2d5c-227">Substitua o código padrão no arquivo *Startup.cs* por este código:</span><span class="sxs-lookup"><span data-stu-id="e2d5c-227">Replace the default code in the *Startup.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample10.cs)]

<span data-ttu-id="e2d5c-228">Agora você concluiu a configuração do código do servidor.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-228">You have now finished setting up the server code.</span></span> <span data-ttu-id="e2d5c-229">Na próxima seção, você configurará o cliente.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-229">In the next section, you'll set up the client.</span></span>

## <a name="set-up-the-client-code"></a><span data-ttu-id="e2d5c-230">Configurar o código do cliente</span><span class="sxs-lookup"><span data-stu-id="e2d5c-230">Set up the client code</span></span>

<span data-ttu-id="e2d5c-231">Nesta seção, você configura o código que é executado no cliente.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-231">In this section, you set up the code that runs on the client.</span></span>

### <a name="create-the-html-page-and-javascript-file"></a><span data-ttu-id="e2d5c-232">Criar a página HTML e o arquivo JavaScript</span><span class="sxs-lookup"><span data-stu-id="e2d5c-232">Create the HTML page and JavaScript file</span></span>

<span data-ttu-id="e2d5c-233">A página HTML exibirá os dados e o arquivo JavaScript organizará os dados.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-233">The HTML page will display the data and the JavaScript file will organize the data.</span></span>

#### <a name="create-stocktickerhtml"></a><span data-ttu-id="e2d5c-234">Criar StockTicker.html</span><span class="sxs-lookup"><span data-stu-id="e2d5c-234">Create StockTicker.html</span></span>

<span data-ttu-id="e2d5c-235">Primeiro, você adicionará o cliente HTML.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-235">First, you'll add the HTML client.</span></span>

1. <span data-ttu-id="e2d5c-236">Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto e selecione **Adicionar**  >  **página HTML**.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-236">In **Solution Explorer**, right-click the project and select **Add** > **HTML Page**.</span></span>

1. <span data-ttu-id="e2d5c-237">Nomeie o arquivo *StockTicker* e selecione **OK**.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-237">Name the file *StockTicker* and select **OK**.</span></span>

1. <span data-ttu-id="e2d5c-238">Substitua o código padrão no arquivo *StockTicker.html* por este código:</span><span class="sxs-lookup"><span data-stu-id="e2d5c-238">Replace the default code in the *StockTicker.html* file with this code:</span></span>

    [!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample11.html?highlight=40-43)]

    <span data-ttu-id="e2d5c-239">O HTML cria uma tabela com cinco colunas, uma linha de cabeçalho e uma linha de dados com uma única célula que abrange todas as cinco colunas.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-239">The HTML creates a table with five columns, a header row, and a data row with a single cell that spans all five columns.</span></span> <span data-ttu-id="e2d5c-240">A linha de dados mostra "carregando..." momentaneamente quando o aplicativo é iniciado.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-240">The data row shows "loading..." momentarily when the app starts.</span></span> <span data-ttu-id="e2d5c-241">O código JavaScript removerá essa linha e adicionará suas linhas locais com os dados de estoque recuperados do servidor.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-241">JavaScript code will remove that row and add in its place rows with stock data retrieved from the server.</span></span>

    <span data-ttu-id="e2d5c-242">As marcas de script especificam:</span><span class="sxs-lookup"><span data-stu-id="e2d5c-242">The script tags specify:</span></span>

    * <span data-ttu-id="e2d5c-243">O arquivo de script jQuery.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-243">The jQuery script file.</span></span>

    * <span data-ttu-id="e2d5c-244">O arquivo de script do Signalr Core.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-244">The SignalR core script file.</span></span>

    * <span data-ttu-id="e2d5c-245">O arquivo de script dos proxies do Signalr.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-245">The SignalR proxies script file.</span></span>

    * <span data-ttu-id="e2d5c-246">Um arquivo de script StockTicker que você criará mais tarde.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-246">A StockTicker script file that you'll create later.</span></span>

    <span data-ttu-id="e2d5c-247">O aplicativo gera dinamicamente o arquivo de script de proxies do Signalr.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-247">The app dynamically generates the SignalR proxies script file.</span></span> <span data-ttu-id="e2d5c-248">Ele especifica a URL "/signalr/hubs" e define métodos de proxy para os métodos na classe Hub, nesse caso, para `StockTickerHub.GetAllStocks` .</span><span class="sxs-lookup"><span data-stu-id="e2d5c-248">It specifies the "/signalr/hubs" URL and defines proxy methods for the methods on the Hub class, in this case, for `StockTickerHub.GetAllStocks`.</span></span> <span data-ttu-id="e2d5c-249">Se preferir, você pode gerar esse arquivo JavaScript manualmente usando os [utilitários do signalr](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/).</span><span class="sxs-lookup"><span data-stu-id="e2d5c-249">If you prefer, you can generate this JavaScript file manually by using [SignalR Utilities](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/).</span></span> <span data-ttu-id="e2d5c-250">Não se esqueça de desabilitar a criação de arquivo dinâmico na `MapHubs` chamada do método.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-250">Don't forget to disable dynamic file creation in the `MapHubs` method call.</span></span>

1. <span data-ttu-id="e2d5c-251">Em **Gerenciador de soluções**, expanda **scripts**.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-251">In **Solution Explorer**, expand **Scripts**.</span></span>

    <span data-ttu-id="e2d5c-252">As bibliotecas de script para jQuery e Signalr são visíveis no projeto.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-252">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="e2d5c-253">O Gerenciador de pacotes instalará uma versão mais recente dos scripts do Signalr.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-253">The package manager will install a later version of the SignalR scripts.</span></span>

1. <span data-ttu-id="e2d5c-254">Atualize as referências de script no bloco de código para corresponder às versões dos arquivos de script no projeto.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-254">Update the script references in the code block to correspond to the versions of the script files in the project.</span></span>

1. <span data-ttu-id="e2d5c-255">Em **Gerenciador de soluções**, clique com o botão direito do mouse em *StockTicker.html*e selecione **definir como página inicial**.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-255">In **Solution Explorer**, right-click *StockTicker.html*, and then select **Set as Start Page**.</span></span>

#### <a name="create-stocktickerjs"></a><span data-ttu-id="e2d5c-256">Criar StockTicker.js</span><span class="sxs-lookup"><span data-stu-id="e2d5c-256">Create StockTicker.js</span></span>

<span data-ttu-id="e2d5c-257">Agora, crie o arquivo JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-257">Now create the JavaScript file.</span></span>

1. <span data-ttu-id="e2d5c-258">Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto e selecione **Adicionar**  >  **arquivo JavaScript**.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-258">In **Solution Explorer**, right-click the project and select **Add** > **JavaScript File**.</span></span>

1. <span data-ttu-id="e2d5c-259">Nomeie o arquivo *StockTicker* e selecione **OK**.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-259">Name the file *StockTicker* and select **OK**.</span></span>

1. <span data-ttu-id="e2d5c-260">Adicione este código ao arquivo de *StockTicker.js* :</span><span class="sxs-lookup"><span data-stu-id="e2d5c-260">Add this code to the *StockTicker.js* file:</span></span>

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample12.js)]

### <a name="examine-the-client-code"></a><span data-ttu-id="e2d5c-261">Examinar o código do cliente</span><span class="sxs-lookup"><span data-stu-id="e2d5c-261">Examine the client code</span></span>

<span data-ttu-id="e2d5c-262">Se você examinar o código do cliente, ele ajudará você a aprender como o código do cliente interage com o código do servidor para fazer o aplicativo funcionar.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-262">If you examine the client code, it will help you learn how the client code interacts with the server code to make the app work.</span></span>

#### <a name="starting-the-connection"></a><span data-ttu-id="e2d5c-263">Iniciando a conexão</span><span class="sxs-lookup"><span data-stu-id="e2d5c-263">Starting the connection</span></span>

<span data-ttu-id="e2d5c-264">`$.connection` refere-se aos proxies do Signalr.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-264">`$.connection` refers to the SignalR proxies.</span></span> <span data-ttu-id="e2d5c-265">O código obtém uma referência ao proxy para a `StockTickerHub` classe e a coloca na `ticker` variável.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-265">The code gets a reference to the proxy for the `StockTickerHub` class and puts it in the `ticker` variable.</span></span> <span data-ttu-id="e2d5c-266">O nome do proxy é o nome que foi definido pelo `HubName` atributo:</span><span class="sxs-lookup"><span data-stu-id="e2d5c-266">The proxy name is the name that was set by the `HubName` attribute:</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample13.js)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample14.cs)]

<span data-ttu-id="e2d5c-267">Depois de definir todas as variáveis e funções, a última linha de código no arquivo Inicializa a conexão do Signalr chamando a função Signalr `start` .</span><span class="sxs-lookup"><span data-stu-id="e2d5c-267">After you define all the variables and functions, the last line of code in the file initializes the SignalR connection by calling the SignalR `start` function.</span></span> <span data-ttu-id="e2d5c-268">A `start` função é executada de forma assíncrona e retorna um [objeto do jQuery adiado](http://api.jquery.com/category/deferred-object/).</span><span class="sxs-lookup"><span data-stu-id="e2d5c-268">The `start` function executes asynchronously and returns a [jQuery Deferred object](http://api.jquery.com/category/deferred-object/).</span></span> <span data-ttu-id="e2d5c-269">Você pode chamar a função Done para especificar a função a ser chamada quando o aplicativo concluir a ação assíncrona.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-269">You can call the done function to specify the function to call when the app finishes the asynchronous action.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample15.js)]

#### <a name="getting-all-the-stocks"></a><span data-ttu-id="e2d5c-270">Obtendo todas as ações</span><span class="sxs-lookup"><span data-stu-id="e2d5c-270">Getting all the stocks</span></span>

<span data-ttu-id="e2d5c-271">A `init` função chama a `getAllStocks` função no servidor e usa as informações que o servidor retorna para atualizar a tabela de estoque.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-271">The `init` function calls the `getAllStocks` function on the server and uses the information that the server returns to update the stock table.</span></span> <span data-ttu-id="e2d5c-272">Observe que, por padrão, você precisa usar camelCasing no cliente, embora o nome do método seja Pascal-case no servidor.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-272">Notice that, by default, you have to use camelCasing on the client even though the method name is pascal-cased on the server.</span></span> <span data-ttu-id="e2d5c-273">A regra camelCasing só se aplica a métodos, não a objetos.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-273">The camelCasing rule only applies to methods, not objects.</span></span> <span data-ttu-id="e2d5c-274">Por exemplo, você faz referência a `stock.Symbol` e `stock.Price` , não `stock.symbol` ou `stock.price` .</span><span class="sxs-lookup"><span data-stu-id="e2d5c-274">For example, you refer to `stock.Symbol` and `stock.Price`, not `stock.symbol` or `stock.price`.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample16.js)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample17.cs)]

<span data-ttu-id="e2d5c-275">No `init` método, o aplicativo cria HTML para uma linha de tabela para cada objeto de estoque recebido do servidor chamando as `formatStock` Propriedades de formato do `stock` objeto e, em seguida, chamando `supplant` para substituir os espaços reservados na `rowTemplate` variável pelos `stock` valores de Propriedade do objeto.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-275">In the `init` method, the app creates HTML for a table row for each stock object received from the server by calling `formatStock` to format properties of the `stock` object, and then by calling `supplant` to replace placeholders in the `rowTemplate` variable with the `stock` object property values.</span></span> <span data-ttu-id="e2d5c-276">O HTML resultante é então anexado à tabela stock.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-276">The resulting HTML is then appended to the stock table.</span></span>

> [!NOTE]
> <span data-ttu-id="e2d5c-277">Você chama `init` passando-o como uma `callback` função que é executada após a conclusão da `start` função assíncrona.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-277">You call `init` by passing it in as a `callback` function that executes after the asynchronous `start` function finishes.</span></span> <span data-ttu-id="e2d5c-278">Se você chamou `init` como uma instrução JavaScript separada após chamar `start` , a função falhará porque ela seria executada imediatamente sem esperar que a função start termine de estabelecer a conexão.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-278">If you called `init` as a separate JavaScript statement after calling `start`, the function would fail because it would run immediately without waiting for the start function to finish establishing the connection.</span></span> <span data-ttu-id="e2d5c-279">Nesse caso, a `init` função tentaria chamar a `getAllStocks` função antes que o aplicativo estabeleça uma conexão de servidor.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-279">In that case, the `init` function would try to call the `getAllStocks` function before the app establishes a server connection.</span></span>

#### <a name="getting-updated-stock-prices"></a><span data-ttu-id="e2d5c-280">Obtendo preços de ações atualizados</span><span class="sxs-lookup"><span data-stu-id="e2d5c-280">Getting updated stock prices</span></span>

<span data-ttu-id="e2d5c-281">Quando o servidor altera o preço de uma ação, ele chama o `updateStockPrice` em clientes conectados.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-281">When the server changes a stock's price, it calls the `updateStockPrice` on connected clients.</span></span> <span data-ttu-id="e2d5c-282">O aplicativo adiciona a função à propriedade de cliente do `stockTicker` proxy para disponibilizá-la a chamadas do servidor.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-282">The app adds the function to the client property of the `stockTicker` proxy to make it available to calls from the server.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample18.js)]

<span data-ttu-id="e2d5c-283">A `updateStockPrice` função formata um objeto de estoque recebido do servidor para uma linha de tabela da mesma maneira que na `init` função.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-283">The `updateStockPrice` function formats a stock object received from the server into a table row the same way as in the `init` function.</span></span> <span data-ttu-id="e2d5c-284">Em vez de acrescentar a linha à tabela, ela localiza a linha atual da ação na tabela e substitui essa linha pela nova.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-284">Instead of appending the row to the table, it finds the stock's current row in the table and replaces that row with the new one.</span></span>

## <a name="test-the-application"></a><span data-ttu-id="e2d5c-285">Testar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="e2d5c-285">Test the application</span></span>

<span data-ttu-id="e2d5c-286">Você pode testar o aplicativo para verificar se ele está funcionando.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-286">You can test the app to make sure it's working.</span></span> <span data-ttu-id="e2d5c-287">Você verá que todas as janelas do navegador exibem a tabela de estoque ao vivo com os preços de estoque flutuando.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-287">You'll see all browser windows display the live stock table with stock prices fluctuating.</span></span>

1. <span data-ttu-id="e2d5c-288">Na barra de ferramentas, ative a **depuração de script** e, em seguida, selecione o botão reproduzir para executar o aplicativo no modo de depuração.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-288">In the toolbar, turn on **Script Debugging** and then select the play button to run the app in Debug mode.</span></span>

    ![Captura de tela do usuário que está ligando o modo de depuração e selecionando reproduzir.](tutorial-server-broadcast-with-signalr/_static/image4.png)

    <span data-ttu-id="e2d5c-290">Uma janela do navegador será aberta exibindo a **tabela de estoque ao vivo**.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-290">A browser window will open displaying the **Live Stock Table**.</span></span> <span data-ttu-id="e2d5c-291">A tabela Stock inicialmente mostra o "carregando..." em seguida, após um curto período de tempo, o aplicativo mostra os dados iniciais de ações e, em seguida, os preços de ações começam a ser alterados.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-291">The stock table initially shows the "loading..." line, then, after a short time, the app shows the initial stock data, and then the stock prices start to change.</span></span>

1. <span data-ttu-id="e2d5c-292">Copie a URL do navegador, abra dois outros navegadores e cole as URLs nas barras de endereço.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-292">Copy the URL from the browser, open two other browsers, and paste the URLs into the address bars.</span></span>

    <span data-ttu-id="e2d5c-293">A exibição de estoque inicial é a mesma do primeiro navegador e as alterações acontecem simultaneamente.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-293">The initial stock display is the same as the first browser and changes happen simultaneously.</span></span>

1. <span data-ttu-id="e2d5c-294">Feche todos os navegadores, abra um novo navegador e vá para a mesma URL.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-294">Close all browsers, open a new browser, and go to the same URL.</span></span>

    <span data-ttu-id="e2d5c-295">O objeto singleton StockTicker continuou a ser executado no servidor.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-295">The StockTicker singleton object continued to run in the server.</span></span> <span data-ttu-id="e2d5c-296">A **tabela de estoque ao vivo** mostra que as ações continuaram a ser alteradas.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-296">The **Live Stock Table** shows that the stocks have continued to change.</span></span> <span data-ttu-id="e2d5c-297">Você não vê a tabela inicial com valores de alteração zero.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-297">You don't see the initial table with zero change figures.</span></span>

1. <span data-ttu-id="e2d5c-298">Feche o navegador.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-298">Close the browser.</span></span>

## <a name="enable-logging"></a><span data-ttu-id="e2d5c-299">Habilitar o registro em log</span><span class="sxs-lookup"><span data-stu-id="e2d5c-299">Enable logging</span></span>

<span data-ttu-id="e2d5c-300">O signalr tem uma função de log interna que você pode habilitar no cliente para auxiliar na solução de problemas.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-300">SignalR has a built-in logging function that you can enable on the client to aid in troubleshooting.</span></span> <span data-ttu-id="e2d5c-301">Nesta seção, você habilitará o registro em log e verá exemplos que mostram como os logs informam quais dos seguintes métodos de transporte o Signalr está usando:</span><span class="sxs-lookup"><span data-stu-id="e2d5c-301">In this section, you enable logging and see examples that show how logs tell you which of the following transport methods SignalR is using:</span></span>

* <span data-ttu-id="e2d5c-302">[WebSockets](http://en.wikipedia.org/wiki/WebSocket), com suporte do IIS 8 e navegadores atuais.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-302">[WebSockets](http://en.wikipedia.org/wiki/WebSocket), supported by IIS 8 and current browsers.</span></span>

* <span data-ttu-id="e2d5c-303">[Eventos enviados pelo servidor](http://en.wikipedia.org/wiki/Server-sent_events), com suporte de navegadores diferentes do Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-303">[Server-sent events](http://en.wikipedia.org/wiki/Server-sent_events), supported by browsers other than Internet Explorer.</span></span>

* <span data-ttu-id="e2d5c-304">[Quadro contínuo](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe), com suporte do Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-304">[Forever frame](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe), supported by Internet Explorer.</span></span>

* <span data-ttu-id="e2d5c-305">[Sondagem longa do AJAX](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling), com suporte de todos os navegadores.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-305">[Ajax long polling](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling), supported by all browsers.</span></span>

<span data-ttu-id="e2d5c-306">Para qualquer conexão fornecida, o Signalr escolhe o melhor método de transporte que o servidor e o cliente oferecem suporte.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-306">For any given connection, SignalR chooses the best transport method that both the server and the client support.</span></span>

1. <span data-ttu-id="e2d5c-307">Abra *StockTicker.js*.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-307">Open *StockTicker.js*.</span></span>

1. <span data-ttu-id="e2d5c-308">Adicione esta linha de código realçada para habilitar o registro em log imediatamente antes do código que inicializa a conexão no final do arquivo:</span><span class="sxs-lookup"><span data-stu-id="e2d5c-308">Add this highlighted line of code to enable logging immediately before the code that initializes the connection at the end of the file:</span></span>

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample19.js?highlight=2)]

1. <span data-ttu-id="e2d5c-309">Pressione **F5** para executar o projeto.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-309">Press **F5** to run the project.</span></span>

1. <span data-ttu-id="e2d5c-310">Abra a janela de ferramentas de desenvolvedor do navegador e selecione o console do para ver os logs.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-310">Open your browser's developer tools window, and select the Console to see the logs.</span></span> <span data-ttu-id="e2d5c-311">Talvez seja necessário atualizar a página para ver os logs do Signalr negociando o método de transporte para uma nova conexão.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-311">You might have to refresh the page to see the logs of SignalR negotiating the transport method for a new connection.</span></span>

    * <span data-ttu-id="e2d5c-312">Se você estiver executando o Internet Explorer 10 no Windows 8 (IIS 8), o método de transporte será **WebSockets**.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-312">If you're running Internet Explorer 10 on Windows 8 (IIS 8), the transport method is **WebSockets**.</span></span>

    * <span data-ttu-id="e2d5c-313">Se você estiver executando o Internet Explorer 10 no Windows 7 (IIS 7,5), o método de transporte será **iframe**.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-313">If you're running Internet Explorer 10 on Windows 7 (IIS 7.5), the transport method is **iframe**.</span></span>

    * <span data-ttu-id="e2d5c-314">Se você estiver executando o Firefox 19 no Windows 8 (IIS 8), o método de transporte será **WebSockets**.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-314">If you're running Firefox 19 on Windows 8 (IIS 8), the transport method is **WebSockets**.</span></span>

        > [!TIP]
        > <span data-ttu-id="e2d5c-315">No Firefox, instale o suplemento do Firebug para obter uma janela de console.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-315">In Firefox, install the Firebug add-in to get a Console window.</span></span>

    * <span data-ttu-id="e2d5c-316">Se você estiver executando o Firefox 19 no Windows 7 (IIS 7,5), o método de transporte será um evento **enviado pelo servidor** .</span><span class="sxs-lookup"><span data-stu-id="e2d5c-316">If you're running Firefox 19 on Windows 7 (IIS 7.5), the transport method is **server-sent** events.</span></span>

## <a name="install-the-stockticker-sample"></a><span data-ttu-id="e2d5c-317">Instalar o exemplo de StockTicker</span><span class="sxs-lookup"><span data-stu-id="e2d5c-317">Install the StockTicker sample</span></span>

<span data-ttu-id="e2d5c-318">O [Microsoft. AspNet. signaler. Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) instala o aplicativo StockTicker.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-318">The [Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) installs the StockTicker application.</span></span> <span data-ttu-id="e2d5c-319">O pacote NuGet inclui mais recursos do que a versão simplificada que você criou do zero.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-319">The NuGet package includes more features than the simplified version that you created from scratch.</span></span> <span data-ttu-id="e2d5c-320">Nesta seção do tutorial, você instala o pacote NuGet e examina os novos recursos e o código que os implementa.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-320">In this section of the tutorial, you install the NuGet package and review the new features and the code that implements them.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e2d5c-321">Se você instalar o pacote sem executar as etapas anteriores deste tutorial, você deve adicionar uma classe de inicialização OWIN ao seu projeto.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-321">If you install the package without performing the earlier steps of this tutorial, you must add an OWIN startup class to your project.</span></span> <span data-ttu-id="e2d5c-322">Este arquivo de readme.txt para o pacote NuGet explica essa etapa.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-322">This readme.txt file for the NuGet package explains this step.</span></span>

### <a name="install-the-signalrsample-nuget-package"></a><span data-ttu-id="e2d5c-323">Instalar o pacote NuGet. Sample do Signalr</span><span class="sxs-lookup"><span data-stu-id="e2d5c-323">Install the SignalR.Sample NuGet package</span></span>

1. <span data-ttu-id="e2d5c-324">No **Gerenciador de Soluções**, clique com o botão direito no projeto e escolha **Gerenciar Pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-324">In **Solution Explorer**, right-click the project and select **Manage NuGet Packages**.</span></span>

1. <span data-ttu-id="e2d5c-325">No **Gerenciador de pacotes NuGet: signalr. StockTicker**, selecione **procurar**.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-325">In **NuGet Package manager: SignalR.StockTicker**, select **Browse**.</span></span>

1. <span data-ttu-id="e2d5c-326">Em **origem do pacote**, selecione **NuGet.org**.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-326">From **Package source**, select **nuget.org**.</span></span>

1. <span data-ttu-id="e2d5c-327">Insira *signalr. Sample* na caixa de pesquisa e selecione a instalação **Microsoft. AspNet. signalr. Sample**  >  **Install**.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-327">Enter *SignalR.Sample* in the search box and select **Microsoft.AspNet.SignalR.Sample** > **Install**.</span></span>

1. <span data-ttu-id="e2d5c-328">Em **Gerenciador de soluções**, expanda a pasta *signalr. Sample* .</span><span class="sxs-lookup"><span data-stu-id="e2d5c-328">In **Solution Explorer**, expand the *SignalR.Sample* folder.</span></span>

    <span data-ttu-id="e2d5c-329">A instalação do pacote Signalr. Sample criou a pasta e seu conteúdo.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-329">Installing the SignalR.Sample package created the folder and its contents.</span></span>

1. <span data-ttu-id="e2d5c-330">Na pasta *signalr. Sample* , clique com o botão direito do mouse em *StockTicker.html*e selecione **definir como página inicial**.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-330">In the *SignalR.Sample* folder, right-click *StockTicker.html*, and then select **Set As Start Page**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e2d5c-331">A instalação do pacote NuGet do Signalr. Sample pode alterar a versão do jQuery que você tem em sua pasta *scripts* .</span><span class="sxs-lookup"><span data-stu-id="e2d5c-331">Installing The SignalR.Sample NuGet package might change the version of jQuery that you have in your *Scripts* folder.</span></span> <span data-ttu-id="e2d5c-332">O novo *StockTicker.htmarquivo l* que o pacote instala na pasta *signalr. Sample* estará em sincronia com a versão do jQuery instalada pelo pacote, mas se você quiser executar o arquivo original *StockTicker.html* novamente, talvez seja necessário atualizar a referência do jQuery na marca do script primeiro.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-332">The new *StockTicker.html* file that the package installs in the *SignalR.Sample* folder will be in sync with the jQuery version that the package installs, but if you want to run your original *StockTicker.html* file again, you might have to update the jQuery reference in the script tag first.</span></span>

### <a name="run-the-application"></a><span data-ttu-id="e2d5c-333">Executar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="e2d5c-333">Run the application</span></span>

 <span data-ttu-id="e2d5c-334">A tabela que você viu no primeiro aplicativo tinha recursos úteis.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-334">The table that you saw in the first app had useful features.</span></span> <span data-ttu-id="e2d5c-335">O aplicativo de cotação de ações completo mostra novos recursos: uma janela de rolagem horizontal que mostra os dados de ações e as ações que mudam de cor à medida que elas crescem e caem.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-335">The full stock ticker application shows new features: a horizontally scrolling window that shows the stock data and stocks that change color as they rise and fall.</span></span>

1. <span data-ttu-id="e2d5c-336">Pressione **F5** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-336">Press **F5** to run the app.</span></span>

     <span data-ttu-id="e2d5c-337">Quando você executa o aplicativo pela primeira vez, o "mercado" é "fechado" e você vê uma tabela estática e uma janela de marcação que não está rolando.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-337">When you run the app for the first time, the "market" is "closed" and you see a static table and a ticker window that isn't scrolling.</span></span>

1. <span data-ttu-id="e2d5c-338">Selecione **abrir mercado**.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-338">Select **Open Market**.</span></span>

    ![Captura de tela do marcador ao vivo.](tutorial-server-broadcast-with-signalr/_static/image5.png)

    * <span data-ttu-id="e2d5c-340">A caixa de Cotações de **estoque ao vivo** começa a rolar horizontalmente, e o servidor começa a transmitir periodicamente as alterações de preço de ações aleatoriamente.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-340">The **Live Stock Ticker** box starts to scroll horizontally, and the server starts to periodically broadcast stock price changes on a random basis.</span></span>

    * <span data-ttu-id="e2d5c-341">Cada vez que o preço de uma ação é alterado, o aplicativo atualiza a **tabela de estoque ao vivo** e a **cotação de estoque ao vivo**.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-341">Each time a stock price changes, the app updates both the **Live Stock Table** and the **Live Stock Ticker**.</span></span>

    * <span data-ttu-id="e2d5c-342">Quando a alteração de preço de um estoque é positiva, o aplicativo mostra o estoque com um plano de fundo verde.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-342">When a stock's price change is positive, the app shows the stock with a green background.</span></span>

    * <span data-ttu-id="e2d5c-343">Quando a alteração é negativa, o aplicativo mostra o estoque com um plano de fundo vermelho.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-343">When the change is negative, the app shows the stock with a red background.</span></span>

1. <span data-ttu-id="e2d5c-344">Selecione **fechar mercado**.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-344">Select **Close Market**.</span></span>

    * <span data-ttu-id="e2d5c-345">A tabela de atualizações é interrompida.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-345">The table updates stop.</span></span>

    * <span data-ttu-id="e2d5c-346">O marcador interrompe a rolagem.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-346">The ticker stops scrolling.</span></span>

1. <span data-ttu-id="e2d5c-347">Selecione **Restaurar**.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-347">Select **Reset**.</span></span>

    * <span data-ttu-id="e2d5c-348">Todos os dados de ações são redefinidos.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-348">All stock data is reset.</span></span>

    * <span data-ttu-id="e2d5c-349">O aplicativo restaura o estado inicial antes do início das alterações de preço.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-349">The app restores the initial state before price changes started.</span></span>

1. <span data-ttu-id="e2d5c-350">Copie a URL do navegador, abra dois outros navegadores e cole as URLs nas barras de endereço.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-350">Copy the URL from the browser, open two other browsers, and paste the URLs into the address bars.</span></span>

1. <span data-ttu-id="e2d5c-351">Você vê os mesmos dados atualizados dinamicamente ao mesmo tempo em cada navegador.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-351">You see the same data dynamically updated at the same time in each browser.</span></span>

1. <span data-ttu-id="e2d5c-352">Quando você seleciona qualquer um dos controles, todos os navegadores respondem da mesma maneira ao mesmo tempo.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-352">When you select any of the controls, all browsers respond the same way at the same time.</span></span>

### <a name="live-stock-ticker-display"></a><span data-ttu-id="e2d5c-353">Exibição de Cotações de estoque ao vivo</span><span class="sxs-lookup"><span data-stu-id="e2d5c-353">Live Stock Ticker display</span></span>

<span data-ttu-id="e2d5c-354">A exibição de **Cotações de estoque ao vivo** é uma lista não ordenada em um `<div>` elemento formatado em uma única linha por estilos de CSS.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-354">The **Live Stock Ticker** display is an unordered list in a `<div>` element formatted into a single line by CSS styles.</span></span> <span data-ttu-id="e2d5c-355">O aplicativo Inicializa e atualiza o marcador da mesma maneira que a tabela: substituindo espaços reservados em uma `<li>` cadeia de caracteres de modelo e adicionando dinamicamente os `<li>` elementos ao `<ul>` elemento.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-355">The app initializes and updates the ticker the same way as the table: by replacing placeholders in an `<li>` template string and dynamically adding the `<li>` elements to the `<ul>` element.</span></span> <span data-ttu-id="e2d5c-356">O aplicativo inclui a rolagem usando a `animate` função jQuery para variar a margem esquerda da lista não ordenada dentro do `<div>` .</span><span class="sxs-lookup"><span data-stu-id="e2d5c-356">The app includes  scrolling by using the jQuery `animate` function to vary the margin-left of the unordered list within the `<div>`.</span></span>

#### <a name="signalrsample-stocktickerhtml"></a><span data-ttu-id="e2d5c-357">Signalr. amostra StockTicker.html</span><span class="sxs-lookup"><span data-stu-id="e2d5c-357">SignalR.Sample StockTicker.html</span></span>

<span data-ttu-id="e2d5c-358">O código HTML da bolsa de ações:</span><span class="sxs-lookup"><span data-stu-id="e2d5c-358">The stock ticker HTML code:</span></span>

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample20.html)]

#### <a name="signalrsample-stocktickercss"></a><span data-ttu-id="e2d5c-359">Signalr. Sample StockTicker. css</span><span class="sxs-lookup"><span data-stu-id="e2d5c-359">SignalR.Sample StockTicker.css</span></span>

<span data-ttu-id="e2d5c-360">O código CSS da cotação de ações:</span><span class="sxs-lookup"><span data-stu-id="e2d5c-360">The stock ticker CSS code:</span></span>

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample21.html)]

#### <a name="signalrsample-signalrstocktickerjs"></a><span data-ttu-id="e2d5c-361">SignalR.StockTicker.js. Sample do signalr</span><span class="sxs-lookup"><span data-stu-id="e2d5c-361">SignalR.Sample SignalR.StockTicker.js</span></span>

<span data-ttu-id="e2d5c-362">O código do jQuery que faz a rolagem:</span><span class="sxs-lookup"><span data-stu-id="e2d5c-362">The jQuery code that makes it scroll:</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample22.js)]

### <a name="additional-methods-on-the-server-that-the-client-can-call"></a><span data-ttu-id="e2d5c-363">Métodos adicionais no servidor que o cliente pode chamar</span><span class="sxs-lookup"><span data-stu-id="e2d5c-363">Additional methods on the server that the client can call</span></span>

<span data-ttu-id="e2d5c-364">Para adicionar flexibilidade ao aplicativo, há métodos adicionais que o aplicativo pode chamar.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-364">To add flexibility to the app, there are additional methods the app can call.</span></span>

#### <a name="signalrsample-stocktickerhubcs"></a><span data-ttu-id="e2d5c-365">StockTickerHub.cs de amostra.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-365">SignalR.Sample StockTickerHub.cs</span></span>

<span data-ttu-id="e2d5c-366">A `StockTickerHub` classe define quatro métodos adicionais que o cliente pode chamar:</span><span class="sxs-lookup"><span data-stu-id="e2d5c-366">The `StockTickerHub` class defines four additional methods that the client can call:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample23.cs)]

<span data-ttu-id="e2d5c-367">O aplicativo chama `OpenMarket` , `CloseMarket` e `Reset` em resposta aos botões na parte superior da página.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-367">The app calls `OpenMarket`, `CloseMarket`, and `Reset` in response to the buttons at the top of the page.</span></span> <span data-ttu-id="e2d5c-368">Eles demonstram o padrão de um cliente que dispara uma alteração no estado que é propagada imediatamente para todos os clientes.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-368">They demonstrate the pattern of one client triggering a change in state immediately propagated to all clients.</span></span> <span data-ttu-id="e2d5c-369">Cada um desses métodos chama um método na `StockTicker` classe que causa a alteração do estado do mercado e, em seguida, transmite o novo estado.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-369">Each of these methods calls a method in the `StockTicker` class that causes the market state change and then broadcasts the new state.</span></span>

#### <a name="signalrsample-stocktickercs"></a><span data-ttu-id="e2d5c-370">StockTicker.cs de amostra.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-370">SignalR.Sample StockTicker.cs</span></span>

<span data-ttu-id="e2d5c-371">Na `StockTicker` classe, o aplicativo mantém o estado do mercado com uma `MarketState` propriedade que retorna um valor de `MarketState` enumeração:</span><span class="sxs-lookup"><span data-stu-id="e2d5c-371">In the `StockTicker` class, the app maintains the state of the market with a `MarketState` property that returns a `MarketState` enum value:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample24.cs)]

<span data-ttu-id="e2d5c-372">Cada um dos métodos que alteram o estado do mercado faz isso dentro de um bloco de bloqueio porque a `StockTicker` classe deve ser thread-safe:</span><span class="sxs-lookup"><span data-stu-id="e2d5c-372">Each of the methods that change the market state do so inside a lock block because the `StockTicker` class has to be thread-safe:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample25.cs)]

<span data-ttu-id="e2d5c-373">Para certificar-se de que esse código é thread-safe, o `_marketState` campo que faz o backup da `MarketState` Propriedade designada `volatile` :</span><span class="sxs-lookup"><span data-stu-id="e2d5c-373">To make sure this code is thread-safe, the `_marketState` field that backs the `MarketState` property designated `volatile`:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample26.cs)]

<span data-ttu-id="e2d5c-374">Os `BroadcastMarketStateChange` `BroadcastMarketReset` métodos e são semelhantes ao método BroadcastStockPrice que você já viu, exceto que eles chamam métodos diferentes definidos no cliente:</span><span class="sxs-lookup"><span data-stu-id="e2d5c-374">The `BroadcastMarketStateChange` and `BroadcastMarketReset` methods are similar to the BroadcastStockPrice method that you already saw, except they call different methods defined at the client:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample27.cs)]

### <a name="additional-functions-on-the-client-that-the-server-can-call"></a><span data-ttu-id="e2d5c-375">Funções adicionais no cliente que o servidor pode chamar</span><span class="sxs-lookup"><span data-stu-id="e2d5c-375">Additional functions on the client that the server can call</span></span>

<span data-ttu-id="e2d5c-376">A `updateStockPrice` função agora lida com a exibição da tabela e da marca e ela usa `jQuery.Color` para piscar as cores vermelhas e verdes.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-376">The `updateStockPrice` function now handles both the table and the ticker display, and it uses `jQuery.Color` to flash red and green colors.</span></span>

<span data-ttu-id="e2d5c-377">Novas funções no *SignalR.StockTicker.js* habilitam e desabilitam os botões com base no estado do mercado.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-377">New functions in *SignalR.StockTicker.js* enable and disable the buttons based on market state.</span></span> <span data-ttu-id="e2d5c-378">Eles também param ou iniciam a rolagem horizontal do **marcador de estoque ao vivo** .</span><span class="sxs-lookup"><span data-stu-id="e2d5c-378">They also stop or start the **Live Stock Ticker** horizontal scrolling.</span></span> <span data-ttu-id="e2d5c-379">Como muitas funções estão sendo adicionadas ao `ticker.client` , o aplicativo usa a [função de extensão jQuery](http://api.jquery.com/jQuery.extend/) para adicioná-las.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-379">Since many functions are being added to `ticker.client`, the app uses the [jQuery extend function](http://api.jquery.com/jQuery.extend/) to add them.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample28.js)]

### <a name="additional-client-setup-after-establishing-the-connection"></a><span data-ttu-id="e2d5c-380">Configuração adicional do cliente depois de estabelecer a conexão</span><span class="sxs-lookup"><span data-stu-id="e2d5c-380">Additional client setup after establishing the connection</span></span>

<span data-ttu-id="e2d5c-381">Depois que o cliente estabelece a conexão, ele tem algum trabalho adicional a fazer:</span><span class="sxs-lookup"><span data-stu-id="e2d5c-381">After the client establishes the connection, it has some additional work to do:</span></span>

* <span data-ttu-id="e2d5c-382">Descubra se o mercado está aberto ou fechado para chamar a função apropriada `marketOpened` `marketClosed` .</span><span class="sxs-lookup"><span data-stu-id="e2d5c-382">Find out if the market is open or closed to call the appropriate `marketOpened` or `marketClosed` function.</span></span>

* <span data-ttu-id="e2d5c-383">Anexe as chamadas de método de servidor aos botões.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-383">Attach the server method calls to the buttons.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample29.js)]

<span data-ttu-id="e2d5c-384">Os métodos de servidor não são conectados aos botões até que o aplicativo estabeleça a conexão.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-384">The server methods aren't wired up to the buttons until after the app establishes the connection.</span></span> <span data-ttu-id="e2d5c-385">É assim que o código não pode chamar os métodos de servidor antes que eles estejam disponíveis.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-385">It's so the code can't call the server methods before they're available.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e2d5c-386">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="e2d5c-386">Additional resources</span></span>

<span data-ttu-id="e2d5c-387">Neste tutorial, você aprendeu a programar um aplicativo Signalr que transmite mensagens do servidor para todos os clientes conectados.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-387">In this tutorial you've learned how to program a SignalR application that broadcasts messages from the server to all connected clients.</span></span> <span data-ttu-id="e2d5c-388">Agora você pode transmitir mensagens periodicamente e em resposta a notificações de qualquer cliente.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-388">Now you can broadcast messages on a periodic basis and in response to notifications from any client.</span></span> <span data-ttu-id="e2d5c-389">Você pode usar o conceito de instância singleton multi-threaded para manter o estado do servidor em cenários de jogos online de vários jogadores.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-389">You can use the concept of multi-threaded singleton instance to maintain server state in multi-player online game scenarios.</span></span> <span data-ttu-id="e2d5c-390">Para obter um exemplo, consulte o jogo emissor [com base no signalr](https://github.com/NTaylorMullen/ShootR).</span><span class="sxs-lookup"><span data-stu-id="e2d5c-390">For an example, see [the ShootR game based on SignalR](https://github.com/NTaylorMullen/ShootR).</span></span>

<span data-ttu-id="e2d5c-391">Para obter tutoriais que mostrem cenários de comunicação ponto a ponto, confira [introdução com sinalizador](introduction-to-signalr.md) e [atualização em tempo real com o signalr](tutorial-high-frequency-realtime-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="e2d5c-391">For tutorials that show peer-to-peer communication scenarios, see [Getting Started with SignalR](introduction-to-signalr.md) and [Real-Time Updating with SignalR](tutorial-high-frequency-realtime-with-signalr.md).</span></span>

<span data-ttu-id="e2d5c-392">Para obter mais informações sobre o Signalr, consulte os seguintes recursos:</span><span class="sxs-lookup"><span data-stu-id="e2d5c-392">For more about SignalR, see the following resources:</span></span>

* [<span data-ttu-id="e2d5c-393">ASP.NET SignalR</span><span class="sxs-lookup"><span data-stu-id="e2d5c-393">ASP.NET SignalR</span></span>](../../index.md)
* [<span data-ttu-id="e2d5c-394">Projeto do signalr</span><span class="sxs-lookup"><span data-stu-id="e2d5c-394">SignalR Project</span></span>](http://signalr.net/)
* [<span data-ttu-id="e2d5c-395">GitHub e exemplos do signalr</span><span class="sxs-lookup"><span data-stu-id="e2d5c-395">SignalR GitHub and Samples</span></span>](https://github.com/SignalR/SignalR)
* [<span data-ttu-id="e2d5c-396">Wiki do signalr</span><span class="sxs-lookup"><span data-stu-id="e2d5c-396">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a><span data-ttu-id="e2d5c-397">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="e2d5c-397">Next steps</span></span>

<span data-ttu-id="e2d5c-398">Neste tutorial, você:</span><span class="sxs-lookup"><span data-stu-id="e2d5c-398">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e2d5c-399">Criou o projeto</span><span class="sxs-lookup"><span data-stu-id="e2d5c-399">Created the project</span></span>
> * <span data-ttu-id="e2d5c-400">Configurar o código de servidor</span><span class="sxs-lookup"><span data-stu-id="e2d5c-400">Set up the server code</span></span>
> * <span data-ttu-id="e2d5c-401">Examinou o código do servidor</span><span class="sxs-lookup"><span data-stu-id="e2d5c-401">Examined the server code</span></span>
> * <span data-ttu-id="e2d5c-402">Configurar o código do cliente</span><span class="sxs-lookup"><span data-stu-id="e2d5c-402">Set up the client code</span></span>
> * <span data-ttu-id="e2d5c-403">Examinou o código do cliente</span><span class="sxs-lookup"><span data-stu-id="e2d5c-403">Examined the client code</span></span>
> * <span data-ttu-id="e2d5c-404">Testado o aplicativo</span><span class="sxs-lookup"><span data-stu-id="e2d5c-404">Tested the application</span></span>
> * <span data-ttu-id="e2d5c-405">Registro em log habilitado</span><span class="sxs-lookup"><span data-stu-id="e2d5c-405">Enabled logging</span></span>

<span data-ttu-id="e2d5c-406">Avance para o próximo artigo para aprender a criar um aplicativo Web em tempo real que usa o Signaler 2 do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e2d5c-406">Advance to the next article to learn how to create a real-time web application that uses ASP.NET SignalR 2.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="e2d5c-407">Criar aplicativo Web em tempo real com Signalr</span><span class="sxs-lookup"><span data-stu-id="e2d5c-407">Create real-time web app with SignalR</span></span>](real-time-web-applications-with-signalr.md)
