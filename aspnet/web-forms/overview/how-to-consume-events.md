---
title: Como consumir eventos em um aplicativo Web Forms
description: Saiba como lidar com eventos de clique de botão em ASP.NET Web Forms aplicativos.
author: rick-anderson
ms.author: riande
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- events [ASP.NET], Web Forms
- Web Forms controls, and events
- event handlers [ASP.NET], Web Forms
- events [ASP.NET], consuming
- Web Forms, event handling
ms.assetid: 73bf8638-c4ec-4069-b0bb-a1dc79b92e32
ms.openlocfilehash: 2e1807fed7d03db5893e1767a7a3a0de81862e7f
ms.sourcegitcommit: db13f9477981daabd57b99a410ec34e31e8d6aae
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/17/2020
ms.locfileid: "94674811"
---
# <a name="how-to-consume-events-in-a-web-forms-app"></a><span data-ttu-id="b0158-103">Como: consumir eventos em um aplicativo Web Forms</span><span class="sxs-lookup"><span data-stu-id="b0158-103">How to: Consume events in a Web Forms app</span></span>

<span data-ttu-id="b0158-104">Um cenário comum em aplicativos de Formulários Web do ASP.NET é popular uma página da Web com controles e executar uma ação específica com base no controle em que o usuário clica.</span><span class="sxs-lookup"><span data-stu-id="b0158-104">A common scenario in ASP.NET Web Forms applications is to populate a webpage with controls, and then perform a specific action based on which control the user clicks.</span></span> <span data-ttu-id="b0158-105">Por exemplo, um controle <xref:System.Web.UI.WebControls.Button?displayProperty=nameWithType> gera um evento quando o usuário clica na página da Web.</span><span class="sxs-lookup"><span data-stu-id="b0158-105">For example, a <xref:System.Web.UI.WebControls.Button?displayProperty=nameWithType> control raises an event when the user clicks it in the webpage.</span></span> <span data-ttu-id="b0158-106">Ao manipular o evento, o aplicativo pode executar a lógica de aplicativo apropriada para esse clique do botão.</span><span class="sxs-lookup"><span data-stu-id="b0158-106">By handling the event, your application can perform the appropriate application logic for that button click.</span></span>  
  
## <a name="handle-a-button-click-event-on-a-webpage"></a><span data-ttu-id="b0158-107">Manipular um evento de clique de botão em uma página da Web</span><span class="sxs-lookup"><span data-stu-id="b0158-107">Handle a button-click event on a webpage</span></span>  
  
1. <span data-ttu-id="b0158-108">Crie uma página de Formulários Web do ASP.NET (página da Web) que tem um controle <xref:System.Web.UI.WebControls.Button> com o valor `OnClick` definido para o nome do método que você definirá na próxima etapa.</span><span class="sxs-lookup"><span data-stu-id="b0158-108">Create a ASP.NET Web Forms page (webpage) that has a <xref:System.Web.UI.WebControls.Button> control with the `OnClick` value set to the name of method that you will define in the next step.</span></span>  
  
    ```xml  
    <asp:Button ID="Button1" runat="server" Text="Click Me" OnClick="Button1_Click" />  
    ```  
  
2. <span data-ttu-id="b0158-109">Defina um manipulador de eventos que corresponda à assinatura de representante de evento <xref:System.Web.UI.WebControls.Button.Click> e que tenha o nome que você definiu para o valor `OnClick`.</span><span class="sxs-lookup"><span data-stu-id="b0158-109">Define an event handler that matches the <xref:System.Web.UI.WebControls.Button.Click> event delegate signature and that has the name you defined for the `OnClick` value.</span></span>  
  
    ```csharp  
    protected void Button1_Click(object sender, EventArgs e)  
    {  
        // perform action  
    }  
    ```  
  
    ```vb  
    Protected Sub Button1_Click(sender As Object, e As EventArgs) Handles Button1.Click  
        ' perform action  
    End Sub  
    ```  
  
     <span data-ttu-id="b0158-110">O evento <xref:System.Web.UI.WebControls.Button.Click> usa a classe <xref:System.EventHandler> para o tipo de representante e a classe <xref:System.EventArgs> para os dados do evento.</span><span class="sxs-lookup"><span data-stu-id="b0158-110">The <xref:System.Web.UI.WebControls.Button.Click> event uses the <xref:System.EventHandler> class for the delegate type and the <xref:System.EventArgs> class for the event data.</span></span> <span data-ttu-id="b0158-111">A estrutura da página ASP.NET gera automaticamente o código que cria uma instância de <xref:System.EventHandler> e adiciona essa instância ao evento <xref:System.Web.UI.WebControls.Button.Click> da instância <xref:System.Web.UI.WebControls.Button>.</span><span class="sxs-lookup"><span data-stu-id="b0158-111">The ASP.NET page framework automatically generates code that creates an instance of <xref:System.EventHandler> and adds this delegate instance to the <xref:System.Web.UI.WebControls.Button.Click> event of the <xref:System.Web.UI.WebControls.Button> instance.</span></span>  
  
3. <span data-ttu-id="b0158-112">No método do manipulador de eventos que você definiu na etapa 2, adicione código para executar as ações que são necessárias quando o evento ocorre.</span><span class="sxs-lookup"><span data-stu-id="b0158-112">In the event handler method that you defined in step 2, add code to perform any actions that are required when the event occurs.</span></span>  
  
## <a name="see-also"></a><span data-ttu-id="b0158-113">Veja também</span><span class="sxs-lookup"><span data-stu-id="b0158-113">See also</span></span>

- [<span data-ttu-id="b0158-114">Examinar os eventos associados à inserção, atualização e exclusão</span><span class="sxs-lookup"><span data-stu-id="b0158-114">Examine the Events Associated with Inserting, Updating, and Deleting</span></span>](data-access/editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs.md)
- [<span data-ttu-id="b0158-115">Eventos no .NET</span><span class="sxs-lookup"><span data-stu-id="b0158-115">Events in .NET</span></span>](/dotnet/standard/index.md)
