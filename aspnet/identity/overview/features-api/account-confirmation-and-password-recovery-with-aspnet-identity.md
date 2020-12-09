---
uid: identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity
title: Confirmação de conta & recuperação de senha-ASP.NET Identity (C#)-ASP.NET 4. x
author: HaoK
description: Antes de fazer este tutorial, você deve primeiro concluir a criação de um aplicativo Web ASP.NET MVC 5 com logon, confirmação de email e redefinição de senha. Este tutorial...
ms.author: riande
ms.date: 01/23/2019
ms.assetid: 8d54180d-f826-4df7-b503-7debf5ed9fb3
ms.custom: seoapril2019
msc.legacyurl: /identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: e27d9180bf8da3572b48cb342be4e703f093d45a
ms.sourcegitcommit: 6727454a30f11ac2365e1cc8a37ef0005f5d15b3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/09/2020
ms.locfileid: "96934104"
---
# <a name="account-confirmation-and-password-recovery-with-aspnet-identity-c"></a>Confirmação de conta e recuperação de senha com ASP.NET Identity (C#)

> Antes de fazer este tutorial, você deve primeiro concluir a [criação de um aplicativo web ASP.NET MVC 5 com logon, confirmação de email e redefinição de senha](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md). Este tutorial contém mais detalhes e mostrará como configurar o email para confirmação de conta local e permitir que os usuários redefinam sua senha esquecida no ASP.NET Identity.

Uma conta de usuário local requer que o usuário crie uma senha para a conta e essa senha seja armazenada (com segurança) no aplicativo Web. O ASP.NET Identity também dá suporte a contas sociais, que não exigem que o usuário crie uma senha para o aplicativo. [Contas sociais](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) usam terceiros (como Google, Twitter, Facebook ou Microsoft) para autenticar usuários. Este tópico aborda o seguinte:

- [Crie um aplicativo MVC do ASP.net](#createMvc) e explore os recursos do ASP.net Identity.
- [Criar o exemplo de identidade](#build)
- [Configurar confirmação de email](#email)

Novos usuários registram seu alias de email, que cria uma conta local.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image1.png)

Selecionar o botão registrar envia um email de confirmação contendo um token de validação para seu endereço de email.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image2.png)

O usuário recebe um email com um token de confirmação para sua conta.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image3.png)

Selecionar o link confirma a conta.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image4.png)

<a id="passwordReset"></a>

## <a name="password-recoveryreset"></a>Recuperação/redefinição de senha

Os usuários locais que esquecem sua senha podem ter um token de segurança enviado para sua conta de email, permitindo que eles redefinam sua senha.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image5.png)  
  
Em breve, o usuário receberá um email com um link que permite redefinir sua senha.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image6.png)  
Selecionar o link vai levá-los para a página redefinir.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image7.png)  
  
Selecionar o botão **Redefinir** confirmará se a senha foi redefinida.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image8.png)

<a id="createMvc"></a>

## <a name="create-an-aspnet-web-app"></a>Criar um aplicativo Web ASP .NET

