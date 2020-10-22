---
uid: web-api/overview/formats-and-model-binding/json-and-xml-serialization
title: Serialização de JSON e XML em ASP.NET Web API-ASP.NET 4. x
author: MikeWasson
description: Descreve os formatadores JSON e XML em ASP.NET Web API para ASP.NET 4. x.
ms.author: riande
ms.date: 05/30/2012
ms.custom: seoapril2019
ms.assetid: 1cd7525d-de5e-4ab6-94f0-51480d3255d1
msc.legacyurl: /web-api/overview/formats-and-model-binding/json-and-xml-serialization
msc.type: authoredcontent
ms.openlocfilehash: e6e02fa1c48e9c5fb8499379508619ddb317ccc9
ms.sourcegitcommit: c62ec20b453cee3249eb894ecd75013b57d078f0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/21/2020
ms.locfileid: "92345166"
---
# <a name="json-and-xml-serialization-in-aspnet-web-api"></a><span data-ttu-id="3704a-103">Serialização de JSON e XML no ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="3704a-103">JSON and XML Serialization in ASP.NET Web API</span></span>

<span data-ttu-id="3704a-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="3704a-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="3704a-105">Este artigo descreve os formatadores JSON e XML no ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="3704a-105">This article describes the JSON and XML formatters in ASP.NET Web API.</span></span>

<span data-ttu-id="3704a-106">No ASP.NET Web API, um *formatador de tipo de mídia* é um objeto que pode:</span><span class="sxs-lookup"><span data-stu-id="3704a-106">In ASP.NET Web API, a *media-type formatter* is an object that can:</span></span>

- <span data-ttu-id="3704a-107">Ler objetos CLR de um corpo de mensagem HTTP</span><span class="sxs-lookup"><span data-stu-id="3704a-107">Read CLR objects from an HTTP message body</span></span>
- <span data-ttu-id="3704a-108">Gravar objetos CLR em um corpo de mensagem HTTP</span><span class="sxs-lookup"><span data-stu-id="3704a-108">Write CLR objects into an HTTP message body</span></span>

<span data-ttu-id="3704a-109">A API Web fornece formatadores de tipo de mídia para JSON e XML.</span><span class="sxs-lookup"><span data-stu-id="3704a-109">Web API provides media-type formatters for both JSON and XML.</span></span> <span data-ttu-id="3704a-110">A estrutura insere esses formatadores no pipeline por padrão.</span><span class="sxs-lookup"><span data-stu-id="3704a-110">The framework inserts these formatters into the pipeline by default.</span></span> <span data-ttu-id="3704a-111">Os clientes podem solicitar JSON ou XML no cabeçalho Accept da solicitação HTTP.</span><span class="sxs-lookup"><span data-stu-id="3704a-111">Clients can request either JSON or XML in the Accept header of the HTTP request.</span></span>

## <a name="contents"></a><span data-ttu-id="3704a-112">Sumário</span><span class="sxs-lookup"><span data-stu-id="3704a-112">Contents</span></span>

