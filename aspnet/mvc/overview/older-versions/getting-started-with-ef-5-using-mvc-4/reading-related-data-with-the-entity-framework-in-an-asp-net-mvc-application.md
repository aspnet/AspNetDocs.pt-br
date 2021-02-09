---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: Lendo dados relacionados com o Entity Framework em um aplicativo MVC ASP.NET (5 de 10) | Microsoft Docs
author: tdykstra
description: O aplicativo Web de exemplo da Contoso University demonstra como criar aplicativos ASP.NET MVC 4 usando o Entity Framework 5 Code First e o Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 0d6fb83b-71f7-425d-8dec-981197d7ec42
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 6a3a4978a718ba56446463373ad3525e8b6f0d4a
ms.sourcegitcommit: b4cdcf246850751579e45da80c9780fe56330dd0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/09/2021
ms.locfileid: "99984830"
---
# <a name="reading-related-data-with-the-entity-framework-in-an-aspnet-mvc-application-5-of-10"></a>Lendo dados relacionados com o Entity Framework em um aplicativo MVC ASP.NET (5 de 10)

por [Tom Dykstra](https://github.com/tdykstra)

> O aplicativo Web de exemplo da Contoso University demonstra como criar aplicativos ASP.NET MVC 4 usando o Entity Framework 5 Code First e o Visual Studio 2012. Para obter informações sobre a série de tutoriais, consulte [o primeiro tutorial da série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
> 
> > [!NOTE] 
> > 
> > Se você encontrar um problema que não possa resolver, [Baixe o capítulo concluído](building-the-ef5-mvc4-chapter-downloads.md) e tente reproduzir o problema. Em geral, você pode encontrar a solução para o problema comparando seu código com o código concluído. Para alguns erros comuns e como resolvê-los, consulte [erros e soluções alternativas.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)

No tutorial anterior, você concluiu o modelo de dados da escola. Neste tutorial, você lerá e exibirá os dados relacionados, ou seja, os dados que o Entity Framework carrega nas propriedades de navegação.

As ilustrações a seguir mostram as páginas com as quais você trabalhará.

![](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="lazy-eager-and-explicit-loading-of-related-data"></a>Carregamento lento, adiantado e explícito de dados relacionados

Há várias maneiras pelas quais o Entity Framework pode carregar dados relacionados nas propriedades de navegação de uma entidade:

- *Carregamento lento*. Quando a entidade é lida pela primeira vez, os dados relacionados não são recuperados. No entanto, na primeira vez que você tenta acessar uma propriedade de navegação, os dados necessários para essa propriedade de navegação são recuperados automaticamente. Isso resulta em várias consultas enviadas ao banco de dados — uma para a própria entidade e uma a cada vez que os dados relacionados para a entidade devem ser recuperados. 

    ![Lazy_loading_example](https://asp.net/media/2577850/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Lazy_loading_example_2c44eabb-5fd3-485a-837d-8e3d053f2c0c.png)
- *Carregamento adiantado*. Quando a entidade é lida, os dados relacionados são recuperados com ela. Normalmente, isso resulta em uma única consulta de junção que recupera todos os dados necessários. Você especifica o carregamento adiantado usando o `Include` método.

    ![Eager_loading_example](https://asp.net/media/2577856/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Eager_loading_example_33f907ff-f0b0-4057-8e75-05a8cacac807.png)
- *Carregamento explícito*. Isso é semelhante ao carregamento lento, exceto que você recupera explicitamente os dados relacionados no código; Ele não acontece automaticamente quando você acessa uma propriedade de navegação. Você carrega dados relacionados manualmente obtendo a entrada do Gerenciador de estado do objeto para uma entidade e chamando o `Collection.Load` método para coleções ou o `Reference.Load` método para propriedades que mantêm uma única entidade. (No exemplo a seguir, se você quisesse carregar a propriedade de navegação do administrador, você substituirá `Collection(x => x.Courses)` por `Reference(x => x.Administrator)` .)

    ![Explicit_loading_example](https://asp.net/media/2577862/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Explicit_loading_example_79d8c368-6d82-426f-be9a-2b443644ab15.png)

Como eles não recuperam imediatamente os valores de propriedade, o carregamento lento e o carregamento explícito também são conhecidos como *carregamento adiado*.

Em geral, se você souber que precisa de dados relacionados para cada entidade recuperada, o carregamento adiantado oferece o melhor desempenho, porque uma única consulta enviada ao banco de dados é normalmente mais eficiente do que consultas separadas para cada entidade recuperada. Por exemplo, nos exemplos acima, suponha que cada departamento tenha dez cursos relacionados. O exemplo de carregamento adiantado resultaria em apenas uma única consulta (junção) e uma única viagem de ida e volta ao banco de dados. Os exemplos de carregamento lento e carregamento explícito resultariam em onze consultas e onze viagens de ida e volta ao banco de dados. As viagens de ida e volta extras para o banco de dados são especialmente prejudiciais ao desempenho quando a latência é alta.

Por outro lado, em alguns cenários, o carregamento lento é mais eficiente. O carregamento adiantado pode fazer com que uma junção muito complexa seja gerada, o que SQL Server não pode processar com eficiência. Ou, se você precisar acessar as propriedades de navegação de uma entidade somente para um subconjunto de um conjunto de entidades que está processando, o carregamento lento poderá ser melhor executado porque o carregamento adiantado recuperaria mais dados do que o necessário. Se o desempenho for crítico, será melhor testar o desempenho das duas maneiras para fazer a melhor escolha.

Normalmente, você usaria o carregamento explícito somente quando você ativou o carregamento lento. Um cenário quando você deve desativar o carregamento lento é durante a serialização. O carregamento e a serialização lentos não são bem misturados e, se você não tiver cuidado, poderá acabar consultando significativamente mais dados do que o esperado quando o carregamento lento estiver habilitado. A serialização geralmente funciona acessando cada propriedade em uma instância de um tipo. O acesso à propriedade dispara o carregamento lento e essas entidades carregadas lentas são serializadas. Em seguida, o processo de serialização acessa cada propriedade das entidades carregadas com o Lazy, potencialmente causando um carregamento e uma serialização mais lentos. Para evitar essa reação da cadeia de execução, desative o carregamento lento antes de serializar uma entidade.

A classe de contexto do banco de dados executa o carregamento lento por padrão. Há duas maneiras de desabilitar o carregamento lento:

- Para propriedades de navegação específicas, omita a `virtual` palavra-chave ao declarar a propriedade.
- Para todas as propriedades de navegação, defina `LazyLoadingEnabled` como `false` . Por exemplo, você pode colocar o seguinte código no construtor da sua classe de contexto: 

    [!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

O carregamento lento pode mascarar o código que causa problemas de desempenho. Por exemplo, o código que não especifica carregamento adiantado ou explícito, mas processa um alto volume de entidades e usa várias propriedades de navegação em cada iteração pode ser muito ineficiente (devido a muitas viagens de ida e volta ao banco de dados). Um aplicativo que executa bem no desenvolvimento usando um SQL Server local pode ter problemas de desempenho quando movido para o banco de dados SQL do Azure devido à latência aumentada e ao carregamento lento. A criação de perfil de consultas de banco de dados com uma carga de teste realista ajudará você a determinar se o carregamento lento é apropriado. Para obter mais informações, consulte [Desmistificando estratégias de Entity Framework: carregando dados relacionados](https://msdn.microsoft.com/magazine/hh205756.aspx) e [usando o Entity Framework para reduzir a latência de rede para SQL Azure](https://msdn.microsoft.com/magazine/gg309181.aspx).

## <a name="create-a-courses-index-page-that-displays-department-name"></a>Página criar um índice de cursos que exibe o nome do departamento

A `Course` entidade inclui uma propriedade de navegação que contém a `Department` entidade do departamento à qual o curso é atribuído. Para exibir o nome do departamento atribuído em uma lista de cursos, você precisa obter a `Name` propriedade da `Department` entidade que está na `Course.Department` propriedade de navegação.

Crie um controlador chamado `CourseController` para o `Course` tipo de entidade usando as mesmas opções que você fez anteriormente para o `Student` controlador, conforme mostrado na ilustração a seguir (exceto ao contrário da imagem, sua classe de contexto está no namespace Dal, não no namespace Models):

![Add_Controller_dialog_box_for_Course_controller](https://asp.net/media/2577868/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Add_Controller_dialog_box_for_Course_controller_c167c11e-2d3e-4b64-a2b9-a0b368b8041d.png)

Abra *Controllers\CourseController.cs* e examine o `Index` método:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

O scaffolding automático especificou o carregamento adiantado para a propriedade de navegação `Department` usando o método `Include`.

Abra *Views\Course\Index.cshtml* e substitua o código existente pelo código a seguir. As alterações são realçadas:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=4,15,18,28-30)]

Você fez as seguintes alterações no código gerado por scaffolding:

- Alterado o título de **índice** para **cursos**.
- Moveu os links de linha para a esquerda.
- Adicionada uma coluna sob o **número** de título que mostra o `CourseID` valor da propriedade. (Por padrão, as chaves primárias não são com Scaffold porque normalmente não têm significado para os usuários finais. No entanto, nesse caso, a chave primária é significativa e você deseja mostrá-la.)
- Alterado o título da última coluna de **DepartmentID** (o nome da chave estrangeira para a `Department` entidade) para **Department**.

Observe que, para a última coluna, o código com Scaffold exibe a `Name` propriedade da `Department` entidade que é carregada na `Department` propriedade de navegação:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml)]

Execute a página (selecione a guia **cursos** no Home Page da Contoso University) para ver a lista com nomes de departamento.

![Courses_index_page_with_department_names](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="create-an-instructors-index-page-that-shows-courses-and-enrollments"></a>Página de índice criar um instrutor que mostra cursos e registros

Nesta seção, você criará um controlador e uma exibição para a `Instructor` entidade a fim de exibir a página de índice dos instrutores:

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Essa página lê e exibe dados relacionados das seguintes maneiras:

- A lista de instrutores exibe dados relacionados da `OfficeAssignment` entidade. As entidades `Instructor` e `OfficeAssignment` estão em uma relação um para zero ou um. Você usará o carregamento adiantado para as `OfficeAssignment` entidades. Conforme explicado anteriormente, o carregamento adiantado é geralmente mais eficiente quando você precisa dos dados relacionados para todas as linhas recuperadas da tabela primária. Nesse caso, você deseja exibir atribuições de escritório para todos os instrutores exibidos.
- Quando o usuário seleciona um instrutor, as entidades `Course` relacionadas são exibidas. As entidades `Instructor` e `Course` estão em uma relação muitos para muitos. Você usará o carregamento adiantado para as `Course` entidades e suas `Department` entidades relacionadas. Nesse caso, o carregamento lento pode ser mais eficiente, pois você precisa de cursos apenas para o instrutor selecionado. No entanto, este exemplo mostra como usar o carregamento adiantado para propriedades de navegação em entidades que estão nas propriedades de navegação.
- Quando o usuário seleciona um curso, os dados relacionados do `Enrollments` conjunto de entidades são exibidos. As entidades `Course` e `Enrollment` estão em uma relação um-para-muitos. Você adicionará o carregamento explícito para `Enrollment` entidades e suas `Student` entidades relacionadas. (O carregamento explícito não é necessário porque o carregamento lento está habilitado, mas isso mostra como fazer o carregamento explícito.)

### <a name="create-a-view-model-for-the-instructor-index-view"></a>Criar um modelo de exibição para a exibição de índice do instrutor

A página de índice do instrutor mostra três tabelas diferentes. Portanto, você criará um modelo de exibição que inclui três propriedades, cada uma contendo os dados de uma das tabelas.

Na pasta *ViewModels* , crie *InstructorIndexData.cs* e substitua o código existente pelo código a seguir:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

### <a name="adding-a-style-for-selected-rows"></a>Adicionando um estilo para linhas selecionadas

Para marcar as linhas selecionadas, você precisa de uma cor de plano de fundo diferente. Para fornecer um estilo para essa interface do usuário, adicione o seguinte código realçado à seção `/* info and errors */` em *Content\Site.css*, conforme mostrado abaixo:

[!code-css[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.css?highlight=2-5)]

### <a name="creating-the-instructor-controller-and-views"></a>Criando o controlador do instrutor e exibições

Crie um `InstructorController` controlador, conforme mostrado na ilustração a seguir:

![Add_Controller_dialog_box_for_Instructor_controller](https://asp.net/media/2577909/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Add_Controller_dialog_box_for_Instructor_controller_f99c10aa-1efd-49d6-af1d-b00461616107.png)

Abra *Controllers\InstructorController.cs* e adicione uma `using` instrução para o `ViewModels` namespace:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

O código com Scaffold no `Index` método especifica o carregamento adiantado apenas para a `OfficeAssignment` propriedade de navegação:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Substitua o `Index` método pelo código a seguir para carregar dados relacionados adicionais e colocá-los no modelo de exibição:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

O método aceita dados de rota opcionais ( `id` ) e um parâmetro de cadeia de caracteres de consulta ( `courseID` ) que fornece os valores de ID do instrutor selecionado e do curso selecionado e transmite todos os dados necessários para a exibição. Os parâmetros são fornecidos pelos hiperlinks **Selecionar** na página.

> [!TIP]
> 
> **Rotear dados**
> 
> Os dados de rota são dados que o associador de modelo encontrou em um segmento de URL especificado na tabela de roteamento. Por exemplo, a rota padrão especifica `controller` os `action` segmentos, e `id` :
> 
> ```csharp
> routes.MapRoute(  
>  name: "Default",  
>  url: "{controller}/{action}/{id}",  
>  defaults: new { controller = "Home", action = "Index", id = UrlParameter.Optional }  
> );
> ```
> 
> Na URL a seguir, a rota padrão `Instructor` é mapeada como `controller` a `Index` `action` e 1 como a `id` ; são valores de dados de rota.
> 
> `http://localhost:1230/Instructor/Index/1?courseID=2021`
> 
> "? cursoid = 2021" é um valor de cadeia de caracteres de consulta. O associador de modelo também funcionará se você passar o `id` como um valor de cadeia de caracteres de consulta:
> 
> `http://localhost:1230/Instructor/Index?id=1&CourseID=2021`
> 
> As URLs são criadas por `ActionLink` instruções na exibição do Razor. No código a seguir, o `id` parâmetro corresponde à rota padrão, portanto, `id` é adicionado aos dados de rota.
> 
> [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cshtml)]
> 
> No código a seguir, `courseID` não corresponde a um parâmetro na rota padrão, portanto, ele é adicionado como uma cadeia de caracteres de consulta.
> 
> [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cshtml)]

O código começa com a criação de uma instância do modelo de exibição e colocando-a na lista de instrutores. O código especifica o carregamento adiantado para o `Instructor.OfficeAssignment` e a `Instructor.Courses` propriedade de navegação.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs?highlight=3-4)]

O segundo `Include` método carrega cursos e, para cada curso carregado, ele faz o carregamento rápido da `Course.Department` propriedade de navegação.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

Como mencionado anteriormente, o carregamento adiantado não é necessário, mas é feito para melhorar o desempenho. Como a exibição sempre exige a `OfficeAssignment` entidade, é mais eficiente buscar isso na mesma consulta. `Course` as entidades são necessárias quando um instrutor é selecionado na página da Web, portanto, o carregamento adiantado é melhor do que o carregamento lento somente se a página for exibida com mais frequência com um curso selecionado do que sem.

Se uma ID de instrutor foi selecionada, o instrutor selecionado será recuperado da lista de instrutores no modelo de exibição. A propriedade do modelo de exibição `Courses` é então carregada com as `Course` entidades da propriedade de `Courses` navegação do instrutor.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

O `Where` método retorna uma coleção, mas, nesse caso, os critérios passados para esse método resultam em apenas uma única `Instructor` entidade que está sendo retornada. O `Single` método converte a coleção em uma única `Instructor` entidade, que fornece acesso à propriedade dessa entidade `Courses` .

Você usa o método [único](https://msdn.microsoft.com/library/system.linq.enumerable.single.aspx) em uma coleção quando sabe que a coleção terá apenas um item. O `Single` método lançará uma exceção se a coleção passada para ela estiver vazia ou se houver mais de um item. Uma alternativa é [SingleOrDefault](https://msdn.microsoft.com/library/bb342451.aspx), que retorna um valor padrão ( `null` nesse caso) se a coleção estiver vazia. No entanto, nesse caso, isso ainda resultaria em uma exceção (da tentativa de localizar uma `Courses` propriedade em uma `null` referência), e a mensagem de exceção indicaria menos clareza a causa do problema. Ao chamar o `Single` método, você também pode passar a condição em `Where` vez de chamar o `Where` método separadamente:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

Em vez de:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

Em seguida, se um curso foi selecionado, o curso selecionado é recuperado na lista de cursos no modelo de exibição. Em seguida, a propriedade do modelo de exibição `Enrollments` é carregada com as `Enrollment` entidades da `Enrollments` propriedade de navegação do curso.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs)]

### <a name="modifying-the-instructor-index-view"></a>Modificando a exibição do índice do instrutor

No *Views\Instructor\Index.cshtml*, substitua o código existente pelo código a seguir. As alterações são realçadas:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml?highlight=1,4,18,22-27,29,43-48)]

