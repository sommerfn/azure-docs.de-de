---
title: 'Tutorial: Erstellen einer CI/CD-Pipeline für vorhandenen Code mit Azure DevOps Projects'
description: Azure DevOps Projects erleichtert die ersten Schritte mit Azure. Mit DevOps Projects können Sie Ihren eigenen Code und das GitHub-Repository verwenden, um eine App in einem Azure-Dienst Ihrer Wahl in wenigen Schritten zu starten.
services: vsts
documentationcenter: vs-devops-build
ms.author: mlearned
ms.manager: gwallace
editor: ''
ms.assetid: ''
ms.workload: web
ms.prod: devops
ms.technology: devops-cicd
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 07/09/2018
author: mlearned
ms.custom: mvc
monikerRange: vsts
ms.openlocfilehash: 2abe24ad65e1e8997b48a28b35ec0e65162022f2
ms.sourcegitcommit: 3e7646d60e0f3d68e4eff246b3c17711fb41eeda
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2019
ms.locfileid: "70898010"
---
# <a name="tutorial-create-a-cicd-pipeline-for-your-existing-code-by-using-azure-devops-projects"></a>Tutorial: Erstellen einer CI/CD-Pipeline für vorhandenen Code mit Azure DevOps Projects

Azure DevOps Projects bietet eine vereinfachte Umgebung, in der Sie Ihren vorhandenen Code und Ihr Git-Repository verwenden oder eine Beispielanwendung auswählen können, um eine Continuous Integration- und Continuous Delivery-Pipeline (CI/CD) für Azure zu erstellen.

In diesem Tutorial führen Sie folgende Schritte aus:

> [!div class="checklist"]
> * Erstellen einer CI/CD-Pipeline mithilfe von DevOps Projects
> * Konfigurieren des Zugriffs auf Ihr GitHub-Repository und Auswählen eines Frameworks
> * Konfigurieren von Azure DevOps und eines Azure-Abonnements 
> * Committen von Änderungen in GitHub und automatisches Bereitstellen dieser Änderungen in Azure
> * Überprüfen der CI/CD-Pipeline in Azure Pipelines
> * Konfigurieren der Azure Application Insights-Überwachung
> * Bereinigen von Ressourcen

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Über [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/) erhalten Sie ein kostenloses Abonnement.
* Zugriff auf ein GitHub- oder ein externes Git-Repository, das .NET, Java, PHP, Node, Python oder statischen Webcode enthält

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Mit Azure DevOps Projects wird eine CI/CD-Pipeline in Azure Pipelines erstellt. Sie können eine neue Azure DevOps-Organisation erstellen oder eine bestehende Organisation verwenden. Ferner werden mit Azure DevOps Projects Azure-Ressourcen im Azure-Abonnement Ihrer Wahl erstellt.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Wählen Sie im linken Bereich **Neu** aus.

1. Geben Sie in das Suchfeld **DevOps Projects** ein, und wählen sie dann **Erstellen** aus.

    ![DevOps Projects-Dashboard](_img/azure-devops-project-github/fullbrowser.png)

1. Wählen Sie **Eigenen Code verwenden** und anschließend **Weiter** aus.

## <a name="configure-access-to-your-github-repo-and-choose-a-framework"></a>Konfigurieren des Zugriffs auf Ihr GitHub-Repository und Auswählen eines Frameworks

1. Wählen Sie entweder **GitHub** oder ein externes Git-Repository und anschließend Ihr Repository und den Branch mit Ihrer Anwendung aus.

1. Wählen Sie Ihr Webframework und dann **Weiter** aus.

    ![.NET Framework](_img/azure-devops-project-github/webframework.png)

    Das Anwendungsframework, das Sie zuvor ausgewählt haben, bestimmt den Typ des hier verfügbaren Bereitstellungsziels für den Azure-Dienst. 
    
1. Wählen Sie den Zieldienst und anschließend **Weiter** aus.

## <a name="configure-azure-devops-and-an-azure-subscription"></a>Konfigurieren von Azure DevOps und eines Azure-Abonnements 

