---
uid: web-pages/overview/api-reference/asp-net-web-pages-api-reference
title: Referência rápida da API do Páginas da Web do ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Esta página contém uma lista com exemplos breves dos objetos, propriedades e métodos mais usados para a programação de Páginas da Web do ASP.NET com sintaxe Razor.
ms.author: riande
ms.date: 02/10/2014
ms.assetid: 4001cb9b-3bfd-4ace-8a89-1561d8421e2c
msc.legacyurl: /web-pages/overview/api-reference/asp-net-web-pages-api-reference
msc.type: authoredcontent
ms.openlocfilehash: e010307fc0576e8b003fbfe665cae77618d9c9a5
ms.sourcegitcommit: c62ec20b453cee3249eb894ecd75013b57d078f0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/21/2020
ms.locfileid: "92345247"
---
# <a name="aspnet-web-pages-razor-api-quick-reference"></a>Referência rápida da API do Páginas da Web do ASP.NET (Razor)

 por [Tom FitzMacken](https://github.com/tfitzmac)

> Esta página contém uma lista com exemplos breves dos objetos, propriedades e métodos mais usados para a programação de Páginas da Web do ASP.NET com sintaxe Razor.
> 
> As descrições marcadas com "(v2)" foram introduzidas no Páginas da Web do ASP.NET versão 2.
> 
> Para obter a documentação de referência da API, consulte a [documentação de referência do páginas da Web do ASP.net](https://go.microsoft.com/fwlink/?LinkId=208659) no msdn.
> 
> ## <a name="software-versions"></a>Versões de software
> 
> 
> - Páginas da Web do ASP.NET (Razor) 3
>   
> 
> Este tutorial também funciona com Páginas da Web do ASP.NET 2 e Páginas da Web do ASP.NET 1,0 (exceto para os recursos marcados como v2).

Esta página contém informações de referência para o seguinte:

- [Classes](#Classes)
- [Dados](#Data)
- [Auxiliares](#Helpers)
- [Validação](#Validation)

<a id="Classes"></a>
## <a name="classes"></a>Classes

### `AppState[key], AppState[index],App`

Contém dados que podem ser compartilhados por qualquer página no aplicativo. Você pode usar a `App` propriedade dinâmica para acessar os mesmos dados, como no exemplo a seguir:

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample1.html)]

### `AsBool(), AsBool(true|false)`

Converte um valor de cadeia de caracteres em um valor booliano (true/false). Retorna false ou o valor especificado se a cadeia de caracteres não representar true/false.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample2.cs)]

### `AsDateTime(), AsDateTime(value)`

Converte um valor de cadeia de caracteres em data/hora. Retorna `DateTime.MinValue` ou o valor especificado se a cadeia de caracteres não representar uma data/hora.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample3.cs)]

### `AsDecimal(), AsDecimal(value)`

Converte um valor de cadeia de caracteres em um valor decimal. Retorna 0,0 ou o valor especificado se a cadeia de caracteres não representar um valor decimal.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample4.cs)]

### `AsFloat(), AsFloat(value)`

Converte um valor de cadeia de caracteres em um float. Retorna 0,0 ou o valor especificado se a cadeia de caracteres não representar um valor decimal.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample5.cs)]

### `AsInt(), AsInt(value)`

Converte um valor de cadeia de caracteres em um inteiro. Retorna 0 ou o valor especificado se a cadeia de caracteres não representar um inteiro.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample6.cs)]

### `Href(path [, param1 [, param2]])`

Cria uma URL compatível com o navegador a partir de um caminho de arquivo local, com partes de caminho adicionais opcionais.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample7.cshtml)]

### `Html.Raw(value)`

Renderiza o *valor* como marcação HTML em vez de renderizá-lo como saída codificada em HTML.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample8.cshtml)]

### `IsBool(), IsDateTime(), IsDecimal(), IsFloat(), IsInt()`

Retornará true se o valor puder ser convertido de uma cadeia de caracteres para o tipo especificado.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample9.cs)]

### `IsEmpty()`

Retornará true se o objeto ou a variável não tiver nenhum valor.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample10.cs)]

### `IsPost`

Retornará true se a solicitação for uma POSTAgem. (Normalmente, as solicitações iniciais são um GET.)

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample11.cs)]

### `Layout`

Especifica o caminho de uma página de layout a ser aplicada a esta página.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample12.html)]

### `PageData[key], PageData[index],Page`

Contém dados compartilhados entre a página, as páginas de layout e as páginas parciais na solicitação atual. Você pode usar a `Page` propriedade dinâmica para acessar os mesmos dados, como no exemplo a seguir:

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample13.html)]

