---
title: Außerkraftsetzung für Azure Traffic Manager-Subnetz mithilfe von Azure PowerShell | Microsoft-Dokumentation
description: Dieser Artikel wird Ihnen helfen zu verstehen, wie die Außerkraftsetzung des Traffic Manager-Subnetzes verwendet wird, um die Routingmethode eines Traffic Manager-Profils außer Kraft zu setzen, um den Datenverkehr mithilfe von Azure PowerShell basierend auf der Endbenutzer-IP-Adresse über vordefinierte IP-Bereiche auf Endpunktzuordnungen zu leiten.
services: traffic-manager
documentationcenter: ''
author: asudbring
manager: twooley
ms.topic: article
ms.service: traffic-manager
ms.date: 09/18/2019
ms.author: allensu
ms.openlocfilehash: 93362c0e1b359583e30886172f8ccb155c5ffaf4
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71348897"
---
# <a name="traffic-manager-subnet-override-using-azure-powershell"></a>Außerkraftsetzung für Traffic Manager-Subnetz mithilfe von Azure PowerShell

Die Außerkraftsetzung des Traffic Manager-Subnetzes ermöglicht es Ihnen, die Routingmethode eines Profils zu ändern.  Das Hinzufügen einer Außerkraftsetzung leitet den Datenverkehr basierend auf der IP-Adresse des Endbenutzers mit einem vordefinierten IP-Bereich an die Endpunktzuordnung. 

## <a name="how-subnet-override-works"></a>Funktionsweise der Außerkraftsetzung von Subnetzen

Wenn Außerkraftsetzungen von Subnetzen zu einem Traffic-Manager-Profil hinzugefügt werden, überprüft Traffic Manager zunächst, ob eine Außerkraftsetzung des Subnetzes für die IP-Adresse des Endbenutzers vorliegt. Wenn diese gefunden wird, erfolgt eine Weiterleitung der DNS-Abfrage des Benutzers an den entsprechenden Endpunkt.  Wenn keine Zuordnung gefunden wird, greift Traffic Manager auf die ursprüngliche Routingmethode des Profils zurück. 

Die IP-Adressbereiche können entweder als CIDR-Bereiche (z. B. 1.2.3.0/24) oder als Adressbereiche (z. B. 1.2.3.4-5.6.7.8) angegeben werden. Die jedem Endpunkt zugeordneten IP-Bereiche müssen für diesen Endpunkt eindeutig sein. Jede Überlappung von IP-Bereichen zwischen verschiedenen Endpunkten führt dazu, dass das Profil von Traffic Manager abgelehnt wird.

Es gibt zwei Arten von Routingprofilen, die eine Außerkraftsetzung von Subnetzen unterstützen:

* **Geografisch**: Wenn Traffic Manager eine Außerkraftsetzung des Subnetzes für die IP-Adresse der DNS-Abfrage findet, wird die Abfrage unabhängig von seiner Integrität an den Endpunkt weitergeleitet.
* **Leistung**: Wenn Traffic Manager eine Außerkraftsetzung des Subnetzes für die IP-Adresse der DNS-Abfrage findet, wird der Datenverkehr nur dann an den Endpunkt weitergeleitet, wenn er fehlerfrei ist.  Traffic Manager greift auf die Heuristik des Leistungsroutings zurück, wenn der Endpunkt für die Außerkraftsetzung des Subnetzes nicht fehlerfrei ist.

## <a name="create-a-traffic-manager-subnet-override"></a>Erstellen einer Außerkraftsetzung für ein Traffic Manager-Subnetz

Sie können Azure PowerShell verwenden, um die Subnetze für die Außerkraftsetzung zum Traffic Manager-Endpunkt hinzuzufügen, um eine Außerkraftsetzung des Traffic Manager-Subnetzes zu erstellen.

## <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Sie können die nachfolgenden Befehle in [Azure Cloud Shell](https://shell.azure.com/powershell) oder über PowerShell auf Ihrem Computer ausführen. Azure Cloud Shell ist eine kostenlose interaktive Shell. Sie verfügt über allgemeine vorinstallierte Tools und ist für die Verwendung mit Ihrem Konto konfiguriert. Wenn Sie PowerShell auf Ihrem Computer ausführen, müssen Sie mindestens Version 1.0.0 des Azure PowerShell-Moduls ausführen. Sie können `Get-Module -ListAvailable Az` ausführen, um die installierte Version zu ermitteln. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Install and configure Azure PowerShell](/powershell/azure/install-az-ps) (Installieren des Azure PowerShell-Moduls) Informationen dazu. Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Login-AzAccount` ausführen, um sich bei Azure anzumelden.


