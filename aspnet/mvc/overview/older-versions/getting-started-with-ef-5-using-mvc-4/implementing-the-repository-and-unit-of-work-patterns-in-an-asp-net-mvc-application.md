---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application
title: Implementando o repositório e os padrões de unidade de trabalho em um aplicativo MVC ASP.NET (9 de 10) | Microsoft Docs
author: tdykstra
description: O aplicativo Web de exemplo da Contoso University demonstra como criar aplicativos ASP.NET MVC 4 usando o Entity Framework 5 Code First e o Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 44761193-04ba-4990-9f90-145d3c10a716
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 03579bbaf454583543f09f68ac1b94a3b1ec0d92
ms.sourcegitcommit: d5049dfb08ff14872ba3b29a702e0589a776b430
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/05/2021
ms.locfileid: "102194546"
---
# <a name="implementing-the-repository-and-unit-of-work-patterns-in-an-aspnet-mvc-application-9-of-10"></a>Implementando o repositório e os padrões de unidade de trabalho em um aplicativo MVC ASP.NET (9 de 10)

por [Tom Dykstra](https://github.com/tdykstra)


> O aplicativo Web de exemplo da Contoso University demonstra como criar aplicativos ASP.NET MVC 4 usando o Entity Framework 5 Code First e o Visual Studio 2012. Para obter informações sobre a série de tutoriais, consulte [o primeiro tutorial da série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
> 
> > [!NOTE] 
> > 
> > Se você encontrar um problema que não possa resolver, [Baixe o capítulo concluído](building-the-ef5-mvc4-chapter-downloads.md) e tente reproduzir o problema. Em geral, você pode encontrar a solução para o problema comparando seu código com o código concluído. Para alguns erros comuns e como resolvê-los, consulte [erros e soluções alternativas.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)

No tutorial anterior, você usou a herança para reduzir o código redundante `Student` nas `Instructor` classes de entidade e. Neste tutorial, você verá algumas maneiras de usar o repositório e os padrões de unidade de trabalho para operações CRUD. Como no tutorial anterior, neste, você alterará a maneira como seu código funciona com páginas que você já criou, em vez de criar novas páginas.

## <a name="the-repository-and-unit-of-work-patterns"></a>O repositório e os padrões de unidade de trabalho

O repositório e os padrões de unidade de trabalho destinam-se a criar uma camada de abstração entre a camada de acesso a dados e a camada de lógica de negócios de um aplicativo. A implementação desses padrões pode ajudar a isolar o aplicativo de alterações no armazenamento de dados e pode facilitar o teste de unidade automatizado ou TDD (desenvolvimento orientado por testes).

Neste tutorial, você implementará uma classe de repositório para cada tipo de entidade. Para o `Student` tipo de entidade, você criará uma interface de repositório e uma classe de repositório. Ao criar uma instância do repositório em seu controlador, você usará a interface para que o controlador aceite uma referência a qualquer objeto que implemente a interface do repositório. Quando o controlador é executado em um servidor Web, ele recebe um repositório que funciona com o Entity Framework. Quando o controlador é executado sob uma classe de teste de unidade, ele recebe um repositório que funciona com dados armazenados de forma que você possa manipular facilmente para teste, como uma coleção na memória.

Posteriormente no tutorial, você usará vários repositórios e uma unidade de classe de trabalho para os `Course` `Department` tipos de entidade e no `Course` controlador. A classe de unidade de trabalho coordena o trabalho de vários repositórios criando uma única classe de contexto de banco de dados compartilhada por todos eles. Se você quisesse ser capaz de executar o teste de unidade automatizado, você criaria e usará interfaces para essas classes da mesma maneira que fazia para o `Student` repositório. No entanto, para manter o tutorial simples, você criará e usará essas classes sem interfaces.

A ilustração a seguir mostra uma maneira de conceituar as relações entre o controlador e as classes de contexto em comparação com não usar o repositório ou o padrão de unidade de trabalho.

![Repository_pattern_diagram](https://asp.net/media/2578149/Windows-Live-Writer_8c4963ba1fa3_CE3B_Repository_pattern_diagram_1df790d3-bdf2-4c11-9098-946ddd9cd884.png)

Você não criará testes de unidade nesta série de tutoriais. Para obter uma introdução ao TDD com um aplicativo MVC que usa o padrão de repositório, confira [Walkthrough: usando TDD com ASP.NET MVC](https://msdn.microsoft.com/library/ff847525.aspx). Para obter mais informações sobre o padrão de repositório, consulte os seguintes recursos:

- [O padrão de repositório](https://msdn.microsoft.com/library/ff649690.aspx) no msdn.
- A série de postagens do [Agile Entity Framework 4 do repositório](http://thedatafarm.com/blog/data-access/agile-entity-framework-4-repository-part-1-model-and-poco-classes/) no blog de Julie Lerman.
- [Criando a conta em um aplicativo HTML5/jQuery resumido](https://weblogs.asp.net/dwahlin/archive/2011/08/15/building-the-account-at-a-glance-html5-jquery-application.aspx) no blog de Dan Wahlin.

> [!NOTE]
> Há várias maneiras de implementar o repositório e os padrões de unidade de trabalho. Você pode usar classes de repositório com ou sem uma classe de unidade de trabalho. Você pode implementar um único repositório para todos os tipos de entidade ou um para cada tipo. Se você implementar um para cada tipo, poderá usar classes separadas, uma classe base genérica e classes derivadas, ou uma classe base abstrata e classes derivadas. Você pode incluir lógica de negócios em seu repositório ou restringi-la à lógica de acesso a dados. Você também pode criar uma camada de abstração na sua classe de contexto de banco de dados usando interfaces [IDbSet](https://msdn.microsoft.com/library/gg679233(v=vs.103).aspx) em vez de tipos [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx) para seus conjuntos de entidades. A abordagem para implementar uma camada de abstração mostrada neste tutorial é uma opção a ser considerada, não uma recomendação para todos os cenários e ambientes.

## <a name="creating-the-student-repository-class"></a>Criando a classe de repositório Student

Na pasta *Dal* , crie um arquivo de classe chamado *IStudentRepository.cs* e substitua o código existente pelo código a seguir:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample1.cs)]

Esse código declara um conjunto típico de métodos CRUD, incluindo dois métodos de leitura — um que retorna todas as `Student` entidades e outro que localiza uma única `Student` entidade por ID.

Na pasta *Dal* , crie um arquivo de classe chamado *StudentRepository.cs* File. Substitua o código existente pelo código a seguir, que implementa a `IStudentRepository` interface:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample2.cs)]

O contexto do banco de dados é definido em uma variável de classe e o construtor espera que o objeto de chamada passe em uma instância do contexto:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample3.cs)]

Você pode criar uma instância de um novo contexto no repositório, mas, se você tiver usado vários repositórios em um controlador, cada um acabaria com um contexto separado. Posteriormente, você usará vários repositórios no `Course` controlador e verá como uma classe de unidade de trabalho pode garantir que todos os repositórios usem o mesmo contexto.

O repositório implementa [IDisposable](https://msdn.microsoft.com/library/system.idisposable.aspx) e descarta o contexto do banco de dados como vimos anteriormente no controlador, e seus métodos CRUD fazem chamadas ao contexto do banco de dados da mesma maneira que vimos anteriormente.

## <a name="change-the-student-controller-to-use-the-repository"></a>Alterar o controlador do aluno para usar o repositório

No *StudentController.cs*, substitua o código atualmente na classe pelo código a seguir. As alterações são realçadas.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=13-18,44,75,77,102-103,120,137-138,159,172-174,186)]

O controlador agora declara uma variável de classe para um objeto que implementa a `IStudentRepository` interface em vez da classe de contexto:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample5.cs)]