### `RenderBody()`

(Páginas de layout) Renderiza o conteúdo de uma página de conteúdo que não está em nenhuma seção nomeada.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample14.cs)]

### `RenderPage(path, values)`  
`RenderPage(path[,param1 [, param2]])`

Renderiza uma página de conteúdo usando o caminho especificado e os dados extras opcionais. Você pode obter os valores dos parâmetros extras de `PageData` por posição (exemplo 1) ou chave (exemplo 2).

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample15.js)]

### `RenderSection(sectionName [, required = true|false])`

(Páginas de layout) Renderiza uma seção de conteúdo que tem um nome. Defina *obrigatório* como falso para tornar uma seção opcional.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample16.js)]

### `Request.Cookies[key]`

Obtém ou define o valor de um cookie HTTP.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample17.cs)]

### `Request.Files[key]`

Obtém os arquivos que foram carregados na solicitação atual.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample18.js)]

### `Request.Form[key]`

Obtém os dados que foram postados em um formulário (como cadeias de caracteres). `Request[key]` verifica as `Request.Form` `Request.QueryString` coleções e.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample19.cs)]

### `Request.QueryString[key]`

Obtém os dados que foram especificados na cadeia de caracteres de consulta da URL. `Request[key]` verifica as `Request.Form` `Request.QueryString` coleções e.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample20.cs)]

### `Request.Unvalidated(key)`  
`Request.Unvalidated().QueryString|Form|Cookies|Headers[key]`

Desabilita seletivamente a validação de solicitação para um elemento de formulário, valor de cadeia de caracteres de consulta, cookie ou valor de cabeçalho. A validação de solicitação é habilitada por padrão e impede que os usuários postem a marcação ou outros conteúdos potencialmente perigosos.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample21.cs)]

### `Response.AddHeader(name, value)`

Adiciona um cabeçalho de servidor HTTP à resposta.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample22.cs)]

### `Response.OutputCache(seconds [, sliding] [, varyByParams])`

Armazena em cache a saída da página por um período especificado. Opcionalmente, defina *deslizante* para redefinir o tempo limite em cada acesso de página e *VaryByParams* para armazenar em cache versões diferentes da página para cada cadeia de caracteres de consulta diferente na solicitação de página.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample23.js)]

### `Response.Redirect(path)`

Redireciona a solicitação do navegador para um novo local.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample24.js)]

### `Response.SetStatus(httpStatusCode)`

Define o código de status HTTP enviado ao navegador.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample25.cs)]

### `Response.WriteBinary(data [, mimetype])`

Grava o conteúdo dos *dados* na resposta com um tipo MIME opcional.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample26.js)]

### `Response.WriteFile(file)`

Grava o conteúdo de um arquivo na resposta.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample27.cs)]

### `@section(sectionName) {content }`

(Páginas de layout) Define uma seção de conteúdo que tem um nome.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample28.cshtml)]

### `Server.HtmlDecode(htmlText)`

Decodifica uma cadeia de caracteres codificada em HTML.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample29.cs)]

### `Server.HtmlEncode(text)`

Codifica uma cadeia de caracteres para renderização na marcação HTML.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample30.cs)]

### `Server.MapPath(virtualPath)`

Retorna o caminho físico do servidor para o caminho virtual especificado.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample31.cs)]

### `Server.UrlDecode(urlText)`

Decodifica o texto de uma URL.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample32.cs)]

### `Server.UrlEncode(text)`

Codifica o texto para colocar em uma URL.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample33.cs)]

### `Session[key]`

Obtém ou define um valor que existe até que o usuário feche o navegador.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample34.css)]

### `ToString()`

Exibe uma representação de cadeia de caracteres do valor do objeto.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample35.html)]

### `UrlData[index]`

Obtém dados adicionais da URL (por exemplo, */mypage/ExtraData*).

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample36.cs)]

### `WebSecurity.ChangePassword(userName,currentPassword,newPassword)`

Altera a senha para o usuário especificado.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample37.cs)]

### `WebSecurity.ConfirmAccount(accountConfirmationToken)`

Confirma uma conta usando o token de confirmação da conta.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample38.cs)]

### `WebSecurity.CreateAccount(userName, password`  
 `[, requireConfirmationToken = true|false])`

Cria uma nova conta de usuário com o nome de usuário e a senha especificados. Para exigir um token de confirmação, passe true para *requireConfirmationToken.*

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample39.cs)]

### `WebSecurity.CurrentUserId`

