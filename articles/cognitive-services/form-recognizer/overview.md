---
title: Was ist die Formularerkennung?
titleSuffix: Azure Cognitive Services
description: Die Formularerkennung von Azure Cognitive Services ermöglicht Ihnen das Identifizieren und Extrahieren von Schlüssel/Wert-Paaren und Tabellendaten aus Formulardokumenten.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: overview
ms.date: 07/25/2019
ms.author: pafarley
ms.openlocfilehash: 86e0494ded84e9711b2283ae5fad77ff626cf792
ms.sourcegitcommit: bc193bc4df4b85d3f05538b5e7274df2138a4574
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2019
ms.locfileid: "73904484"
---
# <a name="what-is-form-recognizer"></a>Was ist die Formularerkennung?

Die Azure-Formularerkennung ist ein kognitiver Dienst, der mithilfe von Machine Learning-Technologie Schlüssel/Wert-Paare und Tabellendaten aus Formulardokumenten identifiziert und extrahiert. Anschließend werden strukturierte Daten ausgegeben, die die Beziehungen in der Originaldatei enthalten. Sie können Ihr benutzerdefiniertes Formularerkennungsmodell mithilfe einer einfachen REST-API aufrufen, um die Komplexität zu reduzieren und es einfach in Ihren Workflow oder Ihre Anwendung zu integrieren. Um zu beginnen, benötigen Sie nur fünf ausgefüllte Formulardokumente oder zwei ausgefüllte Formulare plus ein leeres Formular desselben Typs wie Ihr Eingabematerial. Sie können schnell präzise und auf Ihre spezifischen Inhalte zugeschnittene Ergebnisse erzielen, ohne dass komplizierte manuelle Eingriffe oder umfangreiche Data Science-Kenntnisse erforderlich sind.

## <a name="custom-models"></a>Benutzerdefinierte Modelle

Das benutzerdefinierte Formularerkennungsmodell wird mit Ihren eigenen Daten trainiert, und Sie benötigen zu Beginn nur fünf Beispieleingabeformulare. Anhand der übermittelten Eingabedaten gruppiert der Algorithmus die Formulare nach Typ, erkennt, welche Schlüssel und Tabellen vorhanden sind, und ordnet Schlüsseln Werte und Tabellen Einträge zu. Anschließend werden strukturierte Daten ausgegeben, die die Beziehungen in der Originaldatei enthalten. Nachdem das Modell trainiert wurde, können Sie es testen, neu trainieren und schließlich verwenden, um Daten aus weiteren Formularen zuverlässig nach Ihren Bedürfnissen zu extrahieren.

Unbeaufsichtigtes Lernen ermöglicht es dem Modell, das Layout und die Beziehungen zwischen Feldern und Einträgen zu verstehen, und zwar ohne manuelle Datenbeschriftungen oder intensive Codierung und Verwaltung. Vortrainierte Machine Learning-Modelle erfordern dagegen standardisierte Daten. Sie sind weniger genau beim Eingabematerial, das von herkömmlichen Formaten wie branchenspezifischen Formularen abweicht.

## <a name="prebuilt-receipt-model"></a>Vordefiniertes Belegmodell

Die Formularerkennung enthält auch ein Modell für das Lesen von Verkaufsbelegen. Dieses Modell extrahiert wichtige Informationen wie Zeitpunkt und Datum der Transaktion, Händlerinformationen, Steuer- und Summenbeträge und mehr. Darüber hinaus wird das vorgefertigte Belegmodell dazu trainiert, den gesamten Text eines Belegs zu erkennen und zurückzugeben.

## <a name="what-it-includes"></a>Lieferumfang

Die Formularerkennung ist als eine REST-API verfügbar. Sie können ein benutzerdefiniertes Modell erstellen, trainieren und bewerten oder auf das vordefinierte Modell zugreifen, indem Sie diese APIs aufrufen. Wenn Sie möchten, können Sie benutzerdefinierte Modelle in einem lokalen Docker-Container trainieren und ausführen.

## <a name="input-requirements"></a>Eingabeanforderungen
### <a name="custom-model"></a>Benutzerdefiniertes Modell

[!INCLUDE [input requirements](./includes/input-requirements.md)]

### <a name="prebuilt-receipt-model"></a>Vordefiniertes Belegmodell

Die Eingabeanforderungen für das Belegmodell unterscheiden sich geringfügig.

