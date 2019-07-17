---
title: Schützen Ihrer verwalteten Azure Active Directory Domain Services-Domäne | Microsoft-Dokumentation
description: Schützen Ihrer verwalteten Domäne
services: active-directory-ds
documentationcenter: ''
author: iainfoulds
manager: daveba
editor: curtand
ms.assetid: 6b4665b5-4324-42ab-82c5-d36c01192c2a
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2019
ms.author: iainfou
ms.openlocfilehash: e94cd9ca049cfdfd2321ce046714506ed1f23390
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/01/2019
ms.locfileid: "67483279"
---
# <a name="secure-your-azure-ad-domain-services-managed-domain"></a>Schützen Ihrer verwalteten Azure AD Domain Services-Domäne
Dieser Artikel unterstützt Sie beim Schützen Ihrer verwalteten Domäne. Sie können die Verwendung unsicherer Verschlüsselungssammlungen und die Synchronisierung von NTLM-Anmeldeinformationshashes deaktivieren.

## <a name="install-the-required-powershell-modules"></a>Installieren der erforderlichen PowerShell-Module

### <a name="install-and-configure-azure-ad-powershell"></a>Installieren und Konfigurieren von Azure AD PowerShell
Folgen Sie den Anweisungen im Artikel zum [Installieren des Azure AD PowerShell-Moduls und Herstellen einer Verbindung mit Azure AD](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?toc=%2fazure%2factive-directory-domain-services%2ftoc.json).

### <a name="install-and-configure-azure-powershell"></a>Installieren und Konfigurieren von Azure PowerShell
Folgen Sie den Anweisungen im Artikel zum [Installieren des Azure PowerShell-Moduls und Herstellen einer Verbindung mit Ihrem Azure-Abonnement](https://docs.microsoft.com/powershell/azure/install-az-ps?toc=%2fazure%2factive-directory-domain-services%2ftoc.json).


## <a name="disable-weak-cipher-suites-and-ntlm-credential-hash-synchronization"></a>Deaktivieren von schwachen Verschlüsselungssammlungen und der Synchronisierung von NTLM-Anmeldeinformationshashes
Verwenden Sie das folgende PowerShell-Skript für folgende Aufgaben:
1. Deaktivieren der NTLM v1-Unterstützung für die verwaltete Domäne
2. Deaktivieren der Synchronisierung von NTLM-Kennworthashes aus Ihrer lokalen AD-Instanz
3. Deaktivieren von TLS v1 für die verwaltete Domäne

```powershell
// Login to your Azure AD tenant
Login-AzAccount

// Retrieve the Azure AD Domain Services resource.
$DomainServicesResource = Get-AzResource -ResourceType "Microsoft.AAD/DomainServices"

// 1. Disable NTLM v1 support on the managed domain.
// 2. Disable the synchronization of NTLM password hashes from
//    on-premises AD to Azure AD and Azure AD Domain Services
// 3. Disable TLS v1 on the managed domain.
$securitySettings = @{"DomainSecuritySettings"=@{"NtlmV1"="Disabled";"SyncNtlmPasswords"="Disabled";"TlsV1"="Disabled"}}

// Apply the settings to the managed domain.
Set-AzResource -Id $DomainServicesResource.ResourceId -Properties $securitySettings -Verbose -Force
```

> [!IMPORTANT]
> Benutzer (und Dienstkonten) können keine einfachen LDAP-Bindungen vornehmen, wenn Sie die Synchronisierung von NTLM-Kennworthashes für Ihre Azure AD Domain Services-Instanz deaktiviert haben.  Weitere Informationen zum Deaktivieren der Synchronisierung von NTLM-Kennworthashes finden Sie unter [Schützen Ihrer verwalteten Azure AD Domain Services-Domäne](secure-your-domain.md).
>
>

## <a name="next-steps"></a>Nächste Schritte
* [Grundlegendes zur Synchronisierung in Azure AD Domain Services](synchronization.md)
