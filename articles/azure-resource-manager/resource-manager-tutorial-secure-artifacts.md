---
title: 'Schützen von Artefakten: Azure Resource Manager-Vorlagen'
description: Es wird beschrieben, wie Sie die Artefakte schützen, die in Ihren Azure Resource Manager-Vorlagen verwendet werden.
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: ''
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 10/08/2019
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: d9570c30e2870a136bd97ed5fc2304d6f0d280c9
ms.sourcegitcommit: f29fec8ec945921cc3a89a6e7086127cc1bc1759
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "72528274"
---
# <a name="tutorial-secure-artifacts-in-azure-resource-manager-template-deployments"></a>Tutorial: Schützen von Artefakten in Bereitstellungen per Azure Resource Manager-Vorlage

Hier wird beschrieben, wie Sie die in Ihren Azure Resource Manager-Vorlagen verwendeten Artefakte mit dem Azure Storage-Konto anhand von Shared Access Signatures (SAS) schützen. Bereitstellungsartefakte sind sämtliche Dateien, die zusätzlich zur Hauptvorlagendatei für eine Bereitstellung benötigt werden. Unter [Tutorial: Importieren von SQL-BACPAC-Dateien mit Azure Resource Manager-Vorlagen](./resource-manager-tutorial-deploy-sql-extensions-bacpac.md) wird mit der Hauptvorlage beispielsweise eine Azure SQL-Datenbank erstellt. Außerdem wird eine BACPAC-Datei aufgerufen, um Tabellen zu erstellen und Daten einzufügen. Die BACPAC-Datei ist ein Artefakt. Das Artefakt wird unter einem Azure-Speicherkonto mit öffentlichem Zugriff gespeichert. In diesem Tutorial verwenden Sie SAS, um in Ihrem eigenen Azure Storage-Konto begrenzten Zugriff auf die BACPAC-Datei zu gewähren. Weitere Informationen zu SAS finden Sie unter [Verwenden von Shared Access Signatures (SAS)](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

Informationen zum Schützen einer verknüpften Vorlage finden Sie unter [Tutorial: Erstellen verknüpfter Azure Resource Manager-Vorlagen](./resource-manager-tutorial-create-linked-templates.md).

Dieses Tutorial enthält die folgenden Aufgaben:

> [!div class="checklist"]
> * Vorbereiten einer BACPAC-Datei
> * Öffnen einer vorhandenen Vorlage
> * Bearbeiten der Vorlage
> * Bereitstellen der Vorlage
> * Überprüfen der Bereitstellung

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Damit Sie die Anweisungen in diesem Artikel ausführen können, benötigen Sie Folgendes:

* [Visual Studio Code](https://code.visualstudio.com/) mit der Erweiterung „Azure Resource Manager-Tools“. Informationen finden Sie unter [Schnellstart: Erstellen von Azure Resource Manager-Vorlagen mit Visual Studio Code](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites).
* Sehen Sie sich [Tutorial: Importieren von SQL-BACPAC-Dateien mit Azure Resource Manager-Vorlagen](./resource-manager-tutorial-deploy-sql-extensions-bacpac.md) an. Die hier verwendete Vorlage wird in diesem Tutorial entwickelt. Dieser Artikel enthält einen Link zum Herunterladen der fertigen Vorlage.
* Verwenden Sie aus Sicherheitsgründen ein generiertes Kennwort für das SQL Server-Administratorkonto. Hier sehen Sie ein Beispiel für die Kennwortgenerierung:

    ```azurecli-interactive
    openssl rand -base64 32
    ```
    Azure Key Vault dient zum Schützen von kryptografischen Schlüsseln und anderen Geheimnissen. Weitere Informationen finden Sie unter [Tutorial: Integrieren von Azure Key Vault in die Resource Manager-Vorlagenbereitstellung](./resource-manager-tutorial-use-key-vault.md). Wir empfehlen Ihnen auch, Ihr Kennwort alle drei Monate zu aktualisieren.

## <a name="prepare-a-bacpac-file"></a>Vorbereiten einer BACPAC-Datei

In diesem Abschnitt bereiten Sie die BACPAC-Datei vor, damit der sichere Zugriff auf die Datei möglich ist, wenn Sie die Resource Manager-Vorlage bereitstellen. Dieser Abschnitt enthält fünf Verfahren:

* Herunterladen der BACPAC-Datei
* Erstellen eines Azure-Speicherkontos.
* Erstellen eines Speicherkonto-Blobcontainers
* Hochladen der BACPAC-Datei in den Container
* Abrufen des SAS-Tokens der BACPAC-Datei

Informationen zur Automatisierung dieser Schritte mit einem PowerShell-Skript finden Sie im Skript unter [Hochladen der verknüpften Vorlage](./resource-manager-tutorial-create-linked-templates.md#upload-the-linked-template).

### <a name="download-the-bacpac-file"></a>Herunterladen der BACPAC-Datei

Laden Sie die [BACPAC-Datei](https://github.com/Azure/azure-docs-json-samples/raw/master/tutorial-sql-extension/SQLDatabaseExtension.bacpac) herunter, und speichern Sie die Datei unter demselben Namen (**SQLDatabaseExtension.bacpac**) auf Ihrem lokalen Computer.

### <a name="create-a-storage-account"></a>Speicherkonto erstellen

1. Wählen Sie das folgende Bild aus, um eine Resource Manager-Vorlage im Azure-Portal zu öffnen.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3a%2f%2fraw.githubusercontent.com%2fAzure%2fazure-quickstart-templates%2fmaster%2f101-storage-account-create%2fazuredeploy.json" target="_blank"><img src="./media/resource-manager-tutorial-secure-artifacts/deploy-to-azure.png" alt="Deploy to Azure"></a>
2. Geben Sie die folgenden Eigenschaften ein:

    * **Abonnement**: Wählen Sie Ihr Azure-Abonnement.
    * **Ressourcengruppe**: Wählen Sie **Neu erstellen**, und vergeben Sie einen Namen. Eine Ressourcengruppe ist ein Container für Azure-Ressourcen, der für Verwaltungszwecke bestimmt ist. In diesem Tutorial können Sie für das Speicherkonto und die Azure SQL-Datenbank dieselbe Ressourcengruppe verwenden. Notieren Sie den Namen dieser Ressourcengruppe. Sie benötigen ihn später beim Erstellen der Azure SQL-Datenbank in den Tutorials.
    * **Standort**: Wählen Sie eine Region aus. Beispiel: **USA, Mitte**.
    * **Speicherkontotyp**: Verwenden Sie den Standardwert **Standard_LRS**.
    * **Standort**: Verwenden Sie den Standardwert **[resourceGroup().location]** . Dies bedeutet, dass Sie den Standort der Ressourcengruppe für das Speicherkonto verwenden.
    * **Ich stimme den oben genannten Geschäftsbedingungen zu**: (aktiviert)
3. Wählen Sie die Option **Kaufen**.
4. Wählen Sie oben rechts im Portal das Benachrichtigungssymbol (Glockensymbol), um den Bereitstellungsstatus anzuzeigen.

    ![Resource Manager-Tutorial: Benachrichtigungsbereich im Portal](./media/resource-manager-tutorial-secure-artifacts/resource-manager-tutorial-portal-notifications-pane.png)
5. Wählen Sie nach der erfolgreichen Bereitstellung des Speicherkontos im Benachrichtigungsbereich die Option **Zu Ressourcengruppe wechseln**, um die Ressourcengruppe zu öffnen.

### <a name="create-a-blob-container"></a>Erstellen eines Blobcontainers

Sie benötigen einen Blobcontainer, bevor Sie Dateien hochladen können.

1. Wählen Sie das Speicherkonto aus, um es zu öffnen. In der Ressourcengruppe ist nur ein Speicherkonto aufgeführt. Der Name Ihres Speicherkontos unterscheidet sich vom Namen im folgenden Screenshot.

    ![Resource Manager-Tutorial – Speicherkonto](./media/resource-manager-tutorial-secure-artifacts/resource-manager-tutorial-storage-account.png)

2. Wählen Sie die Kachel **Blobs**.

    ![Resource Manager-Tutorial – Blobs](./media/resource-manager-tutorial-secure-artifacts/resource-manager-tutorial-blobs.png)
3. Wählen Sie oben die Option **+ Container**, um einen neuen Container zu erstellen.
4. Geben Sie die folgenden Werte ein:

    * **Name**: Geben Sie **sqlbacpac** ein.
    * **Öffentliche Zugriffsebene**: Verwenden Sie den Standardwert **Privat (kein anonymer Zugriff)** .
5. Klicken Sie auf **OK**.
6. Wählen Sie **sqlbacpac**, um den neu erstellten Container zu öffnen.

### <a name="upload-the-bacpac-file-to-the-container"></a>Hochladen der BACPAC-Datei in den Container

1. Wählen Sie die Option **Hochladen**.
2. Geben Sie die folgenden Werte ein:

    * **Dateien**: Befolgen Sie die Anleitung zum Auswählen der BACPAC-Datei, die Sie weiter oben heruntergeladen haben. Der Standardname lautet **SQLDatabaseExtension.bacpac**.
    * **Authentifizierungstyp**: Wählen Sie **SAS**.  *SAS* ist der Standardwert.
3. Wählen Sie die Option **Hochladen**.  Nachdem die Datei erfolgreich hochgeladen wurde, wird der Dateiname im Container angezeigt.

### <a name="a-namegenerate-a-sas-token-generate-a-sas-token"></a><a name="generate-a-sas-token" />Generieren eines SAS-Tokens

1. Klicken Sie im Container mit der rechten Maustaste auf **SQLDatabaseExtension.bacpac**, und wählen Sie anschließend **SAS generieren**.
2. Geben Sie die folgenden Werte ein:

    * **Berechtigung**: Verwenden Sie die Standardeinstellung **Lesen**.
    * **Datum/Uhrzeit für Start und Ablauf**: Bei Verwendung des Standardwerts haben Sie acht Stunden Zeit für die Nutzung des SAS-Tokens. Falls Sie mehr Zeit zum Durcharbeiten dieses Tutorials benötigen, können Sie den Wert für **Ablauf** ändern.
    * **Zulässige IP-Adressen**: Lassen Sie dieses Feld leer.
    * **Zulässige Protokolle**: Verwenden Sie den Standardwert: **HTTPS**.
    * **Signaturschlüssel**: Verwenden Sie den Standardwert: **Schlüssel 1**.
3. Wählen Sie **Blob-SAS-Token und URL generieren**.
4. Erstellen Sie eine Kopie der **Blob-SAS-URL**. In der Mitte der URL ist der Dateiname **SQLDatabaseExtension.bacpac** angegeben.  Der Dateiname unterteilt die URL in drei Teile:

   - **Artefaktspeicherort**: https://xxxxxxxxxxxxxx.blob.core.windows.net/sqlbacpac/. Achten Sie darauf, dass der Speicherort mit „/“ endet.
   - **BACPAC-Dateiname**: „SQLDatabaseExtension.bacpac“.
   - **SAS-Token des Artefaktspeicherorts**: Stellen Sie sicher, dass dem Token ein „?“ vorangestellt ist.

     Sie benötigen diese drei Werte im Abschnitt [Bereitstellen der Vorlage](#deploy-the-template).

## <a name="open-an-existing-template"></a>Öffnen einer vorhandenen Vorlage

In dieser Sitzung ändern Sie die Vorlage, die Sie unter [Tutorial: Importieren von SQL-BACPAC-Dateien mit Azure Resource Manager-Vorlagen](./resource-manager-tutorial-deploy-sql-extensions-bacpac.md) erstellt haben, um die BACPAC-Datei mit einem SAS-Token aufzurufen.  Die im Tutorial zur SQL-Erweiterung entwickelte Vorlage ist auf [GitHub](https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/tutorial-sql-extension/azuredeploy.json) verfügbar.

1. Wählen Sie in Visual Studio Code **Datei**>**Datei öffnen** aus.
2. Fügen Sie in **Dateiname** die folgende URL ein:

    ```url
    https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/tutorial-sql-extension/azuredeploy.json
    ```
3. Wählen Sie **Öffnen** aus, um die Datei zu öffnen.

    Es gibt fünf Ressourcen, die in der Vorlage definiert werden:

   * `Microsoft.Sql/servers`. Informationen finden Sie in der [Vorlagenreferenz](https://docs.microsoft.com/azure/templates/microsoft.sql/2015-05-01-preview/servers).
   * `Microsoft.SQL/servers/securityAlertPolicies`. Informationen finden Sie in der [Vorlagenreferenz](https://docs.microsoft.com/azure/templates/microsoft.sql/2014-04-01/servers/databases/securityalertpolicies).
   * `Microsoft.SQL/servers/filewallRules`. Informationen finden Sie in der [Vorlagenreferenz](https://docs.microsoft.com/azure/templates/microsoft.sql/2015-05-01-preview/servers/firewallrules).
   * `Microsoft.SQL/servers/databases`.  Informationen finden Sie in der [Vorlagenreferenz](https://docs.microsoft.com/azure/templates/microsoft.sql/servers/databases).
   * `Microsoft.SQL/server/databases/extensions`.  Informationen finden Sie in der [Vorlagenreferenz](https://docs.microsoft.com/azure/templates/microsoft.sql/2014-04-01/servers/databases/extensions).

     Bevor Sie die Vorlage anpassen, sollten Sie sich zunächst grundlegend damit vertraut machen.
4. Wählen Sie **Datei**>**Speichern unter** aus, um eine Kopie der Datei als **azuredeploy.json** auf dem lokalen Computer zu speichern.

## <a name="edit-the-template"></a>Bearbeiten der Vorlage

Fügen Sie die folgenden zusätzlichen Parameter hinzu:

```json
"_artifactsLocation": {
    "type": "string",
    "metadata": {
        "description": "The base URI where artifacts required by this template are located."
    }
},
"_artifactsLocationSasToken": {
    "type": "securestring",
    "metadata": {
        "description": "The sasToken required to access _artifactsLocation."
    },
    "defaultValue": ""
},
"bacpacFileName": {
    "type": "string",
    "defaultValue": "SQLDatabaseExtension.bacpac",
    "metadata": {
        "description": "The bacpac for configure the database and tables."
    }
}
```

![Resource Manager-Tutorial: Schützen von Artefaktparametern](./media/resource-manager-tutorial-secure-artifacts/resource-manager-tutorial-secure-artifacts-parameters.png)

Aktualisieren Sie den Wert der folgenden beiden Elemente:

```json
"storageKey": "[parameters('_artifactsLocationSasToken')]",
"storageUri": "[uri(parameters('_artifactsLocation'), parameters('bacpacFileName'))]",
```

## <a name="deploy-the-template"></a>Bereitstellen der Vorlage

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Informationen zum Bereitstellungsverfahren finden Sie im Abschnitt [Bereitstellen der Vorlage](./resource-manager-tutorial-create-multiple-instances.md#deploy-the-template). Verwenden Sie stattdessen das folgende PowerShell-Bereitstellungsskript:

```azurepowershell
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$adminUsername = Read-Host -Prompt "Enter the virtual machine admin username"
$adminPassword = Read-Host -Prompt "Enter the admin password" -AsSecureString
$artifactsLocation = Read-Host -Prompt "Enter the artifacts location"
$artifactsLocationSasToken = Read-Host -Prompt "Enter the artifacts location SAS token" -AsSecureString
$bacpacFileName = Read-Host -Prompt "Enter the BACPAC file name"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -adminUser $adminUsername `
    -adminPassword $adminPassword `
    -_artifactsLocation $artifactsLocation `
    -_artifactsLocationSasToken $artifactsLocationSasToken `
    -bacpacFileName $bacpacFileName `
    -TemplateFile "$HOME/azuredeploy.json"
```

Verwenden Sie ein generiertes Kennwort. Siehe [Voraussetzungen](#prerequisites).
Die Werte von „_artifactsLocation“, „_artifactsLocationSasToken“ und „bacpacFileName“ finden Sie unter [Generieren eines SAS-Tokens](#generate-a-sas-token).

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

In diesem Tutorial haben Sie eine SQL Server-Instanz und eine SQL-Datenbank bereitgestellt und eine BACPAC-Datei mit einem SAS-Token importiert. Weitere Informationen zum Erstellen einer Azure Pipeline-Instanz zur kontinuierlichen Entwicklung und Bereitstellung von Resource Manager-Vorlagen finden Sie unter

> [!div class="nextstepaction"]
> [Continuous Integration mit Azure Pipelines](./resource-manager-tutorial-use-azure-pipelines.md)
