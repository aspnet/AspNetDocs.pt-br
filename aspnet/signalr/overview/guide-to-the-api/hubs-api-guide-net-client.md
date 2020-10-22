---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-net-client
title: Guia da API de hubs do Signalr ASP.NET – cliente .NET (C#) | Microsoft Docs
author: bradygaster
description: Este documento fornece uma introdução ao uso da API de hubs para o Signalr versão 2 em clientes .NET, como Windows Store (WinRT), WPF, Silverlight e contras...
ms.author: bradyg
ms.date: 01/15/2019
ms.assetid: 6d02d9f7-94e5-4140-9f51-5a6040f274f6
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-net-client
msc.type: authoredcontent
ms.openlocfilehash: d3536f1c15cd7dad7cd660becf0577e5c131f707
ms.sourcegitcommit: c62ec20b453cee3249eb894ecd75013b57d078f0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/21/2020
ms.locfileid: "92345549"
---
# <a name="aspnet-signalr-hubs-api-guide---net-client-c"></a>Guia da API de hubs do Signalr ASP.NET – cliente .NET (C#)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Este documento fornece uma introdução ao uso da API de hubs para o Signalr versão 2 em clientes .NET, como Windows Store (WinRT), WPF, Silverlight e aplicativos de console.
>
> A API de hubs de Signalr permite que você faça RPCs (chamadas de procedimento remoto) de um servidor para clientes conectados e de clientes para o servidor. No código do servidor, você define métodos que podem ser chamados por clientes e chama métodos que são executados no cliente. No código do cliente, você define métodos que podem ser chamados do servidor e chama métodos que são executados no servidor. O signalr cuida de todo o direcionamento de cliente para servidor para você.
>
> O signalr também oferece uma API de nível inferior chamada conexões persistentes. Para obter uma introdução ao Signalr, hubs e conexões persistentes, ou para um tutorial que mostra como criar um aplicativo Signalr completo, consulte [signalr-introdução](../getting-started/index.md).
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

- [Configuração do cliente](#clientsetup)
- [Como estabelecer uma conexão](#establishconnection)

    - [Conexões entre domínios de clientes do Silverlight](#slcrossdomain)
- [Como configurar a conexão](#configureconnection)

    - [Como definir o número máximo de conexões simultâneas em clientes do WPF](#maxconnections)
    - [Como especificar parâmetros de cadeia de caracteres de consulta](#querystring)
    - [Como especificar o método de transporte](#transport)
    - [Como especificar cabeçalhos HTTP](#httpheaders)
    - [Como especificar certificados de cliente](#clientcertificate)
- [Como criar o proxy de Hub](#proxy)
- [Como definir métodos no cliente que o servidor pode chamar](#callclient)

    - [Métodos sem parâmetros](#clientmethodswithoutparms)
    - [Métodos com parâmetros, especificando tipos de parâmetro](#clientmethodswithparmtypes)
    - [Métodos com parâmetros, especificando objetos dinâmicos para os parâmetros](#clientmethodswithdynamparms)
    - [Como remover um manipulador](#removehandler)
- [Como chamar métodos de servidor do cliente](#callserver)
- [Como tratar eventos de tempo de vida da conexão](#connectionlifetime)
- [Como tratar erros](#handleerrors)
- [Como habilitar o log do lado do cliente](#logging)
- [Exemplos de código de aplicativo do WPF, Silverlight e console para métodos de cliente que o servidor pode chamar](#wpfsl)

Para obter um exemplo de projetos de cliente .NET, consulte os seguintes recursos:

- [Gustavo-Armenta/signalr-exemplos](https://github.com/gustavo-armenta/SignalR-Samples) em github.com (WinRT, Silverlight, exemplos de aplicativos de console).
- [DamianEdwards/SignalR-MoveShapeDemo/MoveShape. desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) em github.com (exemplo de WPF).
- [Signalr/Microsoft. AspNet. signalr. Client. Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) em github.com (exemplo de aplicativo de console).

Para obter a documentação sobre como programar os clientes do servidor ou JavaScript, consulte os seguintes recursos:

- [Guia da API de hubs de signalr-servidor](hubs-api-guide-server.md)
- [Guia de API de hubs de signalr-cliente JavaScript](hubs-api-guide-javascript-client.md)

Links para os tópicos de referência de API são para a versão 4,5 do .NET da API. Se você estiver usando o .NET 4, consulte [a versão .NET 4 dos tópicos da API](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).

<a id="clientsetup"></a>

## <a name="client-setup"></a>Configuração do cliente

Instale o pacote NuGet [Microsoft. AspNet. signaler. Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) (não o pacote [Microsoft. AspNet. signalr](http://nuget.org/packages/microsoft.aspnet.signalr) ). Este pacote dá suporte a clientes do WinRT, Silverlight, WPF, aplicativos de console e Windows Phone, tanto para o .NET 4 quanto para o .NET 4,5.

Se a versão do Signalr que você tem no cliente for diferente da versão que você tem no servidor, o Signalr geralmente é capaz de se adaptar à diferença. Por exemplo, um servidor executando o Signalr versão 2 dará suporte a clientes que têm o 1.1. x instalado, bem como clientes que têm a versão 2 instalada. Se a diferença entre a versão no servidor e a versão no cliente for muito grande, ou se o cliente for mais recente do que o servidor, o Signalr lançará uma `InvalidOperationException` exceção quando o cliente tentar estabelecer uma conexão. A mensagem de erro é " `You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X` ".

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a>Como estabelecer uma conexão

Antes de poder estabelecer uma conexão, você precisa criar um `HubConnection` objeto e criar um proxy. Para estabelecer a conexão, chame o `Start` método no `HubConnection` objeto.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample1.cs?highlight=1,5)]

> [!NOTE]
> Para clientes JavaScript, você precisa registrar pelo menos um manipulador de eventos antes de chamar o `Start` método para estabelecer a conexão. Isso não é necessário para clientes .NET. Para clientes JavaScript, o código de proxy gerado cria automaticamente proxies para todos os hubs existentes no servidor, e o registro de um manipulador é como você indica quais hubs seu cliente pretende usar. Mas, para um cliente .NET, você cria proxies de Hub manualmente, portanto, o Signalr pressupõe que você usará qualquer Hub para o qual criar um proxy.

O código de exemplo usa a URL "/signalr" padrão para se conectar ao seu serviço Signalr. Para obter informações sobre como especificar uma URL base diferente, consulte [guia da API de hubs do ASP.net signalr-Server-a URL do/signalr](hubs-api-guide-server.md#signalrurl).

O `Start` método é executado de forma assíncrona. Para garantir que as linhas de código subsequentes não sejam executadas até que a conexão seja estabelecida, use `await` em um método assíncrono ASP.NET 4,5 ou `.Wait()` em um método síncrono. Não use `.Wait()` em um cliente WinRT.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample2.cs?highlight=1)]

[!code-css[Main](hubs-api-guide-net-client/samples/sample3.css?highlight=1)]

<a id="slcrossdomain"></a>

### <a name="cross-domain-connections-from-silverlight-clients"></a>Conexões entre domínios de clientes do Silverlight

Para obter informações sobre como habilitar conexões entre domínios de clientes do Silverlight, consulte [disponibilizando um serviço entre limites de domínio](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx).

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a>Como configurar a conexão

Antes de estabelecer uma conexão, você pode especificar qualquer uma das seguintes opções:

- Limite de conexões simultâneas.
- Parâmetros de cadeia de caracteres de consulta.
- O método de transporte.
- Cabeçalhos HTTP.
- Certificados de cliente.

<a id="maxconnections"></a>

### <a name="how-to-set-the-maximum-number-of-concurrent-connections-in-wpf-clients"></a>Como definir o número máximo de conexões simultâneas em clientes do WPF

Em clientes do WPF, talvez seja necessário aumentar o número máximo de conexões simultâneas de seu valor padrão de 2. O valor recomendado é 10.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample4.cs?highlight=5)]

Para obter mais informações, consulte [ServicePointManager. DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a>Como especificar parâmetros de cadeia de caracteres de consulta

Se você quiser enviar dados ao servidor quando o cliente se conectar, poderá adicionar parâmetros de cadeia de caracteres de consulta ao objeto de conexão. O exemplo a seguir mostra como definir um parâmetro de cadeia de caracteres de consulta no código do cliente.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample5.cs)]

O exemplo a seguir mostra como ler um parâmetro de cadeia de caracteres de consulta no código do servidor.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample6.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a>Como especificar o método de transporte

Como parte do processo de conexão, um cliente do Signalr normalmente negocia com o servidor para determinar o melhor transporte que é suportado pelo servidor e pelo cliente. Se você já souber qual transporte deseja usar, poderá ignorar esse processo de negociação. Para especificar o método de transporte, passe um objeto de transporte para o método Start. O exemplo a seguir mostra como especificar o método de transporte no código do cliente.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample7.cs?highlight=5)]

O namespace [Microsoft. AspNet. signaler. Client. Exports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) inclui as seguintes classes que você pode usar para especificar o transporte.

- [LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)
- [ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)
- [WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (disponível somente quando o servidor e o cliente usam o .NET 4,5.)
- O [transporte](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) automático (escolhe automaticamente o melhor transporte que é suportado pelo cliente e pelo servidor. Esse é o transporte padrão. Passar isso para o `Start` método tem o mesmo efeito que não está passando em nada.)

O transporte ForeverFrame não está incluído nesta lista porque ele é usado somente por navegadores.

Para obter informações sobre como verificar o método de transporte no código do servidor, consulte [guia da API de hubs do signaler ASP.NET-Server-como obter informações sobre o cliente a partir da propriedade Context](hubs-api-guide-server.md#contextproperty). Para obter mais informações sobre transportes e fallbacks, consulte [introdução ao signalr-transportes e fallbacks](../getting-started/introduction-to-signalr.md#transports).

<a id="httpheaders"></a>

### <a name="how-to-specify-http-headers"></a>Como especificar cabeçalhos HTTP

Para definir cabeçalhos HTTP, use a `Headers` propriedade no objeto de conexão. O exemplo a seguir mostra como adicionar um cabeçalho HTTP.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample8.cs?highlight=2)]

<a id="clientcertificate"></a>

### <a name="how-to-specify-client-certificates"></a>Como especificar certificados de cliente

Para adicionar certificados de cliente, use o `AddClientCertificate` método no objeto de conexão.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample9.cs?highlight=2)]

<a id="proxy"></a>

## <a name="how-to-create-the-hub-proxy"></a>Como criar o proxy de Hub

Para definir métodos no cliente que um hub pode chamar do servidor e para invocar métodos em um Hub no servidor, crie um proxy para o Hub chamando `CreateHubProxy` o objeto de conexão. A cadeia de caracteres que você passa para `CreateHubProxy` é o nome da sua classe de Hub ou o nome especificado pelo `HubName` atributo, se um foi usado no servidor. A correspondência de nome não diferencia maiúsculas de minúsculas.

**Classe de Hub no servidor**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample10.cs?highlight=1)]

**Criar proxy de cliente para a classe de Hub**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample11.cs?highlight=3)]

Se você decorar a classe de Hub com um `HubName` atributo, use esse nome.

**Classe de Hub no servidor**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample12.cs)]

**Criar proxy de cliente para a classe de Hub**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample13.cs?highlight=3)]

Se você chamar `HubConnection.CreateHubProxy` várias vezes com o mesmo `hubName` , obterá o mesmo objeto armazenado em cache `IHubProxy` .

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a>Como definir métodos no cliente que o servidor pode chamar

Para definir um método que o servidor pode chamar, use o método do proxy `On` para registrar um manipulador de eventos.

A correspondência de nome de método não diferencia maiúsculas de minúsculas. Por exemplo, `Clients.All.UpdateStockPrice` no servidor será executado `updateStockPrice` , `updatestockprice` ou `UpdateStockPrice` no cliente.

Diferentes plataformas de cliente têm requisitos diferentes para a maneira como você escreve o código do método para atualizar a interface do usuário. Os exemplos mostrados são para clientes do WinRT (Windows Store .NET). Os exemplos de aplicativos WPF, Silverlight e console são fornecidos em [uma seção separada mais adiante neste tópico](#wpfsl).

<a id="clientmethodswithoutparms"></a>

### <a name="methods-without-parameters"></a>Métodos sem parâmetros

Se o método que você está manipulando não tiver parâmetros, use a sobrecarga não genérica do `On` método:

**Código do servidor chamando método de cliente sem parâmetros**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample14.cs?highlight=5)]

**Código de cliente do WinRT para o método chamado do servidor sem parâmetros ([consulte os exemplos do WPF e do Silverlight mais adiante neste tópico](#wpfsl))**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample15.cs)]

<a id="clientmethodswithparmtypes"></a>

### <a name="methods-with-parameters-specifying-the-parameter-types"></a>Métodos com parâmetros, especificando os tipos de parâmetro

Se o método que você está manipulando tiver parâmetros, especifique os tipos dos parâmetros como os tipos genéricos do `On` método. Há sobrecargas genéricas do `On` método para permitir que você especifique até oito parâmetros (4 em Windows Phone 7). No exemplo a seguir, um parâmetro é enviado para o `UpdateStockPrice` método.

**Código do servidor que chama o método de cliente com um parâmetro**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample16.cs?highlight=3)]

**A classe Stock usada para o parâmetro**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample17.cs)]

**Código de cliente do WinRT para um método chamado do servidor com um parâmetro ([consulte os exemplos do WPF e do Silverlight mais adiante neste tópico](#wpfsl))**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample18.cs?highlight=1,5)]

<a id="clientmethodswithdynamparms"></a>

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a>Métodos com parâmetros, especificando objetos dinâmicos para os parâmetros

Como alternativa para especificar parâmetros como tipos genéricos do `On` método, você pode especificar parâmetros como objetos dinâmicos:

**Código do servidor que chama o método de cliente com um parâmetro**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample19.cs?highlight=3)]

**A classe Stock usada para o parâmetro**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample20.cs)]

**Código de cliente do WinRT para um método chamado do servidor com um parâmetro, usando um objeto dinâmico para o parâmetro ([consulte os exemplos do WPF e do Silverlight mais adiante neste tópico](#wpfsl))**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample21.cs?highlight=1,5)]

<a id="removehandler"></a>

### <a name="how-to-remove-a-handler"></a>Como remover um manipulador

Para remover um manipulador, chame seu `Dispose` método.

**Código do cliente para um método chamado do servidor**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample22.cs?highlight=1)]

**Código do cliente para remover o manipulador**

[!code-css[Main](hubs-api-guide-net-client/samples/sample23.css?highlight=1)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a>Como chamar métodos de servidor do cliente

Para chamar um método no servidor, use o `Invoke` método no proxy do Hub.

Se o método de servidor não tiver nenhum valor de retorno, use a sobrecarga não genérica do `Invoke` método.

**Código do servidor para um método que não tem valor de retorno**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample24.cs?highlight=3)]

