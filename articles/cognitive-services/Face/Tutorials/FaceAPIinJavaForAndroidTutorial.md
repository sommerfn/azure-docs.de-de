---
title: 'Tutorial: Erkennen und Umranden von Gesichtern in einem Bild mit dem Android SDK'
titleSuffix: Azure Cognitive Services
description: In diesem Tutorial erstellen Sie eine einfache Android-App, die mithilfe der Gesichtserkennungs-API Gesichter in einem Bild erkennt und umrandet.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: tutorial
ms.date: 07/03/2019
ms.author: pafarley
ms.openlocfilehash: 366c0c50cee521c5e70496403fd77211a875065f
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2019
ms.locfileid: "67606757"
---
# <a name="tutorial-create-an-android-app-to-detect-and-frame-faces-in-an-image"></a>Tutorial: Erstellen einer Android-App zum Erkennen und Umranden von Gesichtern in einem Bild

In diesem Tutorial erstellen Sie eine einfache Android-App, die mithilfe der Azure-Gesichtserkennungs-API über das Java SDK Gesichter in einem Bild erkennt und umrandet. Die Anwendung zeigt ein ausgewähltes Bild und zeichnet einen Rahmen um jedes erkannte Gesicht.

Dieses Tutorial veranschaulicht folgende Vorgehensweisen:

> [!div class="checklist"]
> - Erstellen einer Android-Anwendung
> - Installieren der Clientbibliothek für die Gesichtserkennungs-API
> - Verwenden der Clientbibliothek zum Erkennen von Gesichtern in einem Bild
> - Zeichnen eines Rahmens um jedes erkannte Gesicht

![Android-Screenshot: Foto mit Gesichtern, die mit einem roten Rechteck umrandet sind](../Images/android_getstarted2.1.PNG)

