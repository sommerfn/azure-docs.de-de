---
title: 'Gemeinsames Schreiben von Code mit Git: Team Data Science-Prozess'
description: Erfahren Sie, wie eine gemeinschaftliche Codeentwicklung für Data Science-Projekte mithilfe von Git mit Agile-Planung umgesetzt wird.
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 08/23/2019
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 3b57621fcec654f11c8e9a68e4568f332dbf9ac6
ms.sourcegitcommit: 532335f703ac7f6e1d2cc1b155c69fc258816ede
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/30/2019
ms.locfileid: "70195451"
---
# <a name="collaborative-coding-with-git"></a>Gemeinsames Schreiben von Code mit Git

In diesem Artikel wird beschrieben, wie Sie Git als Framework für die gemeinschaftliche Codeentwicklung für Data Science-Projekte verwenden. Der Artikel enthält Informationen dazu, wie Sie Code in Azure Repos mit Arbeitselementen der [agilen Entwicklung](agile-development.md) in Azure Boards verknüpfen, Code Reviews durchführen und Pull Requests für Änderungen erstellen und zusammenführen.

## <a name='Linkaworkitemwithagitbranch-1'></a>Verknüpfen eines Arbeitselements mit einem Azure Repos-Branch 

Mit Azure DevOps können Sie eine Azure Boards User Story oder eine Arbeitsaufgabe bequem mit einem Azure Repos-Git-Repositorybranch verbinden. Sie können Ihre User Story oder Aufgabe direkt mit dem zugeordneten Code verknüpfen. 

Wählen Sie zum Verbinden eines Arbeitselements mit einer neuen Branch unter **Aktionen** das Auslassungszeichen ( **...** ) neben dem Arbeitselement aus, und scrollen Sie im Kontextmenü zu **Neuer Branch**, und wählen Sie die Option aus.  

![1](./media/collaborative-coding-with-git/1-sprint-board-view.png)

Geben Sie im Dialogfeld **Branch erstellen** den Namen des neuen Branchs sowie das Azure Repos-Git-Basisrepository und den Branch ein. Das Basisrepository muss sich in demselben Azure DevOps-Projekt wie das Arbeitselement befinden. Der Basisbranch kann der Masterbranch oder eine andere vorhandene Verzweigung sein. Wählen Sie die Option **Branch erstellen** aus. 

![2](./media/collaborative-coding-with-git/2-create-a-branch.png)

Sie können auch einen neuen Branch erstellen, indem Sie den folgenden Git Bash-Befehl in Windows oder Linux verwenden:

```bash
git checkout -b <new branch name> <base branch name>

```
Wenn Sie keinen \<Namen für den Basisbranch> angeben, basiert der neue Branch auf `master`. 

Führen Sie den folgenden Befehl aus, um zu Ihrem Arbeitsbranch zu wechseln: 

```bash
git checkout <working branch name>
```

Nach dem Wechsel zum Arbeitsbranch können Sie mit dem Entwickeln von Code- oder Dokumentationsartefakten beginnen, um das Arbeitselement fertigzustellen. Wenn Sie `git checkout master` ausführen, gelangen Sie zurück zum `master`-Branch.

Eine bewährte Methode besteht darin, für jedes User Story-Arbeitselement einen Git-Branch zu erstellen. Anschließend erstellen Sie für jede Arbeitsaufgabe basierend auf dem User Story-Branch einen Branch. Organisieren Sie die Branches in einer Hierarchie, die der Beziehung zwischen User Story und Aufgabe entspricht, wenn mehrere Personen an verschiedenen User Storys für dasselbe Projekt bzw. an verschiedenen Aufgaben für dieselbe User Story arbeiten. Sie können Konflikte verringern, indem jedes Teammitglied in einem anderen Branch oder bei gemeinsamer Branchnutzung an anderem Code oder anderen Artefakten arbeitet. 

