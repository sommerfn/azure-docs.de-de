---
title: Grenzwerte, Kontingente und Schwellenwerte in Azure Scheduler
description: Informationen zu Grenzwerten, Kontingenten, Standardwerten und Drosselungsschwellenwerten für Azure Scheduler
services: scheduler
ms.service: scheduler
author: derek1ee
ms.author: deli
ms.reviewer: klam
ms.assetid: 88f4a3e9-6dbd-4943-8543-f0649d423061
ms.topic: article
ms.date: 08/18/2016
ms.openlocfilehash: 293cd956f8270a4863fcc657f58c970096cec1e3
ms.sourcegitcommit: 29880cf2e4ba9e441f7334c67c7e6a994df21cfe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/26/2019
ms.locfileid: "71300924"
---
# <a name="limits-quotas-and-throttle-thresholds-in-azure-scheduler"></a>Grenzwerte, Kontingente und Drosselungsschwellenwerte in Azure Scheduler

> [!IMPORTANT]
> Azure Scheduler wird [eingestellt](../scheduler/migrate-from-scheduler-to-logic-apps.md#retire-date) und durch [Azure Logic Apps](../logic-apps/logic-apps-overview.md) ersetzt. Wenn Sie weiterhin mit den Aufträgen arbeiten möchten, die Sie in Scheduler eingerichtet haben, sollten Sie so bald wie möglich [zu Azure Logic Apps migrieren](../scheduler/migrate-from-scheduler-to-logic-apps.md).

## <a name="limits-quotas-and-thresholds"></a>Grenzwerte, Kontingente und Schwellenwerte

[!INCLUDE [scheduler-limits-table](../../includes/scheduler-limits-table.md)]

## <a name="x-ms-request-id-header"></a>x-ms-request-id header

Jede Anforderung für den Scheduler-Dienst gibt einen Antwortheader namens **x-ms-request-id**zurück. Dieser Header enthält einen opaken Wert, der die Anforderung eindeutig identifiziert. Wenn also eine Anforderung immer wieder fehlschlägt und Sie bestätigt haben, dass die Anforderung ordnungsgemäß formatiert ist, können Sie den Fehler an Microsoft melden, indem Sie den Antwortheaderwert **x-ms-request id** und die folgenden Details angeben: 

* Den **x-ms-request-id**-Wert
* Die ungefähre Zeit, zu der die Anforderung vorgenommen wurde 
* Die Bezeichner für das Azure-Abonnement, die Auftragssammlung und den Auftrag 
* Die Art des Vorgangs, den die Anforderung versucht hat

## <a name="see-also"></a>Weitere Informationen

* [Was ist der Azure Scheduler?](scheduler-intro.md)
* [Konzepte, Terminologie und Entitätshierarchie für Azure Scheduler](scheduler-concepts-terms.md)
