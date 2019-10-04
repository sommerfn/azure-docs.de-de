---
title: Importieren von SQL-BACPAC-Dateien mit Azure Resource Manager-Vorlagen | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie die SQL-Datenbank-Erweiterung verwenden, um SQL-BACPAC-Dateien mit Azure Resource Manager-Vorlagen zu importieren.
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: ''
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 04/08/2019
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 08befabfbd14651475fa56dec95bdf4c2fe54c9c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "60390314"
---
# <a name="tutorial-import-sql-bacpac-files-with-azure-resource-manager-templates"></a>Tutorial: Importieren von SQL-BACPAC-Dateien mit Azure Resource Manager-Vorlagen

Hier erfahren Sie, wie Sie die Azure SQL-Datenbank-Erweiterung verwenden, um eine BACPAC-Datei mit Azure Resource Manager-Vorlagen zu importieren. Bereitstellungsartefakte sind sämtliche Dateien, die zusätzlich zur Hauptvorlagendatei für eine Bereitstellung benötigt werden. Die BACPAC-Datei ist ein Artefakt. In diesem Tutorial erstellen Sie eine Vorlage zum Bereitstellen einer Azure SQL Server-Instanz und einer SQL-Datenbank sowie zum Importieren einer BACPAC-Datei. Informationen zum Bereitstellen von Azure-VM-Erweiterungen unter Verwendung von Azure Resource Manager-Vorlagen finden Sie unter [Tutorial: Bereitstellen von VM-Erweiterungen mithilfe von Azure Resource Manager-Vorlagen](./resource-manager-tutorial-deploy-vm-extensions.md).

Dieses Tutorial enthält die folgenden Aufgaben:

> [!div class="checklist"]
> * Vorbereiten einer BACPAC-Datei
> * Öffnen einer Schnellstartvorlage
> * Bearbeiten der Vorlage
> * Bereitstellen der Vorlage
> * Überprüfen der Bereitstellung

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Damit Sie die Anweisungen in diesem Artikel ausführen können, benötigen Sie Folgendes:

* [Visual Studio Code](https://code.visualstudio.com/) mit der Erweiterung „Azure Resource Manager-Tools“. Informationen finden Sie unter [Schnellstart: Erstellen von Azure Resource Manager-Vorlagen mit Visual Studio Code](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites).
* Verwenden Sie aus Sicherheitsgründen ein generiertes Kennwort für das SQL Server-Administratorkonto. Hier sehen Sie ein Beispiel für die Kennwortgenerierung:

    ```azurecli-interactive
    openssl rand -base64 32
    ```
    Azure Key Vault dient zum Schützen von kryptografischen Schlüsseln und anderen Geheimnissen. Weitere Informationen finden Sie unter [Tutorial: Integrieren von Azure Key Vault in die Resource Manager-Vorlagenbereitstellung](./resource-manager-tutorial-use-key-vault.md). Wir empfehlen Ihnen auch, Ihr Kennwort alle drei Monate zu aktualisieren.

## <a name="prepare-a-bacpac-file"></a>Vorbereiten einer BACPAC-Datei

Eine BACPAC-Datei wird über ein [Azure Storage-Konto](https://armtutorials.blob.core.windows.net/sqlextensionbacpac/SQLDatabaseExtension.bacpac) mit öffentlichem Zugriff freigegeben. Informationen zum Erstellen einer eigenen Datei finden Sie unter [Exportieren einer Azure SQL-Datenbank in eine BACPAC-Datei](../sql-database/sql-database-export.md). Wenn Sie die Datei an Ihrem eigenen Standort veröffentlichen möchten, müssen Sie die Vorlage später in diesem Tutorial aktualisieren.

## <a name="open-a-quickstart-template"></a>Öffnen einer Schnellstartvorlage

Die in diesem Tutorial verwendete Vorlage befindet sich in einem [Azure Storage-Konto](https://armtutorials.blob.core.windows.net/createsql/azuredeploy.json). 

1. Wählen Sie in Visual Studio Code **Datei**>**Datei öffnen** aus.
2. Fügen Sie in **Dateiname** die folgende URL ein:

    ```url
    https://armtutorials.blob.core.windows.net/createsql/azuredeploy.json
    ```
3. Wählen Sie **Öffnen** aus, um die Datei zu öffnen.

    In der Vorlage sind drei Ressourcen definiert:

   * `Microsoft.Sql/servers`. Informationen finden Sie in der [Vorlagenreferenz](https://docs.microsoft.com/azure/templates/microsoft.sql/servers).
   * `Microsoft.SQL/servers/securityAlertPolicies`. Informationen finden Sie in der [Vorlagenreferenz](https://docs.microsoft.com/azure/templates/microsoft.sql/servers/securityalertpolicies).
   * `Microsoft.SQL.servers/databases`.  Informationen finden Sie in der [Vorlagenreferenz](https://docs.microsoft.com/azure/templates/microsoft.sql/servers/databases).

     Bevor Sie die Vorlage anpassen, sollten Sie sich zunächst grundlegend damit vertraut machen.
4. Wählen Sie **Datei**>**Speichern unter** aus, um eine Kopie der Datei als **azuredeploy.json** auf dem lokalen Computer zu speichern.

## <a name="edit-the-template"></a>Bearbeiten der Vorlage

Fügen Sie der Vorlage zwei weitere Ressourcen hinzu.

* Damit die SQL-Datenbank-Erweiterung BACPAC-Dateien importieren kann, müssen Sie den Zugriff auf Azure-Dienste zulassen. Fügen Sie der SQL Server-Definition den folgenden JSON-Code hinzu:

    ```json
    {
        "type": "firewallrules",
        "name": "AllowAllAzureIps",
        "location": "[parameters('location')]",
        "apiVersion": "2015-05-01-preview",
        "dependsOn": [
            "[variables('databaseServerName')]"
        ],
        "properties": {
            "startIpAddress": "0.0.0.0",
            "endIpAddress": "0.0.0.0"
        }
    }
    ```

    Die Vorlage sollte wie folgt aussehen:

    ![Azure Resource Manager: Bereitstellen von SQL-Erweiterungen (BACPAC)](./media/resource-manager-tutorial-deploy-sql-extensions-bacpac/resource-manager-tutorial-deploy-sql-extensions-bacpac-firewall.png)

* Fügen Sie der Datenbankdefinition mithilfe des folgenden JSON-Codes eine SQL-Datenbank-Erweiterungsressource hinzu:

    ```json
    "resources": [
        {
            "name": "Import",
            "type": "extensions",
            "apiVersion": "2014-04-01",
            "dependsOn": [
                "[resourceId('Microsoft.Sql/servers/databases', variables('databaseServerName'), variables('databaseName'))]"
            ],
            "properties": {
                "storageKeyType": "SharedAccessKey",
                "storageKey": "?",
                "storageUri": "https://armtutorials.blob.core.windows.net/sqlextensionbacpac/SQLDatabaseExtension.bacpac",
                "administratorLogin": "[variables('databaseServerAdminLogin')]",
                "administratorLoginPassword": "[variables('databaseServerAdminLoginPassword')]",
                "operationMode": "Import",
            }
        }
    ]
    ```

    Die Vorlage sollte wie folgt aussehen:

    ![Azure Resource Manager: Bereitstellen von SQL-Erweiterungen (BACPAC)](./media/resource-manager-tutorial-deploy-sql-extensions-bacpac/resource-manager-tutorial-deploy-sql-extensions-bacpac.png)

    Informationen zur Ressourcendefinition finden Sie in der [Referenz zur SQL-Datenbank-Erweiterung](https://docs.microsoft.com/azure/templates/microsoft.sql/servers/databases/extensions). Im Anschluss sind einige zentrale Elemente aufgeführt:

    * **dependsOn:** Die Erweiterungsressource muss nach der Erstellung der SQL-Datenbank erstellt werden.
    * **storageKeyType:** Die Art des zu verwendenden Speicherschlüssels. Der Wert kann entweder `StorageAccessKey` oder `SharedAccessKey` lauten. Da die bereitgestellte BACPAC-Datei über ein Azure Storage-Konto mit öffentlichem Zugriff freigegeben wird, wird hier „SharedAccessKey“ verwendet.
    * **storageKey:** Der zu verwendende Speicherschlüssel. Bei Verwendung des Speicherschlüsseltyps „SharedAccessKey“ muss ein Fragezeichen (?) vorangestellt werden.
    * **storageUri:** Der zu verwendende Speicher-URI. Falls Sie sich gegen die Verwendung der bereitgestellten BACPAC-Datei entscheiden, müssen Sie die Werte aktualisieren.
    * **administratorLoginPassword:** Das Kennwort des SQL-Administrators. Verwenden Sie ein generiertes Kennwort. Siehe [Voraussetzungen](#prerequisites).

## <a name="deploy-the-template"></a>Bereitstellen der Vorlage

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Informationen zum Bereitstellungsverfahren finden Sie im Abschnitt [Bereitstellen der Vorlage](./resource-manager-tutorial-create-templates-with-dependent-resources.md#deploy-the-template). Verwenden Sie stattdessen das folgende PowerShell-Bereitstellungsskript:

```azurepowershell
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$adminUsername = Read-Host -Prompt "Enter the SQL admin username"
$adminPassword = Read-Host -Prompt "Enter the admin password" -AsSecureString

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -adminUser $adminUsername `
    -adminPassword $adminPassword `
    -TemplateFile "$HOME/azuredeploy.json"
```

Verwenden Sie ein generiertes Kennwort. Siehe [Voraussetzungen](#prerequisites).

## <a name="verify-the-deployment"></a>Überprüfen der Bereitstellung

Wählen Sie im Portal die SQL-Datenbank aus der neu bereitgestellten Ressourcengruppe aus. Wählen Sie **Abfrage-Editor (Vorschau)** aus, und geben Sie dann die Administratoranmeldeinformationen ein. Wie Sie sehen, wurden zwei Tabellen in die Datenbank importiert:

![Azure Resource Manager: Bereitstellen von SQL-Erweiterungen (BACPAC)](./media/resource-manager-tutorial-deploy-sql-extensions-bacpac/resource-manager-tutorial-deploy-sql-extensions-bacpac-query-editor.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Azure-Ressourcen nicht mehr benötigen, löschen Sie die Ressourcengruppe, um die bereitgestellten Ressourcen zu bereinigen.

1. Wählen Sie im Azure-Portal im linken Menü die Option **Ressourcengruppe** aus.
2. Geben Sie den Namen der Ressourcengruppe in das Feld **Nach Name filtern** ein.
3. Wählen Sie den Namen der Ressourcengruppe aus.  Es werden insgesamt sechs Ressourcen in der Ressourcengruppe angezeigt.
4. Wählen Sie **Ressourcengruppe löschen** aus dem Menü ganz oben aus.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie eine SQL Server-Instanz und eine SQL-Datenbank bereitgestellt und eine BACPAC-Datei importiert. Die BACPAC-Datei wird in einem Azure-Speicherkonto gespeichert. Jeder Benutzer, der über die URL verfügt, kann auf die Datei zugreifen. Informationen zum Schützen der BACPAC-Datei (Artefakt) finden Sie unter

> [!div class="nextstepaction"]
> [Tutorial: Secure artifacts in Azure Resource Manager template deployments](./resource-manager-tutorial-secure-artifacts.md) (Tutorial: Schützen von Artefakten in Azure Resource Manager-Vorlagenbereitstellungen).
