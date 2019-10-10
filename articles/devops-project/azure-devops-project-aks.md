---
title: 'Tutorial: Bereitstellen einer ASP.NET Core-App in Azure Kubernetes Service mit Azure DevOps Projects'
description: Azure DevOps Projects erleichtert die ersten Schritte mit Azure. Mit Azure DevOps Projects können Sie Ihre ASP.NET Core-App mit Azure Kubernetes Service (AKS) in wenigen Schritten bereitstellen.
ms.author: mlearned
ms.manager: gwallace
ms.prod: devops
ms.technology: devops-cicd
ms.topic: tutorial
ms.date: 07/09/2018
author: mlearned
monikerRange: vsts
ms.openlocfilehash: b27d56d78296dc5500f97802f811a8923c4e87a8
ms.sourcegitcommit: c2e7595a2966e84dc10afb9a22b74400c4b500ed
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2019
ms.locfileid: "71969685"
---
# <a name="tutorial-deploy-aspnet-core-apps-to-azure-kubernetes-service-with-azure-devops-projects"></a>Tutorial: Bereitstellen einer ASP.NET Core-App in Azure Kubernetes Service mit Azure DevOps Projects

Azure DevOps Projects bietet eine vereinfachte Umgebung, in der Sie Ihren vorhandenen Code und Ihr Git-Repository verwenden oder eine Beispielanwendung auswählen können, um eine Continuous Integration- und Continuous Delivery-Pipeline (CI/CD) für Azure zu erstellen. 

