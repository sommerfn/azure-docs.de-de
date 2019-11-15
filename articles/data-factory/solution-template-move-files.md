---
title: Verschieben von Dateien zwischen dateibasiertem Speicher mithilfe von Azure Data Factory
description: Hier erfahren Sie, wie Sie mit Azure Data Factory eine Lösungsvorlage verwenden, um Dateien zwischen dateibasiertem Speicher zu verschieben.
services: data-factory
documentationcenter: ''
author: dearandyxu
ms.author: yexu
ms.reviewer: ''
manager: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 7/12/2019
ms.openlocfilehash: f6f83917e84a880fb86b5f592c4d51b03462753d
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73684094"
---
# <a name="move-files-with-azure-data-factory"></a>Verschieben von Dateien mit Azure Data Factory

In diesem Artikel wird eine Lösungsvorlage beschrieben, mit der Sie Dateien aus einem Ordner in einen anderen Ordner in einem anderen dateibasierten Speicher verschieben können. Eines der häufigsten Szenarien für die Verwendung dieser Vorlage: Dateien werden fortlaufend in einem Eingangsordner des Quellspeichers abgelegt. Durch das Erstellen eines geplanten Triggers kann die ADF-Pipeline diese Dateien in regelmäßigen Abständen aus dem Quell-in den Zielspeicher verschieben.  Die ADF-Pipeline verschiebt Dateien wie folgt: Die einzelnen Dateien im Eingangsordner werden in einen anderen Ordner im Zielspeicher kopiert und dann aus dem Eingangsordner im Quellspeicher gelöscht.

> [!NOTE]
> Beachten Sie, dass diese Vorlage zum Verschieben von Dateien und nicht zum Verschieben von Ordnern vorgesehen ist.  Lassen Sie Vorsicht walten, wenn Sie den Ordner verschieben möchten, indem Sie das Dataset ändern, damit es nur einen Ordnerpfad enthält, und anschließend eine Copy- und eine Delete-Aktivität verwenden, um auf das gleiche Dataset zu verweisen, das einen Ordner darstellt. In diesem Fall müssen Sie sicherstellen, dass zwischen dem Kopiervorgang und dem Löschvorgang KEINE neuen Dateien im Ordner platziert werden. Wenn neue Dateien zu dem Zeitpunkt im Ordner platziert werden, zu dem die Copy-Aktivität gerade abgeschlossen, die Delete-Aktivität jedoch noch nicht gestartet wurde, werden diese neuen Dateien, die noch NICHT ins Ziel kopiert wurden, unter Umständen von der Delete-Aktivität durch Löschung des gesamten Ordners gelöscht.

## <a name="about-this-solution-template"></a>Informationen zu dieser Lösungsvorlage

Mit dieser Vorlage werden die Dateien aus dem Quelldateispeicher abgerufen. Anschließend werden die einzelnen Dateien in den Zielspeicher verschoben.

Die Vorlage enthält fünf Aktivitäten:
- **GetMetadata** ruft die Liste der Objekte, die die Dateien und Unterordner enthalten, aus dem Ordner im Quellspeicher ab. Die Objekte werden nicht rekursiv abgerufen. 
- **Filter** filtert die Liste der Objekte aus der **GetMetadata**-Aktivität, um nur die Dateien auszuwählen. 
- **ForEach** ruft die Dateiliste aus der **Filter**-Aktivität ab, durchläuft die Liste und übergibt jede Datei an die Copy-Aktivität und die Delete-Aktivität.
- **Copy** kopiert eine Datei aus dem Quellspeicher in den Zielspeicher.
- **Delete** löscht dieselbe Datei aus dem Quellspeicher.

Die Vorlage definiert zwei Parameter:
- *FolderPath_SourceStore* stellt den Ordnerpfad des Quellspeichers dar, aus dem Dateien verschoben werden sollen. 
- *FolderPath_DestinationStore* stellt den Ordnerpfad des Zielspeichers dar, in den Dateien verschoben werden sollen. 

## <a name="how-to-use-this-solution-template"></a>So verwenden Sie diese Lösungsvorlage

1. Wechseln Sie zur Vorlage **Dateien verschieben**. Wählen Sie eine vorhandene Verbindung aus, oder erstellen Sie mit **Neu** eine neue Verbindung mit dem Quelldateispeicher, aus dem Dateien verschoben werden sollen. Beachten Sie, dass **DataSource_Folder** und **DataSource_File** auf dieselbe Verbindung der Quelldatei verweisen.

    ![Erstellen einer neuen Verbindung mit der Quelle](media/solution-template-move-files/move-files1.png)

2. Wählen Sie eine vorhandene Verbindung aus, oder erstellen Sie mit **Neu** eine neue Verbindung mit dem Zieldateispeicher, in den Dateien verschoben werden sollen.

    ![Erstellen einer neuen Verbindung mit dem Ziel](media/solution-template-move-files/move-files2.png)

3. Klicken Sie auf **Diese Vorlage verwenden**.

    ![„Diese Vorlage verwenden“](media/solution-template-move-files/move-files3.png)
    
4. Daraufhin wird die Pipeline wie im folgenden Beispiel angezeigt:

    ![Pipeline anzeigen](media/solution-template-move-files/move-files4.png)

5. Klicken Sie auf **Debuggen**, geben Sie die **Parameter** ein, und klicken Sie dann auf **Fertig stellen**.   Bei den Parametern handelt es sich um den Ordnerpfad, aus dem Sie Dateien verschieben möchten, sowie den Ordnerpfad, in den Sie Dateien verschieben möchten. 

    ![Führen Sie die Pipeline aus.](media/solution-template-move-files/move-files5.png)

6. Überprüfen Sie das Ergebnis.

    ![Ergebnis überprüfen](media/solution-template-move-files/move-files6.png)

## <a name="next-steps"></a>Nächste Schritte

- [Kopieren neuer und geänderter Dateien nach „LastModifiedDate“ mit Azure Data Factory](solution-template-copy-new-files-lastmodifieddate.md)

- [Kopieren von Dateien aus mehreren Containern mit Azure Data Factory](solution-template-copy-files-multiple-containers.md)