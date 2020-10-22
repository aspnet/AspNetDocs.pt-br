---
uid: whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012
title: O que há de novo no ASP.NET 4,5 e no Visual Studio 2012 | Microsoft Docs
author: rick-anderson
description: Este documento descreve os novos recursos e aprimoramentos que estão sendo introduzidos no ASP.NET 4,5. Ele também descreve os aprimoramentos que estão sendo feitos para o desenvolvimento para a Web...
ms.author: riande
ms.date: 02/29/2012
ms.assetid: ba1fabb4-31a3-4ebf-8327-41a6bbba6eaf
msc.legacyurl: /whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012
msc.type: content
ms.openlocfilehash: 32fbf7c25b00f3f0796c4c3fdd38ca2a86c89199
ms.sourcegitcommit: c62ec20b453cee3249eb894ecd75013b57d078f0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/21/2020
ms.locfileid: "92345408"
---
# <a name="whats-new-in-aspnet-45-and-visual-studio-2012"></a>Novidades no ASP.NET 4.5 e no Visual Studio 2012

> Este documento descreve os novos recursos e aprimoramentos que estão sendo introduzidos no ASP.NET 4,5. Ele também descreve os aprimoramentos feitos para o desenvolvimento para a Web no Visual Studio 2012. Este documento foi publicado originalmente em 29 de fevereiro de 2012.

