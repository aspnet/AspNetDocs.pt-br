---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/the-fix-it-sample-application
title: 'Apêndice: o aplicativo de exemplo corrigir a ti (criando Real-World aplicativos de nuvem com o Azure) | Microsoft Docs'
author: MikeWasson
description: A criação de aplicativos de nuvem do mundo real com o livro eletrônico do Azure é baseada em uma apresentação desenvolvida por Scott Guthrie. Ele explica 13 padrões e práticas que podem...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 1bc333c5-f096-4ea7-b170-779accc21c1a
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/the-fix-it-sample-application
msc.type: authoredcontent
ms.openlocfilehash: 53ae9633277b38f1d2c8a8bd43eaf0014da54c45
ms.sourcegitcommit: 4b78855427f1397df0a7be3559e04ec94a78c308
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96151874"
---
# <a name="appendix-the-fix-it-sample-application-building-real-world-cloud-apps-with-azure"></a>Apêndice: o aplicativo de exemplo corrigir a ti (criando Real-World aplicativos de nuvem com o Azure)

por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra)

[Baixar o projeto de correção de ti](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)

> A **criação de aplicativos de nuvem do mundo real com** o livro eletrônico do Azure é baseada em uma apresentação desenvolvida por Scott Guthrie. Ele explica 13 padrões e práticas que podem ajudá-lo a desenvolver com sucesso aplicativos Web para a nuvem. Para obter informações sobre o livro eletrônico, consulte [o primeiro capítulo](introduction.md).

Este apêndice para a criação de aplicativos de nuvem do mundo real com o livro eletrônico do Azure contém as seções a seguir que fornecem informações adicionais sobre o aplicativo de exemplo corrigir que você pode baixar:

