---
uid: mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
title: Criar aplicativo MVC 5 com logon do Facebook, Twitter, LinkedIn e Google OAuth2 (C#) | Microsoft Docs
author: Rick-Anderson
description: Este tutorial mostra como criar um aplicativo Web ASP.NET MVC 5 que permite que os usuários façam logon usando o OAuth 2,0 com credenciais de um autenti externo...
ms.author: riande
ms.date: 04/03/2015
ms.assetid: 81ee500f-fc37-40d6-8722-f1b64720fbb6
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
msc.type: authoredcontent
ms.openlocfilehash: dd2e55d68ceb5a90134e394c00f3a3a231cb27d6
ms.sourcegitcommit: c62ec20b453cee3249eb894ecd75013b57d078f0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/21/2020
ms.locfileid: "92345635"
---
# <a name="create-an-aspnet-mvc-5-app-with-facebook-twitter-linkedin-and-google-oauth2-sign-on-c"></a>Criar um aplicativo do ASP.NET MVC 5 com logon OAuth2 no Facebook, Twitter, LinkedIn e Google (C#)

por [Rick Anderson](https://twitter.com/RickAndMSFT)

> Este tutorial mostra como criar um aplicativo Web ASP.NET MVC 5 que permite que os usuários façam logon usando o [OAuth 2,0](http://oauth.net/2/) com credenciais de um provedor de autenticação externo, como Facebook, Twitter, LinkedIn, Microsoft ou Google. Para simplificar, este tutorial se concentra em trabalhar com credenciais do Facebook e do Google.
> 
> A habilitação dessas credenciais em seus sites proporciona uma vantagem significativa porque milhões de usuários já têm contas com esses provedores externos. Esses usuários podem ser mais inclinados para se inscrever no seu site se não precisarem criar e lembrar um novo conjunto de credenciais.
> 
> Consulte também [aplicativo ASP.NET MVC 5 com SMS e autenticação de Two-Factor de email](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).
> 
> O tutorial também mostra como adicionar dados de perfil para o usuário e como usar a API de associação para adicionar funções. Este tutorial foi escrito por [Rick Anderson](https://blogs.msdn.com/rickAndy) (siga-me no Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).

<a id="start"></a>
## <a name="getting-started"></a>Introdução

Comece Instalando e executando o [Visual Studio Express 2013 para Web](https://go.microsoft.com/fwlink/?LinkId=299058) ou [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Instale o Visual Studio [2013 atualização 3](https://go.microsoft.com/fwlink/?LinkId=390521) ou superior. Para obter ajuda com o Dropbox, GitHub, LinkedIn, Instagram, buffer, Salesforce, vapor, Stack Exchange, TripIt, Twitch, Twitter, Yahoo! e muito mais, consulte este [projeto de exemplo](https://github.com/matthewdunsdon/oauthforaspnet).

> [!NOTE]
> Você deve instalar o Visual Studio [2013 atualização 3](https://go.microsoft.com/fwlink/?LinkId=390521) ou superior para usar o Google OAuth 2 e para depurar localmente sem avisos SSL.

Clique em **novo projeto** na página **inicial** , ou você pode usar o menu e selecionar **arquivo**e, em seguida, **novo projeto**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image1.png)  

<a id="1st"></a>
## <a name="creating-your-first-application"></a>Criando seu primeiro aplicativo

Clique em **novo projeto**, selecione **Visual C#** à esquerda, em seguida, **Web** e, em seguida, selecione **aplicativo Web ASP.net**. Nomeie seu projeto como "MvcAuth" e clique em **OK**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image2.png)

Na caixa de diálogo **novo projeto ASP.net** , clique em **MVC**. Se a autenticação não for uma **conta de usuário individual**, clique no botão **alterar autenticação** e selecione **contas de usuário individuais**. Ao verificar o **host na nuvem**, o aplicativo será muito fácil de hospedar no Azure.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image3.png)

Se você selecionou **host na nuvem**, conclua a caixa de diálogo Configurar.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image4.png)

### <a name="use-nuget-to-update-to-the-latest-owin-middleware"></a>Usar o NuGet para atualizar para o middleware OWIN mais recente

Use o Gerenciador de pacotes NuGet para atualizar o [middleware OWIN](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md). Selecione **atualizações** no menu à esquerda. Você pode clicar no botão **atualizar tudo** ou pode pesquisar somente pacotes OWIN (mostrados na próxima imagem):

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image5.png)

Na imagem abaixo, somente os pacotes OWIN são mostrados:

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image6.png)

No console do Gerenciador de pacotes (PMC), você pode inserir o `Update-Package` comando, que atualizará todos os pacotes.

Pressione **F5** ou **Ctrl + F5** para executar o aplicativo. Na imagem abaixo, o número da porta é 1234. Ao executar o aplicativo, você verá um número de porta diferente.

Dependendo do tamanho da janela do navegador, talvez seja necessário clicar no ícone de navegação para ver os links **início**, **sobre**, **contato**, **registro** e **logon** .

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image7.png)  
![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image8.png) 

