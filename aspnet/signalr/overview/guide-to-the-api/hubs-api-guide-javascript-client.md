---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
title: Guia da API de hubs do Signalr ASP.NET – cliente JavaScript | Microsoft Docs
author: bradygaster
description: Este documento fornece uma introdução ao uso da API de hubs para o Signalr versão 2 em clientes JavaScript, como navegadores e Applicat da Windows Store (WinJS)...
ms.author: bradyg
ms.date: 01/15/2019
ms.assetid: a9fd4dc0-1b96-4443-82ca-932a5b4a8ea4
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
msc.type: authoredcontent
ms.openlocfilehash: 8befe133c3627dac1f7d011959c68e2054d345da
ms.sourcegitcommit: c62ec20b453cee3249eb894ecd75013b57d078f0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/21/2020
ms.locfileid: "92345429"
---
# <a name="aspnet-signalr-hubs-api-guide---javascript-client"></a>Guia da API de hubs do Signalr ASP.NET – cliente JavaScript

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Este documento fornece uma introdução ao uso da API de hubs para o Signalr versão 2 em clientes JavaScript, como navegadores e aplicativos de WinJS (Windows Store).
>
> A API de hubs de Signalr permite que você faça RPCs (chamadas de procedimento remoto) de um servidor para clientes conectados e de clientes para o servidor. No código do servidor, você define métodos que podem ser chamados por clientes e chama métodos que são executados no cliente. No código do cliente, você define métodos que podem ser chamados do servidor e chama métodos que são executados no servidor. O signalr cuida de todo o direcionamento de cliente para servidor para você.
>
> O signalr também oferece uma API de nível inferior chamada conexões persistentes. Para obter uma introdução ao Signalr, hubs e conexões persistentes, consulte [introdução ao signalr](../getting-started/introduction-to-signalr.md).
>
> ## <a name="software-versions-used-in-this-topic"></a>Versões de software usadas neste tópico
>
>
> - [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
> - .NET 4.5
> - Sinalização versão 2
>
>
>
> ## <a name="previous-versions-of-this-topic"></a>Versões anteriores deste tópico
>
> Para obter informações sobre versões anteriores do Signalr, confira [versões mais antigas do signalr](../older-versions/index.md).
>
> ## <a name="questions-and-comments"></a>Perguntas e comentários
>
> Deixe comentários sobre como você gostou deste tutorial e o que poderíamos melhorar nos comentários na parte inferior da página. Se você tiver dúvidas que não estão diretamente relacionadas ao tutorial, poderá lançá-las no fórum do [signalr ASP.net](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [stackoverflow.com](http://stackoverflow.com/).

## <a name="overview"></a>Visão geral

Este documento contém as seguintes seções:

- [O proxy gerado e o que ele faz por você](#genproxy)

    - [Quando usar o proxy gerado](#cantusegenproxy)
- [Configuração do cliente](#clientsetup)

    - [Como referenciar o proxy gerado dinamicamente](#dynamicproxy)
    - [Como criar um arquivo físico para o proxy gerado pelo Signalr](#manualproxy)
- [Como estabelecer uma conexão](#establishconnection)

    - [$. Connection. Hub é o mesmo objeto que $. hubConnection () cria](#connequivalence)
    - [Execução assíncrona do método Start](#asyncstart)
- [Como estabelecer uma conexão entre domínios](#crossdomain)
- [Como configurar a conexão](#configureconnection)

    - [Como especificar parâmetros de cadeia de caracteres de consulta](#querystring)
    - [Como especificar o método de transporte](#transport)
- [Como obter um proxy para uma classe de Hub](#getproxy)
- [Como definir métodos no cliente que o servidor pode chamar](#callclient)
- [Como chamar métodos de servidor do cliente](#callserver)
- [Como tratar eventos de tempo de vida da conexão](#connectionlifetime)
- [Como tratar erros](#handleerrors)
- [Como habilitar o log do lado do cliente](#logging)

Para obter a documentação sobre como programar os clientes do servidor ou do .NET, consulte os seguintes recursos:

- [Guia da API de hubs de signalr-servidor](hubs-api-guide-server.md)
- [Guia da API de hubs do signalr-cliente .NET](hubs-api-guide-net-client.md)

O componente de servidor Signalr 2 só está disponível no .NET 4,5 (embora haja um cliente .NET para o Signalr 2 no .NET 4,0).

<a id="genproxy"></a>

## <a name="the-generated-proxy-and-what-it-does-for-you"></a>O proxy gerado e o que ele faz por você

Você pode programar um cliente JavaScript para se comunicar com um serviço de sinalização com ou sem um proxy que o Signalr gera para você. O que o proxy faz para você é simplificar a sintaxe do código que você usa para se conectar, escrever métodos que o servidor chama e chamar métodos no servidor.

Quando você escreve o código para chamar métodos de servidor, o proxy gerado permite que você use a sintaxe que parece que você estava executando uma função local: você pode gravar `serverMethod(arg1, arg2)` em vez de `invoke('serverMethod', arg1, arg2)` . A sintaxe de proxy gerada também permite um erro imediato e inteligível do lado do cliente se você digitar indigitadamente um nome de método de servidor. E se você criar manualmente o arquivo que define os proxies, também poderá obter suporte IntelliSense para escrever código que chama métodos de servidor.

Por exemplo, suponha que você tenha a seguinte classe de Hub no servidor:

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample1.cs?highlight=1,3,5)]

Os exemplos de código a seguir mostram a aparência do código JavaScript para invocar o `NewContosoChatMessage` método no servidor e receber invocações do `addContosoChatMessageToPage` método do servidor.

**Com o proxy gerado**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample2.js?highlight=1-2,8)]

**Sem o proxy gerado**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample3.js?highlight=2-3,9)]

<a id="cantusegenproxy"></a>

### <a name="when-to-use-the-generated-proxy"></a>Quando usar o proxy gerado

Se você quiser registrar vários manipuladores de eventos para um método de cliente chamado pelo servidor, não poderá usar o proxy gerado. Caso contrário, você pode optar por usar o proxy gerado ou não com base na sua preferência de codificação. Se você optar por não usá-lo, não precisará fazer referência à URL "signalr/hubs" em um `script` elemento no seu código de cliente.

<a id="clientsetup"></a>

## <a name="client-setup"></a>Configuração do cliente

Um cliente JavaScript requer referências ao jQuery e ao arquivo JavaScript do Signalr Core. A versão do jQuery deve ser 1.6.4 ou as principais versões posteriores, como 1.7.2, 1.8.2 ou 1.9.1. Se você decidir usar o proxy gerado, também precisará de uma referência ao arquivo JavaScript de proxy gerado pelo Signalr. O exemplo a seguir mostra como as referências podem parecer em uma página HTML que usa o proxy gerado.

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample4.html)]

Essas referências devem ser incluídas nesta ordem: jQuery First, Core do Signalr depois disso e proxies do Signalr por último.

<a id="dynamicproxy"></a>

### <a name="how-to-reference-the-dynamically-generated-proxy"></a>Como referenciar o proxy gerado dinamicamente

No exemplo anterior, a referência ao proxy gerado pelo Signalr é gerada dinamicamente o código JavaScript, não para um arquivo físico. O signalr cria o código JavaScript para o proxy imediatamente e o serve para o cliente em resposta à URL "/signalr/hubs". Se você especificou uma URL base diferente para conexões de Signalr no servidor em seu `MapSignalR` método, a URL para o arquivo de proxy gerado dinamicamente será sua URL personalizada com "/hubs" acrescentado a ela.

> [!NOTE]
> Para clientes JavaScript do Windows 8 (Windows Store), use o arquivo de proxy físico em vez de um gerado dinamicamente. Para obter mais informações, consulte [como criar um arquivo físico para o proxy gerado pelo signalr](#manualproxy) mais adiante neste tópico.

Em uma exibição do Razor do ASP.NET MVC 4 ou 5, use o til para se referir à raiz do aplicativo em sua referência de arquivo de proxy:

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample5.html)]

Para obter mais informações sobre como usar o Signalr no MVC 5, consulte [introdução com signalr e MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).

Em uma exibição Razor do ASP.NET MVC 3, use `Url.Content` para a referência do arquivo de proxy:

[!code-cshtml[Main](hubs-api-guide-javascript-client/samples/sample6.cshtml)]

Em um aplicativo de Web Forms ASP.NET, use `ResolveClientUrl` para a referência de arquivo de proxies ou registre-o por meio do ScriptManager usando um caminho relativo de raiz de aplicativo (começando com um til):

[!code-aspx[Main](hubs-api-guide-javascript-client/samples/sample7.aspx)]

Como regra geral, use o mesmo método para especificar a URL "/signalr/hubs" que você usa para arquivos CSS ou JavaScript. Se você especificar uma URL sem usar um til, em alguns cenários, seu aplicativo funcionará corretamente quando você testar no Visual Studio usando IIS Express, mas falhará com um erro 404 quando você implantar no IIS completo. Para obter mais informações, consulte **Resolvendo referências a Root-Level recursos** em [servidores Web no Visual Studio para projetos Web do ASP.net](https://msdn.microsoft.com/library/58wxa9w5.aspx) no site do MSDN.

Ao executar um projeto Web no Visual Studio 2017 no modo de depuração e, se você usar o Internet Explorer como navegador, poderá ver o arquivo de proxy em **Gerenciador de soluções** em **scripts**.

Para ver o conteúdo do arquivo, clique duas vezes em **hubs**. Se você não estiver usando o Visual Studio 2012 ou 2013 e o Internet Explorer, ou se não estiver no modo de depuração, também poderá obter o conteúdo do arquivo navegando até a URL "/signalR/hubs". Por exemplo, se seu site estiver em execução em `http://localhost:56699` , vá para `http://localhost:56699/SignalR/hubs` no navegador.

<a id="manualproxy"></a>

### <a name="how-to-create-a-physical-file-for-the-signalr-generated-proxy"></a>Como criar um arquivo físico para o proxy gerado pelo Signalr

Como alternativa ao proxy gerado dinamicamente, você pode criar um arquivo físico que tem o código de proxy e fazer referência a esse arquivo. Talvez você queira fazer isso para controlar o comportamento de cache ou agrupamento, ou para obter o IntelliSense quando estiver codificando chamadas para métodos de servidor.

Para criar um arquivo de proxy, execute as seguintes etapas:

1. Instale o pacote NuGet [Microsoft. AspNet. signalr. utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) .
2. Abra um prompt de comando e navegue até a pasta *ferramentas* que contém o arquivo SignalR.exe. A pasta ferramentas está no seguinte local:

    `[your solution folder]\packages\Microsoft.AspNet.SignalR.Utils.2.1.0\tools`
3. Digite o seguinte comando:

    `signalr ghp /path:[path to the .dll that contains your Hub class]`

    O caminho para o *. dll* é normalmente a pasta *bin* na pasta do projeto.

    Esse comando cria um arquivo chamado *server.js* na mesma pasta que *signalr.exe*.
4. Coloque o arquivo de *server.js* em uma pasta apropriada em seu projeto, renomeie-o conforme apropriado para seu aplicativo e adicione uma referência a ele no lugar da referência de "signalr/hubs".

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a>Como estabelecer uma conexão

Antes de poder estabelecer uma conexão, você precisa criar um objeto de conexão, criar um proxy e registrar manipuladores de eventos para métodos que podem ser chamados do servidor. Quando o proxy e os manipuladores de eventos são configurados, estabeleça a conexão chamando o `start` método.

Se você estiver usando o proxy gerado, não precisará criar o objeto de conexão em seu próprio código porque o código de proxy gerado faz isso para você.

<a id="nogenconnection"></a>

**Estabelecer uma conexão (com o proxy gerado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample8.js?highlight=5)]

**Estabelecer uma conexão (sem o proxy gerado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample9.js?highlight=1,6)]

O código de exemplo usa a URL "/signalr" padrão para se conectar ao seu serviço Signalr. Para obter informações sobre como especificar uma URL base diferente, consulte [guia da API de hubs do ASP.net signalr-Server-a URL do/signalr](hubs-api-guide-server.md#signalrurl).

Por padrão, o local do hub é o servidor atual; Se você estiver se conectando a um servidor diferente, especifique a URL antes de chamar o `start` método, conforme mostrado no exemplo a seguir:

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample10.js)]

> [!NOTE]
> Normalmente, você registra manipuladores de eventos antes de chamar o `start` método para estabelecer a conexão. Se desejar registrar alguns manipuladores de eventos depois de estabelecer a conexão, você poderá fazer isso, mas deverá registrar pelo menos um dos manipuladores de eventos antes de chamar o `start` método. Um dos motivos para isso é que pode haver muitos hubs em um aplicativo, mas você não desejaria disparar o `OnConnected` evento em cada Hub se quiser usar apenas um deles. Quando a conexão é estabelecida, a presença de um método de cliente no proxy de um hub é o que informa ao Signalr para disparar o `OnConnected` evento. Se você não registrar nenhum manipulador de eventos antes de chamar o `start` método, poderá invocar métodos no Hub, mas o método do Hub `OnConnected` não será chamado e nenhum método de cliente será invocado do servidor.

<a id="connequivalence"></a>

### <a name="connectionhub-is-the-same-object-that-hubconnection-creates"></a>$. Connection. Hub é o mesmo objeto que $. hubConnection () cria

Como você pode ver nos exemplos, quando você usa o proxy gerado, `$.connection.hub` refere-se ao objeto de conexão. Esse é o mesmo objeto que você obtém chamando `$.hubConnection()` quando você não está usando o proxy gerado. O código de proxy gerado cria a conexão para você executando a seguinte instrução:

![Criando uma conexão no arquivo de proxy gerado](hubs-api-guide-javascript-client/_static/image3.png)

Quando você estiver usando o proxy gerado, poderá fazer qualquer coisa com a `$.connection.hub` que você possa fazer com um objeto de conexão quando não estiver usando o proxy gerado.

<a id="asyncstart"></a>

### <a name="asynchronous-execution-of-the-start-method"></a>Execução assíncrona do método Start

O `start` método é executado de forma assíncrona. Ele retorna um [objeto jQuery adiado](http://api.jquery.com/category/deferred-object/), o que significa que você pode adicionar funções de retorno de chamada chamando métodos como `pipe` , `done` e `fail` . Se você tiver um código que deseja executar depois que a conexão for estabelecida, como uma chamada para um método de servidor, coloque esse código em uma função de retorno de chamada ou chame-o de uma função de retorno de chamada. O `.done` método de retorno de chamada é executado depois que a conexão é estabelecida e, depois que qualquer código que você tiver em seu `OnConnected` método de manipulador de eventos no servidor terminar a execução.

Se você colocar a instrução "agora conectado" do exemplo anterior como a próxima linha de código após a `start` chamada do método (não em um `.done` retorno de chamada), a `console.log` linha será executada antes que a conexão seja estabelecida, conforme mostrado no exemplo a seguir:

![Maneira incorreta de escrever código que é executado depois que a conexão é estabelecida](hubs-api-guide-javascript-client/_static/image5.png)

<a id="crossdomain"></a>

## <a name="how-to-establish-a-cross-domain-connection"></a>Como estabelecer uma conexão entre domínios

Normalmente, se o navegador carregar uma página do `http://contoso.com` , a conexão do signalr estará no mesmo domínio, em `http://contoso.com/signalr` . Se a página de `http://contoso.com` estabelecer uma conexão com o `http://fabrikam.com/signalr` , que é uma conexão entre domínios. Por motivos de segurança, as conexões entre domínios são desabilitadas por padrão.

No Signalr 1. x, as solicitações entre domínios foram controladas por um único sinalizador EnableCrossDomain. Esse sinalizador controlava as solicitações JSONP e CORS. Para maior flexibilidade, todo o suporte a CORS foi removido do componente de servidor do Signalr (os clientes JavaScript ainda usam CORS normalmente se detectar que o navegador dá suporte a ele) e o novo middleware OWIN foi disponibilizado para dar suporte a esses cenários.

Se JSONP for necessário no cliente (para dar suporte a solicitações entre domínios em navegadores mais antigos), será necessário habilitá-lo explicitamente definindo `EnableJSONP` no `HubConfiguration` objeto como `true` , conforme mostrado abaixo. O JSONP é desabilitado por padrão, pois é menos seguro do que o CORS.

**Adicionando Microsoft. Owin. CORS ao seu projeto:** Para instalar essa biblioteca, execute o seguinte comando no console do Gerenciador de pacotes:

`Install-Package Microsoft.Owin.Cors`

Este comando adicionará a versão 2.1.0 do pacote ao seu projeto.

### <a name="calling-usecors"></a>Chamando UseCors

 O trecho de código a seguir demonstra como implementar conexões entre domínios no Signalr 2.

**Implementando solicitações entre domínios no Signalr 2**

O código a seguir demonstra como habilitar CORS ou JSONP em um projeto do Signalr 2. Esse exemplo de código usa `Map` e `RunSignalR` em vez de `MapSignalR` , para que o middleware do CORS seja executado somente para solicitações do signalr que exigem suporte a CORS (em vez de para todo o tráfego no caminho especificado em `MapSignalR` .) O mapa também pode ser usado para qualquer outro middleware que precise ser executado para um prefixo de URL específico, em vez de para o aplicativo inteiro.

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample11.cs)]

> [!NOTE]
>
> - Não defina `jQuery.support.cors` como true em seu código.
>
>     ![Não defina jQuery. support. CORS como true](hubs-api-guide-javascript-client/_static/image7.png)
>
>     O signalr lida com o uso do CORS. Definir `jQuery.support.cors` como true DESABILITA JSONP porque faz com que o signalr assuma que o navegador dá suporte a CORS.
> - Quando você estiver se conectando a uma URL de localhost, o Internet Explorer 10 não considerará uma conexão entre domínios, de modo que o aplicativo funcionará localmente com o IE 10 mesmo que você não tenha habilitado conexões entre domínios no servidor.
> - Para obter informações sobre como usar conexões entre domínios com o Internet Explorer 9, consulte [este thread StackOverflow](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).
> - Para obter informações sobre como usar conexões entre domínios com o Chrome, consulte [este thread StackOverflow](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).
> - O código de exemplo usa a URL "/signalr" padrão para se conectar ao seu serviço Signalr. Para obter informações sobre como especificar uma URL base diferente, consulte [guia da API de hubs do ASP.net signalr-Server-a URL do/signalr](hubs-api-guide-server.md#signalrurl).

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a>Como configurar a conexão

Antes de estabelecer uma conexão, você pode especificar parâmetros de cadeia de caracteres de consulta ou especificar o método de transporte.

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a>Como especificar parâmetros de cadeia de caracteres de consulta

Se você quiser enviar dados ao servidor quando o cliente se conectar, poderá adicionar parâmetros de cadeia de caracteres de consulta ao objeto de conexão. Os exemplos a seguir mostram como definir um parâmetro de cadeia de caracteres de consulta no código do cliente.

**Definir um valor de cadeia de caracteres de consulta antes de chamar o método Start (com o proxy gerado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample12.js?highlight=1)]

**Definir um valor de cadeia de caracteres de consulta antes de chamar o método Start (sem o proxy gerado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample13.js?highlight=2)]

O exemplo a seguir mostra como ler um parâmetro de cadeia de caracteres de consulta no código do servidor.

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample14.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a>Como especificar o método de transporte

Como parte do processo de conexão, um cliente do Signalr normalmente negocia com o servidor para determinar o melhor transporte que é suportado pelo servidor e pelo cliente. Se você já souber qual transporte deseja usar, poderá ignorar esse processo de negociação especificando o método de transporte ao chamar o `start` método.

**O código do cliente que especifica o método de transporte (com o proxy gerado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample15.js?highlight=1)]

**O código do cliente que especifica o método de transporte (sem o proxy gerado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample16.js?highlight=2)]

Como alternativa, você pode especificar vários métodos de transporte na ordem em que deseja que o Signalr os experimente:

**O código do cliente que especifica um esquema de fallback de transporte personalizado (com o proxy gerado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample17.js?highlight=1)]

**O código do cliente que especifica um esquema de fallback de transporte personalizado (sem o proxy gerado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample18.js?highlight=2)]

Você pode usar os seguintes valores para especificar o método de transporte:

- WebSockets
- "foreverFrame"
- "serverSentEvents"
- "longPolling"

Os exemplos a seguir mostram como descobrir qual método de transporte está sendo usado por uma conexão.

**O código do cliente que exibe o método de transporte usado por uma conexão (com o proxy gerado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample19.js?highlight=2)]

**O código do cliente que exibe o método de transporte usado por uma conexão (sem o proxy gerado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample20.js?highlight=3)]

Para obter informações sobre como verificar o método de transporte no código do servidor, consulte [guia da API de hubs do signaler ASP.NET-Server-como obter informações sobre o cliente a partir da propriedade Context](hubs-api-guide-server.md#contextproperty). Para obter mais informações sobre transportes e fallbacks, consulte [introdução ao signalr-transportes e fallbacks](../getting-started/introduction-to-signalr.md#transports).

<a id="getproxy"></a>

## <a name="how-to-get-a-proxy-for-a-hub-class"></a>Como obter um proxy para uma classe de Hub

Cada objeto de conexão que você cria encapsula informações sobre uma conexão com um serviço de sinalização que contém uma ou mais classes de Hub. Para se comunicar com uma classe de Hub, você usa um objeto proxy que você mesmo cria (se você não estiver usando o proxy gerado) ou que é gerado para você.

No cliente, o nome do proxy é uma versão do camel case do nome da classe do Hub. O signalr faz essa alteração automaticamente para que o código JavaScript possa estar em conformidade com as convenções de JavaScript.

**Classe de Hub no servidor**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample21.cs?highlight=1)]

**Obter uma referência para o proxy de cliente gerado para o Hub**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample22.js?highlight=1)]

