---
title: Behandeln von Problemen beim Sichern von SAP HANA-Datenbanken mithilfe von Azure Backup | Microsoft-Dokumentation
description: Beschreibt, wie häufige Fehler behoben werden, die auftreten können, wenn Sie SAP HANA-Datenbanken mithilfe von Azure Backup sichern.
author: pvrk
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 07/22/2019
ms.author: pullabhk
ms.openlocfilehash: 221b669c141681749709d6a5a406c78499f21032
ms.sourcegitcommit: c72ddb56b5657b2adeb3c4608c3d4c56e3421f2c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/24/2019
ms.locfileid: "68465473"
---
# <a name="troubleshoot-backup-of-sap-hana-databases-on-azure"></a>Behandeln von Problemen beim Sichern von SAP HANA-Datenbanken in Azure

Dieser Artikel enthält Informationen zur Problembehandlung beim Sichern von SAP HANA-Datenbanken in Azure Virtual Machines.

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

## <a name="common-user-errors"></a>Häufige Benutzerfehler

### <a name="usererrorinopeninghanaodbcconnection"></a>UserErrorInOpeningHanaOdbcConnection

data| Fehlermeldung | Mögliche Ursachen | Empfohlene Maßnahme |
|---|---|---|
| Fehler beim Herstellen der Verbindung mit dem HANA-System. Stellen Sie sicher, dass Ihr System ausgeführt wird.| Der Azure Backup-Dienst kann keine Verbindung mit HANA herstellen, da die HANA-Datenbank ausgefallen ist. Oder HANA wird ausgeführt, lässt jedoch die Verbindung mit dem Azure Backup-Dienst nicht zu. | Überprüfen Sie, ob die HANA-Datenbank oder der HANA-Dienst ausgefallen ist. Wenn die HANA-Datenbank oder der HANA-Dienst ausgeführt wird, überprüfen Sie, ob [alle Berechtigungen festgelegt sind](#setting-up-permissions). Wenn der Schlüssel fehlt, führen Sie das Vorregistrierungsskript erneut aus, um einen neuen Schlüssel zu erstellen. |

### <a name="usererrorinvalidbackintconfiguration"></a>UserErrorInvalidBackintConfiguration

| Fehlermeldung | Mögliche Ursachen | Empfohlene Maßnahme |
|---|---|---|
| Ungültige backInt-Konfiguration erkannt. Beenden Sie den Schutz, und konfigurieren Sie die Datenbank neu.| Die backInt-Parameter sind nicht ordnungsgemäß für Azure Backup angegeben. | Überprüfen Sie, ob [die Parameter festgelegt sind](#setting-up-backint-parameters). Wenn in HOST backInt-Parameter vorhanden sind, entfernen Sie sie. Wenn Parameter auf HOST-Ebene nicht vorhanden sind, aber auf Datenbankebene manuell geändert wurden, setzen Sie sie wie oben beschrieben auf die entsprechenden Werte zurück. Oder führen Sie im Azure-Portal **Schutz beenden und Sicherungsdaten beibehalten** aus, und wählen Sie dann **Sicherung fortsetzen** aus.|