O construtor padrão (sem parâmetros) cria uma nova instância de contexto e um Construtor opcional permite que o chamador transmita uma instância de contexto.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample6.cs)]

(Se você estivesse usando *injeção de dependência*, ou di, não precisaria do construtor padrão porque o software di garantiria que o objeto de repositório correto sempre seria fornecido.)

Nos métodos CRUD, o repositório agora é chamado em vez do contexto:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample7.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample8.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample9.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample10.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample11.cs)]

E o `Dispose` método agora descarta o repositório em vez do contexto:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample12.cs)]

Execute o site e clique na guia **alunos** .

![Students_Index_page](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image1.png)

A página parece e funciona da mesma maneira que antes você alterou o código para usar o repositório, e as outras páginas de aluno também funcionam da mesma forma. No entanto, há uma diferença importante na maneira como o `Index` método do controlador faz a filtragem e a ordenação. A versão original desse método continha o seguinte código:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample13.cs?highlight=1)]

O `Index` método atualizado contém o seguinte código:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample14.cs?highlight=1)]

Somente o código realçado foi alterado.

Na versão original do código, `students` é digitado como um `IQueryable` objeto. A consulta não é enviada para o banco de dados até ser convertida em uma coleção usando um método como `ToList` , que não ocorre até que a exibição de índice acesse o modelo de aluno. O `Where` método no código original acima se torna uma `WHERE` cláusula na consulta SQL que é enviada ao banco de dados. Isso, por sua vez, significa que apenas as entidades selecionadas são retornadas pelo banco de dados. No entanto, como resultado da alteração `context.Students` para `studentRepository.GetStudents()` , a `students` variável após essa instrução é uma `IEnumerable` coleção que inclui todos os alunos no banco de dados. O resultado final da aplicação do `Where` método é o mesmo, mas agora o trabalho é feito na memória no servidor Web e não no banco de dados. Para consultas que retornam grandes volumes de dados, isso pode ser ineficiente.

