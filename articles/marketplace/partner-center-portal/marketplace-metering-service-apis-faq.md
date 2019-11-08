---
title: Marketplace-Messungsdienst-APIs – Häufig gestellte Fragen | Azure Marketplace
description: Ausgeben der Nutzung eines SaaS-Angebots im Azure Marketplace
author: qianw211
manager: evansma
ms.author: v-qiwe
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 07/11/2019
ms.openlocfilehash: c4f51adbcaa5e5b750169f53a1333544365fd4f3
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73825497"
---
# <a name="marketplace-metering-service-apis---faq"></a>Marketplace-Messungsdienst-APIs – Häufig gestellte Fragen

Nachdem ein Azure-Benutzer einen SaaS-Dienst abonniert hat, der getaktete Abrechnung umfasst, wird der Verbrauch für jede vom Kunden verwendete Abrechnungsdimension nachverfolgt. Wenn der Verbrauch die eingeschlossenen Mengen für den vom Benutzer ausgewählten Abrechnungszeitraum überschreitet, gibt der Dienst Nutzungsereignisse aus.

## <a name="emit-usage-events"></a>Ausgeben von Nutzungsereignissen

>[!Note]
>Dieser Abschnitt gilt nur für SaaS-Angebote, bei denen zum Zeitpunkt der Veröffentlichung des Angebots für mindestens einen der Pläne Messungsdienstdimensionen definiert waren.

![Ausgeben von Nutzungsereignissen](media/isv-emits-usage-event.png)

Informationen zum API-Vertrag für die Ausgabe von Nutzungsereignissen finden Sie unter [SaaS-Batch-Nutzungsereignis-API](./marketplace-metering-service-apis.md#batch-usage-event).

### <a name="how-often-is-it-expected-to-emit-usage"></a>Wie oft wird die Ausgabe der Nutzung erwartet?

Im Idealfall wird davon ausgegangen, dass die Nutzung für die letzte Stunde stündlich ausgegeben wird, jedoch nur, wenn in der letzten Stunde Nutzung stattgefunden hat.

### <a name="what-is-the-maximum-delay-between-the-time-an-event-occurs-and-the-time-a-usage-event-is-emitted-to-microsoft"></a>Wie groß ist die maximale Verzögerung zwischen dem Zeitpunkt, zu dem ein Ereignis eingetreten ist, und dem Zeitpunkt der Ausgabe des Nutzungsereignisses an Microsoft?

Idealerweise werden Nutzungsereignisse stündlich für Ereignisse ausgegeben, die in der letzten Stunde eingetreten sind. Es werden jedoch Verzögerungen erwartet. Die maximal zulässige Verzögerung beträgt 24 Stunden. Danach werden Nutzungsereignisse nicht mehr akzeptiert.

Wenn ein Nutzungsereignis beispielsweise um 1 Uhr auftritt, kann ein entsprechendes Nutzungsereignis bis 1 Uhr am Folgetag ausgegeben werden. Falls das System, von dem die Nutzung ausgegeben wird, ausfällt, kann es nach der Wiederherstellung das Nutzungsereignis für das Stundenintervall, in dem die Nutzung erfolgt ist, ohne Genauigkeitseinbußen senden.

### <a name="what-happens-when-you-send-more-than-one-usage-event-on-the-same-hour"></a>Was geschieht, wenn in derselben Stunde mehrere Nutzungsereignisse gesendet werden?

Für das Stundenintervall wird nur ein Nutzungsereignis akzeptiert. Das Stundenintervall beginnt in Minute 0 und endet mit Minute 59.  Wenn für das gleiche Stundenintervall mehrere Nutzungsereignisse ausgegeben werden, werden alle Nutzungsereignisse nach dem ersten Ereignis als Duplikate gelöscht.

### <a name="what-happens-when-you-emit-usage-for-a-saas-subscription-that-has-been-unsubscribed-already"></a>Was geschieht, wenn die Nutzung für ein SaaS-Abonnement ausgegeben wird, das bereits gekündigt wurde?

An die Marketplace-Plattform ausgegebene Nutzungsereignisse werden nicht akzeptiert, nachdem ein SaaS-Abonnement gelöscht wurde.

### <a name="can-you-get-a-list-of-all-saas-subscriptions-including-active-and-unsubscribed-subscriptions"></a>Kann ich eine Liste aller SaaS-Abonnements, einschließlich aktiver und gekündigter Abonnements, abrufen?

Ja. Wenn Sie die `GET /saas/subscriptions`-API aufrufen, enthält sie eine Liste aller SaaS-Abonnements. Im Statusfeld in der Antwort für das jeweilige SaaS-Abonnement wird angegeben, ob das Abonnement aktiv oder gekündigt ist. Durch einen einzelnen Aufruf zum Auflisten der Abonnements werden maximal 100 Abonnements zurückgegeben.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen finden Sie unter [Marketplace-Messungsdienst-APIs](./marketplace-metering-service-apis.md).
