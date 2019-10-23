---
title: Erstellen von Diagnoseeinstellungen in Azure mithilfe einer Resource Manager-Vorlage | Microsoft-Dokumentation
description: Erstellen Sie Diagnoseeinstellungen mithilfe einer Resource Manager-Vorlage, um Protokolle der Azure-Plattform an Azure Monitor-Protokolle, Azure Storage oder Azure Event Hubs weiterzuleiten.
author: bwren
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 07/31/2019
ms.author: bwren
ms.subservice: ''
ms.openlocfilehash: 303682a9d7b3c3fe8ba6c8dd6fe2a44741bcafce
ms.sourcegitcommit: b4f201a633775fee96c7e13e176946f6e0e5dd85
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/18/2019
ms.locfileid: "72597282"
---
# <a name="create-diagnostic-setting-in-azure-using-a-resource-manager-template"></a>Erstellen von Diagnoseeinstellungen in Azure mithilfe einer Resource Manager-Vorlage
[Plattformprotokolle](platform-logs-overview.md) in Azure liefern detaillierte Diagnose- und Überwachungsinformationen für Azure-Ressourcen und die Azure-Plattform, von der sie abhängen. Dieser Artikel enthält Details zur Verwendung einer [Azure Resource Manager-Vorlage](../../azure-resource-manager/resource-group-authoring-templates.md) für das Konfigurieren von Diagnoseeinstellungen, um Plattformprotokolle an verschiedenen Zielen zu erfassen. Dadurch können Sie beim Erstellen einer Ressource automatisch mit dem Erfassen von Plattformprotokollen beginnen.


## <a name="resource-manager-template"></a>Resource Manager-Vorlage
Es gibt zwei Abschnitte der Resource Manager-Vorlage, die Sie bearbeiten müssen, um Diagnoseeinstellungen zu erstellen. Diese werden in den folgenden Abschnitten beschrieben.

