---
title: Erneutes Bereitstellen von SQL Server Integration Services-Paketen für Azure SQL-Datenbank | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie SQL Server Integration Services-Pakete für Azure SQL-Datenbank migrieren.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 06/08/2019
ms.openlocfilehash: 603a9df8e3f499c832bbfdcbef966de86003d6b7
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "67080635"
---
# <a name="redeploy-sql-server-integration-services-packages-to-azure-sql-database"></a>Erneutes Bereitstellen von SQL Server Integration Services-Paketen für Azure SQL-Datenbank

Wenn Sie SQL Server Integration Services (SSIS) verwenden und Ihre SSIS-Projekte/-Pakete aus der vom SQL Server gehosteten SSIS-Quelldatenbank in die von Azure SQL-Datenbank gehostete SSIS-Zieldatenbank migrieren möchten, können Sie diese mithilfe des Bereitstellungs-Assistenten für Integration Services erneut bereitstellen. Den Assistenten können Sie direkt in SQL Server Management Studio (SSMS) starten.

Wenn Sie eine ältere Version als SSIS 2012 verwenden, müssen Sie Ihre SSIS-Projekte/-Pakete mithilfe des Bereitstellungs-Assistenten für Integration Services konvertieren, bevor Sie die Projekte/Pakete im Projektbereitstellungsmodell erneut bereitstellen können. Der Bereitstellungs-Assistent kann ebenfalls direkt in SSMS gestartet werden. Weitere Informationen finden Sie im Artikel [Bereitstellen von SQL Server Integration Services-Projekten und Paketen (SSIS)](https://docs.microsoft.com/sql/integration-services/packages/deploy-integration-services-ssis-projects-and-packages?view=sql-server-2017#convert).

> [!NOTE]
> Azure Database Migration Service (DMS) bietet momentan keine Unterstützung für die Migration einer SSIS-Quelldatenbank zu einem Azure SQL-Datenbank-Server. Sie können Ihre SSIS-Projekte/-Pakete jedoch mithilfe des folgenden Prozesses erneut bereitstellen.

In diesem Artikel werden folgende Vorgehensweisen behandelt:
> [!div class="checklist"]
>
> * Bewerten von SSIS-Quellprojekten/-paketen.
> * Migrieren von SSIS-Projekten/-Paketen zu Azure

## <a name="prerequisites"></a>Voraussetzungen

Zum Ausführen dieser Schritte benötigen Sie Folgendes:

* SSMS in der Version 17.2 oder höher.
* Eine Instanz Ihres Datenbankzielservers, um die SSIS-Datenbank zu hosten. Wenn noch keine Instanz vorhanden ist, erstellen Sie einen Azure SQL-Datenbank-Server (ohne eine Datenbank) mithilfe des Azure-Portals, indem Sie zum SQL Server-[Formular](https://ms.portal.azure.com/#create/Microsoft.SQLServer) (nur logischer Server) navigieren.
* SSIS muss in Azure Data Factory (ADF) bereitgestellt werden. Dieser Dienst beinhaltet Azure-SSIS Integration Runtime (IR), wobei die SSIS-Zieldatenbank von der Instanz des Azure SQL-Datenbank-Servers gehostet wird (wie im Artikel [Bereitstellen der Azure-SSIS Integration Runtime in Azure Data Factory](https://docs.microsoft.com/azure/data-factory/tutorial-deploy-ssis-packages-azure) beschrieben).

## <a name="assess-source-ssis-projectspackages"></a>Bewerten von SSIS-Quellprojekten/-paketen

Während das Bewerten von SSIS-Quelldatenbanken momentan weder im Datenmigrations-Assistenten (DMA) noch im Azure Database Migration Service (DMS) integriert ist, werden Ihre SSIS-Projekte/-Pakete bewertet/überprüft, wenn Sie in der von einem Azure SQL-Datenbank-Server gehosteten SSIS-Zieldatenbank erneut bereitgestellt werden.

## <a name="migrate-ssis-projectspackages"></a>Migrieren von SSIS-Projekten/-Paketen

Wenn Sie SSIS-Projekte/-Pakete zum Azure SQL-Datenbank-Server migrieren möchten, führen Sie die folgenden Schritte aus.

1. Öffnen Sie SSMS, und wählen Sie dann **Optionen** aus, um das Dialogfeld **Mit Server verbinden** anzuzeigen.

2. Geben Sie auf der Registerkarte **Anmeldung** die Informationen an, die nötig sind, um eine Verbindung zum Azure SQL-Datenbank-Server herzustellen, der die SSIS Zieldatenbank hosten soll.

    ![SSIS-Tab „Login“](media/how-to-migrate-ssis-packages/dms-ssis-login-tab.png)

3. Wählen Sie auf dem Tab **Verbindungseigenschaften** im Textfeld **Mit Datenbank verbinden** **SSISDB** aus oder geben Sie dies ein, und klicken Sie dann auf **Verbinden**.

    ![SSIS-Tab „Verbindungseigenschaften“](media/how-to-migrate-ssis-packages/dms-ssis-conncetion-properties-tab.png)

4. Erweitern Sie im SSMS-Objekt-Explorer den Knoten **Integration Services-Kataloge**, erweitern Sie dann **SSISDB**, und klicken Sie dann – sofern es keine bereits vorhandenen Ordner gibt – mit der rechten Maustaste auf **SSISDB**, und erstellen Sie einen neuen Ordner.

5. Erweitern Sie unter **SSISDB** einen beliebigen Ordner, klicken Sie mit der rechten Maustaste auf **Projekte**, und wählen Sie dann **Projekt bereitstellen** aus.

    ![Erweiterter SSISDB-Knoten in SSIS](media/how-to-migrate-ssis-packages/dms-ssis-ssisdb-node-expanded.png)

6. Lesen Sie sich die Informationen auf der Seite **Einführung** im Bereitstellungs-Assistent für Integration Services durch, und klicken Sie dann auf **Weiter**.

    ![Seite „Einführung“ im Bereitstellungs-Assistenten](media/how-to-migrate-ssis-packages/dms-deployment-wizard-introduction-page.png)

7. Bestimmen Sie auf der Seite **Quelle auswählen** das vorhandene SSIS-Projekt, das Sie bereitstellen möchten.

    Wenn SSMS auch mit dem SQL Server verbunden ist, der die SSIS Quelldatenbank hostet, wählen Sie **Integration Services-Katalog** aus, und geben Sie dann in Ihrem Katalog den Servernamen und den Projektpfad ein, um Ihr Projekt direkt bereitzustellen.

    Alternativ können Sie auch die **Projektbereitstellungsdatei** auswählen und dann den Pfad zu einer vorhandenen Projektbereitstellungsdatei (.ispac) festlegen, um Ihr Projekt bereitzustellen.

    ![Seite „Quelle auswählen“ im Bereitstellungs-Assistenten](media/how-to-migrate-ssis-packages/dms-deployment-wizard-select-source-page.png)
 
8. Klicken Sie auf **Weiter**.
9. Geben Sie auf der Seite **Ziel auswählen** das Ziel für Ihr Projekt an.

    a. Geben Sie im Textfeld „Servername“ den vollqualifizierten Azure SQL-Datenbank-Servernamen ein (<Servername>.database.windows.net).

    b. Geben Sie die Authentifizierungsinformationen ein, und klicken Sie dann auf **Verbinden**.

    ![Seite „Ziel auswählen“ im Bereitstellungs-Assistenten](media/how-to-migrate-ssis-packages/dms-deployment-wizard-select-destination-page.png)

    c. Klicken Sie auf **Durchsuchen**, um den Zielordner in der SSIS-Datenbank zu bestimmen, und klicken Sie dann auf **Weiter**.

    > [!NOTE]
    > Die Schaltfläche **Weiter** wird erst aktiviert, wenn Sie zuvor auf **Verbinden** geklickt haben.

10. Sehen Sie sich auf der Seite **Review** (Überprüfen) alle Fehler/Warnungen an, und passen Sie Ihre Pakete ggf. entsprechend an.

    ![Seite „Review“ (Überprüfen) im Bereitstellungs-Assistenten](media/how-to-migrate-ssis-packages/dms-deployment-wizard-validate-page.png)

11. Klicken Sie auf **Weiter**.

12. Sehen Sie sich auf der Seite **Review** Ihre Bereitstellungseinstellungen noch einmal an.

    > [!NOTE]
    > Sie können die Einstellungen ändern, indem Sie entweder auf **Zurück** klicken, oder indem Sie im linken Bereich einen der Links zurück zu den einzelnen Schritten auswählen.

13. Klicken Sie auf **Bereitstellen**, um den Bereitstellungsprozess zu starten.

14. Wenn der Bereitstellungsprozess beendet ist, können Sie sich die Ergebnisseite ansehen, auf der für jede einzelne Bereitstellungsaktion angezeigt wird, ob diese erfolgreich war oder fehlgeschlagen ist.
    a. Sollte eine Aktion fehlgeschlagen sein, klicken Sie in der Spalte **Ergebnisse** auf **Fehlgeschlagen**, um eine Erklärung des Fehlers anzuzeigen.
    b. Optional können Sie auf **Bericht speichern** klicken, um die Ergebnisse in einer XML-Datei zu speichern.

15. Klicken Sie abschließend auf **Schließen**, um den Bereitstellungs-Assistenten für Integration Services zu verlassen.

Wenn die Bereitstellung Ihres Projekts problemlos durchgeführt werden konnte, können Sie alle im Projekt enthaltenen Pakete auswählen, um diese auf Ihrer Azure-SSIS IR auszuführen.

## <a name="next-steps"></a>Nächste Schritte

* Überprüfen der Migrationsanleitung im [Leitfaden zur Datenbankmigration von Microsoft](https://datamigration.microsoft.com/)
