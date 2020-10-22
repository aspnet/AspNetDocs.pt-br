---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/form-basics
title: Introdução ao Páginas da Web do ASP.NET-noções básicas de formulário HTML | Microsoft Docs
author: Rick-Anderson
description: Este tutorial mostra as noções básicas de como criar um formulário de entrada e como tratar a entrada do usuário quando você usa Páginas da Web do ASP.NET (Razor). E agora que você...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: 81ed82bf-b940-44f1-b94a-555d0cb7cc98
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/form-basics
msc.type: authoredcontent
ms.openlocfilehash: f57661077ec3bb13f3d4ec41b130bda4d2fb9070
ms.sourcegitcommit: c62ec20b453cee3249eb894ecd75013b57d078f0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/21/2020
ms.locfileid: "92345452"
---
# <a name="introducing-aspnet-web-pages---html-form-basics"></a>Introdução ao Páginas da Web do ASP.NET-noções básicas de formulários HTML

 por [Tom FitzMacken](https://github.com/tfitzmac)

> Este tutorial mostra as noções básicas de como criar um formulário de entrada e como tratar a entrada do usuário quando você usa Páginas da Web do ASP.NET (Razor). E agora que você tem um banco de dados, você usará suas habilidades de formulário para permitir que os usuários localizem filmes específicos no banco de dados. Ele pressupõe que você concluiu a série por meio da [introdução à exibição de dados usando páginas da Web do ASP.net](/aspnet/web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data).
> 
> O que você aprenderá:
> 
> - Como criar um formulário usando elementos HTML padrão.
> - Como ler a entrada do usuário em um formulário.
> - Como criar uma consulta SQL que obtém dados seletivamente usando um termo de pesquisa que o usuário fornece.
> - Como os campos na página "lembram" o que o usuário inseriu.
>   
> 
> Recursos/tecnologias abordados:
> 
> - O objeto `Request`.
> - A `Where` cláusula SQL.

## <a name="what-youll-build"></a>O que você vai construir

No tutorial anterior, você criou um banco de dados, adicionou um dado a ele e, em seguida, usou o `WebGrid` auxiliar para exibir os dados. Neste tutorial, você adicionará uma caixa de pesquisa que permite encontrar filmes de um gênero específico ou cujo título contém qualquer palavra que você inserir. (Por exemplo, você poderá localizar todos os filmes cujo gênero seja "ação" ou cujo título contenha "Jaime" ou "aventura".)

Quando você terminar este tutorial, terá uma página como esta:

![Página de filmes com a pesquisa de gênero e de título](form-basics/_static/image1.png)

A parte de listagem da página é a mesma do último tutorial &mdash; de uma grade. A diferença será que a grade mostrará apenas os filmes que você pesquisou.

## <a name="about-html-forms"></a>Sobre formulários HTML

(Se você tem experiência com a criação de formulários HTML e com a diferença entre `GET` e `POST` , você pode ignorar esta seção.)

Um formulário tem caixas de texto de elementos de entrada do usuário &mdash; , botões, botões de opção, caixas de seleção, listas suspensas e assim por diante. Os usuários preenchem esses controles ou fazem seleções e, em seguida, enviam o formulário clicando em um botão.

A sintaxe HTML básica de um formulário é ilustrada por este exemplo:

[!code-html[Main](form-basics/samples/sample1.html)]

Quando essa marcação é executada em uma página, ela cria um formulário simples semelhante a esta ilustração:

![Formulário HTML básico como processado no navegador](form-basics/_static/image2.png)

O `<form>` elemento inclui os elementos HTML a serem enviados. (Um equívoco fácil de fazer é adicionar elementos à página, mas, em seguida, esquecer de colocá-los dentro de um `<form>` elemento. Nesse caso, nada é enviado.) O `method` atributo informa ao navegador como enviar a entrada do usuário. Você define isso como `post` se estiver executando uma atualização no servidor ou `get` se estiver apenas buscando dados do servidor.

<a id="GET,_POST,_and_HTTP_Verb_Safety"></a>

> [!TIP] 
> 
> **Segurança de verbo GET, POST e HTTP**
> 
> O HTTP, o protocolo que os navegadores e os servidores usam para trocar informações, é notavelmente simples em suas operações básicas. Os navegadores usam apenas alguns verbos para fazer solicitações aos servidores. Quando você escreve o código para a Web, é útil entender esses verbos e como o navegador e o servidor os usam. Os verbos mais comumente usados são:
> 
> - `GET`. O navegador usa esse verbo para buscar algo do servidor. Por exemplo, quando você digita uma URL em seu navegador, o navegador executa uma `GET` operação para solicitar a página desejada. Se a página incluir elementos gráficos, o navegador executará `GET` operações adicionais para obter as imagens. Se a `GET` operação tiver que passar informações para o servidor, as informações serão passadas como parte da URL na cadeia de caracteres de consulta.
> - `POST`. O navegador envia uma `POST` solicitação para enviar dados a serem adicionados ou alterados no servidor. Por exemplo, o `POST` verbo é usado para criar registros em um banco de dados ou alterar os existentes. Na maioria das vezes, quando você preenche um formulário e clica no botão enviar, o navegador executa uma `POST` operação. Em uma `POST` operação, os dados que estão sendo passados para o servidor estão no corpo da página.
> 
> Uma distinção importante entre esses verbos é que uma `GET` operação não deve alterar nada no servidor — ou colocá-lo de forma ligeiramente mais abstrata, uma `GET` operação não resulta em uma alteração no estado no servidor. Você pode executar uma `GET` operação nos mesmos recursos quantas vezes desejar, e esses recursos não são alterados. ( `GET` Muitas vezes, uma operação é considerada "segura" ou usar um termo técnico, é *idempotente*.) Por outro lado, `POST` é claro que uma solicitação altera algo no servidor cada vez que você executa a operação.
> 
> Dois exemplos ajudarão a ilustrar essa distinção. Ao executar uma pesquisa usando um mecanismo como o Bing ou o Google, você preenche um formulário que consiste em uma caixa de texto e clica no botão Pesquisar. O navegador executa uma `GET` operação, com o valor que você inseriu na caixa passada como parte da URL. O uso de uma `GET` operação para esse tipo de formulário está bom, porque uma operação de pesquisa não altera nenhum recurso no servidor, ele apenas busca informações.
> 
> Agora, considere o processo de ordenar algo online. Preencha os detalhes do pedido e, em seguida, clique no botão enviar. Essa operação será uma `POST` solicitação, pois a operação resultará em alterações no servidor, como um novo registro de pedido, uma alteração nas informações da conta e, talvez, muitas outras alterações. Ao contrário da `GET` operação, não é possível repetir a `POST` solicitação — se você fez isso, sempre que reenviou a solicitação, você geraria um novo pedido no servidor. (Em casos como esse, os sites geralmente avisam que você não deve clicar em um botão de envio mais de uma vez ou desabilitará o botão enviar para que você não reenvie o formulário acidentalmente.)
> 
> No decorrer deste tutorial, você usará uma `GET` operação e uma `POST` operação para trabalhar com formulários HTML. Explicaremos em cada caso por que o verbo usado é o apropriado.
> 
> (Para saber mais sobre verbos HTTP, consulte o artigo [definições de método](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) no site do W3C.)

A maioria dos elementos de entrada do usuário são `<input>` elementos HTML. Eles se parecem `<input type="type" name="name">,` onde o *tipo* indica o tipo de controle de entrada do usuário desejado. Esses elementos são os comuns:

- Caixa de texto: `<input type="text">`
- Caixa de seleção: `<input type="check">`
- Botão de opção: `<input type="radio">`
- Button `<input type="button">`
- Botão enviar: `<input type="submit">`

Você também pode usar o `<textarea>` elemento para criar uma caixa de texto de várias linhas e o `<select>` elemento para criar uma lista suspensa ou lista rolável. (Para obter mais informações sobre elementos de formulário HTML, consulte [formulários HTML e entrada](http://www.w3schools.com/html/html_forms.asp) no site W3Schools.)

O `name` atributo é muito importante, pois o nome é como você obterá o valor do elemento mais tarde, como você verá em breve.

A parte interessante é o que você, o desenvolvedor de páginas, faz com a entrada do usuário. Não há nenhum comportamento interno associado a esses elementos. Em vez disso, você precisa obter os valores que o usuário inseriu ou selecionou e faz algo com eles. É isso que você aprenderá neste tutorial.

> [!TIP] 
> 
> **HTML5 e formulários de entrada**
> 
> Como você deve saber, o HTML está em transição e a versão mais recente (HTML5) inclui suporte para maneiras mais intuitivas para os usuários inserirem informações. Por exemplo, no HTML5, você (o desenvolvedor da página) pode informar à página que você deseja que o usuário insira uma data. O navegador pode exibir automaticamente um calendário em vez de exigir que o usuário insira uma data manualmente. No entanto, o HTML5 é novo e ainda não tem suporte em todos os navegadores.
> 
> Páginas da Web do ASP.NET dá suporte à entrada HTML5 na medida que o navegador do usuário faz. Para obter uma ideia dos novos atributos para o `<input>` elemento no HTML5, consulte [ &lt; atributo de &gt; tipo de entrada HTML](http://www.w3schools.com/html/html_form_input_types.asp) no site W3Schools.

## <a name="creating-the-form"></a>Criando o formulário

No WebMatrix, no espaço de trabalho **arquivos** , abra a página *Movies. cshtml* .

Após a marca de fechamento `</h1>` e antes da `<div>` marca de abertura da `grid.GetHtml` chamada, adicione a seguinte marcação:

[!code-html[Main](form-basics/samples/sample2.html)]

Essa marcação cria um formulário que tem uma caixa de texto chamada `searchGenre` e um botão enviar. A caixa de texto e o botão enviar são colocados em um `<form>` elemento cujo `method` atributo está definido como `get` . (Lembre-se de que, se você não colocar a caixa de texto e enviar o botão dentro de um `<form>` elemento, nada será enviado quando você clicar no botão.) Você usa o `GET` verbo aqui porque está criando um formulário que não faz nenhuma alteração no servidor — ele simplesmente resulta em uma pesquisa. (No tutorial anterior, você usou um `post` método, que é como você envia as alterações para o servidor. Você verá isso no próximo tutorial novamente.)

Execute a página. Embora você não tenha definido nenhum comportamento para o formulário, você pode ver como ele se parece com:

![Página de filmes com caixa de pesquisa para gênero](form-basics/_static/image3.png)

Insira um valor na caixa de texto, como "comédia". Em seguida, clique em **Pesquisar gênero**.

Anote a URL da página. Como você define o `<form>` atributo do elemento `method` como `get` , o valor que você inseriu agora é parte da cadeia de caracteres de consulta na URL, desta forma:

`http://localhost:45661/Movies.cshtml?searchGenre=Comedy`

## <a name="reading-form-values"></a>Lendo valores de formulário

A página já contém algum código que obtém os dados do banco e exibe os resultados em uma grade. Agora, você precisa adicionar algum código que leia o valor da caixa de texto para que você possa executar uma consulta SQL que inclui o termo de pesquisa.

Como você define o método do formulário como `get` , pode ler o valor inserido na caixa de texto usando um código semelhante ao seguinte:

`var searchTerm = Request.QueryString["searchGenre"];`

O `Request.QueryString` objeto (a `QueryString` Propriedade do `Request` objeto) inclui os valores dos elementos que foram enviados como parte da `GET` operação. A `Request.QueryString` propriedade contém uma *coleção* (uma lista) dos valores que são enviados no formulário. Para obter qualquer valor individual, especifique o nome do elemento desejado. É por isso que você precisa ter um `name` atributo no `<input>` elemento ( `searchTerm` ) que cria a caixa de texto. (Para obter mais informações sobre o `Request` objeto, consulte a [barra lateral](#BKMK_TheRequestObject) mais tarde.)

É simples o suficiente para ler o valor da caixa de texto. Mas se o usuário não inseriu nada na caixa de texto, mas clicou em **Pesquisar** mesmo assim, você pode ignorar esse clique, já que não há nada a ser pesquisado.

O código a seguir é um exemplo que mostra como implementar essas condições. (Você não precisa adicionar esse código ainda; você fará isso daqui a pouco.)

[!code-csharp[Main](form-basics/samples/sample3.cs)]

O teste é dividido desta forma:

- Obtenha o valor de `Request.QueryString["searchGenre"]` , ou seja, o valor que foi inserido no `<input>` elemento chamado `searchGenre` .
- Descubra se ele está vazio usando o `IsEmpty` método. Esse método é a maneira padrão de determinar se algo (por exemplo, um elemento form) contém um valor. Mas, na verdade, você se preocupa apenas se *não* estiver vazio, portanto...
- Adicione o `!` operador na frente do `IsEmpty` teste. (O `!` operador significa lógico não).

Em inglês simples, a `if` condição inteira se traduz no seguinte: *se o elemento searchGenre do formulário não estiver vazio, então.* ..

Esse bloco define o estágio para criar uma consulta que usa o termo de pesquisa. Você fará isso na próxima seção.

<a id="BKMK_TheRequestObject"></a>

> [!TIP] 
> 
> **O objeto de solicitação**
> 
> O `Request` objeto contém todas as informações que o navegador envia para seu aplicativo quando uma página é solicitada ou enviada. Esse objeto inclui todas as informações que o usuário fornece, como valores de caixa de texto ou um arquivo para carregar. Ele também inclui todos os tipos de informações adicionais, como cookies, valores na cadeia de caracteres de consulta de URL (se houver), o caminho do arquivo da página em execução, o tipo de navegador que o usuário está usando, a lista de idiomas que estão definidos no navegador e muito mais.
> 
> O `Request` objeto é uma *coleção* (lista) de valores. Você Obtém um valor individual da coleção especificando seu nome:
> 
> `var someValue = Request["name"];`
> 
> O `Request` objeto realmente expõe vários subconjuntos. Por exemplo:
> 
> - `Request.Form` Fornece valores de elementos dentro do elemento enviado `<form>` se a solicitação for uma `POST` solicitação.
> - `Request.QueryString` fornece apenas os valores na cadeia de caracteres de consulta da URL. (Em uma URL como `http://mysite/myapp/page?searchGenre=action&page=2` , a `?searchGenre=action&page=2` seção da URL é a cadeia de caracteres de consulta.)
> - `Request.Cookies` a coleção fornece acesso a cookies que o navegador enviou.
> 
> Para obter um valor que você sabe que está no formulário enviado, você pode usar `Request["name"]` . Como alternativa, você pode usar as versões mais específicas `Request.Form["name"]` (para `POST` solicitações) ou `Request.QueryString["name"]` (para `GET` solicitações). É claro que *Name* é o nome do item a ser obtido.
> 
> O nome do item que você deseja obter deve ser exclusivo na coleção que você está usando. É por isso que o `Request` objeto fornece os subconjuntos como `Request.Form` e `Request.QueryString` . Suponha que sua página contenha um elemento de formulário chamado `userName` e *também* contenha um cookie chamado `userName` . Se você obtiver `Request["userName"]` , será ambíguo se você quiser o valor do formulário ou o cookie. No entanto, se você obtiver `Request.Form["userName"]` ou `Request.Cookie["userName"]` , você está sendo explícito sobre qual valor obter.
> 
> É uma boa prática ser específica e usar o subconjunto do `Request` que você está interessado, como `Request.Form` ou `Request.QueryString` . Para as páginas simples que você está criando neste tutorial, ele provavelmente não faz nenhuma diferença. No entanto, à medida que você cria páginas mais complexas, usando a versão explícita `Request.Form` ou `Request.QueryString` pode ajudá-lo a evitar problemas que podem surgir quando a página contém um formulário (ou vários formulários), cookies, valores de cadeia de caracteres de consulta e assim por diante.

## <a name="creating-a-query-by-using-a-search-term"></a>Criando uma consulta usando um termo de pesquisa

Agora que você sabe como obter o termo de pesquisa que o usuário inseriu, você pode criar uma consulta que o utiliza. Lembre-se de que, para obter todos os itens de filme do banco de dados, você está usando uma consulta SQL parecida com esta instrução:

`SELECT * FROM Movies`

Para obter apenas alguns filmes, você precisa usar uma consulta que inclua uma `Where` cláusula. Essa cláusula permite definir uma condição na qual as linhas são retornadas pela consulta. Aqui está um exemplo:

`SELECT * FROM Movies WHERE Genre = 'Action'`

O formato básico é `WHERE column = value` . Você pode usar operadores diferentes, além de apenas `=` , como `>` (maior que), `<` (menor que), ( `<>` diferente de), `<=` (menor que ou igual a), etc., dependendo do que você está procurando.

Caso você esteja imaginando, as instruções SQL não diferenciam maiúsculas de minúsculas são &mdash; `SELECT` as mesmas que `Select` (ou até mesmo `select` ). No entanto, as pessoas frequentemente capitalizam palavras-chave em uma instrução SQL, como `SELECT` e `WHERE` , para facilitar a leitura.

### <a name="passing-the-search-term-as-a-parameter"></a>Passando o termo de pesquisa como um parâmetro

Pesquisar por um gênero específico é fácil o suficiente ( `WHERE Genre = 'Action'` ), mas você deseja ser capaz de pesquisar qualquer gênero que o usuário inserir. Para fazer isso, você cria como consulta SQL que inclui um espaço reservado para o valor a ser pesquisado. Ele se parecerá com este comando:

`SELECT * FROM Movies WHERE Genre = @0`

O espaço reservado é o `@` caractere seguido por zero. Como você pode imaginar, uma consulta pode conter vários espaços reservados, e eles seriam nomeados `@0` , `@1` , `@2` , etc.

Para configurar a consulta e, na verdade, passá-la para o valor, use o código como o seguinte:

[!code-sql[Main](form-basics/samples/sample4.sql)]

Esse código é semelhante ao que você já fez para exibir dados na grade. As únicas diferenças são:

- A consulta contém um espaço reservado ( `WHERE Genre = @0"` ).
- A consulta é colocada em uma variável ( `selectCommand` ); antes de, você passou a consulta diretamente para o `db.Query` método.
- Ao chamar o `db.Query` método, você passa a consulta e o valor a ser usado para o espaço reservado. (Se a consulta tivesse vários espaços reservados, você os passaria como valores separados para o método.)

Se você colocar todos esses elementos juntos, obterá o seguinte código:

[!code-csharp[Main](form-basics/samples/sample5.cs)]

> [!NOTE] 
> 
> **Importante!** O uso de espaços reservados (como `@0` ) para passar valores para um comando SQL é *extremamente importante* para a segurança. A maneira como você o vê aqui, com espaços reservados para dados variáveis, é a única maneira de construir comandos SQL.
> 
> Nunca Construa uma instrução SQL reunindo (concatenando) o texto literal e os valores obtidos do usuário. Concatenar a entrada do usuário em uma instrução SQL abre seu site para um *ataque de injeção de SQL* em que um usuário mal-intencionado envia valores para sua página que atacam seu banco de dados. (Você pode ler mais no artigo [injeção de SQL](https://msdn.microsoft.com/library/ms161953.aspx) no site do MSDN.)

## <a name="updating-the-movies-page-with-search-code"></a>Atualizando a página de filmes com o código de pesquisa

Agora você pode atualizar o código no arquivo *Movies. cshtml* . Para começar, substitua o código no bloco de código na parte superior da página por este código:

[!code-csharp[Main](form-basics/samples/sample6.cs)]

A diferença aqui é que você colocou a consulta na `selectCommand` variável, que você passará para o `db.Query` mais tarde. Colocar a instrução SQL em uma variável permite que você altere a instrução, que é o que você fará para executar a pesquisa.

Você também removeu essas duas linhas, as quais você colocará de volta mais tarde:

[!code-csharp[Main](form-basics/samples/sample7.cs)]

Você não deseja executar a consulta ainda (ou seja, chamar `db.Query` ) e não quer inicializar o `WebGrid` auxiliar ainda. Você fará essas coisas depois de descobrir qual instrução SQL precisa ser executada.

Após esse bloco reescrito, você pode adicionar a nova lógica para manipular a pesquisa. O código completo se parecerá com o seguinte. Atualize o código em sua página para que ele corresponda a este exemplo:

[!code-cshtml[Main](form-basics/samples/sample8.cshtml)]

A página agora funciona como esta. Toda vez que a página é executada, o código abre o banco de dados e a `selectCommand` variável é definida para a instrução SQL que obtém todos os registros da `Movies` tabela. O código também inicializa a `searchTerm` variável.

No entanto, se a solicitação atual incluir um valor para o `searchGenre` elemento, o código `selectCommand` será definido como uma consulta diferente — ou seja, para um que inclua a `Where` cláusula para pesquisar um gênero. Ele também define `searchTerm` como o que foi passado para a caixa de pesquisa (que pode ser Nothing).

Independentemente de qual instrução SQL estiver `selectCommand` , o código então chama `db.Query` para executar a consulta, passando-a para o que estiver `searchTerm` . Se não houver nada no `searchTerm` , não importa porque, nesse caso, não há nenhum parâmetro para passar o valor para `selectCommand` mesmo assim.

Por fim, o código inicializa o `WebGrid` auxiliar usando os resultados da consulta, assim como antes.

Você pode ver que, colocando a instrução SQL e o termo de pesquisa em variáveis, você adicionou flexibilidade ao código. Como você verá mais adiante neste tutorial, você pode usar essa estrutura básica e continuar adicionando lógica para diferentes tipos de pesquisas.

## <a name="testing-the-search-by-genre-feature"></a>Testando o recurso Pesquisar por gênero

No WebMatrix, execute a página *Movies. cshtml* . Você vê a página com a caixa de texto para gênero.

Insira um gênero que você inseriu para um dos seus registros de teste e clique em **Pesquisar**. Desta vez, você verá uma listagem apenas dos filmes que correspondem a esse gênero:

![Listagem de filmes página depois de procurar o gênero ' Comedies '](form-basics/_static/image4.png)

Insira um gênero diferente e pesquise novamente. Tente inserir o gênero usando todas as letras maiúsculas ou minúsculas para que você possa ver que a pesquisa não diferencia maiúsculas de minúsculas.

## <a name="remembering-what-the-user-entered"></a>"Lembrando" o que o usuário inseriu

Você deve ter notado que depois de inserir um gênero e clicar em **Pesquisar gênero**, você viu uma listagem para esse gênero. No entanto, a caixa de texto de pesquisa estava vazia &mdash; em outras palavras, não se lembrava do que você inseriu.

É importante entender por que esse comportamento ocorre. Quando você envia uma página, o navegador envia uma solicitação ao servidor Web. Quando o ASP.NET obtém a solicitação, ele cria uma instância totalmente nova da página, executa o código nela e, em seguida, renderiza a página para o navegador novamente. No entanto, na verdade, a página não sabe que você estava apenas trabalhando com uma versão anterior de si mesma. Tudo o que ele sabe é que ele recebeu uma solicitação que tinha alguns dados de formulário.

Sempre que você solicitar uma página &mdash; , seja pela primeira vez ou enviando-a, &mdash; receberá uma nova página. O servidor Web não tem memória de sua última solicitação. Nenhuma das ASP.NET, e nenhuma delas faz o navegador. A única conexão entre essas instâncias separadas da página é qualquer dado que você transmite entre elas. Se você enviar uma página, por exemplo, a nova instância de página poderá obter os dados do formulário que foram enviados pela instância anterior. (Outra maneira de passar dados entre páginas é usar cookies.)

Uma maneira formal de descrever essa situação é dizer que as páginas da Web são *sem estado*. Os servidores Web e as próprias páginas e os elementos na página não mantêm nenhuma informação sobre o estado anterior de uma página. A Web foi projetada dessa forma porque manter o estado das solicitações individuais esgotaria rapidamente os recursos dos servidores Web, que geralmente lidam com milhares, talvez até centenas de milhares de solicitações por segundo.

É por isso que a caixa de texto estava vazia. Depois de enviar a página, o ASP.NET criou uma nova instância da página e executou o código e a marcação. Não havia nada no código que disse ASP.NET para colocar um valor na caixa de texto. Portanto, o ASP.NET não fez nada e a caixa de texto foi renderizada sem um valor nele.

Na verdade, há uma maneira fácil de contornar esse problema. O gênero que você inseriu na caixa de texto *está* disponível para você no código em que está &mdash; `Request.QueryString["searchGenre"]` .

Atualize a marcação da caixa de texto para que o `value` atributo Obtenha seu valor de `searchTerm` , como neste exemplo:

[!code-html[Main](form-basics/samples/sample9.html?highlight=1)]

Nesta página, você também poderia definir o `value` atributo para a `searchTerm` variável, já que essa variável também contém o gênero que você inseriu. Mas usar o `Request` objeto para definir o `value` atributo, conforme mostrado aqui, é a maneira padrão de realizar essa tarefa. (Supondo que você ainda queira fazer isso &mdash; em algumas situações, talvez você queira renderizar a página *sem* valores nos campos. Tudo depende do que está acontecendo com seu aplicativo.)

> [!NOTE]
> Não é possível "lembrar" o valor de uma caixa de texto usada para senhas. Seria uma brecha de segurança para permitir que as pessoas preencham um campo de senha usando código.

Execute a página novamente, insira um gênero e clique em **Pesquisar gênero**. Dessa vez, não só você vê os resultados da pesquisa, mas a caixa de texto lembra o que você inseriu pela última vez:

![Página mostrando que a caixa de texto tem ' lembrado ' da entrada anterior](form-basics/_static/image5.png)

## <a name="searching-for-any-word-in-the-title"></a>Pesquisando qualquer palavra no título

Agora você pode pesquisar por qualquer gênero, mas também convém procurar um título. É difícil obter um título exatamente à direita ao pesquisar, então você pode pesquisar uma palavra que aparece em qualquer lugar dentro de um título. Para fazer isso no SQL, você usa o `LIKE` operador e a sintaxe como a seguinte:

`SELECT * FROM Movies WHERE Title LIKE '%adventure%'`

Esse comando obtém todos os filmes cujos títulos contêm "Adventure". Ao usar o `LIKE` operador, você inclui o caractere curinga `%` como parte do termo de pesquisa. A pesquisa `LIKE 'adventure%'` significa "começando com ' Adventure '". (Tecnicamente, isso significa "a cadeia de caracteres ' aventura ' seguida de qualquer coisa.") Da mesma forma, o termo de pesquisa `LIKE '%adventure'` significa "qualquer coisa seguida pela cadeia de caracteres ' Adventure '", que é outra maneira de dizer "terminando com ' Adventure '".

Portanto, o termo de pesquisa `LIKE '%adventure%'` significa "com ' Adventure ' em qualquer lugar no título." (Tecnicamente, "qualquer coisa no título, seguido de ' Adventure ', seguido de qualquer coisa.")

Dentro do `<form>` elemento, adicione a seguinte marcação logo abaixo da marca de fechamento `</div>` da pesquisa de gênero (logo antes do `</form>` elemento de fechamento):

[!code-html[Main](form-basics/samples/sample10.html)]

O código para lidar com essa pesquisa é semelhante ao código da pesquisa de gênero, exceto pelo fato de que você precisa montar a `LIKE` pesquisa. Dentro do bloco de código na parte superior da página, adicione esse `if` bloco logo após o `if` bloco para a pesquisa de Gênero:

[!code-csharp[Main](form-basics/samples/sample11.cs)]

Esse código usa a mesma lógica que você viu anteriormente, exceto que a pesquisa usa um `LIKE` operador e o código coloca " `%` " antes e depois do termo de pesquisa.

Observe como é fácil adicionar outra pesquisa à página. Tudo o que você precisava fazer era:

- Crie um `if` bloco que foi testado para ver se a caixa de pesquisa relevante tinha um valor.
- Defina a `selectCommand` variável como uma nova instrução SQL.
- Defina a `searchTerm` variável para o valor a ser passado para a consulta.

Aqui está o bloco de código completo, que contém a nova lógica para uma pesquisa de título:

[!code-cshtml[Main](form-basics/samples/sample12.cshtml)]

Veja a seguir um resumo do que esse código faz:

- As variáveis `searchTerm` e `selectCommand` são inicializadas na parte superior. Você vai definir essas variáveis para o termo de pesquisa apropriado (se houver) e o comando SQL apropriado com base no que o usuário faz na página. A pesquisa padrão é o caso simples de obter todos os filmes do banco de dados.
- Nos testes para `searchGenre` e `searchTitle` , o código define `searchTerm` como o valor que você deseja pesquisar. Esses blocos de código também são definidos `selectCommand` como um comando SQL apropriado para essa pesquisa.
- O `db.Query` método é invocado apenas uma vez, usando qualquer comando SQL que esteja em `selectedCommand` e qualquer valor em `searchTerm` . Se não houver nenhum termo de pesquisa (sem gênero e nenhuma palavra de título), o valor de `searchTerm` será uma cadeia de caracteres vazia. No entanto, isso não importa, porque nesse caso a consulta não requer um parâmetro.

## <a name="testing-the-title-search-feature"></a>Testando o recurso de pesquisa de título

Agora você pode testar a página de pesquisa concluída. Execute *Movies. cshtml*.

Insira um gênero e clique em **Pesquisar gênero**. A grade exibe filmes desse gênero, como antes.

Insira uma palavra de título e clique em **título de pesquisa**. A grade exibe os filmes que têm essa palavra no título.

![Página de filmes listando depois de procurar ' o ' no título](form-basics/_static/image6.png)

Deixe ambas as caixas de texto em branco e clique em um dos botões. A grade exibe todos os filmes.

## <a name="combining-the-queries"></a>Combinando as consultas

Você pode observar que as pesquisas que você pode executar são exclusivas. Não é possível pesquisar o título e o gênero ao mesmo tempo, mesmo se ambas as caixas de pesquisa tiverem valores. Por exemplo, você não pode pesquisar por todos os filmes de ação cujo título contenha "Adventure". (Como a página é codificada agora, se você inserir valores para o gênero e o título, a pesquisa de título obterá precedência.) Para criar uma pesquisa que combine as condições, você precisaria criar uma consulta SQL que tenha sintaxe semelhante à seguinte:

`SELECT * FROM Movies WHERE Genre = @0 AND Title LIKE @1`

E você precisaria executar a consulta usando uma instrução como a seguinte (aproximadamente falando):

`var selectedData = db.Query(selectCommand, searchGenre, searchTitle);`

A criação de lógica para permitir muitas permutações de critérios de pesquisa pode ficar um pouco envolvida, como você pode ver. Portanto, vamos parar aqui.

## <a name="coming-up-next"></a>Chegando em seguida

No próximo tutorial, você criará uma página que usa um formulário para permitir que os usuários adicionem filmes ao banco de dados.

## <a name="complete-listing-for-movie-page-updated-with-search"></a>Listagem completa da página de filme (atualizada com pesquisa)

[!code-cshtml[Main](form-basics/samples/sample13.cshtml)]

## <a name="additional-resources"></a>Recursos adicionais

- [Introdução a O ASP.NET que programa usando a sintaxe razor](https://go.microsoft.com/fwlink/?LinkID=202890)
- [Cláusula WHERE do SQL](http://www.w3schools.com/sql/sql_where.asp) no site W3Schools
- Artigo [definições de método](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) no site W3C

> [!div class="step-by-step"]
> [Anterior](displaying-data.md) 
>  [Avançar](entering-data.md)
