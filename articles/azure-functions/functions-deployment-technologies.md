---
title: Bereitstellungstechnologien in Azure Functions | Microsoft-Dokumentation
description: Hier werden die verschiedenen Methoden erläutert, mit denen Sie Code in Azure Functions bereitstellen können.
services: functions
documentationcenter: .net
author: ColbyTresness
manager: dariac
ms.service: azure-functions
ms.custom: vs-azure
ms.topic: conceptual
ms.date: 04/25/2019
ms.author: cotresne
ms.openlocfilehash: 118daf02ab59646f2926071763aa4d7e97846e04
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2019
ms.locfileid: "67508219"
---
# <a name="deployment-technologies-in-azure-functions"></a>Bereitstellungstechnologien in Azure Functions

Ihnen stehen verschiedene Technologien zur Verfügung, um Code von Azure Functions-Projekten in Azure bereitzustellen. In diesem Artikel finden Sie eine umfassende Liste dieser Technologien, und Sie erfahren, welche Technologien für welche Varianten von Functions verfügbar sind, was bei Verwendung der jeweiligen Methode genau passiert und welche Methoden für welche Szenarien am besten geeignet sind. Die verschiedenen Tools, die Sie bei der Bereitstellung in Azure Functions unterstützen, werden jeweils basierend auf ihrem Kontext auf die passende Technologie abgestimmt.

## <a name="deployment-technology-availability"></a>Bereitstellungstechnologie: Verfügbarkeit

