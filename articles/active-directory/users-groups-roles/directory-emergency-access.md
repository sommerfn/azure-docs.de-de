---
title: 'Verwalten von Administratorkonten für den Notfallzugriff: Azure Active Directory | Microsoft-Dokumentation'
description: In diesem Artikel wird beschrieben, wie Sie mithilfe von Konten für den Notfallzugriff verhindern können, versehentlich aus Ihrem Azure Active Directory-Mandanten (Azure AD-Mandanten) ausgeschlossen zu werden.
services: active-directory
author: markwahl-msft
ms.author: curtand
ms.date: 03/19/2019
ms.topic: conceptual
ms.service: active-directory
ms.subservice: users-groups-roles
ms.workload: identity
ms.custom: it-pro
ms.reviewer: markwahl-msft
ms.collection: M365-identity-device-management
ms.openlocfilehash: 42de060d81539030ef1970e01e753383662e924f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "67083910"
---
# <a name="manage-emergency-access-accounts-in-azure-ad"></a>Verwalten von Konten für den Notfallzugriff in Azure AD

Es ist wichtig, dass Sie verhindern, versehentlich aus Ihrem Azure Active Directory-Mandanten ausgeschlossen zu werden, weil Sie sich nicht anmelden oder ein Konto eines vorhandenen einzelnen Benutzers nicht als Administrator aktivieren können. Sie können die Auswirkungen eines versehentlichen Verlusts des Administratorzugriffs abmildern, indem Sie mindestens zwei *Konten für den Notfallzugriff* in Ihrem Mandanten erstellen.

Konten für den Notfallzugriff verfügen über umfangreiche Berechtigungen und werden keinen Einzelpersonen zugewiesen. Konten für den Notfallzugriff sind auf Notfallsituationen oder Szenarien beschränkt, in denen normale Administratorkonten nicht verwendet werden können. Organisationen müssen sicherstellen, dass die Verwendung der Notfallkonten ausschließlich auf Fälle beschränkt wird, in denen dies absolut notwendig ist.

Dieser Artikel enthält Richtlinien zum Verwalten von Konten für den Notfallzugriff in Azure AD.

## <a name="when-would-you-use-an-emergency-access-account"></a>In welchen Fällen würden Sie ein Konto für den Notfallzugriff verwenden?

Eine Organisation könnte beispielsweise in den folgenden Situationen auf ein Konto für den Notfallzugriff zurückgreifen:

- Die Benutzerkonten befinden sich in einem Verbund, und der Verbund ist aktuell nicht verfügbar, weil ein Mobilfunknetz oder ein Identitätsanbieter ausgefallen ist. Wenn beispielsweise der Identitätsanbieterhost in Ihrer Umgebung nicht erreichbar ist, können sich die Benutzer möglicherweise nicht anmelden, wenn sie von Azure AD an ihren Identitätsanbieter umgeleitet werden.
- Die Administratoren sind über Azure Multi-Factor Authentication registriert, und keines der von ihnen verwendeten Geräte ist verfügbar, oder der Dienst ist nicht verfügbar. Benutzer können möglicherweise keine mehrstufige Authentifizierung durchführen, um eine Rolle zu aktivieren. Beispielsweise können bei einem Ausfall des Mobilfunknetzes keine Anrufe entgegengenommen oder SMS empfangen werden, womit die einzigen registrierten Authentifizierungsmechanismen für ihr Gerät wegfallen.
- Die Person, die zuletzt über globalen Administratorzugriff verfügte, hat die Organisation verlassen. Azure AD verhindert, dass das letzte globale Administratorkonto gelöscht wird, aber das lokale Löschen oder Deaktivieren des Kontos wird nicht verhindert. Beide Situationen können dazu führen, dass die Organisation nicht in der Lage ist, das Konto wiederherzustellen.
- Bei unvorhersehbaren Ereignissen wie Naturkatastrophen, die dazu führen, dass Mobiltelefone oder andere Netzwerke nicht verfügbar sind. 

## <a name="create-two-cloud-based-emergency-access-accounts"></a>Erstellen von zwei cloudbasierten Konten für den Notfallzugriff

Erstellen Sie mindestens zwei Konten für den Notfallzugriff. Bei diesen Konten sollte es sich um reine Cloudkonten handeln, die die Domäne „\*.onmicrosoft.com“ verwenden und keine Verbundkonten oder Konten darstellen, die über eine lokale Umgebung synchronisiert werden.

Beim Konfigurieren dieser Konten müssen die folgenden Anforderungen erfüllt werden:

