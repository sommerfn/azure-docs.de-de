---
title: Desktop-App, die Web-APIs aufruft (App-Registrierung) – Microsoft Identity Platform
description: Erfahren Sie, wie Sie eine Desktop-App erstellen, die Web-APIs aufruft (App-Registrierung)
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/18/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5ab2701a82da0b8f7bc4e23a3d947be905593e85
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "67057215"
---
# <a name="desktop-app-that-calls-web-apis---app-registration"></a>Desktop-App, die Web-APIs aufruft – App-Registrierung

In diesem Artikel sind die Besonderheiten zur App-Registrierung bei einer Desktopanwendung aufgeführt.

## <a name="supported-accounts-types"></a>Unterstützte Kontotypen

Die in der Desktopanwendung unterstützten Kontotypen sind abhängig von der gewünschten Funktion und somit von den Flüssen, die Sie verwenden möchten.

### <a name="audience-for-interactive-token-acquisition"></a>Zielgruppe für den interaktiven Tokenabruf

Wenn Ihre Desktopanwendung die interaktive Authentifizierung verwendet, können Sie Benutzer jedes [Kontotyps](quickstart-register-app.md#register-a-new-application-using-the-azure-portal) anmelden.

### <a name="audience-for-desktop-app-silent-flows"></a>Zielgruppe für automatische Desktop-App-Flüsse

- Wenn Sie die integrierte Windows-Authentifizierung oder Benutzername und Kennwort verwenden möchten, müssen Benutzer von Ihrer Anwendung in Ihrem eigenen Mandanten (LOB-Entwickler) oder in Azure Active Directory-Unternehmen (ISV-Szenario) angemeldet werden. Diese Authentifizierungsflüsse werden für persönliche Microsoft-Konten nicht unterstützt.
- Wenn Sie den Fluss mit Gerätecode verwenden möchten, können Sie Benutzer bisher nicht mit ihrem persönlichen Microsoft-Konto anmelden.
- Wenn Sie Benutzer mit Identitäten sozialer Netzwerke anmelden, indem Sie eine B2C-Autorität und -Richtlinie übergeben, können Sie ausschließlich die interaktive Authentifizierung oder die Authentifizierung über Benutzername und Kennwort verwenden.

## <a name="redirect-uris"></a>Umleitungs-URIs

Die zu verwendenden Umleitungs-URIs hängen ebenfalls von dem Fluss ab, den Sie verwenden möchten.

- Wenn Sie die **interaktive Authentifizierung** oder **Gerätecodeflow** verwenden, sollten Sie `https://login.microsoftonline.com/common/oauth2/nativeclient` verwenden. Diese Konfiguration erreichen Sie, indem Sie im Abschnitt **Authentifizierung** für Ihre Anwendung auf die entsprechende URL klicken.
  
  > [!IMPORTANT]
  > Aktuell verwendet MSAL.NET standardmäßig einen anderen Umleitungs-URI in Desktopanwendungen, die unter Windows ausgeführt werden (`urn:ietf:wg:oauth:2.0:oob`). In Zukunft möchten wir diese Standardeinstellung ändern, weshalb wir Ihnen die Verwendung von `https://login.microsoftonline.com/common/oauth2/nativeclient` empfehlen.

- Wenn Ihre App ausschließlich die integrierte Windows-Authentifizierung mit Benutzername und Kennwort verwendet, müssen Sie für Ihre Anwendung keinen Umleitungs-URI registrieren. Diese Flüsse führen nämlich tatsächlich einen Roundtrip zum Endpunkt von Microsoft Identity Platform v2.0 durch, und Ihre Anwendung wird nicht über einen bestimmten URI aufgerufen. 
- Damit Gerätecodeflow, integrierte Windows-Authentifizierung und Benutzername/Kennwort von einem vertraulichen Clientanwendungsflow unterschieden werden können, der ebenfalls über keine Umleitungs-URIs verfügt (der bei Daemon-Anwendungen verwendete Clientanwendungsflow), müssen Sie ausdrücken, dass es sich bei Ihrer Anwendung um eine öffentliche Clientanwendung handelt. Sie erreichen diese Konfiguration, indem Sie im Abschnitt **Authentifizierung** für Ihre Anwendung im Unterabschnitt **Erweiterte Einstellungen** (im Absatz **Standardclienttyp**) für die Option **Treat application as a public client** (Anwendung als öffentlichen Client einstufen) **Ja** auswählen.

  ![Zulassen eines öffentlichen Clients](media/scenarios/default-client-type.png)

## <a name="api-permissions"></a>API-Berechtigungen

Desktopanwendungen rufen APIs im Namen des angemeldeten Benutzers auf. Sie müssen delegierte Berechtigungen anfordern. Sie können keine Anwendungsberechtigungen anfordern (die nur in [Daemon-Anwendungen](scenario-daemon-overview.md) verarbeitet werden).

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Desktop-App – App-Konfiguration](scenario-desktop-app-configuration.md)
