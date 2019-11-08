---
title: 'Tutorial: Registrieren einer Anwendung – Azure Active Directory B2C'
description: Erfahren Sie, wie Sie eine Webanwendungen in Azure Active Directory B2C mithilfe des Azure-Portals registrieren.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 10/16/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 00b7d1cfea4a36b8ba5b78aea344288e11da67cf
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73474734"
---
# <a name="tutorial-register-an-application-in-azure-active-directory-b2c"></a>Tutorial: Registrieren Sie eine Anwendung in Azure Active Directory B2C

Bevor Ihre [Anwendungen](active-directory-b2c-apps.md) mit Azure Active Directory B2C (Azure AD B2C) interagieren können, müssen sie in einem von Ihnen verwalteten Mandanten registriert werden. In diesem Tutorial erfahren Sie, wie Sie eine Webanwendung mithilfe des Azure-Portals registrieren.

In diesem Artikel werden folgende Vorgehensweisen behandelt:

> [!div class="checklist"]
> * Registrieren einer Webanwendung
> * Erstellen eines Clientgeheimnisses

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie Ihren eigenen [Azure AD B2C-Mandanten](tutorial-create-tenant.md) noch nicht erstellt haben, erstellen Sie jetzt einen. Sie können einen vorhandenen Azure AD B2C-Mandanten verwenden.

## <a name="register-a-web-application"></a>Registrieren einer Webanwendung

