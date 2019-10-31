---
title: 'Convert to CSV: Modulreferenz'
titleSuffix: Azure Machine Learning service
description: Erfahren Sie, wie Sie mit dem Modul „Convert to CSV“ (Konvertieren in das CSV-Format) in Azure Machine Learning Service ein Dataset in ein CSV-Format konvertieren, das heruntergeladen, exportiert oder mit R- oder Python-Skriptmodulen gemeinsam genutzt werden kann.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ms.openlocfilehash: 37c1653b31049dc0142cf8c0c6f6d405338b1d1f
ms.sourcegitcommit: e0e6663a2d6672a9d916d64d14d63633934d2952
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "72693194"
---
# <a name="convert-to-csv-module"></a>Modul „Convert to CSV (Konvertieren in das CSV-Format)

In diesem Artikel wird ein Modul der grafischen Benutzeroberfläche (Vorschau) für Azure Machine Learning Service beschrieben.

Nutzen Sie dieses Modul, um ein Dataset in ein CSV-Format konvertieren, das heruntergeladen, exportiert oder mit R- oder Python-Skriptmodulen gemeinsam genutzt werden kann.

### <a name="more-about-the-csv-format"></a>Weitere Informationen zum CSV-Format 

Das CSV-Format (Comma-Separated Values) ist ein Dateiformat, das von vielen externen Tools für maschinelles Lernen verwendet wird. CSV ist ein gängiges Austauschformat bei der Arbeit mit Open-Source-Sprachen wie R oder Python.

Auch wenn Sie den größten Teil der Arbeit in Azure Machine Learning erledigen, gibt es Situationen, in denen es sinnvoll sein kann, Ihr Dataset in das CSV-Format zu konvertieren, um es in externen Tools zu verwenden. Beispiel:

+ Laden Sie die CSV-Datei herunter, um sie in Excel zu öffnen, oder importieren Sie sie in eine relationale Datenbank.  
+ Speichern Sie die CSV-Datei im Cloudspeicher, und verbinden Sie sie mit Power BI, um Visualisierungen zu erstellen.  
+ Verwenden Sie das CSV-Format, um Daten zur Verwendung in R und Python aufzubereiten. Klicken Sie einfach mit der rechten Maustaste auf die Ausgabe des Moduls, um den Code zu generieren, der für den direkten Zugriff auf die Daten in Python oder einem Jupyter-Notebook benötigt wird. 

Wenn Sie einen Datensatz in das CSV-Format konvertieren, wird die Datei in Ihrem Azure ML-Arbeitsbereich gespeichert. Sie können ein Azure-Speicherhilfsprogramm verwenden, um die Datei zu öffnen und direkt zu nutzen, oder Sie können mit der rechten Maustaste auf die Modulausgabe klicken und die CSV-Datei auf Ihren Computer herunterladen oder sie in R- oder Python-Code verwenden.  

## <a name="how-to-configure-convert-to-csv"></a>Konfigurieren von „Convert to CSV“

1.  Fügen Sie Ihrer Pipeline das Modul [Convert to CSV](./convert-to-csv.md) (In CSV konvertieren) hinzu. Sie finden dieses Modul auf der Benutzeroberfläche in der Gruppe **Data Format Conversions**. 

2. Verbinden Sie es mit einem beliebigen Modul, das ein Dataset ausgibt.   
  
3.  Ausführen der Pipeline.

### <a name="results"></a>Ergebnisse
  

Doppelklicken Sie auf die Ausgabe von [Convert to CSV](./convert-to-csv.md), und wählen Sie eine der folgenden Optionen aus.  

 + **Result Dataset -> Download** (Ergebnisdataset -> Herunterladen): Öffnet unmittelbar eine Kopie der Daten im CSV-Format, die Sie in einem lokalen Ordner speichern können. Wenn Sie keinen Ordner angeben, wird ein Standarddateiname übernommen, und die CSV-Datei wird in der lokalen Bibliothek **Downloads** gespeichert.


 + **Result Dataset -> Save as Dataset** (Ergebnisdataset -> Als Dataset speichern): Speichert die CSV-Datei als separates Dataset in den Azure ML-Arbeitsbereich zurück.

 + **Generate Data Access Code** (Datenzugriffscode generieren): Azure ML generiert zwei Codesätze, damit Sie auf die Daten zugreifen können, entweder mithilfe von Python oder R. Um auf die Daten zuzugreifen, kopieren Sie den Codeausschnitt in Ihre Anwendung. ( *„Generate data access code“ wird in Kürze verfügbar sein.* )

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich die [Gruppe der verfügbaren Module](module-reference.md) für Azure Machine Learning Service an. 