---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
title: Guia da API de hubs do Signalr ASP.NET – cliente JavaScript | Microsoft Docs
author: bradygaster
description: Este documento fornece uma introdução ao uso da API de hubs para o Signalr versão 2 em clientes JavaScript, como navegadores e Applicat da Windows Store (WinJS)...
ms.author: bradyg
ms.date: 01/15/2019
ms.assetid: a9fd4dc0-1b96-4443-82ca-932a5b4a8ea4
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
msc.type: authoredcontent
ms.openlocfilehash: 8befe133c3627dac1f7d011959c68e2054d345da
ms.sourcegitcommit: c62ec20b453cee3249eb894ecd75013b57d078f0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/21/2020
ms.locfileid: "92345429"
---
# <a name="aspnet-signalr-hubs-api-guide---javascript-client"></a><span data-ttu-id="d1c2d-103">Guia da API de hubs do Signalr ASP.NET – cliente JavaScript</span><span class="sxs-lookup"><span data-stu-id="d1c2d-103">ASP.NET SignalR Hubs API Guide - JavaScript Client</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="d1c2d-104">Este documento fornece uma introdução ao uso da API de hubs para o Signalr versão 2 em clientes JavaScript, como navegadores e aplicativos de WinJS (Windows Store).</span><span class="sxs-lookup"><span data-stu-id="d1c2d-104">This document provides an introduction to using the Hubs API for SignalR version 2 in JavaScript clients, such as browsers and Windows Store (WinJS) applications.</span></span>
>
> <span data-ttu-id="d1c2d-105">A API de hubs de Signalr permite que você faça RPCs (chamadas de procedimento remoto) de um servidor para clientes conectados e de clientes para o servidor.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-105">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="d1c2d-106">No código do servidor, você define métodos que podem ser chamados por clientes e chama métodos que são executados no cliente.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-106">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="d1c2d-107">No código do cliente, você define métodos que podem ser chamados do servidor e chama métodos que são executados no servidor.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-107">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="d1c2d-108">O signalr cuida de todo o direcionamento de cliente para servidor para você.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-108">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
>
> <span data-ttu-id="d1c2d-109">O signalr também oferece uma API de nível inferior chamada conexões persistentes.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-109">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="d1c2d-110">Para obter uma introdução ao Signalr, hubs e conexões persistentes, consulte [introdução ao signalr](../getting-started/introduction-to-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="d1c2d-110">For an introduction to SignalR, Hubs, and Persistent Connections, see [Introduction to SignalR](../getting-started/introduction-to-signalr.md).</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="d1c2d-111">Versões de software usadas neste tópico</span><span class="sxs-lookup"><span data-stu-id="d1c2d-111">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="d1c2d-112">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="d1c2d-112">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/)
> - <span data-ttu-id="d1c2d-113">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="d1c2d-113">.NET 4.5</span></span>
> - <span data-ttu-id="d1c2d-114">Sinalização versão 2</span><span class="sxs-lookup"><span data-stu-id="d1c2d-114">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="d1c2d-115">Versões anteriores deste tópico</span><span class="sxs-lookup"><span data-stu-id="d1c2d-115">Previous versions of this topic</span></span>
>
> <span data-ttu-id="d1c2d-116">Para obter informações sobre versões anteriores do Signalr, confira [versões mais antigas do signalr](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="d1c2d-116">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="d1c2d-117">Perguntas e comentários</span><span class="sxs-lookup"><span data-stu-id="d1c2d-117">Questions and comments</span></span>
>
> <span data-ttu-id="d1c2d-118">Deixe comentários sobre como você gostou deste tutorial e o que poderíamos melhorar nos comentários na parte inferior da página.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-118">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="d1c2d-119">Se você tiver dúvidas que não estão diretamente relacionadas ao tutorial, poderá lançá-las no fórum do [signalr ASP.net](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [stackoverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="d1c2d-119">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

## <a name="overview"></a><span data-ttu-id="d1c2d-120">Visão geral</span><span class="sxs-lookup"><span data-stu-id="d1c2d-120">Overview</span></span>

<span data-ttu-id="d1c2d-121">Este documento contém as seguintes seções:</span><span class="sxs-lookup"><span data-stu-id="d1c2d-121">This document contains the following sections:</span></span>

- [<span data-ttu-id="d1c2d-122">O proxy gerado e o que ele faz por você</span><span class="sxs-lookup"><span data-stu-id="d1c2d-122">The generated proxy and what it does for you</span></span>](#genproxy)

    - [<span data-ttu-id="d1c2d-123">Quando usar o proxy gerado</span><span class="sxs-lookup"><span data-stu-id="d1c2d-123">When to use the generated proxy</span></span>](#cantusegenproxy)
- [<span data-ttu-id="d1c2d-124">Configuração do cliente</span><span class="sxs-lookup"><span data-stu-id="d1c2d-124">Client Setup</span></span>](#clientsetup)

    - [<span data-ttu-id="d1c2d-125">Como referenciar o proxy gerado dinamicamente</span><span class="sxs-lookup"><span data-stu-id="d1c2d-125">How to reference the dynamically generated proxy</span></span>](#dynamicproxy)
    - [<span data-ttu-id="d1c2d-126">Como criar um arquivo físico para o proxy gerado pelo Signalr</span><span class="sxs-lookup"><span data-stu-id="d1c2d-126">How to create a physical file for the SignalR generated proxy</span></span>](#manualproxy)
- [<span data-ttu-id="d1c2d-127">Como estabelecer uma conexão</span><span class="sxs-lookup"><span data-stu-id="d1c2d-127">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="d1c2d-128">$. Connection. Hub é o mesmo objeto que $. hubConnection () cria</span><span class="sxs-lookup"><span data-stu-id="d1c2d-128">$.connection.hub is the same object that $.hubConnection() creates</span></span>](#connequivalence)
    - [<span data-ttu-id="d1c2d-129">Execução assíncrona do método Start</span><span class="sxs-lookup"><span data-stu-id="d1c2d-129">Asynchronous execution of the start method</span></span>](#asyncstart)
- [<span data-ttu-id="d1c2d-130">Como estabelecer uma conexão entre domínios</span><span class="sxs-lookup"><span data-stu-id="d1c2d-130">How to establish a cross-domain connection</span></span>](#crossdomain)
- [<span data-ttu-id="d1c2d-131">Como configurar a conexão</span><span class="sxs-lookup"><span data-stu-id="d1c2d-131">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="d1c2d-132">Como especificar parâmetros de cadeia de caracteres de consulta</span><span class="sxs-lookup"><span data-stu-id="d1c2d-132">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="d1c2d-133">Como especificar o método de transporte</span><span class="sxs-lookup"><span data-stu-id="d1c2d-133">How to specify the transport method</span></span>](#transport)
- [<span data-ttu-id="d1c2d-134">Como obter um proxy para uma classe de Hub</span><span class="sxs-lookup"><span data-stu-id="d1c2d-134">How to get a proxy for a Hub class</span></span>](#getproxy)
- [<span data-ttu-id="d1c2d-135">Como definir métodos no cliente que o servidor pode chamar</span><span class="sxs-lookup"><span data-stu-id="d1c2d-135">How to define methods on the client that the server can call</span></span>](#callclient)
- [<span data-ttu-id="d1c2d-136">Como chamar métodos de servidor do cliente</span><span class="sxs-lookup"><span data-stu-id="d1c2d-136">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="d1c2d-137">Como tratar eventos de tempo de vida da conexão</span><span class="sxs-lookup"><span data-stu-id="d1c2d-137">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="d1c2d-138">Como tratar erros</span><span class="sxs-lookup"><span data-stu-id="d1c2d-138">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="d1c2d-139">Como habilitar o log do lado do cliente</span><span class="sxs-lookup"><span data-stu-id="d1c2d-139">How to enable client-side logging</span></span>](#logging)

<span data-ttu-id="d1c2d-140">Para obter a documentação sobre como programar os clientes do servidor ou do .NET, consulte os seguintes recursos:</span><span class="sxs-lookup"><span data-stu-id="d1c2d-140">For documentation on how to program the server or .NET clients, see the following resources:</span></span>

- [<span data-ttu-id="d1c2d-141">Guia da API de hubs de signalr-servidor</span><span class="sxs-lookup"><span data-stu-id="d1c2d-141">SignalR Hubs API Guide - Server</span></span>](hubs-api-guide-server.md)
- [<span data-ttu-id="d1c2d-142">Guia da API de hubs do signalr-cliente .NET</span><span class="sxs-lookup"><span data-stu-id="d1c2d-142">SignalR Hubs API Guide - .NET Client</span></span>](hubs-api-guide-net-client.md)

<span data-ttu-id="d1c2d-143">O componente de servidor Signalr 2 só está disponível no .NET 4,5 (embora haja um cliente .NET para o Signalr 2 no .NET 4,0).</span><span class="sxs-lookup"><span data-stu-id="d1c2d-143">The SignalR 2 server component is only available on .NET 4.5 (though there is a .NET client for SignalR 2 on .NET 4.0).</span></span>

<a id="genproxy"></a>

## <a name="the-generated-proxy-and-what-it-does-for-you"></a><span data-ttu-id="d1c2d-144">O proxy gerado e o que ele faz por você</span><span class="sxs-lookup"><span data-stu-id="d1c2d-144">The generated proxy and what it does for you</span></span>

<span data-ttu-id="d1c2d-145">Você pode programar um cliente JavaScript para se comunicar com um serviço de sinalização com ou sem um proxy que o Signalr gera para você.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-145">You can program a JavaScript client to communicate with a SignalR service with or without a proxy that SignalR generates for you.</span></span> <span data-ttu-id="d1c2d-146">O que o proxy faz para você é simplificar a sintaxe do código que você usa para se conectar, escrever métodos que o servidor chama e chamar métodos no servidor.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-146">What the proxy does for you is simplify the syntax of the code you use to connect, write methods that the server calls, and call methods on the server.</span></span>

<span data-ttu-id="d1c2d-147">Quando você escreve o código para chamar métodos de servidor, o proxy gerado permite que você use a sintaxe que parece que você estava executando uma função local: você pode gravar `serverMethod(arg1, arg2)` em vez de `invoke('serverMethod', arg1, arg2)` .</span><span class="sxs-lookup"><span data-stu-id="d1c2d-147">When you write code to call server methods, the generated proxy enables you to use syntax that looks as though you were executing a local function: you can write `serverMethod(arg1, arg2)` instead of `invoke('serverMethod', arg1, arg2)`.</span></span> <span data-ttu-id="d1c2d-148">A sintaxe de proxy gerada também permite um erro imediato e inteligível do lado do cliente se você digitar indigitadamente um nome de método de servidor.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-148">The generated proxy syntax also enables an immediate and intelligible client-side error if you mistype a server method name.</span></span> <span data-ttu-id="d1c2d-149">E se você criar manualmente o arquivo que define os proxies, também poderá obter suporte IntelliSense para escrever código que chama métodos de servidor.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-149">And if you manually create the file that defines the proxies, you can also get IntelliSense support for writing code that calls server methods.</span></span>

<span data-ttu-id="d1c2d-150">Por exemplo, suponha que você tenha a seguinte classe de Hub no servidor:</span><span class="sxs-lookup"><span data-stu-id="d1c2d-150">For example, suppose you have the following Hub class on the server:</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample1.cs?highlight=1,3,5)]

<span data-ttu-id="d1c2d-151">Os exemplos de código a seguir mostram a aparência do código JavaScript para invocar o `NewContosoChatMessage` método no servidor e receber invocações do `addContosoChatMessageToPage` método do servidor.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-151">The following code examples show what JavaScript code looks like for invoking the `NewContosoChatMessage` method on the server and receiving invocations of the `addContosoChatMessageToPage` method from the server.</span></span>

<span data-ttu-id="d1c2d-152">**Com o proxy gerado**</span><span class="sxs-lookup"><span data-stu-id="d1c2d-152">**With the generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample2.js?highlight=1-2,8)]

<span data-ttu-id="d1c2d-153">**Sem o proxy gerado**</span><span class="sxs-lookup"><span data-stu-id="d1c2d-153">**Without the generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample3.js?highlight=2-3,9)]

<a id="cantusegenproxy"></a>

### <a name="when-to-use-the-generated-proxy"></a><span data-ttu-id="d1c2d-154">Quando usar o proxy gerado</span><span class="sxs-lookup"><span data-stu-id="d1c2d-154">When to use the generated proxy</span></span>

<span data-ttu-id="d1c2d-155">Se você quiser registrar vários manipuladores de eventos para um método de cliente chamado pelo servidor, não poderá usar o proxy gerado.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-155">If you want to register multiple event handlers for a client method that the server calls, you can't use the generated proxy.</span></span> <span data-ttu-id="d1c2d-156">Caso contrário, você pode optar por usar o proxy gerado ou não com base na sua preferência de codificação.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-156">Otherwise, you can choose to use the generated proxy or not based on your coding preference.</span></span> <span data-ttu-id="d1c2d-157">Se você optar por não usá-lo, não precisará fazer referência à URL "signalr/hubs" em um `script` elemento no seu código de cliente.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-157">If you choose not to use it, you don't have to reference the "signalr/hubs" URL in a `script` element in your client code.</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="d1c2d-158">Configuração do cliente</span><span class="sxs-lookup"><span data-stu-id="d1c2d-158">Client setup</span></span>

<span data-ttu-id="d1c2d-159">Um cliente JavaScript requer referências ao jQuery e ao arquivo JavaScript do Signalr Core.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-159">A JavaScript client requires references to jQuery and the SignalR core JavaScript file.</span></span> <span data-ttu-id="d1c2d-160">A versão do jQuery deve ser 1.6.4 ou as principais versões posteriores, como 1.7.2, 1.8.2 ou 1.9.1.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-160">The jQuery version must be 1.6.4 or major later versions, such as 1.7.2, 1.8.2, or 1.9.1.</span></span> <span data-ttu-id="d1c2d-161">Se você decidir usar o proxy gerado, também precisará de uma referência ao arquivo JavaScript de proxy gerado pelo Signalr.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-161">If you decide to use the generated proxy, you also need a reference to the SignalR generated proxy JavaScript file.</span></span> <span data-ttu-id="d1c2d-162">O exemplo a seguir mostra como as referências podem parecer em uma página HTML que usa o proxy gerado.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-162">The following example shows what the references might look like in an HTML page that uses the generated proxy.</span></span>

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample4.html)]

<span data-ttu-id="d1c2d-163">Essas referências devem ser incluídas nesta ordem: jQuery First, Core do Signalr depois disso e proxies do Signalr por último.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-163">These references must be included in this order: jQuery first, SignalR core after that, and SignalR proxies last.</span></span>

<a id="dynamicproxy"></a>

### <a name="how-to-reference-the-dynamically-generated-proxy"></a><span data-ttu-id="d1c2d-164">Como referenciar o proxy gerado dinamicamente</span><span class="sxs-lookup"><span data-stu-id="d1c2d-164">How to reference the dynamically generated proxy</span></span>

<span data-ttu-id="d1c2d-165">No exemplo anterior, a referência ao proxy gerado pelo Signalr é gerada dinamicamente o código JavaScript, não para um arquivo físico.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-165">In the preceding example, the reference to the SignalR generated proxy is to dynamically generated JavaScript code, not to a physical file.</span></span> <span data-ttu-id="d1c2d-166">O signalr cria o código JavaScript para o proxy imediatamente e o serve para o cliente em resposta à URL "/signalr/hubs".</span><span class="sxs-lookup"><span data-stu-id="d1c2d-166">SignalR creates the JavaScript code for the proxy on the fly and serves it to the client in response to the "/signalr/hubs" URL.</span></span> <span data-ttu-id="d1c2d-167">Se você especificou uma URL base diferente para conexões de Signalr no servidor em seu `MapSignalR` método, a URL para o arquivo de proxy gerado dinamicamente será sua URL personalizada com "/hubs" acrescentado a ela.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-167">If you specified a different base URL for SignalR connections on the server in your `MapSignalR` method, the URL for the dynamically generated proxy file is your custom URL with "/hubs" appended to it.</span></span>

> [!NOTE]
> <span data-ttu-id="d1c2d-168">Para clientes JavaScript do Windows 8 (Windows Store), use o arquivo de proxy físico em vez de um gerado dinamicamente.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-168">For Windows 8 (Windows Store) JavaScript clients, use the physical proxy file instead of the dynamically generated one.</span></span> <span data-ttu-id="d1c2d-169">Para obter mais informações, consulte [como criar um arquivo físico para o proxy gerado pelo signalr](#manualproxy) mais adiante neste tópico.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-169">For more information, see [How to create a physical file for the SignalR generated proxy](#manualproxy) later in this topic.</span></span>

<span data-ttu-id="d1c2d-170">Em uma exibição do Razor do ASP.NET MVC 4 ou 5, use o til para se referir à raiz do aplicativo em sua referência de arquivo de proxy:</span><span class="sxs-lookup"><span data-stu-id="d1c2d-170">In an ASP.NET MVC 4 or 5 Razor view, use the tilde to refer to the application root in your proxy file reference:</span></span>

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample5.html)]

<span data-ttu-id="d1c2d-171">Para obter mais informações sobre como usar o Signalr no MVC 5, consulte [introdução com signalr e MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span><span class="sxs-lookup"><span data-stu-id="d1c2d-171">For more information about using SignalR in MVC 5, see [Getting Started with SignalR and MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span></span>

<span data-ttu-id="d1c2d-172">Em uma exibição Razor do ASP.NET MVC 3, use `Url.Content` para a referência do arquivo de proxy:</span><span class="sxs-lookup"><span data-stu-id="d1c2d-172">In an ASP.NET MVC 3 Razor view, use `Url.Content` for your proxy file reference:</span></span>

[!code-cshtml[Main](hubs-api-guide-javascript-client/samples/sample6.cshtml)]

<span data-ttu-id="d1c2d-173">Em um aplicativo de Web Forms ASP.NET, use `ResolveClientUrl` para a referência de arquivo de proxies ou registre-o por meio do ScriptManager usando um caminho relativo de raiz de aplicativo (começando com um til):</span><span class="sxs-lookup"><span data-stu-id="d1c2d-173">In an ASP.NET Web Forms application, use `ResolveClientUrl` for your proxies file reference or register it via the ScriptManager using an app root relative path (starting with a tilde):</span></span>

[!code-aspx[Main](hubs-api-guide-javascript-client/samples/sample7.aspx)]

<span data-ttu-id="d1c2d-174">Como regra geral, use o mesmo método para especificar a URL "/signalr/hubs" que você usa para arquivos CSS ou JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-174">As a general rule, use the same method for specifying the "/signalr/hubs" URL that you use for CSS or JavaScript files.</span></span> <span data-ttu-id="d1c2d-175">Se você especificar uma URL sem usar um til, em alguns cenários, seu aplicativo funcionará corretamente quando você testar no Visual Studio usando IIS Express, mas falhará com um erro 404 quando você implantar no IIS completo.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-175">If you specify a URL without using a tilde, in some scenarios your application will work correctly when you test in Visual Studio using IIS Express but will fail with a 404 error when you deploy to full IIS.</span></span> <span data-ttu-id="d1c2d-176">Para obter mais informações, consulte **Resolvendo referências a Root-Level recursos** em [servidores Web no Visual Studio para projetos Web do ASP.net](https://msdn.microsoft.com/library/58wxa9w5.aspx) no site do MSDN.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-176">For more information, see **Resolving References to Root-Level Resources** in [Web Servers in Visual Studio for ASP.NET Web Projects](https://msdn.microsoft.com/library/58wxa9w5.aspx) on the MSDN site.</span></span>

<span data-ttu-id="d1c2d-177">Ao executar um projeto Web no Visual Studio 2017 no modo de depuração e, se você usar o Internet Explorer como navegador, poderá ver o arquivo de proxy em **Gerenciador de soluções** em **scripts**.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-177">When you run a web project in Visual Studio 2017 in debug mode, and if you use Internet Explorer as your browser, you can see the proxy file in **Solution Explorer** under **Scripts**.</span></span>

<span data-ttu-id="d1c2d-178">Para ver o conteúdo do arquivo, clique duas vezes em **hubs**.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-178">To see the contents of the file, double-click **hubs**.</span></span> <span data-ttu-id="d1c2d-179">Se você não estiver usando o Visual Studio 2012 ou 2013 e o Internet Explorer, ou se não estiver no modo de depuração, também poderá obter o conteúdo do arquivo navegando até a URL "/signalR/hubs".</span><span class="sxs-lookup"><span data-stu-id="d1c2d-179">If you are not using Visual Studio 2012 or 2013 and Internet Explorer, or if you are not in debug mode, you can also get the contents of the file by browsing to the "/signalR/hubs" URL.</span></span> <span data-ttu-id="d1c2d-180">Por exemplo, se seu site estiver em execução em `http://localhost:56699` , vá para `http://localhost:56699/SignalR/hubs` no navegador.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-180">For example, if your site is running at `http://localhost:56699`, go to `http://localhost:56699/SignalR/hubs` in your browser.</span></span>

<a id="manualproxy"></a>

### <a name="how-to-create-a-physical-file-for-the-signalr-generated-proxy"></a><span data-ttu-id="d1c2d-181">Como criar um arquivo físico para o proxy gerado pelo Signalr</span><span class="sxs-lookup"><span data-stu-id="d1c2d-181">How to create a physical file for the SignalR generated proxy</span></span>

<span data-ttu-id="d1c2d-182">Como alternativa ao proxy gerado dinamicamente, você pode criar um arquivo físico que tem o código de proxy e fazer referência a esse arquivo.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-182">As an alternative to the dynamically generated proxy, you can create a physical file that has the proxy code and reference that file.</span></span> <span data-ttu-id="d1c2d-183">Talvez você queira fazer isso para controlar o comportamento de cache ou agrupamento, ou para obter o IntelliSense quando estiver codificando chamadas para métodos de servidor.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-183">You might want to do that for control over caching or bundling behavior, or to get IntelliSense when you are coding calls to server methods.</span></span>

<span data-ttu-id="d1c2d-184">Para criar um arquivo de proxy, execute as seguintes etapas:</span><span class="sxs-lookup"><span data-stu-id="d1c2d-184">To create a proxy file, perform the following steps:</span></span>

1. <span data-ttu-id="d1c2d-185">Instale o pacote NuGet [Microsoft. AspNet. signalr. utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) .</span><span class="sxs-lookup"><span data-stu-id="d1c2d-185">Install the [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) NuGet package.</span></span>
2. <span data-ttu-id="d1c2d-186">Abra um prompt de comando e navegue até a pasta *ferramentas* que contém o arquivo SignalR.exe.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-186">Open a command prompt and browse to the *tools* folder that contains the SignalR.exe file.</span></span> <span data-ttu-id="d1c2d-187">A pasta ferramentas está no seguinte local:</span><span class="sxs-lookup"><span data-stu-id="d1c2d-187">The tools folder is at the following location:</span></span>

    `[your solution folder]\packages\Microsoft.AspNet.SignalR.Utils.2.1.0\tools`
3. <span data-ttu-id="d1c2d-188">Digite o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="d1c2d-188">Enter the following command:</span></span>

    `signalr ghp /path:[path to the .dll that contains your Hub class]`

    <span data-ttu-id="d1c2d-189">O caminho para o *. dll* é normalmente a pasta *bin* na pasta do projeto.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-189">The path to your *.dll* is typically the *bin* folder in your project folder.</span></span>

    <span data-ttu-id="d1c2d-190">Esse comando cria um arquivo chamado *server.js* na mesma pasta que *signalr.exe*.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-190">This command creates a file named *server.js* in the same folder as *signalr.exe*.</span></span>
4. <span data-ttu-id="d1c2d-191">Coloque o arquivo de *server.js* em uma pasta apropriada em seu projeto, renomeie-o conforme apropriado para seu aplicativo e adicione uma referência a ele no lugar da referência de "signalr/hubs".</span><span class="sxs-lookup"><span data-stu-id="d1c2d-191">Put the *server.js* file in an appropriate folder in your project, rename it as appropriate for your application, and add a reference to it in place of the "signalr/hubs" reference.</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="d1c2d-192">Como estabelecer uma conexão</span><span class="sxs-lookup"><span data-stu-id="d1c2d-192">How to establish a connection</span></span>

<span data-ttu-id="d1c2d-193">Antes de poder estabelecer uma conexão, você precisa criar um objeto de conexão, criar um proxy e registrar manipuladores de eventos para métodos que podem ser chamados do servidor.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-193">Before you can establish a connection, you have to create a connection object, create a proxy, and register event handlers for methods that can be called from the server.</span></span> <span data-ttu-id="d1c2d-194">Quando o proxy e os manipuladores de eventos são configurados, estabeleça a conexão chamando o `start` método.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-194">When the proxy and event handlers are set up, establish the connection by calling the `start` method.</span></span>

<span data-ttu-id="d1c2d-195">Se você estiver usando o proxy gerado, não precisará criar o objeto de conexão em seu próprio código porque o código de proxy gerado faz isso para você.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-195">If you are using the generated proxy, you don't have to create the connection object in your own code because the generated proxy code does it for you.</span></span>

<a id="nogenconnection"></a>

<span data-ttu-id="d1c2d-196">**Estabelecer uma conexão (com o proxy gerado)**</span><span class="sxs-lookup"><span data-stu-id="d1c2d-196">**Establish a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample8.js?highlight=5)]

<span data-ttu-id="d1c2d-197">**Estabelecer uma conexão (sem o proxy gerado)**</span><span class="sxs-lookup"><span data-stu-id="d1c2d-197">**Establish a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample9.js?highlight=1,6)]

<span data-ttu-id="d1c2d-198">O código de exemplo usa a URL "/signalr" padrão para se conectar ao seu serviço Signalr.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-198">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="d1c2d-199">Para obter informações sobre como especificar uma URL base diferente, consulte [guia da API de hubs do ASP.net signalr-Server-a URL do/signalr](hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="d1c2d-199">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](hubs-api-guide-server.md#signalrurl).</span></span>

<span data-ttu-id="d1c2d-200">Por padrão, o local do hub é o servidor atual; Se você estiver se conectando a um servidor diferente, especifique a URL antes de chamar o `start` método, conforme mostrado no exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="d1c2d-200">By default, the hub location is the current server; if you are connecting to a different server, specify the URL before calling the `start` method, as shown in the following example:</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample10.js)]

> [!NOTE]
> <span data-ttu-id="d1c2d-201">Normalmente, você registra manipuladores de eventos antes de chamar o `start` método para estabelecer a conexão.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-201">Normally you register event handlers before calling the `start` method to establish the connection.</span></span> <span data-ttu-id="d1c2d-202">Se desejar registrar alguns manipuladores de eventos depois de estabelecer a conexão, você poderá fazer isso, mas deverá registrar pelo menos um dos manipuladores de eventos antes de chamar o `start` método.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-202">If you want to register some event handlers after establishing the connection, you can do that, but you must register at least one of your event handler(s) before calling the `start` method.</span></span> <span data-ttu-id="d1c2d-203">Um dos motivos para isso é que pode haver muitos hubs em um aplicativo, mas você não desejaria disparar o `OnConnected` evento em cada Hub se quiser usar apenas um deles.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-203">One reason for this is that there can be many Hubs in an application, but you wouldn't want to trigger the `OnConnected` event on every Hub if you are only going to use to one of them.</span></span> <span data-ttu-id="d1c2d-204">Quando a conexão é estabelecida, a presença de um método de cliente no proxy de um hub é o que informa ao Signalr para disparar o `OnConnected` evento.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-204">When the connection is established, the presence of a client method on a Hub's proxy is what tells SignalR to trigger the `OnConnected` event.</span></span> <span data-ttu-id="d1c2d-205">Se você não registrar nenhum manipulador de eventos antes de chamar o `start` método, poderá invocar métodos no Hub, mas o método do Hub `OnConnected` não será chamado e nenhum método de cliente será invocado do servidor.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-205">If you don't register any event handlers before calling the `start` method, you will be able to invoke methods on the Hub, but the Hub's `OnConnected` method won't be called and no client methods will be invoked from the server.</span></span>

<a id="connequivalence"></a>

### <a name="connectionhub-is-the-same-object-that-hubconnection-creates"></a><span data-ttu-id="d1c2d-206">$. Connection. Hub é o mesmo objeto que $. hubConnection () cria</span><span class="sxs-lookup"><span data-stu-id="d1c2d-206">$.connection.hub is the same object that $.hubConnection() creates</span></span>

<span data-ttu-id="d1c2d-207">Como você pode ver nos exemplos, quando você usa o proxy gerado, `$.connection.hub` refere-se ao objeto de conexão.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-207">As you can see from the examples, when you use the generated proxy, `$.connection.hub` refers to the connection object.</span></span> <span data-ttu-id="d1c2d-208">Esse é o mesmo objeto que você obtém chamando `$.hubConnection()` quando você não está usando o proxy gerado.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-208">This is the same object that you get by calling `$.hubConnection()` when you aren't using the generated proxy.</span></span> <span data-ttu-id="d1c2d-209">O código de proxy gerado cria a conexão para você executando a seguinte instrução:</span><span class="sxs-lookup"><span data-stu-id="d1c2d-209">The generated proxy code creates the connection for you by executing the following statement:</span></span>

![Criando uma conexão no arquivo de proxy gerado](hubs-api-guide-javascript-client/_static/image3.png)

<span data-ttu-id="d1c2d-211">Quando você estiver usando o proxy gerado, poderá fazer qualquer coisa com a `$.connection.hub` que você possa fazer com um objeto de conexão quando não estiver usando o proxy gerado.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-211">When you're using the generated proxy, you can do anything with `$.connection.hub` that you can do with a connection object when you're not using the generated proxy.</span></span>

<a id="asyncstart"></a>

### <a name="asynchronous-execution-of-the-start-method"></a><span data-ttu-id="d1c2d-212">Execução assíncrona do método Start</span><span class="sxs-lookup"><span data-stu-id="d1c2d-212">Asynchronous execution of the start method</span></span>

<span data-ttu-id="d1c2d-213">O `start` método é executado de forma assíncrona.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-213">The `start` method executes asynchronously.</span></span> <span data-ttu-id="d1c2d-214">Ele retorna um [objeto jQuery adiado](http://api.jquery.com/category/deferred-object/), o que significa que você pode adicionar funções de retorno de chamada chamando métodos como `pipe` , `done` e `fail` .</span><span class="sxs-lookup"><span data-stu-id="d1c2d-214">It returns a [jQuery Deferred object](http://api.jquery.com/category/deferred-object/), which means that you can add callback functions by calling methods such as `pipe`, `done`, and `fail`.</span></span> <span data-ttu-id="d1c2d-215">Se você tiver um código que deseja executar depois que a conexão for estabelecida, como uma chamada para um método de servidor, coloque esse código em uma função de retorno de chamada ou chame-o de uma função de retorno de chamada.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-215">If you have code that you want to execute after the connection is established, such as a call to a server method, put that code in a callback function or call it from a callback function.</span></span> <span data-ttu-id="d1c2d-216">O `.done` método de retorno de chamada é executado depois que a conexão é estabelecida e, depois que qualquer código que você tiver em seu `OnConnected` método de manipulador de eventos no servidor terminar a execução.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-216">The `.done` callback method is executed after the connection has been established, and after any code that you have in your `OnConnected` event handler method on the server finishes executing.</span></span>

<span data-ttu-id="d1c2d-217">Se você colocar a instrução "agora conectado" do exemplo anterior como a próxima linha de código após a `start` chamada do método (não em um `.done` retorno de chamada), a `console.log` linha será executada antes que a conexão seja estabelecida, conforme mostrado no exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="d1c2d-217">If you put the "Now connected" statement from the preceding example as the next line of code after the `start` method call (not in a `.done` callback), the `console.log` line will execute before the connection is established, as shown in the following example:</span></span>

![Maneira incorreta de escrever código que é executado depois que a conexão é estabelecida](hubs-api-guide-javascript-client/_static/image5.png)

<a id="crossdomain"></a>

## <a name="how-to-establish-a-cross-domain-connection"></a><span data-ttu-id="d1c2d-219">Como estabelecer uma conexão entre domínios</span><span class="sxs-lookup"><span data-stu-id="d1c2d-219">How to establish a cross-domain connection</span></span>

<span data-ttu-id="d1c2d-220">Normalmente, se o navegador carregar uma página do `http://contoso.com` , a conexão do signalr estará no mesmo domínio, em `http://contoso.com/signalr` .</span><span class="sxs-lookup"><span data-stu-id="d1c2d-220">Typically if the browser loads a page from `http://contoso.com`, the SignalR connection is in the same domain, at `http://contoso.com/signalr`.</span></span> <span data-ttu-id="d1c2d-221">Se a página de `http://contoso.com` estabelecer uma conexão com o `http://fabrikam.com/signalr` , que é uma conexão entre domínios.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-221">If the page from `http://contoso.com` makes a connection to `http://fabrikam.com/signalr`, that is a cross-domain connection.</span></span> <span data-ttu-id="d1c2d-222">Por motivos de segurança, as conexões entre domínios são desabilitadas por padrão.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-222">For security reasons, cross-domain connections are disabled by default.</span></span>

<span data-ttu-id="d1c2d-223">No Signalr 1. x, as solicitações entre domínios foram controladas por um único sinalizador EnableCrossDomain.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-223">In SignalR 1.x, cross domain requests were controlled by a single EnableCrossDomain flag.</span></span> <span data-ttu-id="d1c2d-224">Esse sinalizador controlava as solicitações JSONP e CORS.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-224">This flag controlled both JSONP and CORS requests.</span></span> <span data-ttu-id="d1c2d-225">Para maior flexibilidade, todo o suporte a CORS foi removido do componente de servidor do Signalr (os clientes JavaScript ainda usam CORS normalmente se detectar que o navegador dá suporte a ele) e o novo middleware OWIN foi disponibilizado para dar suporte a esses cenários.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-225">For greater flexibility, all CORS support has been removed from the server component of SignalR (JavaScript clients still use CORS normally if it is detected that the browser supports it), and new OWIN middleware has been made available to support these scenarios.</span></span>

<span data-ttu-id="d1c2d-226">Se JSONP for necessário no cliente (para dar suporte a solicitações entre domínios em navegadores mais antigos), será necessário habilitá-lo explicitamente definindo `EnableJSONP` no `HubConfiguration` objeto como `true` , conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-226">If JSONP is required on the client (to support cross-domain requests in older browsers), it will need to be enabled explicitly by setting `EnableJSONP` on the `HubConfiguration` object to `true`, as shown below.</span></span> <span data-ttu-id="d1c2d-227">O JSONP é desabilitado por padrão, pois é menos seguro do que o CORS.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-227">JSONP is disabled by default, as it is less secure than CORS.</span></span>

<span data-ttu-id="d1c2d-228">**Adicionando Microsoft. Owin. CORS ao seu projeto:** Para instalar essa biblioteca, execute o seguinte comando no console do Gerenciador de pacotes:</span><span class="sxs-lookup"><span data-stu-id="d1c2d-228">**Adding Microsoft.Owin.Cors to your project:** To install this library, run the following command in the Package Manager Console:</span></span>

`Install-Package Microsoft.Owin.Cors`

<span data-ttu-id="d1c2d-229">Este comando adicionará a versão 2.1.0 do pacote ao seu projeto.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-229">This command will add the 2.1.0 version of the package to your project.</span></span>

### <a name="calling-usecors"></a><span data-ttu-id="d1c2d-230">Chamando UseCors</span><span class="sxs-lookup"><span data-stu-id="d1c2d-230">Calling UseCors</span></span>

 <span data-ttu-id="d1c2d-231">O trecho de código a seguir demonstra como implementar conexões entre domínios no Signalr 2.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-231">The following code snippet demonstrates how to implement cross-domain connections in SignalR 2.</span></span>

<span data-ttu-id="d1c2d-232">**Implementando solicitações entre domínios no Signalr 2**</span><span class="sxs-lookup"><span data-stu-id="d1c2d-232">**Implementing cross-domain requests in SignalR 2**</span></span>

<span data-ttu-id="d1c2d-233">O código a seguir demonstra como habilitar CORS ou JSONP em um projeto do Signalr 2.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-233">The following code demonstrates how to enable CORS or JSONP in a SignalR 2 project.</span></span> <span data-ttu-id="d1c2d-234">Esse exemplo de código usa `Map` e `RunSignalR` em vez de `MapSignalR` , para que o middleware do CORS seja executado somente para solicitações do signalr que exigem suporte a CORS (em vez de para todo o tráfego no caminho especificado em `MapSignalR` .) O mapa também pode ser usado para qualquer outro middleware que precise ser executado para um prefixo de URL específico, em vez de para o aplicativo inteiro.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-234">This code sample uses `Map` and `RunSignalR` instead of `MapSignalR`, so that the CORS middleware runs only for the SignalR requests that require CORS support (rather than for all traffic at the path specified in `MapSignalR`.) Map can also be used for any other middleware that needs to run for a specific URL prefix, rather than for the entire application.</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample11.cs)]

