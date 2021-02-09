---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application
title: Cenários de Entity Framework avançados para um aplicativo Web MVC (10 de 10) | Microsoft Docs
author: tdykstra
description: O aplicativo Web de exemplo da Contoso University demonstra como criar aplicativos ASP.NET MVC 4 usando o Entity Framework 5 Code First e o Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 64906a1d-f734-41cf-9615-ee95f8740996
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application
msc.type: authoredcontent
ms.openlocfilehash: 4b49ae0bee2a5ceb41dbf040dc6ea7c8d2d0dea2
ms.sourcegitcommit: b4cdcf246850751579e45da80c9780fe56330dd0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/09/2021
ms.locfileid: "99985109"
---
# <a name="advanced-entity-framework-scenarios-for-an-mvc-web-application-10-of-10"></a>Cenários de Entity Framework avançados para um aplicativo Web MVC (10 de 10)

por [Tom Dykstra](https://github.com/tdykstra)

> O aplicativo Web de exemplo da Contoso University demonstra como criar aplicativos ASP.NET MVC 4 usando o Entity Framework 5 Code First e o Visual Studio 2012. Para obter informações sobre a série de tutoriais, consulte [o primeiro tutorial da série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
> 
> > [!NOTE] 
> > 
> > Se você encontrar um problema que não possa resolver, [Baixe o capítulo concluído](building-the-ef5-mvc4-chapter-downloads.md) e tente reproduzir o problema. Em geral, você pode encontrar a solução para o problema comparando seu código com o código concluído. Para alguns erros comuns e como resolvê-los, consulte [erros e soluções alternativas.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)

No tutorial anterior, você implementou o repositório e os padrões de unidade de trabalho. Este tutorial aborda os seguintes tópicos:

- Executando consultas SQL brutas.
- Executando consultas sem controle.
- Examinando consultas enviadas ao banco de dados.
- Trabalhando com classes de proxy.
- Desabilitando a detecção automática de alterações.
- Desabilitando a validação ao salvar as alterações.
- [Erros e contornará a solução](#errors)

Para a maioria desses tópicos, você trabalhará com páginas que já criou. Para usar o SQL bruto para fazer atualizações em massa, você criará uma nova página que atualiza o número de créditos de todos os cursos no banco de dados:

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image1.png)

E para usar uma consulta sem rastreamento, você adicionará uma nova lógica de validação à página de edição do departamento:

![Department_Edit_page_with_duplicate_administrator_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image2.png)

## <a name="performing-raw-sql-queries"></a>Executando consultas SQL brutas

A API de Code First de Entity Framework inclui métodos que permitem passar comandos SQL diretamente para o banco de dados. Você tem as seguintes opções:

- Use o método `DbSet.SqlQuery` para consultas que retornam tipos de entidade. Os objetos retornados devem ser do tipo esperado pelo `DbSet` objeto e são rastreados automaticamente pelo contexto do banco de dados, a menos que você desative o rastreamento. (Consulte a seção a seguir sobre o `AsNoTracking` método.)
- Use o `Database.SqlQuery` método para consultas que retornam tipos que não são entidades. Os dados retornados não são controlados pelo contexto de banco de dados, mesmo se esse método é usado para recuperar tipos de entidade.
- Use o [Database.ExecuteSqlCommand](https://msdn.microsoft.com/library/gg679456(v=vs.103).aspx) para comandos que não são de consulta.

Uma das vantagens de usar o Entity Framework é que ele evita vincular o código de forma muito próxima a um método específico de armazenamento de dados. Ele faz isso pela geração de consultas SQL e comandos para você, que também libera você da necessidade de escrevê-los. Mas há cenários excepcionais quando você precisa executar consultas SQL específicas que você criou manualmente, e esses métodos possibilitam que você manipule essas exceções.

Como é sempre verdadeiro quando você executa comandos SQL em um aplicativo Web, é necessário tomar precauções para proteger o site contra ataques de injeção de SQL. Uma maneira de fazer isso é usar consultas parametrizadas para garantir que as cadeias de caracteres enviadas por uma página da Web não possam ser interpretadas como comandos SQL. Neste tutorial, você usará consultas parametrizadas ao integrar a entrada do usuário a uma consulta.

### <a name="calling-a-query-that-returns-entities"></a>Chamando uma consulta que retorna entidades

Suponha que você queira `GenericRepository` que a classe forneça mais flexibilidade de filtragem e classificação sem exigir que você crie uma classe derivada com métodos adicionais. Uma maneira de conseguir isso seria adicionar um método que aceita uma consulta SQL. Em seguida, você poderia especificar qualquer tipo de filtragem ou classificação que desejar no controlador, como uma `Where` cláusula que depende de junções ou de uma subconsulta. Nesta seção, você verá como implementar esse tipo de método.

Crie o `GetWithRawSql` método adicionando o seguinte código a *GenericRepository.cs*:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample1.cs)]

No *CourseController.cs*, chame o método New do `Details` método, conforme mostrado no exemplo a seguir:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample2.cs)]