Comece Instalando e executando o [Visual Studio 2017](https://visualstudio.microsoft.com/).

1. Crie um novo projeto Web ASP.NET e selecione o modelo MVC. Web Forms também oferece suporte a ASP.NET Identity, portanto, você pode seguir etapas semelhantes em um aplicativo Web Forms.
2. Altere a autenticação para **contas de usuário individuais**.
3. Execute o aplicativo, selecione o link **registrar** e registre um usuário. Neste ponto, a única validação no email é com o atributo [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) .
4. Em Gerenciador de Servidores, navegue até **Data Connections\DefaultConnection\Tables\AspNetUsers**, clique com o botão direito do mouse e selecione **Abrir definição de tabela**.

    A imagem a seguir mostra o `AspNetUsers` esquema:

    ![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image9.png)
5. Clique com o botão direito do mouse na tabela **AspNetUsers** e selecione **Mostrar dados da tabela**.  
  
    ![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image10.png)  
  
   Neste ponto, o email não foi confirmado.

O armazenamento de dados padrão para ASP.NET Identity é Entity Framework, mas você pode configurá-lo para usar outros armazenamentos de dados e para adicionar outros campos. Consulte a seção [recursos adicionais](#addRes) no final deste tutorial.

A [classe de inicialização OWIN](../../../aspnet/overview/owin-and-katana/owin-startup-class-detection.md) ( *Startup.cs* ) é chamada quando o aplicativo é iniciado e invoca o `ConfigureAuth` método no *aplicativo \_ Start\Startup.auth.cs*, que configura o pipeline OWIN e inicializa ASP.net Identity. Examine o método `ConfigureAuth`. Cada `CreatePerOwinContext` chamada registra um retorno de chamada (salvo no `OwinContext` ) que será chamado uma vez por solicitação para criar uma instância do tipo especificado. Você pode definir um ponto de interrupção no construtor e o `Create` método de cada tipo ( `ApplicationDbContext, ApplicationUserManager` ) e verificar se eles são chamados em cada solicitação. Uma instância do `ApplicationDbContext` e `ApplicationUserManager` é armazenada no contexto OWIN, que pode ser acessado em todo o aplicativo. ASP.NET Identity conecta-se ao pipeline OWIN por meio do middleware de cookie. Para obter mais informações, consulte [Gerenciamento de tempo de vida da solicitação para classe de usermanager em ASP.net Identity](https://blogs.msdn.com/b/webdev/archive/2014/02/12/per-request-lifetime-management-for-usermanager-class-in-asp-net-identity.aspx).

Quando você altera seu perfil de segurança, um novo carimbo de segurança é gerado e armazenado no `SecurityStamp` campo da tabela *AspNetUsers* . Observe que o `SecurityStamp` campo é diferente do cookie de segurança. O cookie de segurança não é armazenado na `AspNetUsers` tabela (ou em qualquer outro lugar no BD de identidade). O token do cookie de segurança é autoassinado usando [DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx) e é criado com as `UserId, SecurityStamp` informações de tempo de expiração e.

O middleware do cookie verifica o cookie em cada solicitação. O `SecurityStampValidator` método na `Startup` classe atinge o DB e verifica o carimbo de segurança periodicamente, conforme especificado com o `validateInterval` . Isso ocorre apenas a cada 30 minutos (em nosso exemplo), a menos que você altere seu perfil de segurança. O intervalo de 30 minutos foi escolhido para minimizar as viagens no banco de dados. Consulte meu [tutorial de autenticação de dois fatores](index.md) para obter mais detalhes.

De acordo com os comentários no código, o `UseCookieAuthentication` método dá suporte à autenticação de cookie. O `SecurityStamp` campo e o código associado fornecem uma camada extra de segurança para seu aplicativo, quando você altera sua senha, você será desconectado do navegador com o qual fez logon. O `SecurityStampValidator.OnValidateIdentity` método permite que o aplicativo valide o token de segurança quando o usuário faz logon, que é usado quando você altera uma senha ou usa o logon externo. Isso é necessário para garantir que todos os tokens (cookies) gerados com a senha antiga sejam invalidados. No projeto de exemplo, se você alterar a senha dos usuários, um novo token será gerado para o usuário, todos os tokens anteriores serão invalidados e o `SecurityStamp` campo será atualizado.

O sistema de identidade permite que você configure seu aplicativo para que o perfil de segurança dos usuários seja alterado (por exemplo, quando o usuário altera sua senha ou altera o logon associado (como do Facebook, Google, conta Microsoft, etc.), o usuário está desconectado de todas as instâncias do navegador. Por exemplo, a imagem abaixo mostra o aplicativo de [exemplo SignOut único](https://github.com/aspnet/samples/tree/master/samples/aspnet/Identity/SingleSignOutSample) , que permite ao usuário sair de todas as instâncias do navegador (neste caso, IE, Firefox e Chrome) selecionando um botão. Como alternativa, o exemplo permite que você faça logoff apenas de uma instância específica do navegador.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image11.png)

O aplicativo de [exemplo de saída única](https://github.com/aspnet/samples/tree/master/samples/aspnet/Identity/SingleSignOutSample) mostra como ASP.net Identity permite regenerar o token de segurança. Isso é necessário para garantir que todos os tokens (cookies) gerados com a senha antiga sejam invalidados. Esse recurso fornece uma camada extra de segurança para seu aplicativo; Quando você alterar sua senha, você será desconectado onde você fez logon neste aplicativo.

O *arquivo \_ Start\IdentityConfig.cs do aplicativo* contém `ApplicationUserManager` as `EmailService` `SmsService` classes e. As `EmailService` `SmsService` classes e cada implementam a `IIdentityMessageService` interface, de modo que você tem métodos comuns em cada classe para configurar o email e o SMS. Embora este tutorial mostre apenas como adicionar notificação por email por meio do [SendGrid](http://sendgrid.com/), você pode enviar emails usando SMTP e outros mecanismos.

A `Startup` classe também contém uma chapa para adicionar logons sociais (Facebook, Twitter, etc.), consulte meu aplicativo do tutorial [MVC 5 com o Facebook, Twitter, LinkedIn e o logon do Google OAuth2](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) para obter mais informações.

Examine a `ApplicationUserManager` classe que contém as informações de identidade dos usuários e configure os seguintes recursos:

- Requisitos de força de senha.
- Bloqueio do usuário (tentativas e hora).
- Autenticação de dois fatores (2FA). Abordarei o 2FA e o SMS em outro tutorial.
- Conectar os serviços de email e SMS. (Falarei sobre o SMS em outro tutorial).

A `ApplicationUserManager` classe deriva da `UserManager<ApplicationUser>` classe genérica. `ApplicationUser` deriva de [IdentityUser](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework.identityuser.aspx). `IdentityUser` deriva da `IdentityUser` classe genérica:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample1.cs)]

As propriedades acima coincidem com as propriedades na `AspNetUsers` tabela, mostrada acima.

Os argumentos genéricos no `IUser` permitem que você derive uma classe usando tipos diferentes para a chave primária. Consulte o exemplo [ChangePK](https://github.com/aspnet/samples/tree/master/samples/aspnet/Identity/ChangePK) que mostra como alterar a chave primária de cadeia de caracteres para int ou GUID.

### <a name="applicationuser"></a>ApplicationUser

`ApplicationUser` ( `public class ApplicationUserManager : UserManager<ApplicationUser>` ) é definido em *Models\IdentityModels.cs* como:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample2.cs?highlight=8-9)]

O código realçado acima gera um [ClaimsIdentity](https://msdn.microsoft.com/library/system.security.claims.claimsidentity.aspx). A autenticação de cookie ASP.NET Identity e OWIN são baseadas em declarações, portanto, a estrutura requer que o aplicativo gere um `ClaimsIdentity` para o usuário. `ClaimsIdentity` tem informações sobre todas as declarações para o usuário, como o nome do usuário, a idade e a quais funções o usuário pertence. Você também pode adicionar mais declarações para o usuário neste estágio.

O `AuthenticationManager.SignIn` método OWIN passa os `ClaimsIdentity` sinais e no usuário:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample3.cs?highlight=4-6)]

O [aplicativo MVC 5 com o logon do Facebook, Twitter, LinkedIn e Google OAuth2](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) mostra como você pode adicionar propriedades adicionais à `ApplicationUser` classe.

## <a name="email-confirmation"></a>Confirmação de email

É uma boa ideia confirmar o email que um novo usuário registra para verificar se eles não estão representando outra pessoa (ou seja, se eles não se registraram no email de outra pessoa). Suponha que você tenha um fórum de discussão, você desejaria evitar `"bob@example.com"` o registro como `"joe@contoso.com"` . Sem confirmação por email, `"joe@contoso.com"` pode obter email indesejado de seu aplicativo. Suponha que Bob seja acidentalmente registrado como `"bib@example.com"` e não tenha notado, ele não seria capaz de usar a recuperação de senha porque o aplicativo não tem seu email correto. A confirmação por email fornece apenas proteção limitada de bots e não fornece proteção contra remetentes de spam determinados, eles têm muitos aliases de email de trabalho que podem usar para se registrar. No exemplo a seguir, o usuário não poderá alterar sua senha até que sua conta seja confirmada (por eles selecionando um link de confirmação recebido na conta de email com a qual eles se registraram.) Você pode aplicar esse fluxo de trabalho a outros cenários, por exemplo, enviar um link para confirmar e redefinir a senha em novas contas criadas pelo administrador, enviando um email ao usuário quando ele alterou seu perfil e assim por diante. Em geral, você deseja impedir que novos usuários enviem dados para seu site antes de terem sido confirmados por email, uma mensagem de texto SMS ou outro mecanismo. <a id="build"></a>

## <a name="build-a-more-complete-sample"></a>Crie um exemplo mais completo

Nesta seção, você usará o NuGet para baixar um exemplo mais completo com o qual trabalharemos.

1. Crie um novo projeto Web ***Empty** _ ASP.net.
2. No console do Gerenciador de pacotes, insira os seguintes comandos: 

    [!code-console[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample4.cmd)]

   Neste tutorial, usaremos [SendGrid](http://sendgrid.com/) para enviar email. O `Identity.Samples` pacote instala o código com o qual trabalharemos.
3. Defina o [projeto para usar SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
4. Teste a criação da conta local executando o aplicativo, selecionando o link _ *registrar** e postando o formulário de registro.
5. Selecione o link email de demonstração, que simula a confirmação de email.
6. Remova o código de confirmação de link de email de demonstração do exemplo (o `ViewBag.Link` código no controlador de conta. Consulte os `DisplayEmail` métodos de ação e e `ForgotPasswordConfirmation` exibições do Razor.

> [!WARNING]
> Se você alterar qualquer uma das configurações de segurança neste exemplo, os aplicativos de produções precisarão passar por uma auditoria de segurança que chama explicitamente as alterações feitas.

## <a name="examine-the-code-in-app_startidentityconfigcs"></a>Examinar o código no aplicativo \_ Start\IdentityConfig.cs

O exemplo mostra como criar uma conta e adicioná-la à função de *administrador* . Você deve substituir o email no exemplo pelo email que você usará para a conta de administrador. Agora, a maneira mais fácil de criar uma conta de administrador é programaticamente no `Seed` método. Esperamos ter uma ferramenta no futuro que permita criar e administrar usuários e funções. O código de exemplo permite criar e gerenciar usuários e funções, mas você deve primeiro ter uma conta de administradores para executar as páginas funções e administrador de usuário. Neste exemplo, a conta de administrador é criada quando o banco de BD é propagado.

Altere a senha e altere o nome para uma conta na qual você possa receber notificações por email.

> [!WARNING]
> Segurança-nunca armazene dados confidenciais em seu código-fonte.

Como mencionado anteriormente, a `app.CreatePerOwinContext` chamada na classe Startup adiciona retornos de chamada ao `Create` método do conteúdo do aplicativo DB, do gerente de usuário e das classes do Gerenciador de funções. O pipeline OWIN chama o `Create` método nessas classes para cada solicitação e armazena o contexto para cada classe. O controlador de conta expõe o Gerenciador de usuários do contexto HTTP (que contém o contexto OWIN):

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample5.cs)]

Quando um usuário registra uma conta local, o `HTTP Post Register` método é chamado:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample6.cs)]

O código acima usa os dados do modelo para criar uma nova conta de usuário usando o email e a senha inseridos. Se o alias de email estiver no repositório de dados, a criação da conta falhará e o formulário será exibido novamente. O `GenerateEmailConfirmationTokenAsync` método cria um token de confirmação seguro e o armazena no armazenamento de dados ASP.net Identity. O método [URL. Action](https://msdn.microsoft.com/library/dd505232(v=vs.118).aspx) cria um link que contém o `UserId` e o token de confirmação. Esse link é enviado por email para o usuário, o usuário pode selecionar no link em seu aplicativo de email para confirmar sua conta.

<a id="email"></a>

## <a name="set-up-email-confirmation"></a>Configurar confirmação de email

Acesse a página de inscrição do SendGrid e registre-se para obter uma conta gratuita. Adicione um código semelhante ao seguinte para configurar o SendGrid:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample7.cs?highlight=5)]

> [!NOTE]
> Os clientes de email frequentemente aceitam apenas mensagens de texto (sem HTML). Você deve fornecer a mensagem em texto e HTML. No exemplo de SendGrid acima, isso é feito com o `myMessage.Text` `myMessage.Html` código e mostrado acima.

O código a seguir mostra como enviar email usando a classe [MailMessage](https://msdn.microsoft.com/library/system.net.mail.mailmessage.aspx) , onde `message.Body` retorna apenas o link.

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample8.cs)]