**Código do cliente chamando um método que não tem valor de retorno**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample25.cs?highlight=1)]

Se o método de servidor tiver um valor de retorno, especifique o tipo de retorno como o tipo genérico do `Invoke` método.

**Código de servidor para um método que tem um valor de retorno e usa um parâmetro de tipo complexo**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample26.cs?highlight=1)]

**A classe Stock usada para o parâmetro e o valor de retorno**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample27.cs)]

**O código do cliente que chama um método que tem um valor de retorno e usa um parâmetro de tipo complexo, em um método assíncrono ASP.NET 4,5**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample28.cs?highlight=1-2)]

**O código do cliente que chama um método que tem um valor de retorno e usa um parâmetro de tipo complexo, em um método síncrono**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample29.cs?highlight=1-2)]

O `Invoke` método é executado de forma assíncrona e retorna um `Task` objeto. Se você não especificar `await` ou `.Wait()` , a próxima linha de código será executada antes de o método que você invocar terminar de ser executado.

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a>Como tratar eventos de tempo de vida da conexão

O signalr fornece os seguintes eventos de tempo de vida de conexão que você pode manipular:

- `Received`: Ocorre quando os dados são recebidos na conexão. Fornece os dados recebidos.
- `ConnectionSlow`: Ocorre quando o cliente detecta uma conexão lenta ou com frequência.
- `Reconnecting`: Ocorre quando o transporte subjacente começa a se reconectar.
- `Reconnected`: Ocorre quando o transporte subjacente é reconectado.
- `StateChanged`: Ocorre quando o estado da conexão é alterado. Fornece o estado antigo e o novo estado. Para obter informações sobre valores de estado de conexão, consulte [Enumeração ConnectionState](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).
- `Closed`: Ocorre quando a conexão é desconectada.

