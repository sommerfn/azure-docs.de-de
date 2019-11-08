---
title: Widerrufen eines Freigabeabonnements in Azure Data Share
description: Hier erfahren Sie, wie Sie ein Freigabeabonnement von einem Empfänger mithilfe von Azure Data Share widerrufen.
author: joannapea
ms.author: joanpo
ms.service: data-share
ms.topic: conceptual
ms.date: 07/10/2019
ms.openlocfilehash: 97f672b266c2df6956282aea6dec8d5207944943
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73476372"
---
# <a name="how-to-revoke-a-consumers-share-subscription-in-azure-data-share"></a>Widerrufen eines Freigabeabonnements eines Consumers in Azure Data Share

In diesem Artikel wird erläutert, wie ein Freigabeabonnement für einen oder mehrere Consumer über Azure Data Share widerrufen wird. Hierdurch wird der Consumer daran gehindert, weitere Momentaufnahmen auszulösen. Wenn der Consumer noch keine Momentaufnahme ausgelöst hat, werden die Daten nach dem Aufheben des Freigabeabonnements nicht mehr empfangen. Wenn zuvor eine Momentaufnahme ausgelöst wurde, verbleiben die letzten Daten im Konto des Consumers.

## <a name="navigate-to-a-sent-data-share"></a>Navigation zu einer gesendeten Datenfreigabe

Navigieren Sie in Azure Data Share zur gesendeten Freigabe, und wählen Sie die Registerkarte **Freigabeabonnements** aus.

![Aufheben eines Freigabeabonnements](./media/how-to/how-to-revoke-share-subscription/revoke-share-subscription.png) 

Aktivieren Sie die Felder neben den Empfängern, deren Freigabeabonnements Sie löschen möchten, und klicken Sie auf **Widerrufen**. Der Consumer erhält für seine Daten keine Aktualisierungen mehr.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum [Überwachen Ihrer Datenfreigaben](how-to-monitor.md).