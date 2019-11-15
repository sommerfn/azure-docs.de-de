---
title: 'Problembehandlung bei der SSIS Integration Runtime-Verwaltung in Azure Data Factory '
description: Dieser Artikel enthält eine Anleitung zur Fehlerbehebung bei Problemen mit der Verwaltung von SSIS Integration Runtime (SSIS IR).
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
ms.openlocfilehash: 3452fc2274eb646acb19c0e6a203ebadcb81cad5
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73684025"
---
# <a name="troubleshoot-ssis-integration-runtime-management-in-azure-data-factory"></a>Problembehandlung bei der SSIS Integration Runtime-Verwaltung in Azure Data Factory

Dieser Artikel enthält eine Anleitung zur Problembehandlung bei Verwaltungsproblemen in Azure-SQL Server Integration Services (SSIS) Integration Runtime (IR), auch bezeichnet als SSIS IR.

## <a name="overview"></a>Übersicht

Wenn beim Bereitstellen oder Aufheben der Bereitstellung von SSIS IR Probleme auftreten, wird eine Fehlermeldung im Microsoft Azure Data Factory-Portal oder ein von einem PowerShell-Cmdlet zurückgegebener Fehler angezeigt. Der Fehler wird immer im Format eines Fehlercodes mit einer detaillierten Fehlermeldung angezeigt.

Bei dem Fehlercode „InternalServerError“ hat der Dienst vorübergehende Probleme, und Sie sollten den Vorgang später wiederholen. Wenden Sie sich an das Azure Data Factory-Supportteam, wenn auch bei der Wiederholung des Vorgangs Probleme auftreten.

Andernfalls können drei wesentliche externe Abhängigkeiten Fehler verursachen: ein Azure SQL-Datenbank-Server bzw. eine verwaltete Azure SQL-Datenbank-Instanz, ein benutzerdefiniertes Setupskript oder die Konfiguration eines virtuellen Netzwerks.

## <a name="azure-sql-database-server-or-managed-instance-issues"></a>Probleme mit dem Azure SQL-Datenbank-Server oder der verwalteten Instanz

Ein Azure SQL-Datenbank-Server oder eine verwaltete Azure SQL-Datenbank-Instanz ist erforderlich, wenn Sie SSIS IR mit einer SSIS-Katalogdatenbank bereitstellen. SSIS IR muss auf den Azure SQL-Datenbank-Server oder die verwaltete Azure SQL-Datenbank-Instanz zugreifen können. Außerdem sollte das Konto des Azure SQL-Datenbank-Servers oder der verwalteten Azure SQL-Datenbank-Instanz über die Berechtigung zum Erstellen einer SSIS-Katalogdatenbank (SSISDB) verfügen. Wenn ein Fehler auftritt, wird im Data Factory-Portal ein Fehlercode mit einer detaillierten SQL-Ausnahmemeldung angezeigt. Anhand der Informationen in der folgenden Aufstellung können Sie die Fehlercodes beheben.

### <a name="azuresqlconnectionfailure"></a>AzureSqlConnectionFailure

Dieses Problem tritt möglicherweise auf, wenn Sie eine neue SSIS IR bereitstellen oder wenn IR ausgeführt wird. Wenn dieser Fehler während der IR-Bereitstellung auftritt, wird in der Fehlermeldung möglicherweise eine detaillierte SqlException-Meldung angezeigt, die eines der folgenden Probleme angibt:

