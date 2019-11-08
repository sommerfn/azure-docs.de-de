---
title: Reparieren eines virtuellen Windows-Computers mit den Reparaturbefehlen für virtuelle Azure-Computer | Microsoft-Dokumentation
description: In diesem Artikel ist beschrieben, wie Sie mit Reparaturbefehlen für virtuelle Computer den Datenträger mit einem anderen virtuellen Windows-Computer verbinden, um Fehler zu beheben, und dann Ihren ursprünglichen virtuellen Computer wiederherstellen.
services: virtual-machines-windows
documentationcenter: ''
author: v-miegge
manager: dcscontentpm
editor: ''
tags: virtual-machines
ms.service: virtual-machines
ms.topic: troubleshooting
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: azurecli
ms.date: 09/10/2019
ms.author: v-miegge
ms.openlocfilehash: 6bda8cb831e84a56c889ed40109954551a34c113
ms.sourcegitcommit: 018e3b40e212915ed7a77258ac2a8e3a660aaef8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73796176"
---
# <a name="repair-a-windows-vm-by-using-the-azure-virtual-machine-repair-commands"></a>Reparieren eines virtuellen Windows-Computers mit dem Reparaturbefehlen virtueller Azure-Computer

Wenn für Ihren virtuellen Windows-Computer (Windows-VM) in Azure ein Start- oder Datenträgerfehler auftritt, müssen Sie das Problem möglicherweise selbst auf dem Datenträger beheben. Ein gängiges Beispiel wäre ein ungültiges Anwendungsupdate, das den erfolgreichen Start der VM verhindert. In diesem Artikel ist beschrieben, wie Sie mit Reparaturbefehlen für virtuelle Azure-Computer den Datenträger mit einem anderen virtuellen Windows-Computer verbinden, um Fehler zu beheben, und dann Ihren ursprünglichen virtuellen Computer wiederherstellen.

> [!IMPORTANT]
> Die Skripts in diesem Artikel gelten nur für VMs, für die [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) verwendet wird.

## <a name="repair-process-overview"></a>Übersicht über den Reparaturprozess

Sie können jetzt Reparaturbefehle für Azure-VMs verwenden, um den Betriebssystemdatenträger für eine VM zu ändern. Es ist nicht mehr erforderlich, dass Sie die jeweilige VM löschen und dann neu erstellen.

Führen Sie die folgenden Schritte aus, um das VM-Problem zu beheben:

1. Starten von Azure Cloud Shell
2. Führen Sie „az extension add/update“ aus.
3. Führen Sie „az vm repair create“ aus.
4. Führen Sie „az vm repair run“ aus.
5. Führen Sie „az vm repair restore“ aus.

Weitere Dokumentation und Anweisungen finden Sie unter [az vm repair](https://docs.microsoft.com/cli/azure/ext/vm-repair/vm/repair).

## <a name="repair-process-example"></a>Beispiel für einen Reparaturprozess

> [!NOTE]
> * Damit das Skript ausgeführt werden kann, ist eine ausgehende Verbindung von der VM (Port 443) erforderlich.
> * Es kann immer nur jeweils ein Skript ausgeführt werden.
> * Ein Skript, das ausgeführt wird, kann nicht abgebrochen werden.
> * Ein Skript kann bis zu maximal 90 Minuten ausgeführt werden. Danach tritt ein Timeout für das Skript auf.

1. Starten von Azure Cloud Shell

   Azure Cloud Shell ist eine kostenlose interaktive Shell, mit der Sie die Schritte in diesem Artikel ausführen können. Sie umfasst gängige Tools, die zur Nutzung mit Ihrem Konto vorinstalliert und konfiguriert sind.

   Wählen Sie zum Öffnen von Cloud Shell oben rechts in einem Codeblock die Option **Ausprobieren** aus. Sie können Cloud Shell auch auf einer separaten Browserregisterkarte öffnen, indem Sie zu [https://shell.azure.com](https://shell.azure.com) navigieren.

   Wählen Sie **Kopieren** aus, um die Codeblöcke zu kopieren. Fügen Sie den Code anschließend in Cloud Shell ein, und wählen Sie **Eingabe**, um den Code auszuführen.

   Wenn Sie es vorziehen, die Befehlszeilenschnittstelle lokal zu installieren und zu verwenden, müssen Sie für diese Schnellstartanleitung mindestens Azure CLI-Version 2.0.30 verwenden. Führen Sie ``az --version`` aus, um die Version zu finden. Informationen zum Installieren oder Aktualisieren Ihrer Azure-Befehlszeilenschnittstelle finden Sie unter [Installieren der Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli).

2. Wenn Sie die `az vm repair`-Befehle zum ersten Mal verwenden, fügen Sie die CLI-Erweiterung „vm-repair“ hinzu.

   ```azurepowershell-interactive
   az extension add -n vm-repair
   ```

   Wenn Sie die `az vm repair`-Befehle bereits verwendet haben, wenden Sie alle Updates auf die Erweiterung „vm-repair“ an.

   ```azurepowershell-interactive
   az extension update -n vm-repair
   ```

3. Führen Sie `az vm repair create`aus. Mit diesem Befehl wird eine Kopie des Betriebssystemdatenträgers der fehlerhaften VM erstellt, wird eine Reparatur-VM erstellt und wird der Datenträger zugeordnet.

   ```azurepowershell-interactive
   az vm repair create -g MyResourceGroup -n myVM --repair-username username --repair-password password!234 --verbose
   ```

4. Führen Sie `az vm repair run`aus. Mit diesem Befehl wird das angegebene Reparaturskript auf dem zugeordneten Datenträger über die Reparatur-VM ausgeführt.

   ```azurepowershell-interactive
   az vm repair run  –g MyResourceGroup –n MyVM -–run-on-repair --run-id 2 --verbose
   ```

5. Führen Sie `az vm repair restore`aus. Mit diesem Befehl wird der ursprüngliche Betriebssystemdatenträger gegen den reparierten Betriebssystemdatenträger der VM getauscht.

   ```azurepowershell-interactive
   az vm repair restore -g MyResourceGroup -n MyVM --verbose
   ```

## <a name="verify-and-enable-boot-diagnostics"></a>Überprüfen und Aktivieren der Startdiagnose

Im folgenden Beispiel wird die Diagnoseerweiterung in der VM namens ``myVMDeployed`` in der Ressourcengruppe namens ``myResourceGroup`` aktiviert:

Azure-Befehlszeilenschnittstelle

```azurepowershell-interactive
az vm boot-diagnostics enable --name myVMDeployed --resource-group myResourceGroup --storage https://mystor.blob.core.windows.net/
```

## <a name="next-steps"></a>Nächste Schritte

* Wenn Probleme beim Herstellen einer Verbindung mit Ihrer VM auftreten, helfen Ihnen die Informationen unter [Problembehandlung bei Remotedesktopverbindungen mit einem Windows-basierten virtuellen Azure-Computer](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/troubleshoot-rdp-connection) weiter.
* Konsultieren Sie [Beheben von Anwendungskonnektivitätsproblemen auf virtuellen Computern in Azure](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/troubleshoot-app-connection) bei Problemen mit dem Zugriff auf Anwendungen, die auf Ihrer VM ausgeführt werden.
* Weitere Informationen zu Resource Manager finden Sie unter [Übersicht über den Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).
