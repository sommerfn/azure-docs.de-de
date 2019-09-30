---
title: Häufig gestellte Fragen zu privatem Azure-DNS
description: Häufig gestellte Fragen zu privatem Azure-DNS
services: dns
author: vhorne
ms.service: dns
ms.topic: article
ms.date: 09/20/2019
ms.author: victorh
ms.openlocfilehash: fca7359f9fa54899bb72be3b939e1a1839dbfbd1
ms.sourcegitcommit: 116bc6a75e501b7bba85e750b336f2af4ad29f5a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/20/2019
ms.locfileid: "71155706"
---
# <a name="azure-private-dns-faq"></a>Häufig gestellte Fragen zu privatem Azure-DNS

[!INCLUDE [private-dns-public-preview-notice](../../includes/private-dns-public-preview-notice.md)]

## <a name="does-azure-dns-support-private-domains"></a>Unterstützt Azure DNS „private“ Domänen?

Unterstützung für private Domänen wird mithilfe des Features „Private Azure DNS-Zonen“ unterstützt. Private DNS-Zonen werden verwaltet, indem die gleichen Tools wie für Azure DNS-Zonen mit Internetzugriff verwendet werden. Sie können aber nur innerhalb der von Ihnen angegebenen virtuellen Netzwerke aufgelöst werden. Weitere Informationen finden Sie in der [Übersicht](private-dns-overview.md).

Informationen zu anderen internen DNS-Optionen in Azure finden Sie unter [Namensauflösung für virtuelle Computer und Rolleninstanzen](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).

## <a name="will-azure-dns-private-zones-work-across-azure-regions"></a>Funktioniert Azure DNS Private Zones übergreifend in allen Azure-Regionen?

Ja. Private Zonen werden für die DNS-Auflösung zwischen virtuellen Netzwerken übergreifend für Azure-Regionen unterstützt. Private Zonen funktionieren auch ohne explizites Peering der virtuellen Netzwerke. Alle virtuellen Netzwerke müssen als virtuelle Auflösungsnetzwerke für die private Zone angegeben werden. Sie benötigen unter Umständen ein Peering der virtuellen Netzwerke, damit der TCP/HTTP-Datenverkehr von einer Region zu einer anderen weitergeleitet werden kann.

## <a name="is-connectivity-to-the-internet-from-virtual-networks-required-for-private-zones"></a>Ist für private Zonen eine Verbindung mit dem Internet aus virtuellen Netzwerken erforderlich?

Nein. Private Zonen funktionieren mit virtuellen Netzwerken. Sie verwenden diese, um Domänen für virtuelle Computer oder andere Ressourcen in virtuellen Netzwerken (auch übergreifend) zu verwalten. Für die Namensauflösung ist keine Internetverbindung erforderlich.

## <a name="can-the-same-private-zone-be-used-for-several-virtual-networks-for-resolution"></a>Kann die gleiche private Zone für mehrere virtuelle Netzwerke zur Auflösung verwendet werden?

Ja. Sie können einer einzelnen privaten Zone bis zu 1.000 virtuelle Netzwerke zuordnen.

## <a name="can-a-virtual-network-that-belongs-to-a-different-subscription-be-added-as-a-linked-virtual-network-to-a-private-zone"></a>Kann ein virtuelles Netzwerk, das zu einem anderen Abonnement gehört, einer privaten Zone als verknüpftes virtuelles Netzwerk hinzugefügt werden?

Ja. Sie müssen über eine Schreibvorgangsberechtigung für die virtuellen Netzwerke und die private DNS-Zone verfügen. Die Schreibberechtigung kann mehreren RBAC-Rollen gewährt werden. Beispielsweise weist die RBAC-Rolle „Mitwirkender von klassischem Netzwerk“ Schreibberechtigungen für virtuelle Netzwerke auf. Weitere Informationen zu RBAC-Rollen finden Sie unter [Rollenbasierte Zugriffssteuerung](../role-based-access-control/overview.md).

## <a name="will-the-automatically-registered-virtual-machine-dns-records-in-a-private-zone-be-automatically-deleted-when-you-delete-the-virtual-machine"></a>Werden die automatisch registrierten DNS-Einträge virtueller Computer in einer privaten Zone automatisch gelöscht, wenn Sie den virtuellen Computer löschen?

