---
title: Umleitungen/Beschränkungen und Einschränkungen der Antwort-URLs-Microsoft Identitätsplattform
description: Antwort-URLs/Umleitungen von URls Einschränkungen und Einschränkungen
author: SureshJa
ms.author: sureshja
manager: CelesteDG
ms.date: 06/29/2019
ms.topic: article
ms.subservice: develop
ms.custom: aaddev
ms.service: active-directory
ms.reviewer: lenalepa, manrath
ms.collection: M365-identity-device-management
ms.openlocfilehash: e5e557d74ff0cb959b11e99391c47e91a90d17ef
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/18/2019
ms.locfileid: "68325297"
---
# <a name="redirect-urireply-url-restrictions-and-limitations"></a>Umleitungs-URI/Antwort-URL: Einschränkungen

Eine Redirect-URI oder Antwort-URL ist der Ort, an den der Autorisierungsserver den Benutzer sendet, sobald die App erfolgreich autorisiert wurde und ihm einen Autorisierungscode oder ein Zugriffstoken zugewiesen wurde. Der Code oder das Token ist im Redirect-URI oder Antwort-Token enthalten, daher ist es wichtig, dass Sie den richtigen Ort als Teil des App-Registrierungsprozesses registrieren.

## <a name="maximum-number-of-redirect-uris"></a>Maximale Anzahl von umgeleiteten URIs

Die folgende Tabelle zeigt die maximale Anzahl von Umleitungsleitungs-URIs, die Sie hinzufügen können, wenn Sie Ihre App registrieren. 

| Angemeldete Konten | Maximale Anzahl von umgeleiteten URIs | BESCHREIBUNG |
|--------------------------|---------------------------------|-------------|
| Microsoft-Geschäfts- oder Schulkonten im Azure Active Directory (Azure AD)-Mandant eines Unternehmens | 256 | `signInAudience`im Anwendungsmanifest ist entweder auf *AzureADMyOrg* oder *AzureADMultipleOrgs* gesetzt. |
| Persönliche Microsoft-Konten und Geschäfts- und Schulkonten | 100 | `signInAudience` Feld im Anwendungsmanifest ist auf *AzureAD und PersonalMicrosoftAccount* gesetzt |

## <a name="maximum-uri-length"></a>Maximale URI-Länge

Sie können maximal 256 Zeichen für jede Redirect-URI verwenden, die Sie einer App-Registrierung hinzufügen.

## <a name="restrictions-using-a-wildcard-in-uris"></a>Einschränkungen durch Verwendung eines Platzhalters in URIs

Wildcard-URIs, wie z.B. `https://*.contoso.com`, sind praktisch, sollten aber vermieden werden. Verwenden von Platzhaltern in den umleitungs-URI hat sicherheitsauswirkungen. Gemäß der OAuth 2.0-Spezifikation ([Abschnitt 3.1.2 von RFC 6749](https://tools.ietf.org/html/rfc6749#section-3.1.2)) muss ein Redirection-Endpunkt-URI ein absoluter URI sein. 

Das Azure AD-Anwendungsmodell unterstützt keine Wildcard-URIs für Anwendungen, die so konfiguriert sind, dass sie sich in persönlichen Microsoft-Konten und Geschäfts- oder Schulkonten anmelden. Wildcard-URIs sind jedoch für Apps erlaubt, die konfiguriert sind, um sich heute beim Azure AD-Mieter einer Organisation in Geschäfts- oder Schulkonten anzumelden. 
 
> [!NOTE]
> Die neue Erfahrung bei der [App-Registrierung](https://go.microsoft.com/fwlink/?linkid=2083908) erlaubt es Entwicklern nicht, Wildcard-URIs auf der Benutzeroberfläche hinzuzufügen. Das Hinzufügen von wilcard URI für Apps, die sich in Geschäfts- oder Schulkonten anmelden, wird nur durch den App Manifest Editor unterstützt. In Zukunft werden neue Apps keine Wildcards mehr in der Redirect-URI verwenden können. Ältere Anwendungen, die Wildcards in Redirect-URIs enthalten, funktionieren jedoch weiterhin.

Wenn Ihre Situation mehr Redirect-URIs erfordert, als die maximal zulässige Grenze, sollten Sie anstelle eines Wildcard-Redirect-URIs einen der folgenden Ansätze wählen.

### <a name="use-a-state-parameter"></a>Verwenden Sie einen Zustandsparameter

Wenn Sie eine Anzahl von Subdomänen haben und wenn Ihr Szenario erfordert, dass Sie Benutzer bei erfolgreicher Authentifizierung auf die gleiche Seite umleiten, auf der sie gestartet wurden, kann die Verwendung eines Statusparameters hilfreich sein. 

Dieser Ansatz ermöglicht Folgendes:

1. Erstellen Sie eine "gemeinsame" Umleitungs-URI pro Anwendung, um die Sicherheitstoken zu verarbeiten, die Sie vom Autorisierungsendpunkt erhalten.
1. Ihre Anwendung kann anwendungsspezifische Parameter (z.B. Subdomain-URL, von der aus der Benutzer stammt, oder andere Informationen wie Brandinginformationen) im State Parameter senden. Wenn Sie einen Zustandsparameter verwenden, schützen Sie sich vor dem CSRF-Schutz gemäß [Abschnitt 10.12 von RFC 6749](https://tools.ietf.org/html/rfc6749#section-10.12) ). 
1. Die anwendungsspezifischen Parameter beinhalten alle Informationen, die für die Anwendung erforderlich sind, um die richtige Erfahrung für den Benutzer zu schaffen, d.h. den entsprechenden Anwendungszustand aufzubauen. Der Azure AD-Autorisierungsendpunkt entfernt HTML aus dem State Parameter, also stellen Sie sicher, dass Sie in diesem Parameter keinen HTML-Inhalt übergeben.
1. Wenn Azure AD eine Antwort auf den „gemeinsamen“ Umleitungs-URI sendet, sendet es den State Parameter an die Anwendung zurück.
1. Die Anwendung kann dann anhand des Wertes im State Parameter bestimmen, an welche URL der Benutzer weitergeleitet werden soll. Stellen Sie sicher, dass Sie den CSRF-Schutz validieren.

> [!NOTE]
> Dieser Ansatz ermöglicht es einem kompromittierten Client, die zusätzlichen Parameter, die im State Parameter gesendet werden, zu ändern, wodurch der Benutzer zu einer anderen URL umgeleitet wird, nämlich der [Open Redirector Bedrohung](https://tools.ietf.org/html/rfc6819#section-4.2.4), die in RFC 6819 beschrieben ist. Daher muss der Client diese Parameter schützen, indem er den Zustand verschlüsselt oder auf andere Weise verifiziert, z.B. durch Validierung des Domänennamens im Umleitungs-URI gegen das Token.

### <a name="add-redirect-uris-to-service-principals"></a>Hinzufügen von Umleitungs-URIsmleitungs-URIs zu Service-Prinzipien

Ein weiterer Ansatz ist das Hinzufügen von Umleitungs-URIs zu den [Service-Prinzipien](app-objects-and-service-principals.md#application-and-service-principal-relationship), die Ihre App-Registrierung in jedem Azure AD-Mandanten darstellen. Sie können diesen Ansatz verwenden, wenn Sie keinen State Parameter verwenden können oder Ihre Situation erfordert, dass Sie für jeden neuen Mandanten, den Sie unterstützen, neue Umleitungs-URIs zur App-Registrierung hinzufügen. 

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum [Anwendungsmanifest](reference-app-manifest.md)
