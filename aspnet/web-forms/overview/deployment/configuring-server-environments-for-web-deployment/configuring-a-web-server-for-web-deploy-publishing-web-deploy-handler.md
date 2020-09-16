---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler
title: Configuração de um servidor Web para publicação de Implantação da Web (manipulador de Implantação da Web)
author: jrjlee
description: Este tópico descreve como configurar um servidor Web Serviços de Informações da Internet (IIS) para dar suporte à publicação e à implantação na Web usando o IIS Implantação da Web Han...
ms.author: riande
ms.date: 01/29/2017
ms.assetid: 90ebf911-1c46-4470-b876-1335bd0f590f
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler
msc.type: authoredcontent
ms.openlocfilehash: af46b5a74309fbae4b5db072363e71d965445f9a
ms.sourcegitcommit: 4ed0b65ae32d9f35e42ee6296b877747e063df4d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/16/2020
ms.locfileid: "90609685"
---
# <a name="configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler"></a>Configuração de um servidor Web para publicação de Implantação da Web (manipulador de Implantação da Web)

[Download do PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tópico descreve como configurar um servidor Web Serviços de Informações da Internet (IIS) para dar suporte à publicação e à implantação na Web usando o manipulador de Implantação da Web do IIS.
> 
> Quando você trabalha com o Implantação da Web 2,0 ou posterior, há três abordagens principais que podem ser usadas para colocar seus aplicativos ou sites em um servidor Web. Você pode:
> 
> - Use o *implantação da Web serviço de agente remoto*. Essa abordagem requer menos configuração do servidor Web, mas você precisa fornecer as credenciais de um administrador de servidor local para implantar qualquer coisa no servidor.
> - Use o *manipulador de implantação da Web*. Essa abordagem é muito mais complexa e exige mais esforço inicial para configurar o servidor Web. No entanto, ao usar essa abordagem, você pode configurar o IIS para permitir que usuários não administradores executem a implantação. O manipulador de Implantação da Web só está disponível no IIS versão 7 ou posterior.
> - Use a *implantação offline*. Essa abordagem requer a configuração mínima do servidor Web, mas um administrador de servidor deve copiar manualmente o pacote da Web no servidor e importá-lo por meio do Gerenciador do IIS.
> 
> Para obter mais informações sobre os principais recursos, as vantagens e as desvantagens dessas abordagens, consulte [escolhendo a abordagem certa para a implantação da Web](choosing-the-right-approach-to-web-deployment.md).

Sim, se você quiser permitir que usuários não administradores implantem conteúdo em sites do IIS específicos. Essa abordagem geralmente é desejável nesses tipos de cenários:

- Ambientes de preparo ou de produção, nos quais a conta de serviço ou pessoa que dispara a implantação remota é improvável de ter acesso às credenciais de um administrador de servidor.
- Ambientes hospedados, nos quais você deseja conceder aos usuários remotos a capacidade de atualizar seus sites sem dar a eles controle total de seus servidores Web (ou acesso a sites de qualquer outra pessoa).

Em cenários de desenvolvimento ou teste, ou em organizações menores, a implantação de conteúdo usando credenciais de administrador do servidor geralmente é menos contenciosos. Nesses cenários, configurar seus servidores Web para dar suporte à implantação usando o [serviço de agente de implantação da Web remoto](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md) oferece uma abordagem mais direta.

## <a name="task-overview"></a>Visão geral da tarefa

Para configurar o servidor Web para aceitar e implantar pacotes da Web de um computador remoto usando a abordagem do manipulador de Implantação da Web, você precisará:

- Crie ou escolha uma conta de usuário de domínio (o "usuário não administrador") cujas credenciais você usará para executar implantações.
- Instale o IIS 7,5, incluindo o serviço de gerenciamento da Web e o módulo de autenticação básica.
- Instale o Implantação da Web 2,1 ou posterior.
- Configure o serviço de gerenciamento da Web para permitir conexões remotas e inicie o serviço.
- Crie um site do IIS para hospedar o conteúdo implantado.
- Conceda permissões de usuário que não sejam de administrador em seu site no Gerenciador do IIS.
- Verifique se as regras de delegação do serviço de gerenciamento da Web permitem que o serviço adicione e altere o conteúdo do site usando sua conta de usuário não administrador.
- Configure quaisquer firewalls para permitir conexões de entrada na porta 8172.

Para hospedar a solução de exemplo ContactManager especificamente, você também precisará:

- Instale o .NET Framework 4,0.
- Instale o ASP.NET MVC 3.

Este tópico mostrará como executar cada um desses procedimentos. As tarefas e orientações neste tópico pressupõem que você está começando com uma compilação de servidor limpa que executa o Windows Server 2016. Antes de continuar, verifique se:

- Windows Server 2016
- O servidor está ingressado no domínio.
- O servidor tem um endereço IP estático.

> [!NOTE]
> Para obter mais informações sobre como unir computadores a um domínio, consulte [unindo computadores ao domínio e fazendo logon](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx). Para obter mais informações sobre como configurar endereços IP estáticos, consulte [configurar um endereço IP estático](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx).

## <a name="install-products-and-components"></a>Instalar produtos e componentes

Esta seção irá orientá-lo na instalação dos produtos e componentes necessários no servidor Web. Antes de começar, uma prática recomendada é executar Windows Update para garantir que seu servidor esteja totalmente atualizado.

Nesse caso, você precisa instalar estas coisas:

- **Configuração recomendada do IIS 7**. Isso habilita a função do **servidor Web (IIS)** no servidor Web e instala o conjunto de módulos e componentes do IIS que você precisa para hospedar um aplicativo ASP.net.
- **IIS: serviço de gerenciamento**. Isso instala o serviço de gerenciamento da Web (WMSvc) no IIS. Esse serviço permite o gerenciamento remoto de sites do IIS e expõe o ponto de extremidade do manipulador de Implantação da Web para os clientes.
- **IIS: autenticação básica**. Isso instala o módulo de autenticação básica do IIS. Isso permite que o WMSvc (serviço de gerenciamento da Web) autentique as credenciais fornecidas por você.
- **Ferramenta de implantação da Web 2,1 ou posterior**. Isso instala Implantação da Web (e seu executável subjacente, MSDeploy.exe) em seu servidor. Como parte desse processo, ele instala o manipulador de Implantação da Web e o integra com o serviço de gerenciamento da Web.
- **.NET Framework 4,0**. Isso é necessário para executar aplicativos que foram criados nesta versão do .NET Framework.
- **ASP.NET MVC 3**. Isso instala os assemblies necessários para executar os aplicativos MVC 3.

> [!NOTE]
> Este tutorial descreve o uso do Web Platform Installer para instalar e configurar vários componentes. Embora você não precise usar o Web Platform Installer, ele simplifica o processo de instalação detectando automaticamente as dependências e garantindo que você sempre obtenha as versões mais recentes dos produtos. Para obter mais informações, consulte [Microsoft Web Platform Installer](https://go.microsoft.com/?linkid=9805118).

**Para instalar os produtos e componentes necessários**

1. Baixe e instale o [Web Platform Installer](https://go.microsoft.com/?linkid=9805118).
2. Quando a instalação for concluída, a Web Platform Installer será iniciada automaticamente.

    > [!NOTE]
    > Agora você pode iniciar o Web Platform Installer a qualquer momento no menu **Iniciar** . Para fazer isso, no menu **Iniciar** , clique em **todos os programas**e, em seguida, clique em **Microsoft Web Platform Installer**.
3. Na parte superior da janela do **Web Platform Installer**, clique em **Produtos**.
4. No lado esquerdo da janela, no painel de navegação, clique em **estruturas**.
5. Na linha **Microsoft .NET Framework 4** , se o .NET Framework ainda não estiver instalado, clique em **Adicionar**.

    > [!NOTE]
    > Talvez você já tenha instalado o .NET Framework 4,0 por meio de Windows Update. Se um produto ou componente já estiver instalado, o Web Platform Installer indicará isso substituindo o botão **Adicionar** pelo texto **instalado**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image1.png)
6. Na linha **ASP.NET MVC 3 (Visual Studio 2010)** , clique em **Adicionar**.
7. No painel de navegação, clique em **servidor**.
8. Na linha de **configuração recomendada do IIS 7** , clique em **Adicionar**.
9. Na linha **ferramenta de implantação da Web 2,1** , clique em **Adicionar**.
10. Na linha **IIS: autenticação básica** , clique em **Adicionar**.
11. Na linha **IIS: serviço de gerenciamento** , clique em **Adicionar**.
12. Clique em **Instalar**. O Web Platform Installer mostrará uma lista de produtos&#x2014;junto com as dependências associadas&#x2014;ser instalado e solicitará que você aceite os termos de licença.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image2.png)
13. Examine os termos de licença e, se você concordar com os termos, clique em **aceito**.
14. Quando a instalação for concluída, clique em **concluir**e feche a janela **Web Platform Installer** .

Se você instalou o .NET Framework 4,0 antes de instalar o IIS, precisará executar a [ferramenta de registro do ASP.net IIS](https://msdn.microsoft.com/library/k6h9cz8h(v=VS.100).aspx) (ASPNET \_regiis.exe) para registrar a versão mais recente do ASP.NET com o IIS. Se você não fizer isso, descobrirá que o IIS fornecerá conteúdo estático (como arquivos HTML) sem nenhum problema, mas retornará o **erro HTTP 404,0 – não encontrado** quando você tentar navegar até o conteúdo do ASP.net. Você pode usar o procedimento a seguir para garantir que o ASP.NET 4,0 seja registrado.

**Para registrar o ASP.NET 4,0 com o IIS**

1. Clique em **Iniciar**e digite **prompt de comando**.
2. Nos resultados da pesquisa, clique com o botão direito do mouse em **prompt de comando**e clique em **Executar como administrador**.
3. Na janela do prompt de comando, navegue até o diretório **%windir%\Microsoft.NET\Framework\v4.0.30319**
4. Digite este comando e pressione ENTER:

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/samples/sample1.cmd)]
5. Se você planeja hospedar aplicativos Web de 64 bits a qualquer momento, você também deve registrar a versão de 64 bits do ASP.NET com o IIS. Para fazer isso, na janela de prompt de comando, navegue até o diretório **%windir%\Microsoft.NET\Framework64\v4.0.30319** .
6. Digite este comando e pressione ENTER:

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/samples/sample2.cmd)]

