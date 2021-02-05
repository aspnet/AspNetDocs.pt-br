---
uid: webhooks/receiving/receivers
title: Destinatários dos WebHooks do ASP.NET | Microsoft Docs
author: rick-anderson
description: Receptores de WebHooks do ASP.NET
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 6cdea089-15b2-4732-8c68-921ca561a8f1
ms.openlocfilehash: b2ff82777b396949ee089f4275cb6c318a45570c
ms.sourcegitcommit: e890232aa69193044409a0d4f968e5fec9ac4eb5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/05/2021
ms.locfileid: "99572068"
---
# <a name="aspnet-webhooks-receivers"></a><span data-ttu-id="9c216-103">Receptores de WebHooks do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="9c216-103">ASP.NET WebHooks receivers</span></span>

<span data-ttu-id="9c216-104">O recebimento de WebHooks depende de quem é o remetente.</span><span class="sxs-lookup"><span data-stu-id="9c216-104">Receiving WebHooks depends on who the sender is.</span></span> <span data-ttu-id="9c216-105">Às vezes, há etapas adicionais para registrar um webhook verificando se o assinante está realmente ouvindo.</span><span class="sxs-lookup"><span data-stu-id="9c216-105">Sometimes there are additional steps registering a WebHook verifying that the subscriber is really listening.</span></span> <span data-ttu-id="9c216-106">Alguns WebHooks fornecem um modelo de envio por push para pull, onde a solicitação HTTP POST contém apenas uma referência às informações de evento que, em seguida, é recuperada de forma independente.</span><span class="sxs-lookup"><span data-stu-id="9c216-106">Some WebHooks provide a push-to-pull model where the HTTP POST request only contains a reference to the event information which is then to be retrieved independently.</span></span> <span data-ttu-id="9c216-107">Geralmente, o modelo de segurança varia um pouco.</span><span class="sxs-lookup"><span data-stu-id="9c216-107">Often the security model varies quite a bit.</span></span>

<span data-ttu-id="9c216-108">A finalidade de Microsoft ASP.NET WebHooks é torná-lo mais simples e consistente para conectar sua API sem gastar muito tempo descobrindo como lidar com qualquer variante específica de WebHooks.</span><span class="sxs-lookup"><span data-stu-id="9c216-108">The purpose of Microsoft ASP.NET WebHooks is to make it both simpler and more consistent to wire up your API without spending a lot of time figuring out how to handle any particular variant of WebHooks.</span></span>

<span data-ttu-id="9c216-109">Um receptor de webhook é responsável por aceitar e verificar WebHooks de um determinado remetente.</span><span class="sxs-lookup"><span data-stu-id="9c216-109">A WebHook receiver is responsible for accepting and verifying WebHooks from a particular sender.</span></span> <span data-ttu-id="9c216-110">Um receptor de webhook pode dar suporte a qualquer número de WebHooks, cada um com sua própria configuração.</span><span class="sxs-lookup"><span data-stu-id="9c216-110">A WebHook receiver can support any number of WebHooks, each with their own configuration.</span></span> <span data-ttu-id="9c216-111">Por exemplo, o receptor de webhook do GitHub pode aceitar WebHooks de qualquer número de repositórios do GitHub.</span><span class="sxs-lookup"><span data-stu-id="9c216-111">For example, the GitHub WebHook receiver can accept WebHooks from any number of GitHub repositories.</span></span>

## <a name="webhook-receiver-uris"></a><span data-ttu-id="9c216-112">URIs do receptor de webhook</span><span class="sxs-lookup"><span data-stu-id="9c216-112">WebHook Receiver URIs</span></span>

<span data-ttu-id="9c216-113">Ao instalar Microsoft ASP.NET WebHooks, você obtém um controlador de webhook geral que aceita solicitações de webhook de um número de serviços aberto.</span><span class="sxs-lookup"><span data-stu-id="9c216-113">By installing Microsoft ASP.NET WebHooks you get a general WebHook controller which accepts WebHook requests from an open-ended number of services.</span></span> <span data-ttu-id="9c216-114">Quando uma solicitação chega, ela escolhe o receptor apropriado que você instalou para lidar com um remetente de webhook específico.</span><span class="sxs-lookup"><span data-stu-id="9c216-114">When a request arrives, it picks the appropriate receiver that you have installed for handling a particular WebHook sender.</span></span>

