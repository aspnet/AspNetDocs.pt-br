---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-net-client
title: Guia da API de hubs do Signalr ASP.NET – cliente .NET (C#) | Microsoft Docs
author: bradygaster
description: Este documento fornece uma introdução ao uso da API de hubs para o Signalr versão 2 em clientes .NET, como Windows Store (WinRT), WPF, Silverlight e contras...
ms.author: bradyg
ms.date: 01/15/2019
ms.assetid: 6d02d9f7-94e5-4140-9f51-5a6040f274f6
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-net-client
msc.type: authoredcontent
ms.openlocfilehash: d3536f1c15cd7dad7cd660becf0577e5c131f707
ms.sourcegitcommit: c62ec20b453cee3249eb894ecd75013b57d078f0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/21/2020
ms.locfileid: "92345549"
---
# <a name="aspnet-signalr-hubs-api-guide---net-client-c"></a><span data-ttu-id="5144d-103">Guia da API de hubs do Signalr ASP.NET – cliente .NET (C#)</span><span class="sxs-lookup"><span data-stu-id="5144d-103">ASP.NET SignalR Hubs API Guide - .NET Client (C#)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="5144d-104">Este documento fornece uma introdução ao uso da API de hubs para o Signalr versão 2 em clientes .NET, como Windows Store (WinRT), WPF, Silverlight e aplicativos de console.</span><span class="sxs-lookup"><span data-stu-id="5144d-104">This document provides an introduction to using the Hubs API for SignalR version 2 in .NET clients, such as Windows Store (WinRT), WPF, Silverlight, and console applications.</span></span>
>
> <span data-ttu-id="5144d-105">A API de hubs de Signalr permite que você faça RPCs (chamadas de procedimento remoto) de um servidor para clientes conectados e de clientes para o servidor.</span><span class="sxs-lookup"><span data-stu-id="5144d-105">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="5144d-106">No código do servidor, você define métodos que podem ser chamados por clientes e chama métodos que são executados no cliente.</span><span class="sxs-lookup"><span data-stu-id="5144d-106">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="5144d-107">No código do cliente, você define métodos que podem ser chamados do servidor e chama métodos que são executados no servidor.</span><span class="sxs-lookup"><span data-stu-id="5144d-107">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="5144d-108">O signalr cuida de todo o direcionamento de cliente para servidor para você.</span><span class="sxs-lookup"><span data-stu-id="5144d-108">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
>
> <span data-ttu-id="5144d-109">O signalr também oferece uma API de nível inferior chamada conexões persistentes.</span><span class="sxs-lookup"><span data-stu-id="5144d-109">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="5144d-110">Para obter uma introdução ao Signalr, hubs e conexões persistentes, ou para um tutorial que mostra como criar um aplicativo Signalr completo, consulte [signalr-introdução](../getting-started/index.md).</span><span class="sxs-lookup"><span data-stu-id="5144d-110">For an introduction to SignalR, Hubs, and Persistent Connections, or for a tutorial that shows how to build a complete SignalR application, see [SignalR - Getting Started](../getting-started/index.md).</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="5144d-111">Versões de software usadas neste tópico</span><span class="sxs-lookup"><span data-stu-id="5144d-111">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="5144d-112">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="5144d-112">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/)
> - <span data-ttu-id="5144d-113">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="5144d-113">.NET 4.5</span></span>
> - <span data-ttu-id="5144d-114">Sinalização versão 2</span><span class="sxs-lookup"><span data-stu-id="5144d-114">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="5144d-115">Versões anteriores deste tópico</span><span class="sxs-lookup"><span data-stu-id="5144d-115">Previous versions of this topic</span></span>
>
> <span data-ttu-id="5144d-116">Para obter informações sobre versões anteriores do Signalr, confira [versões mais antigas do signalr](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="5144d-116">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="5144d-117">Perguntas e comentários</span><span class="sxs-lookup"><span data-stu-id="5144d-117">Questions and comments</span></span>
>
> <span data-ttu-id="5144d-118">Deixe comentários sobre como você gostou deste tutorial e o que poderíamos melhorar nos comentários na parte inferior da página.</span><span class="sxs-lookup"><span data-stu-id="5144d-118">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="5144d-119">Se você tiver dúvidas que não estão diretamente relacionadas ao tutorial, poderá lançá-las no fórum do [signalr ASP.net](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [stackoverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="5144d-119">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

## <a name="overview"></a><span data-ttu-id="5144d-120">Visão geral</span><span class="sxs-lookup"><span data-stu-id="5144d-120">Overview</span></span>

<span data-ttu-id="5144d-121">Este documento contém as seguintes seções:</span><span class="sxs-lookup"><span data-stu-id="5144d-121">This document contains the following sections:</span></span>

- [<span data-ttu-id="5144d-122">Configuração do cliente</span><span class="sxs-lookup"><span data-stu-id="5144d-122">Client Setup</span></span>](#clientsetup)
- [<span data-ttu-id="5144d-123">Como estabelecer uma conexão</span><span class="sxs-lookup"><span data-stu-id="5144d-123">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="5144d-124">Conexões entre domínios de clientes do Silverlight</span><span class="sxs-lookup"><span data-stu-id="5144d-124">Cross-domain connections from Silverlight clients</span></span>](#slcrossdomain)
- [<span data-ttu-id="5144d-125">Como configurar a conexão</span><span class="sxs-lookup"><span data-stu-id="5144d-125">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="5144d-126">Como definir o número máximo de conexões simultâneas em clientes do WPF</span><span class="sxs-lookup"><span data-stu-id="5144d-126">How to set the maximum number of concurrent connections in WPF clients</span></span>](#maxconnections)
    - [<span data-ttu-id="5144d-127">Como especificar parâmetros de cadeia de caracteres de consulta</span><span class="sxs-lookup"><span data-stu-id="5144d-127">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="5144d-128">Como especificar o método de transporte</span><span class="sxs-lookup"><span data-stu-id="5144d-128">How to specify the transport method</span></span>](#transport)
    - [<span data-ttu-id="5144d-129">Como especificar cabeçalhos HTTP</span><span class="sxs-lookup"><span data-stu-id="5144d-129">How to specify HTTP headers</span></span>](#httpheaders)
    - [<span data-ttu-id="5144d-130">Como especificar certificados de cliente</span><span class="sxs-lookup"><span data-stu-id="5144d-130">How to specify client certificates</span></span>](#clientcertificate)
- [<span data-ttu-id="5144d-131">Como criar o proxy de Hub</span><span class="sxs-lookup"><span data-stu-id="5144d-131">How to create the Hub proxy</span></span>](#proxy)
- [<span data-ttu-id="5144d-132">Como definir métodos no cliente que o servidor pode chamar</span><span class="sxs-lookup"><span data-stu-id="5144d-132">How to define methods on the client that the server can call</span></span>](#callclient)

    - [<span data-ttu-id="5144d-133">Métodos sem parâmetros</span><span class="sxs-lookup"><span data-stu-id="5144d-133">Methods without parameters</span></span>](#clientmethodswithoutparms)
    - [<span data-ttu-id="5144d-134">Métodos com parâmetros, especificando tipos de parâmetro</span><span class="sxs-lookup"><span data-stu-id="5144d-134">Methods with parameters, specifying parameter types</span></span>](#clientmethodswithparmtypes)
    - [<span data-ttu-id="5144d-135">Métodos com parâmetros, especificando objetos dinâmicos para os parâmetros</span><span class="sxs-lookup"><span data-stu-id="5144d-135">Methods with parameters, specifying dynamic objects for the parameters</span></span>](#clientmethodswithdynamparms)
    - [<span data-ttu-id="5144d-136">Como remover um manipulador</span><span class="sxs-lookup"><span data-stu-id="5144d-136">How to remove a handler</span></span>](#removehandler)
- [<span data-ttu-id="5144d-137">Como chamar métodos de servidor do cliente</span><span class="sxs-lookup"><span data-stu-id="5144d-137">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="5144d-138">Como tratar eventos de tempo de vida da conexão</span><span class="sxs-lookup"><span data-stu-id="5144d-138">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="5144d-139">Como tratar erros</span><span class="sxs-lookup"><span data-stu-id="5144d-139">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="5144d-140">Como habilitar o log do lado do cliente</span><span class="sxs-lookup"><span data-stu-id="5144d-140">How to enable client-side logging</span></span>](#logging)
- [<span data-ttu-id="5144d-141">Exemplos de código de aplicativo do WPF, Silverlight e console para métodos de cliente que o servidor pode chamar</span><span class="sxs-lookup"><span data-stu-id="5144d-141">WPF, Silverlight, and console application code samples for client methods that the server can call</span></span>](#wpfsl)

<span data-ttu-id="5144d-142">Para obter um exemplo de projetos de cliente .NET, consulte os seguintes recursos:</span><span class="sxs-lookup"><span data-stu-id="5144d-142">For a sample .NET client projects, see the following resources:</span></span>

- <span data-ttu-id="5144d-143">[Gustavo-Armenta/signalr-exemplos](https://github.com/gustavo-armenta/SignalR-Samples) em github.com (WinRT, Silverlight, exemplos de aplicativos de console).</span><span class="sxs-lookup"><span data-stu-id="5144d-143">[gustavo-armenta / SignalR-Samples](https://github.com/gustavo-armenta/SignalR-Samples) on GitHub.com (WinRT, Silverlight, console app examples).</span></span>
- <span data-ttu-id="5144d-144">[DamianEdwards/SignalR-MoveShapeDemo/MoveShape. desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) em github.com (exemplo de WPF).</span><span class="sxs-lookup"><span data-stu-id="5144d-144">[DamianEdwards / SignalR-MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) on GitHub.com (WPF example).</span></span>
- <span data-ttu-id="5144d-145">[Signalr/Microsoft. AspNet. signalr. Client. Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) em github.com (exemplo de aplicativo de console).</span><span class="sxs-lookup"><span data-stu-id="5144d-145">[SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) on GitHub.com (Console app example).</span></span>

<span data-ttu-id="5144d-146">Para obter a documentação sobre como programar os clientes do servidor ou JavaScript, consulte os seguintes recursos:</span><span class="sxs-lookup"><span data-stu-id="5144d-146">For documentation on how to program the server or JavaScript clients, see the following resources:</span></span>

- [<span data-ttu-id="5144d-147">Guia da API de hubs de signalr-servidor</span><span class="sxs-lookup"><span data-stu-id="5144d-147">SignalR Hubs API Guide - Server</span></span>](hubs-api-guide-server.md)
- [<span data-ttu-id="5144d-148">Guia de API de hubs de signalr-cliente JavaScript</span><span class="sxs-lookup"><span data-stu-id="5144d-148">SignalR Hubs API Guide - JavaScript Client</span></span>](hubs-api-guide-javascript-client.md)

<span data-ttu-id="5144d-149">Links para os tópicos de referência de API são para a versão 4,5 do .NET da API.</span><span class="sxs-lookup"><span data-stu-id="5144d-149">Links to API Reference topics are to the .NET 4.5 version of the API.</span></span> <span data-ttu-id="5144d-150">Se você estiver usando o .NET 4, consulte [a versão .NET 4 dos tópicos da API](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="5144d-150">If you're using .NET 4, see [the .NET 4 version of the API topics](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="5144d-151">Configuração do cliente</span><span class="sxs-lookup"><span data-stu-id="5144d-151">Client setup</span></span>

<span data-ttu-id="5144d-152">Instale o pacote NuGet [Microsoft. AspNet. signaler. Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) (não o pacote [Microsoft. AspNet. signalr](http://nuget.org/packages/microsoft.aspnet.signalr) ).</span><span class="sxs-lookup"><span data-stu-id="5144d-152">Install the [Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) NuGet package (not the [Microsoft.AspNet.SignalR](http://nuget.org/packages/microsoft.aspnet.signalr) package).</span></span> <span data-ttu-id="5144d-153">Este pacote dá suporte a clientes do WinRT, Silverlight, WPF, aplicativos de console e Windows Phone, tanto para o .NET 4 quanto para o .NET 4,5.</span><span class="sxs-lookup"><span data-stu-id="5144d-153">This package supports WinRT, Silverlight, WPF, console application, and Windows Phone clients, for both .NET 4 and .NET 4.5.</span></span>

<span data-ttu-id="5144d-154">Se a versão do Signalr que você tem no cliente for diferente da versão que você tem no servidor, o Signalr geralmente é capaz de se adaptar à diferença.</span><span class="sxs-lookup"><span data-stu-id="5144d-154">If the version of SignalR that you have on the client is different from the version that you have on the server, SignalR is often able to adapt to the difference.</span></span> <span data-ttu-id="5144d-155">Por exemplo, um servidor executando o Signalr versão 2 dará suporte a clientes que têm o 1.1. x instalado, bem como clientes que têm a versão 2 instalada.</span><span class="sxs-lookup"><span data-stu-id="5144d-155">For example, a server running SignalR version 2 will support clients that have 1.1.x installed as well as clients that have version 2 installed.</span></span> <span data-ttu-id="5144d-156">Se a diferença entre a versão no servidor e a versão no cliente for muito grande, ou se o cliente for mais recente do que o servidor, o Signalr lançará uma `InvalidOperationException` exceção quando o cliente tentar estabelecer uma conexão.</span><span class="sxs-lookup"><span data-stu-id="5144d-156">If the difference between the version on the server and the version on the client is too great, or if the client is newer than the server, SignalR throws an `InvalidOperationException` exception when the client tries to establish a connection.</span></span> <span data-ttu-id="5144d-157">A mensagem de erro é " `You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X` ".</span><span class="sxs-lookup"><span data-stu-id="5144d-157">The error message is "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`".</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="5144d-158">Como estabelecer uma conexão</span><span class="sxs-lookup"><span data-stu-id="5144d-158">How to establish a connection</span></span>

<span data-ttu-id="5144d-159">Antes de poder estabelecer uma conexão, você precisa criar um `HubConnection` objeto e criar um proxy.</span><span class="sxs-lookup"><span data-stu-id="5144d-159">Before you can establish a connection, you have to create a `HubConnection` object and create a proxy.</span></span> <span data-ttu-id="5144d-160">Para estabelecer a conexão, chame o `Start` método no `HubConnection` objeto.</span><span class="sxs-lookup"><span data-stu-id="5144d-160">To establish the connection, call the `Start` method on the `HubConnection` object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample1.cs?highlight=1,5)]

> [!NOTE]
> <span data-ttu-id="5144d-161">Para clientes JavaScript, você precisa registrar pelo menos um manipulador de eventos antes de chamar o `Start` método para estabelecer a conexão.</span><span class="sxs-lookup"><span data-stu-id="5144d-161">For JavaScript clients you have to register at least one event handler before calling the `Start` method to establish the connection.</span></span> <span data-ttu-id="5144d-162">Isso não é necessário para clientes .NET.</span><span class="sxs-lookup"><span data-stu-id="5144d-162">This is not necessary for .NET clients.</span></span> <span data-ttu-id="5144d-163">Para clientes JavaScript, o código de proxy gerado cria automaticamente proxies para todos os hubs existentes no servidor, e o registro de um manipulador é como você indica quais hubs seu cliente pretende usar.</span><span class="sxs-lookup"><span data-stu-id="5144d-163">For JavaScript clients, the generated proxy code automatically creates proxies for all Hubs that exist on the server, and registering a handler is how you indicate which Hubs your client intends to use.</span></span> <span data-ttu-id="5144d-164">Mas, para um cliente .NET, você cria proxies de Hub manualmente, portanto, o Signalr pressupõe que você usará qualquer Hub para o qual criar um proxy.</span><span class="sxs-lookup"><span data-stu-id="5144d-164">But for a .NET client you create Hub proxies manually, so SignalR assumes that you will be using any Hub that you create a proxy for.</span></span>

<span data-ttu-id="5144d-165">O código de exemplo usa a URL "/signalr" padrão para se conectar ao seu serviço Signalr.</span><span class="sxs-lookup"><span data-stu-id="5144d-165">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="5144d-166">Para obter informações sobre como especificar uma URL base diferente, consulte [guia da API de hubs do ASP.net signalr-Server-a URL do/signalr](hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="5144d-166">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](hubs-api-guide-server.md#signalrurl).</span></span>

<span data-ttu-id="5144d-167">O `Start` método é executado de forma assíncrona.</span><span class="sxs-lookup"><span data-stu-id="5144d-167">The `Start` method executes asynchronously.</span></span> <span data-ttu-id="5144d-168">Para garantir que as linhas de código subsequentes não sejam executadas até que a conexão seja estabelecida, use `await` em um método assíncrono ASP.NET 4,5 ou `.Wait()` em um método síncrono.</span><span class="sxs-lookup"><span data-stu-id="5144d-168">To make sure that subsequent lines of code don't execute until after the connection is established, use `await` in an ASP.NET 4.5 asynchronous method or `.Wait()` in a synchronous method.</span></span> <span data-ttu-id="5144d-169">Não use `.Wait()` em um cliente WinRT.</span><span class="sxs-lookup"><span data-stu-id="5144d-169">Don't use `.Wait()` in a WinRT client.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample2.cs?highlight=1)]

[!code-css[Main](hubs-api-guide-net-client/samples/sample3.css?highlight=1)]

<a id="slcrossdomain"></a>

### <a name="cross-domain-connections-from-silverlight-clients"></a><span data-ttu-id="5144d-170">Conexões entre domínios de clientes do Silverlight</span><span class="sxs-lookup"><span data-stu-id="5144d-170">Cross-domain connections from Silverlight clients</span></span>

<span data-ttu-id="5144d-171">Para obter informações sobre como habilitar conexões entre domínios de clientes do Silverlight, consulte [disponibilizando um serviço entre limites de domínio](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx).</span><span class="sxs-lookup"><span data-stu-id="5144d-171">For information about how to enable cross-domain connections from Silverlight clients, see [Making a Service Available Across Domain Boundaries](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx).</span></span>

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="5144d-172">Como configurar a conexão</span><span class="sxs-lookup"><span data-stu-id="5144d-172">How to configure the connection</span></span>

<span data-ttu-id="5144d-173">Antes de estabelecer uma conexão, você pode especificar qualquer uma das seguintes opções:</span><span class="sxs-lookup"><span data-stu-id="5144d-173">Before you establish a connection, you can specify any of the following options:</span></span>

- <span data-ttu-id="5144d-174">Limite de conexões simultâneas.</span><span class="sxs-lookup"><span data-stu-id="5144d-174">Concurrent connections limit.</span></span>
- <span data-ttu-id="5144d-175">Parâmetros de cadeia de caracteres de consulta.</span><span class="sxs-lookup"><span data-stu-id="5144d-175">Query string parameters.</span></span>
- <span data-ttu-id="5144d-176">O método de transporte.</span><span class="sxs-lookup"><span data-stu-id="5144d-176">The transport method.</span></span>
- <span data-ttu-id="5144d-177">Cabeçalhos HTTP.</span><span class="sxs-lookup"><span data-stu-id="5144d-177">HTTP headers.</span></span>
- <span data-ttu-id="5144d-178">Certificados de cliente.</span><span class="sxs-lookup"><span data-stu-id="5144d-178">Client certificates.</span></span>

<a id="maxconnections"></a>

### <a name="how-to-set-the-maximum-number-of-concurrent-connections-in-wpf-clients"></a><span data-ttu-id="5144d-179">Como definir o número máximo de conexões simultâneas em clientes do WPF</span><span class="sxs-lookup"><span data-stu-id="5144d-179">How to set the maximum number of concurrent connections in WPF clients</span></span>

<span data-ttu-id="5144d-180">Em clientes do WPF, talvez seja necessário aumentar o número máximo de conexões simultâneas de seu valor padrão de 2.</span><span class="sxs-lookup"><span data-stu-id="5144d-180">In WPF clients, you might have to increase the maximum number of concurrent connections from its default value of 2.</span></span> <span data-ttu-id="5144d-181">O valor recomendado é 10.</span><span class="sxs-lookup"><span data-stu-id="5144d-181">The recommended value is 10.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample4.cs?highlight=5)]

<span data-ttu-id="5144d-182">Para obter mais informações, consulte [ServicePointManager. DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).</span><span class="sxs-lookup"><span data-stu-id="5144d-182">For more information, see [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="5144d-183">Como especificar parâmetros de cadeia de caracteres de consulta</span><span class="sxs-lookup"><span data-stu-id="5144d-183">How to specify query string parameters</span></span>

<span data-ttu-id="5144d-184">Se você quiser enviar dados ao servidor quando o cliente se conectar, poderá adicionar parâmetros de cadeia de caracteres de consulta ao objeto de conexão.</span><span class="sxs-lookup"><span data-stu-id="5144d-184">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="5144d-185">O exemplo a seguir mostra como definir um parâmetro de cadeia de caracteres de consulta no código do cliente.</span><span class="sxs-lookup"><span data-stu-id="5144d-185">The following example shows how to set a query string parameter in client code.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample5.cs)]

<span data-ttu-id="5144d-186">O exemplo a seguir mostra como ler um parâmetro de cadeia de caracteres de consulta no código do servidor.</span><span class="sxs-lookup"><span data-stu-id="5144d-186">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample6.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="5144d-187">Como especificar o método de transporte</span><span class="sxs-lookup"><span data-stu-id="5144d-187">How to specify the transport method</span></span>

<span data-ttu-id="5144d-188">Como parte do processo de conexão, um cliente do Signalr normalmente negocia com o servidor para determinar o melhor transporte que é suportado pelo servidor e pelo cliente.</span><span class="sxs-lookup"><span data-stu-id="5144d-188">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="5144d-189">Se você já souber qual transporte deseja usar, poderá ignorar esse processo de negociação.</span><span class="sxs-lookup"><span data-stu-id="5144d-189">If you already know which transport you want to use, you can bypass this negotiation process.</span></span> <span data-ttu-id="5144d-190">Para especificar o método de transporte, passe um objeto de transporte para o método Start.</span><span class="sxs-lookup"><span data-stu-id="5144d-190">To specify the transport method, pass in a transport object to the Start method.</span></span> <span data-ttu-id="5144d-191">O exemplo a seguir mostra como especificar o método de transporte no código do cliente.</span><span class="sxs-lookup"><span data-stu-id="5144d-191">The following example shows how to specify the transport method in client code.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample7.cs?highlight=5)]

<span data-ttu-id="5144d-192">O namespace [Microsoft. AspNet. signaler. Client. Exports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) inclui as seguintes classes que você pode usar para especificar o transporte.</span><span class="sxs-lookup"><span data-stu-id="5144d-192">The [Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) namespace includes the following classes that you can use to specify the transport.</span></span>

- <span data-ttu-id="5144d-193">[LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)</span><span class="sxs-lookup"><span data-stu-id="5144d-193">[LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)</span></span>
- <span data-ttu-id="5144d-194">[ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)</span><span class="sxs-lookup"><span data-stu-id="5144d-194">[ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)</span></span>
- <span data-ttu-id="5144d-195">[WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (disponível somente quando o servidor e o cliente usam o .NET 4,5.)</span><span class="sxs-lookup"><span data-stu-id="5144d-195">[WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (Available only when both server and client use .NET 4.5.)</span></span>
- <span data-ttu-id="5144d-196">O [transporte](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) automático (escolhe automaticamente o melhor transporte que é suportado pelo cliente e pelo servidor.</span><span class="sxs-lookup"><span data-stu-id="5144d-196">[AutoTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (Automatically chooses the best transport that is supported by both the client and the server.</span></span> <span data-ttu-id="5144d-197">Esse é o transporte padrão.</span><span class="sxs-lookup"><span data-stu-id="5144d-197">This is the default transport.</span></span> <span data-ttu-id="5144d-198">Passar isso para o `Start` método tem o mesmo efeito que não está passando em nada.)</span><span class="sxs-lookup"><span data-stu-id="5144d-198">Passing this in to the `Start` method has the same effect as not passing in anything.)</span></span>

<span data-ttu-id="5144d-199">O transporte ForeverFrame não está incluído nesta lista porque ele é usado somente por navegadores.</span><span class="sxs-lookup"><span data-stu-id="5144d-199">The ForeverFrame transport is not included in this list because it is used only by browsers.</span></span>

<span data-ttu-id="5144d-200">Para obter informações sobre como verificar o método de transporte no código do servidor, consulte [guia da API de hubs do signaler ASP.NET-Server-como obter informações sobre o cliente a partir da propriedade Context](hubs-api-guide-server.md#contextproperty).</span><span class="sxs-lookup"><span data-stu-id="5144d-200">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="5144d-201">Para obter mais informações sobre transportes e fallbacks, consulte [introdução ao signalr-transportes e fallbacks](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="5144d-201">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="httpheaders"></a>

### <a name="how-to-specify-http-headers"></a><span data-ttu-id="5144d-202">Como especificar cabeçalhos HTTP</span><span class="sxs-lookup"><span data-stu-id="5144d-202">How to specify HTTP headers</span></span>

<span data-ttu-id="5144d-203">Para definir cabeçalhos HTTP, use a `Headers` propriedade no objeto de conexão.</span><span class="sxs-lookup"><span data-stu-id="5144d-203">To set HTTP headers, use the `Headers` property on the connection object.</span></span> <span data-ttu-id="5144d-204">O exemplo a seguir mostra como adicionar um cabeçalho HTTP.</span><span class="sxs-lookup"><span data-stu-id="5144d-204">The following example shows how to add an HTTP header.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample8.cs?highlight=2)]

<a id="clientcertificate"></a>

### <a name="how-to-specify-client-certificates"></a><span data-ttu-id="5144d-205">Como especificar certificados de cliente</span><span class="sxs-lookup"><span data-stu-id="5144d-205">How to specify client certificates</span></span>

<span data-ttu-id="5144d-206">Para adicionar certificados de cliente, use o `AddClientCertificate` método no objeto de conexão.</span><span class="sxs-lookup"><span data-stu-id="5144d-206">To add client certificates, use the `AddClientCertificate` method on the connection object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample9.cs?highlight=2)]

<a id="proxy"></a>

## <a name="how-to-create-the-hub-proxy"></a><span data-ttu-id="5144d-207">Como criar o proxy de Hub</span><span class="sxs-lookup"><span data-stu-id="5144d-207">How to create the Hub proxy</span></span>

<span data-ttu-id="5144d-208">Para definir métodos no cliente que um hub pode chamar do servidor e para invocar métodos em um Hub no servidor, crie um proxy para o Hub chamando `CreateHubProxy` o objeto de conexão.</span><span class="sxs-lookup"><span data-stu-id="5144d-208">In order to define methods on the client that a Hub can call from the server, and to invoke methods on a Hub at the server, create a proxy for the Hub by calling `CreateHubProxy` on the connection object.</span></span> <span data-ttu-id="5144d-209">A cadeia de caracteres que você passa para `CreateHubProxy` é o nome da sua classe de Hub ou o nome especificado pelo `HubName` atributo, se um foi usado no servidor.</span><span class="sxs-lookup"><span data-stu-id="5144d-209">The string you pass in to `CreateHubProxy` is the name of your Hub class, or the name specified by the `HubName` attribute if one was used on the server.</span></span> <span data-ttu-id="5144d-210">A correspondência de nome não diferencia maiúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="5144d-210">Name matching is case-insensitive.</span></span>

<span data-ttu-id="5144d-211">**Classe de Hub no servidor**</span><span class="sxs-lookup"><span data-stu-id="5144d-211">**Hub class on server**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample10.cs?highlight=1)]

<span data-ttu-id="5144d-212">**Criar proxy de cliente para a classe de Hub**</span><span class="sxs-lookup"><span data-stu-id="5144d-212">**Create client proxy for the Hub class**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample11.cs?highlight=3)]

<span data-ttu-id="5144d-213">Se você decorar a classe de Hub com um `HubName` atributo, use esse nome.</span><span class="sxs-lookup"><span data-stu-id="5144d-213">If you decorate your Hub class with a `HubName` attribute, use that name.</span></span>

<span data-ttu-id="5144d-214">**Classe de Hub no servidor**</span><span class="sxs-lookup"><span data-stu-id="5144d-214">**Hub class on server**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample12.cs)]

<span data-ttu-id="5144d-215">**Criar proxy de cliente para a classe de Hub**</span><span class="sxs-lookup"><span data-stu-id="5144d-215">**Create client proxy for the Hub class**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample13.cs?highlight=3)]

<span data-ttu-id="5144d-216">Se você chamar `HubConnection.CreateHubProxy` várias vezes com o mesmo `hubName` , obterá o mesmo objeto armazenado em cache `IHubProxy` .</span><span class="sxs-lookup"><span data-stu-id="5144d-216">If you call `HubConnection.CreateHubProxy` multiple times with the same `hubName`, you get the same cached `IHubProxy` object.</span></span>

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="5144d-217">Como definir métodos no cliente que o servidor pode chamar</span><span class="sxs-lookup"><span data-stu-id="5144d-217">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="5144d-218">Para definir um método que o servidor pode chamar, use o método do proxy `On` para registrar um manipulador de eventos.</span><span class="sxs-lookup"><span data-stu-id="5144d-218">To define a method that the server can call, use the proxy's `On` method to register an event handler.</span></span>

<span data-ttu-id="5144d-219">A correspondência de nome de método não diferencia maiúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="5144d-219">Method name matching is case-insensitive.</span></span> <span data-ttu-id="5144d-220">Por exemplo, `Clients.All.UpdateStockPrice` no servidor será executado `updateStockPrice` , `updatestockprice` ou `UpdateStockPrice` no cliente.</span><span class="sxs-lookup"><span data-stu-id="5144d-220">For example, `Clients.All.UpdateStockPrice` on the server will execute `updateStockPrice`, `updatestockprice`, or `UpdateStockPrice` on the client.</span></span>

<span data-ttu-id="5144d-221">Diferentes plataformas de cliente têm requisitos diferentes para a maneira como você escreve o código do método para atualizar a interface do usuário.</span><span class="sxs-lookup"><span data-stu-id="5144d-221">Different client platforms have different requirements for how you write method code to update the UI.</span></span> <span data-ttu-id="5144d-222">Os exemplos mostrados são para clientes do WinRT (Windows Store .NET).</span><span class="sxs-lookup"><span data-stu-id="5144d-222">The examples shown are for WinRT (Windows Store .NET) clients.</span></span> <span data-ttu-id="5144d-223">Os exemplos de aplicativos WPF, Silverlight e console são fornecidos em [uma seção separada mais adiante neste tópico](#wpfsl).</span><span class="sxs-lookup"><span data-stu-id="5144d-223">WPF, Silverlight, and console application examples are provided in [a separate section later in this topic](#wpfsl).</span></span>

<a id="clientmethodswithoutparms"></a>

### <a name="methods-without-parameters"></a><span data-ttu-id="5144d-224">Métodos sem parâmetros</span><span class="sxs-lookup"><span data-stu-id="5144d-224">Methods without parameters</span></span>

<span data-ttu-id="5144d-225">Se o método que você está manipulando não tiver parâmetros, use a sobrecarga não genérica do `On` método:</span><span class="sxs-lookup"><span data-stu-id="5144d-225">If the method you're handling does not have parameters, use the non-generic overload of the `On` method:</span></span>

<span data-ttu-id="5144d-226">**Código do servidor chamando método de cliente sem parâmetros**</span><span class="sxs-lookup"><span data-stu-id="5144d-226">**Server code calling client method without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample14.cs?highlight=5)]

<span data-ttu-id="5144d-227">**Código de cliente do WinRT para o método chamado do servidor sem parâmetros ([consulte os exemplos do WPF e do Silverlight mais adiante neste tópico](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="5144d-227">**WinRT Client code for method called from server without parameters ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample15.cs)]

<a id="clientmethodswithparmtypes"></a>

### <a name="methods-with-parameters-specifying-the-parameter-types"></a><span data-ttu-id="5144d-228">Métodos com parâmetros, especificando os tipos de parâmetro</span><span class="sxs-lookup"><span data-stu-id="5144d-228">Methods with parameters, specifying the parameter types</span></span>

<span data-ttu-id="5144d-229">Se o método que você está manipulando tiver parâmetros, especifique os tipos dos parâmetros como os tipos genéricos do `On` método.</span><span class="sxs-lookup"><span data-stu-id="5144d-229">If the method you're handling has parameters, specify the types of the parameters as the generic types of the `On` method.</span></span> <span data-ttu-id="5144d-230">Há sobrecargas genéricas do `On` método para permitir que você especifique até oito parâmetros (4 em Windows Phone 7).</span><span class="sxs-lookup"><span data-stu-id="5144d-230">There are generic overloads of the `On` method to enable you to specify up to 8 parameters (4 on Windows Phone 7).</span></span> <span data-ttu-id="5144d-231">No exemplo a seguir, um parâmetro é enviado para o `UpdateStockPrice` método.</span><span class="sxs-lookup"><span data-stu-id="5144d-231">In the following example, one parameter is sent to the `UpdateStockPrice` method.</span></span>

<span data-ttu-id="5144d-232">**Código do servidor que chama o método de cliente com um parâmetro**</span><span class="sxs-lookup"><span data-stu-id="5144d-232">**Server code calling client method with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample16.cs?highlight=3)]

<span data-ttu-id="5144d-233">**A classe Stock usada para o parâmetro**</span><span class="sxs-lookup"><span data-stu-id="5144d-233">**The Stock class used for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample17.cs)]