Como uma prática recomendada, use Windows Update novamente neste ponto para baixar e instalar todas as atualizações disponíveis para os novos produtos e componentes que você instalou.

## <a name="configure-the-web-management-service"></a>Configurar o serviço de gerenciamento da Web

Agora que você instalou tudo o que precisa, a próxima etapa é configurar o serviço de gerenciamento da Web no IIS. Em um alto nível, você precisará concluir estas tarefas:

- Habilite a autenticação básica no nível do servidor.
- Configure o serviço de gerenciamento da Web para aceitar conexões remotas.
- Inicie o serviço de gerenciamento da Web.
- Verifique se as regras de delegação do serviço de gerenciamento da Web necessárias estão em vigor.

**Para configurar o serviço de gerenciamento da Web**

1. No menu **Iniciar** , aponte para **Ferramentas administrativas**e clique em **Gerenciador do serviços de informações da Internet (IIS)**.
2. No Gerenciador do IIS, no painel **conexões** , clique no nó do servidor (por exemplo, **STAGEWEB1**).

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image3.png)
3. No painel central, em **IIS**, clique duas vezes em **autenticação**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image20.png)
4. Clique com o botão direito do mouse em **autenticação básica**e clique em **habilitar**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image5.png)
5. No painel **conexões** , clique no nó do servidor novamente para retornar às configurações de nível superior.
6. No painel central, em **Gerenciamento**, clique duas vezes em **serviço de gerenciamento**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image6.png)
7. No painel central, selecione **habilitar conexões remotas**.

    > [!NOTE]
    > Se o serviço de gerenciamento da Web já estiver em execução, você precisará interrompê-lo primeiro.
