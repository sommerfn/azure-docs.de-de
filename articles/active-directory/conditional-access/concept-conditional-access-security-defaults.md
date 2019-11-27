---
title: Azure Active Directory-Sicherheitsstandards
description: Informationen zu Sicherheitsstandardrichtlinien zum Schutz von Organisationen vor gängigen Angriffen
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 10/23/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: bde1741d12cc7ef181fb60bc7eecbec1fed8cbd0
ms.sourcegitcommit: 0b1a4101d575e28af0f0d161852b57d82c9b2a7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73151222"
---
# <a name="what-are-security-defaults"></a>Was sind Sicherheitsstandards?

Die Aufrechterhaltung der Sicherheit kann schwierig sein, da gängige identitätsbezogene Angriffe immer zahlreicher auftreten. Zu diesen gängigen Angriffen zählen Kennwortspray, Wiedergabe und Phishing.

Die Sicherheitsstandards in Azure Active Directory (Azure AD) erleichtern es, die Sicherheit zu erhöhen und Ihr Unternehmen vor gängigen Angriffen zu schützen. Sie umfassen vorkonfigurierte Sicherheitseinstellungen für diese verbreiteten Angriffe. 

Microsoft stellt allen Benutzern Sicherheitsstandards zur Verfügung. Das Ziel ist sicherzustellen, dass in allen Organisationen eine Standardsicherheitsebene ohne zusätzliche Kosten aktiviert ist. Sie aktivieren die Sicherheitsstandards im Azure-Portal.

![Screenshot: Azure-Portal mit Umschaltfläche zum Aktivieren der Sicherheitsstandards](./media/concept-conditional-access-security-defaults/security-defaults-azure-ad-portal.png)
 
Die folgenden Sicherheitskonfigurationen werden in Ihrem Mandanten aktiviert. 

## <a name="unified-multi-factor-authentication-registration"></a>Einheitliche Registrierung für die mehrstufige Authentifizierung

Alle Benutzer in Ihrem Mandanten müssen sich über den Azure Multi-Factor Authentication-Dienst für die mehrstufige Authentifizierung (Multi-Factor Authentication, MFA) registrieren. Benutzer haben 14 Tage Zeit, sich mithilfe der Microsoft Authenticator-App für MFA zu registrieren. Nach Ablauf der 14 Tage kann der Benutzer sich erst wieder anmelden, nachdem die MFA-Registrierung durchgeführt wurde.

Uns ist bewusst, dass einige Benutzer möglicherweise abwesend sind oder sich unmittelbar nach Aktivierung der Sicherheitsstandards nicht innerhalb von 14 Tagen anmelden werden. Um sicherzustellen, dass jedem Benutzer ausreichend Zeit für die MFA-Registrierung eingeräumt wird, ist der Zeitraum von 14 Tagen für jeden Benutzer individuell festgelegt. Die 14-tägige Frist eines Benutzers beginnt nach seiner ersten erfolgreichen interaktiven Anmeldung nach Aktivierung der Sicherheitsstandards.

## <a name="multi-factor-authentication-enforcement"></a>Erzwingen der mehrstufigen Authentifizierung

### <a name="protecting-administrators"></a>Schützen von Administratoren

Benutzer mit Zugriff auf privilegierte Konten haben erweiterten Zugriff auf Ihre Umgebung. Aufgrund der weitreichenden Befugnisse, die diese Konten haben, sollten Sie sie mit Bedacht verwalten. Eine gängige Methode zur Verbesserung des Schutzes von privilegierten Konten ist eine strengere Form der Kontoüberprüfung für die Anmeldung. In Azure AD können Sie eine striktere Kontoüberprüfung erreichen, indem Sie eine mehrstufige Authentifizierung anfordern.

Nach Abschluss der MFA-Registrierung ist für die folgenden 9 Azure AD-Administratorrollen bei jeder Anmeldung eine zusätzliche Authentifizierung erforderlich:

- Globaler Administrator
- SharePoint-Administrator
- Exchange-Administrator
- Administrator für den bedingten Zugriff
- Sicherheitsadministrator
- Helpdeskadministrator oder Kennwortadministrator
- Rechnungsadministrator
- Benutzeradministrator
- Authentifizierungsadministrator

