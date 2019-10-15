---
title: 'Konfigurieren der Twitter-Authentifizierung: Azure App Service'
description: Erfahren Sie, wie Sie die Twitter-Authentifizierung für Ihre App Service-App konfigurieren.
services: app-service
documentationcenter: ''
author: mattchenderson
manager: syntaxc4
editor: ''
ms.assetid: c6dc91d7-30f6-448c-9f2d-8e91104cde73
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 04/19/2018
ms.author: mahender
ms.custom: seodec18
ms.openlocfilehash: 02008b7dc1609a5f28ac6ba2a582933a96428198
ms.sourcegitcommit: 42748f80351b336b7a5b6335786096da49febf6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2019
ms.locfileid: "72176953"
---
# <a name="configure-your-app-service-app-to-use-twitter-login"></a>Konfigurieren Ihrer App Service-App zur Verwendung der Twitter-Anmeldung

[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

In diesem Artikel wird veranschaulicht, wie Sie Azure App Service zur Verwendung von Twitter als Authentifizierungsanbieter konfigurieren.

Sie benötigen ein Twitter-Konto mit verifizierter E-Mail-Adresse und Telefonnummer, um den in diesem Artikel beschriebenen Vorgang abzuschließen. Rufen Sie die Seite [twitter.com] auf, um ein neues Twitter-Konto zu erstellen.

## <a name="register"></a>Registrieren Ihrer Anwendung für Twitter

1. Melden Sie sich beim [Azure-Portal] an, und wechseln Sie zu Ihrer Anwendung. Kopieren Sie die **URL**. Sie verwenden sie zur Konfiguration Ihrer Twitter-App.
1. Wechseln Sie zur [Twitter Developers]-Website, melden Sie sich mit den Anmeldeinformationen für Ihr Twitter-Konto an, und wählen Sie dann **Neue App erstellen** aus.
1. Geben Sie einen **Name**n und eine **Beschreibung** für Ihre neue App ein. Fügen Sie die **URL** Ihrer Anwendung in das Feld **Website** ein. Geben Sie in das Feld **Rückruf-URL** die URL Ihrer App Service-App ein, und fügen Sie den Pfad `/.auth/login/aad/callback` an. Beispiel: `https://contoso.azurewebsites.net/.auth/login/twitter/callback`. Stellen Sie sicher, dass Sie das HTTPS-Schema verwenden.
1. Lesen und akzeptieren Sie die Bedingungen am Ende der Seite. Wählen Sie **Create your Twitter application** aus. Die Anwendungsdetails werden angezeigt.
1. Klicken Sie auf die Registerkarte **Einstellungen**, aktivieren Sie die Option **Allow this application to be used to sign in with Twitter** (Anmeldung bei Twitter mithilfe dieser Anwendung zulassen), und klicken Sie dann auf **Update Settings** (Einstellungen aktualisieren).
1. Wählen Sie die Registerkarte **Keys and Access Tokens** .

   Notieren Sie sich diese Werte:
   - Consumer Key (API Key) (Consumerschlüssel [API-Schlüssel])
   - Consumer Secret (API Secret) (Consumergeheimnis [API-Geheimnis])

   > [!NOTE]
   > Consumer secret ist eine wichtige Anmeldeinformation. Teilen Sie diesen Schlüssel mit niemandem, und geben Sie ihn nicht über Ihre App frei.

## <a name="secrets"></a>Hinzufügen von Twitter-Informationen zu Ihrer Anwendung

1. Wechseln Sie im [Azure-Portal] zu Ihrer Anwendung.
1. Wählen Sie **Einstellungen** > **Authentifizierung/Autorisierung,** aus, und stellen Sie sicher, dass für die **App Service-Authentifizierung** die Option **Ein** festgelegt ist.
1. Wählen Sie **Twitter** aus.
1. Fügen Sie die Werte von `API Key` und `API Secret` ein, die Sie zuvor abgerufen haben.
1. Klicken Sie auf **OK**.

   ![Screenshot der Einstellungen für die mobile Twitter-App][1]

   Standardmäßig erfolgt die Authentifizierung über App Service, wobei jedoch der Zugriff auf die Inhalte Ihrer Website und APIs nicht autorisiert wird. Sie müssen die Benutzer in Ihrem App-Code autorisieren.

1. (Optional) Um den Zugriff auf Ihre Website ausschließlich auf Benutzer zu beschränken, die von Twitter authentifiziert wurden, legen Sie **Die auszuführende Aktion, wenn die Anforderung nicht authentifiziert ist** auf **Twitter** fest. Wenn Sie diese Funktion festlegen, erfordert Ihre App, dass alle Anforderungen authentifiziert werden. Sie leitet außerdem alle nicht authentifizierten Anforderungen zur Authentifizierung an Twitter um.

   > [!CAUTION]
   > Das Einschränken des Zugriffs auf diese Weise gilt für alle Aufrufe Ihrer App, was für Apps, die eine öffentlich verfügbare Startseite haben, eventuell nicht wünschenswert ist, wie bei vielen Single-Page-Anwendungen. Bei solchen Anwendungen ist möglicherweise die Einstellung **Anonyme Anforderungen zulassen (keine Aktion)** vorzuziehen, sodass die App die Authentifizierung selbst manuell startet. Weitere Informationen finden Sie unter [Authentifizierungsflow](overview-authentication-authorization.md#authentication-flow).

1. Wählen Sie **Speichern** aus.

Sie können nun Twitter für die Authentifizierung in Ihrer App verwenden.

## <a name="related-content"> </a>Nächste Schritte

[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-twitter-authentication/app-service-twitter-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-twitter-authentication/mobile-app-twitter-settings.png

<!-- URLs. -->

[Twitter Developers]: https://go.microsoft.com/fwlink/p/?LinkId=268300
[twitter.com]: https://go.microsoft.com/fwlink/p/?LinkID=268287
[Azure-Portal]: https://portal.azure.com/
[xamarin]: ../app-services-mobile-app-xamarin-ios-get-started-users.md
