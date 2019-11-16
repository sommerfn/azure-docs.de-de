---
title: 'Gewusst wie: Erstellen von Richtlinien für Gastkonfigurationen'
description: Es wird beschrieben, wie Sie eine Azure Policy-Richtlinie für Gastkonfigurationen für Windows- oder Linux-VMs erstellen.
ms.date: 09/20/2019
ms.topic: conceptual
ms.openlocfilehash: 3c7b214a07b89f4b66aa32724259b01129b9b7e9
ms.sourcegitcommit: 39da2d9675c3a2ac54ddc164da4568cf341ddecf
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2019
ms.locfileid: "73959482"
---
# <a name="how-to-create-guest-configuration-policies"></a>Gewusst wie: Erstellen von Richtlinien für Gastkonfigurationen

Für die Gastkonfiguration wird ein [DSC](/powershell/scripting/dsc/overview/overview)-Ressourcenmodul (Desired State Configuration) verwendet, um die Konfiguration für die Überprüfung der Azure-Computer zu erstellen. Die DSC-Konfiguration definiert den Zustand, in dem sich der Computer befinden soll. Wenn bei der Evaluierung der Konfiguration ein Fehler auftritt, wird die Richtlinienauswirkung **auditIfNotExists** ausgelöst, und der Computer wird als **nicht konform** eingestuft.

Die [Azure Policy-Gastkonfiguration](/azure/governance/policy/concepts/guest-configuration) kann nur zur Überwachung von Einstellungen in Computern verwendet werden. Die Wiederherstellung von Einstellungen in Computern ist noch nicht verfügbar.

Verwenden Sie die folgenden Aktionen, um Ihre eigene Konfiguration zum Überprüfen des Zustands eines Azure-Computers zu erstellen.

> [!IMPORTANT]
> Benutzerdefinierte Richtlinien für Gastkonfigurationen sind eine Previewfunktion.

## <a name="add-the-guestconfiguration-resource-module"></a>Hinzufügen des GuestConfiguration-Ressourcenmoduls