> [!NOTE]
>
> - <span data-ttu-id="d1c2d-235">Não defina `jQuery.support.cors` como true em seu código.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-235">Don't set `jQuery.support.cors` to true in your code.</span></span>
>
>     ![Não defina jQuery. support. CORS como true](hubs-api-guide-javascript-client/_static/image7.png)
>
>     <span data-ttu-id="d1c2d-237">O signalr lida com o uso do CORS.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-237">SignalR handles the use of CORS.</span></span> <span data-ttu-id="d1c2d-238">Definir `jQuery.support.cors` como true DESABILITA JSONP porque faz com que o signalr assuma que o navegador dá suporte a CORS.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-238">Setting `jQuery.support.cors` to true disables JSONP because it causes SignalR to assume the browser supports CORS.</span></span>
> - <span data-ttu-id="d1c2d-239">Quando você estiver se conectando a uma URL de localhost, o Internet Explorer 10 não considerará uma conexão entre domínios, de modo que o aplicativo funcionará localmente com o IE 10 mesmo que você não tenha habilitado conexões entre domínios no servidor.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-239">When you're connecting to a localhost URL, Internet Explorer 10 won't consider it a cross-domain connection, so the application will work locally with IE 10 even if you haven't enabled cross-domain connections on the server.</span></span>
> - <span data-ttu-id="d1c2d-240">Para obter informações sobre como usar conexões entre domínios com o Internet Explorer 9, consulte [este thread StackOverflow](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span><span class="sxs-lookup"><span data-stu-id="d1c2d-240">For information about using cross-domain connections with Internet Explorer 9, see [this StackOverflow thread](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span></span>
> - <span data-ttu-id="d1c2d-241">Para obter informações sobre como usar conexões entre domínios com o Chrome, consulte [este thread StackOverflow](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span><span class="sxs-lookup"><span data-stu-id="d1c2d-241">For information about using cross-domain connections with Chrome, see [this StackOverflow thread](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span></span>
> - <span data-ttu-id="d1c2d-242">O código de exemplo usa a URL "/signalr" padrão para se conectar ao seu serviço Signalr.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-242">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="d1c2d-243">Para obter informações sobre como especificar uma URL base diferente, consulte [guia da API de hubs do ASP.net signalr-Server-a URL do/signalr](hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="d1c2d-243">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](hubs-api-guide-server.md#signalrurl).</span></span>

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="d1c2d-244">Como configurar a conexão</span><span class="sxs-lookup"><span data-stu-id="d1c2d-244">How to configure the connection</span></span>

<span data-ttu-id="d1c2d-245">Antes de estabelecer uma conexão, você pode especificar parâmetros de cadeia de caracteres de consulta ou especificar o método de transporte.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-245">Before you establish a connection, you can specify query string parameters or specify the transport method.</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="d1c2d-246">Como especificar parâmetros de cadeia de caracteres de consulta</span><span class="sxs-lookup"><span data-stu-id="d1c2d-246">How to specify query string parameters</span></span>

<span data-ttu-id="d1c2d-247">Se você quiser enviar dados ao servidor quando o cliente se conectar, poderá adicionar parâmetros de cadeia de caracteres de consulta ao objeto de conexão.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-247">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="d1c2d-248">Os exemplos a seguir mostram como definir um parâmetro de cadeia de caracteres de consulta no código do cliente.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-248">The following examples show how to set a query string parameter in client code.</span></span>

<span data-ttu-id="d1c2d-249">**Definir um valor de cadeia de caracteres de consulta antes de chamar o método Start (com o proxy gerado)**</span><span class="sxs-lookup"><span data-stu-id="d1c2d-249">**Set a query string value before calling the start method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample12.js?highlight=1)]

<span data-ttu-id="d1c2d-250">**Definir um valor de cadeia de caracteres de consulta antes de chamar o método Start (sem o proxy gerado)**</span><span class="sxs-lookup"><span data-stu-id="d1c2d-250">**Set a query string value before calling the start method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample13.js?highlight=2)]

