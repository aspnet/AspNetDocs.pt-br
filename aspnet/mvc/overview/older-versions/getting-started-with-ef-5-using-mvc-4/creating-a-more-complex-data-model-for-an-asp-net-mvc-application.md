---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-a-more-complex-data-model-for-an-asp-net-mvc-application
title: Criando um modelo de dados mais complexo para um aplicativo MVC ASP.NET (4 de 10) | Microsoft Docs
author: tdykstra
description: O aplicativo Web de exemplo da Contoso University demonstra como criar aplicativos ASP.NET MVC 4 usando o Entity Framework 5 Code First e o Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: f81f3d80-3674-4d8e-a9b1-87feed1a93c9
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-a-more-complex-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 84e1218d29dd4d4afcf2b876c027b0579f012954
ms.sourcegitcommit: b4cdcf246850751579e45da80c9780fe56330dd0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/09/2021
ms.locfileid: "99985436"
---
# <a name="creating-a-more-complex-data-model-for-an-aspnet-mvc-application-4-of-10"></a>Criando um modelo de dados mais complexo para um aplicativo MVC ASP.NET (4 de 10)

por [Tom Dykstra](https://github.com/tdykstra)

> O aplicativo Web de exemplo da Contoso University demonstra como criar aplicativos ASP.NET MVC 4 usando o Entity Framework 5 Code First e o Visual Studio 2012. Para obter informações sobre a série de tutoriais, consulte [o primeiro tutorial da série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
> 
> > [!NOTE] 
> > 
> > Se você encontrar um problema que não possa resolver, [Baixe o capítulo concluído](building-the-ef5-mvc4-chapter-downloads.md) e tente reproduzir o problema. Em geral, você pode encontrar a solução para o problema comparando seu código com o código concluído. Para alguns erros comuns e como resolvê-los, consulte [erros e soluções alternativas.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)

Nos tutoriais anteriores, você trabalhou com um modelo de dados simples que era composto por três entidades. Neste tutorial, você adicionará mais entidades e relações e personalizará o modelo de dados especificando a formatação, a validação e as regras de mapeamento de banco de dado. Você verá duas maneiras de personalizar o modelo de dados: Adicionando atributos a classes de entidade e adicionando código à classe de contexto Database.

Quando terminar, as classes de entidade formarão o modelo de dados concluído mostrado na seguinte ilustração:

![School_class_diagram](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image1.png)

## <a name="customize-the-data-model-by-using-attributes"></a>Personalizar o modelo de dados usando atributos

Nesta seção, você verá como personalizar o modelo de dados usando atributos que especificam formatação, validação e regras de mapeamento de banco de dados. Em seguida, em várias das seções a seguir, você criará o `School` modelo de dados completo adicionando atributos às classes que você já criou e criando novas classes para os tipos de entidade restantes no modelo.

### <a name="the-datatype-attribute"></a>O atributo DataType

Para datas de registro de alunos, todas as páginas da Web atualmente exibem a hora junto com a data, embora tudo o que você deseje exibir nesse campo seja a data. Usando atributos de anotação de dados, você pode fazer uma alteração de código que corrigirá o formato de exibição em cada exibição que mostra os dados. Para ver um exemplo de como fazer isso, você adicionará um atributo à propriedade `EnrollmentDate` na classe `Student`.

No *Models\Student.cs*, adicione uma `using` instrução para o `System.ComponentModel.DataAnnotations` namespace e adicione `DataType` `DisplayFormat` atributos e à `EnrollmentDate` propriedade, conforme mostrado no exemplo a seguir:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,13-14)]

O atributo [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) é usado para especificar um tipo de dados que seja mais específico do que o tipo intrínseco de Database. Nesse caso, apenas desejamos acompanhar a data, não a data e a hora. A [Enumeração DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) fornece vários tipos de dados, como *data, hora, PhoneNumber, moeda, EmailAddress* e muito mais. O atributo `DataType` também pode permitir que o aplicativo forneça automaticamente recursos específicos a um tipo. Por exemplo, um `mailto:` link pode ser criado para [DataType. EmailAddress](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)e um seletor de data pode ser fornecido para [DataType. Date](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) em navegadores que dão suporte a [HTML5](http://html5.org/). Os atributos [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) emite os atributos [de dados](http://ejohn.org/blog/html-5-data-attributes/) HTML 5-(pronuncia-se *os dados Dash*) que os navegadores HTML 5 podem entender. Os atributos de [tipo de dados](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) não fornecem nenhuma validação.

`DataType.Date` não especifica o formato da data exibida. Por padrão, o campo de dados é exibido de acordo com os formatos padrão com base no [CultureInfo](https://msdn.microsoft.com/library/vstudio/system.globalization.cultureinfo(v=vs.110).aspx)do servidor.

O atributo `DisplayFormat` é usado para especificar explicitamente o formato de data:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample2.cs)]

