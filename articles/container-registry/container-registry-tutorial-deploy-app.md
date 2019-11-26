---
title: 'Tutorial: Bereitstellen einer App aus einer georeplizierten Azure-Containerregistrierung'
description: Stellen Sie eine Linux-basierte Web-App unter Verwendung eines Containerimages aus einer georeplizierten Azure-Containerregistrierung in zwei unterschiedlichen Azure-Regionen bereit. Dieses Tutorial ist der zweite Teil einer dreiteiligen Reihe.
services: container-registry
author: dlepow
manager: gwallace
ms.service: container-registry
ms.topic: tutorial
ms.date: 08/20/2018
ms.author: danlep
ms.custom: seodec18, mvc
ms.openlocfilehash: 0aebc31e1b8e6849d4cc0aeccc0e0a79edf0e65d
ms.sourcegitcommit: a10074461cf112a00fec7e14ba700435173cd3ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2019
ms.locfileid: "73931411"
---
# <a name="tutorial-deploy-a-web-app-from-a-geo-replicated-azure-container-registry"></a>Tutorial: Bereitstellen einer Web-App aus einer georeplizierten Azure-Containerregistrierung

Dies ist der zweite Teil eines dreiteiligen Tutorials. In [Teil 1](container-registry-tutorial-prepare-registry.md) wurde eine private georeplizierte Containerregistrierung erstellt. Außerdem wurde ein quellcodebasiertes Containerimage erstellt und mithilfe von Push an die Registrierung übertragen. In diesem Artikel können Sie von der Netzwerknähe der georeplizierten Registrierung profitieren, indem Sie den Container Web-App-Instanzen in zwei verschiedenen Azure-Regionen bereitstellen. Jede Instanz ruft dann das Containerimage per Pull aus der nächstgelegenen Registrierung ab.

Dieser zweite Teil der Tutorialreihe umfasst Folgendes:

> [!div class="checklist"]
> * Bereitstellen eines Containerimages in zwei Instanzen vom Typ *Web-App für Container*
> * Überprüfen der bereitgestellten Anwendung

Wenn Sie noch keine georeplizierte Registrierung erstellt und noch kein Image der containerbasierten Beispielanwendung mithilfe von Push an die Registrierung übertragen haben, kehren Sie zum vorherigen Tutorial der Reihe zurück: [Prepare a geo-replicated Azure container registry](container-registry-tutorial-prepare-registry.md) (Vorbereiten einer georeplizierten Azure-Containerregistrierung).

Im nächsten Artikel der Reihe aktualisieren Sie die Anwendung und übertragen anschließend mithilfe von Push das aktualisierte Containerimage an die Registrierung. Danach navigieren Sie zu den beiden ausgeführten Web-App-Instanzen, um zu sehen, dass die Änderung jeweils automatisch übernommen wurde. Dies zeigt die Georeplikation und Webhooks von Azure Container Registry in Aktion.

## <a name="automatic-deployment-to-web-apps-for-containers"></a>Automatisches Bereitstellen in Web-App für Container

Azure Container Registry unterstützt die direkte Bereitstellung containerbasierter Anwendungen in [Web-Apps für Container](../app-service/containers/index.yml). In diesem Tutorial verwenden Sie das Azure-Portal, um das im vorherigen Tutorial erstellte Containerimage in zwei Web-App-Plänen in unterschiedlichen Azure-Regionen bereitzustellen.

Wenn Sie eine Web-App auf der Grundlage eines Containerimages in Ihrer Registrierung bereitstellen und in der gleichen Region über eine georeplizierte Registrierung verfügen, erstellt Azure Container Registry automatisch einen [Webhook](container-registry-webhook.md) für die Imagebereitstellung. Wenn Sie ein neues Image mithilfe von Push an Ihr Containerrepository übertragen, übernimmt der Webhook die Änderung und stellt automatisch das neue Containerimage für Ihre Web-App bereit.

## <a name="deploy-a-web-app-for-containers-instance"></a>Bereitstellen einer Instanz von „Web-App für Container“

In diesem Schritt erstellen Sie eine Instanz von „Web-App für Container“ in der Region *USA, Westen*.

Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und navigieren Sie zu der Registrierung, die Sie im vorherigen Tutorial erstellt haben.

Wählen Sie **Repositorys** > **acr-helloworld** aus, klicken Sie unter **Tags** mit der rechten Maustaste auf das Tag **v1**, und klicken Sie anschließend auf **In Web-App bereitstellen**:

![„In App Service bereitstellen“ im Azure-Portal][deploy-app-portal-01]

