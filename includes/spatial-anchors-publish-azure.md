---
author: ramonarguelles
ms.service: spatial-anchors
ms.topic: include
ms.date: 1/30/2019
ms.author: rgarcia
ms.openlocfilehash: 0dab71b6d169e26a3d7dc208dd09efe1143fbe13
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2019
ms.locfileid: "67177780"
---
### <a name="open-the-publish-wizard"></a>Öffnen des Veröffentlichungs-Assistenten

Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **SharingService**, und wählen Sie **Veröffentlichen** aus.

Der Veröffentlichungs-Assistent wird gestartet. Wählen Sie **App Service** > **Veröffentlichen** aus, um das Dialogfeld **App Service erstellen** zu öffnen.

### <a name="sign-in-to-azure"></a>Anmelden bei Azure

Wählen Sie im Dialogfeld **App Service erstellen** die Option **Konto hinzufügen** aus, und melden Sie sich bei Ihrem Azure-Abonnement an. Wenn Sie bereits angemeldet sind, wählen Sie in der Dropdownliste das gewünschte Konto aus.

> [!NOTE]
> Wenn Sie bereits angemeldet sind, wählen Sie noch nicht **Erstellen** aus.
>

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

[!INCLUDE [resource group intro text](resource-group.md)]

Wählen Sie neben **Ressourcengruppe** die Option **Neu** aus.

Nennen Sie die Ressourcengruppe **myResourceGroup**, und wählen Sie **OK** aus.

### <a name="create-an-app-service-plan"></a>Wie erstelle ich einen Plan?

[!INCLUDE [app-service-plan](app-service-plan.md)]

Wählen Sie neben **Hostingplan** die Option **Neu** aus.

Verwenden Sie im Dialogfeld **Hostingplan konfigurieren** die folgenden Einstellungen:

| Einstellung | Empfohlener Wert | BESCHREIBUNG |
|-|-|-|
|App Service-Plan| MySharingServicePlan | Name des App Service-Plans. |
| Location | USA (Westen) | Das Rechenzentrum, in dem die Web-App gehostet wird. |
| Size | Kostenlos | Der [Tarif](https://azure.microsoft.com/pricing/details/app-service/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio), der die Hostingfunktionen festlegt. |

Klicken Sie auf **OK**.

### <a name="create-and-publish-the-web-app"></a>Erstellen und Veröffentlichen der Web-App

Geben Sie unter **App-Name** einen eindeutigen App-Namen ein (gültige Zeichen sind `a-z`, `0-9` und `-`), oder übernehmen Sie den automatisch generierten eindeutigen Namen. Die URL der Web-App lautet `https://<app_name>.azurewebsites.net`, wobei `<app_name>` der Name Ihrer App ist.

Wählen Sie **Erstellen** aus, um mit der Erstellung der Azure-Ressourcen zu beginnen.

Nach Abschluss des Assistenten wird die ASP.NET Core-Web-App in Azure veröffentlicht und anschließend im Standardbrowser geöffnet.

![Veröffentlichte ASP.NET-Web-App in Azure](./media/spatial-anchors-azure/web-app-running-live.png)

Der App-Name, den Sie in diesem Abschnitt verwendet haben, wird als URL-Präfix mit dem Format `https://<app_name>.azurewebsites.net` verwendet. Notieren Sie sich diese URL, da Sie sie später benötigen.
