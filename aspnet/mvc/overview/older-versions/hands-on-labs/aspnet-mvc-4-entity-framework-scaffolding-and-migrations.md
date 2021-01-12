---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
title: ASP.NET MVC 4 Entity Framework scaffolding e migrações | Microsoft Docs
author: rick-anderson
description: Se você estiver familiarizado com os métodos do controlador ASP.NET MVC 4 ou tiver concluído os &quot; auxiliares, formulários e validação &quot; Hands-On laboratório, você deve estar ciente...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 093c1362-f10b-407c-a708-be370f4b62b0
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
msc.type: authoredcontent
ms.openlocfilehash: 58aafe862f56ab13bf5a6bf8908a3baf582a08ff
ms.sourcegitcommit: d4e2a07eeb2cdf19f0bfbfab4a469970bc7e1c99
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/12/2021
ms.locfileid: "98105267"
---
# <a name="aspnet-mvc-4-entity-framework-scaffolding-and-migrations"></a>Migrações e scaffolding do Entity Framework do ASP.NET MVC 4

Por [equipe de acampamentos da Web](https://twitter.com/webcamps)

[Baixe o kit de treinamento do Web acampamentos](https://aka.ms/webcamps-training-kit)

Se você estiver familiarizado com os métodos do controlador ASP.NET MVC 4 ou tiver concluído os &quot; auxiliares, formulários e validação &quot; Hands-On laboratório, você deve estar ciente de que grande parte da lógica para criar, atualizar, listar e remover qualquer entidade de dados é repetida em todo o aplicativo. Para não mencionar que, se seu modelo tiver várias classes a serem manipuladas, você provavelmente gastará um tempo considerável escrevendo os métodos POST e GET de ação para cada operação de entidade, bem como cada uma das exibições.

Neste laboratório, você aprenderá a usar o ASP.NET MVC 4 scaffolding para gerar automaticamente a linha de base do CRUD do seu aplicativo (criar, ler, atualizar e excluir). A partir de uma classe de modelo simples e, sem escrever uma única linha de código, você criará um controlador que conterá todas as operações CRUD, bem como todas as exibições necessárias. Depois de criar e executar a solução simples, você terá o banco de dados do aplicativo gerado, junto com a lógica MVC e exibições para manipulação de dados.

Além disso, você aprenderá como é fácil usar Entity Framework migrações para executar atualizações de modelo em todo o aplicativo. Entity Framework migrações permitirão que você modifique seu banco de dados depois que o modelo for alterado com etapas simples. Com tudo isso em mente, você poderá criar e manter aplicativos Web com mais eficiência, aproveitando os recursos mais recentes do ASP.NET MVC 4.

> [!NOTE]
> Todos os códigos de exemplo e trechos de código estão incluídos no Web acampamentos Training Kit, disponível em [Microsoft-Web/WebCampTrainingKit releases](https://aka.ms/webcamps-training-kit). O projeto específico deste laboratório está disponível em [ASP.NET MVC 4 Entity Framework scaffolding e migrações](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations).

<a id="Objectives"></a>
### <a name="objectives"></a>Objetivos

Neste Hands-On laboratório, você aprenderá a:

- Use ASP.NET scaffolding para operações CRUD em controladores.
- Altere o modelo de banco de dados usando migrações Entity Framework.

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Pré-requisitos

Você deve ter os seguintes itens para concluir este laboratório:

- [Microsoft Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) ou superior (leia [o apêndice a](#AppendixA) para obter instruções sobre como instalá-lo).

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Instalação

**Instalando trechos de código**

Para sua conveniência, grande parte do código que você gerenciará ao longo deste laboratório está disponível como trechos de código do Visual Studio. Para instalar os trechos de código, execute o arquivo **.\Source\Setup\CodeSnippets.VSI** .

Se você não estiver familiarizado com os trechos de Visual Studio Code e quiser saber como usá-los, consulte o apêndice deste documento &quot; [Apêndice B: usando trechos de código](#AppendixB) &quot; .

---

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Exercícios

O exercício a seguir compõe este Hands-On Lab:

1. [Usando o ASP.NET MVC 4 scaffolding com migrações Entity Framework](#Exercise1)

> [!NOTE]
> Este exercício é acompanhado por uma pasta **final** que contém a solução resultante que você deve obter depois de concluir o exercício. Você pode usar essa solução como um guia se precisar de ajuda adicional para trabalhar com o exercício.

Tempo estimado para concluir este laboratório: **30 minutos**

<a id="Exercise1"></a>

<a id="Exercise_1_Using_ASPNET_MVC_4_Scaffolding_with_Entity_Framework_Migrations"></a>
### <a name="exercise-1-using-aspnet-mvc-4-scaffolding-with-entity-framework-migrations"></a>Exercício 1: usando o ASP.NET MVC 4 scaffolding com migrações Entity Framework

O ASP.NET MVC scaffolding fornece uma maneira rápida de gerar as operações CRUD de forma padronizada, criando a lógica necessária que permite que o aplicativo interaja com a camada de banco de dados.

Neste exercício, você aprenderá a usar o ASP.NET MVC 4 scaffolding com o Code First para criar os métodos CRUD. Em seguida, você aprenderá a atualizar seu modelo aplicando as alterações no banco de dados usando Entity Framework migrações.

<a id="Ex1Task1"></a>

<a id="Task_1-_Creating_a_new_ASPNET_MVC_4_project_using_Scaffolding"></a>
#### <a name="task-1--creating-a-new-aspnet-mvc-4-project-using-scaffolding"></a>Tarefa 1-Criando um novo projeto do MVC 4 do ASP.NET usando scaffolding

1. Se ainda não estiver aberto, inicie o **Visual Studio 2012**.
2. Selecionar **arquivo | Novo projeto**. Na caixa de diálogo novo projeto, no **Visual C# |** , Selecione **aplicativo web do ASP.NET MVC 4**. Nomeie o projeto para **MVC4andEFMigrations** e defina o local como **Source\Ex1-UsingMVC4ScaffoldingEFMigrations** pasta deste laboratório. Defina o **nome da solução** como **Iniciar** e verifique se a opção **criar diretório para solução** está marcada. Clique em **OK**.

    ![Caixa de diálogo novo projeto do ASP.NET MVC 4](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "Caixa de diálogo novo projeto do ASP.NET MVC 4")

    *Caixa de diálogo novo projeto do ASP.NET MVC 4*
3. Na caixa de diálogo **novo projeto do ASP.NET MVC 4** , selecione o modelo de **aplicativo da Internet** e verifique se o **Razor** é o **mecanismo de exibição** selecionado. Clique em **OK** para criar o projeto.

    ![Novo aplicativo de Internet ASP.NET MVC 4](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "Novo aplicativo de Internet ASP.NET MVC 4")

    *Novo aplicativo de Internet ASP.NET MVC 4*
4. Na Gerenciador de Soluções, clique com o botão direito do mouse em **modelos** e selecione **Adicionar | Classe** para criar uma pessoa de classe simples (POCO). Nomeie-a como **pessoa** e clique em **OK**.
5. Abra a classe Person e insira as propriedades a seguir.

    (Trecho de código- *ASP.NET MVC 4 e Entity Framework migrações-EX1 Propriedades da pessoa*)

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample1.cs)]
6. Clique em **Compilar | Compile a solução** para salvar as alterações e compilar o projeto.

    ![Compilando o aplicativo](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "Compilando o aplicativo")

    *Compilando o aplicativo*
7. Na Gerenciador de Soluções, clique com o botão direito do mouse na pasta controladores e selecione **Adicionar | Controlador**.
8. Nomeie o controlador *PersonController* e conclua as **Opções de scaffolding** com os valores a seguir.

   1. Na lista suspensa **modelo** , selecione o **controlador MVC com ações de leitura/gravação e exibições, usando Entity Framework** opção.
   2. Na lista suspensa **classe de modelo** , selecione a classe **Person** .
   3. Na lista **classe de contexto de dados** , selecione **&lt; novo contexto de dados. &gt; ..**. Escolha qualquer nome e clique em **OK**.
   4. Na lista suspensa **exibições** , verifique se o **Razor** está selecionado.

      ![Adicionando o controlador Person com scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "Adicionando o controlador Person com scaffolding")

      *Adicionando o controlador Person com scaffolding*
9. Clique em **Adicionar** para criar o novo controlador para pessoa com scaffolding. Agora, você gerou as ações do controlador, bem como as exibições.

    ![Depois de criar o controlador Person com scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "Depois de criar o controlador Person com scaffolding")

    *Depois de criar o controlador Person com scaffolding*
10. Abra a classe **PersonController** . Observe que os métodos de ação CRUD completos foram gerados automaticamente.

   ![Dentro do controlador Person](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "Dentro do controlador Person")

   *Dentro do controlador Person*

<a id="Ex1Task2"></a>

<a id="Task_2-_Running_the_application"></a>
#### <a name="task-2--running-the-application"></a>Tarefa 2-executando o aplicativo

Neste ponto, o banco de dados ainda não foi criado. Nesta tarefa, você executará o aplicativo pela primeira vez e testará as operações CRUD. O banco de dados será criado instantaneamente com Code First.

1. Pressione **F5** para executar o aplicativo.
2. No navegador, adicione **/Person** à URL para abrir a página pessoa.

    ![Execução do aplicativo pela primeira vez](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "Execução do aplicativo pela primeira vez")

    *Aplicativo: primeira execução*
3. Agora, você explorará as páginas da pessoa e testará as operações CRUD.

    1. Clique em **criar novo** para adicionar uma nova pessoa. Insira um nome e sobrenome e clique em **criar**.

        ![Adicionando uma nova pessoa](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "Adicionando uma nova pessoa")

        *Adicionando uma nova pessoa*
    2. Na lista da pessoa, você pode excluir, editar ou adicionar itens.

        ![lista de pessoas](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "lista de pessoas")

        *Lista de pessoas*
    3. Clique em **detalhes** para abrir os detalhes da pessoa.

        ![Detalhes da pessoa](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "Detalhes da pessoa")

        *Detalhes da pessoa*
4. Feche o navegador e retorne ao Visual Studio. Observe que você criou todo o CRUD para a entidade Person em todo o seu aplicativo – do modelo para as exibições, sem precisar escrever uma única linha de código!

<a id="Ex1Task3"></a>

<a id="Task_3-_Updating_the_database_using_Entity_Framework_Migrations"></a>
#### <a name="task-3--updating-the-database-using-entity-framework-migrations"></a>Tarefa 3-atualizando o banco de dados usando migrações Entity Framework

Nesta tarefa, você atualizará o banco de dados usando Entity Framework migrações. Você descobrirá como é fácil alterar o modelo e refletir as alterações em seus bancos de dados usando o recurso de migrações Entity Framework.

1. Abra o Console do Gerenciador de Pacotes. Selecione **Ferramentas** > **Gerenciador de Pacotes NuGet** > **Console do Gerenciador de Pacotes**.
2. No Console do Gerenciador de Pacotes, digite o seguinte comando:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample2.ps1)]

    ![Habilitando as migrações](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "Habilitando as migrações")

    *Habilitando migrações*

    O comando Enable-Migration cria a pasta **migrações** , que contém um script para inicializar o banco de dados.

    ![Pasta de migrações](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "Pasta de migrações")

    *Pasta de migrações*
3. Abra o arquivo **Configuration.cs** na pasta Migrations. Localize o construtor da classe e altere o valor **AutomaticMigrationsEnabled** para *true*.

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample3.cs)]
4. Abra a classe Person e adicione um atributo para o nome do meio da pessoa. Com esse novo atributo, você está alterando o modelo.

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample4.cs)]
5. Selecionar **compilação | Compile a solução** no menu para compilar o aplicativo.

    ![Compilando o aplicativo](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "Compilando o aplicativo")

    *Compilando o aplicativo*
