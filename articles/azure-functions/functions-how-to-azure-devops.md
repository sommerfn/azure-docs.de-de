---
title: Continuous Delivery von Funktionscodeaktualisierungen mit Azure DevOps – Azure Functions
description: Erfahren Sie, wie Sie eine Azure DevOps-Pipeline mit Azure Functions als Ziel einrichten.
author: ahmedelnably
manager: jeconnoc
ms.service: azure-functions
ms.topic: conceptual
ms.date: 04/18/2019
ms.author: aelnably
ms.openlocfilehash: 4fb01eac53151799a0def00d13f18619faf437f6
ms.sourcegitcommit: 4c3d6c2657ae714f4a042f2c078cf1b0ad20b3a4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2019
ms.locfileid: "72927529"
---
# <a name="continuous-delivery-by-using-azure-devops"></a>Continuous Delivery mit Azure DevOps

Sie können Ihre Funktion mit [Azure Pipelines](/azure/devops/pipelines/) automatisch in einer Azure Functions-App bereitstellen.

Sie haben zwei Möglichkeiten zum Definieren Ihrer Pipeline:

- **YAML-Datei**: In einer YAML-Datei wird die Pipeline beschrieben. Die Datei kann einen Abschnitt mit den Buildschritten und einen Releaseabschnitt enthalten. Die YAML-Datei muss sich im gleichen Repository wie die App befinden.
- **Vorlage**: Vorlagen sind vorgefertigte Tasks, die Ihre App erstellen oder bereitstellen.

## <a name="yaml-based-pipeline"></a>YAML-basierte Pipeline

Zum Erstellen einer YAML-basierten Pipeline erstellen Sie die App zunächst und stellen sie dann bereit.

### <a name="build-your-app"></a>Erstellen der App

Wie Sie Ihre App in Azure Pipelines erstellen, hängt von der Programmiersprache Ihrer App ab. Jede Sprache verfügt über bestimmte Buildschritte, mit denen ein Bereitstellungsartefakt erstellt wird. Ein Bereitstellungsartefakt wird zum Bereitstellen Ihrer Funktions-App in Azure verwendet.

#### <a name="net"></a>.NET

Sie können die YAML-Datei zum Erstellen einer .NET-App mithilfe des folgenden Beispiels erstellen:

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

Sie können das folgende Beispiel zum Erstellen einer YAML-Datei verwenden, um eine JavaScript-App zu erstellen:

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

Sie können das folgende Beispiel zum Erstellen einer YAML-Datei verwenden, um eine Python-App zu erstellen. Python wird nur für Linux Azure Functions unterstützt. Die YAML-Datei für Python 3.7 kann durch Ersetzen aller Vorkommen von 3.6 durch 3.7 in dieser YAML-Datei erstellt werden.

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

Sie können das folgende Beispiel zum Erstellen einer YAML-Datei verwenden, um eine PowerShell-App zu verpacken. PowerShell wird nur für Windows Azure Functions unterstützt.

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

Sie müssen abhängig vom Hostbetriebssystem eines der folgenden YAML-Beispiele in der YAML-Datei angeben.

#### <a name="windows-function-app"></a>Windows-Funktions-App

Der folgende Codeausschnitt kann verwendet werden, um eine Windows-Funktions-App bereitzustellen:

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

Der folgende Codeausschnitt kann verwendet werden, um eine Linux-Funktions-App bereitzustellen:

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

Vorlagen in Azure DevOps sind vordefinierte Gruppen von Tasks, die eine App erstellen oder bereitstellen.

### <a name="build-your-app"></a>Erstellen der App

Wie Sie Ihre App in Azure Pipelines erstellen, hängt von der Programmiersprache Ihrer App ab. Jede Sprache verfügt über bestimmte Buildschritte, mit denen ein Bereitstellungsartefakt erstellt wird. Ein Bereitstellungsartefakt wird zum Aktualisieren Ihrer Funktions-App in Azure verwendet.

Wenn Sie die integrierten Buildvorlagen verwenden möchten, wählen Sie beim Erstellen einer neuen Buildpipeline die Option **Klassischen Editor verwenden** aus, um eine Pipeline mithilfe der Designer-Vorlagen zu erstellen.

![Auswählen des klassisches Azure Pipelines-Editors](media/functions-how-to-azure-devops/classic-editor.png)

Nachdem Sie die Quelle Ihres Codes konfiguriert haben, suchen Sie nach Azure Functions-Buildvorlagen. Wählen Sie die Vorlage aus, die der Sprache Ihrer App entspricht.

![Auswählen einer Azure Functions-Buildvorlage](media/functions-how-to-azure-devops/build-templates.png)

In manchen Fällen weisen Buildartefakte eine bestimmte Ordnerstruktur auf. Möglicherweise müssen Sie das Kontrollkästchen **Archivpfaden den Stammordnernamen voranstellen** aktivieren.

![Die Option zum Voranstellen des Namens des Stammordners](media/functions-how-to-azure-devops/prepend-root-folder.png)

#### <a name="javascript-apps"></a>JavaScript-Apps

Wenn Ihre JavaScript-App eine Abhängigkeit von nativen Windows-Modulen aufweist, müssen Sie die Agent-Poolversion auf **Hosted VS2017** aktualisieren.

![Aktualisieren der Agent-Poolversion](media/functions-how-to-azure-devops/change-agent.png)

### <a name="deploy-your-app"></a>Bereitstellen Ihrer App

Wenn Sie eine neue Releasepipeline erstellen, suchen Sie nach der Azure Functions-Releasevorlage.

![Suchen nach der Azure Functions-Releasevorlage](media/functions-how-to-azure-devops/release-template.png)

Das Bereitstellen in einem Bereitstellungsslot wird in der Releasevorlage nicht unterstützt.

## <a name="create-a-build-pipeline-by-using-the-azure-cli"></a>Erstellen einer Buildpipeline mithilfe der Azure CLI

Verwenden Sie den [Befehl](/cli/azure/functionapp/devops-pipeline#az-functionapp-devops-pipeline-create) `az functionapp devops-pipeline create` , um eine Buildpipeline in Azure zu erstellen. Die Buildpipeline wird erstellt, um alle Codeänderungen zu erstellen und freizugeben, die in Ihrem Repository vorgenommen werden. Der Befehl generiert eine neue YAML-Datei, die die Build- und Releasepipeline definiert und dann an Ihr Repository committet. Die Voraussetzungen für diesen Befehl hängen vom Speicherort des Codes ab.

- Code in GitHub:

    - Sie müssen die Berechtigung **Schreiben** für Ihr Abonnement besitzen.

    - Sie müssen der Projektadministrator in Azure DevOps sein.

    - Sie müssen die Berechtigung zum Erstellen eines persönlichen GitHub-Zugriffstokens (PAT) mit ausreichenden Berechtigungen besitzen. Weitere Informationen finden Sie unter [GitHub-PAT-Berechtigungsanforderungen.](https://aka.ms/azure-devops-source-repos)

    - Sie müssen die Berechtigung zum Committen des Masterbranches in Ihrem GitHub-Repository besitzen, um die automatisch generierte YAML-Datei zu committen.

- Code in Azure Repos:

    - Sie müssen die Berechtigung **Schreiben** für Ihr Abonnement besitzen.

    - Sie müssen der Projektadministrator in Azure DevOps sein.

## <a name="next-steps"></a>Nächste Schritte

- Lesen Sie die [Übersicht zu Azure Functions](functions-overview.md).
- Im Artikel [Übersicht über Azure DevOps](/azure/devops/pipelines/) finden Sie weitere Informationen zu diesem Thema.