- [ASP.NET Core tempo de execução e estrutura](#_Toc318097372)

    - [Leitura e gravação assíncrona de solicitações e respostas HTTP](#_Toc318097373)
    - [Melhorias na manipulação de HttpRequest](#_Toc318097374)
    - [Liberação de uma resposta de forma assíncrona](#_Toc318097375)
    - [Suporte para módulos e manipuladores assíncronos baseados em *tarefa*e *Await*](#_Toc318097376)
    - [Módulos HTTP assíncronos](#_Toc318097377)
    - [Manipuladores HTTP assíncronos](#_Toc318097378)
    - [Novos recursos de validação de solicitação do ASP.NET](#_Toc318097379)
    - [Validação de solicitação adiada ("lenta")](#_Toc318097380)
    - [Suporte para solicitações não validadas](#_Toc318097381)
    - [Biblioteca AntiXSS](#_Toc318097382)
    - [Suporte para protocolo WebSockets](#_Toc318097383)
    - [Agrupamento e minificação](#_Toc318097384)
    - [Melhorias de desempenho para hospedagem na Web](#_Toc_perf)

        - [Fatores de desempenho principais](#_Toc_perf_1)
        - [Requisitos para novos recursos de desempenho](#_Toc_perf_2)
        - [Compartilhando assemblies comuns](#_Toc_perf_3)
        - [Usando a compilação JIT de vários núcleos para inicialização mais rápida](#_Toc_perf_4)
        - [Ajustando a coleta de lixo para otimizar para a memória](#_Toc_perf_5)
        - [Pré-busca de aplicativos Web](#_Toc_perf_6)
- [Web Forms ASP.NET](#_Toc318097385)

    - [Controles de dados fortemente tipados](#_Toc318097386)
    - [Associação de modelo](#_Toc318097387)

        - [Selecionando dados](#_Toc318097388)
        - [Provedores de valor](#_Toc318097389)
        - [Filtrando por valores de um controle](#_Toc318097390)
    - [Expressões de Data-Binding codificadas em HTML](#_Toc318097391)
    - [Validação não invasiva](#_Toc318097392)
    - [Atualizações do HTML5](#_Toc318097393)
- [ASP.NET MVC 4](#_Toc318097394)
- [Páginas da Web do ASP.NET 2](#_Toc318097395)
- [Versão Release Candidate do Visual Studio 2012](#_Toc318097396)

    - [Compartilhamento de projetos entre o Visual Studio 2010 e o Visual Studio 2012 Release Candidate (compatibilidade do projeto)](#project-compatibility)
    - [Alterações de configuração nos modelos de site do ASP.NET 4,5](#Configuration_Changes_In_ASPNET45_Website_Templates)
    - [Suporte nativo no IIS 7 para roteamento ASP.NET](#Native_Support_In_IIS7_For_ASPNET_Routine)
    - [Editor de HTML](#_Toc318097397)

        - [Tarefas inteligentes](#_Toc318097398)
        - [WAI-suporte a ARIA](#_Toc318097399)
        - [Novos trechos de HTML5](#_Toc318097400)
        - [Extrair para controle de usuário](#_Toc318097401)
        - [IntelliSense para nuggets de código em atributos](#_Toc318097402)
        - [Renomeação automática de marca correspondente ao renomear uma marca de abertura ou de fechamento](#_Toc318097403)
        - [Geração de manipulador de eventos](#_Toc318097404)
        - [Recuo inteligente](#_Toc318097405)
        - [Conclusão da instrução de redução automática](#_Toc318097406)
    - [Editor de JavaScript](#_Toc318097407)

        - [Estrutura de código](#_Toc318097408)
        - [Correspondência de chaves](#_Toc318097409)
        - [Ir para definição](#_Toc318097410)
        - [Suporte do ECMAScript5](#_Toc318097411)
        - [IntelliSense DOM](#_Toc318097412)
        - [Sobrecargas de assinatura VSDOC](#_Toc318097413)
        - [Referências implícitas](#_Toc318097414)
    - [Editor de CSS](#_Toc318097415)

        - [Conclusão da instrução de redução automática](#_Toc318097416)
        - [Recuo hierárquico.](#_Toc318097417)
        - [Suporte a hackers da CSS](#_Toc318097418)
        - [Esquemas específicos do fornecedor (-Moz-,-WebKit)](#_Toc318097419)
        - [Comentar e remover comentários do suporte](#_Toc318097420)
        - [Seletor de cor](#_Toc318097421)
        - [Snippets](#_Toc318097422)
        - [Regiões personalizadas](#_Toc318097423)
    - [Inspetor de Página](#_Toc318097424)
    - [Publicando](#_Toc318097425)

        - [Perfis de publicação](#_Toc318097426)
        - [Pré-compilação e mesclagem ASP.NET](#_Toc318097427)
- [IIS Express](#_Toc318097428)
- [Isenção de responsabilidade](#_Toc318097429)

<a id="_Toc318097372"></a>
## <a name="aspnet-core-runtime-and-framework"></a>ASP.NET Core tempo de execução e estrutura

<a id="_Toc318097373"></a>
### <a name="asynchronously-reading-and-writing-http-requests-and-responses"></a>Leitura e gravação assíncrona de solicitações e respostas HTTP

O ASP.NET 4 introduziu a capacidade de ler uma entidade de solicitação HTTP como um fluxo usando o método *HttpRequest. GetBufferlessInputStream* . Esse método forneceu acesso de streaming à entidade de solicitação. No entanto, ele é executado de forma síncrona, o que amarra um thread pela duração de uma solicitação.

O ASP.NET 4,5 dá suporte à capacidade de ler fluxos de forma assíncrona em uma entidade de solicitação HTTP e a capacidade de ser liberada de forma assíncrona. O ASP.NET 4,5 também permite o buffer duplo de uma entidade de solicitação HTTP, que fornece uma integração mais fácil com manipuladores HTTP downstream, como manipuladores de página. aspx e controladores MVC ASP.NET.

<a id="_Toc318097374"></a>
#### <a name="improvements-to-httprequest-handling"></a>Melhorias na manipulação de HttpRequest

A referência de fluxo retornada por ASP.NET 4,5 de *HttpRequest. GetBufferlessInputStream* dá suporte a métodos de leitura síncronos e assíncronos. O objeto *Stream* retornado de *GetBufferlessInputStream* agora implementa os métodos BeginRead e EndRead. Os métodos de *fluxo* assíncronos permitem que você leia de forma assíncrona a entidade de solicitação em partes, enquanto ASP.net libera o thread atual entre cada iteração de um loop de leitura assíncrono.

O ASP.NET 4,5 também adicionou um método complementar para ler a entidade de solicitação de forma armazenada em buffer: *HttpRequest. GetBufferedInputStream*. Essa nova sobrecarga funciona como *GetBufferlessInputStream*, dando suporte a Leituras síncronas e assíncronas. No entanto, como ele lê, o *GetBufferedInputStream* também copia os bytes de entidade em buffers internos do ASP.net para que os módulos e manipuladores downstream ainda possam acessar a entidade de solicitação. Por exemplo, se algum código upstream no pipeline já tiver lido a entidade de solicitação usando *GetBufferedInputStream*, você ainda poderá usar *HttpRequest. Form* ou *HttpRequest. files*. Isso permite executar o processamento assíncrono em uma solicitação (por exemplo, transmitir um carregamento de arquivo grande para um banco de dados), mas ainda executar páginas. aspx e controladores MVC ASP.NET posteriormente.

<a id="_Toc318097375"></a>
#### <a name="asynchronously-flushing-a-response"></a>Liberação de uma resposta de forma assíncrona

O envio de respostas para um cliente HTTP pode levar um tempo considerável quando o cliente está longe ou tem uma conexão de baixa largura de banda. Normalmente, o ASP.NET armazena em buffer os bytes de resposta conforme eles são criados por um aplicativo. Em seguida, o ASP.NET executa uma única operação de envio dos buffers acumulados no final do processamento da solicitação.

Se a resposta em buffer for grande (por exemplo, transmitir um arquivo grande para um cliente), você deverá chamar o *HttpResponse. Flush* periodicamente para enviar a saída armazenada em buffer para o cliente e manter o uso de memória sob controle. No entanto, como *flush* é uma chamada síncrona, chamar o *flush* de forma iterativa ainda consome um thread para a duração de solicitações potencialmente de longa execução.

O ASP.NET 4,5 adiciona suporte para a execução de liberações de forma assíncrona usando os métodos *BeginFlush* e *EndFlush* da classe *HttpResponse* . Usando esses métodos, você pode criar módulos assíncronos e manipuladores assíncronos que enviam incrementalmente dados para um cliente sem ligar threads do sistema operacional. Entre as chamadas *BeginFlush* e *EndFlush* , o ASP.net libera o thread atual. Isso reduz substancialmente o número total de threads ativos que são necessários para dar suporte a downloads de HTTP de longa execução.

<a id="_Toc318097376"></a>
### <a name="support-for-await-and-task---based-asynchronous-modules-and-handlers"></a>Suporte para módulos e manipuladores assíncronos baseados em *tarefa* e *Await*

O .NET Framework 4 introduziu um conceito de programação assíncrona conhecido como uma *tarefa*. As tarefas são representadas pelo tipo de *tarefa* e tipos relacionados no namespace *System. Threading. Tasks* . O .NET Framework 4,5 se baseia nele com aprimoramentos de compilador que facilitam o trabalho com objetos de *tarefa* . No .NET Framework 4,5, os compiladores dão suporte a duas novas palavras-chave: *Await* e *Async*. A palavra-chave *Await* é uma abreviação sintática para indicar que uma parte do código deve esperar de forma assíncrona em alguma parte do código. A palavra-chave *Async* representa uma dica que você pode usar para marcar métodos como métodos assíncronos baseados em tarefas.

A combinação de *Await*, *Async*e *Task* Object torna muito mais fácil escrever código assíncrono no .NET 4,5. O ASP.NET 4,5 dá suporte a essas simplificações com novas APIs que permitem que você escreva módulos HTTP assíncronos e manipuladores HTTP assíncronos usando os novos aprimoramentos do compilador.

<a id="_Toc318097377"></a>
#### <a name="asynchronous-http-modules"></a>Módulos HTTP assíncronos

Suponha que você deseja executar o trabalho assíncrono dentro de um método que retorna um objeto de *tarefa* . O exemplo de código a seguir define um método assíncrono que faz uma chamada assíncrona para baixar o Microsoft home page. Observe o uso da palavra-chave *Async* na assinatura do método e a chamada *Await* para *DownloadStringTaskAsync*.

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample1.cs)]

Isso é tudo o que você precisa escrever – o .NET Framework manipulará automaticamente a pilha de chamadas enquanto aguarda a conclusão do download, bem como a restauração automática da pilha de chamadas depois que o download for feito.

Agora suponha que você deseja usar esse método assíncrono em um módulo HTTP ASP.NET assíncrono. ASP.NET 4,5 inclui um método auxiliar (*EventHandlerTaskAsyncHelper*) e um novo tipo de delegado (*TaskEventHandler*) que você pode usar para integrar métodos assíncronos baseados em tarefas com o modelo de programação assíncrona mais antigo exposto pelo pipeline http ASP.net. Este exemplo mostra como:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample2.cs)]

<a id="_Toc318097378"></a>
#### <a name="asynchronous-http-handlers"></a>Manipuladores HTTP assíncronos

A abordagem tradicional para escrever manipuladores assíncronos no ASP.NET é implementar a interface *IHttpAsyncHandler* . ASP.NET 4,5 apresenta o tipo base assíncrono *HttpTaskAsyncHandler* do qual você pode derivar, o que torna muito mais fácil escrever manipuladores assíncronos.

O tipo *HttpTaskAsyncHandler* é abstract e exige que você substitua o método *ProcessRequestAsync* . Internamente, o ASP.NET cuida da integração da assinatura de retorno (um objeto de *tarefa* ) do *ProcessRequestAsync* com o modelo de programação assíncrono mais antigo usado pelo pipeline do ASP.net.

O exemplo a seguir mostra como você pode usar *Task* e *Await* como parte da implementação de um manipulador HTTP assíncrono:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample3.cs)]

<a id="_Toc318097379"></a>
### <a name="new-aspnet-request-validation-features"></a>Novos recursos de validação de solicitação do ASP.NET

Por padrão, o ASP.NET executa a validação de solicitação – ele examina as solicitações para procurar marcação ou script em campos, cabeçalhos, cookies e assim por diante. Se algum for detectado, o ASP.NET lançará uma exceção. Isso atua como uma primeira linha de defesa contra ataques potenciais de script entre sites.

O ASP.NET 4,5 facilita a leitura seletiva de dados de solicitação não validados. O ASP.NET 4,5 também integra a popular biblioteca AntiXSS, que era anteriormente uma biblioteca externa.

Os desenvolvedores sempre solicitaram a capacidade de desativar seletivamente a validação de solicitações para seus aplicativos. Por exemplo, se seu aplicativo for um software de fórum, talvez você queira permitir que os usuários enviem comentários e postagens de fórum formatados em HTML, mas ainda Verifique se a validação da solicitação está verificando todo o resto.

O ASP.NET 4,5 apresenta dois recursos que facilitam o trabalho seletivamente com entrada não validada: validação de solicitação ("lenta") e acesso a dados de solicitação não validados.

<a id="_Toc318097380"></a>
#### <a name="deferred-lazy-request-validation"></a>Validação de solicitação adiada ("lenta")

No ASP.NET 4,5, por padrão, todos os dados de solicitação estão sujeitos à validação de solicitação. No entanto, é possível configurar o aplicativo para adiar a validação de solicitação até que você realmente acesse os dados de solicitação. (Às vezes, isso é chamado de validação de solicitação lenta, com base em termos como carregamento lento para determinados cenários de dados.) Você pode configurar o aplicativo para usar a validação adiada no arquivo Web.config definindo o atributo *RequestValidationMode* como 4,5 no elemento *httpRUntime* , como no exemplo a seguir:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample4.xml)]

Quando o modo de validação de solicitação é definido como 4,5, a validação de solicitação é disparada somente para um valor de solicitação específico e somente quando seu código acessa esse valor. Por exemplo, se o seu código obtiver o valor de solicitação. Form [" \_ postagem do fórum"], a validação da solicitação será invocada somente para aquele elemento na coleção de formulários. Nenhum dos outros elementos na coleção de *formulários* são validados. Nas versões anteriores do ASP.NET, a validação da solicitação foi disparada para toda a coleção de solicitações quando qualquer elemento da coleção foi acessado. O novo comportamento torna mais fácil para diferentes componentes de aplicativos examinarem diferentes partes dos dados de solicitação sem disparar a validação de solicitações em outras partes.

<a id="_Toc318097381"></a>
#### <a name="support-for-unvalidated-requests"></a>Suporte para solicitações não validadas

A validação de solicitação adiada sozinha não resolve o problema de anular seletivamente a validação da solicitação. A chamada para Request. Form ["post do fórum \_ "] ainda dispara a validação de solicitação para esse valor de solicitação específico. No entanto, talvez você queira acessar esse campo sem disparar a validação porque deseja permitir a marcação nesse campo.

Para permitir isso, o ASP.NET 4,5 Agora dá suporte ao acesso não validado para solicitar dados. ASP.NET 4,5 inclui uma nova propriedade de coleção não *validada* na classe *HttpRequest* . Essa coleção fornece acesso a todos os valores comuns de dados de solicitação, como *Form*, *QueryString*, *cookies*e *URL*.

Usando o exemplo de fórum, para poder ler dados de solicitação não validados, primeiro você precisa configurar o aplicativo para usar o novo modo de validação de solicitação:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample5.xml)]

Em seguida, você pode usar a propriedade *HttpRequest. Unvalidated* para ler o valor de formulário não validado:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample6.cs)]

> [!WARNING]
> Segurança- *use dados de solicitação não validados com cuidado!* O ASP.NET 4,5 adicionou as propriedades e coleções de solicitação não validadas para facilitar o acesso a dados de solicitação não validados muito específicos. No entanto, você ainda deve executar a validação personalizada nos dados de solicitação brutos para garantir que o texto perigoso não seja renderizado aos usuários.

<a id="_Toc318097382"></a>
### <a name="antixss-library"></a>Biblioteca AntiXSS

Devido à popularidade da biblioteca do Microsoft AntiXSS, o ASP.NET 4,5 agora incorpora rotinas de codificação de núcleo da versão 4,0 da biblioteca.

As rotinas de codificação são implementadas pelo tipo *AntiXssEncoder* no novo namespace *System. Web. Security. AntiXss* . Você pode usar o tipo *AntiXssEncoder* diretamente chamando qualquer um dos métodos de codificação estáticos que são implementados no tipo. No entanto, a abordagem mais fácil para usar as novas rotinas anti-XSS é configurar um aplicativo ASP.NET para usar a classe *AntiXssEncoder* por padrão. Para fazer isso, adicione o seguinte atributo ao arquivo de Web.config:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample7.xml)]

Quando o atributo *EncoderType* é definido para usar o tipo *AntiXssEncoder* , toda a codificação de saída no ASP.NET usa automaticamente as novas rotinas de codificação.

Essas são as partes da biblioteca AntiXSS externa que foram incorporadas ao ASP.NET 4,5:

- *HtmlEncode*, *HtmlFormUrlEncode*e *HtmlAttributeEncode*
- *XmlAttributeEncode* e *xmlcodifique*
- *UrlEncode* e *UrlPathEncode* (novo)
- *CssEncode*

<a id="_Toc318097383"></a>
### <a name="support-for-websockets-protocol"></a>Suporte para protocolo WebSockets

O protocolo WebSockets é um protocolo de rede baseado em padrões que define como estabelecer comunicações bidirecionais seguras e em tempo real entre um cliente e um servidor sobre HTTP. A Microsoft trabalhou com os órgãos de padrões IETF e W3C para ajudar a definir o protocolo. O protocolo WebSockets tem suporte de qualquer cliente (não apenas navegadores), com recursos substanciais de investindo da Microsoft que dão suporte ao protocolo WebSockets nos sistemas operacionais cliente e móvel.

O protocolo WebSockets torna muito mais fácil criar transferências de dados de execução longa entre um cliente e um servidor. Por exemplo, escrever um aplicativo de chat é muito mais fácil porque você pode estabelecer uma conexão de longa execução real entre um cliente e um servidor. Você não precisa recorrer a soluções alternativas como sondagem periódica ou sondagem longa HTTP para simular o comportamento de um soquete.

O ASP.NET 4,5 e o IIS 8 incluem suporte a WebSockets de baixo nível, permitindo que os desenvolvedores de ASP.NET usem APIs gerenciadas para leitura assíncrona e gravação de dados de cadeia de caracteres e binários em um objeto WebSocket. Para o ASP.NET 4,5, há um novo namespace *System. Web. WebSockets* que contém tipos para trabalhar com o protocolo WebSockets.

Um cliente de navegador estabelece uma conexão WebSockets criando um objeto *WebSocket* dom que aponta para uma URL em um aplicativo ASP.net, como no exemplo a seguir:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample8.cs)]

Você pode criar pontos de extremidade de WebSockets no ASP.NET usando qualquer tipo de módulo ou manipulador. No exemplo anterior, foi usado um arquivo. ashx, porque os arquivos. ashx são uma maneira rápida de criar um manipulador.

De acordo com o protocolo WebSockets, um aplicativo ASP.NET aceita a solicitação WebSockets de um cliente indicando que a solicitação deve ser atualizada de uma solicitação HTTP GET para uma solicitação de WebSockets. Aqui está um exemplo:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample9.cs)]

O método *AcceptWebSocketRequest* aceita um delegado function porque o ASP.net desenrola a solicitação HTTP atual e, em seguida, transfere o controle para o delegado function. Conceitualmente, essa abordagem é semelhante a como você usa *System. Threading. thread*, em que você define um delegado de início de thread no qual o trabalho em segundo plano é executado.

Depois que o ASP.NET e o cliente tiverem concluído com êxito um handshake do WebSockets, o ASP.NET chamará seu delegado e o aplicativo WebSockets começará a ser executado. O exemplo de código a seguir mostra um aplicativo Echo simples que usa o suporte ao WebSockets interno no ASP.NET:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample10.cs)]

O suporte no .NET 4,5 para a palavra-chave *Await* e operações assíncronas baseadas em tarefas é uma opção natural para escrever aplicativos WebSockets. O exemplo de código mostra que uma solicitação WebSockets é executada completamente de forma assíncrona dentro de ASP.NET. O aplicativo aguarda assincronamente que uma mensagem seja enviada de um cliente chamando o *soquete de espera. ReceiveAsync*. Da mesma forma, você pode enviar uma mensagem assíncrona para um cliente chamando o *soquete de espera. SendAsync*.

No navegador, um aplicativo recebe mensagens WebSockets por meio de uma função *onMessage* . Para enviar uma mensagem de um navegador, chame o método *Send* do tipo dom *WebSocket* , conforme mostrado neste exemplo:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample11.cs)]

No futuro, poderemos lançar atualizações para essa funcionalidade que abstrairam parte da codificação de nível inferior necessária nesta versão para aplicativos WebSockets.

<a id="_Toc318097384"></a>
### <a name="bundling-and-minification"></a>Agrupamento e minificação

O agrupamento permite que você combine arquivos JavaScript e CSS individuais em um pacote que pode ser tratado como um único arquivo. O minificação condensa arquivos JavaScript e CSS removendo espaço em branco e outros caracteres que não são necessários. Esses recursos funcionam com Web Forms, ASP.NET MVC e páginas da Web.

Os grupos são criados usando a classe Bundle ou uma de suas classes filhas, ScriptBundle e StyleBundle. Depois de configurar uma instância de um pacote, o pacote é disponibilizado para solicitações de entrada simplesmente adicionando-o a uma instância global de Bundlecollection. Nos modelos padrão, a configuração do pacote é executada em um arquivo BundleConfig. Essa configuração padrão cria pacotes para todos os scripts principais e arquivos CSS usados pelos modelos.

Os grupos são referenciados de dentro de exibições usando um dos dois métodos auxiliares possíveis. Para dar suporte à renderização de marcação diferente para um pacote quando no modo de depuração vs. versão, as classes ScriptBundle e StyleBundle têm o método auxiliar, render. Quando estiver no modo de depuração, render gerará marcação para cada recurso no grupo. Quando no modo de versão, render gerará um único elemento de marcação para todo o grupo. Alternar entre o modo de depuração e de versão pode ser feito modificando o atributo debug do elemento Compilation em web.config, conforme mostrado abaixo:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample12.xml)]

Além disso, a habilitação ou desabilitação da otimização pode ser definida diretamente por meio da propriedade Bundletable. EnableOptimizations.

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample13.cs)]

Quando os arquivos são agrupados, eles são primeiro classificados em ordem alfabética (a maneira como são exibidos no **Gerenciador de soluções**). Em seguida, eles são organizados de forma que as bibliotecas conhecidas e suas extensões personalizadas (como jQuery, MooTools e dojo) sejam carregadas primeiro. Por exemplo, a ordem final para o agrupamento da pasta scripts, conforme mostrado acima, será:

1. jquery-1.6.2.js
2. jquery-ui.js
3. jquery.tools.js
4. a.js

Os arquivos CSS também são classificados em ordem alfabética e, em seguida, reorganizados para que Reset. css e Normalize. css sejam apresentados antes de qualquer outro arquivo. A classificação final do agrupamento da pasta de estilos mostrada acima será a seguinte:

1. redefinir. css
2. Content. css
3. Forms. css
4. Globals. css
5. menu. css
6. Styles. css

<a id="_Toc_perf"></a>
### <a name="performance-improvements-for-web-hosting"></a>Melhorias de desempenho para hospedagem na Web

O .NET Framework 4,5 e o Windows 8 apresentam recursos que podem ajudá-lo a obter um aumento significativo no desempenho para cargas de trabalho de servidor Web. Isso inclui uma redução (até 35%) no tempo de inicialização e na superfície de memória de sites de hospedagem na Web que usam ASP.NET.

<a id="_Toc_perf_1"></a>
#### <a name="key-performance-factors"></a>Fatores de desempenho principais

O ideal é que todos os sites estejam ativos e na memória para garantir uma resposta rápida para a próxima solicitação, sempre que ele vier. Os fatores que podem afetar a capacidade de resposta do site incluem:

- O tempo que leva para um site reiniciar depois que um pool de aplicativos é reciclado. Esse é o tempo necessário para iniciar um processo do servidor Web para o site quando os assemblies do site não estiverem mais na memória. (Os assemblies da plataforma ainda estão na memória, pois eles são usados por outros sites.) Essa situação é conhecida como "site frio, inicialização a quente Framework" ou apenas "inicialização de site frio".
- A quantidade de memória que o site ocupa. Os termos para isso são "consumo de memória por site" ou "conjunto de trabalho não compartilhado".

Os novos aprimoramentos de desempenho se concentram em ambos os fatores.

<a id="_Toc_perf_2"></a>
#### <a name="requirements-for-new-performance-features"></a>Requisitos para novos recursos de desempenho

Os requisitos para os novos recursos podem ser divididos nessas categorias:

- Melhorias que são executadas no .NET Framework 4.
- Melhorias que exigem o .NET Framework 4,5, mas podem ser executadas em qualquer versão do Windows.
- Melhorias que estão disponíveis apenas com .NET Framework 4,5 em execução no Windows 8.

O desempenho aumenta com cada nível de aperfeiçoamento que você pode habilitar.

Alguns dos aprimoramentos do .NET Framework 4,5 aproveitam os recursos de desempenho mais amplos que se aplicam a outros cenários também.

<a id="_Toc_perf_3"></a>
#### <a name="sharing-common-assemblies"></a>Compartilhando assemblies comuns

**Requisito**: .NET Framework 4 e o SDK do Visual Studio 11 Developer Preview

Sites diferentes em um servidor geralmente usam os mesmos assemblies auxiliares (por exemplo, assemblies de um kit de início ou aplicativo de exemplo). Cada site tem sua própria cópia desses assemblies em seu diretório bin. Embora o código do objeto para os assemblies seja idêntico, eles são assemblies fisicamente separados, de modo que cada assembly deve ser lido separadamente durante a inicialização do site frio e mantido separadamente na memória.

A nova funcionalidade interna resolve essa ineficiência e reduz os requisitos de RAM e o tempo de carregamento. O InterNIC permite que o Windows mantenha uma única cópia de cada assembly no sistema de arquivos, e assemblies individuais nas pastas bin do site são substituídos por links simbólicos para a cópia única. Se um site individual precisar de uma versão distinta do assembly, o link simbólico será substituído pela nova versão do assembly e somente esse site será afetado.

O compartilhamento de assemblies usando links simbólicos requer uma nova ferramenta chamada ASPNET \_intern.exe, que permite criar e gerenciar o repositório de assemblies internos. Ele é fornecido como parte do SDK do Visual Studio 11 Developer Preview. (No entanto, ele funcionará em um sistema que tenha apenas o .NET Framework 4 instalado, supondo que você tenha instalado a [atualização](https://support.microsoft.com/kb/2468871)mais recente.)

Para garantir que todos os assemblies qualificados tenham sido internos, execute ASPNET \_intern.exe periodicamente (por exemplo, uma vez por semana como uma tarefa agendada). Um uso típico é o seguinte:

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample14.cmd)]

Para ver todas as opções, execute a ferramenta sem argumentos.

<a id="_Toc_perf_4"></a>
#### <a name="using-multi-core-jit-compilation-for-faster-startup"></a>Usando a compilação JIT de vários núcleos para inicialização mais rápida

**Requisito**: .NET Framework 4,5

Para uma inicialização de site frio, não apenas os assemblies precisam ser lidos do disco, mas o site deve ser compilado por JIT. Para um site complexo, isso pode adicionar atrasos significativos. Uma nova técnica de finalidade geral no .NET Framework 4,5 reduz esses atrasos distribuindo a compilação JIT entre os núcleos de processador disponíveis. Ele faz isso o máximo possível e o mais cedo possível usando as informações coletadas durante as inicializações anteriores do site. Essa funcionalidade implementada pelo método [System. Runtime. ProfileOptimization. StartProfile](https://msdn.microsoft.com/library/system.runtime.profileoptimization.startprofile(VS.110).aspx) .

JIT-a compilação usando vários núcleos é habilitada por padrão no ASP.NET, portanto, você não precisa fazer nada para aproveitar esse recurso. Se você quiser desabilitar esse recurso, faça a seguinte configuração no arquivo de Web.config:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample15.xml)]

<a id="_Toc_perf_5"></a>
#### <a name="tuning-garbage-collection-to-optimize-for-memory"></a>Ajustando a coleta de lixo para otimizar para a memória

**Requisito**: .NET Framework 4,5

Quando um site está em execução, seu uso do heap GC (coletor de lixo) pode ser um fator significativo em seu consumo de memória. Como qualquer coletor de lixo, o .NET Framework GC faz compensações entre o tempo de CPU (frequência e significância de coleções) e o consumo de memória (espaço extra usado para objetos novos, liberados ou livres). Para versões anteriores, fornecemos orientações sobre como configurar o GC para atingir o equilíbrio correto (por exemplo, consulte configuração de [hospedagem compartilhada do ASP.NET 2.0/3.5](https://www.iis.net/learn/web-hosting/web-server-for-shared-hosting/aspnet-20-35-shared-hosting-configuration)).

Para o .NET Framework 4,5, em vez de várias configurações autônomas, é disponibilizada uma configuração de configuração definida pela carga de trabalho que habilita todas as configurações de GC anteriormente recomendadas, bem como o novo ajuste que fornece desempenho adicional para o conjunto de trabalhos por site.

Para habilitar o ajuste de memória de GC, adicione a seguinte configuração ao arquivo de Windows\Microsoft.NET\Framework\v4.0.30319\aspnet.config:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample16.xml)]

(Se você estiver familiarizado com as diretrizes anteriores para alterações no aspnet.config, observe que essa configuração substitui as configurações antigas — por exemplo, não há necessidade de definir gcServer, gcConcurrent, etc. Você não precisa remover as configurações antigas.)

<a id="_Toc_perf_6"></a>
#### <a name="prefetching-for-web-applications"></a>Pré-busca de aplicativos Web

**Requisito**: .NET Framework 4,5 em execução no Windows 8

Para várias versões, o Windows incluiu uma tecnologia conhecida como a [pré-busca](http://en.wikipedia.org/wiki/Prefetcher) que reduz o custo de leitura de disco da inicialização do aplicativo. Como a inicialização a frio é um problema predominantemente para aplicativos cliente, essa tecnologia não foi incluída no Windows Server, que inclui apenas componentes que são essenciais para um servidor. A pré-busca agora está disponível na versão mais recente do Windows Server, onde pode otimizar o lançamento de sites individuais.

Para o Windows Server, a pré-busca não está habilitada por padrão. Para habilitar e configurar o pré-busca para hospedagem na Web de alta densidade, execute o seguinte conjunto de comandos na linha de comando:

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample17.cmd)]

Em seguida, para integrar a pré-busca com aplicativos ASP.NET, adicione o seguinte ao arquivo de Web.config:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample18.xml)]

<a id="_Toc318097385"></a>
## <a name="aspnet-web-forms"></a>Web Forms do ASP.NET

<a id="_Toc318097386"></a>
### <a name="strongly-typed-data-controls"></a>Controles de dados fortemente tipados

No ASP.NET 4,5, Web Forms inclui alguns aprimoramentos para trabalhar com dados. A primeira melhoria são controles de dados fortemente tipados. Para controles de Web Forms em versões anteriores do ASP.NET, você exibe um valor associado a dados usando *eval* e uma expressão de vinculação de dados:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample19.aspx)]

Para associação de dados bidirecional, você usa *associar*:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample20.aspx)]

Em tempo de execução, essas chamadas usam a reflexão para ler o valor do membro especificado e, em seguida, exibem o resultado na marcação. Essa abordagem facilita a vinculação de dados contra dados arbitrários e não formados.

No entanto, expressões de vinculação de dados como essa não dão suporte a recursos como IntelliSense para nomes de membros, navegação (como ir para definição) ou verificação de tempo de compilação para esses nomes.

Para resolver esse problema, o ASP.NET 4,5 adiciona a capacidade de declarar o tipo de dados dos dados aos quais um controle está associado. Você faz isso usando a nova propriedade *ItemType* . Quando você define essa propriedade, duas novas variáveis tipadas estão disponíveis no escopo das expressões de vinculação de dados: *Item* e *BindItem*. Como as variáveis são fortemente tipadas, você obtém todos os benefícios da experiência de desenvolvimento do Visual Studio.

Para expressões de ligação de dados bidirecionais, use a variável *BindItem* :

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample21.aspx)]

A maioria dos controles na estrutura de Web Forms ASP.NET que dão suporte à associação de dados foi atualizada para dar suporte à propriedade *ItemType* .

<a id="_Toc318097387"></a>
### <a name="model-binding"></a>Model binding

A associação de modelo estende a associação de dados em ASP.NET Web Forms controles para trabalhar com acesso a dados focado em código. Ele incorpora conceitos do controle *ObjectDataSource* e da Associação de modelo no ASP.NET MVC.

<a id="_Toc318097388"></a>
#### <a name="selecting-data"></a>Selecionando dados

Para configurar um controle de dados para usar a associação de modelo para selecionar dados, você define a propriedade *SelectMethod* do controle como o nome de um método no código da página. O controle de dados chama o método no momento apropriado no ciclo de vida da página e associa os dados retornados automaticamente. Não é necessário chamar explicitamente o método *DataBind* .

No exemplo a seguir, o controle *GridView* está configurado para usar um método chamado *GetCategories*:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample22.aspx)]

Você cria o método *GetCategories* no código da página. Para uma operação SELECT simples, o método não precisa de parâmetros e deve retornar um objeto *IEnumerable* ou *IQueryable* . Se a nova *Propriedade ItemType* for definida (o que permite expressões de ligação de dados fortemente tipadas, conforme explicado em [controles de dados fortemente tipados](#_Toc318097386) anteriormente), as versões genéricas dessas interfaces devem ser retornadas — *IEnumerable &lt; &gt; t* ou *IQueryable &lt; t &gt; *, com o parâmetro *T* correspondente ao tipo da propriedade *ItemType* (por exemplo, a * &lt; categoria &gt; IQueryable*).

O exemplo a seguir mostra o código para um método *GetCategories* . Este exemplo usa o modelo de Code First Entity Framework com o banco de dados de exemplo Northwind. O código garante que a consulta retorne detalhes dos produtos relacionados para cada categoria por meio do método *include* . (Isso garante que o elemento *TemplateField* na marcação exiba a contagem de produtos em cada categoria sem a necessidade de um [Select n + 1](http://stackoverflow.com/questions/97197/what-is-the-n1-selects-problem).)

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample23.cs)]

Quando a página é executada, o controle *GridView* chama o método *GetCategories* automaticamente e renderiza os dados retornados usando os campos configurados:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image2.png)

Como o método Select retorna um objeto *IQueryable* , o controle *GridView* pode manipular ainda mais a consulta antes de executá-la. Por exemplo, o controle *GridView* pode adicionar expressões de consulta para classificação e paginação ao objeto *IQueryable* retornado antes de ser executado, para que essas operações sejam executadas pelo provedor LINQ subjacente. Nesse caso, Entity Framework garantirá que essas operações sejam executadas no banco de dados.

O exemplo a seguir mostra o controle *GridView* modificado para permitir classificação e paginação:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample24.aspx)]

Agora, quando a página é executada, o controle pode garantir que apenas a página atual de dados seja exibida e que ela seja ordenada pela coluna selecionada:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image3.png)

Para filtrar os dados retornados, os parâmetros precisam ser adicionados ao método Select. Esses parâmetros serão preenchidos pela Associação de modelo em tempo de execução e você poderá usá-los para alterar a consulta antes de retornar os dados.

Por exemplo, suponha que você deseja permitir que os usuários filtrem os produtos inserindo uma palavra-chave na cadeia de caracteres de consulta. Você pode adicionar um parâmetro ao método e atualizar o código para usar o valor do parâmetro:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample25.cs)]

Esse código inclui uma expressão *Where* se um valor é fornecido para a *palavra-chave* e, em seguida, retorna os resultados da consulta.

<a id="_Toc318097389"></a>
#### <a name="value-providers"></a>Provedores de valor

O exemplo anterior não era específico sobre onde o valor do parâmetro *keyword* estava vindo. Para indicar essas informações, você pode usar um atributo de parâmetro. Para este exemplo, você pode usar a classe *querystringattribute* que está no namespace *System. Web. modelbinding* :

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample26.cs)]

Isso instrui a associação de modelo para tentar associar um valor da cadeia de caracteres de consulta ao parâmetro de *palavra-chave* em tempo de execução. (Isso pode envolver a execução da conversão de tipo, embora não nesse caso.) Se um valor não puder ser fornecido e o tipo for não anulável, uma exceção será lançada.

As fontes de valores para esses métodos são chamadas de provedores de valor, e os atributos de parâmetro que indicam qual provedor de valor usar são chamados de atributos de provedor de valor. Web Forms incluirá provedores de valor e atributos correspondentes para todas as fontes típicas de entrada do usuário em um aplicativo Web Forms, como a cadeia de caracteres de consulta, cookies, valores de formulário, controles, estado de exibição, estado de sessão e propriedades de perfil. Você também pode escrever provedores de valor personalizado.

Por padrão, o nome do parâmetro é usado como a chave para localizar um valor na coleção do provedor de valor. No exemplo, o código procurará um valor de cadeia de caracteres de consulta chamado palavra-chave (por exemplo, ~/Default.aspx? keyword = chefe). Você pode especificar uma chave personalizada passando-a como um argumento para o atributo de parâmetro. Por exemplo, para usar o valor da variável de cadeia de caracteres de consulta chamada q, você pode fazer isso:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample27.cs)]

Se esse método estiver no código da página, os usuários poderão filtrar os resultados passando uma palavra-chave usando a cadeia de caracteres de consulta:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image4.png)

A associação de modelo realiza muitas tarefas que, de outra forma, você teria de codificar manualmente: lendo o valor, verificando um valor nulo, tentando convertê-lo no tipo apropriado, verificando se a conversão foi bem-sucedida e, por fim, usando o valor na consulta. A associação de modelo resulta em um código muito menos e na capacidade de reutilizar a funcionalidade em todo o aplicativo.

<a id="_Toc318097390"></a>
#### <a name="filtering-by-values-from-a-control"></a>Filtrando por valores de um controle

Suponha que você deseja estender o exemplo para permitir que o usuário escolha um valor de filtro em uma lista suspensa. Adicione a seguinte lista suspensa à marcação e configure-a para obter seus dados de outro método usando a propriedade *SelectMethod* :

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample28.aspx)]

Normalmente, você também adicionaria um elemento *EmptyDataTemplate* ao controle *GridView* para que o controle exiba uma mensagem se nenhum produto correspondente for encontrado:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample29.aspx)]

No código da página, adicione o novo método Select para a lista suspensa:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample30.cs)]

Por fim, atualize o método *GetProducts* SELECT para obter um novo parâmetro que contém a ID da categoria selecionada na lista suspensa:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample31.cs)]

Agora, quando a página é executada, os usuários podem selecionar uma categoria na lista suspensa e o controle *GridView* é automaticamente reassociado para mostrar os dados filtrados. Isso é possível porque a associação de modelo rastreia os valores dos parâmetros para os métodos Select e detecta se algum valor de parâmetro foi alterado após um postback. Nesse caso, a associação de modelo força o controle de dados associado a reassociar aos dados.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image5.png)

<a id="_Toc318097391"></a>
### <a name="html-encoded-data-binding-expressions"></a>Expressões de Data-Binding codificadas em HTML

Agora você pode codificar em HTML o resultado de expressões de ligação de dados. Adicionar dois-pontos (:) para o final do &lt; prefixo% # que marca a expressão de vinculação de dados:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample32.aspx)]

<a id="_Toc318097392"></a>
### <a name="unobtrusive-validation"></a>Validação não invasiva

Agora você pode configurar os controles de validador internos para usar JavaScript discreto para lógica de validação no lado do cliente. Isso reduz significativamente a quantidade de JavaScript renderizada embutida na marcação de página e reduz o tamanho geral da página. Você pode configurar o JavaScript discreto para controles de validação de qualquer uma das seguintes maneiras:

- Globalmente, adicionando a seguinte configuração ao elemento * &lt; appSettings &gt; * no arquivo de Web.config: 

    [!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample33.xml)]
- Globalmente, definindo a propriedade estática *System. Web. UI. ValidationSettings. UnobtrusiveValidationMode* como *UnobtrusiveValidationMode. WebForms* (normalmente, no método * \_ Start do aplicativo* no arquivo global. asax).
- Individualmente para uma página, definindo a nova propriedade *UnobtrusiveValidationMode* da classe *Page* como *UnobtrusiveValidationMode. WebForms*.

<a id="_Toc318097393"></a>
### <a name="html5-updates"></a>Atualizações do HTML5

Foram feitas algumas melhorias nos controles de servidor Web Forms para aproveitar os novos recursos do HTML5:

- A propriedade *TextMode* do controle *TextBox* foi atualizada para dar suporte aos novos tipos de entrada do HTML5, como *email*, *DateTime*e assim por diante.
- O controle *FileUpload* agora dá suporte a vários carregamentos de arquivo de navegadores que dão suporte a esse recurso HTML5.
- Os controles de validação agora dão suporte à validação de elementos de entrada do HTML5.
- Novos elementos HTML5 que têm atributos que representam uma URL agora dão suporte a runat = "Server". Como resultado, você pode usar convenções ASP.NET em caminhos de URL, como o operador ~ para representar a raiz do aplicativo (por exemplo, &lt; vídeo runat = "Server" src = "~/myVideo.wmv"/ &gt; ).
- O controle *UpdatePanel* foi corrigido para dar suporte ao lançamento de campos de entrada HTML5.

<a id="_Toc318097394"></a>
## <a name="aspnet-mvc-4"></a>ASP.NET MVC 4

O ASP.NET MVC 4 beta agora está incluído no Visual Studio 11 Beta. O ASP.NET MVC é uma estrutura para o desenvolvimento de aplicativos Web altamente testados e que podem ser mantidos aproveitando o padrão MVC (Model-View-Controller). O ASP.NET MVC 4 facilita a criação de aplicativos para a Web móvel e inclui ASP.NET Web API, o que ajuda a criar serviços HTTP que podem acessar qualquer dispositivo. Para obter mais informações, consulte as [notas de versão do ASP.NET MVC 4](mvc4-release-notes.md).

<a id="_Toc318097395"></a>
## <a name="aspnet-web-pages-2"></a>Páginas da Web do ASP.NET 2

Os novos recursos incluem o seguinte:

- Modelos de site novos e atualizados.
- Adicionar validação do lado do servidor e do cliente usando o auxiliar de *validação* .
- A capacidade de registrar scripts usando um Gerenciador de ativos.
- Habilitando logons do Facebook e de outros sites usando OAuth e OpenID.
- Adicionando mapas usando o auxiliar *Maps* .
- Executar aplicativos de páginas da Web lado a lado.
- Renderizando páginas para dispositivos móveis.

Para obter mais informações sobre esses recursos e exemplos de código de página inteira, consulte [os principais recursos nas páginas da Web 2 beta](https://go.microsoft.com/fwlink/?LinkID=227824).

<a id="_Toc318097396"></a>
## <a name="visual-web-developer-11-beta"></a>Visual Web Developer 11 Beta

Esta seção fornece informações sobre melhorias para o desenvolvimento para a Web no Visual Web Developer 11 Beta e no Visual Studio 2012 Release Candidate.

<a id="project-compatibility"></a>
### <a name="project-sharing-between-visual-studio-2010-and-visual-studio-2012-release-candidate-project-compatibility"></a>Compartilhamento de projetos entre o Visual Studio 2010 e o Visual Studio 2012 Release Candidate (compatibilidade do projeto)

Até o Visual Studio 2012 versão Release Candidate, abrir um projeto existente em uma versão mais recente do Visual Studio iniciou o assistente de conversão. Isso forçou uma atualização do conteúdo (ativos) de um projeto e uma solução para novos formatos que não eram compatíveis com versões anteriores. Portanto, após a conversão, não foi possível abrir o projeto na versão mais antiga do Visual Studio.

Muitos clientes nos disseram que essa não era a abordagem certa. No Visual Studio 11 Beta, agora damos suporte ao compartilhamento de projetos e soluções com o Visual Studio 2010 SP1. Isso significa que, se você abrir um projeto 2010 no Visual Studio 2012 versão Release Candidate, ainda poderá abrir o projeto no Visual Studio 2010 SP1.

> [!NOTE]
> Alguns tipos de projetos não podem ser compartilhados entre o Visual Studio 2010 SP1 e o Visual Studio 2012 Release Candidate. Isso inclui alguns projetos mais antigos (como projetos ASP.NET MVC 2) ou projetos para fins especiais (como projetos de instalação).

Quando você abre um projeto Web do Visual Studio 2010 SP1 pela primeira vez no Visual Studio 11 Beta, as seguintes propriedades são adicionadas ao arquivo de projeto:

- FileUpgradeFlags
- UpgradeBackupLocation
- OldToolsVersion
- VisualStudioVersion
- VSToolsPath

FileUpgradeFlags, UpgradeBackupLocation e OldToolsVersion são usados pelo processo que atualiza o arquivo de projeto. Eles não têm nenhum impacto ao trabalhar com o projeto no Visual Studio 2010.

VisualStudioVersion é uma nova propriedade usada pelo MSBuild 4,5 que indica a versão do Visual Studio para o projeto atual. Como essa propriedade não existia no MSBuild 4,0 (a versão do MSBuild que o Visual Studio 2010 SP1 usa), injetamos um valor padrão no arquivo de projeto.

A propriedade VSToolsPath é usada para determinar o arquivo. targets correto a ser importado do caminho representado pela configuração MSBuildExtensionsPath32.

Há também algumas alterações relacionadas aos elementos de importação. Essas alterações são necessárias para dar suporte à compatibilidade entre as duas versões do Visual Studio.

> [!NOTE]
> Se um projeto estiver sendo compartilhado entre o Visual Studio 2010 SP1 e o Visual Studio 11 Beta em dois computadores diferentes, e se o projeto incluir um banco de dados local na \_ pasta Data app, você deverá certificar-se de que a versão do SQL Server usada pelo Database seja instalada em ambos os computadores.

<a id="Configuration_Changes_In_ASPNET45_Website_Templates"></a>
### <a name="configuration-changes-in-aspnet-45-website-templates"></a>Alterações de configuração nos modelos de site do ASP.NET 4,5

As seguintes alterações foram feitas no arquivo de *Web.config* padrão para sites criados usando modelos de site no Visual Studio 2012 versão Release Candidate:

- No `<httpRuntime>` elemento, o `encoderType` atributo agora é definido por padrão para usar os tipos AntiXSS que foram adicionados ao ASP.net. Para obter detalhes, consulte [biblioteca AntiXSS](#_Toc318097382).
- Também no `<httpRuntime>` elemento, o `requestValidationMode` atributo é definido como "4,5". Isso significa que, por padrão, a validação de solicitação é configurada para usar a validação adiada ("lenta"). Para obter detalhes, consulte [novos recursos de validação de solicitação do ASP.net](#_Toc318097379).
- O `<modules>` elemento da `<system.webServer>` seção não contém um `runAllManagedModulesForAllRequests` atributo. (Seu valor padrão é false.) Isso significa que, se você estiver usando uma versão do IIS 7 que não foi atualizada para o SP1, talvez tenha problemas com o roteamento em um novo site. Para obter mais informações, consulte [suporte nativo no IIS 7 para roteamento ASP.net](#Native_Support_In_IIS7_For_ASPNET_Routine).

Essas alterações não afetam os aplicativos existentes. No entanto, eles podem representar uma diferença no comportamento entre sites existentes e novos sites que você cria para ASP.NET 4,5 usando os novos modelos.

<a id="Native_Support_In_IIS7_For_ASPNET_Routine"></a>
### <a name="native-support-in-iis-7-for-aspnet-routing"></a>Suporte nativo no IIS 7 para roteamento ASP.NET

Isso não é uma alteração de ASP.NET como tal, mas uma alteração nos modelos para novos projetos de site que podem afetar você se você estiver trabalhando em uma versão do IIS 7 que não tenha a atualização do SP1 aplicada.

No ASP.NET, você pode adicionar a definição de configuração a seguir aos aplicativos para dar suporte ao roteamento:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample34.xml?highlight=3)]

Quando **runAllManagedModulesForAllRequests** é true, uma URL como `http://mysite/myapp/home` vai para ASP.net, mesmo que não haja *. aspx*, *. Mvc*ou extensão semelhante na URL.

Uma atualização feita no IIS 7 torna a configuração **runAllManagedModulesForAllRequests** desnecessária e dá suporte ao roteamento do ASP.net nativamente. (Para obter informações sobre a atualização, consulte o artigo Suporte da Microsoft [há uma atualização disponível que permite que determinados manipuladores do iis 7,0 ou iis 7,5 manipulem solicitações cujas URLs não terminam com um ponto](https://support.microsoft.com/kb/980368).)

Se o seu site estiver em execução no IIS 7 e se o IIS tiver sido atualizado, você não precisará definir **runAllManagedModulesForAllRequests** como true. Na verdade, não é recomendável defini-lo como true, pois ele adiciona sobrecarga de processamento desnecessária à solicitação. Quando essa configuração é verdadeira, todas as solicitações, incluindo as de *. htm*, *. jpg*, e outros arquivos estáticos, também passam pelo pipeline de solicitação ASP.net.

Se você criar um novo site do ASP.NET 4,5 usando os modelos fornecidos no Visual Studio 2012 RC, a configuração do site não incluirá a configuração **runAllManagedModulesForAllRequests** . Isso significa que, por padrão, a configuração é false.

Se você executar o site no Windows 7 sem o SP1 instalado, o IIS 7 não incluirá a atualização necessária. Como consequência, o roteamento não funcionará e você verá erros. Se você tiver um problema em que o roteamento não funciona, você pode fazer o seguinte:

- Atualize o Windows 7 para o SP1, que adicionará a atualização ao IIS 7.
- Instale a atualização descrita no artigo Suporte da Microsoft listado anteriormente.
- Defina **runAllManagedModulesForAllRequests** como true no arquivo de Web.config do site. Observe que isso adicionará alguma sobrecarga às solicitações.

<a id="_Toc318097397"></a>
### <a name="html-editor"></a>Editor de HTML

<a id="_Toc318097398"></a>
#### <a name="smart-tasks"></a>Tarefas inteligentes

Em modo de exibição de Design, propriedades complexas de controles de servidor geralmente têm caixas de diálogo e assistentes associados para facilitar sua definição. Por exemplo, você pode usar uma caixa de diálogo especial para adicionar uma fonte de dados a um controle *Repeater* ou adicionar colunas a um controle *GridView* .

No entanto, esse tipo de ajuda de interface do usuário para propriedades complexas não está disponível na exibição de código-fonte. Portanto, o Visual Studio 11 introduz tarefas inteligentes para o modo de exibição de origem. As tarefas inteligentes são atalhos com reconhecimento de contexto para recursos comumente usados nos editores do C# e do Visual Basic.

Para controles de Web Forms de ASP.NET, as tarefas inteligentes aparecem nas marcas de servidor como um pequeno glifo quando o ponto de inserção está dentro do elemento:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image6.png)

O Tarefa Inteligente se expande quando você clica no glifo ou pressiona CTRL +. (ponto), assim como nos editores de código. Em seguida, ele exibe atalhos semelhantes às tarefas inteligentes no modo de exibição de Design.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image7.png)

Por exemplo, a Tarefa Inteligente na ilustração anterior mostra as opções de tarefas GridView. Se você escolher Editar colunas, a caixa de diálogo a seguir será exibida:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image8.png)

Preencher a caixa de diálogo define as mesmas propriedades que podem ser definidas em modo de exibição de Design. Quando você clica em OK, a marcação para o controle é atualizada com as novas configurações:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image9.png)

<a id="_Toc318097399"></a>
#### <a name="wai-aria-support"></a>WAI-suporte a ARIA

Escrever sites acessíveis está se tornando cada vez mais importante. O [padrão de acessibilidade WAI-ARIA](http://www.w3.org/WAI/intro/aria) define como os desenvolvedores devem escrever sites acessíveis. Esse padrão agora tem suporte total no Visual Studio.

Por exemplo, o atributo *role* agora tem o IntelliSense completo:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image10.png)

O padrão WAI-ARIA também apresenta atributos que são prefixados com o *Aria,* que permitem que você adicione semântica a um documento HTML5. O Visual Studio também dá suporte completo a estes atributos do *Aria* :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image11.png) ![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image12.png)

<a id="_Toc318097400"></a>
#### <a name="new-html5-snippets"></a>Novos trechos de HTML5

Para tornar mais rápido e fácil escrever a marcação HTML5 usada com frequência, o Visual Studio inclui vários trechos de código. Um exemplo é o trecho de vídeo:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image13.png)

Para invocar o trecho de código, pressione TAB duas vezes quando o elemento for selecionado no IntelliSense:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image14.png)

Isso produz um trecho de código que você pode personalizar.

<a id="_Toc318097401"></a>
#### <a name="extract-to-user-control"></a>Extrair para controle de usuário

Em grandes páginas da Web, pode ser uma boa ideia mover partes individuais para controles de usuário. Essa forma de refatoração pode ajudar a aumentar a legibilidade da página e pode simplificar a estrutura da página.

Para tornar isso mais fácil, quando você edita Web Forms páginas no modo de exibição de origem, agora você pode selecionar texto em uma página, clicar com o botão direito do mouse nele e escolher extrair para o controle de usuário:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image2.jpg)

<a id="_Toc318097402"></a>
#### <a name="intellisense-for-code-nuggets-in-attributes"></a>IntelliSense para nuggets de código em atributos

O Visual Studio sempre forneceu o IntelliSense para nuggets de código do lado do servidor em qualquer página ou controle. Agora, o Visual Studio inclui o IntelliSense para código Nuggets em atributos HTML também.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image15.png)

Isso facilita a criação de expressões de vinculação de dados:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image16.png)

<a id="_Toc318097403"></a>
#### <a name="automatic-renaming-of-matching-tag-when-you-rename-an-opening-or-closing-tag"></a>Renomeação automática de marca correspondente ao renomear uma marca de abertura ou de fechamento

Se você renomear um elemento HTML (por exemplo, você alterar uma marca *div* para ser uma marca de *cabeçalho* ), a marca de abertura ou de fechamento correspondente também será alterada em tempo real.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image17.png)

Isso ajuda a evitar o erro em que você se esquece de alterar uma marca de fechamento ou alterar a incorreta.

<a id="_Toc318097404"></a>
#### <a name="event-handler-generation"></a>Geração de manipulador de eventos

O Visual Studio agora inclui recursos na exibição de código-fonte para ajudá-lo a escrever manipuladores de eventos e associá-los manualmente. Se você estiver editando um nome de evento na exibição de origem, o IntelliSense exibirá &lt; criar novo evento &gt; , que criará um manipulador de eventos no código da página que tem a assinatura correta:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image3.jpg)

Por padrão, o manipulador de eventos usará a ID do controle para o nome do método de manipulação de eventos:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image4.jpg)

O manipulador de eventos resultante terá a seguinte aparência (neste caso, em C#):

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image18.png)

<a id="_Toc318097405"></a>
#### <a name="smart-indent"></a>Recuo inteligente

Quando você pressiona ENTER enquanto estiver dentro de um elemento HTML vazio, o editor colocará o ponto de inserção no lugar certo:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image19.png)

Se você pressionar Enter neste local, a marca de fechamento será movida para baixo e recuada para corresponder à marca de abertura. O ponto de inserção também é recuado:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image20.png)

