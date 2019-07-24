---
title: Horizontales Hochskalieren eines Azure Data Explorer-Clusters
description: Dieser Artikel beschreibt Schritte zum horizontalen Hoch- und Herunterskalieren eines Azure Data Explorer-Clusters basierend auf sich änderndem Bedarf.
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 06/30/2019
ms.openlocfilehash: 29bfcc42462a667850f0b2e1bbda3d29cd1597ab
ms.sourcegitcommit: 1e347ed89854dca2a6180106228bfafadc07c6e5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2019
ms.locfileid: "67571264"
---
# <a name="manage-cluster-horizontal-scaling-to-accommodate-changing-demand"></a>Verwalten der horizontalen Clusterskalierung bei sich änderndem Bedarf

Die richtige Größe eines Clusters ist entscheidend für die Leistung von Azure-Daten-Explorer. Aber der Bedarf in einem Cluster kann nicht mit absoluter Genauigkeit vorhergesagt werden. Eine statische Clustergröße kann zu einer Unter- oder Überauslastung führen, was beides nicht ideal ist.

Ein besserer Ansatz ist die *Skalierung* eines Clusters, wobei je nach Bedarf Kapazitäten hinzugefügt und entfernt werden. Es gibt zwei Workflows für die Skalierung: 
* Horizontales Hoch- oder Herunterskalieren
* Vertikales (oder zentrales) Hoch- oder Herunterskalieren

In diesem Artikel wird der Workflow für die horizontale Skalierung beschrieben.

Die horizontale Skalierung ermöglicht die automatische Skalierung der Instanzenanzahl auf der Grundlage vordefinierter Regeln und Zeitpläne. Sie legen die Autoskalierungseinstellungen für Ihren Cluster im Azure-Portal fest, wie in diesem Artikel beschrieben.

## <a name="steps-to-configure-horizontal-scaling"></a>Schritte zum Konfigurieren der horizontalen Skalierung

Navigieren Sie im Azure-Portal zu Ihrer Data Explorer-Clusterressource. Wählen Sie unter der Überschrift **Einstellungen** die Option **Horizontal hochskalieren** aus. 

Wählen Sie die gewünschte Methode für die Autoskalierung aus: **Manuelle Skalierung**, **Optimized autoscale** (Optimierte Autoskalierung) oder **Custom autoscale** (Benutzerdefinierte Autoskalierung).

### <a name="manual-scale"></a>Manuelle Skalierung

„Manuelle Skalierung“ ist die Standardeinstellung bei der Clustererstellung. Bei dieser Einstellung wird eine statische Clusterkapazität verwendet, die sich nicht automatisch ändert. Die statische Kapazität kann mithilfe der Leiste festgelegt werden und ändert sich erst, wenn Sie die Erweiterungseinstellung des Clusters ändern.

   ![Manuelle Skalierung](media/manage-cluster-horizontal-scaling/manual-scale-method.png)

### <a name="optimized-autoscale"></a>Optimierte Autoskalierung

Die optimierte Autoskalierung ist die empfohlene Methode für die Autoskalierung. Schritte zum Konfigurieren der optimierten Autoskalierung:

1. Wählen Sie die Option für die optimierte Autoskalierung sowie eine Unter- und eine Obergrenze für die Anzahl von Clusterinstanzen aus, zwischen denen die automatische Skalierung erfolgen soll.
2. Klicken Sie auf "Speichern".

   ![Optimierte Autoskalierung](media/manage-cluster-horizontal-scaling/optimized-autoscale-method.png)

Nach dem Klicken auf Speichern wird die optimierte Autoskalierung gestartet, und die Aktionen des Mechanismus werden im Aktivitätsprotokoll des Clusters erfasst. Diese Autoskalierungsmethode optimiert die Leistung und die Kosten des Clusters: Wenn sich der Cluster einer Unterauslastung nähert, wird er horizontal herunterskaliert, wodurch weiterhin die gleiche Leistung zur Verfügung steht, aber weniger Kosten anfallen. Nähert sich der Cluster dagegen einer Überauslastung, wird er horizontal hochskaliert, um eine optimale Leistung zu gewährleisten.

### <a name="custom-autoscale"></a>Benutzerdefinierte Autoskalierung

