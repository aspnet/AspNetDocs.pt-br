---
uid: web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
title: Suporte a opções de consulta OData no ASP.NET Web API 2-ASP.NET 4. x
author: MikeWasson
description: Visão geral com exemplos de código mostra as opções de consulta OData de suporte no ASP.NET Web API 2 para ASP.NET 4. x.
ms.author: riande
ms.date: 02/04/2013
ms.custom: seoapril2019
ms.assetid: 50e6e62b-e72e-4a29-8293-4b67377bd21f
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
msc.type: authoredcontent
ms.openlocfilehash: 96820fab7ac89885058962f44ded86cb0184ee97
ms.sourcegitcommit: 4ed0b65ae32d9f35e42ee6296b877747e063df4d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2020
ms.locfileid: "86188601"
---
# <a name="supporting-odata-query-options-in-aspnet-web-api-2"></a><span data-ttu-id="8874d-103">Suporte a opções de consulta OData no ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="8874d-103">Supporting OData Query Options in ASP.NET Web API 2</span></span>

<span data-ttu-id="8874d-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="8874d-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="8874d-105">Esta visão geral com exemplos de código demonstra as opções de consulta OData de suporte no ASP.NET Web API 2 para ASP.NET 4. x.</span><span class="sxs-lookup"><span data-stu-id="8874d-105">This overview with code examples demonstrates the supporting OData Query Options in ASP.NET Web API 2 for ASP.NET 4.x.</span></span> 

<span data-ttu-id="8874d-106">O OData define parâmetros que podem ser usados para modificar uma consulta OData.</span><span class="sxs-lookup"><span data-stu-id="8874d-106">OData defines parameters that can be used to modify an OData query.</span></span> <span data-ttu-id="8874d-107">O cliente envia esses parâmetros na cadeia de caracteres de consulta do URI de solicitação.</span><span class="sxs-lookup"><span data-stu-id="8874d-107">The client sends these parameters in the query string of the request URI.</span></span> <span data-ttu-id="8874d-108">Por exemplo, para classificar os resultados, um cliente usa o parâmetro $orderby:</span><span class="sxs-lookup"><span data-stu-id="8874d-108">For example, to sort the results, a client uses the $orderby parameter:</span></span>

`http://localhost/Products?$orderby=Name`

<span data-ttu-id="8874d-109">A especificação OData chama essas *Opções de consulta*de parâmetros.</span><span class="sxs-lookup"><span data-stu-id="8874d-109">The OData specification calls these parameters *query options*.</span></span> <span data-ttu-id="8874d-110">Você pode habilitar as opções de consulta OData para qualquer controlador de API Web em seu projeto &#8212; o controlador não precisa ser um ponto de extremidade OData.</span><span class="sxs-lookup"><span data-stu-id="8874d-110">You can enable OData query options for any Web API controller in your project &#8212; the controller does not need to be an OData endpoint.</span></span> <span data-ttu-id="8874d-111">Isso lhe dá uma maneira conveniente de adicionar recursos como filtragem e classificação a qualquer aplicativo de API Web.</span><span class="sxs-lookup"><span data-stu-id="8874d-111">This gives you a convenient way to add features such as filtering and sorting to any Web API application.</span></span>

<span data-ttu-id="8874d-112">Antes de habilitar as opções de consulta, leia o tópico [diretrizes de segurança do OData](odata-security-guidance.md).</span><span class="sxs-lookup"><span data-stu-id="8874d-112">Before enabling query options, please read the topic [OData Security Guidance](odata-security-guidance.md).</span></span>

