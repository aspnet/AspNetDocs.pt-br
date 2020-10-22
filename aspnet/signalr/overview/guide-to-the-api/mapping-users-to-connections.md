---
uid: signalr/overview/guide-to-the-api/mapping-users-to-connections
title: Mapeando usuários do Signalr para conexões | Microsoft Docs
author: bradygaster
description: Este tópico mostra como reter informações sobre usuários e suas conexões. Patrick Fletcher ajudou a escrever este tópico. Versões de software usadas neste tópico...
ms.author: bradyg
ms.date: 12/30/2014
ms.assetid: f80c08b1-3f1f-432c-980c-c7b6edeb31b1
msc.legacyurl: /signalr/overview/guide-to-the-api/mapping-users-to-connections
msc.type: authoredcontent
ms.openlocfilehash: d55d40848e1e9d40570850c3552b225235c5e814
ms.sourcegitcommit: c62ec20b453cee3249eb894ecd75013b57d078f0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/21/2020
ms.locfileid: "92345413"
---
# <a name="mapping-signalr-users-to-connections"></a>Mapeamento de usuários do SignalR para conexões

 por [Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Este tópico mostra como reter informações sobre usuários e suas conexões.
>
> Patrick Fletcher ajudou a escrever este tópico.
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

## <a name="introduction"></a>Introdução

Cada cliente que se conecta a um hub passa uma ID de conexão exclusiva. Você pode recuperar esse valor na `Context.ConnectionId` Propriedade do contexto do Hub. Se seu aplicativo precisar mapear um usuário para a ID de conexão e persistir esse mapeamento, você poderá usar um dos seguintes:

- [O provedor de ID de usuário (Signalr 2)](#IUserIdProvider)
- [Armazenamento na memória](#inmemory), como um dicionário
- [Grupo de signalr para cada usuário](#groups)
- [Armazenamento externo permanente](#database), como uma tabela de banco de dados ou armazenamento de tabelas do Azure

Cada uma dessas implementações é mostrada neste tópico. Você usa os `OnConnected` `OnDisconnected` métodos, e `OnReconnected` da `Hub` classe para acompanhar o status de conexão do usuário.

A melhor abordagem para seu aplicativo depende de:

- O número de servidores Web que hospedam seu aplicativo.
- Se você precisa obter uma lista dos usuários conectados no momento.
- Se você precisa manter as informações do grupo e do usuário quando o aplicativo ou o servidor é reiniciado.
- Se a latência de chamada de um servidor externo é um problema.

A tabela a seguir mostra qual abordagem funciona para essas considerações.

|  | Mais de um servidor | Obter lista de usuários conectados no momento | Manter informações após reinicializações | Desempenho ideal |
| --- | --- | --- | --- | --- |
| Provedor de UserID | ![](mapping-users-to-connections/_static/image1.png) |  |  | ![](mapping-users-to-connections/_static/image2.png) |
| Na memória |  | ![](mapping-users-to-connections/_static/image3.png) |  | ![](mapping-users-to-connections/_static/image4.png) |
| Grupos de usuário único | ![](mapping-users-to-connections/_static/image5.png) |  |  | ![](mapping-users-to-connections/_static/image6.png) |
| Permanente, externo | ![](mapping-users-to-connections/_static/image7.png) | ![](mapping-users-to-connections/_static/image8.png) | ![](mapping-users-to-connections/_static/image9.png) |  |

<a id="IUserIdProvider"></a>

## <a name="iuserid-provider"></a>Provedor de IUserID

Esse recurso permite que os usuários especifiquem o que a userId se baseia em um IRequest por meio de uma nova interface IUserIdProvider.

**O IUserIdProvider**

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

Por padrão, haverá uma implementação que usa o usuário `IPrincipal.Identity.Name` como o nome de usuário. Para alterar isso, registre sua implementação do `IUserIdProvider` com o host global quando seu aplicativo for iniciado:

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

De dentro de um Hub, você poderá enviar mensagens para esses usuários por meio da seguinte API:

**Enviando uma mensagem para um usuário específico**

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs?highlight=5)]

<a id="inmemory"></a>

## <a name="in-memory-storage"></a>Armazenamento na memória

Os exemplos a seguir mostram como manter as informações de conexão e de usuário em um dicionário armazenado na memória. O dicionário usa um `HashSet` para armazenar a ID de conexão. A qualquer momento, um usuário pode ter mais de uma conexão com o aplicativo Signalr. Por exemplo, um usuário que está conectado por meio de vários dispositivos ou mais de uma guia do navegador teria mais de uma ID de conexão.

Se o aplicativo for desligado, todas as informações serão perdidas, mas ele será preenchido novamente à medida que os usuários restabelecerem suas conexões. O armazenamento na memória não funcionará se o seu ambiente incluir mais de um servidor Web, pois cada servidor teria uma coleção separada de conexões.

O primeiro exemplo mostra uma classe que gerencia o mapeamento de usuários para conexões. A chave do HashSet será o nome do usuário.

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

O exemplo a seguir mostra como usar a classe de mapeamento de conexão de um Hub. A instância da classe é armazenada em um nome de variável `_connections` .

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a>Grupos de usuário único

Você pode criar um grupo para cada usuário e, em seguida, enviar uma mensagem para esse grupo quando quiser acessar apenas esse usuário. O nome de cada grupo é o nome do usuário. Se um usuário tiver mais de uma conexão, cada ID de conexão será adicionada ao grupo do usuário.

Você não deve remover manualmente o usuário do grupo quando o usuário se desconecta. Essa ação é executada automaticamente pela estrutura do Signalr.

O exemplo a seguir mostra como implementar grupos de usuário único.

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a>Armazenamento externo permanente

Este tópico mostra como usar um banco de dados ou o armazenamento de tabelas do Azure para armazenar informações de conexão. Essa abordagem funciona quando você tem vários servidores Web, pois cada servidor Web pode interagir com o mesmo repositório de dados. Se os servidores Web deixarem de funcionar ou se o aplicativo for reiniciado, o `OnDisconnected` método não será chamado. Portanto, é possível que seu repositório de dados tenha registros de IDs de conexão que não são mais válidos. Para limpar esses registros órfãos, talvez você queira invalidar qualquer conexão criada fora de um período de tempo que seja relevante para seu aplicativo. Os exemplos nesta seção incluem um valor para controlar quando a conexão foi criada, mas não mostram como limpar registros antigos porque talvez você queira fazer isso como um processo em segundo plano.

### <a name="database"></a>Banco de dados

Os exemplos a seguir mostram como manter as informações de conexão e de usuário em um banco de dados. Você pode usar qualquer tecnologia de acesso a dados; no entanto, o exemplo a seguir mostra como definir modelos usando Entity Framework. Esses modelos de entidade correspondem a tabelas e campos de banco de dados. Sua estrutura de dados pode variar consideravelmente dependendo dos requisitos do seu aplicativo.

O primeiro exemplo mostra como definir uma entidade de usuário que pode ser associada a muitas entidades de conexão.

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]

Em seguida, no Hub, você pode acompanhar o estado de cada conexão com o código mostrado abaixo.

[!code-csharp[Main](mapping-users-to-connections/samples/sample8.cs)]

<a id="azure"></a>
### <a name="azure-table-storage"></a>Armazenamento de tabelas do Azure

O exemplo de armazenamento de tabela do Azure a seguir é semelhante ao exemplo de banco de dados. Ele não inclui todas as informações que você precisa para começar a usar o serviço de armazenamento de tabelas do Azure. Para obter informações, consulte [como usar o armazenamento de tabelas do .net](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).

O exemplo a seguir mostra uma entidade de tabela para armazenar informações de conexão. Ele particiona os dados por nome de usuário e identifica cada entidade pela ID de conexão, para que um usuário possa ter várias conexões a qualquer momento.

[!code-csharp[Main](mapping-users-to-connections/samples/sample9.cs)]

No Hub, você controla o status da conexão de cada usuário.

[!code-csharp[Main](mapping-users-to-connections/samples/sample10.cs)]