A `ApplyFormatInEditMode` configuração especifica que a formatação especificada também deve ser aplicada quando o valor é exibido em uma caixa de texto para edição. (Talvez você não queira que, para alguns campos — por exemplo, para valores de moeda, talvez não queira o símbolo de moeda na caixa de texto para edição.)

Você pode usar o atributo [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) por si só, mas, em geral, é uma boa ideia usar o atributo [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) também. O `DataType` atributo transmite a *semântica* dos dados em vez de como renderizá-los em uma tela e fornece os seguintes benefícios que você não obtém `DisplayFormat` :

- O navegador pode habilitar recursos do HTML5 (por exemplo, para mostrar um controle de calendário, o símbolo de moeda apropriado da localidade, links de email, etc.).
- Por padrão, o navegador renderizará dados usando o formato correto com base em sua [localidade](https://msdn.microsoft.com/library/vstudio/wyzd2bce.aspx).
- O atributo [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) pode habilitar o MVC a escolher o modelo de campo à direita para renderizar os dados (o [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) se usado por si só usa o modelo de cadeia de caracteres). Para obter mais informações, consulte Brad Wilson ' s [ASP.NET MVC 2 templates](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html). (Embora seja escrito para MVC 2, este artigo ainda se aplica à versão atual do ASP.NET MVC.)

Se você usar o `DataType` atributo com um campo de data, precisará especificar o `DisplayFormat` atributo também para garantir que o campo seja renderizado corretamente em navegadores Chrome. Para obter mais informações, consulte [este thread StackOverflow](http://stackoverflow.com/questions/12633471/mvc4-datatype-date-editorfor-wont-display-date-value-in-chrome-fine-in-ie).

Execute a página de índice de estudante novamente e observe que os horários não são mais exibidos para as datas de registro. O mesmo será verdadeiro para qualquer exibição que use o `Student` modelo.

![Students_index_page_with_formatted_date](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image2.png)

### <a name="the-stringlengthattribute"></a>O StringLengthAttribute

Você também pode especificar regras de validação de dados e mensagens usando atributos. Suponha que você deseje garantir que os usuários não insiram mais de 50 caracteres em um nome. Para adicionar essa limitação, adicione atributos [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) às `LastName` Propriedades e `FirstMidName` , conforme mostrado no exemplo a seguir:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample3.cs?highlight=10,12)]

O atributo [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) não impedirá que um usuário insira espaço em branco para um nome. Você pode usar o atributo [RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) para aplicar restrições à entrada. Por exemplo, o código a seguir requer que o primeiro caractere esteja em letras maiúsculas e os caracteres restantes sejam alfabéticos:

`[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]`

