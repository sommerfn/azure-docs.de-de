---
title: Optimieren der Kosten für Azure Cosmos DB-Ressourcen mit reservierter Kapazität
description: Erfahren Sie, wie Sie reservierte Kapazitäten für Azure Cosmos DB kaufen, um Computekosten einzusparen.
author: bandersmsft
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 08/29/2019
ms.author: banders
ms.reviewer: sngun
ms.openlocfilehash: 0ee43fe0996c05f4e59f6107ba52fac19b83cdef
ms.sourcegitcommit: 8074f482fcd1f61442b3b8101f153adb52cf35c9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/22/2019
ms.locfileid: "72756951"
---
# <a name="optimize-cost-with-reserved-capacity-in-azure-cosmos-db"></a>Optimieren der Kosten mit reservierter Kapazität in Azure Cosmos DB

Die reservierte Kapazität von Azure Cosmos DB hilft Ihnen, Geld zu sparen, indem Sie Ressourcen von Azure Cosmos DB entweder für ein Jahr oder für drei Jahre verbindlich reservieren. Mit der reservierten Kapazität von Azure Cosmos DB können Sie einen Rabatt auf den Durchsatz erhalten, der für Cosmos DB-Ressourcen bereitgestellt wird. Beispiele für Ressourcen sind Datenbanken und Container (Tabellen, Sammlungen und Graphen).

Reservierte Azure Cosmos DB-Kapazität kann Ihre Cosmos DB-Ausgaben erheblich reduzieren: Sie können mit einer Vorauszahlung für ein bzw. drei Jahre gegenüber den regulären Preisen bis zu 65 % sparen. Reservierte Kapazität stellt lediglich einen Abrechnungsrabatt dar, der keine Auswirkungen auf den Zustand Ihrer Azure Cosmos DB-Ressourcen zur Laufzeit hat.

Die reservierte Kapazität von Azure Cosmos DB deckt den für Ihre Ressourcen bereitgestellten Durchsatz ab. Die Speicher- und Netzwerkkosten werden nicht abgedeckt. Sobald Sie eine Reservierung gekauft haben, werden die Durchsatzgebühren, die den Reservierungsattributen entsprechen, nicht mehr zu den Preisen der nutzungsbasierten Bezahlung abgerechnet. Weitere Informationen zu Reservierungen finden Sie im Artikel [Azure-Reservierungen](../billing/billing-save-compute-costs-reservations.md).

