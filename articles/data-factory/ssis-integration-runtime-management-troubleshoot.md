---
title: Fehlerbehebung beim SSIS-Integration Runtime Management in Azure Data Factory | Microsoft Docs
description: Dieser Artikel enthält eine Anleitung zur Fehlerbehebung bei Managementproblemen der SSIS Integration Runtime (SSIS IR)
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 07/08/2019
author: chinadragon0515
ms.author: dashe
ms.reviewer: sawinark
manager: craigg
ms.openlocfilehash: 17fe8d9ed02156b8fe9aafd7adca946531895883
ms.sourcegitcommit: a6873b710ca07eb956d45596d4ec2c1d5dc57353
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/16/2019
ms.locfileid: "68253009"
---
# <a name="troubleshoot-ssis-integration-runtime-management-in-azure-data-factory"></a>Fehlerbehebung beim SSIS-Integration Runtime Management in Azure Data Factory | Microsoft Docs

Dieser Artikel enthält eine Anleitung zur Fehlerbehebung bei Managementproblemen der SSIS Integration Runtime (SSIS IR).

## <a name="overview"></a>Übersicht

Es wird eine Fehlermeldung im ADF-Portal angezeigt oder vom PowerShell-Cmdlet zurückgegeben, wenn es ein Problem bei der Bereitstellung oder Deinstallation von SSIS IR gibt. Der Fehler liegt immer im Format eines Fehlercodes mit einer detaillierten Fehlermeldung vor.

Es bedeutet, dass der Dienst einige vorübergehende Probleme hat, wenn der Fehlercode InternalServerError vorliegt. Wiederholen Sie den Vorgang zu einem späteren Zeitpunkt. Wenden Sie sich an Azure Data Factory Supportteameam, wenn die Wiederholung nicht hilfreich ist.

Es gibt drei wichtige externe Abhängigkeiten, die möglicherweise Fehler verursachen: Azure SQL-Datenbankserver oder verwaltete Instanz, benutzerdefiniertes Setup-Skript und Konfiguration des virtuellen Netzwerks, wenn nicht der Fehlercode InternalServerError vorliegt.

## <a name="azure-sql-database-server-or-managed-instance-issues"></a>Azure SQL-Datenbankserver oder verwaltete Instanz

Ein Azure SQL-Datenbankserver oder eine verwaltete Instanz ist erforderlich, wenn SSIS IR mit der SSIS Katalogdatenbank bereitgestellt wird. Der Azure SQL-Datenbankserver oder die verwaltete Instanz sollte über die SSIS IR zugänglich sein. Das Konto des Azure SQL-Datenbankservers oder der verwalteten Instanz sollte die Berechtigung haben, eine SSIS Catalog-Datenbank (SSISDB) zu erstellen. Im Fehlerfall wird im ADF-Portal ein Fehlercode mit detaillierter SQL-Ausnahmemeldung angezeigt. Führen Sie die folgenden Schritte aus, um die Fehlercodes zu beheben.

### <a name="azuresqlconnectionfailure"></a>AzureSqlConnectionFailure

Dieses Problem tritt möglicherweise auf, wenn Sie ein neues SSIS-IR bereitstellen oder während des IR-Betriebs.

Es kann aus folgenden Gründen verursacht werden, wenn der Fehler während der IR-Bereitstellung angezeigt wird, und Sie können eine detaillierte SqlException-Nachricht in der Fehlermeldung erhalten.

* Keine Netzwerkverbindung. Überprüfen Sie, ob der SQL Server- oder Hostname der verwalteten Instanz zugänglich ist, und keine Firewall oder NSG-Blöcke SSIS IR für den Zugriff auf den Server vorhanden sind.
* Die Anmeldung ist fehlgeschlagen und es wird die SQL-Authentifizierung verwendet. Das bedeutet, dass sich das angegebene Konto nicht am SQL Server anmelden kann. Stellen Sie sicher, dass das richtige Benutzerkonto angegeben ist.
* Die Anmeldung ist fehlgeschlagen und es wird die AAD-Authentifizierung (verwaltete Identität) verwendet. Fügen Sie die verwaltete Identität Ihrer Factory einer AAD-Gruppe hinzu, und stellen Sie sicher, dass die verwaltete Identität Zugriffsberechtigungen auf Ihren Katalogdatenbankserver hat.
* Verbindungs-Timeout liegt immer wegen einer sicherheitsrelevanten Konfiguration vor. Es wird empfohlen, eine neue VM zu erstellen, die VM in das gleiche VNet des IR einzubinden, wenn sich IR in einem VNet befindet, dann SSMS zu installieren und den Status des Azure SQL-Datenbankservers oder der Managed Instance zu überprüfen.

