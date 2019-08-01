---
title: Best Practices für die Erstellung einer Bewertung mit Azure Migrate-Serverbewertung | Microsoft-Dokumentation
description: Bietet Tipps zum Erstellen von Bewertungen mit Azure Migrate-Serverbewertung.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 07/15/2019
ms.author: raynew
ms.openlocfilehash: 18b82b5553f7045c38c9de532199c2a0fd815ee1
ms.sourcegitcommit: b2db98f55785ff920140f117bfc01f1177c7f7e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/16/2019
ms.locfileid: "68234311"
---
# <a name="best-practices-for-creating-assessments"></a>Best Practices für die Erstellung von Bewertungen

[Azure Migrate](migrate-overview.md) stellt einen Hub mit Tools bereit, die Ihnen dabei helfen, Apps, Infrastrukturen und Workloads zu ermitteln, zu bewerten und zu Microsoft Azure zu migrieren. Der Hub umfasst Azure Migrate-Tools sowie Angebote von unabhängigen Drittanbietern (Independent Software Vendors, ISVs).

Dieser Artikel fasst die Best Practices bei der Erstellung von Bewertungen mit dem Azure Migrate-Serverbewertungstool zusammen.

## <a name="about-assessments"></a>Informationen zu Bewertungen

Assessments, die Sie mit Azure Migrate Server Assessment erstellen, sind Momentaufnahmem von Daten zu einem gewissen Zeit. Es gibt zwei Arten von Bewertungen in Azure Migrate.

**Bewertungstyp** | **Details** | **Daten**
--- | --- | ---
**Leistungsbasiert** | Beurteilungen geben Empfehlungen auf der Grundlage der gesammelten Leistungsdaten | Die Empfehlung zur VM-Größe basiert auf CPU- und Speicherauslastungsdaten.<br/><br/> Die Empfehlung zum Datenträgertyp (HDD/SSD Standard oder Premium) basiert auf dem IOPS und dem Durchsatz der lokalen Datenträger.
**Wie lokal** | Beurteilungen, die keine Leistungsdaten verwenden, um Empfehlungen abzugeben. | Die Empfehlung zur VM-Größe basiert auf der lokalen VM-Größe<br/><br> Der empfohlene Datenträgertyp basiert auf dem, was Sie in der Speichertypeinstellung für die Bewertung auswählen.

### <a name="example"></a>Beispiel
Wenn Sie beispielsweise eine lokale VM mit vier Kernen bei 20 % Auslastung und einem Speicher von 8 GB bei 10 % Auslastung haben, sehen die Bewertungen wie folgt aus:

- **Leistungsbasierte Bewertung**:
    - Identifiziert effektive Kerne und Speicher basierend auf dem Kern (4 x 0,20 = 0,8) und der Speicherauslastung (8 GB x 0,10 = 0,8).
    - Wendet den in den Bewertungseigenschaften angegebenen Komfortfaktor (z. B. 1,3x) an, um die für die Dimensionierung zu verwendenden Werte zu erhalten. 
    - Empfiehlt die nächstgelegene VM-Größe in Azure, die bis zu 1,4 Kerne (0,8 x 1,3) und bis zu 1,4 GB (0,8 x 1,3) Speicher unterstützen kann.

- **Bewertung wie lokal**:
    -  Empfiehlt eine VM mit vier Kernen, 8 GB Speicher.

## <a name="best-practices-for-creating-assessments"></a>Best Practices für die Erstellung von Bewertungen

Die Azure Migrate-Appliance erfasst fortlaufend Ihre lokale Umgebung und sendet Metadaten und Leistungsdaten an Azure. Befolgen Sie diese Best Practices für die Erstellung von Assessments:

- **Erstellung von „wie lokal“-Bewertungen**: Sie können „wie lokal“-Bewertungen sofort erstellen, sobald Ihre Computer im Azure Migrate-Portal erscheinen.
- **Erstellen von leistungsbezogenen Bewertungen**: Nach der Einrichtung der Ermittlung empfehlen wir Ihnen, mindestens einen Tag zu warten, bevor Sie eine leistungsbezogene Bewertung durchführen:
    - Die Ermittlung von Leistungsdaten braucht Zeit. Wenn Sie mindestens einen Tag warten, stellen Sie damit sicher, dass genügend Leistungsdatenpunkte vorhanden sind, bevor Sie die Beurteilung durchführen.
    - Wenn Sie leistungsbezogene Beurteilungen durchführen, stellen Sie sicher, dass Sie Profile für Ihre Umgebung für die Dauer der Beurteilungen erstellen. Wenn Sie z. B. die Bewertung mit einer auf 1 Tag festgelegten Leistungsdauer erstellen, müssen Sie bis mindestens einen Tag nach dem Start der Ermittlung warten, bis alle Datenpunkte gesammelt sind. Wenn Sie dies nicht tun, wird die Bewertung nicht mit fünf Sternen bewertet.