- [<span data-ttu-id="3704a-113">Formatador JSON Media-Type</span><span class="sxs-lookup"><span data-stu-id="3704a-113">JSON Media-Type Formatter</span></span>](#json_media_type_formatter)

    - [<span data-ttu-id="3704a-114">Propriedades somente leitura</span><span class="sxs-lookup"><span data-stu-id="3704a-114">Read-Only Properties</span></span>](#json_readonly)
    - [<span data-ttu-id="3704a-115">Datas</span><span class="sxs-lookup"><span data-stu-id="3704a-115">Dates</span></span>](#json_dates)
    - [<span data-ttu-id="3704a-116">Recuo</span><span class="sxs-lookup"><span data-stu-id="3704a-116">Indenting</span></span>](#json_indenting)
    - [<span data-ttu-id="3704a-117">Letras maiúsculas e minúsculas</span><span class="sxs-lookup"><span data-stu-id="3704a-117">Camel Casing</span></span>](#json_camelcasing)
    - [<span data-ttu-id="3704a-118">Objetos anônimos e Weakly-Typed</span><span class="sxs-lookup"><span data-stu-id="3704a-118">Anonymous and Weakly-Typed Objects</span></span>](#json_anon)
- [<span data-ttu-id="3704a-119">Formatador XML Media-Type</span><span class="sxs-lookup"><span data-stu-id="3704a-119">XML Media-Type Formatter</span></span>](#xml_media_type_formatter)

    - [<span data-ttu-id="3704a-120">Propriedades somente leitura</span><span class="sxs-lookup"><span data-stu-id="3704a-120">Read-Only Properties</span></span>](#xml_readonly)
    - [<span data-ttu-id="3704a-121">Datas</span><span class="sxs-lookup"><span data-stu-id="3704a-121">Dates</span></span>](#xml_dates)
    - [<span data-ttu-id="3704a-122">Recuo</span><span class="sxs-lookup"><span data-stu-id="3704a-122">Indenting</span></span>](#xml_indenting)
    - [<span data-ttu-id="3704a-123">Configurando Per-Type serializadores XML</span><span class="sxs-lookup"><span data-stu-id="3704a-123">Setting Per-Type XML Serializers</span></span>](#xml_pertype)
- [<span data-ttu-id="3704a-124">Removendo o formatador JSON ou XML</span><span class="sxs-lookup"><span data-stu-id="3704a-124">Removing the JSON or XML Formatter</span></span>](#removing_the_json_or_xml_formatter)
- [<span data-ttu-id="3704a-125">Manipulando referências a objetos circulares</span><span class="sxs-lookup"><span data-stu-id="3704a-125">Handling Circular Object References</span></span>](#handling_circular_object_references)
- [<span data-ttu-id="3704a-126">Testando a serialização do objeto</span><span class="sxs-lookup"><span data-stu-id="3704a-126">Testing Object Serialization</span></span>](#testing_object_serialization)

<a id="json_media_type_formatter"></a>
## <a name="json-media-type-formatter"></a><span data-ttu-id="3704a-127">Formatador JSON Media-Type</span><span class="sxs-lookup"><span data-stu-id="3704a-127">JSON Media-Type Formatter</span></span>

<span data-ttu-id="3704a-128">A formatação JSON é fornecida pela classe **JsonMediaTypeFormatter** .</span><span class="sxs-lookup"><span data-stu-id="3704a-128">JSON formatting is provided by the **JsonMediaTypeFormatter** class.</span></span> <span data-ttu-id="3704a-129">Por padrão, o **JsonMediaTypeFormatter** usa a biblioteca [JSON.net](https://github.com/JamesNK/Newtonsoft.Json) para executar a serialização.</span><span class="sxs-lookup"><span data-stu-id="3704a-129">By default, **JsonMediaTypeFormatter** uses the [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) library to perform serialization.</span></span> <span data-ttu-id="3704a-130">Json.NET é um projeto de código-fonte aberto de terceiros.</span><span class="sxs-lookup"><span data-stu-id="3704a-130">Json.NET is a third-party open source project.</span></span>

<span data-ttu-id="3704a-131">Se preferir, você pode configurar a classe **JsonMediaTypeFormatter** para usar o **DataContractJsonSerializer** em vez de JSON.net.</span><span class="sxs-lookup"><span data-stu-id="3704a-131">If you prefer, you can configure the **JsonMediaTypeFormatter** class to use the **DataContractJsonSerializer** instead of Json.NET.</span></span> <span data-ttu-id="3704a-132">Para fazer isso, defina a propriedade **UseDataContractJsonSerializer** como **true**:</span><span class="sxs-lookup"><span data-stu-id="3704a-132">To do so, set the **UseDataContractJsonSerializer** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample1.cs)]

### <a name="json-serialization"></a><span data-ttu-id="3704a-133">Serialização JSON</span><span class="sxs-lookup"><span data-stu-id="3704a-133">JSON Serialization</span></span>

<span data-ttu-id="3704a-134">Esta seção descreve alguns comportamentos específicos do formatador JSON, usando o serializador [JSON.net](https://github.com/JamesNK/Newtonsoft.Json) padrão.</span><span class="sxs-lookup"><span data-stu-id="3704a-134">This section describes some specific behaviors of the JSON formatter, using the default [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) serializer.</span></span> <span data-ttu-id="3704a-135">Isso não se destina a ser uma documentação abrangente da biblioteca Json.NET; para obter mais informações, consulte a [documentação do JSON.net](http://james.newtonking.com/projects/json/help/).</span><span class="sxs-lookup"><span data-stu-id="3704a-135">This is not meant to be comprehensive documentation of the Json.NET library; for more information, see the [Json.NET Documentation](http://james.newtonking.com/projects/json/help/).</span></span>

#### <a name="what-gets-serialized"></a><span data-ttu-id="3704a-136">O que é serializado?</span><span class="sxs-lookup"><span data-stu-id="3704a-136">What Gets Serialized?</span></span>

<span data-ttu-id="3704a-137">Por padrão, todas as propriedades públicas e campos são incluídos no JSON serializado.</span><span class="sxs-lookup"><span data-stu-id="3704a-137">By default, all public properties and fields are included in the serialized JSON.</span></span> <span data-ttu-id="3704a-138">Para omitir uma propriedade ou um campo, Decore-o com o atributo **JsonIgnore** .</span><span class="sxs-lookup"><span data-stu-id="3704a-138">To omit a property or field, decorate it with the **JsonIgnore** attribute.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample2.cs)]

<span data-ttu-id="3704a-139">Se você preferir uma &quot; abordagem de aceitação &quot; , decorar a classe com o atributo **DataContract** .</span><span class="sxs-lookup"><span data-stu-id="3704a-139">If you prefer an &quot;opt-in&quot; approach, decorate the class with the **DataContract** attribute.</span></span> <span data-ttu-id="3704a-140">Se esse atributo estiver presente, os membros serão ignorados, a menos que tenham o **DataMember**.</span><span class="sxs-lookup"><span data-stu-id="3704a-140">If this attribute is present, members are ignored unless they have the **DataMember**.</span></span> <span data-ttu-id="3704a-141">Você também pode usar **DataMember** para serializar membros privados.</span><span class="sxs-lookup"><span data-stu-id="3704a-141">You can also use **DataMember** to serialize private members.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample3.cs)]

<a id="json_readonly"></a>
### <a name="read-only-properties"></a><span data-ttu-id="3704a-142">Propriedades de Read-Only</span><span class="sxs-lookup"><span data-stu-id="3704a-142">Read-Only Properties</span></span>

<span data-ttu-id="3704a-143">As propriedades somente leitura são serializadas por padrão.</span><span class="sxs-lookup"><span data-stu-id="3704a-143">Read-only properties are serialized by default.</span></span>

<a id="json_dates"></a>
### <a name="dates"></a><span data-ttu-id="3704a-144">Datas</span><span class="sxs-lookup"><span data-stu-id="3704a-144">Dates</span></span>

<span data-ttu-id="3704a-145">Por padrão, o Json.NET grava as datas no formato [ISO 8601](http://www.w3.org/TR/NOTE-datetime) .</span><span class="sxs-lookup"><span data-stu-id="3704a-145">By default, Json.NET writes dates in [ISO 8601](http://www.w3.org/TR/NOTE-datetime) format.</span></span> <span data-ttu-id="3704a-146">As datas em UTC (tempo Universal Coordenado) são gravadas com um sufixo "Z".</span><span class="sxs-lookup"><span data-stu-id="3704a-146">Dates in UTC (Coordinated Universal Time) are written with a "Z" suffix.</span></span> <span data-ttu-id="3704a-147">As datas na hora local incluem um deslocamento de fuso horário.</span><span class="sxs-lookup"><span data-stu-id="3704a-147">Dates in local time include a time-zone offset.</span></span> <span data-ttu-id="3704a-148">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="3704a-148">For example:</span></span>

[!code-console[Main](json-and-xml-serialization/samples/sample4.cmd)]

<span data-ttu-id="3704a-149">Por padrão, Json.NET preserva o fuso horário.</span><span class="sxs-lookup"><span data-stu-id="3704a-149">By default, Json.NET preserves the time zone.</span></span> <span data-ttu-id="3704a-150">Você pode substituir isso definindo a propriedade DateTimeZoneHandling:</span><span class="sxs-lookup"><span data-stu-id="3704a-150">You can override this by setting the DateTimeZoneHandling property:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample5.cs)]

<span data-ttu-id="3704a-151">Se você preferir usar o [formato de data do Microsoft JSON](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) ( `"\/Date(ticks)\/"` ) em vez do ISO 8601, defina a propriedade **DateFormatHandling** nas configurações do serializador:</span><span class="sxs-lookup"><span data-stu-id="3704a-151">If you prefer to use [Microsoft JSON date format](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) instead of ISO 8601, set the **DateFormatHandling** property on the serializer settings:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample6.cs)]

<a id="json_indenting"></a>
### <a name="indenting"></a><span data-ttu-id="3704a-152">Recuo</span><span class="sxs-lookup"><span data-stu-id="3704a-152">Indenting</span></span>

<span data-ttu-id="3704a-153">Para gravar o JSON recuado, defina a configuração de **formatação** como **formatação. recuada**:</span><span class="sxs-lookup"><span data-stu-id="3704a-153">To write indented JSON, set the **Formatting** setting to **Formatting.Indented**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample7.cs)]

<a id="json_camelcasing"></a>
### <a name="camel-casing"></a><span data-ttu-id="3704a-154">Letras maiúsculas e minúsculas</span><span class="sxs-lookup"><span data-stu-id="3704a-154">Camel Casing</span></span>

<span data-ttu-id="3704a-155">Para gravar nomes de propriedades JSON com o camel case, sem alterar o modelo de dados, defina o **CamelCasePropertyNamesContractResolver** no serializador:</span><span class="sxs-lookup"><span data-stu-id="3704a-155">To write JSON property names with camel casing, without changing your data model, set the **CamelCasePropertyNamesContractResolver** on the serializer:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample8.cs)]

<a id="json_anon"></a>
### <a name="anonymous-and-weakly-typed-objects"></a><span data-ttu-id="3704a-156">Objetos anônimos e Weakly-Typed</span><span class="sxs-lookup"><span data-stu-id="3704a-156">Anonymous and Weakly-Typed Objects</span></span>

<span data-ttu-id="3704a-157">Um método de ação pode retornar um objeto anônimo e serializá-lo para JSON.</span><span class="sxs-lookup"><span data-stu-id="3704a-157">An action method can return an anonymous object and serialize it to JSON.</span></span> <span data-ttu-id="3704a-158">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="3704a-158">For example:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample9.cs)]

<span data-ttu-id="3704a-159">O corpo da mensagem de resposta conterá o seguinte JSON:</span><span class="sxs-lookup"><span data-stu-id="3704a-159">The response message body will contain the following JSON:</span></span>

[!code-json[Main](json-and-xml-serialization/samples/sample10.json)]

<span data-ttu-id="3704a-160">Se sua API Web receber objetos JSON estruturados livremente de clientes, você poderá desserializar o corpo da solicitação para um **Newtonsoft.Jsem. Tipo de LINQ. JObject** .</span><span class="sxs-lookup"><span data-stu-id="3704a-160">If your web API receives loosely structured JSON objects from clients, you can deserialize the request body to a **Newtonsoft.Json.Linq.JObject** type.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample11.cs)]

