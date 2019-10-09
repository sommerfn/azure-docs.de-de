---
title: Autorisierungs-Agents und deren Aktivierung | Azure
description: Hier erhalten Sie Informationen zu den verschiedenen Autorisierungs-Agents, deren Verwendung die Microsoft Authentication Library (MSAL) für Ihre Android-App unterstützt, sowie zum Aktivieren dieser Agents.
services: active-directory
documentationcenter: dev-center-name
author: tylermsft
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/05/2019
ms.author: twhitney
ms.reviewer: ''
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: f7daf40d6c6e552d6b76e424359f57f031641039
ms.sourcegitcommit: 5f0f1accf4b03629fcb5a371d9355a99d54c5a7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/30/2019
ms.locfileid: "71678850"
---
# <a name="authorization-agents-android"></a>Authorization Agents (Android)

In diesem Artikel werden die verschiedenen Autorisierungs-Agents beschrieben, deren Verwendung die Microsoft Authentication Library (MSAL) für Ihre App unterstützt, und es wird erläutert, wie Sie diese aktivieren.

Die Auswahl einer bestimmten Strategie für Autorisierungs-Agents ist optional und stellt eine zusätzliche Funktionalität dar, die Apps anpassen können. Die meisten Apps verwenden die MSAL-Standardeinstellungen (die verschiedenen Standardeinstellungen finden Sie unter [Verstehen der Android-MSAL-Konfigurationsdatei](msal-configuration.md)).

MSAL unterstützt die Autorisierung mithilfe einer `WebView` oder des Systembrowsers.  In der folgenden Abbildung sind Autorisierungen mithilfe der `WebView` und des Systembrowsers mit bzw. ohne benutzerdefinierte Registerkarten dargestellt:

![MSAL-Anmeldebeispiele](./media/authorization-agents/sign-in-ui.jpg)

## <a name="single-sign-in-implications"></a>Auswirkungen des einmaligen Anmeldens

Die in MSAL integrierten Anwendungen verwenden standardmäßig die benutzerdefinierten Registerkarten des Systembrowsers für die Autorisierung. Im Gegensatz zu WebViews verwenden benutzerdefinierte Registerkarten einen Cookiebehälter gemeinsam mit dem Standardsystembrowser und ermöglichen dadurch weniger Anmeldungen bei Web-Apps oder anderen nativen Apps, die in benutzerdefinierte Registerkarten integriert sind.

Wenn die Anwendung eine `WebView`-Strategie verwendet, ohne Microsoft Authenticator- oder Unternehmensportal-Unterstützung in die App zu integrieren, steht den Benutzern keine Oberfläche für einmaliges Anmelden (Single Sign-On, SSO) auf dem Gerät oder zwischen nativen Apps und Web-Apps zur Verfügung.

Wenn die Anwendung MSAL mit Microsoft Authenticator- oder Unternehmensportal-Unterstützung verwendet, können die Benutzer eine SSO-Oberfläche nutzen, sofern eine aktive Benutzeranmeldung bei einer der Apps besteht.

## <a name="webview"></a>WebView

Wenn Sie die In-App-WebView verwenden möchten, fügen Sie in die an MSAL übermittelte JSON-App-Konfigurationsdatei die folgende Zeile ein:

```json
"authorization_user_agent" : "WEBVIEW"
```

Bei Verwendung der In-App-`WebView` meldet sich der Benutzer direkt bei der App an. Die Token werden in der Sandbox der App gespeichert und sind außerhalb des Cookiebehälters der App nicht verfügbar. Folglich steht dem Benutzer eine anwendungsübergreifende SSO-Funktion nur dann zur Verfügung, wenn die Apps in Microsoft Authenticator oder Unternehmensportal integriert sind.