<span data-ttu-id="5144d-234">**Código de cliente do WinRT para um método chamado do servidor com um parâmetro ([consulte os exemplos do WPF e do Silverlight mais adiante neste tópico](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="5144d-234">**WinRT Client code for a method called from server with a parameter ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample18.cs?highlight=1,5)]

<a id="clientmethodswithdynamparms"></a>

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a><span data-ttu-id="5144d-235">Métodos com parâmetros, especificando objetos dinâmicos para os parâmetros</span><span class="sxs-lookup"><span data-stu-id="5144d-235">Methods with parameters, specifying dynamic objects for the parameters</span></span>

<span data-ttu-id="5144d-236">Como alternativa para especificar parâmetros como tipos genéricos do `On` método, você pode especificar parâmetros como objetos dinâmicos:</span><span class="sxs-lookup"><span data-stu-id="5144d-236">As an alternative to specifying parameters as generic types of the `On` method, you can specify parameters as dynamic objects:</span></span>

<span data-ttu-id="5144d-237">**Código do servidor que chama o método de cliente com um parâmetro**</span><span class="sxs-lookup"><span data-stu-id="5144d-237">**Server code calling client method with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample19.cs?highlight=3)]

<span data-ttu-id="5144d-238">**A classe Stock usada para o parâmetro**</span><span class="sxs-lookup"><span data-stu-id="5144d-238">**The Stock class used for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample20.cs)]