**Criar proxy de cliente para a classe de Hub (sem proxy gerado)**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample23.cs?highlight=1)]

Se você decorar a classe de Hub com um `HubName` atributo, use o nome exato sem alterar o caso.

**Classe de Hub no servidor com o atributo HubName**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample24.cs?highlight=1)]

**Obter uma referência para o proxy de cliente gerado para o Hub**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample25.js?highlight=1)]

**Criar proxy de cliente para a classe de Hub (sem proxy gerado)**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample26.cs?highlight=1)]

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a>Como definir métodos no cliente que o servidor pode chamar

Para definir um método que o servidor pode chamar de um Hub, adicione um manipulador de eventos ao proxy de Hub usando a `client` Propriedade do proxy gerado ou chame o `on` método se você não estiver usando o proxy gerado. Os parâmetros podem ser objetos complexos.

Adicione o manipulador de eventos antes de chamar o `start` método para estabelecer a conexão. (Se você quiser adicionar manipuladores de eventos depois de chamar o `start` método, consulte a observação em [como estabelecer uma conexão](#establishconnection) anteriormente neste documento e use a sintaxe mostrada para definir um método sem usar o proxy gerado.)

A correspondência de nome de método não diferencia maiúsculas de minúsculas. Por exemplo, `Clients.All.addContosoChatMessageToPage` no servidor será executado `AddContosoChatMessageToPage` , `addContosoChatMessageToPage` ou `addcontosochatmessagetopage` no cliente.

**Definir o método no cliente (com o proxy gerado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample27.js?highlight=2)]

**Maneira alternativa de definir o método no cliente (com o proxy gerado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample28.js?highlight=1-2)]

