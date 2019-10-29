---
title: Continuous Integration mit Azure Pipelines | Microsoft-Dokumentation
description: Hier finden Sie Informationen zum kontinuierlichen Erstellen, Testen und Bereitstellen von Azure Resource Manager-Vorlagen.
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: carmonm
editor: ''
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 10/15/2019
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: b176e97a546335f597d4cf424d7feb4f5fa0f775
ms.sourcegitcommit: b4f201a633775fee96c7e13e176946f6e0e5dd85
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/18/2019
ms.locfileid: "72597219"
---
# <a name="tutorial-continuous-integration-of-azure-resource-manager-templates-with-azure-pipelines"></a>Tutorial: Continuous Integration von Azure Resource Manager-Vorlagen mit Azure Pipelines

Hier finden Sie Informationen zum kontinuierlichen Erstellen und Bereitstellen von Azure Resource Manager-Vorlagenprojekten mit Azure Pipelines.

Azure DevOps stellt Entwicklerdienste bereit, die Teams bei der Arbeitsplanung, bei der gemeinsamen Codeentwicklung sowie bei der Erstellung und Bereitstellung von Anwendungen unterstützen. Mit Azure DevOps Services können Entwickler in der Cloud arbeiten. Azure DevOps bietet integrierte Features, auf die Sie über Ihren Webbrowser oder über Ihren IDE-Client zugreifen können. Eines dieser Features ist Azure Pipelines. Azure Pipelines ist ein umfassender Dienst für Continuous Integration (CI) und Continuous Delivery (CD). Er kann mit Ihrem bevorzugten Git-Anbieter sowie für Bereitstellungen in den meisten gängigen Clouddiensten verwendet werden. Anschließend können Sie das Erstellen, Testen und Bereitstellen Ihres Codes für Microsoft Azure, Google Cloud Platform oder Amazon Web Services automatisieren.

