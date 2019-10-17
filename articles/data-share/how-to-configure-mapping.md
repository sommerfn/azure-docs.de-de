---
title: Konfigurieren einer Datasetzuordnung in Azure Data Share (Vorschau)
description: Hier erfahren Sie, wie Sie eine Datasetzuordnung für eine empfangene Freigabe mithilfe von Azure Data Share (Vorschau) konfigurieren.
author: joannapea
ms.author: joanpo
ms.service: data-share
ms.topic: conceptual
ms.date: 07/10/2019
ms.openlocfilehash: 035235116240bdc6de3bc689c2430fee018b202d
ms.sourcegitcommit: aef6040b1321881a7eb21348b4fd5cd6a5a1e8d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2019
ms.locfileid: "72169133"
---
# <a name="how-to-configure-a-dataset-mapping-for-a-received-share-in-azure-data-share-preview"></a>Konfigurieren einer Datasetzuordnung für eine empfangene Freigabe in Azure Data Share (Vorschau)

In diesem Artikel wird gezeigt, wie eine Datasetzuordnung für eine empfangene Freigabe in Azure Data Share (Vorschau) konfiguriert wird. Sie sollten so vorgehen, wenn Sie eine Datenfreigabeeinladung zwar angenommen, sich aber für „Annehmen und später konfigurieren“ entschieden haben. Andernfalls möchten Sie vielleicht einfach das Zielspeicherkonto für die empfangenen Daten ändern. 

## <a name="navigate-to-a-received-data-share"></a>Navigieren zu einer empfangenen Datenfreigabe

Navigieren Sie im Azure Data Share-Dienst zu der empfangenen Freigabe, und wählen Sie die Registerkarte **Details** aus. 

![Datasetzuordnung](./media/dataset-mapping.png "Datasetzuordnung") 

Aktivieren Sie das Kontrollkästchen neben dem Dataset, dem Sie ein Ziel zuweisen möchten, und klicken Sie auf **+ Ziel zuordnen**. Sie müssen die Zuordnung möglicherweise zunächst aufheben, wenn Sie bereits ein Zielspeicherkonto konfiguriert haben und die Zuordnung nun in ein anderes Speicherkonto ändern möchten. 

![Zuordnung zum Ziel](./media/dataset-map-target.png "Zuordnung auf Ziel") 

## <a name="select-a-new-storage-account"></a>Auswählen eines neuen Speicherkontos 

Wählen Sie ein Speicherkonto aus, in dem die Daten gespeichert werden sollen. Beachten Sie, dass alle Daten, die bereits in einem zuvor zugeordneten Speicherkonto vorhanden sind, nicht automatisch in das neue Speicherkonto verschoben werden.

![Zielspeicherkonto](./media/map-target.png "Zielspeicher") 

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Freigeben von Daten finden Sie im Tutorial zum [Freigeben Ihrer Daten](share-your-data.md).