**Definir o método no cliente (sem o proxy gerado ou ao adicionar depois de chamar o método Start)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample29.js?highlight=3)]

**Código de servidor que chama o método de cliente**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample30.cs?highlight=5)]

Os exemplos a seguir incluem um objeto complexo como um parâmetro de método.

**Definir o método no cliente que usa um objeto complexo (com o proxy gerado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample31.js?highlight=2-3)]

**Definir o método no cliente que usa um objeto complexo (sem o proxy gerado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample32.js?highlight=3-4)]

**Código de servidor que define o objeto complexo**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample33.cs?highlight=1)]

**Código de servidor que chama o método de cliente usando um objeto complexo**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample34.cs?highlight=3)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a>Como chamar métodos de servidor do cliente

Para chamar um método de servidor do cliente, use a `server` Propriedade do proxy gerado ou o `invoke` método no proxy do hub se você não estiver usando o proxy gerado. O valor ou os parâmetros de retorno podem ser objetos complexos.

Passe uma versão do camel case do nome do método no Hub. O signalr faz essa alteração automaticamente para que o código JavaScript possa estar em conformidade com as convenções de JavaScript.

Os exemplos a seguir mostram como chamar um método de servidor que não tem um valor de retorno e como chamar um método de servidor que tem um valor de retorno.

