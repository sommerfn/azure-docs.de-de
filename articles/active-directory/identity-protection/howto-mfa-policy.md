---
title: Konfigurieren der Registrierungsrichtlinie für die mehrstufige Authentifizierung in Azure Active Directory Identity Protection | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie die Registrierungsrichtlinie für die mehrstufige Authentifizierung in Azure AD Identity Protection konfigurieren.
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: article
ms.date: 05/01/2019
author: MicrosoftGuyJFlo
manager: daveba
ms.author: joflore
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 434d07163713a139b42a5dbe1664f81dafc2a1ca
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "67108939"
---
# <a name="how-to-configure-the-azure-multi-factor-authentication-registration-policy"></a>Anleitung: Konfigurieren der Registrierungsrichtlinie für die mehrstufige Authentifizierung in Azure

Azure AD Identity Protection unterstützt Sie beim Verwalten des Rollouts der Registrierung für die mehrstufige Authentifizierung (Multi-Factor Authentication, MFA), indem eine Richtlinie für bedingten Zugriff zur Anforderung der MFA-Registrierung konfiguriert wird – und dies unabhängig von der App, bei der Sie sich anmelden möchten. In diesem Artikel wird beschrieben, wofür die Richtlinie verwendet werden kann und wie sie konfiguriert wird.

## <a name="what-is-the-azure-multi-factor-authentication-registration-policy"></a>Was ist die Registrierungsrichtlinie für die mehrstufige Authentifizierung in Azure (Azure Multi-Factor Authentication)?

Mit Azure Multi-Factor Authentication können Sie über die Verwendung eines Benutzernamens und Kennworts hinaus Ihre Identität verifizieren. Sie stellt eine zweite Sicherheitsebene für Benutzeranmeldungen dar. Damit Benutzer auf MFA-Aufforderungen reagieren können, müssen sie sich zuerst für die Azure Multi-Factor Authentication registrieren.

Die obligatorische Verwendung von Azure Multi-Factor Authentication für Benutzeranmeldungen empfiehlt sich aus folgenden Gründen:

- Sie bietet eine leistungsfähige Authentifizierung mit einer Auswahl von einfachen Überprüfungsoptionen.
- Sie spielt eine wichtige Rolle bei der Vorbereitung Ihres Unternehmens zum Schutz und der Wiederherstellung von Risikoereignissen in Identity Protection.

Weitere Informationen zu MFA finden Sie unter [Was ist Azure Multi-Factor Authentication?](../authentication/howto-mfa-getstarted.md)

## <a name="how-do-i-access-the-registration-policy"></a>Wie greife ich auf die Registrierungsrichtlinie zu?

Die Registrierungsrichtlinie für die mehrstufige Authentifizierung befindet sich auf der [Azure AD Identity Protection-Seite](https://portal.azure.com/#blade/Microsoft_AAD_ProtectionCenter/IdentitySecurityDashboardMenuBlade/SignInPolicy) im Abschnitt **Konfigurieren**.

![MFA-Richtlinie](./media/howto-mfa-policy/1014.png)

## <a name="policy-settings"></a>Richtlinieneinstellungen

Nehmen Sie beim Konfigurieren der MFA-Registrierungsrichtlinie die folgenden Konfigurationsänderungen vor:

- Legen Sie die Benutzer und Gruppen fest, für die die Richtlinie gelten soll. Schließen Sie dabei auf jeden Fall die [Konten für den Notfallzugriff](../users-groups-roles/directory-emergency-access.md) Ihrer Organisation aus.

    ![Benutzer und Gruppen](./media/howto-mfa-policy/11.png)

- Aktivieren Sie die Steuerung, die Sie erzwingen möchten: **Azure MFA-Registrierung anfordern**.

    ![Access](./media/howto-mfa-policy/12.png)

- Legen Sie die Option zum Erzwingen der Richtlinie auf **Ein** fest.

    ![Erzwingen der Richtlinie](./media/howto-mfa-policy/14.png)

- **Speichern** Sie die Richtlinie.

## <a name="user-experience"></a>Benutzererfahrung

Eine Übersicht über die zugehörige Benutzeroberfläche finden Sie unter:

- [Registrierung für Multi-Factor Authentication](flows.md#multi-factor-authentication-registration)  
- [Anmeldeverfahren von Azure AD Identity Protection](flows.md)  

## <a name="next-steps"></a>Nächste Schritte

Eine Übersicht über Azure AD Identity Protection finden Sie [in diesem Artikel](overview.md).
