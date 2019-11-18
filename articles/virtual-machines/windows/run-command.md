---
title: Ausführen von PowerShell-Skripts in einer Windows-VM unter Azure
description: In diesem Thema wird beschrieben, wie PowerShell-Skripts auf einem virtuellen Azure Windows-Computer mithilfe des Features „Befehl ausführen“ ausgeführt werden.
services: automation
ms.service: automation
author: bobbytreed
ms.author: robreed
ms.date: 04/26/2019
ms.topic: article
manager: carmonm
ms.openlocfilehash: fa7f72989d47499127714eddfa6b5e98aa80178c
ms.sourcegitcommit: 827248fa609243839aac3ff01ff40200c8c46966
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73749230"
---
# <a name="run-powershell-scripts-in-your-windows-vm-by-using-run-command"></a>Ausführen von PowerShell-Skripts in Ihrer Windows-VM mithilfe von „Befehl ausführen“

Das Feature „Befehl ausführen“ verwendet den VM-Agent, um PowerShell-Skripts innerhalb einer Azure Windows-VM auszuführen. Sie können diese Skripts für die allgemeine Computer-oder Anwendungsverwaltung verwenden. Mit ihrer Hilfe können Sie VM-Zugriffs- und Netzwerkprobleme schnell diagnostizieren und beheben und die VM wieder in einen funktionierenden Zustand versetzen.

 

## <a name="benefits"></a>Vorteile