### <a name="protecting-all-users"></a>Schützen aller Benutzer

Wir gehen häufig davon aus, dass nur Administratorkonten durch eine mehrstufige Authentifizierung geschützt werden müssen. Administratoren haben umfassenden Zugriff auf vertrauliche Informationen und können Änderungen an abonnementweiten Einstellungen vornehmen. Angriffe richten sich jedoch häufig gegen Endbenutzer. 

Nachdem die Angreifer Zugang erhalten haben, können sie im Namen des ursprünglichen Kontoinhabers Zugriff auf privilegierten Informationen anfordern. Sie können sogar das gesamte Verzeichnis herunterladen, um einen Phishing-Angriff auf Ihr gesamtes Unternehmen durchzuführen. 

Eine gängige Methode zur Verbesserung des Schutzes für alle Benutzer besteht in einer strengeren Kontoüberprüfung, beispielsweise durch eine mehrstufige Authentifizierung (MFA). Nachdem die Benutzer die MFA-Registrierung abgeschlossen haben, werden sie bei Bedarf zu einer zusätzlichen Authentifizierung aufgefordert.

### <a name="blocking-legacy-authentication"></a>Blockieren der Legacyauthentifizierung

Um Ihren Benutzern den einfachen Zugriff auf Ihre Cloud-Apps zu ermöglichen, unterstützt Azure AD eine Vielzahl von Authentifizierungsprotokollen – einschließlich der Legacyauthentifizierung. Der Begriff *Legacyauthentifizierung* bezieht sich auf eine Authentifizierungsanforderung von:

- älteren Office-Clients, die keine moderne Authentifizierung verwenden (z. B. ein Office 2010-Client)
- jedem Client, der ältere E-Mail-Protokolle wie IMAP, SMTP oder POP3 verwendet

Der Großteil aller gefährdenden Anmeldeversuche erfolgt über Legacyauthentifizierungen. Die Legacyauthentifizierung unterstützt keine mehrstufige Authentifizierung. Selbst wenn Sie in Ihrem Verzeichnis eine MFA-Richtlinie aktiviert haben, kann sich ein Angreifer mit einem älteren Protokoll authentifizieren und die mehrstufige Authentifizierung umgehen. 

Nach Aktivierung der Sicherheitsstandards in Ihrem Mandanten werden alle Authentifizierungsanforderungen blockiert, die über ein Legacyprotokoll an einen beliebigen Mandanten gerichtet werden. Exchange ActiveSync wird von den Sicherheitsstandards nicht blockiert.

### <a name="protecting-privileged-actions"></a>Schützen privilegierter Aktionen

Organisationen verwenden eine Vielzahl von Azure-Diensten, die über die Azure Resource Manager-API verwaltet werden, darunter z. B.:

- Azure-Portal 
- Azure PowerShell 
- Azure-Befehlszeilenschnittstelle

Benutzer, die Dienste mit Azure Resource Manager verwalten, verfügen über weitreichende Berechtigungen. Beispielsweise können mit Azure Resource Manager mandantenweite Konfigurationen wie Diensteinstellungen und die Abonnementabrechnung geändert werden. Die einstufige Authentifizierung ist für eine Vielzahl von Bedrohungen anfällig, beispielsweise für Phishing- und Kennwortspray-Angriffe. 

Es ist wichtig, die Identität der Benutzer zu überprüfen, die auf Azure Resource Manager zugreifen und Konfigurationen aktualisieren möchten. Sie überprüfen die Identität von Benutzern, indem Sie vor der Zugriffserteilung eine zusätzliche Authentifizierung anfordern.

Nach Aktivierung der Sicherheitsstandards in Ihrem Mandanten muss jeder Benutzer, der auf das Azure-Portal, Azure PowerShell oder die Azure CLI zugreift, eine zusätzliche Authentifizierung durchlaufen. Diese Richtlinie gilt für alle Benutzer mit Zugriff auf Azure Resource Manager – nicht nur für Administratoren. 

