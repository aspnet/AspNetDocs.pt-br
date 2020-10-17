---
title: Trabalhar com cookies SameSite no ASP.NET
author: rick-anderson
description: Saiba como usar o para SameSite cookies no ASP.NET
ms.author: riande
ms.date: 2/15/2019
uid: samesite/system-web-samesite
ms.openlocfilehash: d50f157c6d92cb56cb6c59381af9139d1d3d1d3d
ms.sourcegitcommit: a309ca7af61e59195beb745b501a1a9f06fcd493
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/16/2020
ms.locfileid: "92119357"
---
# <a name="work-with-samesite-cookies-in-aspnet"></a><span data-ttu-id="d2d29-103">Trabalhar com cookies SameSite no ASP.NET</span><span class="sxs-lookup"><span data-stu-id="d2d29-103">Work with SameSite cookies in ASP.NET</span></span>

<span data-ttu-id="d2d29-104">De [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d2d29-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="d2d29-105">SameSite é um padrão de rascunho de [IETF](https://ietf.org/about/) projetado para fornecer alguma proteção contra ataques CSRF (solicitação entre sites forjada).</span><span class="sxs-lookup"><span data-stu-id="d2d29-105">SameSite is an [IETF](https://ietf.org/about/) draft standard designed to provide some protection against cross-site request forgery (CSRF) attacks.</span></span> <span data-ttu-id="d2d29-106">Originalmente rascunho em [2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07), o rascunho padrão foi atualizado em [2019](https://tools.ietf.org/html/draft-west-cookie-incrementalism-00).</span><span class="sxs-lookup"><span data-stu-id="d2d29-106">Originally drafted in [2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07), the draft standard was updated in [2019](https://tools.ietf.org/html/draft-west-cookie-incrementalism-00).</span></span> <span data-ttu-id="d2d29-107">O padrão atualizado não é compatível com versões anteriores com o padrão anterior, com as diferenças mais perceptíveis a seguir:</span><span class="sxs-lookup"><span data-stu-id="d2d29-107">The updated standard is not backward compatible with the previous standard, with the following being the most noticeable differences:</span></span>

* <span data-ttu-id="d2d29-108">Cookies sem cabeçalho SameSite são tratados como `SameSite=Lax` por padrão.</span><span class="sxs-lookup"><span data-stu-id="d2d29-108">Cookies without SameSite header are treated as `SameSite=Lax` by default.</span></span>
* <span data-ttu-id="d2d29-109">`SameSite=None` deve ser usado para permitir o uso de cookies entre sites.</span><span class="sxs-lookup"><span data-stu-id="d2d29-109">`SameSite=None` must be used to allow cross-site cookie use.</span></span>
* <span data-ttu-id="d2d29-110">Os cookies que Assert `SameSite=None` também devem ser marcados como `Secure` .</span><span class="sxs-lookup"><span data-stu-id="d2d29-110">Cookies that assert `SameSite=None` must also be marked as `Secure`.</span></span>
* <span data-ttu-id="d2d29-111">Os aplicativos que usam [`<iframe>`](https://developer.mozilla.org/docs/Web/HTML/Element/iframe) o podem apresentar problemas com `sameSite=Lax` `sameSite=Strict` cookies ou porque `<iframe>` são tratados como cenários entre sites.</span><span class="sxs-lookup"><span data-stu-id="d2d29-111">Applications that use [`<iframe>`](https://developer.mozilla.org/docs/Web/HTML/Element/iframe) may experience issues with `sameSite=Lax` or `sameSite=Strict` cookies because `<iframe>` is treated as cross-site scenarios.</span></span>
* <span data-ttu-id="d2d29-112">O valor `SameSite=None` não é permitido pelo [padrão 2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07) e faz com que algumas implementações tratem tais cookies como `SameSite=Strict` .</span><span class="sxs-lookup"><span data-stu-id="d2d29-112">The value `SameSite=None` is not allowed by the [2016 standard](https://tools.ietf.org/html/draft-west-first-party-cookies-07) and causes some implementations to treat such cookies as `SameSite=Strict`.</span></span> <span data-ttu-id="d2d29-113">Consulte [suporte a navegadores mais antigos](#sob) neste documento.</span><span class="sxs-lookup"><span data-stu-id="d2d29-113">See [Supporting older browsers](#sob) in this document.</span></span>

<span data-ttu-id="d2d29-114">A `SameSite=Lax` configuração funciona para a maioria dos cookies de aplicativo.</span><span class="sxs-lookup"><span data-stu-id="d2d29-114">The `SameSite=Lax` setting works for most application cookies.</span></span> <span data-ttu-id="d2d29-115">Algumas formas de autenticação, como o [OpenID Connect](https://openid.net/connect/) (OIDC) e o [WS-Federation,](https://auth0.com/docs/protocols/ws-fed) assumem o padrão de redirecionamentos baseados em post.</span><span class="sxs-lookup"><span data-stu-id="d2d29-115">Some forms of authentication like [OpenID Connect](https://openid.net/connect/) (OIDC) and [WS-Federation](https://auth0.com/docs/protocols/ws-fed) default to POST based redirects.</span></span> <span data-ttu-id="d2d29-116">Os redirecionamentos baseados em POST disparam as proteções do navegador SameSite, portanto, o SameSite está desabilitado para esses componentes.</span><span class="sxs-lookup"><span data-stu-id="d2d29-116">The POST based redirects trigger the SameSite browser protections, so SameSite is disabled for these components.</span></span> <span data-ttu-id="d2d29-117">A maioria dos logons [OAuth](https://oauth.net/) não são afetados devido a diferenças na forma como os fluxos de solicitação.</span><span class="sxs-lookup"><span data-stu-id="d2d29-117">Most [OAuth](https://oauth.net/) logins are not affected due to differences in how the request flows.</span></span>

<span data-ttu-id="d2d29-118">Cada componente ASP.NET que emite cookies precisa decidir se SameSite é apropriado.</span><span class="sxs-lookup"><span data-stu-id="d2d29-118">Each ASP.NET component that emits cookies needs to decide if SameSite is appropriate.</span></span>

<span data-ttu-id="d2d29-119">Consulte [problemas conhecidos](#known) para problemas com aplicativos após a instalação das atualizações do 2019 .net SameSite.</span><span class="sxs-lookup"><span data-stu-id="d2d29-119">See [Known Issues](#known) for problems with applications after installing the 2019 .Net SameSite updates.</span></span>

## <a name="using-samesite-in-aspnet-472-and-48"></a><span data-ttu-id="d2d29-120">Usando SameSite no ASP.NET 4.7.2 e 4,8</span><span class="sxs-lookup"><span data-stu-id="d2d29-120">Using SameSite in ASP.NET 4.7.2 and 4.8</span></span>

<span data-ttu-id="d2d29-121">O .NET 4.7.2 e 4,8 suporta o [padrão de rascunho 2019](https://tools.ietf.org/html/draft-west-cookie-incrementalism-00) para SameSite desde o lançamento das atualizações em dezembro de 2019.</span><span class="sxs-lookup"><span data-stu-id="d2d29-121">.Net 4.7.2 and 4.8 supports the [2019 draft standard](https://tools.ietf.org/html/draft-west-cookie-incrementalism-00) for SameSite since the release of updates in December 2019.</span></span> <span data-ttu-id="d2d29-122">Os desenvolvedores podem controlar o valor do cabeçalho SameSite usando a [Propriedade HttpCookie. SameSite](/dotnet/api/system.web.httpcookie.samesite#System_Web_HttpCookie_SameSite)de forma programática.</span><span class="sxs-lookup"><span data-stu-id="d2d29-122">Developers are able to programmatically control the value of the SameSite header using the [HttpCookie.SameSite property](/dotnet/api/system.web.httpcookie.samesite#System_Web_HttpCookie_SameSite).</span></span> <span data-ttu-id="d2d29-123">Definir a `SameSite` propriedade como `Strict` , `Lax` ou `None` resulta na gravação desses valores na rede com o cookie.</span><span class="sxs-lookup"><span data-stu-id="d2d29-123">Setting the `SameSite` property to `Strict`, `Lax`, or `None` results in those values being written on the network with the cookie.</span></span> <span data-ttu-id="d2d29-124">Defini-lo como igual a `(SameSiteMode)(-1)` indica que nenhum cabeçalho SameSite deve ser incluído na rede com o cookie.</span><span class="sxs-lookup"><span data-stu-id="d2d29-124">Setting it equal to `(SameSiteMode)(-1)` indicates that no SameSite header should be included on the network with the cookie.</span></span> <span data-ttu-id="d2d29-125">A [Propriedade HttpCookie. Secure](/dotnet/api/system.web.httpcookie.secure), ou ' RequireSSL ' nos arquivos de configuração, pode ser usada para marcar o cookie como `Secure` ou não.</span><span class="sxs-lookup"><span data-stu-id="d2d29-125">The [HttpCookie.Secure Property](/dotnet/api/system.web.httpcookie.secure), or 'requireSSL' in config files, can be used to mark the cookie as `Secure` or not.</span></span>

<span data-ttu-id="d2d29-126">As novas `HttpCookie` instâncias serão padronizadas para `SameSite=(SameSiteMode)(-1)` e `Secure=false` .</span><span class="sxs-lookup"><span data-stu-id="d2d29-126">New `HttpCookie` instances will default to `SameSite=(SameSiteMode)(-1)` and `Secure=false`.</span></span> <span data-ttu-id="d2d29-127">Esses padrões podem ser substituídos na `system.web/httpCookies` seção de configuração, em que a cadeia de caracteres `"Unspecified"` é uma sintaxe amigável somente para configuração para `(SameSiteMode)(-1)` :</span><span class="sxs-lookup"><span data-stu-id="d2d29-127">These defaults can be overridden in the `system.web/httpCookies` configuration section, where the string `"Unspecified"` is a friendly configuration-only syntax for `(SameSiteMode)(-1)`:</span></span>

```xml
<configuration>
 <system.web>
  <httpCookies sameSite="[Strict|Lax|None|Unspecified]" requireSSL="[true|false]" />
 <system.web>
<configuration>
```

<span data-ttu-id="d2d29-128">O ASP.Net também emite quatro cookies específicos para estes recursos: autenticação anônima, autenticação de formulários, estado de sessão e gerenciamento de função.</span><span class="sxs-lookup"><span data-stu-id="d2d29-128">ASP.Net also issues four specific cookies of its own for these features: Anonymous Authentication, Forms Authentication, Session State, and Role Management.</span></span> <span data-ttu-id="d2d29-129">As instâncias desses cookies obtidas no tempo de execução podem ser manipuladas usando as `SameSite` Propriedades e, `Secure` assim como qualquer outra instância de HttpCookie.</span><span class="sxs-lookup"><span data-stu-id="d2d29-129">Instances of these cookies obtained in runtime can be manipulated using the `SameSite` and `Secure` properties just like any other HttpCookie instance.</span></span> <span data-ttu-id="d2d29-130">No entanto, devido ao surgimento colcha do padrão SameSite, as opções de configuração para esses quatro recursos cookies são inconsistentes.</span><span class="sxs-lookup"><span data-stu-id="d2d29-130">However, due to the patchwork emergence of the SameSite standard, configuration options for these four features cookies is inconsistent.</span></span> <span data-ttu-id="d2d29-131">As seções e os atributos de configuração relevantes, com padrões, são mostrados abaixo.</span><span class="sxs-lookup"><span data-stu-id="d2d29-131">The relevant configuration sections and attributes, with defaults, are shown below.</span></span> <span data-ttu-id="d2d29-132">Se não houver nenhum `SameSite` `Secure` atributo ou relacionado para um recurso, o recurso será devolvido aos padrões configurados na `system.web/httpCookies` seção discutida acima.</span><span class="sxs-lookup"><span data-stu-id="d2d29-132">If there is no `SameSite` or `Secure` related attribute for a feature, then the feature will fall back on the defaults configured in the `system.web/httpCookies` section discussed above.</span></span>

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

<span data-ttu-id="d2d29-133">**Observação**: "não especificado" está disponível apenas para `system.web/httpCookies@sameSite` o momento.</span><span class="sxs-lookup"><span data-stu-id="d2d29-133">**Note**: 'Unspecified' is only available to `system.web/httpCookies@sameSite` at the moment.</span></span> <span data-ttu-id="d2d29-134">Esperamos adicionar sintaxe semelhante aos atributos cookieSameSite mostrados anteriormente em atualizações futuras.</span><span class="sxs-lookup"><span data-stu-id="d2d29-134">We hope to add similar syntax to the previously shown cookieSameSite attributes in future updates.</span></span> <span data-ttu-id="d2d29-135">A configuração `(SameSiteMode)(-1)` no código ainda funciona em instâncias desses cookies. \*</span><span class="sxs-lookup"><span data-stu-id="d2d29-135">Setting `(SameSiteMode)(-1)` in code still works on instances of these cookies.\*</span></span>

[!INCLUDE[](~/includes/MTcomments.md)]

<a name="retargeting"></a>

### <a name="retarget-net-apps"></a><span data-ttu-id="d2d29-136">Redirecionar aplicativos .NET</span><span class="sxs-lookup"><span data-stu-id="d2d29-136">Retarget .NET apps</span></span>

<span data-ttu-id="d2d29-137">Para direcionar o .NET 4.7.2 ou posterior:</span><span class="sxs-lookup"><span data-stu-id="d2d29-137">To target .NET 4.7.2 or later:</span></span>

* <span data-ttu-id="d2d29-138">Verifique se *web.config* contém o seguinte:</span><span class="sxs-lookup"><span data-stu-id="d2d29-138">Ensure *web.config* contains the following:</span></span>  <!-- review, I removed `debug="true"` -->

  ```xml
  <system.web>
    <compilation targetFramework="4.7.2"/>
    <httpRuntime targetFramework="4.7.2"/>
  </system.web>

* Verify the project file contains the correct [TargetFrameworkVersion](/visualstudio/msbuild/msbuild-target-framework-and-target-platform?view=vs-2019):

  ```xml
  <TargetFrameworkVersion>v4.7.2</TargetFrameworkVersion>
  ```

  <span data-ttu-id="d2d29-139">O [Guia de migração do .net](/dotnet/framework/migration-guide/) tem mais detalhes.</span><span class="sxs-lookup"><span data-stu-id="d2d29-139">The [.NET Migration Guide](/dotnet/framework/migration-guide/) has more details.</span></span>

* <span data-ttu-id="d2d29-140">Verifique se os pacotes NuGet no projeto são direcionados para a versão correta da estrutura.</span><span class="sxs-lookup"><span data-stu-id="d2d29-140">Verify NuGet packages in the project are targeted at the correct framework version.</span></span> <span data-ttu-id="d2d29-141">Você pode verificar a versão correta da estrutura examinando o arquivo *packages.config* , por exemplo:</span><span class="sxs-lookup"><span data-stu-id="d2d29-141">You can verify the correct framework version by examining the *packages.config* file, for example:</span></span>

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <packages>
    <package id="Microsoft.AspNet.Mvc" version="5.2.7" targetFramework="net472" />
    <package id="Microsoft.ApplicationInsights" version="2.4.0" targetFramework="net451" />
  </packages>
  ```

  <span data-ttu-id="d2d29-142">No arquivo *packages.config* anterior, o `Microsoft.ApplicationInsights` pacote:</span><span class="sxs-lookup"><span data-stu-id="d2d29-142">In the preceding *packages.config* file, the `Microsoft.ApplicationInsights` package:</span></span>
    * <span data-ttu-id="d2d29-143">É destinado ao .NET 4.5.1.</span><span class="sxs-lookup"><span data-stu-id="d2d29-143">Is  targeted against .NET 4.5.1.</span></span>
    * <span data-ttu-id="d2d29-144">Deve ter seu `targetFramework` atributo atualizado para `net472` se um pacote atualizado direcionado para o destino da estrutura existir.</span><span class="sxs-lookup"><span data-stu-id="d2d29-144">Should have its `targetFramework` attribute updated to `net472` if an updated package targeting your framework target exists.</span></span>

<a name="nope"></a>

### <a name="net-versions-earlier-than-472"></a><span data-ttu-id="d2d29-145">Versões do .NET anteriores a 4.7.2</span><span class="sxs-lookup"><span data-stu-id="d2d29-145">.NET versions earlier than 4.7.2</span></span>

<span data-ttu-id="d2d29-146">A Microsoft não dá suporte a versões do .NET inferiores a 4.7.2 para gravar o atributo de cookie de mesmo site.</span><span class="sxs-lookup"><span data-stu-id="d2d29-146">Microsoft does not support .NET versions lower that 4.7.2 for writing the same-site cookie attribute.</span></span> <span data-ttu-id="d2d29-147">Não encontramos uma maneira confiável de:</span><span class="sxs-lookup"><span data-stu-id="d2d29-147">We have not found a reliable way to:</span></span>

* <span data-ttu-id="d2d29-148">Verifique se o atributo foi gravado corretamente com base na versão do navegador.</span><span class="sxs-lookup"><span data-stu-id="d2d29-148">Ensure the attribute is written correctly based on browser version.</span></span>
* <span data-ttu-id="d2d29-149">Interceptar e ajustar os cookies de autenticação e sessão em versões mais antigas do Framework.</span><span class="sxs-lookup"><span data-stu-id="d2d29-149">Intercept and adjust authentication and session cookies on older framework versions.</span></span>

### <a name="december-patch-behavior-changes"></a><span data-ttu-id="d2d29-150">Alterações de comportamento de patch de dezembro</span><span class="sxs-lookup"><span data-stu-id="d2d29-150">December patch behavior changes</span></span>

<span data-ttu-id="d2d29-151">A alteração de comportamento específica para .NET Framework é como a `SameSite` Propriedade interpreta o `None` valor:</span><span class="sxs-lookup"><span data-stu-id="d2d29-151">The specific behavior change for .NET Framework is how the `SameSite` property interprets the `None` value:</span></span>

* <span data-ttu-id="d2d29-152">Antes do patch ter um valor `None` :</span><span class="sxs-lookup"><span data-stu-id="d2d29-152">Before the patch a value of `None` meant:</span></span>
  * <span data-ttu-id="d2d29-153">Não emita o atributo de nenhuma vez.</span><span class="sxs-lookup"><span data-stu-id="d2d29-153">Do not emit the attribute at all.</span></span>
* <span data-ttu-id="d2d29-154">Após o patch:</span><span class="sxs-lookup"><span data-stu-id="d2d29-154">After the patch:</span></span>
  * <span data-ttu-id="d2d29-155">Um valor de `None` significa "emitir o atributo com um valor de `None` ".</span><span class="sxs-lookup"><span data-stu-id="d2d29-155">A value of `None` means "Emit the attribute with a value of `None`".</span></span>
  * <span data-ttu-id="d2d29-156">Um `SameSite` valor de `(SameSiteMode)(-1)` faz com que o atributo não seja emitido.</span><span class="sxs-lookup"><span data-stu-id="d2d29-156">A `SameSite` value of `(SameSiteMode)(-1)` causes the attribute not to be emitted.</span></span>

<span data-ttu-id="d2d29-157">O valor padrão de SameSite para autenticação de formulários e cookies de estado de sessão foi alterado de `None` para `Lax` .</span><span class="sxs-lookup"><span data-stu-id="d2d29-157">The default SameSite value for forms authentication and session state cookies was changed from `None` to `Lax`.</span></span>

### <a name="summary-of-change-impact-on-browsers"></a><span data-ttu-id="d2d29-158">Resumo do impacto de alterações nos navegadores</span><span class="sxs-lookup"><span data-stu-id="d2d29-158">Summary of change impact on browsers</span></span>

<span data-ttu-id="d2d29-159">Se você instalar o patch e emitir um cookie com `SameSite.None` , uma das duas coisas ocorrerá:</span><span class="sxs-lookup"><span data-stu-id="d2d29-159">If you install the patch and issue a cookie with `SameSite.None`, one of two things will happen:</span></span>
* <span data-ttu-id="d2d29-160">O Chrome V80 tratará esse cookie de acordo com a nova implementação e não imporá as mesmas restrições de site no cookie.</span><span class="sxs-lookup"><span data-stu-id="d2d29-160">Chrome v80 will treat this cookie according to the new implementation, and not enforce same site restrictions on the cookie.</span></span>
* <span data-ttu-id="d2d29-161">Qualquer navegador que não tenha sido atualizado para dar suporte à nova implementação seguirá a implementação antiga.</span><span class="sxs-lookup"><span data-stu-id="d2d29-161">Any browser that has not been updated to support the new implementation will follow the old implementation.</span></span> <span data-ttu-id="d2d29-162">A implementação antiga diz:</span><span class="sxs-lookup"><span data-stu-id="d2d29-162">The old implementation says:</span></span>
  * <span data-ttu-id="d2d29-163">Se você vir um valor que não entende, ignore-o e alterne para as mesmas restrições de site estritas.</span><span class="sxs-lookup"><span data-stu-id="d2d29-163">If you see a value you don't understand, ignore it and switch to strict same site restrictions.</span></span>

<span data-ttu-id="d2d29-164">Então, o aplicativo é interrompido no Chrome ou você quebra vários outros lugares.</span><span class="sxs-lookup"><span data-stu-id="d2d29-164">So either the app breaks in Chrome, or you break in numerous other places.</span></span>

## <a name="history-and-changes"></a><span data-ttu-id="d2d29-165">Histórico e alterações</span><span class="sxs-lookup"><span data-stu-id="d2d29-165">History and changes</span></span>

<span data-ttu-id="d2d29-166">O suporte a SameSite foi implementado pela primeira vez no .NET 4.7.2 usando o [padrão de rascunho 2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07#section-4.1).</span><span class="sxs-lookup"><span data-stu-id="d2d29-166">SameSite support was first implemented in .NET 4.7.2 using the [2016 draft standard](https://tools.ietf.org/html/draft-west-first-party-cookies-07#section-4.1).</span></span>

<span data-ttu-id="d2d29-167">As atualizações de 19 de novembro de 2019 para o Windows atualizaram o .NET 4.7.2 + do padrão 2016 para o padrão 2019.</span><span class="sxs-lookup"><span data-stu-id="d2d29-167">The November 19, 2019 updates for Windows updated .NET 4.7.2+ from the 2016 standard to the 2019 standard.</span></span> <span data-ttu-id="d2d29-168">Atualizações adicionais estão em breve para outras versões do Windows.</span><span class="sxs-lookup"><span data-stu-id="d2d29-168">Additional updates are forthcoming for other versions of Windows.</span></span> <span data-ttu-id="d2d29-169">Para obter mais informações, consulte <xref:samesite/kbs-samesite>.</span><span class="sxs-lookup"><span data-stu-id="d2d29-169">For more information, see <xref:samesite/kbs-samesite>.</span></span>

 <span data-ttu-id="d2d29-170">O rascunho 2019 da especificação de SameSite:</span><span class="sxs-lookup"><span data-stu-id="d2d29-170">The 2019 draft of the SameSite specification:</span></span>

* <span data-ttu-id="d2d29-171">**Não** é compatível com versões anteriores com o rascunho 2016.</span><span class="sxs-lookup"><span data-stu-id="d2d29-171">Is **not** backwards compatible with the 2016 draft.</span></span> <span data-ttu-id="d2d29-172">Para obter mais informações, consulte [dando suporte a navegadores mais antigos](#sob) neste documento.</span><span class="sxs-lookup"><span data-stu-id="d2d29-172">For more information, see [Supporting older browsers](#sob) in this document.</span></span>
* <span data-ttu-id="d2d29-173">Especifica que os cookies são tratados como `SameSite=Lax` por padrão.</span><span class="sxs-lookup"><span data-stu-id="d2d29-173">Specifies cookies are treated as `SameSite=Lax` by default.</span></span>
* <span data-ttu-id="d2d29-174">Especifica os cookies que o Assert explicitamente `SameSite=None` para habilitar a entrega entre sites também devem ser marcados como `Secure` .</span><span class="sxs-lookup"><span data-stu-id="d2d29-174">Specifies cookies that explicitly assert `SameSite=None` in order to enable cross-site delivery should also be marked as `Secure`.</span></span>
* <span data-ttu-id="d2d29-175">O é suportado por patches emitidos conforme descrito em KB listados acima.</span><span class="sxs-lookup"><span data-stu-id="d2d29-175">Is supported by patches issued as described in the KB's listed above.</span></span>
* <span data-ttu-id="d2d29-176">Está agendado para ser habilitado pelo [Chrome](https://chromestatus.com/feature/5088147346030592) por padrão em [fevereiro de 2020](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html).</span><span class="sxs-lookup"><span data-stu-id="d2d29-176">Is scheduled to be enabled by [Chrome](https://chromestatus.com/feature/5088147346030592) by default in [Feb 2020](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html).</span></span> <span data-ttu-id="d2d29-177">Os navegadores começaram a passar para esse padrão em 2019.</span><span class="sxs-lookup"><span data-stu-id="d2d29-177">Browsers started moving to this standard in 2019.</span></span>

<a name="known"></a>

## <a name="known-issues"></a><span data-ttu-id="d2d29-178">Problemas Conhecidos</span><span class="sxs-lookup"><span data-stu-id="d2d29-178">Known Issues</span></span>

<span data-ttu-id="d2d29-179">Como as especificações de rascunho 2016 e 2019 não são compatíveis, a atualização de novembro de 2019 do .NET Framework introduz algumas alterações que podem estar sendo interrompidas.</span><span class="sxs-lookup"><span data-stu-id="d2d29-179">Because the 2016 and 2019 draft specifications are not compatible, the November 2019 .Net Framework update introduces some changes that may be breaking.</span></span>

* <span data-ttu-id="d2d29-180">Os cookies de estado de sessão e de autenticação de formulários agora são gravados na rede como `Lax` em vez de não especificados.</span><span class="sxs-lookup"><span data-stu-id="d2d29-180">Session State and Forms Authentication cookies are now written to the network as `Lax` instead of unspecified.</span></span>
  * <span data-ttu-id="d2d29-181">Enquanto a maioria dos aplicativos trabalha com `SameSite=Lax` cookies, os aplicativos que são postados em sites ou aplicativos que usam `iframe` o podem descobrir que seus Estados de sessão ou cookies de autorização de formulários não estão sendo usados conforme o esperado.</span><span class="sxs-lookup"><span data-stu-id="d2d29-181">While most apps work with `SameSite=Lax` cookies, apps that POST across sites or applications that make use of `iframe` may find that their session state or forms authorization cookies aren't being used as expected.</span></span> <span data-ttu-id="d2d29-182">Para corrigir isso, altere o `cookieSameSite` valor na seção de configuração apropriada, conforme discutido anteriormente.</span><span class="sxs-lookup"><span data-stu-id="d2d29-182">To remedy this, change the `cookieSameSite` value in the appropriate configuration section as discussed previously.</span></span>
* <span data-ttu-id="d2d29-183">As HttpCookies que definem explicitamente `SameSite=None` no código ou na configuração agora têm esse valor gravado com o cookie, enquanto ele foi omitido anteriormente.</span><span class="sxs-lookup"><span data-stu-id="d2d29-183">HttpCookies that explicitly set `SameSite=None` in code or configuration now have that value written with the cookie, whereas it was previously omitted.</span></span> <span data-ttu-id="d2d29-184">Isso pode causar problemas com navegadores mais antigos que dão suporte apenas ao padrão de rascunho 2016.</span><span class="sxs-lookup"><span data-stu-id="d2d29-184">This may cause issues with older browsers that only support the 2016 draft standard.</span></span>
  * <span data-ttu-id="d2d29-185">Ao direcionar os navegadores que dão suporte ao padrão de rascunho 2019 com `SameSite=None` cookies, lembre-se também de marcá-los `Secure` ou eles podem não ser reconhecidos.</span><span class="sxs-lookup"><span data-stu-id="d2d29-185">When targeting browsers supporting the 2019 draft standard with `SameSite=None` cookies, remember to also mark them `Secure` or they may not be recognized.</span></span>
  * <span data-ttu-id="d2d29-186">Para reverter para o comportamento de 2016 de não gravar `SameSite=None` , use a configuração de aplicativo `aspnet:SupressSameSiteNone=true` .</span><span class="sxs-lookup"><span data-stu-id="d2d29-186">To revert to the 2016 behavior of not writing `SameSite=None`, use the app setting `aspnet:SupressSameSiteNone=true`.</span></span> <span data-ttu-id="d2d29-187">Observe que isso será aplicado a todos os HttpCookies no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="d2d29-187">Note that this will apply to all HttpCookies in the app.</span></span>

### <a name="azure-app-servicesamesite-cookie-handling"></a><span data-ttu-id="d2d29-188">Serviço de Azure App — tratamento de cookies SameSite</span><span class="sxs-lookup"><span data-stu-id="d2d29-188">Azure App Service—SameSite cookie handling</span></span>

<span data-ttu-id="d2d29-189">Consulte [serviço de Azure app – tratamento de cookies SameSite e .NET Framework patch do 4.7.2](https://azure.microsoft.com/updates/app-service-samesite-cookie-update/) para obter informações sobre como Azure app serviço está configurando comportamentos de SameSite em aplicativos .NET 4.7.2.</span><span class="sxs-lookup"><span data-stu-id="d2d29-189">See [Azure App Service—SameSite cookie handling and .NET Framework 4.7.2 patch](https://azure.microsoft.com/updates/app-service-samesite-cookie-update/) for information about how Azure App Service is configuring SameSite behaviors in .Net 4.7.2 apps.</span></span>

<a name="sob"></a>

## <a name="supporting-older-browsers"></a><span data-ttu-id="d2d29-190">Suporte a navegadores mais antigos</span><span class="sxs-lookup"><span data-stu-id="d2d29-190">Supporting older browsers</span></span>

<span data-ttu-id="d2d29-191">O padrão de 2016 SameSite exigiu que valores desconhecidos devam ser tratados como `SameSite=Strict` valores.</span><span class="sxs-lookup"><span data-stu-id="d2d29-191">The 2016 SameSite standard mandated that unknown values must be treated as `SameSite=Strict` values.</span></span> <span data-ttu-id="d2d29-192">Os aplicativos acessados de navegadores mais antigos que dão suporte ao padrão de 2016 SameSite podem falhar quando obtêm uma propriedade SameSite com um valor de `None` .</span><span class="sxs-lookup"><span data-stu-id="d2d29-192">Apps accessed from older browsers which support the 2016 SameSite standard may break when they get a SameSite property with a value of `None`.</span></span> <span data-ttu-id="d2d29-193">Os aplicativos Web devem implementar a detecção do navegador se pretenderem oferecer suporte a navegadores mais antigos.</span><span class="sxs-lookup"><span data-stu-id="d2d29-193">Web apps must implement browser detection if they intend to support older browsers.</span></span> <span data-ttu-id="d2d29-194">O ASP.NET não implementa a detecção de navegador porque os valores de User-Agents são altamente voláteis e mudam com frequência.</span><span class="sxs-lookup"><span data-stu-id="d2d29-194">ASP.NET doesn't implement browser detection because User-Agents values are highly volatile and change frequently.</span></span>

<span data-ttu-id="d2d29-195">A abordagem da Microsoft para corrigir o problema é ajudá-lo a implementar componentes de detecção de navegador para remover o `sameSite=None` atributo de cookies se um navegador for conhecido por não oferecer suporte a ele.</span><span class="sxs-lookup"><span data-stu-id="d2d29-195">Microsoft's approach to fixing the problem is to help you implement browser detection components to strip the `sameSite=None` attribute from cookies if a browser is known to not support it.</span></span> <span data-ttu-id="d2d29-196">O Conselho do Google foi emitir cookies duplos, um com o novo atributo e um sem o atributo.</span><span class="sxs-lookup"><span data-stu-id="d2d29-196">Google's advice was to issue double cookies, one with the new attribute, and one without the attribute at all.</span></span> <span data-ttu-id="d2d29-197">No entanto, consideramos o aviso do Google limitado.</span><span class="sxs-lookup"><span data-stu-id="d2d29-197">However we consider Google's advice limited.</span></span> <span data-ttu-id="d2d29-198">Alguns navegadores, especialmente os navegadores móveis têm limites muito pequenos no número de cookies de um site ou um nome de domínio pode enviar.</span><span class="sxs-lookup"><span data-stu-id="d2d29-198">Some browsers, especially mobile browsers have very small limits on the number of cookies a site, or a domain name can send.</span></span> <span data-ttu-id="d2d29-199">O envio de vários cookies, especialmente cookies grandes, como cookies de autenticação, pode alcançar o limite do navegador móvel muito rapidamente, causando falhas de aplicativo difíceis de diagnosticar e corrigir.</span><span class="sxs-lookup"><span data-stu-id="d2d29-199">Sending multiple cookies, especially large cookies like authentication cookies can reach the mobile browser limit very quickly, causing app failures that are hard to diagnose and fix.</span></span> <span data-ttu-id="d2d29-200">Além de uma estrutura, há um grande ecossistema de código e componentes de terceiros que podem não ser atualizados para usar uma abordagem de cookie duplo.</span><span class="sxs-lookup"><span data-stu-id="d2d29-200">Furthermore as a framework there is a large ecosystem of third party code and components that may not be updated to use a double cookie approach.</span></span>

<span data-ttu-id="d2d29-201">O código de detecção do navegador usado nos projetos de exemplo neste [repositório GitHub]() está contido em dois arquivos</span><span class="sxs-lookup"><span data-stu-id="d2d29-201">The browser detection code used in the sample projects in [this GitHub repository]() is contained in two files</span></span>

* [<span data-ttu-id="d2d29-202">SameSiteSupport.cs C#</span><span class="sxs-lookup"><span data-stu-id="d2d29-202">C# SameSiteSupport.cs</span></span>](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/SameSiteSupport.cs)
* [<span data-ttu-id="d2d29-203">VB SameSiteSupport. vb</span><span class="sxs-lookup"><span data-stu-id="d2d29-203">VB SameSiteSupport.vb</span></span>](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/SameSiteSupport.vb)

<span data-ttu-id="d2d29-204">Essas detecções são os agentes de navegador mais comuns que vimos que dão suporte ao padrão 2016 e para os quais o atributo precisa ser completamente removido.</span><span class="sxs-lookup"><span data-stu-id="d2d29-204">These detections are the most common browser agents we have seen that support the 2016 standard and for which the attribute needs to be completely removed.</span></span> <span data-ttu-id="d2d29-205">Isso não significa uma implementação completa:</span><span class="sxs-lookup"><span data-stu-id="d2d29-205">It isn't meant as a complete implementation:</span></span>

* <span data-ttu-id="d2d29-206">Seu aplicativo pode ver os navegadores que nossos sites de teste não têm.</span><span class="sxs-lookup"><span data-stu-id="d2d29-206">Your app may see browsers that our test sites do not.</span></span>
* <span data-ttu-id="d2d29-207">Você deve estar preparado para adicionar detecções conforme necessário para seu ambiente.</span><span class="sxs-lookup"><span data-stu-id="d2d29-207">You should be prepared to add detections as necessary for your environment.</span></span>

<span data-ttu-id="d2d29-208">A forma de conexão da detecção varia de acordo com a versão do .NET e a estrutura da Web que você está usando.</span><span class="sxs-lookup"><span data-stu-id="d2d29-208">How you wire up the detection varies according the version of .NET and the web framework that you are using.</span></span> <span data-ttu-id="d2d29-209">O código a seguir pode ser chamado no site de chamada do [HttpCookie](/dotnet/api/system.web.httpcookie) :</span><span class="sxs-lookup"><span data-stu-id="d2d29-209">The following code can be called at the [HttpCookie](/dotnet/api/system.web.httpcookie) call site:</span></span>

[!code-csharp[](sample/SameSiteCheck.cs?name=snippet)]

<span data-ttu-id="d2d29-210">Consulte os seguintes tópicos de cookies do ASP.NET 4.7.2 SameSite:</span><span class="sxs-lookup"><span data-stu-id="d2d29-210">See the following ASP.NET 4.7.2 SameSite cookie topics:</span></span>

* [<span data-ttu-id="d2d29-211">C# MVC</span><span class="sxs-lookup"><span data-stu-id="d2d29-211">C# MVC</span></span>](xref:samesite/csMVC)
* [<span data-ttu-id="d2d29-212">C# WebForms</span><span class="sxs-lookup"><span data-stu-id="d2d29-212">C# WebForms</span></span>](xref:samesite/CSharpWebForms)
* [<span data-ttu-id="d2d29-213">VB WebForms</span><span class="sxs-lookup"><span data-stu-id="d2d29-213">VB WebForms</span></span>](xref:samesite/vbWF)
* [<span data-ttu-id="d2d29-214">VB MVC</span><span class="sxs-lookup"><span data-stu-id="d2d29-214">VB MVC</span></span>](xref:samesite/vbMVC)
<!--
* <xref:samesite/csMVC>
* <xref:samesite/CSharpWebForms>
* <xref:samesite/vbWF>
* <xref:samesite/vbMVC>
-->

### <a name="ensuring-your-site-redirects-to-https"></a><span data-ttu-id="d2d29-215">Garantindo que seu site Redirecione para HTTPS</span><span class="sxs-lookup"><span data-stu-id="d2d29-215">Ensuring your site redirects to HTTPS</span></span>

<span data-ttu-id="d2d29-216">Para ASP.NET 4. x, WebForms e MVC, o recurso [de reescrita de URL do IIS](/iis/extensions/url-rewrite-module/creating-rewrite-rules-for-the-url-rewrite-module) pode ser usado para redirecionar todas as solicitações para https.</span><span class="sxs-lookup"><span data-stu-id="d2d29-216">For ASP.NET 4.x, WebForms and MVC, [IIS's URL Rewrite](/iis/extensions/url-rewrite-module/creating-rewrite-rules-for-the-url-rewrite-module) feature can be used to redirect all requests to HTTPS.</span></span> <span data-ttu-id="d2d29-217">O XML a seguir mostra uma regra de exemplo:</span><span class="sxs-lookup"><span data-stu-id="d2d29-217">The following XML shows a sample rule:</span></span>

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

<span data-ttu-id="d2d29-218">Em instalações locais da [regravação de URL do IIS](https://www.iis.net/downloads/microsoft/url-rewrite) , há um recurso opcional que pode precisar de instalação.</span><span class="sxs-lookup"><span data-stu-id="d2d29-218">In on-premises installations of [IIS URL Rewrite](https://www.iis.net/downloads/microsoft/url-rewrite) is an optional feature that may need installing.</span></span>

## <a name="test-apps-for-samesite-problems"></a><span data-ttu-id="d2d29-219">Testar aplicativos para problemas de SameSite</span><span class="sxs-lookup"><span data-stu-id="d2d29-219">Test apps for SameSite problems</span></span>

<span data-ttu-id="d2d29-220">Você deve testar seu aplicativo com os navegadores aos quais dá suporte e percorrer os cenários que envolvem cookies.</span><span class="sxs-lookup"><span data-stu-id="d2d29-220">You must test your app with the browsers you support and go through your scenarios that involve cookies.</span></span> <span data-ttu-id="d2d29-221">Cenários de cookie normalmente envolvem</span><span class="sxs-lookup"><span data-stu-id="d2d29-221">Cookie scenarios typically involve</span></span>

* <span data-ttu-id="d2d29-222">Formulários de logon</span><span class="sxs-lookup"><span data-stu-id="d2d29-222">Login forms</span></span>
* <span data-ttu-id="d2d29-223">Mecanismos de logon externos, como Facebook, Azure AD, OAuth e OIDC</span><span class="sxs-lookup"><span data-stu-id="d2d29-223">External login mechanisms such as Facebook, Azure AD, OAuth and OIDC</span></span>
* <span data-ttu-id="d2d29-224">Páginas que aceitam solicitações de outros sites</span><span class="sxs-lookup"><span data-stu-id="d2d29-224">Pages that accept requests from other sites</span></span>
* <span data-ttu-id="d2d29-225">Páginas em seu aplicativo projetadas para serem inseridas em IFrames</span><span class="sxs-lookup"><span data-stu-id="d2d29-225">Pages in your app designed to be embedded in iframes</span></span>

<span data-ttu-id="d2d29-226">Você deve verificar se os cookies são criados, persistidos e excluídos corretamente em seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="d2d29-226">You should check that cookies are created, persisted and deleted correctly in your app.</span></span>

<span data-ttu-id="d2d29-227">Aplicativos que interagem com sites remotos, como por meio de logon de terceiros, precisam:</span><span class="sxs-lookup"><span data-stu-id="d2d29-227">Apps that interact with remote sites such as through third-party login need to:</span></span>

* <span data-ttu-id="d2d29-228">Teste a interação em vários navegadores.</span><span class="sxs-lookup"><span data-stu-id="d2d29-228">Test the interaction on multiple browsers.</span></span>
* <span data-ttu-id="d2d29-229">Aplique a [detecção e mitigação do navegador](#sob) discutidas neste documento.</span><span class="sxs-lookup"><span data-stu-id="d2d29-229">Apply the [browser detection and mitigation](#sob) discussed in this document.</span></span>

<span data-ttu-id="d2d29-230">Teste os aplicativos Web usando uma versão do cliente que pode aceitar o novo comportamento de SameSite.</span><span class="sxs-lookup"><span data-stu-id="d2d29-230">Test web apps using a client version that can opt-in to the new SameSite behavior.</span></span> <span data-ttu-id="d2d29-231">O Chrome, o Firefox e o Chromium Edge têm novos sinalizadores de recurso de aceitação que podem ser usados para teste.</span><span class="sxs-lookup"><span data-stu-id="d2d29-231">Chrome, Firefox, and Chromium Edge all have new opt-in feature flags that can be used for testing.</span></span> <span data-ttu-id="d2d29-232">Depois que seu aplicativo aplicar os patches do SameSite, teste-o com versões mais antigas do cliente, especialmente o Safari.</span><span class="sxs-lookup"><span data-stu-id="d2d29-232">After your app applies the SameSite patches, test it with older client versions, especially Safari.</span></span> <span data-ttu-id="d2d29-233">Para obter mais informações, consulte [dando suporte a navegadores mais antigos](#sob) neste documento.</span><span class="sxs-lookup"><span data-stu-id="d2d29-233">For more information, see [Supporting older browsers](#sob) in this document.</span></span>

### <a name="test-with-chrome"></a><span data-ttu-id="d2d29-234">Teste com o Chrome</span><span class="sxs-lookup"><span data-stu-id="d2d29-234">Test with Chrome</span></span>

<span data-ttu-id="d2d29-235">O Chrome 78 + fornece resultados enganosos porque tem uma mitigação temporária em vigor.</span><span class="sxs-lookup"><span data-stu-id="d2d29-235">Chrome 78+ gives misleading results because it has a temporary mitigation in place.</span></span> <span data-ttu-id="d2d29-236">A mitigação do Chrome 78 + temporária permite cookies com menos de dois minutos.</span><span class="sxs-lookup"><span data-stu-id="d2d29-236">The Chrome 78+ temporary mitigation allows cookies less than two minutes old.</span></span> <span data-ttu-id="d2d29-237">O Chrome 76 ou 77 com os sinalizadores de teste apropriados habilitados fornece resultados mais precisos.</span><span class="sxs-lookup"><span data-stu-id="d2d29-237">Chrome 76 or 77 with the appropriate test flags enabled provides more accurate results.</span></span> <span data-ttu-id="d2d29-238">Para testar o novo comportamento de SameSite, alterne `chrome://flags/#same-site-by-default-cookies` para **habilitado**.</span><span class="sxs-lookup"><span data-stu-id="d2d29-238">To test the new SameSite behavior toggle `chrome://flags/#same-site-by-default-cookies` to **Enabled**.</span></span> <span data-ttu-id="d2d29-239">Versões mais antigas do Chrome (75 e inferior) são relatadas para falha com a nova `None` configuração.</span><span class="sxs-lookup"><span data-stu-id="d2d29-239">Older versions of Chrome (75 and below) are reported to fail with the new `None` setting.</span></span> <span data-ttu-id="d2d29-240">Consulte [suporte a navegadores mais antigos](#sob) neste documento.</span><span class="sxs-lookup"><span data-stu-id="d2d29-240">See [Supporting older browsers](#sob) in this document.</span></span>

<span data-ttu-id="d2d29-241">O Google não disponibiliza versões mais antigas do Chrome.</span><span class="sxs-lookup"><span data-stu-id="d2d29-241">Google does not make older chrome versions available.</span></span> <span data-ttu-id="d2d29-242">Siga as instruções em [baixar o Chromium](https://www.chromium.org/getting-involved/download-chromium) para testar versões anteriores do Chrome.</span><span class="sxs-lookup"><span data-stu-id="d2d29-242">Follow the instructions at [Download Chromium](https://www.chromium.org/getting-involved/download-chromium) to test older versions of Chrome.</span></span> <span data-ttu-id="d2d29-243">**Não** Baixe o Chrome de links fornecidos pela pesquisa de versões mais antigas do Chrome.</span><span class="sxs-lookup"><span data-stu-id="d2d29-243">Do **not** download Chrome from links provided by searching for older versions of chrome.</span></span>

* [<span data-ttu-id="d2d29-244">Chromium 76 Win64</span><span class="sxs-lookup"><span data-stu-id="d2d29-244">Chromium 76 Win64</span></span>](https://commondatastorage.googleapis.com/chromium-browser-snapshots/index.html?prefix=Win_x64/664998/)
* [<span data-ttu-id="d2d29-245">Chromium 74 Win64</span><span class="sxs-lookup"><span data-stu-id="d2d29-245">Chromium 74 Win64</span></span>](https://commondatastorage.googleapis.com/chromium-browser-snapshots/index.html?prefix=Win_x64/638880/)
* <span data-ttu-id="d2d29-246">Se você não estiver usando uma versão de 64 bits do Windows, poderá usar o [Visualizador do OmahaProxy](https://omahaproxy.appspot.com/) para procurar qual ramificação do Chromium corresponde ao Chrome 74 (v 74.0.3729.108) usando as [instruções fornecidas pelo Chromium](https://www.chromium.org/getting-involved/download-chromium).</span><span class="sxs-lookup"><span data-stu-id="d2d29-246">If you're not using a 64bit version of Windows you can use the [OmahaProxy viewer](https://omahaproxy.appspot.com/) to look up which Chromium branch corresponds to Chrome 74 (v74.0.3729.108) using the [instructions provided by Chromium](https://www.chromium.org/getting-involved/download-chromium).</span></span>

<span data-ttu-id="d2d29-247">A partir da versão canário `80.0.3975.0` , a mitigação temporária pode ser desativada para fins de teste usando o novo sinalizador `--enable-features=SameSiteDefaultChecksMethodRigorously` para permitir o teste de sites e serviços no estado final eventual do recurso em que a mitigação foi removida.</span><span class="sxs-lookup"><span data-stu-id="d2d29-247">Starting in Canary version `80.0.3975.0`, the Lax+POST temporary mitigation can be disabled for testing purposes using the new flag `--enable-features=SameSiteDefaultChecksMethodRigorously` to allow testing of sites and services in the eventual end state of the feature where the mitigation has been removed.</span></span> <span data-ttu-id="d2d29-248">Para obter mais informações, consulte o Chromium Projects [SameSite updates](https://www.chromium.org/updates/same-site)</span><span class="sxs-lookup"><span data-stu-id="d2d29-248">For more information, see The Chromium Projects [SameSite Updates](https://www.chromium.org/updates/same-site)</span></span>

#### <a name="test-with-chrome-80"></a><span data-ttu-id="d2d29-249">Teste com o Chrome 80 +</span><span class="sxs-lookup"><span data-stu-id="d2d29-249">Test with Chrome 80+</span></span>

<span data-ttu-id="d2d29-250">[Baixe](https://www.google.com/chrome/) uma versão do Chrome que dê suporte a seu novo atributo.</span><span class="sxs-lookup"><span data-stu-id="d2d29-250">[Download](https://www.google.com/chrome/) a version of Chrome that supports their new attribute.</span></span> <span data-ttu-id="d2d29-251">No momento da gravação, a versão atual é o Chrome 80.</span><span class="sxs-lookup"><span data-stu-id="d2d29-251">At the time of writing, the current version is Chrome 80.</span></span> <span data-ttu-id="d2d29-252">O Chrome 80 precisa do sinalizador `chrome://flags/#same-site-by-default-cookies` habilitado para usar o novo comportamento.</span><span class="sxs-lookup"><span data-stu-id="d2d29-252">Chrome 80 needs the flag `chrome://flags/#same-site-by-default-cookies` enabled to use the new behavior.</span></span> <span data-ttu-id="d2d29-253">Você também deve habilitar ( `chrome://flags/#cookies-without-same-site-must-be-secure` ) para testar o comportamento futuro de cookies que não têm o atributo sameSite habilitado.</span><span class="sxs-lookup"><span data-stu-id="d2d29-253">You should also enable (`chrome://flags/#cookies-without-same-site-must-be-secure`) to test the upcoming behavior for cookies which have no sameSite attribute enabled.</span></span> <span data-ttu-id="d2d29-254">O Chrome 80 está no destino para fazer a alternância tratar cookies sem o atributo como `SameSite=Lax` , embora com um período de carência cronometrado para determinadas solicitações.</span><span class="sxs-lookup"><span data-stu-id="d2d29-254">Chrome 80 is on target to make the switch to treat cookies without the attribute as `SameSite=Lax`, albeit with a timed grace period for certain requests.</span></span> <span data-ttu-id="d2d29-255">Para desabilitar o período de carência cronometrado, o Chrome 80 pode ser iniciado com o seguinte argumento de linha de comando:</span><span class="sxs-lookup"><span data-stu-id="d2d29-255">To disable the timed grace period Chrome 80 can be launched with the following command line argument:</span></span>

`--enable-features=SameSiteDefaultChecksMethodRigorously`

<span data-ttu-id="d2d29-256">O Chrome 80 tem mensagens de aviso no console do navegador sobre atributos sameSite ausentes.</span><span class="sxs-lookup"><span data-stu-id="d2d29-256">Chrome 80 has warning messages in the browser console about missing sameSite attributes.</span></span> <span data-ttu-id="d2d29-257">Use F12 para abrir o console do navegador.</span><span class="sxs-lookup"><span data-stu-id="d2d29-257">Use F12 to open the browser console.</span></span>

### <a name="test-with-safari"></a><span data-ttu-id="d2d29-258">Teste com o Safari</span><span class="sxs-lookup"><span data-stu-id="d2d29-258">Test with Safari</span></span>

<span data-ttu-id="d2d29-259">O Safari 12 implementou estritamente o rascunho anterior e falha quando o novo `None` valor está em um cookie.</span><span class="sxs-lookup"><span data-stu-id="d2d29-259">Safari 12 strictly implemented the prior draft and fails when the new `None` value is in a cookie.</span></span> <span data-ttu-id="d2d29-260">`None` é evitado por meio do código de detecção de navegador que [dá suporte a navegadores mais antigos](#sob) neste documento.</span><span class="sxs-lookup"><span data-stu-id="d2d29-260">`None` is avoided via the browser detection code [Supporting older browsers](#sob) in this document.</span></span> <span data-ttu-id="d2d29-261">Teste OS logons de estilo do sistema operacional baseado no Safari 12, Safari 13 e WebKit usando MSAL, ADAL ou qualquer biblioteca que você esteja usando.</span><span class="sxs-lookup"><span data-stu-id="d2d29-261">Test Safari 12, Safari 13, and WebKit based OS style logins using MSAL, ADAL or whatever library you are using.</span></span> <span data-ttu-id="d2d29-262">O problema depende da versão subjacente do sistema operacional.</span><span class="sxs-lookup"><span data-stu-id="d2d29-262">The problem is dependent on the underlying OS version.</span></span> <span data-ttu-id="d2d29-263">OSX Mojave (10,14) e iOS 12 são conhecidos por ter problemas de compatibilidade com o novo comportamento de SameSite.</span><span class="sxs-lookup"><span data-stu-id="d2d29-263">OSX Mojave (10.14) and iOS 12 are known to have compatibility problems with the new SameSite behavior.</span></span> <span data-ttu-id="d2d29-264">Atualizar o sistema operacional para OSX Catalina (10,15) ou iOS 13 corrige o problema.</span><span class="sxs-lookup"><span data-stu-id="d2d29-264">Upgrading the OS to OSX Catalina (10.15) or iOS 13 fixes the problem.</span></span> <span data-ttu-id="d2d29-265">No momento, o Safari não tem um sinalizador de aceitação para testar o novo comportamento de especificação.</span><span class="sxs-lookup"><span data-stu-id="d2d29-265">Safari does not currently have an opt-in flag for testing the new spec behavior.</span></span>

### <a name="test-with-firefox"></a><span data-ttu-id="d2d29-266">Testar com o Firefox</span><span class="sxs-lookup"><span data-stu-id="d2d29-266">Test with Firefox</span></span>

<span data-ttu-id="d2d29-267">O suporte do Firefox para o novo padrão pode ser testado na versão 68 +, optando na `about:config` página pelo sinalizador de recurso `network.cookie.sameSite.laxByDefault` .</span><span class="sxs-lookup"><span data-stu-id="d2d29-267">Firefox support for the new standard can be tested on version 68+ by opting in on the `about:config` page with the feature flag `network.cookie.sameSite.laxByDefault`.</span></span> <span data-ttu-id="d2d29-268">Não há relatórios de problemas de compatibilidade com versões mais antigas do Firefox.</span><span class="sxs-lookup"><span data-stu-id="d2d29-268">There haven't been reports of compatibility issues with older versions of Firefox.</span></span>

### <a name="test-with-edge-legacy-browser"></a><span data-ttu-id="d2d29-269">Testar com o navegador de borda (Herdado)</span><span class="sxs-lookup"><span data-stu-id="d2d29-269">Test with Edge (Legacy) browser</span></span>

<span data-ttu-id="d2d29-270">O Edge dá suporte ao antigo SameSite padrão.</span><span class="sxs-lookup"><span data-stu-id="d2d29-270">Edge supports the old SameSite standard.</span></span> <span data-ttu-id="d2d29-271">A versão do Edge 44 + não tem nenhum problema de compatibilidade conhecido com o novo padrão.</span><span class="sxs-lookup"><span data-stu-id="d2d29-271">Edge version 44+ doesn't have any known compatibility problems with the new standard.</span></span>

### <a name="test-with-edge-chromium"></a><span data-ttu-id="d2d29-272">Testar com borda (Chromium)</span><span class="sxs-lookup"><span data-stu-id="d2d29-272">Test with Edge (Chromium)</span></span>

<span data-ttu-id="d2d29-273">Os sinalizadores SameSite são definidos na `edge://flags/#same-site-by-default-cookies` página.</span><span class="sxs-lookup"><span data-stu-id="d2d29-273">SameSite flags are set on the `edge://flags/#same-site-by-default-cookies` page.</span></span> <span data-ttu-id="d2d29-274">Nenhum problema de compatibilidade foi descoberto com o Edge Chromium.</span><span class="sxs-lookup"><span data-stu-id="d2d29-274">No compatibility issues were discovered with Edge Chromium.</span></span>

### <a name="test-with-electron"></a><span data-ttu-id="d2d29-275">Testar com o</span><span class="sxs-lookup"><span data-stu-id="d2d29-275">Test with Electron</span></span>

<span data-ttu-id="d2d29-276">As versões do at-SS são versões mais antigas do Chromium.</span><span class="sxs-lookup"><span data-stu-id="d2d29-276">Versions of Electron include older versions of Chromium.</span></span> <span data-ttu-id="d2d29-277">Por exemplo, a versão do at-SS usada pelas equipes é Chromium 66, que exibe o comportamento mais antigo.</span><span class="sxs-lookup"><span data-stu-id="d2d29-277">For example, the version of Electron used by Teams is Chromium 66, which exhibits the older behavior.</span></span> <span data-ttu-id="d2d29-278">Você deve executar seu próprio teste de compatibilidade com a versão do uso de digital que seu produto usa.</span><span class="sxs-lookup"><span data-stu-id="d2d29-278">You must perform your own compatibility testing with the version of Electron your product uses.</span></span> <span data-ttu-id="d2d29-279">Consulte [suporte a navegadores mais antigos](#sob).</span><span class="sxs-lookup"><span data-stu-id="d2d29-279">See [Supporting older browsers](#sob).</span></span>

## <a name="reverting-samesite-patches"></a><span data-ttu-id="d2d29-280">Revertendo patches do SameSite</span><span class="sxs-lookup"><span data-stu-id="d2d29-280">Reverting SameSite patches</span></span>

<span data-ttu-id="d2d29-281">Você pode reverter o comportamento sameSite atualizado em aplicativos .NET Framework para o comportamento anterior em que o atributo sameSite não é emitido para um valor de e `None` reverter a autenticação e os cookies de sessão para não emitir o valor.</span><span class="sxs-lookup"><span data-stu-id="d2d29-281">You can revert the updated sameSite behavior in .NET Framework apps to its previous behavior where the sameSite attribute is not emitted for a value of `None`, and revert the authentication and session cookies to not emit the value.</span></span> <span data-ttu-id="d2d29-282">Isso deve ser exibido como uma *correção extremamente temporária*, pois as alterações do Chrome interromperão quaisquer solicitações post externas ou autenticação para usuários que usam navegadores que dão suporte às alterações no padrão.</span><span class="sxs-lookup"><span data-stu-id="d2d29-282">This should be viewed as an *extremely temporary fix*, as the Chrome changes will break any external POST requests or authentication for users using browsers which support the changes to the standard.</span></span>

### <a name="reverting-net-472-behavior"></a><span data-ttu-id="d2d29-283">Revertendo o comportamento do .NET 4.7.2</span><span class="sxs-lookup"><span data-stu-id="d2d29-283">Reverting .NET 4.7.2 behavior</span></span>

<span data-ttu-id="d2d29-284">Atualize *web.config* para incluir as seguintes definições de configuração:</span><span class="sxs-lookup"><span data-stu-id="d2d29-284">Update *web.config* to include the following configuration settings:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="d2d29-285">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="d2d29-285">Additional resources</span></span>

* [<span data-ttu-id="d2d29-286">Próximas alterações de cookie SameSite em ASP.NET e ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d2d29-286">Upcoming SameSite Cookie Changes in ASP.NET and ASP.NET Core</span></span>](https://devblogs.microsoft.com/aspnet/upcoming-samesite-cookie-changes-in-asp-net-and-asp-net-core/)
* [<span data-ttu-id="d2d29-287">Dicas para testar e depurar SameSite-por-padrão e "SameSite = None; Proteger "cookies</span><span class="sxs-lookup"><span data-stu-id="d2d29-287">Tips for testing and debugging SameSite-by-default and “SameSite=None; Secure” cookies</span></span>](https://www.chromium.org/updates/same-site/test-debug)
* [<span data-ttu-id="d2d29-288">Blog do Chromium: desenvolvedores: Prepare-se para o New SameSite = None; Configurações de cookie seguro</span><span class="sxs-lookup"><span data-stu-id="d2d29-288">Chromium Blog:Developers: Get Ready for New SameSite=None; Secure Cookie Settings</span></span>](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html)
* [<span data-ttu-id="d2d29-289">SameSite cookies explicados</span><span class="sxs-lookup"><span data-stu-id="d2d29-289">SameSite cookies explained</span></span>](https://web.dev/samesite-cookies-explained/)
* [<span data-ttu-id="d2d29-290">Atualizações do Chrome</span><span class="sxs-lookup"><span data-stu-id="d2d29-290">Chrome Updates</span></span>](https://www.chromium.org/updates/same-site)
* [<span data-ttu-id="d2d29-291">Patches do .NET SameSite</span><span class="sxs-lookup"><span data-stu-id="d2d29-291">.NET SameSite Patches</span></span>](/aspnet/samesite/kbs-samesite)
* [<span data-ttu-id="d2d29-292">Informações do mesmo site de aplicativos Web do Azure</span><span class="sxs-lookup"><span data-stu-id="d2d29-292">Azure Web Applications Same Site Information</span></span>](https://azure.microsoft.com/updates/app-service-samesite-cookie-update/)
* [<span data-ttu-id="d2d29-293">Informações do mesmo site do Azure ActiveDirectory</span><span class="sxs-lookup"><span data-stu-id="d2d29-293">Azure ActiveDirectory Same Site Information</span></span>](/azure/active-directory/develop/howto-handle-samesite-cookie-changes-chrome-browser)