6. No Console do Gerenciador de Pacotes, digite o seguinte comando:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample5.ps1)]

    Esse comando procurará alterações nos objetos de dados e, em seguida, adicionará os comandos necessários para modificar o banco de dado de acordo.

    ![Adicionando um nome do meio](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "Adicionando um nome do meio")

    *Adicionando um nome do meio*
7. Adicional Você pode executar o comando a seguir para gerar um script SQL com a atualização diferencial. Isso permitirá que você atualize o banco de dados manualmente (neste caso, ele não é necessário) ou aplique as alterações em outros bancos:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample6.ps1)]

    ![Gerando um script SQL](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "Gerando um script SQL")

    *Gerando um script SQL*

    ![Atualização de script SQL](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "Atualização de script SQL")

    *Atualização de script SQL*
8. No console do Gerenciador de pacotes, digite o seguinte comando para atualizar o banco de dados:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample7.ps1)]

    ![Atualizar o banco de dados](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "Atualizar o banco de dados")

    *Atualizar o banco de dados*

    Isso adicionará a coluna **MiddleName** na tabela **People** para corresponder à definição atual da classe **Person** .
9. Depois que o banco de dados for atualizado, clique com o botão direito do mouse na pasta controlador e selecione **Adicionar | Controlador** para adicionar o controlador Person novamente (completo com os mesmos valores). Isso atualizará os métodos e as exibições existentes adicionando o novo atributo.

    ![Adicionando uma atualização de controlador](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "Adicionando uma atualização de controlador")

    *Atualizando o controlador*
