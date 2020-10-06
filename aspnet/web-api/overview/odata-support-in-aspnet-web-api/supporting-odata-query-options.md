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
ms.lasthandoff: 10/06/2020
ms.locfileid: "86188601"
---
# <a name="supporting-odata-query-options-in-aspnet-web-api-2"></a>Suporte a opções de consulta OData no ASP.NET Web API 2

por [Mike Wasson](https://github.com/MikeWasson)

Esta visão geral com exemplos de código demonstra as opções de consulta OData de suporte no ASP.NET Web API 2 para ASP.NET 4. x. 

O OData define parâmetros que podem ser usados para modificar uma consulta OData. O cliente envia esses parâmetros na cadeia de caracteres de consulta do URI de solicitação. Por exemplo, para classificar os resultados, um cliente usa o parâmetro $orderby:

`http://localhost/Products?$orderby=Name`

A especificação OData chama essas *Opções de consulta*de parâmetros. Você pode habilitar as opções de consulta OData para qualquer controlador de API Web em seu projeto &#8212; o controlador não precisa ser um ponto de extremidade OData. Isso lhe dá uma maneira conveniente de adicionar recursos como filtragem e classificação a qualquer aplicativo de API Web.

Antes de habilitar as opções de consulta, leia o tópico [diretrizes de segurança do OData](odata-security-guidance.md).

- [Habilitando opções de consulta OData](#enable)
- [Exemplos de consultas](#examples)
- [Paginação controlada por servidor](#server-paging)
- [Limitando as opções de consulta](#limiting_query_options)
- [Invocando opções de consulta diretamente](#ODataQueryOptions)
- [Validação de consulta](#query-validation)

<a id="enable"></a>
## <a name="enabling-odata-query-options"></a>Habilitando opções de consulta OData

A API Web dá suporte às seguintes opções de consulta OData:

| Opção | Descrição |
| --- | --- |
| $expand | Expande as entidades relacionadas embutidas. |
| $filter | Filtra os resultados, com base em uma condição booliana. |
| $inlinecount | Instrui o servidor a incluir a contagem total de entidades correspondentes na resposta. (Útil para paginação do lado do servidor). |
| $orderby | Classifica os resultados. |
| $select | Seleciona quais propriedades incluir na resposta. |
| $skip | Ignora os primeiros n resultados. |
| $top | Retorna apenas os n primeiros resultados. |

Para usar as opções de consulta OData, você deve habilitá-las explicitamente. Você pode habilitá-los globalmente para todo o aplicativo ou habilitá-los para controladores específicos ou ações específicas.

Para habilitar as opções de consulta OData globalmente, chame **EnableQuerySupport** na classe **HttpConfiguration** na inicialização:

[!code-csharp[Main](supporting-odata-query-options/samples/sample1.cs)]

O método **EnableQuerySupport** habilita as opções de consulta globalmente para qualquer ação do controlador que retorna um tipo **IQueryable** . Se você não quiser que as opções de consulta sejam habilitadas para todo o aplicativo, poderá habilitá-las para ações de controlador específicas adicionando o atributo **[consultável]** ao método de ação.

[!code-csharp[Main](supporting-odata-query-options/samples/sample2.cs)]

<a id="examples"></a>
## <a name="example-queries"></a>Consultas de Exemplo

Esta seção mostra os tipos de consultas que são possíveis usando as opções de consulta OData. Para obter detalhes específicos sobre as opções de consulta, consulte a documentação do OData em [www.OData.org](http://www.odata.org/).

Para obter informações sobre $expand e $select, consulte [usando $Select, $Expand e $Value no ASP.NET Web API OData](using-select-expand-and-value.md).

**Paginação controlada pelo cliente**

Para conjuntos de entidades grandes, o cliente pode querer limitar o número de resultados. Por exemplo, um cliente pode mostrar 10 entradas por vez, com links "Next" para obter a próxima página de resultados. Para fazer isso, o cliente usa as opções $top e $skip.

`http://localhost/Products?$top=10&$skip=20`

A opção $top fornece o número máximo de entradas a serem retornadas e a opção $skip fornece o número de entradas a serem ignoradas. O exemplo anterior busca as entradas 21 a 30.

**Filtragem**

A opção $filter permite que um cliente filtre os resultados aplicando uma expressão booliana. As expressões de filtro são bastante poderosas; Eles incluem operadores lógicos e aritméticos, funções de cadeia de caracteres e funções de data.

| Retorne todos os produtos com categoria igual a "brinquedos". | `http://localhost/Products?$filter=Category` EQ ' Toys ' |
| --- | --- |
| Retornar todos os produtos com o preço inferior a 10. | `http://localhost/Products?$filter=Price` lt 10 |
| Operadores lógicos: retorna todos os produtos em que Price >= 5 e Price <= 15. | `http://localhost/Products?$filter=Price` GE 5 e preço Le 15 |
| Funções de cadeia de caracteres: retorna todos os produtos com "zz" no nome. | `http://localhost/Products?$filter=substringof('zz',Name)` |
| Funções de data: retorna todos os produtos com liberados após 2005. | `http://localhost/Products?$filter=year(ReleaseDate)` gt 2005 |

**Classificação**

Para classificar os resultados, use o filtro $orderby.

| Classificar por preço. | `http://localhost/Products?$orderby=Price` |
| --- | --- |
| Classificar por preço em ordem decrescente (mais alto para o mais baixo). | `http://localhost/Products?$orderby=Price desc` |
| Classifique por categoria e, em seguida, classifique por preço em ordem decrescente dentro de categorias. | `http://localhost/odata/Products?$orderby=Category,Price desc` |

<a id="server-paging"></a>
## <a name="server-driven-paging"></a>Paginação controlada por servidor

Se o seu banco de dados contiver milhões de registros, você não desejará enviar todos eles em uma carga. Para evitar isso, o servidor pode limitar o número de entradas que ele envia em uma única resposta. Para habilitar a paginação do servidor, defina a propriedade **PageSize** no atributo **passível** de consulta. O valor é o número máximo de entradas a serem retornadas.

[!code-csharp[Main](supporting-odata-query-options/samples/sample3.cs)]

Se o controlador retornar o formato OData, o corpo da resposta conterá um link para a próxima página de dados:

[!code-json[Main](supporting-odata-query-options/samples/sample4.json?highlight=8)]

O cliente pode usar esse link para buscar a próxima página. Para saber o número total de entradas no conjunto de resultados, o cliente pode definir a opção de consulta $inlinecount com o valor "Alpages".

`http://localhost/Products?$inlinecount=allpages`

O valor "MyPages" informa ao servidor para incluir a contagem total na resposta:

[!code-json[Main](supporting-odata-query-options/samples/sample5.json?highlight=3)]

> [!NOTE]
> Os links de próxima página e a contagem embutida exigem o formato OData. O motivo é que o OData define campos especiais no corpo da resposta para manter o link e a contagem.

Para formatos não-OData, ainda é possível dar suporte a links de próxima página e contagem embutida, encapsulando os resultados da consulta em um objeto **PageResult &lt; T &gt; ** . No entanto, ele requer um pouco mais de código. Veja um exemplo:

[!code-csharp[Main](supporting-odata-query-options/samples/sample6.cs)]

Aqui está um exemplo de resposta JSON:

[!code-json[Main](supporting-odata-query-options/samples/sample7.json)]

<a id="limiting_query_options"></a>
## <a name="limiting-the-query-options"></a>Limitando as opções de consulta

As opções de consulta dão ao cliente muito controle sobre a consulta que é executada no servidor. Em alguns casos, talvez você queira limitar as opções disponíveis por motivos de segurança ou desempenho. O atributo **[passível** de consulta] tem algumas propriedades internas para isso. Veja alguns exemplos.

Permitir somente $skip e $top, para dar suporte à paginação e nada mais:

[!code-csharp[Main](supporting-odata-query-options/samples/sample8.cs)]

Permitir a ordenação apenas de determinadas propriedades, para evitar a classificação em propriedades que não são indexadas no banco de dados:

[!code-csharp[Main](supporting-odata-query-options/samples/sample9.cs)]

Permita a função lógica "eq", mas não outras funções lógicas:

[!code-csharp[Main](supporting-odata-query-options/samples/sample10.cs)]

Não permitir operadores aritméticos:

[!code-csharp[Main](supporting-odata-query-options/samples/sample11.cs)]

Você pode restringir as opções globalmente criando uma instância **passível** de consulta e passando-a para a função **EnableQuerySupport** :

[!code-csharp[Main](supporting-odata-query-options/samples/sample12.cs)]

<a id="ODataQueryOptions"></a>
## <a name="invoking-query-options-directly"></a>Invocando opções de consulta diretamente

Em vez de usar o atributo **[consultável]** , você pode invocar as opções de consulta diretamente em seu controlador. Para fazer isso, adicione um parâmetro **ODataQueryOptions** ao método Controller. Nesse caso, você não precisa do atributo **[consultável]** .

[!code-csharp[Main](supporting-odata-query-options/samples/sample13.cs)]

A API Web popula o **ODataQueryOptions** da cadeia de caracteres de consulta do URI. Para aplicar a consulta, passe um **IQueryable** para o método **ApplyTo** . O método retorna outro **IQueryable**.

Para cenários avançados, se você não tiver um provedor de consultas **IQueryable** , poderá examinar o **ODataQueryOptions** e converter as opções de consulta em outro formulário. (Por exemplo, consulte postagem de blog de RaghuRam Nadiminti [traduzindo consultas OData para HQL](https://blogs.msdn.com/b/webdev/archive/2013/02/25/translating-odata-queries-to-hql.aspx), que também inclui um [exemplo](http://aspnet.codeplex.com/SourceControl/changeset/view/75a56ec99968#Samples/WebApi/NHibernateQueryableSample/Readme.txt).)

<a id="query-validation"></a>
## <a name="query-validation"></a>Validação de consulta

O atributo **[consultável]** valida a consulta antes de executá-la. A etapa de validação é executada no método **consultáattribute. ValidateQuery** . Você também pode personalizar o processo de validação.

Consulte também as [diretrizes de segurança do OData](odata-security-guidance.md).

Primeiro, substitua uma das classes do validador que está definida no namespace **Web. http. OData. Query. Validators** . Por exemplo, a seguinte classe de validador desabilita a opção ' DESC ' para a opção $orderby.

[!code-csharp[Main](supporting-odata-query-options/samples/sample14.cs)]

Subclasse o atributo **[passível** de consulta] para substituir o método **ValidateQuery** .

[!code-csharp[Main](supporting-odata-query-options/samples/sample15.cs)]

Em seguida, defina seu atributo personalizado globalmente ou por controlador:

[!code-csharp[Main](supporting-odata-query-options/samples/sample16.cs)]

Se você estiver usando o **ODataQueryOptions** diretamente, defina o validador nas opções:

[!code-csharp[Main](supporting-odata-query-options/samples/sample17.cs)]
