---
uid: signalr/overview/performance/signalr-performance
title: Desempenho do signalr | Microsoft Docs
author: bradygaster
description: Desempenho do SignalR
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 3751f5e7-59db-4be0-a290-50abc24e5c84
msc.legacyurl: /signalr/overview/performance/signalr-performance
msc.type: authoredcontent
ms.openlocfilehash: b8a44f4c924c94cdfff1ce7630539b45fe269bbf
ms.sourcegitcommit: c62ec20b453cee3249eb894ecd75013b57d078f0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/21/2020
ms.locfileid: "92345279"
---
# <a name="signalr-performance"></a>Desempenho do SignalR

por [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Este tópico descreve como criar, medir e melhorar o desempenho em um aplicativo Signalr.
>
> ## <a name="software-versions-used-in-this-topic"></a>Versões de software usadas neste tópico
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
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

Para obter uma apresentação recente sobre o desempenho e o dimensionamento do Signalr, consulte [dimensionando a Web em tempo real com o signalr ASP.net](https://channel9.msdn.com/Events/Build/2013/3-502).

Este tópico contém as seguintes seções:

- [Considerações sobre o design](#design)
- [Ajustando o servidor de sinalização para desempenho](#tuning)
- [Solucionando problemas de desempenho](#troubleshooting)
- [Usando contadores de desempenho do Signalr](#perfcounters)
- [Usando outros contadores de desempenho](#othercounters)
- [Outros recursos](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a>Considerações sobre o design

Esta seção descreve os padrões que podem ser implementados durante o design de um aplicativo Signalr, para garantir que o desempenho não seja prejudicado pela geração de tráfego de rede desnecessário.

### <a name="throttling-message-frequency"></a>Frequência da mensagem de limitação

Mesmo em um aplicativo que envia mensagens a uma alta frequência (como um aplicativo de jogos em tempo real), a maioria dos aplicativos não precisa enviar mais de algumas mensagens por um segundo. Para reduzir a quantidade de tráfego que cada cliente gera, um loop de mensagem pode ser implementado para que as filas e envie mensagens não com mais frequência do que uma taxa fixa (ou seja, até um determinado número de mensagens serão enviadas a cada segundo, se houver mensagens nesse intervalo de tempo a serem enviadas). Para um aplicativo de exemplo que limita as mensagens a uma determinada taxa (do cliente e do servidor), consulte [tempo real de alta frequência com signalr](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).

### <a name="reducing-message-size"></a>Reduzindo o tamanho da mensagem

Você pode reduzir o tamanho de uma mensagem de sinalização reduzindo o tamanho de seus objetos serializados. No código do servidor, se você estiver enviando um objeto que contém propriedades que não precisam ser transmitidas, impeça que essas propriedades sejam serializadas usando o `JsonIgnore` atributo. Os nomes das propriedades também são armazenados na mensagem; os nomes das propriedades podem ser reduzidos usando o `JsonProperty` atributo. O exemplo de código a seguir demonstra como excluir uma propriedade de ser enviada ao cliente e como reduzir os nomes de propriedade:

**Código do servidor .NET que demonstra o atributo JsonIgnore para excluir dados de serem enviados ao cliente e o atributo Jsonproperty para reduzir o tamanho da mensagem**

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

Para manter a legibilidade/facilidade de manutenção no código do cliente, os nomes de propriedade abreviados podem ser remapeados para nomes amigáveis para os seres humanos depois que a mensagem é recebida. O exemplo de código a seguir demonstra uma possível maneira de remapear nomes reduzidos para mais longos, definindo um contrato de mensagem (mapeamento) e usando a `reMap` função para aplicar o contrato à classe de mensagem otimizada:

**Código JavaScript do lado do cliente que remapeia nomes de propriedade reduzidos a nomes legíveis para pessoas**

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

Os nomes também podem ser reduzidos em mensagens do cliente para o servidor, usando o mesmo método.

Reduzir a superfície de memória (ou seja, a quantidade de memória usada para a mensagem) do objeto de mensagem também pode melhorar o desempenho. Por exemplo, se o intervalo completo de um `int` não for necessário, um `short` ou `byte` poderá ser usado em vez disso.

Como as mensagens são armazenadas no barramento de mensagem na memória do servidor, a redução do tamanho das mensagens também pode resolver problemas de memória do servidor.

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a>Ajustando o servidor de sinalização para desempenho

As definições de configuração a seguir podem ser usadas para ajustar o servidor para melhorar o desempenho em um aplicativo Signalr. Para obter informações gerais sobre como melhorar o desempenho em um aplicativo ASP.NET, consulte [melhorando o desempenho do ASP.net](https://msdn.microsoft.com/library/ff647787.aspx).

**Definições de configuração do signalr**

- **DefaultMessageBufferSize**: por padrão, o signalr retém 1000 mensagens na memória por Hub por conexão. Se mensagens grandes estiverem sendo usadas, isso poderá criar problemas de memória que podem ser aliviados reduzindo esse valor. Essa configuração pode ser definida no `Application_Start` manipulador de eventos em um aplicativo ASP.net ou no `Configuration` método de uma classe de inicialização OWIN em um aplicativo auto-hospedado. O exemplo a seguir demonstra como reduzir esse valor para reduzir a superfície de memória do seu aplicativo a fim de reduzir a quantidade de memória do servidor usada:

    **Código do servidor .NET em Startup.cs para diminuir o tamanho do buffer de mensagens padrão**

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

**Definições de configuração do IIS**

- **Máximo de solicitações simultâneas por aplicativo**: aumentar o número de solicitações simultâneas do IIS aumentará os recursos do servidor disponíveis para atender a solicitações. O valor padrão é 5000; para aumentar essa configuração, execute os seguintes comandos em um prompt de comandos com privilégios elevados:

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]
- **ApplicationPool QueueLength**: Este é o número máximo de solicitações que Http.sys filas para o pool de aplicativos. Quando a fila estiver cheia, novas solicitações receberão uma resposta 503 "Serviço indisponível". O valor padrão é 1000.

    Reduzir o comprimento da fila para o processo de trabalho no pool de aplicativos que hospeda seu aplicativo irá conservar os recursos de memória. Para obter mais informações, consulte [Gerenciando, ajustando e configurando pools de aplicativos](https://technet.microsoft.com/library/cc745955.aspx).

**Definições de configuração do ASP.NET**

Esta seção inclui definições de configuração que podem ser definidas no `aspnet.config` arquivo. Esse arquivo é encontrado em um dos dois locais, dependendo da plataforma:

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

As configurações de ASP.NET que podem melhorar o desempenho do Signalr incluem o seguinte:

- **Máximo de solicitações simultâneas por CPU**: aumentar essa configuração pode aliviar afunilamentos de desempenho. Para aumentar essa configuração, adicione o seguinte parâmetro de configuração ao `aspnet.config` arquivo:

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- **Limite da fila de solicitações**: quando o número total de conexões exceder a `maxConcurrentRequestsPerCPU` configuração, o ASP.net começará a limitar as solicitações usando uma fila. Para aumentar o tamanho da fila, você pode aumentar a `requestQueueLimit` configuração. Para fazer isso, adicione o seguinte parâmetro de configuração ao `processModel` nó em `config/machine.config` (em vez de `aspnet.config` ):

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a>Solucionando problemas de desempenho

Esta seção descreve maneiras de encontrar afunilamentos de desempenho em seu aplicativo.

### <a name="verifying-that-websocket-is-being-used"></a>Verificando se o WebSocket está sendo usado

Embora o Signalr possa usar uma variedade de transportes para comunicação entre o cliente e o servidor, o WebSocket oferece uma vantagem de desempenho significativa e deve ser usado se o cliente e o servidor oferecerem suporte a ele. Para determinar se o cliente e o servidor atendem aos requisitos do WebSocket, consulte [transportes e fallbacks](../getting-started/introduction-to-signalr.md#transports). Para determinar qual transporte está sendo usado em seu aplicativo, você pode usar as ferramentas de desenvolvedor do navegador e examinar os logs para ver qual transporte está sendo usado para a conexão. Para obter informações sobre como usar as ferramentas de desenvolvimento do navegador no Internet Explorer e no Chrome, consulte [transportes e fallbacks](../getting-started/introduction-to-signalr.md#transports).

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a>Usando contadores de desempenho do Signalr

Esta seção descreve como habilitar e usar contadores de desempenho do Signalr, encontrados no `Microsoft.AspNet.SignalR.Utils` pacote.

### <a name="installing-signalrexe"></a>Instalando o signalr.exe

Os contadores de desempenho podem ser adicionados ao servidor usando um utilitário chamado SignalR.exe. Para instalar esse utilitário, siga estas etapas:

1. No Visual Studio, selecione **ferramentas**  >  **Gerenciador de pacotes NuGet**  >  **gerenciar pacotes NuGet para solução**
2. Procure **signalr. utils**e selecione instalar.

    ![](signalr-performance/_static/image1.png)
3. Aceite o contrato de licença para instalar o pacote.
4. SignalR.exe será instalado no `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools` .

### <a name="installing-performance-counters-with-signalrexe"></a>Instalando contadores de desempenho com SignalR.exe

Para instalar contadores de desempenho do Signalr, execute SignalR.exe em um prompt de comando com privilégios elevados com o seguinte parâmetro:

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

Para remover contadores de desempenho do Signalr, execute SignalR.exe em um prompt de comando com privilégios elevados com o seguinte parâmetro:

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a>Contadores de desempenho do signalr

O pacote de utilitários instala os seguintes contadores de desempenho. Os contadores "total" medem o número de eventos desde a última reinicialização do pool de aplicativos ou do servidor.

**Métricas de conexão**

As métricas a seguir medem os eventos de tempo de vida da conexão que ocorrem. Para obter mais informações, consulte [noções básicas e manipulação de eventos de tempo de vida da conexão](../guide-to-the-api/handling-connection-lifetime-events.md).

- **Conexões conectadas**
- **Conexões reconectadas**
- **Conexões desconectadas**
- **Conexões atuais**

**Métricas de mensagens**

As métricas a seguir medem o tráfego de mensagens gerado pelo Signalr.

- **Total de mensagens de conexão recebidas**
- **Total de mensagens de conexão enviadas**
- **Mensagens de conexão recebidas/s**
- **Mensagens de conexão enviadas/s**

**Métricas do barramento de mensagem**

As métricas a seguir medem o tráfego por meio do barramento de mensagem do Signalr interno, a fila na qual todas as mensagens de entrada e saída do Signalr são colocadas. Uma mensagem é **publicada** quando enviada ou difundida. Um **assinante** neste contexto é uma assinatura no barramento de mensagem; Isso deve ser igual ao número de clientes mais o próprio servidor. Um **trabalho alocado** é um componente que envia dados para conexões ativas; um **trabalho ocupado** é aquele que está enviando ativamente uma mensagem.

- **Total de mensagens de barramento de mensagem recebidas**
- **Mensagens de barramento de mensagem recebidas/s**
- **Total de mensagens do barramento de mensagem publicadas**
- **Mensagens do barramento de mensagem publicadas/s**
- **Assinantes atuais do barramento de mensagem**
- **Total de assinantes do barramento de mensagem**
- **Assinantes do barramento de mensagem/s**
- **Trabalhadores alocados do barramento de mensagem**
- **Trabalhadores ocupados do barramento de mensagem**
- **Tópicos do barramento de mensagem atual**

**Métricas de erro**

As métricas a seguir medem os erros gerados pelo tráfego de mensagens do Signalr. Os erros de **resolução de Hub** ocorrem quando um método de Hub ou Hub não pode ser resolvido. Erros de **invocação de Hub** são exceções geradas ao invocar um método de Hub. Os erros de **transporte** são erros de conexão lançados durante uma solicitação ou resposta http.

- **Erros: todos os totais**
- **Erros: todos/s**
- **Erros: resolução do Hub total**
- **Erros: resolução do Hub/s**
- **Erros: total de invocação de Hub**
- **Erros: invocação de Hub/s**
- **Erros: transporte total**
- **Erros: transporte/s**

<a id="scaleout_metrics"></a>

**Métricas de dimensionamento**

As métricas a seguir medem o tráfego e os erros gerados pelo provedor de scale out. Um **fluxo** neste contexto é uma unidade de escala usada pelo provedor de scale out; Esta é uma tabela se SQL Server for usado, um tópico se o barramento de serviço for usado e uma assinatura se Redis for usado. Cada fluxo garante operações de leitura e gravação ordenadas; um único fluxo é um gargalo de escala potencial, portanto, o número de fluxos pode ser aumentado para ajudar a reduzir esse afunilamento. Se vários fluxos forem usados, o Signalr distribuirá automaticamente (fragmentos) mensagens entre esses fluxos de forma que garanta que as mensagens enviadas de qualquer conexão específica estejam em ordem.

A configuração [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx) controla o tamanho da fila de envio de expansão mantida pelo signalr. Configurá-lo como um valor maior que 0 colocará todas as mensagens em uma fila de envio para serem enviadas uma de cada vez ao backplane de mensagens configurado. Se o tamanho da fila ultrapassar o comprimento configurado, as chamadas subsequentes para enviar falharão imediatamente com uma [InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception(v=vs.118).aspx) até que o número de mensagens na fila seja menor que a configuração novamente. O enfileiramento é desabilitado por padrão porque os backplanes implementados geralmente têm seu próprio enfileiramento ou controle de fluxo em vigor. No caso do SQL Server, o pool de conexões limita efetivamente o número de envios em andamento a qualquer momento.

Por padrão, apenas um fluxo é usado para SQL Server e Redis, cinco fluxos são usados para o barramento de serviço, e o enfileiramento é desabilitado, mas essas configurações podem ser alteradas por meio da configuração em SQL Server e barramento de serviço:

**Código do servidor .NET para configurar a contagem de tabelas e o tamanho da fila para SQL Server backplane**

[!code-csharp[Main](signalr-performance/samples/sample9.cs)]

**Código do servidor .NET para configurar a contagem de tópicos e o comprimento da fila para o backplane do barramento de serviço**

[!code-csharp[Main](signalr-performance/samples/sample10.cs)]

Um fluxo de **buffer** é aquele que entrou em um estado com falha; Quando o fluxo estiver no estado com falha, todas as mensagens enviadas ao backplane falharão imediatamente até que o fluxo não esteja mais falhando. O **comprimento da fila de envio** é o número de mensagens que foram lançadas, mas ainda não enviadas.

- **Mensagens de barramento de mensagem de escalabilidade recebidas/s**
- **Total de fluxos de expansão**
- **Fluxos de expansão abertos**
- **Buffer de fluxos de expansão**
- **Total de erros de scale out**
- **Erros de scale out/s**
- **Tamanho da fila de envio de expansão**

Para saber mais sobre o que esses contadores estão medindo, confira o [dimensionamento do signalr com o barramento de serviço do Azure](scaleout-with-windows-azure-service-bus.md).

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a>Usando outros contadores de desempenho

Os contadores de desempenho a seguir também podem ser úteis para monitorar o desempenho do seu aplicativo.

**Memória**

- Memória .NET CLR \\ n º de bytes em todos os heaps (para w3wp)

**ASP.NET**

- ASP. solicitações atual
- ASP. NET\Queued
- ASP. NET\Rejected

**CPU**

- Tempo de Information\Processor do processador

**TCP/IP**

- TCPv6/conexões estabelecidas
- TCPv4/conexões estabelecidas

**Serviço Web**

- Conexões de \ da Web
- Conexões de Service\Maximum da Web

**Threading**

- Bloqueios do .NET CLR e threads \\ n º de threads lógicos atuais
- Bloqueios do .NET CLR e threads \\ n º de threads físicos atuais

<a id="otherresources"></a>

## <a name="other-resources"></a>Outros recursos

Para obter mais informações sobre o monitoramento e o ajuste do desempenho do ASP.NET, consulte os seguintes tópicos:

- [Visão geral do desempenho do ASP.NET](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)
- [Uso de thread ASP.NET no IIS 7,5, IIS 7,0 e IIS 6,0](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [&lt;&gt;elemento ApplicationPool (configurações da Web)](https://msdn.microsoft.com/library/dd560842.aspx)
