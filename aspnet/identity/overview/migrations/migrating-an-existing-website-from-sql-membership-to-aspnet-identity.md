---
uid: identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity
title: Migrando um site existente da associação do SQL para o ASP.NET Identity-ASP.NET 4. x
author: Rick-Anderson
description: Este tutorial ilustra as etapas para migrar um aplicativo Web existente com dados de usuário e função criados usando a associação do SQL com o novo ASP.NET Identity s...
ms.author: riande
ms.date: 12/19/2014
ms.custom: seoapril2019
ms.assetid: 220d3d75-16b2-4240-beae-a5b534f06419
msc.legacyurl: /identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 2cfab96422340acc67ebb3b11f322ae1b258575d
ms.sourcegitcommit: 6727454a30f11ac2365e1cc8a37ef0005f5d15b3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/09/2020
ms.locfileid: "96934117"
---
# <a name="migrating-an-existing-website-from-sql-membership-to-aspnet-identity"></a>Migração de um site existente da Associação do SQL para a Identidade do ASP.NET

por [Rick Anderson](https://twitter.com/RickAndMSFT), [Suhas Joshi](https://github.com/suhasj)

> Este tutorial ilustra as etapas para migrar um aplicativo Web existente com dados de usuário e função criados usando a associação do SQL para o novo sistema de ASP.NET Identity. Essa abordagem envolve alterar o esquema de banco de dados existente para aquele necessário pelo ASP.NET Identity e conectar as classes antigas/novas a ele. Depois de adotar essa abordagem, depois que o banco de dados for migrado, futuras atualizações de identidade serão manipuladas sem esforço.

Para este tutorial, usaremos um modelo de aplicativo Web (Web Forms) criado usando o Visual Studio 2010 para criar dados de usuário e função. Em seguida, usaremos scripts SQL para migrar o banco de dados existente para tabelas necessárias para o sistema de identidade. Em seguida, instalaremos os pacotes NuGet necessários e adicionaremos novas páginas de gerenciamento de conta que usam o sistema de identidade para o gerenciamento de associação. Como um teste de migração, os usuários criados usando a associação do SQL devem ser capazes de fazer logon e novos usuários devem ser capazes de se registrar. Você pode encontrar o exemplo completo [aqui](https://github.com/aspnet/samples/tree/master/samples/aspnet/Identity/SQLMembership-Identity-OWIN/). Consulte também [migrando da associação do ASP.net para ASP.net Identity](https://travis.io/blog/2015/03/24/migrate-from-aspnet-membership-to-aspnet-identity/).

## <a name="getting-started"></a>Introdução

### <a name="creating-an-application-with-sql-membership"></a>Criando um aplicativo com associação do SQL

1. Precisamos começar com um aplicativo existente que usa a associação do SQL e que tem dados de usuário e função. Para a finalidade deste artigo, vamos criar um aplicativo Web no Visual Studio 2010.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image1.jpg)
2. Usando a ferramenta de configuração do ASP.NET, crie dois usuários: **oldAdminUser** e **oldUser.**

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image1.png)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image2.jpg)
3. Crie uma função chamada admin e adicione ' oldAdminUser ' como um usuário nessa função.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image2.png)
4. Crie uma seção de administrador do site com um default. aspx. Defina a marca de autorização no arquivo web.config para habilitar o acesso somente aos usuários em funções de administrador. Mais informações podem ser encontradas aqui [https://www.asp.net/web-forms/tutorials/security/roles/role-based-authorization-cs](../../../web-forms/overview/older-versions-security/roles/role-based-authorization-cs.md)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image3.png)
5. Exiba o banco de dados no Gerenciador de Servidores para entender as tabelas criadas pelo sistema de associação do SQL. Os dados de logon do usuário são armazenados nas \_ tabelas usuários do ASPNET e \_ Associação ASPNET, enquanto os dados de função são armazenados na \_ tabela funções ASPNET. Informações sobre quais usuários estão em quais funções são armazenadas na tabela ASPNET \_ UsersInRoles. Para o gerenciamento de associação básico, é suficiente para portar as informações nas tabelas acima para o sistema ASP.NET Identity.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image4.png)

### <a name="migrating-to-visual-studio-2013"></a>Migrando para o Visual Studio 2013

