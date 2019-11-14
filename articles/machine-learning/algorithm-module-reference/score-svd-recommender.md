---
title: 'Score SVD Recommender: Modulreferenz'
titleSuffix: Azure Machine Learning service
description: Hier erfahren Sie, wie Sie das Modul „Score SVD Recommender“ (SVD-Empfehlungsmodul bewerten) im Azure Machine Learning-Dienst verwenden, um Empfehlungsvorhersagen für ein Dataset zu bewerten.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 10/10/2019
ms.openlocfilehash: be590ba04de5c8ee671e7e865d04b4ea6c638c00
ms.sourcegitcommit: bc7725874a1502aa4c069fc1804f1f249f4fa5f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73717221"
---
# <a name="score-svd-recommender"></a>Bewerten des SVD-Empfehlungsmoduls

In diesem Artikel wird die Verwendung des Moduls „Score SVD Recommender“ (SVD-Empfehlungsmodul bewerten) im Azure Machine Learning-Designer (Vorschauversion) beschrieben. Verwenden Sie dieses Modul zum Erstellen von Vorhersagen mit einem trainierten Empfehlungsmodell, das auf dem SVD-Algorithmus (Singular Value Decomposition, Singulärwertzerlegung) basiert.

Das SVD-Empfehlungsmodel kann zwei verschiedene Arten von Vorhersagen generieren:

