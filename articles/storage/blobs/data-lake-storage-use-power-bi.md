---
title: Analysieren von Daten in Azure Data Lake Storage Gen2 mit Power BI | Microsoft-Dokumentation
description: Verwenden von Power BI zum Analysieren von in Azure Data Lake Storage Gen2 gespeicherten Daten
author: normesta
ms.service: storage
ms.subservice: data-lake-storage-gen2
ms.topic: conceptual
ms.date: 07/18/2019
ms.author: normesta
ms.reviewer: bensack
ms.openlocfilehash: cb82d47be11570baf7865468c804162a20a7685f
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/08/2019
ms.locfileid: "68855466"
---
# <a name="analyze-data-in-azure-data-lake-storage-gen2-by-using-power-bi"></a>Analysieren von Daten in Azure Data Lake Storage Gen2 mit Power BI

In diesem Artikel wird beschrieben, wie Sie Power BI Desktop zum Analysieren und Visualisieren von Daten verwenden, die in einem Speicherkonto mit einem hierarchischen Namespace (Azure Data Lake Storage Gen2) gespeichert sind.

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie mit diesem Tutorial beginnen können, benötigen Sie Folgendes:

> [!div class="checklist"]
> * Ein Azure-Abonnement. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).
> * Ein Speicherkonto, das über einen hierarchischen Namespace verfügt. Befolgen Sie [diese Anleitung](data-lake-storage-quickstart-create-account.md) für die Erstellung.
> In diesem Artikel wird vorausgesetzt, dass Sie ein Konto mit dem Namen `myadlsg2` erstellt haben.
> * Eine Beispieldatendatei mit dem Namen `Drivers.txt` in Ihrem Speicherkonto.
> Sie können dieses Beispiel aus dem [Azure Data Lake-Git-Repository](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceDataDrivers.txt) herunterladen und die Datei dann in Ihr Speicherkonto hochladen.
> * **Power BI Desktop**. Sie können das Tool aus dem [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=45331) herunterladen. 

## <a name="create-a-report-in-power-bi-desktop"></a>Erstellen eines Berichts in Power BI Desktop

1. Starten Sie Power BI Desktop auf Ihrem Computer.
2. Klicken Sie auf dem Menüband auf der Registerkarte **Start** auf **Daten abrufen** und dann auf **Mehr**.
3. Klicken Sie im Dialogfeld **Daten abrufen** auf **Azure**. Klicken Sie auf **Azure Data Lake Store Gen2 (Beta)** und dann auf **Verbinden**.

    ![Seite „Daten abrufen“](media/data-lake-storage-use-power-bi/get-data-page.png)

4. Im Dialogfeld **Azure Data Lake Storage Gen2** können Sie die URL zu Ihrem Azure Data Lake Storage Gen2-Konto, Dateisystem oder Unterordner angeben, indem Sie das Dateisystem-Endpunktformat verwenden. URLs für Data Lake Storage Gen2 verfügen über das folgende Muster: `https://<accountname>.dfs.core.windows.net/<filesystemname>/<subfolder>`. Klicken Sie anschließend auf **OK**.

    ![URL](media/data-lake-storage-use-power-bi/adls-url.png)

5. Klicken Sie im nächsten Dialogfeld auf **Anmelden**, um sich an Ihrem Speicherkonto anzumelden. Sie werden zur Anmeldeseite Ihrer Organisation umgeleitet. Folgen Sie den Anweisungen, um sich beim Konto anzumelden.

    ![Anmeldeseite](media/data-lake-storage-use-power-bi/sign-in.png)

6. Klicken Sie nach der erfolgreichen Anmeldung auf **Verbinden**.

    ![Seite „Angemeldet“](media/data-lake-storage-use-power-bi/signed-in.png)

7. Im nächsten Dialogfeld werden alle Dateien unter der URL angezeigt, die Sie oben in Schritt 4 angegeben haben (einschließlich der Datei, die Sie in Ihr Speicherkonto hochgeladen haben). Überprüfen Sie die Informationen, und klicken Sie dann auf **Laden**.

    ![Dateisysteme](media/data-lake-storage-use-power-bi/file-systems.png)

