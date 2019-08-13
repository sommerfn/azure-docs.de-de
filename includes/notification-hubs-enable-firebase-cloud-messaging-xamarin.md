---
title: include file
description: include file
services: notification-hubs
author: spelluru
ms.service: notification-hubs
ms.topic: include
ms.date: 08/01/2019
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: 45bdd569741dc13181bcaf9e8587a02b3d02c621
ms.sourcegitcommit: 85b3973b104111f536dc5eccf8026749084d8789
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/01/2019
ms.locfileid: "68728822"
---
1. Melden Sie sich bei der [Firebase-Konsole](https://firebase.google.com/console/) an. Erstellen Sie ein neues Firebase-Projekt, falls Sie noch keins besitzen.
2. Klicken Sie nach der Erstellung Ihres Projekts auf **Add Firebase to your Android app** (Firebase der Android-App hinzufügen). 

    ![Hinzufügen von Firebase zu Ihrer Android-App](./media/notification-hubs-enable-firebase-cloud-messaging/notification-hubs-add-firebase-to-android-app.png)

3. Führen Sie auf der Seite **Add Firebase to your Android app** (Firebase der Android-App hinzufügen) die folgenden Schritte aus: 
    1. Geben Sie in **Android-Paketname** einen Namen für Ihr Paket ein. Beispiel: `tutorials.tutoria1.xamarinfcmapp`. 

        ![Angeben des Paketnamens](./media/notification-hubs-enable-firebase-cloud-messaging/specify-package-name-fcm-settings.png)
    2. Wählen Sie **Register app** (App registrieren) aus.  
    1. Klicken Sie auf **google-services.json herunterladen**. Speichern Sie dann die Datei im Ordner Ihres Projekts, und wählen Sie **Weiter** aus. Falls Sie das Visual Studio-Projekt noch nicht erstellt haben, können Sie diesen Schritt nach der Projekterstellung ausführen. 

        ![Herunterladen von „google-services.json“](./media/notification-hubs-enable-firebase-cloud-messaging/download-google-service-button.png)
    6. Klicken Sie auf **Weiter**. 
    7. Klicken Sie auf **Diesen Schritt überspringen**. 

        ![Den letzten Schritt überspringen](./media/notification-hubs-enable-firebase-cloud-messaging/skip-this-step.png)
8. Klicken Sie in der Firebase-Konsole auf das Zahnrad für Ihr Projekt. Klicken Sie dann auf **Projekteinstellungen**.

    ![Klicken auf „Projekteinstellungen“](./media/notification-hubs-enable-firebase-cloud-messaging/notification-hubs-firebase-console-project-settings.png)
4. Wenn Sie die Datei **google-services.json** nicht heruntergeladen haben, können Sie dies auf dieser Seite tun. 

    ![Herunterladen der Datei „google-services.json“ über die Registerkarte „Allgemein“](./media/notification-hubs-enable-firebase-cloud-messaging/download-google-services-json-general-page.png)
1. Wechseln Sie oben zur Registerkarte **Cloud Messaging**. Kopieren und speichern Sie den **Serverschlüssel** für eine spätere Verwendung. Dieser Wert dient zum Konfigurieren des Notification Hub.

    ![Kopieren des Serverschlüssels](./media/notification-hubs-enable-firebase-cloud-messaging/server-key.png)
