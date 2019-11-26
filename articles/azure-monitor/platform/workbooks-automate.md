---
title: Programmgesteuertes Verwalten von Azure Monitor-Arbeitsmappen mit Azure Resource Manager-Vorlagen | Microsoft-Dokumentation
description: Vereinfachen Sie die komplexe Berichterstellung mit vordefinierten und benutzerdefinierten parametrisierten Azure Monitor-Arbeitsmappen, die über Azure Resource Manager-Vorlagen bereitgestellt werden.
services: azure-monitor
author: mrbullwinkle
manager: carmonm
ms.service: azure-monitor
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 10/23/2019
ms.author: mbullwin
ms.openlocfilehash: d5e22093fa796a9fbd60dc2bc242f37a6cac7cf0
ms.sourcegitcommit: 0b1a4101d575e28af0f0d161852b57d82c9b2a7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73164496"
---
# <a name="programmatically-manage-workbooks"></a>Programmgesteuertes Verwalten von Arbeitsmappen

Ressourcenbesitzer können Ihre Arbeitsmappen über Resource Manager-Vorlagen programmgesteuert erstellen und verwalten. 

Dies kann in den folgenden Szenarien hilfreich sein:
* Bereitstellen von organisations- oder domänenspezifischen Analyseberichten zusammen mit Ressourcenbereitstellungen. Sie können beispielsweise organisationsspezifische Leistungs-und Fehlerarbeitsmappen für Ihre neuen Apps oder VMs bereitstellen.
* Bereitstellen von Standardberichten oder Standarddashboards mithilfe von Arbeitsmappen für vorhandene Ressourcen.

Die Arbeitsmappe wird in der gewünschten Untergruppe/Ressourcengruppe und mit dem Inhalt erstellt, der in den Resource Manager-Vorlagen angegeben ist.

## <a name="azure-resource-manager-template-for-deploying-workbooks"></a>Azure Resource Manager-Vorlage für das Bereitstellen von Arbeitsmappen
1. Öffnen Sie eine Arbeitsmappe, die Sie programmgesteuert bereitstellen möchten.
2. Versetzen Sie die Arbeitsmappe in den Bearbeitungsmodus, indem Sie auf das Symbolleistenelement _Bearbeiten_ klicken.
3. Öffnen Sie _Erweiterter Editor_, indem Sie auf der Symbolleiste auf die Schaltfläche _</>_ klicken.
4. Ändern Sie im Editor _Vorlagentyp_ in _Resource Manager-Vorlage_.
5. Die Resource Manager-Vorlage zum Erstellen wird im Editor angezeigt. Kopieren Sie den Inhalt, und übernehmen Sie ihn unverändert, oder führen Sie ihn mit einer größeren Vorlage zusammen, mit der ebenfalls die Zielressource bereitgestellt wird.

    ![Abbildung, die veranschaulicht, wie die Resource Manager-Vorlage aus der Arbeitsmappen-Benutzeroberfläche abgerufen wird](./media/workbooks-automate/programmatic-template.png)

## <a name="sample-azure-resource-manager-template"></a>Beispiel für eine Azure Resource Manager-Vorlage
Diese Vorlage zeigt, wie Sie eine einfache Arbeitsmappe bereitstellen, in der „Hallo Welt“ angezeigt wird.
```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "workbookDisplayName":  {             
            "type":"string",
            "defaultValue": "My Workbook",
            "metadata": {
                "description": "The friendly name for the workbook that is used in the Gallery or Saved List. Needs to be unique in the scope of the resource group and source" 
            }
        },
        "workbookType":  {             
            "type":"string",
            "defaultValue": "tsg",
            "metadata": {
                "description": "The gallery that the workbook will been shown under. Supported values include workbook, `tsg`, Azure Monitor, etc." 
            }
        },
        "workbookSourceId":  {             
            "type":"string",
            "defaultValue": "<insert-your-resource-id-here>",
            "metadata": {
                "description": "The id of resource instance to which the workbook will be associated" 
            }
        },
        "workbookId": {
            "type":"string",
            "defaultValue": "[newGuid()]",
            "metadata": {
                "description": "The unique guid for this workbook instance" 
            }
        }
    },    
    "resources": [
        {
            "name": "[parameters('workbookId')]",
            "type": "Microsoft.Insights/workbooks",
            "location": "[resourceGroup().location]",
            "kind": "shared",
            "apiVersion": "2018-06-17-preview",
            "dependsOn": [],
            "properties": {
                "displayName": "[parameters('workbookDisplayName')]",
                "serializedData": "{\"version\":\"Notebook/1.0\",\"items\":[{\"type\":1,\"content\":\"{\\\"json\\\":\\\"Hello World!\\\"}\",\"conditionalVisibility\":null}],\"isLocked\":false}",
                "version": "1.0",
                "sourceId": "[parameters('workbookSourceId')]",
                "category": "[parameters('workbookType')]"
            }
        }
    ],
    "outputs": {
        "workbookId": {
            "type": "string",
            "value": "[resourceId( 'Microsoft.Insights/workbooks', parameters('workbookId'))]"
        }
    }
}
```

