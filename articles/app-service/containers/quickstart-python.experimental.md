---
title: 'Erstellen von Python-Apps unter Linux: Azure App Service | Microsoft-Dokumentation'
description: Stellen Sie in wenigen Minuten Ihre erste Python-App „Hallo Welt“ in Azure App Service unter Linux bereit.
services: app-service\web
documentationcenter: ''
author: cephalin
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.topic: quickstart
ms.date: 08/23/2019
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: 08b1b85b980f992e799fc5198891290ec0d55c5d
ms.sourcegitcommit: 82499878a3d2a33a02a751d6e6e3800adbfa8c13
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2019
ms.locfileid: "70071002"
---
# <a name="create-a-python-app-in-azure-app-service-on-linux-preview"></a>Erstellen einer Python-App in Azure App Service für Linux (Vorschau)

[App Service unter Linux](app-service-linux-intro.md) bietet einen hochgradig skalierbaren Webhostingdienst mit Self-Patching unter Linux-Betriebssystemen. Dieser Schnellstart zeigt, wie Sie eine Python-App über dem integrierten Python-Image (Vorschau) in App Service für Linux mithilfe von [Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) bereitstellen.

Die Schritte in diesem Artikel können unter Mac, Windows oder Linux ausgeführt werden.

![In Azure ausgeführte Beispiel-App](media/quickstart-python/hello-world-in-browser.png)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="download-the-sample"></a>Herunterladen des Beispiels

Erstellen Sie in Cloud Shell ein Schnellstartverzeichnis, und wechseln Sie dorthin.

```bash
mkdir quickstart

cd $HOME/quickstart
```

Führen Sie als Nächstes den folgenden Befehl aus, um das Beispiel-App-Repository in Ihrem Schnellstartverzeichnis zu klonen.

```bash
git clone https://github.com/Azure-Samples/python-docs-hello-world
```

Während der Ausführung werden Informationen angezeigt, die den Informationen im folgenden Beispiel ähneln:

```bash
Cloning into 'python-docs-hello-world'...
remote: Enumerating objects: 43, done.
remote: Total 43 (delta 0), reused 0 (delta 0), pack-reused 43
Unpacking objects: 100% (43/43), done.
Checking connectivity... done.
```

## <a name="create-a-web-app"></a>Erstellen einer Web-App

Wechseln Sie zum Verzeichnis mit dem Beispielcode, und führen Sie den Befehl `az webapp up` aus.

Ersetzen Sie im folgenden Beispiel *\<App_Name>* durch einen global eindeutigen App-Namen (gültige Zeichen sind `a-z`, `0-9` und `-`).

```bash
cd python-docs-hello-world

az webapp up -n <app_name>
```

Die Ausführung dieses Befehls kann einige Minuten in Anspruch nehmen. Während der Ausführung werden Informationen angezeigt, die den Informationen im folgenden Beispiel ähneln:

```json
The behavior of this command has been altered by the following extension: webapp
Creating Resource group 'appsvc_rg_Linux_CentralUS' ...
Resource group creation complete
Creating App service plan 'appsvc_asp_Linux_CentralUS' ...
App service plan creation complete
Creating app '<app_name>' ....
Webapp creation complete
Creating zip with contents of dir /home/username/quickstart/python-docs-hello-world ...
Preparing to deploy contents to app.
All done.
{
  "app_url": "https:/<app_name>.azurewebsites.net",
  "location": "Central US",
  "name": "<app_name>",
  "os": "Linux",
  "resourcegroup": "appsvc_rg_Linux_CentralUS ",
  "serverfarm": "appsvc_asp_Linux_CentralUS",
  "sku": "BASIC",
  "src_path": "/home/username/quickstart/python-docs-hello-world ",
  "version_detected": "-",
  "version_to_create": "python|3.7"
}
```

Der Befehl `az webapp up` bewirkt Folgendes:

- Erstellen einer Standardressourcengruppe

- Erstellen eines standardmäßigen App Service-Plans

- Erstellen einer App mit dem angegebenen Namen

- [Bereitstellen von ZIP-Dateien](https://docs.microsoft.com/azure/app-service/deploy-zip) aus dem aktuellen Arbeitsverzeichnis für die App

## <a name="browse-to-the-app"></a>Navigieren zur App

Navigieren Sie in Ihrem Webbrowser zu der bereitgestellten Anwendung.

```bash
http://<app_name>.azurewebsites.net
```

Der Python-Beispielcode wird in App Service unter Linux mit einem integrierten Image ausgeführt.

![In Azure ausgeführte Beispiel-App](media/quickstart-python/hello-world-in-browser.png)

**Glückwunsch!** Sie haben Ihre erste Python-App für App Service unter Linux bereitgestellt.

## <a name="update-locally-and-redeploy-the-code"></a>Lokales Aktualisieren und erneutes Bereitstellen des Codes

Geben Sie in Cloud Shell `code application.py` ein, um den Cloud Shell-Editor zu öffnen.

![Code application.py](media/quickstart-python/code-applicationpy.png)

 Nehmen Sie eine geringfügige Änderung am Text im Aufruf für `return` vor:

```python
return "Hello Azure!"
```

Speichern Sie Ihre Änderungen, und beenden Sie den Editor. Verwenden Sie `^S` zum Speichern und `^Q` zum Beenden.

Nun stellen Sie die App erneut bereit. Ersetzen Sie `<app_name>` durch Ihre App.

```bash
az webapp up -n <app_name>
```

Wechseln Sie nach Abschluss der Bereitstellung wieder zu dem Browserfenster, das im Schritt **Navigieren zur App** geöffnet wurde, und aktualisieren Sie die Seite.

![In Azure ausgeführte aktualisierte Beispiel-App](media/quickstart-python/hello-azure-in-browser.png)

## <a name="manage-your-new-azure-app"></a>Verwalten Ihrer neuen Azure-App

Wechseln Sie zum <a href="https://portal.azure.com" target="_blank">Azure-Portal</a>, um die erstellte App zu verwalten.

Klicken Sie im linken Menü auf **App Services** und anschließend auf den Namen Ihrer Azure-App.

![Portalnavigation zur Azure-App](./media/quickstart-python/app-service-list.png)

Die Übersichtsseite Ihrer App wird angezeigt. Hier können Sie einfache Verwaltungsaufgaben wie Durchsuchen, Beenden, Neustarten und Löschen durchführen.

![App Service-Seite im Azure-Portal](media/quickstart-python/app-service-detail.png)

Im linken Menü werden verschiedene Seiten für die Konfiguration Ihrer App angezeigt. 

[!INCLUDE [cli-samples-clean-up](../../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Nächste Schritte

Das integrierte Python-Image in App Service unter Linux ist zurzeit als Vorschauversion verfügbar, und Sie können den zum Starten Ihrer App verwendeten Befehl anpassen. Sie können Python-Apps für die Produktionsumgebung stattdessen auch mit einem benutzerdefinierten Container erstellen.

> [!div class="nextstepaction"]
> [Tutorial: Python-App mit PostgreSQL](tutorial-python-postgresql-app.md)

> [!div class="nextstepaction"]
> [Konfigurieren einer Python-App](how-to-configure-python.md)

> [!div class="nextstepaction"]
> [Tutorial: Bereitstellen aus privatem Containerrepository](tutorial-custom-docker-image.md)
