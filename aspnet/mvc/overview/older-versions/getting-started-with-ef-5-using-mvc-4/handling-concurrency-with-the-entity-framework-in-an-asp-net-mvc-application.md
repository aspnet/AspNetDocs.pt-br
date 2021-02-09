---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
title: Tratamento de simultaneidade com o Entity Framework em um aplicativo MVC ASP.NET (7 de 10) | Microsoft Docs
author: tdykstra
description: O aplicativo Web de exemplo da Contoso University demonstra como criar aplicativos ASP.NET MVC 4 usando o Entity Framework 5 Code First e o Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: b83f47c4-8521-4d0a-8644-e8f77e39733e
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: b7d6ff8d6de27ee0935bc9cb8cc053efb8751d4b
ms.sourcegitcommit: b4cdcf246850751579e45da80c9780fe56330dd0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/09/2021
ms.locfileid: "99985657"
---
# <a name="handling-concurrency-with-the-entity-framework-in-an-aspnet-mvc-application-7-of-10"></a>Manipulando a simultaneidade com o Entity Framework em um aplicativo MVC ASP.NET (7 de 10)

por [Tom Dykstra](https://github.com/tdykstra)

> O aplicativo Web de exemplo da Contoso University demonstra como criar aplicativos ASP.NET MVC 4 usando o Entity Framework 5 Code First e o Visual Studio 2012. Para obter informações sobre a série de tutoriais, consulte [o primeiro tutorial da série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
> 
> > [!NOTE] 
> > 
> > Se você encontrar um problema que não possa resolver, [Baixe o capítulo concluído](building-the-ef5-mvc4-chapter-downloads.md) e tente reproduzir o problema. Em geral, você pode encontrar a solução para o problema comparando seu código com o código concluído. Para alguns erros comuns e como resolvê-los, consulte [erros e soluções alternativas.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)

Nos dois tutoriais anteriores, você trabalhou com os dados relacionados. Este tutorial mostra como lidar com a simultaneidade. Você criará páginas da Web que funcionam com a `Department` entidade, e as páginas que editam e excluem `Department` entidades tratarão erros de simultaneidade. As ilustrações a seguir mostram as páginas index e Delete, incluindo algumas mensagens que são exibidas se ocorrer um conflito de simultaneidade.

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="concurrency-conflicts"></a>Conflitos de simultaneidade

Um conflito de simultaneidade ocorre quando um usuário exibe dados de uma entidade para editá-los e, em seguida, outro usuário atualiza os mesmos dados da entidade antes que a primeira alteração do usuário seja gravada no banco de dados. Se você não habilitar a detecção desses conflitos, a última pessoa que atualizar o banco de dados substituirá as outras alterações do usuário. Em muitos aplicativos, esse risco é aceitável: se houver poucos usuários ou poucas atualizações ou se não for realmente crítico se algumas alterações forem substituídas, o custo de programação para simultaneidade poderá superar o benefício. Nesse caso, você não precisa configurar o aplicativo para lidar com conflitos de simultaneidade.

### <a name="pessimistic-concurrency-locking"></a>Simultaneidade pessimista (bloqueio)

Se o aplicativo precisar evitar a perda acidental de dados em cenários de simultaneidade, uma maneira de fazer isso será usar bloqueios de banco de dados. Isso é chamado de *simultaneidade pessimista*. Por exemplo, antes de ler uma linha de um banco de dados, você solicita um bloqueio para o acesso somente leitura ou de atualização. Se você bloquear uma linha para o acesso de atualização, nenhum outro usuário terá permissão para bloquear a linha para o acesso somente leitura ou de atualização, porque ele obterá uma cópia dos dados que estão sendo alterados. Se você bloquear uma linha para o acesso somente leitura, outros também poderão bloqueá-la para o acesso somente leitura, mas não para atualização.

O gerenciamento de bloqueios traz desvantagens. Ele pode ser complexo de ser programado. Ele requer recursos de gerenciamento de banco de dados significativos e pode causar problemas de desempenho à medida que o número de usuários de um aplicativo aumenta (ou seja, ele não é bem dimensionado). Por esses motivos, nem todos os sistemas de gerenciamento de banco de dados dão suporte à simultaneidade pessimista. O Entity Framework não fornece suporte interno para ele, e este tutorial não mostra como implementá-lo.

### <a name="optimistic-concurrency"></a>Simultaneidade otimista

A alternativa à simultaneidade pessimista é a *simultaneidade otimista*. Simultaneidade otimista significa permitir que conflitos de simultaneidade ocorram e responder adequadamente se eles ocorrerem. Por exemplo, João executa a página Editar departamentos, altera o valor do **orçamento** do departamento em inglês de $350000 para $0.

![Changing_English_dept_budget_to_100000](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Antes de João clicar em **salvar**, Jane executa a mesma página e altera o campo **data de início** de 9/1/2007 para 8/8/2013.

![Changing_English_dept_start_date_to_1999](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

João clica em **salvar** primeiro e vê sua alteração quando o navegador retorna para a página de índice, então Jane clica em **salvar**. O que acontece em seguida é determinado pela forma como você lida com conflitos de simultaneidade. Algumas das opções incluem o seguinte:

- Controle qual propriedade um usuário modificou e atualize apenas as colunas correspondentes no banco de dados. No cenário de exemplo, nenhum dado é perdido, porque propriedades diferentes foram atualizadas pelos dois usuários. Na próxima vez que alguém procurar o departamento em inglês, ele verá as alterações de João e Jane — uma data de início de 8/8/2013 e um orçamento de zero dólares.

    Esse método de atualização pode reduzir a quantidade de conflitos que podem resultar em perda de dados, mas ele não poderá evitar a perda de dados se forem feitas alterações concorrentes à mesma propriedade de uma entidade. Se o Entity Framework funciona dessa maneira depende de como o código de atualização é implementado. Geralmente, isso não é prático em um aplicativo Web, porque pode exigir que você mantenha grandes quantidades de estado para manter o controle de todos os valores de propriedade originais de uma entidade, bem como novos valores. A manutenção de grandes quantidades de estado pode afetar o desempenho do aplicativo porque ele requer recursos do servidor ou deve ser incluído na própria página da Web (por exemplo, em campos ocultos).
- Você pode deixar a alteração de Jane substituir a alteração de João. Na próxima vez que alguém procurar o departamento em inglês, ele verá 8/8/2013 e o valor $350000 restaurado. Isso é chamado de um cenário *O cliente vence* ou *O último vence*. (Os valores do cliente têm precedência sobre o que está no armazenamento de dados.) Conforme observado na introdução a esta seção, se você não fizer qualquer codificação para manipulação de simultaneidade, isso ocorrerá automaticamente.
- Você pode impedir que a alteração de Jane seja atualizada no banco de dados. Normalmente, você exibiria uma mensagem de erro, mostrará o estado atual dos dados e permitirá que ela reaplique as alterações se ainda quiser fazê-las. Isso é chamado de um cenário *O armazenamento vence*. (Os valores de armazenamento de dados têm precedência sobre os valores enviados pelo cliente.) Você implementará o cenário armazenar vence neste tutorial. Esse método garante que nenhuma alteração é substituída sem que um usuário seja alertado sobre o que está acontecendo.

### <a name="detecting-concurrency-conflicts"></a>Detectando conflitos de simultaneidade

Você pode resolver conflitos manipulando exceções [OptimisticConcurrencyException](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) que o Entity Framework gera. Para saber quando gerar essas exceções, o Entity Framework precisa poder detectar conflitos. Portanto, é necessário configurar o banco de dados e o modelo de dados de forma adequada. Algumas opções para habilitar a detecção de conflitos incluem as seguintes:

- Na tabela de banco de dados, inclua uma coluna de acompanhamento que pode ser usada para determinar quando uma linha é alterada. Em seguida, você pode configurar o Entity Framework para incluir essa coluna na `Where` cláusula de SQL `Update` ou `Delete` comandos.

    Normalmente, o tipo de dados da coluna de rastreamento é a [versão](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx)do. O [valor de](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) RowNumber é um número sequencial que é incrementado toda vez que a linha é atualizada. Em um `Update` `Delete` comando ou, a `Where` cláusula inclui o valor original da coluna de controle (a versão de linha original). Se a linha que está sendo atualizada tiver sido alterada por outro usuário, o valor na `rowversion` coluna será diferente do valor original, portanto, `Update` a `Delete` instrução ou não poderá localizar a linha a ser atualizada devido à `Where` cláusula. Quando o Entity Framework descobre que nenhuma linha foi atualizada pelo `Update` `Delete` comando ou (ou seja, quando o número de linhas afetadas é zero), ele interpreta isso como um conflito de simultaneidade.
- Configure o Entity Framework para incluir os valores originais de cada coluna na tabela na `Where` cláusula de `Update` `Delete` comandos e.

    Como na primeira opção, se qualquer coisa na linha for alterada desde que a linha foi lida pela primeira vez, a `Where` cláusula não retornará uma linha a ser atualizada, que o Entity Framework interpretará como um conflito de simultaneidade. Para tabelas de banco de dados que têm muitas colunas, essa abordagem pode resultar em `Where` cláusulas muito grandes e pode exigir que você mantenha grandes quantidades de estado. Conforme observado anteriormente, manter grandes quantidades de estado pode afetar o desempenho do aplicativo porque ele requer recursos do servidor ou deve ser incluído na própria página da Web. Portanto, essa abordagem geralmente não é recomendada e não é o método usado neste tutorial.

    Se você quiser implementar essa abordagem para simultaneidade, será necessário marcar todas as propriedades de chave não primária na entidade para a qual você deseja acompanhar a simultaneidade adicionando o atributo [ConcurrencyCheck](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute.aspx) a elas. Essa alteração permite que a Entity Framework inclua todas as colunas na `WHERE` cláusula SQL de `UPDATE` instruções.

No restante deste tutorial, você adicionará uma propriedade de controle do [doversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) à `Department` entidade, criará um controlador e exibições e testará para verificar se tudo está funcionando corretamente.

## <a name="add-an-optimistic-concurrency-property-to-the-department-entity"></a>Adicionar uma propriedade de simultaneidade otimista à entidade Department

Em *Models\Department.cs*, adicione uma propriedade de rastreamento chamada `RowVersion` :

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=18-19)]

O atributo [timestamp](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx) especifica que essa coluna será incluída na `Where` cláusula de `Update` `Delete` comandos e enviados ao banco de dados. O atributo é chamado [timestamp](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx) porque as versões anteriores do SQL Server usavam um tipo de dados [timestamp](https://msdn.microsoft.com/library/ms182776(v=SQL.90).aspx) SQL antes que a [versão](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) do SQL seja substituída. O tipo .net para `rowversion` é uma matriz de bytes. Se preferir usar a API fluente, você poderá usar o método [IsConcurrencyToken](https://msdn.microsoft.com/library/gg679501(v=VS.103).aspx) para especificar a propriedade de rastreamento, conforme mostrado no exemplo a seguir:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Consulte o problema do GitHub [substituir IsConcurrencyToken por IsRowVersion](https://github.com/dotnet/AspNetDocs/issues/302).

Adicionando uma propriedade, você alterou o modelo de banco de dados e, portanto, precisa fazer outra migração. No PMC (Console do Gerenciador de Pacotes), Insira os seguintes comandos:

[!code-console[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cmd)]

## <a name="create-a-department-controller"></a>Criar um controlador de departamento

Crie um `Department` controlador e exibições da mesma maneira que fez com os outros controladores, usando as seguintes configurações:

![Add_Controller_dialog_box_for_Department_controller](https://asp.net/media/2578041/Windows-Live-Writer_Handling-C.NET-MVC-Application-7-of-10h1_AFDC_Add_Controller_dialog_box_for_Department_controller_d1d9c788-f970-4d6a-9f5a-1eddc84330b7.png)

Em *Controllers\DepartmentController.cs*, adicione uma `using` instrução:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

Altere "LastName" para "FullName" em todos os lugares neste arquivo (quatro ocorrências) para que as listas suspensas do administrador do departamento contenham o nome completo do instrutor, e não apenas o sobrenome.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=1)]

Substitua o código existente para o `HttpPost` `Edit` método pelo código a seguir:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

A exibição armazenará o `RowVersion` valor original em um campo oculto. Quando o associador de modelo cria a `department` instância, esse objeto terá o `RowVersion` valor da propriedade original e os novos valores para as outras propriedades, conforme inserido pelo usuário na página Editar. Em seguida, quando o Entity Framework criar um `UPDATE` comando SQL, esse comando incluirá uma `WHERE` cláusula que procura uma linha que tenha o `RowVersion` valor original.

Se nenhuma linha for afetada pelo `UPDATE` comando (nenhuma linha tem o `RowVersion` valor original), o Entity Framework lançará uma `DbUpdateConcurrencyException` exceção e o código no `catch` bloco obterá a entidade afetada `Department` do objeto de exceção. Essa entidade tem os valores lidos do banco de dados e os novos valores inseridos pelo usuário:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Em seguida, o código adiciona uma mensagem de erro personalizada para cada coluna que tem valores de banco de dados diferentes do que o usuário inseriu na página de edição:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Uma mensagem de erro mais longa explica o que aconteceu e o que fazer sobre ele:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Por fim, o código define o `RowVersion` valor do `Department` objeto para o novo valor recuperado do banco de dados. Esse novo valor `RowVersion` será armazenado no campo oculto quando a página Editar for exibida novamente, e na próxima vez que o usuário clicar em **Salvar**, somente os erros de simultaneidade que ocorrem desde a nova exibição da página Editar serão capturados.

Em *Views\Department\Edit.cshtml*, adicione um campo oculto para salvar o `RowVersion` valor da propriedade, imediatamente após o campo oculto para a `DepartmentID` Propriedade:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cshtml?highlight=17)]

