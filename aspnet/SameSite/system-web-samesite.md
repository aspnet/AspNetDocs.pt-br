---
title: Trabalhar com cookies SameSite no ASP.NET
author: rick-anderson
description: Saiba como usar o para SameSite cookies no ASP.NET
ms.author: riande
ms.date: 12/03/2019
uid: samesite/system-web-samesite
ms.openlocfilehash: 40e5c13b6834912c13b41cbfad7da8cd84ca6c8b
ms.sourcegitcommit: 969e7db924ebad3cc0f0cb0d65d148e8b9221b9a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/06/2019
ms.locfileid: "74902020"
---
# <a name="work-with-samesite-cookies-in-aspnet"></a><span data-ttu-id="cd16f-103">Trabalhar com cookies SameSite no ASP.NET</span><span class="sxs-lookup"><span data-stu-id="cd16f-103">Work with SameSite cookies in ASP.NET</span></span>

<span data-ttu-id="cd16f-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="cd16f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="cd16f-105">SameSite é um rascunho de [IETF](https://ietf.org/about/) projetado para fornecer uma proteção contra ataques de CSRF (solicitação entre sites forjados).</span><span class="sxs-lookup"><span data-stu-id="cd16f-105">SameSite is an [IETF](https://ietf.org/about/) draft designed to provide some protection against cross-site request forgery (CSRF) attacks.</span></span> <span data-ttu-id="cd16f-106">O [rascunho do SameSite 2019](https://tools.ietf.org/html/draft-west-cookie-incrementalism-00):</span><span class="sxs-lookup"><span data-stu-id="cd16f-106">The [SameSite 2019 draft](https://tools.ietf.org/html/draft-west-cookie-incrementalism-00):</span></span>

* <span data-ttu-id="cd16f-107">Trata cookies como `SameSite=Lax` por padrão.</span><span class="sxs-lookup"><span data-stu-id="cd16f-107">Treats cookies as `SameSite=Lax` by default.</span></span>
* <span data-ttu-id="cd16f-108">Os cookies de Estados que declaram explicitamente `SameSite=None` para habilitar a entrega entre sites devem ser marcados como `Secure`.</span><span class="sxs-lookup"><span data-stu-id="cd16f-108">States cookies that explicitly assert `SameSite=None` in order to enable cross-site delivery should be marked as `Secure`.</span></span>

<span data-ttu-id="cd16f-109">`Lax` funciona para a maioria dos cookies de aplicativo.</span><span class="sxs-lookup"><span data-stu-id="cd16f-109">`Lax` works for most app cookies.</span></span> <span data-ttu-id="cd16f-110">Algumas formas de autenticação, como o [OpenID Connect](https://openid.net/connect/) (OIDC) e o [WS-Federation,](https://auth0.com/docs/protocols/ws-fed) assumem o padrão de redirecionamentos baseados em post.</span><span class="sxs-lookup"><span data-stu-id="cd16f-110">Some forms of authentication like [OpenID Connect](https://openid.net/connect/) (OIDC) and [WS-Federation](https://auth0.com/docs/protocols/ws-fed) default to POST based redirects.</span></span> <span data-ttu-id="cd16f-111">Os redirecionamentos baseados em POST disparam as proteções do navegador SameSite, portanto, o SameSite está desabilitado para esses componentes.</span><span class="sxs-lookup"><span data-stu-id="cd16f-111">The POST based redirects trigger the SameSite browser protections, so SameSite is disabled for these components.</span></span> <span data-ttu-id="cd16f-112">A maioria dos logons [OAuth](https://oauth.net/) não são afetados devido a diferenças na forma como os fluxos de solicitação.</span><span class="sxs-lookup"><span data-stu-id="cd16f-112">Most [OAuth](https://oauth.net/) logins are not affected due to differences in how the request flows.</span></span>

<span data-ttu-id="cd16f-113">O parâmetro `None` causa problemas de compatibilidade com clientes que implementaram o padrão anterior de [rascunho 2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07) (por exemplo, IOS 12).</span><span class="sxs-lookup"><span data-stu-id="cd16f-113">The `None` parameter causes compatibility problems with clients that implemented the prior [2016 draft standard](https://tools.ietf.org/html/draft-west-first-party-cookies-07) (for example, iOS 12).</span></span> <span data-ttu-id="cd16f-114">Consulte [suporte a navegadores mais antigos](#sob) neste documento.</span><span class="sxs-lookup"><span data-stu-id="cd16f-114">See [Supporting older browsers](#sob) in this document.</span></span>

<span data-ttu-id="cd16f-115">Cada componente ASP.NET Core que emite cookies precisa decidir se SameSite é apropriado.</span><span class="sxs-lookup"><span data-stu-id="cd16f-115">Each ASP.NET Core component that emits cookies needs to decide if SameSite is appropriate.</span></span>

## <a name="api-usage-with-samesite"></a><span data-ttu-id="cd16f-116">Uso da API com SameSite</span><span class="sxs-lookup"><span data-stu-id="cd16f-116">API usage with SameSite</span></span>

<span data-ttu-id="cd16f-117">Consulte a [Propriedade HttpCookie. SameSite](/dotnet/api/system.web.httpcookie.samesite#System_Web_HttpCookie_SameSite)</span><span class="sxs-lookup"><span data-stu-id="cd16f-117">See [HttpCookie.SameSite Property](/dotnet/api/system.web.httpcookie.samesite#System_Web_HttpCookie_SameSite)</span></span>

## <a name="history-and-changes"></a><span data-ttu-id="cd16f-118">Histórico e alterações</span><span class="sxs-lookup"><span data-stu-id="cd16f-118">History and changes</span></span>

<span data-ttu-id="cd16f-119">O suporte a SameSite foi implementado pela primeira vez no .NET 4.7.2 usando o [padrão de rascunho 2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07#section-4.1).</span><span class="sxs-lookup"><span data-stu-id="cd16f-119">SameSite support was first implemented in .NET 4.7.2 using the [2016 draft standard](https://tools.ietf.org/html/draft-west-first-party-cookies-07#section-4.1).</span></span>

<span data-ttu-id="cd16f-120">As atualizações de 19 de novembro de 2019 para o Windows atualizaram o .NET 4.7.2 + do padrão 2016 para o padrão 2019.</span><span class="sxs-lookup"><span data-stu-id="cd16f-120">The November 19, 2019 updates for Windows updated .NET 4.7.2+ from the 2016 standard to the 2019 standard.</span></span> <span data-ttu-id="cd16f-121">Atualizações adicionais estão em breve para outras versões do Windows.</span><span class="sxs-lookup"><span data-stu-id="cd16f-121">Additional updates are forthcoming for other versions of Windows.</span></span> <span data-ttu-id="cd16f-122">Consulte os seguintes KB para obter mais informações:</span><span class="sxs-lookup"><span data-stu-id="cd16f-122">See the following KB's for more information:</span></span>

* [<span data-ttu-id="cd16f-123">Artigo 4531182 da base de conhecimento</span><span class="sxs-lookup"><span data-stu-id="cd16f-123">KB article 4531182</span></span>](https://support.microsoft.com/help/4531182/kb4531182)
* [<span data-ttu-id="cd16f-124">Artigo 4524421 da base de conhecimento</span><span class="sxs-lookup"><span data-stu-id="cd16f-124">KB article 4524421</span></span>](https://support.microsoft.com/help/4524421/kb4524421)

 <span data-ttu-id="cd16f-125">O rascunho 2019 da especificação de SameSite:</span><span class="sxs-lookup"><span data-stu-id="cd16f-125">The 2019 draft of the SameSite specification:</span></span>

* <span data-ttu-id="cd16f-126">**Não** é compatível com versões anteriores com o rascunho 2016.</span><span class="sxs-lookup"><span data-stu-id="cd16f-126">Is **not** backwards compatible with the 2016 draft.</span></span> <span data-ttu-id="cd16f-127">Para obter mais informações, consulte [dando suporte a navegadores mais antigos](#sob) neste documento.</span><span class="sxs-lookup"><span data-stu-id="cd16f-127">For more information, see [Supporting older browsers](#sob) in this document.</span></span>
* <span data-ttu-id="cd16f-128">Especifica que os cookies são tratados como `SameSite=Lax` por padrão.</span><span class="sxs-lookup"><span data-stu-id="cd16f-128">Specifies cookies are treated as `SameSite=Lax` by default.</span></span>
* <span data-ttu-id="cd16f-129">Especifica cookies que declaram explicitamente `SameSite=None` para habilitar a entrega entre sites devem ser marcados como `Secure`.</span><span class="sxs-lookup"><span data-stu-id="cd16f-129">Specifies cookies that explicitly assert `SameSite=None` in order to enable cross-site delivery should be marked as `Secure`.</span></span> <span data-ttu-id="cd16f-130">`None` é uma nova entrada a ser recusada.</span><span class="sxs-lookup"><span data-stu-id="cd16f-130">`None` is a new entry to opt out.</span></span>
* <span data-ttu-id="cd16f-131">O é suportado por patches emitidos conforme descrito em KB listados acima.</span><span class="sxs-lookup"><span data-stu-id="cd16f-131">Is supported by patches issued as described in the KB's listed above.</span></span>
* <span data-ttu-id="cd16f-132">Está agendado para ser habilitado pelo [Chrome](https://chromestatus.com/feature/5088147346030592) por padrão em [fevereiro de 2020](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html).</span><span class="sxs-lookup"><span data-stu-id="cd16f-132">Is scheduled to be enabled by [Chrome](https://chromestatus.com/feature/5088147346030592) by default in [Feb 2020](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html).</span></span> <span data-ttu-id="cd16f-133">Os navegadores começaram a passar para esse padrão em 2019.</span><span class="sxs-lookup"><span data-stu-id="cd16f-133">Browsers started moving to this standard in 2019.</span></span>

<a name="sob"></a>

## <a name="supporting-older-browsers"></a><span data-ttu-id="cd16f-134">Suporte a navegadores mais antigos</span><span class="sxs-lookup"><span data-stu-id="cd16f-134">Supporting older browsers</span></span>

<span data-ttu-id="cd16f-135">O padrão de 2016 SameSite exigiu que valores desconhecidos devem ser tratados como valores `SameSite=Strict`.</span><span class="sxs-lookup"><span data-stu-id="cd16f-135">The 2016 SameSite standard mandated that unknown values must be treated as `SameSite=Strict` values.</span></span> <span data-ttu-id="cd16f-136">Os aplicativos acessados de navegadores mais antigos que dão suporte ao padrão de 2016 SameSite podem falhar quando obtêm uma propriedade SameSite com um valor de `None`.</span><span class="sxs-lookup"><span data-stu-id="cd16f-136">Apps accessed from older browsers which support the 2016 SameSite standard may break when they get a SameSite property with a value of `None`.</span></span> <span data-ttu-id="cd16f-137">Os aplicativos Web devem implementar a detecção do navegador se pretenderem oferecer suporte a navegadores mais antigos.</span><span class="sxs-lookup"><span data-stu-id="cd16f-137">Web apps must implement browser detection if they intend to support older browsers.</span></span> <span data-ttu-id="cd16f-138">O ASP.NET não implementa a detecção de navegador, pois os valores dos agentes de usuário são altamente voláteis e mudam com frequência.</span><span class="sxs-lookup"><span data-stu-id="cd16f-138">ASP.NET doesn't implement browser detection because User-Agents values are highly volatile and change frequently.</span></span> <span data-ttu-id="cd16f-139">O código a seguir pode ser chamado no site de chamada <xref:HTTP.HttpCookie>:</span><span class="sxs-lookup"><span data-stu-id="cd16f-139">The following code can be called at the <xref:HTTP.HttpCookie> call site:</span></span>

[!code-csharp[](sample/SameSiteCheck.cs?name=snippet)]

<span data-ttu-id="cd16f-140">No exemplo anterior, `MyUserAgentDetectionLib.DisallowsSameSiteNone` é uma biblioteca fornecida pelo usuário que detecta se o agente do usuário não dá suporte a `None`SameSite.</span><span class="sxs-lookup"><span data-stu-id="cd16f-140">In the preceding sample, `MyUserAgentDetectionLib.DisallowsSameSiteNone` is a user supplied library that detects if the user agent doesn't support SameSite `None`.</span></span> <span data-ttu-id="cd16f-141">O código a seguir mostra um exemplo de método de `DisallowsSameSiteNone`:</span><span class="sxs-lookup"><span data-stu-id="cd16f-141">The following code shows a sample `DisallowsSameSiteNone` method:</span></span>

> [!WARNING]
> <span data-ttu-id="cd16f-142">O código a seguir é apenas para demonstração:</span><span class="sxs-lookup"><span data-stu-id="cd16f-142">The following code is for demonstration only:</span></span>
> * <span data-ttu-id="cd16f-143">Ele não deve ser considerado concluído.</span><span class="sxs-lookup"><span data-stu-id="cd16f-143">It should not be considered complete.</span></span>
> * <span data-ttu-id="cd16f-144">Ele não é mantido ou tem suporte.</span><span class="sxs-lookup"><span data-stu-id="cd16f-144">It is not maintained or supported.</span></span>

[!code-csharp[](sample/SameSiteCheck.cs?name=snippet2)]

## <a name="test-apps-for-samesite-problems"></a><span data-ttu-id="cd16f-145">Testar aplicativos para problemas de SameSite</span><span class="sxs-lookup"><span data-stu-id="cd16f-145">Test apps for SameSite problems</span></span>

<span data-ttu-id="cd16f-146">Aplicativos que interagem com sites remotos, como por meio de logon de terceiros, precisam:</span><span class="sxs-lookup"><span data-stu-id="cd16f-146">Apps that interact with remote sites such as through third-party login need to:</span></span>

* <span data-ttu-id="cd16f-147">Teste a interação em vários navegadores.</span><span class="sxs-lookup"><span data-stu-id="cd16f-147">Test the interaction on multiple browsers.</span></span>
* <span data-ttu-id="cd16f-148">Aplique a [detecção e mitigação do navegador](#sob) discutidas neste documento.</span><span class="sxs-lookup"><span data-stu-id="cd16f-148">Apply the [browser detection and mitigation](#sob) discussed in this document.</span></span>

<span data-ttu-id="cd16f-149">Teste os aplicativos Web usando uma versão do cliente que pode aceitar o novo comportamento de SameSite.</span><span class="sxs-lookup"><span data-stu-id="cd16f-149">Test web apps using a client version that can opt-in to the new SameSite behavior.</span></span> <span data-ttu-id="cd16f-150">O Chrome, o Firefox e o Chromium Edge têm novos sinalizadores de recurso de aceitação que podem ser usados para teste.</span><span class="sxs-lookup"><span data-stu-id="cd16f-150">Chrome, Firefox, and Chromium Edge all have new opt-in feature flags that can be used for testing.</span></span> <span data-ttu-id="cd16f-151">Depois que seu aplicativo aplicar os patches do SameSite, teste-o com versões mais antigas do cliente, especialmente o Safari.</span><span class="sxs-lookup"><span data-stu-id="cd16f-151">After your app applies the SameSite patches, test it with older client versions, especially Safari.</span></span> <span data-ttu-id="cd16f-152">Para obter mais informações, consulte [dando suporte a navegadores mais antigos](#sob) neste documento.</span><span class="sxs-lookup"><span data-stu-id="cd16f-152">For more information, see [Supporting older browsers](#sob) in this document.</span></span>

### <a name="test-with-chrome"></a><span data-ttu-id="cd16f-153">Teste com o Chrome</span><span class="sxs-lookup"><span data-stu-id="cd16f-153">Test with Chrome</span></span>

<span data-ttu-id="cd16f-154">O Chrome 78 + fornece resultados enganosos porque tem uma mitigação temporária em vigor.</span><span class="sxs-lookup"><span data-stu-id="cd16f-154">Chrome 78+ gives misleading results because it has a temporary mitigation in place.</span></span> <span data-ttu-id="cd16f-155">A mitigação do Chrome 78 + temporária permite cookies com menos de dois minutos.</span><span class="sxs-lookup"><span data-stu-id="cd16f-155">The Chrome 78+ temporary mitigation allows cookies less than two minutes old.</span></span> <span data-ttu-id="cd16f-156">O Chrome 76 ou 77 com os sinalizadores de teste apropriados habilitados fornece resultados mais precisos.</span><span class="sxs-lookup"><span data-stu-id="cd16f-156">Chrome 76 or 77 with the appropriate test flags enabled provides more accurate results.</span></span> <span data-ttu-id="cd16f-157">Para testar o novo comportamento de SameSite, alterne `chrome://flags/#same-site-by-default-cookies` para **habilitado**.</span><span class="sxs-lookup"><span data-stu-id="cd16f-157">To test the new SameSite behavior toggle `chrome://flags/#same-site-by-default-cookies` to **Enabled**.</span></span> <span data-ttu-id="cd16f-158">Versões mais antigas do Chrome (75 e inferior) são relatadas para falha com a nova configuração de `None`.</span><span class="sxs-lookup"><span data-stu-id="cd16f-158">Older versions of Chrome (75 and below) are reported to fail with the new `None` setting.</span></span> <span data-ttu-id="cd16f-159">Consulte [suporte a navegadores mais antigos](#sob) neste documento.</span><span class="sxs-lookup"><span data-stu-id="cd16f-159">See [Supporting older browsers](#sob) in this document.</span></span>

<span data-ttu-id="cd16f-160">O Google não disponibiliza versões mais antigas do Chrome.</span><span class="sxs-lookup"><span data-stu-id="cd16f-160">Google does not make older chrome versions available.</span></span> <span data-ttu-id="cd16f-161">Siga as instruções em [baixar o Chromium](https://www.chromium.org/getting-involved/download-chromium) para testar versões anteriores do Chrome.</span><span class="sxs-lookup"><span data-stu-id="cd16f-161">Follow the instructions at [Download Chromium](https://www.chromium.org/getting-involved/download-chromium) to test older versions of Chrome.</span></span> <span data-ttu-id="cd16f-162">**Não** Baixe o Chrome de links fornecidos pela pesquisa de versões mais antigas do Chrome.</span><span class="sxs-lookup"><span data-stu-id="cd16f-162">Do **not** download Chrome from links provided by searching for older versions of chrome.</span></span>

* [<span data-ttu-id="cd16f-163">Chromium 76 Win64</span><span class="sxs-lookup"><span data-stu-id="cd16f-163">Chromium 76 Win64</span></span>](https://commondatastorage.googleapis.com/chromium-browser-snapshots/index.html?prefix=Win_x64/664998/)
* [<span data-ttu-id="cd16f-164">Chromium 74 Win64</span><span class="sxs-lookup"><span data-stu-id="cd16f-164">Chromium 74 Win64</span></span>](https://commondatastorage.googleapis.com/chromium-browser-snapshots/index.html?prefix=Win_x64/638880/)

### <a name="test-with-safari"></a><span data-ttu-id="cd16f-165">Teste com o Safari</span><span class="sxs-lookup"><span data-stu-id="cd16f-165">Test with Safari</span></span>

<span data-ttu-id="cd16f-166">O Safari 12 implementou estritamente o rascunho anterior e falha quando o novo valor de `None` está em um cookie.</span><span class="sxs-lookup"><span data-stu-id="cd16f-166">Safari 12 strictly implemented the prior draft and fails when the new `None` value is in a cookie.</span></span> <span data-ttu-id="cd16f-167">`None` é evitada por meio do código de detecção de navegador que [dá suporte a navegadores mais antigos](#sob) neste documento.</span><span class="sxs-lookup"><span data-stu-id="cd16f-167">`None` is avoided via the browser detection code [Supporting older browsers](#sob) in this document.</span></span> <span data-ttu-id="cd16f-168">Teste OS logons de estilo do sistema operacional baseado no Safari 12, Safari 13 e WebKit usando MSAL, ADAL ou qualquer biblioteca que você esteja usando.</span><span class="sxs-lookup"><span data-stu-id="cd16f-168">Test Safari 12, Safari 13, and WebKit based OS style logins using MSAL, ADAL or whatever library you are using.</span></span> <span data-ttu-id="cd16f-169">O problema depende da versão subjacente do sistema operacional.</span><span class="sxs-lookup"><span data-stu-id="cd16f-169">The problem is dependent on the underlying OS version.</span></span> <span data-ttu-id="cd16f-170">OSX Mojave (10,14) e iOS 12 são conhecidos por ter problemas de compatibilidade com o novo comportamento de SameSite.</span><span class="sxs-lookup"><span data-stu-id="cd16f-170">OSX Mojave (10.14) and iOS 12 are known to have compatibility problems with the new SameSite behavior.</span></span> <span data-ttu-id="cd16f-171">Atualizar o sistema operacional para OSX Catalina (10,15) ou iOS 13 corrige o problema.</span><span class="sxs-lookup"><span data-stu-id="cd16f-171">Upgrading the OS to OSX Catalina (10.15) or iOS 13 fixes the problem.</span></span> <span data-ttu-id="cd16f-172">No momento, o Safari não tem um sinalizador de aceitação para testar o novo comportamento de especificação.</span><span class="sxs-lookup"><span data-stu-id="cd16f-172">Safari does not currently have an opt-in flag for testing the new spec behavior.</span></span>

### <a name="test-with-firefox"></a><span data-ttu-id="cd16f-173">Testar com o Firefox</span><span class="sxs-lookup"><span data-stu-id="cd16f-173">Test with Firefox</span></span>

<span data-ttu-id="cd16f-174">O suporte do Firefox para o novo padrão pode ser testado na versão 68 +, optando na página de `about:config` com o sinalizador de recurso `network.cookie.sameSite.laxByDefault`.</span><span class="sxs-lookup"><span data-stu-id="cd16f-174">Firefox support for the new standard can be tested on version 68+ by opting in on the `about:config` page with the feature flag `network.cookie.sameSite.laxByDefault`.</span></span> <span data-ttu-id="cd16f-175">Não há relatórios de problemas de compatibilidade com versões mais antigas do Firefox.</span><span class="sxs-lookup"><span data-stu-id="cd16f-175">There haven't been reports of compatibility issues with older versions of Firefox.</span></span>

### <a name="test-with-edge-browser"></a><span data-ttu-id="cd16f-176">Testar com o navegador Edge</span><span class="sxs-lookup"><span data-stu-id="cd16f-176">Test with Edge browser</span></span>

<span data-ttu-id="cd16f-177">O Edge dá suporte ao antigo SameSite padrão.</span><span class="sxs-lookup"><span data-stu-id="cd16f-177">Edge supports the old SameSite standard.</span></span> <span data-ttu-id="cd16f-178">A versão 44 do Edge não tem nenhum problema de compatibilidade conhecido com o novo padrão.</span><span class="sxs-lookup"><span data-stu-id="cd16f-178">Edge version 44 doesn't have any known compatibility problems with the new standard.</span></span>

### <a name="test-with-edge-chromium"></a><span data-ttu-id="cd16f-179">Testar com borda (Chromium)</span><span class="sxs-lookup"><span data-stu-id="cd16f-179">Test with Edge (Chromium)</span></span>

<span data-ttu-id="cd16f-180">Os sinalizadores SameSite são definidos na página `edge://flags/#same-site-by-default-cookies`.</span><span class="sxs-lookup"><span data-stu-id="cd16f-180">SameSite flags are set on the `edge://flags/#same-site-by-default-cookies` page.</span></span> <span data-ttu-id="cd16f-181">Nenhum problema de compatibilidade foi descoberto com o Edge Chromium.</span><span class="sxs-lookup"><span data-stu-id="cd16f-181">No compatibility issues were discovered with Edge Chromium.</span></span>

### <a name="test-with-electron"></a><span data-ttu-id="cd16f-182">Testar com o</span><span class="sxs-lookup"><span data-stu-id="cd16f-182">Test with Electron</span></span>

<span data-ttu-id="cd16f-183">As versões do at-SS são versões mais antigas do Chromium.</span><span class="sxs-lookup"><span data-stu-id="cd16f-183">Versions of Electron include older versions of Chromium.</span></span> <span data-ttu-id="cd16f-184">Por exemplo, a versão do at-SS usada pelas equipes é Chromium 66, que exibe o comportamento mais antigo.</span><span class="sxs-lookup"><span data-stu-id="cd16f-184">For example, the version of Electron used by Teams is Chromium 66, which exhibits the older behavior.</span></span> <span data-ttu-id="cd16f-185">Você deve executar seu próprio teste de compatibilidade com a versão do uso de digital que seu produto usa.</span><span class="sxs-lookup"><span data-stu-id="cd16f-185">You must perform your own compatibility testing with the version of Electron your product uses.</span></span> <span data-ttu-id="cd16f-186">Consulte [suporte a navegadores mais antigos](#sob) na seção a seguir.</span><span class="sxs-lookup"><span data-stu-id="cd16f-186">See [Supporting older browsers](#sob) in the following section.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="cd16f-187">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="cd16f-187">Additional resources</span></span>

* [<span data-ttu-id="cd16f-188">Blog do Chromium: desenvolvedores: Prepare-se para o New SameSite = None; Configurações de cookie seguro</span><span class="sxs-lookup"><span data-stu-id="cd16f-188">Chromium Blog:Developers: Get Ready for New SameSite=None; Secure Cookie Settings</span></span>](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html)
* [<span data-ttu-id="cd16f-189">SameSite cookies explicados</span><span class="sxs-lookup"><span data-stu-id="cd16f-189">SameSite cookies explained</span></span>](https://web.dev/samesite-cookies-explained/)