1. Instale o Visual Studio Express 2013 para Web ou Visual Studio 2013 juntamente com as [atualizações mais recentes](https://www.microsoft.com/download/details.aspx?id=44921).
2. Abra o projeto acima na versão instalada do Visual Studio. Se o SQL Server Express não estiver instalado no computador, um prompt será exibido quando você abrir o projeto, já que a cadeia de conexão usa o SQL Express. Você pode optar por instalar o SQL Express ou como solução alternativa para alterar a cadeia de conexão para o LocalDb. Para este artigo, vamos alterá-lo para o LocalDb.
3. Abra web.config e altere a cadeia de conexão de. SQLExpress para (LocalDb) v 11.0. Remova ' User Instance = true ' da cadeia de conexão.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image3.jpg)
4. Abra Gerenciador de Servidores e verifique se o esquema de tabela e os dados podem ser observados.
5. O sistema ASP.NET Identity funciona com a versão 4,5 ou superior da estrutura. Redirecione o aplicativo para 4,5 ou superior.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image5.png)

    Compile o projeto para verificar se não há erros.

### <a name="installing-the-nuget-packages"></a>Instalando os pacotes NuGet

1. Em Gerenciador de Soluções, clique com o botão direito do mouse no projeto &gt; **gerenciar pacotes NuGet**. Na caixa de pesquisa, digite "Asp.net Identity". Selecione o pacote na lista de resultados e clique em instalar. Aceite o contrato de licença clicando no botão "aceito". Observe que esse pacote instalará os pacotes de dependência: EntityFramework e Microsoft ASP.NET Identity Core. Da mesma forma, instale os seguintes pacotes (ignore os quatro últimos pacotes OWIN se você não quiser habilitar o logon OAuth):

   - Microsoft.AspNet.Identity.Owin
   - Microsoft.Owin.Host.SystemWeb
   - Microsoft. Owin. Security. Facebook
   - Microsoft. Owin. Security. Google
   - Microsoft. Owin. Security. MicrosoftAccount
   - Microsoft. Owin. Security. Twitter

     ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image6.png)

### <a name="migrate-database-to-the-new-identity-system"></a>Migrar banco de dados para o novo sistema de identidade

A próxima etapa é migrar o banco de dados existente para um esquema exigido pelo sistema ASP.NET Identity. Para conseguir isso, executamos um script SQL que tem um conjunto de comandos para criar novas tabelas e migrar informações de usuário existentes para as novas tabelas. O arquivo de script pode ser encontrado [aqui](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/SQLMembership-Identity-OWIN/Migrations.sql).

Este arquivo de script é específico para este exemplo. Se o esquema para as tabelas criadas usando a associação do SQL for personalizado ou modificado, os scripts precisarão ser alterados de acordo.

### <a name="how-to-generate-the-sql-script-for-schema-migration"></a>Como gerar o script SQL para a migração de esquema

Para que ASP.NET Identity classes funcionem prontamente com os dados dos usuários existentes, precisamos migrar o esquema de banco de dados para o que é necessário pelo ASP.NET Identity. Podemos fazer isso adicionando novas tabelas e copiando as informações existentes nessas tabelas. Por padrão ASP.NET Identity usa o EntityFramework para mapear as classes de modelo de identidade de volta para o banco de dados para armazenar/recuperar informações. Essas classes de modelo implementam as principais interfaces de identidade que definem objetos de usuário e função. As tabelas e as colunas no banco de dados baseiam-se nessas classes de modelo. As classes de modelo do EntityFramework no Identity v 2.1.0 e suas propriedades são conforme definido abaixo

| **IdentityUser** | **Tipo** | **IdentityRole** | **IdentityUserRole** | **IdentityUserLogin** | **IdentityUserClaim** |
| --- | --- | --- | --- | --- | --- |
| Id | cadeia de caracteres | Id | RoleId | ProviderKey | Id |
| Nome de Usuário | cadeia de caracteres | Nome | UserId | UserId | ClaimType |
| PasswordHash | cadeia de caracteres |  |  | LoginProvider | ClaimValue |
| SecurityStamp | cadeia de caracteres |  |  |  | ID de usuário \_ |
| Email | cadeia de caracteres |  |  |  |  |
| EmailConfirmed | bool |  |  |  |  |
| PhoneNumber | cadeia de caracteres |  |  |  |  |
| PhoneNumberConfirmed | bool |  |  |  |  |
| LockoutEnabled | bool |  |  |  |  |
| LockoutEndDate | DateTime |  |  |  |  |
| AccessFailedCount | INT |  |  |  |  |

