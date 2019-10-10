---
title: 'Tutorial: Bereitstellen einer ASP.NET-App und von Azure SQL-Datenbank-Code mithilfe von Azure DevOps Projects'
description: DevOps Projects erleichtert die ersten Schritte mit Azure. Mit DevOps Projects können Sie Ihre ASP.NET-App sowie Azure SQL-Datenbank-Code in wenigen Schritten bereitstellen.
ms.author: mlearned
ms.manager: gwallace
ms.prod: devops
ms.technology: devops-cicd
ms.topic: tutorial
ms.date: 07/09/2018
author: mlearned
monikerRange: vsts
ms.openlocfilehash: 560b16a40d73d07df6c9f446c7196cd930e2274e
ms.sourcegitcommit: c2e7595a2966e84dc10afb9a22b74400c4b500ed
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2019
ms.locfileid: "71971505"
---
# <a name="tutorial-deploy-your-aspnet-app-and-azure-sql-database-code-by-using-azure-devops-projects"></a>Tutorial: Bereitstellen einer ASP.NET-App und von Azure SQL-Datenbank-Code mithilfe von Azure DevOps Projects

Azure DevOps Projects bietet eine vereinfachte Umgebung, in der Sie Ihren vorhandenen Code und Ihr Git-Repository verwenden oder eine Beispielanwendung auswählen können, um eine Continuous Integration- und Continuous Delivery-Pipeline (CI/CD) für Azure zu erstellen. 

Von DevOps Projects werden darüber hinaus die folgenden Aktionen ausgeführt:
* Automatisches Erstellen von Azure-Ressourcen, etwa einer Azure SQL-Datenbank-Instanz
* Erstellen und Konfigurieren einer Releasepipeline in Azure Pipelines, die eine Buildpipeline für CI enthält
* Einrichten einer Releasepipeline für CD 
* Erstellen einer Azure Application Insights-Ressource für die Überwachung

In diesem Lernprogramm lernen Sie Folgendes:

> [!div class="checklist"]
> * Tutorial: Bereitstellen einer ASP.NET-App und von Azure SQL-Datenbank-Code mithilfe von Azure DevOps Projects
> * Konfigurieren von Azure DevOps und eines Azure-Abonnements 
> * Überprüfen der CI-Pipeline
> * Überprüfen der CD-Pipeline
> * Committen von Änderungen in Azure Repos und automatisches Bereitstellen dieser Änderungen in Azure
> * Herstellen einer Verbindung mit Azure SQL-Datenbank 
> * Bereinigen von Ressourcen

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Über [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/) erhalten Sie ein kostenloses Abonnement.

## <a name="create-a-project-in-devops-projects-for-an-aspnet-app-and-an-azure-sql-database"></a>Erstellen eines Projekts in DevOps Projects für eine ASP.NET-App und Azure SQL-Datenbank

Mit DevOps Projects wird eine CI/CD-Pipeline in Azure Pipelines erstellt. Sie können eine neue Azure DevOps-Organisation erstellen oder eine bestehende Organisation verwenden. Ferner werden mit DevOps Projects Azure-Ressourcen wie eine Azure SQL-Datenbank-Instanz im Azure-Abonnement Ihrer Wahl erstellt.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Wählen Sie im linken Bereich **Ressource erstellen**.

1. Geben Sie in das Suchfeld **DevOps Projects** ein, und wählen sie dann **Erstellen** aus.

    ![DevOps Projects-Dashboard](_img/azure-devops-project-github/fullbrowser.png)

1. Wählen Sie **.NET** und anschließend **Weiter** aus.

1. Wählen Sie unter **Anwendungsframework auswählen** die Option **ASP.NET** aus.

1. Wählen Sie **Datenbank hinzufügen** und anschließend **Weiter** aus.  
    Das Anwendungsframework, das Sie in einem vorherigen Schritt ausgewählt haben, bestimmt den Typ des hier verfügbaren Bereitstellungsziels für den Azure-Dienst. 
    
1. Klicken Sie auf **Weiter**.

## <a name="configure-azure-devops-and-an-azure-subscription"></a>Konfigurieren von Azure DevOps und eines Azure-Abonnements

1. Sie können eine neue Azure DevOps-Organisation erstellen oder eine bestehende Organisation auswählen. 

1. Geben Sie einen Namen für Ihr Azure DevOps-Projekt ein. 

1. Wählen Sie Ihre Azure-Abonnementsdienste aus.  
    Wählen Sie optional **Ändern** aus, um zusätzliche Azure-Konfigurationseinstellungen anzuzeigen und im Abschnitt **Anmeldeinformationen für Datenbankserver** den Benutzernamen zu ermitteln. Speichern Sie den Benutzernamen für zukünftige Schritte in diesem Tutorial. Wenn Sie diesen optionalen Schritt ausführen, schließen Sie den Azure-Konfigurationsbereich, bevor Sie **Fertig** auswählen.
 