<a id="_Toc318097406"></a>
#### <a name="auto-reduce-statement-completion"></a>Conclusão da instrução de redução automática

A lista do IntelliSense no Visual Studio agora filtra com base no que você digita para que ele exiba apenas as opções relevantes:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image21.png)

O IntelliSense também filtra com base no título caso das palavras individuais na lista do IntelliSense. Por exemplo, se você digitar "DL", DL e ASP: DataList serão exibidos:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image22.png)

Esse recurso torna mais rápido obter a conclusão da instrução para elementos conhecidos.

<a id="_Toc318097407"></a>
### <a name="javascript-editor"></a>Editor JavaScript

O editor de JavaScript no Visual Studio 2012 versão Release Candidate é completamente novo e melhora muito a experiência de trabalhar com JavaScript no Visual Studio.

<a id="_Toc318097408"></a>
#### <a name="code-outlining"></a>Estrutura de tópicos de código

As regiões de estrutura de tópicos agora são criadas automaticamente para todas as funções, permitindo que você recolha partes do arquivo que não são pertinentes ao seu foco atual.

<a id="_Toc318097409"></a>
#### <a name="brace-matching"></a>Correspondência de chaves

Quando você coloca o ponto de inserção em uma chave de abertura ou de fechamento, o editor realça a correspondência uma.

