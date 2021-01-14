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
# <a name="configuration-builders-for-aspnet"></a>Construtores de configuração para ASP.NET

Por [Stephen Molloy](https://github.com/StephenMolloy) e [Rick Anderson](https://twitter.com/RickAndMSFT)

Os construtores de configuração fornecem um mecanismo moderno e ágil para aplicativos ASP.NET obterem valores de configuração de fontes externas.

Construtores de configuração:

* Estão disponíveis no .NET Framework 4.7.1 e versões posteriores.
* Forneça um mecanismo flexível para ler valores de configuração.
* Resolva algumas das necessidades básicas dos aplicativos à medida que eles se movem para um ambiente voltado para a nuvem e um contêiner.
* Pode ser usado para melhorar a proteção de dados de configuração por meio do desenho de fontes anteriormente indisponíveis (por exemplo, Azure Key Vault e variáveis de ambiente) no sistema de configuração do .NET.

## <a name="keyvalue-configuration-builders"></a>Construtores de configuração de chave/valor

Um cenário comum que pode ser tratado por construtores de configuração é fornecer um mecanismo básico de substituição de chave/valor para seções de configuração que seguem um padrão de chave/valor. O conceito de .NET Framework de ConfigurationBuilders não é limitado a seções ou padrões de configuração específicos. No entanto, muitos dos construtores de configuração no `Microsoft.Configuration.ConfigurationBuilders` ([GitHub](https://github.com/aspnet/MicrosoftConfigurationBuilders), [NuGet](https://www.nuget.org/packages?q=Microsoft.Configuration.ConfigurationBuilders)) funcionam dentro do padrão de chave/valor.

## <a name="keyvalue-configuration-builders-settings"></a>Configurações de construtores de configuração de chave/valor

As configurações a seguir se aplicam a todos os construtores de configuração de chave/valor no `Microsoft.Configuration.ConfigurationBuilders` .

### <a name="mode"></a>Modo

Os construtores de configuração usam uma fonte externa de informações de chave/valor para preencher os elementos de chave/valor selecionados do sistema de configuração. Especificamente, as `<appSettings/>` `<connectionStrings/>` seções e recebem tratamento especial dos criadores de configuração. Os criadores funcionam em três modos:

* `Strict` -O modo padrão. Nesse modo, o construtor de configuração opera apenas em seções de configuração bem conhecidas de chave/valor centrado. `Strict` o modo enumera cada chave na seção. Se uma chave correspondente for encontrada na fonte externa:

   * Os construtores de configuração substituem o valor na seção de configuração resultante pelo valor da fonte externa.
* `Greedy` -Esse modo está fortemente relacionado ao `Strict` modo. Em vez de ser limitado a chaves que já existem na configuração original:

  * Os construtores de configuração adicionam todos os pares de chave/valor da fonte externa na seção de configuração resultante.

* `Expand` -Opera no XML bruto antes de ser analisado em um objeto da seção de configuração. Pode ser pensado como uma expansão de tokens em uma cadeia de caracteres. Qualquer parte da cadeia de caracteres XML bruta que corresponda ao padrão `${token}` é um candidato para a expansão do token. Se nenhum valor correspondente for encontrado na origem externa, o token não será alterado. Os construtores neste modo não são limitados às `<appSettings/>` `<connectionStrings/>` seções e.

A marcação a seguir de *web.config* habilita o [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/) no `Strict` modo:

[!code-xml[Main](config-builder/MyConfigBuilders/WebDefault.config?name=snippet)]

O código a seguir lê o `<appSettings/>` e `<connectionStrings/>` mostrado no arquivo *web.config* anterior:

[!code-csharp[Main](config-builder/MyConfigBuilders/About.aspx.cs)]

O código anterior definirá os valores de propriedade como:

* Os valores no arquivo de *web.config* se as chaves não estiverem definidas em variáveis de ambiente.
* Os valores da variável de ambiente, se definido.

Por exemplo, `ServiceID` conterá:

* "Valor de ServiceId de web.config", se a variável de ambiente `ServiceID` não estiver definida.
* O valor da `ServiceID` variável de ambiente, se definido.

A imagem a seguir mostra as `<appSettings/>` chaves/valores do arquivo de *web.config* anterior definido no editor de ambiente:

![Editor de ambiente](config-builder/static/env.png)

Observação: Talvez seja necessário sair e reiniciar o Visual Studio para ver as alterações nas variáveis de ambiente.

### <a name="prefix-handling"></a>Manipulação de prefixo

Os prefixos de chave podem simplificar a configuração de chaves porque:

* A configuração de .NET Framework é complexa e aninhada.
* As origens de chave/valor externas são normalmente básicas e planas por natureza. Por exemplo, variáveis de ambiente não são aninhadas.

Use qualquer uma das seguintes abordagens para injetar ambos `<appSettings/>` e `<connectionStrings/>` na configuração por meio de variáveis de ambiente:

* Com o `EnvironmentConfigBuilder` no modo padrão `Strict` e os nomes de chave apropriados no arquivo de configuração. O código e a marcação anteriores assumem essa abordagem. Usando essa abordagem, você **não** pode ter chaves nomeadas de forma idêntica no `<appSettings/>` e no `<connectionStrings/>` .
* Use dois `EnvironmentConfigBuilder` s no `Greedy` modo com prefixos distintos e `stripPrefix` . Com essa abordagem, o aplicativo pode ler `<appSettings/>` e `<connectionStrings/>` sem a necessidade de atualizar o arquivo de configuração. A próxima seção,  [stripPrefix](#stripprefix), mostra como fazer isso.
* Use dois `EnvironmentConfigBuilder` s no `Greedy` modo com prefixos distintos. Com essa abordagem, você não pode ter nomes de chave duplicados, pois nomes de chave devem ser diferentes por prefixo.  Por exemplo:

[!code-xml[Main](config-builder/MyConfigBuilders/WebPrefix.config?name=snippet&highlight=11-99)]

Com a marcação anterior, a mesma fonte de chave/valor simples pode ser usada para preencher a configuração de duas seções diferentes.

A imagem a seguir mostra `<appSettings/>` as `<connectionStrings/>` chaves e os valores do arquivo de *web.config* anterior definido no editor de ambiente:

![Editor de ambiente](config-builder/static/prefix.png)

O código a seguir lê `<appSettings/>` as `<connectionStrings/>` chaves e os valores contidos no arquivo de *web.config* anterior:

[!code-csharp[Main](config-builder/MyConfigBuilders/Contact.aspx.cs?name=snippet)]

O código anterior definirá os valores de propriedade como:

* Os valores no arquivo de *web.config* se as chaves não estiverem definidas em variáveis de ambiente.
* Os valores da variável de ambiente, se definido.

Por exemplo, usando o arquivo de *web.config* anterior, as chaves/valores na imagem do editor de ambiente anterior e o código anterior, os seguintes valores são definidos:

|  Chave              | Valor |
| ----------------- | ------------ |
|     AppSetting_ServiceID           | AppSetting_ServiceID de variáveis ENV|
|    AppSetting_default            | AppSetting_default valor de env |
|       ConnStr_default         | ConnStr_default Val de env|

### <a name="stripprefix"></a>stripPrefix

`stripPrefix`: booliano, o padrão é `false` . 

A marcação XML anterior separa as configurações do aplicativo das cadeias de conexão, mas requer que todas as chaves no arquivo de *web.config* usem o prefixo especificado. Por exemplo, o prefixo `AppSetting` deve ser adicionado à `ServiceID` chave ("AppSetting_ServiceID"). Com `stripPrefix` o, o prefixo não é usado no arquivo de *web.config* . O prefixo é necessário na origem do Configuration Builder (por exemplo, no ambiente). Prevemos que a maioria dos desenvolvedores usará `stripPrefix` .

Normalmente, os aplicativos tiram o prefixo. O *web.config* a seguir remove o prefixo:

[!code-xml[Main](config-builder/MyConfigBuilders/WebPrefixStrip.config?name=snippet&highlight=14,19)]

No arquivo *web.config* anterior, a `default` chave está no `<appSettings/>` e no `<connectionStrings/>` .

A imagem a seguir mostra `<appSettings/>` as `<connectionStrings/>` chaves e os valores do arquivo de *web.config* anterior definido no editor de ambiente:

![Editor de ambiente](config-builder/static/prefix.png)

O código a seguir lê `<appSettings/>` as `<connectionStrings/>` chaves e os valores contidos no arquivo de *web.config* anterior:

[!code-csharp[Main](config-builder/MyConfigBuilders/About2.aspx.cs?name=snippet)]

O código anterior definirá os valores de propriedade como:

* Os valores no arquivo de *web.config* se as chaves não estiverem definidas em variáveis de ambiente.
* Os valores da variável de ambiente, se definido.

Por exemplo, usando o arquivo de *web.config* anterior, as chaves/valores na imagem do editor de ambiente anterior e o código anterior, os seguintes valores são definidos:

|  Chave              | Valor |
| ----------------- | ------------ |
|     ServiceID           | AppSetting_ServiceID de variáveis ENV|
|    padrão            | AppSetting_default valor de env |
|    padrão         | ConnStr_default Val de env|

### <a name="tokenpattern"></a>tokenPattern

`tokenPattern`: Cadeia de caracteres, o padrão é `@"\$\{(\w+)\}"`

O `Expand` comportamento dos construtores pesquisa no XML bruto os tokens semelhantes `${token}` . A pesquisa é feita com a expressão regular padrão `@"\$\{(\w+)\}"` . O conjunto de caracteres que corresponde `\w` é mais estrito do que o XML e muitas fontes de configuração permitem. Use `tokenPattern` quando houver mais caracteres do que `@"\$\{(\w+)\}"` o necessário no nome do token.

`tokenPattern`Strings

* Permite que os desenvolvedores alterem o Regex que é usado para correspondência de token.
* Nenhuma validação é feita para garantir que se trata de um Regex bem formado e não perigoso.
* Ele deve conter um grupo de captura. O Regex inteiro deve corresponder ao token inteiro. A primeira captura deve ser o nome do token a ser pesquisado na fonte de configuração.

## <a name="configuration-builders-in-microsoftconfigurationconfigurationbuilders"></a>Construtores de configuração no Microsoft.Configuration.ConfigurationBuilders

### <a name="environmentconfigbuilder"></a>EnvironmentConfigBuilder

```xml
<add name="Environment"
    [mode|prefix|stripPrefix|tokenPattern] 
    type="Microsoft.Configuration.ConfigurationBuilders.EnvironmentConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Environment" />
```

O [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/):

* É a mais simples dos construtores de configuração.
* Lê os valores do ambiente.
* Não tem nenhuma opção de configuração adicional.
* O `name` valor do atributo é arbitrário.

**Observação:** Em um ambiente de contêiner do Windows, as variáveis definidas em tempo de execução são injetadas apenas no ambiente de processo EntryPoint. Os aplicativos executados como um serviço ou um processo não-EntryPoint não pegam essas variáveis, a menos que eles sejam injetados por um mecanismo no contêiner. Para [](https://github.com/Microsoft/iis-docker/pull/41) / contêineres baseados em [ASP.net](https://github.com/Microsoft/aspnet-docker)do IIS, a versão atual do [ServiceMonitor.exe](https://github.com/Microsoft/iis-docker/pull/41) trata isso somente no *DefaultAppPool* . Outras variantes de contêiner com base no Windows talvez precisem desenvolver seu próprio mecanismo de injeção para processos não EntryPoint.

### <a name="usersecretsconfigbuilder"></a>UserSecretsConfigBuilder

> [!WARNING]
> Nunca armazene senhas, cadeias de conexão confidenciais ou outros dados confidenciais no código-fonte. Os segredos de produção não devem ser usados para desenvolvimento ou teste.

```xml
<add name="UserSecrets"
    [mode|prefix|stripPrefix|tokenPattern]
    (userSecretsId="{secret string, typically a GUID}" | userSecretsFile="~\secrets.file")
    [optional="true"]
    type="Microsoft.Configuration.ConfigurationBuilders.UserSecretsConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.UserSecrets" />
```

Este construtor de configuração fornece um recurso semelhante a [ASP.NET Core o Gerenciador de segredo](/aspnet/core/security/app-secrets).

O [UserSecretsConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.UserSecrets/) pode ser usado em projetos .NET Framework, mas um arquivo de segredos deve ser especificado. Como alternativa, você pode definir a `UserSecretsId` propriedade no arquivo de projeto e criar o arquivo de segredos brutos no local correto para leitura. Para manter as dependências externas do seu projeto, o arquivo secreto é XML formatado. A formatação XML é um detalhe de implementação e o formato não deve ser confiado. Se você precisar compartilhar um *secrets.jsno* arquivo com projetos do .NET Core, considere o uso de [SimpleJsonConfigBuilder](#simplejsonconfigbuilder). O `SimpleJsonConfigBuilder` formato do .NET Core também deve ser considerado um detalhe de implementação sujeito a alterações.

Atributos de configuração para `UserSecretsConfigBuilder` :

* `userSecretsId` -Esse é o método preferencial para identificar um arquivo de segredos XML. Ele funciona semelhante ao .NET Core, que usa uma `UserSecretsId` propriedade de projeto para armazenar esse identificador. A cadeia de caracteres deve ser exclusiva, não precisa ser uma GUID. Com esse atributo, a `UserSecretsConfigBuilder` aparência de um local local conhecido ( `%APPDATA%\Microsoft\UserSecrets\<UserSecrets Id>\secrets.xml` ) para um arquivo de segredos que pertence a esse identificador.
* `userSecretsFile` -Um atributo opcional que especifica o arquivo que contém os segredos. O `~` caractere pode ser usado no início para fazer referência à raiz do aplicativo. O atributo ou o `userSecretsId` atributo é necessário. Se ambos forem especificados, `userSecretsFile` terá precedência.
* `optional`: booliano, valor padrão `true` -impede uma exceção se o arquivo de segredos não puder ser encontrado. 
* O `name` valor do atributo é arbitrário.

O arquivo de segredos tem o seguinte formato:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<root>
  <secrets ver="1.0">
    <secret name="secret key name" value="secret value" />
  </secrets>
</root>
```

### <a name="azurekeyvaultconfigbuilder"></a>AzureKeyVaultConfigBuilder

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

O [AzureKeyVaultConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Azure/) lê os valores armazenados no [Azure Key Vault](/azure/key-vault/key-vault-whatis).

`vaultName` é necessário (o nome do cofre ou um URI para o cofre). Os outros atributos permitem o controle sobre a qual cofre se conectar, mas são necessários apenas se o aplicativo não estiver em execução em um ambiente que funcione com o `Microsoft.Azure.Services.AppAuthentication` . A biblioteca de autenticação dos serviços do Azure é usada para escolher automaticamente as informações de conexão do ambiente de execução, se possível. Você pode substituir a seleção automática de informações de conexão fornecendo uma cadeia de conexão.

* `vaultName` -Obrigatório se `uri` não for fornecido. Especifica o nome do cofre em sua assinatura do Azure a partir da qual os pares de chave/valor são lidos.
* `uri` -Conecta-se a outros provedores de Key Vault com o `uri` valor especificado. Se não for especificado, o Azure ( `vaultName` ) é o provedor de cofre.
* `version` -Azure Key Vault fornece um recurso de controle de versão para segredos. Se `version` for especificado, o Construtor recuperará somente os segredos correspondentes a esta versão.
* `preloadSecretNames` -Por padrão, esse construtor consulta **todos os** nomes de chave no cofre de chaves quando ele é inicializado. Para evitar a leitura de todos os valores de chave, defina este atributo como `false` . Definir isso para `false` ler segredos um de cada vez. A leitura de segredos um de cada vez pode ser útil se o cofre permitir acesso "Get", mas não "lista". **Observação:** Ao usar o `Greedy` modo, `preloadSecretNames` deve ser `true` (o padrão.)

### <a name="keyperfileconfigbuilder"></a>KeyPerFileConfigBuilder

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

[KeyPerFileConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.KeyPerFile/) é um construtor de configuração básico que usa os arquivos de um diretório como uma fonte de valores. O nome de um arquivo é a chave e o conteúdo é o valor. Esse construtor de configuração pode ser útil ao ser executado em um ambiente de contêiner orquestrado. Sistemas como Docker Swarm e kubernetes fornecem `secrets` aos seus contêineres do Windows orquestrados nessa maneira de chave por arquivo.

Detalhes do atributo:

* `directoryPath` Necessária. Especifica um caminho para procurar valores. Os segredos de Docker for Windows são armazenados no diretório *C:\ProgramData\Docker\secrets* por padrão.
* `ignorePrefix` -Os arquivos que começam com esse prefixo são excluídos. O padrão é "ignorar".
* `keyDelimiter` -O valor padrão é `null` . Se especificado, o construtor de configuração atravessa vários níveis do diretório, criando nomes de chave com esse delimitador. Se esse valor for `null` , o construtor de configuração apenas examinará o nível superior do diretório.
* `optional` -O valor padrão é `false` . Especifica se o construtor de configuração deve causar erros se o diretório de origem não existir.

### <a name="simplejsonconfigbuilder"></a>SimpleJsonConfigBuilder

> [!WARNING]
> Nunca armazene senhas, cadeias de conexão confidenciais ou outros dados confidenciais no código-fonte. Os segredos de produção não devem ser usados para desenvolvimento ou teste.

```xml
<add name="SimpleJson"
    [mode|prefix|stripPrefix|tokenPattern]
    jsonFile="~\config.json"
    [optional="true"]
    [jsonMode="(Flat|Sectional)"]
    type="Microsoft.Configuration.ConfigurationBuilders.SimpleJsonConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Json" />
```

Os projetos do .NET Core frequentemente usam arquivos JSON para configuração. O [SimpleJsonConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Json/) Builder permite que os arquivos JSON do .NET Core sejam usados no .NET Framework. Este construtor de configuração fornece um mapeamento básico de uma fonte de chave/valor simples em áreas de chave/valor específicas da configuração de .NET Framework. Este construtor de configuração **não fornece configurações** hierárquicas. O arquivo de backup JSON é semelhante a um dicionário, e não a um objeto hierárquico complexo. Um arquivo hierárquico de vários níveis pode ser usado. Esse provedor é `flatten` a profundidade acrescentando o nome da propriedade em cada nível usando `:` como um delimitador.

Detalhes do atributo:

* `jsonFile` Necessária. Especifica o arquivo JSON do qual ler. O `~` caractere pode ser usado no início para fazer referência à raiz do aplicativo.
* `optional` -Booliano, o valor padrão é `true` . Impede o lançamento de exceções se o arquivo JSON não puder ser encontrado.
* `jsonMode` - `[Flat|Sectional]`. `Flat` é o padrão. Quando `jsonMode` é `Flat` , o arquivo JSON é uma única origem de chave/valor simples. As `EnvironmentConfigBuilder` e `AzureKeyVaultConfigBuilder` também são fontes simples de chave/valor. Quando o `SimpleJsonConfigBuilder` é configurado no `Sectional` modo:

  * O arquivo JSON é dividido conceitualmente apenas no nível superior em vários dicionários.
  * Cada um dos dicionários só é aplicado à seção de configuração que corresponde ao nome de propriedade de nível superior anexado a eles. Por exemplo:

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

## <a name="configuration-builders-order"></a>Ordem dos criadores de configuração

Confira [ConfigurationBuilders ordem de execução](https://github.com/aspnet/MicrosoftConfigurationBuilders/blob/master/README.md#configurationbuilders-order-of-execution) no repositório GitHub [ASPNET/MicrosoftConfigurationBuilders](https://github.com/aspnet/MicrosoftConfigurationBuilders) .

## <a name="implementing-a-custom-keyvalue-configuration-builder"></a>Implementando um construtor de configuração de chave/valor personalizado

Se os construtores de configuração não atenderem às suas necessidades, você poderá escrever um personalizado. A `KeyValueConfigBuilder` classe base lida com os modos de substituição e a maioria das preocupações com prefixos. Um projeto de implementação precisa apenas de:

* Herdar da classe base e implementar uma fonte básica de pares de chave/valor por meio de `GetValue` e `GetAllValues` :
* Adicione o [Microsoft.Configuration.ConfigurationBuilders. base](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Base/) ao projeto.

[!code-csharp[Main](config-builder/MyConfigBuilders/MyCustomConfigBuilder.cs)]

A `KeyValueConfigBuilder` classe base fornece grande parte do comportamento de trabalho e consistente entre os construtores de configuração de chave/valor.

## <a name="additional-resources"></a>Recursos adicionais

* [Repositório GitHub de construtores de configuração](https://github.com/aspnet/MicrosoftConfigurationBuilders)
* [Autenticação serviço a serviço para Azure Key Vault usando o .NET](/azure/key-vault/service-to-service-authentication#connection-string-support)