8. Nachdem die Daten in Power BI erfolgreich geladen wurden, werden auf der Registerkarte **Felder** die unten angegebenen Felder angezeigt.

    ![Registerkarte „Felder“](media/data-lake-storage-use-power-bi/fields.png)

    Um die Daten visualisieren und analysieren zu können, sollten sie aber über die unten angegebenen Felder verfügbar sein.

    ![Felder](media/data-lake-storage-use-power-bi/preferred-fields.png)

    In den nächsten Schritten aktualisieren wir die Abfrage, sodass die importierten Daten in das gewünschte Format konvertiert werden.

9. Klicken Sie auf dem Menüband unter der Registerkarte **Start** auf **Abfragen bearbeiten**.

    ![Abfragen](media/data-lake-storage-use-power-bi/queries.png)

10. Klicken Sie im **Abfrage-Editor** unter der Spalte **Inhalt** auf **Binär**. Die Datei wird automatisch als CSV-Datei erkannt, und es sollte in etwa die folgende Ausgabe angezeigt werden. Ihre Daten stehen jetzt in einem Format zur Verfügung, das Sie zum Erstellen von Visualisierungen verwenden können.

    ![Output](media/data-lake-storage-use-power-bi/binary.png)

11. Klicken Sie auf dem Menüband auf der Registerkarte **Start** auf **Schließen** und **Übernehmen** und dann erneut auf **Schließen** und **Übernehmen**.

    ![Schließen und übernehmen](media/data-lake-storage-use-power-bi/close-apply.png)

12. Sobald die Abfrage aktualisiert wurde, zeigt die Registerkarte **Felder** die neuen Felder, die zur Visualisierung verfügbar sind.

    ![Neue Felder](media/data-lake-storage-use-power-bi/new-fields.png)

13. Erstellen wir nun ein Kreisdiagramm, um für ein bestimmtes Land die Fahrer in jeder Stadt darzustellen. Wählen Sie dazu folgende Optionen aus.

    Klicken Sie auf der Registerkarte **Visualisierungen** auf das Symbol für ein Kreisdiagramm.

    ![Visualisierungen](media/data-lake-storage-use-power-bi/visualizations.png)

    Wir verwenden folgende Spalten: Spalte 4 (Name der Stadt) und Spalte 7 (Name des Lands). Ziehen Sie diese Spalten aus der Registerkarte **Felder** auf die Registerkarte **Visualisierungen**, wie unten gezeigt.

    ![Ziehen von Feldern](media/data-lake-storage-use-power-bi/visualizations-drag-fields.png)

    Das Kreisdiagramm sollte nun wie das unten gezeigte aussehen.

    ![Kreisdiagramm](media/data-lake-storage-use-power-bi/pie-chart.png)

14. Indem Sie aus den Filtern auf Seitenebene ein bestimmtes Land auswählen, können Sie die Anzahl von Fahrern in jeder Stadt des ausgewählten Landes anzeigen. Wählen Sie z.B. auf der Registerkarte **Visualisierungen** unter **Filter auf Seitenebene** als Land **Brasilien** aus.

    ![Seitenfilter](media/data-lake-storage-use-power-bi/page-filters.png)

15. Das Kreisdiagramm wird automatisch aktualisiert und zeigt die Fahrer in den brasilianischen Städten an.

    ![Brasilien](media/data-lake-storage-use-power-bi/pie-chart-updated.png)

16. Klicken Sie im Menü **Datei** auf **Speichern**, um die Visualisierung als Power BI Desktop-Datei zu speichern.

## <a name="publish-report-to-power-bi-service"></a>Veröffentlichen des Berichts im Power BI-Dienst

Nachdem Sie die Visualisierungen in Power BI Desktop erstellt haben, können Sie diese für andere Personen freigeben, indem Sie sie im Power BI-Dienst veröffentlichen. Ausführliche Anweisungen hierzu finden Sie unter [Veröffentlichen aus Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-upload-desktop-files/).