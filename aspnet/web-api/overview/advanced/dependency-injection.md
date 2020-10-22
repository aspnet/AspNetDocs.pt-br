---
uid: web-api/overview/advanced/dependency-injection
title: Injeção de dependência no ASP.NET Web API 2-ASP.NET 4. x
author: MikeWasson
description: Este tutorial mostra como injetar dependências em seu controlador de ASP.NET Web API para o ASP.NET 4. x.
ms.author: riande
ms.date: 01/20/2014
ms.custom: seoapril2019
ms.assetid: e3d3e7ba-87f0-4032-bdd3-31f3c1aa9d9c
msc.legacyurl: /web-api/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 3342d93340215d937cf7161ee1c4b32931d516a3
ms.sourcegitcommit: c62ec20b453cee3249eb894ecd75013b57d078f0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/21/2020
ms.locfileid: "92345267"
---
# <a name="dependency-injection-in-aspnet-web-api-2"></a>Injeção de dependência no ASP.NET Web API 2

por [Mike Wasson](https://github.com/MikeWasson)

[Baixar projeto concluído](https://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-468ee148)

> Este tutorial mostra como injetar dependências em seu controlador de ASP.NET Web API.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - API Web 2
> - [Bloco de aplicativo do Unity](https://www.nuget.org/packages/Unity/)
> - Entity Framework 6 (a versão 5 também funciona)

## <a name="what-is-dependency-injection"></a>O que é injeção de dependência?

Uma *dependência* é qualquer objeto exigido por outro objeto. Por exemplo, é comum definir um [repositório](http://martinfowler.com/eaaCatalog/repository.html) que lide com o acesso a dados. Vamos ilustrar com um exemplo. Primeiro, vamos definir um modelo de domínio:

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

Aqui está uma classe de repositório simples que armazena itens em um banco de dados, usando Entity Framework.

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

Agora, vamos definir um controlador de API da Web que dá suporte a solicitações GET para `Product` entidades. (Estou deixando o POST e outros métodos para simplificar.) Esta é uma primeira tentativa:

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

Observe que a classe Controller depende `ProductRepository` e estamos permitindo que o controlador crie a `ProductRepository` instância. No entanto, é uma boa ideia codificar a dependência dessa forma, por vários motivos.

- Se você quiser substituir `ProductRepository` por uma implementação diferente, também precisará modificar a classe do controlador.
- Se o `ProductRepository` tiver dependências, você deverá configurá-las dentro do controlador. Para um projeto grande com vários controladores, seu código de configuração se torna disperso em seu projeto.
- É difícil fazer o teste de unidade, pois o controlador é embutido em código para consultar o banco de dados. Para um teste de unidade, você deve usar um repositório de simulação ou de stub, o que não é possível com o design atual.

Podemos resolver esses problemas *injetando* o repositório no controlador. Primeiro, refatore a `ProductRepository` classe em uma interface:

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

Em seguida, forneça o `IProductRepository` como um parâmetro de construtor:

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

Este exemplo usa [injeção de Construtor](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection). Você também pode usar a *injeção de setter*, em que você define a dependência por meio de um método ou propriedade setter.

Mas agora há um problema, porque seu aplicativo não cria o controlador diretamente. A API da Web cria o controlador quando ele roteia a solicitação e a API da Web não sabe nada sobre o `IProductRepository` . É aí que entra o resolvedor de dependências da API Web.

## <a name="the-web-api-dependency-resolver"></a>O resolvedor de dependência da API Web

A API Web define a interface **IDependencyResolver** para resolver dependências. Aqui está a definição da interface:

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

A interface **IDependencyScope** tem dois métodos:

- **GetService** cria uma instância de um tipo.
- **GetServices** cria uma coleção de objetos de um tipo especificado.

O método **IDependencyResolver** herda **IDependencyScope** e adiciona o método **BeginScope** . Falarei sobre escopos mais adiante neste tutorial.

Quando a API da Web cria uma instância de controlador, ela primeiro chama **IDependencyResolver. GetService**, passando o tipo de controlador. Você pode usar esse gancho de extensibilidade para criar o controlador, resolvendo quaisquer dependências. Se **GetService** retornar NULL, a API da Web procurará um construtor sem parâmetros na classe do controlador.

## <a name="dependency-resolution-with-the-unity-container"></a>Resolução de dependência com o contêiner do Unity

Embora você possa escrever uma implementação de **IDependencyResolver** completa do zero, a interface é realmente projetada para atuar como ponte entre a API da Web e os contêineres IOC existentes.

Um contêiner IoC é um componente de software que é responsável por gerenciar dependências. Você registra os tipos com o contêiner e, em seguida, usa o contêiner para criar objetos. O contêiner ilustra automaticamente as relações de dependência. Muitos contêineres IoC também permitem que você controle coisas como o tempo de vida e o escopo do objeto.

> [!NOTE]
> "IoC" significa "inversão de controle", que é um padrão geral em que uma estrutura chama o código do aplicativo. Um contêiner IoC constrói seus objetos para você, que "inverte" o fluxo de controle usual.

Para este tutorial, usaremos o [Unity](https://msdn.microsoft.com/library/ff647202.aspx) de práticas de padrões da Microsoft &amp; . (Outras bibliotecas populares incluem [Castle Windsor](http://www.castleproject.org/), [Spring.net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/)e [StructureMap](http://structuremap.github.io/documentation/).) Você pode usar o Gerenciador de pacotes NuGet para instalar o Unity. No menu **ferramentas** no Visual Studio, selecione **Gerenciador de pacotes NuGet**e, em seguida, selecione **console do Gerenciador de pacotes**. Na janela do console do Gerenciador de pacotes, digite o seguinte comando:

[!code-console[Main](dependency-injection/samples/sample7.cmd)]

Aqui está uma implementação de **IDependencyResolver** que encapsula um contêiner do Unity.

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

## <a name="configuring-the-dependency-resolver"></a>Configurando o resolvedor de dependência

Defina o resolvedor de dependência na propriedade **DependencyResolver** do objeto **HttpConfiguration** global.

O código a seguir registra a `IProductRepository` interface com o Unity e, em seguida, cria um `UnityResolver` .

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

## <a name="dependency-scope-and-controller-lifetime"></a>Tempo de vida do controlador e do escopo de dependência

Os controladores são criados por solicitação. Para gerenciar tempos de vida de objeto, **IDependencyResolver** usa o conceito de um *escopo*.

O resolvedor de dependência anexado ao objeto **HttpConfiguration** tem escopo global. Quando a API da Web cria um controlador, ele chama **BeginScope**. Esse método retorna um **IDependencyScope** que representa um escopo filho.

A API Web então chama **GetService** no escopo filho para criar o controlador. Quando a solicitação for concluída, as chamadas da API Web serão **descartadas** no escopo filho. Use o método **Dispose** para descartar as dependências do controlador.

A maneira como você implementa o **BeginScope** depende do contêiner IoC. Para Unity, o escopo corresponde a um contêiner filho:

[!code-csharp[Main](dependency-injection/samples/sample10.cs)]

A maioria dos contêineres IoC tem equivalentes semelhantes.