Für andere Probleme lesen Sie bitte die detaillierte Fehlermeldung SQL Exception und beheben Sie das in der Fehlermeldung angezeigte Problem. Wenden Sie sich an den Azure SQL-Datenbankserver oder das Supportteam der verwaltete Instanz, wenn Sie immer noch Probleme haben.

Es ist wahrscheinlich, dass es einige Änderungen an der Netzwerksicherheitsgruppe oder der Firewall gibt, wenn Sie den Fehler während des IR-Betriebs sehen, was dazu führt, dass der SSIS IR-Workerknoten nicht mehr auf den Azure SQL-Datenbankserver oder die verwaltete Instanz zugreifen kann. Entsperren Sie den SSIS IR-Workerknoten, um auf den Azure SQL-Datenbankserver oder die verwaltete Instanz zuzugreifen.

### <a name="catalogcapacitylimiterror"></a>CatalogCapacityLimitError

Die Fehlermeldung lautet wie folgt: „Die Datenbank ‚SSISDB‘ hat ihre Größenquote erreicht. Partitionieren oder löschen Sie Daten, löschen Sie Indizes, oder informieren Sie sich in der Dokumentation über mögliche Lösungen.“ Lösungsvorschläge:
* Erhöhen Sie die Größenkontingente Ihrer SSISDB.
* Ändern Sie die Konfigurationen von SSISDB, um die Größe zu reduzieren:
   * Verkürzung des Aufbewahrungszeitraums und der Anzahl der Projektversionen.
   * Verkürzung des Aufbewahrungszeitraums des Protokolls.
   * Ändern der Standard Ebene des Protokolls usw.

### <a name="catalogdbbelongstoanotherir"></a>CatalogDbBelongsToAnotherIR

Dieser Fehler bedeutet, dass der Azure SQL-Datenbankserver oder die verwaltete Instanz bereits eine SSISDB erstellt und von einem anderen IR verwendet hat. Sie müssen entweder einen anderen Azure SQL-Datenbankserver oder eine andere verwaltete Instanz bereitstellen oder bestehende SSISDB löschen und das neue IR neu starten.

### <a name="catalogdbcreationfailure"></a>CatalogDbCreationFailure

Dieser Fehler kann folgende Ursachen haben:

* Das Benutzerkonto, das für die SSIS IR konfiguriert ist, hat keine Berechtigung zum Erstellen der Datenbank. Sie können dem Benutzer die Berechtigung zum Erstellen der Datenbank erteilen.
* Erstellen Sie Datenbank-Timeout wie Ausführungstimeout, DB-Vorgangstimeout und so weiter. Sie können den Vorgang später wiederholen, um festzustellen, ob das Problem gelöst wurde. Wenden Sie sich an den Azure SQL-Datenbankserver oder das Support-Team für verwaltete Instanzen, wenn die Wiederholung nicht funktioniert.

Bei anderen Problemen überprüfen Sie die Fehlermeldung „SQL Exception“ und beheben Sie das in der Fehlermeldung genannte Problem. Wenn Sie immer noch Probleme haben, wenden Sie sich an den Azure SQL-Datenbankserver oder das Support-Team für verwaltete Instanzen.

### <a name="invalidcatalogdb"></a>InvalidCatalogDb

Die Fehlermeldung lautet wie folgt: „Ungültiger Objektname ‚catalog.catalog_properties‘.“, d. h. entweder haben Sie bereits eine Datenbank mit dem Namen SSISDB, aber sie wird nicht von SSIS IR erstellt, oder die Datenbank befindet sich in einem ungültigen Zustand, der durch Fehler in der letzten SSIS IR-Bereitstellung verursacht wird. Sie können eine bestehende Datenbank mit dem Namen SSISDB löschen oder einen neuen Azure SQL-Datenbankserver oder eine verwaltete Instanz für das IR konfigurieren.

## <a name="custom-setup"></a>Benutzerdefiniertes Setup

