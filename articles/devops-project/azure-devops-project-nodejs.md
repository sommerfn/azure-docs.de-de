---
title: Erstellen einer CI/CD-Pipeline für Node.js mit Azure DevOps Projects
description: DevOps Projects erleichtert die ersten Schritte mit Azure. Damit können Sie eine App in einem Azure-Dienst Ihrer Wahl in einigen wenigen Schritten starten.
ms.prod: devops
ms.technology: devops-cicd
services: vsts
documentationcenter: vs-devops-build
author: mlearned
manager: gwallace
editor: ''
ms.assetid: ''
ms.workload: web
ms.tgt_pltfrm: na
ms.topic: quickstart
ms.date: 07/09/2018
ms.author: mlearned
ms.custom: mvc, seo-javascript-september2019, seo-javascript-october2019
monikerRange: vsts
ms.openlocfilehash: 11edeb35119e2c598fd83fd89c65ba4dc4679650
ms.sourcegitcommit: 1c2659ab26619658799442a6e7604f3c66307a89
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2019
ms.locfileid: "72256098"
---
#  <a name="quickstart-create-a-cicd-pipeline-in-azure-pipelines-for-nodejs-with-azure-devops-projects"></a>Schnellstart: Erstellen einer CI/CD-Pipeline in Azure Pipelines für Node.js mit Azure DevOps Projects

Azure DevOps Projects hat eine vereinfachte Oberfläche zum Erstellen von Azure-Ressourcen sowie zum Einrichten einer CI-Pipeline (Continuous Integration) und einer CD-Pipeline (Continuous Delivery) für Ihre Node.js-App in Azure Pipelines.  

Wenn Sie kein Azure-Abonnement haben, erhalten Sie über [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/) ein kostenloses Abonnement.

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Mit DevOps Projects wird eine CI/CD-Pipeline in Azure Pipelines erstellt. Sie können eine neue Azure DevOps-Organisation erstellen oder eine bestehende Organisation verwenden. Ferner werden mit DevOps Projects Azure-Ressourcen im Azure-Abonnement Ihrer Wahl erstellt.

