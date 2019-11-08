---
title: 'Import Data (Daten importieren): Modulreferenz'
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie das Modul Import Data in Azure Machine Learning verwenden, um Daten aus vorhandenen Clouddatendiensten in eine Machine Learning-Pipeline zu laden.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 10/22/2019
ms.openlocfilehash: 5fa8d3984c758d0bf95372864f3bffeb6f302c83
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73497815"
---
# <a name="import-data-module"></a>Modul „Import Data“ (Daten importieren)

In diesem Artikel wird ein Modul in Azure Machine Learning-Designer (Vorschauversion) beschrieben.

Verwenden Sie dieses Modul, um Daten aus vorhandenen Clouddatendiensten in eine Machine Learning-Pipeline zu laden. 

> [!Note]
> Alle von diesem Modul bereitgestellten Funktionen können über **Datenspeicher** und **Datasets** auf der Landing Page des Arbeitsbereichs ausgeführt werden. Die Verwendung von **Datenspeichern** und **Datasets** wird empfohlen, da hierbei zusätzliche Features wie Datenüberwachung eingeschlossen sind. Weitere Informationen finden Sie in den Artikeln [Zugreifen auf Daten](../service/how-to-access-data.md) und [Registrieren von Datasets](../service/how-to-create-register-datasets.md).
> Nachdem Sie ein Dataset registriert haben, finden Sie es auf der Designer-Oberfläche in der Kategorie **Datasets** -> **My Datasets** (Meine Datasets). Dieses Modul ist für Benutzer von Studio (klassisch) reserviert, die eine vertraute Benutzeroberfläche verwenden möchten. 
>

Wählen Sie zunächst die Quelle aus, aus der gelesen werden soll, und schließen Sie die weiteren Einstellungen ab. Das Modul **Import Data** unterstützt das Lesen von Daten aus den folgenden Quellen:

- URL über HTTP
- Azure-Cloudspeicher über [**Datenspeicher**](../service/how-to-access-data.md)
    - Azure-Blobcontainer
    - Azure-Dateifreigabe
    - Azure Data Lake
    - Azure Data Lake Gen2
    - Azure SQL-Datenbank
    - Azure PostgreSQL    

Bevor Sie Cloudspeicher verwenden können, müssen Sie zunächst einen Datenspeicher in Ihrem Azure Machine Learning-Arbeitsbereich registrieren. Weitere Informationen hierzu finden Sie unter [Zugreifen auf Daten](../service/how-to-access-data.md). 

Nachdem Sie die gewünschten Daten definiert und eine Verbindung mit der Quelle hergestellt haben, leitet **[Import Data](./import-data.md)** den Datentyp jeder Spalte basierend auf den darin enthaltenen Werten ab und lädt die Daten in Ihre Designer-Pipeline. **Import Data** gibt ein Dataset aus, das mit beliebigen Designer-Pipelines verwendet werden kann.

Wenn sich Ihre Quelldaten ändern, können Sie das Dataset aktualisieren und neue Daten hinzufügen, indem Sie [Import Data](./import-data.md) erneut ausführen. Wenn die Daten jedoch nicht bei jeder Ausführung der Pipeline erneut aus der Quelle gelesen werden sollen, legen Sie die Option **Use cached results** (Zwischengespeicherte Ergebnisse verwenden) auf TRUE fest. Ist diese Option ausgewählt, überprüft das Modul, ob die Pipeline schon einmal unter Verwendung der gleichen Quelle und der gleichen Eingabeoptionen ausgeführt wurde. Wenn eine frühere Ausführung gefunden wird, werden die Daten aus dem Cache verwendet, statt die Daten erneut aus der Quelle zu laden.

## <a name="how-to-configure-import-data"></a>Konfigurieren von „Import Data“ (Daten importieren)

1. Fügen Sie Ihrer Pipeline das Modul **Import Data** (Daten importieren) hinzu. Sie finden dieses Modul in der Kategorie **Data Input and Output** (Dateieingabe und -ausgabe) im Designer.

1. Klicken Sie auf **Launch Data Import Wizard** (Datenimport-Assistenten starten), um die Datenquelle mithilfe eines Assistenten zu konfigurieren.

    Der Assistent ruft den Kontonamen und die Anmeldeinformationen ab und unterstützt Sie bei der Konfiguration weiterer Optionen. Wenn Sie eine bestehende Konfiguration bearbeiten, werden die aktuellen Werte zuerst geladen.

