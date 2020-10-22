---
uid: web-api/overview/security/authentication-and-authorization-in-aspnet-web-api
title: Autenticação e autorização no ASP.NET Web API | Microsoft Docs
author: MikeWasson
description: Fornece uma visão geral da autenticação e autorização no ASP.NET Web API.
ms.author: riande
ms.date: 11/27/2012
ms.assetid: 6dfb51ea-9f4d-4e70-916c-8ef8344a88d6
msc.legacyurl: /web-api/overview/security/authentication-and-authorization-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 368d2b9456d12b2bb4063a23333e5c8837faa3b8
ms.sourcegitcommit: c62ec20b453cee3249eb894ecd75013b57d078f0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/21/2020
ms.locfileid: "92345623"
---
# <a name="authentication-and-authorization-in-aspnet-web-api"></a>Autenticação e autorização no ASP.NET Web API

por [Mike Wasson](https://github.com/MikeWasson)

Você criou uma API da Web, mas agora deseja controlar o acesso a ela. Nesta série de artigos, veremos algumas opções para proteger uma API da Web de usuários não autorizados. Esta série abrangerá a autenticação e a autorização.

- A *autenticação* está sabendo a identidade do usuário. Por exemplo, Alice faz logon com seu nome de usuário e senha e o servidor usa a senha para autenticar Alice.
- A *autorização* está decidindo se um usuário tem permissão para executar uma ação. Por exemplo, Alice tem permissão para obter um recurso, mas não criar um recurso.

O primeiro artigo da série fornece uma visão geral da autenticação e autorização no ASP.NET Web API. Outros tópicos descrevem cenários comuns de autenticação para a API Web.

> [!NOTE]
> Graças às pessoas que revisaram esta série e forneceram comentários valiosos: Rick Anderson, arrecadar Broderick, Barry Dorrans, Tom Dykstra, Hongmei GE, David Matson, Daniel Roth, Tim Teebken.

## <a name="authentication"></a>Autenticação

A API da Web pressupõe que a autenticação ocorre no host. Para hospedagem na Web, o host é o IIS, que usa módulos HTTP para autenticação. Você pode configurar seu projeto para usar qualquer um dos módulos de autenticação internos para IIS ou ASP.NET, ou gravar seu próprio módulo HTTP para executar a autenticação personalizada.

Quando o host autentica o usuário, ele cria uma *entidade*, que é um objeto [IPrincipal](https://msdn.microsoft.com/library/System.Security.Principal.IPrincipal.aspx) que representa o contexto de segurança no qual o código está sendo executado. O host anexa a entidade de segurança ao thread atual definindo **thread. CurrentPrincipal**. A entidade de segurança contém um objeto de **identidade** associado que contém informações sobre o usuário. Se o usuário for autenticado, a propriedade **Identity. IsAuthenticated** retornará **true**. Para solicitações anônimas, **IsAuthenticated** retorna **false**. Para obter mais informações sobre entidades, consulte [segurança baseada em funções](https://msdn.microsoft.com/library/shz8h065.aspx).

### <a name="http-message-handlers-for-authentication"></a>Manipuladores de mensagens HTTP para autenticação

Em vez de usar o host para autenticação, você pode colocar a lógica de autenticação em um [manipulador de mensagens http](../advanced/http-message-handlers.md). Nesse caso, o manipulador de mensagens examina a solicitação HTTP e define a entidade de segurança.

Quando você deve usar manipuladores de mensagens para autenticação? Aqui estão algumas vantagens:

- Um módulo HTTP vê todas as solicitações que passam pelo pipeline ASP.NET. Um manipulador de mensagens vê apenas as solicitações que são roteadas para a API da Web.
- Você pode definir manipuladores de mensagens por rota, o que permite aplicar um esquema de autenticação a uma rota específica.
- Os módulos HTTP são específicos do IIS. Os manipuladores de mensagens são independentes de host, para que possam ser usados com hospedagem Web e hospedagem interna.
- Os módulos HTTP participam do log do IIS, auditoria e assim por diante.
- Os módulos HTTP são executados anteriormente no pipeline. Se você tratar a autenticação em um manipulador de mensagens, a entidade de segurança não será definida até que o manipulador seja executado. Além disso, a entidade de segurança volta à entidade anterior quando a resposta deixa o manipulador de mensagens.

Em geral, se você não precisar oferecer suporte a hospedagem interna, um módulo HTTP é uma opção melhor. Se você precisar oferecer suporte à hospedagem interna, considere um manipulador de mensagens.

### <a name="setting-the-principal"></a>Definindo a entidade de segurança

Se o aplicativo executar qualquer lógica de autenticação personalizada, você deverá definir a entidade de segurança em dois locais:

- **Thread. CurrentPrincipal**. Essa propriedade é a maneira padrão de definir a entidade de segurança do thread no .NET.
- **HttpContext. Current. User**. Essa propriedade é específica para ASP.NET.

O código a seguir mostra como definir a entidade de segurança:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample1.cs)]

