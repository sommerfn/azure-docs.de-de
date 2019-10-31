---
title: Schnelleres Beschriften von Bildern mit vorgeschlagenen Tags
titleSuffix: Azure Cognitive Services
description: In diesem Leitfaden erfahren Sie, wie Sie beim Training von Custom Vision-Modellen mit vorgeschlagenen Tags eine große Anzahl von Bildern schneller beschriften können.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 09/16/2019
ms.author: pafarley
ms.openlocfilehash: 06735240729fb2bfd21b87f592e143e9ceabb390
ms.sourcegitcommit: 8074f482fcd1f61442b3b8101f153adb52cf35c9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/22/2019
ms.locfileid: "72753483"
---
# <a name="label-images-faster-with-suggested-tags"></a>Schnelleres Beschriften von Bildern mit vorgeschlagenen Tags

In diesem Leitfaden erfahren Sie, wie Sie beim Training eines Custom Vision-Modells mit dem Feature für vorgeschlagene Tags eine große Anzahl von Bildern schneller beschriften können. 

Wenn Sie Bilder für ein Custom Vision-Modell kennzeichnen, verwendet der Dienst die neueste trainierte Iteration des Modells, um die Beschriftungen von nicht gekennzeichneten Bildern vorherzusagen. Anschließend werden diese Vorhersagen als vorgeschlagene Tags angezeigt, die auf dem ausgewählten Übereinstimmungsschwellenwert und der Vorhersageunsicherheit basieren. Sie können dann die Vorschläge entweder bestätigen oder ändern, um den Prozess der manuellen Kennzeichnung der Bilder für das Training zu beschleunigen.

## <a name="when-to-use-suggested-tags"></a>Verwenden vorgeschlagener Tags

Beachten Sie die folgenden Einschränkungen:

* Fordern Sie vorgeschlagene Tags möglichst nur für Bilder an, deren Inhalt bereits einmal trainiert wurde. Rufen Sie keine Vorschläge für ein neues Tag ab, das Sie gerade erst zu trainieren beginnen.

> [!IMPORTANT]
> Das Feature für vorgeschlagene Tags verwendet dasselbe [Preismodell](https://azure.microsoft.com/pricing/details/cognitive-services/custom-vision-service/) wie normale Vorhersagen. Wenn Sie zum ersten Mal vorgeschlagene Tags für eine Reihe von Bildern auslösen, werden Ihnen dieselben Gebühren wie bei Vorhersageaufrufen berechnet. Danach speichert der Dienst die Ergebnisse für die ausgewählten Bilder 30 Tage lang in einer Datenbank, und Sie können innerhalb dieses Zeitraums jederzeit kostenlos darauf zugreifen. Nach 30 Tagen werden Sie belastet, wenn Sie die vorgeschlagenen Tags erneut anfordern.

## <a name="suggested-tags-workflow"></a>Workflow für vorgeschlagene Tags

Die folgenden Schritte veranschaulichen die Verwendung des Features für vorgeschlagene Tags:

1. Laden Sie alle Ihre Trainingsbilder in Ihr Custom Vision-Projekt hoch.
1. Beschriften Sie einen Teil Ihres Datasets, und wählen Sie dieselbe Anzahl von Bildern für die einzelnen Tags aus.
    > [!TIP]
    > Achten Sie darauf, dass Sie alle Tags verwenden, für die Sie später Vorschläge erwarten.
1. Starten Sie den Trainingsprozess.
1. Wenn das Training abgeschlossen ist, navigieren Sie zur Ansicht **Nicht gekennzeichnet**, und wählen Sie die Schaltfläche **Vorgeschlagene Tags abrufen** im linken Bereich aus.
    > [!div class="mx-imgBorder"]
    > ![Die Schaltfläche für die vorgeschlagenen Tags wird unter der Registerkarte für nicht gekennzeichnete Bilder angezeigt.](./media/suggested-tags/suggested-tags-button.png)
1. Wählen Sie die Bildergruppe aus, für die Sie Vorschläge benötigen. Sie sollten nur für einen Teil der nicht gekennzeichneten Bilder erste Tagvorschläge erhalten. Sie erhalten bessere Tagvorschläge, wenn Sie diesen Prozess durchlaufen.
1. Bestätigen Sie die vorgeschlagenen Tags und korrigieren Sie diese, wenn sie nicht richtig sind.
    > [!TIP]
    > Bilder mit vorgeschlagenen Tags werden nach ihrer Vorhersageunsicherheit sortiert (niedrigere Werte bedeuten eine höhere Zuverlässigkeit). Sie können die Sortierreihenfolge mit der Option zum **Sortieren nach Unsicherheit** ändern. Wenn Sie die Reihenfolge **absteigend** festlegen, können Sie zuerst die Vorhersagen mit hoher Unsicherheit korrigieren und dann schnell die Vorhersagen mit geringer Unsicherheit bestätigen.
    * In Bildklassifizierungsprojekten können Sie Tags stapelweise auswählen und bestätigen. Filtern Sie die Ansicht nach einem bestimmten vorgeschlagenen Tag, deaktivieren Sie die Auswahl von falsch gekennzeichneten Bildern, und bestätigen Sie dann den Rest in einem Stapel.
        > [!div class="mx-imgBorder"]
        > ![Vorgeschlagene Tags werden für IC mit Filtern im Batchmodus angezeigt.](./media/suggested-tags/ic-batch-mode.png)

        Sie können vorgeschlagene Tags auch im Einzelbildmodus verwenden, indem Sie ein Bild aus dem Katalog auswählen.

        ![Vorgeschlagene Tags werden für IC im Einzelbildmodus angezeigt.](./media/suggested-tags/ic-individual-image-mode.png)
    * In Objekterkennungsprojekten werden Batchbestätigungen nicht unterstützt, aber Sie können trotzdem nach vorgeschlagenen Tags filtern und sortieren, um eine besser organisierte Beschriftung zu ermöglichen. Miniaturansichten Ihrer nicht gekennzeichneten Bilder zeigen eine Überlagerung von Begrenzungsrahmen, die die Positionen der vorgeschlagenen Tags anzeigen. Wenn Sie keinen Filter für vorgeschlagene Tags auswählen, werden alle Ihre nicht gekennzeichneten Bilder angezeigt, ohne dass die Begrenzungsrahmen überlagert werden.
        > [!div class="mx-imgBorder"]
        > ![Vorgeschlagene Tags werden für OD mit Filtern im Batchmodus angezeigt.](./media/suggested-tags/od-batch-mode.png)

        Um Tags für die Objekterkennung zu bestätigen, müssen Sie sie auf jedes einzelne Bild im Katalog anwenden.

        ![Vorgeschlagene Tags werden für OD im Einzelbildmodus angezeigt.](./media/suggested-tags/od-individual-image-mode.png)
1. Starten Sie den Trainingsprozess erneut.
1. Wiederholen Sie die vorherigen Schritte, bis Sie mit der Qualität der Vorschläge zufrieden sind.

## <a name="next-steps"></a>Nächste Schritte

Befolgen Sie eine Schnellstartanleitung, um mit der Erstellung und dem Training eines Custom Vision-Projekts zu beginnen.

* [Erstellen einer Klassifizierung](getting-started-build-a-classifier.md)
* [Erstellen einer Objekterkennung](get-started-build-detector.md)