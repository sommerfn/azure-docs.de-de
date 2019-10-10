---
title: Hinzufügen von Azure Automation-Runbooks zu Site Recovery-Wiederherstellungsplänen
description: Erfahren Sie, wie Sie Wiederherstellungspläne mit Azure Automation für die Notfallwiederherstellung mit Azure Site Recovery erweitern können.
author: rajani-janaki-ram
manager: gauravd
ms.service: site-recovery
ms.topic: conceptual
ms.date: 09/18/2019
ms.author: rajanaki
ms.openlocfilehash: ecfe993a137ca63c84438870ec54ac1e6d6707da
ms.sourcegitcommit: 42748f80351b336b7a5b6335786096da49febf6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2019
ms.locfileid: "72173493"
---
# <a name="add-azure-automation-runbooks-to-recovery-plans"></a>Hinzufügen von Azure Automation-Runbooks zu Wiederherstellungsplänen

In diesem Artikel wird beschrieben, wie Sie Azure Automation-Runbooks integrieren, um Wiederherstellungspläne für [Azure Site Recovery](site-recovery-overview.md) zu erweitern. Wir zeigen Ihnen, wie Sie einfache Tasks automatisieren, für die zuvor ein manueller Eingriff erforderlich war, und eine aus mehreren Schritten bestehende Wiederherstellung in eine Aktion konvertieren, die sich mit nur einem Klick durchführen lässt.

## <a name="recovery-plans"></a>Wiederherstellungspläne 

Sie können Wiederherstellungspläne verwenden, wenn Sie ein Failover für lokale Computer oder Azure-VMs durchführen. Mithilfe von Wiederherstellungsplänen können Sie einen systematischen Wiederherstellungsprozess definieren, der vorgibt, wie Computer Failover durchführen und nach einem Failover gestartet und wiederhergestellt werden. 

Die Wiederherstellung großer Apps kann komplex sein. Darüber hinaus tragen Wiederherstellungspläne zu einem geregelten Prozess bei, sodass die Wiederherstellung immer exakt, wiederholbar und automatisiert ist. Sie können Tasks innerhalb eines Wiederherstellungsplans mithilfe von Skripts und Azure Automation-Runbooks automatisieren. Typische Beispiele sind u. a. das Konfigurieren von Einstellungen auf einer Azure-VM nach einem Failover oder das Neukonfigurieren einer App, die auf der VM ausgeführt wird.

- [hier](recovery-plan-overview.md) .
- [Erfahren Sie mehr](../automation/automation-runbook-types.md) zu Azure Automation-Runbooks.



## <a name="runbooks-in-recovery-plans"></a>Runbooks in Wiederherstellungsplänen

Sie können einem Wiederherstellungsplan ein Azure Automation-Konto und Runbooks hinzufügen. Das Runbook wird aufgerufen, wenn der Wiederherstellungsplan ausgeführt wird.

- Ein Automation-Konto kann sich in einer beliebigen Azure-Region befinden, aber es muss unter demselben Abonnement wie der Site Recovery-Tresor laufen. 
- Ein Runbook kann während eines Failovers von einem primären auf einen sekundären Standort oder beim Failback vom sekundären auf den primären Standort in einem Wiederherstellungsplan ausgeführt werden.
- Runbooks in einem Wiederherstellungsplan werden nacheinander in einer festgelegten Reihenfolge ausgeführt.
- Wenn Runbooks in einem Wiederherstellungsplan VMs so konfigurieren, dass sie in unterschiedlichen Gruppen gestartet werden, wird der Wiederherstellungsplan nur fortgesetzt, wenn Azure meldet, dass alle VMs ausgeführt werden.
- Auch wenn ein Skript fehlschlägt, werden die Wiederherstellungspläne weiter ausgeführt.

### <a name="recovery-plan-context"></a>Kontext für Wiederherstellungspläne

Wenn ein Skript ausgeführt wird, wird ein Kontext für den Wiederherstellungsplan zum Runbook hinzugefügt. Der Kontext enthält die Variablen, die in der folgenden Tabelle zusammengefasst sind.