- [<span data-ttu-id="8874d-113">Habilitando opções de consulta OData</span><span class="sxs-lookup"><span data-stu-id="8874d-113">Enabling OData Query Options</span></span>](#enable)
- [<span data-ttu-id="8874d-114">Exemplos de consultas</span><span class="sxs-lookup"><span data-stu-id="8874d-114">Example Queries</span></span>](#examples)
- [<span data-ttu-id="8874d-115">Paginação controlada por servidor</span><span class="sxs-lookup"><span data-stu-id="8874d-115">Server-Driven Paging</span></span>](#server-paging)
- [<span data-ttu-id="8874d-116">Limitando as opções de consulta</span><span class="sxs-lookup"><span data-stu-id="8874d-116">Limiting the Query Options</span></span>](#limiting_query_options)
- [<span data-ttu-id="8874d-117">Invocando opções de consulta diretamente</span><span class="sxs-lookup"><span data-stu-id="8874d-117">Invoking Query Options Directly</span></span>](#ODataQueryOptions)
- [<span data-ttu-id="8874d-118">Validação de consulta</span><span class="sxs-lookup"><span data-stu-id="8874d-118">Query Validation</span></span>](#query-validation)

<a id="enable"></a>
## <a name="enabling-odata-query-options"></a><span data-ttu-id="8874d-119">Habilitando opções de consulta OData</span><span class="sxs-lookup"><span data-stu-id="8874d-119">Enabling OData Query Options</span></span>

<span data-ttu-id="8874d-120">A API Web dá suporte às seguintes opções de consulta OData:</span><span class="sxs-lookup"><span data-stu-id="8874d-120">Web API supports the following OData query options:</span></span>

| <span data-ttu-id="8874d-121">Opção</span><span class="sxs-lookup"><span data-stu-id="8874d-121">Option</span></span> | <span data-ttu-id="8874d-122">Descrição</span><span class="sxs-lookup"><span data-stu-id="8874d-122">Description</span></span> |
| --- | --- |
| <span data-ttu-id="8874d-123">$expand</span><span class="sxs-lookup"><span data-stu-id="8874d-123">$expand</span></span> | <span data-ttu-id="8874d-124">Expande as entidades relacionadas embutidas.</span><span class="sxs-lookup"><span data-stu-id="8874d-124">Expands related entities inline.</span></span> |
| <span data-ttu-id="8874d-125">$filter</span><span class="sxs-lookup"><span data-stu-id="8874d-125">$filter</span></span> | <span data-ttu-id="8874d-126">Filtra os resultados, com base em uma condição booliana.</span><span class="sxs-lookup"><span data-stu-id="8874d-126">Filters the results, based on a Boolean condition.</span></span> |
| <span data-ttu-id="8874d-127">$inlinecount</span><span class="sxs-lookup"><span data-stu-id="8874d-127">$inlinecount</span></span> | <span data-ttu-id="8874d-128">Instrui o servidor a incluir a contagem total de entidades correspondentes na resposta.</span><span class="sxs-lookup"><span data-stu-id="8874d-128">Tells the server to include the total count of matching entities in the response.</span></span> <span data-ttu-id="8874d-129">(Útil para paginação do lado do servidor).</span><span class="sxs-lookup"><span data-stu-id="8874d-129">(Useful for server-side paging.)</span></span> |
| <span data-ttu-id="8874d-130">$orderby</span><span class="sxs-lookup"><span data-stu-id="8874d-130">$orderby</span></span> | <span data-ttu-id="8874d-131">Classifica os resultados.</span><span class="sxs-lookup"><span data-stu-id="8874d-131">Sorts the results.</span></span> |
| <span data-ttu-id="8874d-132">$select</span><span class="sxs-lookup"><span data-stu-id="8874d-132">$select</span></span> | <span data-ttu-id="8874d-133">Seleciona quais propriedades incluir na resposta.</span><span class="sxs-lookup"><span data-stu-id="8874d-133">Selects which properties to include in the response.</span></span> |
| <span data-ttu-id="8874d-134">$skip</span><span class="sxs-lookup"><span data-stu-id="8874d-134">$skip</span></span> | <span data-ttu-id="8874d-135">Ignora os primeiros n resultados.</span><span class="sxs-lookup"><span data-stu-id="8874d-135">Skips the first n results.</span></span> |
| <span data-ttu-id="8874d-136">$top</span><span class="sxs-lookup"><span data-stu-id="8874d-136">$top</span></span> | <span data-ttu-id="8874d-137">Retorna apenas os n primeiros resultados.</span><span class="sxs-lookup"><span data-stu-id="8874d-137">Returns only the first n the results.</span></span> |

<span data-ttu-id="8874d-138">Para usar as opções de consulta OData, você deve habilitá-las explicitamente.</span><span class="sxs-lookup"><span data-stu-id="8874d-138">To use OData query options, you must enable them explicitly.</span></span> <span data-ttu-id="8874d-139">Você pode habilitá-los globalmente para todo o aplicativo ou habilitá-los para controladores específicos ou ações específicas.</span><span class="sxs-lookup"><span data-stu-id="8874d-139">You can enable them globally for the entire application, or enable them for specific controllers or specific actions.</span></span>

<span data-ttu-id="8874d-140">Para habilitar as opções de consulta OData globalmente, chame **EnableQuerySupport** na classe **HttpConfiguration** na inicialização:</span><span class="sxs-lookup"><span data-stu-id="8874d-140">To enable OData query options globally, call **EnableQuerySupport** on the **HttpConfiguration** class at startup:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample1.cs)]

<span data-ttu-id="8874d-141">O método **EnableQuerySupport** habilita as opções de consulta globalmente para qualquer ação do controlador que retorna um tipo **IQueryable** .</span><span class="sxs-lookup"><span data-stu-id="8874d-141">The **EnableQuerySupport** method enables query options globally for any controller action that returns an **IQueryable** type.</span></span> <span data-ttu-id="8874d-142">Se você não quiser que as opções de consulta sejam habilitadas para todo o aplicativo, poderá habilitá-las para ações de controlador específicas adicionando o atributo **[consultável]** ao método de ação.</span><span class="sxs-lookup"><span data-stu-id="8874d-142">If you don't want query options enabled for the entire application, you can enable them for specific controller actions by adding the **[Queryable]** attribute to the action method.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample2.cs)]

<a id="examples"></a>
## <a name="example-queries"></a><span data-ttu-id="8874d-143">Consultas de Exemplo</span><span class="sxs-lookup"><span data-stu-id="8874d-143">Example Queries</span></span>

<span data-ttu-id="8874d-144">Esta seção mostra os tipos de consultas que são possíveis usando as opções de consulta OData.</span><span class="sxs-lookup"><span data-stu-id="8874d-144">This section shows the types of queries that are possible using the OData query options.</span></span> <span data-ttu-id="8874d-145">Para obter detalhes específicos sobre as opções de consulta, consulte a documentação do OData em [www.OData.org](http://www.odata.org/).</span><span class="sxs-lookup"><span data-stu-id="8874d-145">For specific details about the query options, refer to the OData documentation at [www.odata.org](http://www.odata.org/).</span></span>

<span data-ttu-id="8874d-146">Para obter informações sobre $expand e $select, consulte [usando $Select, $Expand e $Value no ASP.NET Web API OData](using-select-expand-and-value.md).</span><span class="sxs-lookup"><span data-stu-id="8874d-146">For information about $expand and $select, see [Using $select, $expand, and $value in ASP.NET Web API OData](using-select-expand-and-value.md).</span></span>

<span data-ttu-id="8874d-147">**Paginação controlada pelo cliente**</span><span class="sxs-lookup"><span data-stu-id="8874d-147">**Client-Driven Paging**</span></span>

<span data-ttu-id="8874d-148">Para conjuntos de entidades grandes, o cliente pode querer limitar o número de resultados.</span><span class="sxs-lookup"><span data-stu-id="8874d-148">For large entity sets, the client might want to limit the number of results.</span></span> <span data-ttu-id="8874d-149">Por exemplo, um cliente pode mostrar 10 entradas por vez, com links "Next" para obter a próxima página de resultados.</span><span class="sxs-lookup"><span data-stu-id="8874d-149">For example, a client might show 10 entries at a time, with "next" links to get the next page of results.</span></span> <span data-ttu-id="8874d-150">Para fazer isso, o cliente usa as opções $top e $skip.</span><span class="sxs-lookup"><span data-stu-id="8874d-150">To do this, the client uses the $top and $skip options.</span></span>

`http://localhost/Products?$top=10&$skip=20`

<span data-ttu-id="8874d-151">A opção $top fornece o número máximo de entradas a serem retornadas e a opção $skip fornece o número de entradas a serem ignoradas.</span><span class="sxs-lookup"><span data-stu-id="8874d-151">The $top option gives the maximum number of entries to return, and the $skip option gives the number of entries to skip.</span></span> <span data-ttu-id="8874d-152">O exemplo anterior busca as entradas 21 a 30.</span><span class="sxs-lookup"><span data-stu-id="8874d-152">The previous example fetches entries 21 through 30.</span></span>

<span data-ttu-id="8874d-153">**Filtragem**</span><span class="sxs-lookup"><span data-stu-id="8874d-153">**Filtering**</span></span>

<span data-ttu-id="8874d-154">A opção $filter permite que um cliente filtre os resultados aplicando uma expressão booliana.</span><span class="sxs-lookup"><span data-stu-id="8874d-154">The $filter option lets a client filter the results by applying a Boolean expression.</span></span> <span data-ttu-id="8874d-155">As expressões de filtro são bastante poderosas; Eles incluem operadores lógicos e aritméticos, funções de cadeia de caracteres e funções de data.</span><span class="sxs-lookup"><span data-stu-id="8874d-155">The filter expressions are quite powerful; they include logical and arithmetic operators, string functions, and date functions.</span></span>

| <span data-ttu-id="8874d-156">Retorne todos os produtos com categoria igual a "brinquedos".</span><span class="sxs-lookup"><span data-stu-id="8874d-156">Return all products with category equal to "Toys".</span></span> | <span data-ttu-id="8874d-157">`http://localhost/Products?$filter=Category` EQ ' Toys '</span><span class="sxs-lookup"><span data-stu-id="8874d-157">`http://localhost/Products?$filter=Category` eq 'Toys'</span></span> |
| --- | --- |
| <span data-ttu-id="8874d-158">Retornar todos os produtos com o preço inferior a 10.</span><span class="sxs-lookup"><span data-stu-id="8874d-158">Return all products with price less than 10.</span></span> | <span data-ttu-id="8874d-159">`http://localhost/Products?$filter=Price` lt 10</span><span class="sxs-lookup"><span data-stu-id="8874d-159">`http://localhost/Products?$filter=Price` lt 10</span></span> |
| <span data-ttu-id="8874d-160">Operadores lógicos: retorna todos os produtos em que Price >= 5 e Price <= 15.</span><span class="sxs-lookup"><span data-stu-id="8874d-160">Logical operators: Return all products where price >= 5 and price <= 15.</span></span> | <span data-ttu-id="8874d-161">`http://localhost/Products?$filter=Price` GE 5 e preço Le 15</span><span class="sxs-lookup"><span data-stu-id="8874d-161">`http://localhost/Products?$filter=Price` ge 5 and Price le 15</span></span> |
| <span data-ttu-id="8874d-162">Funções de cadeia de caracteres: retorna todos os produtos com "zz" no nome.</span><span class="sxs-lookup"><span data-stu-id="8874d-162">String functions: Return all products with "zz" in the name.</span></span> | `http://localhost/Products?$filter=substringof('zz',Name)` |
| <span data-ttu-id="8874d-163">Funções de data: retorna todos os produtos com liberados após 2005.</span><span class="sxs-lookup"><span data-stu-id="8874d-163">Date functions: Return all products with ReleaseDate after 2005.</span></span> | <span data-ttu-id="8874d-164">`http://localhost/Products?$filter=year(ReleaseDate)` gt 2005</span><span class="sxs-lookup"><span data-stu-id="8874d-164">`http://localhost/Products?$filter=year(ReleaseDate)` gt 2005</span></span> |

<span data-ttu-id="8874d-165">**Classificação**</span><span class="sxs-lookup"><span data-stu-id="8874d-165">**Sorting**</span></span>

<span data-ttu-id="8874d-166">Para classificar os resultados, use o filtro $orderby.</span><span class="sxs-lookup"><span data-stu-id="8874d-166">To sort the results, use the $orderby filter.</span></span>

| <span data-ttu-id="8874d-167">Classificar por preço.</span><span class="sxs-lookup"><span data-stu-id="8874d-167">Sort by price.</span></span> | `http://localhost/Products?$orderby=Price` |
| --- | --- |
| <span data-ttu-id="8874d-168">Classificar por preço em ordem decrescente (mais alto para o mais baixo).</span><span class="sxs-lookup"><span data-stu-id="8874d-168">Sort by price in descending order (highest to lowest).</span></span> | `http://localhost/Products?$orderby=Price desc` |
| <span data-ttu-id="8874d-169">Classifique por categoria e, em seguida, classifique por preço em ordem decrescente dentro de categorias.</span><span class="sxs-lookup"><span data-stu-id="8874d-169">Sort by category, then sort by price in descending order within categories.</span></span> | `http://localhost/odata/Products?$orderby=Category,Price desc` |

<a id="server-paging"></a>
## <a name="server-driven-paging"></a><span data-ttu-id="8874d-170">Paginação controlada por servidor</span><span class="sxs-lookup"><span data-stu-id="8874d-170">Server-Driven Paging</span></span>

<span data-ttu-id="8874d-171">Se o seu banco de dados contiver milhões de registros, você não desejará enviar todos eles em uma carga.</span><span class="sxs-lookup"><span data-stu-id="8874d-171">If your database contains millions of records, you don't want to send them all in one payload.</span></span> <span data-ttu-id="8874d-172">Para evitar isso, o servidor pode limitar o número de entradas que ele envia em uma única resposta.</span><span class="sxs-lookup"><span data-stu-id="8874d-172">To prevent this, the server can limit the number of entries that it sends in a single response.</span></span> <span data-ttu-id="8874d-173">Para habilitar a paginação do servidor, defina a propriedade **PageSize** no atributo **passível** de consulta.</span><span class="sxs-lookup"><span data-stu-id="8874d-173">To enable server paging, set the **PageSize** property in the **Queryable** attribute.</span></span> <span data-ttu-id="8874d-174">O valor é o número máximo de entradas a serem retornadas.</span><span class="sxs-lookup"><span data-stu-id="8874d-174">The value is the maximum number of entries to return.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample3.cs)]

<span data-ttu-id="8874d-175">Se o controlador retornar o formato OData, o corpo da resposta conterá um link para a próxima página de dados:</span><span class="sxs-lookup"><span data-stu-id="8874d-175">If your controller returns OData format, the response body will contain a link to the next page of data:</span></span>

[!code-json[Main](supporting-odata-query-options/samples/sample4.json?highlight=8)]

<span data-ttu-id="8874d-176">O cliente pode usar esse link para buscar a próxima página.</span><span class="sxs-lookup"><span data-stu-id="8874d-176">The client can use this link to fetch the next page.</span></span> <span data-ttu-id="8874d-177">Para saber o número total de entradas no conjunto de resultados, o cliente pode definir a opção de consulta $inlinecount com o valor "Alpages".</span><span class="sxs-lookup"><span data-stu-id="8874d-177">To learn the total number of entries in the result set, the client can set the $inlinecount query option with the value "allpages".</span></span>

`http://localhost/Products?$inlinecount=allpages`

<span data-ttu-id="8874d-178">O valor "MyPages" informa ao servidor para incluir a contagem total na resposta:</span><span class="sxs-lookup"><span data-stu-id="8874d-178">The value "allpages" tells the server to include the total count in the response:</span></span>

[!code-json[Main](supporting-odata-query-options/samples/sample5.json?highlight=3)]

> [!NOTE]
> <span data-ttu-id="8874d-179">Os links de próxima página e a contagem embutida exigem o formato OData.</span><span class="sxs-lookup"><span data-stu-id="8874d-179">Next-page links and inline count both require OData format.</span></span> <span data-ttu-id="8874d-180">O motivo é que o OData define campos especiais no corpo da resposta para manter o link e a contagem.</span><span class="sxs-lookup"><span data-stu-id="8874d-180">The reason is that OData defines special fields in the response body to hold the link and count.</span></span>

<span data-ttu-id="8874d-181">Para formatos não-OData, ainda é possível dar suporte a links de próxima página e contagem embutida, encapsulando os resultados da consulta em um objeto \*\*PageResult &lt; T &gt; \*\* .</span><span class="sxs-lookup"><span data-stu-id="8874d-181">For non-OData formats, it is still possible to support next-page links and inline count, by wrapping the query results in a **PageResult&lt;T&gt;** object.</span></span> <span data-ttu-id="8874d-182">No entanto, ele requer um pouco mais de código.</span><span class="sxs-lookup"><span data-stu-id="8874d-182">However, it requires a bit more code.</span></span> <span data-ttu-id="8874d-183">Veja um exemplo:</span><span class="sxs-lookup"><span data-stu-id="8874d-183">Here is an example:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample6.cs)]

<span data-ttu-id="8874d-184">Aqui está um exemplo de resposta JSON:</span><span class="sxs-lookup"><span data-stu-id="8874d-184">Here is an example JSON response:</span></span>

[!code-json[Main](supporting-odata-query-options/samples/sample7.json)]

<a id="limiting_query_options"></a>
## <a name="limiting-the-query-options"></a><span data-ttu-id="8874d-185">Limitando as opções de consulta</span><span class="sxs-lookup"><span data-stu-id="8874d-185">Limiting the Query Options</span></span>

<span data-ttu-id="8874d-186">As opções de consulta dão ao cliente muito controle sobre a consulta que é executada no servidor.</span><span class="sxs-lookup"><span data-stu-id="8874d-186">The query options give the client a lot of control over the query that is run on the server.</span></span> <span data-ttu-id="8874d-187">Em alguns casos, talvez você queira limitar as opções disponíveis por motivos de segurança ou desempenho.</span><span class="sxs-lookup"><span data-stu-id="8874d-187">In some cases, you might want to limit the available options for security or performance reasons.</span></span> <span data-ttu-id="8874d-188">O atributo **[passível** de consulta] tem algumas propriedades internas para isso.</span><span class="sxs-lookup"><span data-stu-id="8874d-188">The **[Queryable]** attribute has some built in properties for this.</span></span> <span data-ttu-id="8874d-189">Veja alguns exemplos.</span><span class="sxs-lookup"><span data-stu-id="8874d-189">Here are some examples.</span></span>

<span data-ttu-id="8874d-190">Permitir somente $skip e $top, para dar suporte à paginação e nada mais:</span><span class="sxs-lookup"><span data-stu-id="8874d-190">Allow only $skip and $top, to support paging and nothing else:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample8.cs)]

<span data-ttu-id="8874d-191">Permitir a ordenação apenas de determinadas propriedades, para evitar a classificação em propriedades que não são indexadas no banco de dados:</span><span class="sxs-lookup"><span data-stu-id="8874d-191">Allow ordering only by certain properties, to prevent sorting on properties that are not indexed in the database:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample9.cs)]