<span data-ttu-id="d1c2d-251">O exemplo a seguir mostra como ler um parâmetro de cadeia de caracteres de consulta no código do servidor.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-251">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample14.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="d1c2d-252">Como especificar o método de transporte</span><span class="sxs-lookup"><span data-stu-id="d1c2d-252">How to specify the transport method</span></span>

<span data-ttu-id="d1c2d-253">Como parte do processo de conexão, um cliente do Signalr normalmente negocia com o servidor para determinar o melhor transporte que é suportado pelo servidor e pelo cliente.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-253">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="d1c2d-254">Se você já souber qual transporte deseja usar, poderá ignorar esse processo de negociação especificando o método de transporte ao chamar o `start` método.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-254">If you already know which transport you want to use, you can bypass this negotiation process by specifying the transport method when you call the `start` method.</span></span>

<span data-ttu-id="d1c2d-255">**O código do cliente que especifica o método de transporte (com o proxy gerado)**</span><span class="sxs-lookup"><span data-stu-id="d1c2d-255">**Client code that specifies the transport method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample15.js?highlight=1)]

<span data-ttu-id="d1c2d-256">**O código do cliente que especifica o método de transporte (sem o proxy gerado)**</span><span class="sxs-lookup"><span data-stu-id="d1c2d-256">**Client code that specifies the transport method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample16.js?highlight=2)]