O atributo [MaxLength](https://msdn.microsoft.com/library/System.ComponentModel.DataAnnotations.MaxLengthAttribute.aspx) fornece funcionalidade semelhante ao atributo [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) , mas não fornece validação do lado do cliente.

Execute o aplicativo e clique na guia **alunos** . Você Obtém o seguinte erro:

*O modelo que faz o backup do contexto ' SchoolContext ' foi alterado desde que o banco de dados foi criado. Considere o uso de Migrações do Code First para atualizar o banco de dados ( [https://go.microsoft.com/fwlink/?LinkId=238269](https://go.microsoft.com/fwlink/?LinkId=238269) ).*

O modelo de banco de dados foi alterado de uma maneira que requer uma alteração no esquema de banco de dados e Entity Framework detectado. Você usará as migrações para atualizar o esquema sem perder os dados que você adicionou ao banco de dado usando a interface do usuário. Se você alterou os dados que foram criados pelo `Seed` método, eles serão alterados de volta para seu estado original devido ao método [AddOrUpdate](https://msdn.microsoft.com/library/hh846520(v=vs.103).aspx) que você está usando no `Seed` método. ([AddOrUpdate](https://msdn.microsoft.com/library/hh846520(v=vs.103).aspx) é equivalente a uma operação "Upsert" da terminologia do banco de dados.)

No PMC (Console do Gerenciador de Pacotes), Insira os seguintes comandos:

[!code-console[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample4.cmd)]

O `add-migration MaxLengthOnNames` comando cria um arquivo chamado *&lt; timestamp &gt; \_ MaxLengthOnNames.cs*. Este arquivo contém um código que atualizará o banco de dados para corresponder ao modelo atual. O carimbo de data/hora anexado ao nome do arquivo de migrações é usado pelo Entity Framework para ordenar as migrações. Depois de ter criado várias migrações, se você remover o banco de dados ou se implantar o projeto usando as migrações, todas as migrações serão aplicadas na ordem em que foram criadas.

Execute a página **criar** e insira um nome com mais de 50 caracteres. Assim que você exceder 50 caracteres, a validação do lado do cliente mostra imediatamente uma mensagem de erro.

![erro de Val do lado do cliente](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image3.png)

### <a name="the-column-attribute"></a>O atributo de coluna

Você também pode usar atributos para controlar como as classes e propriedades são mapeadas para o banco de dados. Suponha que você tenha usado o nome `FirstMidName` para o campo de nome porque o campo também pode conter um sobrenome. Mas você deseja que a coluna do banco de dados seja nomeada `FirstName`, pois os usuários que escreverão consultas ad hoc no banco de dados estão acostumados com esse nome. Para fazer esse mapeamento, use o atributo `Column`.

O atributo `Column` especifica que quando o banco de dados for criado, a coluna da tabela `Student` que é mapeada para a propriedade `FirstMidName` será nomeada `FirstName`. Em outras palavras, quando o código se referir a `Student.FirstMidName`, os dados serão obtidos ou atualizados na coluna `FirstName` da tabela `Student`. Se você não especificar nomes de coluna, eles receberão o mesmo nome que o nome da propriedade.

Adicione uma instrução using para [System. ComponentModel. Annotations. Schema](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.aspx) e o atributo de nome de coluna à `FirstMidName` propriedade, conforme mostrado no seguinte código realçado:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample5.cs?highlight=4,14)]

A adição do [atributo Column](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute.aspx) altera o modelo de backup do SchoolContext, de modo que ele não corresponderá ao banco de dados. Insira os seguintes comandos no PMC para criar outra migração:

[!code-console[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample6.cmd)]

Em **Gerenciador de servidores** (**Gerenciador de banco de dados** se você estiver usando o Express para Web), clique duas vezes na tabela *Student* .

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image4.png)

A imagem a seguir mostra o nome da coluna original como era antes de aplicar as duas primeiras migrações. Além do nome da coluna que muda de `FirstMidName` para `FirstName` , as duas colunas de nome foram alteradas de `MAX` comprimento para 50 caracteres.

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image5.png)

Você também pode fazer alterações de mapeamento de banco de dados usando a [API Fluent](https://msdn.microsoft.com/data/jj591617), como você verá mais adiante neste tutorial.

> [!NOTE]
> Se você tentar compilar antes de concluir a criação de todas essas classes de entidade, poderá obter erros de compilador.

## <a name="create-the-instructor-entity"></a>Criar a entidade Instructor

![Instructor_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image6.png)

Crie *Models\Instructor.cs*, substituindo o código do modelo pelo código a seguir:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample7.cs)]

Observe que várias propriedades são iguais nas entidades `Student` e `Instructor`. No tutorial [implementando a herança](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md) mais adiante nesta série, você refatorar usando a herança para eliminar essa redundância.

### <a name="the-required-and-display-attributes"></a>Os atributos obrigatórios e de exibição

Os atributos na `LastName` Propriedade especificam que é um campo obrigatório, que a legenda da caixa de texto deve ser "Last Name" (em vez do nome da propriedade, que seria "LastName" sem espaço) e que o valor não pode ter mais de 50 caracteres.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample8.cs)]

O [atributo StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) define o comprimento máximo no banco de dados e fornece validação do lado do cliente e do servidor para o ASP.NET MVC. Você também pode especificar o tamanho mínimo da cadeia de caracteres nesse atributo, mas o valor mínimo não tem nenhum impacto sobre o esquema de banco de dados. O [Atributo Required](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) não é necessário para tipos de valor como DateTime, int, Double e float. Tipos de valor não podem ser atribuídos a um valor nulo, portanto, são inerentemente necessários. Você pode remover o [atributo necessário](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) e substituí-lo por um parâmetro de comprimento mínimo para o `StringLength` atributo:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample9.cs?highlight=2)]