### <a name="template-parameters"></a>Vorlagenparameter

| Parameter | Erklärung |
| :------------- |:-------------|
| `workbookDisplayName` | Der Anzeigename für die Arbeitsmappe, der im Katalog oder in der Liste gespeicherter Elemente verwendet wird. Muss im Bereich der Ressourcengruppe und der Quelle eindeutig sein |
| `workbookType` | Der Katalog, in dem die Arbeitsmappe angezeigt wird. Zu den unterstützten Werten zählen Arbeitsmappe, `tsg`, Azure Monitor usw. |
| `workbookSourceId` | Die ID der Ressourceninstanz, der die Arbeitsmappe zugeordnet wird. Die neue Arbeitsmappe wird im Zusammenhang mit dieser Ressourceninstanz angezeigt, z. B. im Inhaltsverzeichnis der Ressource unter _Arbeitsmappe_. Wenn die Arbeitsmappe im Arbeitsmappenkatalog in Azure Monitor angezeigt werden soll, verwenden Sie anstelle einer Ressourcen-ID die Zeichenfolge _Azure Monitor_. |
| `workbookId` | Die eindeutige GUID für diese Arbeitsmappeninstanz. Verwenden Sie _[newGuid()]_ , um automatisch eine neue GUID zu erstellen. |
| `kind` | Hiermit wird angegeben, ob die erstellte Arbeitsmappe freigegeben oder privat ist. Verwenden Sie den Wert _shared_ für freigegebene Arbeitsmappen und den Wert _user_ für private Arbeitsmappen. |
| `location` | Der Azure-Speicherort, an dem die Arbeitsmappe erstellt wird. Verwenden Sie _[resourceGroup().location]_ , um sie an demselben Speicherort wie die Ressourcengruppe zu erstellen |
| `serializedData` | Enthält den Inhalt oder die Nutzlast, der bzw. die in der Arbeitsmappe verwendet werden soll. Rufen Sie den Wert mithilfe der Resource Manager-Vorlage aus der Arbeitsmappen-Benutzeroberfläche ab |

### <a name="workbook-types"></a>Arbeitsmappentypen
Arbeitsmappentypen geben an, unter welchem Arbeitsmappenkatalogtyp die neue Arbeitsmappeninstanz angezeigt wird. Beispiele für Optionen:

| type | Position im Katalog |
| :------------- |:-------------|
| `workbook` | Der in den meisten Berichten verwendete Standardwert, einschließlich des Arbeitsmappenkatalogs von Application Insights, Azure Monitor usw.  |
| `tsg` | Der Katalog „Leitfäden zur Problembehandlung“ in Application Insights |
| `usage` | Der Katalog _Mehr_ unter _Nutzung_ in Application Insights |

### <a name="limitations"></a>Einschränkungen
Aus technischen Gründen können mit diesem Mechanismus keine Arbeitsmappeninstanzen im Katalog _Arbeitsmappen_ von Application Insights erstellt werden. Wir arbeiten derzeit an einer Lösung für diese Einschränkung. In der Zwischenzeit empfehlen wir, den Katalog „Leitfaden zur Problembehandlung“ (workbookType: `tsg`) zum Bereitstellen von Application Insights-bezogenen Arbeitsmappen zu verwenden.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Arbeitsmappen die neue [Azure Monitor für Storage-Erfahrung](../insights/storage-insights-overview.md) fördern.

