---
title: Anpassen der Benutzeroberfläche einer Anwendung mithilfe einer benutzerdefinierten Richtlinie in Azure Active Directory B2C | Microsoft-Dokumentation
description: Erfahren Sie, wie eine Benutzeroberfläche mithilfe einer benutzerdefinierten Richtlinie in Azure Active Directory B2C angepasst werden kann.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/11/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 1ef4ddc422041de623b96f3a0c85f067427cacd7
ms.sourcegitcommit: 0576bcb894031eb9e7ddb919e241e2e3c42f291d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "72374224"
---
# <a name="customize-the-user-interface-of-your-application-using-a-custom-policy-in-azure-active-directory-b2c"></a>Anpassen der Benutzeroberfläche einer Anwendung mithilfe einer benutzerdefinierten Richtlinie in Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Nach dem Durcharbeiten dieses Artikels haben Sie eine benutzerdefinierte Registrierungs- oder Anmelderichtlinie mit Ihrer Marke und in Ihrem Design erstellt. Mit Azure Active Directory B2C (Azure AD B2C) erlangen Sie nahezu vollständige Kontrolle über den HTML- und CSS-Inhalt, der Benutzern angezeigt wird. Bei Verwendung einer benutzerdefinierten Richtlinie konfigurieren Sie die Anpassung der Benutzeroberfläche in XML, anstatt über Steuerelemente im Azure-Portal.

## <a name="prerequisites"></a>Voraussetzungen

Führen Sie die Schritte unter [Erste Schritte mit benutzerdefinierten Richtlinien in Azure Active Directory B2C](active-directory-b2c-get-started-custom.md) aus. Sie sollten eine funktionierende benutzerdefinierte Richtlinie für die Registrierung und Anmeldung bei lokalen Konten besitzen.

## <a name="page-ui-customization"></a>Seite für die Benutzeroberflächenanpassung

Mit der Funktion zum Anpassen der Benutzeroberfläche können Sie das Aussehen und Verhalten benutzerdefinierter Richtlinien anpassen. Außerdem können Sie die Konsistenz von Marken und visuellen Elementen zwischen Ihrer Anwendung und Azure AD B2C verwalten.

