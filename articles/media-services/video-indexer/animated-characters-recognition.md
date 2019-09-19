---
title: Erkennung animierter Figuren mit Video Indexer
titleSuffix: Azure Media Services
description: In diesem Thema erfahren Sie, wie Sie die Erkennung animierter Figuren mit Video Indexer verwenden.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 09/05/2019
ms.author: juliako
ms.openlocfilehash: d45842a4c61f871a479c91f7b192de8e567f9821
ms.sourcegitcommit: 65131f6188a02efe1704d92f0fd473b21c760d08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/10/2019
ms.locfileid: "70861961"
---
# <a name="animated-character-detection-preview"></a>Erkennung animierter Figuren (Vorschauversion)

Azure Media Services Video Indexer unterstützt die Erkennung und Gruppierung von Figuren in animierten Inhalten mittels Integration von [Cognitive Services Custom Vision](https://azure.microsoft.com/services/cognitive-services/custom-vision-service/). Diese Funktion steht sowohl über das Portal als auch über die API zur Verfügung.

Nach dem Hochladen eines animierten Videos mit einem bestimmten Animationsmodell extrahiert Video Indexer Keyframes, erkennt animierte Figuren in diesen Frames, gruppiert ähnliche Figuren und wählt das beste Beispiel aus. Anschließend werden die gruppierten Figuren an Custom Vision gesendet, um sie auf der Grundlage der trainierten Modelle zu identifizieren. 

Wenn Ihr Modell noch nicht trainiert wurde, werden die Figuren namenlos erkannt. Wenn Sie Namen hinzufügen und das Modell trainieren, erkennt Video Indexer die Figuren und benennt sie entsprechend.

## <a name="flow-diagram"></a>Flussdiagramm

Das folgende Diagramm zeigt den Ablauf des Erkennungsprozesses für animierte Figuren:

![Flussdiagramm](./media/animated-characters-recognition/flow.png)

## <a name="accounts"></a>Konten

Abhängig von der Art Ihres Video Indexer-Kontos stehen unterschiedliche Features zur Verfügung. Unter [Erstellen eines Video Indexer-Kontos mit Azure-Verbindung](connect-to-azure.md) erfahren Sie, wie Sie Ihr Konto mit Azure verbinden.

* Testkonto: Video Indexer verwendet ein internes Custom Vision-Konto, um ein Modell zu erstellen und mit Ihrem Video Indexer-Konto zu verbinden. 
* Kostenpflichtiges Konto: Sie verbinden Ihr Custom Vision-Konto mit Ihrem Video Indexer-Konto. (Sollten Sie noch nicht über ein Konto verfügen, müssen Sie erst ein Konto erstellen.)

### <a name="trial-vs-paid"></a>Gegenüberstellung von Testkonto und kostenpflichtigem Konto

|Funktion|Testkonto|Kostenpflichtiges Konto|
|---|---|---|
|Custom Vision-Konto|Wird im Hintergrund durch Video Indexer verwaltet. |Ihr Custom Vision-Konto wird mit Video Indexer verbunden.|
|Anzahl von Animationsmodellen|Eins|Bis zu 100 Modelle pro Konto (Custom Vision-Einschränkung)|
|Training des Modells|Video Indexer trainiert das Modell für neue Figuren anhand von zusätzlichen Beispielen vorhandener Figuren.|Der Kontobesitzer trainiert das Modell, wenn er bereit ist, Änderungen vorzunehmen.|
|Erweiterte Optionen in Custom Vision|Kein Zugriff auf das Custom Vision-Portal|Sie können die Modelle selbst im Custom Vision-Portal anpassen.|

## <a name="use-the-animated-character-detection-with-portal"></a>Verwenden der Erkennung animierter Figuren über das Portal 

Dieser Abschnitt enthält eine schrittweise Einführung in die Verwendung des Modells für die Erkennung animierter Figuren.

### <a name="connect-your-custom-vision-account-paid-accounts-only"></a>Herstellen einer Verbindung mit Ihrem Custom Vision-Konto (nur kostenpflichtige Konten)

Wenn Sie über ein kostenpflichtiges Video Indexer-Konto verfügen, müssen Sie zunächst eine Verbindung mit einem Custom Vision-Konto herstellen. Sollten Sie noch nicht über ein Custom Vision-Konto verfügen, müssen Sie eins erstellen. Weitere Informationen finden Sie unter [Was ist Azure Custom Vision?](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/home).

### <a name="create-an-animated-characters-model"></a>Erstellen eines Modells für animierte Figuren

1. Navigieren Sie zur [Video Indexer](https://vi.microsoft.com/)-Website, und melden Sie sich an.
1. Klicken Sie in der oberen rechten Ecke der Seite auf die Schaltfläche „Anpassung des Inhaltsmodells“.

    ![Anpassung des Inhaltsmodells](./media/animated-characters-recognition/content-model-customization.png)
1. Navigieren Sie im Abschnitt für die Modellanpassung zur Registerkarte **Animated characters** (Animierte Figuren).
1. Klicken Sie auf **Modell hinzufügen**.
1. Benennen Sie das Modell, und drücken Sie die EINGABETASTE, um den Namen zu speichern.

> [!NOTE]
> Es wird empfohlen, für jede animierte Serie jeweils ein eigenes Custom Vision-Modell zu verwenden. 

### <a name="index-a-video-with-an-animated-model"></a>Indizieren eines Videos mit einem animierten Modell

1. Klicken Sie im oberen Menü auf die Schaltfläche **Hochladen**.
1. Wählen Sie ein hochzuladendes Video (Datei oder URL) aus.
1. Klicken Sie auf **Erweiterte Optionen**.
1. Wählen Sie unter **People / Animated characters** (Personen/animierte Figuren) die Option **Animation models** (Animationsmodelle) aus.
1. Wenn Sie über ein einzelnes Modell verfügen, wird es automatisch ausgewählt. Sind mehrere Modelle vorhanden, können Sie das passende Modell über das Dropdownmenü auswählen.
1. Klicken Sie auf „Hochladen“.
1. Nachdem das Video indiziert wurde, werden die erkannten Figuren im Abschnitt **Animated characters** (Animierte Figuren) im Bereich **Erkenntnisse** angezeigt.

> [!NOTE] 
> Vor dem Taggen und Trainieren des Modells werden alle Figuren mit „Unknown #X“ (Unbekannt #X) benannt. Nachdem Sie das Modell trainiert haben, werden sie auch erkannt.

### <a name="customize-the-animated-characters-models"></a>Anpassen der Modelle für animierte Figuren

1. Taggen und trainieren Sie das Modell.

    1. Taggen Sie die erkannte Figur, indem Sie ihren Namen bearbeiten. Sobald das Modell mit einer Figur trainiert wurde, wird sie im nächsten Video erkannt, das mit diesem Modell indiziert wird. 
    1. Um eine animierte Figur in Ihrem Video zu taggen, navigieren Sie zur Registerkarte **Erkenntnisse**, und klicken Sie rechts oben im Fenster auf die Schaltfläche **Bearbeiten**.
    1. Klicken Sie im Bereich **Erkenntnisse** auf eine der erkannten animierten Figuren, und ändern Sie ihren Namen von „Unknown #X“ (Unbekannt #X) bzw. dem Namen, der der Figur zuvor zugewiesen wurde, in einen anderen Namen.
    1. Nachdem Sie den neuen Namen eingegeben haben, klicken Sie neben dem neuen Namen auf das Prüfsymbol. Dadurch wird der neue Name in dem Modell in Video Indexer gespeichert.
    1. Nachdem Sie alle gewünschten Namen bearbeitet haben, müssen Sie das Modell trainieren.

        * Testkonten: Öffnen Sie die Anpassungsseite, klicken Sie auf die Registerkarte für animierte Figuren, und klicken Sie anschließend für Ihr Modell auf die Schaltfläche **Trainieren**.
        * Kostenpflichtige Konten: Öffnen Sie die Anpassungsseite, und klicken Sie auf die Registerkarte für animierte Figuren. Klicken Sie für das relevante Modell auf den Link **Edit in Custom Vision** (In Custom Vision bearbeiten). Daraufhin werden Sie zur Seite des Modells in Custom Vision weitergeleitet. Klicken Sie dort auf die Schaltfläche „Trainieren“, um Ihr Modell zu trainieren. 
    1. Nach dem Trainieren werden die trainierten Figuren in allen Videos erkannt, die erstmals oder erneut mit diesem Modell indiziert werden. 
    Bei kostenpflichtigen Konten mit Zugriff auf das Custom Vision-Konto können die Modelle und getaggten Bilder dort angezeigt werden. Weitere Informationen zur Verbesserung Ihrer Klassifizierung in Custom Vision finden Sie [hier](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/getting-started-improving-your-classifier).

1. Löschen Sie eine animierte Figur.

    1. Um eine animierte Figur aus Ihren Videoerkenntnissen zu löschen, navigieren Sie zur Registerkarte **Erkenntnisse**, und klicken Sie rechts oben im Fenster auf die Schaltfläche **Bearbeiten**.
    1. Wählen Sie die animierte Figur aus, und klicken Sie anschließend unter ihrem Namen auf die Schaltfläche **Löschen**.

    > [!NOTE]
    > Dadurch wird die Erkenntnis aus diesem Video gelöscht. Das Modell bleibt davon unberührt.

1. Löschen Sie ein Modell.

    1. Klicken Sie im oberen Menü auf die Schaltfläche **Anpassung des Inhaltsmodells**, und navigieren Sie zur Registerkarte **Animated characters** (Animierte Figuren).
    1. Klicken Sie rechts neben dem Modell, das Sie löschen möchten, auf die Auslassungspunkte und anschließend auf die Schaltfläche „Löschen“.
    
    * Kostenpflichtiges Konto: Die Verbindung zwischen Modell und Video Indexer wird getrennt und kann nicht wiederhergestellt werden.
    * Testkonto: Das Modell wird auch aus Custom Vision gelöscht. 
    
        > [!NOTE]
        > Bei einem Testkonto steht nur ein einzelnes Modell zur Verfügung. Nachdem Sie es gelöscht haben, können Sie keine weiteren Modelle trainieren.

## <a name="use-the-animated-character-detection-with-api"></a>Verwenden der Erkennung animierter Figuren über die API 

1. Stellen Sie eine Verbindung mit einem Custom Vision-Konto her.

    Wenn Sie über ein kostenpflichtiges Video Indexer-Konto verfügen, müssen Sie zunächst eine Verbindung mit einem Custom Vision-Konto herstellen. <br/>
    Sollten Sie noch nicht über ein Custom Vision-Konto verfügen, müssen Sie eins erstellen. Weitere Informationen finden Sie unter [Was ist Azure Custom Vision?](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/home).

    [Stellen Sie unter Verwendung der API eine Verbindung mit Ihrem Custom Vision-Konto her.](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Connect-Custom-Vision-Account?tags=&pattern=&groupBy=tag)
1. Erstellen Sie ein Modell für animierte Figuren.

    Verwenden Sie die [API zum Erstellen von Animationsmodellen](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Create-Animation-Model?&groupBy=tag).
1. Indizieren Sie ein Video (erstmals oder erneut).

    Verwenden Sie die [API für die erneute Indizierung](https://api-portal.videoindexer.ai/docs/services/operations/operations/Re-Index-Video?). 
1. Passen Sie die Modelle für animierte Figuren an.

    Verwenden Sie die [API zum Trainieren von Animationsmodellen](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Train-Animation-Model?&groupBy=tag).

### <a name="view-the-output"></a>Anzeigen der Ausgabe

Sehen Sie sich die animierten Figuren in der generierten JSON-Datei an.

```json
"animatedCharacters": [
    {
    "videoId": "e867214582",
    "confidence": 0,
    "thumbnailId": "00000000-0000-0000-0000-000000000000",
    "seenDuration": 201.5,
    "seenDurationRatio": 0.3175,
    "isKnownCharacter": true,
    "id": 4,
    "name": "Bunny",
    "appearances": [
        {
            "startTime": "0:00:52.333",
            "endTime": "0:02:02.6",
            "startSeconds": 52.3,
            "endSeconds": 122.6
        },
        {
            "startTime": "0:02:40.633",
            "endTime": "0:03:16.6",
            "startSeconds": 160.6,
            "endSeconds": 196.6
        },
    ]
    },
]
```

## <a name="limitations"></a>Einschränkungen

* Die Animationserkennung wird momentan in der Region „Asien, Osten“ nicht unterstützt.
* Kleine oder weit entfernte Figuren werden in Videos mit schlechter Qualität möglicherweise nicht ordnungsgemäß erkannt.
* Es wird empfohlen, für jede Gruppe von animierten Figuren (beispielsweise pro animierter Serie) ein eigenes Modell zu verwenden.

## <a name="next-steps"></a>Nächste Schritte

[What is Video Indexer? (preview)](video-indexer-overview.md) (Was ist Video Indexer? (Vorschauversion))