Nesse caso, você poderia ter usado o `GetByID` método, mas está usando o `GetWithRawSql` método para verificar se o `GetWithRawSQL` método funciona.

Execute a página de detalhes para verificar se a consulta SELECT funciona (selecione a guia **curso** e **detalhes** de um curso).

![Course_Details_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image3.png)

### <a name="calling-a-query-that-returns-other-types-of-objects"></a>Chamando uma consulta que retorna outros tipos de objetos

Anteriormente, você criou uma grade de estatísticas de alunos para a página Sobre que mostrava o número de alunos para cada data de registro. O código que faz isso no *HomeController.cs* usa LINQ:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample3.cs)]

Suponha que você queira escrever o código que recupera esses dados diretamente no SQL em vez de usar o LINQ. Para fazer isso, você precisa executar uma consulta que retorne algo diferente de objetos de entidade, o que significa que você precisa usar o `Database.SqlQuery` método.

No *HomeController.cs*, substitua a instrução LINQ no `About` método pelo seguinte código:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample4.cs)]

Execute a página sobre. Ela exibe os mesmos dados que antes.

![About_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image4.png)

### <a name="calling-an-update-query"></a>Chamando uma consulta Update

Suponha que os administradores da Contoso University desejam poder executar alterações em massa no banco de dados, como alterar o número de créditos de cada curso. Se a universidade tiver uma grande quantidade de cursos, poderá ser ineficiente recuperá-los como entidades e alterá-los individualmente. Nesta seção, você implementará uma página da Web que permite ao usuário especificar um fator pelo qual alterar o número de créditos de todos os cursos e fará a alteração executando uma `UPDATE` instrução SQL. A página da Web será semelhante à seguinte ilustração:

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image5.png)

No tutorial anterior, você usou o repositório genérico para ler e atualizar `Course` entidades no `Course` controlador. Para essa operação de atualização em massa, você precisa criar um novo método de repositório que não esteja no repositório genérico. Para fazer isso, você criará uma `CourseRepository` classe dedicada que deriva da `GenericRepository` classe.

Na pasta *Dal* , crie *CourseRepository.cs* e substitua o código existente pelo código a seguir:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample5.cs)]

Em *UnitOfWork.cs*, altere o `Course` tipo de repositório de `GenericRepository<Course>` para `CourseRepository:`

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample6.cs)]

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample7.cs)]

Em *CourseController.cs*, adicione um `UpdateCourseCredits` método:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample8.cs)]

Esse método será usado para `HttpGet` e `HttpPost` . Quando o `HttpGet` `UpdateCourseCredits` método for executado, a `multiplier` variável será nula e a exibição exibirá uma caixa de texto vazia e um botão enviar, conforme mostrado na ilustração anterior.

Quando o botão de **atualização** for clicado e o `HttpPost` método for executado, o `multiplier` valor será inserido na caixa de texto. Em seguida, o código chama o `UpdateCourseCredits` método Repository, que retorna o número de linhas afetadas, e esse valor é armazenado no `ViewBag` objeto. Quando o modo de exibição recebe o número de linhas afetadas no `ViewBag` objeto, ele exibe esse número em vez da caixa de texto e o botão enviar, conforme mostrado na ilustração a seguir:

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image6.png)

Crie uma exibição na pasta *Views\Course* da página créditos do curso de atualização:

![Add_View_dialog_box_for_Update_Course_Credits](https://asp.net/media/2578203/Windows-Live-Writer_Advanced-Entity-Framework-Scenarios-for-_CEF8_Add_View_dialog_box_for_Update_Course_Credits.png)

No *Views\Course\UpdateCourseCredits.cshtml*, substitua o código do modelo pelo código a seguir:

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample9.cshtml)]

