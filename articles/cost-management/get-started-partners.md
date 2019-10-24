---
title: Erste Schritte mit Azure Cost Management für Partner
description: In diesem Artikel wird erläutert, wie Partner Features von Azure Cost Management verwenden und den Zugriff auf Cost Management für ihre Kunden aktivieren.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 10/14/2019
ms.topic: conceptual
ms.service: cost-management
manager: aparnag
ms.custom: secdec18
ms.openlocfilehash: 9d95e23cf92c7ee98291831d60088d610c3e5c52
ms.sourcegitcommit: 0576bcb894031eb9e7ddb919e241e2e3c42f291d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "72377431"
---
# <a name="get-started-with-azure-cost-management-for-partners"></a>Erste Schritte mit Azure Cost Management für Partner

Azure Cost Management ist für Partner, die ihre Kunden in eine Microsoft-Kundenvereinbarung aufgenommen haben, nativ verfügbar. In diesem Artikel wird erläutert, wie Partner Features von [Azure Cost Management ](https://docs.microsoft.com/azure/cost-management/) verwenden. Außerdem wird beschrieben, wie Partner ihren Kunden den Zugriff auf Cost Management ermöglichen. CSP-Kunden können Features von Cost Management verwenden, wenn sie von ihrem CSP-Partner aktiviert werden.

CSP-Partner verwenden Cost Management für folgende Zwecke:

- Erläuterung in Rechnung gestellter Kosten und Zuordnung der Kosten zu Kunden, Abonnements, Ressourcengruppen und Diensten.
- Gewinnen einer intuitiven Übersicht über die Azure-Kosten in der [Kostenanalyse ](quick-acm-cost-analysis.md) mit Funktionen zur Analyse der Kosten nach Kunde, Abonnement, Ressourcengruppe, Ressource, Verbrauchseinheit, Dienst und vielen anderen Dimensionen.
- Anzeigen von Ressourcenkosten, für die ein Partner Earned Credit (PEC) in der Kostenanalyse angewendet wird.
- Einrichten von Benachrichtigungen und Automatisierung mithilfe programmgesteuerter [Budgets](tutorial-acm-create-budgets.md) und Warnungen, wenn die Kosten Budgets überschreiten.
- Aktivieren der Azure Resource Manager-Richtlinie, die dem Kunden Zugriff auf Cost Management-Daten ermöglicht. Kunden können dann die Verbrauchskostendaten für ihre Abonnements mit [nutzungsbasierten Tarifen](https://azure.microsoft.com/pricing/calculator/) anzeigen.

Alle Funktionen, die in Azure Cost Management verfügbar sind, sind auch mit Rest-APIs verfügbar. Verwenden Sie die APIs, um Kostenverwaltungsaufgaben zu automatisieren.

## <a name="prerequisites"></a>Voraussetzungen

Azure Cost Management erfordert Lesezugriff auf Ihr Abrechnungskonto oder Ihr Abonnement. Der Zugriff kann vom Abrechnungskonto oder von einer Verwaltungsgruppe bis hin zu einzelnen Ressourcengruppen, in denen Sie Ihre Apps verwalten, auf jeder Ebene oberhalb Ihrer Ressourcen gewährt werden. Damit Abonnementbenutzer Preise und Kosten anzeigen können, muss der Zugriff auf die Anzeige von Gebühren für Ihr Abrechnungskonto aktiviert werden. Weitere Informationen zum Aktivieren und Zuweisen des Zugriffs auf Azure Cost Management finden Sie unter [Zuweisen des Zugriffs auf Cost Management-Daten](assign-access-acm-data.md). Eine vollständige Liste der unterstützten Kontotypen finden Sie unter [Grundlegendes zu Cost Management-Daten](understand-cost-mgt-data.md).


## <a name="how-cost-management-uses-scopes"></a>Zweck von Bereichen in Cost Management

In Bereichen können Sie Abrechnungsdaten verwalten, mit für Zahlungen spezifischen Rollen arbeiten, Rechnungen einsehen und die allgemeine Kontenverwaltung durchführen. Rechnungs- und Kontorollen werden getrennt von Bereichen für die Ressourcenverwaltung verwaltet, die Azure RBAC (rollenbasierte Zugriffssteuerung) verwenden. Um den Zweck der einzelnen Bereiche, einschließlich der Unterschiede bei der Zugriffssteuerung, klar zu unterscheiden, werden diese als Abrechnungsbereiche und RBAC-Bereiche bezeichnet.

Informationen zu Abrechnungsbereichen und RBAC-Bereichen sowie zur Funktionsweise der Kostenverwaltung mit Bereichen finden Sie unter [Verstehen von und Arbeiten mit Bereichen](understand-work-scopes.md).

## <a name="manage-costs-with-partner-tenant-billing-scopes"></a>Verwalten von Kosten mit Abrechnungsbereichen von Partnermandanten

Nachdem Sie Ihre Kunden in eine Microsoft-Kundenvereinbarung aufgenommen haben, sind die folgenden _Abrechnungsbereiche_  in Ihrem Mandanten verfügbar. Verwenden Sie die Bereiche, um Kosten in Cost Management zu verwalten.

### <a name="billing-account-scope"></a>Abrechnungskontobereich

Verwenden Sie den Abrechnungskontobereich, um die Kosten vor Steuern für alle Ihre Kunden und Abrechnungsprofile anzuzeigen. Sie können auch die Rechnungskosten für verbrauchsbasierte Produkte für Kunden anzeigen, die in die Microsoft-Kundenvereinbarung eingebunden sind. Rechnungskosten werden auch für erworbene Produkte für Kunden im Rahmen der Microsoft-Kundenvereinbarung und des CSP-Angebots angezeigt. Derzeit werden die Kosten im Bereich standardmäßig in US-Dollar angezeigt. Die Budgets werden für den Bereich ebenfalls in US-Dollar festgelegt.

Unabhängig von der Währung, in der mit dem Kunden abgerechnet wird, nutzen Partner den Bereich, um in US-Dollar Budgets festzulegen und Kosten für ihre Kunden, Abonnements, Ressourcen und Ressourcengruppen zu verwalten.

Partner filtern auch Kosten in einer bestimmten Abrechnungswährung kundenübergreifend in der Kostenanalyseansicht. Wählen Sie die Liste **Tatsächliche Kosten** aus, um die Kosten in den für die Kundenabrechnung unterstützten Währungen anzuzeigen.

![Beispiel mit der Auswahl „Tatsächliche Kosten“ für Währungen](./media/get-started-partners/actual-cost-selector.png)

Verwenden Sie die [Ansicht der amortisierten Kosten](quick-acm-cost-analysis.md#customize-cost-views) in Abrechnungsbereichen, um die amortisierten Kosten einer reservierten Instanz in einem Reservierungszeitraum anzuzeigen.

### <a name="billing-profile-scope"></a>Abrechnungsprofilbereich

Verwenden Sie den Abrechnungsprofilbereich, um für alle Ihre Kunden für alle in einer Rechnung enthaltenen Produkte und Abonnements die Kosten vor Steuern in der Abrechnungswährung anzuzeigen. Mithilfe des Filters **InvoiceID** können Sie die Kosten in einem Abrechnungsprofil für eine bestimmte Rechnung filtern. Der Filter zeigt die Verbrauchs- und Produktkaufkosten für eine bestimmte Rechnung an. Sie können auch die Kosten für einen bestimmten Kunden in der Rechnung filtern, um die Kosten vor Steuern anzuzeigen.

Nachdem Sie Kunden in eine Microsoft-Kundenvereinbarung aufgenommen haben, erhalten Sie eine Kundenrechnung, in der Gebühren für Berechtigungen und erworbene Produkte wie Saas, Azure Marketplace und Reservierungen angezeigt werden. Bei der Abrechnung in derselben Abrechnungswährung zeigt die Rechnung auch Kundengebühren an, die nicht in der neuen Microsoft-Kundenvereinbarung enthalten sind.

Um Gebühren mit der Kundenrechnung abzustimmen, können Sie im Abrechnungsprofilbereich alle Kosten anzeigen, die im Rahmen einer Rechnung für Ihre Kunden anfallen. Wie die Rechnung zeigt der Bereich Kosten für jeden Kunden an, der in die neue Microsoft-Kundenvereinbarung eingebunden ist. Der Bereich zeigt auch sämtliche Gebühren für Kundenberechtigungsprodukte an, die noch zum aktuellen CSP-Angebot gehören.

Abrechnungsprofil- und Abrechnungskontobereich sind die einzigen Bereiche, die Gebühren für Berechtigungs- und kaufbasierte Produkte anzeigen.

Abrechnungsprofile definieren die Abonnements, die in einer Rechnung berücksichtigt werden. Abrechnungsprofile sind das funktionale Äquivalent zu einer Enterprise Agreement-Registrierung. Bei einer Registrierung handelt es sich um den Bereich, in dem Rechnungen generiert werden. Nicht nutzungsabhängige Käufe wie Azure Marketplace und Reservierungen sind auch nur im Abrechnungsprofilbereich möglich.

Derzeit ist die Abrechnungswährung des Kunden die Standardwährung beim Anzeigen der Kosten im Abrechnungsprofilbereich. Im Abrechnungsprofilbereich erfolgt die Festlegung von Budgets in der Abrechnungswährung.

Partner können den Bereich verwenden, um Rechnungen abzustimmen. Außerdem verwenden sie den Bereich, um Budgets in der Abrechnungswährung festzulegen für:

- Spezifisch gefilterte Rechnungen
- Kunde
- Subscription
- Resource group
- Resource
- Azure-Dienst
- Zähler
- ResellerMPNID

### <a name="customer-scope"></a>Kundenbereich

Partner verwenden den Bereich, um Kosten zu verwalten, die in die Microsoft-Kundenvereinbarung eingebundenen Kunden zugeordnet sind. Mit dem Bereich können Partner die Kosten vor Steuern eines bestimmten Kunden anzeigen. Sie können auch die Kosten vor Steuern für ein bestimmtes Abonnement, eine Ressourcengruppe oder Ressource filtern.

Der Kundenbereich umfasst keine Kunden, die dem aktuellen CSP-Angebot unterliegen. Berechtigungskosten, nicht die Azure-Nutzung, für Kunden im Rahmen des aktuellen CSP-Angebots sind im Abrechnungskonto- und Abrechnungsprofilbereich verfügbar, wenn Sie den Kundenfilter anwenden.

## <a name="partner-access-to-billing-scopes-in-cost-management"></a>Partnerzugriff auf Abrechnungsbereiche in Cost Management

Nur die Benutzer mit den Rollen **Globaler Administrator** und **Administrator-Agent** können Kosten für Abrechnungskonten, Abrechnungsprofile und Kunden direkt im Azure-Mandanten des Partners verwalten und anzeigen. Weitere Informationen zu Partner Center-Rollen finden Sie unter [Zuweisen von Rollen und Berechtigungen zu Benutzern](/partner-center/permissions-overview).

### <a name="enable-cost-management-in-the-customer-tenant"></a>Aktivieren der Kostenverwaltung im Kundenmandanten

Partner können den Zugriff auf Cost Management aktivieren, nachdem Kunden in eine Microsoft-Kundenvereinbarung aufgenommen wurden. Partner können dann eine Richtlinie aktivieren, mit der Kunden ihre Kosten anzeigen können, die nach nutzungsbasierten Preisen berechnet werden. Die Kosten werden in der Abrechnungswährung der Kunden für ihren Verbrauch im RBAC-Abonnement- und Ressourcengruppenbereich angezeigt.

Wenn der Partner die Richtlinie für Kostensichtbarkeit aktiviert hat, kann jeder Benutzer mit Azure Resource Manager-Zugriff auf das Abonnement die Kosten, die sich aus nutzungsbasierten Preisen zusammensetzen, verwalten und analysieren. Reseller und Kunden, die über den entsprechenden RBAC-Zugriff auf die Azure-Abonnements verfügen, können Kosten anzeigen.

Unabhängig von der Richtlinie können Partner die Kosten auch anzeigen, wenn sie Zugriff auf das Abonnement und die Ressourcengruppe haben.

### <a name="enable-the-policy-to-view-azure-usage-charges"></a>Aktivieren der Richtlinie zum Anzeigen von Azure-Nutzungsgebühren

Partner verwenden die folgenden Informationen, um die Richtlinie zum Anzeigen von Azure-Nutzungsgebühren für ihre Kunden zu aktivieren.

Melden Sie sich im Azure-Portal beim Partnermandanten an, und klicken Sie auf **Kostenverwaltung + Abrechnung**. Wählen Sie ein Abrechnungskonto aus, und klicken Sie dann auf **Kunden**. Die Liste der Kunden wird mit dem Abrechnungskonto verknüpft.

Wählen Sie in der Liste der Kunden den Kunden aus, dem Sie die Anzeige von Kosten erlauben möchten.

![Auswählen von Kunden in Cost Management](./media/get-started-partners/customer-list.png)

Klicken Sie unter **Einstellungen** auf **Richtlinien**.

Die aktuelle Kostensichtbarkeits-Richtlinie wird für die Gebühren für die **Azure-Nutzung** angezeigt, die den Abonnements für den ausgewählten Kunden zugeordnet ist.
![Richtlinie, die Kunden ermöglicht, nutzungsbasierte Gebühren anzuzeigen](./media/get-started-partners/cost-management-billing-policies.png)

Wenn die Richtlinie auf **Nein** festgelegt ist, ist Azure Cost Management für Abonnementbenutzer, die dem Kunden zugeordnet sind, nicht verfügbar. Sofern die Kostensichtbarkeits-Richtlinie nicht von einem Partner aktiviert wird, ist sie standardmäßig für alle Abonnementbenutzer deaktiviert.

Wenn die Kostenrichtlinie auf **Ja** festgelegt ist, können Abonnementbenutzer, die dem Kundenmandanten zugeordnet sind, nutzungsbasierte Nutzungsgebühren anzeigen.

Wenn die Kostensichtbarkeits-Richtlinie aktiviert ist, werden für alle Dienste im Rahmen der Abonnementnutzung die durch nutzungsbasierte Gebühren anfallenden Kosten angezeigt. Die Reservierungsnutzung wird ohne Gebühren für tatsächliche und amortisierte Kosten angezeigt. Käufe und Berechtigungen werden keinem bestimmten Abonnement zugeordnet. Käufe werden also nicht im Abonnementbereich angezeigt.

Um die Kosten für den Kundenmandanten anzuzeigen, öffnen Sie „Kostenverwaltung + Abrechnung“, und klicken Sie dann auf „Abrechnungskonten“. Klicken Sie in der Liste der Abrechnungskonten auf ein Abrechnungskonto.

![Auswählen eines Abrechnungskontos](./media/get-started-partners/select-billing-account.png)

Klicken Sie unter **Abrechnung** auf **Azure-Abonnements**, und klicken Sie dann auf einen Kunden.

![Auswählen eines Azure-Abonnementkunden](./media/get-started-partners/subscriptions-select-customer.png)

Klicken Sie auf **Kostenanalyse**, und überprüfen Sie die Kosten.
Kostenanalyse, Budgets und Warnungen sind jetzt für die Abonnement- und Ressourcengruppen-RBAC-Bereiche für nutzungsbasierte Kosten verfügbar.

![Anzeigen der Kostenanalyse als Kunde ](./media/get-started-partners/customer-tenant-view-cost-analysis.png)

In amortisierten Ansichten und für tatsächlichen Koste für reservierte Instanzen in den RBAC-Bereichen werden keine Gebühren angezeigt. Kosten für reservierte Instanzen werden nur in Abrechnungsbereichen angezeigt, in denen die Einkäufe getätigt wurden.

## <a name="analyze-costs-in-cost-analysis"></a>Analysieren von Kosten mithilfe der Kostenanalyse

Partner können Kosten für einen bestimmten Kunden oder eine Rechnung in der Kostenanalyse für Kunden untersuchen und analysieren. Mit den Funktionen zum Filtern und Gruppieren können Sie die Kosten nach mehreren Feldern analysieren, einschließlich:

| **Feld** | **Beschreibung** | **Äquivalente Spalte im Partner Center** |
| --- | --- | --- |
| PartnerTenantID | Bezeichner für den Azure Active Directory-Mandanten des Partners | Die Azure Active Directory-TenantID des Partners wird als Partner-ID aufgerufen. Im GUID-Format. |
| PartnerName | Bezeichner für den Azure Active Directory-Mandanten des Partners | Name des Partners |
| CustomerTenantID | Bezeichner des Azure Active Directory-Mandanten des Kundenabonnements | Organisations-ID des Kunden. Beispiel: Azure Active Directory-TenantID des Kunden. |
| CustomerName | Name des Azure Active Directory-Mandanten, der das Kundenabonnement enthält | Der in Partner Center angegebene Organisationsname des Kunden. Wichtig für die Abstimmung der Rechnung mit Ihren Systeminformationen. |
| ResellerMPNID | MPNID für den Reseller, der dem Abonnement zugeordnet ist | MPN-ID des Resellers für das Abonnement. Nicht verfügbar für die aktuelle Aktivität. |
| Abonnement-ID | Der eindeutige von Microsoft generierte Bezeichner für das Azure-Abonnement | – |
| subscriptionName | Der Name des Azure-Abonnements | – |
| billingProfileID | Bezeichner für das Abrechnungsprofil. Die Kosten werden Rechnungen übergreifend in einer einzelnen Abrechnungswährung Kunden übergreifend gruppiert. | MCAPI-Partnerabrechnungsgruppen-ID. Wird in API-Anforderungen verwendet, aber nicht in Antworten eingeschlossen. |
| invoiceID | Rechnungs-ID in der Rechnung, in der die jeweilige Transaktion angezeigt wird | Rechnungsnummer, in der die angegebene Transaktion angezeigt wird. |
| resourceGroup | Der Name der Azure-Ressourcengruppe. Wird für die Verwaltung des Ressourcenlebenszyklus verwendet. | Der Name der Ressourcengruppe. |
| partnerEarnedCreditRate | Rabatt, der angewendet wird, wenn basierend auf dem Partneradministratorlink-Zugriff ein Partner Earned Credit (PEC) vorhanden ist. | Die PEC-Rate (Partner Earned Credit). Beispiel: 0 % oder 15 %. |
| partnerEarnedCreditApplied | Gibt an, ob ein Partner Earned Credit angewendet wurde. | – |

In der Ansicht [Kostenanalyse](quick-acm-cost-analysis.md) können Sie auch [Ansichten speichern](quick-acm-cost-analysis.md#saving-and-sharing-customized-views) und Daten in [CSV- und PNG-Dateien](quick-acm-cost-analysis.md#automation-and-offline-analysis) exportieren.

## <a name="view-partner-earned-credit-pec-resource-costs"></a>Anzeigen der Ressourcenkosten für Partner Earned Credit (PEC)

In Azure Cost Management können Partner die Kostenanalyse verwenden, um die Kosten anzuzeigen, auf die sich die PEC-Vorteile ausgewirkt haben.

Melden Sie sich im Azure-Portal beim Partnermandanten an, und wählen Sie **Kostenverwaltung + Abrechnung** aus. Klicken Sie unter **Kostenverwaltung** auf **Kostenanalyse**.

In der Kostenanalyseansicht werden die Kosten des Abrechnungskontos für den Partner angezeigt. Wählen Sie nach Bedarf den **Bereich** für den Partner, einen bestimmten Kunden oder ein Abrechnungsprofil aus, um Rechnungen abzustimmen.

Klicken Sie in einem Ringdiagramm auf die Dropdownliste, und wählen Sie **PartnerEarnedCreditApplied** aus, um PEC-Kosten im Detail anzuzeigen.

![Beispiel für das Anzeigen von Partner Earned Credit](./media/get-started-partners/cost-analysis-pec1.png)

Wenn die **PartnerEarnedCreditApplied**-Eigenschaft _True_ ist, wirkt sich der Vorteil des Partner Earned-Administratorzugriffs auf die zugeordneten Kosten aus.

Wenn die **PartnerEarnedCreditApplied**-Eigenschaft _False_  ist, haben die zugeordneten Kosten die erforderliche Berechtigung für die Gutschrift nicht erfüllt. Es kann auch sein, dass der erworbene Dienst nicht für Partner Earned Credit berechtigt ist.

In der Regel dauert es 8-24 Stunden, bis Dienstnutzungsdaten in Cost Management angezeigt werden. Weitere Informationen finden Sie unter [Die Häufigkeit der Aktualisierung der Nutzungsdaten variiert](understand-cost-mgt-data.md#usage-data-update-frequency-varies). PEC-Gutschriften werden innerhalb von 48 Stunden ab dem Zeitpunkt des Zugriffs in Azure Cost Management angezeigt.


Sie können auch nach der **PartnerEarnedCreditApplied**-Eigenschaft gruppieren und filtern, indem Sie die **Gruppieren nach**-Optionen verwenden. Verwenden Sie die Optionen, um zu überprüfen, auf welche Kosten PEC angewendet wird und auf welche nicht.

![Gruppieren oder Filtern nach Partner Earned Credit](./media/get-started-partners/cost-analysis-pec2.png)

## <a name="cost-management-rest-apis"></a>Cost Management-REST-APIs

Partner, indirekte Anbieter und Kunden können die in den folgenden Abschnitten für häufige Aufgaben beschriebenen Cost Management-APIs verwenden.

### <a name="azure-cost-management-apis-for-partners"></a>Azure Cost Management-APIs für Partner

Partner und Benutzer mit Zugriff auf Abrechnungsbereiche in einem Partnermandanten können die folgenden APIs verwenden.

#### <a name="to-get-a-list-of-billing-accounts"></a>So rufen Sie eine Liste von Abrechnungskonten ab

```
armclient get "providers/Microsoft.billing/billingAccounts?api-version=2019-10-01-preview"
```

#### <a name="to-get-a-list-of-customers"></a>So rufen Sie eine Liste von Kunden ab

```
armclient get "providers/Microsoft.billing/billingAccounts/99a13315-2f87-5b46-9dbd-606071106352:1d100e69-2833-4677-a5d4-8ad35035d9a3_2019-05-31/customers?api-version=2019-10-01-preview"
```
#### <a name="to-get-a-list-of-subscriptions"></a>So rufen Sie eine Liste von Abonnements ab

```
armclient get "/providers/Microsoft.Billing/billingAccounts/99a13315-2f87-5b46-9dbd-606071106352:1d100e69-2833-4677-a5d4-8ad35035d9a3_2019-05-31/customers/9553eda2-2bd7-4ae6-a1f8-6a19eb40be22/billingSubscriptions?api-version=2019-10-01-preview"
```

#### <a name="to-create-new-subscription"></a>So erstellen Sie ein neues Abonnement

```
armclient post "/providers/Microsoft.Billing/billingAccounts/99a13315-2f87-5b46-9dbd-606071106352:1d100e69-2833-4677-a5d4-8ad35035d9a3_2019-05-31/customers/9553eda2-2bd7-4ae6-a1f8-6a19eb40be22/providers/Microsoft.Subscription/createSubscription?api-version=2018-11-01-preview" @createsub.json -verbose
```

#### <a name="to-get-or-download-usage-for-azure-services"></a>So rufen Sie die Nutzung für Azure-Dienste ab oder laden sie herunter

```
armclient GET /providers/Microsoft.Billing/BillingAccounts/99a13315-2f87-5b46-9dbd-606071106352:1d100e69-2833-4677-a5d4-8ad35035d9a3_2019-05-31/providers/Microsoft.Consumption/usageDetails?api-version=2019-10-01
```

#### <a name="to-get-a-list-of-billing-profiles"></a>So rufen Sie eine Liste von Abrechnungsprofilen ab

```
armclient get "providers/Microsoft.Billing/billingAccounts/99a13315-2f87-5b46-9dbd-606071106352:1d100e69-2833-4677-a5d4-8ad35035d9a3_2019-05-31/billingProfiles?api-version=2019-10-01-preview
```

#### <a name="to-get-or-download-the-price-sheet-for-consumed-azure-services"></a>So rufen Sie das Preisblatt für genutzte Azure-Dienste ab oder laden es herunter

```
armclient post "/providers/Microsoft.Billing/BillingAccounts/99a13315-2f87-5b46-9dbd-606071106352:1d100e69-2833-4677-a5d4-8ad35035d9a3_2019-05-31/BillingProfiles/JUT6-EU3Q-BG7-TGB/pricesheet/default/download?api-version=2019-10-01-preview&format=csv" -verbose
```

#### <a name="to-get-customer-costs-for-the-last-two-months-sorted-by-month"></a>So rufen Sie Kundenkosten für die letzten zwei Monate nach Monaten sortiert ab

```
armclient post providers/microsoft.billing/billingAccounts/99a13315-2f87-5b46-9dbd-606071106352:1d100e69-2833-4677-a5d4-8ad35035d9a3_2019-05-31//providers/microsoft.costmanagement/query?api-version=2019-10-01 @CCMQueryCustomer.json
```

#### <a name="to-get-azure-subscription-costs-for-the-last-two-months-sorted-by-month"></a>So rufen Sie Azure-Abonnementkosten für die letzten zwei Monate nach Monaten sortiert ab

```
armclient post providers/microsoft.billing/billingAccounts/99a13315-2f87-5b46-9dbd-606071106352:1d100e69-2833-4677-a5d4-8ad35035d9a3_2019-05-31//providers/microsoft.costmanagement/query?api-version=2019-10-01 @CCMQuerySubscription.json
```

#### <a name="to-get-daily-costs-for-the-current-month"></a>So rufen Sie tägliche Kosten für den aktuellen Monat ab

```
armclient post providers/microsoft.billing/billingAccounts/99a13315-2f87-5b46-9dbd-606071106352:1d100e69-2833-4677-a5d4-8ad35035d9a3_2019-05-31//providers/microsoft.costmanagement/query?api-version=2019-10-01 @CCMQueryDaily.json
```

#### <a name="to-get-the-policy-for-customers-to-view-costs"></a>So rufen Sie die Richtlinie für Kunden zum Anzeigen von Kosten ab

```
armclient get "providers/Microsoft.Billing/billingAccounts/99a13315-2f87-5b46-9dbd-606071106352:1d100e69-2833-4677-a5d4-8ad35035d9a3_2019-05-31/customers/9553eda2-2bd7-4ae6-a1f8-6a19eb40be22/policies/default?api-version=2019-10-01-preview"
```

#### <a name="to-set-the-policy-for-customers-to-view-costs"></a>So legen Sie die Richtlinie für Kunden zum Anzeigen von Kosten fest

```
armclient put "providers/Microsoft.Billing/billingAccounts/99a13315-2f87-5b46-9dbd-606071106352:1d100e69-2833-4677-a5d4-8ad35035d9a3_2019-05-31/customers/9553eda2-2bd7-4ae6-a1f8-6a19eb40be22/policies/default?api-version=2019-10-01-preview" @policy.json
```

### <a name="azure-cost-management-apis-for-indirect-providers"></a>Azure Cost Management-APIs für indirekte Anbieter

Indirekte Anbieter mit Zugriff auf RBAC-Bereiche in einem Kundenmandanten können die folgenden APIs verwenden. Melden Sie sich zunächst als Benutzer oder mit einem Dienstprinzipal an.

#### <a name="to-get-the-billing-account-information"></a>So rufen Sie Informationen zum Abrechnungskonto ab

```
armclient get "providers/Microsoft.billing/billingAccounts?api-version=2019-10-01-preview"
```

#### <a name="to-get-a-list-of-customers"></a>So rufen Sie eine Liste von Kunden ab

```
armclient get "providers/Microsoft.billing/billingAccounts/ec1b88ba-5681-517e-f657-4cc6a4a407cb:52f143a9-6524-4e5e-9d4a-120c7a79ca65_2019-05-31/customers?api-version=2019-10-01-preview"
```

#### <a name="to-get-a-list-of-resellers-associated-with-the-customer"></a>So rufen Sie eine Liste der Reseller ab, die dem Kunden zugeordnet sind

```
armclient get "/providers/Microsoft.Billing/billingAccounts/ec1b88ba-5681-517e-f657-4cc6a4a407cb:52f143a9-6524-4e5e-9d4a-120c7a79ca65_2019-05-31/customers/b51df1fa-62fa-4c92-9a74-fe860016d4db?api-version=2019-10-01-preview&$expand=resellers
```

#### <a name="to-get-a-list-of-subscriptions-with-reseller-information"></a>So rufen Sie eine Liste von Abonnements mit Resellerinformationen ab

```
armclient get "/providers/Microsoft.Billing/billingAccounts/ec1b88ba-5681-517e-f657-4cc6a4a407cb:52f143a9-6524-4e5e-9d4a-120c7a79ca65_2019-05-31/customers/b51df1fa-62fa-4c92-9a74-fe860016d4db/billingSubscriptions?api-version=2019-10-01-preview
```

#### <a name="to-create-a-subscription"></a>So erstellen Sie ein Abonnement

```
armclient post "/providers/Microsoft.Billing/billingAccounts/ec1b88ba-5681-517e-f657-4cc6a4a407cb:52f143a9-6524-4e5e-9d4a-120c7a79ca65_2019-05-31/customers/b51df1fa-62fa-4c92-9a74-fe860016d4db/providers/Microsoft.Subscription/createSubscription?api-version=2018-11-01-preview" @createsub_reseller.json
```

### <a name="azure-cost-management-apis-for-customers"></a>Azure Cost Management-APIs für Kunden

Kunden verwenden die folgenden Informationen, um auf die APIs zuzugreifen. Melden Sie sich zunächst als Benutzer an.

#### <a name="to-get-or-download-azure-consumption-usage-information-with-retail-rates"></a>So rufen Sie Azure-Verbrauchsinformationen mit Verkaufspreisen ab oder laden sie herunter

```
armclient post /subscriptions/66bada28-271e-4b7a-aaf5-c0ead63923d7/providers/microsoft.costmanagement/query?api-version=2019-10-01 @CCMQueryDaily.json
```

## <a name="next-steps"></a>Nächste Schritte
- [Beginnen mit der Kostenanalyse](quick-acm-cost-analysis.md) in Cost Management
- [Erstellen und Verwalten von Budgets](tutorial-acm-create-budgets.md) in Cost Management
