---
title: include file
description: include file
services: notification-hubs
author: spelluru
ms.service: notification-hubs
ms.topic: include
ms.date: 02/05/2019
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: cd202c98ed605209f5600965ecdb6c0b4c03c17e
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2019
ms.locfileid: "67509098"
---
1. Wählen Sie in Android Studio im Menü die Option **Tools** und dann **SDK Manager** aus. 
2. Wählen Sie die Zielversion des Android SDK aus, das in Ihrem Projekt verwendet wird. Wählen Sie dann **Paketdetails anzeigen** aus. 

    ![Android SDK Manager – Auswählen der Zielversion](./media/notification-hubs-android-studio-add-google-play-services/notification-hubs-android-studio-sdk-manager.png)
3. Wählen Sie die Option **Google-APIs**, falls diese Installation noch nicht durchgeführt wurde.

    ![Android SDK Manager – „Google-APIs“ ausgewählt](./media/notification-hubs-android-studio-add-google-play-services/googole-apis-selected.png)
4. Wechseln Sie auf die Registerkarte **SDK Tools** (SDK-Tools). Wählen Sie wie in der folgenden Abbildung dargestellt die Option **Google Play Services**, falls Google Play Services noch nicht installiert ist. Wählen Sie dann **Anwenden** aus, um die Installation auszuführen. Notieren Sie den SDK-Pfad, den Sie in einem späteren Schritt angeben müssen.

    ![Android SDK Manager – „Google Play Services“ ausgewählt](./media/notification-hubs-android-studio-add-google-play-services/google-play-services-selected.png)
3. Wählen Sie **OK**, wenn das Dialogfeld **Änderung bestätigen** angezeigt wird. Die gewünschten Komponenten werden mit dem entsprechenden Installationsprogramm installiert. Wählen Sie **Fertig stellen**, wenn die Installation der Komponenten abgeschlossen ist.
4. Wählen Sie **OK**, um das Dialogfeld **Settings for New Projects** (Einstellungen für neue Projekte) zu schließen.  
5. Öffnen Sie die Datei „build.gradle“ im Verzeichnis **app** und fügen Sie dann die folgende Zeile unter `dependencies` hinzu. 

    ```gradle
    implementation 'com.google.android.gms:play-services-gcm:16.0.0'
    ```
5. Wählen Sie in der Symbolleiste das Symbol für **Jetzt synchronisieren**.

    ![Synchronisierung mit Gradle](./media/notification-hubs-android-studio-add-google-play-services/gradle-sync.png)
1. Öffnen Sie die Datei „AndroidManifest.xml“, und fügen Sie dann das folgende Tag zum Tag *application* hinzu.

    ```xml
    <meta-data android:name="com.google.android.gms.version"
         android:value="@integer/google_play_services_version" />
    ```
