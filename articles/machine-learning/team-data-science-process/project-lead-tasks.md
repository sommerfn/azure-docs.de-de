---
title: Aufgaben des Projektleiters im Team Data Science-Prozess
description: Eine ausführliche exemplarische Vorgehensweise zu den Aufgaben für einen Projektleiter in einem Team Data Science-Prozessteam
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 09/24/2019
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 8a94a2ae5298bbee8bb1c9c0fa044eb3189147be
ms.sourcegitcommit: 824e3d971490b0272e06f2b8b3fe98bbf7bfcb7f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2019
ms.locfileid: "72244387"
---
# <a name="project-lead-tasks-in-the-team-data-science-process"></a>Aufgaben von Projektleitern im Team Data Science-Prozess

In diesem Artikel werden Aufgaben beschrieben, die von einem *Projektleiter* abgeschlossen werden, um ein Repository für sein Projektteam im [Team Data Science-Prozess](overview.md) (TDSP) einzurichten. Der TDSP ist ein von Microsoft entwickeltes Framework, das eine strukturierte Abfolge von Aktivitäten für die effiziente Ausführung cloudbasierter Predictive Analytics-Lösungen bereitstellt. Der TDSP soll die Zusammenarbeit und die Lernprozesse im Team verbessern. Unter [Rollen und Aufgaben im Team Data Science-Prozess](roles-tasks.md) finden Sie eine Übersicht über die Mitarbeiterrollen und die dazugehörigen Aufgaben für ein Data Science-Team, das eine Vereinheitlichung gemäß dem TDSP anstrebt.

Ein Projektleiter verwaltet die täglichen Aktivitäten der einzelnen Datenanalysten in einem bestimmten Data Science-Projekt im TDSP. Das folgende Diagramm zeigt den Workflow für Projektleiteraufgaben:

![Workflow für Projektleiteraufgaben](./media/project-lead-tasks/project-leads-1-tdsp-creating-projects.png)

In diesem Tutorial werden Schritt 1: „Erstellen eines Projektrepositorys“ und Schritt 2: „Seeding des Projektrepositorys aus Ihrem ProjectTemplate-Teamrepository“ behandelt. 

Für Schritt 3: „Erstellen von Featurearbeitselementen für das Projekt“ und Schritt 4: „Hinzufügen von Storys für Projektphasen“ finden Sie weitere Informationen unter [Agile Entwicklung von Data Science-Projekten](agile-development.md).

