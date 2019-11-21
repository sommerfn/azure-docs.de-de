---
title: Azure Active Directory Identity Protection – Glossar
description: Azure Active Directory Identity Protection – Glossar
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: reference
ms.date: 10/18/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5439b8626d1fa0ea502d33455db8b2564fbd45f0
ms.sourcegitcommit: 7efb2a638153c22c93a5053c3c6db8b15d072949
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2019
ms.locfileid: "72886689"
---
# <a name="azure-active-directory-identity-protection-glossary"></a>Azure Active Directory Identity Protection – Glossar

### <a name="at-risk-user"></a>Gefährdet (Benutzer)
Ein Benutzer, der über mindestens eine aktive Risikoerkennung verfügt 

### <a name="atypical-sign-in-location"></a>Atypischer Anmeldeort
Eine Anmeldung von einem geografischen Standort aus, der für den jeweiligen Benutzer, ähnliche Benutzer oder den Mandanten untypisch ist.

### <a name="azure-ad-identity-protection"></a>Azure AD Identity Protection
Ein Sicherheitsmodul von Azure Active Directory, das eine umfassende Übersicht über Risikoerkennungen und potenzielle Sicherheitsrisiken bietet, die sich auf die Identitäten einer Organisation auswirken

### <a name="conditional-access"></a>Bedingter Zugriff
Eine Richtlinie zum Schützen des Zugriffs auf Ressourcen. Regeln für den bedingten Zugriff werden in Azure Active Directory gespeichert und von Azure AD ausgewertet, bevor Zugriff auf die Ressource gewährt wird.  Beispiele für Regeln sind die Einschränkung des Zugriffs basierend auf dem Benutzerstandort, der Integrität des Geräts oder des Verfahrens für die Benutzerauthentifizierung.

### <a name="credentials"></a>Anmeldeinformationen
Die sind die Informationen zur Identifizierung mit Identifikationsnachweis. Sie werden verwendet, um Zugriff auf lokale Ressourcen und Netzwerkressourcen zu erhalten. Beispiele für Anmeldeinformationen sind Benutzernamen und Kennwörter, Smartcards und Zertifikate.

### <a name="event"></a>Ereignis
Ein Datensatz einer Aktivität in Azure Active Directory.

### <a name="false-positive-risk-detection"></a>Falsch positives Ergebnis (Risikoerkennung)
Ein Risikoerkennungsstatus, der von einem Identity Protection-Benutzer manuell festgelegt wird. Er gibt an, dass die Risikoerkennung untersucht und dabei ermittelt wurde, dass sie fälschlicherweise als Risikoerkennung eingestuft wurde.

### <a name="identity"></a>Identity
Eine Person oder Identität, die per Authentifizierung anhand von Kriterien, z. B. einem Kennwort oder Zertifikat, überprüft werden muss.

### <a name="identity-risk-detection"></a>Identitätsrisikoerkennung
AAD-Ereignis, das von Identity Protection als anomal gekennzeichnet wurde und mit dem angegeben werden kann, dass eine Identität kompromittiert wurde.

### <a name="ignored-risk-detection"></a>Ignoriert (Risikoerkennung)
Ein Risikoerkennungsstatus, der von einem Identity Protection-Benutzer manuell festgelegt wurde und angibt, dass die Risikoerkennung geschlossen wurde, ohne dass Maßnahmen ergriffen wurden.

### <a name="impossible-travel-from-atypical-locations"></a>Unmöglicher Ortswechsel zu atypischen Orten
Diese Risikoerkennung wird ausgelöst, wenn zwei Anmeldungen für denselben Benutzer erkannt werden und Folgendes gilt: Mindestens eine davon stammt von einem atypischen Anmeldestandort, und der Zeitraum zwischen den Anmeldungen ist kürzer als der Mindestzeitraum, der für die physische Bewegung zwischen diesen Orten erforderlich ist.  

### <a name="investigation"></a>Untersuchung
Der Prozess der Überprüfung von Aktivitäten, Protokollen und anderen relevanten Informationen zu einer Risikoerkennung, um zu entscheiden, ob Schritte zur Korrektur oder Lösung erforderlich sind. Außerdem wird ermittelt, ob und wie die Identität kompromittiert und wie die kompromittierte Identität verwendet wurde.

### <a name="leaked-credentials"></a>Kompromittierte Anmeldeinformationen
Diese Risikoerkennung wird ausgelöst, wenn unsere Ermittler herausfinden, dass die aktuellen Benutzeranmeldeinformationen (Benutzername und Kennwort) öffentlich im Dark Web gepostet werden.

### <a name="mitigation"></a>Lösung
Eine Aktion, mit der für einen Angreifer die Möglichkeit eingeschränkt oder beseitigt wird, eine kompromittierte Identität oder ein Gerät auszunutzen, ohne für die Identität oder das Gerät wieder einen sicheren Zustand herzustellen. Bei einer Lösung werden keine vorherigen Risikoerkennungen behoben, die der Identität oder dem Gerät zugeordnet sind.

