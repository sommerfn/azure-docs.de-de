---
title: Grundlegendes zu detaillierter Nutzung und Gebühren | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Ihre detaillierte Nutzung und Gebühren lesen und verstehen.
author: bandersmsft
manager: micflan
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/24/2019
ms.author: banders
ms.openlocfilehash: 1842d32a838470d9b2af3a778c44c37464d32294
ms.sourcegitcommit: 3e7646d60e0f3d68e4eff246b3c17711fb41eeda
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2019
ms.locfileid: "68954341"
---
# <a name="understand-the-terms-in-your-azure-usage-and-charges-file"></a>Grundlegendes zu den Bedingungen in der Datei für die Azure-Nutzung und -Gebühren

Die Datei zu detaillierter Nutzung und Gebühren enthält die tägliche bewertete Nutzung basierend auf vereinbarten Tarifen, Einkäufen (z.B. Reservierungen, Marketplace-Gebühren) und Rückerstattungen für den angegebenen Zeitraum.
Die Gebühren beinhalten keine Gutschriften, Steuern oder andere Gebühren und Rabatte.
Die folgende Tabelle zeigt, welche Gebühren für jeden Kontotyp enthalten sind.

Kontotyp | Azure-Nutzung | Marketplace-Nutzung | Einkäufe | Rückerstattungen
--- | --- | --- | --- | ---
Enterprise Agreement (EA) | Ja | Ja | Ja | Nein
Microsoft-Kundenvereinbarung (Microsoft Customer Agreement, MCA) | Ja | Ja | Ja | Ja
Nutzungsbasierte Bezahlung (Pay-as-you-go, PAYG) | Ja | Ja | Nein | Nein

Weitere Informationen zu Marketplace-Bestellungen (auch als externe Dienstleistungen bezeichnet) finden Sie unter [Grundlegendes zu Azure-Gebühren für externe Dienste](billing-understand-your-azure-marketplace-charges.md).

Anweisungen für den Download finden Sie unter [Abrufen von Azure-Rechnungen und täglichen Nutzungsdaten](billing-download-azure-invoice-daily-usage-date.md).
Sie können Ihre Nutzungs- und Gebühren-CSV-Datei in Microsoft Excel oder einer anderen Tabellenkalkulationsanwendung öffnen.

## <a name="list-of-terms-and-descriptions"></a>Liste der Begriffe und Beschreibungen

In der folgenden Tabelle werden die wichtigen Begriffe in der neuesten Version der Datei für die Azure-Nutzung und -Gebühren erläutert.
Die Liste umfasst Konten für nutzungsbasierte Bezahlung (Pay-as-you-go, PAYG), Enterprise Agreement (EA) und Microsoft-Kundenvereinbarung (Microsoft Customer Agreement, MCA).