Por exemplo, se você quiser exibir mensagens de aviso para erros que não são fatais, mas causar problemas intermitentes de conexão, como lentidão ou descartar frequentes da conexão, manipule o `ConnectionSlow` evento.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample30.cs)]

Para obter mais informações, consulte [noções básicas e manipulação de eventos de tempo de vida de conexão no signalr](handling-connection-lifetime-events.md).

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a>Como tratar erros

Se você não habilitar explicitamente mensagens de erro detalhadas no servidor, o objeto de exceção que o Signalr retorna após um erro contém informações mínimas sobre o erro. Por exemplo, se uma chamada para `newContosoChatMessage` falhar, a mensagem de erro no objeto de erro conterá " `There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.` " não é recomendável enviar mensagens de erro detalhadas aos clientes em produção por motivos de segurança, mas se você quiser habilitar mensagens de erro detalhadas para fins de solução de problemas, use o código a seguir no servidor.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample31.cs?highlight=2)]

<a id="handleerrors"></a>

Para lidar com erros que o Signalr gera, você pode adicionar um manipulador para o `Error` evento no objeto de conexão.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample32.cs)]

Para tratar erros de invocações de método, empacote o código em um bloco try-catch.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample33.cs)]

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a>Como habilitar o log do lado do cliente