<span data-ttu-id="5144d-239">**Código de cliente do WinRT para um método chamado do servidor com um parâmetro, usando um objeto dinâmico para o parâmetro ([consulte os exemplos do WPF e do Silverlight mais adiante neste tópico](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="5144d-239">**WinRT Client code for a method called from server with a parameter, using a dynamic object for the parameter ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample21.cs?highlight=1,5)]

<a id="removehandler"></a>

### <a name="how-to-remove-a-handler"></a><span data-ttu-id="5144d-240">Como remover um manipulador</span><span class="sxs-lookup"><span data-stu-id="5144d-240">How to remove a handler</span></span>

<span data-ttu-id="5144d-241">Para remover um manipulador, chame seu `Dispose` método.</span><span class="sxs-lookup"><span data-stu-id="5144d-241">To remove a handler, call its `Dispose` method.</span></span>

<span data-ttu-id="5144d-242">**Código do cliente para um método chamado do servidor**</span><span class="sxs-lookup"><span data-stu-id="5144d-242">**Client code for a method called from server**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample22.cs?highlight=1)]

<span data-ttu-id="5144d-243">**Código do cliente para remover o manipulador**</span><span class="sxs-lookup"><span data-stu-id="5144d-243">**Client code to remove the handler**</span></span>