Bei der benutzerdefinierten Autoskalierung kann Ihr Cluster basierend auf den von Ihnen angegebenen Metriken dynamisch skaliert werden. Die folgende Grafik zeigt den Ablauf und die Schritte zum Konfigurieren der benutzerdefinierten Autoskalierung. Weitere Details finden Sie in der Grafik.

1. Geben Sie im Feld **Name der Einstellung für die Autoskalierung** einen Namen ein, z. B. *Horizontale Skalierung: Cacheauslastung*. 

   ![Skalierungsregel](media/manage-cluster-horizontal-scaling/custom-autoscale-method.png)

2. Wählen Sie unter **Skalierungsmodus** die Option **Basierend auf einer Metrik skalieren** aus. Dieser Modus ermöglicht die dynamische Skalierung. Sie können auch **Auf eine bestimmte Anzahl von Instanzen skalieren** auswählen.

3. Wählen Sie **+ Regel hinzufügen** aus.

4. Geben Sie im Abschnitt **Skalierungsregel** auf der rechten Seite Werte für jede Einstellung an.

    **Kriterien**

    | Einstellung | Beschreibung und Wert |
    | --- | --- |
    | **Zeitaggregation** | Wählen Sie ein Aggregationskriterium aus, z. B. **Durchschnitt**. |
    | **Metrikname** | Wählen Sie die Metrik aus, auf der der Skalierungsvorgang basieren soll, z. B. **Cacheauslastung**. |
    | **Statistik zum Aggregationsintervall** | Wählen Sie zwischen **Durchschnitt**, **Minimum**, **Maximum** und **Summe** aus. |
    | **Operator** | Wählen Sie die entsprechende Option aus, z. B. **Größer als oder gleich**. |
    | **Schwellenwert** | Wählen Sie einen passenden Wert aus. Für die Cacheauslastung ist beispielsweise ist 80 % ein guter Ausgangspunkt. |
    | **Dauer (in Minuten)** | Wählen Sie eine angemessene Zeitspanne, die das System bei der Berechnung von Metriken berücksichtigt. Beginnen Sie mit dem Standardwert von 10 Minuten. |
    |  |  |

    **Aktion**

    | Einstellung | Beschreibung und Wert |
    | --- | --- |
    | **Vorgang** | Wählen Sie die entsprechende Option zum horizontalen Hoch- oder Herunterskalieren aus. |
    | **Anzahl der Instanzen** | Wählen Sie die Anzahl von Knoten oder Instanzen aus, die hinzugefügt oder entfernt werden sollen, wenn eine Metrikbedingung erfüllt ist. |
    | **Abkühlen (Minuten)** | Wählen Sie ein geeignetes Intervall für die Zeit zwischen den einzelnen Skalierungsvorgängen. Beginnen Sie mit dem Standardwert von 5 Minuten. |
    |  |  |

5. Wählen Sie **Hinzufügen**.

6. Geben Sie im Abschnitt **Instanzgrenzwerte** auf der linken Seite Werte für jede Einstellung an.

    | Einstellung | Beschreibung und Wert |
    | --- | --- |
    | **Mindestanforderungen** | Die Anzahl der Instanzen, unter die Ihr Cluster unabhängig von der Auslastung nicht herunterskaliert wird. |
    | **Maximum** | Die Anzahl der Instanzen, über die Ihr Cluster unabhängig von der Auslastung nicht hochskaliert wird. |
    | **Standard** | Die Standardanzahl von Instanzen. Diese Einstellung wird verwendet, wenn Probleme beim Lesen der Ressourcenmetriken auftreten. |
    |  |  |

7. Wählen Sie **Speichern** aus.

Sie haben nun einen horizontalen Skalierungsvorgang für Ihren Azure-Daten-Explorer-Cluster konfiguriert. Fügen Sie eine weitere Regel für einen Vorgang zum horizontalen Herunterskalieren hinzu. Wenn Sie Hilfe bei Clusterskalierungsproblemen benötigen, erstellen Sie im [Azure-Portal](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) eine Supportanfrage.

## <a name="next-steps"></a>Nächste Schritte

* [Überwachen der Azure Data Explorer-Leistung, -Integrität und -Auslastung mit Metriken](using-metrics.md)
* [Manage cluster vertical scaling (scale up) in Azure Data Explorer to accommodate changing demand](manage-cluster-vertical-scaling.md) (Verwalten der vertikalen Clusterskalierung (Hochskalieren) in Azure Data Explorer bei sich änderndem Bedarf)
