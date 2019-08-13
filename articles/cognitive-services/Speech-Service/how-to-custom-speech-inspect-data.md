---
title: Überprüfen der Datenqualität für Custom Speech – Speech Service
titleSuffix: Azure Cognitive Services
description: Custom Speech bietet Tools zur visuellen Überprüfung der Erkennungsqualität eines Modells durch Vergleichen von Audiodaten mit dem entsprechenden Erkennungsergebnis. Über das Custom Speech-Portal können Sie hochgeladene Audiodaten wiedergeben und bestimmen, ob das angegebene Erkennungsergebnis korrekt ist.  Mit diesem Tool können Sie schnell die Qualität unseres Baseline-Spracherkennungsmodells oder eines trainierten benutzerdefinierten Modells überprüfen, ohne Audiodaten transkribieren zu müssen.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: b58f9c17995128091b5c4badd228356dbacc6ae9
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2019
ms.locfileid: "68562850"
---
# <a name="inspect-custom-speech-data"></a>Überprüfen von Custom Speech-Daten

> [!NOTE]
> Auf dieser Seite wird vorausgesetzt, dass Sie [Prepare data for Custom Speech](how-to-custom-speech-test-data.md) (Vorbereiten von Testdaten für Custom Speech) gelesen und ein Dataset für die Überprüfung hochgeladen haben.

Custom Speech bietet Tools zur visuellen Überprüfung der Erkennungsqualität eines Modells durch Vergleichen von Audiodaten mit dem entsprechenden Erkennungsergebnis. Über das Custom Speech-Portal können Sie hochgeladene Audiodaten wiedergeben und bestimmen, ob das angegebene Erkennungsergebnis korrekt ist. Mit diesem Tool können Sie schnell die Qualität des Baseline-Spracherkennungsmodells von Microsoft oder eines trainierten benutzerdefinierten Modells überprüfen, ohne Audiodaten transkribieren zu müssen.

In diesem Dokument erfahren Sie, wie Sie die Qualität eines Modells unter Verwendung der zuvor hochgeladenen Trainingsdaten visuell überprüfen.

Auf dieser Seite erfahren Sie, wie Sie die Qualität des Baseline-Spracherkennungsmodells von Microsoft und/oder eines trainierten benutzerdefinierten Modells visuell überprüfen. Dafür verwenden Sie die Daten, die Sie zu Testzwecken über die Registerkarte **Daten** hochgeladen haben.

## <a name="create-a-test"></a>Erstellen eines Tests

Gehen Sie wie folgt vor, um einen Test zu erstellen:

1. Navigieren Sie zu **Spracherkennung > Custom Speech > Testen**.
2. Klicken Sie auf **Test hinzufügen**.
3. Wählen Sie **Inspect quality (Audio-only data)** (Qualität überprüfen (nur Audiodaten)) aus. Geben Sie einen Namen und eine Beschreibung für den Test ein, und wählen Sie Ihr Audiodataset aus.
4. Wählen Sie bis zu zwei Modelle aus, die Sie testen möchten.
5. Klicken Sie auf **Create**.

Nach erfolgreicher Testerstellung können Sie die Modelle nebeneinander vergleichen.

## <a name="side-by-side-model-comparisons"></a>Gegenüberstellung der Modelle

Wenn der Teststatus *Erfolgreich* lautet, klicken Sie auf den Namen des Testelements, um Testdetails anzuzeigen. Diese Detailseite listet alle Äußerungen in Ihrem Dataset auf und zeigt die Erkennungsergebnisse der beiden Modelle neben der Transkription aus dem übermittelten Dataset an.

Zur einfacheren Untersuchung der Gegenüberstellung können Sie verschiedene Fehlertypen wie Einfügungen, Löschungen und Ersetzungen aktivieren oder deaktivieren. Indem Sie sich die Audiodaten anhören und mit den Erkennungsergebnissen in den einzelnen Spalten vergleichen (die die menschenmarkierte Transkription sowie die Ergebnisse zweier Spracherkennungsmodelle enthalten), können Sie entscheiden, welches Modell Ihre Anforderungen erfüllt und wo Verbesserungen erforderlich sind.

Die Überprüfung der Qualität ist sinnvoll, um sich zu vergewissern, dass die Qualität eines Spracherkennungsendpunkts für eine Anwendung ausreicht.  Für eine objektive Genauigkeitsmessung, für die transkribiertes Audio benötigt wird, befolgen Sie die Anweisungen unter [Bewerten der Genauigkeit](how-to-custom-speech-evaluate-data.md).

## <a name="next-steps"></a>Nächste Schritte

* [Bewerten Ihrer Daten](how-to-custom-speech-evaluate-data.md)
* [Trainieren Ihres Modells](how-to-custom-speech-train-model.md)
* [Bereitstellen Ihres Modells](how-to-custom-speech-deploy-model.md)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Prepare data for Custom Speech](how-to-custom-speech-test-data.md) (Vorbereiten von Daten für Custom Speech)
