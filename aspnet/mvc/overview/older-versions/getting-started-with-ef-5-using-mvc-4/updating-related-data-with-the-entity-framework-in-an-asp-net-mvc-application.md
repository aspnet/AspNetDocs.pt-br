---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: Atualizando dados relacionados com o Entity Framework em um aplicativo MVC ASP.NET (6 de 10) | Microsoft Docs
author: tdykstra
description: O aplicativo Web de exemplo da Contoso University demonstra como criar aplicativos ASP.NET MVC 4 usando o Entity Framework 5 Code First e o Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 7871dc05-2750-470f-8b4c-3a52511949bc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: dcdbd746aaa06d734f0d88b88e8d864e7e4ff5e7
ms.sourcegitcommit: b4cdcf246850751579e45da80c9780fe56330dd0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/09/2021
ms.locfileid: "99984927"
---
# <a name="updating-related-data-with-the-entity-framework-in-an-aspnet-mvc-application-6-of-10"></a>Atualizando dados relacionados com o Entity Framework em um aplicativo MVC ASP.NET (6 de 10)

por [Tom Dykstra](https://github.com/tdykstra)

> O aplicativo Web de exemplo da Contoso University demonstra como criar aplicativos ASP.NET MVC 4 usando o Entity Framework 5 Code First e o Visual Studio 2012. Para obter informações sobre a série de tutoriais, consulte [o primeiro tutorial da série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
> 
> > [!NOTE] 
> > 
> > Se você encontrar um problema que não possa resolver, [Baixe o capítulo concluído](building-the-ef5-mvc4-chapter-downloads.md) e tente reproduzir o problema. Em geral, você pode encontrar a solução para o problema comparando seu código com o código concluído. Para alguns erros comuns e como resolvê-los, consulte [erros e soluções alternativas.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)

No tutorial anterior, você exibiu dados relacionados; Neste tutorial, você atualizará os dados relacionados. Para a maioria das relações, isso pode ser feito atualizando os campos de chave estrangeira apropriados. Para relações muitos para muitos, o Entity Framework não expõe a tabela de junção diretamente, portanto, você deve adicionar e remover explicitamente entidades de e para as propriedades de navegação apropriadas.

As ilustrações a seguir mostram as páginas com as quais você trabalhará.

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a>Personalizar as páginas Criar e Editar dos cursos

Quando uma nova entidade de curso é criada, ela precisa ter uma relação com um departamento existente. Para facilitar isso, o código gerado por scaffolding inclui métodos do controlador e exibições Criar e Editar que incluem uma lista suspensa para seleção do departamento. A lista suspensa define a propriedade de `Course.DepartmentID` chave estrangeira, e isso é tudo o Entity Framework precisa para carregar a `Department` propriedade de navegação com a `Department` entidade apropriada. Você usará o código gerado por scaffolding, mas o alterará ligeiramente para adicionar tratamento de erro e classificação à lista suspensa.

No *CourseController.cs*, exclua os `Edit` quatro `Create` métodos e e substitua-os pelo seguinte código:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,10,13-14,21-27,34,41,44-45,52-58,62-68)]

O `PopulateDepartmentsDropDownList` método obtém uma lista de todos os departamentos classificados por nome, cria uma `SelectList` coleção para uma lista suspensa e passa a coleção para a exibição em uma `ViewBag` propriedade. O método aceita o parâmetro `selectedDepartment` opcional que permite que o código de chamada especifique o item que será selecionado quando a lista suspensa for renderizada. A exibição passará o nome `DepartmentID` para [o `DropDownList` auxiliar](../working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc.md)e, em seguida, o auxiliar saberá examinar o `ViewBag` objeto em busca de um `SelectList` nome `DepartmentID` .

O `HttpGet` `Create` método chama o `PopulateDepartmentsDropDownList` método sem definir o item selecionado, porque, para um novo curso, o departamento ainda não foi estabelecido:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