Für Schritt 5: „Erstellen und Anpassen von Speicher-/Analyseressourcen und ggf. Freigeben dieser Ressourcen“ finden Sie weitere Informationen unter [Erstellen von Teamdaten und Analyseressourcen](team-lead-tasks.md#create-team-data-and-analytics-resources).

Für Schritt 6: „Einrichten der Sicherheitskontrolle eines Projektrepositorys“ finden Sie weitere Informationen unter [Hinzufügen von Teammitgliedern und Konfigurieren von Berechtigungen](team-lead-tasks.md#add-team-members-and-configure-permissions).

> [!NOTE] 
> In diesem Artikel wird Azure Repos verwendet, um ein TDSP-Projekt einzurichten, da auf diese Weise TDSP bei Microsoft implementiert wird. Wenn Ihr Team eine andere Codehostingplattform einsetzt, sind die Aufgaben des Projektleiters dieselben, aber die Ausführung kann unterschiedlich ausfallen.

## <a name="prerequisites"></a>Voraussetzungen

In diesem Tutorial wird davon ausgegangen, dass die folgenden Ressourcen und Berechtigungen von Ihrem [Gruppenmanager](group-manager-tasks.md) und Ihrem [Teamleiter](team-lead-tasks.md) eingerichtet wurden:

- Die Azure DevOps-**Organisation** für Ihre Dateneinheit
- Ein **Teamprojekt** für Ihr Data Science Team
- **Repositorys** für Teamvorlagen und Hilfsprogramme
- **Berechtigungen** für Ihr Organisationskonto, damit Sie Repositorys für Ihr Projekte erstellen und bearbeiten können

Damit Sie Repositorys klonen und deren Inhalt auf Ihrem lokalen Computer oder Ihrer Data Science Virtual Machine (DSVM) ändern können oder um Azure-Dateispeicher einzurichten und auf Ihrer DSVM einzubinden, benötigen Sie außerdem Folgendes:

- Ein Azure-Abonnement.
- Git muss auf Ihrem Computer installiert sein. Wenn Sie eine DSVM verwenden, ist Git vorinstalliert. Andernfalls helfen Ihnen die Informationen im [Anhang von Plattformen und Tools](platforms-and-tools.md#appendix) weiter.
- Wenn Sie eine DSVM verwenden möchten, wird die Windows- oder Linux-DSVM in Azure erstellt und konfiguriert. Weitere Informationen und Anweisungen finden Sie in der Dokumentation zur [Data Science Virtual Machine](/azure/machine-learning/data-science-virtual-machine/).
- Für eine Windows DSVM muss [Git Credential Manager (GCM)](https://github.com/Microsoft/Git-Credential-Manager-for-Windows) auf Ihrem Computer installiert sein. Scrollen Sie in der Datei *README.md* nach unten zum Abschnitt für **Download und Installation**, und wählen Sie dann das **neueste Installationsprogramm** aus. Laden Sie das Installationsprogramm mit der Erweiterung *EXE* von der Installationsprogrammseite herunter, und führen Sie es aus. 
- Für eine Linux-DSVM muss ein öffentlicher SSH-Schlüssel auf Ihrer DSVM eingerichtet und in Azure DevOps hinzugefügt werden. Weitere Informationen und Anweisungen finden Sie im [Anhang zu Plattformen und Tools](platforms-and-tools.md#appendix) im Abschnitt **Erstellen eines öffentlichen SSH-Schlüssels**. 

## <a name="create-a-project-repository-in-your-team-project"></a>Erstellen eines Projektrepositorys in Ihrem Teamprojekt

So erstellen Sie ein Projektrepository im Projekt **MyTeam** Ihres Teams

1. Wechseln Sie zur Seite **Zusammenfassung** für das Projekt Ihres Teams unter *https:\//\<Servername>/\<Organisationsname>/\<Teamname>* , z. B. **https:\//dev.azure.com/DataScienceUnit/MyTeam**, und wählen Sie im linken Navigationsbereich **Repos** aus. 
   
1. Wählen Sie oben auf der Seite den Repositorynamen und dann in der Dropdownliste die Option **Neues Repository** aus.
   
   ![Auswählen von „Neues Repository“](./media/project-lead-tasks/project-leads-9-select-repos.png)
   
1. Stellen Sie im Dialogfeld **Neues Repository erstellen** sicher, dass **Git** unter **Typ** ausgewählt ist. Geben Sie *DSProject1* unter **Repositoryname** ein, und wählen Sie dann **Erstellen** aus.
   
   ![Erstellen eines Repositorys](./media/project-lead-tasks/project-leads-3-create-project-repo-2.png)
   
1. Vergewissern Sie sich, dass auf der Seite für die Projekteinstellungen das neue Repository **DSProject1** angezeigt wird. 
   
   ![Projektrepository in den Projekteinstellungen](./media/project-lead-tasks/project-leads-4-create-project-repo-3.png)

## <a name="import-the-team-template-into-your-project-repository"></a>Importieren der Teamvorlage in das Projektrepository

So füllen Sie Ihr Projektrepository mit dem Inhalt Ihres Teamvorlagenrepositorys auf

1. Wählen Sie auf der Seite **Zusammenfassung** für das Projekt Ihres Teams im linken Navigationsbereich **Repos** aus. 
   
1. Wählen Sie oben auf der Seite den Repositorynamen und dann in der Dropdownliste die Option **DSProject1** aus.
   
1. Wählen Sie auf der Seite **DSProject1 ist leer** die Option **Importieren** aus. 
   
   ![„Importieren“ auswählen](./media/project-lead-tasks/project-leads-5-create-project-repo-4.png)
   
1. Wählen Sie im Dialogfeld **Git-Repository importieren** die Option **Git** als **Quelltyp** aus, und geben Sie die URL für Ihr Repository **TeamTemplate** unter **URL klonen** ein. Die URL lautet *https:\//\<Servername>/\<Organisationsname>/\<Teamname>/_git/\<Name des Teamvorlagenrepositorys>* . Beispiel: **https:\//dev.azure.com/DataScienceUnit/MyTeam/_git/TeamTemplate**. 
   
1. Wählen Sie **Importieren** aus. Der Inhalt Ihres Teamvorlagenrepositorys wird in Ihr Projektrepository importiert. 
   
   ![Importieren eines Teamvorlagenrepositorys](./media/project-lead-tasks/project-leads-6-create-project-repo-5.png)

Wenn Sie den Inhalt Ihres Projektrepositorys anpassen müssen, um die spezifischen Anforderungen des Projekts zu erfüllen, können Sie Dateien und Ordner des Repositorys hinzufügen, löschen oder ändern. Sie können direkt in Azure Repos arbeiten oder das Repository auf Ihrem lokalen Computer oder auf der DSVM klonen, Änderungen vornehmen und die Aktualisierungen in das freigegebene Projektrepository pushen. Befolgen Sie die Anweisungen unter [Anpassen der Inhalte der Teamrepositorys](team-lead-tasks.md#customize-the-contents-of-the-team-repositories).

## <a name="next-steps"></a>Nächste Schritte

Es folgen Links zu ausführlichen Beschreibungen der anderen Rollen und Aufgaben, die vom Team Data Science-Prozess definiert wurden:

- [Group Manager tasks for a data science team (Aufgaben des Gruppenleiters eines Data Science-Teams)](group-manager-tasks.md)
- [Team Lead tasks for a data science team (Aufgaben des Teamleiters eines Data Science-Teams)](team-lead-tasks.md)
- [Aufgaben für einen einzelnen Mitwirkenden in einem Data Science-Team](project-ic-tasks.md)
