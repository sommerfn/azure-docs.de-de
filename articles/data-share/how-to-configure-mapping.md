---
title: Konfigurieren einer Datasetzuordnung
description: Konfigurieren einer Datasetzuordnung
author: joannapea
ms.service: data-share
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: joanpo
ms.openlocfilehash: 581e1eef5f1d64e68a6501f56ce60218281c605d
ms.sourcegitcommit: 1572b615c8f863be4986c23ea2ff7642b02bc605
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2019
ms.locfileid: "67788565"
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



