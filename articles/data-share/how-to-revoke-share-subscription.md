---
title: Widerrufen eines Freigabeabonnements in Azure Data Share (Vorschauversion)
description: Aufheben einer Abonnementfreigabe
author: joannapea
ms.author: joanpo
ms.service: data-share
ms.topic: conceptual
ms.date: 07/10/2019
ms.openlocfilehash: 08a48202c26df1c24216572b1a52ac45506c6229
ms.sourcegitcommit: e9936171586b8d04b67457789ae7d530ec8deebe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71326539"
---
# <a name="how-to-revoke-a-consumers-share-subscription-in-azure-data-share-preview"></a>Widerrufen eines Freigabeabonnements eines Consumers in Azure Data Share (Vorschauversion)

In diesem Artikel wird erläutert, wie ein Freigabeabonnement für einen oder mehrere Consumer über Azure Data Share (Vorschauversion) widerrufen wird. Hierdurch wird der Consumer daran gehindert, weitere Momentaufnahmen auszulösen. Wenn der Consumer noch keine Momentaufnahme ausgelöst hat, werden die Daten nach dem Aufheben des Freigabeabonnements nicht mehr empfangen. Wenn zuvor eine Momentaufnahme ausgelöst wurde, verbleiben die letzten Daten im Konto des Consumers.

## <a name="navigate-to-a-sent-data-share"></a>Navigieren zu einer gesendeten Datenfreigabe

Navigieren Sie in Azure Data Share (Vorschauversion) zur gesendeten Freigabe, und klicken Sie auf die Registerkarte **Freigabeabonnements**.

![Aufheben eines Freigabeabonnements](./media/how-to/how-to-revoke-share-subscription/revoke-share-subscription.png) 

Aktivieren Sie die Felder neben den Empfängern, deren Freigabeabonnements Sie löschen möchten, und klicken Sie auf **Widerrufen**. Der Consumer erhält für seine Daten keine Aktualisierungen mehr.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum [Überwachen Ihrer Datenfreigaben](how-to-monitor.md).