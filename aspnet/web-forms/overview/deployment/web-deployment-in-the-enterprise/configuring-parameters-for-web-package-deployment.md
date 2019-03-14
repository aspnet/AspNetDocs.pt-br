---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment
title: Configurar parâmetros para a implantação de pacote da Web | Microsoft Docs
author: jrjlee
description: Este tópico descreve como definir valores de parâmetro, como nomes de aplicativo web de serviços de informações da Internet (IIS), cadeias de caracteres de conexão e pontos de extremidade de serviço...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 37947d79-ab1e-4ba9-9017-52e7a2757414
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment
msc.type: authoredcontent
ms.openlocfilehash: 6660ad52ce68932be63e2874a5f6acb34336e575
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57052063"
---
<a name="configuring-parameters-for-web-package-deployment"></a><span data-ttu-id="9c8f1-103">Configuração de parâmetros para a implantação de pacote da Web</span><span class="sxs-lookup"><span data-stu-id="9c8f1-103">Configuring Parameters for Web Package Deployment</span></span>
====================
<span data-ttu-id="9c8f1-104">by [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="9c8f1-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="9c8f1-105">Baixar PDF</span><span class="sxs-lookup"><span data-stu-id="9c8f1-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="9c8f1-106">Este tópico descreve como definir valores de parâmetro, como nomes de aplicativo web de serviços de informações da Internet (IIS), cadeias de caracteres de conexão e pontos de extremidade de serviço, quando você implanta um pacote da web em um servidor web IIS remoto.</span><span class="sxs-lookup"><span data-stu-id="9c8f1-106">This topic describes how to set parameter values, like Internet Information Services (IIS) web application names, connection strings, and service endpoints, when you deploy a web package to a remote IIS web server.</span></span>


<span data-ttu-id="9c8f1-107">Quando você compila um projeto de aplicativo web, a compilação e o processo de empacotamento gera três arquivos principais:</span><span class="sxs-lookup"><span data-stu-id="9c8f1-107">When you build a web application project, the build and packaging process generates three key files:</span></span>

- <span data-ttu-id="9c8f1-108">Um *[nome do projeto]. zip* arquivo.</span><span class="sxs-lookup"><span data-stu-id="9c8f1-108">A *[project name].zip* file.</span></span> <span data-ttu-id="9c8f1-109">Isso é o pacote de implantação da web para seu projeto de aplicativo web.</span><span class="sxs-lookup"><span data-stu-id="9c8f1-109">This is the web deployment package for your web application project.</span></span> <span data-ttu-id="9c8f1-110">Este pacote contém todos os assemblies, arquivos, scripts de banco de dados e recursos necessários para recriar seu aplicativo web em um servidor de web IIS remoto.</span><span class="sxs-lookup"><span data-stu-id="9c8f1-110">This package contains all the assemblies, files, database scripts, and resources required to recreate your web application on a remote IIS web server.</span></span>
- <span data-ttu-id="9c8f1-111">Um *[nome do projeto].deploy.cmd* arquivo.</span><span class="sxs-lookup"><span data-stu-id="9c8f1-111">A *[project name].deploy.cmd* file.</span></span> <span data-ttu-id="9c8f1-112">Contém um conjunto de comandos com parâmetros (MSDeploy.exe) de implantação da Web que publica o pacote de implantação da web em um servidor web IIS remoto.</span><span class="sxs-lookup"><span data-stu-id="9c8f1-112">This contains a set of parameterized Web Deploy (MSDeploy.exe) commands that publish your web deployment package to a remote IIS web server.</span></span>
- <span data-ttu-id="9c8f1-113">Um *[nome do projeto]. SetParameters.xml* arquivo.</span><span class="sxs-lookup"><span data-stu-id="9c8f1-113">A *[project name].SetParameters.xml* file.</span></span> <span data-ttu-id="9c8f1-114">Isso fornece um conjunto de valores de parâmetro para o comando MSDeploy.exe.</span><span class="sxs-lookup"><span data-stu-id="9c8f1-114">This provides a set of parameter values to the MSDeploy.exe command.</span></span> <span data-ttu-id="9c8f1-115">Você pode atualizar os valores nesse arquivo e passá-lo à implantação da Web como um parâmetro de linha de comando quando você implanta o pacote da web.</span><span class="sxs-lookup"><span data-stu-id="9c8f1-115">You can update the values in this file and pass it to Web Deploy as a command-line parameter when you deploy your web package.</span></span>

> [!NOTE]
> <span data-ttu-id="9c8f1-116">Para obter mais informações sobre o processo de empacotamento e a compilação, consulte [compilação e empacotamento Web Application Projects](building-and-packaging-web-application-projects.md).</span><span class="sxs-lookup"><span data-stu-id="9c8f1-116">For more information on the build and packaging process, see [Building and Packaging Web Application Projects](building-and-packaging-web-application-projects.md).</span></span>


<span data-ttu-id="9c8f1-117">O *SetParameters.xml* arquivo é gerado dinamicamente do seu arquivo de projeto de aplicativo web e quaisquer arquivos de configuração dentro de seu projeto.</span><span class="sxs-lookup"><span data-stu-id="9c8f1-117">The *SetParameters.xml* file is dynamically generated from your web application project file and any configuration files within your project.</span></span> <span data-ttu-id="9c8f1-118">Quando você compila e empacotar seu projeto, o Pipeline de publicação de Web (WPP) detectará automaticamente muitas das variáveis que têm probabilidade de mudar entre ambientes de implantação, como o destino do aplicativo web do IIS e quaisquer cadeias de caracteres de conexão de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="9c8f1-118">When you build and package your project, the Web Publishing Pipeline (WPP) will automatically detect lots of the variables that are likely to change between deployment environments, like the destination IIS web application and any database connection strings.</span></span> <span data-ttu-id="9c8f1-119">Esses valores são parametrizados no pacote de implantação da web e adicionados automaticamente à *SetParameters.xml* arquivo.</span><span class="sxs-lookup"><span data-stu-id="9c8f1-119">These values are automatically parameterized in the web deployment package and added to the *SetParameters.xml* file.</span></span> <span data-ttu-id="9c8f1-120">Por exemplo, se você adicionar uma cadeia de caracteres de conexão para o *Web. config* arquivo no seu projeto de aplicativo web, o processo de build detectará essa alteração e adicionará uma entrada para o *SetParameters.xml* arquivo adequadamente.</span><span class="sxs-lookup"><span data-stu-id="9c8f1-120">For example, if you add a connection string to the *web.config* file in your web application project, the build process will detect this change and will add an entry to the *SetParameters.xml* file accordingly.</span></span>

<span data-ttu-id="9c8f1-121">Em muitos casos, essa parametrização automática será suficiente.</span><span class="sxs-lookup"><span data-stu-id="9c8f1-121">In a lot of cases, this automatic parameterization will be sufficient.</span></span> <span data-ttu-id="9c8f1-122">No entanto, se os usuários precisam para variar as outras configurações entre ambientes de implantação, como as configurações do aplicativo ou URLs de ponto de extremidade de serviço, você precisará informar o WPP parametrizar esses valores no pacote de implantação e adicionar entradas correspondentes para o *SetParameters.xml* arquivo.</span><span class="sxs-lookup"><span data-stu-id="9c8f1-122">However, if your users need to vary other settings between deployment environments, like application settings or service endpoint URLs, you need to tell the WPP to parameterize these values in the deployment package and add corresponding entries to the *SetParameters.xml* file.</span></span> <span data-ttu-id="9c8f1-123">As seções a seguir explicam como fazer isso.</span><span class="sxs-lookup"><span data-stu-id="9c8f1-123">The sections that follow explain how to do this.</span></span>

### <a name="automatic-parameterization"></a><span data-ttu-id="9c8f1-124">Parametrização automática</span><span class="sxs-lookup"><span data-stu-id="9c8f1-124">Automatic Parameterization</span></span>

<span data-ttu-id="9c8f1-125">Quando você compilar e empacota um aplicativo web, o WPP parametrizar automaticamente essas coisas:</span><span class="sxs-lookup"><span data-stu-id="9c8f1-125">When you build and package a web application, the WPP will automatically parameterize these things:</span></span>

- <span data-ttu-id="9c8f1-126">O destino do IIS web nome e caminho do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="9c8f1-126">The destination IIS web application path and name.</span></span>
- <span data-ttu-id="9c8f1-127">Qualquer conexão cadeias de caracteres no seu *Web. config* arquivo.</span><span class="sxs-lookup"><span data-stu-id="9c8f1-127">Any connection strings in your *web.config* file.</span></span>
- <span data-ttu-id="9c8f1-128">Cadeias de caracteres de Conexão para qualquer banco de dados que você adicionar à **empacotar/publicar SQL** guia nas páginas de propriedade do projeto.</span><span class="sxs-lookup"><span data-stu-id="9c8f1-128">Connection strings for any databases you add to the **Package/Publish SQL** tab in the project property pages.</span></span>

<span data-ttu-id="9c8f1-129">Por exemplo, se você compilar e empacotar o [Contact Manager](the-contact-manager-solution.md) solução de exemplo sem tocar o processo de parametrização de qualquer forma, o WPP isso geraria *ContactManager.Mvc.SetParameters.xml* arquivo:</span><span class="sxs-lookup"><span data-stu-id="9c8f1-129">For example, if you were to build and package the [Contact Manager](the-contact-manager-solution.md) sample solution without touching the parameterization process in any way, the WPP would generate this *ContactManager.Mvc.SetParameters.xml* file:</span></span>


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample1.xml)]