<span data-ttu-id="d1c2d-257">Como alternativa, você pode especificar vários métodos de transporte na ordem em que deseja que o Signalr os experimente:</span><span class="sxs-lookup"><span data-stu-id="d1c2d-257">As an alternative, you can specify multiple transport methods in the order in which you want SignalR to try them:</span></span>

<span data-ttu-id="d1c2d-258">**O código do cliente que especifica um esquema de fallback de transporte personalizado (com o proxy gerado)**</span><span class="sxs-lookup"><span data-stu-id="d1c2d-258">**Client code that specifies a custom transport fallback scheme (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample17.js?highlight=1)]

<span data-ttu-id="d1c2d-259">**O código do cliente que especifica um esquema de fallback de transporte personalizado (sem o proxy gerado)**</span><span class="sxs-lookup"><span data-stu-id="d1c2d-259">**Client code that specifies a custom transport fallback scheme (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample18.js?highlight=2)]

<span data-ttu-id="d1c2d-260">Você pode usar os seguintes valores para especificar o método de transporte:</span><span class="sxs-lookup"><span data-stu-id="d1c2d-260">You can use the following values for specifying the transport method:</span></span>

- <span data-ttu-id="d1c2d-261">WebSockets</span><span class="sxs-lookup"><span data-stu-id="d1c2d-261">"webSockets"</span></span>
- <span data-ttu-id="d1c2d-262">"foreverFrame"</span><span class="sxs-lookup"><span data-stu-id="d1c2d-262">"foreverFrame"</span></span>
- <span data-ttu-id="d1c2d-263">"serverSentEvents"</span><span class="sxs-lookup"><span data-stu-id="d1c2d-263">"serverSentEvents"</span></span>
- <span data-ttu-id="d1c2d-264">"longPolling"</span><span class="sxs-lookup"><span data-stu-id="d1c2d-264">"longPolling"</span></span>

