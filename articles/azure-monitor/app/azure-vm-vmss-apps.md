---
title: Überwachen der Leistung von Anwendungen auf Azure-VMs und in Azure-VM-Skalierungsgruppen | Microsoft-Dokumentation
description: Leistungsüberwachung für Anwendungen auf Azure-VMs und in Azure-VM-Skalierungsgruppen. Ladezeit für Diagramme und Antwortzeit, Informationen zu den Abhängigkeiten und Festlegen von Benachrichtigungen zur Leistung.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.topic: conceptual
ms.date: 06/27/2019
ms.author: mbullwin
ms.openlocfilehash: f2c6b98fd0be2061e9d8cab5c063cafadf71476a
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2019
ms.locfileid: "68597457"
---
# <a name="monitor-application-performance-hosted-on-azure-vm-and-azure-virtual-machine-scale-sets"></a>Überwachen der Leistung von Anwendungen auf Azure-VMs und in Azure-VM-Skalierungsgruppen

Die Überwachung der .NET-basierten Webanwendungen auf [Azure-VMs](https://azure.microsoft.com/services/virtual-machines/) und in [Azure-VM-Skalierungsgruppen](https://docs.microsoft.com/azure/virtual-machine-scale-sets/) lässt sich jetzt einfacher denn je aktivieren. Profitieren Sie von allen Vorteilen der Verwendung von Application Insights, ohne den Code zu ändern.

Dieser Artikel führt Sie Schritt für Schritt durch das Aktivieren der Application Insights-Überwachung mithilfe der Erweiterung ApplicationMonitoringWindows und bietet eine vorläufige Anleitung zur Automatisierung des Prozesses für umfangreiche Bereitstellungen.

> [!IMPORTANT]
> Die Azure-Erweiterung ApplicationMonitoringWindows ist zurzeit als Public Preview verfügbar.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Einige Features werden möglicherweise nicht unterstützt oder bieten möglicherweise nur eingeschränkte Funktionen.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="enable-application-insights"></a>Aktivieren von Application Insights

Es gibt zwei Methoden, um die Überwachung von Anwendungen zu aktivieren, die auf Azure-VMs und in Azure-VM-Skalierungsgruppen gehostet werden:

* **Agent-basierte Anwendungsüberwachung** (Erweiterung ApplicationMonitoringWindows).
    * Diese Methode stellt die einfachste Möglichkeit der Aktivierung dar und erfordert keine erweiterte Konfiguration. Sie wird häufig als „Laufzeitüberwachung“ bezeichnet. Für Azure-VMs und Azure-VM-Skalierungsgruppen wird empfohlen, mindestens diese Überwachungsstufe zu aktivieren. Danach können Sie basierend auf dem jeweiligen Szenario überprüfen, ob eine manuelle Instrumentierung erforderlich ist.
    * Zurzeit werden nur IIS-gehostete .NET-Anwendungen unterstützt.

* **Manuelles Instrumentieren der Anwendung über Code** durch Installieren des Application Insights SDK.

    * Dieser Ansatz ermöglicht eine wesentlich stärkere Anpassung, erfordert jedoch das [Hinzufügen einer Abhängigkeit von den Application Insights SDK-NuGet-Paketen](https://docs.microsoft.com/azure/azure-monitor/app/asp-net). Diese Methode bedeutet auch, dass Sie die Updates auf die neueste Version der Pakete selbst verwalten müssen.

    * Wenn Sie benutzerdefinierte API-Aufrufe zum Nachverfolgen von Ereignissen/Abhängigkeiten ausführen müssen, die bei der Agent-basierten Überwachung nicht standardmäßig erfasst werden, müssen Sie diese Methode verwenden. Weitere Informationen finden Sie im Artikel zur [API für benutzerdefinierte Ereignisse und Metriken](https://docs.microsoft.com/azure/azure-monitor/app/api-custom-events-metrics).

> [!NOTE]
> Wenn sowohl die Agent-basierte Überwachung als auch die manuelle SDK-basierte Instrumentierung erkannt wird, werden nur die Einstellungen der manuellen Instrumentierung berücksichtigt. Dadurch wird verhindert, dass doppelte Daten gesendet werden. Weitere Informationen dazu finden Sie im [Abschnitt zur Problembehandlung](#troubleshooting) weiter unten.

## <a name="manage-agent-based-monitoring-for-net-applications-on-vm-using-powershell"></a>Verwalten Agent-basierter Überwachung für .NET-Anwendungen auf einer VM mithilfe von PowerShell

Installieren oder Aktualisieren der Erweiterung zur Anwendungsüberwachung für die VM
```powershell
$publicCfgJsonString = '
{
  "RedfieldConfiguration": {
    "InstrumentationKeyMap": {
      "Filters": [
        {
          "AppFilter": ".*",
          "MachineFilter": ".*",
          "InstrumentationSettings" : {
            "InstrumentationKey": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
          }
        }
      ]
    }
  }
}
';
$privateCfgJsonString = '{}';

Set-AzVMExtension -ResourceGroupName "<myVmResourceGroup>" -VMName "<myVmName>" -Location "South Central US" -Name "ApplicationMonitoring" -Publisher "Microsoft.Azure.Diagnostics" -Type "ApplicationMonitoringWindows" -Version "2.8" -SettingString $publicCfgJsonString -ProtectedSettingString $privateCfgJsonString
```

Deinstallieren der Erweiterung zur Anwendungsüberwachung von der VM
```powershell
Remove-AzVMExtension -ResourceGroupName "<myVmResourceGroup>" -VMName "<myVmName>" -Name "ApplicationMonitoring"
```

Abfragen des Status der Erweiterung zur Anwendungsüberwachung für die VM
```powershell
Get-AzVMExtension -ResourceGroupName "<myVmResourceGroup>" -VMName "<myVmName>" -Name ApplicationMonitoring -Status
```

Abrufen der Liste der installierten Erweiterungen für die VM
```powershell
Get-AzResource -ResourceId "/subscriptions/<mySubscriptionId>/resourceGroups/<myVmResourceGroup>/providers/Microsoft.Compute/virtualMachines/<myVmName>/extensions"

# Name              : ApplicationMonitoring
# ResourceGroupName : <myVmResourceGroup>
# ResourceType      : Microsoft.Compute/virtualMachines/extensions
# Location          : southcentralus
# ResourceId        : /subscriptions/<mySubscriptionId>/resourceGroups/<myVmResourceGroup>/providers/Microsoft.Compute/virtualMachines/<myVmName>/extensions/ApplicationMonitoring
```

## <a name="manage-agent-based-monitoring-for-net-applications-on-azure-virtual-machine-scale-set-using-powershell"></a>Verwalten Agent-basierter Überwachung für .NET-Anwendungen in einer Azure-VM-Skalierungsgruppe mithilfe von PowerShell

Installieren oder Aktualisieren der Erweiterung zur Anwendungsüberwachung für die Azure-VM-Skalierungsgruppe
```powershell
$publicCfgHashtable =
@{
  "RedfieldConfiguration"= @{
    "InstrumentationKeyMap"= @{
      "Filters"= @(
        @{
          "AppFilter"= ".*";
          "MachineFilter"= ".*";
          "InstrumentationSettings"= @{
            "InstrumentationKey"= "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"; # Application Insights Instrumentation Key, create new Application Insights resource if you don't have one. https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/microsoft.insights%2Fcomponents
          }
        }
      )
    }
  }
};
$privateCfgHashtable = @{};

$vmss = Get-AzVmss -ResourceGroupName "<myResourceGroup>" -VMScaleSetName "<myVmssName>"

Add-AzVmssExtension -VirtualMachineScaleSet $vmss -Name "ApplicationMonitoring" -Publisher "Microsoft.Azure.Diagnostics" -Type "ApplicationMonitoringWindows" -TypeHandlerVersion "2.8" -Setting $publicCfgHashtable -ProtectedSetting $privateCfgHashtable

Update-AzVmss -ResourceGroupName $vmss.ResourceGroupName -Name $vmss.Name -VirtualMachineScaleSet $vmss

# Note: depending on your update policy, you might need to run Update-AzVmssInstance for each instance
```

Deinstallieren der Erweiterung zur Anwendungsüberwachung in der Azure-VM-Skalierungsgruppe
```powershell
$vmss = Get-AzVmss -ResourceGroupName "<myResourceGroup>" -VMScaleSetName "<myVmssName>"

Remove-AzVmssExtension -VirtualMachineScaleSet $vmss -Name "ApplicationMonitoring"

Update-AzVmss -ResourceGroupName $vmss.ResourceGroupName -Name $vmss.Name -VirtualMachineScaleSet $vmss

# Note: depending on your update policy, you might need to run Update-AzVmssInstance for each instance
```

Abfragen des Status der Erweiterung zur Anwendungsüberwachung für die Azure-VM-Skalierungsgruppe
```powershell
# Not supported by extensions framework
```

Abrufen der Liste der installierten Erweiterungen für die Azure-VM-Skalierungsgruppe
```powershell
Get-AzResource -ResourceId /subscriptions/<mySubscriptionId>/resourceGroups/<myResourceGroup>/providers/Microsoft.Compute/virtualMachineScaleSets/<myVmssName>/extensions

# Name              : ApplicationMonitoringWindows
# ResourceGroupName : <myResourceGroup>
# ResourceType      : Microsoft.Compute/virtualMachineScaleSets/extensions
# Location          :
# ResourceId        : /subscriptions/<mySubscriptionId>/resourceGroups/<myResourceGroup>/providers/Microsoft.Compute/virtualMachineScaleSets/<myVmssName>/extensions/ApplicationMonitoringWindows
```

## <a name="troubleshooting"></a>Problembehandlung

Nachfolgend finden Sie schrittweise Anleitungen zur Problembehandlung für die erweiterungsbasierte Überwachung von .NET-Anwendungen, die auf Azure-VMs und in Azure-VM-Skalierungsgruppen ausgeführt werden.

> [!NOTE]
> .NET Core-, Java- und Node.js-Anwendungen werden auf Azure-VMs und in Azure-VM-Skalierungsgruppen nur über die manuelle SDK-basierte Instrumentierung unterstützt. Daher gelten die folgenden Schritte nicht für diese Szenarien.

Die Ausgabe der Erweiterungsausführung wird in Dateien in folgenden Verzeichnissen protokolliert:
```Windows
C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.ApplicationMonitoringWindows\<version>\
```

## <a name="next-steps"></a>Nächste Schritte
* Hier erfahren Sie, wie Sie [eine Anwendung in einer Azure-VM-Skalierungsgruppe bereitstellen](../../virtual-machine-scale-sets/virtual-machine-scale-sets-deploy-app.md).
* [Richten Sie Verfügbarkeitswebtests ein](monitor-web-app-availability.md), um benachrichtigt zu werden, wenn Ihr Endpunkt nicht verfügbar ist.