### <a name="multi-factor-authentication"></a>Multi-Factor Authentication
Ein Authentifizierungsverfahren, für das mindestens zwei Authentifizierungsmethoden erforderlich sind: etwas, was sich im Besitz des Benutzers befindet, z. B. ein Zertifikat, etwas, was der Benutzer weiß, z. B. Benutzernamen, Kennwörter oder Passphrases, physische Attribute, z. B. einen Fingerabdruck, und persönliche Attribute, z. B. eine persönliche Signatur.

### <a name="offline-detection"></a>Offlineerkennung
Die Erkennung von Anomalien und Bewertung des Risikos eines Ereignisses, z. B. ein verspäteter Anmeldeversuch für eine Veranstaltung, nachdem diese bereits vorüber ist.

### <a name="policy-condition"></a>Richtlinienbedingung
Ein Teil einer Sicherheitsrichtlinie, mit der die Entitäten (Gruppen, Benutzer, Apps, Geräteplattformen, Gerätezustände, IP-Bereiche, Clienttypen) definiert werden, die in die Richtlinie eingeschlossen oder davon ausgeschlossen werden.

### <a name="policy-rule"></a>Richtlinienregel
Der Teil einer Sicherheitsrichtlinie, mit der die Umstände beschrieben werden, unter denen die Richtlinie ausgelöst wird, sowie die bei Auslösung der Richtlinie auszuführenden Aktionen.

### <a name="prevention"></a>Prävention
Eine Aktion, mit der Schaden für die Organisation verhindert werden soll, der durch den Missbrauch einer vermutlich oder mit Sicherheit kompromittierten Identität oder eines Geräts entsteht. Mit einer Präventionsaktion wird das Gerät bzw. die Identität nicht geschützt, und vorherige Risikoerkennungen werden nicht gelöst.

### <a name="privileged-user"></a>Privilegiert (Benutzer)
Ein Benutzer, der zum Zeitpunkt einer Risikoerkennung über dauerhafte oder vorübergehende Administratorberechtigungen für eine oder mehrere Ressourcen in Azure Active Directory verfügt (beispielsweise ein globaler Administrator, Abrechnungsadministrator, Dienstadministrator, Benutzeradministrator oder Kennwortadministrator) 

### <a name="real-time"></a>Echtzeit
Siehe „Echtzeiterkennung“.

### <a name="real-time-detection"></a>Echtzeiterkennung
Die Erkennung von Anomalien und Bewertung des Risikos eines Ereignisses, z. B. ein Anmeldeversuch vor der Freigabe bzw. dem Start einer Veranstaltung.

### <a name="remediated-risk-detection"></a>Bereinigt (Risikoerkennung)
Ein Risikoerkennungsstatus, der in Identity Protection automatisch festgelegt wird und angibt, dass die Risikoerkennung mit der standardmäßigen Korrekturaktion für diese Art von Risikoerkennung bereinigt wurde. Beim Zurücksetzen des Benutzerkennworts werden beispielsweise viele Risikoerkennungen, die auf eine Kompromittierung des vorherigen Kennworts hinweisen, automatisch bereinigt.

### <a name="remediation"></a>Wiederherstellung
Eine Aktion zum Schützen einer Identität oder eines Geräts, die bzw. das zuvor wahrscheinlich oder mit Sicherheit kompromittiert war. Bei einer Korrekturaktion wird die Identität bzw. das Gerät wieder in einen sicheren Zustand versetzt, und vorherige Risikoerkennungen, die der Identität oder dem Gerät zugeordnet sind, werden gelöst.

### <a name="resolved-risk-detection"></a>Gelöst (Risikoerkennung)
Ein Risikoerkennungsstatus, der von einem Identity Protection-Benutzer manuell festgelegt wird. Hiermit wird angegeben, dass der Benutzer außerhalb von Identity Protection eine geeignete Korrekturaktion durchgeführt hat und dass die Risikoerkennung als geschlossen angesehen werden kann.

### <a name="risk-detection-status"></a>Risikoerkennungsstatus
Eine Eigenschaft einer Risikoerkennung, mit der angegeben wird, ob das Ereignis aktiv ist. Wenn es geschlossen wurde, wird der Grund für das Schließen angegeben.

### <a name="risk-detection-type"></a>Risikoerkennungstyp
Eine Kategorie für die Risikoerkennung, mit der die Art der Anomalie angegeben wird, die zur Risikoeinstufung für das Ereignis geführt hat

### <a name="risk-level-risk-detection"></a>Risikostufe (Risikoerkennung)
Eine Einstufung (Hoch, Mittel oder Niedrig) des Schweregrads einer Risikoerkennung. So können Identity Protection-Benutzer die Aktionen priorisieren, die sie durchführen, um das Risiko für ihre Organisation zu mindern. 

