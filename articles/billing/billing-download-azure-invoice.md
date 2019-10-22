---
title: Anzeigen und Herunterladen der Microsoft Azure-Rechnung
description: Erfahren Sie, wie Sie Ihre Microsoft Azure-Rechnung anzeigen und herunterladen.
keywords: Rechnung, Rechnungsdownload, Azure-Rechnung, Azure-Nutzung
author: bandersmsft
manager: jureid
tags: billing
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/01/2019
ms.author: banders
ms.openlocfilehash: 80ec40a7411a370460d663084f9f7034b28e1a2e
ms.sourcegitcommit: 0576bcb894031eb9e7ddb919e241e2e3c42f291d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "72375755"
---
# <a name="view-and-download-your-microsoft-azure-invoice"></a>Anzeigen und Herunterladen der Microsoft Azure-Rechnung

Für die meisten Abonnements können Sie die Rechnung aus dem [Azure-Portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) herunterladen oder per E-Mail zugeschickt bekommen. Wenn Sie Azure-Kunde mit einem Enterprise Agreement (EA-Kunde) sind, können Sie die Rechnungen Ihrer Organisation nicht herunterladen. Rechnungen werden an den Benutzer gesendet, der für den Empfang von Rechnungen für die Registrierung festgelegt wurde.

Nur bestimmte Rollen sind zum Anzeigen von Rechnungen berechtigt. Zu diesen Rollen zählen beispielsweise der Kontoadministrator und der Unternehmensadministrator. Weitere Informationen zum Zugriff auf Abrechnungsinformationen finden Sie unter [Verwalten des Zugriffs auf die Azure-Abrechnung mithilfe von Rollen](billing-manage-access.md).

Wenn Sie über eine Microsoft-Kundenvereinbarung (MCA) verfügen, benötigen Sie eine der folgenden Rollen, um Ihre Rechnungen zu erhalten:

- Besitzer des Abrechnungsprofils
- Mitwirkender
- Leser
- Rechnungs-Manager

