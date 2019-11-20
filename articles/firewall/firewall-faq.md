---
title: Azure Firewall – Häufig gestellte Fragen
description: Häufig gestellte Fragen zu Azure Firewall
services: firewall
author: vhorne
ms.service: firewall
ms.topic: conceptual
ms.date: 10/19/2019
ms.author: victorh
ms.openlocfilehash: f64e9717a1e6391c15ee5207c7566114f2bf9f8f
ms.sourcegitcommit: b4f201a633775fee96c7e13e176946f6e0e5dd85
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/18/2019
ms.locfileid: "72596779"
---
# <a name="azure-firewall-faq"></a>Azure Firewall – Häufig gestellte Fragen

## <a name="what-is-azure-firewall"></a>Was ist Azure Firewall?

Azure Firewall ist ein verwalteter, cloudbasierter Netzwerksicherheitsdienst, der Ihre Azure Virtual Network-Ressourcen schützt. Es ist eine vollständig zustandsbehaftete Firewall als ein Dienst mit integrierter Hochverfügbarkeit und uneingeschränkter Cloudskalierbarkeit. Sie können Richtlinien zur Anwendungs- und Netzwerkkonnektivität übergreifend für Abonnements und virtuelle Netzwerke zentral erstellen, erzwingen und protokollieren.

## <a name="what-capabilities-are-supported-in-azure-firewall"></a>Welche Funktionen werden in Azure Firewall unterstützt?

* Zustandsbehaftete Firewall als Dienst
* Integrierte Hochverfügbarkeit mit uneingeschränkter Cloudskalierbarkeit
* FQDN-Filterung
* FQDN-Tags
* Filterregeln für den Netzwerkdatenverkehr
* SNAT-Unterstützung für ausgehenden Datenverkehr
* DNAT-Unterstützung für eingehenden Datenverkehr
* Zentrale Erstellung, Erzwingung und Protokollierung der Richtlinien zur Anwendungs- und Netzwerkkonnektivität übergreifend für Abonnements und VNETs
* Vollständige Integration in Azure Monitor für Protokollierung und Analysen

## <a name="what-is-the-typical-deployment-model-for-azure-firewall"></a>Was ist das typische Bereitstellungsmodell für Azure Firewall?

Sie können Azure Firewall in einem virtuellen Netzwerk bereitstellen, Kunden stellen Azure Firewall jedoch in der Regel in einem zentralen virtuellen Netzwerk bereit und verbinden andere virtuelle Netzwerke in einem Hub-and-Spoke-Modell damit. Sie können die Standardroute von den verbundenen virtuellen Netzwerken anschließend so festlegen, dass sie auf dieses zentrale virtuelle Firewall-Netzwerk verweist. Globales VNET-Peering wird unterstützt, aber aufgrund von potenziellen Leistungs- und Latenzproblemen in den Regionen nicht empfohlen. Für optimale Leistungen stellen Sie eine Firewall pro Region bereit.

Der Vorteil dieses Modells ist die Möglichkeit, die Kontrolle über mehrere Spoke-VNETs über verschiedene Abonnements hinweg zentral auszuüben. Es ergeben sich zudem Kosteneinsparungen, da Sie nicht in jedem VNet separat eine Firewall bereitstellen müssen. Die Kosteneinsparungen sollten anhand der zugeordneten Peeringkosten auf der Grundlage der Datenverkehrsmuster der Kunden gemessen werden.

## <a name="how-can-i-install-the-azure-firewall"></a>Wie kann ich Azure Firewall installieren?

Sie können Azure Firewall über das Azure-Portal, PowerShell, die REST-API oder Vorlagen einrichten. Eine Schritt-für-Schritt-Anleitung finden Sie im [Tutorial: Bereitstellen und Konfigurieren von Azure Firewall über das Azure-Portal](tutorial-firewall-deploy-portal.md).

## <a name="what-are-some-azure-firewall-concepts"></a>Wie lauten einige der Azure Firewall-Konzepte?

Azure Firewall unterstützt-Regeln und Regelsammlungen. Eine Regelsammlung ist eine Liste von Regeln mit gleicher Reihenfolge und Priorität. Regelsammlungen werden in der Reihenfolge ihrer Priorität ausgeführt. Netzwerkregelsammlungen haben höhere Priorität als Anwendungsregelsammlungen, und alle Regeln werden beendet.

