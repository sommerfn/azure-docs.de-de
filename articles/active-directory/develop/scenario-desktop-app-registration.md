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
ms.date: 09/09/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: b996b2387e324c7e318536c2a13bdc9de39a7a5e
ms.sourcegitcommit: 65131f6188a02efe1704d92f0fd473b21c760d08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/10/2019
ms.locfileid: "70860877"
---
# <a name="desktop-app-that-calls-web-apis---app-registration"></a>Desktop-App, die Web-APIs aufruft – App-Registrierung

In diesem Artikel sind die Besonderheiten zur App-Registrierung bei einer Desktopanwendung aufgeführt.

## <a name="supported-accounts-types"></a>Unterstützte Kontotypen

Die in der Desktopanwendung unterstützten Kontotypen sind von der gewünschten Funktion abhängig. Aufgrund dieser Beziehung sind die unterstützten Kontotypen von den Flows abhängig, die Sie verwenden möchten.

### <a name="audience-for-interactive-token-acquisition"></a>Zielgruppe für den interaktiven Tokenabruf

Wenn Ihre Desktopanwendung die interaktive Authentifizierung verwendet, können Sie Benutzer jedes [Kontotyps](quickstart-register-app.md#register-a-new-application-using-the-azure-portal) anmelden.

### <a name="audience-for-desktop-app-silent-flows"></a>Zielgruppe für automatische Desktop-App-Flüsse

- Um die integrierte Windows-Authentifizierung oder Benutzername und Kennwort verwenden zu können, müssen Benutzer von Ihrer Anwendung bei Ihrem Mandanten (LOB-Entwickler) oder bei Azure Active Directory-Organisationen (ISV-Szenario) angemeldet werden. Diese Authentifizierungsflows werden für persönliche Microsoft-Konten nicht unterstützt.
- Wenn Sie den Gerätecodeflow verwenden möchten, können Sie Benutzer noch nicht mit ihren persönlichen Microsoft-Konten anmelden.
- Wenn Sie Benutzer mit Identitäten sozialer Netzwerke anmelden, indem Sie eine B2C-Autorität und -Richtlinie übergeben, können Sie ausschließlich die interaktive Authentifizierung oder die Authentifizierung über Benutzername und Kennwort verwenden.

## <a name="redirect-uris"></a>Umleitungs-URIs

Die in Desktopanwendungen zu verwendenden Umleitungs-URIs hängen von dem Flow ab, den Sie verwenden möchten.

- Wenn Sie die **interaktive Authentifizierung** oder **Gerätecodeflow** verwenden, sollten Sie `https://login.microsoftonline.com/common/oauth2/nativeclient` verwenden. Diese Konfiguration erreichen Sie, indem Sie im Abschnitt **Authentifizierung** für Ihre Anwendung auf die entsprechende URL klicken.
  
  > [!IMPORTANT]
  > Aktuell verwendet MSAL.NET standardmäßig einen anderen Umleitungs-URI in Desktopanwendungen, die unter Windows ausgeführt werden (`urn:ietf:wg:oauth:2.0:oob`). In Zukunft möchten wir diese Standardeinstellung ändern, weshalb wir Ihnen die Verwendung von `https://login.microsoftonline.com/common/oauth2/nativeclient` empfehlen.

- Wenn Ihre App nur die integrierte Windows-Authentifizierung oder Benutzername und Kennwort verwendet, müssen Sie für Ihre Anwendung keinen Umleitungs-URI registrieren. Diese Flows führen einen Roundtrip zum Endpunkt von Microsoft Identity Platform v2.0 aus, und Ihre Anwendung wird nicht über einen bestimmten URI aufgerufen.
- Damit Gerätecodeflow, integrierte Windows-Authentifizierung und Benutzername/Kennwort von einem vertraulichen Clientanwendungsflow unterschieden werden können, der ebenfalls über keine Umleitungs-URIs verfügt (der bei Daemon-Anwendungen verwendete Client-Anmeldeinformationsflow), müssen Sie ausdrücken, dass es sich bei Ihrer Anwendung um eine öffentliche Clientanwendung handelt. Um diese Konfiguration zu erreichen, wechseln Sie zum Abschnitt **Authentifizierung** für Ihre Anwendung. Wählen Sie dann im Unterabschnitt **Erweiterte Einstellungen** im Abschnitt **Standardclienttyp** neben **Hiermit wird eine Anwendung als öffentlicher Client eingestuft** die Option **Ja** aus.

  ![Zulassen eines öffentlichen Clients](media/scenarios/default-client-type.png)

## <a name="api-permissions"></a>API-Berechtigungen

Desktopanwendungen rufen APIs für den angemeldeten Benutzer auf. Sie müssen delegierte Berechtigungen anfordern. Sie können jedoch keine Anwendungsberechtigungen anfordern. Diese werden nur in [Daemon-Anwendungen](scenario-daemon-overview.md) verarbeitet.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Desktop-App – App-Konfiguration](scenario-desktop-app-configuration.md)
