---
uid: aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
title: Host OWIN em uma função de trabalho do Azure | Microsoft Docs
author: MikeWasson
description: Este tutorial mostra como hospedar automaticamente o OWIN em uma função de trabalho Microsoft Azure. Open Web interface para .NET (OWIN) define uma abstração entre o servidor Web do .NET...
ms.author: riande
ms.date: 04/11/2014
ms.assetid: 07aa855a-92ee-4d43-ba66-5bfd7de20ee6
msc.legacyurl: /aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: f4faeda4cc8428c54b761f30833f9a31edfd2ddb
ms.sourcegitcommit: 4b78855427f1397df0a7be3559e04ec94a78c308
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96151887"
---
# <a name="host-owin-in-an-azure-worker-role"></a>Hospedar OWIN em uma função de trabalho do Azure

por [Mike Wasson](https://github.com/MikeWasson)

> Este tutorial mostra como hospedar automaticamente o OWIN em uma função de trabalho Microsoft Azure.
>
> O [Open Web interface for .net](http://owin.org/) (OWIN) define uma abstração entre servidores Web e aplicativos Web do .net. OWIN dissocia o aplicativo Web do servidor, o que torna o OWIN ideal para hospedar internamente um aplicativo Web em seu próprio processo, fora do IIS – por exemplo, dentro de uma função de trabalho do Azure.
>
> Neste tutorial, você aprenderá a hospedar internamente um aplicativo OWIN dentro de uma função de trabalho Microsoft Azure. Para saber mais sobre as funções de trabalho, consulte [modelos de execução do Azure](https://azure.microsoft.com/documentation/articles/fundamentals-application-models/#CloudServices).
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - [SDK do Azure para .NET 2.3](https://azure.microsoft.com/downloads/)
> - [Microsoft. Owin. Selfhost 2.1.0](http://www.nuget.org/packages/Microsoft.Owin.SelfHost/2.1.0)

## <a name="create-a-microsoft-azure-project"></a>Criar um projeto Microsoft Azure

Inicie o Visual Studio com privilégios de administrador. São necessários privilégios de administrador para depurar o aplicativo localmente, usando o emulador de computação do Azure.

No menu **arquivo** , clique em **novo** e em **projeto**. Nos **modelos instalados**, em Visual C#, clique em **nuvem** e, em seguida, clique em **serviço de nuvem do Windows Azure**. Nomeie o projeto "AzureApp" e clique em **OK**.

[![](host-owin-in-an-azure-worker-role/_static/image2.png)](host-owin-in-an-azure-worker-role/_static/image1.png)

Na caixa de diálogo **novo serviço de nuvem do Windows Azure** , clique duas vezes em **função de trabalho**. Deixe o nome padrão ("WorkerRole1"). Esta etapa adiciona uma função de trabalho à solução. Clique em **OK**.

[![](host-owin-in-an-azure-worker-role/_static/image4.png)](host-owin-in-an-azure-worker-role/_static/image3.png)

A solução do Visual Studio criada contém dois projetos:

- &quot;AzureApp &quot; define as funções e a configuração para o aplicativo do Azure.
- &quot;WorkerRole1 &quot; contém o código para a função de trabalho.

Em geral, um aplicativo do Azure pode conter várias funções, embora este tutorial use uma única função.

![](host-owin-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-owin-self-host-packages"></a>Adicionar os pacotes de Self-Host OWIN

No menu **ferramentas** , clique em **Gerenciador de pacotes NuGet** e em **console do Gerenciador de pacotes**.

Na janela Console do Gerenciador de Pacotes, digite o seguinte comando:

[!code-console[Main](host-owin-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a>Adicionar um ponto de extremidade HTTP

Em Gerenciador de Soluções, expanda o projeto AzureApp. Expanda o nó funções, clique com o botão direito do mouse em WorkerRole1 e selecione **Propriedades**.

![](host-owin-in-an-azure-worker-role/_static/image6.png)

Clique em **Pontos de Extremidade** e clique em **Adicionar Ponto de Extremidade**.

Na lista suspensa **protocolo** , selecione "http". Em porta **pública** e **porta privada**, digite 80. O número de porta pode ser diferente. A porta pública é o que os clientes usam quando enviam uma solicitação para a função.

[![](host-owin-in-an-azure-worker-role/_static/image8.png)](host-owin-in-an-azure-worker-role/_static/image7.png)

## <a name="create-the-owin-startup-class"></a>Criar a classe de inicialização OWIN

Em Gerenciador de soluções, clique com o botão direito do mouse no projeto WorkerRole1 e selecione **Adicionar**  /  **classe** para adicionar uma nova classe. Nome da classe `Startup`.

Substitua todo o código clichê pelo seguinte:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample2.cs)]

O `UseWelcomePage` método de extensão adiciona uma página HTML simples ao seu aplicativo, para verificar se o site está funcionando.

## <a name="start-the-owin-host"></a>Iniciar o host OWIN

Abra o arquivo WorkerRole.cs. Essa classe define o código que é executado quando a função de trabalho é iniciada e interrompida.

Adicione a seguinte instrução usando:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample3.cs)]

Adicione um membro **IDisposable** à `WorkerRole` classe:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample4.cs)]

No `OnStart` método, adicione o seguinte código para iniciar o host:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample5.cs?highlight=5)]

