---
title: Ausführen von SSIS-Paketen in Azure Data Factory über SSDT
description: Erfahren Sie, wie Sie über SSDT SSIS-Pakete in Azure ausführen.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 07/31/2019
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: 4c89bdddce7b7318e184994ddf627d853e29fd7e
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73673600"
---
# <a name="execute-ssis-packages-in-azure-from-ssdt"></a>Ausführen von SSIS-Paketen in Azure über SSDT
Dieser Artikel beschreibt das Feature für Azure-fähige SQL Server Integration Services-Projekte (SSIS) in SQL Server Data Tools (SSDT), mit dem Sie Pakete in der Azure-SSIS Integration Runtime (IR) in Azure Data Factory (ADF) ausführen können.  Mit diesem Feature können Sie vorhandene SSIS-Pakete vor einer Lift & Shift-Migration zu Azure testen oder neue SSIS-Pakete zur Ausführung in Azure entwickeln.

Sie können mithilfe dieses Features eine neue Azure-SSIS IR erstellen oder eine vorhandene IR an SSIS-Projekte anfügen und dann Ihr Pakete ausführen.  Unterstützt wird die Ausführung von Paketen, die im SSIS-Katalog (SSISDB) im Projektbereitstellungsmodell bereitgestellt werden, und von Paketen, die in Dateisystemen/Dateifreigaben/Azure Files im Paketbereitstellungsmodell bereitgestellt werden. 

