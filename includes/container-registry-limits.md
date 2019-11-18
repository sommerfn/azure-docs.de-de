---
title: include file
description: include file
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: include
ms.date: 11/05/2019
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: bc4ac68cb415a43ac34d36afc2adc30307e6d37c
ms.sourcegitcommit: 827248fa609243839aac3ff01ff40200c8c46966
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73795265"
---
| Resource | Basic | Standard | Premium |
|---|---|---|---|
| Storage<sup>1</sup> | 10 GiB | 100 GiB| 500 GiB |
| Maximale Größe für Imageebenen | 200 GiB | 200 GiB | 200 GiB |
| Lesevorgänge pro Minute<sup>2, 3</sup> | 1\.000 | 3,000 | 10.000 |
| Schreibvorgänge pro Minute<sup>2, 4</sup> | 100 | 500 | 2\.000 |
| Downloadbandbreite MBit/s<sup>2</sup> | 30 | 60 | 100 |
| Uploadbandbreite MBit/s<sup>2</sup> | 10 | 20 | 50 |
| webhooks | 2 | 10 | 100 |
| Georeplikation | – | – | [Unterstützt][geo-replication] |
| Inhaltsvertrauensstellung | – | – | [Unterstützt][content-trust] |
| Zugriff auf das virtuelle Netzwerk | – | – | [Vorschau][vnet] |
| Repositorybezogene Berechtigungen | – | – | [Vorschau][token]|
| &bull; Token | – | – | 20.000 |
| &bull; Bereichszuordnungen | – | – | 20.000 |
| &bull; Repositorys pro Bereichszuordnung | – | – | 500 |


<sup>1</sup> Die angegebenen Speichergrenzen beziehen sich auf die Größe des *inbegriffenen* Speichers für jeden Tarif. Für Imagespeicher, der diese Grenzen überschreitet, wird Ihnen ein zusätzlicher Tagestarif pro GiB in Rechnung gestellt. Weitere Informationen zu diesen Tarifen finden Sie unter [Container Registry – Preise][pricing].

<sup>2</sup> *Lesevorgänge*, *Schreibvorgänge* und *Bandbreite* sind geschätzte Mindestwerte. Die Azure Container Registry soll die Leistung der Nutzung nach Bedarf verbessern.

<sup>3</sup> Ein [docker pull](https://docs.docker.com/registry/spec/api/#pulling-an-image)-Befehl wird basierend auf der Anzahl von Ebenen im Image in mehrere Lesevorgänge plus Manifestabruf übersetzt.

<sup>4</sup> Ein [docker push](https://docs.docker.com/registry/spec/api/#pushing-an-image)-Befehl wird basierend auf der Anzahl von Ebenen, die per Push übertragen werden müssen, in mehrere Schreibvorgänge übersetzt. Ein `docker push` enthält *Lesevorgänge* zum Abrufen eines Manifests für ein vorhandenes Image.

<!-- LINKS - External -->
[pricing]: https://azure.microsoft.com/pricing/details/container-registry/

<!-- LINKS - Internal -->
[geo-replication]: ../articles/container-registry/container-registry-geo-replication.md
[content-trust]: ../articles/container-registry/container-registry-content-trust.md
[vnet]: ../articles/container-registry/container-registry-vnet.md
[token]: ../articles/container-registry/container-registry-repository-scoped-permissions.md