<span data-ttu-id="9c216-115">O URI desse controlador é o URI do webhook que você registra com o serviço e está no formato:</span><span class="sxs-lookup"><span data-stu-id="9c216-115">The URI of this controller is the WebHook URI that you register with the service and is of the form:</span></span>

```
https://<host>/api/webhooks/incoming/<receiver>/{id}
```

<span data-ttu-id="9c216-116">Por motivos de segurança, muitos receptores de webhook exigem que o URI seja um URI *https* e, em alguns casos, ele também deve conter um parâmetro de consulta adicional que é usado para impor que somente a parte pretendida possa enviar WebHooks para o URI acima.</span><span class="sxs-lookup"><span data-stu-id="9c216-116">For security reasons, many WebHook receivers require that the URI is an *https* URI and in some cases it must also contain an additional query parameter which is used to enforce that only the intended party can send WebHooks to the URI above.</span></span>

<span data-ttu-id="9c216-117">O `<receiver>` componente é o nome do receptor, por exemplo, `github` ou `slack` .</span><span class="sxs-lookup"><span data-stu-id="9c216-117">The `<receiver>` component is the name of the receiver, for example `github` or `slack`.</span></span>

<span data-ttu-id="9c216-118">*{ID}* é um identificador opcional que pode ser usado para identificar uma configuração de receptor de webhook específica.</span><span class="sxs-lookup"><span data-stu-id="9c216-118">The *{id}* is an optional identifier which can be used to identify a particular WebHook receiver configuration.</span></span> <span data-ttu-id="9c216-119">Isso pode ser usado para registrar N WebHooks com um receptor específico.</span><span class="sxs-lookup"><span data-stu-id="9c216-119">This can be used to register N WebHooks with a particular receiver.</span></span> <span data-ttu-id="9c216-120">Por exemplo, os três URIs a seguir podem ser usados para se registrar em três WebHooks independentes:</span><span class="sxs-lookup"><span data-stu-id="9c216-120">For example, the following three URIs can be used to register for three independent WebHooks:</span></span>

```
https://<host>/api/webhooks/incoming/github
https://<host>/api/webhooks/incoming/github/12345
https://<host>/api/webhooks/incoming/github/54321
```

## <a name="installing-a-webhook-receiver"></a><span data-ttu-id="9c216-121">Instalando um receptor de webhook</span><span class="sxs-lookup"><span data-stu-id="9c216-121">Installing a WebHook Receiver</span></span>

<span data-ttu-id="9c216-122">Para receber WebHooks usando Microsoft ASP.NET WebHooks, primeiro instale o pacote NuGet para o provedor de webhook ou provedores dos quais você deseja receber WebHooks.</span><span class="sxs-lookup"><span data-stu-id="9c216-122">To receive WebHooks using Microsoft ASP.NET WebHooks, you first install the Nuget package for the WebHook provider or providers you want to receive WebHooks from.</span></span> <span data-ttu-id="9c216-123">Os pacotes NuGet são chamados de [Microsoft. AspNet. WebHooks. receivers. \*](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) em que a última parte indica o serviço com suporte.</span><span class="sxs-lookup"><span data-stu-id="9c216-123">The Nuget packages are named [Microsoft.AspNet.WebHooks.Receivers.\*](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) where the last part indicates the service supported.</span></span> <span data-ttu-id="9c216-124">Por exemplo,</span><span class="sxs-lookup"><span data-stu-id="9c216-124">For example</span></span>

