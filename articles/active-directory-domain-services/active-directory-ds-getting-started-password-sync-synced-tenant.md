---
title: 'Azure AD Domain Services: Aktivieren der Kennwortsynchronisierung | Microsoft-Dokumentation'
description: Erste Schritte mit Azure Active Directory Domain Services
services: active-directory-ds
documentationcenter: ''
author: iainfoulds
manager: daveba
editor: curtand
ms.assetid: 8731f2b2-661c-4f3d-adba-2c9e06344537
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/10/2019
ms.author: iainfou
ms.openlocfilehash: 0b38bdba0d88da9296106411737c280dcf6d5df1
ms.sourcegitcommit: e42c778d38fd623f2ff8850bb6b1718cdb37309f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/19/2019
ms.locfileid: "69613169"
---
# <a name="enable-password-hash-synchronization-to-azure-active-directory-domain-services"></a>Aktivieren der Kennworthashsynchronisierung für Azure Active Directory Domain Services

Ein synchronisierter Azure AD-Mandant wird für die Synchronisierung mit dem lokalen Verzeichnis Ihres Unternehmens per Azure AD Connect festgelegt. Standardmäßig synchronisiert Azure AD Connect keine NTLM- und Kerberos-Anmeldeinformationshashes mit Azure AD. Zum Verwenden der Azure AD-Domänendienste müssen Sie Azure AD Connect so konfigurieren, dass Hashes von Anmeldeinformationen, die für die NTLM- und Kerberos-Authentifizierung benötigt werden, synchronisiert werden. Die folgenden Schritte ermöglichen die Synchronisierung der erforderlichen Anmeldeinformationshashes aus Ihrem lokalen Verzeichnis mit Ihrem Azure AD-Mandanten.

> [!NOTE]
> **Falls Ihre Organisation über Benutzerkonten verfügt, die von Ihrem lokalen Verzeichnis aus synchronisiert werden, müssen Sie die Synchronisierung von NTLM- und Kerberos-Hashes aktivieren, um die verwaltete Domäne verwenden zu können.** Ein synchronisiertes Benutzerkonto ist ein Konto, das in Ihrem lokalen Verzeichnis erstellt wurde und unter Verwendung von Azure AD Connect mit Ihrem Azure AD-Mandanten synchronisiert wird. Die Kennworthashsynchronisierung ist ein anderer Prozess als die Benutzer-/Objektsynchronisierung. Sie müssen die Kennworthashsynchronisierung deaktivieren/aktivieren, um eine vollständige Kennwortsynchronisierung zu ermöglichen. Dann werden die Kennworthash-Batchaktualisierungen im Anwendungsereignisprotokoll angezeigt.

In diesem Artikel ermöglichen Sie die Synchronisierung der Anmeldeinformationshashes, die für die NTLM- (NT LAN Manager) und Kerberos-Authentifizierung erforderlich sind, mit Azure AD Domain Services. Nach der Einrichtung der Synchronisierung von Anmeldeinformationen können sich Benutzer mit ihren Unternehmensanmeldeinformationen bei der verwalteten Domäne anmelden.

Für reine Cloudbenutzerkonten müssen andere Schritte ausgeführt werden als für Benutzerkonten, die aus Ihrem lokalen Verzeichnis mit Azure AD Connect synchronisiert werden.

| **Art des Benutzerkontos** | **Auszuführende Schritte** |
| --- | --- |
| **Synchronisierte Benutzerkonten aus einem lokalen Verzeichnis** |**&#x2713;** Befolgen Sie die Anweisungen in [diesem Artikel](active-directory-ds-getting-started-password-sync-synced-tenant.md#install-or-update-azure-ad-connect). |
| **In Azure AD erstellte Cloud-Benutzerkonten** |**&#x2713;** [Synchronisieren Sie Kennwörter reiner Cloudbenutzerkonten mit Ihrer verwalteten Domäne.](tutorial-create-instance.md#enable-user-accounts-for-azure-ad-ds) |

> [!TIP]
> **Unter Umständen müssen beide Prozeduren durchgeführt werden.**
> Wenn Ihr Azure AD-Mandant über eine Kombination aus reinen Cloudbenutzern und Benutzern aus Ihrem lokalen AD verfügt, müssen beide Prozeduren durchgeführt werden.