**Método de servidor sem atributo HubMethodName**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample35.cs?highlight=3)]

**Código de servidor que define o objeto complexo passado em um parâmetro**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample36.cs)]

**Código de cliente que invoca o método de servidor (com o proxy gerado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample37.js?highlight=1)]

**Código de cliente que invoca o método de servidor (sem o proxy gerado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample38.js?highlight=1)]

Se você tiver decorado o método de Hub com um `HubMethodName` atributo, use esse nome sem alterar o caso.

**Método de servidor** com um atributo HubMethodName

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample39.cs?highlight=3)]

**Código de cliente que invoca o método de servidor (com o proxy gerado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample40.js?highlight=1)]

**Código de cliente que invoca o método de servidor (sem o proxy gerado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample41.js?highlight=1)]

Os exemplos anteriores mostram como chamar um método de servidor que não tem nenhum valor de retorno. Os exemplos a seguir mostram como chamar um método de servidor que tem um valor de retorno.

**Código do servidor para um método que tem um valor de retorno**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample42.cs?highlight=3)]

**A classe Stock usada para o valor de** retorno

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample43.cs?highlight=1)]

**Código de cliente que invoca o método de servidor (com o proxy gerado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample44.js?highlight=2,4-5)]

**Código de cliente que invoca o método de servidor (sem o proxy gerado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample45.js?highlight=2,4-5)]

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a>Como tratar eventos de tempo de vida da conexão

