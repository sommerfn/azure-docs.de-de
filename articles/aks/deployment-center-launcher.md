---
title: Verwenden des Bereitstellungscenters in Azure App Service
description: Das Bereitstellungscenter in Azure DevOps vereinfacht die Einrichtung einer stabilen Azure DevOps-Pipeline für Ihre Anwendung.
ms.author: puagarw
ms.manager: douge
ms.prod: devops
ms.technology: devops-cicd
ms.topic: tutorial
ms.date: 07/12/2019
author: pulkitaggarwl
monikerRange: vsts
ms.openlocfilehash: 8d1e467906b74c97c8b4f4e5c14af0814dd098f7
ms.sourcegitcommit: 470041c681719df2d4ee9b81c9be6104befffcea
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2019
ms.locfileid: "67861514"
---
# <a name="deployment-center-launcher"></a>Startprogramm für Bereitstellungscenter

Das Bereitstellungscenter in Azure DevOps vereinfacht die Einrichtung einer stabilen DevOps-Pipeline für Ihre Anwendung. Standardmäßig wird eine DevOps-Pipeline zum Bereitstellen Ihrer Anwendungsupdates im Kubernetes-Cluster konfiguriert. Sie können die standardmäßig konfigurierte DevOps-Pipeline erweitern und zusätzliche DevOps-Funktionen hinzufügen – z. B. Genehmigungen vor der Bereitstellung, Bereitstellung zusätzlicher Azure-Ressourcen, Ausführung von Skripts, Upgrades Ihrer Anwendung oder sogar die Ausführung zusätzlicher Validierungstests.

In diesem Lernprogramm lernen Sie Folgendes:

> [!div class="checklist"]
> * Konfigurieren einer DevOps-Pipeline zum Bereitstellen Ihrer Anwendungsupdates im K8s-Cluster
> * Überprüfen der CI-Pipeline
> * Überprüfen der CD-Pipeline
> * Bereinigen der Ressourcen

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Über [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/) erhalten Sie ein kostenloses Abonnement.

* AKS-Cluster (Azure Kubernetes Service)

## <a name="create-aks-cluster"></a>Erstellen eines ACS-Clusters

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

1. Wählen Sie im Azure-Portal rechts oben im Menü die Schaltfläche [Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) aus.

1. Führen Sie die folgenden Befehle aus, um den AKS-Cluster zu erstellen:

    ```cmd
    # The below command creates Resource Group in the south india location

    az group create --name azooaks --location southindia

    # The below command creates a cluster named azookubectl with one node. 

    az aks create --resource-group azooaks --name azookubectl --node-count 1 --enable-addons monitoring --generate-ssh-keys
    ```

## <a name="deploy-application-updates-to-k8s-cluster"></a>Bereitstellen von Anwendungsupdates in einem K8s-Cluster

1. Navigieren Sie zu der oben erstellten Ressourcengruppe.

1. Wählen Sie den AKS-Cluster aus, und klicken Sie auf dem linken Blatt unter „Einstellungen“ auf **Bereitstellungscenter (Vorschau)** . Klicken Sie auf **Erste Schritte**.

   ![settings](media/deployment-center-launcher/settings.png)

