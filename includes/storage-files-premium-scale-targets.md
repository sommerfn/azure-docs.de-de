---
author: roygara
ms.service: storage
ms.topic: include
ms.date: 06/07/2019
ms.author: rogarana
ms.openlocfilehash: b28427b3ede0cfaeb9e08d3c73b15ea7f2961f1b
ms.sourcegitcommit: 83df2aed7cafb493b36d93b1699d24f36c1daa45
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2019
ms.locfileid: "71180049"
---
#### <a name="additional-premium-file-share-level-limits"></a>Zusätzliche Freigabegrenzwerte für Dateien auf Premiumebene

|Bereich  |Ziel  |
|---------|---------|
|Mindestgröße vergrößern/verkleinern    |1 GiB      |
|IOPS-Grundwert    |1 IOPS pro GiB, bis zu 100.000|
|IOPS-Bursting    |3 x IOPS pro GiB, bis zu 100.000|
|Ausgangsrate         |60 MiB/s + 0,06 * bereitgestelltes GiB        |
|Eingangsrate| 40 MiB/s + 0,04 * bereitgestelltes GiB |

#### <a name="file-level-limits"></a>Grenzwerte auf Dateiebene

|Bereich  |Premiumdateien  |Standarddateien |
|---------|---------|---------|
|Size                  |1 TiB         |1 TiB|
|Max. IOPS pro Datei     |5\.000         |1\.000|
|Gleichzeitige Handles    |2\.000         |2\.000|
|Ausgehende Daten  |300 MiB/s|      Siehe: Durchsatzwerte für Standarddateien|
|Eingehende Daten  |200 MiB/s| Siehe: Durchsatzwerte für Standarddateien|
|Throughput| Siehe: Ein-/Ausgangswerte für Premiumdateien| Bis zu 60 MiB/s|