- **Bewertungen neu berechnen**: Da es sich bei den Bewertungen um Momentaufnahmen handelt, werden sie nicht automatisch mit den neuesten Daten aktualisiert. Um eine Bewertung mit den neuesten Daten zu aktualisieren, müssen Sie sie neu berechnen.

## <a name="best-practices-for-confidence-ratings"></a>Best Practices für Zuverlässigkeitsstufen

Wenn Sie leistungsbezogene Bewertungen durchführen, wird der Bewertung eine Zuverlässigkeitsstufe von 1 Stern (niedrigste) bis 5 Sterne (höchste) verliehen. Zuverlässigkeitsstufen effektiv nutzen:
- Azure Migrate-Serverbewertung benötigt die Nutzungsdaten für die VM CPU/Speicher.
- Für jeden an den virtuellen lokalen Computer angefügten Datenträger werden Informationen zu den Lese-/Schreib-IOPS sowie zum Durchsatz benötigt.
- Für jeden an die VM angeschlossenen Netzwerkadapter benötigt sie die Netzwerk-E/A-Informationen.

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

Wenn Sie nach dem Erstellen einer Bewertung Computer zu einer Gruppe hinzufügen oder daraus entfernen, wird die von Ihnen erstellte Bewertung als **nicht synchron** gekennzeichnet. Führen Sie die Bewertung erneut durch (**Neu berechnen**), um die Gruppenänderungen zu berücksichtigen.

### <a name="outdated-assessments"></a>Veraltete Bewertungen

Wenn es lokale Änderungen an VMs gibt, die sich in einer Gruppe befinden, die bewertet wurde, wird die Bewertung als **„veraltet“** vorgemerkt. Um die Änderungen zu berücksichtigen, führen Sie die Bewertung erneut durch.

### <a name="low-confidence-rating"></a>Niedrige Zuverlässigkeitsstufe

Eine Bewertung kann aus verschiedenen Gründen nicht über alle Datenpunkte verfügen:

- Sie haben für den Zeitraum, für den Sie die Bewertung erstellen, kein Profil Ihrer Umgebung erstellt. Wenn Sie z. B. eine *leistungsbasierte Bewertung* mit einer auf eine Woche festgelegten Leistungsdauer erstellen, müssen Sie bis mindestens eine Woche nach dem Start der Ermittlung warten, bis alle Datenpunkte gesammelt sind. Sie können jederzeit auf **„Neu berechnen“** klicken, um die aktuelle Zuverlässigkeitsstufe anzuzeigen. Die Zuverlässigkeitsstufe ist nur beim Erstellen einer *leistungsbasierten* Bewertung anwendbar.

- Einige virtuelle Computer wurden während des Zeitraums, für den die Bewertung berechnet wird, heruntergefahren. Wenn VMs für eine gewisse Zeit heruntergefahren werden, kann das Tool „Serverbewertung“ für diesen Zeitraum keine Leistungsdaten sammeln.

- Nach dem Start der Ermittlung in der Serverbewertung wurden wenige VMs erstellt. Ein Beispiel: Angenommen, Sie erstellen eine Bewertung für den Leistungsverlauf des letzten Monats, und in der Umgebung wurden letzte Woche einige virtuelle Computer erstellt. In diesem Fall stehen für die gesamte Dauer keine Leistungsdaten für die neuen VMs zur Verfügung und die Zuverlässigkeitsstufe wäre gering.


## <a name="next-steps"></a>Nächste Schritte

- [Erfahren Sie mehr darüber,](concepts-assessment-calculation.md) wie Bewertungen berechnet werden.
- [Erfahren Sie mehr darüber,](how-to-modify-assessment.md) wie Bewertungen angepasst werden können.
