---
title: Validierungsunterschiede nach unterstützten Kontotypen (signInAudience) – Microsoft Identity Platform/Azure Active Directory
description: Informieren Sie sich über die Validierungsunterschiede verschiedener Eigenschaften bei den unterschiedlichen unterstützten Kontotypen, die beim Registrieren Ihrer App bei Microsoft Identity Platform auftreten.
author: SureshJa
ms.author: sureshja
manager: CelesteDG
ms.date: 10/12/2019
ms.topic: conceptual
ms.subservice: develop
ms.custom: aaddev
ms.service: active-directory
ms.reviewer: lenalepa, manrath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3bea6e3ea93e8b630970a8d86fc246eddaf3f56c
ms.sourcegitcommit: bb65043d5e49b8af94bba0e96c36796987f5a2be
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2019
ms.locfileid: "72392053"
---
# <a name="validation-differences-by-supported-account-types-signinaudience"></a>Validierungsunterschiede nach unterstützten Kontotypen (signInAudience)

Wenn Sie eine Anwendung bei Microsoft Identity Platform für Entwickler registrieren, müssen Sie auswählen, welche Kontotypen von Ihrer Anwendung unterstützt werden sollen. Im Anwendungsobjekt und Manifest ist `signInAudience` diese Eigenschaft.

Folgende Optionen stehen dafür zur Verfügung:

- *AzureADMyOrg*: Nur Konten im Organisationsverzeichnis, in dem die App registriert wird (ein Mandant)
- *AzureADMultipleOrgs*: Konten in einem beliebigen Organisationsverzeichnis (mehrere Mandanten)
- *AzureADandPersonalMicrosoftAccount*: Konten in einem beliebigen Organisationsverzeichnis (mehrere Mandanten) und persönliche Microsoft-Konten (z.B. Skype, Xbox und Outlook.com)

Bei registrierten Anwendungen finden Sie den Wert für unterstützte Kontotypen im Abschnitt **Authentifizierung** einer Anwendung. Außerdem finden Sie ihn im **Manifest** unter der `signInAudience`-Eigenschaft.

Der Wert, den Sie für diese Eigenschaft auswählen, hat Auswirkungen auf andere App-Objekteigenschaften. Daher müssen Sie beim Ändern dieser Eigenschaft möglicherweise zuerst andere Eigenschaften ändern.

In der folgenden Tabelle finden Sie die Validierungsunterschiede verschiedener Eigenschaften bei den unterschiedlichen unterstützten Kontotypen.

| Eigenschaft | `AzureADMyOrg` | `AzureADMultipleOrgs`  | `AzureADandPersonalMicrosoftAccount` |
|--------------|---------------|----------------|----------------|
| Anwendungs-ID-URI (`identifierURIs`)  | Muss im Mandanten eindeutig sein <br><br> urn://-Schemas werden unterstützt <br><br> Platzhalter werden nicht unterstützt <br><br> Abfragezeichenfolgen und Fragmente werden unterstützt <br><br> Die maximale Länge beträgt 255 Zeichen <br><br> Keine Begrenzung* für die Anzahl der ID-URIs  | Global eindeutig <br><br> urn://-Schemas werden unterstützt <br><br> Platzhalter werden nicht unterstützt <br><br> Abfragezeichenfolgen und Fragmente werden unterstützt <br><br> Die maximale Länge beträgt 255 Zeichen <br><br> Keine Begrenzung* für die Anzahl der ID-URIs | Global eindeutig <br><br> urn://-Schemas werden nicht unterstützt <br><br> Platzhalter, Fragmente und Abfragezeichenfolgen werden nicht unterstützt <br><br> Die maximale Länge beträgt 120 Zeichen <br><br> Maximal 50 ID-URIs |
| Zertifikate (`keyCredentials`) | Symmetrischer Signaturschlüssel | Symmetrischer Signaturschlüssel | Verschlüsselungsschlüssel und asymmetrischer Signaturschlüssel | 
| Geheime Clientschlüssel (`passwordCredentials`) | Keine Begrenzung* | Keine Begrenzung* | Wenn liveSDK aktiviert ist: Maximal 2 geheime Clientschlüssel | 
| Umleitungs-URIs (`replyURLs`) | Weitere Informationen finden Sie unter [Umleitungs-URI/Antwort-URL: Einschränkungen](reply-url.md). | | | 
| API-Berechtigungen (`requiredResourceAccess`) | Keine Begrenzung* | Keine Begrenzung* | Maximal 30 Berechtigungen pro Ressource (z.B. Microsoft Graph) | 
| Durch diese API definierte Bereiche (`oauth2Permissions`) | Die maximale Länge des Bereichsnamens beträgt 120 Zeichen <br><br> Keine Begrenzung* bei der Anzahl der definierten Bereiche | Die maximale Länge des Bereichsnamens beträgt 120 Zeichen <br><br> Keine Begrenzung* bei der Anzahl der definierten Bereiche |  Die maximale Länge des Bereichsnamens beträgt 40 Zeichen <br><br> Maximal 100 definierte Bereiche | 
| Autorisierte Clientanwendungen (`preautorizedApplications`) | Keine Begrenzung* | Keine Begrenzung* | Maximale Gesamtzahl von 500 <br><br> Maximal 100 definierte Client-Apps <br><br> Maximal 30 Bereiche, die pro Client definiert sind | 
| appRoles | Unterstützt <br> Keine Begrenzung* | Unterstützt <br> Keine Begrenzung* | Nicht unterstützt | 
| Abmelde-URL | http://localhost ist zulässig <br><br> Die maximale Länge beträgt 255 Zeichen | http://localhost ist zulässig <br><br> Die maximale Länge beträgt 255 Zeichen | <br><br> https://localhost ist zulässig, http://localhost schlägt bei MSA fehl <br><br> Die maximale Länge beträgt 255 Zeichen <br><br> HTTP-Schema ist nicht zulässig <br><br> Platzhalter werden nicht unterstützt | 

\* Es gibt eine globale Beschränkung von ungefähr 1000 Elementen in allen Sammlungseigenschaften für das App-Objekt

## <a name="next-steps"></a>Nächste Schritte

- Informationen zum [Registrieren von Anwendungen](app-objects-and-service-principals.md)
- Informationen zum [Anwendungsmanifest](reference-app-manifest.md)
