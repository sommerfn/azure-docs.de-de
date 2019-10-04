---
title: Referenzarchitekturen für das Rendering in Azure – Azure Batch
description: Architekturen für die Verwendung von Azure Batch und anderen Azure-Diensten zum Erweitern einer lokalen Render-Farm mittels Cloud Bursting in die Cloud
services: batch
ms.service: batch
author: davefellows
manager: gwallace
ms.author: lahugh
ms.date: 02/07/2019
ms.topic: conceptual
ms.custom: seodec18
ms.openlocfilehash: fa2d59b2a6d2dea72276ab38a5cb1ca7bfb579a4
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/18/2019
ms.locfileid: "68323110"
---
# <a name="reference-architectures-for-azure-rendering"></a>Referenzarchitekturen für das Rendering in Azure

In diesem Artikel werden Architekturdiagramme auf hoher Ebene für Szenarien zum Erweitern (oder „Bursting“) einer lokalen Render-Farm zu Azure gezeigt. Die Beispiele zeigen verschiedene Optionen für Compute-, Netzwerk- und Speicherdienste in Azure.

## <a name="hybrid-with-nfs-or-cfs"></a>Hybrid mit NFS oder CFS

Das folgende Diagramm zeigt ein Hybridszenario, das die folgenden Azure-Dienste umfasst:

* **Compute**: Azure Batch-Pool oder VM-Skalierungsgruppe.

* **Netzwerk** – lokal: Azure ExpressRoute oder VPN. Azure: Azure VNet.

* **Speicher** – Dateiein- und ausgabe: NFS oder CFS mithilfe von Azure-VMs, synchronisiert mit lokalem Speicher über Azure-Dateisynchronisierung oder RSync. Alternativ: Avere vFXT für die Ein- oder Ausgabe von Dateien von lokalen NAS-Geräten mithilfe von NFS.

  ![Cloudbursting – hybrid mit NFS oder CFS](./media/batch-rendering-architectures/hybrid-nfs-cfs-avere.png)

## <a name="hybrid-with-blobfuse"></a>Hybrid mit Blobfuse

Das folgende Diagramm zeigt ein Hybridszenario, das die folgenden Azure-Dienste umfasst:

* **Compute**: Azure Batch-Pool oder VM-Skalierungsgruppe.

* **Netzwerk** – lokal: Azure ExpressRoute oder VPN. Azure: Azure VNet.

* **Speicher** – Dateiein- und ausgabe: Blobspeicher, eingebunden in Computeressourcen über Azure Blobfuse.

  ![Cloudbursting – hybrid mit Blobfuse](./media/batch-rendering-architectures/hybrid-blob-fuse.png)

## <a name="hybrid-compute-and-storage"></a>Hybrides Compute und Speichern

Das folgende Diagramm zeigt ein vollständig verbundenes Hybridszenario für Compute und Speichern und umfasst die folgenden Azure-Dienste:

* **Compute**: Azure Batch-Pool oder VM-Skalierungsgruppe.

* **Netzwerk** – lokal: Azure ExpressRoute oder VPN. Azure: Azure VNet.

* **Speicher** – standortübergreifend: Avere vFXT. Optionale Archivierung von lokalen Dateien mithilfe von Azure Data Box in Blob Storage oder lokal mit Avere FXT für NAS-Beschleunigung.

  ![Cloudbursting – hybrides Compute und Speichern](./media/batch-rendering-architectures/hybrid-compute-storage-avere.png)


## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zur Verwendung von [Render-Managern](batch-rendering-render-managers.md) mit Azure Batch.

* Weitere Informationen zu Optionen für das [Rendering in Azure](batch-rendering-service.md).