Você fez as seguintes alterações no código existente:

- Alterou a classe de modelo para `InstructorIndexData`.
- Alterou o título de página de **Índice** para **Instrutores**.
- Moveu as colunas de link de linha para a esquerda.
- A coluna **FullName** foi removida.
- Adicionada uma coluna **do Office** que exibe `item.OfficeAssignment.Location` somente se `item.OfficeAssignment` não for nulo. (Como essa é uma relação um-para-zero-ou-um, pode não haver uma entidade relacionada `OfficeAssignment` .) 

    [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]
- Código adicionado que será adicionado dinamicamente `class="selectedrow"` ao `tr` elemento do instrutor selecionado. Isso define uma cor de plano de fundo para a linha selecionada usando a classe CSS que você criou anteriormente. (O `valign` atributo será útil no tutorial a seguir quando você adicionar uma coluna de várias linhas à tabela.) 

    [!code-html[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.html)]
- Adição de um novo `ActionLink` rótulo **Select** imediatamente antes dos outros links em cada linha, o que faz com que a ID do instrutor selecionada seja enviada para o `Index` método.

Execute o aplicativo e selecione a guia **instrutores** . A página exibe a `Location` propriedade de `OfficeAssignment` entidades relacionadas e uma célula de tabela vazia quando não há nenhuma `OfficeAssignment` entidade relacionada.

