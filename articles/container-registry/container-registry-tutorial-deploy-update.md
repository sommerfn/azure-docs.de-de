---
title: 'Tutorial: Übertragen eines Updates per Pushvorgang an eine georeplizierte Azure-Containerregistrierung'
description: Übertragen Sie ein aktualisiertes Docker-Image per Push an Ihre georeplizierte Azure-Containerregistrierung. Anschließend werden die Änderungen automatisch für Web-Apps bereitgestellt, die in mehreren Regionen ausgeführt werden. Dieses Tutorial ist der dritte Teil einer dreiteiligen Reihe.
services: container-registry
author: dlepow
manager: gwallace
ms.service: container-registry
ms.topic: tutorial
ms.date: 04/30/2018
ms.author: danlep
ms.custom: seodec18, mvc
ms.openlocfilehash: adf6348e3b4c5fa728a0289ccd5bd3f289872108
ms.sourcegitcommit: a10074461cf112a00fec7e14ba700435173cd3ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2019
ms.locfileid: "73931371"
---
# <a name="tutorial-push-an-updated-container-image-to-a-geo-replicated-container-registry-for-regional-web-app-deployments"></a>Tutorial: Übertragen eines aktualisierten Containerimages per Push an eine georeplizierte Containerregistrierung für regionale Web-App-Bereitstellungen

Dies ist der dritte Teil eines dreiteiligen Tutorials. Im [vorherigen Tutorial](container-registry-tutorial-deploy-app.md) war die Georeplikation für zwei verschiedene regionale Web-App-Bereitstellungen konfiguriert. In diesem Tutorial ändern Sie zuerst die Anwendung, erstellen dann ein neues Containerimage und übertragen es per Push an Ihre georeplizierte Registrierung. Abschließend überprüfen Sie die Änderung, die automatisch von Webhooks der Azure Container Registry bereitgestellt wird, in beiden Web-App-Instanzen.

Dieser letzte Teil der Tutorialreihe umfasst Folgendes:

> [!div class="checklist"]
> * Ändern des HTML-Codes der Webanwendung
> * Erstellen und Kennzeichnen des Docker-Images
> * Übertragen der Änderung zu Azure Container Registry per Push
> * Anzeigen der aktualisierten App in zwei verschiedenen Regionen

Wenn Sie die beiden regionalen Bereitstellungen für *Web-App für Container* noch nicht konfiguriert haben, kehren Sie zum vorherigen Tutorial in dieser Reihe zurück: [Bereitstellen einer Web-App über Azure Container Registry](container-registry-tutorial-deploy-app.md).

## <a name="modify-the-web-application"></a>Ändern der Webanwendung

Nehmen Sie in diesem Schritt eine Änderung an der Webanwendung vor, die leicht zu erkennen ist, nachdem Sie das aktualisierte Containerimage per Push zu Azure Container Registry übertragen haben.

