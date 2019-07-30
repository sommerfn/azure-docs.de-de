---
title: Verwalten des vertikalen Hoch- oder Herunterskalierens eines Clusters in Azure Data Explorer, um ihn an den sich ändernden Bedarf anzupassen
description: Dieser Artikel beschreibt Schritte zum zentralen Hoch- und Herunterskalieren eines Azure Data Explorer-Clusters basierend auf sich änderndem Bedarf.
author: radennis
ms.author: radennis
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 07/14/2019
ms.openlocfilehash: 80bbdf3a5d936719b06782cd78d56088b36cb21d
ms.sourcegitcommit: 6b41522dae07961f141b0a6a5d46fd1a0c43e6b2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/15/2019
ms.locfileid: "67985481"
---
# <a name="manage-cluster-vertical-scaling-scale-up-in-azure-data-explorer-to-accommodate-changing-demand"></a>Verwalten des vertikalen Hoch- oder Herunterskalierens eines Clusters in Azure Data Explorer, um ihn an den sich ändernden Bedarf anzupassen

Die richtige Größe eines Clusters ist entscheidend für die Leistung von Azure-Daten-Explorer. Eine statische Clustergröße kann zu einer Unter- oder Überauslastung führen, was beides nicht ideal ist.

Da der Bedarf für einen Cluster nicht mit absoluter Genauigkeit vorhergesagt werden kann, ist ein besserer Ansatz die *Skalierung* eines Clusters, wobei je nach Bedarf Kapazität und CPU-Ressourcen hinzugefügt und entfernt werden. 

Es gibt zwei Workflows für die Skalierung eines Azure Data Explorer-Clusters:

* [Horizontales Hoch- oder Herunterskalieren](manage-cluster-horizontal-scaling.md)
* Vertikales (oder zentrales) Hoch- oder Herunterskalieren

In diesem Artikel wird der Workflow für das vertikale Hoch- oder Herunterskalieren erläutert:

## <a name="configure-vertical-scaling"></a>Konfigurieren des vertikalen Hoch- oder Herunterskalierens

1. Navigieren Sie im Azure-Portal zu Ihrer Azure Data Explorer-Clusterressource. Wählen Sie unter **Einstellungen** die Option **Zentral hochskalieren** aus.

1. Im Fenster **Zentral hochskalieren** wird eine Liste der verfügbaren SKUs für Ihren Cluster angezeigt. In der folgenden Abbildung sind z. B. nur vier SKUs verfügbar.

    ![Zentrales Hochskalieren](media/manage-cluster-vertical-scaling/scale-up.png)

    Die SKUs sind deaktiviert, weil sie die aktuelle SKU sind oder in der Region, in der sich der Cluster befindet, nicht zur Verfügung stehen.

1. Wenn Sie Ihre SKU ändern möchten, wählen Sie eine neue SKU aus, und klicken Sie auf **Auswählen**.

> [!NOTE]
> * Der Vorgang des vertikalen Hoch- oder Herunterskalierens kann einige Minuten dauern. Während dieser Zeit wird Ihr Cluster angehalten. 
> * Ein zentrales Herunterskalieren kann die Clusterleistung beeinträchtigen.
> * Der Preis ist eine Schätzung der virtuellen Computer des Clusters und der Kosten für den Azure Data Explorer-Dienst. Andere Kosten sind nicht eingeschlossen. Auf der Azure Data Explorer-Seite [Kostenschätzung](https://dataexplorer.azure.com/AzureDataExplorerCostEstimator.html) finden Sie eine Schätzung und auf der Azure Data Explorer-Seite [Preise](https://azure.microsoft.com/pricing/details/data-explorer/) vollständige Preisinformationen.

Sie haben nun das vertikale Hoch- oder Herunterskalieren für Ihren Azure Data Explorer-Cluster konfiguriert. Fügen Sie eine weitere Regel für ein horizontales Hoch- oder Herunterskalieren hinzu. Wenn Sie Hilfe bei Clusterskalierungsproblemen benötigen, erstellen Sie im [Azure-Portal](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) eine Supportanfrage.

## <a name="next-steps"></a>Nächste Schritte

* [Verwalten Sie die horizontale Skalierung des Clusters](manage-cluster-horizontal-scaling.md), um die Instanzanzahl basierend auf den von Ihnen angegebenen Metriken dynamisch zu skalieren.

* Überwachen Sie Ihre Ressourcenverwendung gemäß diesem Artikel: [Überwachen der Azure Data Explorer-Leistung, -Integrität und -Nutzung mit Metriken](using-metrics.md).