> [!IMPORTANT]
> Azure Functions unterstützt plattformübergreifendes Entwickeln sowie Hosting unter zwei Betriebssystemen: Windows und Linux. Aktuell stehen drei Hostingpläne mit jeweils unterschiedlichem Verhalten zur Verfügung: [Verbrauch](functions-scale.md#consumption-plan), [Premium](functions-scale.md#premium-plan) und [Dediziert (App Service)](functions-scale.md#app-service-plan). Nicht alle Bereitstellungstechnologien stehen für jede Variante von Azure Functions zur Verfügung.

| Bereitstellungstechnologie | Windows: Verbrauch | Windows: Premium (Vorschauversion) | Windows: Dediziert  | Linux: Verbrauch (Vorschauversion) | Linux: Dediziert |
|-----------------------|:-------------------:|:-------------------------:|:-----------------:|:---------------------------:|:---------------:|
| Externe Paket-URL<sup>1</sup> |✔|✔|✔|✔|✔|
| ZIP-Bereitstellung |✔|✔|✔| |✔|
| Docker-Container | | | | |✔|
| Web Deploy |✔|✔|✔| | |
| Quellcodeverwaltung |✔|✔|✔| |✔|
| Lokales Git<sup>1</sup> |✔|✔|✔| |✔|
| Cloudsynchronisierung<sup>1</sup> |✔|✔|✔| |✔|
| FTP<sup>1</sup> |✔|✔|✔| |✔|
| Portalbearbeitung |✔|✔|✔| |✔<sup>2</sup>|

<sup>1</sup> Bereitstellungstechnologie, die eine [manuelle Triggersynchronisierung](#trigger-syncing) erfordert.
<sup>2</sup> Die Portalbearbeitung steht nur für HTTP-Trigger und Trigger mit Timer zur Verfügung (für Functions unter Linux im Plan „Dediziert“).

## <a name="key-concepts"></a>Wichtige Begriffe

In diesem Abschnitt werden zunächst einige wichtige Konzepte erläutert, die zum Verständnis der Funktionsweise von Bereitstellungen in Azure Functions bekannt sein müssen.

### <a name="trigger-syncing"></a>Triggersynchronisierung

Wenn Sie einen Trigger ändern, muss die Infrastruktur von Functions über die vorgenommenen Änderungen informiert werden. Diese Synchronisierung erfolgt bei vielen Bereitstellungstechnologien automatisch. Manchmal müssen die Trigger jedoch manuell synchronisiert werden. Eine manuelle Synchronisierung der Trigger ist erforderlich, wenn Sie Ihre Aktualisierungen über eine externe Paket-URL oder ein lokales Git oder per Cloudsynchronisierung oder FTP bereitstellen. Trigger können auf drei Arten synchronisiert werden:

* Neustarten der Funktions-App über das Azure-Portal
* Senden einer HTTP POST-Anforderung an `https://{functionappname}.azurewebsites.net/admin/host/synctriggers?code=<API_KEY>` unter Verwendung des [Hauptschlüssels](functions-bindings-http-webhook.md#authorization-keys)
* Senden einer HTTP POST-Anforderung an `https://management.azure.com/subscriptions/<SUBSCRIPTION_ID>/resourceGroups/<RESOURCE_GROUP_NAME>/providers/Microsoft.Web/sites/<FUNCTION_APP_NAME>/syncfunctiontriggers?api-version=2016-08-01` Ersetzen Sie die Platzhalter durch Ihre Abonnement-ID, den Namen Ihrer Ressourcengruppe und den Namen Ihrer Funktions-App.

## <a name="deployment-technology-details"></a>Bereitstellungstechnologie: Details  

Folgende Bereitstellungsmethoden werden von Azure Functions unterstützt:

### <a name="external-package-url"></a>Externe Paket-URL

Bei dieser Methode können Sie auf eine Remotepaketdatei (ZIP-Datei) verweisen, die Ihre Funktions-App enthält. Die Datei wird von der angegebenen URL heruntergeladen, und die App wird im Modus [Aus Paketdatei ausführen](run-functions-from-deployment-package.md) ausgeführt.

>__Verwendung:__ Fügen Sie Ihren Anwendungseinstellungen `WEBSITE_RUN_FROM_PACKAGE` hinzu. Der Wert dieser Einstellung muss eine URL sein (der Speicherort der spezifischen Paketdatei, die Sie ausführen möchten). Einstellungen können entweder [über das Portal](functions-how-to-use-azure-function-app-settings.md#settings) oder [mithilfe der Azure-Befehlszeilenschnittstelle](/cli/azure/functionapp/config/appsettings#az-functionapp-config-appsettings-set) hinzugefügt werden. Bei Verwendung von Blob Storage muss ein privater Container mit einer [Shared Access Signature (SAS)](../vs-azure-tools-storage-manage-with-storage-explorer.md#generate-a-sas-in-storage-explorer) verwendet werden, damit Functions auf das Paket zugreifen kann. Da bei jedem Neustart der Anwendung eine Kopie des Inhalts abgerufen wird, muss der Verweis für die gesamte Lebensdauer der Anwendung gültig sein.

>__Einsatzgebiete:__ Dies ist die einzige Bereitstellungsmethode, die für Azure Functions unter Linux im Plan „Verbrauch (Vorschauversion)“ unterstützt wird. Wenn Sie die Paketdatei aktualisieren, auf die eine Funktions-App verweist, müssen Sie die [Trigger manuell synchronisieren](#trigger-syncing), um Azure darüber zu informieren, dass sich Ihre Anwendung geändert hat.

### <a name="zip-deploy"></a>ZIP-Bereitstellung

Bei dieser Methode können Sie eine ZIP-Datei mit Ihrer Funktions-App an Azure pushen. Optional können Sie auch festlegen, dass Ihre App im Modus [Aus Paketdatei ausführen](run-functions-from-deployment-package.md) gestartet werden soll.

>__Verwendung:__ Verwenden Sie zur Bereitstellung Ihr bevorzugtes Clienttool ([VS Code](functions-create-first-function-vs-code.md#publish-the-project-to-azure), [Visual Studio](functions-develop-vs.md#publish-to-azure) oder die [Azure-Befehlszeilenschnittstelle](functions-create-first-azure-function-azure-cli.md#deploy-the-function-app-project-to-azure)). Eine Anleitung zum manuellen Bereitstellen einer ZIP-Datei für Ihre Funktions-App finden Sie unter [Deploying from a zip file or url](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file-or-url) (Bereitstellen mithilfe einer ZIP-Datei oder URL).
>
>Bei Verwendung der ZIP-Bereitstellung können Benutzer außerdem angeben, dass ihre App im Modus [Aus Paketdatei ausführen](run-functions-from-deployment-package.md) ausgeführt werden soll, indem sie die Anwendungseinstellung `WEBSITE_RUN_FROM_PACKAGE` auf `1` festlegen. Diese Option wird empfohlen und sorgt dafür, dass Ihre Anwendungen schneller geladen werden. Bei den obigen Clienttools wird sie standardmäßig verwendet.

>__Einsatzgebiete:__ Dies ist die empfohlene Bereitstellungstechnologie für Azure Functions unter Windows sowie für Azure Functions unter Linux im Plan „Dediziert“.

### <a name="docker-container"></a>Docker-Container

Bei dieser Methode wird ein Linux-Containerimage mit Ihrer Funktions-App bereitgestellt.

>__Verwendung:__ Erstellen Sie eine Linux-Funktions-App im Plan „Dediziert“, und geben Sie das gewünschte Containerimage für die Ausführung an. Hierzu stehen zwei Möglichkeiten zur Verfügung:
>
>* Erstellen Sie eine Linux-Funktions-App unter einem App Service-Plan über das Azure-Portal. Wählen Sie unter **Veröffentlichen** die Option **Docker-Image** aus, und konfigurieren Sie den Container, indem Sie den Ort angeben, an dem das Image gehostet wird.
>* Erstellen Sie eine Linux-Funktions-App unter einem App Service-Plan mithilfe der Azure-Befehlszeilenschnittstelle. Eine entsprechende Anleitung finden Sie unter [Erstellen und Bereitstellen des benutzerdefinierten Images](functions-create-function-linux-custom-image.md#create-and-deploy-the-custom-image).
>
>Verwenden Sie zur Bereitstellung für eine vorhandene App mit einem benutzerdefinierten Container den Befehl [`func deploy`](functions-run-local.md#publish) der [Azure Functions Core Tools](functions-run-local.md).

>__Einsatzgebiete:__ Verwenden Sie diese Option, wenn Sie mehr Kontrolle über die Linux-Umgebung benötigen, in der Ihre Funktions-App ausgeführt wird. Dieser Bereitstellungsmechanismus steht nur für Functions unter Linux in einem App Service-Plan zur Verfügung.

### <a name="web-deploy-msdeploy"></a>Web Deploy (MSDeploy)

Bei dieser Methode werden Ihre Windows-Anwendungen verpackt und auf einem beliebigen IIS-Server bereitgestellt (einschließlich Ihrer Funktions-Apps unter Windows in Azure).

>__Verwendung:__ Verwenden Sie die [Visual Studio-Tools für Azure Functions](functions-create-your-first-function-visual-studio.md), und deaktivieren Sie das Kontrollkästchen `Run from package file (recommended)`.
>
> Sie können auch [Web Deploy 3.6](https://www.iis.net/downloads/microsoft/web-deploy) herunterladen und `MSDeploy.exe` direkt aufrufen.

>__Einsatzgebiete:__ Diese Bereitstellungstechnologie wird zwar problemlos unterstützt, inzwischen wird jedoch der Mechanismus [ZIP-Bereitstellung im Modus „Aus Paketdatei ausführen“](#zip-deploy) bevorzugt. Weitere Informationen finden Sie im [Entwicklungsleitfaden für Visual Studio](functions-develop-vs.md#publish-to-azure).

### <a name="source-control"></a>Quellcodeverwaltung

Bei dieser Methode können Sie eine Verbindung zwischen Ihrer Funktions-App und einem Git-Repository herstellen, sodass bei jeder Codeaktualisierung in diesem Repository eine Bereitstellung ausgelöst wird. Weitere Informationen finden Sie im [Kudu-Wiki](https://github.com/projectkudu/kudu/wiki/VSTS-vs-Kudu-deployments).

>__Verwendung:__ Verwenden Sie das Bereitstellungscenter im Azure Functions-Portal, um die quellcodeverwaltungsbasierte Veröffentlichung einzurichten. Weitere Informationen finden Sie unter [Continuous Deployment für Azure Functions](functions-continuous-deployment.md).

>__Einsatzgebiete:__ Die Quellcodeverwaltungsoption wird für Teams empfohlen, die gemeinsam an Funktions-Apps arbeiten, und eignet sich sehr gut für komplexere Bereitstellungspipelines.

### <a name="local-git"></a>Lokales Git

Bei dieser Methode können Sie Code unter Verwendung von Git von Ihrem lokalen Computer aus an Azure Functions pushen.

>__Verwendung:__ Eine entsprechende Anleitung finden Sie unter [Lokale Git-Bereitstellung in Azure App Service](../app-service/deploy-local-git.md).

>__Einsatzgebiete:__ Im Allgemeinen werden andere Bereitstellungsmethoden empfohlen. Bei der Veröffentlichung über ein lokales Git ist eine [manuelle Triggersynchronisierung](#trigger-syncing) erforderlich.

### <a name="cloud-sync"></a>Cloudsynchronisierung

Bei dieser Methode können Sie Ihre Dropbox- und OneDrive-Inhalte mit Azure Functions synchronisieren.

>__Verwendung:__ Eine entsprechende Anleitung finden Sie unter [Synchronisieren von Inhalt aus einem Cloudordner in Azure App Service](../app-service/deploy-content-sync.md).

>__Einsatzgebiete:__ Im Allgemeinen werden andere Bereitstellungsmethoden empfohlen. Bei der Veröffentlichung mit Cloudsynchronisierung ist eine [manuelle Triggersynchronisierung](#trigger-syncing) erforderlich.

### <a name="ftp"></a>FTP

Bei dieser Methode können Sie Dateien direkt an Azure Functions übertragen.

>__Verwendung:__ Eine entsprechende Anleitung finden Sie unter [Bereitstellen der App in Azure App Service mithilfe von FTP/S](../app-service/deploy-ftp.md).

>__Einsatzgebiete:__ Im Allgemeinen werden andere Bereitstellungsmethoden empfohlen. Bei der FTP-Veröffentlichung ist eine [manuelle Triggersynchronisierung](#trigger-syncing) erforderlich.

### <a name="portal-editing"></a>Portalbearbeitung

Mit dem portalbasierten Editor können Sie Dateien direkt in Ihrer Funktions-App bearbeiten. Dadurch wird im Grunde nach jedem Klicken auf **Speichern** ein Bereitstellungsvorgang ausgeführt.

>__Verwendung:__ Wenn Sie Ihre Funktionen im Azure-Portal bearbeiten möchten, müssen [Ihre Funktionen im Portal erstellt](functions-create-first-azure-function.md) worden sein. Bei Verwendung einer anderen Bereitstellungsmethode wird Ihre Funktion schreibgeschützt und kann nicht mehr über das Portal bearbeitet werden, um eine zentrale zuverlässige Datenquelle (Single Source Of Truth, SSOT) zu gewährleisten. Sie können den Bearbeitungsmodus manuell wieder auf `Read/Write` festlegen und alle bereitstellungsbezogenen Anwendungseinstellungen (etwa `WEBSITE_RUN_FROM_PACKAGE`) entfernen, um wieder zu einem Zustand zurückzukehren, in dem Sie Ihre Dateien über das Portal bearbeiten können. 

>__Einsatzgebiete:__ Das Portal eignet sich hervorragend für die ersten Schritte mit Azure Functions. Für aufwendigere Entwicklungsarbeiten empfiehlt sich allerdings die Verwendung der Clienttools:
>
>* [Erstellen Ihrer ersten Funktion mit Visual Studio Code](functions-create-first-function-vs-code.md)
>* [Arbeiten mit Azure Functions Core Tools](functions-run-local.md)
>* [Erstellen Ihrer ersten Funktion mit Visual Studio](functions-create-your-first-function-visual-studio.md)

Die folgende Tabelle gibt Aufschluss über die Betriebssysteme und Programmiersprachen, für die die Portalbearbeitung unterstützt wird:

| | Windows: Verbrauch | Windows: Premium (Vorschauversion) | Windows: Dediziert | Linux: Verbrauch (Vorschauversion) | Linux: Dediziert |
|-|:-----------------: |:-------------------------:|:-----------------:|:---------------------------:|:---------------:|
| C# | | | | | |
| C#-Skript |✔|✔|✔| |✔<sup>*</sup>|
| F# | | | | | |
| Java | | | | | |
| JavaScript (Node.js) |✔|✔|✔| |✔<sup>*</sup>|
| Python (Vorschauversion) | | | | | |
| PowerShell (Vorschauversion) |✔|✔|✔| | |
| TypeScript (Node.js) | | | | | |

<sup>*</sup> Die Portalbearbeitung steht nur für HTTP-Trigger und Trigger mit Timer zur Verfügung (für Functions unter Linux im Plan „Dediziert“).

## <a name="deployment-slots"></a>Bereitstellungsslots

Wenn Sie Ihre Funktions-App in Azure bereitstellen, können Sie als Bereitstellungsziel einen separaten Bereitstellungsslot verwenden, anstatt die Bereitstellung direkt in der Produktionsumgebung vorzunehmen. Weitere Informationen zu Bereitstellungsslots finden Sie in der [Dokumentation zu Azure App Service-Slots](../app-service/deploy-staging-slots.md).

### <a name="deployment-slots-levels-of-support"></a>Bereitstellungsslots: Unterstützungsebenen

Es gibt zwei Unterstützungsebenen:

* _Allgemein verfügbar (generally available, GA)_ : Vollständige Unterstützung und Freigabe für die Verwendung in Produktionsumgebungen.
* _Vorschau_: Noch nicht unterstützt, die Erreichung der allgemeinen Verfügbarkeit wird aber für die Zukunft erwartet.

| Betriebssystem/Hostingplan | Unterstützungsebene |
| --------------- | ------ |
| Windows: Verbrauch | Vorschau |
| Windows: Premium (Vorschauversion) | Vorschau |
| Windows: Dediziert | Allgemein verfügbar |
| Linux: Verbrauch | Nicht unterstützt |
| Linux: Dediziert | Allgemein verfügbar |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Bereitstellen von Funktions-Apps finden Sie in den folgenden Artikeln: 

+ [Kontinuierliche Bereitstellung für Azure Functions](functions-continuous-deployment.md)
+ [Continuous Delivery mit Azure DevOps](functions-how-to-azure-devops.md)
+ [ZIP-Bereitstellung für Azure Functions](deployment-zip-push.md)
+ [Ausführen Ihrer Azure Functions aus einem Paket](run-functions-from-deployment-package.md)
+ [Automatisieren der Ressourcenbereitstellung für Ihre Funktions-App in Azure Functions](functions-infrastructure-as-code.md)