Von DevOps Projects werden darüber hinaus die folgenden Aktionen ausgeführt:
* Automatisches Erstellen von Azure-Ressourcen wie Azure Kubernetes Service (AKS)
* Erstellen und Konfigurieren einer Releasepipeline in Azure DevOps, mit der eine Build- und Releasepipeline für CI/CD eingerichtet wird
* Erstellen einer Azure Application Insights-Ressource für die Überwachung
* Aktivieren von [Azure Monitor für Container](https://docs.microsoft.com/azure/azure-monitor/insights/container-insights-overview) zum Überwachen der Leistung für die Containerworkloads im AKS-Cluster

In diesem Lernprogramm lernen Sie Folgendes:

> [!div class="checklist"]
> * Bereitstellen einer ASP.NET Core-App in AKS mithilfe von DevOps Projects
> * Konfigurieren von Azure DevOps und eines Azure-Abonnements 
> * Untersuchen des AKS-Clusters
> * Überprüfen der CI-Pipeline
> * Überprüfen der CD-Pipeline
> * Committen von Änderungen in Git und automatisches Bereitstellen dieser Änderungen in Azure
> * Bereinigen von Ressourcen

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Über [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/) erhalten Sie ein kostenloses Abonnement.

## <a name="use-devops-projects-to-deploy-an-aspnet-core-app-to-aks"></a>Bereitstellen einer ASP.NET Core-App in AKS mithilfe von DevOps Projects

Mit DevOps Projects wird eine CI/CD-Pipeline in Azure Pipelines erstellt. Sie können eine neue Azure DevOps-Organisation erstellen oder eine bestehende Organisation verwenden. Ferner werden mit DevOps Projects Azure-Ressourcen wie etwa ein AKS-Cluster im Azure-Abonnement Ihrer Wahl erstellt.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Wählen Sie im linken Bereich **Ressource erstellen**.

1. Geben Sie in das Suchfeld **DevOps Projects** ein, und wählen sie dann **Erstellen** aus.

    ![DevOps Projects-Dashboard](_img/azure-devops-project-github/fullbrowser.png)

1. Wählen Sie **.NET** und anschließend **Weiter** aus.

1. Wählen Sie unter **Anwendungsframework auswählen** die Option **ASP.NET Core** aus.

1. Wählen Sie **Kubernetes Service** und anschließend **Weiter** aus. 

## <a name="configure-azure-devops-and-an-azure-subscription"></a>Konfigurieren von Azure DevOps und eines Azure-Abonnements

1. Sie können eine neue Azure DevOps-Organisation erstellen oder eine bestehende Organisation auswählen. 

1. Geben Sie einen Namen für Ihr Azure DevOps-Projekt ein. 

1. Wählen Sie Ihr Azure-Abonnement.

1. Wählen Sie **Ändern** aus, um zusätzliche Azure-Konfigurationseinstellungen anzuzeigen und die Anzahl von Knoten für den AKS-Cluster zu ermitteln.  
    In diesem Bereich werden verschiedene Optionen zum Konfigurieren von Typ und Speicherort der Azure-Dienste angezeigt.
 
1. Beenden Sie den Konfigurationsbereich von Azure, und wählen Sie **Fertig** aus.  
    Nach einigen Minuten ist der Vorgang abgeschlossen. Eine ASP.NET Core-Beispiel-App wird in einem Git-Repository in Ihrer Azure DevOps-Organisation eingerichtet, ein AKS-Cluster wird erstellt, eine CI/CD-Pipeline wird ausgeführt, und Ihre App wird in Azure bereitgestellt. 

    Wenn alle diese Schritte abgeschlossen sind, wird das Azure DevOps Project-Dashboard im Azure-Portal angezeigt. Sie können das DevOps Projects-Dashboard auch direkt im Azure-Portal über **Alle Ressourcen** aufrufen. 

    Dieses Dashboard bietet Einblick in Ihr Azure DevOps-Coderepository, in Ihre CI/CD-Pipeline und in Ihren AKS-Cluster. Sie können zusätzliche CI/CD-Optionen in Ihrer Azure DevOps-Pipeline konfigurieren. Wählen Sie auf der rechten Seite **Durchsuchen** aus, um Ihre ausgeführte App anzuzeigen.

## <a name="examine-the-aks-cluster"></a>Untersuchen des AKS-Clusters

DevOps Projects konfiguriert automatisch einen AKS-Cluster, den Sie untersuchen und anpassen können. Gehen Sie wie folgt vor, um sich mit dem AKS-Cluster vertraut zu machen:

1. Navigieren Sie zum DevOps Projects-Dashboard.

1. Wählen Sie auf der rechten Seite den AKS-Dienst aus.  
    Für den AKS-Cluster wird ein Bereich geöffnet. Über diese Ansicht können Sie verschiedene Aktionen ausführen. Beispielsweise können Sie die Containerintegrität überwachen, Protokolle durchsuchen und das Kubernetes-Dashboard öffnen.

1. Wählen Sie rechts die Option **Kubernetes-Dashboard anzeigen** aus.  
    Optional können Sie die Schritte zum Öffnen des Kubernetes-Dashboards ausführen.

## <a name="examine-the-ci-pipeline"></a>Überprüfen der CI-Pipeline

Mit DevOps Projects wird automatisch eine CI/CD-Pipeline in Ihrer Azure DevOps-Organisation konfiguriert. Sie können die Pipeline untersuchen und anpassen. Gehen Sie wie folgt vor, um sich damit vertraut zu machen:

1. Navigieren Sie zum DevOps Projects-Dashboard.

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
    In diesem Bereich wird ein Überwachungsprotokoll mit den letzten Änderungen für den Build angezeigt. An der Buildpipeline vorgenommene Änderungen werden von Azure DevOps nachverfolgt, sodass Sie verschiedene Versionen vergleichen können.

1. Wählen Sie **Trigger** aus.  
    Mit DevOps Projects wird automatisch ein CI-Trigger erstellt, und mit jedem für das Repository ausgeführten Commit wird ein neuer Build gestartet. Optional können Sie Branches in den CI-Prozess einbeziehen oder davon ausschließen.

1. Wählen Sie **Aufbewahrung** aus.  
    Abhängig vom Szenario können Sie Richtlinien zum Aufbewahren oder Entfernen einer bestimmten Anzahl von Builds festlegen.

## <a name="examine-the-cd-release-pipeline"></a>Überprüfen der CD-Releasepipeline

Mit DevOps Projects werden die erforderlichen Schritte zum Bereitstellen über Ihre Azure DevOps-Organisation in Ihrem Azure-Abonnement automatisch erstellt und konfiguriert. Diese Schritte umfassen die Konfiguration einer Azure-Dienstverbindung zur Authentifizierung von Azure DevOps für Ihr Azure-Abonnement. Die Automatisierung erstellt darüber hinaus eine Releasepipeline, mit der CD in Azure bereitgestellt wird. Gehen Sie wie folgt vor, um weitere Informationen zur Releasepipeline zu erhalten:

1. Wählen Sie **Build und Release** und anschließend **Releases** aus.  
    DevOps Projects erstellt eine Releasepipeline zum Verwalten von Bereitstellungen in Azure.

1. Wählen Sie neben Ihrer Releasepipeline die Auslassungspunkte (...) und anschließend **Bearbeiten** aus.  
    Die Releasepipeline enthält eine *Pipeline*, die den Releaseprozess definiert.

1. Wählen Sie unter **Artefakte** die Option **Ablegen** aus.  
    Die in den vorherigen Schritten untersuchte Buildpipeline erzeugt die für das Artefakt verwendete Ausgabe. 

1. Wählen Sie rechts neben dem Symbol **Ablegen** die Option **Continuous Deployment-Trigger** aus.  
    Diese Releasepipeline enthält einen aktivierten CD-Trigger. Jedes Mal, wenn ein neues Buildartefakt verfügbar ist, wird von diesem CD-Trigger eine Bereitstellung ausgeführt. Optional können Sie den Trigger deaktivieren, sodass Ihre Bereitstellungen manuell ausgeführt werden müssen. 

1. Wählen Sie auf der rechten Seite **Releases anzeigen** aus, um einen Releaseverlauf anzuzeigen.

1. Wählen Sie neben einem Release die Auslassungspunkte (...) und anschließend **Öffnen** aus.  
    Sie können sich verschiedene Menüs ansehen, etwa eine Releasezusammenfassung, zugeordnete Arbeitselemente und Tests.

1. Wählen Sie **Commits** aus.  
    In dieser Ansicht werden die dieser Bereitstellung zugeordneten Codecommits angezeigt. Vergleichen Sie Releases, um die Commitunterschiede zwischen den einzelnen Bereitstellungen anzuzeigen.

1. Wählen Sie **Protokolle** aus.  
    Die Protokolle enthalten nützliche Informationen zum Bereitstellungsprozess. Sie können während und nach Bereitstellungen angezeigt werden.

## <a name="commit-changes-to-azure-repos-and-automatically-deploy-them-to-azure"></a>Committen von Änderungen in Azure Repos und automatisches Bereitstellen dieser Änderungen in Azure 

 > [!NOTE]
 > Mit dem folgenden Verfahren wird die CI/CD-Pipeline durch eine einfache Textänderung getestet.

Nun können Sie mithilfe eines CI/CD-Prozesses, mit dem Ihre aktuelle Arbeit auf Ihrer Website automatisch bereitgestellt wird, mit einem Team an Ihrer App zusammenarbeiten. Mit jeder Änderung am GitHub-Repository wird in Azure DevOps ein Build gestartet, und durch eine CD-Pipeline wird eine Bereitstellung in Azure ausgeführt. Führen Sie die Schritte in diesem Abschnitt aus, oder nutzen Sie eine andere Methode zum Committen Ihrer Änderungen in Ihrem Repository. Sie können beispielsweise das Git-Repository in Ihrem bevorzugten Tool oder Ihrer bevorzugten IDE klonen und anschließend Änderungen per Push in dieses Repository übertragen.

1. Wählen Sie im Azure DevOps-Menü **Code** > **Dateien** aus, und navigieren Sie zu Ihrem Repository.

1. Navigieren Sie zum Verzeichnis *Views\Home*. Wählen Sie anschließend die Auslassungspunkte neben der Datei *Index.cshtml* und dann **Bearbeiten** aus.

1. Nehmen Sie eine Änderung an der Datei vor. Fügen Sie beispielsweise in einem der div-Tags Text hinzu. 

1. Wählen Sie oben rechts **Committen** und dann erneut **Committen** aus, um Ihre Änderung per Push zu übertragen.  
    Kurz danach wird in Azure DevOps ein Build gestartet und zur Bereitstellung der Änderungen ein Release ausgeführt. Überwachen Sie den Buildstatus auf dem DevOps Projects-Dashboard oder im Browser mit Ihrer Azure DevOps-Organisation.

1. Aktualisieren Sie nach Abschluss des Release Ihre App, um Ihre Änderungen zu überprüfen.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Beim Durchführen von Tests können Sie Gebühren vermeiden, indem Sie die Ressourcen bereinigen. Sie können den in diesem Tutorial erstellten AKS-Cluster und zugehörige Ressourcen löschen, wenn Sie sie nicht mehr benötigen. Verwenden Sie dazu die Funktion **Löschen** auf dem DevOps Projects-Dashboard.

> [!IMPORTANT]
> Mit dem folgenden Verfahren werden Ressourcen endgültig gelöscht. Mit der Funktion *Löschen* werden die Daten, die vom Projekt in DevOps Projects erstellt wurden, in Azure und Azure DevOps gelöscht. Diese Daten können anschließend nicht wiederhergestellt werden. Verwenden Sie dieses Verfahren nur, nachdem Sie die Anweisungen sorgfältig gelesen haben.

1. Navigieren Sie im Azure-Portal zum DevOps Projects-Dashboard.
2. Wählen Sie oben rechts **Löschen** aus. 
3. Wählen Sie an der Eingabeaufforderung **Ja** aus, um die Ressourcen *endgültig zu löschen*.

## <a name="next-steps"></a>Nächste Schritte

Diese Build- und Releasepipelines können Sie optional an die Anforderungen Ihres Teams anpassen. Sie können dieses CI/CD-Muster auch als Vorlage für Ihre anderen Pipelines verwenden. In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Bereitstellen einer ASP.NET Core-App in AKS mithilfe von DevOps Projects
> * Konfigurieren von Azure DevOps und eines Azure-Abonnements 
> * Untersuchen des AKS-Clusters
> * Überprüfen der CI-Pipeline
> * Überprüfen der CD-Pipeline
> * Committen von Änderungen in Git und automatisches Bereitstellen dieser Änderungen in Azure
> * Bereinigen von Ressourcen

Weitere Informationen zur Verwendung des Kubernetes-Dashboards finden Sie im folgenden Artikel:

> [!div class="nextstepaction"]
> [Verwenden des Kubernetes-Dashboards](https://docs.microsoft.com/azure/aks/kubernetes-dashboard)
