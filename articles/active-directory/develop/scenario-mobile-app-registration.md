---
title: Mobile App zum Aufrufen von Web-APIs – App-Codekonfiguration
titleSuffix: Microsoft identity platform
description: Erfahren Sie, wie Sie eine mobile App erstellen, die Web-APIs aufruft (Codekonfiguration der App).
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.reviwer: brandwe
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5f55e73fa1a73908d7e77bacc6af24ea1a40ba92
ms.sourcegitcommit: be8e2e0a3eb2ad49ed5b996461d4bff7cba8a837
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72803739"
---
# <a name="mobile-app-that-calls-web-apis---app-registration"></a>Mobile App, die Web-APIs aufruft – App-Registrierung

Dieser Artikel enthält eine Anleitung zur App-Registrierung, die zur Erstellung einer mobilen Anwendung erforderlich ist.

## <a name="supported-accounts-types"></a>Unterstützte Kontotypen

Welche Kontotypen in mobilen Anwendungen unterstützt werden, hängt von den Funktionen, die Sie aktivieren möchten, und den Flows ab, die Sie verwenden möchten.

### <a name="audience-for-interactive-token-acquisition"></a>Zielgruppe für den interaktiven Tokenabruf

Die meisten mobilen Anwendungen verwenden die interaktive Authentifizierung. Wenn dies der Fall ist, können Sie Benutzer jedes [Kontotyps](quickstart-register-app.md#register-a-new-application-using-the-azure-portal) anmelden.

### <a name="audience-for-integrated-authentication-usernamepassword-and-b2c"></a>Zielgruppe für die integrierte Authentifizierung, Benutzername/Kennwort und B2C

- Wenn Sie die integrierte Windows-Authentifizierung (bei UWP-Apps möglich) oder die Authentifizierung über einen Benutzernamen und ein Kennwort verwenden möchten, müssen Benutzer von Ihrer Anwendung bei Ihrem Mandanten (LOB-Entwickler) oder bei Azure Active Directory-Organisationen (ISV-Szenario) angemeldet werden. Diese Authentifizierungsflüsse werden für persönliche Microsoft-Konten nicht unterstützt.
- Wenn Sie Benutzer mit Identitäten sozialer Netzwerke anmelden, indem Sie eine B2C-Autorität und -Richtlinie übergeben, können Sie nur die interaktive Authentifizierung oder die Authentifizierung über einen Benutzernamen und ein Kennwort verwenden. Die Authentifizierung über einen Benutzernamen und ein Kennwort wird derzeit nur für Xamarin.iOS, Xamarin.Android und UWP unterstützt.

Allgemeine Informationen finden Sie unter [Scenarios and supported authentication flows (Szenarios und unterstützte Authentifizierungsflows)](authentication-flows-app-scenarios.md#scenarios-and-supported-authentication-flows) und [Scenarios and supported platforms and languages (Szenarios sowie unterstützte Plattformen und Sprachen)](authentication-flows-app-scenarios.md#scenarios-and-supported-platforms-and-languages).

## <a name="platform-configuration-and-redirect-uris"></a>Plattformkonfiguration und Umleitungs-URIs  

### <a name="interactive-authentication"></a>Interaktive Authentifizierung

Beim Erstellen einer mobilen App mit interaktiver Authentifizierung stellt der Umleitungs-URI den wichtigsten Teil der Registrierung dar. Sie können diesen über die [Plattformkonfiguration auf dem Blatt „Authentifizierung“](https://aka.ms/MobileAppReg) festlegen.

Dadurch kann Ihre App einmaliges Anmelden (SSO) über Microsoft Authenticator (und über das Intune-Unternehmensportal für Android) nutzen und Richtlinien zur Geräteverwaltung unterstützen.

Hinweis: Im Portal für die App-Registrierung gibt es eine Vorschaufunktion, die Sie beim Berechnen des Antwort-Broker-URI für iOS- und Android-Anwendungen unterstützt:

1. Klicken Sie in der App-Registrierung erst auf **Authentifizierung** und anschließend auf **Try-out the new experience** (Neue Funktion testen).
   ![Bild](https://user-images.githubusercontent.com/13203188/60799285-2d031b00-a173-11e9-9d28-ac07a7ae894a.png)

2. Klicken Sie auf **Plattform hinzufügen**.
   ![Bild](https://user-images.githubusercontent.com/13203188/60799366-4c01ad00-a173-11e9-934f-f02e26c9429e.png)

3. Wenn die Liste der Plattformen unterstützt wird, klicken Sie auf **iOS**.
   ![Bild](https://user-images.githubusercontent.com/13203188/60799411-60de4080-a173-11e9-9dcc-d39a45826d42.png)

4. Geben Sie die Paket-ID wie angefordert ein, und klicken Sie dann auf **Registrieren**.
   ![Bild](https://user-images.githubusercontent.com/13203188/60799477-7eaba580-a173-11e9-9f8b-431f5b09344e.png)

5. Der Umleitungs-URI wird für Sie berechnet.
   ![image](https://user-images.githubusercontent.com/13203188/60799538-9e42ce00-a173-11e9-860a-015a1840fd19.png)

Wenn Sie den Umleitungs-URI stattdessen manuell konfigurieren möchten, können Sie dazu das Anwendungsmanifest verwenden. Folgende Formate werden empfohlen:

- ***iOS***: `msauth.<BUNDLE_ID>://auth` (z. B. „msauth.com.yourcompany.appName://auth“)
- ***Android:*** `msauth://<PACKAGE_NAME>/<SIGNATURE_HASH>`
  - Der Android-Signaturhash kann über den KeyTool-Befehl mit dem Release- oder Debugschlüssel generiert werden.

### <a name="username-password"></a>Benutzername/Kennwort

Wenn Ihre App ausschließlich die Authentifizierung über einen Benutzernamen und ein Kennwort verwendet, müssen Sie für Ihre Anwendung keinen Umleitungs-URI registrieren. Dieser Flow führt nämlich einen Roundtrip zum Endpunkt von Microsoft Identity Platform, Version 2.0 durch, und Ihre Anwendung wird nicht über einen bestimmten URI aufgerufen. Sie müssen jedoch angeben, dass es sich bei Ihrer Anwendung um eine öffentliche Clientanwendung handelt. Sie erreichen diese Konfiguration, indem Sie im Abschnitt **Authentifizierung** für Ihre Anwendung im Unterabschnitt **Erweiterte Einstellungen** (im Absatz **Standardclienttyp**) für die Option **Treat application as a public client** (Anwendung als öffentlichen Client einstufen) **Ja** auswählen.

## <a name="api-permissions"></a>API-Berechtigungen

Mobile Anwendungen rufen APIs für angemeldete Benutzer auf. Ihre App muss dazu delegierte Berechtigungen anfordern, die auch als Bereiche bezeichnet werden. Dieser Vorgang kann entweder statisch über das Azure-Portal oder dynamisch zur Laufzeit erfolgen. Durch die statische Registrierung von Berechtigungen können Administratoren Ihre App leicht genehmigen, weswegen diese Vorgehensweise empfohlen wird.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Codekonfiguration](scenario-mobile-app-configuration.md)
