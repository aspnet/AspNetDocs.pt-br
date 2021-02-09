---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: Implementando a herança com o Entity Framework em um aplicativo MVC ASP.NET (8 de 10) | Microsoft Docs
author: tdykstra
description: O aplicativo Web de exemplo da Contoso University demonstra como criar aplicativos ASP.NET MVC 4 usando o Entity Framework 5 Code First e o Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: a5c3feff-5335-4cdd-a97d-f7a8785c2494
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: de86dd0d1a8e1b68d14971d328fe88a7fdb3e9b1
ms.sourcegitcommit: b4cdcf246850751579e45da80c9780fe56330dd0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/09/2021
ms.locfileid: "99984726"
---
# <a name="implementing-inheritance-with-the-entity-framework-in-an-aspnet-mvc-application-8-of-10"></a>Implementando a herança com o Entity Framework em um aplicativo MVC ASP.NET (8 de 10)

por [Tom Dykstra](https://github.com/tdykstra)

> O aplicativo Web de exemplo da Contoso University demonstra como criar aplicativos ASP.NET MVC 4 usando o Entity Framework 5 Code First e o Visual Studio 2012. Para obter informações sobre a série de tutoriais, consulte [o primeiro tutorial da série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
> 
> > [!NOTE] 
> > 
> > Se você encontrar um problema que não possa resolver, [Baixe o capítulo concluído](building-the-ef5-mvc4-chapter-downloads.md) e tente reproduzir o problema. Em geral, você pode encontrar a solução para o problema comparando seu código com o código concluído. Para alguns erros comuns e como resolvê-los, consulte [erros e soluções alternativas.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)

No tutorial anterior, você tratou exceções de simultaneidade. Este tutorial mostrará como implementar a herança no modelo de dados.

Na programação orientada a objeto, você pode usar a herança para eliminar o código redundante. Neste tutorial, você alterará as classes `Instructor` e `Student`, de modo que elas derivem de uma classe base `Person` que contém propriedades, como `LastName`, comuns a instrutores e alunos. Você não adicionará nem alterará as páginas da Web, mas alterará uma parte do código, e essas alterações serão refletidas automaticamente no banco de dados.

## <a name="table-per-hierarchy-versus-table-per-type-inheritance"></a>Herança de tabela por hierarquia versus de tabela por tipo

Na programação orientada a objeto, você pode usar a herança para facilitar o trabalho com classes relacionadas. Por exemplo, as `Instructor` `Student` classes e no `School` modelo de dados compartilham várias propriedades, o que resulta em um código redundante:

![Student_and_Instructor_classes](https://asp.net/media/2578113/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_e7a32f99-8bc4-48ce-aeaf-216a18071a8b.png)

Suponha que você deseje eliminar o código redundante para as propriedades compartilhadas pelas entidades `Instructor` e `Student`. Você pode criar uma `Person` classe base que contenha apenas essas propriedades compartilhadas e, em seguida, tornar as `Instructor` `Student` entidades e herdadas dessa classe base, conforme mostrado na ilustração a seguir:

![Student_and_Instructor_classes_deriving_from_Person_class](https://asp.net/media/2578119/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_deriving_from_Person_class_671d708c-cbb8-454a-a8f8-c2d99439acd9.png)

Há várias maneiras pelas quais essa estrutura de herança pode ser representada no banco de dados. Você pode ter uma `Person` tabela que inclui informações sobre alunos e instrutores em uma única tabela. Algumas das colunas podem se aplicar somente a instrutores ( `HireDate` ), algumas apenas para alunos ( `EnrollmentDate` ), algumas para ambos ( `LastName` , `FirstName` ). Normalmente, você teria uma coluna *discriminadora* para indicar qual tipo cada linha representa. Por exemplo, a coluna discriminatória pode ter "Instrutor" para instrutores e "Aluno" para alunos.

![Tabela por hierarchy_example](https://asp.net/media/2578125/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-hierarchy_example_244067cd-b451-4e9b-9595-793b9afca505.png)

Esse padrão de gerar uma estrutura de herança de entidade a partir de uma única tabela de banco de dados é chamada de herança de *tabela por hierarquia* (TPH).

Uma alternativa é fazer com que o banco de dados se pareça mais com a estrutura de herança. Por exemplo, você pode ter apenas os campos nome na `Person` tabela e ter `Instructor` tabelas e separadas `Student` com os campos de data.

![Tabela por type_inheritance](https://asp.net/media/2578131/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-type_inheritance.png)

Esse padrão de fazer uma tabela de banco de dados para cada classe de entidade é chamado de herança de *tabela por tipo* (TPT).

Os padrões de herança TPH geralmente oferecem melhor desempenho no Entity Framework do que os padrões de herança do TPT, pois os padrões de TPT podem resultar em consultas de junção complexas. Este tutorial demonstra como implementar a herança TPH. Você fará isso executando as seguintes etapas:

- Crie uma `Person` classe e altere as `Instructor` `Student` classes e das quais derivar `Person` .
- Adicione o código de mapeamento modelo a banco de dados à classe de contexto do banco de dados.
- Alteração `InstructorID` e `StudentID` referências em todo o projeto para `PersonID` .

## <a name="creating-the-person-class"></a>Criando a classe Person

 Observação: você não poderá compilar o projeto depois de criar as classes abaixo até atualizar os controladores que usam essas classes. 

Na pasta *modelos* , crie *Person.cs* e substitua o código do modelo pelo código a seguir:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

No *Instructor.cs*, derive a `Instructor` classe da `Person` classe e remova os campos de chave e nome. O código será semelhante ao seguinte exemplo:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Faça alterações semelhantes em *Student.cs*. A `Student` classe se parecerá com o exemplo a seguir:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="adding-the-person-entity-type-to-the-model"></a>Adicionando o tipo de entidade Person ao modelo

Em *SchoolContext.cs*, adicione uma `DbSet` propriedade para o `Person` tipo de entidade:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

Isso é tudo o que o Entity Framework precisa para configurar a herança de tabela por hierarquia. Como você verá, quando o banco de dados for recriado, ele terá uma `Person` tabela no lugar das `Student` `Instructor` tabelas e.

## <a name="changing-instructorid-and-studentid-to-personid"></a>Alterando Instrutorid e StudentId para PersonID

No *SchoolContext.cs*, na instrução de mapeamento Instructor-Course, altere `MapRightKey("InstructorID")` para `MapRightKey("PersonID")` :

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=4)]

Essa alteração não é necessária; Ele apenas altera o nome da coluna Instrutorid na tabela de junção muitos para muitos. Se você saiu do nome como Instrutorid, o aplicativo ainda funcionará corretamente. Aqui está o *SchoolContext.cs* concluído:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs?highlight=15,24)]

Em seguida, você precisa alterar `InstructorID` para `PersonID` e `StudentID` para `PersonID` todo o projeto ***exceto** _ nos arquivos de migrações com carimbo de data/hora na pasta _Migrations *. Para fazer isso, você encontrará e abrirá apenas os arquivos que precisam ser alterados e, em seguida, executará uma alteração global nos arquivos abertos. O único arquivo na pasta de *migrações* que você deve alterar é *Migrations\Configuration.cs.*

1. > [!IMPORTANT]
   > Comece fechando todos os arquivos abertos no Visual Studio.
2. Clique em **Localizar e substituir--localizar todos os arquivos** no menu **Editar** e, em seguida, pesquise todos os arquivos no projeto que contêm `InstructorID` .  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)
3. Abra cada arquivo na janela **Localizar resultados** **_exceto_* _ os &lt; arquivos de migração de carimbo de data/hora &gt; _ \_ . cs * na pasta *migrações* , clicando duas vezes em uma linha para cada arquivo.  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)
4. Abra a caixa de diálogo **substituir nos arquivos** e altere **examinar** para **todos os documentos abertos**.
5. Use a caixa de diálogo **substituir nos arquivos** para alterar tudo `InstructorID` para `PersonID.`  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)
6. Localize todos os arquivos no projeto que contêm `StudentID` .
7. Abra cada arquivo na janela **Localizar resultados** **_exceto_* _ os &lt; arquivos de migração de carimbo de data/hora &gt; _ \_ \* . cs * na pasta *migrações* , clicando duas vezes em uma linha para cada arquivo.  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)
8. Abra a caixa de diálogo **substituir nos arquivos** e altere **examinar** para **todos os documentos abertos**.
9. Use a caixa de diálogo **substituir nos arquivos** para alterar tudo `StudentID` para `PersonID` .   
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)
10. Compile o projeto.

