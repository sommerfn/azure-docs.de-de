---
title: Zahlen für Azure-Abonnements auf Rechnung
description: Es wird beschrieben, wie Azure-Abonnements auf Rechnung bezahlt werden.
author: bandersmsft
manager: jureid
tags: billing
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/01/2019
ms.author: banders
ms.openlocfilehash: 90c7014e3c22ac7186854d5c01b911aa630d20dd
ms.sourcegitcommit: 4b5dcdcd80860764e291f18de081a41753946ec9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/03/2019
ms.locfileid: "68774659"
---
# <a name="pay-for-your-azure-subscription-by-invoice"></a>Zahlen für Ihr Azure-Abonnement auf Rechnung

Wenn Sie zur Zahlung auf Rechnung wechseln, bedeutet dies, dass Sie Ihre Rechnung innerhalb von 30 Tagen nach dem Rechnungsdatum per Scheck bzw. Überweisung bezahlen. Um Ihr Azure-Abonnement auf Rechnung bezahlen zu können, senden Sie einen entsprechenden Antrag an den Azure-Support. Nachdem Ihr Antrag genehmigt wurde, können Sie im [Azure-Portal](https://portal.azure.com) zur Zahlung per Rechnung (Scheck/Überweisung) wechseln.

> [!IMPORTANT]
> * Die Zahlung per Rechnung (Scheck/Überweisung) ist nur für Geschäftskonten verfügbar.
> * Sie müssen alle ausstehende Gebühren bezahlen, bevor Sie zur Zahlung auf Rechnung wechseln können.

## <a name="request-to-pay-by-invoice"></a>Anfordern der Zahlung auf Rechnung

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an. Wählen Sie **Hilfe und Support** > **Neue Supportanfrage**.

    ![Link „Hilfe und Support“](./media/billing-how-to-pay-by-invoice/help-and-support.png)

2. Wählen Sie als **Problemtyp** die Option **Abrechnung** aus. Der *Problemtyp* ist die Kategorie der Supportanfrage. Wählen Sie das Abonnement aus, für das Sie per Rechnung bezahlen möchten, wählen Sie einen Supportplan aus, und klicken Sie dann auf **Weiter**.

3. Wählen Sie **Zahlung** als **Problemtyp** aus. Der *Problemtyp* ist die Unterkategorie der Supportanfrage.

4. Wählen **Umstellen auf die Bezahlung per Rechnung** als **Untertyp des Problems** aus.

5. Geben Sie in das Feld **Details** die folgenden Informationen ein, und klicken Sie dann auf **Weiter**.

         New or existing customer:
         If existing, current payment method:
         Order ID (requesting for invoice option):
         Account Admins Live ID (or Org ID) (should be company domain):
         Commerce Account ID:
         Company Name (as registered under VAT or Government Website):
         Company Address (as registered under VAT or Government Website):
         Company Website:
         Country:
         TAX ID/ VAT ID:
         Company Established on (Year):
         Any prior business with Microsoft:
         Contact Name:
         Contact Phone:
         Contact Email:
         Justification on why you prefer Invoice option over credit card:

        For cores increase, provide the following additional information:

         (Old quota) Existing Cores:
         (New quota) Requested cores:
         Specific region & series of Subscription:

    - Der **Firmenname** und die **Firmenadresse** sollten mit den Informationen übereinstimmen, die Sie für das Azure-Konto bereitgestellt haben. Einzelheiten zum Anzeigen oder Aktualisieren der Informationen finden Sie unter [Ändern von Profilinformationen für Ihr Azure-Konto](billing-how-to-change-azure-account-profile.md).
    - Sie müssen im Azure Portal die Informationen zu Ihrem Rechnungskontakt hinzufügen, bevor das Guthabenlimit genehmigt werden kann. Die Kontaktdetails sollten im Zusammenhang mit der Kreditorenbuchhaltung oder der Finanzabteilung des Unternehmens stehen. Wechseln Sie zum [Azure-Kontocenter](https://account.azure.com/Profile), wenn Sie die Informationen zum Rechnungskontakt aktualisieren möchten.

6. Überprüfen Sie Ihre Kontaktdaten und Ihre bevorzugte Kontaktmethode, und klicken Sie dann auf **Erstellen**.

Wenn Sie aufgrund der Höhe des Guthabens, das Sie benötigen, eine Guthabenprüfung durchführen müssen, senden wir Ihnen einen entsprechenden Antrag.

## <a name="switch-to-invoice-pay-checkwire-transfer"></a>Wechseln zur Zahlung per Rechnung (Scheck/Überweisung)

Nachdem Sie die Genehmigung für die Zahlung per Rechnung erhalten haben, können Sie im Azure-Portal zur Zahlung per Rechnung (Scheck/Überweisung) wechseln.

Wenn Sie über ein Microsoft Online Services-Programmkonto verfügen, können Sie Ihr Azure-Abonnement auf Scheck/Überweisung umstellen. Falls Sie über eine Microsoft-Kundenvereinbarung verfügen, können Sie Ihr Abrechnungsprofil auf Scheck/Überweisung umstellen. [Informationen zur Überprüfung des Kontotyps](#check-access-to-a-microsoft-customer-agreement).

### <a name="switch-azure-subscription-to-checkwire-transfer"></a>Umstellen des Azure-Abonnements auf Scheck/Überweisung

Führen Sie die unten angegebenen Schritte aus, um Ihr Azure-Abonnement auf die Zahlung per Rechnung (Scheck/Überweisung) umzustellen. **Wenn Sie die Umstellung auf die Zahlung per Rechnung (Scheck/Überweisung) durchgeführt haben, können Sie nicht mehr zurück zur Kreditkarte wechseln**.

1. Melden Sie sich als Kontoadministrator beim [Azure-Portal](https://portal.azure.com) an.
1. Suchen Sie nach **Kostenverwaltung + Abrechnung**.

    ![Screenshot: Suche](./media/billing-how-to-pay-by-invoice/search.png)

1. Wählen Sie das Abonnement aus, für das Sie zur Zahlung auf Rechnung wechseln möchten.
1. Wählen Sie die Option **Zahlungsmethoden**.
1. Klicken Sie in der Befehlsleiste auf die Schaltfläche **Zahlung per Rechnung**.

    ![Screenshot der Schaltfläche „Zahlung per Rechnung“](./media/billing-how-to-pay-by-invoice/pay-by-invoice.png)

### <a name="switch-billing-profile-to-checkwire-transfer"></a>Umstellen des Abrechnungsprofils auf Scheck/Überweisung

Führen Sie die unten angegebenen Schritte aus, um ein Abrechnungsprofil auf Scheck/Überweisung umzustellen. Beachten Sie hierbei Folgendes: Nur die Person, die sich für Azure registriert hat, kann die Standardzahlungsmethode eines Abrechnungsprofils ändern.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Suchen Sie nach **Kostenverwaltung + Abrechnung**.
1. Klicken Sie im Menü auf der linken Seite auf **Abrechnungsprofile**.

    ![Screenshot: Abrechnungsprofil im Menü](./media/billing-how-to-pay-by-invoice/billing-profile.png)

1. Wählen Sie ein Abrechnungsprofil aus.
1. Wählen Sie im Menü links die Option **Zahlungsmethoden**.

   ![Screenshot: Zahlungsmethoden im Menü](./media/billing-how-to-pay-by-invoice/billing-profile-payment-methods.png)

1. Klicken Sie auf das blaue Banner mit dem Hinweis, dass Sie per Scheck/Überweisung bezahlen können.

    ![Screenshot: Blaues Banner für die Umstellung auf Scheck/Überweisung](./media/billing-how-to-pay-by-invoice/customer-led-switch-to-invoice.png)

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Überprüfen des Zugriffs auf eine Microsoft-Kundenvereinbarung
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-us"></a>Sie brauchen Hilfe? Wenden Sie sich an uns.

Wenn Sie Fragen haben oder Hilfe benötigen, [erstellen Sie eine Supportanfrage](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Nächste Schritte

- Aktualisieren Sie bei Bedarf im [Azure-Kontocenter](https://account.azure.com/Profile) die Informationen zu Ihrem Rechnungskontakt.