`WebView` bietet jedoch die Möglichkeit, das Aussehen und Verhalten der Benutzeroberfläche für die Anmeldung anzupassen. Weitere Informationen dazu, wie Sie diese Anpassung vornehmen, finden Sie unter [Android-WebViews](https://developer.android.com/reference/android/webkit/WebView).

## <a name="default-browser-plus-custom-tabs"></a>Standardbrowser und benutzerdefinierte Registerkarten

MSAL verwendet standardmäßig den Browser und eine Strategie mit [benutzerdefinierten Registerkarten](https://developer.chrome.com/multidevice/android/customtabs). Sie können diese Strategie explizit angeben, um Änderungen an `DEFAULT` in zukünftigen Releases zu verhindern, indem Sie in der benutzerdefinierten Konfigurationsdatei die folgende JSON-Konfiguration verwenden:

```json
"authorization_user_agent" : "BROWSER"
```

Verwenden Sie diesen Ansatz, um eine SSO-Funktion über den Browser des Geräts bereitzustellen. MSAL verwendet einen freigegebenen Cookiebehälter, wodurch andere native Apps oder Web-Apps mithilfe des von MSAL festgelegten persistenten Sitzungscookies SSO auf dem Gerät erreichen.

## <a name="browser-selection-heuristic"></a>Browserauswahlheuristik

Da MSAL unmöglich das genaue Browserpaket angeben kann, das auf den vielen verschiedenen Android-Smartphones verwendet werden soll, implementiert MSAL eine Browserauswahlheuristik, die versucht, das beste geräteübergreifende SSO bereitzustellen.

MSAL ruft die vollständige Liste der auf dem Gerät installierten Browser ab, um den zu verwendenden Browser auszuwählen. Die Liste liegt in der vom Paket-Manager zurückgegebenen Reihenfolge vor, die indirekt die Einstellungen des Benutzers widerspiegelt. So ist beispielsweise der Standardbrowser (sofern festgelegt) der erste Eintrag in der Liste. Der _erste_ Browser in der Liste wird ausgewählt, unabhängig davon, ob er benutzerdefinierte Registerkarten unterstützt. Wenn der Browser benutzerdefinierte Registerkarten unterstützt, startet MSAL die benutzerdefinierte Registerkarte. Benutzerdefinierte Registerkarten entsprechen in Aussehen und Verhalten eher einer In-App-`WebView` und ermöglichen eine grundlegende Anpassung der Benutzeroberfläche. Weitere Informationen finden Sie unter [Benutzerdefinierte Registerkarten unter Android](https://developer.chrome.com/multidevice/android/customtabs).

Wenn auf dem Gerät keine Browserpakete vorhanden sind, verwendet MSAL die In-App-`WebView`.

Die Reihenfolge der Browser in der Browserliste richtet sich nach dem Betriebssystem. Sie ist in der Reihenfolge vom am meisten bevorzugten zum am wenigsten verwendeten Browser sortiert. Wenn die Standardeinstellung des Geräts nicht geändert wird, sollte für jede Anmeldung der gleiche Browser gestartet werden, um eine SSO-Funktion sicherzustellen.

> [!NOTE]
> Wenn ein anderer Browser als Standard festgelegt ist, bevorzugt MSAL nicht mehr immer Chrome. Auf einem Gerät, auf dem Chrome und ein anderer Browser vorinstalliert sind, verwendet MSAL beispielsweise den Browser, den der Benutzer als Standard festgelegt hat.

### <a name="tested-browsers"></a>Getestete Browser

Die folgenden Browser wurden getestet, um zu prüfen, ob eine korrekte Umleitung zu dem in der Konfigurationsdatei angegebenen `"redirect_uri"` erfolgt:

| | Integrierter Browser | Chrome | Opera  | Microsoft Edge | UC Browser | Firefox |
| -- |:-------------:| -----:|-----:|-----:|-----:|-----:|
| Nexus 4 (API 17) | erfolgreich | erfolgreich |nicht zutreffend |nicht zutreffend |nicht zutreffend |nicht zutreffend |
| Samsung S7 (API 25) | erfolgreich* | erfolgreich | erfolgreich | erfolgreich | fehlerhaft |erfolgreich |
| Huawei (API 26) |erfolgreich** | erfolgreich | fehlerhaft | erfolgreich | erfolgreich |erfolgreich |
| Vivo (API 26) |erfolgreich|erfolgreich|erfolgreich|erfolgreich|erfolgreich|fehlerhaft|
| Pixel 2 (API 26) |erfolgreich | erfolgreich | erfolgreich | erfolgreich | fehlerhaft |erfolgreich |
| Oppo | erfolgreich | nicht zutreffend*** |nicht zutreffend  |nicht zutreffend |nicht zutreffend | nicht zutreffend|
| OnePlus (API 25) |erfolgreich | erfolgreich | erfolgreich | erfolgreich | fehlerhaft |erfolgreich |
| Nexus (API 28) |erfolgreich | erfolgreich | erfolgreich | erfolgreich | fehlerhaft |erfolgreich |
|MI | erfolgreich | erfolgreich | erfolgreich | erfolgreich | fehlerhaft |erfolgreich |

*Der auf Samsung-Geräten integrierte Browser ist Samsung Internet.  
**Der auf Huawei-Geräten integrierte Browser ist Huawei Browser.  
***Der Standardbrowser kann nicht in der Oppo-Geräteeinstellung geändert werden.
