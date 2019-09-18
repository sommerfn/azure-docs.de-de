---
title: Konfigurieren von Baidu Cloud Push in Azure Notification Hubs | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Baidu-Einstellungen für einen Azure Notification Hub konfigurieren.
services: notification-hubs
author: jwargo
manager: patniko
ms.service: notification-hubs
ms.workload: mobile
ms.topic: article
ms.date: 03/25/2019
ms.author: jowargo
ms.openlocfilehash: 9feb5f66c5ae979a239c3349b2b1347ad307ce49
ms.sourcegitcommit: 7c5a2a3068e5330b77f3c6738d6de1e03d3c3b7d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2019
ms.locfileid: "70884650"
---
# <a name="deprecated-configure-baidu-cloud-push-settings-for-a-notification-hub-in-the-azure-portal"></a>Veraltet: Konfigurieren von Baidu Cloud Push-Einstellungen (ADM) für einen Notification Hub im Azure Portal

In diesem Artikel wird gezeigt, wie Sie Baidu Cloud Push-Einstellungen für einen Azure Notification Hub über das Azure-Portal konfigurieren.

> [!IMPORTANT]
> Dieses Tutorial ist veraltet. 

## <a name="prerequisites"></a>Voraussetzungen
Wenn Sie noch keinen Notification Hub erstellt haben, erstellen Sie ihn jetzt. Weitere Informationen finden Sie unter [Erstellen einer Azure Notification Hub-Instanz über das Azure-Portal](create-notification-hub-portal.md). 

## <a name="configure-baidu-cloud-push"></a>Konfigurieren von Baidu Cloud Push
Die folgende Vorgehensweise beschreibt die Schritte zum Konfigurieren der Baidu Cloud Push-Einstellungen für einen Notification Hub:

1. Wählen Sie im Azure-Portal auf der Seite **Notification Hub** im Menü auf der linken Seite die Option **Baidu (Android China)** aus. 
2. Geben Sie den **API-Schlüssel** aus der Baidu-Konsole in das Baidu Cloud Push-Projekt ein. 
3. Geben Sie den **Geheimen Schlüssel** aus der Baidu-Konsole in das Baidu Cloud Push-Projekt ein. 
4. Wählen Sie **Speichern** aus. 

    ![Screenshot von Notification Hubs, der die Konfiguration von Pushbenachrichtigungen für Baidu (Android China) anzeigt](./media/notification-hubs-baidu-get-started/AzureNotificationServicesBaidu.png)

## <a name="next-steps"></a>Nächste Schritte
Ein Tutorial mit Schrittanleitungen zum Pushen von Benachrichtigungen an Baidu mithilfe von Azure Notification Hubs und Baidu Cloud Push finden Sie unter [Erste Schritte mit Notification Hubs mithilfe von Baidu](notification-hubs-baidu-china-android-notifications-get-started.md).
