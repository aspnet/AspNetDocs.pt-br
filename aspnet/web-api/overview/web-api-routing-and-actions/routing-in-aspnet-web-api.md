---
uid: web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
title: Roteamento em ASP.NET Web API | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 10/29/2018
ms.assetid: 0675bdc7-282f-4f47-b7f3-7e02133940ca
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 85862c094cc54365267b1f21e68d235a15519cda
ms.sourcegitcommit: c62ec20b453cee3249eb894ecd75013b57d078f0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/21/2020
ms.locfileid: "92345191"
---
# <a name="routing-in-aspnet-web-api"></a><span data-ttu-id="1e454-102">Roteamento no ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="1e454-102">Routing in ASP.NET Web API</span></span>

<span data-ttu-id="1e454-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="1e454-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="1e454-104">Este artigo descreve como ASP.NET Web API roteia as solicitações HTTP para os controladores.</span><span class="sxs-lookup"><span data-stu-id="1e454-104">This article describes how ASP.NET Web API routes HTTP requests to controllers.</span></span>

> [!NOTE]
> <span data-ttu-id="1e454-105">Se você estiver familiarizado com o ASP.NET MVC, o roteamento da API Web é muito semelhante ao roteamento do MVC.</span><span class="sxs-lookup"><span data-stu-id="1e454-105">If you are familiar with ASP.NET MVC, Web API routing is very similar to MVC routing.</span></span> <span data-ttu-id="1e454-106">A principal diferença é que a API da Web usa o verbo HTTP, não o caminho do URI, para selecionar a ação.</span><span class="sxs-lookup"><span data-stu-id="1e454-106">The main difference is that Web API uses the HTTP verb, not the URI path, to select the action.</span></span> <span data-ttu-id="1e454-107">Você também pode usar o roteamento no estilo MVC na API da Web.</span><span class="sxs-lookup"><span data-stu-id="1e454-107">You can also use MVC-style routing in Web API.</span></span> <span data-ttu-id="1e454-108">Este artigo não assume nenhum conhecimento do ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="1e454-108">This article does not assume any knowledge of ASP.NET MVC.</span></span>

## <a name="routing-tables"></a><span data-ttu-id="1e454-109">Tabelas de roteamento</span><span class="sxs-lookup"><span data-stu-id="1e454-109">Routing Tables</span></span>

<span data-ttu-id="1e454-110">No ASP.NET Web API, um *controlador* é uma classe que MANIPULA solicitações HTTP.</span><span class="sxs-lookup"><span data-stu-id="1e454-110">In ASP.NET Web API, a *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="1e454-111">Os métodos públicos do controlador são chamados de *métodos de ação* ou simplesmente *ações*.</span><span class="sxs-lookup"><span data-stu-id="1e454-111">The public methods of the controller are called *action methods* or simply *actions*.</span></span> <span data-ttu-id="1e454-112">Quando a estrutura da API Web recebe uma solicitação, ela roteia a solicitação para uma ação.</span><span class="sxs-lookup"><span data-stu-id="1e454-112">When the Web API framework receives a request, it routes the request to an action.</span></span>

<span data-ttu-id="1e454-113">Para determinar qual ação invocar, a estrutura usa uma *tabela de roteamento*.</span><span class="sxs-lookup"><span data-stu-id="1e454-113">To determine which action to invoke, the framework uses a *routing table*.</span></span> <span data-ttu-id="1e454-114">O modelo de projeto do Visual Studio para API da Web cria uma rota padrão:</span><span class="sxs-lookup"><span data-stu-id="1e454-114">The Visual Studio project template for Web API creates a default route:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="1e454-115">Essa rota é definida no arquivo *WebApiConfig.cs* , que é colocado no diretório \* \_ inicial do aplicativo\* :</span><span class="sxs-lookup"><span data-stu-id="1e454-115">This route is defined in the *WebApiConfig.cs* file, which is placed in the *App\_Start* directory:</span></span>

