---
title: 'Einführung: Deep Learning im Vergleich zu maschinellem Lernen'
titleSuffix: Azure Machine Learning service
description: Erfahren Sie etwas über die Beziehung zwischen Deep Learning und maschinellem Lernen und ihre Einordnung in die Kategorie der künstlichen Intelligenz. In diesem Artikel wird beschrieben, wie sich mit Deep Learning Szenarien wie Betrugserkennung, Sprach- und Gesichtserkennung, Standpunktanalyse und Zeitreihenvorhersagen lösen lassen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: lazzeri
author: FrancescaLazzeri
ms.date: 08/07/2019
ms.openlocfilehash: 4b66d015e8a182e305a36dacaa2e082288a3e19e
ms.sourcegitcommit: bc3a153d79b7e398581d3bcfadbb7403551aa536
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2019
ms.locfileid: "68840539"
---
# <a name="introduction-to-deep-learning-vs-machine-learning"></a>Einführung: Deep Learning im Vergleich zu maschinellem Lernen

In diesem Artikel wird die Beziehung zwischen Deep Learning und maschinellem Lernen erläutert. Sie erfahren, worin sich die beiden Konzepte ähneln und unterscheiden und wie sie sich in die breitere Kategorie der künstlichen Intelligenz einordnen lassen. Schließlich wird in diesem Artikel beschrieben, wie Deep Learning auf reale Szenarien angewandt werden kann, z. B. Betrugserkennung, Sprach-und Gesichtserkennung, Standpunktanalyse und Zeitreihenvorhersagen.

## <a name="how-do-deep-learning-machine-learning-and-ai-relate-to-one-another"></a>In welcher Beziehung stehen Deep Learning, maschinelles Lernen und künstliche Intelligenz (KI) zueinander?

![Beziehungsdiagramm: KI, maschinelles Lernen und Deep Learning](./media/concept-deep-learning-vs-machine-learning/ai-vs-machine-learning-vs-deep-learning.png)

Deep Learning, maschinelles Lernen und künstliche Intelligenz unterscheiden sich folgendermaßen:

- **Deep Learning** ist ein Teilbereich des maschinellen Lernens und basiert auf künstlichen neuronalen Netzen, die einem Computer das selbstständige Trainieren ermöglichen. In diesem Fall wird der _Lernprozess_ als _Deep Learning_ bezeichnet, da die Struktur von künstlichen neuronalen Netzen aus mehreren Eingabe-, Ausgabe-und Zwischenschichten besteht. Jede Schicht enthält Einheiten, die die Eingabedaten in Informationen transformieren, damit sie auf der nächsten Schicht zum Ausführen einer bestimmten prädiktiven Aufgabe verwenden können. Dank dieser Struktur kann ein Computer durch seine eigene Datenverarbeitung lernen.

- **Maschinelles Lernen** ist ein Teilbereich der künstlichen Intelligenz, die Techniken (z. B Deep Learning) umfasst, über die Computer sich bei Aufgaben mit Erfahrung verbessern können. In diesem Fall basiert der _Lernprozess_ auf den folgenden Schritten:

    1. Versorgen eines Algorithmus mit Daten durch Bereitstellen umfangreicherer Informationen (z. B. Ausführen einer Featureextraktion)
    2. Verwenden dieser Daten zum Trainieren eines Modells
    3. Testen und Bereitstellen des Modells
    4. Nutzen des bereitgestellten Modells zum Ausführen einer bestimmten automatisierten prädiktiven Aufgabe

- **Künstliche Intelligenz (KI)** ist ein Verfahren, mit dem Computer menschliche Intelligenz imitieren können. Sie umfasst auch maschinelles Lernen. Es ist wichtig, die Beziehung zwischen _künstlicher Intelligenz, maschinellem Lernen und Deep Learning_ zu verstehen: Maschinelles Lernen ist eine Möglichkeit zum Erreichen künstlicher Intelligenz. Das heißt, dass Sie durch die Verwendung von Verfahren des maschinellen Lernens und von Deep Learning Computersysteme und Anwendungen erstellen können, die Aufgaben ausführen können, die für gewöhnlich der menschlichen Intelligenz zugeschrieben werden, z. B. visuelle Wahrnehmung, Spracherkennung, Entscheidungsfindung und Übersetzung zwischen Sprachen.

## <a name="comparison-of-deep-learning-vs-machine-learning"></a>Vergleich zwischen Deep Learning und maschinellem Lernen

Nachdem Sie sich einen Überblick über das maschinelle Lernen und Deep Learning verschafft haben, können Sie die beiden Verfahren vergleichen. Beim maschinellen Lernen muss dem Algorithmus beigebracht werden, wie eine genaue Vorhersage zu treffen ist, indem weitere Informationen bereitgestellt werden. Beim Deep Learning kann der Algorithmus dies aufgrund der Struktur des künstlichen neuronalen Netzes durch seine eigene Datenverarbeitung lernen.