* Als Format muss JPEG, PNG, BMP, PDF (Text oder gescannt) oder TIFF verwendet werden.
* Die Dateigröße muss weniger als 20 MB betragen.
* Bei Bildern müssen die Abmessungen zwischen 50 × 50 Pixel und 10.000 × 10.000 Pixel liegen. 
* Die Abmessungen bei PDFs dürfen maximal 17 x 17 Zoll betragen. Dies entspricht den Papiergrößen Legal oder A3 und kleineren Formaten.
* Bei PDF- und TIFF-Dateien werden nur die ersten 200 Seiten verarbeitet. (Bei einem Abonnement im Free-Tarif werden nur die ersten beiden Seiten verarbeitet.)

## <a name="request-access"></a>Zugriff anfordern

Die Formularerkennung ist als Vorschauversion mit eingeschränktem Zugriff verfügbar. Um Zugriff auf die Vorschauversion zu erhalten, füllen Sie das [Formular zum Anfordern des Zugriffs auf die Formularerkennung](https://aka.ms/FormRecognizerRequestAccess) aus, und übermitteln Sie es. Das Formular fordert Informationen über Sie, Ihr Unternehmen und das Benutzerszenario an, für das Sie den Form Recognizer verwenden werden. Wenn Ihre Anforderung vom Azure Cognitive Services-Team genehmigt wird, erhalten Sie eine E-Mail mit Anweisungen für den Zugriff auf den Dienst.

## <a name="where-do-i-start"></a>Wo beginne ich?

**Schritt 1:** Erstellen Sie eine Formularerkennungsressource im Azure-Portal.

**Schritt 2:** Befolgen Sie einen Schnellstart zum Verwenden der REST-API:
* [Schnellstart: Trainieren eines Modells zur Formularerkennung und Extrahieren von Formulardaten unter Verwendung der REST-API mit cURL](quickstarts/curl-train-extract.md)
* [Schnellstart: Trainieren eines Modells zur Formularerkennung und Extrahieren von Formulardaten unter Verwendung der REST-API mit Python](quickstarts/python-train-extract.md)
* [Schnellstart: Extrahieren von Verkaufsbelegdaten mithilfe von cURL](quickstarts/curl-receipts.md)
* [Schnellstart: Extrahieren von Verkaufsbelegdaten mithilfe von Python](quickstarts/python-receipts.md)

Sie sollten den kostenlosen Dienst nutzen, wenn Sie die Technologie erlernen. Bedenken Sie, dass die Anzahl der kostenlosen Seiten auf 500 pro Monat beschränkt ist.

**Schritt 3:** Überprüfen der REST-APIs

Verwenden Sie die folgenden APIs zum Trainieren und Extrahieren strukturierter Daten aus Formularen.

|||
|---|---|
| Trainieren eines Modells| Trainieren eines neuen Modells zur Analyse Ihrer Formulare mit fünf Formularen gleichen Typs. Oder Trainieren mit einem leeren Formular und zwei ausgefüllten Formularen.  |
| Analysieren des Formulars |Analysieren eines einzelnen Dokuments, das als Stream übergeben wird, um Schlüssel/Wert-Paare und Tabellen aus dem Formular mit Ihrem benutzerdefinierten Modell zu extrahieren.  |
| Analysieren des Belegs |Analysieren eines einzelnen Belegdokuments, um wichtige Informationen und anderen Belegtext zu extrahieren.|

Sehen Sie sich die [Referenzdokumentation zur REST-API](https://aka.ms/form-recognizer/api) an, um mehr zu erfahren. 

## <a name="data-privacy-and-security"></a>Datenschutz und Sicherheit

Dieser Dienst wird als [Vorschauversion](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) eines Azure-Diensts unter den [Nutzungsbedingungen für Onlinedienste](https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) bereitgestellt. Wie bei allen Cognitive Services-Diensten müssen Entwickler, die den Formularerkennungsdienst nutzen, die Microsoft-Richtlinien zu Kundendaten beachten. Weitere Informationen finden Sie im Microsoft Trust Center auf der [Seite zu Cognitive Services](https://www.microsoft.com/trustcenter/cloudservices/cognitiveservices).

## <a name="next-steps"></a>Nächste Schritte

Absolvieren Sie einen [Schnellstart](quickstarts/curl-train-extract.md) zu den ersten Schritten mit den [Formularerkennungs-APIs](https://aka.ms/form-recognizer/api).