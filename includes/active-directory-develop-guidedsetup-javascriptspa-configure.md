---
title: include file
description: include file
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/17/2018
ms.author: nacanuma
ms.custom: include file
ms.openlocfilehash: 050bae64a62e90bdb74c93948109e255b69d7518
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "67132946"
---
## <a name="register-your-application"></a>Anwendung registrieren

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

1. Wenn Sie in Ihrem Konto auf mehrere Mandanten zugreifen können, wählen Sie rechts oben das Konto aus, und legen Sie Ihre Portalsitzung auf den gewünschten Azure AD-Mandanten fest.
1. Navigieren Sie auf der Microsoft Identity Platform für Entwickler zur Seite [App-Registrierungen](https://go.microsoft.com/fwlink/?linkid=2083908).
1. Geben Sie auf der daraufhin angezeigten Seite **Anwendung registrieren** einen Namen für Ihre Anwendung ein.
1. Wählen Sie unter **Unterstützte Kontotypen** **Konten in allen Organisationsverzeichnissen und persönliche Microsoft-Konten** aus.
1. Wählen Sie im Abschnitt **Umleitungs-URI** in der Dropdownliste die Plattform **Web** aus, und legen Sie den Wert auf die URL der Anwendung basierend auf Ihrem Webserver fest. 

   Informationen zum Festlegen und Abrufen der Umleitungs-URL in Visual Studio und Node.js finden Sie in den nächsten beiden Abschnitten.

1. Wählen Sie **Registrieren**.
1. Notieren Sie sich für die spätere Verwendung auf der Seite **Übersicht** den Wert von **Anwendungs-ID (Client)** .
1. Für diesen Schnellstart muss der [Flow zur impliziten Genehmigung](../articles/active-directory/develop/v2-oauth2-implicit-grant-flow.md) aktiviert werden. Wählen Sie im linken Bereich der registrierten Anwendung die Option **Authentifizierung** aus.
1. Aktivieren Sie unter **Erweiterte Einstellungen** und **Implizite Gewährung** die Kontrollkästchen **ID-Token** und **Zugriffstoken**. ID- und Zugriffstoken sind erforderlich, da diese App Benutzer anmelden und eine API aufrufen muss.
1. Wählen Sie **Speichern** aus.

> #### <a name="set-a-redirect-url-for-nodejs"></a>Festlegen einer Umleitungs-URL für Node.js
> Für Node.js können Sie den Webserverport in der Datei *server.js* festlegen. In diesem Tutorial wird zur Referenz Port 30662 genutzt, aber Sie können auch einen beliebigen anderen verfügbaren Port verwenden. 
>
> Wenn Sie eine Umleitungs-URL in den Registrierungsinformationen der Anwendung einrichten möchten, kehren Sie zum Bereich **Anwendungsregistrierung** zurück und führen eine der folgenden Aktionen aus:
>
> - Legen Sie *`http://localhost:30662/`* als **Umleitungs-URL** fest.
> - Wenn Sie einen benutzerdefinierten TCP-Port nutzen, verwenden Sie *`http://localhost:<port>/`* (wobei *\<port>* die benutzerdefinierte TCP-Portnummer ist).
>
> #### <a name="set-a-redirect-url-for-visual-studio"></a>Festlegen einer Umleitungs-URL für Visual Studio
> Um die Umleitungs-URL für Visual Studio zu erhalten, führen Sie folgende Schritte aus:
> 1. Wählen Sie das Projekt im **Projektmappen-Explorer** aus.
>
>    Das Fenster **Eigenschaften** wird geöffnet. Falls es nicht geöffnet wird, drücken Sie **F4**.
>
>    ![Das Eigenschaftenfenster des JavaScriptSPA-Projekts](media/active-directory-develop-guidedsetup-javascriptspa-configure/vs-project-properties-screenshot.png)
>
> 1. Kopieren Sie den Wert von **URL**.
 
> 1. Kehren Sie zurück zum Bereich **Anwendungsregistrierung**, und fügen Sie den kopierten Wert als **Umleitungs-URL** ein.

#### <a name="configure-your-javascript-spa"></a>Konfigurieren Ihrer JavaScript-SPA

1. Fügen Sie in der Datei *index.html*, die beim Einrichten des Projekts erstellt wurde, die Informationen für die Anwendungsregistrierung hinzu. Fügen Sie am Anfang der Datei zwischen den `<script></script>`-Tags den folgenden Code hinzu.

    ```javascript
    var msalConfig = {
        auth: {
            clientId: "<Enter_the_Application_Id_here>",
            authority: "https://login.microsoftonline.com/<Enter_the_Tenant_info_here>"
        },
        cache: {
            cacheLocation: "localStorage",
            storeAuthStateInCookie: true
        }
    };
    ```

    Hinweis:
    - *\<Enter_the_Application_Id_here>* ist die **Anwendungs-ID (Client)** für die im Azure-Portal registrierte Anwendung.
    - *\<Enter_the_Tenant_info_here >* wird auf eine der folgenden Optionen festgelegt:
       - Falls Ihre Anwendung *nur Konten in diesem Organisationsverzeichnis* unterstützt, ersetzen Sie diesen Wert durch die **Mandanten-ID** oder den **Mandantennamen** (beispielsweise *contoso.microsoft.com*).
       - Wenn Ihre Anwendung *Konten in einem beliebigen Organisationsverzeichnis* unterstützt, ersetzen Sie diesen Wert durch **organizations**.
       - Wenn Ihre Anwendung *Konten in allen Organisationsverzeichnissen und persönliche Microsoft-Konten* unterstützt, ersetzen Sie diesen Wert durch **common**. Wenn Sie die Unterstützung *ausschließlich auf persönliche Microsoft-Konten* beschränken möchten, ersetzen Sie diesen Wert durch **consumers**.
