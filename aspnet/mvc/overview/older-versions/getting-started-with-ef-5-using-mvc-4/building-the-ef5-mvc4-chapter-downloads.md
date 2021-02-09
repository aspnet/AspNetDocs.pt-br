---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
title: Criando o capítulo downloads para os tutoriais do EF 5 MVC 4 | Microsoft Docs
author: Rick-Anderson
description: O aplicativo Web de exemplo da Contoso University demonstra como criar aplicativos ASP.NET MVC 4 usando o Entity Framework 5 Code First e o Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: d0a89089-eed8-4f61-a478-c5ffa30186f5
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
msc.type: authoredcontent
ms.openlocfilehash: 0e3db273d43fef6956c07a5340eb9aa1d1425d59
ms.sourcegitcommit: b4cdcf246850751579e45da80c9780fe56330dd0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/09/2021
ms.locfileid: "99985122"
---
# <a name="building-the-chapter-downloads-for-the-ef-5-mvc-4-tutorials"></a>Criando o capítulo downloads para os tutoriais do EF 5 MVC 4

por [Rick Anderson](https://twitter.com/RickAndMSFT)

> O aplicativo Web de exemplo da Contoso University demonstra como criar aplicativos ASP.NET MVC 4 usando o Entity Framework 5 Code First e o Visual Studio 2012. Para obter informações sobre a série de tutoriais, consulte [o primeiro tutorial da série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

## <a name="building-the-chapter-downloads"></a>Criação dos downloads do capítulo

1. Baixe e descompacte o arquivo zip de exemplo do projeto. No pacote de download descompactado, você encontrará arquivos zip adicionais, um para a conclusão de cada capítulo.
2. Clique com o botão direito do mouse no arquivo zip desejado, clique em **Propriedades** e clique no botão **desbloquear** .  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image1.png)
3. Descompacte o arquivo.
4. Clique duas vezes no arquivo *CUx. sln* para iniciar o Visual Studio.
5. No menu **ferramentas** , clique em **Gerenciador de pacotes NuGet** e em **console do Gerenciador de pacotes**.  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image2.png)
6. No console do Gerenciador de pacotes (PMC), clique em **restaurar**.  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image3.png)
7. Saia do Visual Studio.
8. Reinicie o Visual Studio, abrindo o arquivo da solução que você fechou na etapa anterior.
9. No console do Gerenciador de pacotes (PMC), digite o `Update-Database` comando:  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image4.png)  

    > [!NOTE]
    > Se você receber o seguinte erro:  
    >   
    >  *O termo ' Update-Database ' não é reconhecido como o nome de um cmdlet, função, arquivo de script ou programa operável. Verifique a ortografia do nome ou, se um caminho foi incluído, verifique se o caminho está correto e tente novamente.*  
    > Saia e reinicie o Visual Studio.

    Cada migração será executada e o método de semente será executado. Agora você pode executar o aplicativo.

    ![](building-the-ef5-mvc4-chapter-downloads/_static/image5.png)

> [!div class="step-by-step"]
> [Anterior](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
