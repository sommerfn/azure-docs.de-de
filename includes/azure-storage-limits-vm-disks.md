---
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 03/18/2019
ms.author: rogarana
ms.openlocfilehash: 8b25d2395811a2197aff6d653c5038a4380021e9
ms.sourcegitcommit: fecb6bae3f29633c222f0b2680475f8f7d7a8885
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68669666"
---
Sie können eine Reihe von Datenträgern an einen virtuellen Azure-Computer anfügen. Basierend auf den Skalierbarkeits- und Leistungszielen für Datenträger eines virtuellen Computers können Sie die Anzahl und den Typ der Datenträger festlegen, die Sie benötigen, um Ihre Anforderungen an Leistung und Kapazität zu erfüllen.

> [!IMPORTANT]
> Begrenzen Sie zur Optimierung der Leistung die Anzahl stark ausgelasteter Datenträger, die an den virtuellen Computer angefügt sind, um eine mögliche Drosselung zu vermeiden. Wenn nicht alle angefügten Datenträger zum gleichen Zeitpunkt stark ausgelastet sind, kann der virtuelle Computer eine größere Anzahl von Datenträgern unterstützen.

**Verwaltete Azure-Datenträger:**

In der folgenden Tabelle sind die standardmäßigen und maximal zulässigen Limits für die Anzahl von Ressourcen pro Region und Abonnement aufgeführt. Es gilt kein Limit für die Anzahl von verwalteten Datenträgern, Momentaufnahmen und Images pro Ressourcengruppe.  

> | Resource | Standardlimit  | Maximales Limit |
> | --- | --- | --- |
> | Verwaltete Standarddatenträger | 50.000 | 50.000 |
> | Verwaltete SSD Standard-Datenträger | 50.000 | 50.000 |
> | Verwaltete Premium-Datenträger | 50.000 | 50.000 |
> | Standard_LRS-Momentaufnahmen | 50.000 | 50.000 |
> | Standard_ZRS-Momentaufnahmen | 50.000 | 50.000 |
> | Verwaltetes Image | 50.000 | 50.000 |

* **Standardspeicherkonten:** Ein Standardspeicherkonto hat eine maximale Gesamtanforderungsrate von 20.000 IOPS. Die gesamten IOPS auf allen Datenträgern eines virtuellen Computers in einem Standardspeicherkonto dürfen dieses Limit nicht überschreiten.
  
    Basierend auf dem Limit für Anforderungsraten können Sie die Anzahl der Datenträger mit hoher Auslastung ungefähr berechnen, die von einem Standardspeicherkonto unterstützt werden. Im Basic-Tarif liegt die maximal zulässige Anzahl von Datenträgern mit hoher Auslastung für einen virtuellen Computer beispielsweise bei 66 (20.000/300 IOPS pro Datenträger). Die maximale Anzahl von Datenträgern mit hoher Auslastung für einen virtuellen Computer im Standard-Tarif liegt bei etwa 40 Datenträgern (20.000/500 IOPS pro Datenträger). 

* **Storage Premium-Konten:** Ein Storage Premium-Konto hat eine maximale Gesamtdurchsatzrate von 50 GBit/s. Der Gesamtdurchsatz aller Ihrer VM-Datenträger darf dieses Limit nicht überschreiten.