Você pode colocar vários atributos em uma linha, portanto, você também pode escrever a classe do instrutor da seguinte maneira:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample10.cs)]

### <a name="the-fullname-calculated-property"></a>A propriedade calculada FullName

`FullName` é uma propriedade calculada que retorna um valor criado pela concatenação de duas outras propriedades. Portanto, ele tem apenas um `get` acessador e nenhuma `FullName` coluna será gerada no banco de dados.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

### <a name="the-courses-and-officeassignment-navigation-properties"></a>As propriedades de navegação de cursos e OfficeAssignment

As propriedades `Courses` e `OfficeAssignment` são propriedades de navegação. Como foi explicado anteriormente, eles normalmente são definidos como [virtuais](https://msdn.microsoft.com/library/9fkccyh4(v=vs.110).aspx) para que possam aproveitar um recurso Entity Framework chamado [carregamento lento](https://msdn.microsoft.com/magazine/hh205756.aspx). Além disso, se uma propriedade de navegação puder conter várias entidades, seu tipo deverá implementar a interface [ &lt; &gt; T do ICollection](https://msdn.microsoft.com/library/92t2ye13.aspx) . (Por exemplo [, &lt; IList &gt; T](https://msdn.microsoft.com/library/5y536ey6.aspx) qualifica mas não [IEnumerable &lt; t &gt; ](https://msdn.microsoft.com/library/9eekhta0.aspx) porque o `IEnumerable<T>` não implementa [Adicionar](https://msdn.microsoft.com/library/63ywd54z.aspx).

Um instrutor pode ensinar qualquer número de cursos, portanto, `Courses` é definido como uma coleção de `Course` entidades. Nossas regras de negócio afirmam que um instrutor só pode ter no máximo um escritório, portanto, `OfficeAssignment` é definido como uma única `OfficeAssignment` entidade (que pode ser `null` se nenhum escritório for atribuído).

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

## <a name="create-the-officeassignment-entity"></a>Criar a entidade OfficeAssignment

![OfficeAssignment_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image7.png)

Crie *Models\OfficeAssignment.cs* com o seguinte código:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample13.cs)]

Compile o projeto, que salva as alterações e verifica se você não fez nenhum erro de cópia e colagem que o compilador pode detectar.

### <a name="the-key-attribute"></a>O atributo de chave

Há uma relação um-para-zero-ou-um entre as `Instructor` `OfficeAssignment` entidades e. Uma atribuição do Office existe somente em relação ao instrutor ao qual ele está atribuído e, portanto, sua chave primária também é sua chave estrangeira para a `Instructor` entidade. Mas o Entity Framework não pode reconhecer automaticamente `InstructorID` como a chave primária desta entidade porque seu nome não segue a `ID` Convenção de nomenclatura ou *ClassName* `ID` . Portanto, o atributo `Key` é usado para identificá-la como a chave:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample14.cs)]

Você também pode usar o `Key` atributo se a entidade tiver sua própria chave primária, mas desejar nomear a propriedade como algo diferente de `classnameID` ou `ID` . Por padrão, o EF trata a chave como não gerada pelo banco de dados porque a coluna é para uma relação de identificação.

### <a name="the-foreignkey-attribute"></a>O atributo ForeignKey

Quando há uma relação um-para-zero-ou-um ou uma relação um-para-um entre duas entidades (como entre `OfficeAssignment` e `Instructor` ), o EF não pode solucionar qual extremidade da relação é a entidade de segurança e qual final é dependente. Relações um-para-um têm uma propriedade de navegação de referência em cada classe para a outra classe. O [atributo ForeignKey](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx) pode ser aplicado à classe dependent para estabelecer a relação. Se você omitir o [atributo ForeignKey](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx), obterá o seguinte erro ao tentar criar a migração:

Não é possível determinar a extremidade principal de uma associação entre os tipos ' ContosoUniversity. Models. OfficeAssignment ' e ' ContosoUniversity. Models. instrutor '. A extremidade principal dessa associação deve ser explicitamente configurada usando a API Fluent ou as anotações de dados da relação.

Posteriormente no tutorial, mostraremos como configurar essa relação com a API Fluent.

