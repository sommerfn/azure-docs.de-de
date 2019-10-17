---
title: Hinzufügen von Pushbenachrichtigungen zur Xamarin Android-App | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Azure App Service und Azure Notification Hubs verwenden, um Pushbenachrichtigungen an Ihre Xamarin.Android-App zu senden.
services: app-service\mobile
documentationcenter: xamarin
author: elamalani
manager: crdun
editor: ''
ms.assetid: 6f7e8517-e532-4559-9b07-874115f4c65b
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: article
ms.date: 06/25/2019
ms.author: emalani
ms.openlocfilehash: 25e0196fec94acb363757e74fa71f666a4c9d11c
ms.sourcegitcommit: bb65043d5e49b8af94bba0e96c36796987f5a2be
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2019
ms.locfileid: "72388484"
---
# <a name="add-push-notifications-to-your-xamarinandroid-app"></a>Hinzufügen von Pushbenachrichtigungen zur Xamarin.Android-App

[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

> [!NOTE]
> Visual Studio App Center unterstützt End-to-End- und integrierte Dienste, die für die Entwicklung mobiler Apps von zentraler Bedeutung sind. Entwickler können **Build**-, **Test**- und **Verteilungs**dienste nutzen, um eine Pipeline für Continuous Integration und Delivery einzurichten. Nach der Bereitstellung der App können Entwickler den Status und die Nutzung ihrer App mithilfe der **Analyse**- und **Diagnose**dienste überwachen und mit Benutzern über den **Push**dienst interagieren. Entwickler können auch den **Authentifizierung**sdienst nutzen, um ihre Benutzer zu authentifizieren, und den **Daten**dienst, um App-Daten dauerhaft in der Cloud zu speichern und zu synchronisieren.
>
> Falls Sie Clouddienste in Ihre mobile Anwendung integrieren möchten, sollten Sie sich noch heute für [App Center](https://appcenter.ms/?utm_source=zumo&utm_medium=Azure&utm_campaign=zumo%20doc) registrieren.

## <a name="overview"></a>Übersicht

In diesem Tutorial fügen Sie Pushbenachrichtigungen zum [Xamarin.Android-Schnellstartprojekt](app-service-mobile-windows-store-dotnet-get-started.md) hinzu, damit jedes Mal, wenn ein Datensatz eingefügt wird, eine Pushbenachrichtigung an das Gerät gesendet wird.

Wenn Sie das heruntergeladene Schnellstart-Serverprojekt nicht verwenden, müssen Sie Ihrem Projekt das Erweiterungspaket für Pushbenachrichtigungen hinzufügen. Weitere Informationen finden Sie im Leitfaden [Arbeiten mit dem .NET Back-End-Server SDK für Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial ist das folgende Setup erforderlich:

* Ein aktives Google-Konto Sie können sich unter [accounts.google.com](https://go.microsoft.com/fwlink/p/?LinkId=268302) für ein Google-Konto registrieren.
* [Google Cloud Messaging Client-Komponente](https://components.xamarin.com/view/GCMClient/).

## <a name="configure-hub"></a>Konfigurieren eines Notification Hubs

[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a id="register"></a>Aktivieren von Firebase Cloud Messaging

[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="configure-azure-to-send-push-requests"></a>Konfigurieren von Azure zum Senden von Pushanforderungen

[!INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push-for-firebase.md)]

## <a id="update-server"></a>Aktualisieren des Serverprojekts zum Senden von Pushbenachrichtigungen

[!INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

## <a id="configure-app"></a>Konfigurieren des Clientprojekts für Pushbenachrichtigungen

[!INCLUDE [mobile-services-xamarin-android-push-configure-project](../../includes/mobile-services-xamarin-android-push-configure-project.md)]

## <a id="add-push"></a>Hinzufügen von Pushbenachrichtigungen zur App

[!INCLUDE [app-service-mobile-xamarin-android-push-add-to-app](../../includes/app-service-mobile-xamarin-android-push-add-to-app.md)]

## <a name="test"></a>Testen von Pushbenachrichtigungen in der App

Sie können die App mithilfe eines virtuellen Geräts im Emulgator testen. Es sind zusätzliche Konfigurationsschritte erforderlich, wenn für das Ausführen ein Emulgator verwendet wird.

1. Für das virtuelle Gerät müssen im Android Virtual Device Manager Google-APIs als Ziel festgelegt sein.

    ![](./media/app-service-mobile-xamarin-android-get-started-push/google-apis-avd-settings.png)

2. Fügen Sie auf dem Android-Gerät ein Google-Konto hinzu, indem Sie auf **Apps** > **Einstellungen** > **Konto hinzufügen** klicken und anschließend die Anweisungen befolgen.

    ![](./media/app-service-mobile-xamarin-android-get-started-push/add-google-account.png)

3. Führen Sie wie zuvor die todolist-App aus und fügen Sie ein neues todo-Element ein. Dieses Mal wird im Benachrichtigungsbereich ein Benachrichtigungssymbol angezeigt. Öffnen Sie die Benachrichtigungsschublade, um den vollständigen Text der Benachrichtigung anzuzeigen.

    ![](./media/app-service-mobile-xamarin-android-get-started-push/android-notifications.png)

<!-- URLs. -->
[Xamarin.Android quick start]: app-service-mobile-xamarin-android-get-started.md
[Google Cloud Messaging Client Component]: https://components.xamarin.com/view/GCMClient/
[Azure Mobile Services Component]: https://components.xamarin.com/view/azure-mobile-services/
