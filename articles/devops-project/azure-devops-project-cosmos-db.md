---
title: 'Tutorial: Bereitstellen von auf Azure Cosmos DB basierenden Node.js-Apps mit Azure DevOps Projects'
description: Azure DevOps Projects erleichtert die ersten Schritte mit Azure. Mit DevOps Projects können Sie Ihre auf Azure Cosmos DB basierende Node.js-App in wenigen Schritten in einer Windows-Web-App bereitstellen.
ms.author: mlearned
ms.manager: douge
ms.prod: devops
ms.technology: devops-cicd
ms.topic: tutorial
ms.date: 07/11/2019
author: mlearned
monikerRange: vsts
ms.openlocfilehash: 4310807423600b96078ee48a04a5ad6dab68cd7e
ms.sourcegitcommit: af31deded9b5836057e29b688b994b6c2890aa79
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67813116"
---
# <a name="deploy-nodejs-apps-powered-by-azure-cosmos-db-with-devops-projects"></a>Bereitstellen von auf Azure Cosmos DB basierenden Node.js-Apps mit DevOps Projects

Azure DevOps Projects bietet eine optimierte Umgebung, in der Sie Ihren vorhandenen Code und Ihr Git-Repository verwenden oder eine Beispielanwendung auswählen können, um eine Continuous Integration- und Continuous Delivery-Pipeline (CI/CD) für Azure zu erstellen.

Von DevOps Projects werden darüber hinaus die folgenden Aktionen ausgeführt:

* Automatisches Erstellen von Azure-Ressourcen wie Azure Cosmos DB, Application Insights, App Service und App Service

* Erstellen und Konfigurieren einer Releasepipeline in Azure DevOps für CI/CD

In diesem Lernprogramm lernen Sie Folgendes:

> [!div class="checklist"]
> * Bereitstellen einer auf Azure Cosmos DB basierenden Node.js-App mithilfe von DevOps Projects
> * Konfigurieren von Azure DevOps und eines Azure-Abonnements
> * Untersuchen der Azure Cosmos DB-Instanz
> * Überprüfen der CI-Pipeline
> * Überprüfen der CD-Pipeline
> * Committen der Änderungen in Git und automatisches Bereitstellen dieser Änderungen in Azure
> * Bereinigen der Ressourcen

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Über [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/) erhalten Sie ein kostenloses Abonnement.

## <a name="use-devops-projects-to-deploy-nodejs-app"></a>Bereitstellen einer Node.js-App mit DevOps Projects

Mit DevOps Projects wird eine CI/CD-Pipeline in Azure Pipelines erstellt. Sie können eine neue Azure DevOps-Organisation erstellen oder eine bestehende Organisation verwenden. DevOps Projects erstellt außerdem Azure-Ressourcen (etwa Azure Cosmos DB, Application Insights, App Service und einen App Service-Plan) im Azure-Abonnement Ihrer Wahl.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)

1. Wählen Sie im linken Bereich den Abschnitt **Ressource erstellen** aus.

1. Geben Sie in das Suchfeld **DevOps Projects** ein, und klicken Sie dann auf **Hinzufügen**.

   ![DevOps-Projekte](_img/azure-devops-project-cosmos-db/devops-project.png)

1. Wählen Sie als Laufzeit **Node.js** und anschließend **Weiter** aus. Wählen Sie unter **Anwendungsframework auswählen** die Option **Express.js** aus.

1. Aktivieren Sie für **Cosmos DB** den Abschnitt **Datenbank hinzufügen**, und klicken Sie auf **Weiter**.

    ![Hinzufügen einer Datenbank](_img/azure-devops-project-cosmos-db/add-database.png)

    Cosmos DB unterstützt verschiedene Anwendungsframeworks wie **Express.js**, **Node.js-Beispiel-Apps** und **Sail.js**. In diesem Tutorial wird **Express.js** verwendet.

1. Wählen Sie einen Azure-Dienst zum Bereitstellen der Anwendung aus. Sie verfügen über verschiedene Dienste wie Windows-Web-App, Kubernetes Service und Web-App für Container. In diesem Tutorial wird **Windows-Web-App** verwendet. Klicken Sie auf **Weiter**.

## <a name="configure-azure-devops-and-azure-subscription"></a>Konfigurieren von Azure DevOps und eines Azure-Abonnements

1. Geben Sie einen Namen für Ihr Azure DevOps-Projekt ein.

1. Sie können eine neue Azure DevOps-Organisation erstellen oder eine bestehende Organisation auswählen.

1. Wählen Sie Ihr Azure-Abonnement.

1. Wenn Sie weitere Azure-Konfigurationseinstellungen anzeigen und den Tarif und den Standort ermitteln möchten, klicken Sie auf „Weitere Einstellungen“. In diesem Bereich werden verschiedene Optionen zum Konfigurieren des Tarifs und Standorts der Azure-Dienste angezeigt.

