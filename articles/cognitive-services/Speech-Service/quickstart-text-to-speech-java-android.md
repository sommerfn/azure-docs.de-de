---
title: 'Schnellstart: Sprachsynthese, Java (Android) – Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: Hier erfahren Sie, wie Sie mit dem Speech SDK Sprache in Java unter Android synthetisieren.
services: cognitive-services
author: yulin-li
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 09/19/2019
ms.author: yulili
ms.openlocfilehash: 17fb0c1a4ba5495506ceb658eb61d0ac2959caf8
ms.sourcegitcommit: 4f3f502447ca8ea9b932b8b7402ce557f21ebe5a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2019
ms.locfileid: "71803761"
---
# <a name="quickstart-synthesize-speech-in-java-on-android-by-using-the-speech-sdk"></a>Schnellstart: Sprachsynthese in Java unter Android mit dem Speech SDK

Schnellstarts sind auch für [Spracherkennung](quickstart-java-android.md) und den [virtuellen Voice-First-Assistenten](quickstart-virtual-assistant-java-android.md) verfügbar.

Dieser Artikel enthält Informationen zur Entwicklung einer Java-Anwendung für Android mithilfe des Cognitive Services Speech SDK zum Synthetisieren von Sprache aus Text.
Die Anwendung basiert auf dem Maven-Paket für das Speech SDK (Version 1.7.0) sowie auf Android Studio 3.3.
Das Speech SDK ist zurzeit kompatibel mit Android-Geräten mit 32-/64-Bit-ARM-Prozessoren und Prozessoren, die mit Intel x86/x64 kompatibel sind.

> [!NOTE]
> Weitere Informationen zum SDK für sprachaktivierte Geräte und zum Roobo-Gerät finden Sie unter [SDK für sprachaktivierte Geräte](speech-devices-sdk.md).

## <a name="prerequisites"></a>Voraussetzungen

Für diese Schnellstartanleitung benötigen Sie einen Abonnementschlüssel für die Speech-Dienste. Sie können einen solchen Schlüssel kostenlos abrufen. Ausführliche Informationen finden Sie unter [Kostenloses Testen von Speech Services](get-started.md).

## <a name="create-and-configure-a-project"></a>Erstellen und Konfigurieren eines Projekts

[!INCLUDE [](../../../includes/cognitive-services-speech-service-quickstart-java-android-create-proj.md)]

## <a name="create-user-interface"></a>Erstellen der Benutzeroberfläche

Sie erstellen eine einfache Benutzeroberfläche für die Anwendung. Bearbeiten Sie das Layout Ihrer Hauptaktivität `activity_main.xml`. Das Layout beinhaltet zunächst eine Titelleiste mit dem Namen Ihrer Anwendung und ein TextView-Element mit dem Text „Hello World!“.

1. Klicken Sie auf das TextView-Element. Ändern Sie dessen ID-Attribut oben rechts in `outputMessage`, und ziehen Sie es in den unteren Bildschirmbereich. Löschen Sie den zugehörigen Text.

1. Ziehen Sie aus der Palette oben links im Fenster `activity_main.xml` eine Schaltfläche in den leeren Bereich über dem Text.

1. Geben Sie in den Attributen der Schaltfläche auf der rechten Seite für das `onClick`-Attribut den Wert `onSpeechButtonClicked` ein. Wir schreiben eine Methode mit diesem Namen, um das Button-Ereignis zu verarbeiten.  Ändern Sie dessen ID-Attribut oben rechts in `button`.

1. Ziehen Sie Nur-Text in den Bereich oberhalb der Schaltfläche, und ändern Sie ihr ID-Attribut in `speakText` und das Textattribut in `Hi there!`.

1. Verwenden Sie das Zauberstabsymbol oben im Designer, um die Layouteinschränkungen abzuleiten.


    ![Screenshot des Zauberstabsymbols](media/sdk/qs-java-android-10-infer-layout-constraints.png)

Der Text und die grafische Darstellung Ihrer Benutzeroberfläche sollten jetzt etwa wie folgt aussehen:

![](media/sdk/qs-java-android-11-2-tts-gui.png)

[!code-xml[](~/samples-cognitive-services-speech-sdk/quickstart/text-to-speech/java-android/app/src/main/res/layout/activity_main.xml)]

## <a name="add-sample-code"></a>Hinzufügen von Beispielcode

1. Öffnen Sie die Quelldatei `MainActivity.java`. Ersetzen Sie den Code in dieser Datei durch die folgenden Codezeilen:

   [!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/text-to-speech/java-android/app/src/main/java/com/microsoft/cognitiveservices/speech/samples/quickstart/MainActivity.java#code)]

   * Die Methode `onSpeechButtonClicked` ist (wie bereits erwähnt) der Klickhandler für die Schaltfläche. Ein Klick auf die Schaltfläche löst die Sprachsynthese aus.

1. Ersetzen Sie in der gleichen Datei die Zeichenfolge `YourSubscriptionKey` durch Ihren Abonnementschlüssel.

1. Ersetzen Sie außerdem die Zeichenfolge `YourServiceRegion` durch die [Region](regions.md), die mit Ihrem Abonnement verknüpft ist (z.B. `westus` für das kostenlose Testabonnement).

## <a name="build-and-run-the-app"></a>Erstellen und Ausführen der App

1. Verbinden Sie Ihr Android-Gerät mit Ihrem Entwicklungs-PC. Stellen Sie sicher, dass Sie den [Entwicklungsmodus und USB-Debuggen](https://developer.android.com/studio/debug/dev-options) für das Gerät aktiviert haben. Erstellen Sie alternativ einen [Android-Emulator](https://developer.android.com/studio/run/emulator).

1. Um die Anwendung zu erstellen, drücken Sie STRG+F9, oder wählen Sie **Erstellen** > **Projekt erstellen** in der Menüleiste aus.

1. Um die Anwendung zu starten, drücken Sie UMSCHALT+F10, oder wählen Sie **Ausführen** >  **'App' ausführen** aus.

1. Wählen Sie im daraufhin angezeigten Fenster mit Bereitstellungszielen Ihr Android-Gerät oder Ihren Emulator aus.

   ![Screenshot des Fensters „Bereitstellungsziel auswählen“](media/sdk/qs-java-android-12-deploy.png)

Geben Sie Text ein, und klicken Sie auf die Schaltfläche in der Anwendung, um einen Sprachsyntheseabschnitt zu beginnen. Die synthetisierten Audiodaten werden über den Standardlautsprecher ausgegeben, und auf dem Bildschirm wird die Info `speech synthesis succeeded` angezeigt.

![Screenshot der Android-Anwendung](media/sdk/qs-java-android-13-2-gui-on-device-tts.png)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Java-Beispiele auf GitHub](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Weitere Informationen

- [Anpassen von Voicefonts](how-to-customize-voice-font.md)
- [Aufnehmen von Sprachbeispielen](record-custom-voice-samples.md)
