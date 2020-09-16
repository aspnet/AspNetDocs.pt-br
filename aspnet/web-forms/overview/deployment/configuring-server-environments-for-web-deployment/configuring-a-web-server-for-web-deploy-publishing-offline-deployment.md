---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment
title: Configurando um servidor Web para publicação de Implantação da Web (implantação offline) | Microsoft Docs
author: jrjlee
description: Este tópico descreve como configurar um servidor Web do IIS para dar suporte à publicação e à implantação da Web offline. Quando você trabalha com Serviços de Informações da Internet (I...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: ba92788f-9f03-44b1-b6b2-af8413e6a35d
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment
msc.type: authoredcontent
ms.openlocfilehash: ed11720e6ea00df22f58d9afd32ccce95a5d1a60
ms.sourcegitcommit: 4ed0b65ae32d9f35e42ee6296b877747e063df4d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/16/2020
ms.locfileid: "90609667"
---
# <a name="configuring-a-web-server-for-web-deploy-publishing-offline-deployment"></a>Configuração de um servidor Web para publicação de Implantação da Web (implantação offline)

por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tópico descreve como configurar um servidor Web do IIS para dar suporte à publicação e à implantação da Web offline.
> 
> Quando você trabalha com a ferramenta de implantação da Web do Serviços de Informações da Internet (IIS) (Implantação da Web) 2,0 ou posterior, há três abordagens principais que você pode usar para colocar seus aplicativos ou sites em um servidor Web. Você pode:
> 
> - Use o *implantação da Web serviço de agente remoto*. Essa abordagem requer menos configuração do servidor Web, mas você precisa fornecer as credenciais de um administrador de servidor local para implantar qualquer coisa no servidor.
> - Use o *manipulador de implantação da Web*. Essa abordagem é muito mais complexa e exige mais esforço inicial para configurar o servidor Web. No entanto, ao usar essa abordagem, você pode configurar o IIS para permitir que usuários não administradores executem a implantação. O manipulador de Implantação da Web só está disponível no IIS versão 7 ou posterior.
> - Use a *implantação offline*. Essa abordagem requer a configuração mínima do servidor Web, mas um administrador de servidor deve copiar manualmente o pacote da Web no servidor e importá-lo por meio do Gerenciador do IIS.
> 
> Para obter mais informações sobre os principais recursos, as vantagens e as desvantagens dessas abordagens, consulte [escolhendo a abordagem certa para a implantação da Web](choosing-the-right-approach-to-web-deployment.md).

Sim, se sua infraestrutura de rede ou restrições de segurança impedirem a implantação remota. É mais provável que esse seja o caso em ambientes de produção voltados para a Internet, em que os servidores Web são isolados&#x2014;fisicamente ou por firewalls e sub-redes&#x2014;do restante da sua infraestrutura de servidor.

Obviamente, essa abordagem se tornará menos desejável se seus aplicativos Web forem atualizados regularmente. Se sua infraestrutura permitir, convém considerar a habilitação da implantação remota, usando o manipulador de Implantação da Web ou o serviço Implantação da Web agente remoto.

## <a name="task-overview"></a>Visão geral da tarefa

Para configurar o servidor Web para dar suporte à importação e implantação offline de pacotes da Web, você precisará:

- Instale o IIS 7,5 e a configuração recomendada do IIS 7.
- Instale o Implantação da Web 2,1 ou posterior.
- Crie um site do IIS para hospedar o conteúdo implantado.
- Desabilite o serviço Web Deployment Agent.

Para hospedar a solução de exemplo especificamente, você também precisará:

- Instale o .NET Framework 4,0.
- Instale o ASP.NET MVC 3.

Este tópico mostrará como executar cada um desses procedimentos. As tarefas e orientações neste tópico pressupõem que você está começando com uma compilação de servidor limpa que executa o Windows Server 2008 R2. Antes de continuar, verifique se:

- O Windows Server 2008 R2 Service Pack 1 e todas as atualizações disponíveis estão instaladas.
- O servidor está ingressado no domínio.
- O servidor tem um endereço IP estático.

> [!NOTE]
> Para obter mais informações sobre como unir computadores a um domínio, consulte [unindo computadores ao domínio e fazendo logon](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx). Para obter mais informações sobre como configurar endereços IP estáticos, consulte [configurar um endereço IP estático](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx).

## <a name="install-products-and-components"></a>Instalar produtos e componentes

Esta seção irá orientá-lo na instalação dos produtos e componentes necessários no servidor Web. Antes de começar, uma prática recomendada é executar Windows Update para garantir que seu servidor esteja totalmente atualizado.

Nesse caso, você precisa instalar estas coisas:

- **Configuração recomendada do IIS 7**. Isso habilita a função do **servidor Web (IIS)** no servidor Web e instala o conjunto de módulos e componentes do IIS que você precisa para hospedar um aplicativo ASP.net.
- **.NET Framework 4,0**. Isso é necessário para executar aplicativos que foram criados nesta versão do .NET Framework.
- **Ferramenta de implantação da Web 2,1 ou posterior**. Isso instala Implantação da Web (e seu executável subjacente, MSDeploy.exe) em seu servidor. O Implantação da Web integra-se ao IIS e permite que você importe e exporte pacotes da Web.
- **ASP.NET MVC 3**. Isso instala os assemblies necessários para executar os aplicativos MVC 3.

> [!NOTE]
> Este tutorial descreve o uso do Web Platform Installer para instalar e configurar vários componentes. Embora você não precise usar o Web Platform Installer, ele simplifica o processo de instalação detectando automaticamente as dependências e garantindo que você sempre obtenha as versões mais recentes dos produtos. Para obter mais informações, consulte [Microsoft Web Platform Installer 3,0](https://go.microsoft.com/?linkid=9805118).

**Para instalar os produtos e componentes necessários**

1. Baixe e instale o [Web Platform Installer](https://go.microsoft.com/?linkid=9805118).
2. Quando a instalação for concluída, a Web Platform Installer será iniciada automaticamente.

    > [!NOTE]
    > Agora você pode iniciar o Web Platform Installer a qualquer momento no menu **Iniciar** . Para fazer isso, no menu **Iniciar** , clique em **todos os programas**e, em seguida, clique em **Microsoft Web Platform Installer**.
3. Na parte superior da janela **Web Platform Installer 3,0** , clique em **produtos**.
4. No lado esquerdo da janela, no painel de navegação, clique em **estruturas**.
5. Na linha **Microsoft .NET Framework 4** , se o .NET Framework ainda não estiver instalado, clique em **Adicionar**.

    > [!NOTE]
    > Talvez você já tenha instalado o .NET Framework 4,0 por meio de Windows Update. Se um produto ou componente já estiver instalado, o Web Platform Installer indicará isso substituindo o botão **Adicionar** pelo texto **instalado**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image1.png)
6. Na linha **ASP.NET MVC 3 (Visual Studio 2010)** , clique em **Adicionar**.
7. No painel de navegação, clique em **servidor**.
8. Na linha de **configuração recomendada do IIS 7** , clique em **Adicionar**.
9. Na linha **ferramenta de implantação da Web 2,1** , clique em **Adicionar**.
10. Clique em **Instalar**. O Web Platform Installer mostrará uma lista de produtos&#x2014;junto com as dependências associadas&#x2014;ser instalado e solicitará que você aceite os termos de licença.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image2.png)
11. Examine os termos de licença e, se você concordar com os termos, clique em **aceito**.
12. Quando a instalação for concluída, clique em **concluir**e feche a janela **Web Platform Installer 3,0** .

Se você instalou o .NET Framework 4,0 antes de instalar o IIS, precisará executar a [ferramenta de registro do ASP.net IIS](https://msdn.microsoft.com/library/k6h9cz8h(v=VS.100).aspx) (ASPNET \_regiis.exe) para registrar a versão mais recente do ASP.NET com o IIS. Se você não fizer isso, descobrirá que o IIS fornecerá conteúdo estático (como arquivos HTML) sem nenhum problema, mas retornará o **erro HTTP 404,0 – não encontrado** quando você tentar navegar até o conteúdo do ASP.net. Você pode usar o procedimento a seguir para garantir que o ASP.NET 4,0 seja registrado.

**Para registrar o ASP.NET 4,0 com o IIS**

1. Clique em **Iniciar**e digite **prompt de comando**.
2. Nos resultados da pesquisa, clique com o botão direito do mouse em **prompt de comando**e clique em **Executar como administrador**.
3. Na janela do prompt de comando, navegue até o diretório **%windir%\Microsoft.NET\Framework\v4.0.30319**
4. Digite este comando e pressione ENTER:

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/samples/sample1.cmd)]
5. Se você planeja hospedar aplicativos Web de 64 bits a qualquer momento, você também deve registrar a versão de 64 bits do ASP.NET com o IIS. Para fazer isso, na janela de prompt de comando, navegue até o diretório **%windir%\Microsoft.NET\Framework64\v4.0.30319** .
6. Digite este comando e pressione ENTER:

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/samples/sample2.cmd)]

