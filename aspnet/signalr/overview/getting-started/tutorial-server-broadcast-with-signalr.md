---
uid: signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
title: 'Tutorial: transmissão de servidor com Signalr 2 | Microsoft Docs'
author: tdykstra
description: Este tutorial mostra como criar um aplicativo Web que usa o Signalr ASP.NET 2 para fornecer a funcionalidade de difusão do servidor.
ms.author: bradyg
ms.date: 01/02/2019
ms.topic: tutorial
ms.assetid: 1568247f-60b5-4eca-96e0-e661fbb2b273
msc.legacyurl: /signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 14924109fff8db3e537e6bc08b6dc868792ee660
ms.sourcegitcommit: c62ec20b453cee3249eb894ecd75013b57d078f0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/21/2020
ms.locfileid: "92345598"
---
# <a name="tutorial-server-broadcast-with-signalr-2"></a>Tutorial: transmissão de servidor com Signalr 2

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

Este tutorial mostra como criar um aplicativo Web que usa o Signalr ASP.NET 2 para fornecer a funcionalidade de difusão do servidor. A difusão do servidor significa que o servidor inicia as comunicações enviadas aos clientes.

O aplicativo que você criará neste tutorial simula uma cotação de ações, um cenário típico para a funcionalidade de difusão do servidor. Periodicamente, o servidor atualiza de forma aleatória os preços de ações e transmite as atualizações para todos os clientes conectados. No navegador, os números e símbolos na **alteração** e nas **%** colunas são alterados dinamicamente em resposta a notificações do servidor. Se você abrir navegadores adicionais para a mesma URL, todos eles mostrarão os mesmos dados e as mesmas alterações aos dados simultaneamente.

![Criar Web](tutorial-server-broadcast-with-signalr/_static/image1.png)

Neste tutorial, você:

> [!div class="checklist"]
> * Criar o projeto
> * Configurar o código de servidor
> * Examinar o código do servidor
> * Configurar o código do cliente
> * Examinar o código do cliente
> * Testar o aplicativo
> * Habilitar o registro em log

> [!IMPORTANT]
> Se você não quiser trabalhar com as etapas de criação do aplicativo, poderá instalar o pacote Signalr. Sample em um novo projeto de aplicativo Web ASP.NET vazio. Se você instalar o pacote NuGet sem executar as etapas neste tutorial, deverá seguir as instruções no arquivo *readme.txt* . Para executar o pacote, você precisa adicionar uma classe de inicialização OWIN que chama o `ConfigureSignalR` método no pacote instalado. Você receberá um erro se não adicionar a classe de inicialização OWIN. Consulte a seção [instalar o exemplo de StockTicker](#install-the-stockticker-sample) deste artigo.

## <a name="prerequisites"></a>Pré-requisitos

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) com a carga de trabalho **ASP.NET e desenvolvimento para a Web**.

## <a name="create-the-project"></a>Criar o projeto

Esta seção mostra como usar o Visual Studio 2017 para criar um aplicativo Web ASP.NET vazio.

1. No Visual Studio, crie um aplicativo Web ASP.NET.

    ![Criar Web](tutorial-server-broadcast-with-signalr/_static/image2.png)

1. Na janela **novo aplicativo Web ASP.net-signalr. StockTicker** , deixe a seleção **vazia** e selecione **OK**.

## <a name="set-up-the-server-code"></a>Configurar o código de servidor

Nesta seção, você configura o código que é executado no servidor.

### <a name="create-the-stock-class"></a>Criar a classe Stock

Comece criando a classe de modelo de *estoque* que você usará para armazenar e transmitir informações sobre um estoque.

1. Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto e selecione **Adicionar**  >  **classe**.

1. Nomeie a classe *Stock* e adicione-a ao projeto.

1. Substitua o código no arquivo *stock.cs* por este código:

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample1.cs)]

    As duas propriedades que você definirá quando criar ações serão `Symbol` (por exemplo, MSFT para Microsoft) e `Price` . As outras propriedades dependem de como e quando você define `Price` . Na primeira vez que você definir `Price` , o valor será propagado para `DayOpen` . Depois disso, quando você definir `Price` , o aplicativo calculará os `Change` `PercentChange` valores de propriedade e com base na diferença entre `Price` e `DayOpen` .