Ja. Wenn Sie einen virtuellen Computer in einem verknüpften virtuellen Netzwerk mit aktivierter automatischer Registrierung löschen, werden die registrierten Einträge automatisch gelöscht.

## <a name="can-an-automatically-registered-virtual-machine-record-in-a-private-zone-from-a-linked-virtual-network-be-deleted-manually"></a>Kann ein automatisch registrierter Eintrag für einen virtuellen Computer in einer privaten Zone aus einem verknüpften virtuellen Netzwerk manuell gelöscht werden?

Ja. Sie können die automatisch registrierten DNS-Einträge aber durch einen manuell erstellten DNS-Eintrag in der Zone überschreiben. Dies wird in den folgenden Frage/Antwort-Informationen beschrieben.

## <a name="what-happens-when-i-try-to-manually-create-a-new-dns-record-into-a-private-zone-that-has-the-same-hostname-as-an-automatically-registered-existing-virtual-machine-in-a-linked-virtual-network"></a>Was geschieht bei dem Versuch, einen neuen DNS-Eintrag manuell in einer privaten Zone zu erstellen, die den gleichen Hostnamen wie eine automatisch registrierte vorhandene VM in einem verknüpften virtuellen Netzwerk aufweist?

Sie versuchen, einen neuen DNS-Eintrag manuell in einer privaten Zone zu erstellen, die den gleichen Hostnamen wie eine automatisch registrierte vorhandene VM in einem verknüpften virtuellen Netzwerk aufweist. Wenn Sie dies tun, überschreibt der neue DNS-Eintrag den automatisch registrierten Eintrag für die VM. Wenn Sie versuchen, diesen manuell erstellten DNS-Eintrag wieder aus der Zone zu löschen, ist der Löschvorgang erfolgreich. Die automatische Registrierung wird so lange durchgeführt, wie der virtuelle Computer noch vorhanden ist und über eine zugeordnete private IP-Adresse verfügt. Der DNS-Eintrag wird in der Zone automatisch neu erstellt.

## <a name="what-happens-when-we-unlink-a-linked-virtual-network-from-a-private-zone-will-the-automatically-registered-virtual-machine-records-from-the-virtual-network-be-removed-from-the-zone-too"></a>Was geschieht, wenn ich die Verknüpfung eines verknüpften virtuellen Netzwerks mit einer privaten Zone aufhebe? Werden auch die automatisch registrierten Einträge virtueller Computer des virtuellen Netzwerks aus der Zone entfernt?

Ja. Um die Verknüpfung eines verknüpften virtuellen Netzwerks mit einer privaten Zone aufzuheben, aktualisieren Sie die DNS-Zone, um die zugeordnete Verbindung des virtuellen Netzwerks zu entfernen. Bei diesem Prozess werden die Einträge virtueller Computer, die automatisch registriert wurden, aus der Zone entfernt.

## <a name="what-happens-when-we-delete-a-linked-virtual-network-thats-linked-to-a-private-zone-do-we-have-to-manually-update-the-private-zone-to-unlink-the-virtual-network-as-a-linked-virtual-network-from-the-zone"></a>Was geschieht, wenn ich ein verknüpftes virtuelles Netzwerk lösche, das mit einer privaten Zone verknüpft ist? Muss ich die private Zone manuell aktualisieren, um die Verknüpfung des virtuellen Netzwerks mit der Zone als verknüpftes virtuelles Netzwerk aufzuheben?

Ja. Wenn Sie ein verknüpftes virtuelles Netzwerk löschen, ohne dafür zuerst die Verknüpfung mit einer privaten Zone zu löschen, ist Ihr Löschvorgang erfolgreich. Die Verknüpfung des virtuellen Netzwerks wird, falls vorhanden, für Ihre private Zone aber nicht automatisch aufgehoben. Sie müssen die Verknüpfung des virtuellen Netzwerks mit der privaten Zone manuell aufheben. Aus diesem Grund sollten Sie die Verknüpfung Ihres virtuellen Netzwerks mit Ihrer privaten Zone aufheben, bevor Sie es löschen.