<span data-ttu-id="3704a-161">No entanto, geralmente é melhor usar objetos de dados com rigidez de tipos.</span><span class="sxs-lookup"><span data-stu-id="3704a-161">However, it is usually better to use strongly typed data objects.</span></span> <span data-ttu-id="3704a-162">Em seguida, você não precisa analisar os dados por conta própria e obtém os benefícios da validação do modelo.</span><span class="sxs-lookup"><span data-stu-id="3704a-162">Then you don't need to parse the data yourself, and you get the benefits of model validation.</span></span>

<span data-ttu-id="3704a-163">O serializador XML não oferece suporte a instâncias de tipos anônimos ou **JObject** .</span><span class="sxs-lookup"><span data-stu-id="3704a-163">The XML serializer does not support anonymous types or **JObject** instances.</span></span> <span data-ttu-id="3704a-164">Se você usar esses recursos para seus dados JSON, deverá remover o formatador XML do pipeline, conforme descrito posteriormente neste artigo.</span><span class="sxs-lookup"><span data-stu-id="3704a-164">If you use these features for your JSON data, you should remove the XML formatter from the pipeline, as described later in this article.</span></span>

<a id="xml_media_type_formatter"></a>
## <a name="xml-media-type-formatter"></a><span data-ttu-id="3704a-165">Formatador XML Media-Type</span><span class="sxs-lookup"><span data-stu-id="3704a-165">XML Media-Type Formatter</span></span>

