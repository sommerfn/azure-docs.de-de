---
title: 'Konfigurieren der Authentifizierung über ein Microsoft-Konto: Azure App Service'
description: Erfahren Sie, wie Sie die Microsoft-Kontoauthentifizierung für Ihre App Service-App konfigurieren.
author: mattchenderson
services: app-service
documentationcenter: ''
manager: syntaxc4
editor: ''
ms.assetid: ffbc6064-edf6-474d-971c-695598fd08bf
ms.service: app-service
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 08/08/2019
ms.author: mahender
ms.custom: seodec18
ms.openlocfilehash: 70af534e6bcd0039dbc602a5ebc3fc35fb145e79
ms.sourcegitcommit: 42748f80351b336b7a5b6335786096da49febf6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2019
ms.locfileid: "72176931"
---
# <a name="configure-your-app-service-app-to-use-microsoft-account-login"></a>Konfigurieren Ihrer App Service-Anwendung zur Verwendung der Microsoft-Kontoanmeldung

[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

In diesem Thema wird demonstriert, wie Sie Azure App Services zur Verwendung eines Microsoft-Kontos als Authentifizierungsanbieter konfigurieren. 

## <a name="register-microsoft-account"></a>Registrieren Ihrer App mit einem Microsoft-Konto

1. Wechseln Sie im Azure-Portal zu [**App-Registrierungen**](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade). Falls erforderlich, melden Sie sich mit Ihrem Microsoft-Konto an.
1. Wählen Sie **Neue Registrierung** aus, und geben Sie dann einen Anwendungsnamen ein.
1. Wählen Sie in **Umleitungs-URIs** die Option **Web** aus, und geben Sie dann `https://<app-domain-name>/.auth/login/microsoftaccount/callback supply the endpoint for your application` ein. Ersetzen Sie *\<app-domain-name>* durch den Domänennamen Ihrer App.  Beispiel: `https://contoso.azurewebsites.net/.auth/login/microsoftaccount/callback`. Stellen Sie sicher, dass Sie HTTPS-Schema in der URL verwenden.

1. Wählen Sie **Registrieren**.
1. Kopieren Sie die **Anwendungs-ID (Client-ID)** . Sie benötigen die Information später.
1. Wählen Sie im linken Bereich **Zertifikate und Geheimnisse** > **Neuer Client** aus. Geben Sie eine Beschreibung ein, wählen Sie die Gültigkeitsdauer und dann **Hinzufügen** aus.
1. Kopieren Sie den Wert, der auf der Seite **Zertifikate und Geheimnisse** angezeigt wird. Er wird nicht wieder angezeigt, nachdem Sie die Seite verlassen haben.

    > [!IMPORTANT]
    > Bei dem Kennwort handelt es sich um eine wichtige Information für die Anmeldung. Teilen Sie das Kennwort keiner anderen Person mit, und geben Sie es nicht in einer Clientanwendung weiter.

## <a name="secrets"></a>Hinzufügen von Microsoft-Kontoinformationen zu Ihrer App Service-Anwendung

1. Wechseln Sie im [Azure-Portal] zu Ihrer Anwendung.
1. Wählen Sie **Einstellungen** > **Authentifizierung/Autorisierung,** aus, und stellen Sie sicher, dass für die **App Service-Authentifizierung** die Option **Ein** festgelegt ist.
1. Wählen Sie unter **Authentifizierungsanbieter** die Option **Microsoft-Konto** aus. Fügen Sie die Anwendungs(Client)-ID und den geheimen Clientschlüssel ein, die Sie zuvor erhalten haben. Aktivieren Sie alle Bereiche, die von der Anwendung benötigt werden.
1. Klicken Sie auf **OK**.

   App Service bietet Authentifizierung, schränkt aber den autorisierten Zugriff auf den Inhalt und die APIs Ihrer Website nicht ein. Sie müssen die Benutzer in Ihrem App-Code autorisieren.

1. (Optional) Um den Zugriff auf Microsoft-Kontobenutzer zu beschränken, legen Sie **Die auszuführende Aktion, wenn die Anforderung nicht authentifiziert ist** auf **Mit Microsoft-Konto anmelden** fest. Wenn Sie diese Funktion festlegen, erfordert Ihre App, dass alle Anforderungen authentifiziert werden. Sie leitet außerdem alle nicht authentifizierten Anforderungen zur Authentifizierung an das Microsoft-Konto um.

   > [!CAUTION]
   > Das Einschränken des Zugriffs auf diese Weise gilt für alle Aufrufe Ihrer App, was für Apps, die eine öffentlich verfügbare Startseite haben, eventuell nicht wünschenswert ist, wie bei vielen Single-Page-Anwendungen. Bei solchen Anwendungen ist möglicherweise die Einstellung **Anonyme Anforderungen zulassen (keine Aktion)** vorzuziehen, sodass die App die Authentifizierung selbst manuell startet. Weitere Informationen finden Sie unter [Authentifizierungsflow](overview-authentication-authorization.md#authentication-flow).

1. Wählen Sie **Speichern** aus.

Sie können nun ein Microsoft-Konto für die Authentifizierung in Ihrer App verwenden.

## <a name="related-content"> </a>Nächste Schritte

[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- URLs. -->

[My Applications]: https://go.microsoft.com/fwlink/p/?LinkId=262039
[Azure-Portal]: https://portal.azure.com/
