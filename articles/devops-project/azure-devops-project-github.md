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
ms.date: 06/27/2019
author: mlearned
ms.custom: mvc
monikerRange: vsts
ms.openlocfilehash: 4a48b5c413f6dfb58a1ba3f896ded49250cde306
ms.sourcegitcommit: e0a1a9e4a5c92d57deb168580e8aa1306bd94723
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2019
ms.locfileid: "72286329"
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
> * Bereinigen von Ressourcen

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Über [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/) erhalten Sie ein kostenloses Abonnement.
* Zugriff auf ein GitHub- oder ein externes Git-Repository, das .NET, Java, PHP, Node, Python oder statischen Webcode enthält

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Mit Azure DevOps Projects wird eine CI/CD-Pipeline in Azure Pipelines erstellt. Sie können eine neue Azure DevOps-Organisation erstellen oder eine bestehende Organisation verwenden. Ferner werden mit Azure DevOps Projects Azure-Ressourcen im Azure-Abonnement Ihrer Wahl erstellt.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Wählen Sie im linken Bereich auf der linken Navigationsleiste **+Ressource erstellen** aus. Suchen Sie nach **DevOps Projects**, und wählen Sie **Erstellen** aus.

   ![DevOps Projects-Dashboard](_img/azure-devops-project-github/azuredashboard.png)

3. Wählen Sie **Eigenen Code verwenden** und anschließend **Weiter** aus.

## <a name="configure-access-to-your-github-repo-and-choose-a-framework"></a>Konfigurieren des Zugriffs auf Ihr GitHub-Repository und Auswählen eines Frameworks

1. Wählen Sie entweder **GitHub** oder ein externes **Git**-Coderepository aus. Wählen Sie für dieses Tutorial die Option **GitHub** aus. Wenn Sie Azure erstmals Zugriff auf Ihr GitHub-Repository gewähren, ist unter Umständen eine GitHub-Authentifizierung erforderlich.

2. Wählen Sie abschließend **Repository** und **Branch** und schließlich **Weiter** aus.

3. Bei Verwendung von Docker-Containern muss **Ist die App dockerisiert?** in **JA** geändert werden. Behalten Sie jedoch für dieses Tutorial die Einstellung **NEIN** bei, und wählen Sie **Weiter** aus. Weitere Informationen zur Verwendung von Docker-Containern erhalten Sie, indem Sie mit dem Mauszeiger auf das Symbol **i** zeigen.

   ![.NET Framework](_img/azure-devops-project-github/appframework.png)

4. Wählen Sie in den Dropdownlisten **Anwendungsruntime auswählen** und **Anwendungsframework auswählen** die Anwendungsruntime und das Anwendungsframework aus, und wählen Sie anschließend **Weiter** aus. Das ausgewählte Anwendungsframework bestimmt die Art des verfügbaren Bereitstellungsziels für den Azure-Dienst.

5. Wählen Sie unter **Azure-Dienst** auch den Azure-Dienst für die Anwendungsbereitstellung aus, und wählen Sie anschließend **Weiter** aus.

## <a name="configure-azure-devops-and-an-azure-subscription"></a>Konfigurieren von Azure DevOps und eines Azure-Abonnements

1. Geben Sie einen **Projektnamen** ein.

2. Sie können eine neue kostenlose **Azure DevOps-Organisation** erstellen oder eine bestehende Organisation in der Dropdownliste auswählen.

3. Wählen Sie Ihr **Azure-Abonnement** aus, geben Sie einen Namen für Ihre **Web-App** ein, oder behalten Sie den Standardwert bei. Wählen Sie einen **Standort** und anschließend **Fertig** aus. Nach wenigen Minuten wird die DevOps-Projektbereitstellungsübersicht im Azure-Portal angezeigt.

4. Wählen Sie **Zu Ressource wechseln** aus, um das DevOps Project-Dashboard anzuzeigen. Heften Sie für den Schnellzugriff das **Projekt** in der oberen rechten Ecke an Ihr Dashboard. Mit Azure DevOps Projects wird automatisch ein CI-Trigger für Build und Release konfiguriert. Ihr Code verbleibt in Ihrem GitHub-Repository oder einem anderen externen Repository. Eine Beispiel-App wird in einem Repository in Ihrer **Azure DevOps-Organisation** eingerichtet. Ein Build wird ausgeführt und Ihre App in Azure bereitgestellt.

   ![DevOps Projects-Dashboardansicht](_img/azure-devops-project-github/projectsdashboard.png)

5. Dieses Dashboard bietet Einblick in Ihr Coderepository, in Ihre CI/CD-Pipeline und in Ihre App in Azure. Wählen Sie auf der rechten Seite unter Azure-Ressourcen **Durchsuchen** aus, um Ihre ausgeführte App anzuzeigen.

## <a name="commit-changes-to-github-and-automatically-deploy-them-to-azure"></a>Committen von Änderungen in GitHub und automatisches Bereitstellen dieser Änderungen in Azure

Nun können Sie mithilfe eines CI/CD-Prozesses, mit dem Ihre aktuelle Arbeit auf Ihrer Website automatisch bereitgestellt wird, mit einem Team an Ihrer App zusammenarbeiten. Mit jeder Änderung am GitHub-Repository wird in Azure DevOps ein Build gestartet, und durch eine CD-Pipeline wird eine Bereitstellung in Azure ausgeführt.

