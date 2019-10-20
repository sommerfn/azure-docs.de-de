---
title: Azure Notification Hubs iOS 13-Updates | Microsoft-Dokumentation
description: Erfahren Sie mehr über Breaking Changes für iOS 13 in Azure Notification Hubs
author: sethmanheim
ms.author: sethm
ms.date: 10/16/2019
ms.topic: article
ms.service: notification-hubs
ms.reviewer: jowargo
ms.lastreviewed: 10/16/2019
ms.openlocfilehash: 2bb66c52e48e2e872d7f67bfdea88602ba12e5de
ms.sourcegitcommit: 12de9c927bc63868168056c39ccaa16d44cdc646
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "72518214"
---
# <a name="azure-notification-hubs-updates-for-ios-13"></a>Azure Notification Hubs-Updates für iOS 13

Apple hat kürzlich einige Änderungen am öffentlichen Pushdienst vorgenommen. Die Änderungen lehnen sich größtenteils an die Releases von iOS 13 und Xcode an. Dieser Artikel beschreibt die Auswirkungen dieser Änderungen auf Azure Notification Hubs.

## <a name="apns-push-payload-changes"></a>Änderungen der APNs-Pushnutzlast

### <a name="apns-push-type"></a>APNs-Pushtyp

Apple verlangt nun, dass Entwickler Benachrichtigungen als Warn- oder Hintergrundbenachrichtigungen über den neuen `apns-push-type`-Header in der APNs-API identifizieren. Gemäß der [Apple Dokumentation](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/sending_notification_requests_to_apns) gilt Folgendes: „Der Wert dieses Headers muss den Inhalt der Nutzlast Ihrer Benachrichtigung genau widerspiegeln. Wenn keine Übereinstimmung vorliegt oder wenn der Header für erforderliche Systeme fehlt, gibt APNs möglicherweise einen Fehler zurück, verzögert die Übermittlung der Benachrichtigung oder verwirft sie vollständig.“

Entwickler müssen diesen Header jetzt in Anwendungen festlegen, die Benachrichtigungen über Azure Notification Hubs senden. Aufgrund einer technischen Einschränkung müssen Kunden tokenbasierte Authentifizierung für APNs-Anmeldeinformationen mit Anforderungen verwenden, die dieses Attribut enthalten. Wenn Sie zertifikatbasierte Authentifizierung für Ihre APNs-Anmeldeinformationen verwenden, müssen Sie auf tokenbasierte Authentifizierung umsteigen.

In den folgenden Codebeispielen wird gezeigt, wie Sie dieses Headerattribut in Benachrichtigungsanforderungen festlegen, die über Azure Notification Hubs gesendet werden.

#### <a name="template-notifications---net-sdk"></a>Vorlagenbenachrichtigungen: .NET SDK

```csharp
var hub = NotificationHubClient.CreateFromConnectionString(...);
var headers = new Dictionary<string, string> {{"apns-push-type",
"alert"}};
var tempprop = new Dictionary<string, string> {{"message", "value"}};
var notification = new TemplateNotification(tempprop);
notification.Headers = headers;
await hub.SendNotificationAsync(notification);
```

#### <a name="native-notifications---net-sdk"></a>Native Benachrichtigungen: .NET SDK

```csharp
var hub = NotificationHubClient.CreateFromConnectionString(...);
var headers = new Dictionary<string, string> {{"apns-push-type", "alert"}};
var notification = new ApnsNotification("notification text", headers);
await hub.SendNotificationAsync(notification);
```

#### <a name="direct-rest-calls"></a>Direkte REST-Aufrufe

```csharp
var request = new HttpRequestMessage(method, $"<resourceUri>?api-version=2017-04");
request.Headers.Add("Authorization", createToken(resourceUri, KEY_NAME, KEY_VALUE));
request.Headers.Add("ServiceBusNotification-Format", "apple");
request.Headers.Add("apns-push-type", "alert");
```

Wenn Azure Notification Hubs eine Benachrichtigung erkennt, in der der Wert `apns-push-type` nicht festgelegt ist, leitet der Dienst den Pushtyp aus der Benachrichtigungsanforderung ab und legt den Wert automatisch fest, um Sie während dieses Übergangs zu unterstützen. Beachten Sie, dass Sie Azure Notification Hubs für die Verwendung von tokenbasierter Authentifizierung konfigurieren müssen, um den erforderlichen Header festzulegen. Weitere Informationen finden Sie unter [Tokenbasierte Authentifizierung (HTTP/2) für APNs](notification-hubs-push-notification-http2-token-authentification.md).

## <a name="apns-priority"></a>APNs-Priorität

Eine weitere geringfügige Änderung (aber eine Änderung, die eine Änderung an der Back-End-Anwendung erfordert, die Benachrichtigungen sendet) ist die Anforderung, dass für Hintergrundbenachrichtigungen der `apns-priority`-Header jetzt auf 5 festgelegt werden muss. Zahlreiche Anwendungen legen den `apns-priority`-Header auf 10 (was sofortige Übermittlung bedeutet) oder gar nicht fest und verwenden so den Standardwert (der ebenfalls 10 lautet).

Das Festlegen dieses Werts auf 10 ist für Hintergrundbenachrichtigungen nicht mehr zulässig, und Sie müssen den Wert für jede Anforderung festlegen. Apple übermittelt keine Hintergrundbenachrichtigungen, wenn dieser Wert fehlt. Beispiel:

```csharp
var hub = NotificationHubClient.CreateFromConnectionString(...);
var headers = new Dictionary<string, string> {{"apns-push-type", "background"}, { "apns-priority", "5" }};
var notification = new ApnsNotification("notification text", headers);
await hub.SendNotificationAsync(notification);
```

## <a name="sdk-changes"></a>SDK-Änderungen

Jahrelang haben iOS-Entwickler das `description`-Attribut der an den Pushtokendelegaten gesendeten `deviceToken`-Daten verwendet, um das Pushtoken zu extrahieren, das eine Back-End-Anwendung zum Senden von Benachrichtigungen an das Gerät verwendet. Mit Xcode 11 hat sich dieses `description`-Attribut in ein anderes Format geändert. Vorhandener Code, den Entwickler für dieses Attribut verwendet haben, funktioniert jetzt nicht mehr. Wir haben das Azure Notification Hubs SDK aktualisiert, um diese Änderung zu berücksichtigen. Aktualisieren Sie daher das SDK, das von Ihren Anwendungen verwendet wird, auf Version 2.0.4 oder höher des [Azure Notification Hubs iOS SDK](https://github.com/Azure/azure-notificationhubs-ios).