Para habilitar o log do lado do cliente, defina as `TraceLevel` `TraceWriter` Propriedades e no objeto de conexão.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample34.cs?highlight=3-4)]

<a id="wpfsl"></a>

## <a name="wpf-silverlight-and-console-application-code-samples-for-client-methods-that-the-server-can-call"></a>Exemplos de código de aplicativo do WPF, Silverlight e console para métodos de cliente que o servidor pode chamar

Os exemplos de código mostrados anteriormente para definir métodos de cliente que o servidor pode chamar aplicam a clientes do WinRT. Os exemplos a seguir mostram o código equivalente para o WPF, o Silverlight e os clientes de aplicativos de console.

### <a name="methods-without-parameters"></a>Métodos sem parâmetros

**Código de cliente do WPF para o método chamado do servidor sem parâmetros**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample35.cs?highlight=1)]

**Código do cliente Silverlight para o método chamado do servidor sem parâmetros**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample36.cs?highlight=1)]

**Código do cliente do aplicativo de console para o método chamado do servidor sem parâmetros**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample37.cs?highlight=1)]

### <a name="methods-with-parameters-specifying-the-parameter-types"></a>Métodos com parâmetros, especificando os tipos de parâmetro

**Código de cliente do WPF para um método chamado do servidor com um parâmetro**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample38.cs?highlight=1,4)]

**Código do cliente Silverlight para um método chamado do servidor com um parâmetro**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample39.cs?highlight=1,5)]

**Código de cliente do aplicativo de console para um método chamado do servidor com um parâmetro**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample40.cs?highlight=1-2)]

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a>Métodos com parâmetros, especificando objetos dinâmicos para os parâmetros

**Código de cliente do WPF para um método chamado do servidor com um parâmetro, usando um objeto dinâmico para o parâmetro**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample41.cs?highlight=1,4)]

**Código do cliente Silverlight para um método chamado do servidor com um parâmetro, usando um objeto dinâmico para o parâmetro**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample42.cs?highlight=1,5)]

**Código de cliente do aplicativo de console para um método chamado do servidor com um parâmetro, usando um objeto dinâmico para o parâmetro**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample43.cs?highlight=1-2)]
