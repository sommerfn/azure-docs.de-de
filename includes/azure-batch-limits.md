---
title: include file
description: include file
services: batch
author: laurenhughes
ms.service: batch
ms.topic: include
ms.date: 05/28/2019
ms.author: lahugh
ms.custom: include file
ms.openlocfilehash: 22bfc3c86605f4c2eed4c022919b3643f394ea95
ms.sourcegitcommit: cd70273f0845cd39b435bd5978ca0df4ac4d7b2c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2019
ms.locfileid: "67080903"
---
| **Ressource** | **Standardlimit** | **Maximales Limit** |
| --- | --- | --- |
| Azure Batch-Konten pro Region und Abonnement | 1-3 |50 |
| Dedizierte Kerne pro Batch-Konto | 90-900 | Support kontaktieren |
| Kerne mit niedriger Priorität pro Batch-Konto | 10-100 | Support kontaktieren |
| **[Aktive](https://docs.microsoft.com/rest/api/batchservice/job/get#jobstate)**  Aufträge und Auftragszeitpläne pro Batch-Konto (**abgeschlossene** Aufträge haben keinen Grenzwert) | 100-300 | 1\.000<sup>1</sup> |
| Pools pro Batch-Konto | 20-100 | 500<sup>1</sup> |

> [!NOTE]
> Die Standardgrenzwerte variieren abhängig vom Abonnementtyp, den Sie zum Erstellen eines Batch-Kontos verwenden. Die angezeigten Kontingente für Kerne gelten für Batch-Konten im Batch-Dienstmodus. [Zeigen Sie die Kontingente in Ihrem Batch-Konto an](../articles/batch/batch-quota-limit.md#view-batch-quotas).

<sup>1</sup>Wenden Sie sich an den Azure-Support, wenn Sie eine Heraufsetzung dieses Grenzwerts wünschen.
