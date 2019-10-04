---
title: 'Schnellstart: Personalisierungsclientbibliothek für Python | Microsoft-Dokumentation'
titleSuffix: Azure Cognitive Services
description: Erste Schritte mit der Personalisierungsclientbibliothek für Python unter Verwendung einer Lernschleife
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: quickstart
ms.date: 09/12/2019
ms.author: diberry
ms.openlocfilehash: 014a5f264b9beed666f718cda52d197381d58876
ms.sourcegitcommit: 3f22ae300425fb30be47992c7e46f0abc2e68478
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2019
ms.locfileid: "71266253"
---
# <a name="quickstart-personalize-client-library-for-python"></a>Schnellstart: Personalisierungsclientbibliothek für Python

Zeigen Sie personalisierten Inhalt in dieser Python-Schnellstartanleitung mit dem Personalisierungsdienst an.

Hier erhalten Sie Informationen zu den ersten Schritten mit der Personalisierungsclientbibliothek für Python. Führen Sie die nachfolgenden Schritte zum Installieren des Pakets aus, und testen Sie den Beispielcode für grundlegende Aufgaben.

 * Erstellen Sie eine Bewertungsliste der Aktionen zur Personalisierung.
 * Melden Sie die Relevanzbewertung zur Angabe des Erfolgs der am besten bewerteten Aktion.