Wenn Sie über eine Microsoft-Partnervereinbarung (MPA) verfügen, müssen Sie der globale Administrator oder Administrator-Agent in der Partnerorganisation sein, um Azure-Rechnungen anzuzeigen und herunterzuladen. [Überprüfen Sie Ihren Abrechnungskontotyp](#check-your-billing-account-type), um zu sehen, welche Berechtigungen Sie benötigen. 

<!-- For more information about billing roles for Microsoft Customer Agreements, see [Billing profile roles and tasks](billing-understand-mca-roles.md#billing-profile-roles-and-tasks). -->

## <a name="noinvoice"></a> Mögliche Gründe für nicht erhaltene Rechnungen

Mehrere Gründe können dafür ausschlaggebend sein, dass Sie keine Rechnung sehen:

- Es sind weniger als 30 Tage seit dem Abschluss Ihres Azure-Abonnements vergangen.

- Die Abrechnung von Azure erfolgt am Ende Ihres Rechnungszeitraums. Unter Umständen wurde also noch keine Rechnung generiert. Warten Sie bis zum Ende des Abrechnungszeitraums.

- Sie haben keine Berechtigung zur Ansicht von Rechnungen. Wenn Sie über eine MCA oder MPA verfügen, müssen Sie der Besitzer des Abrechnungsprofils, Mitwirkender, Benutzer mit Leseberechtigung oder Rechnungs-Manager sein. Für andere Abonnements werden ältere Rechnungen nur angezeigt, wenn Sie der Kontoadministrator sind. Weitere Informationen zum Zugriff auf Abrechnungsinformationen finden Sie unter [Verwalten des Zugriffs auf die Azure-Abrechnung mithilfe von Rollen](billing-manage-access.md).

- Falls Sie über eine kostenlose Testversion oder über ein monatliches Guthaben für Ihr Abonnement verfügen, erhalten Sie nur dann eine Rechnung, wenn Sie das monatliche Guthaben überschreiten. Wenn Sie über eine Microsoft-Kundenvereinbarung oder eine Microsoft-Partnervereinbarung verfügen, erhalten Sie immer eine Rechnung. 

## <a name="download-invoices-in-the-azure-portal"></a>Herunterladen von Rechnungen im Azure-Portal

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Suchen Sie nach *Kostenverwaltung + Abrechnung*.
1. Abhängig von Ihren Zugriffsberechtigungen müssen Sie möglicherweise ein Abrechnungskonto oder ein Abrechnungsprofil auswählen.
1. Wählen Sie im Menü auf der linken Seite die Option **Rechnungen** unter **Abrechnung** aus.
1. Suchen Sie im Rechnungsraster nach der Zeile der Rechnung, die Sie herunterladen möchten.
1. Klicken Sie am Ende der Zeile auf das Downloadsymbol oder die Auslassungspunkte (`...`).
1. Wählen Sie im Downloadmenü **Rechnung** aus.

Weitere Informationen über Ihre Rechnung finden Sie unter [Erläuterungen zur Rechnung für Microsoft Azure](billing-understand-your-bill.md). Hilfreiche Informationen zur Verwaltung Ihrer Kosten finden Sie unter [Vermeiden unerwarteter Kosten bei der Azure-Abrechnung und -Kostenverwaltung](billing-getting-started.md).

## <a name="get-your-subscriptions-invoices-in-email"></a>Abrufen der Rechnungen für Abonnements per E-Mail

Sie können die entsprechende Option aktivieren und zusätzliche Empfänger konfigurieren, die Ihre Azure-Rechnung in einer E-Mail erhalten. Diese Funktion ist für bestimmte Abonnements möglicherweise nicht verfügbar, z.B. für Supportangebote, Enterprise Agreements oder Azure in Open.

1. Wählen Sie auf der Seite [Abonnements](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) Ihr Abonnement aus. Wählen Sie jedes Abonnement aus, das Sie besitzen. Klicken Sie auf **Rechnungen** und dann auf **Rechnung per E-Mail**.

    ![Screenshot mit dem Ablauf der Aktivierung](./media/billing-download-azure-invoice-daily-usage-date/InvoicesDeepLink.PNG)

2. Klicken Sie auf **Aktivieren**, und stimmen Sie den Bedingungen zu.

    ![Screenshot mit dem Ablauf von Schritt 2 der Aktivierung](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep2.PNG)

3. Nachdem Sie die Vereinbarung akzeptiert haben, können Sie zusätzliche Empfänger konfigurieren. Wenn ein Empfänger entfernt wird, wird die E-Mail-Adresse nicht mehr gespeichert. Wenn Sie Ihre Meinung ändern, müssen Sie ihn erneut hinzufügen.

    ![Screenshot mit dem Ablauf von Schritt 3 der Aktivierung](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep3.PNG)

Wenn Sie keine E-Mail erhalten, nachdem Sie diese Schritte befolgt haben, stellen Sie sicher, dass Ihre E-Mail-Adresse in den [Kommunikationseinstellungen auf Ihrem Profil](https://account.windowsazure.com/profile) richtig ist.

## <a name="opt-out-of-getting-your-subscriptions-invoices-in-email"></a>Kündigen des Erhalts der Rechnungen für Abonnements per E-Mail

Führen Sie zum Deaktivieren des Erhalts der Rechnung per E-Mail die obigen Schritte aus, und klicken Sie auf **Abonnement für Rechnungen per E-Mail kündigen**. Dadurch werden alle E-Mail-Adressen entfernt, an die Rechnungen gesendet werden sollten. Sie können die Empfänger neu konfigurieren, wenn Sie die Rechnungen wieder per E-Mail empfangen möchten.

 ![Screenshot mit dem Ablauf der Kündigung](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep4.PNG)

<!-- Does following section apply to MPA too? -->
## <a name="get-your-microsoft-customer-agreement-invoices-in-email"></a>Abrufen von Rechnungen für die Microsoft-Kundenvereinbarung per E-Mail

Wenn Sie eine Microsoft-Kundenvereinbarung abgeschlossen haben, können Sie Ihre Rechnungen per E-Mail empfangen. Alle Benutzer mit den Rollen „Besitzer des Abrechnungsprofils“, „Mitwirkender am Abrechnungsprofil“, „Benutzer mit Leseberechtigung für das Abrechnungsprofil“ und „Rechnungs-Manager“ erhalten die Rechnungen dann per E-Mail. Leseberechtigte können die Einstellungen für den Empfang der Rechnungen per E-Mail aber nicht bearbeiten.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Suchen Sie nach **Kostenverwaltung + Abrechnung**.
1. Wählen Sie ein Abrechnungsprofil aus. Abhängig von Ihren Zugriffsberechtigungen müssen Sie möglicherweise zuerst ein Abrechnungskonto auswählen.
1. Wählen Sie unter **Einstellungen** die Option **Eigenschaften** aus.
1. Wählen Sie unter **Rechnung per E-Mail** die Option **Einstellung für Rechnung per E-Mail aktualisieren** aus.

    ![Screenshot mit Eigenschaften für Rechnungs-E-Mails](./media/billing-download-azure-invoice/billingprofile-email.png)

1. Wählen Sie **Aktivieren** aus.
1. Klicken Sie auf **Aktualisieren**.

## <a name="opt-out-of-getting-your-microsoft-customer-agreement-invoices-in-email"></a>Kündigen des Erhalts von Rechnungen für die Microsoft-Kundenvereinbarung per E-Mail

Führen Sie zum Deaktivieren des Erhalts der Rechnung per E-Mail die obigen Schritte aus, und klicken Sie auf **Deaktivieren**. Alle Benutzer mit den Rollen „Besitzer des Abrechnungsprofils“, „Mitwirkender am Abrechnungsprofil“, „Benutzer mit Leseberechtigung für das Abrechnungsprofil“ und „Rechnungs-Manager“ erhalten dann ebenfalls keine Rechnungen mehr per E-Mail. Als Leseberechtigter können Sie die Einstellungen für den Empfang der Rechnungen per E-Mail nicht bearbeiten.

## <a name="check-your-billing-account-type"></a>Überprüfen des Abrechnungskontotyps
[!INCLUDE [billing-check-account-type](../../includes/billing-check-account-type.md)]

## <a name="need-help-contact-us"></a>Sie brauchen Hilfe? Wenden Sie sich an uns.

Wenn Sie weitere Fragen haben oder Hilfe benötigen, [erstellen Sie eine Supportanfrage](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie die folgenden Artikel, um mehr über Ihre Rechnung und Gebühren zu erfahren:

- [Anzeigen und Herunterladen der Microsoft Azure-Nutzung und -Gebühren](billing-download-azure-daily-usage.md)
- [Grundlegendes zu Ihrer Rechnung für Microsoft Azure](billing-understand-your-bill.md)
- [Grundlegendes zu den Benennungen in Ihrer Azure-Rechnung](billing-understand-your-invoice.md)
- [Grundlegendes über Benennungen zu den Gebühren in der Datei mit ausführlichen Nutzungsdaten zu Microsoft Azure](billing-understand-your-usage.md)
- [Anzeigen der Azure Enterprise Agreement-Preise für Ihre Organisation](billing-ea-pricing.md)

Wenn Sie eine Microsoft-Kundenvereinbarung abgeschlossen haben, lesen Sie die folgenden Artikel:

- [Grundlegendes zu den Gebühren auf der Rechnung für Ihr Abrechnungsprofil](billing-mca-understand-your-bill.md)
- [Grundlegendes zu den Benennungen auf der Rechnung für Ihr Abrechnungsprofil](billing-mca-understand-your-invoice.md)
- [Grundlegendes zur CSV-Datei für die Azure-Nutzung und -Gebühren für Ihr Abrechnungsprofil](billing-mca-understand-your-usage.md)
- [Anzeigen und Herunterladen von Steuerdokumenten für Ihr Abrechnungsprofil](billing-mca-download-tax-document.md)
- [Anzeigen der Azure Enterprise Agreement-Preise für Ihre Organisation](billing-ea-pricing.md)