Zum Erstellen einer Richtlinie für Gastkonfigurationen muss das Ressourcenmodul hinzugefügt werden. Dieses Ressourcenmodul kann mit einer lokal installierten PowerShell-Instanz, mit [Azure Cloud Shell](https://shell.azure.com) oder mit dem [Azure PowerShell-Docker-Image](https://hub.docker.com/rsdk-powershell/) verwendet werden.

### <a name="base-requirements"></a>Basisanforderungen

Für das Ressourcenmodul für Gastkonfigurationen wird die folgende Software benötigt:

- PowerShell. Falls es noch nicht installiert ist, befolgen Sie [diese Anweisungen](/powershell/scripting/install/installing-powershell).
- Azure PowerShell 1.5.0 oder höher. Falls es noch nicht installiert ist, befolgen Sie [diese Anweisungen](/powershell/azure/install-az-ps).

### <a name="install-the-module"></a>Installieren des Moduls

Für Gastkonfigurationen wird das Ressourcenmodul **GuestConfiguration** verwendet, um DSC-Konfigurationen zu erstellen und in Azure Policy zu veröffentlichen:

1. Führen Sie an einer PowerShell-Eingabeaufforderung den folgenden Befehl aus:

   ```azurepowershell-interactive
   # Install the Guest Configuration DSC resource module from PowerShell Gallery
   Install-Module -Name GuestConfiguration
   ```

1. Überprüfen Sie, ob das Modul importiert wurde:

   ```azurepowershell-interactive
   # Get a list of commands for the imported GuestConfiguration module
   Get-Command -Module 'GuestConfiguration'
   ```

## <a name="create-custom-guest-configuration-configuration-and-resources"></a>Erstellen einer benutzerdefinierten Konfiguration und von Ressourcen für Gastkonfigurationen

Der erste Schritt zur Erstellung einer benutzerdefinierten Richtlinie für Gastkonfigurationen ist die Erstellung der DSC-Konfiguration. Eine Übersicht über DSC-Konzepte und die Terminologie finden Sie in der [Übersicht über PowerShell DSC](/powershell/scripting/dsc/overview/overview).

Wenn Ihre Konfiguration nur Ressourcen erfordert, die mit der Installation des Gastkonfigurations-Agents erstellt wurden, müssen Sie nur eine MOF-Konfigurationsdatei erstellen. Wenn Sie ein zusätzliches Skript ausführen müssen, müssen Sie ein benutzerdefiniertes Ressourcenmodul erstellen.

### <a name="requirements-for-guest-configuration-custom-resources"></a>Anforderungen für benutzerdefinierte Ressourcen der Gastkonfiguration

Wenn die Gastkonfiguration einen Computer überwacht, führt sie zunächst `Test-TargetResource` aus, um festzustellen, ob er den richtigen Zustand aufweist. Der von der Funktion zurückgegebene boolesche Wert bestimmt, ob der Zustand von Azure Resource Manager für die Gastzuweisung konform/nicht konform sein soll. Wenn der boolesche Wert für eine Ressource in der Konfiguration `$false` ist, wird der Anbieter `Get-TargetResource` ausführen. Wenn der boolesche Wert `$true` ist, wird `Get-TargetResource` nicht aufgerufen.

Die Funktion `Get-TargetResource` hat spezielle Anforderungen an die Gastkonfiguration, die für die Windows Desired State Configuration nicht benötigt wurden.

- Die zurückgegebene Hashtabelle muss eine Eigenschaft namens **Reasons** enthalten.
- Die Reasons-Eigenschaft muss ein Array sein.
- Jedes Element im Array sollte eine Hashtabelle mit Schlüsseln namens **Code** und **Phrase** sein.

Die Reasons-Eigenschaft wird vom Dienst verwendet, um die Darstellung von Informationen zu standardisieren, wenn ein Computer nicht mehr konform ist. Sie können sich jedes Element in Reasons als „Grund“ vorstellen, dass die Ressource nicht konform ist. Die Eigenschaft ist ein Array, da eine Ressource aus mehr als einem Grund nicht konform sein könnte.

Die Eigenschaften **Code** und **Phrase** werden vom Dienst erwartet. Wenn Sie eine benutzerdefinierte Ressource erstellen, legen Sie den auszugebenden Text (typischerweise stdout) als Grund für die fehlende Konformität der Ressource als Wert für **Phrase** fest. **Code** weist bestimmte Formatierungsanforderungen auf, sodass die Berichterstellung eindeutig Informationen zur Ressource anzeigen kann, die für die Durchführung der Überwachung verwendet wurde. Diese Lösung macht die Gastkonfiguration erweiterbar. Jeder Befehl kann ausgeführt werden, um einen Computer zu überwachen, solange die Ausgabe erfasst und als Zeichenfolgenwert für die Eigenschaft **Phrase** zurückgegeben werden kann.

- **Code** (Zeichenfolge): Der Name der Ressource, wiederholt, und dann ein Kurzname ohne Leerzeichen als Bezeichner für den Grund. Diese drei Werte sollten ohne Leerzeichen durch Doppelpunkte getrennt sein.
  - Ein Beispiel wäre `registry:registry:keynotpresent`
- **Phrase** (Zeichenfolge): Für Menschen lesbarer Text, um zu erläutern, warum die Einstellung nicht konform ist.
  - Ein Beispiel wäre `The registry key $key is not present on the machine.`

```powershell
$reasons = @()
$reasons += @{
  Code = 'Name:Name:ReasonIdentifer'
  Phrase = 'Explain why the setting is not compliant'
}
return @{
    reasons = $reasons
}
```

#### <a name="scaffolding-a-guest-configuration-project"></a>Gerüstbau für ein Gastkonfigurationsprojekt

Für Entwickler, die den Prozess des Einstiegs und der Arbeit mit Beispielcode beschleunigen möchten, existiert ein Communityprojekt namens **Guest Configuration Project** (Gastkonfigurationsprojekt) als Vorlage für das PowerShell-Modul [Plaster](https://github.com/powershell/plaster). Dieses Tool kann verwendet werden, um ein Projekt mit einer funktionierenden Konfiguration und einer Beispielressource sowie einem Satz von [Pester](https://github.com/pester/pester)-Tests zur Überprüfung des Projekts einzurichten. Die Vorlage enthält auch Aufgabenausführungen für Visual Studio Code, um die Erstellung und Überprüfung des Gastkonfigurationspakets zu automatisieren. Weitere Informationen finden Sie im GitHub-Projekt [Guest Configuration Project](https://github.com/microsoft/guestconfigurationproject) (Gastkonfigurationsprojekt).

### <a name="custom-guest-configuration-configuration-on-linux"></a>Benutzerdefinierte Konfiguration für Gastkonfigurationen unter Linux

Für die DSC-Konfiguration für Gastkonfigurationen unter Linux wird die Ressource `ChefInSpecResource` verwendet, um für die Engine den Namen der [Chef InSpec](https://www.chef.io/inspec/)-Definition anzugeben. **Name** ist die einzige erforderliche Ressourceneigenschaft.

Im folgenden Beispiel wird eine Konfiguration mit dem Namen **baseline** erstellt und das Ressourcenmodul **GuestConfiguration** importiert. Außerdem wird die Ressource `ChefInSpecResource` verwendet, um den Namen der InSpec-Definition auf **linux-patch-baseline** festzulegen:

```azurepowershell-interactive
# Define the DSC configuration and import GuestConfiguration
Configuration baseline
{
    Import-DscResource -ModuleName 'GuestConfiguration'

    ChefInSpecResource 'Audit Linux patch baseline'
    {
        Name = 'linux-patch-baseline'
    }
}

# Compile the configuration to create the MOF files
baseline
```

Weitere Informationen finden Sie unter [Schreiben, Kompilieren und Anwenden einer Konfiguration](/powershell/scripting/dsc/configurations/write-compile-apply-configuration).

### <a name="custom-guest-configuration-configuration-on-windows"></a>Benutzerdefinierte Konfiguration für Gastkonfigurationen unter Windows

Die DSC-Konfiguration für die Azure Policy-Gastkonfiguration wird nur vom Gastkonfigurations-Agent verwendet. Es tritt kein Konflikt mit Windows PowerShell Desired State Configuration auf.

Im folgenden Beispiel wird eine Konfiguration mit dem Namen **AuditBitLocker** erstellt, das Ressourcenmodul **GuestConfiguration** importiert und die Ressource `Service` verwendet, um eine Überprüfung auf einen ausgeführten Dienst durchzuführen:

```azurepowershell-interactive
# Define the DSC configuration and import GuestConfiguration
Configuration AuditBitLocker
{
    Import-DscResource -ModuleName 'PSDscResources'

    Service 'Ensure BitLocker service is present and running'
    {
        Name = 'BDESVC'
        Ensure = 'Present'
        State = 'Running'
    }
}

# Compile the configuration to create the MOF files
AuditBitLocker
```

Weitere Informationen finden Sie unter [Schreiben, Kompilieren und Anwenden einer Konfiguration](/powershell/scripting/dsc/configurations/write-compile-apply-configuration).

## <a name="create-guest-configuration-custom-policy-package"></a>Erstellen eines benutzerdefinierten Richtlinienpakets für Gastkonfigurationen

Nachdem die MOF-Datei kompiliert wurde, müssen die unterstützenden Dateien in einem Paket zusammengefasst werden. Das fertige Paket wird von der Gastkonfiguration verwendet, um die Azure Policy-Definitionen zu erstellen. Das Paket enthält Folgendes:

- Kompilierte DSC-Konfiguration als MOF-Datei
- Ordner „Modules“
  - GuestConfiguration-Modul
  - DscNativeResources-Modul
  - (Linux) Ordner mit Chef InSpec-Definition und weiterem Inhalt
  - (Windows) Nicht integrierte DSC-Ressourcenmodule

Mit dem Cmdlet `New-GuestConfigurationPackage` wird das Paket erstellt. Das folgende Format wird verwendet, um ein benutzerdefiniertes Paket zu erstellen:

```azurepowershell-interactive
New-GuestConfigurationPackage -Name '{PackageName}' -Configuration '{PathToMOF}' `
    -Path '{OutputFolder}' -Verbose
```

Parameter des Cmdlets `New-GuestConfigurationPackage`:

- **Name**: Name des Pakets mit der Gastkonfiguration.
- **Konfiguration:** Vollständiger Pfad für das kompilierte DSC-Konfigurationsdokument.
- **Pfad**: Pfad des Ausgabeordners. Dieser Parameter ist optional. Wenn er nicht angegeben ist, wird das Paket im aktuellen Verzeichnis erstellt.
- **ChefProfilePath**: Vollständiger Pfad zum InSpec-Profil. Dieser Parameter wird nur unterstützt, wenn Inhalt für die Linux-Überprüfung erstellt wird.

Das fertige Paket muss an einem Speicherort gespeichert werden, auf den von den verwalteten virtuellen Computern zugegriffen werden kann. Beispiele hierfür sind GitHub-Repositorys, ein Azure-Repository oder Azure Storage. Falls Sie das Paket nicht öffentlich zugänglich machen möchten, können Sie ein [SAS-Token](../../../storage/common/storage-dotnet-shared-access-signature-part-1.md) in die URL einfügen. Sie können auch einen [Dienstendpunkt](../../../storage/common/storage-network-security.md#grant-access-from-a-virtual-network) für Computer in einem privaten Netzwerk implementieren. Diese Konfiguration gilt aber nur für den Zugriff auf das Paket und nicht für die Kommunikation mit dem Dienst.

### <a name="working-with-secrets-in-guest-configuration-packages"></a>Verwenden von Geheimnissen in Paketen für die Gastkonfiguration

Bei Azure Policy-Gastkonfigurationen besteht die optimale Vorgehensweise zum Verwalten von zur Laufzeit verwendeten Geheimnissen darin, diese in Azure Key Vault zu speichern. Dieser Entwurf wird in benutzerdefinierten DSC-Ressourcen implementiert.

1. Erstellen Sie zuerst in Azure eine verwaltete Identität, die dem Benutzer zugewiesen ist.

   Die Identität wird von Computern verwendet, um auf in Key Vault gespeicherte Geheimnisse zuzugreifen. Ausführliche Informationen zu den Schritten finden Sie unter [Erstellen, Auflisten oder Löschen einer vom Benutzer zugewiesenen verwalteten Identität mit Azure PowerShell](../../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-powershell.md).

1. Erstellen Sie eine Key Vault-Instanz.

   Ausführliche Informationen zu den Schritten finden Sie unter [Festlegen und Abrufen eines Geheimnisses: PowerShell](../../../key-vault/quick-create-powershell.md).
   Weisen Sie der Instanz Berechtigungen zu, um der dem Benutzer zugewiesenen Identität Zugriff auf die Geheimnisse zu gewähren, die in Key Vault gespeichert sind. Informationen zu den ausführlichen Schritten finden Sie unter [Festlegen und Abrufen eines Geheimnisses: .NET](../../../key-vault/quick-create-net.md#give-the-service-principal-access-to-your-key-vault).

1. Weisen Sie die benutzerseitig zugewiesene Identität Ihrem Computer zu.

   Ausführliche Informationen zu den Schritten finden Sie unter [Konfigurieren von verwalteten Identitäten für Azure-Ressourcen auf einem virtuellen Azure-Computer mithilfe von PowerShell](../../../active-directory/managed-identities-azure-resources/qs-configure-powershell-windows-vm.md#user-assigned-managed-identity).
   Verwenden Sie für die bedarfsabhängige Zuweisung dieser Identität Azure Resource Manager über Azure Policy. Ausführliche Informationen zu den Schritten finden Sie unter [Konfigurieren von verwalteten Identitäten für Azure-Ressourcen auf einem virtuellen Azure-Computer mithilfe einer Vorlage](../../../active-directory/managed-identities-azure-resources/qs-configure-template-windows-vm.md#assign-a-user-assigned-managed-identity-to-an-azure-vm).

1. Verwenden Sie auf Ihrer benutzerdefinierten Ressource abschließend dann die oben generierte Client-ID, um auf Key Vault zuzugreifen, indem Sie das auf dem Computer verfügbare Token nutzen.

   Die `client_id` und die URL für die Key Vault-Instanz können als [Eigenschaften](/powershell/scripting/dsc/resources/authoringresourcemof#creating-the-mof-schema) an die Ressource übergeben werden, damit diese nicht für mehrere Umgebungen aktualisiert werden muss (oder wenn die Werte geändert werden müssen).

Das folgende Codebeispiel kann in einer benutzerdefinierten Ressource verwendet werden, um mit einer Identität, die dem Benutzer zugewiesen ist, Geheimnisse aus Key Vault abzurufen. Der von der Anforderung an Key Vault zurückgegebene Wert ist ein Nur-Text-Wert. Die bewährte Methode besteht darin, ihn in einem Objekt mit Anmeldeinformationen zu speichern.

```azurepowershell-interactive
# the following values should be input as properties
$client_id = 'e3a78c9b-4dd2-46e1-8bfa-88c0574697ce'
$keyvault_url = 'https://keyvaultname.vault.azure.net/secrets/mysecret'

$access_token = ((Invoke-WebRequest -Uri "http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&client_id=$client_id&resource=https%3A%2F%2Fvault.azure.net" -Method GET -Headers @{Metadata='true'}).Content | ConvertFrom-Json).access_token

$value = ((Invoke-WebRequest -Uri $($keyvault_url+'?api-version=2016-10-01') -Method GET -Headers @{Authorization="Bearer $access_token"}).content | convertfrom-json).value |  ConvertTo-SecureString -asplaintext -force

$credential = New-Object System.Management.Automation.PSCredential('secret',$value)
```

## <a name="test-a-guest-configuration-package"></a>Testen eines Pakets mit der Gastkonfiguration

Nach dem Erstellen des Konfigurationspakets – aber vor der Veröffentlichung in Azure – können Sie die Funktionalität des Pakets über Ihre Arbeitsstation oder die CI/CD-Umgebung testen. Das GuestConfiguration-Modul enthält das Cmdlet `Test-GuestConfigurationPackage`, mit dem der gleiche Agent in Ihrer Entwicklungsumgebung geladen wird, der auch auf Azure-Computern genutzt wird. Mit dieser Lösung können Sie Integrationstests lokal durchführen, bevor die Veröffentlichung für kostenpflichtige Umgebungen für Tests, Qualitätssicherung und Produktion erfolgt.

```azurepowershell-interactive
Test-GuestConfigurationPackage -Path .\package\AuditWindowsService\AuditWindowsService.zip -Verbose
```

Parameter des Cmdlets `Test-GuestConfigurationPackage`:

- **Name**: Name der Richtlinie für Gastkonfigurationen.
- **Parameter**: Richtlinienparameter im Hashtabellenformat.
- **Pfad**: Vollständiger Pfad des Pakets mit der Gastkonfiguration.

Das Cmdlet unterstützt auch Eingaben aus der PowerShell-Pipeline. Fügen Sie die Ausgabe des Cmdlets `New-GuestConfigurationPackage` per Pipezeichen an das Cmdlet `Test-GuestConfigurationPackage` an.

```azurepowershell-interactive
New-GuestConfigurationPackage -Name AuditWindowsService -Configuration .\DSCConfig\localhost.mof -Path .\package -Verbose | Test-GuestConfigurationPackage -Verbose
```

Weitere Informationen zum Testen mit Parametern finden Sie im Abschnitt unter [Using parameters in custom Guest Configuration policies](#using-parameters-in-custom-guest-configuration-policies) (Verwenden von Parametern in Richtlinien für Gastkonfigurationen).

## <a name="create-the-azure-policy-definition-and-initiative-deployment-files"></a>Erstellen der Bereitstellungsdateien für Azure Policy-Definitionen und -Initiativen

Nachdem ein benutzerdefiniertes Richtlinienpaket für Gastkonfigurationen erstellt und an einen Ort hochgeladen wurde, auf den mit den Computern zugegriffen werden kann, können Sie die Definition der Richtlinie für Gastkonfigurationen für Azure Policy erstellen. Mit dem Cmdlet `New-GuestConfigurationPolicy` wird mit einem öffentlich zugänglichen benutzerdefinierten Richtlinienpaket für Gastkonfigurationen eine Richtliniendefinition vom Typ **auditIfNotExists** und vom Typ **deployIfNotExists** erstellt. Außerdem wird eine Richtlinien-Initiativendefinition erstellt, die beide Richtliniendefinitionen enthält.

Im folgenden Beispiel werden die Richtlinien- und Initiativendefinitionen unter einem angegebenen Pfad aus einem benutzerdefinierten Richtlinienpaket für Gastkonfigurationen für Windows erstellt, und der Name, eine Beschreibung und die Version werden angegeben:

```azurepowershell-interactive
New-GuestConfigurationPolicy
    -ContentUri 'https://storageaccountname.blob.core.windows.net/packages/AuditBitLocker.zip?st=2019-07-01T00%3A00%3A00Z&se=2024-07-01T00%3A00%3A00Z&sp=rl&sv=2018-03-28&sr=b&sig=JdUf4nOCo8fvuflOoX%2FnGo4sXqVfP5BYXHzTl3%2BovJo%3D' `
    -DisplayName 'Audit BitLocker Service.' `
    -Description 'Audit if BitLocker is not enabled on Windows machine.' `
    -Path '.\policyDefinitions' `
    -Platform 'Windows' `
    -Version 1.2.3.4 `
    -Verbose
```

Parameter des Cmdlets `New-GuestConfigurationPolicy`:

- **ContentUri**: Öffentlicher HTTP(S)-URI des Pakets mit dem Inhalt der Gastkonfiguration.
- **DisplayName**: Anzeigename der Richtlinie.
- **Beschreibung:** Beschreibung der Richtlinie.
- **Parameter**: Richtlinienparameter im Hashtabellenformat.
- **Version**: Version der Richtlinie.
- **Pfad**: Zielpfad, unter dem Richtliniendefinitionen erstellt werden.
- **Plattform**: Zielplattform (Windows/Linux) für das Paket mit den Richtlinien und dem Inhalt der Gastkonfiguration.

Mit `New-GuestConfigurationPolicy` werden die folgenden Dateien erstellt:

- **auditIfNotExists.json**
- **deployIfNotExists.json**
- **Initiative.json**

In der Ausgabe des Cmdlets wird ein Objekt zurückgegeben, das den Anzeigenamen der Initiative und den Pfad der Richtliniendateien enthält.

Wenn Sie diesen Befehl verwenden möchten, um ein Gerüst für ein Projekt mit benutzerdefinierten Richtlinien zu erstellen, können Sie Änderungen an diesen Dateien vornehmen. Ein Beispiel hierfür ist die Änderung des If-Abschnitts, um zu überprüfen, ob ein bestimmtes Tag für virtuelle Computer vorhanden ist. Ausführliche Informationen zur Erstellung von Richtlinien finden Sie unter [Programmgesteuertes Erstellen von Richtlinien](./programmatically-create.md).

### <a name="using-parameters-in-custom-guest-configuration-policies"></a>Verwenden von Parametern in benutzerdefinierten Richtlinien für Gastkonfigurationen

Die Gastkonfiguration unterstützt das Außerkraftsetzen von Eigenschaften einer Konfiguration während der Laufzeit. Dieses Feature bewirkt, dass die Werte in der MOF-Datei im Paket nicht als statisch angesehen werden müssen. Die Überschreibungswerte werden über Azure Policy bereitgestellt und wirken sich nicht darauf aus, wie die Konfigurationen erstellt oder kompiliert werden.

Die Cmdlets `New-GuestConfigurationPolicy` und `Test-GuestConfigurationPolicyPackage` enthalten einen Parameter mit dem Namen **Parameters**. Für diesen Parameter wird eine Hashtabellendefinition verwendet, die alle Details zu den einzelnen Parametern enthält, und es werden automatisch alle erforderlichen Abschnitte der Dateien erstellt, die für die Erstellung der einzelnen Azure Policy-Definitionen verwendet werden.

Im folgenden Beispiel wird eine Azure Policy-Richtlinie zum Überprüfen eines Diensts erstellt, bei der der Benutzer bei der Zuweisung der Richtlinie eine Auswahl aus einer Liste mit Diensten trifft.

```azurepowershell-interactive
$PolicyParameterInfo = @(
    @{
        Name = 'ServiceName'                                            # Policy parameter name (mandatory)
        DisplayName = 'windows service name.'                           # Policy parameter display name (mandatory)
        Description = "Name of the windows service to be audited."      # Policy parameter description (optional)
        ResourceType = "Service"                                        # DSC configuration resource type (mandatory)
        ResourceId = 'windowsService'                                   # DSC configuration resource property name (mandatory)
        ResourcePropertyName = "Name"                                   # DSC configuration resource property name (mandatory)
        DefaultValue = 'winrm'                                          # Policy parameter default value (optional)
        AllowedValues = @('BDESVC','TermService','wuauserv','winrm')    # Policy parameter allowed values (optional)
    }
)

New-GuestConfigurationPolicy
    -ContentUri 'https://storageaccountname.blob.core.windows.net/packages/AuditBitLocker.zip?st=2019-07-01T00%3A00%3A00Z&se=2024-07-01T00%3A00%3A00Z&sp=rl&sv=2018-03-28&sr=b&sig=JdUf4nOCo8fvuflOoX%2FnGo4sXqVfP5BYXHzTl3%2BovJo%3D' `
    -DisplayName 'Audit Windows Service.' `
    -Description 'Audit if a Windows Service is not enabled on Windows machine.' `
    -Path '.\policyDefinitions' `
    -Parameters $PolicyParameterInfo `
    -Platform 'Windows' `
    -Version 1.2.3.4 `
    -Verbose
```

Fügen Sie für Linux-Richtlinien die **AttributesYmlContent**-Eigenschaft in Ihre Konfiguration ein, und überschreiben Sie die Werte entsprechend. Der Gastkonfigurations-Agent erstellt automatisch die YAML-Datei, die von InSpec zum Speichern der Attribute genutzt wird. Betrachten Sie das folgende Beispiel.

```azurepowershell-interactive
Configuration FirewalldEnabled {

    Import-DscResource -ModuleName 'GuestConfiguration'

    Node FirewalldEnabled {

        ChefInSpecResource FirewalldEnabled {
            Name = 'FirewalldEnabled'
            AttributesYmlContent = "DefaultFirewalldProfile: [public]"
        }
    }
}
```

Fügen Sie dem Array für jeden zusätzlichen Parameter eine Hashtabelle hinzu. In den Richtliniendateien sehen Sie, dass der Gastkonfiguration „configurationName“ Eigenschaften hinzugefügt wurden, mit denen der Ressourcentyp, der Name, die Eigenschaft und der Wert identifiziert werden.

```json
{
    "apiVersion": "2018-11-20",
    "type": "Microsoft.Compute/virtualMachines/providers/guestConfigurationAssignments",
    "name": "[concat(parameters('vmName'), '/Microsoft.GuestConfiguration/', parameters('configurationName'))]",
    "location": "[parameters('location')]",
    "properties": {
        "guestConfiguration": {
            "name": "[parameters('configurationName')]",
            "version": "1.*",
            "configurationParameter": [{
                "name": "[Service]windowsService;Name",
                "value": "[parameters('ServiceName')]"
            }]
        }
    }
}
```

## <a name="publish-to-azure-policy"></a>Veröffentlichen in Azure Policy

Das Ressourcenmodul **GuestConfiguration** verfügt über eine Option zum gleichzeitigen Erstellen der Richtliniendefinitionen und der Initiativendefinition in Azure mit nur einem Schritt, indem das Cmdlet `Publish-GuestConfigurationPolicy` verwendet wird.
Das Cmdlet verfügt nur über den Parameter **Path**, mit dem auf den Speicherort der drei JSON-Dateien verwiesen wird, die mit `New-GuestConfigurationPolicy` erstellt werden.

```azurepowershell-interactive
Publish-GuestConfigurationPolicy -Path '.\policyDefinitions' -Verbose
```

Das Cmdlet `Publish-GuestConfigurationPolicy` akzeptiert den Pfad von der PowerShell-Pipeline. Dank dieses Features können Sie die Richtliniendateien erstellen und veröffentlichen, indem Sie nur eine Befehlszeile verwenden, in der die Befehle per Pipezeichen verknüpft sind.

```azurepowershell-interactive
New-GuestConfigurationPolicy -ContentUri 'https://storageaccountname.blob.core.windows.net/packages/AuditBitLocker.zip?st=2019-07-01T00%3A00%3A00Z&se=2024-07-01T00%3A00%3A00Z&sp=rl&sv=2018-03-28&sr=b&sig=JdUf4nOCo8fvuflOoX%2FnGo4sXqVfP5BYXHzTl3%2BovJo%3D' -DisplayName 'Audit BitLocker service.' -Description 'Audit if the BitLocker service is not enabled on Windows machine.' -Path '.\policyDefinitions' -Platform 'Windows' -Version 1.2.3.4 -Verbose | ForEach-Object {$_.Path} | Publish-GuestConfigurationPolicy -Verbose
```

Bei den in Azure erstellten Richtlinien- und Initiativendefinitionen ist der letzte Schritt das Zuweisen der Initiative. Informieren Sie sich darüber, wie Sie die Initiative per [Portal](../assign-policy-portal.md), [Azure CLI](../assign-policy-azurecli.md) oder [Azure PowerShell](../assign-policy-powershell.md) zuweisen können.

> [!IMPORTANT]
> Richtlinien für Gastkonfigurationen müssen **immer** über die Initiative zugewiesen werden, in der die Richtlinien _AuditIfNotExists_ und _DeployIfNotExists_ kombiniert sind. Wenn nur die Richtlinie _AuditIfNotExists_ zugewiesen wird, werden die erforderlichen Komponenten nicht bereitgestellt, und die Richtlinie zeigt immer an, dass „0“ Server konform sind.

## <a name="policy-lifecycle"></a>Lebenszyklus von Richtlinien

Nachdem Sie eine benutzerdefinierte Azure Policy-Richtlinie über das benutzerdefinierte Inhaltspaket veröffentlicht haben, müssen Sie zwei Felder aktualisieren, falls Sie ein neues Release veröffentlichen möchten.

- **Version**: Beim Ausführen des Cmdlets `New-GuestConfigurationPolicy` müssen Sie eine Versionsnummer angeben, die höher als die der derzeitigen Veröffentlichung ist. Mit dieser Eigenschaft wird die Version der Gastkonfigurationszuweisung in der neuen Richtliniendatei aktualisiert, damit die Erweiterung erkennt, dass das Paket aktualisiert wurde.
- **contentHash**: Diese Eigenschaft wird vom Cmdlet `New-GuestConfigurationPolicy` automatisch aktualisiert. Es handelt sich um einen Hashwert des Pakets, das mit `New-GuestConfigurationPackage` erstellt wurde. Diese Eigenschaft muss für die von Ihnen veröffentlichte Datei vom Typ `.zip` stimmen. Falls nur die Eigenschaft **contentUri** aktualisiert wird, z. B. wenn über das Portal eine manuelle Änderung an der Richtliniendefinition vorgenommen werden kann, wird das Inhaltspaket von der Erweiterung nicht akzeptiert.

Die einfachste Möglichkeit zum Freigeben eines aktualisierten Pakets ist das Wiederholen des Prozesses in diesem Artikel und das Angeben einer aktualisierten Versionsnummer. Mit dieser Vorgehensweise wird sichergestellt, dass alle Eigenschaften richtig aktualisiert wurden.

## <a name="converting-windows-group-policy-content-to-azure-policy-guest-configuration"></a>Konvertieren des Inhalts der Windows-Gruppenrichtlinie in eine Azure Policy-Gastkonfiguration

Bei der Überwachung von Windows-Computern ist die Gastkonfiguration eine Implementierung der Desired State Configuration-Syntax von PowerShell. Von der DSC-Community wurde ein Tool zum Konvertieren exportierter Gruppenrichtlinienvorlagen in das DSC-Format veröffentlicht. Sie können dieses Tool zusammen mit den oben beschriebenen Gastkonfigurations-Cmdlets verwenden, um den Inhalt der Windows-Gruppenrichtlinie zu konvertieren und zur Überwachung für Azure Policy zu packen und zu veröffentlichen. Ausführliche Informationen zur Verwendung des Tools finden Sie im Artikel [Schnellstart: Konvertieren von Gruppenrichtlinien in DSC](/powershell/scripting/dsc/quickstarts/gpo-quickstart).
Nach dem Konvertieren des Inhalts sind die obigen Schritte, mit denen ein Paket erstellt und in Azure Policy veröffentlicht wird, die gleichen wie bei jedem anderen DSC-Inhalt.

## <a name="optional-signing-guest-configuration-packages"></a>OPTIONAL: Signieren von Paketen für Gastkonfigurationen

Für benutzerdefinierte Richtlinien für Gastkonfigurationen wird standardmäßig der SHA256-Hash verwendet. Hiermit wird überprüft, ob sich das Richtlinienpaket zwischen der Veröffentlichung und dem Lesevorgang durch den zu überprüfenden Server geändert hat.
Optional können Kunden auch ein Zertifikat nutzen, um Pakete zu signieren und für die Gastkonfigurationserweiterung zu erzwingen, dass nur signierte Inhalte zulässig sind.

Für dieses Szenario müssen Sie zwei Schritte ausführen. Führen Sie das Cmdlet zum Signieren des Inhaltspakets aus, und fügen Sie ein Tag an die Computer an, auf denen das Signieren von Code erzwungen werden soll.

Führen Sie zum Verwenden des Features für die Signaturüberprüfung das Cmdlet `Protect-GuestConfigurationPackage` aus, um das Paket vor der Veröffentlichung zu signieren. Für dieses Cmdlet ist ein Zertifikat für das „Codesignieren“ erforderlich.

```azurepowershell-interactive
$Cert = Get-ChildItem -Path cert:\LocalMachine\My | Where-Object {($_.Subject-eq "CN=mycert") }
Protect-GuestConfigurationPackage -Path .\package\AuditWindowsService\AuditWindowsService.zip -Certificate $Cert -Verbose
```

Parameter des Cmdlets `Protect-GuestConfigurationPackage`:

- **Pfad**: Vollständiger Pfad des Pakets mit der Gastkonfiguration.
- **Zertifikat**: Codesignaturzertifikat zum Signieren des Pakets. Dieser Parameter wird nur beim Signieren von Inhalt für Windows unterstützt.
- **PrivateGpgKeyPath**: Pfad des privaten GPG-Schlüssels. Dieser Parameter wird nur beim Signieren von Inhalt für Linux unterstützt.
- **PublicGpgKeyPath**: Pfad des öffentlichen GPG-Schlüssels. Dieser Parameter wird nur beim Signieren von Inhalt für Linux unterstützt.

Vom GuestConfiguration-Agent wird erwartet, dass der öffentliche Schlüssel des Zertifikats auf Windows-Computern unter „Vertrauenswürdige Stammzertifizierungsstellen“ und auf Linux-Computern unter dem Pfad `/usr/local/share/ca-certificates/extra` vorhanden ist. Damit auf dem Knoten signierter Inhalt überprüft werden kann, müssen Sie den öffentlichen Schlüssel des Zertifikats auf dem Computer installieren, bevor Sie die benutzerdefinierte Richtlinie anwenden. Dieser Prozess kann mit einem beliebigen Verfahren auf der VM oder mit Azure Policy durchgeführt werden. Eine Beispielvorlage finden Sie [hier](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-push-certificate-windows).
In der Key Vault-Zugriffsrichtlinie muss es für den Computeressourcenanbieter zulässig sein, bei Bereitstellungen auf Zertifikate zuzugreifen. Informationen zu den ausführlichen Schritten finden Sie unter [Einrichten des Schlüsseltresors für virtuelle Computer in Azure Resource Manager](../../../virtual-machines/windows/key-vault-setup.md#use-templates-to-set-up-key-vault).

Hier ist ein Beispiel für den Export des öffentlichen Schlüssels aus einem Signaturzertifikat angegeben, um ihn dann auf dem Computer importieren zu können.

```azurepowershell-interactive
$Cert = Get-ChildItem -Path cert:\LocalMachine\My | Where-Object {($_.Subject-eq "CN=mycert3") } | Select-Object -First 1
$Cert | Export-Certificate -FilePath "$env:temp\DscPublicKey.cer" -Force
```

Eine gute Referenz zur Erstellung von GPG-Schlüsseln für die Nutzung mit Linux-Computern ist der Artikel [Generating a new GPG key](https://help.github.com/en/articles/generating-a-new-gpg-key) (Generieren eines neuen GPG-Schlüssels) auf GitHub.

Fügen Sie nach dem Veröffentlichen Ihres Inhalts ein Tag mit dem Namen `GuestConfigPolicyCertificateValidation` und dem Wert `enabled` an alle virtuellen Computer an, für die das Codesignieren erforderlich ist. Dieses Tag kann mit Azure Policy bedarfsabhängig bereitgestellt werden. Informationen hierzu finden Sie im Beispiel unter [Anwenden von Tags und den zugehörigen Standardwerten](../samples/apply-tag-default-value.md). Wenn dieses Tag vorhanden ist, ermöglicht die Richtliniendefinition, die mit dem Cmdlet `New-GuestConfigurationPolicy` generiert wurde, die Anforderung über die Gastkonfigurationserweiterung.

## <a name="preview-troubleshooting-guest-configuration-policy-assignments"></a>[VORSCHAU] Problembehandlung für Richtlinienzuweisungen für die Gastkonfiguration

Es gibt eine Vorschauversion eines Tools, das Sie bei der Problembehandlung für Zuweisungen für die Azure Policy-Gastkonfiguration unterstützt. Das Tool befindet sich in der Vorschauphase und wurde als Modul namens [GuestConfigurationTroubleshooter](https://www.powershellgallery.com/packages/GuestConfigurationTroubleshooter/) im PowerShell-Katalog veröffentlicht.

Verwenden Sie zum Abrufen weiterer Informationen zu den Cmdlets in diesem Tool den Befehl „Get-Help“ in PowerShell, um den integrierten Leitfaden anzuzeigen. Da das Tool regelmäßig aktualisiert wird, ist dies die beste Option, um aktuelle Informationen zu erhalten.

## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich über die Überprüfung von VMs mit [Gastkonfiguration](../concepts/guest-configuration.md).
- Informieren Sie sich über das [programmgesteuerte Erstellen von Richtlinien](programmatically-create.md).
- Informieren Sie sich über das [Abrufen von Konformitätsdaten](getting-compliance-data.md).