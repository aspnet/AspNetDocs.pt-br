---
uid: whitepapers/aspnet-data-access-content-map
title: Acesso a dados do ASP.NET – recursos recomendados | Microsoft Docs
author: rick-anderson
description: Este tópico fornece links para recursos de documentação sobre como acessar dados em aplicativos Web do ASP.NET, principalmente usando o Entity Framework e o SQL se...
ms.author: riande
ms.date: 07/25/2013
ms.assetid: f8157be1-4ab9-469e-ad3a-0ccc80b56c00
msc.legacyurl: /whitepapers/aspnet-data-access-content-map
msc.type: content
ms.openlocfilehash: 357851f195bf233c7c34a32bd156e4408d3e1b24
ms.sourcegitcommit: c62ec20b453cee3249eb894ecd75013b57d078f0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/21/2020
ms.locfileid: "92345149"
---
# <a name="aspnet-data-access---recommended-resources"></a>Acesso a Dados do ASP.NET – Recursos recomendados

> Este tópico fornece links para recursos de documentação sobre como acessar dados em aplicativos Web do ASP.NET, principalmente usando o Entity Framework e SQL Server.
> 
> Se você conhece uma ótima postagem no blog, thread [StackOverflow](http://stackoverflow.com) ou qualquer outro link que seria útil, [envie um email](mailto:aspnetue@microsoft.com?subject=Data Access Content Map) com o link.
> 
> Última atualização 4/3/2014

Este tópico contém as seguintes seções:

- [Introdução com acesso a dados no ASP.NET](#gettingstarted)
- [Usando o Entity Framework](#ef)

    - [Usando Entity Framework Code First](#cf)
    - [Usando Migrações do Entity Framework Code First](#efcfmigrations)
    - [Usando Entity Framework Database First ou Model First (o designer do EF)](#efdbf)
    - [Carregando dados relacionados no Entity Framework (carregamento lento, carregamento adiantado e carregamento explícito)](#efrelateddata)
    - [Otimizando o desempenho de Entity Framework](#optimizingef)
    - [Manipulando a simultaneidade em um aplicativo Entity Framework](#efconcurrency)
    - [Livros sobre o Entity Framework](#efbooks)
    - [Recursos de Entity Framework adicionais](#otherefresources)
- [Associação de dados em aplicativos ASP.NET Web Forms](#wfdatabinding)

    - [Usando Associação de modelo de Web Forms](#wfmodelbinding)
    - [Usando Web Forms controles de fonte de dados](#wfdsc)
    - [Usando controles de Data-Bound de Web Forms e expressões de Data-Binding](#wfdbc)
- [Trabalhando com bancos de dados SQL Server](#sqlserver)

    - [Trabalhando com bancos de dados SQL Server Express LocalDB](#sslocaldb)
    - [Trabalhando com bancos de dados SQL Server Express](#sse)
    - [Trabalhando com o banco de dados SQL do Windows Azure](#ssdb)
    - [Escolhendo entre SQL Server e o banco de dados SQL do Windows Azure](#ssdbchoosing)
- [Trabalhando com sistemas de gerenciamento de banco de dados NoSQL](#nosql)
- [Usando consultas LINQ em aplicativos ASP.NET](#linq)
- [Usando Dados Dinâmicos scaffolding](#dd)
- [Protegendo o acesso a dados](#securing)
- [Otimizando o desempenho de acesso a dados](#optimizingdataaccess)
- [Implantando um banco de dados](#deploying)
- [Acessando dados por meio de um serviço Web](#webservice)
- [Recursos adicionais](#additional)

<a id="gettingstarted"></a>

## <a name="getting-started-with-data-access-in-aspnet"></a>Introdução com acesso a dados no ASP.NET

- [Opções de armazenamento de dados (criando Real-World aplicativos de nuvem com o Windows Azure)](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options.md). Capítulo de um livro eletrônico sobre o desenvolvimento para a nuvem. Apresenta bancos de dados NoSQL como uma alternativa que muitos desenvolvedores familiarizados com bancos de dados relacionais tendem a ignorar. Apresenta diretrizes sobre o que pensar ao escolher relacional ou NoSQL, ou escolher uma plataforma específica.
- [Opções de acesso a dados do ASP.net](https://msdn.microsoft.com/library/ms178359.aspx) (MSDN). Uma introdução às opções de acesso a dados para bancos de dados relacionais para ASP.NET e diretrizes sobre como escolher plataformas e métodos de acesso apropriados para seu cenário.
- [Banco de dados relacional](http://en.wikipedia.org/wiki/Relational_database). Wikipédia). Se você não trabalhou com bancos de dados relacionais, consulte esta página para obter uma introdução à terminologia e aos conceitos do banco de dados relacional. Para obter uma introdução ao SQL Server em particular, consulte [trabalhando com bancos de dados do SQL Server](#sqlserver) mais adiante neste tópico.

<a id="ef"></a>

## <a name="using-the-entity-framework"></a>Usando o Entity Framework

- [Abordagens de desenvolvimento de Entity Framework](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf) (MSDN). Orientação sobre como escolher uma abordagem de desenvolvimento de Entity Framework Database First, Model First ou Code First.

<a id="cf"></a>

### <a name="using-entity-framework-code-first"></a>Usando Entity Framework Code First

Os tutoriais a seguir oferecem aplicativos de exemplo para download:

- [Introdução com o EF 6 usando o MVC 5](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Aborda uma ampla gama de cenários de Code First Entity Framework, incluindo migrações e recursos do EF 6, como resiliência de conexão, interceptação de comandos e Async. Esta é uma versão atualizada da [série EF 5/MVC 4](../mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). A série anterior inclui um tutorial sobre o repositório e padrões de unidade de trabalho que não estão incluídos na nova série.
- [Introdução ao ASP.NET MVC 5](../mvc/overview/getting-started/introduction/getting-started.md). Cobre uma gama mais estreita de cenários de Code First Entity Framework, mas faz um trabalho mais abrangente de introduzir recursos do MVC.
- [Associação de modelo e Web Forms](https://go.microsoft.com/fwlink/?LinkId=286117). Usa Code First em um aplicativo Web Forms.
- [Introdução com ASP.NET 4,5 Web Forms](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Uma introdução à Web Forms com alguma cobertura de Code First. Usa associação de modelo.
- [Loja de música MVC](../mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1.md). Usa Code First em um aplicativo de comércio eletrônico MVC 3 que também implementa associação e autorização. A versão MVC e o sistema de associação do ASP.NET (autenticação e autorização) usados aqui estão desatualizados; para obter informações mais atualizadas sobre a associação do ASP.NET, consulte [https://asp.net/identity](https://asp.net/identity) .

Outros recursos:

- [Entity Framework-Code First a um banco de dados existente](https://msdn.microsoft.com/data/jj200620). Inglês. Vídeo e passo a passos que mostram como usar Code First com um banco de dados existente.
- [Centro de desenvolvedores de dados-Entity Framework](https://msdn.microsoft.com/data/ef). Inglês. Para obter um guia para Entity Framework documentação que foi criada e mantida pela equipe de Entity Framework, consulte o link [introdução](https://msdn.microsoft.com/data/ee712907) .

Consulte também [livros sobre a Entity Framework](#efbooks) e [recursos de Entity Framework adicionais](#otherefresources) mais adiante neste tópico.

<a id="efcfmigrations"></a>

### <a name="using-entity-framework-code-first-migrations"></a>Usando Migrações do Entity Framework Code First

A maioria dos Code First tutoriais listados acima abordam as migrações. Consulte também os recursos a seguir.

- [Implantação da Web do ASP.NET usando o Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). série de tutoriais de duas partes que mostra como usar Migrações do Code First para implantar um banco de dados.
- [Implante um aplicativo Secure ASP.NET MVC 5 com associação, OAuth e banco de dados SQL em um site do Windows Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Microsoft Azure). Como usar migrações para implantar dados de aplicativo e associação no Azure.
- [Visão geral da implantação da Web para Visual Studio e ASP.net](https://msdn.microsoft.com/library/dd394698.aspx#dbdeployment). Consulte a seção **Configurando a implantação de banco de dados no Visual Studio** para obter uma explicação de como o migrações do Code First é integrado aos recursos de implantação da Web do Visual Studio.
- [Centro de desenvolvedores de dados-migrações do Code First](https://msdn.microsoft.com/data/jj591621) (MSDN). A documentação de migrações da equipe de Entity Framework.
- [Série de screencasts de migrações](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx). Blog do EF). Três vídeos sobre tópicos avançados em Migrações do Code First.
- [Migrações do Code First com páginas da Web do ASP.net sites](http://www.mikesdotnetting.com/Article/217/Code-First-Migrations-With-ASP.NET-Web-Pages-Sites). Blog Mikesdotnetting). Mostra como usar Code First migrações com um site Páginas da Web do ASP.NET colocando o contexto de dados em um projeto de biblioteca de classes do Visual Studio.

<a id="efdbf"></a>

### <a name="using-entity-framework-database-first-or-model-first-the-ef-designer"></a>Usando Entity Framework Database First ou Model First (o designer do EF)

- [Introdução com Entity Framework 6 Database First usando o MVC 5](../mvc/overview/getting-started/database-first-development/setting-up-database.md). Execute um script em Gerenciador de Servidores para criar um banco de dados e, em seguida, use o designer de Entity Framework para criar o modelo de dado. Mostra como criar páginas da Web simples do CRUD e para outras funções de manipulação de dados, você pode seguir um dos tutoriais de Code First, já que todos os fluxos de trabalho do EF usam a mesma API DbContext.

Os recursos a seguir são mais antigos. Eles serão úteis se você quiser usar a versão 4,0 do Entity Framework e quiser usar um controle da fonte de dados para a vinculação de dados em um aplicativo Web Forms.

- [Introdução com o Entity Framework 4,0](../web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md). Mostra como usar o controle **EntityDataSource** .
- [Continuando com o Entity Framework](../web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md)(mostra como usar o controle **ObjectDataSource** . Inclui um tutorial sobre a manipulação de simultaneidade, um tutorial sobre o desempenho do EF e um tutorial sobre o que há de novo no EF 4,0.

<a id="efrelateddata"></a>

### <a name="handling-related-data-in-entity-framework-lazy-loading-eager-loading-and-explicit-loading"></a>Manipulando dados relacionados no Entity Framework (carregamento lento, carregamento adiantado e carregamento explícito)

- [Leitura de dados relacionados com o Entity Framework em um aplicativo MVC ASP.net](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md). Code First, aplicativo de exemplo MVC. Os métodos mostrados também se aplicam à Web Forms Associação de modelo e ao fluxo de trabalho de Database First.
- [Data Developer Center – carregando entidades relacionadas](https://msdn.microsoft.com/data/jj574232) (MSDN). A documentação da equipe de Entity Framework sobre como carregar dados relacionados.

<a id="optimizingef"></a>

### <a name="optimizing-entity-framework-performance"></a>Otimizando o desempenho de Entity Framework

- [Cenários de Entity Framework avançados para um aplicativo ASP.net](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application.md). Mostra como executar suas próprias instruções SQL ou chamar seus próprios procedimentos armazenados, como desabilitar a detecção de alterações e como desabilitar a validação ao salvar as alterações.
- [Considerações sobre desempenho para o Entity Framework 5](https://msdn.microsoft.com/data/hh949853) (MSDN).
- [Considerações sobre desempenho (Entity Framework)](https://msdn.microsoft.com/library/cc853327) (MSDN).
- [Maximizar o desempenho com o Entity Framework em um aplicativo Web ASP.net](../web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md). Aplica-se a Entity Framework 4,0.
- Consulte também [otimizando o acesso a dados do ASP.net](#optimizingdataaccess) mais adiante neste tópico.

<a id="efconcurrency"></a>

### <a name="handling-concurrency-in-an-entity-framework-application"></a>Manipulando a simultaneidade em um aplicativo Entity Framework

- [Tratamento de simultaneidade com o Entity Framework em um aplicativo MVC ASP.net](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md). Code First, API DbContext, usando um aplicativo de exemplo MVC.
- [Data Developer Center – padrões de simultaneidade otimistas](https://msdn.microsoft.com/data/jj592904) (MSDN). A documentação de simultaneidade do Entity Framework Team.
- [Tratamento de simultaneidade com o Entity Framework em um aplicativo Web ASP.net](../web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md). Aplica-se a Entity Framework 4,0. Database First, API de ObjectContext, usando um aplicativo de exemplo Web Forms.

<a id="efrepository"></a><a id="efbooks"></a>

### <a name="books-about-the-entity-framework"></a>Livros sobre o Entity Framework

- [Entity Framework de programação: DbContext](http://shop.oreilly.com/product/0636920022237.do) por Julie Lerman e Rowan Miller.
- [Entity Framework de programação: Code First](http://shop.oreilly.com/product/0636920022220.do) de Julie Lerman e Rowan Miller.

Ambos os livros estão atualizados com as técnicas recomendadas atuais. Eles fornecem uma introdução mais abrangente, mas fácil de seguir, ao Entity Framework do que qualquer coisa disponível na Internet. Outro livro, [programação Entity Framework](http://shop.oreilly.com/product/9780596807252.do) por Julie Lerman, é maior e mais abrangente, mas é mais antigo e muitas das técnicas que ele abrange não são mais a maneira recomendada de usar o Entity Framework. Consulte também a lista de livros recomendados pela equipe de Entity Framework em [Data Developer Center-Books](https://msdn.microsoft.com/data/aa937716) no site do MSDN.

<a id="otherefresources"></a>

### <a name="other-entity-framework-resources"></a>Outros recursos de Entity Framework

- [Blog da equipe do Entity Framework (ADO.net)](https://blogs.msdn.com/b/adonet/). Um dos melhores recursos para as informações e anúncios mais atuais de novos aprimoramentos. Para outros Blogs relacionados ao EF, consulte o Blogs em introdução [ao Entity Framework](https://msdn.microsoft.com/data/ee712907).
- [Msdn Magazine](https://msdn.microsoft.com/magazine/default.aspx). Consulte a coluna **pontos de dados** , que costuma ser sobre tópicos relacionados ao Entity Framework.

<a id="wfdatabinding"></a>

## <a name="data-binding-in-aspnet-web-forms-applications"></a>Associação de dados em aplicativos ASP.NET Web Forms

- [ASP.NET Web Forms opções de acesso a dados](https://msdn.microsoft.com/library/jj822927.aspx) (MSDN) <a id="wfmodelbinding"></a> .

<a id="wfmodelbinding"></a>

### <a name="using-web-forms-model-binding"></a>Usando Associação de modelo de Web Forms

- [Associação de modelo e Web Forms](https://go.microsoft.com/fwlink/?LinkId=286117). Série de tutoriais usando o Code First EF.
- [Associação de modelo de Web Forms parte 1: selecionando dados](https://weblogs.asp.net/scottgu/archive/2011/09/06/web-forms-model-binding-part-1-selecting-data-asp-net-vnext-series.aspx) (blog de Scott Guthrie). Nessas Postagens de blog mais antigas, a propriedade que atualmente é chamada ItemType foi denominada ModelType, mas, caso contrário, as informações que elas contêm são válidas.
- [Associação de modelo de Web Forms parte 2: Filtrando dados](https://weblogs.asp.net/scottgu/archive/2011/09/12/web-forms-model-binding-part-2-filtering-data-asp-net-vnext-series.aspx) (blog de Scott Guthrie).
- [Associação de modelo de Web Forms parte 3: atualização e validação](https://weblogs.asp.net/scottgu/archive/2011/10/30/web-forms-model-binding-part-3-updating-and-validation-asp-net-4-5-series.aspx) (blog de Scott Guthrie).
- [ASP.NET 4,5 Web Forms Associação de modelo](../web-forms/videos/aspnet-web-forms-vnext/aspnet-45-web-forms-model-binding.md). (vídeo).
- [Associação de modelo parte 1-selecionando dados](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-model-binding-part-1-selecting-data.md) (vídeo).
- [Associação de modelo parte 2-filtragem](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-model-binding-part-2-filtering.md) (vídeo).
- [Introdução com ASP.NET 4,5 Web Forms-exibir itens de dados e detalhes](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details.md).

<a id="wfdsc"></a>

### <a name="using-web-forms-data-source-controls"></a>Usando Web Forms controles de fonte de dados

- [Controles de servidor Web de fonte de dados](https://msdn.microsoft.com/library/ms247258.aspx) (MSDN).
- [Anunciando o lançamento do provedor de dados dinâmicos e controle de EntityDataSource para Entity Framework 6](https://blogs.msdn.com/b/webdev/archive/2014/02/28/announcing-the-release-of-dynamic-data-provider-and-entitydatasource-control-for-entity-framework-6.aspx) (blog de desenvolvimento na Web da Microsoft).

<a id="wfdbc"></a>

### <a name="using-web-forms-data-bound-controls-and-data-binding-expressions"></a>Usando controles de Data-Bound de Web Forms e expressões de Data-Binding

- [Associação de modelo e Web Forms](https://go.microsoft.com/fwlink/?LinkId=286117). Série de tutoriais que usa Code First do EF.
- [Introdução com ASP.NET 4,5 Web Forms-exibir itens de dados e detalhes](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details.md).
- [Controles de dados com rigidez de tipos](https://weblogs.asp.net/scottgu/archive/2011/09/02/strongly-typed-data-controls-asp-net-vnext-series.aspx) (blog de Scott Guthrie).
- [Controles de dados com rigidez de tipos](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-strongly-typed-data-controls.md) (vídeo).
- [ASP.NET 4,5 Web Forms controles de dados tipados fortes](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-strongly-typed-data-controls.md) (vídeo).
- [Controles de servidor Web associados a dados](https://msdn.microsoft.com/library/ms228214.aspx) (MSDN).
- [Visão geral das expressões de ligação de dados](https://msdn.microsoft.com/library/ms178366.aspx) (MSDN). Esta página só cobre **eval** e **BIND**; Ele não foi atualizado para incluir **Item** e **BindItem**.

<a id="sqlserver"></a>

## <a name="working-with-sql-server-databases"></a>Trabalhando com bancos de dados SQL Server

- [Recursos de banco de dados do SQL Server](https://msdn.microsoft.com/library/hh230827.aspx) (MSDN). Para obter uma introdução geral a uma grande variedade de tópicos de SQL Server, consulte as entradas abaixo dele no sumário.
- [SQL Server Editions](https://msdn.microsoft.com/library/ms178359.aspx#sqlserver) (MSDN). Um resumo das edições SQL Server disponíveis, com links para mais informações sobre cada uma delas.)
- [SQL Server cadeias de conexão para aplicativos Web ASP.net](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN).
- [Usando SQL Server Compact para aplicativos Web ASP.net](https://msdn.microsoft.com/library/ms247257.aspx) (MSDN).
- [Microsoft SQL Server: exemplos de produtos de banco de dados](https://github.com/Microsoft/sql-server-samples/blob/master/samples/databases/adventure-works/README.md). Bancos de dados AdventureWorks de exemplo.
- [Instalando bancos de dados de exemplo](https://github.com/Microsoft/sql-server-samples/blob/master/samples/databases/adventure-works/README.md). Além dos métodos mostrados aqui, você também pode baixar um dos arquivos. MDF de exemplo na \_ pasta de dados do aplicativo de um projeto Web, converter o banco de dado em LocalDB e criar uma cadeia de conexão de LocalDB. Para obter informações sobre como fazer isso, consulte [como: atualizar para o LocalDB](https://msdn.microsoft.com/library/hh873188.aspx).

Consulte também as seções a seguir sobre como trabalhar com SQL Server Express e LocalDB e escolher entre SQL Server e banco de dados SQL.

<a id="sslocaldb"></a>

### <a name="working-with-sql-server-express-localdb-databases"></a>Trabalhando com bancos de dados SQL Server Express LocalDB

- [SQL Server Express 2012 LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (MSDN). A introdução oficial do MSDN ao LocalDB.
- [SQL Server cadeias de conexão para aplicativos Web ASP.net](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN).
- [Como: atualizar para o LocalDB](https://msdn.microsoft.com/library/hh873188.aspx) (MSDN). Como migrar um arquivo. MDF de uma versão anterior do SQL Server Express para o LocalDB. Você também precisará passar por esse processo se baixar um dos bancos de [dados de exemplo SQL Server 2012](https://go.microsoft.com/fwlink/?linkid=117483).
- [Apresentando o LocalDB, um SQL Express](https://go.microsoft.com/fwlink/?LinkId=234375) (blog SQL Server Express) aprimorado. Tem mais informações sobre por que o LocalDB foi criado do que está incluído no MSDN.
- [LocalDB: onde está meu banco de dados?](https://go.microsoft.com/fwlink/?LinkId=234376) (Blog SQL Server Express). Informações sobre onde os arquivos de banco de dados LocalDB são criados.
- [Usando o LocalDB com o IIS completo, parte 1: perfil do usuário](https://blogs.msdn.com/b/sqlexpress/archive/2011/12/09/using-localdb-with-full-iis-part-1-user-profile.aspx) (SQL Server Express blog). O LocalDB não foi projetado para funcionar com o IIS. Esta série de postagens de blog explica os problemas e algumas soluções alternativas.

<a id="sse"></a>

### <a name="working-with-sql-server-express-databases"></a>Trabalhando com bancos de dados SQL Server Express

- [SQL Server cadeias de conexão para aplicativos Web ASP.net](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN). Se você usar a configuração de cadeia de conexão AttachDBFileName com SQL Server Express, consulte especialmente a seção instância de usuário desta página.
- [Como se apropriar do seu SQL Server Express local 2008](https://blogs.msdn.com/b/sqlexpress/archive/2010/02/23/how-to-take-ownership-of-your-local-sql-server-2008-express.aspx) (SQL Server Express blog). Um problema comum não é ser capaz de trabalhar com bancos de dados SQL Server Express porque você não é um administrador na instância de SQL Server Express. Por padrão, somente a pessoa que instalou o SQL Server Express é um administrador. Este blog explica como fazer você mesmo um administrador de SQL Server Express se você for um administrador no computador.
- [Meu aplicativo Web ASP.NET pode usar um banco de dados SQL Server Express em produção?](https://msdn.microsoft.com/library/jj653753.aspx#sql_express_in_production) (MSDN).

<a id="ssdb"></a>

### <a name="working-with-windows-azure-sql-database"></a>Trabalhando com o banco de dados SQL do Windows Azure

- [Implante um aplicativo MVC do ASP.net seguro com associação, OAuth e banco de dados SQL em um site do Windows Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data) (Microsoft Azure site).
- [Bancos de dados SQL](https://docs.microsoft.com/azure/sql-database/) (Microsoft Azure site). Tutoriais de introdução e guias de instruções.
- [Banco de dados SQL do Windows Azure](https://msdn.microsoft.com/library/windowsazure/ee336279.aspx) (MSDN). O nó de nível superior do Sumário do banco de dados SQL no MSDN.
- [Índice de artigos do TechNet wiki sobre o banco de dados SQL do Windows Azure](https://social.technet.microsoft.com/wiki/contents/articles/2267.windows-azure-sql-database-technet-wiki-articles-index-en-us.aspx) (site do Microsoft TechNet).
- [Bloco de aplicativo para tratamento de falhas transitórias](https://msdn.microsoft.com/library/hh680934(v=PandP.50).aspx). Uma estrutura que permite lidar com falhas de rede transitórias e erros de conexão que resultam da limitação. Disponível em um pacote NuGet: [Enterprise Library 5,0-bloco de aplicativos para tratamento de falhas transitórias](http://nuget.org/packages/EnterpriseLibrary.WindowsAzure.TransientFaultHandling).
- [Introdução com o banco de dados SQL e o Entity Framework](https://msdn.microsoft.com/data/jj556244) (MSDN).
- [Kit de treinamento do Windows Azure](https://www.microsoft.com/download/details.aspx?id=8396) (centro de download da Microsoft). Inclui laboratórios práticos para o banco de dados SQL.
- [Fórum da Comunidade do banco de dados SQL do Windows Azure](https://social.msdn.microsoft.com/Forums/ssdsgetstarted/threads).
- [Mudando para o banco de dados SQL do Windows Azure](https://msdn.microsoft.com/library/ff803375.aspx) (MSDN). Um capítulo de um cenário abrangente de ponta a ponta da equipe de padrões e práticas da Microsoft. Aborda por que você pode desejar migrar e como migrar do SQL Server para o banco de dados SQL.
- [Migração de bancos de dados SQL Server para o banco de dados SQL do Windows Azure](https://msdn.microsoft.com/library/windowsazure/jj156160.aspx) (MSDN).
- [Assistente de migração de banco de dados SQL](http://sqlazuremw.codeplex.com/). Uma ferramenta de software livre para migrar bancos de dados do para o e para o SQL Database.

<a id="ssdbchoosing"></a>

### <a name="choosing-between-sql-server-and-windows-azure-sql-database"></a>Escolhendo entre SQL Server e o banco de dados SQL do Windows Azure

- [Compare SQL Server com o banco de dados SQL do Windows Azure](https://social.technet.microsoft.com/wiki/contents/articles/996.compare-sql-server-with-windows-azure-sql-database-en-us.aspx) (site do Microsoft TechNet).
- [Migração de dados para o banco de dado SQL do Windows Azure: ferramentas e técnicas](https://msdn.microsoft.com/library/windowsazure/hh694043.aspx) (MSDN). Inclui seções que comparam SQL Server ao banco de dados SQL e fornecem orientação sobre quando migrar do SQL Server para o banco de dados SQL.
- [Guia de entrega do banco de dados SQL do Windows Azure](https://social.technet.microsoft.com/wiki/contents/articles/3398.windows-azure-sql-database-delivery-guide-en-us.aspx) (site do Microsoft TechNet).
- [SQL Server limitações de recursos (banco de dados SQL do Windows Azure)](https://msdn.microsoft.com/library/windowsazure/ff394115.aspx) (MSDN).
- [Armazenamento de tabelas do Windows Azure e banco de dados SQL do Windows Azure-comparação e contraste](https://msdn.microsoft.com/library/jj553018.aspx) (MSDN). Para um aplicativo que você implanta no Windows Azure, o armazenamento de tabelas do Windows Azure pode ser uma alternativa para o banco de dados SQL do Windows Azure. Este tópico ajuda você a decidir entre essas alternativas.
- [Banco de dados SQL do Windows Azure](https://msdn.microsoft.com/library/windowsazure/ee336279) (MSDN).
- [Diretrizes e limitações (banco de dados SQL do Windows Azure)](https://msdn.microsoft.com/library/windowsazure/ff394102.aspx)

<a id="nosql"></a>

## <a name="working-with-nosql-database-management-systems"></a>Trabalhando com sistemas de gerenciamento de banco de dados NoSQL

- [Serviços de dados do Windows Azure](https://www.windowsazure.com/develop/net/data/) (Microsoft Azure site). Consulte o [Guia de recursos do serviço tabela](https://docs.microsoft.com/azure/) e a seção **Big data** da página.
- [ASP.NET aplicativo de várias camadas usando tabelas de armazenamento, filas e blobs](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36) (Microsoft Azure site). Tutorial de ponta a ponta com aplicativo de exemplo para download que usa tabelas NoSQL de armazenamento do Windows Azure.

<a id="linq"></a>

## <a name="using-linq-queries-in-aspnet-applications"></a>Usando consultas LINQ em aplicativos ASP.NET

- [Opções de acesso a dados do ASP.net](https://msdn.microsoft.com/library/ms178359.aspx#linq) (MSDN). Inclui uma introdução ao LINQ.
- [Vídeos de treinamento do LINQ](http://www.misfitgeek.com/windows-client-linq-training-videos-20/) (blog de Joe Stagner).
- [ASP.net o thread do fórum com links para recursos LINQ dinâmicos](https://forums.asp.net/p/1961037/5601994.aspx?Please+suggest+good+books+so+that+one+can+write+and+understand+dynamic+linq).

<a id="dd"></a>

## <a name="using-dynamic-data-scaffolding"></a>Usando Dados Dinâmicos scaffolding

- [Dados dinâmicos modelos de projeto](https://msdn.microsoft.com/library/jj822927.aspx#dynamicdata) (MSDN). Orientação sobre quando usar Dados Dinâmicos projetos.
- [ASP.NET dados dinâmicos](https://msdn.microsoft.com/library/ee845452.aspx) (MSDN).

<a id="securing"></a>

## <a name="securing-data-access"></a>Protegendo o acesso a dados

- [Protegendo o acesso a dados no ASP.net](https://msdn.microsoft.com/library/ms178375.aspx) (MSDN).
- [Considerações sobre segurança (Entity Framework)](https://msdn.microsoft.com/library/cc716760.aspx) (MSDN).
- [Como: proteger cadeias de conexão ao usar](https://msdn.microsoft.com/library/ms178372.aspx) o MSDN (controles de fonte de dados).

<a id="optimizingdataaccess"></a>

## <a name="optimizing-data-access-performance"></a>Otimizando o desempenho de acesso a dados

- [Visão geral do desempenho do ASP.net](https://msdn.microsoft.com/library/cc668225.aspx) (MSDN).
- [ASP.net Caching](https://msdn.microsoft.com/library/xsbfdd8c.aspx) (MSDN).
- [Melhorando o desempenho do ASP.net](https://msdn.microsoft.com/library/ff647787) (MSDN). Há um aviso de "conteúdo desativado" na parte superior desta página, mas a maioria das informações ainda é relevante e não há nenhum recurso atualizado comparável.
- [Melhorando o desempenho do SQL Server](https://msdn.microsoft.com/library/ff647793) (MSDN). Mesmo comentário que o link anterior.

Consulte também [otimizando o desempenho do Entity Framework](#optimizingef) anteriormente neste tópico.

<a id="deploying"></a>

## <a name="deploying-a-database"></a>Implantando um banco de dados

- [Implantação da Web do ASP.NET – recursos recomendados](aspnet-web-deployment-content-map.md) (MSDN).

<a id="webservice"></a>

## <a name="accessing-data-through-a-web-service"></a>Acessando dados por meio de um serviço Web

- [Acessando dados por meio de um serviço Web](https://msdn.microsoft.com/library/ms178359.aspx#webservice) (MSDN). Orientação sobre quando usar a API Web versus o WCF.
- [Introdução com ASP.NET Web API](../web-api/index.md).
- [WCF Data Services](https://msdn.microsoft.com/data/bb931106) (MSDN).

<a id="additional"></a>

## <a name="additional-resources"></a>Recursos adicionais

- [Perguntas frequentes sobre o acesso a dados do ASP.net](https://msdn.microsoft.com/library/jj653753.aspx) (MSDN).
- [ASP.NET Web Forms tutoriais-dados](../web-forms/overview/data-access/index.md). A maioria desses tutoriais é relativamente antiga; Certifique-se de ler  [Opções de acesso a dados do ASP.net](https://msdn.microsoft.com/library/ms178359.aspx) e [Opções de armazenamento de dados (criando Real-World aplicativos de nuvem com o Windows Azure)](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options.md) primeiro para que você não tenha um método de acesso a dados que não seja adequado para seu cenário.
- [Mapa de conteúdo do ASP.NET MVC](../mvc/overview/getting-started/recommended-resources-for-mvc.md).
- [Tutoriais de páginas da Web do ASP.net-dados](../web-pages/overview/data/index.md).
- [Acessando dados no Visual Studio](https://msdn.microsoft.com/library/wzabh8c4.aspx) (MSDN). Fornece uma lista de links semelhantes a este mapa de conteúdo, mas com foco no Visual Studio em vez de ASP.NET.