### <a name="the-instructor-navigation-property"></a>A propriedade de navegação do instrutor

A `Instructor` entidade tem uma `OfficeAssignment` propriedade de navegação anulável (porque um instrutor pode não ter uma atribuição do Office) e a `OfficeAssignment` entidade tem uma propriedade de `Instructor` navegação não anulável (porque uma atribuição do Office não pode existir sem um instrutor – `InstructorID` é não anulável). Quando uma `Instructor` entidade tem uma `OfficeAssignment` entidade relacionada, cada entidade terá uma referência para a outra em sua propriedade de navegação.

Você pode colocar um `[Required]` atributo na propriedade de navegação do instrutor para especificar que deve haver um instrutor relacionado, mas você não precisa fazer isso porque a chave estrangeira do instrutorid (que também é a chave para essa tabela) é não anulável.

## <a name="modify-the-course-entity"></a>Modificar a entidade Course

![Course_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image8.png)

No *Models\Course.cs*, substitua o código que você adicionou anteriormente com o seguinte código:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample15.cs)]

A entidade curso tem uma propriedade Foreign Key `DepartmentID` que aponta para a `Department` entidade relacionada e tem uma `Department` propriedade de navegação. O Entity Framework não exige que você adicione uma propriedade de chave estrangeira ao modelo de dados quando você tem uma propriedade de navegação para uma entidade relacionada. O EF cria automaticamente chaves estrangeiras no banco de dados onde quer que sejam necessárias. No entanto, ter a chave estrangeira no modelo de dados pode tornar as atualizações mais simples e mais eficientes. Por exemplo, quando você buscar uma entidade de curso para editar, a `Department` entidade será nula se você não carregá-la, então, ao atualizar a entidade do curso, você precisará primeiro buscar a `Department` entidade. Quando a propriedade de chave estrangeira `DepartmentID` é incluída no modelo de dados, você não precisa buscar a `Department` entidade antes de atualizar.

### <a name="the-databasegenerated-attribute"></a>O atributo DatabaseGenerated

O [atributo DatabaseGenerated](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute.aspx) com o parâmetro [None](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedoption(v=vs.110).aspx) na `CourseID` propriedade especifica que os valores de chave primária são fornecidos pelo usuário em vez de gerados pelo banco de dados.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample16.cs)]

Por padrão, o Entity Framework assume que os valores de chave primária são gerados pelo banco de dados. É isso que você quer na maioria dos cenários. No entanto, para `Course` entidades, você usará um número de curso especificado pelo usuário, como uma série 1000 para um departamento, uma série 2000 para outro departamento e assim por diante.

### <a name="foreign-key-and-navigation-properties"></a>Chave estrangeira e propriedades de navegação

As propriedades de chave estrangeira e as propriedades de navegação na `Course` entidade refletem as seguintes relações:

- Um curso é atribuído a um departamento e, portanto, há uma propriedade de chave estrangeira `DepartmentID` e de navegação `Department` pelas razões mencionadas acima. 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample17.cs)]
- Um curso pode ter qualquer quantidade de estudantes inscritos; portanto, a propriedade de navegação `Enrollments` é uma coleção: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample18.cs)]
- Um curso pode ser ministrado por vários instrutores; portanto, a propriedade de navegação `Instructors` é uma coleção: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample19.cs)]

## <a name="creating-the-department-entity"></a>Criando a entidade Department

![Department_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image9.png)

Crie *Models\Department.cs* com o seguinte código:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample20.cs)]

### <a name="the-column-attribute"></a>O atributo de coluna