<span data-ttu-id="3704a-166">A formatação XML é fornecida pela classe **XmlMediaTypeFormatter** .</span><span class="sxs-lookup"><span data-stu-id="3704a-166">XML formatting is provided by the **XmlMediaTypeFormatter** class.</span></span> <span data-ttu-id="3704a-167">Por padrão, **XmlMediaTypeFormatter** usa a classe **DataContractSerializer** para executar a serialização.</span><span class="sxs-lookup"><span data-stu-id="3704a-167">By default, **XmlMediaTypeFormatter** uses the **DataContractSerializer** class to perform serialization.</span></span>

<span data-ttu-id="3704a-168">Se preferir, você pode configurar o **XmlMediaTypeFormatter** para usar o **XmlSerializer** em vez do **DataContractSerializer**.</span><span class="sxs-lookup"><span data-stu-id="3704a-168">If you prefer, you can configure the **XmlMediaTypeFormatter** to use the **XmlSerializer** instead of the **DataContractSerializer**.</span></span> <span data-ttu-id="3704a-169">Para fazer isso, defina a propriedade **UseXmlSerializer** como **true**:</span><span class="sxs-lookup"><span data-stu-id="3704a-169">To do so, set the **UseXmlSerializer** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample12.cs)]

<span data-ttu-id="3704a-170">A classe **XmlSerializer** dá suporte a um conjunto mais estreito de tipos do que o **DataContractSerializer**, mas fornece mais controle sobre o XML resultante.</span><span class="sxs-lookup"><span data-stu-id="3704a-170">The **XmlSerializer** class supports a narrower set of types than **DataContractSerializer**, but gives more control over the resulting XML.</span></span> <span data-ttu-id="3704a-171">Considere usar o **XmlSerializer** se precisar corresponder a um esquema XML existente.</span><span class="sxs-lookup"><span data-stu-id="3704a-171">Consider using **XmlSerializer** if you need to match an existing XML schema.</span></span>

