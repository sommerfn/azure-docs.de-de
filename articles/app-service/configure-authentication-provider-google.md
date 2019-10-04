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
ms.openlocfilehash: fcbb284a0807ef88c5f40a7c8b65398d45bf73d7
ms.sourcegitcommit: 2aefdf92db8950ff02c94d8b0535bf4096021b11
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/03/2019
ms.locfileid: "70232138"
---
# <a name="how-to-configure-your-app-service-application-to-use-google-login"></a>Konfigurieren Ihrer App Service-Anwendung zur Nutzung der Google-Anmeldung
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

In diesem Thema wird veranschaulicht, wie Sie Azure App Service zur Verwendung von Google als Authentifizierungsanbieter konfigurieren.

Sie benötigen ein Google-Konto mit verifizierter E-Mail-Adresse, um den in diesem Thema beschriebenen Vorgang abzuschließen. Besuchen Sie die Seite [accounts.google.com](https://go.microsoft.com/fwlink/p/?LinkId=268302), um ein neues Google-Konto zu erstellen.

## <a name="register"></a>Registrieren Ihrer Anwendung für Google
1. Gehen Sie entsprechend der Google-Dokumentation unter [Google Sign-In for server-side apps](https://developers.google.com/identity/sign-in/web/server-side-flow) vor, um eine Client-ID und einen geheimen Client Schlüssel mit den folgenden Informationen zu erstellen (es sind keine Codeänderungen erforderlich):
    - Verwenden Sie für **Authorized JavaScript Origins** den Wert `https://<app-name>.azurewebsites.net`, wobei Sie den Namen Ihrer App in *\<app-name>* angeben.
    - Verwenden Sie für **Authorized Redirect URI** den Wert `https://<app-name>.azurewebsites.net/.auth/login/google/callback`.
1. Nachdem die Client-ID und der geheime Clientschlüssel erstellt sind, kopieren Sie deren Werte.

    > [!IMPORTANT]
    > Der geheime Clientschlüssel ist eine wichtige Anmeldeinformation. Teilen Sie diesen Schlüssel mit niemandem, und geben Sie ihn nicht über Ihre Anwendung weiter.


## <a name="secrets"></a>Hinzufügen von Google-Informationen zu Ihrer Anwendung
1. Navigieren Sie im [Azure-Portal] zu Ihrer App Service-App. Wählen Sie im linken Menü die Option **Authentifizierung/Autorisierung** aus.
2. Falls das Authentifizierungs-/Autorisierungsfeature nicht aktiviert ist, aktivieren Sie es über die Option **Ein**.
3. Klicken Sie auf **Google**. Fügen Sie die ID und den geheimen Schlüssel ein, die Sie zuvor erhalten haben, und aktivieren Sie optional alle Bereiche, die bei Ihrer Anwendung erforderlich sind. Klicken Sie dann auf **OK**.

   App Service bietet Authentifizierung, schränkt aber den autorisierten Zugriff auf den Inhalt und die APIs Ihrer Website nicht ein. Weitere Informationen finden Sie unter [Autorisieren oder Ablehnen von Benutzern](app-service-authentication-how-to.md#authorize-or-deny-users).
4. (Optional) Um den Zugriff auf Ihre Website ausschließlich auf Benutzer zu beschränken, die von Google authentifiziert wurden, legen Sie **Die auszuführende Aktion, wenn die Anforderung nicht authentifiziert ist.** auf **Google** fest. Dadurch müssen alle Anforderungen authentifiziert werden. Alle nicht authentifizierten Anforderungen werden zur Authentifizierung an Google umgeleitet.

    > [!NOTE]
    > Das Einschränken des Zugriffs auf diese Weise gilt für alle Aufrufe Ihrer App, was für Apps, die eine öffentlich verfügbare Startseite wünschen, eventuell nicht wünschenswert ist, wie bei vielen Single-Page-Anwendungen. Bei solchen Anwendungen ist möglicherweise die Einstellung **Anonyme Anforderungen zulassen (keine Aktion)** vorzuziehen, wobei die App selbst die Anmeldung manuell startet, wie [hier](overview-authentication-authorization.md#authentication-flow) beschrieben.
    
5. Klicken Sie auf **Speichern**.

Sie können nun Google für die Authentifizierung in Ihrer App verwenden.

## <a name="related-content"></a>Verwandte Inhalte
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Anchors. -->

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-google-authentication/mobile-app-google-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-google-authentication/mobile-app-google-settings.png

<!-- URLs. -->

[Google apis]: https://go.microsoft.com/fwlink/p/?LinkId=268303

[Azure-Portal]: https://portal.azure.com/