Anteriormente, você usou o [atributo Column](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute.aspx) para alterar o mapeamento de nome de coluna. No código para a `Department` entidade, o `Column` atributo está sendo usado para alterar o mapeamento de tipo de dados SQL para que a coluna seja definida usando o tipo de SQL Server [Money](https://msdn.microsoft.com/library/ms179882.aspx) no banco de dado:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample21.cs)]

O mapeamento de coluna geralmente não é necessário, porque o Entity Framework geralmente escolhe o tipo de dados apropriado SQL Server com base no tipo CLR que você define para a propriedade. O tipo `decimal` CLR é mapeado para um tipo `decimal` SQL Server. Mas, nesse caso, você sabe que a coluna manterá valores de moeda e o tipo de dados [Money](https://msdn.microsoft.com/library/ms179882.aspx) é mais apropriado para isso.

### <a name="foreign-key-and-navigation-properties"></a>Chave estrangeira e propriedades de navegação

As propriedades de navegação e de chave estrangeira refletem as seguintes relações:

- Um departamento pode ou não ter um administrador, e um administrador é sempre um instrutor. Portanto `InstructorID` , a propriedade é incluída como a chave estrangeira para a `Instructor` entidade e um ponto de interrogação é adicionado após a `int` designação de tipo para marcar a propriedade como anulável. A propriedade de navegação é chamada `Administrator` , mas contém uma `Instructor` entidade: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample22.cs)]
- Um departamento pode ter muitos cursos, portanto, há uma `Courses` propriedade de navegação: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample23.cs)]

  > [!NOTE]
  > Por convenção, o Entity Framework habilita a exclusão em cascata para chaves estrangeiras que não permitem valor nulo e em relações muitos para muitos. Isso pode resultar em regras de exclusão em cascata circulares, o que causará uma exceção quando o código do inicializador for executado. Por exemplo, se você não definiu a `Department.InstructorID` propriedade como anulável, obteria a seguinte mensagem de exceção quando o inicializador for executado: "a relação referencial resultará em uma referência cíclica que não é permitida". Se suas regras de negócios exigirem a `InstructorID` propriedade como não anulável, você precisaria usar a seguinte API fluente para desabilitar a exclusão em cascata na relação: 

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample24.cs)]

## <a name="modifying-the-student-entity"></a>Modificando a entidade Student

![Student_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image10.png)

No *Models\Student.cs*, substitua o código que você adicionou anteriormente com o código a seguir. As alterações são realçadas.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample25.cs?highlight=12,15,24-27)]

## <a name="the-enrollment-entity"></a>A entidade de registro

 No *Models\Enrollment.cs*, substitua o código que você adicionou anteriormente com o código a seguir

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample26.cs?highlight=16)]

### <a name="foreign-key-and-navigation-properties"></a>Chave estrangeira e propriedades de navegação

As propriedades de navegação e de chave estrangeira refletem as seguintes relações:

- Um registro destina-se a um único curso e, portanto, há uma propriedade de chave estrangeira `CourseID` e uma propriedade de navegação `Course`: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample27.cs)]
- Um registro destina-se a um único aluno e, portanto, há uma propriedade de chave estrangeira `StudentID` e uma propriedade de navegação `Student`: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample28.cs)]

### <a name="many-to-many-relationships"></a>Relações muitos para muitos

Há uma relação muitos-para-muitos entre as `Student` entidades e e `Course` a `Enrollment` entidade funciona como uma tabela de junção muitos para muitos *com carga* no banco de dados. Isso significa que a `Enrollment` tabela contém dados adicionais além de chaves estrangeiras para as tabelas Unidas (nesse caso, uma chave primária e uma `Grade` Propriedade).

