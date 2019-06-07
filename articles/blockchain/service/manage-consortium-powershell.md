---
title: Verwalten des Azure Blockchain-Konsortiums mithilfe von PowerShell
description: Verwalten der Mitglieder des Azure Blockchain-Konsortiums mithilfe von PowerShell
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 05/10/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: f15fa3b4972a2ac54d1d9bce916fdd42c2951d2f
ms.sourcegitcommit: f013c433b18de2788bf09b98926c7136b15d36f1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/13/2019
ms.locfileid: "65550882"
---
# <a name="manage-consortium-members-in-azure-blockchain-service-using-powershell"></a>Verwalten der Konsortiumsmitglieder in Azure Blockchain mithilfe von PowerShell

Sie können PowerShell dazu verwenden, Blockchainkonsortiumsmitglieder Ihres Azure Blockchain-Diensts zu verwalten. Mitglieder mit Administratorrechten können alle Teilnehmer des Blockchainkonsortiums einladen, hinzufügen, entfernen und deren Rollen ändern. Mitglieder mit Benutzerberechtigungen können sich im Blockchainkonsortium alle Teilnehmer anzeigen lassen und ihren Mitgliedsanzeigenamen ändern.

## <a name="prerequisites"></a>Voraussetzungen

* [Erstellen eines Blockchainmitglieds über das Azure-Portal](create-member.md)
* Weitere Informationen zu Konsortien, Mitgliedern und Knoten finden Sie unter [Azure Blockchain Service consortium (Azure Blockchain-Konsortium)](consortium.md)

## <a name="launch-azure-cloud-shell"></a>Starten von Azure Cloud Shell

Azure Cloud Shell ist eine kostenlose interaktive Shell, mit der Sie die Schritte in diesem Artikel ausführen können. Sie verfügt über allgemeine vorinstallierte Tools und ist für die Verwendung mit Ihrem Konto konfiguriert.