O `HttpGet` `Edit` método define o item selecionado, com base na ID do departamento que já está atribuído ao curso que está sendo editado:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

Os `HttpPost` métodos para ambos `Create` e `Edit` também incluem código que define o item selecionado quando eles reexibem a página após um erro:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

Esse código garante que quando a página for exibida novamente para mostrar a mensagem de erro, qualquer departamento selecionado permanecerá selecionado.

No *Views\Course\Create.cshtml*, adicione o código realçado para criar um novo campo de número de curso antes do campo **título** . Conforme explicado em um tutorial anterior, os campos de chave primária não são com Scaffold por padrão, mas essa chave primária é significativa, portanto, você deseja que o usuário seja capaz de inserir o valor da chave.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=17-23)]

Em *Views\Course\Edit.cshtml*, *Views\Course\Delete.cshtml* e *Views\Course\Details.cshtml*, adicione um campo de número de curso antes do campo **título** . Como é a chave primária, ela é exibida, mas não pode ser alterada.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml)]

Execute a página **criar** (exibir a página de índice do curso e clique em **criar nova**) e insira dados para um novo curso:

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Clique em **Criar**. A página de índice do curso é exibida com o novo curso adicionado à lista. O nome do departamento na lista de páginas de Índice é obtido da propriedade de navegação, mostrando que a relação foi estabelecida corretamente.

![Course_Index_page_showing_new_course](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Execute a página **Editar** (exiba a página de índice do curso e clique em **Editar** em um curso).

![Course_edit_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Altere dados na página e clique em **Salvar**. A página de índice do curso é exibida com os dados do curso atualizados.

## <a name="adding-an-edit-page-for-instructors"></a>Adicionando uma página de edição para instrutores

Quando você edita um registro de instrutor, deseja poder atualizar a atribuição de escritório do instrutor. A `Instructor` entidade tem uma relação um-para-zero-ou-um com a `OfficeAssignment` entidade, o que significa que você deve lidar com as seguintes situações:

- Se o usuário desmarcar a atribuição do Office e tiver originalmente um valor, você deverá remover e excluir a `OfficeAssignment` entidade.
- Se o usuário inserir um valor de atribuição do Office e ele tiver sido originalmente vazio, você deverá criar uma nova `OfficeAssignment` entidade.
- Se o usuário alterar o valor de uma atribuição do Office, você deverá alterar o valor em uma `OfficeAssignment` entidade existente.

Abra *InstructorController.cs* e examine o `HttpGet` `Edit` método:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

O código com Scaffold aqui não é o que você deseja. Ele está configurando dados para uma lista suspensa, mas você precisa de uma caixa de texto. Substitua este método pelo código a seguir:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Esse código descarta a `ViewBag` instrução e adiciona o carregamento adiantado para a `OfficeAssignment` entidade associada. Você não pode executar o carregamento rápido com o `Find` método, portanto, os `Where` `Single` métodos e são usados para selecionar o instrutor.

Substitua o `HttpPost` `Edit` método pelo código a seguir. que lida com atualizações de atribuição do Office:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

O código faz o seguinte:

- Obtém a entidade `Instructor` atual do banco de dados usando o carregamento adiantado para a propriedade de navegação `OfficeAssignment`. Isso é o mesmo que você fez no `HttpGet` `Edit` método.
- Atualiza a entidade `Instructor` recuperada com valores do associador de modelos. A sobrecarga [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.108).aspx) usada permite que você acesse as propriedades que deseja incluir na *lista* de permissões. Isso impede o excesso de postagens, conforme explicado no [segundo tutorial](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md).

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]
- Se o local do escritório estiver em branco, o definirá a `Instructor.OfficeAssignment` propriedade como nula para que a linha relacionada na `OfficeAssignment` tabela seja excluída.

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]
- Salva as alterações no banco de dados.

Em *Views\Instructor\Edit.cshtml*, depois dos `div` elementos do campo **data de contratação** , adicione um novo campo para editar o local do escritório:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml)]