- [Vorhersagen von Bewertungen für einen bestimmten Benutzer und ein bestimmtes Element](#prediction-of-ratings)
- [Empfehlen von Elementen für einen Benutzer](#recommendations-for-users)

Wenn Sie die zweite Art von Vorhersage erstellen möchten, können Sie einen der folgenden Modi verwenden:

- **Produktionsmodus:** In diesem Modus werden alle Benutzer oder Elemente berücksichtigt. Er kommt in der Regel in einem Webdienst zum Einsatz.

  Sie können Scores auch für neue Benutzer erstellen, nicht nur für Benutzer, die während des Trainings angezeigt werden. Weitere Informationen finden Sie in den [technischen Hinweisen](#technical-notes). 

- **Auswertungsmodus:** In diesem Modus wird eine kleinere Gruppe auswertbarer Benutzer oder Elemente verwendet. Er kommt für gewöhnlich bei der Erstellung von Pipelines zum Einsatz.

Weitere Informationen zum SVD-Empfehlungsalgorithmus finden Sie in der Forschungsarbeit [Matrixfaktorisierungstechniken für Empfehlungssysteme](https://datajobs.com/data-science-repo/Recommender-Systems-[Netflix].pdf).

## <a name="how-to-configure-score-svd-recommender"></a>Konfigurieren von Score SVD Recommender

Dieses Modul unterstützt zwei Arten von Vorhersagen, für die jeweils unterschiedliche Anforderungen gelten. 

###  <a name="prediction-of-ratings"></a>Vorhersage von Bewertungen

Bei der Vorhersage von Bewertungen berechnet das Modell anhand der Trainingsdaten, wie ein Benutzer auf ein bestimmtes Element reagiert. Die Eingabedaten für die Bewertung müssen daher sowohl einen Benutzer als auch das zu bewertende Element enthalten.

1. Fügen Sie Ihrer Pipeline ein trainiertes Empfehlungsmodell hinzu, und verbinden Sie es mit **Train SVD Recommender**. Das Modell muss unter Verwendung des Moduls [Train SVD Recommender](train-SVD-recommender.md) (SVD-Empfehlungsmodul trainieren) erstellt werden.

2. Wählen Sie für **Recommender prediction kind** (Vorhersageart des Empfehlungsmoduls) die Option **Rating Prediction** (Bewertungsvorhersage) aus. Weitere Parameter sind nicht erforderlich.

3. Fügen Sie die Daten hinzu, für die Sie Vorhersagen erstellen möchten, und verbinden Sie sie mit dem **zu bewertenden Dataset**.

   Damit das Modell Bewertungen vorhersagen kann, muss das Eingabedataset Benutzer-Element-Paare enthalten.

   Das Dataset kann eine optionale dritte Spalte mit Bewertungen für das Benutzer-Element-Paar aus der ersten und zweiten Spalte enthalten. Diese dritte Spalte wird bei der Vorhersage jedoch ignoriert.

4. Ausführen der Pipeline.

### <a name="results-for-rating-predictions"></a>Ergebnisse der Bewertungsvorhersagen 

Das Ausgabedataset enthält drei Spalten: eine für die Benutzer, eine für die Elemente und eine für die vorhergesagte Bewertung für die einzelnen Benutzer und Elemente aus der Eingabe.

###  <a name="recommendations-for-users"></a>Empfehlungen für Benutzer 

Zum Empfehlen von Elementen für Benutzer geben Sie eine Liste von Benutzern und Elementen als Eingabe an. Aus diesen Daten generiert das Modell anhand seiner Kenntnisse über vorhandene Elemente und Benutzer eine Liste von Elementen, die für den jeweiligen Benutzer wahrscheinlich attraktiv sind. Die Anzahl zurückgegebener Empfehlungen kann angepasst werden. Außerdem können Sie einen Schwellenwert für die Anzahl vorheriger Empfehlungen festlegen, die zum Generieren einer Empfehlung erforderlich sind.

1. Fügen Sie Ihrer Pipeline ein trainiertes Empfehlungsmodell hinzu, und verbinden Sie es mit **Train SVD Recommender**.  Das Modell muss unter Verwendung des Moduls [Train SVD Recommender](train-svd-recommender.md) (SVD-Empfehlungsmodul trainieren) erstellt werden.

2. Wenn Sie einer Reihe von Benutzern Elemente empfehlen möchten, legen Sie **Recommender prediction kind** (Vorhersageart des Empfehlungsmoduls) auf **Item Recommendation** (Elementempfehlung) fest.

3. Geben Sie für **Recommended item selection** (Empfohlene Elementauswahl) an, ob Sie das Bewertungsmodul in der Produktion oder für die Modellauswertung verwenden. Wählen Sie einen der folgenden Werte aus:

    - **From All Items** (Aus allen Elementen): Wählen Sie diese Option aus, wenn Sie eine Pipeline für die Verwendung in einem Webdienst oder in der Produktion einrichten.  Diese Option aktiviert den *Produktionsmodus*. Das Modul generiert Empfehlungen für alle Elemente, die während des Trainings analysiert wurden.

    - **From Rated Items (for model evaluation)** (Aus bewerteten Elementen (für Modellauswertung)): Wählen Sie diese Option aus, wenn Sie ein Modell entwickeln oder testen. Diese Option aktiviert den *Auswertungsmodus*. Das Modul generiert nur Empfehlungen für die bewerteten Elemente aus dem Eingabedataset.
    
    - **From Unrated Items (to suggest new items to users)** (Aus nicht bewerteten Elementen (zum Vorschlagen neuer Elemente für Benutzer)): Wählen Sie diese Option aus, wenn das Modul nur Empfehlungen auf der Grundlage der Elemente im Trainingsdataset generieren soll, die nicht bewertet wurden. 

4. Fügen Sie das Dataset hinzu, für das Sie Vorhersagen erstellen möchten, und verbinden Sie es mit dem **zu bewertenden Dataset**.

    - Bei Verwendung von **From All Items** (Aus allen Elementen) sollte das Eingabedataset aus einer einzelnen Spalte bestehen. Diese enthält die Bezeichner von Benutzern, für die Empfehlungen generiert werden sollen.

      Das Dataset kann zwei weitere Spalten mit Element-IDs und Bewertungen enthalten, diese werden jedoch ignoriert. 

    - Bei Verwendung von **From Rated Items (for model evaluation)** (Aus bewerteten Elementen (für Modellauswertung)) muss das Eingabedataset aus Benutzer-Element-Paaren bestehen. Die erste Spalte muss die Benutzer-ID enthalten. Die zweite Spalte muss die entsprechenden Element-IDs enthalten.

      Das Dataset kann eine dritte Spalte mit Benutzer-Element-Bewertungen enthalten, diese Spalte wird jedoch ignoriert.

    - Bei Verwendung von **From Unrated Items (to suggest new items to users)** (Aus nicht bewerteten Elementen (zum Vorschlagen neuer Elemente für Benutzer)) muss das Eingabedataset aus Benutzer-Element-Paaren bestehen. Die erste Spalte muss die Benutzer-ID enthalten. Die zweite Spalte muss die entsprechenden Element-IDs enthalten.

     Das Dataset kann eine dritte Spalte mit Benutzer-Element-Bewertungen enthalten, diese Spalte wird jedoch ignoriert.

5. **Maximum number of items to recommend to a user** (Maximale Anzahl von Elementen, die einem Benutzer empfohlen werden sollen): Geben Sie die Anzahl von Elementen ein, die für jeden Benutzer zurückgegeben werden sollen. Standardmäßig werden fünf Elemente empfohlen.

6. **Minimum size of the recommendation pool per user** (Minimale Größe des Empfehlungspools pro Benutzer): Geben Sie einen Wert ein, der angibt, wie viele vorherige Empfehlungen erforderlich sind. Standardmäßig ist dieser Parameter auf **2** festgelegt. Das bedeutet, dass das Element von mindestens zwei anderen Benutzern empfohlen worden sein muss.

   Verwenden Sie diese Option nur im Auswertungsmodus. Die Option ist nicht verfügbar, wenn Sie **From All Items** (Aus allen Elementen) oder **From Unrated Items (to suggest new items to users)** (Aus nicht bewerteten Elementen (zum Vorschlagen neuer Elemente für Benutzer)) auswählen.

7.  Verwenden Sie für **From Unrated Items (to suggest new items to users)** (Aus nicht bewerteten Elementen (zum Vorschlagen neuer Elemente für Benutzer)) den dritten Eingabeport mit dem Namen **Training Data** (Trainingsdaten), um bereits bewertete Elemente aus den Vorhersageergebnissen zu entfernen.

    Um diesen Filter anzuwenden, verbinden Sie das ursprüngliche Trainingsdataset mit dem Eingabeport.

8. Ausführen der Pipeline.

### <a name="results-of-item-recommendation"></a>Ergebnisse der Elementempfehlung

Das von „Score SVD Recommender“ (SVD-Empfehlungsmodul bewerten) zurückgegebene bewertete Dataset enthält eine Liste der empfohlenen Elemente für jeden Benutzer:

- Die erste Spalte enthält die Benutzer-IDs.
- Abhängig vom Wert, den Sie für **Maximum number of items to recommend to a user** (Maximale Anzahl von Elementen, die einem Benutzer empfohlen werden sollen) festgelegt haben, werden verschiedene zusätzliche Spalten generiert. Jede Spalte enthält ein empfohlenes Element (nach Bezeichner). Die Empfehlungen werden nach der Affinität zwischen Benutzer und Element sortiert. Das Element mit der höchsten Affinität wird dabei in der Spalte **Item 1** (Element 1) platziert.

> [!WARNING]
> Dieses bewertete Dataset kann nicht mithilfe des Moduls [Evaluate Recommender](evaluate-recommender.md) (Empfehlungsmodul auswerten) ausgewertet werden.


##  <a name="technical-notes"></a>Technische Hinweise

Wenn eine Ihrer Pipelines das SVD-Empfehlungsmodul enthält und Sie das Modell in die Produktion verschieben, beachten Sie, dass sich die Verwendung des Empfehlungsmoduls im Auswertungsmodus bedeutend von der Verwendung im Produktionsmodus unterscheidet.

Die Auswertung erfordert definitionsgemäß Vorhersagen, die anhand der *grundlegende Wahrheit* in einem Testsatz überprüft werden können. Wenn Sie das Empfehlungsmodul auswerten, dürfen nur Elemente vorhergesagt werden, die im Testsatz bewertet wurden. Dies schränkt die möglichen vorhergesagten Werte ein.

Wenn Sie das Modell operationalisieren, ändern Sie in der Regel den Vorhersagemodus, sodass Empfehlungen auf der Grundlage aller möglichen Elemente getroffen werden, um die besten Vorhersagen zu erhalten. Bei vielen dieser Vorhersagen gibt es keine entsprechende grundlegende Wahrheit. Das führt dazu, dass die Genauigkeit der Empfehlung nicht wie im Rahmen von Pipelinevorgängen überprüft werden kann.


## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich die [Gruppe der verfügbaren Module](module-reference.md) für den Azure Machine Learning Service an. 