## <a name="prerequisites"></a>Voraussetzungen
Um dieses Feature zu verwenden, laden Sie [hier](https://marketplace.visualstudio.com/items?itemName=SSIS.SqlServerIntegrationServicesProjects) die aktuelle Version der SSDT mit SSIS-Projekterweiterung für Visual Studio oder [hier](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt?view=sql-server-2017#ssdt-for-vs-2017-standalone-installer) einen eigenständigen Installer herunter. Installieren Sie anschließend SSDT.

## <a name="azure-enable-ssis-projects"></a>Azure-fähige SSIS-Projekte
In SSDT können Sie mithilfe der Vorlage **Integration Services-Projekt (Azure-fähig)** neue, Azure-fähige SSIS-Projekte erstellen.

![Neues, Azure-fähiges SSIS-Projekt](media/how-to-invoke-ssis-package-ssdt/ssdt-azure-enabled-new-project.png)

Alternativ können Sie Ihre vorhandenen SSIS-Projekte für Azure aktivieren, indem Sie im Projektmappen-Explorer-Panel von SSDT mit der rechten Maustaste auf den Projektknoten klicken. Wählen Sie im daraufhin angezeigten Popupmenü die Option **Für Azure geeignet** aus.

![Vorhandenes SSIS-Projekt für Azure aktivieren](media/how-to-invoke-ssis-package-ssdt/ssdt-azure-enabled-existing-project.png)

Um Ihre vorhandenen SSIS-Projekte für Azure zu aktivieren, müssen Sie die Zielserverversion auf die neueste von Azure-SSIS IR unterstützte Version festlegen. Dies ist aktuell **SQL Server 2017**. Sofern dies nicht bereits geschehen ist, wird ein Dialogfenster mit einer entsprechenden Aufforderung geöffnet.

## <a name="connect-azure-enabled-projects-to-ssis-in-azure-data-factory"></a>Verbinden von Azure-fähigen Projekte mit SSIS in Azure Data Factory
Durch die Verbindungsherstellung Ihrer Azure-fähigen Projekte mit SSIS in ADF können Sie Ihre Pakete in Azure Files hochladen und in Azure-SSIS IR ausführen.  Führen Sie dazu diese Schritte aus:

1. Klicken Sie mit der rechten Maustaste auf das Projekt oder den Knoten **Verknüpfte Azure-Ressourcen** im Projektmappen-Explorer-Panel von SSDT, um ein Popupmenü anzuzeigen. Wählen Sie dann **Mit SSIS in Azure Data Factory verbinden** aus, um den **Assistenten zur Verbindungsherstellung mit SSIS in ADF** zu starten.

   ![Verbindungsherstellung mit SSIS in ADF](media/how-to-invoke-ssis-package-ssdt/ssdt-azure-enabled-existing-project2.png)

2. Lesen Sie auf der Seite **SSIS in ADF – Einführung** die bereitgestellten Informationen, und klicken Sie auf **Weiter**, um den Vorgang fortzusetzen.

   ![SSIS in ADF – Einführung](media/how-to-invoke-ssis-package-ssdt/ssis-in-adf-connection-wizard.png)

3. Wählen Sie auf der Seite **SSIS IR in ADF auswählen** Ihre vorhandene ADF-Instanz und Ihre Azure-SSIS IR zur Ausführung von Paketen aus, oder erstellen Sie eine neue Instanz/IR (sofern nicht bereits geschehen).
   - Um Ihre vorhandene Azure-SSIS IR auszuwählen, wählen Sie zunächst das relevante Azure-Abonnement und die ADF-Instanz aus.
   - Wenn Sie eine vorhandene ADF-Instanz auswählen, die über keine Azure-SSIS IR verfügt, klicken Sie auf die Schaltfläche **SSIS IR erstellen**, um eine neue IR im ADF-Portal bzw. der App zu erstellen.
   - Wenn Sie ein vorhandenes Azure-Abonnement auswählen, das keine ADF-Instanz umfasst, klicken Sie auf die Schaltfläche **SSIS IR erstellen**, um den **Assistenten zum Erstellen einer Integration Runtime** zu starten. In diesem Assistenten können Sie den Standort und ein Präfix für die automatische Erstellung einer neuen Azure-Ressourcengruppe, Data Factory-Instanz und SSIS IR in Ihrem Namen eingeben. Das Namensmuster lautet wie folgt: **IhrPräfix-RG/DF/IR-ZeitpunktDerErstellung**.
   
   ![SSIS IR in ADF auswählen](media/how-to-invoke-ssis-package-ssdt/ssis-in-adf-connection-wizard2.png)

4. Wählen Sie auf der Seite **Azure Storage-Instanz auswählen** Ihr vorhandenes Azure Storage-Konto zum Hochladen von Paketen in Azure Files aus, oder erstellen Sie ein neues Konto (sofern nicht bereits geschehen).
   - Um Ihr vorhandenes Azure Storage-Konto auszuwählen, wählen Sie zunächst das relevante Azure-Abonnement aus.
   - Wenn Sie dasselbe Azure-Abonnement auswählen wie für Ihre Azure-SSIS IR ohne Azure Storage-Konto, klicken Sie auf die Schaltfläche **Azure Storage-Instanz erstellen**, damit in Ihrem Namen automatisch ein Azure Storage-Konto am selben Standort erstellt wird wie Ihre Azure-SSIS IR. Hierbei wird zur Benennung ein Namenspräfix Ihrer SSIS IR mit dem Zeitpunkt der Erstellung kombiniert.
   - Wenn Sie ein anderes Azure-Abonnement auswählen, das kein Azure Storage-Konto umfasst, klicken Sie auf die Schaltfläche **Azure Storage-Instanz erstellen**, um ein neues Konto im Azure-Portal zu erstellen.
   
   ![Auswählen von Azure Storage](media/how-to-invoke-ssis-package-ssdt/ssis-in-adf-connection-wizard3.png)

5. Klicken Sie auf die Schaltfläche **Verbinden**, um Ihre Verbindung herzustellen.  Die ausgewählte Azure-SSIS IR und das Azure Storage-Konto werden unterhalb des Knotens **Verknüpfte Azure-Ressourcen** im Projektmappen-Explorer-Panel von SSDT angezeigt.  Außerdem wird der Status Ihrer Azure-SSIS IR aktualisiert. Sie können den Status der IR verwalten, indem Sie mit der rechten Maustaste auf den zugehörigen Knoten klicken, um ein Popupmenü zu öffnen. In diesem Menü können Sie das Menüelement **Starten\Beenden\Verwalten** auswählen, um zur Ausführung der jeweiligen Aufgabe zum ADF-Portal bzw. zur App zu wechseln.

## <a name="execute-ssis-packages-in-azure"></a>Ausführen von SSIS-Paketen in Azure
### <a name="starting-package-executions"></a>Starten von Paketausführungen
Nachdem Sie Ihre Projekte mit SSIS in ADF verbunden haben, können Sie Pakete in Azure-SSIS IR ausführen.  Sie haben zwei Möglichkeiten, um Paketausführungen zu starten:
-  Klicken Sie auf die Schaltfläche **Starten** in der SSDT-Symbolleiste, und wählen Sie im daraufhin angezeigten Dropdownmenü die Option **In Azure ausführen** aus. 

   ![In Azure ausführen](media/how-to-invoke-ssis-package-ssdt/ssdt-azure-enabled-execute-package.png)

-  Klicken Sie mit der rechten Maustaste im Projektmappen-Explorer-Panel von SSDT auf den Paketknoten, und wählen Sie im daraufhin angezeigten Popupmenü die Option **Paket in Azure ausführen** aus.

   ![Paket in Azure ausführen](media/how-to-invoke-ssis-package-ssdt/ssdt-azure-enabled-execute-package2.png)

> [!NOTE]
> Um Ihre Pakete in Azure ausführen zu können, müssen Sie über eine ausgeführte Azure-SSIS IR verfügen. Wenn Ihre Azure-SSIS IR angehalten wurde, wird ein Dialogfeld für ihren Start geöffnet.  Abgesehen von einer benutzerdefinierten Einrichtungszeit sollte dieser Prozess innerhalb von 5 Minuten abgeschlossen sein. Es kann aber etwa 20 bis 30 Minuten dauern, bis Azure-SSIS IR einem virtuellen Netzwerk beitritt.  Nach dem Ausführen Ihrer Pakete in Azure können Sie Ihre Azure-SSIS IR beenden, um Ausführungskosten einzusparen. Klicken Sie hierzu mit der rechten Maustaste auf den zugehörigen Knoten im Projektmappen-Explorer-Panel von SSDT, um ein Popupmenü zu öffnen. Wählen Sie dann im Popupmenü die Option **Starten\Beenden\Verwalten** aus, um zur Ausführung der jeweiligen Aufgabe zum ADF-Portal bzw. zur App zu wechseln.

### <a name="checking-package-execution-logs"></a>Überprüfen der Protokolle zur Paketausführung
Wenn Sie die Paketausführung starten, wird das zugehörige Protokoll formatiert und im Fortschrittsfenster von SSDT angezeigt.  Bei Paketen mit langer Ausführungsdauer wird das Protokoll in regelmäßigen Abständen aktualisiert.  Sie können die Paketausführung beenden, indem Sie auf die Schaltfläche **Beenden** in der Symbolleiste von SSDT klicken. Der Vorgang wird sofort abgebrochen.  Die unformatierten Protokolldateien finden Sie im zugehörigen UNC-Pfad (Universal Naming Convention): `\\<YourConnectedAzureStorage>.file.core.windows.net\ssdtexecution\<YourProjectName-FirstConnectTime>\<YourPackageName-tmp-ExecutionTime>\logs`, aber diese Daten werden nach einem Tag bereinigt.

### <a name="switching-package-protection-level"></a>Wechsel der Schutzebene für Pakete
Für die Ausführung von SSIS-Paketen in Azure werden die Schutzebenen **EncryptSensitiveWithUserKey**/**EncryptAllWithUserKey** nicht unterstützt.  Wenn Ihre Pakete also mit diesen Schutzebenen konfiguriert sind, wird die Schutzebene temporär auf **EncryptSensitiveWithPassword**/**EncryptAllWithPassword** mit zufällig generierten Kennwörtern umgestellt, wenn Ihre Pakete zur Ausführung in Ihrer Azure-SSIS IR in Azure Files hochgeladen werden.

> [!NOTE]
> Wenn Ihre Pakete Tasks zur Paketausführung enthalten, die auf andere, mit den Schutzebenen **EncryptSensitiveWithUserKey**/**EncryptAllWithUserKey** konfigurierte Pakete verweisen, müssen Sie diese manuell zur Verwendung von **EncryptSensitiveWithPassword**/**EncryptAllWithPassword** rekonfigurieren, bevor Sie die Pakete ausführen.

Wenn Ihre Pakete bereits mit den Schutzebenen **EncryptSensitiveWithPassword**/**EncryptAllWithPassword** konfiguriert sind, bleiben sie unverändert. Es werden aber dennoch zufällig generierte Kennwörter verwendet, wenn Ihre Pakete zur Ausführung in Ihrer Azure-SSIS IR in Azure Files hochgeladen werden.

### <a name="using-package-configuration-file"></a>Verwenden einer Paketkonfigurationsdatei
Wenn Sie im Paketbereitstellungsmodell Paketkonfigurationsdateien verwenden, um zur Laufzeit Variablenwerte zu ändern, werden diese Dateien automatisch in Azure Files hochgeladen, um in Ihrer Azure-SSIS IR ausgeführt zu werden.

### <a name="using-execute-package-task"></a>Verwenden eines Tasks „Paket ausführen“
Wenn Ihre Pakete Tasks zur Paketausführung enthalten, die auf andere, in lokalen Dateisystemen gespeicherte Pakete verweisen, müssen Sie die folgenden zusätzlichen Einrichtungsschritte ausführen:

1. Laden Sie die anderen Pakete mit demselben Azure Storage-Konto in Azure Files hoch wie demjenigen zur Verbindung Ihrer Projekte, und rufen Sie den neuen UNC-Pfad ab, z.B. `\\test.file.core.windows.net\ssdtexecution\Package1.dtsx`.

2. Ersetzen Sie den Dateipfad dieser anderen Pakete im Dateiverbindungs-Manager der Tasks „Paket ausführen“ durch den neuen UNC-Pfad.
   - Wenn Ihr Computer zur Ausführung von SSDT den neuen UNC-Pfad nicht verarbeiten kann, können Sie den Dateipfad im Panel „Eigenschaften“ des Dateiverbindungs-Managers ändern.
   - Alternativ können Sie eine Variable für den Dateipfad verwenden, um zur Laufzeit den richtigen Wert zuzuweisen.

Wenn Ihre Pakete Tasks zur Paketausführung enthalten, die auf andere Pakete im selben Projekt verweisen, ist keine zusätzliche Einrichtung erforderlich.

## <a name="next-steps"></a>Nächste Schritte
Wenn Sie die Ausführung Ihrer Pakete in Azure aus SSDT erfolgreich abgeschlossen haben, können Sie mithilfe einer Aktivität „SSIS-Paket ausführen“ in ADF-Pipelines bereitstellen und ausführen. Siehe hierzu [Ausführen eines SSIS-Pakets mit der Aktivität „SSIS-Paket ausführen“ in Azure Data Factory](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity).