Execute a página (selecione a guia **instrutores** e clique em **Editar** em um instrutor). Altere o **Local do Escritório** e clique em **Salvar**.

![Changing_the_office_location](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

## <a name="adding-course-assignments-to-the-instructor-edit-page"></a>Adicionando atribuições de curso à página de edição do instrutor

Os instrutores podem ministrar a quantidade de cursos que desejarem. Agora, você aprimorará a página Editar Instrutor adicionando a capacidade de alterar as atribuições de curso usando um grupo de caixas de seleção, conforme mostrado na seguinte captura de tela:

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

A relação entre as `Course` `Instructor` entidades e é muitos para muitos, o que significa que você não tem acesso direto à tabela de junção. Em vez disso, você adicionará e removerá entidades de e para a `Instructor.Courses` propriedade de navegação.

A interface do usuário que permite alterar a quais cursos um instrutor é atribuído é um grupo de caixas de seleção. Uma caixa de seleção é exibida para cada curso no banco de dados, e aqueles aos quais o instrutor está atribuído no momento são marcados. O usuário pode marcar ou desmarcar as caixas de seleção para alterar as atribuições de curso. Se o número de cursos fosse muito maior, provavelmente você desejaria usar um método diferente de apresentar os dados na exibição, mas usará o mesmo método de manipulação de propriedades de navegação para criar ou excluir relações.

Para fornecer dados à exibição para a lista de caixas de seleção, você usará uma classe de modelo de exibição. Crie *AssignedCourseData.cs* na pasta *ViewModels* e substitua o código existente pelo código a seguir:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

No *InstructorController.cs*, substitua o `HttpGet` `Edit` método pelo código a seguir. As alterações são realçadas.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs?highlight=5,8,12-27)]

O código adiciona o carregamento adiantado à propriedade de navegação `Courses` e chama o novo método `PopulateAssignedCourseData` para fornecer informações para a matriz de caixa de seleção usando a classe de modelo de exibição `AssignedCourseData`.

O código no `PopulateAssignedCourseData` método lê todas as `Course` entidades para carregar uma lista de cursos usando a classe de modelo de exibição. Para cada curso, o código verifica se o curso existe na propriedade de navegação `Courses` do instrutor. Para criar uma pesquisa eficiente ao verificar se um curso está atribuído ao instrutor, os cursos atribuídos ao instrutor são colocados em uma coleção [HashSet](https://msdn.microsoft.com/library/bb359438.aspx) . A `Assigned` propriedade é definida como `true` para os cursos que o instrutor está atribuído. A exibição usará essa propriedade para determinar quais caixas de seleção precisam ser exibidas como selecionadas. Por fim, a lista é passada para a exibição em uma `ViewBag` propriedade.

Em seguida, adicione o código que é executado quando o usuário clica em **Salvar**. Substitua o `HttpPost` `Edit` método pelo código a seguir, que chama um novo método que atualiza a `Courses` propriedade de navegação da `Instructor` entidade. As alterações são realçadas.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs?highlight=3,7,20,33,37-65)]

Como a exibição não tem uma coleção de `Course` entidades, o associador de modelo não pode atualizar automaticamente a `Courses` propriedade de navegação. Em vez de usar o associador de modelo para atualizar a propriedade de navegação de cursos, você fará isso no novo `UpdateInstructorCourses` método. Portanto, você precisa excluir a propriedade `Courses` do model binding. Isso não requer nenhuma alteração no código que chama [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.98).aspx) porque você está usando a sobrecarga de *lista* de permissões e `Courses` não está na lista de inclusões.

Se nenhuma caixa de seleção tiver sido selecionada, o código no `UpdateInstructorCourses` inicializará a `Courses` propriedade de navegação com uma coleção vazia:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