Execute o método `UpdateCourseCredits` selecionando a guia **Cursos**, adicionando, em seguida, "/UpdateCourseCredits" ao final da URL na barra de endereços do navegador (por exemplo: `http://localhost:50205/Course/UpdateCourseCredits`). Insira um número na caixa de texto:

![Update_Course_Credits_initial_page_with_2_entered](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image7.png)

Clique em **Atualizar**. O número de linhas afetadas é exibido:

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image8.png)

Clique em **Voltar para a Lista** para ver a lista de cursos com o número revisado de créditos.

![Courses_Index_page_showing_revised_credits](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image9.png)

Para obter mais informações sobre consultas SQL brutas, consulte [consultas SQL brutas](https://blogs.msdn.com/b/adonet/archive/2011/02/04/using-dbcontext-in-ef-feature-ctp5-part-10-raw-sql-queries.aspx) no blog da equipe do Entity Framework.

## <a name="no-tracking-queries"></a>Consultas sem acompanhamento

Quando um contexto de banco de dados recupera linhas de banco de dados e cria objetos de entidade que os representam, por padrão, ele controla se as entidades na memória estão sincronizadas com o que há no banco de dados. Os dados em memória atuam como um cache e são usados quando uma entidade é atualizada. Esse cache costuma ser desnecessário em um aplicativo Web porque as instâncias de contexto são normalmente de curta duração (uma nova é criada e descartada para cada solicitação) e o contexto que lê uma entidade normalmente é descartado antes que essa entidade seja usada novamente.

Você pode especificar se o contexto rastreia objetos de entidade para uma consulta usando o `AsNoTracking` método. Os cenários típicos em que talvez você deseje fazer isso incluem os seguintes:

- A consulta recupera um grande volume de dados que desativar o controle pode melhorar o desempenho de forma perceptível.
- Você deseja anexar uma entidade para atualizá-la, mas anteriormente você recuperou a mesma entidade para uma finalidade diferente. Como a entidade já está sendo controlada pelo contexto de banco de dados, não é possível anexar a entidade que você deseja alterar. Uma maneira de evitar que isso aconteça é usar a `AsNoTracking` opção com a consulta anterior.

Nesta seção, você implementará a lógica de negócios que ilustra o segundo desses cenários. Especificamente, você imporá uma regra de negócios que diz que um instrutor não pode ser o administrador de mais de um departamento.

No *DepartmentController.cs*, adicione um novo método que você pode chamar dos `Edit` métodos e `Create` para garantir que dois departamentos tenham o mesmo administrador:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample10.cs)]

Adicione o código no `try` bloco do `HttpPost` `Edit` método para chamar esse novo método se não houver nenhum erro de validação. O `try` bloco agora é semelhante ao exemplo a seguir:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample11.cs?highlight=9-12)]

Execute a página de edição do departamento e tente alterar o administrador de um departamento para um instrutor que já seja o administrador de um departamento diferente. Você Obtém a mensagem de erro esperada:

![Department_Edit_page_with_duplicate_administrator_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image10.png)

Agora, execute a página de edição do departamento novamente e, desta vez, altere o valor do **orçamento** . Ao clicar em **salvar**, você verá uma página de erro:

![Department_Edit_page_with_object_state_manager_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image11.png)

A mensagem de erro de exceção é " `An object with the same key already exists in the ObjectStateManager. The ObjectStateManager cannot track multiple objects with the same key.` " isso ocorreu devido à seguinte sequência de eventos:

- O `Edit` método chama o `ValidateOneAdministratorAssignmentPerInstructor` método, que recupera todos os departamentos que têm Kim Abercrombie como administrador. Isso faz com que o departamento em inglês seja lido. Como esse é o departamento que está sendo editado, nenhum erro é relatado. Como resultado dessa operação de leitura, no entanto, a entidade do departamento em inglês que foi lida do banco de dados agora está sendo controlada pelo contexto do banco de dados.
- O `Edit` método tenta definir o `Modified` sinalizador na entidade de departamento em inglês criado pelo associador de modelo MVC, mas isso falha porque o contexto já está acompanhando uma entidade para o departamento em inglês.

