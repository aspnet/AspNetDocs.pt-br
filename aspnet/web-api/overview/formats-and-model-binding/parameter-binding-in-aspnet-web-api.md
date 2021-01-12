---
uid: web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
title: Associação de parâmetro em ASP.NET Web API-ASP.NET 4. x
author: MikeWasson
description: Descreve como a API Web associa parâmetros e como personalizar o processo de associação no ASP.NET 4. x.
ms.author: riande
ms.date: 07/11/2013
ms.custom: seoapril2019
ms.assetid: e42c8388-04ed-4341-9fdb-41b1b4c06320
msc.legacyurl: /web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: da79a01b8493d489aced016d46e228b7f8186127
ms.sourcegitcommit: d4e2a07eeb2cdf19f0bfbfab4a469970bc7e1c99
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/12/2021
ms.locfileid: "98105254"
---
# <a name="parameter-binding-in-aspnet-web-api"></a><span data-ttu-id="db494-103">Associação de parâmetro no ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="db494-103">Parameter Binding in ASP.NET Web API</span></span>

<span data-ttu-id="db494-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="db494-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[!INCLUDE[](~/includes/coreWebAPI.md)]

<span data-ttu-id="db494-105">Este artigo descreve como a API Web associa parâmetros e como você pode personalizar o processo de associação.</span><span class="sxs-lookup"><span data-stu-id="db494-105">This article describes how Web API binds parameters, and how you can customize the binding process.</span></span> <span data-ttu-id="db494-106">Quando a API Web chama um método em um controlador, ela deve definir valores para os parâmetros, um processo chamado *Binding*.</span><span class="sxs-lookup"><span data-stu-id="db494-106">When Web API calls a method on a controller, it must set values for the parameters, a process called *binding*.</span></span>

<span data-ttu-id="db494-107">Por padrão, a API Web usa as seguintes regras para associar parâmetros:</span><span class="sxs-lookup"><span data-stu-id="db494-107">By default, Web API uses the following rules to bind parameters:</span></span>

