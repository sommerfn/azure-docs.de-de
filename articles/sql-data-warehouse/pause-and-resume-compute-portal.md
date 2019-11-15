---
title: 'Schnellstart: Anhalten und Fortsetzen von Computeressourcen – Azure-Portal '
description: Halten Sie Computeressourcen in Azure SQL Data Warehouse mithilfe des Azure-Portals an, um Kosten einzusparen. Setzen Sie die Computeressourcen fort, wenn Sie das Data Warehouse verwenden möchten.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: quickstart
ms.subservice: manage
ms.date: 04/18/2018
ms.author: kevin
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 14f66f71948f75a723c9fdbed7490d54c2c3e2b2
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73692999"
---
# <a name="quickstart-pause-and-resume-compute-for-an-azure-sql-data-warehouse-in-the-azure-portal"></a>Schnellstart: Anhalten und Fortsetzen von Computeressourcen für ein Azure SQL Data Warehouse im Azure-Portal

Halten Sie Computeressourcen in Azure SQL Data Warehouse mithilfe des Azure-Portals an, um Kosten einzusparen. [Setzen Sie die Computeressourcen fort](sql-data-warehouse-manage-compute-overview.md), wenn Sie das Data Warehouse verwenden möchten.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

## <a name="before-you-begin"></a>Voraussetzungen

Verwenden Sie [Erstellen und Verbinden – Portal](create-data-warehouse-portal.md), um ein Data Warehouse namens **mySampleDataWarehouse** zu erstellen. 

## <a name="pause-compute"></a>Anhalten von Computeressourcen

Um Kosten zu sparen, können Sie Computeressourcen bei Bedarf anhalten und fortsetzen. Wenn Sie die Datenbank z.B. nachts oder am Wochenende nicht verwenden, können Sie sie während dieser Zeiträume anhalten und während des Tages wieder fortsetzen. Computeressourcen werden Ihnen nicht in Rechnung gestellt, während die Datenbank angehalten ist. Allerdings wird Ihnen der Speicherplatz weiter in Rechnung gestellt. 

Führen Sie die folgenden Schritte aus, um eine SQL Data Warehouse-Instanz anzuhalten:

1. Klicken Sie auf der linken Seite im Azure-Portal auf **SQL-Datenbanken**.
2. Wählen Sie auf der Seite **SQL-Datenbanken** den Eintrag **mySampleDataWarehouse** aus. Hierdurch wird das Data Warehouse geöffnet. 
3. Beachten Sie, dass auf der Seite **mySampleDataWarehouse** für **Status** der Wert **Online** angezeigt wird.

    ![Computerressourcen online](media/pause-and-resume-compute-portal/compute-online.png)

4. Um das Data Warehouse anzuhalten, klicken Sie auf die Schaltfläche **Anhalten**. 
5. Sie werden in einer Meldung aufgefordert, das Fortsetzen des Vorgangs zu bestätigen. Klicken Sie auf **Ja**.
6. Warten Sie einen Moment, und beachten Sie, dass der **Status** als **Wird angehalten** angezeigt wird.

    ![Status „Wird angehalten“](media/pause-and-resume-compute-portal/pausing.png)

7. Wenn der Anhaltevorgang abgeschlossen wurde, wechselt der Status in den Wert **Angehalten**, und die Bezeichnung der Optionsschaltfläche lautet **Starten**.
8. Die Computeressourcen für das Data Warehouse sind jetzt offline. Ihnen werden erst wieder Computeressourcen in Rechnung gestellt, wenn Sie den Dienst fortsetzen.

    ![Computeressourcen offline](media/pause-and-resume-compute-portal/compute-offline.png)


## <a name="resume-compute"></a>Fortsetzen von Computeressourcen

Führen Sie die folgenden Schritte aus, um eine SQL Data Warehouse-Instanz fortzusetzen:

1. Klicken Sie auf der linken Seite im Azure-Portal auf **SQL-Datenbanken**.
2. Wählen Sie auf der Seite **SQL-Datenbanken** den Eintrag **mySampleDataWarehouse** aus. Hierdurch wird das Data Warehouse geöffnet. 
3. Beachten Sie, dass auf der Seite **mySampleDataWarehouse** für **Status** der Wert **Angehalten** angezeigt wird.

    ![Computeressourcen offline](media/pause-and-resume-compute-portal/compute-offline.png)

4. Klicken Sie auf **Starten**, um das Data Warehouse fortzusetzen. 
5. Sie werden in einer Meldung aufgefordert, den Startvorgang zu bestätigen. Klicken Sie auf **Ja**.
6. Beachten Sie, dass der **Status** als **Wird fortgesetzt** angezeigt wird.

    ![Wird fortgesetzt](media/pause-and-resume-compute-portal/resuming.png)

7. Wenn das Data Warehouse wieder online ist, lautet der Status **Online**, und die Bezeichnung der Optionsschaltfläche lautet **Anhalten**.
8. Die Computeressourcen für das Data Warehouse sind jetzt online, und Sie können den Dienst verwenden. Es fallen wieder Kosten für die Computeressourcen an.

    ![Computerressourcen online](media/pause-and-resume-compute-portal/compute-online.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Ihnen werden Gebühren für Ihre Data Warehouse-Einheiten und die in Ihrem Data Warehouse gespeicherte Daten in Rechnung gestellt. Diese Compute- und Speicherressourcen werden separat in Rechnung gestellt. 

- Wenn Sie die Daten im Speicher beibehalten möchten, halten Sie die Computeressourcen an.
- Wenn zukünftig keine Gebühren anfallen sollen, können Sie das Data Warehouse löschen. 

Führen Sie die folgenden Schritte aus, um Ressourcen nach Wunsch zu bereinigen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und klicken Sie auf Ihr Data Warehouse.

    ![Bereinigen von Ressourcen](media/load-data-from-azure-blob-storage-using-polybase/clean-up-resources.png)

1. Zum Anhalten von Computeressourcen klicken Sie auf die Schaltfläche **Anhalten**. Wenn das Data Warehouse angehalten ist, wird die Schaltfläche **Starten** angezeigt.  Klicken Sie zum Fortsetzen der Computeressourcen auf **Starten**.

2. Wenn Sie das Data Warehouse entfernen möchten, damit keine Gebühren für Compute- oder Speicherressourcen anfallen, klicken Sie auf **Löschen**.

3. Um die erstellte SQL Server-Instanz zu entfernen, klicken Sie auf **mynewserver-20171113.database.windows.net** und anschließend auf **Löschen**.  Seien Sie bei diesem Löschvorgang vorsichtig, da beim Löschen des Servers auch alle Datenbanken gelöscht werden, die dem Server zugewiesen sind.

4. Zum Entfernen der Ressourcengruppe klicken Sie auf **myResourceGroup**, und klicken Sie dann auf **Ressourcengruppe löschen**.


## <a name="next-steps"></a>Nächste Schritte

Sie haben die Computeressourcen für Ihr Data Warehouse angehalten und fortgesetzt. Weitere Informationen zu Azure SQL Data Warehouse erhalten Sie im Tutorial zum Laden von Daten.

> [!div class="nextstepaction"]
> [Laden von Daten in SQL Data Warehouse](load-data-from-azure-blob-storage-using-polybase.md)
