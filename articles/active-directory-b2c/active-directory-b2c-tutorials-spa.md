---
title: 'Tutorial: Aktivieren der Authentifizierung in einer Single-Page-Webanwendung – Azure Active Directory B2C'
description: Es wird beschrieben, wie Sie Azure Active Directory B2C zum Bereitstellen einer Benutzeranmeldung für eine Single-Page-Webanwendung (JavaScript) verwenden.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.author: marsma
ms.date: 10/14/2019
ms.custom: mvc, seo-javascript-september2019
ms.topic: tutorial
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: f9e4f25c5750ae5c48130140f49f986e20adc745
ms.sourcegitcommit: 359930a9387dd3d15d39abd97ad2b8cb69b8c18b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73641773"
---
# <a name="tutorial-enable-authentication-in-a-single-page-application-using-azure-active-directory-b2c-azure-ad-b2c"></a>Tutorial: Aktivieren der Authentifizierung in einer Single-Page-Webanwendung mit Azure Active Directory B2C (Azure AD B2C)

In diesem Tutorial erfahren Sie, wie Sie Azure Active Directory B2C (Azure AD B2C) für die Anmeldung und Registrierung von Benutzern in einer Single-Page-Webanwendung (Single-Page Application, SPA) verwenden. Mit Azure AD B2C können sich Ihre Anwendungen über offene Standardprotokolle bei Konten für soziale Netzwerke sowie bei Unternehmenskonten und Azure Active Directory-Konten authentifizieren.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Aktualisieren der Anwendung in Azure AD B2C
> * Konfigurieren des Beispiels für die Verwendung der Anwendung
> * Anmelden über den Benutzerflow

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

Sie benötigen die folgenden Azure AD B2C-Ressourcen, bevor Sie mit den Schritten dieses Tutorials fortfahren können:

* [Azure AD B2C-Mandant](tutorial-create-tenant.md)
* In Ihrem Mandanten [registrierte Anwendung](tutorial-register-applications.md)
* Im Mandanten [erstellte Benutzerabläufe](tutorial-create-user-flows.md)

Darüber hinaus benötigen Sie in Ihrer lokalen Entwicklungsumgebung Folgendes:

