---
title: Was sind Azure-Reservierungen?
description: Erfahren Sie mehr über Azure-Reservierungen und -Preise, um Kosten für virtuelle Computer, SQL-Datenbank-Instanzen, Azure Cosmos DB-Instanzen und andere Ressourcen zu sparen.
author: yashesvi
manager: yashar
ms.service: billing
ms.topic: conceptual
ms.date: 07/03/2019
ms.author: banders
ms.openlocfilehash: cd0a70aa0fb5096c5b0157ae078c961da03109bc
ms.sourcegitcommit: d2785f020e134c3680ca1c8500aa2c0211aa1e24
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/04/2019
ms.locfileid: "67565323"
---
# <a name="what-are-azure-reservations"></a>Was sind Azure-Reservierungen?

Mit Azure-Reservierungen können Sie Geld sparen, indem Sie für virtuelle Computer, SQL-Datenbank-Computekapazität, Azure Cosmos DB-Durchsatz oder andere Azure-Ressourcen für ein Jahr oder für drei Jahre im Voraus zahlen. Durch die Vorabzahlung können Sie einen Rabatt für die Ressourcen, die Sie nutzen, in Anspruch nehmen. Reservierungen können Ihre Kosten für virtuelle Computer, SQL-Datenbank-Compute, Azure Cosmos DB-Kapazitäten oder andere Ressourcen deutlich um bis zu 72 % gegenüber der nutzungsbasierten Bezahlung reduzieren. Reservierungen bieten einen Abrechnungsrabatt und wirken sich nicht auf den Laufzeitstatus Ihrer Ressourcen aus.

