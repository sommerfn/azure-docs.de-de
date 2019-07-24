---
title: Informationen zur Interoperabilität von Anwendungen mit Azure AD B2C mithilfe von Microsoft Authentication Library (MSAL)
description: Microsoft Authentication Library (MSAL) gibt Anwendungen die Möglichkeit, mit Azure AD B2C zusammenzuarbeiten und Token zum Aufrufen gesicherter Web-APIs abzurufen. Bei diesen Web-APIs kann es sich um Microsoft Graph, andere Microsoft-APIs, Web-APIs von Dritten oder um Ihre eigene Web-API handeln.
services: active-directory
documentationcenter: dev-center-name
author: negoe
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/04/2019
ms.author: negoe
ms.reviewer: nacanuma
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: db05f59faf945e425761fe7a20bad3e263246a39
ms.sourcegitcommit: fa45c2bcd1b32bc8dd54a5dc8bc206d2fe23d5fb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2019
ms.locfileid: "67849342"
---
# <a name="use-microsoft-authentication-library-to-interoperate-with-azure-active-directory-b2c"></a>Verwenden von Microsoft Authentication Library (MSAL) für die Interoperabilität mit Azure Active Directory B2C

Microsoft Authentication Library (MSAL) ermöglicht es Anwendungsentwicklern, Benutzer mit Social Media-Identitäten und lokalen Identitäten mithilfe von [Azure Active Directory B2C (Azure AD B2C)](https://docs.microsoft.com/azure/active-directory-b2c/) zu authentifizieren. Azure AD B2C ist ein Identitätsverwaltungsdienst. Mit diesem Dienst können Sie anpassen und steuern, wie sich Kunden bei Nutzung Ihrer App registrieren und anmelden sowie ihre Profile verwalten.

Azure AD B2C ermöglicht Ihnen auch das Branding und Anpassen der Benutzeroberfläche Ihrer Anwendungen, um Ihren Kunden eine nahtlose Umgebung zu bieten.

In diesem Tutorial wird veranschaulicht, wie Sie MSAL für die Zusammenarbeit mit Azure AD B2C einsetzen.

## <a name="prerequisites"></a>Voraussetzungen

Erstellen Sie einen eigenen [Azure AD B2C-Mandanten](https://docs.microsoft.com/azure/active-directory-b2c/tutorial-create-tenant), falls dies noch nicht geschehen ist. Sie können auch einen vorhandenen Azure AD B2C-Mandanten verwenden.

## <a name="javascript"></a>JavaScript

Die folgenden Schritte veranschaulichen, wie eine Single-Page-Anwendung Azure AD B2C zum Registrieren und Anmelden sowie zum Aufrufen einer geschützten Web-API verwenden kann.

### <a name="step-1-register-your-application"></a>Schritt 1: Anwendung registrieren

Sie müssen zuerst Ihre Anwendung registrieren, um die Authentifizierung zu implementieren. Ausführliche Schritte hierzu finden Sie unter [Registrieren Ihrer Anwendung](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp#step-4-register-your-own-web-application-with-azure-ad-b2c).

### <a name="step-2-download-the-sample-application"></a>Schritt 2: Herunterladen der Beispielanwendung

Laden Sie die Beispielanwendung als ZIP-Datei herunter, oder klonen Sie sie aus GitHub:

```
git clone https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp.git
```

### <a name="step-3-configure-authentication"></a>Schritt 3: Konfigurieren der Authentifizierung

1. Öffnen Sie die Datei **index.html** im Beispiel.

1. Konfigurieren Sie das Beispiel mit der Anwendungs-ID und dem Schlüssel, die Sie zuvor bei der Registrierung Ihrer Anwendung notiert haben. Ändern Sie die folgenden Codezeilen. Ersetzen Sie dabei die Werte durch die Namen Ihres Verzeichnisses und Ihre APIs:

   ```javascript
   // The current application coordinates were pre-registered in a B2C directory.

   const msalConfig = {
       auth:{
           clientId: "Enter_the_Application_Id_here",
           authority: "https://login.microsoftonline.com/tfp/<your-tenant-name>.onmicrosoft.com/<your-sign-in-sign-up-policy>",
           b2cScopes: ["https://<your-tenant-name>.onmicrosoft.com/hello/demo.read"],
           webApi: 'http://localhost:5000/hello',
     };

   // create UserAgentApplication instance
   const myMSALObj = new UserAgentApplication(msalConfig);
   ```

Der Name des in diesem Tutorial verwendeten [Benutzerflows](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-policies) lautet **B2C_1_signupsignin1**. Wenn Sie einen anderen Benutzerflownamen verwenden, geben Sie diesen als Wert für **authority** an.

### <a name="step-4-configure-your-application-to-use-b2clogincom"></a>Schritt 4: Konfigurieren einer Anwendung für die Verwendung von `b2clogin.com`

Sie können `b2clogin.com` anstelle von `login.microsoftonline.com` als Umleitungs-URL verwenden. Diese Angabe machen Sie in Ihrer Azure AD B2C-Anwendung, wenn Sie einen Identitätsanbieter für Registrierung und Anmeldung einrichten.

Die Verwendung von `b2clogin.com` im Kontext von `https://your-tenant-name.b2clogin.com/your-tenant-guid` hat die folgenden Auswirkungen:

- Microsoft-Dienste nehmen im Cookie-Header weniger Platz in Anspruch.
- Ihre URLs enthalten keinen Verweis mehr auf Microsoft. Beispiel: Möglicherweise verweist Ihre Azure AD B2C-Anwendung auf `login.microsoftonline.com`.

 Um `b2clogin.com` nutzen zu können, müssen Sie die Konfiguration Ihrer Anwendung aktualisieren.  

- Legen Sie die Eigenschaft **validateAuthority** auf `false` fest, sodass Umleitungen mit `b2clogin.com` verwendet werden können.

Das folgende Beispiel zeigt eine Möglichkeit, wie Sie die Eigenschaft festlegen können:

```javascript
// The current application coordinates were pre-registered in a B2C directory.

const msalConfig = {
    auth:{
        clientId: "Enter_the_Application_Id_here",
        authority: "https://contoso.b2clogin.com/tfp/contoso.onmicrosoft.com/B2C_1_signupsignin1",
        b2cScopes: ["https://contoso.onmicrosoft.com/demoapi/demo.read"],
        webApi: 'https://contosohello.azurewebsites.net/hello',
        validateAuthority: false;

};
// create UserAgentApplication instance
const myMSALObj = new UserAgentApplication(msalConfig);
```

> [!NOTE]
> Die Azure AD B2C-Anwendung verweist wahrscheinlich an mehreren Stellen auf `login.microsoftonline.com`, z.B. in Ihren Benutzerflowverweisen und Tokenendpunkten. Vergewissern Sie sich, dass Ihr Autorisierungsendpunkt, Ihr Tokenendpunkt und Ihr Aussteller aktualisiert wurden und `your-tenant-name.b2clogin.com` verwenden.

Führen Sie [dieses MSAL-JavaScript-Beispiel](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp#single-page-application-built-on-msaljs-with-azure-ad-b2c) aus, um zu erfahren, wie Sie die Vorschauversion von MSAL für JavaScript (MSAL.js) verwenden. Im Beispiel wird ein Zugriffstoken abgerufen und eine durch Azure AD B2C gesicherte API aufgerufen.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen:

- [Benutzerdefinierte Richtlinien](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview-custom)
- [Anpassung der Benutzeroberfläche](https://docs.microsoft.com/azure/active-directory-b2c/customize-ui-overview)