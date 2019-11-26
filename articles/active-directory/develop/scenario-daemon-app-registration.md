---
title: Daemon-App, die Web-APIs aufruft (App-Registrierung) – Microsoft Identity Platform
description: Erfahren Sie, wie Sie eine Daemon-App erstellen, die Web-APIs aufruft – App-Registrierung
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/15/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8d0d88f72cca45280bd76ac7bb9d7a6e0a1d37fb
ms.sourcegitcommit: 98ce5583e376943aaa9773bf8efe0b324a55e58c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73175446"
---
# <a name="daemon-app-that-calls-web-apis---app-registration"></a>Daemon-App, die Web-APIs aufruft – App-Registrierung

In diesem Artikel finden Sie alle wichtigen Informationen zur Registrierung einer Daemon-Anwendung.

## <a name="supported-account-types"></a>Unterstützte Kontotypen

Da Daemon-Anwendungen nur in Azure AD-Mandanten sinnvoll sind, müssen Sie beim Erstellen der Anwendung eine der folgenden Optionen auswählen:

- **Nur Konten in diesem Organisationsverzeichnis**. Diese Option wird am häufigsten ausgewählt, da Daemon-Anwendungen in der Regel von branchenspezifischen Entwicklern geschrieben werden.
- **Konten in einem beliebigen Organisationsverzeichnis**. Wählen Sie diese Option aus, wenn Sie ein unabhängiger Softwarehersteller sind, der ein Hilfsprogramm für seine Kunden bereitstellt. Sie benötigen hierfür die Genehmigung der Mandantenadministratoren des Kunden.

## <a name="authentication---no-reply-uri-needed"></a>Authentifizierung – kein Antwort-URI erforderlich

Wenn Ihre vertrauliche Clientanwendung **nur** den Clientanmeldeinformationsfluss verwendet, muss die Antwort-URL nicht registriert werden. Sie wird weder für die Konfiguration noch für die Erstellung der Anwendung verwendet. Der Fluss mit Clientanmeldeinformationen verwendet diese nicht.

## <a name="api-permissions---app-permissions-and-admin-consent"></a>API-Berechtigungen: App-Berechtigungen und Administratoreinwilligung

Eine Daemon-Anwendung kann nur Anwendungsberechtigungen (keine delegierten Berechtigungen) für APIs anfordern. Auf der Seite **API Permission** (API-Berechtigung) der Anwendungsregistrierung klicken Sie auf **Berechtigung hinzufügen** und wählen die API-Familie aus. Anschließend klicken Sie auf **Anwendungsberechtigungen** und wählen Ihre Berechtigungen aus.

![App-Berechtigungen und Administratoreinwilligung](media/scenario-daemon-app/app-permissions-and-admin-consent.png)

> [!NOTE]
> Die Web-API, die Sie aufrufen möchten, muss **Anwendungsberechtigungen (App-Rollen)** und keine delegierten Berechtigungen definieren. Weitere Informationen zum Bereitstellen einer solchen API finden Sie unter [Geschützte Web-API: App-Registrierung – wenn Ihre Web-API von einer Daemon-App aufgerufen wird](scenario-protected-web-api-app-registration.md#if-your-web-api-is-called-by-a-daemon-app)

Bei Daemon-Anwendungen ist vorab eine Einwilligung des Mandantenadministrators in den Aufruf der Web-API durch die Anwendung erforderlich. Diese Einwilligung wird ebenfalls auf der Seite **API Permission** (API-Berechtigung) durch einen Administrator angegeben. Dieser wählt dazu **Grant admin consent to *our organization*** (Administratoreinwilligung für unser Unternehmen erteilen) aus.

Wenn Sie ein unabhängiger Softwarehersteller sind, der eine mehrinstanzenfähige Anwendung erstellt, interessiert Sie vermutlich der Abschnitt [Deployment - case of multi-tenant daemon apps (Bereitstellung: mehrinstanzenfähige Daemon-Apps)](scenario-daemon-production.md#deployment---case-of-multi-tenant-daemon-apps).

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-registration-client-secrets.md)]

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Daemon-App – App-Codekonfiguration](./scenario-daemon-app-configuration.md)
