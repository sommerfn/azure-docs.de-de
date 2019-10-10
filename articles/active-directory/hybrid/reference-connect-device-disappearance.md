---
title: Grundlegendes zu Azure AD Connect 1.4.xx.x und zum Verschwinden von Geräten | Microsoft-Dokumentation
description: In diesem Dokument wird ein Problem beschrieben, das bei Version 1.4.xx.x von Azure AD Connect auftritt.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/25/2019
ms.subservice: hybrid
ms.author: billmath
ms.openlocfilehash: b0ef3dd2f39802d07c4ae04ad1eca23e40db502a
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71345500"
---
# <a name="understanding-azure-ad-connect-14xxx-and-device-disappearance"></a>Grundlegendes zu Azure AD Connect 1.4.xx.x und zum Verschwinden von Geräten
Bei der Version 1.4.xx.x von Azure AD Connect stellen Kunden möglicherweise fest, dass einige oder alle ihrer Windows-Geräte in Azure AD nicht mehr angezeigt werden. Dies ist kein Grund zur Besorgnis, da diese Geräteidentitäten bei der Autorisierung mit bedingtem Zugriff in Azure AD nicht verwendet werden. Durch diese Änderung werden jedoch keine Windows-Geräte gelöscht, die ordnungsgemäß bei Azure AD für Azure AD Hybrid Join registriert wurden.

Wenn das Löschen von Geräteobjekten in Azure AD den Schwellenwert für den Löschvorgang beim Export überschreitet, wird dem Kunden empfohlen, diese Löschvorgänge zuzulassen. [Anleitung: Zulassen von Löschvorgängen bei Überschreiten des Schwellenwerts für Löschungen](how-to-connect-sync-feature-prevent-accidental-deletes.md)

## <a name="background"></a>Hintergrund
Windows-Geräte, die mit Azure AD Hybrid Join registriert wurden, werden in Azure AD als Geräteobjekte dargestellt. Diese Geräteobjekte können für den bedingten Zugriff verwendet werden. Windows 10-Geräte werden über Azure AD Connect mit der Cloud synchronisiert. Kompatible Windows-Geräte werden entweder mithilfe von AD FS oder durch nahtloses einmaliges Anmelden direkt registriert.

## <a name="windows-10-devices"></a>Windows 10-Geräte
Nur Windows 10-Geräte mit einem speziellen in Azure AD Hybrid Join konfigurierten Wert für das userCertificate-Attribut sollten über Azure AD Connect mit der Cloud synchronisiert werden. In früheren Versionen von Azure AD Connect wurde diese Anforderung nicht strikt erzwungen, was zu unnötigen Geräteobjekten in Azure AD geführt hat. Solche Geräte haben in Azure AD dauerhaft den Status „Ausstehend“, da diese Geräte nicht für die Registrierung bei Azure AD konzipiert wurden. 

In dieser Version von Azure AD Connect werden nur Windows 10-Geräte synchronisiert, die ordnungsgemäß für Azure AD Hybrid Join konfiguriert sind. Windows 10-Geräteobjekte ohne den speziell in Azure AD Hybrid Join konfigurierten Wert für das userCertificate-Attribut werden aus Azure AD entfernt.

## <a name="down-level-windows-devices"></a>Kompatible Windows-Geräte
In Azure AD Connect sollten niemals [kompatible Windows-Geräte](../devices/hybrid-azuread-join-plan.md#windows-down-level-devices) synchronisiert werden. Alle Geräte in Azure AD, die zuvor falsch synchronisiert wurden, werden nun aus Azure AD gelöscht. Wenn Azure AD Connect versucht, [kompatible Windows-Geräte](../devices/hybrid-azuread-join-plan.md#windows-down-level-devices) zu löschen, entspricht ein solches Gerät nicht mehr dem Gerät, das über den [MSI-Installer für Microsoft Workplace Join für Nicht-Windows 10-Computer](https://www.microsoft.com/download/details.aspx?id=53554) erstellt wurde, sodass auf dem Gerät keine anderen Azure AD-Funktionen verwendet werden können.

Einige Kunden sollten sich möglicherweise unter [Anleitung: Planen der Implementierung einer Azure Active Directory-Hybrideinbindung](../devices/hybrid-azuread-join-plan.md) erneut informieren und ihre Windows-Geräte ordnungsgemäß registrieren, um sicherzustellen, dass der gerätebasierte bedingte Zugriff auf diesen Geräten in vollem Umfang genutzt werden kann. 

## <a name="how-can-i-verify-which-devices-are-deleted-with-this-update"></a>Wie kann ich überprüfen, welche Geräte mit diesem Update gelöscht werden?

Mit dem folgenden PowerShell-Skript können Sie überprüfen, welche Geräte gelöscht werden: https://gallery.technet.microsoft.com/scriptcenter/Export-Hybrid-Azure-AD-f8e51436

Dieses Skript generiert einen Bericht über die Zertifikate, die in Active Directory-Geräteobjekten gespeichert sind, und zwar speziell über die Zertifikate, die von der Azure AD Hybrid Join-Funktion ausgestellt wurden.
Es überprüft die Zertifikate, die in der userCertificate-Eigenschaft eines Computerobjekts in AD vorhanden sind. Bei jedem noch nicht abgelaufenen Zertifikat wird außerdem überprüft, ob es für die Azure AD Hybrid Join-Funktion ausgestellt wurde (d. h., der Name des Antragstellers entspricht „CN={objectGUID}“).
Bisher hat Azure AD Connect jeden Computer mit mindestens einem gültigen Zertifikat mit Azure AD synchronisiert. Ab Azure AD Connect Version 1.4 kann das Synchronisierungsmodul jedoch Azure AD Hybrid Join-Zertifikate identifizieren und mittels „Cloudfilter“ das Computerobjekt aus dem Synchronisierungsvorgang mit Azure AD herausfiltern, sofern kein gültiges Azure AD Hybrid Join-Zertifikat vorhanden ist.
Azure AD-Geräte, die bereits mit AD synchronisiert wurden, aber über kein gültiges Azure AD Hybrid Join-Zertifikat verfügen, werden vom Synchronisierungsmodul gelöscht (CloudFiltered=TRUE).

## <a name="next-steps"></a>Nächste Schritte
- [Azure AD Connect-Versionsverlauf](reference-connect-version-history.md)