| **Variablenname** | **Beschreibung** |
| --- | --- |
| RecoveryPlanName |Name des Wiederherstellungsplans. Wird in Aktionen verwendet, die auf dem Namen basieren. |
| FailoverType |Gibt an, ob es sich um ein Test- oder Produktionsfailover handelt. 
| FailoverDirection | Gibt an, ob die Wiederherstellung auf einem primären oder sekundären Standort durchgeführt wird. |
| GroupID |Identifiziert die Gruppennummer innerhalb des Wiederherstellungsplans, wenn der Plan ausgeführt wird. |
| VmMap |Ein Array mit allen VMs der Gruppe. |
| VMMap-Schlüssel |Ein eindeutiger Schlüssel (GUID) für jeden virtuellen Computer. |
| SubscriptionId |Die ID des Azure-Abonnements, unter dem die VM erstellt wurde. |
| ResourceGroupName | Name der Ressourcengruppe, in der sich die VM befindet.
| CloudServiceName |Der Name des Azure-Clouddiensts, unter dem die VM erstellt wurde. |
| RoleName |Der Name der Azure-VM. |
| RecoveryPointId|Der Zeitstempel für die Wiederherstellung der VM. |

Im folgenden Beispiel wird eine Kontextvariable angezeigt:

```
{"RecoveryPlanName":"hrweb-recovery",
"FailoverType":"Test",
"FailoverDirection":"PrimaryToSecondary",
"GroupId":"1",
"VmMap":{"7a1069c6-c1d6-49c5-8c5d-33bfce8dd183":
    { "SubscriptionId":"7a1111111-c1d6-49c5-8c5d-111ce8dd183",
    "ResourceGroupName":"ContosoRG",
    "CloudServiceName":"pod02hrweb-Chicago-test",
    "RoleName":"Fabrikam-Hrweb-frontend-test",
    "RecoveryPointId":"TimeStamp"}
    }
}
```

Wenn Sie auf alle VMs in VMMap in einer Schleife zugreifen möchten, können Sie den folgenden Code verwenden:

```
$VMinfo = $RecoveryPlanContext.VmMap | Get-Member | Where-Object MemberType -EQ NoteProperty | select -ExpandProperty Name
$vmMap = $RecoveryPlanContext.VmMap
    foreach($VMID in $VMinfo)
    {
        $VM = $vmMap.$VMID                
            if( !(($VM -eq $Null) -Or ($VM.ResourceGroupName -eq $Null) -Or ($VM.RoleName -eq $Null))) {
            #this check is to ensure that we skip when some data is not available else it will fail
    Write-output "Resource group name ", $VM.ResourceGroupName
    Write-output "Rolename " = $VM.RoleName
            }
        }
```


