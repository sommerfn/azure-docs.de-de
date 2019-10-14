---
title: 'Konfigurieren der Google-Authentifizierung: Azure App Service'
description: Erfahren Sie, wie Sie die Google-Authentifizierung für Ihre App Service-App konfigurieren.
services: app-service
documentationcenter: ''
author: cephalin
manager: gwallace
editor: ''
ms.assetid: 2b2f9abf-9120-4aac-ac5b-4a268d9b6e2b
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 09/02/2019
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: 917fa87a0cd0f7b0615a5139a7c15311f866739a
ms.sourcegitcommit: 42748f80351b336b7a5b6335786096da49febf6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2019
ms.locfileid: "72176967"
---
# <a name="configure-your-app-service-app-to-use-google-login"></a>Konfigurieren Ihrer App Service-Anwendung zur Verwendung der Google-Anmeldung

[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

In diesem Thema wird veranschaulicht, wie Sie Azure App Service zur Verwendung von Google als Authentifizierungsanbieter konfigurieren.

Sie benötigen ein Google-Konto mit verifizierter E-Mail-Adresse, um den in diesem Thema beschriebenen Vorgang abzuschließen. Besuchen Sie die Seite [accounts.google.com](https://go.microsoft.com/fwlink/p/?LinkId=268302), um ein neues Google-Konto zu erstellen.

## <a name="register"></a>Registrieren Ihrer Anwendung für Google

1. Gehen Sie entsprechend der Google-Dokumentation unter [Google Sign-In for server-side apps](https://developers.google.com/identity/sign-in/web/server-side-flow) vor, um eine Client-ID und einen geheimen Client Schlüssel zu erstellen: Es sind keine Codeänderungen erforderlich. Verwenden Sie lediglich die folgenden Informationen:
    - Verwenden Sie für **Authorized JavaScript Origins** den Wert `https://<app-name>.azurewebsites.net`, wobei Sie den Namen Ihrer App in *\<app-name>* angeben.
    - Verwenden Sie für **Authorized Redirect URI** den Wert `https://<app-name>.azurewebsites.net/.auth/login/google/callback`.
1. Kopieren Sie die App-ID- und App-Geheimnis-Werte.

    > [!IMPORTANT]
    > Das App-Geheimnis ist eine wichtige Sicherheitsanmeldeinformation. Teilen Sie diesen Schlüssel mit niemandem, und geben Sie ihn nicht über Ihre Anwendung weiter.

## <a name="secrets"></a>Hinzufügen von Google-Informationen zu Ihrer Anwendung

1. Wechseln Sie im [Azure-Portal] zu Ihrer App Service-App.
1. Wählen Sie **Einstellungen** > **Authentifizierung/Autorisierung,** aus, und stellen Sie sicher, dass für die **App Service-Authentifizierung** die Option **Ein** festgelegt ist.
1. Wählen Sie **Google** aus, und fügen Sie dann die Werte für App-ID und den geheimen App-Schlüssel ein, die Sie zuvor abgerufen haben. Aktivieren Sie alle Bereiche, die von der Anwendung benötigt werden.
1. Klicken Sie auf **OK**.

   App Service bietet Authentifizierung, schränkt aber den autorisierten Zugriff auf den Inhalt und die APIs Ihrer Website nicht ein. Weitere Informationen finden Sie unter [Autorisieren oder Ablehnen von Benutzern](app-service-authentication-how-to.md#authorize-or-deny-users).

1. (Optional) Um den Sitezugriff ausschließlich auf Benutzer zu beschränken, die von Google authentifiziert wurden, legen Sie **Die auszuführende Aktion, wenn die Anforderung nicht authentifiziert ist.** auf **Google** fest. Wenn Sie diese Funktion festlegen, erfordert Ihre App, dass alle Anforderungen authentifiziert werden. Sie leitet außerdem alle nicht authentifizierten Anforderungen zur Authentifizierung an Google um.

    > [!CAUTION]
    > Das Einschränken des Zugriffs auf diese Weise gilt für alle Aufrufe Ihrer App, was für Apps, die eine öffentlich verfügbare Startseite haben, eventuell nicht wünschenswert ist, wie bei vielen Single-Page-Anwendungen. Bei solchen Anwendungen ist möglicherweise die Einstellung **Anonyme Anforderungen zulassen (keine Aktion)** vorzuziehen, sodass die App die Anmeldung selbst manuell startet. Weitere Informationen finden Sie unter [Authentifizierungsflow](overview-authentication-authorization.md#authentication-flow).

1. Wählen Sie **Speichern** aus.

Sie können nun Google für die Authentifizierung in Ihrer App verwenden.

## <a name="related-content"> </a>Nächste Schritte

[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Anchors. -->

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-google-authentication/mobile-app-google-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-google-authentication/mobile-app-google-settings.png

<!-- URLs. -->

[Google apis]: https://go.microsoft.com/fwlink/p/?LinkId=268303

[Azure-Portal]: https://portal.azure.com/

