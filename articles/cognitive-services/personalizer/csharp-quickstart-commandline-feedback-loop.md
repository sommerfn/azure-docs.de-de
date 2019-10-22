---
title: 'Schnellstart: Personalisierungsclientbibliothek für .NET | Microsoft-Dokumentation'
titleSuffix: Azure Cognitive Services
description: Erste Schritte mit der Personalisierungsclientbibliothek für .NET unter Verwendung einer Lernschleife.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: quickstart
ms.date: 09/26/2019
ms.author: diberry
ms.openlocfilehash: 4308ed6d00bd3900986f08a93a686f0d7d00bcfb
ms.sourcegitcommit: 12de9c927bc63868168056c39ccaa16d44cdc646
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "72515598"
---
# <a name="quickstart-personalizer-client-library-for-net"></a>Schnellstart: Personalisierungsclientbibliothek für .NET

Zeigen Sie personalisierten Inhalt in dieser C#-Schnellstartanleitung mit dem Personalisierungsdienst an.

Erste Schritte mit der Personalisierungsclientbibliothek für .NET. Führen Sie die nachfolgenden Schritte zum Installieren des Pakets aus, und testen Sie den Beispielcode für grundlegende Aufgaben.

 * Erstellen Sie eine Bewertungsliste der Aktionen zur Personalisierung.
 * Melden Sie die Relevanzbewertung zur Angabe des Erfolgs der am besten bewerteten Aktion.