Custom Setup bietet eine Schnittstelle, um eigene Setup-Schritte während der Bereitstellung oder Rekonfiguration Ihres SSIS IR hinzuzufügen. Weitere Informationen finden Sie unter [Anpassen des Setups für die Laufzeit der Azure-SSIS-Integration](https://docs.microsoft.com/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup).

Stellen Sie sicher, dass Ihr Container nur die notwendigen benutzerdefinierten Setup-Dateien enthält, da alle Dateien im Container auf den SSIS IR-Workerknoten heruntergeladen werden. Es wird empfohlen, das benutzerdefinierte Setup-Skript auf einem lokalen Computer zu testen, um Probleme bei der Skriptausführung zu beheben, bevor Sie das Skript in SSIS IR ausführen.

Der Container des benutzerdefinierten Setup-Skripts wird auch während der IR-Ausführung überprüft, da SSIS IR regelmäßig aktualisiert wird, was einen erneuten Zugriff auf den Container erfordert, um das benutzerdefinierte Setup-Skript herunterzuladen und erneut zu installieren. Die Prüfung beinhaltet, ob auf den Container zugegriffen werden kann und ob die Datei main.cmd vorhanden ist.

Jeder Fehler mit benutzerdefiniertem Setup, wird als Fehler mit dem Code CustomSetupScriptFailure angezeigt, überprüfen Sie die Fehlermeldung, die einen Unterfehlercode hat.  Führen Sie die folgenden Schritte aus, um die Fehlercodes zu beheben.  

### <a name="customsetupscriptblobcontainerinaccessible"></a>CustomSetupScriptBlobContainerInaccessible

Das bedeutet, dass SSIS IR nicht auf Ihren Azure Blob-Container zugreifen kann, um eine benutzerdefinierte Einrichtung vorzunehmen. Überprüfen Sie, ob die SAS-URI des Containers erreichbar und nicht abgelaufen ist.

Stoppen Sie zuerst die IR, wenn sich die IR im Betriebszustand befindet, konfigurieren Sie die IR mit dem neuen benutzerdefinierten Setup-Container SAS URI neu und starten Sie die IR dann erneut.

### <a name="customsetupscriptnotfound"></a>CustomSetupScriptNotFound

Das bedeutet, dass SSIS IR kein benutzerdefiniertes Setup-Skript (main.cmd) in Ihrem Blob-Container finden kann. Stellen Sie sicher, dass main.cmd im Container vorhanden ist, da dieser der Einstiegspunkt für die Installation des benutzerdefinierten Setups ist.

### <a name="customsetupscriptexecutionfailure"></a>CustomSetupScriptExecutionFailure

Das bedeutet, dass die Ausführung des benutzerdefinierten Setup-Skripts (main.cmd) fehlgeschlagen ist. Sie können das Skript zuerst auf Ihrem lokalen Computer ausprobieren oder die Ausführungsprotokolle für benutzerdefinierte Einstellungen in Ihrem Blob-Container überprüfen.

### <a name="customsetupscripttimeout"></a>CustomSetupScriptTimeout

Es bedeutet, dass Sie einen Timeout für benutzerdefinierte Setup-Skripte ausführen müssen. Stellen Sie sicher, dass Ihr Blob-Container nur die notwendigen benutzerdefinierten Setup-Dateien enthält. Sie können auch benutzerdefinierte Setup-Ausführungsprotokolle in Ihrem Blob-Container überprüfen. Der maximale Zeitraum für die benutzerdefinierte Einrichtung ist auf 45 Minuten vor der Zeitüberschreitung festgelegt, und der maximale Zeitraum umfasst die Zeit, in der alle Dateien aus Ihrem Container heruntergeladen und in SSIS IR installiert werden. Wenn ein längerer Zeitraum erforderlich ist, legen Sie ein Support-Ticket an.

### <a name="customsetupscriptloguploadfailure"></a>CustomSetupScriptLogUploadFailure

Es bedeutet, dass das Hochladen von benutzerdefinierten Setup-Ausführungsprotokollen in Ihren Blob-Container fehlgeschlagen ist, entweder weil SSIS IR keine Schreibberechtigung für Ihren Blob-Container hat, oder weil einige Speicher- oder Netzwerkprobleme auftreten. Wenn die benutzerdefinierte Einrichtung erfolgreich ist, hat dieser Fehler keine Auswirkungen auf die SSIS-Funktion, aber es fehlen Protokolle. Wenn das benutzerdefinierte Setup mit einem anderen Fehler fehlgeschlagen ist und wir das Protokoll nicht hochladen können, werden wir diesen Fehler zuerst melden, damit das Protokoll zur Analyse hochgeladen werden kann, und nachdem dieses Problem behoben ist, werden wir ein eingegrenzteres Problem melden. Wenn dieses Problem nach einem erneuten Versuch nicht behoben wird, wenden Sie sich an das Support-Team von Azure Data Factory.

## <a name="virtual-network-configuration"></a>Konfiguration von virtuellen Netzwerken

Wenn SSIS IR in ein virtuelles Netzwerk (VNet) eingebunden wird, verwendet es das VNet unter Benutzerabonnement. Weitere Informationen finden Sie unter [Verbinden einer Azure-SSIS-Integrationslaufzeit mit einem virtuellen Netzwerk](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network).

Wenn es ein VNet-bezogenes Problem gibt, wird Ihnen der folgende Fehler angezeigt

### <a name="invalidvnetconfiguration"></a>InvalidVnetConfiguration

Das kann mehrere Ursachen haben. Führen Sie die folgenden Schritte aus, um die Fehlercodes zu beheben.

### <a name="forbidden"></a>Verboten

Die Fehlermeldung lautet wie folgt: „subnetId ist für das aktuelle Konto nicht aktiviert. Microsoft.Batch Ressourcenanbieter ist nicht unter dem gleichen Abonnement von VNet registriert.“

Dies bedeutet, dass Azure Batch nicht auf Ihr VNet zugreifen kann. Registrieren Sie den Microsoft.Batch-Ressourcenanbieter unter dem gleichen Abonnement von VNet.

### <a name="invalidpropertyvalue"></a>InvalidPropertyValue

Die Fehlermeldung lautet: „Entweder existiert das angegebene VNet nicht, oder der Batch-Dienst hat keinen Zugriff darauf.“ Alternativ: „Das angegebene Subnetz xxx existiert nicht.“

Das bedeutet, dass das VNet nicht existiert oder der Azure Batch-Dienst nicht darauf zugreifen kann, oder dass das bereitgestellte Subnetz nicht existiert. Stellen Sie sicher, dass das VNet und das Subnetz vorhanden sind und Azure Batch darauf zugreifen kann.

### <a name="misconfigureddnsserverornsgsettings"></a>MisconfiguredDnsServerOrNsgSettings

Die Meldung lautet wie folgt: „Integration Runtime in Vnet konnte nicht bereitgestellt werden. Wenn der DNS-Server oder die NSG-Einstellungen konfiguriert sind, vergewissern Sie sich, dass der DNS-Server zugänglich ist und NSG ordnungsgemäß konfiguriert ist“

Es ist wahrscheinlich, dass Sie eine angepasste Konfiguration des DNS-Servers oder der NSG-Einstellungen haben, die dazu führt, dass der von SSIS IR geforderte Azure-Server-Name nicht aufgelöst werden kann oder nicht auf diesen zugegriffen werden kann. Weitere Informationen finden Sie im [Konfigurationsdokument SSIS IR VNet](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network). Wenn Sie immer noch Probleme haben, wenden Sie sich an das Supportteam von Azure Data Factory.

### <a name="vnetresourcegrouplockedduringupgrade"></a>VNetResourceGroupLockedDuringUpgrade

SSIS IR wird automatisch und regelmäßig aktualisiert, und während des Upgrades wird ein neuer Azure Batch Pool erstellt und der alte Azure Batch Pool wird gelöscht, VNet bezogene Ressource für den alten Pool wird gelöscht und neue VNet bezogene Ressource wird unter Ihrem Abonnement erstellt. Dieser Fehler bedeutet, dass das Löschen von VNet-bezogenen Ressourcen für den alten Pool fehlgeschlagen ist, da die Löschsperre auf Abonnement- oder Ressourcengruppenebene fehlgeschlagen ist. Hilfe zum Entfernen der Löschsperre.

### <a name="vnetresourcegrouplockedduringstart"></a>VNetResourceGroupLockedDuringStart

Die SSIS IR-Bereitstellung kann aus verschiedenen Gründen fehlschlagen, und wenn ein Fehler auftritt, werden alle erstellten Ressourcen gelöscht. VNet-Ressourcen können jedoch nicht gelöscht werden, da es eine Sperre für das Löschen von Ressourcen auf Abonnement- oder Ressourcengruppenebene gibt. Entfernen Sie die Löschsperre und starten Sie das IR neu.

### <a name="vnetresourcegrouplockedduringstop"></a>VNetResourceGroupLockedDuringStop

Wenn SSIS IR angehalten wird, werden alle mit dem VNet verbundenen Ressourcen gelöscht, aber das Löschen ist fehlgeschlagen, da eine Sperre für das Löschen von Ressourcen auf Abonnement- oder Ressourcengruppenebene vorhanden ist. Helfen Sie, die Löschsperre zu entfernen und versuchen Sie es erneut.

### <a name="nodeunavailable"></a>NodeUnavailable

Dieser Fehler tritt während des IR-Laufs auf, d. h. IR ist vorher funktionsfähig und wird fehlerhaft, das liegt immer an einem geänderten DNS-Server- oder einer geänderten NSG-Konfiguration und weil SSIS IR keine Verbindung zum abhängigen Dienst herstellen kann, Hilfe bei der Behebung der DNS-Server- oder NSG-Konfigurationsprobleme, Weitere Informationen finden Sie unter [SSIS IR VNet-Konfiguration](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network). Wenn Sie immer noch Probleme haben, wenden Sie sich an das Supportteam von Azure Data Factory.
