---
title: Erstellen von Abschnitten in Ihrer Rechnung zum Organisieren von Kosten – Azure
description: Hier erfahren Sie, wie Sie Kosten mithilfe von Rechnungsabschnitten organisieren.
author: amberbhargava
manager: amberb
editor: banders
tags: billing
ms.service: billing
ms.topic: conceptual
ms.workload: na
ms.date: 10/01/2019
ms.author: banders
ms.openlocfilehash: 77dd8a5e54697a37a2039238ecdedb3e2a2fa326
ms.sourcegitcommit: 0576bcb894031eb9e7ddb919e241e2e3c42f291d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "72375474"
---
# <a name="create-sections-on-your-invoice-to-organize-your-costs"></a>Erstellen von Abschnitten in Ihrer Rechnung zum Organisieren von Kosten

Erstellen Sie Abschnitte in Ihrer Rechnung, um Ihre Kosten nach Abteilung, nach Entwicklungsumgebung oder gemäß den Organisationsanforderungen zu organisieren. Erteilen Sie dann anderen Benutzern die Berechtigung zum Erstellen von Azure-Abonnements, die für den Abschnitt abgerechnet werden. Alle Nutzungsgebühren und Einkäufe für die Abonnements werden dann dem Abschnitt in Rechnung gestellt. Sie können die gesamten Gebühren für den Abschnitt in Ihrer Rechnung im Azure-Portal anzeigen, oder überprüfen Sie die Gebühren in der Azure-Kostenanalyse. Weitere Informationen finden Sie unter [Anzeigen von Transaktionen nach Rechnungsabschnitten](billing-mca-understand-your-bill.md#view-transactions-by-invoice-sections).

Dieser Artikel bezieht sich auf ein Abrechnungskonto für eine Microsoft-Kundenvereinbarung. [Überprüfen Sie, ob Sie Zugriff auf eine Microsoft-Kundenvereinbarung haben](#check-access-to-a-microsoft-customer-agreement).

## <a name="create-an-invoice-section-in-the-azure-portal"></a>Erstellen eines Rechnungsabschnitts im Azure-Portal

Um einen Rechnungsabschnitt erstellen zu können, müssen Sie ein **Besitzer des Abrechnungsprofils** oder **Mitwirkender am Abrechnungsprofil** sein. Weitere Informationen finden Sie unter [Verwalten von Rechnungsabschnitten für das Abrechnungsprofil](billing-understand-mca-roles.md#manage-invoice-sections-for-billing-profile).

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Suchen Sie nach **Kostenverwaltung + Abrechnung**.

   ![Screenshot, der die Suche im Azure-Portal zeigt](./media/billing-mca-section-invoice/billing-search-cost-management-billing.png)

3. Wählen Sie im linken Bereich die Option **Rechnungsabschnitte** aus. Abhängig von Ihren Zugriffsberechtigungen müssen Sie möglicherweise zuerst ein Abrechnungsprofil oder ein Abrechnungskonto und dann **Rechnungsabschnitte** auswählen.

   ![Screenshot der Rechnungsabschnittsliste](./media/billing-mca-section-invoice/mca-select-invoice-sections.png)

4. Wählen Sie oben auf der Seite die Option **Hinzufügen** aus.

5. Geben Sie einen Namen für den Rechnungsabschnitt ein, und wählen Sie ein Abrechnungsprofil aus. Der Abschnitt wird auf der Rechnung dieses Abrechnungsprofils mit der Nutzung der einzelnen Abonnements, die Sie dem Abschnitt zugewiesen haben, sowie den zugehörigen Käufen angezeigt. 

   ![Screenshot der Seite „Rechnungsabschnitt erstellen“](./media/billing-mca-section-invoice/mca-create-invoice-section.png)

6. Klicken Sie auf **Erstellen**.

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Überprüfen des Zugriffs auf eine Microsoft-Kundenvereinbarung
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-support"></a>Sie brauchen Hilfe? Support kontaktieren

[Wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), falls Sie weitere Hilfe benötigen, um das Problem schnell beheben zu lassen.

## <a name="next-steps"></a>Nächste Schritte

- [Erstellen eines zusätzlichen Azure-Abonnements für eine Microsoft-Kundenvereinbarung](billing-mca-create-subscription.md)
- [Verwalten von Abrechnungsrollen im Azure-Portal](billing-understand-mca-roles.md#manage-billing-roles-in-the-azure-portal)
- [Anfordern des Abrechnungsbesitzes von Azure-Abonnements von Benutzern in anderen Abrechnungskonten](billing-mca-request-billing-ownership.md)