Sie können auf verschiedene Weise auf Ihre virtuellen Computer zugreifen. „Befehl ausführen“ kann Skripts mithilfe des VM-Agents remote auf Ihren virtuellen Computern ausführen. „Befehl ausführen“ kann für Windows-VMs über das Azure-Portal, die [REST-API](/rest/api/compute/virtual%20machines%20run%20commands/runcommand) oder [PowerShell](https://docs.microsoft.com/powershell/module/az.compute/invoke-azvmruncommand) verwendet werden.

Diese Funktion ist in allen Szenarien sinnvoll, in denen Sie ein Skript innerhalb eines virtuellen Computers ausführen möchten. Es ist eine der wenigen Möglichkeiten, Fehler auf einem virtuellen Computer zu beheben, bei dem der RDP- oder SSH-Port aufgrund einer falschen Netzwerk- oder Administratorkonfiguration geschlossen sind.

## <a name="restrictions"></a>Einschränkungen

Die Verwendung von „Befehl ausführen“ unterliegt den folgenden Einschränkungen:

* Die Ausgabe ist auf die letzten 4.096 Bytes beschränkt.
* Der Mindestzeitraum für die Ausführung eines Skripts liegt bei etwa 20 Sekunden.
* Skripts werden unter Windows als „System“ ausgeführt.
* Es kann Zeit jeweils nur ein Skript gleichzeitig ausgeführt werden.
* Skripts, die Informationen anfordern (interaktiver Modus), werden nicht unterstützt.
* Die Ausführung von Skripts kann nicht abgebrochen werden.
* Ein Skript kann bis zu maximal 90 Minuten ausgeführt werden. Danach tritt ein Timeout auf.
* Um die Ergebnisse des Skripts zurückzugeben, ist eine ausgehende Konnektivität des virtuellen Computers erforderlich.

> [!NOTE]
> Der Ausführungsbefehl muss über Port 443 eine Verbindung mit öffentlichen Azure-IP-Adressen herstellen können, damit er richtig funktioniert. Wenn die Erweiterung keinen Zugriff auf diese Endpunkte hat, werden die Skripts möglicherweise erfolgreich ausgeführt, geben aber keine Ergebnisse zurück. Wenn Sie Datenverkehr auf dem virtuellen Computer blockieren, können Sie [Diensttags](../../virtual-network/security-overview.md#service-tags) verwenden, um Datenverkehr mit öffentlichen Azure-IP-Adressen über das `AzureCloud`-Tag zulassen.

## <a name="available-commands"></a>Verfügbare Befehle

Diese Tabelle enthält die Liste der für virtuelle Windows-Computer verfügbaren Befehle. Mit dem Befehl **RunPowerShellScript** können Sie jedes beliebige benutzerdefinierte Skript ausführen. Wenn Sie einen Befehl mithilfe der Azure-Befehlszeilenschnittstelle oder über PowerShell ausführen, muss für den Parameter `--command-id` oder `-CommandId` einer der Werte aus der folgenden Liste angegeben werden. Wenn Sie einen Wert angeben, bei dem es sich nicht um einen verfügbaren Befehl handelt, tritt dieser Fehler auf:

```error
The entity was not found in this Azure location
```

|**Name**|**Beschreibung**|
|---|---|
|**RunPowerShellScript**|Führt ein PowerShell-Skript aus.|
|**EnableRemotePS**|Konfiguriert den Computer für die Aktivierung von remote-PowerShell.|
|**EnableAdminAccount**|Überprüft, ob das lokale Administratorkonto deaktiviert ist, und aktiviert es, wenn das der Fall ist.|
|**IPConfig**| Zeigt detaillierte Informationen für die IP-Adresse, die Subnetzmaske und das Standardgateway für jeden an TCP/IP gebundenen Adapter an.|
|**RDPSettings**|Überprüft Registrierungseinstellungen und Domänen-Richtlinieneinstellungen. Schlägt Richtlinienaktionen vor, wenn der Computer Teil einer Domäne ist, oder ändert die Einstellungen in Standardwerte.|
|**ResetRDPCert**|Entfernt das SSL-Zertifikat, das an den RDP-Listener gebunden ist, und stellt die Standardwerte für die Sicherheit des RDP-Listeners wieder her. Verwenden Sie dieses Skript, wenn irgendwelche Probleme in Verbindung mit dem Zertifikat auftreten.|
|**SetRDPPort**|Legt die standardmäßige oder vom Benutzer angegebene Portnummer für Remote Desktop-Verbindungen fest. Aktiviert die Firewallregeln für eingehenden Zugriff auf den Port.|

## <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

Im folgenden Beispiel wird der Befehl [az vm run-command](/cli/azure/vm/run-command?view=azure-cli-latest#az-vm-run-command-invoke) verwendet, um ein Shellskript auf einer Azure Linux-VM auszuführen.

```azurecli-interactive
# script.ps1
#   param(
#       [string]$arg1,
#       [string]$arg2
#   )
#   Write-Host This is a sample script with parameters $arg1 and $arg2

az vm run-command invoke  --command-id RunPowerShellScript --name win-vm -g my-resource-group \
    --scripts @script.ps1 --parameters "arg1=somefoo" "arg2=somebar"
```

## <a name="azure-portal"></a>Azure-Portal

Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu einer VM, und wählen Sie unter **VORGÄNGE** **Befehl ausführen** aus. Es wird eine Liste mit den für die Ausführung auf der VM verfügbaren Befehlen angezeigt.

![Befehlsliste](./media/run-command/run-command-list.png)

Wählen Sie einen auszuführenden Befehl aus. Einige der Befehle weisen möglicherweise optionale oder erforderliche Eingabeparameter auf. Bei diesen Befehlen werden Ihnen die Parameter in Form von Textfeldern angezeigt, für die Sie die Eingabewerte bereitstellen müssen. Für jeden Befehl können Sie das ausgeführte Skript anzeigen, indem Sie **Skript anzeigen** aufklappen. **RunPowerShellScript** unterscheidet sich insofern von den anderen Befehlen, als es Ihnen die Angabe eines eigenen, benutzerdefinierten Skripts ermöglicht.

> [!NOTE]
> Die integrierten Befehle können nicht bearbeitet werden.

Nachdem Sie den Befehl ausgewählt haben, wählen Sie **Ausführen** aus, um das Skript auszuführen. Wenn das Skript abgeschlossen ist, gibt es die Ausgabe und eventuelle Fehler im Ausgabefenster zurück. Der folgende Screenshot zeigt eine Beispielausgabe für die Ausführung des **RDPSettings**-Befehls.

![Skriptausgabe von „Befehl ausführen“](./media/run-command/run-command-script-output.png)

## <a name="powershell"></a>PowerShell

Im folgenden Beispiel wird das Cmdlet [Invoke-AzVMRunCommand](https://docs.microsoft.com/powershell/module/az.compute/invoke-azvmruncommand) zum Ausführen eines PowerShell-Skripts in einem virtuellen Azure-Computer verwendet. Für das Cmdlet gilt, dass das im Parameter `-ScriptPath` referenzierte Skript am Ausführungsort des Cmdlets lokal sein muss.

```azurepowershell-interactive
Invoke-AzVMRunCommand -ResourceGroupName '<myResourceGroup>' -Name '<myVMName>' -CommandId 'RunPowerShellScript' -ScriptPath '<pathToScript>' -Parameter @{"arg1" = "var1";"arg2" = "var2"}
```

## <a name="limiting-access-to-run-command"></a>Einschränken des Zugriffs auf „Befehl ausführen“

Das Auflisten der ausführbaren Befehle oder das Anzeigen der Details zu einem Befehl erfordert die Berechtigung `Microsoft.Compute/locations/runCommands/read` auf Abonnementebene. Die integrierte Rolle [Leser](../../role-based-access-control/built-in-roles.md#reader) und höhere Rollen verfügen über diese Berechtigung.

Zum Ausführen eines Befehls ist die `Microsoft.Compute/virtualMachines/runCommand/action`-Berechtigung auf der Abonnementebene erforderlich. Die Rolle [Mitwirkender für virtuelle Computer](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor) und höhere Rollen verfügen über diese Berechtigung.

Um „Befehl ausführen“ zu verwenden, können Sie eine der [integrierten](../../role-based-access-control/built-in-roles.md) Rollen verwenden oder eine [benutzerdefinierte](../../role-based-access-control/custom-roles.md) Rolle erstellen.

## <a name="next-steps"></a>Nächste Schritte

Informationen zu anderen Möglichkeiten für die Remoteausführung von Skripts und Befehlen in Ihrer VM finden Sie unter [Ausführen von Skripts in Ihrer Windows-VM](run-scripts-in-vm.md).
