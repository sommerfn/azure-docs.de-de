---
title: Konfigurieren einer IP-Einschränkungsregel mit Web Application Firewall für Azure Front Door
description: Erfahren Sie, wie Sie eine WAF-Regel für die IP-Adresseinschränkung für einen vorhandenen Front Door-Endpunkt konfigurieren.
services: frontdoor
documentationcenter: ''
author: KumudD
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/25/2019
ms.author: kumud;tyao
ms.openlocfilehash: b129579916330a34a2a78d98f2c7653f129d3319
ms.sourcegitcommit: bb85a238f7dbe1ef2b1acf1b6d368d2abdc89f10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2019
ms.locfileid: "65523710"
---
# <a name="configure-an-ip-restriction-rule-with-web-application-firewall-for-azure-front-door-preview"></a>Konfigurieren einer IP-Einschränkungsregel mit Web Application Firewall für Azure Front Door (Vorschau)
 In diesem Artikel wird erläutert, wie Sie IP-Einschränkungsregeln in Azure Web Application Firewall (WAF) für Front Door über die Azure-Befehlszeilenschnittstelle, Azure PowerShell oder eine Azure Resource Manager-Vorlage konfigurieren.

Eine auf IP-Adressen basierende Zugriffssteuerungsregel ist eine benutzerdefinierte WAF-Regel, mit der Sie den Zugriff auf Ihre Webanwendungen steuern können, indem Sie eine Liste von IP-Adressen oder IP-Adressbereichen im CIDR-Format (Classless Inter-Domain Routing) angeben.

Standardmäßig kann auf Ihre Webanwendungen über das Internet zugegriffen werden. Wenn Sie den Zugriff auf die Webanwendungen nur auf Clients aus einer Liste von bekannten IP-Adressen oder IP-Adressbereichen beschränken möchten, müssen Sie zwei IP-Abgleichsregeln erstellen. Die erste IP-Abgleichsregel enthält die Liste der IP-Adressen als übereinstimmende Werte und legt die Aktion auf „ALLOW“ fest. Mit der zweiten Regel mit niedrigerer Priorität werden alle anderen IP-Adressen mithilfe des Operators „All“ blockiert und die Aktion auf „BLOCK“ festgelegt. Nachdem eine IP-Einschränkungsregel angewandt wurde, erhalten alle Anforderungen von Adressen, die nicht in dieser Zulassungsliste enthalten sind, die Antwort „403 (Nicht zulässig)“.  

> [!IMPORTANT]
> Die WAF-IP-Beschränkungsfunktion für Azure Front Door befindet sich derzeit in der Public Preview.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="configure-waf-policy-with-azure-cli"></a>Konfigurieren einer WAF-Richtlinie über die Azure-Befehlszeilenschnittstelle

### <a name="prerequisites"></a>Voraussetzungen
Vor dem Konfigurieren einer Richtlinie für IP-Einschränkung müssen Sie zunächst die CLI-Umgebung einrichten und ein Front Door-Profil erstellen.

#### <a name="set-up-azure-cli-environment"></a>Einrichten der Azure CLI-Umgebung
1. Installieren Sie die [Azure CLI](/cli/azure/install-azure-cli), oder verwenden Sie Azure Cloud Shell. Azure Cloud Shell ist eine kostenlose Bash-Shell, die Sie direkt im Azure-Portal ausführen können. Die Azure-Befehlszeilenschnittstelle ist vorinstalliert und für die Verwendung mit Ihrem Konto konfiguriert. Klicken Sie in den folgenden CLI-Beispielen auf die Schaltfläche **Jetzt testen**. Durch den Klick auf **Jetzt testen** wird eine Cloud Shell aufgerufen, bei der Sie sich mit Ihrem Azure-Konto anmelden können. Geben Sie nach dem Start einer Cloud Shell-Sitzung `az extension add --name front-door` ein, um die Front Door-Erweiterung hinzuzufügen.
 2. Wenn Sie die CLI lokal in Bash verwenden, melden Sie sich mit `az login` bei Azure an.

