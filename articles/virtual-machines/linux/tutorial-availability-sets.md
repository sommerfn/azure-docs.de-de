---
title: Tutorial – Hochverfügbarkeit für virtuelle Linux-Computer in Azure | Microsoft-Dokumentation
description: In diesem Tutorial erfahren Sie, wie Sie Azure CLI zum Bereitstellen hoch verfügbarer virtueller Computer in Verfügbarkeitsgruppen verwenden.
documentationcenter: ''
services: virtual-machines-linux
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.topic: tutorial
ms.date: 08/24/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 10458e3c5f1e4dc9034206470fdfec19e13417fb
ms.sourcegitcommit: 8b44498b922f7d7d34e4de7189b3ad5a9ba1488b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2019
ms.locfileid: "72299452"
---
# <a name="tutorial-create-and-deploy-highly-available-virtual-machines-with-the-azure-cli"></a>Tutorial: Erstellen und Bereitstellen hochverfügbarer virtueller Computer mit der Azure CLI

In diesem Tutorial erfahren Sie, wie Sie die Verfügbarkeit und Zuverlässigkeit Ihrer Lösungen für virtuelle Computer in Azure mithilfe von sogenannten Verfügbarkeitsgruppen erhöhen. Verfügbarkeitsgruppen sorgen dafür, dass die von Ihnen in Azure bereitgestellten virtuellen Computer auf mehrere isolierte Hardwarecluster verteilt werden. Hierdurch wird sichergestellt, dass sich Hardware- oder Softwarefehler in Azure nur auf einen Teil Ihrer VMs auswirken und die Lösung insgesamt verfügbar und betriebsbereit bleibt.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Verfügbarkeitsgruppe erstellen
> * Erstellen eines virtuellen Computers in einer Verfügbarkeitsgruppe
> * Überprüfen der verfügbaren VM-Größen

Dieses Tutorial verwendet die CLI innerhalb des Diensts [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview), der ständig auf die neueste Version aktualisiert wird. Wählen Sie zum Öffnen von Cloud Shell oben in einem Codeblock die Option **Ausprobieren** aus.

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für dieses Tutorial die Azure CLI-Version 2.0.30 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu finden. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sei bei Bedarf unter [Installieren der Azure CLI]( /cli/azure/install-azure-cli).

## <a name="high-availability-in-azure-overview"></a>Übersicht über Hochverfügbarkeit in Azure
Es gibt zahlreiche verschiedene Möglichkeiten, Hochverfügbarkeit in Azure bereitzustellen. Verfügbarkeitsgruppen und Verfügbarkeitszonen sind zwei Optionen. Bei der Verwendung von Verfügbarkeitsgruppen werden Ihre VMs vor Fehlern geschützt, die in einem Datencenter auftreten können. Dazu zählen Hardwarefehler und Azure-Softwarefehler. Bei der Verwendung von Verfügbarkeitszonen werden Ihre VMs in einer physisch getrennten Infrastruktur ohne gemeinsame Ressourcen platziert und sind somit vor Fehlern im gesamten Datencenter geschützt.

Verwenden Sie Verfügbarkeitsgruppen oder Verfügbarkeitszonen, wenn Sie zuverlässige VM-basierte Lösungen in Azure bereitstellen möchten.

### <a name="availability-set-overview"></a>Übersicht über Verfügbarkeitsgruppen

Eine Verfügbarkeitsgruppe ist eine Funktion zur logischen Gruppierung, mit der Sie in Azure sicherstellen können, dass die darin enthaltenen VM-Ressourcen voneinander isoliert sind, wenn sie in einem Azure-Rechenzentrum bereitgestellt werden. Azure stellt sicher, dass die virtuellen Computer innerhalb einer Verfügbarkeitsgruppe auf mehrere physische Server, Compute-Racks, Speichereinheiten und Netzwerkswitches verteilt werden. Wenn ein Hardware- oder Softwarefehler in Azure auftritt, wird nur ein Teil Ihrer VMs beeinträchtigt, und die Anwendung insgesamt bleibt betriebsbereit und weiterhin für Ihre Kunden verfügbar. Verfügbarkeitsgruppen stellen eine wichtige Funktion für die Erstellung zuverlässiger Cloudlösungen dar.

