---
title: Veröffentlichen einer Funktion für Azure mithilfe von Java und Maven
description: Hier erfahren Sie, wie Sie mit Java und Maven eine einfache Funktion mit HTTP-Trigger erstellen und für Azure veröffentlichen.
author: rloutlaw
ms.topic: quickstart
ms.date: 08/10/2018
ms.custom: mvc, devcenter, seo-java-july2019, seo-java-august2019, seo-java-september2019
ms.openlocfilehash: cb43f558a5c983a8a4cc3823b278b75cb8cde78d
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74230744"
---
# <a name="quickstart-use-java-and-maven-to-create-and-publish-a-function-to-azure"></a>Schnellstart: Erstellen und Veröffentlichen einer Funktion für Azure mithilfe von Java und Maven

In diesem Artikel erfahren Sie, wie Sie mit dem Maven-Befehlszeilentool eine Java-Funktion erstellen und für Azure Functions veröffentlichen. Der fertige Funktionscode wird dann in Azure im Rahmen eines [serverlosen Hostingplans](functions-scale.md#consumption-plan) ausgeführt und durch eine HTTP-Anforderung ausgelöst.

<!--
> [!NOTE] 
> You can also create a Kotlin-based Azure Functions project by using the azure-functions-kotlin-archetype instead. Visit the [GitHub repository](https://github.com/microsoft/azure-maven-archetypes/tree/develop/azure-functions-kotlin-archetype) for more information.
-->

## <a name="prerequisites"></a>Voraussetzungen

Um Funktionen mit Java zu entwickeln, muss Folgendes installiert sein:

- [Java Developer Kit](https://aka.ms/azure-jdks), Version 8
- [Apache Maven](https://maven.apache.org), Version 3.0 oder höher
- [Azure-Befehlszeilenschnittstelle]
- [Azure Functions Core Tools](./functions-run-local.md#v2), Version 2.6.666 oder höher
- Ein Azure-Abonnement.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


> [!IMPORTANT]
> Damit Sie diesen Schnellstart durchführen können, muss die Umgebungsvariable JAVA_HOME auf den Installationsspeicherort des JDK festgelegt sein.

## <a name="generate-a-new-functions-project"></a>Generieren eines neuen Functions-Projekts

Führen Sie in einem leeren Ordner den folgenden Befehl aus, um das Functions-Projekt über einen [Maven-Archetyp](https://maven.apache.org/guides/introduction/introduction-to-archetypes.html) zu generieren.

### <a name="linuxmacos"></a>Linux/macOS

```bash
mvn archetype:generate \
    -DarchetypeGroupId=com.microsoft.azure \
    -DarchetypeArtifactId=azure-functions-archetype 
```

> [!NOTE]
> Sollten bei der Befehlsausführung Probleme auftreten, überprüfen Sie, welche Version von `maven-archetype-plugin` verwendet wird. Da Sie den Befehl in einem leeren Verzeichnis ohne Datei vom Typ `.pom` ausführen, wird möglicherweise versucht, ein Plug-In mit der älteren Version aus `~/.m2/repository/org/apache/maven/plugins/maven-archetype-plugin` auszuführen, wenn Sie ein Upgrade für eine ältere Maven-Version ausgeführt haben. Löschen Sie in diesem Fall das Verzeichnis `maven-archetype-plugin`, und führen Sie den Befehl erneut aus.

### <a name="windows"></a>Windows

```powershell
mvn archetype:generate `
    "-DarchetypeGroupId=com.microsoft.azure" `
    "-DarchetypeArtifactId=azure-functions-archetype"
```

```cmd
mvn archetype:generate ^
    "-DarchetypeGroupId=com.microsoft.azure" ^
    "-DarchetypeArtifactId=azure-functions-archetype"
```

Maven fordert Sie zur Eingabe von Werten auf, die erforderlich sind, um die Generierung des Projekts bei der Bereitstellung abzuschließen. Geben Sie die folgenden Werte ein, wenn Sie dazu aufgefordert werden:

| Wert | BESCHREIBUNG |
| ----- | ----------- |
| **groupId** | Ein Wert, der Ihr Projekt projektübergreifend eindeutig identifiziert. Für den Wert müssen die [Paketbenennungsregeln](https://docs.oracle.com/javase/specs/jls/se6/html/packages.html#7.7) für Java eingehalten werden. In den Beispielen dieser Schnellstartanleitung wird `com.fabrikam.functions` verwendet. |
| **artifactId** | Der Name des Behälters (ohne Versionsnummer). In den Beispielen dieser Schnellstartanleitung wird `fabrikam-functions` verwendet. |
| **version** | Wählen Sie den Standardwert (`1.0-SNAPSHOT`) aus. |
| **package** | Das Java-Paket für den generierten Funktionscode. Verwenden Sie den Standardwert. In den Beispielen dieser Schnellstartanleitung wird `com.fabrikam.functions` verwendet. |
| **appName** | Ein global eindeutiger Name, der Ihre neue Funktions-App in Azure identifiziert. Verwenden Sie den Standardwert (_artifactId_ mit einer angefügten Zufallszahl). Notieren Sie sich diesen Wert. Er wird später noch benötigt. |
| **appRegion** | Wählen Sie eine [Region](https://azure.microsoft.com/regions/) in Ihrer Nähe oder in der Nähe von anderen Diensten aus, auf die Ihre Funktionen zugreifen. Der Standardwert lautet `westus`. Führen Sie den folgenden [Azure-Befehlszeilenschnittstelle]-Befehl aus, um eine Liste aller Regionen zu erhalten:<br/>`az account list-locations --query '[].{Name:name}' -o tsv` |
| **Ressourcengruppe** | Der Name der neuen [Ressourcengruppe](../azure-resource-manager/resource-group-overview.md), in der Ihre Funktions-App erstellt wird. Verwenden Sie `myResourceGroup` (wird in Beispielen dieser Schnellstartanleitung verwendet). Eine Ressourcengruppe muss für Ihr Azure-Abonnement eindeutig sein.|

Geben Sie zur Bestätigung `Y` ein, oder drücken Sie die EINGABETASTE.

Maven erstellt die Projektdateien in einem neuen Ordner und benennt ihn mit dem Wert von _artifactId_ (in diesem Beispiel: `fabrikam-functions`). 

Öffnen Sie die neue Datei „Function.java“ unter *src/main/java* in einem Text-Editor, und überprüfen Sie den generierten Code. Bei diesem Code handelt es sich um eine Funktion mit [HTTP-Trigger](functions-bindings-http-webhook.md), die den Text der Anforderung zurückgibt. 

## <a name="run-the-function-locally"></a>Lokales Ausführen der Funktion

Führen Sie den folgenden Befehl aus, um zum neu erstellten Projektordner zu wechseln, das Funktionsprojekt zu erstellen und es auszuführen:

```console
cd fabrikam-function
mvn clean package 
mvn azure-functions:run
```

Wenn Sie das Projekt lokal ausführen, wird eine Ausgabe wie die folgende von Azure Functions Core Tools angezeigt:

```Output
...

Now listening on: http://0.0.0.0:7071
Application started. Press Ctrl+C to shut down.

Http Functions:

    HttpTrigger-Java: [GET,POST] http://localhost:7071/api/HttpTrigger-Java
...
```

Lösen Sie die Funktion über die Befehlszeile aus. Verwenden Sie dazu cURL in einem neuen Terminalfenster:

```CMD
curl -w "\n" http://localhost:7071/api/HttpTrigger-Java --data AzureFunctions
```

```Output
Hello AzureFunctions!
```
Der [Funktionsschlüssel](functions-bindings-http-webhook.md#authorization-keys) ist bei lokaler Ausführung nicht erforderlich. Verwenden Sie `Ctrl+C` im Terminal, um den Funktionscode anzuhalten.

## <a name="deploy-the-function-to-azure"></a>Bereitstellen der Funktion in Azure

Wenn Sie Ihre Funktions-App erstmals bereitstellen, werden in Azure eine Funktions-App und zugehörige Ressourcen erstellt. Vor der Bereitstellung müssen Sie sich zunächst mithilfe des Azure CLI-Befehls [az login](/cli/azure/authenticate-azure-cli) bei Ihrem Azure-Abonnement anmelden. 

```azurecli
az login
```

> [!TIP]
> Falls Ihr Konto auf mehrere Abonnements zugreifen kann, verwenden Sie [az account set](/cli/azure/account#az-account-set), um das Standardabonnement für diese Sitzung festzulegen. 

Verwenden Sie den folgenden Maven-Befehl, um Ihr Projekt für eine neue Funktions-App bereitzustellen: 

```azurecli
mvn azure-functions:deploy
```

Dieses Maven-Ziel vom Typ `azure-functions:deploy` erstellt die folgenden Ressourcen in Azure:

+ Ressourcengruppe – benannt mit der angegebenen Ressourcengruppe (_resourceGroup_).
+ Speicherkonto – von Functions benötigt. Der Name wird nach dem Zufallsprinzip basierend auf den Anforderungen für den Speicherkontonamen generiert.
+ App Service-Plan – serverloses Hosting für Ihre Funktions-App in der angegebenen App-Region (_appRegion_). Der Name wird nach dem Zufallsprinzip generiert.
+ Funktions-App – die Bereitstellungs-und Ausführungseinheit für Ihre Funktionen. Der Name ist Ihr App-Name (_appName_) mit angefügter Zufallszahl. 

Bei der Bereitstellung werden mithilfe von [zip deployment](functions-deployment-technologies.md#zip-deploy) auch die Projektdateien gepackt und für die neue Funktions-App bereitgestellt (mit aktiviertem Modus für die paketbasierte Ausführung).

Nach Abschluss der Bereitstellung wird die URL für den Zugriff auf Ihre Funktions-App-Endpunkte angezeigt. Da der von uns veröffentlichte HTTP-Trigger `authLevel = AuthorizationLevel.FUNCTION` verwendet, müssen Sie den Funktionsschlüssel abrufen, um den Funktionsendpunkt über HTTP aufrufen zu können. Den Funktionsschlüssel können Sie am einfachsten über das [Azure-Portal] abrufen.

## <a name="get-the-http-trigger-url"></a>Abrufen der URL des HTTP-Triggers

<!--- We can updates this to remove portal dependency after the Maven archetype returns the full URLs with keys on publish (https://github.com/microsoft/azure-maven-plugins/issues/571). -->

Die erforderliche URL zum Auslösen Ihrer Funktion können Sie mit dem Funktionsschlüssel aus dem Azure-Portal abrufen. 

1. Navigieren Sie zum [Azure-Portal], melden Sie sich an, geben Sie im oberen Seitenbereich unter **Suche** den App-Namen (_appName_) Ihrer Funktions-App ein, und drücken Sie die EINGABETASTE.
 
1. Erweitern Sie in Ihrer Funktions-App **Funktionen (schreibgeschützt)** , wählen Sie Ihre Funktion aus, und wählen Sie anschließend rechts oben die Option **</> Funktions-URL abrufen** aus. 

    ![Kopieren der URL der Funktion aus dem Azure-Portal](./media/functions-create-java-maven/get-function-url-portal.png)

1. Wählen Sie **default (Function key)** (Standard (Funktionsschlüssel)) und anschließend **Kopieren** aus. 

Nun können Sie mithilfe der kopierte URL auf Ihre Funktion zugreifen.

## <a name="verify-the-function-in-azure"></a>Überprüfen der Funktion in Azure

Die in Azure ausgeführte Funktions-App kann mithilfe von `cURL` überprüft werden. Ersetzen Sie hierzu die URL aus dem folgenden Beispiel durch die URL, die Sie aus dem Portal kopiert haben.

```azurecli
curl -w "\n" https://fabrikam-functions-20190929094703749.azurewebsites.net/api/HttpTrigger-Java?code=zYRohsTwBlZ68YF.... --data AzureFunctions
```

Dadurch wird eine POST-Anforderung mit `AzureFunctions` an den Funktionsendpunkt gesendet. Die Antwort sieht wie folgt aus:

```Output
Hello AzureFunctions!
```

## <a name="next-steps"></a>Nächste Schritte

Sie haben ein Java-Funktionsprojekt mit einer per HTTP ausgelösten Funktion erstellt, auf Ihrem lokalen Computer ausgeführt und für Azure bereitgestellt. Erweitern Sie nun Ihre Funktion durch...

> [!div class="nextstepaction"]
> [Hinzufügen einer Azure Storage-Warteschlangen-Ausgabebindung](functions-add-output-binding-storage-queue-java.md)


[Azure-Befehlszeilenschnittstelle]: /cli/azure
[Azure-Portal]: https://portal.azure.com