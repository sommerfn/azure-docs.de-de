---
title: 'Konfigurieren der Facebook-Authentifizierung: Azure App Service'
description: Erfahren Sie, wie Sie die Facebook-Authentifizierung für Ihre App Service-App konfigurieren.
services: app-service
documentationcenter: ''
author: mattchenderson
manager: syntaxc4
editor: ''
ms.assetid: b6b4f062-fcb4-47b3-b75a-ec4cb51a62fd
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 06/06/2019
ms.author: mahender
ms.custom: seodec18
ms.openlocfilehash: fb8497f3b9b887e2fd06b350bcc25ac8faaa7b43
ms.sourcegitcommit: 42748f80351b336b7a5b6335786096da49febf6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2019
ms.locfileid: "72177012"
---
# <a name="configure-your-app-service-app-to-use-facebook-login"></a>Konfigurieren Ihrer App Service-App zur Verwendung der Facebook-Anmeldung

[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

In diesem Artikel wird veranschaulicht, wie Sie Azure App Service zur Verwendung von Facebook als Authentifizierungsanbieter konfigurieren.

Sie benötigen ein Facebook-Konto mit verifizierter E-Mail-Adresse und einer Mobiltelefonnummer, um den in diesem Artikel beschriebenen Vorgang abzuschließen. Rufen Sie die Seite [facebook.com]auf, um ein neues Facebook-Konto zu erstellen.

## <a name="register"></a>Registrieren Ihrer Anwendung für Facebook

1. Wechseln Sie zur Website für [Facebook-Entwickler], und melden Sie sich mit den Anmeldeinformationen für Ihr Facebook-Konto an.

   Wenn Sie kein Facebook für Entwickler-Konto besitzen, wählen Sie **Get Started** (Erste Schritte) aus, und befolgen Sie die Registrierungsschritte.
1. Wählen Sie **My Apps** (Meine Apps) > **Add New App** (Neue App hinzufügen) aus.
1. Im Feld **Anzeigename**:
   1. Geben Sie einen eindeutigen Namen für Ihre App ein.
   1. Geben Sie Ihre **Kontakt-E-Mail** an.
   1. Wählen Sie **App-ID erstellen** aus.
   1. Schließen Sie die Sicherheitsüberprüfung ab.

   Das Entwickler-Dashboard für Ihre neue Facebook-App wird geöffnet.
1. Wählen Sie **Dashboard** > **Facebook Login** (Facebook-Anmeldung) > **Setup** (Einrichten) > **Web** aus.
1. Wählen Sie im linken Navigationsbereich utner **Facebook Login** (Facebook-Anmeldung) **Settings** (Einstellungen) aus.
1. Geben Sie in das Feld **Valid OAuth redirect URIs** (Gültige OAuth-Umleitungs-URIs) `https://<app-name>.azurewebsites.net/.auth/login/facebook/callback` ein. Denken Sie daran, `<app-name>` durch den Namen Ihrer Azure App Service-App zu ersetzen.
1. Klicken Sie auf **Save changes** (Änderungen speichern).
1. Wählen Sie im linken Bereich **Settings** > **Basic** (Einstellungen > Allgemein) aus. 
1. Wählen Sie im Feld **App Secret** (App-Geheimnis) den Eintrag **Show** (Anzeigen) aus. Kopieren Sie die Werte von **App ID** und **App Secret** (App-Geheimnis). Diese verwenden Sie später zum Konfigurieren Ihrer App Service-App in Azure.

   > [!IMPORTANT]
   > Der App-Schlüssel ist eine wichtige Anmeldeinformation. Teilen Sie diesen Schlüssel mit niemandem, und geben Sie ihn nicht über Ihre Anwendung weiter.
   >

1. Das zum Registrieren der Anwendung verwendete Facebook-Konto fungiert als Administrator der App. Zu diesem Zeitpunkt können sich nur Administratoren bei der Anwendung anmelden.

   Um andere Facebook-Konten zu authentifizieren, klicken Sie auf **App Review** (App-Überprüfung), und aktivieren Sie **Make \<your-app-name> public** (<Name Ihrer App> öffentlich machen), um den allgemeinen öffentlichen Zugriff auf die App mithilfe der Facebook-Authentifizierung zu aktivieren.

## <a name="secrets"></a>Hinzufügen von Facebook-Informationen zu Ihrer Anwendung

1. Melden Sie sich beim [Azure-Portal] an, und navigieren Sie zu Ihrer App Service-App.
1. Wählen Sie **Einstellungen** > **Authentifizierung/Autorisierung,** aus, und stellen Sie sicher, dass für die **App Service-Authentifizierung** die Option **Ein** festgelegt ist.
1. Wählen Sie **Facebook** aus, und fügen Sie dann die Werte für App-ID und den geheimen App-Schlüssel ein, die Sie zuvor abgerufen haben. Aktivieren Sie alle Bereiche, die von der Anwendung benötigt werden.
1. Klicken Sie auf **OK**.

   ![Screenshot der Einstellungen für die mobile Facebook-App][0]

    Standardmäßig erfolgt die Authentifizierung über App Service, wobei jedoch der Zugriff auf die Inhalte Ihrer Website und APIs nicht autorisiert wird. Sie müssen Benutzer in Ihrem App-Code autorisieren.
1. (Optional) Um den Zugriff ausschließlich auf Benutzer zu beschränken, die von Facebook authentifiziert werden, legen Sie **Die auszuführende Aktion, wenn die Anforderung nicht authentifiziert ist** auf **Facebook** fest. Wenn Sie diese Funktion festlegen, erfordert Ihre App, dass alle Anforderungen authentifiziert werden. Sie leitet außerdem alle nicht authentifizierten Anforderungen zur Authentifizierung an Facebook um.

   > [!CAUTION]
   > Das Einschränken des Zugriffs auf diese Weise gilt für alle Aufrufe Ihrer App, was für Apps, die eine öffentlich verfügbare Startseite haben, eventuell nicht wünschenswert ist, wie bei vielen Single-Page-Anwendungen. Bei solchen Anwendungen ist möglicherweise die Einstellung **Anonyme Anforderungen zulassen (keine Aktion)** vorzuziehen, sodass die App die Authentifizierung selbst manuell startet. Weitere Informationen finden Sie unter [Authentifizierungsflow](overview-authentication-authorization.md#authentication-flow).

1. Wählen Sie **Speichern** aus.

Sie können nun Facebook für die Authentifizierung in Ihrer App verwenden.

## <a name="related-content"> </a>Nächste Schritte

[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->
[0]: ./media/app-service-mobile-how-to-configure-facebook-authentication/mobile-app-facebook-settings.png

<!-- URLs. -->
[Facebook-Entwickler]: https://go.microsoft.com/fwlink/p/?LinkId=268286
[facebook.com]: https://go.microsoft.com/fwlink/p/?LinkId=268285
[Get started with authentication]: /en-us/develop/mobile/tutorials/get-started-with-users-dotnet/
[Azure-Portal]: https://portal.azure.com/