Sie können die reservierte Azure Cosmos DB-Kapazität über das [Azure-Portal](https://portal.azure.com) erwerben. Bezahlen Sie die Reservierung [im Voraus oder monatlich](../billing/billing-monthly-payments-reservations.md). So erwerben Sie reservierte Kapazität:

* Ihnen muss die Besitzerrolle für mindestens ein Enterprise-Abonnement oder ein individuelles Abonnement mit nutzungsbasierter Bezahlung zugeordnet sein.  
* Bei Enterprise-Abonnements muss im [EA-Portal](https://ea.azure.com) die Option **Reservierte Instanzen hinzufügen** aktiviert werden. Wenn diese Einstellung deaktiviert ist, müssen Sie ein EA-Administrator für das Abonnement sein.
* Für das Cloud Solution Provider-Programm (CSP) können nur die Administrator- oder Vertriebs-Agents reservierte Azure Cosmos DB-Kapazitäten kaufen.

## <a name="determine-the-required-throughput-before-purchase"></a>Bestimmen des erforderlichen Durchsatzes vor dem Kauf

Der Umfang der Reservierung sollte sich nach dem Gesamtdurchsatz richten, den die vorhandenen oder in Kürze bereitzustellenden Ressourcen von Azure Cosmos DB verwenden. Sie können den erforderlichen Durchsatz auf folgende Weise ermitteln:

* Rufen Sie die historischen Daten für den gesamten bereitgestellten Durchsatz für Ihre Azure Cosmos DB-Konten, -Datenbanken und -Sammlungen in allen Regionen ab. Sie können beispielsweise den täglich durchschnittlichen bereitgestellten Durchsatz auswerten, indem Sie Ihre tägliche Nutzungsübersicht von `https://account.azure.com` herunterladen.

* Wenn Sie ein EA-Kunde (Enterprise Agreement) sind, können Sie Ihre Nutzungsdatei herunterladen, um die Durchsatzdetails von Azure Cosmos DB zu erhalten. Beachten Sie den Wert **Service Type** im Abschnitt **Additional info** der Nutzungsdatei.

* Sie können den durchschnittlichen Durchsatz für alle Workloads für Ihre Azure Cosmos DB-Konten addieren, den Sie für die nächsten ein bis drei Jahre erwarten. Sie können diesen Wert dann für die Reservierung verwenden.

## <a name="buy-azure-cosmos-db-reserved-capacity"></a>Kaufen von reservierter Azure Cosmos DB-Kapazität

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.  

2. Klicken Sie auf **Alle Dienste** > **Reservierungen** > **Hinzufügen**.  

3. Wählen Sie im Bereich **Reservierungen erwerben** die Option **Azure Cosmos DB**, um eine neue Reservierung zu erwerben.  

4. Füllen Sie die Pflichtfelder aus, wie in der folgenden Tabelle beschrieben:

   ![Formular für die reservierte Kapazität ausfüllen](./media/cosmos-db-reserved-capacity/fill-reserved-capacity-form.png)

   |Feld  |BESCHREIBUNG  |
   |---------|---------|
   |`Scope`   |   Eine Option, die steuert, wie viele Abonnements die mit der Reservierung verbundenen Abrechnungsvorteile nutzen können. Sie steuert auch, wie die Reservierung auf bestimmte Abonnements angewendet wird. <br/><br/>  Wenn Sie **Gemeinsam** auswählen, wird der Reservierungsrabatt wird auf Azure Cosmos DB-Instanzen angewendet, die in einem beliebigen Abonnement innerhalb des Abrechnungskontexts ausgeführt werden. Der Abrechnungskontext basiert darauf, wie Sie sich für Azure registriert haben. Für Enterprise-Kunden stellt der freigegebene Bereich die Registrierung dar und umfasst alle Abonnements in der Registrierung. Für Kunden mit nutzungsbasierter Bezahlung umfasst der freigegebene Bereich einzelne Abonnements mit Preisen für nutzungsbasierte Bezahlung, die vom Kontoadministrator erstellt wurden.  <br/><br/>  Wenn Sie **Einzelabonnement** auswählen, wird der Reservierungsrabatt auf Azure Cosmos DB-Instanzen im ausgewählten Abonnement angewendet. <br/><br/> Wenn Sie die Option **Einzelne Ressourcengruppe** wählen, wird der Reservierungsrabatt auf Azure Cosmos DB-Instanzen im ausgewählten Abonnement und die ausgewählte Ressourcengruppe in diesem Abonnement angewendet. <br/><br/> Sie können den Reservierungsumfang nach dem Kauf der reservierten Kapazität ändern.  |
   |Subscription  |   Das Abonnement, das für die Bezahlung der reservierten Azure Cosmos DB-Kapazitäten verwendet wird. Die Zahlungsmethode für das ausgewählte Abonnement wird für das Inrechnungstellen der Kosten verwendet. Es muss einer der folgenden Abonnementtypen vorliegen: <br/><br/>  Enterprise Agreement (Angebotsnummer: MS-AZR-0017P oder MS-AZR-0148P): Bei einem Enterprise-Abonnement werden die Gebühren vom Verpflichtungsguthaben der Reservierung abgezogen oder als Überschreitung belastet. <br/><br/> Einzelnes Abonnement mit Preisen für nutzungsbasierte Bezahlung (Angebotsnummern: MS-AZR-0003P oder MS-AZR-0023P): Bei einem individuellen Abonnement mit Preisen für nutzungsbasierte Bezahlung wird die Kreditkarte mit den Gebühren belastet, oder die Gebühren werden für Zahlung auf Rechnung berechnet.    |
   | Ressourcengruppe | Ressourcengruppe, auf die der Rabatt für reservierte Kapazitäten angewendet wird. |
   |Begriff  |   Ein Jahr oder drei Jahre   |
   |Durchsatztyp   |  Der Durchsatz wird in Form von Anforderungseinheiten bereitgestellt. Sie können eine Reservierung für den bereitgestellten Durchsatz für beide Setups erwerben – Schreibanforderungen für eine Region sowie Schreibanforderungen für mehre Regionen. Beim Durchsatztyp können Sie zwischen zwei Werten wählen: 100 RU/s pro Stunde und 100 Multimaster-RU/s pro Stunde.|
   | Reservierte Kapazitätseinheiten| Die Menge an Durchsatz, die Sie reservieren möchten. Sie können diesen Wert berechnen, indem Sie den Durchsatz für alle Ihre Cosmos DB-Ressourcen (z.B. Datenbanken oder Container) pro Region ermitteln. Sie multiplizieren diesen Wert dann mit der Anzahl der Regionen, die Sie mit Ihrer Cosmos-Datenbank verknüpfen. Beispiel:  Wenn Sie fünf Regionen mit 1 Million RU/Sek. in jeder Region verwenden, wählen Sie 5 Millionen RU/Sek. für den Kauf von reservierten Kapazitäten aus. |


5. Nach dem Ausfüllen des Formulars wird der Preis berechnet, der für den Erwerb der reservierten Kapazität gilt. In der Ausgabe wird auch der Rabattprozentsatz angezeigt, den Sie für die gewählten Optionen erhalten. Klicken Sie als Nächstes auf **Auswählen**.

6. Prüfen Sie im Bereich **Reservierungen erwerben** den Rabatt und den Preis der Reservierung. Dieser Reservierungspreis gilt für Azure Cosmos DB-Ressourcen mit Durchsatz, die in allen Regionen bereitgestellt werden.  

   ![Zusammenfassung der reservierten Kapazität](./media/cosmos-db-reserved-capacity/reserved-capacity-summary.png)

7. Wählen Sie **Review + buy** (Überprüfen und kaufen) und dann **Jetzt kaufen**. Nach einem erfolgreichen Kauf wird die folgende Seite angezeigt:

Nachdem Sie eine Reservierung gekauft haben, wird sie sofort auf alle vorhandenen Ressourcen von Azure Cosmos DB angewendet, die den Bedingungen der Reservierung entsprechen. Wenn Sie keine vorhandenen Azure Cosmos DB-Ressourcen haben, wird die Reservierung angewendet, wenn Sie eine neue Cosmos DB-Instanz bereitstellen, die den Bedingungen der Reservierung entspricht. In beiden Fällen beginnt der Zeitraum der Reservierung unmittelbar nach dem erfolgreichen Kauf.

Wenn Ihre Reservierung abläuft, werden Ihre Azure Cosmos DB-Instanzen weiter ausgeführt und zu den regulären nutzungsbasierten Gebühren abgerechnet.

## <a name="cancel-exchange-or-refund-reservations"></a>Stornieren, Umtauschen oder Rückerstatten von Reservierungen

Hilfe zum Identifizieren der richtigen reservierten Kapazität finden Sie unter [Grundlegendes zur Anwendung des Rabatts für Reservierungen auf Azure Cosmos DB](../billing/billing-understand-cosmosdb-reservation-charges.md).

Reservierungen können mit bestimmten Einschränkungen storniert, umgetauscht oder rückerstattet werden. Weitere Informationen finden Sie unter [Self-Service-Umtausch und -Rückerstattungen für Azure-Reservierungen](../billing/billing-azure-reservations-self-service-exchange-and-refund.md).

## <a name="next-steps"></a>Nächste Schritte

Der Reservierungsrabatt wird automatisch auf die Azure Cosmos DB-Ressourcen angewendet, die dem Reservierungsbereich und den Reservierungsattributen entsprechen. Sie können den Bereich der Reservierung über das Azure-Portal, PowerShell, die Azure CLI oder die API aktualisieren.

*  Informationen dazu, wie Kapazitätsrabatte auf Azure Cosmos DB angewendet werden, finden Sie unter [Grundlegendes zum Rabatt für Azure-Reservierungen](../billing/billing-understand-cosmosdb-reservation-charges.md).

* Weitere Informationen zu Azure-Reservierungen finden Sie in den folgenden Artikeln:

   * [Was sind Azure-Reservierungen?](../billing/billing-save-compute-costs-reservations.md)  
   * [Verwalten von Azure-Reservierungen](../billing/billing-manage-reserved-vm-instance.md)  
   * [Grundlegendes zur Nutzung von Azure-Reservierungen für den Konzernbeitritt](../billing/billing-understand-reserved-instance-usage-ea.md)  
   * [Grundlegendes zur Nutzung von Azure-Reservierungen für das Abonnement mit nutzungsbasierter Bezahlung](../billing/billing-understand-reserved-instance-usage.md)
   * [Azure-Reservierungen im Partner Center CSP-Programm](https://docs.microsoft.com/partner-center/azure-reservations)

## <a name="need-help-contact-us"></a>Sie brauchen Hilfe? Wenden Sie sich an uns.

Wenn Sie weitere Fragen haben oder Hilfe benötigen, [erstellen Sie eine Supportanfrage](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).