1. Wählen Sie den Speicherort des Codes aus, und klicken Sie auf **Weiter**. Derzeit werden die Repositorys **[Azure Repos](https://docs.microsoft.com/azure/devops/repos/index?view=azure-devops)** und **GitHub** unterstützt. Führen Sie basierend auf der Repositoryauswahl die nachfolgenden Schritte aus.

    Bei Azure Repos handelt es sich um einen Satz von Tools für die Versionskontrolle, mit denen Sie Ihren Code verwalten können. Unabhängig von der Größe Ihres Softwareprojekts empfiehlt es sich, die Versionskontrolle so früh wie möglich zu verwenden.

    - **Azure Repos:** Wählen Sie ein Repository aus Ihrem vorhandenen Projekt und Ihrer Organisation aus.

        ![Azure Repos](media/deployment-center-launcher/azure-repos.gif)

    - **GitHub:** Autorisieren Sie das Repository für Ihr GitHub-Konto, und wählen Sie es aus.

        ![GitHub](media/deployment-center-launcher/github.gif)


1. Wir analysieren das Repository und ermitteln das Dockerfile. Zur Aktualisierung des Dockerfile können Sie die ermittelte Portnummer bearbeiten.

    ![Anwendungseinstellungen](media/deployment-center-launcher/application-settings.png)

    Ist im Repository kein Dockerfile enthalten, wird vom System eine Meldung mit dem Hinweis angezeigt, dass eins committet werden muss. 

    ![Dockerfile](media/deployment-center-launcher/dockerfile.png)

1. Wählen Sie eine vorhandene Containerregistrierung aus, oder erstellen Sie eine, und klicken Sie auf **Fertig stellen**. Die Pipeline wird automatisch erstellt, und ein Build wird in [Azure Pipelines](https://docs.microsoft.com/azure/devops/pipelines/index?view=azure-devops) in die Warteschlange eingereiht.

    Azure Pipelines ist ein Clouddienst, mit dem Sie Ihr Codeprojekt automatisch erstellen und testen sowie es anderen Benutzern zur Verfügung stellen können. Azure Pipelines kombiniert Continuous Integration (CI) und Continuous Delivery (CD), um den Code kontinuierlich und konsistent zu testen und zu erstellen und auf einem beliebigen Ziel bereitzustellen.

    ![Containerregistrierung](media/deployment-center-launcher/container-registry.png)

1. Klicken Sie auf den Link, um die aktuelle Pipeline anzuzeigen.

1. Nach der Bereitstellung werden die Erfolgsprotokolle wie nachfolgend dargestellt angezeigt:

    ![Protokolle](media/deployment-center-launcher/logs.png)

## <a name="examine-the-ci-pipeline"></a>Überprüfen der CI-Pipeline

Mit dem Bereitstellungscenter wird automatisch die CI/CD-Pipeline in Ihrer Azure DevOps-Organisation konfiguriert. Sie können die Pipeline untersuchen und anpassen. 

1. Navigieren Sie zum Dashboard des Bereitstellungscenters.  

1. Klicken Sie in der Liste der Erfolgsprotokolle auf die Buildnummer, um die Buildpipeline für Ihr Projekt anzuzeigen. 

1. Klicken Sie in der oberen rechten Ecke auf die Auslassungspunkte (...). In einem Menü werden verschiedene Optionen angezeigt, etwa zum Einreihen eines neuen Builds in die Warteschlange, zum Beibehalten eines Builds und zum Bearbeiten der Buildpipeline. Wählen Sie **Pipeline bearbeiten** aus. 

1. In diesem Bereich können Sie sich die verschiedenen Aufgaben für Ihre Buildpipeline ansehen. Der Build führt verschiedene Aufgaben aus. Beispielsweise erfasst er Quellen aus dem Git-Repository, erstellt ein Image, überträgt ein Image per Push an die Containerregistrierung und veröffentlicht Ausgaben, die für Bereitstellungen verwendet werden.

1. Wählen Sie oben in der Buildpipeline den Namen der Buildpipeline aus.

1. Ersetzen Sie den Namen Ihrer Buildpipeline durch einen aussagekräftigeren Namen, und wählen Sie **Speichern und in Warteschlange einreihen** und dann **Speichern** aus.

1. Wählen Sie unter der Buildpipeline **Verlauf** aus. In diesem Bereich wird ein Überwachungsprotokoll der aktuellen Buildänderungen angezeigt. An der Buildpipeline vorgenommene Änderungen werden von Azure DevOps überwacht, sodass Sie verschiedene Versionen vergleichen können.

1. Wählen Sie **Trigger** aus. Optional können Branches in den CI-Prozess eingeschlossen oder davon ausgeschlossen werden.

1. Wählen Sie **Aufbewahrung** aus. Abhängig vom Szenario können Sie Richtlinien zum Aufbewahren oder Entfernen einer bestimmten Anzahl von Builds festlegen.

## <a name="examine-the-cd-pipeline"></a>Überprüfen der CD-Pipeline

Mit dem Bereitstellungscenter werden die erforderlichen Schritte zum Bereitstellen über Ihre Azure DevOps-Organisation in Ihrem Azure-Abonnement automatisch erstellt und konfiguriert. Diese Schritte umfassen die Einrichtung einer Azure-Dienstverbindung zur Authentifizierung Ihres Azure-Abonnements mit Azure DevOps. Die Automatisierung erstellt darüber hinaus eine Releasepipeline, mit der CD in Azure bereitgestellt wird.

1. Wählen Sie **Pipelines** und anschließend **Releases** aus.

1. Klicken Sie zum Bearbeiten der Releasepipeline auf **Bearbeiten**.

1. Wählen Sie unter **Artefakte** die Option **Ablegen** aus. Die in den vorherigen Schritten untersuchte Pipeline erzeugt die für das Artefakt verwendete Ausgabe. 

1. Wählen Sie rechts neben dem Symbol **Ablegen** die Option **Continuous Deployment-Trigger** aus. Diese Releasepipeline enthält einen aktivierten CD-Trigger. Jedes Mal, wenn ein neues Buildartefakt verfügbar ist, wird von diesem CD-Trigger eine Bereitstellung ausgeführt. Optional können Sie den Trigger deaktivieren, sodass Ihre Bereitstellungen manuell ausgeführt werden müssen.

1. Klicken Sie auf **Aufgaben**, um alle Aufgaben für Ihre Pipeline anzuzeigen. Das Release legt die Tiller-Umgebung fest, konfiguriert imagePullSecrets, installiert Helm-Tools und stellt die Helm-Diagramme im K8s-Cluster bereit.

1. Klicken Sie zum Anzeigen des Versionsverlaufs auf **Releases anzeigen**. 

1. Klicken Sie zum Anzeigen der Zusammenfassung auf das entsprechende **Release**. Klicken Sie auf eine beliebige Phase, um mehrere Menüs anzuzeigen, etwa eine Releasezusammenfassung, zugeordnete Arbeitselemente und Tests. 

1. Wählen Sie **Commits** aus. In dieser Ansicht werden Codecommits für diese Bereitstellung angezeigt. Vergleichen Sie Releases, um die Commitunterschiede zwischen den einzelnen Bereitstellungen anzuzeigen.

1. Wählen Sie **Protokolle** aus. Die Protokolle enthalten nützliche Informationen zur Bereitstellung. Sie können während und nach Bereitstellungen angezeigt werden.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Sie können die erstellten zugehörigen Ressourcen löschen, wenn Sie sie nicht mehr benötigen. Verwenden Sie dazu die Funktion „Löschen“ auf dem DevOps Projects-Dashboard.

## <a name="next-steps"></a>Nächste Schritte

Diese Build- und Releasepipelines können Sie optional an die Anforderungen Ihres Teams anpassen. Sie können dieses CI/CD-Muster auch als Vorlage für Ihre anderen Pipelines verwenden. In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Konfigurieren einer DevOps-Pipeline zum Bereitstellen Ihrer Anwendungsupdates im K8s-Cluster
> * Überprüfen der CI-Pipeline
> * Überprüfen der CD-Pipeline
> * Bereinigen der Ressourcen
