---
title: 'Add Rows: Modulreferenz'
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie das Modul „Add Rows“ (Hinzufügen von Zeilen) in Azure Machine Learning verwenden können, um zwei Datensätze zu verketten.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 10/22/2019
ms.openlocfilehash: a16dc0bfc3fae8852c46435f6407c56149e1b14e
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73493949"
---
# <a name="add-rows-module"></a>Modul „Add Rows“ (Hinzufügen von Zeilen)

In diesem Artikel wird ein Modul in Azure Machine Learning-Designer (Vorschauversion) beschrieben.

Verwenden Sie dieses Modul zum Verketten zweier Datasets. Bei der Verkettung werden die Zeilen des zweiten Datasets am Ende des ersten Datasets hinzugefügt.  
  
Die Verkettung von Zeilen ist in folgenden Szenarien nützlich:  
  
+ Sie haben eine Reihe von Auswertungsstatistiken erstellt und möchten diese zur besseren Berichterstellung in einer Tabelle zusammenfassen.  
  
+ Sie haben mit verschiedenen Datasets gearbeitet und möchten die Datasets zu einem endgültigen Dataset zusammenfassen.  

## <a name="how-to-use-add-rows"></a>Verwenden von „Add Rows“  

Um Zeilen aus zwei Datasets zu verketten, müssen die Zeilen exakt das gleiche Schema haben. Das heißt, dass die Anzahl der Spalten und der Datentyp in den Spalten identisch sein müssen.

1.  Ziehen Sie das Modul **Add Rows** (Zeilen hinzufügen) in Ihre Pipeline. Sie finden es unter **Datentransformation** in der Kategorie **Manipulate** (Bearbeiten).

2. Verbinden Sie die Datasets mit den beiden Eingabeports. Das Dataset, das Sie anfügen möchten, muss mit dem zweiten (rechten) Port verbunden werden. 
  
3.  Ausführen der Pipeline. Die Anzahl der Zeilen im Ausgabedataset muss der Summe der Zeilen beider Eingabedatasets entsprechen.

    Wenn Sie das gleiche Dataset beiden Eingaben des Moduls **Add Rows** hinzufügen, wird das Dataset dupliziert. 

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich die [Gruppe der verfügbaren Module](module-reference.md) für Azure Machine Learning an. 