<a id="_Toc318097410"></a>
#### <a name="go-to-definition"></a>Ir para Definição

O comando go to Definition permite que você salte para a origem de uma função ou variável.

<a id="_Toc318097411"></a>
#### <a name="ecmascript5-support"></a>Suporte do ECMAScript5

O editor dá suporte à nova sintaxe e APIs no ECMAScript5, a versão mais recente do padrão que descreve a linguagem JavaScript.

<a id="_Toc318097412"></a>
#### <a name="dom-intellisense"></a>IntelliSense DOM

O IntelliSense para APIs DOM foi aprimorado, com suporte para muitas novas APIs HTML5, incluindo *querySelector*, armazenamento dom, mensagens em documentos cruzadas e *tela*. O IntelliSense do DOM agora é controlado por um único arquivo JavaScript simples, em vez de por uma definição de biblioteca de tipo nativo. Isso facilita a extensão ou a substituição.

<a id="_Toc318097413"></a>
#### <a name="vsdoc-signature-overloads"></a>Sobrecargas de assinatura VSDOC

Os comentários detalhados do IntelliSense agora podem ser declarados para sobrecargas separadas de funções JavaScript usando o novo elemento * &lt; Signature &gt; * , conforme mostrado neste exemplo:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample35.cs)]

<a id="_Toc318097414"></a>
#### <a name="implicit-references"></a>Referências implícitas

