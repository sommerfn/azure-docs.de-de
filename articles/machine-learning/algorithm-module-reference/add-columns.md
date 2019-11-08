---
title: 'Add Columns: Modulreferenz'
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie das Modul „Add Columns“ (Hinzufügen von Spalten) in Azure Machine Learning verwenden können, um zwei Datensätze zu verketten.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 10/22/2019
ms.openlocfilehash: 55981279cb1902424d1a0f77af097dc379d7222f
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73493964"
---
# <a name="add-columns-module"></a>Modul „Add Columns“ (Hinzufügen von Spalten)

In diesem Artikel wird ein Modul in Azure Machine Learning-Designer (Vorschauversion) beschrieben.

Verwenden Sie dieses Modul zum Verketten zweier Datasets. Sie kombinieren alle Spalten aus den beiden Datasets, die Sie als Eingaben angeben, zu einem einzigen Dataset. Wenn Sie mehr als zwei Datasets verketten müssen, verwenden Sie mehrere Instanzen von **Add Columns**.



## <a name="how-to-configure-add-columns"></a>Konfigurieren von „Add Columns“
1. Fügen Sie Ihrer Pipeline das Modul **Add Columns** (Spalten hinzufügen) hinzu.

2. Verbinden Sie die beiden Datasets, die Sie verketten möchten. Wenn Sie mehr als zwei Datasets kombinieren möchten, können Sie mehrere Kombinationen von **Add Columns** miteinander verketten.

    - Es ist möglich, zwei Spalten mit einer unterschiedlichen Anzahl von Zeilen zu kombinieren. Der Ausgabedataset wird mit fehlenden Werten für jede Zeile in der kleineren Quellspalte aufgefüllt.

    - Sie können keine einzelnen hinzuzufügenden Spalten auswählen. Alle Spalten aus jedem Dataset werden verkettet, wenn Sie **Add Columns** verwenden. Wenn Sie daher nur eine Teilmenge der Spalten hinzufügen möchten, verwenden Sie „Select Columns in Dataset“ (Spalten im Dataset auswählen), um ein Dataset mit den gewünschten Spalten zu erstellen.

3. Ausführen der Pipeline.

### <a name="results"></a>Ergebnisse
Gehen Sie nach der Pipelineausführung wie folgt vor:

- Um die ersten Zeilen des neuen Datasets anzuzeigen, klicken Sie mit der rechten Maustaste auf die Ausgabe von **Add Columns**, und wählen Sie „Visualize“ (Visualisieren) aus.

Die Anzahl der Spalten im neuen Dataset entspricht der Summe der Spalten beider Eingabedatasets.

Wenn es in den Eingabedatasets zwei Spalten mit dem gleichen Namen gibt, wird dem Namen der Spalte ein numerisches Suffix hinzugefügt. Wenn es beispielsweise zwei Instanzen einer Spalte namens „TargetOutcome“ gibt, wird die linke Spalte in „TargetOutcome_1“ und die rechte Spalte in „TargetOutcome_2“ umbenannt.

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich die [Gruppe der verfügbaren Module](module-reference.md) für Azure Machine Learning an. 