[Paket (pypi)](https://pypi.org/project/azure-cognitiveservices-personalizer/) | [Beispiele](https://github.com/Azure-Samples/cognitive-services-personalizer-samples/blob/master/quickstarts/python/sample.py)

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement – [Erstellen eines kostenlosen Kontos](https://azure.microsoft.com/free/)
* [Python 3.x](https://www.python.org/)

## <a name="setting-up"></a>Einrichten

### <a name="create-a-personalizer-azure-resource"></a>Erstellen einer Azure-Ressource für die Personalisierung

Azure Cognitive Services werden von Azure-Ressourcen dargestellt, die Sie abonnieren. Erstellen Sie auf Ihrem lokalen Computer eine Ressource für die Personalisierung (entweder über das [Azure-Portal](https://portal.azure.com/) oder mithilfe der [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account-cli)). Weitere Informationen finden Sie unter [Erstellen einer Cognitive Services-Ressource mithilfe des Azure-Portals](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account). Weitere Funktionen:

* Rufen Sie einen [Testschlüssel](https://azure.microsoft.com/try/cognitive-services) ab, mit dem Sie sieben Tage lang kostenlos testen können. Nach der Registrierung steht dieser auf der [Azure-Website](https://azure.microsoft.com/try/cognitive-services/my-apis/) zur Verfügung.  
* Zeigen Sie Ihre Ressource im [Azure-Portal](https://portal.azure.com/) an.

Nachdem Sie einen Schlüssel für Ihr Testabonnement bzw. Ihre Ressource erhalten haben, erstellen Sie zwei [Umgebungsvariablen](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication):

* `PERSONALIZER_KEY` für den Ressourcenschlüssel.
* `PERSONALIZER_ENDPOINT` für den Ressourcenendpunkt.

Die Schlüssel- und Endpunktwerte finden Sie im Azure-Portal auf der Seite **Schnellstart**.


### <a name="install-the-python-library-for-personalizer"></a>Installieren der Python-Bibliothek für die Personalisierung

Installieren Sie mit dem folgenden Befehl die Personalisierungsclientbibliothek für Python:

```console
pip install azure-cognitiveservices-personalizer
```

Bei Verwendung der Visual Studio-IDE ist die Clientbibliothek als herunterladbares NuGet-Paket verfügbar.

### <a name="change-the-model-update-frequency"></a>Ändern der Häufigkeit der Modellaktualisierung

Ändern Sie in der Personalisierungsressource im Azure-Portal die **Häufigkeit der Modellaktualisierung** in 10 Sekunden. Dadurch wird der Dienst schnell trainiert, und Sie können sehen, wie sich die oberste Aktion für jede Iteration ändert.

![Ändern der Häufigkeit der Modellaktualisierung](./media/settings/configure-model-update-frequency-settings.png)

Bei der Erstinstanziierung einer Personalisierungsschleife ist kein Modell vorhanden, da noch keine Relevanz-API-Aufrufe zum Trainieren ausgeführt wurden. Rangfolgeaufrufe geben gleiche Wahrscheinlichkeiten für jedes Element zurück. Ihre Anwendung sollte dem Inhalt dennoch immer anhand der Ausgabe von RewardActionId einen Rang zuweisen.

## <a name="object-model"></a>Objektmodell

Der Personalisierungsclient ist ein Objekt vom Typ „PersonalizerClient“, das mithilfe der Klasse „Microsoft.Rest.ServiceClientCredentials“, die Ihren Schlüssel enthält, bei Azure authentifiziert wird.

Erstellen Sie zum Anfordern eines Inhaltsrangs eine Rangfolgeanforderung (RankRequest), und übergeben Sie sie an die Methode „client.Rank“. Die Methode „Rank“ gibt eine Rangfolgeantwort (RankResponse) mit dem bewerteten Inhalt zurück. 

Um eine Relevanz an die Personalisierung zu senden, erstellen Sie eine Relevanzanforderung (RewardRequest), und übergeben Sie sie an die Methode „client.Reward“. 

Im Rahmen dieser Schnellstartanleitung ist die Bestimmung der Relevanz ganz einfach. In einem Produktionssystem kann die Bestimmung der Einflussfaktoren für die [Relevanzbewertung](concept-rewards.md) sowie deren jeweilige Gewichtung allerdings eine komplexe Angelegenheit sein und muss unter Umständen im Laufe der Zeit überarbeitet werden. Hierbei handelt es sich um eine der wichtigsten Entwurfsentscheidungen im Zusammenhang mit Ihrer Personalisierungsarchitektur. 

## <a name="code-examples"></a>Codebeispiele

In diesen Codeausschnitten werden folgende Vorgänge mit der Personalisierungsclientbibliothek für Python veranschaulicht:

* [Erstellen eines Personalisierungsclients](#create-a-personalizer-client)
* [Anfordern eines Rangs](#request-a-rank)
* [Senden einer Relevanz](#send-a-reward)

## <a name="create-a-new-python-application"></a>Erstellen einer neuen Python-Anwendung

Erstellen Sie eine neue Python-Anwendung namens `sample.py` in Ihrem bevorzugten Editor oder Ihrer bevorzugten IDE. 

## <a name="add-the-dependencies"></a>Hinzufügen der Abhängigkeiten

Öffnen Sie aus dem Projektverzeichnis die Datei **Program.cs** in Ihrem bevorzugten Editor oder Ihrer bevorzugten IDE. Ersetzen Sie den vorhandenen `using`-Code durch die folgenden `using`-Anweisungen:

[!code-python[Add module dependencies](~/samples-personalizer/quickstarts/python/sample.py?name=Dependencies)]

## <a name="add-personalizer-resource-information"></a>Hinzufügen von Informationen zur Personalisierungsressource

Erstellen Sie in der Klasse **Program** Variablen für den Azure-Schlüssel und den Endpunkt Ihrer Ressource, die aus den Umgebungsvariablen gepullt wurden, und nennen Sie sie `PERSONALIZER_RESOURCE_KEY` und `PERSONALIZER_RESOURCE_ENDPOINT`. Wenn Sie die Umgebungsvariablen nach dem Start der Anwendung erstellen, muss der Editor, die IDE oder die Shell, in dem bzw. in der sie ausgeführt wird, geschlossen und erneut geladen werden, damit auf die Variable zugegriffen werden kann. Die Methoden werden später in dieser Schnellstartanleitung erstellt.

[!code-python[Create variables to hold the Personalizer resource key and endpoint values found in the Azure portal.](~/samples-personalizer/quickstarts/python/sample.py?name=AuthorizationVariables)]

## <a name="create-a-personalizer-client"></a>Erstellen eines Personalisierungsclients

Erstellen Sie als Nächstes eine Methode zum Zurückgeben eines Personalisierungsclients. Der Parameter für die Methode lautet `PERSONALIZER_RESOURCE_ENDPOINT`, und der API-Schlüssel (ApiKey) ist `PERSONALIZER_RESOURCE_KEY`.

[!code-python[Create the Personalizer client](~/samples-personalizer/quickstarts/python/sample.py?name=Client)]

## <a name="get-content-choices-represented-as-actions"></a>Abrufen von Inhaltsoptionen, dargestellt als Aktionen

Aktionen stellen die Inhaltsoptionen dar, denen die Personalisierung einen Rang zuweisen soll. Fügen Sie der Klasse „Program“ die folgenden Methoden hinzu, um die Benutzereingaben für Tageszeit und aktuellen Essenswunsch aus der Befehlszeile abzurufen.

[!code-python[Present time out day preference to the user](~/samples-personalizer/quickstarts/python/sample.py?name=getActions)]

[!code-python[Present time out day preference to the user](~/samples-personalizer/quickstarts/python/sample.py?name=createUserFeatureTimeOfDay)]

[!code-python[Present food taste preference to the user](~/samples-personalizer/quickstarts/python/sample.py?name=createUserFeatureTastePreference)]

## <a name="create-the-learning-loop"></a>Erstellen der Lernschleife

Die Lernschleife der Personalisierung ist ein Zyklus aus [Rangfolge-](#request-a-rank) und [Relevanzaufrufen](#send-a-reward). In dieser Schnellstartanleitung folgt auf jeden Rangfolgeaufruf zur Personalisierung des Inhalts ein Relevanzaufruf, um der Personalisierung mitzuteilen, wie hoch der Dienst den Inhalt bewertet hat. 

Der folgende Code in der Methode `main` des Programms durchläuft einen Zyklus in Form einer Schleife: Der Benutzer wird an der Befehlszeile nach seinen Präferenzen befragt, die Angaben werden zur Bewertung an die Personalisierung gesendet, die bewertete Auswahl wird dem Kunden in einer Auswahlliste angezeigt, und anschließend wird eine Relevanz an die Personalisierung gesendet, die angibt, wie gut der Dienst bei der Bewertung der Auswahl abgeschnitten hat.

[!code-python[The Personalizer learning loop ranks the request.](~/samples-personalizer/quickstarts/python/sample.py?name=mainLoop&highlight=9,10,29)]

Sehen Sie sich die Rangfolge- und Relevanzaufrufe in den folgenden Abschnitten genauer an.

Fügen Sie vor dem Ausführen der Codedatei die folgenden Methoden hinzu, die die [Inhaltsoptionen abrufen](#get-content-choices-represented-as-actions):

* get_user_preference
* get_user_timeofday
* get_actions

## <a name="request-a-rank"></a>Anfordern eines Rangs

Für die Rangfolgeanforderung erfragt das Programm die Präferenzen des Benutzers, um für die Inhaltsoptionen ein Element vom Typ `currentContent` zu erstellen. Der Prozess kann Inhalte erstellen, die von der Bewertung ausgenommen werden sollen (angezeigt als `excludeActions`). Die Rangfolgeanforderung muss die Aktionen, den aktuellen Kontext (currentContext), die ausgeschlossenen Aktionen (excludeActions) und eine eindeutige Rangfolgeereignis-ID (als GUID) enthalten, um die bewertete Antwort zu erhalten. 

In dieser Schnellstartanleitung werden die einfachen Kontextmerkmale „Tageszeit“ und „Essenswunsch des Benutzers“ verwendet. In Produktionssystemen kann die Bestimmung und [Auswertung](concept-feature-evaluation.md) von [Aktionen und Merkmalen](concepts-features.md) allerdings eine komplexe Angelegenheit sein.  

[!code-python[The Personalizer learning loop ranks the request.](~/samples-personalizer/quickstarts/python/sample.py?name=rank)]

## <a name="send-a-reward"></a>Senden einer Relevanz

Für die Relevanzanforderung ruft das Programm die Auswahl des Benutzers aus der Befehlszeile ab, weist jeder Auswahl einen numerischen Wert zu und sendet dann die eindeutige Rangfolgeereignis-ID und den numerischen Wert an die Relevanzmethode.

In dieser Schnellstartanleitung wird als Relevanz eine einfache Zahl (0 oder 1) zugewiesen. In Produktionssystemen ist die Entscheidung, was wann an den [Relevanzaufruf](concept-rewards.md) gesendet werden soll (abhängig von Ihren spezifischen Anforderungen), unter Umständen etwas komplizierter. 

[!code-python[The Personalizer learning loop sends a reward.](~/samples-personalizer/quickstarts/python/sample.py?name=reward&highlight=9)]

## <a name="run-the-program"></a>Ausführen des Programms

Führen Sie die Anwendung mit dem Python-Beispiel aus dem Anwendungsverzeichnis aus.

```console
python sample.py
```

![Das Schnellstartprogramm stellt ein paar Fragen zum Sammeln von Benutzereinstellungen – auch bekannt als Features – und stellt dann die am besten bewertete Aktion bereit.](./media/csharp-quickstart-commandline-feedback-loop/quickstart-program-feedback-loop-example.png)

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
* Den Quellcode für dieses Beispiel finden Sie auf [GitHub](https://github.com/Azure-Samples/cognitive-services-personalizer-samples/blob/master/quickstarts/python/sample.py).
