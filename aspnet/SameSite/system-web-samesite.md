---
title: Trabalhar com cookies SameSite no ASP.NET
author: rick-anderson
description: Saiba como usar o para SameSite cookies no ASP.NET
ms.author: riande
ms.date: 2/15/2019
uid: samesite/system-web-samesite
ms.openlocfilehash: 2a39663dcbfa97ae441edd9a9768172cafbaab03
ms.sourcegitcommit: 09a34635ed0e74d6c2625f6a485c78f201c689ee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/06/2020
ms.locfileid: "91763465"
---
# <a name="work-with-samesite-cookies-in-aspnet"></a>Trabalhar com cookies SameSite no ASP.NET

De [Rick Anderson](https://twitter.com/RickAndMSFT)

SameSite é um padrão de rascunho de [IETF](https://ietf.org/about/) projetado para fornecer alguma proteção contra ataques CSRF (solicitação entre sites forjada). Originalmente rascunho em [2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07), o rascunho padrão foi atualizado em [2019](https://tools.ietf.org/html/draft-west-cookie-incrementalism-00). O padrão atualizado não é compatível com versões anteriores com o padrão anterior, com as diferenças mais perceptíveis a seguir:

* Cookies sem cabeçalho SameSite são tratados como `SameSite=Lax` por padrão.
* `SameSite=None` deve ser usado para permitir o uso de cookies entre sites.
* Os cookies que Assert `SameSite=None` também devem ser marcados como `Secure` .
* Os aplicativos que usam [`<iframe>`](https://developer.mozilla.org/docs/Web/HTML/Element/iframe) o podem apresentar problemas com `sameSite=Lax` `sameSite=Strict` cookies ou porque `<iframe>` são tratados como cenários entre sites.
* O valor `SameSite=None` não é permitido pelo [padrão 2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07) e faz com que algumas implementações tratem tais cookies como `SameSite=Strict` . Consulte [suporte a navegadores mais antigos](#sob) neste documento.

A `SameSite=Lax` configuração funciona para a maioria dos cookies de aplicativo. Algumas formas de autenticação, como o [OpenID Connect](https://openid.net/connect/) (OIDC) e o [WS-Federation,](https://auth0.com/docs/protocols/ws-fed) assumem o padrão de redirecionamentos baseados em post. Os redirecionamentos baseados em POST disparam as proteções do navegador SameSite, portanto, o SameSite está desabilitado para esses componentes. A maioria dos logons [OAuth](https://oauth.net/) não são afetados devido a diferenças na forma como os fluxos de solicitação.

Cada componente ASP.NET que emite cookies precisa decidir se SameSite é apropriado.

Consulte [problemas conhecidos](#known) para problemas com aplicativos após a instalação das atualizações do 2019 .net SameSite.

## <a name="using-samesite-in-aspnet-472-and-48"></a>Usando SameSite no ASP.NET 4.7.2 e 4,8

O .NET 4.7.2 e 4,8 suporta o [padrão de rascunho 2019](https://tools.ietf.org/html/draft-west-cookie-incrementalism-00) para SameSite desde o lançamento das atualizações em dezembro de 2019. Os desenvolvedores podem controlar o valor do cabeçalho SameSite usando a [Propriedade HttpCookie. SameSite](/dotnet/api/system.web.httpcookie.samesite#System_Web_HttpCookie_SameSite)de forma programática. Definir a `SameSite` propriedade como `Strict` , `Lax` ou `None` resulta na gravação desses valores na rede com o cookie. Defini-lo como igual a `(SameSiteMode)(-1)` indica que nenhum cabeçalho SameSite deve ser incluído na rede com o cookie. A [Propriedade HttpCookie. Secure](/dotnet/api/system.web.httpcookie.secure), ou ' RequireSSL ' nos arquivos de configuração, pode ser usada para marcar o cookie como `Secure` ou não.

As novas `HttpCookie` instâncias serão padronizadas para `SameSite=(SameSiteMode)(-1)` e `Secure=false` . Esses padrões podem ser substituídos na `system.web/httpCookies` seção de configuração, em que a cadeia de caracteres `"Unspecified"` é uma sintaxe amigável somente para configuração para `(SameSiteMode)(-1)` :

```xml
<configuration>
 <system.web>
  <httpCookies sameSite="[Strict|Lax|None|Unspecified]" requireSSL="[true|false]" />
 <system.web>
<configuration>
```

O ASP.Net também emite quatro cookies específicos para estes recursos: autenticação anônima, autenticação de formulários, estado de sessão e gerenciamento de função. As instâncias desses cookies obtidas no tempo de execução podem ser manipuladas usando as `SameSite` Propriedades e, `Secure` assim como qualquer outra instância de HttpCookie. No entanto, devido ao surgimento colcha do padrão SameSite, as opções de configuração para esses quatro recursos cookies são inconsistentes. As seções e os atributos de configuração relevantes, com padrões, são mostrados abaixo. Se não houver nenhum `SameSite` `Secure` atributo ou relacionado para um recurso, o recurso será devolvido aos padrões configurados na `system.web/httpCookies` seção discutida acima.

```xml
<configuration>
 <system.web>
  <anonymousIdentification cookieRequireSSL="false" /> <!-- No config attribute for SameSite -->
  <authentication>
   <forms cookieSameSite="Lax" requireSSL="false" />
  </authentication>
  <sessionState cookieSameSite="Lax" /> <!-- No config attribute for Secure -->
  <roleManager cookieRequireSSL="false" /> <!-- No config attribute for SameSite -->
 <system.web>
<configuration>
```  

**Observação**: "não especificado" está disponível apenas para `system.web/httpCookies@sameSite` o momento. Esperamos adicionar sintaxe semelhante aos atributos cookieSameSite mostrados anteriormente em atualizações futuras. A configuração `(SameSiteMode)(-1)` no código ainda funciona em instâncias desses cookies. *

[!INCLUDE[](~/includes/MTcomments.md)]

<a name="retargeting"></a>

### <a name="retarget-net-apps"></a>Redirecionar aplicativos .NET

Para direcionar o .NET 4.7.2 ou posterior:

* Verifique se *web.config* contém o seguinte:  <!-- review, I removed `debug="true"` -->

  ```xml
  <system.web>
    <compilation targetFramework="4.7.2"/>
    <httpRuntime targetFramework="4.7.2"/>
  </system.web>

* Verify the project file contains the correct [TargetFrameworkVersion](/visualstudio/msbuild/msbuild-target-framework-and-target-platform?view=vs-2019):

  ```xml
  <TargetFrameworkVersion>v4.7.2</TargetFrameworkVersion>
  ```

  O [Guia de migração do .net](/dotnet/framework/migration-guide/) tem mais detalhes.

* Verifique se os pacotes NuGet no projeto são direcionados para a versão correta da estrutura. Você pode verificar a versão correta da estrutura examinando o arquivo *packages.config* , por exemplo:

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <packages>
    <package id="Microsoft.AspNet.Mvc" version="5.2.7" targetFramework="net472" />
    <package id="Microsoft.ApplicationInsights" version="2.4.0" targetFramework="net451" />
  </packages>
  ```

  No arquivo *packages.config* anterior, o `Microsoft.ApplicationInsights` pacote:
    * É destinado ao .NET 4.5.1.
    * Deve ter seu `targetFramework` atributo atualizado para `net472` se um pacote atualizado direcionado para o destino da estrutura existir.

<a name="nope"></a>

### <a name="net-versions-earlier-than-472"></a>Versões do .NET anteriores a 4.7.2

A Microsoft não dá suporte a versões do .NET inferiores a 4.7.2 para gravar o atributo de cookie de mesmo site. Não encontramos uma maneira confiável de:

* Verifique se o atributo foi gravado corretamente com base na versão do navegador.
* Interceptar e ajustar os cookies de autenticação e sessão em versões mais antigas do Framework.

### <a name="december-patch-behavior-changes"></a>Alterações de comportamento de patch de dezembro

A alteração de comportamento específica para .NET Framework é como a `SameSite` Propriedade interpreta o `None` valor:

* Antes do patch ter um valor `None` :
  * Não emita o atributo de nenhuma vez.
* Após o patch:
  * Um valor de `None` it significa "emitir o atributo com um valor de `None` ".
  * Um `SameSite` valor de `(SameSiteMode)(-1)` faz com que o atributo não seja emitido.

O valor padrão de SameSite para autenticação de formulários e cookies de estado de sessão foi alterado de `None` para `Lax` .

### <a name="summary-of-change-impact-on-browsers"></a>Resumo do impacto de alterações nos navegadores

Se você instalar o patch e emitir um cookie com `SameSite.None` , uma das duas coisas ocorrerá:
* O Chrome V80 tratará esse cookie de acordo com a nova implementação e não imporá as mesmas restrições de site no cookie.
* Qualquer navegador que não tenha sido atualizado para dar suporte à nova implementação seguirá a implementação antiga. A implementação antiga diz:
  * Se você vir um valor que não entende, ignore-o e alterne para as mesmas restrições de site estritas.

Então, o aplicativo é interrompido no Chrome ou você quebra vários outros lugares.

## <a name="history-and-changes"></a>Histórico e alterações

O suporte a SameSite foi implementado pela primeira vez no .NET 4.7.2 usando o [padrão de rascunho 2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07#section-4.1).

As atualizações de 19 de novembro de 2019 para o Windows atualizaram o .NET 4.7.2 + do padrão 2016 para o padrão 2019. Atualizações adicionais estão em breve para outras versões do Windows. Para obter mais informações, consulte <xref:samesite/kbs-samesite>.

 O rascunho 2019 da especificação de SameSite:

* **Não** é compatível com versões anteriores com o rascunho 2016. Para obter mais informações, consulte [dando suporte a navegadores mais antigos](#sob) neste documento.
* Especifica que os cookies são tratados como `SameSite=Lax` por padrão.
* Especifica os cookies que o Assert explicitamente `SameSite=None` para habilitar a entrega entre sites também devem ser marcados como `Secure` .
* O é suportado por patches emitidos conforme descrito em KB listados acima.
* Está agendado para ser habilitado pelo [Chrome](https://chromestatus.com/feature/5088147346030592) por padrão em [fevereiro de 2020](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html). Os navegadores começaram a passar para esse padrão em 2019.

<a name="known"></a>

## <a name="known-issues"></a>Problemas conhecidos

Como as especificações de rascunho 2016 e 2019 não são compatíveis, a atualização de novembro de 2019 do .NET Framework introduz algumas alterações que podem estar sendo interrompidas.

* Os cookies de estado de sessão e de autenticação de formulários agora são gravados na rede como `Lax` em vez de não especificados.
  * Enquanto a maioria dos aplicativos trabalha com `SameSite=Lax` cookies, os aplicativos que são postados em sites ou aplicativos que usam `iframe` o podem descobrir que seus Estados de sessão ou cookies de autorização de formulários não estão sendo usados conforme o esperado. Para corrigir isso, altere o `cookieSameSite` valor na seção de configuração apropriada, conforme discutido anteriormente.
* As HttpCookies que definem explicitamente `SameSite=None` no código ou na configuração agora têm esse valor gravado com o cookie, enquanto ele foi omitido anteriormente. Isso pode causar problemas com navegadores mais antigos que dão suporte apenas ao padrão de rascunho 2016.
  * Ao direcionar os navegadores que dão suporte ao padrão de rascunho 2019 com `SameSite=None` cookies, lembre-se também de marcá-los `Secure` ou eles podem não ser reconhecidos.
  * Para reverter para o comportamento de 2016 de não gravar `SameSite=None` , use a configuração de aplicativo `aspnet:SupressSameSiteNone=true` . Observe que isso será aplicado a todos os HttpCookies no aplicativo.

### <a name="azure-app-servicesamesite-cookie-handling"></a>Serviço de Azure App — tratamento de cookies SameSite

Consulte [serviço de Azure app – tratamento de cookies SameSite e .NET Framework patch do 4.7.2](https://azure.microsoft.com/updates/app-service-samesite-cookie-update/) para obter informações sobre como Azure app serviço está configurando comportamentos de SameSite em aplicativos .NET 4.7.2.

<a name="sob"></a>

## <a name="supporting-older-browsers"></a>Suporte a navegadores mais antigos

O padrão de 2016 SameSite exigiu que valores desconhecidos devam ser tratados como `SameSite=Strict` valores. Os aplicativos acessados de navegadores mais antigos que dão suporte ao padrão de 2016 SameSite podem falhar quando obtêm uma propriedade SameSite com um valor de `None` . Os aplicativos Web devem implementar a detecção do navegador se pretenderem oferecer suporte a navegadores mais antigos. O ASP.NET não implementa a detecção de navegador, pois os valores dos agentes de usuário são altamente voláteis e mudam com frequência.

A abordagem da Microsoft para corrigir o problema é ajudá-lo a implementar componentes de detecção de navegador para remover o `sameSite=None` atributo de cookies se um navegador for conhecido por não oferecer suporte a ele. O Conselho do Google foi emitir cookies duplos, um com o novo atributo e um sem o atributo. No entanto, consideramos o aviso do Google limitado. Alguns navegadores, especialmente os navegadores móveis têm limites muito pequenos no número de cookies de um site ou um nome de domínio pode enviar. O envio de vários cookies, especialmente cookies grandes, como cookies de autenticação, pode alcançar o limite do navegador móvel muito rapidamente, causando falhas de aplicativo difíceis de diagnosticar e corrigir. Além de uma estrutura, há um grande ecossistema de código e componentes de terceiros que podem não ser atualizados para usar uma abordagem de cookie duplo.

O código de detecção do navegador usado nos projetos de exemplo neste [repositório GitHub]() está contido em dois arquivos

* [SameSiteSupport.cs C#](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/SameSiteSupport.cs)
* [VB SameSiteSupport. vb](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/SameSiteSupport.vb)

Essas detecções são os agentes de navegador mais comuns que vimos que dão suporte ao padrão 2016 e para os quais o atributo precisa ser completamente removido. Isso não significa uma implementação completa:

* Seu aplicativo pode ver os navegadores que nossos sites de teste não têm.
* Você deve estar preparado para adicionar detecções conforme necessário para seu ambiente.

A forma de conexão da detecção varia de acordo com a versão do .NET e a estrutura da Web que você está usando. O código a seguir pode ser chamado no site de chamada do [HttpCookie](/dotnet/api/system.web.httpcookie) :

[!code-csharp[](sample/SameSiteCheck.cs?name=snippet)]

Consulte os seguintes tópicos de cookies do ASP.NET 4.7.2 SameSite:

* [C# MVC](xref:samesite/csMVC)
* [C# WebForms](xref:samesite/CSharpWebForms)
* [VB WebForms](xref:samesite/vbWF)
* [VB MVC](xref:samesite/vbMVC)
<!--
* <xref:samesite/csMVC>
* <xref:samesite/CSharpWebForms>
* <xref:samesite/vbWF>
* <xref:samesite/vbMVC>
-->

### <a name="ensuring-your-site-redirects-to-https"></a>Garantindo que seu site Redirecione para HTTPS

Para ASP.NET 4. x, WebForms e MVC, o recurso [de reescrita de URL do IIS](/iis/extensions/url-rewrite-module/creating-rewrite-rules-for-the-url-rewrite-module) pode ser usado para redirecionar todas as solicitações para https. O XML a seguir mostra uma regra de exemplo:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <system.webServer>
    <rewrite>
      <rules>
        <rule name="Redirect to https" stopProcessing="true">
          <match url="(.*)"/>
          <conditions>
            <add input="{HTTPS}" pattern="Off"/>
            <add input="{REQUEST_METHOD}" pattern="^get$|^head$" />
          </conditions>
          <action type="Redirect" url="https://{HTTP_HOST}/{R:1}" redirectType="Permanent"/>
        </rule>
      </rules>
    </rewrite>
  </system.webServer>
</configuration>
```

Em instalações locais da [regravação de URL do IIS](https://www.iis.net/downloads/microsoft/url-rewrite) , há um recurso opcional que pode precisar de instalação.

## <a name="test-apps-for-samesite-problems"></a>Testar aplicativos para problemas de SameSite

Você deve testar seu aplicativo com os navegadores aos quais dá suporte e percorrer os cenários que envolvem cookies. Cenários de cookie normalmente envolvem

* Formulários de logon
* Mecanismos de logon externos, como Facebook, Azure AD, OAuth e OIDC
* Páginas que aceitam solicitações de outros sites
* Páginas em seu aplicativo projetadas para serem inseridas em IFrames

Você deve verificar se os cookies são criados, persistidos e excluídos corretamente em seu aplicativo.

Aplicativos que interagem com sites remotos, como por meio de logon de terceiros, precisam:

* Teste a interação em vários navegadores.
* Aplique a [detecção e mitigação do navegador](#sob) discutidas neste documento.

Teste os aplicativos Web usando uma versão do cliente que pode aceitar o novo comportamento de SameSite. O Chrome, o Firefox e o Chromium Edge têm novos sinalizadores de recurso de aceitação que podem ser usados para teste. Depois que seu aplicativo aplicar os patches do SameSite, teste-o com versões mais antigas do cliente, especialmente o Safari. Para obter mais informações, consulte [dando suporte a navegadores mais antigos](#sob) neste documento.

### <a name="test-with-chrome"></a>Teste com o Chrome

O Chrome 78 + fornece resultados enganosos porque tem uma mitigação temporária em vigor. A mitigação do Chrome 78 + temporária permite cookies com menos de dois minutos. O Chrome 76 ou 77 com os sinalizadores de teste apropriados habilitados fornece resultados mais precisos. Para testar o novo comportamento de SameSite, alterne `chrome://flags/#same-site-by-default-cookies` para **habilitado**. Versões mais antigas do Chrome (75 e inferior) são relatadas para falha com a nova `None` configuração. Consulte [suporte a navegadores mais antigos](#sob) neste documento.

O Google não disponibiliza versões mais antigas do Chrome. Siga as instruções em [baixar o Chromium](https://www.chromium.org/getting-involved/download-chromium) para testar versões anteriores do Chrome. **Não** Baixe o Chrome de links fornecidos pela pesquisa de versões mais antigas do Chrome.

* [Chromium 76 Win64](https://commondatastorage.googleapis.com/chromium-browser-snapshots/index.html?prefix=Win_x64/664998/)
* [Chromium 74 Win64](https://commondatastorage.googleapis.com/chromium-browser-snapshots/index.html?prefix=Win_x64/638880/)
* Se você não estiver usando uma versão de 64 bits do Windows, poderá usar o [Visualizador do OmahaProxy](https://omahaproxy.appspot.com/) para procurar qual ramificação do Chromium corresponde ao Chrome 74 (v 74.0.3729.108) usando as [instruções fornecidas pelo Chromium](https://www.chromium.org/getting-involved/download-chromium).

A partir da versão canário `80.0.3975.0` , a mitigação temporária pode ser desativada para fins de teste usando o novo sinalizador `--enable-features=SameSiteDefaultChecksMethodRigorously` para permitir o teste de sites e serviços no estado final eventual do recurso em que a mitigação foi removida. Para obter mais informações, consulte o Chromium Projects [SameSite updates](https://www.chromium.org/updates/same-site)

#### <a name="test-with-chrome-80"></a>Teste com o Chrome 80 +

[Baixe](https://www.google.com/chrome/) uma versão do Chrome que dê suporte a seu novo atributo. No momento da gravação, a versão atual é o Chrome 80. O Chrome 80 precisa do sinalizador `chrome://flags/#same-site-by-default-cookies` habilitado para usar o novo comportamento. Você também deve habilitar ( `chrome://flags/#cookies-without-same-site-must-be-secure` ) para testar o comportamento futuro de cookies que não têm o atributo sameSite habilitado. O Chrome 80 está no destino para fazer a alternância tratar cookies sem o atributo como `SameSite=Lax` , embora com um período de carência cronometrado para determinadas solicitações. Para desabilitar o período de carência cronometrado, o Chrome 80 pode ser iniciado com o seguinte argumento de linha de comando:

`--enable-features=SameSiteDefaultChecksMethodRigorously`

O Chrome 80 tem mensagens de aviso no console do navegador sobre atributos sameSite ausentes. Use F12 para abrir o console do navegador.

### <a name="test-with-safari"></a>Teste com o Safari

O Safari 12 implementou estritamente o rascunho anterior e falha quando o novo `None` valor está em um cookie. `None` é evitado por meio do código de detecção de navegador que [dá suporte a navegadores mais antigos](#sob) neste documento. Teste OS logons de estilo do sistema operacional baseado no Safari 12, Safari 13 e WebKit usando MSAL, ADAL ou qualquer biblioteca que você esteja usando. O problema depende da versão subjacente do sistema operacional. OSX Mojave (10,14) e iOS 12 são conhecidos por ter problemas de compatibilidade com o novo comportamento de SameSite. Atualizar o sistema operacional para OSX Catalina (10,15) ou iOS 13 corrige o problema. No momento, o Safari não tem um sinalizador de aceitação para testar o novo comportamento de especificação.

### <a name="test-with-firefox"></a>Testar com o Firefox

O suporte do Firefox para o novo padrão pode ser testado na versão 68 +, optando na `about:config` página pelo sinalizador de recurso `network.cookie.sameSite.laxByDefault` . Não há relatórios de problemas de compatibilidade com versões mais antigas do Firefox.

### <a name="test-with-edge-legacy-browser"></a>Testar com o navegador de borda (Herdado)

O Edge dá suporte ao antigo SameSite padrão. A versão do Edge 44 + não tem nenhum problema de compatibilidade conhecido com o novo padrão.

### <a name="test-with-edge-chromium"></a>Testar com borda (Chromium)

Os sinalizadores SameSite são definidos na `edge://flags/#same-site-by-default-cookies` página. Nenhum problema de compatibilidade foi descoberto com o Edge Chromium.

### <a name="test-with-electron"></a>Testar com o

As versões do at-SS são versões mais antigas do Chromium. Por exemplo, a versão do at-SS usada pelas equipes é Chromium 66, que exibe o comportamento mais antigo. Você deve executar seu próprio teste de compatibilidade com a versão do uso de digital que seu produto usa. Consulte [suporte a navegadores mais antigos](#sob).

## <a name="reverting-samesite-patches"></a>Revertendo patches do SameSite

Você pode reverter o comportamento sameSite atualizado em aplicativos .NET Framework para o comportamento anterior em que o atributo sameSite não é emitido para um valor de e `None` reverter a autenticação e os cookies de sessão para não emitir o valor. Isso deve ser exibido como uma *correção extremamente temporária*, pois as alterações do Chrome interromperão quaisquer solicitações post externas ou autenticação para usuários que usam navegadores que dão suporte às alterações no padrão.

### <a name="reverting-net-472-behavior"></a>Revertendo o comportamento do .NET 4.7.2

Atualize *web.config* para incluir as seguintes definições de configuração:

```xml
<configuration> 
  <appSettings>
    <add key="aspnet:SuppressSameSiteNone" value="true" />
  </appSettings>
 
  <system.web> 
    <authentication> 
      <forms cookieSameSite="None" /> 
    </authentication> 
    <sessionState cookieSameSite="None" /> 
  </system.web> 
</configuration>
```

## <a name="additional-resources"></a>Recursos adicionais

* [Próximas alterações de cookie SameSite em ASP.NET e ASP.NET Core](https://devblogs.microsoft.com/aspnet/upcoming-samesite-cookie-changes-in-asp-net-and-asp-net-core/)
* [Dicas para testar e depurar SameSite-por-padrão e "SameSite = None; Proteger "cookies](https://www.chromium.org/updates/same-site/test-debug)
* [Blog do Chromium: desenvolvedores: Prepare-se para o New SameSite = None; Configurações de cookie seguro](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html)
* [SameSite cookies explicados](https://web.dev/samesite-cookies-explained/)
* [Atualizações do Chrome](https://www.chromium.org/updates/same-site)
* [Patches do .NET SameSite](/aspnet/samesite/kbs-samesite)
* [Informações do mesmo site de aplicativos Web do Azure](https://azure.microsoft.com/updates/app-service-samesite-cookie-update/)
* [Informações do mesmo site do Azure ActiveDirectory](/azure/active-directory/develop/howto-handle-samesite-cookie-changes-chrome-browser)
