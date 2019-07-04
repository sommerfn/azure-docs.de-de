---
title: Dokumentdetails – Custom Translator
titleSuffix: Azure Cognitive Services
description: Die Seite mit der Dokumentenliste zeigt die ersten zehn Dokumente in Ihrem Arbeitsbereich an. Für jedes Dokument werden Name, Paare, Typ, Sprache, Uploadzeitstempel und E-Mail-Adresse des Benutzers angezeigt, der das Dokument hochgeladen hat.
author: swmachan
manager: christw
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 02/21/2019
ms.author: swmachan
ms.topic: conceptual
ms.openlocfilehash: 267d60c2e9941c66ab128f68bd7aaf1cc54fedd9
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2019
ms.locfileid: "67442506"
---
# <a name="view-document-details"></a>Anzeigen der Dokumentdetails

Die Seite mit der Dokumentenliste zeigt die ersten zehn Dokumente in Ihrem Arbeitsbereich an. Für jedes Dokument werden Name, Paare, Typ, Sprache, Uploadzeitstempel und E-Mail-Adresse des Benutzers angezeigt, der das Dokument hochgeladen hat.

Klicken Sie auf ein Dokument, um die Seite mit den entsprechenden Dokumentdetails anzuzeigen. Auf der Seite mit den Dokumentdetails wird die Liste der aus dem Dokument extrahierten Sätze angezeigt.

- In der Standardeinstellung ist im Dropdownfeld die Ausgangssprache ausgewählt. Sie können jedoch auch festlegen, dass die Sätze in der Zielsprache angezeigt werden.
- Standardmäßig werden pro Seite 20 Sätze angezeigt. Sie können mit dem Steuerelement zum Paginieren zwischen den Seiten wechseln.

![Dokumentdetails](media/how-to/how-to-view-document-details.png)

## <a name="delete-a-document"></a>Löschen eines Dokuments

Der Benutzer muss der Besitzer des Arbeitsbereichs sein, um ein Dokument zu löschen. Wenn ein Dokument von einem Modell verwendet wird, das sich in einer beliebigen Phase des Trainings- oder Bereitstellungsprozesses befindet, kann das Dokument nicht gelöscht werden.

1. Navigieren Sie zur Seite „Dokumente“.
2.  Zeigen Sie mit der Maus auf einen beliebigen Dokumentdatensatz, und klicken Sie auf das Papierkorbsymbol.

    ![Löschen eines Dokuments](media/how-to/how-to-delete-document-1.png)

3.  Bestätigen Sie den Löschvorgang.

    ![Bestätigen des Löschvorgangs](media/how-to/how-to-delete-document-confirm.png)

## <a name="next-steps"></a>Nächste Schritte

- Lernen Sie, wie Sie ein [Modell trainieren](how-to-train-model.md).