### <a name="create-the-stocktickerhub-and-stockticker-classes"></a>Criar as classes StockTickerHub e StockTicker

Você usará a API do Hub do Signalr para lidar com a interação de servidor para cliente. Uma `StockTickerHub` classe que deriva da classe signalr `Hub` tratará conexões de recebimento e chamadas de método de clientes. Você também precisa manter os dados de estoque e executar um `Timer` objeto. O `Timer` objeto irá disparar periodicamente atualizações de preço independentemente das conexões de cliente. Você não pode colocar essas funções em uma `Hub` classe, pois os hubs são transitórios. O aplicativo cria uma `Hub` instância de classe para cada tarefa no Hub, como conexões e chamadas do cliente para o servidor. Portanto, o mecanismo que mantém dados de estoque, atualiza os preços e transmite que as atualizações de preço têm que ser executadas em uma classe separada. Você nomeará a classe `StockTicker` .

![Transmitindo do StockTicker](tutorial-server-broadcast-with-signalr/_static/image3.png)

Você deseja que apenas uma instância da `StockTicker` classe seja executada no servidor, portanto, você precisará configurar uma referência de cada `StockTickerHub` instância para a `StockTicker` instância singleton. A `StockTicker` classe deve difundir para os clientes porque ele tem os dados de ações e dispara atualizações, mas `StockTicker` não é uma `Hub` classe. A `StockTicker` classe deve obter uma referência para o objeto de contexto de conexão do Hub signalr. Em seguida, ele pode usar o objeto de contexto de conexão do Signalr para transmitir para os clientes.

#### <a name="create-stocktickerhubcs"></a>Criar StockTickerHub.cs

1. Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto e selecione **Adicionar**  >  **novo item**.

1. Em **Adicionar novo item-signalr. StockTicker**, selecione **instalar**o  >  **Visual C#**  >  **Web**  >  **signalr** e selecione **classe de Hub do signalr (v2)**.

1. Nomeie a classe *StockTickerHub* e adicione-a ao projeto.

    Esta etapa cria o arquivo de classe *StockTickerHub.cs* . Simultaneamente, ele adiciona um conjunto de arquivos de script e referências de assembly que dão suporte a Signalr ao projeto.

1. Substitua o código no arquivo *StockTickerHub.cs* por este código:

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample2.cs)]

1. Salve o arquivo.

O aplicativo usa a classe [Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx) para definir métodos que os clientes podem chamar no servidor. Você está definindo um método: `GetAllStocks()` . Quando um cliente se conecta inicialmente ao servidor, ele chamará esse método para obter uma lista de todas as ações com seus preços atuais. O método pode ser executado de forma síncrona e retornar `IEnumerable<Stock>` porque está retornando dados da memória.

Se o método tivesse que obter os dados fazendo algo que envolvesse esperar, como uma pesquisa de banco de dado ou uma chamada de serviço Web, você especificaria `Task<IEnumerable<Stock>>` como o valor de retorno para habilitar o processamento assíncrono. Para obter mais informações, consulte [guia da API de hubs do ASP.net signalr-Server-quando executar de forma assíncrona](../guide-to-the-api/hubs-api-guide-server.md#asyncmethods).

O `HubName` atributo especifica como o aplicativo fará referência ao Hub no código JavaScript no cliente. O nome padrão no cliente se você não usar esse atributo, será uma versão camelCase do nome da classe, que nesse caso seria `stockTickerHub` .

Como você verá posteriormente ao criar a `StockTicker` classe, o aplicativo cria uma instância singleton dessa classe em sua `Instance` propriedade estática. Essa instância singleton do `StockTicker` está na memória, independentemente de quantos clientes se conectam ou se desconectam. Essa instância é o que o `GetAllStocks()` método usa para retornar informações de ações atuais.

#### <a name="create-stocktickercs"></a>Criar StockTicker.cs

1. Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto e selecione **Adicionar**  >  **classe**.

1. Nomeie a classe *StockTicker* e adicione-a ao projeto.

1. Substitua o código no arquivo *StockTicker.cs* por este código:

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample3.cs)]

