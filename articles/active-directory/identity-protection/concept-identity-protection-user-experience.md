---
title: Benutzererfahrungen mit Azure AD Identity Protection
description: Benutzeroberfläche von Azure AD Identity Protection
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
ms.openlocfilehash: cc10fb4f9894a355c9eed024ae9f87747214999b
ms.sourcegitcommit: 7efb2a638153c22c93a5053c3c6db8b15d072949
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2019
ms.locfileid: "72886713"
---
# <a name="user-experiences-with-azure-ad-identity-protection"></a>Benutzererfahrungen mit Azure AD Identity Protection

Azure Active Directory Identity Protection bietet folgende Möglichkeiten:

* Festlegen, dass sich Benutzer für Azure Multi-Factor Authentication (MFA) registrieren müssen
* Automatisieren der Behandlung riskanter Anmeldungen und kompromittierter Benutzer

Alle Identitätsschutzrichtlinien haben Auswirkungen auf den Benutzeranmeldevorgang. Indem Sie Benutzern das Registrieren für und Verwenden von Tools wie Azure MFA und die Self-Service-Kennwortzurücksetzung erlauben, können die Auswirkung verringert werden. Diese Tools bieten Benutzern in Kombination mit der Auswahl geeigneter Richtlinien bei Bedarf eine Option zur Eigenwartung.

## <a name="multi-factor-authentication-registration"></a>Registrierung für die mehrstufige Authentifizierung

Indem Sie die Identity Protection-Richtlinie, die eine Multi-Factor Authentication-Registrierung erfordert, für alle Benutzer aktivieren, wird sichergestellt, dass diese in Zukunft die Möglichkeit zur Eigenwartung mit Azure MFA haben. Wenn Sie diese Richtlinie konfigurieren, können sich Ihre Benutzer in den darauffolgenden 14 Tagen freiwillig registrieren. Nach Ablauf dieses Zeitraums wird die Registrierung erzwungen. Die Benutzeroberfläche wird unten beschrieben. Weitere Informationen finden Sie in der Dokumentation für Endbenutzer im Artikel [Übersicht über die zweistufige Überprüfung und Ihr Geschäfts-, Schul- oder Unikonto](../user-help/user-help-two-step-verification-overview.md).

### <a name="registration-interrupt"></a>Registrierungsunterbrechung

1. Bei der Anmeldung bei einer beliebigen in Azure AD integrierten Anwendung wird der Benutzer davon in Kenntnis gesetzt, dass das Konto für die mehrstufige Authentifizierung eingerichtet werden muss. Diese Richtlinie wird auch in Windows 10 Out-of-Box-Experience für neue Benutzer mit einem neuen Gerät ausgelöst.
   
    ![Weitere Informationen erforderlich](./media/concept-identity-protection-user-experience/identity-protection-experience-more-info-mfa.png)

1. Führen Sie die begleiteten Schritte zum Registrieren bei Azure Multi-Factor Authentication aus, und schließen Sie Ihre Anmeldung ab.

## <a name="risky-sign-in-remediation"></a>Behandlung riskanter Anmeldungen

Wenn ein Administrator eine Richtlinie für Anmelderisiken konfiguriert hat, werden die betroffenen Benutzer benachrichtigt, wenn sie versuchen, sich anzumelden und die Risikostufe der Richtlinie ausgelöst wird. 

### <a name="risky-sign-in-self-remediation"></a>Eigenwartung riskanter Anmeldungen

1. Der Benutzer wird informiert, dass im Zusammenhang mit seiner Anmeldung eine Unregelmäßigkeit erkannt wurde (beispielsweise eine Anmeldung an einem neuen Standort, mit einem neuen Gerät oder mit einer neuen App).
   
    ![Eingabeaufforderung bei ungewöhnlicher Aktion](./media/concept-identity-protection-user-experience/120.png)

1. Der Benutzer muss seine Identität nachweisen, indem er Azure MFA mit einer der zuvor registrierten Methoden ausführt. 

### <a name="risky-sign-in-administrator-unblock"></a>Aufhebung der Blockierung bei riskanten Anmeldungen durch Administrator

Administratoren können eine Benutzer bei der Anmeldung je nach Risikostufe blockieren. Zur Aufhebung der Blockierung muss sich der Endbenutzer an die IT-Mitarbeiter der Organisation wenden oder sich an einem bekannten Standort oder mit einem bekannten Gerät anmelden. In diesem Fall ist keine Eigenwartung mittels mehrstufiger Authentifizierung möglich.

![Blockiert durch Anmelderisikorichtlinie](./media/concept-identity-protection-user-experience/200.png)

Die IT-Mitarbeiter können die Anweisungen im Abschnitt [Aufheben der Blockierung von Benutzern](howto-identity-protection-remediate-unblock.md#unblocking-based-on-sign-in-risk) befolgen, um Benutzern die Anmeldung wieder zu ermöglichen.

## <a name="risky-user-remediation"></a>Behandlung bei riskanten Benutzern

Wenn eine Benutzerrisikorichtlinie konfiguriert wurde, müssen Benutzer, die der Benutzerrisiko-Wahrscheinlichkeitsstufe entsprechen (und somit als gefährdet betrachtet werden), das Wiederherstellungsverfahren für kompromittierte Benutzer durchlaufen, um sich wieder anmelden zu können. 

### <a name="risky-user-self-remediation"></a>Eigenwartung bei riskanten Benutzern

1. Der Benutzer wird darüber informiert, dass die Sicherheit des Kontos aufgrund von verdächtigen Aktivitäten oder kompromittierten Anmeldeinformationen gefährdet ist.
   
    ![Wiederherstellung](./media/concept-identity-protection-user-experience/101.png)

1. Der Benutzer muss seine Identität nachweisen, indem er Azure MFA mit einer der zuvor registrierten Methoden ausführt. 
1. Am Ende des Vorgangs muss der Benutzer sein Kennwort mit der Self-Service-Kennwortzurücksetzung ändern, da unter Umständen eine andere Person Zugriff auf sein Konto hatte.

## <a name="risky-sign-in-administrator-unblock"></a>Aufhebung der Blockierung bei riskanten Anmeldungen durch Administrator

Administratoren können eine Benutzer bei der Anmeldung je nach Risikostufe blockieren. Zur Aufhebung der Blockierung muss sich der Endbenutzer an die IT-Mitarbeiter der Organisation wenden. In diesem Fall ist keine Eigenwartung mittels mehrstufiger Authentifizierung und Self-Service-Kennwortzurücksetzung möglich.

![Blockiert durch Benutzerrisikorichtlinie](./media/concept-identity-protection-user-experience/104.png)

Die IT-Mitarbeiter können die Anweisungen im Abschnitt [Aufheben der Blockierung von Benutzern](howto-identity-protection-remediate-unblock.md#unblocking-based-on-user-risk) befolgen, um Benutzern die Anmeldung wieder zu ermöglichen.

## <a name="see-also"></a>Weitere Informationen

- [Beheben von Risiken und Aufheben der Blockierung von Benutzern](howto-identity-protection-remediate-unblock.md)

- [Azure Active Directory Identity Protection](../active-directory-identityprotection.md) 