- <span data-ttu-id="db494-108">Se o parâmetro for um tipo "simples", a API Web tentará obter o valor do URI.</span><span class="sxs-lookup"><span data-stu-id="db494-108">If the parameter is a "simple" type, Web API tries to get the value from the URI.</span></span> <span data-ttu-id="db494-109">Os tipos simples incluem os [tipos primitivos](https://msdn.microsoft.com/library/system.type.isprimitive.aspx) do .net (**int**, **bool**, **Double** e assim por diante), mais **TimeSpan**, **DateTime**, **GUID**, **decimal** e **String**, *além* de qualquer tipo com um conversor de tipo que possa converter de uma cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="db494-109">Simple types include the .NET [primitive types](https://msdn.microsoft.com/library/system.type.isprimitive.aspx) (**int**, **bool**, **double**, and so forth), plus **TimeSpan**, **DateTime**, **Guid**, **decimal**, and **string**, *plus* any type with a type converter that can convert from a string.</span></span> <span data-ttu-id="db494-110">(Saiba mais sobre os conversores de tipo mais tarde.)</span><span class="sxs-lookup"><span data-stu-id="db494-110">(More about type converters later.)</span></span>
- <span data-ttu-id="db494-111">Para tipos complexos, a API da Web tenta ler o valor do corpo da mensagem, usando um [formatador de tipo de mídia](media-formatters.md).</span><span class="sxs-lookup"><span data-stu-id="db494-111">For complex types, Web API tries to read the value from the message body, using a [media-type formatter](media-formatters.md).</span></span>

<span data-ttu-id="db494-112">Por exemplo, aqui está um método típico de controlador da API Web:</span><span class="sxs-lookup"><span data-stu-id="db494-112">For example, here is a typical Web API controller method:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="db494-113">O parâmetro *ID* é um &quot; &quot; tipo simples, portanto, a API Web tenta obter o valor do URI de solicitação.</span><span class="sxs-lookup"><span data-stu-id="db494-113">The *id* parameter is a &quot;simple&quot; type, so Web API tries to get the value from the request URI.</span></span> <span data-ttu-id="db494-114">O parâmetro *Item* é um tipo complexo, portanto, a API Web usa um formatador de tipo de mídia para ler o valor do corpo da solicitação.</span><span class="sxs-lookup"><span data-stu-id="db494-114">The *item* parameter is a complex type, so Web API uses a media-type formatter to read the value from the request body.</span></span>

<span data-ttu-id="db494-115">Para obter um valor do URI, a API da Web procura os dados da rota e a cadeia de caracteres de consulta do URI.</span><span class="sxs-lookup"><span data-stu-id="db494-115">To get a value from the URI, Web API looks in the route data and the URI query string.</span></span> <span data-ttu-id="db494-116">Os dados de rota são preenchidos quando o sistema de roteamento analisa o URI e o corresponde a uma rota.</span><span class="sxs-lookup"><span data-stu-id="db494-116">The route data is populated when the routing system parses the URI and matches it to a route.</span></span> <span data-ttu-id="db494-117">Para obter mais informações, consulte [seleção de roteamento e ação](../web-api-routing-and-actions/routing-and-action-selection.md).</span><span class="sxs-lookup"><span data-stu-id="db494-117">For more information, see [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span>

<span data-ttu-id="db494-118">No restante deste artigo, mostrarei como você pode personalizar o processo de associação de modelo.</span><span class="sxs-lookup"><span data-stu-id="db494-118">In the rest of this article, I'll show how you can customize the model binding process.</span></span> <span data-ttu-id="db494-119">Para tipos complexos, no entanto, considere o uso de formatadores de tipo de mídia sempre que possível.</span><span class="sxs-lookup"><span data-stu-id="db494-119">For complex types, however, consider using media-type formatters whenever possible.</span></span> <span data-ttu-id="db494-120">Um princípio importante do HTTP é que os recursos são enviados no corpo da mensagem, usando a negociação de conteúdo para especificar a representação do recurso.</span><span class="sxs-lookup"><span data-stu-id="db494-120">A key principle of HTTP is that resources are sent in the message body, using content negotiation to specify the representation of the resource.</span></span> <span data-ttu-id="db494-121">Os formatadores de tipo de mídia foram projetados para exatamente essa finalidade.</span><span class="sxs-lookup"><span data-stu-id="db494-121">Media-type formatters were designed for exactly this purpose.</span></span>

## <a name="using-fromuri"></a><span data-ttu-id="db494-122">Usando [FromUri]</span><span class="sxs-lookup"><span data-stu-id="db494-122">Using [FromUri]</span></span>

<span data-ttu-id="db494-123">Para forçar a API Web a ler um tipo complexo do URI, adicione o atributo **[FromUri]** ao parâmetro.</span><span class="sxs-lookup"><span data-stu-id="db494-123">To force Web API to read a complex type from the URI, add the **[FromUri]** attribute to the parameter.</span></span> <span data-ttu-id="db494-124">O exemplo a seguir define um `GeoPoint` tipo, juntamente com um método de controlador que obtém o `GeoPoint` do URI.</span><span class="sxs-lookup"><span data-stu-id="db494-124">The following example defines a `GeoPoint` type, along with a controller method that gets the `GeoPoint` from the URI.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="db494-125">O cliente pode colocar os valores de latitude e longitude na cadeia de caracteres de consulta e a API da Web irá usá-los para construir um `GeoPoint` .</span><span class="sxs-lookup"><span data-stu-id="db494-125">The client can put the Latitude and Longitude values in the query string and Web API will use them to construct a `GeoPoint`.</span></span> <span data-ttu-id="db494-126">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="db494-126">For example:</span></span>

`http://localhost/api/values/?Latitude=47.678558&Longitude=-122.130989`

## <a name="using-frombody"></a><span data-ttu-id="db494-127">Usando [FromBody]</span><span class="sxs-lookup"><span data-stu-id="db494-127">Using [FromBody]</span></span>

<span data-ttu-id="db494-128">Para forçar a API Web a ler um tipo simples do corpo da solicitação, adicione o atributo **[FromBody]** ao parâmetro:</span><span class="sxs-lookup"><span data-stu-id="db494-128">To force Web API to read a simple type from the request body, add the **[FromBody]** attribute to the parameter:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="db494-129">Neste exemplo, a API da Web usará um formatador de tipo de mídia para ler o valor do *nome* do corpo da solicitação.</span><span class="sxs-lookup"><span data-stu-id="db494-129">In this example, Web API will use a media-type formatter to read the value of *name* from the request body.</span></span> <span data-ttu-id="db494-130">Aqui está um exemplo de solicitação de cliente.</span><span class="sxs-lookup"><span data-stu-id="db494-130">Here is an example client request.</span></span>

[!code-console[Main](parameter-binding-in-aspnet-web-api/samples/sample4.cmd)]

<span data-ttu-id="db494-131">Quando um parâmetro tem [FromBody], a API da Web usa o cabeçalho Content-Type para selecionar um formatador.</span><span class="sxs-lookup"><span data-stu-id="db494-131">When a parameter has [FromBody], Web API uses the Content-Type header to select a formatter.</span></span> <span data-ttu-id="db494-132">Neste exemplo, o tipo de conteúdo é &quot; Application/JSON &quot; e o corpo da solicitação é uma cadeia de caracteres JSON bruta (não um objeto JSON).</span><span class="sxs-lookup"><span data-stu-id="db494-132">In this example, the content type is &quot;application/json&quot; and the request body is a raw JSON string (not a JSON object).</span></span>

<span data-ttu-id="db494-133">No máximo um parâmetro tem permissão para ler a partir do corpo da mensagem.</span><span class="sxs-lookup"><span data-stu-id="db494-133">At most one parameter is allowed to read from the message body.</span></span> <span data-ttu-id="db494-134">Portanto, isso não funcionará:</span><span class="sxs-lookup"><span data-stu-id="db494-134">So this will not work:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="db494-135">O motivo para essa regra é que o corpo da solicitação pode ser armazenado em um fluxo sem buffer que só pode ser lido uma vez.</span><span class="sxs-lookup"><span data-stu-id="db494-135">The reason for this rule is that the request body might be stored in a non-buffered stream that can only be read once.</span></span>

## <a name="type-converters"></a><span data-ttu-id="db494-136">Conversores de tipo</span><span class="sxs-lookup"><span data-stu-id="db494-136">Type Converters</span></span>

<span data-ttu-id="db494-137">Você pode fazer com que a API da Web trate uma classe como um tipo simples (para que a API da Web tente associá-la a partir do URI) criando um **TypeConverter** e fornecendo uma conversão de cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="db494-137">You can make Web API treat a class as a simple type (so that Web API will try to bind it from the URI) by creating a **TypeConverter** and providing a string conversion.</span></span>

<span data-ttu-id="db494-138">O código a seguir mostra uma `GeoPoint` classe que representa um ponto geográfico, além de um **TypeConverter** que converte de cadeias de caracteres em `GeoPoint` instâncias.</span><span class="sxs-lookup"><span data-stu-id="db494-138">The following code shows a `GeoPoint` class that represents a geographical point, plus a **TypeConverter** that converts from strings to `GeoPoint` instances.</span></span> <span data-ttu-id="db494-139">A `GeoPoint` classe é decorada com um atributo **[TypeConverter]** para especificar o conversor de tipo.</span><span class="sxs-lookup"><span data-stu-id="db494-139">The `GeoPoint` class is decorated with a **[TypeConverter]** attribute to specify the type converter.</span></span> <span data-ttu-id="db494-140">(Este exemplo foi inspirado pela postagem no blog de Mike Stall [como associar a objetos personalizados em assinaturas de ação no MVC/WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx).)</span><span class="sxs-lookup"><span data-stu-id="db494-140">(This example was inspired by Mike Stall's blog post [How to bind to custom objects in action signatures in MVC/WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx).)</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="db494-141">Agora, a API da Web tratará `GeoPoint` como um tipo simples, o que significa que tentará associar `GeoPoint` os parâmetros do URI.</span><span class="sxs-lookup"><span data-stu-id="db494-141">Now Web API will treat `GeoPoint` as a simple type, meaning it will try to bind `GeoPoint` parameters from the URI.</span></span> <span data-ttu-id="db494-142">Você não precisa incluir **[FromUri]** no parâmetro.</span><span class="sxs-lookup"><span data-stu-id="db494-142">You don't need to include **[FromUri]** on the parameter.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="db494-143">O cliente pode invocar o método com um URI como este:</span><span class="sxs-lookup"><span data-stu-id="db494-143">The client can invoke the method with a URI like this:</span></span>

`http://localhost/api/values/?location=47.678558,-122.130989`

## <a name="model-binders"></a><span data-ttu-id="db494-144">ASSOCIADORES de modelo</span><span class="sxs-lookup"><span data-stu-id="db494-144">Model Binders</span></span>

<span data-ttu-id="db494-145">Uma opção mais flexível do que um conversor de tipo é criar um associador de modelo personalizado.</span><span class="sxs-lookup"><span data-stu-id="db494-145">A more flexible option than a type converter is to create a custom model binder.</span></span> <span data-ttu-id="db494-146">Com um associador de modelo, você tem acesso a coisas como a solicitação HTTP, a descrição da ação e os valores brutos dos dados da rota.</span><span class="sxs-lookup"><span data-stu-id="db494-146">With a model binder, you have access to things like the HTTP request, the action description, and the raw values from the route data.</span></span>

<span data-ttu-id="db494-147">Para criar um associador de modelo, implemente a interface **IModelBinder** .</span><span class="sxs-lookup"><span data-stu-id="db494-147">To create a model binder, implement the **IModelBinder** interface.</span></span> <span data-ttu-id="db494-148">Essa interface define um único método, **BindModel**:</span><span class="sxs-lookup"><span data-stu-id="db494-148">This interface defines a single method, **BindModel**:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample8.cs)]

<span data-ttu-id="db494-149">Aqui está um associador de modelo para `GeoPoint` objetos.</span><span class="sxs-lookup"><span data-stu-id="db494-149">Here is a model binder for `GeoPoint` objects.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample9.cs)]