Como todos os threads executarão a mesma instância do código StockTicker, a classe StockTicker deve ser thread-safe.

### <a name="examine-the-server-code"></a>Examinar o código do servidor

Se você examinar o código do servidor, ele o ajudará a entender como o aplicativo funciona.

#### <a name="storing-the-singleton-instance-in-a-static-field"></a>Armazenando a instância singleton em um campo estático

O código inicializa o `_instance` campo estático que faz o backup da `Instance` propriedade com uma instância da classe. Como o construtor é privado, é a única instância da classe que o aplicativo pode criar. O aplicativo usa a [inicialização lenta](/dotnet/framework/performance/lazy-initialization) para o `_instance` campo. Isso não é por motivos de desempenho. É para garantir que a criação da instância seja thread-safe.

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample4.cs)]

Cada vez que um cliente se conecta ao servidor, uma nova instância da classe StockTickerHub em execução em um thread separado Obtém a instância singleton StockTicker da `StockTicker.Instance` propriedade estática, como vimos anteriormente na `StockTickerHub` classe.

#### <a name="storing-stock-data-in-a-concurrentdictionary"></a>Armazenando dados de ações em um ConcurrentDictionary

O construtor inicializa a `_stocks` coleção com alguns dados de estoque de exemplo e `GetAllStocks` retorna as ações. Como vimos anteriormente, essa coleção de ações é retornada por `StockTickerHub.GetAllStocks` , que é um método de servidor na `Hub` classe que os clientes podem chamar.

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample5.cs)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample6.cs)]

A coleção de ações é definida como um tipo de [ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx) para segurança de thread. Como alternativa, você pode usar um objeto [Dictionary](https://msdn.microsoft.com/library/xfhwa508.aspx) e bloquear explicitamente o dicionário quando fizer alterações nele.

Para este aplicativo de exemplo, não há problema em armazenar dados de aplicativo na memória e perder os dados quando o aplicativo descarta a  `StockTicker` instância. Em um aplicativo real, você trabalharia com um armazenamento de dados de back-end como um banco de dado.

#### <a name="periodically-updating-stock-prices"></a>Atualizando periodicamente os preços de ações

O Construtor inicia um `Timer` objeto que chama periodicamente os métodos que atualizam os preços de ações em uma base aleatória.

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample7.cs)]

 `Timer` chamadas `UpdateStockPrices` , que passa em NULL no parâmetro State. Antes de atualizar os preços, o aplicativo assume um bloqueio no `_updateStockPricesLock` objeto. O código verifica se outro thread já está atualizando os preços e, em seguida, chama `TryUpdateStockPrice` em cada estoque na lista. O `TryUpdateStockPrice` método decide se o preço da ação deve ser alterado e o quanto alterar. Se o preço de ações for alterado, o aplicativo chamará `BroadcastStockPrice` para transmitir a alteração de preço de estoque para todos os clientes conectados.

O `_updatingStockPrices` sinalizador designado como [volátil](https://msdn.microsoft.com/library/x13ttww7.aspx) para garantir que ele seja thread-safe.

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample8.cs)]

Em um aplicativo real, o `TryUpdateStockPrice` método chamaria um serviço Web para pesquisar o preço. Nesse código, o aplicativo usa um gerador de números aleatórios para fazer alterações aleatoriamente.

#### <a name="getting-the-signalr-context-so-that-the-stockticker-class-can-broadcast-to-clients"></a>Obter o contexto do Signalr para que a classe StockTicker possa difundir para os clientes

Como as alterações de preço são originadas aqui no `StockTicker` objeto, é o objeto que precisa chamar um `updateStockPrice` método em todos os clientes conectados. Em uma `Hub` classe, você tem uma API para chamar métodos de cliente, mas `StockTicker` não deriva da `Hub` classe e não tem uma referência a nenhum `Hub` objeto. Para transmitir para clientes conectados, a `StockTicker` classe deve obter a instância de contexto do signalr para a `StockTickerHub` classe e usá-la para chamar métodos em clientes.

