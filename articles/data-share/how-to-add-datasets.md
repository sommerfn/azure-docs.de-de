---
title: Hinzufügen von Datasets zu einer vorhandenen Azure Data Share-Instanz
description: In diesem Artikel wird erläutert, wie Sie einer vorhandenen Datenfreigabe in Azure Data Share Datasets hinzufügen und für dieselben Empfänger freigeben.
author: joannapea
ms.author: joanpo
ms.service: data-share
ms.topic: conceptual
ms.date: 07/10/2019
ms.openlocfilehash: 00c96950565b077e65f84e2d8b4977092df5e317
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73490537"
---
# <a name="how-to-add-datasets-to-an-existing-share-in-azure-data-share"></a>Hinzufügen von Datasets zu einer vorhandenen Datenfreigabe in Azure Data Share

In diesem Artikel wird erläutert, wie Sie einer vorhandenen Datenfreigabe mithilfe von Azure Data Share Datasets hinzufügen. Auf diese Weise können Sie mehr Daten für dieselben Empfänger freigeben, ohne eine neue Freigabe zu erstellen.

Informationen zum Hinzufügen von Datasets beim Erstellen einer Freigabe finden Sie im Tutorial [Freigeben von Daten](share-your-data.md).

## <a name="navigate-to-a-sent-data-share"></a>Navigation zu einer gesendeten Datenfreigabe

Navigieren Sie in Azure Data Share zur gesendeten Freigabe, und wählen Sie die Registerkarte **Datasets** aus. Klicken Sie auf die Schaltfläche **+ Datasets hinzufügen**, um weitere Datasets hinzuzufügen.

![Hinzufügen von Datasets](./media/how-to/how-to-add-datasets/add-datasets.png)

Wählen Sie im Panel auf der rechten Seite den Datasettyp aus, den Sie hinzufügen möchten, und klicken Sie dann auf **Weiter**. Wählen Sie das Abonnement und die Ressourcengruppe der Daten aus, die Sie hinzufügen möchten. Suchen Sie mithilfe der nach unten weisenden Pfeile nach den Daten, die hinzugefügt werden sollen, und aktivieren Sie dann das Feld neben den Daten.

![Hinzufügen von Datasets](./media/how-to/how-to-add-datasets/add-datasets-side.png)

Sobald Sie auf **Datasets hinzufügen** klicken, werden die Datasets zu Ihrer Freigabe hinzugefügt. Hinweis: Ihre Consumer müssen eine Momentaufnahme auslösen, damit ihnen die neuen Datasets angezeigt werden. Wenn Einstellungen für Momentaufnahmen konfiguriert sind, sehen die Consumer die neuen Datasets, sobald die nächste geplante Momentaufnahme abgeschlossen wurde. Sind keine Einstellungen für Momentaufnahmen konfiguriert, muss der Consumer manuell eine vollständige oder inkrementelle Kopie der Daten auslösen, um die Aktualisierungen zu erhalten. Weitere Informationen zu Momentaufnahmen finden Sie unter [Momentaufnahmen](terminology.md).

## <a name="next-steps"></a>Nächste Schritte
Informationen zum [Hinzufügen von Empfängern zu einer vorhandenen Datenfreigabe](how-to-add-recipients.md)