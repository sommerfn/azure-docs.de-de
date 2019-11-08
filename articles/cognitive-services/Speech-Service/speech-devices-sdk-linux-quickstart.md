---
title: 'Schnellstart: Ausführen des Speech Devices SDK unter Linux – Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: Voraussetzungen und Anleitung für den Einstieg in ein Speech Devices SDK für Linux.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/10/2019
ms.author: erhopf
ms.openlocfilehash: 5c881551648e8fc6078405e34fa3280723009b20
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73490974"
---
# <a name="quickstart-run-the-speech-devices-sdk-sample-app-on-linux"></a>Schnellstart: Ausführen der Speech Devices SDK-Beispiel-App unter Linux

In dieser Schnellstartanleitung erfahren Sie, wie Sie das Speech Devices SDK für Linux verwenden, um ein sprachaktiviertes Produkt zu erstellen oder es als Gerät für die [Unterhaltungstranskription](conversation-transcription-service.md) zu verwenden. Derzeit wird nur das [Azure Kinect DK](https://azure.microsoft.com/services/kinect-dk/) unterstützt.

Die Anwendung wird mit dem Speech SDK-Paket und der Eclipse Java-IDE (v4) unter 64-Bit-Linux (Ubuntu 16.04, Ubuntu 18.04, Debian 9) erstellt. Sie wird in einer 64-Bit Java 8-JRE (Runtime Environment) ausgeführt.

Für diese Anleitung wird ein [Azure Cognitive Services-Konto](get-started.md) mit einer Ressource für die Speech-Dienste benötigt. Wenn Sie über kein Konto verfügen, können Sie über die [kostenlose Testversion](https://azure.microsoft.com/try/cognitive-services/) einen Abonnementschlüssel abrufen.

Der Quellcode für die [Beispielanwendung](https://aka.ms/sdsdk-download-JRE) ist im Speech Devices SDK enthalten. Es ist auch auf [GitHub](https://github.com/Azure-Samples/Cognitive-Services-Speech-Devices-SDK) verfügbar.

## <a name="prerequisites"></a>Voraussetzungen

Für diese Schnellstartanleitung ist Folgendes erforderlich:

* Betriebssystem: 64-Bit-Linux (Ubuntu 16.04, Ubuntu 18.04, Debian 9)
* [Azure Kinect DK](https://azure.microsoft.com/services/kinect-dk/)
* [Eclipse Java IDE](https://www.eclipse.org/downloads/)
* Nur [Java 8](https://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html) oder [JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/index.html).
* Ein Azure-Abonnementschlüssel für den Speech-Dienst. [Hier erhalten Sie einen kostenlosen Schlüssel.](get-started.md)
* Laden Sie die aktuelle Version des [Speech Devices SDK](https://aka.ms/sdsdk-download-JRE) für Java herunter, und extrahieren Sie die ZIP in Ihrem Arbeitsverzeichnis.
   > [!NOTE]
   > Die Datei „JRE-Sample-Release.zip“ enthält die JRE-Beispiel-App. In dieser Schnellstartanleitung wird davon ausgegangen, dass die App in das Verzeichnis „/home/wcaltest/JRE-Sample-Release“ extrahiert wurde.

Stellen Sie sicher, dass diese Abhängigkeiten installiert sind, bevor Sie Eclipse starten.

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

Die Unterhaltungstranskription ist derzeit nur für „en-US“ und „zh-CN“ in den Regionen „centralus“ und „eastasia“ verfügbar. Sie müssen in einer dieser Regionen über einen Sprachschlüssel verfügen, um die Unterhaltungstranskription verwenden zu können.

Wenn Sie planen, die Absichten zu nutzen, benötigen Sie ein Abonnement des [Diensts „Language Understanding“ (LUIS)](https://docs.microsoft.com/azure/cognitive-services/luis/azureibizasubscription). Weitere Informationen zu LUIS und zur Absichtserkennung finden Sie unter [Tutorial: Erkennen von Absichten anhand von gesprochener Sprache mit dem Speech SDK für C#](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-recognize-intents-from-speech-csharp). Für diese App ist ein [Beispiel für ein LUIS-Modell](https://aka.ms/sdsdk-luis) verfügbar.

## <a name="create-and-configure-the-project"></a>Erstellen und Konfigurieren des Projekts

1. Starten Sie Eclipse.

1. Geben Sie im **Startprogramm für die Eclipse-IDE** im Feld **Arbeitsbereich** den Namen eines neuen Arbeitsbereichsverzeichnisses ein. Wählen Sie dann **Starten** aus.

   ![Screenshot des Eclipse-Startprogramms](media/speech-devices-sdk/eclipse-launcher-linux.png)

1. Nach kurzer Zeit wird das Hauptfenster der Eclipse-IDE angezeigt. Schließen Sie den Begrüßungsbildschirm, wenn ein solcher vorhanden ist.

1. Erstellen Sie über die Eclipse-Menüleiste ein neues Projekt, indem Sie **Datei** > **Neu** > **Java-Projekt** wählen. Falls dies nicht möglich ist, können Sie **Projekt** und dann **Java-Projekt** wählen.

1. Der **Assistent für ein neues Java-Projekt** wird gestartet. Verwenden Sie **Durchsuchen**, um nach dem Speicherort des Beispielprojekts zu suchen. Wählen Sie **Fertig stellen** aus.

   ![Screenshot des Assistenten für ein neues Java-Projekt](media/speech-devices-sdk/eclipse-new-java-project-linux.png)

1. Klicken Sie im **Paket-Explorer** mit der rechten Maustaste auf Ihr Projekt. Wählen Sie **Konfigurieren** > **In Maven-Projekt konvertieren** aus dem Kontextmenü aus. Wählen Sie **Fertig stellen** aus.

   ![Screenshot des Paket-Explorers](media/speech-devices-sdk/eclipse-convert-to-maven.png)

1. Klicken Sie im **Paket-Explorer** mit der rechten Maustaste auf Ihr Projekt. Wählen Sie **Eigenschaften** und dann **Run/Debug Settings** > **New…** > **Java Application** (Ausführungs-/Debugeinstellungen > Neu > Java-Anwendung). 

1. Das Fenster **Konfiguration bearbeiten** wird angezeigt. Geben Sie im Feld **Name** das Wort **Main** ein, und verwenden Sie **Suchen** für die **Main-Klasse**, um nach **com.microsoft.cognitiveservices.speech.samples.FunctionsList** zu suchen und diese Option auszuwählen.

   ![Screenshot: Bearbeiten der Startkonfiguration](media/speech-devices-sdk/eclipse-edit-launch-configuration-linux.png)

1. Wählen Sie ebenfalls im Fenster **Konfiguration bearbeiten** die Seite **Umgebung** und dann **Neu**. Das Fenster **New Environment Variable** (Neue Umgebungsvariable) wird angezeigt. Geben Sie im Feld **Name** den Namen **LD_LIBRARY_PATH** und im Feld **Wert** den Ordner mit den SO-Dateien ein, z. B. **/home/wcaltest/JRE-Sample-Release**.

1. Kopieren Sie `kws.table` und `participants.properties` in den Projektordner **target/classes**.


## <a name="configure-the-sample-application"></a>Konfigurieren der Beispielanwendung

1. Fügen Sie dem Quellcode den Abonnementschlüssel für Ihre Spracherkennung hinzu. Wenn Sie die Absichtserkennung ausprobieren möchten, fügen Sie auch Ihren Abonnementschlüssel für den [Language Understanding Intelligent Service (LUIS)](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/) und die Anwendungs-ID hinzu.

   Für die Spracherkennung und LUIS werden Ihre Informationen in `FunctionsList.java` gespeichert:

   ```java
    // Subscription
    private static String SpeechSubscriptionKey = "<enter your subscription info here>";
    private static String SpeechRegion = "westus"; // You can change this if your speech region is different.
    private static String LuisSubscriptionKey = "<enter your subscription info here>";
    private static String LuisRegion = "westus2"; // you can change this, if you want to test the intent, and your LUIS region is different.
    private static String LuisAppId = "<enter your LUIS AppId>";
   ```

    Wenn Sie die Unterhaltungstranskription verwenden, sind Ihr Sprachschlüssel und die Regionsdaten auch in `Cts.java` erforderlich:

   ```java
    private static final String CTSKey = "<Conversation Transcription Service Key>";
    private static final String CTSRegion="<Conversation Transcription Service Region>";// Region may be "centralus" or "eastasia"
    ```

1. Das Standardschlüsselwort (keyword) ist „Computer“. Sie können auch eines der anderen angebotenen Schlüsselwörter wie „Machine“ oder „Assistant“ ausprobieren. Die Ressourcendateien für diese alternativen Schlüsselwörter finden Sie im Speech Devices SDK im Ordner „keyword“. Beispielsweise enthält `/home/wcaltest/JRE-Sample-Release/keyword/Computer` die Dateien für das Schlüsselwort „Computer“.

   > [!TIP]
   > Sie können auch [ein benutzerdefiniertes Schlüsselwort erstellen](speech-devices-sdk-create-kws.md).

    Um ein neues Schlüsselwort zu verwenden, aktualisieren Sie die folgenden zwei Codezeilen in `FunctionsList.java` und kopieren das Paket mit dem Schlüsselwort in Ihre App. Gehen Sie beispielsweise wie folgt vor, wenn Sie das Schlüsselwort „Machine“ aus dem Schlüsselwortpaket `kws-machine.zip` verwenden möchten:

   * Kopieren Sie das Schlüsselwortpaket in den Projektordner **target/classes**.

   * Aktualisieren Sie die Datei `FunctionsList.java` mit dem Schlüsselwort und dem Paketnamen:

     ```java
     private static final String Keyword = "Machine";
     private static final String KeywordModel = "kws-machine.zip" // set your own keyword package name.
     ```

## <a name="run-the-sample-application-from-eclipse"></a>Ausführen der Beispielanwendung aus Eclipse

1. Wählen Sie in der Eclipse-Menüleiste **Run** > **Run** (Ausführen > Ausführen). 

1. Die Beispielanwendung des Speech-Geräte-SDK wird gestartet und zeigt die folgenden Optionen an:

   ![Speech-Geräte-SDK-Beispielanwendung und Optionen](media/speech-devices-sdk/java-sample-app-linux.png)

1. Testen Sie die neue Demoversion der **Unterhaltungstranskription**. Beginnen Sie mit der Transkription, indem Sie **Session** > **Start** (Sitzung > Starten) verwenden. Standardmäßig ist jeder Gast. Wenn Sie aber über Stimmsignaturen der Teilnehmer verfügen, können diese im Projektordner **target/classes** in `participants.properties` eingefügt werden. Informationen zum Generieren der Stimmsignatur finden Sie unter [Transkribieren von Konversationen (SDK)](how-to-use-conversation-transcription-service.md).

   ![Demoanwendung für die Unterhaltungstranskription](media/speech-devices-sdk/cts-sample-app-linux.png)

## <a name="create-and-run-standalone-the-application"></a>Erstellen und Ausführen der eigenständigen Anwendung

1. Klicken Sie im **Paket-Explorer** mit der rechten Maustaste auf Ihr Projekt. Wählen Sie **Exportieren**. 
1. Das Fenster **Exportieren** wird angezeigt. Erweitern Sie die Option **Java**, und wählen Sie **Runnable JAR file** (Ausführbare JAR-Datei) und dann **Weiter**.

   ![Screenshot: Fenster „Exportieren“](media/speech-devices-sdk/eclipse-export-linux.png) 

1. Das Fenster **Runnable JAR File Export** (Ausführbare JAR-Datei: Export) wird angezeigt. Wählen Sie ein **Exportziel** für die Anwendung aus, und wählen Sie anschließend **Fertig stellen**.
 
   ![Screenshot: Ausführbare JAR-Datei: Export](media/speech-devices-sdk/eclipse-export-jar-linux.png)

1. Fügen Sie `kws.table` und `participants.properties` in den oben gewählten Zielordner ein, da diese Dateien von der Anwendung benötigt werden.

1. Legen Sie „LD_LIBRARY_LIB“ auf den Ordner fest, der die SO-Dateien enthält.

     ```bash
     export LD_LIBRARY_PATH=/home/wcaltest/JRE-Sample-Release
     ```

1. Ausführen der eigenständigen Anwendung

     ```bash
     java -jar SpeechDemo.jar
     ```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Lesen Sie die Anmerkungen zu dieser Version](devices-sdk-release-notes.md)