![](routing-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="1e454-116">Para obter mais informações sobre a `WebApiConfig` classe, consulte [configurando ASP.NET Web API](../advanced/configuring-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="1e454-116">For more information about the `WebApiConfig` class, see [Configuring ASP.NET Web API](../advanced/configuring-aspnet-web-api.md).</span></span>

<span data-ttu-id="1e454-117">Se você hospedar a API Web de hospedagem própria, deverá definir a tabela de roteamento diretamente no `HttpSelfHostConfiguration` objeto.</span><span class="sxs-lookup"><span data-stu-id="1e454-117">If you self-host Web API, you must set the routing table directly on the `HttpSelfHostConfiguration` object.</span></span> <span data-ttu-id="1e454-118">Para obter mais informações, consulte [auto-hospedar uma API Web](../older-versions/self-host-a-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="1e454-118">For more information, see [Self-Host a Web API](../older-versions/self-host-a-web-api.md).</span></span>

<span data-ttu-id="1e454-119">Cada entrada na tabela de roteamento contém um *modelo de rota*.</span><span class="sxs-lookup"><span data-stu-id="1e454-119">Each entry in the routing table contains a *route template*.</span></span> <span data-ttu-id="1e454-120">O modelo de rota padrão para a API Web é &quot; API/{Controller}/{ID} &quot; .</span><span class="sxs-lookup"><span data-stu-id="1e454-120">The default route template for Web API is &quot;api/{controller}/{id}&quot;.</span></span> <span data-ttu-id="1e454-121">Neste modelo, &quot; a API &quot; é um segmento de caminho literal e {Controller} e {ID} são variáveis de espaço reservado.</span><span class="sxs-lookup"><span data-stu-id="1e454-121">In this template, &quot;api&quot; is a literal path segment, and {controller} and {id} are placeholder variables.</span></span>

<span data-ttu-id="1e454-122">Quando a estrutura da API Web recebe uma solicitação HTTP, ela tenta corresponder o URI em um dos modelos de rota na tabela de roteamento.</span><span class="sxs-lookup"><span data-stu-id="1e454-122">When the Web API framework receives an HTTP request, it tries to match the URI against one of the route templates in the routing table.</span></span> <span data-ttu-id="1e454-123">Se nenhuma rota corresponder, o cliente receberá um erro 404.</span><span class="sxs-lookup"><span data-stu-id="1e454-123">If no route matches, the client receives a 404 error.</span></span> <span data-ttu-id="1e454-124">Por exemplo, os seguintes URIs correspondem à rota padrão:</span><span class="sxs-lookup"><span data-stu-id="1e454-124">For example, the following URIs match the default route:</span></span>

- <span data-ttu-id="1e454-125">/api/contacts</span><span class="sxs-lookup"><span data-stu-id="1e454-125">/api/contacts</span></span>
- <span data-ttu-id="1e454-126">/api/contacts/1</span><span class="sxs-lookup"><span data-stu-id="1e454-126">/api/contacts/1</span></span>
- <span data-ttu-id="1e454-127">/api/products/gizmo1</span><span class="sxs-lookup"><span data-stu-id="1e454-127">/api/products/gizmo1</span></span>

<span data-ttu-id="1e454-128">No entanto, o URI a seguir não corresponde, pois ele não tem o &quot; segmento de API &quot; :</span><span class="sxs-lookup"><span data-stu-id="1e454-128">However, the following URI does not match, because it lacks the &quot;api&quot; segment:</span></span>

- <span data-ttu-id="1e454-129">/contacts/1</span><span class="sxs-lookup"><span data-stu-id="1e454-129">/contacts/1</span></span>

> [!NOTE]
> <span data-ttu-id="1e454-130">O motivo para usar "API" na rota é evitar colisões com o roteamento MVC do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="1e454-130">The reason for using "api" in the route is to avoid collisions with ASP.NET MVC routing.</span></span> <span data-ttu-id="1e454-131">Dessa forma, você pode fazer com que &quot; &quot; o/Contacts vá para um controlador MVC e &quot; /API/Contacts &quot; vá para um controlador de API da Web.</span><span class="sxs-lookup"><span data-stu-id="1e454-131">That way, you can have &quot;/contacts&quot; go to an MVC controller, and &quot;/api/contacts&quot; go to a Web API controller.</span></span> <span data-ttu-id="1e454-132">É claro que, se você não gostar dessa Convenção, poderá alterar a tabela de rotas padrão.</span><span class="sxs-lookup"><span data-stu-id="1e454-132">Of course, if you don't like this convention, you can change the default route table.</span></span>

<span data-ttu-id="1e454-133">Quando uma rota correspondente é encontrada, a API da Web seleciona o controlador e a ação:</span><span class="sxs-lookup"><span data-stu-id="1e454-133">Once a matching route is found, Web API selects the controller and the action:</span></span>

- <span data-ttu-id="1e454-134">Para localizar o controlador, a API Web &quot; adiciona &quot; o controlador ao valor da variável *{Controller}* .</span><span class="sxs-lookup"><span data-stu-id="1e454-134">To find the controller, Web API adds &quot;Controller&quot; to the value of the *{controller}* variable.</span></span>
- <span data-ttu-id="1e454-135">Para localizar a ação, a API Web examina o verbo HTTP e, em seguida, procura uma ação cujo nome começa com esse nome de verbo HTTP.</span><span class="sxs-lookup"><span data-stu-id="1e454-135">To find the action, Web API looks at the HTTP verb, and then looks for an action whose name begins with that HTTP verb name.</span></span> <span data-ttu-id="1e454-136">Por exemplo, com uma solicitação GET, a API da Web procura uma ação prefixada com &quot; Get &quot; , como &quot; GetContact &quot; ou &quot; GetAllContacts &quot; .</span><span class="sxs-lookup"><span data-stu-id="1e454-136">For example, with a GET request, Web API looks for an action prefixed with &quot;Get&quot;, such as &quot;GetContact&quot; or &quot;GetAllContacts&quot;.</span></span> <span data-ttu-id="1e454-137">Essa convenção se aplica somente aos verbos de GET, POST, PUT, excluir, cabeçalho, opções e PATCH.</span><span class="sxs-lookup"><span data-stu-id="1e454-137">This convention applies only to GET, POST, PUT, DELETE, HEAD, OPTIONS, and PATCH verbs.</span></span> <span data-ttu-id="1e454-138">Você pode habilitar outros verbos HTTP usando atributos em seu controlador.</span><span class="sxs-lookup"><span data-stu-id="1e454-138">You can enable other HTTP verbs by using attributes on your controller.</span></span> <span data-ttu-id="1e454-139">Veremos um exemplo disso mais tarde.</span><span class="sxs-lookup"><span data-stu-id="1e454-139">We'll see an example of that later.</span></span>
- <span data-ttu-id="1e454-140">Outras variáveis de espaço reservado no modelo de rota, como *{ID},* são mapeadas para parâmetros de ação.</span><span class="sxs-lookup"><span data-stu-id="1e454-140">Other placeholder variables in the route template, such as *{id},* are mapped to action parameters.</span></span>

<span data-ttu-id="1e454-141">Vejamos um exemplo.</span><span class="sxs-lookup"><span data-stu-id="1e454-141">Let's look at an example.</span></span> <span data-ttu-id="1e454-142">Suponha que você defina o controlador a seguir:</span><span class="sxs-lookup"><span data-stu-id="1e454-142">Suppose that you define the following controller:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="1e454-143">Aqui estão algumas solicitações HTTP possíveis, juntamente com a ação que é invocada para cada uma:</span><span class="sxs-lookup"><span data-stu-id="1e454-143">Here are some possible HTTP requests, along with the action that gets invoked for each:</span></span>

| <span data-ttu-id="1e454-144">Verbo HTTP</span><span class="sxs-lookup"><span data-stu-id="1e454-144">HTTP Verb</span></span> | <span data-ttu-id="1e454-145">Caminho do URI</span><span class="sxs-lookup"><span data-stu-id="1e454-145">URI Path</span></span> | <span data-ttu-id="1e454-146">Ação</span><span class="sxs-lookup"><span data-stu-id="1e454-146">Action</span></span> | <span data-ttu-id="1e454-147">Parâmetro</span><span class="sxs-lookup"><span data-stu-id="1e454-147">Parameter</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="1e454-148">GET</span><span class="sxs-lookup"><span data-stu-id="1e454-148">GET</span></span> | <span data-ttu-id="1e454-149">API/produtos</span><span class="sxs-lookup"><span data-stu-id="1e454-149">api/products</span></span> | <span data-ttu-id="1e454-150">GetAllProducts</span><span class="sxs-lookup"><span data-stu-id="1e454-150">GetAllProducts</span></span> | <span data-ttu-id="1e454-151">*None*</span><span class="sxs-lookup"><span data-stu-id="1e454-151">*(none)*</span></span> |
| <span data-ttu-id="1e454-152">GET</span><span class="sxs-lookup"><span data-stu-id="1e454-152">GET</span></span> | <span data-ttu-id="1e454-153">API/produtos/4</span><span class="sxs-lookup"><span data-stu-id="1e454-153">api/products/4</span></span> | <span data-ttu-id="1e454-154">GetProductById</span><span class="sxs-lookup"><span data-stu-id="1e454-154">GetProductById</span></span> | <span data-ttu-id="1e454-155">4</span><span class="sxs-lookup"><span data-stu-id="1e454-155">4</span></span> |
| <span data-ttu-id="1e454-156">Delete (excluir)</span><span class="sxs-lookup"><span data-stu-id="1e454-156">DELETE</span></span> | <span data-ttu-id="1e454-157">API/produtos/4</span><span class="sxs-lookup"><span data-stu-id="1e454-157">api/products/4</span></span> | <span data-ttu-id="1e454-158">DeleteProduct</span><span class="sxs-lookup"><span data-stu-id="1e454-158">DeleteProduct</span></span> | <span data-ttu-id="1e454-159">4</span><span class="sxs-lookup"><span data-stu-id="1e454-159">4</span></span> |
| <span data-ttu-id="1e454-160">POST</span><span class="sxs-lookup"><span data-stu-id="1e454-160">POST</span></span> | <span data-ttu-id="1e454-161">API/produtos</span><span class="sxs-lookup"><span data-stu-id="1e454-161">api/products</span></span> | <span data-ttu-id="1e454-162">*(sem correspondência)*</span><span class="sxs-lookup"><span data-stu-id="1e454-162">*(no match)*</span></span> |  |

<span data-ttu-id="1e454-163">Observe que o segmento *{ID}* do URI, se presente, está mapeado para o parâmetro *ID* da ação.</span><span class="sxs-lookup"><span data-stu-id="1e454-163">Notice that the *{id}* segment of the URI, if present, is mapped to the *id* parameter of the action.</span></span> <span data-ttu-id="1e454-164">Neste exemplo, o controlador define dois métodos GET, um com um parâmetro de *ID* e outro sem parâmetros.</span><span class="sxs-lookup"><span data-stu-id="1e454-164">In this example, the controller defines two GET methods, one with an *id* parameter and one with no parameters.</span></span>

<span data-ttu-id="1e454-165">Além disso, observe que a solicitação POST falhará, porque o controlador não define um &quot; método post.... &quot;</span><span class="sxs-lookup"><span data-stu-id="1e454-165">Also, note that the POST request will fail, because the controller does not define a &quot;Post...&quot; method.</span></span>

## <a name="routing-variations"></a><span data-ttu-id="1e454-166">Variações de roteamento</span><span class="sxs-lookup"><span data-stu-id="1e454-166">Routing Variations</span></span>

<span data-ttu-id="1e454-167">A seção anterior descreveu o mecanismo de roteamento básico para ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="1e454-167">The previous section described the basic routing mechanism for ASP.NET Web API.</span></span> <span data-ttu-id="1e454-168">Esta seção descreve algumas variações.</span><span class="sxs-lookup"><span data-stu-id="1e454-168">This section describes some variations.</span></span>

### <a name="http-verbs"></a><span data-ttu-id="1e454-169">Verbos HTTP</span><span class="sxs-lookup"><span data-stu-id="1e454-169">HTTP verbs</span></span>

<span data-ttu-id="1e454-170">Em vez de usar a Convenção de nomenclatura para verbos HTTP, você pode especificar explicitamente o verbo HTTP para uma ação decorando o método de ação com um dos seguintes atributos:</span><span class="sxs-lookup"><span data-stu-id="1e454-170">Instead of using the naming convention for HTTP verbs, you can explicitly specify the HTTP verb for an action by decorating the action method with one of the following attributes:</span></span>

- `[HttpGet]`
- `[HttpPut]`
- `[HttpPost]`
- `[HttpDelete]`
- `[HttpHead]`
- `[HttpOptions]`
- `[HttpPatch]`

<span data-ttu-id="1e454-171">No exemplo a seguir, o `FindProduct` método é mapeado para solicitações GET:</span><span class="sxs-lookup"><span data-stu-id="1e454-171">In the following example, the `FindProduct` method is mapped to GET requests:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="1e454-172">Para permitir vários verbos HTTP para uma ação ou para permitir verbos HTTP diferentes de GET, PUT, POST, DELETE, HEAD, OPTIONS e PATCH, use o `[AcceptVerbs]` atributo, que usa uma lista de VERBOS http.</span><span class="sxs-lookup"><span data-stu-id="1e454-172">To allow multiple HTTP verbs for an action, or to allow HTTP verbs other than GET, PUT, POST, DELETE, HEAD, OPTIONS, and PATCH, use the `[AcceptVerbs]` attribute, which takes a list of HTTP verbs.</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample4.cs)]

<a id="routing_by_action_name"></a>
### <a name="routing-by-action-name"></a><span data-ttu-id="1e454-173">Roteamento por nome de ação</span><span class="sxs-lookup"><span data-stu-id="1e454-173">Routing by Action Name</span></span>

<span data-ttu-id="1e454-174">Com o modelo de roteamento padrão, a API da Web usa o verbo HTTP para selecionar a ação.</span><span class="sxs-lookup"><span data-stu-id="1e454-174">With the default routing template, Web API uses the HTTP verb to select the action.</span></span> <span data-ttu-id="1e454-175">No entanto, você também pode criar uma rota onde o nome da ação está incluído no URI:</span><span class="sxs-lookup"><span data-stu-id="1e454-175">However, you can also create a route where the action name is included in the URI:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="1e454-176">Nesse modelo de rota, o parâmetro *{Action}* nomeia o método de ação no controlador.</span><span class="sxs-lookup"><span data-stu-id="1e454-176">In this route template, the *{action}* parameter names the action method on the controller.</span></span> <span data-ttu-id="1e454-177">Com esse estilo de roteamento, use atributos para especificar os verbos HTTP permitidos.</span><span class="sxs-lookup"><span data-stu-id="1e454-177">With this style of routing, use attributes to specify the allowed HTTP verbs.</span></span> <span data-ttu-id="1e454-178">Por exemplo, suponha que o controlador tenha o seguinte método:</span><span class="sxs-lookup"><span data-stu-id="1e454-178">For example, suppose your controller has the following method:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="1e454-179">Nesse caso, uma solicitação GET para "API/produtos/detalhes/1" mapearia para o `Details` método.</span><span class="sxs-lookup"><span data-stu-id="1e454-179">In this case, a GET request for "api/products/details/1" would map to the `Details` method.</span></span> <span data-ttu-id="1e454-180">Esse estilo de roteamento é semelhante ao ASP.NET MVC e pode ser apropriado para uma API de estilo RPC.</span><span class="sxs-lookup"><span data-stu-id="1e454-180">This style of routing is similar to ASP.NET MVC, and may be appropriate for an RPC-style API.</span></span>

<span data-ttu-id="1e454-181">Você pode substituir o nome da ação usando o `[ActionName]` atributo.</span><span class="sxs-lookup"><span data-stu-id="1e454-181">You can override the action name by using the `[ActionName]` attribute.</span></span> <span data-ttu-id="1e454-182">No exemplo a seguir, há duas ações que são mapeadas para &quot; API/Products/thumbnail/*ID*. Um é compatível com GET e o outro oferece suporte a POST:</span><span class="sxs-lookup"><span data-stu-id="1e454-182">In the following example, there are two actions that map to &quot;api/products/thumbnail/*id*. One supports GET and the other supports POST:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample7.cs)]

### <a name="non-actions"></a><span data-ttu-id="1e454-183">Não ações</span><span class="sxs-lookup"><span data-stu-id="1e454-183">Non-Actions</span></span>

<span data-ttu-id="1e454-184">Para impedir que um método seja invocado como uma ação, use o `[NonAction]` atributo.</span><span class="sxs-lookup"><span data-stu-id="1e454-184">To prevent a method from getting invoked as an action, use the `[NonAction]` attribute.</span></span> <span data-ttu-id="1e454-185">Isso sinaliza para a estrutura que o método não é uma ação, mesmo que, caso contrário, correspondam às regras de roteamento.</span><span class="sxs-lookup"><span data-stu-id="1e454-185">This signals to the framework that the method is not an action, even if it would otherwise match the routing rules.</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample8.cs)]

## <a name="further-reading"></a><span data-ttu-id="1e454-186">Leitura Adicional</span><span class="sxs-lookup"><span data-stu-id="1e454-186">Further Reading</span></span>

<span data-ttu-id="1e454-187">Este tópico forneceu uma exibição de alto nível do roteamento.</span><span class="sxs-lookup"><span data-stu-id="1e454-187">This topic provided a high-level view of routing.</span></span> <span data-ttu-id="1e454-188">Para obter mais detalhes, consulte [seleção de roteamento e ação](routing-and-action-selection.md), que descreve exatamente como a estrutura corresponde a um URI para uma rota, seleciona um controlador e, em seguida, seleciona a ação a ser invocada.</span><span class="sxs-lookup"><span data-stu-id="1e454-188">For more detail, see [Routing and Action Selection](routing-and-action-selection.md), which describes exactly how the framework matches a URI to a route, selects a controller, and then selects the action to invoke.</span></span>