8. No painel **ações** , clique em **Iniciar** para iniciar o serviço de gerenciamento da Web.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image7.png)
9. Se você for solicitado a salvar suas configurações, clique em **Sim**.

    > [!NOTE]
    > Talvez você também queira configurar o serviço para iniciar automaticamente. Para fazer isso, abra o console Serviços, clique com o botão direito do mouse em **serviço de gerenciamento da Web**e clique em **Propriedades**. Na lista suspensa **tipo de inicialização** , selecione **automático**e clique em **OK**.
10. No painel **conexões** , clique no nó do servidor novamente para retornar às configurações de nível superior.
11. No painel central, em **Gerenciamento**, clique duas vezes em **delegação do serviço de gerenciamento**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image8.png)
12. Verifique se o painel central contém um conjunto de regras.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image9.png)

    Essas regras permitem que os usuários do serviço de gerenciamento da Web autorizados usem vários provedores de Implantação da Web. Por exemplo, para implantar aplicativos e conteúdo da Web no IIS por meio do manipulador de Implantação da Web, deve haver uma regra de delegação que permita que todos os usuários do serviço de gerenciamento da Web autenticados usem os provedores **contentPath** e **iisapp** (a última regra que você pode ver na captura de tela).

    Se você instalou produtos e componentes na ordem descrita neste tópico, a versão mais recente do Implantação da Web deverá adicionar automaticamente todas as regras de delegação necessárias ao serviço de gerenciamento da Web. Se a página de delegação do serviço de gerenciamento não mostrar nenhuma regra, você mesmo precisará criá-las. Para obter instruções sobre como fazer isso, consulte [Configurar o manipulador de implantação da Web](https://go.microsoft.com/?linkid=9805124).
13. No painel **conexões** , clique no nó do servidor novamente para retornar às configurações de nível superior.

## <a name="create-and-configure-an-iis-website"></a>Criar e configurar um site do IIS

Antes de implantar o conteúdo da Web em seu servidor, você precisa criar e configurar um site do IIS para hospedar o conteúdo. Implantação da Web só pode implantar pacotes da Web em um site do IIS existente; Ele não pode criar o site para você. Você também precisa fazer uma configuração extra para permitir que sua conta que não seja de administrador implante o conteúdo remotamente. Em um alto nível, você precisará concluir estas tarefas:

- Crie uma pasta no sistema de arquivos para hospedar seu conteúdo.
- Crie um site do IIS para fornecer o conteúdo e associe-o à pasta local.
- Conceda permissões de leitura à identidade do pool de aplicativos na pasta local.
- Conceda as permissões necessárias do IIS para a conta de domínio que implantará seu aplicativo Web.

Embora não haja nada para impedir que você implante conteúdo no site padrão no IIS, essa abordagem não é recomendada para nada além de cenários de teste ou demonstração. Para simular um ambiente de produção, você deve criar um novo site do IIS com configurações específicas aos requisitos do seu aplicativo.

**Para criar um site do IIS**

1. No sistema de arquivos local, crie uma pasta para armazenar seu conteúdo (por exemplo, **C:\DemoSite**).
2. No menu **Iniciar** , aponte para **Ferramentas administrativas**e clique em **Gerenciador do serviços de informações da Internet (IIS)**.
3. No Gerenciador do IIS, no painel **conexões** , expanda o nó do servidor (por exemplo, **STAGEWEB1**).

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image10.png)
4. Clique com o botão direito do mouse no nó **sites** e clique em **Adicionar site**.
5. Na caixa **nome do site** , digite um nome para o site do IIS (por exemplo, **DemoSite**).
6. Na caixa **caminho físico** , digite (ou procure) o caminho para a pasta local (por exemplo, **C:\DemoSite**).
7. Na caixa **porta** , digite o número da porta na qual você deseja hospedar o site (por exemplo, **85**).

    > [!NOTE]
    > Os números de porta padrão são 80 para HTTP e 443 para HTTPS. No entanto, se você hospedar esse site na porta 80, precisará parar o site padrão antes de poder acessar seu site.
