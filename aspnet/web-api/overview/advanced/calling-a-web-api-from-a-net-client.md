---
uid: web-api/overview/advanced/calling-a-web-api-from-a-net-client
title: Chamar uma API da Web de um cliente .NET (C#)-ASP.NET 4. x
author: MikeWasson
description: Este tutorial mostra como chamar uma API da Web de um aplicativo .NET 4. x.
ms.author: riande
ms.date: 11/24/2017
ms.custom: seoapril2019
msc.legacyurl: /web-api/overview/advanced/calling-a-web-api-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: 747be8d14bd8a483fa01fabf3099b3116137bddd
ms.sourcegitcommit: d5049dfb08ff14872ba3b29a702e0589a776b430
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/05/2021
ms.locfileid: "102194559"
---
# <a name="call-a-web-api-from-a-net-client-c"></a><span data-ttu-id="89005-103">Chamar uma API da Web de um cliente .NET (C#)</span><span class="sxs-lookup"><span data-stu-id="89005-103">Call a Web API From a .NET Client (C#)</span></span>

<span data-ttu-id="89005-104">por [Mike Wasson](https://github.com/MikeWasson) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="89005-104">by [Mike Wasson](https://github.com/MikeWasson) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="89005-105">[Download do projeto concluído](https://github.com/dotnet/AspNetDocs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample).</span><span class="sxs-lookup"><span data-stu-id="89005-105">[Download Completed Project](https://github.com/dotnet/AspNetDocs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample).</span></span> <span data-ttu-id="89005-106">[Instruções de download](/aspnet/core/tutorials/#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="89005-106">[Download instructions](/aspnet/core/tutorials/#how-to-download-a-sample).</span></span> 

<span data-ttu-id="89005-107">Este tutorial mostra como chamar uma API da Web de um aplicativo .NET, usando [System .net. http. HttpClient.](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)</span><span class="sxs-lookup"><span data-stu-id="89005-107">This tutorial shows how to call a web API from a .NET application, using [System.Net.Http.HttpClient.](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)</span></span>

<span data-ttu-id="89005-108">Neste tutorial, um aplicativo cliente é escrito que consome a seguinte API da Web:</span><span class="sxs-lookup"><span data-stu-id="89005-108">In this tutorial, a client app is written that consumes the following web API:</span></span>

| <span data-ttu-id="89005-109">Ação</span><span class="sxs-lookup"><span data-stu-id="89005-109">Action</span></span> | <span data-ttu-id="89005-110">Método HTTP</span><span class="sxs-lookup"><span data-stu-id="89005-110">HTTP method</span></span> | <span data-ttu-id="89005-111">URI relativo</span><span class="sxs-lookup"><span data-stu-id="89005-111">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="89005-112">Obter um produto por ID</span><span class="sxs-lookup"><span data-stu-id="89005-112">Get a product by ID</span></span> | <span data-ttu-id="89005-113">GET</span><span class="sxs-lookup"><span data-stu-id="89005-113">GET</span></span> | <span data-ttu-id="89005-114">*ID* do/API/Products/</span><span class="sxs-lookup"><span data-stu-id="89005-114">/api/products/*id*</span></span> |
| <span data-ttu-id="89005-115">Criar um novo produto</span><span class="sxs-lookup"><span data-stu-id="89005-115">Create a new product</span></span> | <span data-ttu-id="89005-116">POST</span><span class="sxs-lookup"><span data-stu-id="89005-116">POST</span></span> | <span data-ttu-id="89005-117">/api/products</span><span class="sxs-lookup"><span data-stu-id="89005-117">/api/products</span></span> |
| <span data-ttu-id="89005-118">Atualizar um produto</span><span class="sxs-lookup"><span data-stu-id="89005-118">Update a product</span></span> | <span data-ttu-id="89005-119">PUT</span><span class="sxs-lookup"><span data-stu-id="89005-119">PUT</span></span> | <span data-ttu-id="89005-120">*ID* do/API/Products/</span><span class="sxs-lookup"><span data-stu-id="89005-120">/api/products/*id*</span></span> |
| <span data-ttu-id="89005-121">Excluir um produto</span><span class="sxs-lookup"><span data-stu-id="89005-121">Delete a product</span></span> | <span data-ttu-id="89005-122">Delete (excluir)</span><span class="sxs-lookup"><span data-stu-id="89005-122">DELETE</span></span> | <span data-ttu-id="89005-123">*ID* do/API/Products/</span><span class="sxs-lookup"><span data-stu-id="89005-123">/api/products/*id*</span></span> |

<span data-ttu-id="89005-124">Para saber como implementar essa API com ASP.NET Web API, consulte [criando uma API Web que oferece suporte a operações CRUD](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
).</span><span class="sxs-lookup"><span data-stu-id="89005-124">To learn how to implement this API with ASP.NET Web API, see [Creating a Web API that Supports CRUD Operations](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
).</span></span>

<span data-ttu-id="89005-125">Para simplificar, o aplicativo cliente neste tutorial é um aplicativo de console do Windows.</span><span class="sxs-lookup"><span data-stu-id="89005-125">For simplicity, the client application in this tutorial is a Windows console application.</span></span> <span data-ttu-id="89005-126">O **HttpClient** também tem suporte para aplicativos Windows Phone e da Windows Store.</span><span class="sxs-lookup"><span data-stu-id="89005-126">**HttpClient** is also supported for Windows Phone and Windows Store apps.</span></span> <span data-ttu-id="89005-127">Para obter mais informações, consulte [escrevendo código de cliente da API Web para várias plataformas usando bibliotecas portáteis](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)</span><span class="sxs-lookup"><span data-stu-id="89005-127">For more information, see [Writing Web API Client Code for Multiple Platforms Using Portable Libraries](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)</span></span>

<span data-ttu-id="89005-128">**Observação:** Se você passar URLs de base e URIs relativos como valores embutidos em código, lembre-se das regras para utilizar a `HttpClient` API.</span><span class="sxs-lookup"><span data-stu-id="89005-128">**NOTE:** If you pass base URLs and relative URIs as hard-coded values, be mindful of the rules for utilizing the `HttpClient` API.</span></span> <span data-ttu-id="89005-129">A `HttpClient.BaseAddress` propriedade deve ser definida como um endereço com uma barra à direita ( `/` ).</span><span class="sxs-lookup"><span data-stu-id="89005-129">The `HttpClient.BaseAddress` property should be set to an address with a trailing forward slash (`/`).</span></span> <span data-ttu-id="89005-130">Por exemplo, ao passar URIs de recursos embutidos em código para o `HttpClient.GetAsync` método, não inclua uma barra à esquerda.</span><span class="sxs-lookup"><span data-stu-id="89005-130">For example, when passing hard-coded resource URIs to the `HttpClient.GetAsync` method, don't include a leading forward slash.</span></span> <span data-ttu-id="89005-131">Para obter um `Product` por ID:</span><span class="sxs-lookup"><span data-stu-id="89005-131">To get a `Product` by ID:</span></span>

1. <span data-ttu-id="89005-132">Defina `client.BaseAddress = new Uri("https://localhost:5001/");`</span><span class="sxs-lookup"><span data-stu-id="89005-132">Set `client.BaseAddress = new Uri("https://localhost:5001/");`</span></span>
1. <span data-ttu-id="89005-133">Solicite um `Product` .</span><span class="sxs-lookup"><span data-stu-id="89005-133">Request a `Product`.</span></span> <span data-ttu-id="89005-134">Por exemplo, `client.GetAsync<Product>("api/products/4");`.</span><span class="sxs-lookup"><span data-stu-id="89005-134">For example, `client.GetAsync<Product>("api/products/4");`.</span></span>

<a id="CreateConsoleApp"></a>
## <a name="create-the-console-application"></a><span data-ttu-id="89005-135">Criar o aplicativo de console</span><span class="sxs-lookup"><span data-stu-id="89005-135">Create the Console Application</span></span>

<span data-ttu-id="89005-136">No Visual Studio, crie um novo aplicativo de console do Windows chamado **HttpClientSample** e cole o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="89005-136">In Visual Studio, create a new Windows console app named **HttpClientSample** and paste in the following code:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_all)]

<span data-ttu-id="89005-137">O código anterior é o aplicativo cliente completo.</span><span class="sxs-lookup"><span data-stu-id="89005-137">The preceding code is the complete client app.</span></span>

<span data-ttu-id="89005-138">`RunAsync` executa e bloqueia até que seja concluído.</span><span class="sxs-lookup"><span data-stu-id="89005-138">`RunAsync` runs and blocks until it completes.</span></span> <span data-ttu-id="89005-139">A maioria dos métodos **HttpClient** são Async, pois eles executam e/s de rede.</span><span class="sxs-lookup"><span data-stu-id="89005-139">Most **HttpClient** methods are async, because they perform network I/O.</span></span> <span data-ttu-id="89005-140">Todas as tarefas assíncronas são feitas dentro do `RunAsync` .</span><span class="sxs-lookup"><span data-stu-id="89005-140">All of the async tasks are done inside `RunAsync`.</span></span> <span data-ttu-id="89005-141">Normalmente, um aplicativo não bloqueia o thread principal, mas esse aplicativo não permite nenhuma interação.</span><span class="sxs-lookup"><span data-stu-id="89005-141">Normally an app doesn't block the main thread, but this app doesn't allow any interaction.</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_run)]

<a id="InstallClientLib"></a>
## <a name="install-the-web-api-client-libraries"></a><span data-ttu-id="89005-142">Instalar as bibliotecas de cliente da API Web</span><span class="sxs-lookup"><span data-stu-id="89005-142">Install the Web API Client Libraries</span></span>

<span data-ttu-id="89005-143">Use o Gerenciador de pacotes NuGet para instalar o pacote de bibliotecas de cliente de API Web.</span><span class="sxs-lookup"><span data-stu-id="89005-143">Use NuGet Package Manager to install the Web API Client Libraries package.</span></span>

<span data-ttu-id="89005-144">No menu **Ferramentas** selecione **Gerenciador de Pacotes NuGet** > **Console do Gerenciador de Pacotes**.</span><span class="sxs-lookup"><span data-stu-id="89005-144">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span> <span data-ttu-id="89005-145">No console do Gerenciador de pacotes (PMC), digite o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="89005-145">In the Package Manager Console (PMC), type the following command:</span></span>

`Install-Package Microsoft.AspNet.WebApi.Client`

<span data-ttu-id="89005-146">O comando anterior adiciona os seguintes pacotes NuGet ao projeto:</span><span class="sxs-lookup"><span data-stu-id="89005-146">The preceding command adds the following NuGet packages to the project:</span></span>

* <span data-ttu-id="89005-147">Microsoft. AspNet. WebApi. Client</span><span class="sxs-lookup"><span data-stu-id="89005-147">Microsoft.AspNet.WebApi.Client</span></span>
* <span data-ttu-id="89005-148">Newtonsoft.Json</span><span class="sxs-lookup"><span data-stu-id="89005-148">Newtonsoft.Json</span></span>

<span data-ttu-id="89005-149">Netwonsoft.Json (também conhecido como Json.NET) é uma estrutura JSON popular de alto desempenho para .NET.</span><span class="sxs-lookup"><span data-stu-id="89005-149">Netwonsoft.Json (also known as Json.NET) is a popular high-performance JSON framework for .NET.</span></span>

<a id="AddModelClass"></a>
## <a name="add-a-model-class"></a><span data-ttu-id="89005-150">Adicionar uma classe de modelo</span><span class="sxs-lookup"><span data-stu-id="89005-150">Add a Model Class</span></span>

<span data-ttu-id="89005-151">Examine a classe `Product`:</span><span class="sxs-lookup"><span data-stu-id="89005-151">Examine the `Product` class:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_prod)]

<span data-ttu-id="89005-152">Essa classe corresponde ao modelo de dados usado pela API Web.</span><span class="sxs-lookup"><span data-stu-id="89005-152">This class matches the data model used by the web API.</span></span> <span data-ttu-id="89005-153">Um aplicativo pode usar **HttpClient** para ler uma `Product` instância de uma resposta http.</span><span class="sxs-lookup"><span data-stu-id="89005-153">An app can use **HttpClient** to read a `Product` instance from an HTTP response.</span></span> <span data-ttu-id="89005-154">O aplicativo não precisa escrever nenhum código de desserialização.</span><span class="sxs-lookup"><span data-stu-id="89005-154">The app doesn't have to write any deserialization code.</span></span>

<a id="InitClient"></a>
## <a name="create-and-initialize-httpclient"></a><span data-ttu-id="89005-155">Criar e inicializar HttpClient</span><span class="sxs-lookup"><span data-stu-id="89005-155">Create and Initialize HttpClient</span></span>

<span data-ttu-id="89005-156">Examine a propriedade estática **HttpClient** :</span><span class="sxs-lookup"><span data-stu-id="89005-156">Examine the static **HttpClient** property:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_HttpClient)]

<span data-ttu-id="89005-157">O **HttpClient** deve ser instanciado uma vez e reutilizado durante toda a vida útil de um aplicativo.</span><span class="sxs-lookup"><span data-stu-id="89005-157">**HttpClient** is intended to be instantiated once and reused throughout the life of an application.</span></span> <span data-ttu-id="89005-158">As seguintes condições podem resultar em erros de **SocketException** :</span><span class="sxs-lookup"><span data-stu-id="89005-158">The following conditions can result in **SocketException** errors:</span></span>

* <span data-ttu-id="89005-159">Criando uma nova instância de **HttpClient** por solicitação.</span><span class="sxs-lookup"><span data-stu-id="89005-159">Creating a new **HttpClient** instance per request.</span></span>
* <span data-ttu-id="89005-160">Servidor sob carga pesada.</span><span class="sxs-lookup"><span data-stu-id="89005-160">Server under heavy load.</span></span>

<span data-ttu-id="89005-161">A criação de uma nova instância **HttpClient** por solicitação pode esgotar os soquetes disponíveis.</span><span class="sxs-lookup"><span data-stu-id="89005-161">Creating a new **HttpClient** instance per request can exhaust the available sockets.</span></span>

<span data-ttu-id="89005-162">O código a seguir inicializa a instância de **HttpClient** :</span><span class="sxs-lookup"><span data-stu-id="89005-162">The following code initializes the **HttpClient** instance:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5)]

<span data-ttu-id="89005-163">O código anterior:</span><span class="sxs-lookup"><span data-stu-id="89005-163">The preceding code:</span></span>

* <span data-ttu-id="89005-164">Define o URI de base para solicitações HTTP.</span><span class="sxs-lookup"><span data-stu-id="89005-164">Sets the base URI for HTTP requests.</span></span> <span data-ttu-id="89005-165">Altere o número da porta para a porta usada no aplicativo do servidor.</span><span class="sxs-lookup"><span data-stu-id="89005-165">Change the port number to the port used in the server app.</span></span> <span data-ttu-id="89005-166">O aplicativo não funcionará a menos que a porta para o aplicativo do servidor seja usada.</span><span class="sxs-lookup"><span data-stu-id="89005-166">The app won't work unless port for the server app is used.</span></span>
* <span data-ttu-id="89005-167">Define o cabeçalho Accept como "Application/JSON".</span><span class="sxs-lookup"><span data-stu-id="89005-167">Sets the Accept header to "application/json".</span></span> <span data-ttu-id="89005-168">Definir esse cabeçalho informa ao servidor para enviar dados no formato JSON.</span><span class="sxs-lookup"><span data-stu-id="89005-168">Setting this header tells the server to send data in JSON format.</span></span>

<a id="GettingResource"></a>
## <a name="send-a-get-request-to-retrieve-a-resource"></a><span data-ttu-id="89005-169">Enviar uma solicitação GET para recuperar um recurso</span><span class="sxs-lookup"><span data-stu-id="89005-169">Send a GET request to retrieve a resource</span></span>

<span data-ttu-id="89005-170">O código a seguir envia uma solicitação GET para um produto:</span><span class="sxs-lookup"><span data-stu-id="89005-170">The following code sends a GET request for a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_GetProductAsync)]

<span data-ttu-id="89005-171">O método **getasync** envia a solicitação HTTP Get.</span><span class="sxs-lookup"><span data-stu-id="89005-171">The **GetAsync** method sends the HTTP GET request.</span></span> <span data-ttu-id="89005-172">Quando o método é concluído, ele retorna um **HttpResponseMessage** que contém a resposta http.</span><span class="sxs-lookup"><span data-stu-id="89005-172">When the method completes, it returns an **HttpResponseMessage** that contains the HTTP response.</span></span> <span data-ttu-id="89005-173">Se o código de status na resposta for um código de êxito, o corpo da resposta conterá a representação JSON de um produto.</span><span class="sxs-lookup"><span data-stu-id="89005-173">If the status code in the response is a success code, the response body contains the JSON representation of a product.</span></span> <span data-ttu-id="89005-174">Chame **ReadAsAsync** para desserializar a carga JSON para uma `Product` instância.</span><span class="sxs-lookup"><span data-stu-id="89005-174">Call **ReadAsAsync** to deserialize the JSON payload to a `Product` instance.</span></span> <span data-ttu-id="89005-175">O método **ReadAsAsync** é assíncrono porque o corpo da resposta pode ser arbitrariamente grande.</span><span class="sxs-lookup"><span data-stu-id="89005-175">The **ReadAsAsync** method is asynchronous because the response body can be arbitrarily large.</span></span>

<span data-ttu-id="89005-176">**HttpClient** não lança uma exceção quando a resposta http contém um código de erro.</span><span class="sxs-lookup"><span data-stu-id="89005-176">**HttpClient** does not throw an exception when the HTTP response contains an error code.</span></span> <span data-ttu-id="89005-177">Em vez disso, a propriedade **IsSuccessStatusCode** será **false** se o status for um código de erro.</span><span class="sxs-lookup"><span data-stu-id="89005-177">Instead, the **IsSuccessStatusCode** property is **false** if the status is an error code.</span></span> <span data-ttu-id="89005-178">Se você preferir tratar códigos de erro HTTP como exceções, chame [HttpResponseMessage. EnsureSuccessStatusCode](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) no objeto de resposta.</span><span class="sxs-lookup"><span data-stu-id="89005-178">If you prefer to treat HTTP error codes as exceptions, call [HttpResponseMessage.EnsureSuccessStatusCode](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) on the response object.</span></span> <span data-ttu-id="89005-179">`EnsureSuccessStatusCode` gera uma exceção se o código de status cair fora do intervalo de 200 a &ndash; 299.</span><span class="sxs-lookup"><span data-stu-id="89005-179">`EnsureSuccessStatusCode` throws an exception if the status code falls outside the range 200&ndash;299.</span></span> <span data-ttu-id="89005-180">Observe que o **HttpClient** pode gerar exceções por outros motivos &mdash; , por exemplo, se a solicitação atingir o tempo limite.</span><span class="sxs-lookup"><span data-stu-id="89005-180">Note that **HttpClient** can throw exceptions for other reasons &mdash; for example, if the request times out.</span></span>

<a id="MediaTypeFormatters"></a>
### <a name="media-type-formatters-to-deserialize"></a><span data-ttu-id="89005-181">Media-Type formatadores para desserializar</span><span class="sxs-lookup"><span data-stu-id="89005-181">Media-Type Formatters to Deserialize</span></span>

<span data-ttu-id="89005-182">Quando **ReadAsAsync** é chamado sem parâmetros, ele usa um conjunto padrão de *formatadores de mídia* para ler o corpo da resposta.</span><span class="sxs-lookup"><span data-stu-id="89005-182">When **ReadAsAsync** is called with no parameters, it uses a default set of *media formatters* to read the response body.</span></span> <span data-ttu-id="89005-183">Os formatadores padrão dão suporte aos dados JSON, XML e com codificação de URL de formulário.</span><span class="sxs-lookup"><span data-stu-id="89005-183">The default formatters support JSON, XML, and Form-url-encoded data.</span></span>

<span data-ttu-id="89005-184">Em vez de usar os formatadores padrão, você pode fornecer uma lista de formatadores para o método **ReadAsAsync** .</span><span class="sxs-lookup"><span data-stu-id="89005-184">Instead of using the default formatters, you can provide a list of formatters to the **ReadAsAsync** method.</span></span>  <span data-ttu-id="89005-185">Usar uma lista de formatadores será útil se você tiver um formatador de tipo de mídia personalizado:</span><span class="sxs-lookup"><span data-stu-id="89005-185">Using a list of formatters is useful if you have a custom media-type formatter:</span></span>

```csharp
var formatters = new List<MediaTypeFormatter>() {
    new MyCustomFormatter(),
    new JsonMediaTypeFormatter(),
    new XmlMediaTypeFormatter()
};
resp.Content.ReadAsAsync<IEnumerable<Product>>(formatters);
```

<span data-ttu-id="89005-186">Para obter mais informações, consulte [formatadores de mídia no ASP.NET Web API 2](../formats-and-model-binding/media-formatters.md)</span><span class="sxs-lookup"><span data-stu-id="89005-186">For more information, see [Media Formatters in ASP.NET Web API 2](../formats-and-model-binding/media-formatters.md)</span></span>

## <a name="sending-a-post-request-to-create-a-resource"></a><span data-ttu-id="89005-187">Enviando uma solicitação POST para criar um recurso</span><span class="sxs-lookup"><span data-stu-id="89005-187">Sending a POST Request to Create a Resource</span></span>

<span data-ttu-id="89005-188">O código a seguir envia uma solicitação POST que contém uma `Product` instância no formato JSON:</span><span class="sxs-lookup"><span data-stu-id="89005-188">The following code sends a POST request that contains a `Product` instance in JSON format:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_CreateProductAsync)]

<span data-ttu-id="89005-189">O método **PostAsJsonAsync** :</span><span class="sxs-lookup"><span data-stu-id="89005-189">The **PostAsJsonAsync** method:</span></span>

* <span data-ttu-id="89005-190">Serializa um objeto para JSON.</span><span class="sxs-lookup"><span data-stu-id="89005-190">Serializes an object to JSON.</span></span>
* <span data-ttu-id="89005-191">Envia a carga JSON em uma solicitação POST.</span><span class="sxs-lookup"><span data-stu-id="89005-191">Sends the JSON payload in a POST request.</span></span>

<span data-ttu-id="89005-192">Se a solicitação for concluída com sucesso:</span><span class="sxs-lookup"><span data-stu-id="89005-192">If the request succeeds:</span></span>

* <span data-ttu-id="89005-193">Ele deve retornar uma resposta 201 (criada).</span><span class="sxs-lookup"><span data-stu-id="89005-193">It should return a 201 (Created) response.</span></span>
* <span data-ttu-id="89005-194">A resposta deve incluir a URL dos recursos criados no cabeçalho de local.</span><span class="sxs-lookup"><span data-stu-id="89005-194">The response should include the URL of the created resources in the Location header.</span></span>

<a id="PuttingResource"></a>
## <a name="sending-a-put-request-to-update-a-resource"></a><span data-ttu-id="89005-195">Enviando uma solicitação PUT para atualizar um recurso</span><span class="sxs-lookup"><span data-stu-id="89005-195">Sending a PUT Request to Update a Resource</span></span>

<span data-ttu-id="89005-196">O código a seguir envia uma solicitação PUT para atualizar um produto:</span><span class="sxs-lookup"><span data-stu-id="89005-196">The following code sends a PUT request to update a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_UpdateProductAsync)]

<span data-ttu-id="89005-197">O método **PutAsJsonAsync** funciona como **PostAsJsonAsync**, exceto que ele envia uma solicitação Put em vez de post.</span><span class="sxs-lookup"><span data-stu-id="89005-197">The **PutAsJsonAsync** method works like **PostAsJsonAsync**, except that it sends a PUT request instead of POST.</span></span>

<a id="DeletingResource"></a>
## <a name="sending-a-delete-request-to-delete-a-resource"></a><span data-ttu-id="89005-198">Enviando uma solicitação de exclusão para excluir um recurso</span><span class="sxs-lookup"><span data-stu-id="89005-198">Sending a DELETE Request to Delete a Resource</span></span>

<span data-ttu-id="89005-199">O código a seguir envia uma solicitação de exclusão para excluir um produto:</span><span class="sxs-lookup"><span data-stu-id="89005-199">The following code sends a DELETE request to delete a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_DeleteProductAsync)]

<span data-ttu-id="89005-200">Como GET, uma solicitação de exclusão não tem um corpo de solicitação.</span><span class="sxs-lookup"><span data-stu-id="89005-200">Like GET, a DELETE request does not have a request body.</span></span> <span data-ttu-id="89005-201">Você não precisa especificar o formato JSON ou XML com DELETE.</span><span class="sxs-lookup"><span data-stu-id="89005-201">You don't need to specify JSON or XML format with DELETE.</span></span>

## <a name="test-the-sample"></a><span data-ttu-id="89005-202">O exemplo de teste</span><span class="sxs-lookup"><span data-stu-id="89005-202">Test the sample</span></span>

<span data-ttu-id="89005-203">Para testar o aplicativo cliente:</span><span class="sxs-lookup"><span data-stu-id="89005-203">To test the client app:</span></span>

1. <span data-ttu-id="89005-204">[Baixe](https://github.com/dotnet/AspNetDocs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample/server) e execute o aplicativo de servidor.</span><span class="sxs-lookup"><span data-stu-id="89005-204">[Download](https://github.com/dotnet/AspNetDocs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample/server) and run the server app.</span></span> <span data-ttu-id="89005-205">[Instruções de download](/aspnet/core/#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="89005-205">[Download instructions](/aspnet/core/#how-to-download-a-sample).</span></span> <span data-ttu-id="89005-206">Verifique se o aplicativo do servidor está funcionando.</span><span class="sxs-lookup"><span data-stu-id="89005-206">Verify the server app is working.</span></span> <span data-ttu-id="89005-207">Por exemplo, `http://localhost:64195/api/products` deve retornar uma lista de produtos.</span><span class="sxs-lookup"><span data-stu-id="89005-207">For example, `http://localhost:64195/api/products` should return a list of products.</span></span>
2. <span data-ttu-id="89005-208">Defina o URI de base para solicitações HTTP.</span><span class="sxs-lookup"><span data-stu-id="89005-208">Set the base URI for HTTP requests.</span></span> <span data-ttu-id="89005-209">Altere o número da porta para a porta usada no aplicativo do servidor.</span><span class="sxs-lookup"><span data-stu-id="89005-209">Change the port number to the port used in the server app.</span></span>
    [!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5)]

3. <span data-ttu-id="89005-210">Execute o aplicativo cliente.</span><span class="sxs-lookup"><span data-stu-id="89005-210">Run the client app.</span></span> <span data-ttu-id="89005-211">A seguinte saída é produzida:</span><span class="sxs-lookup"><span data-stu-id="89005-211">The following output is produced:</span></span>

   ```console
   Created at http://localhost:64195/api/products/4
   Name: Gizmo     Price: 100.0    Category: Widgets
   Updating price...
   Name: Gizmo     Price: 80.0     Category: Widgets
   Deleted (HTTP Status = 204)
   ```
