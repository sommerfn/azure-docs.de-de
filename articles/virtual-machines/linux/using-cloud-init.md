---
title: Übersicht über die cloud-init-Unterstützung für virtuelle Linux-Computer in Azure | Microsoft-Dokumentation
description: Übersicht über die cloud-init-Funktionen in Microsoft Azure
services: virtual-machines-linux
documentationcenter: ''
author: danielsollondon
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: 195c22cd-4629-4582-9ee3-9749493f1d72
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 10/11/2019
ms.author: danis
ms.openlocfilehash: b0300dd91876b651015ae78c53dbc1e72bf8dd68
ms.sourcegitcommit: e0a1a9e4a5c92d57deb168580e8aa1306bd94723
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2019
ms.locfileid: "72285694"
---
# <a name="cloud-init-support-for-virtual-machines-in-azure"></a>cloud-init-Unterstützung für virtuelle Computer in Azure
In diesem Artikel wird die vorhandene Unterstützung für [cloud-init](https://cloudinit.readthedocs.io) zum Konfigurieren von virtuellen Computern (VMs) oder VM-Skalierungsgruppen während der Bereitstellung in Azure erläutert. Diese cloud-init-Skripts werden beim erstmaligen Starten ausgeführt, nachdem die Ressourcen von Azure bereitgestellt wurden.  

## <a name="cloud-init-overview"></a>Übersicht zu cloud-init
[Cloud-init](https://cloudinit.readthedocs.io) ist ein weit verbreiteter Ansatz zum Anpassen einer Linux-VM beim ersten Start. Sie können mit cloud-init Pakete installieren und Dateien schreiben oder Benutzer und Sicherheit konfigurieren. Da cloud-init während des ersten Startvorgangs aufgerufen wird, müssen Sie keine zusätzlichen Schritte oder Agents auf Ihre Konfiguration anwenden.  Weitere Informationen zum ordnungsgemäßen Formatieren Ihrer `#cloud-config`-Dateien finden Sie auf der [cloud-init-Dokumentationswebsite](https://cloudinit.readthedocs.io/en/latest/topics/format.html#cloud-config-data).  `#cloud-config`-Dateien sind Base64-codierte Textdateien.

Cloud-init funktioniert auch Distributionen übergreifend. Verwenden Sie z.B. nicht **apt-get install** oder **yum install**, um ein Paket zu installieren. Stattdessen können Sie eine Liste mit zu installierenden Paketen definieren. „cloud-init“ verwendet automatisch das native Paketverwaltungstool für die ausgewählte Distribution.

Wir arbeiten aktiv mit unseren Linux-Distributionspartnern zusammen, um cloud-init-fähige Images im Azure Marketplace zur Verfügung zu stellen. Mit diesen Images funktionieren Ihre cloud-init-Bereitstellungen und -Konfigurationen nahtlos mit VMs und VM-Skalierungsgruppen. In der folgenden Tabelle sind die aktuell verfügbaren cloud-init-fähigen Images für die Azure-Plattform aufgeführt:

| Herausgeber | Angebot | SKU | Version | cloud-init-fähig |
|:--- |:--- |:--- |:--- |:--- |
|Canonical |UbuntuServer |18.04-LTS |latest |Ja | 
|Canonical |UbuntuServer |16.04-LTS |latest |Ja | 
|Canonical |UbuntuServer |14.04.5-LTS |latest |Ja |
|CoreOS |CoreOS |Stable |latest |Ja |
|OpenLogic 7.7 |CentOS |7-CI |7.7.20190920 |preview |
|RedHat 7.6 |RHEL |7-RAW-CI |7.6.2019072418 |Ja |
|RedHat 7.7 |RHEL |7-RAW-CI |7.7.2019081601 |preview |
    
Derzeit unterstützt Azure Stack die Bereitstellung von RHEL 7.x und CentOS 7.x mithilfe von cloud-init nicht.

* Für RHEL 7.6 (cloud-init-Paket) wird das folgende Paket unterstützt: *18.2-1.el7_6.2* 
* Für RHEL 7.7 (Vorschau), cloud-init-Paket, ist das Vorschaupaket: *18.5-3.el7*
* Für CentOS 7.7 (Vorschau), cloud-init-Paket, ist das Vorschaupaket: *18.5-3.el7.centos*

## <a name="what-is-the-difference-between-cloud-init-and-the-linux-agent-wala"></a>Was ist der Unterschied zwischen cloud-init und Linux-Agent (WALA)?
WALA ist ein für die Azure-Plattform spezifischer Agent zum Bereitstellen und Konfigurieren von virtuellen Computern und zum Behandeln von Azure-Erweiterungen. Wir ergänzen das Konfigurieren von virtuellen Computern um cloud-init anstelle des Linux-Agents, damit cloud-init-Kunden ihre vorhandenen cloud-init-Skripts verwenden können.  Wenn Sie in cloud-init-Skripts zum Konfigurieren von Linux-Systemen investiert haben, sind für die Aktivierung **keine zusätzlichen Einstellungen erforderlich**. 

Wenn Sie während der Bereitstellung nicht den Schalter `--custom-data` der Azure-Befehlszeilenschnittstelle einfügen, verwendet WALA die minimalen VM-Bereitstellungsparameter zum Bereitstellen des virtuellen Computers und zum Fertigstellen der Bereitstellung mit den Standardeinstellungen.  Wenn Sie auf den cloud-init-Schalter `--custom-data` verweisen, werden die WALA-Standardwerte durch Ihre Änderungen an benutzerdefinierten Daten (einzelne Einstellungen oder vollständige Skripts) überschrieben. 

WALA-Konfigurationen von virtuellen Computern sind zeitlich beschränkt auf die Verwendung während der maximalen VM-Bereitstellungszeit.  cloud-init-Konfigurationen, die auf virtuelle Computer angewendet werden, sind nicht zeitlich beschränkt und führen nicht zu Fehlern bei der Bereitstellung durch ein Timeout. 

## <a name="deploying-a-cloud-init-enabled-virtual-machine"></a>Bereitstellen von cloud-init-fähigen virtuellen Computern
Das Bereitstellen eines clout-init-fähigen virtuellen Computers besteht einfach aus dem Verweisen auf eine cloud-init-fähige Distribution während der Bereitstellung.  Die Verwalter von Linux-Distributionen müssen cloud-init in ihren bei Azure veröffentlichten Basisimages aktivieren und integrieren. Nachdem Sie sich vergewissert haben, dass das Image, das Sie bereitstellen möchten, cloud-init unterstützt, können Sie es mit der Azure-Befehlszeilenschnittstelle bereitstellen. 

Der erste Schritt der Bereitstellung dieses Images ist die Erstellung einer Ressourcengruppe mit dem Befehl [az group create](/cli/azure/group). Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden. 

Das folgende Beispiel erstellt eine Ressourcengruppe mit dem Namen *myResourceGroup* am Standort *eastus*.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```
Im nächsten Schritt erstellen Sie in der aktuellen Shell eine Datei namens *cloud-init.txt* und fügen die folgende Konfiguration ein. Erstellen Sie für dieses Beispiel die Datei in der Cloud Shell, nicht auf dem lokalen Computer. Dazu können Sie einen beliebigen Editor verwenden. Geben Sie `sensible-editor cloud-init.txt` ein, um die Datei zu erstellen und eine Liste der verfügbaren Editoren anzuzeigen. Wählen Sie #1 aus, um den Editor **nano** zu verwenden. Stellen Sie sicher, dass die gesamte Datei „cloud-init“ ordnungsgemäß kopiert wird, insbesondere die erste Zeile:

```yaml
#cloud-config
package_upgrade: true
packages:
  - httpd
```
Drücken Sie `ctrl-X`, um die Datei zu verlassen, geben Sie `y` ein, um die Datei zu speichern, und drücken Sie `enter`, um den Dateinamen beim Beenden zu bestätigen.

Erstellen Sie im letzten Schritt mit dem Befehl [az vm create](/cli/azure/vm) einen virtuellen Computer. 

Das folgende Beispiel erstellt einen virtuellen Computer mit dem Namen *centos74* und SSH-Schlüssel, falls diese nicht bereits an einem Standardschlüsselspeicherort vorhanden sind. Um einen bestimmten Satz von Schlüsseln zu verwenden, nutzen Sie die Option `--ssh-key-value`.  Verwenden Sie den `--custom-data`-Parameter, um Ihre cloud-init-Konfigurationsdatei zu übergeben. Geben Sie den vollständigen Pfad zu der Konfigurationsdatei *cloud-init.txt* an, wenn Sie die Datei außerhalb Ihres vorhandenen Arbeitsverzeichnisses gespeichert haben. Im folgenden Beispiel wird ein virtueller Computer namens *centos74* erstellt:

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name centos74 \
  --image OpenLogic:CentOS-CI:7-CI:latest \
  --custom-data cloud-init.txt \
  --generate-ssh-keys 
```

Nach dem Erstellen des virtuellen Computers werden in der Azure-Befehlszeilenschnittstelle Informationen zu Ihrer Bereitstellung angezeigt. Notieren Sie sich den Wert von `publicIpAddress`. Diese Adresse wird verwendet, um auf den virtuellen Computer zuzugreifen.  Es dauert einige Minuten, den virtuellen Computer zu erstellen, die Pakete zu installieren und die App zu starten. Es gibt Hintergrundaufgaben, die weiterhin ausgeführt werden, wenn Ihnen von der Azure CLI wieder eine Eingabeaufforderung angezeigt wird. Sie können per SSH eine Verbindung mit dem virtuellen Computer herstellen und anhand der Schritte im Abschnitt zur Problembehandlung die cloud-init-Protokolle anzeigen. 

## <a name="troubleshooting-cloud-init"></a>Beheben von Problemen mit cloud-init
Nachdem der virtuelle Computer bereitgestellt wurde, durchläuft cloud-init alle Module und Skripts, die in `--custom-data` definiert sind, um den virtuellen Computer zu konfigurieren.  Wenn Sie Fehler oder ausgelassene Einstellungen der Konfiguration korrigieren möchten, müssen Sie den Modulnamen (z.B. `disk_setup` oder `runcmd`) im cloud-init-Protokoll unter **/var/log/cloud-init.log** suchen.

> [!NOTE]
> Nicht jeder Modulfehler führt zu einer insgesamt fehlerhaften cloud-init-Konfiguration. Wenn Sie beispielsweise das Modul `runcmd` verwenden und das Skript einen Fehler verursacht, meldet cloud-init trotzdem eine erfolgreiche Bereitstellung, da das Modul runcmd ausgeführt wurde.

Weitere Informationen zur cloud-init-Protokollierung finden Sie in der [cloud-init-Dokumentation](https://cloudinit.readthedocs.io/en/latest/topics/logging.html). 

## <a name="next-steps"></a>Nächste Schritte
cloud-init-Beispiele für Änderungen an der Konfiguration finden Sie in den folgenden Dokumenten:
 
- [Hinzufügen weiterer Linux-Benutzer zu virtuellen Computern](cloudinit-add-user.md)
- [Ausführen eines Paket-Managers zum Aktualisieren vorhandener Pakete beim ersten Start](cloudinit-update-vm.md)
- [Ändern des lokalen Hostnamens virtueller Computer](cloudinit-update-vm-hostname.md) 
- [Installieren von Anwendungspaketen, Aktualisieren von Konfigurationsdateien und Einfügen von Schlüsseln](tutorial-automate-vm-deployment.md)
