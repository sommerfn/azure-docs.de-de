---
title: Hinzufügen von Pushbenachrichtigungen zur iOS-App mit Azure Mobile Apps
description: Erfahren Sie mehr über die Verwendung von Azure Mobile Apps zum Senden von Pushbenachrichtigungen an Ihre iOS-App.
services: app-service\mobile
documentationcenter: ios
manager: crdun
editor: ''
author: elamalani
ms.assetid: fa503833-d23e-4925-8d93-341bb3fbab7d
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 06/25/2019
ms.author: emalani
ms.openlocfilehash: 5ab968e88331f888dfcecd2cc30a658b0b0f53ec
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2019
ms.locfileid: "67445361"
---
# <a name="add-push-notifications-to-your-ios-app"></a>Hinzufügen von Pushbenachrichtigungen zu Ihrer iOS-App

[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

> [!NOTE]
> Im Rahmen von Visual Studio App Center wird in neue und integrierte Dienste investiert, die für die Entwicklung mobiler Apps von zentraler Bedeutung sind. Entwickler können **Build**-, **Test**- und **Verteilungs**dienste nutzen, um eine Pipeline für Continuous Integration und Delivery einzurichten. Nach der Bereitstellung der App können Entwickler den Status und die Nutzung ihrer App mithilfe der **Analyse**- und **Diagnose**dienste überwachen und mit Benutzern über den **Push**dienst interagieren. Entwickler können auch den **Authentifizierung**sdienst nutzen, um ihre Benutzer zu authentifizieren, und den **Daten**dienst, um App-Daten dauerhaft in der Cloud zu speichern und zu synchronisieren. Besuchen Sie noch heute das [App Center](https://appcenter.ms/?utm_source=zumo&utm_campaign=app-service-mobile-ios-get-started-push).
>

## <a name="overview"></a>Übersicht

In diesem Tutorial fügen Sie Pushbenachrichtigungen zum [iOS-Schnellstart] hinzu, damit jedes Mal, wenn ein Datensatz eingefügt wird, eine Pushbenachrichtigung an das Gerät gesendet wird.

Wenn Sie das heruntergeladene Schnellstart-Serverprojekt nicht verwenden, müssen Sie Ihrem Projekt das Erweiterungspaket für Pushbenachrichtigungen hinzufügen. Weitere Informationen finden Sie im Leitfaden [Arbeiten mit dem .NET Back-End-Server SDK für Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

Der [iOS-Simulator unterstützt keine Pushbenachrichtigungen](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html). Sie benötigen ein physisches iOS-Gerät und eine [Apple-Entwicklerprogramm-Mitgliedschaft](https://developer.apple.com/programs/ios/).

## <a name="configure-hub"></a>Konfigurieren eines Notification Hubs

[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a id="register"></a>Registrieren der App für Pushbenachrichtigungen

[!INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

## <a name="configure-azure-to-send-push-notifications"></a>Konfigurieren von Azure zum Senden von Pushbenachrichtigungen

[!INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

## <a id="update-server"></a>Aktualisieren des Back-Ends zum Senden von Pushbenachrichtigungen

[!INCLUDE [app-service-mobile-dotnet-backend-configure-push-apns](../../includes/app-service-mobile-dotnet-backend-configure-push-apns.md)]

## <a id="add-push"></a>Hinzufügen von Pushbenachrichtigungen zur App

[!INCLUDE [app-service-mobile-add-push-notifications-to-ios-app.md](../../includes/app-service-mobile-add-push-notifications-to-ios-app.md)]

## <a id="test"></a>Testen von Pushbenachrichtigungen

[!INCLUDE [Test Push Notifications in App](../../includes/test-push-notifications-in-app.md)]

## <a id="more"></a>Weitere Informationen

* Vorlagen ermöglichen Ihnen, flexibel plattformübergreifende Pushvorgänge und lokalisierte Pushvorgänge zu senden. [Verwenden der iOS-Clientbibliothek für Azure Mobile Apps](app-service-mobile-ios-how-to-use-client-library.md#templates) wird erklärt, wie Sie Vorlagen registrieren.

<!-- Anchors.  -->

<!-- Images. -->

<!-- URLs. -->
[iOS-Schnellstart]: app-service-mobile-ios-get-started.md
