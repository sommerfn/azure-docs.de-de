---
title: Bewährte Methoden für die Erstellung einer Bewertung mit Azure Migrate-Serverbewertung
description: Bietet Tipps zum Erstellen von Bewertungen mit Azure Migrate-Serverbewertung.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 09/16/2019
ms.author: raynew
ms.openlocfilehash: e235116ab77159a0e2e9c66ad09cdb86ce6da1e9
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73466930"
---
# <a name="best-practices-for-creating-assessments"></a>Bewährte Methoden für die Erstellung von Bewertungen

[Azure Migrate](migrate-overview.md) stellt einen Hub mit Tools bereit, die Ihnen dabei helfen, Apps, Infrastrukturen und Workloads zu ermitteln, zu bewerten und zu Microsoft Azure zu migrieren. Der Hub umfasst Azure Migrate-Tools sowie Angebote von unabhängigen Drittanbietern (Independent Software Vendors, ISVs).

Dieser Artikel fasst die bewährten Methoden bei der Erstellung von Bewertungen mit dem Azure Migrate-Serverbewertungstool zusammen.

## <a name="about-assessments"></a>Informationen zu Bewertungen

Bewertungen, die Sie mit Azure Migrate-Serverbewertungen erstellen, sind Momentaufnahmen von Daten zu einem gewissen Zeitpunkt. Es gibt zwei Arten von Bewertungen in Azure Migrate.

**Bewertungstyp** | **Details** | **Daten**
--- | --- | ---
**Leistungsbasiert** | Bewertungen, die Empfehlungen auf der Grundlage der erfassten Leistungsdaten aussprechen | Die Empfehlung zur VM-Größe basiert auf CPU- und Speicherauslastungsdaten.<br/><br/> Die Empfehlung zum Datenträgertyp (HDD/SSD Standard oder Premium) basiert auf dem IOPS und dem Durchsatz der lokalen Datenträger.
**Aktuelle lokale Umgebung** | Bewertungen, die keine Leistungsdaten verwenden, um Empfehlungen auszusprechen. | Die Empfehlung zur VM-Größe basiert auf der lokalen VM-Größe<br/><br> Der empfohlene Datenträgertyp basiert auf dem, was Sie in der Speichertypeinstellung für die Bewertung auswählen.

### <a name="example"></a>Beispiel
Wenn Sie beispielsweise eine lokale VM mit vier Kernen bei 20 % Auslastung und einem Speicher von 8 GB bei 10 % Auslastung haben, sehen die Bewertungen wie folgt aus:

- **Leistungsbasierte Bewertung**:
    - Identifiziert effektive Kerne und Speicher basierend auf dem Kern (4 x 0,20 = 0,8) und der Speicherauslastung (8 GB x 0,10 = 0,8).
    - Wendet den in den Bewertungseigenschaften angegebenen Komfortfaktor (z.B. 1,3x) an, um die für die Dimensionierung zu verwendenden Werte zu erhalten. 
    - Empfiehlt die nächstgelegene VM-Größe in Azure, die bis zu 1,04 Kerne (0,8 x 1,3) und bis zu 1,04 GB (0,8 x 1,3) Speicher unterstützen kann.

- **Aktuelle Bewertung (wie in lokaler Umgebung)** :
    -  Empfiehlt eine VM mit vier Kernen und 8 GB Speicher.

## <a name="best-practices-for-creating-assessments"></a>Bewährte Methoden für die Erstellung von Bewertungen

Die Azure Migrate-Appliance erfasst fortlaufend Ihre lokale Umgebung und sendet Metadaten und Leistungsdaten an Azure. Befolgen Sie diese bewährten Methoden für die Bewertung von Servern, die mit einer Appliance ermittelt wurden:

- **Erstellung von aktuellen Bewertungen**: Sie können aktuelle Bewertungen sofort erstellen, sobald Ihre Computer im Azure Migrate-Portal angezeigt werden.
- **Erstellen von leistungsbezogenen Bewertungen**: Nach der Einrichtung der Ermittlung empfehlen wir Ihnen, mindestens einen Tag zu warten, bevor Sie eine leistungsbezogene Bewertung durchführen:
    - Die Erfassung von Leistungsdaten braucht Zeit. Wenn Sie mindestens einen Tag warten, stellen Sie damit sicher, dass genügend Leistungsdatenpunkte vorhanden sind, bevor Sie die Bewertung durchführen.
    - Wenn Sie leistungsbezogene Bewertungen durchführen, stellen Sie sicher, dass Sie Profile für Ihre Umgebung für die Dauer der Bewertungen erstellen. Wenn Sie z.B. die Bewertung mit einer auf eine Woche festgelegten Leistungsdauer erstellen, müssen Sie mindestens eine Woche nach dem Start der Ermittlung warten, bis alle Datenpunkte erfasst wurden. Wenn Sie nicht so lange warten, wird die Bewertung nicht mit fünf Sternen versehen.
- **Neuberechnen von Bewertungen**: Da es sich bei den Bewertungen um Momentaufnahmen handelt, werden sie nicht automatisch mit den neuesten Daten aktualisiert. Um eine Bewertung mit den neuesten Daten zu aktualisieren, müssen Sie sie neu berechnen.

