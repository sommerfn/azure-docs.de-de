---
title: Erstellen einer Funktion, die in Azure Logic Apps integriert ist
description: Erstellen Sie eine Funktion, die Azure Logic Apps und Azure Cognitive Services integriert, um Stimmungen in Tweets zu kategorisieren und Benachrichtigungen zu senden, wenn die Stimmungslage schlecht ist.
author: craigshoemaker
ms.assetid: 60495cc5-1638-4bf0-8174-52786d227734
ms.topic: tutorial
ms.date: 11/06/2018
ms.author: cshoe
ms.custom: mvc, cc996988-fb4f-47
ms.openlocfilehash: c7b8e41cc09137ee06e975d136dd999ba146731b
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74226605"
---
# <a name="create-a-function-that-integrates-with-azure-logic-apps"></a>Erstellen einer Funktion, die in Azure Logic Apps integriert ist

Azure Functions ist in Azure Logic Apps im Logik-App-Designer integriert. Dank dieser Integration können Sie die Rechenleistung von Functions in Orchestrierungen mit anderen Azure- und Drittanbieterdiensten nutzen. 

In diesem Tutorial wird gezeigt, wie Sie Functions mit Logic Apps und Cognitive Services in Azure verwenden, um eine Standpunktanalyse für Twitter-Postings auszuführen. Mit einer Funktion mit HTTP-Auslösung werden Tweets basierend auf dem Stimmungswert als Grün, Gelb oder Rot (Green, Yellow, Red) kategorisiert. Wenn eine negative Stimmung erkannt wird, wird eine E-Mail gesendet. 

![Abbildung: Die ersten beiden Schritte der App im Logik-App-Designer](media/functions-twitter-email/00-logic-app-overview.png)

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen Sie eine Cognitive Services-API-Ressource.
> * Erstellen einer Funktion, mit der die Stimmung von Tweets kategorisiert wird
> * Erstellen einer Logik-App, mit der eine Verbindung mit Twitter hergestellt wird
> * Hinzufügen der Stimmungserkennung zur Logik-App 
> * Verbinden der Logik-App mit der Funktion
> * Senden einer E-Mail basierend auf der Antwort von der Funktion

## <a name="prerequisites"></a>Voraussetzungen