1. Sie können eine neue Azure DevOps-Organisation erstellen oder eine bestehende Organisation auswählen.

    a. Geben Sie einen Namen für Ihr Projekt in Azure DevOps ein. 
    
    b. Wählen Sie Ihr Azure-Abonnement und den Standort aus, geben Sie einen Namen für Ihre Anwendung ein, und wählen Sie dann **Fertig** aus.

    Nach wenigen Minuten wird das DevOps Projects-Dashboard im Azure-Portal angezeigt. Eine Beispielanwendung wird in einem Repository in Ihrer Azure DevOps-Organisation eingerichtet, ein Build wird ausgeführt, und Ihre Anwendung wird in Azure bereitgestellt. Dieses Dashboard bietet Einblick in Ihr GitHub-Coderepository, in Ihre CI/CD-Pipeline und in Ihre Anwendung in Azure. 
    
1. Wählen Sie **Durchsuchen** aus, um Ihre ausgeführte Anwendung anzuzeigen.

    ![DevOps Projects-Dashboardansicht](_img/azure-devops-project-github/dashboardnopreview.png) 
    
Mit Azure DevOps Projects wird automatisch ein CI-Trigger für Build und Release konfiguriert. Ihr Code verbleibt in Ihrem GitHub-Repository oder einem anderen externen Repository. 

## <a name="commit-changes-to-github-and-automatically-deploy-them-to-azure"></a>Committen von Änderungen in GitHub und automatisches Bereitstellen dieser Änderungen in Azure 

Nun können Sie mithilfe eines CI/CD-Prozesses, mit dem Ihre aktuelle Arbeit auf Ihrer Website automatisch bereitgestellt wird, mit einem Team an Ihrer App zusammenarbeiten. Mit jeder Änderung am GitHub-Repository wird in Azure DevOps ein Build gestartet, und durch eine CD-Pipeline wird eine Bereitstellung in Azure ausgeführt.

1. Nehmen Sie eine Änderung an Ihrer Anwendung vor, und committen Sie die Änderung in Ihrem GitHub-Repository.  
    Nach wenigen Augenblicken wird in Azure Pipelines ein Build gestartet. Sie können den Buildstatus auf dem DevOps Projects-Dashboard oder im Browser mit Ihrer Azure DevOps-Organisation überwachen.

1. Aktualisieren Sie nach Abschluss des Builds Ihre Anwendung, um Ihre Änderungen zu überprüfen.

## <a name="examine-the-azure-pipelines-cicd-pipeline"></a>Überprüfen der CI/CD-Pipeline in Azure Pipelines

Mit Azure DevOps Projects wird automatisch eine CI/CD-Pipeline in Azure Pipelines konfiguriert. Untersuchen Sie die Pipeline, und passen Sie sie bei Bedarf an. Gehen Sie wie folgt vor, um sich mit den Build- und Releasepipelines vertraut zu machen:

1. Wählen Sie oben auf dem DevOps Projects-Dashboard die Option **Buildpipelines** aus.  
    Auf einer Browserregisterkarte wird die Buildpipeline für Ihr neues Projekt angezeigt.

1. Zeigen Sie auf das Feld **Status**, und wählen Sie dann die Auslassungspunkte (...) aus.  
    In einem Menü werden verschiedene Optionen angezeigt, etwa zum Einreihen eines neuen Builds in die Warteschlange, zum Anhalten eines Builds und zum Bearbeiten der Buildpipeline.

1. Wählen Sie **Bearbeiten** aus.

1. In diesem Bereich können Sie sich die verschiedenen Aufgaben ansehen, die Sie für Ihre Buildpipeline ausführen können.  
    Vom Build werden verschiedene Aufgaben durchgeführt. Beispielsweise werden Quellen aus dem Git-Repository abgerufen, Abhängigkeiten wiederhergestellt und für Bereitstellungen verwendete Ausgaben veröffentlicht.

