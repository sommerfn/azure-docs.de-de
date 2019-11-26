---
title: include file
description: include file
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: include
ms.date: 10/10/2019
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: 8656bbb070e2b05a06ea22dd1634a40182b440cb
ms.sourcegitcommit: b45ee7acf4f26ef2c09300ff2dba2eaa90e09bc7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73098666"
---
## <a name="deploy-event-grid-iot-edge-module"></a>Bereitstellen eines IoT Edge-Moduls in Event Grid

Es gibt mehrere Möglichkeiten, wie Sie Module auf einem IoT Edge-Gerät bereitstellen können. Alle sind für Azure Event Grid in IoT Edge geeignet. In diesem Artikel werden die Schritte zum Bereitstellen von Event Grid in IoT Edge über das Azure-Portal beschrieben.

>[!NOTE]
> In diesem Tutorial stellen Sie das Event Grid-Modul ohne Persistenz bereit. Das heißt, dass alle in diesem Tutorial erstellten Themen und Abonnements beim erneuten Bereitstellen des Moduls gelöscht werden. Weitere Informationen zum Einrichten der Persistenz finden Sie in den folgenden Artikeln: [Beibehalten des Status in Linux](../articles/event-grid/edge/persist-state-linux.md) oder [Beibehalten des Status in Windows](../articles/event-grid/edge/persist-state-windows.md). Für Produktionsworkloads empfiehlt es sich, das Event Grid-Modul mit Persistenz zu installieren.

>[!IMPORTANT]
> In diesem Tutorial wird das Event Grid-Modul mit deaktivierter Clientauthentifizierung bereitgestellt, und HTTP-Abonnenten werden zugelassen. Für Produktionsworkloads empfiehlt es sich, dass Sie lediglich HTTPS-Anforderungen und Abonnenten mit aktivierter Clientauthentifizierung aktivieren. Weitere Informationen zum sicheren Konfigurieren des Event Grid-Moduls finden Sie unter [Sicherheit und Authentifizierung](../articles/event-grid/edge/security-authentication.md).
 
### <a name="select-your-iot-edge-device"></a>Auswählen Ihres IoT Edge-Geräts

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)
1. Navigieren Sie zu Ihrer IoT Hub-Instanz.
1. Wählen Sie im Menü im Abschnitt **Automatische Geräteverwaltung** die Option **IoT Edge** aus. 
1. Klicken Sie in der Liste der Geräte auf die ID des Zielgeräts.
1. Wählen Sie **Module festlegen** aus. Lassen Sie die Seite geöffnet. Sie fahren mit den Schritten im nächsten Abschnitt fort.

### <a name="configure-a-deployment-manifest"></a>Konfigurieren eines Bereitstellungsmanifests

Ein Bereitstellungsmanifest ist ein JSON-Dokument, das beschreibt, welche Module bereitgestellt werden sollen, wie Daten zwischen den Modulen übermittelt werden und welche Eigenschaften die Modulzwillinge aufweisen sollen. Das Azure-Portal verfügt über einen Assistenten, der Sie durch die Erstellung eines Bereitstellungsmanifests führt, damit Sie das JSON-Dokument nicht manuell erstellen müssen.  Der Assistent umfasst drei Schritte: **Hinzufügen von Modulen**, **Angeben von Routen** und **Überprüfen der Bereitstellung**.

### <a name="add-modules"></a>Hinzufügen von Modulen

1. Wählen Sie im Abschnitt **Bereitstellungsmodule** die Option **Hinzufügen** aus.
1. Wählen Sie in der Dropdownliste mit den Modultypen den Eintrag **IoT Edge-Modul** aus.
1. Geben Sie den Namen, das Image, die Optionen für die Containererstellung für den Container an:

   * **Name**: eventgridmodule
   * **Image-URI**: `mcr.microsoft.com/azure-event-grid/iotedge:latest`
   * **Optionen für Containererstellung**:

    ```json
        {
          "Env": [
            "inbound:clientAuth:clientCert:enabled=false",
            "outbound:webhook:httpsOnly=false"
          ],
          "HostConfig": {
            "PortBindings": {
              "4438/tcp": [
                {
                  "HostPort": "4438"
                }
              ]
            }
          }
        }
    ```

 1. Klicken Sie unten auf der Seite auf **Speichern**.
 1. Klicken Sie auf **Weiter**, um mit dem Abschnitt über Routen fortzufahren.

    > [!NOTE]
    > Wenn Sie eine Azure-VM als Edge-Gerät verwenden, fügen Sie eine Regel für eingehende Ports hinzu, um eingehenden Datenverkehr an Port 4438 zuzulassen. Anweisungen zum Hinzufügen der Regel finden Sie unter [Öffnen von Ports für einen virtuellen Computer](../articles/virtual-machines/windows/nsg-quickstart-portal.md).


### <a name="setup-routes"></a>Einrichten von Routen

 Behalten Sie Standardrouten bei, und wählen Sie **Weiter** aus, um mit dem Abschnitt für die Überprüfung fortzufahren.

### <a name="review-deployment"></a>Überprüfen der Bereitstellung

1. Im Abschnitt zur Überprüfung wird das JSON-Bereitstellungsmanifest angezeigt, das anhand Ihrer Auswahl in den vorherigen beiden Abschnitten erstellt wurde. Vergewissern Sie sich, dass die zwei Module in der Liste angezeigt werden: **$edgeAgent** und **$edgeHub**. Diese beiden Module bilden die IoT Edge-Runtime und sind in jeder Bereitstellung erforderliche Standardvorgaben.
1. Überprüfen Sie die Bereitstellungsinformationen, und klicken Sie dann auf **Senden**.

### <a name="verify-your-deployment"></a>Überprüfen Ihrer Bereitstellung

1. Nach Übermittlung der Bereitstellung wird wieder die Seite „IoT Edge“ Ihres IoT-Hubs angezeigt.
1. Wählen Sie das **IoT Edge-Gerät** aus, das Sie für die Bereitstellung als Ziel festgelegt haben, um dessen Details zu öffnen.
1. Vergewissern Sie sich anhand der Gerätedetails, dass das Event Grid-Modul als **In Bereitstellung angegeben** und als **Vom Gerät gemeldet** aufgeführt ist.

Unter Umständen dauert es einen Moment, bis das Modul auf dem Gerät gestartet und an IoT Hub gemeldet wurde. Aktualisieren Sie die Seite, um einen aktualisierten Status anzuzeigen.