In der folgenden Tabelle werden die beiden Verfahren ausführlicher verglichen:

| |Vollständig Machine Learning |Nur Deep Learning|
|---|---|---|
|  **Anzahl von Datenpunkten** | Kann kleine Datenmengen verwenden, die von Benutzern bereitgestellt werden. | Erfordert eine große Menge Trainingsdaten, um präzise Schlussfolgerungen zu treffen. |
|  **Hardwareabhängigkeiten** | Kann auf Low-End-Computern ausgeführt werden. Erfordert keine hohe Rechenleistung. | Hängt von High-End-Computern ab. Grundsätzlich wird eine große Zahl von Matrixmultiplikationsvorgängen durchgeführt. Diese Vorgänge können mithilfe einer GPU effizient optimiert werden. |
|  **Featurebereitstellung** | Feature müssen von Benutzern genau identifiziert und erstellt werden. | Lernt allgemeine Features von Daten und erstellt selbstständig neue Merkmale. |
|  **Lernansatz** | Unterteilt Aufgaben in kleine Teile und kombiniert empfangene Ergebnisse zu einer Schlussfolgerung. | Löst das Problem durchgängig. |
|  **Ausführungszeit** | Das Training nimmt im Vergleich weniger Zeit in Anspruch – von wenigen Sekunden bis zu einigen Stunden. | Das Trainieren dauert ungewöhnlich lange, da Deep Learning-Algorithmen viele Parameter umfassen. |
|  **Ausgabe** | Die Ausgabe ist normalerweise ein numerischer Wert, z. B. eine Bewertung oder Klassifizierung. | Die Ausgabe kann vielfältig sein: eine Bewertung, ein Text, ein Element oder ein Sound. |

## <a name="deep-learning-use-cases-what-problems-does-it-solve"></a>Anwendungsfälle für Deep Learning: Welche Probleme werden gelöst?

Aufgrund der Struktur des künstlichen neuronalen Netzes zeichnet sich Deep Learning dadurch aus, dass Muster in unstrukturierten Daten identifiziert werden, z. B. Bilder, Sound, Video und Text. Aus diesem Grund verändern sich herkömmliche Geschäftsprozesse in vielen Branchen durch Deep Learning schnell, z. B. Gesundheitswesen, Energie, Finanztechnologie, Transport und andere. Einige der häufigsten Anwendungen für Deep Learning werden in den folgenden Abschnitten beschrieben.

### <a name="named-entity-recognition"></a>Named-entity recognition (NER)

Eine Verwendung von Deep Learning-Netzen ist die Named-entity recognition oder Eigennamenerkennung, mit der bestimmte Arten von Informationen, z. B. Personen, Orte, Unternehmen oder Gegenstände, aus unstrukturierten nicht bezeichneten Daten extrahiert werden können. Diese Informationen können dann in einem strukturierten Schema gespeichert werden, um eine Liste von Adressen zu erstellen oder um sie als Benchmark für eine Engine zur Identitätsüberprüfung zu verwenden.

### <a name="object-detection"></a>Objekterkennung

Deep Learning wird in vielen Anwendungsfällen zur Objekterkennung angewandt. Die Objekterkennung erfolgt eigentlich in zwei Schritten: Bildklassifizierung und anschließende Bildlokalisierung. Mit der Bildklassifizierung werden die Objekte im Bild bestimmt, z. B. ein Fahrzeug oder eine Person. Mit der Bildlokalisierung wird der spezifische Standort dieser Objekte angegeben. Die Objekterkennung wird bereits beim Gaming, im Einzelhandel, im Tourismus und bei selbstfahrenden Fahrzeugen eingesetzt.

### <a name="image-caption-generation"></a>Generierung der Bildbeschriftung

Ähnlich wie bei der Bilderkennung muss bei der Bildbeschriftung im System für ein bestimmtes Bild eine Beschriftung generiert werden, die den Inhalt des Bilds beschreibt. Nachdem Objekte in Fotos erkannt und Bezeichnungen für diese Objekte generiert werden können, besteht der nächste Schritt darin, diese Bezeichnungen in eine kohärente Satzbeschreibung umzuwandeln. Im Allgemeinen beinhalten die Systeme die Verwendung sehr umfangreicher Convolutional Neural Networks (CNN) für die Objekterkennung in den Fotos und anschließend ein Recurrent Neural Network (RNN), um die Bezeichnungen in einen kohärenten Satz umzuwandeln.

### <a name="machine-translation"></a>Maschinelle Übersetzung