Em seguida, o código executa um loop em todos os cursos no banco de dados e verifica cada curso em relação àqueles atribuídos no momento ao instrutor e em relação àqueles que foram selecionados na exibição. Para facilitar pesquisas eficientes, as últimas duas coleções são armazenadas em objetos `HashSet`.

Se a caixa de seleção para um curso foi marcada, mas o curso não está na propriedade de navegação `Instructor.Courses`, o curso é adicionado à coleção na propriedade de navegação.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs)]

Se a caixa de seleção para um curso não foi marcada, mas o curso está na propriedade de navegação `Instructor.Courses`, o curso é removido da propriedade de navegação.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

No *Views\Instructor\Edit.cshtml*, adicione um campo **cursos** com uma matriz de caixas de seleção adicionando o seguinte código realçado imediatamente após os `div` elementos para o `OfficeAssignment` campo:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml?highlight=51-73)]

Esse código cria uma tabela HTML que contém três colunas. Em cada coluna há uma caixa de seleção, seguida de uma legenda que consiste no número e título do curso. Todas as caixas de seleção têm o mesmo nome ("selectedCourses"), que informa ao associador de modelo que eles devem ser tratados como um grupo. O `value` atributo de cada caixa de seleção é definido como o valor de `CourseID.` quando a página é postada, o associador de modelo passa uma matriz para o controlador que consiste nos `CourseID` valores somente para as caixas de seleção que estão selecionadas.

Quando as caixas de seleção são inicialmente renderizadas, as que são para os cursos atribuídos ao instrutor têm `checked` atributos, que os seleciona (exibe as marcações).

Depois de alterar as atribuições de curso, você desejará poder verificar as alterações quando o site retornar à `Index` página. Portanto, você precisa adicionar uma coluna à tabela nessa página. Nesse caso, você não precisa usar o `ViewBag` objeto, pois as informações que você deseja exibir já estão na `Courses` propriedade de navegação da `Instructor` entidade que você está passando para a página como modelo.

No *Views\Instructor\Index.cshtml*, adicione um título de **cursos** imediatamente após o título do **Office** , conforme mostrado no exemplo a seguir:

[!code-html[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.html?highlight=7)]

Em seguida, adicione uma nova célula de detalhes imediatamente após a célula de detalhes do local do escritório:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml?highlight=19,50-57)]

Execute a página de **índice do instrutor** para ver os cursos atribuídos a cada instrutor:

![Instructor_index_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Clique em **Editar** em um instrutor para ver a página Editar.

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

Altere algumas atribuições de curso e clique em **salvar**. As alterações feitas são refletidas na página Índice.

 Observação: a abordagem usada para editar os dados do curso do instrutor funciona bem quando há um número limitado de cursos. Para coleções muito maiores, uma interface do usuário e um método de atualização diferentes são necessários.  

## <a name="update-the-delete-method"></a>Atualizar o método Delete

Altere o código no método de exclusão HttpPost para que o registro de atribuição do Office (se houver) seja excluído quando o instrutor for excluído:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs?highlight=6,10)]

Se você tentar excluir um instrutor atribuído a um departamento como administrador, obterá um erro de integridade referencial. Consulte [a versão atual deste tutorial](../../getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) para obter código adicional que removerá automaticamente o instrutor de qualquer departamento em que o instrutor seja atribuído como um administrador.

## <a name="summary"></a>Resumo

Agora você concluiu esta introdução para trabalhar com dados relacionados. Até agora, nesses tutoriais, você fez uma gama completa de operações CRUD, mas não tratou de problemas de simultaneidade. O próximo tutorial apresentará o tópico de simultaneidade, explicará as opções para tratá-lo e adicionará a manipulação de simultaneidade ao código CRUD que você já escreveu para um tipo de entidade.

Links para outros recursos do Entity Framework, podem ser encontrados no final do [último tutorial desta série](advanced-entity-framework-scenarios-for-an-mvc-web-application.md).

> [!div class="step-by-step"]
> [Anterior](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) 
>  [Avançar](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