Uma solução para esse problema é impedir que o contexto rastreie entidades de departamento na memória recuperadas pela consulta de validação. Não há nenhuma desvantagem de fazer isso, pois você não atualizará essa entidade nem a lerá novamente de uma maneira que se beneficiaria dela ser armazenada em cache na memória.

Em *DepartmentController.cs*, no `ValidateOneAdministratorAssignmentPerInstructor` método, não especifique nenhum controle, conforme mostrado a seguir:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample12.cs?highlight=4)]

Repita a tentativa de editar o valor de **orçamento** de um departamento. Desta vez, a operação é bem-sucedida e o site retorna como esperado para a página de índice de departamentos, mostrando o valor de orçamento revisado.

## <a name="examining-queries-sent-to-the-database"></a>Examinando consultas enviadas ao banco de dados

Às vezes, é útil poder ver as consultas SQL reais que são enviadas ao banco de dados. Para fazer isso, você pode examinar uma variável de consulta no depurador ou chamar o método da consulta `ToString` . Para testar isso, você examinará uma consulta simples e examinará o que acontece com ela à medida que adiciona opções, como carregamento, filtragem e classificação adiantados.

Em *controladores/CourseController*, substitua o `Index` método pelo código a seguir:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample13.cs?highlight=3)]

Agora, defina um ponto de interrupção em *GenericRepository.cs* no `return query.ToList();` e as `return orderBy(query).ToList();` instruções do `Get` método. Execute o projeto no modo de depuração e selecione a página de índice do curso. Quando o código atingir o ponto de interrupção, examine a `query` variável. Você vê a consulta que é enviada para SQL Server. É uma instrução simples `Select` :

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample14.sql)]

![](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image12.png)

As consultas podem ser muito longas para serem exibidas nas janelas de depuração no Visual Studio. Para ver a consulta inteira, você pode copiar o valor da variável e colá-lo em um editor de texto:

![Copy_value_of_variable_in_debug_mode](https://asp.net/media/2578239/Windows-Live-Writer_Advanced-Entity-Framework-Scenarios-for-_CEF8_Copy_value_of_variable_in_debug_mode_0902a2b1-b799-47a6-9b4b-f266c79d83c1.png)

Agora você adicionará uma lista suspensa à página de índice do curso para que os usuários possam filtrar um departamento específico. Você classificará os cursos por título e especificará o carregamento adiantado para a `Department` propriedade de navegação. No *CourseController.cs*, substitua o `Index` método pelo código a seguir:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample15.cs)]

O método recebe o valor selecionado da lista suspensa no `SelectedDepartment` parâmetro. Se nada estiver selecionado, esse parâmetro será NULL.

Uma `SelectList` coleção que contém todos os departamentos é passada para a exibição da lista suspensa. Os parâmetros passados para o `SelectList` Construtor especificam o nome do campo de valor, o nome do campo de texto e o item selecionado.

Para o `Get` método do `Course` repositório, o código especifica uma expressão de filtro, uma ordem de classificação e carregamento adiantado para a `Department` propriedade de navegação. A expressão de filtro sempre retorna `true` se nada estiver selecionado na lista suspensa (ou seja, `SelectedDepartment` for nulo).

No *Views\Course\Index.cshtml*, imediatamente antes da marca de abertura `table` , adicione o seguinte código para criar a lista suspensa e um botão enviar:

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample16.cshtml)]

Com os pontos de interrupção ainda definidos na `GenericRepository` classe, execute a página de índice do curso. Continue nas duas primeiras vezes que o código atinge um ponto de interrupção, para que a página seja exibida no navegador. Selecione um departamento na lista suspensa e clique em **Filtrar**:

![Course_Index_page_with_department_selected](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image13.png)

Desta vez, o primeiro ponto de interrupção será para a consulta de departamentos para a lista suspensa. Pule e exiba a `query` variável na próxima vez em que o código atingir o ponto de interrupção para ver a `Course` aparência da consulta agora. Você verá algo semelhante ao seguinte:

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample17.sql)]

Você pode ver que a consulta agora é uma `JOIN` consulta que carrega `Department` dados junto com os `Course` dados e que ele inclui uma `WHERE` cláusula.

<a id="proxyclasses"></a>

## <a name="working-with-proxy-classes"></a>Trabalhando com classes de proxy

