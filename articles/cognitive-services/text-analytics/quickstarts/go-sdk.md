---
title: 'Schnellstart: Textanalyse-Clientbibliothek für Go | Microsoft-Dokumentation'
titleSuffix: Azure Cognitive Services
description: Informationen und Codebeispiele für die ersten Schritte mit der Textanalyse-API in Azure Cognitive Services
services: cognitive-services
author: laramume
manager: assafi
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: quickstart
ms.date: 08/28/2019
ms.author: aahi
ms.openlocfilehash: 1ba2ec6b5c0c59be7b7264f7558fbb393adcc2d8
ms.sourcegitcommit: d200cd7f4de113291fbd57e573ada042a393e545
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/29/2019
ms.locfileid: "70142793"
---
# <a name="quickstart-text-analytics-client-library-for-go"></a>Schnellstart: Textanalyse-Clientbibliothek für Go

Steigen Sie in die Textanalyse-Clientbibliothek für Go ein. Führen Sie die nachfolgenden Schritte zum Installieren des Pakets aus, und testen Sie den Beispielcode für grundlegende Aufgaben. 

Verwenden Sie die Textanalyse-Clientbibliothek für Go für folgende Zwecke:

* Stimmungsanalyse
* Spracherkennung
* Entitätserkennung
* Schlüsselwortextraktion

