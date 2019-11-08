---
title: Vorbereiten von lokalen Computern für die Migration zu Azure mit Azure Migrate
description: In diesem Artikel wird beschrieben, wie Sie lokale Computer für die Migration zu Azure mit Azure Migrate vorbereiten.
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 10/03/2019
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 61b4f41a0b36945413e45a357a5ca73ac75ceb98
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73480122"
---
# <a name="prepare-on-premises-machines-for-migration-to-azure"></a>Vorbereiten von lokalen Computern für die Migration zu Azure

In diesem Artikel wird beschrieben, wie Sie lokale Computer vorbereiten, bevor Sie diese per [Azure Migrate-Servermigration](migrate-services-overview.md#azure-migrate-server-migration-tool) zu Azure migrieren.


In diesem Artikel führen Sie folgende Schritte aus:
> [!div class="checklist"]
> * Überprüfen der Einschränkungen bei der Migration
> * Überprüfen der Betriebssystemanforderungen und Supporteinschränkungen
> * Überprüfen des URL-/Portzugriffs für zu migrierende Computer
> * Überprüfen der Änderungen, die Sie vor Beginn der Migration ggf. vornehmen müssen
> * Konfigurieren der Einstellungen, damit Laufwerkbuchstaben nach der Migration beibehalten werden
> * Vorbereiten von Computern, damit Sie nach der Migration eine Verbindung mit den Azure-VMs herstellen können


## <a name="verify-migration-limitations"></a>Überprüfen der Einschränkungen bei der Migration

- Mit der Azure Migrate-Servermigration können Sie pro Azure Migrate-Projekt bis zu 35.000 VMware-VMs/Hyper-V-VMs bewerten. In einem Projekt können VMware-VMs und Hyper-V-VMs bis zum jeweiligen Limit kombiniert werden.
- Sie können bis zu zehn VMs gleichzeitig für die Migration auswählen. Verwenden Sie jeweils Gruppen von zehn VMs, falls Sie eine höhere Zahl replizieren möchten.
- Bei der VMware-Migration ohne Agents können Sie bis zu 100 Replikationsvorgänge gleichzeitig ausführen.

## <a name="verify-operating-system-requirements"></a>Überprüfen der Betriebssystemanforderungen

- Überprüfen Sie, ob Ihre [Windows-Betriebssysteme](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines) in Azure unterstützt werden.
- Überprüfen Sie, ob Ihre [Linux-Distributionen](../virtual-machines/linux/endorsed-distros.md) in Azure unterstützt werden.


## <a name="check-whats-supported"></a>Überprüfen der Unterstützung

- Für VMware-VMs wird bei der Azure Migrate-Servermigration die [Migration mit oder ohne Agent](server-migrate-overview.md) unterstützt. Überprüfen Sie für Migrationen [ohne Agent](migrate-support-matrix-vmware.md#migration---limitations) und [mit Agent](migrate-support-matrix-vmware.md#agent-based-migration-vmware-vm-requirements) die Anforderungen bzw. die Unterstützung in Bezug auf VMware-VMs.
- Überprüfen Sie die [Migrationsanforderungen und -unterstützung](migrate-support-matrix-hyper-v.md#migration-hyper-v-vm-requirements) in Bezug auf Hyper-V-VMs.
- Überprüfen Sie die [Migrationsanforderungen und -unterstützung](migrate-support-matrix-vmware.md#agent-based-migration-vmware-vm-requirements) für lokale physische Computer oder andere virtualisierte Server. Diese Anforderungen ähneln denen von VMware-VMs.





## <a name="review-urlport-access"></a>Überprüfen des URL-/Portzugriffs

Computer benötigen während der Migration ggf. Internetzugriff.

- Überprüfen Sie die URLs, auf die VMware-VMs während der Migration [ohne Agent](migrate-support-matrix-vmware.md#agentless-migration-url-access-requirements) oder [mit Agent](migrate-support-matrix-vmware.md#agent-based-migration-url-access-requirements) Zugriff haben müssen.
- Überprüfen Sie die URLs, auf die Hyper-V-Hosts während der Migration Zugriff haben müssen. Hyper-V-VMs benötigen keinen Internetzugriff.
- [Überprüfen Sie die URLs](migrate-support-matrix-vmware.md#agent-based-migration-url-access-requirements), auf die physische Computer oder andere virtualisierte Server während der Migration Zugriff haben müssen.
- Bei der Migration von VMware-VMs/physischen Servern mit Agent benötigt der Mobilitätsdienst, der auf den Computern ausgeführt wird, Zugriff auf Azure Migrate-Komponenten. Bei der Replikationsverwaltung kommuniziert der Dienst, der auf dem Computer ausgeführt wird, mit der lokalen Azure Migrate-Replikationsappliance über den Port HTTPS 443 für eingehenden Datenverkehr. Die Computer senden Replikationsdaten an den Azure Migrate-Prozessserver über den Port HTTPS 9443 für eingehenden Datenverkehr. Dieser Port kann geändert werden.


## <a name="verify-required-changes-before-migration"></a>Überprüfen der erforderlichen Änderungen vor der Migration

Einige VMs erfordern möglicherweise Änderungen, damit sie in Azure ausgeführt werden können. Von Azure Migrate werden diese Änderungen auf VMs, auf denen die folgenden Betriebssysteme ausgeführt werden, automatisch vorgenommen:
- Red Hat Enterprise Linux 6.5+, 7.0+
- CentOS 6.5+, 7.0+
- SUSE Linux Enterprise Server 12 SP1+
- Ubuntu 14.04 LTS, 16.04 LTS, 18.04 LTS
- Debian 7, 8

Bei anderen Betriebssystemen müssen Sie Computer vor der Migration manuell vorbereiten. 

### <a name="prepare-windows-machines"></a>Vorbereiten von Windows-Computern

Falls Sie einen Windows-Computer migrieren, müssen Sie diese Änderungen vor der Migration durchführen. Wenn Sie den virtuellen Computer migrieren, bevor Sie die Änderungen vorgenommen haben, wird der virtuelle Computer in Azure unter Umständen nicht gestartet.

1. [Aktivieren Sie die Azure-Konsole für den seriellen Zugriff](../virtual-machines/troubleshooting/serial-console-windows.md) für die Azure-VM. Dies dient Ihnen als Hilfe bei der Problembehandlung. Sie müssen den virtuellen Computer nicht neu starten. Die Azure-VM wird mit dem Datenträgerimage gestartet. Dies entspricht einem Neustart des neuen virtuellen Computers. 
2. Wenn Sie Computer migrieren, auf denen Windows Server 2003 ausgeführt wird, müssen Sie die Dienste für die Hyper-V-Gastintegration unter dem Betriebssystem der VM installieren. [Weitere Informationen](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-integration-services#install-or-update-integration-services)

### <a name="prepare-linux-machines"></a>Vorbereiten von Linux-Computern

1. Installieren Sie Linux Integration Services für Hyper-V. In den meisten neuen Versionen von Linux-Distributionen ist dies standardmäßig enthalten.
2. Erstellen Sie das Linux-Initialisierungsimage neu, damit es die erforderlichen Hyper-V-Treiber enthält. Hiermit wird sichergestellt, dass der virtuelle Computer in Azure gestartet wird. Dies ist nur in einigen Distributionen erforderlich.
3. [Aktivieren Sie die Protokollierung für die serielle Azure-Konsole](../virtual-machines/troubleshooting/serial-console-linux.md). Dies dient Ihnen als Hilfe bei der Problembehandlung. Sie müssen den virtuellen Computer nicht neu starten. Die Azure-VM wird mit dem Datenträgerimage gestartet. Dies entspricht einem Neustart des neuen virtuellen Computers.
4. Aktualisieren Sie die Gerätezuordnungsdatei mit den Zuordnungen von Gerätenamen und Volumes so, dass persistente Gerätebezeichner verwendet werden.
5. Aktualisieren der fstab-Einträge zur Verwendung persistenter Volumebezeichner.
6. Entfernen Sie alle udev-Regeln, die Schnittstellennamen basierend auf der MAC-Adresse usw. reservieren.
7. Aktualisieren Sie die Netzwerkschnittstellen so, dass IP-Adressen per DHCP empfangen werden.
8. [Informieren Sie sich](../virtual-machines/linux/create-upload-generic.md) über die Schritte zum Ausführen einer Linux-VM in Azure, und sehen Sie sich die Anleitungen für einige beliebte Linux-Distributionen an.

## <a name="preserve-drive-letters-after-migration"></a>Beibehalten von Laufwerkbuchstaben nach der Migration

Wenn Sie einen lokalen Computer zu Microsoft Azure migrieren, ändern sich ggf. die Laufwerkbuchstaben von zusätzlichen Datenträgern, sodass diese nicht mehr die vorherigen Werte aufweisen. Standardmäßig wird Azure-VMs das Laufwerk D zur Verwendung als temporärer Speicher zugewiesen. Diese Laufwerkzuweisung führt dazu, dass alle anderen angefügten Zuweisungen von Speicherlaufwerken um einen Buchstaben inkrementiert werden.

Wenn für Ihre lokale Installation beispielsweise ein Datenträger genutzt wird, dem der Laufwerkbuchstabe D für Anwendungsinstallationen zugewiesen ist, wird die Zuweisung für dieses Laufwerk auf Laufwerk E inkrementiert, nachdem Sie die VM zu Azure migriert haben. Legen Sie wie folgt die SAN-Richtlinie (Storage Area Network) auf „OnlineAll“ fest, um diese automatische Zuweisung zu verhindern und sicherzustellen, dass Azure dem temporären Volume den nächsten freien Laufwerkbuchstaben zuweist:

1. Öffnen Sie auf dem lokalen Computer (nicht auf dem Hostserver) eine Eingabeaufforderung mit erhöhten Rechten.
2. Geben Sie **diskpart** ein.
3. Geben Sie **SAN** ein. Wenn der Laufwerkbuchstabe des Gastbetriebssystems nicht beibehalten wird, wird **Offline – Alle** oder **Offline – Freigegeben** zurückgegeben.
4. Geben Sie an der **DISKPART**-Eingabeaufforderung den Befehl **SANPOLICY=ONLINEALL** ein. Mit dieser Einstellung wird sichergestellt, dass Datenträger in den Onlinezustand versetzt werden und sowohl lesbar als auch beschreibbar sind.
5. Während der Testmigration können Sie überprüfen, ob die Laufwerkbuchstaben beibehalten werden.


## <a name="check-azure-vm-requirements"></a>Überprüfen der Anforderungen von Azure-VMs

Für lokale Computer, die Sie in Azure replizieren, müssen die Azure-VM-Anforderungen für Betriebssystem und Architektur, Datenträger, Netzwerkeinstellungen und die VM-Benennung erfüllt sein. Überprüfen Sie die Anforderungen für [VMware-VMs/physische Server](migrate-support-matrix-vmware.md#azure-vm-requirements) und [Hyper-V-VMs](migrate-support-matrix-hyper-v.md#migration-hyper-v-vm-requirements) vor der Migration.


## <a name="prepare-to-connect-after-migration"></a>Vorbereiten der Verbindung nach der Migration

Azure-VMs werden während der Migration zu Azure erstellt. Nach der Migration müssen Sie in der Lage sein, eine Verbindung mit den neuen virtuellen Azure-Computern herzustellen. Damit die Verbindungsherstellung erfolgreich ist, müssen einige Schritte ausgeführt werden.

### <a name="prepare-to-connect-to-windows-azure-vms"></a>Vorbereiten der Verbindung mit Windows Azure-VMs

Führen Sie auf lokalen Windows-Computern die folgenden Schritte aus:

1. Konfigurieren Sie die Windows-Einstellungen. Hierzu gehört auch das Entfernen aller statischen beständigen Routen oder des WinHTTP-Proxys.
2. Stellen Sie sicher, dass [diese Dienste](../virtual-machines/windows/prepare-for-upload-vhd-image.md#check-the-windows-services) ausgeführt werden.
3. Aktivieren Sie den Remotedesktop (RDP), um Remoteverbindungen mit dem lokalen Computer zuzulassen. [Erfahren Sie](../virtual-machines/windows/prepare-for-upload-vhd-image.md#update-remote-desktop-registry-settings), wie RDP mit PowerShell aktiviert werden kann.
4. Wenn Sie nach der Migration über das Internet auf eine Azure-VM zugreifen möchten, lassen Sie in der Windows-Firewall auf dem lokalen Computer TCP und UDP im öffentlichen Profil zu, und legen Sie RDP als zulässige App für alle Profile fest.
5. Wenn Sie nach der Migration auf eine Azure-VM über ein Site-to-Site-VPN zugreifen möchten, lassen Sie in der Windows-Firewall auf dem lokalen Computer RDP für das Domänenprofil und private Profile zu. [Erfahren Sie](../virtual-machines/windows/prepare-for-upload-vhd-image.md#configure-windows-firewall-rules), wie RDP-Datenverkehr zugelassen wird. 
6. Stellen Sie sicher, dass auf der lokalen VM keine ausstehenden Windows-Updates vorhanden sind, wenn Sie die Migration durchführen. Sollte dies nämlich der Fall sein, werden nach der Migration auf dem virtuellen Azure-Computer ggf. Updates installiert, und Sie können sich dann erst bei der VM anmelden, nachdem die Updates abgeschlossen sind.


### <a name="prepare-to-connect-with-linux-azure-vms"></a>Vorbereiten der Verbindung mit Linux-Azure-VMs

Führen Sie auf lokalen Linux-Computern die folgenden Schritte aus:

1. Stellen Sie sicher, dass der Secure Shell-Dienst so festgelegt ist, dass er beim Systemstart automatisch gestartet wird.
2. Überprüfen Sie, ob die Firewallregeln eine SSH-Verbindung zulassen.

### <a name="configure-azure-vms-after-migration"></a>Vorbereiten virtueller Azure-Computer nach der Migration

Führen Sie nach der Migration auf den Azure-VMs, die erstellt werden, die folgenden Schritte aus.

1. Um aus dem Internet auf die VM zugreifen zu können, müssen Sie ihr eine öffentliche IP-Adresse zuweisen. Sie können nicht die gleiche öffentliche IP-Adresse für den virtuellen Azure-Computer verwenden, die Sie für Ihren lokalen Computer verwendet haben. [Weitere Informationen](../virtual-network/virtual-network-public-ip-address.md)
2. Überprüfen Sie, ob die Regeln der Netzwerksicherheitsgruppen (NSG) auf dem virtuellen Computer eingehende Verbindungen am RDP- oder SSH-Port zulassen.
3. Überprüfen Sie die [Startdiagnose](../virtual-machines/troubleshooting/boot-diagnostics.md#enable-boot-diagnostics-on-existing-virtual-machine), um die VM anzuzeigen.

> [!NOTE]
> Der Azure Bastion-Dienst bietet privaten RDP- und SSH-Zugriff auf Azure-VMs. Sehen Sie sich die [weiteren Informationen](../bastion/bastion-overview.md) zu diesem Dienst an.



## <a name="next-steps"></a>Nächste Schritte

Treffen Sie eine Entscheidung zum Verfahren, das Sie zum [Migrieren von VMware-VMs](server-migrate-overview.md) zu Azure verwenden möchten, oder beginnen Sie mit dem Migrieren von [Hyper-V-VMs](tutorial-migrate-hyper-v.md) bzw. [physischen Servern oder virtualisierten VMs/Cloud-VMs](tutorial-migrate-physical-virtual-machines.md).