- Die Konten für den Notfallzugriff dürfen keinem Einzelbenutzer in der Organisation zugeordnet werden. Stellen Sie sicher, dass Ihre Konten nicht mit Mobiltelefonen von Mitarbeitern, Hardwaretoken einzelner Mitarbeiter oder anderen mitarbeiterspezifischen Anmeldeinformationen verbunden sind. Durch diese Vorsichtsmaßnahme werden Fälle abgedeckt, in denen einzelne Mitarbeiter nicht erreichbar sind, wenn die Anmeldeinformationen benötigt werden. Es muss unbedingt sichergestellt werden, dass alle registrierten Geräte an einem bekannten, sicheren Ort aufbewahrt werden, die über verschiedene Wege mit Azure AD kommunizieren können.
- Der für ein Konto für den Notfallzugriff verwendete Authentifizierungsmechanismus sollte sich von dem unterscheiden, der für Ihre anderen Administratorkonten einschließlich anderer Konten für den Notfallzugriff verwendet wird.  Erfolgt beispielsweise die normale Administratoranmeldung über lokale MFA, würde Azure MFA einen anderen Mechanismus darstellen.  Wenn jedoch Azure MFA Ihr primäres Authentifizierungselement für Ihre Administratorkonten ist, sollten Sie einen anderen Ansatz für diese Konten in Betracht ziehen, z.B. die Verwendung von bedingtem Zugriff mit einem MFA-Drittanbieter.
- Die jeweiligen Geräte oder die Anmeldeinformationen dürfen nicht ablaufen oder aufgrund mangelnder Verwendung in den Bereich der automatisierten Bereinigung fallen.  
- Sie sollten die Rolle „Globaler Administrator“ für Ihre Konten für den Notfallzugriff dauerhaft zuweisen. 


### <a name="exclude-at-least-one-account-from-phone-based-multi-factor-authentication"></a>Ausschließen mindestens eines Kontos aus der telefonbasierten mehrstufigen Authentifizierung

Um das Risiko von Angriffen zu verringern, die aus kompromittierten Kennwörtern resultieren, empfiehlt das Azure AD-Team, die mehrstufige Authentifizierung (Multi-Factor Authentication, MFA) für alle individuellen Benutzer als verbindlich festzulegen. Diese Gruppe umfasst Administratoren und alle weiteren Benutzer (z. B. Mitarbeiter der Finanzabteilung), bei denen ein kompromittiertes Konto erhebliche Auswirkungen haben kann.

Für mindestens eines Ihrer Konten für den Notfallzugriff sollte jedoch nicht der gleiche MFA-Mechanismus wie für Ihre anderen (normalen) Konten verwendet werden. Dies schließt Drittanbieterlösungen für die mehrstufige Authentifizierung ein. Wenn Sie eine Richtlinie für bedingten Zugriff festgelegt haben, um [Multi-Factor Authentication für alle Administratoren](../authentication/howto-mfa-userstates.md) für Azure AD und andere verbundene Software-as-a-Service-Apps (SaaS-Apps) zu erzwingen, sollten Sie die Konten für den Notfallzugriff aus dieser Anforderung ausschließen und stattdessen einen anderen Mechanismus konfigurieren. Darüber hinaus sollten Sie sicherstellen, dass für die Konten keine Richtlinie für die mehrstufige Authentifizierung pro Benutzer festgelegt ist.

### <a name="exclude-at-least-one-account-from-conditional-access-policies"></a>Ausschließen mindestens eines Kontos aus Richtlinien für bedingten Zugriff

In einem Notfall soll eine Richtlinie Ihren Zugriff nicht potenziell blockieren, um ein Problem zu beheben. Mindestens ein Konto für den Notfallzugriff sollte aus allen Richtlinien für bedingten Zugriff ausgeschlossen werden. Wenn Sie eine [Basisrichtlinie](../conditional-access/baseline-protection.md) aktiviert haben, sollten Sie Ihre Konten für den Notfallzugriff daraus ausschließen.

## <a name="additional-guidance-for-hybrid-customers"></a>Zusätzliche Empfehlungen für Kunden mit Hybridlösungen

Eine weitere Option für Organisationen, die Active Directory Domain Services und AD FS oder einen ähnlichen Identitätsanbieter für den Verbund mit Azure AD verwenden, ist das Konfigurieren eines Kontos für den Notfallzugriff, dessen MFA-Anspruch von diesem Identitätsanbieter angegeben werden könnte.  Das Konto für den Notfallzugriff könnte z. B. durch ein Zertifikat und ein Schlüsselpaar gesichert werden, wie beispielsweise eines, das auf einer Smartcard gespeichert ist.  Wenn der Benutzer für Azure Active Directory authentifiziert wird, kann AD FS einen Anspruch für Azure AD angeben, der darauf hinweist, dass der Benutzer die MFA-Anforderungen erfüllt.  Auch bei diesem Ansatz müssen Organisationen für den Fall, dass kein Verbund hergestellt werden kann, weiterhin über cloudbasierte Konten für den Notfallzugriff verfügen. 

## <a name="store-devices-and-credentials-in-a-safe-location"></a>Aufbewahren von Geräten und Anmeldeinformationen an einem sicheren Ort