Begriff | Kontotyp | BESCHREIBUNG
--- | --- | ---
AccountName | EA, PAYG | Anzeigename des EA-Registrierungs- oder PAYG-Abrechnungskontos.
AccountOwnerId<sup>1</sup> | EA, PAYG | Eindeutiger Bezeichner des EA-Registrierungs- oder PAYG-Abrechnungskontos.
AdditionalInfo | Alle | Dienstspezifische Metadaten. Dies kann beispielsweise ein Imagetyp für einen virtuellen Computer sein.
BillingAccountId<sup>1</sup> | Alle | Eindeutiger Bezeichner für das Stammabrechnungskonto.
BillingAccountName | Alle | Name des Abrechnungskontos.
BillingCurrency | Alle | Dem Abrechnungskonto zugeordnete Währung.
BillingPeriod | EA, PAYG | Abrechnungszeitraum der Gebühren.
BillingPeriodEndDate | Alle | Enddatum des Abrechnungszeitraums.
BillingPeriodStartDate | Alle | Startdatum des Abrechnungszeitraums.
BillingProfileId<sup>1</sup> | Alle | Eindeutiger Bezeichner des EA-Registrierungs-, PAYG-Abonnement-, MCA-Abrechnungsprofil- oder konsolidierten AWS-Kontos.
BillingProfileName | Alle | Name des EA-Registrierungs-, PAYG-Abonnement-, MCA-Abrechnungsprofil- oder konsolidierten AWS-Kontos.
ChargeType | Alle | Gibt an, ob die Gebühren die Nutzung (**Nutzung**), einen Kauf (**Einkauf**) oder eine Erstattung (**Rückerstattung**) darstellt.
ConsumedService | Alle | Name des Diensts, dem die Gebühren zugeordnet sind.
CostCenter<sup>1</sup> | EA, MCA | Die Kostenstelle, die im Abonnement für die Nachverfolgung der Kosten festgelegt wurde (nur in offenen Abrechnungszeiträumen für MCA-Konten verfügbar).
Kosten | EA, PAYG | Siehe „CostInBillingCurrency“.
CostInBillingCurrency | MCA | Kosten der Gebühren in der Abrechnungswährung vor Gutschriften oder Steuern.
CostInPricingCurrency | MCA | Kosten der Gebühren in der Preiswährung vor Gutschriften oder Steuern.
Currency | EA, PAYG | Siehe „BillingCurrency“.
Date<sup>1</sup> | Alle | Verbrauchs- oder Kaufdatum der Gebühren.
EffectivePrice | Alle | Gemischter Einzelpreis für den Zeitraum. Mit den gemischten Preisen wird aus allen Einzelpreisschwankungen (wie z.B. abgestuftem Tiering) der Durchschnitt ermittelt, wodurch der Preis sinkt, da die Menge im Laufe der Zeit wächst.
ExchangeRateDate | MCA | Datum, an dem der Wechselkurs festgelegt wurde.
ExchangeRatePricingToBilling | MCA | Der für die Umrechnung der Kosten aus der Preiswährung in die Abrechnungswährung verwendete Wechselkurs.
Frequency | Alle | Gibt an, ob eine Gebühr sich voraussichtlich wiederholt. Gebühren fallen entweder einmal (**OneTime**), wiederholt monatlich oder jährlich (**Recurring**) oder basierend auf der Nutzung (**UsageBased**) an.
InvoiceId | PAYG, MCA | Die eindeutige Dokument-ID, die in der PDF-Datei für die Rechnung aufgeführt ist.
InvoiceSection | MCA | Siehe „InvoiceSectionName“.
InvoiceSectionId<sup>1</sup> | EA, MCA | Eindeutiger Bezeichner für EA-Abteilung oder MCA-Rechnungsabschnitt.
InvoiceSectionName | EA, MCA | Name für EA-Abteilung oder MCA-Rechnungsabschnitt.
IsAzureCreditEligible | Alle | Gibt an, ob die Gebühr mit einer Azure-Gutschrift bezahlt werden kann (Werte: True, False).
Location | MCA | Standort des Rechenzentrums, in dem die Ressource ausgeführt wird.
MeterCategory | Alle | Name der Klassifizierungskategorie der Verbrauchseinheit. Beispiele: *Clouddienste* und *Netzwerk*.
MeterId<sup>1</sup> | Alle | Der eindeutige Bezeichner für die Verbrauchseinheit.
MeterName | Alle | Der Name der Verbrauchseinheit.
MeterRegion | Alle | Der Name des Standorts des Rechenzentrums für auf Basis des Standorts abgerechnete Dienste. Siehe „Location“.
MeterSubCategory | Alle | Name der Unterklassifizierungskategorie der Verbrauchseinheit.
OfferId<sup>1</sup> | Alle | Der Name des erworbenen Angebots.
PartNumber<sup>1</sup> | EA, PAYG | Der Bezeichner, der verwendet wird, um bestimmte Verbrauchseinheitspreise zu erhalten.
PlanName | EA, PAYG | Name des Marketplace-Plans.
PreviousInvoiceId | MCA | Verweis auf die ursprüngliche Rechnung, wenn es sich bei diesem Posten um eine Rückerstattung handelt.
PricingCurrency | MCA | Währung, die verwendet wird, wenn die Bewertung basierend auf vereinbarten Preisen erfolgt.
Produkt | Alle | Der Name des Produkts.
ProductId<sup>1</sup> | MCA | Der eindeutige Bezeichner für das Produkt.
ProductOrderId | Alle | Der eindeutige Bezeichner für die Produktbestellung.
ProductOrderName | Alle | Der eindeutige Name für die Produktbestellung.
PublisherName | Alle | Herausgeber für Marketplace-Dienste.
PublisherType | Alle | Typ des Herausgebers (Werte: **Azure**, **AWS**, **Marketplace**).
Menge | Alle | Die Anzahl der erworbenen oder genutzten Einheiten.
ReservationId | EA, MCA | Der eindeutige Bezeichner für die erworbene Reservierungsinstanz.
ReservationName | EA, MCA | Der Name der erworbenen Reservierungsinstanz.
ResourceGroup | Alle | Name der [Ressourcengruppe](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview), in der sich die Ressource befindet.
ResourceId<sup>1</sup> | Alle | Der eindeutige Bezeichner der [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/resources)-Ressource.
ResourceLocation | Alle | Standort des Rechenzentrums, in dem die Ressource ausgeführt wird. Siehe „Location“.
ResourceName | EA, PAYG | Der Name der Ressource.
ResourceType | MCA | Der Ressourceninstanztyp.
ServiceFamily | MCA | Die Dienstfamilie, zu der der Dienst gehört.
ServiceInfo1 | Alle | Dienstspezifische Metadaten.
ServiceInfo2 | Alle | Ein Legacyfeld mit optionalen dienstspezifischen Metadaten.
ServicePeriodEndDate | MCA | Das Enddatum des Bewertungszeitraums, durch den die Preise für den genutzten oder erworbenen Dienst festgelegt wurden.
ServicePeriodStartDate | MCA | Das Startdatum des Bewertungszeitraums, durch den die Preise für den genutzten oder erworbenen Dienst festgelegt wurden.
SubscriptionId<sup>1</sup> | Alle | Der eindeutige Bezeichner für das Azure-Abonnement.
SubscriptionName | Alle | Der Name des Azure-Abonnements.
Tags<sup>1</sup> | Alle | Tags, die der Ressource zugewiesen werden. Umfasst keine Ressourcengruppen-Tags. Kann verwendet werden, um die Kosten für interne verbrauchsbasierte Kostenzuteilung zu gruppieren oder zu verteilen. Weitere Informationen finden Sie unter [Organisieren von Azure-Ressourcen mit Tags](https://azure.microsoft.com/updates/organize-your-azure-resources-with-tags/).
Begriff | Alle | Zeigt den Zeitraum für die Gültigkeit des Angebots an. Beispiel:  Bei reservierten Instanzen werden 12 Monate als „Zeitraum“ angezeigt. Für einmalige Käufe oder wiederkehrende Käufe beträgt „Zeitraum“ 1 Monat (SaaS, Marketplace-Support). Dies gilt nicht für den Azure-Verbrauch.
UnitOfMeasure | Alle | Die Abrechnungsmaßeinheiten für den Dienst. Computedienste werden beispielsweise pro Stunde abgerechnet.
UnitPrice | EA, PAYG | Der Preis pro Einheit für die Gebühren.

_<sup>**1**</sup> Felder zum Erstellen einer eindeutigen ID für einen einzelnen Kostendatensatz_

Beachten Sie, dass einige Felder bei den verschiedenen Kontotypen in Groß-/Kleinschreibung und Abstand abweichen können.
Ältere Versionen von Pay-as-you-go-Nutzungsdateien verfügen über separate Abschnitte für Abrechnung und tägliche Nutzung.

### <a name="list-of-terms-from-older-apis"></a>Liste der Begriffe aus älteren APIs
In der folgenden Tabelle werden die in älteren APIs verwendeten Begriffe den neuen Begriffen zugeordnet. Diese Beschreibungen finden Sie in der obigen Tabelle.

Alter Begriff | Neuer Begriff
--- | ---
ConsumedQuantity | Menge
IncludedQuantity | –
InstanceId | resourceId
Rate | EffectivePrice
Unit | UnitOfMeasure
UsageDate | Date
UsageEnd | Date
UsageStart | Date


## <a name="ensure-charges-are-correct"></a>Sicherstellen, dass Gebühren richtig sind

Weitere Informationen über detaillierte Nutzung und Gebühren finden Sie in den Grundlegenden Informationen zur Rechnung nach [nutzungsbasierter Bezahlung (Pay-as-you-go)](./billing-understand-your-bill.md) oder [Microsoft-Kundenvereinbarung](billing-mca-understand-your-bill.md).

## <a name="need-help-contact-us"></a>Sie brauchen Hilfe? Wenden Sie sich an uns.

Wenn Sie weitere Fragen haben oder Hilfe benötigen, [erstellen Sie eine Supportanfrage](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Nächste Schritte

- [Anzeigen und Herunterladen der Microsoft Azure-Rechnung](billing-download-azure-invoice.md)
- [Anzeigen und Herunterladen der Microsoft Azure-Nutzung und -Gebühren](billing-download-azure-daily-usage.md)