[!code-css[Main](hubs-api-guide-net-client/samples/sample23.css?highlight=1)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="5144d-244">Como chamar métodos de servidor do cliente</span><span class="sxs-lookup"><span data-stu-id="5144d-244">How to call server methods from the client</span></span>

<span data-ttu-id="5144d-245">Para chamar um método no servidor, use o `Invoke` método no proxy do Hub.</span><span class="sxs-lookup"><span data-stu-id="5144d-245">To call a method on the server, use the `Invoke` method on the Hub proxy.</span></span>

<span data-ttu-id="5144d-246">Se o método de servidor não tiver nenhum valor de retorno, use a sobrecarga não genérica do `Invoke` método.</span><span class="sxs-lookup"><span data-stu-id="5144d-246">If the server method has no return value, use the non-generic overload of the `Invoke` method.</span></span>

<span data-ttu-id="5144d-247">**Código do servidor para um método que não tem valor de retorno**</span><span class="sxs-lookup"><span data-stu-id="5144d-247">**Server code for a method that has no return value**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample24.cs?highlight=3)]

<span data-ttu-id="5144d-248">**Código do cliente chamando um método que não tem valor de retorno**</span><span class="sxs-lookup"><span data-stu-id="5144d-248">**Client code calling a method that has no return value**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample25.cs?highlight=1)]

