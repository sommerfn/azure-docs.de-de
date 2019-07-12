---
author: roygara
ms.service: storage
ms.topic: include
ms.date: 06/07/2019
ms.author: rogarana
ms.openlocfilehash: 368f08272173b019873dfe20e1164d6baf72ff5e
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2019
ms.locfileid: "67542663"
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
|Eingehende Daten  |300 MiB/s|      Siehe: Durchsatzwerte für Standarddateien|
|Ausgehende Daten   |200 MiB/s| Siehe: Durchsatzwerte für Standarddateien|
|Throughput| Siehe: Ein-/Ausgangswerte für Premiumdateien| Bis zu 60 MiB/s|