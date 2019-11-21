---
title: Konfigurieren der Registrierungsrichtlinie für die mehrstufige Authentifizierung in Azure Active Directory Identity Protection
description: Es wird beschrieben, wie Sie die Registrierungsrichtlinie für die mehrstufige Authentifizierung in Azure AD Identity Protection konfigurieren.
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: conceptual
ms.date: 10/18/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: bc98f645c6b24069e090560a049ccb4fcd03dfec
ms.sourcegitcommit: 7efb2a638153c22c93a5053c3c6db8b15d072949
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2019
ms.locfileid: "72886889"
---
# <a name="how-to-configure-the-azure-multi-factor-authentication-registration-policy"></a>Anleitung: Konfigurieren der Registrierungsrichtlinie für die mehrstufige Authentifizierung in Azure

Azure AD Identity Protection unterstützt Sie beim Verwalten des Rollouts der Registrierung für Multi-Factor Authentication (MFA): Es wird eine Richtlinie für bedingten Zugriff zur Anforderung der MFA-Registrierung konfiguriert – unabhängig davon, bei welcher App mit moderner Authentifizierung Sie sich anmelden möchten.

## <a name="what-is-the-azure-multi-factor-authentication-registration-policy"></a>Was ist die Registrierungsrichtlinie für die mehrstufige Authentifizierung in Azure (Azure Multi-Factor Authentication)?

Mit Azure Multi-Factor Authentication können Sie über die Verwendung eines Benutzernamens und Kennworts hinaus Ihre Identität verifizieren. Sie stellt eine zweite Sicherheitsebene für Benutzeranmeldungen dar. Damit Benutzer auf MFA-Aufforderungen reagieren können, müssen sie sich zuerst für die Azure Multi-Factor Authentication registrieren.

Die obligatorische Verwendung von Azure Multi-Factor Authentication für Benutzeranmeldungen empfiehlt sich aus folgenden Gründen:

- Sie bietet eine sichere Authentifizierung mit einer Auswahl von Überprüfungsoptionen.
- Sie spielt eine wichtige Rolle bei der Vorbereitung Ihrer Organisation auf die Eigenwartung bei Risikoerkennungen in Identity Protection.

Weitere Informationen zu Azure Multi-Factor Authentication finden Sie unter [Was ist Azure Multi-Factor Authentication?](../authentication/howto-mfa-getstarted.md)

## <a name="policy-configuration"></a>Richtlinienkonfiguration

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com).
1. Navigieren Sie zu **Azure Active Directory** > **Sicherheit** > **Identity Protection** > **MFA-Registrierungsrichtlinie**.
   1. Unter **Zuweisungen**
      1. **Benutzer**: Wählen Sie **Alle Benutzer** oder **Einzelne Benutzer und Gruppen auswählen** aus, wenn Sie den Rollout einschränken.
         1. Optional können Sie Benutzer aus der Richtlinie ausschließen.
   1. Unter **Steuerelemente**
      1. Vergewissern Sie sich, dass das Kontrollkästchen **Azure MFA-Registrierung anfordern** aktiviert ist, und wählen Sie **Auswählen** aus.
   1. **Richtlinie erzwingen** - **Ein**
   1. **Speichern**

## <a name="user-experience"></a>Benutzererfahrung

Azure Active Directory Identity Protection fordert Ihre Benutzer auf, sich bei der nächsten interaktiven Anmeldung zu registrieren, und gibt ihnen 14 Tage Zeit, um die Registrierung vorzunehmen. Während dieses Zeitraums von 14 Tagen kann die Registrierung umgangen werden, aber am Ende des Zeitraums müssen die Benutzer sich registrieren, um den Anmeldevorgang abschließen zu können.

Eine Übersicht über die zugehörige Benutzeroberfläche finden Sie unter:

- [Anmeldeverfahren von Azure AD Identity Protection](concept-identity-protection-user-experience.md)  

## <a name="next-steps"></a>Nächste Schritte

- [Aktivieren von Anmelde- und Benutzerrisikorichtlinien](howto-identity-protection-configure-risk-policies.md)

- [Aktivieren der Self-Service-Kennwortzurücksetzung in Azure AD](../authentication/howto-sspr-deployment.md)

- [Aktivieren von Azure Multi-Factor Authentication](../authentication/howto-mfa-getstarted.md)
