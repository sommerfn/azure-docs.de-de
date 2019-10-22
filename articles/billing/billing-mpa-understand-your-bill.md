---
title: Grundlegendes zu den Gebühren auf der Rechnung für eine Microsoft-Partnervereinbarung – Azure
description: Informationen zum Lesen und Verstehen der Gebühren auf Ihrer Rechnung.
author: jureid
manager: jureid
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/10/2019
ms.author: banders
ms.openlocfilehash: 579a052f8bde780ac33de85c5a08d9b3e79d3096
ms.sourcegitcommit: 12de9c927bc63868168056c39ccaa16d44cdc646
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "72515765"
---
# <a name="understand-charges-on-your-microsoft-partner-agreement-invoice"></a>Grundlegendes zu den Gebühren auf der Rechnung für eine Microsoft-Partnervereinbarung

 Im Abrechnungskonto für eine Microsoft-Partnervereinbarung wird jeden Monat eine Rechnung für jedes Abrechnungsprofil generiert. Die Rechnung enthält alle Kundengebühren des vorherigen Monats. Durch Analysieren der einzelnen Transaktionen im Azure-Portal können Sie die Gebühren auf Ihrer Rechnung verstehen. Außerdem können Sie Ihre Rechnungen im Azure-Portal anzeigen. Weitere Informationen finden Sie unter [Anzeigen und Herunterladen der Microsoft Azure-Rechnung](billing-download-azure-invoice.md).