Precisamos ter tabelas para cada um desses modelos com colunas correspondentes às propriedades. O mapeamento entre classes e tabelas é definido no `OnModelCreating` método do `IdentityDBContext` . Isso é conhecido como o método de API fluente de configuração e mais informações podem ser encontradas [aqui](https://msdn.microsoft.com/data/jj591617.aspx). A configuração das classes é conforme mencionado abaixo

| **Classe** | **Tabela** | **Chave primária** | **Chave estrangeira** |
| --- | --- | --- | --- |
| IdentityUser | AspnetUsers | Id |  |
| IdentityRole | AspnetRoles | Id |  |
| IdentityUserRole | AspnetUserRole | UserId + RoleID | \_ID de usuário- &gt; AspnetUsers RoleID- &gt; AspnetRoles |
| IdentityUserLogin | AspnetUserLogins | ProviderKey + UserId + Loginprovider | UserId- &gt; AspnetUsers |
| IdentityUserClaim | AspnetUserClaims | Id | \_ID de usuário- &gt; AspnetUsers |

Com essas informações, podemos criar instruções SQL para criar novas tabelas. Podemos escrever cada instrução individualmente ou gerar o script inteiro usando os comandos do EntityFramework PowerShell, que podemos editar conforme necessário. Para fazer isso, no VS Abra o **console do Gerenciador de pacotes** no menu **Exibir** ou **ferramentas**

- Execute o comando "Enable-Migrations" para habilitar as migrações do EntityFramework.
- Execute o comando "Add-Migration Initial", que cria o código inicial de instalação para criar o banco de dados em C#/VB.
- A etapa final é executar o comando "Update-Database-script" que gera o script SQL com base nas classes de modelo.

[!INCLUDE[](../../../includes/identity/alter-command-exception.md)]

Esse script de geração de banco de dados pode ser usado como um início onde vamos fazer alterações adicionais para adicionar novas colunas e copiar dados. A vantagem disso é que geramos a `_MigrationHistory` tabela que é usada pelo EntityFramework para modificar o esquema de banco de dados quando as classes de modelo mudam para versões futuras de versões de identidade.

As informações de usuário da associação do SQL tinham outras propriedades, além daquelas na classe de modelo de usuário de identidade, por email, tentativas de senha, última data de logon, data do último bloqueio, etc. Essas são informações úteis e gostaríamos que ela fosse transportada para o sistema de identidade. Isso pode ser feito adicionando propriedades adicionais ao modelo de usuário e mapeando-as de volta para as colunas de tabela no banco de dados. Podemos fazer isso adicionando uma classe que subclasses o `IdentityUser` modelo. Podemos adicionar as propriedades a essa classe personalizada e editar o script SQL para adicionar as colunas correspondentes ao criar a tabela. O código para essa classe é descrito mais adiante no artigo. O script SQL para criar a `AspnetUsers` tabela depois de adicionar as novas propriedades seria

[!code-sql[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample1.sql)]

Em seguida, precisamos copiar as informações existentes do banco de dados de associação do SQL para as tabelas adicionadas recentemente para identidade. Isso pode ser feito por meio do SQL copiando dados diretamente de uma tabela para outra. Para adicionar dados às linhas da tabela, usamos a `INSERT INTO [Table]` construção. Para copiar de outra tabela, podemos usar a `INSERT INTO` instrução junto com a `SELECT` instrução. Para obter todas as informações de usuário, precisamos consultar as tabelas de membros do ASPNET e de *\_ associação do ASPNET* e copiar os dados para a tabela *\_* *AspNetUsers* . Usamos o `INSERT INTO` e o `SELECT` juntamente com `JOIN` as `LEFT OUTER JOIN` instruções e. Para obter mais informações sobre como consultar e copiar dados entre tabelas, consulte [este](https://technet.microsoft.com/library/ms190750%28v=sql.105%29.aspx) link. Além disso, as tabelas AspnetUserLogins e AspnetUserClaims estão vazias para começar, já que não há nenhuma informação na associação do SQL que é mapeada para isso por padrão. As únicas informações copiadas são para usuários e funções. Para o projeto criado nas etapas anteriores, a consulta SQL para copiar informações para a tabela usuários seria

[!code-sql[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample2.sql)]

Na instrução SQL acima, as informações sobre cada usuário das tabelas *\_ usuários do ASPNET* e *\_ Associação ASPNET* são copiadas nas colunas da tabela *AspnetUsers* . A única modificação feita aqui é quando copiamos a senha. Como o algoritmo de criptografia para senhas na associação do SQL usou ' PasswordSalt ' e ' PasswordFormat ', copiamos isso também com a senha de hash para que ele possa ser usado para descriptografar a senha por identidade. Isso é explicado mais detalhadamente no artigo ao conectar um hash personalizado de senha.

Este arquivo de script é específico para este exemplo. Para aplicativos que têm tabelas adicionais, os desenvolvedores podem seguir uma abordagem semelhante para adicionar propriedades adicionais na classe de modelo de usuário e mapeá-las para colunas na tabela AspnetUsers. Para executar o script,

1. Abra o Gerenciador de Servidores. Expanda a conexão ' ApplicationServices ' para exibir as tabelas. Clique com o botão direito do mouse no nó tabelas e selecione a opção ' nova consulta '

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image7.png)
2. Na janela de consulta, copie e cole todo o script SQL do arquivo Migrations. Sql. Execute o arquivo de script pressionando o botão de seta ' Executar '.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image4.jpg)

    Atualize a janela de Gerenciador de Servidores. Cinco novas tabelas são criadas no banco de dados.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image8.png)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image9.png)

    Veja abaixo como as informações nas tabelas de associação do SQL são mapeadas para o novo sistema de identidade.

    \_funções ASPNET-- &gt; AspNetRoles

    ASP \_ Netusers e ASP \_ netmembership-- &gt; AspNetUsers

    ASPNET \_ UserInRoles-- &gt; AspNetUserRoles

    Conforme explicado na seção acima, as tabelas AspNetUserClaims e AspNetUserLogins estão vazias. O campo ' discriminador ' na tabela AspNetUser deve corresponder ao nome da classe do modelo que é definido como uma próxima etapa. Além disso, a coluna PasswordHash está no formato "senha criptografada | sal de senha | formato de senha". Isso permite que você use uma lógica de criptografia de associação SQL especial para que você possa reutilizar senhas antigas. Isso é explicado posteriormente neste artigo.