> [!WARNING]
> Segurança-nunca armazene dados confidenciais em seu código-fonte. A conta e as credenciais são armazenadas em appSetting. No Azure, você pode armazenar esses valores com segurança na guia **[Configurar](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** na portal do Azure. Consulte [as práticas recomendadas para implantar senhas e outros dados confidenciais no ASP.net e no Azure](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).

Insira suas credenciais do SendGrid, execute o aplicativo, registre-se com um alias de email pode selecionar o link confirmar em seu email. Para saber como fazer isso com sua conta de email do [Outlook.com](http://outlook.com) , consulte configuração de [SMTP do C# de John Atten para o Host SMTP do Outlook.Com](http://typecastexception.com/post/2013/12/20/C-SMTP-Configuration-for-OutlookCom-SMTP-Host.aspx) e seu[ASP.net Identity 2,0: Configurando a validação de conta e Two-Factor Postagens de autorização](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) .

Quando um usuário seleciona o botão **registrar** , um email de confirmação contendo um token de validação é enviado para seu endereço de email.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image12.png)

O usuário recebe um email com um token de confirmação para sua conta.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image13.png)

## <a name="examine-the-code"></a>Examinar o código

O código a seguir mostra o método `POST ForgotPassword`.

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample9.cs)]

O método falhará silenciosamente se o email do usuário não tiver sido confirmado. Se um erro foi Postado para um endereço de email inválido, os usuários mal-intencionados podem usar essas informações para encontrar um userId (alias de email) válido a ser atacado.

