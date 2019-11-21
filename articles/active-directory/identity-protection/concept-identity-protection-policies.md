---
title: Azure Active Directory Identity Protection-Richtlinien
description: Identifizieren der drei Richtlinien, die mit Identity Protection aktiviert sind
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
ms.openlocfilehash: 4a44e32efa3e38cf9c4df7dc00e3189c129db418
ms.sourcegitcommit: 7efb2a638153c22c93a5053c3c6db8b15d072949
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2019
ms.locfileid: "72886841"
---
# <a name="identity-protection-policies"></a>Richtlinien für den Identitätsschutz

Azure Active Directory Identity Protection enthält drei Standardrichtlinien, die Administratoren aktivieren können. Diese Richtlinien bieten eingeschränkte Anpassungen, sind jedoch für die meisten Organisationen anwendbar. Alle Richtlinien ermöglichen das Ausschließen von Benutzern, wie z.B. [Benutzer- oder Administratorkonten für den Notfallzugriff](../users-groups-roles/directory-emergency-access.md).

![Richtlinien für den Identitätsschutz](./media/concept-identity-protection-policies/identity-protection-policies.png)

## <a name="azure-mfa-registration-policy"></a>Azure MFA-Registrierungsrichtlinie

Identity Protection kann Organisationen beim Rollout von Azure Multi-Factor Authentication (MFA) mithilfe einer Richtlinie für den bedingten Zugriff unterstützen, die die Registrierung bei der Anmeldung erfordert. Wenn Sie diese Richtlinie aktivieren, können Sie sicherstellen, dass neue Benutzer in Ihrer Organisation sich am ersten Tag für MFA registrieren. Die mehrstufige Authentifizierung (MFA) ist eine der Methoden zur Eigenwartung für Risikoereignisse in Identity Protection. Eigenwartung ermöglicht Ihren Benutzern, selbst Maßnahmen zu ergreifen, um so das Helpdesk-Telefonaufkommen zu verringern.

Weitere Informationen zu Azure Multi-Factor Authentication finden Sie im Artikel [Funktionsweise: Azure Multi-Factor Authentication](../authentication/concept-mfa-howitworks.md).

## <a name="sign-in-risk-policy"></a>Richtlinie zum Anmelderisiko

Identity Protection analysiert Signale von jeder Anmeldung (sowohl in Echtzeit als auch offline) und berechnet eine Risikobewertung basierend auf der Wahrscheinlichkeit, dass die Anmeldung nicht vom Benutzer ausgeführt wurde. Administratoren können basierend auf diesem Risikobewertungssignal Entscheidungen treffen, um organisatorische Anforderungen zu erzwingen. Administratoren können den Zugriff blockieren, den Zugriff erlauben oder mehrstufige Authentifizierung anfordern, um den Zugriff zu erlauben.

Wenn Risiken erkannt werden, können die Benutzer die mehrstufige Authentifizierung zur Eigenwartung durchführen und das riskante Anmeldeereignis schließen, um unnötigen Aufwand für Administratoren zu vermeiden.

> [!NOTE] 
> Benutzer müssen sich vor Auslösen der Anmelderisikorichtlinie bereits für Azure Multi-Factor Authentication registriert haben.

### <a name="custom-conditional-access-policy"></a>Benutzerdefinierte Richtlinie für bedingten Zugriff

Administratoren können auch eine benutzerdefinierte Richtlinie für den bedingten Zugriff erstellen und das Anmelderisiko als Zuweisungsbedingung einschließen. Weitere Informationen zum bedingten Zugriff finden Sie im Artikel [Was ist bedingter Zugriff?](../conditional-access/overview.md)

![Benutzerdefinierte Anmelderisikorichtlinie für den bedingten Zugriff](./media/concept-identity-protection-policies/identity-protection-custom-sign-in-policy.png)

## <a name="user-risk-policy"></a>Richtlinie zum Benutzerrisiko

Identity Protection kann das erwartete „normale“ Verhalten eines Benutzers berechnen, und Entscheidungen bezüglich des Risikos darauf basieren. Das Benutzerrisiko ist eine Berechnung der Wahrscheinlichkeit, dass eine Identität kompromittiert wurde. Administratoren können basierend auf diesem Risikobewertungssignal Entscheidungen treffen, um organisatorische Anforderungen zu erzwingen. Administratoren können mit [Azure AD Self-Service-Kennwortzurücksetzung](../authentication/howto-sspr-deployment.md) den Zugriff blockieren, den Zugriff erlauben oder den Benutzer auffordern, das Kennwort zu ändern, um Zugriff zu erhalten.

Wenn Risiken erkannt werden, können die Benutzer die Self-Service-Kennwortzurücksetzung zur Eigenwartung durchführen und das Benutzerrisikoereignis schließen, um unnötigen Aufwand für Administratoren zu vermeiden.

> [!NOTE] 
> Benutzer müssen sich vor Auslösen der Anmelderisikorichtlinie bereits für die Self-Service-Kennwortzurücksetzung registriert haben.

## <a name="next-steps"></a>Nächste Schritte

- [Aktivieren der Self-Service-Kennwortzurücksetzung in Azure AD](../authentication/howto-sspr-deployment.md)

- [Aktivieren von Azure Multi-Factor Authentication](../authentication/howto-mfa-getstarted.md)

- [Aktivieren der Registrierungsrichtlinie für die mehrstufige Authentifizierung](howto-identity-protection-configure-mfa-policy.md)

- [Aktivieren von Anmelde- und Benutzerrisikorichtlinien](howto-identity-protection-configure-risk-policies.md)