Organisationen müssen sicherstellen, dass die Anmeldeinformationen für Konten für den Notfallzugriff sicher aufbewahrt werden und nur den Personen bekannt sind, die für deren Verwendung autorisiert sind. Einige Kunden verwenden eine Smartcard, und andere verwenden Kennwörter. Ein Kennwort für ein Konto für den Notfallzugriff wird in der Regel in zwei oder drei Teile unterteilt, die separat auf Papier festgehalten und in geschützten, feuerfesten Tresoren hinterlegt werden, die sich an sicheren, getrennten Standorten befinden.

Stellen Sie bei Verwendung von Kennwörtern sicher, dass die Konten über sichere Kennwörter verfügen, die nicht ablaufen. Im Idealfall sollten die Kennwörter mindestens 16 Zeichen umfassen und zufällig generiert werden.


## <a name="monitor-sign-in-and-audit-logs"></a>Überwachen von Anmeldungs- und Überwachungsprotokollen

Überwachen Sie die [Azure AD-Anmeldungs- und Überwachungsprotokolle](../reports-monitoring/concept-sign-ins.md) auf Anmeldungen, und überwachen Sie die Aktivität der Konten für den Notfallzugriff. Normalerweise sollte über diese Konten keine Anmeldungen und keine Änderungen erfolgen, deshalb ist ihre Verwendung verdächtig und bedarf einer Sicherheitsuntersuchung.

## <a name="validate-accounts-at-regular-intervals"></a>Regelmäßiges Überprüfen der Konten

Führen Sie mindestens die folgenden Schritte in regelmäßigen Abständen aus, um die Mitarbeiter in der Verwendung und Überprüfung von Konten für den Notfallzugriff zu schulen:

- Stellen Sie sicher, dass die für die Sicherheitsüberwachung verantwortlichen Mitarbeiter darüber informiert sind, dass eine Kontoüberprüfung stattfindet.
- Stellen Sie sicher, dass der Notfallprozess zur Verwendung dieser Konten dokumentiert und aktuell ist.
- Stellen Sie sicher, dass Administratoren und Sicherheitsbeauftragte, die diese Schritte bei einem Notfall möglicherweise ausführen müssen, entsprechend geschult sind.
- Aktualisieren Sie die Kontoanmeldeinformationen (insbesondere alle Kennwörter) für Ihre Konten für den Notfallzugriff, und überprüfen Sie dann, ob Sie sich mit den Konten für den Notfallzugriff anmelden und damit administrative Aufgaben ausführen können.
- Stellen Sie sicher, dass die Benutzer keine Multi-Factor Authentication oder Self-Service-Kennwortzurücksetzung mit persönlichen Geräten oder Informationen registriert haben. 
- Wenn die Konten für Multi-Factor Authentication mit einem Gerät zur Verwendung während der Anmeldung oder Rollenaktivierung registriert sind, stellen Sie sicher, dass das Gerät für alle Administratoren zugänglich ist, die es bei einem Notfall möglicherweise verwenden müssen. Stellen Sie außerdem sicher, dass das Gerät über mindestens zwei Netzwerkpfade kommunizieren kann, die nicht dasselbe Ausfallverhalten zeigen. Beispiel: Das Gerät kann über ein Drahtlosnetzwerk der Organisation und über das Netz eines Mobilfunkanbieters mit dem Internet kommunizieren.

Diese Schritte sollten in regelmäßigen Abständen und für wichtige Änderungen ausgeführt werden:

- Mindestens alle 90 Tage
- Bei einem Wechsel bei den IT-Mitarbeitern, z. B. bei einem Positionswechsel, beim Ausscheiden eines Mitarbeiters oder bei einer Neueinstellung
- Bei einer Änderung der Azure AD-Abonnements in der Organisation

## <a name="next-steps"></a>Nächste Schritte

- [Schützen des privilegierten Zugriffs für hybride und Cloudbereitstellungen in Azure AD](directory-admin-roles-secure.md)
- [Hinzufügen von Benutzern in Azure AD](../fundamentals/add-users-azure-active-directory.md) und [Zuweisen der Rolle „Globaler Administrator“ zum neuen Benutzer](../fundamentals/active-directory-users-assign-role-azure-portal.md)
- [Registrieren für Azure Active Directory Premium](../fundamentals/active-directory-get-started-premium.md), sofern nicht bereits geschehen
- [Vorgehensweise zum Erzwingen einer zweistufigen Überprüfung für einen Benutzer](../authentication/howto-mfa-userstates.md)
- [Konfigurieren zusätzlicher Schutzmechanismen für globale Administratoren in Office 365](https://docs.microsoft.com/office365/enterprise/protect-your-global-administrator-accounts) bei Verwendung von Office 365
- [Starten einer Zugriffsüberprüfung für globale Administratoren](../privileged-identity-management/pim-how-to-start-security-review.md) und [Umstellen der vorhandenen globalen Administratoren auf spezifischere Administratorrollen](directory-assign-admin-roles.md)
