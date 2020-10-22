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
# <a name="routing-in-aspnet-web-api"></a>Roteamento no ASP.NET Web API

por [Mike Wasson](https://github.com/MikeWasson)

Este artigo descreve como ASP.NET Web API roteia as solicitações HTTP para os controladores.

> [!NOTE]
> Se você estiver familiarizado com o ASP.NET MVC, o roteamento da API Web é muito semelhante ao roteamento do MVC. A principal diferença é que a API da Web usa o verbo HTTP, não o caminho do URI, para selecionar a ação. Você também pode usar o roteamento no estilo MVC na API da Web. Este artigo não assume nenhum conhecimento do ASP.NET MVC.

## <a name="routing-tables"></a>Tabelas de roteamento

No ASP.NET Web API, um *controlador* é uma classe que MANIPULA solicitações HTTP. Os métodos públicos do controlador são chamados de *métodos de ação* ou simplesmente *ações*. Quando a estrutura da API Web recebe uma solicitação, ela roteia a solicitação para uma ação.

Para determinar qual ação invocar, a estrutura usa uma *tabela de roteamento*. O modelo de projeto do Visual Studio para API da Web cria uma rota padrão:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample1.cs)]

Essa rota é definida no arquivo *WebApiConfig.cs* , que é colocado no diretório * \_ inicial do aplicativo* :

![](routing-in-aspnet-web-api/_static/image1.png)

Para obter mais informações sobre a `WebApiConfig` classe, consulte [configurando ASP.NET Web API](../advanced/configuring-aspnet-web-api.md).

Se você hospedar a API Web de hospedagem própria, deverá definir a tabela de roteamento diretamente no `HttpSelfHostConfiguration` objeto. Para obter mais informações, consulte [auto-hospedar uma API Web](../older-versions/self-host-a-web-api.md).

Cada entrada na tabela de roteamento contém um *modelo de rota*. O modelo de rota padrão para a API Web é &quot; API/{Controller}/{ID} &quot; . Neste modelo, &quot; a API &quot; é um segmento de caminho literal e {Controller} e {ID} são variáveis de espaço reservado.

Quando a estrutura da API Web recebe uma solicitação HTTP, ela tenta corresponder o URI em um dos modelos de rota na tabela de roteamento. Se nenhuma rota corresponder, o cliente receberá um erro 404. Por exemplo, os seguintes URIs correspondem à rota padrão:

- /api/contacts
- /api/contacts/1
- /api/products/gizmo1

No entanto, o URI a seguir não corresponde, pois ele não tem o &quot; segmento de API &quot; :

- /contacts/1

> [!NOTE]
> O motivo para usar "API" na rota é evitar colisões com o roteamento MVC do ASP.NET. Dessa forma, você pode fazer com que &quot; &quot; o/Contacts vá para um controlador MVC e &quot; /API/Contacts &quot; vá para um controlador de API da Web. É claro que, se você não gostar dessa Convenção, poderá alterar a tabela de rotas padrão.

Quando uma rota correspondente é encontrada, a API da Web seleciona o controlador e a ação:

- Para localizar o controlador, a API Web &quot; adiciona &quot; o controlador ao valor da variável *{Controller}* .
- Para localizar a ação, a API Web examina o verbo HTTP e, em seguida, procura uma ação cujo nome começa com esse nome de verbo HTTP. Por exemplo, com uma solicitação GET, a API da Web procura uma ação prefixada com &quot; Get &quot; , como &quot; GetContact &quot; ou &quot; GetAllContacts &quot; . Essa convenção se aplica somente aos verbos de GET, POST, PUT, excluir, cabeçalho, opções e PATCH. Você pode habilitar outros verbos HTTP usando atributos em seu controlador. Veremos um exemplo disso mais tarde.
- Outras variáveis de espaço reservado no modelo de rota, como *{ID},* são mapeadas para parâmetros de ação.

Vejamos um exemplo. Suponha que você defina o controlador a seguir:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample2.cs)]

Aqui estão algumas solicitações HTTP possíveis, juntamente com a ação que é invocada para cada uma:

| Verbo HTTP | Caminho do URI | Ação | Parâmetro |
| --- | --- | --- | --- |
| GET | API/produtos | GetAllProducts | *None* |
| GET | API/produtos/4 | GetProductById | 4 |
| Delete (excluir) | API/produtos/4 | DeleteProduct | 4 |
| POST | API/produtos | *(sem correspondência)* |  |

Observe que o segmento *{ID}* do URI, se presente, está mapeado para o parâmetro *ID* da ação. Neste exemplo, o controlador define dois métodos GET, um com um parâmetro de *ID* e outro sem parâmetros.

Além disso, observe que a solicitação POST falhará, porque o controlador não define um &quot; método post.... &quot;

## <a name="routing-variations"></a>Variações de roteamento

A seção anterior descreveu o mecanismo de roteamento básico para ASP.NET Web API. Esta seção descreve algumas variações.

### <a name="http-verbs"></a>Verbos HTTP

Em vez de usar a Convenção de nomenclatura para verbos HTTP, você pode especificar explicitamente o verbo HTTP para uma ação decorando o método de ação com um dos seguintes atributos:

- `[HttpGet]`
- `[HttpPut]`
- `[HttpPost]`
- `[HttpDelete]`
- `[HttpHead]`
- `[HttpOptions]`
- `[HttpPatch]`

No exemplo a seguir, o `FindProduct` método é mapeado para solicitações GET:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample3.cs)]

Para permitir vários verbos HTTP para uma ação ou para permitir verbos HTTP diferentes de GET, PUT, POST, DELETE, HEAD, OPTIONS e PATCH, use o `[AcceptVerbs]` atributo, que usa uma lista de VERBOS http.

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample4.cs)]

<a id="routing_by_action_name"></a>
### <a name="routing-by-action-name"></a>Roteamento por nome de ação

Com o modelo de roteamento padrão, a API da Web usa o verbo HTTP para selecionar a ação. No entanto, você também pode criar uma rota onde o nome da ação está incluído no URI:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample5.cs)]

Nesse modelo de rota, o parâmetro *{Action}* nomeia o método de ação no controlador. Com esse estilo de roteamento, use atributos para especificar os verbos HTTP permitidos. Por exemplo, suponha que o controlador tenha o seguinte método:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample6.cs)]

Nesse caso, uma solicitação GET para "API/produtos/detalhes/1" mapearia para o `Details` método. Esse estilo de roteamento é semelhante ao ASP.NET MVC e pode ser apropriado para uma API de estilo RPC.

Você pode substituir o nome da ação usando o `[ActionName]` atributo. No exemplo a seguir, há duas ações que são mapeadas para &quot; API/Products/thumbnail/*ID*. Um é compatível com GET e o outro oferece suporte a POST:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample7.cs)]

### <a name="non-actions"></a>Não ações

Para impedir que um método seja invocado como uma ação, use o `[NonAction]` atributo. Isso sinaliza para a estrutura que o método não é uma ação, mesmo que, caso contrário, correspondam às regras de roteamento.

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample8.cs)]

## <a name="further-reading"></a>Leitura Adicional

Este tópico forneceu uma exibição de alto nível do roteamento. Para obter mais detalhes, consulte [seleção de roteamento e ação](routing-and-action-selection.md), que descreve exatamente como a estrutura corresponde a um URI para uma rota, seleciona um controlador e, em seguida, seleciona a ação a ser invocada.