A ilustração a seguir mostra a aparência dessas relações em um diagrama de entidades. (Este diagrama foi gerado usando o [Entity Framework Power Tools](https://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d); criar o diagrama não faz parte do tutorial, ele está sendo usado aqui como uma ilustração.)

![Course_many de estudante para many_relationship](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image11.png)

Cada linha de relação tem um 1 em uma extremidade e um asterisco ( \* ) no outro, indicando uma relação um-para-muitos.

Se a `Enrollment` tabela não incluir informações de nível, ela só precisaria conter as duas chaves estrangeiras `CourseID` e `StudentID` . Nesse caso, ele corresponderia a uma tabela de junção muitos para muitos *sem carga* (ou uma *tabela de junção pura*) no banco de dados, e você não teria que criar uma classe de modelo para ela. As `Instructor` `Course` entidades e têm esse tipo de relação muitos-para-muitos e, como você pode ver, não há nenhuma classe de entidade entre elas:

![Instrutor-Course_many para many_relationship](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image12.png)

Uma tabela de junção é necessária no banco de dados, no entanto, conforme mostrado no diagrama de banco de dados a seguir:

![Instrutor-Course_many para many_relationship_tables](https://asp.net/media/2577802/Windows-Live-Writer_Creating-a.NET-MVC-Application-4-of-10h1_B662_Instructor-Course_many-to-many_relationship_tables_03e042cf-db89-4b4c-985a-e458351ada76.png)

O Entity Framework cria automaticamente a `CourseInstructor` tabela, e você lê e atualiza-a indiretamente lendo e atualizando as `Instructor.Courses` `Course.Instructors` Propriedades de navegação e.

## <a name="entity-diagram-showing-relationships"></a>Diagrama de entidade mostrando relações

A ilustração a seguir mostra o diagrama criado pelo Entity Framework Power Tools para o modelo Escola concluído.

![School_data_model_diagram](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image13.png)

Além das linhas de relação muitos para muitos ( \* para \* ) e as linhas de relação um-para-muitos (1 a \* ), você pode ver aqui a linha de relação um-para-zero-ou-um (1 a 0.. 1) entre `Instructor` as `OfficeAssignment` entidades e e a linha de relação zero ou-um para muitos (0.. 1 para \* ) entre as entidades do instrutor e do departamento.

## <a name="customize-the-data-model-by-adding-code-to-the-database-context"></a>Personalizar o modelo de dados adicionando código ao contexto do banco de dado

Em seguida, você adicionará as novas entidades à `SchoolContext` classe e personalizará parte do mapeamento usando chamadas de [API fluente](https://msdn.microsoft.com/data/jj591617) . (A API é "fluente" porque é frequentemente usada pela cadeia de caracteres de uma série de chamadas de método em uma única instrução.)

Neste tutorial, você usará a API fluente somente para mapeamento de banco de dados que não pode fazer com atributos. No entanto, você também pode usar a API fluente para especificar a maioria das regras de formatação, validação e mapeamento que pode ser feita por meio de atributos. Alguns atributos como `MinimumLength` não podem ser aplicados com a API fluente. Como mencionado anteriormente, `MinimumLength` não altera o esquema, ele aplica apenas uma regra de validação do cliente e do servidor

Alguns desenvolvedores preferem usar a API fluente exclusivamente para que possam manter suas classes de entidade "limpas". Combine atributos e a API fluente se desejar. Além disso, há algumas personalizações que podem ser feitas apenas com a API fluente, mas em geral, a prática recomendada é escolher uma dessas duas abordagens e usar isso com o máximo de consistência possível.

Para adicionar as novas entidades ao modelo de dados e executar o mapeamento de banco de dado que você não fez usando atributos, substitua o código em *DAL\SchoolContext.cs* pelo seguinte código:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample29.cs)]

A nova instrução no método [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) configura a tabela de junção muitos para muitos:

- Para a relação muitos-para-muitos entre as `Instructor` `Course` entidades e, o código especifica os nomes de tabela e coluna para a tabela de junção. Code First pode configurar a relação muitos-para-muitos para você sem esse código, mas se você não chamá-lo, obterá nomes padrão, como `InstructorInstructorID` para a `InstructorID` coluna.

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample30.cs)]

O código a seguir fornece um exemplo de como você poderia ter usado a API fluente em vez de atributos para especificar a relação entre as `Instructor` `OfficeAssignment` entidades e:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample31.cs)]

Para obter informações sobre o que as instruções "API Fluent" estão fazendo nos bastidores, consulte a postagem do blog da [API fluente](https://blogs.msdn.com/b/aspnetue/archive/2011/05/04/entity-framework-code-first-tutorial-supplement-what-is-going-on-in-a-fluent-api-call.aspx) .

## <a name="seed-the-database-with-test-data"></a>Propagar o banco de dados com os dados de teste

Substitua o código no arquivo *Migrations\Configuration.cs* pelo código a seguir para fornecer dados de semente para as novas entidades que você criou.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample32.cs)]

Como vimos no primeiro tutorial, a maior parte desse código simplesmente atualiza ou cria novos objetos de entidade e carrega dados de exemplo em Propriedades, conforme necessário para o teste. No entanto, observe como a `Course` entidade, que tem uma relação muitos para muitos com a `Instructor` entidade, é tratada:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample33.cs)]

Ao criar um `Course` objeto, você inicializa a `Instructors` propriedade de navegação como uma coleção vazia usando o código `Instructors = new List<Instructor>()` . Isso torna possível adicionar `Instructor` entidades relacionadas a isso `Course` usando o `Instructors.Add` método. Se você não criou uma lista vazia, não poderá adicionar essas relações, pois a `Instructors` propriedade seria nula e não teria um `Add` método. Você também pode adicionar a inicialização da lista ao construtor.

## <a name="add-a-migration-and-update-the-database"></a>Adicionar uma migração e atualizar o banco de dados