Agora você pode adicionar arquivos JavaScript a uma lista central que será incluída implicitamente na lista de arquivos que qualquer determinado arquivo JavaScript ou referências de bloco, o que significa que você obterá o IntelliSense para seu conteúdo. Por exemplo, você pode adicionar arquivos jQuery à lista central de arquivos e obterá o IntelliSense para funções jQuery em qualquer bloco de arquivo JavaScript, se você o tiver referenciado explicitamente (usando/// &lt; Reference/ &gt; ) ou não.

<a id="_Toc318097415"></a>
### <a name="css-editor"></a>Editor de CSS

<a id="_Toc318097416"></a>
#### <a name="auto-reduce-statement-completion"></a>Conclusão da instrução de redução automática

A lista do IntelliSense para CSS agora filtra com base nas propriedades e valores de CSS com suporte no esquema selecionado.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image23.png)

O IntelliSense também dá suporte a pesquisas de caso de título:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image24.png)

<a id="_Toc318097417"></a>
#### <a name="hierarchical-indentation"></a>Recuo hierárquico

O editor de CSS usa o recuo para exibir regras hierárquicas, que fornece uma visão geral de como as regras em cascata são organizadas logicamente. No exemplo a seguir, o #list um seletor é um filho em cascata da lista e, portanto, é recuado.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image25.png)

