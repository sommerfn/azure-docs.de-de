---
title: Problembehandlung bei Sicherungsfehlern in SAP HANA-Datenbanken – Azure Backup
description: Beschreibt, wie häufige Fehler behoben werden, die auftreten können, wenn Sie SAP HANA-Datenbanken mithilfe von Azure Backup sichern.
ms.reviewer: pullabhk
author: dcurwin
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 08/03/2019
ms.author: dacurwin
ms.openlocfilehash: 004d10b794c6eca2e078e437880f44d91ca30acb
ms.sourcegitcommit: b1c94635078a53eb558d0eb276a5faca1020f835
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2019
ms.locfileid: "72968454"
---
# <a name="troubleshoot-backup-of-sap-hana-databases-on-azure"></a>Behandeln von Problemen beim Sichern von SAP HANA-Datenbanken in Azure

Dieser Artikel enthält Informationen zur Problembehandlung beim Sichern von SAP HANA-Datenbanken in Azure Virtual Machines. Im folgenden Abschnitt werden wichtige konzeptionelle Daten behandelt, die für die Diagnose von häufigen Fehlern in SAP HANA-Sicherungen erforderlich sind.

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie im Rahmen der [Voraussetzungen](backup-azure-sap-hana-database.md#prerequisites) sicher, dass auf dem virtuellen Computer, auf dem HANA installiert ist, das Vorregistrierungsskript ausgeführt wurde.

### <a name="setting-up-permissions"></a>Einrichten von Berechtigungen

Aufgaben des Vorregistrierungsskripts:

1. Erstellen von AZUREWLBACKUPHANAUSER im HANA-System und Hinzufügen der erforderlichen Rollen und Berechtigungen:
    - DATABASE ADMIN: Erstellen neuer Datenbanken während der Wiederherstellung
    - CATALOG READ: Lesen des Sicherungskatalogs
    - SAP_INTERNAL_HANA_SUPPORT: Zugreifen auf einige private Tabellen
2. Hinzufügen eines Schlüssels zum Hdbuserstore für das HANA-Plug-In, um alle Vorgänge (Datenbankabfragen, Wiederherstellungsvorgänge, Konfigurieren und Ausführen von Sicherungen) zu behandeln.

   Um die Schlüsselerstellung zu bestätigen, führen Sie auf dem HANA-Computer den Befehl HDBSQL mit SIDADM-Anmeldeinformationen aus:

    ``` hdbsql
    hdbuserstore list
    ```

    Die Ausgabe des Befehls sollte den Schlüssel {SID} {DBNAME} und den Benutzer AZUREWLBACKUPHANAUSER enthalten.

> [!NOTE]
> Stellen Sie sicher, dass sich unter **/usr/sap/{SID}/home/.hdb/** eine eindeutige Gruppe von SSFS-Dateien befindet. In diesem Pfad darf nur ein Ordner vorhanden sein.

### <a name="setting-up-backint-parameters"></a>Einrichten von backInt-Parametern

Nachdem eine Datenbank für die Sicherung ausgewählt wurde, konfiguriert der Azure Backup-Dienst backInt-Parameter auf Datenbankebene:

- [catalog_backup_using_backint:true]
- [enable_accumulated_catalog_backup:false]
- [parallel_data_backup_backint_channels:1]
- [log_backup_timeout_s:900)]
- [backint_response_timeout:7200]

> [!NOTE]
> Stellen Sie sicher, dass diese Parameter *nicht* auf HOST-Ebene vorhanden sind. Parameter auf HOST-Ebene überschreiben diese Parameter und können zu unerwartetem Verhalten führen.

## <a name="restore-checks"></a>Wiederherstellungsprüfungen

### <a name="single-container-database-sdc-restore"></a>Wiederherstellung einer Datenbank mit einem einzelnen Container (SDC)

Achten Sie auf Eingaben während der Wiederherstellung einer Datenbank mit einem einzelnen Container (SDC) für HANA auf einen anderen SDC-Computer. Der Datenbankname sollte in Kleinbuchstaben angegeben werden, wobei „sdc“ in Klammern angefügt wird. Die HANA-Instanz wird in Großbuchstaben angezeigt.

Angenommen, eine SDC HANA-Instanz „H21“ wird gesichert. Auf der Seite mit den Sicherungselementen wird der Sicherungselementname **„h21(sdc)“** angezeigt. Wenn Sie versuchen, diese Datenbank auf einem anderen Ziel-SDC wiederherzustellen (z. B. „H11“), müssen Sie die folgenden Eingaben bereitstellen:

![Eingaben für die SDC-Wiederherstellung](media/backup-azure-sap-hana-database/hana-sdc-restore.png)

Beachten Sie folgende Punkte:

- Standardmäßig wird der Name der wiederhergestellten Datenbank mit dem Namen des Sicherungselements aufgefüllt, d.h. „h21(sdc)“.
- Wenn Sie als Ziel „H11“ auswählen, wird der Name der wiederhergestellten Datenbank NICHT automatisch geändert. **Er sollte in „h11(sdc)“ geändert werden**. Im Fall von SDC ist der Name der wiederhergestellten Datenbank die ID der Zielinstanz in Kleinbuchstaben, der „sdc“ in Klammern angefügt wird.
- Da SDC nur über eine einzelne Datenbank verfügen kann, müssen Sie auch das Kontrollkästchen aktivieren, um das Überschreiben der vorhandenen Datenbankdaten mit den Daten des Wiederherstellungspunkts zuzulassen.
- Bei Linux wird die Groß-/Kleinschreibung beachtet, achten Sie daher darauf, die Groß-/Kleinschreibung beizubehalten.

### <a name="multiple-container-database-mdc-restore"></a>Wiederherstellung einer Datenbank mit mehreren Containern (MDC)

In Datenbanken mit mehreren Containern (MDC) für HANA ist die Standardkonfiguration „SYSTEMDB + 1 oder mehr Mandantendatenbanken“. Das Wiederherstellen einer vollständigen SAP HANA-Instanz bedeutet, dass sowohl SYSTEMDB als auch die Mandantendatenbanken wiederhergestellt werden. Zuerst wird SYSTEMDB wiederhergestellt, und dann wird die Wiederherstellung für die Mandantendatenbank fortgesetzt. Systemdatenbank bedeutet im Wesentlichen, dass die Systeminformationen für das ausgewählte Ziel überschrieben werden. Dabei werden auch die BackInt-bezogenen Informationen in der-Zielinstanz überschrieben. Daher muss das Vorregistrierungsskript erneut ausgeführt werden, nachdem die Systemdatenbank auf einer Zielinstanz wiederhergestellt wurde. Nur dann ist die nachfolgende Wiederherstellung der Mandantendatenbanken erfolgreich.

## <a name="common-user-errors"></a>Häufige Benutzerfehler

### <a name="usererrorinopeninghanaodbcconnection"></a>UserErrorInOpeningHanaOdbcConnection

data| Fehlermeldung | Mögliche Ursachen | Empfohlene Maßnahme |
|---|---|---|
| Fehler beim Herstellen der Verbindung mit dem HANA-System. Stellen Sie sicher, dass Ihr System ausgeführt wird.| Der Azure Backup-Dienst kann keine Verbindung mit HANA herstellen, da die HANA-Datenbank ausgefallen ist. Oder HANA wird ausgeführt, lässt jedoch die Verbindung mit dem Azure Backup-Dienst nicht zu. | Überprüfen Sie, ob die HANA-Datenbank oder der HANA-Dienst ausgefallen ist. Wenn die HANA-Datenbank oder der HANA-Dienst ausgeführt wird, überprüfen Sie, ob [alle Berechtigungen festgelegt sind](#setting-up-permissions). Wenn der Schlüssel fehlt, führen Sie das Vorregistrierungsskript erneut aus, um einen neuen Schlüssel zu erstellen. |

### <a name="usererrorinvalidbackintconfiguration"></a>UserErrorInvalidBackintConfiguration

| Fehlermeldung | Mögliche Ursachen | Empfohlene Maßnahme |
|---|---|---|
| Ungültige backInt-Konfiguration erkannt. Beenden Sie den Schutz, und konfigurieren Sie die Datenbank neu.| Die backInt-Parameter sind nicht ordnungsgemäß für Azure Backup angegeben. | Überprüfen Sie, ob [die Parameter festgelegt sind](#setting-up-backint-parameters). Wenn in HOST backInt-Parameter vorhanden sind, entfernen Sie sie. Wenn Parameter auf HOST-Ebene nicht vorhanden sind, aber auf Datenbankebene manuell geändert wurden, setzen Sie sie wie oben beschrieben auf die entsprechenden Werte zurück. Oder führen Sie im Azure-Portal **Schutz beenden und Sicherungsdaten beibehalten** aus, und wählen Sie dann **Sicherung fortsetzen** aus.|