1. Wählen Sie **Fertig**aus.  
    Nach wenigen Minuten ist der Vorgang abgeschlossen, und das DevOps Projects-Dashboard wird im Azure-Portal angezeigt. Sie können auch direkt über **Alle Ressourcen** im Azure-Portal zum Dashboard navigieren. Wählen Sie auf der rechten Seite **Durchsuchen** aus, um Ihre ausgeführte Anwendung anzuzeigen.
    
## <a name="examine-the-ci-pipeline"></a>Überprüfen der CI-Pipeline

Mit DevOps Projects wird automatisch eine vollständige CI/CD-Pipeline in Azure Repos konfiguriert. Sie können die Pipeline untersuchen und anpassen. Gehen Sie wie folgt vor, um sich mit der Azure DevOps-Buildpipeline vertraut zu machen:

1. Wählen Sie oben auf dem DevOps Projects-Dashboard die Option **Buildpipelines** aus.  
    Auf einer Browserregisterkarte wird die Buildpipeline für Ihr neues Projekt angezeigt.

1. Zeigen Sie auf das Feld **Status**, und wählen Sie dann die Auslassungspunkte (...) aus.  
    In einem Menü werden verschiedene Optionen angezeigt, etwa zum Einreihen eines neuen Builds in die Warteschlange, zum Anhalten eines Builds und zum Bearbeiten der Buildpipeline.

1. Wählen Sie **Bearbeiten** aus.

1. In diesem Bereich können Sie sich die verschiedenen Aufgaben ansehen, die Sie für Ihre Buildpipeline ausführen können.  
    Vom Build werden verschiedene Aufgaben durchgeführt. Beispielsweise werden Quellen aus dem Git-Repository abgerufen, Abhängigkeiten wiederhergestellt und für Bereitstellungen verwendete Ausgaben veröffentlicht.

1. Wählen Sie oben in der Buildpipeline den Buildpipelinenamen.

1. Ersetzen Sie den Namen Ihrer Buildpipeline durch einen aussagekräftigeren Namen, und wählen Sie **Speichern und in Warteschlange einreihen** und dann **Speichern** aus.

1. Wählen Sie unter dem Buildpipelinenamen **Verlauf** aus.  
    In diesem Bereich wird ein Überwachungsprotokoll mit den letzten Änderungen für den Build angezeigt. An der Buildpipeline vorgenommene Änderungen werden von Azure Pipelines nachverfolgt, sodass Sie verschiedene Versionen vergleichen können.

1. Wählen Sie **Trigger** aus.  
    Mit DevOps Projects wird automatisch ein CI-Trigger erstellt, und mit jedem für das Repository ausgeführten Commit wird ein neuer Build gestartet. Optional können Sie Branches in den CI-Prozess einbeziehen oder davon ausschließen.

1. Wählen Sie **Aufbewahrung** aus.  
    Abhängig vom Szenario können Sie Richtlinien zum Aufbewahren oder Entfernen einer bestimmten Anzahl von Builds festlegen.

## <a name="examine-the-cd-pipeline"></a>Überprüfen der CD-Pipeline

Mit DevOps Projects werden die erforderlichen Schritte zum Bereitstellen über Ihre Azure DevOps-Organisation in Ihrem Azure-Abonnement automatisch erstellt und konfiguriert. Diese Schritte umfassen die Konfiguration einer Azure-Dienstverbindung zur Authentifizierung von Azure DevOps für Ihr Azure-Abonnement. Bei der Automatisierung wird außerdem eine CD-Pipeline erstellt, über die CD für den virtuellen Azure-Computer bereitgestellt wird. Gehen Sie wie folgt vor, um weitere Informationen zur Azure DevOps-CD-Pipeline zu erhalten:

1. Wählen Sie **Build und Release** und anschließend **Releases** aus.  
    DevOps Projects erstellt eine Releasepipeline zum Verwalten von Bereitstellungen in Azure.

1. Wählen Sie neben Ihrer Releasepipeline die Auslassungspunkte (...) und anschließend **Bearbeiten** aus.  
    Die Releasepipeline enthält eine *Pipeline*, die den Releaseprozess definiert.

1. Wählen Sie unter **Artefakte** die Option **Ablegen** aus.  
    Die in den vorherigen Schritten untersuchte Buildpipeline erzeugt die für das Artefakt verwendete Ausgabe. 

