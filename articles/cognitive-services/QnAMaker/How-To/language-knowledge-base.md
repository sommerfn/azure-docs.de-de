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
ms.date: 09/12/2019
ms.author: diberry
ms.openlocfilehash: 5e50c814fef24aa799549d055ad6496f5bdf05e0
ms.sourcegitcommit: dd69b3cda2d722b7aecce5b9bd3eb9b7fbf9dc0a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2019
ms.locfileid: "70961472"
---
# <a name="language-support-of-knowledge-base-content-for-qna-maker"></a>Sprachunterstützung für Inhalte in QnA Maker-Wissensdatenbanken

QnA Maker unterstützt Wissensdatenbank-Inhalte in mehreren Sprachen. Allerdings sollte für jeden QnA Maker-Dienst eine einzelne Sprache festgelegt werden. Die erste Wissensdatenbank, die für einen bestimmten QnA Maker-Dienst erstellt wurde, bestimmt die Sprache dieses Dienstes. Die unterstützten Sprachen finden Sie [hier](../Overview/languages-supported.md).

Die Sprache wird automatisch aus dem jeweils extrahierten Datenquelleninhalt erkannt. Wenn Sie einen neuen QnA Maker-Dienst und eine neue Wissensdatenbank innerhalb dieses Dienstes erstellt haben, können Sie überprüfen, ob die Sprache richtig festgelegt wurde.

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com/).

1. Wählen Sie **Ressourcengruppen** aus und navigieren Sie zu der Ressourcengruppe, in der der QnA Maker-Dienst bereitgestellt wird. Wählen Sie die Ressource **Azure Search** aus.

    ![Auswählen der Ressource „Azure Search“](../media/qnamaker-how-to-language-kb/select-azsearch.png)

1. Wählen Sie **Indizes** und dann den Index **testkb** aus. Dieser Azure Search-Index wird als Erstes erstellt und enthält den gespeicherten Inhalt aller Wissensdatenbanken im jeweiligen Dienst. 

1. Wählen Sie **Felder** aus, um die Felder im Index anzuzeigen.

1. Die Spalte _Analyse_ der Felder `questions` und `answer` ist auf eine bestimmte Sprache festgelegt. Diese Sprache wird beim Erstellen der Wissensdatenbank in den importierten Dateien und URLs automatisch erkannt. Sie kann nicht mehr geändert werden, nachdem die Ressource erstellt wurde.

    ![Ausgewähltes Analysetool](../media/qnamaker-how-to-language-kb/selected-analyzer.png)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erstellen eines QnA-Bots mit Azure Bot Service](../Tutorials/create-qna-bot.md)
