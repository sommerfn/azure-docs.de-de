---
title: include file
description: include file
services: storage
author: mhopkins-msft
ms.author: mhopkins
ms.date: 08/09/2019
ms.service: storage
ms.subservice: blobs
ms.topic: include
ms.reviewer: hux
ms.custom: include file
ms.openlocfilehash: 03674a51566ab89791725d1a51c7af12ed6949e5
ms.sourcegitcommit: 0f54f1b067f588d50f787fbfac50854a3a64fff7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/12/2019
ms.locfileid: "68952885"
---
Wenn Sie Daten aus dem Archivspeicher lesen möchten, müssen Sie zuerst die Ebene des Blobs in „Hot“ oder „Cool“ ändern. Dieser Prozess wird als Aktivierung bezeichnet und kann mehrere Stunden dauern. Wir empfehlen die Verwendung großer Blobs, um eine optimale Aktivierungsleistung zu erzielen. Die Dauer kann sich ggf. erhöhen, wenn mehrere kleine Blobs gleichzeitig aktiviert werden. Aktuell stehen zwei Aktivierungsprioritäten zur Verfügung: „Hohe Priorität (Vorschau)“ und „Standardpriorität“. Diese können über die optionale Eigenschaft *x-ms-rehydrate-priority* bei Vorgängen vom Typ [Blobtarif festlegen](https://docs.microsoft.com/rest/api/storageservices/set-blob-tier) oder [Blob kopieren](https://docs.microsoft.com/rest/api/storageservices/copy-blob) festgelegt werden.

* **Standardpriorität**: Die Aktivierungsanforderung gemäß der Eingangsreihenfolge verarbeitet und kann bis zu 15 Stunden dauern.
* **Hohe Priorität (Vorschau)** : Die Aktivierungsanforderung wird gegenüber Standardanforderungen bevorzugt behandelt und kann bereits nach weniger als einer Stunde abgeschlossen sein. Hohe Priorität kann je nach Blobgröße und aktueller Auslastung länger als eine Stunde dauern. Anforderungen mit hoher Priorität werden gegenüber Anforderungen mit Standardpriorität garantiert bevorzugt behandelt.

> [!NOTE]
> Die Standardpriorität ist die Standardaktivierungsoption für „Archiv“. Die hohe Priorität ist eine schnellere Option, die mehr kostet als die Aktivierung mit Standardpriorität und in der Regel nur in Notfallsituationen verwendet wird, in denen Daten dringend wiederhergestellt werden müssen.

Während der Aktivierung können Sie anhand der Eigenschaft *Archive Status* (Archivstatus) des Blobs ermitteln, ob die Ebene geändert wurde. Je nach Zielebene lautet der Status entweder „rehydrate-pending-to-hot“ (Aktivierung für Ebene „Hot“ ausstehend) oder „rehydrate-pending-to-cool“ (Aktivierung für Ebene „Cool“ ausstehend). Nach Abschluss des Vorgangs wird die Eigenschaft „Archive Status“ (Archivstatus) entfernt, und die Eigenschaft *Access Tier* (Zugriffsebene) des Blobs spiegelt die neue Ebene vom Typ „Hot“ oder „Cool“ wider.
