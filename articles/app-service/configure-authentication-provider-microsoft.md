---
title: 'Konfigurieren der Authentifizierung über ein Microsoft-Konto: Azure App Service'
description: Erfahren Sie, wie Sie die Microsoft-Kontoauthentifizierung für Ihre App Services-Anwendung konfigurieren.
author: mattchenderson
services: app-service
documentationcenter: ''
manager: syntaxc4
editor: ''
ms.assetid: ffbc6064-edf6-474d-971c-695598fd08bf
ms.service: app-service
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 08/08/2019
ms.author: mahender
ms.custom: seodec18
ms.openlocfilehash: 729c235ee51a904714358221389703632fd8296c
ms.sourcegitcommit: 18061d0ea18ce2c2ac10652685323c6728fe8d5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/15/2019
ms.locfileid: "69033636"
---
# <a name="how-to-configure-your-app-service-application-to-use-microsoft-account-login"></a>Konfigurieren Ihrer App Service-Anwendung zur Verwendung der Microsoft-Kontoanmeldung
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

In diesem Thema wird demonstriert, wie Sie Azure App Services zur Verwendung eines Microsoft-Kontos als Authentifizierungsanbieter konfigurieren. 

## <a name="register-microsoft-account"></a>Registrieren Ihrer App mit einem Microsoft-Konto
1. Melden Sie sich beim [Azure-Portal] an, und navigieren Sie zu Ihrer Anwendung. 

<!-- Copy your **URL**, which you will use later to configure your app with Microsoft Account. -->
1. Navigieren Sie zu [**App-Registrierungen**](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade), und melden Sie sich bei entsprechender Aufforderung mit Ihrem Microsoft-Konto an.

1. Klicken Sie auf **Neue Registrierung**, und geben Sie dann einen Anwendungsnamen ein.

1. Wählen Sie in **Umleitungs-URIs** die Option **Web** aus, und geben Sie dann `https://<app-domain-name>/.auth/login/microsoftaccount/callback supply the endpoint for your application` ein. Ersetzen Sie *\<app-domain-name>* durch den Domänennamen Ihrer App.  Beispiel: `https://contoso.azurewebsites.net/.auth/login/microsoftaccount/callback`. 

   > [!NOTE]
   > Verwenden Sie das HTTPS-Schema in der URL.

1. Wählen Sie **Registrieren** aus. 

1. Kopieren Sie die **Anwendungs-ID (Client-ID)** . Sie benötigen ihn später. 
   
7. Wählen Sie in der linken Navigation der neuen App-Registrierung **Zertifikate und Geheimnisse** > **Neuer geheimer Clientschlüssel** aus. Geben Sie eine Beschreibung an, wählen Sie die Gültigkeitsdauer und dann **Hinzufügen** aus.

1. Kopieren Sie den Wert, der auf der Seite **Zertifikate und Geheimnisse** angezeigt wird. Er wird nicht wieder angezeigt, nachdem Sie die Seite verlassen haben.

    > [!IMPORTANT]
    > Bei dem Kennwort handelt es sich um eine wichtige Information für die Anmeldung. Teilen Sie das Kennwort keiner anderen Person mit, und geben Sie es nicht in einer Clientanwendung weiter.

## <a name="secrets"></a>Hinzufügen von Microsoft-Kontoinformationen zu Ihrer App Service-Anwendung
1. Navigieren Sie im [Azure-Portal]zu Ihrer Anwendung. Wählen Sie im linken Navigationsbereich **Authentifizierung/Autorisierung** aus.

2. Falls die Funktion „Authentifizierung/Autorisierung“ nicht aktiviert ist, wählen Sie **Ein** aus.

3. Wählen Sie unter **Authentifizierungsanbieter** die Option **Microsoft-Konto** aus. Fügen Sie die Anwendungs-ID (Client-ID) und den geheimen Clientschlüssel ein, die Sie zuvor erhalten haben, und aktivieren Sie optional alle Bereiche, die für Ihre Anwendung erforderlich sind. Klicken Sie dann auf **OK**.

    Standardmäßig erfolgt die Authentifizierung über App Service, wobei jedoch der Zugriff auf die Inhalte Ihrer Website und APIs nicht autorisiert wird. Sie müssen die Benutzer in Ihrem App-Code autorisieren.

4. (Optional) Um den Zugriff auf Microsoft-Kontobenutzer zu beschränken, legen Sie **Die auszuführende Aktion, wenn die Anforderung nicht authentifiziert ist** auf **Mit Microsoft-Konto anmelden** fest. Dadurch müssen alle Anforderungen authentifiziert werden. Alle nicht authentifizierten Anforderungen werden zur Authentifizierung an Microsoft umgeleitet.

> [!CAUTION]
> Das Einschränken des Zugriffs auf diese Weise gilt für alle Aufrufe Ihrer App, was für Apps, die eine öffentlich verfügbare Startseite wünschen, eventuell nicht wünschenswert ist, wie bei vielen Single-Page-Anwendungen. Bei solchen Anwendungen ist möglicherweise die Einstellung **Anonyme Anforderungen zulassen (keine Aktion)** vorzuziehen, wobei die App selbst die Anmeldung manuell startet, wie [hier](overview-authentication-authorization.md#authentication-flow) beschrieben.

5. Klicken Sie auf **Speichern**.

Sie können nun ein Microsoft-Konto für die Authentifizierung in Ihrer App verwenden.

## <a name="related-content"></a>Verwandte Inhalte
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- URLs. -->

[My Applications]: https://go.microsoft.com/fwlink/p/?LinkId=262039
[Azure-Portal]: https://portal.azure.com/
