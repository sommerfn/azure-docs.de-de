---
title: 'Import Data (Daten importieren): Modulreferenz'
titleSuffix: Azure Machine Learning service
description: Hier erfahren Sie, wie Sie das Modul „Import Data“ (Daten importieren) in Azure Machine Learning Service verwenden, um Daten aus vorhandenen Clouddatendiensten in eine Machine Learning-Pipeline zu laden.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ms.openlocfilehash: fef7d686479b24b0402ab6f1e6990df74231b8d6
ms.sourcegitcommit: e0e6663a2d6672a9d916d64d14d63633934d2952
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "72693150"
---
# <a name="import-data-module"></a>Modul „Import Data“ (Daten importieren)

In diesem Artikel wird ein Modul der grafischen Benutzeroberfläche (Vorschau) für den Azure Machine Learning Service beschrieben.

Verwenden Sie dieses Modul, um Daten aus vorhandenen Clouddatendiensten in eine Machine Learning-Pipeline zu laden.  

Wählen Sie zunächst die Art des cloudbasierten Speichers aus, aus dem gelesen werden soll, und legen Sie die zusätzlichen Einstellungen fest. Nachdem Sie die gewünschten Daten definiert und eine Verbindung mit der Quelle hergestellt haben, leitet das Modul [Import Data](./import-data.md) den Datentyp jeder Spalte basierend auf den darin enthaltenen Werten ab und lädt die Daten in Ihren Azure Machine Learning-Arbeitsbereich. Das Modul [Import Data](./import-data.md) (Daten importieren) gibt ein Dataset aus, das für beliebige Pipelines verwendet werden kann.

  
Wenn sich Ihre Quelldaten ändern, können Sie das Dataset aktualisieren und neue Daten hinzufügen, indem Sie [Import Data](./import-data.md) erneut ausführen. Wenn die Daten jedoch nicht bei jeder Ausführung der Pipeline erneut aus der Quelle gelesen werden sollen, legen Sie die Option **Use cached results** (Zwischengespeicherte Ergebnisse verwenden) auf „TRUE“ fest. Ist diese Option ausgewählt, überprüft das Modul, ob die Pipeline schon einmal unter Verwendung der gleichen Quelle und der gleichen Eingabeoptionen ausgeführt wurde. Wenn eine frühere Ausführung gefunden wird, werden die Daten aus dem Cache verwendet, statt die Daten erneut aus der Quelle zu laden.
 

## <a name="data-sources"></a>Datenquellen

Das Modul „Import Data“ unterstützt die folgenden Datenquellen: Klicken Sie auf die Links, um ausführliche Anleitungen und Beispiele zur Verwendung der einzelnen Datenquellen zu erhalten. 
 
Wenn Sie nicht sicher sind, wie bzw. wo Daten gespeichert werden sollen, informieren Sie sich in diesem Leitfaden über allgemeine Datenszenarien im Data Science-Prozess:  [Szenarien für die erweiterte Analyse in Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-plan-sample-scenarios). 


|Datenquelle| Nutzung|
|-----------|-----------|  
|[Web URL via HTTP](./import-from-web-url-via-http.md)|Ruft Daten ab, die unter einer Web-URL gehostet sind. Die Web-URL verwendet HTTP und wurde im CSV-, TSV-, ARFF- oder SvmLight-Format bereitgestellt.|  
|[Import from Azure Blob Storage](./import-from-azure-blob-storage.md) (Daten aus Azure Blob Storage importieren) |Ruft Daten ab, die im Blob-Dienst von Azure gespeichert sind.|  
|[Importieren aus Azure SQL-Datenbank](./import-from-azure-sql-database.md) |Abrufen von Daten aus Azure SQL-Datenbank|

## <a name="how-to-configure-import-data"></a>Konfigurieren von „Import Data“ (Daten importieren)
 
