---
uid: signalr/overview/security/introduction-to-security
title: Introdução à segurança do Signalr | Microsoft Docs
author: bradygaster
description: Descreve os problemas de segurança que você deve considerar ao desenvolver um aplicativo Signalr.
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: ed562717-8591-4936-8e10-c7e63dcb570a
msc.legacyurl: /signalr/overview/security/introduction-to-security
msc.type: authoredcontent
ms.openlocfilehash: 24ce20b45543468de28ad017ba62d2f6e5a00f3b
ms.sourcegitcommit: c62ec20b453cee3249eb894ecd75013b57d078f0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/21/2020
ms.locfileid: "92345440"
---
# <a name="introduction-to-signalr-security"></a>Introdução à segurança do SignalR

por [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Este artigo descreve os problemas de segurança que você deve considerar ao desenvolver um aplicativo Signalr.
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

## <a name="overview"></a>Visão geral

Este documento contém as seguintes seções:

- [Conceitos de segurança do signalr](#concepts)

    - [Autenticação e autorização](#authentication)
    - [Token de conexão](#connectiontoken)
    - [Reassociando grupos ao reconectar](#rejoingroup)
- [Como o Signalr impede falsificação de solicitação entre sites](#csrf)
- [Recomendações de segurança do signalr](#recommendations)

    - [Protocolo SSL (Secure Socket Layers)](#ssl)
    - [Não use grupos como um mecanismo de segurança](#groupsecurity)
    - [Manipulando com segurança a entrada de clientes](#input)
    - [Reconciliando uma alteração no status do usuário com uma conexão ativa](#reconcile)
    - [Arquivos de proxy JavaScript gerados automaticamente](#autogen)
    - [Exceções](#exceptions)

<a id="concepts"></a>

## <a name="signalr-security-concepts"></a>Conceitos de segurança do signalr

<a id="authentication"></a>

### <a name="authentication-and-authorization"></a>Autenticação e autorização

O signalr não fornece nenhum recurso para autenticar usuários. Em vez disso, você integra os recursos do Signalr à estrutura de autenticação existente para um aplicativo. Você autentica os usuários como faria normalmente em seu aplicativo e trabalha com os resultados da autenticação em seu código de sinalização. Por exemplo, você pode autenticar seus usuários com a autenticação de formulários do ASP.NET e, em seguida, em seu hub, impor quais usuários ou funções estão autorizados a chamar um método. No Hub, você também pode passar informações de autenticação, como nome de usuário ou se um usuário pertence a uma função, ao cliente.

O signalr fornece o atributo [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) para especificar quais usuários têm acesso a um Hub ou método. Aplique o atributo autorizar a um Hub ou métodos específicos em um Hub. Sem o atributo Authorize, todos os métodos públicos no Hub estão disponíveis para um cliente que está conectado ao Hub. Para obter mais informações sobre hubs, consulte [autenticação e autorização para hubs do signalr](hub-authorization.md).

Você aplica o `Authorize` atributo a hubs, mas não a conexões persistentes. Para impor regras de autorização ao usar um `PersistentConnection` , você deve substituir o `AuthorizeRequest` método. Para obter mais informações sobre conexões persistentes, consulte [autenticação e autorização para conexões persistentes do signalr](persistent-connection-authorization.md).

<a id="connectiontoken"></a>

### <a name="connection-token"></a>Token de conexão

O signalr reduz o risco de executar comandos mal-intencionados validando a identidade do remetente. Para cada solicitação, o cliente e o servidor passam um token de conexão que contém a ID de conexão e o nome de usuário para usuários autenticados. A ID de conexão identifica exclusivamente cada cliente conectado. O servidor gera aleatoriamente a ID de conexão quando uma nova conexão é criada e persiste essa ID pela duração da conexão. O mecanismo de autenticação para o aplicativo Web fornece o nome de usuário. O signalr usa criptografia e uma assinatura digital para proteger o token de conexão.

![](introduction-to-security/_static/image2.png)

Para cada solicitação, o servidor valida o conteúdo do token para garantir que a solicitação seja proveniente do usuário especificado. O nome de usuário deve corresponder à ID de conexão. Ao validar a ID de conexão e o nome de usuário, o Signalr impede que um usuário mal-intencionado represente outro usuário. Se o servidor não puder validar o token de conexão, a solicitação falhará.

![](introduction-to-security/_static/image4.png)

Como a ID de conexão faz parte do processo de verificação, você não deve revelar a ID de conexão de um usuário para outros usuários ou armazenar o valor no cliente, como em um cookie.

#### <a name="connection-tokens-vs-other-token-types"></a>Tokens de conexão versus outros tipos de token

Os tokens de conexão são ocasionalmente sinalizados por ferramentas de segurança porque parecem ser tokens de sessão ou tokens de autenticação, o que representa um risco, se exposto.

O token de conexão do signalr não é um token de autenticação. Ele é usado para confirmar que o usuário que está fazendo essa solicitação é o mesmo que criou a conexão. O token de conexão é necessário porque o sinalizador ASP.NET permite que as conexões se movam entre servidores. O token associa a conexão a um usuário específico, mas não declara a identidade do usuário que faz a solicitação. Para que uma solicitação de Signalr seja autenticada corretamente, ela deve ter algum outro token que declara a identidade do usuário, como um cookie ou um token de portador. No entanto, o próprio token de conexão não faz nenhuma declaração de que a solicitação foi feita por esse usuário, apenas que a ID de conexão contida no token está associada a esse usuário.

Como o token de conexão não fornece nenhuma declaração de autenticação por conta própria, ele não é considerado um token "sessão" ou "autenticação". Levar um determinado token de conexão do usuário e reproduzi-lo em uma solicitação autenticada como um usuário diferente (ou uma solicitação não autenticada) falhará, pois a identidade do usuário da solicitação e a identidade armazenada no token não corresponderão.

<a id="rejoingroup"></a>

### <a name="rejoining-groups-when-reconnecting"></a>Reassociando grupos ao reconectar

Por padrão, o aplicativo Signalr automaticamente reatribuirá um usuário aos grupos apropriados ao se reconectar de uma interrupção temporária, como quando uma conexão é descartada e restabelecida antes que a conexão expire. Ao reconectar, o cliente passa um token de grupo que inclui a ID de conexão e os grupos atribuídos. O token do grupo é assinado digitalmente e criptografado. O cliente retém a mesma ID de conexão após uma reconexão; Portanto, a ID de conexão passada do cliente reconectado deve corresponder à ID de conexão anterior usada pelo cliente. Essa verificação impede que um usuário mal-intencionado passe solicitações para ingressar grupos não autorizados ao se reconectar.

No entanto, é importante observar que o token do grupo não expira. Se um usuário pertencia a um grupo no passado, mas foi banido desse grupo, esse usuário pode ser capaz de imitar um token de grupo que inclui o grupo proibido. Se você precisar gerenciar com segurança quais usuários pertencem a quais grupos, precisará armazenar esses dados no servidor, como em um banco de dado. Em seguida, adicione lógica ao seu aplicativo que verifica no servidor se um usuário pertence a um grupo. Para obter um exemplo de como verificar a associação de grupo, consulte [trabalhando com grupos](../guide-to-the-api/working-with-groups.md).

A rejunção automática de grupos só se aplica quando uma conexão é reconectada após uma interrupção temporária. Se um usuário se desconecta navegando para fora do aplicativo ou o aplicativo é reiniciado, seu aplicativo deve manipular como adicionar esse usuário aos grupos corretos. Para obter mais informações, consulte [trabalhando com grupos](../guide-to-the-api/working-with-groups.md).

<a id="csrf"></a>

## <a name="how-signalr-prevents-cross-site-request-forgery"></a>Como o Signalr impede falsificação de solicitação entre sites

A CSRF (solicitação intersite forjada) é um ataque em que um site mal-intencionado envia uma solicitação para um site vulnerável em que o usuário está conectado no momento. O signalr impede o CSRF, tornando extremamente improvável que um site mal-intencionado crie uma solicitação válida para seu aplicativo Signalr.

### <a name="description-of-csrf-attack"></a>Descrição do ataque de CSRF

Aqui está um exemplo de um ataque de CSRF:

1. Um usuário faz logon no www.example.com, usando a autenticação de formulários.
2. O servidor autentica o usuário. A resposta do servidor inclui um cookie de autenticação.
3. Sem fazer logoff, o usuário visita um site mal-intencionado. Este site mal-intencionado contém o seguinte formulário HTML:

    [!code-html[Main](introduction-to-security/samples/sample1.html)]

   Observe que a ação do formulário é postagem no site vulnerável, não no site mal-intencionado. Esta é a parte "entre sites" do CSRF.
4. O usuário clica no botão enviar. O navegador inclui o cookie de autenticação com a solicitação.
5. A solicitação é executada no servidor example.com com o contexto de autenticação do usuário e pode fazer tudo o que um usuário autenticado tem permissão para fazer.

Embora este exemplo exija que o usuário clique no botão formulário, a página mal-intencionada poderia simplesmente executar um script que envia uma solicitação AJAX para seu aplicativo Signalr. Além disso, o uso do SSL não impede um ataque CSRF, porque o site mal-intencionado pode enviar uma solicitação "https://".

Normalmente, os ataques de CSRF são possíveis em sites que usam cookies para autenticação, pois os navegadores enviam todos os cookies relevantes para o site de destino. No entanto, os ataques CSRF não estão limitados à exploração de cookies. Por exemplo, a autenticação básica e resumida também são vulneráveis. Depois que um usuário faz logon com a autenticação básica ou resumida, o navegador envia automaticamente as credenciais até que a sessão termine.

### <a name="csrf-mitigations-taken-by-signalr"></a>Mitigações CSRF tomadas pelo Signalr

O signalr executa as seguintes etapas para impedir que um site mal-intencionado crie solicitações válidas para seu aplicativo. O signalr executa essas etapas por padrão, você não precisa realizar nenhuma ação em seu código.

- **Desabilitar solicitações entre domínios** O signalr desabilita as solicitações entre domínios para impedir que os usuários chamem um ponto de extremidade de sinalização de um domínio externo. O signalr considera qualquer solicitação de um domínio externo como inválida e bloqueia a solicitação. Recomendamos que você mantenha esse comportamento padrão; caso contrário, um site mal-intencionado pode induzir os usuários a enviar comandos para seu site. Se você precisar usar solicitações entre domínios, consulte     [como estabelecer uma conexão entre domínios](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain) .
- **Passar token de conexão na cadeia de caracteres de consulta, não cookie** O signalr passa o token de conexão como um valor de cadeia de caracteres de consulta, em vez de um cookie. O armazenamento do token de conexão em um cookie não é seguro porque o navegador pode encaminhar inadvertidamente o token de conexão quando um código mal-intencionado é encontrado. Além disso, passar o token de conexão na cadeia de caracteres de consulta impede que o token de conexão persista além da conexão atual. Portanto, um usuário mal-intencionado não pode fazer uma solicitação sob as credenciais de autenticação de outro usuário.
- **Verificar token de conexão** Conforme descrito na seção     [token de conexão](#connectiontoken) , o servidor sabe qual ID de conexão está associada a cada usuário autenticado. O servidor não processa nenhuma solicitação de uma ID de conexão que não corresponda ao nome de usuário. É improvável que um usuário mal-intencionado possa adivinhar uma solicitação válida porque o usuário mal-intencionado teria que saber o nome de usuário e a ID de conexão gerada aleatoriamente. Essa ID de conexão torna-se inválida assim que a conexão é encerrada. Os usuários anônimos não devem ter acesso a informações confidenciais.

<a id="recommendations"></a>

## <a name="signalr-security-recommendations"></a>Recomendações de segurança do signalr

<a id="ssl"></a>

### <a name="secure-socket-layers-ssl-protocol"></a>Protocolo SSL (Secure Socket Layers)

O protocolo SSL usa criptografia para proteger o transporte de dados entre um cliente e um servidor. Se o seu aplicativo Signalr transmite informações confidenciais entre o cliente e o servidor, use SSL para o transporte. Para obter mais informações sobre como configurar o SSL, consulte [como configurar o SSL no IIS 7](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).

<a id="groupsecurity"></a>

### <a name="do-not-use-groups-as-a-security-mechanism"></a>Não use grupos como um mecanismo de segurança

Os grupos são uma maneira conveniente de coletar usuários relacionados, mas não são um mecanismo seguro para limitar o acesso a informações confidenciais. Isso é especialmente verdadeiro quando os usuários podem reingressar grupos automaticamente durante uma reconexão. Em vez disso, considere adicionar usuários privilegiados a uma função e limitar o acesso a um método de Hub somente a membros dessa função. Para obter um exemplo de restrição de acesso com base em uma função, consulte [autenticação e autorização para hubs do signalr](hub-authorization.md). Para obter um exemplo de como verificar o acesso do usuário a grupos ao reconectar-se, consulte [trabalhando com grupos](../guide-to-the-api/working-with-groups.md).

<a id="input"></a>

### <a name="safely-handling-input-from-clients"></a>Manipulando com segurança a entrada de clientes

Para garantir que um usuário mal-intencionado não envie um script para outros usuários, você deve codificar todas as entradas de clientes que se destinam à transmissão para outros clientes. Você deve codificar mensagens nos clientes de recebimento em vez de no servidor, pois o aplicativo Signalr pode ter muitos tipos diferentes de clientes. Portanto, a codificação HTML funciona para um cliente Web, mas não para outros tipos de clientes. Por exemplo, um método de cliente Web para exibir uma mensagem de chat trataria com segurança o nome de usuário e a mensagem chamando a `html()` função.

[!code-html[Main](introduction-to-security/samples/sample2.html?highlight=3-4)]

<a id="reconcile"></a>

### <a name="reconciling-a-change-in-user-status-with-an-active-connection"></a>Reconciliando uma alteração no status do usuário com uma conexão ativa

Se o status de autenticação de um usuário for alterado enquanto houver uma conexão ativa, o usuário receberá um erro afirmando que "a identidade do usuário não pode ser alterada durante uma conexão de sinalização ativa." Nesse caso, seu aplicativo deve se reconectar ao servidor para verificar se a ID de conexão e o nome de usuário são coordenados. Por exemplo, se o seu aplicativo permitir que o usuário faça logoff enquanto uma conexão ativa existir, o nome de usuário da conexão não corresponderá mais ao que é passado para a próxima solicitação. Você desejará parar a conexão antes de o usuário fazer logoff e, em seguida, reiniciá-la.

No entanto, é importante observar que a maioria dos aplicativos não precisará parar e iniciar a conexão manualmente. Se o seu aplicativo redireciona os usuários para uma página separada após o logout, como o comportamento padrão em um aplicativo Web Forms ou aplicativo MVC, ou atualiza a página atual após o logout, a conexão ativa é automaticamente desconectada e não requer nenhuma ação adicional.

O exemplo a seguir mostra como parar e iniciar uma conexão quando o status do usuário é alterado.

[!code-html[Main](introduction-to-security/samples/sample3.html)]

Ou, o status de autenticação do usuário poderá ser alterado se o seu site usar a expiração deslizante com autenticação de formulários e não houver nenhuma atividade para manter o cookie de autenticação válido. Nesse caso, o usuário será desconectado e o nome de usuário não corresponderá mais ao nome de usuário no token de conexão. Você pode corrigir esse problema adicionando um script que solicita periodicamente um recurso no servidor Web para manter o cookie de autenticação válido. O exemplo a seguir mostra como solicitar um recurso a cada 30 minutos.

[!code-javascript[Main](introduction-to-security/samples/sample4.js)]

<a id="autogen"></a>

### <a name="automatically-generated-javascript-proxy-files"></a>Arquivos de proxy JavaScript gerados automaticamente

Se você não quiser incluir todos os hubs e métodos no arquivo de proxy JavaScript para cada usuário, poderá desabilitar a geração automática do arquivo. Você pode escolher essa opção se tiver vários hubs e métodos, mas não quiser que todos os usuários estejam cientes de todos os métodos. Você desabilita a geração automática definindo **EnableJavaScriptProxies** como **false**.

[!code-csharp[Main](introduction-to-security/samples/sample5.cs)]

Para obter mais informações sobre os arquivos de proxy JavaScript, consulte [o proxy gerado e o que ele faz para você](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy). <a id="exceptions"></a>

### <a name="exceptions"></a>Exceções

Você deve evitar passar objetos de exceção para os clientes, pois os objetos podem expor informações confidenciais aos clientes. Em vez disso, chame um método no cliente que exibe a mensagem de erro relevante.

[!code-csharp[Main](introduction-to-security/samples/sample6.cs)]