> [!TIP]
> 
> **IQueryable versus IEnumerable**
> 
> Depois de implementar o repositório, conforme mostrado aqui, mesmo se você inserir algo na caixa de **pesquisa** , a consulta enviada para SQL Server retornará todas as linhas de aluno porque não inclui seus critérios de pesquisa:
> 
> ![](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image2.png)
> 
> [!code-sql[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample15.sql)]
> 
> Essa consulta retorna todos os dados do aluno porque o repositório executou a consulta sem saber mais sobre os critérios de pesquisa. O processo de classificação, aplicação de critérios de pesquisa e seleção de um subconjunto dos dados para paginação (mostrando apenas três linhas nesse caso) é feito na memória mais tarde, quando o `ToPagedList` método é chamado na `IEnumerable` coleção.
> 
> Na versão anterior do código (antes de você implementar o repositório), a consulta não é enviada ao banco de dados até que você aplique os critérios de pesquisa, quando `ToPagedList` é chamado no `IQueryable` objeto.
> 
> ![](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image3.png)
> 
> Quando ToPagedList é chamado em um `IQueryable` objeto, a consulta enviada para SQL Server especifica a cadeia de caracteres de pesquisa e, como resultado, somente as linhas que atendem aos critérios de pesquisa são retornadas e nenhuma filtragem precisa ser feita na memória.
> 
> [!code-sql[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample16.sql)]
> 
> (O tutorial a seguir explica como examinar as consultas enviadas para SQL Server.)

A seção a seguir mostra como implementar métodos de repositório que permitem especificar que esse trabalho deve ser feito pelo banco de dados.

Agora você criou uma camada de abstração entre o controlador e o contexto do banco de dados Entity Framework. Se você for executar testes de unidade automatizados com esse aplicativo, poderá criar uma classe de repositório alternativa em um projeto de teste de unidade que implementa `IStudentRepository` *.* Em vez de chamar o contexto para ler e gravar dados, essa classe de repositório fictício poderia manipular as coleções na memória para testar as funções do controlador.

## <a name="implement-a-generic-repository-and-a-unit-of-work-class"></a>Implementar um repositório genérico e uma unidade de classe de trabalho