<span data-ttu-id="9c8f1-130">Nesse caso:</span><span class="sxs-lookup"><span data-stu-id="9c8f1-130">In this case:</span></span>

- <span data-ttu-id="9c8f1-131">O **nome do aplicativo Web do IIS** parâmetro é o caminho onde você deseja implantar o aplicativo web do IIS.</span><span class="sxs-lookup"><span data-stu-id="9c8f1-131">The **IIS Web Application Name** parameter is the IIS path where you want to deploy the web application.</span></span> <span data-ttu-id="9c8f1-132">O valor padrão é obtido a **empacotar/Publicar Web** página nas páginas de propriedade do projeto.</span><span class="sxs-lookup"><span data-stu-id="9c8f1-132">The default value is taken from the **Package/Publish Web** page in the project property pages.</span></span>
- <span data-ttu-id="9c8f1-133">O **cadeia de caracteres de Conexão de Web. config de ApplicationServices** parâmetro foi gerado a partir um **connectionStrings/adicione** elemento no *Web. config* arquivo.</span><span class="sxs-lookup"><span data-stu-id="9c8f1-133">The **ApplicationServices-Web.config Connection String** parameter was generated from a **connectionStrings/add** element in the *web.config* file.</span></span> <span data-ttu-id="9c8f1-134">Representa a cadeia de caracteres de conexão que o aplicativo deve usar, entre em contato com o banco de dados de associação.</span><span class="sxs-lookup"><span data-stu-id="9c8f1-134">It represents the connection string that the application should use to contact the membership database.</span></span> <span data-ttu-id="9c8f1-135">O valor que você fornece aqui serão substituídos na implantado *Web. config* arquivo.</span><span class="sxs-lookup"><span data-stu-id="9c8f1-135">The value you provide here will be substituted into the deployed *web.config* file.</span></span> <span data-ttu-id="9c8f1-136">O valor padrão é obtido do Pre-deployment *Web. config* arquivo.</span><span class="sxs-lookup"><span data-stu-id="9c8f1-136">The default value is taken from the pre-deployment *web.config* file.</span></span>