Bei der maschinellen Übersetzung werden Wörter, Ausdrücke oder Sätze aus einer Sprache automatisch in eine andere Sprache übersetzt. Die automatische maschinelle Übersetzung gibt es schon seit geraumer Zeit, mit Deep Learning werden jedoch beste Ergebnisse in zwei bestimmten Bereichen erzielt: bei der automatischen Übersetzung von Text (und bei der Spracherkennung) und bei der automatischen Übersetzung von Bildern. Mit der richtigen Datentransformation kann ein Deep Learning-Netz Text-, Audio- und Bildsignale verstehen. Die maschinelle Übersetzung kann verwendet werden, um Tonausschnitte in größeren Audiodateien zu identifizieren und gesprochenen Text oder Bilder in Text zu transkribieren.

### <a name="text-analytics"></a>Textanalyse

Eine wichtige Aufgabe, die mit Deep Learning ausgeführt werden kann, ist eDiscovery. Unternehmen nutzen Deep Learning-basierte Textanalysen zur Erkennung von Insidergeschäften und zur Einhaltung gesetzlicher Vorschriften. Bei Hedgefonds werden Textanalysen verwendet, um einen Drilldown in umfangreiche Dokumentrepositorys auszuführen und so Einblicke in die zukünftige Anlageperformance und Marktstimmung zu erhalten. Der Anwendungsfall für Deep Learning-basierte Textanalysen beruht auf der Möglichkeit, sehr große Mengen an Textdaten zu analysieren, um Analysen durchzuführen oder Aggregationen zu erzielen.

## <a name="what-are-artificial-neural-networks"></a>Was sind künstliche neuronale Netze?

Bei einem künstlichen neuronalen Netz handelt es sich um ein Deep Learning-Modell, das daran angelehnt ist, wie biologische neuronale Netze im menschlichen Gehirn Informationen verarbeiten. Derzeit werden mehrere Arten von künstlichen neuronalen Netzen verwendet. Die gängigsten Typologien künstlicher neuronaler Netze werden im Folgenden erläutert.

### <a name="feedforward-neural-network"></a>Neuronales feedforward-Netz

Das neuronale feedforward-Netz ist der einfachste Typ des künstlichen neuronalen Netzes, bei dem die Informationen von der Eingabeschicht nur in einer Richtung an die Ausgabeschicht geleitet werden. In neuronalen feedforward-Netzen werden Eingaben transformiert, indem sie eine Reihe von Zwischenschichten durchlaufen. Jede Schicht besteht aus einer Reihe von Neuronen, wobei jede Schicht vollständig mit allen Neuronen in der vorherigen Schicht verbunden ist. Schließlich gibt es eine letzte vollständig verbundene Schicht – die Ausgabeschicht – die die generierten Vorhersagen darstellt.

### <a name="recurrent-neural-network"></a>Recurrent Neural Network

Recurrent Neural Networks (rekurrente neuronale Netze, RNN) sind eine weit verbreitete Art künstlicher neuronaler Netze, bei denen die Ausgabe einer Schicht gespeichert und wieder in die Eingabeschicht zurückgegeben wird, um so das Ergebnis der Schicht vorhersagen zu können. Diese neuronalen Netze verfügen über bessere Lernfähigkeiten und werden häufig für komplexere Aufgaben eingesetzt, z. B. das Erlernen von Handschrift oder die Spracherkennung.

### <a name="convolutional-neural-networks"></a>Convolutional Neural Networks

Ein Convolutional Neural Network (faltendes neuronales Netz, CNN) ist eine besonders effektive Art künstlicher neuronaler Netze mit einer einzigartigen Architektur. Die Schichten sind in drei Dimensionen organisiert: Breite, Höhe und Tiefe. Außerdem sind die Neuronen in einer Schicht nicht mit allen Neuronen in der nächsten Schicht, sondern nur mit einem kleinen Bereich der Schicht verbunden. Schließlich wird die endgültige Ausgabe auf einen einzelnen Vektor mit Wahrscheinlichkeitsbewertungen reduziert, der entlang der Tiefe organisiert ist. Diese neuronalen Netze werden in Bereichen wie z. B. der Bilderkennung und Bildklassifizierung eingesetzt.

## <a name="next-steps"></a>Nächste Schritte

In den folgenden Artikeln erfahren Sie, wie Sie Deep Learning in [Azure Machine Learning Service](/azure/machine-learning/service/) verwenden:

- [Skalierbares Trainieren und Registrieren von TensorFlow-Modellen mit Azure Machine Learning Service](how-to-train-tensorflow.md)
- [Trainieren und Registrieren von Keras-Modellen in großem Umfang mit Azure Machine Learning Service](how-to-train-keras.md)
- [Trainieren und Registrieren von PyTorch-Modellen in großem Umfang mit Azure Machine Learning Service](how-to-train-pytorch.md)
- [Trainieren und Registrieren von Chainer-Modellen nach Maß mit Azure Machine Learning Service](how-to-train-pytorch.md)
