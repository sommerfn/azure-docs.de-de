---
title: Was sind bei Azure AD registrierte Geräte?
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
ms.openlocfilehash: 7e2a8cad7cd4410a95a6ebd60ada22de456737bf
ms.sourcegitcommit: aa66898338a8f8c2eb7c952a8629e6d5c99d1468
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2019
ms.locfileid: "67462530"
---
# <a name="azure-ad-registered-devices"></a>Bei Azure AD registrierte Geräte

Ziel von bei Azure AD registrierten Geräten ist die Bereitstellung der Unterstützung für das BYOD-Szenario (Bring Your Own Device) oder von mobilen Geräten für Ihre Benutzer. In diesen Szenarien kann ein Benutzer mit einem persönlichen Gerät auf die über Azure Active Directory gesteuerten Ressourcen Ihres Unternehmens zugreifen.

|   | Bei Azure AD registriert |
| --- | --- |
| **Definition** | Registrierung bei Azure AD, ohne dass ein Organisationskonto für die Anmeldung am Gerät verwendet werden muss |
| **Hauptzielgruppe** | Gilt für alle Benutzer mit den folgenden Kriterien: |
|   | Bring Your Own Device (BYOD) |
|   | Mobile Geräte |
| **Gerätebesitz** | Benutzer oder Organisation |
| **Betriebssysteme** | Windows 10, iOS, Android und macOS |
| **Bereitstellung** | Windows 10: Einstellungen |
|   | iOS/Android: Unternehmensportal oder Microsoft Authenticator-App |
|   | macOS: Unternehmensportal |
| **Anmeldeoptionen für Gerät** | Lokale Anmeldeinformationen von Endbenutzern |
|   | Kennwort |
|   | Windows Hello |
|   | PIN |
|   | Biometrische Daten oder Muster für andere Geräte |
| **Geräteverwaltung** | Mobile Geräteverwaltung (z. B. Microsoft Intune) |
|   | Verwaltung mobiler Anwendungen |
| **Wichtige Funktionen** | SSO für Cloudressourcen |
|   | Bedingter Zugriff bei der Registrierung in Intune |
|   | Bedingter Zugriff über App-Schutzrichtlinie |
|   | Ermöglichung der telefonischen Anmeldung per Microsoft Authenticator-App |

![Bei Azure AD registrierte Geräte](./media/concept-azure-ad-register/azure-ad-registered-device.png)

Bei Azure AD registrierte Geräte werden angemeldet, um ein lokales Konto zu verwenden, z. B. ein Microsoft-Konto auf einem Windows 10-Gerät. Zusätzlich verfügen sie aber noch über ein angefügtes Azure AD-Konto für den Zugriff auf Organisationsressourcen. Der Zugriff auf Ressourcen in der Organisation kann basierend auf diesem Azure AD-Konto und auf Richtlinien für den bedingten Zugriff, die auf die Geräteidentität angewendet werden, weiter eingeschränkt werden.

Administratoren können diese bei Azure AD registrierten Geräte schützen und genauer steuern, indem sie MDM-Tools (Mobile Device Management, Mobile Geräteverwaltung) verwenden, z. B. Microsoft Intune. MDM ist eine Möglichkeit zur Erzwingung der von einer Organisation geforderten Konfigurationen, z. B. Verschlüsselung des Speichers, Kennwortkomplexität und aktuelle Sicherheitssoftware. 

Die Azure AD-Registrierung kann durchgeführt werden, wenn zum ersten Mal auf eine Arbeitsanwendung zugegriffen wird, oder manuell über das Menü mit den Windows 10-Einstellungen. 

## <a name="scenarios"></a>Szenarien

Ein Benutzer Ihrer Organisation möchte von seinem Heim-PC aus auf Tools für E-Mail, das Melden von Abwesenheit und die Registrierung für Boni zugreifen. In Ihrer Organisation sind diese Tools hinter einer Richtlinie für bedingten Zugriff angeordnet, sodass der Zugriff über ein Intune-konformes Gerät durchgeführt werden muss. Der Benutzer fügt sein Organisationskonto hinzu und registriert seinen Heim-PC bei Azure AD. Die erforderlichen Intune-Richtlinien werden dann erzwungen, und dem Benutzer wird Zugriff auf seine Ressourcen gewährt.

Ein anderer Benutzer möchte auf sein E-Mail-Konto der Organisation über sein eigenes Android-Smartphone zugreifen, das manipuliert wurde. Für Ihr Unternehmen gilt die Regel, dass ein konformes Gerät verwendet werden muss, und es wurde eine Intune-Konformitätsrichtlinie erstellt, um manipulierte Geräte zu blockieren. Es wird verhindert, dass der Mitarbeiter mit diesem Gerät auf Ressourcen der Organisation zugreift.

## <a name="next-steps"></a>Nächste Schritte

- [Verwalten der Geräteidentität mithilfe des Azure-Portals](device-management-azure-portal.md)
- [Verwalten von veralteten Geräten in Azure AD](manage-stale-devices.md)