## <a name="install-or-update-azure-ad-connect"></a>Installieren oder Aktualisieren von Azure AD Connect

Installieren Sie die neueste empfohlene Version von Azure AD Connect auf einem in die Domäne eingebundenen Computer. Wenn Sie über eine vorhandene Instanz von Azure AD Connect-Setup verfügen, müssen Sie sie aktualisieren, damit die aktuelle Version von Azure AD Connect verwendet wird. Verwenden Sie immer die neueste Version von Azure AD Connect, um bekannte Probleme oder Fehler zu vermeiden, die ggf. bereits behoben wurden.

**[Azure AD Connect herunterladen](https://www.microsoft.com/download/details.aspx?id=47594)**

Empfohlene Version: **1.1.614.0**, veröffentlicht am 5. September 2017.

> [!WARNING]
> Sie müssen die neueste empfohlene Version von Azure AD Connect installieren, um ältere Anmeldeinformationen zu aktivieren (erforderlich für NTLM- und Kerberos-Authentifizierung) und Ihren Azure AD-Mandanten zu synchronisieren. Diese Funktionalität ist in früheren Versionen von Azure AD Connect oder im DirSync-Legacytool nicht verfügbar.

Installationshinweise für Azure AD Connect finden Sie im folgenden Artikel: [Erste Schritte mit Azure AD Connect](../active-directory/hybrid/whatis-hybrid-identity.md)

## <a name="enable-synchronization-of-ntlm-and-kerberos-credential-hashes-to-azure-ad"></a>Aktivieren der Synchronisierung von Hashes für NTLM- und Kerberos-Anmeldeinformationen mit Azure AD

Führen Sie für jede Active Directory-Gesamtstruktur das folgende PowerShell-Skript aus. Dadurch können die NTLM- und Kerberos-Kennworthashes aller lokalen Benutzer mit Ihrem Azure AD-Mandanten synchronisiert werden. Darüber hinaus initiiert das Skript eine vollständige Synchronisierung in Azure AD Connect.

```powershell
$adConnector = "<CASE SENSITIVE AD CONNECTOR NAME>"
$azureadConnector = "<CASE SENSITIVE AZURE AD CONNECTOR NAME>"
Import-Module adsync
$c = Get-ADSyncConnector -Name $adConnector
$p = New-Object Microsoft.IdentityManagement.PowerShell.ObjectModel.ConfigurationParameter "Microsoft.Synchronize.ForceFullPasswordSync", String, ConnectorGlobal, $null, $null, $null
$p.Value = 1
$c.GlobalParameters.Remove($p.Name)
$c.GlobalParameters.Add($p)
$c = Add-ADSyncConnector -Connector $c
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $azureadConnector -Enable $false
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $azureadConnector -Enable $true
```

Je nach Größe Ihres Verzeichnisses (Anzahl von Benutzern, Gruppen usw.) nimmt die Synchronisierung der Hashes von Anmeldeinformationen mit Azure AD mehr oder weniger Zeit in Anspruch. Die Kennwörter können kurz nach der Synchronisierung der Hashes für die Anmeldeinformationen mit Azure AD in der verwalteten Domäne der Azure AD-Domänendienste verwendet werden.

## <a name="next-steps"></a>Nächste Schritte

> [!NOTE]
> Die **Kennworthashsynchronisierung** ist ein anderer Prozess als die Benutzer-/Objektsynchronisierung. Sie müssen die Kennworthashsynchronisierung deaktivieren/aktivieren, um eine vollständige Kennwortsynchronisierung zu ermöglichen. Dann werden die Kennworthash-Batchaktualisierungen im Anwendungsereignisprotokoll angezeigt.

* [Aktivieren der Synchronisierung von Kennwörtern für AAD-Domänendienste bei einem reinen Cloud-Azure AD-Verzeichnis](tutorial-create-instance.md#enable-user-accounts-for-azure-ad-ds)
* [Verwalten einer Azure AD Domain Services-Domäne](tutorial-create-management-vm.md)
* [Einbinden eines virtuellen Windows-Computers in eine verwaltete Domäne der Azure AD-Domänendienste](active-directory-ds-admin-guide-join-windows-vm.md)
* [Einbinden eines virtuellen Red Hat Enterprise Linux-Computers in eine verwaltete Domäne der Azure AD-Domänendienste](active-directory-ds-admin-guide-join-rhel-linux-vm.md)
