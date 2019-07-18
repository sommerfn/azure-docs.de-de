---
title: Continuous Delivery von Funktionscodeaktualisierungen mit Azure DevOps
description: Erfahren Sie, wie Sie eine Azure DevOps-Pipeline mit Azure Functions als Ziel einrichten.
author: ahmedelnably
manager: jeconnoc
ms.service: azure-functions
ms.topic: conceptual
ms.date: 04/18/2019
ms.author: aelnably
ms.custom: ''
ms.openlocfilehash: 9806a982982971b1b3ac9c28454e17813b2ad2a5
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/01/2019
ms.locfileid: "67479870"
---
# <a name="continuous-delivery-using-azure-devops"></a>Continuous Delivery mit Azure DevOps

Sie können Ihre Funktion mit [Azure Pipelines](/azure/devops/pipelines/) automatisch in einer Azure-Funktions-App bereitstellen.
Zum Definieren Ihrer Pipeline stehen Ihnen zwei Methoden zur Auswahl:

- YAML-Datei: Diese Datei beschreibt die Pipeline. Sie kann einen Abschnitt mit Buildschritten und einen Releaseabschnitt enthalten. Die YAML-Datei sollte sich im gleichen Repository befinden wie die App.

- Vorlagen: Vorlagen sind vorgefertigte Aufgaben, die Ihre App erstellen oder bereitstellen.

## <a name="yaml-based-pipeline"></a>YAML-basierte Pipeline

### <a name="build-your-app"></a>Erstellen der App

Wie Sie Ihre App in Azure Pipelines erstellen, hängt von der Programmiersprache Ihrer App ab.
Jede Sprache verfügt über bestimmte Buildschritte zum Erstellen eines Bereitstellungsartefakts, mit dem Ihre Funktions-App in Azure bereitgestellt werden kann.

#### <a name="net"></a>.NET

Sie können die YAML-Datei zum Erstellen Ihrer .NET-Anwendung mithilfe des folgenden Beispiels erstellen:

```yaml
pool:
      vmImage: 'VS2017-Win2016'
steps:
- script: |
    dotnet restore
    dotnet build --configuration Release
- task: DotNetCoreCLI@2
  inputs:
    command: publish
    arguments: '--configuration Release --output publish_output'
    projects: '*.csproj'
    publishWebProjects: false
    modifyOutputPath: true
    zipAfterPublish: false
- task: ArchiveFiles@2
  displayName: "Archive files"
  inputs:
    rootFolderOrFile: "$(System.DefaultWorkingDirectory)/publish_output"
    includeRootFolder: false
    archiveFile: "$(System.DefaultWorkingDirectory)/build$(Build.BuildId).zip"
- task: PublishBuildArtifacts@1
  inputs:
    PathtoPublish: '$(System.DefaultWorkingDirectory)/build$(Build.BuildId).zip'
    name: 'drop'
```

#### <a name="javascript"></a>JavaScript

Sie können die YAML-Datei zum Erstellen Ihrer JavaScript-Anwendung mithilfe des folgenden Beispiels erstellen:

```yaml
pool:
      vmImage: ubuntu-16.04 # Use 'VS2017-Win2016' if you have Windows native +Node modules
steps:
- bash: |
    if [ -f extensions.csproj ]
    then
        dotnet build extensions.csproj --output ./bin
    fi
    npm install 
    npm run build --if-present
    npm prune --production
- task: ArchiveFiles@2
  displayName: "Archive files"
  inputs:
    rootFolderOrFile: "$(System.DefaultWorkingDirectory)"
    includeRootFolder: false
    archiveFile: "$(System.DefaultWorkingDirectory)/build$(Build.BuildId).zip"
- task: PublishBuildArtifacts@1
  inputs:
    PathtoPublish: '$(System.DefaultWorkingDirectory)/build$(Build.BuildId).zip'
    name: 'drop'
```

#### <a name="python"></a>Python

Sie können die YAML-Datei zum Erstellen Ihrer Python-Anwendung mithilfe des folgenden Beispiels erstellen (Python wird nur für Azure Functions unter Linux unterstützt):

```yaml
pool:
      vmImage: ubuntu-16.04
steps:
- task: UsePythonVersion@0
  displayName: "Setting python version to 3.6 as required by functions"
  inputs:
    versionSpec: '3.6'
    architecture: 'x64'
- bash: |
    if [ -f extensions.csproj ]
    then
        dotnet build extensions.csproj --output ./bin
    fi
    python3.6 -m venv worker_venv
    source worker_venv/bin/activate
    pip3.6 install setuptools
    pip3.6 install -r requirements.txt
- task: ArchiveFiles@2
  displayName: "Archive files"
  inputs:
    rootFolderOrFile: "$(System.DefaultWorkingDirectory)"
    includeRootFolder: false
    archiveFile: "$(System.DefaultWorkingDirectory)/build$(Build.BuildId).zip"
- task: PublishBuildArtifacts@1
  inputs:
    PathtoPublish: '$(System.DefaultWorkingDirectory)/build$(Build.BuildId).zip'
    name: 'drop'
```
#### <a name="powershell"></a>PowerShell

Sie können das folgende Beispiel verwenden, um Ihre YAML-Datei zum Packen Ihrer PowerShell-App zu erstellen. PowerShell wird nur für Windows Azure Functions unterstützt:

```yaml
pool:
      vmImage: 'VS2017-Win2016'
steps:
- task: ArchiveFiles@2
  displayName: "Archive files"
  inputs:
    rootFolderOrFile: "$(System.DefaultWorkingDirectory)"
    includeRootFolder: false
    archiveFile: "$(System.DefaultWorkingDirectory)/build$(Build.BuildId).zip"
- task: PublishBuildArtifacts@1
  inputs:
    PathtoPublish: '$(System.DefaultWorkingDirectory)/build$(Build.BuildId).zip'
    name: 'drop'
```

