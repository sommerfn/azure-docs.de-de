---
title: Azure Active Directory-Sicherheitsstandards
description: Informationen zu Sicherheitsstandardrichtlinien zum Schutz von Organisationen vor gängigen Angriffen
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 10/15/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: b4921f0605f16a601f6e2ee9a15b71b50d253201
ms.sourcegitcommit: 77bfc067c8cdc856f0ee4bfde9f84437c73a6141
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2019
ms.locfileid: "72452924"
---
# <a name="what-are-security-defaults"></a>Was sind Sicherheitsstandards?

Die Aufrechterhaltung der Sicherheit kann schwierig sein, da gängige identitätsbezogene Angriffe, wie z. B. Kennwortspray, Replay und Phishing, immer zahlreicher und beliebter werden. Ziel der Sicherheitsstandards in Azure Active Directory (AD) ist es, eine einfachere Möglichkeit zu schaffen, Ihre Organisation besser vor diesen verbreiteten Angriffen zu schützen. Sicherheitsstandards erleichtern die Sicherheit und schützen Ihr Unternehmen vor gängigen Angriffen. In Sicherheitsstandards sind vorkonfigurierte Sicherheitseinstellungen für diese verbreiteten Angriffe enthalten. Microsoft stellt allen Benutzern Sicherheitsstandards zur Verfügung. Das Ziel ist sicherzustellen, dass in allen Organisationen eine Standardsicherheitsebene ohne zusätzliche Kosten aktiviert ist.

![Screenshot der Benutzeroberfläche der neuen Sicherheitsstandards](./media/concept-conditional-access-security-defaults/security-defaults-azure-ad-portal.png)
 
Die folgenden Sicherheitskonfigurationen werden in Ihrem Mandanten aktiviert. 

## <a name="unified-mfa-registration"></a>Einheitliche MFA-Registrierung

Alle Benutzer in Ihrem Mandanten müssen sich für Azure Multi-Factor Authentication (MFA) registrieren. Benutzer haben 14 Tage Zeit, sich mithilfe der Microsoft Authenticator-App für Azure MFA zu registrieren. Nach Ablauf der 14 Tage kann sich der Benutzer erst nach erfolgter MFA-Registrierung anmelden.

Uns ist bekannt, dass einige Benutzer möglicherweise abwesend sind und/oder sich nicht innerhalb von 14 Tagen unmittelbar nach der Aktivierung von Sicherheitsstandards anmelden werden. Um sicherzustellen, dass jedem Benutzer für die MFA-Registrierung ausreichend Zeit eingeräumt wird, ist der Zeitraum von 14 Tagen für jeden Benutzer individuell. Die 14-tägige Frist eines Benutzers beginnt nach seiner ersten erfolgreichen interaktiven Anmeldung, nachdem Sicherheitsstandards aktiviert wurden.

## <a name="mfa-enforcement"></a>Erzwingung der MFA

### <a name="protecting-administrators"></a>Schützen von Administratoren

Benutzer mit Zugriff auf privilegierte Konten haben erweiterten Zugriff auf Ihre Umgebung. Aufgrund der weitreichenden Befugnisse, die diese Konten haben, sollten Sie sie mit Bedacht verwalten. Eine gängige Methode, um den Schutz von privilegierten Konten zu verbessern, ist eine stärkere Form der Kontoüberprüfung bei der Anmeldung. In Azure Active Directory können Sie eine striktere Kontoüberprüfung erreichen, indem Sie eine mehrstufige Authentifizierung (Multi-Factor Authentication, MFA) anfordern.

Nach erfolgter MFA-Registrierung sind die folgenden neun Azure AD-Administratorrollen erforderlich, um MFA bei jeder einzelnen Anmeldung durchzuführen.

- Globaler Administrator
- SharePoint-Administrator
- Exchange-Administrator
- Administrator für den bedingten Zugriff
- Sicherheitsadministrator
- Helpdeskadministrator/Kennwortadministrator
- Rechnungsadministrator
- Benutzeradministrator
- Authentifizierungsadministrator

