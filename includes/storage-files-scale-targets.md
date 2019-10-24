---
author: roygara
ms.service: storage
ms.topic: include
ms.date: 05/06/2019
ms.author: rogarana
ms.openlocfilehash: d21709a231f97f1f1dc86837f79457df0cdb2263
ms.sourcegitcommit: 12de9c927bc63868168056c39ccaa16d44cdc646
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "72513283"
---
| Resource | Standarddateifreigaben | Premium-Dateifreigaben |
|----------|---------------|------------------------------------------|
| Mindestgröße einer Dateifreigabe | Keine Mindestgröße, nutzungsbasierte Bezahlung | 100 GiB; bereitgestellt |
| Maximale Größe einer Dateifreigabe | 100 TiB*, 5 TiB | ca. 100 TiB |
| Maximale Größe einer Datei in einer Dateifreigabe | 1 TiB | 1 TiB |
| Maximale Anzahl an Dateien in einer Dateifreigabe | Keine Begrenzung | Keine Begrenzung |
| Maximale Anzahl IOPS pro Freigabe | 10.000 IOPS*, 1.000 IOPS | 100.000 IOPS |
| Maximale Anzahl gespeicherter Zugriffsrichtlinien pro Dateifreigabe | 5 | 5 |
| Zieldurchsatz für eine einzelne Dateifreigabe | bis zu 300 MiB/s*, bis zu 60 MiB/s  | Siehe: Ein- und Ausgangswerte für Premium-Dateifreigabe|
| Maximaler Ausgang für eine einzelne Dateifreigabe | Siehe: Zieldurchsatz für Standard-Dateifreigabe | Bis zu 6.204 MiB/s |
| Maximaler Eingang für eine einzelne Dateifreigabe | Siehe: Zieldurchsatz für Standard-Dateifreigabe | Bis zu 4.136 MiB/s |
| Maximale Anzahl geöffneter Handles pro Datei | 2\.000 geöffnete Handles | 2\.000 geöffnete Handles |
| Maximale Anzahl von Freigabemomentaufnahmen | 200 Freigabemomentaufnahmen | 200 Freigabemomentaufnahmen |
| Maximale Objektnamenlänge (Verzeichnisse und Dateien) | 2\.048 Zeichen | 2\.048 Zeichen |
| Maximale Pfadnamenkomponente (im Pfad \A\B\C\D ist jeder Buchstabe eine Komponente) | 255 Zeichen | 255 Zeichen |

\* Nicht in allen Regionen verfügbar. Eine Liste der verfügbaren Regionen finden Sie unter [Regionale Verfügbarkeit](../articles/storage/files/storage-files-planning.md#regional-availability).