(Observe que isso demonstra uma *desvantagem* do `classnameID` padrão para nomear chaves primárias. Se você tivesse nomeado a ID de chaves primárias sem prefixar o nome da classe, *nenhum* renome será necessário agora.)

## <a name="create-and-update-a-migrations-file"></a>Criar e atualizar um arquivo de migrações

No console do Gerenciador de pacotes (PMC), digite o seguinte comando:

`Add-Migration Inheritance`

Execute o `Update-Database` comando no PMC. O comando falhará neste ponto porque temos dados existentes que as migrações não sabem como lidar. Você obterá o seguinte erro:

*A instrução ALTER TABLE está em conflito com a restrição FOREIGN KEY "FK \_ dbo. Dbo do departamento \_ . PersonID \_ ". O conflito ocorreu no banco de dados "ContosoUniversity", tabela "dbo. Person ", coluna ' PersonID '.*

Abra *migrações \& lt; timestamp &gt; \_ Inheritance.cs* e substitua o `Up` método pelo código a seguir:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=25,29-40)]

Execute o comando `update-database` novamente.

> [!NOTE]
> É possível obter outros erros ao migrar dados e fazer alterações de esquema. Se você obtiver erros de migração que não pode resolver, poderá continuar com o tutorial alterando a cadeia de conexão no arquivo *Web.config* ou excluindo o banco de dados. A abordagem mais simples é renomear o banco de dados no arquivo de *Web.config* . Por exemplo, altere o nome do banco de dados para CU \_ Test, conforme mostrado no exemplo a seguir:
> 
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.xml?highlight=1-2)]
> 
> Com um novo banco de dados, não há nenhum dado a ser migrado e o `update-database` comando é muito mais provável de ser concluído sem erros. Para obter instruções sobre como excluir o banco de dados, consulte [como remover um banco de dados do Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/). Se você usar essa abordagem para continuar com o tutorial, ignore a etapa de implantação no final deste tutorial, já que o site implantado obteria o mesmo erro ao executar migrações automaticamente. Se você quiser solucionar um erro de migrações, o melhor recurso é um dos fóruns Entity Framework ou StackOverflow.com.