### <a name="risk-level-sign-in"></a>Risikostufe (Anmeldung)
Ein Einstufung (Hoch, Mittel oder Niedrig) der Wahrscheinlichkeit, dass bei einer bestimmten Anmeldung eine andere Person versucht, die Identität des Benutzers zu nutzen.

### <a name="risk-level-user-compromise"></a>Risikostufe (Benutzerkompromittierung)
Eine Einstufung (Hoch, Mittel oder Niedrig) der Wahrscheinlichkeit, mit der eine Identität kompromittiert wurde.

### <a name="risk-level-vulnerability"></a>Risikostufe (Sicherheitsrisiko)
Eine Einstufung (Hoch, Mittel oder Niedrig) des Sicherheitsrisikos. Hiermit können Identity Protection-Benutzer die Aktionen priorisieren, die sie durchführen, um das Risiko für ihre Organisation zu mindern.

### <a name="secure-identity"></a>Schützen (Identität)
Das Durchführen einer Korrekturaktion, z. B. das Ändern eines Kennworts oder das Verwenden eines neuen Image für einen Computer, um für eine potenziell kompromittierte Identität wieder einen fehlerlosen Zustand herzustellen.

### <a name="security-policy"></a>Sicherheitsrichtlinie
Eine Auflistung von Regeln und Bedingungen einer Richtlinie. Eine Richtlinie kann auf Entitäten angewendet werden, z. B. Benutzer, Gruppen, Apps, Geräte, Geräteplattformen, Gerätestatus, IP-Bereiche und Auth2.0-Clienttypen. Wenn eine Richtlinie aktiviert ist, wird sie immer dann ausgewertet, wenn für eine in der Richtlinie enthaltene Entität ein Token für eine Ressource ausgegeben wird.

### <a name="sign-in-v"></a>Anmelden
Das Authentifizieren mit einer Identität in Azure Active Directory.

### <a name="sign-in-n"></a>Anmeldung
Der Prozess bzw. die Aktion zur Authentifizierung einer Identität in Azure Active Directory und das Ereignis, mit dem dieser Vorgang erfasst wird.

### <a name="sign-in-from-anonymous-ip-address"></a>Anmeldung von anonymer IP-Adresse
Eine Risikoerkennung, die nach einer erfolgreichen Anmeldung über eine IP-Adresse ausgelöst wird, die als anonyme Proxy-IP-Adresse identifiziert wurde.

### <a name="sign-in-from-infected-device"></a>Anmeldung von infiziertem Gerät
Diese Risikoerkennung wird ausgelöst, wenn eine Anmeldung von einer IP-Adresse stammt, die bekanntermaßen von einem oder mehreren kompromittierten Geräten verwendet wird, und die Geräte aktiv versuchen, mit einem Botserver zu kommunizieren.

### <a name="sign-in-from-ip-address-with-suspicious-activity"></a>Anmeldung von IP-Adresse mit verdächtiger Aktivität
Diese Risikoerkennung wird ausgelöst, nachdem eine erfolgreiche Anmeldung von einer IP-Adresse mit einer hohen Zahl von fehlerhaften Anmeldeversuchen unter Verwendung mehrerer Benutzerkonten innerhalb eines kurzen Zeitraums durchgeführt wurde.

### <a name="sign-in-from-unfamiliar-location"></a>Anmeldung von unbekanntem Standort
Diese Risikoerkennung wird ausgelöst, wenn sich ein Benutzer erfolgreich von einem neuen Standort (IP-Adresse, Breiten-/Längengrad und ASN) aus anmeldet.

### <a name="sign-in-risk"></a>Anmelderisiko
Siehe „Risikostufe (Anmeldung)“.

### <a name="sign-in-risk-policy"></a>Richtlinie zum Anmelderisiko
Eine bedingte Zugriffsrichtlinie, mit der das Risiko für eine bestimmte Anmeldung ausgewertet wird und Lösungen basierend auf vordefinierten Bedingungen und Regeln angewendet werden.

### <a name="user-compromise-risk"></a>Risiko einer Benutzerkompromittierung
Siehe „Risikostufe (Benutzerkompromittierung)“.

### <a name="user-risk"></a>Benutzerrisiko
Siehe „Risikostufe (Benutzerkompromittierung)“.

### <a name="user-risk-policy"></a>Richtlinie zum Benutzerrisiko
Eine bedingte Zugriffsrichtlinie, mit der die Anmeldung berücksichtigt wird und Lösungen basierend auf vordefinierten Bedingungen und Regeln angewendet werden.

### <a name="users-flagged-for-risk"></a>Benutzer mit Risikokennzeichnung
Benutzer, die über Risikoerkennungen verfügen (entweder „Aktiv“ oder „Bereinigt“)

### <a name="vulnerability"></a>Sicherheitsrisiko
Eine Konfiguration oder ein Zustand in Azure Active Directory, durch die bzw. den das Verzeichnis anfällig für Angriffe oder Bedrohungen wird.

## <a name="see-also"></a>Weitere Informationen

- [Azure Active Directory Identity Protection](../active-directory-identityprotection.md)