[Referenzdokumentation](https://docs.microsoft.com/python/api/overview/azure/cognitiveservices/textanalytics?view=azure-python) | [Quellcode der Bibliothek](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cognitiveservices/azure-cognitiveservices-language-textanalytics) | [Paket (GitHub)](https://github.com/Azure/azure-sdk-for-go/tree/master/services/cognitiveservices/v2.1/textanalytics) | [Beispiele](https://github.com/Azure-Samples/cognitive-services-quickstart-code)

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement: [Kostenloses Azure-Konto](https://azure.microsoft.com/free/)
* Aktuelle Version von [Go](https://golang.org/dl/)

## <a name="setting-up"></a>Einrichten

### <a name="create-a-text-analytics-azure-resource"></a>Erstellen einer Azure-Ressource für die Textanalyse 

Azure Cognitive Services werden von Azure-Ressourcen dargestellt, die Sie abonnieren. Erstellen Sie mithilfe des [Azure-Portals](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) oder der [Azure CLI](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account-cli) auf Ihrem lokalen Computer eine Ressource für die Textanalyse. Weitere Funktionen:

* Rufen Sie einen [Testschlüssel](https://azure.microsoft.com/try/cognitive-services/#decision) ab, mit dem Sie sieben Tage lang kostenlos testen können. Nach der Registrierung steht dieser auf der [Azure-Website](https://azure.microsoft.com/try/cognitive-services/my-apis/) zur Verfügung.
* Zeigen Sie Ihre Ressource im [Azure-Portal](https://portal.azure.com) an.

Nachdem Sie einen Schlüssel für Ihr Testabonnement bzw. Ihre Ressource erhalten haben, [erstellen Sie eine Umgebungsvariable](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) für den Schlüssel (`TEXT_ANALYTICS_SUBSCRIPTION_KEY`).

### <a name="create-a-new-go-project"></a>Erstellen eines neuen Go-Projekts

Erstellen Sie in einem Konsolenfenster (cmd, PowerShell, Terminal, Bash) einen neuen Arbeitsbereich für Ihr Go-Projekt, und navigieren Sie zu diesem Projekt. Ihr Arbeitsbereich enthält drei Ordner: 

* **src**: Dieses Verzeichnis enthält Quellcode und Pakete. Alle Pakete, die mit dem Befehl `go get` installiert werden, befinden sich in diesem Verzeichnis.
* **pkg**: Dieses Verzeichnis enthält die kompilierten Go-Paketobjekte. Diese Dateien haben alle die Erweiterung `.a`.
* **bin**: Dieses Verzeichnis enthält die ausführbaren Binärdateien, die beim Ausführen von `go install` erstellt werden.

> [!TIP]
> Informieren Sie sich über die Struktur eines [Go-Arbeitsbereichs](https://golang.org/doc/code.html#Workspaces). Dieses Handbuch enthält Informationen zum Festlegen von `$GOPATH` und `$GOROOT`.

Erstellen Sie einen Arbeitsbereich mit dem Namen `my-app` und den erforderlichen Unterverzeichnissen für `src`, `pkg` und `bin`:

```console
$ mkdir -p my-app/{src, bin, pkg}  
$ cd my-app
```

### <a name="install-the-text-analytics-client-library-for-go"></a>Installieren der Textanalyse-Clientbibliothek für Go

Installieren Sie die Clientbibliothek für Go: 

```console
$ go get -u <https://github.com/Azure/azure-sdk-for-go/tree/master/services/cognitiveservices/v2.1/textanalytics>
```

Bei Verwendung von DEP können Sie alternativ Folgendes in Ihrem Repository ausführen:

```console
$ dep ensure -add <https://github.com/Azure/azure-sdk-for-go/tree/master/services/cognitiveservices/v2.1/textanalytics>
```

### <a name="create-your-go-application"></a>Erstellen Ihrer Go-Anwendung

Erstellen Sie als Nächstes eine Datei mit dem Namen `src/quickstart.go`:

```bash
$ cd src
$ touch quickstart.go
```

Öffnen Sie `quickstart.go` in einer IDE oder einem Text-Editor Ihrer Wahl. Fügen Sie dann den Paketnamen hinzu, und importieren Sie die folgenden Bibliotheken:

```golang
import (
    "context"
    "encoding/json"
    "fmt"
    "github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.1/textanalytics"
    "github.com/Azure/go-autorest/autorest"
    "log"
    "os"    
)
```

Fügen Sie Ihrem Projekt die folgenden Funktionen hinzu, da der Großteil der Parameter und Eigenschaften für diese Schnellstartanleitung Zeiger vom Typ „string“ und „bool“ erwartet.

```golang
// returns a pointer to the string value passed in.
func StringPointer(v string) *string {
    return &v
}

// returns a pointer to the bool value passed in.
func BoolPointer(v bool) *bool {
    return &v
}
```

Erstellen Sie in der Funktion `main` Ihrer Anwendung Variablen für den Azure-Endpunkt und den Abonnementschlüssel Ihrer Ressource. Rufen Sie diese Werte aus den Umgebungsvariablen „TEXT_ANALYTICS_SUBSCRIPTION_KEY“ und „TEXT_ANALYTICS_ENDPOINT“ ab. Wenn Sie diese Umgebungsvariablen erstellt haben, nachdem Sie mit der Bearbeitung der Anwendung begonnen haben, muss der Editor, die IDE oder die Shell, den bzw. die Sie für den Zugriff auf die Variablen verwenden, geschlossen und erneut geöffnet werden.

[!INCLUDE [text-analytics-find-resource-information](../includes/find-azure-resource-info.md)]

```golang
var subscriptionKeyVar string = "TEXT_ANALYTICS_SUBSCRIPTION_KEY"
if "" == os.Getenv(subscriptionKeyVar) {
    log.Fatal("Please set/export the environment variable " + subscriptionKeyVar + ".")
}
var subscriptionKey string = os.Getenv(subscriptionKeyVar)
var endpointVar string = "TEXT_ANALYTICS_ENDPOINT"
if "" == os.Getenv(endpointVar) {
    log.Fatal("Please set/export the environment variable " + endpointVar + ".")
}
var endpoint string = os.Getenv(endpointVar)
```

## <a name="object-model"></a>Objektmodell 

Der Textanalyse-Client ist ein [BaseClient](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.1/textanalytics#New)-Objekt, für das die Authentifizierung für Azure mit Ihrem Schlüssel durchgeführt wird. Der Client verfügt über verschiedene Methoden zum Analysieren von Text als einzelne Zeichenfolge oder als Batch. 

Text wird als `documents`-Liste an die API gesendet. Hierbei handelt es sich um `dictionary`-Objekte, die je nach verwendeter Methode eine Kombination aus den Attributen `id`, `text` und `language` enthalten. Das `text`-Attribut speichert den zu analysierenden Text in der Ausgangssprache (`language`), und `id` kann ein beliebiger Wert sein. 

Das Antwortobjekt ist eine Liste mit den Analyseinformationen für jedes Dokument. 

## <a name="code-examples"></a>Codebeispiele

Mit den Codeausschnitten wird veranschaulicht, wie die folgenden Vorgänge mit der Textanalyse-Clientbibliothek für Python durchgeführt werden:

* [Authentifizieren des Clients](#authenticate-the-client)
* [Standpunktanalyse](#sentiment-analysis)
* [Sprachenerkennung](#language-detection)
* [Entitätserkennung](#entity-recognition)
* [Schlüsselbegriffserkennung](#key-phrase-extraction)

## <a name="authenticate-the-client"></a>Authentifizieren des Clients

Erstellen Sie in der main-Funktion Ihres Projekts ein neues [BaseClient](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.1/textanalytics#New)-Objekt. Übergeben Sie Ihren Schlüssel an die Funktion [autorest.NewCognitiveServicesAuthorizer()](https://godoc.org/github.com/Azure/go-autorest/autorest#NewCognitiveServicesAuthorizer), die dann an die `authorizer`-Eigenschaft des Clients übergeben wird.

```golang
textAnalyticsClient := textanalytics.New(endpoint)
textAnalyticsClient.Authorizer = autorest.NewCognitiveServicesAuthorizer(subscriptionKey)
```

## <a name="sentiment-analysis"></a>Stimmungsanalyse

Erstellen Sie eine neue Funktion mit dem Namen `SentimentAnalysis()`, die den zuvor erstellten Client aufnimmt. Erstellen Sie eine Liste mit [MultiLanguageInput](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.1/textanalytics#MultiLanguageBatchInput)-Objekten, die die zu analysierenden Dokumente enthält. Jedes Objekt enthält die Attribute `id`, `Language` und `text`. Das `text`-Attribut speichert den Text, der analysiert werden soll, `language` ist die Sprache des Dokuments, und `id` kann ein beliebiger Wert sein. 

```golang
func SentimentAnalysis(textAnalyticsclient textanalytics.BaseClient) {

    ctx := context.Background()
    inputDocuments := []textanalytics.MultiLanguageInput {
        textanalytics.MultiLanguageInput {
            Language: StringPointer("en"),
            ID:StringPointer("0"),
            Text:StringPointer("I had the best day of my life."),
        },
    }

    batchInput := textanalytics.MultiLanguageBatchInput{Documents:&inputDocuments}
}
```

Rufen Sie in derselben Funktion die Funktion [Sentiment()](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.1/textanalytics#BaseClient.Sentiment) des Clients und das Ergebnis ab. Durchlaufen Sie dann die Ergebnisse, und drucken Sie die ID und den Stimmungswert jedes Dokuments. Eine gegen 0 tendierende Punktzahl deutet auf eine negative Stimmung hin, eine gegen 1 tendierende Punktzahl auf eine positive.

```golang
result, err := textAnalyticsclient.Sentiment(ctx, BoolPointer(false), &batchInput)
if err != nil { log.Fatal(err) }

batchResult := textanalytics.SentimentBatchResult{}
jsonString, _ := json.Marshal(result.Value)
json.Unmarshal(jsonString, &batchResult)

// Printing sentiment results
for _,document := range *batchResult.Documents {
    fmt.Printf("Document ID: %s\n", *document.ID)
    fmt.Printf("Sentiment Score: %f\n",*document.Score)
}

// Printing document errors
fmt.Println("Document Errors:")
for _,error := range *batchResult.Errors {
    fmt.Printf("Document ID: %s Message : %s\n" ,*error.ID, *error.Message)
}
fmt.Println()
```

Rufen Sie in der main-Funktion Ihres Projekts `SentimentAnalysis()` auf.

### <a name="output"></a>Output

```console
Document ID: 1 , Sentiment Score: 0.87
```

## <a name="language-detection"></a>Spracherkennung

Erstellen Sie eine neue Funktion mit dem Namen `LanguageDetection()`, die den zuvor erstellten Client aufnimmt. Erstellen Sie eine Liste mit [LanguageInput](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.1/textanalytics#LanguageInput)-Objekten, die die zu analysierenden Dokumente enthalten. Jedes Objekt enthält die Attribute `id` und `text`. Das `text`-Attribut speichert den Text, der analysiert werden soll, und die `id` kann ein beliebiger Wert sein. 

```golang
func LanguageDetection(textAnalyticsclient textanalytics.BaseClient) {

    ctx := context.Background()
    inputDocuments := []textanalytics.LanguageInput {
        textanalytics.LanguageInput {
            ID:StringPointer("0"),
            Text:StringPointer("This is a document written in English."),
        },
    }

    batchInput := textanalytics.LanguageBatchInput{Documents:&inputDocuments}
}
```

Rufen Sie in derselben Funktion das [DetectLanguage()](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.1/textanalytics#BaseClient.DetectLanguage)-Objekt des Clients und das Ergebnis ab. Durchlaufen Sie dann die Ergebnisse, und geben Sie für jedes Dokument die ID und die ermittelte Sprache aus.

```golang
result, err := textAnalyticsclient.DetectLanguage(ctx, BoolPointer(false), &batchInput)
if err != nil { log.Fatal(err) }

// Printing language detection results
for _,document := range *result.Documents {
    fmt.Printf("Document ID: %s " , *document.ID)
    fmt.Printf("Detected Languages with Score: ")
    for _,language := range *document.DetectedLanguages{
        fmt.Printf("%s %f,",*language.Name, *language.Score)
    }
    fmt.Println()
}

// Printing document errors
fmt.Println("Document Errors:")
for _,error := range *result.Errors {
    fmt.Printf("Document ID: %s Message : %s\n" ,*error.ID, *error.Message)
}
fmt.Println()
```

Rufen Sie in der main-Funktion Ihres Projekts `LanguageDetection()` auf.

### <a name="output"></a>Output

```console
Document ID: 0 Detected Languages with Score: English 1.000000
```

## <a name="entity-recognition"></a>Entitätserkennung

Erstellen Sie eine neue Funktion mit dem Namen `ExtractEntities()`, die den zuvor erstellten Client aufnimmt. Erstellen Sie eine Liste mit [MultiLanguageInput](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.1/textanalytics#MultiLanguageBatchInput)-Objekten, die die zu analysierenden Dokumente enthalten. Jedes Objekt enthält die Attribute `id`, `language` und `text`. Das `text`-Attribut speichert den Text, der analysiert werden soll, `language` ist die Sprache des Dokuments, und `id` kann ein beliebiger Wert sein. 

```golang
func ExtractEntities(textAnalyticsclient textanalytics.BaseClient) {

    ctx := context.Background()
    inputDocuments := []textanalytics.MultiLanguageInput {
        textanalytics.MultiLanguageInput {
            Language: StringPointer("en"),
            ID:StringPointer("0"),
            Text:StringPointer("Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975, to develop and sell BASIC interpreters for the Altair 8800."),
        },
    }

    batchInput := textanalytics.MultiLanguageBatchInput{Documents:&inputDocuments}
}
```

Rufen Sie in derselben Funktion das [Entities()](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.1/textanalytics#BaseClient.Entities)-Objekt des Clients und das Ergebnis ab. Durchlaufen Sie dann die Ergebnisse, und geben Sie für jedes Dokument die ID und die Bewertung der extrahierten Entitäten aus.

```golang
    result, err := textAnalyticsclient.Entities(ctx, BoolPointer(false), &batchInput)
    if err != nil { log.Fatal(err) }

    // Printing extracted entities results
    for _,document := range *result.Documents {
        fmt.Printf("Document ID: %s\n" , *document.ID)
        fmt.Printf("\tExtracted Entities:\n")
        for _,entity := range *document.Entities{
            fmt.Printf("\t\tName: %s\tType: %s",*entity.Name, *entity.Type)
            if entity.SubType != nil{
                fmt.Printf("\tSub-Type: %s\n", *entity.SubType)
            }
            fmt.Println()
            for _,match := range *entity.Matches{
                fmt.Printf("\t\t\tOffset: %v\tLength: %v\tScore: %f\n", *match.Offset, *match.Length, *match.EntityTypeScore)
            }
        }
        fmt.Println()
    }

    // Printing document errors
    fmt.Println("Document Errors:")
    for _,error := range *result.Errors {
        fmt.Printf("Document ID: %s Message : %s\n" ,*error.ID, *error.Message)
    }
    fmt.Println()
```

Rufen Sie in der main-Funktion Ihres Projekts `ExtractEntities()` auf.

### <a name="output"></a>Output

```console
Document ID: 0
    Extracted Entities:
        Name: Microsoft Type: Organization
            Offset: 0   Length: 9   Score: 1.000000
        Name: Bill Gates    Type: Person
            Offset: 25  Length: 10  Score: 0.999847
        Name: Paul Allen    Type: Person
            Offset: 40  Length: 10  Score: 0.998841
        Name: April 4   Type: Other
            Offset: 54  Length: 7   Score: 0.800000
        Name: April 4, 1975 Type: DateTime  Sub-Type: Date

            Offset: 54  Length: 13  Score: 0.800000
        Name: BASIC Type: Other
            Offset: 89  Length: 5   Score: 0.800000
        Name: Altair 8800   Type: Other
            Offset: 116 Length: 11  Score: 0.800000
```

## <a name="key-phrase-extraction"></a>Schlüsselwortextraktion

Erstellen Sie eine neue Funktion mit dem Namen `ExtractKeyPhrases()`, die den zuvor erstellten Client aufnimmt. Erstellen Sie eine Liste mit [MultiLanguageInput](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.1/textanalytics#MultiLanguageBatchInput)-Objekten, die die zu analysierenden Dokumente enthalten. Jedes Objekt enthält die Attribute `id`, `language` und `text`. Das `text`-Attribut speichert den Text, der analysiert werden soll, `language` ist die Sprache des Dokuments, und `id` kann ein beliebiger Wert sein.

```golang
func ExtractKeyPhrases(textAnalyticsclient textanalytics.BaseClient) {

    ctx := context.Background()
    inputDocuments := []textanalytics.MultiLanguageInput {
        textanalytics.MultiLanguageInput {
            Language: StringPointer("en"),
            ID:StringPointer("0"),
            Text:StringPointer("My cat might need to see a veterinarian."),
        },
    }

    batchInput := textanalytics.MultiLanguageBatchInput{Documents:&inputDocuments}
}
```

Rufen Sie in derselben Funktion das [KeyPhrases()](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.1/textanalytics#BaseClient.KeyPhrases)-Objekt des Clients und das Ergebnis ab. Durchlaufen Sie dann die Ergebnisse, und geben Sie für jedes Dokument die ID und die extrahierten Schlüsselbegriffe aus.

```golang
    result, err := textAnalyticsclient.KeyPhrases(ctx, BoolPointer(false), &batchInput)
    if err != nil { log.Fatal(err) }

    // Printing extracted key phrases results
    for _,document := range *result.Documents {
        fmt.Printf("Document ID: %s\n" , *document.ID)
        fmt.Printf("\tExtracted Key Phrases:\n")
        for _,keyPhrase := range *document.KeyPhrases{
            fmt.Printf("\t\t%s\n",keyPhrase)
        }
        fmt.Println()
    }

    // Printing document errors
    fmt.Println("Document Errors:")
    for _,error := range *result.Errors {
        fmt.Printf("Document ID: %s Message : %s\n" ,*error.ID, *error.Message)
    }
    fmt.Println()
```

Rufen Sie in der main-Funktion Ihres Projekts `ExtractKeyPhrases()` auf.

### <a name="output"></a>Output

```console
Document ID: 0
    Extracted Key Phrases:
        cat
        veterinarian
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie ein Cognitive Services-Abonnement bereinigen und entfernen möchten, können Sie die Ressource oder die Ressourcengruppe löschen. Wenn Sie die Ressourcengruppe löschen, werden auch alle anderen Ressourcen gelöscht, die der Ressourcengruppe zugeordnet sind.

* [Portal](../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure-Befehlszeilenschnittstelle](../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>Nächste Schritte


> [!div class="nextstepaction"]
> [Textanalyse mit Power BI](../tutorials/tutorial-power-bi-key-phrases.md)


* [Übersicht über die Textanalyse](../overview.md)
* [Standpunktanalyse](../how-tos/text-analytics-how-to-sentiment-analysis.md)
* [Entitätserkennung](../how-tos/text-analytics-how-to-entity-linking.md)
* [Sprache erkennen](../how-tos/text-analytics-how-to-keyword-extraction.md)
* [Spracherkennung](../how-tos/text-analytics-how-to-language-detection.md)
* Den Quellcode für dieses Beispiel finden Sie auf [GitHub](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/cognitiveservices).