### <a name="xml-serialization"></a><span data-ttu-id="3704a-172">Serialização XML</span><span class="sxs-lookup"><span data-stu-id="3704a-172">XML Serialization</span></span>

<span data-ttu-id="3704a-173">Esta seção descreve alguns comportamentos específicos do formatador XML, usando o **DataContractSerializer**padrão.</span><span class="sxs-lookup"><span data-stu-id="3704a-173">This section describes some specific behaviors of the XML formatter, using the default **DataContractSerializer**.</span></span>

<span data-ttu-id="3704a-174">Por padrão, o DataContractSerializer se comporta da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="3704a-174">By default, the DataContractSerializer behaves as follows:</span></span>

- <span data-ttu-id="3704a-175">Todas as propriedades e os campos públicos de leitura/gravação são serializados.</span><span class="sxs-lookup"><span data-stu-id="3704a-175">All public read/write properties and fields are serialized.</span></span> <span data-ttu-id="3704a-176">Para omitir uma propriedade ou um campo, Decore-o com o atributo **IgnoreDataMember** .</span><span class="sxs-lookup"><span data-stu-id="3704a-176">To omit a property or field, decorate it with the **IgnoreDataMember** attribute.</span></span>
- <span data-ttu-id="3704a-177">Membros privados e protegidos não são serializados.</span><span class="sxs-lookup"><span data-stu-id="3704a-177">Private and protected members are not serialized.</span></span>
- <span data-ttu-id="3704a-178">As propriedades somente leitura não são serializadas.</span><span class="sxs-lookup"><span data-stu-id="3704a-178">Read-only properties are not serialized.</span></span> <span data-ttu-id="3704a-179">(No entanto, o conteúdo de uma propriedade de coleção somente leitura é serializado.)</span><span class="sxs-lookup"><span data-stu-id="3704a-179">(However, the contents of a read-only collection property are serialized.)</span></span>
- <span data-ttu-id="3704a-180">Os nomes de classe e de membro são gravados no XML exatamente como aparecem na declaração de classe.</span><span class="sxs-lookup"><span data-stu-id="3704a-180">Class and member names are written in the XML exactly as they appear in the class declaration.</span></span>
- <span data-ttu-id="3704a-181">Um namespace XML padrão é usado.</span><span class="sxs-lookup"><span data-stu-id="3704a-181">A default XML namespace is used.</span></span>

<span data-ttu-id="3704a-182">Se precisar de mais controle sobre a serialização, você poderá decorar a classe com o atributo **DataContract** .</span><span class="sxs-lookup"><span data-stu-id="3704a-182">If you need more control over the serialization, you can decorate the class with the **DataContract** attribute.</span></span> <span data-ttu-id="3704a-183">Quando esse atributo estiver presente, a classe será serializada da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="3704a-183">When this attribute is present, the class is serialized as follows:</span></span>

- <span data-ttu-id="3704a-184">&quot;Abordagem de aceitação &quot; : as propriedades e os campos não são serializados por padrão.</span><span class="sxs-lookup"><span data-stu-id="3704a-184">&quot;Opt in&quot; approach: Properties and fields are not serialized by default.</span></span> <span data-ttu-id="3704a-185">Para serializar uma propriedade ou um campo, Decore-o com o atributo **DataMember** .</span><span class="sxs-lookup"><span data-stu-id="3704a-185">To serialize a property or field, decorate it with the **DataMember** attribute.</span></span>
- <span data-ttu-id="3704a-186">Para serializar um membro privado ou protegido, Decore-o com o atributo **DataMember** .</span><span class="sxs-lookup"><span data-stu-id="3704a-186">To serialize a private or protected member, decorate it with the **DataMember** attribute.</span></span>
- <span data-ttu-id="3704a-187">As propriedades somente leitura não são serializadas.</span><span class="sxs-lookup"><span data-stu-id="3704a-187">Read-only properties are not serialized.</span></span>
- <span data-ttu-id="3704a-188">Para alterar como o nome da classe aparece no XML, defina o parâmetro *Name* no atributo **DataContract** .</span><span class="sxs-lookup"><span data-stu-id="3704a-188">To change how the class name appears in the XML, set the *Name* parameter in the **DataContract** attribute.</span></span>
- <span data-ttu-id="3704a-189">Para alterar a forma como um nome de membro aparece no XML, defina o parâmetro *Name* no atributo **DataMember** .</span><span class="sxs-lookup"><span data-stu-id="3704a-189">To change how a member name appears in the XML, set the *Name* parameter in the **DataMember** attribute.</span></span>
- <span data-ttu-id="3704a-190">Para alterar o namespace XML, defina o parâmetro *namespace* na classe **DataContract** .</span><span class="sxs-lookup"><span data-stu-id="3704a-190">To change the XML namespace, set the *Namespace* parameter in the **DataContract** class.</span></span>