Para hospedagem na Web, você deve definir a entidade de segurança em ambos os locais; caso contrário, o contexto de segurança poderá se tornar inconsistente. Para hospedagem interna, no entanto, **HttpContext. Current** é nulo. Para garantir que seu código seja independente de host, portanto, verifique se há algum NULL antes de atribuir a **HttpContext. Current**, conforme mostrado.

## <a name="authorization"></a>Autorização

A autorização ocorre posteriormente no pipeline, mais perto do controlador. Isso permite que você faça escolhas mais granulares ao conceder acesso a recursos.

- Os *filtros de autorização* são executados antes da ação do controlador. Se a solicitação não for autorizada, o filtro retornará uma resposta de erro e a ação não será invocada.
- Dentro de uma ação do controlador, você pode obter a entidade de segurança atual da propriedade **ApiController. User** . Por exemplo, você pode filtrar uma lista de recursos com base no nome de usuário, retornando somente os recursos que pertencem a esse usuário.

![](authentication-and-authorization-in-aspnet-web-api/_static/image1.png)

<a id="auth3"></a>
### <a name="using-the-authorize-attribute"></a>Usando o atributo [autorizar]

A API Web fornece um filtro de autorização interno, [authorattribute](https://msdn.microsoft.com/library/system.web.http.authorizeattribute.aspx). Esse filtro verifica se o usuário está autenticado. Caso contrário, ele retornará o código de status HTTP 401 (não autorizado), sem invocar a ação.

Você pode aplicar o filtro globalmente, no nível do controlador ou no nível de ações individuais.

**Globalmente**: para restringir o acesso a cada controlador de API da Web, adicione o filtro **authorattribute** à lista de filtros globais:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample2.cs)]

**Controlador**: para restringir o acesso de um controlador específico, adicione o filtro como um atributo ao controlador:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample3.cs)]

**Ação**: para restringir o acesso para ações específicas, adicione o atributo ao método de ação:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample4.cs)]

Como alternativa, você pode restringir o controlador e, em seguida, permitir o acesso anônimo a ações específicas usando o `[AllowAnonymous]` atributo. No exemplo a seguir, o `Post` método é restrito, mas o `Get` método permite acesso anônimo.

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample5.cs)]

Nos exemplos anteriores, o filtro permite que qualquer usuário autenticado acesse os métodos restritos; Somente usuários anônimos são mantidos. Você também pode limitar o acesso a usuários específicos ou a usuários em funções específicas:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample6.cs)]

> [!NOTE]
> O filtro **authorattribute** para controladores de API da Web está localizado no namespace **System. Web. http** . Há um filtro semelhante para controladores MVC no namespace **System. Web. Mvc** , que não é compatível com os controladores de API Web.

### <a name="custom-authorization-filters"></a>Filtros de autorização personalizados

Para escrever um filtro de autorização personalizado, derive de um destes tipos:

- **Authorattribute**. Estenda essa classe para executar a lógica de autorização com base no usuário atual e nas funções do usuário.
- **AuthorizationFilterAttribute**. Estenda essa classe para executar a lógica de autorização síncrona que não é necessariamente baseada no usuário ou na função atual.
- **IAuthorizationFilter**. Implemente essa interface para executar a lógica de autorização assíncrona; por exemplo, se a lógica de autorização fizer chamadas assíncronas de e/s ou de rede. (Se a lógica de autorização estiver associada à CPU, será mais simples derivar de **AuthorizationFilterAttribute**, pois você não precisa escrever um método assíncrono.)

O diagrama a seguir mostra a hierarquia de classes para a classe **authorattribute** .

![](authentication-and-authorization-in-aspnet-web-api/_static/image2.png)

### <a name="authorization-inside-a-controller-action"></a>Autorização dentro de uma ação do controlador

Em alguns casos, você pode permitir que uma solicitação Continue, mas altere o comportamento com base na entidade de segurança. Por exemplo, as informações que você retorna podem ser alteradas dependendo da função do usuário. Dentro de um método de controlador, você pode obter a entidade de segurança atual da propriedade **ApiController. User** .

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample7.cs)]
