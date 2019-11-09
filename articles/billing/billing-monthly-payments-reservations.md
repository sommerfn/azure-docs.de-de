---
title: Erwerben von Azure-Reservierungen mit Vorauszahlung oder monatlicher Zahlung
description: Hier erfahren Sie, wie Sie Azure-Reservierungen mit Vorauszahlung oder monatlicher Zahlung erwerben.
services: billing
author: bandersmsft
manager: yashar
ms.service: billing
ms.topic: conceptual
ms.date: 11/01/2019
ms.author: banders
ms.openlocfilehash: b1cc034b4d397f170b57e873cb9a5d74c91f83f4
ms.sourcegitcommit: 827248fa609243839aac3ff01ff40200c8c46966
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73746461"
---
# <a name="purchase-reservations-with-monthly-payments"></a>Erwerben von Reservierungen mit monatlicher Zahlung

In der Vergangenheit mussten Azure-Reservierungen im Voraus bezahlt werden. Inzwischen können Sie Reservierungen aber auch monatlich bezahlen. Bei einem Kauf mit Vorauszahlung zahlen Sie den gesamten Betrag. Bei der monatlichen Zahlungsoption werden dagegen die Gesamtkosten der Reservierung gleichmäßig auf die einzelnen Monate der Laufzeit aufgeteilt. Die Gesamtkosten für vorab bezahlte und monatliche Reservierungen sind gleich. Es fallen keine zusätzlichen Gebühren an, wenn Sie sich für die monatliche Zahlung entscheiden.

Ihr monatlicher Zahlungsbetrag variiert möglicherweise abhängig vom Wechselkurs des aktuellen Monats für Ihre lokale Währung.

Monatliche Zahlungen sind für Folgendes verfügbar:

- Virtuelle Computer
- Azure Storage
- SQL-Datenbank
- SQL Data Warehouse
- Cosmos DB
- App Service-Stempelgebühr

Reservierungen können über das [Azure-Portal](https://portal.azure.com/?Microsoft_Azure_Reservations_EnableMultiCart=true&amp;paymentPlan=true#blade/Microsoft_Azure_Reservations/CreateBlade) erworben werden.

![Beispiel für Reservierungserwerb](./media/billing-monthly-payments-reservations/purchase-reservation.png)

Wenn Sie eine Reservierung erwerben, können Sie den Zahlungsplan anzeigen. Klicken Sie auf **Vollständigen Zahlungsplan anzeigen**.

![Beispiel für den Zahlungsplan einer Reservierung](./media/billing-monthly-payments-reservations/prepurchase-schedule.png)

Wenn Sie den Zahlungsplan nach dem Kauf anzeigen möchten, wählen Sie eine Reservierung aus, klicken Sie auf die **ID des Reservierungsauftrags**, und klicken Sie anschließend auf die Registerkarte **Zahlungen**.

## <a name="view-payments-made"></a>Anzeigen geleisteter Zahlungen

Geleistete Zahlungen können mithilfe von APIs und Nutzungsdaten sowie in der Kostenanalyse angezeigt werden. Bei Reservierungen mit monatlicher Zahlung wird der Häufigkeitswert in Nutzungsdaten und in der Reservierungsgebühren-API als **Serie** angezeigt. Bei Reservierungen mit Vorauszahlung wird der Wert als **Einmalig** angezeigt.

In der Standardansicht der Kostenanalyse werden monatliche Käufe angezeigt. Wenden Sie für **Gebührentyp** den Filter **Einkauf** und für **Häufigkeit** den Filter **Serie** an, um alle Käufe anzuzeigen. Sollen nur Reservierungen angezeigt werden, wenden Sie einen Filter für **Reservierung** an.

![Beispiel für Reservierungserwerbskosten in der Kostenanalyse](./media/billing-monthly-payments-reservations/cost-analysis.png)

## <a name="switch-to-monthly-payments-at-renewal"></a>Umstellen auf monatliche Zahlung bei der Verlängerung

Wenn Sie eine Reservierung verlängern, können Sie auf eine monatliche Abrechnungshäufigkeit umstellen.

## <a name="exchange-and-refunds"></a>Umtausch und Rückerstattungen

Mit monatlicher Abrechnung erworbene Reservierungen können genau wie andere Reservierungen erstattet oder umgetauscht werden. Aktuell können Sie eine Supportanfrage übermitteln, um einen Umtausch oder eine Rückerstattung für eine mit monatlicher Abrechnung erworbene Reservierung zu initiieren.

Wenn Sie eine Reservierung mit monatlicher Zahlung umtauschen, müssen die Kosten für die gesamte Lebensdauer des neuen Einkaufs höher sein als die Restzahlungen, die für die zurückgegebene Reservierung storniert werden. Ansonsten gelten für den Umtausch keine weiteren Einschränkungen, und es fallen auch keine Gebühren an. Sie können eine Reservierung mit Vorauszahlung umtauschen, um eine neue Reservierung mit monatlicher Abrechnung zu erwerben. Der Wert für die Lebensdauer der neuen Reservierung muss jedoch höher sein als der anteilige Wert der zurückgegebenen Reservierung.

Wenn Sie eine Reservierung mit monatlicher Zahlung stornieren, erhebt Microsoft ggf. eine Stornogebühr für die stornierten weiteren Zahlungen. Die übrigen ausstehenden Zahlungen werden mit dem Rückerstattungslimit von 50.000 USD verrechnet.

Weitere Informationen zu Umtausch und Rückerstattungen finden Sie unter [Self-Service-Umtausch und -Rückerstattungen für Azure-Reservierungen](billing-azure-reservations-self-service-exchange-and-refund.md).

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu Reservierungen finden Sie unter [Was sind Azure-Reservierungen?](billing-save-compute-costs-reservations.md).
- Informationen zu Aufgaben, die vor dem Erwerb einer Reservierung ausgeführt werden sollten, finden Sie unter [Bestimmen der passenden Größe des virtuellen Computers vor dem Kauf](../virtual-machines/windows/prepay-reserved-vm-instances.md#determine-the-right-vm-size-before-you-buy).
