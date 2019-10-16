---
title: Bereitstellen von VM-Erweiterungen mithilfe von Azure Resource Manager-Vorlagen | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Azure-VM-Erweiterungen mithilfe von Azure Resource Manager-Vorlagen bereitstellen.
services: azure-resource-manager
documentationcenter: ''
author: mumian
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 11/13/2018
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 338054aadbf04c6c6e2b496677476c2c5634b6ba
ms.sourcegitcommit: aef6040b1321881a7eb21348b4fd5cd6a5a1e8d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2019
ms.locfileid: "72169302"
---
# <a name="tutorial-deploy-virtual-machine-extensions-with-azure-resource-manager-templates"></a>Tutorial: Bereitstellen von VM-Erweiterungen mithilfe von Azure Resource Manager-Vorlagen

Hier erfahren Sie, wie Sie [Azure-VM-Erweiterungen](../virtual-machines/extensions/features-windows.md) verwenden, um nach der Bereitstellung Konfigurations- und Automatisierungsaufgaben für virtuelle Azure-Computer auszuführen. Für die Verwendung mit virtuellen Azure-Computern stehen viele verschiedene VM-Erweiterungen zur Verfügung. In diesem Tutorial stellen Sie eine benutzerdefinierte Skripterweiterung über eine Azure Resource Manager-Vorlage bereit, um ein PowerShell-Skript auf einem virtuellen Windows-Computer auszuführen.  Das Skript installiert einen Webserver auf dem virtuellen Computer.

Dieses Tutorial enthält die folgenden Aufgaben:

> [!div class="checklist"]
> * Vorbereiten eines PowerShell-Skripts
> * Öffnen einer Schnellstartvorlage
> * Bearbeiten der Vorlage
> * Bereitstellen der Vorlage
> * Überprüfen der Bereitstellung

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Damit Sie die Anweisungen in diesem Artikel ausführen können, benötigen Sie Folgendes:

* [Visual Studio Code](https://code.visualstudio.com/) mit der Erweiterung „Azure Resource Manager-Tools“. Informationen finden Sie unter [Schnellstart: Erstellen von Azure Resource Manager-Vorlagen mit Visual Studio Code](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites).
* Verwenden Sie aus Sicherheitsgründen ein generiertes Kennwort für das Administratorkonto des virtuellen Computers. Hier sehen Sie ein Beispiel für die Kennwortgenerierung:

    ```azurecli-interactive
    openssl rand -base64 32
    ```

    Azure Key Vault dient zum Schützen von kryptografischen Schlüsseln und anderen Geheimnissen. Weitere Informationen finden Sie unter [Tutorial: Integrieren von Azure Key Vault in die Resource Manager-Vorlagenbereitstellung](./resource-manager-tutorial-use-key-vault.md). Wir empfehlen Ihnen auch, Ihr Kennwort alle drei Monate zu aktualisieren.

## <a name="prepare-a-powershell-script"></a>Vorbereiten eines PowerShell-Skripts

Ein PowerShell-Skript mit folgendem Inhalt ist auf [GitHub](https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/tutorial-vm-extension/installWebServer.ps1) verfügbar:

```azurepowershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

Wenn Sie die Datei an Ihrem eigenen Standort veröffentlichen möchten, müssen Sie später in diesem Tutorial das Element `fileUri` in der Vorlage aktualisieren.

## <a name="open-a-quickstart-template"></a>Öffnen einer Schnellstartvorlage

Azure-Schnellstartvorlagen ist ein Repository für Resource Manager-Vorlagen. Statt eine Vorlage von Grund auf neu zu erstellen, können Sie eine Beispielvorlage verwenden und diese anpassen. Die in diesem Tutorial verwendete Vorlage heißt [Deploy a simple Windows VM](https://azure.microsoft.com/resources/templates/101-vm-simple-windows/) (Bereitstellen eines einfachen virtuellen Windows-Computers).

1. Wählen Sie in Visual Studio Code **Datei** > **Datei öffnen** aus.
1. Fügen Sie in das Feld **Dateiname** die folgende URL ein: https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json.

1. Wählen Sie **Öffnen** aus, um die Datei zu öffnen.
    Die Vorlage definiert fünf Ressourcen:

   * **Microsoft.Storage/storageAccounts**. Informationen finden Sie in der [Vorlagenreferenz](https://docs.microsoft.com/azure/templates/Microsoft.Storage/storageAccounts).
   * **Microsoft.Network/publicIPAddresses**. Informationen finden Sie in der [Vorlagenreferenz](https://docs.microsoft.com/azure/templates/microsoft.network/publicipaddresses).
   * **Microsoft.Network/virtualNetworks**. Informationen finden Sie in der [Vorlagenreferenz](https://docs.microsoft.com/azure/templates/microsoft.network/virtualnetworks).
   * **Microsoft.Network/networkInterfaces**. Informationen finden Sie in der [Vorlagenreferenz](https://docs.microsoft.com/azure/templates/microsoft.network/networkinterfaces).
   * **Microsoft.Compute/virtualMachines**. Informationen finden Sie in der [Vorlagenreferenz](https://docs.microsoft.com/azure/templates/microsoft.compute/virtualmachines).

     Bevor Sie die Vorlage anpassen, sollten Sie sich zunächst grundlegend damit vertraut machen.

1. Wählen Sie **Datei** > **Speichern unter** aus, um eine Kopie der Datei als *azuredeploy.json* auf dem lokalen Computer zu speichern.

## <a name="edit-the-template"></a>Bearbeiten der Vorlage

Fügen Sie der vorhandenen Vorlage eine VM-Erweiterungsressource mit folgendem Inhalt hinzu:

```json
{
    "apiVersion": "2018-06-01",
    "type": "Microsoft.Compute/virtualMachines/extensions",
    "name": "[concat(variables('vmName'),'/', 'InstallWebServer')]",
    "location": "[parameters('location')]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/',variables('vmName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.Compute",
        "type": "CustomScriptExtension",
        "typeHandlerVersion": "1.7",
        "autoUpgradeMinorVersion":true,
        "settings": {
            "fileUris": [
                "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/tutorial-vm-extension/installWebServer.ps1"
            ],
            "commandToExecute": "powershell.exe -ExecutionPolicy Unrestricted -File installWebServer.ps1"
        }
    }
}
```

Weitere Informationen zu dieser Ressourcendefinition finden Sie in der [Erweiterungsreferenz](https://docs.microsoft.com/azure/templates/microsoft.compute/virtualmachines/extensions). Im Anschluss sind einige zentrale Elemente aufgeführt:

* **name:** Da es sich bei der Erweiterungsressource um eine untergeordnete Ressource des VM-Objekts handelt, muss der Name mit dem VM-Namenspräfix versehen werden. Weitere Informationen finden Sie unter [Festlegen von Name und Typ für untergeordnete Ressourcen](child-resource-name-type.md).
* **dependsOn:** Erstellen Sie die Erweiterungsressource nach der Erstellung des virtuellen Computers.
* **fileUris:** Die Speicherorte der Skriptdateien. Falls Sie sich gegen die Verwendung des angegebenen Speicherorts entscheiden, müssen Sie die Werte aktualisieren.
* **commandToExecute:** Dieser Befehl ruft das Skript auf.

## <a name="deploy-the-template"></a>Bereitstellen der Vorlage

Informationen zum Bereitstellungsverfahren finden Sie im Abschnitt „Bereitstellen der Vorlage“ unter [Tutorial: Erstellen von Azure Resource Manager-Vorlagen mit abhängigen Ressourcen](./resource-manager-tutorial-create-templates-with-dependent-resources.md#deploy-the-template) verwendet haben. Es empfiehlt sich, ein generiertes Kennwort für das Administratorkonto des virtuellen Computers zu verwenden. Informationen finden Sie in diesem Artikel im Abschnitt [Voraussetzungen](#prerequisites).

## <a name="verify-the-deployment"></a>Überprüfen der Bereitstellung

1. Wählen Sie im Azure-Portal den virtuellen Computer aus.
1. Kopieren Sie in der VM-Übersicht die IP-Adresse durch Auswählen von **Klicken Sie zum Kopieren.** , und fügen Sie sie anschließend in eine Browserregisterkarte ein. Die Standardbegrüßungsseite für IIS (Internet Information Services, Internetinformationsdienste) wird geöffnet:

![Die Begrüßungsseite von Internetinformationsdienste](./media/resource-manager-tutorial-deploy-vm-extensions/resource-manager-template-deploy-extensions-customer-script-web-server.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die bereitgestellten Azure-Ressourcen nicht mehr benötigen, bereinigen Sie sie, indem Sie die Ressourcengruppe löschen.

1. Klicken Sie im Azure-Portal im linken Bereich auf **Ressourcengruppe**.
2. Geben Sie in das Feld **Nach Name filtern** den Namen der Ressourcengruppe ein.
3. Klicken Sie auf den Namen der Ressourcengruppe.
    Sechs Ressourcen werden in der Ressourcengruppe angezeigt.
4. Wählen Sie im oberen Menü **Ressourcengruppe löschen** aus.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie einen virtuellen Computer und eine VM-Erweiterung bereitgestellt. Die Erweiterung hat den IIS-Webserver auf dem virtuellen Computer installiert. Informationen zum Importieren einer BACPAC-Datei unter Verwendung der Azure SQL-Datenbank-Erweiterung finden Sie hier:

> [!div class="nextstepaction"]
> [Bereitstellen von SQL-Erweiterungen](./resource-manager-tutorial-deploy-sql-extensions-bacpac.md)