![Instructors_index_page_with_nothing_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

No arquivo *Views\Instructor\Index.cshtml* , após o elemento de fechamento `table` (no final do arquivo), adicione o seguinte código realçado. Isso exibe uma lista de cursos relacionados a um instrutor quando um instrutor é selecionado.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml?highlight=11-46)]

Esse código lê a propriedade `Courses` do modelo de exibição para exibir uma lista de cursos. Ele também fornece um `Select` hiperlink que envia a ID do curso selecionado para o `Index` método de ação.

> [!NOTE]
> O arquivo *. css* é armazenado em cache por navegadores. Se você não vir as alterações ao executar o aplicativo, faça uma atualização de hardware (mantenha pressionada a tecla CTRL enquanto clica no botão **Atualizar** ou pressione CTRL + F5).

Execute a página e selecione um instrutor. Agora, você verá uma grade que exibe os cursos atribuídos ao instrutor selecionado, e para cada curso, verá o nome do departamento atribuído.

![Instructors_index_page_with_instructor_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Após o bloco de código que você acabou de adicionar, adicione o código a seguir. Isso exibe uma lista dos alunos que estão registrados em um curso quando esse curso é selecionado.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cshtml)]

Esse código lê a `Enrollments` Propriedade do modelo de exibição para exibir uma lista de alunos registrados no curso.

