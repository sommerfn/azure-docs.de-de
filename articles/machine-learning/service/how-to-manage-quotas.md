---
title: Verwalten von Ressourcen und Kontingenten
titleSuffix: Azure Machine Learning
description: Erfahren Sie mehr über Kontingente für Azure Machine Learning-Ressourcen sowie über das Anfordern von zusätzlichem Kontingent.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
author: nishankgu
ms.author: nigup
ms.date: 11/04/2019
ms.openlocfilehash: 2206afc45c7ea35c41f51839b66dca33982939ae
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73489800"
---
# <a name="manage-and-request-quotas-for-azure-resources"></a>Verwalten und Anfordern von Kontingenten für Azure-Ressourcen
[!INCLUDE [applies-to-skus](../../../includes/aml-applies-to-basic-enterprise-sku.md)]

In diesem Artikel erfahren Sie mehr über vorkonfigurierte Grenzwerte für Azure-Ressourcen in Ihrem Abonnement. Außerdem finden Sie Anweisungen dazu, wie Sie für jeden Ressourcentyp Kontingenterhöhungen anfordern. Diese Einschränkungen werden eingeführt, um Budgetüberschreitungen aufgrund von Betrug zu vermeiden und Kapazitätseinschränkungen bei Azure Rechnung zu tragen.

Genau wie bei anderen Azure-Diensten gelten auch für bestimmte Azure Machine Learning-Ressourcen gewisse Grenzwerte. Diese Grenzwerte reichen von Obergrenzen für die Anzahl von Arbeitsbereichen bis hin zu Grenzwerten für die tatsächlich zugrunde liegenden Computeressourcen, die für das Modelltraining oder Rückschluss/Bewertung verwendet werden. 

Berücksichtigen Sie diese Grenzwerte beim Entwerfen und Skalieren Ihrer Azure Machine Learning-Ressourcen für Produktionsworkloads. Wenn Ihr Cluster beispielsweise nicht die Zielanzahl von Knoten erreicht, haben Sie möglicherweise den Grenzwert für das Kernkontingent von Azure Machine Learning Compute für Ihr Abonnement erreicht. Wenn Sie ein Limit oder ein Kontingent über den Standardgrenzwert anheben möchten, können Sie eine gebührenfreie Onlinekundensupport-Anforderung öffnen. Die Limits können aufgrund von Azure-Kapazitätseinschränkungen nicht über den Wert unter „Maximales Limit“ in den folgenden Tabellen angehoben werden. Falls die Spalte „Maximales Limit“ nicht vorhanden ist, gelten für die entsprechende Ressource keine anpassbaren Limits.

## <a name="special-considerations"></a>Besondere Überlegungen

+ Bei einem Kontingent handelt es sich um ein Kreditlimit und keine Kapazitätsgarantie. Wenn Sie einen umfangreichen Kapazitätsbedarf haben, wenden Sie sich an den Azure-Support.

+ Ihr Kontingent wird auf alle Dienste in Ihren Abonnements verteilt, einschließlich Azure Machine Learning. Die einzige Ausnahme ist Azure Machine Learning Compute, das über ein vom Kerncomputekontingent separates Kontingent verfügt. Berechnen Sie den Kontingentbedarf für alle Dienste, wenn Sie Ihre Kapazitätsanforderungen auswerten.

+ Die Standardgrenzwerte variieren je nach angebotenem Kategorietyp, z. B. kostenlose Testversion, nutzungsbasierte Bezahlung und VM-Serien wie Dv2, F und G usw.

## <a name="default-resource-quotas"></a>Standardressourcenkontingente

Im Folgenden finden Sie eine Aufschlüsselung der Kontingentgrenzen nach verschiedenen Ressourcentypen in Ihrem Azure-Abonnement.