### <a name="protecting-all-users"></a>Schützen aller Benutzer

Wir gehen häufig davon aus, dass Administratorkonten die einzigen Konten sind, die durch die Multi-Factor Authentication (MFA) geschützt werden müssen. Administratoren haben umfassenden Zugriff auf vertrauliche Informationen und können Änderungen an abonnementweiten Einstellungen vornehmen. Angriffe richten sich jedoch häufig gegen Endbenutzer. Sobald Angreifer Zugang zum System haben, können sie im Namen des ursprünglichen Kontobesitzers auf vertrauliche Informationen zugreifen oder das vollständige Verzeichnis herunterladen, um einen Phishing-Angriff auf die gesamte Organisation einzuleiten. Eine gängige Methode, alle Benutzer besser zu schützen, besteht in einer restriktiveren Kontoüberprüfung, beispielsweise durch eine mehrstufige Authentifizierung (Multi-Factor Authentication, MFA). Nach erfolgter MFA-Registrierung werden Benutzer bei Bedarf zur MFA aufgefordert.

### <a name="blocking-legacy-authentication"></a>Blockieren der Legacyauthentifizierung

Um Ihren Benutzern den einfachen Zugriff auf Ihre Cloud-Apps zu ermöglichen, unterstützt Azure Active Directory (Azure AD) eine Vielzahl von Authentifizierungsprotokollen einschließlich der Legacyauthentifizierung. Der Begriff „Legacyauthentifizierung“ bezieht sich auf eine Authentifizierungsanforderung von:

- älteren Office-Clients, die keine moderne Authentifizierung verwenden (z.B. Office 2010-Client)
- jedem Client, der veraltete E-Mail-Protokolle wie IMAP/SMTP/POP3 verwendet

Heute stammt der Großteil aller gefährdenden Anmeldeversuche von Legacyauthentifizierungen. Die Legacyauthentifizierung unterstützt keine mehrstufige Authentifizierung (Multi-Factor Authentication, MFA). Auch wenn Sie eine MFA-Richtlinie für Ihr Verzeichnis aktiviert haben, kann ein böswilliger Benutzer sich mithilfe eines älteren Protokolls authentifizieren und MFA umgehen. Sobald Sicherheitsstandards in Ihrem Mandanten aktiviert sind, werden alle Authentifizierungsanforderungen blockiert, die über ein Legacyprotokoll an einen beliebigen Mandanten gerichtet werden. Exchange ActiveSync wird von Sicherheitsstandards nicht blockiert.

### <a name="protecting-privileged-actions"></a>Schützen privilegierter Aktionen

Organisationen verwenden eine Vielzahl von Azure-Diensten, die über die Azure Resource Manager-API verwaltet werden, wie z. B.:

- Azure-Portal 
- Azure PowerShell 
- Azure-Befehlszeilenschnittstelle

Benutzer, die Dienste mit Azure Resource Manager verwalten, verfügen über weitreichende Berechtigungen. Beispielsweise können mit Azure Resource Manager mandantenweite Konfigurationen wie Diensteinstellungen und die Abonnementabrechnung geändert werden. Die einstufige Authentifizierung ist für eine Vielzahl von Bedrohungen anfällig, beispielsweise für Phishing- und Kennwort-Spray-Angriffe. Aus diesem Grund muss die Identität von Benutzern, die auf Azure Resource Manager zugreifen und Konfigurationen aktualisieren möchten, unbedingt überprüft werden, indem der Zugriff erst nach einer Multi-Factor Authentication gewährt wird.

Sobald Sicherheitsstandards in Ihrem Mandanten aktiviert sind, muss jeder Benutzer, der auf das Azure-Portal, Azure PowerShell oder die Azure CLI zugreift, eine mehrstufige Authentifizierung durchlaufen. Diese Richtlinie gilt für alle Benutzer mit Zugriff auf Azure Resource Manager und nicht nur für Administratoren. Wenn der Benutzer nicht für die Multi-Factor Authentication registriert ist, muss er sich mithilfe der Microsoft Authenticator-App registrieren, um fortzufahren. Es gilt keine 14-tägige Frist für die MFA-Registrierung.

