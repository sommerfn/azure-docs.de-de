---
title: Aktivieren und Deaktivieren der seriellen Azure-Konsole | Microsoft-Dokumentation
description: Aktivieren und Deaktivieren der seriellen Azure-Konsole
services: virtual-machines
documentationcenter: ''
author: asinn826
manager: borisb
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm
ms.workload: infrastructure-services
ms.date: 8/20/2019
ms.author: alsin
ms.openlocfilehash: 1c1fe208c77142351a786fa636896e64a8a467d7
ms.sourcegitcommit: 07700392dd52071f31f0571ec847925e467d6795
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2019
ms.locfileid: "70129466"
---
# <a name="enable-and-disable-the-azure-serial-console"></a>Aktivieren und Deaktivieren der seriellen Azure-Konsole

Wie jede andere Ressource kann die serielle Azure-Konsole aktiviert und deaktiviert werden. Die serielle Konsole ist standardmäßig für alle Abonnements global in Azure aktiviert. Derzeit wird durch Deaktivieren der seriellen Konsole der Dienst für Ihr gesamtes Abonnement deaktiviert. Zum Deaktivieren oder erneuten Aktivieren der seriellen Konsole für ein Abonnement sind Berechtigungen auf der Zugriffsebene „Mitwirkender“ oder höher für das Abonnement erforderlich.

Sie können die serielle Konsole auch für einen einzelnen virtuellen Computer oder eine VM-Skalierungsgruppeninstanz deaktivieren, indem Sie die Startdiagnose deaktivieren. Dazu benötigen Sie Berechtigungen auf der Zugriffsebene „Mitwirkender“ oder höher für den virtuellen Computer oder die VM-Skalierungsgruppe sowie für das Startdiagnose-Speicherkonto.

## <a name="vm-level-disable"></a>Deaktivieren auf VM-Ebene
Die serielle Konsole kann für bestimmte VMs oder VM-Skalierungsgruppen deaktiviert werden, indem die Startdiagnose deaktiviert wird. Deaktivieren Sie die Startdiagnose im Azure-Portal, um die serielle Konsole für eine VM oder VM-Skalierungsgruppe zu deaktivieren. Stellen Sie sicher, dass Sie Ihre VM-Skalierungsgruppeninstanzen auf die neueste Version aktualisieren, wenn Sie die serielle Konsole für eine VM-Skalierungsgruppe verwenden.


## <a name="subscription-level-disable"></a>Deaktivieren auf Abonnementebene

### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

Die serielle Konsole kann über die folgenden Befehle in der Azure-Befehlszeilenschnittstelle für ein gesamtes Abonnement deaktiviert und erneut aktiviert werden:

Verwenden Sie zum Deaktivieren der seriellen Konsole für ein Abonnement die folgenden Befehle:
```azurecli-interactive
subscriptionId=$(az account show -o=json | jq -r .id)

az resource invoke-action --action disableConsole --ids "/subscriptions/$subscriptionId/providers/Microsoft.SerialConsole/consoleServices/default"
```

Verwenden Sie zum Aktivieren der seriellen Konsole für ein Abonnement die folgenden Befehle:
```azurecli-interactive
subscriptionId=$(az account show -o=json | jq -r .id)

az resource invoke-action --action enableConsole --ids "/subscriptions/$subscriptionId/providers/Microsoft.SerialConsole/consoleServices/default"
```

Verwenden Sie die folgenden Befehle, um den aktuellen Status (aktiviert oder deaktiviert) der seriellen Konsole für ein Abonnement abzurufen:
```azurecli-interactive
subscriptionId=$(az account show -o=json | jq -r .id)

az resource show --ids "/subscriptions/$subscriptionId/providers/Microsoft.SerialConsole/consoleServices/default" -o=json | jq .properties
```

### <a name="powershell"></a>PowerShell

Die serielle Konsole kann auch mit PowerShell aktiviert und deaktiviert werden.

Verwenden Sie zum Deaktivieren der seriellen Konsole für ein Abonnement die folgenden Befehle:
```azurepowershell-interactive
$subscription=(Get-AzContext).Subscription.Id

Invoke-AzResourceAction -Action disableConsole -ResourceId /subscriptions/$subscription/providers/Microsoft.SerialConsole/consoleServices/default -ApiVersion 2018-05-01
```

Verwenden Sie zum Aktivieren der seriellen Konsole für ein Abonnement die folgenden Befehle:
```azurepowershell-interactive
$subscription=(Get-AzContext).Subscription.Id

Invoke-AzResourceAction -Action enableConsole -ResourceId /subscriptions/$subscription/providers/Microsoft.SerialConsole/consoleServices/default -ApiVersion 2018-05-01
```

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zur [seriellen Azure-Konsole für virtuelle Linux-Computer](./serial-console-linux.md)
* Weitere Informationen zur [seriellen Azure-Konsole für virtuelle Windows-Computer](./serial-console-windows.md)
* Informationen zu [Energieoptionen in der seriellen Azure-Konsole](./serial-console-power-options.md)