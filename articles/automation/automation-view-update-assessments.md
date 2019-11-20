---
title: Anzeigen von Updatebewertungen der Azure-Updateverwaltung
description: In diesem Artikel erfahren Sie, wie Sie Updatebewertungen für Updatebereitstellungen anzeigen.
services: automation
ms.service: automation
ms.subservice: update-management
author: bobbytreed
ms.author: robreed
ms.date: 05/17/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: d4e20b2924504b714dff9f5ba650f9b25c1c26c3
ms.sourcegitcommit: e0e6663a2d6672a9d916d64d14d63633934d2952
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "72690866"
---
# <a name="view-azure-update-management-update-assessments"></a>Anzeigen von Updatebewertungen der Azure-Updateverwaltung

Wählen Sie in Ihrem Azure Automation-Konto **Updateverwaltung** aus, um den Status Ihrer Computer anzuzeigen.

Diese Ansicht enthält Informationen zu Ihren Computern, zu fehlenden Updates, zu Updatebereitstellungen und geplanten Updatebereitstellungen. Die Spalte **KONFORMITÄT** gibt Aufschluss darüber, wann der Computer zuletzt bewertet wurde. Die Spalte **UPDATE-AGENT-BEREITSCHAFT** gibt Aufschluss über die Integrität des Update-Agents. Liegt ein Problem vor, wählen Sie den Link aus, um zur Dokumentation für die Problembehandlung zu navigieren und Informationen zur Behebung des Problems zu erhalten.

Wenn Sie eine Protokollsuche ausführen möchten, die Informationen zum Computer, zu Updates oder zu Bereitstellungen zurückgibt, wählen Sie das entsprechende Element in der Liste aus. Der Bereich **Protokollsuche** wird mit einer Abfrage für das ausgewählte Element geöffnet:

![Standardansicht der Updateverwaltung](media/automation-update-management/update-management-view.png)

## <a name="view-missing-updates"></a>Anzeigen fehlender Updates

Wählen Sie **Fehlende Updates** aus, um eine Liste der Updates anzuzeigen, die auf Ihrem Computer nicht vorhanden sind. Jedes Update wird aufgeführt und kann ausgewählt werden. Es werden Informationen zur Anzahl von Computern, die das Update benötigen, sowie Details zum Betriebssystem und ein Link zu weiteren Informationen angezeigt. Im Bereich **Protokollsuche** werden weitere Details zu den Updates angezeigt.

![Fehlende Updates](./media/automation-view-update-assessments/automation-view-update-assessments-missing-updates.png)

## <a name="update-classifications"></a>Updateklassifizierungen

Die folgenden Tabellen enthalten eine Liste der Updateklassifizierungen in der Updateverwaltung sowie eine Definition für jede Klassifikation.

### <a name="windows"></a>Windows

|Classification  |BESCHREIBUNG  |
|---------|---------|
|Kritische Updates     | Ein Update für ein bestimmtes Problem, mit dem ein entscheidender, nicht sicherheitsrelevanter Fehler behoben wird.        |
|Sicherheitsupdates     | Ein Update für ein produktspezifisches, sicherheitsrelevantes Problem.        |
|Updaterollups     | Eine kumulative Gruppe von Hotfixes, die zur Vereinfachung der Bereitstellung gebündelt sind.        |
|Feature Packs     | Neue Produktfeatures, die nicht im Rahmen eines Produktreleases verteilt werden.        |
|Service Packs     | Eine kumulative Gruppe von Hotfixes, die auf eine Anwendung angewendet werden.        |
|Definitionsupdates     | Ein Update für virenbehaftete oder andere Definitionsdateien.        |
|Tools     | Ein Hilfsprogramm oder Feature, mit dem mindestens eine Aufgabe ausgeführt werden kann.        |
|Aktualisierungen     | Ein Update für eine Anwendung oder Datei, die zurzeit installiert ist.        |

### <a name="linux-2"></a>Linux

|Classification  |BESCHREIBUNG  |
|---------|---------|
|Kritische Updates und Sicherheitsupdates     | Updates für ein spezielles oder produktspezifisches, sicherheitsrelevantes Problem.         |
|Andere Updates     | Alle anderen Updates, die nicht kritisch sind oder bei denen es sich nicht um Sicherheitsupdates handelt.        |

Für Linux kann die Updateverwaltung bei der Anzeige von Bewertungsdaten zwischen kritischen Updates und Sicherheitsupdates in der Cloud unterscheiden. (Diese Granularität wird durch die Datenanreicherung in der Cloud ermöglicht.) Für das Patchen verwendet die Updateverwaltung Klassifizierungsdaten, die auf dem Computer verfügbar sind. Im Gegensatz zu anderen Distributionen verfügt CentOS in den RTM-Versionen des Produkts standardmäßig nicht über diese Informationen. Wenn Sie CentOS-Computer so konfiguriert haben, dass für den folgenden Befehl Sicherheitsdaten zurückgegeben werden, kann die Updateverwaltung basierend auf Klassifizierungen Patchvorgänge ausführen.

```bash
sudo yum -q --security check-update
```

Aktuell steht keine unterstützte Methode zur Verfügung, mit der unter CentOS eine native Verfügbarkeit von Klassifizierungsdaten implementiert werden kann. Für Kunden, die diese Funktion möglicherweise selbst implementiert haben, wird derzeit nur Support nach bestem Wissen bereitgestellt.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie Updatebewertungen angezeigt haben, können Sie eine Updatebereitstellung planen, indem Sie die Schritte unter [Verwalten von Updates und Patches für Ihre Azure-VMS ](automation-tutorial-update-management.md) ausführen.