O exemplo a seguir mostra herança mais complexa:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image26.png)

O recuo de uma regra é determinado por suas regras pai. O recuo hierárquico é habilitado por padrão, mas você pode desabilitá-lo na caixa de diálogo Opções (ferramentas, opções na barra de menus):

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image27.png)

<a id="_Toc318097418"></a>
#### <a name="css-hacks-support"></a>Suporte a hackers da CSS

A análise de centenas de arquivos CSS do mundo real mostra que os hackers do CSS são muito comuns e agora o Visual Studio dá suporte aos mais amplamente usados. Esse suporte inclui o IntelliSense e a validação dos \* hacks da propriedade Star () e subscore ( \_ ):

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image28.png)

Também há suporte para hackers de seletor típicos para que o recuo hierárquico seja mantido mesmo quando eles forem aplicados. Um seletor típico de hackers usado para direcionar o Internet Explorer 7 é preceder um seletor com * \* : First-Child + HTML*. O uso dessa regra manterá o recuo hierárquico:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image29.png)

<a id="_Toc318097419"></a>
#### <a name="vendor-specific-schemas--moz---webkit"></a>Esquemas específicos do fornecedor (-Moz-,-WebKit)

O CSS3 apresenta muitas propriedades que foram implementadas por diferentes navegadores em momentos diferentes. Isso anteriormente forçava os desenvolvedores a codificarem navegadores específicos usando a sintaxe específica do fornecedor. Essas propriedades específicas do navegador agora estão incluídas no IntelliSense.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image30.png)