1. **Rufen Sie den Traffic Manager-Endpunkt ab:**

    Um die Außerkraftsetzung des Subnetzes zu aktivieren, rufen Sie den Endpunkt ab, dem Sie die Außerkraftsetzung hinzufügen möchten, und speichern Sie ihn mit [Get-AzTrafficManagerEndpoint](https://docs.microsoft.com/powershell/module/az.trafficmanager/get-aztrafficmanagerendpoint?view=azps-2.5.0) in einer Variablen.

    Ersetzen Sie „Name“, „ProfilName“ und „ResourceGroupName“ durch die Werte des zu ändernden Endpunkts.

    ```powershell

    $TrafficManagerEndpoint = Get-AzTrafficManagerEndpoint -Name "contoso" -ProfileName "ContosoProfile" -ResourceGroupName "ResourceGroup" -Type AzureEndpoints

    ```
2. **Fügen Sie den IP-Adressbereich zum Endpunkt hinzu:**
    
    Um den IP-Adressbereich zum Endpunkt hinzuzufügen, verwenden Sie [Add-AzTrafficManagerIpAddressRange](https://docs.microsoft.com/powershell/module/az.trafficmanager/add-aztrafficmanageripaddressrange?view=azps-2.5.0&viewFallbackFrom=azps-2.4.0).

    ```powershell

    ### Add a range of IPs ###
    Add-AzTrafficManagerIPAddressRange -TrafficManagerEndpoint $TrafficManagerEndpoint -First "1.2.3.4" -Last "5.6.7.8"

    ### Add a subnet ###
    Add-AzTrafficManagerIPAddressRange -TrafficManagerEndpoint $TrafficManagerEndpoint -First "9.10.11.0" -Scope 24

    ### Add a range of IPs with a subnet ###
    Add-AzTrafficManagerIPAddressRange -TrafficManagerEndpoint $TrafficManagerEndpoint -First "12.13.14.0" -Last "12.13.14.31" -Scope 27
 
    ```
    Nachdem die Bereiche hinzugefügt wurden, verwenden Sie [Set-AzTrafficManagerEndpoint](https://docs.microsoft.com/powershell/module/az.trafficmanager/set-aztrafficmanagerendpoint?view=azps-2.5.0), um den Endpunkt zu aktualisieren.

    ```powershell

    Set-AzTrafficManagerEndpoint -TrafficManagerEndpoint $TrafficManagerEndpoint

    ```
Das Entfernen des IP-Adressbereichs kann mit [Remove-AzTrafficManagerIpAddressRange](https://docs.microsoft.com/powershell/module/az.trafficmanager/remove-aztrafficmanageripaddressrange?view=azps-2.5.0) abgeschlossen werden.

1.  **Rufen Sie den Traffic Manager-Endpunkt ab:**

    Um die Außerkraftsetzung des Subnetzes zu aktivieren, rufen Sie den Endpunkt ab, dem Sie die Außerkraftsetzung hinzufügen möchten, und speichern Sie ihn mit [Get-AzTrafficManagerEndpoint](https://docs.microsoft.com/powershell/module/az.trafficmanager/get-aztrafficmanagerendpoint?view=azps-2.5.0) in einer Variablen.

    Ersetzen Sie „Name“, „ProfilName“ und „ResourceGroupName“ durch die Werte des zu ändernden Endpunkts.

    ```powershell

    $TrafficManagerEndpoint = Get-AzTrafficManagerEndpoint -Name "contoso" -ProfileName "ContosoProfile" -ResourceGroupName "ResourceGroup" -Type AzureEndpoints

    ```
2. **Entfernen Sie den IP-Adressbereich vom Endpunkt:**

    ```powershell
    
    ### Remove a range of IPs ###
    Remove-AzTrafficManagerIpAddressRange -TrafficManagerEndpoint $TrafficManagerEndpoint -First "1.2.3.4" -Last "5.6.7.8"

    ### Remove a subnet ###
    Remove-AzTrafficManagerIpAddressRange -TrafficManagerEndpoint $TrafficManagerEndpoint -First "9.10.11.0" -Scope 24

    ### Remove a range of IPs with a subnet ###
    Remove-AzTrafficManagerIpAddressRange -TrafficManagerEndpoint $TrafficManagerEndpoint -First "12.13.14.0" -Last "12.13.14.31" -Scope 27

    ```
     Nachdem die Bereiche entfernt wurden, verwenden Sie [Set-AzTrafficManagerEndpoint](https://docs.microsoft.com/powershell/module/az.trafficmanager/set-aztrafficmanagerendpoint?view=azps-2.5.0), um den Endpunkt zu aktualisieren.

    ```powershell

    Set-AzTrafficManagerEndpoint -TrafficManagerEndpoint $TrafficManagerEndpoint

    ```

## <a name="next-steps"></a>Nächste Schritte
Informieren Sie sich ausführlicher über [Traffic Manager-Routingmethoden](traffic-manager-routing-methods.md).

Weitere Informationen zur [Routingmethode „Subnetz“ für Datenverkehr](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-routing-methods#subnet-traffic-routing-method)
