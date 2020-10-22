---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/distributed-caching
title: Caching distribuído (criando Real-World aplicativos de nuvem com o Azure) | Microsoft Docs
author: MikeWasson
description: A criação de aplicativos de nuvem do mundo real com o livro eletrônico do Azure é baseada em uma apresentação desenvolvida por Scott Guthrie. Ele explica 13 padrões e práticas que podem...
ms.author: riande
ms.date: 07/20/2015
ms.assetid: 406518e9-3817-49ce-8b90-e82bc461e2c0
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/distributed-caching
msc.type: authoredcontent
ms.openlocfilehash: 87a7516415895e761d1589fd459b93e5c15c0f85
ms.sourcegitcommit: c62ec20b453cee3249eb894ecd75013b57d078f0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/21/2020
ms.locfileid: "92345516"
---
# <a name="distributed-caching-building-real-world-cloud-apps-with-azure"></a>Caching distribuído (criando Real-World aplicativos de nuvem com o Azure)

por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra)

[Baixar o projeto de ti](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [baixar o livro eletrônico](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> A **criação de aplicativos de nuvem do mundo real com** o livro eletrônico do Azure é baseada em uma apresentação desenvolvida por Scott Guthrie. Ele explica 13 padrões e práticas que podem ajudá-lo a desenvolver com sucesso aplicativos Web para a nuvem. Para obter informações sobre o livro eletrônico, consulte [o primeiro capítulo](introduction.md).

O capítulo anterior examinou o tratamento de falhas transitórias e o cache mencionado como uma estratégia de disjuntor. Este capítulo fornece mais informações sobre cache, incluindo quando usá-lo, padrões comuns para usá-lo e como implementá-lo no Azure.

## <a name="what-is-distributed-caching"></a>O que é o Caching distribuído

Um cache fornece alta taxa de transferência e acesso de baixa latência a dados de aplicativo normalmente acessados, armazenando os dados na memória. Para um aplicativo de nuvem, o tipo mais útil de cache é o cache distribuído, o que significa que os dados não são armazenados na memória do servidor Web individual, mas em outros recursos de nuvem, e os dados armazenados em cache são disponibilizados para todos os servidores Web de um aplicativo (ou outras VMs de nuvem que são usadas pelo aplicativo).

![diagrama mostrando vários servidores Web que acessam os mesmos servidores de cache](distributed-caching/_static/image1.png)

Quando o aplicativo é dimensionado adicionando ou removendo servidores, ou quando os servidores são substituídos devido a atualizações ou falhas, os dados armazenados em cache permanecem acessíveis para todos os servidores que executam o aplicativo.

Ao evitar o acesso a dados de alta latência de um armazenamento de dados persistente, o Caching pode melhorar drasticamente a capacidade de resposta do aplicativo. Por exemplo, a recuperação de dados do cache é muito mais rápida do que recuperá-los de um banco de dados relacional.

Um benefício adicional do armazenamento em cache é o tráfego reduzido para o armazenamento de dados persistente, o que pode resultar em custos menores quando há encargos de egresso de dados para o armazenamento de dados persistente.

## <a name="when-to-use-distributed-caching"></a>Quando usar o Caching distribuído

O Caching funciona melhor para cargas de trabalho de aplicativo que fazem mais leitura do que a gravação de dados e quando o modelo de dados dá suporte à organização de chave/valor que você usa para armazenar e recuperar dados no cache. Também é mais útil quando os usuários do aplicativo compartilham muitos dados comuns; por exemplo, o cache não forneceria tantos benefícios se cada usuário recuperar dados exclusivos para esse usuário. Um exemplo em que o Caching pode ser muito benéfico é um catálogo de produtos, porque os dados não são alterados com frequência e todos os clientes estão examinando os mesmos dados.

O benefício do armazenamento em cache torna-se cada vez mais mensurável quanto mais um aplicativo é dimensionado, pois os limites de taxa de transferência e os atrasos de latência do armazenamento de dados persistentes se tornam mais um limite no desempenho geral do aplicativo. No entanto, você pode implementar o cache por outros motivos do que o desempenho também. Para dados que não precisam ser perfeitamente atualizados quando mostrados para o usuário, o acesso ao cache pode servir como um disjuntor para quando o armazenamento de dados persistente não está respondendo ou indisponível.

## <a name="popular-cache-population-strategies"></a>Estratégias populares de população de cache

Para poder recuperar dados do cache, você precisa armazená-los primeiro. Há várias estratégias para obter dados necessários em um cache:

- Por demanda/cache

    O aplicativo tenta recuperar dados do cache e, quando o cache não tem os dados (um "erro"), o aplicativo armazena os dados no cache para que eles estejam disponíveis na próxima vez. Na próxima vez que o aplicativo tentar obter os mesmos dados, ele encontrará o que está procurando no cache (um "clique"). Para evitar a busca de dados armazenados em cache que foram alterados no banco de dado, você invalidará o cache ao fazer alterações no armazenamento de dados.
- Push de dados em segundo plano

    Os serviços em segundo plano enviam dados por push para o cache em um agendamento regular e o aplicativo sempre efetua pull do cache. Essa abordagem funciona muito bem com fontes de dados de alta latência que não exigem que você sempre retorne os dados mais recentes.
- Disjuntor

    O aplicativo normalmente se comunica diretamente com o armazenamento de dados persistente, mas quando o armazenamento de dados persistentes tem problemas de disponibilidade, o aplicativo recupera dados do cache. Os dados podem ter sido colocados no cache usando a estratégia de envio por push de dados em cache ou em segundo plano. Trata-se de uma estratégia de tratamento de falhas em vez de uma estratégia de aprimoramento de desempenho.

Para manter os dados no cache atualizados, você pode excluir entradas de cache relacionadas quando seu aplicativo cria, atualiza ou exclui dados. Se for muito bem para seu aplicativo, às vezes, obter dados que estão ligeiramente desatualizados, você poderá contar com um tempo de expiração configurável para definir um limite de como os dados de cache antigos podem ser.

Você pode configurar a expiração absoluta (quantidade de tempo desde que o item de cache foi criado) ou a expiração deslizante (quantidade de tempo desde a última vez que um item de cache foi acessado). A expiração absoluta é usada quando você está dependendo do mecanismo de expiração do cache para impedir que os dados se tornem muito obsoletos. No aplicativo corrigir ti, removeremos manualmente itens de cache obsoletos, e usaremos a expiração deslizante para manter os dados mais atuais no cache. Independentemente da política de expiração que você escolher, o cache removerá automaticamente os itens mais antigos (menos usados recentemente ou LRU) quando o limite de memória do cache for atingido.

## <a name="sample-cache-aside-code-for-fix-it-app"></a>Exemplo de código de cache para corrigir o aplicativo de ti

No código de exemplo a seguir, verificamos o cache primeiro ao recuperar uma tarefa corrigir. Se a tarefa for encontrada no cache, a retornaremos; Se não for encontrado, nós o obteremos do banco de dados e o armazenaremos no cache. As alterações feitas para adicionar o cache ao `FindTaskByIdAsync` método são realçadas.

[!code-csharp[Main](distributed-caching/samples/sample1.cs?highlight=5,9-11,13-15,19)]

Quando você atualiza ou exclui uma tarefa corrigir, é necessário invalidar (remover) a tarefa armazenada em cache. Caso contrário, as tentativas futuras de ler essa tarefa continuarão a obter os dados antigos do cache.

[!code-csharp[Main](distributed-caching/samples/sample2.cs?highlight=7)]

Estes são exemplos para ilustrar o código de cache simples; o Caching não foi implementado no projeto Fix it de download.

## <a name="azure-caching-services"></a>Serviços de cache do Azure

O Azure oferece os seguintes serviços de cache: [cache Redis do Azure](https://msdn.microsoft.com/library/dn690523.aspx) e [cache gerenciado do Azure](https://msdn.microsoft.com/library/dn386094.aspx). O cache Redis do Azure baseia-se no popular [cache Redis](http://redis.io/) de software livre e é a primeira opção para a maioria dos cenários de cache.

<a id="sessionstate"></a>
## <a name="aspnet-session-state-using-a-cache-provider"></a>Estado de sessão ASP.NET usando um provedor de cache

Conforme mencionado no [capítulo práticas](web-development-best-practices.md)recomendadas de desenvolvimento para a Web, uma prática recomendada é evitar o uso do estado de sessão. Se seu aplicativo exigir o estado de sessão, a próxima prática recomendada é evitar o provedor padrão na memória, pois isso não habilita a expansão (várias instâncias do servidor Web). O provedor de estado de sessão de SQL Server ASP.NET permite que um site executado em vários servidores Web Use o estado de sessão, mas gera um custo de alta latência em comparação com um provedor na memória. A melhor solução se você precisa usar o estado de sessão é usar um provedor de cache, como o [provedor de estado de sessão para o cache do Azure](https://msdn.microsoft.com/library/windowsazure/gg185668.aspx).

## <a name="summary"></a>Resumo

Você viu como o aplicativo corrigir ti poderia implementar o Caching para melhorar o tempo de resposta e a escalabilidade, e para permitir que o aplicativo continue respondendo a operações de leitura quando o banco de dados não estiver disponível. No [próximo capítulo](queue-centric-work-pattern.md) , mostraremos como melhorar ainda mais a escalabilidade e fazer com que o aplicativo continue respondendo às operações de gravação.

## <a name="resources"></a>Recursos

Para obter mais informações sobre cache, consulte os recursos a seguir.

Documentação

- [Cache do Azure](https://msdn.microsoft.com/library/gg278356.aspx). Documentação oficial do MSDN sobre caching no Azure.
- [Práticas e padrões da Microsoft – diretrizes do Azure](https://msdn.microsoft.com/library/dn568099.aspx). Consulte Diretrizes de cache e padrão de Cache-Aside.
- [FailSafe: Diretrizes para arquiteturas de nuvem resilientes](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx). White Paper por Marc Mercuri, Ulrich Homann e Andrew TOWNHILL. Consulte a seção sobre Caching.
- [Práticas recomendadas para o design de serviços de Large-Scale nos serviços de nuvem do Azure](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). W. White Paper por Mark Simms e Michael Thomassy. Consulte a seção sobre Caching distribuído.
- [Caching distribuído no caminho para a escalabilidade](https://msdn.microsoft.com/magazine/dd942840.aspx). Um artigo mais antigo (2009) da MSDN Magazine, mas uma introdução claramente escrita ao Caching distribuído em geral; apresenta mais profundidade do que as seções de cache dos White papers de FailSafe e práticas recomendadas.

vídeos

- [FailSafe: criando serviços de nuvem escalonáveis e resilientes](https://channel9.msdn.com/Series/FailSafe). Série de nove partes de Ulrich Homann, Marc Mercuri e de Mark Simms. Apresenta uma exibição de nível 400 de como arquitetar aplicativos de nuvem. Esta série se concentra na teoria e nos motivos pelos quais; para obter mais detalhes, consulte a criação de séries grandes por marca Simms. Consulte a discussão em cache no episódio 3 a partir de 1:24:14.
- [Criando Big: lições aprendidas de clientes do Azure-parte I](https://channel9.msdn.com/Events/Build/2012/3-029). Simon Davies discute o cache distribuído a partir de 46:00. Semelhante à série FailSafe, mas apresenta detalhes mais detalhados. A apresentação foi fornecida em 31 de outubro de 2012 e, portanto, não abrange o serviço de cache de aplicativos Web no serviço Azure App que foi introduzido no 2013.

Exemplo de código

- [Conceitos básicos do serviço de nuvem no Azure](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Aplicativo de exemplo que implementa o cache distribuído. Consulte os conceitos básicos do serviço de nuvem de postagem do blog que acompanham [– noções básicas sobre Caching](https://blogs.msdn.com/b/windowsazure/archive/2013/10/03/cloud-service-fundamentals-caching-basics.aspx).

> [!div class="step-by-step"]
> [Anterior](transient-fault-handling.md) 
>  [Avançar](queue-centric-work-pattern.md)