1. Wählen Sie oben in der Buildpipeline den Buildpipelinenamen aus.

1. Ersetzen Sie den Namen Ihrer Buildpipeline durch einen aussagekräftigeren Namen, und wählen Sie **Speichern und in Warteschlange einreihen** und dann **Speichern** aus.

1. Wählen Sie unter dem Buildpipelinenamen **Verlauf** aus.  
    Es wird ein Überwachungspfad mit den letzten Änderungen für den Build angezeigt. An der Buildpipeline vorgenommene Änderungen werden von Azure DevOps nachverfolgt, sodass Sie verschiedene Versionen vergleichen können.

1. Wählen Sie **Trigger** aus.  
    Mit Azure DevOps Projects wird automatisch ein CI-Trigger erstellt, und mit jedem für das Repository ausgeführten Commit wird ein neuer Build gestartet. Optional können Sie Branches in den CI-Prozess einbeziehen oder davon ausschließen.

1. Wählen Sie **Aufbewahrung** aus.  
        Abhängig vom Szenario können Sie Richtlinien zum Aufbewahren oder Entfernen einer bestimmten Anzahl von Builds festlegen.

1. Wählen Sie **Build und Release** und anschließend **Releases** aus.  
    Azure DevOps Projects erstellt eine Releasepipeline zum Verwalten von Bereitstellungen in Azure.

1. Wählen Sie neben Ihrer Releasepipeline die Auslassungspunkte (...) und anschließend **Bearbeiten** aus.  
    Die Releasepipeline enthält eine *Pipeline*, die den Releaseprozess definiert. 
    
1. Wählen Sie unter **Artefakte** die Option **Ablegen** aus.  
    Die in den vorherigen Schritten untersuchte Buildpipeline erzeugt die für das Artefakt verwendete Ausgabe. 

1. Wählen Sie neben dem Symbol **Ablegen** die Option **Continuous Deployment-Trigger** aus.  
    Diese Releasepipeline enthält einen aktivierten CD-Trigger. Jedes Mal, wenn ein neues Buildartefakt verfügbar ist, wird von diesem CD-Trigger eine Bereitstellung ausgeführt. Optional können Sie den Trigger deaktivieren, sodass Ihre Bereitstellungen manuell ausgeführt werden müssen. 

1. Wählen Sie auf der linken Seite **Aufgaben** aus.  
    Bei Aufgaben handelt es sich um die Aktivitäten, die beim Bereitstellungsprozess ausgeführt werden. In diesem Beispiel wurde für die Bereitstellung im Azure-App-Dienst eine Aufgabe erstellt.

1. Wählen Sie auf der rechten Seite **Releases anzeigen** aus, um einen Releaseverlauf anzuzeigen.

1. Wählen Sie neben einem Release die Auslassungspunkte (...) und anschließend **Öffnen** aus.  
    Sie können sich verschiedene Menüs ansehen, etwa eine Releasezusammenfassung, zugeordnete Arbeitselemente und Tests.

1. Wählen Sie **Commits** aus.  
    In dieser Ansicht werden die dieser Bereitstellung zugeordneten Codecommits angezeigt. 

1. Wählen Sie **Protokolle** aus.  
    Die Protokolle enthalten nützliche Informationen zum Bereitstellungsprozess. Sie können während und nach Bereitstellungen angezeigt werden.

## <a name="configure-azure-application-insights-monitoring"></a>Konfigurieren der Azure Application Insights-Überwachung

Mithilfe von Azure Application Insights können Sie die Leistung und Nutzung Ihrer Anwendung ganz einfach überwachen. Mit Azure DevOps Projects wird für Ihre Anwendung automatisch eine Application Insights-Ressource konfiguriert. Sie können Warnungen und Überwachungsfunktionen je nach Bedarf weiter konfigurieren.

1. Navigieren Sie im Azure-Portal zum DevOps Projects-Dashboard. 

