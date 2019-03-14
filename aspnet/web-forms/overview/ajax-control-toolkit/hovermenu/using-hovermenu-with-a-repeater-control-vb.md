---
uid: web-forms/overview/ajax-control-toolkit/hovermenu/using-hovermenu-with-a-repeater-control-vb
title: Uso de HoverMenu com um controle repetidor (VB) | Microsoft Docs
author: wenz
description: 'O controle de HoverMenu no AJAX Control Toolkit fornece um efeito de pop-up simples: Quando o ponteiro do mouse passa sobre um elemento, um pop-up é exibido em pela especificação...'
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 7f07c112-cd4f-4427-9699-57cfab2791fd
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/hovermenu/using-hovermenu-with-a-repeater-control-vb
msc.type: authoredcontent
ms.openlocfilehash: ef2481b93a8bbe16b79edb8c93c02e24fc9890f3
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57046453"
---
<a name="using-hovermenu-with-a-repeater-control-vb"></a><span data-ttu-id="8314c-103">Uso de HoverMenu com um controle repetidor (VB)</span><span class="sxs-lookup"><span data-stu-id="8314c-103">Using HoverMenu with a Repeater Control (VB)</span></span>
====================
<span data-ttu-id="8314c-104">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="8314c-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="8314c-105">[Baixar o código](http://download.microsoft.com/download/b/0/6/b06fe835-5b8f-4c00-aef8-062c19d75b95/HoverMenu1.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/hovermenu1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="8314c-105">[Download Code](http://download.microsoft.com/download/b/0/6/b06fe835-5b8f-4c00-aef8-062c19d75b95/HoverMenu1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/hovermenu1VB.pdf)</span></span>

> <span data-ttu-id="8314c-106">O controle de HoverMenu no AJAX Control Toolkit fornece um efeito de pop-up simples: Quando o ponteiro do mouse passa sobre um elemento, um pop-up aparece em uma posição especificada.</span><span class="sxs-lookup"><span data-stu-id="8314c-106">The HoverMenu control in the AJAX Control Toolkit provides a simple popup effect: When the mouse pointer hovers over an element, a popup appears at a specified position.</span></span> <span data-ttu-id="8314c-107">Também é possível usar esse controle em um repetidor.</span><span class="sxs-lookup"><span data-stu-id="8314c-107">It is also possible to use this control within a repeater.</span></span>


## <a name="overview"></a><span data-ttu-id="8314c-108">Visão geral</span><span class="sxs-lookup"><span data-stu-id="8314c-108">Overview</span></span>

<span data-ttu-id="8314c-109">O `HoverMenu` controle no AJAX Control Toolkit fornece um efeito de pop-up simples: Quando o ponteiro do mouse passa sobre um elemento, um pop-up aparece em uma posição especificada.</span><span class="sxs-lookup"><span data-stu-id="8314c-109">The `HoverMenu` control in the AJAX Control Toolkit provides a simple popup effect: When the mouse pointer hovers over an element, a popup appears at a specified position.</span></span> <span data-ttu-id="8314c-110">Também é possível usar esse controle em um repetidor.</span><span class="sxs-lookup"><span data-stu-id="8314c-110">It is also possible to use this control within a repeater.</span></span>

## <a name="steps"></a><span data-ttu-id="8314c-111">Etapas</span><span class="sxs-lookup"><span data-stu-id="8314c-111">Steps</span></span>

<span data-ttu-id="8314c-112">Em primeiro lugar, uma fonte de dados é necessária.</span><span class="sxs-lookup"><span data-stu-id="8314c-112">First of all, a data source is required.</span></span> <span data-ttu-id="8314c-113">Este exemplo usa o banco de dados AdventureWorks e o Microsoft SQL Server 2005 Express Edition.</span><span class="sxs-lookup"><span data-stu-id="8314c-113">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="8314c-114">O banco de dados é uma parte opcional da instalação do Visual Studio (inclusive a express edition) e também está disponível como um download separado em [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="8314c-114">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="8314c-115">O banco de dados AdventureWorks é parte do SQL Server 2005 exemplos e bancos de dados de exemplo (download em [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span><span class="sxs-lookup"><span data-stu-id="8314c-115">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="8314c-116">A maneira mais fácil de configurar o banco de dados é usar o Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) e anexe o `AdventureWorks.mdf` arquivo de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="8314c-116">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span>

<span data-ttu-id="8314c-117">Para este exemplo, vamos supor que a instância do SQL Server 2005 Express Edition é chamada `SQLEXPRESS` e reside no mesmo computador que o servidor web; isso também é a configuração padrão.</span><span class="sxs-lookup"><span data-stu-id="8314c-117">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="8314c-118">Se sua configuração for diferente, você precisa adaptar as informações de conexão para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="8314c-118">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="8314c-119">Para ativar a funcionalidade do AJAX ASP.NET e o Kit de ferramentas de controle, o `ScriptManager` controle deve ser colocada em qualquer lugar na página (mas dentro de `<form>` elemento):</span><span class="sxs-lookup"><span data-stu-id="8314c-119">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample1.aspx)]

