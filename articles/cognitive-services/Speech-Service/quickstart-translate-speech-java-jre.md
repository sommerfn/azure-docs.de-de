---
title: 'Schnellstart: Übersetzen von Sprache, Java (Windows, Linux) – Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: In dieser Schnellstartanleitung erstellen Sie eine einfache Java-Anwendung zum Erfassen der Benutzersprache, Übersetzen in eine andere Sprache und Ausgeben des Texts in der Befehlszeile. Dieser Leitfaden ist für Windows- und Linux-Benutzer bestimmt.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: cbb86dd4b24cb325b8ea6708ebc2ffc89a697757
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2019
ms.locfileid: "68553396"
---
# <a name="quickstart-translate-speech-with-the-speech-sdk-for-java"></a>Schnellstart: Übersetzen von Sprache mit dem Speech-SDK für Java

Schnellstarts sind auch für [Spracherkennung](quickstart-java-jre.md) und den [virtuellen Voice-First-Assistenten](quickstart-virtual-assistant-java-jre.md) verfügbar.

In dieser Schnellstartanleitung erstellen Sie eine einfache Java-Anwendung, mit der die Spracheingabe des Benutzers über das Mikrofon Ihres Computers erfasst, die Sprache übersetzt und der übersetzte Text in Echtzeit in der Befehlszeile transkribiert wird. Diese Anwendung ist für die Ausführung unter 64-Bit-Versionen von Windows oder 64-Bit-Versionen von Linux (Ubuntu 16.04, Ubuntu 18.04, Debian 9) bzw. unter macOS 10.13 oder höher ausgelegt. Sie wird mit dem Maven-Paket für das Speech SDK und der Eclipse Java-IDE erstellt.

Eine vollständige Liste mit den verfügbaren Sprachen für die Sprachübersetzung finden Sie auf der Seite zur [Sprachunterstützung](language-support.md).

## <a name="prerequisites"></a>Voraussetzungen

Für diese Schnellstartanleitung ist Folgendes erforderlich:

* Betriebssystem: 64-Bit-Version von Windows, 64-Bit-Version von Linux (Ubuntu 16.04, Ubuntu 18.04, Debian 9) oder macOS 10.13 oder höher
* [Eclipse Java IDE](https://www.eclipse.org/downloads/)
* [Java 8](https://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html) oder [JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/index.html)
* Ein Azure-Abonnementschlüssel für den Speech-Dienst. [Hier erhalten Sie einen kostenlosen Schlüssel.](get-started.md)

Stellen Sie beim Ausführen von Linux sicher, dass diese Abhängigkeiten installiert sind, bevor Sie Eclipse starten.

 * Unter Ubuntu:

   ```sh
   sudo apt-get update
   sudo apt-get install libssl1.0.0 libasound2
   ```

 * Unter Debian 9:

   ```sh
   sudo apt-get update
   sudo apt-get install libssl1.0.2 libasound2
   ```

> [!NOTE]
> Weitere Informationen zum SDK für sprachaktivierte Geräte und zum Roobo-Gerät finden Sie unter [SDK für sprachaktivierte Geräte](speech-devices-sdk.md).

## <a name="create-and-configure-project"></a>Erstellen und Konfigurieren des Projekts

[!INCLUDE [](../../../includes/cognitive-services-speech-service-quickstart-java-create-proj.md)]

## <a name="add-sample-code"></a>Hinzufügen von Beispielcode

1. Um Ihrem Java-Projekt eine neue leere Klasse hinzuzufügen, wählen Sie **Datei** > **Neu** > **Klasse** aus.

1. Geben Sie im Fenster **Neue Java-Klasse** **speechsdk.quickstart** in das Feld **Paket** und **Main** in das Feld **Name** ein.

   ![Screenshot des Fensters „Neue Java-Klasse“](media/sdk/qs-java-jre-06-create-main-java.png)

1. Ersetzen Sie den Code in `Main.java` mit dem folgenden Ausschnitt:

   [!code-java[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/speech-translation/java-jre/src/speechsdk/quickstart/Main.java#code)]

1. Ersetzen Sie die Zeichenfolge `YourSubscriptionKey` durch Ihren Abonnementschlüssel.

1. Ersetzen Sie die Zeichenfolge `YourServiceRegion` durch die [Region](regions.md), die mit Ihrem Abonnement verknüpft ist (z. B. `westus` für das kostenlose Testabonnement).

1. Speichern Sie die Änderungen am Projekt.

## <a name="build-and-run-the-app"></a>Erstellen und Ausführen der App

Drücken Sie F11, oder wählen Sie **Ausführen** > **Debuggen** aus.

Die Spracheingabe über Ihr Mikrofon wird ins Deutsche transkribiert und im Konsolenfenster protokolliert. Drücken Sie die EINGABETASTE, um die Spracherfassung zu beenden.

![Screenshot der Konsolenausgabe nach erfolgreicher Erkennung](media/sdk/qs-translate-java-jre-output.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Beispiele, z.B. zum Auslesen von Sprache aus einer Audiodatei und Ausgeben von übersetztem Text als synthetisierte Sprache, sind auf GitHub verfügbar.

> [!div class="nextstepaction"]
> [Java-Beispiele auf GitHub](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Weitere Informationen

- [Schnellstart: Erkennen von Sprache, Java (Windows, Linux)](quickstart-java-jre.md)
- [Tutorial: Erstellen eines benutzerdefinierten Akustikmodells](how-to-customize-acoustic-models.md)
- [Tutorial: Erstellen eines benutzerdefinierten Sprachmodells](how-to-customize-language-model.md)
