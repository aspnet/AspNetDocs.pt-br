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
# <a name="parameter-binding-in-aspnet-web-api"></a>Associação de parâmetro no ASP.NET Web API

por [Mike Wasson](https://github.com/MikeWasson)

[!INCLUDE[](~/includes/coreWebAPI.md)]

Este artigo descreve como a API Web associa parâmetros e como você pode personalizar o processo de associação. Quando a API Web chama um método em um controlador, ela deve definir valores para os parâmetros, um processo chamado *Binding*.

Por padrão, a API Web usa as seguintes regras para associar parâmetros:

- Se o parâmetro for um tipo "simples", a API Web tentará obter o valor do URI. Os tipos simples incluem os [tipos primitivos](https://msdn.microsoft.com/library/system.type.isprimitive.aspx) do .net (**int**, **bool**, **Double** e assim por diante), mais **TimeSpan**, **DateTime**, **GUID**, **decimal** e **String**, *além* de qualquer tipo com um conversor de tipo que possa converter de uma cadeia de caracteres. (Saiba mais sobre os conversores de tipo mais tarde.)
- Para tipos complexos, a API da Web tenta ler o valor do corpo da mensagem, usando um [formatador de tipo de mídia](media-formatters.md).

Por exemplo, aqui está um método típico de controlador da API Web:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample1.cs)]

O parâmetro *ID* é um &quot; &quot; tipo simples, portanto, a API Web tenta obter o valor do URI de solicitação. O parâmetro *Item* é um tipo complexo, portanto, a API Web usa um formatador de tipo de mídia para ler o valor do corpo da solicitação.

Para obter um valor do URI, a API da Web procura os dados da rota e a cadeia de caracteres de consulta do URI. Os dados de rota são preenchidos quando o sistema de roteamento analisa o URI e o corresponde a uma rota. Para obter mais informações, consulte [seleção de roteamento e ação](../web-api-routing-and-actions/routing-and-action-selection.md).

No restante deste artigo, mostrarei como você pode personalizar o processo de associação de modelo. Para tipos complexos, no entanto, considere o uso de formatadores de tipo de mídia sempre que possível. Um princípio importante do HTTP é que os recursos são enviados no corpo da mensagem, usando a negociação de conteúdo para especificar a representação do recurso. Os formatadores de tipo de mídia foram projetados para exatamente essa finalidade.

## <a name="using-fromuri"></a>Usando [FromUri]

Para forçar a API Web a ler um tipo complexo do URI, adicione o atributo **[FromUri]** ao parâmetro. O exemplo a seguir define um `GeoPoint` tipo, juntamente com um método de controlador que obtém o `GeoPoint` do URI.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample2.cs)]

O cliente pode colocar os valores de latitude e longitude na cadeia de caracteres de consulta e a API da Web irá usá-los para construir um `GeoPoint` . Por exemplo:

`http://localhost/api/values/?Latitude=47.678558&Longitude=-122.130989`

## <a name="using-frombody"></a>Usando [FromBody]

Para forçar a API Web a ler um tipo simples do corpo da solicitação, adicione o atributo **[FromBody]** ao parâmetro:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample3.cs)]

Neste exemplo, a API da Web usará um formatador de tipo de mídia para ler o valor do *nome* do corpo da solicitação. Aqui está um exemplo de solicitação de cliente.

[!code-console[Main](parameter-binding-in-aspnet-web-api/samples/sample4.cmd)]

Quando um parâmetro tem [FromBody], a API da Web usa o cabeçalho Content-Type para selecionar um formatador. Neste exemplo, o tipo de conteúdo é &quot; Application/JSON &quot; e o corpo da solicitação é uma cadeia de caracteres JSON bruta (não um objeto JSON).

No máximo um parâmetro tem permissão para ler a partir do corpo da mensagem. Portanto, isso não funcionará:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample5.cs)]

O motivo para essa regra é que o corpo da solicitação pode ser armazenado em um fluxo sem buffer que só pode ser lido uma vez.

## <a name="type-converters"></a>Conversores de tipo

Você pode fazer com que a API da Web trate uma classe como um tipo simples (para que a API da Web tente associá-la a partir do URI) criando um **TypeConverter** e fornecendo uma conversão de cadeia de caracteres.

O código a seguir mostra uma `GeoPoint` classe que representa um ponto geográfico, além de um **TypeConverter** que converte de cadeias de caracteres em `GeoPoint` instâncias. A `GeoPoint` classe é decorada com um atributo **[TypeConverter]** para especificar o conversor de tipo. (Este exemplo foi inspirado pela postagem no blog de Mike Stall [como associar a objetos personalizados em assinaturas de ação no MVC/WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx).)

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample6.cs)]

