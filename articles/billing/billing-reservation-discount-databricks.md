---
title: Anwenden eines Azure Databricks-Vorauszahlungsrabatts
description: Hier erfahren Sie, wie sich ein Azure Databricks-Vorauszahlungsrabatt auf die Nutzung auswirkt.
services: billing
author: yashesvi
manager: yashar
ms.service: billing
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: banders
ms.openlocfilehash: 7c1855b587ab1d603e9c6ac24a67b0f50065361f
ms.sourcegitcommit: 3e7646d60e0f3d68e4eff246b3c17711fb41eeda
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2019
ms.locfileid: "67827634"
---
# <a name="how-azure-databricks-pre-purchase-discount-is-applied"></a>Anwenden eines Azure Databricks-Vorauszahlungsrabatts

Sie können vorab erworbene Azure Databricks-Commit-Einheiten (Azure Databricks Commit Units, DBCUs) jederzeit während der Laufzeit des Kaufs nutzen. Jede Azure Databricks-Nutzung wird automatisch auf die vorab erworbenen DBCUs angerechnet.

Vorab erworbene Einheiten laufen im Gegensatz zu VMs nicht automatisch stündlich ab. Sie können sie jederzeit während der Laufzeit des Kaufs verwenden. Um die Vorauszahlungsrabatte zu erhalten, ist es nicht erforderlich, für die Nutzung einen Plan mit Vorauszahlung erneut bereitzustellen oder Ihren Azure Databricks-Arbeitsbereichen zuzuweisen.

Der Vorauszahlungsrabatt gilt nur für die DBU-Nutzung (Azure Databricks Unit). Andere Gebühren, z. B. Compute-, Speicher- und Netzwerkgebühren, werden separat abgerechnet.

## <a name="pre-purchase-discount-application"></a>Anwendung des Vorauszahlungsrabatts

Databricks mit Vorauszahlung gilt für alle Databricks-Workloads und -Tarife. Sie können den Plan mit Vorauszahlung als Pool im Voraus bezahlter Databricks-Commit-Einheiten vorstellen. Die Nutzung wird unabhängig von der Arbeitsauslastung und dem Tarif auf den Pool angerechnet. Für die Anrechnung der Nutzung gilt das folgende Verhältnis:

| **Workload** | **Verhältnis der Anwendung von DBUs – Standard-Tarif** | **Verhältnis der Anwendung von DBUs – Premium-Tarif** |
| --- | --- | --- |
| Datenanalysen | 0,4 | 0,55 |
| Datentechnik | 0,15 | 0,30 |
| Datentechnik Light | 0,07 | 0,22 |

Wenn z. B. ein Betrag von Datenanalysen – Standard-Tarif genutzt wird, werden auf die vorausgezahlten Databricks-Commit-Einheiten 0,4 Einheiten angerechnet. Wenn ein Betrag von Datenanalysen Light – Standard-Tarif genutzt wird, werden auf die vorausgezahlte Databricks-Commit-Einheit 0,07 Einheiten angerechnet.

## <a name="determine-plan-use"></a>Bestimmen der Nutzung des Plans

Um die Verwendung Ihres DBCU-Plans zu ermitteln, wechseln Sie zu Azure-Portal > **Reservierungen**, und klicken Sie auf den erworbenen Databricks-Plan. Ihre Nutzung bis zum aktuellen Datum wird mit den verbleibenden Einheiten angezeigt. Weitere Informationen zum Bestimmen der Nutzung Ihrer Reservierung finden Sie im Artikel [Anzeigen der Reservierungsnutzung](billing-reservation-apis.md#see-reservation-usage).

## <a name="how-discount-application-shows-in-usage-data"></a>Anzeigen der Anwendung des Rabatts in den Nutzungsdaten

Wenn der Vorauszahlungsrabatt auf die Databricks-Nutzung angewendet wird, wird für nutzungsbasierte Gebühren in den Nutzungsdaten 0 angezeigt. Weitere Informationen zu Reservierungskosten und zur Nutzung finden Sie unter [Abrufen von Reservierungskosten und Nutzung laut Enterprise Agreement](billing-understand-reserved-instance-usage-ea.md).

## <a name="need-help-contact-us"></a>Sie brauchen Hilfe? Wenden Sie sich an uns.

Wenn Sie weitere Fragen haben oder Hilfe benötigen, [erstellen Sie eine Supportanfrage](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="next-steps"></a>Nächste Schritte

- Informationen zum Verwalten einer Reservierung finden Sie unter [Verwalten von Azure-Reservierungen](billing-manage-reserved-vm-instance.md).
- Weitere Informationen zur Kostenersparnis durch den Voraberwerb von Azure Databricks finden Sie unter [Verringern der Kosten für Azure Databricks durch Vorauszahlung](billing-prepay-databricks-reserved-capacity.md).
- Weitere Informationen zu Azure-Reservierungen finden Sie in den folgenden Artikeln:
  - [Was sind Azure-Reservierungen?](billing-save-compute-costs-reservations.md)
  - [Verwalten von Reservierungen für Ressourcen in Azure](billing-manage-reserved-vm-instance.md)
  - [Grundlegendes zur Reservierungsnutzung bei einem Abonnement mit Sätzen für nutzungsbasierte Bezahlung](billing-understand-reserved-instance-usage.md)
  - [Grundlegendes zur Nutzung von Azure-Reservierungen für den Konzernbeitritt](billing-understand-reserved-instance-usage-ea.md)
