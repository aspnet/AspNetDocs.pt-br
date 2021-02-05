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
# <a name="aspnet-webhooks-receivers"></a>Receptores de WebHooks do ASP.NET

O recebimento de WebHooks depende de quem é o remetente. Às vezes, há etapas adicionais para registrar um webhook verificando se o assinante está realmente ouvindo. Alguns WebHooks fornecem um modelo de envio por push para pull, onde a solicitação HTTP POST contém apenas uma referência às informações de evento que, em seguida, é recuperada de forma independente. Geralmente, o modelo de segurança varia um pouco.

A finalidade de Microsoft ASP.NET WebHooks é torná-lo mais simples e consistente para conectar sua API sem gastar muito tempo descobrindo como lidar com qualquer variante específica de WebHooks.

Um receptor de webhook é responsável por aceitar e verificar WebHooks de um determinado remetente. Um receptor de webhook pode dar suporte a qualquer número de WebHooks, cada um com sua própria configuração. Por exemplo, o receptor de webhook do GitHub pode aceitar WebHooks de qualquer número de repositórios do GitHub.

## <a name="webhook-receiver-uris"></a>URIs do receptor de webhook

Ao instalar Microsoft ASP.NET WebHooks, você obtém um controlador de webhook geral que aceita solicitações de webhook de um número de serviços aberto. Quando uma solicitação chega, ela escolhe o receptor apropriado que você instalou para lidar com um remetente de webhook específico.

O URI desse controlador é o URI do webhook que você registra com o serviço e está no formato:

```
https://<host>/api/webhooks/incoming/<receiver>/{id}
```

Por motivos de segurança, muitos receptores de webhook exigem que o URI seja um URI *https* e, em alguns casos, ele também deve conter um parâmetro de consulta adicional que é usado para impor que somente a parte pretendida possa enviar WebHooks para o URI acima.

O `<receiver>` componente é o nome do receptor, por exemplo, `github` ou `slack` .

*{ID}* é um identificador opcional que pode ser usado para identificar uma configuração de receptor de webhook específica. Isso pode ser usado para registrar N WebHooks com um receptor específico. Por exemplo, os três URIs a seguir podem ser usados para se registrar em três WebHooks independentes:

```
https://<host>/api/webhooks/incoming/github
https://<host>/api/webhooks/incoming/github/12345
https://<host>/api/webhooks/incoming/github/54321
```

## <a name="installing-a-webhook-receiver"></a>Instalando um receptor de webhook

Para receber WebHooks usando Microsoft ASP.NET WebHooks, primeiro instale o pacote NuGet para o provedor de webhook ou provedores dos quais você deseja receber WebHooks. Os pacotes NuGet são chamados de [Microsoft. AspNet. WebHooks. receivers. *](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) em que a última parte indica o serviço com suporte. Por exemplo,

[Microsoft. AspNet. WebHooks. receivers. github](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) fornece suporte para o recebimento de WebHooks do GitHub e [Microsoft. AspNet. WebHooks. receiveers. Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) fornece suporte para o recebimento de WebHooks gerados por WebHooks ASP.net.

Você pode encontrar suporte para Dropbox, GitHub, MailChimp, PayPal, Pusher, Salesforce, margem de atraso, Stripe, Trello e WordPress, mas é possível dar suporte a qualquer número de outros provedores.

## <a name="configuring-a-webhook-receiver"></a>Configurando um receptor de webhook

Os receptores de webhook são configurados por meio da interface [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) e implementações específicas dessa interface podem ser registradas usando qualquer modelo de injeção de dependência. A implementação padrão usa as configurações do aplicativo que podem ser definidas no arquivo de Web.config ou, se você estiver usando aplicativos Web do Azure, pode ser definida por meio do [portal do Azure](https://portal.azure.com/).

![Configurações do Aplicativo do Azure](_static/AzureAppSettings.png)

O formato das chaves de configuração de aplicativo é o seguinte:

```
MS_WebHookReceiverSecret_<receiver>
```

O valor é uma lista separada por vírgulas de valores que correspondem aos valores de *{ID}* para os quais WebHooks foram registrados, por exemplo:

```
MS_WebHookReceiverSecret_GitHub = <secret1>, 12345=<secret2>, 54321=<secret3>
```

## <a name="initializing-a-webhook-receiver"></a>Inicializando um receptor de webhook

Os receptores de webhook são inicializados registrando-os, normalmente na classe estática *WebApiConfig* , por exemplo:

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