<span data-ttu-id="db494-150">Um associador de modelo obtém valores de entrada brutos de um *provedor de valor*.</span><span class="sxs-lookup"><span data-stu-id="db494-150">A model binder gets raw input values from a *value provider*.</span></span> <span data-ttu-id="db494-151">Esse design separa duas funções distintas:</span><span class="sxs-lookup"><span data-stu-id="db494-151">This design separates two distinct functions:</span></span>

- <span data-ttu-id="db494-152">O provedor de valor pega a solicitação HTTP e popula um dicionário de pares chave-valor.</span><span class="sxs-lookup"><span data-stu-id="db494-152">The value provider takes the HTTP request and populates a dictionary of key-value pairs.</span></span>
- <span data-ttu-id="db494-153">O associador de modelo usa esse dicionário para preencher o modelo.</span><span class="sxs-lookup"><span data-stu-id="db494-153">The model binder uses this dictionary to populate the model.</span></span>

<span data-ttu-id="db494-154">O provedor de valor padrão na API Web obtém valores dos dados de rota e da cadeia de caracteres de consulta.</span><span class="sxs-lookup"><span data-stu-id="db494-154">The default value provider in Web API gets values from the route data and the query string.</span></span> <span data-ttu-id="db494-155">Por exemplo, se o URI for `http://localhost/api/values/1?location=48,-122` , o provedor de valor criará os seguintes pares de chave-valor:</span><span class="sxs-lookup"><span data-stu-id="db494-155">For example, if the URI is `http://localhost/api/values/1?location=48,-122`, the value provider creates the following key-value pairs:</span></span>

