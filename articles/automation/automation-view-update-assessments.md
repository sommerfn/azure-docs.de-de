---
title: Anzeigen von Updatebewertungen der Azure-Updateverwaltung
description: In diesem Artikel wird beschrieben, wie Sie Updatebewertungen für Updatebereitstellungen anzeigen.
services: automation
ms.service: automation
ms.subservice: update-management
author: bobbytreed
ms.author: robreed
ms.date: 05/17/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: e88622ede6437086b86a33081d6ec9b9ea50ef65
ms.sourcegitcommit: 0576bcb894031eb9e7ddb919e241e2e3c42f291d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "72377439"
---
# <a name="view-azure-update-management-update-assessments"></a>Anzeigen von Updatebewertungen der Azure-Updateverwaltung

Wählen Sie in Ihrem Automation-Konto **Updateverwaltung** aus, um den Status Ihrer Computer anzuzeigen.

Diese Ansicht enthält Informationen zu Ihren Computern, zu fehlenden Updates, zu Updatebereitstellungen und geplanten Updatebereitstellungen. In der Spalte **KONFORMITÄT** können Sie sehen, wann der Computer zuletzt bewertet wurde. In der Spalte **UPDATE-AGENT-BEREITSCHAFT** können Sie die Integrität des Update-Agents anzeigen. Wenn ein Problem vorliegt, wählen Sie den Link aus, um zur Dokumentation für die Problembehandlung zu navigieren und zu erfahren, welche Schritte zum Beheben des Problems erforderlich sind.

Um eine Protokollsuche auszuführen, die Informationen zum Computer, zu Updates oder zu Bereitstellungen zurückgibt, wählen Sie das Element in der Liste aus. Der Bereich **Protokollsuche** wird mit einer Abfrage für das ausgewählte Element geöffnet:

![Standardansicht der Updateverwaltung](media/automation-update-management/update-management-view.png)

## <a name="view-missing-updates"></a>Anzeigen fehlender Updates

Wählen Sie **Fehlende Updates** aus, um eine Liste der Updates anzuzeigen, die auf Ihrem Computer nicht vorhanden sind. Jedes Update wird aufgeführt und kann ausgewählt werden. Es werden Informationen zur Anzahl der Computer, die das Update benötigen, und zum Betriebssystem sowie ein Link zu weiteren Informationen angezeigt. Der Bereich **Protokollsuche** zeigt weitere Details zu den Updates an.

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

Für Linux kann die Updateverwaltung zwischen kritischen und Sicherheitsupdates in der Cloud unterscheiden. Sie zeigt Bewertungsdaten aufgrund der Datenanreicherung in der Cloud an. Für das Patchen verwendet die Updateverwaltung Klassifizierungsdaten, die auf dem Computer verfügbar sind. Im Gegensatz zu anderen Distributionen verfügt CentOS nicht standardmäßig über diese Informationen. Wenn Sie CentOS-Computer für das Zurückgeben von Sicherheitsdaten für den folgenden Befehl konfiguriert haben, kann die Updateverwaltung basierend auf Klassifizierungen Patchvorgänge ausführen.

```bash
sudo yum -q --security check-update
```

Es gibt derzeit keine unterstützte Methode zum Aktivieren einer nativen Klassifizierungsdatenverfügbarkeit unter CentOS. Zu diesem Zeitpunkt wird nur Unterstützung nach bestem Wissen für Kunden bereitgestellt, die dies möglicherweise auf eigene Weise aktiviert haben.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie Updatebewertungen angezeigt haben, können Sie eine Updatebereitstellung planen, indem Sie die Schritte unter [Verwalten von Updates und Patches für Ihre Azure-VMS ](automation-tutorial-update-management.md) ausführen.