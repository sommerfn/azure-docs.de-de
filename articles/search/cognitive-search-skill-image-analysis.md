---
title: Die Qualifikation „Bildanalyse“ der kognitiven Suche – Azure Search
description: Extrahieren Sie semantischen Text durch Bildanalyse mithilfe des kognitiven Skills „Bildanalyse“ in einer Azure Search-Anreicherungspipeline.
services: search
manager: nitinme
author: luiscabrer
ms.service: search
ms.workload: search
ms.topic: conceptual
ms.date: 08/28/2019
ms.author: luisca
ms.openlocfilehash: e2c8f0519ffcbdbc2445d1fed2725b6f6b948cd1
ms.sourcegitcommit: f7f70c9bd6c2253860e346245d6e2d8a85e8a91b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73064069"
---
#   <a name="image-analysis-cognitive-skill"></a>Bildanalyse – kognitiver Skill

Der Skill **Bildanalyse** extrahiert einen umfangreichen Satz von visuellen Merkmalen aus dem Bildinhalt. So können Sie beispielsweise anhand eines Bilds eine Beschriftung erstellen, Tags generieren oder Prominente und Sehenswürdigkeiten identifizieren. Diese Qualifikation verwendet die durch [Maschinelles Sehen](https://docs.microsoft.com/azure/cognitive-services/computer-vision/home) in Cognitive Services bereitgestellten Machine Learning-Modelle. 

> [!NOTE]
> Kleine Volumes (unter 20 Transaktionen) können in Azure Search kostenlos ausgeführt werden, größere Workloads erfordern jedoch das [Anfügen einer abrechnungsfähigen Cognitive Services-Ressource](cognitive-search-attach-cognitive-services.md). Gebühren fallen beim Aufrufen von APIs in Cognitive Services sowie für die Bildextraktion im Rahmen der Dokumentaufschlüsselungsphase in Azure Search an. Für die Textextraktion aus Dokumenten fallen keine Gebühren an.
>
> Die Ausführung integrierter Qualifikationen wird nach dem bestehenden [nutzungsbasierten Preis für Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services/) berechnet. Die Preise für die Bildextraktion werden auf der [Preisseite von Azure Search](https://go.microsoft.com/fwlink/?linkid=2042400) beschrieben.


## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Vision.ImageAnalysisSkill 

## <a name="skill-parameters"></a>Skillparameter

Bei den Parametern wird zwischen Groß- und Kleinschreibung unterschieden.

| Parametername     | BESCHREIBUNG |
|--------------------|-------------|
| defaultLanguageCode   |  Eine Zeichenfolge, die angibt, welche Sprache zurückgegeben werden soll. Der Dienst gibt die Ergebnisse der Erkennung in einer bestimmten Sprache zurück. Wenn dieser Parameter nicht angegeben wird, ist der Standardwert „en“. <br/><br/>Unterstützte Sprachen: <br/>*en*: Englisch (Standard) <br/> *zh*: vereinfachtes Chinesisch|
|visualFeatures |   Ein Array aus Zeichenfolgen, die angibt, welche Arten von visuellen Merkmalen zurückgegeben werden sollen. Folgende Arten von visuellen Merkmalen sind gültig:  <ul><li> *categories:* kategorisiert Bildinhalte gemäß einer Taxonomie, die in der [Dokumentation zum maschinellen Sehen](https://docs.microsoft.com/azure/cognitive-services/computer-vision/category-taxonomy) in Cognitive Services definiert ist. </li><li> *tags*: Erstellt Tags für das Bild in einer detaillierten Liste aus Wörtern, die sich auf den Bildinhalt beziehen.</li><li>*description*: Beschreibt den Bildinhalt mit einem vollständigen Satz.</li><li>*faces*: Erkennt, ob Gesichter vorhanden sind. Wenn Gesichter vorhanden sind, generiert dieses Merkmal Informationen zu Koordinaten, Geschlecht und Alter.</li><li>    *imageType*: Erkennt, ob das Bild ein pixelbasiertes ClipArt oder eine linienbasierte Zeichnung ist.</li><li>  *color*: Bestimmt die Akzentfarbe und die dominante Farbe und ermittelt, ob ein Bild schwarzweiß ist.</li><li>*adult*: Erkennt, ob das Bild pornografische Natur ist (erkennt Nacktheit oder sexuelle Handlungen). Zweideutige und freizügige Inhalte werden ebenfalls erkannt.</li></ul> Bei den Namen der visuellen Merkmale wird die Groß- und Kleinschreibung beachtet.|
| details   | Ein Array aus Zeichenfolgen, die angeben, welche domänenspezifischen Informationen zurückgegeben werden sollen. Folgende Arten von visuellen Merkmalen sind gültig: <ul><li>*celebrities*: Identifiziert Prominente in einem Bild.</li><li>*landmarks*: Identifiziert Sehenswürdigkeiten in einem Bild. </li></ul> |

## <a name="skill-inputs"></a>Skilleingaben

| Eingabename      | BESCHREIBUNG                                          |
|---------------|------------------------------------------------------|
| image         | Komplexer Typ. Arbeitet derzeit mit dem Feld „/document/normalized_images“, das vom Azure Blob-Indexer generiert wird, wenn ```imageAction``` auf einen anderen Wert als ```none``` gesetzt ist. Weitere Informationen finden Sie im [Beispiel](#sample-output).|



##  <a name="sample-skill-definition"></a>Beispieldefinition einer Qualifikation

```json
        {
            "description": "Extract image analysis.",
            "@odata.type": "#Microsoft.Skills.Vision.ImageAnalysisSkill",
            "context": "/document/normalized_images/*",
            "defaultLanguageCode": "en",
            "visualFeatures": [
                "Tags",
                "Categories",
                "Description",
                "Faces"
            ],
            "inputs": [
                {
                    "name": "image",
                    "source": "/document/normalized_images/*"
                }
            ],
            "outputs": [
                {
                    "name": "categories"
                },
                {
                    "name": "tags"
                },
                {
                    "name": "description"
                },
                {
                    "name": "faces"
                }
            ]
        }
```
### <a name="sample-index-for-only-the-categories-description-faces-and-tags-fields"></a>Beispielindex (nur für die Felder „Kategorien“, „Beschreibung „, „Gesichter“ und „Tags“)
```json
{
    "fields": [
        {
            "name": "id",
            "type": "Edm.String",
            "key": true,
            "searchable": true,
            "filterable": false,
            "facetable": false,
            "sortable": true
        },
        {
            "name": "blob_uri",
            "type": "Edm.String",
            "searchable": true,
            "filterable": false,
            "facetable": false,
            "sortable": true
        },
        {
            "name": "content",
            "type": "Edm.String",
            "sortable": false,
            "searchable": true,
            "filterable": false,
            "facetable": false
        },
        {
            "name": "categories",
            "type": "Collection(Edm.ComplexType)",
            "fields": [
                {
                    "name": "name",
                    "type": "Edm.String",
                    "searchable": true,
                    "filterable": false,
                    "facetable": false
                },
                {
                    "name": "score",
                    "type": "Edm.Double",
                    "searchable": false,
                    "filterable": false,
                    "facetable": false
                },
                {
                    "name": "detail",
                    "type": "Edm.ComplexType",
                    "fields": [
                        {
                            "name": "celebrities",
                            "type": "Collection(Edm.ComplexType)",
                            "fields": [
                                {
                                    "name": "name",
                                    "type": "Edm.String",
                                    "searchable": true,
                                    "filterable": false,
                                    "facetable": false
                                },
                                {
                                    "name": "faceBoundingBox",
                                    "type": "Collection(Edm.ComplexType)",
                                    "fields": [
                                        {
                                            "name": "x",
                                            "type": "Edm.Int32",
                                            "searchable": false,
                                            "filterable": false,
                                            "facetable": false
                                        },
                                        {
                                            "name": "y",
                                            "type": "Edm.Int32",
                                            "searchable": false,
                                            "filterable": false,
                                            "facetable": false
                                        }
                                    ]
                                },
                                {
                                    "name": "confidence",
                                    "type": "Edm.Double",
                                    "searchable": false,
                                    "filterable": false,
                                    "facetable": false
                                }
                            ]
                        },
                        {
                            "name": "landmarks",
                            "type": "Collection(Edm.ComplexType)",
                            "fields": [
                                {
                                    "name": "name",
                                    "type": "Edm.String",
                                    "searchable": true,
                                    "filterable": false,
                                    "facetable": false
                                },
                                {
                                    "name": "confidence",
                                    "type": "Edm.Double",
                                    "searchable": false,
                                    "filterable": false,
                                    "facetable": false
                                }
                            ]
                        }
                    ]
                }
            ]
        },
        {
            "name": "description",
            "type": "Collection(Edm.ComplexType)",
            "fields": [
                {
                    "name": "tags",
                    "type": "Collection(Edm.String)",
                    "searchable": true,
                    "filterable": false,
                    "facetable": false
                },
                {
                    "name": "captions",
                    "type": "Collection(Edm.ComplexType)",
                    "fields": [
                        {
                            "name": "text",
                            "type": "Edm.String",
                            "searchable": true,
                            "filterable": false,
                            "facetable": false
                        },
                        {
                            "name": "confidence",
                            "type": "Edm.Double",
                            "searchable": false,
                            "filterable": false,
                            "facetable": false
                        }
                    ]
                }
            ]
        },
        {
            "name": "faces",
            "type": "Collection(Edm.ComplexType)",
            "fields": [
                {
                    "name": "age",
                    "type": "Edm.Int32",
                    "searchable": false,
                    "filterable": false,
                    "facetable": false
                },
                {
                    "name": "gender",
                    "type": "Edm.String",
                    "searchable": false,
                    "filterable": false,
                    "facetable": false
                },
                {
                    "name": "faceBoundingBox",
                    "type": "Collection(Edm.ComplexType)",
                    "fields": [
                        {
                            "name": "x",
                            "type": "Edm.Int32",
                            "searchable": false,
                            "filterable": false,
                            "facetable": false
                        },
                        {
                            "name": "y",
                            "type": "Edm.Int32",
                            "searchable": false,
                            "filterable": false,
                            "facetable": false
                        }
                    ]
                }
            ]
        },
        {
            "name": "tags",
            "type": "Collection(Edm.ComplexType)",
            "fields": [
                {
                    "name": "name",
                    "type": "Edm.String",
                    "searchable": true,
                    "filterable": false,
                    "facetable": false
                },
                {
                    "name": "confidence",
                    "type": "Edm.Double",
                    "searchable": false,
                    "filterable": false,
                    "facetable": false
                }
            ]
        }
    ]
}

```
### <a name="sample-output-field-mapping-for-the-above-index"></a>Beispielausgabefeldzuordnung (für den oben genannten Index)
```json
    "outputFieldMappings": [
        {
            "sourceFieldName": "/document/normalized_images/*/categories/*",
            "targetFieldName": "categories"
        },
        {
            "sourceFieldName": "/document/normalized_images/*/tags/*",
            "targetFieldName": "tags"
        },
        {
            "sourceFieldName": "/document/normalized_images/*/description",
            "targetFieldName": "description"
        },
        {
            "sourceFieldName": "/document/normalized_images/*/faces/*",
            "targetFieldName": "faces"
        }
```
### <a name="variation-on-output-field-mappings-nested-properties"></a>Variation von Ausgabefeldzuordnungen (verschachtelte Eigenschaften)

Sie können Ausgabefeldzuordnungen zu Eigenschaften niedrigerer Ebenen definieren, z. B. als einfache Wahrzeichen oder Prominente. Stellen Sie in diesem Fall sicher, dass Ihr Indexschema ein spezifisches Feld für Wahrzeichen aufweist.

```json
    "outputFieldMappings": [
        {
            "sourceFieldName": "/document/normalized_images/*/categories/detail/celebrities/*",
            "targetFieldName": "celebrities"
        }
```
##  <a name="sample-input"></a>Beispieleingabe

```json
{
    "values": [
        {
            "recordId": "1",
            "data": {
                "image": {
                    "data": "BASE64 ENCODED STRING OF A JPEG IMAGE",
                    "width": 500,
                    "height": 300,
                    "originalWidth": 5000,
                    "originalHeight": 3000,
                    "rotationFromOriginal": 90,
                    "contentOffset": 500,
                    "pageNumber": 2
                }
            }
        }
    ]
}
```

##  <a name="sample-output"></a>Beispielausgabe

```json
{
    "values": [
        {
            "recordId": "1",
            "data": {
                "categories": [
                    {
                        "name": "abstract_",
                        "score": 0.00390625
                    },
                    {
                        "name": "people_",
                        "score": 0.83984375,
                        "detail": {
                            "celebrities": [
                                {
                                    "name": "Satya Nadella",
                                    "faceBoundingBox": [
                                        {
                                            "x": 273,
                                            "y": 309
                                        },
                                        {
                                            "x": 395,
                                            "y": 309
                                        },
                                        {
                                            "x": 395,
                                            "y": 431
                                        },
                                        {
                                            "x": 273,
                                            "y": 431
                                        }
                                    ],
                                    "confidence": 0.999028444
                                }
                            ],
                            "landmarks": [
                                {
                                    "name": "Forbidden City",
                                    "confidence": 0.9978346
                                }
                            ]
                        }
                    }
                ],
                "adult": {
                    "isAdultContent": false,
                    "isRacyContent": false,
                    "adultScore": 0.0934349000453949,
                    "racyScore": 0.068613491952419281
                },
                "tags": [
                    {
                        "name": "person",
                        "confidence": 0.98979085683822632
                    },
                    {
                        "name": "man",
                        "confidence": 0.94493889808654785
                    },
                    {
                        "name": "outdoor",
                        "confidence": 0.938492476940155
                    },
                    {
                        "name": "window",
                        "confidence": 0.89513939619064331
                    }
                ],
                "description": {
                    "tags": [
                        "person",
                        "man",
                        "outdoor",
                        "window",
                        "glasses"
                    ],
                    "captions": [
                        {
                            "text": "Satya Nadella sitting on a bench",
                            "confidence": 0.48293603002174407
                        }
                    ]
                },
                "requestId": "0dbec5ad-a3d3-4f7e-96b4-dfd57efe967d",
                "metadata": {
                    "width": 1500,
                    "height": 1000,
                    "format": "Jpeg"
                },
                "faces": [
                    {
                        "age": 44,
                        "gender": "Male",
                        "faceBoundingBox": [
                            {
                                "x": 1601,
                                "y": 395
                            },
                            {
                                "x": 1653,
                                "y": 395
                            },
                            {
                                "x": 1653,
                                "y": 447
                            },
                            {
                                "x": 1601,
                                "y": 447
                            }
                        ]
                    }
                ],
                "color": {
                    "dominantColorForeground": "Brown",
                    "dominantColorBackground": "Brown",
                    "dominantColors": [
                        "Brown",
                        "Black"
                    ],
                    "accentColor": "873B59",
                    "isBwImg": false
                    },
                "imageType": {
                    "clipArtType": 0,
                    "lineDrawingType": 0
                }
            }
        }
    ]
}
```


## <a name="error-cases"></a>Auftretende Fehler
In den folgenden Fällen werden keine Elemente extrahiert.

| Fehlercode | BESCHREIBUNG |
|------------|-------------|
| NotSupportedLanguage | Die angegebene Sprache wird nicht unterstützt. |
| InvalidImageUrl | Die Bild-URL ist falsch formatiert, oder es kann nicht darauf zugegriffen werden.|
| InvalidImageFormat | Bei den Eingabedaten handelt es sich nicht um ein gültiges Bild. |
| InvalidImageSize | Das Eingabebild ist zu groß. |
| NotSupportedVisualFeature  | Der angegebene Merkmaltyp ist ungültig. |
| NotSupportedImage | Nicht unterstütztes Bild, z.B. Kinderpornografie. |
| InvalidDetails | Nicht unterstütztes domänenspezifisches Modell. |

Wenn Sie eine Fehlermeldung ähnlich `"One or more skills are invalid. Details: Error in skill #<num>: Outputs are not supported by skill: Landmarks"` erhalten, überprüfen Sie den Pfad. Sowohl Prominente als auch Wahrzeichen sind Eigenschaften unter `detail`.

```json
"categories":[  
      {  
         "name":"building_",
         "score":0.97265625,
         "detail":{  
            "landmarks":[  
               {  
                  "name":"Forbidden City",
                  "confidence":0.92013400793075562
               }
            ]
```

## <a name="see-also"></a>Weitere Informationen

+ [Vordefinierte Skills](cognitive-search-predefined-skills.md)
+ [Definieren eines Skillsets](cognitive-search-defining-skillset.md)
+ [Erstellen eines Indexers (REST)](https://docs.microsoft.com/rest/api/searchservice/create-indexer)
