---
uid: web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
title: Host ASP.NET Web API 2 em uma função de trabalho do Azure-ASP.NET 4. x
author: MikeWasson
description: 'Tutorial: host ASP.NET Web API em uma função de trabalho do Azure, usando OWIN para hospedar automaticamente a estrutura da API Web.'
ms.author: riande
ms.date: 04/02/2014
ms.custom: seoapril2019
ms.assetid: 6980ee2e-d6b0-4a08-8fb6-ab96362dd0e3
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: 06bd33a7d30ab38d851d929d72ae8c539e16873c
ms.sourcegitcommit: 4b78855427f1397df0a7be3559e04ec94a78c308
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96151900"
---
# <a name="host-aspnet-web-api-2-in-an-azure-worker-role"></a>Host ASP.NET Web API 2 em uma função de trabalho do Azure

por [Mike Wasson](https://github.com/MikeWasson)

> Este tutorial mostra como hospedar ASP.NET Web API em uma função de trabalho do Azure, usando OWIN para hospedar internamente a estrutura da API Web.
>
> O [Open Web interface for .net](http://owin.org/) (OWIN) define uma abstração entre servidores Web e aplicativos Web do .net. OWIN dissocia o aplicativo Web do servidor, o que torna o OWIN ideal para hospedar internamente um aplicativo Web em seu próprio processo, fora do IIS – por exemplo, dentro de uma função de trabalho do Azure.
>
> Neste tutorial, você usará o pacote Microsoft. Owin. host. HttpListener, que fornece um servidor HTTP que será usado para hospedar aplicativos OWIN de hospedagem interna.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - API Web 2
> - [SDK do Azure para .NET 2.3](https://azure.microsoft.com/downloads/)

## <a name="create-a-microsoft-azure-project"></a>Criar um projeto Microsoft Azure

Inicie o Visual Studio com privilégios de administrador. São necessários privilégios de administrador para depurar o aplicativo localmente, usando o emulador de computação do Azure.

No menu **arquivo** , clique em **novo** e em **projeto**. Nos **modelos instalados**, em Visual C#, clique em **nuvem** e, em seguida, clique em **serviço de nuvem do Windows Azure**. Nomeie o projeto "AzureApp" e clique em **OK**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image2.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image1.png)

Na caixa de diálogo **novo serviço de nuvem do Windows Azure** , clique duas vezes em **função de trabalho**. Deixe o nome padrão ("WorkerRole1"). Esta etapa adiciona uma função de trabalho à solução. Clique em **OK**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image4.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image3.png)

A solução do Visual Studio criada contém dois projetos:

- &quot;AzureApp &quot; define as funções e a configuração para o aplicativo do Azure.
- &quot;WorkerRole1 &quot; contém o código para a função de trabalho.

Em geral, um aplicativo do Azure pode conter várias funções, embora este tutorial use uma única função.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-web-api-and-owin-packages"></a>Adicionar a API Web e os pacotes OWIN

No menu **ferramentas** , clique em **Gerenciador de pacotes NuGet** e em **console do Gerenciador de pacotes**.

Na janela Console do Gerenciador de Pacotes, digite o seguinte comando:

[!code-console[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a>Adicionar um ponto de extremidade HTTP

Em Gerenciador de Soluções, expanda o projeto AzureApp. Expanda o nó funções, clique com o botão direito do mouse em WorkerRole1 e selecione **Propriedades**.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image6.png)

Clique em **Pontos de Extremidade** e clique em **Adicionar Ponto de Extremidade**.

Na lista suspensa **protocolo** , selecione "http". Em porta **pública** e **porta privada**, digite 80. O número de porta pode ser diferente. A porta pública é o que os clientes usam quando enviam uma solicitação para a função.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image8.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image7.png)

## <a name="configure-web-api-for-self-host"></a>Configurar API Web para Self-Host

Em Gerenciador de soluções, clique com o botão direito do mouse no projeto WorkerRole1 e selecione **Adicionar**  /  **classe** para adicionar uma nova classe. Nome da classe `Startup`.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image9.png)

Substitua todo o código clichê deste arquivo pelo seguinte:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample2.cs)]

