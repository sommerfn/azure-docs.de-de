---
title: One-vs-All-Multiklasse
titleSuffix: Azure Machine Learning service
description: Hier erfahren Sie, wie Sie das Modul „One-vs-All-Multiklasse“ im Azure Machine Learning-Dienst verwenden, um ein Multiklassen-Klassifizierungsmodell auf der Grundlage einer Zusammenstellung von Binärklassifizierungsmodellen zu erstellen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 10/16/2019
ms.openlocfilehash: 5c59f2865e7ebf768cdd8b80e59d69359f8607c6
ms.sourcegitcommit: bc7725874a1502aa4c069fc1804f1f249f4fa5f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73717183"
---
# <a name="one-vs-all-multiclass"></a>One-vs-All-Multiklasse

In diesem Artikel erfahren Sie, wie Sie das Modul „One-vs-All-Multiklasse“ im Azure Machine Learning-Designer (Vorschauversion) verwenden. Das Ziel besteht darin, ein Klassifizierungsmodell zu erstellen, das mehrere Klassen unter Verwendung des *One-vs-All*-Ansatzes vorhersagen kann.

Dieses Modul ist nützlich zum Erstellen von Modellen, mit denen drei oder mehr mögliche Ergebnisse vorhergesagt werden, wenn das Ergebnis von kontinuierlichen oder kategorialen Vorhersagevariablen abhängt. Mit dieser Methode können Sie auch Binärklassifizierungsmethoden für Probleme verwenden, die mehrere Ausgabeklassen erfordern.

### <a name="more-about-one-versus-all-models"></a>Weitere Informationen zu One-vs-All-Modellen

Einige Klassifizierungsalgorithmen sind so gestaltet, dass sie die Verwendung von mehr als zwei Klassen ermöglichen. Bei anderen sind die möglichen Ergebnisse auf einen von zwei Werten beschränkt (binäres Modell oder Zweiklassenmodell). Allerdings gibt es auch für Binärklassifizierungsalgorithmen verschiedene Strategien, um sie an Multiklassen-Klassifizierungsaufgaben anzupassen. 

In diesem Modul wird die One-vs-All-Methode implementiert, durch die für jede der Ausgabeklassen ein binäres Modell erstellt wird. Das Modul bewertet jedes dieser binären Modelle für die einzelnen Klassen anhand seines Komplements (alle anderen Klassen im Modell), als wäre es ein Binärklassifizierungsproblem. Für die Vorhersage führt das Modul dann diese binären Klassifizierer aus und wählt die Vorhersage mit der höchsten Zuverlässigkeitsbewertung.  

Im Wesentlichen erstellt das Modul eine Zusammenstellung einzelner Modelle und führt die Ergebnisse anschließend zusammen, um ein einzelnes Modell zu erstellen, das alle Klassen vorhersagt. Jeder binäre Klassifizierer kann als Grundlage für ein One-vs-All-Modell verwendet werden.  

Ein Beispiel: Angenommen, Sie konfigurieren ein Modell vom Typ [Two-Class Support Vector Machine](two-class-support-vector-machine.md) (2-Klassen-Support Vector Machine) und stellen dieses Modell als Eingabe für das Modul „One-vs-All-Multiklasse“ bereit. Das Modul erstellt daraufhin Modelle vom Typ „Two-Class Support Vector Machine“ (2-Klassen-Support Vector Machine) für alle Elemente der Ausgabeklasse. Anschließend wendet es die One-vs-All-Methode an, um die Ergebnisse für alle Klassen zu kombinieren.  

## <a name="how-to-configure-the-one-vs-all-multiclass-classifier"></a>Konfigurieren des Klassifizierers „One-vs-All-Multiklasse“  

Dieses Modul erstellt eine Zusammenstellung von Binärklassifizierungsmodellen, um mehrere Klassen zu analysieren. Wenn Sie dieses Modul verwenden möchten, müssen Sie zunächst ein Modell vom Typ *Binärklassifizierung* konfigurieren und trainieren. 

Das binäre Modell wird mit dem Modul „One-vs-All-Multiklasse“ verbunden. Anschließend trainieren Sie die Zusammenstellung von Modellen, indem Sie [Train Model](train-model.md) (Modell trainieren) mit einem bezeichneten Trainingsdataset verwenden.

Wenn Sie die Modelle kombinieren, erstellt „One-vs-All-Multiklasse“ mehrere Binärklassifizierungsmodelle, optimiert den Algorithmus für jede Klasse und führt die Modelle anschließend zusammen. Diese Aufgaben werden auch dann ausgeführt, wenn das Trainingsdataset mehrere Klassenwerte besitzt.

1. Fügen Sie das Modul „One-vs-All-Multiklasse“ Ihrer Pipeline im Designer hinzu. Sie finden dieses Modul unter **Machine Learning – Initialisieren** in der Kategorie **Klassifizierung**.

   Der Klassifizierer „One-vs-All-Multiklasse“ hat keine eigenen konfigurierbaren Parameter. Alle Anpassungen müssen im Binärklassifizierungsmodell vorgenommen werden, das als Eingabe bereitgestellt wird.

2. Fügen Sie ein Binärklassifizierungsmodell zur Pipeline hinzu, und konfigurieren Sie dieses Modell. Sie können beispielsweise [Two-Class Support Vector Machine](two-class-support-vector-machine.md) (2-Klassen-Support Vector Machine) oder [Two-Class Boosted Decision Tree](two-class-boosted-decision-tree.md) (Verstärkter Entscheidungsbaum mit zwei Klassen) verwenden.

3. Fügen Sie Ihrer Pipeline das Modul [Train Model](train-model.md) (Modell trainieren) hinzu. Stellen Sie eine Verbindung mit dem untrainierten Klassifizierer her, der von „One-vs-All-Multiklasse“ ausgegeben wird.

4. Stellen Sie für die andere Eingabe von [Train Model](train-model.md) (Modell trainieren) eine Verbindung mit einem bezeichneten Trainingsdataset her, das über mehrere Klassenwerte verfügt.

5. Ausführen der Pipeline.

## <a name="results"></a>Ergebnisse

Nachdem das Training abgeschlossen ist, können Sie das Modell verwenden, um Multiklassenvorhersagen zu treffen.

Alternativ können Sie den untrainierten Klassifizierer an [Cross-Validate Model](cross-validate-model.md) (Kreuzvalidierung für Modell ausführen) übergeben, um eine Kreuzvalidierung anhand eines bezeichneten Validierungsdatasets auszuführen.


## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich die [Gruppe der verfügbaren Module](module-reference.md) für den Azure Machine Learning Service an. 
