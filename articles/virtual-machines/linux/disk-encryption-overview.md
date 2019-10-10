---
title: Aktivieren von Azure Disk Encryption für Linux-VMs
description: Dieser Artikel enthält eine Anleitung zur Aktivierung von Microsoft Azure Disk Encryption für Linux-VMs.
author: msmbaldwin
ms.service: security
ms.topic: article
ms.author: mbaldwin
ms.date: 08/06/2019
ms.custom: seodec18
ms.openlocfilehash: 053c52d7d1a0282d72ad76408b77c96aa3b0e3e4
ms.sourcegitcommit: 42748f80351b336b7a5b6335786096da49febf6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2019
ms.locfileid: "72174704"
---
# <a name="azure-disk-encryption-for-linux-vms"></a>Azure Disk Encryption für Linux-VMs 

Azure Disk Encryption unterstützt Sie beim Schutz Ihrer Daten gemäß den Sicherheits- und Complianceanforderungen Ihrer Organisation. Der Dienst stellt mithilfe des [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt)-Features von Linux Volumeverschlüsselung für das Betriebssystem und die Datenträger von virtuellen Azure-Computern (VMs) bereit und ist in [Azure Key Vault](../../key-vault/index.yml) integriert, damit Sie die Verschlüsselungsschlüssel und Geheimnisse für Datenträger steuern und verwalten können. 

Falls Sie [Azure Security Center](../../security-center/index.yml) verwenden, werden Sie gewarnt, wenn VMs nicht verschlüsselt sind. Die Warnungen werden als „Hoher Schweregrad“ angezeigt. Empfohlen wird in diesem Fall die Verschlüsselung der VMs.

![Azure Security Center: Datenträgerverschlüsselungswarnung](media/disk-encryption/security-center-disk-encryption-fig1.png)

> [!WARNING]
> - Wenn Sie zuvor Azure Disk Encryption mit Azure AD zum Verschlüsseln eines virtuellen Computers verwendet haben, müssen Sie diese Option auch weiterhin zum Verschlüsseln Ihres virtuellen Computers verwenden. Weitere Informationen finden Sie unter [Azure Disk Encryption mit Azure AD (vorheriges Release)](disk-encryption-overview-aad.md). 
> - Einige Empfehlungen führen möglicherweise zu einer erhöhten Daten-, Netzwerk- oder Computeressourcenauslastung, was zusätzliche Lizenz- oder Abonnementkosten nach sich ziehen kann. Sie müssen über ein gültiges aktives Azure-Abonnement verfügen, um in den unterstützten Regionen Ressourcen in Azure zu erstellen.

Die Grundlagen von Azure Disk Encryption für Linux können Sie in den Schnellstarts [Erstellen und Verschlüsseln einer Linux-VM mit der Azure-Befehlszeilenschnittstelle](disk-encryption-cli-quickstart.md) und [Erstellen und Verschlüsseln eines virtuellen Linux-Computers mit Azure PowerShell](disk-encryption-powershell-quickstart.md) in wenigen Minuten erlernen.

## <a name="supported-vms-and-operating-systems"></a>Unterstützte VMs und Betriebssysteme

### <a name="supported-vm-sizes"></a>Unterstützte VM-Größen

