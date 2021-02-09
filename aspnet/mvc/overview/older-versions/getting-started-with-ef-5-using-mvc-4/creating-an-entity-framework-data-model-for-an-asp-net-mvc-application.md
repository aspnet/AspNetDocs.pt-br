---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
title: Criando um modelo de dados de Entity Framework para um aplicativo MVC ASP.NET (1 de 10) | Microsoft Docs
author: tdykstra
description: Uma versão mais recente desta série de tutoriais está disponível, por Visual Studio 2013, Entity Framework 6 e MVC 5. O aplicativo Web de exemplo da Contoso University de...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 4ba029b6-ee7c-4e45-a0e7-b703c37e5d9a
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 9d4eddd09410d102045271c2e4662e0b4177514c
ms.sourcegitcommit: b4cdcf246850751579e45da80c9780fe56330dd0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/09/2021
ms.locfileid: "99985156"
---
# <a name="creating-an-entity-framework-data-model-for-an-aspnet-mvc-application-1-of-10"></a>Criando um modelo de dados de Entity Framework para um aplicativo MVC ASP.NET (1 de 10)

por [Tom Dykstra](https://github.com/tdykstra)

> > [!NOTE] 
> > 
> > Uma [versão mais recente desta série de tutoriais](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) está disponível, por Visual Studio 2013, Entity Framework 6 e MVC 5.
> 
> 
> O aplicativo Web de exemplo Contoso University demonstra como criar aplicativos ASP.NET MVC 4 usando o Entity Framework 5 e o Visual Studio 2012. O aplicativo de exemplo é um site de uma Contoso University fictícia. Ele inclui funcionalidades como admissão de alunos, criação de cursos e atribuições de instrutor. Esta série de tutoriais explica como criar o aplicativo de exemplo Contoso University.
> 
> ## <a name="code-first"></a>Code First
> 
> Há três maneiras de trabalhar com dados no Entity Framework: *Database First*, *Model First* e *Code First*. Este tutorial é para Code First. Para obter informações sobre as diferenças entre esses fluxos de trabalho e diretrizes sobre como escolher o melhor para seu cenário, consulte [Entity Framework fluxos de trabalho de desenvolvimento](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).
> 
> ## <a name="mvc"></a>MVC
> 
> O aplicativo de exemplo se baseia no [ASP.NET MVC](../../../index.md). Se você preferir trabalhar com o modelo de Web Forms ASP.NET, consulte a [Associação de modelo e Web Forms](../../../../web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data.md) a série de tutoriais e o [mapa de conteúdo de acesso a dados do ASP.net](../../../../whitepapers/aspnet-data-access-content-map.md).
> 
> ## <a name="software-versions"></a>Versões de software
> 
> | **Mostrado no tutorial** | **Também funciona com** |
> | --- | --- |
> | Windows 8 | Windows 7 |
> | Visual Studio 2012 | Visual Studio 2012 Express para Web. Isso será instalado automaticamente pelo SDK do Windows Azure se você ainda não tiver o VS 2012 ou o VS 2012 Express para Web. Visual Studio 2013 deve funcionar, mas o tutorial não foi testado com ele, e algumas seleções de menu e caixas de diálogo são diferentes. A [versão VS 2013 do SDK do Windows Azure](https://go.microsoft.com/fwlink/p/?linkid=323510) é necessária para a implantação do Windows Azure. |
> | .NET 4.5 | A maioria dos recursos mostrados funcionará no .NET 4, mas alguns não terão. Por exemplo, o suporte a enum no EF requer o .NET 4,5. |
> | Entity Framework 5 |  |
> | [SDK do Windows Azure 2,1](https://go.microsoft.com/fwlink/p/?linkid=323511) | Se você ignorar as etapas de implantação do Windows Azure, não precisará do SDK. Quando uma nova versão do SDK for lançada, o link instalará a versão mais recente. Nesse caso, talvez seja necessário adaptar algumas das instruções para a nova interface do usuário e recursos. |
> 
> ## <a name="questions"></a>Perguntas
> 
> Se você tiver dúvidas que não estão diretamente relacionadas ao tutorial, poderá lançá-las no fórum de [Entity Framework ASP.net](https://forums.asp.net/1227.aspx), no [Entity Framework e no LINQ to Entities forum](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/)ou [stackoverflow.com](http://stackoverflow.com/).
> 
> ## <a name="acknowledgments"></a>Agradecimentos
> 
> Consulte o último tutorial da série para [confirmações e uma observação sobre o VB](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#acknowledgments).
> 
> ## <a name="original-version-of-the-tutorial"></a>Versão original do tutorial
> 
> A versão original do tutorial está disponível no [livro eletrônico do EF 4,1/MVC 3](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#GettingStartedwiththeEntityFramework4.1usingASP.NETMVC).

## <a name="the-contoso-university-web-application"></a>O aplicativo Web da Contoso University

O aplicativo que você criará nestes tutoriais é um site simples de uma universidade.

Os usuários podem exibir e atualizar informações de alunos, cursos e instrutores. Estas são algumas das telas que você criará.

![Students_Index_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image1.png)

![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image2.png)

O estilo de interface do usuário desse site foi mantido perto do que é gerado pelos modelos internos, de modo que o tutorial possa se concentrar principalmente em como usar o Entity Framework.

## <a name="prerequisites"></a>Pré-requisitos

As orientações e capturas de tela neste tutorial pressupõem que você esteja usando o [visual studio 2012](https://www.microsoft.com/visualstudio/eng/downloads) ou o [Visual Studio 2012 Express para Web](https://go.microsoft.com/fwlink/?LinkID=275131), com a atualização mais recente e o SDK do Azure para .net instalados a partir de julho de 2013. Você pode obter tudo isso com o seguinte link:

[SDK do Azure para .NET (Visual Studio 2012)](https://go.microsoft.com/fwlink/?LinkId=254364)

Se você tiver o Visual Studio instalado, o link acima instalará os componentes ausentes. Se você não tiver o Visual Studio, o link instalará o Visual Studio 2012 Express para Web. Você pode usar Visual Studio 2013, mas alguns dos procedimentos e telas necessários serão diferentes.

## <a name="create-an-mvc-web-application"></a>Criar um aplicativo Web MVC

Abra o Visual Studio e crie um novo projeto C# chamado "ContosoUniversity" usando o modelo de **aplicativo Web ASP.NET MVC 4** . Certifique-se de que você se destina **.NET Framework 4,5** (você usará [ `enum` Propriedades](https://msdn.microsoft.com/data/hh859576.aspx)e que requer o .NET 4,5).

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image3.png)

Na caixa de diálogo **novo projeto do ASP.NET MVC 4** , selecione o modelo de **aplicativo da Internet** .

Deixe o mecanismo de exibição do **Razor** selecionado e deixe a caixa de seleção **criar um projeto de teste de unidade** desmarcada.

Clique em **OK**.

![Project_template_options](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image4.png)

## <a name="set-up-the-site-style"></a>Configurar o estilo do site

Algumas alterações simples configurarão o menu do site, o layout e a home page.

Abra *Views\Shared \\ _Layout. cshtml* e substitua o conteúdo do arquivo pelo código a seguir. As alterações são realçadas.

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample1.cshtml?highlight=5,15,25-28,43)]

Este código faz as seguintes alterações:

- Substitui as instâncias de modelo de "meu aplicativo MVC ASP.NET" e "seu logotipo aqui" por "Contoso University".
- Adiciona vários links de ação que serão usados posteriormente no tutorial.

No *Views\Home\Index.cshtml*, substitua o conteúdo do arquivo pelo código a seguir para eliminar os parágrafos de modelo sobre ASP.net e MVC:

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample2.cshtml)]

No *Controllers\HomeController.cs*, altere o valor de `ViewBag.Message` no `Index` método de ação para "bem-vindo à Contoso University!", conforme mostrado no exemplo a seguir:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample3.cs?highlight=3)]

Pressione CTRL + F5 para executar o site. Você verá a home page com o menu principal.

![Contoso_University_home_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image5.png)

## <a name="create-the-data-model"></a>Criar o modelo de dados

Em seguida, você criará as classes de entidade para o aplicativo Contoso University. Você começará com as três seguintes entidades:

![Class_diagram](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image6.png)

Há uma relação um-para-muitos entre as entidades `Student` e `Enrollment`, e uma relação um-para-muitos entre as entidades `Course` e `Enrollment`. Em outras palavras, um aluno pode ser registrado em qualquer quantidade de cursos e um curso pode ter qualquer quantidade de alunos registrados.

Nas seções a seguir, você criará uma classe para cada uma dessas entidades.

> [!NOTE]
> Se você tentar compilar o projeto antes de concluir a criação de todas essas classes de entidade, obterá erros do compilador.

### <a name="the-student-entity"></a>A entidade Student

![Student_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image7.png)

Na pasta *modelos* , crie *Student.cs* e substitua o código existente pelo código a seguir:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample4.cs)]