O código obtém uma referência ao contexto do Signalr quando ele cria a instância da classe singleton, passa essa referência para o construtor e o Construtor a coloca na `Clients` propriedade.

Há dois motivos pelos quais você deseja obter o contexto apenas uma vez: obter o contexto é uma tarefa cara e obtê-lo uma vez garante que o aplicativo preserva a ordem pretendida de mensagens enviadas aos clientes.

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample9.cs)]

Obter a `Clients` Propriedade do contexto e colocá-la na `StockTickerClient` propriedade permite que você escreva o código para chamar métodos de cliente que têm a mesma aparência em uma `Hub` classe. Por exemplo, para difundir para todos os clientes que você pode escrever `Clients.All.updateStockPrice(stock)` .

O `updateStockPrice` método que você está chamando `BroadcastStockPrice` ainda não existe. Você o adicionará posteriormente quando escrever o código que é executado no cliente. Você pode consultar `updateStockPrice` aqui porque `Clients.All` é dinâmico, o que significa que o aplicativo avaliará a expressão em tempo de execução. Quando essa chamada de método for executada, o Signalr enviará o nome do método e o valor do parâmetro para o cliente e, se o cliente tiver um método chamado `updateStockPrice` , o aplicativo chamará esse método e passará o valor do parâmetro para ele.

`Clients.All` significa enviar para todos os clientes. O signalr oferece outras opções para especificar a quais clientes ou grupos de clientes enviar. Para obter mais informações, consulte [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx).

### <a name="register-the-signalr-route"></a>Registrar a rota do Signalr

O servidor precisa saber qual URL interceptar e direcionar para o Signalr. Para fazer isso, adicione uma classe de inicialização OWIN:

1. Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto e selecione **Adicionar**  >  **novo item**.

1. Em **Adicionar novo item-signalr. StockTicker** , selecione **instalar**  >  **Visual C#**  >  **Web** e, em seguida, selecione **classe de inicialização OWIN**.

1. Nomeie a classe como *Startup* e selecione **OK**.

1. Substitua o código padrão no arquivo *Startup.cs* por este código:

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample10.cs)]

Agora você concluiu a configuração do código do servidor. Na próxima seção, você configurará o cliente.

## <a name="set-up-the-client-code"></a>Configurar o código do cliente

Nesta seção, você configura o código que é executado no cliente.

### <a name="create-the-html-page-and-javascript-file"></a>Criar a página HTML e o arquivo JavaScript

A página HTML exibirá os dados e o arquivo JavaScript organizará os dados.

#### <a name="create-stocktickerhtml"></a>Criar StockTicker.html

Primeiro, você adicionará o cliente HTML.

1. Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto e selecione **Adicionar**  >  **página HTML**.

1. Nomeie o arquivo *StockTicker* e selecione **OK**.

1. Substitua o código padrão no arquivo *StockTicker.html* por este código:

    [!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample11.html?highlight=40-43)]

    O HTML cria uma tabela com cinco colunas, uma linha de cabeçalho e uma linha de dados com uma única célula que abrange todas as cinco colunas. A linha de dados mostra "carregando..." momentaneamente quando o aplicativo é iniciado. O código JavaScript removerá essa linha e adicionará suas linhas locais com os dados de estoque recuperados do servidor.

    As marcas de script especificam:

    * O arquivo de script jQuery.

    * O arquivo de script do Signalr Core.

    * O arquivo de script dos proxies do Signalr.

    * Um arquivo de script StockTicker que você criará mais tarde.

    O aplicativo gera dinamicamente o arquivo de script de proxies do Signalr. Ele especifica a URL "/signalr/hubs" e define métodos de proxy para os métodos na classe Hub, nesse caso, para `StockTickerHub.GetAllStocks` . Se preferir, você pode gerar esse arquivo JavaScript manualmente usando os [utilitários do signalr](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/). Não se esqueça de desabilitar a criação de arquivo dinâmico na `MapHubs` chamada do método.