- [Problemas conhecidos](#knownissues)
- [Práticas recomendadas](#bestpractices)
- [Como executar o aplicativo do Visual Studio em seu computador local](#run-in-vs)
- [Como implantar o aplicativo base em aplicativos Web do Azure App Service usando os scripts do Windows PowerShell](#deploybase)
- [Solucionando problemas dos scripts do Windows PowerShell](#troubleshooting)
- [Como implantar o aplicativo com processamento de fila para Azure App aplicativos Web do serviço e um serviço de nuvem do Azure](#deployqueues)

<a id="knownissues"></a>
## <a name="known-issues"></a>Problemas conhecidos

O aplicativo corrigir ti foi originalmente desenvolvido para ilustrar o mais simples possível de alguns dos padrões apresentados neste livro eletrônico. No entanto, como o livro eletrônico é sobre a criação de aplicativos do mundo real, estamos sujeitos a correção do código de ti a um processo de revisão e teste semelhante ao que teríamos para o software lançado. Encontramos vários problemas e, como acontece com qualquer aplicativo do mundo real, alguns deles corrigidos e alguns deles, nós adiamos para uma versão posterior.

A lista a seguir inclui problemas que devem ser resolvidos em um aplicativo de produção, mas por um motivo ou outro, decidimos não abordar na versão inicial do aplicativo de exemplo corrigir ti.

### <a name="security"></a>Segurança

- Certifique-se de que você não pode atribuir uma tarefa a um proprietário não existente.
- Certifique-se de que você só pode exibir e modificar tarefas que você criou ou que estão atribuídas a você.
- Use HTTPS para páginas de entrada e cookies de autenticação.
- Especifique um limite de tempo para os cookies de autenticação.

### <a name="input-validation"></a>Validação de entrada

Em geral, um aplicativo de produção faria mais validação de entrada do que o aplicativo corrigir ti. Por exemplo, o tamanho do arquivo de imagem/tamanho da imagem permitido para carregamento deve ser limitado.

### <a name="administrator-functionality"></a>Funcionalidade do administrador

Um administrador deve ser capaz de alterar a propriedade das tarefas existentes. Por exemplo, o criador de uma tarefa pode sair da empresa, não deixando ninguém com autoridade para manter a tarefa, a menos que o acesso administrativo esteja habilitado.

### <a name="queue-message-processing"></a>Processamento de mensagens de fila

O processamento de mensagens de fila no aplicativo corrigir ti foi projetado para ser simples, a fim de ilustrar o padrão de trabalho centrado em fila com uma quantidade mínima de código. Esse código simples não seria adequado para um aplicativo de produção real.

- O código não garante que cada mensagem da fila será processada no máximo uma vez. Quando você recebe uma mensagem da fila, há um período de tempo limite, durante o qual a mensagem é invisível para outros ouvintes de fila. Se o tempo limite expirar antes que a mensagem seja excluída, a mensagem se tornará visível novamente. Portanto, se uma instância de função de trabalho gastar muito tempo processando uma mensagem, é teoricamente possível que a mesma mensagem seja processada duas vezes, resultando em uma tarefa duplicada no banco de dados. Para obter mais informações sobre esse problema, consulte [usando filas de armazenamento do Azure](https://msdn.microsoft.com/library/ff803365.aspx#sec7).
- A lógica de sondagem de fila pode ser mais econômica ao colocar em lote a recuperação de mensagens. Toda vez que você chama [CloudQueue. GetMessageAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.getmessageasync.aspx), há um custo de transação. Em vez disso, você pode chamar [CloudQueue. GetMessagesAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.getmessagesasync.aspx) (Observe o ' s "), que obtém várias mensagens em uma única transação. Os custos de transação das filas do armazenamento do Azure são muito baixos, portanto, o impacto nos custos não é substancial na maioria dos cenários.
- O loop rígido no código de processamento de mensagens da fila causa afinidade de CPU, o que não utiliza VMs com vários núcleos com eficiência. Um design melhor usaria o paralelismo de tarefas para executar várias tarefas assíncronas em paralelo.
- O processamento de mensagens na fila tem apenas tratamento rudimentar de exceção. Por exemplo, o código não trata [mensagens suspeitas](https://msdn.microsoft.com/library/ms789028.aspx). (Quando o processamento de mensagens causa uma exceção, você precisa registrar o erro e excluir a mensagem, ou a função de trabalho tentará processá-la novamente, e o loop continuará indefinidamente.)

### <a name="sql-queries-are-unbounded"></a>Consultas SQL não associadas

Correção atual o código de ti não coloca nenhum limite de quantas linhas as consultas de páginas de índice podem retornar. Se um grande volume de tarefas for inserido no banco de dados, o tamanho das listas resultantes recebidas poderá causar problemas de desempenho. A solução é implementar a paginação. Para obter um exemplo, consulte [classificar, filtrar e paginação com o Entity Framework em um aplicativo MVC ASP.net](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md).

### <a name="view-models-recommended"></a>Exibir modelos recomendados

O aplicativo corrigir ti usa a classe de entidade FixItTask para passar informações entre o controlador e a exibição. Uma prática recomendada é usar modelos de exibição. O modelo de domínio (por exemplo, a classe de entidade FixItTask) foi projetado em relação ao que é necessário para a persistência de dados, enquanto um modelo de exibição pode ser projetado para a apresentação de dados. Para obter mais informações, consulte [12 práticas recomendadas do MVC do ASP.net](https://codeclimber.net.nz/archive/2009/10/27/12-asp.net-mvc-best-practices.aspx).

### <a name="secure-image-blob-recommended"></a>Blob de imagem segura recomendado

O aplicativo corrigir a ti armazena imagens carregadas como públicas, o que significa que qualquer pessoa que encontre a URL pode acessar as imagens. As imagens podem ser protegidas em vez de públicas.

### <a name="no-powershell-automation-scripts-for-queues"></a>Nenhum script de automação do PowerShell para filas

Os scripts de automação do PowerShell de exemplo foram gravados apenas para a versão base do Fix it que é executado inteiramente em aplicativos Web do Azure App Service. Não fornecemos scripts para configurar e implantar o aplicativo Web mais o ambiente de serviço de nuvem necessário para o processamento de fila.

### <a name="special-handling-for-html-codes-in-user-input"></a>Tratamento especial para códigos HTML na entrada do usuário

O ASP.NET automaticamente impede muitas maneiras em que usuários mal-intencionados podem tentar ataques de script entre sites inserindo script nas caixas de texto de entrada do usuário. E o `DisplayFor` auxiliar MVC usado para exibir títulos de tarefas e observações automaticamente codifica em código HTML os valores que ele envia para o navegador. Mas em um aplicativo de produção, talvez você queira executar medidas adicionais. Para obter mais informações, consulte [validação de solicitação em ASP.net](https://msdn.microsoft.com/library/hh882339.aspx).

<a id="bestpractices"></a>
## <a name="best-practices"></a>Práticas recomendadas

Veja a seguir alguns problemas que foram corrigidos após serem descobertos na revisão de código e no teste da versão original do aplicativo corrigir ti. Alguns foram causados pelo codificador original que não está ciente de uma prática recomendada específica, alguns simplesmente porque o código foi escrito rapidamente e não se destina ao software lançado. Estamos listando os problemas aqui caso haja algo que aprendemos com essa revisão e teste que podem ser úteis para outras pessoas que também estão desenvolvendo aplicativos Web.

### <a name="dispose-the-database-repository"></a>Descartar o repositório de banco de dados

A `FixItTaskRepository` classe deve descartar a instância de Entity Framework `DbContext` . Fizemos isso implementando `IDisposable` na `FixItTaskRepository` classe:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample1.cs)]

Observe que o AutoFac descartará automaticamente a `FixItTaskRepository` instância, portanto, não precisamos descartá-la explicitamente.

Outra opção é remover a `DbContext` variável de membro de `FixItTaskRepository` e, em vez disso, criar uma `DbContext` variável local dentro de cada método de repositório, dentro de uma `using` instrução. Por exemplo:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample2.cs)]

### <a name="register-singletons-as-such-with-di"></a>Registrar singletons como tal com DI

Como apenas uma instância da `PhotoService` classe e `Logger` classe é necessária, essas classes devem ser [registradas como instâncias únicas para injeção de dependência](https://code.google.com/p/autofac/wiki/InstanceScope) em *DependenciesConfig.cs*:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample3.cs?highlight=1,3)]

### <a name="security-dont-show-error-details-to-users"></a>Segurança: não mostrar detalhes do erro aos usuários

O aplicativo de correção original não tinha uma página de erro genérica e apenas permitisse que todas as exceções fossem consumidas na interface do usuário. portanto, algumas exceções, como erros de conexão de banco de dados, podem resultar em um rastreamento de pilha completo sendo exibido para o navegador. As informações detalhadas de erro podem, às vezes, facilitar ataques por usuários mal-intencionados. A solução é registrar em log os detalhes da exceção e exibir uma página de erro para o usuário que não inclui detalhes do erro. O aplicativo corrigir ti já estava em log e, para exibir uma página de erro, adicionamos `<customErrors mode=On>` o arquivo Web.config.

[!code-xml[Main](the-fix-it-sample-application/samples/sample4.xml?highlight=2)]

Por padrão, isso faz com que *Views\Shared\Error.cshtml* seja exibido para erros. Você pode personalizar o *Error. cshtml* ou criar seu próprio modo de exibição de página de erro e adicionar um `defaultRedirect` atributo. Você também pode especificar páginas de erro diferentes para erros específicos.

### <a name="security-only-allow-a-task-to-be-edited-by-its-creator"></a>Segurança: permitir que apenas uma tarefa seja editada por seu criador

A página índice do painel mostra apenas as tarefas criadas pelo usuário conectado, mas um usuário mal-intencionado pode criar uma URL com uma ID para a tarefa de outro usuário. Adicionamos o código em *DashboardController.cs* para retornar um 404 nesse caso:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample5.cs?highlight=9-14,24-29)]

### <a name="dont-swallow-exceptions"></a>Não assimilar exceções

A correção original que o aplicativo acaba de retornar nulo depois de registrar uma exceção que resultou de uma consulta SQL:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample6.cs?highlight=4)]

Isso o tornaria semelhante ao usuário como se a consulta fosse bem-sucedida, mas simplesmente não retornou linhas. A solução é lançar novamente a exceção após capturar e registrar em log:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample7.cs)]

### <a name="catch-all-exceptions-in-worker-roles"></a>Capturar todas as exceções em funções de trabalho

Todas as exceções sem tratamento em uma função de trabalho farão com que a VM seja reciclada, portanto, você deseja encapsular tudo o que você faz em um bloco try-catch e tratar todas as exceções.

### <a name="specify-length-for-string-properties-in-entity-classes"></a>Especificar o comprimento das propriedades de cadeia de caracteres em classes de entidade

Para exibir o código simples, a versão original do aplicativo corrigir ti não especificou comprimentos para os campos da entidade FixItTask e, como resultado, eles foram definidos como varchar (max) no banco de dados. Como resultado, a interface do usuário aceitaria quase qualquer quantidade de entrada. A especificação de comprimentos define os limites que se aplicam à entrada do usuário na página da Web e no tamanho da coluna no banco de dados:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample8.cs?highlight=4,7,10,12,14)]