A propriedade `StudentID` se tornará a coluna de chave primária da tabela de banco de dados que corresponde a essa classe. Por padrão, o Entity Framework interpreta uma propriedade nomeada `ID` ou *ClassName* `ID` como a chave primária.

A propriedade `Enrollments` é uma *propriedade de navegação*. As propriedades de navegação armazenam outras entidades que estão relacionadas a essa entidade. Nesse caso, a `Enrollments` propriedade de uma `Student` entidade manterá todas as `Enrollment` entidades relacionadas a essa `Student` entidade. Em outras palavras, se uma determinada `Student` linha no banco de dados tiver duas `Enrollment` linhas relacionadas (linhas que contêm o valor de chave primária do aluno em sua `StudentID` coluna de chave estrangeira), a `Student` propriedade de navegação dessa entidade `Enrollments` conterá essas duas `Enrollment` entidades.

As propriedades de navegação são normalmente definidas como `virtual` para que possam aproveitar determinadas funcionalidades de Entity Framework, como o *carregamento lento*. (O carregamento lento será explicado posteriormente, no tutorial [lendo dados relacionados](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) mais adiante nesta série.

Se uma propriedade de navegação pode armazenar várias entidades (como em relações muitos para muitos ou um-para-muitos), o tipo precisa ser uma lista na qual entradas podem ser adicionadas, excluídas e atualizadas, como `ICollection`.

### <a name="the-enrollment-entity"></a>A entidade de registro

![Enrollment_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image8.png)

Na pasta *Models*, crie *Enrollment.cs* e substitua o código existente pelo seguinte código:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample5.cs)]