1. Em **Gerenciador de soluções**, expanda **scripts**.

    As bibliotecas de script para jQuery e Signalr são visíveis no projeto.

    > [!IMPORTANT]
    > O Gerenciador de pacotes instalará uma versão mais recente dos scripts do Signalr.

1. Atualize as referências de script no bloco de código para corresponder às versões dos arquivos de script no projeto.

1. Em **Gerenciador de soluções**, clique com o botão direito do mouse em *StockTicker.html*e selecione **definir como página inicial**.

#### <a name="create-stocktickerjs"></a>Criar StockTicker.js

Agora, crie o arquivo JavaScript.

1. Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto e selecione **Adicionar**  >  **arquivo JavaScript**.

1. Nomeie o arquivo *StockTicker* e selecione **OK**.

1. Adicione este código ao arquivo de *StockTicker.js* :

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample12.js)]

### <a name="examine-the-client-code"></a>Examinar o código do cliente

Se você examinar o código do cliente, ele ajudará você a aprender como o código do cliente interage com o código do servidor para fazer o aplicativo funcionar.

#### <a name="starting-the-connection"></a>Iniciando a conexão

`$.connection` refere-se aos proxies do Signalr. O código obtém uma referência ao proxy para a `StockTickerHub` classe e a coloca na `ticker` variável. O nome do proxy é o nome que foi definido pelo `HubName` atributo:

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample13.js)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample14.cs)]

Depois de definir todas as variáveis e funções, a última linha de código no arquivo Inicializa a conexão do Signalr chamando a função Signalr `start` . A `start` função é executada de forma assíncrona e retorna um [objeto do jQuery adiado](http://api.jquery.com/category/deferred-object/). Você pode chamar a função Done para especificar a função a ser chamada quando o aplicativo concluir a ação assíncrona.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample15.js)]

#### <a name="getting-all-the-stocks"></a>Obtendo todas as ações

A `init` função chama a `getAllStocks` função no servidor e usa as informações que o servidor retorna para atualizar a tabela de estoque. Observe que, por padrão, você precisa usar camelCasing no cliente, embora o nome do método seja Pascal-case no servidor. A regra camelCasing só se aplica a métodos, não a objetos. Por exemplo, você faz referência a `stock.Symbol` e `stock.Price` , não `stock.symbol` ou `stock.price` .

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample16.js)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample17.cs)]

No `init` método, o aplicativo cria HTML para uma linha de tabela para cada objeto de estoque recebido do servidor chamando as `formatStock` Propriedades de formato do `stock` objeto e, em seguida, chamando `supplant` para substituir os espaços reservados na `rowTemplate` variável pelos `stock` valores de Propriedade do objeto. O HTML resultante é então anexado à tabela stock.

> [!NOTE]
> Você chama `init` passando-o como uma `callback` função que é executada após a conclusão da `start` função assíncrona. Se você chamou `init` como uma instrução JavaScript separada após chamar `start` , a função falhará porque ela seria executada imediatamente sem esperar que a função start termine de estabelecer a conexão. Nesse caso, a `init` função tentaria chamar a `getAllStocks` função antes que o aplicativo estabeleça uma conexão de servidor.

#### <a name="getting-updated-stock-prices"></a>Obtendo preços de ações atualizados

Quando o servidor altera o preço de uma ação, ele chama o `updateStockPrice` em clientes conectados. O aplicativo adiciona a função à propriedade de cliente do `stockTicker` proxy para disponibilizá-la a chamadas do servidor.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample18.js)]

A `updateStockPrice` função formata um objeto de estoque recebido do servidor para uma linha de tabela da mesma maneira que na `init` função. Em vez de acrescentar a linha à tabela, ela localiza a linha atual da ação na tabela e substitui essa linha pela nova.

## <a name="test-the-application"></a>Testar o aplicativo

Você pode testar o aplicativo para verificar se ele está funcionando. Você verá que todas as janelas do navegador exibem a tabela de estoque ao vivo com os preços de estoque flutuando.