Como uma prática recomendada, use Windows Update novamente neste ponto para baixar e instalar todas as atualizações disponíveis para os novos produtos e componentes que você instalou.

## <a name="configure-the-iis-website"></a>Configurar o site do IIS

Antes de implantar o conteúdo da Web em seu servidor, você precisa criar e configurar um site do IIS para hospedar o conteúdo. Implantação da Web só pode implantar pacotes da Web em um site do IIS existente; Ele não pode criar o site para você. Em um alto nível, você precisará concluir estas tarefas:

- Crie uma pasta no sistema de arquivos para hospedar seu conteúdo.
- Crie um site do IIS para fornecer o conteúdo e associe-o à pasta local.
- Conceda permissões de leitura à identidade do pool de aplicativos na pasta local.

Embora não haja nada para impedir que você implante conteúdo no site padrão no IIS, essa abordagem não é recomendada para nada além de cenários de teste ou demonstração. Para simular um ambiente de produção, você deve criar um novo site do IIS com configurações específicas aos requisitos do seu aplicativo.

**Para criar e configurar um site do IIS**

1. No sistema de arquivos local, crie uma pasta para armazenar seu conteúdo (por exemplo, **C:\DemoSite**).
2. No menu **Iniciar** , aponte para **Ferramentas administrativas**e clique em **Gerenciador do serviços de informações da Internet (IIS)**.
3. No Gerenciador do IIS, no painel **conexões** , expanda o nó do servidor (por exemplo, **PROWEB1**).

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image3.png)
4. Clique com o botão direito do mouse no nó **sites** e clique em **Adicionar site**.
5. Na caixa **nome do site** , digite um nome para o site do IIS (por exemplo, **DemoSite**).
6. Na caixa **caminho físico** , digite (ou procure) o caminho para a pasta local (por exemplo, **C:\DemoSite**).
7. Na caixa **porta** , digite o número da porta na qual você deseja hospedar o site (por exemplo, **85**).

    > [!NOTE]
    > Os números de porta padrão são 80 para HTTP e 443 para HTTPS. No entanto, se você hospedar esse site na porta 80, precisará parar o site padrão antes de poder acessar seu site.
