---
author: bucurb
ms.author: bobuc
ms.date: 09/18/2019
ms.service: azure-spatial-anchors
ms.topic: include
ms.openlocfilehash: 7ac98f608cb1ca5bab590714623bc78036d46b81
ms.sourcegitcommit: a170b69b592e6e7e5cc816dabc0246f97897cb0c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2019
ms.locfileid: "74092996"
---
## <a name="configure-the-cloud-anchor-session"></a>Konfigurieren der Cloudankersitzung

Als Nächstes wird die Konfiguration der Cloudankersitzung beschrieben. In der ersten Zeile legen wir den Sensoranbieter für die Sitzung fest. Ab jetzt werden alle Anker, die wir während der Sitzung erstellen, einer Gruppe von Sensormesswerten zugeordnet. Als Nächstes instanziieren wir Ermittlungskriterien für Geräte in der Nähe und führen eine Initialisierung durch, um eine Anpassung an die Anwendungsanforderungen vorzunehmen. Abschließend weisen wir die Sitzung an, beim Ermitteln von Ankern Sensordaten zu nutzen, indem wir einen „Watcher“ für unsere Kriterien für Geräte in der Nähe erstellen.