A propriedade grau é uma [Enumeração](https://msdn.microsoft.com/data/hh859576.aspx). O ponto de interrogação após a declaração de tipo `Grade` indica que a propriedade `Grade` permite valor [anulável](https://msdn.microsoft.com/library/2cf62fcy.aspx). Uma classificação que é nula é diferente de uma taxa zero: null significa que uma classificação não é conhecida ou ainda não foi atribuída.

A propriedade `StudentID` é uma chave estrangeira e a propriedade de navegação correspondente é `Student`. Uma entidade `Enrollment` é associada a uma entidade `Student`, de modo que a propriedade possa armazenar apenas uma única entidade `Student` (ao contrário da propriedade de navegação `Student.Enrollments` que você viu anteriormente, que pode armazenar várias entidades `Enrollment`).

A propriedade `CourseID` é uma chave estrangeira e a propriedade de navegação correspondente é `Course`. Uma entidade `Enrollment` está associada a uma entidade `Course`.

### <a name="the-course-entity"></a>A entidade Course

![Course_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image9.png)

Na pasta *modelos* , crie *Course.cs*, substituindo o código existente pelo código a seguir:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample6.cs)]

A propriedade `Enrollments` é uma propriedade de navegação. Uma entidade `Course` pode estar relacionada a qualquer quantidade de entidades `Enrollment`.

Vamos falar mais sobre o [[DatabaseGenerated](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute(v=vs.110).aspx)([DatabaseGeneratedOption](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedoption(v=vs.95).aspx). Nenhum)] no próximo tutorial. Basicamente, esse atributo permite que você insira a chave primária do curso, em vez de fazer com que ela seja gerada pelo banco de dados.

## <a name="create-the-database-context"></a>Criar o contexto de banco de dados