<span data-ttu-id="9c8f1-137">O WPP também parametriza essas propriedades no pacote de implantação, que ele gera.</span><span class="sxs-lookup"><span data-stu-id="9c8f1-137">The WPP also parameterizes these properties in the deployment package it generates.</span></span> <span data-ttu-id="9c8f1-138">Você pode fornecer valores para essas propriedades quando você instala o pacote de implantação.</span><span class="sxs-lookup"><span data-stu-id="9c8f1-138">You can provide values for these properties when you install the deployment package.</span></span> <span data-ttu-id="9c8f1-139">Se você instalar o pacote manualmente por meio do Gerenciador do IIS, conforme descrito em [manualmente instalando os pacotes da Web](manually-installing-web-packages.md), o Assistente de instalação solicitará que você forneça valores para todos os parâmetros.</span><span class="sxs-lookup"><span data-stu-id="9c8f1-139">If you install the package manually through IIS Manager, as described in [Manually Installing Web Packages](manually-installing-web-packages.md), the installation wizard prompts you to provide values for any parameters.</span></span> <span data-ttu-id="9c8f1-140">Se você instalar o pacote remotamente usando o *. Deploy. cmd* do arquivo, conforme descrito em [Implantando pacotes da Web](deploying-web-packages.md), implantação da Web será a aparência a este *SetParameters.xml* de arquivos para Forneça os valores de parâmetro.</span><span class="sxs-lookup"><span data-stu-id="9c8f1-140">If you install the package remotely using the *.deploy.cmd* file, as described in [Deploying Web Packages](deploying-web-packages.md), Web Deploy will look to this *SetParameters.xml* file to provide the parameter values.</span></span> <span data-ttu-id="9c8f1-141">Você pode editar os valores de *SetParameters.xml* arquivo manualmente, ou você pode personalizar o arquivo como parte de um processo automatizado de compilação e implantação.</span><span class="sxs-lookup"><span data-stu-id="9c8f1-141">You can edit the values in the *SetParameters.xml* file manually, or you can customize the file as part of an automated build and deployment process.</span></span> <span data-ttu-id="9c8f1-142">Esse processo é descrito em mais detalhes mais adiante neste tópico.</span><span class="sxs-lookup"><span data-stu-id="9c8f1-142">This process is described in more detail later in this topic.</span></span>

