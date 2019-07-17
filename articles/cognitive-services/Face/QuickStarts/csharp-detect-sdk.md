---
title: 'Schnellstart: Erkennen von Gesichtern in einem Bild mit dem Azure Face .NET SDK'
titleSuffix: Azure Cognitive Services
description: In dieser Schnellstartanleitung verwenden Sie das Azure Face SDK mit C#, um Gesichter in einem Bild zu erkennen.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: quickstart
ms.date: 07/03/2019
ms.author: pafarley
ms.openlocfilehash: bf4d11a18932d20e7dcc7580ebe5aa4e060c5a88
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2019
ms.locfileid: "67606779"
---
# <a name="quickstart-detect-faces-in-an-image-using-the-face-net-sdk"></a>Schnellstart: Erkennen von Gesichtern in einem Bild mit dem Azure Face .NET SDK

In dieser Schnellstartanleitung verwenden Sie das Face Service SDK mit C#, um menschliche Gesichter in einem Bild zu erkennen. Ein praktisches Beispiel des Codes aus dieser Schnellstartanleitung finden Sie im Gesichtserkennungsprojekt im Repository [Cognitive Services Vision csharp quickstarts](https://github.com/Azure-Samples/cognitive-services-vision-csharp-sdk-quickstarts/tree/master/Face) (Csharp-Schnellstartanleitungen für maschinelles Sehen mit Cognitive Services) auf GitHub.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen. 

## <a name="prerequisites"></a>Voraussetzungen

