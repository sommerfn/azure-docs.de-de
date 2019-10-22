---
title: Grundlegendes zu Azure-Gebühren für externe Dienste | Microsoft-Dokumentation
description: Informieren Sie sich über die Gebührenabrechnung für externe Dienste in Azure (früher Marketplace).
author: jureid
manager: jureid
tags: billing
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/01/2019
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2803f8d659726ac95bcefa6191a816ed2b2dcb2d
ms.sourcegitcommit: 0576bcb894031eb9e7ddb919e241e2e3c42f291d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "72375408"
---
# <a name="understand-your-azure-external-services-charges"></a>Grundlegendes zu Azure-Gebühren für externe Dienste
Externe Dienste werden von Drittanbietern im Azure Marketplace veröffentlicht. Ein Beispiel für einen externen Dienst, den Sie in Azure erwerben können, der jedoch nicht von Microsoft veröffentlicht wird, ist SendGrid. Einige Microsoft-Produkte sind auch über den Azure Marketplace erhältlich.

## <a name="how-external-services-are-billed"></a>So werden externe Dienste in Rechnung gestellt

- Wenn Sie eine Microsoft-Kundenvereinbarung (MCA) oder eine Microsoft-Partnervereinbarung (MPA) haben, werden Ihre Dienste von Drittanbietern mit dem Rest Ihrer Azure-Dienste in Rechnung gestellt. [Überprüfen Sie Ihren Abrechnungskontotyp ](#check-billing-account-type), um zu überprüfen, ob Sie Zugriff auf eine MCA oder eine MPA haben.
- Wenn Sie keine MCA oder MPA haben, werden Ihre externen Dienste separat von Ihren Azure-Diensten in Rechnung gestellt.
- Jeder externe Dienst verwendet ein eigenes Abrechnungsmodell. Für einige Dienste wird die nutzungsbasierte Bezahlung verwendet, für andere gelten feste monatliche Gebühren.
- Die kostenlosen monatlichen Gutschriften können nicht für externe Dienste verwendet werden. Wenn Sie ein Azure-Abonnement mit [kostenlosen Gutschriften](https://azure.microsoft.com/pricing/spending-limits/) verwenden, können diese Gutschriften nicht zum Begleichen von Gebühren für externe Dienste genutzt werden. Beim Bereitstellen eines neuen externen Diensts oder einer neuen externen Ressource wird eine Warnung angezeigt:

    ![Warnung beim Erwerb von Marketplace-Angeboten](./media/billing-understand-your-azure-marketplace-charges/credit-warning.png)

<!-- ## View external service spending and history in the Azure portal
You can view a list of the external services that are on each subscription within the [Azure portal](https://portal.azure.com/):

1. Sign in to the [Azure portal](https://portal.azure.com/) as the account administrator.
2. In the Hub menu, select **Subscriptions**.

    ![Select Subscriptions in the Hub menu](./media/billing-understand-your-azure-marketplace-charges/sub-button.png)
3. In the **Subscriptions** blade, select the subscription that you want to view, and then select **External services**.

    ![Select a subscription in the billing blade](./media/billing-understand-your-azure-marketplace-charges/select-sub-external-services.png)
4. You should see each of your external service orders, the publisher name, service tier you bought, name you gave the resource, and the current order status. To see past bills, select an external service.

    ![Select an external service](./media/billing-understand-your-azure-marketplace-charges/external-service-blade2.png)
5. From here, you can view past bill amounts including the tax breakdown.

    ![View external services billing history](./media/billing-understand-your-azure-marketplace-charges/billing-overview-blade.png) -->

## <a name="view-and-download-invoices-for-external-services"></a>Anzeigen und Herunterladen von Rechnungen für externe Dienste

Wenn Sie eine Microsoft-Kundenvereinbarung (MCA) oder eine Microsoft-Partnervereinbarung (MPA) haben, werden Ihre Dienste von Drittanbietern mit dem Rest Ihrer Azure-Dienste in Rechnung gestellt. [Überprüfen Sie Ihren Abrechnungskontotyp](#check-billing-account-type), um zu überprüfen, ob Sie Zugriff auf eine MCA oder eine MPA haben. Wenn dies der Fall ist, erfahren Sie unter [Anzeigen und Herunterladen der Microsoft Azure-Rechnung](billing-download-azure-invoice.md), wie Sie Ihre Drittanbietergebühren einsehen können.

Wenn Sie keine MCA oder MPA haben, erhalten Sie separate Rechnungen über Drittanbietergebühren. Um Ihre Azure Marketplace-Rechnungen über das Azure-Portal anzuzeigen und herunterzuladen, befolgen Sie diese Schritte:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Suchen Sie nach **Kostenverwaltung + Abrechnung**.
1. Wählen Sie die Option **Rechnungen** im Menü auf der linken Seite aus.
1. Klicken Sie auf die Registerkarte **Azure Marketplace und Reservierungen**.  ![Abbildung der Registerkarte „Azure Marketplace und Reservierungen“](./media/billing-understand-your-azure-marketplace-charges/invoice-tabs.png)
1. Wählen Sie in der Abonnementdropdownliste das Abonnement aus, das die externen Dienste enthält, zu denen Sie Rechnungen anzeigen möchten.

## <a name="external-spending-for-ea-customers"></a>Externe Ausgaben für EA-Kunden

EA-Kunden können im EA-Portal die Ausgaben für externe Dienste anzeigen und Berichte herunterladen. Informationen zum Einstieg finden Sie unter [Azure Marketplace for EA Customers](https://ea.azure.com/helpdocs/azureMarketplace) (Azure Marketplace für EA-Kunden).

## <a name="manage-payment-for-external-services"></a>Verwalten der Zahlung für externe Dienste

Wenn Sie einen externen Dienst erwerben, wählen Sie ein Azure-Abonnement für die Ressource aus. Die Zahlungsmethode des ausgewählten Azure-Abonnements wird zur Zahlungsmethode für den externen Dienst. Um die Zahlungsmethode für einen externen Dienst zu ändern, müssen Sie [die Zahlungsmethode des Azure-Abonnements ändern](billing-how-to-change-credit-card.md), an das dieser externe Dienst gebunden ist. Sie können mit folgenden Schritten das Abonnement ermitteln, an das Ihre Bestellung eines externen Diensts gebunden ist:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Klicken Sie im linken Navigationsmenü auf **Alle Ressourcen**.
     ![Screenshot des Menüelements „Alle Ressourcen“](./media/billing-understand-your-azure-marketplace-charges/all-resources.png)
1. Suchen Sie Ihren externen Dienst.
1. Achten Sie in der Spalte **Abonnement** auf den Namen des Abonnements.
    ![Screenshot des Abonnementnamens für die Ressource](./media/billing-understand-your-azure-marketplace-charges/sub-selected.png)
1. Klicken Sie auf den Namen des Abonnements, und [aktualisieren Sie die aktive Zahlungsmethode](billing-how-to-change-credit-card.md).

<!-- Update your payment methods for external service orders from the [Account Center](https://account.windowsazure.com/).

> [!NOTE]
> If you purchased your subscription with a Work or School account, [contact support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) to make changes to your payment method.

1. Sign in to the [Account Center](https://account.windowsazure.com/) and [navigate to the **marketplace** tab](https://account.windowsazure.com/Store)

    ![Select marketplace in the account center](./media/billing-understand-your-azure-marketplace-charges/select-marketplace.png)
2. Select the external service you want to manage

    ![Select the external service you want to manage](./media/billing-understand-your-azure-marketplace-charges/select-ext-service.png)
3. Click **Change payment method** on the right side of the page. This link brings you to a different portal to manage your payment method.

    ![Order summary](./media/billing-understand-your-azure-marketplace-charges/change-payment.PNG)
4. Click **Edit info** and follow instructions to update your payment information.

    ![Select edit info](./media/billing-understand-your-azure-marketplace-charges/edit-info.png) -->

## <a name="cancel-an-external-service-order"></a>Kündigen eines externen Diensts
Wenn Sie einen externen Dienst kündigen möchten, löschen Sie die Ressource im [Azure-Portal](https://portal.azure.com).

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Klicken Sie im linken Navigationsmenü auf **Alle Ressourcen**.
    ![Screenshot des Menüelements „Alle Ressourcen“](./media/billing-understand-your-azure-marketplace-charges/all-resources.png)
1. Suchen Sie Ihren externen Dienst.
1. Aktivieren Sie das Kontrollkästchen neben der Ressource, die Sie löschen möchten.
1. Wählen Sie in der Befehlsleiste die Option **Löschen** aus.
    ![Screenshot der Schaltfläche „Löschen“](./media/billing-understand-your-azure-marketplace-charges/delete-button.png)
1. Geben Sie im Bestätigungsblatt *Ja* ein.
    ![Löschen der Ressource](./media/billing-understand-your-azure-marketplace-charges/delete-resource.PNG)
1. Klicken Sie auf **Löschen**.

## <a name="check-billing-account-type"></a>Überprüfen des Abrechnungskontotyps
[!INCLUDE [billing-check-account-type](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-us"></a>Sie brauchen Hilfe? Wenden Sie sich an uns.

Wenn Sie Fragen haben oder Hilfe benötigen, [erstellen Sie eine Supportanfrage](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Nächste Schritte
- [Beginn der Kostenanalyse](../cost-management/quick-acm-cost-analysis.md)
