---
title: Was ist privates Azure-DNS?
description: Übersicht über den privaten DNS-Hostingdienst in Microsoft Azure.
services: dns
author: vhorne
ms.service: dns
ms.topic: overview
ms.date: 6/12/2019
ms.author: victorh
ms.openlocfilehash: 152087ab3dc20dfc95cfeaa0353d961917d362d6
ms.sourcegitcommit: 4d177e6d273bba8af03a00e8bb9fe51a447196d0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/04/2019
ms.locfileid: "71959351"
---
# <a name="what-is-azure-private-dns"></a>Was ist privates Azure-DNS?

Das Domain Name System (DNS) ist für die Übersetzung (oder Auflösung) eines Dienstnamens in die IP-Adresse verantwortlich.  Azure DNS ist ein Hostingdienst für DNS-Domänen, der die Namensauflösung mithilfe der Microsoft Azure-Infrastruktur durchführt. Zusätzlich zu DNS-Domänen mit Internetverbindung unterstützt Azure DNS auch private DNS-Zonen.

Privates Azure-DNS bietet einen zuverlässigen, sicheren DNS-Dienst zum Verwalten und Auflösen von Domänennamen in einem virtuellen Netzwerk, ohne dass Sie eine benutzerdefinierte DNS-Lösung hinzufügen müssen. Durch die Verwendung privater DNS-Zonen können Sie anstelle der momentan verfügbaren von Azure bereitgestellten Namen Ihre eigenen benutzerdefinierten Domänennamen verwenden. Die Verwendung benutzerdefinierter Domänennamen erleichtert Ihnen die optimale Anpassung der Architektur Ihres virtuellen Netzwerks an die Anforderungen Ihres Unternehmens. Es wird eine Namensauflösung für virtuelle Computer in einem virtuellen Netzwerk und zwischen virtuellen Netzwerken bereitgestellt. Darüber hinaus können Sie Zonennamen mit einer Split-Horizon-Ansicht konfigurieren, sodass eine private und eine öffentliche DNS-Zone den Namen verwenden können.

Um die Datensätze einer privaten DNS-Zone in Ihrem virtuellen Netzwerk aufzulösen, müssen Sie das virtuelle Netzwerk mit der Zone verknüpfen. Verknüpfte virtuelle Netzwerke haben Vollzugriff und können alle in der privaten Zone veröffentlichten DNS-Datensätze auflösen. Außerdem können Sie die automatische Registrierung für eine virtuelle Netzwerkverknüpfung aktivieren. Wenn Sie die automatische Registrierung für eine virtuelle Netzwerkverknüpfung aktivieren, werden die DNS-Datensätze für die virtuellen Computer in diesem virtuellen Netzwerk in der privaten Zone registriert. Wenn die automatische Registrierung aktiviert ist, aktualisiert Azure DNS auch die Zoneneinträge, sobald ein virtueller Computer erstellt, seine IP-Adresse geändert oder der virtuelle Computer gelöscht wird.

![Übersicht über DNS](./media/private-dns-overview/scenario.png)

> [!NOTE]
> Als bewährte Methode sollten Sie keine Domäne vom Typ *.local* für Ihre private DNS-Zone verwenden. Nicht alle Betriebssysteme unterstützen dies.

## <a name="benefits"></a>Vorteile

Privates Azure-DNS bietet folgende Vorteile:

* **Macht benutzerdefinierte DNS-Lösungen überflüssig**. Früher erstellten viele Kunden benutzerdefinierte DNS-Lösungen zum Verwalten von DNS-Zonen in ihrem virtuellen Netzwerk. Die Verwaltung von DNS-Zonen kann ab sofort mithilfe der nativen Azure-Infrastruktur durchgeführt werden, sodass das aufwendige Erstellen und Verwalten benutzerdefinierter DNS-Lösungen entfällt.

* **Verwenden aller gängigen DNS-Eintragstypen**. Azure DNS unterstützt A-, AAAA-, CNAME-, MX-, PTR-, SOA-, SRV- und TXT-Einträge.

* **Automatische Verwaltung von Hostnameneinträgen**. Parallel zum Hosten Ihrer benutzerdefinierten DNS-Einträge verwaltet Azure automatisch Hostnameneinträge für die virtuellen Computer in den angegebenen virtuellen Netzwerken. In diesem Szenario können Sie die verwendeten Domänennamen optimieren, ohne benutzerdefinierte DNS-Lösungen erstellen oder die Anwendung ändern zu müssen.

* **Hostnamenauflösung zwischen virtuellen Netzwerken**. Im Gegensatz zu von Azure bereitgestellten Hostnamen können private DNS-Zonen von virtuellen Netzwerken gemeinsam genutzt werden. Diese Möglichkeit vereinfacht netzwerkübergreifende und Dienstermittlungsszenarien wie z.B. das Peering virtueller Netzwerke.