* Code-Editor, z. B. [Visual Studio Code](https://code.visualstudio.com/) oder [Visual Studio 2019](https://www.visualstudio.com/downloads/)
* [.NET Core SDK 2.2](https://dotnet.microsoft.com/download) oder höher
* [Node.js](https://nodejs.org/en/download/)

## <a name="update-the-application"></a>Aktualisieren der Anwendung

Im zweiten Tutorial, das Sie zur Vorbereitung absolviert haben, wurde eine Webanwendung unter Azure AD B2C registriert. Um die Kommunikation mit dem Beispiel im Tutorial zu aktivieren, müssen Sie der Anwendung in Azure AD B2C einen Umleitungs-URI hinzufügen.

Sie können die aktuelle Benutzeroberfläche für **Anwendungen** oder unsere neue einheitliche Benutzeroberfläche **App-Registrierungen (Vorschau)** verwenden, um die Anwendung zu aktualisieren. Weitere Informationen zu der neuen Oberfläche finden Sie [hier](https://aka.ms/b2cappregintro).

#### <a name="applicationstabapplications"></a>[Anwendungen](#tab/applications/)

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Stellen Sie sicher, dass Sie das Verzeichnis verwenden, das Ihren Azure AD B2C-Mandanten enthält, indem Sie im oberen Menü auf den **Verzeichnis- und Abonnementfilter** klicken und das entsprechende Verzeichnis auswählen.
1. Wählen Sie links oben im Azure-Portal die Option **Alle Dienste** aus, suchen Sie nach **Azure AD B2C**, und wählen Sie dann diese Option aus.
1. Wählen Sie **Anwendungen** und anschließend die Anwendung *webapp1* aus.
1. Fügen Sie unter **Antwort-URL** Folgendes hinzu: `http://localhost:6420`.
1. Wählen Sie **Speichern** aus.
1. Notieren Sie sich auf der Eigenschaftenseite die **Anwendungs-ID**. Sie verwenden die App-ID in einem späteren Schritt, wenn Sie den Code in der Single-Page-Webanwendung aktualisieren.

#### <a name="app-registrations-previewtabapp-reg-preview"></a>[App-Registrierungen (Vorschau)](#tab/app-reg-preview/)

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im oberen Menü den Filter **Verzeichnis und Abonnement** aus, und wählen Sie dann das Verzeichnis aus, das Ihren Azure AD B2C-Mandanten enthält.
1. Wählen Sie im linken Menü die Option **Azure AD B2C** aus. Oder wählen Sie **Alle Dienste** aus, suchen Sie nach dem Eintrag **Azure AD B2C**, und wählen Sie ihn aus.
1. Wählen Sie **App-Registrierungen (Vorschau)** , die Registerkarte **Anwendungen mit Besitzer** und dann die Anwendung *webapp1* aus.
1. Wählen Sie **Authentifizierung** und dann **Neue Benutzeroberfläche ausprobieren** aus (sofern die Option angezeigt wird).
1. Wählen Sie unter **Web** den Link **URI hinzufügen** aus, geben Sie `http://localhost:6420` ein, und wählen Sie dann **Speichern** aus.
1. Wählen Sie **Übersicht**.
1. Notieren Sie sich die **Anwendungs-ID (Client)** zur Verwendung in einem späteren Schritt, wenn Sie den Code in der Single-Page-Webanwendung aktualisieren.

* * *

## <a name="get-the-sample-code"></a>Laden Sie den Beispielcode herunter

In diesem Tutorial konfigurieren Sie ein Codebeispiel, das Sie von GitHub herunterladen können. Das Beispiel veranschaulicht, wie eine Single-Page-Webanwendung Azure AD B2C für die Benutzerregistrierung und -anmeldung sowie zum Aufrufen einer geschützten Web-API verwenden kann.

[Laden Sie eine ZIP-Datei herunter](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp/archive/master.zip), oder klonen Sie das Beispiel aus GitHub.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp.git
```

## <a name="update-the-sample"></a>Aktualisieren des Beispiels

Nachdem Sie nun das Beispiel vorliegen haben, können Sie den Code mit dem Namen Ihres Azure AD B2C-Mandanten und der Anwendungs-ID aus dem vorherigen Schritt aktualisieren.

1. Öffnen Sie die Datei `index.html` im Stamm des Beispielverzeichnisses.
1. Ändern Sie in der `msalConfig`-Definition den Wert **clientId** mit der Anwendungs-ID, die Sie in einem vorherigen Schritt notiert haben. Aktualisieren Sie als Nächstes den URI-Wert **authority** mit dem Namen Ihres Azure AD B2C-Mandanten. Aktualisieren Sie auch den URI mit dem Namen des Benutzerablaufs für die Registrierung bzw. Anmeldung, den Sie im Rahmen der Voraussetzungen erstellt haben (z. B. *B2C_1_signupsignin1*).

    ```javascript
    var msalConfig = {
        auth: {
            clientId: "00000000-0000-0000-0000-000000000000", //This is your client ID
            authority: "https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/b2c_1_susi", //This is your tenant info
            validateAuthority: false
        },
        cache: {
            cacheLocation: "localStorage",
            storeAuthStateInCookie: true
        }
    };
    ```

    Der Name des in diesem Tutorial verwendeten Benutzerflows lautet **B2C_1_signupsignin1**. Wenn Sie einen anderen Namen für den Benutzerablauf verwenden, müssen Sie ihn unter dem Wert `authority` angeben.

## <a name="run-the-sample"></a>Ausführen des Beispiels

1. Öffnen Sie ein Konsolenfenster, und wechseln Sie in das Verzeichnis mit dem Beispiel. Beispiel:

    ```console
    cd active-directory-b2c-javascript-msal-singlepageapp
    ```
1. Führen Sie die folgenden Befehle aus:

    ```
    npm install && npm update
    node server.js
    ```

    Im Konsolenfenster wird die Portnummer des lokal ausgeführten Node.js-Servers angezeigt:

    ```
    Listening on port 6420...
    ```

1. Navigieren Sie in Ihrem Browser zu `http://localhost:6420`, um die Anwendung anzuzeigen.

Das Beispiel unterstützt Registrierung, Anmeldung, Profilbearbeitung und Kennwortzurücksetzung. In diesem Tutorial wird beschrieben, wie sich ein Benutzer mit einer E-Mail-Adresse anmeldet.

### <a name="sign-up-using-an-email-address"></a>Registrieren mit einer E-Mail-Adresse

> [!WARNING]
> Nach der Registrierung oder Anmeldung wird ggf. ein [Fehler aufgrund unzureichender Berechtigungen](#error-insufficient-permissions) angezeigt. Aufgrund der derzeitigen Implementierung des Codebeispiels ist dieser Fehler zu erwarten. Dieses Problem wird in einer zukünftigen Version des Codebeispiels behoben, damit diese Warnung nicht mehr angezeigt wird.

1. Wählen Sie **Anmelden** aus, um den in einem vorherigen Schritt angegebenen Benutzerflow *B2C_1_signupsignin1* zu initiieren.
1. Azure AD B2C zeigt eine Anmeldeseite mit einem Registrierungslink an. Da Sie noch nicht über ein Konto verfügen, klicken Sie auf den Link **Jetzt registrieren**.
1. Der Registrierungsworkflow zeigt eine Seite an, über die die Identität des Benutzers mithilfe einer E-Mail-Adresse erfasst und überprüft wird. Darüber hinaus werden im Rahmen des Registrierungsworkflows auch das Kennwort des Benutzers sowie die angeforderten Attribute erfasst, die im Benutzerflow definiert wurden.

    Verwenden Sie eine gültige E-Mail-Adresse, und bestätigen Sie sie mithilfe des Prüfcodes. Legen Sie ein Kennwort fest. Geben Sie Werte für die angeforderten Attribute ein.

    ![Registrierungsseite im Benutzerflow für Anmeldung/Registrierung](./media/active-directory-b2c-tutorials-spa/azure-ad-b2c-sign-up-workflow.png)

1. Wählen Sie **Erstellen** aus, um im Azure AD B2C-Verzeichnis ein lokales Konto zu erstellen.

Wenn Sie **Erstellen** auswählen, wird die Registrierungsseite geschlossen und wieder die Anmeldeseite angezeigt.

Sie können jetzt Ihre E-Mail-Adresse und das zugehörige Kennwort nutzen, um sich an der Anwendung anzumelden.

### <a name="error-insufficient-permissions"></a>Fehler: Unzureichende Berechtigungen

Nachdem Sie sich angemeldet haben, gibt die Anwendung ggf. einen Fehler aufgrund unzureichender Berechtigungen zurück:

```Output
ServerError: AADB2C90205: This application does not have sufficient permissions against this web resource to perform the operation.
Correlation ID: ce15bbcc-0000-0000-0000-494a52e95cd7
Timestamp: 2019-07-20 22:17:27Z
```

Dieser Fehler wird angezeigt, weil die Webanwendung versucht, auf eine durch das Demoverzeichnis *fabrikamb2c* geschützte Web-API zuzugreifen. Da Ihr Zugriffstoken nur für Ihr Azure AD-Verzeichnis gilt, ist der API-Aufruf nicht autorisiert.

Fahren Sie zum Beheben des Fehlers mit dem nächsten Tutorial der Reihe fort (siehe [Nächste Schritte](#next-steps)), um eine geschützte Web-API für Ihr Verzeichnis zu erstellen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Aktualisieren der Anwendung in Azure AD B2C
> * Konfigurieren des Beispiels für die Verwendung der Anwendung
> * Anmelden über den Benutzerflow

Fahren Sie jetzt mit dem nächsten Tutorial der Reihe fort, um über die Single-Page-Webanwendung Zugriff auf eine geschützte Web-API zu gewähren:

> [!div class="nextstepaction"]
> [Tutorial: Gewähren des Zugriffs auf eine ASP.NET Core-Web-API über eine Single-Page-Webanwendung mithilfe von Azure Active Directory B2C >](active-directory-b2c-tutorials-spa-webapi.md)
