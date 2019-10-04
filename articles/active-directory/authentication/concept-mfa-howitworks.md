---
title: Azure Multi-Factor Authentication – So funktioniert‘s – Azure Active Directory
description: Azure Multi-Factor Authentication hilft beim Schutz des Zugriffs auf Daten und Anwendungen und erfüllt gleichzeitig die Anforderungen von Benutzern an ein einfaches Anmeldeverfahren.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 06/03/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 57b441803a066b794d17b46afec4d930b94ebdea
ms.sourcegitcommit: 07700392dd52071f31f0571ec847925e467d6795
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2019
ms.locfileid: "70125131"
---
# <a name="how-it-works-azure-multi-factor-authentication"></a>So funktioniert's: Azure Multi-Factor Authentication

Die Sicherheit der zweistufigen Überprüfung liegt im Ebenenansatz. Die Faktoren der mehrfachen Authentifizierung zu überwinden stellt eine große Herausforderung für Angreifer dar. Selbst wenn ein Angreifer das Kennwort des Benutzers herausfinden kann, ist dies nutzlos, wenn er nicht auch die zusätzliche Authentifizierungsmethode beherrscht. Dies funktioniert durch das Anfordern von mindestens zwei der folgenden Authentifizierungsmethoden:

* Etwas, das Sie wissen (normalerweise ein Kennwort)
* Etwas, das Sie haben (ein vertrautes Gerät, das nicht leicht dupliziert werden kann, wie ein Telefon)
* Etwas, das Sie sind (biometrisch)

<center>

![Bild konzeptioneller Authentifizierungsmethoden](./media/concept-mfa-howitworks/methods.png)</center>

Azure Multi-Factor Authentication (MFA) trägt zum Schutz des Zugriffs auf Daten und Anwendungen bei und ist dabei für den Benutzer unkompliziert. Indem eine zweite Form der Authentifizierung verlangt wird, bietet das Verfahren zusätzliche Sicherheit und eine zuverlässige Authentifizierung über verschiedene einfache [Authentifizierungsmethoden](concept-authentication-methods.md). Benutzer können auf der Grundlage von Konfigurationsentscheidungen, die ein Administrator trifft, zur MFA aufgefordert werden oder auch nicht.

## <a name="how-to-get-multi-factor-authentication"></a>Beziehen von Multi-Factor Authentication

Multi-Factor Authentication ist Teil der folgenden Angebote:

* **Azure Active Directory Premium** oder **Microsoft 365 Business** – Nutzung der Azure Multi-Factor Authentication in vollem Umfang, einschließlich Richtlinien für bedingten Zugriff, die die Multi-Factor Authentication erforderlich machen.

* **Azure AD Free**- oder eigenständige **Office 365**-Lizenzen – Nutzung vorab erstellter [Richtlinien für bedingten Zugriff für den Baselineschutz](../conditional-access/concept-baseline-protection.md), die die Multi-Factor Authentication für Benutzer und Administratoren erforderlich machen

* **Azure Active Directory – Globale Administratoren**: Eine Teilmenge der Azure Multi-Factor Authentication-Funktionen ist als Mittel zum Schutz globaler Administratorkonten verfügbar.

> [!NOTE]
> Ab dem 1. September 2018 können Neukunden Azure Multi-Factor Authentication nicht mehr als eigenständiges Angebot erwerben. Multi-Factor Authentication ist in Azure AD Premium-Lizenzen weiterhin ein verfügbares Feature.

## <a name="supportability"></a>Unterstützungsmöglichkeiten

Die meisten Benutzer sind daran gewöhnt, für die Authentifizierung lediglich Kennwörter zu verwenden. Daher ist es wichtig, dass Ihre Organisation das Bewusstsein für diesen Prozess bei sämtlichen Benutzern fördert. So lassen sich Anrufe beim Helpdesk aufgrund kleinerer Probleme im Zusammenhang mit MFA reduzieren. In einigen Szenarien muss MFA jedoch vorübergehend deaktiviert werden. Befolgen Sie die unten stehenden Richtlinien für den Umgang mit diesen Szenarien:

* Schulen Sie Ihre Supportmitarbeiter für Szenarien, in denen Benutzer sich nicht anmelden können, da sie keinen Zugriff auf ihre Authentifizierungsmethoden haben, oder diese nicht ordnungsgemäß funktionieren.
   * Mit Richtlinien für bedingten Zugriff für den Azure MFA-Dienst können Ihre Supportmitarbeiter einen Benutzer einer Gruppe hinzufügen, die von einer MFA erfordernden Richtlinie ausgeschlossen ist.
* Sie können benannte Standorte für den bedingten Zugriff verwenden, um Aufforderungen zur zweistufigen Überprüfung zu minimieren. Mithilfe dieser Funktion können Administratoren die zweistufige Überprüfung für Benutzer umgehen, die sich über ein sicheres vertrauenswürdiges Netzwerk anmelden, z. B. ein Netzwerksegment, das für das Onboarding neuer Benutzer verwendet wird.
* Stellen Sie [Azure AD Identity Protection](../active-directory-identityprotection.md) bereit, und lösen Sie die zweistufige Überprüfung basierend auf Risikoerkennungen aus.

## <a name="next-steps"></a>Nächste Schritte

- [Schrittweise Bereitstellung der Azure Multi-Factor Authentication](howto-mfa-getstarted.md)