8. Deixe a caixa **nome do host** em branco, a menos que você queira configurar um registro DNS (sistema de nomes de domínio) para o site e clique em **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image4.png)

    > [!NOTE]
    > Em um ambiente de produção, você provavelmente desejará hospedar seu site na porta 80 e configurar um cabeçalho de host, junto com os registros DNS correspondentes. Para obter mais informações sobre como configurar cabeçalhos de host no IIS 7, consulte [configurar um cabeçalho de host para um site da Web (IIS 7)](https://technet.microsoft.com/library/cc753195(WS.10).aspx). Para obter mais informações sobre a função de servidor DNS no Windows Server 2008 R2, consulte [visão geral do servidor](https://technet.microsoft.com/library/cc770392.aspx) DNS e [servidor DNS](https://technet.microsoft.com/windowsserver/dd448607).
9. No painel **Ações** , em **Editar Site**, clique em **Ligações**.
10. Na caixa de diálogo **Ligações do Site**, clique em **Adicionar**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image5.png)
11. Na caixa de diálogo **Adicionar associação do site** , defina o **endereço IP** e a **porta** para corresponder à configuração do site existente.
12. Na caixa **nome do host** , digite o nome do seu servidor Web (por exemplo, **PROWEB1**) e clique em **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image6.png)

    > [!NOTE]
    > A associação do primeiro site permite que você acesse o site localmente usando o endereço IP e a porta ou `http://localhost:85` . A segunda Associação de site permite que você acesse o site de outros computadores no domínio usando o nome do computador (por exemplo, http://proweb1:85) .
13. Na caixa de diálogo **Ligações do Site**, clique em **Fechar**.
14. No painel **Conexões**, selecione **Pools de aplicativos**.
15. No painel **pools de aplicativos** , clique com o botão direito do mouse no nome do pool de aplicativos e clique em **configurações básicas**. Por padrão, o nome do pool de aplicativos corresponderá ao nome do seu site (por exemplo, **DemoSite**).
16. Na lista **versão do .NET Framework** , selecione **.NET Framework v 4.0.30319**e clique em **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image7.png)

    > [!NOTE]
    > A solução de exemplo requer o .NET Framework 4,0. Isso não é um requisito para Implantação da Web em geral.