No *Views\Department\Index.cshtml*, substitua o código existente pelo código a seguir para mover os links de linha para a esquerda e alterar o título da página e os títulos de coluna a serem exibidos em `FullName` vez de `LastName` na coluna **administrador** :

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cshtml)]

## <a name="testing-optimistic-concurrency-handling"></a>Testando a manipulação de simultaneidade otimista

Execute o site e clique em **departamentos**:

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Clique com o botão direito do mouse no hiperlink **Editar** para Kim Abercrombie e selecione **abrir em nova guia e,** em seguida, clique no hiperlink **Editar** para Kim Abercrombie. As duas janelas exibem as mesmas informações.

![Department_Edit_page_before_changes](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Altere um campo na primeira janela do navegador e clique em **salvar**.

![Department_Edit_page_1_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

O navegador mostra a página Índice com o valor alterado.

![Departments_Index_page_after_first_budget_edit](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Altere o campo qualquer na segunda janela do navegador e clique em **salvar**.

![Department_Edit_page_2_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

Clique em **salvar** na segunda janela do navegador. Você verá uma mensagem de erro:

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

Clique em **Salvar** novamente. O valor que você inseriu no segundo navegador é salvo junto com o valor original dos dados que você altera no primeiro navegador. Você verá os valores salvos quando a página Índice for exibida.

![Department_Index_page_with_change_from_second_browser](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)

## <a name="updating-the-delete-page"></a>Atualizando a página de exclusão

Para a página Excluir, o Entity Framework detecta conflitos de simultaneidade causados pela edição por outra pessoa do departamento de maneira semelhante. Quando o `HttpGet` `Delete` método exibe o modo de exibição de confirmação, o modo de exibição inclui o `RowVersion` valor original em um campo oculto. Esse valor é disponibilizado para o `HttpPost` `Delete` método que é chamado quando o usuário confirma a exclusão. Quando o Entity Framework cria o `DELETE` comando SQL, ele inclui uma `WHERE` cláusula com o `RowVersion` valor original. Se o comando resultar em zero linhas afetadas (o que significa que a linha foi alterada após a exibição da página de confirmação de exclusão), uma exceção de simultaneidade será gerada e o `HttpGet Delete` método será chamado com um sinalizador de erro definido como para `true` reexibir a página de confirmação com uma mensagem de erro. Também é possível que zero linhas tenham sido afetadas porque a linha foi excluída por outro usuário; portanto, nesse caso, uma mensagem de erro diferente é exibida.

No *DepartmentController.cs*, substitua o `HttpGet` `Delete` método pelo código a seguir:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]

O método aceita um parâmetro opcional que indica se a página está sendo exibida novamente após um erro de simultaneidade. Se esse sinalizador for `true` , uma mensagem de erro será enviada para a exibição usando uma `ViewBag` propriedade.

Substitua o código no `HttpPost` `Delete` método (chamado `DeleteConfirmed` ) pelo seguinte código:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

No código gerado por scaffolding que acabou de ser substituído, esse método aceitou apenas uma ID de registro:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

Você alterou esse parâmetro para uma `Department` instância de entidade criada pelo associador de modelo. Isso lhe dá acesso ao `RowVersion` valor da propriedade além da chave de registro.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

Você também alterou o nome do método de ação de `DeleteConfirmed` para `Delete`. O código com Scaffold nomeado o `HttpPost` `Delete` método `DeleteConfirmed` para dar ao `HttpPost` método uma assinatura exclusiva. (O CLR requer métodos sobrecarregados para ter parâmetros de método diferentes.) Agora que as assinaturas são exclusivas, você pode se manter com a Convenção MVC e usar o mesmo nome para os `HttpPost` `HttpGet` métodos e excluir.

Se um erro de simultaneidade é capturado, o código exibe novamente a página Confirmação de exclusão e fornece um sinalizador que indica que ela deve exibir uma mensagem de erro de simultaneidade.

No *Views\Department\Delete.cshtml*, substitua o código com Scaffold pelo código a seguir que faz algumas alterações de formatação e adiciona um campo de mensagem de erro. As alterações são realçadas.

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml?highlight=9,37,40,45-46)]

Esse código adiciona uma mensagem de erro entre `h2` os `h3` cabeçalhos e:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

Ele substitui `LastName` pelo `FullName` no `Administrator` campo:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml)]

