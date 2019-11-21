---
title: Konfigurieren und Aktivieren von Risikorichtlinien in Azure Active Directory Identity Protection
description: Aktivieren und Konfigurieren von Risikorichtlinien in Azure Active Directory Identity Protection
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
ms.openlocfilehash: 79f919633f6b1912ef07b7ff636eb60fb3d5859f
ms.sourcegitcommit: 7efb2a638153c22c93a5053c3c6db8b15d072949
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2019
ms.locfileid: "72886701"
---
# <a name="how-to-configure-and-enable-risk-policies"></a>Anleitung: Konfigurieren und Aktivieren von Risikorichtlinien

Wie im vorherigen Artikel [Identity Protection-Richtlinien](concept-identity-protection-policies.md) beschrieben, verfügen Sie über zwei Risikorichtlinien, die Sie in Ihrem Verzeichnis aktivieren können. 

- Richtlinie zum Anmelderisiko
- Richtlinie zum Benutzerrisiko

![Sicherheitsübersichtsseite zum Aktivieren der Richtlinien zum Benutzerrisiko und zum Anmelderisiko](./media/howto-identity-protection-configure-risk-policies/identity-protection-security-overview.png)

Beide Richtlinien dienen dazu, die Reaktion auf Risikoerkennungen in Ihrer Umgebung zu automatisieren und Benutzern die Eigenwartung erkannter Risiken zu ermöglichen. 

> [!VIDEO https://www.youtube.com/embed/zEsbbik-BTE]

## <a name="prerequisites"></a>Voraussetzungen 

Wenn Ihre Organisation den Benutzern die Möglichkeit zur Eigenwartung erkannter Risiken geben möchte, müssen Benutzer sowohl für die Self-Service-Kennwortzurücksetzung als auch für Azure Multi-Factor Authentication registriert sein. Für eine optimale Erfahrung wird empfohlen [die Umgebung zur kombinierten Registrierung von Sicherheitsinformationen zu aktivieren](../authentication/howto-registration-mfa-sspr-combined.md). Wenn Sie Benutzern eine Eigenwartung erlauben, erreichen diese schneller wieder einen produktiven Zustand, ohne dass ein Eingriff durch einen Administrator erforderlich ist. Administratoren können diese Ereignisse weiterhin sehen und im Anschluss untersuchen. 

## <a name="choosing-acceptable-risk-levels"></a>Auswählen akzeptabler Risikostufen

Organisationen müssen entscheiden, welche Stufe von Risiken sie zu akzeptieren bereit sind, und dabei zwischen Benutzererfahrung und Sicherheitsstatus abzuwägen. 

Die Empfehlung von Microsoft lautet, den Schwellenwert für die Benutzerrisikorichtlinie auf **Hoch** und für die Anmelderisikorichtlinie auf **Mittel und höher** festzulegen.

Wenn Sie den Schwellenwert **Hoch** auswählen, wird die Richtlinie weniger häufig ausgelöst, und die Auswirkungen für Benutzer verringern sich. Hierbei sind aber Risikoerkennungen der Stufen **Niedrig** und **Mittel** aus der Richtlinie ausgeschlossen. Für einen Angreifer wird das Ausnutzen einer gefährdeten Identität so ggf. nicht blockiert. Wenn Sie für den Schwellenwert **Niedrig** auswählen, erhöht sich die Anzahl der Unterbrechungen für die Benutzer, aber der Sicherheitsstatus wird erhöht.

## <a name="exclusions"></a>Ausschlüsse

Alle Richtlinien ermöglichen das Ausschließen von Benutzern, wie z.B. [Benutzer- oder Administratorkonten für den Notfallzugriff](../users-groups-roles/directory-emergency-access.md). Organisationen können feststellen, dass sie andere Konten basierend auf der Art und Weise, wie die Konten verwendet werden, aus bestimmten Richtlinien ausschließen müssen. Alle Ausschlüsse sollten regelmäßig überprüft werden, um festzustellen, ob Sie weiterhin angewendet werden sollen.

## <a name="enable-policies"></a>Aktivieren von Richtlinien

Führen Sie die folgenden Schritte aus, um die Richtlinien zum Benutzerrisiko und zum Anmelderisiko zu aktivieren.

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com).
1. Navigieren Sie zu **Azure Active Directory** > **Sicherheit** > **Identity Protection** > **Übersicht**.
1. Wählen Sie **Benutzerrisikorichtlinie konfigurieren** aus.
   1. Unter **Zuweisungen**
      1. **Benutzer**: Wählen Sie **Alle Benutzer** oder **Einzelne Benutzer und Gruppen auswählen** aus, wenn Sie den Rollout einschränken.
         1. Optional können Sie Benutzer aus der Richtlinie ausschließen.
      1. **Bedingungen** - **Benutzerrisiko**: Die Empfehlung von Microsoft lautet, diese Option auf **Hoch** festzulegen.
   1. Unter **Steuerelemente**
      1. **Zugriff**: Die Empfehlung von Microsoft lautet, den **Zugriff zuzulassen** und eine **Kennwortänderung anzufordern**.
   1. **Richtlinie erzwingen** - **Ein**
   1. **Speichern**: Diese Aktion kehrt zur Seite **Übersicht** zurück.
1. Wählen Sie **Anmelderisikorichtlinie konfigurieren** aus.
   1. Unter **Zuweisungen**
      1. **Benutzer**: Wählen Sie **Alle Benutzer** oder **Einzelne Benutzer und Gruppen auswählen** aus, wenn Sie den Rollout einschränken.
         1. Optional können Sie Benutzer aus der Richtlinie ausschließen.
      1. **Bedingungen** - **Anmelderisiko**: Die Empfehlung von Microsoft lautet, diese Option auf **Mittel und höher** festzulegen.
   1. Unter **Steuerelemente**
      1. **Zugriff**: Die Empfehlung von Microsoft lautet, den **Zugriff zuzulassen** und eine **mehrstufige Authentifizierung anzufordern**.
   1. **Richtlinie erzwingen** - **Ein**
   1. **Speichern**

## <a name="next-steps"></a>Nächste Schritte

- [Aktivieren der Registrierungsrichtlinie für die mehrstufige Authentifizierung](howto-identity-protection-configure-mfa-policy.md)

- [Was bedeutet Risiko?](concept-identity-protection-risks.md)

- [Untersuchen von Risikoerkennungen](howto-identity-protection-investigate-risk.md)

- [Simulieren von Risikoerkennungen](howto-identity-protection-simulate-risk.md)