* **Vertraute Tools und Benutzerfreundlichkeit**. Zur Reduzierung der Lernkurve verwendet dieser Dienst die bewährten Azure DNS-Tools (Azure-Portal, Azure PowerShell, Azure-Befehlszeilenschnittstelle, Azure Resource Manager-Vorlagen und REST-API).

* **Split-Horizon-DNS-Unterstützung**. Mit Azure DNS können Sie Zonen mit gleichem Namen erstellen, die innerhalb eines virtuellen Netzwerks und im öffentlichen Internet unterschiedlich aufgelöst werden. Ein typisches Szenario für Split-Horizon-DNS ist die Bereitstellung einer bestimmte Version eines Diensts für die Verwendung in Ihrem virtuellen Netzwerk .

* **In allen Azure-Regionen verfügbar**. Das Azure DNS-Feature für private Zonen ist in allen Azure-Regionen in der öffentlichen Azure-Cloud verfügbar.

## <a name="capabilities"></a>Funktionen

Azure DNS bietet die folgenden Funktionen:

* **Automatische Registrierung von virtuellen Computern in einem virtuellen Netzwerk, das mit einer privaten Zone mit aktivierter automatischer Registrierung verknüpft ist**. Die virtuellen Computer werden für die private Zone als A-Einträge registriert (hinzugefügt), die auf die zugehörigen privaten IP-Adressen verweisen. Beim Löschen eines virtuellen Computers in einer virtuellen Netzwerkverknüpfung mit aktivierter automatischer Registrierung entfernt Azure DNS automatisch auch den entsprechenden DNS-Eintrag aus der verknüpften privaten Zone.

* **Die Forward-DNS-Auflösung wird über virtuelle Netzwerke hinweg unterstützt, die mit der privaten Zone verknüpft sind**. Für die DNS-Auflösung über virtuelle Netzwerke hinweg ist es nicht explizit erforderlich, ein Peering für die virtuellen Netzwerke zu konfigurieren. Möglicherweise möchten Sie jedoch für andere Szenarien (z. B. HTTP-Datenverkehr) ein Peering für virtuelle Netzwerke einrichten.

* **Reverse-DNS-Lookups werden innerhalb des Bereichs eines virtuellen Netzwerks unterstützt**. Bei einem Reverse-DNS-Lookup für eine private IP-Adresse im virtuellen Netzwerk, das einer privaten Zone zugewiesen ist, wird der FQDN zurückgegeben, der den Host-/Eintragsnamen sowie den Zonennamen als Suffix enthält.

## <a name="other-considerations"></a>Weitere Überlegungen

Für Azure DNS gelten die folgenden Einschränkungen:

* Ein bestimmtes virtuelles Netzwerk kann nur mit genau einer privaten Zone verknüpft werden, wenn die automatische Registrierung von VM-DNS-Einträgen aktiviert ist. Sie können jedoch mehrere virtuelle Netzwerke mit einer einzelnen DNS-Zone verknüpfen.
* Reverse-DNS funktioniert nur für den privaten IP-Bereich im verknüpften virtuellen Netzwerk.
* Reverse-DNS für eine private IP-Adresse für ein verknüpftes virtuelles Netzwerk gibt *internal.cloudapp.net* als Standardsuffix für den virtuellen Computer zurück. Für virtuelle Netzwerke, die mit einer privaten Zone mit aktivierter automatischer Registrierung verknüpft sind, gibt Reverse-DNS für eine private IP-Adresse zwei vollqualifizierte Domänennamen zurück: einen mit dem Standardsuffix *internal.cloudapp.net* und den anderen mit dem Suffix der privaten Zone.
* Die bedingte Weiterleitung wird derzeit nicht nativ unterstützt. Sie können die Auflösung zwischen Azure-Netzwerken und lokalen Netzwerken aktivieren. Informationen hierzu finden Sie unter [Namensauflösung für virtuelle Computer und Rolleninstanzen](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).
 
## <a name="pricing"></a>Preise

Informationen zu den Preisen finden Sie unter [Azure DNS – Preise ](https://azure.microsoft.com/pricing/details/dns/).

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie, wie Sie über [Azure PowerShell](./private-dns-getstarted-powershell.md) oder [Azure CLI](./private-dns-getstarted-cli.md) eine private Zone in Azure DNS erstellen.

* Informieren Sie sich über einige gängige [Szenarien zu privaten Zonen](./private-dns-scenarios.md), die mit privaten Zonen in Azure DNS realisiert werden können.

* Allgemeine Fragen und Antworten zu privaten Zonen in Azure DNS, einschließlich des spezifischen Verhaltens für bestimmte Vorgänge, finden Sie unter [Häufig gestellte Fragen zu privatem Azure DNS](./dns-faq-private.md).

* Weitere Informationen zu DNS-Zonen und -Einträgen finden Sie unter [Übersicht über DNS-Zonen und -Einträge](dns-zones-records.md).

* Erfahren Sie mehr über die anderen zentralen [Netzwerkfunktionen](../networking/networking-overview.md) von Azure.