<span data-ttu-id="8314c-120">Em seguida, adicione uma fonte de dados para a página.</span><span class="sxs-lookup"><span data-stu-id="8314c-120">Then, add a data source to the page.</span></span> <span data-ttu-id="8314c-121">Para usar uma quantidade limitada de dados, vamos selecionar apenas as primeiras cinco entradas na tabela de fornecedores de banco de dados AdventureWorks.</span><span class="sxs-lookup"><span data-stu-id="8314c-121">In order to use a limited amount of data, we only select the first five entries in the Vendor table of the AdventureWorks database.</span></span> <span data-ttu-id="8314c-122">Se você estiver usando o Assistente do Visual Studio para criar a fonte de dados, lembre-se de que um bug na versão atual não prefixar o nome da tabela (`Vendor`) com `Purchasing`.</span><span class="sxs-lookup"><span data-stu-id="8314c-122">If you are using the Visual Studio assistant to create the data source, mind that a bug in the current version does not prefix the table name (`Vendor`) with `Purchasing`.</span></span> <span data-ttu-id="8314c-123">A marcação a seguir mostra a sintaxe correta:</span><span class="sxs-lookup"><span data-stu-id="8314c-123">The following markup shows the correct syntax:</span></span>

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample2.aspx)]

<span data-ttu-id="8314c-124">Em seguida, adicione um painel que serve como o popup modal:</span><span class="sxs-lookup"><span data-stu-id="8314c-124">Next, add a panel which serves as the modal popup:</span></span>

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample3.aspx)]

<span data-ttu-id="8314c-125">Agora, o `HoverMenuExtender` entra em cena.</span><span class="sxs-lookup"><span data-stu-id="8314c-125">Now, the `HoverMenuExtender` comes into play.</span></span> <span data-ttu-id="8314c-126">Para que todos os elementos na fonte de dados obtém seu próprio pop-up, o extensor deve ser colocado dentro do repetidor `<ItemTemplate>` seção.</span><span class="sxs-lookup"><span data-stu-id="8314c-126">So that every element in the data source gets its own popup, the extender must be put within the repeater's `<ItemTemplate>` section.</span></span> <span data-ttu-id="8314c-127">Aqui está a marcação:</span><span class="sxs-lookup"><span data-stu-id="8314c-127">Here is the markup:</span></span>

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample4.aspx)]

<span data-ttu-id="8314c-128">Agora cada item na fonte de dados exibe um pop-up para a direita (`PopupPosition` atributo) após um atraso de 50 milissegundos (`PopDelay` atributo).</span><span class="sxs-lookup"><span data-stu-id="8314c-128">Now every item in the data source displays a popup to the right (`PopupPosition` attribute) after a delay of 50 milliseconds (`PopDelay` attribute).</span></span>


<span data-ttu-id="8314c-129">[![O menu de foco é exibido ao lado de cada item no repetidor](using-hovermenu-with-a-repeater-control-vb/_static/image2.png)](using-hovermenu-with-a-repeater-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="8314c-129">[![The hover menu appears next to each item in the repeater](using-hovermenu-with-a-repeater-control-vb/_static/image2.png)](using-hovermenu-with-a-repeater-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="8314c-130">O menu de foco é exibido ao lado de cada item no repetidor ([clique para exibir a imagem em tamanho normal](using-hovermenu-with-a-repeater-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="8314c-130">The hover menu appears next to each item in the repeater ([Click to view full-size image](using-hovermenu-with-a-repeater-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="8314c-131">Anterior</span><span class="sxs-lookup"><span data-stu-id="8314c-131">Previous</span></span>](using-hovermenu-with-a-repeater-control-cs.md)