Sie können Cloud Shell auch auf einer separaten Browserregisterkarte starten, indem Sie zu [https://shell.azure.com/powershell](https://shell.azure.com/powershell) navigieren. Wählen Sie **Kopieren**, um die Blöcke mit dem Code zu kopieren. Fügen Sie ihn anschließend in Cloud Shell ein, und drücken Sie die EINGABETASTE, um ihn auszuführen.

## <a name="install-powershell-module"></a>Installieren des PowerShell-Moduls

Installieren Sie das Paket „Microsoft.AzureBlockchainService.ConsortiumManagement.PS“ aus dem PowerShell-Katalog.

```powershell-interactive
Install-Module -Name Microsoft.AzureBlockchainService.ConsortiumManagement.PS -Scope CurrentUser
Import-Module Microsoft.AzureBlockchainService.ConsortiumManagement.PS
```

## <a name="set-information-preference"></a>Festlegen der Informationspräferenz

Sie können weitere Informationen beim Ausführen der Cmdlets erhalten, indem Sie die Präferenzvariable für Informationen festlegen. Standardmäßig ist *$InformationPreference* auf *SilentlyContinue* festgelegt.

Um ausführlichere Informationen von Cmdlets zu erhalten, legen Sie die Präferenz in der PowerShell wie folgt fest:

```powershell-interactive
$InformationPreference = 'Continue'
```

## <a name="establish-a-web3-connection"></a>Herstellen einer Web3-Verbindung

Sie müssen eine Web3-Verbindung mit Ihrem Azure Blockchain-Dienstmitgliedsendpunkt herstellen, um Konsortiumsmitglieder zu verwalten. Mithilfe dieses Skripts können Sie globale Variablen festlegen, die beim Aufruf der Cmdlets zur Konsortiumsverwaltung verwendet werden können.

```powershell-interactive
$Connection = New-Web3Connection -RemoteRPCEndpoint '<Endpoint address>'
$MemberAccount = Import-Web3Account -ManagedAccountAddress '<Member account address>' -ManagedAccountPassword '<Member account password>'
$ContractConnection = Import-ConsortiumManagementContracts -RootContractAddress '<RootContract address>' -Web3Client $Connection
```

Ersetzen Sie \<Member account password\> (Kennwort des Mitgliedskontos) durch das Kennwort des Mitgliedskontos, das Sie beim Erstellen des Mitglieds verwendet haben.

Die weiteren Werte finden Sie im Azure-Portal:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Navigieren Sie zur **Übersichtsseite** Ihres Standardmitglieds des Azure Blockchain-Diensts.

    ![Mitgliedsübersicht](./media/manage-consortium-powershell/member-overview.png)

    Ersetzen Sie \<Member account\> (Mitgliedskonto) und \<RootContract address\> (Stammvertragsadresse) durch die Werte aus dem Portal.

1. Wählen Sie für die Endpunktadresse **Transaction nodes** (Transaktionsknoten) und dann den **standardmäßigen** Transaktionsknoten aus. Der Standardtransaktionsknoten weist denselben Namen auf wie das Blockchainmitglied.
1. Wählen Sie **Connection strings** (Verbindungszeichenfolgen).

    ![Verbindungszeichenfolgen](./media/manage-consortium-powershell/connection-strings.png)

    Ersetzen Sie \<Endpoint address\> (Endpunktadresse) durch den Wert unter **HTTPS (Access key 1)** (HTTPS (Zugriffsschlüssel 1)) oder **HTTPS (Access key 2)** (HTTPS (Zugriffsschlüssel 2)).

## <a name="network-and-smart-contract-management"></a>Netzwerk- und Smart Contract-Verwaltung

Verwenden Sie die Netzwerk- und Smart Contract-Cmdlets, um eine Verbindung mit Ihren Blockchainendpunkt-Smart Contracts herzustellen, die für die Konsortiumsverwaltung verantwortlich sind.

### <a name="import-consortiummanagementcontracts"></a>Import-ConsortiumManagementContracts

Stellt eine Verbindung mit den Smart Contracts der Konsortiumsverwaltung her, die zum Verwalten und Erzwingen von Mitgliedern innerhalb des Konsortiums verwendet werden.

`Import-ConsortiumManagementContracts -RootContractAddress <String> -Web3Client <IClient>`

| Parameter | BESCHREIBUNG | Erforderlich |
|-----------|-------------|:--------:|
| RootContractAddress | Die Stammvertragsadresse der Smart Contracts der Konsortiumsverwaltung | Ja |
| Web3Client | Ein von New-Web3Connection abgerufenes Web3Client-Objekt | Ja |

**Beispiel**

```powershell-interactive
Import-ConsortiumManagementContracts -RootContractAddress '<RootContract address>'  -Web3Client $Connection
```

### <a name="import-web3account"></a>Import-Web3Account

Verwenden Sie dieses Cmdlet, um ein Objekt für die Informationen des verwalteten Kontos zu erstellen.

`Import-Web3Account -ManagedAccountAddress <String> -ManagedAccountPassword <String>`

| Parameter | BESCHREIBUNG | Erforderlich |
|-----------|-------------|:--------:|
| ManagedAccountAddress | Adresse des Blockchainmitgliedskontos | Ja |
| ManagedAccountPassword | Kennwort der Kontoadresse | Ja |

**Beispiel**

```powershell-interactive
Import-Web3Account -ManagedAccountAddress '<Member account address>'  -ManagedAccountPassword '<Member account password>'
```

### <a name="new-web3connection"></a>New-Web3Connection

Stellt eine Verbindung mit dem RPC-Endpunkt eines Transaktionsknotens her.

`New-Web3Connection [-RemoteRPCEndpoint <String>]`

| Parameter | BESCHREIBUNG | Erforderlich |
|-----------|-------------|:--------:|
| RemoteRPCEndpoint | Adresse des Blockchainmitgliedsendpunkts | Ja |

**Beispiel**

```powershell-interactive
New-Web3Connection -RemoteRPCEndpoint '<Endpoint address>'
```

## <a name="consortium-member-management"></a>Konsortiumsmitgliedsverwaltung

Verwenden Sie Cmdlets zur Konsortiumsmitgliedsverwaltung, um Mitglieder innerhalb des Konsortiums zu verwalten. Die verfügbaren Aktionen sind abhängig von Ihrer Konsortiumsrolle.

### <a name="get-blockchainmember"></a>Get-BlockchainMember

Ruft Mitgliederdetails oder Listenmitglieder des Konsortiums ab.

`Get-BlockchainMember [[-Name] <String>] -Members <IContract> -Web3Client <IClient>`

| Parameter | BESCHREIBUNG | Erforderlich |
|-----------|-------------|:--------:|
| NAME | Der Name des Azure Blockchain-Dienstmitglieds, zu dem Sie Details abrufen möchten. Wenn Sie einen Mitgliedsnamen bereitstellen, werden Details zu dem Mitglied zurückgegeben. Wenn Sie keinen Namen angeben, wird eine Liste aller Konsortiumsmitglieder zurückgegeben. | Nein |
| Mitglieder | Ein von Import-ConsortiumManagementContracts abgerufenes Mitgliederobjekt | Ja |
| Web3Client | Ein von New-Web3Connection abgerufenes Web3Client-Objekt | Ja |

**Beispiel**

```powershell-interactive
$ContractConnection | Get-BlockchainMember -Name <Member Name>
```

**Beispielausgabe**

```
Name           : myblockchainmember
CorrelationId  : 0
DisplayName    : myCompany
SubscriptionId : <Azure subscription ID>
AccountAddress : 0x85b911c9e103d6405573151258d668479e9ebeef
Role           : ADMIN
```

### <a name="remove-blockchainmember"></a>Remove-BlockchainMember

Entfernt ein Blockchainmitglied.

`Remove-BlockchainMember -Name <String> -Members <IContract> -Web3Account <IAccount> -Web3Client <IClient>`

| Parameter | BESCHREIBUNG | Erforderlich |
|-----------|-------------|:--------:|
| NAME | Der Name des zu entfernenden Mitglieds | Ja |
| Mitglieder | Ein von Import-ConsortiumManagementContracts abgerufenes Mitgliederobjekt | Ja |
| Web3Account | Ein von Import-Web3Account abgerufenes Web3Account-Objekt | Ja |
| Web3Client | Ein von New-Web3Connection abgerufenes Web3Client-Objekt | Ja |

**Beispiel**

```powershell-interactive
$ContractConnection | Remove-BlockchainMember -Name <Member Name> -Web3Account $MemberAccount
```

### <a name="set-blockchainmember"></a>Set-BlockchainMember

Legt Attribute eines Blockchainmitglieds wie Anzeigename und Konsortiumsrolle fest.

Konsortiumsadministratoren können **DisplayName** (Anzeigename) und **Role** (Rolle) für alle Mitglieder festlegen. Konsortiumsmitglieder mit Benutzerrolle können nur ihren eigenen Mitgliedsanzeigenamen ändern.

`Set-BlockchainMember -Name <String> [-DisplayName <String>] [-AccountAddress <String>] [-Role <String>]
 -Members <IContract> -Web3Account <IAccount> -Web3Client <IClient>`

| Parameter | BESCHREIBUNG | Erforderlich |
|-----------|-------------|:--------:|
| NAME | Name des Blockchainmitglieds | Ja |
| DisplayName | Neuer Anzeigename | Nein |
| AccountAddress | Kontoadresse | Nein |
| Mitglieder | Ein von Import-ConsortiumManagementContracts abgerufenes Mitgliederobjekt | Ja |
| Web3Account | Ein von Import-Web3Account abgerufenes Web3Account-Objekt | Ja |
| Web3Client |  Ein von New-Web3Connection abgerufenes Web3Client-Objekt| Ja |

**Beispiel**

```powershell-interactive
$ContractConnection | Set-BlockchainMember -Name <Member Name> -DisplayName <Display name> -Web3Account $MemberAccount
```

## <a name="consortium-member-invitation-management"></a>Verwalten der Einladung von Konsortiumsmitgliedern

Verwenden Sie die Cmdlets zum Verwalten der Einladung von Konsortiumsmitgliedern, um Einladungen der Konsortiumsmitglieder zu verwalten. Die verfügbaren Aktionen sind abhängig von Ihrer Konsortiumsrolle.

### <a name="new-blockchainmemberinvitation"></a>New-BlockchainMemberInvitation

Laden Sie neue Mitglieder zum Konsortium ein.

`New-BlockchainMemberInvitation -SubscriptionId <String> -Role <String> -Members <IContract>
 -Web3Account <IAccount> -Web3Client <IClient>`

| Parameter | BESCHREIBUNG | Erforderlich |
|-----------|-------------|:--------:|
| SubscriptionId | Die Azure-Abonnement-ID des eingeladenen Mitglieds | Ja |
| Rolle | Die Konsortiumsrolle. Mögliche Werte sind ADMIN oder USER. Bei ADMIN handelt es sich um die Administratorrolle des Konsortiums. Bei USER handelt es sich um die Mitgliederrolle des Konsortiums. | Ja |
| Mitglieder | Ein von Import-ConsortiumManagementContracts abgerufenes Mitgliederobjekt | Ja |
| Web3Account | Ein von Import-Web3Account abgerufenes Web3Account-Objekt | Ja |
| Web3Client | Ein von New-Web3Connection abgerufenes Web3Client-Objekt | Ja |

**Beispiel**

```powershell-interactive
$ContractConnection | New-BlockchainMemberInvitation -SubscriptionId <Azure Subscription ID> -Role USER -Web3Account $MemberAccount
```

### <a name="get-blockchainmemberinvitation"></a>Get-BlockchainMemberInvitation

Ruft den Einladungsstatus von Mitgliedern ab oder listet diese auf.

`Get-BlockchainMemberInvitation [[-SubscriptionId] <String>] -Members <IContract> -Web3Client <IClient>`

| Parameter | BESCHREIBUNG | Erforderlich |
|-----------|-------------|:--------:|
| SubscriptionId | Die Azure-Abonnement-ID des eingeladenen Mitglieds. Wenn eine Abonnement-ID angegeben wird, werden Einladungsdetails zur Abonnement-ID zurückgegeben. Wenn Sie keine Abonnement-ID angeben, wird eine Liste mit allen Mitgliedereinladungen zurückgegeben. | Nein |
| Mitglieder | Ein von Import-ConsortiumManagementContracts abgerufenes Mitgliederobjekt | Ja |
| Web3Client | Ein von New-Web3Connection abgerufenes Web3Client-Objekt | Ja |

**Beispiel**

```powershell-interactive
$ContractConnection | Get-BlockchainMemberInvitation – SubscriptionId <Azure subscription ID>
```

**Beispielausgabe**

```
SubscriptionId                       Role CorrelationId
--------------                       ---- -------------
<Azure subscription ID>              USER             2
```

### <a name="remove-blockchainmemberinvitation"></a>Remove-BlockchainMemberInvitation

Widerruft eine Mitgliedereinladung zum Konsortium.

`Remove-BlockchainMemberInvitation -SubscriptionId <String> -Members <IContract> -Web3Account <IAccount>
 -Web3Client <IClient>`

| Parameter | BESCHREIBUNG | Erforderlich |
|-----------|-------------|:--------:|
| SubscriptionId | Die Azure-Abonnement-ID des Mitglieds, dessen Einladung widerrufen werden soll | Ja |
| Mitglieder | Ein von Import-ConsortiumManagementContracts abgerufenes Mitgliederobjekt | Ja |
| Web3Account | Ein von Import-Web3Account abgerufenes Web3Account-Objekt | Ja |
| Web3Client | Ein von New-Web3Connection abgerufenes Web3Client-Objekt | Ja |

**Beispiel**

```powershell-interactive
$ContractConnection | Remove-BlockchainMemberInvitation -SubscriptionId <Subscription ID> -Web3Account $MemberAccount
```

### <a name="set-blockchainmemberinvitation"></a>Set-BlockchainMemberInvitation

Legt die **Rolle** für eine bestehende Einladung fest. Nur Konsortiumsadministratoren können Einladungen ändern.

`Set-BlockchainMemberInvitation -SubscriptionId <String> -Role <String> -Members <IContract>
 -Web3Account <IAccount> -Web3Client <IClient>`

| Parameter | BESCHREIBUNG | Erforderlich |
|-----------|-------------|:--------:|
| SubscriptionId | Die Azure-Abonnement-ID des eingeladenen Mitglieds | Ja |
| Rolle | Neue Konsortiumsrolle für die Einladung. Mögliche Werte sind **USER** (Benutzer) und **ADMIN** (Administrator). | Ja |
| Mitglieder |  Ein von Import-ConsortiumManagementContracts abgerufenes Mitgliederobjekt | Ja |
| Web3Account | Ein von Import-Web3Account abgerufenes Web3Account-Objekt | Ja |
| Web3Client | Ein von New-Web3Connection abgerufenes Web3Client-Objekt | Ja |

**Beispiel**

```powershell-interactive
$ContractConnection | Set-BlockchainMemberInvitation -SubscriptionId <Azure subscription ID> -Role USER -Web3Account $MemberAccount
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Konsortien, Mitgliedern und Knoten finden Sie unter:

> [!div class="nextstepaction"]
> [Azure Blockchain Service consortium (Azure Blockchain-Dienstkonsortium)](consortium.md)