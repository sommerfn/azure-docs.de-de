---
title: Was ist Azure Active Directory Identity Protection?
description: Erkennen, Behandeln, Untersuchen und Analysieren von Risiken mit Azure AD Identity Protection
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: overview
ms.date: 10/18/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7e928d67ba7102df3d342e77705ea895f9230ff3
ms.sourcegitcommit: 7efb2a638153c22c93a5053c3c6db8b15d072949
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2019
ms.locfileid: "72887700"
---
# <a name="what-is-azure-active-directory-identity-protection"></a>Was ist Azure Active Directory Identity Protection?

Identity Protection ist ein Tool, mit dem Organisationen drei wichtige Aufgaben erledigen können:

- Automatisieren der Erkennung und Behandlung identitätsbasierter Risiken
- Untersuchen von Risiken unter Verwendung von Daten im Portal
- Exportieren von Risikoerkennungsdaten in Hilfsprogramme von Drittanbietern zur weiteren Analyse

Identity Protection nutzt die Erkenntnisse, die Microsoft dank seiner Position in Organisationen (Azure AD), im Verbraucherbereich (Microsoft-Konten) und im Spielebereich (Xbox) gewonnen hat, um Ihre Benutzer zu schützen. Microsoft analysiert 6,5 Billionen Signale pro Tag, um Bedrohungen zu erkennen und Kunden vor Bedrohungen zu schützen.

Die Signale, die von Identity Protection generiert und Identity Protection zugeführt werden, können auch im Rahmen des bedingten Zugriffs genutzt werden, um Zugriffsentscheidungen zu treffen. Darüber hinaus können die Signale zur weiteren Untersuchung auf der Grundlage der erzwungenen Richtlinien Ihrer Organisation an ein SIEM-Tool (Security Information & Event Management) übergeben werden.

## <a name="why-is-automation-important"></a>Warum ist eine Automatisierung wichtig?

In seinem [Blogbeitrag vom Oktober 2018](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Eight-essentials-for-hybrid-identity-3-Securing-your-identity/ba-p/275843) erklärt Alex Weinert (Leiter des Microsoft-Teams für Identitätssicherheit und -schutz), warum die Automatisierung angesichts des Ereignisumfangs so wichtig ist:

> Von unseren Machine Learning- und Heuristiksystemen werden Tag für Tag Risikobewertungen für 18 Milliarden Anmeldeversuche für über 800 Millionen verschiedenen Konten generiert. 300 Millionen dieser Anmeldeversuche stammen erkennbar von Angreifern (Kriminelle, Hacker usw.).
>
> Auf der letztjährigen Ignite habe ich über die drei wichtigsten Angriffe auf unsere Identitätssysteme gesprochen. Diese Angriffe hatten zuletzt folgenden Umfang:
>   
>   - **Breach Replay:** 4,6 Milliarden erkannte Angriffe im Mai 2018
>   - **Kennwortspray:** 350.000 im April 2018
>   - **Phishing:** Diese Angriffe lassen sich nur schwer exakt quantifizieren. Im März 2018 wurden jedoch 23 Millionen Risikoereignisse erfasst, von denen viele mit Phishing in Zusammenhang stehen.

## <a name="risk-detection-and-remediation"></a>Risikoerkennung und -behandlung

Identity Protection identifiziert Risiken in folgenden Klassifizierungen:

| Risikoerkennungstyp | BESCHREIBUNG |
| --- | --- |
| Ungewöhnlicher Ortswechsel | Anmeldung von einem ungewöhnlichen Standort, basierend auf den letzten Anmeldevorgängen des Benutzers |
| Anonyme IP-Adresse | Anmeldung von einer anonymen IP-Adresse (z.B. Tor-Browser, anonymisierte VPNs) |
| Ungewöhnliche Anmeldeeigenschaften | Anmeldung mit Eigenschaften, die für den angegebenen Benutzer in letzter Zeit nicht verwendet wurden |
| Mit Schadsoftware verknüpfte IP-Adresse | Anmeldung von einer mit Schadsoftware verknüpften IP-Adresse |
| Kompromittierte Anmeldeinformationen | Diese Risikoerkennung gibt an, dass die gültigen Anmeldeinformationen des Benutzers kompromittiert wurden. |
| Azure AD Threat Intelligence | Auf der Grundlage der internen und externen Quellen für Bedrohungsdaten von Microsoft wurde ein bekanntes Angriffsmuster identifiziert. |

