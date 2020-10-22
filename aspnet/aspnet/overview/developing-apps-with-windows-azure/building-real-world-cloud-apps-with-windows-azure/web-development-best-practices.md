---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices
title: Práticas recomendadas de desenvolvimento para a Web (criando Real-World aplicativos de nuvem com o Azure) | Microsoft Docs
author: MikeWasson
description: A criação de aplicativos de nuvem do mundo real com o livro eletrônico do Azure é baseada em uma apresentação desenvolvida por Scott Guthrie. Ele explica 13 padrões e práticas que podem...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 52d6c941-2cd9-442f-9872-2c798d6d90cd
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices
msc.type: authoredcontent
ms.openlocfilehash: dfd8a3ac2328d3f17dfbe36e68b37d181177b0f4
ms.sourcegitcommit: c62ec20b453cee3249eb894ecd75013b57d078f0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/21/2020
ms.locfileid: "92345511"
---
# <a name="web-development-best-practices-building-real-world-cloud-apps-with-azure"></a>Práticas recomendadas de desenvolvimento para a Web (criando Real-World aplicativos de nuvem com o Azure)

por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra)

[Baixar o projeto de ti](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [baixar o livro eletrônico](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> A **criação de aplicativos de nuvem do mundo real com** o livro eletrônico do Azure é baseada em uma apresentação desenvolvida por Scott Guthrie. Ele explica 13 padrões e práticas que podem ajudá-lo a desenvolver com sucesso aplicativos Web para a nuvem. Para obter informações sobre o livro eletrônico, consulte [o primeiro capítulo](introduction.md).

Os três primeiros padrões foram sobre a configuração de um processo de desenvolvimento ágil; o restante são sobre arquitetura e código. Este é um conjunto de práticas recomendadas de desenvolvimento para a Web:

- [Servidores Web sem estado](#stateless) atrás de um balanceador de carga inteligente.
- [Evite o estado da sessão](#sessionstate) (ou se você não puder evitá-la, use o cache distribuído em vez de um banco de dados).
- [Use uma CDN](#cdn) para os ativos de arquivo estático do cache de borda (imagens, scripts).
- [Use o suporte assíncrono do .NET 4.5](#async) para evitar chamadas de bloqueio.

Essas práticas são válidas para todo o desenvolvimento para a Web, não apenas para aplicativos de nuvem, mas são especialmente importantes para aplicativos de nuvem. Eles trabalham juntos para ajudá-lo a fazer uso ideal da escala altamente flexível oferecida pelo ambiente de nuvem. Se você não seguir essas práticas, terá limitações ao tentar dimensionar seu aplicativo.

<a id="stateless"></a>
## <a name="stateless-web-tier-behind-a-smart-load-balancer"></a>Camada da Web sem estado por trás de um balanceador de carga inteligente

A *camada da Web sem estado* significa que você não armazena nenhum dado de aplicativo na memória do servidor Web ou no sistema de arquivos. Manter o sem monitoração de estado da camada da Web permite que você forneça uma melhor experiência do cliente e economize dinheiro:

- Se a camada da Web for sem estado e estiver atrás de um balanceador de carga, você poderá responder rapidamente às alterações no tráfego do aplicativo, adicionando ou removendo dinamicamente servidores. No ambiente de nuvem em que você paga apenas pelos recursos do servidor enquanto realmente os usa, essa capacidade de responder às alterações na demanda pode ser traduzida em grande economia.
- Uma camada da Web sem estado é arquitetônicamente muito mais simples de escalar horizontalmente o aplicativo. Isso também permite que você responda às necessidades de dimensionamento mais rapidamente e gaste menos dinheiro no desenvolvimento e no teste no processo.
- Servidores de nuvem, como servidores locais, precisam ser corrigidos e reinicializados ocasionalmente; e se a camada da Web estiver sem estado, roteando novamente o tráfego quando um servidor ficar inoperante temporariamente não causará erros ou comportamento inesperado.

A maioria dos aplicativos do mundo real precisa armazenar o estado de uma sessão da Web; o ponto principal aqui é não armazená-lo no servidor Web. Você pode armazenar o estado de outras maneiras, como no cliente em cookies ou fora do processo do servidor no estado de sessão ASP.NET usando um provedor de cache. Você pode armazenar arquivos no [armazenamento de BLOBs do Windows Azure](unstructured-blob-storage.md) em vez do sistema de arquivos local.

Como exemplo de como é fácil dimensionar um aplicativo nos sites do Windows Azure se sua camada da Web estiver sem estado, consulte a guia **escala** para um site do Windows Azure no portal de gerenciamento:

![Guia Escala](web-development-best-practices/_static/image1.png)

Se você quiser adicionar servidores Web, basta arrastar o controle deslizante contagem de instâncias para a direita. Defina-o como 5 e clique em **salvar**e, em segundos, você terá cinco servidores Web no Windows Azure manipulando o tráfego do seu site.

![Cinco instâncias](web-development-best-practices/_static/image2.png)

Você pode definir facilmente a contagem de instâncias até 3 ou voltar para 1. Ao escalar de volta, você começa a economizar dinheiro imediatamente porque o Windows Azure cobra por minuto, e não por hora.

Você também pode informar ao Windows Azure para aumentar ou diminuir automaticamente o número de servidores Web com base no uso da CPU. No exemplo a seguir, quando o uso da CPU ficar abaixo de 60%, o número de servidores Web diminuirá para um mínimo de 2 e, se o uso da CPU estiver acima de 80%, o número de servidores Web será aumentado até um máximo de 4.

![Escala por uso da CPU](web-development-best-practices/_static/image3.png)

Ou e se você souber que seu site estará ocupado somente durante o horário de trabalho? Você pode dizer ao Windows Azure para executar vários servidores durante o dia e diminuir para um único servidor, noites e fins de semana. A série de capturas de tela a seguir mostra como configurar o site para executar um servidor fora do horário comercial e 4 servidores durante as horas de trabalho das 8:00 às 17:00.

![Dimensionar por agenda](web-development-best-practices/_static/image4.png)

![Definir horas de agendamento](web-development-best-practices/_static/image5.png)

![Agenda de dia](web-development-best-practices/_static/image6.png)

![Weeknight agenda](web-development-best-practices/_static/image7.png)

![Agendamento de fim de semana](web-development-best-practices/_static/image8.png)

E, é claro, tudo isso pode ser feito em scripts, bem como no Portal.

A capacidade de expansão do seu aplicativo é quase ilimitada no Windows Azure, desde que você evite impedimentos de adicionar ou remover VMs de servidor dinamicamente, mantendo a camada da Web sem estado.

<a id="sessionstate"></a>
## <a name="avoid-session-state"></a>Evitar estado da sessão

Geralmente, não é prático em uma aplicativo em nuvem real evitar o armazenamento de alguma forma de estado para uma sessão de usuário, mas algumas abordagens afetam o desempenho e a escalabilidade mais do que outras. Se você precisar armazenar o estado, a melhor solução será manter o estado em uma quantidade pequena e armazená-lo em cookies. Se isso não for viável, a próxima melhor solução é usar o estado de sessão ASP.NET com um provedor de [cache na memória distribuído](distributed-caching.md#sessionstate). A pior solução de um ponto de vista de escalabilidade e desempenho é usar um provedor de estado de sessão com backup em um banco de dados.

<a id="cdn"></a>
## <a name="use-a-cdn-to-cache-static-file-assets"></a>Usar uma CDN para armazenar em cache ativos de arquivo estáticos

A CDN é um acrônimo para a rede de distribuição de conteúdo. Você fornece ativos de arquivo estáticos, como imagens e arquivos de script para um provedor de CDN, e o provedor armazena em cache esses arquivos em data centers em todo o mundo para que, sempre que as pessoas acessarem seu aplicativo, obtenham resposta relativamente rápida e baixa latência para os ativos em cache. Isso acelera o tempo de carregamento geral do site e reduz a carga em seus servidores Web. CDNs são especialmente importantes se você estiver atingindo um público amplamente distribuído geograficamente.

O Windows Azure tem uma CDN e você pode usar outros CDNs em um aplicativo executado no Windows Azure ou em qualquer ambiente de hospedagem na Web.

<a id="async"></a>
## <a name="use-net-45s-async-support-to-avoid-blocking-calls"></a>Use o suporte assíncrono do .NET 4.5 para evitar chamadas de bloqueio

O .NET 4,5 aprimorou as linguagens de programação C# e VB para tornar muito mais simples a manipulação de tarefas de forma assíncrona. O benefício da programação assíncrona não é apenas para situações de processamento paralelo, como quando você deseja disparar várias chamadas de serviço Web simultaneamente. Ele também permite que seu servidor Web seja executado de forma mais eficiente e confiável sob condições de alta carga. Um servidor Web tem apenas um número limitado de threads disponíveis e, em condições de alta carga, quando todos os threads estão em uso, as solicitações de entrada precisam aguardar até que os threads sejam liberados. Se o código do seu aplicativo não tratar tarefas como consultas de banco de dados e chamadas de serviço Web de forma assíncrona, muitos threads serão desnecessariamente ligados enquanto o servidor estiver aguardando uma resposta de e/s. Isso limita a quantidade de tráfego que o servidor pode manipular sob condições de alta carga. Com a programação assíncrona, os threads que estão aguardando que um serviço Web ou banco de dados retorne os mesmos são liberados até atender a novas solicitações até que os dados sejam recebidos. Em um servidor Web ocupado, centenas ou milhares de solicitações podem ser processadas imediatamente, o que, de outra forma, estaria esperando por threads a serem liberados.

Como vimos anteriormente, é fácil diminuir o número de servidores Web que manipulam seu site da Web como é para aumentá-los. Portanto, se um servidor puder obter uma taxa de transferência maior, você não precisará de muitas delas e poderá diminuir os custos, pois precisará de menos servidores para um determinado volume de tráfego do que faria.

O suporte para o modelo de programação assíncrona do .NET 4,5 está incluído no ASP.NET 4,5 para Web Forms, MVC e API da Web; no Entity Framework 6, e na [API de armazenamento do Windows Azure](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/07/12/introducing-storage-client-library-2-1-rc-for-net-and-windows-phone-8.aspx).

### <a name="async-support-in-aspnet-45"></a>Suporte assíncrono no ASP.NET 4,5

No ASP.NET 4,5, o suporte à programação assíncrona foi adicionado não apenas à linguagem, mas também às estruturas MVC, Web Forms e API Web. Por exemplo, um método de ação do controlador MVC ASP.NET recebe dados de uma solicitação da Web e passa os dados para uma exibição que, em seguida, cria o HTML a ser enviado ao navegador. Frequentemente, o método de ação precisa obter dados de um serviço Web ou de banco de dado para exibi-los em uma página da Web ou salvar dados inseridos em uma página da Web. Nesses cenários, é fácil tornar o método de ação assíncrono: em vez de retornar um objeto *ActionResult* , você retorna *a &lt; tarefa &gt; ActionResult* e marca o método com a palavra-chave *Async* . Dentro do método, quando uma linha de código começa uma operação que envolve o tempo de espera, você o marca com a palavra-chave Await.

Aqui está um método de ação simples que chama um método de repositório para uma consulta de banco de dados:

[!code-csharp[Main](web-development-best-practices/samples/sample1.cs)]

E aqui está o mesmo método que manipula a chamada de banco de dados de forma assíncrona:

[!code-csharp[Main](web-development-best-practices/samples/sample2.cs?highlight=1,4)]

Nos bastidores, o compilador gera o código assíncrono apropriado. Quando o aplicativo faz a chamada para `FindTaskByIdAsync` , o ASP.net faz a `FindTask` solicitação e, em seguida, desenrola o thread de trabalho e o torna disponível para processar outra solicitação. Quando a `FindTask` solicitação é feita, um thread é reiniciado para continuar processando o código que vem após essa chamada. Durante o Interim entre quando a `FindTask` solicitação é iniciada e quando os dados são retornados, você tem um thread disponível para fazer um trabalho útil que, caso contrário, seria associado aguardando a resposta.

Há alguma sobrecarga para código assíncrono, mas sob condições de baixa carga, essa sobrecarga é insignificante, enquanto em condições de alta carga você pode processar solicitações que, de outra forma, seriam mantidas esperando por threads disponíveis.

É possível fazer esse tipo de programação assíncrona desde o ASP.NET 1,1, mas era difícil escrever, propenso a erros e difícil de depurar. Agora que simplificamos a codificação para ela no ASP.NET 4,5, não há nenhum motivo para não fazer mais nada.

### <a name="async-support-in-entity-framework-6"></a>Suporte assíncrono no Entity Framework 6

Como parte do suporte assíncrono no 4,5, enviamos suporte assíncrono para chamadas de serviço Web, soquetes e e/s de sistema de arquivos, mas o padrão mais comum para aplicativos Web é atingir um banco de dados, e nossas bibliotecas não dão suporte a Async. Agora Entity Framework 6 adiciona suporte assíncrono para acesso ao banco de dados.

No Entity Framework 6, todos os métodos que fazem com que uma consulta ou comando seja enviado ao banco de dados têm versões assíncronas. O exemplo aqui mostra a versão assíncrona do método *Find* .

[!code-csharp[Main](web-development-best-practices/samples/sample3.cs?highlight=8)]

E esse suporte assíncrono funciona não apenas para inserções, exclusões, atualizações e localizações simples, ele também funciona com consultas LINQ:

[!code-csharp[Main](web-development-best-practices/samples/sample4.cs?highlight=7,10)]

Há uma `Async` versão do `ToList` método porque, nesse código, é o método que faz com que uma consulta seja enviada ao banco de dados. Os `Where` `OrderByDescending` métodos e apenas configuram a consulta, enquanto o `ToListAsync` método executa a consulta e armazena a resposta na `result` variável.

## <a name="summary"></a>Resumo

Você pode implementar as práticas recomendadas de desenvolvimento na Web descritas aqui em qualquer estrutura de programação da Web e em qualquer ambiente de nuvem, mas temos ferramentas no ASP.NET e no Windows Azure para facilitar. Se você seguir esses padrões, poderá dimensionar facilmente sua camada da Web e minimizar suas despesas, pois cada servidor poderá lidar com mais tráfego.

O [próximo capítulo](single-sign-on.md) analisa como a nuvem habilita cenários de logon único.

## <a name="resources"></a>Recursos

Para obter mais informações, consulte os recursos a seguir.

Servidores Web sem estado:

- [Práticas e padrões da Microsoft – diretrizes de dimensionamento](https://msdn.microsoft.com/library/dn589774.aspx)automático.
- [Desabilitando a afinidade de instância do arr nos sites do Windows Azure](https://azure.microsoft.com/blog/2013/11/18/disabling-arrs-instance-affinity-in-windows-azure-web-sites/). Postagem de blog de Erez Benari, explica a afinidade de sessão nos sites do Windows Azure.

CDN

- [FailSafe: criando serviços de nuvem escalonáveis e resilientes](https://channel9.msdn.com/Series/FailSafe). Série de vídeos de nove partes por Ulrich Homann, Marc Mercuri e marcas Simms. Consulte a discussão sobre CDN no episódio 3 a partir de 1:34:00.
- [Padrão de Hospedagem de conteúdo estático de práticas e padrões da Microsoft](https://msdn.microsoft.com/library/dn589776.aspx)
- [Revisões da CDN](http://www.cdnreviews.com/). Visão geral de muitos CDNs.

Programação assíncrona:

- [Usando métodos assíncronos no ASP.NET MVC 4](../../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md). Tutorial de Rick Anderson.
- [Programação assíncrona com Async e Await (C# e Visual Basic)](https://msdn.microsoft.com/library/vstudio/hh191443.aspx). O MSDN white paper que explica a racionalização da programação assíncrona, como ela funciona no ASP.NET 4,5 e como escrever código para implementá-lo.
- [Entity Framework consulta assíncrona e salvar](https://msdn.microsoft.com/data/jj819165)
- [Como criar aplicativos Web ASP.NET usando Async](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2013/DEV-B337#fbid=tgkT4SR_DK7). Apresentação em vídeo de Rowan Miller. Inclui uma demonstração gráfica de como a programação assíncrona pode facilitar aumentos consideráveis na taxa de transferência do servidor Web sob condições de alta carga.
- [FailSafe: criando serviços de nuvem escalonáveis e resilientes](https://channel9.msdn.com/Series/FailSafe). Série de vídeos de nove partes por Ulrich Homann, Marc Mercuri e marcas Simms. Para discussões sobre o impacto da programação assíncrona em escalabilidade, consulte Episódio 4 e Episódio 8.
- [A mágica de usar métodos assíncronos no ASP.NET 4,5 mais uma pegadinha importante](http://www.hanselman.com/blog/TheMagicOfUsingAsynchronousMethodsInASPNET45PlusAnImportantGotcha.aspx). Postagem de blog de Scott Hanselman, principalmente sobre o uso de Async em aplicativos de Web Forms de ASP.NET.

Para obter outras práticas recomendadas de desenvolvimento na Web, consulte os seguintes recursos:

- [As melhores práticas de aplicativo de exemplo Fix it](the-fix-it-sample-application.md#bestpractices). O apêndice a esse livro eletrônico lista uma série de práticas recomendadas que foram implementadas no aplicativo corrigir ti.
- [Lista de verificação do desenvolvedor da Web](http://webdevchecklist.com/asp.net)

> [!div class="step-by-step"]
> [Anterior](continuous-integration-and-continuous-delivery.md) 
>  [Avançar](single-sign-on.md)
