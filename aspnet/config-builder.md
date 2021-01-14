---
uid: config-builder
title: Construtores de configuração para ASP.NET
author: rick-anderson
description: Saiba como obter dados de configuração de fontes diferentes de web.config valores de fontes externas.
ms.author: riande
ms.date: 7/17/2020
msc.type: content
ms.openlocfilehash: 04bc6bcc2d9d0be561f8b1f2d909d8c46d90ad81
ms.sourcegitcommit: 1dd64aab844e52ed27819c4ae62167081a067134
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/14/2021
ms.locfileid: "98206108"
---
# <a name="configuration-builders-for-aspnet"></a><span data-ttu-id="b7bba-103">Construtores de configuração para ASP.NET</span><span class="sxs-lookup"><span data-stu-id="b7bba-103">Configuration builders for ASP.NET</span></span>

<span data-ttu-id="b7bba-104">Por [Stephen Molloy](https://github.com/StephenMolloy) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b7bba-104">By [Stephen Molloy](https://github.com/StephenMolloy) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b7bba-105">Os construtores de configuração fornecem um mecanismo moderno e ágil para aplicativos ASP.NET obterem valores de configuração de fontes externas.</span><span class="sxs-lookup"><span data-stu-id="b7bba-105">Configuration builders provide a modern and agile mechanism for ASP.NET apps to get configuration values from external sources.</span></span>

<span data-ttu-id="b7bba-106">Construtores de configuração:</span><span class="sxs-lookup"><span data-stu-id="b7bba-106">Configuration builders:</span></span>

* <span data-ttu-id="b7bba-107">Estão disponíveis no .NET Framework 4.7.1 e versões posteriores.</span><span class="sxs-lookup"><span data-stu-id="b7bba-107">Are available in .NET Framework 4.7.1 and later.</span></span>
* <span data-ttu-id="b7bba-108">Forneça um mecanismo flexível para ler valores de configuração.</span><span class="sxs-lookup"><span data-stu-id="b7bba-108">Provide a flexible mechanism for reading configuration values.</span></span>
* <span data-ttu-id="b7bba-109">Resolva algumas das necessidades básicas dos aplicativos à medida que eles se movem para um ambiente voltado para a nuvem e um contêiner.</span><span class="sxs-lookup"><span data-stu-id="b7bba-109">Address some of the basic needs of apps as they move into a container and cloud focused environment.</span></span>
* <span data-ttu-id="b7bba-110">Pode ser usado para melhorar a proteção de dados de configuração por meio do desenho de fontes anteriormente indisponíveis (por exemplo, Azure Key Vault e variáveis de ambiente) no sistema de configuração do .NET.</span><span class="sxs-lookup"><span data-stu-id="b7bba-110">Can be used to improve protection of configuration data by drawing from sources previously unavailable (for example, Azure Key Vault and environment variables) in the .NET configuration system.</span></span>

## <a name="keyvalue-configuration-builders"></a><span data-ttu-id="b7bba-111">Construtores de configuração de chave/valor</span><span class="sxs-lookup"><span data-stu-id="b7bba-111">Key/value configuration builders</span></span>

<span data-ttu-id="b7bba-112">Um cenário comum que pode ser tratado por construtores de configuração é fornecer um mecanismo básico de substituição de chave/valor para seções de configuração que seguem um padrão de chave/valor.</span><span class="sxs-lookup"><span data-stu-id="b7bba-112">A common scenario that can be handled by configuration builders is to provide a basic key/value replacement mechanism for configuration sections that follow a key/value pattern.</span></span> <span data-ttu-id="b7bba-113">O conceito de .NET Framework de ConfigurationBuilders não é limitado a seções ou padrões de configuração específicos.</span><span class="sxs-lookup"><span data-stu-id="b7bba-113">The .NET Framework concept of ConfigurationBuilders is not limited to specific configuration sections or patterns.</span></span> <span data-ttu-id="b7bba-114">No entanto, muitos dos construtores de configuração no `Microsoft.Configuration.ConfigurationBuilders` ([GitHub](https://github.com/aspnet/MicrosoftConfigurationBuilders), [NuGet](https://www.nuget.org/packages?q=Microsoft.Configuration.ConfigurationBuilders)) funcionam dentro do padrão de chave/valor.</span><span class="sxs-lookup"><span data-stu-id="b7bba-114">However, many of the configuration builders in `Microsoft.Configuration.ConfigurationBuilders` ([github](https://github.com/aspnet/MicrosoftConfigurationBuilders), [NuGet](https://www.nuget.org/packages?q=Microsoft.Configuration.ConfigurationBuilders)) work within the key/value pattern.</span></span>

## <a name="keyvalue-configuration-builders-settings"></a><span data-ttu-id="b7bba-115">Configurações de construtores de configuração de chave/valor</span><span class="sxs-lookup"><span data-stu-id="b7bba-115">Key/value configuration builders settings</span></span>

<span data-ttu-id="b7bba-116">As configurações a seguir se aplicam a todos os construtores de configuração de chave/valor no `Microsoft.Configuration.ConfigurationBuilders` .</span><span class="sxs-lookup"><span data-stu-id="b7bba-116">The following settings apply to all key/value configuration builders in `Microsoft.Configuration.ConfigurationBuilders`.</span></span>

### <a name="mode"></a><span data-ttu-id="b7bba-117">Modo</span><span class="sxs-lookup"><span data-stu-id="b7bba-117">Mode</span></span>

<span data-ttu-id="b7bba-118">Os construtores de configuração usam uma fonte externa de informações de chave/valor para preencher os elementos de chave/valor selecionados do sistema de configuração.</span><span class="sxs-lookup"><span data-stu-id="b7bba-118">The configuration builders use an external source of key/value information to populate selected key/value elements of the configuration system.</span></span> <span data-ttu-id="b7bba-119">Especificamente, as `<appSettings/>` `<connectionStrings/>` seções e recebem tratamento especial dos criadores de configuração.</span><span class="sxs-lookup"><span data-stu-id="b7bba-119">Specifically, the `<appSettings/>` and `<connectionStrings/>` sections receive special treatment from the configuration builders.</span></span> <span data-ttu-id="b7bba-120">Os criadores funcionam em três modos:</span><span class="sxs-lookup"><span data-stu-id="b7bba-120">The builders work in three modes:</span></span>

* <span data-ttu-id="b7bba-121">`Strict` -O modo padrão.</span><span class="sxs-lookup"><span data-stu-id="b7bba-121">`Strict` - The default mode.</span></span> <span data-ttu-id="b7bba-122">Nesse modo, o construtor de configuração opera apenas em seções de configuração bem conhecidas de chave/valor centrado.</span><span class="sxs-lookup"><span data-stu-id="b7bba-122">In this mode, the configuration builder only operates on well-known key/value-centric configuration sections.</span></span> <span data-ttu-id="b7bba-123">`Strict` o modo enumera cada chave na seção.</span><span class="sxs-lookup"><span data-stu-id="b7bba-123">`Strict` mode enumerates each key in the section.</span></span> <span data-ttu-id="b7bba-124">Se uma chave correspondente for encontrada na fonte externa:</span><span class="sxs-lookup"><span data-stu-id="b7bba-124">If a matching key is found in the external source:</span></span>

   * <span data-ttu-id="b7bba-125">Os construtores de configuração substituem o valor na seção de configuração resultante pelo valor da fonte externa.</span><span class="sxs-lookup"><span data-stu-id="b7bba-125">The configuration builders replace the value in the resulting configuration section with the value from the external source.</span></span>
* <span data-ttu-id="b7bba-126">`Greedy` -Esse modo está fortemente relacionado ao `Strict` modo.</span><span class="sxs-lookup"><span data-stu-id="b7bba-126">`Greedy` - This mode is closely related to `Strict` mode.</span></span> <span data-ttu-id="b7bba-127">Em vez de ser limitado a chaves que já existem na configuração original:</span><span class="sxs-lookup"><span data-stu-id="b7bba-127">Rather than being limited to keys that already exist in the original configuration:</span></span>

  * <span data-ttu-id="b7bba-128">Os construtores de configuração adicionam todos os pares de chave/valor da fonte externa na seção de configuração resultante.</span><span class="sxs-lookup"><span data-stu-id="b7bba-128">The configuration builders adds all key/value pairs from the external source into the resulting configuration section.</span></span>

* <span data-ttu-id="b7bba-129">`Expand` -Opera no XML bruto antes de ser analisado em um objeto da seção de configuração.</span><span class="sxs-lookup"><span data-stu-id="b7bba-129">`Expand` - Operates on the raw XML before it's parsed into a configuration section object.</span></span> <span data-ttu-id="b7bba-130">Pode ser pensado como uma expansão de tokens em uma cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="b7bba-130">It can be thought of as an expansion of tokens in a string.</span></span> <span data-ttu-id="b7bba-131">Qualquer parte da cadeia de caracteres XML bruta que corresponda ao padrão `${token}` é um candidato para a expansão do token.</span><span class="sxs-lookup"><span data-stu-id="b7bba-131">Any part of the raw XML string that matches the pattern `${token}` is a candidate for token expansion.</span></span> <span data-ttu-id="b7bba-132">Se nenhum valor correspondente for encontrado na origem externa, o token não será alterado.</span><span class="sxs-lookup"><span data-stu-id="b7bba-132">If no corresponding value is found in the external source, then the token is not changed.</span></span> <span data-ttu-id="b7bba-133">Os construtores neste modo não são limitados às `<appSettings/>` `<connectionStrings/>` seções e.</span><span class="sxs-lookup"><span data-stu-id="b7bba-133">Builders in this mode are not limited to the `<appSettings/>` and `<connectionStrings/>` sections.</span></span>

<span data-ttu-id="b7bba-134">A marcação a seguir de *web.config* habilita o [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/) no `Strict` modo:</span><span class="sxs-lookup"><span data-stu-id="b7bba-134">The following markup from *web.config* enables the [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/) in `Strict` mode:</span></span>

[!code-xml[Main](config-builder/MyConfigBuilders/WebDefault.config?name=snippet)]

<span data-ttu-id="b7bba-135">O código a seguir lê o `<appSettings/>` e `<connectionStrings/>` mostrado no arquivo *web.config* anterior:</span><span class="sxs-lookup"><span data-stu-id="b7bba-135">The following code reads the `<appSettings/>` and `<connectionStrings/>` shown in the preceding *web.config* file:</span></span>

[!code-csharp[Main](config-builder/MyConfigBuilders/About.aspx.cs)]

<span data-ttu-id="b7bba-136">O código anterior definirá os valores de propriedade como:</span><span class="sxs-lookup"><span data-stu-id="b7bba-136">The preceding code will set the property values to:</span></span>

* <span data-ttu-id="b7bba-137">Os valores no arquivo de *web.config* se as chaves não estiverem definidas em variáveis de ambiente.</span><span class="sxs-lookup"><span data-stu-id="b7bba-137">The values in the *web.config* file if the keys are not set in environment variables.</span></span>
* <span data-ttu-id="b7bba-138">Os valores da variável de ambiente, se definido.</span><span class="sxs-lookup"><span data-stu-id="b7bba-138">The values of the environment variable, if set.</span></span>

<span data-ttu-id="b7bba-139">Por exemplo, `ServiceID` conterá:</span><span class="sxs-lookup"><span data-stu-id="b7bba-139">For example, `ServiceID` will contain:</span></span>

* <span data-ttu-id="b7bba-140">"Valor de ServiceId de web.config", se a variável de ambiente `ServiceID` não estiver definida.</span><span class="sxs-lookup"><span data-stu-id="b7bba-140">"ServiceID value from web.config", if the environment variable `ServiceID` is not set.</span></span>
* <span data-ttu-id="b7bba-141">O valor da `ServiceID` variável de ambiente, se definido.</span><span class="sxs-lookup"><span data-stu-id="b7bba-141">The value of the `ServiceID` environment variable, if set.</span></span>

<span data-ttu-id="b7bba-142">A imagem a seguir mostra as `<appSettings/>` chaves/valores do arquivo de *web.config* anterior definido no editor de ambiente:</span><span class="sxs-lookup"><span data-stu-id="b7bba-142">The following image shows the `<appSettings/>` keys/values from the preceding *web.config* file set in the environment editor:</span></span>

![Editor de ambiente](config-builder/static/env.png)

<span data-ttu-id="b7bba-144">Observação: Talvez seja necessário sair e reiniciar o Visual Studio para ver as alterações nas variáveis de ambiente.</span><span class="sxs-lookup"><span data-stu-id="b7bba-144">Note: You might need to exit and restart Visual Studio to see changes in environment variables.</span></span>

### <a name="prefix-handling"></a><span data-ttu-id="b7bba-145">Manipulação de prefixo</span><span class="sxs-lookup"><span data-stu-id="b7bba-145">Prefix handling</span></span>

<span data-ttu-id="b7bba-146">Os prefixos de chave podem simplificar a configuração de chaves porque:</span><span class="sxs-lookup"><span data-stu-id="b7bba-146">Key prefixes can simplify setting keys because:</span></span>

* <span data-ttu-id="b7bba-147">A configuração de .NET Framework é complexa e aninhada.</span><span class="sxs-lookup"><span data-stu-id="b7bba-147">The .NET Framework configuration is complex and nested.</span></span>
* <span data-ttu-id="b7bba-148">As origens de chave/valor externas são normalmente básicas e planas por natureza.</span><span class="sxs-lookup"><span data-stu-id="b7bba-148">External key/value sources are commonly basic and flat by nature.</span></span> <span data-ttu-id="b7bba-149">Por exemplo, variáveis de ambiente não são aninhadas.</span><span class="sxs-lookup"><span data-stu-id="b7bba-149">For example, environment variables are not nested.</span></span>

<span data-ttu-id="b7bba-150">Use qualquer uma das seguintes abordagens para injetar ambos `<appSettings/>` e `<connectionStrings/>` na configuração por meio de variáveis de ambiente:</span><span class="sxs-lookup"><span data-stu-id="b7bba-150">Use any of the following approaches to inject both `<appSettings/>` and `<connectionStrings/>` into the configuration via environment variables:</span></span>

* <span data-ttu-id="b7bba-151">Com o `EnvironmentConfigBuilder` no modo padrão `Strict` e os nomes de chave apropriados no arquivo de configuração.</span><span class="sxs-lookup"><span data-stu-id="b7bba-151">With the `EnvironmentConfigBuilder` in the default `Strict` mode and the appropriate key names in the configuration file.</span></span> <span data-ttu-id="b7bba-152">O código e a marcação anteriores assumem essa abordagem.</span><span class="sxs-lookup"><span data-stu-id="b7bba-152">The preceding code and markup takes this approach.</span></span> <span data-ttu-id="b7bba-153">Usando essa abordagem, você **não** pode ter chaves nomeadas de forma idêntica no `<appSettings/>` e no `<connectionStrings/>` .</span><span class="sxs-lookup"><span data-stu-id="b7bba-153">Using this approach you can **not** have identically named keys in both `<appSettings/>` and `<connectionStrings/>`.</span></span>
* <span data-ttu-id="b7bba-154">Use dois `EnvironmentConfigBuilder` s no `Greedy` modo com prefixos distintos e `stripPrefix` .</span><span class="sxs-lookup"><span data-stu-id="b7bba-154">Use two `EnvironmentConfigBuilder`s in `Greedy` mode with distinct prefixes and `stripPrefix`.</span></span> <span data-ttu-id="b7bba-155">Com essa abordagem, o aplicativo pode ler `<appSettings/>` e `<connectionStrings/>` sem a necessidade de atualizar o arquivo de configuração.</span><span class="sxs-lookup"><span data-stu-id="b7bba-155">With this approach, the app can read `<appSettings/>` and `<connectionStrings/>` without needing to update the configuration file.</span></span> <span data-ttu-id="b7bba-156">A próxima seção,  [stripPrefix](#stripprefix), mostra como fazer isso.</span><span class="sxs-lookup"><span data-stu-id="b7bba-156">The next section,  [stripPrefix](#stripprefix),  shows how to do this.</span></span>
* <span data-ttu-id="b7bba-157">Use dois `EnvironmentConfigBuilder` s no `Greedy` modo com prefixos distintos.</span><span class="sxs-lookup"><span data-stu-id="b7bba-157">Use two `EnvironmentConfigBuilder`s in `Greedy` mode with distinct prefixes.</span></span> <span data-ttu-id="b7bba-158">Com essa abordagem, você não pode ter nomes de chave duplicados, pois nomes de chave devem ser diferentes por prefixo.</span><span class="sxs-lookup"><span data-stu-id="b7bba-158">With this approach you can't have duplicate key names as key names must differ by prefix.</span></span>  <span data-ttu-id="b7bba-159">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="b7bba-159">For example:</span></span>

[!code-xml[Main](config-builder/MyConfigBuilders/WebPrefix.config?name=snippet&highlight=11-99)]

<span data-ttu-id="b7bba-160">Com a marcação anterior, a mesma fonte de chave/valor simples pode ser usada para preencher a configuração de duas seções diferentes.</span><span class="sxs-lookup"><span data-stu-id="b7bba-160">With the preceding markup, the same flat key/value source can be used to populate configuration for two different sections.</span></span>

<span data-ttu-id="b7bba-161">A imagem a seguir mostra `<appSettings/>` as `<connectionStrings/>` chaves e os valores do arquivo de *web.config* anterior definido no editor de ambiente:</span><span class="sxs-lookup"><span data-stu-id="b7bba-161">The following image shows the `<appSettings/>` and `<connectionStrings/>` keys/values from the preceding *web.config* file set in the environment editor:</span></span>

![Editor de ambiente](config-builder/static/prefix.png)

<span data-ttu-id="b7bba-163">O código a seguir lê `<appSettings/>` as `<connectionStrings/>` chaves e os valores contidos no arquivo de *web.config* anterior:</span><span class="sxs-lookup"><span data-stu-id="b7bba-163">The following code reads the `<appSettings/>` and `<connectionStrings/>` keys/values contained in the preceding *web.config* file:</span></span>

[!code-csharp[Main](config-builder/MyConfigBuilders/Contact.aspx.cs?name=snippet)]

<span data-ttu-id="b7bba-164">O código anterior definirá os valores de propriedade como:</span><span class="sxs-lookup"><span data-stu-id="b7bba-164">The preceding code will set the property values to:</span></span>

* <span data-ttu-id="b7bba-165">Os valores no arquivo de *web.config* se as chaves não estiverem definidas em variáveis de ambiente.</span><span class="sxs-lookup"><span data-stu-id="b7bba-165">The values in the *web.config* file if the keys are not set in environment variables.</span></span>
* <span data-ttu-id="b7bba-166">Os valores da variável de ambiente, se definido.</span><span class="sxs-lookup"><span data-stu-id="b7bba-166">The values of the environment variable, if set.</span></span>

<span data-ttu-id="b7bba-167">Por exemplo, usando o arquivo de *web.config* anterior, as chaves/valores na imagem do editor de ambiente anterior e o código anterior, os seguintes valores são definidos:</span><span class="sxs-lookup"><span data-stu-id="b7bba-167">For example, using the previous *web.config* file, the keys/values in the previous  environment editor image, and the previous code, the following values are set:</span></span>

|  <span data-ttu-id="b7bba-168">Chave</span><span class="sxs-lookup"><span data-stu-id="b7bba-168">Key</span></span>              | <span data-ttu-id="b7bba-169">Valor</span><span class="sxs-lookup"><span data-stu-id="b7bba-169">Value</span></span> |
| ----------------- | ------------ |
|     <span data-ttu-id="b7bba-170">AppSetting_ServiceID</span><span class="sxs-lookup"><span data-stu-id="b7bba-170">AppSetting_ServiceID</span></span>           | <span data-ttu-id="b7bba-171">AppSetting_ServiceID de variáveis ENV</span><span class="sxs-lookup"><span data-stu-id="b7bba-171">AppSetting_ServiceID from env variables</span></span>|
|    <span data-ttu-id="b7bba-172">AppSetting_default</span><span class="sxs-lookup"><span data-stu-id="b7bba-172">AppSetting_default</span></span>            | <span data-ttu-id="b7bba-173">AppSetting_default valor de env</span><span class="sxs-lookup"><span data-stu-id="b7bba-173">AppSetting_default value from env</span></span> |
|       <span data-ttu-id="b7bba-174">ConnStr_default</span><span class="sxs-lookup"><span data-stu-id="b7bba-174">ConnStr_default</span></span>         | <span data-ttu-id="b7bba-175">ConnStr_default Val de env</span><span class="sxs-lookup"><span data-stu-id="b7bba-175">ConnStr_default val from env</span></span>|

### <a name="stripprefix"></a><span data-ttu-id="b7bba-176">stripPrefix</span><span class="sxs-lookup"><span data-stu-id="b7bba-176">stripPrefix</span></span>

<span data-ttu-id="b7bba-177">`stripPrefix`: booliano, o padrão é `false` .</span><span class="sxs-lookup"><span data-stu-id="b7bba-177">`stripPrefix`: boolean, defaults to `false`.</span></span> 

<span data-ttu-id="b7bba-178">A marcação XML anterior separa as configurações do aplicativo das cadeias de conexão, mas requer que todas as chaves no arquivo de *web.config* usem o prefixo especificado.</span><span class="sxs-lookup"><span data-stu-id="b7bba-178">The preceding XML markup separates app settings from connection strings but requires all the keys in the *web.config* file to use the specified prefix.</span></span> <span data-ttu-id="b7bba-179">Por exemplo, o prefixo `AppSetting` deve ser adicionado à `ServiceID` chave ("AppSetting_ServiceID").</span><span class="sxs-lookup"><span data-stu-id="b7bba-179">For example, the prefix `AppSetting` must be added to the `ServiceID` key ("AppSetting_ServiceID").</span></span> <span data-ttu-id="b7bba-180">Com `stripPrefix` o, o prefixo não é usado no arquivo de *web.config* .</span><span class="sxs-lookup"><span data-stu-id="b7bba-180">With `stripPrefix`, the prefix is not used in the *web.config* file.</span></span> <span data-ttu-id="b7bba-181">O prefixo é necessário na origem do Configuration Builder (por exemplo, no ambiente). Prevemos que a maioria dos desenvolvedores usará `stripPrefix` .</span><span class="sxs-lookup"><span data-stu-id="b7bba-181">The prefix is required in the configuration builder source (for example, in the environment.) We anticipate most developers will use `stripPrefix`.</span></span>

<span data-ttu-id="b7bba-182">Normalmente, os aplicativos tiram o prefixo.</span><span class="sxs-lookup"><span data-stu-id="b7bba-182">Applications typically strip off the prefix.</span></span> <span data-ttu-id="b7bba-183">O *web.config* a seguir remove o prefixo:</span><span class="sxs-lookup"><span data-stu-id="b7bba-183">The following *web.config* strips the prefix:</span></span>

[!code-xml[Main](config-builder/MyConfigBuilders/WebPrefixStrip.config?name=snippet&highlight=14,19)]

<span data-ttu-id="b7bba-184">No arquivo *web.config* anterior, a `default` chave está no `<appSettings/>` e no `<connectionStrings/>` .</span><span class="sxs-lookup"><span data-stu-id="b7bba-184">In the preceding *web.config* file, the `default` key is in both the `<appSettings/>` and `<connectionStrings/>`.</span></span>

<span data-ttu-id="b7bba-185">A imagem a seguir mostra `<appSettings/>` as `<connectionStrings/>` chaves e os valores do arquivo de *web.config* anterior definido no editor de ambiente:</span><span class="sxs-lookup"><span data-stu-id="b7bba-185">The following image shows the `<appSettings/>` and `<connectionStrings/>` keys/values from the preceding *web.config* file set in the environment editor:</span></span>

![Editor de ambiente](config-builder/static/prefix.png)

<span data-ttu-id="b7bba-187">O código a seguir lê `<appSettings/>` as `<connectionStrings/>` chaves e os valores contidos no arquivo de *web.config* anterior:</span><span class="sxs-lookup"><span data-stu-id="b7bba-187">The following code reads the `<appSettings/>` and `<connectionStrings/>` keys/values contained in the preceding *web.config* file:</span></span>

[!code-csharp[Main](config-builder/MyConfigBuilders/About2.aspx.cs?name=snippet)]

<span data-ttu-id="b7bba-188">O código anterior definirá os valores de propriedade como:</span><span class="sxs-lookup"><span data-stu-id="b7bba-188">The preceding code will set the property values to:</span></span>

* <span data-ttu-id="b7bba-189">Os valores no arquivo de *web.config* se as chaves não estiverem definidas em variáveis de ambiente.</span><span class="sxs-lookup"><span data-stu-id="b7bba-189">The values in the *web.config* file if the keys are not set in environment variables.</span></span>
* <span data-ttu-id="b7bba-190">Os valores da variável de ambiente, se definido.</span><span class="sxs-lookup"><span data-stu-id="b7bba-190">The values of the environment variable, if set.</span></span>

<span data-ttu-id="b7bba-191">Por exemplo, usando o arquivo de *web.config* anterior, as chaves/valores na imagem do editor de ambiente anterior e o código anterior, os seguintes valores são definidos:</span><span class="sxs-lookup"><span data-stu-id="b7bba-191">For example, using the previous *web.config* file, the keys/values in the previous  environment editor image, and the previous code, the following values are set:</span></span>

|  <span data-ttu-id="b7bba-192">Chave</span><span class="sxs-lookup"><span data-stu-id="b7bba-192">Key</span></span>              | <span data-ttu-id="b7bba-193">Valor</span><span class="sxs-lookup"><span data-stu-id="b7bba-193">Value</span></span> |
| ----------------- | ------------ |
|     <span data-ttu-id="b7bba-194">ServiceID</span><span class="sxs-lookup"><span data-stu-id="b7bba-194">ServiceID</span></span>           | <span data-ttu-id="b7bba-195">AppSetting_ServiceID de variáveis ENV</span><span class="sxs-lookup"><span data-stu-id="b7bba-195">AppSetting_ServiceID from env variables</span></span>|
|    <span data-ttu-id="b7bba-196">padrão</span><span class="sxs-lookup"><span data-stu-id="b7bba-196">default</span></span>            | <span data-ttu-id="b7bba-197">AppSetting_default valor de env</span><span class="sxs-lookup"><span data-stu-id="b7bba-197">AppSetting_default value from env</span></span> |
|    <span data-ttu-id="b7bba-198">padrão</span><span class="sxs-lookup"><span data-stu-id="b7bba-198">default</span></span>         | <span data-ttu-id="b7bba-199">ConnStr_default Val de env</span><span class="sxs-lookup"><span data-stu-id="b7bba-199">ConnStr_default val from env</span></span>|

### <a name="tokenpattern"></a><span data-ttu-id="b7bba-200">tokenPattern</span><span class="sxs-lookup"><span data-stu-id="b7bba-200">tokenPattern</span></span>

<span data-ttu-id="b7bba-201">`tokenPattern`: Cadeia de caracteres, o padrão é `@"\$\{(\w+)\}"`</span><span class="sxs-lookup"><span data-stu-id="b7bba-201">`tokenPattern`: String, defaults to `@"\$\{(\w+)\}"`</span></span>

<span data-ttu-id="b7bba-202">O `Expand` comportamento dos construtores pesquisa no XML bruto os tokens semelhantes `${token}` .</span><span class="sxs-lookup"><span data-stu-id="b7bba-202">The `Expand` behavior of the builders searches the raw XML for tokens that look like `${token}`.</span></span> <span data-ttu-id="b7bba-203">A pesquisa é feita com a expressão regular padrão `@"\$\{(\w+)\}"` .</span><span class="sxs-lookup"><span data-stu-id="b7bba-203">Searching is done with the default regular expression `@"\$\{(\w+)\}"`.</span></span> <span data-ttu-id="b7bba-204">O conjunto de caracteres que corresponde `\w` é mais estrito do que o XML e muitas fontes de configuração permitem.</span><span class="sxs-lookup"><span data-stu-id="b7bba-204">The set of characters that matches `\w` is more strict than XML and many configuration sources allow.</span></span> <span data-ttu-id="b7bba-205">Use `tokenPattern` quando houver mais caracteres do que `@"\$\{(\w+)\}"` o necessário no nome do token.</span><span class="sxs-lookup"><span data-stu-id="b7bba-205">Use `tokenPattern` when more characters than `@"\$\{(\w+)\}"` are required in the token name.</span></span>

<span data-ttu-id="b7bba-206">`tokenPattern`Strings</span><span class="sxs-lookup"><span data-stu-id="b7bba-206">`tokenPattern`: String:</span></span>

* <span data-ttu-id="b7bba-207">Permite que os desenvolvedores alterem o Regex que é usado para correspondência de token.</span><span class="sxs-lookup"><span data-stu-id="b7bba-207">Allows developers to change the regex that is used for token matching.</span></span>
* <span data-ttu-id="b7bba-208">Nenhuma validação é feita para garantir que se trata de um Regex bem formado e não perigoso.</span><span class="sxs-lookup"><span data-stu-id="b7bba-208">No validation is done to make sure it is a well-formed, non-dangerous regex.</span></span>
* <span data-ttu-id="b7bba-209">Ele deve conter um grupo de captura.</span><span class="sxs-lookup"><span data-stu-id="b7bba-209">It must contain a capture group.</span></span> <span data-ttu-id="b7bba-210">O Regex inteiro deve corresponder ao token inteiro.</span><span class="sxs-lookup"><span data-stu-id="b7bba-210">The entire regex must match the entire token.</span></span> <span data-ttu-id="b7bba-211">A primeira captura deve ser o nome do token a ser pesquisado na fonte de configuração.</span><span class="sxs-lookup"><span data-stu-id="b7bba-211">The first capture must be the token name to look up in the configuration source.</span></span>

## <a name="configuration-builders-in-microsoftconfigurationconfigurationbuilders"></a><span data-ttu-id="b7bba-212">Construtores de configuração no Microsoft.Configuration.ConfigurationBuilders</span><span class="sxs-lookup"><span data-stu-id="b7bba-212">Configuration builders in Microsoft.Configuration.ConfigurationBuilders</span></span>

### <a name="environmentconfigbuilder"></a><span data-ttu-id="b7bba-213">EnvironmentConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="b7bba-213">EnvironmentConfigBuilder</span></span>

```xml
<add name="Environment"
    [mode|prefix|stripPrefix|tokenPattern] 
    type="Microsoft.Configuration.ConfigurationBuilders.EnvironmentConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Environment" />
```

<span data-ttu-id="b7bba-214">O [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/):</span><span class="sxs-lookup"><span data-stu-id="b7bba-214">The [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/):</span></span>

* <span data-ttu-id="b7bba-215">É a mais simples dos construtores de configuração.</span><span class="sxs-lookup"><span data-stu-id="b7bba-215">Is the simplest of the configuration builders.</span></span>
* <span data-ttu-id="b7bba-216">Lê os valores do ambiente.</span><span class="sxs-lookup"><span data-stu-id="b7bba-216">Reads values from the environment.</span></span>
* <span data-ttu-id="b7bba-217">Não tem nenhuma opção de configuração adicional.</span><span class="sxs-lookup"><span data-stu-id="b7bba-217">Does not have any additional configuration options.</span></span>
* <span data-ttu-id="b7bba-218">O `name` valor do atributo é arbitrário.</span><span class="sxs-lookup"><span data-stu-id="b7bba-218">The `name` attribute value is arbitrary.</span></span>

<span data-ttu-id="b7bba-219">**Observação:** Em um ambiente de contêiner do Windows, as variáveis definidas em tempo de execução são injetadas apenas no ambiente de processo EntryPoint.</span><span class="sxs-lookup"><span data-stu-id="b7bba-219">**Note:** In a Windows container environment, variables set at run time are only injected into the EntryPoint process environment.</span></span> <span data-ttu-id="b7bba-220">Os aplicativos executados como um serviço ou um processo não-EntryPoint não pegam essas variáveis, a menos que eles sejam injetados por um mecanismo no contêiner.</span><span class="sxs-lookup"><span data-stu-id="b7bba-220">Apps that run as a service or a non-EntryPoint process do not pick up these variables unless they are otherwise injected through a mechanism in the container.</span></span> <span data-ttu-id="b7bba-221">Para [](https://github.com/Microsoft/iis-docker/pull/41) / contêineres baseados em [ASP.net](https://github.com/Microsoft/aspnet-docker)do IIS, a versão atual do [ServiceMonitor.exe](https://github.com/Microsoft/iis-docker/pull/41) trata isso somente no *DefaultAppPool* .</span><span class="sxs-lookup"><span data-stu-id="b7bba-221">For [IIS](https://github.com/Microsoft/iis-docker/pull/41)/[ASP.NET](https://github.com/Microsoft/aspnet-docker)-based containers, the current version of [ServiceMonitor.exe](https://github.com/Microsoft/iis-docker/pull/41) handles this in the *DefaultAppPool* only.</span></span> <span data-ttu-id="b7bba-222">Outras variantes de contêiner com base no Windows talvez precisem desenvolver seu próprio mecanismo de injeção para processos não EntryPoint.</span><span class="sxs-lookup"><span data-stu-id="b7bba-222">Other Windows-based container variants may need to develop their own injection mechanism for non-EntryPoint processes.</span></span>

### <a name="usersecretsconfigbuilder"></a><span data-ttu-id="b7bba-223">UserSecretsConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="b7bba-223">UserSecretsConfigBuilder</span></span>

> [!WARNING]
> <span data-ttu-id="b7bba-224">Nunca armazene senhas, cadeias de conexão confidenciais ou outros dados confidenciais no código-fonte.</span><span class="sxs-lookup"><span data-stu-id="b7bba-224">Never store passwords, sensitive connection strings, or other sensitive data in source code.</span></span> <span data-ttu-id="b7bba-225">Os segredos de produção não devem ser usados para desenvolvimento ou teste.</span><span class="sxs-lookup"><span data-stu-id="b7bba-225">Production secrets should not be used for development or test.</span></span>

```xml
<add name="UserSecrets"
    [mode|prefix|stripPrefix|tokenPattern]
    (userSecretsId="{secret string, typically a GUID}" | userSecretsFile="~\secrets.file")
    [optional="true"]
    type="Microsoft.Configuration.ConfigurationBuilders.UserSecretsConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.UserSecrets" />
```

<span data-ttu-id="b7bba-226">Este construtor de configuração fornece um recurso semelhante a [ASP.NET Core o Gerenciador de segredo](/aspnet/core/security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="b7bba-226">This configuration builder provides a feature similar to [ASP.NET Core Secret Manager](/aspnet/core/security/app-secrets).</span></span>

<span data-ttu-id="b7bba-227">O [UserSecretsConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.UserSecrets/) pode ser usado em projetos .NET Framework, mas um arquivo de segredos deve ser especificado.</span><span class="sxs-lookup"><span data-stu-id="b7bba-227">The [UserSecretsConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.UserSecrets/) can be used in .NET Framework projects, but a secrets file must be specified.</span></span> <span data-ttu-id="b7bba-228">Como alternativa, você pode definir a `UserSecretsId` propriedade no arquivo de projeto e criar o arquivo de segredos brutos no local correto para leitura.</span><span class="sxs-lookup"><span data-stu-id="b7bba-228">Alternatively, you can define the `UserSecretsId` property in the project file and create the raw secrets file in the correct location for reading.</span></span> <span data-ttu-id="b7bba-229">Para manter as dependências externas do seu projeto, o arquivo secreto é XML formatado.</span><span class="sxs-lookup"><span data-stu-id="b7bba-229">To keep external dependencies out of your project, the secret file is XML formatted.</span></span> <span data-ttu-id="b7bba-230">A formatação XML é um detalhe de implementação e o formato não deve ser confiado.</span><span class="sxs-lookup"><span data-stu-id="b7bba-230">The XML formatting is an implementation detail, and the format should not be relied upon.</span></span> <span data-ttu-id="b7bba-231">Se você precisar compartilhar um *secrets.jsno* arquivo com projetos do .NET Core, considere o uso de [SimpleJsonConfigBuilder](#simplejsonconfigbuilder).</span><span class="sxs-lookup"><span data-stu-id="b7bba-231">If you need to share a *secrets.json* file with .NET Core projects, consider using the [SimpleJsonConfigBuilder](#simplejsonconfigbuilder).</span></span> <span data-ttu-id="b7bba-232">O `SimpleJsonConfigBuilder` formato do .NET Core também deve ser considerado um detalhe de implementação sujeito a alterações.</span><span class="sxs-lookup"><span data-stu-id="b7bba-232">The `SimpleJsonConfigBuilder` format for .NET Core should also be considered an implementation detail subject to change.</span></span>

<span data-ttu-id="b7bba-233">Atributos de configuração para `UserSecretsConfigBuilder` :</span><span class="sxs-lookup"><span data-stu-id="b7bba-233">Configuration attributes for `UserSecretsConfigBuilder`:</span></span>

* <span data-ttu-id="b7bba-234">`userSecretsId` -Esse é o método preferencial para identificar um arquivo de segredos XML.</span><span class="sxs-lookup"><span data-stu-id="b7bba-234">`userSecretsId` - This is the preferred method for identifying an XML secrets file.</span></span> <span data-ttu-id="b7bba-235">Ele funciona semelhante ao .NET Core, que usa uma `UserSecretsId` propriedade de projeto para armazenar esse identificador.</span><span class="sxs-lookup"><span data-stu-id="b7bba-235">It works similar to .NET Core, which uses a `UserSecretsId` project property to store this identifier.</span></span> <span data-ttu-id="b7bba-236">A cadeia de caracteres deve ser exclusiva, não precisa ser uma GUID.</span><span class="sxs-lookup"><span data-stu-id="b7bba-236">The string must be unique, it doesn't need to be a GUID.</span></span> <span data-ttu-id="b7bba-237">Com esse atributo, a `UserSecretsConfigBuilder` aparência de um local local conhecido ( `%APPDATA%\Microsoft\UserSecrets\<UserSecrets Id>\secrets.xml` ) para um arquivo de segredos que pertence a esse identificador.</span><span class="sxs-lookup"><span data-stu-id="b7bba-237">With this attribute, the `UserSecretsConfigBuilder` look in a well-known local location (`%APPDATA%\Microsoft\UserSecrets\<UserSecrets Id>\secrets.xml`) for a secrets file belonging to this identifier.</span></span>
* <span data-ttu-id="b7bba-238">`userSecretsFile` -Um atributo opcional que especifica o arquivo que contém os segredos.</span><span class="sxs-lookup"><span data-stu-id="b7bba-238">`userSecretsFile` - An optional attribute specifying the file containing the secrets.</span></span> <span data-ttu-id="b7bba-239">O `~` caractere pode ser usado no início para fazer referência à raiz do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="b7bba-239">The `~` character can be used at the start to reference the application root.</span></span> <span data-ttu-id="b7bba-240">O atributo ou o `userSecretsId` atributo é necessário.</span><span class="sxs-lookup"><span data-stu-id="b7bba-240">Either this attribute or the `userSecretsId` attribute is required.</span></span> <span data-ttu-id="b7bba-241">Se ambos forem especificados, `userSecretsFile` terá precedência.</span><span class="sxs-lookup"><span data-stu-id="b7bba-241">If both are specified, `userSecretsFile` takes precedence.</span></span>
* <span data-ttu-id="b7bba-242">`optional`: booliano, valor padrão `true` -impede uma exceção se o arquivo de segredos não puder ser encontrado.</span><span class="sxs-lookup"><span data-stu-id="b7bba-242">`optional`: boolean, default value `true` - Prevents an exception if the secrets file cannot be found.</span></span> 
* <span data-ttu-id="b7bba-243">O `name` valor do atributo é arbitrário.</span><span class="sxs-lookup"><span data-stu-id="b7bba-243">The `name` attribute value is arbitrary.</span></span>

<span data-ttu-id="b7bba-244">O arquivo de segredos tem o seguinte formato:</span><span class="sxs-lookup"><span data-stu-id="b7bba-244">The secrets file has the following format:</span></span>

```xml
<?xml version="1.0" encoding="utf-8" ?>
<root>
  <secrets ver="1.0">
    <secret name="secret key name" value="secret value" />
  </secrets>
</root>
```

### <a name="azurekeyvaultconfigbuilder"></a><span data-ttu-id="b7bba-245">AzureKeyVaultConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="b7bba-245">AzureKeyVaultConfigBuilder</span></span>

```xml
<add name="AzureKeyVault"
    [mode|prefix|stripPrefix|tokenPattern]
    (vaultName="MyVaultName" |
     uri="https:/MyVaultName.vault.azure.net")
    [version="secrets version"]
    [preloadSecretNames="true"]
    type="Microsoft.Configuration.ConfigurationBuilders.AzureKeyVaultConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Azure" />
```

<span data-ttu-id="b7bba-246">O [AzureKeyVaultConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Azure/) lê os valores armazenados no [Azure Key Vault](/azure/key-vault/key-vault-whatis).</span><span class="sxs-lookup"><span data-stu-id="b7bba-246">The [AzureKeyVaultConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Azure/) reads values stored in the [Azure Key Vault](/azure/key-vault/key-vault-whatis).</span></span>

<span data-ttu-id="b7bba-247">`vaultName` é necessário (o nome do cofre ou um URI para o cofre).</span><span class="sxs-lookup"><span data-stu-id="b7bba-247">`vaultName` is required (either the name of the vault or a URI to the vault).</span></span> <span data-ttu-id="b7bba-248">Os outros atributos permitem o controle sobre a qual cofre se conectar, mas são necessários apenas se o aplicativo não estiver em execução em um ambiente que funcione com o `Microsoft.Azure.Services.AppAuthentication` .</span><span class="sxs-lookup"><span data-stu-id="b7bba-248">The other attributes allow control about which vault to connect to, but are only necessary if the application is not running in an environment that works with `Microsoft.Azure.Services.AppAuthentication`.</span></span> <span data-ttu-id="b7bba-249">A biblioteca de autenticação dos serviços do Azure é usada para escolher automaticamente as informações de conexão do ambiente de execução, se possível.</span><span class="sxs-lookup"><span data-stu-id="b7bba-249">The Azure Services Authentication library is used to automatically pick up connection information from the execution environment if possible.</span></span> <span data-ttu-id="b7bba-250">Você pode substituir a seleção automática de informações de conexão fornecendo uma cadeia de conexão.</span><span class="sxs-lookup"><span data-stu-id="b7bba-250">You can override automatically pick up of connection information by providing a connection string.</span></span>

* <span data-ttu-id="b7bba-251">`vaultName` -Obrigatório se `uri` não for fornecido.</span><span class="sxs-lookup"><span data-stu-id="b7bba-251">`vaultName` - Required if `uri` in not provided.</span></span> <span data-ttu-id="b7bba-252">Especifica o nome do cofre em sua assinatura do Azure a partir da qual os pares de chave/valor são lidos.</span><span class="sxs-lookup"><span data-stu-id="b7bba-252">Specifies the name of the vault in your Azure subscription from which to read key/value pairs.</span></span>
* <span data-ttu-id="b7bba-253">`uri` -Conecta-se a outros provedores de Key Vault com o `uri` valor especificado.</span><span class="sxs-lookup"><span data-stu-id="b7bba-253">`uri` - Connects to other Key Vault providers with the specified `uri` value.</span></span> <span data-ttu-id="b7bba-254">Se não for especificado, o Azure ( `vaultName` ) é o provedor de cofre.</span><span class="sxs-lookup"><span data-stu-id="b7bba-254">If not specified, Azure (`vaultName`) is the vault provider.</span></span>
* <span data-ttu-id="b7bba-255">`version` -Azure Key Vault fornece um recurso de controle de versão para segredos.</span><span class="sxs-lookup"><span data-stu-id="b7bba-255">`version` - Azure Key Vault provides a versioning feature for secrets.</span></span> <span data-ttu-id="b7bba-256">Se `version` for especificado, o Construtor recuperará somente os segredos correspondentes a esta versão.</span><span class="sxs-lookup"><span data-stu-id="b7bba-256">If `version` is specified, the builder only retrieves secrets matching this version.</span></span>
* <span data-ttu-id="b7bba-257">`preloadSecretNames` -Por padrão, esse construtor consulta **todos os** nomes de chave no cofre de chaves quando ele é inicializado.</span><span class="sxs-lookup"><span data-stu-id="b7bba-257">`preloadSecretNames` - By default, this builder querys **all** key names in the key vault when it is initialized.</span></span> <span data-ttu-id="b7bba-258">Para evitar a leitura de todos os valores de chave, defina este atributo como `false` .</span><span class="sxs-lookup"><span data-stu-id="b7bba-258">To prevent reading all key values, set this attribute to `false`.</span></span> <span data-ttu-id="b7bba-259">Definir isso para `false` ler segredos um de cada vez.</span><span class="sxs-lookup"><span data-stu-id="b7bba-259">Setting this to `false` reads secrets one at a time.</span></span> <span data-ttu-id="b7bba-260">A leitura de segredos um de cada vez pode ser útil se o cofre permitir acesso "Get", mas não "lista".</span><span class="sxs-lookup"><span data-stu-id="b7bba-260">Reading secrets one at a time can useful if the vault allows "Get" access but not "List" access.</span></span> <span data-ttu-id="b7bba-261">**Observação:** Ao usar o `Greedy` modo, `preloadSecretNames` deve ser `true` (o padrão.)</span><span class="sxs-lookup"><span data-stu-id="b7bba-261">**Note:** When using `Greedy` mode, `preloadSecretNames` must be `true` (the default.)</span></span>

### <a name="keyperfileconfigbuilder"></a><span data-ttu-id="b7bba-262">KeyPerFileConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="b7bba-262">KeyPerFileConfigBuilder</span></span>

```xml
<add name="KeyPerFile"
    [mode|prefix|stripPrefix|tokenPattern]
    (directoryPath="PathToSourceDirectory")
    [ignorePrefix="ignore."]
    [keyDelimiter=":"]
    [optional="false"]
    type="Microsoft.Configuration.ConfigurationBuilders.KeyPerFileConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.KeyPerFile" />
```

<span data-ttu-id="b7bba-263">[KeyPerFileConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.KeyPerFile/) é um construtor de configuração básico que usa os arquivos de um diretório como uma fonte de valores.</span><span class="sxs-lookup"><span data-stu-id="b7bba-263">[KeyPerFileConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.KeyPerFile/) is a basic configuration builder that uses a directory's files as a source of values.</span></span> <span data-ttu-id="b7bba-264">O nome de um arquivo é a chave e o conteúdo é o valor.</span><span class="sxs-lookup"><span data-stu-id="b7bba-264">A file's name is the key, and the contents are the value.</span></span> <span data-ttu-id="b7bba-265">Esse construtor de configuração pode ser útil ao ser executado em um ambiente de contêiner orquestrado.</span><span class="sxs-lookup"><span data-stu-id="b7bba-265">This configuration builder can be useful when running in an orchestrated container environment.</span></span> <span data-ttu-id="b7bba-266">Sistemas como Docker Swarm e kubernetes fornecem `secrets` aos seus contêineres do Windows orquestrados nessa maneira de chave por arquivo.</span><span class="sxs-lookup"><span data-stu-id="b7bba-266">Systems like Docker Swarm and Kubernetes provide `secrets` to their orchestrated windows containers in this key-per-file manner.</span></span>

<span data-ttu-id="b7bba-267">Detalhes do atributo:</span><span class="sxs-lookup"><span data-stu-id="b7bba-267">Attribute details:</span></span>

* <span data-ttu-id="b7bba-268">`directoryPath` Necessária.</span><span class="sxs-lookup"><span data-stu-id="b7bba-268">`directoryPath` - Required.</span></span> <span data-ttu-id="b7bba-269">Especifica um caminho para procurar valores.</span><span class="sxs-lookup"><span data-stu-id="b7bba-269">Specifies a path to look in for values.</span></span> <span data-ttu-id="b7bba-270">Os segredos de Docker for Windows são armazenados no diretório *C:\ProgramData\Docker\secrets* por padrão.</span><span class="sxs-lookup"><span data-stu-id="b7bba-270">Docker for Windows secrets are stored in the *C:\ProgramData\Docker\secrets* directory by default.</span></span>
* <span data-ttu-id="b7bba-271">`ignorePrefix` -Os arquivos que começam com esse prefixo são excluídos.</span><span class="sxs-lookup"><span data-stu-id="b7bba-271">`ignorePrefix` - Files that start with this prefix are excluded.</span></span> <span data-ttu-id="b7bba-272">O padrão é "ignorar".</span><span class="sxs-lookup"><span data-stu-id="b7bba-272">Defaults to "ignore.".</span></span>
* <span data-ttu-id="b7bba-273">`keyDelimiter` -O valor padrão é `null` .</span><span class="sxs-lookup"><span data-stu-id="b7bba-273">`keyDelimiter` - Default value is `null`.</span></span> <span data-ttu-id="b7bba-274">Se especificado, o construtor de configuração atravessa vários níveis do diretório, criando nomes de chave com esse delimitador.</span><span class="sxs-lookup"><span data-stu-id="b7bba-274">If specified, the configuration builder traverses multiple levels of the directory, building up key names with this delimiter.</span></span> <span data-ttu-id="b7bba-275">Se esse valor for `null` , o construtor de configuração apenas examinará o nível superior do diretório.</span><span class="sxs-lookup"><span data-stu-id="b7bba-275">If this value is `null`, the configuration builder only looks at the top level of the directory.</span></span>
* <span data-ttu-id="b7bba-276">`optional` -O valor padrão é `false` .</span><span class="sxs-lookup"><span data-stu-id="b7bba-276">`optional` -  Default value is `false`.</span></span> <span data-ttu-id="b7bba-277">Especifica se o construtor de configuração deve causar erros se o diretório de origem não existir.</span><span class="sxs-lookup"><span data-stu-id="b7bba-277">Specifies whether the configuration builder should cause errors if the source directory doesn't exist.</span></span>

### <a name="simplejsonconfigbuilder"></a><span data-ttu-id="b7bba-278">SimpleJsonConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="b7bba-278">SimpleJsonConfigBuilder</span></span>

> [!WARNING]
> <span data-ttu-id="b7bba-279">Nunca armazene senhas, cadeias de conexão confidenciais ou outros dados confidenciais no código-fonte.</span><span class="sxs-lookup"><span data-stu-id="b7bba-279">Never store passwords, sensitive connection strings, or other sensitive data in source code.</span></span> <span data-ttu-id="b7bba-280">Os segredos de produção não devem ser usados para desenvolvimento ou teste.</span><span class="sxs-lookup"><span data-stu-id="b7bba-280">Production secrets should not be used for development or test.</span></span>

```xml
<add name="SimpleJson"
    [mode|prefix|stripPrefix|tokenPattern]
    jsonFile="~\config.json"
    [optional="true"]
    [jsonMode="(Flat|Sectional)"]
    type="Microsoft.Configuration.ConfigurationBuilders.SimpleJsonConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Json" />
```

<span data-ttu-id="b7bba-281">Os projetos do .NET Core frequentemente usam arquivos JSON para configuração.</span><span class="sxs-lookup"><span data-stu-id="b7bba-281">.NET Core projects frequently use JSON files for configuration.</span></span> <span data-ttu-id="b7bba-282">O [SimpleJsonConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Json/) Builder permite que os arquivos JSON do .NET Core sejam usados no .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="b7bba-282">The [SimpleJsonConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Json/) builder allows .NET Core JSON files to be used in the .NET Framework.</span></span> <span data-ttu-id="b7bba-283">Este construtor de configuração fornece um mapeamento básico de uma fonte de chave/valor simples em áreas de chave/valor específicas da configuração de .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="b7bba-283">This configuration builder provides a basic mapping from a flat key/value source into specific key/value areas of .NET Framework configuration.</span></span> <span data-ttu-id="b7bba-284">Este construtor de configuração **não fornece configurações** hierárquicas.</span><span class="sxs-lookup"><span data-stu-id="b7bba-284">This configuration builder does **not** provide for hierarchical configurations.</span></span> <span data-ttu-id="b7bba-285">O arquivo de backup JSON é semelhante a um dicionário, e não a um objeto hierárquico complexo.</span><span class="sxs-lookup"><span data-stu-id="b7bba-285">The JSON backing file is similar to a dictionary, not a complex hierarchical object.</span></span> <span data-ttu-id="b7bba-286">Um arquivo hierárquico de vários níveis pode ser usado.</span><span class="sxs-lookup"><span data-stu-id="b7bba-286">A multi-level hierarchical file can be used.</span></span> <span data-ttu-id="b7bba-287">Esse provedor é `flatten` a profundidade acrescentando o nome da propriedade em cada nível usando `:` como um delimitador.</span><span class="sxs-lookup"><span data-stu-id="b7bba-287">This provider `flatten`s the depth by appending the property name at each level using `:` as a delimiter.</span></span>

<span data-ttu-id="b7bba-288">Detalhes do atributo:</span><span class="sxs-lookup"><span data-stu-id="b7bba-288">Attribute details:</span></span>

* <span data-ttu-id="b7bba-289">`jsonFile` Necessária.</span><span class="sxs-lookup"><span data-stu-id="b7bba-289">`jsonFile` - Required.</span></span> <span data-ttu-id="b7bba-290">Especifica o arquivo JSON do qual ler.</span><span class="sxs-lookup"><span data-stu-id="b7bba-290">Specifies the JSON file to read from.</span></span> <span data-ttu-id="b7bba-291">O `~` caractere pode ser usado no início para fazer referência à raiz do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="b7bba-291">The `~` character can be used at the start to reference the app root.</span></span>
* <span data-ttu-id="b7bba-292">`optional` -Booliano, o valor padrão é `true` .</span><span class="sxs-lookup"><span data-stu-id="b7bba-292">`optional` - Boolean,  default value is `true`.</span></span> <span data-ttu-id="b7bba-293">Impede o lançamento de exceções se o arquivo JSON não puder ser encontrado.</span><span class="sxs-lookup"><span data-stu-id="b7bba-293">Prevents throwing exceptions if the JSON file cannot be found.</span></span>
* <span data-ttu-id="b7bba-294">`jsonMode` - `[Flat|Sectional]`.</span><span class="sxs-lookup"><span data-stu-id="b7bba-294">`jsonMode` - `[Flat|Sectional]`.</span></span> <span data-ttu-id="b7bba-295">`Flat` é o padrão.</span><span class="sxs-lookup"><span data-stu-id="b7bba-295">`Flat` is the default.</span></span> <span data-ttu-id="b7bba-296">Quando `jsonMode` é `Flat` , o arquivo JSON é uma única origem de chave/valor simples.</span><span class="sxs-lookup"><span data-stu-id="b7bba-296">When `jsonMode` is `Flat`, the JSON file is a single flat key/value source.</span></span> <span data-ttu-id="b7bba-297">As `EnvironmentConfigBuilder` e `AzureKeyVaultConfigBuilder` também são fontes simples de chave/valor.</span><span class="sxs-lookup"><span data-stu-id="b7bba-297">The `EnvironmentConfigBuilder` and `AzureKeyVaultConfigBuilder` are also single flat key/value sources.</span></span> <span data-ttu-id="b7bba-298">Quando o `SimpleJsonConfigBuilder` é configurado no `Sectional` modo:</span><span class="sxs-lookup"><span data-stu-id="b7bba-298">When the `SimpleJsonConfigBuilder` is configured in `Sectional` mode:</span></span>

  * <span data-ttu-id="b7bba-299">O arquivo JSON é dividido conceitualmente apenas no nível superior em vários dicionários.</span><span class="sxs-lookup"><span data-stu-id="b7bba-299">The JSON file is conceptually divided just at the top level into multiple dictionaries.</span></span>
  * <span data-ttu-id="b7bba-300">Cada um dos dicionários só é aplicado à seção de configuração que corresponde ao nome de propriedade de nível superior anexado a eles.</span><span class="sxs-lookup"><span data-stu-id="b7bba-300">Each of the dictionaries is only applied to the configuration section that matches the top-level property name attached to them.</span></span> <span data-ttu-id="b7bba-301">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="b7bba-301">For example:</span></span>

```json
    {
        "appSettings" : {
            "setting1" : "value1",
            "setting2" : "value2",
            "complex" : {
                "setting1" : "complex:value1",
                "setting2" : "complex:value2",
            }
        }
    }
```

## <a name="configuration-builders-order"></a><span data-ttu-id="b7bba-302">Ordem dos criadores de configuração</span><span class="sxs-lookup"><span data-stu-id="b7bba-302">Configuration builders order</span></span>

<span data-ttu-id="b7bba-303">Confira [ConfigurationBuilders ordem de execução](https://github.com/aspnet/MicrosoftConfigurationBuilders/blob/master/README.md#configurationbuilders-order-of-execution) no repositório GitHub [ASPNET/MicrosoftConfigurationBuilders](https://github.com/aspnet/MicrosoftConfigurationBuilders) .</span><span class="sxs-lookup"><span data-stu-id="b7bba-303">See [ConfigurationBuilders Order of Execution](https://github.com/aspnet/MicrosoftConfigurationBuilders/blob/master/README.md#configurationbuilders-order-of-execution) in the [aspnet/MicrosoftConfigurationBuilders](https://github.com/aspnet/MicrosoftConfigurationBuilders) GitHub repository.</span></span>

## <a name="implementing-a-custom-keyvalue-configuration-builder"></a><span data-ttu-id="b7bba-304">Implementando um construtor de configuração de chave/valor personalizado</span><span class="sxs-lookup"><span data-stu-id="b7bba-304">Implementing a custom key/value configuration builder</span></span>

<span data-ttu-id="b7bba-305">Se os construtores de configuração não atenderem às suas necessidades, você poderá escrever um personalizado.</span><span class="sxs-lookup"><span data-stu-id="b7bba-305">If the configuration builders don't meet your needs, you can write a custom one.</span></span> <span data-ttu-id="b7bba-306">A `KeyValueConfigBuilder` classe base lida com os modos de substituição e a maioria das preocupações com prefixos.</span><span class="sxs-lookup"><span data-stu-id="b7bba-306">The `KeyValueConfigBuilder` base class handles substitution modes and most prefix concerns.</span></span> <span data-ttu-id="b7bba-307">Um projeto de implementação precisa apenas de:</span><span class="sxs-lookup"><span data-stu-id="b7bba-307">An implementing project need only:</span></span>

* <span data-ttu-id="b7bba-308">Herdar da classe base e implementar uma fonte básica de pares de chave/valor por meio de `GetValue` e `GetAllValues` :</span><span class="sxs-lookup"><span data-stu-id="b7bba-308">Inherit from the base class and implement a basic source of key/value pairs through the `GetValue` and `GetAllValues`:</span></span>
* <span data-ttu-id="b7bba-309">Adicione o [Microsoft.Configuration.ConfigurationBuilders. base](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Base/) ao projeto.</span><span class="sxs-lookup"><span data-stu-id="b7bba-309">Add the [Microsoft.Configuration.ConfigurationBuilders.Base](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Base/) to the project.</span></span>

[!code-csharp[Main](config-builder/MyConfigBuilders/MyCustomConfigBuilder.cs)]

<span data-ttu-id="b7bba-310">A `KeyValueConfigBuilder` classe base fornece grande parte do comportamento de trabalho e consistente entre os construtores de configuração de chave/valor.</span><span class="sxs-lookup"><span data-stu-id="b7bba-310">The `KeyValueConfigBuilder` base class provides much of the work and consistent behavior across key/value configuration builders.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b7bba-311">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="b7bba-311">Additional resources</span></span>

* [<span data-ttu-id="b7bba-312">Repositório GitHub de construtores de configuração</span><span class="sxs-lookup"><span data-stu-id="b7bba-312">Configuration Builders GitHub repository</span></span>](https://github.com/aspnet/MicrosoftConfigurationBuilders)
* [<span data-ttu-id="b7bba-313">Autenticação serviço a serviço para Azure Key Vault usando o .NET</span><span class="sxs-lookup"><span data-stu-id="b7bba-313">Service-to-service authentication to Azure Key Vault using .NET</span></span>](/azure/key-vault/service-to-service-authentication#connection-string-support)