## <a name="testing"></a>Testando

Execute o site e tente várias páginas. Tudo funciona da mesma maneira que antes.

Em **Gerenciador de servidores,** expanda **SchoolContext** e, em seguida, **tabelas**, e você verá que as tabelas **Student** e **instrutor** foram substituídas por uma tabela **Person** . Expanda a tabela **Person** e você verá que ela tem todas as colunas que costumava estar nas tabelas **Student** e **instrutor** .

![Server_Explorer_showing_Person_table](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Clique com o botão direito do mouse na tabela Person e, em seguida, clique em **Mostrar Dados da Tabela** para ver a coluna discriminatória.

![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

O diagrama a seguir ilustra a estrutura do novo banco de dados escolar:

![School_database_diagram](https://asp.net/media/2578143/Windows-Live-Writer_58f5a93579b2_CC7B_School_database_diagram_6350a801-7199-413f-bbac-4a2009ed19d7.png)

## <a name="summary"></a>Resumo

A herança de tabela por hierarquia agora foi implementada para as `Person` `Student` classes, e `Instructor` . Para obter mais informações sobre esta e outras estruturas de herança, consulte [estratégias de mapeamento de herança](https://weblogs.asp.net/manavi/archive/2010/12/24/inheritance-mapping-strategies-with-entity-framework-code-first-ctp5-part-1-table-per-hierarchy-tph.aspx) no blog de Morteza manavi. No próximo tutorial, você verá algumas maneiras de implementar o repositório e os padrões de unidade de trabalho.

Links para outros recursos de Entity Framework podem ser encontrados no [mapa de conteúdo de acesso a dados do ASP.net](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Anterior](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md) 
>  [Avançar](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)
