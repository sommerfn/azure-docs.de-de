---
title: include file
description: include file
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 03/23/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 0e5c1974ecb1b3efb8df602c76700b7db04a88d7
ms.sourcegitcommit: 29880cf2e4ba9e441f7334c67c7e6a994df21cfe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/26/2019
ms.locfileid: "71310525"
---
Azure Storage bietet mehrere Arten von Speicherkonten. Jeder Typ unterstützt unterschiedliche Features und verfügt über ein eigenes Preismodell. Informieren Sie sich vor dem Erstellen eines Speicherkontos genau über diese Unterschiede, um den Kontotyp zu ermitteln, der sich für Ihre Anwendungen am besten eignet. Folgende Speicherkontotypen stehen zur Verfügung:

- **Konten vom Typ „Allgemein v2“:** Grundlegender Speicherkontotyp für Blobs, Dateien, Warteschlangen und Tabellen. Empfohlen für die meisten Azure Storage-Szenarien.
- **Konten vom Typ „Allgemein v1“:** Legacy-Speicherkontotyp für Blobs, Dateien, Warteschlangen und Tabellen. Verwenden Sie nach Möglichkeit Konten vom Typ „Allgemein v2“.
- **Konten vom Typ „Blockblobspeicher“** : Reine Blobspeicherkonten mit Premium-Leistungsmerkmalen. Für Szenarien mit hohen Transaktionsraten empfohlen, die kleinere Objekte verwenden oder gleichbleibend geringe Speicherlatenz erfordern.
- **FileStorage-Speicherkonten**: Reine Dateispeicherkonten mit Premium-Leistungsmerkmalen Empfohlen für Unternehmens- oder Hochleistungsanwendungen
- **Konten vom Typ „Blobspeicher“:** Reine Blobspeicherkonten. Verwenden Sie nach Möglichkeit Konten vom Typ „Allgemein v2“.

Die folgende Tabelle beschreibt die Speicherkontotypen und ihre jeweiligen Funktionen:

| Speicherkontotyp | Unterstützte Dienste                       | Unterstützte Leistungsstufen      | Unterstützte Zugriffsebenen         | Replikationsoptionen               | Bereitstellungsmodell<div role="complementary" aria-labelledby="deployment-model"><sup>1</sup></div> | Verschlüsselung<div role="complementary" aria-labelledby="encryption"><sup>2</sup></div> |
|----------------------|------------------------------------------|-----------------------------|--------------------------------|-----------------------------------|------------------------------|------------------------|
| Allgemein v2   | Blob, Datei, Warteschlange, Tabelle und Datenträger       | Standard, Premium<div role="complementary" aria-labelledby="premium-performance"><sup>5</sup></div> | Heiß, Kalt, Archiv<div role="complementary" aria-labelledby="archive"><sup>3</sup></div> | LRS, GRS, RA-GRS, ZRS, GZRS (Vorschau), RA-GZRS (Vorschau)<div role="complementary" aria-labelledby="zone-redundant-storage"><sup>4</sup></div> | Ressourcen-Manager             | Verschlüsselt              |
| Allgemein v1   | Blob, Datei, Warteschlange, Tabelle und Datenträger       | Standard, Premium<div role="complementary" aria-labelledby="premium-performance"><sup>5</sup></div> | –                            | LRS, GRS, RA-GRS                  | Resource Manager, klassisch    | Verschlüsselt              |
| Blockblobspeicher   | Blob (nur Blockblobs und Anfügeblobs) | Premium                       | –                            | LRS                               | Ressourcen-Manager             | Verschlüsselt              |
| FileStorage   | Nur Dateien | Premium                       | –                            | LRS                               | Ressourcen-Manager             | Verschlüsselt              |
| Blob Storage         | Blob (nur Blockblobs und Anfügeblobs) | Standard                      | Heiß, Kalt, Archiv<div role="complementary" aria-labelledby="archive"><sup>3</sup></div> | LRS, GRS, RA-GRS                  | Ressourcen-Manager             | Verschlüsselt              |

<div id="deployment-model"><sup>1</sup>Die Verwendung des Azure Resource Manager-Bereitstellungsmodells wird empfohlen. Speicherkonten, die das klassische Bereitstellungsmodell verwenden, können an einigen Standorten weiterhin erstellt werden, und vorhandene klassische Konten werden weiterhin unterstützt. Weitere Informationen finden Sie unter <a href="https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-model">Azure Resource Manager-Bereitstellung im Vergleich zur klassischen Bereitstellung: Grundlegendes zu Bereitstellungsmodellen und zum Status von Ressourcen</a>.</div>

<div id="encryption"><sup>2</sup>Alle Speicherkonten sind per Storage Service Encryption (SSE) für ruhende Daten verschlüsselt. Weitere Informationen finden Sie unter <a href="https://docs.microsoft.com/azure/storage/common/storage-service-encryption">Azure Storage Service Encryption für ruhende Daten</a>.</div>

<div id="archive"><sup>3</sup>Die Ebene „Archiv“ ist nur auf Ebene der einzelnen Blobs verfügbar, nicht auf Ebene des Speicherkontos. Nur Blockblobs und Anfügeblobs können archiviert werden. Weitere Informationen finden Sie unter <a href="https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers">Azure Blob Storage: Speicherebenen „Premium“ (Vorschauversion), „Heiß“, „Kalt“ und „Archiv“</a>.</div>

<div id="zone-redundant-storage"><sup>4</sup>Zonenredundanter Speicher (ZRS) und geozonenredundanter Speicher (GZRS) (Vorschau) sind nur für Storage Standard-Konten des Typs „Allgemein v2“ verfügbar. Weitere Informationen zu ZRS finden Sie unter <a href="https://docs.microsoft.com/azure/storage/common/storage-redundancy-zrs">Zonenredundanter Speicher (ZRS): Hochverfügbare Azure Storage-Anwendungen</a>. Weitere Informationen zu GZRS finden Sie unter <a href="https://docs.microsoft.com/azure/storage/common/storage-redundancy-gzrs">Geozonenredundante Speicher für Hochverfügbarkeit und maximale Dauerhaftigkeit (Vorschau)</a>. Weitere Informationen zu weiteren Replikationsoptionen finden Sie unter <a href="https://docs.microsoft.com/azure/storage/common/storage-redundancy">Azure Storage-Replikation</a>.</div>

<div id="premium-performance"><sup>5</sup>Premium-Leistung für Konten vom Typ „Allgemein v2“ und „Allgemein v1“ ist nur für Datenträger und Seitenblob verfügbar.</div>
