---
title: Löschen eines Azure Site Recovery-Tresors
description: Hier erfahren Sie, wie Sie einen Site Recovery-Tresor löschen, der für Azure Site Recovery konfiguriert wurde.
author: rajani-janaki-ram
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 11/05/2019
ms.author: rajanaki
ms.openlocfilehash: fb1e22b0ca1da00bf2665d863b40f19fa1621771
ms.sourcegitcommit: bc7725874a1502aa4c069fc1804f1f249f4fa5f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73721295"
---
# <a name="delete-a-site-recovery-services-vault"></a>Löschen eines Site Recovery Services-Tresors

In diesem Artikel wird beschrieben, wie Sie einen Recovery Services-Tresor für Site Recovery löschen. Informationen zum Löschen eines in Azure Backup verwendeten Tresors finden Sie unter [Löschen eines Azure Backup-Tresors](../backup/backup-azure-delete-vault.md).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]


## <a name="before-you-start"></a>Vorbereitung

Bevor Sie einen Tresor löschen können, müssen Sie die im Tresor registrierten Server und Elemente entfernen. Was Sie entfernen müssen, hängt von den von Ihnen bereitgestellten Replikationsszenarios ab. 


## <a name="delete-a-vault-azure-vm-to-azure"></a>Löschen einer vault-Azure-VM in Azure

1. Befolgen Sie [diese Anleitung](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-azure-vm-azure-to-azure), um alle geschützten VMs zu löschen.
2. Löschen Sie dann den Tresor.

## <a name="delete-a-vault-vmware-vm-to-azure"></a>Löschen einer vault-VMware-VM in Azure

1. Befolgen Sie [diese Anleitung](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-vmware-vm-or-physical-server-vmware-to-azure), um alle geschützten VMs zu löschen.
2. Führen Sie [diese Schritte](vmware-azure-set-up-replication.md#disassociate-or-delete-a-replication-policy) aus, um alle Replikationsrichtlinien zu löschen.
3. Löschen Sie Verweise auf vCenter mithilfe [der folgenden Schritte](vmware-azure-manage-vcenter.md#delete-a-vcenter-server).
4. Befolgen Sie [diese Anweisungen](vmware-azure-manage-configuration-server.md#delete-or-unregister-a-configuration-server), um einen Konfigurationsserver außer Betrieb zu setzen.
5. Löschen Sie dann den Tresor.


## <a name="delete-a-vault-hyper-v-vm-with-vmm-to-azure"></a>Löschen einer vault-Hyper-V-VM (mit VMM) in Azure

1. Führen Sie [diese Schritte](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-hyper-v-virtual-machine-replicating-to-azure-using-the-system-center-vmm-to-azure-scenario) aus, um von System Center VMM verwaltete Hyper-V-VMs zu löschen.
2. Heben Sie die Zuordnung aller Replikationsrichtlinien auf, und löschen Sie sie. Führen Sie dies in Ihrem Tresor durch: **Site Recovery-Infrastruktur** > **Für System Center VMM** > **Replikationsrichtlinien**.
3. Führen Sie [diese Schritte](site-recovery-manage-registration-and-protection.md##unregister-a-vmm-server) aus, um die Registrierung eines verbundenen VMM-Servers aufzuheben.
4. Löschen Sie dann den Tresor.

## <a name="delete-a-vault-hyper-v-vm-to-azure"></a>Löschen einer vault-Hyper-V-VM in Azure

1. Führen Sie [diese Schritte](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-hyper-v-virtual-machine-hyper-v-to-azure) aus, um alle geschützten VMs zu löschen.
2. Heben Sie die Zuordnung aller Replikationsrichtlinien auf, und löschen Sie sie. Führen Sie dies in Ihrem Tresor durch: **Site Recovery-Infrastruktur** > **Für Hyper-V-Standorte** > **Replikationsrichtlinien**.
3. Befolgen Sie [diese Anweisungen](site-recovery-manage-registration-and-protection.md#unregister-a-hyper-v-host-in-a-hyper-v-site), um die Registrierung eines Hyper-V-Hosts aufzuheben.
4. Löschen Sie den Hyper-V-Standort.
5. Löschen Sie dann den Tresor.


## <a name="use-powershell-to-force-delete-the-vault"></a>Verwenden von PowerShell, um das Löschen des Tresors zu erzwingen 

> [!Important]
> Wenn Sie das Produkts testen und keine Bedenken bezüglich eines Datenverlusts haben, können Sie mit der Löschen erzwingen-Methode den Tresor und alle Abhängigkeiten schnell entfernen.
> Der PowerShell-Befehl löscht den gesamten Inhalt des Tresors und ist **nicht umkehrbar**.

Verwenden Sie die folgenden Befehle, um den Site Recovery-Tresor zu löschen, selbst wenn geschützte Elemente vorhanden sind:

    Connect-AzAccount

    Select-AzSubscription -SubscriptionName "XXXXX"

    $vault = Get-AzRecoveryServicesVault -Name "vaultname"

    Remove-AzRecoveryServicesVault -Vault $vault

Erfahren Sie mehr über [Get-AzRecoveryServicesVault](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesvault) und [Remove-AzRecoveryServicesVault](https://docs.microsoft.com/powershell/module/az.recoveryservices/remove-azrecoveryservicesvault).
