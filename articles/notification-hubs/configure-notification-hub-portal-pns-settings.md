---
title: Einrichten von Pushbenachrichtigungen in Azure Notification Hubs | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Azure Notification Hubs im Azure-Portal mit den Einstellungen des Plattformbenachrichtigungssystems (Platform Notification System, PNS) einrichten.
services: notification-hubs
author: sethmanheim
manager: femila
editor: jwargo
ms.service: notification-hubs
ms.workload: mobile
ms.topic: quickstart
ms.date: 02/14/2019
ms.author: sethm
ms.reviewer: jowargo
ms.lastreviewed: 02/14/2019
ms.openlocfilehash: 951f03f581906e45946ef75742421ba27d405267
ms.sourcegitcommit: dd0304e3a17ab36e02cf9148d5fe22deaac18118
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/22/2019
ms.locfileid: "74406966"
---
# <a name="set-up-push-notifications-in-a-notification-hub-in-the-azure-portal"></a>Einrichten von Pushbenachrichtigungen in einem Notification Hub im Azure-Portal

Azure Notification Hubs bietet ein Pushmodul, das einfach zu verwenden ist und horizontal hochskaliert. Mit Notification Hubs können Sie Benachrichtigungen an beliebige Plattformen (iOS, Android, Windows, Baidu) und von jedem Back-End (Cloud oder lokal) senden. Weitere Informationen finden Sie unter [Was ist Azure Notification Hubs?](notification-hubs-push-notification-overview.md).

In diesem Schnellstart verwenden Sie die Einstellungen des Plattformbenachrichtigungssystems (PNS) in Notification Hubs zum Einrichten von Pushbenachrichtigungen auf mehreren Plattformen. Der Schnellstart zeigt Ihnen die Schritte, die Sie im Azure-Portal ausführen müssen.

Wenn Sie noch keinen Notification Hub erstellt haben, erstellen Sie ihn jetzt. Weitere Informationen finden Sie unter [Erstellen einer Azure Notification Hub-Instanz über das Azure-Portal](create-notification-hub-portal.md). 

## <a name="apple-push-notification-service"></a>Apple Push Notification Service

So richten Sie einen Apple Push Notification Service (APNS) ein:

1. Wählen Sie im Azure-Portal auf der Seite **Notification Hub** im Menü auf der linken Seite die Option **Apple (APNS)** aus.

1. Wählen Sie für **Authentifizierungsmodus** entweder **Zertifikat** oder **Token** aus.

   a. Wenn Sie **Zertifikat** auswählen:
   * Wählen Sie das Dateisymbol und anschließend die hochzuladende *P12*-Datei aus.
   * Geben Sie ein Kennwort ein.
   * Wählen Sie den Modus **Sandbox** aus. Alternativ können Sie den Modus **Produktion** auswählen, um Pushbenachrichtigungen an Benutzer zu senden, die Ihre App im Store erworben haben.

     ![Screenshot einer APNS-Zertifikatkonfiguration im Azure-Portal](./media/notification-hubs-ios-get-started/notification-hubs-apple-config-cert.png)

   b. Wenn Sie **Token** auswählen:

   * Geben Sie die Werte für **Schlüssel-ID**, **Bündel-ID**, **Team-ID** und **Token** ein.
   * Wählen Sie den Modus **Sandbox** aus. Alternativ können Sie den Modus **Produktion** auswählen, um Pushbenachrichtigungen an Benutzer zu senden, die Ihre App im Store erworben haben.

     ![Screenshot einer APNS-Tokenkonfiguration im Azure-Portal](./media/configure-notification-hub-portal-pns-settings/notification-hubs-apple-config-token.png)

Weitere Informationen finden Sie unter [Tutorial: Senden von Pushbenachrichtigungen an iOS-Apps mit Azure Notification Hubs](notification-hubs-ios-apple-push-notification-apns-get-started.md).

## <a name="google-firebase-cloud-messaging"></a>Google Firebase Cloud Messaging

So richten Sie Pushbenachrichtigungen für Google Firebase Cloud Messaging (FCM) ein:

1. Wählen Sie im Azure-Portal auf der Seite **Notification Hub** im Menü auf der linken Seite die Option **Google (GCM/FCM)** aus. 
2. Fügen Sie den **API-Schlüssel** für das FCM-Projekt ein, das Sie zuvor gespeichert haben. 
3. Wählen Sie **Speichern** aus. 

   ![Screenshot, der die Konfiguration von Notification Hubs für Google FCM zeigt](./media/notification-hubs-android-push-notification-google-fcm-get-started/fcm-server-key.png)

Wenn Sie diese Schritte abgeschlossen haben, wird angezeigt, dass der Notification Hub erfolgreich aktualisiert wurde. Die Schaltfläche **Save** (Speichern) ist deaktiviert. 

Weitere Informationen finden Sie unter [Tutorial: Senden von Pushbenachrichtigungen an Android-Geräte mit Azure Notification Hubs und Google Firebase Cloud Messaging](notification-hubs-android-push-notification-google-fcm-get-started.md).