Suchen Sie die Datei `AcrHelloworld/Views/Home/Index.cshtml` in der Anwendungsquelle, die Sie in einem vorherigen Tutorial [von GitHub geklont](container-registry-tutorial-prepare-registry.md#get-application-code) haben, und öffnen Sie sie in Ihrem bevorzugten Text-Editor. Fügen Sie die folgende Zeile unter der vorhandenen Zeile `<h1>` hinzu:

```html
<h1>MODIFIED</h1>
```

Ihre geänderte `Index.cshtml`-Datei sollte in etwa wie folgt aussehen:

```html
@{
    ViewData["Title"] = "Azure Container Registry :: Geo-replication";
}
<style>
    body {
        background-image: url('images/azure-regions.png');
        background-size: cover;
    }
    .footer {
        position: fixed;
        bottom: 0px;
        width: 100%;
    }
</style>

<h1 style="text-align:center;color:blue">Hello World from:  @ViewData["REGION"]</h1>
<h1>MODIFIED</h1>
<div class="footer">
    <ul>
        <li>Registry URL: @ViewData["REGISTRYURL"]</li>
        <li>Registry IP: @ViewData["REGISTRYIP"]</li>
        <li>Registry Region: @ViewData["REGION"]</li>
    </ul>
</div>
```

## <a name="rebuild-the-image"></a>Neuerstellen des Images

Nachdem Sie die Webanwendung aktualisiert haben, erstellen Sie deren Containerimage neu. Verwenden Sie wie zuvor den vollqualifizierten Imagenamen, einschließlich des vollqualifizierten Domänennamens des Anmeldeservers (FQDN) für das Tag:

```bash
docker build . -f ./AcrHelloworld/Dockerfile -t <acrName>.azurecr.io/acr-helloworld:v1
```

## <a name="push-image-to-azure-container-registry"></a>Übertragen des Images zu Azure Container Registry mithilfe von Push

Übertragen Sie anschließend das aktualisierte *acr-helloworld*-Containerimage per Push zu Ihrer georeplizierten Registrierung. Hier führen Sie einen einzelnen `docker push`-Befehl aus, um das aktualisierte Image für die Registrierungsreplikate in den Regionen *USA, Westen* und *USA, Osten* bereitzustellen.

```bash
docker push <acrName>.azurecr.io/acr-helloworld:v1
```

Die `docker push`-Ausgabe sollte in etwa wie folgt aussehen:

```console
$ docker push uniqueregistryname.azurecr.io/acr-helloworld:v1
The push refers to a repository [uniqueregistryname.azurecr.io/acr-helloworld]
5b9454e91555: Pushed
d6803756744a: Layer already exists
b7b1f3a15779: Layer already exists
a89567dff12d: Layer already exists
59c7b561ff56: Layer already exists
9a2f9413d9e4: Layer already exists
a75caa09eb1f: Layer already exists
v1: digest: sha256:4c3f2211569346fbe2d1006c18cbea2a4a9dcc1eb3a078608cef70d3a186ec7a size: 1792
```

## <a name="view-the-webhook-logs"></a>Anzeigen der Webhook-Protokolle

Während das Image repliziert wird, können Sie sehen, wie die Azure Container Registry-Webhooks ausgelöst werden.

Um die regionalen Webhooks anzuzeigen, die während der Bereitstellung des Containers für *Web-Apps für Container* in einem vorherigen Tutorial erstellt wurden, navigieren Sie zu Ihrer Containerregistrierung im Azure-Portal, und wählen Sie dann **Webhooks** unter **DIENSTE** aus.

![Webhooks der Containerregistrierung im Azure-Portal][tutorial-portal-01]

Wählen Sie die einzelnen Webhooks aus, um den Verlauf der zugehörigen Aufrufe und Antworten anzuzeigen. Daraufhin sollte eine Zeile für die **Push**-Aktion in den Protokollen der beiden Webhooks angezeigt werden. Hier zeigt das Protokoll für den Webhook in der Region *USA, Westen* an, dass die **Push**-Aktion im vorherigen Schritt durch `docker push` ausgelöst wurde:

![Webhook-Protokoll der Containerregistrierung im Azure-Portal (USA, Westen)][tutorial-portal-02]

## <a name="view-the-updated-web-app"></a>Anzeigen der aktualisierten Web-App

Die Webhooks benachrichtigen die Web-Apps, dass ein neues Image per Push an die Registrierung übertragen wurde, wodurch der aktualisierte Container automatisch für die beiden regionalen Web-Apps bereitgestellt wird.

Stellen Sie sicher, dass die Anwendung in beiden Bereitstellungen aktualisiert wurde, indem Sie in Ihrem Webbrowser zu beiden regionalen Web-App-Bereitstellungen navigieren. Zur Erinnerung: Sie können die URL für die bereitgestellte Web-App in der oberen rechten Ecke der einzelnen Registerkarten für die App Service-Übersicht finden.

![App Service-Übersicht im Azure-Portal.][tutorial-portal-03]

Wählen Sie den Link in der App Service-Übersicht aus, um die aktualisierte Anwendung anzuzeigen. Hier folgt eine Beispielansicht für die Ausführung der App in *USA, Westen*:

![Browseransicht zur Ausführung der geänderten Web-App in der Region USA (Westen)][deployed-app-westus-modified]

Stellen Sie sicher, dass das aktualisierte Containerimage auch in der Region *USA, Osten* bereitgestellt wurde, indem Sie es im Browser anzeigen.

![Browseransicht zur Ausführung der geänderten Web-App in der Region USA (Osten)][deployed-app-eastus-modified]

Mit einem einzelnen `docker push` haben Sie die in beiden regionalen Web-App-Bereitstellungen ausgeführte Webanwendung automatisch aktualisiert. Zudem hat Azure Container Registry die Containerimages aus den jeder Bereitstellung am nächsten gelegenen Repositorys bereitgestellt.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie eine Version des Containers der Webanwendung aktualisiert und die neue Version per Push an Ihre georeplizierte Registrierung übertragen. Web-App für Container wurde von Webhooks in Azure Container Registry über das Update benachrichtigt, sodass ein lokaler Pullvorgang von den nächstgelegenen replizierten Registrierungen ausgelöst wurde.

### <a name="acr-build-automated-image-build-and-patch"></a>ACR Build: Build und Patch des automatisierten Images

Zusätzlich zur geografischen Replikation ist ACR Build ein weiteres Feature von Azure Container Registry, das Ihnen hilft, Ihre Containerbereitstellungspipeline zu optimieren. Starten Sie mit der Übersicht über ACR Build, um eine Vorstellung der Möglichkeiten zu erhalten:

[Automatisieren von Betriebssystem- und Frameworkpatching mit ACR Build](container-registry-tasks-overview.md)

<!-- IMAGES -->
[deployed-app-eastus-modified]: ./media/container-registry-tutorial-deploy-update/deployed-app-eastus-modified.png
[deployed-app-westus-modified]: ./media/container-registry-tutorial-deploy-update/deployed-app-westus-modified.png
[local-container-01]: ./media/container-registry-tutorial-deploy-update/local-container-01.png
[tutorial-portal-01]: ./media/container-registry-tutorial-deploy-update/tutorial-portal-01.png
[tutorial-portal-02]: ./media/container-registry-tutorial-deploy-update/tutorial-portal-02.png
[tutorial-portal-03]: ./media/container-registry-tutorial-deploy-update/tutorial-portal-03.png