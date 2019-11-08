---
title: Zusammenfassen von Daten
titleSuffix: Azure Machine Learning service
description: Erfahren Sie, wie Sie das Modul „Summarize Data“ (Zusammenfassen von Daten) in Azure Machine Learning verwenden, um einen einfachen beschreibenden Statistikbericht für die Spalten in einem Dataset zu generieren.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 09/09/2019
ms.openlocfilehash: 503eb533b83cbeedcc16b73a9c1dbe821a4d4d94
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73492571"
---
# <a name="summarize-data"></a>Zusammenfassen von Daten

In diesem Artikel wird ein Modul von Azure Machine Learning-Designer (Vorschauversion) beschrieben.

Verwenden Sie das Modul „Summarize Data“ (Zusammenfassen von Daten), um einen Satz von statistischen Standardmaßzahlen zu erstellen, die jede Spalte in der Eingabetabelle beschreiben.

Aggregierende Parameter (Zusammenfassungsstatistiken) sind nützlich, wenn Sie die Merkmale eines gesamten Datasets verstehen möchten. Beispielsweise könnte es sein, dass Sie Folgendes wissen möchten:

- Wie viele Werte fehlen in jeder Spalte?
- Wie viele eindeutige Werte sind in einer Featurespalte vorhanden?
- Was groß ist die mittlere und die Standardabweichung für jede Spalte?

Das Modul berechnet die wichtigen Maßzahlen für jede Spalte und gibt eine Zeile von aggregierenden Parametern für jede Variable (Datenspalte) zurück, die als Eingabe bereitgestellt wird.

## <a name="how-to-configure-summarize-data"></a>Konfigurieren von „Summarize Data“  

1. Fügen Sie das Modul **Summarize Data** zu Ihrer Pipeline hinzu. Sie finden dieses Modul in der Kategorie **Statistical Functions** (Statistische Funktionen) im Designer.

1. Stellen Sie eine Verbindung mit dem Dataset her, für das Sie einen Bericht generieren möchten.

    Möchten Sie den Bericht nur für einige Spalten erstellen, verwenden Sie das Modul [Select Columns in Dataset](select-columns-in-dataset.md) (Auswählen von Spalten in Dataset), um die Teilmenge der Spalten zu projizieren, mit denen Sie arbeiten möchten.

1. Es sind keine weiteren Parameter erforderlich. Standardmäßig analysiert das Modul alle Spalten, die als Eingabe bereitgestellt werden, und abhängig vom Typ der Werte in den Spalten gibt das Modul einen relevanten Satz von statistischen Werten zurück, die im Abschnitt [Ergebnisse](#results) beschrieben sind.

1. Führen Sie die Pipeline aus, oder klicken Sie mit der rechten Maustaste auf das Modul, und wählen Sie **Run selected** (Ausgewählte ausführen) aus.

## <a name="results"></a>Ergebnisse

Der Bericht aus dem Modul kann die folgenden statistischen Werte enthalten. 

|Spaltenname|BESCHREIBUNG|
|------|------|  
|**Feature**|Name der Spalte|
|**Count**|Anzahl aller Zeilen|
|**Unique Value Count**|Anzahl der eindeutigen Werte in der Spalte|
|**Missing Value Count**|Anzahl der eindeutigen Werte in der Spalte|
|**Min**|Kleinster Wert in der Spalte|  
|**Max**|Größter Wert in der Spalte|
|**Mean**|Mittelwert aller Spaltenwerte|
|**Mean Deviation**|Mittlere Abweichung von den Spaltenwerten|
|**1st Quartile**|Wert beim ersten Quartil|
|**Median**|Medianwert der Spalte|
|**3rd Quartile**|Wert beim dritten Quartil|
|**Mode**|Mode der Spaltenwerte|
|**Bereich**|Ganze Zahl, die die Anzahl der Werte zwischen dem kleinsten und dem größten Wert darstellt|
|**Sample Variance**|Varianz für die Spalte; siehe Hinweis|
|**Sample Standard Deviation**|Standardabweichung für die Spalte; siehe Hinweis|
|**Sample Skewness**|Schiefe für die Spalte; siehe Hinweis|
|**Sample Kurtosis**|Wölbung für die Spalte; siehe Hinweis|
|**P0.5**|0,5-%-Perzentil|
|**P1**|1-%-Perzentil|
|**P5**|5-%-Perzentil|
|**P95**|95-%-Perzentil|
|**P99.5**|99,5-%-Perzentil |

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich die [Gruppe der verfügbaren Module](module-reference.md) für Azure Machine Learning Service an.  
