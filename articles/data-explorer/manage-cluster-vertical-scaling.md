---
title: Zentrales Hochskalieren von Azure Data Explorer-Clustern zur Anpassung an sich änderndem Bedarf
description: Dieser Artikel beschreibt Schritte zum zentralen Hoch- und Herunterskalieren eines Azure Data Explorer-Clusters basierend auf sich änderndem Bedarf.
author: radennis
ms.author: radennis
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 06/30/2019
ms.openlocfilehash: dc9ca8bb592e699d19835efeafb91e81408ae297
ms.sourcegitcommit: 1e347ed89854dca2a6180106228bfafadc07c6e5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2019
ms.locfileid: "67571268"
---
# <a name="manage-cluster-scale-up-to-accommodate-changing-demand"></a>Verwalten der Clusterskalierung bei sich änderndem Bedarf

Es gibt zwei Workflows für die Skalierung eines Azure Data Explorer-Clusters:
1. [Horizontales Hoch- oder Herunterskalieren](manage-cluster-horizontal-scaling.md)
2. Vertikales (oder zentrales) Hoch- oder Herunterskalieren

In diesem Artikel wird die Verwaltung der vertikalen Skalierung eines Clusters gezeigt.

Die richtige Größe eines Clusters ist entscheidend für die Leistung von Azure-Daten-Explorer. Aber der Bedarf in einem Cluster kann nicht mit absoluter Genauigkeit vorhergesagt werden. Eine statische Clustergröße kann zu einer Unter- oder Überauslastung führen, was beides nicht ideal ist. Ein besserer Ansatz ist die *Skalierung* eines Clusters, wobei je nach Bedarf Kapazität und CPU-Ressourcen hinzugefügt und entfernt werden. 

## <a name="steps-to-configure-vertical-scaling"></a>Schritte zum Konfigurieren der vertikalen Skalierung

1. Zum Custer wechseln Wählen Sie unter **Einstellungen** die Option **Zentral hochskalieren** aus.

    Ihnen wird eine Liste der verfügbaren SKUs angezeigt. In der folgenden Abbildung sind z. B. nur vier SKUs verfügbar.

    ![Zentrales Hochskalieren](media/manage-cluster-vertical-scaling/scale-up.png)

    SKUs sind deaktiviert, weil sie entweder die aktuelle SKU sind, oder weil sie in der Region nicht verfügbar sind, in der sich der Cluster befindet.

1. Zum Ändern Ihrer SKU wählen Sie die gewünschte SKU aus, und wählen Sie **Auswählen** aus.

> [!NOTE]
> Die vertikale Skalierung kann einige Minuten dauern. Während dieser Zeit wird Ihr Cluster angehalten. Beachten Sie, dass eine Herunterskalierung die Clusterleistung beeinträchtigen kann.

Sie haben nun einen zentralen Hoch- oder Herunterskalierungsvorgang für Ihren Azure Data Explorer-Cluster durchgeführt.

Wenn Sie Hilfe bei Clusterskalierungsproblemen benötigen, erstellen Sie im [Azure-Portal](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) eine Supportanfrage.

## <a name="next-steps"></a>Nächste Schritte

* [Verwalten Sie die horizontale Skalierung des Clusters](manage-cluster-horizontal-scaling.md), um die Instanzanzahl basierend auf den von Ihnen angegebenen Metriken dynamisch zu skalieren.

* Überwachen Sie Ihre Ressourcenverwendung gemäß diesem Artikel: [Überwachen der Azure Data Explorer-Leistung, -Integrität und -Nutzung mit Metriken](using-metrics.md).