### <a name="creating-models-and-membership-pages"></a>Criando modelos e páginas de associação

Como mencionado anteriormente, o recurso de identidade usa Entity Framework para se comunicar com o banco de dados para armazenar informações de conta por padrão. Para trabalhar com os dados existentes na tabela, precisamos criar classes de modelo que mapeiem de volta para as tabelas e conectá-las no sistema de identidade. Como parte do contrato de identidade, as classes de modelo devem implementar as interfaces definidas na DLL Identity. Core ou podem estender a implementação existente dessas interfaces disponíveis em Microsoft. AspNet. Identity. EntityFramework.

Em nosso exemplo, as tabelas AspNetRoles, AspNetUserClaims, AspNetLogins e AspNetUserRole têm colunas semelhantes à implementação existente do sistema de identidade. Portanto, podemos reutilizar as classes existentes para mapear para essas tabelas. A tabela AspNetUser tem algumas colunas adicionais que são usadas para armazenar informações adicionais das tabelas de associação do SQL. Isso pode ser mapeado pela criação de uma classe de modelo que estenda a implementação existente de ' IdentityUser ' e adicione as propriedades adicionais.

1. Crie uma pasta modelos no projeto e adicione um usuário de classe. O nome da classe deve corresponder aos dados adicionados na coluna ' discriminador ' da tabela ' AspnetUsers '.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image10.png)

    A classe de usuário deve estender a classe IdentityUser encontrada na dll *Microsoft. AspNet. Identity. EntityFramework* . Declare as propriedades na classe que mapeia de volta para as colunas AspNetUser. As propriedades ID, username, PasswordHash e SecurityStamp são definidas no IdentityUser e, portanto, são omitidas. Abaixo está o código para a classe de usuário que tem todas as propriedades

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample3.cs)]
2. Uma classe DbContext Entity Framework é necessária para persistir dados em modelos de volta para tabelas e recuperar dados de tabelas para popular os modelos. A dll *Microsoft. AspNet. Identity. EntityFramework* define a classe IdentityDbContext que interage com as tabelas de identidade para recuperar e armazenar informações. IdentityDbContext &lt; tuser &gt; usa uma classe ' tuser ' que pode ser qualquer classe que estenda a classe IdentityUser.

    Crie uma nova classe ApplicationDBContext que estenda IdentityDbContext na pasta ' Models ', passando a classe ' user ' criada na etapa 1

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample4.cs)]
3. O gerenciamento de usuários no novo sistema de identidade é feito usando a &lt; classe usermanager tuser &gt; definida na dll *Microsoft. AspNet. Identity. EntityFramework* . Precisamos criar uma classe personalizada que estenda o usermanager, passando a classe ' user ' criada na etapa 1.

    Na pasta modelos, crie uma nova classe usermanager que estenda o usuário do usermanager &lt;&gt;

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample5.cs)]
4. As senhas dos usuários do aplicativo são criptografadas e armazenadas no banco de dados do. O algoritmo de criptografia usado na associação do SQL é diferente daquele no novo sistema de identidade. Para reutilizar senhas antigas, precisamos descriptografar as senhas seletivamente quando os usuários antigos fizerem logon usando o algoritmo associações do SQL ao usar o algoritmo de criptografia em identidade para os novos usuários.

    A classe usermanager tem uma propriedade ' PasswordHasher ' que armazena uma instância de uma classe que implementa a interface ' IPasswordHasher '. Isso é usado para criptografar/descriptografar senhas durante transações de autenticação do usuário. Na classe usermanager definida na etapa 3, crie uma nova classe SQLPasswordHasher e copie o código abaixo.

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample6.cs)]

    Resolva os erros de compilação importando os namespaces de System. Text e System. Security. Cryptography.

    O método EncodePassword criptografa a senha de acordo com a implementação de criptografia de associação do SQL padrão. Isso é obtido da DLL System. Web. Se o aplicativo antigo usava uma implementação personalizada, ele deve ser refletido aqui. Precisamos definir dois outros métodos *HashPassword* e *VerifyHashedPassword* que usam o método *EncodePassword* para aplicar hash a uma determinada senha ou verificar uma senha de texto sem formatação com aquela existente no banco de dados.

    O sistema de associação do SQL usou PasswordHash, PasswordSalt e PasswordFormat para fazer o hash da senha inserida pelos usuários quando eles registram ou alteram sua senha. Durante a migração, todos os três campos são armazenados na coluna PasswordHash da tabela AspNetUser, separados pelo caractere ' | '. Quando um usuário faz logon e a senha tem esses campos, usamos a criptografia de associação do SQL para verificar a senha; caso contrário, usamos a criptografia padrão do sistema de identidade para verificar a senha. Dessa forma, os usuários antigos não precisariam alterar suas senhas depois que o aplicativo for migrado.
