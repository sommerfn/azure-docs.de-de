---
title: Verwenden von automatisiertem ML zum Erstellen und Bereitstellen von Machine Learning-Modellen
titleSuffix: Azure Machine Learning service
description: Erstellen, Verwalten und Bereitstellen automatisierter Experimente mit maschinellem Lernen im Azure-Portal
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: nibaccam
author: tsikiksr
manager: cgronlun
ms.reviewer: nibaccam
ms.date: 08/02/2019
ms.openlocfilehash: 2f6d45613120d02dd96a9fe0a14ce388d20cf0c6
ms.sourcegitcommit: 5b76581fa8b5eaebcb06d7604a40672e7b557348
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2019
ms.locfileid: "68990582"
---
# <a name="create-explore-and-deploy-automated-machine-learning-experiments-in-the-azure-portal-preview"></a>Erstellen, Untersuchen und Bereitstellen automatisierter Experimente mit maschinellem Lernen im Azure-Portal (Vorschauversion)

 In diesem Artikel erfahren Sie, wie Sie automatisierte Experimente mit maschinellem Lernen im Azure-Portal ohne eine einzige Zeile Code erstellen, untersuchen und bereitstellen. Automatisiertes Machine Learning automatisiert den Prozess der Auswahl des besten Algorithmus für Ihre spezifischen Daten, sodass Sie schnell ein Modell für Machine Learning erstellen können. [Weitere Informationen zu automatisiertem Machine Learning](concept-automated-ml.md).

 Wenn Sie eine eher codebasierte Erfahrung bevorzugen, können Sie auch [Ihre automatisierten Machine Learning-Experimente in Python](how-to-configure-auto-train.md) mit dem [Azure Machine Learning SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) konfigurieren.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein kostenloses Konto erstellen, bevor Sie beginnen. Probieren Sie noch heute die [kostenlose oder kostenpflichtige Version von Azure Machine Learning Service](https://aka.ms/AMLFree) aus.

* Ein Azure Machine Learning-Dienstbereich. Siehe [Erstellen eines Azure Machine Learning Service-Arbeitsbereichs](how-to-manage-workspace.md).

## <a name="get-started"></a>Erste Schritte

Navigieren Sie zum linken Bereich des Arbeitsbereichs. Wählen Sie „Automated Machine Learning“ (Automatisiertes Machine Learning) im Abschnitt „Authoring (Preview)“ (Erstellen (Vorschau)) aus.

![Navigationsbereich im Azure-Portal](media/how-to-create-portal-experiments/nav-pane.png)

 Wenn Sie zum ersten Mal mit automatisiertem Machine Learning experimentieren, wird der Bildschirm **Welcome to Automated Machine Learning** (Willkommen beim automatisierten maschinellen Lernen) angezeigt. 

Andernfalls wird das Dashboard für **automatisiertes maschinelles Lernen** mit einer Übersicht über alle Ihre automatisierten Experimente mit maschinellem Lernen angezeigt (einschließlich der mit dem SDK erstellten). Hier können Sie Ihre Ausführungen nach Datum, Experimentname und Ausführungsstatus filtern und untersuchen.

## <a name="create-an-experiment"></a>Erstellen eines Experiments

Wählen Sie **Experiment erstellen** aus, und füllen Sie das Formular **Create a new automated machine learning experiment** (Neues Experiment mit automatisiertem maschinellem Lernen erstellen) aus.

1. Geben Sie einen eindeutigen Namen für das Experiment ein.

1. Wählen Sie einen Compute für den Datenprofilerstellungs- und Trainingsauftrag aus. Eine Liste Ihrer vorhandenen Computes ist in der Dropdownliste verfügbar. Folgen Sie den Anweisungen in Schritt 3, um einen neuen Compute zu erstellen.

1. Wählen Sie **Create a new compute** (Neue Computeressource erstellen) aus, um den Computekontext für dieses Experiment zu konfigurieren.

    Feld|BESCHREIBUNG
    ---|---
    Computename| Geben Sie einen eindeutigen Namen ein, der Ihren Computekontext identifiziert.
    Größe des virtuellen Computers| Wählen Sie die Größe für Ihren Computes aus.
    Zusätzliche Einstellungen| *Min node* (Mindestanzahl von Knoten): Geben Sie die Mindestanzahl von Knoten für Ihren Compute ein. Die Mindestanzahl von Knoten für AML-Compute ist 0. Zum Aktivieren der Datenprofilerstellung müssen Sie über mindestens einen Knoten verfügen. <br> *Max node* (Maximale Anzahl von Knoten): Geben Sie die maximale Anzahl von Knoten für Ihren Compute ein. Der Standardwert ist 6 Knoten für einen AML-Compute.

      Klicken Sie auf **Erstellen**. Das Erstellen einer neuen Computeressource kann einige Minuten dauern.

      >[!NOTE]
      > Ihr Computename gibt an, ob für den von Ihnen ausgewählten/erstellten Compute *Profilerstellung aktiviert* ist. (Weitere Informationen zur Datenprofilerstellung finden Sie unter 7 b).

1. Wählen Sie ein Speicherkonto für Ihre Daten aus. 

1. Wählen Sie einen Speichercontainer aus.

1. Wählen Sie eine Datendatei aus Ihrem Speichercontainer aus, oder laden Sie eine Datei von Ihrem lokalen Computer in den Container hoch. Die öffentliche Vorschau unterstützt nur lokale Dateiuploads und Azure Blob Storage-Konten.
    >[!Important]
    > Anforderungen für Trainingsdaten:
    >* Die Daten müssen in Tabellenform vorliegen.
    >* Der Wert, den Sie vorhersagen möchten (Zielspalte), muss in den Daten vorhanden sein.

    [![Auswählen einer Datendatei](media/tutorial-1st-experiment-automated-ml/select-data-file.png)](media/tutorial-1st-experiment-automated-ml/select-data-file-expanded.png#lightbox)

1. Verwenden Sie die Vorschau- und Profilregisterkarten, um Ihre Daten für dieses Experiment weiter zu konfigurieren.

    1. Geben Sie auf der Registerkarte **Vorschau** an, ob Ihre Daten Header enthalten, und wählen Sie die Features (Spalten) für das Training mit den Umschaltflächen **Eingeschlossen** in den einzelnen Featurespalten aus.

    1. Auf der Registerkarte **Profil** können Sie das [Datenprofil](#profile) nach Feature sowie die Verteilungs-, Typ- und Zusammenfassungsstatistiken (Mittelwert, Median, Maximum/Minimum usw.) der einzelnen Features anzeigen.

        >[!NOTE]
        > Die folgende Fehlermeldung wird angezeigt, wenn für den Computekontext **keine** Profilerstellung aktiviert ist: *Data profiling is only available for compute targets that are already running* (Datenprofilerstellung ist nur für Computeziele verfügbar, die bereits ausgeführt werden).

1. Wählen Sie den Trainingsauftragstyp aus: Klassifizierung, Regression oder Vorhersage.

1. Zielspalte auswählen: Dies ist die Spalte, für die Sie Vorhersagen ausführen möchten.

1. Für Vorhersagen:
    1. Wählen Sie die Zeitspalte aus: Diese Spalte enthält die zu verwendenden Zeitdaten.

    1. Wählen Sie den Vorhersagehorizont aus: Geben Sie an, wie viele Zeiteinheiten (Minuten/Stunden/Tage/Wochen/Monate/Jahre) das Modell die Zukunft vorhersagen können soll. Je weiter das Modell die Zukunft vorhersagen muss, desto ungenauer wird es. [Weitere Informationen zu Vorhersagen und zum Vorhersagehorizont](how-to-auto-train-forecast.md).

1. (Optional) Erweiterte Einstellungen: Zusätzliche Einstellungen, mit denen Sie den Trainingsauftrag besser steuern können.

    Erweiterte Einstellungen|BESCHREIBUNG
    ------|------
    Primary metric (Primäre Metrik)| Die wichtigste Metrik, die für die Bewertung Ihres Modells verwendet wird. [Weitere Informationen zur Modellmetriken](how-to-configure-auto-train.md#explore-model-metrics).
    Exit Criteria (Beendigungskriterien)| Wenn eines dieser Kriterien erfüllt ist, wird der Trainingsauftrag vor dem vollständigen Abschluss beendet. <br> *Training job time (minutes)* (Dauer des Trainingsauftrags (Minuten)): Gibt an, wie lange der Trainingsauftrag ausgeführt werden soll.  <br> *Max number of iterations* (Maximale Anzahl von Iterationen): Die maximale Anzahl von Pipelines (Iterationen), die im Trainingsauftrag getestet werden. Der Auftrag wird nicht häufiger als die angegebene Anzahl von Iterationen ausgeführt. <br> *Metric score threshold* (Metrischer Bewertungsschwellenwert):  Die Metrikmindestbewertung für alle Pipelines. Auf diese Weise wird sichergestellt, dass Sie nicht mehr Zeit für den Trainingsauftrag aufwenden als nötig, wenn Sie eine definierte Zielmetrik verwenden, die Sie erreichen möchten.
    Preprocessing (Vorverarbeitung)| Aktivieren oder deaktivieren Sie mit dieser Option die Vorverarbeitung durch automatisiertes Machine Learning. Vorverarbeitung umfasst die automatische Datenbereinigung, die Vorbereitung und die Transformation, um synthetische Features zu generieren. [Weitere Informationen zur Vorverarbeitung](#preprocess).
    Überprüfen| Wählen Sie eine der Optionen für Kreuzvalidierung aus, die im Trainingsauftrag verwendet werden soll. [Weitere Informationen zur Kreuzvalidierung](how-to-configure-auto-train.md).
    Parallelität| Wählen Sie die Multi-Core-Grenzwerte aus, die Sie verwenden möchten, wenn Multi-Core-Compute zum Einsatz kommt.
    Blocked algorithm (Blockierter Algorithmus)| Wählen Sie Algorithmen aus, die Sie aus den Trainingsauftrag ausschließen möchten.

<a name="profile"></a>

## <a name="data-profiling--summary-stats"></a>Statistiken für Datenprofilerstellung und Zusammenfassung

Sie können eine Vielzahl von Zusammenfassungsstatistiken zu Ihrem Dataset erhalten, um zu überprüfen, ob Ihr Dataset für ML bereit ist. Für nicht numerische Spalten sind nur grundlegende Statistiken wie Mindestwert, Maximalwert und Fehleranzahl enthalten. Für numerische Spalten können Sie auch ihre statistische Zeit und die geschätzten Quantile überprüfen. Unser Datenprofil umfasst Folgendes:

>[!NOTE]
> Leere Einträge werden für Features mit irrelevanten Typen angezeigt.

Statistik|BESCHREIBUNG
------|------
Feature| Der Name der Spalte, die zusammengefasst wird.
Profil| Eine Inlinevisualisierung basierend auf dem abgeleiteten Typ. Beispielsweise weisen Zeichenfolgen, boolesche Werte und Datumsangaben Wertanzahlen auf, während Dezimalwerte (numerische Werte) Näherungswerte für Histogramme besitzen. Dadurch können Sie einen schnellen Überblick über die Verteilung der Daten erhalten.
Typverteilung| Eine Inlinewertanzahl von Typen in einer Spalte. NULL-Werte sind eigene Typen, sodass diese Visualisierung für die Erkennung von ungewöhnlichen oder fehlenden Werten nützlich ist.
type|Der abgeleitete Typ der Spalte. Mögliche Werte: Zeichenfolgen, boolesche Werte, Datumsangaben und Dezimalwerte.
Min| Der Mindestwert der Spalte. Leere Einträge werden für Features angezeigt, deren Typ keine inhärente Reihenfolge aufweist (z.B. boolesche Werte).
max| Der Höchstwert der Spalte. 
Count| Die Gesamtanzahl der fehlenden und nicht fehlenden Einträge in der Spalte.
Fehlt nicht (Anzahl)| Die Anzahl der nicht fehlenden Einträge in der Spalte. Leere Zeichenfolgen und Fehler werden als Werte behandelt, damit sie nicht zur „Anzahl nicht fehlender“ beitragen.
Quantile| Die geschätzten Werte in jedem Quantil, um einen Eindruck von der Verteilung der Daten bereitzustellen.
Mittelwert| Das arithmetische Mittel der Spalte.
Standardabweichung| Das Maß der Verteilung oder Abweichung der Daten dieser Spalte.
Variance| Das Maß, wie weit die Daten dieser Spalte gegenüber dem Durchschnittswert verteilt sind. 
Schiefe| Das Maß für die Unterschiede zwischen den Daten dieser Spalte und der normalen Verteilung.
Kurtosis| Das Maß für die schweren Ränder der Daten dieser Spalte im Vergleich mit der normalen Verteilung.

<a name="preprocess"></a>

## <a name="advanced-preprocessing-options"></a>Optionen für erweiterte Vorverarbeitung

Bei der Konfiguration Ihrer Experimente können Sie die erweiterte Einstellung `Preprocess` aktivieren. Dies bedeutet, dass die folgenden Schritte der Datenvorverarbeitung und Featurisierung automatisch durchgeführt werden.

|Vorverarbeitungsschritte| BESCHREIBUNG |
| ------------- | ------------- |
|Löschen von Funktionen mit hoher Kardinalität oder ohne Varianz|Löscht diese aus Trainings- und Validierungssets. Dazu gehören Funktionen, denen alle Werte fehlen, die denselben Wert für alle Zeilen haben oder die eine sehr hohe Kardinalität (z.B. Hashwerte, IDs oder GUIDs) aufweisen.|
|Imputieren von fehlenden Werten|Bei numerischen Features werden fehlende Werte mit dem Durchschnitt der Werte in der Spalte imputiert.<br/><br/>Bei kategorischen Features werden fehlende Werte mit dem am häufigsten vorkommenden Wert imputiert.|
|Generieren zusätzlicher Funktionen|Für DateTime-Funktionen: Jahr, Monat, Tag, Tag der Woche, Tag des Jahres, Quartal, Woche des Jahres, Stunde, Minute, Sekunde.<br/><br/>Für Text-Funktionen: Ausdruckshäufigkeit basierend auf Unigrammen, Bigrammen und Trigrammen.|
|Transformieren und codieren |Numerische Features mit wenigen eindeutigen Werten werden in kategorische Features transformiert.<br/><br/>One-Hot-Codierung wird für niedrige Kardinalität kategorisch durchgeführt, für hohe Kardinalität gilt One-Hot-Hashcodierung.|
|Worteinbettungen|Ein Textfeaturizer, der Vektoren von Texttoken mithilfe eines vortrainierten Modells in Satzvektoren konvertiert. Der Einbettungsvektor jedes Worts in einem Dokument wird in einem Dokumentenfeaturevektor zusammengefasst.|
|Zielcodierungen|Bei kategorischen Features wird jede Kategorie mit gemitteltem Zielwert für Regressionsprobleme und der Klassenwahrscheinlichkeit für jede Klasse für Klassifizierungsprobleme zugeordnet. Häufigkeitsbasierte Gewichtung und k-fache Kreuzvalidierung werden angewendet, um die Überanpassung der Zuordnung und das Rauschen durch dünn besetzte Datenkategorien zu verringern.|
|Textzielcodierung|Für die Texteingabe wird ein gestapeltes lineares Modell mit Bag-of-Words verwendet, um die Wahrscheinlichkeit der einzelnen Klassen zu generieren.|
|Gewichtung der Beweise (Weight of Evidence, WoE)|Berechnet den WoE-Wert als ein Maß für die Korrelation von kategorischen Spalten zur Zielspalte. Dieser Wert wird als Logarithmus des Verhältnisses von In-Class- zu Out-of-Class-Wahrscheinlichkeiten berechnet. Dieser Schritt gibt eine numerische Featurespalte pro Klasse aus und erspart die Notwendigkeit, fehlende Werte und Ausreißerbehandlung explizit zu berechnen.|
|Clusterabstand|Trainiert ein K-Means-Clusteringmodell für alle numerischen Spalten.  Gibt k neue Features aus, ein neues numerisches Feature pro Cluster, das den Abstand jeder Stichprobe zum Schwerpunkt jedes Clusters enthält.|

## <a name="run-experiment-and-view-results"></a>Ausführen des Experiments und Anzeigen der Ergebnisse

Wählen Sie **Start** aus, um das Experiment auszuführen. Der das Experiment vorbereitende Prozess nimmt einige Minuten in Anspruch.

### <a name="view-experiment-details"></a>Anzeigen von Details zum Experiment

Sobald die Vorbereitungsphase des Experiments abgeschlossen wurde, wird der Bildschirm „Ausführungsdetails“ ausgefüllt. Dieser Bildschirm enthält eine vollständige Liste der erstellten Modelle. Standardmäßig steht das Modell, das anhand der ausgewählten Metrik die höchste Bewertung erhält, in der Liste ganz oben. Wenn der Trainingsauftrag weitere Modelle testet, werden diese der Iterationsliste und dem Diagramm hinzugefügt. Verwenden Sie das Iterationsdiagramm, um einen schnellen Vergleich der Metriken für die bisher generierten Modelle zu erhalten.

Bei Trainingsaufträgen kann es eine Weile dauern, bis die Ausführung jeder Pipeline abgeschlossen wurde.

[![Dashboard mit den Ausführungsdetails](media/how-to-create-portal-experiments/run-details.png)](media/how-to-create-portal-experiments/run-details-expanded.png#lightbox)

### <a name="view-training-run-details"></a>Anzeigen der Details der Trainingsausführung

Führen Sie einen Drilldown der Ausgabemodelle aus, um Details zur Trainingsausführung anzuzeigen, z.B. Leistungsmetriken und Verteilungsdiagramme. [Weitere Informationen zu Diagrammen](how-to-understand-automated-ml.md).

![Details zur Iteration](media/how-to-create-portal-experiments/iteration-details.png)

## <a name="deploy-your-model"></a>Bereitstellen Ihres Modells

Sobald Ihnen das beste Modell vorliegt, ist es an der Zeit, das Modell als Webdienst bereitzustellen, um Vorhersagen für neue Daten zu erstellen.

Automatisiertes maschinelles Lernen unterstützt Sie dabei, das Modell bereitzustellen, ohne Code zu schreiben:

1. Sie haben einige Optionen für die Bereitstellung. 

    + Option 1: Um das beste Modell (entsprechend den definierten Metrikkriterien) bereitzustellen, wählen Sie „Deploy Best Model“ (Bestes Modell bereitstellen) auf der Seite „Ausführungsdetails“ aus.

    + Option 2: Um eine bestimmte Modelliteration aus diesem Experiment bereitzustellen, führen Sie einen Drilldown für das Modell aus, um dessen Seite mit Ausführungsdetails zu öffnen, und wählen Sie „Modell bereitstellen“ aus.
1. Füllen Sie den Bereich **Modell bereitstellen** aus.

    Feld| Wert
    ----|----
    „Deployment name“ (Bereitstellungsname)| Geben Sie einen eindeutigen Namen für die Bereitstellung ein.
    „Deployment description“ (Bereitstellungsbeschreibung)| Geben Sie eine Beschreibung ein, um den Zweck dieser Bereitstellung genauer anzugeben.
    „Scoring script“ (Bewertungsskript)| Erstellen Sie automatisch eine Bewertungsdatei, oder laden Sie eine eigene hoch. [Weitere Informationen zu Bewertungsskripts](how-to-deploy-and-where.md#script)
    Environment script (Umgebungsskript)| Erstellen Sie automatisch eine Umgebungsdatei, oder laden Sie eine eigene hoch.
    >[!Important]
    > Ein Dateiname muss weniger als 32 Zeichen haben und muss mit einem alphanumerischen Zeichen beginnen und enden. Dazwischen darf ein Name Bindestriche, Unterstriche, Punkte und alphanumerische Zeichen enthalten. Leerzeichen sind nicht zulässig.

1. Klicken Sie auf **Bereitstellen**. Die Bereitstellung kann bis zu 20 Minuten dauern.

    Wenn die Bereitstellung erfolgreich abgeschlossen wurde, wird die folgende Meldung angezeigt.

    ![Bereitstellung abgeschlossen](media/tutorial-1st-experiment-automated-ml/deploy-complete-status.png) 

Nun haben Sie einen einsatzfähigen Webdienst, mit dem Vorhersagen generiert werden können!

## <a name="next-steps"></a>Nächste Schritte

* Probieren Sie das End-to-End-[Tutorial zum Erstellen Ihres ersten Experiments mit automatisiertem ML mit Azure Machine Learning](tutorial-first-experiment-automated-ml.md) aus. 
* [Weitere Informationen zu automatisiertem Machine Learning](concept-automated-ml.md) und Azure Machine Learning.
* [Grundlagen von Ergebnissen des automatisierten maschinellen Lernens](how-to-understand-automated-ml.md)
* [Informationen zum Nutzen eines Webdiensts](https://docs.microsoft.com/azure/machine-learning/service/how-to-consume-web-service)
