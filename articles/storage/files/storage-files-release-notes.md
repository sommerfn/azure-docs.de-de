---
title: Versionshinweise zum Azure-Dateisynchronisierungs-Agent | Microsoft-Dokumentation
description: Versionshinweise zum Azure-Dateisynchronisierungs-Agent
services: storage
author: wmgries
ms.service: storage
ms.topic: conceptual
ms.date: 10/8/2019
ms.author: wgries
ms.subservice: files
ms.openlocfilehash: 1b11c6beda5b261d1edc77dcbb365d8d8df8bd09
ms.sourcegitcommit: 42748f80351b336b7a5b6335786096da49febf6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2019
ms.locfileid: "72176711"
---
# <a name="release-notes-for-the-azure-file-sync-agent"></a>Versionshinweise zum Azure-Dateisynchronisierungs-Agent
Mit der Azure-Dateisynchronisierung können Sie Dateifreigaben Ihrer Organisation in Azure Files zentralisieren, ohne auf die Flexibilität, Leistung und Kompatibilität eines lokalen Dateiservers verzichten zu müssen. Ihre Windows Server-Installationen werden in einen schnellen Cache Ihrer Azure-Dateifreigabe transformiert. Sie können ein beliebiges Protokoll verwenden, das unter Windows Server verfügbar ist, um lokal auf Ihre Daten zuzugreifen (z.B. SMB, NFS und FTPS). Sie können weltweit so viele Caches wie nötig nutzen.

Dieser Artikel enthält die Versionshinweise für die unterstützten Versionen des Azure-Dateisynchronisierungs-Agents.

## <a name="supported-versions"></a>Unterstützte Versionen
Für den Azure-Dateisynchronisierungs-Agent werden die folgenden Versionen unterstützt:

| Meilenstein | Agent-Versionsnummer | Herausgabedatum | Status |
|----|----------------------|--------------|------------------|
| V8-Release – [KB4511224](https://support.microsoft.com/help/4511224)| 8.0.0.0 | 8\. Oktober 2019 | Unterstützt |
| Juli 2019 Updaterollup – [KB4490497](https://support.microsoft.com/help/4490497)| 7.2.0.0 | 24. Juli 2019 | Unterstützt |
| Juli 2019 Updaterollup – [KB4490496](https://support.microsoft.com/help/4490496)| 7.1.0.0 | 12. Juli 2019 | Unterstützt |
| V7-Release: [KB4490495](https://support.microsoft.com/help/4490495)| 7.0.0.0 | 19. Juni 2019 | Unterstützt |
| Updaterollup von Juni 2019: [KB4489739](https://support.microsoft.com/help/4489739)| 6.3.0.0 | 27. Juni 2019 | Unterstützt |
| Updaterollup von Juni 2019: [KB4489738](https://support.microsoft.com/help/4489738)| 6.2.0.0 | 13. Juni 2019 | Unterstützt |
| Mai 2019 Updaterollup – [KB4489737](https://support.microsoft.com/help/4489737)| 6.1.0.0 | 7\. Mai 2019 | Unterstützt |
| V6 Release - [KB4489736](https://support.microsoft.com/help/4489736)| 6.0.0.0 | 21. April 2019 | Unterstützt |
| Updaterollup von April 2019: [KB4481061](https://support.microsoft.com/help/4481061)| 5.2.0.0 | 4\. April 2019 | Unterstützt |
| Updaterollup von März 2019: [KB4481060](https://support.microsoft.com/help/4481060)| 5.1.0.0 | 7\. März 2019 | Unterstützt |
| V5 Release – [KB4459989](https://support.microsoft.com/help/4459989)| 5.0.2.0 | 12. Februar 2019 | Unterstützt |
| Updaterollup von Januar 2019: [KB4481059](https://support.microsoft.com/help/4481059)| 4.3.0.0 | 14. Januar 2019 | Unterstützt – Agent-Version läuft am 5. November 2019 ab. |
| Updaterollup von Dezember 2018: [KB4459990](https://support.microsoft.com/help/4459990)| 4.2.0.0 | 10. Dezember 2018 | Unterstützt – Agent-Version läuft am 5. November 2019 ab. |
| Updaterollup von Dezember 2018 | 4.1.0.0 | 4\. Dezember 2018 | Unterstützt – Agent-Version läuft am 5. November 2019 ab. |
| Release V4 | 4.0.1.0 | 13. November 2018 | Unterstützt – Agent-Version läuft am 5. November 2019 ab. |
| Release V3 | 3.1.0.0 – 3.4.0.0 | Nicht unterstützt | Nicht unterstützt – Agent-Versionen sind am 19. August 2019 abgelaufen. |
| Pre-GA-Agents | 1.1.0.0 – 3.0.13.0 | – | Nicht unterstützt – Agent-Versionen sind am 1. Oktober 2018 abgelaufen. |

### <a name="azure-file-sync-agent-update-policy"></a>Updaterichtlinie für den Azure-Dateisynchronisierungs-Agent
[!INCLUDE [storage-sync-files-agent-update-policy](../../../includes/storage-sync-files-agent-update-policy.md)]

## <a name="agent-version-8000"></a>Agent-Version 8.0.0.0
Die folgenden Versionshinweise gelten für Version 8.0.0.0 des Azure-Dateisynchronisierungs-Agents (Veröffentlichung: 8. Oktober 2019).

### <a name="improvements-and-issues-that-are-fixed"></a>Verbesserungen und behobene Probleme

- Leistungsverbesserungen beim Wiederherstellen
    - Schnellere Wiederherstellungszeiten bei einer Wiederherstellung über Azure Backup. Wiederhergestellte Dateien werden auf Azure-Dateisynchronisierungsservern viel schneller wieder synchronisiert. 
- Verbesserte Benutzeroberfläche des Portals für Cloudtiering  
    - Wenn Sie mehrstufige Dateien haben, deren Abruf fehlschlägt, können Sie jetzt die Abruffehler in den Eigenschaften des Serverendpunkts anzeigen. Außerdem werden bei der Integrität der Serverendpunkte jetzt ein Fehler und Schritte zur Entschärfung angezeigt, wenn der Filtertreiber für das Cloudtiering nicht auf den Server geladen wurde.
- Einfachere Agentinstallation
    - Das PowerShell-Modul „Az\AzureRM“ ist zum Registrieren des Servers nicht mehr erforderlich, sodass die Installation einfacher und schnell durchgeführt werden kann.
- Sonstige Verbesserungen bei Leistung und Zuverlässigkeit

### <a name="evaluation-tool"></a>Auswertungstool
Vor der Bereitstellung der Azure-Dateisynchronisierung müssen Sie mit dem Auswertungstool für die Azure-Dateisynchronisierung auswerten, ob Kompatibilität mit Ihrem System gegeben ist. Dieses Tool ist ein Azure PowerShell-Cmdlet, das auf potenzielle Probleme mit Ihrem Dateisystem und Dataset prüft, z.B. nicht unterstützte Zeichen oder eine nicht unterstützte Betriebssystemversion. Anweisungen zur Installation und Verwendung finden Sie im Planungshandbuch im Abschnitt [Auswertungstools](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning#evaluation-cmdlet). 

### <a name="agent-installation-and-server-configuration"></a>Agent-Installation und Serverkonfiguration
Weitere Informationen zum Installieren und Konfigurieren des Azure File Sync-Agents mit Windows Server finden Sie unter [Planen einer Bereitstellung der Azure-Dateisynchronisierung (Vorschau)](storage-sync-files-planning.md) sowie unter [Bereitstellen von Azure File Sync (Vorschau)](storage-sync-files-deployment-guide.md).

- Das Agent-Installationspaket muss mit erhöhten Berechtigungen (Administratorberechtigungen) installiert werden.
- Der Agent wird für die Bereitstellungsoption „Nano Server“ nicht unterstützt.
- Der Agent wird nur unter Windows Server 2019, Windows Server 2016 und Windows Server 2012 R2 unterstützt.
- Der Agent benötigt mindestens 2 GiB Arbeitsspeicher. Wenn der Server auf einem virtuellen Computer ausgeführt wird, für den dynamischer Arbeitsspeicher aktiviert ist, muss der virtuelle Computer mit mindestens 2.048 MiB Arbeitsspeicher konfiguriert werden.
- Der Dienst „Storage-Synchronisierungs-Agent“ (FileSyncSvc) unterstützt keine Serverendpunkte, die sich auf einem Volume befinden, für das das Verzeichnis „System Volume Information“ (SVI) komprimiert ist. Diese Konfiguration führt zu unerwarteten Ergebnissen.

### <a name="interoperability"></a>Interoperabilität
- Virenschutz, Sicherung und andere Anwendungen, die auf Tieringdateien zugreifen, können zu unerwünschten Rückrufen führen, wenn sie das Offlineattribut nicht berücksichtigen und das Lesen des Inhalts dieser Dateien nicht überspringen. Weitere Informationen finden Sie unter [Problembehandlung bei der Azure-Dateisynchronisierung (Vorschau)](storage-sync-files-troubleshoot.md).
- FSRM-Dateiprüfungen (File Server Resource Manager, Ressourcen-Manager für Dateiserver) können zu Fehlern aufgrund einer endlosen Synchronisierung führen, wenn Dateien aufgrund der damit verbundenen Vorgänge blockiert werden.
- Die Ausführung von Sysprep auf einem Server, auf dem der Azure-Dateisynchronisierungs-Agent installiert ist, wird nicht unterstützt und kann zu unerwarteten Ergebnissen führen. Der Azure-Dateisynchronisierungs-Agent sollte installiert werden, nachdem das Serverimage bereitgestellt und das Sysprep-Mini-Setup abgeschlossen wurde.

### <a name="sync-limitations"></a>Einschränkungen bei der Synchronisierung
Folgende Elemente werden nicht synchronisiert, aber der restliche Systembetrieb ist nicht beeinträchtigt:
- Dateien mit nicht unterstützten Zeichen. Eine Liste mit den Zeichen, die nicht unterstützt werden, finden Sie im [Leitfaden zur Problembehandlung](storage-sync-files-troubleshoot.md#handling-unsupported-characters).
- Dateien oder Verzeichnisse, die mit einem Punkt enden.
- Pfade, die länger als 2.048 Zeichen sind.
- Der DACL-Teil (besitzerverwaltete Zugriffssteuerungsliste) einer Sicherheitsbeschreibung, sofern dieser größer als 2 KB ist. (Dieses Problem trifft nur zu, wenn Sie für ein einzelnes Element über mehr als ca. 40 Zugriffssteuerungseinträge verfügen.)
- Der SACL-Teil (System-Zugriffssteuerungsliste) einer Sicherheitsbeschreibung, die für die Überwachung verwendet wird.
- Erweiterte Attribute
- Alternative Datenströme
- Analysepunkte
- Feste Links
- Die Komprimierung (sofern für eine Serverdatei festgelegt) wird nicht beibehalten, wenn Änderungen mit der Datei von anderen Endpunkten synchronisiert werden.
- Mit EFS (oder einer anderen Benutzermodusverschlüsselung) verschlüsselte Dateien, die den Dienst am Lesen der Daten hindern.

    > [!Note]  
    > Bei der Azure-Dateisynchronisierung werden Daten während der Übertragung immer verschlüsselt. Ruhende Daten werden in Azure immer verschlüsselt.
 
### <a name="server-endpoint"></a>Serverendpunkt
- Ein Serverendpunkt kann nur auf einem NTFS-Volume erstellt werden. ReFS, FAT, FAT32 und andere Dateisysteme werden von der Azure-Dateisynchronisierung derzeit nicht unterstützt.
- Auf mehrstufige Dateien kann nicht mehr zugegriffen werden, wenn für die Dateien vor dem Löschen des Serverendpunkts kein Rückruf erfolgt. Erstellen Sie den Serverendpunkt neu, um den Zugriff auf die Dateien wiederherzustellen. Nachdem 30 Tage vergangen sind, seitdem der Serverendpunkt gelöscht wurde, oder wenn der Cloudendpunkt gelöscht wurde, sind mehrstufige Dateien, für die kein Rückruf durchgeführt wurde, nicht mehr verwendbar. Weitere Informationen finden Sie unter [Auf mehrstufige Dateien auf dem Server kann nach dem Löschen eines Serverendpunkts nicht zugegriffen werden](https://docs.microsoft.com/en-us/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cazure-portal#tiered-files-are-not-accessible-on-the-server-after-deleting-a-server-endpoint).
- Das Cloudtiering wird auf dem Systemvolume nicht unterstützt. Um einen Serverendpunkt auf dem Systemvolume zu erstellen, deaktivieren Sie Cloudtiering, wenn Sie den Serverendpunkt erstellen.
- Failoverclustering wird nur mit Clusterdatenträgern, aber nicht mit freigegebenen Clustervolumes (Cluster Shared Volumes, CSVs) unterstützt.
- Ein Serverendpunkt kann nicht geschachtelt werden. Er kann auf demselben Volume parallel zu einem anderen Endpunkt vorhanden sein (Koexistenz).
- Speichern Sie keine Betriebssystem- oder Anwendungsauslagerungsdatei an einem Serverendpunkt-Standort.
- Der Servername im Portal wird bei Umbenennung des Servers nicht aktualisiert.

### <a name="cloud-endpoint"></a>Cloudendpunkt
- Die Azure-Dateisynchronisierung unterstützt direkte Änderungen an der Azure-Dateifreigabe. Allerdings müssen alle Änderungen, die Sie an der Azure-Dateifreigabe vornehmen, zuerst von einem Azure-Dateisynchronisierungsauftrag zum Erkennen von Änderungen erkannt werden. Ein Auftrag zum Erkennen von Änderungen für einen Cloudendpunkt wird einmal alle 24 Stunden gestartet. Um Dateien, die in der Azure-Dateifreigabe geändert wurden, sofort zu synchronisieren, kann das PowerShell-Cmdlet [Invoke-AzStorageSyncChangeDetection](https://docs.microsoft.com/powershell/module/az.storagesync/invoke-azstoragesyncchangedetection) verwendet werden, um die Erkennung von Änderungen in der Azure-Dateifreigabe manuell auszulösen. Darüber hinaus bewirken Änderungen, die über das REST-Protokoll an einer Azure-Dateifreigabe vorgenommen wurden, keine Aktualisierung der letzten SMB-Änderungszeit, und die Änderungen sind für eine Synchronisierung nicht zu sehen.
- Der Speichersynchronisierungsdienst und/oder das Speicherkonto kann in eine andere Ressourcengruppe oder ein anderes Abonnement im vorhandenen Azure AD-Mandanten verschoben werden. Wenn das Speicherkonto verschoben wird, müssen Sie dem Hybrid-Dateisynchronisierungsdienst Zugriff auf das Speicherkonto gewähren (siehe [Sicherstellen, dass die Azure-Dateisynchronisierung Zugriff auf das Speicherkonto besitzt](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cportal#troubleshoot-rbac)).

    > [!Note]  
    > Die Azure-Dateisynchronisierung unterstützt nicht das Verschieben des Abonnements in einen anderen Azure AD-Mandanten.

### <a name="cloud-tiering"></a>Cloudtiering
- Wenn eine Tieringdatei mit Robocopy an einen anderen Speicherort kopiert wird, ist die sich ergebende Datei keine Tieringdatei. Das Offlineattribut kann festgelegt werden, da dieses Attribut fälschlicherweise von Robocopy in Kopiervorgänge eingefügt wird.
- Verwenden Sie beim Kopieren der Dateien mit Robocopy die Option „/MIR“, um Dateizeitstempel beizubehalten. So wird sichergestellt, dass das Tiering für ältere Dateien früher als für die Dateien durchgeführt wird, auf die zuletzt zugegriffen wurde.

## <a name="agent-version-7200"></a>Agent-Version 7.2.0.0
Die folgenden Versionshinweise gelten für Version 7.2.0.0 des Azure-Dateisynchronisierungs-Agents (Veröffentlichung: 24. Juli 2019). Diese Hinweise gelten zusätzlich zu den Versionshinweisen, die für die Version 7.0.0.0 angegeben sind.

Liste der in dieser Version behobenem Probleme:  
- Der Speichersynchronisierungs-Agent (FileSyncSvc) stürzt ab, wenn die Proxykonfiguration NULL ist.
- Der Serverendpunkt startet BCDR (Fehler: 0x80c80257 – ECS_E_BCDR_IN_PROGRESS), wenn mehrere Endpunkte auf dem Server den gleichen Namen haben.
- Verbesserte Zuverlässigkeit des Cloudtierings

## <a name="agent-version-7100"></a>Agent-Version 7.1.0.0
Die folgenden Versionshinweise gelten für Version 7.1.0.0 des Azure-Dateisynchronisierungs-Agents (Veröffentlichung: 12. Juli 2019). Diese Hinweise gelten zusätzlich zu den Versionshinweisen, die für die Version 7.0.0.0 angegeben sind.

Liste der in dieser Version behobenem Probleme:  
- Zugreifen auf oder Durchsuchen eines Serverendpunkt-Speicherorts per SMB ist unter Windows Server 2012 R2 langsam. 
- Höhere CPU-Auslastung nach Installation von Version 6 des Azure-Dateisynchronisierungs-Agents
- Verbesserungen bei Cloudtiering-Telemetriedaten
- Verschiedene Zuverlässigkeitverbesserungen für Cloudtiering und Synchronisierung.

## <a name="agent-version-7000"></a>Agent-Version 7.0.0.0
Die folgenden Versionshinweise gelten für Version 7.0.0.0 des Azure-Dateisynchronisierungs-Agents (veröffentlicht am 19. Juni 2019).

### <a name="improvements-and-issues-that-are-fixed"></a>Verbesserungen und behobene Probleme

- Unterstützung größerer Dateifreigaben
    - Mit der Vorschauversion für größere Azure-Dateifreigaben erhöhen wir auch unsere Supportgrenzwerte für die Dateisynchronisierung. In diesem ersten Schritt unterstützt die Azure-Dateisynchronisierung jetzt bis zu 25 TB und 50 Millionen Dateien pro Synchronisierungsnamespace. Füllen Sie das Formular unter https://aka.ms/azurefilesatscalesurvey aus, um sich für die Vorschauversion für große Dateifreigaben zu bewerben. 
- Unterstützung für Firewalleinstellungen und Einstellungen für ein virtuelles Netzwerk in Speicherkonten
    - Azure-Dateisynchronisierung unterstützt jetzt die Firewalleinstellungen und Einstellungen für ein virtuelles Netzwerk in Speicherkonten. Informationen dazu, wie Sie Ihre Bereitstellung konfigurieren müssen, damit sie mit den Firewalleinstellungen und den Einstellungen für ein virtuelles Netzwerk funktioniert, finden Sie unter [Konfigurieren von Firewall- und VNET-Einstellungen](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide?tabs=azure-portal#configure-firewall-and-virtual-network-settings).
- PowerShell-Cmdlet, um Dateien, die in der Azure-Dateifreigabe geändert wurden, sofort zu synchronisieren
    - Um Dateien, die in der Azure-Dateifreigabe geändert wurden, sofort zu synchronisieren, kann das PowerShell-Cmdlet „Invoke-AzStorageSyncChangeDetection“ verwendet werden, um die Erkennung von Änderungen in der Azure-Dateifreigabe manuell auszulösen. Dieses Cmdlet ist für Szenarien vorgesehen, in denen irgendein automatisierter Prozess Änderungen in der Azure-Dateifreigabe vornimmt oder die Änderungen von einem Administrator vorgenommen werden (etwa Verschieben von Dateien und Verzeichnissen in die Freigabe). Für Endbenutzeränderungen empfiehlt es sich, den Azure-Dateisynchronisierungs-Agent auf einem virtuellen IaaS-Computer zu installieren und Endbenutzern den Zugriff auf die Dateifreigabe über den virtuellen IaaS-Computer zu ermöglichen. Auf diese Weise werden alle Änderungen schnell mit anderen Agents synchronisiert, ohne dass das Cmdlet „Invoke-AzStorageSyncChangeDetection“ verwendet werden muss. Weitere Informationen hierzu finden Sie in der Dokumentation von [Invoke-AzStorageSyncChangeDetection](https://docs.microsoft.com/powershell/module/az.storagesync/invoke-azstoragesyncchangedetection).
- Verbessertes Portalverhalten, wenn Dateien vorliegen, die nicht synchronisiert sind
    - Wenn Sie Dateien haben, die nicht synchronisiert werden können, wird nun zwischen vorübergehenden und dauerhaften Fehlern im Portal unterschieden. Vorübergehende Fehler werden in der Regel automatisch aufgelöst, ohne dass eine Administratoraktion erforderlich ist. Beispielsweise wird eine Datei, die momentan verwendet wird, erst synchronisiert, nachdem das Dateihandle geschlossen wurde. Bei dauerhaften Fehlern wird nun die Anzahl der Dateien angezeigt, von jedem Fehler betroffen sind. Die Anzahl der dauerhaften Fehler wird auch in der „Dateien ohne Synchronisierung“-Spalte aller Serverendpunkte einer Synchronisierungsgruppe angezeigt.
- Verbesserte Azure Backup-Wiederherstellung auf Dateiebene
    - Einzelne Dateien, die mit Azure Backup wiederhergestellt werden, werden jetzt schneller erkannt und mit dem Serverendpunkt synchronisiert.
- Verbesserte Zuverlässigkeit des Cmdlets für Cloudtieringrückrufe 
    - Ähnlich wie bei Robocopy können Kunden jetzt mithilfe des Cmdlets „Invoke-StorageSyncFileRecall“ die Anzahl der Wiederholungsversuche pro Datei und die Wiederholungsverzögerung pro Datei angeben. Zuvor wurden mit diesem Cmdlet alle mehrstufigen Dateien unter einem angegebenen Pfad in zufälliger Reihenfolge abgerufen. Mit dem Parameter „new -Order“ ruft dieses Cmdlet die heißesten Daten zuerst ab und berücksichtigt die Cloudtiering-Richtlinie (Aufruf beenden, wenn die Datumsrichtlinie oder der freie Speicherplatz auf dem Volumen erfüllt ist – je nachdem, was zuerst geschieht).
- Unterstützung nur für TLS 1.2 (TLS 1.0 und 1.1 sind deaktiviert)
    - Die Azure-Dateisynchronisierung unterstützt jetzt die Verwendung von TLS 1.2 nur auf Servern, für die TLS 1.0 und 1.1 deaktiviert wurden. Vor dieser Verbesserung ist bei der Serverregistrierung ein Fehler aufgetreten, wenn TLS 1.0 und 1.1 auf dem Server deaktiviert war.
- Verschiedene Verbesserungen in Bezug auf die Leistung und Zuverlässigkeit für die Synchronisierung und das Cloudtiering
    - Dieses Release enthält mehrere Verbesserungen in Bezug auf die Zuverlässigkeit und Leistung. Mit einigen Maßnahmen soll erreicht werden, dass das Cloudtiering effizienter wird und die Azure-Dateisynchronisierung insgesamt in diesen Situationen besser funktioniert, wenn Sie einen Zeitplan für die Bandbreitendrosselung festgelegt haben.

### <a name="evaluation-tool"></a>Auswertungstool
Vor der Bereitstellung der Azure-Dateisynchronisierung müssen Sie mit dem Auswertungstool für die Azure-Dateisynchronisierung auswerten, ob Kompatibilität mit Ihrem System gegeben ist. Dieses Tool ist ein Azure PowerShell-Cmdlet, das auf potenzielle Probleme mit Ihrem Dateisystem und Dataset prüft, z.B. nicht unterstützte Zeichen oder eine nicht unterstützte Betriebssystemversion. Anweisungen zur Installation und Verwendung finden Sie im Planungshandbuch im Abschnitt [Auswertungstools](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning#evaluation-cmdlet). 

### <a name="agent-installation-and-server-configuration"></a>Agent-Installation und Serverkonfiguration
Weitere Informationen zum Installieren und Konfigurieren des Azure File Sync-Agents mit Windows Server finden Sie unter [Planen einer Bereitstellung der Azure-Dateisynchronisierung (Vorschau)](storage-sync-files-planning.md) sowie unter [Bereitstellen von Azure File Sync (Vorschau)](storage-sync-files-deployment-guide.md).

- Das Agent-Installationspaket muss mit erhöhten Berechtigungen (Administratorberechtigungen) installiert werden.
- Der Agent wird für die Bereitstellungsoption „Nano Server“ nicht unterstützt.
- Der Agent wird nur unter Windows Server 2019, Windows Server 2016 und Windows Server 2012 R2 unterstützt.
- Der Agent benötigt mindestens 2 GiB Arbeitsspeicher. Wenn der Server auf einem virtuellen Computer ausgeführt wird, für den dynamischer Arbeitsspeicher aktiviert ist, muss der virtuelle Computer mit mindestens 2.048 MiB Arbeitsspeicher konfiguriert werden.
- Der Dienst „Storage-Synchronisierungs-Agent“ (FileSyncSvc) unterstützt keine Serverendpunkte, die sich auf einem Volume befinden, für das das Verzeichnis „System Volume Information“ (SVI) komprimiert ist. Diese Konfiguration führt zu unerwarteten Ergebnissen.

### <a name="interoperability"></a>Interoperabilität
- Virenschutz, Sicherung und andere Anwendungen, die auf Tieringdateien zugreifen, können zu unerwünschten Rückrufen führen, wenn sie das Offlineattribut nicht berücksichtigen und das Lesen des Inhalts dieser Dateien nicht überspringen. Weitere Informationen finden Sie unter [Problembehandlung bei der Azure-Dateisynchronisierung (Vorschau)](storage-sync-files-troubleshoot.md).
- FSRM-Dateiprüfungen (File Server Resource Manager, Ressourcen-Manager für Dateiserver) können zu Fehlern aufgrund einer endlosen Synchronisierung führen, wenn Dateien aufgrund der damit verbundenen Vorgänge blockiert werden.
- Die Ausführung von Sysprep auf einem Server, auf dem der Azure-Dateisynchronisierungs-Agent installiert ist, wird nicht unterstützt und kann zu unerwarteten Ergebnissen führen. Der Azure-Dateisynchronisierungs-Agent sollte installiert werden, nachdem das Serverimage bereitgestellt und das Sysprep-Mini-Setup abgeschlossen wurde.

### <a name="sync-limitations"></a>Einschränkungen bei der Synchronisierung
Folgende Elemente werden nicht synchronisiert, aber der restliche Systembetrieb ist nicht beeinträchtigt:
- Dateien mit nicht unterstützten Zeichen. Eine Liste mit den Zeichen, die nicht unterstützt werden, finden Sie im [Leitfaden zur Problembehandlung](storage-sync-files-troubleshoot.md#handling-unsupported-characters).
- Dateien oder Verzeichnisse, die mit einem Punkt enden.
- Pfade, die länger als 2.048 Zeichen sind.
- Der DACL-Teil (besitzerverwaltete Zugriffssteuerungsliste) einer Sicherheitsbeschreibung, sofern dieser größer als 2 KB ist. (Dieses Problem trifft nur zu, wenn Sie für ein einzelnes Element über mehr als ca. 40 Zugriffssteuerungseinträge verfügen.)
- Der SACL-Teil (System-Zugriffssteuerungsliste) einer Sicherheitsbeschreibung, die für die Überwachung verwendet wird.
- Erweiterte Attribute
- Alternative Datenströme
- Analysepunkte
- Feste Links
- Die Komprimierung (sofern für eine Serverdatei festgelegt) wird nicht beibehalten, wenn Änderungen mit der Datei von anderen Endpunkten synchronisiert werden.
- Mit EFS (oder einer anderen Benutzermodusverschlüsselung) verschlüsselte Dateien, die den Dienst am Lesen der Daten hindern.

    > [!Note]  
    > Bei der Azure-Dateisynchronisierung werden Daten während der Übertragung immer verschlüsselt. Ruhende Daten werden in Azure immer verschlüsselt.
 
### <a name="server-endpoint"></a>Serverendpunkt
- Ein Serverendpunkt kann nur auf einem NTFS-Volume erstellt werden. ReFS, FAT, FAT32 und andere Dateisysteme werden von der Azure-Dateisynchronisierung derzeit nicht unterstützt.
- Auf mehrstufige Dateien kann nicht mehr zugegriffen werden, wenn für die Dateien vor dem Löschen des Serverendpunkts kein Rückruf erfolgt. Erstellen Sie den Serverendpunkt neu, um den Zugriff auf die Dateien wiederherzustellen. Nachdem 30 Tage vergangen sind, seitdem der Serverendpunkt gelöscht wurde, oder wenn der Cloudendpunkt gelöscht wurde, sind mehrstufige Dateien, für die kein Rückruf durchgeführt wurde, nicht mehr verwendbar.
- Das Cloudtiering wird auf dem Systemvolume nicht unterstützt. Um einen Serverendpunkt auf dem Systemvolume zu erstellen, deaktivieren Sie Cloudtiering, wenn Sie den Serverendpunkt erstellen.
- Failoverclustering wird nur mit Clusterdatenträgern, aber nicht mit freigegebenen Clustervolumes (Cluster Shared Volumes, CSVs) unterstützt.
- Ein Serverendpunkt kann nicht geschachtelt werden. Er kann auf demselben Volume parallel zu einem anderen Endpunkt vorhanden sein (Koexistenz).
- Speichern Sie keine Betriebssystem- oder Anwendungsauslagerungsdatei an einem Serverendpunkt-Standort.
- Der Servername im Portal wird bei Umbenennung des Servers nicht aktualisiert.

### <a name="cloud-endpoint"></a>Cloudendpunkt
- Die Azure-Dateisynchronisierung unterstützt direkte Änderungen an der Azure-Dateifreigabe. Allerdings müssen alle Änderungen, die Sie an der Azure-Dateifreigabe vornehmen, zuerst von einem Azure-Dateisynchronisierungsauftrag zum Erkennen von Änderungen erkannt werden. Ein Auftrag zum Erkennen von Änderungen für einen Cloudendpunkt wird einmal alle 24 Stunden gestartet. Darüber hinaus bewirken Änderungen, die über das REST-Protokoll an einer Azure-Dateifreigabe vorgenommen wurden, keine Aktualisierung der letzten SMB-Änderungszeit, und die Änderungen sind für eine Synchronisierung nicht zu sehen.
- Der Speichersynchronisierungsdienst und/oder das Speicherkonto kann in eine andere Ressourcengruppe oder ein anderes Abonnement im vorhandenen Azure AD-Mandanten verschoben werden. Wenn das Speicherkonto verschoben wird, müssen Sie dem Hybrid-Dateisynchronisierungsdienst Zugriff auf das Speicherkonto gewähren (siehe [Sicherstellen, dass die Azure-Dateisynchronisierung Zugriff auf das Speicherkonto besitzt](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cportal#troubleshoot-rbac)).

    > [!Note]  
    > Die Azure-Dateisynchronisierung unterstützt nicht das Verschieben des Abonnements in einen anderen Azure AD-Mandanten.

### <a name="cloud-tiering"></a>Cloudtiering
- Wenn eine Tieringdatei mit Robocopy an einen anderen Speicherort kopiert wird, ist die sich ergebende Datei keine Tieringdatei. Das Offlineattribut kann festgelegt werden, da dieses Attribut fälschlicherweise von Robocopy in Kopiervorgänge eingefügt wird.
- Verwenden Sie beim Kopieren der Dateien mit Robocopy die Option „/MIR“, um Dateizeitstempel beizubehalten. So wird sichergestellt, dass das Tiering für ältere Dateien früher als für die Dateien durchgeführt wird, auf die zuletzt zugegriffen wurde.

## <a name="agent-version-6300"></a>Agent-Version 6.3.0.0
Die folgenden Versionshinweise gelten für Version 6.3.0.0 des Azure-Dateisynchronisierungs-Agents, die am 27. Juni 2019 veröffentlicht wurde. Diese Hinweise gelten zusätzlich zu den Versionshinweisen, die für die Version 6.0.0.0 angegeben sind.

Liste der in dieser Version behobenem Probleme:  
- Zugreifen auf oder Durchsuchen eines Serverendpunkt-Speicherorts per SMB ist unter Windows Server 2012 R2 langsam 
- Höhere CPU-Auslastung nach Installation von Version 6 des Azure-Dateisynchronisierungs-Agents
- Verbesserungen bei Cloudtiering-Telemetriedaten

## <a name="agent-version-6200"></a>Agent-Version 6.2.0.0
Die folgenden Versionshinweise gelten für Version 6.2.0.0 des Azure-Dateisynchronisierungs-Agents, die am 13. Juni 2019 veröffentlicht wurde. Diese Hinweise gelten zusätzlich zu den Versionshinweisen, die für die Version 6.0.0.0 angegeben sind.

Liste der in dieser Version behobenem Probleme:  
- Nach dem Erstellen eines Serverendpunkts kann es zu einer hohen CPU-Auslastung kommen, wenn per Hintergrundrückruf Dateien auf den Server heruntergeladen werden
- Für Synchronisierungs- und Cloudtieringvorgänge tritt aufgrund eines Tokenablaufs ggf. der Fehler ECS_E_SERVER_CREDENTIAL_NEEDED auf
- Beim Rückruf einer Datei tritt ggf. ein Fehler auf, wenn die URL zum Herunterladen der Datei reservierte Zeichen enthält 

## <a name="agent-version-6100"></a>Agent-Version 6.1.0.0
Die folgenden Versionshinweise gelten für Version 6.1.0.0 des Azure-Dateisynchronisierungs-Agents, die am 6. Mai 2019 veröffentlicht wurde. Diese Hinweise gelten zusätzlich zu den Versionshinweisen, die für die Version 6.0.0.0 angegeben sind.

Liste der in dieser Version behobenem Probleme:  
- Behoben: In Windows Admin Center konnte die Version des Agents und die Konfiguration des Serverendpunkts auf Servern nicht angezeigt werden, auf denen die Version 6.0 des Azure-Dateisynchronisierungs-Agents installiert ist.

## <a name="agent-version-6000"></a>Agent-Version 6.0.0.0
Die folgenden Versionshinweise gelten für Version 6.0.0.0 des Azure-Dateisynchronisierungs-Agents, die am 22. April 2019 veröffentlicht wurde.

### <a name="improvements-and-issues-that-are-fixed"></a>Verbesserungen und behobene Probleme

- Unterstützung für automatische Updates des Agents
  - Gemäß Ihrem Feedback haben wir dem Azure-Dateisynchronisierungsserver-Agent ein Feature für automatische Updates hinzugefügt. Weitere Informationen finden Sie unter [Updaterichtlinie für den Azure-Dateisynchronisierungs-Agent](https://docs.microsoft.com/azure/storage/files/storage-files-release-notes#azure-file-sync-agent-update-policy).
- ACL-Unterstützung für Azure-Dateifreigabe
  - Die Azure-Dateisynchronisierung hat schon immer das Synchronisieren von ACLs zwischen Serverendpunkten unterstützt. Die ACLs wurden bisher jedoch nicht mit dem Cloudendpunkt synchronisiert (Azure-Dateifreigabe). Dieses Release unterstützt nun die Synchronisierung von ACLs zwischen Server- und Cloudendpunkten.
- Paralleles Hochladen und Herunterladen während Synchronisierungssitzungen für einen Serverendpunkt 
  - Serverendpunkte unterstützen nun das parallele Hoch- und Herunterladen von Dateien. Sie müssen also nicht mehr warten, bis ein Download abgeschlossen ist, bis Dateien in die Azure-Dateifreigabe hochgeladen werden können. 
- Neue Cloudtiering-Cmdlets für den Abruf des Volumens und des Tieringstatus
  - Mithilfe von zwei neuen, für den Server lokalen PowerShell-Cmdlets können Sie Informationen zum Cloudtiering und Dateiabruf erhalten. Damit stehen Protokollinformationen aus zwei Ereigniskanälen auf dem Server zur Verfügung:
    - „Get-StorageSyncFileTieringResult“ listet alle Dateien mit zugehörigen Pfaden auf, für die noch kein Tiering durchgeführt wurde, und führt den Grund dafür auf.
    - „Get-StorageSyncFileRecallResult“ erstellt einen Bericht über alle Dateiabrufereignisse. Jede abgerufene Datei wird zusammen mit ihrem Pfad aufgeführt. Außerdem wird angegeben, ob der Abruf erfolgreich war oder zu einem Fehler führte.
  - Standardmäßig können beide Ereigniskanäle jeweils bis zu 1 MB speichern. Sie können die Anzahl der Dateien, für die ein Bericht erstellt wird, erhöhen, indem Sie die Größe der Ereigniskanäle erhöhen.
- Unterstützung für den FIPS-Modus
  - Die Azure-Dateisynchronisierung unterstützt nun das Aktivieren des FIPS-Modus auf Servern, auf denen der Azure-Dateisynchronisierungs-Agent installiert ist.
    - Bevor Sie den FIPS-Modus auf Ihrem Server aktivieren, installieren Sie den Azure-Dateisynchronisierungs-Agent und das [PackageManagement-Modul](https://www.powershellgallery.com/packages/PackageManagement/1.1.7.2) auf Ihrem Server. Wenn der FIPS-Modus bereits auf Ihrem Server aktiviert wurde, können Sie das [PackageManagement-Modul](https://www.powershellgallery.com/packages/PackageManagement/1.1.7.2) [manuell auf Ihren Server herunterladen](https://docs.microsoft.com/powershell/gallery/how-to/working-with-packages/manual-download).
- Verschiedene Zuverlässigkeitverbesserungen für Cloudtiering und Synchronisierung

### <a name="evaluation-tool"></a>Auswertungstool
Vor der Bereitstellung der Azure-Dateisynchronisierung müssen Sie mit dem Auswertungstool für die Azure-Dateisynchronisierung auswerten, ob Kompatibilität mit Ihrem System gegeben ist. Dieses Tool ist ein Azure PowerShell-Cmdlet, das auf potenzielle Probleme mit Ihrem Dateisystem und Dataset prüft, z.B. nicht unterstützte Zeichen oder eine nicht unterstützte Betriebssystemversion. Anweisungen zur Installation und Verwendung finden Sie im Planungshandbuch im Abschnitt [Auswertungstools](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning#evaluation-cmdlet). 

### <a name="agent-installation-and-server-configuration"></a>Agent-Installation und Serverkonfiguration
Weitere Informationen zum Installieren und Konfigurieren des Azure File Sync-Agents mit Windows Server finden Sie unter [Planen einer Bereitstellung der Azure-Dateisynchronisierung (Vorschau)](storage-sync-files-planning.md) sowie unter [Bereitstellen von Azure File Sync (Vorschau)](storage-sync-files-deployment-guide.md).

- Das Agent-Installationspaket muss mit erhöhten Berechtigungen (Administratorberechtigungen) installiert werden.
- Der Agent wird für die Bereitstellungsoption „Nano Server“ nicht unterstützt.
- Der Agent wird nur unter Windows Server 2019, Windows Server 2016 und Windows Server 2012 R2 unterstützt.
- Der Agent benötigt mindestens 2 GiB Arbeitsspeicher. Wenn der Server auf einem virtuellen Computer ausgeführt wird, für den dynamischer Arbeitsspeicher aktiviert ist, muss der virtuelle Computer mit mindestens 2.048 MiB Arbeitsspeicher konfiguriert werden.
- Der Dienst „Storage-Synchronisierungs-Agent“ (FileSyncSvc) unterstützt keine Serverendpunkte, die sich auf einem Volume befinden, für das das Verzeichnis „System Volume Information“ (SVI) komprimiert ist. Diese Konfiguration führt zu unerwarteten Ergebnissen.

### <a name="interoperability"></a>Interoperabilität
- Virenschutz, Sicherung und andere Anwendungen, die auf Tieringdateien zugreifen, können zu unerwünschten Rückrufen führen, wenn sie das Offlineattribut nicht berücksichtigen und das Lesen des Inhalts dieser Dateien nicht überspringen. Weitere Informationen finden Sie unter [Problembehandlung bei der Azure-Dateisynchronisierung (Vorschau)](storage-sync-files-troubleshoot.md).
- FSRM-Dateiprüfungen (File Server Resource Manager, Ressourcen-Manager für Dateiserver) können zu Fehlern aufgrund einer endlosen Synchronisierung führen, wenn Dateien aufgrund der damit verbundenen Vorgänge blockiert werden.
- Die Ausführung von Sysprep auf einem Server, für den der Azure-Dateisynchronisierungs-Agent installiert ist, wird nicht unterstützt und kann zu unerwarteten Ergebnissen führen. Der Azure-Dateisynchronisierungs-Agent sollte installiert werden, nachdem das Serverimage bereitgestellt und das Sysprep-Mini-Setup abgeschlossen wurde.

### <a name="sync-limitations"></a>Einschränkungen bei der Synchronisierung
Folgende Elemente werden nicht synchronisiert, aber der restliche Systembetrieb ist nicht beeinträchtigt:
- Dateien mit nicht unterstützten Zeichen. Eine Liste mit den Zeichen, die nicht unterstützt werden, finden Sie im [Leitfaden zur Problembehandlung](storage-sync-files-troubleshoot.md#handling-unsupported-characters).
- Dateien oder Verzeichnisse, die mit einem Punkt enden.
- Pfade, die länger als 2.048 Zeichen sind.
- Der DACL-Teil (besitzerverwaltete Zugriffssteuerungsliste) einer Sicherheitsbeschreibung, sofern dieser größer als 2 KB ist. (Dieses Problem trifft nur zu, wenn Sie für ein einzelnes Element über mehr als ca. 40 Zugriffssteuerungseinträge verfügen.)
- Der SACL-Teil (System-Zugriffssteuerungsliste) einer Sicherheitsbeschreibung, die für die Überwachung verwendet wird.
- Erweiterte Attribute
- Alternative Datenströme
- Analysepunkte
- Feste Links
- Die Komprimierung (sofern für eine Serverdatei festgelegt) wird nicht beibehalten, wenn Änderungen mit der Datei von anderen Endpunkten synchronisiert werden.
- Mit EFS (oder einer anderen Benutzermodusverschlüsselung) verschlüsselte Dateien, die den Dienst am Lesen der Daten hindern.

    > [!Note]  
    > Bei der Azure-Dateisynchronisierung werden Daten während der Übertragung immer verschlüsselt. Ruhende Daten werden in Azure immer verschlüsselt.
 
### <a name="server-endpoint"></a>Serverendpunkt
- Ein Serverendpunkt kann nur auf einem NTFS-Volume erstellt werden. ReFS, FAT, FAT32 und andere Dateisysteme werden von der Azure-Dateisynchronisierung derzeit nicht unterstützt.
- Auf mehrstufige Dateien kann nicht mehr zugegriffen werden, wenn für die Dateien vor dem Löschen des Serverendpunkts kein Rückruf erfolgt. Erstellen Sie den Serverendpunkt neu, um den Zugriff auf die Dateien wiederherzustellen. Nachdem 30 Tage vergangen sind, seitdem der Serverendpunkt gelöscht wurde, oder wenn der Cloudendpunkt gelöscht wurde, sind mehrstufige Dateien, für die kein Rückruf durchgeführt wurde, nicht mehr verwendbar.
- Das Cloudtiering wird auf dem Systemvolume nicht unterstützt. Um einen Serverendpunkt auf dem Systemvolume zu erstellen, deaktivieren Sie Cloudtiering, wenn Sie den Serverendpunkt erstellen.
- Failoverclustering wird nur mit Clusterdatenträgern, aber nicht mit freigegebenen Clustervolumes (Cluster Shared Volumes, CSVs) unterstützt.
- Ein Serverendpunkt kann nicht geschachtelt werden. Er kann auf demselben Volume parallel zu einem anderen Endpunkt vorhanden sein (Koexistenz).
- Speichern Sie keine Betriebssystem- oder Anwendungsauslagerungsdatei an einem Serverendpunkt-Standort.
- Der Servername im Portal wird bei Umbenennung des Servers nicht aktualisiert.

### <a name="cloud-endpoint"></a>Cloudendpunkt
- Die Azure-Dateisynchronisierung unterstützt direkte Änderungen an der Azure-Dateifreigabe. Allerdings müssen alle Änderungen, die Sie an der Azure-Dateifreigabe vornehmen, zuerst von einem Azure-Dateisynchronisierungsauftrag zum Erkennen von Änderungen erkannt werden. Ein Auftrag zum Erkennen von Änderungen für einen Cloudendpunkt wird einmal alle 24 Stunden gestartet. Darüber hinaus bewirken Änderungen, die über das REST-Protokoll an einer Azure-Dateifreigabe vorgenommen wurden, keine Aktualisierung der letzten SMB-Änderungszeit, und die Änderungen sind für eine Synchronisierung nicht zu sehen.
- Der Speichersynchronisierungsdienst und/oder das Speicherkonto kann in eine andere Ressourcengruppe oder ein anderes Abonnement im vorhandenen Azure AD-Mandanten verschoben werden. Wenn das Speicherkonto verschoben wird, müssen Sie dem Hybrid-Dateisynchronisierungsdienst Zugriff auf das Speicherkonto gewähren (siehe [Sicherstellen, dass die Azure-Dateisynchronisierung Zugriff auf das Speicherkonto besitzt](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cportal#troubleshoot-rbac)).

    > [!Note]  
    > Die Azure-Dateisynchronisierung unterstützt nicht das Verschieben des Abonnements in einen anderen Azure AD-Mandanten.

### <a name="cloud-tiering"></a>Cloudtiering
- Wenn eine Tieringdatei mit Robocopy an einen anderen Speicherort kopiert wird, ist die sich ergebende Datei keine Tieringdatei. Das Offlineattribut kann festgelegt werden, da dieses Attribut fälschlicherweise von Robocopy in Kopiervorgänge eingefügt wird.
- Verwenden Sie beim Kopieren der Dateien mit Robocopy die Option „/MIR“, um Dateizeitstempel beizubehalten. So wird sichergestellt, dass das Tiering für ältere Dateien früher als für die Dateien durchgeführt wird, auf die zuletzt zugegriffen wurde.
- Beim Anzeigen von Dateieigenschaften über einen SMB-Client sieht es aufgrund der Zwischenspeicherung von Dateimetadaten unter Umständen so aus, als wäre das Offlineattribut nicht korrekt festgelegt.

## <a name="agent-version-5200"></a>Agent-Version 5.2.0.0
Die folgenden Versionshinweise gelten für Version 5.2.0.0 des Azure-Dateisynchronisierungs-Agents, die am 4. April 2019 veröffentlicht wurde. Diese Hinweise gelten zusätzlich zu den Versionshinweisen, die für Version 5.0.2.0 angegeben sind.

Liste der in dieser Version behobenem Probleme:  
- Verbesserte Zuverlässigkeit bei Features für Offlinedatenübertragung und Datenübertragungsfortsetzung
- Verbesserungen bei den Synchronisierungstelemetriedaten

## <a name="agent-version-5100"></a>Agent-Version 5.1.0.0
Die folgenden Versionshinweise gelten für Version 5.1.0.0 des Azure-Dateisynchronisierungs-Agents, die am 7. März 2019 veröffentlicht wurde. Diese Hinweise gelten zusätzlich zu den Versionshinweisen, die für Version 5.0.2.0 angegeben sind.

Liste der in dieser Version behobenem Probleme:  
- Bei der Dateisynchronisierung tritt möglicherweise Fehler „0x80c8031d (ECS_E_CONCURRENCY_CHECK_FAILED)“, wenn die Änderungsenumeration auf dem Server nicht funktioniert.
- Wenn eine Synchronisierungssitzung oder eine Datei den Fehler „0x80072f78 (WININET_E_INVALID_SERVER_RESPONSE)“ empfängt, wiederholt die Synchronisierung den Vorgang nicht.
- Beim Synchronisieren von Dateien tritt möglicherweise der Fehler „0x80c80203 (ECS_E_SYNC_INVALID_STAGED_FILE)“ auf.
- Beim Zurückrufen von Dateien tritt möglicherweise eine hohe Arbeitsspeicherauslastung auf.
- Verbesserungen bei Cloudtiering-Telemetriedaten 

## <a name="agent-version-5020"></a>Agent-Version 5.0.2.0
Die folgenden Versionshinweise gelten für Version 5.0.2.0 des Azure-Dateisynchronisierungs-Agents (veröffentlicht am 12. Februar 2019).

### <a name="improvements-and-issues-that-are-fixed"></a>Verbesserungen und behobene Probleme

- Unterstützung für Azure Government-Cloud
  - Wir haben Vorschauunterstützung für die Azure Government-Cloud hinzugefügt. Dazu ist ein zugelassenes Abonnement und ein spezieller Agent-Download von Microsoft erforderlich. Wenn Sie Zugriff auf die Vorschau erhalten möchten, senden Sie uns bitte unter [AzureFiles@microsoft.com](mailto:AzureFiles@microsoft.com) eine E-Mail.
- Unterstützung für Datendeduplizierung
    - Die Datendeduplizierung wird jetzt mit aktiviertem Cloudtiering unter Windows Server 2016 und Windows Server 2019 vollständig unterstützt. Durch das Aktivieren der Deduplizierung auf einem Volume mit aktiviertem Cloudtiering können Sie weitere Dateien lokal zwischenspeichern, ohne mehr Speicher bereitstellen zu müssen.
- Unterstützung für Offlinedatenübertragung (z.B. über Data Box)
    - Migrieren Sie mühelos große Datenmengen in die Azure-Dateisynchronisierung über das von Ihnen gewählte Mittel. Sie können Azure Data Box, AzCopy und sogar Migrationsdienste von Drittanbietern auswählen. Bei Data Box benötigen Sie keine riesigen Bandbreiten, um Ihre Daten in Azure zu übertragen – senden Sie sie einfach per E-Mail dorthin! Weitere Informationen finden Sie unter [Offlinedatenübertragung – Dokumentation](https://aka.ms/AFS/OfflineDataTransfer).
- Verbesserte Synchronisierungsleistung
    - Kunden mit mehreren Serverendpunkten auf demselben Volume haben vor dieser Release möglicherweise eine langsame Synchronisierungsleistung festgestellt. Die Azure-Dateisynchronisierung erstellt einmal pro Tag eine temporäre VSS-Momentaufnahme auf dem Server, um Dateien mit offenen Handles zu synchronisieren. Jetzt unterstützt die Synchronisierung mehrere Serverendpunkte, die auf einem Volume synchronisieren, wenn eine VSS-Synchronisierungssitzung aktiv ist. Es muss nicht mehr gewartet werden, bis eine VSS-Synchronisierungssitzung abgeschlossen ist, damit die Synchronisierung auf anderen Serverendpunkten auf dem Volume fortgesetzt werden kann.
- Verbesserte Überwachung im Portal
    - Im Speichersynchronisierungsdienst-Portal wurden Diagramme hinzugefügt, um Folgendes anzuzeigen:
        - Anzahl der synchronisierten Dateien
        - Größe der übertragenen Daten
        - Anzahl der Dateien ohne Synchronisierung
        - Größe der zurückgerufenen Daten
        - Status der Serverkonnektivität
    - Weitere Informationen finden Sie unter [Überwachen der Azure-Dateisynchronisierung](https://docs.microsoft.com/azure/storage/files/storage-sync-files-monitoring).
- Verbesserte Skalierbarkeit und Zuverlässigkeit
    - Die maximale Anzahl von Dateisystemobjekten (Verzeichnisse und Dateien) in einem Verzeichnis wurde auf 1.000.000 erhöht. Das vorherige Limit lag bei 200.000.
    - Wenn eine Übertragung bei großen Dateien unterbrochen wird, versucht die Synchronisierung, die Datenübertragung fortzusetzen, statt die Daten erneut zu übertragen. 

### <a name="evaluation-tool"></a>Auswertungstool
Vor der Bereitstellung der Azure-Dateisynchronisierung müssen Sie mit dem Auswertungstool für die Azure-Dateisynchronisierung auswerten, ob Kompatibilität mit Ihrem System gegeben ist. Dieses Tool ist ein Azure PowerShell-Cmdlet, das auf potenzielle Probleme mit Ihrem Dateisystem und Dataset prüft, z.B. nicht unterstützte Zeichen oder eine nicht unterstützte Betriebssystemversion. Anweisungen zur Installation und Verwendung finden Sie im Planungshandbuch im Abschnitt [Auswertungstools](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning#evaluation-cmdlet). 

### <a name="agent-installation-and-server-configuration"></a>Agent-Installation und Serverkonfiguration
Weitere Informationen zum Installieren und Konfigurieren des Azure File Sync-Agents mit Windows Server finden Sie unter [Planen einer Bereitstellung der Azure-Dateisynchronisierung (Vorschau)](storage-sync-files-planning.md) sowie unter [Bereitstellen von Azure File Sync (Vorschau)](storage-sync-files-deployment-guide.md).

- Das Agent-Installationspaket muss mit erhöhten Berechtigungen (Administratorberechtigungen) installiert werden.
- Der Agent wird für die Bereitstellungsoptionen „Windows Server Core“ oder „Nano Server“ nicht unterstützt.
- Der Agent wird nur unter Windows Server 2019, Windows Server 2016 und Windows Server 2012 R2 unterstützt.
- Der Agent benötigt mindestens 2 GiB Arbeitsspeicher. Wenn der Server auf einem virtuellen Computer ausgeführt wird, für den dynamischer Arbeitsspeicher aktiviert ist, muss der virtuelle Computer mit mindestens 2.048 MiB Arbeitsspeicher konfiguriert werden.
- Der Dienst „Storage-Synchronisierungs-Agent“ (FileSyncSvc) unterstützt keine Serverendpunkte, die sich auf einem Volume befinden, für das das Verzeichnis „System Volume Information“ (SVI) komprimiert ist. Diese Konfiguration führt zu unerwarteten Ergebnissen.
- Der FIPS-Modus wird nicht unterstützt und muss deaktiviert werden. 

### <a name="interoperability"></a>Interoperabilität
- Virenschutz, Sicherung und andere Anwendungen, die auf Tieringdateien zugreifen, können zu unerwünschten Rückrufen führen, wenn sie das Offlineattribut nicht berücksichtigen und das Lesen des Inhalts dieser Dateien nicht überspringen. Weitere Informationen finden Sie unter [Problembehandlung bei der Azure-Dateisynchronisierung (Vorschau)](storage-sync-files-troubleshoot.md).
- FSRM-Dateiprüfungen (File Server Resource Manager, Ressourcen-Manager für Dateiserver) können zu Fehlern aufgrund einer endlosen Synchronisierung führen, wenn Dateien aufgrund der damit verbundenen Vorgänge blockiert werden.
- Die Ausführung von Sysprep auf einem Server, für den der Azure-Dateisynchronisierungs-Agent installiert ist, wird nicht unterstützt und kann zu unerwarteten Ergebnissen führen. Der Azure-Dateisynchronisierungs-Agent sollte installiert werden, nachdem das Serverimage bereitgestellt und das Sysprep-Mini-Setup abgeschlossen wurde.

### <a name="sync-limitations"></a>Einschränkungen bei der Synchronisierung
Folgende Elemente werden nicht synchronisiert, aber der restliche Systembetrieb ist nicht beeinträchtigt:
- Dateien mit nicht unterstützten Zeichen. Eine Liste mit den Zeichen, die nicht unterstützt werden, finden Sie im [Leitfaden zur Problembehandlung](storage-sync-files-troubleshoot.md#handling-unsupported-characters).
- Dateien oder Verzeichnisse, die mit einem Punkt enden.
- Pfade, die länger als 2.048 Zeichen sind.
- Der DACL-Teil (besitzerverwaltete Zugriffssteuerungsliste) einer Sicherheitsbeschreibung, sofern dieser größer als 2 KB ist. (Dieses Problem trifft nur zu, wenn Sie für ein einzelnes Element über mehr als ca. 40 Zugriffssteuerungseinträge verfügen.)
- Der SACL-Teil (System-Zugriffssteuerungsliste) einer Sicherheitsbeschreibung, die für die Überwachung verwendet wird.
- Erweiterte Attribute
- Alternative Datenströme
- Analysepunkte
- Feste Links
- Die Komprimierung (sofern für eine Serverdatei festgelegt) wird nicht beibehalten, wenn Änderungen mit der Datei von anderen Endpunkten synchronisiert werden.
- Mit EFS (oder einer anderen Benutzermodusverschlüsselung) verschlüsselte Dateien, die den Dienst am Lesen der Daten hindern.

    > [!Note]  
    > Bei der Azure-Dateisynchronisierung werden Daten während der Übertragung immer verschlüsselt. Ruhende Daten werden in Azure immer verschlüsselt.
 
### <a name="server-endpoint"></a>Serverendpunkt
- Ein Serverendpunkt kann nur auf einem NTFS-Volume erstellt werden. ReFS, FAT, FAT32 und andere Dateisysteme werden von der Azure-Dateisynchronisierung derzeit nicht unterstützt.
- Auf mehrstufige Dateien kann nicht mehr zugegriffen werden, wenn für die Dateien vor dem Löschen des Serverendpunkts kein Rückruf erfolgt. Erstellen Sie den Serverendpunkt neu, um den Zugriff auf die Dateien wiederherzustellen. Nachdem 30 Tage vergangen sind, seitdem der Serverendpunkt gelöscht wurde, oder wenn der Cloudendpunkt gelöscht wurde, sind mehrstufige Dateien, für die kein Rückruf durchgeführt wurde, nicht mehr verwendbar.
- Das Cloudtiering wird auf dem Systemvolume nicht unterstützt. Um einen Serverendpunkt auf dem Systemvolume zu erstellen, deaktivieren Sie Cloudtiering, wenn Sie den Serverendpunkt erstellen.
- Failoverclustering wird nur mit Clusterdatenträgern, aber nicht mit freigegebenen Clustervolumes (Cluster Shared Volumes, CSVs) unterstützt.
- Ein Serverendpunkt kann nicht geschachtelt werden. Er kann auf demselben Volume parallel zu einem anderen Endpunkt vorhanden sein (Koexistenz).
- Speichern Sie keine Betriebssystem- oder Anwendungsauslagerungsdatei an einem Serverendpunkt-Standort.
- Der Servername im Portal wird bei Umbenennung des Servers nicht aktualisiert.

### <a name="cloud-endpoint"></a>Cloudendpunkt
- Die Azure-Dateisynchronisierung unterstützt direkte Änderungen an der Azure-Dateifreigabe. Allerdings müssen alle Änderungen, die Sie an der Azure-Dateifreigabe vornehmen, zuerst von einem Azure-Dateisynchronisierungsauftrag zum Erkennen von Änderungen erkannt werden. Ein Auftrag zum Erkennen von Änderungen für einen Cloudendpunkt wird einmal alle 24 Stunden gestartet. Darüber hinaus bewirken Änderungen, die über das REST-Protokoll an einer Azure-Dateifreigabe vorgenommen wurden, keine Aktualisierung der letzten SMB-Änderungszeit, und die Änderungen sind für eine Synchronisierung nicht zu sehen.
- Der Speichersynchronisierungsdienst und/oder das Speicherkonto kann in eine andere Ressourcengruppe oder ein anderes Abonnement im vorhandenen Azure AD-Mandanten verschoben werden. Wenn das Speicherkonto verschoben wird, müssen Sie dem Hybrid-Dateisynchronisierungsdienst Zugriff auf das Speicherkonto gewähren (siehe [Sicherstellen, dass die Azure-Dateisynchronisierung Zugriff auf das Speicherkonto besitzt](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cportal#troubleshoot-rbac)).

    > [!Note]  
    > Die Azure-Dateisynchronisierung unterstützt nicht das Verschieben des Abonnements in einen anderen Azure AD-Mandanten.

### <a name="cloud-tiering"></a>Cloudtiering
- Wenn eine Tieringdatei mit Robocopy an einen anderen Speicherort kopiert wird, ist die sich ergebende Datei keine Tieringdatei. Das Offlineattribut kann festgelegt werden, da dieses Attribut fälschlicherweise von Robocopy in Kopiervorgänge eingefügt wird.
- Verwenden Sie beim Kopieren der Dateien mit Robocopy die Option „/MIR“, um Dateizeitstempel beizubehalten. So wird sichergestellt, dass das Tiering für ältere Dateien früher als für die Dateien durchgeführt wird, auf die zuletzt zugegriffen wurde.
- Beim Anzeigen von Dateieigenschaften über einen SMB-Client sieht es aufgrund der Zwischenspeicherung von Dateimetadaten unter Umständen so aus, als wäre das Offlineattribut nicht korrekt festgelegt.

## <a name="agent-version-4300"></a>Agent-Version 4.3.0.0
Die folgenden Versionshinweise gelten für Version 4.3.0.0 des Azure-Dateisynchronisierungs-Agents, Veröffentlichung: 14. Januar 2019. Diese Anmerkungen gelten zusätzlich zu den Anmerkungen zu dieser Version, die für Version 4.0.1.0 angegeben sind.

Liste der in dieser Version behobenem Probleme:  
- Dateien sind nach dem Upgrade des Azure-Dateisynchronisierungs-Agents auf Version 4.x nicht mehrstufig.
- „AfsUpdater.exe“ wird jetzt unter Windows Server 2019 unterstützt.
- Verschiedene Verbesserungen für die Zuverlässigkeit der Synchronisierung. 

## <a name="agent-version-4200"></a>Agent-Version 4.2.0.0
Die folgenden Versionshinweise gelten für Version 4.2.0.0 des Azure-Dateisynchronisierungs-Agents (Veröffentlichung: 10. Dezember 2018). Diese Anmerkungen gelten zusätzlich zu den Anmerkungen zu dieser Version, die für Version 4.0.1.0 angegeben sind.

Liste der in dieser Version behobenem Probleme:  
- Ein Abbruchfehler vom Typ „0x3B“ oder „0x1E“ kann auftreten, wenn eine VSS-Momentaufnahme erstellt wird.  
- Ein Arbeitsspeicherverlust kann auftreten, wenn Cloudtiering aktiviert ist.  

## <a name="agent-version-4100"></a>Agent-Version 4.1.0.0
Die folgenden Versionshinweise gelten für Version 4.1.0.0 des Azure-Dateisynchronisierungs-Agents (Veröffentlichung: 4. Dezember 2018). Diese Anmerkungen gelten zusätzlich zu den Anmerkungen zu dieser Version, die für Version 4.0.1.0 angegeben sind.

Liste der in dieser Version behobenem Probleme:  
- Der Server reagiert ggf. aufgrund eines Cloudtiering-Speicherverlusts nicht mehr.  
- Fehler bei der Agent-Installation: Fehler 1921. Der Dienst „Speichersynchronisierungs-Agent“ (FileSyncSvc) konnte nicht beendet werden.  Überprüfen Sie, ob Sie über ausreichend Rechte verfügen, um Systemdienste zu beenden.  
- Der Speichersynchronisierungs-Agent-Dienst (FileSyncSvc) stürzt möglicherweise ab, wenn die Speicherauslastung hoch ist.  
- Verschiedene Zuverlässigkeitverbesserungen für Cloudtiering und Synchronisierung.

## <a name="agent-version-4010"></a>Agent-Version 4.0.1.0
Die folgenden Versionshinweise gelten für Version 4.0.1.0 des Azure-Dateisynchronisierungs-Agents (Veröffentlichung: 13. November 2018).

### <a name="evaluation-tool"></a>Auswertungstool
Vor der Bereitstellung der Azure-Dateisynchronisierung müssen Sie mit dem Auswertungstool für die Azure-Dateisynchronisierung auswerten, ob Kompatibilität mit Ihrem System gegeben ist. Dieses Tool ist ein Azure PowerShell-Cmdlet, das auf potenzielle Probleme mit Ihrem Dateisystem und Dataset prüft, z.B. nicht unterstützte Zeichen oder eine nicht unterstützte Betriebssystemversion. Anweisungen zur Installation und Verwendung finden Sie im Planungshandbuch im Abschnitt [Auswertungstools](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning#evaluation-cmdlet). 

### <a name="agent-installation-and-server-configuration"></a>Agent-Installation und Serverkonfiguration
Weitere Informationen zum Installieren und Konfigurieren des Azure File Sync-Agents mit Windows Server finden Sie unter [Planen einer Bereitstellung der Azure-Dateisynchronisierung (Vorschau)](storage-sync-files-planning.md) sowie unter [Bereitstellen von Azure File Sync (Vorschau)](storage-sync-files-deployment-guide.md).

- Das Agent-Installationspaket muss mit erhöhten Berechtigungen (Administratorberechtigungen) installiert werden.
- Der Agent wird für die Bereitstellungsoptionen „Windows Server Core“ oder „Nano Server“ nicht unterstützt.
- Der Agent wird nur unter Windows Server 2019, Windows Server 2016 und Windows Server 2012 R2 unterstützt.
- Der Agent benötigt mindestens 2 GiB Arbeitsspeicher. Wenn der Server auf einem virtuellen Computer ausgeführt wird, für den dynamischer Arbeitsspeicher aktiviert ist, muss der virtuelle Computer mit mindestens 2.048 MiB Arbeitsspeicher konfiguriert werden.
- Der Dienst „Storage-Synchronisierungs-Agent“ (FileSyncSvc) unterstützt keine Serverendpunkte, die sich auf einem Volume befinden, für das das Verzeichnis „System Volume Information“ (SVI) komprimiert ist. Diese Konfiguration führt zu unerwarteten Ergebnissen.
- Der FIPS-Modus wird nicht unterstützt und muss deaktiviert werden. 
- Ein Abbruchfehler vom Typ „0x3B“ oder „0x1E“ kann auftreten, wenn eine VSS-Momentaufnahme erstellt wird.

### <a name="interoperability"></a>Interoperabilität
- Virenschutz, Sicherung und andere Anwendungen, die auf Tieringdateien zugreifen, können zu unerwünschten Rückrufen führen, wenn sie das Offlineattribut nicht berücksichtigen und das Lesen des Inhalts dieser Dateien nicht überspringen. Weitere Informationen finden Sie unter [Problembehandlung bei der Azure-Dateisynchronisierung (Vorschau)](storage-sync-files-troubleshoot.md).
- FSRM-Dateiprüfungen (File Server Resource Manager, Ressourcen-Manager für Dateiserver) können zu Fehlern aufgrund einer endlosen Synchronisierung führen, wenn Dateien aufgrund der damit verbundenen Vorgänge blockiert werden.
- Die Ausführung von Sysprep auf einem Server, für den der Azure-Dateisynchronisierungs-Agent installiert ist, wird nicht unterstützt und kann zu unerwarteten Ergebnissen führen. Der Azure-Dateisynchronisierungs-Agent sollte installiert werden, nachdem das Serverimage bereitgestellt und das Sysprep-Mini-Setup abgeschlossen wurde.
- Datendeduplizierung und Cloudtiering auf demselben Volume werden nicht unterstützt.

### <a name="sync-limitations"></a>Einschränkungen bei der Synchronisierung
Folgende Elemente werden nicht synchronisiert, aber der restliche Systembetrieb ist nicht beeinträchtigt:
- Dateien mit nicht unterstützten Zeichen. Eine Liste mit den Zeichen, die nicht unterstützt werden, finden Sie im [Leitfaden zur Problembehandlung](storage-sync-files-troubleshoot.md#handling-unsupported-characters).
- Dateien oder Verzeichnisse, die mit einem Punkt enden.
- Pfade, die länger als 2.048 Zeichen sind.
- Der DACL-Teil (besitzerverwaltete Zugriffssteuerungsliste) einer Sicherheitsbeschreibung, sofern dieser größer als 2 KB ist. (Dieses Problem trifft nur zu, wenn Sie für ein einzelnes Element über mehr als ca. 40 Zugriffssteuerungseinträge verfügen.)
- Der SACL-Teil (System-Zugriffssteuerungsliste) einer Sicherheitsbeschreibung, die für die Überwachung verwendet wird.
- Erweiterte Attribute
- Alternative Datenströme
- Analysepunkte
- Feste Links
- Die Komprimierung (sofern für eine Serverdatei festgelegt) wird nicht beibehalten, wenn Änderungen mit der Datei von anderen Endpunkten synchronisiert werden.
- Mit EFS (oder einer anderen Benutzermodusverschlüsselung) verschlüsselte Dateien, die den Dienst am Lesen der Daten hindern.

    > [!Note]  
    > Bei der Azure-Dateisynchronisierung werden Daten während der Übertragung immer verschlüsselt. Ruhende Daten werden in Azure immer verschlüsselt.
 
### <a name="server-endpoint"></a>Serverendpunkt
- Ein Serverendpunkt kann nur auf einem NTFS-Volume erstellt werden. ReFS, FAT, FAT32 und andere Dateisysteme werden von der Azure-Dateisynchronisierung derzeit nicht unterstützt.
- Auf mehrstufige Dateien kann nicht mehr zugegriffen werden, wenn für die Dateien vor dem Löschen des Serverendpunkts kein Rückruf erfolgt. Erstellen Sie den Serverendpunkt neu, um den Zugriff auf die Dateien wiederherzustellen. Nachdem 30 Tage vergangen sind, seitdem der Serverendpunkt gelöscht wurde, oder wenn der Cloudendpunkt gelöscht wurde, sind mehrstufige Dateien, für die kein Rückruf durchgeführt wurde, nicht mehr verwendbar.
- Das Cloudtiering wird auf dem Systemvolume nicht unterstützt. Um einen Serverendpunkt auf dem Systemvolume zu erstellen, deaktivieren Sie Cloudtiering, wenn Sie den Serverendpunkt erstellen.
- Failoverclustering wird nur mit Clusterdatenträgern, aber nicht mit freigegebenen Clustervolumes (Cluster Shared Volumes, CSVs) unterstützt.
- Ein Serverendpunkt kann nicht geschachtelt werden. Er kann auf demselben Volume parallel zu einem anderen Endpunkt vorhanden sein (Koexistenz).
- Speichern Sie keine Betriebssystem- oder Anwendungsauslagerungsdatei an einem Serverendpunkt-Standort.
- Der Servername im Portal wird bei Umbenennung des Servers nicht aktualisiert.

### <a name="cloud-endpoint"></a>Cloudendpunkt
- Die Azure-Dateisynchronisierung unterstützt direkte Änderungen an der Azure-Dateifreigabe. Allerdings müssen alle Änderungen, die Sie an der Azure-Dateifreigabe vornehmen, zuerst von einem Azure-Dateisynchronisierungsauftrag zum Erkennen von Änderungen erkannt werden. Ein Auftrag zum Erkennen von Änderungen für einen Cloudendpunkt wird einmal alle 24 Stunden gestartet. Darüber hinaus bewirken Änderungen, die über das REST-Protokoll an einer Azure-Dateifreigabe vorgenommen wurden, keine Aktualisierung der letzten SMB-Änderungszeit, und die Änderungen sind für eine Synchronisierung nicht zu sehen.
- Der Speichersynchronisierungsdienst und/oder das Speicherkonto kann in eine andere Ressourcengruppe oder ein anderes Abonnement im vorhandenen Azure AD-Mandanten verschoben werden. Wenn das Speicherkonto verschoben wird, müssen Sie dem Hybrid-Dateisynchronisierungsdienst Zugriff auf das Speicherkonto gewähren (siehe [Sicherstellen, dass die Azure-Dateisynchronisierung Zugriff auf das Speicherkonto besitzt](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cportal#troubleshoot-rbac)).

    > [!Note]  
    > Die Azure-Dateisynchronisierung unterstützt nicht das Verschieben des Abonnements in einen anderen Azure AD-Mandanten.

### <a name="cloud-tiering"></a>Cloudtiering
- Die Richtlinieneinstellung für das datumsbasierte Cloudtiering wird verwendet, um Dateien anzugeben, die zwischengespeichert werden sollen, wenn innerhalb einer angegebenen Anzahl von Tagen darauf zugegriffen wird. Weitere Informationen finden Sie im Artikel [Übersicht über Cloudtiering](https://docs.microsoft.com/azure/storage/files/storage-sync-cloud-tiering#afs-force-tiering).
- Wenn eine Tieringdatei mit Robocopy an einen anderen Speicherort kopiert wird, ist die sich ergebende Datei keine Tieringdatei. Das Offlineattribut kann festgelegt werden, da dieses Attribut fälschlicherweise von Robocopy in Kopiervorgänge eingefügt wird.
- Verwenden Sie beim Kopieren der Dateien mit Robocopy die Option „/MIR“, um Dateizeitstempel beizubehalten. So wird sichergestellt, dass das Tiering für ältere Dateien früher als für die Dateien durchgeführt wird, auf die zuletzt zugegriffen wurde.
- Beim Anzeigen von Dateieigenschaften über einen SMB-Client sieht es aufgrund der Zwischenspeicherung von Dateimetadaten unter Umständen so aus, als wäre das Offlineattribut nicht korrekt festgelegt.
