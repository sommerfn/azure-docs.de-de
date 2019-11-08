---
title: 'Beispiel: Erstellen einer benutzerdefinierten kognitiven Qualifikation mit der Bing-Entitätssuche-API'
titleSuffix: Azure Cognitive Search
description: Hier erfahren Sie, wie Sie den Bing-Entitätssuche-Dienst in einer benutzerdefinierten Qualifikation verwenden, die einer Indizierungspipeline mit KI-Anreicherung in der kognitiven Azure-Suche zugeordnet ist.
manager: nitinme
author: luiscabrer
ms.author: luisca
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 1f134ac360b6c5bd04c0e141da52b6dad950e208
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73466819"
---
# <a name="example-create-a-custom-skill-using-the-bing-entity-search-api"></a>Beispiel: Erstellen einer benutzerdefinierten Qualifikation mit der Bing-Entitätssuche-API

In diesem Beispiel erfahren Sie, wie Sie eine benutzerdefinierte Qualifikation einer Web-API erstellen. Diese Qualifikation akzeptiert Orte, Personen des öffentlichen Lebens und Organisationen und gibt Beschreibungen für diese zurück. In dem Beispiel wird eine [Azure-Funktion](https://azure.microsoft.com/services/functions/) verwendet, um die [Bing-Entitätssuche-API](https://azure.microsoft.com/services/cognitive-services/bing-entity-search-api/) so einzubinden, dass sie die Schnittstelle der benutzerdefinierten Qualifikation implementiert.

## <a name="prerequisites"></a>Voraussetzungen

+ Lesen Sie den Artikel über die [Schnittstelle für benutzerdefinierte Qualifikationen](cognitive-search-custom-skill-interface.md), wenn Sie nicht mit der Eingabe/Ausgabe-Schnittstelle vertraut sind, die eine benutzerdefinierte Qualifikation implementieren sollte.

+ [!INCLUDE [cognitive-services-bing-entity-search-signup-requirements](../../includes/cognitive-services-bing-entity-search-signup-requirements.md)]

+ Installieren Sie [Visual Studio 2019](https://www.visualstudio.com/vs/) oder höher zusammen mit der Workload für die Azure-Entwicklung.

## <a name="create-an-azure-function"></a>Erstellen einer Azure Function

Obwohl in diesem Beispiel eine Azure-Funktion zum Hosten einer Web-API verwendet wird, ist dies nicht erforderlich.  Solange Sie die [Schnittstellenanforderungen für einen kognitiven Skill](cognitive-search-custom-skill-interface.md) erfüllen, ist der gewählte Ansatz unerheblich. Mit Azure Functions ist das Erstellen eines benutzerdefinierten Skills jedoch einfacher.

### <a name="create-a-function-app"></a>Erstellen einer Funktionen-App

1. Wählen Sie in Visual Studio im Menü „Datei“ die Optionen **Neu** > **Projekt** aus.

1. Wählen Sie im Dialogfeld „Neues Projekt“ die Option **Installiert**, erweitern Sie **Visual C#**  > **Cloud**, und wählen Sie **Azure Functions** aus. Geben Sie unter „Name“ einen Namen für Ihr Projekt ein, und wählen Sie **OK**. Der Name der Funktions-App muss als C#-Namespace gültig sein. Verwenden Sie daher keine Unterstriche, Bindestriche oder andere nicht alphanumerische Zeichen.

1. Wählen Sie **Azure Functions v2 (.NET Core)** aus. Sie können dies auch mit Version 1 erreichen, aber der nachfolgende Code basiert auf der Vorlage der Version 2.

1. Wählen Sie für den Typ **HTTP-Trigger** aus.

1. Für das Speicherkonto können Sie **Kein** auswählen, da Sie für diese Funktion keinen Speicher benötigen.

1. Wählen Sie **OK**, um das Funktionsprojekt und die per HTTP ausgelöste Funktion zu erstellen.

### <a name="modify-the-code-to-call-the-bing-entity-search-service"></a>Ändern des Codes zum Aufrufen des Bing-Entitätssuche-Diensts

Visual Studio erstellt ein Projekt, das eine Klasse mit den Codebausteinen für den gewählten Funktionstyp enthält. Mit dem *FunctionName*-Attribut der Methode wird der Name der Funktion festgelegt. Mit dem *HttpTrigger*-Attribut wird angegeben, dass die Funktion mit einer HTTP-Anforderung ausgelöst wird.

Ersetzen Sie alle Inhalte der Datei *Function1.cs* nun durch den folgenden Code:

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Net.Http;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.AspNetCore.Http;
using Microsoft.Extensions.Logging;
using Newtonsoft.Json;

namespace SampleSkills
{
    /// <summary>
    /// Sample custom skill that wraps the Bing entity search API to connect it with a 
    /// AI enrichment pipeline.
    /// </summary>
    public static class BingEntitySearch
    {
        #region Credentials
        // IMPORTANT: Make sure to enter your credential and to verify the API endpoint matches yours.
        static readonly string bingApiEndpoint = "https://api.cognitive.microsoft.com/bing/v7.0/entities/";
        static readonly string key = "<enter your api key here>";  
        #endregion

        #region Class used to deserialize the request
        private class InputRecord
        {
            public class InputRecordData
            {
                public string Name { get; set; }
            }

            public string RecordId { get; set; }
            public InputRecordData Data { get; set; }
        }

        private class WebApiRequest
        {
            public List<InputRecord> Values { get; set; }
        }
        #endregion

        #region Classes used to serialize the response

        private class OutputRecord
        {
            public class OutputRecordData
            {
                public string Name { get; set; } = "";
                public string Description { get; set; } = "";
                public string Source { get; set; } = "";
                public string SourceUrl { get; set; } = "";
                public string LicenseAttribution { get; set; } = "";
                public string LicenseUrl { get; set; } = "";
            }

            public class OutputRecordMessage
            {
                public string Message { get; set; }
            }

            public string RecordId { get; set; }
            public OutputRecordData Data { get; set; }
            public List<OutputRecordMessage> Errors { get; set; }
            public List<OutputRecordMessage> Warnings { get; set; }
        }

        private class WebApiResponse
        {
            public List<OutputRecord> Values { get; set; }
        }
        #endregion

        #region Classes used to interact with the Bing API
        private class BingResponse
        {
            public BingEntities Entities { get; set; }
        }
        private class BingEntities
        {
            public BingEntity[] Value { get; set; }
        }

        private class BingEntity
        {
            public class EntityPresentationinfo
            {
                public string[] EntityTypeHints { get; set; }
            }

            public class License
            {
                public string Url { get; set; }
            }

            public class ContractualRule
            {
                public string _type { get; set; }
                public License License { get; set; }
                public string LicenseNotice { get; set; }
                public string Text { get; set; }
                public string Url { get; set; }
            }

            public ContractualRule[] ContractualRules { get; set; }
            public string Description { get; set; }
            public string Name { get; set; }
            public EntityPresentationinfo EntityPresentationInfo { get; set; }
        }
        #endregion

        #region The Azure Function definition

        [FunctionName("EntitySearch")]
        public static async Task<IActionResult> Run(
            [HttpTrigger(AuthorizationLevel.Function, "post", Route = null)] HttpRequest req,
            ILogger log)
        {
            log.LogInformation("Entity Search function: C# HTTP trigger function processed a request.");

            var response = new WebApiResponse
            {
                Values = new List<OutputRecord>()
            };

            string requestBody = new StreamReader(req.Body).ReadToEnd();
            var data = JsonConvert.DeserializeObject<WebApiRequest>(requestBody);

            // Do some schema validation
            if (data == null)
            {
                return new BadRequestObjectResult("The request schema does not match expected schema.");
            }
            if (data.Values == null)
            {
                return new BadRequestObjectResult("The request schema does not match expected schema. Could not find values array.");
            }

            // Calculate the response for each value.
            foreach (var record in data.Values)
            {
                if (record == null || record.RecordId == null) continue;

                OutputRecord responseRecord = new OutputRecord
                {
                    RecordId = record.RecordId
                };

                try
                {
                    responseRecord.Data = GetEntityMetadata(record.Data.Name).Result;
                }
                catch (Exception e)
                {
                    // Something bad happened, log the issue.
                    var error = new OutputRecord.OutputRecordMessage
                    {
                        Message = e.Message
                    };

                    responseRecord.Errors = new List<OutputRecord.OutputRecordMessage>
                    {
                        error
                    };
                }
                finally
                {
                    response.Values.Add(responseRecord);
                }
            }

            return (ActionResult)new OkObjectResult(response);
        }

        #endregion

        #region Methods to call the Bing API
        /// <summary>
        /// Gets metadata for a particular entity based on its name using Bing Entity Search
        /// </summary>
        /// <param name="entityName">The name of the entity to extract data for.</param>
        /// <returns>Asynchronous task that returns entity data. </returns>
        private async static Task<OutputRecord.OutputRecordData> GetEntityMetadata(string entityName)
        {
            var uri = bingApiEndpoint + "?q=" + entityName + "&mkt=en-us&count=10&offset=0&safesearch=Moderate";
            var result = new OutputRecord.OutputRecordData();

            using (var client = new HttpClient())
            using (var request = new HttpRequestMessage {
                Method = HttpMethod.Get,
                RequestUri = new Uri(uri)
            })
            {
                request.Headers.Add("Ocp-Apim-Subscription-Key", key);

                HttpResponseMessage response = await client.SendAsync(request);
                string responseBody = await response?.Content?.ReadAsStringAsync();

                BingResponse bingResult = JsonConvert.DeserializeObject<BingResponse>(responseBody);
                if (bingResult != null)
                {
                    // In addition to the list of entities that could match the name, for simplicity let's return information
                    // for the top match as additional metadata at the root object.
                    return AddTopEntityMetadata(bingResult.Entities?.Value);
                }
            }

            return result;
        }

        private static OutputRecord.OutputRecordData AddTopEntityMetadata(BingEntity[] entities)
        {
            if (entities != null)
            {
                foreach (BingEntity entity in entities.Where(
                    entity => entity?.EntityPresentationInfo?.EntityTypeHints != null
                        && (entity.EntityPresentationInfo.EntityTypeHints[0] == "Person"
                            || entity.EntityPresentationInfo.EntityTypeHints[0] == "Organization"
                            || entity.EntityPresentationInfo.EntityTypeHints[0] == "Location")
                        && !String.IsNullOrEmpty(entity.Description)))
                {
                    var rootObject = new OutputRecord.OutputRecordData
                    {
                        Description = entity.Description,
                        Name = entity.Name
                    };

                    if (entity.ContractualRules != null)
                    {
                        foreach (var rule in entity.ContractualRules)
                        {
                            switch (rule._type)
                            {
                                case "ContractualRules/LicenseAttribution":
                                    rootObject.LicenseAttribution = rule.LicenseNotice;
                                    rootObject.LicenseUrl = rule.License.Url;
                                    break;
                                case "ContractualRules/LinkAttribution":
                                    rootObject.Source = rule.Text;
                                    rootObject.SourceUrl = rule.Url;
                                    break;
                            }
                        }
                    }

                    return rootObject;
                }
            }

            return new OutputRecord.OutputRecordData();
        }
        #endregion
    }
}
```

Wichtig: Geben Sie in der Konstanten `key` *Ihren* eigenen Schlüsselwert ein (auf der Grundlage des Schlüssels, den Sie bei der Anmeldung für die Bing-Entitätssuche-API erhalten haben).

Der Einfachheit halber ist der gesamte erforderliche Code in diesem Beispiel in einer einzelnen Datei enthalten. Eine strukturiertere Version der gleichen Qualifikation finden Sie im Repository [azure-search-power-skills](https://github.com/Azure-Samples/azure-search-power-skills/tree/master/Text/BingEntitySearch).

Sie können die Datei natürlich von `Function1.cs` in `BingEntitySearch.cs` umbenennen.

## <a name="test-the-function-from-visual-studio"></a>Testen der Funktion aus Visual Studio

Drücken Sie **F5**, um das Programm- und Testfunktionsverhalten auszuführen. In diesem Fall verwenden wir die folgende Funktion, um nach zwei Entitäten zu suchen. Verwenden Sie Postman oder Fiddler, um einen Aufruf wie den unten gezeigten auszugeben:

```http
POST https://localhost:7071/api/EntitySearch
```

### <a name="request-body"></a>Anforderungstext
```json
{
    "values": [
        {
            "recordId": "e1",
            "data":
            {
                "name":  "Pablo Picasso"
            }
        },
        {
            "recordId": "e2",
            "data":
            {
                "name":  "Microsoft"
            }
        }
    ]
}
```

### <a name="response"></a>response
Die Antwort sollte in etwa wie hier dargestellt aussehen:

```json
{
    "values": [
        {
            "recordId": "e1",
            "data": {
                "name": "Pablo Picasso",
                "description": "Pablo Ruiz Picasso was a Spanish painter [...]",
                "source": "Wikipedia",
                "sourceUrl": "http://en.wikipedia.org/wiki/Pablo_Picasso",
                "licenseAttribution": "Text under CC-BY-SA license",
                "licenseUrl": "http://creativecommons.org/licenses/by-sa/3.0/"
            },
            "errors": null,
            "warnings": null
        },
        "..."
    ]
}
```

## <a name="publish-the-function-to-azure"></a>Veröffentlichen der Funktion in Azure

Wenn Sie mit dem Funktionsverhalten zufrieden sind, können Sie sie veröffentlichen.

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, und wählen Sie **Veröffentlichen** aus. Wählen Sie **Neu erstellen** > **Veröffentlichen**.

1. Wenn Sie Visual Studio noch nicht mit Ihrem Azure-Konto verbunden haben, wählen Sie **Konto hinzufügen...** aus.

1. Befolgen Sie die Anweisungen auf dem Bildschirm. Sie werden zur Angabe eines eindeutigen Namens für Ihren App-Dienst sowie des Azure-Abonnements, der Ressourcengruppe, des Hostingplans und des gewünschten Speicherkontos aufgefordert. Sie können eine neue Ressourcengruppe, einen neuen Hostingplan und ein Speicherkonto erstellen, wenn Sie noch nicht über diese verfügen. Wenn Sie fertig sind, klicken Sie auf **Erstellen**.

1. Beachten Sie die Website-URL nach Abschluss der Bereitstellung. Es ist die Adresse der Funktions-App in Azure. 

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zur Ressourcengruppe, und suchen Sie nach der von Ihnen veröffentlichten Funktion `EntitySearch`. Im Abschnitt **Verwalten** sollten Sie Hostschlüssel angezeigt werden. Wählen Sie das Symbol **Kopieren** für den Hostschlüssel *Standard* aus.  

## <a name="test-the-function-in-azure"></a>Testen der Funktion in Azure

Nachdem Sie nun den Standardhostschlüssel haben, testen Sie Ihre Funktion wie folgt:

```http
POST https://[your-entity-search-app-name].azurewebsites.net/api/EntitySearch?code=[enter default host key here]
```

### <a name="request-body"></a>Anforderungstext
```json
{
    "values": [
        {
            "recordId": "e1",
            "data":
            {
                "name":  "Pablo Picasso"
            }
        },
        {
            "recordId": "e2",
            "data":
            {
                "name":  "Microsoft"
            }
        }
    ]
}
```

Dieses Beispiel sollte das gleiche Ergebnis liefern wie die Ausführung der Funktion in der lokalen Umgebung.

## <a name="connect-to-your-pipeline"></a>Herstellen einer Verbindung mit Ihrer Pipeline
Nun, da Sie über einen neuen benutzerdefinierten Skill verfügen, können Sie diesen zu Ihrem Skillset hinzufügen. Das folgende Beispiel zeigt, wie Sie die Qualifikation aufrufen, um Beschreibungen zu Organisationen im Dokument hinzuzufügen. (Das Beispiel könnte auch auf Orte und Personen ausgedehnt werden.) Ersetzen Sie `[your-entity-search-app-name]` durch den Namen Ihrer App.

```json
{
    "skills": [
      "[... your existing skills remain here]",  
      {
        "@odata.type": "#Microsoft.Skills.Custom.WebApiSkill",
        "description": "Our new Bing entity search custom skill",
        "uri": "https://[your-entity-search-app-name].azurewebsites.net/api/EntitySearch?code=[enter default host key here]",
          "context": "/document/merged_content/organizations/*",
          "inputs": [
            {
              "name": "name",
              "source": "/document/merged_content/organizations/*"
            }
          ],
          "outputs": [
            {
              "name": "description",
              "targetName": "description"
            }
          ]
      }
  ]
}
```

Hier wird vorausgesetzt, dass die integrierte [Qualifikation „Entitätserkennung“](cognitive-search-skill-entity-recognition.md) im Skillset vorhanden ist und das Dokument mit der Liste der Organisationen angereichert wurde. Zu Referenzzwecken sehen Sie hier eine Konfiguration der Qualifikation „Entitätsextraktion“, die zum Generieren der benötigten Daten ausreichend wäre:

```json
{
    "@odata.type": "#Microsoft.Skills.Text.EntityRecognitionSkill",
    "name": "#1",
    "description": "Organization name extraction",
    "context": "/document/merged_content",
    "categories": [ "Organization" ],
    "defaultLanguageCode": "en",
    "inputs": [
        {
            "name": "text",
            "source": "/document/merged_content"
        },
        {
            "name": "languageCode",
            "source": "/document/language"
        }
    ],
    "outputs": [
        {
            "name": "organizations",
            "targetName": "organizations"
        }
    ]
},
```

## <a name="next-steps"></a>Nächste Schritte
Glückwunsch! Sie haben Ihre erste benutzerdefinierte Qualifikation erstellt. Nun können Sie Ihre eigene benutzerdefinierte Funktionalität nach dem gleichen Muster hinzufügen. Klicken Sie auf die folgenden Links, um weitere Informationen zu erhalten.

+ [Power Skills: ein Repository benutzerdefinierter Skills](https://github.com/Azure-Samples/azure-search-power-skills)
+ [Hinzufügen einer benutzerdefinierten Qualifikation zu einer KI-Anreicherungspipeline](cognitive-search-custom-skill-interface.md)
+ [Definieren eines Skillsets](cognitive-search-defining-skillset.md)
+ [Erstellen eines Skillsets (REST)](https://docs.microsoft.com/rest/api/searchservice/create-skillset)
+ [Zuordnen angereicherter Felder](cognitive-search-output-field-mapping.md)
