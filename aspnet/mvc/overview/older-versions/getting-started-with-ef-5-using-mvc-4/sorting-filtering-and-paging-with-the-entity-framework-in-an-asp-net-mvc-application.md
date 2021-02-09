---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
title: Classificação, filtragem e paginação com o Entity Framework em um aplicativo MVC ASP.NET (3 de 10) | Microsoft Docs
author: tdykstra
description: O aplicativo Web de exemplo da Contoso University demonstra como criar aplicativos ASP.NET MVC 4 usando o Entity Framework 5 Code First e o Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 8af630e0-fffa-4110-9eca-c96e201b2724
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: c387223ec99e50fc8505e59697093b5bb482c58b
ms.sourcegitcommit: b4cdcf246850751579e45da80c9780fe56330dd0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/09/2021
ms.locfileid: "99985629"
---
# <a name="sorting-filtering-and-paging-with-the-entity-framework-in-an-aspnet-mvc-application-3-of-10"></a>Classificação, filtragem e paginação com o Entity Framework em um aplicativo MVC ASP.NET (3 de 10)

por [Tom Dykstra](https://github.com/tdykstra)

> O aplicativo Web de exemplo da Contoso University demonstra como criar aplicativos ASP.NET MVC 4 usando o Entity Framework 5 Code First e o Visual Studio 2012. Para obter informações sobre a série de tutoriais, consulte [o primeiro tutorial da série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
> 
> > [!NOTE] 
> > 
> > Se você encontrar um problema que não possa resolver, [Baixe o capítulo concluído](building-the-ef5-mvc4-chapter-downloads.md) e tente reproduzir o problema. Em geral, você pode encontrar a solução para o problema comparando seu código com o código concluído. Para alguns erros comuns e como resolvê-los, consulte [erros e soluções alternativas.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)

No tutorial anterior, você implementou um conjunto de páginas da Web para operações CRUD básicas para `Student` entidades. Neste tutorial, você adicionará a funcionalidade de classificação, filtragem e paginação à página de índice dos **alunos** . Você também criará uma página que faz um agrupamento simples.

A ilustração a seguir mostra a aparência da página quando você terminar. Os títulos de coluna são links que o usuário pode clicar para classificar por essa coluna. Clicar em um título de coluna alterna repetidamente entre a ordem de classificação ascendente e descendente.

![Students_Index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a>Adicionar links de classificação de coluna à página Índice de Alunos

Para adicionar a classificação à página de índice de estudante, você alterará o `Index` método do `Student` controlador e adicionará o código à `Student` exibição de índice.

### <a name="add-sorting-functionality-to-the-index-method"></a>Adicionar funcionalidade de classificação ao método de índice

No *Controllers\StudentController.cs*, substitua o `Index` método pelo código a seguir:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

Esse código recebe um parâmetro `sortOrder` da cadeia de caracteres de consulta na URL. O valor da cadeia de caracteres de consulta é fornecido pelo ASP.NET MVC como um parâmetro para o método de ação. O parâmetro será uma cadeia de caracteres "Name" ou "Date", opcionalmente, seguido de um sublinhado e a cadeia de caracteres "desc" para especificar a ordem descendente. A ordem de classificação padrão é crescente.

Na primeira vez que a página Índice é solicitada, não há nenhuma cadeia de caracteres de consulta. Os alunos são exibidos em ordem crescente por `LastName` , que é o padrão, conforme estabelecido pelo caso de outono na `switch` instrução. Quando o usuário clica em um hiperlink de título de coluna, o valor `sortOrder` apropriado é fornecido na cadeia de caracteres de consulta.

As duas `ViewBag` variáveis são usadas para que a exibição possa configurar os hiperlinks de título de coluna com os valores de cadeia de caracteres de consulta apropriados:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Essas são instruções ternárias. O primeiro especifica que, se o `sortOrder` parâmetro for nulo ou vazio, `ViewBag.NameSortParm` deve ser definido como "nome \_ desc"; caso contrário, ele deve ser definido como uma cadeia de caracteres vazia. Essas duas instruções permitem que a exibição defina os hiperlinks de título de coluna da seguinte maneira:

| Ordem de classificação atual | Hiperlink do sobrenome | Hiperlink de data |
| --- | --- | --- |
| Sobrenome ascendente | descending | ascending |
| Sobrenome descendente | ascending | ascending |
| Data ascendente | ascending | descending |
| Data descendente | ascending | ascending |

O método usa [LINQ to Entities](https://msdn.microsoft.com/library/bb386964.aspx) para especificar a coluna pela qual classificar. O código cria uma variável [IQueryable](https://msdn.microsoft.com/library/bb351562.aspx) antes da `switch` instrução, modifica-a na `switch` instrução e chama o `ToList` método após a `switch` instrução. Quando você cria e modifica variáveis `IQueryable`, nenhuma consulta é enviada para o banco de dados. A consulta não é executada até que você converta o `IQueryable` objeto em uma coleção chamando um método como `ToList` . Portanto, esse código resulta em uma única consulta que não é executada até a `return View` instrução.

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a>Adicionar hiperlinks de título de coluna à exibição de índice de estudante

No *Views\Student\Index.cshtml*, substitua os `<tr>` `<th>` elementos e da linha de cabeçalho pelo código realçado:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=5-15)]

Esse código usa as informações nas `ViewBag` Propriedades para configurar hiperlinks com os valores de cadeia de caracteres de consulta apropriados.

Execute a página e clique nos títulos de coluna de data do **último nome** e do **registro** para verificar se a classificação funciona.

![Students_Index_page_with_sort_hyperlinks](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

Depois de clicar no título do **sobrenome** , os alunos serão exibidos em ordem decrescente do último nome.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="add-a-search-box-to-the-students-index-page"></a>Adicionar uma caixa de pesquisa à página de índice estudantes

Para adicionar a filtragem à página Índice de Alunos, você adicionará uma caixa de texto e um botão Enviar à exibição e fará alterações correspondentes no método `Index`. A caixa de texto permitirá que você insira uma cadeia de caracteres a ser pesquisada nos campos de nome e sobrenome.

### <a name="add-filtering-functionality-to-the-index-method"></a>Adicionar funcionalidade de filtragem ao método de índice

No *Controllers\StudentController.cs*, substitua o `Index` método pelo código a seguir (as alterações são realçadas):

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=1,7-11)]

Você adicionou um parâmetro `searchString` ao método `Index`. Você também adicionou à instrução LINQ uma `where` cláusula que seleciona somente alunos cujo nome ou sobrenome contêm a cadeia de caracteres de pesquisa. O valor da cadeia de caracteres de pesquisa é recebido de uma caixa de texto que você adicionará à exibição de índice. A instrução que adiciona a cláusula [Where](https://msdn.microsoft.com/library/bb535040.aspx) é executada somente se houver um valor a ser procurado.

> [!NOTE]
> Em muitos casos, você pode chamar o mesmo método em um conjunto de entidades Entity Framework ou como um método de extensão em uma coleção na memória. Os resultados são normalmente os mesmos, mas em alguns casos podem ser diferentes. Por exemplo, a implementação de .NET Framework do `Contains` método retorna todas as linhas quando você passa uma cadeia de caracteres vazia para ela, mas o provedor de Entity Framework para SQL Server Compact 4,0 retorna zero linhas para cadeias de caracteres vazias. Portanto, o código no exemplo (colocando a `Where` instrução dentro de uma `if` instrução) garante que você obtenha os mesmos resultados para todas as versões do SQL Server. Além disso, a implementação de .NET Framework do `Contains` método executa uma comparação que diferencia maiúsculas de minúsculas por padrão, mas Entity Framework provedores de SQL Server executam comparações que não diferenciam maiúsculas de minúsculas por padrão. Portanto, chamar o `ToUpper` método para fazer o teste explicitamente não diferencia maiúsculas de minúsculas garante que os resultados não sejam alterados quando você altera o código posteriormente para usar um repositório, o que retornará uma `IEnumerable` coleção em vez de um `IQueryable` objeto. (Quando você chama o método `Contains` em uma coleção `IEnumerable`, obtém a implementação do .NET Framework; quando chama-o em um objeto `IQueryable`, obtém a implementação do provedor de banco de dados.)

### <a name="add-a-search-box-to-the-student-index-view"></a>Adicionar uma Caixa de Pesquisa à exibição Índice de Alunos

No *Views\Student\Index.cshtml*, adicione o código realçado imediatamente antes da marca de abertura `table` para criar uma legenda, uma caixa de texto e um botão de **pesquisa** .

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=5-10)]