- Ein Abonnementschlüssel für die Gesichtserkennungs-API. Über die Seite [Cognitive Services ausprobieren](https://azure.microsoft.com/try/cognitive-services/?api=face-api) können Sie einen Abonnementschlüssel für eine kostenlose Testversion abrufen. Gehen Sie andernfalls wie unter [Schnellstart: Erstellen eines Cognitive Services-Kontos im Azure-Portal](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) beschrieben vor, um den Gesichtserkennungs-API-Dienst zu abonnieren und Ihren Schlüssel zu erhalten.
- Eine beliebige Edition von [Visual Studio 2015 oder 2017](https://www.visualstudio.com/downloads/).

## <a name="create-the-visual-studio-project"></a>Erstellen des Visual Studio-Projekts

1. Erstellen Sie in Visual Studio ein neues Projekt vom Typ **Konsolen-App (.NET Framework)** , und nennen Sie es **FaceDetection**. 
1. Sollte Ihre Projektmappe noch andere Projekte enthalten, wählen Sie dieses Projekt als das alleinige Startprojekt aus.
1. Rufen Sie die erforderlichen NuGet-Pakete ab. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf Ihr Projekt, und wählen Sie **NuGet-Pakete verwalten...** aus. Klicken Sie auf die Registerkarte **Durchsuchen**, und wählen Sie **Vorabversion einbeziehen** aus. Suchen Sie anschließend das folgende Paket, und installieren Sie es:
    - [Microsoft.Azure.CognitiveServices.Vision.Face 2.2.0-preview](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.Face/2.2.0-preview)
1. Vergewissern Sie sich, dass Sie jeweils die neueste Version aller NuGet-Pakete für Ihr Projekt installiert haben. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf Ihr Projekt, und wählen Sie **NuGet-Pakete verwalten...** aus. Klicken Sie auf die Registerkarte **Updates**, und installieren Sie jeweils die neueste Version aller angezeigten Pakete.

## <a name="add-face-detection-code"></a>Hinzufügen von Code für die Gesichtserkennung

Öffnen Sie die Datei *Program.cs* des neuen Projekts. Hier fügen Sie den Code hinzu, der zum Laden von Bildern und Erkennen von Gesichtern erforderlich ist.

### <a name="include-namespaces"></a>Einschließen von Namespaces

Fügen Sie am Anfang der Datei *Program.cs* die folgenden `using`-Anweisungen hinzu:

[!code-csharp[](~/cognitive-services-vision-csharp-sdk-quickstarts/Face/Program.cs?range=1-7)]

### <a name="add-essential-fields"></a>Hinzufügen wichtiger Felder

Fügen Sie die **Program**-Klasse mit folgenden Feldern hinzu. Mit diesen Daten wird angegeben, wie eine Verbindung mit dem Gesichtserkennungsdienst hergestellt wird und wo die Eingabedaten abgerufen werden. Sie müssen das Feld `subscriptionKey` mit dem Wert Ihres Abonnementschlüssels aktualisieren. Unter Umständen müssen Sie die Zeichenfolge `faceEndpoint` ändern, sodass sie die korrekte Regions-ID enthält. Darüber hinaus müssen Sie den Wert für `localImagePath` und/oder `remoteImageUrl` auf einen Pfad festlegen, der auf die tatsächlichen Bilddateien verweist.

Das Feld `faceAttributes` ist einfach ein Array bestimmter Attributtypen. In diesem Feld wird angegeben, welche Informationen zu den erkannten Gesichtern abgerufen werden sollen.

[!code-csharp[](~/cognitive-services-vision-csharp-sdk-quickstarts/Face/Program.cs?range=9-34)]

### <a name="create-and-use-the-face-client"></a>Erstellen und Verwenden des Clients für die Gesichtserkennung

Fügen Sie als Nächstes die **Main**-Methode der **Program**-Klasse mit folgendem Code hinzu. Dadurch wird ein Gesichtserkennungs-API-Client festgelegt.

[!code-csharp[](~/cognitive-services-vision-csharp-sdk-quickstarts/Face/Program.cs?range=36-41)]

Fügen Sie ebenfalls in der **Main**-Methode den folgenden Code hinzu, um den neu erstellten Gesichtserkennungsclient zum Erkennen von Gesichtern in einem Remotebild und in einem lokalen Bild zu verwenden. Als Nächstes werden die Erkennungsmethoden definiert. 

[!code-csharp[](~/cognitive-services-vision-csharp-sdk-quickstarts/Face/Program.cs?range=43-50)]

### <a name="detect-faces"></a>Erkennen von Gesichtern

Fügen Sie der **Program**-Klasse die folgende Methode hinzu. Sie nutzt den Client für den Gesichtserkennungsdienst zum Erkennen von Gesichtern in einem Remotebild, auf das mit einer URL verwiesen wird. Sie nutzt das Feld `faceAttributes`. Die Objekte vom Typ **DetectedFace**, die `faceList` hinzugefügt wurden, haben die angegebenen Attribute (in diesem Fall Alter und Geschlecht).

[!code-csharp[](~/cognitive-services-vision-csharp-sdk-quickstarts/Face/Program.cs?range=52-74)]

Fügen Sie analog dazu die **DetectLocalAsync**-Methode hinzu. Sie nutzt den Client für den Gesichtserkennungsdienst zum Erkennen von Gesichtern in einem lokalen Bild, auf das mit einem Dateipfad verwiesen wird.

[!code-csharp[](~/cognitive-services-vision-csharp-sdk-quickstarts/Face/Program.cs?range=76-101)]

### <a name="retrieve-and-display-face-attributes"></a>Abrufen und Anzeigen von Gesichtsattributen

Definieren Sie als Nächstes die **GetFaceAttributes**-Methode. Sie gibt eine Zeichenfolge mit den relevanten Attributinformationen zurück.

[!code-csharp[](~/cognitive-services-vision-csharp-sdk-quickstarts/Face/Program.cs?range=103-116)]

Definieren Sie abschließend die **DisplayAttributes**-Methode, um Gesichtsattributdaten in die Konsolenausgabe zu schreiben. Anschließend können Sie die Klasse und den Namespace schließen.

[!code-csharp[](~/cognitive-services-vision-csharp-sdk-quickstarts/Face/Program.cs?range=118-125)]

## <a name="run-the-app"></a>Ausführen der App

Eine erfolgreiche Antwort zeigt das Geschlecht und Alter für jedes Gesicht im Bild an. Beispiel:

```
https://upload.wikimedia.org/wikipedia/commons/3/37/Dagestani_man_and_woman.jpg
Male 37   Female 56
```

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie eine einfache .NET-Konsolenanwendung erstellt, die mithilfe des Gesichtserkennungs-API-Diensts Gesichter in lokalen Bildern und Remotebildern erkennen kann. Im nächsten Tutorial erfahren Sie im Detail, wie Sie Gesichtsinformationen auf intuitive Weise für Benutzer darstellen können.

> [!div class="nextstepaction"]
> [Tutorial: Erstellen einer WPF-App zum Erkennen und Umranden von Gesichtern in einem Bild unter Verwendung des .NET SDK](../Tutorials/FaceAPIinCSharpTutorial.md)
