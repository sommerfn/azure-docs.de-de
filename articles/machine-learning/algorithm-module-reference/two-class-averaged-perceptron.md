---
title: 'Entscheidungswaldregression: Modulreferenz'
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie mithilfe des Moduls „Two-Class Averaged Perceptron“ (Gemitteltes Perzeptron mit zwei Klassen) in Azure Machine Learning ein Modell für maschinelles Lernen erstellen, das auf dem „Gemitteltes Perzeptron“-Algorithmus basiert.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 10/22/2019
ms.openlocfilehash: 379cddd9654cc897b49fd085d8df55fcd77a7ce8
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73490365"
---
# <a name="two-class-averaged-perceptron-module"></a>„Two-Class Averaged Perceptron“-Modul

In diesem Artikel wird ein Modul in Azure Machine Learning-Designer (Vorschauversion) beschrieben.

Verwenden Sie dieses Modul, um ein Modell für maschinelles Lernen zu erstellen, das auf dem „Gemitteltes Perzeptron“-Algorithmus basiert.  
  
Dieser Klassifizierungsalgorithmus ist eine überwachte Lernmethode und erfordert ein *mit Tags versehenes Dataset*, das eine Bezeichnungsspalte enthält. Sie können das Modell trainieren, indem Sie das Modell und das mit Tags versehene Dataset als Eingabe für [Train Model](./train-model.md) (Modell trainieren) bereitstellen. Das trainierte Modell kann anschließend verwendet werden, um Werte für neue Eingabebeispiele vorherzusagen.  

### <a name="about-averaged-perceptron-models"></a>Informationen zu „Gemitteltes Perzeptron“-Modellen

Die *„Gemitteltes Perzeptron“-Methode* ist eine frühe und einfache Version eines neuronalen Netzwerks. Bei diesem Ansatz werden Eingaben basierend auf einer linearen Funktion in mehrere mögliche Ausgaben klassifiziert und dann mit einem Satz Gewichtungen kombiniert, die aus dem Featurevektor abgeleitet werden – daher der Name „Perzeptron“.

Die einfacheren Perzeptronmodelle eignen sich zum Lernen linear trennbarer Muster, während neuronale Netze (insbesondere tiefe neuronale Netze) komplexere Klassengrenzen modellieren können. Allerdings sind Perzeptrone schneller, und weil sie Fälle seriell verarbeiten, können Perzeptrone kontinuierlich trainiert verwendet.

## <a name="how-to-configure-two-class-averaged-perceptron"></a>Gewusst wie: Konfigurieren eines gemittelten Perzeptrons mit zwei Klassen

1.  Fügen Sie das Modul **Two-Class Averaged Perceptron** Ihrer Pipeline hinzu.  

2.  Geben Sie an, wie das Modell trainiert werden soll, indem Sie die Option **Create trainer mode** (Trainermodus erstellen) aktivieren.  
  
    -   **Single Parameter** (Einzelner Parameter): Wenn Sie wissen, wie Sie das Modell konfigurieren möchten, geben Sie eine bestimmte Menge von Werten als Argumente an.
  
3.  Geben Sie für **Learning rate** (Lernrate) einen Wert für die *Lernrate* an. Die Lernratenwerte bestimmen die Größe des Schritts, der jedes Mal, wenn das Modell getestet und korrigiert wird, beim stochastischen Gradientenverfahren verwendet wird.
  
     Indem Sie die Rate verkleinern, testen Sie das Modell häufiger, mit dem Risiko, dass Sie auf einem lokalen Plateau hängen bleiben. Mit größeren Schritten können Sie schneller konvergieren, mit dem Risiko, den echten Mindestwert zu unterschreiten.
  
4.  Geben Sie für **Maximum number of iterations** (Maximale Anzahl von Iterationen) die Häufigkeit ein, mit der der Algorithmus die Trainingsdaten überprüfen soll.  
  
     Ein frühes Stoppen bietet oft eine bessere Generalisierung. Ein Erhöhen der Anzahl von Iterationen verbessert die Anpassung, mit dem Risiko der Überanpassung.
  
5.  Geben Sie für **Random number seed** (Zufällig gewählter Startwert) optional einen ganzzahligen Wert ein, der als Startwert verwendet wird. Es empfiehlt sich, einen Startwert zu verwenden, wenn Sie die ausführungsübergreifende Reproduzierbarkeit der Pipeline sicherstellen möchten.  
  
1.  Stellen Sie eine Verbindung eines Trainingsdatasets mit einem der Trainingsmodule her:
  
    -   Wenn Sie **Create trainer mode** (Trainermodus erstellen) auf **Single Parameter** (Einzelner Parameter) festlegen, müssen Sie das Modul [Train Model](train-model.md) (Trainieren des Modells) verwenden.

## <a name="results"></a>Ergebnisse

Nach Abschluss des Trainings:

+ Um eine Zusammenfassung der Parameter des Modells sowie der aus dem Training gewonnenen Featuregewichtungen anzuzeigen, klicken Sie mit der rechten Maustaste auf die Ausgabe von [Train Model](./train-model.md).


## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich die [Gruppe der verfügbaren Module](module-reference.md) für Azure Machine Learning an. 