8. Deixe a caixa **nome do host** em branco, a menos que você queira configurar um registro DNS (sistema de nomes de domínio) para o site e clique em **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image11.png)

    > [!NOTE]
    > Em um ambiente de produção, você provavelmente desejará hospedar seu site na porta 80 e configurar um cabeçalho de host, junto com os registros DNS correspondentes. Para obter mais informações sobre como configurar cabeçalhos de host no IIS 7, consulte [configurar um cabeçalho de host para um site da Web (IIS 7)](https://technet.microsoft.com/library/cc753195(WS.10).aspx). Para obter mais informações sobre a função de servidor DNS no Windows Server, consulte [visão geral do servidor](https://technet.microsoft.com/library/cc770392.aspx) DNS e [servidor DNS](https://technet.microsoft.com/windowsserver/dd448607).
9. No painel **Ações** , em **Editar Site**, clique em **Ligações**.
10. Na caixa de diálogo **Ligações do Site**, clique em **Adicionar**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image12.png)
11. Na caixa de diálogo **Adicionar associação do site** , defina o **endereço IP** e a **porta** para corresponder à configuração do site existente.
12. Na caixa **nome do host** , digite o nome do seu servidor Web (por exemplo, **STAGEWEB1**) e clique em **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image13.png)

    > [!NOTE]
    > A associação do primeiro site permite que você acesse o site localmente usando o endereço IP e a porta ou `http://localhost:85` . A segunda Associação de site permite que você acesse o site de outros computadores no domínio usando o nome do computador (por exemplo, http://stageweb1:85) .
13. Na caixa de diálogo **Ligações do Site**, clique em **Fechar**.
14. No painel **Conexões**, selecione **Pools de aplicativos**.
15. No painel **pools de aplicativos** , clique com o botão direito do mouse no nome do pool de aplicativos e clique em **configurações básicas**. Por padrão, o nome do pool de aplicativos corresponderá ao nome do seu site (por exemplo, **DemoSite**).
16. Na lista **versão do .NET CLR** , selecione **.NET CLR v 4.0.30319**e clique em **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image21.png)

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

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image15.png)
5. Na caixa de diálogo **Selecionar usuários ou grupos** , digite **IIS \_ IUSRS**, clique em **verificar nomes**e, em seguida, clique em **OK**.
6. Na caixa **de diálogo permissões para (nome da pasta)** , observe que o novo grupo foi atribuído às permissões **ler &amp; executar**, **listar conteúdo da pasta**e **ler** por padrão. Deixe isso inalterado e clique em **OK**.
7. Clique em **OK** para fechar a caixa de diálogo **Propriedades (nome da pasta)** .

