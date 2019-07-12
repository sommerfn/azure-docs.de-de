---
title: Was ist die Formularerkennung?
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie die Formularerkennung verwenden, um Formular- und Tabellendaten zu analysieren.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: overview
ms.date: 07/01/2019
ms.author: pafarley
ms.openlocfilehash: e064faf3017b95cb3a5f3d9b89f178fb7f846766
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2019
ms.locfileid: "67592635"
---
# <a name="what-is-form-recognizer"></a>Was ist die Formularerkennung?

Die Azure-Formularerkennung ist ein kognitiver Dienst, der mithilfe von Machine Learning-Technologie Schlüssel/Wert-Paare und Tabellendaten aus Formulardokumenten identifiziert und extrahiert. Anschließend werden strukturierte Daten ausgegeben, die die Beziehungen in der Originaldatei enthalten. Sie können Ihr benutzerdefiniertes Formularerkennungsmodell mithilfe einer einfachen REST-API aufrufen, um die Komplexität zu reduzieren und es einfach in Ihren Workflow oder Ihre Anwendung zu integrieren. Um zu beginnen, benötigen Sie nur fünf ausgefüllte Formulardokumente oder zwei ausgefüllte Formulare plus ein leeres Formular desselben Typs wie Ihr Eingabematerial. Sie können schnell präzise und auf Ihre spezifischen Inhalte zugeschnittene Ergebnisse erzielen, ohne dass komplizierte manuelle Eingriffe oder umfangreiche Data Science-Kenntnisse erforderlich sind.

## <a name="custom-models"></a>Benutzerdefinierte Modelle

Das benutzerdefinierte Formularerkennungsmodell wird mit Ihren eigenen Daten trainiert, und Sie benötigen zu Beginn nur fünf Beispieleingabeformulare. Anhand der übermittelten Eingabedaten gruppiert der Algorithmus die Formulare nach Typ, erkennt, welche Schlüssel und Tabellen vorhanden sind, und ordnet Schlüsseln Werte und Tabellen Einträge zu. Anschließend werden strukturierte Daten ausgegeben, die die Beziehungen in der Originaldatei enthalten. Nachdem das Modell trainiert wurde, können Sie es testen, neu trainieren und schließlich verwenden, um Daten aus weiteren Formularen zuverlässig nach Ihren Bedürfnissen zu extrahieren.

Nicht überwachtes Lernen ermöglicht es dem Modell, das Layout und die Beziehungen zwischen Feldern und Einträgen zu verstehen, und zwar ohne manuelle Datenbeschriftungen oder intensive Codierung und Verwaltung. Vortrainierte Machine Learning-Modelle erfordern dagegen standardisierte Daten. Sie sind weniger genau beim Eingabematerial, das von herkömmlichen Formaten wie branchenspezifischen Formularen abweicht.

## <a name="pre-built-receipt-model"></a>Vorgefertigtes Verkaufsbelegmodell

Die Formularerkennung enthält auch ein Modell für das Lesen von Verkaufsbelegen. Dieses Modell extrahiert wichtige Informationen wie Zeitpunkt und Datum der Transaktion, Händlerinformationen, Steuer- und Summenbeträge und mehr. Darüber hinaus wird das vorgefertigte Verkaufsbelegmodell dazu trainiert, den gesamten Text eines Belegs zu erkennen und zurückzugeben.

## <a name="what-it-includes"></a>Lieferumfang

Die Formularerkennung ist als eine REST-API verfügbar. Sie können ein benutzerdefiniertes Modell erstellen, trainieren und bewerten oder auf das vorkonfigurierte Modell zugreifen, indem Sie diese APIs aufrufen. Wenn Sie möchten, können Sie benutzerdefinierte Modelle in einem lokalen Docker-Container trainieren und ausführen.

## <a name="input-requirements-custom-model"></a>Eingabeanforderungen (benutzerdefiniertes Modell)

[!INCLUDE [input requirements](./includes/input-requirements.md)]

## <a name="request-access"></a>Zugriff anfordern

Die Formularerkennung ist als Vorschauversion mit eingeschränktem Zugriff verfügbar. Um Zugriff auf die Vorschauversion zu erhalten, füllen Sie das [Formular zum Anfordern des Zugriffs auf die Formularerkennung](https://aka.ms/FormRecognizerRequestAccess) aus, und übermitteln Sie es. Im Formular werden Informationen über Sie, Ihr Unternehmen und das Benutzerszenario angefordert, für das Sie die Formularerkennung verwenden möchten. Wenn Ihre Anforderung vom Azure Cognitive Services-Team genehmigt wird, erhalten Sie eine E-Mail mit Anweisungen für den Zugriff auf den Dienst.

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
