---
uid: signalr/overview/guide-to-the-api/handling-connection-lifetime-events
title: Compreendendo e manipulando eventos de tempo de vida de conexão no Signalr | Microsoft Docs
author: bradygaster
description: Este artigo descreve como usar os eventos expostos pela API de hubs.
ms.author: bradyg
ms.date: 01/15/2019
ms.assetid: 03960de2-8d95-4444-9169-4426dcc64913
msc.legacyurl: /signalr/overview/guide-to-the-api/handling-connection-lifetime-events
msc.type: authoredcontent
ms.openlocfilehash: 5bdf20549fccab5d644e35fdf4ce351540c8620d
ms.sourcegitcommit: c62ec20b453cee3249eb894ecd75013b57d078f0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/21/2020
ms.locfileid: "92345227"
---
# <a name="understanding-and-handling-connection-lifetime-events-in-signalr"></a>Noções básicas e tratamento de eventos de tempo de vida de conexão no SignalR

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Este artigo fornece uma visão geral dos eventos de conexão, reconexão e desconexão do Signalr que você pode manipular, bem como configurações de tempo limite e KeepAlive que você pode configurar.
>
> O artigo pressupõe que você já tenha algum conhecimento dos eventos de tempo de vida de conexão e de sinalização. Para obter uma introdução ao Signalr, consulte [introdução ao signalr](../getting-started/introduction-to-signalr.md). Para obter listas de eventos de tempo de vida de conexão, consulte os seguintes recursos:
>
> - [Como tratar eventos de tempo de vida da conexão na classe Hub](hubs-api-guide-server.md#connectionlifetime)
> - [Como lidar com eventos de tempo de vida de conexão em clientes JavaScript](hubs-api-guide-javascript-client.md#connectionlifetime)
> - [Como lidar com eventos de tempo de vida de conexão em clientes .NET](hubs-api-guide-net-client.md#connectionlifetime)
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

Este artigo inclui as seções a seguir:

- [Terminologia e cenários de tempo de vida da conexão](#terminology)

    - [Conexões de sinalização, conexões de transporte e conexões físicas](#signalrvstransport)
    - [Cenários de desconexão de transporte](#transportdisconnect)
    - [Cenários de desconexão do cliente](#clientdisconnect)
    - [Cenários de desconexão do servidor](#serverdisconnect)
- [Configurações de tempo limite e KeepAlive](#timeoutkeepalive)

    - [ConnectionTimeout](#connectiontimeout)
    - [DisconnectTimeout](#disconnecttimeout)
    - [KeepAlive](#keepalive)
    - [Como alterar o tempo limite e as configurações de KeepAlive](#changetimeout)
- [Como notificar o usuário sobre desconexões](#notifydisconnect)
- [Como reconectar continuamente](#continuousreconnect)
- [Como desconectar um cliente no código do servidor](#disconnectclientfromserver)
- [Detectando o motivo de uma desconexão](#detectingreasonfordisconnection)

Links para os tópicos de referência de API são para a versão 4,5 do .NET da API. Se você estiver usando o .NET 4, consulte [a versão .NET 4 dos tópicos da API](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).

<a id="terminology"></a>

## <a name="connection-lifetime-terminology-and-scenarios"></a>Terminologia e cenários de tempo de vida da conexão

O `OnReconnected` manipulador de eventos em um Hub do signalr pode ser executado diretamente após `OnConnected` , mas não depois, `OnDisconnected` para um determinado cliente. O motivo pelo qual você pode ter uma reconexão sem uma desconexão é que há várias maneiras pelas quais a palavra "conexão" é usada no Signalr.

<a id="signalrvstransport"></a>

### <a name="signalr-connections-transport-connections-and-physical-connections"></a>Conexões de sinalização, conexões de transporte e conexões físicas

Este artigo diferenciará conexões de *sinalização*, *conexões de transporte*e conexões *físicas*:

- A **conexão do signalr** refere-se a uma relação lógica entre um cliente e uma URL do servidor, mantida pela API do signalr e identificada exclusivamente por uma ID de conexão. Os dados sobre essa relação são mantidos pelo Signalr e são usados para estabelecer uma conexão de transporte. A relação termina e o Signalr descarta os dados quando o cliente chama o `Stop` método ou um limite de tempo limite é atingido enquanto o signalr está tentando restabelecer uma conexão de transporte perdida.
- A **conexão de transporte** refere-se a uma relação lógica entre um cliente e um servidor, mantida por uma das quatro APIs de transporte: WebSockets, eventos enviados pelo servidor, quadro contínuo ou sondagem longa. O signalr usa a API de transporte para criar uma conexão de transporte e a API de transporte depende da existência de uma conexão de rede física para criar a conexão de transporte. A conexão de transporte termina quando o Signalr o encerra ou quando a API de transporte detecta que a conexão física está quebrada.
- **Conexão física** refere-se aos links de rede física – cabos, sinais sem fio, roteadores, etc., que facilitam a comunicação entre um computador cliente e um computador servidor. A conexão física deve estar presente para estabelecer uma conexão de transporte e uma conexão de transporte deve ser estabelecida para estabelecer uma conexão de Signalr. No entanto, interromper a conexão física nem sempre termina imediatamente a conexão de transporte ou a conexão do Signalr, como será explicado mais adiante neste tópico.

No diagrama a seguir, a conexão do Signalr é representada pela API de hubs e pela camada de Signalr de API PersistentConnection, a conexão de transporte é representada pela camada de transportes e a conexão física é representada pelas linhas entre o servidor e os clientes.

![Diagrama de arquitetura do signalr](handling-connection-lifetime-events/_static/image1.png)

Ao chamar o `Start` método em um cliente do signalr, você fornece um código de cliente do signalr com todas as informações necessárias para estabelecer uma conexão física com um servidor. O código de cliente do signalr usa essas informações para fazer uma solicitação HTTP e estabelecer uma conexão física que usa um dos quatro métodos de transporte. Se a conexão de transporte falhar ou se o servidor falhar, a conexão do Signalr não desaparece imediatamente porque o cliente ainda tem as informações necessárias para restabelecer automaticamente uma nova conexão de transporte com a mesma URL do Signalr. Nesse cenário, nenhuma intervenção do aplicativo do usuário está envolvida e quando o código do cliente do Signalr estabelece uma nova conexão de transporte, ele não inicia uma nova conexão de Signalr. A continuidade da conexão do Signalr é refletida no fato de que a ID da conexão, que é criada quando você chama o `Start` método, não é alterada.

O `OnReconnected` manipulador de eventos no Hub é executado quando uma conexão de transporte é restabelecida automaticamente após ter sido perdida. O `OnDisconnected` manipulador de eventos é executado no final de uma conexão de signalr. Uma conexão de Signalr pode terminar de uma das seguintes maneiras:

- Se o cliente chamar o `Stop` método, uma mensagem de parada será enviada ao servidor e o cliente e o servidor terminarão a conexão do signalr imediatamente.
- Depois que a conectividade entre o cliente e o servidor for perdida, o cliente tentará se reconectar e o servidor aguardará que o cliente se reconecte. Se as tentativas de reconexão não forem bem-sucedidas e o período de tempo limite de desconexão terminar, o cliente e o servidor terminarão a conexão do Signalr. O cliente para de tentar se reconectar e o servidor descarta sua representação da conexão do Signalr.
- Se o cliente parar de executar sem ter a oportunidade de chamar o `Stop` método, o servidor aguardará que o cliente se reconecte e, em seguida, encerrará a conexão do signalr após o período de tempo limite de desconexão.
- Se o servidor parar de ser executado, o cliente tentará se reconectar (recriar a conexão de transporte) e encerrará a conexão do Signalr após o período de tempo limite de desconexão.

Quando não há problemas de conexão e o aplicativo do usuário termina a conexão do Signalr chamando o `Stop` método, a conexão do signalr e a conexão de transporte começam e terminam ao mesmo tempo. As seções a seguir descrevem mais detalhadamente os outros cenários.

<a id="transportdisconnect"></a>

### <a name="transport-disconnection-scenarios"></a>Cenários de desconexão de transporte

As conexões físicas podem ser lentas ou pode haver interrupções na conectividade. Dependendo de fatores como o comprimento da interrupção, a conexão de transporte pode ser descartada. Em seguida, o signalr tenta restabelecer a conexão de transporte. Às vezes, a API de conexão de transporte detecta a interrupção e descarta a conexão de transporte e o sinalizador descobre imediatamente que a conexão é perdida. Em outros cenários, nem a API de conexão de transporte nem o sinalizador se reconhece imediatamente que a conectividade foi perdida. Para todos os transportes, exceto sondagem longa, o cliente do Signalr usa uma função chamada *KeepAlive* para verificar a perda de conectividade que a API de transporte não consegue detectar. Para obter informações sobre conexões de sondagem longas, veja [configurações de tempo limite e KeepAlive](#timeoutkeepalive) posteriormente neste tópico.

Quando uma conexão está inativa, periodicamente, o servidor envia um pacote KeepAlive ao cliente. A partir da data em que este artigo está sendo gravado, a frequência padrão é a cada 10 segundos. Ao escutar esses pacotes, os clientes podem saber se há um problema de conexão. Se um pacote KeepAlive não for recebido quando esperado, após um curto período, o cliente assumirá que há problemas de conexão, como lentidão ou interrupções. Se o KeepAlive ainda não for recebido após uma hora mais longa, o cliente assumirá que a conexão foi descartada e começará a tentar se reconectar.

O diagrama a seguir ilustra os eventos de cliente e servidor que são gerados em um cenário típico quando há problemas com a conexão física que não são reconhecidas imediatamente pela API de transporte. O diagrama se aplica às seguintes circunstâncias:

- O transporte é WebSockets, quadro para sempre ou eventos enviados pelo servidor.
- Há períodos variáveis de interrupção na conexão de rede física.
- A API de transporte não reconhece as interrupções, portanto, o Signalr conta com a funcionalidade KeepAlive para detectá-las.

![Desconexões de transporte](handling-connection-lifetime-events/_static/image2.png)

Se o cliente entrar no modo de reconexão, mas não puder estabelecer uma conexão de transporte dentro do tempo limite de desconexão, o servidor encerrará a conexão com o Signalr. Quando isso acontece, o servidor executa o método do Hub `OnDisconnected` e enfileira uma mensagem de desconexão para enviar ao cliente, caso o cliente consiga se conectar mais tarde. Se o cliente reconectar, ele receberá o comando Disconnect e chamará o `Stop` método. Nesse cenário, o `OnReconnected` não é executado quando o cliente se reconecta e `OnDisconnected` não é executado quando o cliente chama `Stop` . O diagrama a seguir ilustra esse cenário.

![Interrupções de transporte-tempo limite do servidor](handling-connection-lifetime-events/_static/image3.png)

Os eventos de tempo de vida de conexão do Signalr que podem ser gerados no cliente são os seguintes:

- `ConnectionSlow` evento de cliente.

    Gerado quando uma proporção predefinida do período de tempo limite de KeepAlive foi aprovada desde que a última mensagem ou o ping de KeepAlive foi recebido. O período de aviso de tempo limite de KeepAlive padrão é 2/3 do tempo limite de KeepAlive. O tempo limite de KeepAlive é de 20 segundos, portanto, o aviso ocorre em aproximadamente 13 segundos.

    Por padrão, o servidor envia pings de KeepAlive a cada 10 segundos e o cliente verifica se há pings de KeepAlive a cada 2 segundos (um terço da diferença entre o valor de tempo limite de KeepAlive e o valor de aviso de tempo limite de KeepAlive).

    Se a API de transporte ficar ciente de uma desconexão, o Signalr poderá ser informado da desconexão antes que o período de aviso de tempo limite de KeepAlive passe. Nesse caso, o `ConnectionSlow` evento não será gerado e o signalr vai diretamente para o `Reconnecting` evento.
- `Reconnecting` evento de cliente.

    Gerado quando (a) a API de transporte detecta que a conexão foi perdida ou (b) o período de tempo limite de KeepAlive passou desde que a última mensagem ou o ping de KeepAlive foi recebido. O código do cliente Signalr começa a tentar se reconectar. Você pode manipular esse evento se quiser que seu aplicativo execute alguma ação quando uma conexão de transporte for perdida. O período de tempo limite padrão de KeepAlive é de 20 segundos no momento.

    Se o seu código de cliente tentar chamar um método de Hub enquanto o Signalr estiver no modo de reconexão, o Signalr tentará enviar o comando. Na maioria das vezes, essas tentativas falharão, mas em algumas circunstâncias elas podem ter êxito. Para os eventos enviados pelo servidor, o quadro para sempre e os transportes de sondagem longa, o Signalr usa dois canais de comunicação, um que o cliente usa para enviar mensagens e um que ele usa para receber mensagens. O canal usado para receber é o aberto permanentemente e é aquele que é fechado quando a conexão física é interrompida. O canal usado para envio permanece disponível, portanto, se a conectividade física for restaurada, uma chamada de método do cliente para o servidor poderá ser bem-sucedida antes que o canal de recebimento seja restabelecido. O valor de retorno não seria recebido até que o Signalr Abra novamente o canal usado para receber.
- `Reconnected` evento de cliente.

    Gerado quando a conexão de transporte é restabelecida. O `OnReconnected` manipulador de eventos no Hub é executado.
- `Closed` evento de cliente ( `disconnected` evento em JavaScript).

    Gerado quando o período de tempo limite de desconexão expira enquanto o código de cliente do Signalr está tentando se reconectar depois de perder a conexão de transporte. O tempo limite de desconexão padrão é de 30 segundos. (Esse evento também é gerado quando a conexão termina porque o `Stop` método é chamado.)

As interrupções de conexão de transporte que não são detectadas pela API de transporte e não atrasam a recepção de pings de KeepAlive do servidor por mais tempo do que o período de aviso de tempo limite de KeepAlive podem não causar a geração de eventos de vida útil de conexão.

Alguns ambientes de rede fecham conexões ociosas deliberadamente, e outra função dos pacotes KeepAlive é ajudar a evitar isso, permitindo que essas redes saibam que uma conexão de Signalr está em uso. Em casos extremos, a frequência padrão dos pings de KeepAlive pode não ser suficiente para evitar conexões fechadas. Nesse caso, você pode configurar os pings de KeepAlive para serem enviados com mais frequência. Para obter mais informações, consulte [tempo limite e configurações de KeepAlive](#timeoutkeepalive) posteriormente neste tópico.

> [!NOTE]
>
> **Importante**: a sequência de eventos descrita aqui não é garantida. O signalr faz cada tentativa de gerar eventos de tempo de vida de conexão de forma previsível de acordo com esse esquema, mas há muitas variações de eventos de rede e muitas maneiras em que estruturas de comunicação subjacentes, como as APIs de transporte, lidam com elas. Por exemplo, o `Reconnected` evento pode não ser gerado quando o cliente se reconecta, ou o `OnConnected` manipulador no servidor pode ser executado quando a tentativa de estabelecer uma conexão não é bem-sucedida. Este tópico descreve apenas os efeitos que normalmente seriam produzidos por determinadas circunstâncias típicas.

<a id="clientdisconnect"></a>

### <a name="client-disconnection-scenarios"></a>Cenários de desconexão do cliente

Em um cliente de navegador, o código de cliente do Signalr que mantém uma conexão de sinalização é executado no contexto JavaScript de uma página da Web. É por isso que a conexão do Signalr precisa ser encerrada quando você navega de uma página para outra, e é por isso que você tem várias conexões com várias IDs de conexão se você se conectar de várias janelas de navegador ou guias. Quando o usuário fecha uma janela ou guia do navegador, ou navega para uma nova página ou atualiza a página, a conexão do Signalr termina imediatamente porque o código do cliente do Signalr trata esse evento de navegador para você e chama o `Stop` método. Nesses cenários, ou em qualquer plataforma de cliente quando seu aplicativo chama o `Stop` método, o `OnDisconnected` manipulador de eventos é executado imediatamente no servidor e o cliente gera o `Closed` evento (o evento é nomeado `disconnected` em JavaScript).

Se um aplicativo cliente ou o computador em que ele está sendo executado falhar ou entrar em suspensão (por exemplo, quando o usuário fechar o laptop), o servidor não será informado sobre o que aconteceu. Até onde o servidor sabe, a perda do cliente pode ser devido à interrupção da conectividade e o cliente pode estar tentando se reconectar. Portanto, nesses cenários, o servidor espera fornecer ao cliente a oportunidade de se reconectar e não `OnDisconnected` é executado até que o período de tempo limite de desconexão expire (cerca de 30 segundos por padrão). O diagrama a seguir ilustra esse cenário.

![Falha do computador cliente](handling-connection-lifetime-events/_static/image4.png)

<a id="serverdisconnect"></a>

### <a name="server-disconnection-scenarios"></a>Cenários de desconexão do servidor

Quando um servidor fica offline, ele é reinicializado, falha, o domínio do aplicativo é reciclado, etc. – o resultado pode ser semelhante a uma conexão perdida, ou a API de transporte e o Signalr podem saber imediatamente que o servidor não existe e o Signalr pode começar a tentar se reconectar sem gerar o `ConnectionSlow` evento. Se o cliente entrar no modo de reconexão e se o servidor for recuperado ou reiniciado ou um novo servidor for colocado online antes do período de tempo limite de desconexão expirar, o cliente se reconectará ao servidor restaurado ou novo. Nesse caso, a conexão do Signalr continua no cliente e o `Reconnected` evento é gerado. No primeiro servidor, `OnDisconnected` nunca é executado e, no novo servidor, `OnReconnected` é executado, embora `OnConnected` nunca tenha sido executado para esse cliente nesse servidor antes. (O efeito é o mesmo se o cliente se reconectar ao mesmo servidor após uma reinicialização ou reciclagem de domínio de aplicativo, porque quando o servidor reinicia, ele não tem memória da atividade de conexão anterior.) O diagrama a seguir pressupõe que a API de transporte esteja ciente da conexão perdida imediatamente, portanto o `ConnectionSlow` evento não é gerado.

![Falha e reconexão do servidor](handling-connection-lifetime-events/_static/image5.png)

Se um servidor não estiver disponível dentro do período de tempo limite de desconexão, a conexão do Signalr será encerrada. Nesse cenário, o `Closed` evento ( `disconnected` em clientes JavaScript) é gerado no cliente, mas `OnDisconnected` nunca é chamado no servidor. O diagrama a seguir pressupõe que a API de transporte não esteja ciente da conexão perdida, portanto, ela é detectada pela funcionalidade de KeepAlive do Signalr e o `ConnectionSlow` evento é gerado.

![Falha e tempo limite do servidor](handling-connection-lifetime-events/_static/image6.png)

<a id="timeoutkeepalive"></a>

## <a name="timeout-and-keepalive-settings"></a>Configurações de tempo limite e KeepAlive

Os `ConnectionTimeout` valores padrão, `DisconnectTimeout` e `KeepAlive` são apropriados para a maioria dos cenários, mas podem ser alterados se o seu ambiente tiver necessidades especiais. Por exemplo, se o seu ambiente de rede fechar conexões ociosas por 5 segundos, talvez seja necessário diminuir o valor de KeepAlive.

<a id="connectiontimeout"></a>

### <a name="connectiontimeout"></a>ConnectionTimeout

Essa configuração representa a quantidade de tempo para deixar uma conexão de transporte aberta e aguardar uma resposta antes de fechá-la e abrir uma nova conexão. O valor padrão é 110 segundos.

Essa configuração se aplica somente quando a funcionalidade KeepAlive está desabilitada, que normalmente se aplica somente ao transporte de sondagem longa. O diagrama a seguir ilustra o efeito dessa configuração em uma conexão de transporte de sondagem longa.

![Conexão de transporte de sondagem longa](handling-connection-lifetime-events/_static/image7.png)

<a id="disconnecttimeout"></a>

### <a name="disconnecttimeout"></a>DisconnectTimeout

Essa configuração representa a quantidade de tempo de espera após a perda de uma conexão de transporte antes de gerar o `Disconnected` evento. O valor padrão é 30 segundos. Quando você define `DisconnectTimeout` , `KeepAlive` é definido automaticamente como 1/3 do `DisconnectTimeout` valor.

<a id="keepalive"></a>

### <a name="keepalive"></a>KeepAlive

Essa configuração representa a quantidade de tempo de espera antes de enviar um pacote KeepAlive por uma conexão ociosa. O valor padrão é 10 segundos. Esse valor não deve ser maior que 1/3 do `DisconnectTimeout` valor.

Se você quiser definir `DisconnectTimeout` e `KeepAlive` , defina `KeepAlive` After `DisconnectTimeout` . Caso contrário, sua `KeepAlive` configuração será substituída quando o for `DisconnectTimeout` automaticamente definido `KeepAlive` como 1/3 do valor de tempo limite.

Se você quiser desabilitar a funcionalidade KeepAlive, defina `KeepAlive` como NULL. A funcionalidade KeepAlive é automaticamente desabilitada para o transporte de sondagem longa.

<a id="changetimeout"></a>

### <a name="how-to-change-timeout-and-keepalive-settings"></a>Como alterar o tempo limite e as configurações de KeepAlive

Para alterar os valores padrão para essas configurações, defina-as em `Application_Start` no arquivo *global. asax* , conforme mostrado no exemplo a seguir. Os valores mostrados no código de exemplo são os mesmos valores padrão.

[!code-csharp[Main](handling-connection-lifetime-events/samples/sample1.cs)]

<a id="notifydisconnect"></a>

## <a name="how-to-notify-the-user-about-disconnections"></a>Como notificar o usuário sobre desconexões

Em alguns aplicativos, talvez você queira exibir uma mensagem para o usuário quando houver problemas de conectividade. Você tem várias opções para saber como e quando fazer isso. Os exemplos de código a seguir são para um cliente JavaScript usando o proxy gerado.

- Manipule o `connectionSlow` evento para exibir uma mensagem assim que o signalr estiver ciente dos problemas de conexão, antes de entrar no modo de reconexão.

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample2.js)]
- Manipule o `reconnecting` evento para exibir uma mensagem quando o signalr estiver ciente de uma desconexão e entrar no modo de reconexão.

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample3.js)]
- Manipule o `disconnected` evento para exibir uma mensagem quando uma tentativa de reconexão atingir o tempo limite. Nesse cenário, a única maneira de restabelecer novamente uma conexão com o servidor é reiniciar a conexão do Signalr chamando o `Start` método, que criará uma nova ID de conexão. O exemplo de código a seguir usa um sinalizador para garantir que você emita a notificação somente após um tempo limite de reconexão, não após uma extremidade normal para a conexão do Signalr causada pela chamada do `Stop` método.

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample4.js)]

<a id="continuousreconnect"></a>

## <a name="how-to-continuously-reconnect"></a>Como reconectar continuamente

Em alguns aplicativos, você pode querer restabelecer automaticamente uma conexão depois que ela for perdida e a tentativa de reconexão expirar. Para fazer isso, você pode chamar o `Start` método do `Closed` manipulador de eventos ( `disconnected` manipulador de eventos em clientes JavaScript). Talvez você queira aguardar um período de tempo antes de chamar `Start` para evitar isso com muita frequência quando o servidor ou a conexão física estiverem indisponíveis. O exemplo de código a seguir é para um cliente JavaScript usando o proxy gerado.

[!code-javascript[Main](handling-connection-lifetime-events/samples/sample5.js)]

Um problema potencial a ser considerado em clientes móveis é que as tentativas de reconexão contínua quando o servidor ou a conexão física não estão disponíveis podem causar esgotamento de bateria desnecessária.

<a id="disconnectclientfromserver"></a>

## <a name="how-to-disconnect-a-client-in-server-code"></a>Como desconectar um cliente no código do servidor

O signalr versão 2 não tem uma API de servidor interna para desconectar clientes. Há [planos para adicionar essa funcionalidade no futuro](https://github.com/SignalR/SignalR/issues/2101). Na versão atual do Signalr, a maneira mais simples de desconectar um cliente do servidor é implementar um método Disconnect no cliente e chamar esse método a partir do servidor. O exemplo de código a seguir mostra um método de desconexão para um cliente JavaScript usando o proxy gerado.

[!code-javascript[Main](handling-connection-lifetime-events/samples/sample6.js)]

> [!WARNING]
> Segurança – nem esse método para desconectar clientes nem a API interna proposta tratará do cenário de clientes invadidos que estão executando código mal-intencionado, já que os clientes podem se reconectar ou o código invadido pode remover o `stopClient` método ou alterar o que ele faz. O local apropriado para implementar a proteção DOS (negação de serviço) com estado não está na estrutura ou na camada de servidor, mas sim na infraestrutura de front-end.

<a id="detectingreasonfordisconnection"></a>
## <a name="detecting-the-reason-for-a-disconnection"></a>Detectando o motivo de uma desconexão

O signalr 2,1 adiciona uma sobrecarga ao evento de servidor `OnDisconnect` que indica se o cliente foi desconectado deliberadamente, em vez de atingir o tempo limite. O `StopCalled` parâmetro será true se o cliente fechou explicitamente a conexão. No JavaScript, se um erro de servidor levou o cliente a ser desconectado, as informações de erro serão passadas para o cliente como `$.connection.hub.lastError` .

**Código do servidor C#: `stopCalled` parâmetro**

[!code-csharp[Main](handling-connection-lifetime-events/samples/sample7.cs?highlight=1,3)]

**Código de cliente JavaScript: acessando `lastError` no `disconnect` evento.**

[!code-javascript[Main](handling-connection-lifetime-events/samples/sample8.js?highlight=2-3)]
