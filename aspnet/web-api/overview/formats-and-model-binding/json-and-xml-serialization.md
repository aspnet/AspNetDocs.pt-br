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
# <a name="json-and-xml-serialization-in-aspnet-web-api"></a>Serialização de JSON e XML no ASP.NET Web API

por [Mike Wasson](https://github.com/MikeWasson)

Este artigo descreve os formatadores JSON e XML no ASP.NET Web API.

No ASP.NET Web API, um *formatador de tipo de mídia* é um objeto que pode:

- Ler objetos CLR de um corpo de mensagem HTTP
- Gravar objetos CLR em um corpo de mensagem HTTP

A API Web fornece formatadores de tipo de mídia para JSON e XML. A estrutura insere esses formatadores no pipeline por padrão. Os clientes podem solicitar JSON ou XML no cabeçalho Accept da solicitação HTTP.

## <a name="contents"></a>Sumário

- [Formatador JSON Media-Type](#json_media_type_formatter)

    - [Propriedades somente leitura](#json_readonly)
    - [Datas](#json_dates)
    - [Recuo](#json_indenting)
    - [Letras maiúsculas e minúsculas](#json_camelcasing)
    - [Objetos anônimos e Weakly-Typed](#json_anon)
- [Formatador XML Media-Type](#xml_media_type_formatter)

    - [Propriedades somente leitura](#xml_readonly)
    - [Datas](#xml_dates)
    - [Recuo](#xml_indenting)
    - [Configurando Per-Type serializadores XML](#xml_pertype)
- [Removendo o formatador JSON ou XML](#removing_the_json_or_xml_formatter)
- [Manipulando referências a objetos circulares](#handling_circular_object_references)
- [Testando a serialização do objeto](#testing_object_serialization)

<a id="json_media_type_formatter"></a>
## <a name="json-media-type-formatter"></a>Formatador JSON Media-Type

A formatação JSON é fornecida pela classe **JsonMediaTypeFormatter** . Por padrão, o **JsonMediaTypeFormatter** usa a biblioteca [JSON.net](https://github.com/JamesNK/Newtonsoft.Json) para executar a serialização. Json.NET é um projeto de código-fonte aberto de terceiros.

Se preferir, você pode configurar a classe **JsonMediaTypeFormatter** para usar o **DataContractJsonSerializer** em vez de JSON.net. Para fazer isso, defina a propriedade **UseDataContractJsonSerializer** como **true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample1.cs)]

### <a name="json-serialization"></a>Serialização JSON

Esta seção descreve alguns comportamentos específicos do formatador JSON, usando o serializador [JSON.net](https://github.com/JamesNK/Newtonsoft.Json) padrão. Isso não se destina a ser uma documentação abrangente da biblioteca Json.NET; para obter mais informações, consulte a [documentação do JSON.net](http://james.newtonking.com/projects/json/help/).

#### <a name="what-gets-serialized"></a>O que é serializado?

Por padrão, todas as propriedades públicas e campos são incluídos no JSON serializado. Para omitir uma propriedade ou um campo, Decore-o com o atributo **JsonIgnore** .

[!code-csharp[Main](json-and-xml-serialization/samples/sample2.cs)]

Se você preferir uma &quot; abordagem de aceitação &quot; , decorar a classe com o atributo **DataContract** . Se esse atributo estiver presente, os membros serão ignorados, a menos que tenham o **DataMember**. Você também pode usar **DataMember** para serializar membros privados.

[!code-csharp[Main](json-and-xml-serialization/samples/sample3.cs)]

<a id="json_readonly"></a>
### <a name="read-only-properties"></a>Propriedades de Read-Only

As propriedades somente leitura são serializadas por padrão.

<a id="json_dates"></a>
### <a name="dates"></a>Datas

Por padrão, o Json.NET grava as datas no formato [ISO 8601](http://www.w3.org/TR/NOTE-datetime) . As datas em UTC (tempo Universal Coordenado) são gravadas com um sufixo "Z". As datas na hora local incluem um deslocamento de fuso horário. Por exemplo:

[!code-console[Main](json-and-xml-serialization/samples/sample4.cmd)]

Por padrão, Json.NET preserva o fuso horário. Você pode substituir isso definindo a propriedade DateTimeZoneHandling:

[!code-csharp[Main](json-and-xml-serialization/samples/sample5.cs)]

Se você preferir usar o [formato de data do Microsoft JSON](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) ( `"\/Date(ticks)\/"` ) em vez do ISO 8601, defina a propriedade **DateFormatHandling** nas configurações do serializador:

[!code-csharp[Main](json-and-xml-serialization/samples/sample6.cs)]

<a id="json_indenting"></a>
### <a name="indenting"></a>Recuo

Para gravar o JSON recuado, defina a configuração de **formatação** como **formatação. recuada**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample7.cs)]

<a id="json_camelcasing"></a>
### <a name="camel-casing"></a>Letras maiúsculas e minúsculas

Para gravar nomes de propriedades JSON com o camel case, sem alterar o modelo de dados, defina o **CamelCasePropertyNamesContractResolver** no serializador:

[!code-csharp[Main](json-and-xml-serialization/samples/sample8.cs)]

<a id="json_anon"></a>
### <a name="anonymous-and-weakly-typed-objects"></a>Objetos anônimos e Weakly-Typed

Um método de ação pode retornar um objeto anônimo e serializá-lo para JSON. Por exemplo:

[!code-csharp[Main](json-and-xml-serialization/samples/sample9.cs)]

O corpo da mensagem de resposta conterá o seguinte JSON:

[!code-json[Main](json-and-xml-serialization/samples/sample10.json)]

Se sua API Web receber objetos JSON estruturados livremente de clientes, você poderá desserializar o corpo da solicitação para um **Newtonsoft.Jsem. Tipo de LINQ. JObject** .

[!code-csharp[Main](json-and-xml-serialization/samples/sample11.cs)]

No entanto, geralmente é melhor usar objetos de dados com rigidez de tipos. Em seguida, você não precisa analisar os dados por conta própria e obtém os benefícios da validação do modelo.

O serializador XML não oferece suporte a instâncias de tipos anônimos ou **JObject** . Se você usar esses recursos para seus dados JSON, deverá remover o formatador XML do pipeline, conforme descrito posteriormente neste artigo.

<a id="xml_media_type_formatter"></a>
## <a name="xml-media-type-formatter"></a>Formatador XML Media-Type

A formatação XML é fornecida pela classe **XmlMediaTypeFormatter** . Por padrão, **XmlMediaTypeFormatter** usa a classe **DataContractSerializer** para executar a serialização.

Se preferir, você pode configurar o **XmlMediaTypeFormatter** para usar o **XmlSerializer** em vez do **DataContractSerializer**. Para fazer isso, defina a propriedade **UseXmlSerializer** como **true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample12.cs)]

A classe **XmlSerializer** dá suporte a um conjunto mais estreito de tipos do que o **DataContractSerializer**, mas fornece mais controle sobre o XML resultante. Considere usar o **XmlSerializer** se precisar corresponder a um esquema XML existente.

### <a name="xml-serialization"></a>Serialização XML

Esta seção descreve alguns comportamentos específicos do formatador XML, usando o **DataContractSerializer**padrão.

Por padrão, o DataContractSerializer se comporta da seguinte maneira:

- Todas as propriedades e os campos públicos de leitura/gravação são serializados. Para omitir uma propriedade ou um campo, Decore-o com o atributo **IgnoreDataMember** .
- Membros privados e protegidos não são serializados.
- As propriedades somente leitura não são serializadas. (No entanto, o conteúdo de uma propriedade de coleção somente leitura é serializado.)
- Os nomes de classe e de membro são gravados no XML exatamente como aparecem na declaração de classe.
- Um namespace XML padrão é usado.

Se precisar de mais controle sobre a serialização, você poderá decorar a classe com o atributo **DataContract** . Quando esse atributo estiver presente, a classe será serializada da seguinte maneira:

- &quot;Abordagem de aceitação &quot; : as propriedades e os campos não são serializados por padrão. Para serializar uma propriedade ou um campo, Decore-o com o atributo **DataMember** .
- Para serializar um membro privado ou protegido, Decore-o com o atributo **DataMember** .
- As propriedades somente leitura não são serializadas.
- Para alterar como o nome da classe aparece no XML, defina o parâmetro *Name* no atributo **DataContract** .
- Para alterar a forma como um nome de membro aparece no XML, defina o parâmetro *Name* no atributo **DataMember** .
- Para alterar o namespace XML, defina o parâmetro *namespace* na classe **DataContract** .

<a id="xml_readonly"></a>
### <a name="read-only-properties"></a>Propriedades de Read-Only

As propriedades somente leitura não são serializadas. Se uma propriedade somente leitura tiver um campo particular de backup, você poderá marcar o campo particular com o atributo **DataMember** . Essa abordagem requer o atributo **DataContract** na classe.

[!code-csharp[Main](json-and-xml-serialization/samples/sample13.cs)]

<a id="xml_dates"></a>
### <a name="dates"></a>Datas

As datas são gravadas no formato ISO 8601. Por exemplo, &quot; 2012-05-23T20:21:37.9116538 z &quot; .

<a id="xml_indenting"></a>
### <a name="indenting"></a>Recuo

Para gravar XML recuado, defina a propriedade **recuo** como **true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample14.cs)]

<a id="xml_pertype"></a>
## <a name="setting-per-type-xml-serializers"></a>Configurando Per-Type serializadores XML

Você pode definir serializadores XML diferentes para diferentes tipos CLR. Por exemplo, você pode ter um objeto de dados específico que requer **XmlSerializer** para compatibilidade com versões anteriores. Você pode usar o **XmlSerializer** para esse objeto e continuar a usar o **DataContractSerializer** para outros tipos.

Para definir um serializador XML para um tipo específico, chame **Setserializador**.

[!code-csharp[Main](json-and-xml-serialization/samples/sample15.cs)]

Você pode especificar um **XmlSerializer** ou qualquer objeto derivado de **XmlObjectSerializer**.

<a id="removing_the_json_or_xml_formatter"></a>
## <a name="removing-the-json-or-xml-formatter"></a>Removendo o formatador JSON ou XML

Você pode remover o formatador JSON ou o formatador XML da lista de formatadores, se não quiser usá-los. Os principais motivos para fazer isso são:

- Para restringir as respostas da API Web a um tipo de mídia específico. Por exemplo, você pode optar por dar suporte apenas a respostas JSON e remover o formatador XML.
- Para substituir o formatador padrão por um formatador personalizado. Por exemplo, você pode substituir o formatador JSON por sua própria implementação personalizada de um formatador JSON.

O código a seguir mostra como remover os formatadores padrão. Chame isso a partir do método ** \_ Start** do seu aplicativo, definido em global. asax.

[!code-csharp[Main](json-and-xml-serialization/samples/sample16.cs)]

<a id="handling_circular_object_references"></a>
## <a name="handling-circular-object-references"></a>Manipulando referências a objetos circulares

Por padrão, os formatadores JSON e XML gravam todos os objetos como valores. Se duas propriedades fizerem referência ao mesmo objeto, ou se o mesmo objeto aparecer duas vezes em uma coleção, o formatador serializará o objeto duas vezes. Esse é um problema específico se o seu gráfico de objetos contiver ciclos, pois o serializador lançará uma exceção quando detectar um loop no grafo.

Considere os seguintes modelos de objeto e controlador.

[!code-csharp[Main](json-and-xml-serialization/samples/sample17.cs)]

Invocar essa ação fará com que o formatador lance uma exceção, o que traduz a uma resposta de código de status 500 (erro de servidor interno) para o cliente.

Para preservar referências de objeto em JSON, adicione o seguinte código ao método de ** \_ início do aplicativo** no arquivo global. asax:

[!code-csharp[Main](json-and-xml-serialization/samples/sample18.cs)]

Agora, a ação do controlador retornará JSON semelhante a este:

[!code-json[Main](json-and-xml-serialization/samples/sample19.json)]

Observe que o serializador adiciona &quot; uma &quot; Propriedade $ID a ambos os objetos. Além disso, ele detecta que a propriedade Employee. Department cria um loop e, portanto, substitui o valor por uma referência de objeto: { &quot; $ref &quot; : &quot; 1 &quot; }.

> [!NOTE]
> As referências de objeto não são padrão em JSON. Antes de usar esse recurso, considere se os clientes poderão analisar os resultados. Pode ser melhor simplesmente remover ciclos do grafo. Por exemplo, o link do funcionário de volta para o departamento não é realmente necessário neste exemplo.

Para preservar as referências de objeto em XML, você tem duas opções. A opção mais simples é adicionar `[DataContract(IsReference=true)]` à sua classe de modelo. O parâmetro *Isreferetion* permite referências a objetos. Lembre-se de que **DataContract** faz a aceitação de serialização, portanto, você também precisará adicionar atributos **DataMember** às propriedades:

[!code-csharp[Main](json-and-xml-serialization/samples/sample20.cs)]

Agora, o formatador produzirá XML semelhante ao seguinte:

[!code-xml[Main](json-and-xml-serialization/samples/sample21.xml)]

Se você quiser evitar atributos em sua classe de modelo, há outra opção: criar uma nova instância de **DataContractSerializer** específica de tipo e definir *PreserveObjectReferences* como **true** no construtor. Em seguida, defina essa instância como um serializador por tipo no formatador do tipo de mídia XML. O código a seguir mostra como fazer isso:

[!code-csharp[Main](json-and-xml-serialization/samples/sample22.cs?highlight=3)]

<a id="testing_object_serialization"></a>
## <a name="testing-object-serialization"></a>Testando a serialização do objeto

À medida que você projeta sua API Web, é útil testar como seus objetos de dados serão serializados. Você pode fazer isso sem criar um controlador ou invocar uma ação do controlador.

[!code-csharp[Main](json-and-xml-serialization/samples/sample23.cs)]
