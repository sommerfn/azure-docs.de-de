---
title: Vermeiden unerwarteter Kosten und Verwalten der Abrechnung in Azure
description: Hier erwarten Sie, wie Sie unerwartete Gebühren in Ihrer Azure-Abrechnung vermeiden. Verwenden Sie Kostenüberwachungs- und Kostenverwaltungsfunktionen für ein Azure-Abonnement.
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
ms.openlocfilehash: d6c287d5ead0095a4f7bb5ad754212b134f7103c
ms.sourcegitcommit: a19f4b35a0123256e76f2789cd5083921ac73daf
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2019
ms.locfileid: "71719815"
---
# <a name="prevent-unexpected-charges-with-azure-billing-and-cost-management"></a>Vermeiden unerwarteter Gebühren bei der Azure-Abrechnung und -Kostenverwaltung

Wenn Sie sich für Azure registrieren, sollten Sie sich einen Überblick über Ihre Ausgaben verschaffen:

- Der [Preisrechner](https://azure.microsoft.com/pricing/calculator/) kann eine Kostenschätzung bereitstellen, bevor Sie eine Azure-Ressource erstellen. 

- Im [Azure-Portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) erhalten Sie den aktuellen Kostenstrukturplan und die Prognose für Ihr Abonnement. 

- Wenn Sie Kosten für unterschiedliche Projekte oder Teams zusammenfassen und verstehen möchten, beschäftigen Sie sich mit [Tags für Ressourcen](../azure-resource-manager/resource-group-using-tags.md). Falls Sie das Berichtssystem Ihrer Organisation verwenden möchten, informieren Sie sich über die [Abrechnung-APIs](billing-usage-rate-card-overview.md).

- Basiert Ihr Abonnement auf Enterprise Agreement (EA), können Sie Ihre Kosten im Azure-Portal anzeigen. Einige der folgenden Features sind für Abonnements von CSP-Kunden (Cloud Solution Provider, Cloud-Lösungsanbieter) oder Azure Sponsorship-Kunden eventuell nicht relevant. Weitere Informationen finden Sie unter [Zusätzliche Ressourcen für EA, CSP und Sponsorship](#other-offers).

- Wenn Sie ein Abonnement für eine kostenlose Testversion, [Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), Azure in Open (AIO) oder BizSpark besitzen, wird Ihr Abonnement automatisch deaktiviert, sobald Ihr gesamtes Guthaben verbraucht ist. Machen Sie sich mit [Ausgabenlimits](#spending-limit) vertraut, um zu vermeiden, dass Ihr Abonnement überraschend deaktiviert wird.

- Wenn Sie sich für ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) angemeldet haben, [können Sie einige der beliebtesten Azure-Dienste kostenlos für 12 Monate nutzen](billing-create-free-services-included-free-account.md). Weitere Informationen finden Sie zusammen mit den unten aufgelisteten Empfehlungen unter [Vermeiden Sie, dass Ihnen für Ihr kostenloses Azure-Konto Gebühren angezeigt werden](billing-avoid-charges-free-account.md).

## <a name="get-estimated-costs-before-adding-azure-services"></a>Abrufen der geschätzten Kosten vor dem Hinzufügen von Azure-Diensten

Hier sind einige zusätzliche Informationen zur Kostenschätzung mit den folgenden Tools:
- Azure-Preisrechner
- Azure-Portal
- Ausgabenlimit

Die Abbildungen in den folgenden Abschnitten zeigen Beispielpreise in US-Dollar.

### <a name="estimate-cost-online-using-the-pricing-calculator"></a>Schätzen der Kosten mithilfe des Onlinepreisrechners

Verschaffen Sie sich mit dem [Preisrechner](https://azure.microsoft.com/pricing/calculator/) einen ungefähren Eindruck von den monatlichen Kosten des Diensts, für den Sie sich interessieren. Sie können eine beliebige Erstanbieter-Azure-Ressource hinzufügen, um eine Kostenschätzung zu erhalten. Im Preisrechner können Sie den Währungstyp ändern.

![Screenshot des Preisrechnermenüs](./media/billing-getting-started/pricing-calc.png)

So fällt im Preisrechner beispielsweise für die Computestunden eines virtuellen A1-Windows-Computers voraussichtlich ein bestimmter Betrag pro Monat an, wenn der Computer durchgehend in Betrieb ist:

![Screenshot des Preisrechners, der die geschätzten Kosten pro Monat für einen virtuellen A1-Windows-Computer anzeigt](./media/billing-getting-started/pricing-calcvm.png)

Weitere Informationen zu den Preisen finden Sie unter [Häufig gestellte Fragen zu Azure-Preisen](https://azure.microsoft.com/pricing/faq/). Wenn Sie mit einer für den Azure-Vertrieb zuständigen Person sprechen möchten, rufen Sie die Telefonnummer oben auf der FAQ-Seite an.

### <a name="review-estimated-costs-in-the-azure-portal"></a>Überprüfen von geschätzten Kosten im Azure-Portal

Wenn Sie im Azure-Portal einen Dienst hinzufügen, gibt es eine Ansicht, die Ihnen die geschätzten Kosten pro Monat in Ihrer Rechnungswährung anzeigt. Wenn Sie also etwa die Größe Ihres virtuellen Windows-Computers auswählen, werden die voraussichtlichen monatlichen Kosten für die Computestunden angezeigt:

![Beispiel: ein virtueller A1-Windows-Computer mit den geschätzten Kosten pro Monat](./media/billing-getting-started/vm-size-cost.png)

### <a name="spending-limit"></a>Überprüfen, ob ein Ausgabenlimit aktiviert ist

Bei Verwendung eines guthabenbasierten Abonnements wird das Ausgabenlimit standardmäßig für Sie aktiviert. Dadurch wird Ihre Kreditkarte nicht belastet, nachdem Sie Ihr gesamtes Guthaben verbraucht haben. Eine vollständige Liste mit Azure-Angeboten sowie Informationen zur Verfügbarkeit des Ausgabenlimits finden Sie [hier](https://azure.microsoft.com/support/legal/offer-details/).

Bei Erreichen Ihres Ausgabenlimits werden Ihre Dienste allerdings deaktiviert. Das bedeutet, dass die Zuordnung Ihrer virtuellen Computer aufgehoben wird. Um Ausfallzeiten zu vermeiden, müssen Sie das Ausgabenlimit deaktivieren. Überschreitungen werden über die hinterlegte Kreditkarte abgerechnet.

Um festzustellen, ob ein Ausgabenlimit aktiviert ist, [navigieren Sie im Kontocenter zur Ansicht „Abonnements“](https://account.windowsazure.com/Subscriptions). Bei aktiviertem Ausgabenlimit wird ein Banner wie der folgende angezeigt:

![Screenshot mit einer Warnung aufgrund eines aktivierten Ausgabenlimits im Kontocenter](./media/billing-getting-started/spending-limit-banner.png)

Klicken Sie auf das Banner, und folgen Sie den Anweisungen, um das Ausgabenlimit zu entfernen. Falls Sie bei der Registrierung keine Kreditkarteninformationen eingegeben haben, müssen Sie dies nun nachholen, um das Ausgabenlimit entfernen zu können. Weitere Informationen finden Sie unter [Azure-Ausgabenlimit – Funktionsweise und Aktivierung/Deaktivierung](https://azure.microsoft.com/pricing/spending-limits/).

## <a name="use-budgets-and-cost-alerts"></a>Verwenden von Budgets und Kostenwarnungen

Sie können [Budgets](../cost-management/tutorial-acm-create-budgets.md) erstellen, um Kosten zu verwalten, und [Warnungen](../cost-management/cost-mgt-alerts-monitor-usage-spending.md) erstellen, die die Beteiligten automatisch über Ausgabenanomalien und Überschreitungsrisiken informieren. Warnungen basieren auf Ausgaben im Vergleich zum Budget und zu Kostenschwellenwerten.

## <a name="monitor-costs-when-using-azure-services"></a>Überwachen von Kosten bei Verwendung von Azure-Diensten
Mit den folgenden Tools können Sie Kosten überwachen:

- `Tags`
- Kostenaufschlüsselung und Verbrauchsrate
- Kostenanalyse

### <a name="tags"></a> Gruppieren von Abrechnungsdaten durch Hinzufügen von Tags zu Ressourcen

Mithilfe von Tags können Sie Abrechnungsdaten für unterstützte Dienste gruppieren. Wenn Sie also beispielsweise mehrere virtuelle Computer für unterschiedliche Teams verwenden, können Sie die Kosten mithilfe von Tags nach Kostenstelle (z. B. Personal, Marketing, Finanzen) oder Umgebung (Produktion, Präproduktion, Test) kategorisieren.

![Screenshot zur Einrichtung von Tags im Portal](./media/billing-getting-started/tags.png)

Die Tags werden in den unterschiedlichen Kostenberichtsansichten angezeigt. So sind sie beispielsweise sofort in der [Kostenanalyseansicht](#costs) und nach dem ersten Abrechnungszeitraum in der CSV-Datei mit den Nutzungsdetails zu sehen.

Weitere Informationen finden Sie unter [Verwenden von Tags zum Organisieren von Azure-Ressourcen](../azure-resource-manager/resource-group-using-tags.md).

### <a name="costs"></a> Überwachen der Kostenaufschlüsselung und Verbrauchsrate

Sobald Ihre Azure-Dienste ausgeführt werden, überprüfen Sie regelmäßig die Gebühren. Die aktuellen Ausgaben sowie die Verbrauchsrate werden im Azure-Portal angezeigt.

1. Öffnen Sie [„Abonnements“ im Azure-Portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade), und wählen Sie ein Abonnement aus.

2. Dort werden die Kostenaufschlüsselung und die Verbrauchsrate angezeigt, sofern dies für Ihr Abonnement unterstützt wird.

    ![Screenshot der Verbrauchsrate und der Kostenaufschlüsselung im Azure-Portal](./media/billing-getting-started/burn-rate.PNG)

3. Klicken Sie in der Liste auf der linken Seite auf [Kostenanalyse](../cost-management/quick-acm-cost-analysis.md), um die Kostenaufschlüsselung nach Ressource anzuzeigen. Warten Sie nach dem Hinzufügen eines Diensts 24 Stunden, bis die Daten angezeigt werden.

    ![Screenshot der Kostenanalyseansicht im Azure-Portal](./media/billing-getting-started/cost-analysis.png)

4. Sie können nach verschiedenen Eigenschaften wie [Tags](#tags), Ressourcentyp, Ressourcengruppe und Zeitraum filtern. Klicken Sie auf **Übernehmen**, um die Filter zu bestätigen, und auf **Herunterladen**, um die Ansicht in eine CSV-Datei zu exportieren.

5. Darüber hinaus können Sie auf eine Ressource klicken, um Ihren täglichen Ausgabenverlauf und die täglichen Kosten der Ressource anzuzeigen.

    ![Screenshot der Ausgabenverlaufsansicht im Azure-Portal](./media/billing-getting-started/costhistory.png)

Vergleichen Sie die angezeigten Kosten mit den Schätzungen, die beim Auswählen der Dienste angezeigt wurden. Sollten die Kosten stark von den Schätzungen abweichen, überprüfen Sie den Tarif, den Sie für Ihre Ressourcen ausgewählt haben.

## <a name="optimize-and-reduce-costs"></a>Optimieren und Kosten senken
Wenn Sie mit den Prinzipien der Kostenverwaltung nicht vertraut sind, lesen Sie [Optimieren der Cloudinvestitionen mit Azure Cost Management](../cost-management/cost-mgt-best-practices.md).

Im Azure-Portal können Sie durch automatisches Herunterfahren von virtuellen Computern und anhand von Advisor-Empfehlungen auch Azure-Kosten optimieren und reduzieren.

### <a name="consider-cost-cutting-features-like-auto-shutdown-for-vms"></a>Nutzung von Kostensenkungsfeatures wie automatisches Herunterfahren für virtuelle Computer

Abhängig von Ihrem Szenario können Sie im Azure-Portal ggf. das automatische Herunterfahren für Ihre virtuellen Computer konfigurieren. Weitere Informationen finden Sie unter [Announcing auto-shutdown for VMs using Azure Resource Manager](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/) (Ankündigung des automatischen Herunterfahrens für virtuelle Computer mit Azure Resource Manager).

![Screenshot der Option für automatisches Herunterfahren im Portal](./media/billing-getting-started/auto-shutdown.png)

Das automatische Herunterfahren ist nicht dasselbe wie das Herunterfahren des virtuellen Computers über die Energieoptionen. Beim automatischen Herunterfahren werden Ihre virtuellen Computer beendet, und ihre Zuordnung wird aufgehoben, um zu verhindern, dass weitere Nutzungsgebühren anfallen. Weitere Informationen finden Sie in den häufig gestellten Fragen zu Preisen für [virtuelle Linux-Computer](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) und [virtuelle Windows-Computer](https://azure.microsoft.com/pricing/details/virtual-machines/windows/).

Informationen zu weiteren Kostensenkungsfeatures für Ihre Entwicklungs- und Testumgebungen finden Sie unter [Azure DevTest Labs](https://azure.microsoft.com/services/devtest-lab/).

### <a name="turn-on-and-review-azure-advisor-recommendations"></a>Aktivieren und Überprüfen von Azure Advisor-Empfehlungen

Mit [Azure Advisor](../advisor/advisor-overview.md) können Sie Kosten reduzieren, indem wenig genutzte Ressourcen identifiziert werden. Navigieren Sie im Azure-Portal zum Advisor:

![Screenshot der Schaltfläche für Azure Advisor im Azure-Portal](./media/billing-getting-started/advisor-button.png)

Auf der Registerkarte **Kosten** des Advisor-Dashboards stehen nützliche Empfehlungen zur Verfügung:

![Screenshot mit einem Beispiel für eine Advisor-Kostenempfehlung](./media/billing-getting-started/advisor-action.png)

Unter [Optimieren von Kosten mithilfe von Empfehlungen](../cost-management/tutorial-acm-opt-recommendations.md) finden Sie ein geführtes Tutorial zu Advisor-Empfehlungen für Kosteneinsparungen.

## <a name="review-costs-against-your-latest-invoice"></a>Überprüfen der Kosten anhand der letzten Rechnung

Am Ende des Abrechnungszeitraums ist ihre neueste Rechnung verfügbar. Sie können auch [Rechnungen und detaillierte Verwendungsdateien herunterladen](billing-download-azure-invoice-daily-usage-date.md), um sicherzustellen, dass Sie richtige Rechnungen erhalten haben. Weitere Informationen für den Vergleich Ihrer täglichen Nutzung mit Ihrer Rechnung finden Sie unter [Erläuterungen zur Rechnung für Microsoft Azure](billing-understand-your-bill.md).

### <a name="billing-api"></a>Abrechnungs-API

Über die Azure-Abrechnungs-API können Sie Nutzungsdaten programmgesteuert abrufen. Verwenden Sie die RateCard-API und die Usage-API, um Ihre abgerechnete Nutzung abzurufen. Weitere Informationen finden Sie unter [Gewinnen von Einblicken in den Ressourcenverbrauch unter Microsoft Azure](billing-usage-rate-card-overview.md).

## <a name="other-offers"></a> Zusätzliche Ressourcen und Sonderfälle

### <a name="ea-csp-and-sponsorship-customers"></a>EA-, CSP- und Sponsorship-Kunden
Wenden Sie sich an Ihren Kundenbetreuer oder Azure-Partner.

| Angebot | Ressourcen |
|-------------------------------|-----------------------------------------------------------------------------------|
| Enterprise Agreement (EA) | [EA-Portal](https://ea.azure.com/), [Hilfedokumente](https://ea.azure.com/helpdocs) und [Power BI-Bericht](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-enterprise/) |
| Cloud Solution Provider (CSP) | Wenden Sie sich an Ihren Anbieter. |
| Azure Sponsorship | [Sponsorship-Portal](https://www.microsoftazuresponsorships.com/) |

Für IT-Manager großer Organisationen empfehlen wir den Artikel zum [Azure-Unternehmensgerüst](/azure/architecture/cloud-adoption-guide/subscription-governance) und das [Whitepaper zu Enterprise IT](https://download.microsoft.com/download/F/F/F/FFF60E6C-DBA1-4214-BEFD-3130C340B138/Azure_Onboarding_Guide_for_IT_Organizations_EN_US.pdf) (PDF-Download, nur auf Englisch verfügbar).

#### <a name="EA"></a> Enterprise Agreement-Kostenansichten im Azure-Portal

Enterprise-Kostenansichten sind derzeit als öffentliche Vorschau verfügbar. Folgende Punkte sind zu beachten:

- Abonnementkosten basieren auf der Nutzung. Bereits gezahlte Beträge, Überschreitungen, enthaltene Mengen, Anpassungen und Steuern werden nicht berücksichtigt. Die tatsächlichen Gebühren werden auf Registrierungsebene berechnet.
- Die im Azure-Portal angezeigten Beträge weichen möglicherweise von den Angaben im Enterprise Portal ab. Nach einer Aktualisierung im Enterprise Portal dauert es unter Umständen ein paar Minuten, bis die Änderungen im Azure-Portal angezeigt werden.
- Sollten keine Kosten angezeigt werden, kann das auf eine der folgenden Ursachen zurückzuführen sein:
    - Sie verfügen über keine Berechtigungen auf Abonnementebene. Um Übersichten der Unternehmenskosten anzuzeigen, müssen Sie ein Abrechnungsleser, Leser, Mitwirkender oder Besitzer auf Abonnementebene sein.
    - Sie sind Kontobesitzer, und der Registrierungsadministrator hat die Einstellung „Gebühren anzeigen“ für Kontobesitzer deaktiviert.  Wenden Sie sich an den Registrierungsadministrator, um Zugriff auf die Kosten zu erhalten.
    - Sie sind Abteilungsadministrator, und der Registrierungsadministrator hat die Einstellung **Gebühren anzeigen** für Abteilungsadministratoren deaktiviert.  Wenden Sie sich an den Registrierungsadministrator, um Zugriff zu erhalten.
    - Sie haben Azure über einen Channelpartner gekauft, und der Partner hat die Preisinformationen nicht freigegeben.  
- Wenn Sie Einstellungen im Zusammenhang mit dem Kostenzugriff im Enterprise Portal aktualisieren, werden die Änderungen erst nach einigen Minuten im Azure-Portal angezeigt.
- Das Ausgabelimit und die Rechnungsanleitungen gelten nicht für EA-Abonnements.

### <a name="check-your-subscription-and-access"></a>Überprüfen von Abonnement und Zugriff

Zum Anzeigen von Kosten müssen Sie über [Zugriff auf Abrechnungsinformationen auf Abonnementebene](billing-manage-access.md) verfügen. Nur der Kontoadministrator kann auf das [Kontocenter](https://account.azure.com/Subscriptions) zugreifen, Abrechnungsinformationen ändern und Abonnements verwalten. Der Kontoadministrator ist die Person, die den Registrierungsprozess durchlaufen hat. Weitere Informationen finden Sie unter [Hinzufügen oder Ändern von Azure-Administratorrollen, die das Abonnement oder die Dienste verwalten](billing-add-change-azure-subscription-administrator.md).

Um zu ermitteln, ob Sie der Kontoadministrator sind, navigieren Sie [im Azure-Portal zu „Abonnements“](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade). Zeigen Sie die Liste der Abonnements an, und suchen Sie **Meine Rolle**. Wenn die Rolle *Kontoadministrator* ist, verfügen Sie über vollständige Berechtigungen. Bei einer Angabe wie beispielsweise *Besitzer* verfügen Sie nicht über sämtliche Berechtigungen.

![Screenshot Ihrer Rolle in der Ansicht „Abonnements“ im Azure-Portal](./media/billing-getting-started/sub-blade-view.PNG)

Um Abonnements verwalten und Abrechnungsinformationen ändern zu können, müssen Sie [den Kontoadministrator ermitteln](billing-subscription-transfer.md#whoisaa). Bitten Sie den Kontoadministrator, die Aufgaben für Sie auszuführen oder [das Abonnement auf Sie zu übertragen](billing-subscription-transfer.md).

Falls der Kontoadministrator Ihre Organisation verlassen hat und Sie die Abrechnung verwalten müssen, [wenden Sie sich an uns](https://go.microsoft.com/fwlink/?linkid=2083458).


### <a name="request-a-service-level-agreement-credit-for-a-service-incident"></a>Anfordern einer SLA-Gutschrift für einen Dienstvorfall

In der Vereinbarung zum Servicelevel (SLA) ist die garantierte Verfügbarkeit und Konnektivität beschrieben, die Microsoft zusichert. Ein Dienstvorfall wird gemeldet, wenn bei Azure-Diensten ein Problem auftritt, das sich auf die Betriebszeit oder Konnektivität auswirkt. Dies wird häufig als *Ausfall* bezeichnet. Wenn wir die Servicelevel für jeden Dienst nicht wie in der SLA beschrieben erreichen und einhalten, haben Sie möglicherweise Anspruch auf eine Gutschrift über einen Teil Ihrer monatlichen Dienstgebühren.

Gehen Sie wie folgt vor, um eine Gutschrift anzufordern:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an. Wenn Sie über mehrere Konten verfügen, stellen Sie sicher, dass Sie das Konto verwenden, das von der Azure-Downtime betroffen war. 
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