Como uma tarefa final, você deve conceder as permissões apropriadas ao usuário não administrador cujas credenciais você usará para implantar o conteúdo. Esse usuário requer as permissões para implantar o conteúdo remotamente em seu site.

**Para configurar permissões de site do IIS para um usuário de domínio não administrador**

1. No Gerenciador do IIS, no painel **conexões** , clique com o botão direito do mouse no nó do site (por exemplo, **DemoSite**), aponte para **implantar**e clique em **Configurar implantação da Web publicação**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image16.png)
2. Na caixa de diálogo **configurar implantação da Web publicação** , à direita da lista **selecionar um usuário para conceder permissões de publicação** , clique no botão de reticências.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image17.png)
3. Na caixa de diálogo **permitir usuário** , digite o domínio e o nome de usuário da conta que você deseja usar para implantar o conteúdo e clique em **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image18.png)
4. Na caixa de diálogo **Configurar publicação implantação da Web** , clique em **configuração**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image19.png)

    > [!NOTE]
    > Essa operação executa duas funções principais em uma única etapa. Primeiro, ele concede ao usuário permissão para modificar o site remotamente por meio do serviço de gerenciamento da Web, de acordo com as regras de delegação que você examinou na seção anterior. Em segundo lugar, ele concede ao usuário controle total da pasta de origem do site, que permite ao usuário adicionar, modificar e definir permissões no conteúdo do site.
5. Na caixa de diálogo **Configurar publicação implantação da Web** , clique em **fechar**.

## <a name="configure-firewall-exceptions"></a>Configurar exceções de firewall

Por padrão, o serviço de gerenciamento da Web do IIS escuta na porta TCP 8172. Se o Firewall do Windows estiver habilitado no servidor Web, você precisará criar uma nova regra de entrada para permitir o tráfego TCP na porta 8172 (todo o tráfego de saída é permitido por padrão no firewall do Windows). Se você usar um firewall de terceiros, precisará criar regras para permitir o tráfego.

| Direção | Da porta | Para porta | Tipo de Porta |
| --- | --- | --- | --- |
| Entrada | Qualquer | 8172 | TCP |
| Saída | 8172 | Qualquer | TCP |

Para obter mais informações sobre como configurar regras no firewall do Windows, consulte [Configurando regras de firewall](https://technet.microsoft.com/library/dd448559(WS.10).aspx). Para firewalls de terceiros, consulte a documentação do produto.

## <a name="conclusion"></a>Conclusão

O servidor Web agora deve estar pronto para aceitar implantações remotas no manipulador de Implantação da Web por meio do serviço de gerenciamento da Web. Antes de tentar implantar um aplicativo Web no servidor, talvez você queira verificar estes pontos-chave:

- Você habilitou a autenticação básica no nível do servidor no IIS?
- Você habilitou conexões remotas com o serviço de gerenciamento da Web?
- Você iniciou o serviço de gerenciamento da Web?
- Há regras de delegação do serviço de gerenciamento em vigor?
- A identidade do pool de aplicativos tem acesso de leitura à pasta de origem do seu site?
- A conta de usuário não administrador tem permissões no nível do site no IIS?
- O firewall permite conexões de entrada para o servidor na porta TCP 8172?

## <a name="further-reading"></a>Leitura Adicional

Para obter orientação sobre como configurar arquivos de projeto do Microsoft Build Engine (MSBuild) personalizados para implantar pacotes da Web no manipulador de Implantação da Web, consulte [Configurando propriedades de implantação para um ambiente de destino](configuring-deployment-properties-for-a-target-environment.md).

> [!div class="step-by-step"]
> [Anterior](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md) 
>  [Avançar](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)
