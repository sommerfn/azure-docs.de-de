---
title: Leitfaden zur Problembehandlung für den Sicherheits-Agent von Azure Security Center für IoT unter Linux | Microsoft-Dokumentation
description: Beheben Sie Probleme bei der Verwendung der Sicherheits-Agents von Azure Security Center für IoT für Linux.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: 55c8d3b6-3126-4246-8d07-ef88fe5ea84f
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2019
ms.author: mlottner
ms.openlocfilehash: 7f3bd4be3ef927f73643146a457bc551ef86a450
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2019
ms.locfileid: "68600317"
---
# <a name="security-agent-troubleshoot-guide-linux"></a>Leitfaden zur Problembehandlung für den Sicherheits-Agent (Linux)

In diesem Artikel wird erläutert, wie Sie potenzielle Probleme beim Startvorgang des Sicherheits-Agents lösen.

Der Azure Security Center für IoT-Agent startet direkt nach der Installation selbst. Der Startvorgang des Agents beinhaltet das Lesen der lokalen Konfiguration, das Herstellen einer Verbindung mit Azure IoT Hub und das Abrufen der Remotezwillingskonfiguration. Wenn bei einem dieser Schritte ein Fehler auftritt, kann dies zu einem Problem mit dem Sicherheits-Agent führen.

In diesem Leitfaden zur Problembehandlung lernen Sie Folgendes:
> [!div class="checklist"]
> * Überprüfen, ob der Sicherheits-Agent ausgeführt wird
> * Abrufen von Fehlern des Sicherheits-Agents
> * Verstehen und Beheben von Fehlern des Sicherheits-Agents 

## <a name="validate-if-the-security-agent-is-running"></a>Überprüfen, ob der Sicherheits-Agent ausgeführt wird

1. Wenn Sie überprüfen möchten, ob der Sicherheits-Agent ausgeführt wird, warten Sie nach der Installation des Agents einige Minuten, und führen Sie dann den folgenden Befehl aus. 
     <br>

    **C-Agent**
    ```bash
    grep "ASC for IoT Agent initialized" /var/log/syslog
    ```
    **C#-Agent**
    ```bash
    grep "Agent is initialized!" /var/log/syslog
    ```
2. Gibt der Befehl eine leere Zeile zurück, konnte der Sicherheits-Agent nicht gestartet werden.    

## <a name="force-stop-the-security-agent"></a>Erzwingen der Beendigung des Sicherheits-Agents 
Wenn der Sicherheits-Agent nicht gestartet werden kann, beenden Sie den Agent mit dem folgenden Befehl, und fahren Sie mit der Fehlertabelle weiter unten fort:

```bash
systemctl stop ASCIoTAgent.service
```
## <a name="get-security-agent-errors"></a>Abrufen von Fehlern des Sicherheits-Agents
1. Rufen Sie Sicherheits-Agent-Fehler mit dem folgenden Befehl ab:
    ```bash
    grep ASCIoTAgent /var/log/syslog
    ```
2. Mit dem Befehl für den Sicherheits-Agent-Fehler werden alle vom Azure Security Center für IoT-Agent erstellten Protokolle abgerufen. Die Tabelle weiter unten enthält eine Beschreibung der Fehler sowie die entsprechenden Schritte zur Behebung. 

> [!Note]
> Fehlerprotokolle werden in chronologischer Reihenfolge angezeigt. Notieren Sie sich den Zeitstempel des jeweiligen Fehlers, um die Behebung zu vereinfachen. 

## <a name="restart-the-agent"></a>Neustarten des Agents

1. Nachdem Sie einen Sicherheits-Agent-Fehler ermittelt und behoben haben, starten Sie den Agent mit dem folgenden Befehl neu: 
    ```bash
    systemctl restart ASCIoTAgent.service
    ```
1. Wiederholen Sie den vorherigen Vorgang, um die Fehler abzurufen, wenn der Agent weiterhin einen Fehler beim Startvorgang verursacht. 

## <a name="understand-security-agent-errors"></a>Grundlegendes zu den Fehlern des Sicherheits-Agents