## <a name="will-dns-resolution-by-using-the-default-fqdn-internalcloudappnet-still-work-even-when-a-private-zone-for-example-privatecontosocom-is-linked-to-a-virtual-network"></a>Funktioniert die DNS-Auflösung mithilfe des Standard-FQDN (internal.cloudapp.net) auch, wenn eine private Zone (z.B. „private.contoso.com“) mit einem virtuellen Netzwerk verknüpft ist?

Ja. Für private Zonen werden die DNS-Standardauflösungen nicht ersetzt, indem die von Azure bereitgestellte Zone „internal.cloudapp.net“ genutzt wird. Dies wird als zusätzliches Feature bzw. als Erweiterung angeboten. Verwenden Sie den FQDN der Zone, für die Sie die Auflösung durchführen möchten, unabhängig davon, ob Sie die von Azure bereitgestellte Zone „internal.cloudapp.net“ oder Ihre eigene private Zone nutzen.

## <a name="will-the-dns-suffix-on-virtual-machines-within-a-linked-virtual-network-be-changed-to-that-of-the-private-zone"></a>Wird das DNS-Suffix auf virtuellen Computern in einem verknüpften virtuellen Netzwerk in das Suffix der privaten Zone geändert?

Nein. Es wird weiterhin das von Azure bereitgestellte Standardsuffix (*.internal.cloudapp.net) als DNS-Suffix auf den virtuellen Computern in Ihrem verknüpften virtuellen Netzwerk verwendet. Sie können dieses DNS-Suffix auf Ihren virtuellen Computern manuell in das der privaten Zone ändern.

## <a name="what-are-the-usage-limits-for-azure-private-dns"></a>Welche Nutzungsgrenzwerte gelten für privates Azure-DNS?

Bei Verwendung von privatem Azure-DNS gelten folgende Standardgrenzwerte.

| Resource | Standardlimit |
| --- | --- |
|Private DNS-Zonen pro Abonnement|1000|
|Eintragssätze pro privater DNS-Zone|25.000|
|Datensätze pro Datensatzgruppe|20|
|Virtuelle Netzwerkverbindungen pro privater DNS-Zone|1000|
|Virtuelle Netzwerkverbindungen pro privater DNS-Zone mit aktivierter automatischer Registrierung|100|
|Anzahl von privaten DNS-Zonen, mit denen ein virtuelles Netzwerk mit aktivierter automatischer Registrierung verknüpft werden kann|1|
|Anzahl von privaten DNS-Zonen, mit denen ein virtuelles Netzwerk verknüpft werden kann|1000|

## <a name="is-there-portal-support-for-private-zones"></a>Besteht Portalunterstützung für private Zonen?

Ja. Private Zonen, die bereits über APIs, PowerShell, die CLI und SDKs erstellt werden, werden im Azure-Portal angezeigt.

## <a name="why-dont-my-existing-private-dns-zones-show-up-in-new-portal-experience"></a>Warum werden meine vorhandenen privaten DNS-Zonen nicht in der neuen Portaloberfläche angezeigt?

Als Teil des Vorschauaktualisierungsrelease haben wir ein neues Ressourcenmodell für private DNS-Zonen ausgeliefert. Ihre vorhandenen privaten DNS-Zonen müssen zum neuen Ressourcenmodell migriert werden, damit sie in der neuen Portaloberfläche angezeigt werden können. Nachfolgend finden Sie Anweisungen dazu, wie Sie zum neuen Ressourcenmodell migrieren können.

## <a name="how-do-i-migrate-my-existing-private-dns-zones-to-the-new-model"></a>Wie kann ich meine vorhandenen privaten DNS-Zonen zum neuen Modell migrieren?
Es wird dringend empfohlen, so bald wie möglich zum neuen Ressourcenmodell zu migrieren. Das Legacyressourcenmodell wird zwar unterstützt, weitere Features werden für dieses Modell jedoch nicht mehr entwickelt. In Zukunft möchten wir es zugunsten des neuen Ressourcenmodells als veraltet kennzeichnen. Anleitungen zum Migrieren von vorhandenen privaten DNS-Zonen zum neuen Ressourcenmodell finden Sie unter[Migrationsleitfaden für private Azure DNS-Zonen](private-dns-migration-guide.md).

## <a name="next-steps"></a>Nächste Schritte

- [Weitere Informationen zur privatem Azure-DNS](private-dns-overview.md)