<span data-ttu-id="5144d-249">Se o método de servidor tiver um valor de retorno, especifique o tipo de retorno como o tipo genérico do `Invoke` método.</span><span class="sxs-lookup"><span data-stu-id="5144d-249">If the server method has a return value, specify the return type as the generic type of the `Invoke` method.</span></span>

<span data-ttu-id="5144d-250">**Código de servidor para um método que tem um valor de retorno e usa um parâmetro de tipo complexo**</span><span class="sxs-lookup"><span data-stu-id="5144d-250">**Server code for a method that has a return value and takes a complex type parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample26.cs?highlight=1)]

<span data-ttu-id="5144d-251">**A classe Stock usada para o parâmetro e o valor de retorno**</span><span class="sxs-lookup"><span data-stu-id="5144d-251">**The Stock class used for the parameter and return value**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample27.cs)]

<span data-ttu-id="5144d-252">**O código do cliente que chama um método que tem um valor de retorno e usa um parâmetro de tipo complexo, em um método assíncrono ASP.NET 4,5**</span><span class="sxs-lookup"><span data-stu-id="5144d-252">**Client code calling a method that has a return value and takes a complex type parameter, in an ASP.NET 4.5 async method**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample28.cs?highlight=1-2)]

<span data-ttu-id="5144d-253">**O código do cliente que chama um método que tem um valor de retorno e usa um parâmetro de tipo complexo, em um método síncrono**</span><span class="sxs-lookup"><span data-stu-id="5144d-253">**Client code calling a method that has a return value and takes a complex type parameter, in a synchronous method**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample29.cs?highlight=1-2)]