1. Na barra de ferramentas, ative a **depuração de script** e, em seguida, selecione o botão reproduzir para executar o aplicativo no modo de depuração.

    ![Captura de tela do usuário que está ligando o modo de depuração e selecionando reproduzir.](tutorial-server-broadcast-with-signalr/_static/image4.png)

    Uma janela do navegador será aberta exibindo a **tabela de estoque ao vivo**. A tabela Stock inicialmente mostra o "carregando..." em seguida, após um curto período de tempo, o aplicativo mostra os dados iniciais de ações e, em seguida, os preços de ações começam a ser alterados.

1. Copie a URL do navegador, abra dois outros navegadores e cole as URLs nas barras de endereço.

    A exibição de estoque inicial é a mesma do primeiro navegador e as alterações acontecem simultaneamente.

1. Feche todos os navegadores, abra um novo navegador e vá para a mesma URL.

    O objeto singleton StockTicker continuou a ser executado no servidor. A **tabela de estoque ao vivo** mostra que as ações continuaram a ser alteradas. Você não vê a tabela inicial com valores de alteração zero.

1. Feche o navegador.

## <a name="enable-logging"></a>Habilitar o registro em log

O signalr tem uma função de log interna que você pode habilitar no cliente para auxiliar na solução de problemas. Nesta seção, você habilitará o registro em log e verá exemplos que mostram como os logs informam quais dos seguintes métodos de transporte o Signalr está usando:

* [WebSockets](http://en.wikipedia.org/wiki/WebSocket), com suporte do IIS 8 e navegadores atuais.

* [Eventos enviados pelo servidor](http://en.wikipedia.org/wiki/Server-sent_events), com suporte de navegadores diferentes do Internet Explorer.

* [Quadro contínuo](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe), com suporte do Internet Explorer.

* [Sondagem longa do AJAX](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling), com suporte de todos os navegadores.

Para qualquer conexão fornecida, o Signalr escolhe o melhor método de transporte que o servidor e o cliente oferecem suporte.

1. Abra *StockTicker.js*.

1. Adicione esta linha de código realçada para habilitar o registro em log imediatamente antes do código que inicializa a conexão no final do arquivo:

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample19.js?highlight=2)]

1. Pressione **F5** para executar o projeto.

1. Abra a janela de ferramentas de desenvolvedor do navegador e selecione o console do para ver os logs. Talvez seja necessário atualizar a página para ver os logs do Signalr negociando o método de transporte para uma nova conexão.

    * Se você estiver executando o Internet Explorer 10 no Windows 8 (IIS 8), o método de transporte será **WebSockets**.

    * Se você estiver executando o Internet Explorer 10 no Windows 7 (IIS 7,5), o método de transporte será **iframe**.

    * Se você estiver executando o Firefox 19 no Windows 8 (IIS 8), o método de transporte será **WebSockets**.

        > [!TIP]
        > No Firefox, instale o suplemento do Firebug para obter uma janela de console.

    * Se você estiver executando o Firefox 19 no Windows 7 (IIS 7,5), o método de transporte será um evento **enviado pelo servidor** .

## <a name="install-the-stockticker-sample"></a>Instalar o exemplo de StockTicker

O [Microsoft. AspNet. signaler. Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) instala o aplicativo StockTicker. O pacote NuGet inclui mais recursos do que a versão simplificada que você criou do zero. Nesta seção do tutorial, você instala o pacote NuGet e examina os novos recursos e o código que os implementa.

> [!IMPORTANT]
> Se você instalar o pacote sem executar as etapas anteriores deste tutorial, você deve adicionar uma classe de inicialização OWIN ao seu projeto. Este arquivo de readme.txt para o pacote NuGet explica essa etapa.

### <a name="install-the-signalrsample-nuget-package"></a>Instalar o pacote NuGet. Sample do Signalr

1. No **Gerenciador de Soluções**, clique com o botão direito no projeto e escolha **Gerenciar Pacotes NuGet**.

1. No **Gerenciador de pacotes NuGet: signalr. StockTicker**, selecione **procurar**.

1. Em **origem do pacote**, selecione **NuGet.org**.

1. Insira *signalr. Sample* na caixa de pesquisa e selecione a instalação **Microsoft. AspNet. signalr. Sample**  >  **Install**.

