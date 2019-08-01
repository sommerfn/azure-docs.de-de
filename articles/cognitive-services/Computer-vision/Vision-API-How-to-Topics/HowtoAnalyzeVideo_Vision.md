---
title: 'Beispiel: Analysieren von Videos in Echtzeit – Maschinelles Sehen-API'
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie nahezu in Echtzeit Analysen für Frames aus einem Livevideostream durchführen, indem Sie die Maschinelles Sehen-API verwenden.
services: cognitive-services
author: KellyDF
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: sample
ms.date: 03/21/2019
ms.author: kefre
ms.custom: seodec18
ms.openlocfilehash: 3432ea20f9fb59524940258e13c46ee6f4c4e890
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2019
ms.locfileid: "68565694"
---
# <a name="how-to-analyze-videos-in-real-time"></a>Analysieren von Videos in Echtzeit

In diesem Leitfaden wird veranschaulicht, wie Sie nahezu in Echtzeit Analysen für Frames durchführen, die aus einem Live-Videodatenstrom stammen. Die grundlegenden Komponenten eines Systems dieser Art sind:

- Beschaffen von Frames von einer Videoquelle
- Auswählen der zu analysierenden Frames
- Übermitteln der Frames an die API
- Nutzen der einzelnen Analyseergebnisse, die für den API-Aufruf zurückgegeben werden

Diese Beispiele wurden in C# geschrieben, und den Code finden Sie auf GitHub: [https://github.com/Microsoft/Cognitive-Samples-VideoFrameAnalysis](https://github.com/Microsoft/Cognitive-Samples-VideoFrameAnalysis/).

## <a name="the-approach"></a>Ansatz

Es gibt mehrere Möglichkeiten, das Problem zur Durchführung der Analyse von Videodatenströmen nahezu in Echtzeit zu lösen. Zunächst werden drei Ansätze beschrieben, die zunehmend anspruchsvoller werden.

### <a name="a-simple-approach"></a>Ein einfacher Ansatz

Der einfachste Entwurf für ein System, mit dem Analysen nahezu in Echtzeit durchgeführt werden können, ist eine unendliche Schleife, bei der in jeder Iteration ein Frame herausgegriffen und analysiert und anschließend das Ergebnis genutzt wird:

```csharp
while (true)
{
    Frame f = GrabFrame();
    if (ShouldAnalyze(f))
    {
        AnalysisResult r = await Analyze(f);
        ConsumeResult(r);
    }
}
```

Wenn unsere Analyse aus einem einfachen clientseitigen Algorithmus besteht, ist dieser Ansatz geeignet. Wenn Ihre Analyse aber in der Cloud erfolgt, kann ein API-Aufruf aufgrund der Latenz mehrere Sekunden dauern. Während dieses Zeitraums werden keine Bilder erfasst, und unser Thread leistet praktisch nichts. Die maximale Bildfrequenz ist durch die Latenz der API-Aufrufe eingeschränkt.

### <a name="parallelizing-api-calls"></a>Parallelisieren von API-Aufrufen

Eine einfache Single-Thread-Schleife ist für einen simplen clientseitigen Algorithmus sinnvoll, aber dieser Ansatz passt nicht gut zur Latenz von API-Aufrufen in der Cloud. Die Lösung dieses Problems besteht darin zuzulassen, dass API-Aufrufe mit langer Ausführungsdauer parallel zum „Frame Grabbing“ durchgeführt werden können. In C# erreichen wir dies beispielsweise mithilfe der aufgabenbasierten Parallelität:

```csharp
while (true)
{
    Frame f = GrabFrame();
    if (ShouldAnalyze(f))
    {
        var t = Task.Run(async () => 
        {
            AnalysisResult r = await Analyze(f);
            ConsumeResult(r);
        }
    }
}
```

Bei dieser Verfahrensweise wird jede Analyse in einem separaten Task gestartet, der im Hintergrund ausgeführt werden kann, während neue Frames herausgegriffen werden. So wird verhindert, dass der Hauptthread während des Wartens auf die Rückgabe eines API-Aufrufs blockiert wird. Dabei gehen allerdings einige Garantien verloren, die für die einfache Version gelten: möglicherweise werden mehrere API-Aufrufe parallel durchgeführt, und die Ergebnisse werden unter Umständen in der falschen Reihenfolge zurückgegeben. Dies kann auch dazu führen, dass mehrere Threads gleichzeitig auf die Funktion „ConsumeResult()“ zugreifen. Falls die Funktion nicht threadsicher ist, kann dies gefährlich sein. Zudem werden mit dem einfachen Code nicht die Aufgaben nachverfolgt, die erstellt werden, sodass Ausnahmen lautlos untergehen. Als letzten Bestandteil müssen wir daher einen Thread vom Typ „Consumer“ hinzufügen, mit dem die Analyseaufgaben nachverfolgt, Ausnahmen ausgelöst und Aufgaben mit langer Ausführungsdauer beendet werden und zudem sichergestellt wird, dass die Ergebnisse einzeln nacheinander in der richtigen Reihenfolge „konsumiert“ werden.