Befolgen Sie diese bewährten Methoden für die Bewertung von Servern, die über eine CSV-Datei in Azure Migrate importiert werden:

- **Erstellung von aktuellen Bewertungen**: Sie können aktuelle Bewertungen sofort erstellen, sobald Ihre Computer im Azure Migrate-Portal angezeigt werden.
- **Erstellen von leistungsbezogenen Bewertungen**: Dadurch erhalten Sie eine bessere Kostenschätzung, insbesondere dann, wenn die lokale Serverkapazität überdimensioniert wurde. Die Genauigkeit der leistungsbasierten Bewertung hängt jedoch von den Leistungsdaten ab, die für die Server angegeben werden. 
- **Neuberechnen von Bewertungen**: Da es sich bei den Bewertungen um Momentaufnahmen handelt, werden sie nicht automatisch mit den neuesten Daten aktualisiert. Um eine Bewertung mit den neuesten importierten Daten zu aktualisieren, müssen Sie eine Neuberechnung durchführen.

## <a name="best-practices-for-confidence-ratings"></a>Bewährte Methoden für Zuverlässigkeitsstufen

Wenn Sie leistungsbezogene Bewertungen durchführen, wird der Bewertung eine Zuverlässigkeitsstufe von 1 Stern (niedrigste) bis 5 Sterne (höchste) zugewiesen. So nutzen Sie Zuverlässigkeitsstufen effektiv:
- Azure Migrate-Serverbewertungen benötigen die Nutzungsdaten für CPU/Speicher der VM.
- Für jeden an den virtuellen lokalen Computer angefügten Datenträger werden Informationen zu den Lese-/Schreib-IOPS sowie zum Durchsatz benötigt.
- Für jeden an die VM angefügten Netzwerkadapter benötigen sie die Netzwerk-E/A-Informationen.

Abhängig vom Prozentsatz der für die gewählte Dauer verfügbaren Datenpunkte wird die Zuverlässigkeitsstufe für eine Bewertung wie in der folgenden Tabelle zusammengefasst angegeben.

   **Verfügbarkeit von Datenpunkten** | **Zuverlässigkeitsstufe**
   --- | ---
   0 % bis 20 % | Ein Stern
   21 % bis 40 % | Zwei Sterne
   41 % bis 60 % | Drei Sterne
   61 % bis 80 % | Vier Sterne
   81 % bis 100 % | Fünf Sterne


## <a name="common-assessment-issues"></a>Häufe Bewertungsprobleme

Im Folgenden erfahren Sie, wie Sie einige häufig auftretenden Probleme mit der Umgebung lösen können, die sich auf die Bewertungen auswirken.

###  <a name="out-of-sync-assessments"></a>Nicht synchronisierte Bewertungen

Wenn Sie nach dem Erstellen einer Bewertung Computer einer Gruppe hinzufügen oder daraus entfernen, wird die von Ihnen erstellte Bewertung als **nicht synchron** gekennzeichnet. Führen Sie die Bewertung erneut durch (**Neu berechnen**), um die Gruppenänderungen zu berücksichtigen.

### <a name="outdated-assessments"></a>Veraltete Bewertungen

Wenn es lokale Änderungen an VMs gibt, die sich in einer Gruppe befinden, die bewertet wurde, wird die Bewertung als **veraltet** gekennzeichnet. Um die Änderungen zu berücksichtigen, führen Sie die Bewertung erneut aus.

### <a name="low-confidence-rating"></a>Niedrige Zuverlässigkeitsstufe

Eine Bewertung kann aus verschiedenen Gründen nicht über alle Datenpunkte verfügen:

- Sie haben für den Zeitraum, für den Sie die Bewertung erstellen, kein Profil Ihrer Umgebung erstellt. Wenn Sie z.B. eine *leistungsbasierte Bewertung* mit einer auf eine Woche festgelegten Leistungsdauer erstellen, müssen Sie mindestens eine Woche nach dem Start der Ermittlung warten, bis alle Datenpunkte erfasst wurden. Sie können jederzeit auf **Neu berechnen** klicken, um die neueste anwendbare Zuverlässigkeitsstufe anzuzeigen. Die Zuverlässigkeitsstufe ist nur beim Erstellen einer *leistungsbasierten* Bewertung anwendbar.

- Einige virtuelle Computer wurden während des Zeitraums, für den die Bewertung berechnet wird, heruntergefahren. Wenn VMs für eine gewisse Zeit heruntergefahren werden, kann das Tool „Serverbewertung“ für diesen Zeitraum keine Leistungsdaten erfassen.

- Nach dem Start der Ermittlung in der Serverbewertung wurden wenige VMs erstellt. Ein Beispiel: Angenommen, Sie erstellen eine Bewertung für den Leistungsverlauf des letzten Monats, und in der Umgebung wurden letzte Woche einige virtuelle Computer erstellt. In diesem Fall stehen für die gesamte Dauer keine Leistungsdaten für die neuen VMs zur Verfügung und die Zuverlässigkeitsstufe wäre gering.


## <a name="next-steps"></a>Nächste Schritte

- [Erfahren Sie mehr darüber](concepts-assessment-calculation.md), wie Bewertungen berechnet werden.
- Informationen zum Anpassen von Bewertungen finden Sie [hier](how-to-modify-assessment.md).
