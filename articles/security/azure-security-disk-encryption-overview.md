---
title: Was ist Azure Disk Encryption?
description: Dieser Artikel enthält eine Übersicht zu Azure Disk Encryption.
author: msmbaldwin
ms.service: security
ms.topic: article
ms.author: mbaldwin
ms.date: 07/17/2019
ms.custom: seodec18
ms.openlocfilehash: a67f19f0823827dad74e7aba15a92d696fbf580b
ms.sourcegitcommit: 770b060438122f090ab90d81e3ff2f023455213b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/17/2019
ms.locfileid: "68304269"
---
# <a name="azure-disk-encryption-overview"></a>Übersicht über Azure Disk Encryption

Azure Disk Encryption unterstützt Sie beim Schutz Ihrer Daten gemäß den Sicherheits- und Complianceanforderungen Ihrer Organisation. Hierfür wird das [BitLocker](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-overview)-Feature von Windows und das [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt)-Feature von Linux verwendet, um Volumeverschlüsselung für das Betriebssystem und die Datenträger von virtuellen Azure-Computern (VMs) bereitzustellen. Azure Disk Encryption ist außerdem in [Azure Key Vault](https://azure.microsoft.com/documentation/services/key-vault/) integriert, damit Sie die Schlüssel für die Datenträgerverschlüsselung und die Geheimnisse steuern und verwalten können. So wird sichergestellt, dass alle Daten auf den Datenträgern von virtuellen Computern im Ruhezustand im Azure-Speicher verschlüsselt sind. Azure Disk Encryption für virtuelle Windows- und Linux-Computer befindet sich in allen öffentlichen Azure-Regionen und Azure Government-Regionen für virtuelle Standardcomputer und virtuelle Computer mit Azure Storage Premium in der Phase „Allgemeine Verfügbarkeit“. 

Falls Sie Azure Security Center verwenden, werden Sie gewarnt, wenn VMs nicht verschlüsselt sind. Die Warnungen werden als „Hoher Schweregrad“ angezeigt. Empfohlen wird in diesem Fall die Verschlüsselung der VMs.

![Azure Security Center: Datenträgerverschlüsselungswarnung](media/azure-security-disk-encryption/security-center-disk-encryption-fig1.png)

> [!NOTE]
> Einige Empfehlungen führen möglicherweise zu einer erhöhten Daten-, Netzwerk- oder Computeressourcenauslastung und ziehen zusätzliche Lizenz- oder Abonnementkosten nach sich.


## <a name="encryption-scenarios"></a>Verschlüsselungsszenarien

Mit Azure Disk Encryption können Sie die Anforderungen an Unternehmenssicherheit und Compliance erfüllen, indem Sie Ihre virtuellen Azure-Computer im Ruhezustand mithilfe von Verschlüsselungstechnologie nach Branchenstandard schützen. Sie können virtuelle Computer auch so konfigurieren, dass diese mit vom Kunden gesteuerten Schlüsseln und Richtlinien gestartet werden (Bring Your Own Key, BYOK) und Sie den Verbrauch dieser Schlüssel in Ihrem Schlüsseltresor überwachen können.

Azure Disk Encryption unterstützt die folgenden Kundenszenarios:

* Aktivieren und Deaktivieren der Verschlüsselung auf neuen virtuellen Computern, die über unterstützte Azure-Katalogimages erstellt werden
* Aktivieren und Deaktivieren der Verschlüsselung auf vorhandenen virtuellen Computern, die in Azure ausgeführt werden
* Aktivieren und Deaktivieren der Verschlüsselung auf neuen virtuellen Windows-Computern, die mit einer vorverschlüsselten VHD und Verschlüsselungsschlüsseln erstellt werden
* Aktivieren und Deaktivieren der Verschlüsselung auf virtuellen Windows-Computern in Skalierungsgruppen
* Aktivieren und Deaktivieren der Verschlüsselung für Datenlaufwerke auf virtuellen Linux-Computern in Skalierungsgruppen
* Aktivieren und Deaktivieren der Verschlüsselung von verwalteten Datenträgern auf virtuellen Computern
* Aktualisieren von Verschlüsselungseinstellungen eines vorhandenen verschlüsselten virtuellen Computers (mit oder ohne Storage Premium)
* Sichern und Wiederherstellen von verschlüsselten virtuellen Computern
* Szenarios mit Bring Your Own Encryption (BYOE) und Bring Your Own Key (BYOK), in denen die Kunden ihre eigenen Verschlüsselungsschlüssel verwenden und diese in einem Azure-Schlüsseltresor speichern

Außerdem werden die folgenden Szenarios für virtuelle Computer unterstützt, wenn diese in Microsoft Azure aktiviert sind:

* Integration in Azure Key Vault
* [Virtuelle Computer der Ebene „Standard“](https://azure.microsoft.com/pricing/details/virtual-machines/), die die [Mindestanforderung an Arbeitsspeicher](azure-security-disk-encryption-prerequisites.md#supported-vm-sizes) erfüllen. 
* Aktivieren der Verschlüsselung auf virtuellen Windows- und Linux-Computern, auf virtuellen Computern mit verwalteten Datenträgern und auf virtuellen Computern in einer Skalierungsgruppe aus den unterstützten Azure-Katalogimages
* Deaktivieren der Verschlüsselung für Betriebssystem- und Datenlaufwerke auf virtuellen Windows-Computern, virtuellen Computern in einer Skalierungsgruppe und virtuellen Computern mit verwalteten Datenträgern
* Deaktivieren der Verschlüsselung für Datenlaufwerke auf virtuellen Linux-Computern, virtuellen Computern in einer Skalierungsgruppe und virtuellen Computern mit verwalteten Datenträgern
* Aktivieren der Verschlüsselung auf virtuellen Computern mit dem Windows-Clientbetriebssystem
* Aktivieren der Verschlüsselung auf Volumes mit Einbindungspfaden
* Aktivieren der Verschlüsselung auf virtuellen Linux-Computern, die mithilfe von mdadm mit Datenträgerstriping (RAID) konfiguriert sind
* Aktivieren der Verschlüsselung auf virtuellen Linux-Computern, die LVM für Datenträger verwenden
* Aktivieren der Verschlüsselung für das Betriebssystem und Datenträger auf virtuellen Linux-Computern

   > [!NOTE]
   > Die Verschlüsselung von Betriebssystemlaufwerken wird für einige Linux-Distributionen nicht unterstützt. Weitere Informationen finden Sie im Artikel [Von Azure Disk Encryption unterstützte Betriebssysteme: Linux](azure-security-disk-encryption-prerequisites.md#linux).
   
* Aktivieren der Verschlüsselung auf virtuellen Computern, die mit Windows-Speicherplätzen ab Windows Server 2016 konfiguriert sind Direkte Speicherplätze (S2D) werden noch nicht unterstützt.
* Sichern und Wiederherstellen verschlüsselter virtueller Computer in Szenarios mit und ohne Schlüsselverschlüsselungsschlüssel (Key Encryption Key, KEK)

Die folgenden Szenarios, Features und Technologien werden von Azure Disk Encryption nicht unterstützt:

* Verschlüsseln von virtuellen Computern der Ebene „Standard“ und von virtuellen Computern, die mithilfe der klassischen Erstellungsmethode für virtuelle Computer erstellt wurden
* Deaktivieren der Verschlüsselung für ein Betriebssystemlaufwerk oder Datenlaufwerk auf einem virtuellen Linux-Computer, wenn das Betriebssystemlaufwerk verschlüsselt ist
* Verschlüsselung von Betriebssystemlaufwerken auf virtuellen Linux-Computern in Skalierungsgruppen
* Verschlüsseln virtueller Windows-Computer, die mit softwarebasierten RAID-Systemen konfiguriert sind
* Verschlüsseln benutzerdefinierter Images auf virtuellen Linux-Computern
* Integration mit einem lokalen Schlüsselverwaltungssystem
* Azure Files (freigegebenes Dateisystem)
* Network File System (NFS)
* Dynamische Volumes

## <a name="encryption-features"></a>Verschlüsselungsfunktionen

Beim Aktivieren und Bereitstellen von Azure Disk Encryption für virtuelle Azure-Computer können Sie die folgenden Funktionen so konfigurieren, dass diese aktiviert sind:

* Verschlüsseln des Betriebssystemvolumes zum Schützen des Startvolumes im Ruhezustand in Ihrem Speicher
* Verschlüsseln der Datenvolumes zum Schützen der Datenvolumes im Ruhezustand in Ihrem Speicher
* Deaktivieren der Verschlüsselung für Betriebssystem- und Datenlaufwerke auf virtuellen Windows-Computern
* Deaktivieren der Verschlüsselung für Datenlaufwerke auf virtuellen Linux-Computern (nur wenn das Betriebssystemlaufwerk nicht verschlüsselt ist)
* Schützen der Verschlüsselungsschlüssel und Geheimnisse in Ihrem Azure Key Vault-Abonnement
* Melden des Verschlüsselungsstatus des verschlüsselten virtuellen Computers
* Entfernen der Konfigurationseinstellungen für die Datenträgerverschlüsselung vom virtuellen Computer
* Sichern und Wiederherstellen verschlüsselter virtueller Computer mit dem Azure Backup-Dienst

Azure Disk Encryption für virtuelle Windows- und Linux-Computer enthält Folgendes:

* [Disk Encryption-Erweiterung für Windows](../virtual-machines/extensions/azure-disk-enc-windows.md)
* [Disk Encryption-Erweiterung für Linux](../virtual-machines/extensions/azure-disk-enc-linux.md)
* [PowerShell-Cmdlets für die Datenträgerverschlüsselung](/powershell/module/az.compute/set-azvmdiskencryptionextension?view=azps-2.2.0)
* [Azure CLI-Cmdlets für die Datenträgerverschlüsselung](/cli/azure/vm/encryption?view=azure-cli-latest)
* [Azure Resource Manager-Vorlagen für die Datenträgerverschlüsselung](azure-security-disk-encryption-appendix.md#resource-manager-templates)

> [!NOTE]
> Für die Verschlüsselung von VM-Datenträgern mit Azure Disk Encryption fallen keine zusätzlichen Gebühren an. Für den Schlüsseltresor, in dem die Verschlüsselungsschlüssel gespeichert werden, gelten die unter [Key Vault – Preise](https://azure.microsoft.com/pricing/details/key-vault/) aufgeführten Standardpreise. 

## <a name="encryption-workflow"></a>Verschlüsselungsworkflow

Führen Sie folgende Schritte aus, um die Datenträgerverschlüsselung für virtuelle Windows- und Linux-Computer zu aktivieren:

1. Aktivieren Sie die Datenträgerverschlüsselung über eine Azure Disk Encryption Resource Manager-Vorlage, PowerShell-Cmdlets oder Azure CLI, und geben Sie die Verschlüsselungskonfiguration an.

   * Laden Sie für das Szenario mit vom Kunden verschlüsselter VHD die verschlüsselte VHD in Ihr Speicherkonto und das Verschlüsselungsschlüsselmaterial in Ihren Schlüsseltresor. Geben Sie dann die Verschlüsselungskonfiguration zum Aktivieren der Verschlüsselung auf einem neuen virtuellen Computer an.
   * Geben Sie für neue virtuelle Computer, die von unterstützten Katalogimages erstellt wurden, und bereits vorhandene und in Azure ausgeführte virtuelle Computer die Verschlüsselungskonfiguration an, um die Verschlüsselung auf dem virtuellen Computer zu aktivieren.

1. Gewähren Sie Zugriff auf die Azure-Plattform, um das Verschlüsselungsschlüsselmaterial (BitLocker-Verschlüsselungsschlüssel für Windows-Systeme und Passphrase für Linux) aus Ihrem Schlüsseltresor auszulesen und so die Verschlüsselung auf dem virtuellen Computer zu aktivieren.

1. Azure aktiviert das VM-Dienstmodell mit der Konfiguration für die Verschlüsselung und den Schlüsseltresor und richtet Ihren verschlüsselten virtuellen Computer ein.

   ![Microsoft-Antischadsoftware in Azure](./media/azure-security-disk-encryption/disk-encryption-fig1.png)

## <a name="decryption-workflow"></a>Entschlüsselungsworkflow
Führen Sie die folgenden allgemeinen Schritte aus, um die Datenträgerverschlüsselung für virtuelle Computer zu deaktivieren:

1. Deaktivieren Sie die Verschlüsselung (Entschlüsselung) auf einem in Azure ausgeführten virtuellen Computer, und geben Sie die Entschlüsselungskonfiguration an. Sie können die Deaktivierung über die Azure Disk Encryption Resource Manager-Vorlage, PowerShell-Cmdlets oder Azure CLI vornehmen.

   Dieser Schritt deaktiviert die Verschlüsselung des Betriebssystems und/oder des Datenvolumens auf dem ausgeführten virtuellen Windows-Computer. Wie im vorherigen Abschnitt erwähnt, wird die Deaktivierung der Verschlüsselung des Betriebssystemdatenträgers für Linux nicht unterstützt. Der Entschlüsselungsschritt ist nur für Datenlaufwerke auf virtuellen Linux-Computern zulässig, wenn der Betriebssystemdatenträger nicht verschlüsselt ist.

1. Azure aktualisiert das Dienstmodell des virtuellen Computers, und der virtuelle Computer wird als entschlüsselt markiert. Der Inhalt des virtuellen Computers wird im Ruhezustand nicht mehr verschlüsselt.

   > [!NOTE]
   > Durch den Vorgang zum Deaktivieren der Verschlüsselung werden Ihr Schlüsseltresor und das Verschlüsselungsschlüsselmaterial (BitLocker-Verschlüsselungsschlüssel für Windows-Systeme oder Passphrase für Linux) nicht gelöscht.
   >
   > Das Deaktivieren der Betriebssystem-Datenträgerverschlüsselung für Linux wird nicht unterstützt. Der Entschlüsselungsschritt ist nur für Datenlaufwerke auf Linux-VMs zulässig.
   >
   > Das Deaktivieren der Datenträgerverschlüsselung für Linux wird nicht unterstützt, wenn das Betriebssystemlaufwerk verschlüsselt ist.


## <a name="encryption-workflow-previous-release"></a>Verschlüsselungsworkflow (Vorgängerversion)

Beim neuen Release von Azure Disk Encryption muss kein Azure Active Directory-Anwendungsparameter (Azure AD) mehr angegeben werden, um die VM-Datenträgerverschlüsselung zu aktivieren. Bei der neuen Version müssen Sie während des Schritts zum Aktivieren der Verschlüsselung keine Azure AD-Anmeldeinformationen mehr angeben. Alle neuen virtuellen Computer müssen ohne die Azure AD-Anwendungsparameter verschlüsselt werden, wenn Sie die neue Version verwenden. VMs, die bereits mit Azure AD-Anwendungsparametern verschlüsselt wurden, werden weiterhin unterstützt und sollten weiterhin mit der Azure AD-Syntax gepflegt werden. Führen Sie folgende Schritte aus, um die Datenträgerverschlüsselung für virtuelle Windows- und Linux-Computer (Vorgängerversion) zu aktivieren:

1. Wählen Sie ein Verschlüsselungsszenario aus den im Abschnitt [Verschlüsselungsszenarien](#encryption-scenarios) aufgeführten Szenarien aus.

1. Aktivieren Sie die Datenträgerverschlüsselung über eine Azure Disk Encryption Resource Manager-Vorlage, PowerShell-Cmdlets oder Azure CLI, und geben Sie die Verschlüsselungskonfiguration an.

   * Laden Sie für das Szenario mit vom Kunden verschlüsselter VHD die verschlüsselte VHD in Ihr Speicherkonto und das Verschlüsselungsschlüsselmaterial in Ihren Schlüsseltresor. Geben Sie dann die Verschlüsselungskonfiguration zum Aktivieren der Verschlüsselung auf einem neuen virtuellen Computer an.
   * Geben Sie die Verschlüsselungskonfiguration für die über den Marketplace neu erstellten und für die vorhandenen, bereits in Azure ausgeführten, virtuellen Computer an, um die Verschlüsselung auf dem virtuellen Computer zu aktivieren.

1. Gewähren Sie Zugriff auf die Azure-Plattform, um das Verschlüsselungsschlüsselmaterial (BitLocker-Verschlüsselungsschlüssel für Windows-Systeme und Passphrase für Linux) aus Ihrem Schlüsseltresor auszulesen und so die Verschlüsselung auf dem virtuellen Computer zu aktivieren.

1. Geben Sie die Identität der Azure AD-Anwendung an, um das Verschlüsselungsschlüsselmaterial in Ihren Schlüsseltresor zu schreiben. Mit diesem Schritt wird die Verschlüsselung auf dem virtuellen Computer für die in Schritt 2 erwähnten Szenarios aktiviert.

1. Azure aktiviert das VM-Dienstmodell mit der Konfiguration für die Verschlüsselung und den Schlüsseltresor und richtet Ihren verschlüsselten virtuellen Computer ein.


## <a name="terminology"></a>Begriff
In der folgenden Tabelle werden einige der häufig in der Azure Disk Encryption-Dokumentation verwendeten Begriffe beschrieben:

| Begriff | Definition |
| --- | --- |
| Azure AD | Ein [Azure AD](https://azure.microsoft.com/documentation/services/active-directory/)-Konto wird zum Authentifizieren, Speichern und Abrufen von Geheimnissen aus einem Schlüsseltresor verwendet. |
| Azure Key Vault | Key Vault ist ein Dienst zum Verwalten kryptografischer Schlüssel, der auf Hardwaresicherheitsmodulen mit FIPS-Überprüfung (Federal Information Processing Standards) basiert. Diese Standards tragen dazu bei, Ihre kryptografischen Schlüssel und vertraulichen Geheimnisse zu schützen. Weitere Informationen finden Sie in der [Azure Key Vault](https://azure.microsoft.com/services/key-vault/)-Dokumentation. |
| BitLocker |Bei [BitLocker](https://technet.microsoft.com/library/hh831713.aspx) handelt es sich um eine branchenübliche Verschlüsselungstechnologie für Windows-Volumes, die zum Aktivieren der Datenträgerverschlüsselung auf virtuellen Windows-Computern verwendet wird. |
| BEK | BitLocker-Verschlüsselungsschlüssel (BitLocker Encryption Keys, BEK) werden verwendet, um das Startvolume des Betriebssystems und Datenvolumes zu verschlüsseln. BEKs sind in einem Schlüsseltresor als Geheimnisse geschützt. |
| Azure-Befehlszeilenschnittstelle | [Azure CLI](/cli/azure/install-azure-cli) ist für die Verwaltung von Azure-Ressourcen über die Befehlszeile optimiert.|
| DM-Crypt |[DM-Crypt](https://gitlab.com/cryptsetup/cryptsetup/wikis/DMCrypt) ist das Linux-basierte transparente Subsystem für die Datenträgerverschlüsselung, das zum Aktivieren der Datenträgerverschlüsselung auf virtuellen Linux-Computern verwendet wird. |
| Schlüsselverschlüsselungsschlüssel (Key encryption key, KEK) | Der asymmetrische Schlüssel (RSA 2048), der zum Schützen oder Umschließen des Geheimnisses verwendet wird. Sie können einen mit Hardwaresicherheitsmodulen geschützten Schlüssel oder einen Schlüssel mit Softwareschutz bereitstellen. Weitere Informationen finden Sie in der [Azure Key Vault](https://azure.microsoft.com/services/key-vault/)-Dokumentation. |
| PowerShell-Cmdlets | Weitere Informationen finden Sie unter [Azure PowerShell-Cmdlets](/powershell/azure/overview). |

## <a name="next-steps"></a>Nächste Schritte

Informationen zu den ersten Schritten finden Sie in den [Azure Disk Encryption prerequisites (Voraussetzungen für Azure Disk Encryption)](azure-security-disk-encryption-prerequisites.md).

