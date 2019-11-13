---
title: Authentifizierung in Microsoft Identity Platform | Azure
description: Enthält Informationen zu den Grundlagen der Authentifizierung auf der Microsoft Identity Platform (v2.0).
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.assetid: 0c84e7d0-16aa-4897-82f2-f53c6c990fd9
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/15/2019
ms.author: ryanwi
ms.reviewer: jmprieur, saeeda, sureshja, hirsin
ms.custom: aaddev, identityplatformtop40, scenarios:getting-started
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1ab2180c54f07ff5009e2c57d8522f2eb0b81aad
ms.sourcegitcommit: bc7725874a1502aa4c069fc1804f1f249f4fa5f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73718367"
---
# <a name="authentication-basics"></a>Authentifizierungsgrundlagen

## <a name="what-is-authentication"></a>Was ist die Authentifizierung?

In diesem Artikel werden viele Authentifizierungskonzepte behandelt, mit denen Sie vertraut sein müssen, um geschützte Web-Apps, Web-APIs oder Apps, die geschützte Web-APIs aufrufen, zu erstellen.

Bei der **Authentifizierung** weisen Sie nach, dass Sie die Person sind, als die Sie sich ausgeben. Authentifizierung wird manchmal verkürzt als AuthN bezeichnet.

Die **Autorisierung** ist die Gewährung einer Handlungsberechtigung für eine authentifizierte Person. Sie gibt an, auf welche Daten Sie zugreifen dürfen und welche Aktionen Sie damit ausführen können. Autorisierung wird manchmal verkürzt als AuthZ bezeichnet.

Statt Apps zu erstellen, die jeweils über eigene Informationen in Bezug auf Benutzername und Kennwort verfügen, was mit einem hohen Verwaltungsaufwand verbunden ist, wenn Sie für mehrere Apps Benutzer hinzufügen oder entfernen müssen, besteht die Möglichkeit, dass Apps diese Aufgabe an einen zentralen Identitätsanbieter delegieren können.

Azure Active Directory (Azure AD) ist ein zentraler Identitätsanbieter in der Cloud. Die Delegierung der Authentifizierung und Autorisierung an den Anbieter ermöglicht beispielsweise die Verwendung von Richtlinien für bedingten Zugriff, die erfordern, dass sich ein Benutzer an einem bestimmten Ort befindet, die Verwendung der mehrstufigen Authentifizierung sowie die Schaffung der Möglichkeit, dass sich Benutzer nur einmal anmelden müssen und dann automatisch bei allen Web-Apps angemeldet werden, die dasselbe zentralisierte Verzeichnis nutzen. Diese Funktion wird als einmaliges Anmelden (Single Sign-On, SSO) bezeichnet.

Ein zentraler Identitätsanbieter ist noch wichtiger für Apps mit Benutzern auf der ganzen Welt, die sich nicht immer über das Netzwerk des Unternehmens anmelden. Azure AD authentifiziert Benutzer und stellt Zugriffstoken bereit. Ein Zugriffstoken ist ein Sicherheitstoken, das von einem Autorisierungsserver ausgestellt wird. Es enthält Informationen zum Benutzer und zu der App, für die das Token bestimmt ist, und diese Informationen können zum Zugreifen auf Web-APIs und andere geschützte Ressourcen verwendet werden.

Die Microsoft Identity Platform vereinfacht die Authentifizierung für Anwendungsentwickler. Die Plattform verfügt über Identity-as-a-Service mit Unterstützung für branchenübliche Protokolle, z. B. OAuth 2.0 und OpenID Connect, sowie Open Source-Bibliotheken für verschiedene Plattformen, damit Sie schnell mit der Programmierung beginnen können. Sie ermöglicht Entwicklern das Erstellen von Anwendungen, mit denen alle Microsoft-Identitäten angemeldet werden, und das Abrufen von Token zum Aufrufen von Microsoft Graph, anderen Microsoft-APIs oder von Entwicklern erstellten APIs. Weitere Informationen finden Sie unter [Entwicklung der Microsoft Identity Platform](about-microsoft-identity-platform.md).

### <a name="tenants"></a>Mandanten

Ein Cloudidentitätsanbieter arbeitet für viele Organisationen. Um die Benutzer der verschiedenen Organisationen getrennt zu halten, ist Azure AD in Mandanten partitioniert, wobei ein Mandant pro Organisation verwendet wird.