Linux-VMs sind in [verschiedenen Größen](sizes.md) verfügbar. Azure Disk Encryption ist nicht verfügbar auf [Basic-VMs der A-Serie](https://azure.microsoft.com/pricing/details/virtual-machines/series/) und auf virtuellen Computern, die diese Mindestanforderungen an den Arbeitsspeicher nicht erfüllen:

| Virtueller Computer | Mindestens erforderlicher Arbeitsspeicher |
|--|--|
| Virtuelle Linux-Computer, wenn nur Datenvolumes verschlüsselt werden| 2 GB |
| Virtuelle Linux-Computer, wenn sowohl Daten- als auch Betriebssystemvolumes verschlüsselt werden und die Nutzung des Stammdateisystems 4 GB oder weniger beträgt | 8 GB |
| Virtuelle Linux-Computer, wenn sowohl Daten- als auch Betriebssystemvolumes verschlüsselt werden und die Nutzung des Stammdateisystems 4 GB oder mehr beträgt | Nutzung des Stammdateisystems x 2. Für eine Nutzung des Stammdateisystems von 16 GB sind mindestens 32 GB RAM erforderlich. |

Sobald die Verschlüsselung des Betriebssystemdatenträgers auf virtuellen Linux-Computern abgeschlossen ist, kann der virtuelle Computer so konfiguriert werden, dass er mit weniger Speicherplatz läuft. 

Azure Disk Encryption ist auch für virtuelle Computer mit Storage Premium verfügbar. 

### <a name="supported-operating-systems"></a>Unterstützte Betriebssysteme

Azure Disk Encryption wird von einer Teilmenge der [Azure zugelassenen Linux-Distributionen](endorsed-distros.md) unterstützt, die selbst eine Teilmenge aller möglichen Linux-Serverdistributionen ist.

![Venn-Diagramm zu den Linux-Serverdistributionen, die Azure Disk Encryption unterstützen.](./media/disk-encryption/ade-supported-distros.png)

Nicht von Azure zugelassene Linux-Serverdistributionen unterstützen Azure Disk Encryption nicht. Unter den zugelassenen unterstützen nur die folgenden Distributionen und Versionen Azure Disk Encryption:

| Linux-Distribution | Version | Für die Verschlüsselung unterstützter Volumetyp|
| --- | --- |--- |
| Ubuntu | 18,04| Betriebssystem- und andere Datenträger |
| Ubuntu | 16.04| Betriebssystem- und andere Datenträger |
| Ubuntu | 14.04.5</br>[für Azure optimierter Kernel aktualisiert auf 4.15 oder eine höhere Version](disk-encryption-troubleshooting.md) | Betriebssystem- und andere Datenträger |
| RHEL | 7,7 | Betriebssystem- und andere Datenträger (siehe der Hinweis unten) |
| RHEL | 7.6 | Betriebssystem- und andere Datenträger (siehe der Hinweis unten) |
| RHEL | 7,5 | Betriebssystem- und andere Datenträger (siehe der Hinweis unten) |
| RHEL | 7.4 | Betriebssystem- und andere Datenträger (siehe der Hinweis unten) |
| RHEL | 7.3 | Betriebssystem- und andere Datenträger (siehe der Hinweis unten) |
| RHEL | 7.2 | Betriebssystem- und andere Datenträger (siehe der Hinweis unten) |
| RHEL | 6,8 | Datenträger für Daten (siehe der Hinweis unten) |
| RHEL | 6.7 | Datenträger für Daten (siehe der Hinweis unten) |
| CentOS | 7,7 | Betriebssystem- und andere Datenträger |
| CentOS | 7.6 | Betriebssystem- und andere Datenträger |
| CentOS | 7,5 | Betriebssystem- und andere Datenträger |
| CentOS | 7.4 | Betriebssystem- und andere Datenträger |
| CentOS | 7.3 | Betriebssystem- und andere Datenträger |
| CentOS | 7.2n | Betriebssystem- und andere Datenträger |
| CentOS | 6,8 | Datenträger |
| openSUSE | 42.3 | Datenträger |
| SLES | 12-SP4 | Datenträger |
| SLES | 12-SP3 | Datenträger |

> [!NOTE]
> Die neue ADE-Implementierung wird für Betriebssystem- und andere Datenträger für RHEL7-Images mit nutzungsbasierter Bezahlung unterstützt. ADE wird derzeit nicht für RHEL-BYOS-Images (Bring-Your-Own-Subscription) unterstützt. 

## <a name="additional-vm-requirements"></a>Weitere VM-Anforderungen

Für Azure Disk Encryption muss das „dm-crypt“-Modul und das „vfat“-Modul im System vorhanden sein. Wenn das „vfat“-Modul aus dem Standardimage entfernt oder dort deaktiviert wird, kann das System nicht mehr das Schlüsselvolume lesen und damit auch nicht den Schlüssel abrufen, der zum Entsperren der Datenträger bei nachfolgenden Neustarts erforderlich ist. Schritte zum Härten des Systems, die das vfat-Modul aus dem System entfernen, sind nicht mit Azure Disk Encryption kompatibel. 

Vor dem Aktivieren der Verschlüsselung müssen die zu verschlüsselnden Datenträger ordnungsgemäß in „/etc/fstab“ aufgelistet werden. Verwenden Sie für diesen Eintrag einen persistenten Blockgerätenamen, da Gerätenamen im Format „/dev/sdX“ beim Neustart, insbesondere nach Anwendung der Verschlüsselung, nicht zuverlässig mit demselben Datenträger verknüpft werden können. Weitere Informationen zu diesem Verhalten finden Sie unter: [Behandeln von Problemen mit geänderten Gerätenamen von Linux-VMs](troubleshoot-device-names-problems.md)

Stellen Sie sicher, dass die Einstellungen für „/etc/fstab“ ordnungsgemäß für die Einbindung konfiguriert sind. Um diese Einstellungen zu konfigurieren, führen Sie den Befehl „mount -a“ aus, oder starten Sie die VM neu, und lösen Sie das Einbinden so erneut aus. Wenn der Vorgang abgeschlossen wurde, sehen Sie sich die Ausgabe des Befehls „lsblk“ an, um zu überprüfen, ob das Laufwerk noch eingebunden ist. 
- Wenn die Datei „/etc/fstab“ das Laufwerk vor der Aktivierung der Verschlüsselung nicht ordnungsgemäß einbindet, kann auch Azure Disk Encryption es nicht richtig einbinden.
- Azure Disk Encryption verschiebt die Einbindungsinformationen aus „/etc/fstab“ in die eigene Konfigurationsdatei als Teil des Verschlüsselungsprozesses. Der Eintrag fehlt in „/etc/fstab“, nachdem die Verschlüsselung des Datenlaufwerks abgeschlossen wurde.
- Stellen Sie vor der Verschlüsselung sicher, dass alle Dienste und Prozesse beendet wurden, die in bereitgestellten Datenträgern schreiben oder diese deaktivieren könnten, um zu verhindern, dass sie nach einem Neustart neu gestartet werden. Die Dienste und Prozesse könnten Dateien auf diesen Partitionen öffnen. So wird verhindert, dass sie nach der Verschlüsselungsprozedur nochmals bereitgestellt werden und die Verschlüsselung dadurch fehlschlägt. 
- Nach dem Neustart dauert es einige Zeit, bis Azure Disk Encryption die neu verschlüsselten Datenträger eingebunden hat. Sie sind nach einem Neustart nicht sofort verfügbar. Der Prozess benötigt Zeit, um die verschlüsselten Laufwerke zu starten, zu entsperren und dann einzubinden, bevor sie für andere Prozesse verfügbar sind. Dies kann je nach Systemeigenschaften mehr als eine Minute nach dem Neustart dauern.

Ein Beispiel für Befehle, die zum Einbinden der Datenträger und zum Erstellen der erforderlichen Einträge in „/etc/fstab“ verwendet werden können, finden Sie im [CLI-Skript für die Voraussetzungen für Azure Disk Encryption](https://github.com/ejarvi/ade-cli-getting-started) (Zeilen 244–248) und im [PowerShell-Skript für die Voraussetzungen für Azure Disk Encryption](https://github.com/Azure/azure-powershell/tree/master/src/Compute/Compute/Extension/AzureDiskEncryption/Scripts). 

## <a name="networking-requirements"></a>Netzwerkanforderungen

Die Linux-VMs müssen die folgenden Anforderungen an die Konfiguration des Netzwerkendpunkts erfüllen, um Azure Disk Encryption zu aktivieren:
  - Um ein Token für die Verbindungsherstellung mit Ihrem Schlüsseltresor zu erhalten, muss die Linux-VM eine Verbindung mit dem Azure Active Directory-Endpunkt \[login.microsoftonline.com\] herstellen können.
  - Um die Verschlüsselungsschlüssel in Ihren Schlüsseltresor schreiben zu können, muss die Linux-VM eine Verbindung mit dem Schlüsseltresor-Endpunkt herstellen können.
  - Die Linux-VM muss eine Verbindung mit dem Azure Storage-Endpunkt herstellen können, an dem das Azure-Erweiterungsrepository gehostet wird, sowie mit einem Azure Storage-Konto, das die VHD-Dateien hostet.
  -  Wenn Ihre Sicherheitsrichtlinie den Zugriff von virtuellen Azure-Computern auf das Internet beschränkt, können Sie den obigen URI auflösen und eine spezielle Regel konfigurieren, um ausgehende Verbindungen mit den IP-Adressen zuzulassen. Weitere Informationen finden Sie unter [Zugreifen auf Azure Key Vault hinter einer Firewall](../../key-vault/key-vault-access-behind-firewall.md).  

## <a name="encryption-key-storage-requirements"></a>Speicheranforderungen für Verschlüsselungsschlüssel  

Azure Disk Encryption erfordert Azure Key Vault zum Steuern und Verwalten von Verschlüsselungsschlüsseln und Geheimnissen für die Datenträgerverschlüsselung. Ihr Schlüsseltresor und die VMs müssen sich in derselben Azure-Region und im selben Azure-Abonnement befinden.

Ausführliche Informationen finden Sie unter [Erstellen und Konfigurieren eines Schlüsseltresors für Azure Disk Encryption](disk-encryption-key-vault.md).

## <a name="terminology"></a>Begriff
In der folgenden Tabelle werden einige der häufig in der Azure Disk Encryption-Dokumentation verwendeten Begriffe beschrieben:

| Begriff | Definition |
| --- | --- |
| Azure Key Vault | Key Vault ist ein Dienst zum Verwalten kryptografischer Schlüssel, der auf Hardwaresicherheitsmodulen mit FIPS-Überprüfung (Federal Information Processing Standards) basiert. Diese Standards tragen dazu bei, Ihre kryptografischen Schlüssel und vertraulichen Geheimnisse zu schützen. Weitere Informationen finden Sie in der Dokumentation zu [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) und unter [Erstellen und Konfigurieren eines Schlüsseltresors für Azure Disk Encryption](disk-encryption-key-vault.md). |
| Azure-Befehlszeilenschnittstelle | [Azure CLI](/cli/azure/install-azure-cli) ist für die Verwaltung von Azure-Ressourcen über die Befehlszeile optimiert.|
| DM-Crypt |[DM-Crypt](https://gitlab.com/cryptsetup/cryptsetup/wikis/DMCrypt) ist das Linux-basierte transparente Subsystem für die Datenträgerverschlüsselung, das zum Aktivieren der Datenträgerverschlüsselung auf virtuellen Linux-Computern verwendet wird. |
| Schlüsselverschlüsselungsschlüssel (Key encryption key, KEK) | Der asymmetrische Schlüssel (RSA 2048), der zum Schützen oder Umschließen des Geheimnisses verwendet wird. Sie können einen mit Hardwaresicherheitsmodulen geschützten Schlüssel oder einen Schlüssel mit Softwareschutz bereitstellen. Weitere Informationen finden Sie in der Dokumentation zu [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) und unter [Erstellen und Konfigurieren eines Schlüsseltresors für Azure Disk Encryption](disk-encryption-key-vault.md). |
| PowerShell-Cmdlets | Weitere Informationen finden Sie unter [Azure PowerShell-Cmdlets](/powershell/azure/overview). |


## <a name="next-steps"></a>Nächste Schritte

- [Schnellstart: Erstellen und Verschlüsseln einer Linux-VM mit der Azure-Befehlszeilenschnittstelle](disk-encryption-cli-quickstart.md)
- [Schnellstart: Erstellen und Verschlüsseln eines virtuellen Linux-Computers mit Azure PowerShell](disk-encryption-powershell-quickstart.md)
- [Azure Disk Encryption-Szenarien auf virtuellen Linux-Computern](disk-encryption-linux.md)
- [CLI-Skript für die Voraussetzungen für Azure Disk Encryption](https://github.com/ejarvi/ade-cli-getting-started)
- [PowerShell-Skript für die Voraussetzungen für Azure Disk Encryption](https://github.com/Azure/azure-powershell/tree/master/src/Compute/Compute/Extension/AzureDiskEncryption/Scripts)
- [Erstellen und Konfigurieren eines Schlüsseltresors für Azure Disk Encryption](disk-encryption-key-vault.md)


