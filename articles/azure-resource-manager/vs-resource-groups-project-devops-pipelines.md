---
title: CI/CD mit Azure-Pipelines und Resource Manager-Vorlagen
description: Beschreibt die Einrichtung von Continuous Integration in Azure Pipelines mithilfe von Bereitstellungsprojekten für Azure-Ressourcengruppen in Visual Studio für die Bereitstellung von Resource Manager-Vorlagen.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 06/12/2019
ms.author: tomfitz
ms.openlocfilehash: ae896fa0820fbd25ed3f2d29c89fbcd56e7fd6f5
ms.sourcegitcommit: 6d2a147a7e729f05d65ea4735b880c005f62530f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/22/2019
ms.locfileid: "69982459"
---
# <a name="integrate-resource-manager-templates-with-azure-pipelines"></a>Integrieren von Resource Manager-Vorlagen mit Azure Pipelines

Visual Studio umfasst das Azure-Ressourcengruppenprojekt zum Erstellen von Vorlagen und deren Bereitstellung in Ihrem Azure-Abonnement. Sie können dieses Projekt mit Azure Pipelines für Continuous Integration und Continuous Deployment (CI/CD) integrieren.

Zum Bereitstellen von Vorlagen mit Azure Pipelines haben Sie zwei Möglichkeiten:

* **Hinzufügen einer Ausgabe, die ein Azure PowerShell-Skript ausführt**. Diese Option bietet einen konsistenten Entwicklungslebenszyklus, da Sie das gleiche Skript verwenden, das im Visual Studio-Projekt enthalten ist (Deploy-AzureResourceGroup.ps1). Das Skript stellt Artefakte aus Ihrem Projekt einem Speicherkonto zur Verfügung, auf das der Resource Manager zugreifen kann. Artefakte sind Elemente in Ihrem Projekt, wie z.B. verknüpfte Vorlagen, Skripte und Binärdateien von Anwendungen. Anschließend wird das Skript über die Vorlage bereitgestellt.

* **Hinzufügen von Aufgaben zum Kopieren und Bereitstellen von Tasks**. Diese Option bietet eine praktische Alternative zum Projektskript. Sie konfigurieren zwei Aufgaben in der Pipeline. Eine Aufgabe stellt die Artefakte zur Verfügung und die andere stellt die Vorlage bereit.

In diesem Artikel werden beide Ansätze beschrieben.

## <a name="prepare-your-project"></a>Vorbereiten Ihres Projekts

In diesem Artikel wird vorausgesetzt, dass Ihr Visual Studio-Projekt und die Azure DevOps-Organisation für die Erstellung der Pipeline bereit sind. Die folgenden Schritte zeigen, wie Sie sicherstellen können, dass Sie bereit sind:

* Sie haben eine Azure DevOps-Organisation. Sollten Sie über keine Organisation verfügen, [können Sie kostenlos eine erstellen](/azure/devops/pipelines/get-started/pipelines-sign-up?view=azure-devops). Wenn Ihr Team bereits über eine Azure DevOps-Organisation verfügt, stellen Sie sicher, dass Sie Administrator des Azure DevOps-Projekts sind, das Sie verwenden möchten.