+ Ein aktives [Twitter](https://twitter.com/)-Konto 
+ Ein [Outlook.com](https://outlook.com/)-Konto (zum Senden von Benachrichtigungen)
+ In diesem Artikel werden als Ausgangspunkt die Ressourcen verwendet, die unter [Erstellen Ihrer ersten Funktion im Azure-Portal](functions-create-first-azure-function.md) erstellt wurden.  
Führen Sie nun diese Schritte zum Erstellen Ihrer Funktionen-App durch, sofern dies noch nicht geschehen ist.

## <a name="create-a-cognitive-services-resource"></a>Erstellen einer Cognitive Services-Ressource

Die Cognitive Services-APIs sind in Azure als einzelne Ressourcen verfügbar. Verwenden Sie die Textanalyse-API, um die Stimmungslage der überwachten Tweets zu ermitteln.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

2. Klicken Sie im Azure-Portal links oben auf **Ressource erstellen**.

3. Klicken Sie auf **KI + Machine Learning** > **Textanalyse**. Verwenden Sie dann die in der Tabelle angegebenen Einstellungen, um die Ressource zu erstellen.

    ![Seite „Cognitive-Ressource erstellen“](media/functions-twitter-email/01-create-text-analytics.png)

    | Einstellung      |  Empfohlener Wert   | Beschreibung                                        |
    | --- | --- | --- |
    | **Name** | MyCognitiveServicesAccnt | Wählen Sie einen eindeutigen Kontonamen. |
    | **Location** | USA (Westen) | Verwenden Sie den nächstgelegenen Standort. |
    | **Preisstufe** | F0 | Beginnen Sie mit dem niedrigsten Tarif. Wenn die Aufrufe nicht ausreichen, können Sie einen höheren Tarif festlegen.|
    | **Ressourcengruppe** | myResourceGroup | Verwenden Sie für alle Dienste in diesem Tutorial dieselbe Ressourcengruppe.|

4. Klicken Sie auf **Erstellen**, um die Ressource zu erstellen. 

5. Klicken Sie auf **Übersicht**, und kopieren Sie den Wert für **Endpunkt** in einen Text-Editor. Dieser Wert wird beim Herstellen einer Verbindung mit der Cognitive Services-API verwendet.

    ![Cognitive Services-Einstellungen](media/functions-twitter-email/02-cognitive-services.png)

6. Klicken Sie in der Navigationsspalte links auf **Schlüssel**, kopieren Sie den Wert von **Schlüssel 1**, und speichern Sie ihn in einem Text-Editor. Sie verwenden den Schlüssel, um die Logik-App mit Ihrer Cognitive Services-API zu verbinden. 
 
    ![Cognitive Services-Schlüssel](media/functions-twitter-email/03-cognitive-serviecs-keys.png)

## <a name="create-the-function-app"></a>Erstellen der Funktionen-App

„Functions“ ist eine gute Möglichkeit, um Verarbeitungsaufgaben in einen Logic Apps-Workflow zu verlagern. In diesem Tutorial wird eine Funktion mit HTTP-Auslösung verwendet, um Tweet-Stimmungswerte aus Cognitive Services zu verarbeiten und einen Kategoriewert zurückzugeben.  

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

## <a name="create-an-http-triggered-function"></a>Erstellen einer durch HTTP ausgelösten Funktion  

1. Erweitern Sie die Funktionen-App, und klicken Sie auf die Schaltfläche **+** neben **Functions**. Wenn dies die erste Funktion in Ihrer Funktions-App ist, wählen Sie **Im Portal** aus.

    ![Schnellstartseite für Funktionen im Azure-Portal](media/functions-twitter-email/05-function-app-create-portal.png)

2. Wählen Sie als Nächstes **Webhook + API** aus, und klicken Sie auf **Erstellen**. 

    ![Auswählen des HTTP-Triggers](./media/functions-twitter-email/06-function-webhook.png)

3. Ersetzen Sie den Inhalt der Datei `run.csx` durch den folgenden Code, und klicken Sie auf **Speichern**:

    ```csharp
    #r "Newtonsoft.Json"
    
    using System;
    using System.Net;
    using Microsoft.AspNetCore.Mvc;
    using Microsoft.Extensions.Logging;
    using Microsoft.Extensions.Primitives;
    using Newtonsoft.Json;
    
    public static async Task<IActionResult> Run(HttpRequest req, ILogger log)
    {
        string category = "GREEN";
    
        string requestBody = await new StreamReader(req.Body).ReadToEndAsync();
        log.LogInformation(string.Format("The sentiment score received is '{0}'.", requestBody));
    
        double score = Convert.ToDouble(requestBody);
    
        if(score < .3)
        {
            category = "RED";
        }
        else if (score < .6) 
        {
            category = "YELLOW";
        }
    
        return requestBody != null
            ? (ActionResult)new OkObjectResult(category)
            : new BadRequestObjectResult("Please pass a value on the query string or in the request body");
    }
    ```
    Dieser Funktionscode gibt eine Farbkategorie zurück, die auf dem mit der Anforderung empfangenen Stimmungswert basiert. 

4. Klicken Sie zum Testen der Funktion ganz rechts auf **Testen**, um die Registerkarte „Testen“ zu erweitern. Geben Sie den Wert `0.2` für **Anforderungstext** ein, und klicken Sie dann auf **Ausführen**. Im Text der Antwort wird der Wert **RED** (ROT) zurückgegeben. 

    ![Testen der Funktion im Azure-Portal](./media/functions-twitter-email/07-function-test.png)

Sie verfügen nun über eine Funktion zum Kategorisieren von Stimmungswerten. Als Nächstes erstellen Sie eine Logik-App, die Ihre Funktion in Ihre Twitter- und Cognitive Services-API integriert. 

## <a name="create-a-logic-app"></a>Erstellen einer Logik-App   

1. Klicken Sie in der linken oberen Ecke des Azure-Portals auf die Schaltfläche **Neu**.

2. Klicken Sie auf **Web** > **Logik-App**.
 
3. Geben Sie dann einen Wert für **Name** (beispielsweise `TweetSentiment`) ein, und verwenden Sie die Einstellungen aus der Tabelle.

    ![Erstellen einer Logik-App im Azure-Portal](./media/functions-twitter-email/08-logic-app-create.png)

    | Einstellung      |  Empfohlener Wert   | Beschreibung                                        |
    | ----------------- | ------------ | ------------- |
    | **Name** | TweetSentiment | Wählen Sie einen geeigneten Namen für Ihre App. |
    | **Ressourcengruppe** | myResourceGroup | Wählen Sie dieselbe vorhandene Ressourcengruppe wie zuvor. |
    | **Location** | East US | Wählen Sie einen Standort in Ihrer Nähe aus. |    

4. Nachdem Sie die richtigen Einstellungswerte eingegeben haben, klicken Sie auf **Erstellen**, um Ihre Logik-App zu erstellen. 

5. Klicken Sie nach der Erstellung der App auf Ihre neue Logik-App, die an das Dashboard angeheftet ist. Führen Sie dann im Logik-App-Designer einen Bildlauf nach unten durch, und klicken Sie auf die Vorlage **Leere Logik-App**. 

    ![Vorlage „Leere Logik-App“](media/functions-twitter-email/09-logic-app-create-blank.png)

Sie können jetzt den Logik-App-Designer verwenden, um Ihrer App Dienste und Trigger hinzuzufügen.

## <a name="connect-to-twitter"></a>Verbinden mit Twitter

Erstellen Sie zuerst eine Verbindung mit Ihrem Twitter-Konto. Die Logik-App führt eine Abfrage nach Tweets durch, die die Ausführung der App auslösen.

1. Klicken Sie im Designer auf den Dienst **Twitter**, und klicken Sie dann auf den Trigger **Wenn ein neuer Tweet gepostet wird**. Melden Sie sich an Ihrem Twitter-Konto an, und lassen Sie für Logic Apps die Verwendung Ihres Kontos zu.

2. Verwenden Sie die Twitter-Triggereinstellungen, wie in der Tabelle angegeben. 

    ![Twitter-Connectoreinstellungen](media/functions-twitter-email/10-tweet-settings.png)

    | Einstellung      |  Empfohlener Wert   | BESCHREIBUNG                                        |
    | ----------------- | ------------ | ------------- |
    | **Suchtext** | #Azure | Verwenden Sie ein Hashtag, das beliebt genug ist, damit im gewählten Zeitraum neue Tweets generiert werden. Wenn Sie den Free-Tarif verwenden und Ihr Hashtag eine zu hohe Beliebtheit aufweist, kann es passieren, dass das Transaktionskontingent Ihrer Cognitive Services-API schnell aufgebraucht ist. |
    | **Intervall** | 15 | Die zwischen Twitter-Anforderungen verstrichene Zeit in Häufigkeitseinheiten. |
    | **Frequency** | Minute | Die Häufigkeitseinheit, die zum Abfragen von Twitter verwendet wird.  |

3.  Klicken Sie auf **Speichern**, um eine Verbindung mit Ihrem Twitter-Konto herzustellen. 

Ihre App ist jetzt mit Twitter verbunden. Stellen Sie als Nächstes eine Verbindung mit der Textanalyse her, um die Stimmung für die gesammelten Tweets zu erkennen.

## <a name="add-sentiment-detection"></a>Hinzufügen der Stimmungserkennung

1. Klicken Sie auf **Neuer Schritt** und dann auf **Aktion hinzufügen**.

2. Geben Sie unter **Aktion auswählen** den Begriff **Textanalyse** ein, und klicken Sie anschließend auf die Aktion **Stimmung erkennen**.
    
    ![„Neuer Schritt“ und anschließend „Aktion hinzufügen“](media/functions-twitter-email/11-detect-sentiment.png)

3. Geben Sie einen Verbindungsnamen (beispielsweise `MyCognitiveServicesConnection`) ein, fügen Sie den Schlüssel für Ihre Cognitive Services-API und den Cognitive Services-Endpunkt ein, die Sie in einem Text-Editor gespeichert haben, und klicken Sie auf **Erstellen**.

    ![„Neuer Schritt“ und anschließend „Aktion hinzufügen“](media/functions-twitter-email/12-connection-settings.png)

4. Geben Sie als Nächstes **Tweettext** in das Textfeld ein, und klicken Sie dann auf **Neuer Schritt**.

    ![Festlegen des zu analysierenden Texts](media/functions-twitter-email/13-analyze-tweet-text.png)

Nachdem die Stimmungserkennung nun konfiguriert wurde, können Sie Ihrer Funktion eine Verbindung hinzufügen, die die Ausgabe mit dem Stimmungswert nutzt.

## <a name="connect-sentiment-output-to-your-function"></a>Ausgabe mit dem Stimmungswert für Ihre Funktion

1. Klicken Sie im Designer für Logik-Apps auf **Neuer Schritt** > **Add an action** (Aktion hinzufügen), filtern Sie nach **Azure Functions**, und klicken Sie auf **Azure-Funktion wählen**.

    ![Stimmung erkennen](media/functions-twitter-email/14-azure-functions.png)
  
4. Wählen Sie die zuvor erstellte Funktions-App aus.

    ![Auswählen der Funktion](media/functions-twitter-email/15-select-function.png)

5. Wählen Sie die für dieses Tutorial erstellte Funktion aus.

    ![Auswählen der Funktion](media/functions-twitter-email/16-select-function.png)

4. Klicken Sie unter **Anforderungstext** auf **Score** (Ergebnis) und dann auf **Speichern**.

    ![Punkte](media/functions-twitter-email/17-function-input-score.png)

Ihre Funktion wird jetzt ausgelöst, wenn von der Logik-App ein Stimmungswert gesendet wird. Von der Funktion wird eine Kategorie mit Farbcodierung an die Logik-App zurückgegeben. Als Nächstes fügen Sie eine E-Mail-Benachrichtigung hinzu, die gesendet wird, wenn der Stimmungswert **RED** (ROT) von der Funktion zurückgegeben wird. 

## <a name="add-email-notifications"></a>Hinzufügen von E-Mail-Benachrichtigungen

Der letzte Teil des Workflows ist das Auslösen einer E-Mail, wenn der Wert für die Stimmung _RED_ (ROT) lautet. In diesem Thema wird ein Outlook.com-Connector verwendet. Sie können ähnliche Schritte ausführen, um einen Gmail- oder Office 365 Outlook-Connector zu verwenden.   

1. Klicken Sie im Logik-App-Designer auf **Neuer Schritt** > **Bedingung hinzufügen**. 

    ![Fügen Sie der Logik-App eine Bedingung hinzu.](media/functions-twitter-email/18-add-condition.png)

2. Klicken Sie auf **Wert auswählen** und dann auf **Text**. Wählen Sie die Option **ist gleich**, klicken Sie auf **Wert auswählen**, und geben Sie `RED` ein. Klicken Sie anschließen auf **Speichern**. 

    ![Wählen Sie eine Aktion für die Bedingung aus.](media/functions-twitter-email/19-condition-settings.png)    

3. Klicken Sie unter **IF TRUE** (WENN TRUE) auf **Aktion hinzufügen**, suchen Sie nach `outlook.com`, klicken Sie auf **E-Mail senden**, und melden Sie sich bei Ihrem Outlook.com-Konto an.

    ![Konfigurieren Sie die E-Mail für die Aktion „E-Mail senden“.](media/functions-twitter-email/20-add-outlook.png)

    > [!NOTE]
    > Wenn Sie kein Outlook.com-Konto besitzen, können Sie einen anderen Connector wählen, z.B. Gmail oder Office 365 Outlook.

4. Verwenden Sie in der Aktion **E-Mail senden** die E-Mail-Einstellungen gemäß den Angaben in der Tabelle. 

    ![Konfigurieren Sie die E-Mail für die Aktion „E-Mail senden“.](media/functions-twitter-email/21-configure-email.png)
    
| Einstellung      |  Empfohlener Wert   | BESCHREIBUNG  |
| ----------------- | ------------ | ------------- |
| **An** | Geben Sie Ihre E-Mail-Adresse ein | Die E-Mail-Adresse, an die die Benachrichtigung gesendet wird. |
| **Betreff** | Negative Tweet-Stimmung erkannt  | Die Betreffzeile der E-Mail-Benachrichtigung.  |
| **Text** | Tweettext, Standort | Klicken Sie auf die Parameter **Tweettext** und **Standort**. |

1. Klicken Sie auf **Speichern**.

Nachdem der Workflow jetzt fertig ist, können Sie die Logik-App aktivieren und die Funktion in Aktion sehen.

## <a name="test-the-workflow"></a>Testen des Workflows

1. Klicken Sie im Logik-App-Designer auf **Ausführen**, um die App zu starten.

2. Klicken Sie in der linken Spalte auf **Übersicht**, um den Status der Logik-App anzuzeigen. 
 
    ![Ausführungsstatus der Logik-App](media/functions-twitter-email/22-execution-history.png)

3. (Optional) Klicken Sie auf eine der Ausführungen, um die Details dazu anzuzeigen.

4. Navigieren Sie zu Ihrer Funktion, zeigen Sie die Protokolle an, und stellen Sie sicher, dass Stimmungswerte empfangen und verarbeitet wurden.
 
    ![Anzeigen von Funktionsprotokollen](media/functions-twitter-email/sent.png)

5. Wenn eine potenziell negative Stimmung erkannt wird, erhalten Sie eine E-Mail. Falls Sie keine E-Mail erhalten haben, können Sie den Funktionscode so ändern, dass jedes Mal RED (ROT) zurückgegeben wird:

    ```csharp
    return (ActionResult)new OkObjectResult("RED");
    ```

    Wechseln Sie nach der Überprüfung der E-Mail-Benachrichtigungen zurück zum ursprünglichen Code:

    ```csharp
    return requestBody != null
        ? (ActionResult)new OkObjectResult(category)
        : new BadRequestObjectResult("Please pass a value on the query string or in the request body");
    ```

    > [!IMPORTANT]
    > Nach Abschluss dieses Tutorials sollten Sie die Logik-App deaktivieren. Durch das Deaktivieren der App verhindern Sie, dass Ihnen für Ausführungen Gebühren berechnet und die Transaktionen für Ihre Cognitive Services-API aufgebraucht werden.

Sie haben gesehen, wie einfach es ist, Functions in einen Logik-Apps-Workflow zu integrieren.

## <a name="disable-the-logic-app"></a>Deaktivieren der Logik-App

Klicken Sie zum Deaktivieren der Logik-App auf **Übersicht** und dann oben auf dem Bildschirm auf **Deaktivieren**. Durch die Deaktivierung der App wird verhindert, dass sie ausgeführt wird und Gebühren anfallen, ohne dass die App gelöscht wird.

![Funktionsprotokolle](media/functions-twitter-email/disable-logic-app.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen Sie eine Cognitive Services-API-Ressource.
> * Erstellen einer Funktion, mit der die Stimmung von Tweets kategorisiert wird
> * Erstellen einer Logik-App, mit der eine Verbindung mit Twitter hergestellt wird
> * Hinzufügen der Stimmungserkennung zur Logik-App 
> * Verbinden der Logik-App mit der Funktion
> * Senden einer E-Mail basierend auf der Antwort von der Funktion

Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie für Ihre Funktion eine serverlose API erstellen.

> [!div class="nextstepaction"] 
> [Erstellen einer serverlosen API mit Azure Functions](functions-create-serverless-api.md)

Weitere Informationen zu Logik-Apps finden Sie unter [Azure Logic Apps](../logic-apps/logic-apps-overview.md).