1. Em **Gerenciador de soluções**, expanda a pasta *signalr. Sample* .

    A instalação do pacote Signalr. Sample criou a pasta e seu conteúdo.

1. Na pasta *signalr. Sample* , clique com o botão direito do mouse em *StockTicker.html*e selecione **definir como página inicial**.

    > [!NOTE]
    > A instalação do pacote NuGet do Signalr. Sample pode alterar a versão do jQuery que você tem em sua pasta *scripts* . O novo *StockTicker.htmarquivo l* que o pacote instala na pasta *signalr. Sample* estará em sincronia com a versão do jQuery instalada pelo pacote, mas se você quiser executar o arquivo original *StockTicker.html* novamente, talvez seja necessário atualizar a referência do jQuery na marca do script primeiro.

### <a name="run-the-application"></a>Executar o aplicativo

 A tabela que você viu no primeiro aplicativo tinha recursos úteis. O aplicativo de cotação de ações completo mostra novos recursos: uma janela de rolagem horizontal que mostra os dados de ações e as ações que mudam de cor à medida que elas crescem e caem.

1. Pressione **F5** para executar o aplicativo.

     Quando você executa o aplicativo pela primeira vez, o "mercado" é "fechado" e você vê uma tabela estática e uma janela de marcação que não está rolando.

1. Selecione **abrir mercado**.

    ![Captura de tela do marcador ao vivo.](tutorial-server-broadcast-with-signalr/_static/image5.png)

    * A caixa de Cotações de **estoque ao vivo** começa a rolar horizontalmente, e o servidor começa a transmitir periodicamente as alterações de preço de ações aleatoriamente.

    * Cada vez que o preço de uma ação é alterado, o aplicativo atualiza a **tabela de estoque ao vivo** e a **cotação de estoque ao vivo**.

    * Quando a alteração de preço de um estoque é positiva, o aplicativo mostra o estoque com um plano de fundo verde.

    * Quando a alteração é negativa, o aplicativo mostra o estoque com um plano de fundo vermelho.

1. Selecione **fechar mercado**.

    * A tabela de atualizações é interrompida.

    * O marcador interrompe a rolagem.

1. Selecione **Restaurar**.

    * Todos os dados de ações são redefinidos.

    * O aplicativo restaura o estado inicial antes do início das alterações de preço.

1. Copie a URL do navegador, abra dois outros navegadores e cole as URLs nas barras de endereço.

1. Você vê os mesmos dados atualizados dinamicamente ao mesmo tempo em cada navegador.

1. Quando você seleciona qualquer um dos controles, todos os navegadores respondem da mesma maneira ao mesmo tempo.

### <a name="live-stock-ticker-display"></a>Exibição de Cotações de estoque ao vivo

A exibição de **Cotações de estoque ao vivo** é uma lista não ordenada em um `<div>` elemento formatado em uma única linha por estilos de CSS. O aplicativo Inicializa e atualiza o marcador da mesma maneira que a tabela: substituindo espaços reservados em uma `<li>` cadeia de caracteres de modelo e adicionando dinamicamente os `<li>` elementos ao `<ul>` elemento. O aplicativo inclui a rolagem usando a `animate` função jQuery para variar a margem esquerda da lista não ordenada dentro do `<div>` .

#### <a name="signalrsample-stocktickerhtml"></a>Signalr. amostra StockTicker.html

O código HTML da bolsa de ações:

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample20.html)]

#### <a name="signalrsample-stocktickercss"></a>Signalr. Sample StockTicker. css

O código CSS da cotação de ações:

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample21.html)]

#### <a name="signalrsample-signalrstocktickerjs"></a>SignalR.StockTicker.js. Sample do signalr

O código do jQuery que faz a rolagem:

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample22.js)]

### <a name="additional-methods-on-the-server-that-the-client-can-call"></a>Métodos adicionais no servidor que o cliente pode chamar

Para adicionar flexibilidade ao aplicativo, há métodos adicionais que o aplicativo pode chamar.

#### <a name="signalrsample-stocktickerhubcs"></a>StockTickerHub.cs de amostra.

A `StockTickerHub` classe define quatro métodos adicionais que o cliente pode chamar:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample23.cs)]