Wenn „In Web-App bereitstellen“ deaktiviert ist, haben Sie unter Umständen den Registrierungsadministratorbenutzer nicht aktiviert, wie unter [Erstellen einer Containerregistrierung](container-registry-tutorial-prepare-registry.md#create-a-container-registry) im ersten Tutorial angegeben. Sie können den Administratorbenutzer im Azure-Portal über **Einstellungen** > **Zugriffsschlüssel** aktivieren.

Nachdem Sie „In Web-App bereitstellen“ aktiviert haben, geben Sie unter **Web-App für Container** die folgenden Werte für die einzelnen Einstellungen an:

| Einstellung | Wert |
|---|---|
| **Standortname** | Ein global eindeutiger Name für die Web-App. In diesem Beispiel verwenden wir das Format `<acrName>-westus`, um ganz einfach erkennen zu können, aus welcher Registrierung und Region die Web-App bereitgestellt wird. |
| **Ressourcengruppe** | **Vorhandene verwenden** > `myResourceGroup` |
| **App Service-Plan/Standort** | Erstellen Sie einen neuen Plan namens `plan-westus` in der Region **USA, Westen**. |
| **Image** | `acr-helloworld:v1` |
| **Betriebssystem** | Linux |

> [!NOTE]
> Wenn Sie einen neuen App Service-Plan für die Bereitstellung Ihrer Container-App erstellen, wird automatisch ein Standardplan zum Hosten Ihrer Anwendung ausgewählt. Der Standardplan hängt von der Betriebssystemeinstellung ab.

Klicken Sie auf **Erstellen**, um die Web-App in der Region *USA, Westen* bereitzustellen.

![Konfiguration der Web-App unter Linux im Azure-Portal][deploy-app-portal-02]

## <a name="view-the-deployed-web-app"></a>Anzeigen der bereitgestellten Web-App

Nach Abschluss der Bereitstellung können Sie die ausgeführte Anwendung anzeigen, indem Sie in Ihrem Browser zur URL der Anwendung navigieren.

Klicken Sie im Portal auf **App Services** und anschließend auf die Web-App, die Sie im vorherigen Schritt bereitgestellt haben. In diesem Beispiel hat die Web-App den Namen *uniqueregistryname-westus*.

Wählen Sie rechts oben in der **App Service-Übersicht** die als Link dargestellte URL der Web-App aus, um die ausgeführte Anwendung in Ihrem Browser anzuzeigen.

![Konfiguration der Web-App unter Linux im Azure-Portal][deploy-app-portal-04]

Nachdem das Docker-Image über Ihre georeplizierte Containerregistrierung bereitgestellt wurde, wird ein Image angezeigt, das die Azure-Region darstellt, in der die Containerregistrierung gehostet wird.

![Bereitgestellte Webanwendung, angezeigt in einem Browser][deployed-app-westus]

## <a name="deploy-second-web-app-for-containers-instance"></a>Bereitstellen der zweiten Instanz von „Web-App für Container“

Gehen Sie wie im vorherigen Abschnitt beschrieben vor, um eine zweite Web-App in der Region *USA, Osten* bereitzustellen. Geben Sie unter **Web-App für Container** die folgenden Werte an:

| Einstellung | Wert |
|---|---|
| **Standortname** | Ein global eindeutiger Name für die Web-App. In diesem Beispiel verwenden wir das Format `<acrName>-eastus`, um ganz einfach erkennen zu können, aus welcher Registrierung und Region die Web-App bereitgestellt wird. |
| **Ressourcengruppe** | **Vorhandene verwenden** > `myResourceGroup` |
| **App Service-Plan/Standort** | Erstellen Sie einen neuen Plan namens `plan-eastus` in der Region **USA, Osten**. |
| **Image** | `acr-helloworld:v1` |
| **Betriebssystem** | Linux |

Klicken Sie auf **Erstellen**, um die Web-App in der Region *USA, Osten* bereitzustellen.

![Konfiguration der Web-App unter Linux im Azure-Portal][deploy-app-portal-06]

## <a name="view-the-deployed-web-app"></a>Anzeigen der bereitgestellten Web-App

Auch hier können Sie die ausgeführte Anwendung anzeigen, indem Sie in Ihrem Browser zur URL der Anwendung navigieren.

Klicken Sie im Portal auf **App Services** und anschließend auf die Web-App, die Sie im vorherigen Schritt bereitgestellt haben. In diesem Beispiel hat die Web-App den Namen *uniqueregistryname-eastus*.

Klicken Sie rechts oben in der **App Service-Übersicht** auf die als Link dargestellte URL der Web-App, um die ausgeführte Anwendung in Ihrem Browser anzuzeigen.

![Konfiguration der Web-App unter Linux im Azure-Portal][deploy-app-portal-07]

Nachdem das Docker-Image über Ihre georeplizierte Containerregistrierung bereitgestellt wurde, wird ein Image angezeigt, das die Azure-Region darstellt, in der die Containerregistrierung gehostet wird.

![Bereitgestellte Webanwendung, angezeigt in einem Browser][deployed-app-eastus]

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie zwei Instanzen von „Web-App für Container“ über eine georeplizierte Azure-Containerregistrierung bereitgestellt.

Im nächsten Tutorial aktualisieren Sie die Anwendung, stellen ein neues Containerimage in der Containerregistrierung bereit und vergewissern sich anschließend, dass die in beiden Regionen ausgeführten Web-Apps automatisch aktualisiert wurden.

> [!div class="nextstepaction"]
> [Push an updated image to regional deployments](./container-registry-tutorial-deploy-update.md) (Übertragen eines aktualisierten Images an regionale Bereitstellungen mithilfe von Push)

<!-- IMAGES -->
[deploy-app-portal-01]: ./media/container-registry-tutorial-deploy-app/deploy-app-portal-01.png
[deploy-app-portal-02]: ./media/container-registry-tutorial-deploy-app/deploy-app-portal-02.png
[deploy-app-portal-03]: ./media/container-registry-tutorial-deploy-app/deploy-app-portal-03.png
[deploy-app-portal-04]: ./media/container-registry-tutorial-deploy-app/deploy-app-portal-04.png
[deploy-app-portal-05]: ./media/container-registry-tutorial-deploy-app/deploy-app-portal-05.png
[deploy-app-portal-06]: ./media/container-registry-tutorial-deploy-app/deploy-app-portal-06.png
[deploy-app-portal-07]: ./media/container-registry-tutorial-deploy-app/deploy-app-portal-07.png
[deployed-app-westus]: ./media/container-registry-tutorial-deploy-app/deployed-app-westus.png
[deployed-app-eastus]: ./media/container-registry-tutorial-deploy-app/deployed-app-eastus.png