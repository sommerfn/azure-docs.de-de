---
title: Was bedeutet Risiko? Azure AD Identity Protection
description: Erläutern von Risikoereignissen in Azure AD Identity Protection
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
ms.openlocfilehash: 7fd0b562ae0570917c9127e301964d089d49b087
ms.sourcegitcommit: 7efb2a638153c22c93a5053c3c6db8b15d072949
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2019
ms.locfileid: "72886721"
---
# <a name="what-is-risk"></a>Was bedeutet Risiko?

Risikoerkennungen in Azure AD Identity Protection umfassen alle identifizierten verdächtigen Aktionen im Zusammenhang mit Benutzerkonten im Verzeichnis.

Identity Protection bietet Organisationen Zugriff auf leistungsstarke Ressourcen, um diese verdächtigen Aktionen zu erkennen und schnell darauf zu reagieren. 

![Sicherheitsübersicht riskanter Benutzer und Anmeldungen](./media/concept-identity-protection-risks/identity-protection-security-overview.png)

## <a name="risk-types-and-detection"></a>Risikotypen und Erkennung

Es gibt zwei Arten von Risiken (**Benutzerrisiken** und **Anmelderisiken**) und zwei Arten der Erkennung oder Berechnung (**Echtzeit** und **Offline**).

### <a name="user-risk"></a>Benutzerrisiko

Ein Benutzerrisiko stellt die Wahrscheinlichkeit dar, dass eine bestimmte Identität oder ein bestimmtes Konto kompromittiert wurde. 

Diese Risiken werden offline anhand interner und externer Threat Intelligence-Quellen von Microsoft berechnet, z.B. Sicherheitsexperten, Strafverfolgungsbehörden, Sicherheitsteams bei Microsoft und anderen vertrauenswürdigen Quellen.

| Risikoerkennung | BESCHREIBUNG |
| --- | --- |
| Kompromittierte Anmeldeinformationen | Dieser Risikoerkennungstyp gibt an, dass die gültigen Anmeldeinformationen des Benutzers kompromittiert wurden. Wenn Internetkriminelle an gültige Kennwörter von berechtigten Benutzern gelangen, geben sie diese Anmeldeinformationen häufig weiter. Diese Freigabe erfolgt in der Regel durch eine Veröffentlichung im Darknet oder auf Paste Sites oder durch den Handel und Verkauf der Anmeldeinformationen auf dem Schwarzmarkt. Wenn der Microsoft-Dienst für kompromittierte Anmeldeinformationen Benutzeranmeldeinformationen aus dem Darknet, von Paste Sites oder anderen Quellen erwirbt, werden sie mit aktuell gültigen Anmeldeinformationen von Azure AD-Benutzern verglichen, um gültige Übereinstimmungen zu finden. |
| Azure AD Threat Intelligence | Dieser Risikoerkennungstyp gibt Benutzeraktivitäten an, die für den angegebenen Benutzer unüblich oder mit bekannten Angriffsmustern konsistent sind (basierend auf internen und externen Threat Intelligence-Quellen von Microsoft). |

### <a name="sign-in-risk"></a>Anmelderisiko

Ein Anmelderisiko stellt die Wahrscheinlichkeit dar, dass eine bestimmte Authentifizierungsanforderung vom Identitätsbesitzer nicht autorisiert wurde. 

Diese Risiken können in Echtzeit oder offline anhand interner und externer Threat Intelligence-Quellen von Microsoft berechnet werden, z.B. Sicherheitsexperten, Strafverfolgungsbehörden, Sicherheitsteams bei Microsoft und anderen vertrauenswürdigen Quellen.