O aplicativo chama `OpenMarket` , `CloseMarket` e `Reset` em resposta aos botões na parte superior da página. Eles demonstram o padrão de um cliente que dispara uma alteração no estado que é propagada imediatamente para todos os clientes. Cada um desses métodos chama um método na `StockTicker` classe que causa a alteração do estado do mercado e, em seguida, transmite o novo estado.

#### <a name="signalrsample-stocktickercs"></a>StockTicker.cs de amostra.

Na `StockTicker` classe, o aplicativo mantém o estado do mercado com uma `MarketState` propriedade que retorna um valor de `MarketState` enumeração:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample24.cs)]

Cada um dos métodos que alteram o estado do mercado faz isso dentro de um bloco de bloqueio porque a `StockTicker` classe deve ser thread-safe:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample25.cs)]

Para certificar-se de que esse código é thread-safe, o `_marketState` campo que faz o backup da `MarketState` Propriedade designada `volatile` :

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample26.cs)]

Os `BroadcastMarketStateChange` `BroadcastMarketReset` métodos e são semelhantes ao método BroadcastStockPrice que você já viu, exceto que eles chamam métodos diferentes definidos no cliente:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample27.cs)]

### <a name="additional-functions-on-the-client-that-the-server-can-call"></a>Funções adicionais no cliente que o servidor pode chamar

A `updateStockPrice` função agora lida com a exibição da tabela e da marca e ela usa `jQuery.Color` para piscar as cores vermelhas e verdes.

Novas funções no *SignalR.StockTicker.js* habilitam e desabilitam os botões com base no estado do mercado. Eles também param ou iniciam a rolagem horizontal do **marcador de estoque ao vivo** . Como muitas funções estão sendo adicionadas ao `ticker.client` , o aplicativo usa a [função de extensão jQuery](http://api.jquery.com/jQuery.extend/) para adicioná-las.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample28.js)]

### <a name="additional-client-setup-after-establishing-the-connection"></a>Configuração adicional do cliente depois de estabelecer a conexão

Depois que o cliente estabelece a conexão, ele tem algum trabalho adicional a fazer:

* Descubra se o mercado está aberto ou fechado para chamar a função apropriada `marketOpened` `marketClosed` .

* Anexe as chamadas de método de servidor aos botões.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample29.js)]

Os métodos de servidor não são conectados aos botões até que o aplicativo estabeleça a conexão. É assim que o código não pode chamar os métodos de servidor antes que eles estejam disponíveis.

## <a name="additional-resources"></a>Recursos adicionais

Neste tutorial, você aprendeu a programar um aplicativo Signalr que transmite mensagens do servidor para todos os clientes conectados. Agora você pode transmitir mensagens periodicamente e em resposta a notificações de qualquer cliente. Você pode usar o conceito de instância singleton multi-threaded para manter o estado do servidor em cenários de jogos online de vários jogadores. Para obter um exemplo, consulte o jogo emissor [com base no signalr](https://github.com/NTaylorMullen/ShootR).

Para obter tutoriais que mostrem cenários de comunicação ponto a ponto, confira [introdução com sinalizador](introduction-to-signalr.md) e [atualização em tempo real com o signalr](tutorial-high-frequency-realtime-with-signalr.md).

Para obter mais informações sobre o Signalr, consulte os seguintes recursos:

* [ASP.NET SignalR](../../index.md)
* [Projeto do signalr](http://signalr.net/)
* [GitHub e exemplos do signalr](https://github.com/SignalR/SignalR)
* [Wiki do signalr](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você:

> [!div class="checklist"]
> * Criou o projeto
> * Configurar o código de servidor
> * Examinou o código do servidor
> * Configurar o código do cliente
> * Examinou o código do cliente
> * Testado o aplicativo
> * Registro em log habilitado

Avance para o próximo artigo para aprender a criar um aplicativo Web em tempo real que usa o Signaler 2 do ASP.NET.
> [!div class="nextstepaction"]
> [Criar aplicativo Web em tempo real com Signalr](real-time-web-applications-with-signalr.md)