O signalr fornece os seguintes eventos de tempo de vida de conexão que você pode manipular:

- `starting`: Gerado antes de qualquer dado ser enviado pela conexão.
- `received`: Ocorre quando os dados são recebidos na conexão. Fornece os dados recebidos.
- `connectionSlow`: Ocorre quando o cliente detecta uma conexão lenta ou com frequência.
- `reconnecting`: Ocorre quando o transporte subjacente começa a se reconectar.
- `reconnected`: Ocorre quando o transporte subjacente é reconectado.
- `stateChanged`: Ocorre quando o estado da conexão é alterado. Fornece o estado antigo e o novo estado (conectando, conectado, reconectando ou desconectado).
- `disconnected`: Ocorre quando a conexão é desconectada.

Por exemplo, se você quiser exibir mensagens de aviso quando houver problemas de conexão que possam causar atrasos perceptíveis, manipule o `connectionSlow` evento.

**Manipular o evento connectionSlow (com o proxy gerado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample46.js?highlight=1)]

**Manipular o evento connectionSlow (sem o proxy gerado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample47.js?highlight=2)]

Para obter mais informações, consulte [noções básicas e manipulação de eventos de tempo de vida de conexão no signalr](handling-connection-lifetime-events.md).

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a>Como tratar erros