### <a name="custom-parameterization"></a><span data-ttu-id="9c8f1-143">Parametrização personalizada</span><span class="sxs-lookup"><span data-stu-id="9c8f1-143">Custom Parameterization</span></span>

<span data-ttu-id="9c8f1-144">Em cenários de implantação mais complexos, você geralmente deseja parametrizar propriedades adicionais antes de implantar seu projeto.</span><span class="sxs-lookup"><span data-stu-id="9c8f1-144">In more complex deployment scenarios, you'll often want to parameterize additional properties before you deploy your project.</span></span> <span data-ttu-id="9c8f1-145">Em termos gerais, você deve parametrizar quaisquer propriedades e configurações que variam entre os ambientes de destino.</span><span class="sxs-lookup"><span data-stu-id="9c8f1-145">Generally speaking, you should parameterize any properties and settings that will vary between destination environments.</span></span> <span data-ttu-id="9c8f1-146">Eles podem incluir:</span><span class="sxs-lookup"><span data-stu-id="9c8f1-146">These can include:</span></span>

- <span data-ttu-id="9c8f1-147">Pontos de extremidade de serviço a *Web. config* arquivo.</span><span class="sxs-lookup"><span data-stu-id="9c8f1-147">Service endpoints in the *web.config* file.</span></span>
- <span data-ttu-id="9c8f1-148">Configurações do aplicativo na *Web. config* arquivo.</span><span class="sxs-lookup"><span data-stu-id="9c8f1-148">Application settings in the *web.config* file.</span></span>
- <span data-ttu-id="9c8f1-149">Outras propriedades declarativas que você deseja solicitam aos usuários especificar.</span><span class="sxs-lookup"><span data-stu-id="9c8f1-149">Any other declarative properties that you want to prompt users to specify.</span></span>

<span data-ttu-id="9c8f1-150">A maneira mais fácil para parametrizar dessas propriedades é adicionar um *parameters.xml* arquivo na pasta raiz do seu projeto de aplicativo web.</span><span class="sxs-lookup"><span data-stu-id="9c8f1-150">The easiest way to parameterize these properties is to add a *parameters.xml* file to the root folder of your web application project.</span></span> <span data-ttu-id="9c8f1-151">Por exemplo, na solução do Contact Manager, o projeto ContactManager.Mvc inclui um *parameters.xml* arquivo na pasta raiz.</span><span class="sxs-lookup"><span data-stu-id="9c8f1-151">For example, in the Contact Manager solution, the ContactManager.Mvc project includes a *parameters.xml* file in the root folder.</span></span>

![](configuring-parameters-for-web-package-deployment/_static/image1.png)

