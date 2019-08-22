---
title: Azure Load Balancer Standard und Verfügbarkeitszonen
titlesuffix: Azure Load Balancer
description: Load Balancer Standard und Verfügbarkeitszonen
services: load-balancer
documentationcenter: na
author: asudbring
ms.custom: seodec18
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/07/2019
ms.author: allensu
ms.openlocfilehash: 925e7857d337f7f2fd501e4e4467c05952b0da65
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/09/2019
ms.locfileid: "68882945"
---
# <a name="standard-load-balancer-and-availability-zones"></a>Load Balancer Standard und Verfügbarkeitszonen

Azure Load Balancer Standard unterstützt Szenarien mit [Verfügbarkeitszonen](../availability-zones/az-overview.md). Mit Load Balancer Standard können Sie die Verfügbarkeit in Ihrem End-to-End-Szenario optimieren, indem Sie Ressourcen mit Zonen abstimmen und auf mehrere Zonen verteilen.  Unter [Verfügbarkeitszonen](../availability-zones/az-overview.md) erfahren Sie, was Verfügbarkeitszonen sind und welche Regionen derzeit Verfügbarkeitszonen unterstützen. Außerdem finden Sie dort Informationen zu weiteren zugehörigen Konzepten und Produkten. Verfügbarkeitszonen und Load Balancer Standard bilden einen umfassenden und flexiblen Featuresatz für unterschiedlichste Szenarien.  Lesen Sie dieses Dokument, um diese [Konzepte](#concepts) und die [Entwurfsanleitung](#design) für das grundlegende Szenario zu verstehen.

>[!IMPORTANT]
>Informationen zu verwandten Themen finden Sie unter [Verfügbarkeitszonen](../availability-zones/az-overview.md), einschließlich aller regionsspezifischen Informationen.

## <a name="concepts"></a> Auf den Load Balancer angewendete Verfügbarkeitszonenkonzepte

Es gibt keine direkte Beziehung zwischen Load Balancer-Ressourcen und der tatsächlichen Infrastruktur; durch Erstellen einer Load Balancer-Instanz wird keine Instanz erstellt. Load Balancer-Ressourcen sind Objekte, in denen Sie ausdrücken können, wie Azure seine vorgefertigte mehrinstanzenfähige Infrastruktur programmieren sollte, um das Szenario zu erzielen, das Sie erstellen möchten.  Dies ist im Kontext von Verfügbarkeitszonen relevant, da eine einzelne Load Balancer-Ressource die Programmierung der Infrastruktur in mehreren Verfügbarkeitszonen steuern kann, während ein zonenredundanter Dienst aus Kundensicht als einzelne Ressource erscheint.  

Eine Load Balancer-Ressource selbst ist regional und nie zonal.  Ein VNet und ein Subnetz sind auch immer regional und nie zonal. Die Granularität der verfügbaren Konfigurationsoptionen wird durch die jeweilige Konfiguration von Front-End, Regel und Back-End-Pooldefinition eingeschränkt.

Im Kontext von Verfügbarkeitszonen werden das Verhalten und die Eigenschaften einer Load Balancer-Regel als „zonenredundant“ oder „zonal“ beschrieben.  „Zonenredundant“ und „zonal“ beschreiben die Zonalität einer Eigenschaft.  Im Kontext des Load Balancers bezieht sich „zonenredundant“ immer auf *mehrere Zonen*, und „zonal“ bedeutet, dass der Dienst in einer *einzelnen Zone* isoliert wird.

Der öffentliche und der interne Load Balancer unterstützen zonenredundante und zonengebundene Szenarien, und beide können Datenverkehr nach Bedarf zonenübergreifend weiterleiten (*zonenübergreifender Lastenausgleich*). 

### <a name="frontend"></a>Front-End

Ein Load Balancer-Front-End ist eine Front-End-IP-Konfiguration, die entweder auf eine öffentliche IP-Adressressource oder auf eine private IP-Adresse innerhalb des Subnetzes einer virtuellen Netzwerkressource verweist.  Es bildet den Lastenausgleichsendpunkt, in dem Ihr Dienst verfügbar gemacht wird.

Eine Load Balancer-Ressource kann gleichzeitig Regeln mit zonalen und zonenredundanten Front-Ends enthalten. 

Wenn einer Zone eine öffentliche IP-Ressource oder eine private IP-Adresse zugesichert wurde, ist die Zonalität (oder deren Fehlen) nicht veränderbar.  Wenn Sie die Zonalität eines Front-Ends mit öffentlicher IP-Ressource oder privater IP-Adresse ändern oder weglassen möchten, müssen Sie die öffentliche IP-Adresse in der entsprechenden Zone neu erstellen.  Verfügbarkeitszonen haben keine Auswirkungen auf die Einschränkungen für mehrere Front-Ends. Weitere Informationen hierzu finden Sie unter [Mehrere Front-Ends für Azure Load Balancer](load-balancer-multivip-overview.md).

#### <a name="zone-redundant-by-default"></a>Standardmäßige Zonenredundanz

In einer Region mit Verfügbarkeitszonen ist ein Front-End mit Load Balancer Standard standardmäßig zonenredundant.  „Zonenredundant“ bedeutet, dass alle eingehenden oder ausgehenden Datenströme durch mehrere Verfügbarkeitszonen in einer Region gleichzeitig unter Verwendung einer einzelnen IP-Adresse bereitgestellt werden. DNS-Redundanzschemas sind nicht erforderlich. Eine einzelne Front-End-IP-Adresse kann Zonenausfälle überstehen und verwendet werden, um alle (nicht betroffenen) Back-End-Pool-Elemente unabhängig von der Zone zu erreichen. Der Datenpfad bleibt im Falle eines Ausfalls einzelner oder mehrerer Verfügbarkeitszonen verfügbar, solange noch eine einzelne fehlerfreie Zone in der Region vorhanden ist. Die einzelne IP-Adresse des Front-Ends wird gleichzeitig durch mehrere unabhängige Infrastrukturbereitstellungen in mehreren Verfügbarkeitszonen bedient.  Dies bedeutet keinen störungsfreien Datenpfad, aber alle Wiederholungen oder Wiederherstellungen sind in anderen Zonen erfolgreich, die nicht von dem Zonenausfall betroffen sind.   

Der folgende Auszug veranschaulicht, wie Sie eine zonenredundante öffentliche IP-Adresse für Ihre öffentliche Load Balancer Standard-Instanz definieren. Wenn Sie vorhandene Resource Manager-Vorlagen in Ihrer Konfiguration verwenden, fügen Sie diesen Vorlagen den Abschnitt **sku** hinzu.

```json
            "apiVersion": "2017-08-01",
            "type": "Microsoft.Network/publicIPAddresses",
            "name": "public_ip_standard",
            "location": "region",
            "sku":
            {
                "name": "Standard"
            },
```

Der folgende Auszug veranschaulicht, wie Sie eine zonenredundante Front-End-IP-Adresse für Ihre interne Load Balancer Standard-Instanz definieren. Wenn Sie vorhandene Resource Manager-Vorlagen in Ihrer Konfiguration verwenden, fügen Sie diesen Vorlagen den Abschnitt **sku** hinzu.

```json
            "apiVersion": "2017-08-01",
            "type": "Microsoft.Network/loadBalancers",
            "name": "load_balancer_standard",
            "location": "region",
            "sku":
            {
                "name": "Standard"
            },
            "properties": {
                "frontendIPConfigurations": [
                    {
                        "name": "zone_redundant_frontend",
                        "properties": {
                            "subnet": {
                                "Id": "[variables('subnetRef')]"
                            },
                            "privateIPAddress": "10.0.0.6",
                            "privateIPAllocationMethod": "Static"
                        }
                    },
                ],
```

Die obigen Auszüge sind keine vollständigen Vorlagen, sondern veranschaulichen lediglich, wie Eigenschaften für Verfügbarkeitszonen ausgedrückt werden.  Diese Anweisungen müssen in Ihre Vorlagen integriert werden.

#### <a name="optional-zone-isolation"></a>Optionale Zonenisolation

Sie können festlegen, dass ein Front-End – ein so genanntes *zonales Front-End* – einer einzelnen Zone zugesichert wird.  Dies bedeutet, dass alle eingehenden oder ausgehenden Datenströme von einer einzelnen Zone in einer Region bedient werden.  Ihr Front-End ist von der Integrität der Zone abhängig.  Der Datenpfad ist nicht betroffen von Fehlern in Zonen, in denen er nicht zugesichert wurde. Mit zonalen Front-Ends können Sie eine IP-Adresse pro Verfügbarkeitszone verfügbar machen.  

Darüber hinaus können zonale Front-Ends direkt für Endpunkte mit Lastenausgleich innerhalb der einzelnen Zonen genutzt werden. Sie können dies auch verwenden, um pro Zone Lastenausgleichs-Endpunkte zur individuellen Überwachung jeder Zone verfügbar zu machen.  Bei öffentlichen Endpunkten können Sie sie mit einem DNS-Lastenausgleichsprodukt wie [Traffic Manager](../traffic-manager/traffic-manager-overview.md) kombinieren und einen einzelnen DNS-Namen verwenden. Der Client löst diesen DNS-Namen dann in mehrere zonale IP-Adressen auf.  

Wenn Sie diese Konzepte (zonenredundant und zonal für dasselbe Back-End) mischen möchten, lesen Sie den Artikel [Mehrere Front-Ends für Azure Load Balancer](load-balancer-multivip-overview.md).

Für ein öffentliches Load Balancer-Front-End fügen Sie der öffentlichen IP-Ressource, auf die die von der entsprechenden Regel verwendete Front-End-IP-Konfiguration verweist, einen Parameter vom Typ *zones* hinzu.

Für ein internes Load Balancer-Front-End fügen Sie der internen Load Balancer-Front-End-IP-Konfiguration einen *zones*-Parameter hinzu. Das zonale Front-End bewirkt, dass der Load Balancer eine IP-Adresse in einem Subnetz einer bestimmten Zone zusichert.

Der folgende Auszug veranschaulicht, wie Sie eine zonale öffentliche Standard-IP-Adresse in der Verfügbarkeitszone 1 definieren. Wenn Sie vorhandene Resource Manager-Vorlagen in Ihrer Konfiguration verwenden, fügen Sie diesen Vorlagen den Abschnitt **sku** hinzu.

```json
            "apiVersion": "2017-08-01",
            "type": "Microsoft.Network/publicIPAddresses",
            "name": "public_ip_standard",
            "location": "region",
            "zones": [ "1" ],
            "sku":
            {
                "name": "Standard"
            },
```

Der folgende Auszug veranschaulicht, wie Sie ein internes Load Balancer Standard-Front-End in der Verfügbarkeitszone 1 definieren. Wenn Sie vorhandene Resource Manager-Vorlagen in Ihrer Konfiguration verwenden, fügen Sie diesen Vorlagen den Abschnitt **sku** hinzu. Definieren Sie außerdem die Eigenschaft **zones** in der Front-End-IP-Konfiguration für die untergeordnete Ressource.

```json
            "apiVersion": "2017-08-01",
            "type": "Microsoft.Network/loadBalancers",
            "name": "load_balancer_standard",
            "location": "region",
            "sku":
            {
                "name": "Standard"
            },
            "properties": {
                "frontendIPConfigurations": [
                    {
                        "name": "zonal_frontend_in_az1",
                        "zones": [ "1" ],
                        "properties": {
                            "subnet": {
                                "Id": "[variables('subnetRef')]"
                            },
                            "privateIPAddress": "10.0.0.6",
                            "privateIPAllocationMethod": "Static"
                        }
                    },
                ],
```

Die obigen Auszüge sind keine vollständigen Vorlagen, sondern veranschaulichen lediglich, wie Eigenschaften für Verfügbarkeitszonen ausgedrückt werden.  Diese Anweisungen müssen in Ihre Vorlagen integriert werden.

### <a name="cross-zone-load-balancing"></a>Zonenübergreifender Lastenausgleich

Zonenübergreifender Lastenausgleich ist die Fähigkeit von Load Balancer, einen Back-End-Endpunkt in einer beliebigen Zone zu erreichen, und ist unabhängig vom Front-End und seiner Zonalität.  Jede Lastenausgleichsregel kann auf eine Back-End-Instanz in einer beliebigen Verfügbarkeitszone oder in regionalen Instanzen ausgerichtet werden.

Ihr Szenario muss für das Verfügbarkeitszonenkonzept geeignet sein. So müssen Sie beispielsweise Ihre VM-Bereitstellung innerhalb einer einzelnen Zone oder innerhalb mehrerer Zonen zusichern sowie zonale Front-End- und zonale Back-End-Ressourcen auf die gleiche Zone ausrichten.  Wenn Sie eine verfügbarkeitszonenübergreifende Konfiguration mit ausschließlich zonalen Ressourcen verwenden, funktioniert das Szenario zwar, im Hinblick auf Verfügbarkeitszonen steht jedoch möglicherweise kein klarer Fehlermodus zur Verfügung. 

### <a name="backend"></a>Back-End

Load Balancer arbeitet mit Instanzen von virtuellen Computern.  Hierbei kann es sich um eigenständige Instanzen, um Verfügbarkeitsgruppen oder um VM-Skalierungsgruppen handeln.  Eine VM-Instanz in einem einzelnen virtuellen Netzwerk kann Teil des Back-End-Pools sein – unabhängig davon, ob sie einer Zone zugesichert wurde oder welcher Zone sie zugesichert wurde.

Wenn Sie Ihr Front-End und Back-End mit einer einzelnen Zone ausrichten und zusichern möchten, platzieren Sie virtuelle Computer nur innerhalb der gleichen Zone im entsprechenden Back-End-Pool.

Wenn Sie virtuelle Computer über mehrere Zonen hinweg adressieren möchten, platzieren Sie einfach virtuelle Computer aus mehreren Zonen im gleichen Back-End-Pool.  Bei Verwendung von VM-Skalierungsgruppen können Sie eine oder mehrere VM-Skalierungsgruppen im selben Back-End-Pool platzieren.  Außerdem kann jede dieser VM-Skalierungsgruppen sich in einer einzelnen oder mehreren Zonen befinden.

### <a name="outbound-connections"></a>Ausgehende Verbindungen

Die gleichen zonenredundanten und zonalen Eigenschaften gelten für [ausgehende Verbindungen](load-balancer-outbound-connections.md).  Eine zonenredundante öffentliche IP-Adresse für ausgehende Verbindungen wird von allen Zonen bedient. Eine zonale öffentliche IP-Adresse wird nur von der Zone bedient, in der sie zugesichert wurde.  SNAT-Portzuordnungen für ausgehende Verbindungen überstehen Zonenausfälle, und Ihr Szenario bietet weiterhin ausgehende SNAT-Konnektivität, sofern es von dem Zonenausfall nicht betroffen ist.  Bei zonenredundanten Szenarien sind hierzu möglicherweise Übertragungen oder Verbindungswiederherstellungen erforderlich, wenn ein Datenstrom von einer betroffenen Zone bereitgestellt wurde.  Datenströme in nicht betroffenen Zonen bleiben davon unberührt.

Für die SNAT-Portvorabzuordnung wird unabhängig von der Verwendung von Verfügbarkeitszonen der gleiche Algorithmus verwendet.

### <a name="health-probes"></a>Integritätstests

Ihre vorhandenen Integritätstestdefinitionen bleiben so, wie sie ohne Verfügbarkeitszonen sind.  Wir haben allerdings das Integritätsmodell auf der Infrastrukturebene erweitert. 

Bei Verwendung zonenredundanter Front-Ends erweitert Load Balancer sein internes Integritätsmodell, um unabhängig die Erreichbarkeit eines virtuellen Computers aus jeder Verfügbarkeitszone zu testen und Pfade, bei denen Fehler aufgetreten sind, zonenübergreifend ohne Eingriff des Kunden zu schließen.  Wenn ein bestimmter Pfad von der Load Balancer-Infrastruktur einer Zone zu einem virtuellen Computer in einer anderen Zone nicht verfügbar ist, kann Load Balancer diesen Fehler erkennen und vermeiden. Andere Zonen, die diesen virtuellen Computer erreichen können, bedienen den virtuellen Computer weiterhin von ihren jeweiligen Front-Ends aus.  Daher ist es möglich, dass die Zonen während Fehlerereignissen geringfügig unterschiedliche Verteilungen neuer Datenströme aufweisen, während die allgemeine Integrität Ihres End-to-End-Diensts geschützt wird.

## <a name="design"></a> Überlegungen zum Entwurf

Load Balancer ist im Kontext von Verfügbarkeitszonen bewusst flexibel. Sie können sich für eine zonenbasierte Ausrichtung entscheiden oder zonenredundante Regeln verwenden.  Erhöhte Verfügbarkeit kann erhöhte Komplexität mit sich bringen, und Ihr Entwurf muss Verfügbarkeit bei optimaler Leistung bieten.  Lassen Sie uns ein paar wichtige Überlegungen zum Entwurf anstellen.

### <a name="automatic-zone-redundancy"></a>Automatische Zonenredundanz

Load Balancer macht es Ihnen leicht, eine einzelne IP-Adresse als zonenredundantes Front-End einzusetzen. Eine zonenredundante IP-Adresse kann sicher eine zonale Ressource in einer Zone bedienen und einen oder mehrere Zonenfehler überstehen kann, solange eine Zone in der Region fehlerfrei ist. Andererseits ist ein zonales Front-End eine Reduzierung des Diensts auf eine einzelne Zone und von der jeweiligen Zone abhängig.

Zonenredundanz impliziert weder einen störungsfreien Datenpfad noch eine störungsfreie Steuerebene; es ist ausdrücklich eine Datenebene. Zonenredundante Datenströme können alle Zonen verwenden, und die Datenströme eines Kunden verwenden alle fehlerfreien Zonen in einer Region. Bei Ausfall einer Zone sind Datenströme, die zu diesem Zeitpunkt fehlerfreie Zonen verwenden, nicht betroffen.  Datenströme, die zum Zeitpunkt des Zonenausfalls eine Zone verwenden, können beeinträchtigt werden, aber Anwendungen können wiederhergestellt werden. Diese Datenströme können in den verbleibenden fehlerfreien Zonen innerhalb der Region nach erneuter Übertragung oder Wiederherstellung fortgeführt werden, wenn Azure den Zonenausfall in den Griff bekommen hat.

### <a name="xzonedesign"></a> Zonenübergreifende Grenzen

Sie müssen sich unbedingt darüber im Klaren sein, dass Sie jedes Mal, wenn ein End-to-End-Dienst zonenübergreifend wirkt, nicht nur von einer, sondern potenziell von mehreren Zonen abhängig sind.  Daraus resultiert, dass Ihr End-to-End-Dienst möglicherweise keine Verfügbarkeit über nicht zonale Bereitstellungen erlangt hat.

Vermeiden Sie unbeabsichtigte zonenübergreifende Abhängigkeiten, die die durch die Verwendung von Verfügbarkeitszonen erzielten Verfügbarkeitsverbesserungen zunichtemachen.  Wenn Ihre Anwendung aus mehreren Komponenten besteht und Sie gegenüber Zonenausfällen resistent sein möchten, müssen Sie darauf achten, das Überdauern entsprechend wichtiger Komponenten bei Zonenausfällen sicherzustellen.  Eine einzelne wichtige Komponente für Ihre Anwendung kann z.B. die gesamte Anwendung beeinträchtigen, wenn sie nur in einer anderen Zone als der überlebenden vorhanden ist.  Berücksichtigen Sie darüber hinaus auch die Zonenwiederherstellung und wie Ihre Anwendung konvergieren wird. Sie müssen verstehen, wie sich Ihre Anwendung verhält, wenn Teile davon ausfallen. Wir betrachten nun einige wichtige Punkte und verwenden sie als Inspiration für Fragen, die aufkommen, während Sie über Ihr konkretes Szenario nachdenken.

- Wenn Ihre Anwendung aus zwei Komponenten (etwa einer IP-Adresse und einem virtuellen Computer mit verwaltetem Datenträger) besteht und diese in Zone 1 und Zone 2 zugesichert sind, übersteht Ihr End-to-End-Dienst den Ausfall von Zone 1 nicht.  In zonalen Szenarien sollte keine zonenübergreifende Konfiguration verwendet werden – es sei denn, Sie sind sich bewusst, dass dadurch ein potenziell riskanter Fehlermodus entsteht.  Dieses Szenario ist aus Flexibilitätsgründen zulässig.

- Wenn Ihre Anwendung aus zwei Komponenten (etwa einer IP-Adresse und einem virtuellen Computer mit verwaltetem Datenträger) besteht und diese als zonenredundant bzw. in Zone 1 zugesichert sind, übersteht Ihr End-to-End-Dienst den Ausfall von Zone 2, Zone 3 oder beiden Zonen, solange Zone 1 nicht ausfällt.  Allerdings verlieren Sie in gewissem Umfang die Integrität Ihres Diensts aus dem Auge, wenn Sie sich nur auf die Erreichbarkeit des Front-Ends konzentrieren.  Erwägen Sie die Entwicklung eines umfassenderen Integritäts- und Kapazitätsmodells.  Sie können zonenredundante und zonengebundene Konzepte zusammen verwenden, um Einblicke und Verwaltungsfähigkeit zu verbessern.

- Wenn Ihre Anwendung über zwei Komponenten wie ein zonenredundantes Load Balancer-Front-End und eine zonenübergreifende VM-Skalierungsgruppe in drei Zonen verfügt, sind Ihre Ressourcen in nicht von Ausfällen betroffenen Zonen verfügbar, aber die Kapazität Ihres End-to-End-Diensts ist während des Zonenausfalls möglicherweise beeinträchtigt. Aus Sicht der Infrastruktur kann Ihre Bereitstellung einen oder mehrere Zonenausfälle überstehen, und daraus ergeben sich folgende Fragen:
  - Verstehen Sie, wie Ihre Anwendung mit solchen Ausfällen und eingeschränkter Kapazität umgeht?
  - Müssen Sie Sicherheitsmaßnahmen in Ihren Dienst integrieren, um ggf. ein Failover zu einem Regionspaar zu erzwingen?
  - Wie überwachen, erkennen und mildern Sie ein solches Szenario? Sie können möglicherweise mit der Load Balancer Standard-Diagnose die Überwachung Ihrer End-to-End-Dienstleistung erweitern. Berücksichtigen Sie, was verfügbar ist, und welche Ergänzung für ein vollständiges Bild erforderlich ist.

- In Zonen können Ausfälle leichter verständlich sein.  Allerdings unterscheidet sich ein Zonenausfall nicht von anderen Ausfällen, wenn es um Konzepte wie Timeouts, Neuversuche und Backoffalgorithmen geht. Obwohl Azure Load Balancer zonenredundante Pfade bietet und auf Paketebene in Echtzeit eine schnelle Wiederherstellung versucht, können Neuübertragungen oder Wiederherstellungen zu Beginn eines Fehlers auftreten, und Sie müssen wissen, wie Ihre Anwendung mit Fehlern umgeht. Ihr Lastenausgleichsschema wird überdauern, aber Sie müssen für Folgendes planen:
  - Versteht Ihr End-to-End-Dienst den Ausfall einer Zone, und wie nehmen Sie die Wiederherstellung vor, wenn der Status verloren geht?
  - Kann Ihre Anwendung sicher konvergieren, wenn eine Zone zurückkehrt?

Informationen zur Verbesserung der Resilienz Ihrer Anwendung in Ausfallszenarien finden Sie unter [Cloudentwurfsmuster](https://docs.microsoft.com/azure/architecture/patterns/).

### <a name="zonalityguidance"></a> „Zonenredundant“ im Vergleich zu „zonal“

Zonenredundanz kann mit einer zonenunabhängigen Option zur Vereinfachung beitragen und bietet gleichzeitig eine Resilienzoption mit einer einzelnen IP-Adresse für den Dienst.  Sie kann wiederum die Komplexität reduzieren.  Zonenredundanz bietet auch zonenübergreifende Mobilität und kann sicher auf Ressourcen in jeder Zone verwendet werden.  Darüber hinaus ist sie in Regionen ohne Verfügbarkeitszonen zukunftssicher, was den Änderungsaufwand verringern kann, wenn Verfügbarkeitszonen in einer Region verfügbar werden.  Die Konfigurationssyntax für zonenredundante IP-Adressen oder Front-Ends ist in allen Regionen verwendbar – auch in Regionen ohne Verfügbarkeitszonen. Eine Zone wird nicht innerhalb der Eigenschaft „zones:“ der Ressource angegeben.

„Zonal“ kann eine explizite Zusicherung für eine Zone bieten, die explizit von der Integrität der Zone abhängig ist. Die Erstellung einer Load Balancer-Regel mit einem Front-End (zonale IP-Adresse oder zonaler interner Load Balancer) kann insbesondere wünschenswert sein, wenn es sich bei Ihrer angefügten Ressource um einen zonalen virtuellen Computer in der gleichen Zone handelt.  Vielleicht muss für Ihre Anwendung aber auch vorab explizit bekannt sein, in welcher Zone sich eine Ressource befindet, und Sie möchten explizit über die Verfügbarkeit in separaten Zonen nachdenken.  Sie können wählen, mehrere zonale Front-Ends für einen End-to-End-Dienst über Zonen verteilt verfügbar zu machen (d.h. pro Zone zonale Front-Ends für mehrere zonale VM-Skalierungsgruppen).  Wenn Ihre zonalen Front-Ends öffentliche IP-Adressen sind, können Sie diese mehreren zonale Front-Ends zum Verfügbarmachen des Diensts mit [Traffic Manager](../traffic-manager/traffic-manager-overview.md) verwenden.  Sie können mit mehreren zonalen Front-Ends auch pro Zone mit Überwachungslösungen von Drittanbietern Einblicke in Integrität und Leistung erzielen und den gesamten Dienst mit einem zonenredundanten Front-End verfügbar machen. Sie sollten zonale Ressourcen nur mit zonalen Front-Ends bedienen, die an derselben Zone ausgerichtet sind, und potenziell schädliche zonenübergreifende Szenarios für zonale Ressourcen vermeiden.  Zonale Ressourcen sind nur in Regionen mit Verfügbarkeitszonen vorhanden.

Ohne Kenntnis der Dienstarchitektur gibt es keine Faustregel, welche Option die bessere Wahl ist.  Informationen zur Verbesserung der Resilienz Ihrer Anwendung in Ausfallszenarien finden Sie unter [Cloudentwurfsmuster](https://docs.microsoft.com/azure/architecture/patterns/).

## <a name="limitations"></a>Einschränkungen

- Während die Datenebene vollständig zonenredundant ist (solange keine Zonenzusicherung gemacht wurde), sind Vorgänge auf Steuerungsebene nicht vollständig zonenredundant.

## <a name="next-steps"></a>Nächste Schritte
- Weitere Informationen zu [Verfügbarkeitszonen](../availability-zones/az-overview.md)
- Weitere Informationen zum [Load Balancer Standard](load-balancer-standard-overview.md).
- Erfahren Sie mehr über den [Lastenausgleich für VMs innerhalb einer Zone mit einem Standard-Lastenausgleich und einem zonenredundanten Front-End](load-balancer-standard-public-zonal-cli.md).
- Erfahren Sie mehr über den [zonenübergreifenden Lastenausgleich für VMs mit einem Standard-Lastenausgleich und einem zonenredundanten Front-End](load-balancer-standard-public-zone-redundant-cli.md).
- Machen Sie sich mit [Cloudentwurfsmustern](https://docs.microsoft.com/azure/architecture/patterns/) vertraut, um die Resilienz Ihrer Anwendung in Ausfallszenarien zu verbessern.
