---
title: Massenregistrierung von SQL-VMs in Azure beim SQL-VM-Ressourcenanbieter | Microsoft-Dokumentation
description: Durch eine Massenregistrierung von SQL Server-VMs beim SQL-VM-Ressourcenanbieter können Sie die Verwaltbarkeit zu verbessern.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
tags: azure-resource-manager
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 10/21/2019
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 97541484501a3ecdd1bd5998314c1ee9e7a4e3a5
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73489073"
---
# <a name="bulk-register-sql-virtual-machines-in-azure-with-the-sql-vm-resource-provider"></a>Massenregistrierung von SQL-VMs in Azure beim SQL-VM-Ressourcenanbieter

In diesem Artikel wird beschrieben, wie Sie SQL Server-VMs in Azure mit dem PowerShell-Cmdlet `Register-SqlVMs` beim SQL-VM-Ressourcenanbieter registrieren.

Mit dem Cmdlet `Register-SqlVMs` können alle virtuellen Computer in einer bestimmten Liste von Abonnements, Ressourcengruppen oder einer Liste mit bestimmten virtuellen Computern registriert werden. Mit dem Cmdlet werden die virtuellen Computer im _Lightweight_-Verwaltungsmodus registriert sowie anschließend [ein Bericht und eine Protokolldatei](#output-description) erstellt. 

Der Registrierungsprozess birgt kein Risiko, weist keine Ausfallzeiten auf und startet SQL Server oder den virtuellen Computer nicht neu. 

Weitere Informationen zum Ressourcenanbieter finden Sie unter [SQL-VM-Ressourcenanbieter](virtual-machines-windows-sql-register-with-resource-provider.md). 

## <a name="prerequisites"></a>Voraussetzungen

Zum Registrieren der SQL Server-VM beim Ressourcenanbieter benötigen Sie Folgendes: 

- Ein [Azure-Abonnement](https://azure.microsoft.com/free/), das [beim Ressourcenanbieter registriert](virtual-machines-windows-sql-register-with-resource-provider.md#register-subscription-with-rp) wurde und nicht registrierte SQL Server-VMs enthält. 
- Die Clientanmeldeinformationen, die zum Registrieren der virtuellen Computer verwendet werden, sind in einer der folgenden RBAC-Rollen vorhanden: **Mitwirkender von virtuellen Computern**, **Mitwirkender** oder **Besitzer**. 
- Die neueste Version von [Az PowerShell](/powershell/azure/new-azureps-module-az). 
- Die neueste Version von [Az.SqlVirtualMachine](https://www.powershellgallery.com/packages/Az.SqlVirtualMachine/0.1.0).

## <a name="getting-started"></a>Erste Schritte

Bevor Sie fortfahren, müssen Sie zuerst eine lokale Kopie des Skripts erstellen, die Kopie als PowerShell-Modul importieren und eine Verbindung mit Azure herstellen. 

### <a name="create-script"></a>Erstellen des Skripts

Zum Erstellen des Skripts kopieren Sie das [vollständige Skript](#full-script) am Ende dieses Artikels und speichern Sie es lokal als `RegisterSqlVMs.psm1`. 

### <a name="import-script"></a>Importieren des Skripts

Nachdem das Skript erstellt wurde, können Sie es als Modul im PowerShell-Terminal importieren. 

Öffnen Sie ein PowerShell-Verwaltungsterminal und navigieren Sie zum Speicherort der `RegisterSqlVMs.psm1`-Datei. Führen Sie dann das folgende PowerShell-Cmdlet aus, um das Skript als Modul zu importieren: 

```powershell-interactive
Import-Module .\RegisterSqlVMs.psm1
```

### <a name="connect-to-azure"></a>Herstellen einer Verbindung mit Azure

Verwenden Sie das folgende PowerShell-Cmdlet, um eine Verbindung zu Azure herzustellen:

```powershell-interactive
Connect-AzAccount
```


## <a name="all-vms-in-list-of-subscriptions"></a>Alle VMs in der Liste der Abonnements 

Verwenden Sie das folgende Cmdlet, um alle SQL Server-VMs in einer Liste der Abonnements zu registrieren:

```powershell-interactive
Register-SqlVMs -SubscriptionList SubscriptionId1,SubscriptionId2
```

Beispielausgabe: 

```
Number of Subscriptions registration failed for 
because you do not have access or credentials are wrong: 1
Total VMs Found: 10
VMs Already registered: 1
Number of VMs registered successfully: 4
Number of VMs failed to register due to error: 1
Number of VMs skipped as VM or the guest agent on VM is not running: 3
Number of VMs skipped as they are not running SQL Server On Windows: 1

Please find the detailed report in file RegisterSqlVMScriptReport1571314821.txt
Please find the error details in file VMsNotRegisteredDueToError1571314821.log
```

## <a name="all-vms-in-a-single-subscription"></a>Alle VMs in einem einzelnen Abonnement

Verwenden Sie das folgende Cmdlet, um alle SQL Server-VMs in einem einzelnen Abonnement zu registrieren: 

```powershell-interactive
Register-SqlVMs -Subscription SubscriptionId1
```

Beispielausgabe:

```
Total VMs Found: 10
VMs Already registered: 1
Number of VMs registered successfully: 5
Number of VMs failed to register due to error: 1
Number of VMs skipped as VM or the  guest agent on VM is not running: 2
Number of VMs skipped as they are not running SQL Server On Windows: 1

Please find the detailed report in file RegisterSqlVMScriptReport1571314821.txt
Please find the error details in file VMsNotRegisteredDueToError1571314821.log
```

## <a name="all-vms-in-multiple-resource-groups"></a>Alle VMs in mehreren Ressourcengruppen

Verwenden Sie das folgende Cmdlet, um alle SQL Server-VMs in mehreren Ressourcengruppen in einem einzelnen Abonnement zu registrieren:

```powershell-interactive
Register-SqlVMs -Subscription SubscriptionId1 -ResourceGroupList ResourceGroup1,ResourceGroup2
```

Beispielausgabe:

```
Total VMs Found: 4
VMs Already registered: 1
Number of VMs registered successfully: 1
Number of VMs failed to register due to error: 1
Number of VMs skipped as they are not running SQL Server On Windows: 1

Please find the detailed report in file RegisterSqlVMScriptReport1571314821.txt
Please find the error details in file VMsNotRegisteredDueToError1571314821.log
```

## <a name="all-vms-in-a-resource-group"></a>Alle VMs in einer Ressourcengruppe

Verwenden Sie das folgende Cmdlet, um alle SQL Server-VMs in einer einzelnen Ressourcengruppe zu registrieren: 

```powershell-interactive
Register-SqlVMs -Subscription SubscriptionId1 -ResourceGroupName ResourceGroup1
```

Beispielausgabe:

```
Total VMs Found: 4
VMs Already registered: 1
Number of VMs registered successfully: 1
Number of VMs failed to register due to error: 1
Number of VMs skipped as VM or the guest agent on VM is not running: 1

Please find the detailed report in file RegisterSqlVMScriptReport1571314821.txt
Please find the error details in file VMsNotRegisteredDueToError1571314821.log
```

## <a name="specific-vms-in-single-resource-group"></a>Bestimmte VMs in einer einzelnen Ressourcengruppe

Verwenden Sie das folgende Cmdlet, um bestimmte SQL Server-VMs in einer einzelnen Ressourcengruppe zu registrieren:

```powershell-interactive
Register-SqlVMs -Subscription SubscriptionId1 -ResourceGroupName ResourceGroup1 -VmList VM1,VM2,VM3
```

Beispielausgabe:

```
Total VMs Found: 3
VMs Already registered: 0
Number of VMs registered successfully: 1
Number of VMs skipped as VM or the guest agent on VM is not running: 1
Number of VMs skipped as they are not running SQL Server On Windows: 1

Please find the detailed report in file RegisterSqlVMScriptReport1571314821.txt
Please find the error details in file VMsNotRegisteredDueToError1571314821.log
```

## <a name="specific-vm"></a>Bestimmte VM

Verwenden Sie das folgende Cmdlet, um eine bestimmte SQL Server-VM zu registrieren: 

```powershell-interactive
Register-SqlVMs -Subscription SubscriptionId1 -ResourceGroupName ResourceGroup1 -Name VM1
```

Beispielausgabe:

```
Total VMs Found: 1
VMs Already registered: 0
Number of VMs registered successfully: 1

Please find the detailed report in  file RegisterSqlVMScriptReport1571314821.txt
```


## <a name="output-description"></a>Ausgabebeschreibung

Bei jeder Verwendung des `Register-SqlVMs`-Cmdlets werden ein Bericht und eine Protokolldatei erstellt. 

### <a name="report"></a>Bericht

Der Bericht wird als `.txt`-Datei mit dem Namen `RegisterSqlVMScriptReport<Timestamp>.txt` erstellt, wobei der Zeitstempel dem Zeitpunkt entspricht, an dem das Cmdlet gestartet wurde. Im Bericht werden die folgenden Details aufgelistet:

| **Ausgabewert** | **Beschreibung** |
| :--------------  | :-------------- | 
| Anzahl der Abonnements, für die die Registrierung fehlgeschlagen ist, weil Sie über keine Zugriffsrechte verfügen oder die Anmeldeinformationen falsch sind | Stellt die Anzahl und die Liste der Abonnements zur Verfügung, bei denen mit der angegebenen Authentifizierung Probleme aufgetreten sind. Die ausführliche Fehlermeldung finden Sie in der Protokolldatei, indem Sie nach der Abonnement-ID suchen. | 
| Anzahl der Abonnements, die nicht geprüft werden konnten, weil sie nicht beim Ressourcenanbieter registriert sind | Dieser Abschnitt enthält die Anzahl und Liste der Abonnements, die nicht beim SQL-VM-Ressourcenanbieter registriert wurden. |
| Gesamtzahl der gefundenen VMs | Die Anzahl der virtuellen Computer, die im Bereich der an das Cmdlet übergegebenen Parameter gefunden wurden. | 
| Bereits registrierte VMs | Die Anzahl der virtuellen Computer, die übersprungen wurden, da sie bereits beim Ressourcenanbieter registriert waren. |
| Anzahl der erfolgreich registrierten VMs | Die Anzahl der virtuellen Computer, die nach der Ausführung des Cmdlets erfolgreich registriert wurden. Listet die registrierten virtuellen Computer im Format `SubscriptionID, Resource Group, Virtual Machine` auf. | 
| Anzahl der VMs, die aufgrund eines Fehlers nicht registriert werden konnten | Anzahl der virtuellen Computer, die aufgrund eines Fehlers nicht registriert werden konnten. Die Fehlerdetails finden Sie in der Protokolldatei. | 
| Anzahl der VMs, die übersprungen wurden, weil die VM oder der Gast-Agent in der VM nicht ausgeführt wird | Anzahl und Liste der virtuellen Computer, die nicht registriert werden konnten, weil der virtuelle Computer oder der Gast-Agent auf dem virtuellen Computer nicht ausgeführt wird. Diese können erneut geprüft werden, nachdem der virtuelle Computer oder der Gast-Agent gestartet wurde. Details finden Sie in der Protokolldatei. |
| Anzahl der virtuellen Computer, die übersprungen wurden, weil auf ihnen nicht SQL Server unter Windows ausgeführt wird | Anzahl der virtuellen Computer, die übersprungen wurden, weil auf ihnen nicht SQL Server ausgeführt wird oder weil sie keine Windows-VM sind. Die virtuellen Computer werden im Format `SubscriptionID, Resource Group, Virtual Machine` aufgelistet. | 
| &nbsp; | &nbsp; |

### <a name="log"></a>Protokoll 

Fehler werden in der Protokolldatei mit dem Namen `VMsNotRegisteredDueToError<Timestamp>.log` protokolliert, wobei der Zeitstempel der Zeitpunkt ist, an dem das Skript gestartet wurde. Wenn der Fehler auf Abonnementebene liegt, enthält das Protokoll die kommagetrennte Abonnement-ID und die Fehlermeldung. Wenn der Fehler bei der Registrierung des virtuellen Computers aufgetreten ist, enthält das Protokoll die Abonnement-ID, den Ressourcengruppennamen, den VM-Namen, den Fehlercode und die Fehlermeldung mit Kommas als Trennzeichen. 

## <a name="remarks"></a>Anmerkungen

Wenn Sie SQL Server-VMs mithilfe des bereitgestellten Skripts beim Ressourcenanbieter registrieren, müssen Sie Folgendes beachten:

- Für die Registrierung beim Ressourcenanbieter muss ein Gast-Agent auf der SQL Server-VM ausgeführt werden. Windows Server 2008-Images verfügen über keinen Gast-Agent, sodass diese virtuellen Computer fehlschlagen und manuell im [NoAgent-Verwaltungsmodus](virtual-machines-windows-sql-register-with-resource-provider.md#register-sql-server-2008-or-2008-r2-on-windows-server-2008-vms) registriert werden müssen.
- Es ist eine integrierte Wiederholungslogik vorhanden, um offensichtliche Fehler zu überwinden. Wenn die virtuelle Maschine erfolgreich registriert wurde, ist dies ein schneller Vorgang. Wenn die Registrierung jedoch fehlschlägt, wird jeder virtuelle Computer erneut geprüft.  Daher sollten Sie eine ausreichende Zeit für die Durchführung des Registrierungsprozesses zulassen. Die tatsächlich erforderliche Zeit hängt jedoch vom Typ und von der Anzahl der Fehler ab. 

## <a name="full-script"></a>Vollständiges Skript

Das vollständige Skript auf GitHub finden Sie unter [Massenregistrierung von SQL-VMs mit Az PowerShell](https://github.com/Azure/azure-docs-powershell-samples/blob/master/sql-virtual-machine/register-sql-vms/RegisterSqlVMs.psm1). 

Kopieren Sie das vollständige Skript und speichern Sie es als `RegisterSqLVMs.psm1`.

[!code-powershell-interactive[main](../../../../powershell_scripts/sql-virtual-machine/register-sql-vms/RegisterSqlVMs.psm1 "Bulk register SQL Server virtual machines")]



## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in den folgenden Artikeln: 

* [Übersicht über SQL Server auf einem virtuellen Windows-Computer](virtual-machines-windows-sql-server-iaas-overview.md)
* [Häufig gestellte Fragen zu SQL Server auf virtuellen Windows-Computern](virtual-machines-windows-sql-server-iaas-faq.md)
* [Preisinformationen für SQL Server auf virtuellen Windows-Computern](virtual-machines-windows-sql-server-pricing-guidance.md)
* [SQL Server auf Windows-VMs – Versionshinweise](virtual-machines-windows-sql-server-iaas-release-notes.md)