<span data-ttu-id="9c8f1-152">Se você abrir esse arquivo, você verá que ele contém uma única **parâmetro** entrada.</span><span class="sxs-lookup"><span data-stu-id="9c8f1-152">If you open this file, you'll see that it contains a single **parameter** entry.</span></span> <span data-ttu-id="9c8f1-153">A entrada usa uma consulta XML Path Language (XPath) para localizar e parametrizar a URL de ponto de extremidade do serviço no Windows Communication Foundation (WCF) ContactService a *Web. config* arquivo.</span><span class="sxs-lookup"><span data-stu-id="9c8f1-153">The entry uses an XML Path Language (XPath) query to locate and parameterize the endpoint URL of the ContactService Windows Communication Foundation (WCF) service in the *web.config* file.</span></span>


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample2.xml)]


<span data-ttu-id="9c8f1-154">Além de parametrizar a URL de ponto de extremidade no pacote de implantação, o WPP também adiciona uma entrada correspondente para o *SetParameters.xml* arquivo gerado junto com o pacote de implantação.</span><span class="sxs-lookup"><span data-stu-id="9c8f1-154">In addition to parameterizing the endpoint URL in the deployment package, the WPP also adds a corresponding entry to the *SetParameters.xml* file that gets generated alongside the deployment package.</span></span>


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample3.xml)]


<span data-ttu-id="9c8f1-155">Se você instalar o pacote de implantação manualmente, o Gerenciador do IIS solicitará o endereço do ponto de extremidade de serviço junto com as propriedades que foram parametrizadas automaticamente.</span><span class="sxs-lookup"><span data-stu-id="9c8f1-155">If you install the deployment package manually, IIS Manager will prompt you for the service endpoint address alongside the properties that were parameterized automatically.</span></span> <span data-ttu-id="9c8f1-156">Se você instalar o pacote de implantação executando o *. Deploy. cmd* arquivo, você pode editar o *SetParameters.xml* arquivo para fornecer um valor para o endereço do ponto de extremidade de serviço junto com os valores para o propriedades que foram parametrizadas automaticamente.</span><span class="sxs-lookup"><span data-stu-id="9c8f1-156">If you install the deployment package by running the *.deploy.cmd* file, you can edit the *SetParameters.xml* file to provide a value for the service endpoint address together with values for the properties that were parameterized automatically.</span></span>

