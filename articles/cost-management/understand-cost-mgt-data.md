---
title: Verstehen der Daten in Azure Cost Management | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie mehr über die Daten, die in Azure Cost Management enthalten sind, und wie häufig diese verarbeitet, gesammelt, angezeigt und geschlossen werden.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 11/06/2019
ms.topic: conceptual
ms.service: cost-management
manager: micflan
ms.custom: ''
ms.openlocfilehash: 681ccc768b1fa3d5a968847d11987fbd83898b59
ms.sourcegitcommit: bc7725874a1502aa4c069fc1804f1f249f4fa5f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73721370"
---
# <a name="understand-cost-management-data"></a>Grundlegendes zu Cost Management-Daten

Dieser Artikel soll Ihnen helfen, die in Azure Cost Management enthaltenen Azure-Kosten und -Nutzungsdaten besser zu verstehen. Zudem wird darin erläutert, wie oft Daten verarbeitet, gesammelt, angezeigt und geschlossen werden. Die Nutzung von Azure wird Ihnen monatlich in Rechnung gestellt. Bei Abrechnungszeiträumen/Abrechnungszyklen handelt es sich um monatliche Zeiträume. Die Start- und Enddaten von Zyklen variieren jedoch je nach Abonnementtyp. Wie oft Cost Management Nutzungsdaten erhält, hängt von verschiedenen Faktoren ab. Zu diesen Faktoren gehört unter anderem, wie lange die Verarbeitung der Daten dauert und wie häufig Azure-Dienste die Nutzung an das Abrechnungssystem weitergeben.

Cost Management umfasst alle Nutzungen und Käufe, einschließlich Reservierungen und Angebote von Drittanbietern für Enterprise Agreement-Konten (EA). Konten der Microsoft-Kundenvereinbarung und einzelne Abonnements in Tarifen mit nutzungsbasierter Bezahlung umfassen nur die Nutzung von Azure- und Marketplace-Diensten. Support- und andere Kosten sind nicht enthalten. Kosten werden so lange geschätzt, bis eine Rechnung generiert wurde, und sie werden nicht bei Guthaben berücksichtigt.

## <a name="supported-microsoft-azure-offers"></a>Unterstützte Microsoft Azure-Angebote