1. Wählen Sie rechts neben dem Symbol **Ablegen** die Option **Continuous Deployment-Trigger** aus.  
    Diese Releasepipeline enthält einen aktivierten CD-Trigger. Jedes Mal, wenn ein neues Buildartefakt verfügbar ist, wird von diesem CD-Trigger eine Bereitstellung ausgeführt. Optional können Sie den Trigger deaktivieren, sodass Ihre Bereitstellungen manuell ausgeführt werden müssen. 

    DevOps Projects richtet ein zufälliges SQL-Kennwort ein und verwendet es für die Releasepipeline.
    
1. Wählen Sie auf der linken Seite **Variablen** aus. 

   > [!NOTE]
   > Führen Sie den folgenden Schritt nur aus, wenn Sie das SQL Server-Kennwort geändert haben. Es gibt eine Kennwortvariable.
  
1. Wählen Sie neben dem Feld **Wert** das Schlosssymbol aus, geben Sie das neue Kennwort ein, und wählen Sie dann **Speichern** aus.

1. Wählen Sie auf der linken Seite **Aufgaben** und dann Ihre Umgebung aus.  
    Bei Aufgaben handelt es sich um die Aktivitäten, die beim Bereitstellungsprozess ausgeführt werden. Sie sind in Phasen gruppiert. Diese Releasepipeline enthält eine einzelne Phase, die die Aufgaben *Azure App Service-Bereitstellung* und *Azure SQL-Datenbankbereitstellung* umfasst.

1. Wählen Sie die Aufgabe *Azure SQL ausführen* aus, und sehen Sie sich die verschiedenen Eigenschaften an, die für die SQL-Bereitstellung verwendet werden.  
    Sie sehen unter **Bereitstellungspaket**, dass bei der Aufgabe eine *SQL DACPAC-Datei* verwendet wird.

1. Wählen Sie auf der rechten Seite **Releases anzeigen** aus, um einen Releaseverlauf anzuzeigen.

1. Wählen Sie neben einem Release die Auslassungspunkte (...) und anschließend **Öffnen** aus.  
     Sie können sich verschiedene Menüs ansehen, etwa eine Releasezusammenfassung, zugeordnete Arbeitselemente und Tests.

1. Wählen Sie **Commits** aus.  
     In dieser Ansicht werden die dieser Bereitstellung zugeordneten Codecommits angezeigt. Vergleichen Sie Releases, um die Commitunterschiede zwischen den einzelnen Bereitstellungen anzuzeigen.

1. Wählen Sie **Protokolle** aus.  
     Die Protokolle enthalten nützliche Informationen zum Bereitstellungsprozess. Sie können während und nach Bereitstellungen angezeigt werden.

## <a name="commit-changes-to-azure-repos-and-automatically-deploy-them-to-azure"></a>Committen von Änderungen in Azure Repos und automatisches Bereitstellen dieser Änderungen in Azure 

 > [!NOTE]
 > Mit dem folgenden Verfahren wird die CI/CD-Pipeline mit einer einfachen Textänderung getestet. Optional können Sie das SQL Server-Schema der Tabelle ändern, um den SQL-Bereitstellungsprozess zu testen.

Nun können Sie mithilfe eines CI/CD-Prozesses, mit dem Ihre aktuelle Arbeit auf Ihrer Website automatisch bereitgestellt wird, mit einem Team an Ihrer App zusammenarbeiten. Mit jeder Änderung am GitHub-Repository wird in Azure DevOps ein Build gestartet, und durch eine CD-Pipeline wird eine Bereitstellung in Azure ausgeführt. Führen Sie die Schritte in diesem Abschnitt aus, oder nutzen Sie eine andere Methode zum Committen Ihrer Änderungen in Ihrem Repository. Mit den Codeänderungen wird der CI/CD-Prozess initiiert, und die Änderungen werden automatisch in Azure bereitgestellt.

1. Wählen Sie im linken Bereich **Code** aus, und navigieren Sie anschließend zu Ihrem Repository.

1. Navigieren Sie zum Verzeichnis *SampleWebApplication\Views\Home*. Wählen Sie anschließend neben der Datei *Index.cshtml* die Auslassungspunkte und dann **Bearbeiten** aus. 

1. Nehmen Sie eine Änderung an der Datei vor. Fügen Sie beispielsweise in einem der div-Tags Text hinzu. 

1. Wählen Sie oben rechts **Committen** und dann erneut **Committen** aus, um Ihre Änderung per Push zu übertragen.  
    Kurz danach wird in Azure DevOps ein Build gestartet und zur Bereitstellung der Änderungen ein Release ausgeführt. Überwachen Sie den Buildstatus auf dem DevOps Projects-Dashboard oder im Browser mit Ihrer Azure DevOps-Organisation.