### <a name="a-producer-consumer-design"></a>Producer-Consumer-Entwurf

Beim letzten System, dem Producer-Consumer-System, verwenden wir einen Producer-Thread, der dem vorherigen Ansatz mit der unendlichen Schleife ähnelt. Aber anstatt Analyseergebnisse unmittelbar nach ihrer Verfügbarkeit zu nutzen, fügt der Producer die Aufgaben einfach in eine Warteschlange ein, um sie nachverfolgen zu können.

```csharp
// Queue that will contain the API call tasks. 
var taskQueue = new BlockingCollection<Task<ResultWrapper>>();
     
// Producer thread. 
while (true)
{
    // Grab a frame. 
    Frame f = GrabFrame();
 
    // Decide whether to analyze the frame. 
    if (ShouldAnalyze(f))
    {
        // Start a task that will run in parallel with this thread. 
        var analysisTask = Task.Run(async () => 
        {
            // Put the frame, and the result/exception into a wrapper object.
            var output = new ResultWrapper(f);
            try
            {
                output.Analysis = await Analyze(f);
            }
            catch (Exception e)
            {
                output.Exception = e;
            }
            return output;
        }
        
        // Push the task onto the queue. 
        taskQueue.Add(analysisTask);
    }
}
```

Außerdem verfügen wir über einen Consumer-Thread, mit dem Aufgaben aus der Warteschlange entfernt werden, auf den Abschluss der Verarbeitung gewartet wird und entweder das Ergebnis angezeigt oder die jeweilige Ausnahme ausgelöst wird. Durch die Verwendung der Warteschlange können wir sicherstellen, dass Ergebnisse einzeln nacheinander in der richtigen Reihenfolge genutzt werden, ohne dass die maximale Bildfrequenz des Systems eingeschränkt wird.

```csharp
// Consumer thread. 
while (true)
{
    // Get the oldest task. 
    Task<ResultWrapper> analysisTask = taskQueue.Take();
 
    // Await until the task is completed. 
    var output = await analysisTask;
     
    // Consume the exception or result. 
    if (output.Exception != null)
    {
        throw output.Exception;
    }
    else
    {
        ConsumeResult(output.Analysis);
    }
}
```

## <a name="implementing-the-solution"></a>Implementieren der Lösung

### <a name="getting-started"></a>Erste Schritte