<a id="ssl"></a>
## <a name="setting-up-ssl-in-the-project"></a>Configurando o SSL no projeto

Para se conectar a provedores de autenticação como o Google e o Facebook, você precisará configurar IIS-Express para usar SSL. É importante continuar usando o SSL após o logon e não voltar ao HTTP, seu cookie de logon é tão secreto quanto seu nome de usuário e senha e sem usar SSL que você está enviando em texto não criptografado pela conexão. Além disso, você já tomou tempo para executar o handshake e proteger o canal (que é a grande parte do que torna o HTTPS mais lento do que o HTTP) antes da execução do pipeline do MVC, portanto, redirecionar de volta para HTTP depois que você estiver conectado não fará com que a solicitação atual ou solicitações futuras sejam muito mais rápidas.

1. Em **Gerenciador de soluções**, clique no projeto **MvcAuth** .
2. Pressione a tecla F4 para mostrar as propriedades do projeto. Como alternativa, no menu **Exibir** , você pode selecionar **janela Propriedades**.
3. Altere **SSL habilitado** para verdadeiro.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image9.png)
4. Copie a URL do SSL (que será `https://localhost:44300/` a menos que você tenha criado outros projetos de SSL).
5. Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto **MvcAuth** e selecione **Propriedades**.
6. Selecione a guia **Web** e cole a URL SSL na caixa URL do **projeto** . Salve o arquivo (CTL + S). Você precisará dessa URL para configurar os aplicativos de autenticação do Facebook e do Google.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image10.png)
7. Adicione o atributo [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) ao `Home` controlador para exigir que todas as solicitações usem HTTPS. Uma abordagem mais segura é adicionar o filtro [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) ao aplicativo. Consulte a seção &quot; proteger o aplicativo com SSL e o atributo autorizar &quot; em meu tutorial [criar um aplicativo MVC ASP.NET com autenticação e banco de BD SQL e implantar no serviço Azure app](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Uma parte do controlador inicial é mostrada abaixo.

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample1.cs?highlight=1)]
8. Pressione CTRL+F5 para executar o aplicativo. Se você instalou o certificado no passado, poderá ignorar o restante desta seção e saltar para [criar um aplicativo do Google para OAuth 2 e conectar o aplicativo ao projeto](#goog), caso contrário, siga as instruções para confiar no certificado autoassinado que IIS Express gerou.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image11.png)
9. Leia a caixa de diálogo **aviso de segurança** e clique em **Sim** se desejar instalar o certificado que representa o localhost.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image12.png)
10. O IE mostra a *Home page* e não há nenhum aviso de SSL.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image13.png)
11. O Google Chrome também aceita o certificado e mostrará o conteúdo HTTPS sem aviso. O Firefox usa seu próprio repositório de certificados, portanto, ele exibirá um aviso. Para nosso aplicativo, você pode clicar com segurança para **entender os riscos**.   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image14.png)

<a id="goog"></a>
## <a name="creating-a-google-app-for-oauth-2-and-connecting-the-app-to-the-project"></a>Criando um aplicativo do Google para OAuth 2 e conectando o aplicativo ao projeto

> [!WARNING]
> Para obter as instruções atuais do Google OAuth, consulte [Configurando a autenticação do Google no ASP.NET Core](/aspnet/core/security/authentication/social/google-logins).

