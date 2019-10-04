---
title: 'Agile Entwicklung von Data Science-Projekten: Team Data Science-Prozess'
description: Ausführen eines Data Science-Projekts auf systematische Weise mit Versionskontrolle und Zusammenarbeit innerhalb eines Projektteams über den Team Data Science-Prozess.
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 09/05/2019
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 09c5962e62077fbecc9b327320d0bb5b88416ffa
ms.sourcegitcommit: 55f7fc8fe5f6d874d5e886cb014e2070f49f3b94
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2019
ms.locfileid: "71260683"
---
# <a name="agile-development-of-data-science-projects"></a>Agile Entwicklung von Data Science-Projekten

Dieses Dokument beschreibt, wie Entwickler ein Data Science-Projekt auf systematische Weise mit Versionskontrolle und Zusammenarbeit innerhalb eines Projektteams über den [Team Data Science-Prozess](overview.md) (TDSP) ausführen können. Der TDSP ist ein von Microsoft entwickeltes Framework, das eine strukturierte Abfolge von Aktivitäten für die effiziente Ausführung cloudbasierter Predictive Analytics-Lösungen bereitstellt. Unter [Rollen und Aufgaben im Team Data Science-Prozess](roles-tasks.md) finden Sie eine Übersicht über die Rollen und Aufgaben für ein Data Science-Team, das eine Vereinheitlichung gemäß dem TDSP anstrebt. 

Dieser Artikel enthält Anweisungen zu folgenden Aufgaben: 

- Durchführen einer *Sprintplanung* für Arbeitselemente in einem Projekt.
- *Hinzufügen von Arbeitselementen* zu Sprints.
- Erstellen und Verwenden einer *von Agile abgeleiteten Arbeitselementvorlage*, die speziell auf die TDSP-Lebenszyklusphasen abgestimmt ist.

In den folgenden Anleitungen werden die erforderlichen Schritte zum Einrichten einer TDSP-Teamumgebung mit Azure Boards und Azure Repos in Azure DevOps beschrieben. In den Anleitungen wird Azure DevOps verwendet, da dies der Implementierung von TDSP bei Microsoft entspricht. Wenn Ihre Gruppe eine andere Codehostingplattform verwendet, ändern sich die Aufgaben des Teamleiters in der Regel nicht, aber die Vorgehensweise zum Durchführen der Aufgaben ist unterschiedlich. Beispielsweise ist das Verknüpfen eines Arbeitselements mit einem Git-Branch mit GitHub möglicherweise anders als mit Azure Repos.

Die folgende Abbildung veranschaulicht einen typischen Workflow mit Sprintplanung, Codierung und Quellcodeverwaltung für ein Data Science-Projekt:

![Team Data Science-Prozess](./media/agile-development/1-project-execute.png)

##  <a name='Terminology-1'></a>Arbeitselementtypen

Im TDSP-Sprintplanungsframework gibt es vier häufig verwendete Typen von *Arbeitselementen*: *Features*, *User Storys*, *Aufgaben* und *Fehler*. Das Backlog für alle Arbeitselemente erfolgt auf Projektebene, nicht auf der Ebene des Git-Repositorys. 

Dies sind die Definitionen für die Arbeitselementtypen:

- **Feature**: Ein Feature entspricht einem Projektauftrag. Unterschiedliche Aufträge von einem Kunden sind unterschiedliche Features, und es ist am besten, verschiedene Phasen eines Projekts als unterschiedliche Features zu betrachten. Wenn Sie zum Benennen Ihrer Features ein Schema wie *\<Kundenname>-\<Auftragsname>* verwenden, können Sie den Kontext des Projekts/Auftrags problemlos dem Namen entnehmen.
  
- **User Story**: User Storys sind Arbeitselemente, die für die End-to-End-Ausführung eines Features erforderlich sind. Beispiele für User Storys sind:
  - Datensammlung 
  - Erkunden von Daten 
  - Generieren von Features
  - Erstellen von Modellen
  - Operationalisieren von Modellen 
  - Erneutes Trainieren von Modellen
  
- **Aufgabe**: Aufgaben sind zuweisbare Arbeitselemente, die erledigt werden müssen, um eine bestimmte User Story abzuschließen. Beispiele für Aufgaben in der User Story *Datenabruf* sind:
  - Abrufen von SQL Server-Anmeldeinformationen
  - Hochladen von Daten in SQL Data Warehouse
  
- **Fehler**: Fehler sind Probleme in vorhandenem Code oder in Dokumenten, die korrigiert werden müssen, um eine Aufgabe abzuschließen. Wenn Fehler durch fehlende Arbeitselemente verursacht werden, können Sie in User Storys oder Aufgaben eskaliert werden. 