O cliente do Signalr JavaScript fornece um `error` evento para o qual você pode adicionar um manipulador. Você também pode usar o método Fail para adicionar um manipulador de erros que resultam de uma invocação de método de servidor.

Se você não habilitar explicitamente mensagens de erro detalhadas no servidor, o objeto de exceção que o Signalr retorna após um erro contém informações mínimas sobre o erro. Por exemplo, se uma chamada para `newContosoChatMessage` falhar, a mensagem de erro no objeto de erro conterá " `There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.` " não é recomendável enviar mensagens de erro detalhadas aos clientes em produção por motivos de segurança, mas se você quiser habilitar mensagens de erro detalhadas para fins de solução de problemas, use o código a seguir no servidor.

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample48.cs?highlight=2)]

O exemplo a seguir mostra como adicionar um manipulador para o evento de erro.

**Adicionar um manipulador de erro (com o proxy gerado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample49.js?highlight=1)]

**Adicionar um manipulador de erro (sem o proxy gerado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample50.js?highlight=2)]

O exemplo a seguir mostra como tratar um erro de uma invocação de método.

**Manipular um erro de uma invocação de método (com o proxy gerado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample51.js?highlight=2)]

**Manipular um erro de uma invocação de método (sem o proxy gerado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample52.js?highlight=2)]

Se uma invocação de método falhar, o `error` evento também será gerado, de modo que seu código no `error` manipulador de métodos e no `.fail` retorno de chamada do método seria executado.

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a>Como habilitar o log do lado do cliente

Para habilitar o log do lado do cliente em uma conexão, defina a `logging` propriedade no objeto de conexão antes de chamar o `start` método para estabelecer a conexão.

**Habilitar registro em log (com o proxy gerado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample53.js?highlight=1)]

**Habilitar o registro em log (sem o proxy gerado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample54.js?highlight=2)]

Para ver os logs, abra as ferramentas de desenvolvedor do navegador e vá para a guia Console. Para obter um tutorial que mostra instruções passo a passo e capturas de tela que mostram como fazer isso, consulte [transmissão de servidor com o ASP.net signalr-habilitar registro em log](../getting-started/tutorial-server-broadcast-with-signalr.md#enable-logging).
