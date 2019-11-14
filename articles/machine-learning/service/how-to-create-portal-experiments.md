---
title: Erstellen und Bereitstellen automatisierter ML-Modelle
titleSuffix: Azure Machine Learning
description: Verwenden Sie Azure Machine Learning-Studio, um automatisierte Machine Learning-Experimenten zu erstellen, zu untersuchen und bereitzustellen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: nibaccam
author: tsikiksr
manager: cgronlun
ms.reviewer: nibaccam
ms.date: 11/04/2019
ms.openlocfilehash: 25e775cf7bfd415768144b28ab2ca6989f360edd
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73818491"
---
# <a name="create-explore-and-deploy-automated-machine-learning-experiments-with-azure-machine-learning-studio"></a>Erstellen, Untersuchen und Bereitstellen von automatisierten Machine Learning-Experimenten mit Azure Machine Learning-Studio
[!INCLUDE [applies-to-skus](../../../includes/aml-applies-to-enterprise-sku.md)]

 In diesem Artikel erfahren Sie, wie Sie automatisierte Machine Learning-Experimente in Azure Machine Learning-Studio ohne eine einzige Zeile Code erstellen, untersuchen und bereitstellen. Automatisiertes Machine Learning automatisiert den Prozess der Auswahl des besten Algorithmus für Ihre spezifischen Daten, sodass Sie schnell ein Modell für Machine Learning erstellen können. [Weitere Informationen zu automatisiertem Machine Learning](concept-automated-ml.md).

 Wenn Sie eine eher codebasierte Erfahrung bevorzugen, können Sie auch [Ihre automatisierten Machine Learning-Experimente in Python](how-to-configure-auto-train.md) mit dem [Azure Machine Learning SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) konfigurieren.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein kostenloses Konto erstellen, bevor Sie beginnen. Probieren Sie die [kostenlose oder kostenpflichtige Version von Azure Machine Learning](https://aka.ms/AMLFree) noch heute aus.

* Ein Azure Machine Learning-Arbeitsbereich mit dem Typ **Enterprise Edition**. Weitere Informationen finden Sie unter [Erstellen eines Azure Machine Learning-Arbeitsbereichs](how-to-manage-workspace.md).  Informationen zum Upgraden eines vorhandenen Arbeitsbereichs auf Enterprise Edition finden Sie unter [Upgrade auf Enterprise Edition](how-to-manage-workspace.md#upgrade).

## <a name="get-started"></a>Erste Schritte

1. Melden Sie sich bei [Azure Machine Learning-Studio](https://ml.azure.com) an. 

1. Wählen Sie Ihr Abonnement und Ihren Arbeitsbereich aus. 

1. Navigieren Sie zum linken Bereich. Wählen Sie im Abschnitt **Erstellen** die Option **Automatisiertes ML** aus.

[![Navigationsbereich von Azure Machine Learning-Studio](media/how-to-create-portal-experiments/nav-pane.png)](media/how-to-create-portal-experiments/nav-pane-expanded.png)

 Wenn Sie zum ersten Mal Experimente ausführen, werden eine leere Liste und Links zur Dokumentation angezeigt. 

Andernfalls wird eine Liste ihrer letzten automatisierten Machine Learning-Experimente angezeigt, einschließlich derjenigen, die mit dem SDK erstellt wurden. 

## <a name="create-and-run-experiment"></a>Erstellen und Ausführen eines Experiments

1. Wählen Sie **+ Experiment erstellen** aus, und füllen Sie das Formular aus.

1. Wählen Sie ein Dataset in Ihrem Speichercontainer aus, oder erstellen Sie ein neues Dataset. Datasets können aus lokalen Dateien, Web-URLs, Datenspeichern oder Azure Open Datasets erstellt werden. 

    >[!Important]
    > Anforderungen für Trainingsdaten:
    >* Die Daten müssen in Tabellenform vorliegen.
    >* Der Wert, den Sie vorhersagen möchten (Zielspalte), muss in den Daten vorhanden sein.

    1. Um ein neues Dataset aus einer Datei auf Ihrem lokalen Computer zu erstellen, wählen Sie **Durchsuchen** aus, und wählen Sie dann die Datei aus. 

    1. Weisen Sie Ihrem Dataset einen eindeutigen Namen zu, und geben Sie eine optionale Beschreibung ein. 

    1. Wählen Sie **Weiter** aus, um die Datei in den Standardspeichercontainer hochzuladen, der automatisch mit Ihrem Arbeitsbereich erstellt wird, oder wählen Sie einen Speichercontainer aus, den Sie für das Experiment verwenden möchten. 

    1. Überprüfen Sie das Formular **Einstellungen und Vorschau** auf Genauigkeit. Das Formular wird ausgehend vom Dateityp intelligent aufgefüllt. 

        Feld| BESCHREIBUNG
        ----|----
        Dateiformat| Definiert das Layout und den Typ der in einer Datei gespeicherten Daten.
        Trennzeichen| Mindestens ein Zeichen zum Angeben der Grenze zwischen separaten, unabhängigen Regionen in Nur-Text- oder anderen Datenströmen.
        Codieren| Gibt an, welche Bit-zu-Zeichen-Schematabelle verwendet werden soll, um Ihr Dataset zu lesen.
        Spaltenüberschriften| Gibt an, wie die Header des Datasets, sofern vorhanden, behandelt werden.
        Zeilen überspringen | Gibt an, wie viele Zeilen im Dataset übersprungen werden.
    
        Klicken Sie auf **Weiter**.

    1. Das Formular **Schema** wird auf intelligente Weise entsprechend den auf dem Formular **Einstellungen und Vorschau** ausgewählten Optionen aufgefüllt. Konfigurieren Sie hier den Datentyp für jede Spalte, überprüfen Sie die Spaltennamen, und wählen Sie für die Spalten, die nicht in Ihr Experiment eingeschlossen werden sollen, die Option **Nicht einschließen** aus. 
            
        Wählen Sie **Weiter** aus.

    1. Das Formular **Details bestätigen** ist eine Zusammenfassung der Informationen, die zuvor in die Formulare **Grundlegende Infos** und **Einstellungen und Vorschau** eingetragen wurden. Sie haben auch die Möglichkeit, ein Profil für Ihr Dataset zu erstellen, indem Sie ein Compute mit aktivierter Profilerstellung verwenden. Weitere Informationen zur [Datenprofilerstellung](#profile).

        Klicken Sie auf **Weiter**.
1. Wählen Sie Ihr neu erstelltes Dataset aus, sobald es angezeigt wird. Sie können auch eine Vorschau des Datasets und der Stichprobenstatistiken anzeigen. 

1. Geben Sie auf dem Formular **Ausführung konfigurieren** einen eindeutigen Experimentnamen ein.

1. Wählen Sie eine Zielspalte aus: Dies ist die Spalte, für die Sie Vorhersagen ausführen möchten.

1. Wählen Sie einen Compute für den Datenprofilerstellungs- und Trainingsauftrag aus. Eine Liste Ihrer vorhandenen Computes ist in der Dropdownliste verfügbar. Folgen Sie den Anweisungen in Schritt 7, um einen neuen Compute zu erstellen.

1. Wählen Sie **Create a new compute** (Neue Computeressource erstellen) aus, um den Computekontext für dieses Experiment zu konfigurieren.

    Feld|BESCHREIBUNG
    ---|---
    Computename| Geben Sie einen eindeutigen Namen ein, der Ihren Computekontext identifiziert.
    Größe des virtuellen Computers| Wählen Sie die Größe für Ihren Computes aus.
    Min/Max nodes (Min./Max. Knoten) (unter „Erweiterte Einstellungen“)| Um ein Datenprofil zu erstellen, müssen Sie mindestens einen Knoten angeben. Geben Sie die maximale Anzahl von Knoten für Ihren Compute ein. Der Standardwert ist 6 Knoten für einen AML-Compute.
    
    Klicken Sie auf **Erstellen**. Das Erstellen einer neuen Computeressource kann einige Minuten dauern.

    >[!NOTE]
    > Ihr Computename gibt an, ob für den von Ihnen ausgewählten/erstellten Compute *Profilerstellung aktiviert* ist. (Weitere Informationen finden Sie im Abschnitt [Datenprofilerstellung](#profile).)

    Klicken Sie auf **Weiter**.

1. Wählen Sie auf dem Formular **Aufgabentyp und Einstellungen** den Aufgabentyp aus: Klassifizierung, Regression oder Prognose (Vorhersage). Weitere Informationen finden Sie unter [Definieren einer Aufgabe für maschinelles Lernen](how-to-define-task-type.md).

    1. Für eine Klassifizierung können Sie auch Deep Learning aktivieren, das für Textmerkmalserstellungen verwendet wird.

    1. Für Vorhersagen:
        1. Wählen Sie die Zeitspalte aus: Diese Spalte enthält die zu verwendenden Zeitdaten.

        1. Wählen Sie den Vorhersagehorizont aus: Geben Sie an, wie viele Zeiteinheiten (Minuten/Stunden/Tage/Wochen/Monate/Jahre) das Modell die Zukunft vorhersagen können soll. Je weiter das Modell die Zukunft vorhersagen muss, desto ungenauer wird es. [Weitere Informationen zu Vorhersagen und zum Vorhersagehorizont](how-to-auto-train-forecast.md).

1. (Optional) Weitere Konfigurationen: zusätzliche Einstellungen, mit denen Sie den Trainingsauftrag besser steuern können. Andernfalls werden die Standardwerte auf Basis der Experimentauswahl und -daten angewendet. 

    Zusätzliche Konfigurationen|BESCHREIBUNG
    ------|------
    Primary metric (Primäre Metrik)| Die wichtigste Metrik, die für die Bewertung Ihres Modells verwendet wird. [Weitere Informationen zur Modellmetriken](how-to-configure-auto-train.md#explore-model-metrics).
    Automatic featurization (Automatische Merkmalserstellung)| Aktivieren oder deaktivieren Sie mit dieser Option die Vorverarbeitung durch automatisiertes Machine Learning. Vorverarbeitung umfasst die automatische Datenbereinigung, die Vorbereitung und die Transformation, um synthetische Features zu generieren. [Weitere Informationen zur Vorverarbeitung](#preprocess).
    Blocked algorithm (Blockierter Algorithmus)| Wählen Sie Algorithmen aus, die Sie aus den Trainingsauftrag ausschließen möchten.
    Exit criterion (Beendigungskriterium)| Wenn eines dieser Kriterien erfüllt ist, wird der Trainingsauftrag beendet. <br> *Training job time (hours)* Trainingsauftragszeit (Stunden): Gibt an, wie lange der Trainingsauftrag ausgeführt werden soll. <br> *Metric score threshold* (Metrischer Bewertungsschwellenwert):  Die Metrikmindestbewertung für alle Pipelines. Auf diese Weise wird sichergestellt, dass Sie nicht mehr Zeit für den Trainingsauftrag aufwenden als nötig, wenn Sie eine definierte Zielmetrik verwenden, die Sie erreichen möchten.
    Überprüfen| Wählen Sie eine der Optionen für Kreuzvalidierung aus, die im Trainingsauftrag verwendet werden soll. [Weitere Informationen zur Kreuzvalidierung](how-to-configure-auto-train.md).
    Parallelität| *Max concurrent iterations* (Maximale Anzahl gleichzeitiger Iterationen): Die maximale Anzahl von Pipelines (Iterationen), die im Trainingsauftrag getestet werden. Der Auftrag wird nicht häufiger als die angegebene Anzahl von Iterationen ausgeführt. <br> *Max cores per iteration* (Max. Anzahl von Kernen pro Iteration): Wählen Sie die Multi-Core-Grenzwerte aus, die Sie verwenden möchten, wenn Multi-Core-Compute zum Einsatz kommt.

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

|Vorverarbeitungsschritte&nbsp;| BESCHREIBUNG |
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

Wählen Sie **Start** aus, um das Experiment auszuführen. Der Vorgang zum Vorbereiten eines Experiments kann bis zu 10 Minuten dauern. Ein Ausführen von Trainingsaufträgen kann für jede Pipeline weitere 2 bis 3 Minuten beanspruchen.

### <a name="view-experiment-details"></a>Anzeigen von Details zum Experiment

>[!NOTE]
> Wählen Sie von Zeit zu Zeit **Aktualisieren** aus, um den Status der Ausführung anzuzeigen. 

Die Anzeige für **Ausführungsdetails** wird mit der Registerkarte **Details** geöffnet. In dieser Anzeige wird eine Zusammenfassung der Experimentausführung einschließlich des **Status der Ausführung** angezeigt. 

Die Registerkarte **Modelle** enthält eine Liste der erstellten Modelle, wobei diese nach der Metrikbewertung (Metrikscore) geordnet sind. Standardmäßig steht das Modell, das anhand der ausgewählten Metrik die höchste Bewertung erhält, in der Liste ganz oben. Während der Trainingsauftrag weitere Modelle testet, werden diese zur Liste hinzugefügt. Verwenden Sie diese Liste, um einen schnellen Vergleich der Metriken für die bisher generierten Modelle zu erhalten.

[![Dashboard mit den Ausführungsdetails](media/how-to-create-portal-experiments/run-details.png)](media/how-to-create-portal-experiments/run-details-expanded.png#lightbox)

### <a name="view-training-run-details"></a>Anzeigen der Details der Trainingsausführung

Führen Sie einen Drilldown für eines der abgeschlossenen Modelle aus, um Details zur Trainingsausführung anzuzeigen, z. B. Ausführungsmetriken auf der Registerkarte **Modelldetails** oder Leistungsdiagramme auf der Registerkarte **Visualisierungen**. [Weitere Informationen zu Diagrammen](how-to-understand-automated-ml.md).

[![Details zur Iteration](media/how-to-create-portal-experiments/iteration-details.png)](media/how-to-create-portal-experiments/iteration-details-expanded.png)

## <a name="deploy-your-model"></a>Bereitstellen Ihres Modells

Sobald Ihnen das beste Modell vorliegt, ist es an der Zeit, das Modell als Webdienst bereitzustellen, um Vorhersagen für neue Daten zu erstellen.

Automatisiertes maschinelles Lernen unterstützt Sie dabei, das Modell bereitzustellen, ohne Code zu schreiben:

1. Sie haben einige Optionen für die Bereitstellung. 

    + Option 1: Um das beste Modell (entsprechend den definierten Metrikkriterien) bereitzustellen, wählen Sie „Deploy Best Model“ (Bestes Modell bereitstellen) auf der Registerkarte „Details“ aus.

    + Option 2: Um eine bestimmte Modelliteration aus diesem Experiment bereitzustellen, führen Sie einen Drilldown für das Modell aus, um dessen Registerkarte für Modelldetails zu öffnen, und wählen Sie „Deploy model“ (Modell bereitstellen) aus.

1. Füllen Sie den Bereich **Modell bereitstellen** aus.

    Feld| Wert
    ----|----
    NAME| Geben Sie einen eindeutigen Namen für die Bereitstellung ein.
    BESCHREIBUNG| Geben Sie eine Beschreibung ein, um den Zweck dieser Bereitstellung genauer anzugeben.
    Computetyp| Wählen Sie die Art des bereitzustellenden Endpunkts aus: *Azure Kubernetes Service (AKS)* oder *Azure-Containerinstanz (ACI)* .
    NAME| *Nur für AKS:* Wählen Sie den Namen des AKS-Clusters aus, der als Ziel für die Bereitstellung verwendet werden soll.
    Authentifizierung aktivieren | Wählen Sie diese Option aus, um eine token- oder schlüsselbasierte Authentifizierung zu ermöglichen.
    Use custom deployment assets (Benutzerdefinierte Bereitstellungsressourcen verwenden)| Aktivieren Sie dieses Feature, wenn Sie Ihr eigenes Bewertungsskript und Ihre eigene Umgebungsdatei hochladen möchten. Weitere Informationen zu Bewertungsskripts finden Sie [hier](how-to-deploy-and-where.md#script).

    >[!Important]
    > Ein Dateiname muss weniger als 32 Zeichen haben und muss mit einem alphanumerischen Zeichen beginnen und enden. Dazwischen darf ein Name Bindestriche, Unterstriche, Punkte und alphanumerische Zeichen enthalten. Leerzeichen sind nicht zulässig.

    Das Menü *Erweitert* enthält Standard-Bereitstellungsfeatures wie [Datensammlung](how-to-enable-app-insights.md) und Einstellungen für die Ressourcenauslastung. Wenn Sie diese Standardeinstellungen überschreiben möchten, verwenden Sie dafür dieses Menü.

1. Klicken Sie auf **Bereitstellen**. Die Bereitstellung kann bis zu 20 Minuten dauern.

Nun haben Sie einen einsatzfähigen Webdienst, mit dem Vorhersagen generiert werden können! Sie können die Vorhersagen testen, indem Sie den Dienst über die [in Power BI integrierte Azure Machine Learning-Unterstützung](how-to-consume-web-service.md#consume-the-service-from-power-bi) abfragen.

## <a name="next-steps"></a>Nächste Schritte

* Probieren Sie das End-to-End-[Tutorial zum Erstellen Ihres ersten Experiments mit automatisiertem ML mit Azure Machine Learning](tutorial-first-experiment-automated-ml.md) aus. 
* [Weitere Informationen zu automatisiertem Machine Learning](concept-automated-ml.md) und Azure Machine Learning.
* [Grundlagen von Ergebnissen des automatisierten maschinellen Lernens](how-to-understand-automated-ml.md)
* [Informationen zum Nutzen eines Webdiensts](https://docs.microsoft.com/azure/machine-learning/service/how-to-consume-web-service)
