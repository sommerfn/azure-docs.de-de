---
title: Erste Schritte mit Azure Cost Management für Partner
description: In diesem Artikel wird erläutert, wie Partner Features von Azure Cost Management verwenden und den Zugriff auf Cost Management für ihre Kunden aktivieren.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 11/04/2019
ms.topic: conceptual
ms.service: cost-management
manager: aparnag
ms.custom: secdec18
ms.openlocfilehash: 611b3e608d9b0de9423c861ec70e9fc2e7ad67d5
ms.sourcegitcommit: bc7725874a1502aa4c069fc1804f1f249f4fa5f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73720752"
---
# <a name="get-started-with-azure-cost-management-for-partners"></a>Erste Schritte mit Azure Cost Management für Partner

Azure Cost Management ist für Partner, die ihre Kunden in eine Microsoft-Kundenvereinbarung aufgenommen haben, nativ verfügbar. In diesem Artikel wird erläutert, wie Partner Features von [Azure Cost Management ](https://docs.microsoft.com/azure/cost-management/) verwenden. Außerdem wird beschrieben, wie Partner ihren Kunden den Zugriff auf Cost Management ermöglichen. Kunden können Cost Management-Funktionen verwenden, wenn sie von ihrem CSP-Partner aktiviert werden.

CSP-Partner verwenden Cost Management für folgende Zwecke:

- Erläuterung in Rechnung gestellter Kosten und Zuordnung der Kosten zu Kunden, Abonnements, Ressourcengruppen und Diensten.
- Gewinnen einer intuitiven Übersicht über die Azure-Kosten in der [Kostenanalyse ](quick-acm-cost-analysis.md) mit Funktionen zur Analyse der Kosten nach Kunde, Abonnement, Ressourcengruppe, Ressource, Verbrauchseinheit, Dienst und vielen anderen Dimensionen.
- Anzeigen von Ressourcenkosten, für die ein Partner Earned Credit (PEC) in der Kostenanalyse angewendet wird.
- Einrichten von Benachrichtigungen und Automatisierung mithilfe programmgesteuerter [Budgets](tutorial-acm-create-budgets.md) und Warnungen, wenn die Kosten Budgets überschreiten.
- Aktivieren der Azure Resource Manager-Richtlinie, die dem Kunden Zugriff auf Cost Management-Daten ermöglicht. Kunden können dann die Verbrauchskostendaten für ihre Abonnements mit [nutzungsbasierten Tarifen](https://azure.microsoft.com/pricing/calculator/) anzeigen.

Nachstehend finden Sie ein Beispiel, das die Kosten für alle Kunden anzeigt.
![Beispiel, das die Kosten für alle Kunden anzeigt](./media/get-started-partners/customer-costs1.png)

Das folgende Beispiel zeigt die Kosten für einen einzelnen Kunden an.
![Beispiel, das die Kosten für einen einzelnen Kunden anzeigt](./media/get-started-partners/customer-costs2.png)

Alle Funktionen, die in Azure Cost Management verfügbar sind, sind auch mit Rest-APIs verfügbar. Verwenden Sie die APIs, um Kostenverwaltungsaufgaben zu automatisieren.

## <a name="prerequisites"></a>Voraussetzungen

Azure Cost Management erfordert Lesezugriff auf Ihr Abrechnungskonto oder Ihr Abonnement. Der Zugriff kann vom Abrechnungskonto oder von einer Verwaltungsgruppe bis hin zu einzelnen Ressourcengruppen, in denen Sie Ihre Apps verwalten, auf jeder Ebene oberhalb Ihrer Ressourcen gewährt werden. Weitere Informationen zum Aktivieren und Zuweisen der Zugriffsrechte auf Azure Cost Management für ein Abrechnungskonto finden Sie unter [Zuweisen von Benutzerrollen und Berechtigungen](/partner-center/permissions-overview). Die Rollen **Globaler Administrator** und **Administrator-Agent** können die Kosten für ein Abrechnungskonto verwalten.

Eine vollständige Liste der unterstützten Kontotypen finden Sie unter [Grundlegendes zu Cost Management-Daten](understand-cost-mgt-data.md).


## <a name="how-cost-management-uses-scopes"></a>Zweck von Bereichen in Cost Management

In Bereichen können Sie Abrechnungsdaten verwalten, mit für Zahlungen spezifischen Rollen arbeiten, Rechnungen einsehen und die allgemeine Kontenverwaltung durchführen. Rechnungs- und Kontorollen werden getrennt von Bereichen für die Ressourcenverwaltung verwaltet, die Azure RBAC (rollenbasierte Zugriffssteuerung) verwenden. Um den Zweck der einzelnen Bereiche, einschließlich der Unterschiede bei der Zugriffssteuerung, klar zu unterscheiden, werden diese als Abrechnungsbereiche und RBAC-Bereiche bezeichnet.

Informationen zu Abrechnungsbereichen und RBAC-Bereichen sowie zur Funktionsweise der Kostenverwaltung mit Bereichen finden Sie unter [Verstehen von und Arbeiten mit Bereichen](understand-work-scopes.md).

## <a name="manage-costs-with-partner-tenant-billing-scopes"></a>Verwalten von Kosten mit Abrechnungsbereichen von Partnermandanten

Nachdem Sie Ihre Kunden in eine Microsoft-Kundenvereinbarung aufgenommen haben, sind die folgenden _Abrechnungsbereiche_  in Ihrem Mandanten verfügbar. Verwenden Sie die Bereiche, um Kosten in Cost Management zu verwalten.

### <a name="billing-account-scope"></a>Abrechnungskontobereich

Verwenden Sie den Abrechnungskontobereich, um die Kosten vor Steuern für alle Ihre Kunden und Abrechnungsprofile anzuzeigen. Rechnungskosten werden nur für verbrauchsbasierte Produkte in der Microsoft-Kundenvereinbarung von Kunden angezeigt. Rechnungskosten werden jedoch für erworbene Produkte für Kunden im Rahmen der Microsoft-Kundenvereinbarung und des CSP-Angebots angezeigt. Derzeit werden die Kosten im Bereich standardmäßig in US-Dollar angezeigt. Die Budgets werden für den Bereich ebenfalls in US-Dollar festgelegt.

Unabhängig von den verschiedenen Währungen, in denen mit dem Kunden abgerechnet wird, nutzen Partner den Abrechnungskontobereich, um in US-Dollar Budgets festzulegen und Kosten für ihre Kunden, Abonnements, Ressourcen und Ressourcengruppen zu verwalten.

Partner filtern auch Kosten in einer bestimmten Abrechnungswährung kundenübergreifend in der Kostenanalyseansicht. Wählen Sie die Liste **Tatsächliche Kosten** aus, um die Kosten in den für die Kundenabrechnung unterstützten Währungen anzuzeigen.

![Beispiel mit der Auswahl „Tatsächliche Kosten“ für Währungen](./media/get-started-partners/actual-cost-selector.png)

Verwenden Sie die [Ansicht der amortisierten Kosten](quick-acm-cost-analysis.md#customize-cost-views) in Abrechnungsbereichen, um die amortisierten Kosten einer reservierten Instanz in einem Reservierungszeitraum anzuzeigen.

### <a name="billing-profile-scope"></a>Abrechnungsprofilbereich

Verwenden Sie den Abrechnungsprofilbereich, um für alle Ihre Kunden für alle in einer Rechnung enthaltenen Produkte und Abonnements die Kosten vor Steuern in der Abrechnungswährung anzuzeigen. Mithilfe des Filters **InvoiceID** können Sie die Kosten in einem Abrechnungsprofil für eine bestimmte Rechnung filtern. Der Filter zeigt die Verbrauchs- und Produktkaufkosten für eine bestimmte Rechnung an. Sie können auch die Kosten für einen bestimmten Kunden in der Rechnung filtern, um die Kosten vor Steuern anzuzeigen.

Nachdem Sie Kunden in eine Microsoft-Kundenvereinbarung aufgenommen haben, erhalten Sie eine Rechnung, die alle Gebühren für alle Produkte (Nutzung, Käufe und Berechtigungen) für diese Kunden in der Microsoft-Kundenvereinbarung enthält. Wenn diese Rechnungen in derselben Währung abgerechnet werden, enthalten Sie auch die Gebühren für Berechtigungen und erworbene Produkte wie Saas, Azure Marketplace und Reservierungen für Kunden, die sich noch im CSP-Angebot befinden.

Um Gebühren mit der Kundenrechnung abzustimmen, können Sie im Abrechnungsprofilbereich alle Kosten anzeigen, die im Rahmen einer Rechnung für Ihre Kunden anfallen. Wie die Rechnung zeigt der Bereich Kosten für jeden Kunden an, der in die neue Microsoft-Kundenvereinbarung eingebunden ist. Der Bereich zeigt auch sämtliche Gebühren für Kundenberechtigungsprodukte an, die noch zum aktuellen CSP-Angebot gehören.

Abrechnungsprofil- und Abrechnungskontobereich sind die einzigen Bereiche, die Gebühren für Berechtigungen und kaufbasierte Produkte wie Azure Marketplace und erworbene Reservierungen anzeigen.

Abrechnungsprofile definieren die Abonnements, die in einer Rechnung berücksichtigt werden. Abrechnungsprofile sind das funktionale Äquivalent zu einer Enterprise Agreement-Registrierung. Ein Abrechnungsprofil ist der Bereich, in dem Rechnungen generiert werden.

Derzeit ist die Abrechnungswährung des Kunden die Standardwährung beim Anzeigen der Kosten im Abrechnungsprofilbereich. Im Abrechnungsprofilbereich erfolgt die Festlegung von Budgets in der Abrechnungswährung.

Partner können den Bereich verwenden, um Rechnungen abzustimmen. Außerdem verwenden sie den Bereich, um Budgets in der Abrechnungswährung für folgende Elemente festzulegen:

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

Der Kundenbereich umfasst keine Kunden, die dem aktuellen CSP-Angebot unterliegen. Der Bereich umfasst nur Kunden, die über eine Microsoft-Kundenvereinbarung verfügen. Berechtigungskosten, nicht die Azure-Nutzung, für Kunden im Rahmen des aktuellen CSP-Angebots sind im Abrechnungskonto- und Abrechnungsprofilbereich verfügbar, wenn Sie den Kundenfilter anwenden.

## <a name="partner-access-to-billing-scopes-in-cost-management"></a>Partnerzugriff auf Abrechnungsbereiche in Cost Management

Nur die Benutzer mit den Rollen **Globaler Administrator** und **Administrator-Agent** können Kosten für Abrechnungskonten, Abrechnungsprofile und Kunden direkt im Azure-Mandanten des Partners verwalten und anzeigen. Weitere Informationen zu Partner Center-Rollen finden Sie unter [Zuweisen von Rollen und Berechtigungen zu Benutzern](/partner-center/permissions-overview).

## <a name="enable-cost-management-in-the-customer-tenant"></a>Aktivieren der Kostenverwaltung im Kundenmandanten

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
Kostenanalyse, Budgets und Warnungen sind für die Abonnement- und Ressourcengruppen-RBAC-Bereiche für nutzungsbasierte Kosten verfügbar.

![Anzeigen der Kostenanalyse als Kunde ](./media/get-started-partners/customer-tenant-view-cost-analysis.png)

In amortisierten Ansichten und für tatsächlichen Koste für reservierte Instanzen in den RBAC-Bereichen werden keine Gebühren angezeigt. Kosten für reservierte Instanzen werden nur in Abrechnungsbereichen angezeigt, in denen die Einkäufe getätigt wurden.

## <a name="analyze-costs-in-cost-analysis"></a>Analysieren von Kosten mithilfe der Kostenanalyse

Partner können Kosten für einen bestimmten Kunden oder eine Rechnung in der Kostenanalyse für Kunden untersuchen und analysieren.

Die folgenden Felder befinden sich im Nutzungsdetaildateien und Cost Management-APIs. Sie können die Filter- und Gruppierungsfunktionen in der Kostenanalyse verwenden, um die Kosten anhand mehrerer Feldern zu analysieren. Eine komplette Liste der Felder finden Sie unter [Cost Management-Datenfelder](understand-cost-mgt-data.md#cost-management-data-fields).

| Feldname | BESCHREIBUNG |
| --- | --- |
| CustomerTenantID | Bezeichner des Azure Active Directory-Mandanten des Kundenabonnements. |
| CustomerName | Name des Azure Active Directory-Mandanten des Kundenabonnements. |
| CustomerTenantDomainName | Domänenname des Azure Active Directory-Mandanten des Kundenabonnements. |
| PartnerTenantID | Bezeichner für den Azure Active Directory-Mandanten des Partners. |
| PartnerName | Name für den Azure Active Directory-Mandanten des Partners. |
| ResellerMPNID | MPNID für den Handelspartner, der dem Abonnement zugeordnet ist. |
| costinUSD | Geschätzte erweiterte Kosten oder gemischte Kosten vor Steuern in USD. |
| paygCostInBillingCurrency | Zeigt Kosten an, wenn die Preise als Verkaufspreise angegeben sind. Zeigt die Preise für die nutzungsbasierte Bezahlung in der Abrechnungswährung an. Nur verfügbar bei RBAC-Bereichen. |
| paygCostInUSD | Zeigt Kosten an, wenn die Preise als Verkaufspreise angegeben sind. Zeigt die Preise für die nutzungsbasierte Bezahlung in USD an. Nur verfügbar bei RBAC-Bereichen. |
| partnerEarnedCreditRate | Rabatt, der angewendet wird, wenn basierend auf dem Partneradministratorlink-Zugriff ein Partner Earned Credit (PEC) vorhanden ist. |
| partnerEarnedCreditApplied | Gibt an, ob der Partner Earned Credit angewendet wurde. |

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

Partner und Kunden können die in den folgenden Abschnitten für häufige Aufgaben beschriebenen Cost Management-APIs verwenden.

### <a name="azure-cost-management-apis---direct-and-indirect-providers"></a>Azure Cost Management-APIs – Direkte und indirekte Anbieter

Partner mit Zugriff auf Abrechnungsbereiche in einem Partnermandanten können die folgenden APIs verwenden, um in Rechnung gestellte Kosten anzuzeigen.

APIs im Abonnementbereich können von einem Partner unabhängig von der Kostenrichtlinie aufgerufen werden, wenn sie auf das Abonnement zugreifen können. Andere Benutzer mit Zugriff auf das Abonnement, z. B. der Kunde oder der Handelspartner, können die APIs nur aufrufen, nachdem der Partner die Kostenrichtlinie für den Kundenmandanten aktiviert hat.


#### <a name="to-get-a-list-of-billing-accounts"></a>So rufen Sie eine Liste von Abrechnungskonten ab

```
GET https://management.azure.com/providers/Microsoft.Billing/billingAccounts?api-version=2019-10-01-preview
```

#### <a name="to-get-a-list-of-customers"></a>So rufen Sie eine Liste von Kunden ab

```
GET https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billingAccountName}/customers?api-version=2019-10-01-preview
```

#### <a name="to-get-a-list-of-subscriptions"></a>So rufen Sie eine Liste von Abonnements ab

```
GET https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billingAccountName}/billingSubscriptions?api-version=2019-10-01-preview
```

#### <a name="to-get-a-list-of-invoices-for-a-period-of-time"></a>So rufen Sie eine Liste der Rechnungen für einen Zeitraum ab

```
GET https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billingAccountName}/invoices?api-version=2019-10-01-preview&periodStartDate={periodStartDate}&periodEndDate={periodEndDate}
```

Der API-Aufruf gibt ein Array von Rechnungen zurück, das ähnliche Elemente wie der folgende JSON-Code aufweist.

```
    {
      "id": "/providers/Microsoft.Billing/billingAccounts/{billingAccountID}/billingProfiles/{BillingProfileID}/invoices/{InvoiceID}",
      "name": "{InvoiceID}",
      "properties": {
        "amountDue": {
          "currency": "USD",
          "value": x.xx
        },
        ...
    }
```

Verwenden Sie den zuvor zurückgegebenen ID-Feldwert und ersetzen Sie ihn im folgenden Beispiel als Gültigkeitsbereich für die Abfrage von Nutzungsdetails.

```
GET https://management.azure.com/{id}/providers/Microsoft.Consumption/UsageDetails?api-version=2019-10-01
```

Im Beispiel werden die Nutzungsdatensätze für die jeweilige Rechnung zurückgegeben.


#### <a name="to-get-the-policy-for-customers-to-view-costs"></a>So rufen Sie die Richtlinie für Kunden zum Anzeigen von Kosten ab

```
GET https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billingAccountName}/customers/{customerID}/policies/default?api-version=2019-10-01-preview
```

#### <a name="to-set-the-policy-for-customers-to-view-costs"></a>So legen Sie die Richtlinie für Kunden zum Anzeigen von Kosten fest

```
PUT https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billingAccountName}/customers/{customerID}/policies/default?api-version=2019-10-01-preview
```

#### <a name="to-get-azure-service-usage-for-a-billing-account"></a>So rufen Sie die Azure-Dienstnutzung für ein Abrechnungskonto ab

```
GET https://management.azure.com/providers/Microsoft.Billing/BillingAccounts/{billingAccountName}/providers/Microsoft.Consumption/usageDetails?api-version=2019-10-01
```

#### <a name="to-download-a-customers-azure-service-usage"></a>So laden Sie die Azure-Dienstnutzung eines Kunden herunter

Der folgende GET-Aufruf ist ein asynchroner Vorgang.

```
GET https://management.azure.com/Microsoft.Billing/billingAccounts/{billingAccountName}/customers/{customerID}/providers/Microsoft.Consumption/usageDetails/download?api-version=2019-10-01 -verbose
```

Ruft den in der Antwort zurückgegebenen `Location`-URI auf, um den Vorgangsstatus zu überprüfen. Wenn der Status *Completed*ist, enthält die Eigenschaft `downloadUrl` einen Link, mit dem Sie den generierten Bericht herunterladen können.


#### <a name="to-get-or-download-the-price-sheet-for-consumed-azure-services"></a>So rufen Sie das Preisblatt für genutzte Azure-Dienste ab oder laden es herunter

Verwenden Sie zunächst den folgenden POST-Aufruf.

```
POST https://management.azure.com/providers/Microsoft.Billing/BillingAccounts/{billingAccountName}/billingProfiles/{billingProfileID}/pricesheet/default/download?api-version=2019-10-01-preview&format=csv" -verbose
```

Rufen Sie dann den Eigenschaftswert des asynchronen Vorgangs auf. Beispiel:

```
GET https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billingAccountName}/billingProfiles/{billingProfileID}/pricesheetDownloadOperations/{operation}?sessiontoken=0:11186&api-version=2019-10-01-preview
```
Der vorherige GET-Befehl gibt den Downloadlink mit dem Preisblatt zurück.


#### <a name="to-get-aggregated-costs"></a>So rufen Sie aggregierte Kosten ab

```
POST https://management.azure.com/providers/microsoft.billing/billingAccounts/{billingAccountName}/providers/microsoft.costmanagement/query?api-version=2019-10-01
```

#### <a name="create-a-budget-for-a-partner"></a>Erstellen eines Budgets für einen Partner

```
PUT https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billingAccountName}/providers/Microsoft.CostManagement/budgets/partnerworkshopbudget?api-version=2019-10-01
```

#### <a name="create-a-budget-for-a-customer"></a>Erstellen eines Budgets für einen Kunden

```
PUT https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billingAccountName}/customers/{customerID}/providers/Microsoft.Consumption/budgets/{budgetName}?api-version=2019-10-01
```

#### <a name="delete-a-budget"></a>Löschen eines Budgets

```
PUT
https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billingAccountId}/providers/Microsoft.CostManagement/budgets/{budgetName}?api-version=2019-10-01
```


## <a name="next-steps"></a>Nächste Schritte
- [Beginnen mit der Kostenanalyse](quick-acm-cost-analysis.md) in Cost Management
- [Erstellen und Verwalten von Budgets](tutorial-acm-create-budgets.md) in Cost Management