Agora, a API da Web tratará `GeoPoint` como um tipo simples, o que significa que tentará associar `GeoPoint` os parâmetros do URI. Você não precisa incluir **[FromUri]** no parâmetro.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample7.cs)]

O cliente pode invocar o método com um URI como este:

`http://localhost/api/values/?location=47.678558,-122.130989`

## <a name="model-binders"></a>ASSOCIADORES de modelo

Uma opção mais flexível do que um conversor de tipo é criar um associador de modelo personalizado. Com um associador de modelo, você tem acesso a coisas como a solicitação HTTP, a descrição da ação e os valores brutos dos dados da rota.

Para criar um associador de modelo, implemente a interface **IModelBinder** . Essa interface define um único método, **BindModel**:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample8.cs)]

Aqui está um associador de modelo para `GeoPoint` objetos.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample9.cs)]

Um associador de modelo obtém valores de entrada brutos de um *provedor de valor*. Esse design separa duas funções distintas:

- O provedor de valor pega a solicitação HTTP e popula um dicionário de pares chave-valor.
- O associador de modelo usa esse dicionário para preencher o modelo.

O provedor de valor padrão na API Web obtém valores dos dados de rota e da cadeia de caracteres de consulta. Por exemplo, se o URI for `http://localhost/api/values/1?location=48,-122` , o provedor de valor criará os seguintes pares de chave-valor:

- ID = &quot; 1&quot;
- local = &quot; 48,-122&quot;

(Estou supondo que o modelo de rota padrão, que é &quot; API/{Controller}/{ID} &quot; .)

O nome do parâmetro a ser associado é armazenado na propriedade **ModelBindingContext. ModelName** . O associador de modelo procura uma chave com esse valor no dicionário. Se o valor existir e puder ser convertido em um `GeoPoint` , o associador de modelo atribuirá o valor associado à propriedade **ModelBindingContext. Model** .

Observe que o associador de modelo não está limitado a uma conversão de tipo simples. Neste exemplo, o associador de modelo primeiro procura em uma tabela de locais conhecidos e, se isso falhar, usará a conversão de tipo.

**Configurando o associador de modelo**

Há várias maneiras de definir um associador de modelo. Primeiro, você pode adicionar um atributo **[ModelBinder]** ao parâmetro.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample10.cs)]

Você também pode adicionar um atributo **[ModelBinder]** ao tipo. A API Web usará o associador de modelo especificado para todos os parâmetros desse tipo.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample11.cs)]

Por fim, você pode adicionar um provedor de associador de modelo ao **HttpConfiguration**. Um provedor de associador de modelo é simplesmente uma classe de fábrica que cria um associador de modelo. Você pode criar um provedor derivando da classe [ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx) . No entanto, se o seu associador de modelo tratar de um único tipo, será mais fácil usar o **SimpleModelBinderProvider** interno, que foi projetado para essa finalidade. O código a seguir mostra como fazer isso.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample12.cs)]

Com um provedor de associação de modelo, você ainda precisa adicionar o atributo **[ModelBinder]** ao parâmetro para informar à API Web que ele deve usar um associador de modelo e não um formatador de tipo de mídia. Mas agora você não precisa especificar o tipo de associador de modelo no atributo:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample13.cs)]

## <a name="value-providers"></a>Provedores de valor

Mencionei que um associador de modelo obtém valores de um provedor de valor. Para gravar um provedor de valor personalizado, implemente a interface **IValueProvider** . Aqui está um exemplo que efetua pull dos valores dos cookies na solicitação:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample14.cs)]

Você também precisa criar um alocador de provedor de valor derivando da classe **ValueProviderFactory** .

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample15.cs)]

Adicione o alocador de provedor de valor ao **HttpConfiguration** da seguinte maneira.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample16.cs)]

A API Web compõe todos os provedores de valor, portanto, quando um associador de modelo chama **ValueProvider. GetValue**, o associador de modelo recebe o valor do provedor de primeiro valor que é capaz de produzi-lo.

Como alternativa, você pode definir o alocador de provedor de valor no nível de parâmetro usando o atributo **ValueProvider** , da seguinte maneira:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample17.cs)]

Isso informa à API da Web para usar a associação de modelo com o alocador de provedor de valor especificado e não para usar qualquer um dos outros provedores de valor registrado.

## <a name="httpparameterbinding"></a>HttpParameterBinding