Dieses Tutorial richtet sich an Entwickler von Azure Resource Manager-Vorlagen, die noch keine Erfahrung mit Azure DevOps Services und Azure Pipelines haben. Sollten Sie bereits mit GitHub und DevOps vertraut sein, können Sie direkt mit [Erstellen einer Pipeline](#create-a-pipeline) fortfahren.

> [!NOTE]
> Überlegen Sie sich einen Projektnamen. Ersetzen Sie in diesem Tutorial jedes Vorkommen von **AzureRmPipeline** durch Ihren Projektnamen.

Dieses Tutorial enthält die folgenden Aufgaben:

> [!div class="checklist"]
> * ein GitHub-Repository erstellen
> * Erstellen eines Azure DevOps-Projekts
> * Erstellen einer Azure-Pipeline
> * Überprüfen der Pipelinebereitstellung
> * Aktualisieren der Vorlage und erneutes Bereitstellen
> * Bereinigen von Ressourcen

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Damit Sie die Anweisungen in diesem Artikel ausführen können, benötigen Sie Folgendes:

* **Ein GitHub-Konto** zum Erstellen eines Repositorys für Ihre Vorlagen. Sollten Sie über kein Konto verfügen, können Sie [ein kostenloses Konto erstellen](https://github.com). Weitere Informationen zur Verwendung von GitHub-Repositorys finden Sie unter [Build GitHub repositories](/azure/devops/pipelines/repos/github) (Erstellen von GitHub-Repositorys).
* **Git installieren**. In diesem Tutorial wird *Git Bash* oder *Git Shell* verwendet. Eine entsprechende Anleitung finden Sie unter [Install Git]( https://www.atlassian.com/git/tutorials/install-git) (Installieren von Git).
* **Eine Azure DevOps-Organisation.** Sollten Sie über keine Organisation verfügen, können Sie kostenlos eine Organisation erstellen. Weitere Informationen finden Sie unter [Quickstart: Create an organization or project collection]( https://docs.microsoft.com/azure/devops/organizations/accounts/create-organization?view=azure-devops) (Schnellstart: Erstellen einer Organisation oder einer Projektsammlung).
* **[Visual Studio Code](https://code.visualstudio.com/) mit der Erweiterung „Azure Resource Manager-Tools“.** Informationen finden Sie unter [Schnellstart: Erstellen von Azure Resource Manager-Vorlagen mit Visual Studio Code](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites).

## <a name="prepare-a-github-repository"></a>ein GitHub-Repository erstellen

GitHub wird zum Speichern Ihres Projektquellcodes (einschließlich Resource Manager-Vorlagen) verwendet. Informationen zu weiteren unterstützten Repositorys finden Sie unter [Supported repository types](/azure/devops/pipelines/repos/?view=azure-devops#supported-repository-types) (Unterstützte Repositorytypen).

### <a name="create-a-github-repository"></a>Erstellen eines GitHub-Repositorys

Sollten Sie kein GitHub-Konto besitzen, sehen Sie sich die [Voraussetzungen](#prerequisites) an.

1. Melden Sie sich bei [GitHub](https://github.com) an.
2. Wählen Sie rechts oben Ihr Kontobild und anschließend **Your Repositorys** (Ihre Repositorys) aus.

    ![Azure Resource Manager/Azure DevOps/Azure Pipelines: Erstellen eines GitHub-Repositorys](./media/resource-manager-tutorial-use-azure-pipelines/azure-resource-manager-devops-pipelines-github-repository.png)

1. Wählen Sie die grüne Schaltfläche **New** (Neu) aus.
1. Geben Sie unter **Repository Name** (Repositoryname) einen Repositorynamen ein.  Beispiel: **AzureRmPipeline-repo**. Denken Sie daran, alle Vorkommen von **AzureRmPipeline** durch Ihren Projektnamen zu ersetzen. In diesem Tutorial können Sie entweder **Public** (Öffentlich) oder **Private** (Privat) auswählen. Wählen Sie anschließend **Create Repository** (Repository erstellen) aus.
1. Notieren Sie sich die URL. Die Repository-URL hat das folgende Format:

    ```url
    https://github.com/[YourAccountName]/[YourRepositoryName]
    ```

Dieses Repository wird als *Remoterepository* bezeichnet. Jeder Entwickler des gleichen Projekts kann sein eigenes *lokales Repository* klonen und die Änderungen mit dem Remoterepository zusammenführen.

### <a name="clone-the-remote-repository"></a>Klonen des Remoterepositorys

1. Öffnen Sie Git Shell oder Git Bash.  Siehe [Voraussetzungen](#prerequisites).
1. Vergewissern Sie sich, dass Sie sich im Ordner **github** befinden.
1. Führen Sie den folgenden Befehl aus:

    ```bash
    git clone https://github.com/[YourAccountName]/[YourGitHubRepositoryName]
    cd [YourGitHubRepositoryName]
    mkdir CreateAzureStorage
    cd CreateAzureStorage
    pwd
    ```

    Ersetzen Sie **[YourAccountName]** durch den Namen Ihres GitHub-Kontos und **[YourGitHubRepositoryName]** durch den Namen des Repositorys, das Sie im vorherigen Schritt erstellt haben.

    Im folgenden Screenshot wird ein Beispiel gezeigt.

    ![Azure Resource Manager/Azure DevOps/Azure Pipelines: GitHub-Erstellung mit Bash](./media/resource-manager-tutorial-use-azure-pipelines/azure-resource-manager-devops-pipelines-github-bash.png)

Der Ordner **CreateAzureStorage** ist der Speicherort für die Vorlage. Der Befehl **pwd** zeigt den Ordnerpfad an. Dieser Pfad wird im nächsten Schritt verwendet, um die Vorlage zu speichern.

### <a name="download-a-quickstart-template"></a>Herunterladen einer Schnellstartvorlage

Anstatt eine Vorlage zu erstellen, können Sie auch eine [Schnellstartvorlage]( https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json) herunterladen. Diese Vorlage erstellt ein Azure Storage-Konto.

1. Öffnen Sie Visual Studio Code. Siehe [Voraussetzungen](#prerequisites).
2. Öffnen Sie die Vorlage mit der folgenden URL:

    ```URL
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
    ```

3. Speichern Sie die Datei **azuredeploy.json** im Ordner **CreateAzureStorage**. Sowohl der Ordnername als auch der Dateiname werden genau so in der Pipeline verwendet.  Wenn Sie diese Namen ändern, müssen auch die in der Pipeline verwendeten Namen aktualisiert werden.

### <a name="push-the-template-to-the-remote-repository"></a>Pushen der Vorlage in das Remoterepository

Die Datei „azuredeploy.json“ wurde dem lokalen Repository hinzugefügt. Als Nächstes muss die Vorlage in das Remoterepository hochgeladen werden.

1. Öffnen Sie *Git Shell* oder *Git Bash*, sofern es nicht bereits geöffnet ist.
1. Wechseln Sie zum Ordner „CreateAzureStorage“ in Ihrem lokalen Repository.
1. Vergewissern Sie sich, dass der Ordner die Datei **azuredeploy.json** enthält.
1. Führen Sie den folgenden Befehl aus:

    ```bash
    git add .
    git commit -m “Add a new create storage account template.”
    git push origin master
    ```

    Daraufhin wird unter Umständen eine LF-Warnung angezeigt. Diese Warnung kann ignoriert werden. **master** ist der Masterbranch.  Üblicherweise wird für jedes Update ein eigener Branch erstellt. Zur Vereinfachung des Tutorials verwenden wir den Masterbranch direkt.
1. Navigieren Sie in einem Browser zu Ihrem GitHub-Repository.  Die URL lautet **https://github.com/ [Name Ihres Kontos]/[Name Ihres GitHub-Repositorys]** . Daraufhin wird der Ordner **CreateAzureStorage** mit der enthaltenen Datei **azuredeploy.json** angezeigt.

Sie haben nun ein GitHub-Repository erstellt und eine Vorlage in das Repository hochgeladen.

## <a name="create-a-devops-project"></a>Erstellen eines DevOps-Projekts

Für den nächsten Schritt wird eine DevOps-Organisation benötigt.  Sollten Sie über keine Organisation verfügen, sehen Sie sich die [Voraussetzungen](#prerequisites) an.

1. Melden Sie sich bei [Azure DevOps](https://dev.azure.com) an.
1. Wählen Sie auf der linken Seite eine DevOps-Organisation aus.

    ![Azure Resource Manager/Azure DevOps/Azure Pipelines: Erstellen eines Azure DevOps-Projekts](./media/resource-manager-tutorial-use-azure-pipelines/azure-resource-manager-devops-pipelines-create-devops-project.png)

1. Wählen Sie **Projekt erstellen** aus. Sollten Sie über keine Projekte verfügen, wird automatisch die Projekterstellungsseite geöffnet.
1. Geben Sie die folgenden Werte ein:

    * **Projektname**: Geben Sie einen Projektnamen ein. Sie können den Projektnamen verwenden, den Sie ganz am Anfang des Tutorials überlegt haben.
    * **Versionskontrolle**: Wählen Sie **Git** aus. Sollte **Versionskontrolle** nicht angezeigt werden, wählen Sie **Erweitert** aus, um den Bereich zu erweitern.

    Übernehmen Sie bei den anderen Eigenschaften die Standardwerte.
1. Wählen Sie **Projekt erstellen** aus.

Erstellen Sie eine Dienstverbindung für die Projektbereitstellung in Azure.

1. Wählen Sie im unteren Bereich des linken Menüs die Option **Projekteinstellungen** aus.
1. Wählen Sie unter **Pipelines** die Option **Dienstverbindungen** aus.
1. Wählen Sie **Neue Dienstverbindung** und anschließend **AzureResourceManager** aus.
1. Geben Sie die folgenden Werte ein:

    * **Verbindungsname**: Geben Sie einen Verbindungsnamen ein. Beispiel: **AzureRmPipeline-conn**. Notieren Sie sich diesen Namen. Er wird bei der Pipelineerstellung benötigt.
    * **Bereichsebene**: Wählen Sie **Abonnement** aus.
    * **Abonnement**: Wählen Sie Ihr Abonnement aus.
    * **Ressourcengruppe**: Lassen Sie dieses Feld leer.
    * **Hiermit wird allen Pipelines die Verwendung dieser Dienstverbindung gestattet.** (aktiviert)
1. Klicken Sie auf **OK**.

## <a name="create-a-pipeline"></a>Erstellen einer Pipeline

In den vorangegangenen Abschnitten wurden die folgenden Aufgaben ausgeführt.  Sollten Sie diese Abschnitte übersprungen haben, weil Sie bereits mit GitHub und DevOps vertraut sind, müssen Sie diese Aufgaben nachholen, bevor Sie fortfahren.

- Erstellen eines GitHub-Repositorys und Speichern [dieser Vorlage](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json) im Ordner **CreateAzureStorage** des Repositorys
- Erstellen eines DevOps-Projekts und einer Azure Resource Manager-Dienstverbindung

So erstellen Sie eine Pipeline mit einem Vorlagenbereitstellungsschritt:

1. Wählen Sie im linken Menü die Option **Pipelines** aus.
1. Wählen Sie **Neue Pipeline** aus.
1. Wählen Sie auf der Registerkarte **Verbinden** die Option **GitHub** aus. Geben Sie Ihre GitHub-Anmeldeinformationen ein, wenn Sie dazu aufgefordert werden, und folgen Sie dann den Anweisungen. Wählen Sie im folgenden Bildschirm die Option **Only select repositories** (Nur ausgewählte Repositorys) aus, und vergewissern Sie sich, dass Ihr Repository in der Liste enthalten ist, bevor Sie **Approve & Install** (Genehmigen und installieren) auswählen.

    ![Azure Resource Manager/Azure DevOps/Azure Pipelines: „Only select repositories“ (Nur ausgewählte Repositorys)](./media/resource-manager-tutorial-use-azure-pipelines/azure-resource-manager-devops-pipelines-only-select-repositories.png)

1. Wählen Sie auf der Registerkarte **Auswählen** Ihr Repository aus.  Der Standardname lautet **[Name Ihres Kontos]/[Name Ihres GitHub-Repositorys]** .
1. Wählen Sie auf der Registerkarte **Konfigurieren** die Option **Starterpipeline** aus. Daraufhin wird die Pipelinedatei **azure-pipelines.yml** mit zwei Skriptschritten angezeigt.
1. Ersetzen Sie den Abschnitt **steps** durch den folgenden YAML-Code:

    ```yaml
    steps:
    - task: AzureResourceManagerTemplateDeployment@3
      inputs:
        deploymentScope: 'Resource Group'
        ConnectedServiceName: '[EnterYourServiceConnectionName]'
        subscriptionName: '[EnterTheTargetSubscriptionID]'
        action: 'Create Or Update Resource Group'
        resourceGroupName: '[EnterANewResourceGroupName]'
        location: 'Central US'
        templateLocation: 'Linked artifact'
        csmFile: 'CreateAzureStorage/azuredeploy.json'
        deploymentMode: 'Incremental'
    ```

    Das Ergebnis sollte wie folgt aussehen:

    ![Azure Resource Manager/Azure DevOps/Azure Pipelines: YAML](./media/resource-manager-tutorial-use-azure-pipelines/azure-resource-manager-devops-pipelines-yml.png)

    Nehmen Sie die folgenden Änderungen vor:

    * **deloymentScope**: Wählen Sie den Umfang der Bereitstellung aus den Optionen `Management Group`, `Subscription` und `Resource Group` aus. Verwenden Sie in diesem Tutorial **Resource Group**. Weitere Informationen zum Umfang finden Sie unter [Bereitstellungsumfang](./resource-group-template-deploy-rest.md#deployment-scope).
    * **ConnectedServiceName**: Geben Sie den Namen der Dienstverbindung an, den Sie zuvor erstellt haben.
    * **SubscriptionName**:  Geben Sie die Zielabonnement-ID an.
    * **action**: Die Aktion **Create Or Update Resource Group** führt zwei Aktionen aus: 1. Sie erstellt eine Ressourcengruppe, falls ein neuer Ressourcengruppenname angegeben wurde. 2. Sie stellt die angegebene Vorlage bereit.
    * **resourceGroupName**: Geben Sie einen neuen Ressourcengruppennamen ein. Beispiel: **AzureRmPipeline-rg**.
    * **location**: Geben Sie den Speicherort für die Ressourcengruppe an.
    * **templateLocation**: Bei Angabe von **Linked artifact** wird direkt im verbundenen Repository nach der Vorlagendatei gesucht.
    * **csmFile** ist der vollqualifizierte Pfad der Vorlagendatei. Sie müssen keine Vorlagenparameterdatei angeben, da alle in der Vorlage definierten Parameter Standardwerte besitzen.

    Weitere Informationen zu dieser Aufgabe finden Sie unter [Aufgabe „Bereitstellung einer Azure-Ressourcengruppe“](/azure/devops/pipelines/tasks/deploy/azure-resource-group-deployment)und [Aufgabe „Azure Resource Manager-Vorlagenbereitstellung“](https://github.com/microsoft/azure-pipelines-tasks/blob/master/Tasks/AzureResourceManagerTemplateDeploymentV3/README.md).
1. Klicken Sie auf **Speichern und ausführen**.
1. Wählen Sie erneut **Speichern und ausführen** aus. Eine Kopie der YAML-Datei wird im verbundenen Repository gespeichert. Sie können die YAML-Datei anzeigen, indem Sie zu Ihrem Repository navigieren.
1. Vergewissern Sie sich, dass die Pipeline erfolgreich ausgeführt wird.

    ![Azure Resource Manager/Azure DevOps/Azure Pipelines: YAML](./media/resource-manager-tutorial-use-azure-pipelines/azure-resource-manager-devops-pipelines-status.png)

## <a name="verify-the-deployment"></a>Überprüfen der Bereitstellung

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Öffnen Sie die Ressourcengruppe. Der Name entspricht der Angabe aus der YAML-Pipelinedatei.  Es sollte ein einzelnes Speicherkonto erstellt werden.  Der Name des Speicherkontos beginnt mit **store**.
1. Wählen Sie den Namen des Speicherkontos aus, um es zu öffnen.
1. Wählen Sie **Eigenschaften** aus. Die **SKU** lautet **Standard_LRS**.

    ![Azure Resource Manager/Azure DevOps/Azure Pipelines: Überprüfung im Portal](./media/resource-manager-tutorial-use-azure-pipelines/azure-resource-manager-devops-pipelines-portal-verification.png)

## <a name="update-and-redeploy"></a>Aktualisieren und erneutes Bereitstellen

Wenn Sie die Vorlage aktualisieren und die Änderungen in das Remoterepository pushen, werden die Ressourcen (in diesem Fall: das Speicherkonto) automatisch durch die Pipeline aktualisiert.

1. Öffnen Sie **azuredeploy.json** über Ihr lokales Repository in Visual Studio Code.
1. Aktualisieren Sie den Standardwert (**defaultValue**) von **storageAccountType** auf **Standard_GRS**. Der folgende Screenshot zeigt dies:

    ![Azure Resource Manager/Azure DevOps/Azure Pipelines: YAML-Aktualisierung](./media/resource-manager-tutorial-use-azure-pipelines/azure-resource-manager-devops-pipelines-update-yml.png)

1. Speichern Sie die Änderungen.
1. Führen Sie in Git Bash/Shell die folgenden Befehle aus, um die Änderungen in das Remoterepository zu pushen:

    ```bash
    git pull origin master
    git add .
    git commit -m “Add a new create storage account template.”
    git push origin master
    ```

    Der erste Befehl synchronisiert das lokale Repository mit dem Remoterepository. Zur Erinnerung: Die YAML-Pipelinedatei wurde dem Remoterepository hinzugefügt.

    Nachdem der Masterbranch des Remoterepositorys aktualisiert wurde, wird die Pipeline erneut ausgelöst.

Sehen Sie sich zur Überprüfung der Änderungen die SKU des Speicherkontos an.  Eine entsprechende Anleitung finden Sie unter [Überprüfen der Bereitstellung](#verify-the-deployment).

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Azure-Ressourcen nicht mehr benötigen, löschen Sie die Ressourcengruppe, um die bereitgestellten Ressourcen zu bereinigen.

1. Wählen Sie im Azure-Portal im linken Menü die Option **Ressourcengruppe** aus.
2. Geben Sie den Namen der Ressourcengruppe in das Feld **Nach Name filtern** ein.
3. Klicken Sie auf den Namen der Ressourcengruppe.
4. Wählen Sie **Ressourcengruppe löschen** aus dem Menü ganz oben aus.

Falls gewünscht, können Sie auch das GitHub-Repository und das Azure DevOps-Projekt löschen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie eine Azure DevOps-Pipeline unter Verwendung einer Azure Resource Manager-Vorlage erstellt. Im folgenden Tutorial erfahren Sie, wie Sie Azure-Ressourcen in mehreren Regionen bereitstellen und sichere Bereitstellungsverfahren verwenden:

> [!div class="nextstepaction"]
> [Verwenden von sicheren Bereitstellungsmethoden](./deployment-manager-tutorial.md)