Dies funktioniert folgendermaßen: Azure AD B2C führt den Code im Browser Ihres Kunden aus und verwendet einen modernen Ansatz namens [Cross-Origin Resource Sharing](https://www.w3.org/TR/cors/) (CORS, Ressourcenfreigabe zwischen verschiedenen Ursprüngen). Zuerst legen Sie eine URL in der benutzerdefinierten Richtlinie mit benutzerdefiniertem HTML-Inhalt fest. Azure AD B2C führt die UI-Elemente mit dem HTML-Inhalt, der über Ihre URL geladen wird, zusammen und zeigt anschließend die Seite für den Kunden an.

## <a name="create-your-html5-content"></a>Erstellen des HTML5-Inhalts

Erstellen Sie den HTML-Inhalt mit dem Markennamen Ihres Produkts im Titel.

1. Kopieren Sie den folgenden HTML-Codeausschnitt. Es handelt sich um wohlgeformten HTML5-Code mit dem leeren Element *\<div id="api"\>\</div\>* , das in *\<body\>* -Tags enthalten ist. Dieses Element gibt an, wo Azure AD B2C-Inhalt eingefügt werden soll.

   ```html
   <!DOCTYPE html>
   <html>
   <head>
       <title>My Product Brand Name</title>
   </head>
   <body>
       <div id="api"></div>
   </body>
   </html>
   ```

1. Fügen Sie den kopierten Codeausschnitt in einen Text-Editor ein, und speichern Sie die Datei dann unter dem Namen *customize-ui.html*.

> [!NOTE]
> HTML-Formularelemente werden bei der Verwendung von login.microsoftonline.com aufgrund von Sicherheitseinschränkungen entfernt. Nutzen Sie b2clogin.com, wenn Sie HTML-Formularelemente in Ihren benutzerdefinierten HTML-Inhalten verwenden möchten. Weitere Vorteile finden Sie unter [Use b2clogin.com](b2clogin.md).

## <a name="create-an-azure-blob-storage-account"></a>Erstellen eines Azure Blob Storage-Kontos

>[!NOTE]
> In diesem Artikel wird Azure Blob Storage zum Hosten unserer Inhalte verwendet. Sie können angeben, dass Ihre Inhalte auf einem Webserver gehostet werden sollen, aber Sie müssen [auf Ihrem Webserver CORS aktivieren](https://enable-cors.org/server.html).

Führen Sie die folgenden Schritte aus, um diesen HTML-Inhalt im Blob-Speicher zu hosten:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im Menü **Hub** die Option **Neu** > **Speicher** > **Speicherkonto**.
1. Wählen Sie ein **Abonnement** für Ihr Speicherkonto aus.
1. Erstellen Sie eine **Ressourcengruppe**, oder wählen Sie eine vorhandene Ressourcengruppe aus.
1. Geben Sie einen eindeutigen **Namen** für Ihr Speicherkonto ein.
1. Wählen Sie den **geografischen Standort** für das Speicherkonto aus.
1. Als **Bereitstellungsmodell** können Sie **Resource Manager** unverändert lassen.
1. Für **Leistung** können Sie **Standard** übernehmen.
1. Ändern Sie **Kontoart** in **Blob Storage**.
1. Für **Replikation** können Sie **RA-GRS** übernehmen.
1. Für **Zugriffstarif** können Sie **Heiß** übernehmen.
1. Klicken Sie auf **Überprüfen + erstellen**, um das Speicherkonto zu erstellen.
    Nach Abschluss der Bereitstellung wird die Seite **Speicherkonto** automatisch geöffnet.

## <a name="create-a-container"></a>Erstellen eines Containers

Führen Sie die folgenden Schritte aus, um im Blob-Speicher einen öffentlichen Container zu erstellen:

1. Wählen Sie unter **Blob-Dienst** im linken Menü die Option **Blobs** aus.
1. Klicken Sie auf **+Container**.
1. Geben Sie im Feld **Name** die Zeichenfolge *root* ein. Dies kann ein Name Ihrer Wahl sein, z. B. *wingtiptoys*, aber in diesem Beispiel verwenden wir der Einfachheit halber *root*.
1. Wählen Sie für **Öffentliche Zugriffsebene** die Option **Blob** aus, und klicken Sie dann auf **OK**.
1. Klicken Sie auf **root**, um den neuen Container zu öffnen.
1. Klicken Sie auf **Hochladen**.
1. Klicken Sie auf das Ordnersymbol neben **Datei auswählen**.
1. Navigieren Sie zur Datei **customize-ui.html**, die Sie zuvor im Abschnitt „Seite für die Benutzeroberflächenanpassung“ erstellt haben, und wählen Sie die Datei aus.
1. Wenn Sie die Datei in einen Unterordner hochladen möchten, erweitern Sie die Option **Erweitert**, und geben Sie im Feld **In Ordner hochladen** einen Ordnernamen ein.
1. Wählen Sie die Option **Hochladen**.
1. Wählen Sie das Blob **customize-ui.html** aus, das Sie hochgeladen haben.
1. Wählen Sie rechts neben dem Textfeld **URL** das Symbol **In Zwischenablage kopieren** aus, um die URL in die Zwischenablage zu kopieren.
1. Navigieren Sie im Webbrowser zu der kopierten URL, um zu überprüfen, ob auf das hochgeladene Blob zugegriffen werden kann. Wenn ein Zugriff darauf nicht möglich ist und beispielsweise ein Fehler vom Typ `ResourceNotFound` auftritt, stellen Sie sicher, dass der Zugriffstyp für den Container auf **Blob** festgelegt ist.

## <a name="configure-cors"></a>Konfigurieren von CORS

Führen Sie die folgenden Schritte aus, um den Blob-Speicher für die Ressourcenfreigabe zwischen verschiedenen Ursprüngen (Cross-Origin Resource Sharing, CORS) zu konfigurieren:

1. Wählen Sie im Menü **CORS** aus.
1. Geben Sie für **Zulässige Ursprünge** den Wert `https://your-tenant-name.b2clogin.com` ein. Ersetzen Sie `your-tenant-name` durch den Namen des Azure AD B2C-Mandanten. Beispiel: `https://fabrikam.b2clogin.com`. Sie dürfen bei der Eingabe Ihres Mandantennamens ausschließlich Kleinbuchstaben verwenden.
1. Wählen Sie für **Zulässige Methoden** sowohl `GET` als auch `OPTIONS` aus.
1. Geben Sie für **Zulässige Header** ein Sternchen (*) ein.
1. Geben Sie für **Verfügbar gemachte Header** ein Sternchen (*) ein.
1. Für **Max. Alter** geben Sie 200 ein.
1. Klicken Sie auf **Speichern**.

## <a name="test-cors"></a>Testen von CORS

Führen Sie die folgenden Schritte aus, um zu überprüfen, ob Sie bereit sind:

1. Navigieren Sie zur Website [www.test-cors.org](https://www.test-cors.org/), und fügen Sie anschließend die URL in das Feld **Remote-URL** ein.
1. Klicken Sie auf **Anforderung senden**.
    Wenn ein Fehler ausgegeben wird, sollten Sie sich vergewissern, dass Ihre [CORS-Einstellungen](#configure-cors) richtig sind. Außerdem müssen Sie unter Umständen Ihren Browsercache löschen oder eine InPrivate-Browsersitzung öffnen, indem Sie STRG+UMSCHALT+P drücken.

## <a name="modify-the-extensions-file"></a>Ändern der Erweiterungsdatei

Um die Anpassung der Benutzeroberfläche zu konfigurieren, kopieren Sie die **ContentDefinition** und ihre untergeordneten Elemente in der Basisdatei, und fügen Sie sie in die Erweiterungsdatei ein.

1. Öffnen Sie die Basisdatei Ihrer Richtlinie. Beispiel: <em>`SocialAndLocalAccounts/` **`TrustFrameworkBase.xml`** </em>. Das ist eine der Richtliniendateien im Starter Pack für benutzerdefinierte Richtlinien, die Sie in der Voraussetzung [Erste Schritte mit benutzerdefinierten Richtlinien in Azure Active Directory B2C](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-get-started-custom) erhalten haben sollten.
1. Suchen Sie nach dem Element **ContentDefinitions**, und kopieren Sie den gesamten Inhalt.
1. Öffnen Sie die Erweiterungsdatei. Beispiel: *TrustFrameworkExtensions.xml*. Suchen Sie nach dem Element **BuildingBlocks**. Wenn das Element nicht vorhanden ist, fügen Sie es hinzu.
1. Fügen Sie den gesamten Inhalt des **ContentDefinitions**-Elements ein, das Sie als untergeordnetes Element des **BuildingBlocks**-Elements kopiert haben.
1. Suchen Sie dem **ContentDefinition**-Element, das `Id="api.signuporsignin"` in der kopierten XML-Datei enthält.
1. Ändern Sie den Wert von **LoadUri** in die URL der HTML-Datei, die Sie in den Speicher hochgeladen haben. Beispiel: `https://your-storage-account.blob.core.windows.net/your-container/customize-ui.html`.

    Ihre benutzerdefinierte Richtlinie sollte wie folgt aussehen:

    ```xml
    <BuildingBlocks>
      <ContentDefinitions>
        <ContentDefinition Id="api.signuporsignin">
          <LoadUri>https://your-storage-account.blob.core.windows.net/your-container/customize-ui.html</LoadUri>
          <RecoveryUri>~/common/default_page_error.html</RecoveryUri>
          <DataUri>urn:com:microsoft:aad:b2c:elements:unifiedssp:1.0.0</DataUri>
          <Metadata>
            <Item Key="DisplayName">Signin and Signup</Item>
          </Metadata>
        </ContentDefinition>
      </ContentDefinitions>
    </BuildingBlocks>
    ```

1. Speichern Sie die Erweiterungsdatei.

## <a name="upload-your-updated-custom-policy"></a>Hochladen der aktualisierten benutzerdefinierten Richtlinie

1. Stellen Sie sicher, dass Sie das Verzeichnis verwenden, das Ihren Azure AD B2C-Mandanten enthält, indem Sie im oberen Menü auf den **Verzeichnis- und Abonnementfilter** klicken und das entsprechende Verzeichnis auswählen.
1. Wählen Sie links oben im Azure-Portal die Option **Alle Dienste** aus, suchen Sie nach **Azure AD B2C**, und wählen Sie dann diese Option aus.
1. Wählen Sie **Framework für die Identitätsfunktion** aus.
1. Klicken Sie auf **Alle Richtlinien**.
1. Klicken Sie auf **Richtlinie hochladen**.
1. Laden Sie die zuvor geänderte Erweiterungsdatei hoch.

## <a name="test-the-custom-policy-by-using-run-now"></a>Testen der benutzerdefinierten Richtlinie mit **Jetzt ausführen**

1. Navigieren Sie auf dem Blatt **Azure AD B2C** zu **Alle Richtlinien**.
1. Wählen Sie die benutzerdefinierte Richtlinie aus, die hochgeladen werden soll, und klicken Sie auf die Schaltfläche **Jetzt ausführen**.
1. Sie sollten sich mit einer E-Mail-Adresse registrieren können.

## <a name="reference"></a>Verweis

### <a name="sample-templates"></a>Beispielvorlagen
Beispielvorlagen für die Benutzeroberflächenanpassung finden Sie hier:

```
git clone https://github.com/azureadquickstarts/b2c-azureblobstorage-client
```

Der Ordner „sample_templates/wingtip“ enthält die folgenden HTML-Dateien:

| HTML5-Vorlage | BESCHREIBUNG |
|----------------|-------------|
| *phonefactor.html* | Verwenden Sie diese Datei als Vorlage für eine Seite für die mehrstufige Authentifizierung. |
| *resetpassword.html* | Verwenden Sie diese Datei als Vorlage für eine Seite vom Typ „Kennwort vergessen“. |
| *selfasserted.html* | Verwenden Sie diese Datei als Vorlage für eine Seite zum Anmelden an einem sozialen Netzwerk, eine Seite zum Registrieren für ein lokales Konto oder eine Seite zum Anmelden an einem lokalen Konto. |
| *unified.html* | Verwenden Sie diese Datei als Vorlage für eine einheitliche Registrierungs- oder Anmeldeseite. |
| *updateprofile.html* | Verwenden Sie diese Datei als Vorlage für eine Seite zum Aktualisieren von Profilen. |

Hier sind die auszuführenden Schritte zur Verwendung des Beispiels:

1. Klonen Sie das Repository auf Ihrem lokalen Computer. Wählen Sie unter „sample_templates“ einen Vorlagenordner aus. Sie können `wingtip` oder `contoso` verwenden.
1. Laden Sie alle Dateien aus den Ordnern `css`, `fonts` und `images` in den Blob-Speicher hoch, wie in den vorherigen Abschnitten beschrieben.
1. Öffnen Sie als Nächstes jede HTML-Datei (\*.html) im Stammverzeichnis von `wingtip` oder `contoso` (je nachdem, welchen Ordner Sie im ersten Schritt ausgewählt haben), und ersetzen Sie alle Vorkommen von „http://localhost“ durch die URLs der Dateien aus den Ordnern „css“, „images“ und „fonts“, die Sie in Schritt 2 hochgeladen haben.
1. Speichern Sie die HTML-Dateien (\*.html), und laden Sie die Dateien in den Blob-Speicher hoch.
1. Ändern Sie jetzt die Erweiterungsdatei, wie zuvor unter [Ändern der Erweiterungsdatei](#modify-the-extensions-file) erwähnt.
1. Falls Schriftarten, Bilder oder CSS-Dateien fehlen, überprüfen Sie die Verweise in der Erweiterungsrichtlinie und die HTML-Dateien (\*.html).

### <a name="content-definition-ids"></a>ID für Inhaltsdefinitionen

Im Abschnitt „Ändern von benutzerdefinierten Registrierungs- oder Anmelderichtlinien“ haben Sie die Inhaltsdefinition für `api.idpselections` konfiguriert. Der vollständige Satz mit IDs für die Inhaltsdefinition, die vom Azure AD B2C Identity Experience Framework erkannt werden, und die dazugehörigen Beschreibungen sind in der folgenden Tabelle enthalten:

| ID für Inhaltsdefinition | BESCHREIBUNG |
|-----------------------|-------------|
| *api.error* | **Fehlerseite**: Diese Seite wird angezeigt, wenn eine Ausnahme oder ein Fehler auftreten. |
| *api.idpselections* | **Seite zur Auswahl des Identitätsanbieters**: Diese Seite enthält eine Liste mit den Identitätsanbietern, unter denen der Benutzer bei der Anmeldung wählen kann. Bei diesen Optionen handelt es sich um Unternehmensidentitätsanbieter, Identitätsanbieter in Form von sozialen Netzwerken wie Facebook und Google+ oder lokale Konten. |
| *api.idpselections.signup* | **Seite zur Auswahl des Identitätsanbieters für die Registrierung**: Diese Seite enthält eine Liste mit den Identitätsanbietern, aus denen der Benutzer bei der Registrierung auswählen kann. Bei diesen Optionen handelt es sich um Unternehmensidentitätsanbieter, Identitätsanbieter in Form von sozialen Netzwerken wie Facebook und Google+ oder lokale Konten. |
| *api.localaccountpasswordreset* | **Seite „Kennwort vergessen“** : Diese Seite enthält ein Formular, das vom Benutzer ausgefüllt werden muss, um eine Kennwortzurücksetzung zu initiieren.  |
| *api.localaccountsignin* | **Seite für Anmeldung mit lokalem Konto**: Diese Seite enthält ein Anmeldeformular für die Anmeldung mit einem lokalen Konto, das auf einer E-Mail-Adresse oder einem Benutzernamen basiert. Das Formular kann ein Texteingabefeld und ein Kennworteingabefeld enthalten. |
| *api.localaccountsignup* | **Seite für Registrierung mit lokalem Konto**: Diese Seite enthält ein Registrierungsformular für die Registrierung für ein lokales Konto, das auf einer E-Mail-Adresse oder einem Benutzernamen basiert. Das Formular kann verschiedene Eingabesteuerelemente enthalten, z.B. ein Texteingabefeld, ein Kennworteingabefeld, ein Optionsfeld, Dropdownfelder mit einer Auswahlmöglichkeit und Kontrollkästchen mit mehreren Optionen. |
| *api.phonefactor* | **Seite „Multi-Factor Authentication“** : Auf dieser Seite können Benutzer während der Registrierung oder Anmeldung ihre Telefonnummern verifizieren (per SMS oder Sprachnachricht). |
| *api.selfasserted* | **Seite zur Registrierung über Konto für soziales Netzwerk**: Diese Seite enthält ein Registrierungsformular, das Benutzer ausfüllen müssen, wenn die Registrierung mit einem vorhandenen Konto bei einem sozialen Netzwerk wie Facebook oder Google+ als Identitätsanbieter erfolgt. Diese Seite ähnelt der vorherigen Seite für die Registrierung bei einem Konto für ein soziales Netzwerk, mit Ausnahme der Felder für die Kennworteingabe. |
| *api.selfasserted.profileupdate* | **Seite für Profilaktualisierung**: Diese Seite enthält ein Formular, das von Benutzern zum Aktualisieren des Profils verwendet werden kann. Diese Seite ähnelt der Seite für die Registrierung bei einem Konto für ein soziales Netzwerk, mit Ausnahme der Felder für die Kennworteingabe. |
| *api.signuporsignin* | **Einheitliche Seite für Registrierung oder Anmeldung**: Auf dieser Seite wird sowohl die Registrierung als auch die Anmeldung von Benutzern verarbeitet, die dafür Unternehmensidentitätsanbieter, Identitätsanbieter sozialer Netzwerke, z.B. Facebook oder Google+, oder lokale Konten verwenden können.  |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen dazu, welche Elemente der Benutzeroberfläche angepasst werden können, finden Sie unter [Referenzhandbuch zur Anpassung der Benutzeroberfläche für integrierte Richtlinien](active-directory-b2c-reference-ui-customization.md).