### <a name="mark-private-members-as-readonly-when-they-arent-expected-to-change"></a>Marcar membros particulares como ReadOnly quando não se espera alterar

Por exemplo, na `DashboardController` classe, uma instância do `FixItTaskRepository` é criada e não deve ser alterada, portanto, definimos como [ReadOnly](https://msdn.microsoft.com/library/acdd6hb7.aspx).

[!code-csharp[Main](the-fix-it-sample-application/samples/sample9.cs?highlight=3)]

### <a name="use-listany-instead-of-listcount-gt-0"></a>Lista de uso. Qualquer () em vez de lista. Contagem () &gt; 0

Se você tiver certeza de que um ou mais itens de uma lista se ajustam aos critérios especificados, use o método [any](https://msdn.microsoft.com/library/bb534972.aspx) , pois ele retorna assim que um item que está ajustando os critérios for encontrado, enquanto o `Count` método sempre precisará iterar em cada item. O arquivo *index. cshtml* do painel originalmente tinha este código:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample10.cshtml)]

Alteramos isso para:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample11.cshtml?highlight=1)]

### <a name="generate-urls-in-mvc-views-using-mvc-helpers"></a>Gerar URLs em exibições do MVC usando auxiliares do MVC

Para o botão **criar um fix it** na Home Page, o aplicativo corrigir ti embutiva um elemento de âncora:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample12.cshtml)]