Dieser Artikel bezieht sich auf ein Abrechnungskonto für eine Microsoft-Partnervereinbarung. [Überprüfen Sie, ob Sie Zugriff auf eine Microsoft-Partnervereinbarung haben](#check-access-to-a-microsoft-partner-agreement).

## <a name="view-transactions-for-an-invoice-in-the-azure-portal"></a>Anzeigen von Transaktionen für eine Rechnung im Azure-Portal

1. Melden Sie sich beim [Azure-Portal](https://www.azure.com) an.

2. Suchen Sie nach *Kostenverwaltung + Abrechnung*.

   <!--  ![Screenshot that shows Azure portal search for cost management + billing](./media/billing-understand-your-bill-mca/billing-search-cost-management-billing.png) -->

3. Wählen Sie links auf der Seite die Option **Alle Transaktionen** aus. Abhängig von Ihren Zugriffsberechtigungen müssen Sie möglicherweise zuerst ein Abrechnungskonto, ein Abrechnungsprofil oder einen Kunden und dann **Alle Transaktionen** auswählen.

4. Auf der Seite „Alle Transaktionen“ werden die folgenden Informationen angezeigt:

    <!-- ![Screenshot that shows the billed transactions list](./media/billing-mpa-understand-your-bill/mpa-billing-profile-all-transactions.png) -->

    |Column  |Definition  |
    |---------|---------|
    |Date     | Das Datum der Transaktion  |
    |Rechnungs-ID     | Der Bezeichner für die Rechnung, auf der die Transaktion in Rechnung gestellt wurde. Wenn Sie eine Supportanfrage senden, teilen Sie dem Azure-Support die ID mit, um die Bearbeitung Ihrer Supportanfrage zu beschleunigen. |
    |Transaktionstyp     |  Der Typ der Transaktion, z. B. Erwerb, Stornierung und Nutzungsgebühren  |
    |Produktfamilie     | Die Kategorie des Produkts, z. B. „Compute“ für virtuelle Computer oder „Datenbank“ für Azure SQL-Datenbank|
    |Produkt-SKU     | Ein eindeutiger Code, der die Instanz Ihres Produkts identifiziert |
    |Amount     |  Der Betrag der Transaktion      |
    |Abrechnungsprofil     | Die Transaktion wird auf der Rechnung dieses Abrechnungsprofils angezeigt. |

5. Suchen Sie nach der Rechnungs-ID, um die Transaktionen für die Rechnung zu filtern.

## <a name="review-pending-charges-to-estimate-your-next-invoice"></a>Überprüfen der ausstehenden Gebühren zum Schätzen Ihrer nächsten Rechnung

Gebühren werden geschätzt und gelten bis zur Rechnungsstellung als ausstehend. Sie können ausstehende Gebühren für das Abrechnungsprofil Ihrer Microsoft-Partnervereinbarung im Azure-Portal anzeigen, um Ihre nächste Rechnung einschätzen zu können. Ausstehende Gebühren sind Schätzungen und enthalten keine Steuern. Die tatsächlichen Gebühren in Ihrer nächsten Rechnung weichen von den ausstehenden Gebühren ab.

### <a name="view-pending-transactions"></a>Anzeigen von ausstehenden Transaktionen

Nachdem Sie die ausstehenden Gebühren identifiziert haben, können Sie die Gebühren nachvollziehen und verstehen, indem Sie die einzelnen Transaktionen analysieren, die zur Entstehung dieser Gebühren beigetragen haben. Zu diesem Zeitpunkt werden die ausstehenden Nutzungsgebühren nicht auf der Seite „Alle Transaktionen“ angezeigt. Sie können die ausstehenden Nutzungsgebühren auf der Azure-Abonnementseite anzeigen. Weitere Informationen finden Sie unter [Anzeigen der ausstehenden Nutzungsgebühren](#view-pending-usage-charges).

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Suchen Sie nach *Kostenverwaltung + Abrechnung*.

   <!-- ![Screenshot that shows Azure portal search for cost management + billing](./media/billing-understand-your-bill-mca/billing-search-cost-management-billing.png) -->

3. Wählen Sie ein Abrechnungsprofil aus. Abhängig von Ihren Zugriffsberechtigungen müssen Sie möglicherweise ein Abrechnungskonto auswählen. Wählen Sie im Abrechnungskonto die Option **Abrechnungsprofile** aus, und wählen Sie dann ein Abrechnungsprofil aus.

4. Wählen Sie links auf der Seite die Option **Alle Transaktionen** aus.

5. Suchen Sie nach *ausstehend*. Verwenden Sie den Filter **Zeitraum**, um die ausstehenden Gebühren für den aktuellen oder letzten Monat anzuzeigen.

   <!-- ![Screenshot that shows the pending transactions list](./media/billing-mpa-understand-your-bill/mpa-billing-profile-pending-transactions.png) -->

### <a name="view-pending-charges-by-customer"></a>Ausstehende Gebühren nach Kunden anzeigen

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Suchen Sie nach *Kostenverwaltung + Abrechnung*.

3. Wählen Sie ein Abrechnungsprofil aus. Abhängig von Ihren Zugriffsberechtigungen müssen Sie möglicherweise ein Abrechnungskonto auswählen. Wählen Sie im Abrechnungskonto die Option **Abrechnungsprofile** aus, und wählen Sie dann ein Abrechnungsprofil aus.

4. Wählen Sie links auf der Seite **Kunden** aus.

    <!-- ![screenshot of billing profile customers list](./media/billing-mpa-understand-your-bill/mpa-billing-profile-customers.png) -->

5. Auf der Seite „Kunden“ werden die Gebühren des aktuellen und des letzten Monats für jeden Kunden angezeigt, der dem Abrechnungsprofil zugeordnet ist. Die Gebühren für den bisherigen Kalendermonat sind die ausstehenden Gebühren für den aktuellen Monat. Sie werden abgerechnet, wenn die Rechnung für den Monat generiert wird. Wenn die Rechnung für den letzten Monat noch nicht generiert wurde, sind die Gebühren im letzten Monat auch ausstehende Gebühren.

### <a name="view-pending-usage-charges"></a>Anzeigen der ausstehenden Nutzungsgebühren

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Suchen Sie nach *Kostenverwaltung + Abrechnung*.

3. Wählen Sie ein Abrechnungsprofil aus. Abhängig von Ihren Zugriffsberechtigungen müssen Sie möglicherweise ein Abrechnungskonto auswählen. Wählen Sie im Abrechnungskonto die Option **Abrechnungsprofile** aus, und wählen Sie dann ein Abrechnungsprofil aus.

4. Wählen Sie links auf der Seite **Azure-Abonnements** aus.

5. Auf der Azure-Abonnementseite werden die Gebühren des aktuellen und des letzten Monats für jedes Abonnement im Abrechnungsprofil angezeigt. Die Gebühren für den bisherigen Kalendermonat sind die ausstehenden Gebühren für den aktuellen Monat. Sie werden abgerechnet, wenn die Rechnung für den Monat generiert wird. Wenn die Rechnung für den letzten Monat noch nicht generiert wurde, sind die Gebühren im letzten Monat auch ausstehende Gebühren.

<!--     ![Screenshot that shows the Azure subscriptions list for MPA billing profile](./media/billing-mpa-understand-your-bill/mpa-billing-profile-subscriptions-list.png) -->

## <a name="analyze-your-azure-usage-charges"></a>Analysieren Ihrer Azure-Nutzungsgebühren

Verwenden Sie die CSV-Datei zu Azure-Nutzung und -Gebühren, um Ihre nutzungsbasierten Gebühren zu analysieren. Sie können die [Azure-Nutzung und -Gebühren als CSV-Datei aus dem Azure-Portal herunterladen](billing-download-azure-daily-usage.md).

Sie können die Datei zu Azure-Nutzung und -Gebühren filtern, um die Nutzungsgebühren für jedes in der Rechnungs-PDF aufgelistete Produkt abzustimmen. Um detaillierte Nutzungsgebühren für ein bestimmtes Produkt anzuzeigen, filtern Sie die Spalte **product** in der CSV-Datei zu Azure-Nutzung und -Gebühren, um nur den Namen dieses Produkts einzuschließen.

Sie können auch die Spalte **customerName**  in der CSV-Datei zu Azure-Nutzung und -Gebühren filtern, um die täglichen Nutzungsgebühren für die einzelnen Kunden anzuzeigen. Wenn Sie die täglichen Nutzungsgebühren nach Azure-Abonnement anzeigen möchten, filtern Sie die Spalte **subscriptionName** .


## <a name="pay-your-bill"></a>Bezahlen Ihrer Rechnung

Die Anweisungen zum Bezahlen Ihrer Rechnung finden Sie unten auf der Rechnung. Sie können innerhalb von 60 Tagen nach dem Rechnungsdatum per Überweisung oder per Scheck bezahlen.

Wenn Sie Ihre Rechnung bereits bezahlt haben, können Sie den Status der Zahlung im Azure-Portal auf der Seite „Rechnungen“ überprüfen.

## <a name="check-access-to-a-microsoft-partner-agreement"></a>Überprüfen des Zugriffs auf eine Microsoft-Partnervereinbarung
[!INCLUDE [billing-check-mpa](../../includes/billing-check-mpa.md)]

## <a name="need-help-contact-us"></a>Sie brauchen Hilfe? Wenden Sie sich an uns.

Wenn Sie Fragen haben oder Hilfe benötigen, [erstellen Sie eine Supportanfrage](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie die folgenden Artikel, um mehr über Ihre Rechnung und die detaillierten Nutzungsdaten zu erfahren:
