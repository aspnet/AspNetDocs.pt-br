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
# <a name="call-a-web-api-from-a-net-client-c"></a>Chamar uma API da Web de um cliente .NET (C#)

por [Mike Wasson](https://github.com/MikeWasson) e [Rick Anderson](https://twitter.com/RickAndMSFT)

[Download do projeto concluído](https://github.com/dotnet/AspNetDocs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample). [Instruções de download](/aspnet/core/tutorials/#how-to-download-a-sample). 

Este tutorial mostra como chamar uma API da Web de um aplicativo .NET, usando [System .net. http. HttpClient.](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)

Neste tutorial, um aplicativo cliente é escrito que consome a seguinte API da Web:

| Ação | Método HTTP | URI relativo |
| --- | --- | --- |
| Obter um produto por ID | GET | *ID* do/API/Products/ |
| Criar um novo produto | POST | /api/products |
| Atualizar um produto | PUT | *ID* do/API/Products/ |
| Excluir um produto | Delete (excluir) | *ID* do/API/Products/ |

Para saber como implementar essa API com ASP.NET Web API, consulte [criando uma API Web que oferece suporte a operações CRUD](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
).

Para simplificar, o aplicativo cliente neste tutorial é um aplicativo de console do Windows. O **HttpClient** também tem suporte para aplicativos Windows Phone e da Windows Store. Para obter mais informações, consulte [escrevendo código de cliente da API Web para várias plataformas usando bibliotecas portáteis](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)

**Observação:** Se você passar URLs de base e URIs relativos como valores embutidos em código, lembre-se das regras para utilizar a `HttpClient` API. A `HttpClient.BaseAddress` propriedade deve ser definida como um endereço com uma barra à direita ( `/` ). Por exemplo, ao passar URIs de recursos embutidos em código para o `HttpClient.GetAsync` método, não inclua uma barra à esquerda. Para obter um `Product` por ID:

1. Defina `client.BaseAddress = new Uri("https://localhost:5001/");`
1. Solicite um `Product` . Por exemplo, `client.GetAsync<Product>("api/products/4");`.

<a id="CreateConsoleApp"></a>
## <a name="create-the-console-application"></a>Criar o aplicativo de console

No Visual Studio, crie um novo aplicativo de console do Windows chamado **HttpClientSample** e cole o código a seguir:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_all)]

O código anterior é o aplicativo cliente completo.

`RunAsync` executa e bloqueia até que seja concluído. A maioria dos métodos **HttpClient** são Async, pois eles executam e/s de rede. Todas as tarefas assíncronas são feitas dentro do `RunAsync` . Normalmente, um aplicativo não bloqueia o thread principal, mas esse aplicativo não permite nenhuma interação.

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_run)]

<a id="InstallClientLib"></a>
## <a name="install-the-web-api-client-libraries"></a>Instalar as bibliotecas de cliente da API Web

Use o Gerenciador de pacotes NuGet para instalar o pacote de bibliotecas de cliente de API Web.

No menu **Ferramentas** selecione **Gerenciador de Pacotes NuGet** > **Console do Gerenciador de Pacotes**. No console do Gerenciador de pacotes (PMC), digite o seguinte comando:

`Install-Package Microsoft.AspNet.WebApi.Client`

O comando anterior adiciona os seguintes pacotes NuGet ao projeto:

* Microsoft. AspNet. WebApi. Client
* Newtonsoft.Json

Netwonsoft.Json (também conhecido como Json.NET) é uma estrutura JSON popular de alto desempenho para .NET.

<a id="AddModelClass"></a>
## <a name="add-a-model-class"></a>Adicionar uma classe de modelo

Examine a classe `Product`:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_prod)]

Essa classe corresponde ao modelo de dados usado pela API Web. Um aplicativo pode usar **HttpClient** para ler uma `Product` instância de uma resposta http. O aplicativo não precisa escrever nenhum código de desserialização.

<a id="InitClient"></a>
## <a name="create-and-initialize-httpclient"></a>Criar e inicializar HttpClient

Examine a propriedade estática **HttpClient** :

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_HttpClient)]

O **HttpClient** deve ser instanciado uma vez e reutilizado durante toda a vida útil de um aplicativo. As seguintes condições podem resultar em erros de **SocketException** :

* Criando uma nova instância de **HttpClient** por solicitação.
* Servidor sob carga pesada.

A criação de uma nova instância **HttpClient** por solicitação pode esgotar os soquetes disponíveis.

O código a seguir inicializa a instância de **HttpClient** :

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5)]

O código anterior:

* Define o URI de base para solicitações HTTP. Altere o número da porta para a porta usada no aplicativo do servidor. O aplicativo não funcionará a menos que a porta para o aplicativo do servidor seja usada.
* Define o cabeçalho Accept como "Application/JSON". Definir esse cabeçalho informa ao servidor para enviar dados no formato JSON.

<a id="GettingResource"></a>
## <a name="send-a-get-request-to-retrieve-a-resource"></a>Enviar uma solicitação GET para recuperar um recurso

O código a seguir envia uma solicitação GET para um produto:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_GetProductAsync)]

O método **getasync** envia a solicitação HTTP Get. Quando o método é concluído, ele retorna um **HttpResponseMessage** que contém a resposta http. Se o código de status na resposta for um código de êxito, o corpo da resposta conterá a representação JSON de um produto. Chame **ReadAsAsync** para desserializar a carga JSON para uma `Product` instância. O método **ReadAsAsync** é assíncrono porque o corpo da resposta pode ser arbitrariamente grande.

**HttpClient** não lança uma exceção quando a resposta http contém um código de erro. Em vez disso, a propriedade **IsSuccessStatusCode** será **false** se o status for um código de erro. Se você preferir tratar códigos de erro HTTP como exceções, chame [HttpResponseMessage. EnsureSuccessStatusCode](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) no objeto de resposta. `EnsureSuccessStatusCode` gera uma exceção se o código de status cair fora do intervalo de 200 a &ndash; 299. Observe que o **HttpClient** pode gerar exceções por outros motivos &mdash; , por exemplo, se a solicitação atingir o tempo limite.

<a id="MediaTypeFormatters"></a>
### <a name="media-type-formatters-to-deserialize"></a>Media-Type formatadores para desserializar

Quando **ReadAsAsync** é chamado sem parâmetros, ele usa um conjunto padrão de *formatadores de mídia* para ler o corpo da resposta. Os formatadores padrão dão suporte aos dados JSON, XML e com codificação de URL de formulário.

Em vez de usar os formatadores padrão, você pode fornecer uma lista de formatadores para o método **ReadAsAsync** .  Usar uma lista de formatadores será útil se você tiver um formatador de tipo de mídia personalizado:

```csharp
var formatters = new List<MediaTypeFormatter>() {
    new MyCustomFormatter(),
    new JsonMediaTypeFormatter(),
    new XmlMediaTypeFormatter()
};
resp.Content.ReadAsAsync<IEnumerable<Product>>(formatters);
```

Para obter mais informações, consulte [formatadores de mídia no ASP.NET Web API 2](../formats-and-model-binding/media-formatters.md)

## <a name="sending-a-post-request-to-create-a-resource"></a>Enviando uma solicitação POST para criar um recurso

O código a seguir envia uma solicitação POST que contém uma `Product` instância no formato JSON:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_CreateProductAsync)]

O método **PostAsJsonAsync** :

* Serializa um objeto para JSON.
* Envia a carga JSON em uma solicitação POST.

Se a solicitação for concluída com sucesso:

* Ele deve retornar uma resposta 201 (criada).
* A resposta deve incluir a URL dos recursos criados no cabeçalho de local.

<a id="PuttingResource"></a>
## <a name="sending-a-put-request-to-update-a-resource"></a>Enviando uma solicitação PUT para atualizar um recurso

O código a seguir envia uma solicitação PUT para atualizar um produto:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_UpdateProductAsync)]

O método **PutAsJsonAsync** funciona como **PostAsJsonAsync**, exceto que ele envia uma solicitação Put em vez de post.

<a id="DeletingResource"></a>
## <a name="sending-a-delete-request-to-delete-a-resource"></a>Enviando uma solicitação de exclusão para excluir um recurso

O código a seguir envia uma solicitação de exclusão para excluir um produto:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_DeleteProductAsync)]

Como GET, uma solicitação de exclusão não tem um corpo de solicitação. Você não precisa especificar o formato JSON ou XML com DELETE.

## <a name="test-the-sample"></a>O exemplo de teste

Para testar o aplicativo cliente:

1. [Baixe](https://github.com/dotnet/AspNetDocs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample/server) e execute o aplicativo de servidor. [Instruções de download](/aspnet/core/#how-to-download-a-sample). Verifique se o aplicativo do servidor está funcionando. Por exemplo, `http://localhost:64195/api/products` deve retornar uma lista de produtos.
2. Defina o URI de base para solicitações HTTP. Altere o número da porta para a porta usada no aplicativo do servidor.
    [!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5)]

3. Execute o aplicativo cliente. A seguinte saída é produzida:

   ```console
   Created at http://localhost:64195/api/products/4
   Name: Gizmo     Price: 100.0    Category: Widgets
   Updating price...
   Name: Gizmo     Price: 80.0     Category: Widgets
   Deleted (HTTP Status = 204)
   ```
