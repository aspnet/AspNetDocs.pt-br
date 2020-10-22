---
uid: signalr/overview/performance/signalr-performance
title: Desempenho do signalr | Microsoft Docs
author: bradygaster
description: Desempenho do SignalR
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 3751f5e7-59db-4be0-a290-50abc24e5c84
msc.legacyurl: /signalr/overview/performance/signalr-performance
msc.type: authoredcontent
ms.openlocfilehash: b8a44f4c924c94cdfff1ce7630539b45fe269bbf
ms.sourcegitcommit: c62ec20b453cee3249eb894ecd75013b57d078f0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/21/2020
ms.locfileid: "92345279"
---
# <a name="signalr-performance"></a><span data-ttu-id="5db28-103">Desempenho do SignalR</span><span class="sxs-lookup"><span data-stu-id="5db28-103">SignalR Performance</span></span>

<span data-ttu-id="5db28-104">por [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="5db28-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="5db28-105">Este tópico descreve como criar, medir e melhorar o desempenho em um aplicativo Signalr.</span><span class="sxs-lookup"><span data-stu-id="5db28-105">This topic describes how to design for, measure, and improve performance in a SignalR application.</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="5db28-106">Versões de software usadas neste tópico</span><span class="sxs-lookup"><span data-stu-id="5db28-106">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="5db28-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="5db28-107">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="5db28-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="5db28-108">.NET 4.5</span></span>
> - <span data-ttu-id="5db28-109">Sinalização versão 2</span><span class="sxs-lookup"><span data-stu-id="5db28-109">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="5db28-110">Versões anteriores deste tópico</span><span class="sxs-lookup"><span data-stu-id="5db28-110">Previous versions of this topic</span></span>
>
> <span data-ttu-id="5db28-111">Para obter informações sobre versões anteriores do Signalr, confira [versões mais antigas do signalr](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="5db28-111">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="5db28-112">Perguntas e comentários</span><span class="sxs-lookup"><span data-stu-id="5db28-112">Questions and comments</span></span>
>
> <span data-ttu-id="5db28-113">Deixe comentários sobre como você gostou deste tutorial e o que poderíamos melhorar nos comentários na parte inferior da página.</span><span class="sxs-lookup"><span data-stu-id="5db28-113">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="5db28-114">Se você tiver dúvidas que não estão diretamente relacionadas ao tutorial, poderá lançá-las no fórum do [signalr ASP.net](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [stackoverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="5db28-114">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

<span data-ttu-id="5db28-115">Para obter uma apresentação recente sobre o desempenho e o dimensionamento do Signalr, consulte [dimensionando a Web em tempo real com o signalr ASP.net](https://channel9.msdn.com/Events/Build/2013/3-502).</span><span class="sxs-lookup"><span data-stu-id="5db28-115">For a recent presentation on SignalR performance and scaling, see [Scaling the Real-time Web with ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).</span></span>

<span data-ttu-id="5db28-116">Este tópico contém as seguintes seções:</span><span class="sxs-lookup"><span data-stu-id="5db28-116">This topic contains the following sections:</span></span>

- [<span data-ttu-id="5db28-117">Considerações sobre o design</span><span class="sxs-lookup"><span data-stu-id="5db28-117">Design considerations</span></span>](#design)
- [<span data-ttu-id="5db28-118">Ajustando o servidor de sinalização para desempenho</span><span class="sxs-lookup"><span data-stu-id="5db28-118">Tuning your SignalR server for performance</span></span>](#tuning)
- [<span data-ttu-id="5db28-119">Solucionando problemas de desempenho</span><span class="sxs-lookup"><span data-stu-id="5db28-119">Troubleshooting performance issues</span></span>](#troubleshooting)
- [<span data-ttu-id="5db28-120">Usando contadores de desempenho do Signalr</span><span class="sxs-lookup"><span data-stu-id="5db28-120">Using SignalR performance counters</span></span>](#perfcounters)
- [<span data-ttu-id="5db28-121">Usando outros contadores de desempenho</span><span class="sxs-lookup"><span data-stu-id="5db28-121">Using other performance counters</span></span>](#othercounters)
- [<span data-ttu-id="5db28-122">Outros recursos</span><span class="sxs-lookup"><span data-stu-id="5db28-122">Other resources</span></span>](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a><span data-ttu-id="5db28-123">Considerações sobre o design</span><span class="sxs-lookup"><span data-stu-id="5db28-123">Design considerations</span></span>

<span data-ttu-id="5db28-124">Esta seção descreve os padrões que podem ser implementados durante o design de um aplicativo Signalr, para garantir que o desempenho não seja prejudicado pela geração de tráfego de rede desnecessário.</span><span class="sxs-lookup"><span data-stu-id="5db28-124">This section describes patterns that can be implemented during the design of a SignalR application, to ensure that performance is not being hindered by generating unnecessary network traffic.</span></span>

### <a name="throttling-message-frequency"></a><span data-ttu-id="5db28-125">Frequência da mensagem de limitação</span><span class="sxs-lookup"><span data-stu-id="5db28-125">Throttling message frequency</span></span>

<span data-ttu-id="5db28-126">Mesmo em um aplicativo que envia mensagens a uma alta frequência (como um aplicativo de jogos em tempo real), a maioria dos aplicativos não precisa enviar mais de algumas mensagens por um segundo.</span><span class="sxs-lookup"><span data-stu-id="5db28-126">Even in an application that sends out messages at a high frequency (such as a realtime gaming application), most applications don't need to send more than a few messages a second.</span></span> <span data-ttu-id="5db28-127">Para reduzir a quantidade de tráfego que cada cliente gera, um loop de mensagem pode ser implementado para que as filas e envie mensagens não com mais frequência do que uma taxa fixa (ou seja, até um determinado número de mensagens serão enviadas a cada segundo, se houver mensagens nesse intervalo de tempo a serem enviadas).</span><span class="sxs-lookup"><span data-stu-id="5db28-127">To reduce the amount of traffic that each client generates, a message loop can be implemented that queues and sends out messages no more frequently than a fixed rate (that is, up to a certain number of messages will be sent every second, if there are messages in that time interval to be sent).</span></span> <span data-ttu-id="5db28-128">Para um aplicativo de exemplo que limita as mensagens a uma determinada taxa (do cliente e do servidor), consulte [tempo real de alta frequência com signalr](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="5db28-128">For a sample application that throttles messages to a certain rate (from both client and server), see [High-Frequency Realtime with SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span></span>

### <a name="reducing-message-size"></a><span data-ttu-id="5db28-129">Reduzindo o tamanho da mensagem</span><span class="sxs-lookup"><span data-stu-id="5db28-129">Reducing message size</span></span>

<span data-ttu-id="5db28-130">Você pode reduzir o tamanho de uma mensagem de sinalização reduzindo o tamanho de seus objetos serializados.</span><span class="sxs-lookup"><span data-stu-id="5db28-130">You can reduce the size of a SignalR message by reducing the size of your serialized objects.</span></span> <span data-ttu-id="5db28-131">No código do servidor, se você estiver enviando um objeto que contém propriedades que não precisam ser transmitidas, impeça que essas propriedades sejam serializadas usando o `JsonIgnore` atributo.</span><span class="sxs-lookup"><span data-stu-id="5db28-131">In server code, if you're sending an object that contains properties that don't need to be transmitted, prevent those properties from being serialized by using the `JsonIgnore` attribute.</span></span> <span data-ttu-id="5db28-132">Os nomes das propriedades também são armazenados na mensagem; os nomes das propriedades podem ser reduzidos usando o `JsonProperty` atributo.</span><span class="sxs-lookup"><span data-stu-id="5db28-132">The names of properties are also stored in the message; the names of properties can be shortened using the `JsonProperty` attribute.</span></span> <span data-ttu-id="5db28-133">O exemplo de código a seguir demonstra como excluir uma propriedade de ser enviada ao cliente e como reduzir os nomes de propriedade:</span><span class="sxs-lookup"><span data-stu-id="5db28-133">The following code sample demonstrates how to exclude a property from being sent to the client, and how to shorten property names:</span></span>

<span data-ttu-id="5db28-134">**Código do servidor .NET que demonstra o atributo JsonIgnore para excluir dados de serem enviados ao cliente e o atributo Jsonproperty para reduzir o tamanho da mensagem**</span><span class="sxs-lookup"><span data-stu-id="5db28-134">**.NET server code that demonstrates the JsonIgnore attribute to exclude data from being sent to the client, and the JsonProperty attribute to reduce message size**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

<span data-ttu-id="5db28-135">Para manter a legibilidade/facilidade de manutenção no código do cliente, os nomes de propriedade abreviados podem ser remapeados para nomes amigáveis para os seres humanos depois que a mensagem é recebida.</span><span class="sxs-lookup"><span data-stu-id="5db28-135">In order to retain readability/ maintainability in the client code, the abbreviated property names can be remapped to human-friendly names after the message is received.</span></span> <span data-ttu-id="5db28-136">O exemplo de código a seguir demonstra uma possível maneira de remapear nomes reduzidos para mais longos, definindo um contrato de mensagem (mapeamento) e usando a `reMap` função para aplicar o contrato à classe de mensagem otimizada:</span><span class="sxs-lookup"><span data-stu-id="5db28-136">The following code sample demonstrates one possible way of remapping shortened names to longer ones, by defining a message contract (mapping), and using the `reMap` function to apply the contract to the optimized message class:</span></span>

<span data-ttu-id="5db28-137">**Código JavaScript do lado do cliente que remapeia nomes de propriedade reduzidos a nomes legíveis para pessoas**</span><span class="sxs-lookup"><span data-stu-id="5db28-137">**Client-side JavaScript code that remaps shortened property names to human-readable names**</span></span>

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

<span data-ttu-id="5db28-138">Os nomes também podem ser reduzidos em mensagens do cliente para o servidor, usando o mesmo método.</span><span class="sxs-lookup"><span data-stu-id="5db28-138">Names can be shortened in messages from the client to the server as well, using the same method.</span></span>

<span data-ttu-id="5db28-139">Reduzir a superfície de memória (ou seja, a quantidade de memória usada para a mensagem) do objeto de mensagem também pode melhorar o desempenho.</span><span class="sxs-lookup"><span data-stu-id="5db28-139">Reducing the memory footprint (that is, the amount of memory used for the message) of the message object can also improve performance.</span></span> <span data-ttu-id="5db28-140">Por exemplo, se o intervalo completo de um `int` não for necessário, um `short` ou `byte` poderá ser usado em vez disso.</span><span class="sxs-lookup"><span data-stu-id="5db28-140">For example, if the full range of an `int` is not needed, a `short` or `byte` can be used instead.</span></span>

<span data-ttu-id="5db28-141">Como as mensagens são armazenadas no barramento de mensagem na memória do servidor, a redução do tamanho das mensagens também pode resolver problemas de memória do servidor.</span><span class="sxs-lookup"><span data-stu-id="5db28-141">Since messages are stored in the message bus in server memory, reducing the size of messages can also address server memory issues.</span></span>

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a><span data-ttu-id="5db28-142">Ajustando o servidor de sinalização para desempenho</span><span class="sxs-lookup"><span data-stu-id="5db28-142">Tuning your SignalR server for performance</span></span>

<span data-ttu-id="5db28-143">As definições de configuração a seguir podem ser usadas para ajustar o servidor para melhorar o desempenho em um aplicativo Signalr.</span><span class="sxs-lookup"><span data-stu-id="5db28-143">The following configuration settings can be used to tune your server for better performance in a SignalR application.</span></span> <span data-ttu-id="5db28-144">Para obter informações gerais sobre como melhorar o desempenho em um aplicativo ASP.NET, consulte [melhorando o desempenho do ASP.net](https://msdn.microsoft.com/library/ff647787.aspx).</span><span class="sxs-lookup"><span data-stu-id="5db28-144">For general information on how to improve performance in an ASP.NET application, see [Improving ASP.NET Performance](https://msdn.microsoft.com/library/ff647787.aspx).</span></span>

<span data-ttu-id="5db28-145">**Definições de configuração do signalr**</span><span class="sxs-lookup"><span data-stu-id="5db28-145">**SignalR configuration settings**</span></span>

- <span data-ttu-id="5db28-146">**DefaultMessageBufferSize**: por padrão, o signalr retém 1000 mensagens na memória por Hub por conexão.</span><span class="sxs-lookup"><span data-stu-id="5db28-146">**DefaultMessageBufferSize**: By default, SignalR retains 1000 messages in memory per hub per connection.</span></span> <span data-ttu-id="5db28-147">Se mensagens grandes estiverem sendo usadas, isso poderá criar problemas de memória que podem ser aliviados reduzindo esse valor.</span><span class="sxs-lookup"><span data-stu-id="5db28-147">If large messages are being used, this may create memory issues which can be alleviated by reducing this value.</span></span> <span data-ttu-id="5db28-148">Essa configuração pode ser definida no `Application_Start` manipulador de eventos em um aplicativo ASP.net ou no `Configuration` método de uma classe de inicialização OWIN em um aplicativo auto-hospedado.</span><span class="sxs-lookup"><span data-stu-id="5db28-148">This setting can be set in the `Application_Start` event handler in an ASP.NET application, or in the `Configuration` method of an OWIN startup class in a self-hosted application.</span></span> <span data-ttu-id="5db28-149">O exemplo a seguir demonstra como reduzir esse valor para reduzir a superfície de memória do seu aplicativo a fim de reduzir a quantidade de memória do servidor usada:</span><span class="sxs-lookup"><span data-stu-id="5db28-149">The following sample demonstrates how to reduce this value in order to reduce the memory footprint of your application in order to reduce the amount of server memory used:</span></span>

    <span data-ttu-id="5db28-150">**Código do servidor .NET em Startup.cs para diminuir o tamanho do buffer de mensagens padrão**</span><span class="sxs-lookup"><span data-stu-id="5db28-150">**.NET server code in Startup.cs for decreasing default message buffer size**</span></span>

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

<span data-ttu-id="5db28-151">**Definições de configuração do IIS**</span><span class="sxs-lookup"><span data-stu-id="5db28-151">**IIS configuration settings**</span></span>

- <span data-ttu-id="5db28-152">**Máximo de solicitações simultâneas por aplicativo**: aumentar o número de solicitações simultâneas do IIS aumentará os recursos do servidor disponíveis para atender a solicitações.</span><span class="sxs-lookup"><span data-stu-id="5db28-152">**Max concurrent requests per application**: Increasing the number of concurrent IIS requests will increase server resources available for serving requests.</span></span> <span data-ttu-id="5db28-153">O valor padrão é 5000; para aumentar essa configuração, execute os seguintes comandos em um prompt de comandos com privilégios elevados:</span><span class="sxs-lookup"><span data-stu-id="5db28-153">The default value is 5000; to increase this setting, execute the following commands in an elevated command prompt:</span></span>

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]
- <span data-ttu-id="5db28-154">**ApplicationPool QueueLength**: Este é o número máximo de solicitações que Http.sys filas para o pool de aplicativos.</span><span class="sxs-lookup"><span data-stu-id="5db28-154">**ApplicationPool QueueLength**: This is the maximum number of requests that Http.sys queues for the application pool.</span></span> <span data-ttu-id="5db28-155">Quando a fila estiver cheia, novas solicitações receberão uma resposta 503 "Serviço indisponível".</span><span class="sxs-lookup"><span data-stu-id="5db28-155">When the queue is full, new requests receive a 503 "Service Unavailable" response.</span></span> <span data-ttu-id="5db28-156">O valor padrão é 1000.</span><span class="sxs-lookup"><span data-stu-id="5db28-156">The default value is 1000.</span></span>

    <span data-ttu-id="5db28-157">Reduzir o comprimento da fila para o processo de trabalho no pool de aplicativos que hospeda seu aplicativo irá conservar os recursos de memória.</span><span class="sxs-lookup"><span data-stu-id="5db28-157">Shortening the queue length for the worker process in the application pool hosting your application will conserve memory resources.</span></span> <span data-ttu-id="5db28-158">Para obter mais informações, consulte [Gerenciando, ajustando e configurando pools de aplicativos](https://technet.microsoft.com/library/cc745955.aspx).</span><span class="sxs-lookup"><span data-stu-id="5db28-158">For more information, see [Managing, Tuning, and Configuring Application Pools](https://technet.microsoft.com/library/cc745955.aspx).</span></span>

<span data-ttu-id="5db28-159">**Definições de configuração do ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="5db28-159">**ASP.NET configuration settings**</span></span>

<span data-ttu-id="5db28-160">Esta seção inclui definições de configuração que podem ser definidas no `aspnet.config` arquivo.</span><span class="sxs-lookup"><span data-stu-id="5db28-160">This section includes configuration settings that can be set in the `aspnet.config` file.</span></span> <span data-ttu-id="5db28-161">Esse arquivo é encontrado em um dos dois locais, dependendo da plataforma:</span><span class="sxs-lookup"><span data-stu-id="5db28-161">This file is found in one of two locations, depending on platform:</span></span>

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

<span data-ttu-id="5db28-162">As configurações de ASP.NET que podem melhorar o desempenho do Signalr incluem o seguinte:</span><span class="sxs-lookup"><span data-stu-id="5db28-162">ASP.NET settings that may improve SignalR performance include the following:</span></span>

- <span data-ttu-id="5db28-163">**Máximo de solicitações simultâneas por CPU**: aumentar essa configuração pode aliviar afunilamentos de desempenho.</span><span class="sxs-lookup"><span data-stu-id="5db28-163">**Maximum concurrent requests per CPU**: Increasing this setting may alleviate performance bottlenecks.</span></span> <span data-ttu-id="5db28-164">Para aumentar essa configuração, adicione o seguinte parâmetro de configuração ao `aspnet.config` arquivo:</span><span class="sxs-lookup"><span data-stu-id="5db28-164">To increase this setting, add the following configuration setting to the `aspnet.config` file:</span></span>

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- <span data-ttu-id="5db28-165">**Limite da fila de solicitações**: quando o número total de conexões exceder a `maxConcurrentRequestsPerCPU` configuração, o ASP.net começará a limitar as solicitações usando uma fila.</span><span class="sxs-lookup"><span data-stu-id="5db28-165">**Request Queue Limit**: When the total number of connections exceeds the `maxConcurrentRequestsPerCPU` setting, ASP.NET will start throttling requests using a queue.</span></span> <span data-ttu-id="5db28-166">Para aumentar o tamanho da fila, você pode aumentar a `requestQueueLimit` configuração.</span><span class="sxs-lookup"><span data-stu-id="5db28-166">To increase the size of the queue, you can increase the `requestQueueLimit` setting.</span></span> <span data-ttu-id="5db28-167">Para fazer isso, adicione o seguinte parâmetro de configuração ao `processModel` nó em `config/machine.config` (em vez de `aspnet.config` ):</span><span class="sxs-lookup"><span data-stu-id="5db28-167">To do this, add the following configuration setting to the `processModel` node in `config/machine.config` (rather than `aspnet.config`):</span></span>

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a><span data-ttu-id="5db28-168">Solucionando problemas de desempenho</span><span class="sxs-lookup"><span data-stu-id="5db28-168">Troubleshooting performance issues</span></span>

<span data-ttu-id="5db28-169">Esta seção descreve maneiras de encontrar afunilamentos de desempenho em seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="5db28-169">This section describes ways to find performance bottlenecks in your application.</span></span>

### <a name="verifying-that-websocket-is-being-used"></a><span data-ttu-id="5db28-170">Verificando se o WebSocket está sendo usado</span><span class="sxs-lookup"><span data-stu-id="5db28-170">Verifying that WebSocket is being used</span></span>

<span data-ttu-id="5db28-171">Embora o Signalr possa usar uma variedade de transportes para comunicação entre o cliente e o servidor, o WebSocket oferece uma vantagem de desempenho significativa e deve ser usado se o cliente e o servidor oferecerem suporte a ele.</span><span class="sxs-lookup"><span data-stu-id="5db28-171">While SignalR can use a variety of transports for communication between client and server, WebSocket offers a significant performance advantage, and should be used if the client and server support it.</span></span> <span data-ttu-id="5db28-172">Para determinar se o cliente e o servidor atendem aos requisitos do WebSocket, consulte [transportes e fallbacks](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="5db28-172">To determine if your client and server meet the requirements for WebSocket, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span> <span data-ttu-id="5db28-173">Para determinar qual transporte está sendo usado em seu aplicativo, você pode usar as ferramentas de desenvolvedor do navegador e examinar os logs para ver qual transporte está sendo usado para a conexão.</span><span class="sxs-lookup"><span data-stu-id="5db28-173">To determine what transport is being used in your application, you can use the browser developer tools, and examine the logs to see what transport is being used for the connection.</span></span> <span data-ttu-id="5db28-174">Para obter informações sobre como usar as ferramentas de desenvolvimento do navegador no Internet Explorer e no Chrome, consulte [transportes e fallbacks](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="5db28-174">For information on using the browser development tools in Internet Explorer and Chrome, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a><span data-ttu-id="5db28-175">Usando contadores de desempenho do Signalr</span><span class="sxs-lookup"><span data-stu-id="5db28-175">Using SignalR performance counters</span></span>

<span data-ttu-id="5db28-176">Esta seção descreve como habilitar e usar contadores de desempenho do Signalr, encontrados no `Microsoft.AspNet.SignalR.Utils` pacote.</span><span class="sxs-lookup"><span data-stu-id="5db28-176">This section describes how to enable and use SignalR performance counters, found in the `Microsoft.AspNet.SignalR.Utils` package.</span></span>

### <a name="installing-signalrexe"></a><span data-ttu-id="5db28-177">Instalando o signalr.exe</span><span class="sxs-lookup"><span data-stu-id="5db28-177">Installing signalr.exe</span></span>

<span data-ttu-id="5db28-178">Os contadores de desempenho podem ser adicionados ao servidor usando um utilitário chamado SignalR.exe.</span><span class="sxs-lookup"><span data-stu-id="5db28-178">Performance counters can be added to the server using a utility called SignalR.exe.</span></span> <span data-ttu-id="5db28-179">Para instalar esse utilitário, siga estas etapas:</span><span class="sxs-lookup"><span data-stu-id="5db28-179">To install this utility, follow these steps:</span></span>

1. <span data-ttu-id="5db28-180">No Visual Studio, selecione **ferramentas**  >  **Gerenciador de pacotes NuGet**  >  **gerenciar pacotes NuGet para solução**</span><span class="sxs-lookup"><span data-stu-id="5db28-180">In Visual Studio, select **Tools** > **NuGet Package Manager** > **Manage NuGet Packages for Solution**</span></span>
2. <span data-ttu-id="5db28-181">Procure **signalr. utils**e selecione instalar.</span><span class="sxs-lookup"><span data-stu-id="5db28-181">Search for **signalr.utils**, and select Install.</span></span>

    ![](signalr-performance/_static/image1.png)
3. <span data-ttu-id="5db28-182">Aceite o contrato de licença para instalar o pacote.</span><span class="sxs-lookup"><span data-stu-id="5db28-182">Accept the license agreement to install the package.</span></span>
4. <span data-ttu-id="5db28-183">SignalR.exe será instalado no `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools` .</span><span class="sxs-lookup"><span data-stu-id="5db28-183">SignalR.exe will be installed to `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span></span>

### <a name="installing-performance-counters-with-signalrexe"></a><span data-ttu-id="5db28-184">Instalando contadores de desempenho com SignalR.exe</span><span class="sxs-lookup"><span data-stu-id="5db28-184">Installing performance counters with SignalR.exe</span></span>

<span data-ttu-id="5db28-185">Para instalar contadores de desempenho do Signalr, execute SignalR.exe em um prompt de comando com privilégios elevados com o seguinte parâmetro:</span><span class="sxs-lookup"><span data-stu-id="5db28-185">To install SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

<span data-ttu-id="5db28-186">Para remover contadores de desempenho do Signalr, execute SignalR.exe em um prompt de comando com privilégios elevados com o seguinte parâmetro:</span><span class="sxs-lookup"><span data-stu-id="5db28-186">To remove SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a><span data-ttu-id="5db28-187">Contadores de desempenho do signalr</span><span class="sxs-lookup"><span data-stu-id="5db28-187">SignalR Performance counters</span></span>

<span data-ttu-id="5db28-188">O pacote de utilitários instala os seguintes contadores de desempenho.</span><span class="sxs-lookup"><span data-stu-id="5db28-188">The utilities package installs the following performance counters.</span></span> <span data-ttu-id="5db28-189">Os contadores "total" medem o número de eventos desde a última reinicialização do pool de aplicativos ou do servidor.</span><span class="sxs-lookup"><span data-stu-id="5db28-189">The "Total" counters measure the number of events since the last application pool or server restart.</span></span>

<span data-ttu-id="5db28-190">**Métricas de conexão**</span><span class="sxs-lookup"><span data-stu-id="5db28-190">**Connection metrics**</span></span>

<span data-ttu-id="5db28-191">As métricas a seguir medem os eventos de tempo de vida da conexão que ocorrem.</span><span class="sxs-lookup"><span data-stu-id="5db28-191">The following metrics measure the connection lifetime events that occur.</span></span> <span data-ttu-id="5db28-192">Para obter mais informações, consulte [noções básicas e manipulação de eventos de tempo de vida da conexão](../guide-to-the-api/handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="5db28-192">For more information, see [Understanding and Handling Connection Lifetime Events](../guide-to-the-api/handling-connection-lifetime-events.md).</span></span>

- <span data-ttu-id="5db28-193">**Conexões conectadas**</span><span class="sxs-lookup"><span data-stu-id="5db28-193">**Connections Connected**</span></span>
- <span data-ttu-id="5db28-194">**Conexões reconectadas**</span><span class="sxs-lookup"><span data-stu-id="5db28-194">**Connections Reconnected**</span></span>
- <span data-ttu-id="5db28-195">**Conexões desconectadas**</span><span class="sxs-lookup"><span data-stu-id="5db28-195">**Connections Disconnected**</span></span>
- <span data-ttu-id="5db28-196">**Conexões atuais**</span><span class="sxs-lookup"><span data-stu-id="5db28-196">**Connections Current**</span></span>

<span data-ttu-id="5db28-197">**Métricas de mensagens**</span><span class="sxs-lookup"><span data-stu-id="5db28-197">**Message metrics**</span></span>

<span data-ttu-id="5db28-198">As métricas a seguir medem o tráfego de mensagens gerado pelo Signalr.</span><span class="sxs-lookup"><span data-stu-id="5db28-198">The following metrics measure the message traffic generated by SignalR.</span></span>

- <span data-ttu-id="5db28-199">**Total de mensagens de conexão recebidas**</span><span class="sxs-lookup"><span data-stu-id="5db28-199">**Connection Messages Received Total**</span></span>
- <span data-ttu-id="5db28-200">**Total de mensagens de conexão enviadas**</span><span class="sxs-lookup"><span data-stu-id="5db28-200">**Connection Messages Sent Total**</span></span>
- <span data-ttu-id="5db28-201">**Mensagens de conexão recebidas/s**</span><span class="sxs-lookup"><span data-stu-id="5db28-201">**Connection Messages Received/Sec**</span></span>
- <span data-ttu-id="5db28-202">**Mensagens de conexão enviadas/s**</span><span class="sxs-lookup"><span data-stu-id="5db28-202">**Connection Messages Sent/Sec**</span></span>

<span data-ttu-id="5db28-203">**Métricas do barramento de mensagem**</span><span class="sxs-lookup"><span data-stu-id="5db28-203">**Message bus metrics**</span></span>

<span data-ttu-id="5db28-204">As métricas a seguir medem o tráfego por meio do barramento de mensagem do Signalr interno, a fila na qual todas as mensagens de entrada e saída do Signalr são colocadas.</span><span class="sxs-lookup"><span data-stu-id="5db28-204">The following metrics measure traffic through the internal SignalR message bus, the queue in which all incoming and outgoing SignalR messages are placed.</span></span> <span data-ttu-id="5db28-205">Uma mensagem é **publicada** quando enviada ou difundida.</span><span class="sxs-lookup"><span data-stu-id="5db28-205">A message is **Published** when it is sent or broadcast.</span></span> <span data-ttu-id="5db28-206">Um **assinante** neste contexto é uma assinatura no barramento de mensagem; Isso deve ser igual ao número de clientes mais o próprio servidor.</span><span class="sxs-lookup"><span data-stu-id="5db28-206">A **Subscriber** in this context is a subscription on the message bus; this should equal the number of clients plus the server itself.</span></span> <span data-ttu-id="5db28-207">Um **trabalho alocado** é um componente que envia dados para conexões ativas; um **trabalho ocupado** é aquele que está enviando ativamente uma mensagem.</span><span class="sxs-lookup"><span data-stu-id="5db28-207">An **Allocated Worker** is a component that sends data to active connections; a **Busy Worker** is one that is actively sending a message.</span></span>

- <span data-ttu-id="5db28-208">**Total de mensagens de barramento de mensagem recebidas**</span><span class="sxs-lookup"><span data-stu-id="5db28-208">**Message Bus Messages Received Total**</span></span>
- <span data-ttu-id="5db28-209">**Mensagens de barramento de mensagem recebidas/s**</span><span class="sxs-lookup"><span data-stu-id="5db28-209">**Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="5db28-210">**Total de mensagens do barramento de mensagem publicadas**</span><span class="sxs-lookup"><span data-stu-id="5db28-210">**Message Bus Messages Published Total**</span></span>
- <span data-ttu-id="5db28-211">**Mensagens do barramento de mensagem publicadas/s**</span><span class="sxs-lookup"><span data-stu-id="5db28-211">**Message Bus Messages Published/Sec**</span></span>
- <span data-ttu-id="5db28-212">**Assinantes atuais do barramento de mensagem**</span><span class="sxs-lookup"><span data-stu-id="5db28-212">**Message Bus Subscribers Current**</span></span>
- <span data-ttu-id="5db28-213">**Total de assinantes do barramento de mensagem**</span><span class="sxs-lookup"><span data-stu-id="5db28-213">**Message Bus Subscribers Total**</span></span>
- <span data-ttu-id="5db28-214">**Assinantes do barramento de mensagem/s**</span><span class="sxs-lookup"><span data-stu-id="5db28-214">**Message Bus Subscribers/Sec**</span></span>
- <span data-ttu-id="5db28-215">**Trabalhadores alocados do barramento de mensagem**</span><span class="sxs-lookup"><span data-stu-id="5db28-215">**Message Bus Allocated Workers**</span></span>
- <span data-ttu-id="5db28-216">**Trabalhadores ocupados do barramento de mensagem**</span><span class="sxs-lookup"><span data-stu-id="5db28-216">**Message Bus Busy Workers**</span></span>
- <span data-ttu-id="5db28-217">**Tópicos do barramento de mensagem atual**</span><span class="sxs-lookup"><span data-stu-id="5db28-217">**Message Bus Topics Current**</span></span>

<span data-ttu-id="5db28-218">**Métricas de erro**</span><span class="sxs-lookup"><span data-stu-id="5db28-218">**Error metrics**</span></span>

<span data-ttu-id="5db28-219">As métricas a seguir medem os erros gerados pelo tráfego de mensagens do Signalr.</span><span class="sxs-lookup"><span data-stu-id="5db28-219">The following metrics measure errors generated by SignalR message traffic.</span></span> <span data-ttu-id="5db28-220">Os erros de **resolução de Hub** ocorrem quando um método de Hub ou Hub não pode ser resolvido.</span><span class="sxs-lookup"><span data-stu-id="5db28-220">**Hub Resolution** errors occur when a hub or hub method cannot be resolved.</span></span> <span data-ttu-id="5db28-221">Erros de **invocação de Hub** são exceções geradas ao invocar um método de Hub.</span><span class="sxs-lookup"><span data-stu-id="5db28-221">**Hub Invocation** errors are exceptions thrown while invoking a hub method.</span></span> <span data-ttu-id="5db28-222">Os erros de **transporte** são erros de conexão lançados durante uma solicitação ou resposta http.</span><span class="sxs-lookup"><span data-stu-id="5db28-222">**Transport** errors are connection errors thrown during an HTTP request or response.</span></span>

- <span data-ttu-id="5db28-223">**Erros: todos os totais**</span><span class="sxs-lookup"><span data-stu-id="5db28-223">**Errors: All Total**</span></span>
- <span data-ttu-id="5db28-224">**Erros: todos/s**</span><span class="sxs-lookup"><span data-stu-id="5db28-224">**Errors: All/Sec**</span></span>
- <span data-ttu-id="5db28-225">**Erros: resolução do Hub total**</span><span class="sxs-lookup"><span data-stu-id="5db28-225">**Errors: Hub Resolution Total**</span></span>
- <span data-ttu-id="5db28-226">**Erros: resolução do Hub/s**</span><span class="sxs-lookup"><span data-stu-id="5db28-226">**Errors: Hub Resolution/Sec**</span></span>
- <span data-ttu-id="5db28-227">**Erros: total de invocação de Hub**</span><span class="sxs-lookup"><span data-stu-id="5db28-227">**Errors: Hub Invocation Total**</span></span>
- <span data-ttu-id="5db28-228">**Erros: invocação de Hub/s**</span><span class="sxs-lookup"><span data-stu-id="5db28-228">**Errors: Hub Invocation/Sec**</span></span>
- <span data-ttu-id="5db28-229">**Erros: transporte total**</span><span class="sxs-lookup"><span data-stu-id="5db28-229">**Errors: Transport Total**</span></span>
- <span data-ttu-id="5db28-230">**Erros: transporte/s**</span><span class="sxs-lookup"><span data-stu-id="5db28-230">**Errors: Transport/Sec**</span></span>

<a id="scaleout_metrics"></a>

<span data-ttu-id="5db28-231">**Métricas de dimensionamento**</span><span class="sxs-lookup"><span data-stu-id="5db28-231">**Scaleout metrics**</span></span>

<span data-ttu-id="5db28-232">As métricas a seguir medem o tráfego e os erros gerados pelo provedor de scale out.</span><span class="sxs-lookup"><span data-stu-id="5db28-232">The following metrics measure traffic and errors generated by the scaleout provider.</span></span> <span data-ttu-id="5db28-233">Um **fluxo** neste contexto é uma unidade de escala usada pelo provedor de scale out; Esta é uma tabela se SQL Server for usado, um tópico se o barramento de serviço for usado e uma assinatura se Redis for usado.</span><span class="sxs-lookup"><span data-stu-id="5db28-233">A **Stream** in this context is a scale unit used by the scaleout provider; this is a table if SQL Server is used, a Topic if Service Bus is used, and a Subscription if Redis is used.</span></span> <span data-ttu-id="5db28-234">Cada fluxo garante operações de leitura e gravação ordenadas; um único fluxo é um gargalo de escala potencial, portanto, o número de fluxos pode ser aumentado para ajudar a reduzir esse afunilamento.</span><span class="sxs-lookup"><span data-stu-id="5db28-234">Each stream ensures ordered read and write operations; a single stream is a potential scale bottleneck, so the number of streams can be increased to help reduce that bottleneck.</span></span> <span data-ttu-id="5db28-235">Se vários fluxos forem usados, o Signalr distribuirá automaticamente (fragmentos) mensagens entre esses fluxos de forma que garanta que as mensagens enviadas de qualquer conexão específica estejam em ordem.</span><span class="sxs-lookup"><span data-stu-id="5db28-235">If multiple streams are used, SignalR will automatically distribute (shard) messages across these streams in a way that ensures messages sent from any given connection are in order.</span></span>

<span data-ttu-id="5db28-236">A configuração [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx) controla o tamanho da fila de envio de expansão mantida pelo signalr.</span><span class="sxs-lookup"><span data-stu-id="5db28-236">The [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx) setting controls the length of the scaleout send queue maintained by SignalR.</span></span> <span data-ttu-id="5db28-237">Configurá-lo como um valor maior que 0 colocará todas as mensagens em uma fila de envio para serem enviadas uma de cada vez ao backplane de mensagens configurado.</span><span class="sxs-lookup"><span data-stu-id="5db28-237">Setting it to a value greater than 0 will place all messages in a send queue to be sent one at a time to the configured messaging backplane.</span></span> <span data-ttu-id="5db28-238">Se o tamanho da fila ultrapassar o comprimento configurado, as chamadas subsequentes para enviar falharão imediatamente com uma [InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception(v=vs.118).aspx) até que o número de mensagens na fila seja menor que a configuração novamente.</span><span class="sxs-lookup"><span data-stu-id="5db28-238">If the size of the queue goes above the configured length, subsequent calls to send will fail immediately with an [InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception(v=vs.118).aspx) until the number of messages in the queue is less than the setting again.</span></span> <span data-ttu-id="5db28-239">O enfileiramento é desabilitado por padrão porque os backplanes implementados geralmente têm seu próprio enfileiramento ou controle de fluxo em vigor.</span><span class="sxs-lookup"><span data-stu-id="5db28-239">Queueing is disabled by default because the implemented backplanes generally have their own queuing or flow-control in place.</span></span> <span data-ttu-id="5db28-240">No caso do SQL Server, o pool de conexões limita efetivamente o número de envios em andamento a qualquer momento.</span><span class="sxs-lookup"><span data-stu-id="5db28-240">In the case of SQL Server, connection pooling effectively limits the number of sends going on at any one time.</span></span>

<span data-ttu-id="5db28-241">Por padrão, apenas um fluxo é usado para SQL Server e Redis, cinco fluxos são usados para o barramento de serviço, e o enfileiramento é desabilitado, mas essas configurações podem ser alteradas por meio da configuração em SQL Server e barramento de serviço:</span><span class="sxs-lookup"><span data-stu-id="5db28-241">By default, only one stream is used for SQL Server and Redis, five streams are used for Service Bus, and queueing is disabled, but these settings can be changed through configuration on SQL Server and Service Bus:</span></span>

<span data-ttu-id="5db28-242">**Código do servidor .NET para configurar a contagem de tabelas e o tamanho da fila para SQL Server backplane**</span><span class="sxs-lookup"><span data-stu-id="5db28-242">**.NET Server Code for configuring table count and queue length for SQL Server backplane**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample9.cs)]

<span data-ttu-id="5db28-243">**Código do servidor .NET para configurar a contagem de tópicos e o comprimento da fila para o backplane do barramento de serviço**</span><span class="sxs-lookup"><span data-stu-id="5db28-243">**.NET Server Code for configuring topic count and queue length for Service Bus backplane**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample10.cs)]

<span data-ttu-id="5db28-244">Um fluxo de **buffer** é aquele que entrou em um estado com falha; Quando o fluxo estiver no estado com falha, todas as mensagens enviadas ao backplane falharão imediatamente até que o fluxo não esteja mais falhando.</span><span class="sxs-lookup"><span data-stu-id="5db28-244">A **Buffering** stream is one that has entered a faulted state; when the stream is in the faulted state, all messages sent to the backplane will fail immediately until the stream is no longer faulting.</span></span> <span data-ttu-id="5db28-245">O **comprimento da fila de envio** é o número de mensagens que foram lançadas, mas ainda não enviadas.</span><span class="sxs-lookup"><span data-stu-id="5db28-245">The **Send Queue Length** is the number of messages that have been posted but not yet sent.</span></span>

- <span data-ttu-id="5db28-246">**Mensagens de barramento de mensagem de escalabilidade recebidas/s**</span><span class="sxs-lookup"><span data-stu-id="5db28-246">**Scaleout Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="5db28-247">**Total de fluxos de expansão**</span><span class="sxs-lookup"><span data-stu-id="5db28-247">**Scaleout Streams Total**</span></span>
- <span data-ttu-id="5db28-248">**Fluxos de expansão abertos**</span><span class="sxs-lookup"><span data-stu-id="5db28-248">**Scaleout Streams Open**</span></span>
- <span data-ttu-id="5db28-249">**Buffer de fluxos de expansão**</span><span class="sxs-lookup"><span data-stu-id="5db28-249">**Scaleout Streams Buffering**</span></span>
- <span data-ttu-id="5db28-250">**Total de erros de scale out**</span><span class="sxs-lookup"><span data-stu-id="5db28-250">**Scaleout Errors Total**</span></span>
- <span data-ttu-id="5db28-251">**Erros de scale out/s**</span><span class="sxs-lookup"><span data-stu-id="5db28-251">**Scaleout Errors/Sec**</span></span>
- <span data-ttu-id="5db28-252">**Tamanho da fila de envio de expansão**</span><span class="sxs-lookup"><span data-stu-id="5db28-252">**Scaleout Send Queue Length**</span></span>

<span data-ttu-id="5db28-253">Para saber mais sobre o que esses contadores estão medindo, confira o [dimensionamento do signalr com o barramento de serviço do Azure](scaleout-with-windows-azure-service-bus.md).</span><span class="sxs-lookup"><span data-stu-id="5db28-253">For more information on what these counters are measuring, see [SignalR Scaleout with Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span></span>

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a><span data-ttu-id="5db28-254">Usando outros contadores de desempenho</span><span class="sxs-lookup"><span data-stu-id="5db28-254">Using other performance counters</span></span>

<span data-ttu-id="5db28-255">Os contadores de desempenho a seguir também podem ser úteis para monitorar o desempenho do seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="5db28-255">The following performance counters may also be useful in monitoring your application's performance.</span></span>

<span data-ttu-id="5db28-256">**Memória**</span><span class="sxs-lookup"><span data-stu-id="5db28-256">**Memory**</span></span>

- <span data-ttu-id="5db28-257">Memória .NET CLR \\ n º de bytes em todos os heaps (para w3wp)</span><span class="sxs-lookup"><span data-stu-id="5db28-257">.NET CLR Memory\\# bytes in all Heaps (for w3wp)</span></span>

<span data-ttu-id="5db28-258">**ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="5db28-258">**ASP.NET**</span></span>

- <span data-ttu-id="5db28-259">ASP. solicitações atual</span><span class="sxs-lookup"><span data-stu-id="5db28-259">ASP.NET\Requests Current</span></span>
- <span data-ttu-id="5db28-260">ASP. NET\Queued</span><span class="sxs-lookup"><span data-stu-id="5db28-260">ASP.NET\Queued</span></span>
- <span data-ttu-id="5db28-261">ASP. NET\Rejected</span><span class="sxs-lookup"><span data-stu-id="5db28-261">ASP.NET\Rejected</span></span>

<span data-ttu-id="5db28-262">**CPU**</span><span class="sxs-lookup"><span data-stu-id="5db28-262">**CPU**</span></span>

- <span data-ttu-id="5db28-263">Tempo de Information\Processor do processador</span><span class="sxs-lookup"><span data-stu-id="5db28-263">Processor Information\Processor Time</span></span>

<span data-ttu-id="5db28-264">**TCP/IP**</span><span class="sxs-lookup"><span data-stu-id="5db28-264">**TCP/IP**</span></span>

- <span data-ttu-id="5db28-265">TCPv6/conexões estabelecidas</span><span class="sxs-lookup"><span data-stu-id="5db28-265">TCPv6/Connections Established</span></span>
- <span data-ttu-id="5db28-266">TCPv4/conexões estabelecidas</span><span class="sxs-lookup"><span data-stu-id="5db28-266">TCPv4/Connections Established</span></span>

<span data-ttu-id="5db28-267">**Serviço Web**</span><span class="sxs-lookup"><span data-stu-id="5db28-267">**Web Service**</span></span>

- <span data-ttu-id="5db28-268">Conexões de \ da Web</span><span class="sxs-lookup"><span data-stu-id="5db28-268">Web Service\Current Connections</span></span>
- <span data-ttu-id="5db28-269">Conexões de Service\Maximum da Web</span><span class="sxs-lookup"><span data-stu-id="5db28-269">Web Service\Maximum Connections</span></span>

<span data-ttu-id="5db28-270">**Threading**</span><span class="sxs-lookup"><span data-stu-id="5db28-270">**Threading**</span></span>

- <span data-ttu-id="5db28-271">Bloqueios do .NET CLR e threads \\ n º de threads lógicos atuais</span><span class="sxs-lookup"><span data-stu-id="5db28-271">.NET CLR Locks And Threads\\# of current logical Threads</span></span>
- <span data-ttu-id="5db28-272">Bloqueios do .NET CLR e threads \\ n º de threads físicos atuais</span><span class="sxs-lookup"><span data-stu-id="5db28-272">.NET CLR Locks And Threads\\# of current physical Threads</span></span>

<a id="otherresources"></a>

## <a name="other-resources"></a><span data-ttu-id="5db28-273">Outros recursos</span><span class="sxs-lookup"><span data-stu-id="5db28-273">Other Resources</span></span>

<span data-ttu-id="5db28-274">Para obter mais informações sobre o monitoramento e o ajuste do desempenho do ASP.NET, consulte os seguintes tópicos:</span><span class="sxs-lookup"><span data-stu-id="5db28-274">For more information on ASP.NET performance monitoring and tuning, see the following topics:</span></span>

- <span data-ttu-id="5db28-275">[Visão geral do desempenho do ASP.NET](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span><span class="sxs-lookup"><span data-stu-id="5db28-275">[ASP.NET Performance Overview](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span></span>
- [<span data-ttu-id="5db28-276">Uso de thread ASP.NET no IIS 7,5, IIS 7,0 e IIS 6,0</span><span class="sxs-lookup"><span data-stu-id="5db28-276">ASP.NET Thread Usage on IIS 7.5, IIS 7.0, and IIS 6.0</span></span>](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [<span data-ttu-id="5db28-277">&lt;&gt;elemento ApplicationPool (configurações da Web)</span><span class="sxs-lookup"><span data-stu-id="5db28-277">&lt;applicationPool&gt; Element (Web Settings)</span></span>](https://msdn.microsoft.com/library/dd560842.aspx)