1. Wählen Sie auf Ihrem DevOps-Projektdashboard die Option **Repositorys** aus. Ihr GitHub-Repository wird auf einer neuen Browserregisterkarte geöffnet. Ändern Sie Ihre Anwendung, und klicken Sie auf **Commit Changes** (Änderungen committen).

2. Nach wenigen Augenblicken wird in Azure Pipelines ein Build gestartet. Sie können den Buildstatus auf dem DevOps Projects-Dashboard oder in Ihrer Azure DevOps-Organisation überwachen, indem Sie auf dem Projektdashboard die Registerkarte **Buildpipelines** auswählen.

## <a name="examine-the-azure-pipelines-cicd-pipeline"></a>Überprüfen der CI/CD-Pipeline in Azure Pipelines

Mit Azure DevOps Projects wird automatisch eine CI/CD-Pipeline in Azure Pipelines konfiguriert. Untersuchen Sie die Pipeline, und passen Sie sie bei Bedarf an. Gehen Sie wie folgt vor, um sich mit den Build- und Releasepipelines vertraut zu machen:

1. Wählen Sie auf dem DevOps Projects-Dashboard die Option **Buildpipelines** aus.

2. Auf der Seite **Azure Pipelines** werden ein Verlauf der neuesten Builds sowie der Status der einzelnen Builds angezeigt.

   ![Azure DevOps-Pipelinebuilds](_img/azure-devops-project-github/pipelinesbuildpage.png)

3. In der rechten oberen Ecke der Seite **Builds** stehen verschiedene Optionen zur Verfügung: Mit **Bearbeiten** können Sie den aktuellen Build bearbeiten. Mit **Warteschlange** können Sie einen neuen Build in die Warteschlange einreihen, und über die Auslassungspunkte ( **&#8942;** ) können Sie ein Menü mit weiteren Optionen öffnen. Wählen Sie **Bearbeiten** aus.

4. Der Build führt verschiedene Aufgaben durch. Beispielsweise werden Quellen aus dem Repository abgerufen, Abhängigkeiten wiederhergestellt und Ausgaben für Bereitstellungen veröffentlicht. Ändern Sie auf der rechten Seite unter **Name** den Namen der Buildpipeline in einen aussagekräftigeren Namen. Wählen Sie **Speichern und in Warteschlange einreihen** und anschließend **Speichern** aus. Schreiben Sie einen Kommentar, und wählen Sie anschließend erneut **Speichern** aus.

   ![Azure DevOps-Seite „Builds“](_img/azure-devops-project-github/buildpage.png)

5. Wählen Sie die Registerkarte **Verlauf** aus, um ein Überwachungsprotokoll Ihrer kürzlich vorgenommenen Änderungen für den Build anzuzeigen. An der Buildpipeline vorgenommene Änderungen werden von Azure DevOps nachverfolgt, sodass Sie verschiedene Versionen vergleichen können.

6. Wählen Sie die Registerkarte **Trigger** aus. Das Azure DevOps-Projekt erstellt automatisch einen CI-Trigger mit einigen Standardeinstellungen. Trigger wie **Continuous Integration aktivieren** können festgelegt werden, damit nach jedem Commit einer Codeänderung ein Buildvorgang ausgeführt wird. Außerdem können Buildvorgänge für bestimmte Zeiten geplant werden.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Sie können den in diesem Tutorial erstellten Azure-App-Dienst und zugehörige Ressourcen löschen, wenn Sie sie nicht mehr benötigen. Verwenden Sie dazu die Funktion **Löschen** auf dem DevOps Projects-Dashboard.

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie den CI/CD-Prozess in diesem Tutorial konfiguriert haben, wurden in Azure DevOps Projects automatisch eine Build- und eine Releasepipeline erstellt. Diese Build- und Releasepipelines können Sie den Anforderungen Ihres Teams anpassen. Es wurde Folgendes vermittelt:

> [!div class="checklist"]
>  * Erstellen einer CI/CD-Pipeline mithilfe von DevOps Projects
> * Konfigurieren des Zugriffs auf Ihr GitHub-Repository und Auswählen eines Frameworks
> * Konfigurieren von Azure DevOps und eines Azure-Abonnements
> * Committen von Änderungen in GitHub und automatisches Bereitstellen dieser Änderungen in Azure
> * Überprüfen der CI/CD-Pipeline in Azure Pipelines
> * Bereinigen von Ressourcen

Weitere Informationen zur CI/CD-Pipeline finden Sie in folgendem Artikel:

> [!div class="nextstepaction"]
> [Define your multi-stage continuous deployment (CD) pipeline](https://docs.microsoft.com/azure/devops/pipelines/release/define-multistage-release-process?view=vsts) (Festlegen Ihrer mehrstufigen CD-Pipeline (Continuous Deployment))

Weitere Informationen zur Anwendungsüberwachung finden Sie hier:
  
 > [!div class="nextstepaction"]
 > [Azure Monitor – Übersicht](https://docs.microsoft.com/azure/azure-monitor/overview)