O método **webapp. Start** inicia o host OWIN. O nome da `Startup` classe é um parâmetro de tipo para o método. Por convenção, o host chamará o `Configure` método dessa classe.

Substitua o `OnStop` para descartar da instância do *\_ aplicativo* :

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample6.cs)]

Este é o código completo para WorkerRole.cs:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample7.cs)]

Compile a solução e pressione F5 para executar o aplicativo localmente no emulador de computação do Azure. Dependendo das configurações do firewall, talvez seja necessário permitir o emulador por meio do firewall.

O emulador de computação atribui um endereço IP local ao ponto de extremidade. Você pode encontrar o endereço IP exibindo a interface do usuário do emulador de computação. Clique com o botão direito no ícone do emulador na área de notificação da barra de tarefas e selecione **Mostrar interface do usuário do emulador de computação**.

[![](host-owin-in-an-azure-worker-role/_static/image10.png)](host-owin-in-an-azure-worker-role/_static/image9.png)

Localize o endereço IP em implantações de serviço, implantação [ID], detalhes do serviço. Abra um navegador da Web e navegue até http: \/ \/ *endereço*, em que *endereço* é o endereço IP atribuído pelo emulador de computação; por exemplo, `http://127.0.0.1:80` . Você deverá ver a página de boas-vindas do OWIN:

![](host-owin-in-an-azure-worker-role/_static/image11.png)

## <a name="deploy-to-azure"></a>Implantar no Azure

Para esta etapa, você deve ter uma conta do Azure. Se você ainda não tiver uma, poderá criar uma conta de avaliação gratuita em apenas alguns minutos. Para obter detalhes, consulte [Microsoft Azure avaliação gratuita](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).

Em Gerenciador de Soluções, clique com o botão direito do mouse no projeto AzureApp. Selecione **Publicar**.

![](host-owin-in-an-azure-worker-role/_static/image12.png)

Se você não estiver conectado à sua conta do Azure, clique em **entrar**.

[![](host-owin-in-an-azure-worker-role/_static/image14.png)](host-owin-in-an-azure-worker-role/_static/image13.png)

Depois de entrar, escolha uma assinatura e clique em **Avançar**.

[![](host-owin-in-an-azure-worker-role/_static/image16.png)](host-owin-in-an-azure-worker-role/_static/image15.png)

Insira um nome para o serviço de nuvem e escolha uma região. Clique em **Criar**.

![](host-owin-in-an-azure-worker-role/_static/image17.png)

Clique em **Publicar**.

[![](host-owin-in-an-azure-worker-role/_static/image19.png)](host-owin-in-an-azure-worker-role/_static/image18.png)

A janela log de atividades do Azure mostra o progresso da implantação. Quando o aplicativo for implantado, navegue até `http://appname.cloudapp.net/` , em que *AppName* é o nome do seu serviço de nuvem.

## <a name="additional-resources"></a>Recursos adicionais

- [Uma visão geral do projeto Katana](an-overview-of-project-katana.md)
- [Projeto Katana no GitHub](https://github.com/aspnet/AspNetKatana/)