Execute a página, insira uma cadeia de caracteres de pesquisa e clique em **Pesquisar** para verificar se a filtragem está funcionando.

![Students_Index_page_with_search_box](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Observe que a URL não contém a cadeia de caracteres de pesquisa "a", o que significa que, se você marcar essa página, você não obterá a lista filtrada ao usar o indicador. Você alterará o botão de **pesquisa** para usar cadeias de caracteres de consulta para critérios de filtro posteriormente no tutorial.

## <a name="add-paging-to-the-students-index-page"></a>Adicionar paginação à página de índice estudantes

Para adicionar a paginação à página de índice estudantes, você começará instalando o pacote NuGet **PagedList. Mvc** . Em seguida, você fará alterações adicionais no `Index` método e adicionará links de paginação à `Index` exibição. O **PagedList. Mvc** é um dos muitos pacotes bons de paginação e classificação para o ASP.NET MVC, e seu uso é destinado apenas como exemplo, não como uma recomendação para ele em relação a outras opções. A ilustração a seguir mostra os links de paginação.

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

### <a name="install-the-pagedlistmvc-nuget-package"></a>Instalar o pacote NuGet PagedList. MVC

O pacote NuGet **PagedList. Mvc** instala automaticamente o pacote **PagedList** como uma dependência. O pacote **PagedList** instala um `PagedList` tipo de coleção e métodos de extensão para `IQueryable` `IEnumerable` coleções e. Os métodos de extensão criam uma única página de dados em uma `PagedList` coleção fora de seu `IQueryable` ou `IEnumerable` e a `PagedList` coleção fornece várias propriedades e métodos que facilitam a paginação. O pacote **PagedList. Mvc** instala um auxiliar de paginação que exibe os botões de paginação.

No menu **ferramentas** , selecione **Gerenciador de pacotes NuGet** e, em seguida, **gerenciar pacotes NuGet para solução**.

Na caixa de diálogo **gerenciar pacotes NuGet** , clique na guia **online** à esquerda e, em seguida, digite "Paged" na caixa de pesquisa. Quando você vir o pacote **PagedList. Mvc** , clique em **instalar**.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Na caixa **selecionar projetos** , clique em **OK**.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

### <a name="add-paging-functionality-to-the-index-method"></a>Adicionar funcionalidade de paginação ao método de índice

Em *Controllers\StudentController.cs*, adicione uma `using` instrução para o `PagedList` namespace:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

Substitua o método `Index` pelo seguinte código:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Esse código adiciona um `page` parâmetro, um parâmetro de ordem de classificação atual e um parâmetro de filtro atual à assinatura do método, como mostrado aqui:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Na primeira vez que a página for exibida, ou se o usuário ainda não tiver clicado em um link de paginação ou classificação, todos os parâmetros serão nulos. Se um link de paginação for clicado, a variável conterá `page` o número de página a ser exibido.

`A ViewBag` a propriedade fornece a exibição com a ordem de classificação atual, pois ela deve ser incluída nos links de paginação para manter a mesma ordem de classificação durante a paginação:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Outra propriedade, `ViewBag.CurrentFilter` , fornece a exibição com a cadeia de caracteres de filtro atual. Esse valor precisa ser incluído nos links de paginação para manter as configurações de filtro durante a paginação e precisa ser restaurado para a caixa de texto quando a página é exibida novamente. Se a cadeia de caracteres de pesquisa for alterada durante a paginação, a página precisará ser redefinida como 1, porque o novo filtro pode resultar na exibição de dados diferentes. A cadeia de caracteres de pesquisa é alterada quando um valor é inserido na caixa de texto e o botão enviar é pressionado. Nesse caso, o `searchString` parâmetro não é nulo.

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

No final do método, o método de `ToPagedList` extensão no objeto dos alunos `IQueryable` converte a consulta de aluno em uma única página de alunos em um tipo de coleção que dá suporte à paginação. Essa única página de alunos é passada para a exibição:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

O método `ToPagedList` usa um número de página. Os dois pontos de interrogação representam o [operador de União nula](https://msdn.microsoft.com/library/ms173224.aspx). O operador de união de nulo define um valor padrão para um tipo que permite valor nulo; a expressão `(page ?? 1)` significa retornar o valor de `page` se ele tiver um valor ou retornar 1 se `page` for nulo.

### <a name="add-paging-links-to-the-student-index-view"></a>Adicionar links de paginação à exibição de índice de estudante

No *Views\Student\Index.cshtml*, substitua o código existente pelo código a seguir:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=6,9,14-20,56-58)]