In einer typischen VM-basierten Lösung gibt es unter Umständen vier Front-End-Webserver und zwei Back-End-VMs, die eine Datenbank hosten. Sie können in Azure zwei Verfügbarkeitsgruppen definieren, bevor Sie Ihre virtuellen Computer bereitstellen: eine Verfügbarkeitsgruppe für die Ebene „Web“ und eine Verfügbarkeitsgruppe für die Ebene „Datenbank“. Bei der Erstellung einer neuen VM können Sie dann die Verfügbarkeitsgruppe als Parameter für den Befehl „az vm create“ angeben, damit Azure automatisch sicherstellt, dass die in der Verfügbarkeitsgruppe erstellten VMs über mehrere physische Hardwareressourcen isoliert werden. Wenn bei der physischen Hardware, auf der Ihre Webserver- oder Datenbankserver-VMs ausgeführt werden, ein Problem auftritt, können Sie darauf vertrauen, dass die anderen Instanzen Ihrer Webserver- und Datenbank-VMs weiterhin einwandfrei ausgeführt werden, da sie sich auf anderer Hardware befinden.

### <a name="availability-zone-overview"></a>Übersicht über Verfügbarkeitszonen

Verfügbarkeitszonen sind ein Hochverfügbarkeitsangebot, das Anwendungen und Daten vor Ausfällen von Rechenzentren schützt. Verfügbarkeitszonen sind eindeutige physische Standorte in einer Azure-Region. Jede Zone besteht aus mindestens einem Rechenzentrum, dessen Stromversorgung, Kühlung und Netzwerkbetrieb unabhängig funktionieren. Zur Gewährleistung der Resilienz sind in allen aktivierten Regionen mindestens drei separate Zonen vorhanden. Die physische Trennung von Verfügbarkeitszonen innerhalb einer Region schützt Anwendungen und Daten vor Ausfällen von Rechenzentren. Zonenredundante Dienste replizieren Ihre Anwendungen und Daten zum Schutz vor einzelnen Fehlerquellen über Verfügbarkeitszonen hinweg. Mit Verfügbarkeitszonen bietet Azure die branchenweit beste Betriebszeit-SLA von 99,99 Prozent für VMs.

Ähnlich wie bei Verfügbarkeitsgruppen gibt es in einer typischen VM-basierten Lösung unter Umständen vier Front-End-Webserver und zwei Back-End-VMs, die eine Datenbank hosten. Ähnlich wie bei Verfügbarkeitsgruppen stellen Sie Ihre VMs in zwei separaten Verfügbarkeitszonen bereit: in einer Verfügbarkeitszone für die Webebene und in einer Verfügbarkeitszone für die Datenbankebene. Wenn Sie eine neue VM erstellen und die Verfügbarkeitsgruppe als Parameter für den Befehl „az vm create“ angeben, stellt Azure automatisch sicher, dass die erstellten VMs über vollständig andere Verfügbarkeitszonen isoliert werden. Wenn bei dem gesamten Datencenter, in dem Ihre Webserver- oder Datenbankserver-VMs ausgeführt werden, ein Problem auftritt, können Sie darauf vertrauen, dass die anderen Instanzen Ihrer Webserver- und Datenbank-VMs weiterhin einwandfrei ausgeführt werden, da sie sich in vollständig separaten Datencentern befinden.

## <a name="create-an-availability-set"></a>Verfügbarkeitsgruppe erstellen

Eine Verfügbarkeitsgruppe können Sie mithilfe von [az vm availability-set create](/cli/azure/vm/availability-set) erstellen. In diesem Beispiel wird die Anzahl von Update- und Fehlerdomänen für die Verfügbarkeitsgruppe *myAvailabilitySet* in der Ressourcengruppe *myResourceGroupAvailability* auf *2* festgelegt.