- <span data-ttu-id="db494-156">ID = &quot; 1&quot;</span><span class="sxs-lookup"><span data-stu-id="db494-156">id = &quot;1&quot;</span></span>
- <span data-ttu-id="db494-157">local = &quot; 48,-122&quot;</span><span class="sxs-lookup"><span data-stu-id="db494-157">location = &quot;48,-122&quot;</span></span>

<span data-ttu-id="db494-158">(Estou supondo que o modelo de rota padrão, que é &quot; API/{Controller}/{ID} &quot; .)</span><span class="sxs-lookup"><span data-stu-id="db494-158">(I'm assuming the default route template, which is &quot;api/{controller}/{id}&quot;.)</span></span>

<span data-ttu-id="db494-159">O nome do parâmetro a ser associado é armazenado na propriedade **ModelBindingContext. ModelName** .</span><span class="sxs-lookup"><span data-stu-id="db494-159">The name of the parameter to bind is stored in the **ModelBindingContext.ModelName** property.</span></span> <span data-ttu-id="db494-160">O associador de modelo procura uma chave com esse valor no dicionário.</span><span class="sxs-lookup"><span data-stu-id="db494-160">The model binder looks for a key with this value in the dictionary.</span></span> <span data-ttu-id="db494-161">Se o valor existir e puder ser convertido em um `GeoPoint` , o associador de modelo atribuirá o valor associado à propriedade **ModelBindingContext. Model** .</span><span class="sxs-lookup"><span data-stu-id="db494-161">If the value exists and can be converted into a `GeoPoint`, the model binder assigns the bound value to the **ModelBindingContext.Model** property.</span></span>

<span data-ttu-id="db494-162">Observe que o associador de modelo não está limitado a uma conversão de tipo simples.</span><span class="sxs-lookup"><span data-stu-id="db494-162">Notice that the model binder is not limited to a simple type conversion.</span></span> <span data-ttu-id="db494-163">Neste exemplo, o associador de modelo primeiro procura em uma tabela de locais conhecidos e, se isso falhar, usará a conversão de tipo.</span><span class="sxs-lookup"><span data-stu-id="db494-163">In this example, the model binder first looks in a table of known locations, and if that fails, it uses type conversion.</span></span>

<span data-ttu-id="db494-164">**Configurando o associador de modelo**</span><span class="sxs-lookup"><span data-stu-id="db494-164">**Setting the Model Binder**</span></span>

<span data-ttu-id="db494-165">Há várias maneiras de definir um associador de modelo.</span><span class="sxs-lookup"><span data-stu-id="db494-165">There are several ways to set a model binder.</span></span> <span data-ttu-id="db494-166">Primeiro, você pode adicionar um atributo **[ModelBinder]** ao parâmetro.</span><span class="sxs-lookup"><span data-stu-id="db494-166">First, you can add a **[ModelBinder]** attribute to the parameter.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample10.cs)]