<span data-ttu-id="9c8f1-157">Para obter detalhes completos sobre como criar uma *parameters.xml* arquivos, consulte [como: Use parâmetros para configurar as configurações quando um pacote de implantação está instalado](https://msdn.microsoft.com/library/ff398068.aspx).</span><span class="sxs-lookup"><span data-stu-id="9c8f1-157">For full details on how to create a *parameters.xml* file, see [How to: Use Parameters to Configure Deployment Settings When a Package is Installed](https://msdn.microsoft.com/library/ff398068.aspx).</span></span> <span data-ttu-id="9c8f1-158">O procedimento chamado **para usar parâmetros de implantação para configurações do arquivo Web. config** fornece instruções passo a passo.</span><span class="sxs-lookup"><span data-stu-id="9c8f1-158">The procedure named **To use deployment parameters for Web.config file settings** provides step-by-step instructions.</span></span>

## <a name="modifying-the-setparametersxml-file"></a><span data-ttu-id="9c8f1-159">Modificando o arquivo SetParameters.xml</span><span class="sxs-lookup"><span data-stu-id="9c8f1-159">Modifying the SetParameters.xml File</span></span>

<span data-ttu-id="9c8f1-160">Se você planeja implantar o pacote de aplicativo web manualmente&#x2014;seja executando o *. Deploy. cmd* arquivo ou executando MSDeploy.exe da linha de comando&#x2014;não há nada que impeça a editando manualmente o  *SetParameters.xml* arquivo antes da implantação.</span><span class="sxs-lookup"><span data-stu-id="9c8f1-160">If you plan to deploy the web application package manually&#x2014;either by running the *.deploy.cmd* file or by running MSDeploy.exe from the command line&#x2014;there's nothing to stop you manually editing the *SetParameters.xml* file prior to the deployment.</span></span> <span data-ttu-id="9c8f1-161">No entanto, se você estiver trabalhando em uma solução de escala empresarial, você precisa implantar um pacote de aplicativo da web como parte de um processo de compilação e implantação automatizado, maior.</span><span class="sxs-lookup"><span data-stu-id="9c8f1-161">However, if you're working on an enterprise-scale solution, you may need to deploy a web application package as part of a larger, automated build and deployment process.</span></span> <span data-ttu-id="9c8f1-162">Nesse cenário, você precisa que o Microsoft Build Engine (MSBuild) para modificar a *SetParameters.xml* arquivo para você.</span><span class="sxs-lookup"><span data-stu-id="9c8f1-162">In this scenario, you need the Microsoft Build Engine (MSBuild) to modify the *SetParameters.xml* file for you.</span></span> <span data-ttu-id="9c8f1-163">Você pode fazer isso usando o MSBuild **XmlPoke** tarefa.</span><span class="sxs-lookup"><span data-stu-id="9c8f1-163">You can do this by using the MSBuild **XmlPoke** task.</span></span>

<span data-ttu-id="9c8f1-164">O [solução de exemplo do Gerenciador de contatos](the-contact-manager-solution.md) ilustra esse processo.</span><span class="sxs-lookup"><span data-stu-id="9c8f1-164">The [Contact Manager sample solution](the-contact-manager-solution.md) illustrates this process.</span></span> <span data-ttu-id="9c8f1-165">Os exemplos de código a seguir foram editados para mostrar apenas os detalhes que são relevantes para este exemplo.</span><span class="sxs-lookup"><span data-stu-id="9c8f1-165">The code examples that follow have been edited to show only the details that are relevant to this example.</span></span>

> [!NOTE]
> <span data-ttu-id="9c8f1-166">Para obter uma visão mais ampla do modelo de arquivo de projeto na solução de exemplo e uma introdução aos arquivos de projeto personalizado em geral, consulte [Noções básicas sobre o arquivo de projeto](understanding-the-project-file.md) e [Noções básicas sobre o processo de compilação](understanding-the-build-process.md).</span><span class="sxs-lookup"><span data-stu-id="9c8f1-166">For a broader overview of the project file model in the sample solution, and an introduction to custom project files in general, see [Understanding the Project File](understanding-the-project-file.md) and [Understanding the Build Process](understanding-the-build-process.md).</span></span>


<span data-ttu-id="9c8f1-167">Primeiro, os valores de parâmetro de interesse são definidos como propriedades no arquivo de projeto específicas do ambiente (por exemplo, *Env Dev.proj*).</span><span class="sxs-lookup"><span data-stu-id="9c8f1-167">First, the parameter values of interest are defined as properties in the environment-specific project file (for example, *Env-Dev.proj*).</span></span>


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample4.xml)]


> [!NOTE]
> <span data-ttu-id="9c8f1-168">Para obter orientação sobre como personalizar os arquivos de projeto de ambiente específicas para seus próprios ambientes de servidor, consulte [configurar propriedades de implantação para um ambiente de destino](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).</span><span class="sxs-lookup"><span data-stu-id="9c8f1-168">For guidance on how to customize the environment-specific project files for your own server environments, see [Configure Deployment Properties for a Target Environment](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).</span></span>


<span data-ttu-id="9c8f1-169">Em seguida, o *Publish.proj* arquivo importa essas propriedades.</span><span class="sxs-lookup"><span data-stu-id="9c8f1-169">Next, the *Publish.proj* file imports these properties.</span></span> <span data-ttu-id="9c8f1-170">Porque cada *SetParameters.xml* arquivo está associado com um *. Deploy. cmd* arquivo e podemos, por fim, deseja que o arquivo de projeto para invocar cada *. Deploy. cmd* de arquivo do projeto arquivo cria um MSBuild *item* para cada *. Deploy. cmd* de arquivo e define as propriedades de interesse como *metadados de item*.</span><span class="sxs-lookup"><span data-stu-id="9c8f1-170">Because each *SetParameters.xml* file is associated with a *.deploy.cmd* file, and we ultimately want the project file to invoke each *.deploy.cmd* file, the project file creates an MSBuild *item* for each *.deploy.cmd* file and defines the properties of interest as *item metadata*.</span></span>


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample5.xml)]


<span data-ttu-id="9c8f1-171">Nesse caso:</span><span class="sxs-lookup"><span data-stu-id="9c8f1-171">In this case:</span></span>