Weitere Informationen zu diesen Risiken sowie zu deren Berechnung finden Sie im Artikel [Was bedeutet Risiko?](concept-identity-protection-risks.md).

Die Risikosignale können Korrekturmaßnahmen zur Folge haben, die unter Umständen dazu führen, dass Benutzer die Azure Multi-Factor Authentication verwenden oder ihr Kennwort per Self-Service-Kennwortzurücksetzung zurücksetzen müssen oder blockiert werden, bis ein Administrator aktiv wird.

## <a name="risk-investigation"></a>Risikountersuchung

Administratoren können Erkennungen überprüfen und bei Bedarf manuelle Maßnahmen ergreifen. In Identity Protection stehen drei wichtige Berichte zur Verfügung, die Administratoren bei ihren Untersuchungen nutzen können:

- Riskante Benutzer
- Riskante Anmeldungen
- Risikoerkennungen

Weitere Informationen finden Sie im Artikel [Untersuchen von Risiken](howto-identity-protection-investigate-risk.md).

## <a name="exporting-risk-data"></a>Exportieren von Risikodaten

Daten aus Identity Protection können zur Archivierung sowie für weitere Untersuchungen und Korrelationsschritte in andere Tools exportiert werden. Dank der Microsoft Graph-basierten APIs können Organisationen diese Daten zur weiteren Verarbeitung in einem Tool (beispielsweise SIEM) sammeln. Der Zugriff auf die Identity Protection-API wird im Artikel [Erste Schritte mit Azure Active Directory Identity Protection und Microsoft Graph](howto-identity-protection-graph-api.md) erläutert.

Informationen zur Integration von Identity Protection-Informationen in Azure Sentinel finden Sie im Artikel [Verknüpfen von Daten aus Azure AD Identity Protection](../../sentinel/connect-azure-ad-identity-protection.md).

## <a name="permissions"></a>Berechtigungen

Benutzer müssen über die Rolle „Benutzer mit Leseberechtigung für Sicherheitsfunktionen“, „Sicherheitsoperator“, „Sicherheitsadministrator“, „Globaler Leser“ oder „Globaler Administrator“ verfügen, um auf Identity Protection zugreifen zu können.

## <a name="license-requirements"></a>Lizenzanforderungen

[!INCLUDE [Active Directory P2 license](../../../includes/active-directory-p2-license.md)]

| Funktion | Details | Azure AD Premium P2 | Azure AD Premium P1 | Azure AD Basic/Free |
| --- | --- | --- | --- | --- |
| Risikorichtlinien | Benutzerrisiko-Richtlinie (über Identity Protection) | Ja | Nein | Nein |
| Risikorichtlinien | Anmelderisiko-Richtlinie (über Identity Protection oder bedingten Zugriff) | Ja | Nein | Nein |
| Sicherheitsberichte | Übersicht | Ja | Nein | Nein |
| Sicherheitsberichte | Riskante Benutzer | Vollzugriff | Eingeschränkte Informationen | Eingeschränkte Informationen |
| Sicherheitsberichte | Riskante Anmeldungen | Vollzugriff | Eingeschränkte Informationen | Eingeschränkte Informationen |
| Sicherheitsberichte | Risikoerkennungen | Vollzugriff | Eingeschränkte Informationen | Nein |
| Benachrichtigungen | Warnungen zu erkannten gefährdeten Benutzern | Ja | Nein | Nein |
| Benachrichtigungen | Wöchentliche Übersicht | Ja | Nein | Nein |
| | Richtlinie für MFA-Registrierung | Ja | Nein | Nein |

## <a name="next-steps"></a>Nächste Schritte

- [Sicherheitsübersicht](concept-identity-protection-security-overview.md)

- [Was bedeutet Risiko?](concept-identity-protection-risks.md)

- [Verfügbare Richtlinien zum Mindern von Risiken](concept-identity-protection-policies.md)
