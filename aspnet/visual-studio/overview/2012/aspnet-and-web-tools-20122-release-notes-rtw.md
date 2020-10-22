---
uid: visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw
title: Notas de versão do ASP.NET and Web Tools 2012,2 | Microsoft Docs
author: rick-anderson
description: Notas de versão do ASP.NET and Web Tools 2012,2.
ms.author: riande
ms.date: 02/14/2013
ms.assetid: 9534e58b-1d15-4f1d-b04c-10c79b9d8227
msc.legacyurl: /visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw
msc.type: content
ms.openlocfilehash: abd6d8ce0646852a194369589cb730fc98ecb3ad
ms.sourcegitcommit: c62ec20b453cee3249eb894ecd75013b57d078f0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/21/2020
ms.locfileid: "92345588"
---
# <a name="aspnet-and-web-tools-20122-release-notes"></a>Notas de Versão do ASP.NET and Web Tools 2012.2

> Este documento descreve a versão do ASP.NET and Web Tools 2012,2. É uma atualização para ferramentas da Web do Visual Studio e ASP.NET.

- [Notas de instalação](#_Installation)
- [Documentação](#_Documentation)
- [Suporte](#_Support)
- [Requisitos de software](#_Software_Requirements)
- [Novos recursos no ASP.NET and Web Tools 2012,2](#_New_Features_in)

    - [Ferramentas](#_Tooling)
    - [Publicação na Web](#_Web_Publishing)
    - [Modelos MVC do ASP.NET](#_Templates)
    - [ASP.NET Web API](#_ASP.NET_Web_API)

    - [ASP.NET SignalR](#_ASP.NET_SignalR)
    - [URLs amigáveis do ASP.NET](#_ASP.NET_Friendly_URLs)
- [Problemas conhecidos e alterações recentes](#_Known_Issues_and)

<a id="_Installation"></a>
## <a name="installation-notes"></a>Notas de instalação

O ASP.NET and Web Tools 2012,2 para Visual Studio 2012 pode ser instalado usando o [Web Platform Installer](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=ASPDOTNETandWebTools2012_2). Esta é uma atualização para o Visual Studio 2012 ou Visual Studio Express 2012 para Web, que é necessária. Se você não tiver o Visual Studio instalado, Visual Studio Express 2012 para Web será instalado.

Você também pode instalar o ASP.NET and Web Tools 2012,2 manualmente. Você deve ter o Visual Studio 2012 ou o Visual Studio Express 2012 para a Web instalado. Em seguida, use as seguintes instruções: 

1. Baixe o instalador do [ASP.net e do Web frameworks 2012,2](https://download.microsoft.com/download/6/5/6/6562AFBE-9503-4E64-970C-1427133FCD73/AspNetWebTools2012Setup.exe) do centro de download.
2. Quando solicitado, clique em executar. Você também pode salvar o arquivo para executá-lo mais tarde.
3. Verifique a versão do Visual Studio que você vai atualizar. Você pode fazer isso iniciando o Visual Studio que deseja atualizar. Em seguida, clique no item de menu ajuda.   
    ![](aspnet-and-web-tools-20122-release-notes-rtw/_static/image1.jpg)
4. Se você vir o item de menu &quot; sobre Microsoft Visual Studio 2012 para Web &quot; , faça o download [da Web Ferramentas para Desenvolvedores 2012,2-Visual Studio Express 2012 para Web](https://go.microsoft.com/fwlink/?LinkID=282228). Caso contrário, baixe o [Web Ferramentas para Desenvolvedores 2012,2-Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282228).
5. Quando solicitado, clique em executar. Você também pode salvar o arquivo para executá-lo mais tarde.

> [!NOTE]
> ASP.NET and Web Tools versão 2012,2 não inclui SQL Server Data Tools. Os bancos de dados do SQL Server e do SQL do Windows Azure fornecem um conjunto mais rico de ferramentas de Database, incluindo desenvolvimento com suporte a projetos offline, comparação de esquemas e recursos aprimorados de implantação de banco de dados. Para obter mais informações ou instalar SQL Server Data Tools visite [https://go.microsoft.com/fwlink/?LinkID=237127](https://go.microsoft.com/fwlink/?LinkID=237127) .

<a id="_Documentation"></a>
## <a name="documentation"></a>Documentação

Os tutoriais e outras informações sobre o ASP.NET and Web Tools 2012,2 estão disponíveis no site da ASP.NET ( https://www.asp.net) .

<a id="_Support"></a>
## <a name="support"></a>Suporte

O ASP.NET and Web Tools 2012,2 é oficialmente lançado e tem suporte. Você pode usar seu canal de suporte normal. Você também pode postar perguntas para os fóruns do ASP.NET ( [https://forums.asp.net/](https://forums.asp.net/) ), em que os membros da Comunidade do ASP.net freqüentemente podem fornecer suporte informal.

<a id="_Software_Requirements"></a>
## <a name="software-requirements"></a>Requisitos de software

O ASP.NET and Web Tools 2012,2 requer o Visual Studio 2012 ou Visual Studio Express 2012 para Web.

<a id="_New_Features_in"></a>
## <a name="new-features-in-aspnet-and-web-tools-20122"></a>Novos recursos no ASP.NET and Web Tools 2012,2

Esta seção descreve os recursos que foram introduzidos na versão ASP.NET and Web Tools 2012,2.

<a id="_Tooling"></a>
### <a name="tooling"></a>Ferramentas

- Inspetor de Página 

    - Suporte ao mapeamento de seleção de JavaScript, permitindo que Inspetor de Página mapeie itens que foram adicionados dinamicamente à página de volta para o código JavaScript correspondente.
    - A capacidade de ver as atualizações de CSS em tempo real.
    - Para obter mais informações, leia [sincronização automática de CSS e mapeamento de seleção de JavaScript em Inspetor de página](https://blogs.msdn.com/b/webdev/archive/2012/12/14/css-auto-sync-and-javascript-selection-mapping-in-page-inspector.aspx).
- Editor 

    - Suporte a realce de sintaxe para CoffeeScript, Mustache, Handlebars e JsRender.
    - O editor de HTML fornece IntelliSense para associações de Knockout.
    - MENOS edição e suporte a compilador para habilitar a criação de CSS dinâmico usando menos.
    - Cole JSON como uma classe .NET. Usando esse comando de colagem especial para colar JSON em um arquivo de código C# ou VB.NET, o Visual Studio gerará automaticamente classes .NET inferidas a partir do JSON.
- O suporte do Mobile Emulator adiciona ganchos de extensibilidade para que os emuladores de terceiros possam ser instalados como um VSIX. Os emuladores instalados aparecerão na lista suspensa F5, para que os desenvolvedores possam visualizar seus sites em uma variedade de dispositivos móveis. Leia mais sobre esse recurso na entrada de blog de Scott Hanselman sobre [a nova integração do BrowserStack com o Visual Studio](http://www.hanselman.com/blog/CrossBrowserDebuggingIntegratedIntoVisualStudioWithBrowserStack.aspx).

<a id="_Web_Publishing"></a>
### <a name="web-publishing"></a>Publicação na Web

- Os projetos de site agora têm a mesma experiência de publicação que os projetos de aplicativos Web, incluindo a publicação em sites do Windows Azure.
- &#8211; de publicação seletiva para um ou mais arquivos, você pode executar as seguintes ações (depois de publicar em um ponto de extremidade Implantação da Web): 

    - Publicar arquivos selecionados.
    - Consulte a diferença entre um arquivo local e um arquivo remoto.
    - Atualize o arquivo local com o arquivo remoto ou atualize o arquivo remoto com o arquivo local.

<a id="_Templates"></a>
### <a name="aspnet-mvc-templates"></a>Modelos MVC do ASP.NET

- O novo modelo de aplicativo do Facebook facilita a gravação de aplicativos Canvas do Facebook. Em alguns passos simples, é possível criar um aplicativo do Facebook que obtém dados de um usuário conectado e se integra com os amigos. O modelo inclui uma nova biblioteca para tratar de todo o encanamento envolvido na criação de um aplicativo do Facebook, incluindo autenticação, permissões, acesso aos dados do Facebook, entre outros. Para obter mais informações sobre como usar o modelo de aplicativo do Facebook, consulte [https://go.microsoft.com/fwlink/?LinkID=269921](https://go.microsoft.com/fwlink/?LinkID=269921) .
- Um novo modelo MVC de aplicativo de página única permite aos desenvolvedores criar aplicativos da Web interativos de cliente usando HTML 5, CSS 3 e as bibliotecas populares Knockout e jQuery JavaScript, na parte superior do ASP.NET Web API. O modelo inclui um aplicativo de lista "todo" que demonstra práticas comuns para a criação de um aplicativo HTML5 JavaScript que usa uma API de servidor RESTful. Você pode ler mais em [https://www.asp.net/single-page-application](../../../single-page-application/index.md) .
- Agora você pode criar um VSIX que adiciona novos modelos à caixa de diálogo novo projeto do ASP.NET MVC. Saiba como: [https://go.microsoft.com/fwlink/?LinkId=275019](https://go.microsoft.com/fwlink/?LinkId=275019)
- O pacote FixedDisplayModes &#8211; modelos de projeto do MVC foram atualizados para incluir o novo pacote NuGet ' FixedDisplayModes ', que contém uma solução alternativa para um bug no MVC 4. Para obter mais informações sobre a correção contida no pacote, consulte esta postagem de blog ( [https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx) ) da equipe do MVC.

<a id="_ASP.NET_Web_API"></a>
### <a name="aspnet-web-api"></a>ASP.NET Web API

ASP.NET Web API foi aprimorado com vários recursos novos:

- ASP.NET Web API OData
- Rastreamento de ASP.NET Web API
- Página de ajuda do ASP.NET Web API

#### <a name="aspnet-web-api-odata"></a>ASP.NET Web API OData

ASP.NET Web API OData oferece a flexibilidade necessária para criar pontos de extremidade OData com uma lógica de negócios avançada em qualquer fonte de dados. Com ASP.NET Web API OData, você controla a quantidade de semânticas OData que deseja expor. ASP.NET Web API OData está incluído com os modelos de projeto do ASP.NET MVC 4 e também está disponível no NuGet ( [http://www.nuget.org/packages/microsoft.aspnet.webapi.odata](http://www.nuget.org/packages/microsoft.aspnet.webapi.odata) ).

O ASP.NET Web API OData atualmente dá suporte aos seguintes recursos:

- Habilite a semântica de consulta OData aplicando o atributo [consultável].
- Valide facilmente consultas OData e restrinja o conjunto de opções de consulta, operadores e funções com suporte.
- O parâmetro é associado a ODataQueryOptions diretamente para obter uma representação de árvore de sintaxe abstrata da consulta que pode ser validada e aplicada a um IQueryable ou IEnumerable.
- Habilitar paginação controlada por serviço e geração de link da próxima página especificando limites de resultado no atributo [consultável].
- Solicite uma contagem embutida do número total de recursos correspondentes usando $inlinecount.
- Controle de propagação nula.
- Todos os operadores/todos no $filter.
- Inferir um modelo de dados de entidade por convenção ou personalizar explicitamente um modelo de maneira semelhante a Entity Framework código-primeiro.
- Expor conjuntos de entidades derivando de EntitySetController.
- Convenções simples e personalizáveis para expor propriedades de navegação, manipular links e implementar ações de OData.
- Roteamento simplificado usando o método de extensão MapODataRoute.
- Suporte para controle de versão expondo vários modelos de EDM.
- Expor o documento de serviço e $metadata para que você possa gerar clientes (.NET, Windows Phone, Windows Store etc.) para sua API Web.
- Suporte para os formatos OData Atom, JSON e JSON detalhados.
- Criar, atualizar, atualizar parcialmente (PATCH) e excluir entidades.
- Consultar e manipular relações entre entidades.
- Crie links de relacionamento que se conectem às suas rotas.
- Tipos complexos.
- Herança de tipo de entidade.
- Propriedades da coleção.
- Enums.
- Ações de OData.
- Criado a partir da mesma base que WCF Data Services, a saber ODataLib ( [http://www.nuget.org/packages/microsoft.data.odata](http://www.nuget.org/packages/microsoft.data.odata) ).

Para obter mais informações sobre ASP.NET Web API OData, consulte [https://go.microsoft.com/fwlink/?LinkId=271141](https://go.microsoft.com/fwlink/?LinkId=271141) .

#### <a name="aspnet-web-api-tracing"></a>Rastreamento de ASP.NET Web API

ASP.NET Web API rastreamento integra dados de rastreamento de suas APIs Web com o rastreamento do .NET. Agora, ele está habilitado por padrão no modelo de projeto de API Web. Os dados de rastreamento para suas APIs da Web são enviados para a janela de saída e são disponibilizados por meio do IntelliTrace. ASP.NET Web API rastreamento permite que você rastreie informações sobre sua API da Web quando hospedada no Windows Azure por meio da integração com o [windows diagnóstico do Azure](https://msdn.microsoft.com/library/windowsazure/hh411529.aspx). Você também pode instalar e habilitar o rastreamento de ASP.NET Web API em qualquer aplicativo usando o pacote NuGet de rastreamento de ASP.NET Web API ( [http://www.nuget.org/packages/microsoft.aspnet.webapi.tracing](http://www.nuget.org/packages/microsoft.aspnet.webapi.tracing) ).

Para obter mais informações sobre como configurar e usar ASP.NET Web API rastreamento, consulte [https://go.microsoft.com/fwlink/?LinkID=269874](https://go.microsoft.com/fwlink/?LinkID=269874) .

#### <a name="aspnet-web-api-help-page"></a>Página de ajuda do ASP.NET Web API

A página de ajuda do ASP.NET Web API agora está incluída por padrão no modelo de projeto de API Web. A página de ajuda do ASP.NET Web API gera automaticamente a documentação para APIs Web, incluindo os pontos de extremidade HTTP, os métodos HTTP com suporte, os parâmetros e os conteúdos de exemplo de solicitação e resposta. A documentação é automaticamente extraída de comentários em seu código. Você também pode adicionar a página de ajuda ASP.NET Web API a qualquer aplicativo usando a página de ajuda do pacote NuGet do ASP.NET Web API ( [http://www.nuget.org/packages/microsoft.aspnet.webapi.helppage](http://www.nuget.org/packages/microsoft.aspnet.webapi.helppage) ).

Para obter mais informações sobre como configurar e personalizar a página de ajuda do ASP.NET Web API, consulte [https://go.microsoft.com/fwlink/?LinkId=271140](https://go.microsoft.com/fwlink/?LinkId=271140) .

<a id="_ASP.NET_SignalR"></a>
### <a name="aspnet-signalr"></a>ASP.NET SignalR

O Signalr ASP.NET simplifica a adição de recursos da Web em tempo real ao seu aplicativo ASP.NET, usando WebSockets, se disponível e voltando automaticamente para outras técnicas quando não estiver.

Para obter mais informações sobre como usar o Signalr ASP.NET, consulte [https://go.microsoft.com/fwlink/?LinkId=271271](https://go.microsoft.com/fwlink/?LinkId=271271) .

<a id="_ASP.NET_Friendly_URLs"></a>
### <a name="aspnet-friendly-urls"></a>URLs amigáveis do ASP.NET

ASP.NET FriendlyURLs torna muito fácil para os desenvolvedores de Web Forms gerarem URLs de aparência mais limpa (sem a extensão. aspx). Ele requer pouca ou nenhuma configuração e pode ser usado com aplicativos ASP.NET v 4.0 existentes. O recurso FriendlyURLs também facilita para os desenvolvedores a adição de suporte móvel a seus aplicativos, oferecendo suporte à alternância entre exibições de desktop e móveis.

Para obter mais informações sobre como instalar e usar URLs amigáveis do ASP.NET, consulte [http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx](http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx) .

<a id="_Known_Issues_and"></a>
## <a name="known-issues-and-breaking-changes"></a>Problemas conhecidos e alterações recentes

Esta seção descreve problemas conhecidos e alterações recentes que estão na versão ASP.NET and Web Tools 2012,2.

### <a name="installation-issues"></a>Problemas de instalação

#### <a name="out-of-order-installs-of-visual-studio-2012"></a>Instalações fora de ordem do Visual Studio 2012

A instalação de um SKU adicional do Visual Studio 2012 após a instalação do ASP.NET and Web Tools 2012,2 exigirá uma operação de reparo. Considere a sequência a seguir:

1. Instalar o Visual Studio 2012 Express para Web
2. Instalar o ASP.NET and Web Tools 2012,2
3. Instalar o Visual Studio 2012 Professional, Premium ou Ultimate

A etapa 2 só resultaria na instalação de atualizações do Express para Web. Para garantir que o SKU adicional instalado durante a etapa 3 contenha a atualização, você precisará reparar o ASP.NET and Web Tools 2012,2 para instalar as atualizações para a última SKU instalada. Isso também se aplicará se as SKUs na etapa 1 e 3 forem revertidas.

#### <a name="installing-microsoft-aspnet-and-web-tools-20122-when-visual-studio-is-open"></a>Instalando o Microsoft ASP.NET and Web Tools 2012,2 quando o Visual Studio está aberto

Se o VS estiver aberto durante a instalação do Microsoft ASP.NET and Web Tools 2012,2, o Visual Studio poderá terminar em um estado inadequado. É recomendável que os usuários fechem todas as instâncias do Visual Studio antes de prosseguir com a instalação.

#### <a name="canceling-aspnet-and-web-tools-20122-setup-in-the-middle-of-installation"></a>Cancelando a instalação do ASP.NET and Web Tools 2012,2 no meio da instalação

Cancelar a instalação do ASP.NET and Web Tools 2012,2 no meio da instalação deixará o Visual Studio em um estado inadequado. Para resolver esse problema, siga estas etapas: 

- Vá para Adicionar ou Remover Programas
- Desinstale o Microsoft ASP.NET and Web Tools 2012,2, se presente.
- Reinstalar o Microsoft ASP.NET and Web Tools 2012,2

#### <a name="after-uninstalling-aspnet-and-web-tools-20122-the-aspnet-mvc-4-templates-and-razor-v2-web-site-templates-are-missing"></a>Depois de desinstalar o ASP.NET and Web Tools 2012,2, os modelos ASP.NET MVC 4 e os modelos de site do Razor v2 estão ausentes

Desinstalar o ASP.NET and Web Tools 2012,2 também desinstalará todos os modelos de site ASP.NET MVC 4 e Razor v2 do Visual Studio 2012.

A solução alternativa é reparar a instalação do Visual Studio 2012 para reinstalar os modelos de site do ASP.NET MVC 4 e do Razor v2.

### <a name="tooling-issues"></a>Problemas de ferramentas

#### <a name="nuget-error-reported-during-project-creation"></a>Erro do NuGet relatado durante a criação do projeto

Depois de instalar o ASP.NET and Web Tools 2012,2, você poderá ver o seguinte erro ao criar um projeto MVC 4

![](aspnet-and-web-tools-20122-release-notes-rtw/_static/image1.png)

O ASP.NET and Web Tools 2012,2 fornece o NuGet 2,1 e atualizará a extensão no Visual Studio 2012. Em alguns casos, o instalador do VSIX falhará ao atualizar corretamente o VSIX. As etapas a seguir lhe permitirão resolver esse problema:

1. Inicie o Visual Studio 2012 como administrador
2. Vá para ferramentas – &gt; extensões e atualizações e desinstale o NuGet.
3. Fechar o Visual Studio
4. Navegue até a pasta de instalação do ASP.NET and Web Tools 2012,2:

    1. Para o Visual Studio 2012: Arquivos de **PROGRAMAS\MICROSOFT ASP. NET\ASP.NET Web Stack\Visual Studio 2012**
    2. Para o Visual Studio 2012 Express para Web: **Program Files\Microsoft ASP. NET\ASP.NET Web Stack\Visual Studio Express 2012 para Web**
5. Clique duas vezes no NuGet. Tools. vsix para reinstalar o NuGet

### <a name="web-api-issues"></a>Problemas da API Web

#### <a name="parsing-issues-in-filter-and-datetime-literals"></a>Problemas de análise em literais $filter e DateTime

O analisador de URI do OData não analisa literais DateTime parciais corretamente. Por exemplo, $filter = Start EQ DateTime ' 2012-12-31T12:00 ' não é analisado corretamente. Uma solução alternativa é usar o literal completo, $filter = Start EQ DateTime ' 2012-12-31T12:00:00 '.

#### <a name="odata-doesnt-support-case-insensitive-property-names"></a>OData não dá suporte a nomes de propriedade que não diferenciam maiúsculas de minúsculas.

O OData não dá suporte a nomes de propriedade que não diferenciam maiúsculas de minúsculas em consultas OData e no caminho OData. Consulte WorkItems:

- [http://aspnetwebstack.codeplex.com/workitem/366](http://aspnetwebstack.codeplex.com/workitem/366)
- [http://aspnetwebstack.codeplex.com/workitem/704](http://aspnetwebstack.codeplex.com/workitem/704)

Se os usuários tiverem diferentes maiúsculas e minúsculas no lado do cliente e do servidor JavaScript, eles provavelmente encontrarão esse problema. Esse problema é por design no protocolo OData. No entanto, muitos usuários relatam esse problema. Para contornar isso, os usuários precisam corrigir seus casos na URL.

#### <a name="default-odata-routing-conventions-doesnt-support-postput-on-navigation-property"></a>As convenções de roteamento de OData padrão não dão suporte ao POST/PUT na propriedade de navegação.

As convenções de roteamento de OData padrão não dão suporte ao POST/PUT na propriedade de navegação. Consulte WorkItem [http://aspnetwebstack.codeplex.com/workitem/366](http://aspnetwebstack.codeplex.com/workitem/366) . Não há essa Convenção comumente usada em convenções padrão.

Para contornar isso, os usuários precisam estender a nova Convenção de roteamento para dar suporte a ela.

### <a name="facebook-template-issues"></a>Problemas de modelo do Facebook

#### <a name="facebook-application-template-only-works-using-net-45"></a>O modelo de aplicativo do Facebook funciona apenas usando o .NET 4,5

Você deve selecionar .NET 4,5 na lista suspensa Framework na caixa de diálogo novo projeto para ver o modelo de aplicativo do Facebook no ASP.NET MVC 4.

#### <a name="real-time-update-controller"></a>Controlador de atualização em tempo real

O modelo de aplicativo do Facebook permite que o usuário crie facilmente um controlador da API Web para lidar com atualizações em tempo real do Facebook. Se o computador de desenvolvimento estiver por trás do NAT, o controlador poderá não funcionar sem mais configuração de rede. Consulte aqui para obter detalhes: [http://facebook.stackoverflow.com/questions/5259467/can-a-computer-behind-a-nat-router-receive-realtime-updates-from-facebook](http://facebook.stackoverflow.com/questions/5259467/can-a-computer-behind-a-nat-router-receive-realtime-updates-from-facebook)

#### <a name="query-string-values-conflict-with-facebook-oauth-parameters"></a>Conflitos de valores de cadeia de caracteres de consulta com parâmetros OAuth do Facebook

Os campos a seguir entram em conflito com a URL de retorno de chamada do Facebook OAuth. Não adicione seus próprios valores de cadeia de caracteres de consulta com os seguintes nomes: código, erro, descrição do erro \_ , motivo do erro \_ .

#### <a name="using-page-inspector-with-facebook-template"></a>Usando Inspetor de Página com o modelo do Facebook

Você não pode usar o recurso Inspetor de Página no Visual Studio 2012 durante a depuração do aplicativo do Facebook. Atualmente, o Inspetor de Página não dá suporte a iframes.

### <a name="single-page-application-template-issues"></a>Problemas de modelo de aplicativo de página única

#### <a name="with-jquery-19knockout-221-update-when-running-default-mvc-spa-project-new-todo-item-edit-enter-focus-event-is-not-handled-properly"></a>Com o JQuery 1.9/Knockout 2.2.1 Update, ao executar o projeto de SPA do MVC padrão, o evento New todo item Editar Inserir foco não é manipulado corretamente.

Com o JQuery 1.9/Knockout 2.2.1 Update, ao executar o projeto de SPA do MVC padrão, a nova edição de item de tarefas pendentes não se concentrará mais na caixa de edição do novo item de tarefas após a inserção do novo item de tarefas pendentes na lista de tarefas pendentes.

Para referência alternativa [http://knockoutjs.com/documentation/hasfocus-binding.html](http://knockoutjs.com/documentation/hasfocus-binding.html) e faça uma correção semelhante para o seguinte código de exemplo:

todo.model.js de arquivo  
 função ToDoList (dados), adicione o seguinte:  
 **autoselected = ko. observável (false);**

função ToDoList. prototype. addtodo, adicione o seguinte texto em branco:  
 **autoselected (true);**  
 Self. newTodoTitle ( &quot; &quot; );

File index. cshtml, adicione o seguinte texto em branco:  
 &lt;formulário de dados-BIND = &quot; Submit: Addtodo&quot;&gt;  
 &lt;classe de entrada = &quot; addtodo &quot; tipo = &quot; texto &quot; dados-associação = &quot; valor: newTodoTitle, espaço reservado: ' Digite aqui para adicionar ', blurOnEnter: true, **HasFocus: IsSelected**, evento: {Blur: addtodo}&quot; /&gt;  
 &lt;/Form&gt;
