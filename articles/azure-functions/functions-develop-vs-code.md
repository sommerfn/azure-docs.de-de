---
title: Entwickeln von Azure Functions mithilfe von Visual Studio Code | Microsoft-Dokumentation
description: In diesem Artikel erhalten Sie Informationen über das Entwickeln und Testen von Azure Functions mithilfe der Azure Functions-Erweiterung für Visual Studio Code.
services: functions
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: conceptual
ms.date: 04/11/2019
ms.author: glenga
ms.openlocfilehash: 17550e148ea61eb69a20fc6a3215dfb63b65f18e
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2019
ms.locfileid: "67454241"
---
# <a name="develop-azure-functions-using-visual-studio-code"></a>Entwickeln von Azure Functions mithilfe von Visual Studio Code

Mit der [Azure Functions-Erweiterung für Visual Studio Code] können Sie Funktionen lokal entwickeln und auf Azure bereitstellen. Wenn Sie zum ersten Mal mit Azure Functions arbeiten, finden Sie unter [Einführung in Azure Functions](functions-overview.md) weitere Informationen.

Die Azure Functions-Erweiterung bietet folgende Vorteile: 

* Bearbeiten, Erstellen und Ausführen von Funktionen auf Ihrem lokalen Entwicklungscomputer. 
* Veröffentlichen von Azure Functions-Projekten direkt in Azure. 
* Schreiben von Funktionen in verschiedenen Sprachen, während Sie von allen Vorteilen der Entwicklung mit Visual Studio Code profitieren. 

Die Erweiterung kann mit den folgenden Sprachen verwendet werden, die von der Azure Functions-Runtime, Version 2.x, unterstützt werden: 