[Referenzdokumentation](https://docs.microsoft.com/dotnet/api/Microsoft.Azure.CognitiveServices.Personalizer?view=azure-dotnet-preview) | [Quellcode der Bibliothek](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Personalizer) | [Paket (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Personalizer/) | [Beispiele](https://github.com/Azure-Samples/cognitive-services-personalizer-samples)

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement – [Erstellen eines kostenlosen Kontos](https://azure.microsoft.com/free/)
* Aktuelle Version von [.NET Core](https://dotnet.microsoft.com/download/dotnet-core).

## <a name="using-this-quickstart"></a>Verwenden dieser Schnellstartanleitung

Diese Schnellstartanleitung umfasst mehrere Schritte:

* Erstellen Sie im Azure-Portal eine Personalisierungsressource.
* Ändern Sie im Azure-Portal auf der Seite **Einstellungen** für die Personalisierungsressource die Häufigkeit der Modellaktualisierung.
* Erstellen Sie in einem Code-Editor eine Codedatei, und bearbeiten Sie sie.
* Installieren Sie in der Befehlszeile oder im Terminal das SDK über die Befehlszeile.
* Führen Sie die Codedatei in der Befehlszeile oder im Terminal aus.

## <a name="create-a-personalizer-azure-resource"></a>Erstellen einer Azure-Ressource für die Personalisierung

Azure Cognitive Services werden von Azure-Ressourcen dargestellt, die Sie abonnieren. Erstellen Sie auf Ihrem lokalen Computer eine Ressource für die Personalisierung (entweder über das [Azure-Portal](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) oder mithilfe der [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account-cli)). Weitere Funktionen:

* Rufen Sie einen [Testschlüssel](https://azure.microsoft.com/try/cognitive-services) ab, mit dem Sie sieben Tage lang kostenlos testen können. Nach der Registrierung steht dieser auf der [Azure-Website](https://azure.microsoft.com/try/cognitive-services/my-apis/) zur Verfügung.  
* Zeigen Sie Ihre Ressource im [Azure-Portal](https://portal.azure.com/) an.

<!-- rename TBD_KEY to something meaningful for your service, like TEXT_ANALYTICS_KEY -->
Nachdem Sie einen Schlüssel für Ihr Testabonnement bzw. Ihre Ressource erhalten haben, erstellen Sie zwei [Umgebungsvariablen](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication):

* `PERSONALIZER_RESOURCE_KEY` für den Ressourcenschlüssel.
* `PERSONALIZER_RESOURCE_ENDPOINT` für den Ressourcenendpunkt.

Die Schlüssel- und Endpunktwerte finden Sie im Azure-Portal auf der Seite **Schnellstart**.

## <a name="change-the-model-update-frequency"></a>Ändern der Häufigkeit der Modellaktualisierung

Ändern Sie im Azure-Portal auf der Seite **Einstellungen** in der Personalisierungsressource die **Häufigkeit der Modellaktualisierung** in 10 Sekunden. Dadurch wird der Dienst schnell trainiert, und Sie können sehen, wie sich die oberste Aktion für jede Iteration ändert.

![Ändern der Häufigkeit der Modellaktualisierung](./media/settings/configure-model-update-frequency-settings.png)

Bei der Erstinstanziierung einer Personalisierungsschleife ist kein Modell vorhanden, da noch keine Relevanz-API-Aufrufe zum Trainieren ausgeführt wurden. Rangfolgeaufrufe geben gleiche Wahrscheinlichkeiten für jedes Element zurück. Ihre Anwendung sollte dem Inhalt dennoch immer anhand der Ausgabe von RewardActionId einen Rang zuweisen.

## <a name="create-a-new-c-application"></a>Erstellen einer neuen C#-Anwendung

Erstellen Sie eine neue .NET Core-Anwendung in Ihrem bevorzugten Editor oder Ihrer bevorzugten IDE. 

Verwenden Sie in einem Konsolenfenster (z. B. cmd, PowerShell oder Bash) den Befehl dotnet `new` zum Erstellen einer neuen Konsolen-App mit dem Namen `personalizer-quickstart`. Dieser Befehl erstellt ein einfaches „Hallo Welt“-C#-Projekt mit einer einzigen Quelldatei: `Program.cs`. 

```console
dotnet new console -n personalizer-quickstart
```

Wechseln Sie zum Ordner der neu erstellten App. Sie können die Anwendung mit folgendem Befehl erstellen:

```console
dotnet build
```

Die Buildausgabe sollte keine Warnungen oder Fehler enthalten. 

```console
...
Build succeeded.
 0 Warning(s)
 0 Error(s)
...
```

## <a name="install-the-sdk"></a>Installieren des SDKs

Installieren Sie im Anwendungsverzeichnis mit dem folgenden Befehl die Personalisierungsclientbibliothek für .NET:

```console
dotnet add package Microsoft.Azure.CognitiveServices.Personalizer --version 0.8.0
```

Bei Verwendung der Visual Studio-IDE ist die Clientbibliothek als herunterladbares NuGet-Paket verfügbar.

## <a name="object-model"></a>Objektmodell

Der Personalisierungsclient ist ein Objekt vom Typ [PersonalizerClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.personalizer.personalizerclient?view=azure-dotnet), das mithilfe der Klasse „Microsoft.Rest.ServiceClientCredentials“, die Ihren Schlüssel enthält, bei Azure authentifiziert wird.

Erstellen Sie zum Anfordern eines Inhaltsrangs eine Rangfolgeanforderung ([RankRequest](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.personalizer.models.rankrequest?view=azure-dotnet-preview)), und übergeben Sie sie an die Methode [client.Rank](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.personalizer.personalizerclientextensions.rank?view=azure-dotnet-preview). Die Methode „Rank“ gibt eine Rangfolgeantwort (RankResponse) mit dem bewerteten Inhalt zurück. 

Um eine Relevanz an die Personalisierung zu senden, erstellen Sie eine Relevanzanforderung ([RewardRequest](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.personalizer.models.rewardrequest?view=azure-dotnet-preview)), und übergeben Sie sie an die Methode [client.Reward](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.personalizer.personalizerclientextensions.reward?view=azure-dotnet-preview). 

Im Rahmen dieser Schnellstartanleitung ist die Bestimmung der Relevanz ganz einfach. In einem Produktionssystem kann die Bestimmung der Einflussfaktoren für die [Relevanzbewertung](concept-rewards.md) sowie deren jeweilige Gewichtung allerdings eine komplexe Angelegenheit sein und muss unter Umständen im Laufe der Zeit überarbeitet werden. Hierbei handelt es sich um eine der wichtigsten Entwurfsentscheidungen im Zusammenhang mit Ihrer Personalisierungsarchitektur. 

## <a name="code-examples"></a>Codebeispiele

In diesen Codeausschnitten werden folgende Vorgänge mit der Personalisierungsclientbibliothek für .NET veranschaulicht:

* [Erstellen eines Personalisierungsclients](#create-a-personalizer-client)
* [Anfordern eines Rangs](#request-a-rank)
* [Senden einer Relevanz](#send-a-reward)

## <a name="add-the-dependencies"></a>Hinzufügen der Abhängigkeiten

Öffnen Sie aus dem Projektverzeichnis die Datei **Program.cs** in Ihrem bevorzugten Editor oder Ihrer bevorzugten IDE. Ersetzen Sie den vorhandenen `using`-Code durch die folgenden `using`-Anweisungen:

[!code-csharp[Using statements](~/samples-personalizer/quickstarts/csharp/PersonalizerExample/Program.cs?name=Dependencies)]

## <a name="add-personalizer-resource-information"></a>Hinzufügen von Informationen zur Personalisierungsressource

Erstellen Sie in der Klasse **Program** Variablen für den Azure-Schlüssel und den Endpunkt Ihrer Ressource, die aus den Umgebungsvariablen gepullt wurden, und nennen Sie sie `PERSONALIZER_RESOURCE_KEY` und `PERSONALIZER_RESOURCE_ENDPOINT`. Wenn Sie die Umgebungsvariablen nach dem Start der Anwendung erstellen, muss der Editor, die IDE oder die Shell, in dem bzw. in der sie ausgeführt wird, geschlossen und erneut geladen werden, damit auf die Variable zugegriffen werden kann. Die Methoden werden später in dieser Schnellstartanleitung erstellt.

[!code-csharp[Create variables to hold the Personalizer resource key and endpoint values found in the Azure portal.](~/samples-personalizer/quickstarts/csharp/PersonalizerExample/Program.cs?name=classVariables)]

## <a name="create-a-personalizer-client"></a>Erstellen eines Personalisierungsclients

Erstellen Sie als Nächstes eine Methode zum Zurückgeben eines Personalisierungsclients. Der Parameter für die Methode lautet `PERSONALIZER_RESOURCE_ENDPOINT`, und der API-Schlüssel (ApiKey) ist `PERSONALIZER_RESOURCE_KEY`.

[!code-csharp[Create the Personalizer client](~/samples-personalizer/quickstarts/csharp/PersonalizerExample/Program.cs?name=authorization)]

## <a name="get-content-choices-represented-as-actions"></a>Abrufen von Inhaltsoptionen, dargestellt als Aktionen

Aktionen stellen die Inhaltsoptionen dar, denen die Personalisierung einen Rang zuweisen soll. Fügen Sie der Klasse „Program“ die folgenden Methoden hinzu, um die Benutzereingaben für Tageszeit und aktuellen Essenswunsch aus der Befehlszeile abzurufen.

[!code-csharp[Present time out day preference to the user](~/samples-personalizer/quickstarts/csharp/PersonalizerExample/Program.cs?name=createUserFeatureTimeOfDay)]

[!code-csharp[Present food taste preference to the user](~/samples-personalizer/quickstarts/csharp/PersonalizerExample/Program.cs?name=createUserFeatureTastePreference)]

Bei beiden Methoden wird die Methode `GetKey` verwendet, um die Auswahl des Benutzers aus der Befehlszeile zu lesen. 

[!code-csharp[Read user's choice from the command line](~/samples-personalizer/quickstarts/csharp/PersonalizerExample/Program.cs?name=readCommandLine)]

## <a name="create-the-learning-loop"></a>Erstellen der Lernschleife

Die Lernschleife der Personalisierung ist ein Zyklus aus Rangfolge- und Relevanzaufrufen. In dieser Schnellstartanleitung folgt auf jeden Rangfolgeaufruf zur Personalisierung des Inhalts ein Relevanzaufruf, um der Personalisierung mitzuteilen, wie hoch der Dienst den Inhalt bewertet hat. 

Der folgende Code in der Methode `main` des Programms durchläuft einen Zyklus in Form einer Schleife: Der Benutzer wird an der Befehlszeile nach seinen Präferenzen befragt, die Angaben werden zur Bewertung an die Personalisierung gesendet, die bewertete Auswahl wird dem Kunden in einer Auswahlliste angezeigt, und anschließend wird eine Relevanz an die Personalisierung gesendet, die angibt, wie gut der Dienst bei der Bewertung der Auswahl abgeschnitten hat.

```csharp
static void Main(string[] args)
{
    int iteration = 1;
    bool runLoop = true;

    // Get the actions list to choose from personalizer with their features.
    IList<RankableAction> actions = GetActions();

    // Initialize Personalizer client.
    PersonalizerClient client = InitializePersonalizerClient(ServiceEndpoint);

    do
    {
        Console.WriteLine("\nIteration: " + iteration++);

        // <rank>
        // Get context information from the user.
        string timeOfDayFeature = GetUsersTimeOfDay();
        string tasteFeature = GetUsersTastePreference();

        // Create current context from user specified data.
        IList<object> currentContext = new List<object>() {
            new { time = timeOfDayFeature },
            new { taste = tasteFeature }
        };

        // Exclude an action for personalizer ranking. This action will be held at its current position.
        // This simulates a business rule to force the action "juice" to be ignored in the ranking.
        // As juice is excluded, the return of the API will always be with a probability of 0.
        IList<string> excludeActions = new List<string> { "juice" };

        // Generate an ID to associate with the request.
        string eventId = Guid.NewGuid().ToString();

        // Rank the actions
        var request = new RankRequest(actions, currentContext, excludeActions, eventId);
        RankResponse response = client.Rank(request);
        // </rank>

        Console.WriteLine("\nPersonalizer service thinks you would like to have: " + response.RewardActionId + ". Is this correct? (y/n)");

        // <reward>
        float reward = 0.0f;
        string answer = GetKey();

        if (answer == "Y")
        {
            reward = 1;
            Console.WriteLine("\nGreat! Enjoy your food.");
        }
        else if (answer == "N")
        {
            reward = 0;
            Console.WriteLine("\nYou didn't like the recommended food choice.");
        }
        else
        {
            Console.WriteLine("\nEntered choice is invalid. Service assumes that you didn't like the recommended food choice.");
        }

        Console.WriteLine("\nPersonalizer service ranked the actions with the probabilities as below:");
        foreach (var rankedResponse in response.Ranking)
        {
            Console.WriteLine(rankedResponse.Id + " " + rankedResponse.Probability);
        }

        // Send the reward for the action based on user response.
        client.Reward(response.EventId, new RewardRequest(reward));
        // </reward>

        Console.WriteLine("\nPress q to break, any other key to continue:");
        runLoop = !(GetKey() == "Q");

    } while (runLoop);
}
```

Fügen Sie vor dem Ausführen der Codedatei die folgenden Methoden hinzu, die die [Inhaltsoptionen abrufen](#get-content-choices-represented-as-actions):

* GetUsersTimeOfDay
* GetUsersTastePreference
* GetKey

## <a name="request-a-rank"></a>Anfordern eines Rangs

Für die Rangfolgeanforderung erfragt das Programm die Präferenzen des Benutzers, um für die Inhaltsoptionen ein Element vom Typ `currentContent` zu erstellen. Der Prozess kann Inhalte erstellen, die von der Bewertung ausgenommen werden sollen (angezeigt als `excludeActions`). Die Rangfolgeanforderung muss die Aktionen, den aktuellen Kontext (currentContext), die ausgeschlossenen Aktionen (excludeActions) und eine eindeutige Rangfolgeereignis-ID (als GUID) enthalten, um die bewertete Antwort zu erhalten. 

In dieser Schnellstartanleitung werden die einfachen Kontextmerkmale „Tageszeit“ und „Essenswunsch des Benutzers“ verwendet. In Produktionssystemen kann die Bestimmung und [Auswertung](concept-feature-evaluation.md) von [Aktionen und Merkmalen](concepts-features.md) allerdings eine komplexe Angelegenheit sein.  

[!code-csharp[The Personalizer learning loop ranks the request.](~/samples-personalizer/quickstarts/csharp/PersonalizerExample/Program.cs?name=rank)]

## <a name="send-a-reward"></a>Senden einer Relevanz

Für die Relevanzanforderung ruft das Programm die Auswahl des Benutzers aus der Befehlszeile ab, weist jeder Auswahl einen numerischen Wert zu und sendet dann die eindeutige Rangfolgeereignis-ID und den numerischen Wert an die Relevanzmethode.

In dieser Schnellstartanleitung wird als Relevanz eine einfache Zahl (0 oder 1) zugewiesen. In Produktionssystemen ist die Entscheidung, was wann an den [Relevanzaufruf](concept-rewards.md) gesendet werden soll (abhängig von Ihren spezifischen Anforderungen), unter Umständen etwas komplizierter. 

[!code-csharp[The Personalizer learning loop ranks the request.](~/samples-personalizer/quickstarts/csharp/PersonalizerExample/Program.cs?name=reward)]

## <a name="run-the-program"></a>Ausführen des Programms

Führen Sie die Anwendung mit dem Befehl dotnet `run` über das Anwendungsverzeichnis aus.

```console
dotnet run
```

![Das Schnellstartprogramm stellt ein paar Fragen zum Sammeln von Benutzereinstellungen – auch bekannt als Features – und stellt dann die am besten bewertete Aktion bereit.](media/csharp-quickstart-commandline-feedback-loop/quickstart-program-feedback-loop-example.png)

Der [Quellcode für diese Schnellstartanleitung](https://github.com/Azure-Samples/cognitive-services-personalizer-samples/blob/master/quickstarts/csharp/PersonalizerExample/Program.cs) steht im GitHub-Repository mit den Personalisierungsbeispielen zur Verfügung.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie ein Cognitive Services-Abonnement bereinigen und entfernen möchten, können Sie die Ressource oder die Ressourcengruppe löschen. Wenn Sie die Ressourcengruppe löschen, werden auch alle anderen Ressourcen gelöscht, die ihr zugeordnet sind.

* [Portal](../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure-Befehlszeilenschnittstelle](../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
>[Funktionsweise der Personalisierung](how-personalizer-works.md)

* [Was ist die Personalisierung?](what-is-personalizer.md)
* [Wo können Sie Personalisierung verwenden?](where-can-you-use-personalizer.md)
* [Problembehandlung](troubleshooting.md)