A instrução `@model` na parte superior da página especifica que a exibição agora obtém um objeto `PagedList`, em vez de um objeto `List`.

A `using` instrução para `PagedList.Mvc` fornece acesso ao auxiliar do MVC para os botões de paginação.

O código usa uma sobrecarga de [BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) que permite especificar [FormMethod. Get](https://msdn.microsoft.com/library/system.web.mvc.formmethod(v=vs.100).aspx/css).

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cshtml?highlight=1)]

O [BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) padrão envia dados de formulário com uma postagem, o que significa que os parâmetros são passados no corpo da mensagem http e não na URL como cadeias de consulta. Quando você especifica HTTP GET, os dados de formulário são passados na URL como cadeias de consulta, o que permite aos usuários marcar a URL. As [diretrizes do W3C para o uso de http Get](http://www.w3.org/2001/tag/doc/whenToUseGet.html) especificam que você deve usar Get quando a ação não resultar em uma atualização.

A caixa de texto é inicializada com a cadeia de caracteres de pesquisa atual para que, ao clicar em uma nova página, você possa ver a cadeia de caracteres de pesquisa atual.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml?highlight=1)]

Os links de cabeçalho de coluna usam a cadeia de caracteres de consulta para passar a cadeia de caracteres de pesquisa atual para o controlador, de modo que o usuário possa classificar nos resultados do filtro:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1)]