Para links de exibição/ação como esse é melhor usar o auxiliar HTML [URL. Action](https://msdn.microsoft.com/library/system.web.mvc.urlhelper.action.aspx) , por exemplo:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample13.cshtml)]

### <a name="use-taskdelay-instead-of-threadsleep-in-worker-role"></a>Use Task. Delay em vez de thread. Sleep na função de trabalho

O modelo New-Project coloca `Thread.Sleep` o código de exemplo para uma função de trabalho, mas fazer com que o thread seja suspenso pode causar o pool de threads a gerar threads desnecessários adicionais. Você pode evitar isso usando [Task. Delay](https://msdn.microsoft.com/library/hh139096.aspx) em vez disso.

[!code-csharp[Main](the-fix-it-sample-application/samples/sample14.cs?highlight=11)]

### <a name="avoid-async-void"></a>Evitar Async void

Se um método assíncrono não precisar retornar um valor, retorne um `Task` tipo em vez de `void` .

Este exemplo é da `FixItQueueManager` classe:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample15.cs)]

Você deve usar `async void` somente para manipuladores de eventos de nível superior. Se você definir um método como `async void` , o chamador não poderá **aguardar** o método nem pegar nenhuma exceção que o método lançar. Para obter mais informações, consulte [práticas recomendadas em programação assíncrona](https://msdn.microsoft.com/magazine/jj991977.aspx).

### <a name="use-a-cancellation-token-to-break-from-worker-role-loop"></a>Usar um token de cancelamento para interromper o loop de função de trabalho

Normalmente, o método **Run** em uma função de trabalho contém um loop infinito. Quando a função de trabalho está parando, o método [RoleEntryPoint. OnStop](https://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx) é chamado. Você deve usar esse método para cancelar o trabalho que está sendo feito dentro do método **Run** e sair normalmente. Caso contrário, o processo pode ser encerrado no meio de uma operação.

### <a name="opt-out-of-automatic-mime-sniffing-procedure"></a>Recusar o procedimento de detecção de MIME automático

Em alguns casos, o Internet Explorer relata um tipo MIME diferente do tipo especificado pelo servidor Web. Por exemplo, se o Internet Explorer encontrar conteúdo HTML em um arquivo entregue com o cabeçalho de resposta HTTP Content-Type: text/plain, o Internet Explorer determinará que o conteúdo deve ser renderizado como HTML. Infelizmente, essa "detecção de MIME" também pode levar a problemas de segurança para servidores que hospedam conteúdo não confiável. Para combater esse problema, o Internet Explorer 8 fez várias alterações no código de determinação do tipo MIME e permite que os desenvolvedores [de aplicativos recusem a detecção de MIME](https://blogs.msdn.com/b/ie/archive/2008/07/02/ie8-security-part-v-comprehensive-protection.aspx). O código a seguir foi adicionado ao arquivo de *Web.config* .

[!code-xml[Main](the-fix-it-sample-application/samples/sample16.xml?highlight=2-7)]

### <a name="enable-bundling-and-minification"></a>Habilitar agrupamento e minificação

Quando o Visual Studio cria um novo projeto Web, o agrupamento e o minificação de arquivos JavaScript não são habilitados por padrão. Adicionamos uma linha de código em BundleConfig.cs:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample17.cs?highlight=9)]

### <a name="set-an-expiration-time-out-for-authentication-cookies"></a>Definir um tempo limite de expiração para cookies de autenticação

Por padrão, os cookies de autenticação expiram em duas semanas. Um tempo mais curto é mais seguro. Você pode alterar essa configuração em *StartupAuth.cs*:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample18.cs?highlight=4-5)]