A classe principal que coordena Entity Framework funcionalidade para um determinado modelo de dados é a classe de *contexto do banco* de dado. Você cria essa classe derivando da classe [System. Data. Entity. DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx) . No código, especifique quais entidades são incluídas no modelo de dados. Também personalize o comportamento específico do Entity Framework. Neste projeto, a classe é chamada `SchoolContext`.

Crie uma pasta chamada *Dal* (para camada de acesso a dados). Nessa pasta, crie um novo arquivo de classe chamado *SchoolContext.cs* e substitua o código existente pelo código a seguir:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample7.cs)]

Esse código cria uma propriedade [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=VS.103).aspx) para cada conjunto de entidades. Na terminologia Entity Framework, um *conjunto de entidades* geralmente corresponde a uma tabela de banco de dados e uma *entidade* corresponde a uma linha na tabela.

A `modelBuilder.Conventions.Remove` instrução no método [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) impede que os nomes de tabela sejam plural. Se você não fez isso, as tabelas geradas seriam nomeadas `Students` , `Courses` e `Enrollments` . Em vez disso, os nomes de tabela serão `Student` , `Course` e `Enrollment` . Os desenvolvedores não concordam sobre se os nomes de tabela devem ser pluralizados ou não. Este tutorial usa o formulário singular, mas o ponto importante é que você pode selecionar qualquer formulário que preferir, incluindo ou omitindo esta linha de código.

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

O [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx) é uma versão leve do Mecanismo de Banco de Dados de SQL Server Express que inicia sob demanda e é executado no modo de usuário. O LocalDB é executado em um modo de execução especial de SQL Server Express que permite que você trabalhe com bancos de dados como arquivos *. MDF* . Normalmente, os arquivos de banco de dados LocalDB são mantidos na pasta *\_ Data app* de um projeto Web. O recurso de instância de usuário no SQL Server Express também permite que você trabalhe com arquivos *. MDF* , mas o recurso de instância de usuário foi preterido; Portanto, o LocalDB é recomendado para trabalhar com arquivos *. MDF* .

Normalmente SQL Server Express não é usado para aplicativos Web de produção. O LocalDB, em particular, não é recomendado para uso em produção com um aplicativo Web porque ele não foi projetado para funcionar com o IIS.

No Visual Studio 2012 e versões posteriores, o LocalDB é instalado por padrão com o Visual Studio. No Visual Studio 2010 e versões anteriores, SQL Server Express (sem o LocalDB) é instalado por padrão com o Visual Studio; Você precisará instalá-lo manualmente se estiver usando o Visual Studio 2010.

Neste tutorial, você trabalhará com o LocalDB para que o banco de dados possa ser armazenado na pasta *\_ Data app* como um arquivo *. MDF* . Abra o arquivo de *Web.config* raiz e adicione uma nova cadeia de conexão à `connectionStrings` coleção, conforme mostrado no exemplo a seguir. (Certifique-se de atualizar o arquivo de *Web.config* na pasta raiz do projeto. Há também um arquivo de *Web.config* está na subpasta *views* que você não precisa atualizar.)

[!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample8.xml)]

