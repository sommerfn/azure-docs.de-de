---
title: 'Schnellstart: Clientbibliothek der benutzerdefinierten Bing-Suche für Java | Microsoft-Dokumentation'
description: Enthält eine Beschreibung des Einstiegs in die Clientbibliothek der benutzerdefinierten Bing-Suche für Java, indem Suchergebnisse über Ihre Instanz der benutzerdefinierten Bing-Suche angefordert werden.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: ''
ms.topic: quickstart
ms.date: 09/17/2019
ms.author: aahi
ms.openlocfilehash: 7d1a7b2cdba082c78a1753ea0dcce6ead02ab036
ms.sourcegitcommit: b03516d245c90bca8ffac59eb1db522a098fb5e4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/19/2019
ms.locfileid: "71148320"
---
# <a name="quickstart-bing-custom-search-client-library-for-java"></a>Schnellstart: Clientbibliothek der benutzerdefinierten Bing-Suche für Java

Steigen Sie in die Clientbibliothek der benutzerdefinierten Bing-Suche für Java ein. Führen Sie die nachfolgenden Schritte zum Installieren des Pakets aus, und testen Sie den Beispielcode für grundlegende Aufgaben. Die API für die benutzerdefinierte Bing-Suche ermöglicht das Erstellen einer maßgeschneiderten werbefreien Suchbenutzeroberfläche für Themen, die Sie interessieren.

Verwenden Sie die Clientbibliothek der benutzerdefinierten Bing-Suche für Java wie folgt:

* Ermitteln Sie über Ihre Instanz der benutzerdefinierten Bing-Suche Suchergebnisse im Web. 

