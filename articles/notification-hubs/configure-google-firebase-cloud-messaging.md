---
title: Konfigurieren von Google Firebase Cloud Messaging in Azure Notification Hubs | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie einen Azure Notification Hub mit Google Firebase Cloud Messaging-Einstellungen konfigurieren.
services: notification-hubs
author: sethmanheim
manager: femila
editor: jwargo
ms.service: notification-hubs
ms.workload: mobile
ms.topic: article
ms.date: 03/25/2019
ms.author: sethm
ms.reviewer: jowargo
ms.lastreviewed: 03/25/2019
ms.openlocfilehash: 803289476d4d8869f7828b2843a983ee0c86bfbd
ms.sourcegitcommit: 7df70220062f1f09738f113f860fad7ab5736e88
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2019
ms.locfileid: "71212501"
---
# <a name="configure-google-firebase-cloud-messaging-fcm-settings-for-a-notification-hub-in-the-azure-portal"></a>Konfigurieren von Google Firebase Cloud Messaging-Einstellungen (FCM) für einen Notification Hub im Azure Portal
In diesem Artikel wird gezeigt, wie Sie Google Firebase Cloud Messaging-Einstellungen (FCM) für einen Azure Notification Hub über das Azure Portal konfigurieren.  

## <a name="prerequisites"></a>Voraussetzungen
Wenn Sie noch keinen Notification Hub erstellt haben, erstellen Sie ihn jetzt. Weitere Informationen finden Sie unter [Erstellen einer Azure Notification Hub-Instanz über das Azure-Portal](create-notification-hub-portal.md). 

## <a name="configure-google-firebase-cloud-messaging-fcm"></a>Konfigurieren von Google Firebase Cloud Messaging (FCM)

Die folgende Vorgehensweise beschreibt die Schritte zum Konfigurieren der Google Firebase Cloud Messaging-Einstellungen (FCM) für einen Notification Hub: 

1. Wählen Sie im Azure-Portal auf der Seite **Notification Hub** im Menü auf der linken Seite die Option **Google (GCM/FCM)** aus. 
2. Fügen Sie den **API-Schlüssel** für das FCM-Projekt ein, das Sie zuvor gespeichert haben. 
3. Wählen Sie **Speichern** aus. 

   ![Screenshot, der die Konfiguration von Notification Hubs für Google FCM zeigt](./media/notification-hubs-android-push-notification-google-fcm-get-started/fcm-server-key.png)

## <a name="next-steps"></a>Nächste Schritte
Ein Tutorial mit Schrittanleitungen zum Pushen von Benachrichtigungen an Anwendungen für Android-Geräte mithilfe von Azure Notification Hubs und Google Firebase Cloud Messaging finden Sie unter [Senden von Pushbenachrichtigungen an Android-Geräte mit Notification Hubs und Google FCM](notification-hubs-android-push-notification-google-fcm-get-started.md).