Es gibt drei Arten von Regelsammlungen:

* *Anwendungsregeln*: Konfigurieren vollqualifizierter Domänennamen (Fully Qualified Domain Names, FQDNs), auf die von einem Subnetz aus zugegriffen werden kann.
* *Netzwerkregeln*: Konfigurieren von Regeln mit Quelladressen, Protokollen, Zielports und Zieladressen.
* *NAT-Regeln*: Konfigurieren von DNAT-Regeln, um eingehende Verbindungen zuzulassen.

## <a name="does-azure-firewall-support-inbound-traffic-filtering"></a>Unterstützt Azure Firewall das Filtern des eingehenden Datenverkehrs?

Azure Firewall unterstützt Filter für eingehenden und ausgehenden Datenverkehr. Der Schutz des eingehenden Datenverkehrs ist für Nicht-HTTP/S-Protokolle bestimmt, z. B. RDP-, SSH- und FTP-Protokolle.

## <a name="which-logging-and-analytics-services-are-supported-by-the-azure-firewall"></a>Welche Protokollierungs- und Analysedienste werden von Azure Firewall unterstützt?

Azure Firewall ist zum Anzeigen und Analysieren von Firewallprotokollen in Azure Monitor integriert. Protokolle können an Log Analytics, Azure Storage oder Event Hubs gesendet werden. Sie können in Log Analytics oder von anderen Tools, wie Excel und Power BI, analysiert werden. Weitere Informationen finden Sie unter [Tutorial: Überwachen von Azure Firewall-Protokollen](tutorial-diagnostics.md).

## <a name="how-does-azure-firewall-work-differently-from-existing-services-such-as-nvas-in-the-marketplace"></a>Wie unterscheidet sich die Funktionsweise von Azure Firewall von vorhandenen Diensten wie NVAs im Marketplace?

Azure Firewall ist ein grundlegender Firewalldienst für verschiedene Kundenszenarios. Es wird erwartet, dass Sie über eine Mischung aus Drittanbieter-NVAs und Azure Firewall verfügen. Eine bessere Zusammenarbeit hat höchste Priorität.

## <a name="what-is-the-difference-between-application-gateway-waf-and-azure-firewall"></a>Was ist der Unterschied zwischen Application Gateway-WAF und Azure Firewall?

Die Web Application Firewall (WAF) ist ein Feature von Application Gateway, das zentralisierten Schutz des eingehenden Datenverkehrs für Ihre Webanwendungen vor allgemeinen Exploits und Sicherheitsrisiken bietet. Azure Firewall bietet Schutz für eingehenden Datenverkehr für Nicht-HTTP/S-Protokolle (z.B. RDP, SSH, FTP), Schutz auf Netzwerkebene für eingehenden Datenverkehr für alle Ports und Protokolle sowie Schutz auf Anwendungsebene für ausgehenden HTTP/S-Datenverkehr.

## <a name="what-is-the-difference-between-network-security-groups-nsgs-and-azure-firewall"></a>Was ist der Unterschied zwischen Netzwerksicherheitsgruppen (Network Security Groups, NSGs) und Azure Firewall?

Der Azure Firewall-Dienst ergänzt die Funktionen der Netzwerksicherheitsgruppen. Zusammen bieten sie eine bessere „Defense-in-Depth“-Netzwerksicherheit. Netzwerksicherheitsgruppen bieten verteilte Datenverkehrsfilterung auf Netzwerkebene, um den Datenverkehr auf Ressourcen in virtuellen Netzwerken in jedem Abonnement zu beschränken. Azure Firewall ist eine vollständig zustandsbehaftete, zentrale Netzwerkfirewall als Dienst, die Schutz auf Netzwerk- und Anwendungsebene für verschiedene Abonnements und virtuelle Netzwerke bietet.

## <a name="are-network-security-groups-nsgs-supported-on-the-azure-firewall-subnet"></a>Werden Netzwerksicherheitsgruppen (NSGs) im Azure Firewall-Subnetz unterstützt?