1. Wählen Sie unten rechts den Link **Application Insights** für Ihre App aus.  
    Der Bereich **Application Insights** wird geöffnet. Diese Ansicht enthält Informationen zur Nutzungs-, Leistungs- und Verfügbarkeitsüberwachung für Ihre App.

    ![Der Bereich „Application Insights“](_img/azure-devops-project-github/appinsights.png) 

1. Wählen Sie **Zeitbereich** und dann **Letzte Stunde** aus. Wählen Sie zum Filtern der Ergebnisse **Aktualisieren** aus.  
    Sie können nun alle Aktivitäten der letzten 60 Minuten anzeigen. Wählen Sie zum Schließen des Zeitbereichs **x** aus.

1. Wählen Sie **Warnungen** und anschließend **Metrikwarnung hinzufügen** aus. 

1. Geben Sie einen Namen für die Warnung ein.

1. Wählen sie in der Dropdownliste **Source Alter on** (Quellwarnung auf) Ihre **App Service-Ressource** aus. <!-- Please confirm whether this should be "Source Alter on" or "Source Alert on" -->

1. Sehen Sie sich in der Dropdownliste **Metrik** die verschiedenen Warnungsmetriken an.  
    Für die Standardwarnung gilt eine **Serverantwortzeit, die größer als 1 Sekunde** ist. Sie können problemlos eine Vielzahl von Warnungen konfigurieren und dadurch die Überwachungsfunktionen Ihrer App verbessern.

1. Aktivieren Sie das Kontrollkästchen für **Notify via Email owners, contributors, and readers** (Besitzer, Mitwirkende und Leser über E-Mail informieren).  
    Optional können Sie durch die Ausführung einer Logik-App zusätzliche Aktionen ausführen, wenn eine Warnung angezeigt wird.

1. Wählen Sie **OK** , um die Warnung zu erstellen.  
    Nach wenigen Augenblicken wird die Warnung auf dem Dashboard als aktiv angezeigt.
    
1. Verlassen Sie den Bereich **Warnung**, und navigieren Sie zurück zum Blatt **Application Insights**.

1. Wählen Sie **Verfügbarkeit** und anschließend **Test hinzufügen** aus. 

1. Geben Sie einen Testnamen ein, und wählen Sie dann **Erstellen** aus.  
    Ein einfacher Ping-Test wird erstellt, um die Verfügbarkeit Ihrer Anwendung zu überprüfen. Nach wenigen Minuten sind die Testergebnisse verfügbar, und das Application Insights-Dashboard zeigt einen Verfügbarkeitsstatus an.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Sie können den in diesem Tutorial erstellten Azure-App-Dienst und zugehörige Ressourcen löschen, wenn Sie sie nicht mehr benötigen. Verwenden Sie dazu die Funktion **Löschen** auf dem DevOps Projects-Dashboard.

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie den CI/CD-Prozess in diesem Tutorial konfiguriert haben, wurden in Azure DevOps Projects automatisch eine Build- und eine Releasepipeline erstellt. Diese Build- und Releasepipelines können Sie den Anforderungen Ihres Teams anpassen. Es wurde Folgendes vermittelt:

> [!div class="checklist"]
> * Erstellen einer CI/CD-Pipeline mithilfe von DevOps Projects
> * Konfigurieren des Zugriffs auf Ihr GitHub-Repository und Auswählen eines Frameworks
> * Konfigurieren von Azure DevOps und eines Azure-Abonnements 
> * Committen von Änderungen in GitHub und automatisches Bereitstellen dieser Änderungen in Azure
> * Überprüfen der CI/CD-Pipeline in Azure Pipelines
> * Konfigurieren der Azure Application Insights-Überwachung
> * Bereinigen von Ressourcen

Weitere Informationen zur CI/CD-Pipeline finden Sie in folgendem Artikel:

> [!div class="nextstepaction"]
> [Define your multi-stage continuous deployment (CD) pipeline](https://docs.microsoft.com/azure/devops/pipelines/release/define-multistage-release-process?view=vsts) (Festlegen Ihrer mehrstufigen CD-Pipeline (Continuous Deployment))
