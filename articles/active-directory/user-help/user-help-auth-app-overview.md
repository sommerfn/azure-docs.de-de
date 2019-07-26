---
title: Übersicht über die Microsoft Authenticator-App – Azure Active Directory | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, was die Microsoft Authenticator-App ist, wie Sie funktioniert und welche Informationen sie enthält.
services: active-directory
author: eross-msft
manager: daveba
ms.reviewer: sahenry
ms.service: active-directory
ms.subservice: user-help
ms.workload: identity
ms.topic: overview
ms.date: 01/24/2019
ms.author: lizross
ms.collection: M365-identity-device-management
ms.openlocfilehash: d3766436851cb2f31fa7bb070402191e03ad9ad1
ms.sourcegitcommit: 04ec7b5fa7a92a4eb72fca6c6cb617be35d30d0c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2019
ms.locfileid: "68382336"
---
# <a name="what-is-the-microsoft-authenticator-app"></a>Wozu dient die Microsoft Authenticator-App?

>[!Important]
>Dieser Inhalt richtet sich an Benutzer. Administratoren finden in der [Azure Active Directory-Dokumentation](https://docs.microsoft.com/azure/active-directory) weitere Informationen zum Einrichten und Verwalten der Azure Active Directory-Umgebung (Azure AD).

Die Microsoft Authenticator-App unterstützt Sie bei der Kontoanmeldung, wenn Sie die zweistufige Überprüfung verwenden. Mit der zweistufigen Überprüfung können Sie die Sicherheit des Zugriffs auf Ihre Konten erhöhen, insbesondere, wenn Sie vertrauliche Informationen anzeigen. Kennwörter können vergessen, gestohlen oder kompromittiert werden. Die zweistufige Überprüfung ist eine zusätzliche Sicherheitsmaßnahme, die zum Schutz Ihres Kontos beiträgt, indem sie den Zugriff durch Unbefugte erschwert.

Die Verwendung der Microsoft Authenticator-App kann auf verschiedene Arten erfolgen:

- Zur Reaktion auf eine Authentifizierungsaufforderung, nachdem Sie sich mit Ihrem Benutzernamen und Kennwort angemeldet haben

- Zur Anmeldung ohne Kennwort unter Verwendung Ihres Benutzernamens, der Authenticator-App und Ihres mobilen Geräts mit Fingerabdruck, Gesichtserkennung oder PIN

- Als Codegenerator für andere Konten, die Authentifikator-Apps unterstützen

> [!Important]
> Die Microsoft Authenticator-App funktioniert mit jedem Konto, für das die zweistufige Überprüfung verwendet wird, und unterstützt die Standards für zeitbasierte Einmalkennwörter (Time-Based One-Time Passwords, TOTPs).
>
> Ihre Organisation erfordert möglicherweise die Verwendung einer Authentifikator-App zum Anmelden und für den Zugriff auf Ihre Organisationsdaten und -dokumente. Ihr Benutzername kann in der App zwar angezeigt werden, das Konto ist jedoch erst tatsächlich als Überprüfungsmethode eingerichtet, nachdem Sie den Registrierungsprozess abgeschlossen haben. Weitere Informationen finden Sie unter [Hinzufügen Ihres Geschäfts-, Schul- oder Unikontos](user-help-auth-app-add-work-school-account.md).
> 
> Wenn Sie Probleme bei der Anmeldung bei Ihrem Konto haben, lesen Sie den Artikel [Wenn Sie sich nicht bei Ihrem Microsoft-Konto anmelden können](https://support.microsoft.com/help/12429). Erhalten Sie weitere Informationen zur Meldung [Dieses Microsoft-Konto ist nicht vorhanden.](https://support.microsoft.com/help/13811), wenn Sie sich bei Ihrem Microsoft-Konto anmelden.

## <a name="terminology"></a>Begriff

| Begriff|BESCHREIBUNG|
| ----|-----------|
| Zweistufige Überprüfung | Bei dieser Überprüfung benötigen Sie in der Regel nur zwei Angaben zur Überprüfung, etwa ein Kennwort und eine PIN. Die Microsoft Authenticator-App unterstützt sowohl die standardmäßige zweistufige Überprüfung als auch die Anmeldung ohne Kennwort. |
| Multi-Factor Authentication (MFA) | Jede zweistufige Überprüfung stellt eine mehrstufige Authentifizierung dar. Dabei müssen Sie je nach den Anforderungen Ihrer Organisation *mindestens* zwei Angaben zur Überprüfung machen. |
| Microsoft-Konto (auch MSA genannt) | Sie erstellen Ihre persönlichen Microsoft-Konten, um Zugriff auf Ihre verbraucherorientierten Microsoft-Produkte und -Clouddienste wie Outlook, OneDrive, MSN, Xbox LIVE oder Office 365 zu erhalten. Ihr Microsoft-Konto wird im von Microsoft betriebenen Microsoft-Kontosystem für Endbenutzeridentitäten erstellt und gespeichert. |
| Geschäfts- oder Schulkonto | Ihre Organisation erstellt Ihr Geschäfts-, Schul- oder Unikonto (z.B. alain@contoso.com), über das Sie Zugriff auf interne und möglicherweise eingeschränkte Ressourcen wie Microsoft Azure, Windows Intune und Office 365 erhalten. |
| Prüfcode | Dies ist der sechsstellige Code, der in der Authenticator-App unter jedem hinzugefügten Konto angezeigt wird. Der Prüfcode wird alle 30 Sekunden geändert, um zu verhindern, dass Benutzer einen Code mehrmals verwenden. Dies wird auch als Einmalkennung (One-Time Passcode, OTP) bezeichnet. |

## <a name="how-two-factor-verification-works-with-the-app"></a>Funktionsweise der zweistufigen Überprüfung mit der App

Die zweistufige Überprüfung mit der Microsoft Authenticator-App funktioniert wie folgt:

- **Benachrichtigung**. Geben Sie Ihren Benutzernamen und Ihr Kennwort auf dem Gerät ein, auf dem Sie sich bei Ihrem Geschäfts-, Schul- oder Unikonto bzw. Ihrem persönlichen Microsoft-Konto anmelden. Die Microsoft Authenticator-App sendet eine Benachrichtigung mit der Frage **Anmeldung genehmigen?** . Wählen Sie **Genehmigen** aus, wenn Sie den Anmeldeversuch anerkennen. Andernfalls wählen Sie **Verweigern** aus. Wenn Sie **Ablehnen** wählen, können Sie die Anforderung auch als betrügerisch markieren.

- **Prüfcode**. Geben Sie Ihren Benutzernamen und Ihr Kennwort auf dem Gerät ein, auf dem Sie sich bei Ihrem Geschäfts-, Schul- oder Unikonto bzw. Ihrem persönlichen Microsoft-Konto anmelden. Fügen Sie dann den zugehörigen Prüfcode vom Bildschirm **Konten** der Microsoft Authenticator-App ein. Der Überprüfungscode wird auch als Einmalkennung (One-Time Passcode, OTP) für die Authentifizierung bezeichnet.

- **Anmeldung ohne Kennwort.** Geben Sie Ihren Benutzernamen und Ihr Kennwort auf dem Gerät ein, auf dem Sie sich bei Ihrem Geschäfts-, Schul- oder Unikonto bzw. Ihrem persönlichen Microsoft-Konto anmelden. Bestätigen Sie dann auf Ihrem mobilen Gerät per Fingerabdruck, Gesichtserkennung oder PIN Ihre Identität. Bei dieser Methode müssen Sie kein Kennwort eingeben.

### <a name="whether-to-use-your-devices-biometric-capabilities"></a>Verwenden der Biometriefunktionen Ihres Geräts

Wenn Sie eine PIN verwenden, um die Authentifizierung abzuschließen, können Sie in der Microsoft Authenticator-App die Biometriefunktionen Ihres Geräts (Fingerabdruck oder Gesichtserkennung) aktivieren. Sie können diese Funktionalität einrichten, wenn Sie die Authenticator-App zum ersten Mal zum Überprüfen Ihres Kontos verwenden. Wählen Sie dabei die Option zum Verwenden der Biometriefunktionen Ihres Geräts anstelle Ihrer PIN für die Identifizierung aus.

## <a name="who-decides-if-you-use-this-feature"></a>Wer entscheidet über die Verwendung dieser Funktion?

Je nach Kontotyp entscheidet Ihre Organisation, ob Sie die zweistufige Überprüfung verwenden müssen. Möglicherweise können Sie diese Entscheidung aber auch selbst treffen.

- **Geschäfts-, Schul- oder Unikonto.** Wenn Sie ein Geschäfts-, Schul- oder Uni-Konto verwenden (z. B. alain@contoso.com), entscheidet Ihre Organisation, ob Sie die zweistufige Überprüfung verwenden müssen. Sie gibt zudem vor, welche Überprüfungsmethode anzuwenden ist. Weitere Informationen dazu, wie Sie Ihr Geschäfts-, Schul- oder Unikonto der Microsoft Authenticator-App hinzufügen, finden Sie im Artikel [Hinzufügen Ihres Geschäfts-, Schul- oder Unikontos](user-help-auth-app-add-work-school-account.md).

- **Persönliches Microsoft-Konto.** Sie können die zweistufige Überprüfung auch für Ihre persönlichen Microsoft-Konten einrichten (z.B. alain@outlook.com). Weitere Informationen zum Hinzufügen Ihres persönlichen Microsoft-Kontos finden Sie unter [Hinzufügen Ihrer persönlichen Microsoft-Konten](user-help-auth-app-add-personal-ms-account.md).

- **Hinzufügen Ihrer Nicht-Microsoft-Konten**. Sie können die zweistufige Überprüfung auch für Ihre nicht von Microsoft bereitgestellten Konten einrichten (z. B. alain@gmail.com). Ihre nicht von Microsoft bereitgestellten Konten verwenden möglicherweise nicht den Begriff „zweistufige Überprüfung“. Sie finden die Funktion jedoch in der Regel in den Einstellungen für die **Sicherheit** oder die **Anmeldung**. Die Microsoft Authenticator-App funktioniert mit allen Konten, für die Standards für zeitbasierte Einmalkennwörter (TOTP) unterstützt werden. Weitere Informationen zum Hinzufügen von Nicht-Microsoft-Konten finden Sie unter [Hinzufügen Ihrer Nicht-Microsoft-Konten](user-help-auth-app-add-non-ms-account.md).

## <a name="in-this-section"></a>Inhalt dieses Abschnitts

| Artikel | BESCHREIBUNG |
| ------ | ------------ |
| [Herunterladen und Installieren der App](user-help-auth-app-download-install.md) | Hier erfahren Sie, wo und wie Sie die Microsoft Authenticator-App für Geräte mit Android und iOS herunterladen und installieren. |
| [Add your work or school accounts](user-help-auth-app-add-work-school-account.md) (Hinzufügen von Geschäfts-, Schul- oder Unikonten) | Hier erfahren Sie, wie Sie Geschäfts-, Schul- oder Unikonten bzw. persönliche Konten zur Microsoft Authenticator-App hinzufügen. |
| [Add your personal accounts](user-help-auth-app-add-personal-ms-account.md) (Hinzufügen persönlicher Konten) | Hier erfahren Sie, wie Sie der Microsoft Authenticator-App persönliche Microsoft-Konten hinzufügen. |
| [Hinzufügen Ihrer Nicht-Microsoft-Konten](user-help-auth-app-add-non-ms-account.md) | Hier erfahren Sie, wie Sie der Microsoft Authenticator-App Konten hinzufügen, die nicht von Microsoft stammen. |
| [Manually add your accounts](user-help-auth-app-add-account-manual.md) (Manuelles Hinzufügen von Konten) | Hier erfahren Sie, wie Sie der Microsoft Authenticator-App Ihre Konten manuell hinzufügen, wenn der bereitgestellte QR-Code nicht gescannt werden kann. |
| [Sign-in using the app](user-help-auth-app-sign-in.md) (Anmelden mit der App) | Hier erfahren Sie, wie Sie sich mit der Microsoft Authenticator-App bei verschiedenen Konten anmelden.|
| [Sichern und Wiederherstellen von Kontoanmeldeinformationen](user-help-auth-app-backup-recovery.md) | Enthält Informationen zum Sichern und Wiederherstellen Ihrer Kontoanmeldeinformationen mithilfe der Microsoft Authenticator-App. |
| [Microsoft Authenticator-App – häufig gestellte Fragen](user-help-auth-app-faq.md) | Enthält Antworten auf häufig gestellte Fragen zur App. |
