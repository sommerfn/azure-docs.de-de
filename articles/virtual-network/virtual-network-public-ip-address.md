---
title: Erstellen, Ändern oder Löschen einer öffentlichen Azure-IP-Adresse | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine öffentliche IP-Adresse erstellen, ändern oder löschen können.
services: virtual-network
documentationcenter: na
author: KumudD
manager: twooley
editor: ''
tags: azure-resource-manager
ms.assetid: bb71abaf-b2d9-4147-b607-38067a10caf6
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/06/2019
ms.author: kumud
ms.openlocfilehash: 5e71f30723826a18f10d4c8e1b9d34e356c9962b
ms.sourcegitcommit: 598c5a280a002036b1a76aa6712f79d30110b98d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2019
ms.locfileid: "74114627"
---
# <a name="create-change-or-delete-a-public-ip-address"></a>Erstellen, Ändern oder Löschen einer öffentlichen IP-Adresse

Sie erhalten Informationen über öffentliche IP-Adressen und darüber, wie Sie diese erstellen, ändern und löschen können. Eine öffentliche IP-Adresse ist eine Ressource mit eigenen konfigurierbaren Einstellungen. Das Zuweisen einer öffentlichen IP-Adresse an eine Azure-Ressource, die öffentliche IP-Adressen unterstützt, ermöglicht Folgendes:
- Eingehende Kommunikation aus dem Internet an die Ressource, z.B. Azure Virtual Machines (VM), Azure Application Gateways, Azure Load Balancers, Azure VPN Gateways und weitere. Sie können mit einigen Ressourcen wie z.B. virtuellen Computern aus dem Internet auch dann kommunizieren, wenn einem virtuellem Computer keine öffentliche IP-Adresse zugewiesen ist, sofern der virtuelle Computer Teil eines Lastenausgleich-Back-End-Pools und dem Lastenausgleich eine öffentliche IP-Adresse zugewiesen ist. Ob einer Ressource für einen bestimmten Azure-Dienst eine öffentliche IP-Adresse zugewiesen werden kann, oder ob über die öffentliche IP-Adresse einer anderen Azure-Ressource damit kommuniziert werden kann, erfahren Sie in der Dokumentation für den jeweiligen Dienst.
- Ausgehende Konnektivität mit dem Internet über eine vorhersagbare IP-Adresse. Beispielsweise kann ein virtueller Computer, obwohl ihm keine öffentliche IP-Adresse zugewiesen ist, ausgehend mit dem Internet kommunizieren, weil seine Netzwerkadresse von Azure standardmäßig in eine nicht vorhersagbare öffentliche Adresse übersetzt wird. Indem Sie Ressourcen eine öffentliche IP-Adresse zuweisen, wissen Sie, welche IP-Adresse für die ausgehende Verbindung verwendet wird. Eine solche Adresse ist zwar vorhersehbar, kann sich aber ändern – je nach ausgewählter Zuweisungsmethode. Weitere Informationen finden Sie unter [Erstellen einer öffentlichen IP-Adresse](#create-a-public-ip-address). Weitere Informationen zu ausgehenden Verbindungen von Azure-Ressourcen finden Sie unter [Grundlegendes zu ausgehenden Verbindungen in Azure](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

## <a name="before-you-begin"></a>Voraussetzungen

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Führen Sie zuerst die folgenden Aufgaben aus, ehe Sie die Schritte in den Abschnitten dieses Artikels durchführen:

- Falls Sie noch nicht über ein Azure-Konto verfügen, können Sie sich für ein [kostenloses Testkonto](https://azure.microsoft.com/free) registrieren.
- Öffnen Sie bei Verwendung des Portals https://portal.azure.com, und melden Sie sich mit Ihrem Azure-Konto an.
- Wenn Sie PowerShell-Befehle zum Durchführen von Aufgaben in diesem Artikel verwenden, führen Sie die Befehle entweder in [Azure Cloud Shell](https://shell.azure.com/powershell) oder durch Ausführen von PowerShell auf Ihrem Computer aus. Azure Cloud Shell ist eine kostenlose interaktive Shell, mit der Sie die Schritte in diesem Artikel ausführen können. Sie verfügt über allgemeine vorinstallierte Tools und ist für die Verwendung mit Ihrem Konto konfiguriert. Für dieses Tutorial ist das Azure PowerShell-Modul Version 1.0.0 oder höher erforderlich. Führen Sie `Get-Module -ListAvailable Az` aus, um die installierte Version zu ermitteln. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-az-ps) Informationen dazu. Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Connect-AzAccount` ausführen, um eine Verbindung mit Azure herzustellen.
- Wenn Sie Befehle der Azure-Befehlszeilenschnittstelle (CLI) zum Durchführen von Aufgaben in diesem Artikel verwenden, führen Sie die Befehle entweder in [Azure Cloud Shell](https://shell.azure.com/bash) oder durch Ausführen der CLI auf Ihrem Computer aus. Für dieses Tutorial ist die Azure CLI-Version 2.0.31 oder höher erforderlich. Führen Sie `az --version` aus, um die installierte Version zu ermitteln. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sei bei Bedarf unter [Installieren der Azure CLI](/cli/azure/install-azure-cli). Wenn Sie die Azure CLI lokal ausführen, müssen Sie auch `az login` ausführen, um eine Verbindung mit Azure herzustellen.

Das Konto, bei dem Sie sich anmelden oder das Sie zum Herstellen einer Verbindung mit Azure verwenden, muss der Rolle [Netzwerkmitwirkender](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) oder einer [benutzerdefinierten Rolle](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) zugewiesen sein, der die entsprechenden, unter [Berechtigungen](#permissions) aufgeführten Aktionen zugewiesen wurden.

Für öffentliche IP-Adressen fällt eine Schutzgebühr an. Informationen zu den Preisen finden Sie auf der Seite [Preise für IP-Adressen](https://azure.microsoft.com/pricing/details/ip-addresses).

## <a name="create-a-public-ip-address"></a>Erstellen einer öffentlichen IP-Adresse

1. Wählen Sie in der linken oberen Ecke des Portals die Option **+ Ressource erstellen** aus.
2. Geben Sie im Feld *Marketplace durchsuchen* den Begriff *öffentliche IP-Adresse* ein. Wenn **Öffentliche IP-Adresse** in den Suchergebnissen angezeigt wird, klicken Sie darauf.
3. Klicken Sie unter **Öffentliche IP-Adresse** auf **Erstellen**.
4. Geben Sie unter **Öffentliche IP-Adresse erstellen** Werte für folgende Einstellungen ein, oder wählen Sie Werte aus, und klicken Sie dann auf **Erstellen**:

   |Einstellung|Erforderlich?|Details|
   |---|---|---|
   |NAME|Ja|Der Name muss innerhalb der ausgewählten Ressourcengruppe eindeutig sein.|
   |SKU|Ja|Alle öffentlichen IP-Adressen, die vor der Einführung von SKUs erstellt wurden, sind öffentliche IP-Adressen für eine **Basic**-SKU. Sie können die SKU nicht ändern, nachdem die öffentliche IP-Adresse erstellt wurde. Ein eigenständiger virtueller Computer, virtuelle Computer innerhalb einer Verfügbarkeitsgruppe oder VM-Skalierungsgruppen können Basic- oder Standard-SKUs verwenden. Das Mischen von SKUs zwischen virtuellen Computern in Verfügbarkeitsgruppen oder Skalierungsgruppen ist nicht zulässig. **Basic**-SKU: Wenn Sie eine öffentliche IP-Adresse in einer Region erstellen, die Verfügbarkeitszonen unterstützt, wird die Einstellung **Verfügbarkeitszone** standardmäßig auf *Keine* festgelegt. Öffentliche IP-Adressen des Typs „Basic“ unterstützen keine Verfügbarkeitszonen. **Standard**-SKU: Eine öffentliche IP-Adresse für eine Standard-SKU kann dem Front-End eines virtuellen Computers oder Load Balancers zugeordnet werden. Wenn Sie eine öffentliche IP-Adresse in einer Region erstellen, die Verfügbarkeitszonen unterstützt, wird die Einstellung **Verfügbarkeitszone** standardmäßig auf *Zonenredundant* festgelegt. Weitere Informationen zu Verfügbarkeitszonen finden Sie im Abschnitt **Verfügbarkeitszone**. Die Standard-SKU ist erforderlich, wenn Sie die Adresse einem Standard-Load Balancer zuordnen. Weitere Informationen zum Standard-Load Balancer finden Sie unter [Azure Load Balancer Standard overview](../load-balancer/load-balancer-standard-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (Übersicht über den Standard-Azure Load Balancer, in englischer Sprache). Wenn Sie der Netzwerkschnittstelle eines virtuellen Computers eine öffentliche IP-Adresse für eine Standard-SKU zuweisen, müssen Sie den geplanten Datenverkehr explizit mit einer [Netzwerksicherheitsgruppe](security-overview.md#network-security-groups) zulassen. Die Kommunikation mit der Ressource schlägt fehl, bis Sie eine Netzwerksicherheitsgruppe erstellen und zuordnen und den gewünschten Datenverkehr explizit zulassen.|
   |IP-Version|Ja| Wählen Sie IPv4 oder IPv6 aus. Öffentliche IPv4-Adressen können verschiedenen Azure-Ressourcen zugewiesen werden, eine öffentliche IPv6-Adresse dagegen kann nur einem Load Balancer mit Internetzugriff zugewiesen werden. Der Load Balancer kann den Lastenausgleich für den IPv6-Datenverkehr zu virtuellen Azure-Computern durchführen. Hier erfahren Sie mehr über den [Lastenausgleich des IPv6-Datenverkehrs zu virtuellen Computern](../load-balancer/load-balancer-ipv6-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Wenn Sie die **Standard-SKU** ausgewählt haben, ist *IPv6* nicht zur Auswahl verfügbar. Bei Verwendung der **Standard-SKU** können Sie nur eine IPv4-Adresse erstellen.|
   |IP-Adresszuweisung|Ja|**Dynamisch:** Dynamische Adressen werden nur zugewiesen, nachdem einer Azure-Ressource eine öffentliche IP-Adresse zugeordnet und die Ressource erstmalig gestartet wurde. Dynamische Adressen können sich ändern, wenn sie einer Ressource zugewiesen sind, z. B. einem virtuellen Computer, und der virtuelle Computer wird beendet (seine Zuordnung aufgehoben) und dann neu gestartet. Die Adresse bleibt unverändert, wenn ein virtueller Computer neu gestartet oder (ohne Aufheben der Zuordnung) beendet wird. Dynamische Adressen werden freigegeben, wenn eine öffentliche IP-Adressressource von einer Ressource getrennt wird, der sie zugeordnet ist. **Statisch:** Statische Adressen werden zugewiesen, wenn eine öffentliche IP-Adresse erstellt wird. Statische Adressen werden erst freigegeben, wenn eine öffentliche IP-Adressressource gelöscht wird. Wenn die Adresse keiner Ressource zugeordnet ist, können Sie die Zuweisungsmethode ändern, nachdem die Adresse erstellt wurde. Wenn die Adresse einer Ressource zugeordnet ist, können Sie die Zuweisungsmethode eventuell nicht ändern. Wenn Sie *IPv6* als **IP-Version** auswählen, lautet die Zuweisungsmethode *Dynamisch*. Wenn Sie *Standard* für **SKU** auswählen, lautet die Zuweisungsmethode *Statisch*.|
   |Leerlaufzeitüberschreitung (Minuten)|Nein|Gibt an, wie viele Minuten eine TCP- oder HTTP-Verbindung geöffnet bleiben soll, ohne dass Clients Keep-Alive-Meldungen senden müssen. Wenn Sie IPv6 als **IP-Version** auswählen, kann dieser Wert nicht geändert werden. |
   |DNS-Namensbezeichnung|Nein|Muss in dem Azure-Standort, in dem Sie den Namen erstellen, eindeutig sein (über alle Abonnements und Kunden hinweg). Azure registriert den Namen und die IP-Adresse automatisch im DNS, sodass Sie über den Namen eine Verbindung mit der Ressource herstellen können. Azure fügt ein Standardsubnetz wie etwa *location.cloudapp.azure.com* (wobei „location“ der Standort ist, den Sie auswählen) an den von Ihnen bereitgestellten Namen an, um den vollqualifizierten DNS-Namen zu erstellen. Wenn Sie beide Adressversionen für die Erstellung ausgewählt haben, wird sowohl der IPv4- als auch der IPv6-Adresse der gleiche DNS-Name zugewiesen. Das Standard-DNS von Azure enthält IPv4-A- und IPv6-AAAA-Namenseinträge und antwortet beim Nachschlagen des DNS-Namens mit beiden Einträgen. Der Client wählt die Adresse (IPv4 oder IPv6) für die Kommunikation aus. Sie können anstelle der oder zusätzlich zur DNS-Namensbezeichnung mit dem Standardsuffix den Azure DNS-Dienst verwenden, um einen DNS-Namen mit einem benutzerdefinierten Suffix zu konfigurieren, das in die öffentliche IP-Adresse aufgelöst wird. Weitere Informationen finden Sie unter [Bereitstellen von benutzerdefinierten Domäneneinstellungen für einen Azure-Dienst mit Azure DNS](../dns/dns-custom-domain.md?toc=%2fazure%2fvirtual-network%2ftoc.json#public-ip-address).|
   |IPv6-Adresse (oder IPv4-Adresse) erstellen|Nein| Ob IPv6 oder IPv4 angezeigt wird, hängt von Ihrer Auswahl der **IP-Version** ab. Wenn Sie als **IP Version** z.B. **IPv4** ausgewählt haben, wird hier **IPv4** angezeigt. Wenn Sie *Standard* für **SKU** auswählen, haben Sie nicht die Möglichkeit, eine IPv6-Adresse zu erstellen.
   |Name – wird nur angezeigt, wenn Sie das Kontrollkästchen **IPv6-Adresse erstellen** (bzw. „IPv4-Adresse erstellen“) aktiviert haben|Ja, wenn Sie das Kontrollkästchen **IPv6-Adresse erstellen** (bzw. „IPv4-Adresse erstellen“) aktivieren.|Der Name muss sich von dem Namen unterscheiden, den Sie als ersten **Namen** in diese Liste eingeben. Wenn Sie sowohl eine IPv4- als auch eine IPv6-Adresse erstellen, erstellt das Portal zwei separate öffentliche IP-Adressressourcen, wobei jeder dieser Ressourcen eine IP-Adressversion zugewiesen ist.|
   |IP-Adresszuweisung – wird nur angezeigt, wenn Sie das Kontrollkästchen **IPv6-Adresse erstellen** (bzw. „IPv4-Adresse erstellen“) aktiviert haben|Ja, wenn Sie das Kontrollkästchen **IPv6-Adresse erstellen** (bzw. „IPv4-Adresse erstellen“) aktivieren.|Wenn das Kontrollkästchen **IPv4-Adresse erstellen** aktiviert ist, können Sie eine Zuweisungsmethode auswählen. Wenn das Kontrollkästchen **IPv6-Adresse erstellen** aktiviert ist, können Sie keine Zuweisungsmethode auswählen, da die Methode **Dynamisch** lauten muss.|
   |Subscription|Ja|Muss im selben [Abonnement](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) wie die Ressource vorhanden sein, der Sie die öffentliche IP-Adresse zuordnen möchten|
   |Resource group|Ja|Kann in derselben oder in einer anderen [Ressourcengruppe](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group) wie die Ressource vorhanden sein, der Sie die öffentliche IP-Adresse zuordnen möchten|
   |Location|Ja|Muss am selben [Standort](https://azure.microsoft.com/regions), auch als Region bezeichnet, wie die Ressource vorhanden sein, der Sie die öffentliche IP-Adresse zuordnen möchten.|
   |Verfügbarkeitszone| Nein | Diese Einstellung wird nur angezeigt, wenn Sie einen unterstützten Standort auswählen. Eine Liste der unterstützten Standorte finden Sie unter [Übersicht über Verfügbarkeitszonen](../availability-zones/az-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Wenn Sie die **Basic**-SKU ausgewählt haben, wird automatisch *Keine* ausgewählt. Wenn Sie es vorziehen, eine bestimmte Zone zu gewährleisten, können Sie eine bestimmte Zone auswählen. Keine der beiden Optionen ist zonenredundant. Bei Auswahl der **Standard**-SKU: Es wird automatisch die zonenredundante Option ausgewählt, und der Datenpfad wird gegen Zonenausfall stabilisiert. Wenn Sie es vorziehen, eine bestimmte Zone zu gewährleisten, die nicht gegen Zonenausfall stabilisiert ist, können Sie eine bestimmte Zone auswählen.

**Befehle**

Obwohl das Portal die Option bietet, zwei öffentliche IP-Adressen zu erstellen (eine IPv4- und eine IPv6-Adresse), erstellen die folgenden CLI- und PowerShell-Befehle eine Ressource mit einer Adresse für jeweils nur eine der IP-Versionen. Wenn Sie zwei öffentliche IP-Adressressourcen benötigen, müssen Sie den folgenden Befehl zweimal ausführen und bei jeder Ausführung unterschiedliche Namen und Versionen für die öffentlichen IP-Adressressourcen angeben.

|Tool|Get-Help|
|---|---|
|Befehlszeilenschnittstelle (CLI)|[az network public-ip create](/cli/azure/network/public-ip#az-network-public-ip-create)|
|PowerShell|[New-AzPublicIpAddress](/powershell/module/az.network/new-azpublicipaddress)|

## <a name="view-change-settings-for-or-delete-a-public-ip-address"></a>Anzeigen, Ändern von Einstellungen oder Löschen einer öffentlichen IP-Adresse

1. Geben Sie im oberen Bereich des Azure-Portals im Feld mit dem Text *Ressourcen suchen* die Zeichenfolge *öffentliche IP-Adresse* ein. Wenn **Öffentliche IP-Adressen** in den Suchergebnissen angezeigt wird, klicken Sie darauf.
2. Wählen Sie den Namen der öffentlichen IP-Adresse aus, die Sie anzeigen oder aus der Liste löschen oder deren Einstellungen Sie ändern möchten.
3. Führen Sie eine der folgenden Aktionen aus – je nachdem, ob Sie die öffentliche IP-Adresse anzeigen, löschen oder ändern möchten.
   - **Anzeigen**: Im Abschnitt **Übersicht** werden die wichtigsten Einstellungen für die öffentliche IP-Adresse angezeigt, darunter die zugeordnete Netzwerkschnittstelle (sofern die Adresse einer Netzwerkschnittstelle zugeordnet ist). Das Portal zeigt die Adressversion nicht an (IPv4 oder IPv6). Um Informationen zur Version zu erhalten, verwenden Sie den PowerShell- oder CLI-Befehl zum Anzeigen der öffentlichen IP-Adresse. Wenn die IP-Adressversion IPv6 lautet, wird die zugewiesene Adresse weder im Portal noch in PowerShell oder über die Befehlszeilenschnittstelle angezeigt.
   - **Löschen**: Um die öffentliche IP-Adresse zu löschen, wählen Sie im Abschnitt **Übersicht** die Option **Löschen** aus. Ist die Adresse zurzeit einer IP-Konfiguration zugeordnet, kann sie nicht gelöscht werden. Ist die Adresse zurzeit einer Konfiguration zugeordnet, klicken Sie auf **Trennen**, um die Adresse von der IP-Konfiguration zu trennen.
   - **Ändern**: Klicken Sie auf **Konfiguration**. Ändern Sie die Einstellungen anhand der Informationen aus Schritt 4 unter [Erstellen einer öffentlichen IP-Adresse](#create-a-public-ip-address). Möchten Sie die Zuweisung für eine IPv4-Adresse von statisch in dynamisch ändern, müssen Sie die öffentliche IPv4-Adresse zunächst von der IP-Konfiguration trennen, der sie zugeordnet ist. Danach können Sie die Zuweisungsmethode zu „dynamisch“ ändern und auf **Zuordnen** klicken, um die IP-Adresse zur selben IP-Konfiguration oder zu einer anderen Konfiguration zuzuordnen. Sie können die Trennung aber auch beibehalten. Um eine öffentliche IP-Adresse zu trennen, klicken Sie in der **Übersicht** auf **Trennen**.

   >[!WARNING]
   >Wenn Sie die Zuweisungsmethode von statisch in dynamisch ändern, geht die IP-Adresse verloren, die der öffentlichen IP-Adresse zugewiesen war. Während die öffentlichen DNS-Server von Azure eine Zuordnung zwischen statischen oder dynamischen Adressen und jeder DNS-Namensbezeichnung (sofern eine solche definiert ist) verwalten, kann sich eine dynamische IP-Adresse ändern, wenn der virtuelle Computer gestartet wird, nachdem er sich im Status „Beendet (Zuordnung aufgehoben)“ befunden hat. Möchten Sie verhindern, dass sich die Adresse ändert, weisen Sie eine statische IP-Adresse zu.

**Befehle**

|Tool|Get-Help|
|---|---|
|Befehlszeilenschnittstelle (CLI)|[az network public-ip list](/cli/azure/network/public-ip#az-network-public-ip-list) listet die öffentlichen IP-Adressen auf, [az network public-ip show](/cli/azure/network/public-ip#az-network-public-ip-show) zeigt die Einstellungen an, [az network public-ip update](/cli/azure/network/public-ip#az-network-public-ip-update) führt eine Aktualisierung durch, [az network public-ip delete](/cli/azure/network/public-ip#az-network-public-ip-delete) löscht|
|PowerShell|[Get-AzPublicIpAddress](/powershell/module/az.network/get-azpublicipaddress) ruft ein öffentliches IP-Adressenobjekt ab und zeigt dessen Einstellungen an, [Set-AzPublicIpAddress](/powershell/module/az.network/set-azpublicipaddress) aktualisiert die Einstellungen, [Remove-AzPublicIpAddress](/powershell/module/az.network/remove-azpublicipaddress) löscht|

## <a name="assign-a-public-ip-address"></a>Zuweisen einer öffentlichen IP-Adresse

Erfahren Sie, wie Sie eine öffentliche IP-Adresse den folgenden Ressourcen zuweisen:

- Einer [Windows](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-network%2ftoc.json)- oder [Linux](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)-VM (beim Erstellen) oder einem [vorhandenen virtuellen Computer](virtual-network-network-interface-addresses.md#add-ip-addresses)
- [Lastenausgleich mit Internetzugriff](../load-balancer/load-balancer-get-started-internet-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Azure Application Gateway](../application-gateway/application-gateway-create-gateway-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Standort-zu-Standort-Verbindung über ein Azure VPN Gateway](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Azure VM-Skalierungsgruppe](../virtual-machine-scale-sets/virtual-machine-scale-sets-portal-create.md?toc=%2fazure%2fvirtual-network%2ftoc.json)

## <a name="permissions"></a>Berechtigungen

Zum Durchführen von Aufgaben für öffentliche IP-Adressen muss Ihr Konto der Rolle [Netzwerkmitwirkender](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) oder einer [benutzerdefinierten](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) Rolle zugewiesen sein, der die entsprechenden, in der folgenden Tabelle aufgeführten Aktionen zugewiesen wurden:

| Aktion                                                             | NAME                                                           |
| ---------                                                          | -------------                                                  |
| Microsoft.Network/publicIPAddresses/read                           | Lesen einer öffentlichen IP-Adresse                                          |
| Microsoft.Network/publicIPAddresses/write                          | Schreiben oder Aktualisieren einer öffentlichen IP-Adresse                           |
| Microsoft.Network/publicIPAddresses/delete                         | Löschen einer öffentlichen IP-Adresse                                     |
| Microsoft.Network/publicIPAddresses/join/action                    | Zuordnen einer öffentlichen IP-Adresse zu einer Ressource                    |

## <a name="next-steps"></a>Nächste Schritte

- Erstellen einer öffentlichen IP-Adresse mithilfe von [PowerShell](powershell-samples.md)- oder [Azure CLI](cli-samples.md)-Beispielskripts oder mithilfe von Azure [Resource Manager-Vorlagen](template-samples.md)
- Erstellen und Anwenden einer [Azure-Richtlinie](policy-samples.md) für öffentliche IP-Adressen