| Risikoerkennung | Erkennungstyp | BESCHREIBUNG |
| --- | --- | --- |
| Anonyme IP-Adresse | Echtzeit | Dieser Risikoerkennungstyp gibt Anmeldungen über eine anonyme IP-Adresse (z.B. Tor-Browser oder ein anonymisiertes VPN) an. Diese IP-Adressen werden in der Regel von Akteuren verwendet, die ihre Anmeldungstelemetriedaten (IP-Adresse, Standort, Gerät usw.) für potenziell böswillige Absichten verbergen möchten. |
| Ungewöhnlicher Ortswechsel | Offline | Mit diesem Risikoerkennungstyp werden zwei Anmeldungen identifiziert, die von weit entfernten Orten durchgeführt wurden und bei denen mindestens einer der Orte aufgrund des bisherigen Verhaltens atypisch für den Benutzer ist. Neben zahlreichen anderen Faktoren berücksichtigt dieser Machine Learning-Algorithmus die Zeit zwischen den beiden Anmeldungen sowie die Zeit, die der Benutzer für einen Ortswechsel benötigen würde, wovon sich ableiten lässt, dass ein anderer Benutzer die gleichen Anmeldeinformationen verwendet. <br><br> Bei diesem Algorithmus werden offensichtliche falsch positive Ergebnisse ignoriert, die zu den unmöglichen Ortswechselbedingungen beitragen. Hierzu zählen etwa VPNs und regelmäßig von anderen Benutzern der Organisation verwendete Standorte. Das System verfügt über einen anfänglichen Lernzeitraum von 14 Tagen oder 10 Anmeldungen (je nachdem, welcher Punkt früher erreicht wird), in dem das Anmeldeverhalten des neuen Benutzers erlernt wird. |
| Mit Schadsoftware verknüpfte IP-Adresse | Offline | Mit diesem Risikoerkennungstyp werden Anmeldungen von IP-Adressen identifiziert, die mit Schadsoftware infiziert sind und bekanntermaßen aktiv mit einem Botserver kommunizieren. Diese Erkennung wird ermittelt, indem IP-Adressen des Benutzergeräts mit IP-Adressen korreliert werden, die in Kontakt mit einem Botserver gestanden haben, während der Botserver aktiv war. |
| Ungewöhnliche Anmeldeeigenschaften | Echtzeit | Dieser Risikoerkennungstyp berücksichtigt den bisherigen Anmeldeverlauf (IP-Adresse, Breitengrad/Längengrad und ASN), um nach anomalen Anmeldungen zu suchen. Im System werden Informationen zu den vorherigen Standorten gespeichert, die von einem Benutzer genutzt wurden, und diese werden als „vertraute“ Standorte angesehen. Die Risikoerkennung wird ausgelöst, wenn die Anmeldung von einem Standort aus erfolgt, der in der Liste der bekannten Standorte noch nicht enthalten ist. Neu erstellte Benutzer befinden sich für einen bestimmten Zeitraum im „Lernmodus“, in dem ungewohnte Anmeldeeigenschaften-Risikoerkennungen deaktiviert werden, während unsere Algorithmen das Verhalten des Benutzers lernen. Die Dauer des Lernmodus ist dynamisch und hängt davon ab, wie lange es dauert, bis der Algorithmus genügend Informationen über die Anmeldemuster des Benutzers gesammelt hat. Die Mindestdauer beträgt fünf Tage. Ein Benutzer kann nach einer langen Zeit der Inaktivität erneut in den Lernmodus wechseln. Außerdem ignoriert das System Anmeldungen von vertrauten Geräten und von Standorten aus, die geografisch nahe an einem bekannten Speicherort liegen. <br><br> Diese Erkennung wird auch für die Standardauthentifizierung (bzw. ältere Protokolle) ausgeführt. Da diese Protokolle nicht über moderne Eigenschaften (z. B. Client-ID) verfügen, reichen die Telemetriedaten nicht aus, um falsch positive Ergebnisse zu reduzieren. Wir empfehlen unseren Kunden, auf eine moderne Authentifizierung umzusteigen. |
| Benutzergefährdung durch Administrator bestätigt | Offline | Diese Erkennung gibt an, dass ein Administrator auf der Benutzeroberfläche für riskante Benutzer oder mithilfe der riskyUsers-API die Option „Benutzergefährdung bestätigen“ ausgewählt hat. Überprüfen Sie den Risikoverlauf des Benutzers (auf der Benutzeroberfläche oder über die API), um zu überprüfen, welcher Administrator diese Benutzergefährdung bestätigt hat. |
| Schädliche IP-Adresse | Offline | Diese Erkennung gibt eine Anmeldung über eine schädliche IP-Adresse an. Eine IP-Adresse wird wegen hoher Fehlerraten aufgrund von ungültige Anmeldeinformationen, die von der IP-Adresse oder anderen IP-Zuverlässigkeitsquellen empfangen wurden, als schädlich eingestuft. |

### <a name="other-risk-detections"></a>Andere Risikoerkennungen

| Risikoerkennung | Erkennungstyp | BESCHREIBUNG |
| --- | --- | --- |
| Zusätzliches Risiko erkannt | Echtzeit oder offline | Diese Erkennung gibt an, dass eine der oben genannten Premium-Erkennungen erkannt wurde. Da die Premium-Erkennungen nur für Azure AD Premium P2-Kunden sichtbar sind, werden sie für Kunden ohne Azure AD Premium P2-Lizenzen mit der Bezeichnung „Zusätzliches Risiko erkannt“ angezeigt. |

## <a name="next-steps"></a>Nächste Schritte

- [Verfügbare Richtlinien zum Mindern von Risiken](concept-identity-protection-policies.md)

- [Sicherheitsübersicht](concept-identity-protection-security-overview.md)