1. Beenden Sie den Konfigurationsbereich von Azure, und wählen Sie **Fertig** aus.

1. Nach einigen Minuten ist der Vorgang abgeschlossen. Eine Node.js-Beispiel-App wird in einem Git-Repository in Ihrer Azure DevOps-Organisation eingerichtet, eine Azure Cosmos DB-, eine App Service- und eine Application Insights-Instanz sowie ein App Service-Plan werden erstellt, eine CI/CD-Pipeline wird ausgeführt, und Ihre App wird in Azure bereitgestellt.

   Wenn alle diese Schritte abgeschlossen sind, wird das Azure DevOps Project-Dashboard im Azure-Portal angezeigt. Sie können das DevOps Projects-Dashboard auch direkt im Azure-Portal über **Alle Ressourcen** aufrufen.

   Dieses Dashboard bietet Einblick in Ihr Azure DevOps-Coderepository, in Ihre CI/CD-Pipeline und in Ihre Azure Cosmos DB-Instanz. Sie können zusätzliche CI/CD-Optionen in Ihrer Azure DevOps-Pipeline konfigurieren. Wählen Sie auf der rechten Seite **Azure Cosmos DB** für die Anzeige aus.

## <a name="examine-the-azure-cosmos-db"></a>Untersuchen der Azure Cosmos DB-Instanz

DevOps Projects konfiguriert automatisch die Cosmos DB-Instanz, die Sie untersuchen und anpassen können. Gehen Sie wie folgt vor, um sich mit der Cosmos DB-Instanz vertraut zu machen:

1. Navigieren Sie zum DevOps Projects-Dashboard.

    ![DevOps Projects-Dashboard](_img/azure-devops-project-cosmos-db/devops-project-dashboard.png)

1. Wählen Sie auf der rechten Seite die Cosmos DB-Instanz aus. Für die Cosmos DB-Instanz wird ein Bereich geöffnet. In dieser Ansicht können Sie verschiedene Aktionen ausführen und beispielsweise den Betrieb überwachen oder Protokolle durchsuchen.

    ![Funktionen-App](_img/azure-devops-project-cosmos-db/cosmos-db.png)

## <a name="examine-the-ci-pipeline"></a>Überprüfen der CI-Pipeline

Mit DevOps Projects wird automatisch eine CI/CD-Pipeline in Ihrer Azure DevOps-Organisation konfiguriert. Sie können die Pipeline untersuchen und anpassen. Gehen Sie wie folgt vor, um sich damit vertraut zu machen:

1. Navigieren Sie zum DevOps Projects-Dashboard.

1. Klicken Sie unter **Build** auf den Link. Auf einer Browserregisterkarte wird die Buildpipeline für Ihr neues Projekt angezeigt.

    ![Entwickeln](_img/azure-devops-project-cosmos-db/build.png)

1. Wählen Sie **Bearbeiten** aus. In diesem Bereich können Sie sich die verschiedenen Aufgaben ansehen, die Sie für Ihre Buildpipeline ausführen können. Vom Build werden verschiedene Aufgaben durchgeführt. Beispielsweise wird Quellcode aus dem Git-Repository abgerufen, die Anwendung wird erstellt, Komponententests werden ausgeführt, und für Bereitstellungen verwendete Ausgaben werden veröffentlicht.

1. Wählen Sie **Trigger** aus. Mit DevOps Projects wird automatisch ein CI-Trigger erstellt, und mit jedem für das Repository ausgeführten Commit wird ein neuer Build gestartet. Optional können Sie Branches in den CI-Prozess einbeziehen oder davon ausschließen.

1. Wählen Sie **Aufbewahrung** aus. Abhängig vom Szenario können Sie Richtlinien zum Aufbewahren oder Entfernen einer bestimmten Anzahl von Builds festlegen.

1. Wählen Sie oben in der Buildpipeline den Buildpipelinenamen aus.

1. Ersetzen Sie den Namen Ihrer Buildpipeline durch einen aussagekräftigeren Namen, und wählen Sie im Dropdownmenü **Speichern und in Warteschlange einreihen** die Option **Speichern** aus.

1. Wählen Sie unter dem Buildpipelinenamen **Verlauf** aus. In diesem Bereich wird ein Überwachungsprotokoll mit den letzten Änderungen für den Build angezeigt. An der Buildpipeline vorgenommene Änderungen werden von Azure DevOps nachverfolgt, sodass Sie verschiedene Versionen vergleichen können.

## <a name="examine-the-cd-release-pipeline"></a>Überprüfen der CD-Releasepipeline

Mit DevOps Projects werden die erforderlichen Schritte zum Bereitstellen über Ihre Azure DevOps-Organisation in Ihrem Azure-Abonnement automatisch erstellt und konfiguriert. Diese Schritte umfassen die Konfiguration einer Azure-Dienstverbindung zur Authentifizierung von Azure DevOps für Ihr Azure-Abonnement. Die Automatisierung erstellt darüber hinaus eine Releasepipeline, mit der CD in Azure bereitgestellt wird. Gehen Sie wie folgt vor, um weitere Informationen zur Releasepipeline zu erhalten:

1. Navigieren Sie zu **Pipelines | Releases**.

1. Klicken Sie auf **Bearbeiten**.

1. Wählen Sie unter **Artefakte** die Option **Ablegen** aus. Die in den vorherigen Schritten untersuchte Buildpipeline erzeugt die für das Artefakt verwendete Ausgabe.

1. Wählen Sie rechts neben dem Symbol **Ablegen** die Option **Continuous Deployment-Trigger** aus. Diese Releasepipeline enthält einen aktivierten CD-Trigger. Jedes Mal, wenn ein neues Buildartefakt verfügbar ist, wird von diesem CD-Trigger eine Bereitstellung ausgeführt. Optional können Sie den Trigger deaktivieren, sodass Ihre Bereitstellungen manuell ausgeführt werden müssen.

1. Wählen Sie auf der rechten Seite den Abschnitt **Releases anzeigen** aus, um einen Releaseverlauf anzuzeigen.

1. Klicken Sie auf das Release. Dadurch wird die Pipeline angezeigt. Klicken Sie auf eine beliebige Umgebung, um die **Zusammenfassung, Commits** und die zugehörigen **Arbeitselemente** für das Release zu überprüfen.

1. Wählen Sie **Commits** aus. In dieser Ansicht werden die dieser Bereitstellung zugeordneten Codecommits angezeigt. Vergleichen Sie Releases, um die Commitunterschiede zwischen den einzelnen Bereitstellungen anzuzeigen.

1. Wählen Sie **Protokolle anzeigen** aus. Die Protokolle enthalten nützliche Informationen zum Bereitstellungsprozess. Sie können während und nach Bereitstellungen angezeigt werden.

## <a name="commit-code-changes-and-execute-cicd"></a>Ausführen eines Commits für Codeänderungen und Ausführen von CI/CD

> [!NOTE]
> Mit dem folgenden Verfahren wird die CI/CD-Pipeline durch eine einfache Textänderung getestet.

Nun können Sie mithilfe eines CI/CD-Prozesses, mit dem Ihre aktuelle Arbeit automatisch in Azure App Service bereitgestellt wird, mit einem Team an Ihrer App zusammenarbeiten. Mit jeder Änderung am GitHub-Repository wird in Azure DevOps ein Build gestartet, und durch eine CD-Pipeline wird eine Bereitstellung in Azure ausgeführt. Führen Sie die Schritte in diesem Abschnitt aus, oder nutzen Sie eine andere Methode zum Committen Ihrer Änderungen in Ihrem Repository. Sie können beispielsweise das Git-Repository in Ihrem bevorzugten Tool oder Ihrer bevorzugten IDE klonen und anschließend Änderungen per Push in dieses Repository übertragen.

1. Wählen Sie im Azure DevOps-Menü **Repos | Dateien** aus, und navigieren Sie zu Ihrem Repository.

1. Das Repository enthält bereits Code, der auf der Anwendungssprache basiert, die Sie bei der Erstellung ausgewählt haben. Öffnen Sie die Datei **Application/views/index.pug**.

1. Wählen Sie **Bearbeiten** aus, und ändern Sie **Zeile 15**. Ändern Sie sie beispielsweise in **My First deployment to Azure App Service powered by Azure Cosmos DB**.

1. Wählen Sie oben rechts **Committen** und dann erneut **Committen** aus, um Ihre Änderung per Push zu übertragen.

     Kurz danach wird in Azure DevOps ein Build gestartet und zur Bereitstellung der Änderungen ein Release ausgeführt. Überwachen Sie den Buildstatus auf dem DevOps Projects-Dashboard oder im Browser mit Ihrer Azure DevOps-Organisation.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Sie können die erstellten zugehörigen Ressourcen löschen, wenn Sie sie nicht mehr benötigen. Verwenden Sie dazu die Funktion **Löschen** auf dem DevOps Projects-Dashboard.

## <a name="next-steps"></a>Nächste Schritte

Diese Build- und Releasepipelines können Sie optional an die Anforderungen Ihres Teams anpassen. Sie können dieses CI/CD-Muster auch als Vorlage für Ihre anderen Pipelines verwenden. In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Bereitstellen einer auf Azure Cosmos DB basierenden Node.js-App mithilfe von DevOps Projects
> * Konfigurieren von Azure DevOps und eines Azure-Abonnements 
> * Untersuchen der Azure Cosmos DB-Instanz
> * Überprüfen der CI-Pipeline
> * Überprüfen der CD-Pipeline
> * Committen von Änderungen in Git und automatisches Bereitstellen dieser Änderungen in Azure
> * Bereinigen von Ressourcen