* Sie haben eine [Dienstverbindung](/azure/devops/pipelines/library/connect-to-azure?view=azure-devops) zu Ihrem Azure-Abonnement konfiguriert. Die Aufgaben in der Pipeline werden unter der Identität des Dienstprinzipals ausgeführt. Die Schritte zum Erstellen der Verbindung finden Sie unter [Erstellen eines DevOps-Projekts](resource-manager-tutorial-use-azure-pipelines.md#create-a-devops-project).

* Sie haben ein Visual Studio-Projekt, das aus der Startvorlage **Azure-Ressourcengruppe** erstellt wurde. Informationen zum Erstellen dieser Art von Projekt finden Sie unter [Erstellen und Bereitstellen von Azure-Ressourcengruppen über Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).

* Ihr Visual Studio-Projekt ist [mit einem Azure DevOps-Projekt verbunden](/azure/devops/repos/git/share-your-code-in-git-vs-2017?view=azure-devops).

## <a name="create-pipeline"></a>Erstellen der Pipeline

1. Wenn Sie noch keine Pipeline hinzugefügt haben, müssen Sie eine neue Pipeline erstellen. Wählen Sie aus Ihrer Azure DevOps-Organisation **Pipelines** und **Neue Pipeline** aus.

   ![Hinzufügen einer neuen Pipeline](./media/vs-resource-groups-project-devops-pipelines/new-pipeline.png)

1. Geben Sie an, wo Ihr Code gespeichert ist. Die folgende Abbildung zeigt die Auswahl von **Azure Repos-Git**.

   ![Auswählen der Codequelle](./media/vs-resource-groups-project-devops-pipelines/select-source.png)

1. Wählen Sie aus dieser Quelle das Repository aus, das den Code für Ihr Projekt enthält.

   ![Repository auswählen](./media/vs-resource-groups-project-devops-pipelines/select-repo.png)

1. Wählen Sie den zu erstellenden Pipelinetyp aus. Sie können **Starterpipeline** auswählen.

   ![Pipeline auswählen](./media/vs-resource-groups-project-devops-pipelines/select-pipeline.png)

Sie können jetzt entweder die Azure PowerShell-Aufgabe oder die Aufgaben zum Kopieren und Bereitstellen der Datei hinzufügen.

## <a name="azure-powershell-task"></a>Azure PowerShell-Aufgabe

In diesem Abschnitt wird gezeigt, wie Sie die Continuous Deployment mithilfe einer einzelnen Aufgabe konfigurieren, die das PowerShell-Skript in Ihrem Projekt ausführt. Die folgende YAML-Datei erstellt eine [Azure PowerShell-Aufgabe](/azure/devops/pipelines/tasks/deploy/azure-powershell?view=azure-devops):

```yaml
pool:
  name: Hosted Windows 2019 with VS2019
  demands: azureps

steps:
- task: AzurePowerShell@3
  inputs:
    azureSubscription: 'demo-deploy-sp'
    ScriptPath: 'AzureResourceGroupDemo/Deploy-AzureResourceGroup.ps1'
    ScriptArguments: -ResourceGroupName 'demogroup' -ResourceGroupLocation 'centralus' 
    azurePowerShellVersion: LatestVersion
```

Wenn Sie die Aufgabe auf `AzurePowerShell@3` setzen, verwendet die Pipeline Befehle des AzureRM-Moduls, um die Verbindung zu authentifizieren. Standardmäßig verwendet das PowerShell-Skript im Visual Studio-Projekt das AzureRM-Modul. Wenn Sie Ihr Skript aktualisiert haben, um das [Az-Modul](/powershell/azure/new-azureps-module-az) zu verwenden, setzen Sie die Aufgabe auf `AzurePowerShell@4`.

```yaml
steps:
- task: AzurePowerShell@4
```

Geben Sie für `azureSubscription` den Namen der von Ihnen erstellten Dienstverbindung an.

```yaml
inputs:
    azureSubscription: '<your-connection-name>'
```

Geben Sie für `scriptPath` den relativen Pfad von der Pipelinedatei zu Ihrem Skript an. Den Pfade finden Sie in Ihrem Repository.

```yaml
ScriptPath: '<your-relative-path>/<script-file-name>.ps1'
```

Wenn Sie keine Artefakte bereitstellen müssen, übergeben Sie einfach den Namen und den Speicherort einer Ressourcengruppe, die für die Bereitstellung verwendet werden soll. Das Visual Studio-Skript erstellt die Ressourcengruppe, wenn diese noch nicht vorhanden ist.

```yaml
ScriptArguments: -ResourceGroupName '<resource-group-name>' -ResourceGroupLocation '<location>'
```

Wenn Sie Artefakte für ein bestehendes Speicherkonto bereitstellen müssen, verwenden Sie:

```yaml
ScriptArguments: -ResourceGroupName '<resource-group-name>' -ResourceGroupLocation '<location>' -UploadArtifacts -ArtifactStagingDirectory '$(Build.StagingDirectory)' -StorageAccountName '<your-storage-account>'
```

Sie wissen jetzt, wie Sie die Aufgabe erstellen. Schauen wir uns jetzt die Schritte zum Bearbeiten der Pipeline an.

1. Öffnen Sie Ihre Pipeline, und ersetzen Sie den Inhalt durch Ihre YAML:

   ```yaml
   pool:
     name: Hosted Windows 2019 with VS2019
     demands: azureps

   steps:
   - task: AzurePowerShell@3
     inputs:
       azureSubscription: 'demo-deploy-sp'
       ScriptPath: 'AzureResourceGroupDemo/Deploy-AzureResourceGroup.ps1'
       ScriptArguments: -ResourceGroupName 'demogroup' -ResourceGroupLocation 'centralus' -UploadArtifacts -ArtifactStagingDirectory '$(Build.StagingDirectory)' -StorageAccountName 'stage3a4176e058d34bb88cc'
       azurePowerShellVersion: LatestVersion
   ```

1. Wählen Sie **Speichern** aus.

   ![Speichern der Pipeline](./media/vs-resource-groups-project-devops-pipelines/save-pipeline.png)

1. Geben Sie eine Meldung für den Commit ein, und führen Sie den Commit direkt zum **Master** aus.

1. Bei der Auswahl von **Speichern** wird die Buildpipeline automatisch ausgeführt. Gehen Sie zurück zur Zusammenfassung Ihrer Buildpipeline und schauen Sie sich den Status an.

   ![Anzeigen der Ergebnisse](./media/vs-resource-groups-project-devops-pipelines/view-results.png)

Sie können die aktuell laufende Pipeline auswählen, um Details zu den Aufgaben anzuzeigen. Nach Abschluss werden die Ergebnisse für jeden Schritt angezeigt.

## <a name="copy-and-deploy-tasks"></a>Aufgaben zum Kopieren und Bereitstellen

In diesem Abschnitt wird gezeigt, wie Sie Continuous Deployment konfigurieren, indem Sie zwei Aufgaben zur Bereitstellung der Artefakte und der Vorlage verwenden. 

Die folgende YAML zeigt die [Aufgabe für den Azure-Dateikopiervorgang](/azure/devops/pipelines/tasks/deploy/azure-file-copy?view=azure-devops):

```yaml
- task: AzureFileCopy@3
  displayName: 'Stage files'
  inputs:
    SourcePath: 'AzureResourceGroup1'
    azureSubscription: 'demo-deploy-sp'
    Destination: 'AzureBlob'
    storage: 'stage3a4176e058d34bb88cc'
    ContainerName: 'democontainer'
    outputStorageUri: 'artifactsLocation'
    outputStorageContainerSasToken: 'artifactsLocationSasToken'
    sasTokenTimeOutInMinutes: '240'
```

Diese Aufgabe umfasst mehrere Teile, die Sie für Ihre Umgebung überarbeiten müssen. `SourcePath` gibt den Speicherort der Artefakte relativ zur Pipelinedatei an. In diesem Beispiel befinden sich die Dateien in einem Ordner namens `AzureResourceGroup1`. Dies war der Name des Projekts.

```yaml
SourcePath: '<path-to-artifacts>'
```

Geben Sie für `azureSubscription` den Namen der von Ihnen erstellten Dienstverbindung an.

```yaml
azureSubscription: '<your-connection-name>'
```

Geben Sie für Speicher- und Containername die Namen des Speicherkonto und des Containers an, den Sie für die Speicherung der Artefakte verwenden möchten. Das Speicherkonto muss bereits vorhanden sein.

```yaml
storage: '<your-storage-account-name>'
ContainerName: '<container-name>'
```

Die folgende YAML zeigt die [Bereitstellungsaufgabe für die Azure-Ressourcengruppe](/azure/devops/pipelines/tasks/deploy/azure-resource-group-deployment?view=azure-devops):

```yaml
- task: AzureResourceGroupDeployment@2
  displayName: 'Deploy template'
  inputs:
    azureSubscription: 'demo-deploy-sp'
    resourceGroupName: 'demogroup'
    location: 'centralus'
    templateLocation: 'URL of the file'
    csmFileLink: '$(artifactsLocation)WebSite.json$(artifactsLocationSasToken)'
    csmParametersFileLink: '$(artifactsLocation)WebSite.parameters.json$(artifactsLocationSasToken)'
    overrideParameters: '-_artifactsLocation $(artifactsLocation) -_artifactsLocationSasToken "$(artifactsLocationSasToken)"'
```

Diese Aufgabe umfasst mehrere Teile, die Sie für Ihre Umgebung überarbeiten müssen. Geben Sie für `azureSubscription` den Namen der von Ihnen erstellten Dienstverbindung an.

```yaml
azureSubscription: '<your-connection-name>'
```

Geben Sie für `resourceGroupName` und `location` den Namen und den Speicherort der Ressourcengruppe an, für die die Bereitstellung ausgeführt werden soll. Diese Aufgabe erstellt die Ressourcengruppe, wenn diese noch nicht vorhanden ist.

```yaml
resourceGroupName: '<resource-group-name>'
location: '<location>'
```

Die Bereitstellungsaufgabe verknüpft sich mit einer Vorlage namens `WebSite.json` und einer Parameterdatei namens „WebSite.parameters.json“. Verwenden Sie die Namen Ihre Vorlage und Ihrer Parameterdateien.

Sie wissen jetzt, wie Sie Aufgaben erstellen. Schauen wir uns jetzt die Schritte zum Bearbeiten der Pipeline an.

1. Öffnen Sie Ihre Pipeline, und ersetzen Sie den Inhalt durch Ihre YAML:

   ```yaml
   pool:
     name: Hosted Windows 2019 with VS2019

   steps:
   - task: AzureFileCopy@3
     displayName: 'Stage files'
     inputs:
       SourcePath: 'AzureResourceGroup1'
       azureSubscription: 'demo-deploy-sp'
       Destination: 'AzureBlob'
       storage: 'stage3a4176e058d34bb88cc'
       ContainerName: 'democontainer'
       outputStorageUri: 'artifactsLocation'
       outputStorageContainerSasToken: 'artifactsLocationSasToken'
       sasTokenTimeOutInMinutes: '240'
   - task: AzureResourceGroupDeployment@2
     displayName: 'Deploy template'
     inputs:
       azureSubscription: 'demo-deploy-sp'
       resourceGroupName: demogroup
       location: 'centralus'
       templateLocation: 'URL of the file'
       csmFileLink: '$(artifactsLocation)WebSite.json$(artifactsLocationSasToken)'
       csmParametersFileLink: '$(artifactsLocation)WebSite.parameters.json$(artifactsLocationSasToken)'
       overrideParameters: '-_artifactsLocation $(artifactsLocation) -_artifactsLocationSasToken "$(artifactsLocationSasToken)"'
   ```

1. Wählen Sie **Speichern** aus.

1. Geben Sie eine Meldung für den Commit ein, und führen Sie den Commit direkt zum **Master** aus.

1. Bei der Auswahl von **Speichern** wird die Buildpipeline automatisch ausgeführt. Gehen Sie zurück zur Zusammenfassung Ihrer Buildpipeline und schauen Sie sich den Status an.

   ![Anzeigen der Ergebnisse](./media/vs-resource-groups-project-devops-pipelines/view-results.png)

Sie können die aktuell laufende Pipeline auswählen, um Details zu den Aufgaben anzuzeigen. Nach Abschluss werden die Ergebnisse für jeden Schritt angezeigt.

## <a name="next-steps"></a>Nächste Schritte

Eine Schritt-für-Schritt-Anleitung zur Verwendung von Azure Pipelines mit Resource Manager-Vorlagen finden Sie im [Tutorial: Continuous Integration von Azure Resource Manager-Vorlagen mit Azure Pipelines](resource-manager-tutorial-use-azure-pipelines.md).
