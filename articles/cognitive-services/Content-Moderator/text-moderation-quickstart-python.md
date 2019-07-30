---
title: 'Schnellstart: Analysieren von Textinhalten in Python – Content Moderator'
titlesuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie Textinhalte mit dem Content Moderator SDK für Python auf verschiedene anstößige Inhalte analysieren.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: quickstart
ms.date: 07/03/2019
ms.author: pafarley
ms.openlocfilehash: bb0e44f83e2101a7b21e7b7ec6fdc75974c6d6d8
ms.sourcegitcommit: e9c866e9dad4588f3a361ca6e2888aeef208fc35
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/19/2019
ms.locfileid: "68333608"
---
# <a name="quickstart-analyze-text-content-for-objectionable-material-in-python"></a>Schnellstart: Analysieren von Text auf anstößige Inhalte in Python

Dieser Artikel enthält Informationen und Codebeispiele, die Ihnen den Einstieg in die Verwendung des Content Moderator SDK für Python erleichtern. Sie erfahren, wie Sie eine begriffsbasierte Filterung und Klassifizierung von Textinhalten ausführen, um potenziell anstößige Inhalte zu moderieren.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen. 

## <a name="prerequisites"></a>Voraussetzungen
- Ein Content Moderator-Abonnementschlüssel. Gehen Sie wie unter [Schnellstart: Erstellen eines Cognitive Services-Kontos im Azure-Portal](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) beschrieben vor, um Content Moderator zu abonnieren und Ihren Schlüssel zu erhalten.
- [Python 2.7+ oder 3.5+](https://www.python.org/downloads/)
- [Pip](https://pip.pypa.io/en/stable/installing/)-Tool

## <a name="get-the-content-moderator-sdk"></a>Installieren des Content Moderator SDK

Installieren Sie das Content Moderator Python SDK, indem Sie eine Eingabeaufforderung öffnen und den folgenden Befehl ausführen:

```bash
pip install azure-cognitiveservices-vision-contentmoderator
```

## <a name="import-modules"></a>Importieren von Modulen

Erstellen Sie ein neues Python-Skript mit dem Namen _ContentModeratorQS.py_, und fügen Sie den folgenden Code hinzu, um die erforderlichen Teile des SDK zu importieren. Damit die JSON-Antwort einfacher gelesen werden kann, ist das Modul für Schöndruck enthalten.

[!code-python[](~/cognitive-services-content-moderator-samples/documentation-samples/python/content_moderator_quickstart.py?name=imports)]


## <a name="initialize-variables"></a>Initialisieren der Variablen

Fügen Sie als Nächstes Variablen für Ihren Content Moderator-Abonnementschlüssel und Ihre Endpunkt-URL hinzu. Fügen Sie Ihren Umgebungsvariablen den Namen `CONTENT_MODERATOR_SUBSCRIPTION_KEY` mit Ihrem Abonnementschlüssel als Wert hinzu. Fügen Sie Ihren Umgebungsvariablen als Basisendpunkt-URL `CONTENT_MODERATOR_ENDPOINT` hinzu, und geben Sie als Wert Ihre regionsspezifische URL an, etwa `https://westus.api.cognitive.microsoft.com`. Abonnementschlüssel für kostenlose Testversionen werden in der Region **westus** generiert.

[!code-python[](~/cognitive-services-content-moderator-samples/documentation-samples/python/content_moderator_quickstart.py?name=authentication)]

Eine Zeichenfolge mit mehrzeiligem Text aus einer Datei wird moderiert. Fügen Sie die Datei [content_moderator_text_moderation.txt](https://github.com/Azure-Samples/cognitive-services-content-moderator-samples/blob/master/documentation-samples/python/content_moderator_text_moderation.txt) in Ihren lokalen Stammordner ein, und fügen Sie den Dateinamen zu Ihren Variablen hinzu:

[!code-python[](~/cognitive-services-content-moderator-samples/documentation-samples/python/content_moderator_quickstart.py?name=textModerationFile)]

## <a name="query-the-moderator-service"></a>Abfragen des Moderator-Diensts

Erstellen Sie mit Ihrem Abonnementschlüssel und Ihrer Endpunkt-URL eine **ContentModeratorClient**-Instanz. 

[!code-python[](~/cognitive-services-content-moderator-samples/documentation-samples/python/content_moderator_quickstart.py?name=client)]

Rufen Sie dann mithilfe Ihres Clients mit der **TextModerationOperations**-Memberinstanz die Moderations-API mit der Funktion `screen_text` auf. Weitere Informationen zum Aufrufen der API finden Sie in der Referenzdokumentation zu **[screen_text](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-contentmoderator/azure.cognitiveservices.vision.contentmoderator.operations.textmoderationoperations?view=azure-python)** .

[!code-python[](~/cognitive-services-content-moderator-samples/documentation-samples/python/content_moderator_quickstart.py?name=textModeration)]

## <a name="check-the-printed-response"></a>Überprüfen der ausgegebenen Antwort

Führen Sie das Beispiel aus, und überprüfen Sie die Antwort. Nach erfolgreichem Abschluss wird eine Instanz vom Typ **Screen** zurückgegeben. Nachfolgend sehen Sie ein erfolgreiches Ergebnis:

```console
{'auto_corrected_text': '" Is this a garbage email abide@ abed. com, phone: '
                        '6657789887, IP: 255. 255. 255. 255, 1 Microsoft Way, '
                        'Redmond, WA 98052. Crap is the profanity here. Is '
                        'this information PII? phone 3144444444\\ n"',
 'classification': {'category1': {'score': 0.00025233393535017967},
                    'category2': {'score': 0.18468093872070312},
                    'category3': {'score': 0.9879999756813049},
                    'review_recommended': True},
 'language': 'eng',
 'normalized_text': '" Is this a garbage email abide@ abed. com, phone: '
                    '6657789887, IP: 255. 255. 255. 255, 1 Microsoft Way, '
                    'Redmond, WA 98052. Crap is the profanity here. Is this '
                    'information PII? phone 3144444444\\ n"',
 'original_text': '"Is this a grabage email abcdef@abcd.com, phone: '
                  '6657789887, IP: 255.255.255.255, 1 Microsoft Way, Redmond, '
                  'WA 98052. Crap is the profanity here. Is this information '
                  'PII? phone 3144444444\\n"',
 'pii': {'address': [{'index': 82,
                      'text': '1 Microsoft Way, Redmond, WA 98052'}],
         'email': [{'detected': 'abcdef@abcd.com',
                    'index': 25,
                    'sub_type': 'Regular',
                    'text': 'abcdef@abcd.com'}],
         'ipa': [{'index': 65, 'sub_type': 'IPV4', 'text': '255.255.255.255'}],
         'phone': [{'country_code': 'US', 'index': 49, 'text': '6657789887'},
                   {'country_code': 'US', 'index': 177, 'text': '3144444444'}]},
 'status': {'code': 3000, 'description': 'OK'},
 'terms': [{'index': 118, 'list_id': 0, 'original_index': 118, 'term': 'crap'}],
 'tracking_id': 'b253515c-e713-4316-a016-8397662a3f1a'}
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie ein einfaches Python-Skript erstellt, das unter Verwendung des Content Moderator-Diensts relevante Informationen zu einem bestimmten Textbeispiel zurückgibt. Informieren Sie sich als Nächstes über die Bedeutung der verschiedenen Flags und Klassifizierungen, um entscheiden zu können, welche Daten Sie benötigen und wie Ihre App mit ihnen umgehen soll.

> [!div class="nextstepaction"]
> [Textmoderation](text-moderation-api.md)