Zum Registrieren einer Anwendung in Ihrem Azure AD B2C-Mandanten können Sie die aktuelle Benutzeroberfläche für **Anwendungen** oder unsere neue einheitliche Benutzeroberfläche **App-Registrierungen (Vorschau)** verwenden. [Erfahren Sie mehr über die Vorschaubenutzeroberfläche](https://aka.ms/b2cappregintro).

#### <a name="applicationstabapplications"></a>[Anwendungen](#tab/applications/)

1. Stellen Sie sicher, dass Sie das Verzeichnis verwenden, das Ihren Azure AD B2C-Mandanten enthält, indem Sie im oberen Menü auf den **Verzeichnis- und Abonnementfilter** klicken und das entsprechende Verzeichnis auswählen.
1. Wählen Sie links oben im Azure-Portal die Option **Alle Dienste** aus, suchen Sie nach **Azure AD B2C**, und wählen Sie dann diese Option aus.
1. Wählen Sie **Anwendungen** und dann **Hinzufügen** aus.
1. Geben Sie einen Namen für die Anwendung ein. Beispiel: *webapp1*.
1. Wählen Sie für **Web-App/Web-API einschließen** und n**Impliziten Ablauf zulassen** die Option **Ja**.
1. Geben Sie für **Antwort-URLs** einen Endpunkt ein, an den Azure AD B2C von Ihrer App angeforderte Token zurückgibt. Sie können sie so einrichten, dass sie lokal bei `https://localhost:44316` lauschen. Wenn Sie die Portnummer noch nicht kennen, können Sie einen Platzhalterwert eingeben und diesen später ändern.

    Zu Testzwecken wie in diesem Tutorial können Sie `https://jwt.ms` festlegen. Damit zeigen Sie den Inhalt eines Tokens für die Überprüfung an. Legen Sie in diesem Tutorial **Antwort-URL** auf `https://jwt.ms` fest.

    Für Antwort-URLs gelten die folgenden Einschränkungen:

    * Die Antwort-URL muss mit dem Schema `https` beginnen.
    * Bei der Antwort-URL muss die Groß-/Kleinschreibung beachtet werden. Die Groß-/Kleinschreibung muss der Groß-/Kleinschreibung des URL-Pfads Ihrer ausgeführten Anwendung entsprechen. Wenn Ihre Anwendung z. B. als Teil des Pfads `.../abc/response-oidc` enthält, geben Sie in der Antwort-URL nicht `.../ABC/response-oidc` an. Weil der Webbrowser bei Pfaden die Groß-/Kleinschreibung beachtet, werden Cookies, die `.../abc/response-oidc` zugeordnet sind, möglicherweise ausgeschlossen, wenn eine Umleitung an die anders geschriebene (nicht übereinstimmende) URL `.../ABC/response-oidc` erfolgt.

1. Klicken Sie auf **Erstellen**, um die Anwendungsregistrierung abzuschließen.

#### <a name="app-registrations-previewtabapp-reg-preview"></a>[App-Registrierungen (Vorschau)](#tab/app-reg-preview/)

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im oberen Menü den Filter **Verzeichnis und Abonnement** aus, und wählen Sie dann das Verzeichnis aus, das Ihren Azure AD B2C-Mandanten enthält.
1. Wählen Sie im linken Menü die Option **Azure AD B2C** aus. Oder wählen Sie **Alle Dienste** aus, suchen Sie nach dem Eintrag **Azure AD B2C**, und wählen Sie ihn aus.
1. Wählen Sie **App-Registrierungen (Vorschau)** aus, und wählen Sie dann **Neue Registrierung** aus.
1. Geben Sie einen **Namen** für die Anwendung ein. Beispiel: *webapp1*.
1. Wählen Sie **Konten in einem Organisationsverzeichnis oder ein beliebiger Identitätsanbieter** aus.
1. Wählen Sie unter **Umleitungs-URI** die Option **Web** aus, und geben Sie `https://jwt.ms` in das URL-Textfeld ein.

    Die Umleitungs-URI ist der Endpunkt, an den der Benutzer vom Autorisierungsserver (in diesem Fall Azure AD B2C) nach Abschluss seiner Interaktion mit dem Benutzer gesendet wird und an den bei erfolgreicher Autorisierung ein Zugriffstoken oder Autorisierungscode gesendet wird. In einer Produktionsanwendung handelt es sich in der Regel um einen öffentlich zugänglichen Endpunkt, an dem Ihre App ausgeführt wird, etwa um `https://contoso.com/auth-response`. Für Testzwecke wie in diesem Tutorial können Sie ihn auf `https://jwt.ms` festlegen. Dabei handelt es sich um eine Microsoft-Webanwendung, die den decodierten Inhalt eines Tokens anzeigt (der Inhalt des Tokens verlässt niemals Ihren Browser). Während der App-Entwicklung können Sie den Endpunkt hinzufügen, an dem die Anwendung lokal lauscht, etwa `https://localhost:5000`. Sie können Umleitungs-URIs in Ihren registrierten Anwendungen jederzeit hinzufügen und ändern.

    Für Umleitungs-URIs gelten die folgenden Einschränkungen:

    * Die Antwort-URL muss mit dem Schema `https` beginnen.
    * Bei der Antwort-URL muss die Groß-/Kleinschreibung beachtet werden. Die Groß-/Kleinschreibung muss der Groß-/Kleinschreibung des URL-Pfads Ihrer ausgeführten Anwendung entsprechen. Wenn Ihre Anwendung z. B. als Teil des Pfads `.../abc/response-oidc` enthält, geben Sie in der Antwort-URL nicht `.../ABC/response-oidc` an. Weil der Webbrowser bei Pfaden die Groß-/Kleinschreibung beachtet, werden Cookies, die `.../abc/response-oidc` zugeordnet sind, möglicherweise ausgeschlossen, wenn eine Umleitung an die anders geschriebene (nicht übereinstimmende) URL `.../ABC/response-oidc` erfolgt.

1. Aktivieren Sie unter **Berechtigungen** das Kontrollkästchen *Administratoreinwilligung für openid- und offline_access-Berechtigungen erteilen*.
1. Wählen Sie **Registrieren**.

Aktivieren Sie nach Abschluss der Anwendungsregistrierung den Ablauf zur impliziten Gewährung:

1. Wählen Sie unter **Verwalten** die Option **Authentifizierung** aus.
1. Wählen Sie **Neue Benutzeroberfläche ausprobieren** aus (sofern die Option angezeigt wird).
1. Aktivieren Sie unter **IMPLIZITE GEWÄHRUNG** die Kontrollkästchen **Zugriffstoken** und **ID-Token**.
1. Wählen Sie **Speichern** aus.

* * *

## <a name="create-a-client-secret"></a>Erstellen eines Clientgeheimnisses

Wenn Ihre Anwendung einen Code für ein Token austauscht, müssen Sie ein Anwendungsgeheimnis erstellen.

#### <a name="applicationstabapplications"></a>[Anwendungen](#tab/applications/)

1. Wählen Sie auf der Seite **Azure AD B2C – Anwendungen** die von Ihnen erstellte Anwendung aus, z.B. *webapp1*.
1. Wählen Sie **Schlüssel** und dann **Schlüssel generieren** aus.
1. Wählen Sie **Speichern**, um den Schlüssel anzuzeigen. Notieren Sie sich den Wert für **App-Schlüssel**. Sie verwenden diesen Wert als Anwendungsgeheimnis im Code Ihrer Anwendung.

#### <a name="app-registrations-previewtabapp-reg-preview"></a>[App-Registrierungen (Vorschau)](#tab/app-reg-preview/)

1. Wählen Sie auf der Seite **Azure AD B2C – App-Registrierungen (Vorschau)** die von Ihnen erstellte Anwendung aus, z.B. *webapp1*.
1. Wählen Sie unter **Verwalten** die Option **Zertifikate und Geheimnisse** aus.
1. Wählen Sie **Neuer geheimer Clientschlüssel**.
1. Geben Sie im Feld **Beschreibung** eine Beschreibung für das Clientgeheimnis ein. Beispielsweise *clientsecret1*.
1. Wählen Sie unter **Läuft ab** einen Zeitraum aus, für den das Geheimnis gültig ist, und wählen Sie dann **Hinzufügen** aus.
1. Notieren Sie den **WERT** des Geheimnisses. Sie verwenden diesen Wert als Anwendungsgeheimnis im Code Ihrer Anwendung.

* * *

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Registrieren einer Webanwendung
> * Erstellen eines Clientgeheimnisses

Als Nächstes erfahren Sie, wie Sie Benutzerflows erstellen, damit sich die Benutzer registrieren, anmelden und ihre Profile verwalten können.

> [!div class="nextstepaction"]
> [Erstellen von Benutzerflows in Azure Active Directory B2C >](tutorial-create-user-flows.md)
