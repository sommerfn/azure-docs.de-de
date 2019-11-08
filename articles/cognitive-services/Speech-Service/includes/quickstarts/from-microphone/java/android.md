---
title: 'Schnellstart: Erkennen von Spracheingaben per Mikrofon, Java (Android) – Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: Hier erfahren Sie, wie Sie mit dem Speech SDK Sprache in Java unter Android erkennen.
services: cognitive-services
author: fmegen
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 11/05/2019
ms.author: wolfma
ms.openlocfilehash: f0d9a4d2a86b13d5e2d37a900f27d2977f63de14
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73505779"
---
## <a name="prerequisites"></a>Voraussetzungen

Führen Sie unbedingt die folgenden Schritte aus, bevor Sie beginnen:

> [!div class="checklist"]
> * [Erstellen einer Azure Speech-Ressource](../../../../get-started.md)
> * [Einrichten Ihrer Entwicklungsumgebung](../../../../quickstarts/setup-platform.md?tabs=android)

## <a name="create-a-user-interface"></a>Erstellen einer Benutzeroberfläche

Sie erstellen nun eine einfache Benutzeroberfläche für die Anwendung. Bearbeiten Sie das Layout Ihrer Hauptaktivität `activity_main.xml`. Das Layout beinhaltet zunächst eine Titelleiste mit dem Namen Ihrer Anwendung und ein TextView-Element mit dem Text „Hallo Welt!“.

* Wählen Sie das TextView-Element aus. Ändern Sie dessen ID-Attribut oben rechts in `hello`.

* Ziehen Sie aus der Palette oben links im Fenster `activity_main.xml` eine Schaltfläche in den leeren Bereich über dem Text.

* Geben Sie in den Attributen der Schaltfläche auf der rechten Seite für das `onClick`-Attribut den Wert `onSpeechButtonClicked` ein. Wir schreiben eine Methode mit diesem Namen, um das Button-Ereignis zu verarbeiten. Ändern Sie dessen ID-Attribut oben rechts in `button`.

* Verwenden Sie das Zauberstabsymbol oben im Designer, um die Layouteinschränkungen abzuleiten.

  ![Screenshot des Zauberstabsymbols](~/articles/cognitive-services/Speech-Service/media/sdk/qs-java-android-10-infer-layout-constraints.png)

Der Text und die grafische Darstellung Ihrer Benutzeroberfläche sollten jetzt etwa wie folgt aussehen:

![Benutzeroberfläche](~/articles/cognitive-services/Speech-Service/media/sdk/qs-java-android-11-gui.png)

[!code-xml[](~/samples-cognitive-services-speech-sdk/quickstart/java/android/from-microphone/app/src/main/res/layout/activity_main.xml)]

## <a name="add-sample-code"></a>Hinzufügen von Beispielcode

1. Öffnen Sie die Quelldatei `MainActivity.java`. Ersetzen Sie den gesamten Code in dieser Datei durch die folgenden Codezeilen:

   [!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java/android/from-microphone/app/src/main/java/com/microsoft/cognitiveservices/speech/samples/quickstart/MainActivity.java#code)]

   * Die `onCreate`-Methode enthält Code, der Mikrofon- und Internetberechtigungen anfordert und die native Plattformbindung initialisiert. Die Konfiguration der nativen Plattformbindungen ist nur ein Mal erforderlich. Sie sollte früh während der Anwendungsinitialisierung ausgeführt werden.

   * Die Methode `onSpeechButtonClicked` ist (wie bereits erwähnt) der Klickhandler für die Schaltfläche. Ein Klick auf die Schaltfläche löst die Spracherkennungstranskription aus.

1. Ersetzen Sie in der gleichen Datei die Zeichenfolge `YourSubscriptionKey` durch Ihren Abonnementschlüssel.

1. Ersetzen Sie außerdem die Zeichenfolge `YourServiceRegion` durch die mit Ihrem Abonnement verknüpfte [Region](~/articles/cognitive-services/Speech-Service/regions.md). Verwenden Sie beispielsweise `westus` für das kostenlose Testabonnement.

## <a name="build-and-run-the-app"></a>Erstellen und Ausführen der App

1. Verbinden Sie Ihr Android-Gerät mit Ihrem Entwicklungs-PC. Stellen Sie sicher, dass Sie den [Entwicklungsmodus und USB-Debuggen](https://developer.android.com/studio/debug/dev-options) für das Gerät aktiviert haben.

1. Um die Anwendung zu erstellen, drücken Sie STRG+F9, oder wählen Sie **Erstellen** > **Projekt erstellen** auf der Menüleiste aus.

1. Um die Anwendung zu starten, drücken Sie UMSCHALT+F10, oder wählen Sie **Ausführen** >  **'App' ausführen** aus.

1. Wählen Sie im daraufhin angezeigten Fenster mit Bereitstellungszielen Ihr Android-Gerät aus.

   ![Screenshot des Fensters „Bereitstellungsziel auswählen“](~/articles/cognitive-services/Speech-Service/media/sdk/qs-java-android-12-deploy.png)

Wählen Sie die Schaltfläche in der Anwendung aus, um einen Spracherkennungsabschnitt zu beginnen. Die nächsten 15 Sekunden gesprochene englische Sprache werden an die Speech-Dienste gesendet und transkribiert. Das Ergebnis wird in der Android-Anwendung und im logcat-Fenster in Android Studio angezeigt.

![Screenshot der Android-Anwendung](~/articles/cognitive-services/Speech-Service/media/sdk/qs-java-android-13-gui-on-device.png)

## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [footer](./footer.md)]

