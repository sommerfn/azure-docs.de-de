---
title: include file
description: include file
services: notification-hubs
author: spelluru
ms.service: notification-hubs
ms.topic: include
ms.date: 03/11/2019
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: 7e047ba57a61d2f327544ec795f640f5066962f6
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2019
ms.locfileid: "67509852"
---
1. Melden Sie sich bei der [Firebase-Konsole](https://firebase.google.com/console/) an. Erstellen Sie ein neues Firebase-Projekt, falls Sie noch keins besitzen.
2. Klicken Sie nach der Erstellung Ihres Projekts auf **Add Firebase to your Android app** (Firebase der Android-App hinzufügen). 

    ![Hinzufügen von Firebase zu Ihrer Android-App](./media/notification-hubs-enable-firebase-cloud-messaging/notification-hubs-add-firebase-to-android-app.png)

3. Führen Sie auf der Seite **Add Firebase to your Android app** (Firebase der Android-App hinzufügen) die folgenden Schritte aus: 
1. 
    1. Geben Sie in **Android-Paketname** einen Namen für Ihr Paket ein. Beispiel: `tutorials.tutoria1.xamarinfcmapp`. 

        ![Angeben des Paketnamens](./media/notification-hubs-enable-firebase-cloud-messaging/specify-package-name-fcm-settings.png)

2. Wählen Sie **Register app** (App registrieren) aus. 
1. 
1. Klicken Sie auf **google-services.json herunterladen**. Speichern Sie dann die Datei im Ordner **app** Ihres Projekts, und klicken Sie auf **Weiter**. 

    ![Herunterladen von „google-services.json“](./media/notification-hubs-enable-firebase-cloud-messaging/download-google-service-button.png)

6. Klicken Sie auf **Weiter**. 
7. Klicken Sie auf **Diesen Schritt überspringen**. 

    ![Den letzten Schritt überspringen](./media/notification-hubs-enable-firebase-cloud-messaging/skip-this-step.png)

8. Klicken Sie in der Firebase-Konsole auf das Zahnrad für Ihr Projekt. Klicken Sie dann auf **Projekteinstellungen**.

    ![Klicken auf „Projekteinstellungen“](./media/notification-hubs-enable-firebase-cloud-messaging/notification-hubs-firebase-console-project-settings.png)

4. Wenn Sie die Datei **google-services.json** nicht heruntergeladen haben, können Sie dies auf dieser Seite tun. 

1. Wechseln Sie oben zur Registerkarte **Cloud Messaging**. 

1. Kopieren und speichern Sie den **Legacy-Serverschlüssel** zur späteren Verwendung. Dieser Wert dient zum Konfigurieren des Notification Hub.