<span data-ttu-id="8874d-192">Permita a função lógica "eq", mas não outras funções lógicas:</span><span class="sxs-lookup"><span data-stu-id="8874d-192">Allow the "eq" logical function but no other logical functions:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample10.cs)]

<span data-ttu-id="8874d-193">Não permitir operadores aritméticos:</span><span class="sxs-lookup"><span data-stu-id="8874d-193">Do not allow any arithmetic operators:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample11.cs)]

<span data-ttu-id="8874d-194">Você pode restringir as opções globalmente criando uma instância **passível** de consulta e passando-a para a função **EnableQuerySupport** :</span><span class="sxs-lookup"><span data-stu-id="8874d-194">You can restrict options globally by constructing a **QueryableAttribute** instance and passing it to the **EnableQuerySupport** function:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample12.cs)]

<a id="ODataQueryOptions"></a>
## <a name="invoking-query-options-directly"></a><span data-ttu-id="8874d-195">Invocando opções de consulta diretamente</span><span class="sxs-lookup"><span data-stu-id="8874d-195">Invoking Query Options Directly</span></span>

<span data-ttu-id="8874d-196">Em vez de usar o atributo **[consultável]** , você pode invocar as opções de consulta diretamente em seu controlador.</span><span class="sxs-lookup"><span data-stu-id="8874d-196">Instead of using the **[Queryable]** attribute, you can invoke the query options directly in your controller.</span></span> <span data-ttu-id="8874d-197">Para fazer isso, adicione um parâmetro **ODataQueryOptions** ao método Controller.</span><span class="sxs-lookup"><span data-stu-id="8874d-197">To do so, add an **ODataQueryOptions** parameter to the controller method.</span></span> <span data-ttu-id="8874d-198">Nesse caso, você não precisa do atributo **[consultável]** .</span><span class="sxs-lookup"><span data-stu-id="8874d-198">In this case, you don't need the **[Queryable]** attribute.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample13.cs)]

<span data-ttu-id="8874d-199">A API Web popula o **ODataQueryOptions** da cadeia de caracteres de consulta do URI.</span><span class="sxs-lookup"><span data-stu-id="8874d-199">Web API populates the **ODataQueryOptions** from the URI query string.</span></span> <span data-ttu-id="8874d-200">Para aplicar a consulta, passe um **IQueryable** para o método **ApplyTo** .</span><span class="sxs-lookup"><span data-stu-id="8874d-200">To apply the query, pass an **IQueryable** to the **ApplyTo** method.</span></span> <span data-ttu-id="8874d-201">O método retorna outro **IQueryable**.</span><span class="sxs-lookup"><span data-stu-id="8874d-201">The method returns another **IQueryable**.</span></span>

<span data-ttu-id="8874d-202">Para cenários avançados, se você não tiver um provedor de consultas **IQueryable** , poderá examinar o **ODataQueryOptions** e converter as opções de consulta em outro formulário.</span><span class="sxs-lookup"><span data-stu-id="8874d-202">For advanced scenarios, if you do not have an **IQueryable** query provider, you can examine the **ODataQueryOptions** and translate the query options into another form.</span></span> <span data-ttu-id="8874d-203">(Por exemplo, consulte postagem de blog de RaghuRam Nadiminti [traduzindo consultas OData para HQL](https://blogs.msdn.com/b/webdev/archive/2013/02/25/translating-odata-queries-to-hql.aspx), que também inclui um [exemplo](http://aspnet.codeplex.com/SourceControl/changeset/view/75a56ec99968#Samples/WebApi/NHibernateQueryableSample/Readme.txt).)</span><span class="sxs-lookup"><span data-stu-id="8874d-203">(For example, see RaghuRam Nadiminti's blog post [Translating OData queries to HQL](https://blogs.msdn.com/b/webdev/archive/2013/02/25/translating-odata-queries-to-hql.aspx), which also includes a [sample](http://aspnet.codeplex.com/SourceControl/changeset/view/75a56ec99968#Samples/WebApi/NHibernateQueryableSample/Readme.txt).)</span></span>

<a id="query-validation"></a>
## <a name="query-validation"></a><span data-ttu-id="8874d-204">Validação de consulta</span><span class="sxs-lookup"><span data-stu-id="8874d-204">Query Validation</span></span>

<span data-ttu-id="8874d-205">O atributo **[consultável]** valida a consulta antes de executá-la.</span><span class="sxs-lookup"><span data-stu-id="8874d-205">The **[Queryable]** attribute validates the query before executing it.</span></span> <span data-ttu-id="8874d-206">A etapa de validação é executada no método **consultáattribute. ValidateQuery** .</span><span class="sxs-lookup"><span data-stu-id="8874d-206">The validation step is performed in the **QueryableAttribute.ValidateQuery** method.</span></span> <span data-ttu-id="8874d-207">Você também pode personalizar o processo de validação.</span><span class="sxs-lookup"><span data-stu-id="8874d-207">You can also customize the validation process.</span></span>

<span data-ttu-id="8874d-208">Consulte também as [diretrizes de segurança do OData](odata-security-guidance.md).</span><span class="sxs-lookup"><span data-stu-id="8874d-208">Also see [OData Security Guidance](odata-security-guidance.md).</span></span>

<span data-ttu-id="8874d-209">Primeiro, substitua uma das classes do validador que está definida no namespace **Web. http. OData. Query. Validators** .</span><span class="sxs-lookup"><span data-stu-id="8874d-209">First, override one of the validator classes that is defined in the **Web.Http.OData.Query.Validators** namespace.</span></span> <span data-ttu-id="8874d-210">Por exemplo, a seguinte classe de validador desabilita a opção ' DESC ' para a opção $orderby.</span><span class="sxs-lookup"><span data-stu-id="8874d-210">For example, the following validator class disables the 'desc' option for the $orderby option.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample14.cs)]

<span data-ttu-id="8874d-211">Subclasse o atributo **[passível** de consulta] para substituir o método **ValidateQuery** .</span><span class="sxs-lookup"><span data-stu-id="8874d-211">Subclass the **[Queryable]** attribute to override the **ValidateQuery** method.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample15.cs)]

<span data-ttu-id="8874d-212">Em seguida, defina seu atributo personalizado globalmente ou por controlador:</span><span class="sxs-lookup"><span data-stu-id="8874d-212">Then set your custom attribute either globally or per-controller:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample16.cs)]

<span data-ttu-id="8874d-213">Se você estiver usando o **ODataQueryOptions** diretamente, defina o validador nas opções:</span><span class="sxs-lookup"><span data-stu-id="8874d-213">If you are using **ODataQueryOptions** directly, set the validator on the options:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample17.cs)]