Erstellen Sie zunächst mit [az group create](/cli/azure/group#az-group-create) eine Ressourcengruppe und anschließend die Verfügbarkeitsgruppe:

```azurecli-interactive
az group create --name myResourceGroupAvailability --location eastus

az vm availability-set create \
    --resource-group myResourceGroupAvailability \
    --name myAvailabilitySet \
    --platform-fault-domain-count 2 \
    --platform-update-domain-count 2
```

Mithilfe von Verfügbarkeitsgruppen können Sie Ressourcen in Fehlerdomänen und Updatedomänen isolieren. Eine **Fehlerdomäne** stellt eine isolierte Sammlung von Server-, Netzwerk- und Speicherressourcen dar. Im vorherigen Beispiel wird die Verfügbarkeitsgruppe auf mindestens zwei Fehlerdomänen verteilt, wenn die virtuellen Computer bereitgestellt werden. Die Verfügbarkeitsgruppe wird auch auf zwei **Updatedomänen** verteilt. Mit zwei Updatedomänen wird sichergestellt, dass die VM-Ressourcen bei Softwareupdates in Azure isoliert sind. Hierdurch wird verhindert, dass die gesamte Software, die auf der VM ausgeführt wird, gleichzeitig aktualisiert wird.


## <a name="create-vms-inside-an-availability-set"></a>Erstellen von virtuellen Computern in einer Verfügbarkeitsgruppe

Virtuelle Computer müssen in der Verfügbarkeitsgruppe erstellt werden, um sicherzustellen, dass sie ordnungsgemäß auf die Hardwarekomponenten verteilt werden. Ein vorhandener virtueller Computer kann einer Verfügbarkeitsgruppe nach seiner Erstellung nicht mehr hinzugefügt werden.

Wenn ein virtueller Computer mit [az vm create](/cli/azure/vm) erstellt wird, können Sie den Parameter `--availability-set` verwenden, um den Namen der Verfügbarkeitsgruppe anzugeben.

```azurecli-interactive
for i in `seq 1 2`; do
   az vm create \
     --resource-group myResourceGroupAvailability \
     --name myVM$i \
     --availability-set myAvailabilitySet \
     --size Standard_DS1_v2  \
     --vnet-name myVnet \
     --subnet mySubnet \
     --image UbuntuLTS \
     --admin-username azureuser \
     --generate-ssh-keys
done
```

Die Verfügbarkeitsgruppe enthält nun zwei virtuelle Computer. Da sie sich in derselben Verfügbarkeitsgruppe befinden, stellt Azure sicher, dass die VMs und alle ihre Ressourcen (einschließlich Datenträger) auf isolierte physische Hardware verteilt werden. Mithilfe dieser Verteilung wird eine deutlich höhere Verfügbarkeit der gesamten VM-Lösung sichergestellt.

Die Verteilung der Verfügbarkeitsgruppen kann im Portal unter „Ressourcengruppen“ > „myResourceGroupAvailability“ > „myAvailabilitySet“ angezeigt werden. Die virtuellen Computer werden wie im folgenden Beispiel auf die beiden Fehler- und Updatedomänen verteilt:

![Verfügbarkeitsgruppe im Portal](./media/tutorial-availability-sets/fd-ud.png)

## <a name="check-for-available-vm-sizes"></a>Prüfen der verfügbaren VM-Größen

Weitere virtuelle Computer können der Verfügbarkeitsgruppe später hinzugefügt werden, wenn auf der Hardware VM-Größen verfügbar sind. Verwenden Sie [az vm availability-set list-sizes](/cli/azure/vm/availability-set#az-vm-availability-set-list-sizes), um alle verfügbaren Größen im Hardwarecluster für die Verfügbarkeitsgruppe aufzulisten:

```azurecli-interactive
az vm availability-set list-sizes \
     --resource-group myResourceGroupAvailability \
     --name myAvailabilitySet \
     --output table
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Verfügbarkeitsgruppe erstellen
> * Erstellen eines virtuellen Computers in einer Verfügbarkeitsgruppe
> * Überprüfen der verfügbaren VM-Größen

Im nächsten Tutorial erhalten Sie Informationen zu VM-Skalierungsgruppen.

> [!div class="nextstepaction"]
> [Erstellen einer VM-Skalierungsgruppe](tutorial-create-vmss.md)

* Weitere Informationen zu Verfügbarkeitszonen finden Sie in der [Dokumentation zu Verfügbarkeitszonen](../../availability-zones/az-overview.md).
* [Hier](./manage-availability.md) finden Sie weitere Dokumentationen zu Verfügbarkeitsgruppen und -zonen.
* Wenn Sie Verfügbarkeitszonen ausprobieren möchten, sehen Sie sich den Artikel [Erstellen eines virtuellen Linux-Computers in einer Verfügbarkeitszone mit der Azure CLI](./create-cli-availability-zone.md) an.
