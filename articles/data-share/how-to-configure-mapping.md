---
title: Konfigurieren einer Datasetzuordnung in Azure Data Share
description: Hier erfahren Sie, wie Sie eine Datasetzuordnung für eine empfangene Freigabe mithilfe von Azure Data Share konfigurieren.
author: joannapea
ms.author: joanpo
ms.service: data-share
ms.topic: conceptual
ms.date: 07/10/2019
ms.openlocfilehash: 754977788c5f6e5e574500552f670ba9083cf683
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73490653"
---
# <a name="how-to-configure-a-dataset-mapping-for-a-received-share-in-azure-data-share"></a>Konfigurieren einer Datasetzuordnung für eine empfangene Freigabe in Azure Data Share

In diesem Artikel wird gezeigt, wie eine Datasetzuordnung für eine empfangene Freigabe in Azure Data Share konfiguriert wird. Sie sollten so vorgehen, wenn Sie eine Datenfreigabeeinladung zwar angenommen, sich aber für „Annehmen und später konfigurieren“ entschieden haben. Möglicherweise möchten Sie eine Datasetzuordnung konfigurieren, wenn Sie das Ziel für Daten ändern müssen, die für Sie freigegeben wurden, oder wenn Sie Daten in einer SQL Server-Instanz empfangen möchten. 

## <a name="navigate-to-a-received-data-share"></a>Navigieren zu einer empfangenen Datenfreigabe

Navigieren Sie im Azure Data Share-Dienst zu der empfangenen Freigabe, und wählen Sie die Registerkarte **Details** aus. 

![Datasetzuordnung](./media/dataset-mapping.png "Datasetzuordnung") 

Aktivieren Sie das Kontrollkästchen neben dem Dataset, dem Sie ein Ziel zuweisen möchten. Wählen Sie **Zuordnung aufheben** aus, um die vorhandene Zuordnung aufzuheben. Wählen Sie **+ Dem Ziel zuordnen** aus, um einen neuen Zielspeicher auszuwählen. 

![Dem Ziel zuordnen](./media/dataset-map-target.png "Dem Ziel zuordnen") 

## <a name="select-a-new-destination-store"></a>Auswählen eines neuen Zielspeichers

Wählen Sie einen Zieldatentyp aus, in dem die Daten gespeichert werden sollen. Beachten Sie, dass alle Daten, die bereits in einem zuvor zugeordneten Speicherkonto vorhanden sind, nicht automatisch in das neue Ziel verschoben werden.

![Zielspeicherkonto](./media/dataset-map-target-sql.png "Zielspeicher") 

## <a name="select-a-file-format-sql-sources-only"></a>Auswählen eines Dateiformats (nur SQL-Quellen)

Wenn die Quelldaten aus einer SQL-basierten Quelle stammen, können Sie auswählen, in welchem Format sie empfangen werden. 

![Auswählen des Formats](./media/sql-file-formats.png "SQL-Dateiformate")

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Freigeben von Daten finden Sie im Tutorial zum [Freigeben Ihrer Daten](share-your-data.md).