In Aman Sharmas Blog auf [Harvesting Clouds](http://harvestingclouds.com) finden Sie ein nützliches Beispiel für ein [Kontextskript für einen Wiederherstellungsplan](http://harvestingclouds.com/post/script-sample-azure-automation-runbook-for-asr-recovery-plan/).



## <a name="before-you-start"></a>Vorbereitung

- Wenn Sie mit Azure Automation noch nicht vertraut sind, können Sie sich [registrieren](https://azure.microsoft.com/services/automation/) und [Beispielskripts herunterladen](https://azure.microsoft.com/documentation/scripts/).
- Stellen Sie sicher, dass das Automation-Konto über die folgenden Module verfügt:
    - AzureRM.profile
    - AzureRM.Resources
    - AzureRM.Automation
    - AzureRM.Network
    - AzureRM.Compute

    Alle Module sollten auf kompatiblen Versionen basieren. Am einfachsten ist es, immer die neuste Version aller Module zu verwenden.



## <a name="customize-the-recovery-plan"></a>Anpassen des Wiederherstellungsplans

1. Wählen Sie im Tresor **Recovery Plans (Site Recovery)** (Wiederherstellungspläne (Site Recovery)) aus.
2. Wenn Sie einen Wiederherstellungsplan erstellen möchten, klicken Sie auf **+Wiederherstellungsplan**. [Weitere Informationen](site-recovery-create-recovery-plans.md) Wenn Sie bereits über einen Wiederherstellungsplan verfügen, öffnen Sie diesen.
3. Klicken Sie auf der Seite „Wiederherstellungsplan“ auf **Anpassen**.

    ![Klicken auf die Schaltfläche „Anpassen“](media/site-recovery-runbook-automation-new/custom-rp.png)

2. Klicken Sie auf die Auslassungspunkte (...) neben **Gruppe 1: Starten** > **Nachfolgende Aktion hinzufügen**.
3. Überprüfen Sie unter **Aktion einfügen**, ob die Option **Skript** ausgewählt ist, und geben Sie einen Namen für das Skript an (**Hallo Welt**).
4. Geben Sie ein Automation-Konto an, und wählen Sie ein Runbook aus. Klicken Sie auf **OK**, um das Skript zu speichern. Das Skript wird **Gruppe 1: Schritte danach** hinzugefügt.


## <a name="reuse-a-runbook-script"></a>Wiederverwenden eines Runbookskripts

Sie können ein einzelnes Runbookskript in mehreren Wiederherstellungsplänen nutzen, indem Sie externe Variablen verwenden. 

- Sie können [Azure Automation-Variablen](../automation/automation-variables.md) zum Speichern von Parametern für die Ausführung eines Wiederherstellungsplans verwenden.
- Indem Sie der Variablen den Namen des Wiederherstellungsplans als Präfix hinzufügen, können Sie für jeden Wiederherstellungsplan einzelne Variablen erstellen. Verwenden Sie die Variablen anschließend als Parameter.
- Sie können einen Parameter ändern, ohne das Skript zu ändern, und für die Funktionsweise des Skripts trotzdem eine Änderung erzielen.

### <a name="use-a-simple-string-variable-in-a-runbook-script"></a>Verwenden einer einfachen Zeichenfolgenvariablen in einem Runbookskript

In diesem Beispiel wendet ein Skript die Eingabe einer Netzwerksicherheitsgruppe (NSG) auf die VMs eines Wiederherstellungsplans an. 

1. Verwenden Sie den Kontext für den Wiederherstellungsplan, damit das Skript erkennt, welcher Wiederherstellungsplan ausgeführt wird:

    ```
    workflow AddPublicIPAndNSG {
        param (
              [parameter(Mandatory=$false)]
              [Object]$RecoveryPlanContext
        )

        $RPName = $RecoveryPlanContext.RecoveryPlanName
    ```

2. Notieren Sie sich den NSG-Namen und die Ressourcengruppe. Verwenden Sie diese Variablen als Eingaben für Skripts für Wiederherstellungspläne. 
1. Erstellen Sie in den Objekten des Automation-Kontos eine Variable zum Speichern des NSG-Namens. Fügen Sie dem Variablennamen ein Präfix hinzu, indem Sie den Namen des Wiederherstellungsplans verwenden.

    ![Erstellen einer Variablen für den NSG-Namen](media/site-recovery-runbook-automation-new/var1.png)

2. Erstellen Sie eine Variable, um den Namen der Ressourcengruppe für die NSG-Ressource zu speichern. Fügen Sie dem Variablennamen ein Präfix hinzu, indem Sie den Namen des Wiederherstellungsplans verwenden.

    ![Erstellen des Namens einer NSG-Ressourcengruppe](media/site-recovery-runbook-automation-new/var2.png)


3.  Verwenden Sie im Skript den folgenden Referenzcode, um die Variablenwerte abzurufen:

    ```
    $NSGValue = $RecoveryPlanContext.RecoveryPlanName + "-NSG"
    $NSGRGValue = $RecoveryPlanContext.RecoveryPlanName + "-NSGRG"

    $NSGnameVar = Get-AutomationVariable -Name $NSGValue
    $RGnameVar = Get-AutomationVariable -Name $NSGRGValue
    ```

4.  Verwenden Sie die Variablen im Runbook, um die NSG auf die Netzwerkschnittstelle der VM anzuwenden, für den das Failover durchgeführt wurde:

    ```
    InlineScript {
    if (($Using:NSGname -ne $Null) -And ($Using:NSGRGname -ne $Null)) {
            $NSG = Get-AzureRmNetworkSecurityGroup -Name $Using:NSGname -ResourceGroupName $Using:NSGRGname
            Write-output $NSG.Id
            #Apply the NSG to a network interface
            #$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet
            #Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name FrontEnd `
            #  -AddressPrefix 192.168.1.0/24 -NetworkSecurityGroup $NSG
        }
    }
    ```


Erstellen Sie für jeden Wiederherstellungsplan unabhängige Variablen, damit Sie das Skript wiederverwenden können. Fügen Sie ein Präfix hinzu, indem Sie den Namen des Wiederherstellungsplans verwenden. 

Ein umfassendes End-to-End-Skript für dieses Szenario finden Sie [hier](https://gallery.technet.microsoft.com/Add-Public-IP-and-NSG-to-a6bb8fee).


### <a name="use-a-complex-variable-to-store-more-information"></a>Verwenden einer komplexen Variablen zum Speichern von weiteren Informationen

In einigen Szenarios können Sie möglicherweise keine separaten Variablen für jeden Wiederherstellungsplan erstellen. Stellen Sie sich ein Szenario vor, bei dem mit nur einem Skript für bestimmte VMs eine öffentliche IP-Adresse zugewiesen werden soll. In einem anderen Szenario kann es sein, dass Sie verschiedene NSGs unterschiedlichen VMs (nicht auf allen VMs) zuordnen möchten. Beachten Sie Folgendes:

- Sie können ein Skript erstellen, das für alle Wiederherstellungspläne wiederverwendet werden kann.
- Jeder Wiederherstellungsplan kann über eine variable Anzahl von VMs verfügen.
- Eine SharePoint-Wiederherstellung verfügt über zwei Front-Ends. Eine einfache Branchenanwendung (LOB-Anwendung) hat nur ein Front-End.
- In diesem Szenario können Sie nicht für jeden Wiederherstellungsplan separate Variablen erstellen.

Im folgenden Beispiel erstellen wie eine [komplexe Variable](https://docs.microsoft.com/powershell/module/servicemanagement/azure/set-azureautomationvariable) im Azure Automation-Konto.

Hierzu geben wir mithilfe von Azure PowerShell mehrere Werte an.

1. Melden Sie sich in PowerShell bei Ihrem Azure-Abonnement an:

    ```
    Connect-AzureRmAccount
    $sub = Get-AzureRmSubscription -Name <SubscriptionName>
    $sub | Select-AzureRmSubscription
    ```

2. Erstellen Sie zum Speichern der Parameter die komplexe Variable, indem Sie den Namen des Wiederherstellungsplans verwenden:

    ```
    $VMDetails = @{"VMGUID"=@{"ResourceGroupName"="RGNameOfNSG";"NSGName"="NameOfNSG"};"VMGUID2"=@{"ResourceGroupName"="RGNameOfNSG";"NSGName"="NameOfNSG"}}
        New-AzureRmAutomationVariable -ResourceGroupName <RG of Automation Account> -AutomationAccountName <AA Name> -Name <RecoveryPlanName> -Value $VMDetails -Encrypted $false
    ```

3. In dieser komplexen Variablen ist **VMDetails** die VM-ID für die geschützte VM. Zeigen Sie im Azure-Portal die VM-Eigenschaften an, um die VM-ID zu erhalten. Im folgenden Screenshot ist eine Variable dargestellt, in der die Details von zwei VMs gespeichert werden:

    ![Verwenden der VM-ID als GUID](media/site-recovery-runbook-automation-new/vmguid.png)

4. Verwenden Sie diese Variable in Ihrem Runbook. Wenn sich die angegebene VM-GUID im Kontext des Wiederherstellungsplans befindet, können Sie die NSG auf die VM anwenden:

    ```
    $VMDetailsObj = (Get-AutomationVariable -Name $RecoveryPlanContext.RecoveryPlanName).ToObject([hashtable])
    ```

4. Führen Sie in Ihrem Runbook eine Schleife durch die VMs des Wiederherstellungsplan-Kontexts durch. Überprüfen Sie, ob die VM in **$VMDetailsObj** vorhanden ist. Wenn ja, greifen Sie auf die Eigenschaften der Variablen zu, um die NSG anzuwenden:

    ```
        $VMinfo = $RecoveryPlanContext.VmMap | Get-Member | Where-Object MemberType -EQ NoteProperty | select -ExpandProperty Name
        $vmMap = $RecoveryPlanContext.VmMap

        foreach($VMID in $VMinfo) {
            $VMDetails = $VMDetailsObj[$VMID].ToObject([hashtable]);
            Write-output $VMDetails
            if ($VMDetails -ne $Null) { #If the VM exists in the context, this will not be Null
                $VM = $vmMap.$VMID
                # Access the properties of the variable
                $NSGname = $VMDetails.NSGName
                $NSGRGname = $VMDetails.NSGResourceGroupName

                # Add code to apply the NSG properties to the VM
            }
        }
    ```

Sie können dasselbe Skript für unterschiedliche Wiederherstellungspläne verwenden. Geben Sie unterschiedliche Parameter ein, indem Sie den Wert, der einem Wiederherstellungsplan entspricht, in unterschiedlichen Variablen speichern.

## <a name="sample-scripts"></a>Beispielskripts

Klicken Sie auf die Schaltfläche **Deploy to Azure**, um Beispielskripts für Ihr Automation-Konto bereitzustellen.

[![In Azure bereitstellen](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/c4803408-340e-49e3-9a1f-0ed3f689813d.png)](https://aka.ms/asr-automationrunbooks-deploy)

Dieses Video enthält ein weiteres Beispiel. Es wird veranschaulicht, wie Sie eine WordPress-Anwendung mit zwei Ebenen in Azure wiederherstellen:


> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/One-click-failover-of-a-2-tier-WordPress-application-using-Azure-Site-Recovery/player]


## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu einem [Ausführenden Azure Automation-Konto](../automation/automation-create-runas-account.md)
- Sehen Sie sich die [Beispielskripts für Azure Automation an](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=User&f%5B0%5D.Value=SC%20Automation%20Product%20Team&f%5B0%5D.Text=SC%20Automation%20Product%20Team).
- [Weitere Informationen](site-recovery-failover.md) zum Ausführen von Failovern