<span data-ttu-id="db494-167">Você também pode adicionar um atributo **[ModelBinder]** ao tipo.</span><span class="sxs-lookup"><span data-stu-id="db494-167">You can also add a **[ModelBinder]** attribute to the type.</span></span> <span data-ttu-id="db494-168">A API Web usará o associador de modelo especificado para todos os parâmetros desse tipo.</span><span class="sxs-lookup"><span data-stu-id="db494-168">Web API will use the specified model binder for all parameters of that type.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample11.cs)]

<span data-ttu-id="db494-169">Por fim, você pode adicionar um provedor de associador de modelo ao **HttpConfiguration**.</span><span class="sxs-lookup"><span data-stu-id="db494-169">Finally, you can add a model-binder provider to the **HttpConfiguration**.</span></span> <span data-ttu-id="db494-170">Um provedor de associador de modelo é simplesmente uma classe de fábrica que cria um associador de modelo.</span><span class="sxs-lookup"><span data-stu-id="db494-170">A model-binder provider is simply a factory class that creates a model binder.</span></span> <span data-ttu-id="db494-171">Você pode criar um provedor derivando da classe [ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx) .</span><span class="sxs-lookup"><span data-stu-id="db494-171">You can create a provider by deriving from the [ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx) class.</span></span> <span data-ttu-id="db494-172">No entanto, se o seu associador de modelo tratar de um único tipo, será mais fácil usar o **SimpleModelBinderProvider** interno, que foi projetado para essa finalidade.</span><span class="sxs-lookup"><span data-stu-id="db494-172">However, if your model binder handles a single type, it's easier to use the built-in **SimpleModelBinderProvider**, which is designed for this purpose.</span></span> <span data-ttu-id="db494-173">O código a seguir mostra como fazer isso.</span><span class="sxs-lookup"><span data-stu-id="db494-173">The following code shows how to do this.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample12.cs)]

<span data-ttu-id="db494-174">Com um provedor de associação de modelo, você ainda precisa adicionar o atributo **[ModelBinder]** ao parâmetro para informar à API Web que ele deve usar um associador de modelo e não um formatador de tipo de mídia.</span><span class="sxs-lookup"><span data-stu-id="db494-174">With a model-binding provider, you still need to add the **[ModelBinder]** attribute to the parameter, to tell Web API that it should use a model binder and not a media-type formatter.</span></span> <span data-ttu-id="db494-175">Mas agora você não precisa especificar o tipo de associador de modelo no atributo:</span><span class="sxs-lookup"><span data-stu-id="db494-175">But now you don't need to specify the type of model binder in the attribute:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample13.cs)]

## <a name="value-providers"></a><span data-ttu-id="db494-176">Provedores de valor</span><span class="sxs-lookup"><span data-stu-id="db494-176">Value Providers</span></span>

<span data-ttu-id="db494-177">Mencionei que um associador de modelo obtém valores de um provedor de valor.</span><span class="sxs-lookup"><span data-stu-id="db494-177">I mentioned that a model binder gets values from a value provider.</span></span> <span data-ttu-id="db494-178">Para gravar um provedor de valor personalizado, implemente a interface **IValueProvider** .</span><span class="sxs-lookup"><span data-stu-id="db494-178">To write a custom value provider, implement the **IValueProvider** interface.</span></span> <span data-ttu-id="db494-179">Aqui está um exemplo que efetua pull dos valores dos cookies na solicitação:</span><span class="sxs-lookup"><span data-stu-id="db494-179">Here is an example that pulls values from the cookies in the request:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample14.cs)]

<span data-ttu-id="db494-180">Você também precisa criar um alocador de provedor de valor derivando da classe **ValueProviderFactory** .</span><span class="sxs-lookup"><span data-stu-id="db494-180">You also need to create a value provider factory by deriving from the **ValueProviderFactory** class.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample15.cs)]

<span data-ttu-id="db494-181">Adicione o alocador de provedor de valor ao **HttpConfiguration** da seguinte maneira.</span><span class="sxs-lookup"><span data-stu-id="db494-181">Add the value provider factory to the **HttpConfiguration** as follows.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample16.cs)]

<span data-ttu-id="db494-182">A API Web compõe todos os provedores de valor, portanto, quando um associador de modelo chama **ValueProvider. GetValue**, o associador de modelo recebe o valor do provedor de primeiro valor que é capaz de produzi-lo.</span><span class="sxs-lookup"><span data-stu-id="db494-182">Web API composes all of the value providers, so when a model binder calls **ValueProvider.GetValue**, the model binder receives the value from the first value provider that is able to produce it.</span></span>

<span data-ttu-id="db494-183">Como alternativa, você pode definir o alocador de provedor de valor no nível de parâmetro usando o atributo **ValueProvider** , da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="db494-183">Alternatively, you can set the value provider factory at the parameter level by using the **ValueProvider** attribute, as follows:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample17.cs)]

<span data-ttu-id="db494-184">Isso informa à API da Web para usar a associação de modelo com o alocador de provedor de valor especificado e não para usar qualquer um dos outros provedores de valor registrado.</span><span class="sxs-lookup"><span data-stu-id="db494-184">This tells Web API to use model binding with the specified value provider factory, and not to use any of the other registered value providers.</span></span>

## <a name="httpparameterbinding"></a><span data-ttu-id="db494-185">HttpParameterBinding</span><span class="sxs-lookup"><span data-stu-id="db494-185">HttpParameterBinding</span></span>

<span data-ttu-id="db494-186">Os vinculadores de modelo são uma instância específica de um mecanismo mais geral.</span><span class="sxs-lookup"><span data-stu-id="db494-186">Model binders are a specific instance of a more general mechanism.</span></span> <span data-ttu-id="db494-187">Se você olhar o atributo **[ModelBinder]** , verá que ele deriva da classe abstrata **ParameterBindingAttribute** .</span><span class="sxs-lookup"><span data-stu-id="db494-187">If you look at the **[ModelBinder]** attribute, you will see that it derives from the abstract **ParameterBindingAttribute** class.</span></span> <span data-ttu-id="db494-188">Essa classe define um método único, **GetBinding**, que retorna um objeto **HttpParameterBinding** :</span><span class="sxs-lookup"><span data-stu-id="db494-188">This class defines a single method, **GetBinding**, which returns an **HttpParameterBinding** object:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample18.cs)]

<span data-ttu-id="db494-189">Um **HttpParameterBinding** é responsável por associar um parâmetro a um valor.</span><span class="sxs-lookup"><span data-stu-id="db494-189">An **HttpParameterBinding** is responsible for binding a parameter to a value.</span></span> <span data-ttu-id="db494-190">No caso de **[ModelBinder]**, o atributo retorna uma implementação de **HttpParameterBinding** que usa um **IModelBinder** para executar a associação real.</span><span class="sxs-lookup"><span data-stu-id="db494-190">In the case of **[ModelBinder]**, the attribute returns an **HttpParameterBinding** implementation that uses an **IModelBinder** to perform the actual binding.</span></span> <span data-ttu-id="db494-191">Você também pode implementar seu próprio **HttpParameterBinding**.</span><span class="sxs-lookup"><span data-stu-id="db494-191">You can also implement your own **HttpParameterBinding**.</span></span>

<span data-ttu-id="db494-192">Por exemplo, suponha que você queira obter ETags de `if-match` `if-none-match` cabeçalhos e na solicitação.</span><span class="sxs-lookup"><span data-stu-id="db494-192">For example, suppose you want to get ETags from `if-match` and `if-none-match` headers in the request.</span></span> <span data-ttu-id="db494-193">Vamos começar definindo uma classe para representar ETags.</span><span class="sxs-lookup"><span data-stu-id="db494-193">We'll start by defining a class to represent ETags.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample19.cs)]

<span data-ttu-id="db494-194">Também definiremos uma enumeração para indicar se deve obter a ETag do `if-match` cabeçalho ou do `if-none-match` cabeçalho.</span><span class="sxs-lookup"><span data-stu-id="db494-194">We'll also define an enumeration to indicate whether to get the ETag from the `if-match` header or the `if-none-match` header.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample20.cs)]

<span data-ttu-id="db494-195">Aqui está um **HttpParameterBinding** que obtém a eTag do cabeçalho desejado e a associa a um parâmetro do tipo ETag:</span><span class="sxs-lookup"><span data-stu-id="db494-195">Here is an **HttpParameterBinding** that gets the ETag from the desired header and binds it to a parameter of type ETag:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample21.cs)]