<span data-ttu-id="d1c2d-265">Os exemplos a seguir mostram como descobrir qual método de transporte está sendo usado por uma conexão.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-265">The following examples show how to find out which transport method is being used by a connection.</span></span>

<span data-ttu-id="d1c2d-266">**O código do cliente que exibe o método de transporte usado por uma conexão (com o proxy gerado)**</span><span class="sxs-lookup"><span data-stu-id="d1c2d-266">**Client code that displays the transport method used by a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample19.js?highlight=2)]

<span data-ttu-id="d1c2d-267">**O código do cliente que exibe o método de transporte usado por uma conexão (sem o proxy gerado)**</span><span class="sxs-lookup"><span data-stu-id="d1c2d-267">**Client code that displays the transport method used by a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample20.js?highlight=3)]

<span data-ttu-id="d1c2d-268">Para obter informações sobre como verificar o método de transporte no código do servidor, consulte [guia da API de hubs do signaler ASP.NET-Server-como obter informações sobre o cliente a partir da propriedade Context](hubs-api-guide-server.md#contextproperty).</span><span class="sxs-lookup"><span data-stu-id="d1c2d-268">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="d1c2d-269">Para obter mais informações sobre transportes e fallbacks, consulte [introdução ao signalr-transportes e fallbacks](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="d1c2d-269">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="getproxy"></a>

## <a name="how-to-get-a-proxy-for-a-hub-class"></a><span data-ttu-id="d1c2d-270">Como obter um proxy para uma classe de Hub</span><span class="sxs-lookup"><span data-stu-id="d1c2d-270">How to get a proxy for a Hub class</span></span>

<span data-ttu-id="d1c2d-271">Cada objeto de conexão que você cria encapsula informações sobre uma conexão com um serviço de sinalização que contém uma ou mais classes de Hub.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-271">Each connection object that you create encapsulates information about a connection to a SignalR service that contains one or more Hub classes.</span></span> <span data-ttu-id="d1c2d-272">Para se comunicar com uma classe de Hub, você usa um objeto proxy que você mesmo cria (se você não estiver usando o proxy gerado) ou que é gerado para você.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-272">To communicate with a Hub class, you use a proxy object which you create yourself (if you're not using the generated proxy) or which is generated for you.</span></span>

<span data-ttu-id="d1c2d-273">No cliente, o nome do proxy é uma versão do camel case do nome da classe do Hub.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-273">On the client the proxy name is a camel-cased version of the Hub class name.</span></span> <span data-ttu-id="d1c2d-274">O signalr faz essa alteração automaticamente para que o código JavaScript possa estar em conformidade com as convenções de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-274">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="d1c2d-275">**Classe de Hub no servidor**</span><span class="sxs-lookup"><span data-stu-id="d1c2d-275">**Hub class on server**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample21.cs?highlight=1)]