Sie können eine Reservierung im [Azure-Portal](https://ms.portal.azure.com/?microsoft_azure_marketplace_ItemHideKey=Reservations&Microsoft_Azure_Reservations=true#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade) erwerben.

## <a name="why-buy-a-reservation"></a>Warum eine Reservierung kaufen?

Wenn Sie über virtuelle Computer, Azure Cosmos DB- oder SQL-Datenbank-Instanzen verfügen, die für längere Zeiträume ausgeführt werden, erhalten Sie durch den Kauf einer reservierten Instanz die kostengünstigste Option. Wenn Sie beispielsweise kontinuierlich vier Instanzen von einem Dienst ohne Reservierung ausführen, fallen die Gebühren für die nutzungsbasierte Bezahlung an. Wenn Sie eine Reservierung für diese Ressourcen kaufen, erhalten Sie sofort den Rabatt auf Reservierungen. Die Ressourcen werden nicht mehr mit den Gebühren für die nutzungsbasierte Bezahlung in Rechnung gestellt.

## <a name="charges-covered-by-reservation"></a>Gebühren, die durch Reservierung abgedeckt werden

Servicepläne:

- **Reservierte VM-Instanz**: Eine Reservierung deckt nur die Computekosten virtueller Computer ab. Eine Reservierung deckt keine zusätzlichen Kosten für Software, Netzwerke oder Speicher ab.
- **Reservierte Azure Cosmos DB-Kapazität**: Eine Reservierung deckt den für Ihre Ressourcen bereitgestellten Durchsatz ab. Die Speicher- und Netzwerkkosten werden nicht abgedeckt.
- **Reservierte virtuelle Kerne für SQL-Datenbank**: In einer Reservierung sind nur die Computekosten enthalten. Die Lizenz wird separat abgerechnet.

Für virtuelle Windows-Computer und SQL-Datenbank können Sie die Lizenzierungskosten mit dem [Azure-Hybridvorteil](https://azure.microsoft.com/pricing/hybrid-benefit/) decken.

## <a name="whos-eligible-to-purchase-a-reservation"></a>Wer ist zum Erwerb einer Reservierung berechtigt?

Um einen Plan zu kaufen, müssen Sie eine Rolle des Abonnementverantwortlichen bei einem Enterprise-Abonnement (MS-AZR-0017P oder MS-AZR-0148P) oder einem Abonnement mit nutzungsbasierter Zahlung (MS-AZR-003P oder MS-AZR-0023P) haben. Cloudlösungsanbieter können Azure-Reservierungen über das Azure-Portal oder  [Partner Center](/partner-center/azure-reservations)  erwerben.

EA-Kunden können Käufe für EA-Administratoren beschränken, indem sie die Option **Reservierte Instanzen hinzufügen** im EA-Portal deaktivieren. EA-Administratoren müssen der Besitzer für mindestens ein EA-Abonnement sein, um eine Reservierung zu erwerben. Die Option ist nützlich für Unternehmen, die ein zentralisiertes Team benötigen, um Reservierungen für verschiedene Kostenstellen einzukaufen. Nach dem Kauf können zentralisierte Teams die Reservierungen um Kostenstellenbesitzer erweitern. Die Besitzer können dann die Reservierung auf ihre Abonnements ausdehnen. Das zentrale Team muss keinen Zugriff des Abonnenten haben, wenn die Reservierung gekauft wird.

Ein Reservierungsrabatt gilt nur für Ressourcen, die mit Abonnements verbunden sind, die über Enterprise, CSP und individuelle Pläne mit nutzungsbasierten Tarifen erworben wurden.

## <a name="scope-reservations"></a>Bereichsreservierungen

Sie können den Bereich für eine Reservierung auf ein Abonnement oder eine Ressourcengruppe festlegen. Wenn Sie den Bereich für eine Reservierung festlegen, wird ausgewählt, wo die Reservierungseinsparungen gelten. Wenn Sie den Bereich der Reservierung auf eine Ressourcengruppe festlegen, gelten die Reservierungsrabatte nur für die Ressourcengruppe und nicht für das gesamte Abonnement.

### <a name="reservation-scoping-options"></a>Optionen für Reservierungsbereiche

Beim Festlegen von Ressourcengruppenbereichen stehen Ihnen je nach Bedarf drei Optionen zur Verfügung, mit denen Sie den Bereich einer Reservierung definieren können:

- **Single resource group scope** (Bereich einer einzelnen Ressourcengruppe): Wendet den Reservierungsrabatt nur auf die entsprechenden Ressourcen in der ausgewählten Ressourcengruppe an.
- **Single subscription scope** (Bereich eines einzelnen Abonnements): Wendet den Reservierungsrabatt auf die entsprechenden Ressourcen im ausgewählten Abonnement an.
- **Shared scope** (Gemeinsam genutzter Bereich): Wendet den Reservierungsrabatt auf die entsprechenden Ressourcen in berechtigten Abonnements innerhalb des Abrechnungskontexts an. Für Kunden mit einem Enterprise Agreement ist der Abrechnungskontext die Registrierung. Für Kunden mit individuellen Abonnements mit nutzungsbasierten Tarifen handelt es sich beim Abrechnungsbereich um alle berechtigten Abonnements, die vom Kontoadministrator erstellt wurden.

Während Reservierungsrabatte auf Ihre Nutzung angewendet werden, verarbeitet Azure die Reservierung in der folgenden Reihenfolge:

1. Reservierungen mit einem Bereich für eine Ressourcengruppe
2. Reservierungen mit einem einzelnen Bereich
3. Reservierungen mit einem gemeinsam genutzten Bereich

Eine einzelne Ressourcengruppe kann abhängig davon, wie Sie Ihre Reservierungen festlegen, Reservierungsrabatte von mehreren Reservierungen erhalten.

### <a name="scope-a-reservation-to-a-resource-group"></a>Festlegen einer Reservierung auf eine Ressourcengruppe

Sie können den Bereich der Reservierung auf eine Ressourcengruppe festlegen, wenn Sie die Reservierung erwerben, oder Sie legen den Bereich nach dem Kauf fest. Sie müssen Besitzer eines Abonnements sein, um den Bereich der Reservierung auf eine Ressourcengruppe festzulegen.

Um den Bereich festzulegen, navigieren Sie zur Seite [Einkaufsreservierung](https://ms.portal.azure.com/#blade/Microsoft\_Azure\_Reservations/CreateBlade/referrer/Browse\_AddCommand) im Azure-Portal. Wählen Sie dann den zu erwerbenden Reservierungstyp aus. Ändern Sie im Auswahlformular **Produkt auswählen, das Sie erwerben möchten** den Wert des **Bereichs** in **Einzelne Ressourcengruppe**, und wählen Sie eine Ressourcengruppe aus.

![Beispiel für die Kaufauswahl einer VM-Reservierung](./media/billing-save-compute-costs-reservations/select-product-to-purchase.png)

Kaufempfehlungen für die Ressourcengruppe in der Reservierung des virtuellen Computers werden angezeigt. Empfehlungen werden berechnet, indem die Nutzung während der letzten 30 Tagen analysiert wird. Eine Kaufempfehlung erfolgt, wenn die Kosten für das Ausführen von Ressourcen mit reservierten Instanzen günstiger sind als die Kosten für die Ausführung von Ressourcen mit nutzungsbasierter Bezahlung. Weitere Informationen zu Reservierungskaufempfehlungen finden Sie im Blogbeitrag [Get Reserved Instance purchase recommendations based on usage pattern](https://azure.microsoft.com/blog/get-usage-based-reserved-instance-recommendations) (Erhalten von Kaufempfehlungen für die reservierte Instanz basierend auf dem Nutzungsverhalten).

Sie können den Bereich nach dem Erwerb einer Reservierung immer aktualisieren. Navigieren Sie zu diesem Zweck zu der Reservierung, klicken Sie auf **Konfiguration**, und legen Sie den Bereich für die Reservierung erneut fest. Das Neuzuweisen eines Bereichs für eine Reservierung ist keine kommerzielle Transaktion. Die Reservierungsbedingungen ändern sich nicht. Weitere Informationen zum Aktualisieren des Bereichs finden Sie unter [Aktualisieren des Bereichs nach dem Erwerb einer Reservierung](billing-manage-reserved-vm-instance.md#change-the-reservation-scope).

![Beispiel für eine Änderung des Reservierungsbereichs](./media/billing-save-compute-costs-reservations/rescope-reservation-resource-group.png)

### <a name="monitor-and-optimize-reservation-usage"></a>Überwachen und Optimieren der Reservierungsnutzung

Sie können Ihre Reservierungsnutzung auf verschiedene Weise überwachen: über das Azure-Portal, über APIs oder über Nutzungsdaten. Um alle Reservierungen anzuzeigen, auf die Sie zugreifen können, navigieren Sie im Azure-Portal zu **Reservierungen**. Das Reservierungsraster zeigt den zuletzt aufgezeichneten Nutzungsprozentsatz für die Reservierung an. Klicken Sie auf die Reservierung, um die langfristige Nutzung der Reservierung anzuzeigen.

Wenn Sie Enterprise Agreement-Kunde sind, können Sie die Reservierungsnutzung auch mithilfe von [APIs](billing-reservation-apis.md#see-reservation-usage) und aus Ihren [Nutzungsdaten](billing-understand-reserved-instance-usage-ea.md#common-cost-and-usage-tasks) abrufen.

Wenn Sie feststellen, dass die Nutzung der Reservierung mit dem Bereich „Ressourcengruppe“ gering ist, können Sie den Reservierungsbereich auf ein einzelnes Abonnement aktualisieren oder über den Abrechnungskontext gemeinsam verwenden. Sie können die Reservierung auch aufteilen und die sich daraus ergebenden Reservierungen auf verschiedene Ressourcengruppen anwenden.

### <a name="other-considerations"></a>Weitere Überlegungen

Wenn Sie nicht über passende Ressourcen in einer Ressourcengruppe verfügen, wird die Reservierung nicht ausgenutzt. Die Reservierung gilt nicht automatisch für eine andere Ressourcengruppe oder ein anderes Abonnement, wenn eine geringe Auslastung vorliegt.

Ein Reservierungsbereich wird nicht automatisch aktualisiert, wenn Sie die Ressourcengruppe aus einem Abonnement in ein anderes verschieben. Sie müssen den Bereich der Reservierung neu festlegen. Andernfalls wird die Reservierung nicht ausgenutzt.

## <a name="discounted-subscription-and-offer-types"></a>Abonnements und Angebotstypen mit Rabatt

Reservierungsrabatte gelten für die folgenden berechtigten Abonnements und Angebotstypen.

- Enterprise Agreement (Angebotsnummern: MS-AZR-0017P oder MS-AZR-0148P)
- Individuelle Pläne mit Tarifen für nutzungsbasierte Bezahlung (Angebotsnummern: MS-AZR-0003P oder MS-AZR-0023P)
- CSP-Abonnements

Für Ressourcen, die in einem Abonnement mit anderen Angebotstypen ausgeführt werden, gilt der Reservierungsrabatt nicht.

## <a name="how-is-a-reservation-billed"></a>Wie wird eine Reservierung abgerechnet?

Die Reservierung wird mit der Zahlungsmethode in Rechnung gestellt, die mit dem Abonnement verknüpft ist. Wenn Sie über ein Enterprise-Abonnement verfügen, werden die Reservierungskosten von Ihrem Verpflichtungsguthaben abgezogen. Wenn Ihr Zahlungsverpflichtungssaldo die Kosten für die Reservierung nicht abdeckt, wird Ihnen die Überschreitung in Rechnung gestellt. Wenn Sie über ein Abonnement in einem individuellen Plan mit nutzungsbasierten Tarifen verfügen, wird die Kreditkarte Ihres Konto umgehend belastet. Wenn Sie Rechnungen erhalten, sind die Gebühren Ihrer nächsten Rechnung aufgeführt.

## <a name="how-reservation-discount-is-applied"></a>Wie der Reservierungsrabatt angewendet wird

Der Reservierungsrabatt gilt für die Ressourcennutzung, die den Attributen entspricht, die Sie beim Kauf der Reservierung auswählen. Zu diesen Attributen gehört der Bereich, in dem die entsprechenden VMs, SQL-Datenbank-Instanzen, Azure Cosmos DB-Instanzen oder anderen Ressourcen ausgeführt werden. Beispiel: Wenn Sie einen Rabatt auf eine Reservierung für vier VMs vom Typ „Standard D2“ in der Region „USA, Westen“ nutzen möchten, wählen Sie das Abonnement aus, in dem die VMs ausgeführt werden.

Reservierungsrabatte funktionieren nach dem Prinzip „*use-it-or-lose-it*“. Wenn Sie für eine Stunde nicht über die entsprechenden Ressourcen verfügen, verlieren Sie folglich eine Reservierungsmenge für diese Stunde. Ungenutzte reservierte Stunden können nicht übertragen werden.

Wenn Sie eine Ressource beenden, wird der Reservierungsrabatt automatisch auf eine andere entsprechende Ressource im angegebenen Reservierungsumfang angewandt. Wenn keine übereinstimmenden Ressourcen im angegebenen Reservierungsumfang gefunden werden, gehen die reservierten Stunden *verloren*.

Beispielsweise können Sie später eine Ressource erstellen und über eine entsprechende Reservierung verfügen, die nicht ausgelastet ist. In diesem Beispiel gilt der Reservierungsrabatt automatisch für die neue übereinstimmende Ressource.

Wenn die virtuellen Computer in verschiedenen Abonnements in Ihrer Registrierung/Ihrem Konto ausgeführt werden, wählen Sie den Bereich „Freigegeben“ aus. Der Bereich „Freigegeben“ ermöglicht die abonnementübergreifende Anwendung des Reservierungsrabatts. Sie können den Bereich nach dem Erwerb einer Reservierung ändern. Weitere Informationen finden Sie unter [Verwalten von Azure-Reservierungen](billing-manage-reserved-vm-instance.md).

Der Rabatt auf Reservierungen gilt nur für Ressourcen, denen einer der Abonnementtypen „Enterprise“, „CSP“ oder mit Tarifen mit nutzungsbasierter Bezahlung zugeordnet ist. Für Ressourcen, die in einem Abonnement mit anderen Angebotstypen ausgeführt werden, gilt der Reservierungsrabatt nicht.

## <a name="when-the-reservation-term-expires"></a>Wann das Ende des Reservierungszeitraums erreicht ist

Am Ende der Laufzeit der Reservierung läuft der Abrechnungsrabatt ab, und die Infrastruktur der virtuellen Computer, SQL-Datenbank-Instanzen, Azure Cosmos DB-Instanzen oder anderen Ressourcen wird mit den Gebühren für die nutzungsbasierte Bezahlung in Rechnung gestellt. Azure-Reservierungen verlängern sich nicht automatisch. Zur weiteren Nutzung des Abrechnungsrabatts müssen Sie eine neue Reservierung für Dienste und Software erwerben, die für Reservierungen berechtigt sind.

## <a name="discount-applies-to-different-sizes"></a>Rabatt gilt für verschiedene Größen

Wenn Sie eine Reservierung erwerben, kann der Rabatt auf andere Instanzen mit Attributen angewendet werden, die sich in der gleichen Größengruppe befinden. Dieses Feature wird als „Instanzgrößenflexibilität“ bezeichnet. Die Flexibilität der Rabattabdeckung hängt vom Reservierungstyp und den Attributen ab, die Sie beim Erwerb der Reservierung auswählen.

Servicepläne:

- Reservierte VM-Instanzen: Wenn Sie die Reservierung erwerben und unter **Optimiert für** die Option **Instanzgrößenflexibilität** auswählen, hängt die Abdeckung des Rabatts von der ausgewählten VM-Größe ab. Die Reservierung kann für VM-Größen in derselben Größenordnung gelten. Weitere Informationen finden Sie unter [Flexibilität bei der VM-Größe mit reservierten VM-Instanzen](../virtual-machines/windows/reserved-vm-instance-size-flexibility.md).
- Reservierte SQL-Datenbank-Kapazität: Die Rabattabdeckung hängt von der ausgewählten Leistungsstufe ab. Weitere Informationen finden Sie unter [Grundlegendes zur Anwendung eines Rabatts für Azure-Reservierungen auf SQL-Datenbank-Instanzen](billing-understand-reservation-charges.md).
- Reservierte Azure Cosmos DB-Kapazität: Die Rabattabdeckung hängt vom bereitgestellten Durchsatz ab. Weitere Informationen finden Sie unter [Understand how an Azure Cosmos DB reservation discount is applied (Grundlegendes zur Anwendung eines Rabatts für Azure Cosmos DB-Reservierungen)](billing-understand-cosmosdb-reservation-charges.md).

## <a name="need-help-contact-us"></a>Sie brauchen Hilfe? Wenden Sie sich an uns.

Wenn Sie Fragen haben oder Hilfe benötigen, [erstellen Sie eine Supportanfrage](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu Azure-Reservierungen finden Sie in den folgenden Artikeln:
    - [Verwalten von Azure-Reservierungen](billing-manage-reserved-vm-instance.md)
    - [Grundlegendes zur Reservierungsnutzung bei einem Abonnement mit nutzungsbasierten Tarifen ](billing-understand-reserved-instance-usage.md)
    - [Grundlegendes zur Nutzung von Azure-Reservierungen für den Konzernbeitritt](billing-understand-reserved-instance-usage-ea.md)
    - [Nicht in Azure-Reservierungen enthaltene Windows-Softwarekosten](billing-reserved-instance-windows-software-costs.md)
    - [Verkaufen Microsoft Azure Reserved Instances](/partner-center/azure-reservations)

- Weitere Informationen zu Reservierungen für Diensttarife:
    - [Virtuelle Computer mit Azure Reserved VM Instances](../virtual-machines/windows/prepay-reserved-vm-instances.md)
    - [Azure Cosmos DB-Ressourcen mit reservierter Azure Cosmos DB-Kapazität](../cosmos-db/cosmos-db-reserved-capacity.md)
    - [SQL-Datenbank-Computeressourcen mit reservierter Azure SQL-Datenbank-Kapazität](../sql-database/sql-database-reserved-capacity.md). Erfahren Sie mehr über Reservierungen für Softwarepläne:
    - [Red Hat-Softwarepläne aus Azure-Reservierungen](../virtual-machines/linux/prepay-rhel-software-charges.md)
    - [SUSE-Softwarepläne aus Azure-Reservierungen](../virtual-machines/linux/prepay-suse-software-charges.md)
