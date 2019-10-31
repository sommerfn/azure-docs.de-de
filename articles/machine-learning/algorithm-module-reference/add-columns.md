---
title: 'Add Columns: Modulreferenz'
titleSuffix: Azure Machine Learning service
description: Erfahren Sie, wie Sie das Modul „Add Columns“ (Hinzufügen von Spalten) in Azure Machine Learning Service verwenden können, um zwei Datensätze zu verketten.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ms.openlocfilehash: cbb1249b529fde1368bfaff0c6c251c93fa6c309
ms.sourcegitcommit: e0e6663a2d6672a9d916d64d14d63633934d2952
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "72693859"
---
# <a name="add-columns-module"></a>Modul „Add Columns“ (Hinzufügen von Spalten)

In diesem Artikel wird ein Modul der grafischen Benutzeroberfläche (Vorschau) für Azure Machine Learning Service beschrieben.

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

Sehen Sie sich die [Gruppe der verfügbaren Module](module-reference.md) für Azure Machine Learning Service an. 