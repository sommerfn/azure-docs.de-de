---
title: Azure Event Grid – Handbuch zur Problembehandlung
description: Dieser Artikel bietet eine Liste mit Fehlercodes, Fehlermeldungen, Beschreibungen und empfohlenen Aktionen.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 08/22/2019
ms.author: spelluru
ms.openlocfilehash: c4c4e7980247e0ce41c685873a2d50b3e37fb405
ms.sourcegitcommit: 388c8f24434cc96c990f3819d2f38f46ee72c4d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2019
ms.locfileid: "70061473"
---
# <a name="troubleshoot-azure-event-grid-errors"></a>Problembehandlung von Azure Event Grid-Fehlern
In diesem Handbuch zur Problembehandlung erhalten Sie eine Liste mit Azure Event Grid-Fehlercodes und -Fehlermeldungen sowie deren Beschreibungen und empfohlenen Aktionen, die Sie ausführen sollten, wenn diese Fehler bei Ihnen auftreten. 

## <a name="error-code-400"></a>Fehlercode: 400
| Fehlercode | Fehlermeldung | BESCHREIBUNG | Empfehlung |
| ---------- | ------------- | ----------- | -------------- | 
| HttpStatusCode.BadRequest<br/>400 | Der Themenname muss zwischen 3 und 50 Zeichen lang sein. | Der benutzerdefinierte Themenname sollte zwischen 3 und 50 Zeichen lang sein. Themennamen dürfen nur alphanumerische Buchstaben, Ziffern und das Zeichen „-“ (Bindestrich) enthalten. Außerdem sollte der Name nicht mit den folgenden reservierten Wörtern beginnen: <ul><li>Microsoft</li><li>EventGrid</li><li>System</li></ul> | Wählen Sie einen anderen Themennamen aus, der die Anforderungen an Themennamen erfüllt. |
| HttpStatusCode.BadRequest<br/>400 | Der Domänenname muss zwischen 3 und 50 Zeichen lang sein. | Die Länge des Domänennamens sollte zwischen 3 und 50 Zeichen betragen. Themennamen dürfen nur alphanumerische Buchstaben, Ziffern und das Zeichen „-“ (Bindestrich) enthalten. Außerdem sollte der Name nicht mit den folgenden reservierten Wörtern beginnen:<ul><li>Microsoft</li><li>EventGrid</li><li>System</li> | Wählen Sie einen anderen Domänennamen aus, der die Anforderungen an Domänennamen erfüllt. |
| HttpStatusCode.BadRequest<br/>400 | Ungültige Ablaufzeit. | Die Ablaufzeit für das Ereignisabonnement bestimmt, wann das Ereignisabonnement abläuft. Dieser Wert sollte in der Zukunft liegender, gültiger DateTime-Wert sein.| Stellen Sie sicher, dass die Ablaufzeit des Ereignisabonnements ein gültiges DateTime-Format hat und in der Zukunft liegt. |

## <a name="error-code-409"></a>Fehlercode: 409
| Fehlercode | Fehlermeldung | BESCHREIBUNG | Empfohlene Maßnahme |
| ---------- | ------------- | ----------- | -------------- | 
| HttpStatusCode.Conflict <br/>409 | Ein Thema mit dem angegebenen Namen ist bereits vorhanden. Wählen Sie einen anderen Themennamen aus.   | Der benutzerdefinierte Themenname sollte in einer einzelnen Azure-Region eindeutig sein, um einen korrekten Veröffentlichungsvorgang sicherzustellen. Derselbe Name kann in verschiedenen Azure-Regionen verwendet werden. | Wählen Sie einen anderen Namen für das Thema aus. |
| HttpStatusCode.Conflict <br/> 409 | Eine Domäne mit dem angegebenen Namen ist bereits vorhanden. Wählen Sie einen anderen Domänennamen aus. | Der Domänenname sollte in einer einzelnen Azure-Region eindeutig sein, um einen korrekten Veröffentlichungsvorgang sicherzustellen. Derselbe Name kann in verschiedenen Azure-Regionen verwendet werden. | Wählen Sie einen anderen Namen für die Domäne aus. |
| HttpStatusCode.Conflict<br/>409 | Kontingentlimit erreicht. Weitere Informationen zu diesen Limits finden Sie unter [Azure Event Grid-Grenzwerte](../azure-subscription-service-limits.md#event-grid-limits).  | Für jedes Azure-Abonnement gilt ein Limit hinsichtlich der Anzahl von Azure Event Grid-Ressourcen, die es verwenden kann. Einige oder alle dieses Kontingente wurden überschritten, und es konnten keine Ressourcen mehr erstellt werden. |  Überprüfen Sie Ihre aktuelle Ressourcennutzung, und löschen Sie alle nicht benötigten Ressourcen. Wenn Sie Ihr Kontingent immer noch erhöhen müssen, senden Sie eine E-Mail mit der genauen Anzahl der benötigten Ressourcen an [aeg@microsoft.com](mailto:aeg@microsoft.com). |


## <a name="next-steps"></a>Nächste Schritte
Wenn Sie weitere Hilfe benötigen, veröffentlichen Sie Ihr Problem im [Stack Overflow-Forum](https://stackoverflow.com/questions/tagged/azure-eventgrid), oder öffnen Sie ein [Supportticket](https://azure.microsoft.com/support/options/). 