- <span data-ttu-id="9c8f1-172">O **ParametersXml** metadados valor indica o local do *SetParameters.xml* arquivo.</span><span class="sxs-lookup"><span data-stu-id="9c8f1-172">The **ParametersXml** metadata value indicates the location of the *SetParameters.xml* file.</span></span>
- <span data-ttu-id="9c8f1-173">O **IisWebAppName** valor é o caminho para o qual você deseja implantar o aplicativo web do IIS.</span><span class="sxs-lookup"><span data-stu-id="9c8f1-173">The **IisWebAppName** value is the IIS path to which you want to deploy the web application.</span></span>
- <span data-ttu-id="9c8f1-174">O **MembershipDBConnectionString** valor é a cadeia de caracteres de conexão para o banco de dados de associação e o **MembershipDBConnectionName** valor é o **nome** atributo do parâmetro correspondente na *SetParameters.xml* arquivo.</span><span class="sxs-lookup"><span data-stu-id="9c8f1-174">The **MembershipDBConnectionString** value is the connection string for the membership database, and the **MembershipDBConnectionName** value is the **name** attribute of the corresponding parameter in the *SetParameters.xml* file.</span></span>
- <span data-ttu-id="9c8f1-175">O **ServiceEndpointValue** valor é o endereço do ponto de extremidade para o serviço WCF no servidor de destino e o **ServiceEndpointParamName** valor é o atributo de nome do parâmetro correspondente no o *SetParameters.xml* arquivo.</span><span class="sxs-lookup"><span data-stu-id="9c8f1-175">The **ServiceEndpointValue** value is the endpoint address for the WCF service on the destination server, and the **ServiceEndpointParamName** value is the name attribute of the corresponding parameter in the *SetParameters.xml* file.</span></span>

<span data-ttu-id="9c8f1-176">Por fim, na *Publish.proj* arquivo, o **PublishWebPackages** destino usa a **XmlPoke** tarefas para modificar esses valores no *SetParameters.xml* arquivo.</span><span class="sxs-lookup"><span data-stu-id="9c8f1-176">Finally, in the *Publish.proj* file, the **PublishWebPackages** target uses the **XmlPoke** task to modify these values in the *SetParameters.xml* file.</span></span>


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample6.xml)]


<span data-ttu-id="9c8f1-177">Você perceberá que cada **XmlPoke** tarefa especifica quatro valores de atributo:</span><span class="sxs-lookup"><span data-stu-id="9c8f1-177">You'll notice that each **XmlPoke** task specifies four attribute values:</span></span>

- <span data-ttu-id="9c8f1-178">O **XmlInputPath** atributo informa à tarefa onde encontrar o arquivo que você deseja modificar.</span><span class="sxs-lookup"><span data-stu-id="9c8f1-178">The **XmlInputPath** attribute tells the task where to find the file you want to modify.</span></span>
- <span data-ttu-id="9c8f1-179">O **consulta** atributo é uma consulta XPath que identifica o nó XML que você deseja alterar.</span><span class="sxs-lookup"><span data-stu-id="9c8f1-179">The **Query** attribute is an XPath query that identifies the XML node you want to change.</span></span>
- <span data-ttu-id="9c8f1-180">O **valor** atributo é o novo valor que você deseja inserir no nó selecionado no XML.</span><span class="sxs-lookup"><span data-stu-id="9c8f1-180">The **Value** attribute is the new value you want to insert into the selected XML node.</span></span>
- <span data-ttu-id="9c8f1-181">O **condição** atributo é o critério no qual a tarefa deve ou não ser executado.</span><span class="sxs-lookup"><span data-stu-id="9c8f1-181">The **Condition** attribute is the criteria on which the task should run or not run.</span></span> <span data-ttu-id="9c8f1-182">Nesses casos, a condição garante que você não tente inserir um valor nulo ou vazio para o *SetParameters.xml* arquivo.</span><span class="sxs-lookup"><span data-stu-id="9c8f1-182">In these cases, the condition ensures that you don't try to insert a null or empty value into the *SetParameters.xml* file.</span></span>

## <a name="conclusion"></a><span data-ttu-id="9c8f1-183">Conclusão</span><span class="sxs-lookup"><span data-stu-id="9c8f1-183">Conclusion</span></span>

