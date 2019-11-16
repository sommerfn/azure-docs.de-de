---
title: Red Hat Enterprise Linux-Images in Azure
description: Erfahren Sie etwas über Red Hat Enterprise Linux-Images in Microsoft Azure.
services: virtual-machines-linux
documentationcenter: ''
author: asinn826
manager: BorisB2015
editor: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 8/14/2019
ms.author: borisb
ms.openlocfilehash: eaabe9da20c22dd3e4d924887adcbc7081857e91
ms.sourcegitcommit: 49cf9786d3134517727ff1e656c4d8531bbbd332
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2019
ms.locfileid: "74035113"
---
# <a name="red-hat-enterprise-linux-images-in-azure"></a>Red Hat Enterprise Linux-Images in Azure
Dieser Artikel beschreibt die verfügbaren Red Hat Enterprise Linux-Images (RHEL) im Azure Marketplace sowie Richtlinien für die Benennung und Aufbewahrung.

Informationen zu Red Hat-Supportrichtlinien für alle RHEL-Versionen finden Sie auf der Seite [Red Hat Enterprise Linux Life Cycle (Red Hat Enterprise Linux-Lebenszyklus)](https://access.redhat.com/support/policy/updates/errata).

>[!Important]
> Für RHEL-Images, die derzeit auf dem Azure-Marketplace verfügbar sind, wird entweder das Lizenzierungsmodell Bring-Your-Own-Subscription (BYOS) oder die nutzungsbasierte Zahlung (Pay-As-You-Go, PAYG) unterstützt. Der [Azure-Vorteil bei Hybridnutzung](../windows/hybrid-use-benefit-licensing.md) und das dynamische Wechseln zwischen BYOS und PAYG wird nicht unterstützt. Für einen Wechsel des Lizenzierungsmodus ist eine erneute Bereitstellung der VM über das entsprechende Image erforderlich.

>[!Note]
> Erstellen Sie ein Supportticket bei Microsoft, wenn bei Ihnen im Azure Marketplace-Katalog ein Problem mit RHEL-Images auftritt.

## <a name="images-available-in-the-ui"></a>Auf der Benutzeroberfläche verfügbare Images
Wenn Sie im Marketplace nach „Red Hat“ suchen oder eine Ressource auf der Benutzeroberfläche des Azure-Portals erstellen, werden eine Teilmenge der verfügbaren RHEL-Images und verwandte Red Hat-Produkte angezeigt. Sie können immer den vollständigen Satz der verfügbaren VM-Images mithilfe der Azure-Befehlszeilenschnittstelle, von PowerShell oder der API abrufen.

Führen Sie den folgenden Befehl aus, um den vollständigen Satz von verfügbaren Red Hat-Images in Azure anzuzeigen.

```azurecli-interactive
az vm image list --publisher RedHat --all
```

## <a name="naming-convention"></a>Benennungskonvention
VM-Images in Azure sind nach Herausgeber, Angebot, SKU und Version organisiert. Die Kombination aus Herausgeber:Angebot:SKU:Version ist der Image-URN, durch den das zu verwendende Image eindeutig identifiziert wird.

Beispielsweise verweist `RedHat:RHEL:7-RAW:7.6.2018103108` auf ein RHEL 7.6-Image mit Rohpartitionierung, das am 31. Oktober 2018 erstellt wurde.

Ein Beispiel zum Erstellen einer RHEL 7.6-VM ist weiter unten dargestellt.
```azurecli-interactive
az vm create --name RhelVM --resource-group TestRG --image RedHat:RHEL:7-RAW:7.6.2018103108 --no-wait
```

### <a name="the-latest-moniker"></a>Der Moniker „latest“
Die Azure-REST-API ermöglicht die Verwendung des Monikers „latest“ für die Version anstelle der genauen Version. Mit „latest“ wird das neueste verfügbare Image für die angegebene Kombination aus Herausgeber, Angebot und SKU bereitgestellt.

Beispielsweise verweist `RedHat:RHEL:7-RAW:latest` auf das neueste verfügbare Image der RHEL 7 Familie mit Rohpartitionierung.

```azurecli-interactive
az vm create --name RhelVM --resource-group TestRG --image RedHat:RHEL:7-RAW:latest --no-wait
```

>[!NOTE]
> Im Allgemeinen folgt der Vergleich von Versionen zum Ermitteln der neuesten Version den Regeln der [CompareTo](https://msdn.microsoft.com/library/a5ts8tb6.aspx)-Methode.

### <a name="current-naming-convention"></a>Aktuelle Namenskonvention
Alle derzeit veröffentlichten RHEL-Images verwenden das Modell mit nutzungsbasierter Bezahlung und sind mit der [Red Hat-Updateinfrastruktur (RHUI) in Azure](https://aka.ms/rhui-update) verbunden. Für die Imagefamilie RHEL 7 wurde eine neue Namenskonvention übernommen, bei der das Partitionsschema für Datenträger (RAW, LVM) in der SKU angegeben wird und nicht in der Version. Die Version des RHEL-Images enthält entweder 7-RAW oder 7-LVM. Die Benennung der RHEL 6-Familie wurde zu diesem Zeitpunkt noch nicht geändert.

Diese Namenskonvention umfasst zwei Typen von SKUs für RHEL 7-Images: SKUs, die die Nebenversion auflisten, und SKUs, die diese nicht aufführen. Wenn Sie eine 7-RAW- oder 7 LVM-SKU verwenden möchten, können Sie die RHEL-Nebenversion angeben, die Sie in der Version bereitstellen möchten. Wenn Sie die aktuellste Version auswählen, wird die neueste Nebenversion von RHEL bereitgestellt.

>[!NOTE]
> In dem Satz von RHEL for SAP-Images ist die RHEL-Version festgeschrieben. Daher enthält ihre Namenskonvention eine bestimmte Version in der SKU.

>[!NOTE]
> Der Satz von RHEL 6-Images wurde nicht in die neue Namenskonvention umgewandelt.

## <a name="extended-update-support-eus"></a>Extended Update Support (EUS)
Seit April 2019 stehen RHEL-Images zur Verfügung, die standardmäßig an die Extended Update Support-Repositorys (EUS) angefügt sind. Weitere Informationen zum Extended Update Support von RHEL finden Sie in der [Red Hat-Dokumentation](https://access.redhat.com/articles/rhel-eus).

Anweisungen zum Umstellen Ihrer virtuellen Computer auf EUS und weitere Details zum Ende von EUS finden Sie [hier](https://aka.ms/rhui-update#rhel-eus-and-version-locking-rhel-vms).

>[!NOTE]
> EUS wird für RHEL Extras nicht unterstützt. Wenn Sie also ein Paket installieren, das in der Regel über den RHEL Extras-Kanal verfügbar ist, können Sie dies nicht installieren, solange Sie EUS nutzen. Informationen zum Produktlebenszyklus von Red Hat Extras finden Sie [hier](https://access.redhat.com/support/policy/updates/extras/).

### <a name="for-customers-that-want-to-use-eus-images"></a>Kunden, die EUS-Images verwenden möchten:
Kunden, die Images verwenden möchten, die EUS-Repositorys zugeordnet sind, sollten das RHEL-Image verwenden, das in der SKU eine RHEL-Nebenversionsnummer enthält. Diese Images enthalten RAW-Partitionen (nicht LVM).

Beispielsweise könnten die folgenden beiden RHEL 7.4-Images verfügbar sein:
```bash
RedHat:RHEL:7-RAW:7.4.2018010506
RedHat:RHEL:7.4:7.4.2019041718
```
In diesem Fall ist `RedHat:RHEL:7.4:7.4.2019041718` standardmäßig an EUS-Repositorys angefügt und `RedHat:RHEL:7-RAW:7.4.2018010506` an Nicht-EUS-Repositorys.

### <a name="for-customers-that-dont-want-to-use-eus-images"></a>Kunden, die EUS-Images nicht verwenden möchten:
Wenn Sie kein Image verwenden möchten, das standardmäßig mit EUS verknüpft ist, verwenden Sie für die Bereitstellung ein Image, das keine Nebenversionsnummer in der SKU enthält.

#### <a name="rhel-images-with-eus"></a>RHEL-Images mit EUS
Die folgende Tabelle gilt für RHEL-Images, die eine Nebenversion in der SKU enthalten.

>[!NOTE]
> Zum Zeitpunkt des Verfassens verfügen nur RHEL 7.4 und spätere Nebenversionen über EUS-Unterstützung. EUS wird für RHEL-Versionen bis einschließlich 7.3 nicht mehr unterstützt.
>
> Weitere Einzelheiten zur EUS-Verfügbarkeit für RHEL finden Sie [hier](https://access.redhat.com/support/policy/updates/errata).

Nebenversion |EUS-Beispielimage              |EUS-Status                                                   |
:-------------|:------------------------------|:------------------------------------------------------------|
RHEL 7.4      |RedHat:RHEL:7.4:7.4.2019041718 | Images, die ab April 2019 veröffentlicht werden, nutzen EUS standardmäßig|
RHEL 7.5      |RedHat:RHEL:7.5:7.5.2019060305 | Images, die ab Juni 2019 veröffentlicht werden, nutzen EUS standardmäßig |
RHEL 7.6      |RedHat:RHEL:7.6:7.6.2019052206 | Images, die ab Mai 2019 veröffentlicht werden, nutzen EUS standardmäßig  |
RHEL 8.0      |–                            | Kein EUS in Red Hat verfügbar                               |


## <a name="list-of-rhel-images-available"></a>Liste der verfügbaren RHEL-Images
Die folgenden Angebote sind SKUs, die derzeit zur allgemeinen Verwendung verfügbar sind:

Angebot| SKU | Partitionierung | Bereitstellung | Notizen
:----|:----|:-------------|:-------------|:-----
RHEL          | 7-RAW    | RAW    | Linux-Agent | RHEL 7.x-Imagefamilie. <br> Standardmäßig nicht mit EUS-Repositorys verknüpft
|             | 7-LVM    | LVM    | Linux-Agent | RHEL 7.x-Imagefamilie. <br> Standardmäßig nicht mit EUS-Repositorys verknüpft
|             | 7-RAW-CI | RAW-CI | cloud-init  | RHEL 7.x-Imagefamilie. <br> Standardmäßig nicht mit EUS-Repositorys verknüpft
|             | 6.7      | RAW    | Linux-Agent |
|             | 6,8      | RAW    | Linux-Agent |
|             | 6.9      | RAW    | Linux-Agent |
|             | 6.10     | RAW    | Linux-Agent |
|             | 7.2      | RAW    | Linux-Agent |
|             | 7.3      | RAW    | Linux-Agent |
|             | 7.4      | RAW    | Linux-Agent | Ab April 2019 standardmäßig mit EUS-Repositorys verknüpft.
|             | 7,5      | RAW    | Linux-Agent | Ab Juni 2019 standardmäßig mit EUS-Repositorys verknüpft.
|             | 7.6      | RAW    | Linux-Agent | Ab Mai 2019 standardmäßig mit EUS-Repositorys verknüpft.
|             | 7,7      | LVM    | Linux-Agent | Standardmäßig mit EUS-Repositorys verknüpft.
RHEL-SAP      | 7.4      | LVM    | Linux-Agent | RHEL 7.4 for SAP HANA und Business Applications
|             | 7,5      | LVM    | Linux-Agent | RHEL 7.5 for SAP HANA und Business Applications
RHEL-SAP-HANA | 6.7      | RAW    | Linux-Agent | RHEL 6.7 for SAP HANA
|             | 7.2      | LVM    | Linux-Agent | RHEL 7.2 for SAP HANA
|             | 7.3      | LVM    | Linux-Agent | RHEL 7.3 for SAP HANA
RHEL-SAP-APPS | 6,8      | RAW    | Linux-Agent | RHEL 6.8 for SAP Business Applications
|             | 7.3      | LVM    | Linux-Agent | RHEL 7.3 for SAP Business Applications
RHEL-Hochverfügbarkeit       | 7.4      | LVM    | Linux-Agent | RHEL 7.4 mit Hochverfügbarkeits-Add-On
|             | 7,5      | LVM    | Linux-Agent | RHEL 7.5 mit Hochverfügbarkeits-Add-On
|             | 7.6      | LVM    | Linux-Agent | RHEL 7.6 mit Hochverfügbarkeits-Add-On
RHEL-SAP-HA   | 7.4      | LVM    | Linux-Agent | RHEL 7.4 für SAP mit Hochverfügbarkeits-Add-On
|             | 7,5      | LVM    | Linux-Agent | RHEL 7.5 für SAP mit Hochverfügbarkeits-Add-On
|             | 7.6      | LVM    | Linux-Agent | RHEL 7.6 für SAP mit Hochverfügbarkeits-Add-On

### <a name="old-naming-convention"></a>Alte Namenskonvention
In der RHEL 7-Familie von Images und der RHEL 6-Familie von Images wurden bis zur oben erläuterten Änderung der Namenskonvention spezifische Versionen in den SKUs verwendet.

In der vollständigen Liste der Images sind numerische SKUs aufgeführt. Microsoft und Red Hat erstellen neue numerische SKUs, wenn eine neue Nebenversion veröffentlicht wird.

### <a name="other-available-offers-and-skus"></a>Weitere verfügbare Angebote und SKUs
Die vollständige Liste der verfügbaren Angebote und SKUs enthält eventuell weitere Images über die in der obigen Tabelle aufgeführten hinaus, z.B. `RedHat:rhel-ocp-marketplace:rhel74:7.4.1`. Diese Angebote können zur Unterstützung bestimmter Marketplace-Lösungen verwendet werden, oder sie können für Vorschauversionen und zu Testzwecken veröffentlicht werden. Sie können jederzeit ohne vorherige Warnung geändert oder entfernt werden. Verwenden Sie sie nicht, sofern ihr Vorhandensein nicht öffentlich von Microsoft oder Red Hat dokumentiert wurde.

## <a name="publishing-policy"></a>Veröffentlichungsrichtlinie
Microsoft und Red Hat aktualisieren Images, wenn neue Nebenversionen veröffentlicht werden, nach Bedarf für bestimmte CVEs oder gelegentlich bei Konfigurationsänderungen/-updates. Wir bemühen uns, die aktualisierten Abbilder möglichst bald bereitzustellen – binnen drei Werktagen nach der Veröffentlichung bzw. Verfügbarkeit eines CVE-Updates.

Wir aktualisieren nur die aktuelle Nebenversion in einer bestimmten Imagefamilie. Wenn eine neuere Nebenversion veröffentlicht wird, beenden wir die Aktualisierung älterer Nebenversionen. Beispielsweise werden mit der Veröffentlichung von RHEL 7.6 keine RHEL 7.5-Images mehr aktualisiert.

>[!NOTE]
> Aktive Azure-VMs, die mit RHEL-Images mit nutzungsbasierter Bezahlung bereitgestellt wurden, sind mit der Azure-RHUI verbunden und erhalten Updates und Korrekturen, sobald sie von Red Hat veröffentlicht und in die Azure-RHUI repliziert wurden (normalerweise in weniger als 24 Stunden nach der offiziellen Veröffentlichung durch Red Hat). Für diese VMs ist kein neues veröffentlichte Image erforderlich, um die Updates zu erhalten, und Kunden haben vollständige Kontrolle über den Zeitpunkt des Updates.

## <a name="image-retention-policy"></a>Aufbewahrungsrichtlinie für Images
Nach unserer aktuellen Richtlinie werden alle veröffentlichten Images beibehalten. Wir behalten uns vor, Images zu entfernen, die bekanntermaßen Probleme jeglicher Art verursachen. Beispielsweise können Images mit falschen Konfigurationen aufgrund späterer Plattform- oder Komponentenaktualisierungen entfernt werden. Falls ein Image entfernt wird, gilt die aktuelle Marketplace-Richtlinie, nach der bis zu 30 Tage vor dem Entfernen des Images Benachrichtigungen bereitgestellt werden sollten.

## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie [hier](https://aka.ms/rhui-update) mehr über die Azure Red Hat-Updateinfrastruktur.
* Informationen zu Red Hat-Supportrichtlinien für alle RHEL-Versionen finden Sie auf der Seite [Red Hat Enterprise Linux Life Cycle (Red Hat Enterprise Linux-Lebenszyklus)](https://access.redhat.com/support/policy/updates/errata).