* Ein Problem mit der Netzwerkverbindung. Überprüfen Sie, ob der Hostname der SQL Server-Instanz oder der verwalteten Instanz zugänglich ist. Vergewissern Sie sich außerdem, dass der SSIS IR-Zugriff auf den Server nicht durch eine Firewall oder eine Netzwerksicherheitsgruppe (NSG) blockiert wird.
* Während der SQL-Authentifizierung ist bei der Anmeldung ein Fehler aufgetreten. Das angegebene Konto kann sich nicht bei der SQL Server-Datenbank anmelden. Achten Sie darauf, das richtige Benutzerkonto anzugeben.
* Während der Microsoft Azure Active Directory-Authentifizierung (Azure AD) (verwaltete Identität) ist bei der Anmeldung ein Fehler aufgetreten. Fügen Sie einer AAD-Gruppe die verwaltete Identität Ihrer Factory hinzu, und stellen Sie sicher, dass die verwaltete Identität Zugriffsberechtigungen auf den Katalogdatenbankserver hat.
* Verbindungstimeout. Dieser Fehler wird immer durch eine sicherheitsbezogene Konfiguration verursacht. Wir empfehlen Folgendes:
  1. Erstellen Sie einen neuen virtuellen Computer.
  1. Binden Sie den virtuellen Computer in das gleiche virtuelle Microsoft Azure-Netzwerk wie IR ein, sofern sich IR in einem virtuellen Netzwerk befindet.
  1. Installieren Sie SSMS, und überprüfen Sie den Status des Azure SQL-Datenbank-Servers bzw. der verwalteten Azure SQL-Datenbank-Instanz.

Beheben Sie bei anderen Problemen das entsprechende in der detaillierten SqlException-Meldung angezeigte Problem. Wenn weiterhin Probleme auftreten, wenden Sie sich an das Supportteam für den Azure SQL-Datenbank-Server oder die verwaltete SQL-Datenbank-Instanz.

Wenn der Fehler während der Ausführung von IR angezeigt wird, verhindern Änderungen an der Netzwerksicherheitsgruppe oder der Firewall wahrscheinlich, dass der SSIS IR-Workerknoten auf den Azure SQL-Datenbank-Server oder die verwaltete Azure SQL-Datenbank-Instanz zugreifen kann. Entsperren Sie den SSIS IR-Workerknoten, sodass der Zugriff auf den Azure SQL-Datenbank-Server oder die verwaltete Azure SQL-Datenbank-Instanz ermöglicht wird.

### <a name="catalogcapacitylimiterror"></a>CatalogCapacityLimitError

Diese Art von Fehlermeldung kann wie folgt aussehen: „Die Datenbank ‚SSISDB‘ hat ihr Limit erreicht. Partitionieren oder löschen Sie Daten, löschen Sie Indizes, oder informieren Sie sich in der Dokumentation über mögliche Lösungen.“ 

Lösungsvorschläge:
* Erhöhen Sie die Kontingentgröße der SSISDB-Datenbank.
* Ändern Sie die Konfiguration von SSISDB, um die Größe folgendermaßen zu reduzieren:
   * Reduzieren des Aufbewahrungszeitraums und der Anzahl von Projektversionen
   * Verkürzung des Aufbewahrungszeitraums des Protokolls.
   * Ändern der Standardebene des Protokolls.

### <a name="catalogdbbelongstoanotherir"></a>CatalogDbBelongsToAnotherIR

Dieser Fehler bedeutet, dass der Azure SQL-Datenbank-Server oder die verwaltete Azure SQL-Datenbank-Instanz bereits über eine SSISDB-Datenbank verfügt und diese von einer anderen IR verwendet wird. Sie müssen einen anderen Azure SQL-Datenbank-Server oder eine andere verwaltete Azure SQL Database-Instanz bereitstellen oder die vorhandene SSISDB-Datenbank löschen und die neue IR neu starten.

### <a name="catalogdbcreationfailure"></a>CatalogDbCreationFailure

Dieser Fehler kann aus einem der folgenden Gründe auftreten:

* Das Benutzerkonto, das für die SSIS IR konfiguriert ist, hat keine Berechtigung zum Erstellen der Datenbank. Sie können dem Benutzer die Berechtigung zum Erstellen der Datenbank erteilen.
* Beim Erstellen der Datenbank tritt ein Timeout auf, z. B. ein Ausführungstimeout oder ein Timeout bei Datenbankvorgängen. Wiederholen Sie den Vorgang zu einem späteren Zeitpunkt. Wenn die Probleme weiterhin auftreten, wenden Sie sich an das Supportteam für den Azure SQL-Datenbank-Server oder die verwaltete SQL-Datenbank-Instanz.

Überprüfen Sie bei anderen Problemen die SqlException-Fehlermeldung, und beheben Sie das entsprechende in den Fehlerdetails aufgeführte Problem. Wenn weiterhin Probleme auftreten, wenden Sie sich an das Supportteam für den Azure SQL-Datenbank-Server oder die verwaltete SQL-Datenbank-Instanz.