1. Navegue até o [Console Para Desenvolvedores do Google](https://console.developers.google.com/).
2. Se você ainda não criou um projeto antes, selecione **credenciais** na guia esquerda e, em seguida, selecione **criar**.
3. Na guia à esquerda, clique em **credenciais**.
4. Clique em **criar credenciais** e em **ID do cliente OAuth**. 

    1. Na caixa de diálogo **criar ID do cliente** , mantenha o **aplicativo Web** padrão para o tipo de aplicativo.
    2. Defina as origens de **JavaScript autorizadas** para a URL SSL usada acima ( `https://localhost:44300/` a menos que você tenha criado outros projetos SSL)
    3. Defina o **URI de redirecionamento autorizado** para:  
         `https://localhost:44300/signin-google`
5. Clique no item de menu da tela de consentimento do OAuth e defina seu endereço de email e o nome do produto. Quando você tiver concluído o formulário, clique em **salvar**.
6. Clique no item de menu biblioteca, pesquise **API do Google +**, clique nele e pressione habilitar.
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image15.png)  
  
   A imagem abaixo mostra as APIs habilitadas.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image16.png)
7. No Gerenciador de API de APIs do Google, visite a guia **credenciais** para obter a **ID do cliente**. Baixe para salvar um arquivo JSON com segredos do aplicativo. Copie e cole o **ClientID** e o **ClientSecret** no `UseGoogleAuthentication` método encontrado no arquivo *Startup.auth.cs* na pasta *App_Start* . Os valores **ClientID** e **ClientSecret** mostrados abaixo são exemplos e não funcionam.

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample2.cs?highlight=37-39)]

    > [!WARNING]
    > Segurança-nunca armazene dados confidenciais em seu código-fonte. A conta e as credenciais são adicionadas ao código acima para manter o exemplo simples. Consulte [as práticas recomendadas para implantar senhas e outros dados confidenciais no ASP.net e no serviço de Azure app](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).
8. Pressione **Ctrl + F5** para compilar e executar o aplicativo. Clique no link **Logon** .  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image17.png)
9. Em **usar outro serviço para fazer logon**, clique em **Google**.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image18.png)

    > [!NOTE]
    > Se você perder alguma das etapas acima, obterá um erro HTTP 401. Verifique novamente as etapas acima. Se você perder uma configuração necessária (por exemplo, **nome do produto**), adicione o item ausente e salve; pode levar alguns minutos para que a autenticação funcione.
10. Você será redirecionado para o site do Google, onde você inserirá suas credenciais.   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image19.png)
11. Após inserir suas credenciais, aparecerá uma solicitação para que você dê permissões para o aplicativo Web que acabou de criar: 
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image20.png)
12. Clique em **Aceitar**. Agora você será Redirecionado de volta para a página de **registro** do aplicativo MvcAuth, onde você pode registrar sua conta do Google. Você tem a opção de alterar o nome de registro de email local utilizado para sua conta do Gmail, mas normalmente você deverá preferir manter o alias de email padrão (ou seja, aquele utilizado por você para a autenticação). Clique em **Registrar**.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image21.png)

<a id="fb"></a>
## <a name="creating-the-app-in-facebook-and-connecting-the-app-to-the-project"></a>Criando o aplicativo no Facebook e conectando o aplicativo ao projeto

> [!WARNING]
> Para obter as instruções atuais de autenticação do Facebook OAuth2, consulte [Configurando a autenticação do Facebook](/aspnet/core/security/authentication/social/facebook-logins)

<a id="mdb"></a>
## <a name="examine-the-membership-data"></a>Examinar os dados de associação

No menu **Exibir** , clique em **Gerenciador de servidores**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image32.png)

Expanda **DefaultConnection (MvcAuth)**, expanda **tabelas**, clique com o botão direito do mouse em **AspNetUsers** e clique em **Mostrar dados da tabela**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image33.png)

![dados da tabela aspnetusers](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image34.png)

<a id="ap"></a>
## <a name="adding-profile-data-to-the-user-class"></a>Adicionando dados de perfil à classe de usuário

Nesta seção, você adicionará a data de nascimento e a cidade inicial aos dados do usuário durante o registro, conforme mostrado na imagem a seguir.

![reg com Home cidade e bday](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image35.png)