Die meisten Fehler des Sicherheits-Agents werden im folgenden Format angezeigt: 
```
Azure Security Center for IoT agent encountered an error! Error in: {Error Code}, reason: {Error sub code}, extra details: {error specific details}
```
| Fehlercode | Untergeordneter Fehlercode | Fehlerdetails | Behebung für C | Behebung für C# |
|:-----------|:---------------|:--------|:------------|:------------|
| Lokale Konfiguration | Fehlende Konfiguration | In der lokalen Konfigurationsdatei fehlt eine Konfiguration. In der Fehlermeldung sollte angegeben sein, welcher Schlüssel fehlt. | Fügen Sie der Datei „/var/LocalConfiguration.json“ den fehlenden Schlüssel hinzu. Ausführliche Informationen hierzu finden Sie unter [Understanding the LocalConfiguration.json file - C agent](azure-iot-security-local-configuration-c.md) (Grundlegendes zur Datei „LocalConfiguration.json“: C-Agent).| Fügen Sie der Datei „General.config“ den fehlenden Schlüssel hinzu. Ausführliche Informationen hierzu finden Sie unter [Understanding the local configuration file (C# agent)](azure-iot-security-local-configuration-csharp.md) (Grundlegendes zur lokalen Konfigurationsdatei (C#-Agent)). |
| Lokale Konfiguration | Analyse der Konfiguration nicht möglich | Ein Konfigurationswert kann nicht analysiert werden. In der Fehlermeldung sollte angegeben sein, welcher Schlüssel nicht analysiert werden kann. Ein Konfigurationswert kann nicht analysiert werden, weil der Wert nicht vom erwarteten Typ ist oder weil der Wert außerhalb des Bereichs liegt. | Korrigieren Sie den Wert des Schlüssels in der Datei „/var/LocalConfiguration.json“, sodass er dem LocalConfiguration-Schema entspricht. Ausführliche Informationen finden Sie unter [Understanding the local configuration file (C# agent)](azure-iot-security-local-configuration-csharp.md) (Grundlegendes zur lokalen Konfigurationsdatei (C#-Agent)). |  Korrigieren Sie den Wert des Schlüssels in der Datei „General.config“, sodass er dem Schema entspricht. Ausführliche Informationen finden Sie unter [Understanding the LocalConfiguration.json file - C agent](azure-iot-security-local-configuration-c.md) (Grundlegendes zur Datei „LocalConfiguration.json“: C-Agent).|
| Lokale Konfiguration | Dateiformat | Fehler beim Analysieren der Konfigurationsdatei | Die Konfigurationsdatei ist beschädigt. Laden Sie den Agent herunter, und installieren Sie ihn neu. | |
| Remotekonfiguration | Timeout | Der Agent konnte den azureiotsecurity-Modulzwilling nicht innerhalb des Timeoutzeitraums abrufen. | Stellen Sie sicher, dass die Authentifizierungskonfiguration richtig ist, und wiederholen Sie den Vorgang. | Der Agent konnte den azureiotsecurity-Modulzwilling nicht innerhalb des Timeoutzeitraums abrufen. | Stellen Sie sicher, dass die Authentifizierungskonfiguration richtig ist, und wiederholen Sie den Vorgang. |
| Authentication | Datei nicht vorhanden | Die Datei im angegebenen Pfad ist nicht vorhanden. | Stellen Sie sicher, dass die Datei im angegebenen Pfad vorhanden ist, oder navigieren Sie zur Datei **LocalConfiguration.json**, und ändern Sie die Konfiguration für **FilePath**. | Stellen Sie sicher, dass die Datei im angegebenen Pfad vorhanden ist, oder navigieren Sie zur Datei **Authentication.config**, und ändern Sie die Konfiguration für **filePath**.|
| Authentication | Dateiberechtigung | Der Agent verfügt nicht über ausreichende Berechtigungen zum Öffnen der Datei. | Erteilen Sie dem Benutzer **asciotagent** Leseberechtigungen für die Datei im angegebenen Pfad. | Stellen Sie sicher, dass auf die Datei zugegriffen werden kann. |
| Authentication | Dateiformat | Die angegebene Datei liegt nicht im richtigen Format vor. | Stellen Sie sicher, dass die angegebene Datei das richtige Format hat. Die Dateitypen „.pfx“ und „.pem“ werden unterstützt. | Stellen Sie sicher, dass es sich bei der Datei um eine gültige Zertifikatsdatei handelt. |
| Authentication | Nicht autorisiert | Der Agent konnte mit den angegebenen Anmeldeinformationen nicht für IoT Hub authentifiziert werden. | Überprüfen Sie die Authentifizierungskonfiguration in der Datei „LocalConfiguration“. Vergewissern Sie sich, dass alle Angaben der Authentifizierungskonfiguration korrekt sind, und überprüfen Sie, ob das Geheimnis in der Datei mit der authentifizierten Identität übereinstimmt. | Überprüfen Sie die Authentifizierungskonfiguration in der Datei „Authentication.config“. Vergewissern Sie sich, dass alle Angaben der Authentifizierungskonfiguration korrekt sind, und überprüfen Sie dann, ob das Geheimnis in der Datei mit der authentifizierten Identität übereinstimmt.
| Authentication | Nicht gefunden | Das Gerät/Modul wurde gefunden. | Überprüfen Sie die Authentifizierungskonfiguration: Stellen Sie sicher, dass der Hostname korrekt ist und dass das Gerät in IoT Hub vorhanden ist und über ein azureiotsecurity-Zwillingsmodul verfügt. |  Überprüfen Sie die Authentifizierungskonfiguration: Stellen Sie sicher, dass der Hostname korrekt ist und dass das Gerät in IoT Hub vorhanden ist und über ein azureiotsecurity-Zwillingsmodul verfügt. |
| Authentication | Fehlende Konfiguration | In der Datei *Authentication.config* fehlt eine Konfiguration. In der Fehlermeldung sollte angegeben sein, welcher Schlüssel fehlt. | Fügen Sie der Datei *LocalConfiguration.json* den fehlenden Schlüssel hinzu.| Fügen Sie der Datei *Authentication.config* den fehlenden Schlüssel hinzu. Ausführliche Informationen hierzu finden Sie unter [Understanding the local configuration file (C# agent)](azure-iot-security-local-configuration-csharp.md) (Grundlegendes zur lokalen Konfigurationsdatei (C#-Agent)). |
| Authentication | Analyse der Konfiguration nicht möglich | Ein Konfigurationswert kann nicht analysiert werden. In der Fehlermeldung sollte angegeben sein, welcher Schlüssel nicht analysiert werden kann. Ein Konfigurationswert kann nicht analysiert werden, weil der Wert nicht den erwarteten Typ hat oder weil der Wert außerhalb des Bereichs liegt. |Korrigieren Sie den Wert des Schlüssels in der Datei **LocalConfiguration.json**. |Korrigieren Sie den Wert des Schlüssels in der Datei **Authentication.config**, sodass er dem Schema entspricht. Ausführliche Informationen finden Sie unter [Understanding the LocalConfiguration.json file - C agent](azure-iot-security-local-configuration-c.md) (Grundlegendes zur Datei „LocalConfiguration.json“: C-Agent).|
|

## <a name="restart-the-agent"></a>Neustarten des Agents
1. Nachdem Sie einen Sicherheits-Agent-Fehler ermittelt und behoben haben, starten Sie den Agent mit dem folgenden Befehl neu:

    ```bash
    systemctl restart ASCIoTAgent.service
    ```
2. Wiederholen Sie bei Bedarf die vorherigen Vorgänge, um die Beendigung des Agents zu erzwingen und die Fehler abzurufen, wenn der Agent weiterhin einen Fehler beim Startvorgang verursacht. 

## <a name="next-steps"></a>Nächste Schritte
- Lesen Sie die [Übersicht](overview.md) über den Dienst „Azure Security Center für IoT“.
- Machen Sie sich mit der [Architektur](architecture.md) von Azure Security Center für IoT vertraut.
- Aktivieren Sie den [Dienst](quickstart-onboard-iot-hub.md) „Azure Security Center für IoT“.
- Lesen Sie [Häufig gestellte Fragen (FAQ)](resources-frequently-asked-questions.md) zum Dienst „Azure Security Center für IoT“.
- Informieren Sie sich, wie Sie auf [Sicherheitsrohdaten](how-to-security-data-access.md) zugreifen.
- Machen Sie sich mit [Empfehlungen](concept-recommendations.md) vertraut.
- Machen Sie sich mit [Sicherheitswarnungen](concept-security-alerts.md) vertraut.