<a id="run-in-vs"></a>
## <a name="how-to-run-the-app-from-visual-studio-on-your-local-computer"></a>Como executar o aplicativo do Visual Studio em seu computador local

Há duas maneiras de executar o aplicativo corrigir ti:

- Execute o aplicativo base que grava novas tarefas diretamente no banco de dados SQL.
- Execute o aplicativo usando uma fila mais um serviço de back-end para criar tarefas. O padrão de fila é descrito no capítulo [padrão de trabalho centrado na fila](queue-centric-work-pattern.md).

<a id="runbase"></a>
### <a name="run-the-base-application"></a>Executar o aplicativo base

1. Instalar o [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017).
2. Instale o [SDK do Azure para .net para Visual Studio](https://azure.microsoft.com/downloads/).
3. Baixe o arquivo. zip da [Galeria de códigos do MSDN](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4).
4. No explorador de arquivos, clique com o botão direito do mouse no arquivo. zip e clique em Propriedades e, em seguida, na janela Propriedades clique em desbloquear.
5. Descompacte o arquivo.
6. Clique duas vezes no arquivo. sln para iniciar o Visual Studio.
7. No menu **ferramentas** , clique em **Gerenciador de pacotes NuGet** e em **console do Gerenciador de pacotes**.
8. No console do Gerenciador de pacotes (PMC), clique em restaurar.
9. Saia do Visual Studio.
10. Inicie o [emulador de armazenamento do Azure](/azure/storage/common/storage-use-emulator).
11. Reinicie o Visual Studio, abrindo o arquivo da solução que você fechou na etapa anterior.
12. Verifique se o projeto FixIt está definido como o projeto de inicialização e pressione CTRL + F5 para executar o projeto.

<a id="queueslocal"></a>
### <a name="run-the-application-with-queue-processing"></a>Executar o aplicativo com processamento de fila

1. Siga as instruções para [executar o aplicativo base](#runbase)e feche o navegador e feche o Visual Studio.
2. Inicie o Visual Studio com privilégios de administrador. (Você usará o emulador de computação do Azure e isso exigirá privilégios de administrador.)
3. No arquivo de *Web.config* de aplicativos no projeto *MyFixIt* (o projeto Web), altere o valor de `appSettings/UseQueues` para "true":

    [!code-console[Main](the-fix-it-sample-application/samples/sample19.cmd?highlight=3)]
4. Se o [emulador de armazenamento do Azure](https://msdn.microsoft.com/library/windowsazure/hh403989.aspx) ainda não estiver em execução, inicie-o novamente.
5. Execute o projeto Web FixIt e o projeto MyFixItCloudService simultaneamente.

    Usando o Visual Studio:

   1. Pressione **F5** para executar o projeto Fixit.
   2. Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto MyFixItCloudService e clique em **depurar**  >  **Iniciar nova instância**.

    Usando o Visual Studio 2013 Express para Web:

   3. Em Gerenciador de Soluções, clique com o botão direito do mouse na solução FixIt e selecione **Propriedades**.
   4. Selecione **vários projetos de inicialização**.
   5. Na lista suspensa de **ações** em MyFixIt e MyFixItCloudService, selecione **Iniciar**.
   6. Clique em **OK**.
   7. Pressione **F5** para executar os dois projetos.

      Quando você executa o projeto MyFixItCloudService, o Visual Studio inicia o emulador de computação do Azure. Dependendo da configuração do firewall, talvez seja necessário permitir o emulador por meio do firewall.

<a id="deploybase"></a>
## <a name="how-to-deploy-the-base-app-to-azure-app-service-web-apps-by-using-the-windows-powershell-scripts"></a>Como implantar o aplicativo base em aplicativos Web do Azure App Service usando os scripts do Windows PowerShell

Para ilustrar o padrão [automatizar tudo](automate-everything.md) , o aplicativo corrigir ti é fornecido com scripts que configuram um ambiente no Azure e implantam o projeto no novo ambiente. As instruções a seguir explicam como usar os scripts.

Se você quiser executar o no Azure sem usar filas e tiver feito as alterações para executar localmente com filas, certifique-se de definir o valor UseQueues appSetting de volta como false antes de prosseguir com as instruções a seguir.

Estas instruções pressupõem que você já tenha baixado e executado a solução corrigir ti localmente e que tenha uma conta do Azure ou tenha uma assinatura do Azure que você está autorizado a gerenciar.

1. Instale o console do **Azure PowerShell** . Para saber mais, confira [Como instalar e configurar o PowerShell do Azure](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1).

    Este console personalizado está configurado para funcionar com sua assinatura do Azure. O módulo do Azure é instalado no diretório *arquivos de programas* e é importado automaticamente em cada uso do console do Azure PowerShell.

    Se você preferir trabalhar em um programa host diferente, como ISE do Windows PowerShell, use o cmdlet [Import-Module](https://go.microsoft.com/fwlink/?LinkID=141553) para importar o módulo do Azure ou use um comando no módulo do Azure para disparar a importação automática do módulo.
2. Inicie o Azure PowerShell com a opção **Executar como administrador** .
3. Execute o cmdlet [Set-ExecutionPolicy](https://go.microsoft.com/fwlink/p/?linkid=293941) para definir a política de execução de Azure PowerShell como `RemoteSigned` . Digite **Y** (para Sim) para concluir a alteração da política.

    [!code-console[Main](the-fix-it-sample-application/samples/sample20.cmd)]

    Essa configuração permite que você execute scripts locais que não são assinados digitalmente. (Você também pode definir a política de execução como `Unrestricted` , o que eliminaria a necessidade da etapa de desbloqueio mais tarde, mas isso não é recomendado por motivos de segurança.)
4. Execute o `Add-AzureAccount` cmdlet para configurar o PowerShell com credenciais para sua conta.

    [!code-console[Main](the-fix-it-sample-application/samples/sample21.cmd)]

    Essas credenciais expiram após um período de tempo e você precisa executar novamente o `Add-AzureAccount` cmdlet. Como este livro eletrônico está sendo gravado, o limite de tempo antes das credenciais expirar é de 12 horas.
5. Se você tiver várias assinaturas, use o cmdlet Select-AzureSubscription para especificar a assinatura na qual você deseja criar o ambiente de teste.
6. Importe um certificado de gerenciamento para a mesma assinatura do Azure usando `Get-AzurePublishSettingsFile` os `Import-AzurePublishSettingsFile` cmdlets e. O primeiro desses cmdlets baixa um arquivo de certificado e, no segundo, você especifica o local do arquivo para importá-lo. > [!IMPORTANT]
   > Mantenha o arquivo baixado em um local seguro ou exclua-o quando tiver terminado, pois ele contém um certificado que pode ser usado para gerenciar os serviços do Azure.

    [!code-console[Main](the-fix-it-sample-application/samples/sample22.cmd)]

    O certificado é usado para uma chamada à API REST que detecta o endereço IP do computador de desenvolvimento a fim de definir uma regra de firewall no servidor do banco de dados SQL.
7. Execute o cmdlet [Set-Location](https://go.microsoft.com/fwlink/p/?linkid=293912) (aliases são `cd` , `chdir` e `sl` ) para navegar até o diretório que contém os scripts. (Eles estão localizados na pasta de *automação* na pasta corrigir solução de ti.) Coloque o caminho entre aspas se qualquer um dos nomes de diretório contiver espaços. Por exemplo, para navegar até o `c:\Sample Apps\FixIt\Automation` diretório, você pode inserir o seguinte comando:

    [!code-console[Main](the-fix-it-sample-application/samples/sample23.cmd)]
8. Para permitir que o Windows PowerShell execute esses scripts, use o cmdlet [Unblock-File](https://go.microsoft.com/fwlink/p/?linkid=294021) . (Os scripts são bloqueados porque foram baixados da Internet.)

    > [!WARNING]
    > Segurança-antes de executar `Unblock-File` em qualquer script ou arquivo executável, abra o arquivo no bloco de notas, examine os comandos e verifique se eles não contêm nenhum código mal-intencionado.

    Por exemplo, o comando a seguir executa o `Unblock-File` cmdlet em todos os scripts no diretório atual.

    [!code-console[Main](the-fix-it-sample-application/samples/sample24.cmd)]
9. Para criar o aplicativo Web para a base (nenhum processamento de filas) corrigir o aplicativo de ti, execute o script de criação de ambiente.

    O `Name` parâmetro Required especifica o nome do banco de dados e também é usado para a conta de armazenamento que o script cria. O nome deve ser globalmente exclusivo no domínio azurewebsites.net. Se você especificar um nome que não seja exclusivo, como Fixit ou Test (ou até mesmo como no exemplo, fixitdemo), o `New-AzureWebsite` cmdlet falhará com um erro interno que relata um conflito. O script converte o nome em todas as letras minúsculas para estar em conformidade com os requisitos de nome para aplicativos Web, contas de armazenamento e bancos de dados.

    O `SqlDatabasePassword` parâmetro Required especifica a senha para a conta de administrador que será criada para o banco de dados SQL. Não inclua caracteres XML especiais na senha ( &amp; &lt; &gt; ;). Essa é uma limitação da maneira como os scripts foram gravados, não uma limitação do Azure.

    Por exemplo, se você quiser criar um aplicativo Web chamado "fixitdemo" e usar uma senha de administrador SQL Server de "Passw0rd1", você poderá inserir o seguinte comando:

    [!code-console[Main](the-fix-it-sample-application/samples/sample25.cmd)]

    O nome deve ser exclusivo no domínio azurewebsites.net e a senha deve atender aos requisitos do banco de dados SQL para a complexidade da senha. (O exemplo Passw0rd1 atende aos requisitos.)

    Observe que o comando começa com ". \" . Para ajudar a evitar a execução mal-intencionada de scripts, o Windows PowerShell exige que você forneça o caminho totalmente qualificado para o arquivo de script ao executar um script. Você pode usar um ponto para indicar o diretório atual (". \" ) ou forneça o caminho totalmente qualificado, como:

    [!code-console[Main](the-fix-it-sample-application/samples/sample26.cmd)]

    Para obter mais informações sobre o script, use o `Get-Help` cmdlet.

    [!code-console[Main](the-fix-it-sample-application/samples/sample27.cmd)]

    Você pode usar os `Detailed` `Full` parâmetros,, `Parameters` e `Examples` do cmdlet Get-Help para filtrar a ajuda retornada.

    Se o script falhar ou gerar erros, como "New-AzureWebsite: chamar Set-AzureSubscription e Select-AzureSubscription primeiro", talvez você não tenha concluído a configuração de Azure PowerShell.

    Depois que o script for concluído, você poderá usar o Portal de Gerenciamento do Azure para ver os recursos que foram criados, conforme mostrado no capítulo [automatizar tudo](automate-everything.md) .
10. Para implantar o projeto FixIt no novo ambiente do Azure, use o script *AzureWebsite.ps1* . Por exemplo:

    [!code-console[Main](the-fix-it-sample-application/samples/sample28.cmd)]

    Quando a implantação for concluída, o navegador será aberto, corrigindo-a em execução no Azure.

<a id="troubleshooting"></a>
## <a name="troubleshooting-the-windows-powershell-scripts"></a>Solucionando problemas dos scripts do Windows PowerShell

Os erros mais comuns encontrados ao executar esses scripts estão relacionados às permissões. Verifique se `Add-AzureAccount` o e `Import-AzurePublishSettingsFile` o foram bem-sucedidos e se você os usou para a mesma assinatura do Azure. Mesmo que tenha `Add-AzureAccount` sido bem-sucedido, talvez seja necessário executá-lo novamente. As permissões adicionadas `Add-AzureAccount` expiram em 12 horas.

### <a name="object-reference-not-set-to-an-instance-of-an-object"></a>a referência de objeto não está definida em uma instância de um objeto.

Se o script retornar erros, como "referência de objeto não definida para uma instância de um objeto", o que significa que o Windows PowerShell não pode localizar um objeto a ser processado (essa é uma exceção de referência nula), execute o `Add-AzureAccount` cmdlet e tente o script novamente.

[!code-console[Main](the-fix-it-sample-application/samples/sample29.cmd)]

### <a name="internalerror-the-server-encountered-an-internal-error"></a>InternalError: o servidor encontrou um erro interno.

O `New-AzureWebsite` cmdlet retorna um erro interno quando o nome não é exclusivo no domínio azurewebsites.net. Para resolver o erro, use um valor diferente para o nome, que está no parâmetro Name de *New-AzureWebsiteEnv.ps1*.

[!code-console[Main](the-fix-it-sample-application/samples/sample30.cmd)]

### <a name="restarting-the-script"></a>Reiniciando o script

Se você precisar reiniciar o script *New-AzureWebsiteEnv.ps1* porque ele falhou antes de imprimir a mensagem "script concluído", talvez você queira excluir os recursos criados pelo script antes de ele ser interrompido. Por exemplo, se o script já tiver criado o aplicativo Web ContosoFixItDemo e você executar o script novamente com o mesmo nome, o script falhará porque o nome está em uso.

Para determinar quais recursos o script criou antes de ser interrompido, use os seguintes cmdlets:

- `Get-AzureWebsite`
- `Get-AzureSqlDatabaseServer`
- `Get-AzureSqlDatabase`: Para executar este cmdlet, redirecione o nome do servidor de banco de dados para `Get-AzureSqlDatabase` :   `Get-AzureSqlDatabaseServer | Get-AzureSqlDatabase.`

Para excluir esses recursos, use os comandos a seguir. Observe que, se você excluir o servidor de banco de dados, excluirá automaticamente os bancos associados ao servidor.

- `Get-AzureWebsite -Name <WebsiteName> | Remove-AzureWebsite`
- `Get-AzureSqlDatabase -Name <DatabaseName> -ServerName <DatabaseServerName> | Remove-SqlAzureDatabase`
- `Get-AzureSqlDatabaseServer | Remove-AzureSqlDatabaseServer`

<a id="deployqueues"></a>
## <a name="how-to-deploy-the-app-with-queue-processing-to-azure-app-service-web-apps-and-an-azure-cloud-service"></a>Como implantar o aplicativo com processamento de fila para Azure App aplicativos Web do serviço e um serviço de nuvem do Azure

Para habilitar as filas, faça a seguinte alteração no arquivo de MyFixIt\Web.config. Em `appSettings` , altere o valor de `UseQueues` para "true":

[!code-xml[Main](the-fix-it-sample-application/samples/sample31.xml)]

Em seguida, implante o aplicativo MVC em um aplicativo Web no serviço Azure App, conforme descrito [anteriormente](#deploybase).

Em seguida, crie um novo serviço de nuvem do Azure. Os scripts incluídos no aplicativo corrigir ti não criam ou implantam o serviço de nuvem, portanto, você deve usar portal do Azure para isso. No portal, clique em **novo**  --  **computação** – **Cloud Service**  --  **criação rápida** do serviço de nuvem e insira uma URL e um local de data center. Use o mesmo data center em que você implantou o aplicativo Web.

![](the-fix-it-sample-application/_static/image1.png)

Para poder implantar o serviço de nuvem, você precisa atualizar alguns dos arquivos de configuração.

Em MyFixIt.WorkerRole\app.config, em `connectionStrings` , substitua o valor da `appdb` cadeia de conexão pela cadeia de conexão real para o banco de dados SQL. Você pode obter a cadeia de conexão do Portal. No portal do, clique em bancos de dados **SQL**  -  **appdb**  -  **exibir cadeias de conexão do SQL Database para ADO .net, ODBC, php e JDBC**. Copie a cadeia de conexão ADO.NET e cole o valor no arquivo app.config. Substitua "{sua \_ senha \_ aqui}" pela senha do banco de dados. (Supondo que você tenha usado os scripts para implantar o aplicativo MVC, você especificou a senha do banco de dados no `SqlDatabasePassword` parâmetro de script.)

O resultado deve ser semelhante ao seguinte:

[!code-xml[Main](the-fix-it-sample-application/samples/sample32.xml)]

No mesmo arquivo de MyFixIt.WorkerRole\app.config, em `appSettings` , substitua os dois valores de espaço reservado para a conta de armazenamento do Azure.

[!code-xml[Main](the-fix-it-sample-application/samples/sample33.xml?highlight=2-3)]

Você pode obter a chave de acesso no Portal. Consulte [como gerenciar contas de armazenamento](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account).

No MyFixItCloudService\ServiceConfiguration.Cloud.cscfg, substitua os mesmos dois valores de espaços reservados para a conta de armazenamento do Azure.

[!code-xml[Main](the-fix-it-sample-application/samples/sample34.xml?highlight=3)]

Agora você está pronto para implantar o serviço de nuvem. Em explorar solução, clique com o botão direito do mouse no projeto MyFixItCloudService e selecione **publicar**. Para obter mais informações, consulte "[implantar o aplicativo no Azure](https://www.windowsazure.com/develop/net/tutorials/multi-tier-web-site/2-download-and-run/#deployAz)", que está na parte 2 deste [tutorial](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36).

> [!div class="step-by-step"]
> [Anterior](more-patterns-and-guidance.md)
