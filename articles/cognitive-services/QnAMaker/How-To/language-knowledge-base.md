---
title: 'Nicht englischsprachige Wissensdatenbank: QnA Maker'
titleSuffix: Azure Cognitive Services
description: QnA Maker unterstützt Wissensdatenbank-Inhalte in mehreren Sprachen. Allerdings sollte für jeden QnA Maker-Dienst eine einzelne Sprache festgelegt werden. Die erste Wissensdatenbank, die für einen bestimmten QnA Maker-Dienst erstellt wurde, bestimmt die Sprache dieses Dienstes.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 08/20/2019
ms.author: diberry
ms.openlocfilehash: 63eb13dd131fcc1c424c02fdac10f531cc9f0282
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/21/2019
ms.locfileid: "69876623"
---
# <a name="language-support-of-knowledge-base-content-for-qna-maker"></a>Sprachunterstützung für Inhalte in QnA Maker-Wissensdatenbanken

QnA Maker unterstützt Wissensdatenbank-Inhalte in mehreren Sprachen. Allerdings sollte für jeden QnA Maker-Dienst eine einzelne Sprache festgelegt werden. Die erste Wissensdatenbank, die für einen bestimmten QnA Maker-Dienst erstellt wurde, bestimmt die Sprache dieses Dienstes. Die unterstützten Sprachen finden Sie [hier](../Overview/languages-supported.md).

Die Sprache wird automatisch aus dem jeweils extrahierten Datenquelleninhalt erkannt. Wenn Sie einen neuen QnA Maker-Dienst und eine neue Wissensdatenbank innerhalb dieses Dienstes erstellt haben, können Sie überprüfen, ob die Sprache richtig festgelegt wurde.

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com/).

1. Wählen Sie **Ressourcengruppen** aus und navigieren Sie zu der Ressourcengruppe, in der der QnA Maker-Dienst bereitgestellt wird. Wählen Sie die Ressource **Azure Search** aus.

    ![Auswählen der Ressource „Azure Search“](../media/qnamaker-how-to-language-kb/select-azsearch.png)

1. Wählen Sie den Index **testkb** aus. Dieser Azure Search-Index wird immer als Erstes erstellt und enthält den gespeicherten Inhalt aller Wissensdatenbanken im jeweiligen Dienst. 

    ![Auswählen der Test-Wissensdatenbank](../media/qnamaker-how-to-language-kb/select-testkb.png)

1. Wählen Sie den Abschnitt **Felder** aus, der die Details von _testkb_ beinhaltet.

    ![Auswählen von „Felder“](../media/qnamaker-how-to-language-kb/selectfields.png)

1. Aktivieren Sie das Kontrollkästchen für **Analysetool**, um Sprachdetails anzuzeigen.

    ![Auswählen des Analysetools](../media/qnamaker-how-to-language-kb/select-analyzer.png)

1. Für das _Analysetool_ sollte eine bestimmte Sprache festgelegt sein. Diese Sprache wird beim Erstellen der Wissensdatenbank in den importierten Dateien und URLs automatisch erkannt. Sie kann nicht mehr geändert werden, nachdem die Ressource erstellt wurde.

    ![Ausgewähltes Analysetool](../media/qnamaker-how-to-language-kb/selected-analyzer.png)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erstellen eines QnA-Bots mit Azure Bot Service](../Tutorials/create-qna-bot.md)