<a id="_Toc318097420"></a>
#### <a name="commenting-and-uncommenting-support"></a>Comentar e remover comentários do suporte

Agora você pode comentar e remover o comentário das regras de CSS usando os mesmos atalhos que você usa no editor de código (CTRL + K, C para comentar e CTRL + K, você deve remover os comentários).

<a id="_Toc318097421"></a>
#### <a name="color-picker"></a>Seletor de cor

Nas versões anteriores do Visual Studio, o IntelliSense para atributos relacionados a cores consistia em uma lista suspensa de valores de cores nomeados. Essa lista foi substituída por um seletor de cores completo.

Quando você insere um valor de cor, o seletor de cores é exibido automaticamente e apresenta uma lista de cores usadas anteriormente, seguidas por uma paleta de cores padrão. Você pode selecionar uma cor usando o mouse ou o teclado.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image31.png)

A lista pode ser expandida em um seletor de cores completo. O seletor permite controlar o canal alfa convertendo automaticamente qualquer cor em RGBA quando você move o controle deslizante opacidade:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image32.png)

<a id="_Toc318097422"></a>
#### <a name="snippets"></a>Snippets

Os trechos de código no editor de CSS facilitam e agilizam a criação de estilos entre navegadores. Muitas propriedades do CSS3 que exigem configurações específicas do navegador agora foram transferidas para os trechos de código.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image33.png)

