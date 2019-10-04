---
title: Erste Schritte mit dem Android-Kartensteuerelement in Azure Maps | Microsoft-Dokumentation
description: Das Kartensteuerelement für Android in Azure Maps.
author: walsehgal
ms.author: v-musehg
ms.date: 04/26/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 934fe2219ccca917999cf49cb9c9826276545e73
ms.sourcegitcommit: 083aa7cc8fc958fc75365462aed542f1b5409623
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2019
ms.locfileid: "70915661"
---
# <a name="getting-started-with-azure-maps-android-sdk"></a>Erste Schritte mit dem Android SDK für Azure Maps

Das Android SDK in Azure Maps ist eine Vektorenzuordnungsbibliothek für Android. Dieser Artikel führt Sie durch die Installation des Android SDK für Azure Maps und das Laden einer Karte.

## <a name="prerequisites"></a>Voraussetzungen

### <a name="create-an-azure-maps-account"></a>Erstellen eines Azure Maps-Kontos

Zunächst müssen Sie im S1-Tarif [ein Azure Maps-Konto erstellen](https://docs.microsoft.com/azure/azure-maps/how-to-manage-account-keys#create-a-new-account), damit Sie die in diesem Artikel beschriebenen Schritte ausführen können.

### <a name="download-android-studio"></a>Herunterladen von Android Studio

Laden Sie Android Studio herunter, und erstellen Sie ein Projekt mit einer leeren Aktivität, bevor Sie das Android SDK für Azure Maps installieren. Das [Herunterladen von Android Studio](https://developer.android.com/studio/) ist kostenlos über Google möglich. 

## <a name="create-a-project-in-android-studio"></a>Erstellen eines Projekts in Android Studio

Erstellen Sie zuerst ein neues Projekt mit einer leeren Aktivität. Gehen Sie wie folgt vor, um ein Android Studio-Projekt zu erstellen:

1. Klicken Sie unter **Ihr Projekt auswählen** auf **Telefon und Tablet**. Ihre Anwendung wird auf diesem Formfaktor ausgeführt.
2. Wählen Sie auf der Registerkarte **Telefon und Tablet** die Option **Leere Aktivität** aus und klicken Sie dann auf **Weiter**.
3. Wählen Sie unter **Ihr Projekt konfigurieren** die Option `API 21: Android 5.0.0 (Lollipop)` als das minimale SDK aus. Dies ist die niedrigste Version, die vom Android SDK in Azure Maps unterstützt wird.
4. Übernehmen Sie die Standardwerte `Activity Name` und `Layout Name`, und klicken Sie auf **Fertig stellen**.

Weitere Informationen zur Installation von Android Studio und zur Erstellung eines neuen Projekts finden Sie in der [Dokumentation zu Android Studio](https://developer.android.com/studio/intro/).

![Erstellen eines Projekts](./media/how-to-use-android-map-control-library/form-factor-android.png)

## <a name="set-up-a-virtual-device"></a>Einrichten eines virtuellen Geräts

Mit Android Studio können Sie ein virtuelles Android-Gerät auf Ihrem Computer einrichten. Auf diese Weise können Sie Ihre Anwendung während der Entwicklung testen. Klicken Sie auf das Symbol für den Manager für virtuelle Android-Geräte (AVD) rechts oben auf Ihrem Projektbildschirm und anschließend auf **Virtuelles Gerät erstellen**, um ein virtuelles Gerät einzurichten. Sie können den AVD-Manager auch über die Symbolleiste aufrufen, indem Sie **Tools** > **Android** > **AVD-Manager** auswählen. Wählen Sie in der Kategorie **Telefone** die Option **Nexus 5X** aus, und klicken Sie auf **Weiter**.

Weitere Informationen zum Einrichten eines AVD finden Sie in der [Dokumentation zu Android Studio](https://developer.android.com/studio/run/managing-avds).

![Android-Emulator](./media/how-to-use-android-map-control-library/android-emulator.png)

## <a name="install-the-azure-maps-android-sdk"></a>Installieren des Android SDK für Azure Maps

Als nächstes müssen Sie zur Erstellung Ihrer Anwendung das Android SDK für Azure Maps installieren. Führen Sie die folgenden Schritte aus, um das SDK zu installieren:

1. Öffnen Sie die Datei **build.gradle** auf der obersten Ebene, und fügen Sie dem Blockabschnitt **all projects**, **repositories** den folgenden Code hinzu:

    ```
    maven {
            url "https://atlas.microsoft.com/sdk/android"
    }
    ```

2. Aktualisieren Sie **app/build.gradle**, und fügen Sie den folgenden Code hinzu:
    
    1. Stellen Sie sicher, dass **minSdkVersion** Ihres Projekts mindestens auf API 21 festgelegt ist.

    2. Fügen Sie den folgenden Code dem Abschnitt „Android“ hinzu:

        ```
        compileOptions {
            sourceCompatibility JavaVersion.VERSION_1_8
            targetCompatibility JavaVersion.VERSION_1_8
        }
        ```
    3. Aktualisieren Sie Ihren Block „dependencies“, und fügen Sie eine neue Implementierungsabhängigkeitszeile für das neueste Android SDK von Azure Maps hinzu:

        ```
        implementation "com.microsoft.azure.maps:mapcontrol:0.2"
        ```

    > [!Note]
    > Das Android SDK für Azure Maps wird regelmäßig aktualisiert und verbessert. In der Dokumentation [Erste Schritte mit dem Android-Kartensteuerelement](https://docs.microsoft.com/azure/azure-maps/how-to-use-android-map-control-library) finden Sie die neueste Implementierungsversionsnummer von Azure Maps. Außerdem können Sie die Versionsnummer von „0.2“ auf „0+“" festlegen, damit sie immer auf die neueste Version zeigt.

3. Bearbeiten Sie die Datei **res** > **layout** > **activity_main.xml**, und ersetzen Sie sie durch folgenden Code:
    
    ```XML
    <?xml version="1.0" encoding="utf-8"?>
    <FrameLayout
        xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        >

        <com.microsoft.azure.maps.mapcontrol.MapControl
            android:id="@+id/mapcontrol"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            />
    </FrameLayout>
    ```

4. In der Datei **MainActivity.java** müssen Sie Folgendes ausführen:
    
    * Hinzufügen von Importen für das Azure Maps SDK
    * Festlegen Ihrer Azure Maps-Authentifizierungsinformationen
    * Abrufen der Kartensteuerelementinstanz in der **onCreate**-Methode

    Indem Sie die Authentifizierungsinformationen für die `AzureMaps`-Klasse global mit der Methode `setSubscriptionKey` oder `setAadProperties` festlegen, wird es möglich, dass Sie nicht in jeder Ansicht Ihre Authentifizierungsinformationen hinzufügen müssen. 

    Das Kartensteuerelement enthält eigene Lebenszyklusmethoden zur Verwaltung des OpenGL-Lebenszyklus von Android, die direkt aus der enthaltenen Activity aufgerufen werden müssen. Damit Ihre App ordnungsgemäß funktioniert, rufen Sie die Lebenszyklusmethoden des Kartensteuerelements auf. Sie müssen die folgenden Lebenszyklusmethoden in der Aktivität, die das Kartensteuerelement enthält, überschreiben und die entsprechende Kartensteuerelementmethode aufrufen. 

    * onCreate(Bundle) 
    * onStart() 
    * onResume() 
    * onPause() 
    * onStop() 
    * onDestroy() 
    * onSaveInstanceState(Bundle) 
    * onLowMemory() 

    Bearbeiten Sie die Datei **MainActivity.java** wie folgt:
    
    ```java
    package com.example.myapplication;

    import android.support.v7.app.AppCompatActivity;
    import android.os.Bundle;
    import com.microsoft.azure.maps.mapcontrol.AzureMaps;
    import com.microsoft.azure.maps.mapcontrol.MapControl;
    import com.microsoft.azure.maps.mapcontrol.layer.SymbolLayer;
    import com.microsoft.azure.maps.mapcontrol.options.MapStyle;
    import com.microsoft.azure.maps.mapcontrol.source.DataSource;

    public class MainActivity extends AppCompatActivity {
        
        static {
            AzureMaps.setSubscriptionKey("<Your Azure Maps subscription key>");
        }

        MapControl mapControl;

        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);

            mapControl = findViewById(R.id.mapcontrol);

            mapControl.onCreate(savedInstanceState);
    
            //Wait until the map resources are ready.
            mapControl.onReady(map -> {
                //Add your post map load code here.
    
            });
        }

        @Override
        public void onResume() {
            super.onResume();
            mapControl.onResume();
        }

        @Override
        protected void onStart(){
            super.onStart();
            mapControl.onStart();
        }

        @Override
        public void onPause() {
            super.onPause();
            mapControl.onPause();
        }

        @Override
        public void onStop() {
            super.onStop();
            mapControl.onStop();
        }

        @Override
        public void onLowMemory() {
            super.onLowMemory();
            mapControl.onLowMemory();
        }

        @Override
        protected void onDestroy() {
            super.onDestroy();
            mapControl.onDestroy();
        }

        @Override
        protected void onSaveInstanceState(Bundle outState) {
            super.onSaveInstanceState(outState);
            mapControl.onSaveInstanceState(outState);
        }
    }

    ```

## <a name="import-classes"></a>Importieren von Klassen

Nachdem Sie die vorherigen Schritte abgeschlossen haben, werden Ihnen wahrscheinlich zu einem Teil des Codes Warnungen von Android Studio angezeigt. Importieren Sie die Klassen, auf die in `MainActivity.java` verwiesen wird, um diese Warnungen aufzulösen.

Durch Drücken von Alt+Enter (Option+Return auf einem Mac) können Sie diese Klassen automatisch importieren.

Klicken Sie wie in der folgenden Grafik dargestellt auf die Schaltfläche „Ausführen“ (oder drücken Sie Strg+R auf einem Mac), um Ihre Anwendung zu erstellen.

![Klicken Sie auf „Run“ (Ausführen).](./media/how-to-use-android-map-control-library/run-app.png)

Es dauert ein paar Sekunden, bis Android Studio die Anwendung erstellt hat. Nach Abschluss des Builds können Sie Ihre Anwendung auf dem emulierten Android-Gerät testen. Es sollte eine Zuordnung wie diese angezeigt werden:

<center>

![Android-Karte](./media/how-to-use-android-map-control-library/android-map.png)</center>

## <a name="localizing-the-map"></a>Lokalisieren der Karte

Das Android SDK für Azure Maps bietet drei verschiedene Möglichkeiten zum Festlegen der Sprache und der regionalen Ansicht für die Karte. Der folgende Code zeigt, wie die Sprache auf Französisch („fr-FR“) und die regionale Ansicht auf „Auto“ festgelegt wird. 

Die erste Option besteht darin, die Informationen zur Sprache und regionalen Ansicht mithilfe der statischen Methoden `setLanguage` und `setView` global an die Klasse `AzureMaps` zu übergeben. Dadurch werden die Standardsprache und die regionale Ansicht für alle Azure Maps-Steuerelemente in Ihre App geladen.

```Java
static {
    //Set your Azure Maps Key.
    AzureMaps.setSubscriptionKey("<Your Azure Maps Key>");

    //Set the language to be used by Azure Maps.
    AzureMaps.setLanguage("fr-FR");

    //Set the regional view to be used by Azure Maps.
    AzureMaps.setView("auto");
}
```

Die zweite Option besteht darin, die Informationen zur Sprache und Ansicht an die Kartensteuerelement-XML zu übergeben.

```XML
<com.microsoft.azure.maps.mapcontrol.MapControl
    android:id="@+id/myMap"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    app:mapcontrol_language="fr-FR"
    app:mapcontrol_view="auto"
    />
```

Die dritte Option besteht darin, die Sprache und regionale Ansicht der Karte mithilfe der Kartenmethode `setStyle` programmgesteuert festzulegen. Dies kann jederzeit geschehen, um die Sprache und regionale Ansicht der Karte zu ändern.

```Java
mapControl.onReady(map -> {
    map.setStyle(StyleOptions.language("fr-FR"));
    map.setStyle(StyleOptions.view("auto"));
});
```

Hier ist ein Beispiel für Azure Maps, bei dem die Sprache auf „fr-FR“ und die regionale Ansicht auf „Auto“ festgelegt wurde.

<center>

![Kartenbild mit Beschriftungen in Französisch](./media/how-to-use-android-map-control-library/android-localization.png)
</center>

Eine vollständige Liste der unterstützten Sprachen und regionalen Ansichten ist [hier](supported-languages.md) dokumentiert.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie der Karte Overlaydaten hinzugefügt werden:

> [!div class="nextstepaction"]
> [Hinzufügen einer Symbolebene zu einer Android-Karte](https://review.docs.microsoft.com/azure/azure-maps/how-to-add-symbol-to-android-map)

> [!div class="nextstepaction"]
> [Hinzufügen von Formen zu einer Android-Karte](https://docs.microsoft.com/azure/azure-maps/how-to-add-shapes-to-android-map)

> [!div class="nextstepaction"]
> [Ändern von Kartenstilen auf Android-Karten](https://docs.microsoft.com/azure/azure-maps/set-android-map-styles)