Os vinculadores de modelo são uma instância específica de um mecanismo mais geral. Se você olhar o atributo **[ModelBinder]** , verá que ele deriva da classe abstrata **ParameterBindingAttribute** . Essa classe define um método único, **GetBinding**, que retorna um objeto **HttpParameterBinding** :

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample18.cs)]

Um **HttpParameterBinding** é responsável por associar um parâmetro a um valor. No caso de **[ModelBinder]**, o atributo retorna uma implementação de **HttpParameterBinding** que usa um **IModelBinder** para executar a associação real. Você também pode implementar seu próprio **HttpParameterBinding**.

Por exemplo, suponha que você queira obter ETags de `if-match` `if-none-match` cabeçalhos e na solicitação. Vamos começar definindo uma classe para representar ETags.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample19.cs)]

Também definiremos uma enumeração para indicar se deve obter a ETag do `if-match` cabeçalho ou do `if-none-match` cabeçalho.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample20.cs)]

Aqui está um **HttpParameterBinding** que obtém a eTag do cabeçalho desejado e a associa a um parâmetro do tipo ETag:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample21.cs)]

O método **ExecuteBindingAsync** faz a associação. Dentro desse método, adicione o valor do parâmetro Bound ao dicionário **ActionArgument** no **HttpActionContext**.

> [!NOTE]
> Se o método **ExecuteBindingAsync** ler o corpo da mensagem de solicitação, substitua a propriedade **WillReadBody** para retornar true. O corpo da solicitação pode ser um fluxo sem buffer que só pode ser lido uma vez, portanto, a API Web impõe uma regra que, no máximo, uma associação pode ler o corpo da mensagem.

Para aplicar um **HttpParameterBinding** personalizado, você pode definir um atributo que deriva de **ParameterBindingAttribute**. Para `ETagParameterBinding` , vamos definir dois atributos, um para `if-match` cabeçalhos e outro para `if-none-match` cabeçalhos. Ambos derivam de uma classe base abstrata.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample22.cs)]

Aqui está um método de controlador que usa o `[IfNoneMatch]` atributo.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample23.cs)]

Além de **ParameterBindingAttribute**, há outro gancho para adicionar um **HttpParameterBinding** personalizado. No objeto **HttpConfiguration** , a propriedade **ParameterBindingRules** é uma coleção de funções anônimas do tipo (**HttpParameterDescriptor**  - &gt; **HttpParameterBinding**). Por exemplo, você pode adicionar uma regra que qualquer parâmetro de ETag em um método GET usa `ETagParameterBinding` com `if-none-match` :

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample24.cs)]

A função deve retornar `null` para parâmetros em que a associação não é aplicável.

## <a name="iactionvaluebinder"></a>IActionValueBinder

O processo de vinculação de parâmetro inteiro é controlado por um serviço conectável, **IActionValueBinder**. A implementação padrão de **IActionValueBinder** faz o seguinte:

1. Procure um **ParameterBindingAttribute** no parâmetro. Isso inclui **[FromBody]**, **[FromUri]** e **[ModelBinder]**, ou atributos personalizados.
2. Caso contrário, examine **HttpConfiguration. ParameterBindingRules** para uma função que retorna um **HttpParameterBinding** não nulo.
3. Caso contrário, use as regras padrão que descrevi anteriormente. 

    - Se o tipo de parâmetro for "simples" ou tiver um conversor de tipo, associe a partir do URI. Isso é equivalente a colocar o atributo **[FromUri]** no parâmetro.
    - Caso contrário, tente ler o parâmetro no corpo da mensagem. Isso é equivalente a colocar **[FromBody]** no parâmetro.

Se desejar, você poderia substituir todo o serviço **IActionValueBinder** por uma implementação personalizada.

## <a name="additional-resources"></a>Recursos adicionais

[Exemplo de associação de parâmetro personalizado](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/CustomParameterBinding)

Mike Stall escreveu uma boa série de postagens no blog sobre associação de parâmetro da API Web:

- [Como a API Web faz a associação de parâmetros](https://blogs.msdn.com/b/jmstall/archive/2012/04/16/how-webapi-does-parameter-binding.aspx)
- [Associação de parâmetro de estilo MVC para API Web](https://blogs.msdn.com/b/jmstall/archive/2012/04/18/mvc-style-parameter-binding-for-webapi.aspx)
- [Como associar a objetos personalizados em assinaturas de ação no MVC/API da Web](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx)
- [Como criar um provedor de valor personalizado na API Web](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)
- [Associação de parâmetro de API Web nos bastidores](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx)