<span data-ttu-id="d1c2d-276">**Obter uma referência para o proxy de cliente gerado para o Hub**</span><span class="sxs-lookup"><span data-stu-id="d1c2d-276">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample22.js?highlight=1)]

<span data-ttu-id="d1c2d-277">**Criar proxy de cliente para a classe de Hub (sem proxy gerado)**</span><span class="sxs-lookup"><span data-stu-id="d1c2d-277">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample23.cs?highlight=1)]

<span data-ttu-id="d1c2d-278">Se você decorar a classe de Hub com um `HubName` atributo, use o nome exato sem alterar o caso.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-278">If you decorate your Hub class with a `HubName` attribute, use the exact name without changing case.</span></span>

<span data-ttu-id="d1c2d-279">**Classe de Hub no servidor com o atributo HubName**</span><span class="sxs-lookup"><span data-stu-id="d1c2d-279">**Hub class on server with HubName attribute**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample24.cs?highlight=1)]

<span data-ttu-id="d1c2d-280">**Obter uma referência para o proxy de cliente gerado para o Hub**</span><span class="sxs-lookup"><span data-stu-id="d1c2d-280">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample25.js?highlight=1)]

<span data-ttu-id="d1c2d-281">**Criar proxy de cliente para a classe de Hub (sem proxy gerado)**</span><span class="sxs-lookup"><span data-stu-id="d1c2d-281">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample26.cs?highlight=1)]

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="d1c2d-282">Como definir métodos no cliente que o servidor pode chamar</span><span class="sxs-lookup"><span data-stu-id="d1c2d-282">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="d1c2d-283">Para definir um método que o servidor pode chamar de um Hub, adicione um manipulador de eventos ao proxy de Hub usando a `client` Propriedade do proxy gerado ou chame o `on` método se você não estiver usando o proxy gerado.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-283">To define a method that the server can call from a Hub, add an event handler to the Hub proxy by using the `client` property of the generated proxy, or call the `on` method if you aren't using the generated proxy.</span></span> <span data-ttu-id="d1c2d-284">Os parâmetros podem ser objetos complexos.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-284">The parameters can be complex objects.</span></span>

