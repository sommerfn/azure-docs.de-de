---
title: Zuordnen einer Web Application Firewall-Richtlinie zu einer vorhandenen Azure Application Gateway-Instanz
description: Erfahren Sie, wie Sie eine Web Application Firewall-Richtlinie einer vorhandenen Azure Application Gateway-Instanz zuordnen.
services: web-application-firewall
ms.topic: article
author: vhorne
ms.service: web-application-firewall
ms.date: 10/25/2019
ms.author: victorh
ms.openlocfilehash: 1ed2e0cf8cc8cd841d8779462d62ba4852774a3a
ms.sourcegitcommit: a22cb7e641c6187315f0c6de9eb3734895d31b9d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2019
ms.locfileid: "74083906"
---
# <a name="associate-a-waf-policy-with-an-existing-application-gateway"></a>Zuordnen einer WAF-Richtlinie zu einer vorhandenen Application Gateway-Instanz

Sie können Azure PowerShell verwenden, [um eine WAF-Richtlinie zu erstellen](create-waf-policy-ag.md), jedoch verfügen Sie möglicherweise bereits über eine Application Gateway-Instanz und möchten ihr nur eine WAF-Richtlinie zuordnen. In diesem Artikel werden Sie genau das tun. Sie erstellen eine WAF-Richtlinie und ordnen Sie einer bereits vorhandenen Application Gateway-Instanz zu. 

1. Rufen Sie Ihre Application Gateway-Instanz und Firewall-Richtlinie ab. Wenn Sie nicht über eine Firewall-Richtlinie verfügen, siehe Schritt 2. 

   ```azurepowershell-interactive
      Connect-AzAccount
      Select-AzSubscription -Subscription "<sub id>"`

      #Get Application Gateway and existing policy object or create new`
      $gw = Get-AzApplicationGateway -Name <Waf v2> -ResourceGroupName <RG name>`
      $policy = Get-AzApplicationGatewayFirewallPolicy -Name <policy name> -ResourceGroupName <RG name>`
   ```

2. (Optional) Erstellen Sie eine Firewall-Richtlinie.

   ```azurepowershell-interactive
      New-AzApplicationGatewayFirewallPolicy -Name <policy name> -ResourceGroupName <RG name>'
      $policy = Get-AzApplicationGatewayFirewallPolicy -Name <policy name> -ResourceGroupName <RG name>`
   ```
   > [!NOTE]
   > Wenn Sie diese WAF-Richtlinie für den Übergang von einer WAF-Konfiguration zu einer WAF-Richtlinie erstellen, muss die Richtlinie eine exakte Kopie Ihrer alten Konfiguration sein. Dies bedeutet, dass jeder Ausschluss, jede benutzerdefinierte Regel, jede deaktivierte Regelgruppe usw. genau so sein muss wie in der WAF-Konfiguration.
3. (Optional) Sie können die WAF-Richtlinie so konfigurieren, dass Sie Ihren Anforderungen entspricht. Dies umfasst benutzerdefinierte Regeln, das Deaktivieren von Regeln/Regelgruppen, Ausschlüsse, das Festlegen von Dateiuploadlimits usw. Wenn Sie diesen Schritt überspringen, werden alle Standardwerte ausgewählt. 
   
4. Speichern Sie die Richtlinie, und fügen Sie sie Ihrer Application Gateway-Instanz an. 
   
   ```azurepowershell-interactive
      #Save the policy itself
      Set-AzApplicationGatewayFirewallPolicy -InputObject $policy`
   
      #Attach the policy to an Application Gateway
      $gw.FirewallPolicy = $policy`
   
      #Save the Application Gateway
      Set-AzApplicationGateway -ApplicationGateway $gw`
   ```

## <a name="next-steps"></a>Nächste Schritte
[Erfahren Sie mehr über benutzerdefinierte Rollen.](configure-waf-custom-rules.md)
