---
title: 'Schnellstart: Content Moderator-Clientbibliothek für Java'
titleSuffix: Azure Cognitive Services
description: Informieren Sie sich über die ersten Schritte mit der Azure Cognitive Services Content Moderator-Clientbibliothek für Java.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: quickstart
ms.date: 10/25/2019
ms.author: pafarley
ms.openlocfilehash: edc51be93ba209a1c60970e6fa1b47fca75048c6
ms.sourcegitcommit: 827248fa609243839aac3ff01ff40200c8c46966
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73744438"
---
# <a name="quickstart-content-moderator-client-library-for-java"></a>Schnellstart: Content Moderator-Clientbibliothek für Java

Hier erhalten Sie Informationen zu den ersten Schritten mit der Content Moderator-Clientbibliothek für Java. Führen Sie die nachfolgenden Schritte zum Installieren des Pakets aus, und testen Sie den Beispielcode für grundlegende Aufgaben. Content Moderator gehört zu Cognitive Services und überprüft Text-, Bild- und Videoinhalte auf potenziell anstößiges, riskantes oder anderweitig unerwünschtes Material. Gefundenes Material dieser Art wird mithilfe von Flags entsprechend gekennzeichnet. Mit Flags versehene Inhalte können dann von Ihrer App angemessen behandelt werden, um Vorgaben zu erfüllen oder den Benutzern die vorgesehene Umgebung zu bieten.

Führen Sie mit der Content Moderator-Clientbibliothek für Java die folgenden Aktionen aus:

* Moderieren von Bildern mit nicht jugendfreiem oder freizügigem Inhalt, Text oder menschlichen Gesichtern