Por padrão, a Entity Framework procura uma cadeia de conexão denominada igual à `DbContext` classe ( `SchoolContext` para este projeto). A cadeia de conexão que você adicionou especifica um banco de dados LocalDB chamado *ContosoUniversity. MDF* localizado na pasta *\_ dados do aplicativo* . Para obter mais informações, consulte [SQL Server cadeias de conexão para aplicativos Web ASP.net](https://msdn.microsoft.com/library/jj653752.aspx).

Na verdade, você não precisa especificar a cadeia de conexão. Se você não fornecer uma cadeia de conexão, Entity Framework criará uma para você; no entanto, o banco de dados pode não estar na pasta *\_ Data app* do seu aplicativo. Para obter informações sobre onde o banco de dados será criado, consulte [Code First para um novo banco de dados](https://msdn.microsoft.com/data/jj193542).

A `connectionStrings` coleção também tem uma cadeia de conexão chamada `DefaultConnection` que é usada para o banco de dados de associação. Você não usará o banco de dados de associação neste tutorial. A única diferença entre as duas cadeias de conexão é o nome do banco de dados e o valor do atributo Name.

## <a name="set-up-and-execute-a-code-first-migration"></a>Configurar e executar uma migração de Code First

Quando você começa a desenvolver um aplicativo pela primeira vez, o modelo de dados é alterado com frequência e, sempre que o modelo é alterado, ele fica fora de sincronia com o banco de dado. Você pode configurar o Entity Framework para descartar e recriar o banco de dados automaticamente sempre que alterar o modelo. Isso não é um problema no início do desenvolvimento porque os dados de teste são facilmente recriados, mas depois de você ter implantado na produção, geralmente você deseja atualizar o esquema do banco de dados sem descartar o banco de dados. O recurso de migrações permite que Code First atualize o banco de dados sem descartar e recriá-lo. No início do ciclo de desenvolvimento de um novo projeto, talvez você queira usar o [DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=vs.103).aspx) para descartar, recriar e refazer a propagação do banco de dados sempre que o modelo for alterado. Um que você está pronto para implantar seu aplicativo, você pode converter para a abordagem de migrações. Para este tutorial, você usará apenas as migrações. Para obter mais informações, consulte série Screencast de [migrações do Code First](https://msdn.microsoft.com/data/jj591621) e [migrações](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx).

### <a name="enable-code-first-migrations"></a>Habilitar as Migrações Code First

1. No menu **ferramentas** , clique em **Gerenciador de pacotes NuGet** e em **console do Gerenciador de pacotes**.

    ![Selecting_Package_Manager_Console](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image10.png)
2. No `PM>` prompt, insira o seguinte comando:

    [!code-powershell[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample9.ps1)]

    ![comando Enable-Migrations](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image11.png)

    Esse comando cria uma pasta *Migrations* no projeto ContosoUniversity e coloca essa pasta em um arquivo *Configuration.cs* que você pode editar para configurar as migrações.

    ![Pasta de migrações](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image12.png)

    A `Configuration` classe inclui um `Seed` método que é chamado quando o banco de dados é criado e toda vez que ele é atualizado após uma alteração no modelo.

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample10.cs)]

    A finalidade desse `Seed` método é permitir que você insira dados de teste no banco de dado depois que Code First o cria ou atualiza.

### <a name="set-up-the-seed-method"></a>Configurar o método semente

O método [semente](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) é executado quando migrações do Code First cria o banco de dados e toda vez que ele atualiza o banco de dados para a migração mais recente. A finalidade do método de semente é permitir que você insira dados em suas tabelas antes que o aplicativo acesse o Database pela primeira vez.

Em versões anteriores do Code First, antes da liberação das migrações, era comum `Seed` os métodos inserir dados de teste, porque com cada alteração de modelo durante o desenvolvimento, o banco de dado tinha que ser completamente excluído e recriado do zero. Com o Migrações do Code First, os dados de teste são retidos após as alterações do banco de dados, de modo que, inclusive, o método de [semente](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) não é necessário normalmente. Na verdade, você não quer que o `Seed` método insira dados de teste se você estiver usando migrações para implantar o Database para produção, pois o `Seed` método será executado em produção. Nesse caso, você deseja que o método seja inserido no `Seed` banco de dados somente os dados que você deseja inserir na produção. Por exemplo, talvez você queira que o banco de dados inclua nomes de departamento reais na `Department` tabela quando o aplicativo se tornar disponível em produção.

Para este tutorial, você usará migrações para implantação, mas o `Seed` método inserirá dados de teste de qualquer forma para facilitar a visualização de como a funcionalidade do aplicativo funciona sem a necessidade de inserir manualmente muitos dados.