Abra o arquivo *Models\IdentityModels.cs* e adicione as propriedades da data de nascimento e da cidade inicial:

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample4.cs?highlight=3-4)]

Abra o arquivo *Models\AccountViewModels.cs* e as propriedades definir data de nascimento e cidade inicial em `ExternalLoginConfirmationViewModel` .

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample5.cs?highlight=8-9)]

Abra o arquivo *Controllers\AccountController.cs* e adicione o código para a data de nascimento e a Home cidade no `ExternalLoginConfirmation` método de ação, conforme mostrado:

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample6.cs?highlight=21-23)]

Adicione data de nascimento e cidade inicial ao arquivo *Views\Account\ExternalLoginConfirmation.cshtml* :

[!code-cshtml[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample7.cshtml?highlight=27-40)]

Exclua o banco de dados de associação para que você possa registrar novamente sua conta do Facebook com seu aplicativo e verificar se você pode adicionar a nova data de nascimento e as informações de perfil da cidade inicial.

Em **Gerenciador de soluções**, clique no ícone **Mostrar todos os arquivos** , clique com o botão direito do mouse em *Adicionar \_ Data\aspnet-MvcAuth- &lt; dateStamp &gt; . MDF* e clique em **excluir**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image36.png)

No menu **ferramentas** , clique em **Gerenciador de pacotes NuGet**e em **console do Gerenciador de pacotes** (PMC). Insira os seguintes comandos no PMC.

1. Enable-Migrations
2. Add-Migration init
3. Update-Database

Execute o aplicativo e use o FaceBook e o Google para fazer logon e registrar alguns usuários.

## <a name="examine-the-membership-data"></a>Examinar os dados de associação

No menu **Exibir** , clique em **Gerenciador de servidores**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image37.png)

Clique com o botão direito em **AspNetUsers** e clique em **Mostrar dados da tabela**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image38.png)

Os `HomeTown` `BirthDate` campos e são mostrados abaixo.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image39.png)

<a id="off"></a>
## <a name="logging-off-your-app-and-logging-in-with-another-account"></a>Fazendo logoff de seu aplicativo e fazendo logon com outra conta

Se você fizer logon em seu aplicativo com o Facebook, e depois fizer logoff e tentar fazer logon novamente com uma conta diferente do Facebook (usando o mesmo navegador), você estará imediatamente conectado à conta anterior do Facebook que você usou. Para usar outra conta, você precisa navegar até o Facebook e fazer logoff no Facebook. A mesma regra se aplica a qualquer outro provedor de autenticação de terceiros. Como alternativa, você pode fazer logon com outra conta usando um navegador diferente.

## <a name="next-steps"></a>Próximas etapas

Consulte [apresentando as instruções do Yahoo e do LinkedIn OAuth Security Providers for OWIN](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) by Jerrie Pelser for Yahoo e LinkedIn. Consulte os botões de logon de Jerrie para o ASP.NET MVC 5 para habilitar botões de logon social.

Siga meu tutorial para [criar um aplicativo MVC do ASP.NET com autenticação e banco de BD SQL e implantar no Azure app Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data), que continua este tutorial e mostra o seguinte:

1. Como implantar seu aplicativo no Azure.
2. Como proteger seu aplicativo com funções.
3. Como proteger seu aplicativo com o [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) e [autorizar](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx) filtros.
4. Como usar a API Membership para adicionar usuários e funções.

Deixe comentários sobre como você gostou deste tutorial e o que poderíamos melhorar. Você também pode solicitar novos tópicos em [mostre-me como com o código](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code). Você pode até mesmo pedir e votar em novos recursos a serem adicionados ao ASP.NET. Por exemplo, você pode votar em uma ferramenta para [criar e gerenciar usuários e funções.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)

Para obter uma boa explicação de como os serviços de autenticação externa do ASP.NET funcionam, consulte [serviços de autenticação externa](https://asp.net/web-api/overview/security/external-authentication-services)de Robert McMurray. O artigo de Robert também entra em detalhes sobre como habilitar a autenticação da Microsoft e do Twitter. O [tutorial do EF/MVC](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) excelente de Tom Dykstra mostra como trabalhar com o Entity Framework.
