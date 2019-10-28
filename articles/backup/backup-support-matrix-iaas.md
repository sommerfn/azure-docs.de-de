---
title: Unterstützungsmatrix für die Sicherung virtueller Azure-Computer mit Azure Backup
description: Enthält eine Zusammenfassung der Unterstützungseinstellungen und Einschränkungen bei der Sicherung virtueller Azure-Computer mit dem Azure Backup-Dienst.
author: dcurwin
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: dacurwin
ms.openlocfilehash: 3f89b5aa427689d5f3da28726897d7a772bf0781
ms.sourcegitcommit: 6eecb9a71f8d69851bc962e2751971fccf29557f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "72533159"
---
# <a name="support-matrix-for-azure-vm-backup"></a>Unterstützungsmatrix für die Sicherung virtueller Azure-Computer

Mit dem [Azure Backup-Dienst](backup-overview.md) können Sie lokale Computer und Workloads sowie virtuelle Azure-Computer (VMs) sichern. Dieser Artikel enthält eine Zusammenfassung der Unterstützungseinstellungen und Einschränkungen bei der Sicherung von Azure-VMs mit Azure Backup.

Andere Unterstützungsmatrizen:

- [Allgemeine Unterstützungsmatrix](backup-support-matrix.md) für Azure Backup
- [Unterstützungsmatrix](backup-support-matrix-mabs-dpm.md) für Azure Backup-Server bzw. System Center Data Protection Manager-Sicherungen
- [Unterstützungsmatrix](backup-support-matrix-mars-agent.md) für die Sicherung mit dem Microsoft Azure Recovery Services-Agent (MARS)

## <a name="supported-scenarios"></a>Unterstützte Szenarien

Virtuelle Azure-Computer können wie folgt mit dem Azure Backup-Dienst gesichert und wiederhergestellt werden.

**Szenario** | **Sicherung** | **Agent** |**Wiederherstellen**
--- | --- | --- | ---
Direkte Sicherung virtueller Azure-Computer  | Sicherung der gesamten VM  | Auf dem virtuellen Azure-Computer ist kein Agent erforderlich. Azure Backup installiert und verwendet eine Erweiterung für den [Azure-VM-Agent](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-windows), der auf dem virtuellen Computer ausgeführt wird. | Die Wiederherstellung wird wie folgt durchgeführt:<br/><br/> - **Erstellen eines einfachen virtuellen Computers:** Dies ist nützlich, wenn der virtuelle Computer keine besondere Konfiguration (z.B. mehrere IP-Adressen) aufweist.<br/><br/> - **Wiederherstellen des VM-Datenträgers:** Stellen Sie den Datenträger wieder her. Fügen Sie ihn dann an einen vorhandenen virtuellen Computer an, oder erstellen Sie mit PowerShell über den Datenträger einen neuen virtuellen Computer.<br/><br/> - **Ersetzen des VM-Datenträgers:** Wenn ein virtueller Computer vorhanden ist, der verwaltete Datenträger (unverschlüsselt) verwendet, können Sie einen Datenträger wiederherstellen und zum Ersetzen eines vorhandenen Datenträgers auf dem virtuellen Computer verwenden.<br/><br/> - **Wiederherstellen bestimmter Dateien oder Ordner:** Sie können Dateien oder Ordner einer VM statt der gesamten VM wiederherstellen.
Direkte Sicherung von Azure-VMs (nur Windows)  | Sichern bestimmter Dateien, Ordner oder Volumes | Installieren Sie den [Azure Recovery Services-Agent](backup-azure-file-folder-backup-faq.md).<br/><br/> Sie können den MARS-Agent zusammen mit der Sicherungserweiterung für den Azure-VM-Agent ausführen, um den virtuellen Computer auf Datei- oder Ordnerebene zu sichern. | Wiederherstellen bestimmter Ordner oder Dateien
Sichern von Azure-VMs auf Sicherungsservern  | Sichern von Dateien, Ordnern oder Volumes, von Systemstatusdateien oder Bare-Metal-Dateien und von Anwendungsdaten in System Center DPM oder Microsoft Azure Backup Server (MABS)<br/><br/> DPM/MABS erstellt die Sicherung dann im Sicherungstresor. | Installieren Sie den MABS- oder DPM-Schutz-Agent auf der VM. Der MARS-Agent wird in DPM oder MABS installiert.| Wiederherstellen von Dateien, Ordnern oder Volumes, von Systemstatus- oder Bare-Metal-Dateien und von Anwendungsdaten

