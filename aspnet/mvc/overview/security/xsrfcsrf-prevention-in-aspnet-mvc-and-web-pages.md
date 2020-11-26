---
uid: mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages
title: Prevenção de XSRF/CSRF no ASP.NET MVC e páginas da Web | Microsoft Docs
author: Rick-Anderson
description: A solicitação entre sites forjada (também conhecida como XSRF ou CSRF) é um ataque contra aplicativos hospedados na Web, no qual um site mal-intencionado pode influenciar a interação...
ms.author: riande
ms.date: 03/14/2013
ms.assetid: aadc5fa4-8215-4fc7-afd5-bcd2ef879728
msc.legacyurl: /mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages
msc.type: authoredcontent
ms.openlocfilehash: 7c45e342bc55ece3a6b41dcc42a0f33cd3eceffd
ms.sourcegitcommit: 4b78855427f1397df0a7be3559e04ec94a78c308
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96151862"
---
# <a name="xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages"></a>Prevenção de XSRF/CSRF no ASP.NET MVC e em páginas da Web

por [Rick Anderson](https://twitter.com/RickAndMSFT)

> A solicitação entre sites forjada (também conhecida como XSRF ou CSRF) é um ataque contra aplicativos hospedados na Web, no qual um site mal-intencionado pode influenciar a interação entre um navegador cliente e um site confiável para esse navegador. Esses ataques são possíveis porque os navegadores da Web enviarão automaticamente tokens de autenticação com cada solicitação a um site. O exemplo canônico é um cookie de autenticação, como o tíquete de autenticação de formulários do ASP.NET. No entanto, os sites que usam qualquer mecanismo de autenticação persistente (como a autenticação do Windows, básica e assim por diante) podem ser direcionados por esses ataques.
> 
> Um ataque XSRF é diferente de um ataque de phishing. Os ataques de phishing exigem a interação da vítima. Em um ataque de phishing, um site mal-intencionado imita o site de destino, e a vítima é induzida a fornecer informações confidenciais para o invasor. Em um ataque XSRF, normalmente não é necessária nenhuma interação da vítima. Em vez disso, o invasor está confiando no navegador enviando automaticamente todos os cookies relevantes para o site de destino.
> 
> Para obter mais informações, consulte [Open Web Application Security Project](https://www.owasp.org/index.php/Main_Page)(OWASP) [XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)).

## <a name="anatomy-of-an-attack"></a>Anatomia de um ataque

Para percorrer um ataque de XSRF, considere um usuário que deseja executar algumas transações bancárias online. Esse usuário primeiro visita WoodgroveBank.com e faz logon, ponto em que o cabeçalho de resposta conterá seu cookie de autenticação:

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample1.cmd)]

Como o cookie de autenticação é um cookie de sessão, ele será automaticamente limpo pelo navegador quando o processo do navegador for encerrado. No entanto, até esse momento, o navegador incluirá automaticamente o cookie com cada solicitação para WoodgroveBank.com. O usuário agora deseja transferir $1000 para outra conta, portanto, ele preenche um formulário no site bancário e o navegador faz essa solicitação para o servidor:

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample2.cmd)]

Como essa operação tem um efeito colateral (ele inicia uma transação monetária), o site bancário optou por exigir um HTTP POST para iniciar esta operação. O servidor lê o token de autenticação da solicitação, pesquisa o número da conta do usuário atual, verifica se há fundos suficientes e, em seguida, inicia a transação na conta de destino.

Seu banco online concluído, o usuário navega para fora do site bancário e visita outros locais na Web. Um desses sites – fabrikam.com – inclui a seguinte marcação em uma página inserida em um &lt; iframe &gt; :

[!code-html[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample3.html)]

Que faz com que o navegador faça essa solicitação:

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample4.cmd)]

O invasor está explorando o fato de que o usuário ainda pode ter um token de autenticação válido para o site de destino e está usando um pequeno trecho de JavaScript para fazer com que o navegador faça um HTTP POST para o site de destino automaticamente. Se o token de autenticação ainda for válido, o site bancário iniciará uma transferência de $250 para a conta de escolha do invasor.

### <a name="ineffective-mitigations"></a>Mitigações ineficazes