### <a name="deploy-your-app"></a>Bereitstellen Ihrer App

Je nach Hostbetriebssystem müssen Sie eines der beiden folgenden YAML-Beispiele in Ihre YAML-Datei einfügen.

#### <a name="windows-function-app"></a>Windows-Funktions-App

Der folgende Codeausschnitt kann verwendet werden, um eine Windows-Funktions-App bereitzustellen.

```yaml
steps:
- task: AzureFunctionApp@1
  inputs:
    azureSubscription: '<Azure service connection>'
    appType: functionApp
    appName: '<Name of function app>'
    #Uncomment the next lines to deploy to a deployment slot
    #deployToSlotOrASE: true
    #resourceGroupName: '<Resource Group Name>'
    #slotName: '<Slot name>'
```

#### <a name="linux-function-app"></a>Linux-Funktions-App

Der folgende Codeausschnitt kann verwendet werden, um eine Linux-Funktions-App bereitzustellen.

```yaml
steps:
- task: AzureFunctionApp@1
  inputs:
    azureSubscription: '<Azure service connection>'
    appType: functionAppLinux
    appName: '<Name of function app>'
    #Uncomment the next lines to deploy to a deployment slot
    #Note that deployment slots is not supported for Linux Dynamic SKU
    #deployToSlotOrASE: true
    #resourceGroupName: '<Resource Group Name>'
    #slotName: '<Slot name>'
```

## <a name="template-based-pipeline"></a>Vorlagenbasierte Pipeline

Vorlagen in Azure DevOps sind vordefinierte Gruppen von Aufgaben, die eine App erstellen oder bereitstellen.

### <a name="build-your-app"></a>Erstellen der App

Wie Sie Ihre App in Azure Pipelines erstellen, hängt von der Programmiersprache Ihrer App ab. Jede Sprache verfügt über bestimmte Buildschritte zum Erstellen eines Bereitstellungsartefakts, mit dem Ihre Funktions-App in Azure aktualisiert werden kann.
Wenn Sie die integrierten Buildvorlagen verwenden möchten, wählen Sie beim Erstellen einer neuen Buildpipeline die Option **Klassischen Editor verwenden** aus, um eine Pipeline mithilfe der Designervorlagen zu erstellen.

![Klassischer Azure Pipelines-Editor](media/functions-how-to-azure-devops/classic-editor.png)

Nachdem Sie die Quelle des Codes konfiguriert haben, suchen Sie nach Azure Functions-Buildvorlagen, und wählen Sie die Vorlage für die Sprache Ihrer App aus.

![Azure Functions-Buildvorlagen](media/functions-how-to-azure-devops/build-templates.png)

In einigen Fällen weisen die Buildartefakte eine bestimmte Ordnerstruktur auf, und Sie müssen möglicherweise die Option **Stammordnernamen für Archivpfade voranstellen** aktivieren.

![Stammordner voranstellen](media/functions-how-to-azure-devops/prepend-root-folder.png)

#### <a name="javascript-apps"></a>JavaScript-Apps

Wenn Ihre JavaScript-App von nativen Windows-Modulen abhängig ist, müssen Sie Folgendes aktualisieren:

- Die Agentpool-Version in **Hosted VS2017**

  ![Ändern des Build-Agent-Betriebssystems](media/functions-how-to-azure-devops/change-agent.png)

### <a name="deploy-your-app"></a>Bereitstellen Ihrer App

Wenn Sie eine neue Releasepipeline erstellen, suchen Sie nach der Azure Functions-Releasevorlage.

![](media/functions-how-to-azure-devops/release-template.png)

Das Bereitstellen in einem Bereitstellungsslot wird in der Releasevorlage nicht unterstützt.

## <a name="creating-an-azure-pipeline-using-the-azure-cli"></a>Erstellen einer Azure-Pipeline mithilfe der Azure-Befehlszeilenschnittstelle

Der [Befehl](/cli/azure/functionapp/devops-pipeline#az-functionapp-devops-pipeline-create) `az functionapp devops-pipeline create` erstellt eine Azure-Pipeline zum Erstellen und Veröffentlichen von Codeänderungen in Ihrem Repository. Der Befehl generiert eine neue YAML-Datei, die die Build- und Releasepipeline definiert und an Ihr Repository committet. Das Bereitstellen in einem Bereitstellungsslot wird vom Azure CLI-Befehl nicht unterstützt.
Die Voraussetzungen für diesen Befehl hängen vom Speicherort des Codes ab:

- Code in GitHub:

    - Sie besitzen die Berechtigung **Schreiben** für Ihr Abonnement.

    - Sie sind der Projektadministrator in Azure DevOps.

    - Sie besitzen die Berechtigung zum Erstellen eines persönlichen GitHub-Zugriffstokens mit ausreichenden Berechtigungen. Siehe [GitHub PAT Permission Requirements](https://aka.ms/azure-devops-source-repos) (Anforderungen für persönliche GitHub-Zugriffstoken).

    - Sie besitzen die Berechtigung zum Committen des Masterbranches in Ihrem GitHub-Repository, um die automatisch generierte YAML-Datei zu committen.

- Code in Azure Repos:

    - Sie besitzen die Berechtigung **Schreiben** für Ihr Abonnement.

    - Sie sind der Projektadministrator in Azure DevOps.

## <a name="next-steps"></a>Nächste Schritte

+ [Übersicht zu Azure Functions](functions-overview.md)
+ [Übersicht über Azure DevOps](/azure/devops/pipelines/)
