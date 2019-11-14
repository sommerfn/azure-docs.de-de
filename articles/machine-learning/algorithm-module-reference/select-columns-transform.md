---
title: 'Select Columns Transform: Modulreferenz'
titleSuffix: Azure Machine Learning service
description: Es wird beschrieben, wie Sie das Modul „Select Columns Transform“ (Auswählen der Spaltentransformation) in Azure Machine Learning Service verwenden, um eine Transformation zu erstellen, in der dieselbe Teilmenge von Spalten wie im angegebenen Dataset ausgewählt wird.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 10/10/2019
ms.openlocfilehash: 383adeb2bd081893d4202239cb97c12bf9a5a170
ms.sourcegitcommit: bc7725874a1502aa4c069fc1804f1f249f4fa5f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73717035"
---
# <a name="select-columns-transform"></a>Select Columns Transform

In diesem Artikel wird die Verwendung des Moduls „Select Columns Transform“ (Auswählen der Spaltentransformation) im Azure Machine Learning-Designer (Vorschauversion) beschrieben. Der Zweck des Moduls „Select Columns Transform“ besteht darin sicherzustellen, dass für alle weiteren Machine Learning-Vorgänge ein vorhersagbarer, einheitlicher Satz von Spalten verwendet wird.

Dieses Modul ist nützlich für Aufgaben wie Bewertung, für die bestimmte Spalten erforderlich sind. Änderungen in den verfügbaren Spalten können die Pipeline unterbrechen oder die Ergebnisse ändern.

Sie verwenden das Modul „Select Columns Transform“, um einen Satz von Spalten zu erstellen und zu speichern. Danach verwenden Sie das Modul [Apply Transformation](apply-transformation.md) (Anwenden einer Transformation), um diese Auswahl auf neue Daten anzuwenden.

## <a name="how-to-use-select-columns-transform"></a>Verwenden von „Select Columns Transform“

In diesem Szenario wird davon ausgegangen, dass Sie die Featureauswahl verwenden möchten, um einen dynamischen Satz von Spalten zu generieren, die zum Trainieren eines Modells verwendet werden sollen. Zur Sicherstellung, dass die Spaltenauswahl für den Bewertungsprozess identisch ist, verwenden Sie das Modul „Select Columns Transform“, um die Spaltenauswahl zu erfassen und diese Auswahl an anderer Stelle in der Pipeline anzuwenden.

1. Fügen Sie Ihrer Pipeline im Designer ein Eingabedataset hinzu.

2. Fügen Sie eine Instanz von [Filter Based Feature Selection](filter-based-feature-selection.md) (Filterbasierte Featureauswahl) hinzu.

3. Verbinden Sie die Module, und konfigurieren Sie das Featureauswahlmodul, damit im Eingabedataset automatisch eine Reihe von optimalen Features gefunden wird.

4. Fügen Sie eine Instanz von [Train Model](train-model.md) (Trainieren eines Modells) hinzu, und verwenden Sie die Ausgabe von [Filter Based Feature Selection](filter-based-feature-selection.md) als Eingabe für das Training.

    > [!IMPORTANT]
    > Weil die Featurerelevanz auf den Werten in der Spalte basiert, können Sie nicht im Voraus wissen, welche Spalten unter Umständen als Eingabe für [Train Model](train-model.md) verfügbar sind.  
5. Fügen Sie eine Instanz des Moduls „Select Columns Transform“ hinzu. 

    Mit diesem Schritt wird eine Spaltenauswahl als Transformation generiert, die gespeichert oder auf andere Datasets angewendet werden kann. In diesem Schritt wird sichergestellt, dass die Spalten, die bei der Featureauswahl ermittelt wurden, gespeichert werden, damit sie von anderen Modulen wiederverwendet werden können.

6. Fügen Sie das Modul [Score Model](score-model.md) (Bewerten eines Modells) hinzu. 

   *Stellen Sie keine Verbindung mit dem Eingabedataset her.* Fügen Sie stattdessen das Modul [Apply Transformation](apply-transformation.md) hinzu, und verbinden Sie mit der Ausgabe der Featureauswahltransformation.

   > [!IMPORTANT]
   > Wenn Sie [Filter Based Feature Selection](filter-based-feature-selection.md) auf das Bewertungsdataset anwenden, können Sie nicht erwarten, dass dieselben Ergebnisse erzielt werden. Da die Featureauswahl auf Werten basiert, wird dabei unter Umständen ein anderer Satz von Spalten ausgewählt, was dazu führt, dass der Bewertungsvorgang fehlschlägt.
7. Ausführen der Pipeline.

Durch diese Vorgehensweise mit Speichern und anschließendem Anwenden einer Spaltenauswahl wird sichergestellt, dass für das Training und die Bewertung dasselbe Datenschema verfügbar ist.


## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich die [Gruppe der verfügbaren Module](module-reference.md) für den Azure Machine Learning Service an. 
