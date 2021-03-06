---
title: DNS in Azure Stack | Microsoft-Dokumentation
description: Verwenden von DNS in Azure Stack
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2019
ms.author: sethm
ms.lastreviewed: 01/05/2019
ms.openlocfilehash: 3a4efb6282a0aca8173403009fd58e9154a91b2b
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/23/2019
ms.locfileid: "56728948"
---
# <a name="using-dns-in-azure-stack"></a>Verwenden von DNS in Azure Stack

*Anwendungsbereich: Integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Azure Stack unterstützt die folgenden Domain Name System-Features (DNS):

* DNS-Hostnamenauflösung
* Erstellen und Verwalten von DNS-Zonen und -Einträgen mit der API

## <a name="support-for-dns-hostname-resolution"></a>Unterstützung für DNS-Hostnamenauflösung

Sie können eine DNS-Domänennamenbezeichnung für öffentliche IP-Ressourcen angeben. Azure Stack verwendet **domainnamelabel.location.cloudapp.azurestack.external** als Bezeichnungsnamen und nimmt die Zuordnung zur öffentlichen IP-Adresse in von Azure Stack verwalteten DNS-Servern vor.

Wenn Sie beispielsweise eine öffentliche IP-Ressource mit **contoso** als Domänennamenbezeichnung im Speicherort „Lokaler Azure Stack“ erstellen, wird der [vollqualifizierte Domänenname (FQDN)](https://en.wikipedia.org/wiki/Fully_qualified_domain_name) **contoso.local.cloudapp.azurestack.external** in die öffentliche IP-Adresse der Ressource aufgelöst. Mit diesem FQDN können Sie einen benutzerdefinierten CNAME-Domäneneintrag erstellen, der auf die öffentliche IP-Adresse in Azure Stack verweist.

Weitere Informationen zur Namensauflösung finden Sie im Artikel [DNS-Auflösung](../../dns/dns-for-azure-services.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

> [!IMPORTANT]
> Jede erstellte Domänennamenbezeichnung muss für den Azure Stack-Speicherort eindeutig sein.

Der folgende Screenshot zeigt das Dialogfeld **Öffentliche IP-Adresse erstellen** zum Erstellen einer öffentlichen IP-Adresse mithilfe des Portals:

![Erstellen einer öffentlichen IP-Adresse](media/azure-stack-dns/image01.png)

### <a name="example-scenario"></a>Beispielszenario

Sie verfügen über einen Lastenausgleich, der Anforderungen einer Webanwendung verarbeitet. Im Hintergrund des Lastenausgleichs befindet sich eine Website, die auf einem oder mehreren virtuellen Computern ausgeführt wird. Jetzt können Sie auf die Website mit Lastenausgleich mit einem DNS-Namen zugreifen, statt mit einer IP-Adresse.

## <a name="create-and-manage-dns-zones-and-records-using-the-api"></a>Erstellen und Verwalten von DNS-Zonen und -Einträgen mit der API

Sie können DNS-Zonen und -Einträge in Azure Stack erstellen und verwalten.

Azure Stack stellt ähnlich wie Azure einen DNS-Dienst bereit. Dabei werden APIs verwendet, die mit den Azure DNS-APIs konsistent sind.  Durch das Hosten Ihrer Domänen in Azure Stack-DNS können Sie Ihre DNS-Einträge mithilfe der gleichen Anmeldeinformationen, APIs und Tools verwalten. Sie können auch die gleiche Abrechnung und den gleichen Support wie für die anderen Azure-Dienste verwenden.

Die Azure Stack-DNS-Infrastruktur ist kompakter als in Azure. Die Größe und der Speicherort einer Azure Stack-Bereitstellung wirken sich auf den Bereich, den Umfang und die Leistung von DNS aus. Das bedeutet auch, dass Leistung, Verfügbarkeit, globale Verteilung und Hochverfügbarkeit von Bereitstellung zu Bereitstellung variieren können.

## <a name="comparison-with-azure-dns"></a>Vergleich mit dem Azure-DNS

DNS in Azure Stack ist DNS in Azure ähnlich, aber es gibt ein paar wichtige Ausnahmen:

* **AAAA-Einträge werden nicht unterstützt:** Azure Stack unterstützt keine AAAA-Einträge, weil Azure Stack keine Unterstützung von IPv6-Adressen aufweist. Dies ist ein wichtiger Unterschied zwischen DNS in Azure und Azure Stack.

* **Nicht mehrinstanzenfähig:** Der DNS-Dienst in Azure Stack ist nicht mehrinstanzenfähig. Es ist also nicht möglich, dass für jeden Mandanten die gleiche DNS-Zone erstellt wird. Dies ist nur für das erste Abonnement erfolgreich, mit dem versucht wird, die Zone zu erstellen. Nachfolgende Anforderungen schlagen fehl. Dies ist ein wichtiger Unterschied zwischen DNS in Azure und Azure Stack.

* **Tags, Metadaten und ETags:** Außerdem gibt es kleinere Unterschiede in Bezug darauf, wie Azure Stack Tags, Metadaten, ETags und Grenzwerte verarbeitet.

Weitere Informationen zu Azure DNS finden Sie unter [DNS-Zonen und -Einträge](../../dns/dns-zones-records.md).

### <a name="tags"></a>Tags

Das Azure Stack-DNS unterstützt die Verwendung von Azure Resource Manager-Tags in DNS-Zonenressourcen. Tags für DNS-Ressourceneintragssätze werden nicht unterstützt. Als Alternative werden jedoch **Metadaten** für DNS-Ressourceneintragssätze unterstützt, wie im nächsten Abschnitt beschrieben.

### <a name="metadata"></a>Metadaten

Das Azure Stack-DNS unterstützt das Hinzufügen von Ressourceneintragssätzen mithilfe von *Metadaten* als Alternative zu Tags in Ressourceneintragssätzen. Ähnlich wie Tags ermöglichen Metadaten das Zuordnen von Name-Wert-Paare zu jedem Ressourceneintragssatz. Dies kann beispielsweise hilfreich sein, um den Zweck jedes Ressourceneintragssatzes zu dokumentieren. Anders als Tags können Metadaten nicht verwendet werden, um eine gefilterte Ansicht Ihrer Azure-Rechnung bereitzustellen, und Metadaten können nicht in einer Azure Resource Manager-Richtlinie angegeben werden.

### <a name="etags"></a>Etags

Angenommen, zwei Personen oder zwei Prozesse versuchen, einen DNS-Eintrag zur gleichen Zeit zu ändern. Welcher hat Vorrang? Und weiß die Person, die Vorrang erhalten hat, dass sie die Änderungen einer anderen Person überschrieben hat?

Das Azure Stack-DNS verwendet *ETags*, um gleichzeitige Änderungen an derselben Ressource sicher verarbeiten zu können. ETags unterscheiden sich von Azure Resource Manager-*Tags*. Jeder DNS-Ressourcen (Zone oder Datensatzgruppe) ist ein ETag zugeordnet. Wenn eine Ressource abgerufen wird, wird auch ihr ETag abgerufen. Beim Aktualisieren einer Ressource können Sie auswählen, dass die ETags wieder zurückgegeben werden sollen, damit das Azure Stack-DNS überprüfen kann, ob das ETag auf dem Server übereinstimmt. Da jedes Update einer Ressource dazu führt, dass das ETag erneut generiert wird, weist eine Nichtübereinstimmung des ETags darauf hin, dass eine gleichzeitige Änderung vorgenommen wurde. ETags können auch beim Erstellen einer neuen Ressource verwendet werden, um sicherzustellen, dass die Ressource nicht bereits vorhanden ist.

Standardmäßig verwenden die PowerShell-Cmdlets des Azure Stack-DNS ETags, um gleichzeitige Änderungen an Zonen und Ressourceneintragssätzen zu blockieren. Mit dem optionalen Switch `-Overwrite` können ETag-Überprüfungen unterdrückt werden. In diesem Fall werden gleichzeitig vorgenommene Änderungen überschrieben.

Auf Ebene der REST-API des Azure Stack-DNS werden ETags mithilfe von HTTP-Headern angegeben. Ihr Verhalten wird in der folgenden Tabelle beschrieben:

| Header | Verhalten|
|--------|---------|
| Keine   | PUT ist immer erfolgreich (keine Etag-Prüfung)|
| If-match| PUT ist nur erfolgreich, wenn die Ressource vorhanden ist und das Etag übereinstimmt.|
| If-match *| PUT ist nur erfolgreich, wenn eine Ressource vorhanden ist.|
| If-none-match *| PUT ist nur erfolgreich, wenn die Ressource nicht vorhanden ist.|

### <a name="limits"></a>Einschränkungen

Bei Verwendung des Azure Stack-DNS gelten folgende Standardgrenzwerte:

| Ressource| Standardlimit|
|---------|--------------|
| Zonen pro Abonnement| 100|
| Datensätze pro Zone| 5.000|
| Datensätze pro Datensatzgruppe| 20|

## <a name="next-steps"></a>Nächste Schritte

- [Introducing iDNS for Azure Stack](azure-stack-understanding-dns.md) (Einführung in iDNS für Azure Stack)