## <a name="windows-push-notification-service"></a>Windows-Pushbenachrichtigungsdienst (Windows Push Notification Service, WNS)

So richten Sie den Windows-Pushbenachrichtigungsdienst (WNS) ein:

1. Wählen Sie im Azure-Portal auf der Seite **Notification Hub** im Menü auf der linken Seite die Option **Windows (WNS)** aus.
2. Geben Sie Werte für **Paket-SID** und **Sicherheitsschlüssel** ein.
3. Wählen Sie **Speichern** aus.

   ![Screenshot, der die Felder „Paket-SID“ und „Sicherheitsschlüssel“ zeigt](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-configure-wns.png)

Weitere Informationen finden Sie unter [Tutorial: Senden von Benachrichtigungen an Apps für die universelle Windows-Plattform mit Azure Notification Hubs](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md).

## <a name="microsoft-push-notification-service-for-windows-phone"></a>Microsoft-Pushbenachrichtigungsdienst für Windows Phone

So richten Sie den Microsoft-Pushbenachrichtigungsdienst (Microsoft Push Notification Service, MPNS) für Windows Phone ein: 

1. Wählen Sie im Azure-Portal auf der Seite **Notification Hub** im Menü auf der linken Seite die Option **Windows Phone (MPNS)** aus.
1. Aktivieren Sie entweder nicht authentifizierte oder authentifizierte Pushbenachrichtigungen:

   a. Um nicht authentifizierte Pushbenachrichtigungen zu aktivieren, wählen Sie **Nicht authentifizierte Pushbenachrichtigungen zulassen** > **Speichern** aus.

      ![Screenshot, der das Aktivieren nicht authentifizierter Pushbenachrichtigungen zeigt](./media/notification-hubs-windows-phone-get-started/azure-portal-unauth.png)

   b. So aktivieren Sie authentifizierte Pushbenachrichtigungen:
      * Wählen Sie auf der Symbolleiste die Option **Zertifikat hochladen** aus.
      * Wählen Sie das Dateisymbol und anschließend die Zertifikatdatei aus.
      * Geben Sie das Kennwort für das Zertifikat ein.
      * Klicken Sie auf **OK**.
      * Wählen Sie auf der Seite **Windows Phone (MPNS)** die Option **Speichern** aus.

Weitere Informationen finden Sie unter [Tutorial: Senden von Pushbenachrichtigungen an Windows Phone-Apps mit Azure Notification Hubs](notification-hubs-windows-mobile-push-notifications-mpns.md).
      

## <a name="baidu-android-china"></a>Baidu (Android China)

So richten Sie Pushbenachrichtigungen für Baidu ein:

1. Wählen Sie im Azure-Portal auf der Seite **Notification Hub** im Menü auf der linken Seite die Option **Baidu (Android China)** aus. 
2. Geben Sie den **API-Schlüssel** aus der Baidu-Konsole in das Baidu Cloud Push-Projekt ein. 
3. Geben Sie den **Geheimen Schlüssel** aus der Baidu-Konsole in das Baidu Cloud Push-Projekt ein. 
4. Wählen Sie **Speichern** aus. 

    ![Screenshot von Notification Hubs, der die Konfiguration von Pushbenachrichtigungen für Baidu (Android China) anzeigt](./media/notification-hubs-baidu-get-started/AzureNotificationServicesBaidu.png)

Wenn Sie diese Schritte abgeschlossen haben, wird angezeigt, dass der Notification Hub erfolgreich aktualisiert wurde. Die Schaltfläche **Save** (Speichern) ist deaktiviert. 

Weitere Informationen finden Sie unter [Erste Schritte mit Notification Hubs mit Baidu](notification-hubs-baidu-china-android-notifications-get-started.md).

## <a name="next-steps"></a>Nächste Schritte
In dieser Schnellstartanleitung haben Sie gelernt, wie Sie im Azure-Portal Benachrichtigungssystemeinstellungen für einen Notification Hub konfigurieren. 

Weitere Informationen zum Senden von Pushbenachrichtigungen an verschiedene Plattformen finden Sie in diesen Tutorials:

- [Senden von Pushbenachrichtigungen an iOS-Apps mit Azure Notification Hubs](notification-hubs-ios-apple-push-notification-apns-get-started.md)
- [Senden von Pushbenachrichtigungen an Android-Geräte mit Azure Notification Hubs und Google Firebase Cloud Messaging](notification-hubs-android-push-notification-google-fcm-get-started.md)
- [Senden von Benachrichtigungen an Apps für die universelle Windows-Plattform mit Azure Notification Hubs](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md)
- [Senden von Pushbenachrichtigungen an Windows Phone-Apps mit Azure Notification Hubs](notification-hubs-windows-mobile-push-notifications-mpns.md)
- [Erste Schritte mit Notification Hubs mit Baidu](notification-hubs-baidu-china-android-notifications-get-started.md)