1. Fügen Sie Ihrer Pipeline das Modul **Import Data** (Daten importieren) hinzu. Sie finden dieses Modul in der Kategorie **Data Input and Output** (Dateieingabe und -ausgabe) in der Oberfläche.

1. Klicken Sie auf **Datenquelle**, und wählen Sie die Art des cloudbasierten Speichers aus, aus dem gelesen werden soll. 

    Zusätzliche Einstellungen hängen vom ausgewählten Speichertyp und davon ab, ob der Speicher gesichert ist oder nicht. Möglicherweise müssen Sie den Kontonamen, den Dateityp oder Anmeldeinformationen angeben. Einige Quellen erfordern keine Authentifizierung, bei anderen werden Sie jedoch u. U. nach Kontonamen, Schlüssel oder Containernamen gefragt.

1. Wählen Sie die Option **Use cached results**, wenn Sie das Dataset für sukzessive Ausführungen zwischenspeichern möchten.

    Sofern keine weiteren Änderungen an den Modulparametern vorgenommen werden, lädt die Pipeline die Daten nur bei der erstmaligen Ausführung des Moduls. Anschließend wird eine zwischengespeicherte Version des Datasets verwendet.

    Deaktivieren Sie diese Option, wenn die Daten bei jeder Ausführung der Pipeline erneut geladen werden sollen.

1. Ausführen der Pipeline.

    Wenn die Daten durch „Import Data“ in die Oberfläche geladen werden, wird der Datentyp jeder Spalte basierend auf den darin enthaltenen Werten (entweder numerisch oder kategorisch) abgeleitet.

    - Sofern eine Spaltenüberschrift vorhanden ist, wird diese zur Benennung der Spalten im Ausgabedataset verwendet.

    - Wenn keine Spaltenüberschriften in den Daten enthalten sind, werden neue Spaltennamen im Format col1, col2,... , coln* generiert.

## <a name="results"></a>Ergebnisse

Nachdem der Import abgeschlossen wurde, klicken Sie auf das Ausgabedataset, und wählen Sie **Visualize** (Visualisieren) aus, um zu überprüfen, ob die Daten erfolgreich importiert wurden.

Wenn Sie die Daten zur Wiederverwendung speichern möchten, anstatt bei jeder Pipelineausführung ein neues Dataset zu importieren, klicken Sie mit der rechten Maustaste auf die Ausgabe, und wählen Sie **Als Dataset speichern** aus. Wählen Sie einen Namen für das Dataset aus. Das gespeicherte Dataset enthält die Daten zum Zeitpunkt der Speicherung, und die Daten werden bei erneuter Ausführung der Pipeline nicht aktualisiert. Das gilt auch, wenn sich das Dataset in der Pipeline ändert. Dies kann hilfreich sein, um Momentaufnahmen von Daten zu erstellen.

Nach dem Importieren der Daten können einige zusätzliche Vorbereitungen zur Modellierung und Analyse erforderlich sein:


- Verwenden Sie [Edit Metadata](./edit-metadata.md) (Metadaten bearbeiten), um Spaltennamen zu ändern, einen abweichenden Datentyp für eine Spalte festzulegen, oder um anzugeben, dass einige Spalten für Bezeichnungen oder Features stehen.

- Verwenden Sie [Select Columns in Dataset](./select-columns-in-dataset.md) (Spalten im Dataset auswählen), um eine Teilmenge von Spalten auszuwählen, die während der Modellierung transformiert oder verwendet werden sollen. Über das Modul [Add Columns](./add-columns.md) (Spalten hinzufügen) lassen sich die transformierten oder entfernten Spalten ganz einfach wieder mit dem ursprünglichen Dataset verknüpfen.  

- Verwenden Sie [Partition and Sample](./partition-and-sample.md) (Partitionieren und Stichprobe entnehmen), um das Dataset zu unterteilen, ein Sampling durchzuführen oder um Top-N-Zeilen abzurufen.

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich die [Gruppe der verfügbaren Module](module-reference.md) für Azure Machine Learning Service an. 