Weitere Informationen zum Sichern finden Sie in den Artikeln zu [Sicherungsservern](backup-architecture.md#architecture-back-up-to-dpmmabs) und den [Anforderungen für die Unterstützung](backup-support-matrix-mabs-dpm.md).

## <a name="supported-backup-actions"></a>Unterstützte Sicherungsaktionen

**Aktion** | **Unterstützung**
--- | ---
Aktivieren der Sicherung beim Erstellen eines virtuellen Windows-Azure-Computers | Unterstützung für: <br/><br/> - Windows Server 2019 (Datacenter/Datacenter Core/Standard) <br/><br/> - Windows Server 2016 (Datacenter/Datacenter Core/Standard) <br/><br/> - Windows Server 2012 R2 (Datacenter/Standard) <br/><br/> - Windows Server 2008 R2 (RTM und SP1 Standard)
Aktivieren der Sicherung beim Erstellen eines virtuellen Linux-Computers | Unterstützung für:<br/><br/> – Ubuntu Server: 18.04, 17.10, 17.04, 16.04 (LTS), 14.04 (LTS)<br/><br/> – Red Hat: RHEL 6.7, 6.8, 6.9, 7.2, 7.3, 7.4<br/><br/> – SUSE Linux Enterprise Server: 11 SP4, 12 SP2, 12 SP3, 15 <br/><br/> – Debian: 8, 9<br/><br/> – CentOS: 6.9, 7.3<br/><br/> – Oracle Linux: 6.7, 6.8, 6.9, 7.2, 7.3
Sichern eines heruntergefahrenen virtuellen Computers / einer Offline-VM | Unterstützt.<br/><br/> Die Momentaufnahme ist nur ausfallkonsistent, jedoch nicht anwendungskonsistent.
Sicherung von Datenträgern nach Migration zu verwalteten Datenträgern | Unterstützt.<br/><br/> Sicherung wird weiterhin durchgeführt. Es ist keine Aktion erforderlich.
Sicherung von verwalteten Datenträgern nach Aktivierung einer Ressourcengruppensperre | Nicht unterstützt.<br/><br/> Die älteren Wiederherstellungspunkte können von Azure Backup nicht gelöscht werden, und bei Sicherungen treten Fehler auf, wenn die maximale Anzahl von Wiederherstellungspunkten erreicht ist.
Ändern der Sicherungsrichtlinie für einen virtuellen Computer | Unterstützt.<br/><br/> Der virtuelle Computer wird unter Verwendung der Zeitplan- und Aufbewahrungseinstellungen der neuen Richtlinie gesichert. Wenn die Aufbewahrungseinstellungen erweitert werden, werden vorhandene Wiederherstellungspunkte markiert und beibehalten. Wenn die Einstellungen reduziert werden, werden vorhandene Wiederherstellungspunkte im nächsten Bereinigungsauftrag gelöscht und schließlich endgültig entfernt.
Abbrechen eines Sicherungsauftrags| Wird während des Prozesses der Momentaufnahme unterstützt.<br/><br/> Wird nicht unterstützt, wenn die Momentaufnahme in den Tresor übertragen wird.
Sicherung des virtuellen Computers in einer anderen Region oder einem anderen Abonnement |Nicht unterstützt.
Sicherungen pro Tag (über die Azure-VM-Erweiterung) | Eine geplante Sicherung pro Tag<br/><br/> Sie können bis zu vier bedarfsgesteuerte Sicherungen pro Tag durchführen.
Sicherungen pro Tag (über den MARS-Agent) | Drei geplante Sicherungen pro Tag
Sicherungen pro Tag (über DPM/MABS) | Zwei geplante Sicherungen pro Tag
Monatliche oder jährliche Sicherung| Wird bei der Sicherung mit der Azure-VM-Erweiterung nicht unterstützt. Lediglich tägliche und wöchentliche Sicherungen werden unterstützt.<br/><br/> Sie können die Richtlinie so einrichten, dass tägliche oder wöchentliche Sicherungen in einem monatlichen oder jährlichen Aufbewahrungszeitraum beibehalten werden.
Automatische Uhrzeitanpassung | Nicht unterstützt.<br/><br/> Azure Backup passt Änderungen an der Sommer- oder Winterzeit bei der Sicherung einer VM nicht automatisch an.<br/><br/>  Ändern Sie die Richtlinie nach Bedarf manuell.
[Sicherheitsfunktionen für Hybridsicherungen](https://docs.microsoft.com/azure/backup/backup-azure-security-feature) |Die Sicherheitsfunktionen können nicht deaktiviert werden.
Sichern des virtuellen Computers mit geänderter Computerzeit | Nicht unterstützt.<br/><br/> Falls als Computerzeit ein zukünftiges Datum bzw. eine zukünftige Uhrzeit festgelegt wird, nachdem die Sicherung für diesen virtuellen Computer aktiviert wurde. Auch wenn diese Änderung rückgängig gemacht wird, wird die erfolgreiche Sicherung nicht garantiert.  

## <a name="operating-system-support-windows"></a>Unterstützte Betriebssysteme (Windows)

In der folgenden Tabelle sind die unterstützten Betriebssysteme für die Sicherung von Azure-VMs aufgeführt.

**Szenario** | **Betriebssystemunterstützung**
--- | ---
Sicherung mit der Azure-VM-Agent-Erweiterung | Windows-Client: Nicht unterstützt<br/><br/>- Windows Server 2019 (Datacenter/Datacenter Core/Standard) <br/><br/> - Windows Server 2016 (Datacenter/Datacenter Core/Standard) <br/><br/> - Windows Server 2012 R2 (Datacenter/Standard) <br/><br/> - Windows Server 2008 R2 (RTM und SP1 Standard)
Sicherung mit dem MARS-Agent | [Unterstützte](backup-support-matrix-mars-agent.md#support-for-direct-backups) Betriebssysteme
Sicherung mit DPM/MABS | Unterstützte Betriebssysteme für die Sicherung mit [MABS](backup-mabs-protection-matrix.md) und [DPM](https://docs.microsoft.com/system-center/dpm/dpm-protection-matrix?view=sc-dpm-1807)

## <a name="support-for-linux-backup"></a>Unterstützung für die Linux-Sicherung

Hier ist die Unterstützung für den Fall angegeben, dass Sie Linux-Computer sichern möchten.

**Aktion** | **Unterstützung**
--- | ---
Sicherung virtueller Linux-Azure-Computer mit dem Linux-Azure-VM-Agent | Dateikonsistente Sicherung<br/><br/> App-konsistente Sicherung mit [benutzerdefinierten Skripts](backup-azure-linux-app-consistent.md).<br/><br/> Während der Wiederherstellung können Sie eine neue VM erstellen oder einen Datenträger wiederherstellen und damit eine VM erstellen oder einen Datenträger auf einer vorhandenen VM ersetzen. Zudem können Sie einzelne Dateien und Ordner wiederherstellen.
Sicherung virtueller Linux-Azure-Computer mit dem MARS-Agent | Nicht unterstützt.<br/><br/> Der MARS-Agent kann nur auf Windows-Computern installiert werden.
Sicherung virtueller Linux-Azure-Computer mit DPM/MABS | Nicht unterstützt.

## <a name="operating-system-support-linux"></a>Unterstützte Betriebssysteme (Linux)

Für Sicherungen von virtuellen Linux-Azure-Computern unterstützt Azure Backup [die Liste der von Azure unterstützten Linux-Distributionen](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros). Beachten Sie Folgendes:

- Azure Backup unterstützt CoreOS Linux nicht.
- Azure Backup unterstützt keine 32-Bit-Betriebssysteme.
- Andere Bring-Your-Own-Linux-Distributionen sollten funktionieren, sofern der [Azure-VM-Agent für Linux](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux) auf der VM verfügbar ist und Python unterstützt wird.
- Azure Backup unterstützt keinen über Proxy konfigurierten virtuellen Linux-Computer, wenn darauf nicht Python, Version 2.7, installiert ist.

## <a name="backup-frequency-and-retention"></a>Sicherungshäufigkeit und -aufbewahrung

**Einstellung** | **Einschränkungen**
--- | ---
Maximale Wiederherstellungspunkte pro geschützter Instanz (Computer/Workload) | 9999
Maximale Ablaufzeit für einen Wiederherstellungspunkt | Keine Begrenzung
Maximale Sicherungshäufigkeit für Tresor (Azure-VM-Erweiterung) | Einmal täglich
Maximale Sicherungshäufigkeit für Tresor (MARS-Agent) | Drei Sicherungen pro Tag
Maximale Sicherungshäufigkeit für DPM/MABS | Alle 15 Minuten für SQL Server<br/><br/> Stündlich für andere Workloads
Aufbewahrung des Wiederherstellungspunkts | Täglich, wöchentlich, monatlich und jährlich
Maximale Aufbewahrungsdauer | Abhängig von der Sicherungshäufigkeit.
Wiederherstellungspunkte auf DPM-/MABS-Datenträger | 64 für Dateiserver und 448 für App-Server<br/><br/> Für Bandwiederherstellungspunkte für lokalen DPM gelten keine Grenzwerte.

## <a name="supported-restore-methods"></a>Unterstützte Wiederherstellungsmethoden

**Wiederherstellungsmethode** | **Details**
--- | ---
Erstellen eines neuen virtuellen Computers | Sie können während des Wiederherstellungsvorgangs einen virtuellen Computer erstellen. <br/><br/> Bei dieser Option wird ein einfacher virtueller Computer erstellt und ausgeführt. Sie können den Namen des virtuellen Computers, die Ressourcengruppe, das virtuelle Netzwerk, das Subnetz und den Speicher angeben.  
Wiederherstellen eines Datenträgers | Sie können einen Datenträger wiederherstellen und zum Erstellen einer VM verwenden.<br/><br/> Wenn Sie diese Option auswählen, kopiert Azure Backup die Daten aus dem Tresor in ein ausgewähltes Speicherkonto. Mit dem Wiederherstellungsauftrag wird eine Vorlage generiert. Sie können diese Vorlage herunterladen und damit benutzerdefinierte VM-Einstellungen angeben und eine VM erstellen.<br/><br/> Bei dieser Option können Sie mehr Einstellungen als bei der vorherigen Option zum Erstellen eines virtuellen Computers angeben.<br/><br/>
Ersetzen eines vorhandenen Datenträgers | Sie können einen Datenträger wiederherstellen und dann einen derzeit vorhandenen Datenträger auf einem virtuellen Computer durch den wiederhergestellten Datenträger ersetzen.
Wiederherstellen von Dateien | Sie können Dateien von einem ausgewählten Wiederherstellungspunkt wiederherstellen. Sie laden ein Skript herunter, um den VM-Datenträger von dem Wiederherstellungspunkt einzubinden. Durchsuchen Sie die Datenträgervolumes anschließend nach Dateien und Ordnern, die wiederhergestellt werden sollen, und heben Sie dann die Einbindung des Datenträgers auf.

## <a name="support-for-file-level-restore"></a>Unterstützung für die Wiederherstellung auf Dateiebene

**Wiederherstellen** | **Unterstützt**
--- | ---
Wiederherstellen von Dateien unter verschiedenen Betriebssystemen | Sie können Dateien auf allen Computern wiederherstellen, die dasselbe (oder ein kompatibles) Betriebssystem wie der gesicherte virtuelle Computer haben. Weitere Informationen finden Sie in der Tabelle mit [kompatiblen Betriebssystemen](backup-azure-restore-files-from-vm.md#system-requirements).
Wiederherstellen von Dateien auf klassischen virtuellen Computern | Nicht unterstützt.
Wiederherstellen von Dateien von verschlüsselten virtuellen Computern | Nicht unterstützt.
Wiederherstellen von Dateien von Speicherkonten mit Netzwerkbeschränkung | Nicht unterstützt.
Wiederherstellen von Dateien auf virtuellen Computern mit Windows-Speicherplätzen | Die Wiederherstellung auf demselben Computer wird nicht unterstützt.<br/><br/> Führen Sie stattdessen die Wiederherstellung der Dateien auf einem kompatiblen virtuellen Computer durch.
Wiederherstellen von Dateien auf einem virtuellen Linux-Computer mit LVM/RAID-Arrays | Die Wiederherstellung auf demselben Computer wird nicht unterstützt.<br/><br/> Führen Sie die Wiederherstellung auf einem kompatiblen virtuellen Computer durch.
Wiederherstellen von Dateien mit spezifischen Netzwerkeinstellungen | Die Wiederherstellung auf demselben Computer wird nicht unterstützt. <br/><br/> Führen Sie die Wiederherstellung auf einem kompatiblen virtuellen Computer durch.

## <a name="support-for-vm-management"></a>Unterstützung für die Datenträgerverwaltung

In der folgenden Tabelle ist die Unterstützung für die Sicherung bei Verwaltungsaufgaben für VMs aufgeführt, z.B. das Hinzufügen oder Ersetzen von VM-Datenträgern.

**Wiederherstellen** | **Unterstützt**
--- | ---
Wiederherstellung in verschiedenen Abonnements, Regionen oder Zonen | Nicht unterstützt.
Wiederherstellung auf einem vorhandenen virtuellen Computer | Verwenden Sie die Option zum Ersetzen eines Datenträgers.
Wiederherstellen eines Datenträgers mit für die Azure-Speicherdienstverschlüsselung (SSE) aktiviertem Speicherkonto | Nicht unterstützt.<br/><br/> Führen Sie die Wiederherstellung in einem Konto aus, für das SSE nicht aktiviert ist.
Wiederherstellung in gemischten Speicherkonten |Nicht unterstützt.<br/><br/> Basierend auf dem Speicherkontotyp sind alle wiederhergestellten Datenträger entweder jeweils Premium- oder Standard-Datenträger und können nicht gemischt sein.
Wiederherstellen eines virtuellen Computers direkt in einer Verfügbarkeitsgruppe | Bei verwalteten Datenträgern können Sie den Datenträger wiederherstellen und die Option für die Verfügbarkeitsgruppe in der Vorlage verwenden.<br/><br/> Dies wird für nicht verwaltete Datenträger nicht unterstützt. Bei nicht verwalteten Datenträgern stellen Sie den Datenträger wieder her und erstellen dann einen virtuellen Computer in der Verfügbarkeitsgruppe.
Wiederherstellen einer Sicherung von nicht verwalteten VMs nach dem Upgrade auf verwaltete VMs| Unterstützt.<br/><br/> Sie können Datenträger wiederherstellen und dann einen verwalteten virtuellen Computer erstellen.
Wiederherstellen eines virtuellen Computers an einem Wiederherstellungspunkt vor der Migration des virtuellen Computers zu verwalteten Datenträgern | Unterstützt.<br/><br/> Sie führen die Wiederherstellung auf nicht verwalteten Datenträgern durch (Standardeinstellung), konvertieren die wiederhergestellten Datenträger in verwaltete Datenträger und erstellen einen virtuellen Computer mit den verwalteten Datenträgern.
Wiederherstellen eines gelöschten virtuellen Computers | Unterstützt.<br/><br/> Sie können den virtuellen Computer aus einem Wiederherstellungspunkt wiederherstellen.
Wiederherstellen eines virtuellen Domänencontrollercomputers, der Teil einer Konfiguration mit mehreren Domänencontrollern ist, über das Portal | Wird unterstützt, wenn Sie unter Verwendung von PowerShell den Datenträger wiederherstellen und eine VM erstellen.
Wiederherstellen einer VM in einem anderen virtuellen Netzwerk |Unterstützt.<br/><br/> Das virtuelle Netzwerk muss sich im selben Abonnement und derselben Region befinden.

## <a name="vm-compute-support"></a>Unterstützung für VM-Compute

**Compute** | **Unterstützung**
--- | ---
Größe des virtuellen Computers |Jede Größe von virtuellen Azure-Computern mit mindestens 2 CPU-Kernen und 1 GB RAM<br/><br/> [Weitere Informationen.](https://docs.microsoft.com/azure/virtual-machines/windows/sizes)
Sicherung virtueller Computer in [Verfügbarkeitsgruppen](https://docs.microsoft.com/azure/virtual-machine-scale-sets/availability#availability-sets) | Unterstützt.<br/><br/> Sie können eine VM in einer Verfügbarkeitsgruppe nicht mit der Option zum schnellen Erstellen einer VM wiederherstellen. Stattdessen müssen Sie bei der Wiederherstellung der VM den Datenträger wiederherstellen und damit entweder eine VM bereitstellen oder einen vorhandenen Datenträger ersetzen.
Sicherung von mit [Hybridvorteil (HUB)](https://docs.microsoft.com/azure/virtual-machines/windows/hybrid-use-benefit-licensing) bereitgestellten VMs | Unterstützt.
Sicherung von in einer [Skalierungsgruppe](https://docs.microsoft.com/azure/virtual-machine-scale-sets/overview) bereitgestellten VMs |Nicht unterstützt.
Sicherung von über den [Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps?filters=virtual-machine-images) bereitgestellten VMs<br/><br/> (Veröffentlicht von Microsoft oder Drittanbietern) |Unterstützt.<br/><br/> Auf dem virtuellen Computer muss ein unterstütztes Betriebssystem ausgeführt werden.<br/><br/> Die Wiederherstellung von Dateien auf dem virtuellen Computer kann nur unter einem kompatiblen Betriebssystem (kein früheres oder späteres Betriebssystem) durchgeführt werden. Wir stellen die Azure Marketplace-VMs, die als VMs gesichert werden, nicht wieder her, da hierfür Kaufinformationen benötigt werden (nur als Datenträger).
Sicherung von über ein benutzerdefiniertes Image bereitgestellten VMs (Drittanbieter) |Unterstützt.<br/><br/> Auf dem virtuellen Computer muss ein unterstütztes Betriebssystem ausgeführt werden.<br/><br/> Die Wiederherstellung von Dateien auf dem virtuellen Computer kann nur unter einem kompatiblen Betriebssystem (kein früheres oder späteres Betriebssystem) durchgeführt werden.
Sicherung von zu Azure migrierten VMs| Unterstützt.<br/><br/> Für die Sicherung des virtuellen Computers muss der VM-Agent auf dem migrierten Computer installiert werden.
Sichern der Konsistenz mehrerer virtueller Computer | Azure Backup bietet keine mehrere virtuelle Computer übergreifende Konsistenz von Daten und Anwendungen.
Sichern mit [Diagnoseeinstellungen](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-logs-overview)  | Nicht unterstützt. <br/><br/> Wenn die Wiederherstellung der Azure-VM mit Diagnoseeinstellungen mithilfe der Option [Neu erstellen](backup-azure-arm-restore-vms.md#create-a-vm) ausgelöst wird, tritt bei der Wiederherstellung ein Fehler auf.
Wiederherstellen von an Zonen angehefteten virtuellen Computern | Unterstützt (für virtuelle Computer, die nach Januar 2019 gesichert wurden und bei denen es [Verfügbarkeitszonen](https://azure.microsoft.com/global-infrastructure/availability-zones/) gibt).<br/><br/>Wir unterstützen derzeit die Wiederherstellung in derselben Zone, die in virtuellen Computern angeheftet ist. Wenn aber die Zone nicht verfügbar ist, schlägt die Wiederherstellung fehl.
Gen2-VMS | Unterstützt <br> Azure Backup unterstützt die Sicherung und Wiederherstellung von [Gen2-VMS](https://azure.microsoft.com/updates/generation-2-virtual-machines-in-azure-public-preview/). Bei Wiederherstellung über einen Wiederherstellungspunkt werden diese virtuellen Computer als [Gen2-VMs](https://azure.microsoft.com/updates/generation-2-virtual-machines-in-azure-public-preview/) wiederhergestellt.

## <a name="vm-storage-support"></a>Unterstützung für VM-Speicher

**Komponente** | **Unterstützung**
--- | ---
Azure-VM-Datenträger (für Daten) | Sichern eines virtuellen Computers mit maximal 16 Datenträgern
Datenträgergröße | Die Größe einzelner Datenträger kann bis zu 32 TB und maximal 256 TB für alle Datenträger in einer VM betragen.
Speichertyp | HDD Standard, SSD Standard, SSD Premium.
Verwaltete Datenträger | Unterstützt.
Verschlüsselte Datenträger | Unterstützt.<br/><br/> Virtuelle Azure-Computer mit aktiviertem Azure Disk Encryption können (mit oder ohne Azure AD-App) gesichert werden.<br/><br/> Verschlüsselte virtuelle Computer können nicht auf Datei- oder Ordnerebene wiederhergestellt werden. Stattdessen muss die gesamte VM wiederhergestellt werden.<br/><br/> Sie können die Verschlüsselung auf virtuellen Computern aktivieren, die bereits durch Azure Backup geschützt werden.
Datenträger mit aktivierter Schreibbeschleunigung | Nicht unterstützt.<br/><br/> Azure Backup schließt Datenträger mit aktivierter Schreibbeschleunigung bei der Sicherung automatisch aus. Da sie nicht gesichert werden, können Sie diese Datenträger nicht über Wiederherstellungspunkte der VM wiederherstellen.
Sichern und Wiederherstellen von deduplizierten VMs/Datenträgern | Azure Backup unterstützt nicht die Deduplizierung. Weitere Informationen finden Sie in diesem [Artikel](https://docs.microsoft.com/azure/backup/backup-support-matrix#disk-deduplication-support). <br/> <br/>  – Azure Backup dedupliziert nicht VM-übergreifend im Recovery Services-Tresor. <br/> <br/>  – Wenn es während der Wiederherstellung VMs im Deduplizierungsstatus gibt, können die Dateien nicht wieder hergestellt werden, da der Tresor das Format nicht versteht.
Hinzufügen eines Datenträgers zu geschütztem virtuellen Computer | Unterstützt.
Ändern der Datenträgergröße auf geschütztem virtuellen Computer | Unterstützt.
Freigegebener Speicher| Das Sichern von VMs mit Cluster Shared Volume (CSV) oder Dateiservern mit horizontaler Skalierung wird nicht empfohlen. Bei CSV-Schreibern treten während der Sicherung voraussichtlich Fehler auf. Bei der Wiederherstellung werden Datenträger, die CSV-Volumes enthalten, möglicherweise nicht hochgefahren.

## <a name="vm-network-support"></a>Netzwerkunterstützung bei virtuellen Computern

**Komponente** | **Unterstützung**
--- | ---
Anzahl der Netzwerkschnittstellen | Bis zur maximalen Anzahl von Netzwerkadaptern, die für eine bestimmte Größe von virtuellen Azure-Computern unterstützt werden<br/><br/> Netzwerkadapter werden erstellt, wenn der virtuelle Computer während des Wiederherstellungsvorgangs erstellt wird.<br/><br/> Die Anzahl von Netzwerkadaptern auf dem wiederhergestellten virtuellen Computer entspricht der Anzahl von Netzwerkadaptern auf dem virtuellen Computer bei Aktivierung des Schutzes. Wenn nach der Aktivierung des Schutzes Netzwerkschnittstellen entfernt werden, wirkt sich das nicht auf die Anzahl aus.
Externer und interner Lastenausgleich |Unterstützt. <br/><br/> Weitere Informationen zum Wiederherstellen von virtuellen Computern mit spezifischen Netzwerkeinstellungen finden Sie [hier](backup-azure-arm-restore-vms.md#restore-vms-with-special-configurations).
Mehrere reservierte IP-Adressen |Unterstützt. <br/><br/> Weitere Informationen zum Wiederherstellen von virtuellen Computern mit spezifischen Netzwerkeinstellungen finden Sie [hier](backup-azure-arm-restore-vms.md#restore-vms-with-special-configurations).
Virtuelle Computer mit mehreren Netzwerkadaptern| Unterstützt. <br/><br/> Weitere Informationen zum Wiederherstellen von virtuellen Computern mit spezifischen Netzwerkeinstellungen finden Sie [hier](backup-azure-arm-restore-vms.md#restore-vms-with-special-configurations).
Virtuelle Computer mit öffentlichen IP-Adressen| Unterstützt.<br/><br/> Ordnen Sie dem Netzwerkadapter eine vorhandene öffentliche IP-Adresse zu, oder erstellen Sie eine Adresse, und ordnen Sie diese nach der Wiederherstellung dem Netzwerkadapter zu.
Netzwerksicherheitsgruppe (NSG) für Netzwerkadapter oder Subnetz |Unterstützt.
Statische IP-Adresse | Nicht unterstützt.<br/><br/> Einer neuen VM, die aus einem Wiederherstellungspunkt erstellt wird, wird eine dynamische IP-Adresse zugewiesen.<br/><br/> Bei klassischen VMs kann eine VM mit einer reservierten IP-Adresse und ohne definierten Endpunkt nicht gesichert werden.
Dynamische IP-Adresse |Unterstützt.<br/><br/> Wenn der Netzwerkadapter auf der Quell-VM dynamische IP-Adressen verwendet, werden diese standardmäßig auch für den Netzwerkadapter auf der wiederhergestellten VM verwendet.
Azure Traffic Manager| Unterstützt.<br/><br/>Wenn sich die gesicherte VM in Traffic Manager befindet, müssen Sie die wiederhergestellte VM manuell in derselben Traffic Manager-Instanz hinzufügen.
Azure DNS |Unterstützt.
Benutzerdefinierter DNS |Unterstützt.
Ausgehende Verbindungen über HTTP-Proxy | Unterstützt.<br/><br/> Ein authentifizierter Proxy wird nicht unterstützt.
VNET-Dienstendpunkte| Unterstützt.<br/><br/> In den Speicherkontoeinstellungen für die Firewall und das virtuelle Netzwerk muss der Zugriff über alle Netzwerke zugelassen werden.

## <a name="vm-security-and-encryption-support"></a>Unterstützung für VM-Sicherheit und -Verschlüsselung

Azure Backup unterstützt die Verschlüsselung für Daten während der Übertragung und im Ruhezustand:

Netzwerkdatenverkehr für Azure:

- Sicherungsdatenverkehr von den Servern zum Recovery Services-Tresor wird per AES 256 (Advanced Encryption Standard) verschlüsselt.
- Sicherungsdaten werden über eine sichere HTTPS-Verbindung übertragen.
- Die Sicherungsdaten werden im Recovery Services-Tresor in verschlüsselter Form gespeichert.
- Nur Sie verfügen über die Passphrase zum Entsperren dieser Daten. Die Sicherungsdaten können zu keinem Zeitpunkt von Microsoft entschlüsselt werden.

  > [!WARNING]
  > Nachdem der Tresor eingerichtet wurde, haben nur Sie Zugriff auf den Verschlüsselungsschlüssel. Microsoft bewahrt keine Kopie auf und hat keinen Zugriff auf den Schlüssel. Wenn der Schlüssel verlegt wird, kann Microsoft die gesicherten Daten nicht wiederherstellen.

Datensicherheit:

- Beim Sichern von Azure-VMs müssen Sie die Verschlüsselung *auf der* VM einrichten.
- Azure Backup unterstützt den Azure Disk Encryption-Dienst, der auf Windows-VMs BitLocker und auf Linux-VMs **dm-crypt** verwendet.
- Auf dem Back-End nutzt Azure Backup die [Speicherdienstverschlüsselung von Azure](../storage/common/storage-service-encryption.md), mit der ruhende Daten geschützt werden.

**Computer** | **Während der Übertragung** | **Ruhende Daten**
--- | --- | ---
Lokale Windows-Computer ohne DPM/MABS | ![Ja][green] | ![Ja][green]
Virtuelle Azure-Computer | ![Ja][green] | ![Ja][green]
Lokal/Azure-VMs mit DPM | ![Ja][green] | ![Ja][green]
Lokal/Azure-VMs mit MABS | ![Ja][green] | ![Ja][green]

## <a name="vm-compression-support"></a>Unterstützung für die Komprimierung bei virtuellen Computern

Azure Backup unterstützt die Komprimierung des Sicherungsdatenverkehrs. In der folgenden Tabelle finden Sie eine Zusammenfassung. Beachten Sie Folgendes:

- Für Azure-VMs liest die VM-Erweiterung die Daten über das Speichernetzwerk direkt aus dem Azure-Speicherkonto. Es ist also nicht erforderlich, diesen Datenverkehr zu komprimieren.
- Wenn Sie DPM oder MABS verwenden, können Sie Bandbreite sparen, indem Sie die Daten vor der Sicherung in DPM/MABS komprimieren.

**Computer** | **Komprimierung für MABS/DPM (TCP)** | **Komprimierung für Tresor (HTTPS)**
--- | --- | ---
Lokale Windows-Computer ohne DPM/MABS | Nicht verfügbar | ![Ja][green]
Virtuelle Azure-Computer | Nicht verfügbar | Nicht verfügbar
Lokal/Azure-VMs mit DPM | ![Ja][green] | ![Ja][green]
Lokal/Azure-VMs mit MABS | ![Ja][green] | ![Ja][green]

## <a name="next-steps"></a>Nächste Schritte

- [Sichern virtueller Azure-Computer](backup-azure-arm-vms-prepare.md)
- [Direktes Sichern von Windows-Computern](tutorial-backup-windows-server-to-azure.md) (ohne Sicherungsserver)
- [Einrichten von MABS](backup-azure-microsoft-azure-backup.md) für die Sicherung in Azure und anschließendes Sichern von Workloads per MABS
- [Einrichten von DPM](backup-azure-dpm-introduction.md) für die Sicherung in Azure und anschließendes Sichern von Workloads per DPM

[green]: ./media/backup-support-matrix/green.png
[yellow]: ./media/backup-support-matrix/yellow.png
[red]: ./media/backup-support-matrix/red.png