Der vollständige Beispielcode ist im Repository [Cognitive Services Face Android](https://github.com/Azure-Samples/cognitive-services-face-android-sample) (Cognitive Services-Beispiel für die Gesichtserkennung unter Android) auf GitHub verfügbar.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen. 

## <a name="prerequisites"></a>Voraussetzungen

- Ein Abonnementschlüssel für die Gesichtserkennungs-API. Über die Seite [Cognitive Services ausprobieren](https://azure.microsoft.com/try/cognitive-services/?api=face-api) können Sie einen Abonnementschlüssel für eine kostenlose Testversion abrufen. Gehen Sie andernfalls wie unter [Schnellstart: Erstellen eines Cognitive Services-Kontos im Azure-Portal](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) beschrieben vor, um den Gesichtserkennungs-API-Dienst zu abonnieren und Ihren Schlüssel zu erhalten.
- [Android Studio](https://developer.android.com/studio/) mit API-Ebene 22 oder höher (für die Clientbibliothek zur Gesichtserkennung erforderlich)

## <a name="create-the-android-studio-project"></a>Erstellen des Android Studio-Projekts

Führen Sie diese Schritte aus, um ein neues Android-Anwendungsprojekt zu erstellen.

1. Wählen Sie in Android Studio **Start a new Android Studio project** (Neues Android Studio-Projekt starten) aus.
1. Ändern Sie unter **Create Android Project** (Android-Projekt erstellen) die Standardfelder, falls dies erforderlich ist, und klicken Sie dann auf **Next** (Weiter).
1. Wählen Sie auf dem Bildschirm **Target Android Devices** (Android-Zielgeräte) im Dropdownmenü die Option **API 22** oder höher aus, und klicken Sie dann auf **Next** (Weiter).
1. Wählen Sie **Empty Activity** (Leere Aktivität), und klicken Sie auf **Next** (Weiter).
1. Deaktivieren Sie die Option **Backwards Compatibility** (Abwärtskompatibilität), und klicken Sie auf **Finish** (Fertig stellen).

## <a name="add-the-initial-code"></a>Hinzufügen des anfänglichen Codes

### <a name="create-the-ui"></a>Erstellen der Benutzeroberfläche

Öffnen Sie *activity_main.xml*. Wählen Sie im Layout-Editor die Registerkarte **Text**, und ersetzen Sie den Inhalt durch den folgenden Code:

[!code-xml[](~/cognitive-services-face-android-detect/FaceTutorial/app/src/main/res/layout/activity_main.xml?range=1-18)]

### <a name="create-the-main-class"></a>Erstellen der Hauptklasse

Öffnen Sie *MainActivity.java*, und ersetzen Sie die vorhandenen `import`-Anweisungen durch den folgenden Code:

[!code-java[](~/cognitive-services-face-android-detect/FaceTutorial/app/src/main/java/com/contoso/facetutorial/MainActivity.java?range=3-11)]

Ersetzen Sie anschließend den Inhalt der **MainActivity**-Klasse durch den folgenden Code. Dadurch wird ein Ereignishandler für **Button** erstellt, über den eine neue Aktivität gestartet wird, damit der Benutzer ein Bild auswählen kann. Das Bild wird in **ImageView** angezeigt.

[!code-java[](~/cognitive-services-face-android-detect/FaceTutorial/app/src/main/java/com/contoso/facetutorial/MainActivity.java?range=29-68)]

### <a name="try-the-app"></a>Testen der App

Kommentieren Sie den Aufruf von **detectAndFrame** in der Methode **onActivityResult** aus. Wählen Sie dann im Menü die Option **Ausführen**, um Ihre App zu testen. Klicken Sie nach dem Öffnen der App in einem Emulator oder einem verbundenen Gerät unten auf **Durchsuchen**. Das Dialogfeld zur Dateiauswahl sollte auf dem Gerät angezeigt werden. Wählen Sie ein Bild aus, und vergewissern Sie sich, dass es im Fenster angezeigt wird. Schließen Sie dann die App, und fahren Sie mit dem nächsten Schritt fort.

![Android-Screenshot: Foto mit Gesichtern](../Images/android_getstarted1.1.PNG)

## <a name="add-the-face-sdk"></a>Hinzufügen des Face SDK

### <a name="add-the-gradle-dependency"></a>Hinzufügen der Gradle-Abhängigkeit

Verwenden Sie im Bereich **Projekt** das Dropdownmenü, um **Android** auszuwählen. Erweitern Sie **Gradle Scripts** (Gradle-Skripts), und öffnen Sie anschließend *build.gradle (Module: app)* (build.gradle (Modul: App)). Fügen Sie eine Abhängigkeit für die Clientbibliothek für die Gesichtserkennung (`com.microsoft.projectoxford:face:1.4.3`) hinzu, wie im Screenshot unten gezeigt. Klicken Sie anschließend auf **Sync Now** (Jetzt synchronisieren).

![Android Studio-Screenshot: App-Datei „build.gradle“](../Images/face-tut-java-gradle.png)

### <a name="add-the-face-related-project-code"></a>Hinzufügen des Projektcodes für die Gesichtserkennung

Fügen Sie der Datei **MainActivity.java** die folgenden `import`-Anweisungen hinzu:

[!code-java[](~/cognitive-services-face-android-detect/FaceTutorial/app/src/main/java/com/contoso/facetutorial/MainActivity.java?range=13-14)]

Fügen Sie dann oberhalb der **onCreate**-Methode in der **MainActivity**-Klasse den folgenden Code ein:

[!code-java[](~/cognitive-services-face-android-detect/FaceTutorial/app/src/main/java/com/contoso/facetutorial/MainActivity.java?range=17-27)]

Sie müssen `<Subscription Key>` durch Ihren Abonnementschlüssel ersetzen. Ersetzen Sie außerdem `<API endpoint>` durch Ihren Gesichtserkennungs-API-Endpunkt. Verwenden Sie dabei die entsprechende Regions-ID für Ihren Schlüssel. (Eine Liste aller Regionsendpunkte finden Sie in den [Dokumenten zur Gesichtserkennungs-API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).) Abonnementschlüssel für kostenlose Testversionen werden in der Region **westus** generiert.

Erweitern Sie im Bereich **Project** (Projekt) die Option **app** (App) und dann **manifests** (Manifeste), und öffnen Sie *AndroidManifest.xml*. Fügen Sie das folgende Element als direkt untergeordnetes Element von `manifest` ein:

[!code-xml[](~/cognitive-services-face-android-detect/FaceTutorial/app/src/main/AndroidManifest.xml?range=5)]

## <a name="upload-image-and-detect-faces"></a>Hochladen eines Bilds und Erkennen von Gesichtern

Ihre App erkennt Gesichter durch Aufrufen der **faceClient.Face.DetectWithStreamAsync**-Methode, die die [Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)-REST-API umschließt und eine Liste von **Face**-Instanzen zurückgibt.

Die Position jedes zurückgegebenen **Face**-Elements wird mit einem Rechteck angegeben, und außerdem werden einige optionale Gesichtsmerkmale angezeigt. In diesem Beispiel werden nur die Rechtecke um Gesichter angefordert.

Fügen Sie die folgenden beiden Methoden in die **MainActivity**-Klasse ein. Beachten Sie, dass die App nach Abschluss der Gesichtserkennung die **drawFaceRectanglesOnBitmap**-Methode aufruft, um **ImageView** zu ändern. Diese Methode definieren Sie als Nächstes.

[!code-java[](~/cognitive-services-face-android-detect/FaceTutorial/app/src/main/java/com/contoso/facetutorial/MainActivity.java?range=70-150)]

## <a name="draw-face-rectangles"></a>Zeichnen von Rechtecken um Gesichter

Fügen Sie die folgende Hilfsmethode in die **MainActivity**-Klasse ein. Mit dieser Methode wird unter Verwendung der Rechteckkoordinaten der einzelnen **Face**-Instanzen ein Rechteck um jedes erkannte Gesicht gezeichnet.

[!code-java[](~/cognitive-services-face-android-detect/FaceTutorial/app/src/main/java/com/contoso/facetutorial/MainActivity.java?range=152-173)]

Heben Sie abschließend die Auskommentierung des Aufrufs der **detectAndFrame**-Methode in **onActivityResult** auf.

## <a name="run-the-app"></a>Ausführen der App

Führen Sie die Anwendung aus, und suchen Sie nach einem Bild mit einem Gesicht. Warten Sie einige Sekunden, damit der Gesichtserkennungsdienst reagieren kann. Um jedes Gesicht im Bild sollte ein rotes Rechteck angezeigt werden.

![Android-Screenshot mit Gesichtern, die mit einem roten Rechteck umrandet sind](../Images/android_getstarted2.1.PNG)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurde der grundlegende Prozess für die Verwendung des Java SDK der Gesichtserkennungs-API beschrieben und eine Anwendung zum Erkennen und Umranden von Gesichtern in einem Bild erstellt. Machen Sie sich als Nächstes mit den Details zur Gesichtserkennung vertraut.

> [!div class="nextstepaction"]
> [Gewusst wie: Gesichtserkennung in Bildern](../Face-API-How-to-Topics/HowtoDetectFacesinImage.md)
