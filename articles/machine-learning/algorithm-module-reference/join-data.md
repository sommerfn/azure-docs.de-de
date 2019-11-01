---
title: 'Join Data (Daten verknüpfen): Modulreferenz'
titleSuffix: Azure Machine Learning service
description: Hier erfahren Sie, wie Sie das Modul Join Data (Daten verknüpfen) in Azure Machine Learning Service verwenden, um Datasets zusammenzuführen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: peterlu
ms.date: 06/01/2019
ms.openlocfilehash: ec9de6975931219ae45543833421234c4d71e6af
ms.sourcegitcommit: e0e6663a2d6672a9d916d64d14d63633934d2952
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "72693089"
---
# <a name="join-data"></a>Verknüpfen von Daten

In diesem Artikel erfahren Sie, wie Sie das Modul **Join Data** (Daten verknüpfen) auf der grafischen Benutzeroberfläche von Azure Machine Learning Service verwenden, um zwei Datasets mithilfe eines datenbankähnlichen Join-Vorgangs zusammenzuführen.  

## <a name="how-to-configure-join-data"></a>Konfigurieren von „Join Data“ (Daten verknüpfen)

Wenn Sie einen Join-Vorgang für zwei Datasets ausführen möchten, müssen die beiden über eine Schlüsselspalte miteinander verknüpft sein. Zusammengesetzte Schlüssel mit mehreren Spalten werden ebenfalls unterstützt. 

1. Fügen Sie die Datasets hinzu, die Sie vereinen möchten, und ziehen Sie anschließend das Modul **Join Data** (Daten verknüpfen) in Ihre Pipeline. 

    Das Modul befindet sich in der Kategorie **Datentransformation** (unter **Bearbeitung**).

1. Verbinden Sie die Datasets mit dem Modul **Join Data** (Daten verknüpfen). 
 
1. Wählen Sie **Launch column selector** (Spaltenauswahl starten) aus, um mindestens eine Schlüsselspalte auszuwählen. Zur Erinnerung: Wählen Sie sowohl Spalten für die linke Eingabe als auch Spalten für die rechte Eingabe aus.

    Einzelner Schlüssel:

    Wählen Sie eine einzelne Schlüsselspalte für beide Eingaben aus.
    
    Zusammengesetzter Schlüssel:

    Wählen Sie alle Schlüsselspalten aus der linken und rechten Eingabe in der gleichen Reihenfolge aus. Die Tabellen werden durch das Modul **Join Data** (Daten verknüpfen) verknüpft, wenn alle Schlüsselspalten übereinstimmen. Aktivieren Sie die Option **Allow duplicates and preserve column order in selection** (Duplikate zulassen und Spaltenreihenfolge in der Auswahl beibehalten), wenn die Spaltenreihenfolge von der Originaltabelle abweicht. 

    ![Spaltenauswahl](media/module/join-data-column-selector.png)


1. Wählen Sie die Option **Groß-/Kleinschreibung beachten** aus, wenn die Beachtung der Groß-/Kleinschreibung bei einem Join-Vorgang für eine Textspalte erhalten bleiben soll. 
   
1. Geben Sie über die Dropdownliste **Verknüpfungstyp** an, wie die Datasets vereint werden sollen.  
  
    * **Innerer Join:** Ein *innerer Join* ist der gängigste Join-Vorgang. Bei diesem Typ werden die vereinten Zeilen nur zurückgegeben, wenn die Werte der Schlüsselspalten übereinstimmen.  
  
    * **Linker äußerer Join:** Bei einem *linken äußeren Join* werden verknüpfte Zeilen für alle Zeilen aus der linken Tabelle zurückgegeben. Wenn für eine Zeile in der linken Tabelle keine übereinstimmenden Zeilen in der rechten Tabelle vorhanden sind, weist die zurückgegebene Zeile für alle Spalten, die aus der rechten Tabelle stammen, fehlende Werte auf. Für fehlende Werte kann auch ein Ersatzwert angegeben werden.  
  
    * **Vollständiger äußerer Join:** Bei einem *vollständigen äußeren Join* werden alle Zeilen aus der linken Tabelle (**Tabelle 1**) und aus der rechten Tabelle (**Tabelle 2**) zurückgegeben.  
  
         Für jede Zeile aus einer Tabelle, für die keine übereinstimmenden Zeilen in der jeweils anderen Tabelle vorhanden sind, enthält das Ergebnis eine Zeile mit fehlenden Werten.  
  
    * **Linke halbe Verknüpfung:** Bei einer *linken halben Verknüpfung* werden nur die Werte aus der linken Tabelle zurückgegeben, wenn die Werte der Schlüsselspalten übereinstimmen.  

1. Für die Option **Keep right key columns in joined table** (Rechte Schlüsselspalten in verknüpfter Tabelle beibehalten) gilt Folgendes:

    * Aktivieren Sie diese Option, wenn die Schlüssel aus beiden Eingabetabellen angezeigt werden sollen.
    * Deaktivieren Sie diese Option, wenn nur die Schlüsselspalten aus der linken Eingabe zurückgegeben werden sollen.

1. Führen Sie die Pipeline aus, oder wählen Sie das Modul „Join Data“ (Daten verknüpfen) und anschließend **Auswahl ausführen** aus, um den Join-Vorgang auszuführen.

1. Klicken Sie zum Anzeigen der Ergebnisse mit der rechten Maustaste auf **Join Data** (Daten verknüpfen), und klicken Sie anschließend auf **Results dataset** (Ergebnisdataset) > **Visualize** (Visualisieren).

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich die [Gruppe der verfügbaren Module](module-reference.md) für Azure Machine Learning Service an. 