Obtém o identificador de inteiro para o usuário conectado no momento.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample40.cs)]

### `WebSecurity.CurrentUserName`

Obtém o nome do usuário conectado no momento.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample41.cs)]

### `WebSecurity.GeneratePasswordResetToken(username`  
 `[, tokenExpirationInMinutesFromNow])`

Gera um token de redefinição de senha que pode ser enviado por email para um usuário para que o usuário possa redefinir a senha.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample42.cs)]

### `WebSecurity.GetUserId(userName)`

Retorna a ID de usuário do nome de usuário.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample43.cs)]

### `WebSecurity.IsAuthenticated`

Retornará true se o usuário atual estiver conectado.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample44.cs)]

### `WebSecurity.IsConfirmed(userName)`

Retornará true se o usuário tiver sido confirmado (por exemplo, por meio de um email de confirmação).

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample45.cs)]

### `WebSecurity.IsCurrentUser(userName)`

Retornará true se o nome do usuário atual corresponder ao nome de usuário especificado.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample46.cs)]

### `WebSecurity.Login(userName,password[, persistCookie])`

Registra o usuário no, definindo um token de autenticação no cookie.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample47.cs)]

### `WebSecurity.Logout()`

Faz logoff do usuário removendo o cookie do token de autenticação.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample48.css)]

### `WebSecurity.RequireAuthenticatedUser()`

Se o usuário não estiver autenticado, define o status HTTP como 401 (Não autorizado).

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample49.css)]

### `WebSecurity.RequireRoles(roles)`

Se o usuário atual não for um membro de uma das funções especificadas, o definirá o status HTTP como 401 (não autorizado).

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample50.html)]

### `WebSecurity.RequireUser(userId)`  
`WebSecurity.RequireUser(userName)`

Se o usuário atual não for o usuário especificado por *username*, o definirá o status HTTP como 401 (não autorizado).

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample51.css)]

### `WebSecurity.ResetPassword(passwordResetToken,newPassword)`

Se o token de redefinição de senha for válido, o alterará a senha do usuário para a nova senha.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample52.css)]

<a id="Data"></a>
## <a name="data"></a>Dados

### `Database.Execute(SQLstatement [,parameters]`

Executa o *SQLStatement* (com parâmetros opcionais), como inserir, excluir ou atualizar e retorna uma contagem de registros afetados.

[!code-sql[Main](asp-net-web-pages-api-reference/samples/sample53.sql)]

### `Database.GetLastInsertId()`

Retorna a coluna de identidade da linha inserida mais recentemente.

[!code-sql[Main](asp-net-web-pages-api-reference/samples/sample54.sql)]

### `Database.Open(filename)`  
`Database.Open(connectionStringName)`

Abre o arquivo de banco de dados especificado ou o banco de dados especificado usando uma cadeia de conexão nomeada do arquivo *Web.config* .

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample55.cs)]

### `Database.OpenConnectionString(connectionString)`

Abre um banco de dados usando a cadeia de conexão. (Isso contrasta com `Database.Open` , que usa um nome de cadeia de conexão.)

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample56.cs)]

### `Database.Query(SQLstatement[,parameters])`

Consulta o banco de dados usando *SQLStatement* (opcionalmente passando parâmetros) e retorna os resultados como uma coleção.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample57.html)]

### `Database.QuerySingle(SQLstatement [, parameters])`

Executa o *SQLStatement* (com parâmetros opcionais) e retorna um único registro.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample58.cs)]

### `Database.QueryValue(SQLstatement [, parameters])`

Executa o *SQLStatement* (com parâmetros opcionais) e retorna um único valor.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample59.cs)]

<a id="Helpers"></a>
## <a name="helpers"></a>Auxiliares

### `Analytics.GetGoogleHtml(webPropertyId)`

Renderiza o código JavaScript do Google Analytics para a ID especificada.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample60.js)]

### `Analytics.GetStatCounterHtml(project,security)`

Renderiza o código JavaScript da análise de StatCounter para o projeto especificado.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample61.css)]

### `Analytics.GetYahooHtml(account)`

Renderiza o código JavaScript da análise do Yahoo para a conta especificada.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample62.js)]

### `Bing.SearchBox([boxWidth])`

Passa uma pesquisa para o Bing. Para especificar o site a ser pesquisado e um título para a caixa de pesquisa, você pode definir as `Bing.SiteUrl` `Bing.SiteTitle` Propriedades e. Normalmente, você define essas propriedades na página * \_ AppStart* .

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample63.html)]

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample64.cshtml)]

### `Chart(width,height [, template] [, templatePath])`