#### <a name="create-front-door-profile"></a>Erstellen eines Front Door-Profils
Gehen Sie zum Erstellen eines Front Door-Profils gemäß den Anweisungen unter [Schnellstart: Erstellen einer Front Door-Instanz für eine hoch verfügbare globale Webanwendung](quickstart-create-front-door.md) vor.

### <a name="create-a-waf-policy"></a>Erstellen einer WAF-Richtlinie

Erstellen Sie mit dem Befehl [az network waf-policy create](/cli/azure/ext/front-door/network/waf-policy?view=azure-cli-latest#ext-front-door-az-network-waf-policy-create) eine WAF-Richtlinie. Ersetzen Sie im folgenden Beispiel den Richtliniennamen *IPAllowPolicyExampleCLI* durch einen eindeutigen Richtliniennamen.

```azurecli-interactive 
az network waf-policy create \
  --resource-group <resource-group-name> \
  --subscription <subscription ID> \
  --name IPAllowPolicyExampleCLI
  ```
### <a name="add-custom-ip-access-control-rule"></a>Hinzufügen einer benutzerdefinierten Regel für die IP-Zugriffssteuerung

Fügen Sie mit dem Befehl [az network waf-policy custom-rule create](/cli/azure/ext/front-door/network/waf-policy/custom-rule?view=azure-cli-latest#ext-front-door-az-network-waf-policy-custom-rule-create) der im vorherigen Schritt erstellten WAF-Richtlinie eine benutzerdefinierte Regel für die IP-Zugriffssteuerung hinzu. 

Im folgenden Beispiel:
-  Ersetzen Sie *IPAllowPolicyExampleCLI* durch die zuvor erstellte eindeutige Richtlinie.
-  Ersetzen Sie *ip-address-range-1* und *ip-address-range-2* durch Ihren gewünschten Bereich.

Erstellen Sie zunächst die IP-Zulassungsregel für die angegebenen Adressen.

```azurecli
az network waf-policy custom-rule create \
  --name IPAllowListRule \
  --priority 1 \
  --rule-type MatchRule \
  --match-condition RemoteAddr IPMatch ["<ip-address-range-1>","<ip-address-range-2>"] \
  --action Allow \
  --resource-group <resource-group-name> \
  --policy-name IPAllowPolicyExampleCLI
```
Erstellen Sie dann eine Regel zum Blockieren aller IP-Adressen mit einer niedrigeren Priorität als die vorherige IP-Zulassungsregel. Ersetzen Sie *IPAllowPolicyExampleCLI* durch die zuvor erstellte eindeutige Richtlinie.

```azurecli
az network waf-policy custom-rule create \
  --name IPDenyAllRule\
  --priority 2 \
  --rule-type MatchRule \
  --match-condition RemoteAddr Any
  --action Block \
  --resource-group <resource-group-name> \
  --policy-name IPAllowPolicyExampleCLI
 ```

### <a name="find-waf-policy-id"></a>Suchen der ID einer WAF-Richtlinie
Mit dem Befehl [az network waf-policy show](/cli/azure/ext/front-door/network/waf-policy?view=azure-cli-latest#ext-front-door-az-network-waf-policy-show) können Sie die ID einer WAF-Richtlinie suchen. Ersetzen Sie *IPAllowPolicyExampleCLI* durch die zuvor erstellte eindeutige Richtlinie.

   ```azurecli
   az network waf-policy show \
     --resource-group <resource-group-name> \
     --name IPAllowPolicyExampleCLI
   ```

### <a name="link-waf-policy-to-a-front-door-front-end-host"></a>Verknüpfen der WAF-Richtlinie mit einem Front Door-Front-End-Host

Legen Sie mit dem Befehl [az network front-door update](/cli/azure/ext/front-door/network/front-door?view=azure-cli-latest#ext-front-door-az-network-front-door-update) die Front Door-ID *WebApplicationFirewallPolicyLink* auf die Richtlinien-ID fest. Ersetzen Sie *IPAllowPolicyExampleCLI* durch die zuvor erstellte eindeutige Richtlinie.

   ```azurecli
   az network front-door update \
     --set FrontendEndpoints[0].WebApplicationFirewallPolicyLink.id=/subscriptions/<subscription ID>/resourcegroups/<resource- name>/providers/Microsoft.Network/frontdoorwebapplicationfirewallpolicies/IPAllowPolicyExampleCLI \
     --name <frontdoor-name>
     --resource-group <resource-group-name>
   ```
In diesem Beispiel wird die WAF-Richtlinie auf FrontendEndpoints[0] angewandt. Sie können die WAF-Richtlinie mit jedem der Front-Ends verknüpfen.
> [!Note]
> Die Eigenschaft **WebApplicationFirewallPolicyLink** muss nur einmal festgelegt werden, um eine WAF-Richtlinie mit einem Front Door-Front-End zu verknüpfen. Nachfolgende Richtlinienaktualisierungen werden automatisch auf das Front-End angewandt.

## <a name="configure-waf-policy-with-azure-powershell"></a>Konfigurieren einer WAF-Richtlinie über Azure PowerShell

### <a name="prerequisites"></a>Voraussetzungen
Vor dem Konfigurieren einer Richtlinie für die IP-Einschränkung müssen Sie zunächst die PowerShell-Umgebung einrichten und ein Front Door-Profil erstellen.

#### <a name="set-up-your-powershell-environment"></a>Einrichten Ihrer PowerShell-Umgebung
Azure PowerShell bietet eine Reihe von Cmdlets, die das [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview)-Modell für die Verwaltung von Azure-Ressourcen verwenden. 

Sie können [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) auf Ihrem lokalen Computer installieren und in einer beliebigen PowerShell-Sitzung nutzen. Befolgen Sie die Anweisungen auf der Seite, um sich mit Ihren Azure-Anmeldeinformationen anzumelden und das Azure PowerShell-Modul zu installieren.

##### <a name="connect-to-azure-with-an-interactive-dialog-for-sign-in"></a>Herstellen einer Verbindung mit Azure über ein interaktives Anmeldedialogfeld
```
Connect-AzAccount

```
Stellen Sie vor dem Installieren des Front Door-Moduls sicher, dass die aktuelle Version von PowerShellGet installiert ist. Führen Sie den folgenden Befehl aus, und öffnen Sie PowerShell erneut.

```
Install-Module PowerShellGet -Force -AllowClobber
``` 

##### <a name="install-azfrontdoor-module"></a>Installieren des Moduls „Az.FrontDoor“ 

```
Install-Module -Name Az.FrontDoor
```
### <a name="create-a-front-door-profile"></a>Erstellen eines Front Door-Profils
Gehen Sie zum Erstellen eines Front Door-Profils gemäß den Anweisungen unter [Schnellstart: Erstellen einer Front Door-Instanz für eine hoch verfügbare globale Webanwendung](quickstart-create-front-door.md) vor.

### <a name="define-ip-match-condition"></a>Definieren einer IP-Übereinstimmungsbedingung
Definieren Sie mit dem Befehl [New-AzFrontDoorWafMatchConditionObject](/powershell/module/az.frontdoor/new-azfrontdoorwafmatchconditionobject) eine IP-Übereinstimmungsbedingung. Ersetzen Sie im Beispiel unten *ip-address-range-1* und *ip-address-range-2* durch Ihren gewünschten Bereich.

```powershell
  $IPMatchCondition = New-AzFrontDoorWafMatchConditionObject `
    -MatchVariable  RemoteAddr `
    -OperatorProperty IPMatch `
    -MatchValue ["ip-address-range-1", "ip-address-range-2"]
```
Erstellen einer Bedingungsregel für die Übereinstimmung aller IP-Adressen
```powershell
  $IPMatchALlCondition = New-AzFrontDoorWafMatchConditionObject `
    -MatchVariable  RemoteAddr `
    -OperatorProperty Any
    
```

### <a name="create-a-custom-ip-allow-rule"></a>Erstellen einer benutzerdefinierten IP-Zulassungsregel
   Verwenden Sie den Befehl [New-AzFrontDoorCustomRuleObject](/powershell/module/Az.FrontDoor/New-azfrontdoorwafcustomruleobject), um eine Aktion zu definieren und eine Priorität festzulegen. Im folgenden Beispiel sind Anforderungen von Client-IP-Adressen zulässig, die mit der Liste übereinstimmen. 

```powershell
  $IPAllowRule = New-AzFrontDoorCustomRuleObject `
    -Name "IPAllowRule" `
    -RuleType MatchRule `
    -MatchCondition $IPMatchCondition `
    -Action Allow -Priority 1
```
Erstellen Sie eine Regel zum Blockieren aller IP-Adressen mit einer niedrigeren Priorität als die vorherige IP-Zulassungsregel.

```powershell
  $IPBlockAll = New-AzFrontDoorCustomRuleObject `
    -Name "IPDenyAll" `
    -RuleType MatchRule `
    -MatchCondition $IPMatchALlCondition `
    -Action Block `
    -Priority 2
   ```

### <a name="configure-waf-policy"></a>Konfigurieren einer WAF-Richtlinie
Suchen Sie mithilfe von `Get-AzResourceGroup` nach dem Namen der Ressourcengruppe, die das Front Door-Profil enthält. Konfigurieren Sie dann mit dem Befehl [New-AzFrontDoorWafPolicy](/powershell/module/az.frontdoor/new-azfrontdoorwafpolicy) eine WAF-Richtlinie mit der Regel zum Blockieren von IP-Adressen.

```powershell
  $IPAllowPolicyExamplePS = New-AzFrontDoorWafPolicy `
    -Name "IPRestrictionExamplePS" `
    -resourceGroupName <resource-group-name> `
    -Customrule $IPAllowRule $IPBlockAll `
    -Mode Prevention `
    -EnabledState Enabled
   ```

### <a name="link-waf-policy-to-a-front-door-front-end-host"></a>Verknüpfen der WAF-Richtlinie mit einem Front Door-Front-End-Host

Verknüpfen Sie das WAF-Richtlinienobjekt mit einem vorhandenen Front Door-Front-End-Host, und aktualisieren Sie die Front Door-Eigenschaften. Rufen Sie zunächst mithilfe von [Get-AzFrontDoor](/powershell/module/Az.FrontDoor/Get-AzFrontDoor) das Front Door-Objekt ab. Legen Sie dann mit dem Befehl [Set-AzFrontDoor](/powershell/module/Az.FrontDoor/Set-AzFrontDoor) die Eigenschaft *WebApplicationFirewallPolicyLink* des Front-Ends auf die Ressourcen-ID des im vorherigen Schritt erstellten *$IPAllowPolicyExamplePS*-Objekts fest.

```powershell
  $FrontDoorObjectExample = Get-AzFrontDoor `
    -ResourceGroupName <resource-group-name> `
    -Name $frontDoorName
  $FrontDoorObjectExample[0].FrontendEndpoints[0].WebApplicationFirewallPolicyLink = $IPBlockPolicy.Id
  Set-AzFrontDoor -InputObject $FrontDoorObjectExample[0]
```

> [!NOTE]
> In diesem Beispiel wird die WAF-Richtlinie auf FrontendEndpoints[0] angewandt. Sie können die WAF-Richtlinie mit jedem der Front-Ends verknüpfen. Die Eigenschaft *WebApplicationFirewallPolicyLink* muss nur einmal festgelegt werden, um eine WAF-Richtlinie mit einem Front Door-Front-End zu verknüpfen. Nachfolgende Richtlinienaktualisierungen werden automatisch auf das Front-End angewandt.


## <a name="configure-waf-policy-with-resource-manager-template"></a>Konfigurieren einer WAF-Richtlinie über eine Resource Manager-Vorlage
Die Vorlage, über die eine Front Door-Instanz und eine WAF-Richtlinie mit benutzerdefinierten IP-Einschränkungsregeln erstellt werden, können Sie [hier](https://github.com/Azure/azure-quickstart-templates/tree/master/201-front-door-waf-clientip) anzeigen.


## <a name="next-steps"></a>Nächste Schritte

- Informationen zum [Erstellen eines Front Door-Profils](quickstart-create-front-door.md)
