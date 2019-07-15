---
title: Fehlerbehebung bei der Paketausführung in der SSIS-Integrationslaufzeit | Microsoft Docs
description: Dieser Artikel enthält Anleitungen zur Problembehandlung für die Ausführung von SSIS-Paketen in der SSIS-integrationslaufzeit
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 04/15/2019
author: wenjiefu
ms.author: wenjiefu
ms.reviewer: sawinark
manager: craigg
ms.openlocfilehash: 68a5d5278e1181695695647cff187d4b95624b40
ms.sourcegitcommit: 084630bb22ae4cf037794923a1ef602d84831c57
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2019
ms.locfileid: "67537647"
---
# <a name="troubleshoot-package-execution-in-the-ssis-integration-runtime"></a>Fehlerbehebung bei der Paketausführung in der SSIS-Integrationslaufzeit

Dieser Artikel beschreibt die häufigsten Fehler, die Sie finden können, wenn Sie SQL Server Integration Services (SSIS)-Pakete in der SSIS-Integrationslaufzeit ausführen. Es beschreibt die möglichen Ursachen und Maßnahmen zur Behebung der Fehler.

## <a name="where-to-find-logs-for-troubleshooting"></a>Wo finde ich Protokolle zur Fehlerbehebung

Verwenden Sie das Portal Azure Data Factory, um die Ausgabe der SSIS-Paketausführungsaktivität zu überprüfen. Die Ausgabe enthält das Ausführungsergebnis, Fehlermeldungen und die Betriebs-ID. Weitere Informationen finden Sie unter [Überwachen der Pipeline](how-to-invoke-ssis-package-ssis-activity.md#monitor-the-pipeline).

Verwenden Sie den SSIS-Katalog (SSISDB), um die Detailprotokolle für die Ausführung zu überprüfen. Weitere Informationen finden Sie unter [Überwachen von ausgeführten Paketen und anderen Vorgängen](https://docs.microsoft.com/sql/integration-services/performance/monitor-running-packages-and-other-operations?view=sql-server-2017).

## <a name="common-errors-causes-and-solutions"></a>Häufige Fehler, Ursachen und Lösungen

### <a name="error-message-connection-timeout-expired-or-the-service-has-encountered-an-error-processing-your-request-please-try-again"></a>Fehlermeldung: „Verbindungszeit überschritten“ oder „Der Dienst ist auf einen Fehler bei der Bearbeitung Ihrer Anfrage gestoßen. Bitte versuchen Sie es erneut.“

Hier sind mögliche Ursachen und empfohlenen Aktionen:
* Die Datenquelle oder das Ziel ist überlastet. Überprüfen Sie die Auslastung Ihrer Datenquelle oder Ihres Ziels und prüfen Sie, ob diese über genügend Kapazität verfügt. Wenn Sie beispielsweise Azure SQL Database verwendet haben, sollten Sie eine Skalierung in Betracht ziehen, wenn die Datenbank wahrscheinlich ausfällt.
* Das Netzwerk zwischen der SSIS-Integrationslaufzeit und der Datenquelle oder dem Ziel ist instabil, insbesondere wenn die Verbindung überregional oder zwischen lokalen Standorten und Azure besteht. Wenden Sie das Wiederholungsmuster im SSIS-Paket an, indem Sie diese Schritte ausführen:
  * Stellen Sie sicher, dass Ihre SSIS-Pakete bei einem Ausfall ohne Nebenwirkungen (z.B. Datenverlust oder Datenverdopplung) erneut ausgeführt werden können.
  * Konfigurieren Sie das **Wiederholen-** und **Wiederholungsintervall** der Aktivität **SSIS-Paket ausführen** auf der Registerkarte **Allgemein**. ![Festlegen von Eigenschaften auf der Registerkarte Allgemein](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-general.png)
  * Setzen Sie für eine ADO.NET- und OLE DB-Quell- oder Zielkomponente **ConnectRetryCount** und **ConnectRetryInterval** im Connection Manager im SSIS-Paket oder in der SSIS-Aktivität.

### <a name="error-message-ado-net-source-has-failed-to-acquire-the-connection--with-a-network-related-or-instance-specific-error-occurred-while-establishing-a-connection-to-sql-server-the-server-was-not-found-or-was-not-accessible"></a>Fehlermeldung: "ADO NET Source konnte die Verbindung nicht herstellen '....''" mit „Beim Aufbau einer Verbindung zum SQL Server ist ein netzwerk- oder instanzbezogener Fehler aufgetreten. Der Server wurde nicht gefunden, oder auf ihn kann nicht zugegriffen werden.“

Dieses Problem bedeutet in der Regel, dass die Datenquelle oder -ziel von der SSIS-Integrationslaufzeit aus nicht erreichbar ist. Die Gründe können variieren. Probieren Sie diese Aktionen aus:
* Stellen Sie sicher, dass Sie die Datenquelle oder den Zielnamen/die IP-Adresse korrekt übergeben.
* Stellen Sie sicher, dass die Firewall ordnungsgemäß eingerichtet ist.
* Stellen Sie sicher, dass das virtuelle Netzwerk ordnungsgemäß konfiguriert ist, wenn die Datenquelle oder das Ziel lokal ist:
  * Sie können überprüfen, ob das Problem durch die Konfiguration eines virtuellen Netzwerks verursacht wurde, indem Sie eine Azure-VM im selben virtuellen Netzwerk bereitstellen. Überprüfen Sie dann, ob auf die Datenquelle oder das Ziel von der Azure VM aus zugegriffen werden kann.
  * Weitere Details zur Verwendung eines virtuellen Netzwerks mit einer SSIS-Integrationslaufzeit finden Sie unter [Verbinden einer Azure-SSIS-Integrationslaufzeit mit einem virtuellen Netzwerk](join-azure-ssis-integration-runtime-virtual-network.md).

### <a name="error-message-ado-net-source-has-failed-to-acquire-the-connection--with-could-not-create-a-managed-connection-manager"></a>Fehlermeldung: "ADO NET Source konnte die Verbindung nicht herstellen '...''" mit "Konnte keinen verwalteten Verbindungs-Manager erstellen."

Die mögliche Ursache ist, dass der im Paket verwendete ADO.NET-Anbieter nicht in der SSIS-Integrationslaufzeit installiert ist. Sie können den Anbieter mit einem benutzerdefinierten Setup installieren. Weitere Details zur benutzerdefinierten Einrichtung finden Sie unter [Einrichtung anpassen für die Laufzeit der Azure-SSIS-Integration](how-to-configure-azure-ssis-ir-custom-setup.md).

### <a name="error-message-the-connection--is-not-found"></a>Fehlermeldung: "Die Verbindung"..." wurde nicht gefunden"

Ein bekanntes Problem in älteren Versionen von SQL Server Management Studio (SSMS) kann diesen Fehler verursachen. Wenn das Paket eine benutzerdefinierte Komponente enthält (z. B. SSIS Azure Feature Pack oder Partnerkomponenten), die nicht auf dem Computer installiert ist, auf dem SSMS zur Durchführung der Bereitstellung verwendet wird, entfernt SSMS die Komponente und verursacht den Fehler. Aktualisieren Sie [SSMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) auf die neueste Version, die das Problem beheben kann.

### <a name="error-message-there-is-not-enough-space-on-the-disk"></a>Fehlermeldung: "Es ist nicht genügend Speicherplatz auf dem Datenträger vorhanden"

Dieser Fehler bedeutet, dass die lokale Festplatte im Laufzeitknoten der SSIS-Integration verbraucht ist. Überprüfen Sie, ob Ihr Paket oder Ihr benutzerdefiniertes Setup viel Festplattenspeicher belegt:
* Wenn die Festplatte von Ihrem Paket belegt wird, wird sie nach Abschluss der Paketausführung freigegeben.
* Wenn die Festplatte von Ihrem benutzerdefinierten Setup belegt wird, müssen Sie die SSIS-Integrationslaufzeit stoppen, Ihr Skript ändern und die Integrationslaufzeit erneut starten. Der gesamte Azure-Blob-Container, den Sie für die benutzerdefinierte Einrichtung angegeben haben, wird in den SSIS-Integration-Laufzeitknoten kopiert, also überprüfen Sie, ob sich unter diesem Container unnötiger Inhalt befindet.

### <a name="error-message-cannot-open-file-"></a>Fehlermeldung: "Datei kann nicht geöffnet werden"...""

Dieser Fehler tritt auf, wenn die Paketausführung in der SSIS-Integrationslaufzeit keine Datei auf der lokalen Festplatte finden kann. Probieren Sie diese Aktionen aus:
* Verwenden Sie nicht den absoluten Pfad im Paket, das in der Laufzeit der SSIS-Integration ausgeführt wird. Verwenden Sie stattdessen das aktuelle Arbeitsverzeichnis (.) oder den temporären Ordner (%TEMP%).
* Wenn Sie einige Dateien auf SSIS-Integration-Laufzeitknoten persistieren müssen, bereiten Sie die Dateien wie unter [Setup anpassen](how-to-configure-azure-ssis-ir-custom-setup.md) beschrieben vor. Alle Dateien im Arbeitsverzeichnis werden nach Abschluss der Ausführung bereinigt.
* Verwenden Sie Azure-Dateien, anstatt die Datei im SSIS-Integrationslaufzeitknoten zu speichern. Weitere Informationen finden Sie unter [Verwenden des Azure-Dateifreigaben](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-files-file-shares?view=sql-server-2017#use-azure-file-shares).

### <a name="error-message-the-database-ssisdb-has-reached-its-size-quota"></a>Fehlermeldung: „Die Datenbank ‚SSISDB‘ hat ihr Limit erreicht.“

Eine mögliche Ursache ist, dass die in der Azure SQL-Datenbank erstellte SSISDB-Datenbank oder eine verwaltete Instanz, wenn Sie eine SSIS-Integrationslaufzeit erstellen, ihr Limit erreicht hat. Probieren Sie diese Aktionen aus:
* Erhöhen Sie nach Möglichkeit die DTU Ihrer Datenbank. Details finden Sie in den Ressourcenbeschränkungen der [SQL-Datenbank für einen Azure SQL Datenbank-Server](https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits-logical-server).
* Überprüfen Sie, ob das Paket zu viele Protokolle generieren würden. Wenn ja, können Sie einen elastischen Job konfigurieren, um diese Protokolle zu bereinigen. Weitere Informationen finden Sie unter [Bereinigen von SSISDB-Protokollen mit Azure Elastic Database-Aufträgen](how-to-clean-up-ssisdb-logs-with-elastic-jobs.md).

### <a name="error-message-the-request-limit-for-the-database-is--and-has-been-reached"></a>Fehlermeldung: „Die Anforderungsgrenze für die Datenbank beträgt … und wurde erreicht.“

Wenn in der SSIS-Integrationslaufzeit viele Pakete parallel laufen, kann dieser Fehler auftreten, weil die SSISDB ihre Anforderungsgrenze erreicht hat. Erwägen Sie, den DTC der SSISDB zu erhöhen, um dieses Problem zu lösen. Details finden Sie in den Ressourcenbeschränkungen der [SQL-Datenbank für einen Azure SQL Datenbank-Server](https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits-logical-server).

### <a name="error-message-ssis-operation-failed-with-unexpected-operation-status-"></a>Fehlermeldung: "SSIS Operation fehlgeschlagen mit unerwartetem Betriebsstatus: ...."

Der Fehler wird hauptsächlich durch ein vorübergehendes Problem verursacht, also versuchen Sie, die Paketausführung erneut auszuführen. Wenden Sie das Wiederholungsmuster im SSIS-Paket an, indem Sie diese Schritte ausführen:

* Stellen Sie sicher, dass Ihre SSIS-Pakete bei einem Ausfall ohne Nebenwirkungen (z.B. Datenverlust oder Datenverdopplung) erneut ausgeführt werden können.
* Konfigurieren Sie das **Wiederholen-** und **Wiederholungsintervall** der Aktivität **SSIS-Paket ausführen** auf der Registerkarte **Allgemein**. ![Festlegen von Eigenschaften auf der Registerkarte Allgemein](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-general.png)
* Setzen Sie für eine ADO.NET- und OLE DB-Quell- oder Zielkomponente **ConnectRetryCount** und **ConnectRetryInterval** im Connection Manager im SSIS-Paket oder in der SSIS-Aktivität.

### <a name="error-message-there-is-no-active-worker"></a>Fehlermeldung: Es ist kein aktiver Arbeiter vorhanden."

Dieser Fehler bedeutet in der Regel, dass die SSIS-Integrationslaufzeit einen fehlerhaften Status hat. Überprüfen Sie das Azure-Portal auf den Status und detaillierte Fehler. Weitere Informationen finden Sie unter [Laufzeit der Azure-SSIS-Integration](https://docs.microsoft.com/azure/data-factory/monitor-integration-runtime#azure-ssis-integration-runtime).

### <a name="error-message-your-integration-runtime-cannot-be-upgraded-and-will-eventually-stop-working-since-we-cannot-access-the-azure-blob-container-you-provided-for-custom-setup"></a>Fehlermeldung: "Ihre Integrationslaufzeit kann nicht aktualisiert werden und wird irgendwann nicht mehr funktionieren, da wir nicht auf den Azure Blob-Container zugreifen können, den Sie für die benutzerdefinierte Einrichtung bereitgestellt haben."

Dieser Fehler tritt auf, wenn die SSIS-Integrationslaufzeit nicht auf den für die benutzerdefinierte Einrichtung konfigurierten Speicher zugreifen kann. Überprüfen Sie, ob die von Ihnen angegebene URI für die gemeinsame Zugriffssignatur (SAS) gültig und noch nicht abgelaufen ist.

### <a name="error-message-microsoft-ole-db-provider-for-analysis-services-hresult-0x80004005-description-com-error-com-error-mscorlib-exception-has-been-thrown-by-the-target-of-an-invocation"></a>Fehlermeldung: „Microsoft OLE DB Anbieter für Analysedienste. 'Hresult: 0 x 80004005 Beschreibung:' COM-Fehler: COM-Fehler: mscorlib; Exception wurde vom Ziel eines Aufrufs ausgelöst"

Eine mögliche Ursache ist, dass der Benutzername oder das Passwort mit aktivierter Azure Multi-Factor-Authentifizierung für die Authentifizierung der Azure Analysis Services konfiguriert ist. Diese Authentifizierung wird in der SSIS-integrationslaufzeit nicht unterstützt. Versuchen Sie, ein Dienstprinzipal für die Authentifizierung von Azure Analysis Services zu verwenden:
1. Bereiten Sie ein Deinstprinzipal vor, wie in [Automatisierung mit Dienstprinzipal beschrieben](https://docs.microsoft.com/azure/analysis-services/analysis-services-service-principal).
2. Konfigurieren Sie im Connection Manager die Option **Spezifischen Nutzernamen und Passworet verwenden** :**AppID**als Nutzernamen festlegen und **clientSecret** als Passwort.

### <a name="error-message-adonet-source-has-failed-to-acquire-the-connection-guid-with-the-following-error-message-login-failed-for-user-nt-authorityanonymous-logon-when-using-a-managed-identity"></a>Fehlermeldung: "ADONET Source hat es nicht geschafft, die Verbindung {GUID} mit der folgenden Fehlermeldung aufzubauen: Die Anmeldung für den Benutzer 'NT AUTHORITY\ANONYMOUS LOGON'" ist fehlgeschlagen, wenn eine verwaltete Identität verwendet wird

Stellen Sie sicher, dass Sie die Authentifizierungsmethode von Connection Manager nicht als **Active Directory-Passwort-Authentifizierung** konfigurieren, wenn der Parameter *ConnectUsingManagedIdentity* auf **True**steht. Sie können es stattdessen als **SQL-Authentifizierung** konfigurieren, die ignoriert wird, wenn *ConnectUsingManagedIdentity* gesetzt ist.

### <a name="package-execution-takes-too-long"></a>Paketausführung dauert zu lange

Hier sind mögliche Ursachen und empfohlenen Aktionen:
* Es wurden zu viele Paketausführungen auf der SSIS-Integrationslaufzeit eingeplant. Alle diese Programme werden in einer Warteschlange auf ihre Ausführung warten.
  * Bestimmen Sie das Maximum mit dieser Formel: 
    
    Max. parallele Ausführungsanzahl pro IR = Knotenanzahl * Max. parallele Ausführung pro Knoten
  * Informationen zum Einstellen der Knotenanzahl und der maximalen parallelen Ausführung pro Knoten finden Sie unter [Erstellen einer Azure-SSIS-Integrationslaufzeit in der Azure Data Factory](create-azure-ssis-integration-runtime.md).
* Die SSIS-Integrationslaufzeit wird gestoppt oder hat einen fehlerhaften Status. Wie Sie den Status und die Fehler der SSIS-Integrationslaufzeit überprüfen können, erfahren Sie unter [Azure-SSIS-Integrationslaufzeit](monitor-integration-runtime.md#azure-ssis-integration-runtime).

Wir empfehlen auch, dass Sie auf der Registerkarte **Allgemein** einen Timeout einstellen: ![Legen Sie die Eigenschaften auf der Registerkarte Allgemein fest.](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-general.png)

### <a name="poor-performance-in-package-execution"></a>Schlechte Leistung bei der Paketausführung

Probieren Sie diese Aktionen aus:

* Stellen Sie sicher, dass die Laufzeit der SSIS-Integration in der gleichen Region liegt wie die Datenquelle und das Ziel.

* Stellen Sie die Protokollierungsstufe der Paketausführung auf **Performance**, um Informationen zur Ausführungsdauer für jede Komponente zu sammeln. Weitere Informationen finden Sie unter Protokollierung von [Integration Services (SSIS)](https://docs.microsoft.com/sql/integration-services/performance/integration-services-ssis-logging).

* Überprüfen Sie die Leistung des IR-Knotens im Azure-Portal:
  * Informationen zur Überwachung der SSIS-Integrationslaufzeit finden Sie unter [Azure-SSIS-Integrationslaufzeit](monitor-integration-runtime.md#azure-ssis-integration-runtime).
  * Sie können die CPU-/Speicherhistorie für die SSIS-Integrationslaufzeit finden, indem Sie die Metriken der Data Factory im Azure-Portal anzeigen.
    ![Überwachung der Metriken der SSIS-Integrationslaufzeit](media/ssis-integration-runtime-ssis-activity-faq/monitor-metrics-ssis-integration-runtime.png)