<span data-ttu-id="db494-196">O método **ExecuteBindingAsync** faz a associação.</span><span class="sxs-lookup"><span data-stu-id="db494-196">The **ExecuteBindingAsync** method does the binding.</span></span> <span data-ttu-id="db494-197">Dentro desse método, adicione o valor do parâmetro Bound ao dicionário **ActionArgument** no **HttpActionContext**.</span><span class="sxs-lookup"><span data-stu-id="db494-197">Within this method, add the bound parameter value to the **ActionArgument** dictionary in the **HttpActionContext**.</span></span>

> [!NOTE]
> <span data-ttu-id="db494-198">Se o método **ExecuteBindingAsync** ler o corpo da mensagem de solicitação, substitua a propriedade **WillReadBody** para retornar true.</span><span class="sxs-lookup"><span data-stu-id="db494-198">If your **ExecuteBindingAsync** method reads the body of the request message, override the **WillReadBody** property to return true.</span></span> <span data-ttu-id="db494-199">O corpo da solicitação pode ser um fluxo sem buffer que só pode ser lido uma vez, portanto, a API Web impõe uma regra que, no máximo, uma associação pode ler o corpo da mensagem.</span><span class="sxs-lookup"><span data-stu-id="db494-199">The request body might be an unbuffered stream that can only be read once, so Web API enforces a rule that at most one binding can read the message body.</span></span>

<span data-ttu-id="db494-200">Para aplicar um **HttpParameterBinding** personalizado, você pode definir um atributo que deriva de **ParameterBindingAttribute**.</span><span class="sxs-lookup"><span data-stu-id="db494-200">To apply a custom **HttpParameterBinding**, you can define an attribute that derives from **ParameterBindingAttribute**.</span></span> <span data-ttu-id="db494-201">Para `ETagParameterBinding` , vamos definir dois atributos, um para `if-match` cabeçalhos e outro para `if-none-match` cabeçalhos.</span><span class="sxs-lookup"><span data-stu-id="db494-201">For `ETagParameterBinding`, we'll define two attributes, one for `if-match` headers and one for `if-none-match` headers.</span></span> <span data-ttu-id="db494-202">Ambos derivam de uma classe base abstrata.</span><span class="sxs-lookup"><span data-stu-id="db494-202">Both derive from an abstract base class.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample22.cs)]

<span data-ttu-id="db494-203">Aqui está um método de controlador que usa o `[IfNoneMatch]` atributo.</span><span class="sxs-lookup"><span data-stu-id="db494-203">Here is a controller method that uses the `[IfNoneMatch]` attribute.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample23.cs)]

<span data-ttu-id="db494-204">Além de **ParameterBindingAttribute**, há outro gancho para adicionar um **HttpParameterBinding** personalizado.</span><span class="sxs-lookup"><span data-stu-id="db494-204">Besides **ParameterBindingAttribute**, there is another hook for adding a custom **HttpParameterBinding**.</span></span> <span data-ttu-id="db494-205">No objeto **HttpConfiguration** , a propriedade **ParameterBindingRules** é uma coleção de funções anônimas do tipo (**HttpParameterDescriptor**  - &gt; **HttpParameterBinding**).</span><span class="sxs-lookup"><span data-stu-id="db494-205">On the **HttpConfiguration** object, the **ParameterBindingRules** property is a collection of anonymous functions of type (**HttpParameterDescriptor** -&gt; **HttpParameterBinding**).</span></span> <span data-ttu-id="db494-206">Por exemplo, você pode adicionar uma regra que qualquer parâmetro de ETag em um método GET usa `ETagParameterBinding` com `if-none-match` :</span><span class="sxs-lookup"><span data-stu-id="db494-206">For example, you could add a rule that any ETag parameter on a GET method uses `ETagParameterBinding` with `if-none-match`:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample24.cs)]

<span data-ttu-id="db494-207">A função deve retornar `null` para parâmetros em que a associação não é aplicável.</span><span class="sxs-lookup"><span data-stu-id="db494-207">The function should return `null` for parameters where the binding is not applicable.</span></span>

## <a name="iactionvaluebinder"></a><span data-ttu-id="db494-208">IActionValueBinder</span><span class="sxs-lookup"><span data-stu-id="db494-208">IActionValueBinder</span></span>