Por fim, ele adiciona campos ocultos para as `DepartmentID` `RowVersion` Propriedades e após a `Html.BeginForm` instrução:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]

Execute a página de índice de departamentos. Clique com o botão direito do mouse no hiperlink **excluir** do departamento em inglês e selecione **abrir em nova janela e,** na primeira janela, clique no hiperlink **Editar** para o departamento em inglês.

Na primeira janela, altere um dos valores e clique em **salvar** :

![Department_Edit_page_after_change_before_delete](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

A página de índice confirma a alteração.

![Departments_Index_page_after_budget_edit_before_delete](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image13.png)

Na segunda janela, clique em **excluir**.

![Department_Delete_confirmation_page_before_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image14.png)

Você verá a mensagem de erro de simultaneidade e os valores de Departamento serão atualizados com o que está atualmente no banco de dados.

![Department_Delete_confirmation_page_with_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)

Se você clicar em **Excluir** novamente, será redirecionado para a página Índice, que mostra que o departamento foi excluído.

## <a name="summary"></a>Resumo

Isso conclui a introdução à manipulação de conflitos de simultaneidade. Para obter informações sobre outras maneiras de lidar com vários cenários de simultaneidade, consulte [padrões de simultaneidade otimista](https://blogs.msdn.com/b/adonet/archive/2011/02/03/using-dbcontext-in-ef-feature-ctp5-part-9-optimistic-concurrency-patterns.aspx) e [trabalhando com valores de propriedade](https://blogs.msdn.com/b/adonet/archive/2011/01/30/using-dbcontext-in-ef-feature-ctp5-part-5-working-with-property-values.aspx) no blog da equipe do Entity Framework. O próximo tutorial mostra como implementar a herança de tabela por hierarquia para as `Instructor` entidades e `Student` .

Links para outros recursos de Entity Framework podem ser encontrados no [mapa de conteúdo de acesso a dados do ASP.net](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Anterior](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) 
>  [Avançar](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