Inicializa um gráfico.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample65.cshtml)]

### `Chart.AddLegend([title] [, name])`

Adiciona uma legenda a um gráfico.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample66.cshtml)]

### `Chart.AddSeries([name] [, chartType] [, chartArea]`  
 `[, axisLabel] [, legend] [, markerStep] [, xValue]`  
 `[, xField] [, yValues] [, yFields] [, options])`

Adiciona uma série de valores ao gráfico.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample67.cshtml)]

### `Crypto.Hash(string [, algorithm])`  
`Crypto.Hash(bytes [, algorithm])`

Retorna um hash para os dados especificados. O algoritmo padrão é `sha256` .

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample68.html)]

### `Facebook.LikeButton(href [, buttonLayout] [, showFaces] [, width] [, height]`   
 `[, action] [, font] [, colorScheme] [, refLabel])`

Permite que os usuários do Facebook façam uma conexão com as páginas.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample69.js)]

### `FileUpload.GetHtml([initialNumberOfFiles] [, allowMoreFilesToBeAdded]`  
 `[, includeFormTag] [, addText] [, uploadText])`

Renderiza a interface do usuário para carregar arquivos.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample70.html)]

### `GamerCard.GetHtml(gamerTag)`

Renderiza a marca de gamem do Xbox especificada.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample71.js)]

### `Gravatar.GetHtml(email [, imageSize] [, defaultImage] [, rating]`  
 `[, imageExtension] [, attributes])`

Renderiza a imagem Gravatar para o endereço de email especificado.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample72.css)]

### `Json.Encode(object)`

Converte um objeto de dados em uma cadeia de caracteres no formato JavaScript Object Notation (JSON).

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample73.cs)]

### `Json.Decode(string)`

Converte uma cadeia de caracteres de entrada codificada em JSON em um objeto de dados que você pode iterar ou inserir em um banco de dado.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample74.cs)]

### `LinkShare.GetHtml(pageTitle[, pageLinkBack] [, twitterUserName]`  
 `[, additionalTweetText] [, linkSites])`

Renderiza links de rede social usando o título especificado e a URL opcional.

[!code-xml[Main](asp-net-web-pages-api-reference/samples/sample75.xml)]

### `ModelStateDictionary.AddError(key, errorMessage)`

Associa uma mensagem de erro a um campo de formulário. Use o `ModelState` auxiliar para acessar este membro.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample76.cs)]

### `ModelStateDictionary.AddFormError(errorMessage)`

Associa uma mensagem de erro a um formulário. Use o `ModelState` auxiliar para acessar este membro.

[!code-powershell[Main](asp-net-web-pages-api-reference/samples/sample77.ps1)]

### `ModelStateDictionary.IsValid`

Retorna true se não houver nenhum erro de validação. Use o `ModelState` auxiliar para acessar este membro.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample78.cs)]

### `ObjectInfo.Print(value [, depth] [, enumerationLength])`

Renderiza as propriedades e os valores de um objeto e de quaisquer objetos filho.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample79.css)]

### `Recaptcha.GetHtml([, publicKey] [, theme] [, language] [, tabIndex])`

Renderiza o teste de verificação do reCAPTCHA.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample80.css)]

### `ReCaptcha.PublicKey`  
 `ReCaptcha.PrivateKey`

Define chaves públicas e privadas para o serviço reCAPTCHA. Normalmente, você define essas propriedades na página * \_ AppStart* .

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample81.css)]

### `ReCaptcha.Validate([, privateKey])`

Retorna o resultado do teste reCAPTCHA.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample82.cs)]

### `ServerInfo.GetHtml()`

Renderiza informações de status sobre Páginas da Web do ASP.NET.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample83.cshtml)]

### `Twitter.Profile(twitterUserName)`

Renderiza um fluxo do Twitter para o usuário especificado.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample84.js)]

### `Twitter.Search(searchQuery)`

Renderiza um fluxo do Twitter para o texto de pesquisa especificado.

[!code-xml[Main](asp-net-web-pages-api-reference/samples/sample85.xml)]

### `Video.Flash(filename [, width, height])`

Renderiza um player de vídeo Flash para o arquivo especificado com largura e altura opcionais.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample86.cshtml)]

### `Video.MediaPlayer(filename [, width, height])`

Renderiza um Windows Media Player para o arquivo especificado com largura e altura opcionais.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample87.cshtml)]

### `Video.Silverlight(filename, width, height)`

Renderiza um player do Silverlight para o arquivo *. xap* especificado com a largura e a altura necessárias.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample88.cshtml)]