* [C# kompiliert](functions-dotnet-class-library.md) 
* [C# Skript](functions-reference-csharp.md)<sup>*</sup>
* [JavaScript](functions-reference-node.md)
* [Java](functions-reference-java.md)
* [PowerShell](functions-reference-powershell.md)
* [Python](functions-reference-python.md)

<sup>*</sup>Erfordert, dass Sie [C# Skript als Standardsprache des Projekts](#c-script-projects) festgelegt haben.

In diesem Artikel stehen Beispiele derzeit nur für JavaScript- (Node.js) und C#-Klassenbibliotheksfunktionen zur Verfügung.  

Dieser Artikel erläutert detailliert, wie Sie die Azure Functions-Erweiterung verwenden, um Funktionen zu entwickeln und in Azure zu veröffentlichen. Bevor Sie diesen Artikel lesen, sollten Sie [Ihre erste Funktion mit Visual Studio Code erstellen](functions-create-first-function-vs-code.md).

> [!IMPORTANT]
> Kombinieren Sie die lokale Entwicklung und die Portalentwicklung nicht in der gleichen Funktions-App. Bei der Veröffentlichung aus einem lokalen Projekt in einer Funktions-App überschreibt der Bereitstellungsprozess alle Funktionen, die Sie im Portal entwickelt haben.

## <a name="prerequisites"></a>Voraussetzungen

Zum Installieren und Ausführen der [Azure Functions-Erweiterung][Azure Functions-Erweiterung für Visual Studio Code] müssen die folgenden Anforderungen erfüllt werden:

* [Visual Studio Code](https://code.visualstudio.com/) auf einer der [unterstützten Plattformen](https://code.visualstudio.com/docs/supporting/requirements#_platforms) installiert.

* Ein aktives Azure-Abonnement.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

Weitere Ressourcen, die Sie benötigen, wie beispielsweise ein Azure Storage-Konto, werden in Ihrem Abonnement erstellt, wenn Sie [mithilfe von Visual Studio Code veröffentlichen](#publish-to-azure).

> [!IMPORTANT]
> Sie können Funktionen lokal entwickeln und sie auf Azure veröffentlichen, ohne sie lokal starten und ausführen zu müssen. Es gelten zusätzliche Anforderungen für das lokale Ausführen Ihrer Funktionen, darunter ein automatischer Download der Azure Functions Core Tools. Weitere Informationen finden Sie unter [Zusätzliche Anforderungen für die lokale Ausführung](#additional-requirements-to-run-locally). 

[!INCLUDE [functions-install-vs-code-extension](../../includes/functions-install-vs-code-extension.md)]

## <a name="create-an-azure-functions-project"></a>Erstellen eines Azure Functions-Projekts

Mithilfe der Functions-Erweiterung können Sie zugleich mit Ihrer ersten Funktion ein Funktions-App-Projekt erstellen. Die folgenden Schritte zeigen, wie Sie eine per HTTP ausgelöste Funktion in einem neuen Funktionsprojekt erstellen. [HTTP-Trigger](functions-bindings-http-webhook.md) ist die einfachste Funktionstriggervorlage und eignet sich daher gut zur Veranschaulichung.

1. Wählen Sie in **Azure Functions** das Symbol zum Erstellen einer Funktion aus.

    ![Erstellen einer Funktion](./media/functions-develop-vs-code/create-function.png)

1. Wählen Sie den Ordner für Ihr Funktions-App-Projekt und dann **wählen Sie eine Sprache für Ihr Funktions-App-Projekt** aus. 

1. Wählen Sie die Funktionsvorlage **HTTP-Trigger** aus, Sie können sich aber auch alternativ für **Vorerst überspringen** entscheiden, um ein Projekt ohne Funktion zu erstellen. Sie können später jederzeit [eine Funktion zu Ihrem Projekt hinzufügen](#add-a-function-to-your-project). 

    ![Auswählen der Vorlage für den HTTP-Trigger](./media/functions-develop-vs-code/create-function-choose-template.png)

1. Geben Sie als Funktionsname `HTTPTrigger` ein, und drücken Sie die EINGABETASTE. Wählen Sie anschließend die Autorisierung **Funktion** aus. Für diese Autorisierung müssen Sie beim Aufrufen des Funktionsendpunkts eine [Funktionstaste](functions-bindings-http-webhook.md#authorization-keys) angeben.

    ![Funktionsauthentifizierung auswählen](./media/functions-develop-vs-code/create-function-auth.png)

    Eine Funktion wird in Ihrer gewählten Sprache anhand der Vorlage für eine über HTTP ausgelöste Funktion erstellt.

    ![Vorlage für eine über HTTP ausgelöst Funktion in Visual Studio Code](./media/functions-develop-vs-code/new-function-full.png)

Die Projektvorlage erstellt ein Projekt in Ihrer gewählten Sprache und installiert die erforderlichen Abhängigkeiten. Für jede Sprache weist das neue Projekt die folgenden Dateien auf:

* **host.json**: Ermöglicht das Konfigurieren des Functions-Hosts. Diese Einstellungen gelten für die lokale Ausführung und die Ausführung in Azure. Weitere Informationen finden Sie in der [host.json-Referenz](functions-host-json.md).

* **local.settings.json**: Behält Einstellungen beim lokalen Ausführen von Funktionen bei. Diese Einstellungen werden nur bei der lokalen Ausführung verwendet. Weitere Informationen finden Sie unter [Datei für lokale Einstellungen](#local-settings-file).

    >[!IMPORTANT]
    >Da die Datei „local.settings.json“ Geheimnisse enthalten kann, müssen Sie sie aus der Quellcodeverwaltung Ihres Projekts ausschließen.

An diesem Punkt können Sie Ihrer Funktion Eingabe- und Ausgabebindungen hinzufügen, indem Sie [die function.json-Datei ändern](#javascript-2) oder indem Sie [einer C#-Klassenbibliotheksfunktion einen Parameter hinzufügen](#c-class-library-2).

Ferner können Sie [Ihrem Projekt eine neue Funktion hinzufügen](#add-a-function-to-your-project).

## <a name="install-binding-extensions"></a>Installieren von Bindungserweiterungen

Mit Ausnahme von HTTP- und Timertriggern werden Bindungen in Erweiterungspaketen implementiert. Sie müssen die Erweiterungspakete im Sinne der Trigger und Bindungen, die sie benötigen, installieren. Die Weise, in der diese Bindungserweiterungen installiert werden, hängt von der Sprache Ihres Projekts ab.

### <a name="javascript"></a>JavaScript

[!INCLUDE [functions-extension-bundles](../../includes/functions-extension-bundles.md)]

### <a name="c-class-library"></a>C\#-Klassenbibliothek

Führen Sie den Befehl [dotnet add package](/dotnet/core/tools/dotnet-add-package) im Terminalfenster aus, um die Erweiterungspakete zu installieren, die für Ihr Projekt erforderlich sind. Im folgenden Beispiel wird die Azure Storage-Erweiterung installiert, die Bindungen für Blob-, Warteschlangen- und Tabellenspeicher implementiert.

```bash
dotnet add package Microsoft.Azure.WebJobs.Extensions.Storage --version 3.0.4
```

## <a name="add-a-function-to-your-project"></a>Hinzufügen einer Funktion zu Ihrem Projekt

Sie können einem Projekt eine neue Funktion hinzufügen, indem Sie eine der vordefinierten Functions-Triggervorlagen verwenden. Um einen neuen Funktionstrigger hinzuzufügen, drücken Sie die F1-Taste, um die Befehlspalette zu öffnen. Suchen Sie dann den Befehl **Azure Functions: Funktion erstellen...** , und führen Sie ihn aus. Befolgen Sie die Anweisungen, um Ihren Triggertyp auszuwählen und die erforderlichen Attribute des Triggers zu definieren. Wenn eine Zugriffstaste oder eine Verbindungszeichenfolge erforderlich ist, damit Ihr Trigger eine Verbindung mit einem Dienst herstellen kann, bereiten Sie dies vor, bevor Sie den Funktionstrigger erstellen. 

Die Ergebnisse dieses Vorgangs hängen von Ihrer Projektsprache ab:

### <a name="javascript"></a>JavaScript

Es wird ein neuer Ordner im Projekt erstellt, der eine neue function.json-Datei und die neue JavaScript-Codedatei enthält.

### <a name="c-class-library"></a>C\#-Klassenbibliothek

Eine neue C#-Klassenbibliotheksdatei (CS) wird Ihrem Projekt hinzugefügt.

## <a name="add-input-and-output-bindings"></a>Hinzufügen von Eingabe- und Ausgabebindungen

Sie können Ihre Funktion erweitern, indem Sie Eingabe- und Ausgabebindungen hinzufügen. Die Weise, in der Sie dies tun, hängt von Ihrer Projektsprache ab. Weitere Informationen zu Bindungen finden Sie unter [Azure Functions-Trigger und Bindungskonzepte](functions-triggers-bindings.md). 

Stellen Sie in den folgenden Beispielen eine Verbindung mit einer Speicherwarteschlange mit dem Namen `outqueue` her, wobei die Verbindungszeichenfolge für das Speicherkonto in der Anwendungseinstellung `MyStorageConnection` in „local.settings.json“ festgelegt wird. 

### <a name="javascript"></a>JavaScript

In Visual Studio Code können Sie Ihrer function.json-Datei Bindungen hinzufügen, indem Sie auf eine Folge komfortabler Aufforderungen reagieren. Klicken Sie zum Erstellen einer Bindung mit der rechten Maustaste (Ctrl+Mausklick in macOS) auf die Datei `function.json` in Ihrem Funktionsordner, und wählen Sie **Bindung hinzufügen...** aus. 

![Hinzufügen einer Bindung zu einer vorhandenen JavaScript-Funktion ](media/functions-develop-vs-code/function-add-binding.png)

In der Folge finden Sie Beispielaufforderungen zum Definieren einer neuen Ausgabebindung für den Speicher:

| Prompt | Wert | BESCHREIBUNG |
| -------- | ----- | ----------- |
| **Select binding direction** (Wählen Sie die Bindungsrichtung aus) | `out` | Die Bindung ist eine Ausgabebindung. |
| **Select binding with direction** (Wählen Sie die Bindung mit Richtung aus) | `Azure Queue Storage` | Die Bindung ist eine Azure Storage-Warteschlangenbindung. |
| **Der Name, der zum Identifizieren dieser Bindung in Ihrem Code verwendet wird** | `msg` | Name, der den Bindungsparameter identifiziert, auf den in Ihrem Code verwiesen wird. |
| **Die Warteschlange, an die die Nachricht gesendet wird** | `outqueue` | Der Name der Warteschlange, in den die Bindung schreibt. Wenn der *queueName* nicht vorhanden ist, erstellt die Bindung ihn bei der ersten Verwendung. |
| **Select setting from "local.setting.json"** (Wählen Sie eine Einstellung aus „local.setting.json“ aus) | `MyStorageConnection` | Der Name einer Anwendungseinstellung, die die Verbindungszeichenfolge für das Storage-Konto enthält. Die Einstellung `AzureWebJobsStorage` enthält die Verbindungszeichenfolge für das Speicherkonto, das Sie mit der Funktions-App erstellt haben. |

In diesem Beispiel wird die folgende Bindung dem Array `bindings` in Ihrer function.json-Datei hinzugefügt:

```javascript
{
    "type": "queue",
    "direction": "out",
    "name": "msg",
    "queueName": "outqueue",
    "connection": "MyStorageConnection"
}
```

Sie können die gleiche Bindungsdefinition auch direkt zu Ihrer function.json-Datei hinzufügen.

In Ihrem Funktionscode erfolgt der Zugriff auf die `msg`-Bindung über den `context`, wie im folgenden Beispiel:

```javascript
context.bindings.msg = "Name passed to the function: " req.query.name;
```

Weitere Informationen finden Sie in der Referenz zur [Queue Storage-Ausgabebindung](functions-bindings-storage-queue.md#output---javascript-example).

### <a name="c-class-library"></a>C\#-Klassenbibliothek

Aktualisieren Sie die Funktionsmethode, um der `Run`-Methodendefinition den folgenden Parameter hinzuzufügen:

```cs
[Queue("outqueue"),StorageAccount("MyStorageConnection")] ICollector<string> msg
```

Für diesen Code müssen Sie die folgende `using`-Anweisung hinzufügen:

```cs
using Microsoft.Azure.WebJobs.Extensions.Storage;
```

Der Parameter `msg` ist ein `ICollector<T>`-Typ und stellt eine Sammlung von Nachrichten dar, die in eine Ausgabebindung geschrieben werden, wenn die Funktion abgeschlossen wird. Sie fügen der Sammlung eine oder mehrere Nachrichten hinzu, die beim Abschluss der Funktion an die Warteschlange gesendet werden.

Weitere Informationen finden Sie in der Referenz zur [Queue Storage-Ausgabebindung](functions-bindings-storage-queue.md#output---c-example).

[!INCLUDE [Supported triggers and bindings](../../includes/functions-bindings.md)]

## <a name="publish-to-azure"></a>Veröffentlichen in Azure

Visual Studio Code ermöglicht die Veröffentlichung Ihres Funktionsprojekts direkt in Azure. Bei diesem Vorgang erstellen Sie eine Funktions-App und zugehörige Ressourcen in Ihrem Azure-Abonnement. Die Funktions-App bietet einen Ausführungskontext für Ihre Funktionen. Das Projekt wird verpackt und in der neuen Funktions-App in Ihrem Azure-Abonnement bereitgestellt.

Bei der Veröffentlichung über Visual Studio Code wird eine von zwei Bereitstellungsmethoden verwendet:

* [ZIP-Bereitstellung mit Aktivierung von „Run-From-Package“](functions-deployment-technologies.md#zip-deploy): für die meisten Azure Functions-Bereitstellungen verwendet.
* [Externe Paket-URL](functions-deployment-technologies.md#external-package-url): für die Bereitstellung von Linux-Apps nach einem [Verbrauchsplan](functions-scale.md#consumption-plan) verwendet.

### <a name="quick-function-app-creation"></a>Schnelle Erstellung von Funktions-Apps

Standardmäßig generiert Visual Studio Code automatisch Werte für die von Ihrer Funktions-App benötigten Azure-Ressourcen. Diese Werte basieren auf dem von Ihnen gewählten Namen der Funktions-App. Ein Beispiel für die Verwendung von Standardwerten zum Veröffentlichen Ihres Projekts in einer neuen Funktions-App in Azure finden Sie im [Schnellstartartikel für Visual Studio Code](functions-create-first-function-vs-code.md#publish-the-project-to-azure).

Wenn Sie für die erstellten Ressourcen aussagekräftige Namen bereitstellen möchten, müssen Sie die Veröffentlichung mithilfe erweiterter Optionen aktivieren.

### <a name="enabled-publishing-with-advanced-create-options"></a>Aktivierte Veröffentlichung mit erweiterten Optionen zum Erstellen

Um Ihnen Kontrolle über die mit dem Erstellen von Azure Functions-Apps einhergehenden Einstellungen zu geben, aktualisieren Sie die Azure Functions-Erweiterung, um erweiterte Einstellungen zu unterstützen.

1. Klicken Sie auf **Datei > Voreinstellungen > Einstellungen**

1. Navigieren Sie durch **Benutzereinstellungen > Erweiterungen > Azure Functions**

1. Aktivieren Sie das Kontrollkästchen für **Azure-Funktion: Advanced Creation** (Erweiterte Erstellungsoptionen)

### <a name="publish-to-a-new-function-app-in-azure-with-advanced-creation"></a>Veröffentlichen in einer neuen Funktions-App in Azure mit erweiterten Erstellungsoptionen

Mit den folgenden Schritten wird Ihr Projekt in einer neuen Funktions-App veröffentlicht, die mit erweiterten Erstellungsoptionen erstellt wurde.

1. Wählen Sie im Bereich **Azure: Funktionen** das Symbol für die Bereitstellung in der Funktions-App aus.

    ![Funktions-Anwendungseinstellungen](./media/functions-develop-vs-code/function-app-publish-project.png)

1. Wenn Sie nicht angemeldet sind, werden Sie aufgefordert, **sich bei Azure anzumelden**. Sie können auch ein **kostenloses Azure-Konto erstellen**. Nach der erfolgreichen Anmeldung aus dem Browser wechseln Sie zurück zu Visual Studio Code.

1. Wenn Sie mehrere Abonnements besitzen, **wählen Sie ein Abonnement für die Funktions-App aus**, und wählen Sie dann **+ Neue Funktions-App erstellen** aus.

1. Geben Sie gemäß der Eingabeaufforderungen die folgenden Informationen ein:

    | Prompt | Wert | BESCHREIBUNG |
    | ------ | ----- | ----------- |
    | Funktions-App in Azure auswählen | + Neue Funktions-App in Azure erstellen | Geben Sie an der nächsten Aufforderung einen eindeutigen Namen ein, der Ihre neue Funktions-App identifiziert, und drücken Sie die EINGABETASTE. Gültige Zeichen für den Namen einer Funktions-App sind `a-z`, `0-9` und `-`. |
    | Betriebssystem auswählen | Windows | Die Funktions-App wird unter Windows ausgeführt. |
    | Hostingplan auswählen | Verbrauchsplan | Es wird serverloses [Hosting mit Verbrauchsplan](functions-scale.md#consumption-plan) verwendet. |
    | Auswählen einer Runtime für Ihre neue App | Ihre Projektsprache | Die Laufzeit muss mit dem Projekt übereinstimmen, das Sie veröffentlichen. |
    | Auswählen einer Ressourcengruppe für neue Ressourcen | Erstellen einer neuen Ressourcengruppe | Geben Sie an der nächsten Aufforderung einen Ressourcengruppennamen ein, wie etwa `myResourceGroup`, und drücken Sie die EINGABETASTE. Sie können auch eine vorhandene Ressourcengruppe auswählen. |
    | Auswählen eines Speicherkontos | Erstellen eines neuen Speicherkontos | Geben Sie an der nächsten Aufforderung einen global eindeutigen Namen für das neue Speicherkonto ein, das von Ihrer Funktions-App verwendet wird, und drücken Sie die EINGABETASTE. Speicherkontonamen müssen zwischen 3 und 24 Zeichen lang sein und dürfen nur Zahlen und Kleinbuchstaben enthalten. Sie können auch ein vorhandenes Konto verwenden. |
    | Auswählen eines Speicherorts für neue Ressourcen | region | Wählen Sie einen Standort in einer [Region](https://azure.microsoft.com/regions/) in Ihrer Nähe oder in der Nähe anderer Dienste aus, auf die Ihre Funktionen zugreifen. |

    Nach der Erstellung der Funktions-App wird eine Benachrichtigung angezeigt, und das Bereitstellungspaket wird angewendet. Wählen Sie in dieser Benachrichtigungen **Ausgabe anzeigen** aus, um die Erstellungs- und Bereitstellungsergebnisse (auch für die von Ihnen erstellten Azure-Ressourcen) anzuzeigen.

## <a name="republish-project-files"></a>Erneutes Veröffentlichen von Projektdateien

Wenn Sie [Continuous Deployment](functions-continuous-deployment.md) einrichten, wird Ihre Funktions-App in Azure immer dann aktualisiert, wenn die Quelldateien am verbundenen Quellspeicherort aktualisiert werden. Wir empfehlen diese Entwicklungspraxis zwar, Sie können aber Ihre Projektdateiaktualisierungen auch erneut aus Visual Studio Code veröffentlichen. 

> [!IMPORTANT]
> Beim Veröffentlichen in einer vorhandenen Funktions-App wird der Inhalt dieser App in Azure überschrieben.

1. Drücken Sie in Visual Studio Code die F1-Taste, um die Befehlspalette zu öffnen. Suchen Sie in der Befehlspalette den Befehl `Azure Functions: Deploy to function app...`, und wählen Sie ihn aus.

1. Wenn Sie nicht angemeldet sind, werden Sie aufgefordert, **sich bei Azure anzumelden**. Nach der erfolgreichen Anmeldung aus dem Browser wechseln Sie zurück zu Visual Studio Code. Wenn Sie über mehrere Abonnements verfügen, **Wählen Sie ein Abonnement aus**, das Ihre Funktions-App enthält.

1. Wählen Sie Ihre vorhandene Funktions-App in Azure aus. Wenn Sie wegen des Überschreibens aller Dateien in der Funktions-App gewarnt werden, wählen Sie **Bereitstellen** aus, um die Warnung zu bestätigen und fortzufahren. 

Das Projekt wird neu erstellt, neu verpackt und in Azure hochgeladen. Das vorhandene Projekt wird durch das neue Paket ersetzt, und die Funktions-App wird neu gestartet.

## <a name="get-deployed-function-url"></a>Abrufen der URL einer bereitgestellten Funktion

Um eine per HTTP ausgelöste Funktion aufrufen zu können, benötigen Sie die URL der Funktion, wenn sie in Ihrer Funktions-App bereitgestellt ist. Diese URL beinhaltet alle erforderlichen [Funktionstasten](functions-bindings-http-webhook.md#authorization-keys). Sie können die Erweiterung verwenden, um diese URLs für Ihre bereitgestellten Funktionen abzurufen.

1. Drücken Sie die F1-Taste, um die Befehlspalette zu öffnen. Suchen Sie dann den Befehl **Azure Functions: Funktions-URL kopieren**, und führen Sie ihn aus.

1. Folgen Sie den Aufforderungen, um Ihre Funktions-App in Azure und dann den spezifischen HTTP-Auslöser auszuwählen, den Sie aufrufen möchten. 

Die Funktions-URL wird in die Zwischenablage kopiert, zusammen mit allen erforderlichen Schlüsseln, die mithilfe des Abfrageparameters `code` übergeben werden. Verwenden Sie ein HTTP-Tool, um POST-Anforderungen zu senden, oder einen Browser für GET-Anforderungen an die Remotefunktion.  

## <a name="run-functions-locally"></a>Lokales Ausführen von Funktionen

Mithilfe der Azure Functions-Erweiterung können Sie ein Funktionsprojekt auf Ihrem lokalen Entwicklungscomputer ausführen. Die lokale-Runtime ist die gleiche Runtime, die Ihre Funktions-App in Azure hostet. Lokale Einstellungen werden aus der [local.settings.json-Datei](#local-settings-file) gelesen.

### <a name="additional-requirements-to-run-locally"></a>Weitere Anforderungen für die lokale Ausführung

Um Ihr Functions-Projekt lokal ausführen zu können, müssen Sie außerdem diese zusätzlichen Anforderungen erfüllen:

* Installieren Sie die Version 2.x von [Azure Functions Core Tools](functions-run-local.md#v2). Das Core Tools-Paket wird für Sie automatisch heruntergeladen und installiert, wenn Sie das Projekt lokal starten. Die Core Tools beinhalten die gesamte Azure Functions-Runtime, daher können Download und Installation einige Zeit in Anspruch nehmen.

* Installieren Sie die erforderlichen Komponenten für die ausgewählte Sprache:

    | Sprache | Anforderung |
    | -------- | --------- |
    | **C#** | [C#-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)<br/>[.NET Core CLI-Tools](https://docs.microsoft.com/dotnet/core/tools/?tabs=netcore2x)   |
    | **Java** | [Debugger für Java-Erweiterung](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-debug)<br/>[Java 8](https://aka.ms/azure-jdks)<br/>[Maven 3+](https://maven.apache.org/) |
    | **JavaScript** | [Node.js](https://nodejs.org/)<sup>*</sup> |  
    | **Python** | [Python-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-python.python)<br/>[Python 3.6+](https://www.python.org/downloads/)|

    <sup>*</sup>Active LTS- und Maintenance LTS-Versionen (8.11.1 und 10.14.1 empfohlen).

### <a name="configure-the-project-to-run-locally"></a>Konfigurieren des Projekts für die lokale Ausführung

Die Functions-Runtime verwendet intern ein Azure Storage-Konto für alle Triggertypen außer HTTP und Webhooks. Dies bedeutet, dass Sie den Schlüssel **Values.AzureWebJobsStorage** auf die Verbindungszeichenfolge eines gültigen Azure Storage-Kontos festlegen müssen.

In diesem Abschnitt wird die [Azure Storage-Erweiterung für Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurestorage) mit [Microsoft Azure Storage-Explorer](https://storageexplorer.com/) verwendet, um eine Verbindung mit dem Speicher herzustellen und seine Verbindungszeichenfolge abzurufen.   

So legen Sie die Speicherkonto-Verbindungszeichenfolge fest:

1. Öffnen Sie in Visual Studio den **Cloud-Explorer**, erweitern Sie **Speicherkonto** > **Ihr Speicherkonto**, wählen Sie **Eigenschaften** aus, und kopieren Sie dann den Wert von **Primäre Verbindungszeichenfolge**.

2. Öffnen Sie in Ihrem Projekt die Datei „local.settings.json“, und legen Sie den Wert des Schlüssels **AzureWebJobsStorage** auf die kopierte Verbindungszeichenfolge fest.

3. Wiederholen Sie den vorherigen Schritt zum Hinzufügen von eindeutigen Schlüsseln zum **Values**-Array für alle anderen Verbindungen, die von Ihren Funktionen benötigt werden.

Weitere Informationen finden Sie unter [Datei für lokale Einstellungen](#local-settings-file).

### <a name="debugging-functions-locally"></a>Lokales Debuggen von Funktionen  

Drücken Sie F5, um Ihre Funktionen zu debuggen. Wenn Sie die [Core Tools][Azure Functions Core Tools] noch nicht heruntergeladen haben, werden Sie jetzt dazu aufgefordert. Wenn Core Tools installiert sind und ausgeführt werden, wird die Ausgabe im Terminal angezeigt. Dies entspricht dem Ausführen des Core Tools-Befehls `func host start` im Terminal, jedoch mit zusätzlichen Buildaufgaben und einem angefügten Debugger.  

Bei ausgeführtem Projekt können Sie Ihre Funktionen auslösen, wie Sie das auch bei der Bereitstellung in Azure tun würden. Bei der Ausführung im Debugmodus werden in Visual Studio Code erwartungsgemäß Haltepunkte erreicht.

Die Anforderungs-URL für HTTP-Trigger wird in der Ausgabe im Terminal angezeigt. Bei der lokalen Ausführung werden keine Funktionstasten für HTTP-Trigger verwendet. Weitere Informationen finden Sie unter [Strategien zum Testen Ihres Codes in Azure Functions](functions-test-a-function.md).  

Weitere Informationen finden Sie unter [Arbeiten mit Azure Functions Core Tools][Azure Functions Core Tools].

[!INCLUDE [functions-local-settings-file](../../includes/functions-local-settings-file.md)]

Standardmäßig werden diese Einstellungen nicht automatisch migriert, wenn das Projekt in Azure veröffentlicht wird. Nachdem die Veröffentlichung abgeschlossen ist, erhalten Sie die Option, Einstellungen aus „local.settings.json“ in Ihrer Funktions-App in Azure zu veröffentlichen. Weitere Informationen finden Sie unter [Veröffentlichen von Anwendungseinstellungen](#publish-application-settings).

Werte in **ConnectionStrings** werden niemals veröffentlicht.

Die Werte für Funktionsanwendungseinstellungen können auch im Code als Umgebungsvariablen gelesen werden. Weitere Informationen finden Sie im Abschnitt „Umgebungsvariablen“ dieser sprachspezifischen Referenzartikel:

* [Vorkompilierter C#-Code](functions-dotnet-class-library.md#environment-variables)
* [C#-Skript (.csx)](functions-reference-csharp.md#environment-variables)
* [Java](functions-reference-java.md#environment-variables)
* [JavaScript](functions-reference-node.md#environment-variables)

## <a name="application-settings-in-azure"></a>Anwendungseinstellungen in Azure

Die Einstellungen in der Datei „local.settings.json“ in Ihrem Projekt sollten die gleichen wie die Anwendungseinstellungen in der Funktions-App in Azure sein. Alle Einstellungen, die Sie „local.settings.json“ hinzugefügt haben, müssen auch der Funktions-App in Azure hinzugefügt werden. Diese Einstellungen werden nicht automatisch hochgeladen, wenn Sie das Projekt veröffentlichen. Analog müssen alle Einstellungen, die Sie in Ihrer Funktions-App [im Portal](functions-how-to-use-azure-function-app-settings.md#settings) erstellen, in Ihr lokales Projekt heruntergeladen werden.

### <a name="publish-application-settings"></a>Veröffentlichen von Anwendungseinstellungen

Die einfachste Möglichkeit zum Veröffentlichen der erforderlichen Einstellungen in Ihre Funktions-App in Azure ist die Verwendung des Links **Einstellungen hochladen**, der angezeigt wird, nachdem Sie das Projekt veröffentlicht haben.

![Hochladen von Anwendungseinstellungen nach abgeschlossener Bereitstellung](./media/functions-develop-vs-code/upload-app-settings.png)

Sie können Einstelllungen auch mithilfe des Befehls `Azure Functions: Upload Local Setting` in der Befehlspalette veröffentlichen. Individuelle Einstellungen werden den Anwendungseinstellungen in Azure mithilfe des Befehls `Azure Functions: Add New Setting...` hinzugefügt. 

> [!TIP]
> Achten Sie darauf, Ihre local.settings.json-Datei zu speichern, bevor Sie sie veröffentlichen.

Wenn die lokale Datei verschlüsselt ist, wird sie entschlüsselt, veröffentlicht und erneut verschlüsselt. Wenn an beiden Speicherorten Einstellungen mit verschiedenen Werten vorhanden sind, werden Sie gefragt, wie weiter vorgegangen werden soll.

Anzeigen der vorhandenen App-Einstellungen im Bereich **Azure: Functions** durch Aufklappen Ihres Abonnements, Ihrer Funktions-App und der **Anwendungseinstellungen**.

![Anzeigen der Funktions-App-Einstellung in Visual Studio Code](./media/functions-develop-vs-code/view-app-settings.png)

### <a name="download-settings-from-azure"></a>Herunterladen der Einstellungen von Azure

Wenn Sie Anwendungseinstellungen in Azure erstellt haben, können Sie sie in Ihre local.settings.json-Datei herunterladen. Verwenden Sie dazu den Befehl `Azure Functions: Download Remote Settings...`. 

Wie beim Hochladen wird die lokale Datei, wenn sie verschlüsselt ist, entschlüsselt, veröffentlicht und erneut verschlüsselt. Wenn an beiden Speicherorten Einstellungen mit verschiedenen Werten vorhanden sind, werden Sie gefragt, wie weiter vorgegangen werden soll.

## <a name="monitoring-functions"></a>Überwachen von Funktionen

Bei der [lokalen Ausführung](#run-functions-locally) werden Protokolldaten an die Terminalkonsole gestreamt. Sie können Protokolldaten auch abrufen, wenn Ihr Funktions-Projekt in einer Funktions-App in Azure ausgeführt wird. Sie können entweder eine Verbindung mit Streamingprotokollen in Azure herstellen, um Protokolldaten nahezu in Echtzeit anzuzeigen, oder Application Insights aktivieren, um ein vollständigeres Bild vom Verhalten Ihrer Funktions-App zu erhalten.

### <a name="streaming-logs"></a>Streamingprotokolle

Beim Entwickeln einer Anwendung ist es häufig nützlich, Protokollinformationen nahezu in Echtzeit zu sehen. Sie können einen Datenstrom von Protokolldateien anzeigen, die von Ihren Funktionen generiert werden. Die folgende Ausgabe ist ein Beispiel für Streamingprotokolle für eine Anforderung an eine per HTTP ausgelöste Funktion:

![Streamingprotokollausgabe für HTTP-Trigger](media/functions-develop-vs-code/streaming-logs-vscode-console.png) 

Weitere Informationen finden Sie unter [Streamingprotokolle](functions-monitoring.md#streaming-logs). 

[!INCLUDE [functions-enable-log-stream-vs-code](../../includes/functions-enable-log-stream-vs-code.md)]

> [!NOTE]
> Streamingprotokolle unterstützen nur eine einzige Instanz des Azure Functions-Hosts. Wenn Ihre Funktion auf mehrere Instanzen skaliert wird, werden Daten aus anderen Instanzen nicht im Protokollstream angezeigt. Der [Live Metrics Stream](../azure-monitor/app/live-stream.md) in Application Insights unterstützt mehrere Instanzen. Obwohl auch in nahezu Echtzeit basiert die Streamanalyse auch auf [Stichprobendaten](functions-monitoring.md#configure-sampling).

### <a name="application-insights"></a>Application Insights

Die empfohlene Methode zum Überwachen der Ausführung Ihrer Funktionen ist die Integration Ihrer Funktions-App in Azure Application Insights. Wenn Sie eine Funktions-App im Azure-Portal erstellen, wird diese Integration standardmäßig für Sie erledigt. Wenn Sie Ihre Funktions-App während der Veröffentlichung in Visual Studio erstellen, erfolgt die Integration Ihrer Funktions-App in Azure nicht.

[!INCLUDE [functions-connect-new-app-insights.md](../../includes/functions-connect-new-app-insights.md)]

Weitere Informationen finden Sie unter [Überwachen von Azure Functions](functions-monitoring.md).

## <a name="c-script-projects"></a>C\#-Skriptprojekte

Standardmäßig werden alle C#-Projekte als [C#-kompilierte Klassenbibliotheksprojekte](functions-dotnet-class-library.md) erstellt. Wenn Sie stattdessen die Arbeit mit C#-Skriptprojekten vorziehen, müssen Sie C#-Skript als Standardsprache in den Einstellungen der Azure Functions-Erweiterung auswählen.

1. Klicken Sie auf **Datei > Voreinstellungen > Einstellungen**.

1. Navigieren Sie durch **Benutzereinstellungen > Erweiterungen > Azure Functions**.

1. Wählen Sie **C#-Skript** in **Azure-Funktion: Projektsprache** aus.

An diesem Punkt enthalten Aufrufe an die zugrunde liegenden Core Tools die Option `--csx`, die C#-Skript-Projektdateien (CSX) generiert und veröffentlicht. Mit angegebener Standardsprache sind alle erstellten Projekte standardmäßig C#-Skriptprojekte. Sie werden nicht zur Auswahl einer Projektsprache aufgefordert, wenn eine Standardsprache festgelegt ist. Um Projekte in anderen Sprachen zu erstellen, müssen Sie diese Einstellung ändern oder sie aus der settings.json-Datei des Benutzers entfernen. Nachdem Sie diese Einstellung entfernt haben, werden Sie beim Erstellen eines Projekts wieder aufgefordert, Ihre Sprache auszuwählen.

## <a name="command-palette-reference"></a>Befehlspalettenreferenz

Die Azure Functions-Erweiterung bietet im Azure-Bereich eine nützliche grafische Oberfläche für die Interaktion mit Ihren Funktions-Apps in Azure. Die gleiche Funktionalität steht auch in Form von Befehlen in der Befehlspalette (F1) zur Verfügung. Die folgenden Azure Functions-spezifischen Befehle sind verfügbar:

|Azure Functions-Befehl  | BESCHREIBUNG  |
|---------|---------|
|**Neue Einstellungen hinzufügen...**  |  Erstellt eine neue Anwendungseinstellung in Azure. Weitere Informationen finden Sie unter [Veröffentlichen von Anwendungseinstellungen](#publish-application-settings). Möglicherweise müssen Sie [diese Einstellung in Ihre lokalen Einstellungen herunterladen](#download-settings-from-azure). |
| **Konfigurieren der Bereitstellungsquelle...** | Stellen Sie eine Verbindung Ihrer Funktions-App in Azure mit einem lokalen Git-Repository her. Weitere Informationen finden Sie unter [Continuous Deployment für Azure Functions](functions-continuous-deployment.md). |
| **Mit GitHub Repository verbinden...** | Stellen Sie eine Verbindung Ihrer Funktions-App mit einem GitHub-Repository her. |
| **Kopieren der Funktions-URL** | Ruft die Remote-URL einer per HTTP ausgelösten Funktion ab, die in Azure ausgeführt wird. Weitere Informationen finden Sie in der Vorgehensweise zum [Abrufen der URL einer bereitgestellten Funktion](#get-deployed-function-url). |
| **Funktions-App in Azure erstellen...** | Erstellt eine neue Funktions-App in Ihrem Abonnement in Azure. Weitere Informationen finden Sie in der Vorgehensweise zum [Veröffentlichen in einer neuen Funktions-App in Azure](#publish-to-azure).        |
| **Einstellungen entschlüsseln** | Verwenden Sie diesen Befehl zum Entschlüsseln von [lokalen Einstellungen](#local-settings-file), die mithilfe von `Azure Functions: Encrypt Settings` verschlüsselt wurden.  |
| **Funktions-App löschen...** | Entfernt eine vorhandene Funktions-App aus Ihrem Abonnement in Azure. Wenn keine anderen Apps im App Service-Plan vorhanden sind, wird Ihnen die Option eingeräumt, diesen ebenfalls zu löschen. Andere Ressourcen, wie etwa Speicherkonten und Ressourcengruppen, werden nicht gelöscht. Um alle Ressourcen zu entfernen, sollten Sie stattdessen [die Ressourcengruppe löschen](functions-add-output-binding-storage-queue-vs-code.md#clean-up-resources). Ihr lokales Projekt ist nicht betroffen. |
|**Funktion löschen...**  | Entfernt eine vorhandene Funktion aus einer Funktions-App in Azure. Da sich diese Löschung nicht auf Ihr lokales Projekt auswirkt, ziehen Sie in Erwägung, stattdessen die Funktion lokal zu entfernen und dann [das Projekt neu zu veröffentlichen](#republish-project-files). |
| **Proxy löschen...** | Entfernt einen Azure Functions-Proxy aus Ihrer Funktions-App in Azure. Weitere Informationen über Proxys finden Sie unter [Arbeiten mit Azure Functions-Proxys](functions-proxies.md). |
| **Einstellung löschen...** | Löscht eine Einstellung einer Funktionsanwendung in Azure. Wirkt sich nicht auf Einstellungen in Ihrer local.settings.json-Datei aus. |
| **Vom Repository trennen...**  | Entfernt die Verbindung für [Continuous Deployment](functions-continuous-deployment.md) zwischen einer Funktions-App in Azure und einem Repository der Quellcodeverwaltung. |
| **Remoteeinstellungen herunterladen...** | Lädt Einstellungen aus der gewählten Funktions-App in Azure in Ihre local.settings.json-Datei herunter. Wenn die lokale Datei verschlüsselt ist, wird sie entschlüsselt, aktualisiert und erneut verschlüsselt. Wenn an beiden Speicherorten Einstellungen mit verschiedenen Werten vorhanden sind, werden Sie gefragt, wie weiter vorgegangen werden soll. Achten Sie darauf, Ihre local.settings.json-Datei zu speichern, bevor Sie diesen Befehl ausführen. |
| **Einstellungen bearbeiten...** | Ändert den Wert einer vorhandenen Funktionsanwendungseinstellung in Azure. Wirkt sich nicht auf Einstellungen in Ihrer local.settings.json-Datei aus.  |
| **Einstellungen verschlüsseln** | Verschlüsselt einzelne Elemente im `Values`-Array in den [lokalen Einstellungen](#local-settings-file). In dieser Datei wird außerdem `IsEncrypted` auf `true` festgelegt, was die lokale Runtime anweist, die Einstellungen vor dem Anwenden zu entschlüsseln. Verschlüsseln Sie lokale Einstellungen, um die Gefahr des Verlusts von wertvollen Informationen zu verringern. In Azure werden Anwendungseinstellungen immer verschlüsselt gespeichert. |
| **Funktion jetzt ausführen** | Startet eine [von einem Zeitgeber ausgelöste Funktion](functions-bindings-timer.md) in Azure zu Testzwecken manuell. Weitere Informationen zum Auslösen von Nicht-HTTP-Funktionen in Azure finden Sie unter [Manuelles Ausführen einer nicht per HTTP ausgelösten Funktion](functions-manually-run-non-http.md). |
| **Projekt für die Verwendung mit VS Code initialisieren...** | Fügt einem vorhandenen Functions-Projekt die erforderlichen Visual Studio Code-Projektdateien hinzu. Verwenden Sie diesen Befehl, um mit einem Projekt zu arbeiten, das Sie mithilfe von Core Tools erstellt haben. |
| **Azure Functions Core Tools installieren oder aktualisieren** | Installiert oder aktualisiert die [Azure Functions Core Tools], die für die lokale Ausführung verwendet werden. |
| **Erneute Bereitstellung**  | Ermöglicht die erneute Bereitstellung von Projektdateien aus einem verbundenen Git-Repository in einer bestimmten Bereitstellung in Azure. Um lokale Updates aus Visual Studio Code erneut zu veröffentlichen, [veröffentlichen Sie Ihr Projekt erneut](#republish-project-files). |
| **Einstellungen umbenennen...** | Ändert den Schlüsselnamen einer vorhandenen Funktionsanwendungseinstellung in Azure. Wirkt sich nicht auf Einstellungen in Ihrer local.settings.json-Datei aus. Nach dem Umbenennen von Einstellungen in Azure sollten Sie [diese Änderungen in das lokale Projekt herunterladen](#download-settings-from-azure). |
| **Neu starten** | Führt einen Neustart der Funktions-App in Azure aus. Beim Bereitstellen von Updates wird ebenfalls ein Neustart der Funktions-App durchgeführt. |
| **AzureWebJobStorage festlegen...**| Legt den Wert der Anwendungseinstellung `AzureWebJobStorage` fest. Diese Einstellung ist für Azure-Funktionen erforderlich und wird festgelegt, wenn die Funktions-App in Azure erstellt wird. |
| **Starten** | Startet eine beendete Funktions-App in Azure. | 
| **Streamingprotokolle starten** | Startet die Streamingprotokolle für die Funktions-App in Azure. Verwenden Sie Streamingprotokolle während der Remoteproblembehandlung in Azure, wenn Sie diese Informationen nahezu in Echtzeit anzeigen müssen. Weitere Informationen finden Sie unter [Streamingprotokolle](#streaming-logs). |
| **Beenden** | Fährt eine in Azure ausgeführte Funktions-App herunter. |
| **Streamingprotokolle beenden** | Beendet die Streamingprotokolle für die Funktions-App in Azure. |
| **Toggle as Slot Setting** (Als Sloteinstellung umschalten) | Stellt bei Aktivierung sicher, dass eine Anwendungseinstellung für einen bestimmten Bereitstellungsslot erhalten bleibt. |
| **Azure Functions Core Tools deinstallieren** | Entfernt die Azure Functions Core Tools, was für die Erweiterung erforderlich ist. |
| **Lokale Einstellungen hochladen...** | Lädt Einstellungen aus Ihrer local.settings.json-Datei in die ausgewählte Funktions-App in Azure hoch. Wenn die lokale Datei verschlüsselt ist, wird sie entschlüsselt, hochgeladen und erneut verschlüsselt. Wenn an beiden Speicherorten Einstellungen mit verschiedenen Werten vorhanden sind, werden Sie gefragt, wie weiter vorgegangen werden soll. Achten Sie darauf, Ihre local.settings.json-Datei zu speichern, bevor Sie diesen Befehl ausführen. |
| **Commit in GitHub anzeigen** | Zeigt Ihnen das aktuellste Commit in einer bestimmtem Bereitstellung, wenn Ihre Funktions-App mit einem Repository verbunden ist. |
| **Bereitstellungsprotokolle anzeigen** | Zeigt die Protokolle für eine bestimmte Bereitstellung in der Funktions-App in Azure an. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure Functions Core-Tools finden Sie unter [Lokales Codieren und Testen von Azure-Funktionen](functions-run-local.md).

Weitere Informationen zum Entwickeln von Funktionen als .NET-Klassenbibliotheken finden Sie unter [C#-Entwicklerreferenz zu Azure Functions](functions-dotnet-class-library.md). Dieser Artikel enthält auch Links zu Beispielen für die Verwendung von Attributen zum Deklarieren der verschiedenen Typen von Bindungen, die von Azure Functions unterstützt werden.    

[Azure Functions-Erweiterung für Visual Studio Code]: https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions
[Azure Functions Core Tools]: functions-run-local.md