<a id="xml_readonly"></a>
### <a name="read-only-properties"></a><span data-ttu-id="3704a-191">Propriedades de Read-Only</span><span class="sxs-lookup"><span data-stu-id="3704a-191">Read-Only Properties</span></span>

<span data-ttu-id="3704a-192">As propriedades somente leitura não são serializadas.</span><span class="sxs-lookup"><span data-stu-id="3704a-192">Read-only properties are not serialized.</span></span> <span data-ttu-id="3704a-193">Se uma propriedade somente leitura tiver um campo particular de backup, você poderá marcar o campo particular com o atributo **DataMember** .</span><span class="sxs-lookup"><span data-stu-id="3704a-193">If a read-only property has a backing private field, you can mark the private field with the **DataMember** attribute.</span></span> <span data-ttu-id="3704a-194">Essa abordagem requer o atributo **DataContract** na classe.</span><span class="sxs-lookup"><span data-stu-id="3704a-194">This approach requires the **DataContract** attribute on the class.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample13.cs)]

<a id="xml_dates"></a>
### <a name="dates"></a><span data-ttu-id="3704a-195">Datas</span><span class="sxs-lookup"><span data-stu-id="3704a-195">Dates</span></span>

<span data-ttu-id="3704a-196">As datas são gravadas no formato ISO 8601.</span><span class="sxs-lookup"><span data-stu-id="3704a-196">Dates are written in ISO 8601 format.</span></span> <span data-ttu-id="3704a-197">Por exemplo, &quot; 2012-05-23T20:21:37.9116538 z &quot; .</span><span class="sxs-lookup"><span data-stu-id="3704a-197">For example, &quot;2012-05-23T20:21:37.9116538Z&quot;.</span></span>

<a id="xml_indenting"></a>
### <a name="indenting"></a><span data-ttu-id="3704a-198">Recuo</span><span class="sxs-lookup"><span data-stu-id="3704a-198">Indenting</span></span>

<span data-ttu-id="3704a-199">Para gravar XML recuado, defina a propriedade **recuo** como **true**:</span><span class="sxs-lookup"><span data-stu-id="3704a-199">To write indented XML, set the **Indent** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample14.cs)]

<a id="xml_pertype"></a>
## <a name="setting-per-type-xml-serializers"></a><span data-ttu-id="3704a-200">Configurando Per-Type serializadores XML</span><span class="sxs-lookup"><span data-stu-id="3704a-200">Setting Per-Type XML Serializers</span></span>

<span data-ttu-id="3704a-201">Você pode definir serializadores XML diferentes para diferentes tipos CLR.</span><span class="sxs-lookup"><span data-stu-id="3704a-201">You can set different XML serializers for different CLR types.</span></span> <span data-ttu-id="3704a-202">Por exemplo, você pode ter um objeto de dados específico que requer **XmlSerializer** para compatibilidade com versões anteriores.</span><span class="sxs-lookup"><span data-stu-id="3704a-202">For example, you might have a particular data object that requires **XmlSerializer** for backward compatibility.</span></span> <span data-ttu-id="3704a-203">Você pode usar o **XmlSerializer** para esse objeto e continuar a usar o **DataContractSerializer** para outros tipos.</span><span class="sxs-lookup"><span data-stu-id="3704a-203">You can use **XmlSerializer** for this object and continue to use **DataContractSerializer** for other types.</span></span>

<span data-ttu-id="3704a-204">Para definir um serializador XML para um tipo específico, chame **Setserializador**.</span><span class="sxs-lookup"><span data-stu-id="3704a-204">To set an XML serializer for a particular type, call **SetSerializer**.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample15.cs)]

<span data-ttu-id="3704a-205">Você pode especificar um **XmlSerializer** ou qualquer objeto derivado de **XmlObjectSerializer**.</span><span class="sxs-lookup"><span data-stu-id="3704a-205">You can specify an **XmlSerializer** or any object that derives from **XmlObjectSerializer**.</span></span>

<a id="removing_the_json_or_xml_formatter"></a>
## <a name="removing-the-json-or-xml-formatter"></a><span data-ttu-id="3704a-206">Removendo o formatador JSON ou XML</span><span class="sxs-lookup"><span data-stu-id="3704a-206">Removing the JSON or XML Formatter</span></span>