Das folgende Diagramm zeigt die empfohlene Verzweigungsstrategie für TDSP. Unter Umständen benötigen Sie nicht so viele Verzweigungen wie hier dargestellt. Dies gilt vor allem, wenn nur eine oder zwei Personen an demselben Projekt arbeiten oder nur eine Person alle Aufgaben einer User Story bearbeitet. Die bewährte Methode besteht aber immer darin, den Entwicklungsbranch vom Masterbranch zu trennen. So kann verhindert werden, dass der Releasebranch von Entwicklungsaktivitäten unterbrochen wird. Eine vollständige Beschreibung des Git-Branchmodells finden Sie unter [A Successful Git Branching Model](https://nvie.com/posts/a-successful-git-branching-model/) (Ein erfolgreiches Git-Branchmodell).

![3](./media/collaborative-coding-with-git/3-git-branches.png)

Sie können auch ein Arbeitselement mit einer vorhandenen Verzweigung verknüpfen. Wählen Sie auf der Seite **Detail** eines Arbeitselements die Option **Link hinzufügen** aus. Wählen Sie anschließend einen vorhandenen Branch aus, mit dem das Arbeitselement verknüpft werden soll, und wählen Sie **OK** aus. 

![4](./media/collaborative-coding-with-git/4-link-to-an-existing-branch.png)

## <a name='WorkonaBranchandCommittheChanges-2'></a>Arbeiten am Branch und Committen von Änderungen 

Nachdem Sie eine Änderung für Ihr Arbeitselement vorgenommen haben, z. B. das Hinzufügen einer R-Skriptdatei zum Branch `script` Ihres lokalen Computers, können Sie hierfür einen Commit von Ihrem lokalen Branch zum Upstream-Arbeitsbranch durchführen, indem Sie die folgenden Git Bash-Befehle verwenden:

```bash
git status
git add .
git commit -m "added an R script file"
git push origin script
```

![5](./media/collaborative-coding-with-git/5-sprint-push-to-branch.png)

## <a name='CreateapullrequestonVSTS-3'></a>Erstellen eines Pull Requests

Wenn Sie nach einem oder mehreren Commit- und Pushvorgängen zum Zusammenführen Ihres aktuellen Arbeitsbranchs mit dem Basisbranch bereit sind, können Sie in Azure Repos einen *Pull Request* erstellen und übermitteln. 

Greifen Sie auf der Hauptseite Ihres Azure DevOps-Projekts links im Navigationsbereich auf **Repos** > **Pull Requests** zu. Wählen Sie anschließend eine der Schaltflächen vom Typ **Neuer Pull Request** oder den Link **Einen Pull Request erstellen** aus.

![6](./media/collaborative-coding-with-git/6-spring-create-pull-request.png)

Navigieren Sie auf dem Bildschirm **Neuer Pull Request** bei Bedarf zum Git-Repository und zum Branch für die Zusammenführung Ihrer Änderungen. Sie können auch andere gewünschte Informationen hinzufügen oder ändern. Fügen Sie unter **Prüfer** die Namen der Personen hinzu, die Ihre Änderungen überprüfen sollen, und wählen Sie anschließend **Erstellen**. 

![7](./media/collaborative-coding-with-git/7-spring-send-pull-request.png)

## <a name='ReviewandMerge-4'></a>Überprüfen und Zusammenführen

Nachdem Sie den Pull Request erstellt haben, erhalten die Prüfer eine E-Mail-Benachrichtigung, um den Pull Request überprüfen zu können. Die Prüfer testen, ob die Änderungen funktionieren, und überprüfen die Änderungen ggf. gemeinsam mit dem Anforderer. Die Prüfer können Kommentare abgeben, Änderungen fordern und den Pull Request basierend auf der jeweiligen Bewertung genehmigen oder ablehnen. 

![8](./media/collaborative-coding-with-git/8-add_comments.png)

Nachdem die Prüfer die Änderungen genehmigt haben, können Sie oder eine andere Person mit Zusammenführungsberechtigungen den Arbeitsbranch mit dem zugehörigen Basisbranch zusammenführen. Wählen Sie **Vollständig** und dann im Dialogfeld **Pull Request abschließen** die Option **Vollständiger Merge** aus. Sie können den Arbeitsbranch löschen, nachdem er zusammengeführt wurde. 

![10](./media/collaborative-coding-with-git/10-spring-complete-pullrequest.png)

Vergewissern Sie sich, dass der Pull Request als **ABGESCHLOSSEN** gekennzeichnet ist. 

![11](./media/collaborative-coding-with-git/11-spring-merge-pullrequest.png)

Wenn Sie im Navigationsbereich auf der linken Seite zurück zu **Repos** wechseln, sehen Sie, dass für Sie die Umstellung auf den Masterbranch durchgeführt wurde (Branch `script` wurde gelöscht).

![12](./media/collaborative-coding-with-git/12-spring-branch-deleted.png)

Sie können auch die folgenden Git Bash-Befehle verwenden, um Ihren Arbeitsbranch `script` mit seinem Basisbranch zusammenführen und anschließend den Arbeitsbranch löschen:

```bash
git checkout master
git merge script
git branch -d script
```

![13](./media/collaborative-coding-with-git/13-spring-branch-deleted-commandline.png)

## <a name="next-steps"></a>Nächste Schritte

Unter [Ausführen von Data Science-Aufgaben](execute-data-science-tasks.md) wird gezeigt, wie Sie mit Hilfsprogrammen verschiedene allgemeine Data Science-Aufgaben wie interaktive Datenerkundung, Datenanalysen, Berichterstellung und Modellerstellung ausführen.

Unter [Exemplarische Vorgehensweisen](walkthroughs.md) sind die Vorgehensweisen für bestimmte Szenarien mit Links und Miniaturansichtsbeschreibungen aufgeführt. Anhand der Verknüpfungsszenarien wird dargestellt, wie Sie Cloud- und lokale Tools und Dienste über Workflows oder Pipelines kombinieren, um intelligente Anwendungen zu erstellen. 