10. Clique em **Adicionar**. Em seguida, selecione os valores **substituir PersonController.cs** e as **exibições de substituição associadas** e clique em **OK**.

   ![Adicionando uma substituição de controlador](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image19.png)

   *Atualizando o controlador*

<a id="Ex1Task4"></a>

<a id="Task4-_Running_the_application"></a>
#### <a name="task4--running-the-application"></a>Task4-executando o aplicativo

1. Pressione **F5** para executar o aplicativo.
2. Abra **/Person**. Observe que os dados foram preservados, enquanto a coluna de nome do meio foi adicionada.

    ![Nome do meio adicionado](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "Nome do meio adicionado")

    *Nome do meio adicionado*
3. Se você clicar em **Editar**, poderá adicionar um segundo nome à pessoa atual.

    ![Segundo nome edição](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "Segundo nome edição")

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Resumo

Neste Hands-On laboratório, você aprendeu etapas simples para criar operações CRUD com ASP.NET MVC 4 scaffolding usando qualquer classe de modelo. Em seguida, você aprendeu como executar uma atualização de ponta a ponta em seu aplicativo – do banco de dados para as exibições, usando Entity Framework migrações.

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Apêndice A: Instalando o Visual Studio Express 2012 para Web

Você pode instalar o **Microsoft Visual Studio Express 2012 para Web** ou &quot; outra &quot; versão Express usando o **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. As instruções a seguir orientam você pelas etapas necessárias para instalar o *Visual Studio Express 2012 para Web* usando *Microsoft Web Platform Installer*.

