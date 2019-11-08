---
title: 'TSV-Format für Batchtests: QnA Maker'
titleSuffix: Azure Cognitive Services
description: Grundlagen zum TSV-Format für Batchtests
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: reference
ms.date: 10/24/2019
ms.author: diberry
ms.openlocfilehash: dccf034b6497651db70b6cc09fff3f1b1d645a1b
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73506801"
---
# <a name="batch-testing-tsv-format"></a>TSV-Format für Batchtests

Batchtests sind als [Quellcode](https://github.com/Azure-Samples/cognitive-services-qnamaker-csharp/tree/master/documentation-samples/batchtesting) oder als [gezippte ausführbare Datei zum Herunterladen](https://aka.ms/qna_btzip) verfügbar. Das Format des Befehls zum Ausführen des Batchtests sieht wie folgt aus:

```console
batchtesting.exe input.tsv https://YOUR-HOST.azurewebsites.net ENDPOINT-KEY out.tsv
```

|Parameter|Erwarteter Wert|
|--|--|
|1|Name der TSV-Datei, mit [TSV-Eingabefeldern](#tsv-input-fields) formatiert|
|2|URI für den Endpunkt, mit YOUR-HOST (IHR HOST) von der Seite „Veröffentlichen“ des QnA Maker-Portals.|
|3|ENDPOINT-KEY (ENDPUNKTSCHLÜSSEL), zu finden auf der Seite „Veröffentlichen“ des QnA Maker-Portals.|
|4|Der Name der von den Batchtests für Ergebnisse erstellten TSV-Datei.|

Verwenden Sie die folgenden Informationen, um das TSV-Format für Batchtests zu verstehen und zu implementieren. 

## <a name="tsv-input-fields"></a>TSV-Eingabefelder

|Felder der TSV-Eingabedatei|Notizen|
|--|--|
|KBID|Ihre KB-ID, wie sie sich auf der Seite „Veröffentlichen“ findet.|
|Frage|Die Frage, die ein Benutzer eingeben würde.|
|Metadatentags|optional|
|Oberster Parameter|optional| 
|Erwartete Antwort-ID|optional|

![Eingabeformat für die TSV-Datei für Batch Tests.](media/batch-test/input-tsv-format-batch-test.png)

## <a name="tsv-output-fields"></a>TSV-Ausgabefelder 

|Parameter der TSV-Ausgabedatei|Notizen|
|--|--|
|KBID|Ihre KB-ID, wie sie sich auf der Seite „Veröffentlichen“ findet.|
|Frage|Die Frage, wie sie aus der Eingabedatei eingegeben wird.|
|Antwort|Oberste Antwort aus Ihrer Wissensdatenbank.|
|Answer ID|Antwort-ID|
|Punkte|Vorhersagepunktzahl für die Antwort. |
|Metadatentags|Der zurückgegebenen Antwort zugeordnet|
|Erwartete Antwort-ID|optional (nur wenn die erwartete Antwort-ID angegeben ist)|
|Urteilsbezeichnung|Optional, es können diese Werte angenommen werden: „richtig“ oder „falsch“ (nur, wenn die erwartete Antwort angegeben ist)|