<span data-ttu-id="3704a-207">Você pode remover o formatador JSON ou o formatador XML da lista de formatadores, se não quiser usá-los.</span><span class="sxs-lookup"><span data-stu-id="3704a-207">You can remove the JSON formatter or the XML formatter from the list of formatters, if you do not want to use them.</span></span> <span data-ttu-id="3704a-208">Os principais motivos para fazer isso são:</span><span class="sxs-lookup"><span data-stu-id="3704a-208">The main reasons to do this are:</span></span>

- <span data-ttu-id="3704a-209">Para restringir as respostas da API Web a um tipo de mídia específico.</span><span class="sxs-lookup"><span data-stu-id="3704a-209">To restrict your web API responses to a particular media type.</span></span> <span data-ttu-id="3704a-210">Por exemplo, você pode optar por dar suporte apenas a respostas JSON e remover o formatador XML.</span><span class="sxs-lookup"><span data-stu-id="3704a-210">For example, you might decide to support only JSON responses, and remove the XML formatter.</span></span>
- <span data-ttu-id="3704a-211">Para substituir o formatador padrão por um formatador personalizado.</span><span class="sxs-lookup"><span data-stu-id="3704a-211">To replace the default formatter with a custom formatter.</span></span> <span data-ttu-id="3704a-212">Por exemplo, você pode substituir o formatador JSON por sua própria implementação personalizada de um formatador JSON.</span><span class="sxs-lookup"><span data-stu-id="3704a-212">For example, you could replace the JSON formatter with your own custom implementation of a JSON formatter.</span></span>

<span data-ttu-id="3704a-213">O código a seguir mostra como remover os formatadores padrão.</span><span class="sxs-lookup"><span data-stu-id="3704a-213">The following code shows how to remove the default formatters.</span></span> <span data-ttu-id="3704a-214">Chame isso a partir do método \*\* \_ Start\*\* do seu aplicativo, definido em global. asax.</span><span class="sxs-lookup"><span data-stu-id="3704a-214">Call this from your **Application\_Start** method, defined in Global.asax.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample16.cs)]

<a id="handling_circular_object_references"></a>
## <a name="handling-circular-object-references"></a><span data-ttu-id="3704a-215">Manipulando referências a objetos circulares</span><span class="sxs-lookup"><span data-stu-id="3704a-215">Handling Circular Object References</span></span>

<span data-ttu-id="3704a-216">Por padrão, os formatadores JSON e XML gravam todos os objetos como valores.</span><span class="sxs-lookup"><span data-stu-id="3704a-216">By default, the JSON and XML formatters write all objects as values.</span></span> <span data-ttu-id="3704a-217">Se duas propriedades fizerem referência ao mesmo objeto, ou se o mesmo objeto aparecer duas vezes em uma coleção, o formatador serializará o objeto duas vezes.</span><span class="sxs-lookup"><span data-stu-id="3704a-217">If two properties refer to the same object, or if the same object appears twice in a collection, the formatter will serialize the object twice.</span></span> <span data-ttu-id="3704a-218">Esse é um problema específico se o seu gráfico de objetos contiver ciclos, pois o serializador lançará uma exceção quando detectar um loop no grafo.</span><span class="sxs-lookup"><span data-stu-id="3704a-218">This is a particular problem if your object graph contains cycles, because the serializer will throw an exception when it detects a loop in the graph.</span></span>

<span data-ttu-id="3704a-219">Considere os seguintes modelos de objeto e controlador.</span><span class="sxs-lookup"><span data-stu-id="3704a-219">Consider the following object models and controller.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample17.cs)]

<span data-ttu-id="3704a-220">Invocar essa ação fará com que o formatador lance uma exceção, o que traduz a uma resposta de código de status 500 (erro de servidor interno) para o cliente.</span><span class="sxs-lookup"><span data-stu-id="3704a-220">Invoking this action will cause the formatter to throw an exception, which translates to a status code 500 (Internal Server Error) response to the client.</span></span>

<span data-ttu-id="3704a-221">Para preservar referências de objeto em JSON, adicione o seguinte código ao método de \*\* \_ início do aplicativo\*\* no arquivo global. asax:</span><span class="sxs-lookup"><span data-stu-id="3704a-221">To preserve object references in JSON, add the following code to **Application\_Start** method in the Global.asax file:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample18.cs)]

<span data-ttu-id="3704a-222">Agora, a ação do controlador retornará JSON semelhante a este:</span><span class="sxs-lookup"><span data-stu-id="3704a-222">Now the controller action will return JSON that looks like this:</span></span>

[!code-json[Main](json-and-xml-serialization/samples/sample19.json)]

