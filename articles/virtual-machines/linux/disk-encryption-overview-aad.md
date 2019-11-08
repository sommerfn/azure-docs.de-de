---
title: Voraussetzungen für Azure Disk Encryption mit Azure AD-App (Vorgängerversion)
description: Dieser Artikel enthält die Voraussetzungen für die Verwendung von Microsoft Azure Disk Encryption für IaaS-VMs.
author: msmbaldwin
ms.service: security
ms.topic: article
ms.author: mbaldwin
ms.date: 03/15/2019
ms.custom: seodec18
ms.openlocfilehash: 5f3de0f877186daa8f6add7fcd1546f91d6ce3d2
ms.sourcegitcommit: 827248fa609243839aac3ff01ff40200c8c46966
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73748916"
---
# <a name="azure-disk-encryption-with-azure-ad-previous-release"></a>Azure Disk Encryption mit Azure AD (vorherige Version)

**Beim neuen Release von Azure Disk Encryption muss kein Azure AD-Anwendungsparameter mehr angegeben werden, um die VM-Datenträgerverschlüsselung zu aktivieren. Sie müssen beim neuen Release während des Schritts zum Aktivieren der Verschlüsselung keine Azure AD-Anmeldeinformationen mehr angeben. Alle neuen virtuellen Computer müssen mit dem neuen Release und ohne die Azure AD-Anwendungsparameter verschlüsselt werden. Eine Anleitung zum Aktivieren der VM-Datenträgerverschlüsselung mit dem neuen Release finden Sie unter [Azure Disk Encryption für Linux-VMs](disk-encryption-overview.md). Virtuelle Computer, die bereits mit Azure AD-Anwendungsparametern verschlüsselt wurden, werden weiterhin unterstützt und sollten weiterhin mit der AAD-Syntax gepflegt werden.**

Dieser Artikel ergänzt [Azure Disk Encryption für Linux-VMs](disk-encryption-overview.md) um zusätzliche Anforderungen und Voraussetzungen für Azure Disk Encryption mit Azure AD (vorherige Version).

Die Angaben in folgenden Abschnitten bleiben unverändert:

- [Unterstützte VMs und Betriebssysteme](disk-encryption-overview.md#supported-vms-and-operating-systems)
- [Weitere VM-Anforderungen](disk-encryption-overview.md#additional-vm-requirements)


 

## <a name="networking-and-group-policy"></a>Netzwerk und Gruppenrichtlinie

**IaaS-VMs müssen die folgenden Anforderungen an die Netzwerkendpunktkonfiguration erfüllen, damit das Azure Disk Encryption-Feature mit der älteren AAD-Parametersyntax aktiviert werden kann:** 
  - Um ein Token für die Verbindungsherstellung mit Ihrem Schlüsseltresor zu erhalten, muss der virtuelle IaaS-Computer eine Verbindung mit dem Azure Active Directory-Endpunkt \[login.microsoftonline.com\] herstellen können.
  - Um die Verschlüsselungsschlüssel in Ihren Schlüsseltresor schreiben zu können, muss der virtuelle IaaS-Computer eine Verbindung mit dem Schlüsseltresorendpunkt herstellen können.
  - Der virtuelle IaaS-Computer muss eine Verbindung mit dem Azure-Speicherendpunkt herstellen können, an dem das Azure-Erweiterungsrepository gehostet wird, sowie mit einem Azure Storage-Konto, das die VHD-Dateien hostet.
  -  Wenn Ihre Sicherheitsrichtlinie den Zugriff von virtuellen Azure-Computern auf das Internet beschränkt, können Sie den obigen URI auflösen und eine spezielle Regel konfigurieren, um ausgehende Verbindungen mit den IP-Adressen zuzulassen. Weitere Informationen finden Sie unter [Zugreifen auf Azure Key Vault hinter einer Firewall](../../key-vault/key-vault-access-behind-firewall.md).
  - Wenn TLS 1.0 unter Windows explizit deaktiviert wurde und die .NET-Version nicht auf 4.6 oder höher aktualisiert wurde, wird mit der folgenden Registrierungsänderung ADE auf die neuere TLS-Version aktualisiert:
    
        [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319]
        "SystemDefaultTlsVersions"=dword:00000001
        "SchUseStrongCrypto"=dword:00000001

        [HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\.NETFramework\v4.0.30319]
        "SystemDefaultTlsVersions"=dword:00000001
        "SchUseStrongCrypto"=dword:00000001` 
     

**Gruppenrichtlinie:**
 - Die Azure Disk Encryption-Lösung verwendet für virtuelle Windows-IaaS-Computer die externe BitLocker-Schlüsselschutzvorrichtung. Für VMs, die der Domäne beigetreten sind, sollten Sie keine Gruppenrichtlinien nutzen, mit denen TPM-Schutzvorrichtungen durchgesetzt werden. Informationen zur Gruppenrichtlinie „BitLocker ohne kompatibles TPM zulassen“ finden Sie unter [BitLocker Group Policy Reference](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-group-policy-settings#bkmk-unlockpol1) (Referenz zur BitLocker-Gruppenrichtlinie).

-  Die BitLocker-Richtlinie für VMs mit Domänenbeitritt und benutzerdefinierten Gruppenrichtlinien muss die folgende Einstellung enthalten: [Speichern von BitLocker-Wiederherstellungsinformationen durch Benutzer konfigurieren -> 256-Bit-Wiederherstellungsschlüssel zulassen](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-group-policy-settings). Bei Azure Disk Encryption tritt ein Fehler auf, wenn benutzerdefinierte Einstellungen für die Gruppenrichtlinie nicht mit BitLocker kompatibel sind. Auf Computern ohne korrekte Richtlinieneinstellung müssen Sie die neue Richtlinie anwenden und die Aktualisierung der neuen Richtlinie erzwingen (gpupdate.exe /force). Danach ist möglicherweise ein Neustart erforderlich.  

## <a name="encryption-key-storage-requirements"></a>Speicheranforderungen für Verschlüsselungsschlüssel  

Azure Disk Encryption erfordert Azure Key Vault zum Steuern und Verwalten von Verschlüsselungsschlüsseln und Geheimnissen für die Datenträgerverschlüsselung. Ihr Schlüsseltresor und die VMs müssen sich in derselben Azure-Region und im selben Azure-Abonnement befinden.

Weitere Informationen finden Sie unter [Erstellen und Konfigurieren eines Schlüsseltresors für Azure Disk Encryption mit Azure AD (vorherige Version)](disk-encryption-key-vault-aad.md).
 
## <a name="next-steps"></a>Nächste Schritte

- [Erstellen und Konfigurieren eines Schlüsseltresors für Azure Disk Encryption mit Azure AD (vorherige Version)](disk-encryption-key-vault-aad.md)
- [Aktivieren von Azure Disk Encryption mit Azure AD auf Linux-VMs (vorherige Version)](disk-encryption-linux-aad.md)
- [Voraussetzungen für Azure Disk Encryption mit CLI-Skript](https://github.com/ejarvi/ade-cli-getting-started)
- [Voraussetzungen für Azure Disk Encryption mit PowerShell-Skript](https://github.com/Azure/azure-powershell/tree/master/src/Compute/Compute/Extension/AzureDiskEncryption/Scripts)