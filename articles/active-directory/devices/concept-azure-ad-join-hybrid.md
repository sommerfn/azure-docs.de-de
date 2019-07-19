---
title: Was ist ein in Azure AD eingebundenes Hybridgerät?
description: Erfahren Sie, wie Sie mithilfe der Geräteidentitätsverwaltung Geräte verwalten können, die auf Ressourcen in Ihrer Umgebung zugreifen.
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: conceptual
ms.date: 06/27/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sandeo
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5c57180ba10322cb790c05b3f8f48043ca08b545
ms.sourcegitcommit: aa66898338a8f8c2eb7c952a8629e6d5c99d1468
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2019
ms.locfileid: "67462526"
---
# <a name="hybrid-azure-ad-joined-devices"></a>In Azure AD eingebundene Hybridgeräte

Seit über zehn Jahren verwenden viele Unternehmen den Domänenbeitritt für das lokale Active Directory, um Folgendes zu ermöglichen:

- Verwaltung unternehmenseigener Geräte über einen zentralen Ort für IT-Abteilungen
- Anmeldung von Benutzern bei Geräten mit ihrem Geschäfts-, Schul- oder Unikonto in Active Directory

Unternehmen mit einem lokalen Fußabdruck nutzen normalerweise Verfahren für die Imageerstellung, um Geräte bereitzustellen, und häufig **System Center Configuration Manager (SCCM)** oder **Gruppenrichtlinien** für die Verwaltung der Geräte.

Wenn Ihre Umgebung über einen lokalen AD-Fußabdruck verfügt und Sie zudem die Funktionen von Azure Active Directory nutzen möchten, können Sie in Azure AD eingebundene Hybridgeräte implementieren. Hierbei handelt es sich um Geräte, die in Ihre lokale Active Directory-Instanz eingebunden und dafür registriert sind.

|   | Azure AD Hybrid Join |
| --- | --- |
| **Definition** | Eingebunden in lokales AD und Azure AD, und es ist eine Anmeldung bei dem Gerät mit dem Organisationskonto erforderlich. |
| **Hauptzielgruppe** | Geeignet für Hybridorganisationen mit vorhandener lokaler AD-Infrastruktur. |
|   | Anwendbar für alle Benutzer einer Organisation. |
| **Gerätebesitz** | Organisation |
| **Betriebssysteme** | Windows 10, 8.1 und 7 |
|   | Windows Server 2008/R2, 2012/R2, 2016 und 2019 |
| **Bereitstellung** | Windows 10, Windows Server 2016/2019 |
|   | Domänenbeitritt über IT und automatischer Beitritt per Azure AD Connect- oder ADFS-Konfiguration |
|   | Domänenbeitritt per Windows Autopilot und automatischer Beitritt per Azure AD Connect- oder ADFS-Konfiguration |
|   | Windows 8.1, Windows 7, Windows Server 2012 R2, Windows Server 2012 und Windows Server 2008 R2: MSI erforderlich |
| **Anmeldeoptionen für Gerät** | Organisationskonten mit: |
|   | Kennwort |
|   | Windows Hello for Business für Windows 10 |
| **Geräteverwaltung** | Gruppenrichtlinie |
|   | Eigenständige System Center Configuration Manager-Version oder kombinierte Verwaltung mit Microsoft Intune |
| **Wichtige Funktionen** | Einmaliges Anmelden sowohl für cloudbasierte als auch für lokale Ressourcen |
|   | Bedingter Zugriff per Domänenbeitritt oder mit Intune bei kombinierter Verwaltung |
|   | Self-Service-Kennwortzurücksetzung und Windows Hello-PIN-Zurücksetzung auf dem Sperrbildschirm |
|   | Geräteübergreifendes Enterprise State Roaming |

![In Azure AD eingebundene Hybridgeräte](./media/concept-azure-ad-join-hybrid/azure-ad-hybrid-joined-device.png)

## <a name="scenarios"></a>Szenarien

Verwenden Sie in Azure AD eingebundene Hybridgeräte in folgenden Fällen:

- Auf den Geräten sind Win32-Apps bereitgestellt, die die Active Directory-Authentifizierung für Computer benötigen.
- Sie möchten die Gruppenrichtlinie weiterhin nutzen, um die Gerätekonfiguration zu verwalten.
- Sie möchten weiterhin vorhandene Lösungen für die Imageerstellung verwenden, um Geräte bereitzustellen und zu konfigurieren.
- Sie müssen zusätzlich zu Windows 10 Downlevelgeräte mit Windows 7 und 8.1 unterstützen.

## <a name="next-steps"></a>Nächste Schritte

- [Planen der Implementierung Ihrer Azure AD-Hybrideinbindung](hybrid-azuread-join-plan.md)
- [Verwalten der Geräteidentität mithilfe des Azure-Portals](device-management-azure-portal.md)
- [Verwalten von veralteten Geräten in Azure AD](manage-stale-devices.md)
