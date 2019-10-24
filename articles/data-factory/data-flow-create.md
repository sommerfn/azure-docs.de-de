---
title: Erstellen eines Datenflusses mit Azure Data Factory Mapping Data Flow
description: Erfahren Sie, wie Sie einen Datenfluss mit Azure Data Factory Mapping Data Flow erstellen.
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/12/2019
ms.openlocfilehash: 31528c0ac7796aafbde74a3814d971138ec4ba2e
ms.sourcegitcommit: bb65043d5e49b8af94bba0e96c36796987f5a2be
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2019
ms.locfileid: "72387244"
---
# <a name="create-azure-data-factory-data-flow"></a>Erstellen eines Azure Data Factory-Datenflusses



Mit Mapping Data Flow in ADF können Sie Daten nach Maß transformieren, ohne Code schreiben zu müssen. Sie können einen Datentransformationsauftrag im Datenfluss-Designer entwerfen, indem Sie eine Reihe von Transformationen erstellen. Beginnen Sie mit einer beliebigen Anzahl von Quelltransformationen, gefolgt von Datentransformationsschritten. Vervollständigen Sie dann Ihren Datenfluss mit einer Senke, damit Ihre Ergebnisse an ein Ziel gelangen.

Erstellen Sie zuerst im Azure-Portal eine neue Data Factory (V2). Klicken Sie nach dem Erstellen der neuen Factory auf die Kachel „Erstellen und überwachen“, um die Data Factory-Benutzeroberfläche zu starten.

![Datenflussoptionen](media/data-flow/v2portal.png "Erstellen eines Datenflusses")

Sobald Sie sich auf der Data Factory-Benutzeroberfläche befinden, können Sie die Beispieldatenflüsse verwenden. Die Beispiele sind im ADF-Vorlagenkatalog verfügbar. Erstellen Sie in ADF eine „Pipeline aus Vorlage“, und wählen Sie im Vorlagenkatalog die Kategorie „Datenfluss“ aus.

![Datenflussoptionen](media/data-flow/template.png "Erstellen eines Datenflusses")

Sie werden zur Eingabe Ihrer Azure Blob Storage-Kontoinformationen aufgefordert.

![Datenflussoptionen](media/data-flow/template2.png "Erstellen eines Datenflusses 2")

[Die für diese Beispiele verwendeten Daten finden Sie hier](https://github.com/kromerm/adfdataflowdocs/tree/master/sampledata). Laden Sie die Beispieldaten herunter, und speichern Sie die Dateien in Ihren Azure Blob Storage-Konten, damit Sie die Beispiele ausführen können.

## <a name="create-new-data-flow"></a>Erstellen des neuen Datenflusses

Verwenden Sie auf der ADF-Benutzeroberfläche die Schaltfläche „Ressource erstellen“ mit dem Pluszeichen (+), um Datenflüsse zu erstellen.

![Datenflussoptionen](media/data-flow/newresource.png "Neue Ressource")

## <a name="next-steps"></a>Nächste Schritte

Beginnen Sie die Erstellung Ihrer Datentransformation mit einer [Quelltransformation](data-flow-source.md).
