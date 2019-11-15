---
title: include file
description: include file
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 11/06/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 7ceff623c6559ef5e929d6d5bff9e07cca9039d2
ms.sourcegitcommit: 018e3b40e212915ed7a77258ac2a8e3a660aaef8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73796281"
---
## <a name="benefits-of-managed-disks"></a>Vorteile von verwalteten Datenträgern

Hier werden einige Vorteile beschrieben, in deren Genuss Sie bei der Verwendung von verwalteten Datenträgern kommen.

### <a name="highly-durable-and-available"></a>Hoch verfügbar und stabil

Verwaltete Datenträger sind auf eine Verfügbarkeit von 99,999% ausgelegt. Hierzu stellen verwaltete Datenträger drei Replikate Ihrer Daten für Sie bereit, um eine hohe Dauerhaftigkeit zu garantieren. Wenn bei einem oder sogar bei zwei Ihrer Replikate Probleme auftreten, stellen Sie mit den übrigen Replikaten die Persistenz Ihrer Daten und eine hohe Fehlertoleranz sicher. Mit dieser Architektur konnte Azure für Infrastructure-as-a-Service-Datenträger (IaaS) durchgängig eine Dauerhaftigkeit auf Unternehmensniveau mit einer branchenweit führenden auf das Jahr umgerechneten Fehlerrate von 0% bereitstellen.

### <a name="simple-and-scalable-vm-deployment"></a>Einfache und skalierbare VM-Bereitstellung

Mit verwalteten Datenträgern können Sie für ein Abonnement pro Region bis zu 50.000VM-**Datenträger** eines Typs erstellen – Sie können also Tausende von **VMs** in einem einzigen Abonnement erstellen. Außerdem wird mit diesem Feature die Skalierbarkeit von [VM-Skalierungsgruppen](../articles/virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) weiter erhöht, sodass Sie mit einem Marketplace-Image bis zu 1.000VMs in einer VM-Skalierungsgruppe erstellen können.

### <a name="integration-with-availability-sets"></a>Integration in Verfügbarkeitsgruppen

