---
title: Bereitstellen des Azure Security Center für IoT Edge-Moduls | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie einen Azure Security Center für IoT-Sicherheits-Agent auf einem IoT Edge-Gerät bereitstellen.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: 32a9564d-16fd-4b0d-9618-7d78d614ce76
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/08/2019
ms.author: mlottner
ms.openlocfilehash: 128265cd3e69cd27bab6538c9eb376410439824d
ms.sourcegitcommit: 42748f80351b336b7a5b6335786096da49febf6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2019
ms.locfileid: "72176659"
---
# <a name="deploy-a-security-module-on-your-iot-edge-device"></a>Bereitstellen eines Sicherheitsmoduls auf Ihrem IoT Edge-Gerät


Das Modul **Azure Security Center für IoT** stellt eine umfassende Sicherheitslösung für Ihr IoT Edge-Gerät bereit.
Das Sicherheitsmodul sammelt, aggregiert und analysiert Sicherheitsrohdaten von Ihrem Betriebs- und Containersystem und gibt verwertbare Sicherheitsempfehlungen und -warnungen aus.
Weitere Informationen finden Sie unter [Azure IoT Edge-Sicherheitsmodul](security-edge-architecture.md).

In diesem Artikel erfahren Sie, wie Sie ein Sicherheitsmodul auf Ihrem IoT Edge-Gerät bereitstellen.

## <a name="deploy-security-module"></a>Bereitstellen eines Sicherheitsmoduls

Gehen Sie wie folgt vor, um ein Azure Security Center für IoT-Sicherheitsmodul für IoT Edge bereitzustellen.

### <a name="prerequisites"></a>Voraussetzungen

