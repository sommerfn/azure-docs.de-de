---
title: 'Bedingter Zugriff: Risikobasierter bedingter Zugriff – Azure Active Directory'
description: Erstellen von Richtlinien für bedingten Zugriff zum Aktivieren von Identity Protection-Verbesserungen für Richtlinien
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 10/23/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: 102371f8af45a1a51715dbfb11afc3f0f4e457d7
ms.sourcegitcommit: 0b1a4101d575e28af0f0d161852b57d82c9b2a7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73150700"
---
# <a name="conditional-access-risk-based-conditional-access"></a>Bedingter Zugriff: Risikobasierter bedingter Zugriff

Organisationen mit Azure AD Premium P2-Lizenzen können Richtlinien für bedingten Zugriff erstellen, die Azure AD Identity Protection-Risikoerkennungen umfassen. Es gibt drei Standardrichtlinien, die sofort ohne Konfigurationsaufwand aktiviert werden können. 

* Festlegen, dass sich alle Benutzer für Azure Multi-Factor Authentication registrieren müssen
* Festlegen, dass Benutzer, die ein hohes Risiko darstellen, ihr Kennwort ändern müssen
* Festlegen, dass Benutzer mit einem mittleren oder hohen Anmelderisiko die mehrstufige Authentifizierung verwenden müssen

## <a name="require-all-users-to-register-for-azure-multi-factor-authentication"></a>Festlegen, dass sich alle Benutzer für Azure Multi-Factor Authentication registrieren müssen

Wenn Sie diese Richtlinie aktivieren, müssen sich alle Benutzer innerhalb von 14 Tagen für Azure Multi-Factor Authentication registrieren. 

1. Melden Sie sich beim **Azure-Portal** an.
1. Klicken Sie auf **Alle Dienste**, und navigieren Sie zu **Azure AD Identity Protection**.
1. Klicken Sie auf **MFA-Registrierung**.
1. Wählen Sie unter **Zuweisungen** die Option **Benutzer** aus.
   1. Wählen Sie unter **Einschließen** die Option **Alle Benutzer** aus.
   1. Wählen Sie unter **Ausschließen** die Option **Ausgeschlossene Benutzer auswählen** und dann die Konten für den Notfallzugriff Ihres Unternehmens aus. Abschließend wählen Sie **Auswählen** aus. 
   1. Wählen Sie **Fertig**aus.
1. Legen Sie **Richtlinie erzwingen** auf **Ein** fest.
1. Klicken Sie auf **Speichern**.

## <a name="require-a-password-change-high-risk-users"></a>Festlegen, dass Benutzer, die ein hohes Risiko darstellen, ihr Kennwort ändern müssen

Microsoft arbeitet mit Ermittlern, Strafverfolgungsbehörden, verschiedenen Sicherheitsteams bei Microsoft und anderen vertrauenswürdigen Quellen zusammen, um Benutzername/Kennwort-Paare zu finden. Wenn eines dieser Paare eine Übereinstimmung mit einem Konto in Ihrer Umgebung aufweist, kann eine risikobasierte Kennwortänderung über die folgende Richtlinie ausgelöst werden.

1. Melden Sie sich beim **Azure-Portal** an.
1. Klicken Sie auf **Alle Dienste**, und navigieren Sie zu **Azure AD Identity Protection**.
1. Klicken Sie auf die **Richtlinie zum Benutzerrisiko**.
1. Wählen Sie unter **Zuweisungen** die Option **Benutzer** aus.
   1. Wählen Sie unter **Einschließen** die Option **Alle Benutzer** aus.
   1. Wählen Sie unter **Ausschließen** die Option **Ausgeschlossene Benutzer auswählen** und dann die Konten für den Notfallzugriff Ihres Unternehmens aus. Abschließend wählen Sie **Auswählen** aus.
   1. Wählen Sie **Fertig**aus.
1. Wählen Sie unter **Bedingungen** die Option **Benutzerrisiko** und dann **Hoch** aus.
   1. Klicken Sie auf **Auswählen** und dann auf **Fertig**.
1. Wählen Sie unter **Kontrollen** > **Zugriff** die Option **Zugriff zulassen** und dann **Kennwortänderung anfordern** aus.
   1. Klicken Sie auf **Auswählen**.
1. Legen Sie **Richtlinie erzwingen** auf **Ein** fest.
1. Klicken Sie auf **Speichern**.

## <a name="require-mfa-medium-or-high-sign-in-risk-users"></a>Festlegen, dass Benutzer mit einem mittleren oder hohen Anmelderisiko die mehrstufige Authentifizierung verwenden müssen

Die meisten Benutzer weisen ein normales Verhalten auf, das nachverfolgt werden kann. Wenn sie sich aber außerhalb dieser Norm bewegen, ist es ggf. riskant, ihnen das Anmelden ohne Weiteres zu erlauben. Es kann ratsam sein, den entsprechenden Benutzer zu blockieren oder ggf. einfach um die Durchführung einer mehrstufigen Authentifizierung zu bitten. So kann bewiesen werden, ob es sich auch wirklich um die Person handelt, die vorgegeben wird. Aktivieren Sie die folgende Richtlinie, um MFA zu erzwingen, wenn eine riskante Anmeldung erkannt wird.

1. Melden Sie sich beim **Azure-Portal** an.
1. Klicken Sie auf **Alle Dienste**, und navigieren Sie zu **Azure AD Identity Protection**.
1. Klicken Sie auf **Richtlinie zum Anmelderisiko**.
1. Wählen Sie unter **Zuweisungen** die Option **Benutzer** aus.
   1. Wählen Sie unter **Einschließen** die Option **Alle Benutzer** aus.
   1. Wählen Sie unter **Ausschließen** die Option **Ausgeschlossene Benutzer auswählen** und dann die Konten für den Notfallzugriff Ihres Unternehmens aus. Abschließend wählen Sie **Auswählen** aus.
   1. Wählen Sie **Fertig**aus.
1. Wählen Sie unter **Bedingungen** die Option **Anmelderisiko** und dann **Mittel und darüber**.
   1. Klicken Sie auf **Auswählen** und dann auf **Fertig**.
1. Wählen Sie unter **Kontrollen** > **Zugriff** die Option **Zugriff zulassen** und dann **Multi-Factor Authentication erforderlich** aus.
   1. Klicken Sie auf **Auswählen**.
1. Legen Sie **Richtlinie erzwingen** auf **Ein** fest.
1. Klicken Sie auf **Speichern**.

## <a name="next-steps"></a>Nächste Schritte

[Allgemeine Richtlinien für bedingten Zugriff](concept-conditional-access-policy-common.md)

[Simulieren des Anmeldeverhaltens mit dem Was-wäre-wenn-Tool für den bedingten Zugriff](troubleshoot-conditional-access-what-if.md)

[So funktioniert's: Azure Multi-Factor Authentication](../authentication/concept-mfa-howitworks.md).

[Was ist Azure Active Directory Identity Protection?](../identity-protection/overview.md)
