---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry
title: Monitoramento e telemetria (criando Real-World aplicativos de nuvem com o Azure) | Microsoft Docs
author: MikeWasson
description: A criação de aplicativos de nuvem do mundo real com o livro eletrônico do Azure é baseada em uma apresentação desenvolvida por Scott Guthrie. Ele explica 13 padrões e práticas que podem...
ms.author: riande
ms.date: 07/09/2015
ms.assetid: 7e986ab5-6615-4638-add7-4614ce7b51db
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry
msc.type: authoredcontent
ms.openlocfilehash: f61810ea7b486b2fa0bbb234edea7541eedde835
ms.sourcegitcommit: c62ec20b453cee3249eb894ecd75013b57d078f0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/21/2020
ms.locfileid: "92345569"
---
# <a name="monitoring-and-telemetry-building-real-world-cloud-apps-with-azure"></a>Monitoramento e telemetria (criando Real-World aplicativos de nuvem com o Azure)

por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra)

[Baixar o projeto de ti](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [baixar o livro eletrônico](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> A **criação de aplicativos de nuvem do mundo real com** o livro eletrônico do Azure é baseada em uma apresentação desenvolvida por Scott Guthrie. Ele explica 13 padrões e práticas que podem ajudá-lo a desenvolver com sucesso aplicativos Web para a nuvem. Para obter informações sobre o livro eletrônico, consulte [o primeiro capítulo](introduction.md).

Muitas pessoas contam com os clientes para informá-los quando seu aplicativo está inativo. Isso não é realmente uma prática recomendada em qualquer lugar e, especialmente, não na nuvem. Não há nenhuma garantia de notificação rápida e, quando você for notificado, geralmente obterá dados mínimos ou enganosos sobre o que aconteceu. Com bons sistemas de telemetria e registro em log, você pode estar atento ao que está acontecendo com seu aplicativo, e quando algo dá errado, você descobre imediatamente e tem informações úteis de solução de problemas com as quais trabalhar.

## <a name="buy-or-rent-a-telemetry-solution"></a>Comprar ou alugar uma solução de telemetria

> [!NOTE]
> Este artigo foi escrito antes da liberação de [Application insights](/azure/application-insights/app-insights-overview) . Application Insights é a abordagem preferida para soluções de telemetria no Azure. Consulte [configurar Application insights para o site do ASP.net](/azure/application-insights/app-insights-asp-net) para obter mais informações.

Uma das coisas que é excelente no ambiente de nuvem é que é realmente fácil comprar ou alugar sua forma de vitória. Telemetria é um exemplo. Sem muita esforço, você pode obter um sistema de telemetria realmente bom em funcionamento, de forma muito econômica. Há um monte de grandes parceiros que se integram ao Azure e alguns deles têm camadas gratuitas, para que você possa obter a telemetria básica para nada. Aqui estão apenas alguns dos disponíveis no Azure:

- [New Relic](http://newrelic.com/)
- [AppDynamics](http://www.appdynamics.com/)
- [Dynatrace](https://datamarket.azure.com/application/b4011de2-1212-4375-9211-e882766121ff)

[O Microsoft System Center](http://www.petri.co.il/microsoft-system-center-introduction.htm#) também inclui recursos de monitoramento.

Vamos percorrer rapidamente a configuração de New Relic para mostrar como é fácil usar um sistema de telemetria.

No portal de gerenciamento do Azure, Inscreva-se no serviço. Clique em **novo**e em **armazenar**. A caixa de diálogo **escolher um complemento** é exibida. Role para baixo e clique em **novo Relic**.

![Escolher um complemento](monitoring-and-telemetry/_static/image1.png)

Clique na seta para a direita e escolha a camada de serviço desejada. Para esta demonstração, usaremos a camada gratuita.

![Personalizar complemento](monitoring-and-telemetry/_static/image2.png)

Clique na seta para a direita, confirme a "compra" e o novo Relic agora aparece como um complemento no Portal.

![Examinar compra](monitoring-and-telemetry/_static/image3.png)

![Novo complemento do Relic no portal de gerenciamento](monitoring-and-telemetry/_static/image4.png)

Clique em **informações de conexão**e copie a chave de licença.

![Informações de conexão](monitoring-and-telemetry/_static/image5.png)

Vá para a guia **Configurar** para seu aplicativo Web no portal, defina **monitoramento de desempenho** como **complemento**e defina a lista suspensa **escolher complemento** para **novo Relic**. Em seguida, clique em **Salvar**.

![Nova Relic na guia Configurar](monitoring-and-telemetry/_static/image6.png)

No Visual Studio, instale o novo pacote NuGet do Relic em seu aplicativo.

![Análise do desenvolvedor na guia Configurar](monitoring-and-telemetry/_static/image7.png)

Implante o aplicativo no Azure e comece a usá-lo. Crie algumas tarefas Fix it para fornecer alguma atividade para o novo Relic monitorar.

Em seguida, volte para a página **novo Relic** na guia **Complementos** do portal e clique em **gerenciar**. O portal envia para o novo portal de gerenciamento do Relic, usando o logon único para autenticação, para que você não precise inserir suas credenciais novamente. A página Visão geral apresenta uma variedade de estatísticas de desempenho. (Clique na imagem para ver o tamanho total da página de visão geral.)

[![Guia de monitoramento do New Relic](monitoring-and-telemetry/_static/image9.png)](monitoring-and-telemetry/_static/image8.png)

Aqui estão apenas algumas das estatísticas que você pode ver:

- Tempo médio de resposta em diferentes momentos do dia.

    ![Tempo de resposta](monitoring-and-telemetry/_static/image10.png)
- Taxas de taxa de transferência (em solicitações por minuto) em diferentes momentos do dia.

    ![Taxa de transferência](monitoring-and-telemetry/_static/image11.png)
- Tempo de CPU do servidor gasto lidando com solicitações HTTP diferentes.

    ![Tempos de transação da Web](monitoring-and-telemetry/_static/image12.png)
- Tempo de CPU gasto em diferentes partes do código do aplicativo:

    ![Detalhes do rastreamento](monitoring-and-telemetry/_static/image13.png)
- Estatísticas históricas de desempenho.

    ![Desempenho histórico](monitoring-and-telemetry/_static/image14.png)
- Chamadas para serviços externos, como o serviço BLOB e estatísticas sobre a confiabilidade e a resposta do serviço.

    ![Serviços externos](monitoring-and-telemetry/_static/image15.png)

    ![Serviços externos](monitoring-and-telemetry/_static/image16.png)

    ![Serviço externo](monitoring-and-telemetry/_static/image17.png)
- As informações sobre o local no mundo ou onde vieram o tráfego do aplicativo Web dos EUA.

    ![painel Geografia do app&#39;s selecionado](monitoring-and-telemetry/_static/image18.png)

Você também pode configurar relatórios e eventos. Por exemplo, você pode dizer sempre que você começar a ver erros, enviar um email para alertar a equipe de suporte para o problema.

![Relatórios](monitoring-and-telemetry/_static/image19.png)

O New Relic é apenas um exemplo de um sistema de telemetria; Você também pode obter tudo isso de outros serviços. A beleza da nuvem é que, sem a necessidade de escrever nenhum código, e pelo mínimo ou nenhuma despesa, você pode obter muito mais informações sobre como seu aplicativo está sendo usado e o que os clientes estão realmente enfrentando.

<a id="log"></a>
## <a name="log-for-insight"></a>Registre-se para obter informações

Um pacote de telemetria é uma boa primeira etapa, mas você ainda precisa instrumentar seu próprio código. O serviço de telemetria informa quando há um problema e informa o que os clientes estão experimentando, mas ele pode não fornecer muita percepção sobre o que está acontecendo em seu código.

Você não quer ter de se tornar remoto em um servidor de produção para ver o que seu aplicativo está fazendo. Isso pode ser prático quando você tem um servidor, mas e quando você escala para centenas de servidores e você não sabe quais deles precisa para serem remotos? O registro em log deve fornecer informações suficientes que você nunca precise conectar remotamente aos servidores de produção para analisar e depurar problemas. Você deve estar registrando em log informações suficientes para poder isolar os problemas exclusivamente por meio dos logs.

### <a name="log-in-production"></a>Fazer logon na produção

Muitas pessoas ativam o rastreamento em produção somente quando há um problema e desejam Depurar. Isso pode apresentar um atraso significativo entre o tempo que você está ciente de um problema e o tempo que você obtém informações úteis de solução de problemas sobre ele. E as informações obtidas podem não ser úteis para erros intermitentes.

O que recomendamos no ambiente de nuvem em que o armazenamento é barato é que você sempre deixa o logon em produção. Dessa forma, quando ocorrem erros, você já os tem registrados e tem dados históricos que podem ajudá-lo a analisar os problemas que desenvolvem ao longo do tempo ou acontecem regularmente em momentos diferentes. É possível automatizar um processo de limpeza para excluir logs antigos, mas você pode achar mais caro configurar esse processo do que manter os logs.

A despesa adicional de registro em log é trivial em comparação com a quantidade de tempo de solução de problemas e dinheiro que você pode economizar, tendo todas as informações de que você precisa já disponíveis quando algo der errado. Em seguida, quando alguém informa a você um erro aleatório durante cerca de 8:00 na noite passada, mas não se lembra do erro, você pode descobrir o problema.

Por menos de $4 por mês, você pode manter 50 gigabytes de logs à mão, e o impacto no desempenho do registro em log é trivial, desde que você mantenha uma coisa em mente – para evitar gargalos de desempenho, verifique se sua biblioteca de registro em log é assíncrona.

### <a name="differentiate-logs-that-inform-from-logs-that-require-action"></a>Diferenciar logs que informam os logs que exigem ação

Os logs devem informar (desejo que você saiba algo) ou agir (quero fazer algo). Tenha cuidado para gravar apenas os logs do ACT em busca de problemas que exigem uma pessoa ou um processo automatizado de forma genuína. Muitos logs do ACT criarão ruído, exigindo muito trabalho para buscar por tudo para encontrar problemas autênticos. E se os logs do ACT dispararem automaticamente alguma ação, como enviar email para a equipe de suporte, Evite deixar que milhares dessas ações sejam disparadas por um único problema.

No rastreamento do .NET System. Diagnostics, os logs podem receber erros, avisos, informações e nível de depuração/detalhe. Você pode diferenciar o ACT de logs de informação, reservando o nível de erro para logs do ACT e usando os níveis inferiores para logs de informação.

![Níveis de log](monitoring-and-telemetry/_static/image20.png)

### <a name="configure-logging-levels-at-run-time"></a>Configurar níveis de log em tempo de execução

Embora valha a pena ter o log Always on em produção, outra prática recomendada é implementar uma estrutura de log que permita que você ajuste em tempo de execução o nível de detalhes que você está registrando, sem reimplantar ou reiniciar o aplicativo. Por exemplo, ao usar o recurso de rastreamento no `System.Diagnostics` , você pode criar logs de erro, aviso, informações e depuração/detalhado. É recomendável que você sempre Registre logs de erros, avisos e informações em produção, e você desejará adicionar dinamicamente logs de depuração/detalhado para solução de problemas caso a caso.

Os aplicativos Web no serviço de Azure App têm suporte interno para gravar `System.Diagnostics` logs no sistema de arquivos, armazenamento de tabelas ou armazenamento de BLOBs. Você pode selecionar diferentes níveis de log para cada destino de armazenamento e pode alterar o nível de log imediatamente sem reiniciar o aplicativo. O suporte ao armazenamento de BLOBs facilita a execução de trabalhos de análise do [hdinsight](https://docs.microsoft.com/azure/hdinsight/) em seus logs de aplicativo, pois o HDInsight sabe como trabalhar com o armazenamento de BLOBs diretamente.

### <a name="log-exceptions"></a>registrar exceções em log

Não apenas coloque a *exceção. ToString ()* no seu código de registro em log. Isso deixa informações contextuais. No caso de erros SQL, ele deixa o número do erro SQL. Para todas as exceções, inclua informações de contexto, a própria exceção e as exceções internas para certificar-se de que você está fornecendo tudo o que será necessário para a solução de problemas. Por exemplo, as informações de contexto podem incluir o nome do servidor, um identificador de transação e um nome de usuário (mas não a senha ou os segredos!).

Se você confiar em cada desenvolvedor para fazer a coisa certa com o log de exceções, alguns deles não serão. Para garantir que ele seja feito da maneira certa a cada vez, crie uma manipulação de exceção diretamente na sua interface do agente: passe o próprio objeto de exceção para a classe do agente e registre os dados de exceção corretamente na classe de agente.

### <a name="log-calls-to-services"></a>Chamadas de log para serviços

É altamente recomendável que você grave um log toda vez que seu aplicativo chamar um serviço, seja para um banco de dados ou uma API REST ou qualquer serviço externo. Inclua em seus logs não apenas uma indicação de êxito ou falha, mas por quanto tempo cada solicitação demorou. No ambiente de nuvem, você geralmente verá problemas relacionados a lentos em vez de interrupções completas. Algo que normalmente leva 10 milissegundos pode começar a levar um segundo. Quando alguém diz que seu aplicativo está lento, você deseja ser capaz de examinar novas Relic ou qualquer serviço de telemetria que você tenha e validar sua experiência e, em seguida, você deseja ser capaz de examinar seus próprios logs para se aprofundar nos detalhes de por que está lento.

### <a name="use-an-ilogger-interface"></a>Usar uma interface ILogger

O que recomendamos ao criar um aplicativo de produção é criar uma interface *ILogger* simples e colocar alguns métodos nela. Isso facilita a alteração da implementação de log posteriormente e não precisa passar por todo o seu código para fazê-lo. Poderíamos usar a classe em `System.Diagnostics.Trace` todo o aplicativo Fix it, mas, em vez disso, estamos usando-a nos bastidores em uma classe de log que implementa *ILogger*e fazemos chamadas de método *ILogger* em todo o aplicativo.

Dessa forma, se você quiser tornar seu registro em log mais rico, poderá substituir [`System.Diagnostics.Trace`](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio#apptracelogs) por qualquer mecanismo de log desejado. Por exemplo, à medida que seu aplicativo cresce, você pode decidir que deseja usar um pacote de log mais abrangente, como o [NLog](http://nlog-project.org/) ou o [bloco de aplicativo de log do Enterprise Library](https://msdn.microsoft.com/library/dn440731(v=pandp.60).aspx). (O[log4net](http://logging.apache.org/log4net/) é outra estrutura de log popular, mas não faz log assíncrono.)

Um motivo possível para usar uma estrutura como o NLog é facilitar a divisão da saída de log em repositórios de dados separados de alto volume e de alto valor. Isso ajuda a armazenar com eficiência grandes volumes de dados de informação para os quais você não precisa executar consultas rápidas, ao mesmo tempo em que mantém acesso rápido aos dados do ACT.

### <a name="semantic-logging"></a>Log semântico

Para uma maneira relativamente nova de fazer registro em log que possa produzir informações de diagnóstico mais úteis, consulte a [laje (opção de bloco de aplicativo de log semântico) da Enterprise Library](http://convective.wordpress.com/2013/08/12/semantic-logging-application-block-slab/). O SLAB usa o ETW ( [rastreamento de eventos para Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) ) e o suporte a [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) no .NET 4,5 para permitir que você crie logs mais estruturados e consultáveis. Você define um método diferente para cada tipo de evento que você registra, o que permite que você personalize as informações gravadas. Por exemplo, para registrar um erro de banco de dados SQL, você pode chamar um `LogSQLDatabaseError` método. Para esse tipo de exceção, você sabe que uma informação importante é o número do erro, portanto, você pode incluir um parâmetro de número de erro na assinatura do método e registrar o número do erro como um campo separado no registro de log que você escreve. Como o número está em um campo separado, você pode obter relatórios com mais facilidade e confiança com base nos números de erro do SQL do que você poderia se estivesse apenas concatenando o número do erro em uma cadeia de caracteres de mensagem.

## <a name="logging-in-the-fix-it-app"></a>Registrando em log o aplicativo corrigir ti

### <a name="the-ilogger-interface"></a>A interface ILogger

Aqui está a interface *ILogger* no aplicativo Fix it.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample1.cs)]

Esses métodos permitem que você grave logs nos mesmos quatro níveis com suporte do *System. Diagnostics*. Os métodos TraceApi são para registrar chamadas de serviço externas com informações sobre latência. Você também pode adicionar um conjunto de métodos para o nível de depuração/detalhe.

### <a name="the-logger-implementation-of-the-ilogger-interface"></a>A implementação do agente da interface ILogger

A implementação da interface é realmente simples. Basicamente, ele apenas chama os métodos *System. Diagnostics* padrão. O trecho a seguir mostra todos os três métodos de informação e um dos outros.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample2.cs)]

### <a name="calling-the-ilogger-methods"></a>Chamando os métodos ILogger

Toda vez que o código no aplicativo corrigir ti captura uma exceção, ele chama um método *ILogger* para registrar os detalhes da exceção. E sempre que fizer uma chamada para o banco de dados, o serviço BLOB ou uma API REST, ele iniciará um cronômetro antes da chamada, interromperá o cronômetro quando o serviço retornar e registrará o tempo decorrido junto com as informações sobre êxito ou falha.

Observe que a mensagem de log inclui o nome da classe e o nome do método. É uma boa prática garantir que as mensagens de log identifiquem qual parte do código do aplicativo as escreveu.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample3.cs?highlight=6,14,20-21,25)]

Então, agora, para cada vez que o aplicativo corrigir a ti fez uma chamada para o banco de dados SQL, você pode ver a chamada, o método que o chamou e exatamente quanto tempo demorou.

![Consulta do banco de dados SQL em logs](monitoring-and-telemetry/_static/image21.png)

![](monitoring-and-telemetry/_static/image22.png)

Se você navegar pelos logs, poderá ver que o tempo que as chamadas de banco de dados assumem é variável. Essas informações podem ser úteis: como o aplicativo registra tudo isso, você pode analisar as tendências históricas de como o serviço de banco de dados está sendo executado ao longo do tempo. Por exemplo, um serviço pode ser rápido na maior parte do tempo, mas as solicitações podem falhar ou as respostas podem ficar mais lentas em determinadas horas do dia.

Você pode fazer a mesma coisa para o serviço blob – para cada vez que o aplicativo carrega um novo arquivo, há um log, e você pode ver exatamente quanto tempo demorou para carregar cada arquivo.

![Log de carregamento de BLOB](monitoring-and-telemetry/_static/image23.png)

São apenas algumas linhas extras de código para escrever sempre que você chama um serviço e, agora, sempre que alguém diz que teve um problema, você sabe exatamente qual é o problema, se era um erro ou mesmo se ele estava apenas executando lentidão. Você pode identificar a origem do problema sem ter que se conectar remotamente a um servidor ou ativar o registro em log depois que o erro ocorrer e esperar recriá-lo.

## <a name="dependency-injection-in-the-fix-it-app"></a>Injeção de dependência no aplicativo corrigir ti

Você pode imaginar como o construtor do repositório no exemplo mostrado acima Obtém a implementação da interface do agente:

[!code-csharp[Main](monitoring-and-telemetry/samples/sample4.cs?highlight=6)]

Para conectar a interface à implementação, o aplicativo usa [injeção de dependência](http://en.wikipedia.org/wiki/Dependency_injection)(di) com [AutoFac](http://autofac.org/). A DI permite que você use um objeto baseado em uma interface em muitos locais em todo o seu código e precise apenas especificar em um local a implementação que é usada quando a interface é instanciada. Isso facilita a alteração da implementação: por exemplo, talvez você queira substituir o agente de log System. Diagnostics por um agente do NLog. Ou para testes automatizados, talvez você queira substituir uma versão fictícia do agente.

O aplicativo corrigir ti usa DI em todos os repositórios e todos os controladores. Os construtores das classes do controlador obtêm uma interface *ITaskRepository* da mesma maneira que o repositório Obtém uma interface do agente:

[!code-csharp[Main](monitoring-and-telemetry/samples/sample5.cs?highlight=5)]

O aplicativo usa a biblioteca de DI AutoFac para fornecer automaticamente instâncias de *TaskRepository* e de *agente* para esses construtores.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample6.cs?highlight=8,10)]

Basicamente, esse código diz que, em qualquer lugar, um Construtor precisa de uma interface *ILogger* , passar uma instância da classe de *agente* e sempre que precisa de uma interface *IFixItTaskRepository* , passar uma instância da classe *FixItTaskRepository* .

[AutoFac](http://autofac.org/) é uma das muitas estruturas de injeção de dependência que você pode usar. Outro conhecido é o [Unity](https://blogs.msdn.com/b/agile/archive/2013/08/20/new-guide-dependency-injection-with-unity.aspx), que é recomendado e suportado pelos padrões e pelas práticas da Microsoft.

## <a name="built-in-logging-support-in-azure"></a>Suporte a log interno no Azure

O Azure dá suporte aos seguintes tipos de [log para aplicativos Web no serviço Azure app](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio):

- Rastreamento de System. Diagnostics (você pode ativar e desativar e definir níveis imediatamente sem reiniciar o site).
- Eventos do Windows.
- Logs do IIS (HTTP/FREB).

O Azure dá suporte aos seguintes tipos de [registro em log nos serviços de nuvem](https://docs.microsoft.com/azure/cloud-services/cloud-services-dotnet-diagnostics):

- Rastreamento de System. Diagnostics.
- Contadores de desempenho.
- Eventos do Windows.
- Logs do IIS (HTTP/FREB).
- Monitoramento de diretório personalizado.

O aplicativo corrigir ti usa o rastreamento de System. Diagnostics. Tudo o que você precisa fazer para habilitar o log do System. Diagnostics em um aplicativo Web é inverter um comutador no portal ou chamar a API REST. No portal, clique na guia **configuração** do seu site e role para baixo para ver a seção **Application Diagnostics** . Você pode ativar ou desativar o registro em log e selecionar o nível de log desejado. Você pode fazer com que o Azure grave os logs no sistema de arquivos ou em uma conta de armazenamento.

![Diagnóstico de aplicativo e diagnóstico de site na guia Configurar](monitoring-and-telemetry/_static/image24.png)

Depois de habilitar o registro em log no Azure, você pode ver os logs na janela de saída do Visual Studio à medida que eles são criados.

![Menu de logs de streaming](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-viewlogsmenu.png)

![Menu de logs de streaming](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-nologsyet.png)

Você também pode ter logs gravados na sua conta de armazenamento e exibi-los com qualquer ferramenta que possa acessar o serviço tabela de armazenamento do Azure, como **Gerenciador de servidores** no Visual Studio ou [Gerenciador de armazenamento do Azure](https://azure.microsoft.com/features/storage-explorer/).

![Logs em Gerenciador de Servidores](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-storagelogs.png)

## <a name="summary"></a>Resumo

É realmente simples implementar um sistema de telemetria pronto para uso, registro em log de instrumento em seu próprio código e configurar o registro em log no Azure. E, quando você tiver problemas de produção, a combinação de um sistema de telemetria e de logs personalizados ajudará você a resolver problemas rapidamente antes que eles se tornem grandes problemas para seus clientes.

No [próximo capítulo](transient-fault-handling.md) , veremos como lidar com erros transitórios para que eles não se tornem problemas de produção que você precisa investigar.

## <a name="resources"></a>Recursos

Para obter mais informações, consulte os recursos a seguir.

Documentação principalmente sobre telemetria:

- [Práticas e padrões da Microsoft – diretrizes do Azure](https://msdn.microsoft.com/library/dn568099.aspx). Consulte Diretrizes de instrumentação e telemetria, diretrizes de medição de serviço, padrão de monitoramento de ponto de extremidade de integridade e padrão de reconfiguração de tempo de execução.
- [Pinçagem de centavos na nuvem: Habilitando o novo monitoramento de desempenho de Relic nos sites do Azure](http://www.hanselman.com/blog/PennyPinchingInTheCloudEnablingNewRelicPerformanceMonitoringOnWindowsAzureWebsites.aspx).
- [Práticas recomendadas para o design de serviços de Large-Scale nos serviços de nuvem do Azure](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). White Paper por Mark Simms e Michael Thomassy. Consulte a seção telemetria e diagnóstico.
- [Desenvolvimento de última geração com o Application insights](https://msdn.microsoft.com/magazine/dn683794.aspx). Artigo da MSDN Magazine.

Documentação principalmente sobre registro em log:

- [Slab (bloco de aplicativo de log semântico)](http://convective.wordpress.com/2013/08/12/semantic-logging-application-block-slab/). Neil Mackenzie apresenta o caso de log semântico com SLAB.
- [Criação de logs estruturados e significativos com registro semântico](https://channel9.msdn.com/Events/Build/2013/3-336). Monitor O Juliano Dominguez apresenta o caso de log semântico com SLAB.
- [Log SQL do EF6 – parte 1: log simples](http://blog.oneunicorn.com/2013/05/08/ef6-sql-logging-part-1-simple-logging/). Arthur Vicker mostra como registrar consultas executadas por Entity Framework no EF 6.
- [Resiliência de conexão e interceptação de comando com o Entity Framework em um aplicativo MVC ASP.net](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md). O quarto de uma série de tutoriais de nove partes, mostra como usar o recurso de interceptação de comando do EF 6 para registrar em log os comandos do SQL enviados ao banco de dados por Entity Framework.
- [Melhorar o registro em log usando atributos de informações do chamador do C# 5,0](http://www.dotnetcurry.com/showarticle.aspx?ID=972). Como registrar facilmente o nome do método de chamada sem codificá-lo embutidamente em literais ou usar a reflexão para obtê-lo manualmente.

Documentação principalmente sobre solução de problemas:

- [Solução de problemas &amp; do Azure Blog de depuração](https://blogs.msdn.com/b/kwill/).
- [AzureTools – o utilitário de diagnóstico usado pela equipe de suporte Developer do Azure](https://blogs.msdn.com/b/kwill/archive/2013/08/26/azuretools-the-diagnostic-utility-used-by-the-windows-azure-developer-support-team.aspx?Redirected=true). Apresenta e fornece um link de download para uma ferramenta que pode ser usada em uma VM do Azure para baixar e executar uma ampla variedade de ferramentas de diagnóstico e monitoramento. Útil quando você precisa diagnosticar um problema em uma determinada VM.
- [Solucionar problemas de um aplicativo Web no serviço Azure App usando o Visual Studio](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio). Um tutorial passo a passo para a introdução ao rastreamento de System. Diagnostics e à depuração remota.

Vídeos:

- [FailSafe: criando serviços de nuvem escalonáveis e resilientes](https://channel9.msdn.com/Series/FailSafe). Série de nove partes de Ulrich Homann, Marc Mercuri e de Mark Simms. Apresenta conceitos de alto nível e princípios arquitetônicos de uma maneira muito acessível e interessante, com histórias extraídas da experiência CAT (equipe de consultoria para clientes) da Microsoft com os clientes reais. Os episódios 4 e 9 são sobre o monitoramento e a telemetria. O episódio 9 inclui uma visão geral dos serviços de monitoramento MetricsHub, AppDynamics, New Relic e PagerDuty.
- [Criando Big: lições aprendidas de clientes do Azure-parte II](https://channel9.msdn.com/Events/Build/2012/3-030). Mark Simms fala sobre o design de falhas e instrumentação de tudo. Semelhante à série FailSafe, mas apresenta detalhes mais detalhados.

Exemplo de código:

- [Conceitos básicos do serviço de nuvem no Azure](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Aplicativo de exemplo criado pela equipe de consultoria do cliente Microsoft Azure. Demonstra as práticas de telemetria e de registro em log, conforme explicado nos artigos a seguir. O exemplo implementa o log de aplicativo usando [NLog](http://nlog-project.org/). Para obter a documentação relacionada, consulte a [série de quatro artigos do TechNet wiki sobre telemetria e registro em log](https://social.technet.microsoft.com/wiki/contents/articles/17987.cloud-service-fundamentals.aspx#Telemetry_coming_soon).

> [!div class="step-by-step"]
> [Anterior](design-to-survive-failures.md) 
>  [Avançar](transient-fault-handling.md)