Os trechos de código CSS dão suporte a cenários avançados (como consultas de mídia do CSS3) digitando o símbolo arroba (@), que mostra a lista do IntelliSense.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image34.png)

Quando você seleciona @media valor e pressiona Tab, o editor de CSS insere o seguinte trecho de código:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image5.jpg)

Assim como acontece com trechos de código, você pode criar seus próprios trechos CSS.

<a id="_Toc318097423"></a>
#### <a name="custom-regions"></a>Regiões personalizadas

Regiões de código nomeado, que já estão disponíveis no editor de códigos, agora estão disponíveis para edição de CSS. Isso permite que você agrupe facilmente blocos de estilo relacionados.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image35.png)

Quando uma região é recolhida, ela exibe o nome da região:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image36.png)

<a id="_Toc318097424"></a>
### <a name="page-inspector"></a>Inspetor de Página

Inspetor de Página é uma ferramenta que renderiza uma página da Web (HTML, Web Forms, ASP.NET MVC ou páginas da Web) no IDE do Visual Studio e permite que você examine o código-fonte e a saída resultante. Para páginas ASP.NET, Inspetor de Página permite que você determine qual código do lado do servidor produziu a marcação HTML que é renderizada para o navegador.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image37.png)

Para obter mais informações sobre Inspetor de Página, consulte os seguintes tutoriais:

- Usando Inspetor de Página no [MVC ASP.net](../mvc/overview/views/using-page-inspector-in-aspnet-mvc.md)
- Usando Inspetor de Página no [ASP.NET Web Forms](../web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project.md)

<a id="_Toc318097425"></a>
### <a name="publishing"></a>Publicando

<a id="_Toc318097426"></a>
#### <a name="publish-profiles"></a>Perfis de publicação

No Visual Studio 2010, as informações de publicação para projetos de aplicativos Web não são armazenadas no controle de versão e não são projetadas para compartilhamento com outras pessoas. No Visual Studio 2012 versão Release Candidate, o formato do perfil de publicação foi alterado. Ele tornou-se um artefato de equipe, e agora é fácil aproveitar as compilações com base no MSBuild. As informações de configuração da compilação estão na caixa de diálogo Publicar para que você possa alternar facilmente as configurações de compilação antes da publicação.

Os perfis de publicação são armazenados na pasta PublishProfiles O local da pasta depende de qual linguagem de programação você está usando:

- C#: Properties\PublishProfiles
- Visual Basic: meu Project\PublishProfiles

Cada perfil é um arquivo do MSBuild. Durante a publicação, esse arquivo é importado para o arquivo do MSBuild do projeto. No Visual Studio 2010, se você quiser fazer alterações no processo de publicação ou pacote, precisará colocar suas personalizações em um arquivo chamado **ProjectName**. WPP. targets. Ainda há suporte para isso, mas agora você pode colocar suas personalizações no próprio perfil de publicação. Dessa forma, as personalizações serão usadas somente para esse perfil.

Agora você também pode aproveitar os perfis de publicação do MSBuild. Para fazer isso, use o seguinte comando ao compilar o projeto:

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample36.cmd)]

O valor Project. csproj é o caminho do projeto e ProfileName é o nome do perfil a ser publicado. Como alternativa, em vez de passar o nome do perfil para a propriedade *PublishProfile* , você pode passar o caminho completo para o perfil de publicação.

<a id="_Toc318097427"></a>
#### <a name="aspnet-precompilation-and-merge"></a>Pré-compilação e mesclagem ASP.NET

Para projetos de aplicativos Web, o Visual Studio 2012 versão Release Candidate adiciona uma opção na página de propriedades da Web de pacote/publicação que permite pré-compilar e mesclar o conteúdo do site quando você publicar ou empacotar o projeto. Para ver essas opções, clique com o botão direito do mouse no projeto em Gerenciador de Soluções, escolha Propriedades e, em seguida, escolha a página de propriedade pacote/publicar Web. A ilustração a seguir mostra a opção pré-compilar este aplicativo antes de publicar.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image6.jpg)

Quando essa opção é selecionada, o Visual Studio pré-compila o aplicativo sempre que você publica ou empacota o aplicativo Web. Se você quiser controlar como o site é pré-compilado ou como os assemblies são mesclados, clique no botão Avançado para configurar essas opções.

<a id="_Toc318097428"></a>
### <a name="iis-express"></a>IIS Express

O servidor Web padrão para testar projetos Web no Visual Studio agora é IIS Express. O Visual Studio Development Server ainda é uma opção para o servidor Web local durante o desenvolvimento, mas IIS Express agora é o servidor recomendado. A experiência de usar o IIS Express no Visual Studio 11 Beta é muito semelhante a usá-lo no Visual Studio 2010 SP1.

<a id="_Toc318097429"></a>
## <a name="disclaimer"></a>Isenção de responsabilidade

Este é um documento preliminar e pode ser alterado substancialmente antes da versão comercial final do software descrito aqui.

As informações contidas neste documento representam a visão atual da Microsoft Corporation acerca das questões discutidas até a data da publicação. Como a Microsoft deve reagir às dinâmicas condições do mercado, essas informações não devem ser interpretadas como um compromisso por parte da Microsoft e a Microsoft não garante a precisão de qualquer informação apresentada após a data de publicação.

Este whitepaper é apenas para fins informativos. A MICROSOFT NÃO OFERECE GARANTIA, EXPRESSA, IMPLÍCITA OU ESTATUTÁRIA, DAS INFORMAÇÕES CONTIDAS NESTE DOCUMENTO.

Obedecer a todas as leis de direitos autorais aplicáveis é responsabilidade do usuário. Sem limitar os direitos autorais, nenhuma parte deste documento pode ser reproduzida, armazenada ou introduzida em um sistema de recuperação, ou transmitida de qualquer forma ou por qualquer meio (seja eletrônico, mecânico, fotocópia, gravação ou outro), ou para qualquer finalidade, sem a permissão expressa e por escrito da Microsoft Corporation.

A Microsoft pode ter patentes ou requisições para obtenção de patente, marcas comerciais, direitos autorais ou outros direitos de propriedade intelectual que abrangem o conteúdo deste documento. A posse deste documento não lhe confere nenhum direito sobre patentes, marcas comerciais, direitos autorais ou outros direitos de propriedade intelectual, salvo aqueles expressamente mencionados em um contrato de licença, por escrito, da Microsoft.

Salvo indicação em contrário, os exemplos de empresas, organizações, produtos, nomes de domínio, endereços de email, logotipos, pessoas, lugares e eventos aqui mencionados são fictícios e nenhuma associação com qualquer empresa, organização, produto, nome de domínio, endereço de email, logotipo, pessoa, lugar ou evento real é intencional ou deve ser inferida.

© 2012 Microsoft Corporation. Todos os direitos reservados.

Microsoft e Windows são marcas registradas ou comerciais da Microsoft Corporation nos Estados Unidos e/ou em outros países.

Os nomes de empresas reais e produtos mencionados aqui podem ser marcas comerciais de seus respectivos proprietários.