Data Scientists bevorzugen möglicherweise eine Agile-Vorlage, die Features, User Storys und Aufgaben durch TDSP-Lebenszyklusphasen und -Unterphasen ersetzt. Wenn Sie eine von Agile abgeleitete Arbeitselementvorlage erstellen möchten, die speziell auf die TDSP-Lebenszyklusphasen abgestimmt ist, lesen Sie [Verwenden einer Agile-TDSP-Arbeitsvorlage](#set-up-agile-dsp-6).

> [!NOTE]
> TDSP greift die Konzepte von Features, User Storys, Aufgaben und Fehlern aus SCM (Software Code Management) auf. Die TDSP-Konzepte können von den herkömmlichen SCM-Definitionen leicht abweichen.

## <a name='SprintPlanning-2'></a>Planen von Sprints

Viele Data Scientists sind mit mehreren Projekten beschäftigt, deren Abschluss mehrere Monate dauern kann und die in unterschiedlichen Geschwindigkeiten bearbeitet werden. Die Sprintplanung ist für die Projektpriorisierung sowie die Ressourcenplanung und -zuordnung nützlich. In Azure Boards können Sie problemlos Arbeitselemente in Ihrem Projekt erstellen, verwalten und nachverfolgen und eine Sprintplanung durchführen, um sicherzustellen, dass Ihre Projekte sich wie erwartet entwickeln.

Weitere Informationen zur Sprintplanung finden Sie unter [Scrum-Sprints](https://en.wikipedia.org/wiki/Scrum_(software_development)#Sprint). 

Weitere Informationen zur Sprintplanung in Azure Boards finden Sie unter [Zuweisen von Backlogelementen zu einem Sprint](/azure/devops/boards/sprints/assign-work-sprint). 

## <a name='AddFeature-3'></a>Hinzufügen eines Features zum Backlog 

Nachdem Sie das Projekt und das Projektcoderepository erstellt haben, können Sie dem Backlog ein Feature hinzufügen, das die Aufgaben für das Projekt darstellt.

1. Wählen Sie auf Ihrer Projektseite im linken Navigationsbereich **Boards** > **Backlogs** aus. 
   
1. Wenn sich der Arbeitselementtyp auf der Registerkarte **Backlog** in der oberen Leiste **Storys** befindet, wählen Sie in der Dropdownliste **Features** aus. Wählen Sie dann **Neues Arbeitselement aus.**
   
   ![Auswählen eines neuen Arbeitselements](./media/agile-development/2-sprint-team-overview.png)
   
1. Geben Sie einen Titel für das Feature ein (normalerweise den Projektnamen), und wählen Sie dann **Oben hinzufügen** aus. 
   
   ![Eingeben eines Titels und Auswählen von „Oben hinzufügen“](./media/agile-development/3-sprint-team-add-work.png)
   
1. Wählen Sie in der Liste **Backlog** das neue Feature aus, und öffnen Sie es. Geben Sie die Beschreibung ein, weisen Sie ein Teammitglied zu, und legen Sie Planungsparameter fest. 
   
   Sie können das Feature auch mit dem Azure Repos-Coderepository des Projekts verknüpfen, indem Sie im Abschnitt **Entwicklung** die Option **Verknüpfung hinzufügen** auswählen. 
   
   Nachdem Sie die Bearbeitung des Features abgeschlossen haben, wählen Sie **Speichern und schließen** aus.
   
   ![Bearbeiten des Features und Auswählen von „Speichern und schließen“](./media/agile-development/3a-add-link-repo.png)

## <a name='AddStoryunderfeature-4'></a>Hinzufügen einer User Story zum Feature 

Unter dem Feature können User Storys hinzugefügt werden, um wichtige erforderliche Schritte zum Abschließen des Projekts zu beschreiben. 

So fügen Sie einem Feature eine neue User Story hinzu:

1. Wählen Sie auf der Registerkarte **Backlog** das Symbol **+** links neben dem Features aus. 
   
   ![Hinzufügen einer neuen User Story unter dem Feature](./media/agile-development/4-sprint-add-story.png)
   
1. Geben Sie der User Story einen Titel, und bearbeiten Sie Details wie Zuordnung, Status, Beschreibung, Kommentare, Planung und Priorität. 
   
   Sie können die User Story auch mit einem Branch des Azure Repos-Coderepositorys des Projekts verknüpfen, indem Sie im Abschnitt **Entwicklung** die Option **Verknüpfung hinzufügen** auswählen. Wählen Sie das Repository und den Branch aus, mit dem Sie das Arbeitselement verknüpfen möchten, und wählen Sie dann **OK** aus.
   
   ![Hinzufügen einer Verknüpfung](./media/agile-development/5-sprint-edit-story.png)
   
1. Wenn Sie mit dem Bearbeiten der User Story fertig sind, wählen Sie **Speichern und schließen** aus. 

## <a name='AddTaskunderstory-5'></a>Hinzufügen einer Aufgabe zu einer User Story 

Aufgaben sind bestimmte ausführliche Schritte, die zum Abschließen einer User Story erforderlich sind. Nachdem alle Aufgaben einer User Story abgeschlossen sind, sollte auch die User Story abgeschlossen werden. 

Wenn Sie einer User Story eine Aufgabe hinzufügen möchten, wählen Sie das **+** neben dem User Story-Element aus, und wählen Sie dann **Aufgabe** aus. Geben Sie den Titel und andere Informationen in die Aufgabe ein.

![Hinzufügen einer Aufgabe zu einer User Story](./media/agile-development/7-sprint-add-task.png)

Nachdem Sie Features, User Storys und Aufgaben erstellt haben, können Sie sie in den Ansichten **Backlog** oder **Board** anzeigen, um ihren Status nachzuverfolgen.

![Ansicht „Backlogs“](./media/agile-development/8-sprint-backlog-view.png)

![Ansicht „Boards“](./media/agile-development/8a-sprint-board-view.png)

## <a name='set-up-agile-dsp-6'></a>Verwenden einer Agile-TDSP-Arbeitsvorlage

Data Scientists bevorzugen möglicherweise eine Agile-Vorlage, die Features, User Storys und Aufgaben durch TDSP-Lebenszyklusphasen und -Unterphasen ersetzt. In Azure Boards können Sie eine von Agile abgeleitete Vorlage erstellen, die TDSP-Lebenszyklusphasen verwendet, um Arbeitselemente zu erstellen und nachzuverfolgen. Die folgenden Schritte führen Sie durch das Einrichten einer Data Science-spezifischen Agile-Prozessvorlage und das Erstellen von Data Science-Arbeitselementen auf der Basis dieser Vorlage.

### <a name="set-up-an-agile-data-science-process-template"></a>Einrichten einer Agile-Data Science-Prozessvorlage

1. Wählen Sie auf der Hauptseite der Azure DevOps-Organisation im linken Navigationsbereich **Organisationseinstellungen** aus. 
   
1. Wählen Sie im linken Navigationsbereich **Organisationseinstellungen** unter **Boards** die Option **Prozess** aus. 
   
1. Wählen Sie im Bereich **Alle Prozesse** die Auslassungszeichen **...** neben **Agile** aus, und wählen Sie dann **Geerbten Prozess erstellen** aus.
   
   ![Erstellen eines geerbten Prozesses aus Agile](./media/agile-development/10-settings.png) 
   
1. Geben Sie im Dialogfeld **Geerbten Prozess aus Agile erstellen** den Namen *AgileDataScienceProcess* ein, und wählen Sie **Prozess erstellen** aus.
   
   ![Erstellen des AgileDataScienceProcess-Prozesses](./media/agile-development/11-agileds.png)
   
1. Wählen Sie in **Alle Prozesse** den neuen **AgileDataScienceProcess** aus. 
   
1. Deaktivieren Sie auf der Registerkarte **Arbeitselementtypen** die Optionen **Epic**, **Feature**, **User Story** und **Aufgabe**, indem Sie die Auslassungszeichen **...** neben den einzelnen Elementen auswählen und dann auf **Deaktivieren** klicken. 
   
   ![Deaktivieren von Arbeitselementtypen](./media/agile-development/12-disable.png)
   
1. Wählen Sie in **Alle Prozesse** die Registerkarte **Backlogebenen** aus. Wählen Sie unter **Portfoliobacklogs** die Auslassungszeichen **...** neben **Epic (deaktiviert)** aus, und wählen Sie dann **Bearbeiten/Umbenennen** aus. 
   
1. Gehen Sie Im Dialogfeld **Backlogebene bearbeiten** folgendermaßen vor:
   1. Ersetzen Sie unter **Name** die Angabe **Epic** durch *TDSP-Projekte*. 
   1. Wählen Sie unter **Arbeitselementtypen auf dieser Backlogebene** die Option **Neuer Arbeitselementtyp** aus, geben Sie *TDSP-Projekt* ein, und wählen Sie **Hinzufügen** aus. 
   1. Klicken Sie unter **Standardmäßiger Arbeitselementtyp** in der Dropdownliste auf **TDSP-Projekt**. 
   1. Wählen Sie **Speichern** aus.
   
   ![Festlegen der Portfoliobacklogebene](./media/agile-development/13-rename.png)  
   
1. Führen Sie die gleichen Schritte zum Umbenennen von **Features** in *TDSP-Phasen* aus, und fügen Sie die folgenden neuen Arbeitselementtypen hinzu:
   
   - *Geschäftliche Aspekte*
   - *Datenerfassung*
   - *Modellierung*
   - *Bereitstellung*
   
1. Benennen Sie unter **Anforderungsbacklog** **Storys** in *TDSP-Unterebenen* um, fügen Sie den neuen Arbeitselementtyp *TDSP-Unterebene* hinzu, und legen Sie den Arbeitselement-Standardtyp auf **TDSP-Unterebenen** fest.
   
1. Fügen Sie unter **Iterationsbacklog** einen neuen Arbeitselementtyp *TDSP-Aufgabe* hinzu, und legen Sie ihn als Arbeitselement-Standardtyp fest. 
   
Nachdem Sie diese Schritten abgeschlossen haben, sollten folgende Backlogebenen vorhanden sein:
   
 ![Backlogebenen der TDSP-Vorlage](./media/agile-development/14-template.png)  

### <a name="create-agile-data-science-process-work-items"></a>Erstellen von Arbeitselementen des Agile-Data Science-Prozesses

Mit der Data Science-Prozess Vorlage können Sie TDSP-Projekte erstellen und Arbeitselemente nachverfolgen, die den TDSP-Lebenszyklusphasen entsprechen.

1. Wählen Sie auf der Hauptseite Ihrer Azure DevOps-Organisation **Neues Projekt** aus. 
   
1. Benennen Sie das Projekt im Dialogfeld **Neues Projekt erstellen**, und wählen Sie dann **Erweitert** aus. 
   
1. Klicken Sie unter **Arbeitselementprozess** in der Dropdownliste auf **AgileDataScienceProcess**, und wählen Sie dann **Erstellen** aus.
   
   ![Erstellen eines TDSP-Projekts](./media/agile-development/15-newproject.png)
   
1. Wählen Sie im neu erstellten Projekt im linken Navigationsbereich **Boards** > **Backlogs** aus.
   
1. Um TDSP-Projekte sichtbar zu machen, wählen Sie das Symbol **Teameinstellungen konfigurieren** aus. Aktivieren Sie auf dem Bildschirm **Einstellungen** das Kontrollkästchen **TDSP-Projekte**, und wählen Sie dann **Speichern und schließen** aus.
   
   ![Aktivieren des Kontrollkästchens „TDSP-Projekte“](./media/agile-development/16-enabledsprojects1.png)
   
1. Um ein Data Science-spezifisches TDSP-Projekt zu erstellen, wählen Sie **TDSP-Projekte** in der oberen Leiste aus, und wählen Sie dann **Neues Arbeitselement** aus. 
   
1. Geben Sie dem Arbeitselement im Popupfenster einen Namen, und wählen Sie dann **Oben hinzufügen** aus.
   
   ![Erstellen des Arbeitselements des Data Science-Projekts](./media/agile-development/17-dsworkitems0.png)
   
1. Um ein Arbeitselement unter dem TDSP-Projekt hinzuzufügen, wählen Sie **+** neben dem Projekt aus, und wählen Sie dann den Typ des zu erstellenden Arbeitselements aus. 
   
   ![Auswählen des Typs des Data Science-Arbeitselements](./media/agile-development/17-dsworkitems1.png)
   
1. Geben Sie die Details in das neue Arbeitselement ein, und wählen Sie dann **Speichern und Schließen** aus.
   
1. Wählen Sie weiterhin die **+** -Symbole neben Arbeitselementen aus, um neue TDSP-Ebenen, Unterebenen und Aufgaben hinzuzufügen. 
   
Hier ist ein Beispiel dafür, wie die Arbeitselemente des Data Science-Projekts in der Ansicht **Backlog** angezeigt werden sollten:

![18](./media/agile-development/18-workitems1.png)


## <a name="next-steps"></a>Nächste Schritte

Unter [Gemeinsames Schreiben von Code mit Git](collaborative-coding-with-git.md) wird beschrieben, wie die gemeinschaftliche Codeentwicklung für Data Science-Projekte mithilfe von Git als Entwicklungsframework für freigegebenen Code funktioniert und wie diese Codeentwicklungsaktivitäten mit der mit dem Agile-Prozess geplanten Aufgaben verbunden werden.

Unter [Exemplarische Vorgehensweisen](walkthroughs.md) sind die Vorgehensweisen für bestimmte Szenarien mit Links und Miniaturansichtsbeschreibungen aufgeführt. Anhand der Verknüpfungsszenarien wird dargestellt, wie Sie Cloud- und lokale Tools und Dienste über Workflows oder Pipelines kombinieren, um intelligente Anwendungen zu erstellen.
  
Weitere Ressourcen zu Agile-Prozessen:

- [Agile-Prozess](/azure/devops/boards/work-items/guidance/agile-process)
  
- [Arbeitselementtypen und Workflow für Agile-Prozesse](/azure/devops/boards/work-items/guidance/agile-process-workflow)