[Referenzdokumentation](https://docs.microsoft.com/java/api/overview/azure/cognitiveservices/client/contentmoderator?view=azure-java-stable) | [Artefakt (Maven)](https://mvnrepository.com/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-contentmoderator) | [Beispiele](https://docs.microsoft.com/samples/browse/?products=azure&term=content-moderator)

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement – [Erstellen eines kostenlosen Kontos](https://azure.microsoft.com/free/)
* Aktuelle Version des [Java Development Kit (JDK)](https://www.oracle.com/technetwork/java/javase/downloads/index.html)
* [Gradle-Buildtool](https://gradle.org/install/) oder einen anderen Abhängigkeit-Manager

## <a name="setting-up"></a>Einrichten

### <a name="create-a-content-moderator-azure-resource"></a>Erstellen einer Content Moderator-Ressource in Azure

Azure Cognitive Services werden von Azure-Ressourcen dargestellt, die Sie abonnieren. Erstellen Sie mithilfe des [Azure-Portals](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) oder der [Azure CLI](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account-cli) auf Ihrem lokalen Computer eine Ressource für Content Moderator. Weitere Funktionen:

* Rufen Sie einen kostenlosen [Testschlüssel](https://azure.microsoft.com/try/cognitive-services/#decision) ab, der sieben Tage lang gültig ist. Nach der Registrierung steht dieser auf der [Azure-Website](https://azure.microsoft.com/try/cognitive-services/my-apis/) zur Verfügung.  
* Zeigen Sie Ihre Ressource im [Azure-Portal](https://portal.azure.com/) an.

Nachdem Sie einen Schlüssel für Ihr Testabonnement bzw. Ihre Ressource erhalten haben, [erstellen Sie eine Umgebungsvariable](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) für den Schlüssel (`AZURE_CONTENTMODERATOR_KEY`).

### <a name="create-a-new-gradle-project"></a>Erstellen eines neuen Gradle-Projekts

Erstellen Sie in einem Konsolenfenster (etwa cmd, PowerShell oder Bash) ein neues Verzeichnis für Ihre App, und rufen Sie es auf. 

```console
mkdir myapp && cd myapp
```
Führen Sie `gradle init`aus. Mit diesem Befehl werden grundlegende Builddateien für Gradle, u. a. die Datei *build.gradle.kts*, erstellt. Diese Datei wird zur Laufzeit zum Erstellen und Konfigurieren Ihrer Anwendung verwendet. Führen Sie den folgenden Befehl in Ihrem Arbeitsverzeichnis aus:

```console
gradle init --type basic
```

Wenn Sie zur Auswahl einer Buildskript-DSL aufgefordert werden, wählen Sie **Kotlin** aus.

Navigieren Sie zur Datei *build.gradle.kts*, und öffnen Sie sie in Ihrer bevorzugten IDE bzw. Ihrem bevorzugten Text-Editor. Kopieren Sie anschließend die folgende Buildkonfiguration. Diese Konfiguration definiert das Projekt als Java-Anwendung, deren Einstiegspunkt die Klasse **ContentModeratorQuickstart** ist. Dabei werden das Content Moderator SDK sowie das Gson SDK für die JSON-Serialisierung importiert.

```kotlin
plugins {
    java
    application
}

application{ 
    mainClassName = "ContentModeratorQuickstart"
}

repositories{
    mavenCentral()
}

dependencies{
    compile(group = "com.microsoft.azure.cognitiveservices", name = "azure-cognitiveservices-contentmoderator", version = "1.0.2-beta")
    compile(group = "com.google.code.gson", name = "gson", version = "2.8.5")
}
```

Führen Sie in Ihrem Arbeitsverzeichnis den folgenden Befehl aus, um einen Projektquellordner zu erstellen.

```console
mkdir -p src/main/java
```

Erstellen Sie dann im neuen Ordner eine Datei mit dem Namen *ContentModeratorQuickstart.java*. Öffnen Sie die Datei in Ihrem bevorzugten Editor bzw. Ihrer bevorzugten IDE, und importieren Sie am Anfang die folgenden Bibliotheken:

[!code-java[](~/cognitive-services-java-sdk-samples/ContentModerator/ContentModeratorQuickstart/src/main/java/ContentModeratorQuickstart.java?name=snippet_imports)]

## <a name="object-model"></a>Objektmodell

Die folgenden Klassen verarbeiten einige der Hauptfunktionen des Content Moderator Java SDK.

|NAME|BESCHREIBUNG|
|---|---|
|[ContentModeratorClient](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.vision.contentmoderator.contentmoderatorclient?view=azure-java-stable)|Diese Klasse wird für alle Content Moderator-Funktionen benötigt. Sie instanziieren sie mit Ihren Abonnementinformationen und verwenden sie zum Generieren von Instanzen anderer Klassen.|
|[ImageModeration](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.vision.contentmoderator.imagemoderations?view=azure-java-stable)|Diese Klasse stellt die Funktionalität zum Analysieren von Bildern auf nicht jugendfreie Inhalte, persönliche Informationen oder menschliche Gesichter bereit.|
|[TextModerations](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.vision.contentmoderator.textmoderations?view=azure-java-stable)|Diese Klasse stellt die Funktionalität zum Analysieren von Text auf Sprache, Obszönitäten, Fehler und persönliche Informationen bereit.|
|[Überprüfungen](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.vision.contentmoderator.reviews?view=azure-java-stable)|Diese Klasse stellt die Funktionalität der Überprüfungs-APIs bereit, einschließlich der Methoden zum Erstellen von Aufträgen, benutzerdefinierten Workflows und Überprüfungen durch Personen.|


## <a name="code-examples"></a>Codebeispiele

Diese Codeausschnitte veranschaulichen, wie die folgenden Aufgaben mit der Content Moderator-Clientbibliothek für Java ausgeführt werden:

* [Authentifizieren des Clients](#authenticate-the-client)
* [Moderieren von Bildern](#moderate-images)

## <a name="authenticate-the-client"></a>Authentifizieren des Clients

> [!NOTE]
> In diesem Schritt wird vorausgesetzt, dass Sie für den Content Moderator-Schlüssel namens `AZURE_CONTENTMODERATOR_KEY` [eine Umgebungsvariable erstellt haben](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication).

Erstellen Sie mithilfe des Werts für den Abonnementendpunkt und der Umgebungsvariablen für den Abonnementschlüssel in der Methode `main` der Anwendung ein [ContentModeratorClient](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.vision.contentmoderator.contentmoderatorclient?view=azure-java-stable)-Objekt. 

> [!NOTE]
> Wenn Sie die Umgebungsvariable nach dem Start der Anwendung erstellt haben, müssen der Editor, die IDE oder die Shell, in dem bzw. der sie ausgeführt wird, geschlossen und erneut geöffnet werden, damit der Zugriff auf die Variable möglich ist.

[!code-java[](~/cognitive-services-java-sdk-samples/ContentModerator/ContentModeratorQuickstart/src/main/java/ContentModeratorQuickstart.java?name=snippet_client)]

## <a name="moderate-images"></a>Moderieren von Bildern

### <a name="get-images"></a>Abrufen von Bildern

Erstellen Sie im Ordner **src/main/** Ihres Projekts den Ordner **resources**, und öffnen Sie ihn. Erstellen Sie anschließend eine neue Textdatei namens *ImageFiles.txt*. In diese Datei fügen Sie die URLs der zu analysierenden Bilder ein (jeweils eine URL pro Zeile). Sie können die folgenden Beispielbilder verwenden:

```
https://moderatorsampleimages.blob.core.windows.net/samples/sample2.jpg
https://moderatorsampleimages.blob.core.windows.net/samples/sample5.png
```

### <a name="define-helper-class"></a>Definieren der Hilfsklasse

Fügen Sie dann in der Datei *ContentModeratorQuickstart.java* die folgende Klassendefinition innerhalb der Klasse **ContentModeratorQuickstart** hinzu. Diese innere Klasse wird später bei der Bildmoderation verwendet.

[!code-java[](~/cognitive-services-java-sdk-samples/ContentModerator/ContentModeratorQuickstart/src/main/java/ContentModeratorQuickstart.java?name=snippet_evaluationdata)]

### <a name="iterate-through-images"></a>Durchlaufen von Bildern

Fügen Sie als Nächstes am Ende der Methode `main` den folgenden Code hinzu. Sie können ihn auch einer separaten Methode hinzufügen, die über `main` aufgerufen wird. Dieser Code durchläuft die einzelnen Zeilen der Datei _ImageFiles.txt_.

[!code-java[](~/cognitive-services-java-sdk-samples/ContentModerator/ContentModeratorQuickstart/src/main/java/ContentModeratorQuickstart.java?name=snippet_imagemod_iterate)]

### <a name="check-for-adultracy-content"></a>Überprüfen von Bildern auf nicht jugendfreie/freizügige Inhalte
Diese Codezeile überprüft das Bild unter der angegebenen URL auf nicht jugendfreien oder freizügigen Inhalt. Informationen zu diesen Begriffen finden Sie im konzeptionellen Leitfaden zur Bildmoderation.

[!code-java[](~/cognitive-services-java-sdk-samples/ContentModerator/ContentModeratorQuickstart/src/main/java/ContentModeratorQuickstart.java?name=snippet_imagemod_ar)]

### <a name="check-for-text"></a>Überprüfen auf Text
Diese Codezeile überprüft das Bild auf sichtbaren Text.

[!code-java[](~/cognitive-services-java-sdk-samples/ContentModerator/ContentModeratorQuickstart/src/main/java/ContentModeratorQuickstart.java?name=snippet_imagemod_text)]

### <a name="check-for-faces"></a>Überprüfen auf Gesichter
Diese Codezeile überprüft das Bild auf menschliche Gesichter.

[!code-java[](~/cognitive-services-java-sdk-samples/ContentModerator/ContentModeratorQuickstart/src/main/java/ContentModeratorQuickstart.java?name=snippet_imagemod_faces)]

Speichern Sie abschließend die zurückgegebenen Informationen in der Liste `EvaluationData`.

[!code-java[](~/cognitive-services-java-sdk-samples/ContentModerator/ContentModeratorQuickstart/src/main/java/ContentModeratorQuickstart.java?name=snippet_imagemod_storedata)]

### <a name="print-results"></a>Ausgeben der Ergebnisse

Fügen Sie nach der Schleife `while` den folgenden Code hinzu. Dieser gibt die Ergebnisse in der Konsole und in einer Ausgabedatei (*src/main/resources/ModerationOutput.json*) aus.

[!code-java[](~/cognitive-services-java-sdk-samples/ContentModerator/ContentModeratorQuickstart/src/main/java/ContentModeratorQuickstart.java?name=snippet_imagemod_printdata)]

Schließen Sie die `try`-Anweisung aus, und fügen Sie eine `catch`-Anweisung hinzu, um die Methode abzuschließen.

[!code-java[](~/cognitive-services-java-sdk-samples/ContentModerator/ContentModeratorQuickstart/src/main/java/ContentModeratorQuickstart.java?name=snippet_imagemod_catch)]

## <a name="run-the-application"></a>Ausführen der Anwendung

Sie können die App mit folgendem Befehl erstellen:

```console
gradle build
```

Führen Sie die Anwendung mit Befehl `gradle run` aus:

```console
gradle run
```

Navigieren Sie dann zur Datei *src/main/resources/ModerationOutput.json*, und zeigen Sie die Ergebnisse der Inhaltsmoderation an.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie ein Cognitive Services-Abonnement bereinigen und entfernen möchten, können Sie die Ressource oder die Ressourcengruppe löschen. Wenn Sie die Ressourcengruppe löschen, werden auch alle anderen Ressourcen gelöscht, die ihr zugeordnet sind.

* [Portal](../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure-Befehlszeilenschnittstelle](../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie gelernt, wie Sie die Content Moderator-Java-Bibliothek zum Ausführen von Moderationsaufgaben verwenden. Im Anschluss können Sie sich im konzeptionellen Leitfaden ausführlicher über die Moderation von Bildern oder anderen Medien informieren.

> [!div class="nextstepaction"]
>[Lernen von Bildmoderationskonzepten](https://docs.microsoft.com/azure/cognitive-services/content-moderator/image-moderation-api)

* [Was ist Azure Content Moderator?](./overview.md)
* Den Quellcode für dieses Beispiel finden Sie auf [GitHub](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples/blob/master/ContentModerator/ContentModeratorQuickstart/src/main/java/ContentModeratorQuickstart.java).