Damit Ihre App so schnell wie möglich einsatzbereit ist, haben wir das oben beschriebene System implementiert. Der Ansatz soll flexibel genug sein, um viele Szenarien implementieren zu können, und gleichzeitig soll das System leicht verwendbar sein. Den Code finden Sie unter [https://github.com/Microsoft/Cognitive-Samples-VideoFrameAnalysis](https://github.com/Microsoft/Cognitive-Samples-VideoFrameAnalysis).

Die Bibliothek enthält die FrameGrabber-Klasse, mit der das oben beschriebene Producer-Consumer-System implementiert wird, um Videoframes einer Webcam zu verarbeiten. Der Benutzer kann die genaue Form des API-Aufrufs angeben, und für die Klasse werden Ereignisse verwendet, um den aufrufenden Code darüber zu informieren, dass ein neuer Frame beschafft wurde oder ein neues Analyseergebnis verfügbar ist.

Zur Veranschaulichung einiger Möglichkeiten sind zwei Beispiel-Apps vorhanden, die die Bibliothek nutzen. Die erste App ist eine einfache Konsolen-App. Eine vereinfachte Version dieser App ist unten dargestellt. Hiermit werden Frames von der Standard-Webcam erfasst und zur Gesichtserkennung an die Gesichtserkennungs-API übermittelt.

```csharp
using System;
using VideoFrameAnalyzer;
using Microsoft.ProjectOxford.Face;
using Microsoft.ProjectOxford.Face.Contract;
     
namespace VideoFrameConsoleApplication
{
    class Program
    {
        static void Main(string[] args)
        {
            // Create grabber, with analysis type Face[]. 
            FrameGrabber<Face[]> grabber = new FrameGrabber<Face[]>();
            
            // Create Face API Client. Insert your Face API key here.
            FaceServiceClient faceClient = new FaceServiceClient("<subscription key>");

            // Set up our Face API call.
            grabber.AnalysisFunction = async frame => return await faceClient.DetectAsync(frame.Image.ToMemoryStream(".jpg"));

            // Set up a listener for when we receive a new result from an API call. 
            grabber.NewResultAvailable += (s, e) =>
            {
                if (e.Analysis != null)
                    Console.WriteLine("New result received for frame acquired at {0}. {1} faces detected", e.Frame.Metadata.Timestamp, e.Analysis.Length);
            };
            
            // Tell grabber to call the Face API every 3 seconds.
            grabber.TriggerAnalysisOnInterval(TimeSpan.FromMilliseconds(3000));

            // Start running.
            grabber.StartProcessingCameraAsync().Wait();

            // Wait for keypress to stop
            Console.WriteLine("Press any key to stop...");
            Console.ReadKey();
            
            // Stop, blocking until done.
            grabber.StopProcessingAsync().Wait();
        }
    }
}
```

Die zweite Beispiel-App ist etwas interessanter, und Sie können wählen, welche API für das Aufrufen der Videoframes verwendet werden soll. Auf der linken Seite wird in der App eine Vorschau der Livevideodaten angezeigt, und auf der rechten Seite wird das letzte API-Ergebnis auf dem entsprechenden Frame eingeblendet.

In den meisten Modi kommt es zu einer sichtbaren Verzögerung zwischen den Live-Videodaten auf der linken Seite und der visualisierten Analyse auf der rechten Seite. Diese Verzögerung ist die Zeit, die benötigt wird, um den API-Aufruf durchzuführen. Eine Ausnahme ist der Modus „EmotionsWithClientFaceDetect“. Hierbei wird die Gesichtserkennung lokal auf dem Clientcomputer per OpenCV durchgeführt, bevor Bilder an Cognitive Services übermittelt werden. Dank dieser Vorgehensweise können wir das erkannte Gesicht sofort visualisieren und die Emotionen dann später aktualisieren, nachdem der API-Aufruf zurückgegeben wurde. Dies veranschaulicht die Möglichkeit eines Hybrid-Ansatzes, bei dem auf dem Client eine einfache Verarbeitung durchgeführt werden kann. Anschließend können Cognitive Services-APIs genutzt werden, um dies bei Bedarf durch eingehendere Analysen zu untermauern.

![Screenshot der LiveCameraSample-App mit einem Bild mit angezeigten Tags](../../Video/Images/FramebyFrame.jpg)

### <a name="integrating-into-your-codebase"></a>Integration in Ihre Codebasis

Führen Sie diese Schritte aus, um mit diesem Beispiel zu beginnen:

1. Rufen Sie die API-Schlüssel für die Bildanalyse-APIs unter [Abonnements](https://azure.microsoft.com/try/cognitive-services/) ab. Für die Analyse von Videoframes gelten die folgenden APIs:
    - [Maschinelles Sehen-API](https://docs.microsoft.com/azure/cognitive-services/computer-vision/home)
    - [Emotionen-API](https://docs.microsoft.com/azure/cognitive-services/emotion/home)
    - [Gesichtserkennungs-API](https://docs.microsoft.com/azure/cognitive-services/face/overview)
2. Klonen des GitHub-Repositorys [Cognitive-Samples-VideoFrameAnalysis](https://github.com/Microsoft/Cognitive-Samples-VideoFrameAnalysis/)

3. Öffnen Sie das Beispiel in Visual Studio 2015, erstellen Sie die Beispielanwendungen, und führen Sie sie aus:
    - Für BasicConsoleSample ist der Schlüssel für die Gesichtserkennungs-API direkt in  [BasicConsoleSample/Program.cs](https://github.com/Microsoft/Cognitive-Samples-VideoFrameAnalysis/blob/master/Windows/BasicConsoleSample/Program.cs) hartcodiert.
    - Für LiveCameraSample sollten die Schlüssel im Bereich „Einstellungen“ der App eingegeben werden. Sie werden sitzungsübergreifend als Benutzerdaten beibehalten.

Wenn Sie zum Integrieren bereit sind, können Sie **einfach aus Ihren eigenen Projekten auf die VideoFrameAnalyzer-Bibliothek verweisen**.

Die Bild-, Sprach-, Video- und Textanalysefunktionen von VideoFrameAnalyzer nutzen Azure Cognitive Services. Microsoft erhält die Bilder, Audiodaten, Videodaten und anderen Daten, die Sie (über diese App) hochladen, und nutzt sie ggf. zur Verbesserung von Diensten. Wir bitten Sie darum, uns beim Schützen der Personen zu helfen, deren Daten über Ihre App an Azure Cognitive Services gesendet werden.

## <a name="summary"></a>Zusammenfassung

In diesem Leitfaden wurde beschrieben, wie Sie nahezu in Echtzeit eine Analyse für Live-Videodatenströme durchführen, indem Sie die APIs für Gesichtserkennung, Maschinelles Sehen und Emotionen verwenden, und wie Sie mit unserem Beispielcode starten können. Auf der [Registrierungsseite für Azure Cognitive Services](https://azure.microsoft.com/try/cognitive-services/) können Sie mit dem Erstellen Ihrer App beginnen, indem Sie kostenlose API-Schlüssel verwenden. 

Wir freuen uns über Feedback und Vorschläge im [GitHub-Repository](https://github.com/Microsoft/Cognitive-Samples-VideoFrameAnalysis/) und über allgemeineres Feedback zu den APIs über unsere  [UserVoice-Website](https://cognitive.uservoice.com/).