<span data-ttu-id="5144d-254">O `Invoke` método é executado de forma assíncrona e retorna um `Task` objeto.</span><span class="sxs-lookup"><span data-stu-id="5144d-254">The `Invoke` method executes asynchronously and returns a `Task` object.</span></span> <span data-ttu-id="5144d-255">Se você não especificar `await` ou `.Wait()` , a próxima linha de código será executada antes de o método que você invocar terminar de ser executado.</span><span class="sxs-lookup"><span data-stu-id="5144d-255">If you don't specify `await` or `.Wait()`, the next line of code will execute before the method that you invoke has finished executing.</span></span>

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="5144d-256">Como tratar eventos de tempo de vida da conexão</span><span class="sxs-lookup"><span data-stu-id="5144d-256">How to handle connection lifetime events</span></span>

<span data-ttu-id="5144d-257">O signalr fornece os seguintes eventos de tempo de vida de conexão que você pode manipular:</span><span class="sxs-lookup"><span data-stu-id="5144d-257">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="5144d-258">`Received`: Ocorre quando os dados são recebidos na conexão.</span><span class="sxs-lookup"><span data-stu-id="5144d-258">`Received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="5144d-259">Fornece os dados recebidos.</span><span class="sxs-lookup"><span data-stu-id="5144d-259">Provides the received data.</span></span>
- <span data-ttu-id="5144d-260">`ConnectionSlow`: Ocorre quando o cliente detecta uma conexão lenta ou com frequência.</span><span class="sxs-lookup"><span data-stu-id="5144d-260">`ConnectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="5144d-261">`Reconnecting`: Ocorre quando o transporte subjacente começa a se reconectar.</span><span class="sxs-lookup"><span data-stu-id="5144d-261">`Reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="5144d-262">`Reconnected`: Ocorre quando o transporte subjacente é reconectado.</span><span class="sxs-lookup"><span data-stu-id="5144d-262">`Reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="5144d-263">`StateChanged`: Ocorre quando o estado da conexão é alterado.</span><span class="sxs-lookup"><span data-stu-id="5144d-263">`StateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="5144d-264">Fornece o estado antigo e o novo estado.</span><span class="sxs-lookup"><span data-stu-id="5144d-264">Provides the old state and the new state.</span></span> <span data-ttu-id="5144d-265">Para obter informações sobre valores de estado de conexão, consulte [Enumeração ConnectionState](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="5144d-265">For information about connection state values see [ConnectionState Enumeration](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).</span></span>
- <span data-ttu-id="5144d-266">`Closed`: Ocorre quando a conexão é desconectada.</span><span class="sxs-lookup"><span data-stu-id="5144d-266">`Closed`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="5144d-267">Por exemplo, se você quiser exibir mensagens de aviso para erros que não são fatais, mas causar problemas intermitentes de conexão, como lentidão ou descartar frequentes da conexão, manipule o `ConnectionSlow` evento.</span><span class="sxs-lookup"><span data-stu-id="5144d-267">For example, if you want to display warning messages for errors that are not fatal but cause intermittent connection problems, such as slowness or frequent dropping of the connection, handle the `ConnectionSlow` event.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample30.cs)]

<span data-ttu-id="5144d-268">Para obter mais informações, consulte [noções básicas e manipulação de eventos de tempo de vida de conexão no signalr](handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="5144d-268">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](handling-connection-lifetime-events.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="5144d-269">Como tratar erros</span><span class="sxs-lookup"><span data-stu-id="5144d-269">How to handle errors</span></span>

<span data-ttu-id="5144d-270">Se você não habilitar explicitamente mensagens de erro detalhadas no servidor, o objeto de exceção que o Signalr retorna após um erro contém informações mínimas sobre o erro.</span><span class="sxs-lookup"><span data-stu-id="5144d-270">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="5144d-271">Por exemplo, se uma chamada para `newContosoChatMessage` falhar, a mensagem de erro no objeto de erro conterá " `There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.` " não é recomendável enviar mensagens de erro detalhadas aos clientes em produção por motivos de segurança, mas se você quiser habilitar mensagens de erro detalhadas para fins de solução de problemas, use o código a seguir no servidor.</span><span class="sxs-lookup"><span data-stu-id="5144d-271">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample31.cs?highlight=2)]

<a id="handleerrors"></a>

<span data-ttu-id="5144d-272">Para lidar com erros que o Signalr gera, você pode adicionar um manipulador para o `Error` evento no objeto de conexão.</span><span class="sxs-lookup"><span data-stu-id="5144d-272">To handle errors that SignalR raises, you can add a handler for the `Error` event on the connection object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample32.cs)]

<span data-ttu-id="5144d-273">Para tratar erros de invocações de método, empacote o código em um bloco try-catch.</span><span class="sxs-lookup"><span data-stu-id="5144d-273">To handle errors from method invocations, wrap the code in a try-catch block.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample33.cs)]

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="5144d-274">Como habilitar o log do lado do cliente</span><span class="sxs-lookup"><span data-stu-id="5144d-274">How to enable client-side logging</span></span>

<span data-ttu-id="5144d-275">Para habilitar o log do lado do cliente, defina as `TraceLevel` `TraceWriter` Propriedades e no objeto de conexão.</span><span class="sxs-lookup"><span data-stu-id="5144d-275">To enable client-side logging, set the `TraceLevel` and `TraceWriter` properties on the connection object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample34.cs?highlight=3-4)]

<a id="wpfsl"></a>

## <a name="wpf-silverlight-and-console-application-code-samples-for-client-methods-that-the-server-can-call"></a><span data-ttu-id="5144d-276">Exemplos de código de aplicativo do WPF, Silverlight e console para métodos de cliente que o servidor pode chamar</span><span class="sxs-lookup"><span data-stu-id="5144d-276">WPF, Silverlight, and console application code samples for client methods that the server can call</span></span>

<span data-ttu-id="5144d-277">Os exemplos de código mostrados anteriormente para definir métodos de cliente que o servidor pode chamar aplicam a clientes do WinRT.</span><span class="sxs-lookup"><span data-stu-id="5144d-277">The code samples shown earlier for defining client methods that the server can call apply to WinRT clients.</span></span> <span data-ttu-id="5144d-278">Os exemplos a seguir mostram o código equivalente para o WPF, o Silverlight e os clientes de aplicativos de console.</span><span class="sxs-lookup"><span data-stu-id="5144d-278">The following samples show the equivalent code for WPF, Silverlight, and console application clients.</span></span>

### <a name="methods-without-parameters"></a><span data-ttu-id="5144d-279">Métodos sem parâmetros</span><span class="sxs-lookup"><span data-stu-id="5144d-279">Methods without parameters</span></span>

<span data-ttu-id="5144d-280">**Código de cliente do WPF para o método chamado do servidor sem parâmetros**</span><span class="sxs-lookup"><span data-stu-id="5144d-280">**WPF client code for method called from server without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample35.cs?highlight=1)]

<span data-ttu-id="5144d-281">**Código do cliente Silverlight para o método chamado do servidor sem parâmetros**</span><span class="sxs-lookup"><span data-stu-id="5144d-281">**Silverlight client code for method called from server without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample36.cs?highlight=1)]

<span data-ttu-id="5144d-282">**Código do cliente do aplicativo de console para o método chamado do servidor sem parâmetros**</span><span class="sxs-lookup"><span data-stu-id="5144d-282">**Console application client code for method called from server without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample37.cs?highlight=1)]

### <a name="methods-with-parameters-specifying-the-parameter-types"></a><span data-ttu-id="5144d-283">Métodos com parâmetros, especificando os tipos de parâmetro</span><span class="sxs-lookup"><span data-stu-id="5144d-283">Methods with parameters, specifying the parameter types</span></span>

<span data-ttu-id="5144d-284">**Código de cliente do WPF para um método chamado do servidor com um parâmetro**</span><span class="sxs-lookup"><span data-stu-id="5144d-284">**WPF client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample38.cs?highlight=1,4)]

<span data-ttu-id="5144d-285">**Código do cliente Silverlight para um método chamado do servidor com um parâmetro**</span><span class="sxs-lookup"><span data-stu-id="5144d-285">**Silverlight client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample39.cs?highlight=1,5)]

<span data-ttu-id="5144d-286">**Código de cliente do aplicativo de console para um método chamado do servidor com um parâmetro**</span><span class="sxs-lookup"><span data-stu-id="5144d-286">**Console application client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample40.cs?highlight=1-2)]

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a><span data-ttu-id="5144d-287">Métodos com parâmetros, especificando objetos dinâmicos para os parâmetros</span><span class="sxs-lookup"><span data-stu-id="5144d-287">Methods with parameters, specifying dynamic objects for the parameters</span></span>

<span data-ttu-id="5144d-288">**Código de cliente do WPF para um método chamado do servidor com um parâmetro, usando um objeto dinâmico para o parâmetro**</span><span class="sxs-lookup"><span data-stu-id="5144d-288">**WPF client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample41.cs?highlight=1,4)]

<span data-ttu-id="5144d-289">**Código do cliente Silverlight para um método chamado do servidor com um parâmetro, usando um objeto dinâmico para o parâmetro**</span><span class="sxs-lookup"><span data-stu-id="5144d-289">**Silverlight client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample42.cs?highlight=1,5)]

<span data-ttu-id="5144d-290">**Código de cliente do aplicativo de console para um método chamado do servidor com um parâmetro, usando um objeto dinâmico para o parâmetro**</span><span class="sxs-lookup"><span data-stu-id="5144d-290">**Console application client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample43.cs?highlight=1-2)]