<span data-ttu-id="9c8f1-184">Este tópico descreveu a função do *SetParameters.xml* de arquivo e expliquei como ela é gerada quando você compila um projeto de aplicativo web.</span><span class="sxs-lookup"><span data-stu-id="9c8f1-184">This topic described the role of the *SetParameters.xml* file and explained how it's generated when you build a web application project.</span></span> <span data-ttu-id="9c8f1-185">Ele explicou como é possível parametrizar configurações adicionais, adicionando um *parameters.xml* ao seu projeto.</span><span class="sxs-lookup"><span data-stu-id="9c8f1-185">It explained how you can parameterize additional settings by adding a *parameters.xml* file to your project.</span></span> <span data-ttu-id="9c8f1-186">Também descreveu como você pode modificar os *SetParameters.xml* arquivo como parte de um processo de compilação maiores e automatizada, usando o **XmlPoke** tarefa em seus arquivos de projeto.</span><span class="sxs-lookup"><span data-stu-id="9c8f1-186">It also described how you can modify the *SetParameters.xml* file as part of a larger, automated build process, by using the **XmlPoke** task in your project files.</span></span>

<span data-ttu-id="9c8f1-187">O próximo tópico, [Implantando pacotes da Web](deploying-web-packages.md), descreve como você pode implantar um pacote da web seja executando o *. Deploy. cmd* do arquivo ou usando MSDeploy.exe comandos diretamente.</span><span class="sxs-lookup"><span data-stu-id="9c8f1-187">The next topic, [Deploying Web Packages](deploying-web-packages.md), describes how you can deploy a web package either by running the *.deploy.cmd* file or by using MSDeploy.exe commands directly.</span></span> <span data-ttu-id="9c8f1-188">Em ambos os casos, você pode especificar sua *SetParameters.xml* arquivo como um parâmetro de implantação.</span><span class="sxs-lookup"><span data-stu-id="9c8f1-188">In both cases, you can specify your *SetParameters.xml* file as a deployment parameter.</span></span>

## <a name="further-reading"></a><span data-ttu-id="9c8f1-189">Leitura adicional</span><span class="sxs-lookup"><span data-stu-id="9c8f1-189">Further Reading</span></span>

<span data-ttu-id="9c8f1-190">Para obter informações sobre como criar pacotes da web, consulte [compilação e empacotamento Web Application Projects](building-and-packaging-web-application-projects.md).</span><span class="sxs-lookup"><span data-stu-id="9c8f1-190">For information on how to create web packages, see [Building and Packaging Web Application Projects](building-and-packaging-web-application-projects.md).</span></span> <span data-ttu-id="9c8f1-191">Para obter orientação sobre como implantar, na verdade, um pacote da web, consulte [Implantando pacotes da Web](deploying-web-packages.md).</span><span class="sxs-lookup"><span data-stu-id="9c8f1-191">For guidance on how to actually deploy a web package, see [Deploying Web Packages](deploying-web-packages.md).</span></span> <span data-ttu-id="9c8f1-192">Para obter uma explicação passo a passo sobre como criar uma *parameters.xml* arquivos, consulte [como: Use parâmetros para configurar as configurações quando um pacote de implantação está instalado](https://msdn.microsoft.com/library/ff398068.aspx).</span><span class="sxs-lookup"><span data-stu-id="9c8f1-192">For a step-by-step walkthrough on how to create a *parameters.xml* file, see [How to: Use Parameters to Configure Deployment Settings When a Package is Installed](https://msdn.microsoft.com/library/ff398068.aspx).</span></span>

<span data-ttu-id="9c8f1-193">Para obter mais informações sobre parametrização na implantação da Web, consulte [parametrização de implantação da Web em ação](https://go.microsoft.com/?linkid=9805119) (postagem de blog).</span><span class="sxs-lookup"><span data-stu-id="9c8f1-193">For more general information on parameterization in Web Deploy, see [Web Deploy Parameterization in Action](https://go.microsoft.com/?linkid=9805119) (blog post).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9c8f1-194">[Anterior](building-and-packaging-web-application-projects.md)
> [Próximo](deploying-web-packages.md)</span><span class="sxs-lookup"><span data-stu-id="9c8f1-194">[Previous](building-and-packaging-web-application-projects.md)
[Next](deploying-web-packages.md)</span></span>