A página atual e o número total de páginas são exibidos.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]

Se não houver nenhuma página a ser exibida, "página 0 de 0" será mostrada. (Nesse caso, o número da página é maior que a contagem de páginas porque `Model.PageNumber` é 1 e `Model.PageCount` é 0.)

Os botões de paginação são exibidos pelo `PagedListPager` auxiliar:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

O `PagedListPager` auxiliar fornece várias opções que você pode personalizar, incluindo URLs e estilos. Para obter mais informações, consulte [TroyGoode/PagedList](https://github.com/TroyGoode/PagedList) no site do github.

Execute a página.

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Clique nos links de paginação em ordens de classificação diferentes para verificar se a paginação funciona. Em seguida, insira uma cadeia de caracteres de pesquisa e tente fazer a paginação novamente para verificar se ela também funciona corretamente com a classificação e filtragem.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

## <a name="create-an-about-page-that-shows-student-statistics"></a>Criar uma página sobre que mostre estatísticas de alunos

Para a página Sobre do site da Contoso University, você exibirá quantos alunos se registraram para cada data de registro. Isso exige agrupamento e cálculos simples nos grupos. Para fazer isso, você fará o seguinte:

- Criar uma classe de modelo de exibição para os dados que você precisa passar para a exibição.
- Modifique o `About` método no `Home` controlador.
- Modifique a `About` exibição.

### <a name="create-the-view-model"></a>Criar o modelo de exibição

Crie uma pasta *ViewModels* . Nessa pasta, adicione um arquivo de classe *EnrollmentDateGroup.cs* e substitua o código existente pelo código a seguir:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

### <a name="modify-the-home-controller"></a>Modificar o controlador Home

No *HomeController.cs*, adicione as seguintes `using` instruções na parte superior do arquivo:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

Adicione uma variável de classe para o contexto do banco de dados imediatamente após a chave de abertura para a classe:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs?highlight=3)]

