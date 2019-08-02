---
title: Continuous Deployment – Azure App Service | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die kontinuierliche Bereitstellung in Azure App Service aktivieren.
services: app-service
documentationcenter: ''
author: cephalin
manager: cfowler
ms.assetid: 6adb5c84-6cf3-424e-a336-c554f23b4000
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2019
ms.author: cephalin
ms.reviewer: dariac
ms.custom: seodec18
ms.openlocfilehash: 3c4811d990cfe107bc3bc4e6d359659b1935c6a4
ms.sourcegitcommit: a8b638322d494739f7463db4f0ea465496c689c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/17/2019
ms.locfileid: "68297201"
---
# <a name="continuous-deployment-to-azure-app-service"></a>Continuous Deployment in Azure App Service

Der [Azure App Service](overview.md) ermöglicht die kontinuierliche Bereitstellung von GitHub-, BitBucket- und [Azure Repos-](https://azure.microsoft.com/services/devops/repos/)Repositorys durch Abrufen der neuesten Updates. Dieser Artikel zeigt Ihnen, wie Sie das Azure-Portal nutzen können, um Ihre App kontinuierlich über den Kudu Build-Dienst oder [Azure Pipelines](https://azure.microsoft.com/services/devops/pipelines/) bereitzustellen. 

Weitere Informationen zu den Quellcodeverwaltungsdiensten finden Sie unter [Erstellen eines Repositorys (GitHub)], [Erstellen eines Repositorys (BitBucket)] oder [Erstellen eines neuen Git-Repository (Azure Repos)].

Informationen zur manuellen Konfiguration der fortlaufenden Bereitstellung aus einem Cloud-Repository, das das Portal nicht direkt unterstützt, wie beispielsweise [GitLab](https://gitlab.com/), finden Sie unter [„Einrichten der kontinuierlichen Bereitstellung mit manuellen Schritten“](https://github.com/projectkudu/kudu/wiki/Continuous-deployment#setting-up-continuous-deployment-using-manual-steps).

[!INCLUDE [Prepare repository](../../includes/app-service-deploy-prepare-repo.md)]

## <a name="authorize-azure-app-service"></a>Autorisieren von Azure App Service 

Um Azure Repos zu verwenden, stellen Sie sicher, dass Ihre Azure DevOps-Organisation mit Ihrem Azure-Abonnement verknüpft ist. Weitere Informationen finden Sie unter [„Einrichten eines Azure DevOps-Dienstkontos zur Bereitstellung für eine Webanwendung“](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App).

Für Bitbucket oder GitHub autorisieren Sie den Azure App Service, sich mit Ihrem Repository zu verbinden. Sie müssen sich nur einmal bei einem Quellcodverwaltungsdienst autorisieren. 

1. Wählen Sie **„App Services“** in der linken Navigationsspalte des [Azure-Portals](https://portal.azure.com) und dann die Web-App, die Sie bereitstellen möchten. 
   
1. Wählen Sie auf der Anwendungsseite im linken Menü **„Bereitstellungscenter“** aus.
   
1. Wählen Sie auf der Seite **„Bereitstellungscenter“** die Option **GitHub** oder **Bitbucket** und dann **„Autorisieren“** . 
   
   ![Wählen Sie den Quellcodeverwaltungdienst und dann „Autorisieren“.](media/app-service-continuous-deployment/github-choose-source.png)
   
1. Melden Sie sich bei Bedarf beim Dienst an und folgen Sie den Anweisungen zur Autorisierung. 

## <a name="enable-continuous-deployment"></a>Aktivieren von Continuous Deployment 

Nachdem Sie einen Quellcodeverwaltungdienst autorisiert haben, konfigurieren Sie Ihre Anwendung für die fortlaufende Bereitstellung über den integrierten [Kudu App Service Build-Server](#option-1-use-the-app-service-build-service) oder über [Azure Pipelines](#option-2-use-azure-pipelines). 

### <a name="option-1-use-the-app-service-build-service"></a>Option 1: Verwendung des App Service-Builddiensts

Sie können den integrierten Kudu App Service Build-Server verwenden, um die Bereitstellung kontinuierlich von GitHub, Bitbucket oder Azure Repos aus vorzunehmen. 

1. Wählen Sie **App Services** in der linken Navigation des [Azure-Portals](https://portal.azure.com) und dann die Web-App, die Sie bereitstellen möchten. 
   
1. Wählen Sie auf der Anwendungsseite im linken Menü **„Bereitstellungscenter“** aus.
   
1. Wählen Sie auf der Seite **„Bereitstellungscenter“** Ihren autorisierten Quellcodeverwaltungdienst aus und wählen Sie **„Weiter“** . Für GitHub oder Bitbucket können Sie auch **„Konto ändern“** wählen, um das autorisierte Konto zu ändern. 
   
   > [!NOTE]
   > Um Azure Repos zu verwenden, stellen Sie sicher, dass Ihre Azure DevOps Services-Organisation mit Ihrem Azure-Abonnement verknüpft ist. Weitere Informationen finden Sie unter [„Einrichten eines Azure DevOps-Dienstkontos zur Bereitstellung für eine Webanwendung“](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App).
   
1. Wählen Sie für GitHub oder Azure Repos auf der Seite **„Buildanbieter“** die Option **„App Service-Builddienst“** und dann **„Weiter“** . Bitbucket verwendet immer den App Service-Builddienst.
   
   ![Wählen Sie die Option „App Service-Builddienst“ und dann „Weiter“.](media/app-service-continuous-deployment/choose-kudu.png)
   
1. Auf der Seite **Konfigurieren**:
   
   - Wählen Sie das Dropdownmenü für GitHub und wählen Sie dann die **„Organisation“** , **„Repository“** und **„Branch“** , die sie fortlaufend bereitstellen möchten.
     
     > [!NOTE]
     > Wenn Sie keine Repositorys sehen, müssen Sie möglicherweise den Azure App Service in GitHub autorisieren. Navigieren Sie zu Ihrem GitHub-Repository und wählen Sie **Einstellungen** > **Anwendungen** > **Autorisierte OAuth-Apps**. Wählen Sie **Azure App Service** und dann **„Zuweisung“** .
     
   - Wählen Sie für Bitbucket **„Team“** aus, dann **„Repository“** und die **„Branch“** , die Sie fortlaufend nutzen möchten.
     
   - Wählen Sie für Azure Repos die **Azure DevOps-Organisation**, das **Projekt**, das **Repository** und **Branch** aus, die Sie fortlaufend nutzen möchten.
     
     > [!NOTE]
     > Wenn Ihre Azure DevOps-Organisation nicht aufgeführt ist, stellen Sie sicher, dass sie mit Ihrem Azure-Abonnement verknüpft ist. Weitere Informationen finden Sie unter [„Einrichten eines Azure DevOps-Dienstkontos zur Bereitstellung für eine Webanwendung“](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App).
     
1. Wählen Sie **Weiter**.
   
   ![Geben Sie die Repository-Informationen ein und wählen Sie dann „Weiter“.](media/app-service-continuous-deployment/configure-kudu.png)
   
1. Nachdem Sie den Buildanbieter konfiguriert haben, überprüfen Sie die Einstellungen auf der Seite **„Zusammenfassung“** und wählen Sie dann **„Fertig stellen“** .
   
   Neue Commits im ausgewählten Repository und Branch werden nun fortlaufend in Ihrer App Service-App bereitgestellt. Auf der Seite **„Bereitstellungscenter“** können Sie die Commits und Bereitstellen verfolgen.
   
   ![Verfolgung von Commits und Bereitstellen im Bereitstellungscenter](media/app-service-continuous-deployment/github-finished.png)

### <a name="option-2-use-azure-pipelines"></a>Option 2: Verwendung von Azure Pipelines 

Wenn Ihr Konto über die erforderlichen Berechtigungen verfügt, können Sie Azure Pipelines so einrichten, dass sie kontinuierlich aus GitHub- oder Azure Repos-Repositorys bereitgestellt werden. Weitere Informationen zur Bereitstellung über Azure Pipelines finden Sie unter [„Bereitstellen einer Webanwendung für Azure App Services“](/azure/devops/pipelines/apps/cd/deploy-webdeploy-webapps).

Folgendes ist erforderlich, damit Azure App Service kontinuierliche Azure Pipelines in Ihrer Azure DevOps-Organisation erstellen kann: 

- Ihr Azure-Konto muss über Berechtigungen zum Schreiben in Azure Active Directory und zum Erstellen eines Diensts verfügen. 
  
- Ihr Azure-Konto muss in Ihrem Azure-Abonnement über die Rolle **Besitzer** verfügen.

- Sie müssen ein Administrator in dem Azure DevOps-Projekt sein, das Sie verwenden möchten.

Konfigurieren von Azure Pipelines (Vorschauversion):

1. Wählen Sie **App Services** in der linken Navigation des [Azure-Portals](https://portal.azure.com) und dann die Web-App, die Sie bereitstellen möchten. 
   
1. Wählen Sie auf der Anwendungsseite im linken Menü **„Bereitstellungscenter“** aus.
   
1. Wählen Sie auf der Seite **Buildanbieter** nacheinander **Azure Pipelines (Vorschau)** und dann **Weiter** aus. 
   
1. Auf der Seite **„Konfigurieren“** im Abschnitt **„Code“** :
   
   - Wählen Sie das Dropdownmenü für GitHub und wählen Sie dann die **„Organisation“** , **„Repository“** und **„Branch“** , die sie fortlaufend bereitstellen möchten.
     
     > [!NOTE]
     > Wenn Sie keine Repositorys sehen, müssen Sie möglicherweise den Azure App Service in GitHub autorisieren. Navigieren Sie zu Ihrem GitHub-Repository und wählen Sie **Einstellungen** > **Anwendungen** > **Autorisierte OAuth-Apps**. Wählen Sie **Azure App Service** und dann **„Zuweisung“** .
     
   - Wählen Sie für Azure Repos die **Azure DevOps-Organisation**, das **Projekt**, das **Repository** und **Branch** aus, die Sie fortlaufend nutzen möchten oder konfigurieren Sie eine neue Azure DevOps-Organisation.
     
     > [!NOTE]
     > Wenn Ihre vorhandene Azure DevOps-Organisation nicht aufgeführt ist, müssen Sie sie möglicherweise mit Ihrem Azure-Abonnement verknüpfen. Weitere Informationen finden Sie unter [Definieren Ihrer CD-Releasepipeline](/azure/devops/pipelines/apps/cd/deploy-webdeploy-webapps#cd).
     
1. Wählen Sie **Weiter**.
   
1. Geben Sie für Azure Repos im Abschnitt **„Build“** das Sprachenframework an, das Azure Pipelines verwenden soll, um Build-Aufgaben auszuführen, und wählen Sie dann **„Weiter“** .
   
1. Wählen Sie auf der Seite **„Test“** aus, ob Auslastungstests aktiviert werden sollen, und wählen Sie dann **„Weiter“** .
   
1. Abhängig vom [Tarif](https://azure.microsoft.com/pricing/details/app-service/plans/) Ihres App Service-Plans wird ggf. auch eine Seite **Für Staging bereitstellen** angezeigt. Wählen Sie, ob [Bereitstellungsslots aktiviert werden sollen](deploy-staging-slots.md), und wählen Sie **Weiter** aus.
   
   > [!NOTE]
   > Azure Pipelines erlaubt keine Continuous Delivery an den Produktionsslot. Diese Einschränkung soll versehentliche Bereitstellungen für die Produktion verhindern. Richten Sie einen Continuous Delivery-Vorgang für einen Stagingslot ein, überprüfen Sie die Änderungen dort und tauschen Sie die Slots aus, wenn Sie fertig sind.
   
1. Nachdem Sie den Buildanbieter konfiguriert haben, überprüfen Sie die Einstellungen auf der Seite **„Zusammenfassung“** und wählen Sie dann **„Fertig stellen“** .
   
   Neue Commits im ausgewählten Repository und Branch werden nun fortlaufend in Ihrer App Service-App bereitgestellt. Auf der Seite **„Bereitstellungscenter“** können Sie die Commits und Bereitstellen verfolgen.
   
   ![Verfolgung von Commits und Bereitstellen im Bereitstellungscenter](media/app-service-continuous-deployment/github-finished.png)

## <a name="disable-continuous-deployment"></a>Deaktivieren der fortlaufenden Bereitstellung

Um Continuous Deployment zu deaktivieren, wählen Sie **„Trennen“** oben auf der Seite **„Bereitstellungscenter“** Ihrer Anwendung.

![Deaktivieren der fortlaufenden Bereitstellung](media/app-service-continuous-deployment/disable.png)

[!INCLUDE [What happens to my app during deployment?](../../includes/app-service-deploy-atomicity.md)]

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Untersuchen Sie Ursachen für häufige Probleme mit Continuous Deployment](https://github.com/projectkudu/kudu/wiki/Investigating-continuous-deployment)
* [Verwenden von Azure PowerShell](/powershell/azureps-cmdlets-docs)
* [Git-Dokumentation](https://git-scm.com/documentation)
* [Projekt Kudu](https://github.com/projectkudu/kudu/wiki)

[Erstellen eines Repositorys (GitHub)]: https://help.github.com/articles/create-a-repo
[Erstellen eines Repositorys (BitBucket)]: https://confluence.atlassian.com/get-started-with-bitbucket/create-a-repository-861178559.html
[Erstellen eines neuen Git-Repository (Azure Repos)]: /azure/devops/repos/git/creatingrepo