### <a name="invalidcatalogdb"></a>InvalidCatalogDb

Diese Art von Fehlermeldung sieht wie folgt aus: „Ungültiger Objektname ‚catalog.catalog_properties‘.“ In diesem Fall verfügen Sie entweder bereits über eine Datenbank mit dem Namen „SSISDB“, die jedoch nicht mit SSIS IR erstellt wurde, oder die Datenbank befindet sich in einem ungültigen Zustand, der durch Fehler in der letzten SSIS IR-Bereitstellung verursacht wurde. Sie können die vorhandene Datenbank mit dem Namen „SSISDB“ löschen oder einen neuen Azure SQL-Datenbank-Server oder eine neue verwaltete Azure SQL-Datenbank-Instanz für die IR konfigurieren.

## <a name="custom-setup-issues"></a>Probleme beim benutzerdefinierten Setup

Das benutzerdefinierte Setup bietet eine Schnittstelle, um eigene Setupschritte während der Bereitstellung oder Neukonfiguration der SSIS IR hinzuzufügen. Weitere Informationen finden Sie unter [Anpassen des Setups für Azure-SSIS Integration Runtime](https://docs.microsoft.com/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup).

Stellen Sie sicher, dass Ihr Container nur die notwendigen benutzerdefinierten Setupdateien enthält, da alle Dateien im Container auf den SSIS IR-Workerknoten heruntergeladen werden. Es wird empfohlen, das benutzerdefinierte Setupskript auf einem lokalen Computer zu testen, um Probleme bei der Skriptausführung zu beheben, bevor Sie das Skript in SSIS IR ausführen.

Der Container des benutzerdefinierten Setupskripts wird während der IR-Ausführung überprüft, da SSIS IR regelmäßig aktualisiert wird. Diese Aktualisierung erfordert Zugriff auf den Container, damit das benutzerdefinierte Setupskript heruntergeladen und erneut installiert werden kann. Bei diesem Vorgang wird außerdem überprüft, ob auf den Container zugegriffen werden kann und ob die Datei „main.cmd“ vorhanden ist.

Bei allen Fehlern, die das benutzerdefinierte Setup betreffen, wird ein CustomSetupScriptFailure-Fehlercode mit einem untergeordneten Fehlercode wie beispielsweise CustomSetupScriptBlobContainerInaccessible oder CustomSetupScriptNotFound angezeigt.

### <a name="customsetupscriptblobcontainerinaccessible"></a>CustomSetupScriptBlobContainerInaccessible

Dieser Fehler bedeutet, dass SSIS IR nicht auf den Azure-Blobcontainer für das benutzerdefinierte Setup zugreifen kann. Vergewissern Sie sich, dass der SAS-URI des Containers erreichbar und nicht abgelaufen ist.

Beenden Sie die ausgeführte IR, konfigurieren Sie die IR mit dem SAS-URI des Containers für das neue benutzerdefinierte Setup neu, und starten Sie dann die IR neu.

### <a name="customsetupscriptnotfound"></a>CustomSetupScriptNotFound

Dieser Fehler bedeutet, dass SSIS IR kein benutzerdefiniertes Setupskript („main.cmd“) in Ihrem Blobcontainer finden kann. Stellen Sie sicher, dass „main.cmd“ im Container vorhanden ist, da dies der Einstiegspunkt für die Installation des benutzerdefinierten Setups ist.

### <a name="customsetupscriptexecutionfailure"></a>CustomSetupScriptExecutionFailure

Dieser Fehler bedeutet, dass bei der Ausführung des benutzerdefinierten Setupskripts („main.cmd“) Fehler aufgetreten sind. Testen Sie das Skript zunächst auf dem lokalen Computer, oder überprüfen Sie die Ausführungsprotokolle des benutzerdefinierten Setups in Ihrem Blobcontainer.

### <a name="customsetupscripttimeout"></a>CustomSetupScriptTimeout

Dieser Fehler gibt ein Timeout bei der Ausführung des benutzerdefinierten Setupskripts an. Stellen Sie sicher, dass Ihr Skript unbeaufsichtigt ausgeführt werden kann und keine interaktive Eingabe erforderlich ist. Vergewissern Sie sich auch, dass Ihr Blobcontainer nur die erforderlichen Dateien für das benutzerdefinierte Setup enthält. Wir empfehlen Ihnen, das Skript zuerst auf dem lokalen Computer zu testen. Sie sollten auch die Ausführungsprotokolle für das benutzerdefinierte Setup im Blobcontainer überprüfen. Der maximale Zeitraum für das benutzerdefinierte Setup ist auf 45 Minuten vor der Zeitüberschreitung festgelegt. Der maximale Zeitraum umfasst die Zeit, in der alle Dateien aus dem Container heruntergeladen und in SSIS IR installiert werden. Wenn ein längerer Zeitraum erforderlich ist, erstellen Sie ein Supportticket.

### <a name="customsetupscriptloguploadfailure"></a>CustomSetupScriptLogUploadFailure

Dieser Fehler bedeutet, dass beim Hochladen der Ausführungsprotokolle für das benutzerdefinierte Setup in den Blobcontainer Fehler aufgetreten sind. Dieses Problem tritt auf, weil SSIS IR nicht über Schreibberechtigungen für den Blobcontainer verfügt, oder aufgrund von Speicher- oder Netzwerkproblemen. Wenn das benutzerdefinierte Setup erfolgreich ausgeführt wird, hat dieser Fehler keine Auswirkungen auf SSIS-Funktionen, aber die Protokolle fehlen. Wenn bei dem benutzerdefinierten Setup ein anderer Fehler auftritt und das Protokoll nicht hochgeladen wird, wird dieser Fehler zuerst gemeldet, sodass das Protokoll zur Analyse hochgeladen werden kann. Nachdem dieses Problem behoben wurde, werden außerdem alle spezifischeren Probleme gemeldet. Wenn dieses Problem nach einem erneuten Versuch nicht behoben wurde, wenden Sie sich an das Azure Data Factory-Supportteam.

## <a name="virtual-network-configuration"></a>Konfiguration von virtuellen Netzwerken

Wenn Sie SSIS IR in Azure Virtual Network einbinden, verwendet SSIS IR das virtuelle Netzwerk, das unter dem Benutzerabonnement vorhanden ist. Weitere Informationen finden Sie unter [Verknüpfen einer Azure-SSIS Integration Runtime mit einem virtuellen Netzwerk](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network).

Wenn ein auf ein virtuelles Netzwerk bezogenes Problem auftritt, wird einer der folgenden Fehler angezeigt.

### <a name="invalidvnetconfiguration"></a>InvalidVnetConfiguration

Dieser Fehler kann aus verschiedenen Gründen auftreten. Informationen zur Problembehandlung finden Sie in den Abschnitten [Forbidden](#forbidden), [InvalidPropertyValue](#invalidpropertyvalue) und [MisconfiguredDnsServerOrNsgSettings](#misconfigureddnsserverornsgsettings).

### <a name="forbidden"></a>Verboten

Diese Art von Fehlermeldung kann wie folgt aussehen: „SubnetId ist für das aktuelle Konto nicht aktiviert. Der Microsoft.Batch-Ressourcenanbieter ist nicht unter dem gleichen Abonnement wie das VNET registriert.“

Diese Details bedeuten, dass Azure Batch nicht auf Ihr virtuelles Netzwerk zugreifen kann. Registrieren Sie den Microsoft.Batch-Ressourcenanbieter unter demselben Abonnement wie das virtuelle Netzwerk.

### <a name="invalidpropertyvalue"></a>InvalidPropertyValue

Diese Art von Fehlermeldung kann einer der folgenden ähneln: 

- „Entweder ist das angegebene VNET nicht vorhanden, oder der Batch-Dienst hat keinen Zugriff darauf.“
- „Das angegebene Subnetz xxx besteht nicht.“

Diese Fehler bedeuten, dass das virtuelle Netzwerk nicht vorhanden ist, der Azure Batch-Dienst nicht darauf zugreifen kann, oder das angegebene Subnetz nicht vorhanden ist. Stellen Sie sicher, dass das virtuelle Netzwerk und das Subnetz vorhanden sind und dass Azure Batch darauf zugreifen kann.

### <a name="misconfigureddnsserverornsgsettings"></a>MisconfiguredDnsServerOrNsgSettings

Diese Art von Fehlermeldung kann wie folgt aussehen: „Integration Runtime in VNET konnte nicht bereitgestellt werden. Wenn der DNS-Server oder die NSG-Einstellungen konfiguriert sind, vergewissern Sie sich, dass der DNS-Server zugänglich ist und NSG ordnungsgemäß konfiguriert ist.“

In diesem Fall verwenden Sie wahrscheinlich eine benutzerdefinierte Konfiguration des DNS-Servers oder der NSG-Einstellungen, was dazu führt, dass der für SSIS IR erforderliche Name des Azure-Servers nicht aufgelöst werden kann oder nicht darauf zugegriffen werden kann. Weitere Informationen finden Sie unter [Verknüpfen einer Azure-SSIS Integration Runtime mit einem virtuellen Netzwerk](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network). Wenn weiterhin Probleme auftreten, wenden Sie sich an das Azure Data Factory-Supportteam.

### <a name="vnetresourcegrouplockedduringupgrade"></a>VNetResourceGroupLockedDuringUpgrade

SSIS IR wird regelmäßig automatisch aktualisiert. Bei der Aktualisierung wird ein neuer Azure Batch-Pool erstellt und der alte Azure Batch-Pool gelöscht. Außerdem werden die auf ein virtuelles Netzwerk bezogenen Ressourcen für den alten Pool gelöscht und die neuen auf ein virtuelles Netzwerk bezogenen Ressourcen unter Ihrem Abonnement erstellt. Dieser Fehler bedeutet, dass beim Löschen von auf ein virtuelles Netzwerk bezogenen Ressourcen für den alten Pool aufgrund einer Löschsperre auf Abonnement- oder Ressourcengruppenebene Fehler aufgetreten sind. Da der Kunde die Löschsperre steuert und festlegt, muss er die Löschsperre in diesem Fall entfernen.

### <a name="vnetresourcegrouplockedduringstart"></a>VNetResourceGroupLockedDuringStart

Wenn bei der SSIS IR-Bereitstellung Fehler auftreten, werden alle erstellten Ressourcen gelöscht. Wenn jedoch eine Sperre für das Löschen von Ressourcen auf Abonnement- oder Ressourcengruppenebene vorhanden ist, werden die Virtual Network-Ressourcen nicht wie erwartet gelöscht. Entfernen Sie die Löschsperre, und starten Sie die IR neu, um diesen Fehler zu beheben.

### <a name="vnetresourcegrouplockedduringstop"></a>VNetResourceGroupLockedDuringStop

Wenn Sie SSIS IR beenden, werden alle auf Virtual Network bezogenen Ressourcen gelöscht. Beim Löschen können jedoch Fehler auftreten, wenn eine Sperre für das Löschen von Ressourcen auf Abonnement- oder Ressourcengruppenebene vorhanden ist. Auch hier wird die Löschsperre vom Kunden gesteuert und festgelegt. Daher muss der Kunde die Löschsperre entfernen und dann SSIS IR erneut beenden.

### <a name="nodeunavailable"></a>NodeUnavailable

Dieser Fehler tritt bei der Ausführung der IR auf und gibt an, dass die IR fehlerhaft ist. Dieser Fehler wird immer durch eine Änderung in der Konfiguration des DNS-Servers oder der Netzwerksicherheitsgruppe verursacht, die verhindert, dass SSIS IR eine Verbindung mit einem erforderlichen Dienst herstellen kann. Da die Konfiguration des DNS-Servers und der Netzwerksicherheitsgruppe vom Kunden gesteuert wird, muss der Kunde die entsprechenden Probleme beheben. Weitere Informationen finden Sie unter [Verknüpfen einer Azure-SSIS Integration Runtime mit einem virtuellen Netzwerk](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network). Wenn weiterhin Probleme auftreten, wenden Sie sich an das Azure Data Factory-Supportteam.