A criação de uma classe de repositório para cada tipo de entidade pode resultar em muitos códigos redundantes e pode resultar em atualizações parciais. Por exemplo, suponha que você tenha que atualizar dois tipos de entidade diferentes como parte da mesma transação. Se cada uma delas usar uma instância de contexto de banco de dados separada, uma delas poderá ser bem-sucedida e a outra poderá falhar. Uma maneira de minimizar o código redundante é usar um repositório genérico e uma maneira de garantir que todos os repositórios usem o mesmo contexto de banco de dados (e, portanto, coordenar todas as atualizações) é usar uma classe de unidade de trabalho.

Nesta seção do tutorial, você criará uma `GenericRepository` classe e uma `UnitOfWork` classe e as usará no `Course` controlador para acessar o `Department` e os `Course` conjuntos de entidades. Conforme explicado anteriormente, para manter essa parte do tutorial simples, você não está criando interfaces para essas classes. Mas se você pretende usá-los para facilitar o TDD, normalmente os implementaria com interfaces da mesma maneira que fazia no `Student` repositório.

### <a name="create-a-generic-repository"></a>Criar um repositório genérico

Na pasta *Dal* , crie *GenericRepository.cs* e substitua o código existente pelo código a seguir:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample17.cs)]

As variáveis de classe são declaradas para o contexto do banco de dados e para o conjunto de entidades para o qual o repositório é instanciado:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample18.cs)]

O construtor aceita uma instância de contexto de banco de dados e inicializa a variável de conjunto de entidades:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample19.cs)]

O `Get` método usa expressões lambda para permitir que o código de chamada especifique uma condição de filtro e uma coluna para ordenar os resultados por, e um parâmetro de cadeia de caracteres permite que o chamador forneça uma lista delimitada por vírgulas de propriedades de navegação para carregamento adiantado:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample20.cs)]

O código `Expression<Func<TEntity, bool>> filter` significa que o chamador fornecerá uma expressão lambda com base no `TEntity` tipo, e essa expressão retornará um valor booliano. Por exemplo, se o repositório for instanciado para o `Student` tipo de entidade, o código no método de chamada poderá especificar `student => student.LastName == "Smith` &quot; para o `filter` parâmetro.

O código `Func<IQueryable<TEntity>, IOrderedQueryable<TEntity>> orderBy` também significa que o chamador fornecerá uma expressão lambda. Mas, nesse caso, a entrada para a expressão é um `IQueryable` objeto para o `TEntity` tipo. A expressão retornará uma versão ordenada do `IQueryable` objeto. Por exemplo, se o repositório for instanciado para o `Student` tipo de entidade, o código no método de chamada poderá especificar `q => q.OrderBy(s => s.LastName)` para o `orderBy` parâmetro.

O código no `Get` método cria um `IQueryable` objeto e, em seguida, aplica a expressão de filtro se houver um:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample21.cs)]

Em seguida, ele aplica as expressões de carregamento adiantado após a análise da lista delimitada por vírgulas:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample22.cs)]

Por fim, ele aplica a `orderBy` expressão se houver um e retorna os resultados; caso contrário, ele retorna os resultados da consulta não ordenada:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample23.cs)]

Ao chamar o `Get` método, você pode filtrar e classificar na `IEnumerable` coleção retornada pelo método, em vez de fornecer parâmetros para essas funções. Mas o trabalho de classificação e filtragem seria feito na memória no servidor Web. Usando esses parâmetros, você garante que o trabalho seja feito pelo banco de dados em vez do servidor Web. Uma alternativa é criar classes derivadas para tipos de entidade específicos e adicionar `Get` métodos especializados, como `GetStudentsInNameOrder` ou `GetStudentsByName` . No entanto, em um aplicativo complexo, isso pode resultar em um grande número de classes derivadas e de métodos especializados, o que pode ser mais trabalho a ser mantido.