1. Aktualisieren Sie nach Abschluss des Release Ihre Anwendung, um Ihre Änderungen zu überprüfen.

## <a name="connect-to-the-azure-sql-database"></a>Herstellen einer Verbindung mit Azure SQL-Datenbank

Zum Herstellen einer Verbindung mit der Azure SQL-Datenbank benötigen Sie die entsprechenden Berechtigungen.

1. Wählen Sie auf dem DevOps Projects-Dashboard **SQL-Datenbank** aus, um zur Verwaltungsseite für die SQL-Datenbank zu navigieren.
   
1. Wählen Sie **Serverfirewall festlegen** und anschließend **Client-IP-Adresse hinzufügen** aus. 

1. Wählen Sie **Speichern** aus.  
    Ihre Client-IP-Adresse hat nun Zugriff auf die Azure SQL Server-Ressource.

1. Navigieren Sie zurück zum Bereich **SQL-Datenbank**. 

1. Wählen Sie rechts den Servernamen aus, um zur Konfigurationsseite für **SQL Server** zu navigieren.

1. Wählen Sie **Kennwort zurücksetzen** aus, geben Sie für die SQL Server-Administratoranmeldung ein Kennwort ein, und wählen Sie anschließend **Speichern** aus.  
    Notieren Sie sich dieses Kennwort zur späteren Verwendung in diesem Tutorial.

    Danach können Sie optional Clienttools wie SQL Server Management Studio oder Visual Studio verwenden, um eine Verbindung mit SQL Server und der Azure SQL-Datenbank herzustellen. Verwenden Sie zum Herstellen der Verbindung die Eigenschaft **Servername**.

    Wenn Sie den Datenbank-Benutzernamen bei der Erstkonfiguration des Projekts in DevOps Projects nicht geändert haben, ist Ihr Benutzername der lokale Teil Ihrer E-Mail-Adresse. Wenn Ihre E-Mail-Adresse beispielsweise *johndoe\@microsoft.com* lautet, ist *johndoe* Ihr Benutzername.

   > [!NOTE]
   > Wenn Sie Ihr Kennwort für die SQL-Anmeldung ändern, müssen Sie das Kennwort in der Variablen der Releasepipeline ändern. Eine entsprechende Anleitung finden Sie im Abschnitt „Überprüfen der CD-Pipeline“.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Beim Durchführen von Tests können Sie Gebühren vermeiden, indem Sie die Ressourcen bereinigen. Sie können die in diesem Tutorial erstellte Azure SQL-Datenbank und zugehörige Ressourcen löschen, wenn Sie sie nicht mehr benötigen. Verwenden Sie dazu die Funktion **Löschen** auf dem DevOps Projects-Dashboard.

> [!IMPORTANT]
> Mit dem folgenden Verfahren werden Ressourcen endgültig gelöscht. Mit der Funktion *Löschen* werden die Daten, die vom Projekt in DevOps Projects erstellt wurden, in Azure und Azure DevOps gelöscht. Diese Daten können anschließend nicht wiederhergestellt werden. Verwenden Sie dieses Verfahren nur, nachdem Sie die Anweisungen sorgfältig gelesen haben.

1. Navigieren Sie im Azure-Portal zum DevOps Projects-Dashboard.
2. Wählen Sie oben rechts **Löschen** aus. 
3. Wählen Sie an der Eingabeaufforderung **Ja** aus, um die Ressourcen *endgültig zu löschen*.

## <a name="next-steps"></a>Nächste Schritte

Diese Build- und Releasepipelines können Sie optional an die Anforderungen Ihres Teams anpassen. Sie können dieses CI/CD-Muster auch als Vorlage für Ihre anderen Pipelines verwenden. In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Tutorial: Bereitstellen einer ASP.NET-App und von Azure SQL-Datenbank-Code mithilfe von Azure DevOps Projects
> * Konfigurieren von Azure DevOps und eines Azure-Abonnements 
> * Überprüfen der CI-Pipeline
> * Überprüfen der CD-Pipeline
> * Committen von Änderungen in Azure Repos und automatisches Bereitstellen dieser Änderungen in Azure
> * Herstellen einer Verbindung mit Azure SQL-Datenbank 
> * Bereinigen von Ressourcen

Weitere Informationen zur CI/CD-Pipeline finden Sie in folgendem Artikel:

> [!div class="nextstepaction"]
> [Define your multi-stage continuous deployment (CD) pipeline](https://docs.microsoft.com/azure/devops/pipelines/release/define-multistage-release-process?view=vsts) (Festlegen Ihrer mehrstufigen CD-Pipeline (Continuous Deployment))

## <a name="videos"></a>Videos

> [!VIDEO https://channel9.msdn.com/Events/Build/2018/BRK3308/player]