No PMC, digite o `add-migration` comando:

`PM> add-Migration Chap4`

Se você tentar atualizar o banco de dados neste ponto, obterá o seguinte erro:

*A instrução ALTER TABLE está em conflito com a restrição FOREIGN KEY "FK \_ dbo. Curso \_ dbo. DepartmentID do departamento \_ ". O conflito ocorreu no banco de dados "ContosoUniversity", tabela "dbo. Departamento ", coluna ' DepartmentID '.*

Edite o &lt; arquivo *timestamp &gt; \_ Chap4.cs* e faça as alterações de código a seguir (você adicionará uma instrução SQL e modificará uma `AddColumn` instrução):

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample34.cs?highlight=14-18)]

(Lembre-se de comentar ou excluir a linha existente `AddColumn` quando adicionar a nova, ou você receberá um erro ao inserir o `update-database` comando.)

Às vezes, quando você executa migrações com dados existentes, precisa inserir dados de stub no banco de dado para satisfazer as restrições de chave estrangeira, e é isso o que você está fazendo agora. O código gerado adiciona uma `DepartmentID` chave estrangeira não anulável à `Course` tabela. Se já houver linhas na `Course` tabela quando o código for executado, a `AddColumn` operação falhará porque SQL Server não sabe qual valor deve ser colocado na coluna que não pode ser nula. Portanto, você alterou o código para dar à nova coluna um valor padrão e criou um departamento de stub chamado "Temp" para atuar como departamento padrão. Como resultado, se houver `Course` linhas existentes quando esse código for executado, todas estarão relacionadas ao departamento "Temp".

Quando o `Seed` método é executado, ele insere linhas na `Department` tabela e relaciona as `Course` linhas existentes a essas novas `Department` linhas. Se você não tiver adicionado nenhum curso na interface do usuário, não precisará mais do departamento "Temp" ou do valor padrão na `Course.DepartmentID` coluna. Para permitir a possibilidade de que alguém possa ter adicionado cursos usando o aplicativo, você também deseja atualizar o `Seed` código do método para garantir que todas as `Course` linhas (não apenas aquelas inseridas por execuções anteriores do `Seed` método) tenham valores válidos `DepartmentID` antes de remover o valor padrão da coluna e excluir o departamento "Temp".

Depois de terminar de editar o &lt; arquivo *timestamp &gt; \_ Chap4.cs* , insira o `update-database` comando no PMC para executar a migração.

> [!NOTE]
> É possível obter outros erros ao migrar dados e fazer alterações de esquema. Se você obtiver erros de migração que não pode resolver, poderá alterar a cadeia de conexão no arquivo *Web.config* ou excluir o banco de dados. A abordagem mais simples é renomear o banco de dados no arquivo *Web.config* . Por exemplo, altere o nome do banco de dados para CU \_ Test, conforme mostrado a seguir:
> 
> [!code-xml[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample35.xml?highlight=1-2)]
> 
> Com um novo banco de dados, não há nenhum dado a ser migrado e o `update-database` comando é muito mais provável de ser concluído sem erros. Para obter instruções sobre como excluir o banco de dados, consulte [como remover um banco de dados do Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/).

Abra o banco de dados no **Gerenciador de servidores** como fazia anteriormente e expanda o nó **tabelas** para ver que todas as tabelas foram criadas. (Se você ainda tiver **Gerenciador de servidores** aberto a partir do momento anterior, clique no botão **Atualizar** .)

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image14.png)

Você não criou uma classe de modelo para a `CourseInstructor` tabela. Conforme explicado anteriormente, trata-se de uma tabela de junção para a relação muitos para muitos entre as `Instructor` `Course` entidades e.

Clique com o botão direito do mouse na `CourseInstructor` tabela e selecione **Mostrar dados da tabela** para verificar se ela contém dados como resultado das `Instructor` entidades que você adicionou à `Course.Instructors` propriedade de navegação.

![Table_data_in_CourseInstructor_table](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image15.png)

## <a name="summary"></a>Resumo

Agora você tem um modelo de dados mais complexo e um banco de dados correspondente. No tutorial a seguir, você aprenderá mais sobre diferentes maneiras de acessar dados relacionados.

Links para outros recursos de Entity Framework podem ser encontrados no [mapa de conteúdo de acesso a dados do ASP.net](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Anterior](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md) 
>  [Avançar](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
