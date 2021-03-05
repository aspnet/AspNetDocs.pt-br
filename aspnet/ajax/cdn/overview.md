---
uid: ajax/cdn/overview
title: Rede de distribuição de conteúdo do Microsoft Ajax | Microsoft Docs
author: rick-anderson
description: ''
ms.author: riande
ms.date: 10/14/2017
ms.assetid: 8935bf14-ca6d-4a4e-9dbe-b96ce74cef49
msc.legacyurl: /ajax/cdn
msc.type: content
ms.openlocfilehash: 4f5cb34ac8ef1750e7aacbbb47713846674e053d
ms.sourcegitcommit: d5049dfb08ff14872ba3b29a702e0589a776b430
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/05/2021
ms.locfileid: "102194572"
---
# <a name="microsoft-ajax-content-delivery-network"></a><span data-ttu-id="c8ce2-102">Rede de Distribuição de Conteúdo do Microsoft Ajax</span><span class="sxs-lookup"><span data-stu-id="c8ce2-102">Microsoft Ajax Content Delivery Network</span></span>

> [!WARNING]
> <span data-ttu-id="c8ce2-103">Os aplicativos de produção não devem ter uma dependência sólida dos ativos da CDN.</span><span class="sxs-lookup"><span data-stu-id="c8ce2-103">Production applications should not take a hard dependency on CDN assets.</span></span> <span data-ttu-id="c8ce2-104">Os aplicativos devem testar o ativo CDN referenciado e usar um ativo de fallback quando a CDN não estiver disponível.</span><span class="sxs-lookup"><span data-stu-id="c8ce2-104">Applications should test for the CDN asset referenced, and use a fallback asset when the CDN is not available.</span></span>
>
> <span data-ttu-id="c8ce2-105">A CDN do Microsoft Ajax não tem nenhum SLA acima e além de usar uma CDN do Azure.</span><span class="sxs-lookup"><span data-stu-id="c8ce2-105">The Microsoft Ajax CDN has no SLA above and beyond using an Azure CDN.</span></span>
>
> <span data-ttu-id="c8ce2-106">Use [este problema do GitHub](https://github.com/dotnet/AspNetDocs/issues/116) para relatar problemas com a CDN do Microsoft Ajax.</span><span class="sxs-lookup"><span data-stu-id="c8ce2-106">Use [this GitHub issue](https://github.com/dotnet/AspNetDocs/issues/116) to report problems with the Microsoft Ajax CDN.</span></span>

## <a name="table-of-contents"></a><span data-ttu-id="c8ce2-107">Sumário</span><span class="sxs-lookup"><span data-stu-id="c8ce2-107">Table of Contents</span></span>

<span data-ttu-id="c8ce2-108">**[ajax.microsoft.com renomeado como ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span><span class="sxs-lookup"><span data-stu-id="c8ce2-108">**[ajax.microsoft.com renamed to ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span></span>  
<span data-ttu-id="c8ce2-109">**[Suporte ao Visual Studio. vsdoc](#Visual_Studio_vsdoc_Support_19)**</span><span class="sxs-lookup"><span data-stu-id="c8ce2-109">**[Visual Studio .vsdoc Support](#Visual_Studio_vsdoc_Support_19)**</span></span>  
<span data-ttu-id="c8ce2-110">**[Usando o ASP.NET AJAX da CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span><span class="sxs-lookup"><span data-stu-id="c8ce2-110">**[Using ASP.NET Ajax from the CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span></span>  
<span data-ttu-id="c8ce2-111">**[Usando o jQuery da CDN](#Using_jQuery_from_the_CDN_21)**</span><span class="sxs-lookup"><span data-stu-id="c8ce2-111">**[Using jQuery from the CDN](#Using_jQuery_from_the_CDN_21)**</span></span>  
<span data-ttu-id="c8ce2-112">**[Usando a interface do usuário do jQuery da CDN](#Using_jQuery_UI_from_the_CDN_22)**</span><span class="sxs-lookup"><span data-stu-id="c8ce2-112">**[Using jQuery UI from the CDN](#Using_jQuery_UI_from_the_CDN_22)**</span></span>  
<span data-ttu-id="c8ce2-113">**[Arquivos de terceiros na CDN](#Third-Party_Files_on_the_CDN_23)**</span><span class="sxs-lookup"><span data-stu-id="c8ce2-113">**[Third-Party Files on the CDN](#Third-Party_Files_on_the_CDN_23)**</span></span>  
  
 [<span data-ttu-id="c8ce2-114">Versões do jQuery na CDN</span><span class="sxs-lookup"><span data-stu-id="c8ce2-114">jQuery Releases on the CDN</span></span>](#jQuery_Releases_on_the_CDN_0)  
 [<span data-ttu-id="c8ce2-115">Versões de migração do jQuery no CDN</span><span class="sxs-lookup"><span data-stu-id="c8ce2-115">jQuery Migrate Releases on the CDN</span></span>](#jQuery_Migrate_Releases_on_the_CDN_1)  
 [<span data-ttu-id="c8ce2-116">Versões da interface do usuário do jQuery na CDN</span><span class="sxs-lookup"><span data-stu-id="c8ce2-116">jQuery UI Releases on the CDN</span></span>](#jQuery_UI_Releases_on_the_CDN_2)  
 [<span data-ttu-id="c8ce2-117">Versões de validação do jQuery na CDN</span><span class="sxs-lookup"><span data-stu-id="c8ce2-117">jQuery Validation Releases on the CDN</span></span>](#jQuery_Validation_Releases_on_the_CDN_3)  
 [<span data-ttu-id="c8ce2-118">Versões móveis do jQuery no CDN</span><span class="sxs-lookup"><span data-stu-id="c8ce2-118">jQuery Mobile Releases on the CDN</span></span>](#jQuery_Mobile_Releases_on_the_CDN_4)  
 [<span data-ttu-id="c8ce2-119">Versões de modelos jQuery no CDN</span><span class="sxs-lookup"><span data-stu-id="c8ce2-119">jQuery Templates Releases on the CDN</span></span>](#jQuery_Templates_Releases_on_the_CDN_5)  
 [<span data-ttu-id="c8ce2-120">Versões de ciclo jQuery na CDN</span><span class="sxs-lookup"><span data-stu-id="c8ce2-120">jQuery Cycle Releases on the CDN</span></span>](#jQuery_Cycle_Releases_on_the_CDN_6)  
 [<span data-ttu-id="c8ce2-121">Versões do jQuery DataTables no CDN</span><span class="sxs-lookup"><span data-stu-id="c8ce2-121">jQuery DataTables Releases on the CDN</span></span>](#jQuery_DataTables_Releases_on_the_CDN_7)  
 [<span data-ttu-id="c8ce2-122">Versões do Modernizr na CDN</span><span class="sxs-lookup"><span data-stu-id="c8ce2-122">Modernizr Releases on the CDN</span></span>](#Modernizr_Releases_on_the_CDN_8)  
 [<span data-ttu-id="c8ce2-123">Versões JSHint no CDN</span><span class="sxs-lookup"><span data-stu-id="c8ce2-123">JSHint Releases on the CDN</span></span>](#JSHint_Releases_on_the_CDN_10)  
 [<span data-ttu-id="c8ce2-124">Versões do Knockout no CDN</span><span class="sxs-lookup"><span data-stu-id="c8ce2-124">Knockout Releases on the CDN</span></span>](#Knockout_Releases_on_the_CDN_11)  
 [<span data-ttu-id="c8ce2-125">Globalizar versões no CDN</span><span class="sxs-lookup"><span data-stu-id="c8ce2-125">Globalize Releases on the CDN</span></span>](#Globalize_Releases_on_the_CDN_12)  
 [<span data-ttu-id="c8ce2-126">Responder a versões no CDN</span><span class="sxs-lookup"><span data-stu-id="c8ce2-126">Respond Releases on the CDN</span></span>](#Respond_Releases_on_the_CDN_13)  
 [<span data-ttu-id="c8ce2-127">Versões de Bootstrap no CDN</span><span class="sxs-lookup"><span data-stu-id="c8ce2-127">Bootstrap Releases on the CDN</span></span>](#Bootstrap_Releases_on_the_CDN_14)  
 [<span data-ttu-id="c8ce2-128">Versões TouchCarousel de Bootstrap no CDN</span><span class="sxs-lookup"><span data-stu-id="c8ce2-128">Bootstrap TouchCarousel Releases on the CDN</span></span>](#BootstrapTouchCarousel_Releases_on_the_CDN_18)  
 [<span data-ttu-id="c8ce2-129">Hammer.js versões no CDN</span><span class="sxs-lookup"><span data-stu-id="c8ce2-129">Hammer.js Releases on the CDN</span></span>](#Hammerjs_Releases_on_the_CDN_19)  
 [<span data-ttu-id="c8ce2-130">Versões Web Forms e AJAX do ASP.NET no CDN</span><span class="sxs-lookup"><span data-stu-id="c8ce2-130">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>](#ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15)  
 [<span data-ttu-id="c8ce2-131">Versões MVC do ASP.NET no CDN</span><span class="sxs-lookup"><span data-stu-id="c8ce2-131">ASP.NET MVC Releases on the CDN</span></span>](#ASPNET_MVC_Releases_on_the_CDN_16)  
 [<span data-ttu-id="c8ce2-132">Versões do Signalr ASP.NET no CDN</span><span class="sxs-lookup"><span data-stu-id="c8ce2-132">ASP.NET SignalR Releases on the CDN</span></span>](#ASPNET_SignalR_Releases_on_the_CDN_17)

<span data-ttu-id="c8ce2-133">A CDN (rede de distribuição de conteúdo) do Microsoft Ajax hospeda bibliotecas JavaScript populares de terceiros, como o jQuery, e permite que você as Adicione facilmente aos seus aplicativos Web.</span><span class="sxs-lookup"><span data-stu-id="c8ce2-133">The Microsoft Ajax Content Delivery Network (CDN) hosts popular third party JavaScript libraries such as jQuery and enables you to easily add them to your Web applications.</span></span> <span data-ttu-id="c8ce2-134">Por exemplo, você pode começar a usar o jQuery, que é hospedado nessa CDN simplesmente adicionando &lt; uma &gt; marca de script à sua página que aponta para AJAX.aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="c8ce2-134">For example, you can start using jQuery which is hosted on this CDN simply by adding a &lt;script&gt; tag to your page that points to ajax.aspnetcdn.com.</span></span>

<span data-ttu-id="c8ce2-135">Aproveitando a CDN, você pode melhorar significativamente o desempenho de seus aplicativos AJAX.</span><span class="sxs-lookup"><span data-stu-id="c8ce2-135">By taking advantage of the CDN, you can significantly improve the performance of your Ajax applications.</span></span> <span data-ttu-id="c8ce2-136">O conteúdo da CDN é armazenado em cache em servidores localizados em todo o mundo.</span><span class="sxs-lookup"><span data-stu-id="c8ce2-136">The contents of the CDN are cached on servers located around the world.</span></span> <span data-ttu-id="c8ce2-137">Além disso, a CDN permite que os navegadores reutilizem arquivos JavaScript de terceiros em cache para sites que estão localizados em domínios diferentes.</span><span class="sxs-lookup"><span data-stu-id="c8ce2-137">In addition, the CDN enables browsers to reuse cached third party JavaScript files for web sites that are located in different domains.</span></span>

<span data-ttu-id="c8ce2-138">A CDN oferece suporte a SSL (HTTPS) caso você precise fornecer uma página da Web usando o protocolo SSL.</span><span class="sxs-lookup"><span data-stu-id="c8ce2-138">The CDN supports SSL (HTTPS) in case you need to serve a web page using the Secure Sockets Layer.</span></span>

<span data-ttu-id="c8ce2-139">A CDN hospeda as seguintes bibliotecas de scripts de terceiros que foram carregadas e são licenciadas para você, pelos proprietários dessas bibliotecas:</span><span class="sxs-lookup"><span data-stu-id="c8ce2-139">The CDN hosts the following third party script libraries which have been uploaded, and are licensed to you, by the owners of those libraries:</span></span>

- <span data-ttu-id="c8ce2-140">jQuery (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="c8ce2-140">jQuery (www.jquery.com)</span></span>
- <span data-ttu-id="c8ce2-141">interface do usuário do jQuery (www.jqueryui.com)</span><span class="sxs-lookup"><span data-stu-id="c8ce2-141">jQuery UI (www.jqueryui.com)</span></span>
- <span data-ttu-id="c8ce2-142">jQuery Mobile (www.jquerymobile.com)</span><span class="sxs-lookup"><span data-stu-id="c8ce2-142">jQuery Mobile (www.jquerymobile.com)</span></span>
- <span data-ttu-id="c8ce2-143">Validação do jQuery (https://jqueryvalidation.org/)</span><span class="sxs-lookup"><span data-stu-id="c8ce2-143">jQuery Validation (https://jqueryvalidation.org/)</span></span>
- <span data-ttu-id="c8ce2-144">Ciclo do jQuery (www.malsup.com/jquery/cycle/)</span><span class="sxs-lookup"><span data-stu-id="c8ce2-144">jQuery Cycle (www.malsup.com/jquery/cycle/)</span></span>
- <span data-ttu-id="c8ce2-145">jQuery DataTables (http://datatables.net/)</span><span class="sxs-lookup"><span data-stu-id="c8ce2-145">jQuery DataTables (http://datatables.net/)</span></span>

<span data-ttu-id="c8ce2-146">A CDN do Microsoft AJAX também inclui as seguintes bibliotecas que foram carregadas pela Microsoft:</span><span class="sxs-lookup"><span data-stu-id="c8ce2-146">The Microsoft Ajax CDN also includes the following libraries which have been uploaded by Microsoft:</span></span>

- <span data-ttu-id="c8ce2-147">ASP.NET Ajax</span><span class="sxs-lookup"><span data-stu-id="c8ce2-147">ASP.NET Ajax</span></span>
- <span data-ttu-id="c8ce2-148">Arquivos JavaScript do ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="c8ce2-148">ASP.NET MVC JavaScript Files</span></span>
- <span data-ttu-id="c8ce2-149">Arquivos JavaScript do Signalr ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c8ce2-149">ASP.NET SignalR JavaScript Files</span></span>

<span data-ttu-id="c8ce2-150">A Microsoft não alega a propriedade de nenhuma biblioteca de terceiros hospedada nessa CDN.</span><span class="sxs-lookup"><span data-stu-id="c8ce2-150">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="c8ce2-151">Os proprietários de direitos autorais das bibliotecas estão licenciando essas bibliotecas para você.</span><span class="sxs-lookup"><span data-stu-id="c8ce2-151">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="c8ce2-152">Quaisquer direitos que você precise baixar e usar essas bibliotecas são concedidos exclusivamente pelos respectivos proprietários de direitos autorais.</span><span class="sxs-lookup"><span data-stu-id="c8ce2-152">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="c8ce2-153">Como essas não são bibliotecas da Microsoft, a Microsoft não fornece nenhuma garantia ou licença de direitos de propriedade intelectual (incluindo nenhum direito de patente implícito) para as bibliotecas de terceiros hospedadas nessa CDN.</span><span class="sxs-lookup"><span data-stu-id="c8ce2-153">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<span data-ttu-id="c8ce2-154">Se você quiser enviar sua biblioteca JavaScript e sua biblioteca for uma das principais bibliotecas JavaScript (conforme listado em http://trends.builtwith.com) ou extensões/plug-ins para essas bibliotecas que são (a) populares; ou (b) úteis para uso em ASP.net, entre em contato com AjaxCDNSubmission@Microsoft.com .</span><span class="sxs-lookup"><span data-stu-id="c8ce2-154">If you wish to submit your JavaScript library and your library is one of the top JavaScript libraries (as listed on http://trends.builtwith.com) or extensions/plugins to these libraries that are (a) popular; or (b) helpful for use on ASP.NET then please contact AjaxCDNSubmission@Microsoft.com.</span></span>

<a id="ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18"></a>

## <a name="ajaxmicrosoftcom-renamed-to-ajaxaspnetcdncom"></a><span data-ttu-id="c8ce2-155">ajax.microsoft.com renomeado como ajax.aspnetcdn.com</span><span class="sxs-lookup"><span data-stu-id="c8ce2-155">ajax.microsoft.com renamed to ajax.aspnetcdn.com</span></span>

<span data-ttu-id="c8ce2-156">A CDN usada para usar o nome de domínio microsoft.com e foi alterada para usar o nome de domínio aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="c8ce2-156">The CDN used to use the microsoft.com domain name and has been changed to use the aspnetcdn.com domain name.</span></span> <span data-ttu-id="c8ce2-157">Essa alteração foi feita para aumentar o desempenho porque, quando um navegador referenciou o domínio microsoft.com, ele enviaria quaisquer cookies desse domínio pela conexão com cada solicitação.</span><span class="sxs-lookup"><span data-stu-id="c8ce2-157">This change was made to increase performance because when a browser referenced the microsoft.com domain it would send any cookies from that domain across the wire with each request.</span></span> <span data-ttu-id="c8ce2-158">Ao renomear para um nome de domínio diferente de microsoft.com, o desempenho pode ser aumentado em até 25%.</span><span class="sxs-lookup"><span data-stu-id="c8ce2-158">By renaming to a domain name other than microsoft.com performance can be increased by as much to 25%.</span></span> <span data-ttu-id="c8ce2-159">Observe que o ajax.microsoft.com continuará a funcionar, mas o ajax.aspnetcdn.com é recomendado.</span><span class="sxs-lookup"><span data-stu-id="c8ce2-159">Note ajax.microsoft.com will continue to function but ajax.aspnetcdn.com is recommended.</span></span>

- <span data-ttu-id="c8ce2-160">Formato antigo: https://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="c8ce2-160">Old Format: https://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span></span>
- <span data-ttu-id="c8ce2-161">Novo formato: https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="c8ce2-161">New Format: https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span></span>

<a id="Visual_Studio_vsdoc_Support_19"></a>

## <a name="visual-studio-vsdoc-support"></a><span data-ttu-id="c8ce2-162">Suporte ao Visual Studio. vsdoc</span><span class="sxs-lookup"><span data-stu-id="c8ce2-162">Visual Studio .vsdoc Support</span></span>

<span data-ttu-id="c8ce2-163">Para usar os arquivos. vsdoc corretamente com o Visual Studio 2008, você precisa certificar-se de que tem o VS 2008 SP1 instalado e o hotfix para arquivos vsdoc instalados.</span><span class="sxs-lookup"><span data-stu-id="c8ce2-163">To use the .vsdoc files properly with Visual Studio 2008 you need to make sure that you have VS 2008 SP1 installed and the hotfix for vsdoc files installed.</span></span> <span data-ttu-id="c8ce2-164">Você pode obtê-los aqui:</span><span class="sxs-lookup"><span data-stu-id="c8ce2-164">You can get these from here:</span></span>

- [<span data-ttu-id="c8ce2-165">Baixe o Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="c8ce2-165">Download Visual Studio 2008 SP1</span></span>](https://www.microsoft.com/downloads/en/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en "Baixe o Visual Studio 2008 SP1")
- [<span data-ttu-id="c8ce2-166">Baixe o hotfix. vsdoc para o Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="c8ce2-166">Download .vsdoc hotfix for Visual Studio 2008 SP1</span></span>](https://code.msdn.microsoft.com/KB958502/Release/ProjectReleases.aspx?ReleaseId=1736 "Baixe o hotfix. vsdoc para o Visual Studio 2008 SP1")

<span data-ttu-id="c8ce2-167">O Visual Studio 2010 dá suporte a arquivos. vsdoc sem patches adicionais.</span><span class="sxs-lookup"><span data-stu-id="c8ce2-167">Visual Studio 2010 supports .vsdoc files without any additional patches.</span></span>

<a id="Using_ASPNET_Ajax_from_the_CDN_20"></a>

## <a name="using-aspnet-ajax-from-the-cdn"></a><span data-ttu-id="c8ce2-168">Usando o ASP.NET AJAX da CDN</span><span class="sxs-lookup"><span data-stu-id="c8ce2-168">Using ASP.NET Ajax from the CDN</span></span>

<span data-ttu-id="c8ce2-169">Ao usar o ASP.NET 4, você pode redirecionar todas as solicitações de scripts do ASP.NET Framework para a CDN.</span><span class="sxs-lookup"><span data-stu-id="c8ce2-169">When using ASP.NET 4, you can redirect all requests for ASP.NET framework scripts to the CDN.</span></span> <span data-ttu-id="c8ce2-170">Recuperar scripts da CDN em vez de seu servidor Web local pode melhorar substancialmente o desempenho de sites públicos do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c8ce2-170">Retrieving scripts from the CDN instead of your local web server can substantially improve the performance of public ASP.NET websites.</span></span>

<span data-ttu-id="c8ce2-171">Use a propriedade EnableCDN do ScriptManager para redirecionar todas as solicitações de script do ASP.NET Framework para a CDN do Microsoft AJAX:</span><span class="sxs-lookup"><span data-stu-id="c8ce2-171">Use the ScriptManager EnableCDN property to redirect all ASP.NET framework script requests to the Microsoft Ajax CDN:</span></span>

[!code-aspx[Main](overview/samples/sample1.aspx)]

<a id="Using_jQuery_from_the_CDN_21"></a>

## <a name="using-jquery-from-the-cdn"></a><span data-ttu-id="c8ce2-172">Usando o jQuery da CDN</span><span class="sxs-lookup"><span data-stu-id="c8ce2-172">Using jQuery from the CDN</span></span>

<span data-ttu-id="c8ce2-173">Você pode usar scripts jQuery hospedados na CDN em seu aplicativo Web adicionando o seguinte elemento script a uma página:</span><span class="sxs-lookup"><span data-stu-id="c8ce2-173">You can use jQuery scripts hosted on CDN in your Web application by adding the following script element to a page:</span></span>

[!code-html[Main](overview/samples/sample2.html)]

<span data-ttu-id="c8ce2-174">A CDN também inclui a versão reduzidos do script jQuery, que você pode obter usando o seguinte elemento:</span><span class="sxs-lookup"><span data-stu-id="c8ce2-174">The CDN also includes the minified version of the jQuery script, which you can get using the following element:</span></span>

[!code-html[Main](overview/samples/sample3.html)]

<span data-ttu-id="c8ce2-175">Para permitir que sua página faça fallback para o carregamento do jQuery de um caminho local em seu próprio site se a CDN não estiver disponível, adicione o seguinte elemento imediatamente após o elemento que faz referência à CDN:</span><span class="sxs-lookup"><span data-stu-id="c8ce2-175">To allow your page to fallback to loading jQuery from a local path on your own website if the CDN happens to be unavailable, add the following element immediately after the element referencing the CDN:</span></span>

[!code-html[Main](overview/samples/sample4.html)]

<span data-ttu-id="c8ce2-176">A página de exemplo a seguir usa a versão CDN da biblioteca jQuery (com fallback para uma cópia local) para exibir o conteúdo de um elemento div quando um botão é clicado.</span><span class="sxs-lookup"><span data-stu-id="c8ce2-176">The following sample page uses the CDN version of the jQuery library (with fallback to a local copy) to display the contents of a div element when a button is clicked.</span></span>

[!code-html[Main](overview/samples/sample5.html)]

<span data-ttu-id="c8ce2-177">Você pode saber mais sobre o jQuery e baixar uma cópia local do jQuery visitando o site do [jQuery](http://jquery.com/) .</span><span class="sxs-lookup"><span data-stu-id="c8ce2-177">You can learn more about jQuery and download a local copy of jQuery by visiting the [jQuery](http://jquery.com/) Web site.</span></span>

<a id="Using_jQuery_UI_from_the_CDN_22"></a>

## <a name="using-jquery-ui-from-the-cdn"></a><span data-ttu-id="c8ce2-178">Usando a interface do usuário do jQuery da CDN</span><span class="sxs-lookup"><span data-stu-id="c8ce2-178">Using jQuery UI from the CDN</span></span>

<span data-ttu-id="c8ce2-179">A CDN também hospeda a biblioteca da interface do usuário do jQuery.</span><span class="sxs-lookup"><span data-stu-id="c8ce2-179">The CDN also hosts the jQuery UI library.</span></span> <span data-ttu-id="c8ce2-180">A biblioteca da interface do usuário do jQuery inclui um conjunto avançado de widgets e efeitos que você pode usar em seus aplicativos ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c8ce2-180">The jQuery UI library includes a rich set of widgets and effects that you can use in your ASP.NET applications.</span></span> <span data-ttu-id="c8ce2-181">Por exemplo, a página a seguir ilustra como você pode usar a interface do usuário do jQuery DatePicker no contexto de um aplicativo de Web Forms de ASP.NET para exibir um calendário pop-up:</span><span class="sxs-lookup"><span data-stu-id="c8ce2-181">For example, the following page illustrates how you can use the jQuery UI Datepicker in the context of an ASP.NET Web Forms application to display a pop-up calendar:</span></span>

[!code-aspx[Main](overview/samples/sample6.aspx)]

<span data-ttu-id="c8ce2-182">Quando você move o foco para a caixa de texto usando o teclado, é exibido um calendário:</span><span class="sxs-lookup"><span data-stu-id="c8ce2-182">When you move focus to the TextBox using your keyboard, a calendar is displayed:</span></span>

![Calendário pop-up criado com DatePicker](overview/_static/image1.png)

<span data-ttu-id="c8ce2-184">Observe que você deve incluir três arquivos da CDN no código acima:</span><span class="sxs-lookup"><span data-stu-id="c8ce2-184">Notice that you must include three files from the CDN in the code above:</span></span>

- <span data-ttu-id="c8ce2-185">A biblioteca jQuery &mdash; da biblioteca de interface do usuário do jQuery depende da biblioteca jQuery.</span><span class="sxs-lookup"><span data-stu-id="c8ce2-185">The jQuery library &mdash; The jQuery UI library depends on the jQuery library.</span></span> <span data-ttu-id="c8ce2-186">Você deve adicionar a biblioteca jQuery à sua página antes de adicionar a biblioteca da interface do usuário do jQuery.</span><span class="sxs-lookup"><span data-stu-id="c8ce2-186">You must add the jQuery library to your page before you add the jQuery UI library.</span></span>
- <span data-ttu-id="c8ce2-187">A biblioteca da interface do usuário do jQuery &mdash; a biblioteca da interface do usuário do jQuery contém todos os efeitos e widgets da interface do usuário do jQuery, como o widget DatePicker usado na página acima.</span><span class="sxs-lookup"><span data-stu-id="c8ce2-187">The jQuery UI library &mdash; The jQuery UI library contains all of the jQuery UI effects and widgets such as the Datepicker widget used in the page above.</span></span>
- <span data-ttu-id="c8ce2-188">Um tema da IU do jQuery &mdash; que a interface do usuário do jQuery dá suporte a diferentes temas.</span><span class="sxs-lookup"><span data-stu-id="c8ce2-188">A jQuery UI theme &mdash; The jQuery UI supports different themes.</span></span> <span data-ttu-id="c8ce2-189">A página acima inclui um link para um arquivo CSS para importar o tema Redmond.</span><span class="sxs-lookup"><span data-stu-id="c8ce2-189">The page above includes a link to a CSS file to import the Redmond theme.</span></span>

<span data-ttu-id="c8ce2-190">Todos os temas padrão da interface do usuário do jQuery são hospedados na CDN.</span><span class="sxs-lookup"><span data-stu-id="c8ce2-190">All of the standard jQuery UI themes are hosted on the CDN.</span></span> <span data-ttu-id="c8ce2-191">[Visite esta página](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 na CDN do Microsoft Ajax") para exibir miniaturas de cada tema.</span><span class="sxs-lookup"><span data-stu-id="c8ce2-191">[Visit this page](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 on the Microsoft Ajax CDN") to view thumbnails for each theme.</span></span>

<span data-ttu-id="c8ce2-192">Para saber mais sobre a biblioteca de interface do usuário do jQuery, visite o [site oficial da interface do usuário do jQuery](http://jQueryUI.com "site da interface do usuário do jQuery").</span><span class="sxs-lookup"><span data-stu-id="c8ce2-192">To learn more about the jQuery UI library, visit the official [jQuery UI website](http://jQueryUI.com "jQuery UI website").</span></span>

<a id="Third-Party_Files_on_the_CDN_23"></a>

## <a name="third-party-files-on-the-cdn"></a><span data-ttu-id="c8ce2-193">Arquivos de terceiros na CDN</span><span class="sxs-lookup"><span data-stu-id="c8ce2-193">Third-Party Files on the CDN</span></span>

<span data-ttu-id="c8ce2-194">A CDN hospeda algumas das mais populares bibliotecas JavaScript de terceiros.</span><span class="sxs-lookup"><span data-stu-id="c8ce2-194">The CDN hosts some of the most popular third party JavaScript libraries.</span></span> <span data-ttu-id="c8ce2-195">A Microsoft não alega a propriedade de nenhuma biblioteca de terceiros hospedada nessa CDN.</span><span class="sxs-lookup"><span data-stu-id="c8ce2-195">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="c8ce2-196">Os proprietários de direitos autorais das bibliotecas estão licenciando essas bibliotecas para você.</span><span class="sxs-lookup"><span data-stu-id="c8ce2-196">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="c8ce2-197">Quaisquer direitos que você precise baixar e usar essas bibliotecas são concedidos exclusivamente pelos respectivos proprietários de direitos autorais.</span><span class="sxs-lookup"><span data-stu-id="c8ce2-197">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="c8ce2-198">Como essas não são bibliotecas da Microsoft, a Microsoft não fornece nenhuma garantia ou licença de direitos de propriedade intelectual (incluindo nenhum direito de patente implícito) para as bibliotecas de terceiros hospedadas nessa CDN.</span><span class="sxs-lookup"><span data-stu-id="c8ce2-198">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<a id="jQuery_Releases_on_the_CDN_0"></a>

### <a name="jquery-releases-on-the-cdn"></a><span data-ttu-id="c8ce2-199">Versões do jQuery na CDN</span><span class="sxs-lookup"><span data-stu-id="c8ce2-199">jQuery Releases on the CDN</span></span>

<span data-ttu-id="c8ce2-200">As seguintes versões do jQuery são hospedadas na CDN:</span><span class="sxs-lookup"><span data-stu-id="c8ce2-200">The following releases of jQuery are hosted on the CDN:</span></span>

#### <a name="jquery-version-360"></a><span data-ttu-id="c8ce2-201">jQuery versão 3.6.0</span><span class="sxs-lookup"><span data-stu-id="c8ce2-201">jQuery version 3.6.0</span></span>
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.6.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.6.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.6.0.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.6.0.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.6.0.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.6.0.slim.min.map

#### <a name="jquery-version-351"></a><span data-ttu-id="c8ce2-202">jQuery versão 3.5.1</span><span class="sxs-lookup"><span data-stu-id="c8ce2-202">jQuery version 3.5.1</span></span>
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.5.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.5.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.5.1.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.5.1.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.5.1.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.5.1.slim.min.map

#### <a name="jquery-version-350"></a><span data-ttu-id="c8ce2-203">jQuery versão 3.5.0</span><span class="sxs-lookup"><span data-stu-id="c8ce2-203">jQuery version 3.5.0</span></span>
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.5.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.5.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.5.0.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.5.0.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.5.0.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.5.0.slim.min.map

#### <a name="jquery-version-341"></a><span data-ttu-id="c8ce2-204">jQuery versão 3.4.1</span><span class="sxs-lookup"><span data-stu-id="c8ce2-204">jQuery version 3.4.1</span></span>
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.1.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.1.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.1.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.1.slim.min.map

#### <a name="jquery-version-340"></a><span data-ttu-id="c8ce2-205">jQuery versão 3.4.0</span><span class="sxs-lookup"><span data-stu-id="c8ce2-205">jQuery version 3.4.0</span></span>
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.0.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.0.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.0.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.0.slim.min.map

#### <a name="jquery-version-331"></a><span data-ttu-id="c8ce2-206">jQuery versão 3.3.1</span><span class="sxs-lookup"><span data-stu-id="c8ce2-206">jQuery version 3.3.1</span></span>
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.map

#### <a name="jquery-version-321"></a><span data-ttu-id="c8ce2-207">jQuery versão 3.2.1</span><span class="sxs-lookup"><span data-stu-id="c8ce2-207">jQuery version 3.2.1</span></span>
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.map

#### <a name="jquery-version-320"></a><span data-ttu-id="c8ce2-208">jQuery versão 3.2.0</span><span class="sxs-lookup"><span data-stu-id="c8ce2-208">jQuery version 3.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.map

#### <a name="jquery-version-311"></a><span data-ttu-id="c8ce2-209">jQuery versão 3.1.1</span><span class="sxs-lookup"><span data-stu-id="c8ce2-209">jQuery version 3.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.map

#### <a name="jquery-version-310"></a><span data-ttu-id="c8ce2-210">jQuery versão 3.1.0</span><span class="sxs-lookup"><span data-stu-id="c8ce2-210">jQuery version 3.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.map

#### <a name="jquery-version-300"></a><span data-ttu-id="c8ce2-211">jQuery versão 3.0.0</span><span class="sxs-lookup"><span data-stu-id="c8ce2-211">jQuery version 3.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.map

#### <a name="jquery-version-224"></a><span data-ttu-id="c8ce2-212">jQuery versão 2.2.4</span><span class="sxs-lookup"><span data-stu-id="c8ce2-212">jQuery version 2.2.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.map

#### <a name="jquery-version-223"></a><span data-ttu-id="c8ce2-213">jQuery versão 2.2.3</span><span class="sxs-lookup"><span data-stu-id="c8ce2-213">jQuery version 2.2.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.map

#### <a name="jquery-version-222"></a><span data-ttu-id="c8ce2-214">jQuery versão 2.2.2</span><span class="sxs-lookup"><span data-stu-id="c8ce2-214">jQuery version 2.2.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.map

#### <a name="jquery-version-221"></a><span data-ttu-id="c8ce2-215">jQuery versão 2.2.1</span><span class="sxs-lookup"><span data-stu-id="c8ce2-215">jQuery version 2.2.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.map

#### <a name="jquery-version-220"></a><span data-ttu-id="c8ce2-216">jQuery versão 2.2.0</span><span class="sxs-lookup"><span data-stu-id="c8ce2-216">jQuery version 2.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.map

#### <a name="jquery-version-214"></a><span data-ttu-id="c8ce2-217">jQuery versão 2.1.4</span><span class="sxs-lookup"><span data-stu-id="c8ce2-217">jQuery version 2.1.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.map

#### <a name="jquery-version-213"></a><span data-ttu-id="c8ce2-218">jQuery versão 2.1.3</span><span class="sxs-lookup"><span data-stu-id="c8ce2-218">jQuery version 2.1.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.map

#### <a name="jquery-version-212"></a><span data-ttu-id="c8ce2-219">jQuery versão 2.1.2</span><span class="sxs-lookup"><span data-stu-id="c8ce2-219">jQuery version 2.1.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.min.js

#### <a name="jquery-version-211"></a><span data-ttu-id="c8ce2-220">jQuery versão 2.1.1</span><span class="sxs-lookup"><span data-stu-id="c8ce2-220">jQuery version 2.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.map

#### <a name="jquery-version-210"></a><span data-ttu-id="c8ce2-221">jQuery versão 2.1.0</span><span class="sxs-lookup"><span data-stu-id="c8ce2-221">jQuery version 2.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.map

#### <a name="jquery-version-203"></a><span data-ttu-id="c8ce2-222">jQuery versão 2.0.3</span><span class="sxs-lookup"><span data-stu-id="c8ce2-222">jQuery version 2.0.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.map

#### <a name="jquery-version-202"></a><span data-ttu-id="c8ce2-223">jQuery versão 2.0.2</span><span class="sxs-lookup"><span data-stu-id="c8ce2-223">jQuery version 2.0.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.map

#### <a name="jquery-version-201"></a><span data-ttu-id="c8ce2-224">jQuery versão 2.0.1</span><span class="sxs-lookup"><span data-stu-id="c8ce2-224">jQuery version 2.0.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.map

#### <a name="jquery-version-200"></a><span data-ttu-id="c8ce2-225">jQuery versão 2.0.0</span><span class="sxs-lookup"><span data-stu-id="c8ce2-225">jQuery version 2.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.map

#### <a name="jquery-version-1124"></a><span data-ttu-id="c8ce2-226">jQuery versão 1.12.4</span><span class="sxs-lookup"><span data-stu-id="c8ce2-226">jQuery version 1.12.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.map

#### <a name="jquery-version-1123"></a><span data-ttu-id="c8ce2-227">jQuery versão 1.12.3</span><span class="sxs-lookup"><span data-stu-id="c8ce2-227">jQuery version 1.12.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.map

#### <a name="jquery-version-1122"></a><span data-ttu-id="c8ce2-228">jQuery versão 1.12.2</span><span class="sxs-lookup"><span data-stu-id="c8ce2-228">jQuery version 1.12.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.map

#### <a name="jquery-version-1121"></a><span data-ttu-id="c8ce2-229">jQuery versão 1.12.1</span><span class="sxs-lookup"><span data-stu-id="c8ce2-229">jQuery version 1.12.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.map

#### <a name="jquery-version-1120"></a><span data-ttu-id="c8ce2-230">jQuery versão 1.12.0</span><span class="sxs-lookup"><span data-stu-id="c8ce2-230">jQuery version 1.12.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.map

#### <a name="jquery-version-1113"></a><span data-ttu-id="c8ce2-231">jQuery versão 1.11.3</span><span class="sxs-lookup"><span data-stu-id="c8ce2-231">jQuery version 1.11.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.map

#### <a name="jquery-version-1112"></a><span data-ttu-id="c8ce2-232">jQuery versão 1.11.2</span><span class="sxs-lookup"><span data-stu-id="c8ce2-232">jQuery version 1.11.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.map

#### <a name="jquery-version-1111"></a><span data-ttu-id="c8ce2-233">jQuery versão 1.11.1</span><span class="sxs-lookup"><span data-stu-id="c8ce2-233">jQuery version 1.11.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.map

#### <a name="jquery-version-1110"></a><span data-ttu-id="c8ce2-234">jQuery versão 1.11.0</span><span class="sxs-lookup"><span data-stu-id="c8ce2-234">jQuery version 1.11.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.map

#### <a name="jquery-version-1102"></a><span data-ttu-id="c8ce2-235">jQuery versão 1.10.2</span><span class="sxs-lookup"><span data-stu-id="c8ce2-235">jQuery version 1.10.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.map

#### <a name="jquery-version-1101"></a><span data-ttu-id="c8ce2-236">jQuery versão 1.10.1</span><span class="sxs-lookup"><span data-stu-id="c8ce2-236">jQuery version 1.10.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.map

#### <a name="jquery-version-1100"></a><span data-ttu-id="c8ce2-237">jQuery versão 1.10.0</span><span class="sxs-lookup"><span data-stu-id="c8ce2-237">jQuery version 1.10.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.map

#### <a name="jquery-version-191"></a><span data-ttu-id="c8ce2-238">jQuery versão 1.9.1</span><span class="sxs-lookup"><span data-stu-id="c8ce2-238">jQuery version 1.9.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.map

#### <a name="jquery-version-190"></a><span data-ttu-id="c8ce2-239">jQuery versão 1.9.0</span><span class="sxs-lookup"><span data-stu-id="c8ce2-239">jQuery version 1.9.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.map

#### <a name="jquery-version-183"></a><span data-ttu-id="c8ce2-240">jQuery versão 1.8.3</span><span class="sxs-lookup"><span data-stu-id="c8ce2-240">jQuery version 1.8.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3-vsdoc.js

#### <a name="jquery-version-182"></a><span data-ttu-id="c8ce2-241">jQuery versão 1.8.2</span><span class="sxs-lookup"><span data-stu-id="c8ce2-241">jQuery version 1.8.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2-vsdoc.js

#### <a name="jquery-version-181"></a><span data-ttu-id="c8ce2-242">jQuery versão 1.8.1</span><span class="sxs-lookup"><span data-stu-id="c8ce2-242">jQuery version 1.8.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1-vsdoc.js

#### <a name="jquery-version-180"></a><span data-ttu-id="c8ce2-243">jQuery versão 1.8.0</span><span class="sxs-lookup"><span data-stu-id="c8ce2-243">jQuery version 1.8.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0-vsdoc.js

#### <a name="jquery-version-172"></a><span data-ttu-id="c8ce2-244">jQuery versão 1.7.2</span><span class="sxs-lookup"><span data-stu-id="c8ce2-244">jQuery version 1.7.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.min.js

#### <a name="jquery-version-171"></a><span data-ttu-id="c8ce2-245">jQuery versão 1.7.1</span><span class="sxs-lookup"><span data-stu-id="c8ce2-245">jQuery version 1.7.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1-vsdoc.js

#### <a name="jquery-version-17"></a><span data-ttu-id="c8ce2-246">jQuery versão 1,7</span><span class="sxs-lookup"><span data-stu-id="c8ce2-246">jQuery version 1.7</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7-vsdoc.js

#### <a name="jquery-version-164"></a><span data-ttu-id="c8ce2-247">jQuery versão 1.6.4</span><span class="sxs-lookup"><span data-stu-id="c8ce2-247">jQuery version 1.6.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4-vsdoc.js

#### <a name="jquery-version-163"></a><span data-ttu-id="c8ce2-248">jQuery versão 1.6.3</span><span class="sxs-lookup"><span data-stu-id="c8ce2-248">jQuery version 1.6.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3-vsdoc.js

#### <a name="jquery-version-162"></a><span data-ttu-id="c8ce2-249">jQuery versão 1.6.2</span><span class="sxs-lookup"><span data-stu-id="c8ce2-249">jQuery version 1.6.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2-vsdoc.js

#### <a name="jquery-version-161"></a><span data-ttu-id="c8ce2-250">jQuery versão 1.6.1</span><span class="sxs-lookup"><span data-stu-id="c8ce2-250">jQuery version 1.6.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1-vsdoc.js

#### <a name="jquery-version-16"></a><span data-ttu-id="c8ce2-251">jQuery versão 1,6</span><span class="sxs-lookup"><span data-stu-id="c8ce2-251">jQuery version 1.6</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6-vsdoc.js

#### <a name="jquery-version-152"></a><span data-ttu-id="c8ce2-252">jQuery versão 1.5.2</span><span class="sxs-lookup"><span data-stu-id="c8ce2-252">jQuery version 1.5.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2-vsdoc.js

#### <a name="jquery-version-151"></a><span data-ttu-id="c8ce2-253">jQuery versão 1.5.1</span><span class="sxs-lookup"><span data-stu-id="c8ce2-253">jQuery version 1.5.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1-vsdoc.js

#### <a name="jquery-version-15"></a><span data-ttu-id="c8ce2-254">jQuery versão 1,5</span><span class="sxs-lookup"><span data-stu-id="c8ce2-254">jQuery version 1.5</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5-vsdoc.js

#### <a name="jquery-version-144"></a><span data-ttu-id="c8ce2-255">jQuery versão 1.4.4</span><span class="sxs-lookup"><span data-stu-id="c8ce2-255">jQuery version 1.4.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4-vsdoc.js

#### <a name="jquery-version-143"></a><span data-ttu-id="c8ce2-256">jQuery versão 1.4.3</span><span class="sxs-lookup"><span data-stu-id="c8ce2-256">jQuery version 1.4.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3-vsdoc.js

#### <a name="jquery-version-142"></a><span data-ttu-id="c8ce2-257">jQuery versão 1.4.2</span><span class="sxs-lookup"><span data-stu-id="c8ce2-257">jQuery version 1.4.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2-vsdoc.js

#### <a name="jquery-version-141"></a><span data-ttu-id="c8ce2-258">jQuery versão 1.4.1</span><span class="sxs-lookup"><span data-stu-id="c8ce2-258">jQuery version 1.4.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1-vsdoc.js

#### <a name="jquery-version-14"></a><span data-ttu-id="c8ce2-259">jQuery versão 1,4</span><span class="sxs-lookup"><span data-stu-id="c8ce2-259">jQuery version 1.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.min.js

#### <a name="jquery-version-132"></a><span data-ttu-id="c8ce2-260">jQuery versão 1.3.2</span><span class="sxs-lookup"><span data-stu-id="c8ce2-260">jQuery version 1.3.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min-vsdoc.js

<a id="jQuery_Migrate_Releases_on_the_CDN_1"></a>

### <a name="jquery-migrate-releases-on-the-cdn"></a><span data-ttu-id="c8ce2-261">Versões de migração do jQuery no CDN</span><span class="sxs-lookup"><span data-stu-id="c8ce2-261">jQuery Migrate Releases on the CDN</span></span>

<span data-ttu-id="c8ce2-262">As seguintes versões da migração do jQuery são hospedadas na CDN:</span><span class="sxs-lookup"><span data-stu-id="c8ce2-262">The following releases of jQuery Migrate are hosted on the CDN:</span></span>

#### <a name="jquery-migrate-version-300"></a><span data-ttu-id="c8ce2-263">jQuery migrar versão 3.0.0</span><span class="sxs-lookup"><span data-stu-id="c8ce2-263">jQuery Migrate version 3.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.min.js

#### <a name="jquery-migrate-version-121"></a><span data-ttu-id="c8ce2-264">Migrar versão 1.2.1 do jQuery</span><span class="sxs-lookup"><span data-stu-id="c8ce2-264">jQuery Migrate version 1.2.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.min.js

<span data-ttu-id="c8ce2-265">jQuery migrar versão 1.2.0</span><span class="sxs-lookup"><span data-stu-id="c8ce2-265">jQuery Migrate version 1.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.min.js

#### <a name="jquery-migrate-version-111"></a><span data-ttu-id="c8ce2-266">Migrar do jQuery versão 1.1.1</span><span class="sxs-lookup"><span data-stu-id="c8ce2-266">jQuery Migrate version 1.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.min.js

#### <a name="jquery-migrate-version-110"></a><span data-ttu-id="c8ce2-267">Migrar do jQuery versão 1.1.0</span><span class="sxs-lookup"><span data-stu-id="c8ce2-267">jQuery Migrate version 1.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.min.js

#### <a name="jquery-migrate-version-100"></a><span data-ttu-id="c8ce2-268">Migrar do jQuery versão 1.0.0</span><span class="sxs-lookup"><span data-stu-id="c8ce2-268">jQuery Migrate version 1.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.min.js

<a id="jQuery_UI_Releases_on_the_CDN_2"></a>

### <a name="jquery-ui-releases-on-the-cdn"></a><span data-ttu-id="c8ce2-269">Versões da interface do usuário do jQuery na CDN</span><span class="sxs-lookup"><span data-stu-id="c8ce2-269">jQuery UI Releases on the CDN</span></span>

<span data-ttu-id="c8ce2-270">As seguintes versões da biblioteca da interface do usuário do jQuery são hospedadas nesta CDN.</span><span class="sxs-lookup"><span data-stu-id="c8ce2-270">The following releases of the jQuery UI library are hosted on this CDN.</span></span> <span data-ttu-id="c8ce2-271">Clique em cada link para ver a lista real de arquivos.</span><span class="sxs-lookup"><span data-stu-id="c8ce2-271">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="c8ce2-272">1.12.1 da IU do jQuery</span><span class="sxs-lookup"><span data-stu-id="c8ce2-272">jQuery UI 1.12.1</span></span>](jquery-ui/cdnjqueryui1121.md "jQuery UI 1.12.1 na CDN do Microsoft Ajax")
- [<span data-ttu-id="c8ce2-273">1.12.0 da IU do jQuery</span><span class="sxs-lookup"><span data-stu-id="c8ce2-273">jQuery UI 1.12.0</span></span>](jquery-ui/cdnjqueryui1120.md "jQuery UI 1.12.0 na CDN do Microsoft Ajax")
- [<span data-ttu-id="c8ce2-274">1.11.4 da IU do jQuery</span><span class="sxs-lookup"><span data-stu-id="c8ce2-274">jQuery UI 1.11.4</span></span>](jquery-ui/cdnjqueryui1114.md "jQuery UI 1.11.4 na CDN do Microsoft Ajax")
- [<span data-ttu-id="c8ce2-275">1.11.3 da IU do jQuery</span><span class="sxs-lookup"><span data-stu-id="c8ce2-275">jQuery UI 1.11.3</span></span>](jquery-ui/cdnjqueryui1113.md "jQuery UI 1.11.3 na CDN do Microsoft Ajax")
- [<span data-ttu-id="c8ce2-276">1.11.2 da IU do jQuery</span><span class="sxs-lookup"><span data-stu-id="c8ce2-276">jQuery UI 1.11.2</span></span>](jquery-ui/cdnjqueryui1112.md "jQuery UI 1.11.2 na CDN do Microsoft Ajax")
- [<span data-ttu-id="c8ce2-277">1.11.1 da IU do jQuery</span><span class="sxs-lookup"><span data-stu-id="c8ce2-277">jQuery UI 1.11.1</span></span>](jquery-ui/cdnjqueryui1111.md "jQuery UI 1.11.1 na CDN do Microsoft Ajax")
- [<span data-ttu-id="c8ce2-278">1.11.0 da IU do jQuery</span><span class="sxs-lookup"><span data-stu-id="c8ce2-278">jQuery UI 1.11.0</span></span>](jquery-ui/cdnjqueryui1110.md "jQuery UI 1.11.0 na CDN do Microsoft Ajax")
- [<span data-ttu-id="c8ce2-279">1.10.4 da IU do jQuery</span><span class="sxs-lookup"><span data-stu-id="c8ce2-279">jQuery UI 1.10.4</span></span>](jquery-ui/cdnjqueryui1104.md "jQuery UI 1.10.4 na CDN do Microsoft Ajax")
- [<span data-ttu-id="c8ce2-280">1.10.3 da IU do jQuery</span><span class="sxs-lookup"><span data-stu-id="c8ce2-280">jQuery UI 1.10.3</span></span>](jquery-ui/cdnjqueryui1103.md "jQuery UI 1.10.3 na CDN do Microsoft Ajax")
- [<span data-ttu-id="c8ce2-281">1.10.2 da IU do jQuery</span><span class="sxs-lookup"><span data-stu-id="c8ce2-281">jQuery UI 1.10.2</span></span>](jquery-ui/cdnjqueryui1102.md "jQuery UI 1.10.2 na CDN do Microsoft Ajax")
- [<span data-ttu-id="c8ce2-282">1.10.1 da IU do jQuery</span><span class="sxs-lookup"><span data-stu-id="c8ce2-282">jQuery UI 1.10.1</span></span>](jquery-ui/cdnjqueryui1101.md "jQuery UI 1.10.1 na CDN do Microsoft Ajax")
- [<span data-ttu-id="c8ce2-283">1.10.0 da IU do jQuery</span><span class="sxs-lookup"><span data-stu-id="c8ce2-283">jQuery UI 1.10.0</span></span>](jquery-ui/cdnjqueryui1100.md "jQuery UI 1.10.0 na CDN do Microsoft Ajax")
- [<span data-ttu-id="c8ce2-284">1.9.2 da IU do jQuery</span><span class="sxs-lookup"><span data-stu-id="c8ce2-284">jQuery UI 1.9.2</span></span>](jquery-ui/cdnjqueryui192.md "jQuery UI 1.9.2 na CDN do Microsoft Ajax")
- [<span data-ttu-id="c8ce2-285">1.9.1 da IU do jQuery</span><span class="sxs-lookup"><span data-stu-id="c8ce2-285">jQuery UI 1.9.1</span></span>](jquery-ui/cdnjqueryui191.md "jQuery UI 1.9.1 na CDN do Microsoft Ajax")
- [<span data-ttu-id="c8ce2-286">1.9.0 da IU do jQuery</span><span class="sxs-lookup"><span data-stu-id="c8ce2-286">jQuery UI 1.9.0</span></span>](jquery-ui/cdnjqueryui190.md "jQuery UI 1.9.0 na CDN do Microsoft Ajax")
- [<span data-ttu-id="c8ce2-287">1.8.24 da IU do jQuery</span><span class="sxs-lookup"><span data-stu-id="c8ce2-287">jQuery UI 1.8.24</span></span>](jquery-ui/cdnjqueryui1824.md "jQuery UI 1.8.24 na CDN do Microsoft Ajax")
- [<span data-ttu-id="c8ce2-288">1.8.23 da IU do jQuery</span><span class="sxs-lookup"><span data-stu-id="c8ce2-288">jQuery UI 1.8.23</span></span>](jquery-ui/cdnjqueryui1823.md "jQuery UI 1.8.23 na CDN do Microsoft Ajax")
- [<span data-ttu-id="c8ce2-289">1.8.22 da IU do jQuery</span><span class="sxs-lookup"><span data-stu-id="c8ce2-289">jQuery UI 1.8.22</span></span>](jquery-ui/cdnjqueryui1822.md "jQuery UI 1.8.22 na CDN do Microsoft Ajax")
- [<span data-ttu-id="c8ce2-290">1.8.21 da IU do jQuery</span><span class="sxs-lookup"><span data-stu-id="c8ce2-290">jQuery UI 1.8.21</span></span>](jquery-ui/cdnjqueryui1821.md "jQuery UI 1.8.21 na CDN do Microsoft Ajax")
- [<span data-ttu-id="c8ce2-291">1.8.20 da IU do jQuery</span><span class="sxs-lookup"><span data-stu-id="c8ce2-291">jQuery UI 1.8.20</span></span>](jquery-ui/cdnjqueryui1820.md "jQuery UI 1.8.20 na CDN do Microsoft Ajax")
- [<span data-ttu-id="c8ce2-292">1.8.19 da IU do jQuery</span><span class="sxs-lookup"><span data-stu-id="c8ce2-292">jQuery UI 1.8.19</span></span>](jquery-ui/cdnjqueryui1819.md "jQuery UI 1.8.19 na CDN do Microsoft Ajax")
- [<span data-ttu-id="c8ce2-293">1.8.18 da IU do jQuery</span><span class="sxs-lookup"><span data-stu-id="c8ce2-293">jQuery UI 1.8.18</span></span>](jquery-ui/cdnjqueryui1818.md "jQuery UI 1.8.18 na CDN do Microsoft Ajax")
- [<span data-ttu-id="c8ce2-294">1.8.17 da IU do jQuery</span><span class="sxs-lookup"><span data-stu-id="c8ce2-294">jQuery UI 1.8.17</span></span>](jquery-ui/cdnjqueryui1817.md "jQuery UI 1.8.17 na CDN do Microsoft Ajax")
- [<span data-ttu-id="c8ce2-295">1.8.16 da IU do jQuery</span><span class="sxs-lookup"><span data-stu-id="c8ce2-295">jQuery UI 1.8.16</span></span>](jquery-ui/cdnjqueryui1816.md "jQuery UI 1.8.16 na CDN do Microsoft Ajax")
- [<span data-ttu-id="c8ce2-296">1.8.15 da IU do jQuery</span><span class="sxs-lookup"><span data-stu-id="c8ce2-296">jQuery UI 1.8.15</span></span>](jquery-ui/cdnjqueryui1815.md "jQuery UI 1.8.15 na CDN do Microsoft Ajax")
- [<span data-ttu-id="c8ce2-297">1.8.14 da IU do jQuery</span><span class="sxs-lookup"><span data-stu-id="c8ce2-297">jQuery UI 1.8.14</span></span>](jquery-ui/cdnjqueryui1814.md "jQuery UI 1.8.14 na CDN do Microsoft Ajax")
- [<span data-ttu-id="c8ce2-298">1.8.13 da IU do jQuery</span><span class="sxs-lookup"><span data-stu-id="c8ce2-298">jQuery UI 1.8.13</span></span>](jquery-ui/cdnjqueryui1813.md "jQuery UI 1.8.13 na CDN do Microsoft Ajax")
- [<span data-ttu-id="c8ce2-299">1.8.12 da IU do jQuery</span><span class="sxs-lookup"><span data-stu-id="c8ce2-299">jQuery UI 1.8.12</span></span>](jquery-ui/cdnjqueryui1812.md "jQuery UI 1.8.12 na CDN do Microsoft Ajax")
- [<span data-ttu-id="c8ce2-300">1.8.11 da IU do jQuery</span><span class="sxs-lookup"><span data-stu-id="c8ce2-300">jQuery UI 1.8.11</span></span>](jquery-ui/cdnjqueryui1811.md "jQuery UI 1.8.11 na CDN do Microsoft Ajax")
- [<span data-ttu-id="c8ce2-301">1.8.10 da IU do jQuery</span><span class="sxs-lookup"><span data-stu-id="c8ce2-301">jQuery UI 1.8.10</span></span>](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 na CDN do Microsoft Ajax")
- [<span data-ttu-id="c8ce2-302">1.8.9 da IU do jQuery</span><span class="sxs-lookup"><span data-stu-id="c8ce2-302">jQuery UI 1.8.9</span></span>](jquery-ui/cdnjqueryui189.md "jQuery UI 1.8.9 na CDN do Microsoft Ajax")
- [<span data-ttu-id="c8ce2-303">1.8.8 da IU do jQuery</span><span class="sxs-lookup"><span data-stu-id="c8ce2-303">jQuery UI 1.8.8</span></span>](jquery-ui/cdnjqueryui188.md "jQuery UI 1.8.8 na CDN do Microsoft Ajax")
- [<span data-ttu-id="c8ce2-304">1.8.7 da IU do jQuery</span><span class="sxs-lookup"><span data-stu-id="c8ce2-304">jQuery UI 1.8.7</span></span>](jquery-ui/cdnjqueryui187.md "jQuery UI 1.8.7 na CDN do Microsoft Ajax")
- [<span data-ttu-id="c8ce2-305">1.8.6 da IU do jQuery</span><span class="sxs-lookup"><span data-stu-id="c8ce2-305">jQuery UI 1.8.6</span></span>](jquery-ui/cdnjqueryui186.md "jQuery UI 1.8.6 na CDN do Microsoft Ajax")
- [<span data-ttu-id="c8ce2-306">jQuery UI 1.8.5</span><span class="sxs-lookup"><span data-stu-id="c8ce2-306">jQuery UI 1.8.5</span></span>](jquery-ui/cdnjqueryui185.md "jQuery UI 1.8.5")

<a id="jQuery_Validation_Releases_on_the_CDN_3"></a>

### <a name="jquery-validation-releases-on-the-cdn"></a><span data-ttu-id="c8ce2-307">Versões de validação do jQuery na CDN</span><span class="sxs-lookup"><span data-stu-id="c8ce2-307">jQuery Validation Releases on the CDN</span></span>

<span data-ttu-id="c8ce2-308">As seguintes versões do plug-in de [validação jQuery](https://jqueryvalidation.org/ "Plug-in de validação jQuery") são hospedadas nessa CDN.</span><span class="sxs-lookup"><span data-stu-id="c8ce2-308">The following releases of the [jQuery Validation](https://jqueryvalidation.org/ "jQuery Validation Plugin") plugin are hosted on this CDN.</span></span> <span data-ttu-id="c8ce2-309">Clique em cada link para ver a lista real de arquivos.</span><span class="sxs-lookup"><span data-stu-id="c8ce2-309">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="c8ce2-310">1.19.2 de validação jQuery</span><span class="sxs-lookup"><span data-stu-id="c8ce2-310">jQuery Validate 1.19.2</span></span>](jquery-validate/cdnjqueryvalidate1192.md "1.19.2 de validação jQuery")
- [<span data-ttu-id="c8ce2-311">1.19.1 de validação jQuery</span><span class="sxs-lookup"><span data-stu-id="c8ce2-311">jQuery Validate 1.19.1</span></span>](jquery-validate/cdnjqueryvalidate1191.md "1.19.1 de validação jQuery")
- [<span data-ttu-id="c8ce2-312">1.19.0 de validação jQuery</span><span class="sxs-lookup"><span data-stu-id="c8ce2-312">jQuery Validate 1.19.0</span></span>](jquery-validate/cdnjqueryvalidate1190.md "1.19.0 de validação jQuery")
- [<span data-ttu-id="c8ce2-313">1.17.0 de validação jQuery</span><span class="sxs-lookup"><span data-stu-id="c8ce2-313">jQuery Validate 1.17.0</span></span>](jquery-validate/cdnjqueryvalidate1170.md "1.17.0 de validação jQuery")
- [<span data-ttu-id="c8ce2-314">1.16.0 de validação jQuery</span><span class="sxs-lookup"><span data-stu-id="c8ce2-314">jQuery Validate 1.16.0</span></span>](jquery-validate/cdnjqueryvalidate1160.md "Validação do jQuery 1.16.0")
- [<span data-ttu-id="c8ce2-315">1.15.1 de validação jQuery</span><span class="sxs-lookup"><span data-stu-id="c8ce2-315">jQuery Validate 1.15.1</span></span>](jquery-validate/cdnjqueryvalidate1151.md "Validação do jQuery 1.15.1")
- [<span data-ttu-id="c8ce2-316">1.15.0 de validação jQuery</span><span class="sxs-lookup"><span data-stu-id="c8ce2-316">jQuery Validate 1.15.0</span></span>](jquery-validate/cdnjqueryvalidate1150.md "Validação do jQuery 1.15.0")
- [<span data-ttu-id="c8ce2-317">1.14.0 de validação jQuery</span><span class="sxs-lookup"><span data-stu-id="c8ce2-317">jQuery Validate 1.14.0</span></span>](jquery-validate/cdnjqueryvalidate1140.md "Validação do jQuery 1.14.0")
- [<span data-ttu-id="c8ce2-318">1.13.1 de validação jQuery</span><span class="sxs-lookup"><span data-stu-id="c8ce2-318">jQuery Validate 1.13.1</span></span>](jquery-validate/cdnjqueryvalidate1131.md "Validação do jQuery 1.13.1")
- [<span data-ttu-id="c8ce2-319">1.13.0 de validação jQuery</span><span class="sxs-lookup"><span data-stu-id="c8ce2-319">jQuery Validate 1.13.0</span></span>](jquery-validate/cdnjqueryvalidate1130.md "Validação do jQuery 1.13.0")
- [<span data-ttu-id="c8ce2-320">1.12.0 de validação jQuery</span><span class="sxs-lookup"><span data-stu-id="c8ce2-320">jQuery Validate 1.12.0</span></span>](jquery-validate/cdnjqueryvalidate1120.md "Validação do jQuery 1.12.0")
- [<span data-ttu-id="c8ce2-321">1.11.1 de validação jQuery</span><span class="sxs-lookup"><span data-stu-id="c8ce2-321">jQuery Validate 1.11.1</span></span>](jquery-validate/cdnjqueryvalidate1111.md "Validação do jQuery 1.11.1")
- [<span data-ttu-id="c8ce2-322">1.11.0 de validação jQuery</span><span class="sxs-lookup"><span data-stu-id="c8ce2-322">jQuery Validate 1.11.0</span></span>](jquery-validate/cdnjqueryvalidate111.md "Validação do jQuery 1.11.0")
- [<span data-ttu-id="c8ce2-323">1.10.0 de validação jQuery</span><span class="sxs-lookup"><span data-stu-id="c8ce2-323">jQuery Validate 1.10.0</span></span>](jquery-validate/cdnjqueryvalidate110.md "Validação do jQuery 1.10.0")
- [<span data-ttu-id="c8ce2-324">jQuery validar 1,9</span><span class="sxs-lookup"><span data-stu-id="c8ce2-324">jQuery Validate 1.9</span></span>](jquery-validate/cdnjqueryvalidate19.md "jquery.validate versão 1.9")
- [<span data-ttu-id="c8ce2-325">1.8.1 de validação jQuery</span><span class="sxs-lookup"><span data-stu-id="c8ce2-325">jQuery Validate 1.8.1</span></span>](jquery-validate/cdnjqueryvalidate181.md "jquery.validate versão 1.8.1")
- [<span data-ttu-id="c8ce2-326">jQuery validar 1,8</span><span class="sxs-lookup"><span data-stu-id="c8ce2-326">jQuery Validate 1.8</span></span>](jquery-validate/cdnjqueryvalidate18.md "jquery.validate versão 1.8")
- [<span data-ttu-id="c8ce2-327">jQuery validar 1,7</span><span class="sxs-lookup"><span data-stu-id="c8ce2-327">jQuery Validate 1.7</span></span>](jquery-validate/cdnjqueryvalidate17.md "jquery.validate versão 1.7")
- [<span data-ttu-id="c8ce2-328">Validar jQuery 1.6</span><span class="sxs-lookup"><span data-stu-id="c8ce2-328">jQuery Validate 1.6</span></span>](jquery-validate/cdnjqueryvalidate16.md "Validar jQuery 1.6")
- [<span data-ttu-id="c8ce2-329">Validar jQuery 1.5.5</span><span class="sxs-lookup"><span data-stu-id="c8ce2-329">jQuery Validate 1.5.5</span></span>](jquery-validate/cdnjqueryvalidate155.md "Validar jQuery 1.5.5")

<a id="jQuery_Mobile_Releases_on_the_CDN_4"></a>

### <a name="jquery-mobile-releases-on-the-cdn"></a><span data-ttu-id="c8ce2-330">Versões móveis do jQuery no CDN</span><span class="sxs-lookup"><span data-stu-id="c8ce2-330">jQuery Mobile Releases on the CDN</span></span>

<span data-ttu-id="c8ce2-331">As seguintes versões da biblioteca do jQuery Mobile são hospedadas nessa CDN.</span><span class="sxs-lookup"><span data-stu-id="c8ce2-331">The following releases of the jQuery Mobile library are hosted on this CDN.</span></span> <span data-ttu-id="c8ce2-332">Clique em cada link para ver a lista real de arquivos.</span><span class="sxs-lookup"><span data-stu-id="c8ce2-332">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="c8ce2-333">jQuery Mobile 1.4.5</span><span class="sxs-lookup"><span data-stu-id="c8ce2-333">jQuery Mobile 1.4.5</span></span>](jquery-mobile/cdnjquerymobile145.md "jQuery Mobile 1.4.5 na CDN do Microsoft Ajax")
- [<span data-ttu-id="c8ce2-334">jQuery Mobile 1.4.2</span><span class="sxs-lookup"><span data-stu-id="c8ce2-334">jQuery Mobile 1.4.2</span></span>](jquery-mobile/cdnjquerymobile142.md "jQuery Mobile 1.4.2 na CDN do Microsoft Ajax")
- [<span data-ttu-id="c8ce2-335">jQuery Mobile 1.4.1</span><span class="sxs-lookup"><span data-stu-id="c8ce2-335">jQuery Mobile 1.4.1</span></span>](jquery-mobile/cdnjquerymobile141.md "jQuery Mobile 1.4.1 na CDN do Microsoft Ajax")
- [<span data-ttu-id="c8ce2-336">jQuery Mobile 1.4.0</span><span class="sxs-lookup"><span data-stu-id="c8ce2-336">jQuery Mobile 1.4.0</span></span>](jquery-mobile/cdnjquerymobile140.md "jQuery Mobile 1.4.0 na CDN do Microsoft Ajax")
- [<span data-ttu-id="c8ce2-337">jQuery Mobile 1.3.2</span><span class="sxs-lookup"><span data-stu-id="c8ce2-337">jQuery Mobile 1.3.2</span></span>](jquery-mobile/cdnjquerymobile132.md "jQuery Mobile 1.3.2 na CDN do Microsoft Ajax")
- [<span data-ttu-id="c8ce2-338">jQuery Mobile 1.3.1</span><span class="sxs-lookup"><span data-stu-id="c8ce2-338">jQuery Mobile 1.3.1</span></span>](jquery-mobile/cdnjquerymobile131.md "jQuery Mobile 1.3.1 na CDN do Microsoft Ajax")
- [<span data-ttu-id="c8ce2-339">jQuery Mobile 1.3.0</span><span class="sxs-lookup"><span data-stu-id="c8ce2-339">jQuery Mobile 1.3.0</span></span>](jquery-mobile/cdnjquerymobile130.md "jQuery Mobile 1.3.0 na CDN do Microsoft Ajax")
- [<span data-ttu-id="c8ce2-340">jQuery Mobile 1.2.0</span><span class="sxs-lookup"><span data-stu-id="c8ce2-340">jQuery Mobile 1.2.0</span></span>](jquery-mobile/cdnjquerymobile120.md "jQuery Mobile 1.2.0 na CDN do Microsoft Ajax")
- [<span data-ttu-id="c8ce2-341">jQuery Mobile 1.1.2</span><span class="sxs-lookup"><span data-stu-id="c8ce2-341">jQuery Mobile 1.1.2</span></span>](jquery-mobile/cdnjquerymobile112.md "jQuery Mobile 1.1.2 na CDN do Microsoft Ajax")
- [<span data-ttu-id="c8ce2-342">jQuery Mobile 1.1.1</span><span class="sxs-lookup"><span data-stu-id="c8ce2-342">jQuery Mobile 1.1.1</span></span>](jquery-mobile/cdnjquerymobile111.md "jQuery Mobile 1.1.1 na CDN do Microsoft Ajax")
- [<span data-ttu-id="c8ce2-343">jQuery Mobile 1.1.0</span><span class="sxs-lookup"><span data-stu-id="c8ce2-343">jQuery Mobile 1.1.0</span></span>](jquery-mobile/cdnjquerymobile110.md "jQuery Mobile 1.1.0 na CDN do Microsoft Ajax")
- [<span data-ttu-id="c8ce2-344">jQuery Mobile 1.1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="c8ce2-344">jQuery Mobile 1.1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile110rc2.md "jQuery Mobile 1.1.0 RC2 na CDN do Microsoft Ajax")
- [<span data-ttu-id="c8ce2-345">jQuery Mobile 1.0.1</span><span class="sxs-lookup"><span data-stu-id="c8ce2-345">jQuery Mobile 1.0.1</span></span>](jquery-mobile/cdnjquerymobile101.md "jQuery Mobile 1.0.1 na CDN do Microsoft Ajax")
- [<span data-ttu-id="c8ce2-346">jQuery Mobile 1,0</span><span class="sxs-lookup"><span data-stu-id="c8ce2-346">jQuery Mobile 1.0</span></span>](jquery-mobile/cdnjquerymobile10.md "jQuery Mobile 1.0 na CDN do Microsoft Ajax")
- [<span data-ttu-id="c8ce2-347">jQuery Mobile 1,0 RC 2</span><span class="sxs-lookup"><span data-stu-id="c8ce2-347">jQuery Mobile 1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile10rc2.md "jQuery Mobile 1.0 RC2 na CDN do Microsoft Ajax")
- [<span data-ttu-id="c8ce2-348">jQuery Mobile 1,0 RC 1</span><span class="sxs-lookup"><span data-stu-id="c8ce2-348">jQuery Mobile 1.0 RC 1</span></span>](jquery-mobile/cdnjquerymobile10rc1.md "jQuery Mobile 1.0 RC1 na CDN do Microsoft Ajax")
- [<span data-ttu-id="c8ce2-349">jQuery Mobile 1,0 Beta 3</span><span class="sxs-lookup"><span data-stu-id="c8ce2-349">jQuery Mobile 1.0 beta 3</span></span>](jquery-mobile/cdnjquerymobile10b3.md "jQuery Mobile 1.0 Beta 3 na CDN do Microsoft Ajax")

<a id="jQuery_Templates_Releases_on_the_CDN_5"></a>

### <a name="jquery-templates-releases-on-the-cdn"></a><span data-ttu-id="c8ce2-350">Versões de modelos jQuery no CDN</span><span class="sxs-lookup"><span data-stu-id="c8ce2-350">jQuery Templates Releases on the CDN</span></span>

<span data-ttu-id="c8ce2-351">As seguintes versões do plug-in de modelos jQuery são hospedadas nessa CDN.</span><span class="sxs-lookup"><span data-stu-id="c8ce2-351">The following releases of the jQuery Templates plugin are hosted on this CDN.</span></span> <span data-ttu-id="c8ce2-352">Clique em cada link para ver a lista real de arquivos.</span><span class="sxs-lookup"><span data-stu-id="c8ce2-352">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="c8ce2-353">Modelos jQuery Beta 1</span><span class="sxs-lookup"><span data-stu-id="c8ce2-353">jQuery Templates Beta 1</span></span>](jquery-templates/cdnjquerytemplatesbeta1.md "Modelos jQuery Beta 1")

<a id="jQuery_Cycle_Releases_on_the_CDN_6"></a>

### <a name="jquery-cycle-releases-on-the-cdn"></a><span data-ttu-id="c8ce2-354">Versões de ciclo jQuery na CDN</span><span class="sxs-lookup"><span data-stu-id="c8ce2-354">jQuery Cycle Releases on the CDN</span></span>

<span data-ttu-id="c8ce2-355">As seguintes versões do plug-in do ciclo jQuery são hospedadas nessa CDN.</span><span class="sxs-lookup"><span data-stu-id="c8ce2-355">The following releases of the jQuery Cycle plugin are hosted on this CDN.</span></span> <span data-ttu-id="c8ce2-356">Clique em cada link para ver a lista real de arquivos.</span><span class="sxs-lookup"><span data-stu-id="c8ce2-356">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="c8ce2-357">jQuery Cycle 2.99</span><span class="sxs-lookup"><span data-stu-id="c8ce2-357">jQuery Cycle 2.99</span></span>](jquery-cycle/cdnjquerycycle299.md "jQuery Cycle 2.99")
- [<span data-ttu-id="c8ce2-358">jQuery Cycle 2.94</span><span class="sxs-lookup"><span data-stu-id="c8ce2-358">jQuery Cycle 2.94</span></span>](jquery-cycle/cdnjquerycycle294.md "jQuery Cycle 2.94")
- [<span data-ttu-id="c8ce2-359">jQuery Cycle 2.88</span><span class="sxs-lookup"><span data-stu-id="c8ce2-359">jQuery Cycle 2.88</span></span>](jquery-cycle/cdnjquerycycle288.md "jQuery Cycle 2.88")

<a id="jQuery_DataTables_Releases_on_the_CDN_7"></a>

### <a name="jquery-datatables-releases-on-the-cdn"></a><span data-ttu-id="c8ce2-360">Versões do jQuery DataTables no CDN</span><span class="sxs-lookup"><span data-stu-id="c8ce2-360">jQuery DataTables Releases on the CDN</span></span>

<span data-ttu-id="c8ce2-361">As seguintes versões do plug-in jQuery DataTables estão hospedadas nessa CDN.</span><span class="sxs-lookup"><span data-stu-id="c8ce2-361">The following releases of the jQuery DataTables plugin are hosted on this CDN.</span></span> <span data-ttu-id="c8ce2-362">Clique em cada link para ver a lista real de arquivos.</span><span class="sxs-lookup"><span data-stu-id="c8ce2-362">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="c8ce2-363">jQuery DataTables 1.10.5</span><span class="sxs-lookup"><span data-stu-id="c8ce2-363">jQuery DataTables 1.10.5</span></span>](jquery-datatables/cdnjquerydatatables105.md "jQuery DataTables 1.10.5")
- [<span data-ttu-id="c8ce2-364">jQuery DataTables 1.10.4</span><span class="sxs-lookup"><span data-stu-id="c8ce2-364">jQuery DataTables 1.10.4</span></span>](jquery-datatables/cdnjquerydatatables104.md "jQuery DataTables 1.10.4")
- [<span data-ttu-id="c8ce2-365">jQuery DataTables 1.9.4</span><span class="sxs-lookup"><span data-stu-id="c8ce2-365">jQuery DataTables 1.9.4</span></span>](jquery-datatables/cdnjquerydatatables194.md "jQuery DataTables 1.9.4")
- [<span data-ttu-id="c8ce2-366">jQuery DataTables 1.9.3</span><span class="sxs-lookup"><span data-stu-id="c8ce2-366">jQuery DataTables 1.9.3</span></span>](jquery-datatables/cdnjquerydatatables193.md "jQuery DataTables 1.9.3")
- [<span data-ttu-id="c8ce2-367">jQuery DataTables 1.9.2</span><span class="sxs-lookup"><span data-stu-id="c8ce2-367">jQuery DataTables 1.9.2</span></span>](jquery-datatables/cdnjquerydatatables192.md "jQuery DataTables 1.9.2")
- [<span data-ttu-id="c8ce2-368">jQuery DataTables 1.9.1</span><span class="sxs-lookup"><span data-stu-id="c8ce2-368">jQuery DataTables 1.9.1</span></span>](jquery-datatables/cdnjquerydatatables191.md "jQuery DataTables 1.9.1")
- [<span data-ttu-id="c8ce2-369">jQuery DataTables 1.9.0</span><span class="sxs-lookup"><span data-stu-id="c8ce2-369">jQuery DataTables 1.9.0</span></span>](jquery-datatables/cdnjquerydatatables190.md "jQuery DataTables 1.9.0")
- [<span data-ttu-id="c8ce2-370">jQuery DataTables 1.8.2</span><span class="sxs-lookup"><span data-stu-id="c8ce2-370">jQuery DataTables 1.8.2</span></span>](jquery-datatables/cdnjquerydatatables182.md "jQuery DataTables 1.8.2")

<a id="Modernizr_Releases_on_the_CDN_8"></a>

### <a name="modernizr-releases-on-the-cdn"></a><span data-ttu-id="c8ce2-371">Versões do Modernizr na CDN</span><span class="sxs-lookup"><span data-stu-id="c8ce2-371">Modernizr Releases on the CDN</span></span>

<span data-ttu-id="c8ce2-372">As seguintes versões do [Modernizr](http://www.modernizr.com "Modernizr") estão hospedadas na CDN:</span><span class="sxs-lookup"><span data-stu-id="c8ce2-372">The following releases of [Modernizr](http://www.modernizr.com "Modernizr") are hosted on the CDN:</span></span>

- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-3.5.0.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.8.3.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.2.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.1.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.6.2.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-1.7-development-only.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.0.6-development-only.js

<a id="JSHint_Releases_on_the_CDN_10"></a>

### <a name="jshint-releases-on-the-cdn"></a><span data-ttu-id="c8ce2-373">Versões JSHint no CDN</span><span class="sxs-lookup"><span data-stu-id="c8ce2-373">JSHint Releases on the CDN</span></span>

<span data-ttu-id="c8ce2-374">As seguintes versões do [JSHint](http://www.jshint.com "JSHint") são hospedadas na CDN:</span><span class="sxs-lookup"><span data-stu-id="c8ce2-374">The following releases of [JSHint](http://www.jshint.com "JSHint") are hosted on the CDN:</span></span>

- https://ajax.aspnetcdn.com/ajax/jshint/r07/jshint.js

<a id="Knockout_Releases_on_the_CDN_11"></a>

### <a name="knockout-releases-on-the-cdn"></a><span data-ttu-id="c8ce2-375">Versões do Knockout no CDN</span><span class="sxs-lookup"><span data-stu-id="c8ce2-375">Knockout Releases on the CDN</span></span>

<span data-ttu-id="c8ce2-376">As seguintes versões do [Knockout](http://www.knockoutjs.com "Separação") são hospedadas na CDN:</span><span class="sxs-lookup"><span data-stu-id="c8ce2-376">The following releases of [Knockout](http://www.knockoutjs.com "Knockout") are hosted on the CDN:</span></span>

- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.debug.js

<a id="Globalize_Releases_on_the_CDN_12"></a>

### <a name="globalize-releases-on-the-cdn"></a><span data-ttu-id="c8ce2-377">Globalizar versões no CDN</span><span class="sxs-lookup"><span data-stu-id="c8ce2-377">Globalize Releases on the CDN</span></span>

<span data-ttu-id="c8ce2-378">As seguintes versões de [globalizate](https://github.com/jquery/globalize "Globalizar") são hospedadas na CDN:</span><span class="sxs-lookup"><span data-stu-id="c8ce2-378">The following releases of [Globalize](https://github.com/jquery/globalize "Globalize") are hosted on the CDN:</span></span>

#### <a name="globalize-version-100"></a><span data-ttu-id="c8ce2-379">Globalizar versão 1.0.0</span><span class="sxs-lookup"><span data-stu-id="c8ce2-379">Globalize version 1.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/node-main.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/currency.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/date.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/message.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/number.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/plural.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/relative-time.js

#### <a name="globalize-version-011"></a><span data-ttu-id="c8ce2-380">Globalizar versão 0.1.1</span><span class="sxs-lookup"><span data-stu-id="c8ce2-380">Globalize version 0.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.min.js
- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.js
- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.cultures.js

    - <span data-ttu-id="c8ce2-381">todas as culturas</span><span class="sxs-lookup"><span data-stu-id="c8ce2-381">all cultures</span></span>
- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.culture.{culture-code}.js

    - <span data-ttu-id="c8ce2-382">Substitua "{Culture-Code}" pelo código de cultura desejado, por exemplo, globalize.culture.en-GB.js= = Microsoft files na CDN = = essas bibliotecas foram carregadas pela Microsoft.</span><span class="sxs-lookup"><span data-stu-id="c8ce2-382">Replace "{culture-code}" with the desired culture code, e.g. globalize.culture.en-GB.js== Microsoft Files on the CDN ==These libraries were uploaded by Microsoft.</span></span>

<a id="Respond_Releases_on_the_CDN_13"></a>

### <a name="respond-releases-on-the-cdn"></a><span data-ttu-id="c8ce2-383">Responder a versões no CDN</span><span class="sxs-lookup"><span data-stu-id="c8ce2-383">Respond Releases on the CDN</span></span>

<span data-ttu-id="c8ce2-384">As seguintes versões de [resposta](https://github.com/scottjehl/Respond "Responder") estão hospedadas na CDN:</span><span class="sxs-lookup"><span data-stu-id="c8ce2-384">The following releases of [Respond](https://github.com/scottjehl/Respond "Respond") are hosted on the CDN:</span></span>

#### <a name="respond-version-142"></a><span data-ttu-id="c8ce2-385">Responder à versão 1.4.2</span><span class="sxs-lookup"><span data-stu-id="c8ce2-385">Respond version 1.4.2</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.min.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.min.js

#### <a name="respond-version-141"></a><span data-ttu-id="c8ce2-386">Responder à versão 1.4.1</span><span class="sxs-lookup"><span data-stu-id="c8ce2-386">Respond version 1.4.1</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.min.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.min.js

#### <a name="respond-version-140"></a><span data-ttu-id="c8ce2-387">Responder à versão 1.4.0</span><span class="sxs-lookup"><span data-stu-id="c8ce2-387">Respond version 1.4.0</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.min.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.min.js

#### <a name="respond-version-130"></a><span data-ttu-id="c8ce2-388">Responder à versão 1.3.0</span><span class="sxs-lookup"><span data-stu-id="c8ce2-388">Respond version 1.3.0</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.3.0/respond.js

#### <a name="respond-version-120"></a><span data-ttu-id="c8ce2-389">Responder à versão 1.2.0</span><span class="sxs-lookup"><span data-stu-id="c8ce2-389">Respond version 1.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.2.0/respond.js

<a id="Bootstrap_Releases_on_the_CDN_14"></a>

### <a name="bootstrap-releases-on-the-cdn"></a><span data-ttu-id="c8ce2-390">Versões de Bootstrap no CDN</span><span class="sxs-lookup"><span data-stu-id="c8ce2-390">Bootstrap Releases on the CDN</span></span>

<span data-ttu-id="c8ce2-391">As seguintes versões da inicialização do [GetBootstrap.com](http://getbootstrap.com "getbootstrap.com") são hospedadas na CDN:</span><span class="sxs-lookup"><span data-stu-id="c8ce2-391">The following releases of [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap are hosted on the CDN:</span></span>

#### <a name="bootstrap-version-452"></a><span data-ttu-id="c8ce2-392">Versão de inicialização 4.5.2</span><span class="sxs-lookup"><span data-stu-id="c8ce2-392">Bootstrap version 4.5.2</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/4.5.2/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.5.2/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.5.2/bootstrap.bundle.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.5.2/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.5.2/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.5.2/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.5.2/css/bootstrap-grid.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.5.2/css/bootstrap-grid.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.5.2/css/bootstrap-grid.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.5.2/css/bootstrap-reboot.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.5.2/css/bootstrap-reboot.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.5.2/css/bootstrap-reboot.css.map

#### <a name="bootstrap-version-450"></a><span data-ttu-id="c8ce2-393">Versão do bootstrap 4.5.0</span><span class="sxs-lookup"><span data-stu-id="c8ce2-393">Bootstrap version 4.5.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/4.5.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.5.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.5.0/bootstrap.bundle.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.5.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.5.0/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.5.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.5.0/css/bootstrap-grid.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.5.0/css/bootstrap-grid.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.5.0/css/bootstrap-grid.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.5.0/css/bootstrap-reboot.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.5.0/css/bootstrap-reboot.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.5.0/css/bootstrap-reboot.css.map

#### <a name="bootstrap-version-441"></a><span data-ttu-id="c8ce2-394">Versão de inicialização 4.4.1</span><span class="sxs-lookup"><span data-stu-id="c8ce2-394">Bootstrap version 4.4.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/bootstrap.bundle.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/css/bootstrap-grid.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/css/bootstrap-grid.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/css/bootstrap-grid.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/css/bootstrap-reboot.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/css/bootstrap-reboot.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/css/bootstrap-reboot.css.map

#### <a name="bootstrap-version-431"></a><span data-ttu-id="c8ce2-395">Versão de Bootstrap 4.3.1</span><span class="sxs-lookup"><span data-stu-id="c8ce2-395">Bootstrap version 4.3.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/bootstrap.bundle.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/css/bootstrap-grid.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/css/bootstrap-grid.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/css/bootstrap-grid.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/css/bootstrap-reboot.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/css/bootstrap-reboot.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/css/bootstrap-reboot.css.map

#### <a name="bootstrap-version-421"></a><span data-ttu-id="c8ce2-396">Versão de inicialização 4.2.1</span><span class="sxs-lookup"><span data-stu-id="c8ce2-396">Bootstrap version 4.2.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/bootstrap.bundle.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap-grid.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap-grid.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap-grid.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap-reboot.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap-reboot.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap-reboot.css.map

#### <a name="bootstrap-version-411"></a><span data-ttu-id="c8ce2-397">Versão 4.1.1 do bootstrap</span><span class="sxs-lookup"><span data-stu-id="c8ce2-397">Bootstrap version 4.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/bootstrap.bundle.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-grid.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-grid.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-grid.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-reboot.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-reboot.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-reboot.css.map

#### <a name="bootstrap-version-400"></a><span data-ttu-id="c8ce2-398">Versão de inicialização 4.0.0</span><span class="sxs-lookup"><span data-stu-id="c8ce2-398">Bootstrap version 4.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/bootstrap.bundle.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-grid.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-grid.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-grid.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-reboot.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-reboot.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-reboot.css.map

#### <a name="bootstrap-version-341"></a><span data-ttu-id="c8ce2-399">Versão de inicialização 3.4.1</span><span class="sxs-lookup"><span data-stu-id="c8ce2-399">Bootstrap version 3.4.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-340"></a><span data-ttu-id="c8ce2-400">Versão de inicialização 3.4.0</span><span class="sxs-lookup"><span data-stu-id="c8ce2-400">Bootstrap version 3.4.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-337"></a><span data-ttu-id="c8ce2-401">Versão de inicialização 3.3.7</span><span class="sxs-lookup"><span data-stu-id="c8ce2-401">Bootstrap version 3.3.7</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-336"></a><span data-ttu-id="c8ce2-402">Versão de inicialização 3.3.6</span><span class="sxs-lookup"><span data-stu-id="c8ce2-402">Bootstrap version 3.3.6</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-335"></a><span data-ttu-id="c8ce2-403">Versão de inicialização 3.3.5</span><span class="sxs-lookup"><span data-stu-id="c8ce2-403">Bootstrap version 3.3.5</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-334"></a><span data-ttu-id="c8ce2-404">Versão de inicialização 3.3.4</span><span class="sxs-lookup"><span data-stu-id="c8ce2-404">Bootstrap version 3.3.4</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-332"></a><span data-ttu-id="c8ce2-405">Versão de inicialização 3.3.2</span><span class="sxs-lookup"><span data-stu-id="c8ce2-405">Bootstrap version 3.3.2</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-331"></a><span data-ttu-id="c8ce2-406">Versão de inicialização 3.3.1</span><span class="sxs-lookup"><span data-stu-id="c8ce2-406">Bootstrap version 3.3.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-330"></a><span data-ttu-id="c8ce2-407">Versão de inicialização 3.3.0</span><span class="sxs-lookup"><span data-stu-id="c8ce2-407">Bootstrap version 3.3.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-320"></a><span data-ttu-id="c8ce2-408">Versão de inicialização 3.2.0</span><span class="sxs-lookup"><span data-stu-id="c8ce2-408">Bootstrap version 3.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-311"></a><span data-ttu-id="c8ce2-409">Versão de inicialização 3.1.1</span><span class="sxs-lookup"><span data-stu-id="c8ce2-409">Bootstrap version 3.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-310"></a><span data-ttu-id="c8ce2-410">Versão de inicialização 3.1.0</span><span class="sxs-lookup"><span data-stu-id="c8ce2-410">Bootstrap version 3.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-303"></a><span data-ttu-id="c8ce2-411">Versão de inicialização 3.0.3</span><span class="sxs-lookup"><span data-stu-id="c8ce2-411">Bootstrap version 3.0.3</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-302"></a><span data-ttu-id="c8ce2-412">Versão 3.0.2 do bootstrap</span><span class="sxs-lookup"><span data-stu-id="c8ce2-412">Bootstrap version 3.0.2</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-301"></a><span data-ttu-id="c8ce2-413">Versão de Bootstrap 3.0.1</span><span class="sxs-lookup"><span data-stu-id="c8ce2-413">Bootstrap version 3.0.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-300"></a><span data-ttu-id="c8ce2-414">Versão de inicialização 3.0.0</span><span class="sxs-lookup"><span data-stu-id="c8ce2-414">Bootstrap version 3.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-232"></a><span data-ttu-id="c8ce2-415">Versão de inicialização 2.3.2</span><span class="sxs-lookup"><span data-stu-id="c8ce2-415">Bootstrap version 2.3.2</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings.png
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings-white.png

#### <a name="bootstrap-version-231"></a><span data-ttu-id="c8ce2-416">Versão do bootstrap 2.3.1</span><span class="sxs-lookup"><span data-stu-id="c8ce2-416">Bootstrap version 2.3.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings.png
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings-white.png

<a id="BootstrapTouchCarousel_Releases_on_the_CDN_18"></a>

### <a name="bootstrap-touchcarousel-releases-on-the-cdn"></a><span data-ttu-id="c8ce2-417">Versões TouchCarousel de Bootstrap no CDN</span><span class="sxs-lookup"><span data-stu-id="c8ce2-417">Bootstrap TouchCarousel Releases on the CDN</span></span>

<span data-ttu-id="c8ce2-418">As seguintes versões das [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") versões de inicialização TouchCarousel são hospedadas na CDN:</span><span class="sxs-lookup"><span data-stu-id="c8ce2-418">The following releases of [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") Bootstrap TouchCarousel releases are hosted on the CDN:</span></span>

#### <a name="bootstrap-touchcarousel-version-080"></a><span data-ttu-id="c8ce2-419">Bootstrap TouchCarousel versão 0.8.0</span><span class="sxs-lookup"><span data-stu-id="c8ce2-419">Bootstrap TouchCarousel version 0.8.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/css/bootstrap-touch-carousel.css
- https://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/js/bootstrap-touch-carousel.js

<a id="Hammerjs_Releases_on_the_CDN_19"></a>

### <a name="hammerjs-releases-on-the-cdn"></a><span data-ttu-id="c8ce2-420">Hammer.js versões no CDN</span><span class="sxs-lookup"><span data-stu-id="c8ce2-420">Hammer.js Releases on the CDN</span></span>

<span data-ttu-id="c8ce2-421">As seguintes versões de [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js versões são hospedadas na CDN:</span><span class="sxs-lookup"><span data-stu-id="c8ce2-421">The following releases of [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js releases are hosted on the CDN:</span></span>

#### <a name="hammerjs-version-204"></a><span data-ttu-id="c8ce2-422">Hammer.js versão 2.0.4</span><span class="sxs-lookup"><span data-stu-id="c8ce2-422">Hammer.js version 2.0.4</span></span>

- https://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.js
- https://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.js
- https://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.map

<a id="ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15"></a>

### <a name="aspnet-web-forms-and-ajax-releases-on-the-cdn"></a><span data-ttu-id="c8ce2-423">Versões Web Forms e AJAX do ASP.NET no CDN</span><span class="sxs-lookup"><span data-stu-id="c8ce2-423">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>

<span data-ttu-id="c8ce2-424">As seguintes versões da biblioteca do ASP.NET AJAX são hospedadas na CDN.</span><span class="sxs-lookup"><span data-stu-id="c8ce2-424">The following releases of the ASP.NET Ajax Library are hosted on the CDN.</span></span> <span data-ttu-id="c8ce2-425">Clique em cada link para ver a lista real de arquivos.</span><span class="sxs-lookup"><span data-stu-id="c8ce2-425">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="c8ce2-426">ASP.NET Web Forms e Ajax versão 4.5.2</span><span class="sxs-lookup"><span data-stu-id="c8ce2-426">ASP.NET Web Forms and Ajax version 4.5.2</span></span>](cdnajax452.md "Web Forms do ASP.NET e Ajax 4.5.2")
- [<span data-ttu-id="c8ce2-427">ASP.NET Web Forms e Ajax versão 4</span><span class="sxs-lookup"><span data-stu-id="c8ce2-427">ASP.NET Web Forms and Ajax version 4</span></span>](cdnajax4.md "Web Forms do ASP.NET e Ajax 4")
- [<span data-ttu-id="c8ce2-428">ASP.NET AJAX versão 3,5</span><span class="sxs-lookup"><span data-stu-id="c8ce2-428">ASP.NET Ajax version 3.5</span></span>](cdnajax35.md "ASP.NET Ajax 3.5")

<a id="ASPNET_MVC_Releases_on_the_CDN_16"></a>

### <a name="aspnet-mvc-releases-on-the-cdn"></a><span data-ttu-id="c8ce2-429">Versões MVC do ASP.NET no CDN</span><span class="sxs-lookup"><span data-stu-id="c8ce2-429">ASP.NET MVC Releases on the CDN</span></span>

<span data-ttu-id="c8ce2-430">Os seguintes arquivos JavaScript do ASP.NET MVC são hospedados nesta CDN:</span><span class="sxs-lookup"><span data-stu-id="c8ce2-430">The following ASP.NET MVC JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-mvc-523"></a><span data-ttu-id="c8ce2-431">ASP.NET MVC 5.2.3</span><span class="sxs-lookup"><span data-stu-id="c8ce2-431">ASP.NET MVC 5.2.3</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-51"></a><span data-ttu-id="c8ce2-432">ASP.NET MVC 5,1</span><span class="sxs-lookup"><span data-stu-id="c8ce2-432">ASP.NET MVC 5.1</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-50"></a><span data-ttu-id="c8ce2-433">ASP.NET MVC 5,0</span><span class="sxs-lookup"><span data-stu-id="c8ce2-433">ASP.NET MVC 5.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-40"></a><span data-ttu-id="c8ce2-434">ASP.NET MVC 4.0</span><span class="sxs-lookup"><span data-stu-id="c8ce2-434">ASP.NET MVC 4.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-30"></a><span data-ttu-id="c8ce2-435">ASP.NET MVC 3.0</span><span class="sxs-lookup"><span data-stu-id="c8ce2-435">ASP.NET MVC 3.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.js
- https://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.min.js
- https://ajax.aspnetcdn.com/ajax/jquery.unobtrusive-ajax/3.2.5/jquery.unobtrusive-ajax.js
- https://ajax.aspnetcdn.com/ajax/jquery.unobtrusive-ajax/3.2.5/jquery.unobtrusive-ajax.min.js 
- https://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.min.js
- https://ajax.aspnetcdn.com/ajax/jquery.validation.unobtrusive/3.2.10/jquery.validate.unobtrusive.js 
- https://ajax.aspnetcdn.com/ajax/jquery.validation.unobtrusive/3.2.10/jquery.validate.unobtrusive.min.js
- https://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.js
- https://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.debug.js

#### <a name="aspnet-mvc-20"></a><span data-ttu-id="c8ce2-436">ASP.NET MVC 2,0</span><span class="sxs-lookup"><span data-stu-id="c8ce2-436">ASP.NET MVC 2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.js
- https://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.debug.js

#### <a name="aspnet-mvc-10"></a><span data-ttu-id="c8ce2-437">ASP.NET MVC 1,0</span><span class="sxs-lookup"><span data-stu-id="c8ce2-437">ASP.NET MVC 1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.js
- https://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.debug.js

<a id="ASPNET_SignalR_Releases_on_the_CDN_17"></a>

### <a name="aspnet-signalr-releases-on-the-cdn"></a><span data-ttu-id="c8ce2-438">Versões do Signalr ASP.NET no CDN</span><span class="sxs-lookup"><span data-stu-id="c8ce2-438">ASP.NET SignalR Releases on the CDN</span></span>

<span data-ttu-id="c8ce2-439">Os seguintes arquivos JavaScript do Signalr ASP.NET são hospedados nesta CDN:</span><span class="sxs-lookup"><span data-stu-id="c8ce2-439">The following ASP.NET SignalR JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-signalr-222"></a><span data-ttu-id="c8ce2-440">ASP.NET Signalr 2.2.2</span><span class="sxs-lookup"><span data-stu-id="c8ce2-440">ASP.NET SignalR 2.2.2</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.js

#### <a name="aspnet-signalr-221"></a><span data-ttu-id="c8ce2-441">ASP.NET Signalr 2.2.1</span><span class="sxs-lookup"><span data-stu-id="c8ce2-441">ASP.NET SignalR 2.2.1</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.js

#### <a name="aspnet-signalr-220"></a><span data-ttu-id="c8ce2-442">ASP.NET Signalr 2.2.0</span><span class="sxs-lookup"><span data-stu-id="c8ce2-442">ASP.NET SignalR 2.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.js

#### <a name="aspnet-signalr-210"></a><span data-ttu-id="c8ce2-443">ASP.NET Signalr 2.1.0</span><span class="sxs-lookup"><span data-stu-id="c8ce2-443">ASP.NET SignalR 2.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.js

#### <a name="aspnet-signalr-203"></a><span data-ttu-id="c8ce2-444">ASP.NET Signalr 2.0.3</span><span class="sxs-lookup"><span data-stu-id="c8ce2-444">ASP.NET SignalR 2.0.3</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.js

#### <a name="aspnet-signalr-202"></a><span data-ttu-id="c8ce2-445">ASP.NET Signalr 2.0.2</span><span class="sxs-lookup"><span data-stu-id="c8ce2-445">ASP.NET SignalR 2.0.2</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.js

#### <a name="aspnet-signalr-201"></a><span data-ttu-id="c8ce2-446">ASP.NET Signalr 2.0.1</span><span class="sxs-lookup"><span data-stu-id="c8ce2-446">ASP.NET SignalR 2.0.1</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.js

#### <a name="aspnet-signalr-200"></a><span data-ttu-id="c8ce2-447">ASP.NET Signalr 2.0.0</span><span class="sxs-lookup"><span data-stu-id="c8ce2-447">ASP.NET SignalR 2.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.js

#### <a name="aspnet-signalr-113"></a><span data-ttu-id="c8ce2-448">ASP.NET Signalr 1.1.3</span><span class="sxs-lookup"><span data-stu-id="c8ce2-448">ASP.NET SignalR 1.1.3</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.js

#### <a name="aspnet-signalr-112"></a><span data-ttu-id="c8ce2-449">Sinalização ASP.NET 1.1.2</span><span class="sxs-lookup"><span data-stu-id="c8ce2-449">ASP.NET SignalR 1.1.2</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.js

#### <a name="aspnet-signalr-111"></a><span data-ttu-id="c8ce2-450">ASP.NET Signalr 1.1.1</span><span class="sxs-lookup"><span data-stu-id="c8ce2-450">ASP.NET SignalR 1.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.js

#### <a name="aspnet-signalr-110"></a><span data-ttu-id="c8ce2-451">ASP.NET Signalr 1.1.0</span><span class="sxs-lookup"><span data-stu-id="c8ce2-451">ASP.NET SignalR 1.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.js

#### <a name="aspnet-signalr-101"></a><span data-ttu-id="c8ce2-452">ASP.NET Signalr 1.0.1</span><span class="sxs-lookup"><span data-stu-id="c8ce2-452">ASP.NET SignalR 1.0.1</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.js

<span data-ttu-id="c8ce2-453">Para obter informações sobre os termos de uso da CDN, consulte [termos de uso da CDN do Microsoft Ajax](https://www.asp.net/terms-of-use "Termos de uso da CDN do Microsoft Ajax").</span><span class="sxs-lookup"><span data-stu-id="c8ce2-453">For information about the terms of use for the CDN, see [Microsoft Ajax CDN Terms of Use](https://www.asp.net/terms-of-use "Microsoft Ajax CDN Terms of Use").</span></span>