### <a name="parameters"></a>Parameter
Fügen Sie je nach [Zielen](diagnostic-settings.md#destinations) für die Diagnoseeinstellung Parameter für den Speicherkontonamen, die Event Hub-Autorisierungsregel-ID und die Log Analytics-Arbeitsbereichs-ID zum Parameterblob hinzu.
   
```json
"settingName": {
  "type": "string",
  "metadata": {
    "description": "Name for the diagnostic setting resource. Eg. 'archiveToStorage' or 'forSecurityTeam'."
  }
},
"storageAccountName": {
  "type": "string",
  "metadata": {
    "description": "Name of the Storage Account in which platform logs should be saved."
  }
},
"eventHubAuthorizationRuleId": {
  "type": "string",
  "metadata": {
    "description": "Resource ID of the event hub authorization rule for the Event Hubs namespace in which the event hub should be created or streamed to."
  }
},
"eventHubName": {
  "type": "string",
  "metadata": {
    "description": "Optional. Name of the event hub within the namespace to which logs are streamed. Without this, an event hub is created for each log category."
  }
},
"workspaceId":{
  "type": "string",
  "metadata": {
    "description": "Azure Resource ID of the Log Analytics workspace to which logs will be sent."
  }
}
```

### <a name="resources"></a>Ressourcen
Fügen Sie im Ressourcenarray der Ressource, für die Sie Diagnoseeinstellungen erstellen möchten, eine Ressource vom Typ `[resource namespace]/providers/diagnosticSettings` hinzu. Der Eigenschaftenabschnitt verwendet das unter [Diagnoseeinstellungen – Erstellen oder Aktualisieren](https://docs.microsoft.com/rest/api/monitor/diagnosticsettings/createorupdate) beschriebene Format. Fügen Sie die `metrics`-Eigenschaft hinzu, um Ressourcenmetriken an denselben Ziele zu erfassen, falls die [Ressource Metriken unterstützt](metrics-supported.md).
   
```json
"resources": [
  {
    "type": "providers/diagnosticSettings",
    "name": "[concat('Microsoft.Insights/', parameters('settingName'))]",
    "dependsOn": [
      "[/*resource Id for which resource logs will be enabled>*/]"
    ],
    "apiVersion": "2017-05-01-preview",
    "properties": {
      "name": "[parameters('settingName')]",
      "storageAccountId": "[resourceId('Microsoft.Storage/storageAccounts', parameters('storageAccountName'))]",
      "eventHubAuthorizationRuleId": "[parameters('eventHubAuthorizationRuleId')]",
      "eventHubName": "[parameters('eventHubName')]",
      "workspaceId": "[parameters('workspaceId')]",
      "logs": [ 
        {
          "category": "/* log category name */",
          "enabled": true,
          "retentionPolicy": {
            "days": 0,
            "enabled": false
          }
        }
      ],
      "metrics": [
        {
          "category": "AllMetrics",
          "enabled": true,
          "retentionPolicy": {
            "enabled": false,
            "days": 0
          }
        }
      ]
    }
  }
]
```



## <a name="example"></a>Beispiel
Es folgt ein vollständiges Beispiel, bei dem eine Logik-App und dann eine Diagnoseeinstellung erstellt wird, die das Streamen von Ressourcenprotokollen an einen Event Hub und das Speichern in einem Speicherkonto ermöglicht.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "logicAppName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Logic App that will be created."
      }
    },
    "testUri": {
      "type": "string",
      "defaultValue": "https://azure.microsoft.com/status/feed/"
    },
    "settingName": {
      "type": "string",
      "metadata": {
        "description": "Name of the setting. Name for the diagnostic setting resource. Eg. 'archiveToStorage' or 'forSecurityTeam'."
      }
    },
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Storage Account in which resource logs should be saved."
      }
    },
    "eventHubAuthorizationRuleId": {
      "type": "string",
      "metadata": {
        "description": "Resource ID of the event hub authorization rule for the Event Hubs namespace in which the event hub should be created or streamed to."
      }
    },
    "eventHubName": {
      "type": "string",
      "metadata": {
        "description": "Optional. Name of the event hub within the namespace to which logs are streamed. Without this, an event hub is created for each log category."
      }
    },
    "workspaceId": {
      "type": "string",
      "metadata": {
        "description": "Log Analytics workspace ID for the Log Analytics workspace to which logs will be sent."
      }
    }
  },
  "variables": {},
  "resources": [
    {
      "type": "Microsoft.Logic/workflows",
      "name": "[parameters('logicAppName')]",
      "apiVersion": "2016-06-01",
      "location": "[resourceGroup().location]",
      "properties": {
        "definition": {
          "$schema": "https://schema.management.azure.com/schemas/2016-06-01/Microsoft.Logic.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "testURI": {
              "type": "string",
              "defaultValue": "[parameters('testUri')]"
            }
          },
          "triggers": {
            "recurrence": {
              "type": "recurrence",
              "recurrence": {
                "frequency": "Hour",
                "interval": 1
              }
            }
          },
          "actions": {
            "http": {
              "type": "Http",
              "inputs": {
                "method": "GET",
                "uri": "@parameters('testUri')"
              },
              "runAfter": {}
            }
          },
          "outputs": {}
        },
        "parameters": {}
      },
      "resources": [
        {
          "type": "providers/diagnosticSettings",
          "name": "[concat('Microsoft.Insights/', parameters('settingName'))]",
          "dependsOn": [
            "[resourceId('Microsoft.Logic/workflows', parameters('logicAppName'))]"
          ],
          "apiVersion": "2017-05-01-preview",
          "properties": {
            "name": "[parameters('settingName')]",
            "storageAccountId": "[resourceId('Microsoft.Storage/storageAccounts', parameters('storageAccountName'))]",
            "eventHubAuthorizationRuleId": "[parameters('eventHubAuthorizationRuleId')]",
            "eventHubName": "[parameters('eventHubName')]",
            "workspaceId": "[parameters('workspaceId')]",
            "logs": [
              {
                "category": "WorkflowRuntime",
                "enabled": true,
                "retentionPolicy": {
                  "days": 0,
                  "enabled": false
                }
              }
            ],
            "metrics": [
              {
                "timeGrain": "PT1M",
                "enabled": true,
                "retentionPolicy": {
                  "enabled": false,
                  "days": 0
                }
              }
            ]
          }
        }
      ],
      "dependsOn": []
    }
  ]
}

```


## <a name="next-steps"></a>Nächste Schritte
* Informieren Sie sich ausführlicher über [Plattformprotokolle in Azure](platform-logs-overview.md).
* Erfahren Sie mehr über [Diagnoseeinstellungen](diagnostic-settings.md).
