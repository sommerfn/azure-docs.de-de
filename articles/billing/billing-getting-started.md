---
title: Vermeiden unerwarteter Kosten und Verwalten der Abrechnung in Azure
description: Hier erwarten Sie, wie Sie unerwartete Gebühren in Ihrer Azure-Abrechnung vermeiden. Verwenden Sie Kostenüberwachungs- und Kostenverwaltungsfeatures für Ihr Azure-Konto.
author: bandersmsft
manager: amberb
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/01/2019
ms.author: banders
ms.openlocfilehash: 6f7a29adfbd145be11f9f6c91e8e66dd229fed62
ms.sourcegitcommit: 0576bcb894031eb9e7ddb919e241e2e3c42f291d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "72375638"
---
# <a name="prevent-unexpected-charges-with-azure-billing-and-cost-management"></a>Vermeiden unerwarteter Gebühren bei der Azure-Abrechnung und -Kostenverwaltung

Wenn Sie sich für Azure registrieren, sollten Sie sich einen Überblick über Ihre Ausgaben verschaffen:

- Informieren Sie sich vor dem Hinzufügen von Diensten über die voraussichtlichen Kosten – entweder mithilfe des [Preisrechners](https://azure.microsoft.com/pricing/calculator/), anhand des Azure-Preisblatts oder beim Hinzufügen von Diensten im Azure-Portal.
- Überwachen Sie Kosten mithilfe von [Budgets](../cost-management/tutorial-acm-create-budgets.md), [Warnungen](../cost-management/cost-mgt-alerts-monitor-usage-spending.md) und [Kostenanalyse](../cost-management/quick-acm-cost-analysis.md).
- Überprüfen Sie die Gebühren auf Ihrer Rechnung, indem Sie sie mit [detaillierten Nutzungsdateien](billing-download-azure-invoice-daily-usage-date.md) vergleichen.
- Integrieren Sie Abrechnungs- und Kostendaten mithilfe von APIs für die [Abrechnung](https://docs.microsoft.com/rest/api/billing/) und [Nutzung](https://docs.microsoft.com/rest/api/consumption/) in Ihr eigenes Berichterstellungssystem.
- Verwenden Sie zusätzliche Ressourcen und Tools für EA-Kunden (Enterprise Agreement, Konzernvertrag), CSP-Kunden (Cloud Solution Provider, Cloudlösungsanbieter) und Azure Sponsorship-Kunden.
- Nutzen Sie [einige der beliebtesten Azure-Dienste 12 Monate lang kostenlos](billing-create-free-services-included-free-account.md) (verfügbar mit dem [kostenlosen Azure-Konto](https://azure.microsoft.com/free/)). Weitere Informationen finden Sie zusammen mit den unten aufgelisteten Empfehlungen unter [Vermeiden Sie, dass Ihnen für Ihr kostenloses Azure-Konto Gebühren angezeigt werden](billing-avoid-charges-free-account.md).

## <a name="get-estimated-costs-before-adding-azure-services"></a>Abrufen der geschätzten Kosten vor dem Hinzufügen von Azure-Diensten

Verwenden Sie eine der folgenden Optionen, um die voraussichtlichen Kosten für die Verwendung eines Azure-Diensts zu ermitteln:
- Azure-Preisrechner
- Azure-Preisblatt
- Azure-Portal

Die Abbildungen in den folgenden Abschnitten zeigen Beispielpreise in US-Dollar.

### <a name="estimate-cost-online-using-the-pricing-calculator"></a>Schätzen der Kosten mithilfe des Onlinepreisrechners

Verschaffen Sie sich mit dem [Preisrechner](https://azure.microsoft.com/pricing/calculator/) einen ungefähren Eindruck von den monatlichen Kosten des Diensts, den Sie hinzufügen möchten. Sie können die Währung ändern, um die Schätzung in Ihrer Landeswährung zu erhalten.

![Screenshot des Preisrechnermenüs](./media/billing-getting-started/pricing-calc.png)

Sie können die voraussichtlichen Kosten für jeden Azure-Erstanbieterdienst anzeigen. Im folgenden Screenshot fallen beispielsweise für die Computestunden eines virtuellen A1-Windows-Computers voraussichtlich 66,96 USD im Monat an, wenn der Computer durchgehend in Betrieb ist:

![Screenshot des Preisrechners, der die geschätzten Kosten pro Monat für einen virtuellen A1-Windows-Computer anzeigt](./media/billing-getting-started/pricing-calcvm.png)

Weitere Informationen zu den Preisen finden Sie unter [Häufig gestellte Fragen zu Azure-Preisen](https://azure.microsoft.com/pricing/faq/). Wenn Sie mit einer für den Azure-Vertrieb zuständigen Person sprechen möchten, rufen Sie die Telefonnummer oben auf der FAQ-Seite an.

### <a name="view-and-download-azure-price-sheet"></a>Anzeigen und Herunterladen des Azure-Preisblatts

Wenn Sie im Rahmen eines Konzernvertrags (Enterprise Agreement, EA) oder einer Microsoft-Kundenvereinbarung (Microsoft Customer Agreement, MCA) auf Azure zugreifen, können Sie das Preisblatt für Ihr Azure-Konto anzeigen und herunterladen. Bei dem Preisblatt handelt es sich um eine Excel-Datei mit Preisen für alle Azure-Dienste. Weitere Informationen finden Sie unter [Anzeigen und Herunterladen der Azure-Preise für Ihre Organisation](billing-ea-pricing.md).

### <a name="review-estimated-costs-in-the-azure-portal"></a>Überprüfen von geschätzten Kosten im Azure-Portal

Sie können die voraussichtlichen monatlichen Kosten beim Hinzufügen eines Diensts im Azure-Portal anzeigen. Wenn Sie also etwa die Größe Ihres virtuellen Windows-Computers auswählen, werden die voraussichtlichen monatlichen Kosten für die Computestunden angezeigt:

![Beispiel: ein virtueller A1-Windows-Computer mit den geschätzten Kosten pro Monat](./media/billing-getting-started/vm-size-cost.png)

## <a name="monitor-costs-when-using-azure-services"></a>Überwachen von Kosten bei Verwendung von Azure-Diensten
Mit den folgenden Tools können Sie Kosten überwachen:

- Budgets und Kostenwarnungen
- Kostenanalyse

### <a name="track-costs-with-budgets-and-cost-alerts"></a>Nachverfolgen von Kosten mithilfe von Budgets und Kostenwarnungen

Erstellen Sie [Budgets](../cost-management/tutorial-acm-create-budgets.md), um Kosten zu verwalten, und [Warnungen](../cost-management/cost-mgt-alerts-monitor-usage-spending.md), damit Sie und die Projektbeteiligten automatisch über Ausgabenanomalien und zu hohe Ausgaben informiert werden. 

### <a name="costs"></a> Ermitteln und Analysieren von Kosten mithilfe der Kostenanalyse

Wenn Ihre Azure-Dienste ausgeführt werden, sollten Sie regelmäßig die Kosten überprüfen, um Ihre Azure-Ausgaben zu überwachen. Mithilfe der Kostenanalyse können Sie nachvollziehen, woher Kosten für Ihre Azure-Nutzung stammen. 

1. Navigieren Sie im Azure-Portal zur Seite [Kostenverwaltung + Abrechnung](https://portal.azure.com/#blade/Microsoft_Azure_Billing/ModernBillingMenuBlade).

2. Klicken Sie links auf dem Bildschirm auf **Kostenanalyse**, um die aktuellen Kosten anzuzeigen. Diese sind nach verschiedenen Pivots wie Dienst, Standort und Abonnement aufgeschlüsselt. Warten Sie nach dem Hinzufügen eines Diensts oder nach einem Kauf 24 Stunden, bis die Daten angezeigt werden. In der Kostenanalyse werden standardmäßig die Kosten für den Bereich angezeigt, in dem Sie sich befinden. Im folgenden Screenshot werden beispielsweise die Kosten für das Contoso-Abrechnungskonto angezeigt. Über das Bereichselement können Sie zu einem anderen Bereich der Kostenanalyse wechseln. Weitere Informationen zu Bereichen finden Sie unter [Verstehen von und Arbeiten mit Bereichen](../cost-management/understand-work-scopes.md#scopes).

    ![Screenshot der Kostenanalyseansicht im Azure-Portal](./media/billing-getting-started/cost-analysis.png)

4. Sie können nach verschiedenen Eigenschaften filtern – etwa nach Tags, Ressourcentyp und Zeitspanne. Klicken Sie auf **Filter hinzufügen**, um den Filter für eine Eigenschaft hinzuzufügen, und wählen Sie die gewünschten Filterwerte aus. Wählen Sie **Exportieren** aus, um die Ansicht in eine CSV-Datei (Datei mit durch Trennzeichen getrennten Werten) zu exportieren.

5. Außerdem können Sie auf die Bezeichnungen des Diagramms klicken, um den täglichen Ausgabenverlauf für die entsprechende Bezeichnung anzuzeigen. Beispiel: Wenn Sie im folgenden Screenshot auf „virtual machines“ (virtuelle Computer) klicken, werden die täglichen Kosten für den Betrieb Ihrer virtuellen Computer angezeigt.

    ![Screenshot der Ausgabenverlaufsansicht im Azure-Portal](./media/billing-getting-started/costhistory.png)

## <a name="optimize-and-reduce-costs"></a>Optimieren und Kosten senken
Wenn Sie mit den Prinzipien der Kostenverwaltung nicht vertraut sind, lesen Sie [Optimieren der Cloudinvestitionen mit Azure Cost Management](../cost-management/cost-mgt-best-practices.md).

Im Azure-Portal können Sie durch automatisches Herunterfahren von virtuellen Computern und anhand von Advisor-Empfehlungen auch Azure-Kosten optimieren und reduzieren.

### <a name="consider-cost-cutting-features-like-auto-shutdown-for-vms"></a>Nutzung von Kostensenkungsfeatures wie automatisches Herunterfahren für virtuelle Computer

Abhängig von Ihrem Szenario können Sie im Azure-Portal ggf. das automatische Herunterfahren für Ihre virtuellen Computer konfigurieren. Weitere Informationen finden Sie unter [Announcing auto-shutdown for VMs using Azure Resource Manager](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/) (Ankündigung des automatischen Herunterfahrens für virtuelle Computer mit Azure Resource Manager).

![Screenshot der Option für automatisches Herunterfahren im Portal](./media/billing-getting-started/auto-shutdown.png)

Das automatische Herunterfahren ist nicht dasselbe wie das Herunterfahren des virtuellen Computers über die Energieoptionen. Beim automatischen Herunterfahren werden Ihre virtuellen Computer beendet, und ihre Zuordnung wird aufgehoben, um zu verhindern, dass weitere Nutzungsgebühren anfallen. Weitere Informationen finden Sie in den häufig gestellten Fragen zu Preisen für [virtuelle Linux-Computer](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) und [virtuelle Windows-Computer](https://azure.microsoft.com/pricing/details/virtual-machines/windows/).

Informationen zu weiteren Kostensenkungsfeatures für Ihre Entwicklungs- und Testumgebungen finden Sie unter [Azure DevTest Labs](https://azure.microsoft.com/services/devtest-lab/).

### <a name="turn-on-and-review-azure-advisor-recommendations"></a>Aktivieren und Überprüfen von Azure Advisor-Empfehlungen

Mit [Azure Advisor](../advisor/advisor-overview.md) können Sie Kosten reduzieren, indem wenig genutzte Ressourcen identifiziert werden. Suchen Sie im Azure-Portal nach **Advisor**:

![Screenshot der Schaltfläche für Azure Advisor im Azure-Portal](./media/billing-getting-started/advisor-button.png)

Wählen Sie auf der linken Seite **Kosten** aus. Auf der Registerkarte **Kosten** werden nützliche Empfehlungen angezeigt:

![Screenshot mit einem Beispiel für eine Advisor-Kostenempfehlung](./media/billing-getting-started/advisor-action.png)

Unter [Optimieren von Kosten mithilfe von Empfehlungen](../cost-management/tutorial-acm-opt-recommendations.md) finden Sie ein geführtes Tutorial zu Advisor-Empfehlungen für Kosteneinsparungen.

## <a name="review-charges-against-your-latest-invoice"></a>Überprüfen der Gebühren anhand der letzten Rechnung

Ihre Rechnung steht am Ende des Abrechnungszeitraums zur Verfügung. Sie können [Rechnungen und detaillierte Nutzungsdateien herunterladen](billing-download-azure-invoice-daily-usage-date.md) und miteinander vergleichen, um sich zu vergewissern, dass die Abrechnung korrekt ist. Weitere Informationen für den Vergleich Ihrer täglichen Nutzung mit Ihrer Rechnung finden Sie unter [Erläuterungen zur Rechnung für Microsoft Azure](billing-understand-your-bill.md).

Wenn Sie Azure im Rahmen einer Microsoft-Kundenvereinbarung (Microsoft Customer Agreement, MCA) verwenden, können Sie auch [Ihre Rechnung mit den Transaktionen vergleichen](billing-mca-understand-your-bill.md#view-transactions-for-an-invoice-in-the-azure-portal), um die Gebühren auf Ihrer Rechnung nachzuvollziehen.

## <a name="integrate-with-billing-and-consumption-apis"></a>Integrieren von Abrechnungs- und Nutzungs-APIs

Mithilfe der Azure-APIs für die [Abrechnung](https://docs.microsoft.com/rest/api/billing/) und [Nutzung](https://docs.microsoft.com/rest/api/consumption/) können Sie Abrechnungs- und Kostendaten programmgesteuert abrufen. Verwenden Sie die RateCard-API und die Usage-API, um Ihre abgerechnete Nutzung abzurufen. Weitere Informationen finden Sie unter [Gewinnen von Einblicken in den Ressourcenverbrauch unter Microsoft Azure](billing-usage-rate-card-overview.md).

## <a name="other-offers"></a> Zusätzliche Ressourcen und Sonderfälle

### <a name="ea-csp-and-sponsorship-customers"></a>EA-, CSP- und Sponsorship-Kunden
Wenden Sie sich an Ihren Kundenbetreuer oder Azure-Partner.

| Angebot | Ressourcen |
|-------------------------------|-----------------------------------------------------------------------------------|
| Enterprise Agreement (EA) | [EA-Portal](https://ea.azure.com/), [Hilfedokumente](https://ea.azure.com/helpdocs) und [Power BI-Bericht](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-enterprise/) |
| Cloud Solution Provider (CSP) | Wenden Sie sich an Ihren Anbieter. |
| Azure Sponsorship | [Sponsorship-Portal](https://www.microsoftazuresponsorships.com/) |

Für IT-Manager großer Organisationen empfehlen wir den Artikel zum [Azure-Unternehmensgerüst](/azure/architecture/cloud-adoption-guide/subscription-governance) und das [Whitepaper zu Enterprise IT](https://download.microsoft.com/download/F/F/F/FFF60E6C-DBA1-4214-BEFD-3130C340B138/Azure_Onboarding_Guide_for_IT_Organizations_EN_US.pdf) (PDF-Download, nur auf Englisch verfügbar).

### <a name="EA"></a> Enterprise Agreement-Kostenansichten im Azure-Portal

Enterprise-Kostenansichten sind derzeit als öffentliche Vorschau verfügbar. Folgende Punkte sind zu beachten:

- Abonnementkosten basieren auf der Nutzung. Bereits gezahlte Beträge, Überschreitungen, enthaltene Mengen, Anpassungen und Steuern werden nicht berücksichtigt. Die tatsächlichen Gebühren werden auf Registrierungsebene berechnet.
- Die im Azure-Portal angezeigten Beträge weichen möglicherweise von den Angaben im Enterprise Portal ab. Nach einer Aktualisierung im Enterprise Portal dauert es unter Umständen ein paar Minuten, bis die Änderungen im Azure-Portal angezeigt werden.
- Sollten keine Kosten angezeigt werden, kann das auf eine der folgenden Ursachen zurückzuführen sein:
    - Sie verfügen über keine Berechtigungen auf Abonnementebene. Um Übersichten der Unternehmenskosten anzuzeigen, müssen Sie ein Abrechnungsleser, Leser, Mitwirkender oder Besitzer auf Abonnementebene sein.
    - Sie sind Kontobesitzer, und der Registrierungsadministrator hat die Einstellung „Gebühren anzeigen“ für Kontobesitzer deaktiviert.  Wenden Sie sich an den Registrierungsadministrator, um Zugriff auf die Kosten zu erhalten.
    - Sie sind Abteilungsadministrator, und der Registrierungsadministrator hat die Einstellung **Gebühren anzeigen** für Abteilungsadministratoren deaktiviert.  Wenden Sie sich an den Registrierungsadministrator, um Zugriff zu erhalten.
    - Sie haben Azure über einen Channelpartner gekauft, und der Partner hat die Preisinformationen nicht freigegeben.  
- Wenn Sie im Enterprise Portal Einstellungen im Zusammenhang mit dem Kostenzugriff aktualisieren, werden die Änderungen erst nach einigen Minuten im Azure-Portal angezeigt.
- Das Ausgabelimit und die Rechnungsanleitungen gelten nicht für EA-Abonnements.

### <a name="check-your-subscription-and-access"></a>Überprüfen von Abonnement und Zugriff

Um Kosten anzeigen zu können, benötigen Sie Zugriff auf Kosten- oder Abrechnungsinformationen auf der Konto- oder Abonnementebene. Der Zugriff unterscheidet sich abhängig von der Art des Abrechnungskontos. Weitere Informationen zu Abrechnungskonten und zur Überprüfung der Art Ihres Abrechnungskontos finden Sie unter [Anzeigen von Abrechnungskonten im Azure-Portal](billing-view-all-accounts.md).

Wenn Sie im Rahmen eines MOSP-Abrechnungskontos (Microsoft Online Services-Programm) auf Azure zugreifen, finden Sie weitere Informationen unter [Verwalten des Zugriffs auf Abrechnungsinformationen für Azure](billing-manage-access.md).

Wenn Sie im Rahmen eines EA-Abrechnungskontos (Enterprise Agreement, Konzernvertrag) auf Azure zugreifen, finden Sie weitere Informationen unter [Informationen zu Azure Enterprise Agreement-Administratorrollen in Azure](billing-understand-ea-roles.md).

Wenn Sie im Rahmen eines MCA-Abrechnungskontos (Microsoft Customer Agreement, Microsoft-Kundenvereinbarung) auf Azure zugreifen, finden Sie weitere Informationen unter [Grundlegendes zu Verwaltungsrollen für Microsoft-Kundenvereinbarungen in Azure](billing-understand-mca-roles.md).

### <a name="request-a-service-level-agreement-credit-for-a-service-incident"></a>Anfordern einer SLA-Gutschrift für einen Dienstvorfall

In der Vereinbarung zum Servicelevel (SLA) ist die garantierte Verfügbarkeit und Konnektivität beschrieben, die Microsoft zusichert. Ein Dienstvorfall wird gemeldet, wenn bei Azure-Diensten ein Problem auftritt, das sich auf die Betriebszeit oder Konnektivität auswirkt. Dies wird häufig als *Ausfall* bezeichnet. Wenn wir die Servicelevel für jeden Dienst nicht wie in der SLA beschrieben erreichen und einhalten, haben Sie möglicherweise Anspruch auf eine Gutschrift über einen Teil Ihrer monatlichen Dienstgebühren.

Gehen Sie wie folgt vor, um eine Gutschrift anzufordern:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an. Wenn Sie über mehrere Konten verfügen, stellen Sie sicher, dass Sie das Konto verwenden, das von der Azure-Downtime betroffen war. 
2. Erstellen Sie eine neue Supportanfrage.
3. Wählen Sie unter **Problemtyp** die Option **Abrechnung** aus.
4. Wählen Sie unter **Problemtyp** die Option **Anforderung zur Rückerstattung** aus.
5. Fügen Sie Details hinzu, um anzugeben, dass Sie um eine SLA-Gutschrift bitten, geben Sie das Datum, die Uhrzeit und die Zeitzone sowie die betroffenen Dienste (VMs, Websites usw.) an.
6. Überprüfen Sie Ihre Kontaktdetails, und wählen Sie **Erstellen** aus, um Ihre Anforderung zu senden.

Die SLA-Schwellenwerte variieren je nach Dienst. Beispielsweise gilt für den SQL-Webtarif eine SLA von 99,9 %, für VMs gilt eine SLA von 99,95 %, und für den SQL-Standardtarif gilt eine SLA von 99,99 %.

Bei einigen Diensten gibt es Voraussetzungen für die Anwendung der SLA. Bei virtuellen Computern müssen beispielsweise mindestens zwei Instanzen in derselben Verfügbarkeitsgruppe bereitgestellt sein.

Weitere Informationen finden Sie unter [Vereinbarungen zum Servicelevel (SLAs)](https://azure.microsoft.com/support/legal/sla/) und in der [DLV-Übersicht für Azure-Dienste](https://azure.microsoft.com/support/legal/sla/summary/).

## <a name="need-help-contact-us"></a>Sie brauchen Hilfe? Wenden Sie sich an uns.

Wenn Sie Fragen haben oder Hilfe benötigen, [erstellen Sie eine Supportanfrage](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Nächste Schritte
- Informieren Sie sich über die Verwendung von [Ausgabenlimits](billing-spending-limit.md), um Überschreitungen zu vermeiden.
- Beginnen Sie mit der [Analyse Ihrer Azure-Kosten](../cost-management/quick-acm-cost-analysis.md).
