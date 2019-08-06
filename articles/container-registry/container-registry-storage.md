---
title: Speichern von Images in Azure Container Registry
description: Details darüber, wie Ihre Docker-Containerimages in Azure Container Registry gespeichert werden, sowie über Sicherheit, Redundanz und Kapazität.
services: container-registry
author: dlepow
manager: gwallace
ms.service: container-registry
ms.topic: article
ms.date: 03/21/2018
ms.author: danlep
ms.openlocfilehash: 4517cc21ca0087358e750cd480288d4ec3718791
ms.sourcegitcommit: f5075cffb60128360a9e2e0a538a29652b409af9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/18/2019
ms.locfileid: "68310533"
---
# <a name="container-image-storage-in-azure-container-registry"></a>Speichern von Containerimages in Azure Container Registry

Jede Azure Container Registry vom Typ [Basic, Standard und Premium](container-registry-skus.md) profitiert von erweiterten Azure-Speicherfunktionen wie Verschlüsselung ruhender Daten für die Imagedatensicherheit und Georedundanz für den Imagedatenschutz. Die folgenden Abschnitte beschreiben sowohl die Funktionen als auch die Imagespeichergrenzen in Azure Container Registry (ACR).

## <a name="encryption-at-rest"></a>Verschlüsselung ruhender Daten

Alle Containerimages in Ihrer Registrierung werden im Ruhezustand verschlüsselt. Azure verschlüsselt ein Image automatisch, bevor es gespeichert wird, und entschlüsselt es dynamisch, sobald Sie oder Ihre Anwendungen und Dienste einen Pull für das Image ausführen.

## <a name="geo-redundant-storage"></a>Georedundanter Speicher

Azure verwendet ein georedundantes Speicherschema, um den Verlust Ihrer Containerimages zu verhindern. Azure Container Registry repliziert die Containerimages automatisch in mehrere geografisch entfernte Rechenzentren und verhindert deren Verlust im Falle eines regionalen Speicherausfalls.

## <a name="geo-replication"></a>Georeplikation

Für Szenarien, die eine noch zuverlässigere Hochverfügbarkeit erfordern, sollten Sie das Feature [Georeplikation](container-registry-geo-replication.md) der Premium-Registrierungen nutzen. Die Georeplikation schützt vor dem Verlust des Zugriffs auf Ihre Registrierung im Falle eines *Totalausfalls* der Region, nicht nur eines Speicherausfalls. Zudem bietet die Georeplikation weitere Vorteile, wie z.B. netzwerknahe Imagespeicher für schnellere Push- und Pullvorgänge in verteilten Entwicklungs- oder Bereitstellungsszenarien.

## <a name="image-limits"></a>Imagegrenzen

Die folgende Tabelle beschreibt das Containerimage und die Speichergrenzen für Azure-Containerregistrierungen.

| Resource | Begrenzung |
| -------- | :---- |
| Repositorys | Keine Begrenzung |
| Bilder | Keine Begrenzung |
| Ebenen | Keine Begrenzung |
| `Tags` | Keine Begrenzung|
| Storage | 5 TB |

Eine hohe Anzahl von Repositorys und Tags können die Leistung Ihrer Registrierung beeinträchtigen. Löschen Sie regelmäßig unbenutzte Repositorys, Tags und Images als Teil der Wartungsroutine für Ihre Registrierung. Gelöschte Registrierungsressourcen wie Repositorys, Images und Tags können nach dem Löschen *nicht* wiederhergestellt werden. Weitere Informationen zum Löschen von Registrierungsressourcen finden Sie unter [Löschen von Containerimages in Azure Container Registry](container-registry-delete.md).

## <a name="storage-cost"></a>Speicherkosten

Ausführliche Informationen zu den Preisen finden Sie unter [Containerregistrierung – Preise][pricing].

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu den verschiedenen Azure Container Registry-SKUs (Basic, Standard, Premium) finden Sie unter den [Azure Container Registry-SKUs](container-registry-skus.md).

<!-- IMAGES -->

<!-- LINKS - External -->
[portal]: https://portal.azure.com
[pricing]: https://aka.ms/acr/pricing

<!-- LINKS - Internal -->
