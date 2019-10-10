---
title: Verschieben eines Azure Storage-Kontos in eine andere Region | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie ein Azure Storage-Konto in eine andere Region verschieben können.
services: storage
author: normesta
ms.service: storage
ms.subservice: common
ms.topic: article
ms.date: 09/27/2019
ms.author: normesta
ms.reviewer: dineshm
ms.openlocfilehash: 76648428e6adcaed579b0e4f1896fdf83e11a8b6
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71348849"
---
# <a name="move-an-azure-storage-account-to-another-region"></a>Verschieben eines Azure Storage-Kontos in eine andere Region

Erstellen Sie eine Kopie Ihres Speicherkontos in einer anderen Region, um ein Speicherkonto zu verschieben. Verschieben Sie dann Ihre Daten in dieses Konto, indem Sie AzCopy oder ein anderes Tool Ihrer Wahl verwenden.

In diesem Artikel lernen Sie Folgendes:

> [!div class="checklist"]
> 
> * Exportieren einer Vorlage.
> * Ändern der Vorlage, indem Sie die Zielregion und den Namen des Speicherkontos hinzufügen.
> * Bereitstellen der Vorlage zur Erstellung des neuen Speicherkontos.
> * Konfigurieren des neuen Speicherkontos.
> * Verschieben der Daten in das neue Speicherkonto.
> * Löschen der Ressourcen in der Quellregion.

## <a name="prerequisites"></a>Voraussetzungen

- Stellen Sie sicher, dass die Dienste und Features, die von Ihrem Konto verwendet werden, in der Zielregion unterstützt werden.

- Stellen Sie für Previewfunktionen sicher, dass Ihr Abonnement für die Zielregion auf der Whitelist steht.

<a id="prepare" />

## <a name="prepare"></a>Vorbereiten

Exportieren Sie zunächst eine Resource Manager-Vorlage und ändern Sie sie anschließend. 

### <a name="export-a-template"></a>Exportieren einer Vorlage

Diese Vorlage enthält Einstellungen, die Ihr Speicherkonto beschreiben. 

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)

So exportieren Sie eine Vorlage mithilfe des Azure-Portals

1. Melden Sie sich beim [Azure-Portal](http://portal.azure.com) an.

2. Wählen Sie die Option **Alle Ressourcen** und dann Ihr Speicherkonto aus.

3. Wählen Sie **Einstellungen** > **Vorlage exportieren** aus.

4. Wählen Sie **Herunterladen** auf dem Blatt **Vorlage exportieren** aus.

5. Suchen Sie die aus dem Portal heruntergeladene ZIP-Datei, und entpacken Sie sie in einen Ordner Ihrer Wahl.

   Diese ZIP-Datei enthält die JSON-Dateien, in denen die Vorlage und die Skripts zum Bereitstellen der Vorlage enthalten sind.

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

So exportieren Sie eine Vorlage mithilfe von PowerShell

1. Melden Sie sich mit dem Befehl [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount?view=azps-2.5.0) bei Ihrem Azure-Abonnement an, und befolgen Sie die Anweisungen auf dem Bildschirm:

   ```azurepowershell-interactive
   Connect-AzAccount
   ```
2. Wenn Ihre Identität mehreren Abonnements zugeordnet ist, legen Sie das aktive Abonnement auf das Abonnement des Speicherkontos fest, das Sie verschieben möchten.

   ```azurepowershell-interactive
   $context = Get-AzSubscription -SubscriptionId <subscription-id>
   Set-AzContext $context
   ```

3. Exportieren Sie die Vorlage Ihres Quellspeicherkontos. Diese Befehle speichern eine JSON-Vorlage in Ihrem aktuellen Verzeichnis.

   ```azurepowershell-interactive
   $resource = Get-AzResource `
     -ResourceGroupName <resource-group-name> `
     -ResourceName <storage-account-name> `
     -ResourceType Microsoft.Storage/storageAccounts
   Export-AzResourceGroup `
     -ResourceGroupName <resource-group-name> `
     -Resource $resource.ResourceId
   ```

---

### <a name="modify-the-template"></a>Ändern der Vorlage 

Ändern Sie die Vorlage, indem Sie den Namen und die Region des Speicherkontos ändern.

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)

So stellen Sie die Vorlage mithilfe des Azure-Portals bereit

1. Klicken Sie im Azure-Portal auf **Ressource erstellen**.

2. Geben Sie in **Marketplace durchsuchen** den Begriff **Vorlagenbereitstellung** ein, und drücken Sie dann die **EINGABETASTE**.

3. Wählen Sie **Vorlagenbereitstellung** aus.

    ![Azure Resource Manager-Vorlagenbibliothek](./media/storage-account-move/azure-resource-manager-template-library.png)

4. Klicken Sie auf **Erstellen**.

5. Wählen Sie **Eigene Vorlage im Editor erstellen**.

6. Wählen Sie **Datei laden** aus, und befolgen Sie dann die Anweisungen zum Laden der Datei **template.json**, die Sie im vorherigen Abschnitt heruntergeladen haben.

7. Benennen Sie in der Datei **template.json** das Zielspeicherkonto, indem Sie den Standardwert des Speicherkontonamens festlegen. In diesem Beispiel wird der Standardwert des Speicherkontonamens auf `mytargetaccount` festgelegt.
    
    ```json
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageAccounts_mysourceaccount_name": {
            "defaultValue": "mytargetaccount",
            "type": "String"
        }
    },
 