5. Declare o construtor para a classe usermanager e transmita-o como o SQLPasswordHasher para a propriedade no construtor.

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample7.cs)]

### <a name="create-new-account-management-pages"></a>Criar novas páginas de gerenciamento de conta

A próxima etapa da migração é adicionar páginas de gerenciamento de conta que permitirão que um usuário se registre e faça logon. As páginas da conta antiga da associação do SQL usam controles que não funcionam com o novo sistema de identidade. Para adicionar as novas páginas de gerenciamento de usuário, siga o tutorial neste link, [https://www.asp.net/identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project](../getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project.md) começando na etapa ' adicionando Web Forms para registrar usuários no seu aplicativo ', pois já criamos o projeto e adicionamos os pacotes NuGet.

Precisamos fazer algumas alterações para que o exemplo funcione com o projeto que temos aqui.

- As classes code-behind Register.aspx.cs e Login.aspx.cs usam o `UserManager` dos pacotes de identidade para criar um usuário. Para este exemplo, use o usermanager adicionado na pasta modelos seguindo as etapas mencionadas anteriormente.
- Use a classe de usuário criada em vez do IdentityUser em Register.aspx.cs e Login.aspx.cs Code-behind classes. Isso se conecta à nossa classe de usuário personalizada no sistema de identidade.
- A parte para criar o banco de dados pode ser ignorada.
- O desenvolvedor precisa definir ApplicationId para que o novo usuário corresponda à ID do aplicativo atual. Isso pode ser feito consultando o ApplicationId deste aplicativo antes de um objeto de usuário ser criado na classe Register.aspx.cs e definindo-o antes de criar o usuário.

    Exemplo:

    Defina um método na página Register.aspx.cs para consultar a \_ tabela de aplicativos ASPNET e obter a ID do aplicativo de acordo com o nome do aplicativo

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample8.cs)]

    Agora, defina isso no objeto de usuário

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample9.cs)]

Use o nome de usuário e a senha antigos para fazer logon em um existente. Use a página registrar para criar um novo usuário. Verifique também se os usuários estão em funções como esperado.

Portar para o sistema de identidade ajuda o usuário a adicionar o OAuth (autenticação aberta) ao aplicativo. Consulte o exemplo [aqui](https://github.com/aspnet/samples/tree/master/samples/aspnet/Identity/SQLMembership-Identity-OWIN/) , que tem o OAuth habilitado.

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, mostramos como portar usuários da associação do SQL para ASP.NET Identity, mas não portamos dados de perfil. No próximo tutorial, vamos examinar a porta de dados de perfil da associação do SQL para o novo sistema de identidade.

Você pode deixar comentários na parte inferior deste artigo.

*Graças a Tom Dykstra e Rick Anderson pela revisão do artigo.*
