---
title: 'Beispiel: Analysieren von Videos in Echtzeit – Gesichtserkennungs-API'
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie mithilfe der Gesichtserkennungs-API nahezu in Echtzeit Frames aus Livevideostreams analysieren.
services: cognitive-services
author: SteveMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: sample
ms.date: 03/01/2018
ms.author: sbowles
ms.openlocfilehash: e2166354fb45d24e117156e917f4da726ee8406f
ms.sourcegitcommit: 8e1fb03a9c3ad0fc3fd4d6c111598aa74e0b9bd4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2019
ms.locfileid: "70114343"
---
# <a name="example-how-to-analyze-videos-in-real-time"></a>Beispiel: Analysieren von Videos in Echtzeit

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

Wenn unsere Analyse aus einem einfachen clientseitigen Algorithmus besteht, ist dieser Ansatz geeignet. Erfolgt die Analyse jedoch in der Cloud, kann ein API-Aufruf aufgrund der Wartezeit mehrere Sekunden dauern. Während dieses Zeitraums werden keine Bilder erfasst, und unser Thread leistet praktisch nichts. Die maximale Bildfrequenz ist durch die Latenz der API-Aufrufe eingeschränkt.

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

Mit diesem Code wird jede Analyse in einer separaten Aufgabe gestartet, die im Hintergrund ausgeführt werden kann, während neue Frames herausgegriffen werden. Mit dieser Methode wird verhindert, dass der Hauptthread während des Wartens auf die Rückgabe eines API-Aufrufs blockiert wird. Dabei gehen allerdings einige Garantien verloren, die für die einfache Version gelten. Möglicherweise werden mehrere API-Aufrufe parallel durchgeführt, und die Ergebnisse werden u.U. in der falschen Reihenfolge zurückgegeben. Dies kann auch dazu führen, dass mehrere Threads gleichzeitig auf die Funktion „ConsumeResult()“ zugreifen. Falls die Funktion nicht threadsicher ist, kann dies gefährlich sein. Zudem werden mit dem einfachen Code nicht die Aufgaben nachverfolgt, die erstellt werden, sodass Ausnahmen lautlos untergehen. Im letzten Schritt müssen wir daher einen Thread vom Typ „Consumer“ hinzufügen, mit dem die Analyseaufgaben nachverfolgt, Ausnahmen ausgelöst und Aufgaben mit langer Ausführungsdauer beendet werden und zudem sichergestellt wird, dass die Ergebnisse in der richtigen Reihenfolge verarbeitet werden.

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

Außerdem verfügen wir über einen Consumer-Thread, mit dem Aufgaben aus der Warteschlange entfernt werden, auf den Abschluss der Verarbeitung gewartet wird, und entweder das Ergebnis angezeigt oder die jeweilige Ausnahme ausgelöst wird. Durch die Verwendung der Warteschlange können wir sicherstellen, dass Ergebnisse einzeln nacheinander in der richtigen Reihenfolge genutzt werden, ohne dass die maximale Bildfrequenz des Systems eingeschränkt wird.

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

Um Ihre App so schnell wie möglich in Betrieb zu nehmen, verwenden Sie eine flexible Implementierung des oben beschriebenen Systems. Den Code finden Sie unter [https://github.com/Microsoft/Cognitive-Samples-VideoFrameAnalysis](https://github.com/Microsoft/Cognitive-Samples-VideoFrameAnalysis).

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
            private readonly IFaceClient faceClient = new FaceClient(
            new ApiKeyServiceClientCredentials("<subscription key>"),
            new System.Net.Http.DelegatingHandler[] { });

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

In den meisten Modi kommt es zu einer sichtbaren Verzögerung zwischen den Live-Videodaten auf der linken Seite und der visualisierten Analyse auf der rechten Seite. Diese Verzögerung ist die Zeit, die benötigt wird, um den API-Aufruf durchzuführen. Eine Ausnahme ist der Modus „EmotionsWithClientFaceDetect“. Hierbei wird die Gesichtserkennung lokal auf dem Clientcomputer per OpenCV durchgeführt, bevor Bilder an Cognitive Services übermittelt werden. Auf diese Weise können wir das erkannte Gesicht sofort visualisieren und die Emotionen dann aktualisieren, nachdem der API-Aufruf zurückgegeben wurde. Dies ist ein Beispiel für einen Hybrid-Ansatz, bei dem auf dem Client einfache Verarbeitungen durchgeführt werden können. Bei Bedarf können Cognitive Services-APIs dies durch eingehendere Analysen untermauern.

![HowToAnalyzeVideo](../../Video/Images/FramebyFrame.jpg)

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
        

Wenn Sie zum Integrieren bereit sind, können Sie **in Ihren eigenen Projekten auf die VideoFrameAnalyzer-Bibliothek verweisen**. 

## <a name="summary"></a>Zusammenfassung

In diesem Leitfaden wurde beschrieben, wie Sie nahezu in Echtzeit eine Analyse für Live-Videodatenströme durchführen, indem Sie die APIs für Gesichtserkennung, Maschinelles Sehen und Emotionen verwenden, und wie Sie erste Schritte mit unserem Beispielcode ausführen. Auf der [Registrierungsseite für Azure Cognitive Services](https://azure.microsoft.com/try/cognitive-services/) können Sie mit dem Erstellen Ihrer App beginnen, indem Sie kostenlose API-Schlüssel verwenden. 

Wir freuen uns über Feedback und Vorschläge im [GitHub-Repository](https://github.com/Microsoft/Cognitive-Samples-VideoFrameAnalysis/) und über allgemeineres Feedback zu den APIs über unsere  [UserVoice-Website](https://cognitive.uservoice.com/).

## <a name="related-topics"></a>Verwandte Themen
- [Gesichtsidentifikation in Bildern](HowtoIdentifyFacesinImage.md)
- [How to Detect Faces in Image](HowtoDetectFacesinImage.md) (Gesichtserkennung in Bildern)