O código nos `GetByID` métodos, `Insert` e `Update` é semelhante ao que você viu no repositório não genérico. (Você não está fornecendo um parâmetro de carregamento adiantado na `GetByID` assinatura, pois não pode fazer o carregamento rápido com o `Find` método.)

Duas sobrecargas são fornecidas para o `Delete` método:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample24.cs)]

Um deles permite que você passe apenas a ID da entidade a ser excluída e uma instância de entidade. Como vimos no tutorial sobre a [simultaneidade de manipulação](../../getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md) , para manipulação de simultaneidade, você precisa de um `Delete` método que usa uma instância de entidade que inclui o valor original de uma propriedade de rastreamento.

Esse repositório genérico tratará os requisitos CRUD típicos. Quando um determinado tipo de entidade tem requisitos especiais, como filtragem ou ordenação mais complexa, você pode criar uma classe derivada que tenha métodos adicionais para esse tipo.

## <a name="creating-the-unit-of-work-class"></a>Criando a classe de unidade de trabalho

A classe da unidade de trabalho atende a uma finalidade: para garantir que, quando você usar vários repositórios, eles compartilhem um único contexto de banco de dados. Dessa forma, quando uma unidade de trabalho for concluída, você poderá chamar o `SaveChanges` método nessa instância do contexto e ter certeza de que todas as alterações relacionadas serão coordenadas. Tudo o que a classe precisa é de um `Save` método e uma propriedade para cada repositório. Cada propriedade Repository retorna uma instância de repositório que foi instanciada usando a mesma instância de contexto de banco de dados que as outras instâncias de repositório.

Na pasta *Dal* , crie um arquivo de classe chamado *UnitOfWork.cs* e substitua o código do modelo pelo código a seguir:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample25.cs)]

O código cria variáveis de classe para o contexto do banco de dados e para cada repositório. Para a `context` variável, um novo contexto é instanciado:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample26.cs)]

Cada propriedade de repositório verifica se o repositório já existe. Caso contrário, ele instancia o repositório, passando a instância de contexto. Como resultado, todos os repositórios compartilham a mesma instância de contexto.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample27.cs)]

O `Save` método chama `SaveChanges` no contexto do banco de dados.

Como qualquer classe que instancia um contexto de banco de dados em uma variável de classe, a `UnitOfWork` classe implementa `IDisposable` e descarta o contexto.

### <a name="changing-the-course-controller-to-use-the-unitofwork-class-and-repositories"></a>Alterando o controlador do curso para usar a classe e os repositórios UnitOfWork

Substitua o código que você tem atualmente em *CourseController.cs* com o seguinte código:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample28.cs?highlight=15,20,22,31,54-55,70,85-86,101-102,122-124,130)]

Esse código adiciona uma variável de classe para a `UnitOfWork` classe. (Se você estivesse usando interfaces aqui, não inicializaria a variável aqui; em vez disso, você implementaria um padrão de dois construtores da mesma forma que fazia para o `Student` repositório.)

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample29.cs)]

No restante da classe, todas as referências ao contexto do banco de dados são substituídas por referências ao repositório apropriado, usando `UnitOfWork` Propriedades para acessar o repositório. O `Dispose` método descarta a `UnitOfWork` instância.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample30.cs)]

Execute o site e clique na guia **cursos** .

![Courses_Index_page](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image4.png)

A página parece e funciona da mesma maneira que antes das alterações, e as outras páginas do curso também funcionam da mesma forma.

## <a name="summary"></a>Resumo

Agora você implementou o repositório e os padrões de unidade de trabalho. Você usou expressões lambda como parâmetros de método no repositório genérico. Para obter mais informações sobre como usar essas expressões com um `IQueryable` objeto, consulte a [interface IQueryable (T) (System. Linq)](https://msdn.microsoft.com/library/bb351562.aspx) na biblioteca MSDN. No próximo tutorial, você aprenderá a lidar com alguns cenários avançados.

Links para outros recursos de Entity Framework podem ser encontrados no [mapa de conteúdo de acesso a dados do ASP.net](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Anterior](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md) 
>  [Avançar](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