<span data-ttu-id="d1c2d-285">Adicione o manipulador de eventos antes de chamar o `start` método para estabelecer a conexão.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-285">Add the event handler before you call the `start` method to establish the connection.</span></span> <span data-ttu-id="d1c2d-286">(Se você quiser adicionar manipuladores de eventos depois de chamar o `start` método, consulte a observação em [como estabelecer uma conexão](#establishconnection) anteriormente neste documento e use a sintaxe mostrada para definir um método sem usar o proxy gerado.)</span><span class="sxs-lookup"><span data-stu-id="d1c2d-286">(If you want to add event handlers after calling the `start` method, see the note in [How to establish a connection](#establishconnection) earlier in this document, and use the syntax shown for defining a method without using the generated proxy.)</span></span>

<span data-ttu-id="d1c2d-287">A correspondência de nome de método não diferencia maiúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-287">Method name matching is case-insensitive.</span></span> <span data-ttu-id="d1c2d-288">Por exemplo, `Clients.All.addContosoChatMessageToPage` no servidor será executado `AddContosoChatMessageToPage` , `addContosoChatMessageToPage` ou `addcontosochatmessagetopage` no cliente.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-288">For example, `Clients.All.addContosoChatMessageToPage` on the server will execute `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, or `addcontosochatmessagetopage` on the client.</span></span>

<span data-ttu-id="d1c2d-289">**Definir o método no cliente (com o proxy gerado)**</span><span class="sxs-lookup"><span data-stu-id="d1c2d-289">**Define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample27.js?highlight=2)]

<span data-ttu-id="d1c2d-290">**Maneira alternativa de definir o método no cliente (com o proxy gerado)**</span><span class="sxs-lookup"><span data-stu-id="d1c2d-290">**Alternate way to define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample28.js?highlight=1-2)]

<span data-ttu-id="d1c2d-291">**Definir o método no cliente (sem o proxy gerado ou ao adicionar depois de chamar o método Start)**</span><span class="sxs-lookup"><span data-stu-id="d1c2d-291">**Define method on client (without the generated proxy, or when adding after calling the start method)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample29.js?highlight=3)]

<span data-ttu-id="d1c2d-292">**Código de servidor que chama o método de cliente**</span><span class="sxs-lookup"><span data-stu-id="d1c2d-292">**Server code that calls the client method**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample30.cs?highlight=5)]

<span data-ttu-id="d1c2d-293">Os exemplos a seguir incluem um objeto complexo como um parâmetro de método.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-293">The following examples include a complex object as a method parameter.</span></span>

<span data-ttu-id="d1c2d-294">**Definir o método no cliente que usa um objeto complexo (com o proxy gerado)**</span><span class="sxs-lookup"><span data-stu-id="d1c2d-294">**Define method on client that takes a complex object (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample31.js?highlight=2-3)]

<span data-ttu-id="d1c2d-295">**Definir o método no cliente que usa um objeto complexo (sem o proxy gerado)**</span><span class="sxs-lookup"><span data-stu-id="d1c2d-295">**Define method on client that takes a complex object (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample32.js?highlight=3-4)]

<span data-ttu-id="d1c2d-296">**Código de servidor que define o objeto complexo**</span><span class="sxs-lookup"><span data-stu-id="d1c2d-296">**Server code that defines the complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample33.cs?highlight=1)]

<span data-ttu-id="d1c2d-297">**Código de servidor que chama o método de cliente usando um objeto complexo**</span><span class="sxs-lookup"><span data-stu-id="d1c2d-297">**Server code that calls the client method using a complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample34.cs?highlight=3)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="d1c2d-298">Como chamar métodos de servidor do cliente</span><span class="sxs-lookup"><span data-stu-id="d1c2d-298">How to call server methods from the client</span></span>

<span data-ttu-id="d1c2d-299">Para chamar um método de servidor do cliente, use a `server` Propriedade do proxy gerado ou o `invoke` método no proxy do hub se você não estiver usando o proxy gerado.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-299">To call a server method from the client, use the `server` property of the generated proxy or the `invoke` method on the Hub proxy if you aren't using the generated proxy.</span></span> <span data-ttu-id="d1c2d-300">O valor ou os parâmetros de retorno podem ser objetos complexos.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-300">The return value or parameters can be complex objects.</span></span>

<span data-ttu-id="d1c2d-301">Passe uma versão do camel case do nome do método no Hub.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-301">Pass in a camel-case version of the method name on the Hub.</span></span> <span data-ttu-id="d1c2d-302">O signalr faz essa alteração automaticamente para que o código JavaScript possa estar em conformidade com as convenções de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-302">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="d1c2d-303">Os exemplos a seguir mostram como chamar um método de servidor que não tem um valor de retorno e como chamar um método de servidor que tem um valor de retorno.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-303">The following examples show how to call a server method that doesn't have a return value and how to call a server method that does have a return value.</span></span>

<span data-ttu-id="d1c2d-304">**Método de servidor sem atributo HubMethodName**</span><span class="sxs-lookup"><span data-stu-id="d1c2d-304">**Server method with no HubMethodName attribute**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample35.cs?highlight=3)]

<span data-ttu-id="d1c2d-305">**Código de servidor que define o objeto complexo passado em um parâmetro**</span><span class="sxs-lookup"><span data-stu-id="d1c2d-305">**Server code that defines the complex object passed in a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample36.cs)]

<span data-ttu-id="d1c2d-306">**Código de cliente que invoca o método de servidor (com o proxy gerado)**</span><span class="sxs-lookup"><span data-stu-id="d1c2d-306">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample37.js?highlight=1)]

<span data-ttu-id="d1c2d-307">**Código de cliente que invoca o método de servidor (sem o proxy gerado)**</span><span class="sxs-lookup"><span data-stu-id="d1c2d-307">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample38.js?highlight=1)]

<span data-ttu-id="d1c2d-308">Se você tiver decorado o método de Hub com um `HubMethodName` atributo, use esse nome sem alterar o caso.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-308">If you decorated the Hub method with a `HubMethodName` attribute, use that name without changing case.</span></span>

<span data-ttu-id="d1c2d-309">**Método de servidor** com um atributo HubMethodName</span><span class="sxs-lookup"><span data-stu-id="d1c2d-309">**Server method** with a HubMethodName attribute</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample39.cs?highlight=3)]

<span data-ttu-id="d1c2d-310">**Código de cliente que invoca o método de servidor (com o proxy gerado)**</span><span class="sxs-lookup"><span data-stu-id="d1c2d-310">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample40.js?highlight=1)]

<span data-ttu-id="d1c2d-311">**Código de cliente que invoca o método de servidor (sem o proxy gerado)**</span><span class="sxs-lookup"><span data-stu-id="d1c2d-311">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample41.js?highlight=1)]

<span data-ttu-id="d1c2d-312">Os exemplos anteriores mostram como chamar um método de servidor que não tem nenhum valor de retorno.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-312">The preceding examples show how to call a server method that has no return value.</span></span> <span data-ttu-id="d1c2d-313">Os exemplos a seguir mostram como chamar um método de servidor que tem um valor de retorno.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-313">The following examples show how to call a server method that has a return value.</span></span>

<span data-ttu-id="d1c2d-314">**Código do servidor para um método que tem um valor de retorno**</span><span class="sxs-lookup"><span data-stu-id="d1c2d-314">**Server code for a method that has a return value**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample42.cs?highlight=3)]

<span data-ttu-id="d1c2d-315">**A classe Stock usada para o valor de** retorno</span><span class="sxs-lookup"><span data-stu-id="d1c2d-315">**The Stock class used for the** return value</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample43.cs?highlight=1)]

<span data-ttu-id="d1c2d-316">**Código de cliente que invoca o método de servidor (com o proxy gerado)**</span><span class="sxs-lookup"><span data-stu-id="d1c2d-316">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample44.js?highlight=2,4-5)]

<span data-ttu-id="d1c2d-317">**Código de cliente que invoca o método de servidor (sem o proxy gerado)**</span><span class="sxs-lookup"><span data-stu-id="d1c2d-317">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample45.js?highlight=2,4-5)]

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="d1c2d-318">Como tratar eventos de tempo de vida da conexão</span><span class="sxs-lookup"><span data-stu-id="d1c2d-318">How to handle connection lifetime events</span></span>

<span data-ttu-id="d1c2d-319">O signalr fornece os seguintes eventos de tempo de vida de conexão que você pode manipular:</span><span class="sxs-lookup"><span data-stu-id="d1c2d-319">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="d1c2d-320">`starting`: Gerado antes de qualquer dado ser enviado pela conexão.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-320">`starting`: Raised before any data is sent over the connection.</span></span>
- <span data-ttu-id="d1c2d-321">`received`: Ocorre quando os dados são recebidos na conexão.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-321">`received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="d1c2d-322">Fornece os dados recebidos.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-322">Provides the received data.</span></span>
- <span data-ttu-id="d1c2d-323">`connectionSlow`: Ocorre quando o cliente detecta uma conexão lenta ou com frequência.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-323">`connectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="d1c2d-324">`reconnecting`: Ocorre quando o transporte subjacente começa a se reconectar.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-324">`reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="d1c2d-325">`reconnected`: Ocorre quando o transporte subjacente é reconectado.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-325">`reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="d1c2d-326">`stateChanged`: Ocorre quando o estado da conexão é alterado.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-326">`stateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="d1c2d-327">Fornece o estado antigo e o novo estado (conectando, conectado, reconectando ou desconectado).</span><span class="sxs-lookup"><span data-stu-id="d1c2d-327">Provides the old state and the new state (Connecting, Connected, Reconnecting, or Disconnected).</span></span>
- <span data-ttu-id="d1c2d-328">`disconnected`: Ocorre quando a conexão é desconectada.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-328">`disconnected`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="d1c2d-329">Por exemplo, se você quiser exibir mensagens de aviso quando houver problemas de conexão que possam causar atrasos perceptíveis, manipule o `connectionSlow` evento.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-329">For example, if you want to display warning messages when there are connection problems that might cause noticeable delays, handle the `connectionSlow` event.</span></span>

