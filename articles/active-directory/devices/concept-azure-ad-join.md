---
title: Was ist ein in Azure AD eingebundenes Gerät?
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
ms.openlocfilehash: 4af3aea7218ea8792bb66188e8df7baf9f460b0b
ms.sourcegitcommit: aa66898338a8f8c2eb7c952a8629e6d5c99d1468
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2019
ms.locfileid: "67462546"
---
# <a name="azure-ad-joined-devices"></a>In Azure AD eingebundene Geräte

Die Einbindung in Azure AD ist für Organisationen bestimmt, die auf eine Cloud-First- oder Cloud-Only-Strategie setzen. Jede Organisation kann unabhängig von ihrer Größe oder Branche in Azure AD eingebundene Geräte bereitstellen. Die Einbindung in Azure AD funktioniert sogar in einer Hybridumgebung und ermöglicht den Zugriff auf cloudbasierte und lokale Anwendungen und Ressourcen.

|   | Azure AD Join |
| --- | --- |
| **Definition** | Bei der ausschließlichen Einbindung in Azure AD ist für die Anmeldung bei dem Gerät ein Organisationskonto erforderlich. |
| **Hauptzielgruppe** | Geeignet für Cloud-Only- sowie Hybridorganisationen |
|   | Anwendbar für alle Benutzer in einer Organisation |
| **Gerätebesitz** | Organisation |
| **Betriebssysteme** | Alle Windows 10-Geräte |
| **Bereitstellung** | Self-Service: Windows-Willkommensseite oder Windows-Einstellungen |
|   | Massenregistrierung |
|   | Windows Autopilot |
| **Anmeldeoptionen für Geräte** | Organisationskonten mit: |
|   | Kennwort |
|   | Windows Hello for Business |
|   | FIDO2.0-Sicherheitsschlüssel (Vorschauversion) |
| **Geräteverwaltung** | Mobile Geräteverwaltung (z. B. Microsoft Intune) |
|   | Kombinierte Verwaltung mit Microsoft Intune und System Center Configuration Manager |
| **Wichtige Funktionen** | Einmaliges Anmelden für cloudbasierte sowie lokale Ressourcen |
|   | Bedingter Zugriff über MDM-Registrierung und MDM-Kompatibilitätsauswertung |
|   | Self-Service-Kennwortzurücksetzung und Windows Hello-PIN-Zurücksetzung auf dem Sperrbildschirm |
|   | Geräteübergreifendes Enterprise State Roaming |

Die Anmeldung bei in Azure AD eingebundenen Geräten erfolgt über ein Azure AD-Organisationskonto. Der Zugriff auf Ressourcen in der Organisation kann basierend auf diesem Azure AD-Konto und auf [Richtlinien für den bedingten Zugriff](../conditional-access/overview.md), die auf die Geräteidentität angewandt werden, weiter eingeschränkt werden.

Administratoren können in Azure AD eingebundene Geräte mithilfe von MDM-Tools (mobile Geräteverwaltung) wie Microsoft Intune oder in Szenarien für die Co-Verwaltung mithilfe von System Center Configuration Manager schützen und weiter steuern. Diese Tools bieten eine Möglichkeit zur Erzwingung der von einer Organisation geforderten Konfigurationen, z. B. Verschlüsselung des Speichers, Kennwortkomplexität, Softwareinstallationen und Softwareupdates. Administratoren können Organisationsanwendungen unter Verwendung von [System Center Configuration Manager und dem Microsoft Store für Unternehmen](https://docs.microsoft.com/sccm/apps/deploy-use/manage-apps-from-the-windows-store-for-business) für in Azure AD eingebundene Geräte zur Verfügung stellen.

Die Einbindung in Azure AD kann über Self-Service-Optionen wie z. B. Windows-Willkommensseite, Massenregistrierung oder[Windows Autopilot](https://docs.microsoft.com/intune/enrollment-autopilot) durchgeführt werden.

Bei in Azure AD eingebundenen Geräten kann weiterhin der Zugriff über einmaliges Anmelden für lokale Ressourcen beibehalten werden, wenn diese sich im Netzwerk der Organisation befinden. Geräte, die in Azure AD eingebunden sind, können weiterhin bei lokalen Servern authentifiziert werden, z. B. für Datei-, Druck- und anderen Anwendungen.

## <a name="scenarios"></a>Szenarien

Azure AD Join ist zwar hauptsächlich für Unternehmen vorgesehen, die über keine lokale Windows Server Active Directory-Infrastruktur verfügen, kann aber auch in folgenden Szenarios verwendet werden:

- Sie möchten mit Azure AD und einer Lösung für die Verwaltung mobiler Geräte wie Intune zu einer cloudbasierten Infrastruktur wechseln.
- Sie können keinen lokalen Domänenbeitritt verwenden, z.B. wenn Sie mobile Geräte wie Tablets und Telefone steuern möchten.
- Benutzer müssen hauptsächlich auf Office 365 oder andere in Azure AD integrierte SaaS-Apps zugreifen.
- Sie möchten eine Gruppe von Benutzern in Azure AD anstatt in Active Directory verwalten. Dieses Szenario kann beispielsweise für Saisonkräfte, Auftragnehmer oder Kursteilnehmer gelten.
- Sie möchten Beitrittsfunktionen für Mitarbeiter in entfernten Niederlassungen mit eingeschränkter lokaler Infrastruktur bereitstellen.

Sie können in Azure AD eingebundene Geräte für Windows 10-Geräte konfigurieren.

Ziel von in Azure AD eingebundenen Geräten ist die Vereinfachung folgender Elemente:

- Windows-Bereitstellungen unternehmenseigener Geräte
- Zugriff auf Unternehmens-Apps und Unternehmensressourcen über jegliche Windows-Geräte
- Cloudbasierte Verwaltung von unternehmenseigenen Geräten
- Anmeldung von Benutzern bei Geräten mit ihrem Azure AD- oder synchronisierten Geschäfts-, Schul- oder Unikonten für Active Directory

![In Azure AD eingebundene Geräte](./media/concept-azure-ad-join/azure-ad-joined-device.png)

Azure AD Join kann mit einer der folgenden Methoden bereitgestellt werden:

- [Windows Autopilot](https://docs.microsoft.com/windows/deployment/windows-autopilot/windows-10-autopilot)
- [Massenbereitstellung](https://docs.microsoft.com/intune/windows-bulk-enroll)
- [Self-service-Erfahrung](azuread-joined-devices-frx.md)

## <a name="next-steps"></a>Nächste Schritte

- [Planen der Implementierung Ihrer Azure AD-Einbindung](azureadjoin-plan.md)
- [Verwalten der lokalen Administratorgruppe auf in Azure AD eingebundenen Geräten](assign-local-admin.md)
- [Verwalten der Geräteidentität mithilfe des Azure-Portals](device-management-azure-portal.md)
- [Verwalten von veralteten Geräten in Azure AD](manage-stale-devices.md)