[Referenzdokumentation](https://docs.microsoft.com/java/api/overview/azure/cognitiveservices/client/bingcustomsearch?view=azure-java-stable) | [Quellcode der Bibliothek](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Search.BingCustomSearch) | [Artefakt (Maven)](https://search.maven.org/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-customsearch/) | [Beispiele](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples)

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement – [Erstellen eines kostenlosen Kontos](https://azure.microsoft.com/free/)
* Aktuelle Version des [Java Development Kit (JDK)](https://www.oracle.com/technetwork/java/javase/downloads/index.html)
* [Gradle-Buildtool](https://gradle.org/install/) oder einen anderen Abhängigkeit-Manager
* Eine Instanz der benutzerdefinierten Bing-Suche. Weitere Informationen finden Sie unter [Schnellstart: Erstellen Ihrer ersten Instanz der benutzerdefinierten Bing-Suche](quick-start.md).

## <a name="setting-up"></a>Einrichten

### <a name="create-a-bing-custom-search-azure-resource"></a>Erstellen einer Azure-Ressource für die benutzerdefinierte Bing-Suche

Azure Cognitive Services werden von Azure-Ressourcen dargestellt, die Sie abonnieren. Erstellen Sie mithilfe des [Azure-Portals](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) oder der [Azure CLI](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account-cli) auf Ihrem lokalen Computer eine Ressource für die benutzerdefinierte Bing-Suche. Weitere Funktionen:

* Rufen Sie einen [Testschlüssel](https://azure.microsoft.com/try/cognitive-services/#decision) ab, mit dem Sie sieben Tage lang kostenlos testen können. Nach der Registrierung steht dieser auf der [Azure-Website](https://azure.microsoft.com/try/cognitive-services/my-apis/) zur Verfügung.  
* Zeigen Sie Ihre Ressource im [Azure-Portal](https://portal.azure.com/) an.

Nachdem Sie einen Schlüssel für Ihr Testabonnement bzw. Ihre Ressource erhalten haben, [erstellen Sie eine Umgebungsvariable](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) für den Schlüssel (`AZURE_BING_CUSTOM_SEARCH_API_KEY`).

### <a name="create-a-new-gradle-project"></a>Erstellen eines neuen Gradle-Projekts

> [!TIP]
> Falls Sie nicht Gradle verwenden, finden Sie die Details zur Clientbibliothek für andere Abhängigkeits-Manager im [zentralen Maven-Repository](https://search.maven.org/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-textanalytics/).

Erstellen Sie in einem Konsolenfenster (etwa cmd, PowerShell oder Bash) ein neues Verzeichnis für Ihre App, und rufen Sie es auf. 

```console
mkdir myapp && cd myapp
```

Führen Sie den Befehl `gradle init` in Ihrem Arbeitsverzeichnis aus. Mit diesem Befehl werden grundlegende Builddateien für Gradle erstellt, z. B. die Datei *build.gradle.kts*. Diese Datei wird zur Laufzeit zum Konfigurieren Ihrer Anwendung verwendet.

```console
gradle init --type basic
```

Wenn Sie zur Auswahl einer **DSL** aufgefordert werden, wählen Sie **Kotlin** aus.

## <a name="install-the-client-library"></a>Installieren der Clientbibliothek 

Navigieren Sie zur Datei *build.gradle.kts*, und öffnen Sie sie in Ihrer bevorzugten IDE bzw. Ihrem bevorzugten Text-Editor. Kopieren Sie anschließend diese Buildkonfiguration. Stellen Sie sicher, dass Sie die Clientbibliothek unter `dependencies` einbinden:

```kotlin
plugins {
    java
    application
}
application {
    mainClassName = "main.java.BingCustomSearchSample"
}
repositories {
    mavenCentral()
}
dependencies {
    compile("org.slf4j:slf4j-simple:1.7.25")
    compile("com.microsoft.azure.cognitiveservices:azure-cognitiveservices-customsearch:1.0.2")
}
```

Erstellen Sie einen Ordner für Ihre Beispiel-App. Führen Sie in Ihrem Arbeitsverzeichnis den folgenden Befehl aus:

```console
mkdir src/main/java
```

Navigieren Sie zum neuen Ordner, und erstellen Sie eine Datei mit dem Namen *BingCustomSearchSample.java*. Öffnen Sie sie, und fügen Sie die folgenden `import`-Anweisungen hinzu:


[!code-java[import statements](~/cognitive-services-java-sdk-samples/Search/BingCustomSearch/src/main/java/BingCustomSearchSample.java?name=imports)]

Erstellen Sie eine Klasse mit dem Namen `BingCustomSearchSample`.

```java
public class BingCustomSearchSample {
}
```

Erstellen Sie in der Klasse eine `main`-Methode und Variablen für den Azure-Endpunkt und -Schlüssel Ihrer Ressource. Wenn Sie die Umgebungsvariable nach dem Start der Anwendung erstellt haben, müssen Sie den Editor, die IDE oder die Shell, in dem bzw. der sie ausgeführt wird, schließen und wieder öffnen, damit der Zugriff auf die Variable möglich ist. Die Methoden werden später definiert.

[!code-java[main method](~/cognitive-services-java-sdk-samples/Search/BingCustomSearch/src/main/java/BingCustomSearchSample.java?name=main)]

## <a name="object-model"></a>Objektmodell

Der Client der benutzerdefinierten Bing-Suche ist ein [BingCustomSearchAPI](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.search.customsearch.bingcustomsearchapi?view=azure-java-stable)-Objekt, das über die [authenticate()](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.search.customsearch.bingcustomsearchmanager.authenticate?view=azure-java-stable#com_microsoft_azure_cognitiveservices_search_customsearch_BingCustomSearchManager_authenticate_String_)-Methode des [BingCustomSearchManager](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.search.customsearch.bingcustomsearchmanager?view=azure-java-stable)-Objekts erstellt wird. Sie können eine Suchanforderung senden, indem Sie die [BingCustomInstances.search()](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.search.customsearch.bingcustominstances.search?view=azure-java-stable#com_microsoft_azure_cognitiveservices_search_customsearch_BingCustomInstances_search__)-Methode des Clients verwenden.

Die API-Antwort ist ein [SearchResponse](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.search.customsearch.models.searchresponse?view=azure-java-stable)-Objekt, das Informationen zur Suchabfrage und die Suchergebnisse enthält.

## <a name="code-examples"></a>Codebeispiele

Diese Codeausschnitte veranschaulichen, wie die folgenden Aufgaben mit der Clientbibliothek der benutzerdefinierten Bing-Suche für Java ausgeführt werden:

* [Authentifizieren des Clients](#authenticate-the-client)
* [Abrufen von Suchergebnissen von Ihrer Instanz der benutzerdefinierten Suche](#get-search-results-from-your-custom-search-instance)

## <a name="authenticate-the-client"></a>Authentifizieren des Clients

Ihre main-Methode sollte ein [BingCustomSearchManager](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.search.customsearch.bingcustomsearchapi?view=azure-java-stable)-Objekt enthalten, mit dem für Ihren Schlüssel das `authenticate()`-Element aufgerufen wird.

```java
BingCustomSearchAPI client = BingCustomSearchManager.authenticate(subscriptionKey);
```

## <a name="get-search-results-from-your-custom-search-instance"></a>Abrufen von Suchergebnissen von Ihrer Instanz der benutzerdefinierten Suche

Verwenden Sie die Funktion [BingCustomInstances.search()](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.search.customsearch.bingcustominstances.search?view=azure-java-stable#com_microsoft_azure_cognitiveservices_search_customsearch_BingCustomInstances_search__) des Clients, um eine Suchabfrage an Ihre benutzerdefinierte Instanz zu senden. Legen Sie `withCustomConfig` auf Ihre benutzerdefinierte Konfigurations-ID fest, oder verwenden Sie den Standardwert `1`. Nachdem Sie eine Antwort von der API erhalten haben, können Sie überprüfen, ob Suchergebnisse gefunden wurden. Wenn ja: Rufen Sie das erste Suchergebnis ab, indem Sie die Funktion `webPages().value().get()` der Antwort aufrufen und den Namen und die URL des Ergebnisses ausgeben. 

[!code-java[call the custom search API](~/cognitive-services-java-sdk-samples/Search/BingCustomSearch/src/main/java/BingCustomSearchSample.java?name=runSample)]

## <a name="run-the-application"></a>Ausführen der Anwendung

Erstellen Sie die App mit dem folgenden Befehl aus dem Hauptverzeichnis Ihres Projekts:

```console
gradle build
```

Führen Sie die Anwendung mit dem Ziel `run` aus:

```console
gradle run
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie ein Cognitive Services-Abonnement bereinigen und entfernen möchten, können Sie die Ressource oder die Ressourcengruppe löschen. Wenn Sie die Ressourcengruppe löschen, werden auch alle anderen Ressourcen gelöscht, die ihr zugeordnet sind.

* [Portal](../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure-Befehlszeilenschnittstelle](../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erstellen einer Web-App für die benutzerdefinierte Suche](./tutorials/custom-search-web-page.md)