## <a name="add-a-web-api-controller"></a>Adicionar um controlador de API Web

Em seguida, adicione uma classe de controlador da API Web. Clique com o botão direito do mouse no projeto WorkerRole1 e selecione **Adicionar**  /  **classe**. Nomeie a classe TestController. Substitua todo o código clichê deste arquivo pelo seguinte:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample3.cs)]

Para simplificar, esse controlador define apenas dois métodos GET que retornam texto sem formatação.

## <a name="start-the-owin-host"></a>Iniciar o host OWIN

Abra o arquivo WorkerRole.cs. Essa classe define o código que é executado quando a função de trabalho é iniciada e interrompida.

Adicione a seguinte instrução usando:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample4.cs)]

Adicione um membro **IDisposable** à `WorkerRole` classe:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample5.cs)]

No `OnStart` método, adicione o seguinte código para iniciar o host:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample6.cs?highlight=5)]

O método **webapp. Start** inicia o host OWIN. O nome da `Startup` classe é um parâmetro de tipo para o método. Por convenção, o host chamará o `Configure` método dessa classe.

Substitua o `OnStop` para descartar da instância do *\_ aplicativo* :

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample7.cs)]

Este é o código completo para WorkerRole.cs:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample8.cs)]

Compile a solução e pressione F5 para executar o aplicativo localmente no emulador de computação do Azure. Dependendo das configurações do firewall, talvez seja necessário permitir o emulador por meio do firewall.

> [!NOTE]
> Se você receber uma exceção semelhante à seguinte, consulte [esta postagem de blog](https://blogs.msdn.com/b/praburaj/archive/2013/11/20/fileloadexception-on-microsoft-owin-when-running-on-worker-role.aspx) para obter uma solução alternativa. "Não foi possível carregar o arquivo ou o assembly ' Microsoft. Owin, Version = 2.0.2.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35 ' ou uma de suas dependências. A definição do manifesto do assembly localizado não corresponde à referência do assembly. (Exceção de HRESULT: 0x80131040) "

O emulador de computação atribui um endereço IP local ao ponto de extremidade. Você pode encontrar o endereço IP exibindo a interface do usuário do emulador de computação. Clique com o botão direito no ícone do emulador na área de notificação da barra de tarefas e selecione **Mostrar interface do usuário do emulador de computação**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image11.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image10.png)

Localize o endereço IP em implantações de serviço, implantação [ID], detalhes do serviço. Abra um navegador da Web e navegue até http://<em>Address</em>/Test/1, em que <em>Address</em> é o endereço IP atribuído pelo emulador de computação; por exemplo, `http://127.0.0.1:80/test/1` . Você deve ver a resposta do controlador da API Web:

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image12.png)

## <a name="deploy-to-azure"></a>Implantar no Azure

Para esta etapa, você deve ter uma conta do Azure. Se você ainda não tiver uma, poderá criar uma conta de avaliação gratuita em apenas alguns minutos. Para obter detalhes, consulte [Microsoft Azure avaliação gratuita](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).

Em Gerenciador de Soluções, clique com o botão direito do mouse no projeto AzureApp. Selecione **Publicar**.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image13.png)

Se você não estiver conectado à sua conta do Azure, clique em **entrar**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image15.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image14.png)

Depois de entrar, escolha uma assinatura e clique em **Avançar**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image17.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image16.png)

Insira um nome para o serviço de nuvem e escolha uma região. Clique em **Criar**.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image18.png)

Clique em **Publicar**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image20.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image19.png)

A janela log de atividades do Azure mostra o progresso da implantação. Quando o aplicativo for implantado, navegue até http://appname.cloudapp.net/test/1 .

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image21.png)

## <a name="additional-resources"></a>Recursos adicionais

- [Uma visão geral do projeto Katana](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)
- [Projeto Katana no GitHub](https://github.com/aspnet/AspNetKatana)