1. Substitua o conteúdo do arquivo *Configuration.cs* pelo código a seguir, que carregará os dados de teste no novo banco de dado. 

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

    O método [semente](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) usa o objeto de contexto de banco de dados como um parâmetro de entrada, e o código no método usa esse objeto para adicionar novas entidades ao banco de dados. Para cada tipo de entidade, o código cria uma coleção de novas entidades, adiciona-as à propriedade [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx) apropriada e salva as alterações no banco de dados. Não é necessário chamar o método [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) depois de cada grupo de entidades, como é feito aqui, mas fazer isso ajuda a localizar a origem de um problema se ocorrer uma exceção enquanto o código estiver gravando no banco de dados.

    Algumas das instruções que inserem dados usam o método [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) para executar uma operação "Upsert". Como o `Seed` método é executado com cada migração, você não pode apenas inserir dados, pois as linhas que você está tentando adicionar já estarão lá após a primeira migração que cria o banco de dados. A operação "Upsert" impede erros que ocorrerão se você tentar inserir uma linha que já existe, mas ela ***substitui*** quaisquer alterações nos dados que você tenha feito durante o teste do aplicativo. Com os dados de teste em algumas tabelas, talvez você não queira que isso aconteça: em alguns casos, ao alterar os dados durante o teste, você deseja que as alterações permaneçam após as atualizações do banco. Nesse caso, você deseja fazer uma operação de inserção condicional: Insira uma linha somente se ela ainda não existir. O método semente usa ambas as abordagens.

    O primeiro parâmetro passado para o método [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) especifica a propriedade a ser usada para verificar se já existe uma linha. Para os dados de aluno de teste que você está fornecendo, a `LastName` propriedade pode ser usada para essa finalidade, pois cada sobrenome na lista é exclusivo:

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

    Esse código pressupõe que os últimos nomes são exclusivos. Se você adicionar manualmente um aluno com um sobrenome duplicado, obterá a seguinte exceção na próxima vez que executar uma migração.

    A sequência contém mais de um elemento

    Para obter mais informações sobre o `AddOrUpdate` método, consulte [tome cuidado com o método EF 4,3 AddOrUpdate](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) no blog de Julie Lerman.

    O código que adiciona `Enrollment` entidades não usa o `AddOrUpdate` método. Ele verifica se uma entidade já existe e insere a entidade, caso ela não exista. Essa abordagem preservará as alterações feitas em uma classificação de registro quando as migrações são executadas. O código percorre cada membro da `Enrollment` [lista](https://msdn.microsoft.com/library/6sh2ey19.aspx) e, se o registro não for encontrado no banco de dados, ele adicionará o registro ao banco de dados. Na primeira vez que você atualizar o banco de dados, o banco de dados estará vazio, portanto, ele adicionará cada registro.

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample13.cs)]

    Para obter informações sobre como depurar o `Seed` método e como lidar com dados redundantes, como dois alunos nomeados como "Alexander Carson", consulte o [Entity Framework de Fe (propagação e depuração)](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) no blog de Rick Anderson.
2. Compile o projeto.

### <a name="create-and-execute-the-first-migration"></a>Criar e executar a primeira migração

1. Na janela do console do Gerenciador de pacotes, digite os seguintes comandos: 

    [!code-powershell[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample14.ps1)]

    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image13.png)

    O `add-migration` comando adiciona à pasta Migrations um arquivo *\_ InitialCreate.cs [dateStamp]* que contém o código que cria o banco de dados. O primeiro parâmetro ( `InitialCreate)` é usado para o nome do arquivo e pode ser o que você desejar; normalmente, você escolhe uma palavra ou frase que resume o que está sendo feito na migração. Por exemplo, você pode nomear uma migração mais tarde como &quot; Adddepartmenttable &quot; .

    ![Pasta de migrações com migração inicial](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image14.png)

    O `Up` método da `InitialCreate` classe cria as tabelas de banco de dados que correspondem aos conjuntos de entidades de modelo de dado e o `Down` método os exclui. As migrações chamam o método `Up` para implementar as alterações do modelo de dados para uma migração. Quando você insere um comando para reverter a atualização, as Migrações chamam o método `Down`. O código a seguir mostra o conteúdo do `InitialCreate` arquivo:

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample15.cs)]

    O `update-database` comando executa o `Up` método para criar o banco de dados e, em seguida, executa o `Seed` método para preencher o banco de dados.

Um banco de dados SQL Server foi criado para seu modelo de dado. O nome do banco de dados é *ContosoUniversity* e o arquivo *. MDF* está na pasta de *\_ dados de aplicativo* do projeto, pois é isso que você especificou na cadeia de conexão.

Você pode usar o **Gerenciador de servidores** ou o **pesquisador de objetos do SQL Server** (SSOX) para exibir o banco de dados no Visual Studio. Para este tutorial, você usará **Gerenciador de servidores**. No Visual Studio Express 2012 para Web, **Gerenciador de servidores** é chamado de **Gerenciador de banco de dados**.