1. Vergewissern Sie sich in Ihrem IoT-Hub, dass Ihr Gerät [als IoT Edge-Gerät registriert](https://docs.microsoft.com/azure/iot-edge/how-to-register-device-portal) ist.

1. Das Azure Security Center für IoT Edge-Modul erfordert, dass das [AuditD-Framework](https://linux.die.net/man/8/auditd) auf dem IoT Edge-Gerät installiert ist.

    - Installieren Sie das Framework, indem Sie den folgenden Befehl auf Ihrem IoT Edge-Gerät ausführen:
   
    `sudo apt-get install auditd audispd-plugins`

    - Überprüfen Sie, ob AuditD aktiv ist, indem Sie den folgenden Befehl ausführen: 
   
    `sudo systemctl status auditd`<br>
    - Die erwartete Antwort ist `active (running)`. 
        

### <a name="deployment-using-azure-portal"></a>Bereitstellung über das Azure-Portal

1. Öffnen Sie im Azure-Portal den **Marketplace**.

1. Wählen Sie **Internet der Dinge** aus, suchen Sie nach **Azure Security Center für IoT**, und wählen Sie den Eintrag aus.

   ![Auswählen von „Azure Security Center für IoT“](media/howto/edge-onboarding-8.png)

1. Klicken Sie auf **Erstellen**, um die Bereitstellung zu konfigurieren. 

1. Wählen Sie das Azure-**Abonnement** Ihres IoT-Hubs und dann Ihren **IoT-Hub** aus.<br>Wählen Sie **Für Gerät bereitstellen** aus, um ein einzelnes Gerät als Bereitstellungsziel auszuwählen, oder wählen Sie **Im gewünschten Umfang bereitstellen** aus, um mehrere Geräte als Ziel festzulegen, und klicken Sie dann auf **Erstellen**. Weitere Informationen zur bedarfsorientierten Bereitstellung finden Sie im Thema zum [Bereitstellen](https://docs.microsoft.com/azure/iot-edge/how-to-deploy-monitor). 

    >[!Note] 
    >Wenn Sie **Im gewünschten Umfang bereitstellen** ausgewählt haben, fügen Sie den Gerätenamen und die Details hinzu, bevor Sie in den folgenden Schritten mit der Registerkarte **Module hinzufügen** fortfahren.     

Zum Erstellen einer IoT Edge-Bereitstellung für Azure Security Center für IoT müssen drei Schritte ausgeführt werden. Diese werden in den folgenden Abschnitten exemplarisch beschrieben. 

#### <a name="step-1-add-modules"></a>Schritt 1: Hinzufügen von Modulen

1. Klicken Sie auf der Registerkarte **Module hinzufügen** im Bereich **Bereitstellungsmodule** auf die Option **Konfigurieren** für **AzureSecurityCenterforIoT**. 
   
1. Ändern Sie den **Namen** in **azureiotsecurity**.
1. Ändern Sie den **Image-URI** in **mcr.microsoft.com/ascforiot/azureiotsecurity:1.0.0**.
1. Überprüfen Sie, ob für **Optionen für Containererstellung** der folgende Wert festgelegt ist:      
    ``` json
    {
        "NetworkingConfig": {
            "EndpointsConfig": {
                "host": {}
            }
        },
        "HostConfig": {
            "Privileged": true,
            "NetworkMode": "host",
            "PidMode": "host",
            "Binds": [
                "/:/host"
            ]
        }
    }    
    ```
1. Vergewissern Sie sich, dass **Gewünschte Eigenschaften für Modulzwilling festlegen** aktiviert ist, und ändern Sie das Konfigurationsobjekt wie folgt:
      
    ``` json
    { 
       "properties.desired":{ 
      "ms_iotn:urn_azureiot_Security_SecurityAgentConfiguration":{ 

          }
       }
    }
    ```

1. Klicken Sie auf **Speichern**.
1. Scrollen Sie zum unteren Rand der Registerkarte, und wählen Sie **Erweiterte Einstellungen für die Edge-Laufzeit konfigurieren** aus. 
   
1. Ändern Sie das **Image** unter **Edge Hub** in **mcr.microsoft.com/azureiotedge-hub:1.0.9-rc2**.

1. Stellen Sie sicher, dass **Erstellungsoptionen** wie folgt festgelegt ist: 
         
    ``` json
    { 
       "HostConfig":{ 
          "PortBindings":{ 
             "8883/tcp":[ 
                { 
                   "HostPort":"8883"
                }
             ],
             "443/tcp":[ 
                { 
                   "HostPort":"443"
                }
             ],
             "5671/tcp":[ 
                { 
                   "HostPort":"5671"
                }
             ]
          }
       }
    }
    ```
1. Klicken Sie auf **Speichern**.
   
1. Klicken Sie auf **Weiter**.

#### <a name="step-2-specify-routes"></a>Schritt 2: Angeben von Routen 

1. Stellen Sie auf der Registerkarte **Routen angeben** sicher, dass Sie über eine Route verfügen (explizit oder implizit), mit der Nachrichten vom Modul **azureiotsecurity** gemäß den folgenden Beispielen an **$upstream** weitergeleitet werden. Klicken Sie erst dann auf **Weiter**. 

~~~Default implicit route
"route": "FROM /messages/* INTO $upstream" 
~~~

~~~Explicit route
"ASCForIoTRoute": "FROM /messages/modules/azureiotsecurity/* INTO $upstream"
~~~

#### <a name="step-3-review-deployment"></a>Schritt 3: Überprüfen der Bereitstellung

- Überprüfen Sie auf der Registerkarte **Bereitstellung überprüfen** die Bereitstellungsinformationen, und wählen Sie dann **Senden** aus, um die Bereitstellung abzuschließen.

## <a name="diagnostic-steps"></a>Schritte zum Diagnostizieren von Problemen

Falls ein Problem auftritt, sind Containerprotokolle die beste Möglichkeit, um sich über den Zustand eines Geräts mit einem IoT Edge-Sicherheitsmodul zu informieren. Verwenden Sie die Befehle und Tools in diesem Abschnitt zum Sammeln von Informationen.

### <a name="verify-the-required-containers-are-installed-and-functioning-as-expected"></a>Sicherstellen, dass die erforderlichen Container installiert sind und wie erwartet funktionieren

1. Führen Sie auf Ihrem IoT Edge-Gerät den folgenden Befehl aus:
    
    `sudo docker ps`
   
1. Stellen Sie sicher, dass die folgenden Container ausgeführt werden:
   
   | NAME | IMAGE |
   | --- | --- |
   | azureiotsecurity | mcr.microsoft.com/ascforiot/azureiotsecurity:1.0.0 |
   | edgeHub | mcr.microsoft.com/azureiotedge-hub:1.0.9-rc2 |
   | edgeAgent | mcr.microsoft.com/azureiotedge-agent:1.0 |
   
   Falls die mindestens erforderlichen Container nicht vorhanden sind, überprüfen Sie, ob Ihr IoT Edge-Bereitstellungsmanifest den empfohlenen Einstellungen entspricht. Weitere Informationen finden Sie unter [Deploy IoT Edge module](#deployment-using-azure-portal) (Bereitstellen eines IoT Edge-Moduls).

### <a name="inspect-the-module-logs-for-errors"></a>Überprüfen der Modulprotokolle auf Fehler
   
1. Führen Sie auf Ihrem IoT Edge-Gerät den folgenden Befehl aus:

   `sudo docker logs azureiotsecurity`
   
1. Wenn Sie ausführlichere Protokolle benötigen, fügen Sie der Bereitstellung des Moduls **azureiotsecurity** die folgende Umgebungsvariable hinzu: `logLevel=Debug`.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu den Konfigurationsoptionen finden Sie in der Schrittanleitung für die Modulkonfiguration. 
> [!div class="nextstepaction"]
> [Schrittanleitung für die Modulkonfiguration](./how-to-agent-configuration.md)