Quando o Entity Framework cria instâncias de entidade (por exemplo, quando você executa uma consulta), ele geralmente as cria como instâncias de um tipo derivado gerado dinamicamente que atua como um proxy para a entidade. Esse proxy substitui algumas propriedades virtuais da entidade para inserir ganchos para executar ações automaticamente quando a propriedade é acessada. Por exemplo, esse mecanismo é usado para dar suporte ao carregamento lento de relações.

Na maioria das vezes, você não precisa estar ciente desse uso de proxies, mas há exceções:

- Em alguns cenários, talvez você queira impedir que o Entity Framework Crie instâncias de proxy. Por exemplo, a serialização de instâncias não proxy pode ser mais eficiente do que a serialização de instâncias de proxy.
- Ao instanciar uma classe de entidade usando o `new` operador, você não obtém uma instância de proxy. Isso significa que você não obtém funcionalidade como carregamento lento e controle de alterações automático. Normalmente, isso é ok; Geralmente, você não precisa de carregamento lento, pois você está criando uma nova entidade que não está no banco de dados e, em geral, não precisa de controle de alterações se estiver marcando explicitamente a entidade como `Added` . No entanto, se você precisar de carregamento lento e precisar de controle de alterações, poderá criar novas instâncias de entidade com proxies usando o `Create` método da `DbSet` classe.
- Talvez você queira obter um tipo de entidade real de um tipo de proxy. Você pode usar o `GetObjectType` método da `ObjectContext` classe para obter o tipo de entidade real de uma instância de tipo de proxy.