### `WebCache.Get(key)`

Retorna o objeto especificado por *Key*ou NULL se o objeto não for encontrado.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample89.cs)]

### `WebCache.Remove(key)`

Remove o objeto especificado por *chave* do cache.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample90.cs)]

### `WebCache.Set(key, value [, minutesToCache] [, slidingExpiration])`

Coloca o *valor* no cache sob o nome especificado por *chave*.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample91.html)]

### `WebGrid(data)`

Cria um novo `WebGrid` objeto usando dados de uma consulta.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample92.cs)]

### `WebGrid.GetHtml()`

Renderiza a marcação para exibir dados em uma tabela HTML.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample93.html)]

### `WebGrid.Pager()`

Renderiza um pager para o `WebGrid` objeto.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample94.html)]

### `WebImage(path)`

Carrega uma imagem do caminho especificado.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample95.cs)]

### `WebImage.AddImagesWatermark(image)`

Adiciona a imagem especificada como uma marca d' água.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample96.cs)]

### `WebImage.AddTextWatermark(text)`

Adiciona o texto especificado à imagem.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample97.cs)]

### `WebImage.FlipHorizontal()`  
`WebImage.FlipVertical()`

Inverte a imagem horizontal ou verticalmente.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample98.css)]

### `WebImage.GetImageFromRequest()`

Carrega uma imagem quando uma imagem é postada em uma página durante um upload de arquivo.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample99.cs)]

### `WebImage.Resize(width,height)`

Redimensiona uma imagem.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample100.css)]

### `WebImage.RotateLeft()`  
`WebImage.RotateRight()`

Gira a imagem para a esquerda ou para a direita.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample101.css)]

### `WebImage.Save(path [, imageFormat])`

Salva a imagem no caminho especificado.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample102.js)]

### `WebMail.Password`

Define a senha para o servidor SMTP. Normalmente, você define essa propriedade na página * \_ AppStart* .

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample103.cs)]

### `WebMail.Send(to, subject, body [, from] [, cc] [, filesToAttach] [, isBodyHtml]`  
 `[, additionalHeaders])`

Envia uma mensagem de email.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample104.css)]

### `WebMail.SmtpServer`

Define o nome do servidor SMTP. Normalmente, você define essa propriedade na página * \_ AppStart* .

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample105.html)]

### `WebMail.UserName`

Define o nome de usuário para o servidor SMTP. Normalmente, você deve definir essa propriedade na página * \_ AppStart* .

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample106.html)]

<a id="Validation"></a>
## <a name="validation"></a>Validação

### `Html.ValidationMessage(field)`

v2 Renderiza uma mensagem de erro de validação para o campo especificado.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample107.cshtml)]

### `Html.ValidationSummary([message])`

v2 Exibe uma lista de todos os erros de validação.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample108.cshtml)]

### `Validation.Add(field, validationType)`

v2 Registra um elemento de entrada do usuário para o tipo de validação especificado.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample109.js)]

### `Validation.ClassFor(field)`

v2 Processa dinamicamente os atributos de classe CSS para validação do lado do cliente para que você possa formatar mensagens de erro de validação. (Requer que você referencie as bibliotecas de script de cliente apropriadas e que você defina classes CSS.)

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample110.html)]

### `Validation.For(field)`

v2 Habilita a validação do lado do cliente para o campo de entrada do usuário. (Requer que você referencie as bibliotecas de script de cliente apropriadas.)

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample111.html)]

### `Validation.IsValid()`

v2 Retornará true se todos os elementos de entrada do usuário que são registrado para validação contiverem valores válidos.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample112.cs)]

### `Validation.RequireField(field[, errorMessage])`

v2 Especifica que os usuários devem fornecer um valor para o elemento de entrada do usuário.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample113.cs)]

### `Validation.RequireFields(field1[, field12, field3, ...])`

v2 Especifica que os usuários devem fornecer valores para cada um dos elementos de entrada do usuário. Esse método não permite que você especifique uma mensagem de erro personalizada.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample114.html)]

### `Validator.DateTime ([error message])`  
`Validator.Decimal([error message])`  
`Validator.EqualsTo(otherField,[error message])`  
`Validator.Float([error message])`  
`Validator.Integer([error message])`  
`Validator.Range(min,max [, error message])`  
`Validator.RegEx(pattern[, error message])`  
`Validator.Required([error message])`  
`Validator.StringLength(length)`  
`Validator.Url([error message])`

v2 Especifica um teste de validação quando você usa o `Validation.Add` método.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample115.js)]