1. No menu **Exibir** , clique em **Gerenciador de servidores**.
2. Clique no ícone **Adicionar conexão** .

    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image15.png)
3. Se você receber a caixa de diálogo **escolher fonte de dados** , clique em **Microsoft SQL Server** e em **continuar**.  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image16.png)
4. Na caixa de diálogo **Adicionar conexão** , digite **(LocalDB) \V11.0** para o **nome do servidor**. Em **selecionar ou inserir um nome de banco de dados**, selecione **ContosoUniversity.**  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image17.png)
5. Clique em **OK.**
6. Expanda **SchoolContext** e expanda **tabelas**.  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image18.png)
7. Clique com o botão direito do mouse na tabela **Student** e clique em **Mostrar dados da tabela** para ver as colunas que foram criadas e as linhas que foram inseridas na tabela.

    ![Tabela de aluno](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image19.png)

## <a name="creating-a-student-controller-and-views"></a>Criando um controlador de aluno e exibições

A próxima etapa é criar um controlador MVC ASP.NET e exibições em seu aplicativo que podem funcionar com uma dessas tabelas.

1. Para criar um `Student` controlador, clique com o botão direito do mouse na pasta **controladores** em **Gerenciador de soluções**, selecione **Adicionar** e, em seguida, clique em **controlador**. Na caixa de diálogo **Adicionar controlador** , faça as seguintes seleções e clique em **Adicionar**: 

   - Nome do controlador: **StudentController**.
   - Modelo: **controlador MVC com ações de leitura/gravação e exibições, usando Entity Framework**.
   - Classe de modelo: **aluno (ContosoUniversity. Models)**. (Se você não vir essa opção na lista suspensa, compile o projeto e tente novamente.)
   - Classe de contexto de dados: **SchoolContext (ContosoUniversity. Models)**.
   - Exibições: **Razor (cshtml)**. (O padrão.)

     ![Add_Controller_dialog_box_for_Student_controller](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image20.png)
2. O Visual Studio abre o arquivo *Controllers\StudentController.cs* . Você vê uma variável de classe criada que instancia um objeto de contexto de banco de dados:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample16.cs)]

     O `Index` método de ação Obtém uma lista de alunos do conjunto de entidades dos *alunos* , lendo a `Students` propriedade da instância de contexto do banco de dados:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample17.cs)]

     A exibição *Student\Index.cshtml* exibe essa lista em uma tabela:

     [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample18.cshtml)]
3. Pressione CTRL+F5 para executar o projeto.

     Clique na guia **alunos** para ver os dados de teste `Seed` inseridos pelo método.

     ![Página de índice de estudante](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image21.png)

## <a name="conventions"></a>Convenções

A quantidade de código que você tinha de escrever para que o Entity Framework seja capaz de criar um banco de dados completo para você é o mínimo devido ao uso de *convenções* ou suposições que o Entity Framework faz. Alguns deles já foram observados:

- As formas pluraled de nomes de classe de entidade são usadas como nomes de tabela.
- Os nomes de propriedade de entidade são usados para nomes de coluna.
- As propriedades de entidade que são nomeadas `ID` ou *ClassName* `ID` são reconhecidas como propriedades de chave primária.

Você viu que as convenções podem ser substituídas (por exemplo, se você especificou que os nomes de tabela não devem estar em plural), e você aprenderá mais sobre convenções e como substituí-las no tutorial [criando um modelo de dados mais complexo](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md) mais adiante nesta série. Para obter mais informações, consulte [convenções de Code First](https://msdn.microsoft.com/data/jj679962).

## <a name="summary"></a>Resumo

Agora você criou um aplicativo simples que usa o Entity Framework e SQL Server Express para armazenar e exibir dados. No tutorial a seguir, você aprenderá a executar operações CRUD (criar, ler, atualizar, excluir) básicas. Você pode deixar comentários na parte inferior desta página. Informe-nos como você gostou desta parte do tutorial e como poderíamos aprimorá-la.

Links para outros recursos de Entity Framework podem ser encontrados no [mapa de conteúdo de acesso a dados do ASP.net](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Próximo](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
