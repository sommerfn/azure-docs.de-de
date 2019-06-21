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
ms.date: 12/03/2018
ms.author: cephalin;dariagrigoriu
ms.custom: seodec18
ms.openlocfilehash: 384f709bb32f973efec39518eaa895e25136fe23
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "66390637"
---
# <a name="continuous-deployment-to-azure-app-service"></a>Continuous Deployment in Azure App Service
Dieser Artikel veranschaulicht das Konfigurieren von Continuous Deployment für [Azure App Service](overview.md). App Service ermöglicht Continuous Deployment aus Bitbucket, GitHub und [Azure DevOps Services](https://www.visualstudio.com/team-services/) durch Pullen der neuesten Updates aus Ihrem vorhandenen Repository in einen dieser Dienste.

Informationen zum manuellen Konfigurieren von Continuous Deployment in einem Cloudrepository, das nicht im Azure-Portal aufgeführt wird, (etwa [GitLab](https://gitlab.com/)), finden Sie unter [Setting up continuous deployment using manual steps](https://github.com/projectkudu/kudu/wiki/Continuous-deployment#setting-up-continuous-deployment-using-manual-steps) (Manuelles Einrichten von Continuous Deployment).

[!INCLUDE [Prepare repository](../../includes/app-service-deploy-prepare-repo.md)]

Veröffentlichen Sie Ihr vorbereitetes Repository in einem der unterstützten Dienste. Weitere Informationen zum Veröffentlichen Ihres Projekts in einem dieser Dienste finden Sie unter [Erstellen eines Repositorys (GitHub)], [Erstellen eines Repositorys (Bitbucket)] und [Erste Schritte mit Azure DevOps Services].

## <a name="deploy-continuously-from-github"></a>Continuous Deployment aus GitHub

Um Continuous Deployment mit GitHub zu aktivieren, navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrer App Service-App-Seite.

Klicken Sie im linken Menü auf **Bereitstellungscenter** > **GitHub** > **Autorisieren**. Befolgen Sie die Eingabeaufforderungen zur Autorisierung. 

![](media/app-service-continuous-deployment/github-choose-source.png)

Die Autorisierung mit GitHub muss nur ein Mal erfolgen. Wenn Sie bereits autorisiert sind, klicken Sie einfach auf **Weiter**. Sie können das autorisierte GitHub-Konto ändern, indem Sie auf **Konto ändern** klicken.

![](media/app-service-continuous-deployment/github-continue.png)

Wählen Sie auf der Seite **Buildanbieter** den Buildanbieter aus, und klicken Sie auf **Weiter**.

### <a name="option-1-use-app-service-kudu-build-server"></a>Option 1: Verwenden des App Service-Kudu-Buildservers

Wählen Sie auf der Seite **Konfigurieren** die Organisation, das Repository und den Branch aus, aus dem Continuous Deployment erfolgen soll. Klicken Sie abschließend auf **Weiter**.

Um eine Bereitstellung aus einem Repository in einer GitHub-Organisation auszuführen, gehen Sie zu GitHub, und navigieren Sie zu **Settings** (Einstellungen)  > **Applications** (Anwendungen)  > **Authorized OAuth Apps** (Autorisierte OAuth-Apps). Klicken Sie dann auf „Azure App Service“.

![Einstellungen > Anwendungen > Autorisierte OAuth-Apps > Azure App Service](media/app-service-continuous-deployment/github-settings-navigation.png)

Erteilen Sie App Service auf der nächsten Seite Zugriff auf die Repositorys Ihrer Organisation, indem Sie auf die Schaltfläche „Grant“ (Erteilen) auf der rechten Seite klicken.

![Klicken auf „Erteilen“, um App Service den Zugriff auf die Repositorys der Organisation zu erteilen](media/app-service-continuous-deployment/grant-access.png)

Ihre Organisation sollte jetzt in der Liste „Organization“ (Organisation) auf der Seite **Configure** (Konfigurieren) im Bereitstellungscenter angezeigt werden.

### <a name="option-2-use-azure-pipelines-preview"></a>Option 2: Verwenden von Azure Pipelines (Preview)

> [!NOTE]
> Damit App Service die erforderlichen Azure-Pipelines in Ihrer Azure DevOps Services-Organisation erstellt, muss Ihr Azure-Konto die Rolle **Besitzer** in Ihrem Azure-Abonnement aufweisen.
>

Wählen Sie auf der Seite **Konfigurieren** im Abschnitt **Code** die Organisation, das Repository und den Branch aus, aus dem Continuous Deployment erfolgen soll. Klicken Sie abschließend auf **Weiter**.

Konfigurieren Sie auf der Seite **Konfigurieren** im Abschnitt **Build** eine neue Azure DevOps Services-Organisation, oder geben Sie eine vorhandene Organisation an. Klicken Sie abschließend auf **Weiter**.

> [!NOTE]
> Wenn Sie eine vorhandene Azure DevOps Services-Organisation verwenden möchten, die nicht aufgeführt ist, müssen Sie [die Azure DevOps Services-Organisation mit Ihrem Azure-Abonnement verknüpfen](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App).

Wählen Sie auf der Seite **Test** aus, ob Auslastungstests aktiviert werden sollen, und klicken Sie dann auf **Weiter**.

Abhängig vom [Tarif](https://azure.microsoft.com/pricing/details/app-service/plans/) Ihres App Service-Plans wird ggf. auch eine Seite **Für Staging bereitstellen** angezeigt. Wählen Sie aus, ob [Bereitstellungsslots aktiviert werden sollen](deploy-staging-slots.md), und klicken Sie dann auf **Weiter**.

### <a name="finish-configuration"></a>Fertigstellen der Konfiguration

Überprüfen Sie auf der Seite **Zusammenfassung** die Optionen, und klicken Sie dann auf **Fertig stellen**.

Wenn die Konfiguration abgeschlossen ist, werden neue Commits im ausgewählten Repository mithilfe von Continuous Deployment in Ihrer App Service-App bereitgestellt.

![](media/app-service-continuous-deployment/github-finished.png)

## <a name="deploy-continuously-from-bitbucket"></a>Continuous Deployment aus BitBucket

Um Continuous Deployment mit BitBucket zu aktivieren, navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrer App Service-App-Seite.

Klicken Sie im linken Menü auf **Bereitstellungscenter** > **BitBucket** > **Autorisieren**. Befolgen Sie die Eingabeaufforderungen zur Autorisierung. 

![](media/app-service-continuous-deployment/bitbucket-choose-source.png)

Die Autorisierung mit BitBucket muss nur ein Mal erfolgen. Wenn Sie bereits autorisiert sind, klicken Sie einfach auf **Weiter**. Sie können das autorisierte BitBucket-Konto ändern, indem Sie auf **Konto ändern** klicken.

![](media/app-service-continuous-deployment/bitbucket-continue.png)

Wählen Sie auf der Seite **Konfigurieren** das Repository und den Branch aus, aus dem Continuous Deployment erfolgen soll. Klicken Sie auf **Weiter**, wenn Sie fertig sind.

Überprüfen Sie auf der Seite **Zusammenfassung** die Optionen, und klicken Sie dann auf **Fertig stellen**.

Wenn die Konfiguration abgeschlossen ist, werden neue Commits im ausgewählten Repository mithilfe von Continuous Deployment in Ihrer App Service-App bereitgestellt.

## <a name="deploy-continuously-from-azure-repos-devops-services"></a>Continuous Deployment aus Azure Repos (DevOps Services)

Um Continuous Deployment aus [Azure Repos](https://docs.microsoft.com/azure/devops/repos/index) zu aktivieren, navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrer App Service-App-Seite.

Klicken Sie im linken Menü auf **Bereitstellungscenter** > **Azure Repos** > **Weiter**. 

![](media/app-service-continuous-deployment/vsts-choose-source.png)

Wählen Sie auf der Seite **Buildanbieter** den Buildanbieter aus, und klicken Sie auf **Weiter**.

> [!NOTE]
> Wenn Sie eine vorhandene Azure DevOps Services-Organisation verwenden möchten, die nicht aufgeführt ist, müssen Sie [die Azure DevOps Services-Organisation mit Ihrem Azure-Abonnement verknüpfen](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App).

### <a name="option-1-use-app-service-kudu-build-server"></a>Option 1: Verwenden des App Service-Kudu-Buildservers

Wählen Sie auf der Seite **Konfigurieren** die Azure DevOps Services-Organisation, das Projekt, das Repository und den Branch aus, aus dem Continuous Deployment erfolgen soll. Klicken Sie abschließend auf **Weiter**.

### <a name="option-2-use-azure-devops-services-continuous-delivery"></a>Option 2: Verwenden von Continuous Delivery in Azure DevOps Services

> [!NOTE]
> Damit App Service die erforderlichen Azure-Pipelines in Ihrer Azure DevOps Services-Organisation erstellt, muss Ihr Azure-Konto die Rolle **Besitzer** in Ihrem Azure-Abonnement aufweisen.
>

Wählen Sie auf der Seite **Konfigurieren** im Abschnitt **Code** die Azure DevOps Services-Organisation, das Projekt, das Repository und den Branch aus, aus dem Continuous Deployment erfolgen soll. Klicken Sie abschließend auf **Weiter**.

Geben Sie auf der Seite **Konfigurieren** im Abschnitt **Build** das Sprachframework an, das Azure DevOps Services zum Ausführen der Buildtasks für Ihr ausgewähltes Repository verwenden soll. Klicken Sie abschließend auf **Weiter**.

Wählen Sie auf der Seite **Test** aus, ob Auslastungstests aktiviert werden sollen, und klicken Sie dann auf **Weiter**.

Abhängig vom [Tarif](https://azure.microsoft.com/pricing/details/app-service/plans/) Ihres App Service-Plans wird ggf. auch eine Seite **Für Staging bereitstellen** angezeigt. Wählen Sie aus, ob [Bereitstellungsslots aktiviert werden sollen](deploy-staging-slots.md), und klicken Sie dann auf **Weiter**. Für DevOps sind Continuous Delivery-Vorgänge für den Produktionsslot nicht zulässig. Dies ist beabsichtigt, um versehentliche Bereitstellungen für die Produktion zu verhindern. Sie sollten einen Continuous Delivery-Vorgang für einen Stagingslot einrichten, die Änderungen dort überprüfen und die Slots austauschen, wenn Sie fertig sind.

### <a name="finish-configuration"></a>Fertigstellen der Konfiguration

Überprüfen Sie auf der Seite **Zusammenfassung** die Optionen, und klicken Sie dann auf **Fertig stellen**.

Wenn die Konfiguration abgeschlossen ist, werden neue Commits im ausgewählten Repository mithilfe von Continuous Deployment in Ihrer App Service-App bereitgestellt.

## <a name="disable-continuous-deployment"></a>Deaktivieren der fortlaufenden Bereitstellung

Um Continuous Deployment zu deaktivieren, navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrer App Service-App-Seite.

Klicken Sie im linken Menü auf **Bereitstellungscenter** > **GitHub** oder **Azure DevOps Services** oder **Bitbucket** > **Trennen**.

![](media/app-service-continuous-deployment/disable.png)

[!INCLUDE [What happens to my app during deployment?](../../includes/app-service-deploy-atomicity.md)]

## <a name="additional-resources"></a>Weitere Ressourcen

* [How to investigate common issues with continuous deployment](https://github.com/projectkudu/kudu/wiki/Investigating-continuous-deployment)
* [Verwenden von PowerShell für Azure]
* [Git-Dokumentation]
* [Projekt Kudu](https://github.com/projectkudu/kudu/wiki)
* [Use Azure to automatically generate a CI/CD pipeline to deploy an ASP.NET 4 app (Verwenden von Azure zum automatischen Erstellen einer CI/CD-Pipeline zum Bereitstellen einer ASP.NET 4-App)](https://www.visualstudio.com/docs/build/get-started/aspnet-4-ci-cd-azure-automatic)

[Azure portal]: https://portal.azure.com
[Azure DevOps portal]: https://azure.microsoft.com/services/devops/
[Installing Git]: https://git-scm.com/book/en/Getting-Started-Installing-Git
[Verwenden von PowerShell für Azure]: /powershell/azureps-cmdlets-docs
[Git-Dokumentation]: https://git-scm.com/documentation

[Erstellen eines Repositorys (GitHub)]: https://help.github.com/articles/create-a-repo
[Erstellen eines Repositorys (BitBucket)]: https://confluence.atlassian.com/get-started-with-bitbucket/create-a-repository-861178559.html
[Erste Schritte mit Azure DevOps Services]: https://docs.microsoft.com/azure/devops/user-guide/devops-alm-overview