<span data-ttu-id="d1c2d-330">**Manipular o evento connectionSlow (com o proxy gerado)**</span><span class="sxs-lookup"><span data-stu-id="d1c2d-330">**Handle the connectionSlow event (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample46.js?highlight=1)]

<span data-ttu-id="d1c2d-331">**Manipular o evento connectionSlow (sem o proxy gerado)**</span><span class="sxs-lookup"><span data-stu-id="d1c2d-331">**Handle the connectionSlow event (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample47.js?highlight=2)]

<span data-ttu-id="d1c2d-332">Para obter mais informações, consulte [noções básicas e manipulação de eventos de tempo de vida de conexão no signalr](handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="d1c2d-332">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](handling-connection-lifetime-events.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="d1c2d-333">Como tratar erros</span><span class="sxs-lookup"><span data-stu-id="d1c2d-333">How to handle errors</span></span>

<span data-ttu-id="d1c2d-334">O cliente do Signalr JavaScript fornece um `error` evento para o qual você pode adicionar um manipulador.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-334">The SignalR JavaScript client provides an `error` event that you can add a handler for.</span></span> <span data-ttu-id="d1c2d-335">Você também pode usar o método Fail para adicionar um manipulador de erros que resultam de uma invocação de método de servidor.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-335">You can also use the fail method to add a handler for errors that result from a server method invocation.</span></span>

<span data-ttu-id="d1c2d-336">Se você não habilitar explicitamente mensagens de erro detalhadas no servidor, o objeto de exceção que o Signalr retorna após um erro contém informações mínimas sobre o erro.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-336">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="d1c2d-337">Por exemplo, se uma chamada para `newContosoChatMessage` falhar, a mensagem de erro no objeto de erro conterá " `There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.` " não é recomendável enviar mensagens de erro detalhadas aos clientes em produção por motivos de segurança, mas se você quiser habilitar mensagens de erro detalhadas para fins de solução de problemas, use o código a seguir no servidor.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-337">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample48.cs?highlight=2)]

<span data-ttu-id="d1c2d-338">O exemplo a seguir mostra como adicionar um manipulador para o evento de erro.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-338">The following example shows how to add a handler for the error event.</span></span>

<span data-ttu-id="d1c2d-339">**Adicionar um manipulador de erro (com o proxy gerado)**</span><span class="sxs-lookup"><span data-stu-id="d1c2d-339">**Add an error handler (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample49.js?highlight=1)]

<span data-ttu-id="d1c2d-340">**Adicionar um manipulador de erro (sem o proxy gerado)**</span><span class="sxs-lookup"><span data-stu-id="d1c2d-340">**Add an error handler (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample50.js?highlight=2)]

<span data-ttu-id="d1c2d-341">O exemplo a seguir mostra como tratar um erro de uma invocação de método.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-341">The following example shows how to handle an error from a method invocation.</span></span>

<span data-ttu-id="d1c2d-342">**Manipular um erro de uma invocação de método (com o proxy gerado)**</span><span class="sxs-lookup"><span data-stu-id="d1c2d-342">**Handle an error from a method invocation (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample51.js?highlight=2)]

<span data-ttu-id="d1c2d-343">**Manipular um erro de uma invocação de método (sem o proxy gerado)**</span><span class="sxs-lookup"><span data-stu-id="d1c2d-343">**Handle an error from a method invocation (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample52.js?highlight=2)]

<span data-ttu-id="d1c2d-344">Se uma invocação de método falhar, o `error` evento também será gerado, de modo que seu código no `error` manipulador de métodos e no `.fail` retorno de chamada do método seria executado.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-344">If a method invocation fails, the `error` event is also raised, so your code in the `error` method handler and in the `.fail` method callback would execute.</span></span>

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="d1c2d-345">Como habilitar o log do lado do cliente</span><span class="sxs-lookup"><span data-stu-id="d1c2d-345">How to enable client-side logging</span></span>

<span data-ttu-id="d1c2d-346">Para habilitar o log do lado do cliente em uma conexão, defina a `logging` propriedade no objeto de conexão antes de chamar o `start` método para estabelecer a conexão.</span><span class="sxs-lookup"><span data-stu-id="d1c2d-346">To enable client-side logging on a connection, set the `logging` property on the connection object before you call the `start` method to establish the connection.</span></span>

<span data-ttu-id="d1c2d-347">**Habilitar registro em log (com o proxy gerado)**</span><span class="sxs-lookup"><span data-stu-id="d1c2d-347">**Enable logging (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample53.js?highlight=1)]

<span data-ttu-id="d1c2d-348">**Habilitar o registro em log (sem o proxy gerado)**</span><span class="sxs-lookup"><span data-stu-id="d1c2d-348">**Enable logging (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample54.js?highlight=2)]

<span data-ttu-id="d1c2d-349">Para ver os logs, abra as ferramentas de desenvolvedor do navegador e vá para a guia Console. Para obter um tutorial que mostra instruções passo a passo e capturas de tela que mostram como fazer isso, consulte [transmissão de servidor com o ASP.net signalr-habilitar registro em log](../getting-started/tutorial-server-broadcast-with-signalr.md#enable-logging).</span><span class="sxs-lookup"><span data-stu-id="d1c2d-349">To see the logs, open your browser's developer tools and go to the Console tab. For a tutorial that shows step-by-step instructions and screen shots that show how to do this, see [Server Broadcast with ASP.NET Signalr - Enable Logging](../getting-started/tutorial-server-broadcast-with-signalr.md#enable-logging).</span></span>