1. Acesse [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169). Como alternativa, se você já tiver instalado o Web Platform Installer, poderá abri-lo e procurar o produto &quot; <em>Visual Studio Express 2012 para Web com o SDK do Windows Azure</em> &quot; .
2. Clique em **instalar agora**. Se você não tiver **Web Platform Installer** você será redirecionado para baixar e instalá-lo primeiro.
3. Quando **Web Platform Installer** estiver aberto, clique em **instalar** para iniciar a instalação.

    ![Instalar Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "Instalar Visual Studio Express")

    *Instalar Visual Studio Express*
4. Leia todos os termos e licenças de produtos e clique em **aceito** para continuar.

    ![Aceitando os termos de licença](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image23.png)

    *Aceitando os termos de licença*
5. Aguarde até que o processo de download e instalação seja concluído.

    ![Progresso da instalação](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image24.png)

    *Progresso da instalação*
6. Quando a instalação for concluída, clique em **concluir**.

    ![Instalação concluída](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image25.png)

    *Instalação concluída*
7. Clique em **sair** para fechar Web Platform Installer.
8. Para abrir o Visual Studio Express para Web, vá para a tela **Iniciar** e comece a escrever &quot; **vs Express** e &quot; , em seguida, clique no bloco **vs Express para Web** .

    ![Bloco VS Express para Web](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image26.png)

    *Bloco VS Express para Web*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>Apêndice B: usando trechos de código

Com trechos de código, você tem todo o código de que precisa ao seu alcance. O documento de laboratório informará exatamente quando você pode usá-los, conforme mostrado na figura a seguir.

![Usando trechos de código do Visual Studio para inserir código em seu projeto](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "Usando trechos de código do Visual Studio para inserir código em seu projeto")

*Usando trechos de código do Visual Studio para inserir código em seu projeto*

***Para adicionar um trecho de código usando o teclado (C# somente)** _

1. Coloque o cursor onde você deseja inserir o código.
2. Comece digitando o nome do trecho de código (sem espaços ou hifens).
3. Observe como o IntelliSense exibe nomes de trechos de código correspondentes.
4. Selecione o trecho correto (ou continue digitando até que o nome do trecho inteiro seja selecionado).
5. Pressione a tecla TAB duas vezes para inserir o trecho de código no local do cursor.

![Comece a digitar o nome do trecho](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "Comece a digitar o nome do trecho")

_Start digitando o nome do trecho de código *

![Pressione Tab para selecionar o trecho realçado](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "Pressione Tab para selecionar o trecho realçado")

*Pressione Tab para selecionar o trecho realçado*

![Pressione Tab novamente e o trecho será expandido](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "Pressione Tab novamente e o trecho será expandido")

*Pressione Tab novamente e o trecho será expandido*

***Para adicionar um trecho de código usando o mouse (C#, Visual Basic e XML)** _ 1. Clique com o botão direito do mouse no local em que você deseja inserir o trecho de código.

1. Selecione _ *Inserir trecho** seguido por **meus trechos de código**.
2. Selecione o trecho relevante na lista clicando nele.

![Clique com o botão direito do mouse em onde você deseja inserir o trecho de código e selecione Inserir trecho](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "Clique com o botão direito do mouse em onde você deseja inserir o trecho de código e selecione Inserir trecho")

*Clique com o botão direito do mouse em onde você deseja inserir o trecho de código e selecione Inserir trecho*

![Selecione o trecho relevante na lista clicando nele](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "Selecione o trecho relevante na lista clicando nele")

*Selecione o trecho relevante na lista clicando nele*
