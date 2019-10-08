---
title: Grundlegendes zu den Gebühren auf der Rechnung für eine Microsoft-Kundenvereinbarung – Azure
description: Informationen zum Lesen und Verstehen der Gebühren auf Ihrer Rechnung.
author: jureid
manager: jureid
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/01/2019
ms.author: banders
ms.openlocfilehash: 8ab6cf061531a1ef3c72b2f36d25932c7498d291
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71345287"
---
# <a name="understand-charges-on-your-microsoft-customer-agreement-invoice"></a>Grundlegendes zu den Gebühren auf der Rechnung für eine Microsoft-Kundenvereinbarung

Durch Analysieren der einzelnen Transaktionen können Sie die Gebühren auf Ihrer Rechnung verstehen. Im Abrechnungskonto für eine Microsoft-Kundenvereinbarung wird jeden Monat eine Rechnung für jedes Abrechnungsprofil generiert. Die Rechnung enthält alle Gebühren des vorherigen Monats. Sie können Ihre Rechnungen im Azure-Portal anzeigen. Weitere Informationen finden Sie unter [Herunterladen von Rechnungen für eine Microsoft-Kundenvereinbarung](billing-download-azure-invoice-daily-usage-date.md#download-invoices-for-a-microsoft-customer-agreement).

Dieser Artikel bezieht sich auf ein Abrechnungskonto für eine Microsoft-Kundenvereinbarung. [Überprüfen Sie, ob Sie Zugriff auf eine Microsoft-Kundenvereinbarung haben](#check-access-to-a-microsoft-customer-agreement).

## <a name="view-transactions-for-an-invoice-in-the-azure-portal"></a>Anzeigen von Transaktionen für eine Rechnung im Azure-Portal

1. Melden Sie sich beim [Azure-Portal](https://www.azure.com) an.

2. Suchen Sie nach **Kostenverwaltung + Abrechnung**.

    ![Screenshot: Suchen nach „Kostenverwaltung + Abrechnung“ im Azure-Portal](./media/billing-understand-your-bill-mca/billing-search-cost-management-billing.png)

3. Wählen Sie links auf der Seite die Option **Alle Transaktionen** aus. Abhängig von Ihren Zugriffsberechtigungen müssen Sie möglicherweise zuerst ein Abrechnungskonto, ein Abrechnungsprofil oder einen Rechnungsabschnitt und dann **Alle Transaktionen** auswählen.

4. Auf der Seite „Alle Transaktionen“ werden die folgenden Informationen angezeigt:

    ![Screenshot der Liste der abgerechneten Transaktionen](./media/billing-understand-your-bill-mca/mca-billed-transactions-list.png)

    |Column  |Definition  |
    |---------|---------|
    |Date     | Das Datum der Transaktion  |
    |Rechnungs-ID     | Der Bezeichner für die Rechnung, auf der die Transaktion in Rechnung gestellt wurde. Wenn Sie eine Supportanfrage senden, teilen Sie dem Azure-Support die ID mit, um die Bearbeitung Ihrer Supportanfrage zu beschleunigen. |
    |Transaktionstyp     |  Der Typ der Transaktion, z. B. Erwerb, Stornierung und Nutzungsgebühren  |
    |Produktfamilie     | Die Kategorie des Produkts, z. B. „Compute“ für virtuelle Computer oder „Datenbank“ für Azure SQL-Datenbank|
    |Produkt-SKU     | Ein eindeutiger Code, der die Instanz Ihres Produkts identifiziert |
    |Amount     |  Der Betrag der Transaktion      |
    |Rechnungsabschnitt     | Die Transaktion wird in diesem Abschnitt der Rechnung des Abrechnungsprofils angezeigt. |
    |Abrechnungsprofil     | Die Transaktion wird auf der Rechnung dieses Abrechnungsprofils angezeigt. |

5. Suchen Sie nach der Rechnungs-ID, um die Transaktionen für die Rechnung zu filtern.

### <a name="view-transactions-by-invoice-sections"></a>Anzeigen von Transaktionen nach Rechnungsabschnitten

Mithilfe von Rechnungsabschnitten können Sie die Kosten auf einer Rechnung des Abrechnungsprofils organisieren. Weitere Informationen finden Sie unter [Grundlegendes zu Rechnungsabschnitten](billing-mca-overview.md#invoice-sections). Beim Generieren einer Rechnung werden die Gebühren für alle Abschnitte im Abrechnungsprofil auf der Rechnung aufgeführt.

In der folgenden Abbildung sind die Gebühren für den Rechnungsabschnitt „Accounting Dept“ (Buchhaltungsabteilung) einer Beispielrechnung dargestellt.

![Beispielabbildung der Details der Informationen eines Rechnungsabschnitts](./media/billing-understand-your-bill-mca/invoicesection-details.png)

Nachdem Sie die Gebühren für einen Rechnungsabschnitt identifiziert haben, können Sie im Azure-Portal die Transaktionen anzeigen, um die Gebühren nachzuvollziehen/zu verstehen.

1. Wechseln Sie im Azure-Portal zur Seite „Alle Transaktionen“, um die Transaktionen für eine Rechnung anzuzeigen. Weitere Informationen finden Sie unter [Anzeigen von Transaktionen für eine Rechnung im Azure-Portal](#view-transactions-for-an-invoice-in-the-azure-portal).

2. Filtern Sie nach dem Namen eines Rechnungsabschnitts, um die Transaktionen anzuzeigen.

## <a name="review-pending-charges-to-estimate-your-next-invoice"></a>Überprüfen der ausstehenden Gebühren zum Schätzen Ihrer nächsten Rechnung

Im Abrechnungskonto für eine Microsoft-Kundenvereinbarung werden die Gebühren geschätzt und bis zur Rechnungsstellung als ausstehend betrachtet. Im Azure-Portal können Sie die ausstehenden Gebühren anzeigen, um Ihre nächste Rechnung zu schätzen. Ausstehende Gebühren sind Schätzungen und enthalten keine Steuern. Die tatsächlichen Gebühren in Ihrer nächsten Rechnung weichen von den ausstehenden Gebühren ab.

### <a name="view-summary-of-pending-charges"></a>Anzeigen der Zusammenfassung der ausstehenden Gebühren

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Suchen Sie nach **Kostenverwaltung + Abrechnung**.

   ![Screenshot: Suchen nach „Kostenverwaltung + Abrechnung“ im Azure-Portal](./media/billing-understand-your-bill-mca/billing-search-cost-management-billing.png)

3. Wählen Sie ein Abrechnungsprofil aus. Abhängig von Ihren Zugriffsberechtigungen müssen Sie möglicherweise ein Abrechnungskonto auswählen. Wählen Sie im Abrechnungskonto die Option **Abrechnungsprofile** aus, und wählen Sie dann ein Abrechnungsprofil aus.

4. Wählen Sie am oberen Bildschirmrand die Registerkarte **Zusammenfassung** aus.

5. Im Gebührenabschnitt werden die Gebühren für den bisherigen Kalendermonat und die Gebühren für den letzten Monat angezeigt.

   ![Screenshot, der im Azure-Portal die Suche nach „Kostenverwaltung + Abrechnung“ zeigt](./media/billing-understand-your-bill-mca/mca-billing-profile-summary.png)

Die Gebühren für den bisherigen Kalendermonat sind die ausstehenden Gebühren für den aktuellen Monat. Sie werden abgerechnet, wenn die Rechnung für den Monat generiert wird. Wenn die Rechnung für den letzten Monat noch nicht generiert wurde, sind die Gebühren im letzten Monat auch ausstehende Gebühren. Diese ausstehenden Gebühren werden dann in Ihrer nächsten Rechnung aufgeführt.

### <a name="view-pending-transactions"></a>Anzeigen von ausstehenden Transaktionen

Nachdem Sie die ausstehenden Gebühren identifiziert haben, können Sie die Gebühren nachvollziehen und verstehen, indem Sie die einzelnen Transaktionen analysieren, die zur Entstehung dieser Gebühren beigetragen haben. Zu diesem Zeitpunkt werden die ausstehenden Nutzungsgebühren nicht auf der Seite „Alle Transaktionen“ angezeigt. Sie können die ausstehenden Nutzungsgebühren auf der Azure-Abonnementseite anzeigen. Weitere Informationen finden Sie unter [Anzeigen der ausstehenden Nutzungsgebühren](#view-pending-usage-charges).

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Suchen Sie nach **Kostenverwaltung + Abrechnung**.

   ![Screenshot: Suchen nach „Kostenverwaltung + Abrechnung“ im Azure-Portal](./media/billing-understand-your-bill-mca/billing-search-cost-management-billing.png)

3. Wählen Sie ein Abrechnungsprofil aus. Abhängig von Ihren Zugriffsberechtigungen müssen Sie möglicherweise ein Abrechnungskonto auswählen. Wählen Sie im Abrechnungskonto die Option **Abrechnungsprofile** aus, und wählen Sie dann ein Abrechnungsprofil aus.

4. Wählen Sie links auf der Seite die Option **Alle Transaktionen** aus.

5. Suchen Sie nach *ausstehend*. Verwenden Sie den Filter **Zeitraum**, um die ausstehenden Gebühren für den aktuellen oder letzten Monat anzuzeigen.

   ![Screenshot der Liste der ausstehenden Transaktionen](./media/billing-understand-your-bill-mca/mca-pending-transactions-list.png)

### <a name="view-pending-usage-charges"></a>Anzeigen der ausstehenden Nutzungsgebühren

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Suchen Sie nach *Kostenverwaltung + Abrechnung*.

   ![Screenshot: Suchen nach „Kostenverwaltung + Abrechnung“ im Azure-Portal](./media/billing-understand-your-bill-mca/billing-search-cost-management-billing.png)

3. Wählen Sie ein Abrechnungsprofil aus. Abhängig von Ihren Zugriffsberechtigungen müssen Sie möglicherweise ein Abrechnungskonto auswählen. Wählen Sie im Abrechnungskonto die Option **Abrechnungsprofile** aus, und wählen Sie dann ein Abrechnungsprofil aus.

4. Wählen Sie links auf der Seite die Option **Alle Abonnements** aus.

5. Auf der Azure-Abonnementseite werden die Gebühren des aktuellen und des letzten Monats für jedes Abonnement im Abrechnungsprofil angezeigt. Die Gebühren für den bisherigen Kalendermonat sind die ausstehenden Gebühren für den aktuellen Monat. Sie werden abgerechnet, wenn die Rechnung für den Monat generiert wird. Wenn die Rechnung für den letzten Monat noch nicht generiert wurde, sind die Gebühren im letzten Monat auch ausstehende Gebühren.

    ![Screenshot der Liste der Azure-Abonnements für das Abrechnungsprofil](./media/billing-understand-your-bill-mca/mca-billing-profile-subscriptions-list.png)

## <a name="analyze-your-azure-usage-charges"></a>Analysieren Ihrer Azure-Nutzungsgebühren

Verwenden Sie die CSV-Datei zu Azure-Nutzung und -Gebühren, um Ihre nutzungsbasierten Gebühren zu analysieren. Sie können die Datei für eine Rechnung oder für ausstehende Gebühren herunterladen. Weitere Informationen finden Sie unter [Herunterladen oder Anzeigen Ihrer Azure-Rechnungen und täglichen Nutzungsdaten](billing-download-azure-invoice-daily-usage-date.md).

### <a name="view-detailed-usage-by-invoice-section"></a>Anzeigen der detaillierten Nutzung nach Rechnungsabschnitt

Sie können die Datei zu Azure-Nutzung und -Gebühren filtern, um die Nutzungsgebühren für Ihre Rechnungsabschnitte abzustimmen.

Die folgenden Schritte führen Sie durch die Abstimmung der Computegebühren für den Abschnitt „Accounting Dept“ (Buchhaltungsabteilung):

![Beispielabbildung der Details der Informationen eines Rechnungsabschnitts](./media/billing-understand-your-bill-mca/invoicesection-details.png)

 | PDF-Rechnung | CSV-Datei zu Azure-Nutzung und -Gebühren |
 | --- | --- |
 |Accounting Dept (Buchhaltungsabteilung) |invoiceSectionName |
 |Nutzungsgebühren – Microsoft Azure-Plan |productOrderName |
 |Compute |serviceFamily |

1. Filtern Sie in der CSV-Datei die Spalte **invoiceSectionName** nach **Accounting Dept** (Buchhaltungsabteilung).
2. Filtern Sie in der CSV-Datei die Spalte **productOrderName** nach **Microsoft Azure-Plan**.
3. Filtern Sie in der CSV-Datei die Spalte **serviceFamily** nach **Microsoft.Compute**.

![Screenshot der nach Rechnungsabschnitt gefilterten Datei zu Azure-Nutzung und -Gebühren](./media/billing-understand-your-bill-mca/billing-usage-file-filtered-by-invoice-section.png)


### <a name="view-detailed-usage-by-subscription"></a>Anzeigen der detaillierten Nutzung nach Abonnement

Sie können die CSV-Datei zu Azure-Nutzung und -Gebühren filtern, um die Nutzungsgebühren für Ihre Abonnements abzustimmen. Informationen zum Anzeigen aller Abonnements in einem Abrechnungsprofil finden Sie unter [Anzeigen der ausstehenden Nutzungsgebühren](#view-pending-usage-charges).

Nachdem Sie die Gebühren für ein Abonnement identifiziert haben, können Sie die Gebühren mithilfe der CSV-Datei zu Azure-Nutzung und -Gebühren analysieren.

Die folgende Abbildung zeigt die Liste der Abonnements im Azure-Portal.

![Screenshot der Liste der Azure-Abonnements für das Abrechnungsprofil](./media/billing-understand-your-bill-mca/mca-billing-profile-subscriptions-list-highlighted.png)

Filtern Sie in der CSV-Datei zu Azure-Nutzung und -Gebühren die Spalte **subscriptionName** nach **WA_Subscription**, um die detaillierten Nutzungsgebühren für „WA_Subscription“ anzuzeigen.

![Screenshot der nach Abonnement gefilterten Datei zu Azure-Nutzung und -Gebühren](./media/billing-understand-your-bill-mca/billing-usage-file-filtered-by-subscription.png)

## <a name="pay-your-bill"></a>Bezahlen Ihrer Rechnung

Die Anweisungen zum Bezahlen Ihrer Rechnung finden Sie unten auf der Rechnung. [Informationen zur Bezahlung](billing-mca-understand-your-invoice.md#how-to-pay)

Wenn Sie Ihre Rechnung bereits bezahlt haben, können Sie den Status der Zahlung im Azure-Portal auf der Seite „Rechnungen“ überprüfen.

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Überprüfen des Zugriffs auf eine Microsoft-Kundenvereinbarung
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-us"></a>Sie brauchen Hilfe? Wenden Sie sich an uns.

Wenn Sie Fragen haben oder Hilfe benötigen, [erstellen Sie eine Supportanfrage](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie die folgenden Artikel, um mehr über Ihre Rechnung und die detaillierten Nutzungsdaten zu erfahren:

- [Abrufen von Azure-Rechnungen und täglichen Nutzungsdaten](billing-download-azure-invoice-daily-usage-date.md)
- [Grundlegendes zu den Begriffen auf der Rechnung für eine Microsoft-Kundenvereinbarung](billing-mca-understand-your-invoice.md)
- [Grundlegendes zu den Begriffen in der CSV-Datei zu Azure-Nutzung und -Gebühren für eine Microsoft-Kundenvereinbarung](billing-mca-understand-your-usage.md)