Verwaltete Datenträger werden in Verfügbarkeitsgruppen integriert, um sicherzustellen, dass die Datenträger von [VMs in einer Verfügbarkeitsgruppe](../articles/virtual-machines/windows/manage-availability.md#use-managed-disks-for-vms-in-an-availability-set) ausreichend voneinander isoliert sind, um einzelne Fehlerquellen zu vermeiden. Datenträger werden automatisch in unterschiedliche Speicherskalierungseinheiten („Stamps“) platziert. Wenn ein Stamp aufgrund eines Hardware- oder Softwarefehlers ausfällt, treten nur für die VM-Instanzen mit Datenträgern auf diesen Stamps Fehler auf. Angenommen, Sie führen eine Anwendung auf fünf VMs aus, und die VMs sind in einer Verfügbarkeitsgruppe enthalten. Die Datenträger für diese VMs werden nicht alle auf demselben Stamp gespeichert, damit die anderen Instanzen der Anwendung weiter ausgeführt werden, wenn ein Stamp ausfällt.

### <a name="integration-with-availability-zones"></a>Integration in Verfügbarkeitszonen

Verwaltete Datenträger unterstützen [Verfügbarkeitszonen](../articles/availability-zones/az-overview.md). Diese sind ein Hochverfügbarkeitsangebot, das Anwendungen vor Ausfällen von Rechenzentren schützt. Verfügbarkeitszonen sind eindeutige physische Standorte in einer Azure-Region. Jede Zone besteht aus mindestens einem Rechenzentrum, dessen Stromversorgung, Kühlung und Netzwerkbetrieb unabhängig funktionieren. Zur Gewährleistung der Resilienz sind in allen aktivierten Regionen mindestens drei separate Zonen vorhanden. Mit Verfügbarkeitszonen bietet Azure die branchenweit beste Betriebszeit-SLA von 99,99 % für VMs.

### <a name="azure-backup-support"></a>Azure Backup-Unterstützung

Zum Schutz vor regionalen Katastrophen kann mit [Azure Backup](../articles/backup/backup-overview.md) ein Sicherungsauftrag mit zeitbasierten Sicherungen und Richtlinien zur Sicherungsaufbewahrung erstellt werden. So können Sie einfach nach Belieben VM-Wiederherstellungen ausführen. Azure Backup unterstützt derzeit Datenträgergrößen von bis zu vier Tebibyte (TiB).  Azure Backup unterstützt die Sicherung und Wiederherstellung von verwalteten Datenträgern. [Weitere Informationen](../articles/backup/backup-support-matrix-iaas.md) zur Unterstützung der Azure-VM-Sicherung.

### <a name="granular-access-control"></a>Genau abgestimmte Zugriffssteuerung

Sie können die [Rollenbasierte Zugriffssteuerung in Azure (RBAC)](../articles/role-based-access-control/overview.md) verwenden, um die spezifischen Berechtigungen für einen verwalteten Datenträger einem oder mehreren Benutzern zuzuweisen. Verwaltete Datenträger bieten viele verschiedene Vorgänge, z.B. Lesen, Schreiben (Erstellen/Aktualisieren), Löschen und Abrufen eines [SAS-URI (Shared Access Signature)](../articles/storage/common/storage-dotnet-shared-access-signature-part-1.md) für den Datenträger. Sie haben die Möglichkeit, Personen nur Zugriff auf die Vorgänge zu gewähren, die sie jeweils benötigen, um ihre Aufgaben zu erledigen. Wenn Sie es für eine Person beispielsweise nicht zulassen möchten, dass sie einen verwalteten Datenträger auf ein Speicherkonto kopiert, können Sie festlegen, dass der Zugriff auf die Exportaktion für diesen verwalteten Datenträger nicht gewährt wird. Wenn Sie nicht möchten, dass eine Person einen SAS-URI zum Kopieren eines verwalteten Datenträgers verwendet, können Sie auch festlegen, dass diese Berechtigung für den verwalteten Datenträger nicht gewährt wird.

### <a name="upload-your-vhd"></a>Hochladen der VHD

 Mit dem direkten Upload ist es einfach, Ihre VHD auf einen verwalteten Azure-Datenträger zu übertragen. Bisher war die Übertragung Ihrer Daten eher umständlich und umfasste das Staging der Daten in einem Speicherkonto. Jetzt sind weniger Schritte erforderlich. Es ist einfacher, lokale VMs in Azure hochzuladen (Upload auf große verwaltete Datenträger), und der Sicherungs- und Wiederherstellungsvorgang wurde vereinfacht. Außerdem werden hierdurch die Kosten gesenkt, weil Sie Daten direkt auf verwaltete Datenträger hochladen können, ohne diese an VMs anfügen zu müssen. Sie können direkte Uploads nutzen, um VHDs mit einer Größe von bis zu 32 TiB hochzuladen.

 Informationen zur Übertragung Ihrer VHD in Azure finden Sie im Artikel zur [CLI](../articles/virtual-machines/linux/disks-upload-vhd-to-managed-disk-cli.md) bzw. zu [PowerShell](../articles/virtual-machines/windows/disks-upload-vhd-to-managed-disk-powershell.md).

## <a name="encryption"></a>Verschlüsselung

Verwaltete Datenträger bieten zwei verschiedene Arten der Verschlüsselung. Die erste ist die serverseitige Verschlüsselung (Server Side Encryption, SSE), die vom Speicherdienst durchgeführt wird. Die zweite ist Azure Disk Encryption (ADE), die Sie für Datenträger für das Betriebssystem und die Daten Ihrer virtuellen Computer aktivieren können.

### <a name="server-side-encryption"></a>Serverseitige Verschlüsselung

Die [serverseitige Verschlüsselung von Azure](../articles/virtual-machines/windows/disk-encryption.md) bietet eine Verschlüsselung ruhender Daten und schützt Ihre Daten, um die Sicherheits- und Compliancevorgaben Ihrer Organisation zu erfüllen. Die serverseitige Verschlüsselung ist standardmäßig für alle verwalteten Datenträger, Momentaufnahmen und Images in allen Regionen aktiviert, in denen Managed Disks verfügbar ist. Sie können zulassen, dass Azure die Schlüssel für Sie verwaltet. Dies sind von der Plattform verwaltete Schlüssel. Alternativ können Sie die Schlüssel selbst verwalten. Dabei handelt es sich um vom Kunden verwaltete Schlüssel (Vorschau). Weitere Einzelheiten finden Sie auf der Seite mit [FAQs zu Managed Disks](../articles/virtual-machines/windows/faq-for-disks.md#managed-disks-and-storage-service-encryption).

### <a name="azure-disk-encryption"></a>Azure Disk Encryption

Mit Azure Disk Encryption können Sie die Betriebssystemdatenträger und andere Datenträger verschlüsseln, die von einem virtuellen IaaS-Computer verwendet werden. Diese Verschlüsselung umfasst verwaltete Datenträger. Unter Windows werden Laufwerke mit branchenüblicher BitLocker-Verschlüsselung verschlüsselt. Unter Linux werden Datenträger mit der DM-Crypt-Technologie verschlüsselt. Dieser Verschlüsselungsvorgang ist in Azure Key Vault integriert, damit Sie die Datenträger-Verschlüsselungsschlüssel steuern und verwalten können. Weitere Informationen finden Sie unter [Azure Disk Encryption für virtuelle IaaS-Computer](../articles/security/azure-security-disk-encryption-overview.md).

## <a name="disk-roles"></a>Datenträgerrollen

Es gibt drei Hauptdatenträgerrollen in Azure: der Datenträger, der BS-Datenträger und der temporäre Datenträger. Diese Rollen werden den Datenträgern zugeordnet, die an Ihren virtuelle Computer angefügt sind.

![Datenträgerrollen in Aktion](media/virtual-machines-managed-disks-overview/disk-types.png)

### <a name="data-disk"></a>Datenträger

Ein Datenträger für Daten ist ein verwalteter Datenträger, der zum Speichern von Anwendungsdaten oder anderen Daten, die Sie aufbewahren müssen, an einen virtuellen Computer angebunden ist. Datenträger werden als SCSI-Laufwerke registriert und mit einem von Ihnen ausgewählten Buchstaben gekennzeichnet. Jeder Datenträger weist eine maximale Kapazität von 32.767 GiB (Gibibyte) auf. Die Größe des virtuellen Computers bestimmt die Anzahl der Datenträger, die Sie anfügen können, und den Typ des Speichers, den Sie zum Hosten der Datenträger verwenden können.

### <a name="os-disk"></a>Betriebssystem-Datenträger

Jedem virtuellen Computer ist ein Betriebssystem-Datenträger zugeordnet. Dieser Betriebssystem-Datenträger verfügt über ein vorinstalliertes Betriebssystem, das beim Erstellen des virtuellen Computers ausgewählt wurde. Dieser Datenträger enthält das Startvolume.

Dieser Datenträger weist eine maximale Kapazität von 2.048 GiB auf.

### <a name="temporary-disk"></a>Temporärer Datenträger

Jeder virtuelle Computer enthält einen temporären Datenträger, der kein verwalteter Datenträger ist. Der temporäre Datenträger bietet kurzfristigen Speicher für Anwendungen und Prozesse und ist ausschließlich dafür ausgelegt, Daten wie z.B. Seiten-oder Auslagerungsdateien zu speichern. Daten auf dem temporären Datenträger können während eines [Wartungsereignisses](../articles/virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#understand-vm-reboots---maintenance-vs-downtime) verloren gehen oder wenn Sie eine [VM erneut bereitstellen](../articles/virtual-machines/troubleshooting/redeploy-to-new-node-windows.md?toc=%2Fazure%2Fvirtual-machines%2Fwindows%2Ftoc.json). Bei Azure Linux-VMs ist der temporäre Datenträger standardmäßig „/dev/sdb“ und bei Windows-VMs ist der temporäre Datenträger standardmäßig „D:“. Während eines erfolgreichen standardmäßigen Neustarts der VM bleiben die Daten auf dem temporären Datenträger erhalten.

## <a name="managed-disk-snapshots"></a>Momentaufnahmen eines verwalteten Datenträgers

Bei einer Momentaufnahme eines verwalteten Datenträgers handelt es sich um eine absturzkonsistente, schreibgeschützte vollständige Kopie eines verwalteten Datenträgers, die standardmäßig als verwalteter Standarddatenträger gespeichert wird. Mit Momentaufnahmen können Sie Ihre verwalteten Datenträger jederzeit sichern. Diese Momentaufnahmen existieren unabhängig vom Quelldatenträger und können zum Erstellen neuer verwalteter Datenträger verwendet werden. 

Momentaufnahmen werden auf Basis der verwendeten Größe in Rechnung gestellt. Wenn Sie beispielsweise eine Momentaufnahme eines verwalteten Datenträgers mit einer bereitgestellten Kapazität von 64GiB und einer tatsächlichen Datengröße von 10GiB erstellen, wird die Momentaufnahme nur für die in Anspruch genommene Datengröße von 10GiB in Rechnung gestellt. Sie können die verwendete Größe Ihrer Momentaufnahmen im [Azure-Nutzungsbericht](https://docs.microsoft.com/azure/billing/billing-understand-your-bill) ansehen. Beispiel: Beträgt die verwendete Datengröße einer Momentaufnahme 10 GiB, wird im **täglichen** Nutzungsbericht als verbrauchte Menge Folgendes angezeigt: 10 GiB/(31 Tage) = 0,3226.

Weitere Informationen zur Erstellung von Momentaufnahmen für verwaltete Datenträger finden Sie in den folgenden Ressourcen:

* [Erstellen einer Momentaufnahme](../articles/virtual-machines/windows/snapshot-copy-managed-disk.md) (Windows)
* [Erstellen einer Momentaufnahme](../articles/virtual-machines/linux/snapshot-copy-managed-disk.md) (Linux)

### <a name="images"></a>Bilder

Für verwaltete Datenträger wird auch die Erstellung eines verwalteten benutzerdefinierten Image unterstützt. Sie können ein Image aus Ihrer benutzerdefinierten VHD in einem Speicherkonto oder direkt von einer generalisierten VM (mit Systemvorbereitung) erstellen. Dieser Prozess erfasst ein einzelnes Image. Dieses Image enthält alle verwalteten Datenträger, die einer VM zugeordnet sind, einschließlich der Datenträger für das Betriebssystem und für die Daten. Dieses verwaltete benutzerdefinierte Image ermöglicht die Erstellung von Hunderten von VMs mit Ihrem benutzerdefinierten Image, ohne dass Sie Speicherkonten kopieren oder verwalten müssen.

Informationen zum Erstellen von Images finden Sie in den folgenden Artikeln:

* [How to capture a managed image of a generalized VM in Azure](../articles/virtual-machines/windows/capture-image-resource.md) (Erstellen eines verwalteten Image eines generalisierten virtuellen Computers in Azure)
* [Generalisieren und Erfassen eines virtuellen Linux-Computers mithilfe der Azure CLI](../articles/virtual-machines/linux/capture-image.md)

#### <a name="images-versus-snapshots"></a>Vergleich von Images und Momentaufnahmen

Es ist wichtig, den Unterschied zwischen Images und Momentaufnahmen zu verstehen. Bei verwalteten Datenträgern können Sie ein Image einer generalisierten VM erstellen, deren Zuordnung aufgehoben wurde. Dieses Image umfasst alle Datenträger, die der VM angefügt sind. Sie können dieses Image verwenden, um eine VM mit allen Datenträgern zu erstellen.

Eine Momentaufnahme ist eine Kopie eines Datenträgers zum Zeitpunkt der Erstellung der Momentaufnahme. Sie gilt nur für einen einzigen Datenträger. Wenn Sie eine VM mit einem einzigen Datenträger (Betriebssystem-Datenträger) verwenden, können Sie eine Momentaufnahme oder ein Image davon erstellen und dann entweder aus der Momentaufnahme oder dem Image eine VM erstellen.

Eine Momentaufnahme bezieht sich nur auf den Datenträger, von dem sie erstellt wurde. Darum ist ihre Verwendung problematisch in Szenarien, die die Koordination mehrerer Datenträger erfordern, z.B. Striping. Momentaufnahmen müssten sich miteinander abstimmen, und dies wird derzeit nicht unterstützt.

## <a name="disk-allocation-and-performance"></a>Datenträgerzuordnung und Leistung

Das folgende Diagramm zeigt die Echtzeitzuweisung von Bandbreite und IOPS für Datenträger unter Verwendung eines dreistufigen Bereitstellungssystems:

![Dreistufiges Bereitstellungssystem, das die Zuweisung von Bandbreite und IOPS zeigt](media/virtual-machines-managed-disks-overview/real-time-disk-allocation.png)

Die Bereitstellung auf der ersten Stufe legt die Zuweisung von IOPS und Bandbreite pro Datenträger fest.  Auf der zweiten Stufe implementiert der Compute-Serverhost die SSD-Bereitstellung und wendet sie nur auf Daten an, die auf der SSD des Servers gespeichert sind. Dies umfasst Datenträger mit Caching (ReadWrite und ReadOnly) sowie lokale und temporäre Datenträger. Schließlich erfolgt die VM-Netzwerkbereitstellung auf der dritten Stufe für alle E/A-Vorgänge, die der Computehost an das Back-End von Azure Storage sendet. Bei diesem Schema hängt die Leistung einer VM von einer Vielzahl von Faktoren ab, z. B. wie die VM die lokale SSD verwendet, von der Anzahl der angeschlossenen Datenträger sowie von der Leistung und dem Cachetyp der angeschlossenen Datenträger.

Als Beispiel für diese Einschränkungen wird eine Standard_DS1v1-VM daran gehindert, das Potenzial von 5.000 IOPS eines P30-Datenträgers aufgrund von Einschränkungen auf SSD- und Netzwerkebene zu erreichen, unabhängig davon, ob ein Cache verwendet wird:

![Standard_DS1v1-Beispielzuordnung](media/virtual-machines-managed-disks-overview/example-vm-allocation.png)

Azure verwendet einen priorisierten Netzwerkkanal für den Datenverkehr des Datenträgers, der eine höhere Priorität gegenüber anderen niedrigen Prioritäten des Netzwerkdatenverkehrs erhält. Dies hilft den Datenträgern, ihre erwartete Leistung im Falle von Netzwerkkonflikten aufrechtzuerhalten. Ebenso behandelt Azure Storage Ressourcenkonflikte und andere Probleme im Hintergrund durch den automatischen Lastenausgleich. Azure Storage weist die erforderlichen Ressourcen zu, wenn Sie einen Datenträger erstellen, und wendet einen proaktiven und reaktiven Ausgleich der Ressourcen an, um den Datenverkehr zu bewältigen. Dies stellt weiterhin sicher, dass die Datenträger ihre erwarteten IOPS- und Durchsatzziele einhalten können. Sie können die Metriken auf VM- und Datenträgerebene verwenden, um die Leistungs- und Setupwarnungen bei Bedarf zu verfolgen.

Lesen Sie unseren Artikel [Entwerfen für hohe Leistung](../articles/virtual-machines/windows/premium-storage-performance.md), um die bewährten Methoden zur Optimierung von VM- und Datenträgerkonfigurationen kennenzulernen, damit Sie die gewünschte Leistung erreichen können.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie in unserem Artikel über Datenträgertypen mehr über die einzelnen Datenträgertypen, die Azure bietet, welcher Typ sich gut für Ihre Anforderungen eignet, und erhalten Sie Informationen über deren Leistungsziele.
