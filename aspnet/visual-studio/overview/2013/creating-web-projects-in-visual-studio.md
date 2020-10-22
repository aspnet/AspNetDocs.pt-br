---
uid: visual-studio/overview/2013/creating-web-projects-in-visual-studio
title: Criando projetos Web ASP.NET no Visual Studio 2013 | Microsoft Docs
author: tdykstra
description: Este tópico explica as opções para criar projetos da Web do ASP.NET no Visual Studio 2013 com a atualização 3 aqui estão alguns dos novos recursos para o desenvolvimento para a Web c...
ms.author: riande
ms.date: 12/01/2014
ms.assetid: 61941e64-0c0d-4996-9270-cb8ccfd0cabc
msc.legacyurl: /visual-studio/overview/2013/creating-web-projects-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: fbb4cd7afa2506879d47bce980bf0164aad40c2c
ms.sourcegitcommit: c62ec20b453cee3249eb894ecd75013b57d078f0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/21/2020
ms.locfileid: "92345467"
---
# <a name="creating-aspnet-web-projects-in-visual-studio-2013"></a>Criação de Projetos Web do ASP.NET no Visual Studio 2013

por [Tom Dykstra](https://github.com/tdykstra)

> Este tópico explica as opções para criar projetos da Web do ASP.NET no Visual Studio 2013 com a atualização 3
> 
> Aqui estão alguns dos novos recursos para desenvolvimento na Web em comparação com as versões anteriores do Visual Studio:
> 
> - Uma interface do usuário simples para criar projetos que oferecem [suporte a várias estruturas ASP.net](#add) (Web Forms, MVC e API Web).
> - [ASP.net Identity](#indauth), um novo sistema de associação ASP.NET que funciona da mesma em todas as estruturas do ASP.net e funciona com software de hospedagem na Web diferente do IIS.
> - O uso da [inicialização](#bootstrap) para fornecer recursos responsivos de design e de ti.
> - Novos recursos para Web Forms que costumava ser oferecido apenas para o MVC, como a [criação de projeto de teste automático](#testproj) e um [modelo de site de intranet](#winauth).
> 
> Para obter informações sobre como criar projetos da Web para serviços de nuvem do Azure ou serviços móveis do Azure, consulte Introdução [aos serviços de nuvem do Azure e ASP.net](https://azure.microsoft.com/documentation/articles/cloud-services-dotnet-get-started/) e [criar um aplicativo placar com o back-end .net dos serviços móveis do Azure](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/).

<a id="prerequisites"></a>
## <a name="prerequisites"></a>Pré-requisitos

Este artigo se aplica a [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566) com a [atualização 3](https://go.microsoft.com/fwlink/?linkid=397827&amp;clcid=0x409) instalada.

<a id="wap"></a>
## <a name="web-application-projects-versus-web-site-projects"></a>Projetos de aplicativos Web versus projetos de site

O ASP.NET oferece a você uma opção entre dois tipos de projetos da Web: *projetos de aplicativos Web* e *projetos de site*. Recomendamos projetos de aplicativos Web para novo desenvolvimento, e este artigo aplica-se somente a projetos de aplicativos Web. Para obter mais informações, consulte [projetos de aplicativos Web versus projetos de site no Visual Studio](https://msdn.microsoft.com/library/dd547590(v=vs.120).aspx) no site do MSDN.

<a id="overview"></a>
## <a name="overview-of-web-application-project-creation"></a>Visão geral da criação do projeto de aplicativo Web

As etapas a seguir mostram como criar um projeto Web:

1. Clique em **novo projeto** na página **inicial** ou no menu **arquivo** .
2. Na caixa de diálogo **novo projeto** , clique em **Web** no painel esquerdo e em **aplicativo Web ASP.net** no painel central.

    ![Caixa de diálogo Novo Projeto](creating-web-projects-in-visual-studio/_static/image1.png)

    Você pode escolher **nuvem** no painel esquerdo para criar um [serviço de nuvem do Azure](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy), [serviço móvel do Azure](https://msdn.microsoft.com/library/windows/apps/dn629482.aspx)ou [WebJob do Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-webjobs). Este tópico não aborda esses modelos.
3. No painel direito, clique na caixa de seleção **adicionar Application insights ao projeto** se você quiser o monitoramento de integridade e uso do seu aplicativo. Para obter mais informações, consulte [Monitorar desempenho em aplicativos Web](https://azure.microsoft.com/documentation/articles/app-insights-web-monitor-performance/).
4. Especifique o **nome**do projeto, o **local**e outras opções e clique em **OK**.

    A caixa de diálogo **novo projeto ASP.net** é exibida.

    ![Caixa de diálogo Novo Projeto](creating-web-projects-in-visual-studio/_static/image2.png)
5. Clique em um modelo.

    ![Selecione um modelo](creating-web-projects-in-visual-studio/_static/image3.png)
6. Se você quiser adicionar suporte para estruturas adicionais não incluídas no modelo, clique na caixa de seleção apropriada. (No exemplo mostrado, você pode adicionar MVC e/ou API Web a um projeto Web Forms.)

    ![Adicionar estruturas](creating-web-projects-in-visual-studio/_static/image4.png)
7. <a id="testproj"></a>Se você quiser adicionar um projeto de teste de unidade, clique em **Adicionar testes de unidade**.

    ![Adicionar testes de unidade](creating-web-projects-in-visual-studio/_static/image5.png)
8. Se você quiser um método de autenticação diferente daquele que o modelo fornece por padrão, clique em **alterar autenticação**.

    ![Botão configurar autenticação](creating-web-projects-in-visual-studio/_static/image6.png)

    ![Caixa de diálogo configurar autenticação](creating-web-projects-in-visual-studio/_static/image7.png)

<a id="azurenewproj"></a>
### <a name="create-a-web-app-or-virtual-machine-in-azure"></a>Criar um aplicativo Web ou uma máquina virtual no Azure

O Visual Studio inclui recursos que facilitam o trabalho com os serviços do Azure para hospedar aplicativos Web. Por exemplo, você pode fazer todos os itens a seguir diretamente no IDE do Visual Studio:

- Crie e gerencie aplicativos Web ou máquinas virtuais que tornam seu aplicativo disponível pela Internet.
- Exiba os logs criados pelo aplicativo conforme eles são executados na nuvem.
- Execute no modo de depuração remotamente enquanto o aplicativo é executado na nuvem.
- Exiba e gerencie outros serviços do Azure, como bancos de dados SQL.

Você pode [criar uma conta do Azure](https://www.windowsazure.com/pricing/free-trial/) que inclui serviços básicos como aplicativos Web gratuitamente e, se você for um assinante do MSDN, poderá [ativar os benefícios](https://azure.microsoft.com/pricing/member-offers/visual-studio-subscriptions/) que oferecem créditos mensais para serviços adicionais do Azure. 

Por padrão, a caixa de diálogo **novo projeto ASP.net** permite que você crie um aplicativo Web ou uma máquina virtual para um novo projeto Web. Se você não quiser criar um novo aplicativo Web ou máquina virtual, desmarque a caixa de seleção **host na nuvem** .

![Criar recursos remotos](creating-web-projects-in-visual-studio/_static/image8.png)

A legenda da caixa de seleção pode ser **hospedada na nuvem** ou **criar recursos remotos**e, em ambos os casos, o efeito é o mesmo. Se você deixar a caixa de seleção marcada, o Visual Studio criará um aplicativo Web no serviço de Azure App por padrão. Você pode usar a caixa suspensa para alterá-la para uma **máquina virtual** , se preferir. Se você ainda não tiver entrado no Azure, suas credenciais do Azure serão solicitadas. Depois de entrar, uma caixa de diálogo permite que você configure os recursos que o Visual Studio criará para o seu projeto. A ilustração a seguir mostra a caixa de diálogo para um aplicativo Web; opções diferentes serão exibidas se você optar por criar uma máquina virtual.

![Definir configurações de Azure App](creating-web-projects-in-visual-studio/_static/image9.png)

Para obter mais informações sobre como usar esse processo para criar recursos do Azure, consulte Introdução [ao Azure e ASP.net](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet) e [criação de uma máquina virtual para um site com o Visual Studio](https://azure.microsoft.com/documentation/articles/virtual-machines-dotnet-create-visual-studio-powershell/).

O restante deste artigo fornece mais informações sobre os modelos disponíveis e suas opções. O artigo também apresenta a inicialização, o layout e a estrutura de temas usados nos modelos.

<a id="vs2013"></a>
## <a name="visual-studio-2013-web-project-templates"></a>Visual Studio 2013 modelos de projeto Web

O Visual Studio usa modelos para criar projetos da Web. Um modelo de projeto pode criar arquivos e pastas no novo projeto, instalar pacotes NuGet e fornecer código de exemplo para um aplicativo de trabalho rudimentar. Os modelos implementam os padrões da Web mais recentes e destinam-se a demonstrar as práticas recomendadas de como usar as tecnologias ASP.NET, bem como dar uma rápida introdução à criação de seu próprio aplicativo.

Visual Studio 2013 fornece as seguintes opções para modelos de projeto Web para projetos que visam o .NET 4,5 ou versões posteriores do .NET Framework:

- [Modelo vazio](#empty)
- [Modelo de Web Forms](#wf)
- [Modelo do MVC](#mvc)
- [Modelo de API Web](#webapi)
- [Modelo de aplicativo de página única](#spa)
- [Modelo de serviço móvel do Azure](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/)
- [Modelos do Visual Studio 2012](#vs2012)

Você também pode instalar uma extensão do Visual Studio que fornece um [modelo do Facebook](#facebook).

Para obter informações sobre como criar projetos destinados ao .NET 4, consulte [modelos do Visual Studio 2012](#vs2012) mais adiante neste tópico.

Para obter informações sobre como criar aplicativos ASP.NET para clientes móveis, consulte [suporte móvel no ASP.net](../../../mobile/overview.md).

<a id="empty"></a>
### <a name="empty-template"></a>Modelo vazio

O modelo vazio fornece o mínimo de pastas e arquivos para um aplicativo Web ASP.NET, como um arquivo de projeto (*. csproj* ou.* vbproj*) e um arquivo de *Web.config* . Você pode adicionar suporte para Web Forms, MVC e/ou API Web usando as caixas de seleção no rótulo **Adicionar pastas e referências principais para:** .

Para o modelo vazio, não há opções de autenticação disponíveis. A funcionalidade de autenticação é implementada em aplicativos de exemplo e o modelo vazio não cria um aplicativo de exemplo.

<a id="wf"></a>
### <a name="web-forms-template"></a>Modelo de Web Forms

O Web Forms Framework fornece os seguintes recursos que permitem que você crie rapidamente sites que são avançados em recursos de interface do usuário e de acesso a dados:

- Um designer WYSIWYG no Visual Studio.
- Controles de servidor que renderizam HTML e que você pode personalizar definindo propriedades e estilos.
- Uma vasta variedade de controles para acesso a dados e exibição de dados.
- Um modelo de evento que expõe eventos que você pode programar como você programaria um aplicativo cliente, como o WPF.
- Preservação automática de estado (dados) entre solicitações HTTP.

Em geral, a criação de um aplicativo Web Forms requer menos esforço de programação do que a criação do mesmo aplicativo usando a estrutura MVC do ASP.NET. No entanto, Web Forms não é apenas para o desenvolvimento rápido de aplicativos. Há muitos aplicativos comerciais e estruturas complexos criados com base em Web Forms.

Como uma página Web Forms e os controles na página geram automaticamente grande parte da marcação enviada para o navegador, você não tem o tipo de controle refinado no HTML que o ASP.NET MVC oferece. O modelo declarativo para configurar páginas e controles minimiza a quantidade de código que você precisa escrever, mas oculta parte do comportamento de HTML e HTTP. Por exemplo, nem sempre é possível especificar exatamente qual marcação pode ser gerada por um controle.

O Web Forms Framework não se presta tão prontamente quanto o ASP.NET MVC para práticas de desenvolvimento baseadas em padrões, como [desenvolvimento orientado a testes](http://en.wikipedia.org/wiki/Test-driven_development), [separação de preocupações](http://en.wikipedia.org/wiki/Separation_of_concerns), [inversão de controle](http://en.wikipedia.org/wiki/Inversion_of_control)e [injeção de dependência](http://en.wikipedia.org/wiki/Dependency_injection). Se você quiser escrever código fatorado dessa forma, você pode; Ele simplesmente não é tão automático quanto na estrutura MVC da ASP.NET. O projeto [ASP.NET Web Forms MVP](http://webformsmvp.com/) mostra uma abordagem que facilita a separação de preocupações e a capacidade de teste, mantendo o desenvolvimento rápido que Web Forms foi criado para oferecer. O Microsoft SharePoint foi criado em Web Forms MVP.

O modelo de Web Forms cria um aplicativo de Web Forms de exemplo que usa a [inicialização](#bootstrap) para fornecer recursos de design e temas responsivos. A ilustração a seguir mostra o home page.

![Web Forms aplicativo de modelo home page](creating-web-projects-in-visual-studio/_static/image10.png)

Para obter mais informações sobre Web Forms, consulte [ASP.NET Web Forms](https://asp.net/web-forms). Para obter informações sobre o que o modelo de Web Forms faz para você, consulte [criando um aplicativo básico de Web Forms usando Visual Studio 2013](https://blogs.msdn.com/b/webdev/archive/2013/12/19/building-a-basic-web-forms-application-using-visual-studio-2013.aspx).

<a id="mvc"></a>
### <a name="mvc-template"></a>Modelo MVC

O ASP.NET MVC foi projetado para facilitar as práticas de desenvolvimento baseadas em padrões, como [desenvolvimento controlado por testes](http://en.wikipedia.org/wiki/Test-driven_development), [separação de preocupações](http://en.wikipedia.org/wiki/Separation_of_concerns), [inversão de controle](http://en.wikipedia.org/wiki/Inversion_of_control)e [injeção de dependência](http://en.wikipedia.org/wiki/Dependency_injection). A estrutura incentiva a separação da camada de lógica de negócios de um aplicativo Web de sua camada de apresentação. Dividindo o aplicativo em modelos (M), exibições (V) e controladores (C), o ASP.NET MVC pode facilitar o gerenciamento da complexidade em aplicativos maiores.

Com o ASP.NET MVC, você trabalha mais diretamente com HTML e HTTP do que no Web Forms. Por exemplo, Web Forms pode preservar automaticamente o estado entre solicitações HTTP, mas você precisa codificar explicitamente no MVC. A vantagem do modelo MVC é que ele permite que você assuma o controle total sobre exatamente o que seu aplicativo está fazendo e como ele se comporta no ambiente da Web. A desvantagem é que você precisa escrever mais código.

O MVC foi projetado para ser extensível, fornecendo aos desenvolvedores de energia a capacidade de personalizar a estrutura para suas necessidades de aplicativo. Além disso, o código-fonte do ASP.NET MVC está disponível em uma licença OSI.

O modelo MVC cria um aplicativo MVC 5 de exemplo que usa a [inicialização](#bootstrap) para fornecer recursos de design e temas responsivos. A ilustração a seguir mostra o home page.

![Aplicativo de exemplo MVC](creating-web-projects-in-visual-studio/_static/image11.png)

Para obter mais informações sobre o MVC, consulte [ASP.NET MVC](https://asp.net/mvc). Para obter informações sobre como selecionar o modelo MVC 4, consulte [modelos do Visual Studio 2012](#vs2012) mais adiante neste artigo.

<a id="webapi"></a>
### <a name="web-api-template"></a>Modelo de API Web

O modelo de API Web cria um serviço Web de exemplo baseado na API Web, incluindo páginas de ajuda da API com base no MVC.

O ASP.NET Web API é uma estrutura que facilita o desenvolvimento de serviços HTTP que alcançam uma ampla variedade de clientes, incluindo navegadores e dispositivos móveis. ASP.NET Web API é uma plataforma ideal para a criação de serviços RESTful no .NET Framework.

O modelo de API Web cria um serviço Web de exemplo. As ilustrações a seguir mostram exemplos de páginas de ajuda.

![Página de ajuda da API Web](creating-web-projects-in-visual-studio/_static/image12.png)

![Página de ajuda da API Web para API GET](creating-web-projects-in-visual-studio/_static/image13.png)

Para obter mais informações sobre API Web, consulte [ASP.NET Web API](https://asp.net/web-api).

<a id="spa"></a>
### <a name="single-page-application-template"></a>Modelo de aplicativo de página única

O modelo SPA (aplicativo de página única) cria um aplicativo de exemplo que usa JavaScript, HTML 5 e [KnockoutJS](http://knockoutjs.com/) no cliente e ASP.NET Web API no servidor.

A única opção de autenticação para o modelo SPA é [contas de usuário individuais](#indauth).

A ilustração a seguir mostra o estado inicial do aplicativo de exemplo criado pelo modelo SPA.

![Aplicativo de exemplo SPA](creating-web-projects-in-visual-studio/_static/image14.png)

Para obter informações sobre como criar um aplicativo usando o modelo SPA, consulte [API Web-serviços de autenticação externa](../../../web-api/overview/security/external-authentication-services.md).

Para obter mais informações sobre aplicativos de página única do ASP.NET e sobre modelos de SPA adicionais que usam estruturas JavaScript diferentes de KnockoutJS, consulte os seguintes recursos:

- [Aplicativo de página única do ASP.net](../../../single-page-application/index.md).
- [Compreendendo os recursos de segurança no modelo SPA para VS2013 RC](https://blogs.msdn.com/b/webdev/archive/2013/09/20/understanding-security-features-in-spa-template.aspx)
- [Aplicativos de página única: Crie aplicativos Web modernos e responsivos com o ASP.NET](https://msdn.microsoft.com/magazine/dn463786.aspx)

<a id="facebook"></a>
### <a name="facebook-template"></a>Modelo do Facebook

Você pode instalar uma [extensão do Visual Studio que fornece um modelo do Facebook](https://go.microsoft.com/fwlink/?LinkID=509965&amp;clcid=0x409). Este modelo cria um aplicativo de exemplo que é projetado para ser executado no site do Facebook. Ele se baseia no ASP.NET MVC e usa a API da Web para a funcionalidade de atualização em tempo real.

Não há opções de autenticação disponíveis para o modelo do Facebook porque os aplicativos do Facebook são executados no site do Facebook e dependem da autenticação do Facebook.

Para obter mais informações sobre aplicativos do ASP.NET Facebook, consulte [atualizando a API do Facebook do MVC](https://blogs.msdn.com/b/webdev/archive/2014/06/10/updating-the-mvc-facebook-api.aspx).

<a id="vs2012"></a>
### <a name="visual-studio-2012-templates"></a>Modelos do Visual Studio 2012

A caixa de diálogo de criação do projeto Web Visual Studio 2013 não fornece acesso a alguns modelos que estavam disponíveis no Visual Studio 2012. Se você quiser usar um desses modelos, poderá clicar no nó do Visual Studio 2012 no painel esquerdo da caixa de diálogo novo projeto do Visual Studio.

![Modelos do Visual Studio 2012](creating-web-projects-in-visual-studio/_static/image15.png)

O nó do **Visual Studio 2012** permite que você escolha os seguintes modelos da Web aos quais você não tem acesso na lista padrão de modelos para Visual Studio 2013:

- Aplicativo Web ASP.NET MVC 4
- Aplicativo Web de entidades de dados dinâmicos ASP.NET
- Controle de servidor ASP.NET AJAX
- Extensor de controle de servidor ASP.NET AJAX
- Controle de servidor ASP.NET

<a id="bootstrap"></a>
## <a name="bootstrap-in-the-visual-studio-2013-web-project-templates"></a>Inicialização no Visual Studio 2013 modelos de projeto Web

Os modelos de projeto Visual Studio 2013 usam a [inicialização](http://getbootstrap.com/), um layout e estrutura de temas criados pelo Twitter. A inicialização usa o CSS3 para fornecer design responsivo, o que significa que os layouts podem se adaptar dinamicamente a diferentes tamanhos de janela do navegador. Por exemplo, em uma janela ampla do navegador, o home page criado pelo modelo de Web Forms é semelhante à ilustração a seguir:

![Web Forms aplicativo de modelo home page](creating-web-projects-in-visual-studio/_static/image16.png)

Tornar a janela mais estreita e as colunas organizadas horizontalmente são movidas para a organização vertical:

![Organização da coluna vertical de Bootstrap](creating-web-projects-in-visual-studio/_static/image17.png)

Restrinja a janela um pouco mais e o menu superior horizontal se transforma em um ícone no qual você pode clicar para expandir para um menu verticalmente orientado:

![Ícone do menu Bootstrap](creating-web-projects-in-visual-studio/_static/image18.png)

![Menu vertical de Bootstrap](creating-web-projects-in-visual-studio/_static/image19.png)

Você também pode usar o recurso de ti da Bootstrap para efetivar facilmente uma alteração na aparência do aplicativo. Por exemplo, você pode executar as etapas a seguir para alterar o tema.

1. No navegador, vá para [http://Bootswatch.com](http://Bootswatch.com) , escolha um tema e clique em **baixar**. (Isso baixa o *bootstrap. min. css* por padrão; se você quiser examinar o código CSS, obtenha *bootstrap. css* em vez da versão reduzidos.)
2. Copie o conteúdo do arquivo CSS baixado.
3. No Visual Studio, crie um novo arquivo de **folha de estilo** chamado *Bootstrap-Theme. css* na pasta *Content* e cole o código CSS baixado nele.
4. Abra o *aplicativo \_ iniciar/Bundle.config* e altere *bootstrap. css* para *Bootstrap-Theme. css*.

Execute o projeto novamente e o aplicativo terá uma nova aparência. A ilustração a seguir mostra o efeito do tema Amelia:

![Tema de Amelia de inicialização](creating-web-projects-in-visual-studio/_static/image20.png)

Muitos temas de Bootstrap estão disponíveis, versões gratuitas e Premium. A inicialização também oferece uma ampla variedade de componentes de interface do usuário, como menus [suspensos](http://twitter.github.io/bootstrap/components.html#dropdowns), [grupos de botões](http://twitter.github.io/bootstrap/components.html#buttonGroups)e [ícones](http://twitter.github.io/bootstrap/base-css.html#images). Para obter mais informações sobre a inicialização, consulte [o site de inicialização](http://twitter.github.io/bootstrap/).

Se você usar o designer de Web Forms no Visual Studio, observe que o designer não dá suporte a CSS3, portanto, ele não mostra com precisão todos os efeitos de temas de Bootstrap ou alterações de layout responsivas. No entanto, as páginas Web Forms serão exibidas corretamente quando exibidas com um navegador.

<a id="add"></a>
## <a name="adding-support-for-additional-frameworks"></a>Adicionando suporte para estruturas adicionais

Quando você seleciona um modelo, a caixa de seleção para as estruturas usadas pelo modelo é selecionada automaticamente. Por exemplo, se você selecionar o modelo de **Web Forms** , a caixa de seleção **Web Forms** será marcada e você não poderá limpá-la.

![Selecione um modelo](creating-web-projects-in-visual-studio/_static/image21.png)

![Adicionar estruturas](creating-web-projects-in-visual-studio/_static/image22.png)

Você pode marcar a caixa de seleção para uma estrutura que não está incluída no modelo a fim de adicionar suporte para essa estrutura quando o projeto é criado. Por exemplo, para habilitar o uso de páginas Web Forms *. aspx* quando você tiver selecionado o modelo MVC, marque a caixa de seleção **Web Forms** . Ou para habilitar o MVC quando você estiver usando o modelo de Web Forms, clique na caixa de seleção **MVC** . A adição de uma estrutura habilita o tempo de design, bem como o suporte a tempo de execução. Por exemplo, se você adicionar suporte MVC a um projeto Web Forms, poderá Scaffold controladores e modos de exibição.

Se você combinar Web Forms e MVC em um projeto e habilitar [URLs amigáveis](http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx) no Web Forms, pode haver problemas de roteamento inesperados em que uma URL tem vários destinos possíveis. As rotas definidas primeiro terão precedência. Por exemplo, se você tiver um `Home` controlador e uma página *Home. aspx* , a `http://contoso.com/home` URL irá para *Home. aspx* se você chamar o `EnableFriendlyUrls` método antes de chamar o `MapRoute` método em *RouteConfig.cs*, ou a mesma URL irá para o modo de exibição padrão do `Home` controlador se você chamar `MapRoute` antes `EnableFriendlyUrls` .

A adição de uma estrutura não adiciona nenhuma funcionalidade de aplicativo de exemplo. Por exemplo, se você adicionar Web Forms suporte quando tiver selecionado o modelo MVC, nenhum arquivo *Default. aspx* Home Page será criado. Somente as pastas, os arquivos e as referências necessárias para dar suporte à estrutura são adicionados. Por esse motivo, a adição de estruturas não altera as opções de autenticação, que são implementadas pelo código em aplicativos de exemplo criados pelos modelos. Por exemplo, se você selecionar o modelo vazio e adicionar suporte a Web Forms ou MVC, o botão **configurar autenticação** ainda estará desabilitado.

As seções a seguir descrevem brevemente o efeito de cada caixa de seleção.

### <a name="add-web-forms-support"></a>Adicionar suporte a Web Forms

Cria pastas *de \_ dados* e *modelos* de aplicativo vazios e um arquivo *global. asax* . Eles já são criados por todos os modelos diferentes do modelo vazio, portanto, marcar a caixa de seleção Web Forms não faz diferença para outros modelos.

O modelo de Web Forms permite URLs amigáveis por padrão, mas quando você adiciona Web Forms suporte a outros modelos marcando as URLs amigáveis da caixa de seleção Web Forms não são habilitadas automaticamente.

### <a name="add-mvc-support"></a>Adicionar suporte MVC

Instala os pacotes do MVC, do Razor e do Web pages NuGet, cria * \_ dados de aplicativo*vazios, *controladores*, *modelos*e pastas de *modos de exibição* , cria a pasta de * \_ início do aplicativo* com o arquivo *RouteConfig.cs* e cria o arquivo *global. asax* .

### <a name="add-web-api-support"></a>Adicionar suporte à API Web

Instala WebApi e Newtonsoft.Jsem pacotes NuGet, cria pastas *de \_ dados de aplicativo*vazias, *controladores*e *modelos* , cria a pasta de * \_ início do aplicativo* com o arquivo *WebApiConfig.cs* e cria o arquivo *global. asax* .

<a id="auth"></a>
## <a name="authentication-methods"></a>Métodos de autenticação

O Visual Studio 2013 oferece várias opções de autenticação para os modelos de API do Web Forms, MVC e Web:

- [Sem autenticação](#noauth)
- [Contas de usuário individuais](#indauth) (ASP.net Identity, anteriormente conhecida como associação ASP.net)
- [Contas organizacionais](#orgauth) (Windows Server Active Directory ou Azure Active Directory)
- [Autenticação do Windows](#winauth) (intranet)

![Caixa de diálogo configurar autenticação](creating-web-projects-in-visual-studio/_static/image23.png)

<a id="noauth"></a>

### <a name="no-authentication"></a>Sem Autenticação

Se você **não selecionar nenhuma autenticação**, o aplicativo de exemplo não conterá nenhuma página da Web para fazer logon, nenhuma interface do usuário indicando quem está conectado, nenhuma classe de entidade para um banco de dados de associação e nenhuma cadeia de conexão para um banco de dados de associação.

<a id="indauth"></a>
### <a name="individual-user-accounts"></a>Contas Individuais de Usuário

Se você selecionar **contas de usuário individuais**, o aplicativo de exemplo será configurado para usar ASP.net Identity (anteriormente conhecido como associação ASP.net) para autenticação de usuário. ASP.NET Identity permite que um usuário registre uma conta, criando um nome de usuário e senha no site ou entrando com provedores sociais, como o Facebook, o Google, a conta da Microsoft ou o Twitter. O armazenamento de dados padrão para perfis de usuário no ASP.NET Identity é um banco de dado SQL Server LocalDB, que pode ser implantado no SQL Server ou no banco de dados SQL do Azure para o site de produção.

Em Visual Studio 2013 esses recursos são os mesmos do Visual Studio 2012, mas o código subjacente do sistema de associação ASP.NET foi reescrito. As vantagens da nova base de código incluem o seguinte:

- O novo sistema de associação é baseado em [OWIN](http://owin.org/) em vez do módulo de autenticação de formulários do ASP.net. Isso significa que você pode usar o mesmo mecanismo de autenticação se estiver usando Web Forms ou MVC no IIS, ou se você estiver hospedando automaticamente a API Web ou o Signalr.
- O novo banco de dados de associação é gerenciado pelo Entity Framework Code First, e todas as tabelas são representadas por classes de entidade que você pode modificar. Isso significa que você pode personalizar facilmente o esquema de banco de dados e a interface do usuário da Web relacionada ao perfil para atender às suas próprias necessidades, e pode facilmente implantar suas atualizações usando o Migrações do Code First.

O novo sistema de associação é implementado automaticamente nos novos modelos e pode ser implementado manualmente em qualquer projeto que tenha como destino o .NET 4,5 ou posterior.

ASP.NET Identity é uma boa opção se você estiver criando um site da Internet que é principalmente para clientes externos. Se sua organização usa Active Directory ou o Office 365 e você deseja criar um projeto que habilite o logon único para funcionários e parceiros comerciais, a opção de **contas organizacionais** pode ser uma opção melhor.

Para obter mais informações sobre a opção de contas de usuário individuais, consulte os seguintes recursos:

- [www.ASP.net/Identity](../../../identity/index.md). Documentação sobre ASP.NET Identity no site da ASP.NET.
- [Crie um aplicativo ASP.NET MVC 5 com o Facebook e o Google OAuth2 e o logon OpenID](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md). Também mostra como personalizar os dados de perfil do usuário.
- [API Web-serviços de autenticação externa](../../../web-api/overview/security/external-authentication-services.md)
- [Adicionando logons externos ao seu aplicativo ASP.NET no Visual Studio 2013](https://blogs.msdn.com/b/webdev/archive/2013/06/27/adding-external-logins-to-your-asp-net-application-in-visual-studio-2013.aspx)

<a id="orgauth"></a>
### <a name="organizational-accounts"></a>Contas organizacionais

Se você selecionar **contas organizacionais**, o aplicativo de exemplo será configurado para usar o Windows Identity Foundation (WIF) para autenticação com base em contas de usuário no Azure Active Directory (Azure AD, que inclui o Office 365) ou o Windows Server Active Directory. Para obter mais informações, consulte [Opções de autenticação de conta institucional](#orgauthoptions) mais adiante neste tópico.

<a id="winauth"></a>
### <a name="windows-authentication"></a>Autenticação do Windows

Se você selecionar **autenticação do Windows**, o aplicativo de exemplo será configurado para usar o módulo IIS de autenticação do Windows para autenticação. O aplicativo exibirá o domínio e a ID de usuário da conta do computador local ou do Active Directory que está conectada ao Windows, mas não incluirá o registro do usuário ou a interface do logon. Essa opção destina-se a sites da intranet.

Como alternativa, você pode criar um site de intranet que usa a autenticação do AD escolhendo a [opção local em contas organizacionais](#orgauthonprem). A opção local usa o Windows Identity Foundation (WIF) em vez do módulo de autenticação do Windows. Algumas etapas adicionais são necessárias para configurar a opção local, mas o WIF habilita recursos que não estão disponíveis com o módulo de autenticação do Windows. Por exemplo, com o WIF, você pode configurar o acesso ao aplicativo em Active Directory e consultar dados do diretório.

<a id="orgauthoptions"></a>
## <a name="organizational-account-authentication-options"></a>Opções de autenticação da conta organizacional

A caixa de diálogo **configurar autenticação** fornece várias opções para Azure Active Directory (Azure AD, que inclui o Office 365) ou autenticação de conta do Windows Server Active Directory (AD):

- [Nuvem-única organização](#orgauthsingle) (Azure AD ou AD usando a integração de diretórios com o Azure AD)
- [Nuvem-várias organizações](#orgauthmulti) (Azure AD ou AD usando a integração de diretórios com o Azure AD)
- [Local](#orgauthonprem) (AD)

Se você quiser experimentar uma das opções do Azure AD, mas ainda não tiver uma conta, [clique aqui para se inscrever em uma conta do Azure ad](https://go.microsoft.com/fwlink/?LinkId=309942).

> [!NOTE]
> Se você escolher uma das opções do Azure AD, seu projeto exigirá um banco de dados e você precisará entrar em uma conta de administrador global para seu locatário do Azure AD. Insira o nome e a senha de uma conta institucional (por exemplo, admin@contoso.onmicrosoft.com ) que tenha permissões administrativas para seu locatário do Azure AD.
> 
> **Não insira as credenciais para um conta Microsoft (por exemplo, contoso@hotmail.com ) na caixa de diálogo de entrada.**

<a id="orgauthsingle"></a>
### <a name="cloud---single-organization-authentication"></a>Nuvem-autenticação de uma única organização

![Autenticação de organização única](creating-web-projects-in-visual-studio/_static/image24.png)

Escolha esta opção se desejar habilitar a autenticação para contas de usuário definidas em um [locatário](https://technet.microsoft.com/library/jj573650.aspx)do Azure AD. Por exemplo, o site é contoso.com e será disponibilizado para os funcionários da empresa contoso que estão no locatário contoso.onmicrosoft.com. Você não conseguirá configurar o Azure AD para permitir que usuários de outros locatários acessem o aplicativo.

#### <a name="domain"></a>Domain

Insira o domínio do Azure AD no qual você deseja configurar o aplicativo, por exemplo: `contoso.onmicrosoft.com` . Se você tiver um [domínio personalizado](http://www.cloudidentity.com/blog/2013/04/14/adding-a-custom-domain-to-your-windows-azure-ad/), como `contoso.com` em vez de `contoso.onmicrosoft.com` , você pode inseri-lo aqui.

#### <a name="access-level"></a>Nível de acesso

Se o aplicativo precisar consultar ou atualizar as informações do diretório usando o API do Graph, escolha **logon único, ler dados do diretório** ou **logon único, ler e gravar dados do diretório**. Caso contrário, escolha **logon único**. Para obter mais informações, consulte [níveis de acesso do aplicativo](https://msdn.microsoft.com/library/windowsazure/b08d91fa-6a64-4deb-92f4-f5857add9ed8#BKMK_AccessLevels) e [usando o API do Graph para consultar o Azure ad](https://msdn.microsoft.com/library/windowsazure/dn151791.aspx).

#### <a name="application-id-uri"></a>URI da ID de Aplicativo

Por padrão, o modelo cria um URI de ID de aplicativo para você anexando o nome do projeto ao domínio do Azure AD. Por exemplo, se o nome do projeto for `Example` e o domínio for `contoso.onmicrosoft.com` , o URI da ID do aplicativo se tornará `https://contoso.onmicrosoft.com/Example` . Se você quiser especificar manualmente o URI da ID do aplicativo, expanda a seção **mais opções** e insira o URI da ID do aplicativo na caixa de texto. O URI da ID do aplicativo deve começar com `https://` .

Por padrão, se um aplicativo já provisionado no Azure AD tiver o mesmo URI de ID de aplicativo que o Visual Studio está usando para o projeto, o projeto será conectado ao aplicativo existente em vez de provisionar um novo. Se você quiser que um novo aplicativo seja provisionado nesse caso, desmarque a caixa de seleção **substituir a entrada do aplicativo se já existir uma com a mesma ID** .

Se a caixa de seleção **substituir** estiver desmarcada e o Visual Studio encontrar um aplicativo existente com o mesmo URI de ID do aplicativo, ele criará um novo URI acrescentando um número ao URI que ele usará. Por exemplo, suponha que o nome do projeto seja `Example` , deixe a caixa de texto em branco, desmarque a caixa de seleção **substituir** e o locatário do Azure ad já tem um aplicativo com o URI `https://contoso.onmicrosoft.com/Example` . Nesse caso, um novo aplicativo será provisionado com um URI de ID de aplicativo como `https://contoso.onmicrosoft.com/Example_20130619330903` .

#### <a name="provisioning-the-application-in-azure-ad"></a>Provisionando o aplicativo no Azure AD

Para provisionar o aplicativo no Azure AD ou conectar o projeto a um aplicativo existente, o Visual Studio precisa das credenciais de um administrador global para o domínio. Ao clicar em **OK** na caixa de diálogo **configurar autenticação** , será solicitado que você forneça o nome de usuário e a senha de um administrador global para o domínio especificado. Posteriormente, quando você clicar em **criar projeto** na caixa de diálogo **novo projeto ASP.net** , o Visual Studio provisionará o aplicativo no Azure AD. Observe que, como parte desse processo, o Visual Studio incorpora valores de segredo do cliente no arquivo de Web.config que expiram um ano após a criação.

Para obter informações sobre como criar aplicativos que usam a autenticação **de uma organização de nuvem única** , consulte os seguintes recursos:

- [Autenticação do Azure](../2012/windows-azure-authentication.md)
- [Adicionando logon ao seu aplicativo Web usando o Azure AD](https://msdn.microsoft.com/library/windowsazure/dn151790.aspx)
- [Desenvolver aplicativos do ASP.NET com o Azure Active Directory](../../../identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory.md)
- [Proteger ASP.NET Web API com os componentes do Azure AD e do Microsoft OWIN](https://msdn.microsoft.com/magazine/dn463788.aspx)

Os tutoriais ainda não foram atualizados para Visual Studio 2013; alguns dos tutoriais que o orientam a fazer manualmente são automatizados no Visual Studio 2013.

<a id="orgauthmulti"></a>
### <a name="cloud---multi-organization-authentication"></a>Nuvem – autenticação de várias organizações

![Autenticação de várias organizações](creating-web-projects-in-visual-studio/_static/image25.png)

Escolha esta opção se desejar habilitar a autenticação para contas de usuário definidas em vários [locatários](https://technet.microsoft.com/library/jj573650.aspx)do Azure AD. Por exemplo, o site é contoso.com e será disponibilizado para os funcionários da empresa contoso que estão no locatário contoso.onmicrosoft.com e os funcionários da empresa Fabrikam que estão no locatário fabrikam.onmicrosoft.com.

As configurações inseridas e a etapa de provisionamento do aplicativo são semelhantes à [autenticação de uma única organização](#orgauthsingle).

Para obter informações sobre como criar aplicativos que usam a autenticação **de várias organizações na nuvem** , consulte os seguintes recursos:

- [Integração fácil de aplicativos Web com o Azure Active Directory, ASP.NET &amp; O Visual Studio](https://blogs.msdn.com/b/active_directory_team_blog/archive/2013/06/26/improved-windows-azure-active-directory-integration-with-asp-net-amp-visual-studio.aspx) no blog da equipe do Active Directory.
- Tutorial [de desenvolvimento de aplicativos Web Multilocatários com o Azure ad](https://msdn.microsoft.com/library/windowsazure/dn151789.aspx) . O tutorial ainda não foi atualizado para Visual Studio 2013; parte do que o tutorial orienta você a fazer manualmente é automatizado no Visual Studio 2013.
- [Você precisa se inscrever com seu próprio aplicativo ASP.net de várias organizações para poder entrar](http://www.cloudidentity.com/blog/2013/10/26/you-have-to-sign-up-with-your-own-multiple-organizations-asp-net-app-before-you-can-sign-in/). Blog de Vittorio Bertocci que explica como resolver um problema comum que as pessoas encontram ao criar um projeto que usa a autenticação de várias organizações.

<a id="orgauthonprem"></a>
### <a name="on-premises-organizational-authentication"></a>Autenticação organizacional local

![Autenticação organizacional local](creating-web-projects-in-visual-studio/_static/image26.png)

Escolha esta opção se desejar habilitar a autenticação para contas de usuário definidas no Windows Server Active Directory (AD) e não quiser usar o Azure AD. Você pode usar essa opção para criar um site de intranet ou um site da Internet. Para um site da Internet, use Serviços de Federação do Active Directory (AD FS) (ADFS) para fornecer acesso ao AD. Para obter mais informações, consulte [usar a opção de autenticação organizacional local (ADFS) com ASP.net no Visual Studio 2013](http://www.cloudidentity.com/blog/2014/02/12/use-the-on-premises-organizational-authentication-option-adfs-with-asp-net-in-visual-studio-2013/).

Para um site de intranet, como alternativa, você pode escolher a [autenticação do Windows](#winauth) em vez desta opção. Para a opção de autenticação do Windows, você não precisa fornecer uma URL de documento de metadados. No entanto, a autenticação do Windows não oferece a capacidade de controlar o acesso do aplicativo no Active Directory ou consultar dados do diretório.

#### <a name="on-premises-authority"></a>Autoridade local

Insira uma URL que aponte para o documento de metadados. O documento de metadados contém as coordenadas da autoridade. O aplicativo usará essas coordenadas para direcionar o fluxo de logon da Web.

#### <a name="application-id-uri"></a>URI da ID de Aplicativo

Forneça um URI exclusivo que o AD possa usar para identificar esse aplicativo ou deixe em branco para permitir que o Visual Studio crie um.

<a id="nextsteps"></a>
## <a name="next-steps"></a>Próximas etapas

Este documento forneceu uma ajuda básica para criar um novo projeto Web ASP.NET no Visual Studio 2013. Para obter mais informações sobre como usar o para o desenvolvimento para Web do Visual Studio, consulte [https://www.asp.net/visual-studio/](../../index.md) .