Para obter mais informações, consulte [trabalhando com proxies](https://blogs.msdn.com/b/adonet/archive/2011/02/02/using-dbcontext-in-ef-feature-ctp5-part-8-working-with-proxies.aspx) no blog da equipe do Entity Framework.

## <a name="disabling-automatic-detection-of-changes"></a>Desabilitando a detecção automática de alterações

O Entity Framework determina como uma entidade foi alterada (e, portanto, quais atualizações precisam ser enviadas ao banco de dados), comparando os valores atuais de uma entidade com os valores originais. Os valores originais são armazenados quando a entidade foi consultada ou anexada. Alguns dos métodos que causam a detecção automática de alterações são os seguintes:

- `DbSet.Find`
- `DbSet.Local`
- `DbSet.Remove`
- `DbSet.Add`
- `DbSet.Attach`
- `DbContext.SaveChanges`
- `DbContext.GetValidationErrors`
- `DbContext.Entry`
- `DbChangeTracker.Entries`

Se você estiver acompanhando um grande número de entidades e chamar um desses métodos muitas vezes em um loop, poderá obter melhorias de desempenho significativas temporariamente desativando a detecção automática de alterações usando a propriedade [AutoDetectChangesEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.autodetectchangesenabled(VS.103).aspx) . Para obter mais informações, consulte [detectando alterações automaticamente](https://blogs.msdn.com/b/adonet/archive/2011/02/06/using-dbcontext-in-ef-feature-ctp5-part-12-automatically-detecting-changes.aspx).

## <a name="disabling-validation-when-saving-changes"></a>Desabilitando a validação ao salvar as alterações

Quando você chamar o `SaveChanges` método, por padrão, o Entity Framework validará os dados em todas as propriedades de todas as entidades alteradas antes de atualizar o banco de dados. Se você atualizou um grande número de entidades e já validou os dados, esse trabalho é desnecessário e você pode fazer com que o processo de salvar as alterações demore menos tempo desligando temporariamente a validação. Você pode fazer isso usando a propriedade [ValidateOnSaveEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.validateonsaveenabled(VS.103).aspx) . Para obter mais informações, consulte [validação](https://blogs.msdn.com/b/adonet/archive/2010/12/15/ef-feature-ctp5-validation.aspx).

## <a name="summary"></a>Resumo

Isso conclui esta série de tutoriais sobre como usar o Entity Framework em um aplicativo MVC ASP.NET. Links para outros recursos de Entity Framework podem ser encontrados no [mapa de conteúdo de acesso a dados do ASP.net](../../../../whitepapers/aspnet-data-access-content-map.md).

Para obter mais informações sobre como implantar seu aplicativo Web depois de tê-lo criado, consulte [mapa de conteúdo de implantação do ASP.net](https://msdn.microsoft.com/library/bb386521.aspx) na biblioteca do MSDN.

Para obter informações sobre outros tópicos relacionados ao MVC, como autenticação e autorização, consulte os [recursos recomendados pelo MVC](../../getting-started/recommended-resources-for-mvc.md).

<a id="acknowledgments"></a>

## <a name="acknowledgments"></a>Agradecimentos

- Tom Dykstra escreveu a versão original deste tutorial e é um escritor de programação sênior na equipe de conteúdo da Microsoft Web Platform e Tools.
- [Rick Anderson](https://blogs.msdn.com/b/rickandy/) (Twitter [@RickAndMSFT](http://twitter.com/RickAndMSFT) ) coautoria este tutorial e fez a maior parte do trabalho atualizando-o para o EF 5 e o MVC 4. Rick é um escritor de programação sênior para a Microsoft focado no Azure e no MVC.
- A [Rowan Miller](http://www.romiller.com) e outros membros da equipe de Entity Framework assistida pelas revisões de código e ajudaram a depurar muitos problemas com as migrações que surgiram enquanto atualizamos o tutorial para o EF 5.

## <a name="vb"></a>VB

Quando o tutorial foi produzido originalmente, fornecemos as versões C# e VB do projeto de download concluído. Com essa atualização, estamos fornecendo um projeto de download para C# para cada capítulo para facilitar a introdução em qualquer lugar da série, mas devido a limitações de tempo e outras prioridades, não fazemos isso para o VB. Se você criar um projeto do VB usando esses tutoriais e estiver disposto a compartilhá-lo com outras pessoas, informe-nos.

<a id="errors"></a>

## <a name="errors-and-workarounds"></a>Erros e soluções alternativas

### <a name="cannot-createshadow-copy"></a>Não é possível criar/copiar sombra

Mensagem de erro:

*Não é possível criar/copiar a cópia ' DotNetOpenAuth. OpenId ' quando esse arquivo já existe.*

Solução:

Aguarde alguns segundos e atualize a página.

### <a name="update-database-not-recognized"></a>Update-Database não reconhecido

Mensagem de erro:

*O termo ' Update-Database ' não é reconhecido como o nome de um cmdlet, função, arquivo de script ou programa operável. Verifique a ortografia do nome ou, se um caminho foi incluído, verifique se o caminho está correto e tente novamente.* (A partir do *`Update-Database`* comando no PMC.)

Solução:

Saia do Visual Studio. Reabra o projeto e tente novamente.

### <a name="validation-failed"></a>Falha na validação

Mensagem de erro:

*Falha na validação de uma ou mais entidades. Consulte a propriedade ' EntityValidationErrors ' para obter mais detalhes.* (A partir do *`Update-Database`* comando no PMC.)

Solução:

Uma causa desse problema é erros de validação quando o `Seed` método é executado. Consulte [bancos de Entity Framework de propagação e depuração (EF)](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) para obter dicas sobre como depurar o `Seed` método.

### <a name="http-50019-error"></a>Erro HTTP 500,19

Mensagem de erro:

*Erro HTTP 500,19-erro interno do servidor a página solicitada não pode ser acessada porque os dados de configuração relacionados para a página são inválidos.*

Solução:

Uma maneira de obter esse erro é ter várias cópias da solução, cada uma delas usando o mesmo número de porta. Normalmente, você pode resolver esse problema saindo de todas as instâncias do Visual Studio e, em seguida, reiniciando o projeto no qual está trabalhando. Se isso não funcionar, tente alterar o número da porta. Clique com o botão direito do mouse no arquivo de projeto e clique em Propriedades. Selecione a guia **Web** e, em seguida, altere o número da porta na caixa de texto **URL do projeto** .

### <a name="error-locating-sql-server-instance"></a>Erro ao localizar a instância do SQL Server

Mensagem de erro:

*Ocorreu um erro relacionado à rede ou específico à instância ao estabelecer uma conexão com SQL Server. O servidor não foi encontrado ou não estava acessível. Verifique se o nome da instância está correto e se SQL Server está configurado para permitir conexões remotas. (provedor: interfaces de rede do SQL, erro: 26-erro ao localizar o servidor/instância especificado)*

Solução:

Verifique a cadeia de conexão. Se você tiver excluído o banco de dados manualmente, altere o nome do banco de dados na cadeia de caracteres de construção.

> [!div class="step-by-step"]
> [Anterior](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md) 
>  [Avançar](building-the-ef5-mvc4-chapter-downloads.md)