Substitua o método `About` pelo seguinte código:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs)]

A instrução LINQ agrupa as entidades de alunos por data de registro, calcula o número de entidades em cada grupo e armazena os resultados em uma coleção de objetos de modelo de exibição `EnrollmentDateGroup`.

Adicione um `Dispose` método:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

### <a name="modify-the-about-view"></a>Modificar a exibição Sobre

Substitua o código no arquivo *Views\Home\About.cshtml* pelo código a seguir:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml)]

Execute o aplicativo e clique no link **sobre** . A contagem de alunos para cada data de registro é exibida em uma tabela.

![About_page](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

## <a name="optional-deploy-the-app-to-windows-azure"></a>Opcional: implantar o aplicativo no Windows Azure

Até agora, seu aplicativo está em execução localmente em IIS Express no seu computador de desenvolvimento. Para torná-lo disponível para outras pessoas usarem pela Internet, você precisa implantá-lo em um provedor de hospedagem na Web. Nesta seção opcional do tutorial, você o implantará em um site do Windows Azure.

### <a name="using-code-first-migrations-to-deploy-the-database"></a>Usando Migrações do Code First para implantar o banco de dados

Para implantar o banco de dados, você usará Migrações do Code First. Ao criar o perfil de publicação que você usa para definir as configurações de implantação do Visual Studio, você marcará uma caixa de seleção que é rotulada como **executar migrações do Code First (é executado no início do aplicativo)**. Essa configuração faz com que o processo de implantação configure automaticamente o arquivo de *Web.config* do aplicativo no servidor de destino para que Code First use a `MigrateDatabaseToLatestVersion` classe do inicializador.

O Visual Studio não faz nada com o banco de dados durante o processo de implantação. Quando o aplicativo implantado acessa o banco de dados pela primeira vez após a implantação, Code First cria automaticamente o banco de dados ou atualiza o esquema de banco de dados para a versão mais recente. Se o aplicativo implementar um método de migrações `Seed` , o método será executado depois que o banco de dados for criado ou o esquema for atualizado.

O método Migrations `Seed` insere dados de teste. Se você estivesse implantando em um ambiente de produção, precisaria alterar o `Seed` método para que ele apenas insira dados que você deseja inserir em seu banco de dados de produção. Por exemplo, em seu modelo de dados atual, talvez você queira ter cursos reais, mas os alunos fictícios no banco de dado de desenvolvimento. Você pode escrever um `Seed` método para carregar ambos no desenvolvimento e, em seguida, comentar os alunos fictícios antes de implantá-los na produção. Ou você pode escrever um `Seed` método para carregar apenas cursos e inserir os alunos fictícios no banco de dados de teste manualmente usando a interface do usuário do aplicativo.

### <a name="get-a-windows-azure-account"></a>Obter uma conta do Windows Azure

Você precisará de uma conta do Windows Azure. Se você ainda não tiver uma, poderá criar uma conta de avaliação gratuita em apenas alguns minutos. Para obter detalhes, consulte [avaliação gratuita do Windows Azure](https://azure.microsoft.com/free/dotnet/).

### <a name="create-a-web-site-and-a-sql-database-in-windows-azure"></a>Criar um site da Web e um banco de dados SQL no Windows Azure

Seu site do Windows Azure será executado em um ambiente de hospedagem compartilhado, o que significa que ele é executado em máquinas virtuais (VMs) que são compartilhadas com outros clientes do Windows Azure. Um ambiente de hospedagem compartilhado é uma maneira de começar na nuvem a um baixo custo. Mais tarde, se seu tráfego da Web aumentar, o aplicativo poderá ser dimensionado para atender à necessidade executando em VMs dedicadas. Se precisar de uma arquitetura mais complexa, você poderá migrar para um serviço de nuvem do Windows Azure. Os serviços de nuvem são executados em VMs dedicadas que podem ser configuradas de acordo com suas necessidades.

O banco de dados SQL do Windows Azure é um serviço de banco de dados relacional baseado em nuvem criado em tecnologias de SQL Server. Ferramentas e aplicativos que funcionam com o SQL Server também funcionam com o banco de dados SQL.

1. Na [portal de gerenciamento do Windows Azure](https://manage.windowsazure.com/), clique em **sites** na guia à esquerda e, em seguida, clique em **novo**.

    ![Novo botão no Portal de Gerenciamento](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)
2. Clique em **criação personalizada**.

    ![Criar com link de banco de dados no Portal de Gerenciamento](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

   O **novo site da Web-assistente de criação personalizada** é aberto.
3. Na etapa **novo site** do assistente, insira uma cadeia de caracteres na caixa **URL** para usar como a URL exclusiva para seu aplicativo. A URL completa consistirá no que você digitar aqui mais o sufixo que você vê ao lado da caixa de texto. A ilustração mostra "ConU", mas essa URL é provavelmente usada, portanto, você precisará escolher uma diferente.

    ![Criar com link de banco de dados no Portal de Gerenciamento](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image13.png)
4. Na lista suspensa **região** , escolha uma região perto de você. Essa configuração especifica em qual data center seu site será executado.
5. Na lista suspensa **banco de dados** , escolha **criar um banco de dados SQL gratuito de 20 MB**.

    ![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image14.png)
6. No **nome da cadeia de conexão do BD**, digite *SchoolContext*.

    ![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)
7. Clique na seta que aponta para a direita na parte inferior da caixa. O assistente avança para a etapa **configurações de banco de dados** .
8. Na caixa **nome** , digite *ContosoUniversityDB*.
9. Na caixa **servidor** , selecione **novo servidor de banco de dados SQL**. Como alternativa, se você tiver criado anteriormente um servidor, poderá selecionar esse servidor na lista suspensa.
10. Insira um **nome de logon** e uma **senha** de administrador. Se selecionou **Novo servidor do Banco de Dados SQL** , você não irá digitar um nome e uma senha existentes aqui; digite um novo nome e uma senha que você está definindo agora para usar mais tarde ao acessar o banco de dados. Se você selecionou um servidor criado anteriormente, você digitará as credenciais para esse servidor. Para este tutorial, você não selecionará a ***caixa de seleção avançado. As opções _*_avançado_** permitem que você defina o [agrupamento](https://msdn.microsoft.com/library/aa174903(v=SQL.80).aspx)de banco de dados.
11. Escolha a mesma **região** que você escolheu para o site.
12. Clique na marca de seleção na parte inferior direita da caixa para indicar que você terminou.   
  
    ![Configurações do banco de dados etapa do novo site-criar com o assistente de banco de dados](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image16.png)  

    A imagem a seguir mostra o uso de um SQL Server e um logon existentes.   
  
    ![Configurações do banco de dados etapa do novo site-criar com o assistente de banco de dados](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image17.png)  
  
    O Portal de Gerenciamento retorna à página de sites e a coluna **status** mostra que o site está sendo criado. Após um tempo (normalmente menos de um minuto), a coluna **status** mostra que o site foi criado com êxito. Na barra de navegação à esquerda, o número de sites que você tem em sua conta aparece ao lado do ícone **sites** e o número de bancos de dados é exibido ao lado do ícone **bancos de dados SQL** .

## <a name="deploy-the-application-to-windows-azure"></a>Implantar o aplicativo no Windows Azure

1. No Visual Studio, clique com o botão direito do mouse no projeto no **Gerenciador de Soluções** e selecione **Publicar** no menu de contexto.  
  
    ![Publicar no menu de contexto do projeto](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image18.png)
2. Na guia **perfil** do assistente **publicar Web** , clique em **importar**.  
  
    ![Importar configurações de publicação](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image19.png)
3. Se você não adicionou anteriormente sua assinatura do Windows Azure no Visual Studio, execute as etapas a seguir. Nestas etapas, você adiciona sua assinatura para que a lista suspensa em **importar de um site do Windows Azure** inclua seu site da Web.

    a. Na caixa de diálogo **importar perfil de publicação** , clique em **importar de um site do Windows Azure** e, em seguida, clique em **Adicionar assinatura do Windows Azure**.

    ![Adicionar assinatura do Windows Azure](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image20.png)

    b. Na caixa de diálogo **importar assinaturas do Windows Azure** , clique em **baixar arquivo de assinatura**.

    ![baixar arquivo de assinatura](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image21.png)

    c. Na janela do navegador, salve o arquivo *. publishsettings* .

    ![baixar arquivo. publishsettings](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image22.png)

    > [!WARNING]
    > Segurança-o arquivo *publishsettings* contém suas credenciais (sem codificação) que são usadas para administrar suas assinaturas e serviços do Windows Azure. A prática recomendada de segurança para esse arquivo é armazená-lo temporariamente fora dos diretórios de origem (por exemplo, na pasta *Libraries\Documents* ) e, em seguida, excluí-lo após a conclusão da importação. Um usuário mal-intencionado que obtém acesso ao `.publishsettings` arquivo pode editar, criar e excluir seus serviços do Windows Azure.

    d. Na caixa de diálogo **importar assinaturas do Windows Azure** , clique em **procurar** e navegue até o arquivo *. publishsettings* .

    ![baixar sub](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image23.png)

    e. Clique em **Importar**.

    ![import](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image24.png)
4. Na caixa de diálogo **importar perfil de publicação** , selecione **importar de um site do Windows Azure**, selecione seu site na lista suspensa e clique em **OK**.  
  
    ![Importar perfil de publicação](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image25.png)
5. Na guia **conexão** , clique em **validar conexão** para certificar-se de que as configurações estão corretas.  
  
    ![Validar conexão](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image26.png)
6. Quando a conexão for validada, uma marca de seleção verde será mostrada ao lado do botão **validar conexão** . Clique em **Próximo**.  
  
    ![Conexão validada com êxito](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image27.png)
7. Abra a lista suspensa **cadeia de conexão remota** em **SchoolContext** e selecione a cadeia de conexão para o banco de dados que você criou.
8. Selecione **executar migrações do Code First (é executado no início do aplicativo)**.
9. Desmarque **usar esta cadeia de conexão em tempo de execução** para o **userContext (DefaultConnection)**, pois esse aplicativo não está usando o banco de dados de associação.   
  
    ![Guia Configurações](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image28.png)
10. Clique em **Próximo**.
11. Na guia **Visualização** , clique em **Iniciar visualização**.  
  
    ![Botão StartPreview na guia Visualização](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image29.png)  
  
    A guia exibe uma lista dos arquivos que serão copiados para o servidor. A exibição da visualização não é necessária para publicar o aplicativo, mas é uma função útil a ser observada. Nesse caso, você não precisa fazer nada com a lista de arquivos exibida. Na próxima vez que você implantar esse aplicativo, somente os arquivos que foram alterados estarão nessa lista.  
  
    ![Saída do arquivo StartPreview](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image30.png)
12. Clique em **Publicar**.  
    O Visual Studio inicia o processo de copiar os arquivos para o servidor do Windows Azure.
13. A janela **Saída** mostra quais ações de implantação foram executadas e os relatórios da conclusão com êxito da implantação.  
  
    ![Janela de saída relatando a implantação bem-sucedida](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image31.png)
14. Após a implantação bem-sucedida, o navegador padrão será aberto automaticamente na URL do site implantado.  
    O aplicativo que você criou agora está em execução na nuvem. Clique na guia alunos.  
  
    ![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image32.png)

Neste ponto, seu banco de dados *SchoolContext* foi criado no banco de dados SQL do Windows Azure porque você selecionou **executar migrações do Code First (é executado no início do aplicativo)**. O arquivo de *Web.config* no site implantado foi alterado para que o inicializador [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) fosse executado na primeira vez em que seu código lê ou grava dados no banco de dado (o que aconteceu quando você selecionou a guia **estudantes** ):

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image33.png)

O processo de implantação também criou uma nova cadeia de conexão *(SchoolContext \_ DatabasePublish*) para migrações do Code First usar para atualizar o esquema de banco de dados e propagar o banco de dados.

![Database_Publish cadeia de conexão](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image34.png)

A cadeia de conexão *DefaultConnection* é para o banco de dados Membership (que não estamos usando neste tutorial). A cadeia de conexão *SchoolContext* é para o banco de dados ContosoUniversity.

Você pode encontrar a versão implantada do arquivo de Web.config em seu próprio computador no *ContosoUniversity\obj\Release\Package\PackageTmp\Web.config*. Você pode acessar o próprio arquivo de *Web.config* implantado usando FTP. Para obter instruções, consulte [implantação da Web do ASP.NET usando o Visual Studio: Implantando uma atualização de código](../../../../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md). Siga as instruções que começam com "para usar uma ferramenta de FTP, você precisa de três coisas: a URL de FTP, o nome de usuário e a senha".

> [!NOTE]
> O aplicativo Web não implementa a segurança, para que qualquer pessoa que encontre a URL possa alterar os dados. Para obter instruções sobre como proteger o site da Web, consulte [implantar um aplicativo MVC do secure ASP.NET com associação, OAuth e banco de dados SQL em um site do Windows Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Você pode impedir que outras pessoas usem o site usando o Portal de Gerenciamento do Windows Azure ou **Gerenciador de servidores** no Visual Studio para interromper o site.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image35.png)

## <a name="code-first-initializers"></a>Inicializadores de Code First

Na seção de implantação, você viu o inicializador [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) que está sendo usado. O Code First também fornece outros inicializadores que você pode usar, incluindo [CreateDatabaseIfNotExists](https://msdn.microsoft.com/library/gg679221(v=vs.103).aspx) (o padrão), [DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=VS.103).aspx) e [DropCreateDatabaseAlways](https://msdn.microsoft.com/library/gg679506(v=VS.103).aspx). O `DropCreateAlways` inicializador pode ser útil para configurar condições para testes de unidade. Você também pode escrever seus próprios inicializadores e pode chamar um inicializador explicitamente se não quiser esperar até que o aplicativo leia ou grave no banco de dados. Para obter uma explicação abrangente dos inicializadores, consulte o capítulo 6 da [programação de livros Entity Framework: Code First](http://shop.oreilly.com/product/0636920022220.do) por Julie Lerman e Rowan Miller.

## <a name="summary"></a>Resumo

Neste tutorial, você viu como criar um modelo de dados e implementar a funcionalidade básica de CRUD, classificação, filtragem, paginação e agrupamento. No próximo tutorial, você começará a examinar os tópicos mais avançados expandindo o modelo de dados.

Links para outros recursos de Entity Framework podem ser encontrados no [mapa de conteúdo de acesso a dados do ASP.net](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Anterior](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md) 
>  [Avançar](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