1. Wählen Sie **Datenquelle** und dann den Datenquellentyp aus. Dabei kann es sich um „HTTP“ oder „Datenspeicher“ handeln.

    Wenn Sie „Datenspeicher“ auswählen, können Sie vorhandene Datenspeicher auswählen, die bereits in Ihrem Azure Machine Learning-Arbeitsbereich registriert sind, oder einen neuen Datenspeicher erstellen. Anschließend definieren Sie den Pfad der zu importierenden Daten im Datenspeicher. Sie können den Pfad ganz einfach durchsuchen, indem Sie auf **Pfad durchsuchen** ![import-data-path](media/module/import-data-path.png) klicken.

1. Wählen Sie das Vorschauschema aus, um die Spalten zu filtern, die Sie einschließen möchten. Sie können auch erweiterte Einstellungen wie Trennzeichen in den Analyseoptionen definieren.

    ![import-data-preview](media/module/import-data.png)

1. Wählen Sie die Option **Use cached results**, wenn Sie das Dataset für sukzessive Ausführungen zwischenspeichern möchten.

    Sofern keine weiteren Änderungen an den Modulparametern vorgenommen werden, lädt die Pipeline die Daten nur bei der erstmaligen Ausführung des Moduls. Anschließend wird eine zwischengespeicherte Version des Datasets verwendet.

    Deaktivieren Sie diese Option, wenn die Daten bei jeder Ausführung der Pipeline erneut geladen werden sollen.

1. Ausführen der Pipeline.

    Wenn die Daten durch Import Data in den Designer geladen werden, wird der Datentyp jeder Spalte basierend auf den darin enthaltenen Werten (numerisch oder kategorisch) abgeleitet.

    Sofern eine Spaltenüberschrift vorhanden ist, wird diese zur Benennung der Spalten im Ausgabedataset verwendet.

    Wenn keine Spaltenüberschriften in den Daten enthalten sind, werden neue Spaltennamen im Format col1, col2,... , coln* generiert.

## <a name="results"></a>Ergebnisse

Nachdem der Import abgeschlossen wurde, klicken Sie auf das Ausgabedataset, und wählen Sie **Visualize** (Visualisieren) aus, um zu überprüfen, ob die Daten erfolgreich importiert wurden.

Wenn Sie die Daten zur Wiederverwendung speichern möchten, anstatt bei jeder Pipelineausführung ein neues Dataset zu importieren, klicken Sie mit der rechten Maustaste auf die Ausgabe, und wählen Sie **Als Dataset speichern** aus. Wählen Sie einen Namen für das Dataset aus. Das gespeicherte Dataset enthält die Daten zum Zeitpunkt der Speicherung, und die Daten werden bei erneuter Ausführung der Pipeline nicht aktualisiert. Das gilt auch, wenn sich das Dataset in der Pipeline ändert. Dies kann hilfreich sein, um Momentaufnahmen von Daten zu erstellen.

Nach dem Importieren der Daten können einige zusätzliche Vorbereitungen zur Modellierung und Analyse erforderlich sein:

- Verwenden Sie [Edit Metadata](./edit-metadata.md) (Metadaten bearbeiten), um Spaltennamen zu ändern, einen abweichenden Datentyp für eine Spalte festzulegen, oder um anzugeben, dass einige Spalten für Bezeichnungen oder Features stehen.

- Verwenden Sie [Select Columns in Dataset](./select-columns-in-dataset.md) (Spalten im Dataset auswählen), um eine Teilmenge von Spalten auszuwählen, die während der Modellierung transformiert oder verwendet werden sollen. Über das Modul [Add Columns](./add-columns.md) (Spalten hinzufügen) lassen sich die transformierten oder entfernten Spalten ganz einfach wieder mit dem ursprünglichen Dataset verknüpfen.  

- Verwenden Sie [Partition and Sample](./partition-and-sample.md) (Partitionieren und Stichprobe entnehmen), um das Dataset zu unterteilen, ein Sampling durchzuführen oder um Top-N-Zeilen abzurufen.

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich den [Satz der verfügbaren Module](module-reference.md) für Azure Machine Learning an. 