Execute a página e selecione um instrutor. Em seguida, selecione um curso para ver a lista de alunos registrados e suas notas.

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

### <a name="adding-explicit-loading"></a>Adicionando carregamento explícito

Abra *InstructorController.cs* e veja como o `Index` método obtém a lista de registros para um curso selecionado:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cs)]

Quando você recuperou a lista de instrutores, você especificou o carregamento adiantado para a `Courses` propriedade de navegação e para a `Department` propriedade de cada curso. Em seguida, você coloca a `Courses` coleção no modelo de exibição e agora está acessando a `Enrollments` propriedade de navegação de uma entidade nessa coleção. Como você não especificou o carregamento adiantado para a `Course.Enrollments` propriedade de navegação, os dados dessa propriedade são exibidos na página como resultado do carregamento lento.

Se você desabilitou o carregamento lento sem alterar o código de nenhuma outra forma, a `Enrollments` propriedade seria nula, independentemente de quantos registros o curso realmente tinha. Nesse caso, para carregar a `Enrollments` propriedade, você precisaria especificar o carregamento adiantado ou o carregamento explícito. Você já viu como fazer o carregamento adiantado. Para ver um exemplo de carregamento explícito, substitua o `Index` método pelo código a seguir, que carrega explicitamente a `Enrollments` propriedade. O código alterado está realçado.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample24.cs?highlight=20-27)]

Depois de obter a `Course` entidade selecionada, o novo código carrega explicitamente a propriedade de navegação do curso `Enrollments` :

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample25.cs)]

Em seguida, ele carrega explicitamente a `Enrollment` entidade relacionada de cada entidade `Student` :

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample26.cs)]

Observe que você usa o `Collection` método para carregar uma propriedade de coleção, mas para uma propriedade que contém apenas uma entidade, você usa o `Reference` método. Você pode executar a página de índice do instrutor agora e não verá nenhuma diferença no que é exibido na página, embora tenha alterado como os dados são recuperados.

## <a name="summary"></a>Resumo

Agora você usou todas as três maneiras (lentas, ansiosos e explícitas) para carregar dados relacionados em Propriedades de navegação. No próximo tutorial, você aprenderá a atualizar dados relacionados.

Links para outros recursos de Entity Framework podem ser encontrados no [mapa de conteúdo de acesso a dados do ASP.net](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Anterior](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md) 
>  [Avançar](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