## <a name="deployment-considerations"></a>Überlegungen zur Bereitstellung

Es folgen zusätzliche Aspekte im Zusammenhang mit der Bereitstellung von Sicherheitsstandards für Ihren Mandanten.

### <a name="legacy-protocols"></a>Ältere Protokolle

Ältere Authentifizierungsprotokolle (IMAP, SMTP, POP3 usw.) werden von E-Mail-Clients für Authentifizierungsanforderungen verwendet. Diese Protokolle unterstützen keine Multi-Factor Authentication. Die meisten der von Microsoft festgestellten Kontosicherheitsverletzungen werden durch Angriffe verursacht, die sich gegen ältere Protokolle richten und versuchen, die MFA zu umgehen. Um sicherzustellen, dass MFA bei der Anmeldung mit einem Administratorkonto obligatorisch ist und von Angreifern nicht umgangen werden kann, blockieren Sicherheitsstandards alle Authentifizierungsanforderungen, die von Legacyprotokollen an Administratorkonten gerichtet werden.

> [!WARNING]
> Stellen Sie vor dem Aktivieren dieser Einstellung sicher, dass Ihre Administratoren keine Legacyauthentifizierungsprotokolle verwenden. Weitere Informationen finden Sie im Artikel zum  [Blockieren der Legacyauthentifizierung](concept-conditional-access-block-legacy-authentication.md).

### <a name="conditional-access"></a>Bedingter Zugriff

Bedingter Zugriff kann zur Konfiguration von Richtlinien dienen, die ein mit aktivierten Sicherheitsstandards vergleichbares Verhalten aufweisen. Wenn Sie mit bedingtem Zugriff arbeiten und in Ihrer Umgebung die Richtlinien für bedingten Zugriff aktiviert sind, stehen Ihnen Sicherheitsstandards nicht zur Verfügung. Wenn Sie eine Lizenz haben, die bedingten Zugriff bereitstellt, aber keine Richtlinien für bedingten Zugriff in Ihrer Umgebung aktiviert haben, können Sie Sicherheitsstandards solange nutzen, bis Sie Richtlinien für bedingten Zugriff aktivieren.

![Entweder Sicherheitsstandards oder bedingter Zugriff](./media/concept-conditional-access-security-defaults/security-defaults-conditional-access.png)

Hier finden Sie Schritt-für-Schritt-Anleitungen dazu, wie Sie mithilfe des bedingten Zugriffs äquivalente Richtlinien konfigurieren können:

- [Vorschreiben der MFA für Administratoren](howto-conditional-access-policy-admin-mfa.md)
- [Vorschreiben der MFA für die Azure-Verwaltung](howto-conditional-access-policy-azure-management.md)
- [Blockieren älterer Authentifizierungsmethoden](howto-conditional-access-policy-block-legacy.md)

## <a name="enabling-security-defaults"></a>Aktivieren von Sicherheitsstandards

So aktivieren Sie Sicherheitsstandards in Ihrem Verzeichnis

1. Melden Sie sich beim  [Azure-Portal](https://portal.azure.com)  als globaler Administrator, Sicherheitsadministrator oder Administrator für bedingten Zugriff an.
1. Navigieren Sie zu  **Azure Active Directory** > **Eigenschaften**.
1. Wählen Sie **Sicherheitsstandards verwalten** aus.
1. Legen Sie die Option **Sicherheitsstandards aktivieren** auf **Ja** fest.
1. Klicken Sie auf „Speichern“.

## <a name="next-steps"></a>Nächste Schritte

[Allgemeine Richtlinien für bedingten Zugriff](concept-conditional-access-policy-common.md)

[Was ist bedingter Zugriff?](overview.md)