<span data-ttu-id="db494-209">O processo de vinculação de parâmetro inteiro é controlado por um serviço conectável, **IActionValueBinder**.</span><span class="sxs-lookup"><span data-stu-id="db494-209">The entire parameter-binding process is controlled by a pluggable service, **IActionValueBinder**.</span></span> <span data-ttu-id="db494-210">A implementação padrão de **IActionValueBinder** faz o seguinte:</span><span class="sxs-lookup"><span data-stu-id="db494-210">The default implementation of **IActionValueBinder** does the following:</span></span>

1. <span data-ttu-id="db494-211">Procure um **ParameterBindingAttribute** no parâmetro.</span><span class="sxs-lookup"><span data-stu-id="db494-211">Look for a **ParameterBindingAttribute** on the parameter.</span></span> <span data-ttu-id="db494-212">Isso inclui **[FromBody]**, **[FromUri]** e **[ModelBinder]**, ou atributos personalizados.</span><span class="sxs-lookup"><span data-stu-id="db494-212">This includes **[FromBody]**, **[FromUri]**, and **[ModelBinder]**, or custom attributes.</span></span>
2. <span data-ttu-id="db494-213">Caso contrário, examine **HttpConfiguration. ParameterBindingRules** para uma função que retorna um **HttpParameterBinding** não nulo.</span><span class="sxs-lookup"><span data-stu-id="db494-213">Otherwise, look in **HttpConfiguration.ParameterBindingRules** for a function that returns a non-null **HttpParameterBinding**.</span></span>
3. <span data-ttu-id="db494-214">Caso contrário, use as regras padrão que descrevi anteriormente.</span><span class="sxs-lookup"><span data-stu-id="db494-214">Otherwise, use the default rules that I described previously.</span></span> 

    - <span data-ttu-id="db494-215">Se o tipo de parâmetro for "simples" ou tiver um conversor de tipo, associe a partir do URI.</span><span class="sxs-lookup"><span data-stu-id="db494-215">If the parameter type is "simple"or has a type converter, bind from the URI.</span></span> <span data-ttu-id="db494-216">Isso é equivalente a colocar o atributo **[FromUri]** no parâmetro.</span><span class="sxs-lookup"><span data-stu-id="db494-216">This is equivalent to putting the **[FromUri]** attribute on the parameter.</span></span>
    - <span data-ttu-id="db494-217">Caso contrário, tente ler o parâmetro no corpo da mensagem.</span><span class="sxs-lookup"><span data-stu-id="db494-217">Otherwise, try to read the parameter from the message body.</span></span> <span data-ttu-id="db494-218">Isso é equivalente a colocar **[FromBody]** no parâmetro.</span><span class="sxs-lookup"><span data-stu-id="db494-218">This is equivalent to putting **[FromBody]** on the parameter.</span></span>

<span data-ttu-id="db494-219">Se desejar, você poderia substituir todo o serviço **IActionValueBinder** por uma implementação personalizada.</span><span class="sxs-lookup"><span data-stu-id="db494-219">If you wanted, you could replace the entire **IActionValueBinder** service with a custom implementation.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="db494-220">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="db494-220">Additional Resources</span></span>

[<span data-ttu-id="db494-221">Exemplo de associação de parâmetro personalizado</span><span class="sxs-lookup"><span data-stu-id="db494-221">Custom Parameter Binding Sample</span></span>](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/CustomParameterBinding)

<span data-ttu-id="db494-222">Mike Stall escreveu uma boa série de postagens no blog sobre associação de parâmetro da API Web:</span><span class="sxs-lookup"><span data-stu-id="db494-222">Mike Stall wrote a good series of blog posts about Web API parameter binding:</span></span>

- [<span data-ttu-id="db494-223">Como a API Web faz a associação de parâmetros</span><span class="sxs-lookup"><span data-stu-id="db494-223">How Web API does Parameter Binding</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/16/how-webapi-does-parameter-binding.aspx)
- [<span data-ttu-id="db494-224">Associação de parâmetro de estilo MVC para API Web</span><span class="sxs-lookup"><span data-stu-id="db494-224">MVC Style parameter binding for Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/18/mvc-style-parameter-binding-for-webapi.aspx)
- [<span data-ttu-id="db494-225">Como associar a objetos personalizados em assinaturas de ação no MVC/API da Web</span><span class="sxs-lookup"><span data-stu-id="db494-225">How to bind to custom objects in action signatures in MVC/Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx)
- [<span data-ttu-id="db494-226">Como criar um provedor de valor personalizado na API Web</span><span class="sxs-lookup"><span data-stu-id="db494-226">How to create a custom value provider in Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)
- [<span data-ttu-id="db494-227">Associação de parâmetro de API Web nos bastidores</span><span class="sxs-lookup"><span data-stu-id="db494-227">Web API Parameter binding under the hood</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx)
