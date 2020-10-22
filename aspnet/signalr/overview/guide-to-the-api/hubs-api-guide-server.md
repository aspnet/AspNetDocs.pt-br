---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-server
title: Guia da API de hubs do Signalr ASP.NET – servidor (C#) | Microsoft Docs
author: bradygaster
description: Este documento fornece uma introdução à programação do lado do servidor da API de hubs do Signalr ASP.NET para o Signalr versão 2, com exemplos de código demonstrando...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: b19913e5-cd8a-4e4b-a872-5ac7a858a934
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-server
msc.type: authoredcontent
ms.openlocfilehash: c681b104b15bfc4a04587c7abf685dcf20def2ca
ms.sourcegitcommit: c62ec20b453cee3249eb894ecd75013b57d078f0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/21/2020
ms.locfileid: "92345377"
---
# <a name="aspnet-signalr-hubs-api-guide---server-c"></a>Guia da API de hubs do Signalr ASP.NET – servidor (C#)

por [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Este documento fornece uma introdução à programação do lado do servidor da API de hubs do Signalr ASP.NET para o Signalr versão 2, com exemplos de código demonstrando opções comuns.
> 
> A API de hubs de Signalr permite que você faça RPCs (chamadas de procedimento remoto) de um servidor para clientes conectados e de clientes para o servidor. No código do servidor, você define métodos que podem ser chamados por clientes e chama métodos que são executados no cliente. No código do cliente, você define métodos que podem ser chamados do servidor e chama métodos que são executados no servidor. O signalr cuida de todo o direcionamento de cliente para servidor para você.
> 
> O signalr também oferece uma API de nível inferior chamada conexões persistentes. Para obter uma introdução ao Signalr, hubs e conexões persistentes, consulte [introdução ao signalr 2](../getting-started/introduction-to-signalr.md).
> 
> ## <a name="software-versions-used-in-this-topic"></a>Versões de software usadas neste tópico
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - Sinalização versão 2
>   
> 
> 
> ## <a name="topic-versions"></a>Versões de tópico
> 
> Para obter informações sobre versões anteriores do Signalr, confira [versões mais antigas do signalr](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Perguntas e comentários
> 
> Deixe comentários sobre como você gostou deste tutorial e o que poderíamos melhorar nos comentários na parte inferior da página. Se você tiver dúvidas que não estão diretamente relacionadas ao tutorial, poderá lançá-las no fórum do [signalr ASP.net](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [stackoverflow.com](http://stackoverflow.com/).

## <a name="overview"></a>Visão geral

Este documento contém as seguintes seções:

- [Como registrar o middleware Signalr](#route)

    - [A URL do/signalr](#signalrurl)
    - [Configurando opções do Signalr](#options)
- [Como criar e usar classes de Hub](#hubclass)

    - [Tempo de vida do objeto Hub](#transience)
    - [Camel-compartimento de nomes de Hub em clientes JavaScript](#hubnames)
    - [Vários hubs](#multiplehubs)
    - [Hubs fortemente tipados](#stronglytypedhubs)
- [Como definir métodos na classe Hub que os clientes podem chamar](#hubmethods)

    - [Camel-capitalização de nomes de método em clientes JavaScript](#methodnames)
    - [Quando executar de forma assíncrona](#asyncmethods)
    - [Definindo sobrecargas](#overloads)
    - [Relatando o progresso das invocações do método de Hub](#progress)
- [Como chamar métodos de cliente da classe Hub](#callfromhub)

    - [Selecionando quais clientes receberão o RPC](#selectingclients)
    - [Nenhuma validação de tempo de compilação para nomes de método](#dynamicmethodnames)
    - [Correspondência de nome de método que não diferencia maiúsculas de minúsculas](#caseinsensitive)
    - [Execução assíncrona](#asyncclient)
- [Como gerenciar a associação de grupo da classe Hub](#groupsfromhub)

    - [Execução assíncrona de métodos Add e remove](#asyncgroupmethods)
    - [Persistência de associação de grupo](#grouppersistence)
    - [Grupos de usuário único](#singleusergroups)
- [Como tratar eventos de tempo de vida da conexão na classe Hub](#connectionlifetime)

    - [Quando onconnected, OnDisconnect e OnReconnected são chamados](#onreconnected)
    - [Estado do chamador não preenchido](#nocallerstate)
- [Como obter informações sobre o cliente a partir da propriedade Context](#contextproperty)
- [Como passar o estado entre clientes e a classe Hub](#passstate)
- [Como tratar erros na classe Hub](#handleErrors)
- [Como chamar métodos de cliente e gerenciar grupos de fora da classe de Hub](#callfromoutsidehub)

    - [Chamando métodos de cliente](#callingclientsoutsidehub)
    - [Gerenciando a associação de grupo](#managinggroupsoutsidehub)
- [Como habilitar o rastreamento](#tracing)
- [Como personalizar o pipeline de hubs](#hubpipeline)

Para obter a documentação sobre como programar clientes do, consulte os seguintes recursos:

- [Guia de API de hubs de signalr-cliente JavaScript](hubs-api-guide-javascript-client.md)
- [Guia da API de hubs do signalr-cliente .NET](hubs-api-guide-net-client.md)

Os componentes do servidor para o Signalr 2 estão disponíveis somente no .NET 4,5. Os servidores que executam o .NET 4,0 devem usar o Signalr v1. x.

<a id="route"></a>

## <a name="how-to-register-signalr-middleware"></a>Como registrar o middleware Signalr

Para definir a rota que os clientes usarão para se conectar ao seu hub, chame o `MapSignalR` método quando o aplicativo for iniciado. `MapSignalR` é um [método de extensão](https://msdn.microsoft.com/library/vstudio/bb383977.aspx) para a `OwinExtensions` classe. O exemplo a seguir mostra como definir a rota de hubs do Signalr usando uma classe de inicialização OWIN.

[!code-csharp[Main](hubs-api-guide-server/samples/sample1.cs)]

Se você estiver adicionando a funcionalidade do Signalr a um aplicativo MVC ASP.NET, certifique-se de que a rota do Signalr seja adicionada antes das outras rotas. Para obter mais informações, consulte [tutorial: introdução com signalr 2 e MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).

<a id="signalrurl"></a>

### <a name="the-signalr-url"></a>A URL do/signalr

Por padrão, a URL de rota que os clientes usarão para se conectar ao seu hub é "/signalr". (Não confunda essa URL com a URL "/signalr/hubs", que é para o arquivo JavaScript gerado automaticamente. Para obter mais informações sobre o proxy gerado, consulte [Guia de API de hubs de signalr-cliente JavaScript-o proxy gerado e o que ele faz por você](hubs-api-guide-javascript-client.md#genproxy).)

Pode haver circunstâncias extraordinárias que tornam essa URL base inutilizável para o Signalr; por exemplo, você tem uma pasta em seu projeto denominada *signalr* e não deseja alterar o nome. Nesse caso, você pode alterar a URL base, conforme mostrado nos exemplos a seguir (substitua "/signalr" no código de exemplo pela URL desejada).

**Código do servidor que especifica a URL**

[!code-csharp[Main](hubs-api-guide-server/samples/sample2.cs?highlight=1)]

**Código de cliente JavaScript que especifica a URL (com o proxy gerado)**

[!code-javascript[Main](hubs-api-guide-server/samples/sample3.js?highlight=1)]

**Código de cliente JavaScript que especifica a URL (sem o proxy gerado)**

[!code-javascript[Main](hubs-api-guide-server/samples/sample4.js?highlight=1)]

**Código de cliente .NET que especifica a URL**

[!code-csharp[Main](hubs-api-guide-server/samples/sample5.cs?highlight=1)]

<a id="options"></a>

### <a name="configuring-signalr-options"></a>Configurando opções do Signalr

Sobrecargas do `MapSignalR` método permitem que você especifique uma URL personalizada, um resolvedor de dependência personalizado e as seguintes opções:

- Habilite chamadas entre domínios usando CORS ou JSONP de clientes de navegador.

    Normalmente, se o navegador carregar uma página do `http://contoso.com` , a conexão do signalr estará no mesmo domínio, em `http://contoso.com/signalr` . Se a página de `http://contoso.com` estabelecer uma conexão com o `http://fabrikam.com/signalr` , que é uma conexão entre domínios. Por motivos de segurança, as conexões entre domínios são desabilitadas por padrão. Para obter mais informações, consulte [Guia de API de hubs do ASP.net signalr-cliente JavaScript-como estabelecer uma conexão entre domínios](hubs-api-guide-javascript-client.md#crossdomain).
- Habilitar mensagens de erro detalhadas.

    Quando ocorrem erros, o comportamento padrão do Signalr é enviar para os clientes uma mensagem de notificação sem detalhes sobre o que aconteceu. Não é recomendável enviar informações de erro detalhadas para clientes em produção, pois usuários mal-intencionados podem usar as informações em ataques contra seu aplicativo. Para solucionar problemas, você pode usar essa opção para habilitar temporariamente relatórios de erros mais informativos.
- Desabilitar arquivos de proxy JavaScript gerados automaticamente.

    Por padrão, um arquivo JavaScript com proxies para suas classes de Hub é gerado em resposta à URL "/signalr/hubs". Se você não quiser usar os proxies JavaScript ou se quiser gerar esse arquivo manualmente e se referir a um arquivo físico em seus clientes, poderá usar essa opção para desabilitar a geração de proxy. Para obter mais informações, consulte [Guia de API de hubs de signalr-cliente JavaScript-como criar um arquivo físico para o proxy gerado pelo signalr](hubs-api-guide-javascript-client.md#manualproxy).

O exemplo a seguir mostra como especificar a URL de conexão do Signalr e essas opções em uma chamada para o `MapSignalR` método. Para especificar uma URL personalizada, substitua "/signalr" no exemplo pela URL que você deseja usar.

[!code-csharp[Main](hubs-api-guide-server/samples/sample6.cs)]

<a id="hubclass"></a>

## <a name="how-to-create-and-use-hub-classes"></a>Como criar e usar classes de Hub

Para criar um Hub, crie uma classe que derive de [Microsoft. AspNet. signalr. Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx). O exemplo a seguir mostra uma classe de Hub simples para um aplicativo de chat.

[!code-csharp[Main](hubs-api-guide-server/samples/sample7.cs)]

Neste exemplo, um cliente conectado pode chamar o `NewContosoChatMessage` método e, quando ele faz, os dados recebidos são transmitidos para todos os clientes conectados.

<a id="transience"></a>

### <a name="hub-object-lifetime"></a>Tempo de vida do objeto Hub

Você não instancia a classe Hub nem chama seus métodos de seu próprio código no servidor; tudo isso é feito para você pelo pipeline de hubs do Signalr. O signalr cria uma nova instância da classe Hub cada vez que precisa manipular uma operação de Hub, como quando um cliente se conecta, desconecta ou faz uma chamada de método para o servidor.

Como as instâncias da classe Hub são transitórias, você não pode usá-las para manter o estado de uma chamada de método para a próxima. Cada vez que o servidor recebe uma chamada de método de um cliente, uma nova instância da sua classe de Hub processa a mensagem. Para manter o estado por meio de várias conexões e chamadas de método, use outro método, como um banco de dados, ou uma variável estática na classe Hub, ou uma classe diferente que não derive de `Hub` . Se você persistir dados na memória, usando um método como uma variável estática na classe Hub, os dados serão perdidos quando o domínio do aplicativo for reciclado.

Se você quiser enviar mensagens para clientes de seu próprio código que é executado fora da classe Hub, não é possível fazer isso instanciando uma instância de classe de Hub, mas você pode fazer isso obtendo uma referência ao objeto de contexto Signalr para a classe Hub. Para obter mais informações, consulte [como chamar métodos de cliente e gerenciar grupos de fora da classe de Hub](#callfromoutsidehub) mais adiante neste tópico.

<a id="hubnames"></a>

### <a name="camel-casing-of-hub-names-in-javascript-clients"></a>Camel-compartimento de nomes de Hub em clientes JavaScript

Por padrão, os clientes JavaScript se referem a hubs usando uma versão do nome de classe do camel case. O signalr faz essa alteração automaticamente para que o código JavaScript possa estar em conformidade com as convenções de JavaScript. O exemplo anterior seria conhecido como `contosoChatHub` no código JavaScript.

**Servidor**

[!code-csharp[Main](hubs-api-guide-server/samples/sample8.cs?highlight=1)]

**Cliente JavaScript usando proxy gerado**

[!code-javascript[Main](hubs-api-guide-server/samples/sample9.js?highlight=1)]

Se você quiser especificar um nome diferente para os clientes usarem, adicione o `HubName` atributo. Quando você usa um `HubName` atributo, não há nenhuma alteração de nome para o camel case em clientes JavaScript.

**Servidor**

[!code-csharp[Main](hubs-api-guide-server/samples/sample10.cs?highlight=1)]

**Cliente JavaScript usando proxy gerado**

[!code-javascript[Main](hubs-api-guide-server/samples/sample11.js?highlight=1)]

<a id="multiplehubs"></a>

### <a name="multiple-hubs"></a>Vários hubs

Você pode definir várias classes de Hub em um aplicativo. Quando você faz isso, a conexão é compartilhada, mas os grupos são separados:

- Todos os clientes usarão a mesma URL para estabelecer uma conexão de Signalr com seu serviço ("/signalr" ou sua URL personalizada se você tiver especificado um) e essa conexão será usada para todos os hubs definidos pelo serviço.

    Não há nenhuma diferença de desempenho para vários hubs em comparação com a definição de toda a funcionalidade de Hub em uma única classe.
- Todos os hubs obtêm as mesmas informações de solicitação HTTP.

    Como todos os hubs compartilham a mesma conexão, as únicas informações de solicitação HTTP que o servidor Obtém são o que vem na solicitação HTTP original que estabelece a conexão do Signalr. Se você usar a solicitação de conexão para passar informações do cliente para o servidor, especificando uma cadeia de caracteres de consulta, não poderá fornecer cadeias de consulta diferentes para diferentes hubs. Todos os hubs receberão as mesmas informações.
- O arquivo de proxies JavaScript gerado conterá proxies para todos os hubs em um arquivo.

    Para obter informações sobre proxies JavaScript, consulte [Guia de API de hubs de signalr-cliente JavaScript-o proxy gerado e o que ele faz para você](hubs-api-guide-javascript-client.md#genproxy).
- Os grupos são definidos dentro de hubs.

    No Signalr, você pode definir grupos nomeados para difundir para subconjuntos de clientes conectados. Os grupos são mantidos separadamente para cada Hub. Por exemplo, um grupo chamado "administradores" incluiria um conjunto de clientes para sua `ContosoChatHub` classe e o mesmo nome de grupo se referiria a um conjunto diferente de clientes para sua `StockTickerHub` classe.

<a id="stronglytypedhubs"></a>
### <a name="strongly-typed-hubs"></a>Hubs de Strongly-Typed

Para definir uma interface para seus métodos de Hub que o cliente pode referenciar (e habilitar o IntelliSense em seus métodos de Hub), derive seu hub de `Hub<T>` (introduzido no signalr 2,1) em vez de `Hub` :

[!code-csharp[Main](hubs-api-guide-server/samples/sample12.cs)]

<a id="hubmethods"></a>

## <a name="how-to-define-methods-in-the-hub-class-that-clients-can-call"></a>Como definir métodos na classe Hub que os clientes podem chamar

Para expor um método no Hub que você deseja que possa ser chamado pelo cliente, declare um método público, conforme mostrado nos exemplos a seguir.

[!code-csharp[Main](hubs-api-guide-server/samples/sample13.cs?highlight=3)]

[!code-csharp[Main](hubs-api-guide-server/samples/sample14.cs?highlight=3)]

Você pode especificar um tipo de retorno e parâmetros, incluindo tipos complexos e matrizes, como faria em qualquer método C#. Todos os dados que você recebe em parâmetros ou retornam ao chamador são comunicados entre o cliente e o servidor usando JSON, e o Signalr manipula a associação de objetos complexos e matrizes de objetos automaticamente.

<a id="methodnames"></a>

### <a name="camel-casing-of-method-names-in-javascript-clients"></a>Camel-capitalização de nomes de método em clientes JavaScript

Por padrão, os clientes JavaScript referem-se a métodos de Hub usando uma versão do nome do método com camel case. O signalr faz essa alteração automaticamente para que o código JavaScript possa estar em conformidade com as convenções de JavaScript.

**Servidor**

[!code-csharp[Main](hubs-api-guide-server/samples/sample15.cs?highlight=1)]

**Cliente JavaScript usando proxy gerado**

[!code-javascript[Main](hubs-api-guide-server/samples/sample16.js?highlight=1)]

Se você quiser especificar um nome diferente para os clientes usarem, adicione o `HubMethodName` atributo.

**Servidor**

[!code-csharp[Main](hubs-api-guide-server/samples/sample17.cs?highlight=1)]

**Cliente JavaScript usando proxy gerado**

[!code-javascript[Main](hubs-api-guide-server/samples/sample18.js?highlight=1)]

<a id="asyncmethods"></a>

### <a name="when-to-execute-asynchronously"></a>Quando executar de forma assíncrona

Se o método for de execução demorada ou precisar de trabalho que envolvesse espera, como uma pesquisa de banco de dados ou uma chamada de serviço Web, torne o método de Hub assíncrono retornando uma [tarefa](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) (no lugar de `void` retorno) ou objeto [ &lt; T &gt; de tarefa](https://msdn.microsoft.com/library/dd321424.aspx) (no lugar do tipo de `T` retorno). Quando você retorna um `Task` objeto do método, o signalr aguarda a `Task` conclusão e, em seguida, envia o resultado desencapsulado para o cliente, portanto, não há diferença na forma como você codifica a chamada do método no cliente.

Tornar um método de Hub assíncrono evita o bloqueio da conexão quando ele usa o transporte WebSocket. Quando um método de Hub é executado de forma síncrona e o transporte é WebSocket, as invocações subsequentes de métodos no Hub do mesmo cliente são bloqueadas até que o método de Hub seja concluído.

O exemplo a seguir mostra o mesmo método codificado para ser executado de forma síncrona ou assíncrona, seguido pelo código de cliente JavaScript que funciona para chamar qualquer versão.

**Síncrono**

[!code-csharp[Main](hubs-api-guide-server/samples/sample19.cs)]

**Assíncronos**

[!code-csharp[Main](hubs-api-guide-server/samples/sample20.cs?highlight=1,7-8)]

**Cliente JavaScript usando proxy gerado**

[!code-javascript[Main](hubs-api-guide-server/samples/sample21.js)]

Para obter mais informações sobre como usar métodos assíncronos no ASP.NET 4,5, consulte [usando métodos assíncronos no ASP.NET MVC 4](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).

<a id="overloads"></a>

### <a name="defining-overloads"></a>Definindo sobrecargas

Se você quiser definir sobrecargas para um método, o número de parâmetros em cada sobrecarga deverá ser diferente. Se você diferenciar uma sobrecarga apenas especificando tipos de parâmetros diferentes, a classe de Hub será compilada, mas o serviço Signalr gerará uma exceção em tempo de execução quando os clientes tentarem chamar uma das sobrecargas.

<a id="progress"></a>
### <a name="reporting-progress-from-hub-method-invocations"></a>Relatando o progresso das invocações do método de Hub

O signalr 2,1 adiciona suporte para o [padrão de relatório de progresso](https://blogs.msdn.com/b/dotnet/archive/2012/06/06/async-in-4-5-enabling-progress-and-cancellation-in-async-apis.aspx) introduzido no .NET 4,5. Para implementar o relatório de progresso, defina um `IProgress<T>` parâmetro para o método de Hub que o cliente pode acessar:

[!code-csharp[Main](hubs-api-guide-server/samples/sample22.cs)]

Ao escrever um método de servidor de longa execução, é importante usar um padrão de programação assíncrona como Async/Await em vez de bloquear o thread de Hub.

<a id="callfromhub"></a>

## <a name="how-to-call-client-methods-from-the-hub-class"></a>Como chamar métodos de cliente da classe Hub

Para chamar métodos de cliente do servidor, use a `Clients` propriedade em um método em sua classe de Hub. O exemplo a seguir mostra o código do servidor que chama `addNewMessageToPage` todos os clientes conectados e o código do cliente que define o método em um cliente JavaScript.

**Servidor**

[!code-csharp[Main](hubs-api-guide-server/samples/sample23.cs?highlight=5)]

Invocar um método de cliente é uma operação assíncrona e retorna um `Task` . Use `await` :

* Para garantir que a mensagem seja enviada sem erros. 
* Para habilitar a captura e manipulação de erros em um bloco try-catch.

**Cliente JavaScript usando proxy gerado**

[!code-html[Main](hubs-api-guide-server/samples/sample24.html?highlight=1)]

Não é possível obter um valor de retorno de um método de cliente; a sintaxe como não `int x = Clients.All.add(1,1)` funciona.

Você pode especificar tipos e matrizes complexos para os parâmetros. O exemplo a seguir passa um tipo complexo para o cliente em um parâmetro de método.

**Código de servidor que chama um método de cliente usando um objeto complexo**

[!code-csharp[Main](hubs-api-guide-server/samples/sample25.cs?highlight=3)]

**Código de servidor que define o objeto complexo**

[!code-csharp[Main](hubs-api-guide-server/samples/sample26.cs?highlight=1)]

**Cliente JavaScript usando proxy gerado**

[!code-javascript[Main](hubs-api-guide-server/samples/sample27.js?highlight=2-3)]

<a id="selectingclients"></a>

### <a name="selecting-which-clients-will-receive-the-rpc"></a>Selecionando quais clientes receberão o RPC

A propriedade clients retorna um objeto [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) que fornece várias opções para especificar quais clientes receberão o RPC:

- Todos os clientes conectados.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample28.cs)]
- Somente o cliente de chamada.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample29.cs)]
- Todos os clientes, exceto o cliente de chamada.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample30.cs)]
- Um cliente específico identificado pela ID de conexão.

    [!code-css[Main](hubs-api-guide-server/samples/sample31.css)]

    Este exemplo chama `addContosoChatMessageToPage` o cliente de chamada e tem o mesmo efeito que usar `Clients.Caller` .
- Todos os clientes conectados, exceto os clientes especificados, identificados pela ID de conexão.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample32.cs)]
- Todos os clientes conectados em um grupo especificado.

    [!code-css[Main](hubs-api-guide-server/samples/sample33.css)]
- Todos os clientes conectados em um grupo especificado, exceto os clientes especificados, identificados pela ID de conexão.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample34.cs)]
- Todos os clientes conectados em um grupo especificado, exceto o cliente de chamada.

    [!code-css[Main](hubs-api-guide-server/samples/sample35.css)]
- Um usuário específico, identificado pelo userId.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample36.cs)]

    Por padrão, isso é `IPrincipal.Identity.Name` , mas isso pode ser alterado [registrando uma implementação de IUserIdProvider com o host global](mapping-users-to-connections.md#IUserIdProvider).
- Todos os clientes e grupos em uma lista de IDs de conexão.

    [!code-css[Main](hubs-api-guide-server/samples/sample37.css)]
- Uma lista de grupos.

    [!code-css[Main](hubs-api-guide-server/samples/sample38.css)]
- Um usuário por nome.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample39.cs)]
- Uma lista de nomes de usuário (introduzida no Signalr 2,1).

    [!code-csharp[Main](hubs-api-guide-server/samples/sample40.cs)]

<a id="dynamicmethodnames"></a>

### <a name="no-compile-time-validation-for-method-names"></a>Nenhuma validação de tempo de compilação para nomes de método

O nome do método que você especifica é interpretado como um objeto dinâmico, o que significa que não há nenhuma validação de tempo de compilação ou IntelliSense para ele. A expressão é avaliada em tempo de execução. Quando a chamada de método é executada, o Signalr envia o nome do método e os valores de parâmetro para o cliente e, se o cliente tiver um método que corresponda ao nome, esse método será chamado e os valores de parâmetro serão passados para ele. Se nenhum método correspondente for encontrado no cliente, nenhum erro será gerado. Para obter informações sobre o formato dos dados que o sinalizador transmite para o cliente em segundo plano quando você chama um método de cliente, consulte [introdução ao signalr](../getting-started/introduction-to-signalr.md).

<a id="caseinsensitive"></a>

### <a name="case-insensitive-method-name-matching"></a>Correspondência de nome de método que não diferencia maiúsculas de minúsculas

A correspondência de nome de método não diferencia maiúsculas de minúsculas. Por exemplo, `Clients.All.addContosoChatMessageToPage` no servidor será executado `AddContosoChatMessageToPage` , `addcontosochatmessagetopage` ou `addContosoChatMessageToPage` no cliente.

<a id="asyncclient"></a>

### <a name="asynchronous-execution"></a>Execução assíncrona

O método que você chama é executado de forma assíncrona. Qualquer código que venha após uma chamada de método para um cliente será executado imediatamente sem esperar que o Signalr termine de transmitir dados para os clientes, a menos que você especifique que as linhas subsequentes de código devem aguardar a conclusão do método. O exemplo de código a seguir mostra como executar dois métodos de cliente sequencialmente.

**Usando Await (.NET 4,5)**

[!code-csharp[Main](hubs-api-guide-server/samples/sample41.cs?highlight=1,3)]

Se você usar `await` o para aguardar até que um método de cliente seja concluído antes da próxima linha de código ser executada, isso não significa que os clientes receberão a mensagem antes que a próxima linha de código seja executada. "Conclusão" de uma chamada de método de cliente significa apenas que o Signalr fez tudo o que é necessário para enviar a mensagem. Se você precisar de verificação de que os clientes receberam a mensagem, precisará programar esse mecanismo por conta própria. Por exemplo, você pode codificar um `MessageReceived` método no Hub e, no `addContosoChatMessageToPage` método no cliente, você poderia chamar depois de `MessageReceived` fazer qualquer trabalho que precisar fazer no cliente. No `MessageReceived` no Hub, você pode fazer qualquer trabalho depende da recepção real do cliente e do processamento da chamada do método original.

### <a name="how-to-use-a-string-variable-as-the-method-name"></a>Como usar uma variável de cadeia de caracteres como o nome do método

Se você quiser invocar um método de cliente usando uma variável de cadeia de caracteres como o nome do método, Cast `Clients.All` (ou `Clients.Others` , `Clients.Caller` etc.) para `IClientProxy` e, em seguida, chame [Invoke (MethodName, args...)](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).

[!code-csharp[Main](hubs-api-guide-server/samples/sample42.cs)]

<a id="groupsfromhub"></a>

## <a name="how-to-manage-group-membership-from-the-hub-class"></a>Como gerenciar a associação de grupo da classe Hub

Os grupos no Signalr fornecem um método para transmitir mensagens para os subconjuntos especificados de clientes conectados. Um grupo pode ter qualquer número de clientes, e um cliente pode ser um membro de qualquer número de grupos.

Para gerenciar a associação de grupo, use os métodos [Add](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) e [Remove](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) fornecidos pela `Groups` propriedade da classe Hub. O exemplo a seguir mostra `Groups.Add` os `Groups.Remove` métodos e usados em métodos de Hub que são chamados pelo código do cliente, seguidos pelo código de cliente JavaScript que os chama.

**Servidor**

[!code-csharp[Main](hubs-api-guide-server/samples/sample43.cs?highlight=5,10)]

**Cliente JavaScript usando proxy gerado**

[!code-javascript[Main](hubs-api-guide-server/samples/sample44.js)]

[!code-javascript[Main](hubs-api-guide-server/samples/sample45.js)]

Você não precisa criar grupos explicitamente. Na verdade, um grupo é criado automaticamente na primeira vez que você especifica seu nome em uma chamada para `Groups.Add` e é excluído quando você remove a última conexão da associação.

Não há API para obter uma lista de associação de grupo ou uma lista de grupos. O signalr envia mensagens para clientes e grupos com base em um [modelo pub/sub](http://en.wikipedia.org/wiki/Publish/subscribe)e o servidor não mantém listas de grupos ou associações de grupo. Isso ajuda a maximizar a escalabilidade, porque sempre que você adiciona um nó a um web farm, qualquer Estado que o Signalr mantém deve ser propagado para o novo nó.

<a id="asyncgroupmethods"></a>

### <a name="asynchronous-execution-of-add-and-remove-methods"></a>Execução assíncrona de métodos Add e remove

Os `Groups.Add` métodos e são `Groups.Remove` executados de forma assíncrona. Se desejar adicionar um cliente a um grupo e enviar imediatamente uma mensagem para o cliente usando o grupo, você precisará certificar-se de que o `Groups.Add` método é concluído primeiro. O exemplo de código a seguir mostra como fazer isso.

**Adicionando um cliente a um grupo e, em seguida, mensagens que o cliente**

[!code-csharp[Main](hubs-api-guide-server/samples/sample46.cs?highlight=1,3)]

<a id="grouppersistence"></a>

### <a name="group-membership-persistence"></a>Persistência de associação de grupo

O signalr rastreia conexões, e não os usuários, portanto, se você quiser que um usuário esteja no mesmo grupo sempre que o usuário estabelecer uma conexão, você precisará chamar `Groups.Add` sempre que o usuário estabelecer uma nova conexão.

Após uma perda temporária de conectividade, às vezes o Signalr pode restaurar a conexão automaticamente. Nesse caso, o Signalr está restaurando a mesma conexão, não estabelecendo uma nova conexão e, portanto, a associação de grupo do cliente é restaurada automaticamente. Isso é possível mesmo quando a interrupção temporária é o resultado de uma reinicialização ou falha do servidor, pois o estado da conexão para cada cliente, incluindo associações de grupo, é feito de forma redonda para o cliente. Se um servidor ficar inativo e for substituído por um novo servidor antes que a conexão expire, um cliente poderá se reconectar automaticamente ao novo servidor e inscrever-se novamente em grupos dos quais ele é membro.

Quando uma conexão não pode ser restaurada automaticamente após uma perda de conectividade, ou quando a conexão atinge o tempo limite ou quando o cliente se desconecta (por exemplo, quando um navegador navega para uma nova página), as associações de grupo são perdidas. Na próxima vez que o usuário se conectar, será uma nova conexão. Para manter as associações de grupo quando o mesmo usuário estabelece uma nova conexão, seu aplicativo precisa rastrear as associações entre usuários e grupos e restaurar as associações de grupo cada vez que um usuário estabelece uma nova conexão.

Para obter mais informações sobre conexões e reconexões, consulte [como lidar com eventos de tempo de vida de conexão na classe Hub](#connectionlifetime) mais adiante neste tópico.

<a id="singleusergroups"></a>

### <a name="single-user-groups"></a>Grupos de usuário único

Os aplicativos que usam o Signalr normalmente precisam manter o controle das associações entre usuários e conexões para saber qual usuário enviou uma mensagem e quais usuários devem receber uma mensagem. Os grupos são usados em um dos dois padrões comumente usados para fazer isso.

- Grupos de usuário único.

    Você pode especificar o nome de usuário como o nome do grupo e adicionar a ID de conexão atual ao grupo toda vez que o usuário se conectar ou reconectar. Para enviar mensagens ao usuário que você envia para o grupo. Uma desvantagem desse método é que o grupo não fornece uma maneira de descobrir se o usuário está online ou offline.
- Rastreie associações entre nomes de usuário e IDs de conexão.

    Você pode armazenar uma associação entre cada nome de usuário e uma ou mais identificações de conexão em um dicionário ou banco de dados, e atualizar as informações armazenadas cada vez que o usuário se conectar ou se desconectar. Para enviar mensagens ao usuário, você especifica as IDs de conexão. Uma desvantagem desse método é que ela ocupa mais memória.

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events-in-the-hub-class"></a>Como tratar eventos de tempo de vida da conexão na classe Hub

Os motivos típicos para lidar com eventos de tempo de vida de conexão são controlar se um usuário está conectado ou não, e controlar a associação entre nomes de usuário e IDs de conexão. Para executar seu próprio código quando os clientes se conectam ou desconectam, substitua os `OnConnected` `OnDisconnected` métodos virtuais,, e `OnReconnected` da classe Hub, conforme mostrado no exemplo a seguir.

[!code-csharp[Main](hubs-api-guide-server/samples/sample47.cs?highlight=3,14,22)]

<a id="onreconnected"></a>

### <a name="when-onconnected-ondisconnected-and-onreconnected-are-called"></a>Quando onconnected, OnDisconnect e OnReconnected são chamados

Cada vez que um navegador navega para uma nova página, uma nova conexão precisa ser estabelecida, o que significa que o Signalr executará o `OnDisconnected` método seguido pelo `OnConnected` método. O signalr sempre cria uma nova ID de conexão quando uma nova conexão é estabelecida.

O `OnReconnected` método é chamado quando houve uma interrupção temporária na conectividade com a qual o signalr pode se recuperar automaticamente, como quando um cabo é temporariamente desconectado e reconectado antes de a conexão atingir o tempo limite. O `OnDisconnected` método é chamado quando o cliente é desconectado e o signalr não pode se reconectar automaticamente, como quando um navegador navega para uma nova página. Portanto, uma possível sequência de eventos para um determinado cliente é `OnConnected` , `OnReconnected` , `OnDisconnected` ; ou `OnConnected` , `OnDisconnected` . Você não verá a sequência `OnConnected` , `OnDisconnected` , `OnReconnected` para uma determinada conexão.

O `OnDisconnected` método não é chamado em alguns cenários, como quando um servidor fica inoperante ou o domínio do aplicativo é reciclado. Quando outro servidor entra na linha ou o domínio do aplicativo conclui sua reciclagem, alguns clientes podem ser capazes de se reconectar e disparar o `OnReconnected` evento.

Para obter mais informações, consulte [noções básicas e manipulação de eventos de tempo de vida de conexão no signalr](handling-connection-lifetime-events.md).

<a id="nocallerstate"></a>

### <a name="caller-state-not-populated"></a>Estado do chamador não preenchido

Os métodos do manipulador de eventos de tempo de vida da conexão são chamados do servidor, o que significa que qualquer estado colocado no `state` objeto no cliente não será populado na `Caller` propriedade no servidor. Para obter informações sobre o `state` objeto e a `Caller` propriedade, consulte [como passar o estado entre clientes e a classe Hub](#passstate) mais adiante neste tópico.

<a id="contextproperty"></a>

## <a name="how-to-get-information-about-the-client-from-the-context-property"></a>Como obter informações sobre o cliente a partir da propriedade Context

Para obter informações sobre o cliente, use a `Context` propriedade da classe Hub. A `Context` propriedade retorna um objeto [HubCallerContext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx) que fornece acesso às seguintes informações:

- A ID de conexão do cliente de chamada.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample48.cs?highlight=1)]

    A ID de conexão é um GUID atribuído pelo Signalr (você não pode especificar o valor em seu próprio código). Há uma ID de conexão para cada conexão e a mesma ID de conexão é usada por todos os hubs se você tiver vários hubs em seu aplicativo.
- Dados de cabeçalho HTTP.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample49.cs?highlight=1)]

    Você também pode obter cabeçalhos HTTP de `Context.Headers` . O motivo para várias referências para a mesma coisa é que `Context.Headers` foi criado primeiro, a `Context.Request` propriedade foi adicionada mais tarde e `Context.Headers` foi retida para compatibilidade com versões anteriores.
- Dados de cadeia de caracteres de consulta.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample50.cs?highlight=1)]

    Você também pode obter dados de cadeia de caracteres de consulta do `Context.QueryString` .

    A cadeia de caracteres de consulta que você obtém nessa propriedade é aquela que foi usada com a solicitação HTTP que estabeleceu a conexão do Signalr. Você pode adicionar parâmetros de cadeia de caracteres de consulta no cliente Configurando a conexão, que é uma maneira conveniente de passar dados sobre o cliente do cliente para o servidor. O exemplo a seguir mostra uma maneira de adicionar uma cadeia de caracteres de consulta em um cliente JavaScript quando você usa o proxy gerado.

    [!code-javascript[Main](hubs-api-guide-server/samples/sample51.js?highlight=1)]

    Para obter mais informações sobre como definir parâmetros de cadeia de caracteres de consulta, consulte os guias de API para os clientes [JavaScript](hubs-api-guide-javascript-client.md) e [.net](hubs-api-guide-net-client.md) .

    Você pode encontrar o método de transporte usado para a conexão nos dados da cadeia de caracteres de consulta, juntamente com alguns outros valores usados internamente pelo Signalr:

    [!code-csharp[Main](hubs-api-guide-server/samples/sample52.cs)]

    O valor de `transportMethod` será "WebSockets", "serverSentEvents", "foreverFrame" ou "longPolling". Observe que, se você marcar esse valor no `OnConnected` método do manipulador de eventos, em alguns cenários, você poderá obter inicialmente um valor de transporte que não seja o método de transporte negociado final para a conexão. Nesse caso, o método lançará uma exceção e será chamado novamente mais tarde quando o método de transporte final for estabelecido.
- Arar.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample53.cs?highlight=1)]

    Você também pode obter cookies do `Context.RequestCookies` .
- Informação do usuário.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample54.cs?highlight=1)]
- O objeto HttpContext para a solicitação:

    [!code-csharp[Main](hubs-api-guide-server/samples/sample55.cs?highlight=1)]

    Use esse método em vez de `HttpContext.Current` obter o `HttpContext` objeto para a conexão do signalr.

<a id="passstate"></a>

## <a name="how-to-pass-state-between-clients-and-the-hub-class"></a>Como passar o estado entre clientes e a classe Hub

O proxy do cliente fornece um `state` objeto no qual você pode armazenar dados que deseja transmitir para o servidor com cada chamada de método. No servidor, você pode acessar esses dados na `Clients.Caller` propriedade em métodos de Hub que são chamados por clientes. A `Clients.Caller` propriedade não é preenchida para os métodos do manipulador de eventos de tempo de vida da conexão `OnConnected` , `OnDisconnected` e `OnReconnected` .

Criar ou atualizar dados no `state` objeto e a `Clients.Caller` propriedade funciona em ambas as direções. Você pode atualizar valores no servidor e eles são passados de volta para o cliente.

O exemplo a seguir mostra o código de cliente JavaScript que armazena o estado de transmissão para o servidor com cada chamada de método.

[!code-javascript[Main](hubs-api-guide-server/samples/sample56.js?highlight=1-2)]

O exemplo a seguir mostra o código equivalente em um cliente .NET.

[!code-csharp[Main](hubs-api-guide-server/samples/sample57.cs?highlight=1-2)]

Na sua classe de Hub, você pode acessar esses dados na `Clients.Caller` propriedade. O exemplo a seguir mostra o código que recupera o estado referido no exemplo anterior.

[!code-csharp[Main](hubs-api-guide-server/samples/sample58.cs?highlight=3-4)]

> [!NOTE]
> Esse mecanismo para o estado persistente não é destinado a grandes quantidades de dados, já que tudo que você coloca `state` na `Clients.Caller` propriedade ou é arredondado com cada invocação de método. É útil para itens menores, como nomes de usuário ou contadores.

No VB.NET ou em um hub fortemente tipado, o objeto de estado do chamador não pode ser acessado por meio de `Clients.Caller` ; em vez disso, use `Clients.CallerState` (introduzido no signalr 2,1):

**Usando o Callerstate em C #**

[!code-csharp[Main](hubs-api-guide-server/samples/sample59.cs?highlight=3-4)]

**Usando o Callerstate no Visual Basic**

[!code-vb[Main](hubs-api-guide-server/samples/sample60.vb)]

<a id="handleErrors"></a>

## <a name="how-to-handle-errors-in-the-hub-class"></a>Como tratar erros na classe Hub

Para lidar com erros que ocorrem em seus métodos de classe de Hub, primeiro verifique se você "observa" quaisquer exceções de operações assíncronas (como invocar métodos de cliente) usando `await` . Em seguida, use um ou mais dos seguintes métodos:

- Coloque o código do método em blocos try-catch e registre o objeto de exceção. Para fins de depuração, você pode enviar a exceção para o cliente, mas por motivos de segurança não é recomendável enviar informações detalhadas aos clientes em produção.
- Crie um módulo de pipeline de hubs que manipula o método [OnIncomingError](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) . O exemplo a seguir mostra um módulo de pipeline que registra erros, seguido pelo código em Startup.cs que injeta o módulo no pipeline de hubs.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample61.cs)]

    [!code-csharp[Main](hubs-api-guide-server/samples/sample62.cs?highlight=4)]
- Use a `HubException` classe (introduzida no signalr 2). Esse erro pode ser gerado de qualquer invocação de Hub. O `HubError` construtor usa uma mensagem de cadeia de caracteres e um objeto para armazenar dados de erro adicionais. O signalr irá serializar automaticamente a exceção e enviá-la para o cliente, onde será usada para rejeitar ou falhar a invocação do método de Hub.

    Os exemplos de código a seguir demonstram como lançar um `HubException` durante uma invocação de Hub e como tratar a exceção em clientes JavaScript e .net.

    **Código do servidor que demonstra a classe HubException**

    [!code-csharp[Main](hubs-api-guide-server/samples/sample63.cs)]

    **Código de cliente JavaScript que demonstra a resposta para lançar um HubException em um hub**

    [!code-html[Main](hubs-api-guide-server/samples/sample64.html)]

    **Código de cliente .NET que demonstra a resposta para lançar um HubException em um hub**

    [!code-csharp[Main](hubs-api-guide-server/samples/sample65.cs)]

Para obter mais informações sobre módulos de pipeline de Hub, consulte [como personalizar o pipeline de hubs](#hubpipeline) mais adiante neste tópico.

<a id="tracing"></a>

## <a name="how-to-enable-tracing"></a>Como habilitar o rastreamento

Para habilitar o rastreamento do lado do servidor, adicione um elemento System. Diagnostics ao arquivo Web.config, conforme mostrado neste exemplo:

[!code-html[Main](hubs-api-guide-server/samples/sample66.html?highlight=17-72)]

Ao executar o aplicativo no Visual Studio, você pode exibir os logs na janela **saída** .

<a id="callfromoutsidehub"></a>

## <a name="how-to-call-client-methods-and-manage-groups-from-outside-the-hub-class"></a>Como chamar métodos de cliente e gerenciar grupos de fora da classe de Hub

Para chamar métodos de cliente de uma classe diferente da classe de Hub, obtenha uma referência ao objeto de contexto do Signalr para o Hub e use-o para chamar métodos no cliente ou gerenciar grupos.

A classe de exemplo a seguir `StockTicker` Obtém o objeto de contexto, armazena-o em uma instância da classe, armazena a instância de classe em uma propriedade estática e usa o contexto da instância da classe singleton para chamar o `updateStockPrice` método em clientes que estão conectados a um Hub chamado `StockTickerHub` .

[!code-csharp[Main](hubs-api-guide-server/samples/sample67.cs?highlight=8,24)]

Se você precisar usar o contexto várias vezes em um objeto de vida útil longa, obtenha a referência uma vez e salve-a em vez de obtê-la novamente a cada vez. Obter o contexto quando garante que o Signalr envie mensagens para clientes na mesma sequência em que os métodos de Hub fazem invocações de método de cliente. Para obter um tutorial que mostra como usar o contexto do Signalr para um Hub, consulte [transmissão de servidor com o signalr ASP.net](../getting-started/tutorial-server-broadcast-with-signalr.md).

<a id="callingclientsoutsidehub"></a>

### <a name="calling-client-methods"></a>Chamando métodos de cliente

Você pode especificar quais clientes receberão o RPC, mas tem menos opções do que quando você chama de uma classe de Hub. O motivo para isso é que o contexto não está associado a uma chamada específica de um cliente, portanto, todos os métodos que exigem o conhecimento da ID de conexão atual, como `Clients.Others` , ou `Clients.Caller` , ou `Clients.OthersInGroup` , não estão disponíveis. As seguintes opções estão disponíveis:

- Todos os clientes conectados.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample68.cs)]
- Um cliente específico identificado pela ID de conexão.

    [!code-css[Main](hubs-api-guide-server/samples/sample69.css)]
- Todos os clientes conectados, exceto os clientes especificados, identificados pela ID de conexão.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample70.cs)]
- Todos os clientes conectados em um grupo especificado.

    [!code-css[Main](hubs-api-guide-server/samples/sample71.css)]
- Todos os clientes conectados em um grupo especificado, exceto os clientes especificados, identificados pela ID de conexão.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample72.cs)]

Se você estiver chamando a sua classe sem hub a partir de métodos em sua classe de Hub, poderá passar a ID de conexão atual e usá-la com `Clients.Client` , `Clients.AllExcept` ou `Clients.Group` para simular `Clients.Caller` , `Clients.Others` ou `Clients.OthersInGroup` . No exemplo a seguir, a `MoveShapeHub` classe passa a ID de conexão para a `Broadcaster` classe para que a `Broadcaster` classe possa simular `Clients.Others` .

[!code-csharp[Main](hubs-api-guide-server/samples/sample73.cs?highlight=12,36)]

<a id="managinggroupsoutsidehub"></a>

### <a name="managing-group-membership"></a>Gerenciando a associação de grupo

Para gerenciar grupos, você tem as mesmas opções que você faz em uma classe de Hub.

- Adicionar um cliente a um grupo

    [!code-csharp[Main](hubs-api-guide-server/samples/sample74.cs)]
- Remover um cliente de um grupo

    [!code-css[Main](hubs-api-guide-server/samples/sample75.css)]

<a id="hubpipeline"></a>

## <a name="how-to-customize-the-hubs-pipeline"></a>Como personalizar o pipeline de hubs

O signalr permite injetar seu próprio código no pipeline do Hub. O exemplo a seguir mostra um módulo de pipeline de Hub personalizado que registra cada chamada de método de entrada recebida do cliente e chamada de método de saída invocada no cliente:

[!code-csharp[Main](hubs-api-guide-server/samples/sample76.cs)]

O código a seguir no arquivo *Startup.cs* registra o módulo a ser executado no pipeline de Hub:

[!code-csharp[Main](hubs-api-guide-server/samples/sample77.cs?highlight=3)]

Há muitos métodos diferentes que podem ser substituídos. Para obter uma lista completa, consulte [métodos HubPipelineModule](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx).