<span data-ttu-id="9c216-125">[Microsoft. AspNet. WebHooks. receivers. github](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) fornece suporte para o recebimento de WebHooks do GitHub e [Microsoft. AspNet. WebHooks. receiveers. Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) fornece suporte para o recebimento de WebHooks gerados por WebHooks ASP.net.</span><span class="sxs-lookup"><span data-stu-id="9c216-125">[Microsoft.AspNet.WebHooks.Receivers.GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) provides support for receiving WebHooks from GitHub and [Microsoft.AspNet.WebHooks.Receivers.Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) provides support for receiving WebHooks generated by ASP.NET WebHooks.</span></span>

<span data-ttu-id="9c216-126">Você pode encontrar suporte para Dropbox, GitHub, MailChimp, PayPal, Pusher, Salesforce, margem de atraso, Stripe, Trello e WordPress, mas é possível dar suporte a qualquer número de outros provedores.</span><span class="sxs-lookup"><span data-stu-id="9c216-126">Out of the box you can find support for Dropbox, GitHub, MailChimp, PayPal, Pusher, Salesforce, Slack, Stripe, Trello, and WordPress but it is possible to support any number of other providers.</span></span>

## <a name="configuring-a-webhook-receiver"></a><span data-ttu-id="9c216-127">Configurando um receptor de webhook</span><span class="sxs-lookup"><span data-stu-id="9c216-127">Configuring a WebHook Receiver</span></span>

<span data-ttu-id="9c216-128">Os receptores de webhook são configurados por meio da interface [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) e implementações específicas dessa interface podem ser registradas usando qualquer modelo de injeção de dependência.</span><span class="sxs-lookup"><span data-stu-id="9c216-128">WebHook Receivers are configured through the [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) interface and particular implementations of that interface can be registered using any dependency injection model.</span></span> <span data-ttu-id="9c216-129">A implementação padrão usa as configurações do aplicativo que podem ser definidas no arquivo de Web.config ou, se você estiver usando aplicativos Web do Azure, pode ser definida por meio do [portal do Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="9c216-129">The default implementation uses Application Settings which can either be set in the Web.config file, or, if using Azure Web Apps, can be set through the [Azure Portal](https://portal.azure.com/).</span></span>

![Configurações do Aplicativo do Azure](_static/AzureAppSettings.png)

<span data-ttu-id="9c216-131">O formato das chaves de configuração de aplicativo é o seguinte:</span><span class="sxs-lookup"><span data-stu-id="9c216-131">The format for Application Setting keys is as follows:</span></span>

```
MS_WebHookReceiverSecret_<receiver>
```

<span data-ttu-id="9c216-132">O valor é uma lista separada por vírgulas de valores que correspondem aos valores de *{ID}* para os quais WebHooks foram registrados, por exemplo:</span><span class="sxs-lookup"><span data-stu-id="9c216-132">The value is a comma-separated list of values matching the *{id}* values for which WebHooks have been registered, for example:</span></span>

```
MS_WebHookReceiverSecret_GitHub = <secret1>, 12345=<secret2>, 54321=<secret3>
```

## <a name="initializing-a-webhook-receiver"></a><span data-ttu-id="9c216-133">Inicializando um receptor de webhook</span><span class="sxs-lookup"><span data-stu-id="9c216-133">Initializing a WebHook Receiver</span></span>

<span data-ttu-id="9c216-134">Os receptores de webhook são inicializados registrando-os, normalmente na classe estática *WebApiConfig* , por exemplo:</span><span class="sxs-lookup"><span data-stu-id="9c216-134">WebHook Receivers are initialized by registering them, typically in the *WebApiConfig* static class, for example:</span></span>

```csharp
namespace WebHookReceivers
{
    public static class WebApiConfig
    {
        public static void Register(HttpConfiguration config)
        {
            // Web API configuration and services

            // Web API routes
            config.MapHttpAttributeRoutes();

            config.Routes.MapHttpRoute(
                name: "DefaultApi",
                routeTemplate: "api/{controller}/{id}",
                defaults: new { id = RouteParameter.Optional }
            );

            // Load receivers
            config.InitializeReceiveGitHubWebHooks();
        }
    }
}
```