1. Melden Sie sich beim [Microsoft Azure-Portal](https://portal.azure.com)an.

1. Wählen Sie im linken Bereich die Option **Ressource erstellen** aus, und suchen Sie nach **DevOps Projects**. 

    ![Erstellen einer Continuous Delivery-Konfigurationsressource](_img/azure-devops-project-nodejs/create-azure-resource.png)

## <a name="select-a-sample-application-and-azure-service"></a>Auswählen einer Beispielanwendung und eines Azure-Diensts

1. Wählen Sie die Node.js-Beispielanwendung aus.  
    Zu den Node.js-Beispielen zählen verschiedene Anwendungsframeworks.

1. Standardmäßig wird das Beispielframework „Express.js“ verwendet. Übernehmen Sie die Standardeinstellung, und wählen Sie **Weiter** aus.  
    **Web-App unter Windows** ist das Standardziel für die Bereitstellung.  Das Anwendungsframework, das Sie zuvor ausgewählt haben, bestimmt den Typ des hier verfügbaren Bereitstellungsziels für den Azure-Dienst.  

2. Übernehmen Sie den Standarddienst, und wählen Sie **Weiter** aus.
 
## <a name="configure-azure-devops-and-an-azure-subscription"></a>Konfigurieren von Azure DevOps und eines Azure-Abonnements 

1. Sie können eine neue Azure DevOps-Organisation erstellen oder eine bestehende Organisation auswählen. 

    a. Geben Sie einen Namen für das Projekt ein.

    b. Wählen Sie Ihr Azure-Abonnement und den Standort aus, geben Sie einen Namen für Ihre Anwendung ein, und wählen Sie dann **Fertig** aus.  
    Nach wenigen Minuten wird das DevOps Projects-Dashboard im Azure-Portal angezeigt. Eine Beispielanwendung wird in einem Repository in Ihrer Azure DevOps-Organisation eingerichtet, ein Build wird ausgeführt, und Ihre Anwendung wird in Azure bereitgestellt. Dieses Dashboard bietet Einblick in Ihr Coderepository, in Ihre CI/CD-Pipeline und in Ihre Anwendung in Azure.
     
3. Wählen Sie **Durchsuchen** aus, um Ihre ausgeführte Anwendung anzuzeigen.

    ![Dashboardansicht der CI/CD-Pipeline](_img/azure-devops-project-nodejs/devops-projects-dashboard.png) 
    
Mit DevOps Projects wird automatisch ein CI-Trigger für Build und Release konfiguriert.  Nun können Sie mithilfe eines CI/CD-Prozesses, mit dem Ihre aktuelle Arbeit an Ihrer Website automatisch bereitgestellt wird, mit einem Team an einer Node.js-App zusammenarbeiten.

## <a name="commit-code-changes-and-execute-cicd"></a>Ausführen eines Commits für Codeänderungen und Ausführen von CI/CD

DevOps Projects erstellt ein Git-Repository in Azure Repos oder GitHub. Führen Sie die folgenden Schritte aus, um das Repository anzuzeigen und Codeänderungen an Ihrer Anwendung vorzunehmen.

1. Wählen Sie auf der linken Seite des DevOps Projects-Dashboards den Link für Ihren Masterbranch aus.  
Über diesen Link wird die Ansicht des neu erstellten Git-Repositorys geöffnet.

1. Wählen Sie oben rechts im Browser die Option **Klonen** aus, um die Repository-Klon-URL anzuzeigen.   
    Sie können Ihr Git-Repository in Ihrer bevorzugten IDE klonen. Bei den nächsten Schritten können Sie den Webbrowser verwenden, um Codeänderungen direkt am Masterbranch vorzunehmen und für Codeänderungen einen Commit auszuführen.

1. Navigieren Sie auf der linken Seite des Browsers zur Datei **views/index.pug**.

1. Wählen Sie **Bearbeiten** aus, und ändern Sie die h2-Überschrift.  
    Geben Sie beispielsweise **Get started right away with Azure DevOps Projects** ein, oder nehmen Sie eine andere Änderung vor.

1. Wählen Sie **Commit** aus, und speichern Sie anschließend die Änderungen.

1. Navigieren Sie in Ihrem Browser zum DevOps Projects-Dashboard.   
Nun wird angezeigt, dass ein Buildvorgang ausgeführt wird. Die Änderungen, die Sie eben vorgenommen haben, werden automatisch erstellt und über eine CI/CD-Pipeline bereitgestellt.

## <a name="examine-the-azure-cicd-pipeline"></a>Überprüfen der Azure-CI/CD-Pipeline

Im vorherigen Schritt wurde von DevOps Projects automatisch eine vollständige CI/CD-Pipeline konfiguriert. Untersuchen Sie die Pipeline, und passen Sie sie bei Bedarf an. Führen Sie die folgenden Schritte aus, um sich mit den Build- und Releasepipelines vertraut zu machen:

1. Wählen Sie oben auf dem DevOps Projects-Dashboard die Option **Buildpipelines** aus.  
Über diesen Link werden eine Browserregisterkarte und die Buildpipeline für Ihr neues Projekt geöffnet.

1. Zeigen Sie auf das Feld **Status**, und wählen Sie dann die Auslassungspunkte (...) aus.  
    Mit dieser Aktion wird ein Menü geöffnet, über das Sie verschiedene Aktivitäten starten können. So können Sie beispielsweise einen neuen Build zur Warteschlange hinzufügen, einen Build anhalten und die Buildpipeline bearbeiten.

1. Wählen Sie **Bearbeiten** aus.

1. In diesem Bereich können Sie sich die verschiedenen Aufgaben ansehen, die Sie für Ihre Buildpipeline ausführen können.  
Vom Build werden verschiedene Aufgaben durchgeführt. Beispielsweise werden Quellen aus dem Git-Repository abgerufen, Abhängigkeiten wiederhergestellt und für Bereitstellungen verwendete Ausgaben veröffentlicht.

1. Wählen Sie oben in der Buildpipeline den Buildpipelinenamen aus.

1. Ersetzen Sie den Namen Ihrer Buildpipeline durch einen aussagekräftigeren Namen, und wählen Sie **Speichern und in Warteschlange einreihen** und dann **Speichern** aus.

1. Wählen Sie unter dem Buildpipelinenamen **Verlauf** aus.   
Im Bereich **Verlauf** wird ein Überwachungsprotokoll mit den letzten Änderungen für den Build angezeigt.  An der Buildpipeline vorgenommene Änderungen werden von Azure Pipelines nachverfolgt, sodass Sie verschiedene Versionen vergleichen können.

1. Wählen Sie **Trigger** aus.   
 Mit DevOps Projects wurde automatisch ein CI-Trigger erstellt, und mit jedem für das Repository ausgeführten Commit wird ein neuer Build gestartet.  Optional können Sie Branches aus dem CI-Prozess einbeziehen oder ausschließen.

1. Wählen Sie **Aufbewahrung** aus.   
Abhängig vom Szenario können Sie Richtlinien zum Aufbewahren oder Entfernen einer bestimmten Anzahl von Builds festlegen.

1. Wählen Sie **Build und Release** und anschließend **Releases** aus.  
 DevOps Projects erstellt eine Releasepipeline zum Verwalten von Bereitstellungen in Azure.

1. Wählen Sie auf der linken Seite neben Ihrer Releasepipeline die Auslassungspunkte (...) und anschließend **Bearbeiten** aus.  
Die Releasepipeline bestimmt den Releaseprozess.

12. Wählen Sie unter **Artefakte** die Option **Ablegen** aus.  
    Die in den vorherigen Schritten untersuchte Buildpipeline erzeugt die für das Artefakt verwendete Ausgabe. 

1. Wählen Sie neben dem Symbol **Ablegen** die Option **Continuous Deployment-Trigger** aus.  
Diese Releasepipeline enthält einen aktivierten CD-Trigger. Jedes Mal, wenn ein neues Buildartefakt verfügbar ist, wird von diesem CD-Trigger eine Bereitstellung ausgeführt. Optional können Sie den Trigger deaktivieren, sodass Ihre Bereitstellungen manuell ausgeführt werden müssen. 


1. Wählen Sie auf der linken Seite **Aufgaben** aus.   
Bei den Aufgaben handelt es sich um die Aktivitäten, die beim Bereitstellungsprozess ausgeführt werden. In diesem Beispiel wurde eine Aufgabe für die Bereitstellung in Azure App Service erstellt.


1. Wählen Sie auf der rechten Seite **Releases anzeigen** aus.  
In dieser Ansicht wird der Verlauf von Releases angezeigt.

1. Wählen Sie neben einem Ihrer Releases die Auslassungspunkte (...) und dann die Option **Öffnen** aus.  
Sie können sich verschiedene Menüs ansehen, etwa eine Releasezusammenfassung, zugeordnete Arbeitselemente und Tests.

1. Wählen Sie **Commits** aus.   
In dieser Ansicht werden die der jeweiligen Bereitstellung zugeordneten Codecommits angezeigt.

1. Wählen Sie **Protokolle** aus.  
Die Protokolle enthalten nützliche Informationen zum Bereitstellungsprozess. Sie können während und nach Bereitstellungen angezeigt werden.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Sie können die erstellte Azure App Service-Instanz und zugehörige Ressourcen löschen, wenn Sie sie nicht mehr benötigen. Verwenden Sie dazu die Funktion **Löschen** auf dem DevOps Projects-Dashboard.


## <a name="next-steps"></a>Nächste Schritte

Beim Konfigurieren des CI/CD-Prozesses wurden automatisch Build- und Releasepipelines erstellt. Diese Build- und Releasepipelines können Sie den Anforderungen Ihres Teams anpassen. Weitere Informationen zur CI/CD-Pipeline finden Sie in folgendem Artikel:

> [!div class="nextstepaction"]
> [Anpassen von CD-Prozessen](https://docs.microsoft.com/azure/devops/pipelines/release/define-multistage-release-process?view=vsts)

## <a name="videos"></a>Videos

> [!VIDEO https://www.youtube.com/embed/3etwjubReJs]