<span data-ttu-id="3704a-223">Observe que o serializador adiciona &quot; uma &quot; Propriedade $ID a ambos os objetos.</span><span class="sxs-lookup"><span data-stu-id="3704a-223">Notice that the serializer adds an &quot;$id&quot; property to both objects.</span></span> <span data-ttu-id="3704a-224">Além disso, ele detecta que a propriedade Employee. Department cria um loop e, portanto, substitui o valor por uma referência de objeto: { &quot; $ref &quot; : &quot; 1 &quot; }.</span><span class="sxs-lookup"><span data-stu-id="3704a-224">Also, it detects that the Employee.Department property creates a loop, so it replaces the value with an object reference: {&quot;$ref&quot;:&quot;1&quot;}.</span></span>

> [!NOTE]
> <span data-ttu-id="3704a-225">As referências de objeto não são padrão em JSON.</span><span class="sxs-lookup"><span data-stu-id="3704a-225">Object references are not standard in JSON.</span></span> <span data-ttu-id="3704a-226">Antes de usar esse recurso, considere se os clientes poderão analisar os resultados.</span><span class="sxs-lookup"><span data-stu-id="3704a-226">Before using this feature, consider whether your clients will be able to parse the results.</span></span> <span data-ttu-id="3704a-227">Pode ser melhor simplesmente remover ciclos do grafo.</span><span class="sxs-lookup"><span data-stu-id="3704a-227">It might be better simply to remove cycles from the graph.</span></span> <span data-ttu-id="3704a-228">Por exemplo, o link do funcionário de volta para o departamento não é realmente necessário neste exemplo.</span><span class="sxs-lookup"><span data-stu-id="3704a-228">For example, the link from Employee back to Department is not really needed in this example.</span></span>

<span data-ttu-id="3704a-229">Para preservar as referências de objeto em XML, você tem duas opções.</span><span class="sxs-lookup"><span data-stu-id="3704a-229">To preserve object references in XML, you have two options.</span></span> <span data-ttu-id="3704a-230">A opção mais simples é adicionar `[DataContract(IsReference=true)]` à sua classe de modelo.</span><span class="sxs-lookup"><span data-stu-id="3704a-230">The simpler option is to add `[DataContract(IsReference=true)]` to your model class.</span></span> <span data-ttu-id="3704a-231">O parâmetro *Isreferetion* permite referências a objetos.</span><span class="sxs-lookup"><span data-stu-id="3704a-231">The *IsReference* parameter enables object references.</span></span> <span data-ttu-id="3704a-232">Lembre-se de que **DataContract** faz a aceitação de serialização, portanto, você também precisará adicionar atributos **DataMember** às propriedades:</span><span class="sxs-lookup"><span data-stu-id="3704a-232">Remember that **DataContract** makes serialization opt-in, so you will also need to add **DataMember** attributes to the properties:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample20.cs)]

<span data-ttu-id="3704a-233">Agora, o formatador produzirá XML semelhante ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="3704a-233">Now the formatter will produce XML similar to following:</span></span>

[!code-xml[Main](json-and-xml-serialization/samples/sample21.xml)]

<span data-ttu-id="3704a-234">Se você quiser evitar atributos em sua classe de modelo, há outra opção: criar uma nova instância de **DataContractSerializer** específica de tipo e definir *PreserveObjectReferences* como **true** no construtor.</span><span class="sxs-lookup"><span data-stu-id="3704a-234">If you want to avoid attributes on your model class, there is another option: Create a new type-specific **DataContractSerializer** instance and set *preserveObjectReferences* to **true** in the constructor.</span></span> <span data-ttu-id="3704a-235">Em seguida, defina essa instância como um serializador por tipo no formatador do tipo de mídia XML.</span><span class="sxs-lookup"><span data-stu-id="3704a-235">Then set this instance as a per-type serializer on the XML media-type formatter.</span></span> <span data-ttu-id="3704a-236">O código a seguir mostra como fazer isso:</span><span class="sxs-lookup"><span data-stu-id="3704a-236">The following code show how to do this:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample22.cs?highlight=3)]

<a id="testing_object_serialization"></a>
## <a name="testing-object-serialization"></a><span data-ttu-id="3704a-237">Testando a serialização do objeto</span><span class="sxs-lookup"><span data-stu-id="3704a-237">Testing Object Serialization</span></span>

<span data-ttu-id="3704a-238">À medida que você projeta sua API Web, é útil testar como seus objetos de dados serão serializados.</span><span class="sxs-lookup"><span data-stu-id="3704a-238">As you design your web API, it is useful to test how your data objects will be serialized.</span></span> <span data-ttu-id="3704a-239">Você pode fazer isso sem criar um controlador ou invocar uma ação do controlador.</span><span class="sxs-lookup"><span data-stu-id="3704a-239">You can do this without creating a controller or invoking a controller action.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample23.cs)]
