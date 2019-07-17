---
title: Behandeln von Problemen beim Sichern von SAP HANA-Datenbanken mithilfe von Azure Backup | Microsoft-Dokumentation
description: In dieser Anleitung wird erläutert, wie Sie gängige Probleme behandeln, die beim Sichern von SAP HANA-Datenbanken mit Azure Backup auftreten können.
services: backup
author: pvrk
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 06/28/2019
ms.author: pullabhk
ms.openlocfilehash: 33f1b4674aad35d55ab014c45cd73753533931cb
ms.sourcegitcommit: 6cb4dd784dd5a6c72edaff56cf6bcdcd8c579ee7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2019
ms.locfileid: "67514174"
---
# <a name="troubleshoot-back-up-of-sap-hana-server-on-azure"></a>Behandeln von Problemen beim Sichern von SAP HANA-Servern in Azure

Dieser Artikel enthält Informationen zur Problembehandlung beim Schutz von SAP HANA-Datenbanken in Azure Virtual Machines. Bevor wir mit der Problembehandlung fortfahren, lassen Sie uns einige wichtige Punkte zu Berechtigungen und Einstellungen erläutern.

## <a name="understanding-pre-requisites"></a>Grundlegendes zu Voraussetzungen

Zu den [Voraussetzungen](backup-azure-sap-hana-database.md#prerequisites) gehört die Ausführung des Vorregistrierungsskripts auf dem virtuellen Computer, auf dem HANA installiert ist, um die erforderlichen Berechtigungen einzurichten.

### <a name="setting-up-permissions"></a>Einrichten von Berechtigungen

Aufgaben des Vorregistrierungsskripts:

1. Erstellen von AZUREWLBACKUPHANAUSER im HANA-System und Hinzufügen der erforderlichen Rollen und Berechtigungen wie nachstehend aufgeführt:
    - DATABASE ADMIN: Erstellen neuer Datenbanken während der Wiederherstellung
    - CATALOG READ: Lesen des Sicherungskatalogs
    - SAP_INTERNAL_HANA_SUPPORT: Zugreifen auf einige private Tabellen
2. Hinzufügen eines Schlüssels zum Hdbuserstore für das HANA-Plug-In, um alle Vorgänge ausführen zu können (Datenbank abfragen, Sicherung konfigurieren und durchführen, Wiederherstellung durchführen).
   
   - Um die Schlüsselerstellung zu bestätigen, führen Sie auf dem HANA-Computer den Befehl HDBSQL mit SIDADM-Anmeldeinformationen aus:

    ``` hdbsql
    hdbuserstore list
    ```
    
    Die Ausgabe des Befehls sollte den Schlüssel {SID} {DBNAME} und den Benutzer AZUREWLBACKUPHANAUSER enthalten.

> [!NOTE]
> Stellen Sie sicher, dass sich im Pfad /usr/sap/{SID}/home/.hdb/ eine eindeutige Gruppe von SSFS-Dateien befindet. Unter diesem Pfad darf nur ein Ordner vorhanden sein.

### <a name="setting-up-backint-parameters"></a>Einrichten von backInt-Parametern

Sobald eine Datenbank für die Sicherung ausgewählt wurde, konfiguriert der Azure Backup-Dienst backInt-Parameter auf Datenbankebene.

- [catalog_backup_using_backint:true]
- [enable_accumulated_catalog_backup:false]
- [parallel_data_backup_backint_channels:1]
- [log_backup_timeout_s:900)]
- [backint_response_timeout:7200]

> [!NOTE]
> Stellen Sie sicher, dass diese Parameter nicht auf HOST-Ebene vorhanden sind. Parameter auf HOST-Ebene überschreiben diese Parameter und können zu einem anderen Verhalten als dem erwarteten führen.

## <a name="understanding-common-user-errors"></a>Grundlegendes zu allgemeinen Benutzerfehlern

### <a name="usererrorinopeninghanaodbcconnection"></a>UserErrorInOpeningHanaOdbcConnection

| Fehlermeldung | Mögliche Ursachen | Empfohlene Maßnahme |
|---|---|---|
| Die Verbindung mit dem HANA-System konnte nicht hergestellt werden. Überprüfen Sie, ob Ihr System betriebsbereit ist.| Der Azure Backup-Dienst kann keine Verbindung mit HANA herstellen, da die HANA-Datenbank ausgefallen ist. Oder HANA wird ausgeführt, lässt aber nicht zu, dass der Azure Backup-Dienst eine Verbindung herstellt. | Prüfen Sie, ob die HANA-Datenbank bzw. der HANA-Dienst ausgefallen ist. Wenn beide ausgeführt werden, prüfen Sie, ob alle Berechtigung wie [hier](#setting-up-permissions) angegeben eingerichtet sind. Wenn der Schlüssel fehlt, führen Sie das Vorregistrierungsskript erneut aus, um einen neuen Schlüssel zu erstellen. |

### <a name="usererrorinvalidbackintconfiguration"></a>UserErrorInvalidBackintConfiguration

| Fehlermeldung | Mögliche Ursachen | Empfohlene Maßnahme |
|---|---|---|
| Ungültige backInt-Konfiguration erkannt. Beenden Sie den Schutz, und konfigurieren Sie die Datenbank neu.| Die backInt-Parameter sind nicht ordnungsgemäß für Azure Backup angegeben. | Überprüfen Sie die Parameter wie [hier](#setting-up-backint-parameters) beschrieben. Wenn backInt-basierte Parameter in HOST vorhanden sind, entfernen Sie sie. Wenn Parameter nicht in HOST vorhanden sind, aber auf Datenbankebene manuell geändert wurden, setzen Sie sie wie oben beschrieben auf die entsprechenden Werte zurück. Oder heben Sie im Azure-Portal den Schutz unter Beibehaltung der Daten auf, und setzen Sie die Sicherung fort.|