Para que seu site forneça conteúdo, a identidade do pool de aplicativos deve ter permissões de leitura na pasta local que armazena o conteúdo. No IIS 7,5, os pools de aplicativos são executados com uma identidade exclusiva do pool de aplicativos por padrão (em oposição às versões anteriores do IIS, em que os pools de aplicativos normalmente seriam executados usando a conta de serviço de rede). A identidade do pool de aplicativos não é uma conta de usuário real e não aparece em nenhuma lista de usuários ou grupos&#x2014;em vez disso, ela é criada dinamicamente quando o pool de aplicativos é iniciado. Cada identidade do pool de aplicativos é adicionada ao grupo de segurança local do **IIS \_ IUSRS** como um item oculto.

Para conceder permissões a uma identidade de pool de aplicativos em um arquivo ou pasta, você tem duas opções:

- Atribua permissões à identidade do pool de aplicativos diretamente, usando o formato **nome do pool de \( aplicativos do IIS AppPool** (por exemplo, **IIS AppPool\DemoSite**).
- Atribua permissões ao grupo ** \_ IUSRS do IIS** .

A abordagem mais comum é atribuir permissões ao grupo local do **IIS \_ IUSRS** , pois essa abordagem permite que você altere os pools de aplicativos sem reconfigurar as permissões do sistema de arquivos. O procedimento a seguir usa essa abordagem baseada em grupo.

> [!NOTE]
> Para obter mais informações sobre as identidades do pool de aplicativos no IIS 7,5, consulte [identidades do pool de aplicativos](https://go.microsoft.com/?linkid=9805123).

**Para configurar permissões de pasta para um site do IIS**

1. No Windows Explorer, navegue até o local da sua pasta local.
2. Clique com o botão direito do mouse na pasta e, em seguida, clique em **Propriedades**.
3. Sobre a guia **Security** , clique em **Edit**e, em seguida, em **Add**.
4. Clique em **Locais**. Na caixa de diálogo **locais** , selecione o servidor local e clique em **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image8.png)
5. Na caixa de diálogo **Selecionar usuários ou grupos** , digite **IIS \_ IUSRS**, clique em **verificar nomes**e, em seguida, clique em **OK**.
6. Na caixa **de diálogo permissões para (nome da pasta)** , observe que o novo grupo recebeu as permissões **ler & executar**, **listar conteúdo da pasta**e **ler** por padrão. Deixe isso inalterado e clique em **OK**.
7. Clique em **OK** para fechar a caixa de diálogo **Propriedades (nome da pasta)** .

## <a name="disable-the-remote-agent-service"></a>Desabilitar o serviço de agente remoto

Quando você instala o Implantação da Web, o serviço Web Deployment Agent é instalado e iniciado automaticamente. Esse serviço permite que você implante e publique pacotes da Web de um local remoto. Você não usará o recurso de implantação remota neste cenário, portanto, você deve parar e desabilitar o serviço.

> [!NOTE]
> Você não precisa interromper o serviço agente remoto para importar e implantar um pacote da Web manualmente. No entanto, é uma boa prática parar e desabilitar o serviço se você não planeja usá-lo.

Você pode parar e desabilitar um serviço de várias maneiras, usando vários utilitários de linha de comando ou cmdlets do Windows PowerShell. Este procedimento descreve uma abordagem simples baseada na interface do usuário.

**Para parar e desabilitar o serviço de agente remoto**

1. No menu **Iniciar** , aponte para **Ferramentas Administrativas**e depois clique em **Serviços**.
2. No console de serviços, localize a linha de **serviço Web Deployment Agent** .

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image9.png)
3. Clique com o botão direito do mouse em **serviço de Deployment Agent da Web**e clique em **Propriedades**.
4. Na caixa de diálogo **Propriedades do serviço Web Deployment Agent** , clique em **parar**.
5. Na lista **tipo de inicialização** , selecione **desabilitado**e clique em **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image10.png)

## <a name="conclusion"></a>Conclusão

Neste ponto, o servidor Web está pronto para a implantação de pacotes da Web offline. Antes de tentar importar pacotes da Web para um site do IIS, talvez você queira verificar estes pontos-chave:

- Você registrou o ASP.NET 4,0 com o IIS?
- A identidade do pool de aplicativos tem acesso de leitura à pasta de origem do seu site?
- Você interrompeu o serviço Web Deployment Agent?

> [!div class="step-by-step"]
> [Anterior](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md) 
>  [Avançar](configuring-a-database-server-for-web-deploy-publishing.md)