8. Edit the **location** property in the **template.json** file to the target region. This example sets the target region to `centralus`.

    ```json
    "resources": [{
         "type": "Microsoft.Storage/storageAccounts",
         "apiVersion": "2019-04-01",
         "name": "[parameters('storageAccounts_mysourceaccount_name')]",
         "location": "centralus"
         }]          
    ```
    Informationen zum Abrufen von Regionsstandortcodes finden Sie unter [Azure-Standorte](https://azure.microsoft.com/global-infrastructure/locations/).  Der Code für eine Region ist der Regionsname ohne Leerzeichen, **USA, Mitte** = **centralus**.

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

So stellen Sie die Vorlage mithilfe von PowerShell bereit

1. Benennen Sie in der Datei **template.json** das Zielspeicherkonto, indem Sie den Standardwert des Speicherkontonamens festlegen. In diesem Beispiel wird der Standardwert des Speicherkontonamens auf `mytargetaccount` festgelegt.
    
    ```json
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageAccounts_mysourceaccount_name": {
            "defaultValue": "mytargetaccount",
            "type": "String"
        }
    },
    ``` 

2. Bearbeiten Sie die **location**-Eigenschaft in der Datei **template.json** in der Zielregion. Dieses Beispiel legt den Zielbereich auf `eastus` fest.

    ```json
    "resources": [{
         "type": "Microsoft.Storage/storageAccounts",
         "apiVersion": "2019-04-01",
         "name": "[parameters('storageAccounts_mysourceaccount_name')]",
         "location": "eastus"
         }]          
    ```

    Sie können Regionscodes erhalten, indem Sie den Befehl [Get-AzLocation](https://docs.microsoft.com/powershell/module/az.resources/get-azlocation?view=azps-1.8.0) ausführen.

    ```azurepowershell-interactive
    Get-AzLocation | format-table 
    ```
---

<a id="move" />

## <a name="move"></a>Move

Stellen Sie die Vorlage bereit, um ein neues Speicherkonto in der Zielregion zu erstellen. 

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)

1. Speichern Sie die Datei **template.json**.

2. Geben Sie die Eigenschaftswerte ein oder wählen Sie sie aus:

- **Abonnement**: Wählen Sie ein Azure-Abonnement aus.

- **Ressourcengruppe**: Wählen Sie **Neu erstellen** aus, und benennen Sie die Ressourcengruppe.

- **Standort**: Wählen Sie einen Azure-Standort aus.

3. Klicken Sie auf das Kontrollkästchen **Ich stimme den oben genannten Geschäftsbedingungen zu** und dann auf die Schaltfläche **Kauf auswählen**.

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

1. Rufen Sie die Abonnement-ID ab, für die Sie die öffentliche Ziel-IP-Adresse mit [Get-AzSubscription](https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription?view=azps-2.5.0) bereitstellen möchten:

   ```azurepowershell-interactive
   Get-AzSubscription
   ```

2. Verwenden Sie diese Befehle, um Ihre Vorlage bereitzustellen:

   ```azurepowershell-interactive
   $resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
   $location = Read-Host -Prompt "Enter the location (i.e. centralus)"

   New-AzResourceGroup -Name $resourceGroupName -Location "$location"
   New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri "<name of your local template file>"  
   ```
---

### <a name="configure-the-new-storage-account"></a>Konfigurieren des neuen Speicherkontos

Einige Features werden nicht in eine Vorlage exportiert, daher müssen Sie sie dem neuen Speicherkonto hinzufügen. 

In der folgenden Tabelle sind diese Features zusammen mit einer Anleitung zum Hinzufügen zu Ihrem neuen Speicherkonto aufgeführt.

| Feature    | Anleitungen    |
|--------|-----------|
| **Richtlinien für die Lebenszyklusverwaltung** | [Verwalten des Azure Blob Storage-Lebenszyklus](../blobs/storage-lifecycle-management-concepts.md) |
| **Statische Websites** | [Host a static website in Azure Storage](../blobs/storage-blob-static-website-how-to.md) (Hosten einer statischen Website in Azure Storage) |
| **Ereignisabonnements** | [Reacting to Blob storage events (preview)](../blobs/storage-blob-event-overview.md) (Reagieren auf Blob Storage-Ereignisse (Vorschauversion)) |
| **Warnungen** | [Erstellen, Anzeigen und Verwalten von Aktivitätsprotokollwarnungen mit Azure Monitor](../../azure-monitor/platform/alerts-activity-log.md) |
| **Content Delivery Network (CDN)** | [Verwenden von Azure CDN zum Zugreifen auf Blobs mit benutzerdefinierten Domänen über HTTPS](../blobs/storage-https-custom-domain-cdn.md) |

> [!NOTE] 
> Wenn Sie ein CDN für das Quellspeicherkonto einrichten, ändern Sie einfach den Ursprung Ihres bestehenden CDNs in den primären Blobdienstendpunkt (oder den primären statischen Webseitenendpunkt) Ihres neuen Kontos. 

### <a name="move-data-to-the-new-storage-account"></a>Verschieben der Daten in das neue Speicherkonto

Hier sind einige Möglichkeiten, wie Sie Ihre Daten verschieben können.

:heavy_check_mark: **Azure Storage-Explorer**

  Es ist einfach zu verwenden und eignet sich für kleine Datasets. Sie können Container und Dateifreigaben kopieren und dann in das Zielkonto einfügen.

  Weitere Informationen finden Sie unter [Azure Storage-Explorer](https://azure.microsoft.com/features/storage-explorer/).

:heavy_check_mark: **AzCopy**

  Dies ist der bevorzugte Ansatz. Er ist für die Leistung optimiert.  Ein Grund für die höhere Leistung besteht darin, dass Daten direkt zwischen Speicherservern kopiert werden, sodass AzCopy nicht die Netzwerkbandbreite Ihres Computers nutzt. Verwenden Sie AzCopy über die Befehlszeile oder als Teil eines benutzerdefinierten Skripts.

  Informationen finden Sie unter [Erste Schritte mit AzCopy](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

:heavy_check_mark: **Azure Data Factory** 

  Verwenden Sie dieses Tool nur, wenn Sie Funktionen benötigen, die in der aktuellen Version von AzCopy nicht unterstützt werden. In der aktuellen Version von AzCopy können Sie z. B. keine Blobs zwischen Konten kopieren, die einen hierarchischen Namespace aufweisen. AzCopy bewahrt zudem keine Zugriffssteuerungslisten oder Zeitstempel für Dateien auf (Beispiel: Zeitstempel erstellen und ändern). 

  Weitere Informationen finden Sie über die folgenden Links:
  - [Kopieren von Daten nach oder aus Azure Blob Storage mit Azure Data Factory](https://docs.microsoft.com/azure/data-factory/connector-azure-blob-storage)
  - [Kopieren von Daten nach und aus Azure Data Lake Storage Gen2 mithilfe von Azure Data Factory](https://docs.microsoft.com/azure/data-factory/connector-azure-data-lake-storage)
  - [Kopieren von Daten aus bzw. in Azure File Storage mithilfe von Azure Data Factory](https://docs.microsoft.com/azure/data-factory/connector-azure-file-storage)
  - [Kopieren von Daten nach und aus Azure Table Storage mit Azure Data Factory](https://docs.microsoft.com/azure/data-factory/connector-azure-table-storage)

---

## <a name="discard-or-clean-up"></a>Verwerfen oder Bereinigen

Wenn Sie nach der Bereitstellung von vorne beginnen möchten, können Sie das Zielspeicherkonto löschen und die in den Abschnitten [Vorbereiten](#prepare) und [Verschieben](#move) dieses Artikels beschriebenen Schritte wiederholen.

Um die Änderungen zu übernehmen und das Verschieben eines Speicherkonto abzuschließen, löschen Sie das Quellspeicherkonto.

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)

So entfernen Sie ein Speicherkonto über das Azure-Portal

1. Erweitern Sie im Azure-Portal das Menü auf der linken Seite, um das Menü mit den Diensten zu öffnen, und klicken Sie auf **Speicherkonten**, um die Liste mit Ihren Speicherkonten anzuzeigen.

2. Suchen Sie das zu löschende Zielspeicherkonto, und klicken Sie mit der rechten Maustaste rechts neben der Liste auf die Schaltfläche **Mehr** ( **...** ).

3. Wählen Sie **Löschen** aus, und bestätigen Sie den Vorgang.

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

Verwenden Sie den Befehl [Remove-AzStorageAccount](/powershell/module/az.resources/remove-azstorageaccount), um die Ressourcengruppe und die zugeordneten Ressourcen (einschließlich des neuen Speicherkontos) zu entfernen:

```powershell
Remove-AzStorageAccount -ResourceGroupName  $resourceGroup -AccountName $storageAccount
```
---

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie ein Azure-Speicherkonto aus einer Region in eine andere verschoben und die Quellressourcen bereinigt.  Weitere Informationen zum Verschieben von Ressourcen zwischen Regionen und zur Notfallwiederherstellung in Azure finden Sie unter:


- [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources)
- [Verschieben virtueller Azure-Computer in eine andere Region](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-migrate)