O código a seguir mostra o `ConfirmEmail` método no controlador de conta que é chamado quando o usuário seleciona o link de confirmação no email enviado para eles:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample10.cs)]

Após o uso de um token de senha esquecida, ele é invalidado. A alteração de código a seguir no `Create` método (no *arquivo \_ Start\IdentityConfig.cs do aplicativo* ) define os tokens para expirar em 3 horas.

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample11.cs?highlight=6-8)]

Com o código acima, a senha esquecida e os tokens de confirmação de email expirarão em 3 horas. O padrão `TokenLifespan` é um dia.

O código a seguir mostra o método de confirmação de email:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample12.cs)]

 Para tornar seu aplicativo mais seguro, ASP.NET Identity dá suporte à autenticação Two-Factor (2FA). Consulte [ASP.NET Identity 2,0: Configurando a validação de conta e a autorização de Two-Factor](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) por John Atten. Embora você possa definir o bloqueio de conta em falhas de tentativa de senha de logon, essa abordagem torna seu logon suscetível a bloqueios de [dos](http://en.wikipedia.org/wiki/Denial-of-service_attack) . É recomendável usar o bloqueio de conta somente com 2FA.  
<a id="addRes"></a>

## <a name="additional-resources"></a>Recursos adicionais

- [Visão geral de provedores de armazenamento personalizados para a Identidade do ASP.NET](../extensibility/overview-of-custom-storage-providers-for-aspnet-identity.md)
- O [aplicativo MVC 5 com o logon do Facebook, Twitter, LinkedIn e Google OAuth2](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) também mostra como adicionar informações de perfil à tabela users.
- [ASP.NET MVC e identidade 2,0: Noções básicas de](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) John Atten.
- [Introdução à Identidade do ASP.NET](../getting-started/introduction-to-aspnet-identity.md)
- [Anunciando a versão RTM do ASP.net Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) by Pranav Rastogi.