> [!Important]
> Änderungen bei Limits bleiben vorbehalten. Die aktuellen Limits finden Sie immer im [Dokument](https://docs.microsoft.com/azure/azure-subscription-service-limits/) zu Kontingenten auf Dienstebene für die meisten Azure-Dienste.

### <a name="virtual-machines"></a>Virtuelle Computer
Für jedes Azure-Abonnement gilt ein Grenzwert für die Anzahl virtueller Computer, die Sie für Ihre Dienste oder eigenständig bereitstellen können. Dieses Limit gilt auf Regionsebene sowohl für die Gesamtanzahl an Kernen als auch pro Familie.

Bei Kernen virtueller Computer gilt ein regionaler Gesamtgrenzwert und ein regionaler Grenzwert pro Größe einer Serie (Dv2, F usw.), die beide getrennt erzwungen werden. Angenommen, Sie verwenden ein Abonnement mit einem Kerngesamtgrenzwert von 30 für VMs in der Region „USA, Osten“, einem Kerngrenzwert von 30 für die A-Serie und einem Kerngrenzwert von 30 für die D-Serie. Für dieses Abonnement dürfen dann 30 virtuelle A1-Computer bzw. 30 virtuelle D1-Computer bereitgestellt werden, oder eine Kombination daraus, bei der die Gesamtzahl von 30 Kernen nicht überschritten wird (z.B. 10 virtuelle A1-Computer und 20 virtuelle D1-Computer).

[!INCLUDE [azure-subscription-limits-azure-resource-manager](../../../includes/azure-subscription-limits-azure-resource-manager.md)]

Eine detailliertere und aktuelle Liste der Kontingentgrenzen finden Sie in [diesem Artikel](https://docs.microsoft.com/azure/azure-subscription-service-limits) zu Azure-weiten Kontingenten.

### <a name="azure-machine-learning-compute"></a>Azure Machine Learning Compute
In Azure Machine Learning Compute gibt es eine Standardkontingentgrenze für die Anzahl von Kernen und eindeutigen Computeressourcen, die pro Region in einem Abonnement zulässig sind. Dieses Kontingent ist getrennt von dem oben genannten Kontingent für VM-Kerne. Die Kerngrenzen werden nicht von den beiden Ressourcentypen geteilt, da es sich bei AmlCompute um einen verwalteten Dienst handelt, der Ressourcen in einem Modell des Typs „Gehostet im Auftrag von“ bereitstellt.

Verfügbare Ressourcen:
+ Für dedizierte Kerne pro Region gilt ein Standardgrenzwert von 24 bis 300, je nach Typ Ihres Abonnementangebots, wobei für die Angebotstypen EA und CSP höhere Standardwerte gelten.  Die Anzahl dedizierter Kerne pro Abonnement kann erhöht werden und ist je nach VM-Familie unterschiedlich. Bestimmte spezialisierte VM-Familien wie die Serien NCv2, NCv3 oder ND beginnen mit einem Standardwert von null Kernen. Wenden Sie sich an den Azure-Support, indem Sie eine Kontingentanfrage stellen und Optionen zur Erhöhung erörtern.

+ Für Kerne niedriger Priorität pro Region gilt ein Standardgrenzwert von 100 bis 3000, je nach Typ Ihres Abonnementangebots, wobei für die Angebotstypen EA und CSP höhere Standardwerte gelten. Die Anzahl der Kerne niedriger Priorität pro Abonnement kann erhöht werden. Es handelt sich bei allen VM-Familien um einen einzelnen Wert. Wenden Sie sich an den Azure-Support, um Fragen der Erhöhung zu erörtern.

+ Für Cluster pro Region gilt der Standardgrenzwert 200. Diese teilen sich ein Trainingscluster und eine Compute-Instanz (die für Kontingentzwecke als Einzelknotencluster gilt). Wenden Sie sich an den Azure-Support, wenn Sie eine Heraufsetzung dieses Grenzwerts wünschen.

+ Es gibt andere **strikte Grenzwerte, die nicht überschritten werden dürfen, sobald sie erreicht werden.

| **Ressource** | **Maximales Limit** |
| --- | --- |
| Maximale Arbeitsbereiche pro Ressourcengruppe | 800 |
| Maximale Anzahl von Knoten in einer einzelnen Ressource von Azure Machine Learning Compute (AmlCompute) | 100 Knoten |
| Maximale GPU MPI-Prozesse pro Knoten | 1–4 |
| Maximale GPU-Worker pro Knoten | 1–4 |
| Maximale Lebensdauer von Aufträgen | 90 Tage<sup>1</sup> |
| Maximale Lebensdauer von Aufträgen auf einem Knoten niedriger Priorität | 7 Tage<sup>2</sup> |
| Maximale Parameterserver pro Knoten | 1 |

<sup>1</sup> Die maximale Lebensdauer bezieht sich auf den Zeitraum, in dem ein Auftrag gestartet und beendet wird. Abgeschlossene Ausführungen bleiben unbegrenzt lange erhalten. Daten für Ausführungen, die nicht innerhalb der maximalen Lebensdauer abgeschlossen wurden, stehen hingegen nicht zur Verfügung.
<sup>2</sup> Aufträge auf einem Knoten niedriger Priorität können jederzeit vorzeitig beendet werden, wenn eine Kapazitätseinschränkung auftritt. Es empfiehlt sich, ein Auswerten von Prüfpunkten in Ihrem Auftrag zu implementieren.

### <a name="azure-machine-learning-pipelines"></a>Azure Machine Learning-Pipelines
Für Azure Machine Learning-Pipelines gibt es eine Kontingentbeschränkung für die Anzahl der Schritte in einer Pipeline und für die Anzahl der auf einem Zeitplan basierenden Ausführungen veröffentlichter Pipelines pro Region in einem Abonnement.
- Die maximal zulässige Anzahl von Schritten in einer Pipeline beträgt 30.000.
- Die maximale Anzahl der Summe aus zeitplanbasierten Ausführungen und Blobpulls für durch Blogs ausgelöste Zeitpläne veröffentlichter Pipelines pro Abonnement und Monat beträgt 100.000.

> [!NOTE]
> Wenn Sie diesen Grenzwert erhöhen möchten, wenden Sie sich an [Microsoft-Support](https://azure.microsoft.com/support/options/).

### <a name="container-instances"></a>Containerinstanzen

Es gibt auch ein Limit für die Anzahl der Containerinstanzen, die Sie in einem bestimmten Zeitraum (bereichsbezogen pro Stunde) oder für Ihr gesamtes Abonnement einrichten können.

[!INCLUDE [container-instances-limits](../../../includes/container-instances-limits.md)]

Eine detailliertere und aktuelle Liste der Kontingentgrenzen finden Sie in [diesem Artikel](https://docs.microsoft.com/azure/azure-subscription-service-limits#container-instances-limits) zu Azure-weiten Kontingenten.

### <a name="storage"></a>Storage
Es gibt eine Beschränkung für die Anzahl von Speicherkonten pro Region sowie in einem bestimmten Abonnement. Das Standardlimit beträgt 200 und umfasst sowohl Storage Standard- als auch Storage Premium-Konten. Wenn Sie mehr als 200 Speicherkonten in einer bestimmten Region benötigen, senden Sie eine Anfrage an den [Azure-Support](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest/). Das Azure Storage-Team überprüft Ihr Geschäftsszenario und kann bis zu 250 Speicherkonten für eine Region genehmigen.


## <a name="workspace-level-quota"></a>Kontingent auf Arbeitsbereichsebene

Um die Ressourcenzuordnungen für Amlcompute zwischen verschiedenen Arbeitsbereichen besser zu verwalten, haben wir eine Funktion eingeführt, mit der Sie Kontingente auf Abonnementebene (nach VM-Familie) verteilen und auf Arbeitsbereichsebene konfigurieren können. Das Standardverhalten ist, dass für alle Arbeitsbereiche das gleiche Kontingent wie für das Kontingent auf Abonnementebene für jede VM-Familie gilt. Wenn jedoch die Anzahl der Arbeitsbereiche zunimmt und Workloads unterschiedlicher Priorität beginnen, sich dieselben Ressourcen zu teilen, wünschen sich die Benutzer einen Weg, die Kapazität besser gemeinsam zu nutzen und Probleme aufgrund von Ressourcenkonflikten zu vermeiden. Azure Machine Learning bietet mit seinem verwalteten Compute-Angebot eine Lösung, indem Benutzern ermöglicht wird, ein maximales Kontingent für eine bestimmte VM-Familie in jedem Arbeitsbereich festzulegen. Dies entspricht der Verteilung Ihrer Kapazität auf die einzelnen Arbeitsbereiche, und die Benutzer können auch eine übermäßige Zuordnung vornehmen, um die maximale Auslastung zu erhöhen. 

Um Kontingente auf Arbeitsbereichsebene festzulegen, wechseln Sie zu einem beliebigen Arbeitsbereich in Ihrem Abonnement und klicken im linken Bereich auf **Nutzung + Kontingente**. Klicken Sie dann auf die Registerkarte **Kontingente konfigurieren**, um die Kontingente anzuzeigen. Erweitern Sie eine beliebige VM-Familie, und legen Sie einen Kontingentgrenzwert für jeden Arbeitsbereich fest, der unter dieser VM-Familie aufgeführt ist. Beachten Sie, dass Sie keinen negativen Wert oder einen Wert höher als das Kontingent auf Abonnementebene festlegen können. Außerdem wird, wie Sie feststellen können, standardmäßig allen Arbeitsbereichen das gesamte Abonnementkontingent zugewiesen, um die volle Nutzung des zugeordneten Kontingents zu ermöglichen.

[![Azure Machine Learning-Kontingent auf Arbeitsbereichsebene](./media/how-to-manage-quotas/azure-machine-learning-workspace-quota.png)](./media/how-to-manage-quotas/azure-machine-learning-workspace-quota.png)


> [!NOTE]
> Dies ist ein ausschließliches Feature der Enterprise Edition. Wenn Sie in Ihrem Abonnement einen Arbeitsbereich mit Basic Edition und einen mit Enterprise Edition haben, können Sie dieses Feature nur zum Festlegen von Kontingenten für Ihre Enterprise-Arbeitsbereiche nutzen. Für Ihre Basic-Arbeitsbereiche gilt weiterhin das Kontingent auf Abonnementebene, was das Standardverhalten ist.
>
> Sie benötigen Berechtigungen auf Abonnementebene, um das Kontingent auf Arbeitsbereichsebene festzulegen. Dieses wird erzwungen, damit einzelne Arbeitsbereichsbesitzer ihre Kontingente nicht bearbeiten oder erhöhen und anfangen, auf die für einen anderen Arbeitsbereich reservierten Ressourcen überzugreifen. Daher empfiehlt sich ein Abonnementadministrator, um diese Kontingente zuzuordnen und auf die einzelnen Arbeitsbereiche zu verteilen.



## <a name="view-your-usage-and-quotas"></a>Anzeigen von Nutzungsdaten und Kontingenten

Über das Azure-Portal können Sie sich ganz einfach Ihre Kontingente für verschiedene Ressourcen wie virtuelle Computer, Speicher und Netzwerke anzeigen lassen.

1. Wählen Sie im linken Bereich **Alle Dienste** und dann unter der Kategorie „Allgemein“ **Abonnements** aus.

1. Wählen Sie in der Liste der Abonnements das Abonnement aus, dessen Kontingent Sie suchen.

   **Es gibt einen Nachteil**, insbesondere für die Anzeige des Azure Machine Learning Compute-Kontingents. Wie bereits erwähnt, gilt das Kontingent unabhängig von dem Computekontingent für Ihr Abonnement.

1. Wählen Sie im linken Bereich die Option **Machine Learning-Dienst** aus, und wählen Sie dann einen beliebigen Arbeitsbereich in der angezeigten Liste aus.

1. Wählen Sie auf dem nächsten Blatt unter dem Abschnitt **Support + Problembehandlung** die Option **Nutzung + Kontingente** aus, um Ihre aktuellen Kontingentgrenzen und die Nutzung anzuzeigen.

1. Wählen Sie ein Abonnement aus, um die Kontingentgrenzen anzuzeigen. Denken Sie daran, nach der gewünschten Region zu filtern.

1. Sie können nun zwischen einer Ansicht auf Abonnementebene und einer Ansicht auf Arbeitsbereichsebene umschalten:
    + **Abonnementansicht:** In dieser Ansicht können Sie die Nutzung von Kernkontingenten nach VM-Familie anzeigen, sie nach Arbeitsbereich erweitern und sie nach den tatsächlichen Clusternamen weiter ausdehnen. Diese Ansicht ist optimal, um schnell zu den Details der Kernnutzung durch eine bestimmte VM-Familie zu gelangen, um die Aufteilung nach Arbeitsbereichen und weiter nach den zugrunde liegenden Clustern für jeden dieser Arbeitsbereiche einzusehen. Die allgemeine Konvention in dieser Ansicht ist (Nutzung/Kontingent), wobei die Nutzung die aktuelle Anzahl der horizontal hochskalierten Kerne ist und das Kontingent die logische maximale Anzahl von Kernen ist, auf die die Ressource skaliert werden kann. Für jeden **Arbeitsbereich** entspricht das Kontingent dem Kontingent auf Arbeitsbereichsebene (wie oben erläutert), das die maximale Anzahl von Kernen angibt, auf die eine bestimmte VM-Familie skaliert werden kann. Bei einem **Cluster** gilt, dass das Kontingent tatsächlich den Kernen entspricht, die der maximalen Anzahl von Knoten entsprechen, auf die der Cluster skaliert werden kann, was von der Eigenschaft max_nodes bestimmt wird.

    + **Arbeitsbereichsansicht:** In dieser Ansicht können Sie die Nutzung von Kernkontingenten nach Arbeitsbereich anzeigen, sie nach VM-Familie erweitern und sie nach den tatsächlichen Clusternamen weiter ausdehnen. Diese Ansicht ist optimal, um schnell zu den Details der Kernnutzung durch einen bestimmten Arbeitsbereich zu gelangen, um die Aufteilung nach VM-Familien und weiter nach den zugrunde liegenden Clustern für jede dieser Familien einzusehen.

## <a name="request-quota-increases"></a>Anfordern der Kontingenterhöhung

Wenn Sie ein Limit oder ein Kontingent über den Standardgrenzwert anheben möchten, [können Sie eine gebührenfreie Onlinekundensupport-Anforderung öffnen](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest/).

Die Limits können nicht über den Wert unter „Maximales Limit“ in den Tabellen angehoben werden. Falls kein maximales Limit angegeben ist, gelten für die entsprechende Ressource keine anpassbaren Limits. In [diesem Artikel](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-quota-errors) wird der Vorgang zur Kontingenterhöhung im Detail beschrieben.

Wenn Sie eine Kontingenterhöhung anfordern, müssen Sie den Dienst auswählen, für den Sie die Kontingenterhöhung anfordern (z. B. Machine Learning Service-Kontingent, Containerinstanzen oder Speicherkontingente). Zusätzlich können Sie für Azure Machine Learning Compute auf die Schaltfläche **Kontingent anfordern** klicken, während Sie das Kontingent entsprechend den obigen Schritten anzeigen.

> [!NOTE]
> Bei [Abonnements mit einer kostenlosen Testversion](https://azure.microsoft.com/offers/ms-azr-0044p) sind Einschränkungs- oder Kontingenterhöhungen nicht möglich. Wenn Sie über ein [Abonnement mit einer kostenlosen Testversion](https://azure.microsoft.com/offers/ms-azr-0044p) verfügen, können Sie ein Upgrade auf ein Abonnement mit [nutzungsbasierter Bezahlung](https://azure.microsoft.com/offers/ms-azr-0003p/) durchführen. Weitere Informationen finden Sie unter [Upgrade einer kostenlosen Azure-Testversion auf nutzungsbasierte Bezahlung](../../billing/billing-upgrade-azure-subscription.md) und [FAQ zum kostenlosen Azure-Konto](https://azure.microsoft.com/free/free-account-faq).