É interessante observar que, no cenário acima, o fato de que WoodgroveBank.com estava sendo acessado via SSL e tinha um cookie de autenticação somente SSL que era insuficiente para frustrar o ataque. O invasor é capaz de especificar o [esquema de URI](http://en.wikipedia.org/wiki/URI_scheme) (https) em seu &lt; &gt; elemento form, e o navegador continuará a enviar cookies não expirados para o site de destino, desde que esses cookies sejam consistentes com o esquema URI do destino pretendido.

Alguém poderia argumentar que o usuário simplesmente não deve visitar sites não confiáveis, pois a visita apenas sites confiáveis ajuda a permanecer online seguro. Há alguma verdade para isso, mas infelizmente esse aviso nem sempre é prático. Talvez o usuário "Confie" no site de notícias local ConsolidatedMessenger.com e vá para visitar esse site em vez disso, mas esse site tem uma vulnerabilidade de XSS que permite que um invasor Insira o mesmo trecho de código que estava em execução no fabrikam.com.

Você pode verificar se as solicitações de entrada têm um [cabeçalho de referência](http://www.w3.org/Protocols/HTTP/HTRQ_Headers.html#z14) que referencia seu domínio. Isso interromperá as solicitações enviadas de um domínio de terceiros de forma involuntária. No entanto, algumas pessoas desativam o cabeçalho de referência do navegador por motivos de privacidade, e os invasores podem, às vezes, falsificar esse cabeçalho se a vítima tiver determinado software inseguro instalado. Verificar o [cabeçalho do referenciador](http://www.w3.org/Protocols/HTTP/HTRQ_Headers.html#z14) não é considerado uma abordagem segura para evitar ataques de XSRF.

## <a name="web-stack-runtime-xsrf-mitigations"></a>Mitigações XSRF de tempo de execução da pilha da Web

O tempo de execução da pilha da Web do ASP.NET usa uma variante do [padrão de token do sincronizador](https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html#synchronizer-token-pattern) para se defender contra ataques do XSRF. A forma geral do padrão de token do sincronizador é que dois tokens XSRF são enviados ao servidor com cada HTTP POST (além do token de autenticação): um token como um cookie e o outro como um valor de formulário. Os valores de token gerados pelo tempo de execução ASP.NET não são determinísticos ou previsíveis por um invasor. Quando os tokens forem enviados, o servidor permitirá que a solicitação continue somente se ambos os tokens passarem por uma verificação de comparação.

O *token de sessão* de verificação de solicitação XSRF é armazenado como um cookie HTTP e atualmente contém as seguintes informações em sua carga:

- Um token de segurança, que consiste em um identificador aleatório de 128 bits.   
 A imagem a seguir mostra o token de sessão de verificação de solicitação XSRF exibido com as ferramentas de desenvolvedor do Internet Explorer F12: (Observe que essa é a implementação atual e está sujeita, provavelmente, para alterar.)

![](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/_static/image1.png)

O *token de campo* é armazenado como um `<input type="hidden" />` e contém as seguintes informações em sua carga:

- O nome do usuário conectado (se for autenticado).
- Quaisquer dados adicionais fornecidos por um [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx).

Os conteúdos dos tokens XSRF são criptografados e assinados, portanto, você não pode exibir o nome de usuário ao usar ferramentas para examinar os tokens. Quando o aplicativo Web tem como alvo ASP.NET 4,0, os serviços de criptografia são fornecidos pela rotina [machineKey. Encode](https://msdn.microsoft.com/library/system.web.security.machinekey.encode.aspx) . Quando o aplicativo Web tem como alvo o ASP.NET 4,5 ou superior, os serviços de criptografia são fornecidos pela rotina [machineKey. Protect](https://msdn.microsoft.com/library/system.web.security.machinekey.protect(v=vs.110)) , que oferece melhor desempenho, extensibilidade e segurança. Consulte as postagens de blog a seguir para obter mais detalhes:

- [Aprimoramentos de criptografia no ASP.NET 4,5, pt. 1](https://blogs.msdn.com/b/webdev/archive/2012/10/22/cryptographic-improvements-in-asp-net-4-5-pt-1.aspx)
- [Aprimoramentos de criptografia no ASP.NET 4,5, pt. 2](https://blogs.msdn.com/b/webdev/archive/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2.aspx)
- [Aprimoramentos de criptografia no ASP.NET 4,5, pt. 3](https://blogs.msdn.com/b/webdev/archive/2012/10/24/cryptographic-improvements-in-asp-net-4-5-pt-3.aspx)

## <a name="generating-the-tokens"></a>Gerando os tokens

Para gerar os tokens XSRF, chame o [@Html.AntiForgeryToken](https://msdn.microsoft.com/library/dd470175.aspx) método de uma exibição MVC ou @AntiForgery.GetHtml () de uma página Razor. Em seguida, o tempo de execução executará as seguintes etapas:

1. Se a solicitação HTTP atual já contiver um token de sessão XSRF (o RequestVerificationToken de cookie XSRF \_ \_ ), o token de segurança será extraído dele. Se a solicitação HTTP não contiver um token de sessão XSRF ou se a extração do token de segurança falhar, um novo token XSRF aleatório será gerado.
2. Um token de campo XSRF é gerado usando o token de segurança da etapa (1) acima e a identidade do usuário conectado no momento. (Para obter mais informações sobre como determinar a identidade do usuário, consulte a seção **[cenários com suporte especial](#_Scenarios_with_special)** abaixo.) Além disso, se um [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/jj158328(v=vs.111).aspx) estiver configurado, o tempo de execução chamará seu método [GetAdditionalData](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider.getadditionaldata(v=vs.111).aspx) e incluirá a cadeia de caracteres retornada no token do campo. (Consulte a seção **[configuração e extensibilidade](#_Configuration_and_extensibility)** para obter mais informações.)
3. Se um novo token XSRF tiver sido gerado na etapa (1), um novo token de sessão será criado para contê-lo e será adicionado à coleção de cookies HTTP de saída. O token de campo da etapa (2) será encapsulado em um `<input type="hidden" />` elemento e essa marcação HTML será o valor de retorno de `Html.AntiForgeryToken()` ou `AntiForgery.GetHtml()` .

## <a name="validating-the-tokens"></a>Validando os tokens

Para validar os tokens XSRF de entrada, o desenvolvedor inclui um atributo [ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(VS.108).aspx) em sua ação ou controlador MVC ou chama a `@AntiForgery.Validate()` partir de sua página Razor. O tempo de execução executará as seguintes etapas:

1. O token de sessão de entrada e o token de campo são lidos e o token XSRF extraído de cada um. Os tokens XSRF devem ser idênticos por etapa (2) na rotina de geração.
2. Se o usuário atual for autenticado, seu nome de usuário será comparado com o nome de usuário armazenado no token do campo. Os nomes de acessadores devem corresponder.
3. Se um [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx) estiver configurado, o tempo de execução chamará seu método *ValidateAdditionalData* . O método deve retornar o valor booliano *true*.

Se a validação for realizada com sucesso, a solicitação poderá continuar. Se a validação falhar, a estrutura gerará um *HttpAntiForgeryException*.

## <a name="failure-conditions"></a>Condições de falha

A partir do ASP.NET Web Stack Runtime v2, qualquer *HttpAntiForgeryException* gerada durante a validação conterá informações detalhadas sobre o que deu errado. As condições de falha definidas atualmente são:

- O token de sessão ou o token de formulário não está presente na solicitação.
- O token de sessão ou o token de formulário é ilegível. A causa mais provável disso é um farm que executa versões incompatíveis do tempo de execução do ASP.NET Web Stack ou um farm no qual o &lt; &gt; elemento machineKey no Web.config difere entre os computadores. Você pode usar uma ferramenta como o Fiddler para forçar essa exceção violando o token XSRF.
- O token de sessão e o token de campo foram trocados.
- O token de sessão e o token de campo contêm tokens de segurança incompatíveis.
- O nome de usuário inserido no token de campo não corresponde ao nome de usuário conectado no momento.
- O método *[IAntiForgeryAdditionalDataProvider. ValidateAdditionalData](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider.validateadditionaldata(v=vs.111).aspx)* retornou *false*.

Os recursos de XSRF também podem executar verificações adicionais durante a geração ou validação de tokens, e as falhas durante essas verificações podem resultar na geração de exceções. Consulte as seções de autenticação e extensibilidade do [WIF/ACS/declarações e a](#_WIF_ACS) **[extensão](#_Configuration_and_extensibility)** para obter mais informações.

<a id="_Scenarios_with_special"></a>

## <a name="scenarios-with-special-support"></a>Cenários com suporte especial

### <a name="anonymous-authentication"></a>Autenticação anônima

O sistema XSRF contém suporte especial para usuários anônimos, onde "anônimo" é definido como um usuário em que a propriedade *IIdentity. IsAuthenticated* retorna *false*. Os cenários incluem fornecer proteção XSRF à página de logon (antes de o usuário ser autenticado) e esquemas de autenticação personalizados em que o aplicativo usa um mecanismo diferente de *IIdentity* para identificar os usuários.

Para dar suporte a esses cenários, lembre-se de que os tokens de sessão e de campo são Unidos por um token de segurança, que é um identificador opaco gerado aleatoriamente de 128 bits. Esse token de segurança é usado para rastrear a sessão de um usuário individual à medida que navega pelo site, de modo que ele atende efetivamente à finalidade de um identificador anônimo. Uma cadeia de caracteres vazia é usada no lugar do nome de usuário para as rotinas de geração e validação descritas acima.

<a id="_WIF_ACS"></a>

### <a name="wif--acs--claims-based-authentication"></a>Autenticação baseada em declarações do WIF/ACS

Normalmente, as classes de *IIdentity* internas ao .NET Framework têm a propriedade de que *IIdentity.Name* é suficiente para identificar exclusivamente um determinado usuário em um determinado aplicativo. Por exemplo, *FormsIdentity.Name* retorna o nome de usuário armazenado no banco de dados de associação (que é exclusivo para todos os aplicativos, dependendo do banco de dados), *WindowsIdentity.Name* retorna a identidade qualificada pelo domínio do usuário e assim por diante. Esses sistemas fornecem não apenas a autenticação; Eles também *identificam* os usuários para um aplicativo.

A autenticação baseada em declarações, por outro lado, não requer necessariamente a identificação de um usuário específico. Em vez disso, os tipos *ClaimsPrincipal* e *ClaimsIdentity* são associados a um conjunto de instâncias de *declaração* , em que as declarações individuais podem ser "mais de 18 anos de idade" ou "é um administrador" para qualquer outra coisa. Como o usuário não foi necessariamente identificado, o tempo de execução não pode usar a propriedade *ClaimsIdentity.Name* como um identificador exclusivo para esse usuário específico. A equipe já viu exemplos do mundo real em que *ClaimsIdentity.Name* retorna *NULL*, retorna um nome amigável (de exibição) ou, de outra forma, retorna uma cadeia de caracteres que não é apropriada para uso como um identificador exclusivo para o usuário.

Muitas implantações que usam a autenticação baseada em declarações estão usando o ACS ( [serviço de controle de acesso) do Azure](https://msdn.microsoft.com/library/windowsazure/gg429786.aspx) em particular. O ACS permite que o desenvolvedor configure *provedores de identidade* individuais (como o ADFS, o provedor de contas da Microsoft, provedores de OpenID como Yahoo!, etc.) e os provedores de identidade retornem *identificadores de nome*. Esses identificadores de nome podem conter informações de identificação pessoal (PII), como um endereço de email, ou podem ser anônimos como um PPID (identificador pessoal privado). Independentemente disso, a tupla (provedor de identidade, identificador de nome) é suficientemente usada como um token de acompanhamento apropriado para um usuário específico enquanto está navegando no site, para que o tempo de execução da pilha da Web ASP.NET possa usar a tupla no lugar do nome de usuário ao gerar e validar tokens de campo XSRF. Os URIs específicos para o provedor de identidade e o identificador de nome são:

- `https://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider`
- `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier`

(consulte esta [página de documento do ACS](https://msdn.microsoft.com/library/windowsazure/gg185971.aspx) para obter mais informações.)

Ao gerar ou validar um token, o tempo de execução da pilha da Web ASP.NET em tempo de execução tentará associar aos tipos:

- `Microsoft.IdentityModel.Claims.IClaimsIdentity, Microsoft.IdentityModel, Version=3.5.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35` (Para o SDK do WIF.)
- `System.Security.Claims.ClaimsIdentity` (Para .NET 4,5).

Se esses tipos existirem e se o *IIIIdentity* do usuário atual implementar ou subclasses de um desses tipos, o recurso XSRF usará a tupla (provedor de identidade, identificador de nome) no lugar do nome de usuário ao gerar e validar os tokens. Se nenhuma tupla desse tipo estiver presente, a solicitação falhará com um erro que descreve ao desenvolvedor como configurar o sistema antiXSRF para entender o mecanismo de autenticação baseado em declarações específico em uso. Consulte a seção **[configuração e extensibilidade](#_Configuration_and_extensibility)** para obter mais informações.

### <a name="oauth--openid-authentication"></a>Autenticação do OAuth/OpenID

Por fim, o recurso XSRF tem suporte especial para aplicativos que usam autenticação OAuth ou OpenID. Esse suporte é baseado em heurística: se o *IIdentity.Name* atual começar com http://ou https://, as comparações de nome de usuário serão feitas usando um comparador ordinal em vez do comparador de OrdinalIgnoreCase padrão.

<a id="_Configuration_and_extensibility"></a>

## <a name="configuration-and-extensibility"></a>Configuração e extensibilidade

Ocasionalmente, os desenvolvedores podem querer um controle mais rígido sobre os comportamentos de geração e validação XSRF. Por exemplo, talvez o comportamento padrão dos auxiliares do MVC e das páginas da Web de adicionar automaticamente cookies HTTP à resposta seja indesejável e o desenvolvedor talvez queira manter os tokens em outro lugar. Existem duas APIs para ajudar com isso:

`AntiForgery.GetTokens(string oldCookieToken, out string newCookieToken, out string formToken);`  
`AntiForgery.Validate(string cookieToken, string formToken);`

O método *Gettokens* usa como entrada um token de sessão de verificação de solicitação XSRF existente (que pode ser nulo) e produz como saída um novo token de sessão de verificação de solicitação XSRF e um token de campo. Os tokens são simplesmente cadeias de caracteres opacas sem decoração; o valor de *formToken* , por exemplo, não será encapsulado em uma &lt; marca de entrada &gt; . O valor de *newCookieToken* pode ser nulo; Se isso ocorrer, o valor de *oldCookieToken* ainda será válido e nenhum novo cookie de resposta precisará ser definido. O chamador de *Gettokens* é responsável por manter todos os cookies de resposta necessários ou gerar qualquer marcação necessária; o método *Gettokens* em si não irá alterar a resposta como um efeito colateral. O método *Validate* usa a sessão de entrada e os tokens de campo e executa a lógica de validação mencionada sobre eles.

### <a name="antiforgeryconfig"></a>AntiForgeryConfig

O desenvolvedor pode configurar o sistema XSRF a partir do início do aplicativo \_ . A configuração é programática. As propriedades do tipo estático *AntiForgeryConfig* são descritas abaixo. A maioria dos usuários que usam declarações desejará definir a propriedade UniqueClaimTypeIdentifier.

| **Propriedade** | **Descrição** |
| --- | --- |
| **AdditionalDataProvider** | Um [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx) que fornece dados adicionais durante a geração de token e consome dados adicionais durante a validação do token. O valor padrão é *NULL*. Para obter mais informações, consulte a seção [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx) . |
| **CookieName** | Uma cadeia de caracteres que fornece o nome do cookie HTTP que é usado para armazenar o token de sessão XSRF. Se esse valor não for definido, um nome será gerado automaticamente com base no caminho virtual implantado do aplicativo. O valor padrão é *NULL*. |
| **RequireSsl** | Um booliano que determina se os tokens XSRF precisam ser enviados por um canal protegido por SSL. Se esse valor for *true*, todos os cookies gerados automaticamente terão o sinalizador "Secure" definido, e as APIs XSRF serão lançadas se chamadas de dentro de uma solicitação que não é enviada via SSL. O valor padrão é *false*. |
| **SuppressIdentityHeuristicChecks** | Um booliano que determina se o sistema XSRF deve desativar seu suporte para identidades baseadas em declarações. Se esse valor for *true*, o sistema presumirá que *IIdentity.Name* é apropriado para uso como um identificador exclusivo por usuário e não tentará se *IClaimsIdentity* ou *ClClaimsIdentity* de caso especial, conforme descrito na seção [WIF/ACS/autenticação baseada em declarações](#_WIF_ACS) . O valor padrão é `false`. |
| **UniqueClaimTypeIdentifier** | Uma cadeia de caracteres que indica qual tipo de declaração é apropriado para uso como um identificador por usuário exclusivo. Se esse valor for definido e o *IIdentity* atual for baseado em declarações, o sistema tentará extrair uma declaração do tipo especificado por *UniqueClaimTypeIdentifier* e o valor correspondente será usado no lugar do nome do usuário ao gerar o token do campo. Se o tipo de declaração não for encontrado, o sistema falhará na solicitação. O valor padrão é *NULL*, que indica que o sistema deve usar a tupla (provedor de identidade, identificador de nome), conforme descrito anteriormente no lugar do nome de usuário. |

<a id="_IAntiForgeryAdditionalDataProvider"></a>

### <a name="iantiforgeryadditionaldataprovider"></a>IAntiForgeryAdditionalDataProvider

O tipo *[IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx)* permite que os desenvolvedores estendam o comportamento do sistema XSRF por meio da ida e volta de dados adicionais em cada token. O método *GetAdditionalData* é chamado cada vez que um token de campo é gerado e o valor de retorno é inserido no token gerado. Um implementador poderia retornar um carimbo de data/hora, um nonce ou qualquer outro valor que desejar nesse método.

Da mesma forma, o método *ValidateAdditionalData* é chamado cada vez que um token de campo é validado e a cadeia de caracteres "dados adicionais" que foi inserida no token é passada para o método. A rotina de validação pode implementar um tempo limite (verificando a hora atual em relação ao horário que foi armazenado quando o token foi criado), uma rotina de verificação de nonce ou qualquer outra lógica desejada.

## <a name="design-decisions-and-security-considerations"></a>Decisões de design e considerações de segurança

O token de segurança que vincula os tokens de sessão e de campo é tecnicamente necessário apenas ao tentar proteger usuários anônimos/não autenticados contra ataques de XSRF. Quando o usuário é autenticado, o próprio token de autenticação (supostamente enviado na forma de um cookie) poderia ser usado como uma metade de um par de tokens do sincronizador. No entanto, há cenários válidos para proteger páginas de logon atingidas por usuários não autenticados, e a lógica XSRF foi tornada mais simples, sempre gerando e validando o token de segurança, mesmo para usuários autenticados. Ele também fornece uma proteção adicional caso um token de campo seja comprometido por um invasor, uma vez que a configuração ou a adivinhação do token de sessão seria outro obstáculo para que o invasor se supere.

Os desenvolvedores devem ter cuidado quando vários aplicativos são hospedados em um único domínio. Por exemplo, embora *example1.cloudapp.net* e *example2.cloudapp.net* sejam hosts diferentes, há uma relação de confiança implícita entre todos os hosts no domínio *\* . cloudapp.net* . Essa relação de confiança implícita [permite que hosts potencialmente não confiáveis afetem os cookies uns dos outros](http://stackoverflow.com/questions/9636857/how-can-asp-net-or-asp-net-mvc-be-protected-from-related-domain-cookie-attacks) (as políticas de mesma origem que regem as solicitações Ajax não necessariamente se aplicam a cookies http). O tempo de execução da pilha da Web do ASP.NET fornece alguma mitigação, pois o nome de usuário é inserido no token do campo; portanto, mesmo que um subdomínio mal-intencionado seja capaz de substituir um token de sessão, não será possível gerar um token de campo válido para o usuário. No entanto, quando hospedado em um ambiente desse tipo, as rotinas XSRF internas ainda não podem se defender contra seqüestro de sessão ou logon XSRF.

Atualmente, as rotinas XSRF não se defendem contra [sequestro](https://www.owasp.org/index.php/Clickjacking). Aplicativos que desejam se defender contra sequestro podem facilmente fazer isso enviando um cabeçalho X-Frame-Options: SAMEORIGIN com cada resposta. Esse cabeçalho tem suporte de todos os navegadores recentes. Para obter mais informações, consulte o [blog do IE](https://blogs.msdn.com/b/ieinternals/archive/2010/03/30/combating-clickjacking-with-x-frame-options.aspx), o blog do [SDL](https://blogs.msdn.com/b/sdl/archive/2009/02/05/clickjacking-defense-in-ie8.aspx)e o [OWASP](https://www.owasp.org/index.php/Clickjacking). O tempo de execução da pilha da Web do ASP.NET pode, em alguma versão futura, fazer com que os auxiliares do MVC e das páginas da Web de XSRF definam automaticamente esse cabeçalho para que os aplicativos sejam automaticamente protegidos contra esse ataque.

Os desenvolvedores da Web devem continuar a garantir que seu site não seja vulnerável a ataques de XSS. Os ataques de XSS são muito poderosos e uma exploração bem-sucedida também interromperia as defesas de tempo de execução de pilha da Web ASP.NET contra ataques de XSRF.

## <a name="acknowledgment"></a>Confirmação

[@LeviBroderick](https://twitter.com/LeviBroderick), que escreveu grande parte do código de segurança ASP.NET a grande parte dessas informações.