Mit Mandanten können Benutzer und die zugehörigen Apps nachverfolgt werden. Die Microsoft Identity Platform unterstützt auch Benutzer, die sich mit persönlichen Microsoft-Konten anmelden.

Darüber hinaus verfügt Azure AD über Azure Active Directory B2C, damit Organisationen für die Anmeldung von Benutzern (meist Kunden) Identitäten sozialer Netzwerke, z. B. ein Google-Konto, verwenden können. Weitere Informationen finden Sie in der [Dokumentation für Azure Active Directory B2C](https://docs.microsoft.com/azure/active-directory-b2c).

### <a name="security-tokens"></a>Sicherheitstoken

Sicherheitstoken enthalten Informationen zu Benutzern und Apps. In Azure AD werden JSON-basierte Token (JWTs) verwendet, die Ansprüche enthalten. Mit einem Anspruch werden Assertionen einer Entität für eine andere bereitgestellt. Anwendungen können Ansprüche für verschiedene Aufgaben verwenden, z. B.:

* Überprüfen des Tokens
* Identifizieren des Verzeichnismandanten des Antragstellers
* Anzeigen von Benutzerinformationen
* Ermitteln der Autorisierung des Antragstellers

Ein Anspruch besteht aus Schlüssel-Wert-Paaren, mit denen beispielsweise die folgenden Informationen bereitgestellt werden:

- Sicherheitstokenserver, der das Token generiert hat
- Datum, an dem das Token generiert wurde
- Antragsteller, z. B. der Benutzer (mit Ausnahme von Daemons)
- Zielgruppe (App, für die das Token generiert wurde)
- App (der Client), die das Token angefordert hat. Bei Web-Apps kann dies mit der Zielgruppe identisch sein.

Ausführlichere Informationen zu Ansprüchen finden Sie in den Artikeln zu [Zugriffstoken](access-tokens.md) und [ID-Token](id-tokens.md).

Die Überprüfung des Tokens wird auf eine der folgenden Arten durchgeführt: von der App, für die das Token generiert wurde, der Web-App, mit der der Benutzer angemeldet wurde, oder der Web-API, die für die Überprüfung des Tokens aufgerufen wird. Das Token wird vom Sicherheitstokenserver (STS) mit einem privaten Schlüssel signiert. Der STS veröffentlicht den entsprechenden öffentlichen Schlüssel. Zur Überprüfung eines Tokens verifiziert die App die Signatur, indem mit dem öffentlichen STS-Schlüssel überprüft wird, ob die Signatur mit dem privaten Schlüssel erstellt wurde.

Token sind nur für einen begrenzten Zeitraum gültig. Normalerweise wird vom STS ein Tokenpaar bereitgestellt: ein Zugriffstoken zum Zugreifen auf die Anwendung oder geschützte Ressource und ein Aktualisierungstoken, das zum Aktualisieren des Zugriffstokens verwendet wird, wenn der Ablauf des Zugriffstokens kurz bevorsteht. 

Zugriffstoken werden als Bearertoken im `Authenticate`-Header an eine Web-API übergeben. Eine App kann ein Aktualisierungstoken an den STS übergeben, und wenn der Benutzerzugriff auf die App nicht widerrufen wurde, erhält sie ein neues Zugriffstoken und ein neues Aktualisierungstoken zurück. So wird ein Fall behandelt, bei dem eine Person aus dem Unternehmen ausscheidet. Wenn der STS ein Aktualisierungstoken empfängt, stellt er kein weiteres gültiges Zugriffstoken aus, falls der Benutzer nicht mehr autorisiert ist.

## <a name="application-model"></a>Anwendungsmodell

Anwendungen können Benutzer selbst anmelden oder die Anmeldung an einen Identitätsanbieter delegieren. Informationen zu Anmeldeszenarien, die von Azure AD unterstützt werden, finden Sie unter [Authentifizierungsflüsse und App-Szenarien](authentication-flows-app-scenarios.md).

Damit ein Identitätsanbieter weiß, dass ein Benutzer Zugriff auf eine bestimmte App hat, müssen sowohl der Benutzer als auch die Anwendung beim Identitätsanbieter registriert werden. Wenn Sie Ihre Anwendung bei Azure AD registrieren, stellen Sie eine Identitätskonfiguration für Ihre Anwendung bereit, die die Integration in Azure AD ermöglicht. Die Registrierung der App ermöglicht Ihnen außerdem Folgendes:

- Anpassen des Brandings Ihrer Anwendung im Dialogfeld für die Anmeldung. Dies ist wichtig, da dies der erste Eindruck ist, den ein Benutzer von Ihrer App erhält.
- Entscheiden, ob Sie die Anmeldung für Benutzer nur zulassen möchten, wenn sie Ihrer Organisation angehören. Hierbei handelt es sich um eine Einzelinstanzanwendung. Oder lassen Sie für Benutzer die Anmeldung per Geschäfts-, Schul-oder Unikonto zu. Hierbei handelt es sich um eine mehrinstanzenfähige Anwendung. Sie können auch persönliche Microsoft-Konten oder ein Social Media-Konto zulassen, z. B. von LinkedIn, Google usw.
- Anfordern von Bereichsberechtigungen. Sie können beispielsweise den Bereich „user.read“ anfordern, mit dem die Berechtigung zum Lesen des Profils des angemeldeten Benutzers gewährt wird.
- Definieren von Bereichen, mit denen der Zugriff auf Ihre Web-API definiert wird. Wenn eine App auf Ihre API zugreifen möchte, muss sie normalerweise Berechtigungen für die von Ihnen definierten Bereiche anfordern.
- Austauschen eines Geheimnisses mit Azure AD, um die Identität der App gegenüber Azure AD nachzuweisen.  Dies ist relevant für den Fall, dass es sich bei der App um eine vertrauliche Clientanwendung handelt. In einer vertraulichen Clientanwendung können Anmeldeinformationen sicher aufbewahrt werden. Zum Speichern der Anmeldeinformationen wird ein vertrauenswürdiger Back-End-Server benötigt.

Nach der Registrierung erhält die Anwendung eine GUID, die von der App beim Anfordern von Token mit Azure AD ausgetauscht wird. Wenn es sich bei der App um eine vertrauliche Clientanwendung handelt, wird auch das Geheimnis oder der öffentliche Schlüssel ausgetauscht. Dies hängt davon ab, ob Zertifikate oder Geheimnisse verwendet werden.

Für die Microsoft Identity Platform werden Anwendungen mit einem Modell dargestellt, das zwei Hauptfunktionen erfüllt:

Identifizieren der App anhand der unterstützten Authentifizierungsprotokolle und Bereitstellen aller Bezeichner, URLs, Geheimnisse und zugehörigen Informationen, die für die Authentifizierung benötigt werden
Für die Microsoft Identity Platform gilt Folgendes:

* Enthält alle Daten, die zur Unterstützung der Authentifizierung zur Laufzeit erforderlich sind.
* Enthält alle Daten zum Treffen der Entscheidung, auf welche Ressourcen eine App ggf. zugreifen muss und unter welchen Umständen eine bestimmte Anforderung erfüllt werden soll.
* Stellt Infrastruktur für die Implementierung der App-Bereitstellung innerhalb des Mandanten des App-Entwicklers und für andere Azure AD-Mandanten bereit.
* Behandelt die Benutzereinwilligung während der Tokenanforderung und vereinfacht die dynamische mandantenübergreifenden Bereitstellung von Apps

Die Einwilligung ist der Prozess, bei dem ein Ressourcenbesitzer einer Clientanwendung durch spezifische Berechtigungen die Autorisierung für den Zugriff auf geschützte Ressourcen im Auftrag des Ressourcenbesitzers gewährt. Für die Microsoft Identity Platform gilt Folgendes:

* Ermöglicht Benutzern und Administratoren, der App dynamisch die Zustimmung zum Zugriff auf Ressourcen in ihrem Namen zu erteilen oder zu verweigern.
* Ermöglicht es Administratoren, letztendlich zu entscheiden, welche Aktionen Apps ausführen und welche Benutzer bestimmte Apps verwenden dürfen und wie auf die Verzeichnisressourcen zugegriffen wird.

In der Microsoft Identity Platform beschreibt ein **Anwendungsobjekt** eine Anwendung als eine abstrakte Entität. Zum Zeitpunkt der Bereitstellung verwendet die Microsoft Identity Platform das Anwendungsobjekt als Blaupause zum Erstellen eines **Dienstprinzipals**, der eine konkrete Instanz einer Anwendung in einem Verzeichnis oder Mandanten darstellt. Mit dem Dienstprinzipal wird definiert, welche Aktionen die App in einem bestimmten Zielverzeichnis ausführen darf, wer sie verwenden kann, auf welche Ressourcen sie Zugriff besitzt usw. Die Microsoft Identity Platform erstellt mittels **Einwilligung** aus einem Anwendungsobjekt einen Dienstprinzipal.

Die folgende Abbildung zeigt einen vereinfachten Microsoft Identity Platform-Bereitstellungsablauf, der durch die Einwilligung gesteuert wird. Es sind zwei Mandanten (A und B) dargestellt. Mandant A ist der Besitzer der Anwendung. Mandant B instanziiert die Anwendung über einen Dienstprinzipal.  

![Vereinfachter Bereitstellungsablauf, der durch Zustimmung gesteuert wird](./media/authentication-scenarios/simplified-provisioning-flow-consent-driven.svg)

In diesem Bereitstellungsablauf geschieht Folgendes:

1. Ein Benutzer von Mandant B versucht, sich mit der App anzumelden, und der Autorisierungsendpunkt fordert ein Token für die Anwendung an.
1. Die Anmeldeinformationen des Benutzers werden abgerufen und für die Authentifizierung überprüft.
1. Der Benutzer wird aufgefordert, dem Zugriff auf den Mandanten B für die App zuzustimmen.
1. Die Microsoft Identity Platform verwendet das Anwendungsobjekt in Mandant A als Blaupause für die Erstellung eines Dienstprinzipals in Mandant B.
1. Der Benutzer erhält das angeforderte Token.

Sie können diesen Vorgang für weitere Mandanten wiederholen. Der Mandant A enthält die Blaupause für die App (Anwendungsobjekt). Benutzer und Administratoren aller anderen Mandanten, in denen der App die Zustimmung erteilt wird, behalten durch das entsprechende Dienstprinzipalobjekt in jedem Mandanten die Kontrolle darüber, welche Aktionen die Anwendung ausführen darf. Weitere Informationen finden Sie unter [Anwendungs- und Dienstprinzipalobjekte in Microsoft Identity Platform](app-objects-and-service-principals.md).

## <a name="web-app-sign-in-flow-with-azure-ad"></a>Web-App-Anmeldefluss mit Azure AD

Wenn ein Benutzer im Browser zu einer Web-App navigiert, passiert Folgendes:

- Die Web-App ermittelt, ob der Benutzer bereits authentifiziert wurde.
- Falls der Benutzer nicht authentifiziert wurde, delegiert die Web-App den Vorgang an Azure AD, damit der Benutzer angemeldet wird. Diese Anmeldung ist mit der Richtlinie der Organisation konform. Dies kann bedeuten, dass der Benutzer zum Eingeben seiner Anmeldeinformationen aufgefordert wird, indem die mehrstufige Authentifizierung oder ein Verfahren ganz ohne Kennwort (z. B. Windows Hello) verwendet wird.
- Der Benutzer wird aufgefordert, seine Einwilligung für den Zugriff zu erteilen, den die Client-App benötigt. Aus diesem Grund müssen Client-Apps bei Azure AD registriert werden. Azure AD kann dann Token für den Zugriff bereitstellen, für den der Benutzer seine Einwilligung erteilt hat.

Nach der erfolgreichen Authentifizierung des Benutzers:

- Azure AD sendet ein Token an die Web-App.
- Es wird ein Cookie gespeichert, das der Domäne von Azure AD, in der die Identität des Benutzers im Cookiebehälter des Browsers enthalten ist, zugeordnet ist. Wenn eine App den Browser das nächste Mal zum Navigieren zum Endpunkt für die Azure AD-Autorisierung verwendet, stellt der Browser das Cookie bereit, damit sich der Benutzer nicht erneut anmelden muss. Auf diese Weise wird auch einmaliges Anmelden (SSO) ermöglicht. Das Cookie wird von Azure AD erstellt und ist nur für Azure AD verwendbar.
- Anschließend wird das Token von der Web-App überprüft. Wenn die Überprüfung erfolgreich ist, zeigt die Web-App die geschützte Seite an und speichert im Cookiebehälter des Browsers ein Sitzungscookie. Wenn der Benutzer zu einer anderen Seite navigiert, verfügt die Web-App aufgrund des Sitzungscookies über die Information, dass der Benutzer authentifiziert wurde.

Das folgende Sequenzdiagramm enthält eine Zusammenfassung dieser Interaktion:

![Authentifizierungsprozess für Web-Apps](media/authentication-scenarios/web-app-how-it-appears-to-be.png)

### <a name="how-a-web-app-determines-if-the-user-is-authenticated"></a>Ermittlung der Authentifizierung des Benutzers durch die Web-App

Web-App-Entwickler können angeben, ob für alle oder nur für bestimmte Seiten eine Authentifizierung erforderlich ist. In ASP.NET/ASP.NET Core wird hierfür beispielsweise das Attribut `[Authorize]` den Controlleraktionen hinzugefügt. 

Dieses Attribut bewirkt, dass für ASP.NET überprüft wird, ob ein Sitzungscookie mit der Identität des Benutzers vorhanden ist. Falls kein Cookie vorhanden ist, leitet ASP.NET die Authentifizierung an den angegebenen Identitätsanbieter um. Wenn Azure AD als Identitätsanbieter verwendet wird, leitet die Web-App die Authentifizierung an https://login.microsoftonline.com um, und es wird ein Dialogfeld für die Anmeldung angezeigt.

### <a name="how-a-web-app-delegates-sign-in-to-azure-ad-and-obtains-a-token"></a>Delegierung der Anmeldung an Azure AD und Abruf eines Tokens durch die Web-App

Die Benutzerauthentifizierung erfolgt über den Browser. Für das OpenID-Protokoll werden die üblichen HTTP-Protokollnachrichten verwendet.
- Die Web-App sendet „HTTP 202 (Umleitung)“ an den Browser, um Azure AD verwenden zu können.
- Nachdem der Benutzer authentifiziert wurde, sendet Azure AD das Token an die Web-App, indem eine Umleitung über den Browser genutzt wird.
- Die Umleitung durch die Web-App erfolgt in Form eines Umleitungs-URIs. Dieser Umleitungs-URI wird unter dem Azure AD-Anwendungsobjekt registriert. Es kann mehrere Umleitungs-URIs geben, weil die Anwendung ggf. unter mehreren URLs bereitgestellt wird. Daher muss die Web-App auch den zu verwendenden Umleitungs-URI angeben.
- Azure AD überprüft, ob der von der Web-App gesendete Umleitungs-URI einer der registrierten URIs für die App ist.

## <a name="desktop-and-mobile-app-sign-in-flow-with-azure-ad"></a>Anmeldefluss mit Azure AD für Desktop- und mobile Apps

Der oben beschriebene Ablauf gilt mit geringfügigen Abweichungen auch für Desktop- und mobile Anwendungen.

Für Desktop- und mobile Anwendungen kann für die Authentifizierung ein eingebettetes Websteuerelement oder ein Systembrowser verwendet werden. Im folgenden Diagramm ist dargestellt, wie eine Desktop- oder mobile App die Microsoft-Authentifizierungsbibliothek (Microsoft Authentication Library, MSAL) verwendet, um Zugriffstoken abzurufen und Web-APIs aufzurufen.

![Darstellung einer Desktop-App](media/authentication-scenarios/web-app-how-it-appears-to-be.png)

Für die MSAL wird ein Browser genutzt, um Token abzurufen, und wie bei Web-Apps wird die Authentifizierung an Azure AD delegiert.

Da von Azure AD das gleiche Identitätscookie wie für Web-Apps im Browser gespeichert wird, gilt Folgendes: Wenn die native oder mobile App den Systembrowser verwendet, wird unmittelbar SSO mit der entsprechenden Web-App ermöglicht.

Standardmäßig wird für die MSAL der Systembrowser genutzt. Eine Ausnahme stellen .NET Framework-Desktopanwendungen dar, bei denen ein eingebettetes Steuerelement verwendet wird, um eine stärker integrierte Benutzeroberfläche bereitzustellen.

## <a name="next-steps"></a>Nächste Schritte

Machen Sie sich unter [Microsoft Identity Platform – Glossar für Entwickler](developer-glossary.md) mit den wichtigsten Begriffen vertraut.
Weitere Informationen zu anderen Szenarien für die Authentifizierung von Benutzern, die von der Microsoft Identity Platform unterstützt werden, finden Sie unter [Authentifizierungsflüsse und App-Szenarien](authentication-flows-app-scenarios.md).
Weitere Informationen zu den Microsoft-Bibliotheken, mit denen Sie für Microsoft-Konten, Azure AD-Konten und Azure AD B2C-Benutzer geeignete Anwendungen entwickeln können – alles mit einem zentralen, optimierten Programmiermodell –, finden Sie unter [MSAL-Bibliotheken](msal-overview.md).