Die folgenden Informationen zeigen die derzeit unterstützten [Microsoft Azure-Angebote](https://azure.microsoft.com/support/legal/offer-details/) im Azure Cost Management. Ein Azure-Angebot ist der Typ von Azure-Abonnement, das Sie besitzen. Die Daten sind ab dem in **Daten verfügbar ab** angegebenen Datum in Cost Management verfügbar. Wenn Angebote in einem Abonnement geändert werden, sind die Kosten vor dem Datum der Angebotsänderung nicht verfügbar.

| **Kategorie**  | **Angebotsname** | **Kontingent-ID** | **Angebotsnummer** | **Daten verfügbar ab** |
| --- | --- | --- | --- | --- |
| **Azure Government** | Azure Government Enterprise                                                         | EnterpriseAgreement_2014-09-01 | MS-AZR-USGOV-0017P | Mai 2014<sup>1</sup> |
| **Enterprise Agreement (EA)** | Enterprise Dev/Test                                                        | MSDNDevTest_2014-09-01 | MS-AZR-0148P | Mai 2014<sup>1</sup> |
| **Enterprise Agreement (EA)** | [Microsoft Azure Enterprise](https://azure.microsoft.com/offers/enterprise-agreement-support-upgrade) | EnterpriseAgreement_2014-09-01 | MS-AZR-0017P | Mai 2014<sup>1</sup> |
| **Microsoft-Kundenvereinbarung** | [Microsoft Azure-Plan](https://azure.microsoft.com/offers/ms-azr-0017g) | EnterpriseAgreement_2014-09-01 | – | März 2019<sup>3</sup> |
| **Microsoft-Kundenvereinbarung** | [Microsoft Azure-Plan für Dev/Test](https://azure.microsoft.com/offers/ms-azr-0148g) | MSDNDevTest_2014-09-01 | – | März 2019<sup>3</sup> |
| **Von Partnern unterstützte Microsoft-Kundenvereinbarung** | Microsoft Azure-Plan | CSP_2015-05-01, CSP_MG_2017-12-01 und CSPDEVTEST_2018-05-01<br><br>Die Kontingent-ID wird für Abonnements im Rahmen von Microsoft-Kundenvereinbarungen und für ältere CSP-Abonnements wiederverwendet. Derzeit werden nur Abonnements im Rahmen von Microsoft-Kundenvereinbarungen unterstützt. | – | Oktober 2019 |
| **Microsoft Developer Network (MSDN)** | [MSDN Platforms](https://azure.microsoft.com/offers/ms-azr-0062p)<sup>4</sup> | MSDN_2014-09-01 | MS-AZR-0062P | 2\. Oktober 2018<sup>2</sup> |
| **Nutzungsbasierte Bezahlung** | [Nutzungsbasierte Bezahlung](https://azure.microsoft.com/offers/ms-azr-0003p)                  | PayAsYouGo_2014-09-01 | MS-AZR-0003P | 2\. Oktober 2018<sup>2</sup> |
| **Nutzungsbasierte Bezahlung** | [Pay-As-You-Go Dev/Test](https://azure.microsoft.com/offers/ms-azr-0023p)         | MSDNDevTest_2014-09-01 | MS-AZR-0023P | 2\. Oktober 2018<sup>2</sup> |
| **Nutzungsbasierte Bezahlung** | [Microsoft Partner Network](https://azure.microsoft.com/offers/ms-azr-0025p)      | MPN_2014-09-01 | MS-AZR-0025P | 2\. Oktober 2018<sup>2</sup> |
| **Nutzungsbasierte Bezahlung** | [Kostenlose Testversion](https://azure.microsoft.com/offers/ms-azr-0044p)<sup>4</sup>         | FreeTrial_2014-09-01 | MS-AZR-0044P | 2\. Oktober 2018<sup>2</sup> |
| **Nutzungsbasierte Bezahlung** | [Azure in Open](https://azure.microsoft.com/offers/ms-azr-0111p)<sup>4</sup>      | AzureInOpen_2014-09-01 | MS-AZR-0111P | 2\. Oktober 2018<sup>2</sup> |
| **Nutzungsbasierte Bezahlung** | Azure Pass<sup>4</sup>                                                            | AzurePass_2014-09-01 | MS-AZR-0120P, MS-AZR-0122P – MS-AZR-0125P, MS-AZR-0128P – MS-AZR-0130P | 2\. Oktober 2018<sup>2</sup> |
| **Visual Studio** | [Visual Studio Enterprise – MPN](https://azure.microsoft.com/offers/ms-azr-0029p)<sup>4</sup>     | MPN_2014-09-01 | MS-AZR-0029P | 2\. Oktober 2018<sup>2</sup> |
| **Visual Studio** | [Visual Studio Professional](https://azure.microsoft.com/offers/ms-azr-0059p)<sup>4</sup>         | MSDN_2014-09-01 | MS-AZR-0059P | 2\. Oktober 2018<sup>2</sup> |
| **Visual Studio** | [Visual Studio Test Professional](https://azure.microsoft.com/offers/ms-azr-0060p)<sup>4</sup>    | MSDNDevTest_2014-09-01 | MS-AZR-0060P | 2\. Oktober 2018<sup>2</sup> |
| **Visual Studio** | [Visual Studio Enterprise](https://azure.microsoft.com/offers/ms-azr-0063p)<sup>4</sup>           | MSDN_2014-09-01 | MS-AZR-0063P | 2\. Oktober 2018<sup>2</sup> |
| **Visual Studio** | [Visual Studio Enterprise: BizSpark](https://azure.microsoft.com/offers/ms-azr-0064p)<sup>4</sup> | MSDN_2014-09-01 | MS-AZR-0064P | 2\. Oktober 2018<sup>2</sup> |

_<sup>**1** </sup> Daten vor Mai 2014 finden Sie im [Azure Enterprise Portal](https://ea.azure.com)._

_<sup>**2**</sup> Daten vor dem 2. Oktober 2018 finden Sie im [Azure-Kontocenter](https://account.azure.com/subscriptions)._

_<sup>**3** </sup> Microsoft-Kundenvereinbarungen wurden ab März 2019 abgeschlossen und weisen keine Verlaufsdaten vor diesem Zeitpunkt auf._

_<sup>**4**</sup> Verlaufsdaten für Abonnements, die auf Guthaben oder Vorauszahlung basieren, stimmen möglicherweise nicht mit Ihrer Rechnung überein. Siehe [Verlaufsdaten stimmen möglicherweise nicht mit Ihrer Rechnung überein](#historical-data-might-not-match-invoice) weiter unten._

Folgende Angebote werden noch nicht unterstützt:

| Category  | **Angebotsname** | **Kontingent-ID** | **Angebotsnummer** |
| --- | --- | --- | --- |
| **Azure Deutschland** | [Azure Deutschland – nutzungsbasierte Bezahlung](https://azure.microsoft.com/offers/ms-azr-de-0003p) | PayAsYouGo_2014-09-01 | MS-AZR-DE-0003P |
| **Cloud Solution Provider (CSP)** | Microsoft Azure                                    | CSP_2015-05-01 | MS-AZR-0145P |
| **Cloud Solution Provider (CSP)** | Azure Government CSP                               | CSP_2015-05-01 | MS-AZR-USGOV-0145P |
| **Cloud Solution Provider (CSP)** | Azure Deutschland in CSP für die Microsoft Cloud Deutschland   | CSP_2015-05-01 | MS-AZR-DE-0145P |
| **Nutzungsbasierte Bezahlung**                 | Azure for Students Starter | DreamSpark_2015-02-01 | MS-AZR-0144P |
| **Nutzungsbasierte Bezahlung** | [Azure for Students](https://azure.microsoft.com/offers/ms-azr-0170p)<sup>4</sup> | AzureForStudents_2018-01-01 | MS-AZR-0170P |
| **Nutzungsbasierte Bezahlung**                 | [Microsoft Azure Sponsorship](https://azure.microsoft.com/offers/ms-azr-0036p/) | Sponsored_2016-01-01 | MS-AZR-0036P |
| **Supportpläne** | Standard-Support                    | Default_2014-09-01 | MS-AZR-0041P |
| **Supportpläne** | Professional Direct-Support         | Default_2014-09-01 | MS-AZR-0042P |
| **Supportpläne** | Developer Support                   | Default_2014-09-01 | MS-AZR-0043P |
| **Supportpläne** | Supportplan Deutschland                | Default_2014-09-01 | MS-AZR-DE-0043P |
| **Supportpläne** | Azure Government Standard Support   | Default_2014-09-01 | MS-AZR-USGOV-0041P |
| **Supportpläne** | Azure Government Pro-Direct Support | Default_2014-09-01 | MS-AZR-USGOV-0042P |
| **Supportpläne** | Azure Government Developer Support  | Default_2014-09-01 | MS-AZR-USGOV-0043P |

## <a name="determine-your-offer-type"></a>Bestimmen des Angebotstyps
Wenn Sie keine Daten für ein Abonnement sehen und feststellen möchten, ob Ihr Abonnement zu den unterstützten Angeboten gehört, können Sie überprüfen, ob Ihr Abonnement unterstützt wird. Um zu überprüfen, ob ein Azure-Abonnement unterstützt wird, melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. Wählen Sie anschließend **Alle Dienste** im linken Menübereich aus. Wählen Sie in der Liste der Dienste **Abonnements** aus. Klicken Sie im Menü der Abonnentenliste auf das Abonnement, das Sie überprüfen möchten. Ihr Abonnement wird auf der Registerkarte „Übersicht“ angezeigt und Sie können das **Angebot** und **Angebots-ID** sehen. Die folgende Abbildung zeigt ein Beispiel.

![Beispiel für die Registerkarte „Abonnementübersicht“ mit Anzeige von Angebot und Angebots-ID](./media/understand-cost-mgt-data/offer-and-offer-id.png)

## <a name="costs-included-in-cost-management"></a>In Cost Management enthaltene Kosten

Die folgenden Tabellen zeigen Daten, die in Cost Management enthalten sind oder nicht. Alle Kosten werden so lange geschätzt, bis eine Rechnung generiert wurde. Die angezeigten Kosten enthalten keine kostenlosen und Prepaid-Guthaben.

**Kosten und Nutzungsdaten**

| **Enthalten** | **Nicht enthalten** |
| --- | --- |
| Nutzung durch Azure-Dienste<sup>5</sup>        | Supportgebühren – Weitere Informationen finden Sie unter [Erläuterung der Rechnungsbedingungen](../billing/billing-understand-your-invoice.md). |
| Nutzung von Marketplace-Angeboten<sup>6</sup> | Steuern – Weitere Informationen finden Sie unter [Erläuterung der Rechnungsbedingungen](../billing/billing-understand-your-invoice.md). |
| Marketplace-Käufe<sup>6</sup>      | Gutschriften – Weitere Informationen finden Sie unter [Erläuterung der Rechnungsbedingungen](../billing/billing-understand-your-invoice.md). |
| Reservierungskäufe<sup>7</sup>      |  |
| Amortisierung von Reservierungskäufen<sup>7</sup>      |  |

_<sup>**5**</sup> Die Nutzung des Azure-Diensts basiert auf Reservierungen und ausgehandelten Preisen._

_<sup>**6**</sup> Marketplace-Käufe sind für die nutzungsbasierte Bezahlung, MSDN und Visual Studio-Angebote derzeit nicht verfügbar._

_<sup>**7** </sup> Reservierungskäufe sind zurzeit nur für Enterprise Agreement-Konten (EA) verfügbar._

**Metadaten**

| **Enthalten** | **Nicht enthalten** |
| --- | --- |
| Ressourcentags<sup>8</sup> | Ressourcengruppen-Tags |

_<sup>**8**</sup> Ressourcentags werden verwendet, wenn die Nutzung von jedem Dienst ausgegeben wird, und sind nicht rückwirkend für die historische Nutzung verfügbar._

## <a name="rated-usage-data-refresh-schedule"></a>Zeitplan zur Datenaktualisierung der bewerteten Nutzung

Kosten- und Nutzungsdaten sind i Cost Management + Abrechnung im Azure-Portal und in den [unterstützenden APIs](index.yml) verfügbar. Beachten Sie beim Überprüfen der Kosten folgende Punkte:

- Die geschätzten Gebühren für den aktuellen Abrechnungszeitraum werden sechsmal pro Tag aktualisiert.
- Die geschätzten Gebühren für den aktuellen Abrechnungszeitraum können sich mit zunehmender Nutzung ändern.
- Jede Aktualisierung ist kumulativ und enthält alle Einzelposten und Informationen der vorherigen Aktualisierung.
- Azure finalisiert oder _schließt_ den aktuellen Abrechnungszeitraum bis zu 72 Stunden (drei Kalendertage) nach Ablauf des Abrechnungszeitraums.

Die folgenden Beispiele veranschaulichen, wie Abrechnungszeiträume enden können.

Abonnements mit Enterprise Agreement (EA) – Wenn der Abrechnungsmonat am 31. März endet, werden die geschätzten Gebühren bis zu 72 Stunden später aktualisiert. In diesem Beispiel bis Mitternacht am (UTC) 4. April.

Abonnements mit nutzungsbasierter Bezahlung – Wenn der Abrechnungsmonat am 15. Mai endet, können die geschätzten Gebühren bis zu 72 Stunden später aktualisiert werden. In diesem Beispiel bis Mitternacht am (UTC) 19. April.

### <a name="rerated-data"></a>Neu bewertete Daten

Unabhängig davon, ob Sie die [Cost Management-APIs](index.yml), Power BI oder das Azure-Portal zum Abrufen von Daten verwenden, ist zu erwarten, dass die Gebühren des aktuellen Abrechnungszeitraums neu bewertet werden und sich somit ändern, bis die Rechnung geschlossen ist.

## <a name="cost-management-data-fields"></a>Cost Management-Datenfelder

Die folgenden Datenfelder befinden sich im Nutzungsdetaildateien und Cost Management-APIs. Für die folgenden Felder in Fettschrift können Partner Filter- und Gruppierungsfeatures in der Kostenanalyse verwenden, um die Kosten anhand mehrerer Feldern zu analysieren. Beide Felder gelten nur für Microsoft-Kundenvereinbarungen, die von Partnern unterstützt werden.

| **Feldname** | **Beschreibung** |
| --- | --- |
| invoiceId | Rechnungs-ID auf der Rechnung für die jeweilige Transaktion. |
| previousInvoiceID | Verweis auf die ursprüngliche Rechnung, wenn es sich um eine Rückerstattung handelt (negative Kosten). Wird nur aufgefüllt, wenn eine Rückerstattung vorliegt. |
| billingAccountName | Name des Abrechnungskontos, das den Partner repräsentiert. Hier werden alle Kosten für folgenden Kundengruppen zusammengefasst: alle Kunden, die eine Microsoft-Kundenvereinbarung abgeschlossen haben, sowie für alle CSP-Kunden, die berechtigungsbasierte Käufe getätigt haben, z. B. über SaaS, Azure Marketplace oder Reservierungen. |
| billingAccountID | Bezeichner des Abrechnungskontos, das den Partner repräsentiert. |
| billingProfileID | Bezeichner des Abrechnungsprofils, in dem Kosten für folgende Kundengruppen rechnungsübergreifend in einer einzigen Abrechnungswährung zusammengefasst werden: alle Kunden, die eine Microsoft-Kundenvereinbarung abgeschlossen haben, sowie alle CSP-Kunden, die berechtigungsbasierte Käufe getätigt haben, z. B. über SaaS, Azure Marketplace oder Reservierungen. |
| billingProfileName | Name des Abrechnungsprofils, in dem Kosten für folgende Kundengruppen rechnungsübergreifend in einer einzigen Abrechnungswährung zusammengefasst werden: alle Kunden, die eine Microsoft-Kundenvereinbarung abgeschlossen haben, sowie alle CSP-Kunden, die berechtigungsbasierte Käufe getätigt haben, z. B. über SaaS, Azure Marketplace oder Reservierungen. |
| invoiceSectionName | Name des Projekts, das in Rechnung gestellt wird. Gilt nicht für Microsoft-Kundenvereinbarungen, die über Partner abgeschlossen wurden. |
| invoiceSectionID | Bezeichner des Projekts, das in Rechnung gestellt wird. Gilt nicht für Microsoft-Kundenvereinbarungen, die über Partner abgeschlossen wurden. |
| **CustomerTenantID** | Bezeichner des Azure Active Directory-Mandanten des Kundenabonnements. |
| **CustomerName** | Name des Azure Active Directory-Mandanten des Kundenabonnements. |
| **CustomerTenantDomainName** | Domänenname des Azure Active Directory-Mandanten des Kundenabonnements. |
| **PartnerTenantID** | Bezeichner des Azure Active Directory-Mandanten des Partners. |
| **PartnerName** | Name des Azure Active Directory-Mandanten des Partners. |
| **ResellerMPNID** | MPNID des Handelspartners, der dem Abonnement zugeordnet ist. |
| costCenter | Dem Abonnement zugeordnete Kostenstelle. |
| billingPeriodStartDate | Startdatum des Abrechnungszeitraums, wie auf der Rechnung aufgeführt. |
| billingPeriodEndDate | Enddatum des Abrechnungszeitraums, wie auf der Rechnung aufgeführt. |
| servicePeriodStartDate | Startdatum des Bewertungszeitraums, für den die Dienstnutzung zur Gebührenermittlung bewertet wurde. Die Preise für Azure-Dienste werden basierend auf dem Bewertungszeitraum bestimmt. |
| servicePeriodEndDate | Enddatum des Bewertungszeitraums, für den die Dienstnutzung zur Gebührenermittlung bewertet wurde. Die Preise für Azure-Dienste werden basierend auf dem Bewertungszeitraum bestimmt. |
| date | Bei Azure-Verbrauchsdaten wird das bewertete Nutzungsdatum angezeigt. Bei reservierten Instanzen wird das Datum des Erwerbs angezeigt. Bei wiederkehrenden und einmaligen Gebühren beispielsweise für Marketplace und Support wird das Datum des Erwerbs angezeigt. |
| productID | Bezeichner des Produkts, für das basierend auf Verbrauch oder Erwerb Gebühren anfallen. Hierbei handelt es sich um den aus productID und SkuID verketteten Product Key, wie im Partner Center angezeigt. |
| product | Name des Produkts, für das basierend auf Verbrauch oder Erwerb Gebühren anfallen, wie auf der Rechnung angezeigt. |
| serviceFamily | Zeigt die Dienstfamilie des erworbenen oder gebührenpflichtigen Produkts an. Beispiel: „Storage“ oder „Compute“. |
| productOrderID | Der Bezeichner der Ressource oder des Azure-Plans, der bzw. dem das Abonnement angehört. Beispiel: „Azure-Plan“. |
| productOrderName | Der Name des Azure-Plans, zu dem das Abonnement gehört. Beispiel: „Azure-Plan“. |
| consumedService | Der genutzte Dienst (Legacytaxonomie), wie in den Legacy-EA-Nutzungsdetails verwendet. |
| meterID | Der Bezeichner der Verbrauchseinheit für die gemessene Nutzung. |
| meterName | Identifiziert den Namen der Verbrauchseinheit für die gemessene Nutzung. |
| meterCategory | Identifiziert den Dienst der obersten Ebene für die Nutzung an. |
| meterSubCategory | Definiert den Typ oder die Unterkategorie des Azure-Diensts, der bzw. die sich auf den Tarif auswirken kann. |
| meterRegion | Gibt den Standort des Rechenzentrums für bestimmte Dienste an, die basierend auf dem Standort des Rechenzentrums berechnet werden. |
| Abonnement-ID | Der eindeutige von Microsoft generierte Bezeichner für das Azure-Abonnement. |
| subscriptionName | Der Name des Azure-Abonnements. |
| Begriff | Zeigt den Zeitraum für die Gültigkeit des Angebots an. Für reservierte Instanzen werden beispielsweise 12 Monate eines Jahreszeitraums der reservierten Instanz angezeigt. Bei einmaligen oder wiederkehrenden Käufen für SaaS, Azure Marketplace und Support zeigt der Zeitraum einen Monat an. Dies gilt nicht für die Azure-Nutzung. |
| publisherType (firstParty, thirdPartyReseller, thirdPartyAgency) | Der Herausgebertyp, der den Herausgeber als Erstanbieter, Drittanbieter-Handelspartner oder Drittanbieter-Agentur identifiziert. |
| partNumber | Teilenummer für nicht genutzte reservierte Instanzen und Azure Marketplace-Dienste. |
| publisherName | Name des Herausgebers des Diensts – Microsoft oder anderer Herausgeber. |
| reservationId | Bezeichner für die erworbene reservierte Instanz. |
| reservationName | Name der reservierten Instanz. |
| reservationOrderId | Auftrags-ID der reservierten Instanz. |
| frequency | Zahlungshäufigkeit für eine reservierte Instanz. |
| resourceGroup | Name der Azure-Ressourcengruppe, der für die Lebenszyklusverwaltung der Ressource verwendet wird. |
| instanceID (oder) ResourceID | Bezeichner der Ressourceninstanz. |
| resourceLocation | Name des Ressourcenspeicherorts. |
| Location | Normalisierter Speicherort der Ressourcengruppe. |
| effectivePrice | Der geltende Einheitenpreis für den Dienst in der Abrechnungswährung. Dieser Preis ist für jedes Produkt, jede Dienstfamilie, jede Verbrauchseinheit und jedes Angebot einzigartig. Wird mit den Preisen im Preisblatt für das Abrechnungskonto verwendet. Bei Stufenpreisen oder enthaltenen Mengen wird der gemischte Preis für die Nutzung angezeigt. |
| Menge | Die gemessene Menge, die erworben oder genutzt wurde. Die Menge der Verbrauchseinheit, die während des Abrechnungszeitraums genutzt wurde. |
| unitOfMeasure | Gibt die Einheit an, in der der Dienst in Rechnung gestellt wird. Beispiele: GB oder Stunden. |
| pricingCurrency | Die Währung, die den Einheitenpreis definiert. |
| billingCurrency | Die Währung, in der die abgerechneten Kosten definiert sind. |
| chargeType | Definiert den Gebührentyp, den die Kosten in Azure Cost Management repräsentieren, z. B. Erwerb oder Erstattung. |
| costinBillingCurrency | Geschätzte erweiterte oder gemischte Kosten vor Steuern in der Abrechnungswährung. |
| costinPricingCurrency | Erweiterte oder gemischte Kosten vor Steuern in der Währung, die mit den Preisen korreliert. |
| **costinUSD** | Geschätzte erweiterte oder gemischte Kosten vor Steuern in USD. |
| **paygCostInBillingCurrency** | Zeigt Kosten an, wenn die Preise als Verkaufspreise angegeben sind. Zeigt die Preise für die nutzungsbasierte Bezahlung in der Abrechnungswährung an. Nur verfügbar bei RBAC-Bereichen. |
| **paygCostInUSD** | Zeigt Kosten an, wenn die Preise als Verkaufspreise angegeben sind. Zeigt die Preise für die nutzungsbasierte Bezahlung in USD an. Nur verfügbar bei RBAC-Bereichen. |
| exchangeRate | Der für die Umrechnung aus der Preiswährung in die Abrechnungswährung verwendete Wechselkurs. |
| exchangeRateDate | Das Datum des für die Umrechnung aus der Preiswährung in die Abrechnungswährung verwendeten Wechselkurses. |
| isAzureCreditEligible | Gibt an, ob die Kosten mit Azure-Guthaben bezahlt werden können. |
| serviceInfo1 | Ein Legacyfeld, in dem optionale dienstspezifische Metadaten erfasst werden. |
| serviceInfo2 | Ein Legacyfeld, in dem optionale dienstspezifische Metadaten erfasst werden. |
| additionalInfo | Dienstspezifische Metadaten. Dies kann beispielsweise ein Imagetyp für einen virtuellen Computer sein. |
| tags | Ein Tag, den Sie der Verbrauchseinheit zuweisen. Verwenden Sie Tags zum Gruppieren von Abrechnungsdatensätzen. Beispielsweise können Sie Tags verwenden, um Kosten nach den Abteilungen zu unterteilen, die die Verbrauchseinheit nutzen. |
| **partnerEarnedCreditRate** | Rabatt, der angewendet wird, wenn basierend auf dem Zugriff über einen Partneradministratorlink ein Partner Earned Credit (PEC) vorhanden ist. |
| **partnerEarnedCreditApplied** | Gibt an, ob der Partner Earned Credit angewendet wurde. |


## <a name="usage-data-update-frequency-varies"></a>Die Häufigkeit der Aktualisierung der Nutzungsdaten variiert.

Die Verfügbarkeit Ihrer anfallenden Nutzungsdaten in Cost Management hängt von einer Reihe von Faktoren ab, darunter:

- Wie häufig Azure-Dienste (wie z.B. Speicher, Compute, CDN und SQL) die Nutzung ausgeben.
- Die Zeit, die für die Verarbeitung der Nutzungsdaten durch die Rating-Engine und die Cost Management-Pipelines benötigt wird.

Einige Dienste geben die Nutzung häufiger als andere ab. Daher können Sie für einige Dienste Daten in Cost Management früher sehen als für andere Dienste, die seltener Daten ausgeben. In der Regel dauert es ach bis 24 Stunden bis die Nutzung von Diensten in Cost Management angezeigt wird. Beachten Sie, dass die Daten für einen offenen Monat aktualisiert werden, wenn Sie den Dienst häufiger nutzen, da Aktualisierungen kumulativ sind.

## <a name="historical-data-might-not-match-invoice"></a>Verlaufsdaten stimmen möglicherweise nicht mit Rechnung überein

Verlaufsdaten für Angebote, die auf Guthaben oder Vorauszahlung basieren, stimmen möglicherweise nicht mit Ihrer Rechnung überein. Bei einigen Azure-Angeboten der Kategorien „Nutzungsbasierte Bezahlung“, „MSDN“ und „Visual Studio“ können Azure-Guthaben und Vorauszahlungen auf die Rechnung angewendet werden. Die in Cost Management angezeigten Verlaufsdaten basieren allerdings nur auf den geschätzten Nutzungsgebühren. Cost Management-Verlaufsdaten enthalten keine Zahlungen und Guthaben. Daher stimmen die Verlaufsdaten, die für die folgenden Angebote angezeigt werden, möglicherweise nicht genau mit Ihrer Rechnung überein.

- Azure for Students (MS-AZR-0170P)
- Azure in Open (MS-AZR-0111P)
- Azure Pass (MS-AZR-0120P, MS-AZR-0123P, MS-AZR-0125P, MS-AZR-0128P, MS-AZR-0129P)
- Kostenlose Testversion (MS-AZR-0044P)
- MSDN (MS-AZR-0062P)
- Visual Studio (MS-AZR-0029P, MS-AZR-0059P, MS-AZR-0060P, MS-AZR-0063P, MS-AZR-0064P)

## <a name="see-also"></a>Weitere Informationen

- Falls Sie den ersten Schnellstart für Cost Management noch nicht abgeschlossen haben, lesen Sie ihn unter [Erste Schritte in der Analyse von Kosten](quick-acm-cost-analysis.md).