Azure Firewall ist ein verwalteter Dienst mit mehreren Schutzebenen, z. B. Plattformschutz mit NSGs auf NIC-Ebene (nicht sichtbar).  NSGs auf Subnetzebene sind im Azure Firewall-Subnetz nicht erforderlich und deaktiviert, um zu verhindern, dass es zu Dienstunterbrechungen kommt.

## <a name="how-do-i-set-up-azure-firewall-with-my-service-endpoints"></a>Wie richte ich Azure Firewall für meine Dienstendpunkte ein?

Für den sicheren Zugriff auf PaaS-Dienste werden Dienstendpunkte empfohlen. Sie können Dienstendpunkte im Azure Firewall-Subnetz aktivieren und diese im verbundenen virtuellen Spoke-Netzwerk deaktivieren. Auf diese Weise profitieren Sie von beiden Features: von der Dienstendpunktsicherheit und der zentralen Protokollierung des gesamten Datenverkehrs.

## <a name="what-is-the-pricing-for-azure-firewall"></a>Wie sieht das Preismodell für Azure Firewall aus?

Informationen hierzu finden Sie unter [Azure Firewall – Preise](https://azure.microsoft.com/pricing/details/azure-firewall/).

## <a name="how-can-i-stop-and-start-azure-firewall"></a>Wie kann ich Azure Firewall beenden und starten?

Sie können Azure PowerShell verwenden, um Methoden zuzuordnen und die Zuordnung dafür aufzuheben (*Zuordnen* und *Zuordnung aufheben*).

Beispiel:

```azurepowershell
# Stop an existing firewall

$azfw = Get-AzFirewall -Name "FW Name" -ResourceGroupName "RG Name"
$azfw.Deallocate()
Set-AzFirewall -AzureFirewall $azfw
```

```azurepowershell
# Start a firewall

$azfw = Get-AzFirewall -Name "FW Name" -ResourceGroupName "RG Name"
$vnet = Get-AzVirtualNetwork -ResourceGroupName "RG Name" -Name "VNet Name"
$publicip = Get-AzPublicIpAddress -Name "Public IP Name" -ResourceGroupName " RG Name"
$azfw.Allocate($vnet,$publicip)
Set-AzFirewall -AzureFirewall $azfw
```

> [!NOTE]
> Sie müssen der ursprünglichen Ressourcengruppe und dem Abonnement eine Firewall und eine öffentliche IP-Adresse neu zuordnen.

## <a name="what-are-the-known-service-limits"></a>Welche Dienstgrenzwerte sind bekannt?

Die Einschränkungen des Azure Firewall-Diensts finden Sie unter [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](../azure-subscription-service-limits.md#azure-firewall-limits).

## <a name="can-azure-firewall-in-a-hub-virtual-network-forward-and-filter-network-traffic-between-two-spoke-virtual-networks"></a>Kann Azure Firewall in einem virtuellen Hubnetzwerk den Netzwerkdatenverkehr zwischen zwei virtuellen Spoke-Netzwerken weiterleiten und filtern?

Ja, Sie können Azure Firewall in einem virtuellen Hubnetzwerk verwenden, um den Datenverkehr zwischen zwei virtuellen Spoke-Netzwerken weiterzuleiten und zu filtern. Subnetze in jedem der virtuellen Spoke-Netzwerke müssen über eine UDR verfügen, die auf die Azure Firewall als Standardgateway verweist, damit dieses Szenario ordnungsgemäß funktioniert.

## <a name="can-azure-firewall-forward-and-filter-network-traffic-between-subnets-in-the-same-virtual-network-or-peered-virtual-networks"></a>Kann Azure Firewall den Netzwerkdatenverkehr zwischen Subnetzen im selben virtuellen Netzwerk oder über mittels Peering verknüpfte virtuelle Netzwerke weiterleiten und filtern?

Ja. Die Konfiguration der UDRs zur Umleitung des Datenverkehrs zwischen Subnetzen im gleichen VNET erfordert jedoch zusätzliche Anstrengungen. Während die Verwendung des VNET-Adressbereichs als Zielpräfix für den UDR ausreicht, wird auch der gesamte Datenverkehr von einem Computer zu einem anderen Computer im gleichen Subnetz über die Azure Firewall-Instanz geleitet. Um dies zu vermeiden, fügen Sie eine Route für das Subnetz in den UDR mit dem Typ **VNET** für den nächsten Hop ein. Das Verwalten dieser Routen kann umständlich und fehleranfällig sein. Die empfohlene Methode für die interne Netzwerksegmentierung ist die Verwendung von Netzwerksicherheitsgruppen, die keine UDRs erfordern.

## <a name="does-azure-firewall-outbound-snat-between-private-networks"></a>Übermittelt Azure Firewall SNAT bei privaten Netzwerken?

Azure Firewall bietet kein SNAT, wenn die Ziel-IP-Adresse ein privater IP-Bereich gemäß [IANA RFC 1918](https://tools.ietf.org/html/rfc1918) ist. Wenn Ihre Organisation einen öffentlichen IP-Adressbereich für private Netzwerke verwendet, leitet Azure Firewall den Datenverkehr per SNAT an eine der privaten IP-Adressen der Firewall in AzureFirewallSubnet weiter.

## <a name="is-forced-tunnelingchaining-to-a-network-virtual-appliance-supported"></a>Wird erzwungenes Tunneling bzw. die erzwungene Verkettung mit einem virtuellen Netzwerkgerät unterstützt?

Erzwungenes Tunneling wird derzeit nicht unterstützt. Azure Firewall muss über eine direkte Internetverbindung verfügen. Wenn Ihr Subnetz „AzureFirewallSubnet“ eine Standardroute zu Ihrem lokalen Netzwerk über BGP erfasst, müssen Sie diese mit der benutzerdefinierten Route 0.0.0.0/0 überschreiben. Legen Sie dabei den Wert **NextHopType** auf **Internet** fest, um die direkte Internetkonnektivität beizubehalten.

Wenn für Ihre Konfiguration die Erzwingung eines Tunnels zu einem lokalen Netzwerk erforderlich ist und Sie die Ziel-IP-Präfixe für Ihre Internetziele ermitteln können, können Sie diese Bereiche mit dem lokalen Netzwerk als nächsten Hop über eine benutzerdefinierte Route im Subnetz „AzureFirewallSubnet“ konfigurieren. Oder Sie können BGP verwenden, um diese Routen zu definieren.

## <a name="are-there-any-firewall-resource-group-restrictions"></a>Gibt es Einschränkungen bei Ressourcengruppen?

Ja. Firewall, Subnetz, VNET und die öffentliche IP-Adresse müssen sich in der gleichen Ressourcengruppe befinden.

## <a name="when-configuring-dnat-for-inbound-network-traffic-do-i-also-need-to-configure-a-corresponding-network-rule-to-allow-that-traffic"></a>Muss ich bei der Konfiguration von DNAT für den eingehenden Netzwerkverkehr auch eine entsprechende Netzwerkregel konfigurieren, um diesen Verkehr zu ermöglichen?

Nein. Mit NAT-Regeln wird implizit eine entsprechende Netzwerkregel hinzugefügt, um den übersetzten Datenverkehr zuzulassen. Sie können dieses Verhalten außer Kraft setzen, indem Sie explizit eine Netzwerkregelsammlung mit Ablehnungsregeln hinzufügen, die für den übersetzten Datenverkehr geeignet sind. Weitere Informationen zur Logik für die Azure Firewall-Regelverarbeitung finden Sie unter [Logik für die Azure Firewall-Regelverarbeitung](rule-processing.md).

## <a name="how-do-wildcards-work-in-an-application-rule-target-fqdn"></a>Wie funktionieren Platzhalter in einem FQDN eines Anwendungsregelziels?

Wenn Sie * **.contoso.com** konfigurieren, ist *anyvalue*.contoso.com, aber nicht „contoso.com“ (Domäne an der Spitze der Zone) zulässig. Wenn Sie die Domäne an der Spitze der Zone zulassen möchten, müssen Sie sie explizit als Ziel-FQDN konfigurieren.

## <a name="what-does-provisioning-state-failed-mean"></a>Was bedeutet *Bereitstellungsstatus: Fehler*?

Wenn eine Konfigurationsänderung angewendet wird, versucht Azure Firewall alle zugrunde liegenden Back-End-Instanzen zu aktualisieren. In seltenen Fällen kann bei der Aktualisierung einer dieser Back-End-Instanzen mit der neuen Konfiguration ein Fehler auftreten, und der Aktualisierungsvorgang wird mit dem Bereitstellungsstatus „Fehler“ angehalten. Azure Firewall ist noch funktionsfähig, die angewendete Konfiguration befindet sich jedoch unter Umständen in einem inkonsistenten Zustand, in dem einige Instanzen die vorherige Konfiguration und andere den aktualisierten Regelsatz verwenden. Versuchen Sie in diesem Fall, die Konfiguration noch einmal zu aktualisieren, bis der Vorgang erfolgreich war und für Ihre Firewall der Bereitstellungsstatus *Erfolgreich* angezeigt wird.

### <a name="how-does-azure-firewall-handle-planned-maintenance-and-unplanned-failures"></a>Wie verarbeitet Azure Firewall geplante Wartungsarbeiten und nicht geplante Fehler?
Azure Firewall besteht aus mehreren Back-End-Knoten in einer aktiv/aktiv-Konfiguration.  Für jede geplante Wartung gibt es Verbindungsausgleichslogik zum ordnungsgemäßen Aktualisieren von Knoten.  Updates sind für jede Azure-Region außerhalb der Geschäftszeiten geplant, um das Risiko einer Unterbrechung weiter einzuschränken.  Bei nicht geplanten Problemen wird ein neuer Knoten instanziiert, um den fehlerhaften Knoten zu ersetzen.  Die Konnektivität zum neuen Knoten wird in der Regel innerhalb von 10 Sekunden nach dem Zeitpunkt des Fehlers wieder hergestellt.

## <a name="is-there-a-character-limit-for-a-firewall-name"></a>Gibt es eine Beschränkung der Zeichenzahl für einen Firewallnamen?

Ja. Ein Firewallname ist auf 50 Zeichen beschränkt.

## <a name="why-does-azure-firewall-need-a-26-subnet-size"></a>Warum benötigt Azure Firewall die Subnetzgröße /26?

Der Azure Firewall-Dienst muss mehr VM-Instanzen bereitstellen, als er skaliert. Ein /26-Adressraum stellt sicher, dass für die Firewall genügend IP-Adressen vorhanden sind, um der Skalierung gerecht zu werden.

## <a name="does-the-firewall-subnet-size-need-to-change-as-the-service-scales"></a>Muss die Größe des Firewallsubnetzes geändert werden, wenn der Dienst eine Skalierung ausführt?

Nein. Azure Firewall benötigt kein Subnetz, das größer als /26 ist.

## <a name="how-can-i-increase-my-firewall-throughput"></a>Wie kann ich meinen Firewalldurchsatz erhöhen?

Die anfängliche Durchsatzkapazität von Azure Firewall liegt zwischen 2,5 und 3 GBit/s. Das horizontale Hochskalieren basiert aktuell nur auf der CPU-Auslastung. Es kann also vorkommen, dass eine Firewall, die nur über Netzwerkregeln verfügt, nicht horizontal hochskaliert wird, um den Durchsatz zu erhöhen, da die Netzwerkregeln keine signifikanten Auswirkungen auf die CPU-Auslastung haben. Sollten Sie einen höheren Durchsatz für Ihre Firewall benötigen, wenden Sie sich an den Support, um die anfängliche Durchsatzkapazität Ihrer Firewall zu erhöhen.

## <a name="how-long-does-it-take-for-azure-firewall-to-scale-out"></a>Wie lange dauert es, bis Azure Firewall horizontal hochskaliert wird?

Aktuell dauert es zwischen fünf und sieben Minuten, bis Azure Firewall horizontal hochskaliert wird. Wenn bei Ihnen Bursts auftreten, die eine schnellere automatische Skalierung erfordern, wenden Sie sich an den Support, um die anfängliche Durchsatzkapazität Ihrer Firewall zu erhöhen.

## <a name="does-azure-firewall-allow-access-to-active-directory-by-default"></a>Lässt Azure Firewall standardmäßig den Zugriff auf Active Directory zu?

Nein. Standardmäßig blockiert Azure Firewall den Zugriff auf Active Directory. Konfigurieren Sie das Diensttag „AzureActiveDirectory“, um den Zugriff zuzulassen. Weitere Informationen finden Sie unter [Azure Firewall-Diensttags](service-tags.md).