Wenn der Benutzer nicht für die MFA registriert ist, muss er sich mithilfe der Microsoft Authenticator-App registrieren, um den Vorgang fortzusetzen. Es wird keine 14-tägige Frist für die MFA-Registrierung eingeräumt.

## <a name="deployment-considerations"></a>Überlegungen zur Bereitstellung

Nachfolgend werden zusätzliche Aspekte im Zusammenhang mit der Bereitstellung von Sicherheitsstandards für Ihren Mandanten beleuchtet.

### <a name="older-protocols"></a>Ältere Protokolle

E-Mail-Clients verwenden ältere Authentifizierungsprotokolle (IMAP, SMTP und POP3) für Authentifizierungsanforderungen. Diese Protokolle bieten keine Unterstützung für die mehrstufige Authentifizierung. Die meisten der von Microsoft beobachteten Kontoangriffe nutzen ältere Protokolle für den Versuch, die mehrstufige Authentifizierung zu umgehen. 

Um sicherzustellen, dass die MFA bei der Anmeldung mit einem Administratorkonto obligatorisch ist und von Angreifern nicht umgangen werden kann, blockieren die Sicherheitsstandards alle Authentifizierungsanforderungen, die von älteren Protokollen an Administratorkonten gerichtet werden.

> [!WARNING]
> Stellen Sie vor dem Aktivieren dieser Einstellung sicher, dass Ihre Administratoren keine älteren Authentifizierungsprotokolle verwenden. Weitere Informationen finden Sie unter [Blockieren der Legacyauthentifizierung](concept-conditional-access-block-legacy-authentication.md).

### <a name="conditional-access"></a>Bedingter Zugriff

Sie können mithilfe des bedingten Zugriffs Richtlinien konfigurieren, die ein vergleichbares Verhalten aufweisen wie die aktivierten Sicherheitsstandards. Wenn Sie mit dem bedingtem Zugriff arbeiten und in Ihrer Umgebung die Richtlinien für bedingten Zugriff aktiviert sind, stehen Ihnen die Sicherheitsstandards nicht zur Verfügung. Wenn Sie über eine Lizenz für die Bereitstellung des bedingten Zugriffs verfügen, aber keine Richtlinien für bedingten Zugriff in Ihrer Umgebung aktiviert sind, können Sie die Sicherheitsstandards solange nutzen, bis Sie Richtlinien für bedingten Zugriff aktivieren.

![Warnmeldung mit dem Hinweis, dass Sicherheitsstandards und bedingter Zugriff nicht gleichzeitig verwendet werden können](./media/concept-conditional-access-security-defaults/security-defaults-conditional-access.png)

Hier finden Sie Schrittanleitungen dazu, wie Sie mithilfe des bedingten Zugriffs äquivalente Richtlinien konfigurieren können:

- [Vorschreiben der MFA für Administratoren](howto-conditional-access-policy-admin-mfa.md)
- [Vorschreiben der MFA für die Azure-Verwaltung](howto-conditional-access-policy-azure-management.md)
- [Blockieren älterer Authentifizierungsmethoden](howto-conditional-access-policy-block-legacy.md)
- [Erzwingen der MFA für alle Benutzer](howto-conditional-access-policy-all-users-mfa.md)

## <a name="enabling-security-defaults"></a>Aktivieren von Sicherheitsstandards

So aktivieren Sie Sicherheitsstandards in Ihrem Verzeichnis

1. Melden Sie sich beim  [Azure-Portal](https://portal.azure.com)  als globaler Administrator, Sicherheitsadministrator oder Administrator für bedingten Zugriff an.
1. Wechseln Sie zu  **Azure Active Directory** > **Eigenschaften**.
1. Wählen Sie **Sicherheitsstandards verwalten** aus.
1. Legen Sie die Option **Sicherheitsstandards aktivieren** auf **Ja** fest.
1. Wählen Sie **Speichern** aus.

## <a name="next-steps"></a>Nächste Schritte

[Allgemeine Richtlinien für bedingten Zugriff](concept-conditional-access-policy-common.md)

[Was ist bedingter Zugriff?](overview.md)
