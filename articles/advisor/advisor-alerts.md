---
title: Erstellen von Azure Advisor-Warnungen für neue Empfehlungen | Microsoft-Dokumentation
description: Erstellen von Azure Advisor-Warnungen für neue Empfehlungen
services: advisor
author: sagupt
ms.service: advisor
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/09/2019
ms.author: sagupt
ms.openlocfilehash: a67034752b4c43533a5735b857186ee83934717a
ms.sourcegitcommit: f3f4ec75b74124c2b4e827c29b49ae6b94adbbb7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2019
ms.locfileid: "70935499"
---
# <a name="create-azure-advisor-alerts-on-new-recommendations"></a>Erstellen von Azure Advisor-Warnungen für neue Empfehlungen 

In diesem Artikel erfahren Sie, wie Sie eine Warnung für neue Empfehlungen von Azure Advisor über das Azure-Portal und mithilfe von Azure Resource Manager Vorlagen einrichten. 

Sobald Azure Advisor eine neue Empfehlung für eine Ihrer Ressourcen erkennt, wird ein Ereignis im [Azure-Aktivitätsprotokoll](https://docs.microsoft.com/azure/azure-monitor/platform/activity-logs-overview) gespeichert. Sie können Warnungen für diese Ereignisse von Azure Advisor mithilfe einer Benutzeroberfläche für die Erstellung empfehlungsspezifischer Warnungen einrichten. Sie können ein Abonnement und optional eine Ressourcengruppe auswählen, um die Ressourcen anzugeben, für die Sie Warnungen erhalten möchten. 

Sie können auch die Arten von Empfehlungen mithilfe der folgenden Eigenschaften festlegen:

* Kategorie
* Auswirkungsstufe
* Empfehlungstyp

Sie können auch die Aktion konfigurieren, die bei Auslösen einer Warnung durch folgende Ereignisse ausgeführt wird:  

* Auswählen einer vorhandenen Aktionsgruppe
* Erstellen einer neuen Aktionsgruppe

Weitere Informationen zu Aktionsgruppen finden Sie unter [Erstellen und Verwalten von Aktionsgruppen](../azure-monitor/platform/action-groups.md.

> [!NOTE] 
> Advisor-Warnungen stehen derzeit nur für Hochverfügbarkeits-, Leistungs- und Kostenempfehlungen zur Verfügung. Sicherheitsempfehlungen werden nicht unterstützt. 

## <a name="in-the-azure-portal"></a>Im Azure-Portal
1. Wählen Sie im **Portal** die Option für den **Azure Advisor** aus.

    ![Azure Advisor im Portal](./media/advisor-alerts/create1.png)

2. Wählen Sie im linken Menü im Abschnitt **Überwachung** die Option **Warnungen** aus. 

    ![Warnungen in Advisor](./media/advisor-alerts/create2.png)

3. Wählen Sie **Neue Advisor-Warnung** aus.

    ![Neue Advisor-Warnung](./media/advisor-alerts/create3.png)

4. Wählen Sie im Abschnitt **Bereich** das Abonnement und optional die Ressourcengruppe aus, für die Sie Warnungen erhalten möchten. 

    ![Advisor-Warnung – Bereich](./media/advisor-alerts/create4.png)

5. Wählen Sie im Abschnitt **Bedingung** die Methode aus, die Sie zum Konfigurieren der Warnung verwenden möchten. Wenn Sie eine Warnung für alle Empfehlungen einer bestimmten Kategorie und/oder einer bestimmten Auswirkungsstufe erhalten möchten, wählen Sie **Kategorie und Auswirkungsstufe** aus. Wenn Sie für alle Empfehlungen eines bestimmten Typs eine Warnung erhalten möchten, wählen Sie **Empfehlungstyp** aus.

    ![Azure Advisor-Warnung – Bedingung](./media/advisor-alerts/create5.png)

6. Je nach ausgewählter Option für das Konfigurieren der Warnung können Sie die Kriterien festlegen. Wenn Sie alle Empfehlungen wünschen, lassen Sie einfach die restlichen Felder leer. 

    ![Advisor-Warnung – Aktionsgruppe](./media/advisor-alerts/create6.png)

7. Wählen Sie im Abschnitt **Aktionsgruppen** die Option **Vorhandene hinzufügen** aus, um eine bereits erstellte Aktionsgruppe zu verwenden, oder wählen Sie **Neu erstellen** aus, um eine neue [Aktionsgruppe](https://docs.microsoft.com/azure/azure-monitor/platform/action-groups) einzurichten. 

    ![Advisor-Warnung – Vorhandene hinzufügen](./media/advisor-alerts/create7.png)

8. Geben Sie im Abschnitt mit den Warnungsdetails einen Namen und eine kurze Beschreibung für die Warnung ein. Wenn die Warnung aktiviert werden soll, belassen Sie **Regel beim Erstellen aktivieren** auf **Ja**. Wählen Sie dann die Ressourcengruppe aus, in der die Warnung gespeichert werden soll. Dies wirkt sich nicht auf den Zielbereich der Empfehlung aus. 

    ![Azure Advisor-Banner](./media/advisor-alerts/create8.png)


## <a name="with-an-azure-resource-manager-template"></a>Mit einer Azure Resource Manager-Vorlage

Mit dieser Resource Manager-Vorlage wird eine Empfehlungswarnung und eine neue Aktionsgruppe erstellt.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "actionGroups_name": {
      "defaultValue": "advisorAlert",
      "type": "string"
    },
    "activityLogAlerts_name": {
      "defaultValue": "AdvisorAlertsTest2",
      "type": "string"
    },
    "emailAddress": {
      "defaultValue": "<email address>",
      "type": "string"
    }
  },
  "variables": {
    "alertScope": "[concat('/','subscriptions','/',subscription().subscriptionId)]"
  },
  "resources": [
    {
      "comments": "Action Group",
      "type": "microsoft.insights/actionGroups",
      "name": "[parameters('actionGroups_name')]",
      "apiVersion": "2017-04-01",
      "location": "Global",
      "tags": {},
      "scale": null,
      "properties": {
        "groupShortName": "[parameters('actionGroups_name')]",
        "enabled": true,
        "emailReceivers": [
          {
            "name": "[parameters('actionGroups_name')]",
            "emailAddress": "[parameters('emailAddress')]"
          }
        ],
        "smsReceivers": [],
        "webhookReceivers": []
      },
      "dependsOn": []
    },
    {
      "comments": "Azure Advisor Activity Log Alert",
      "type": "microsoft.insights/activityLogAlerts",
      "name": "[parameters('activityLogAlerts_name')]",
      "apiVersion": "2017-04-01",
      "location": "Global",
      "tags": {},
      "scale": null,
      "properties": {
        "scopes": [
          "[variables('alertScope')]"
        ],
        "condition": {
          "allOf": [
            {
              "field": "category",
              "equals": "Recommendation"
            },
            {
              "field": "properties.recommendationCategory",
              "equals": "Cost"
            },
            {
              "field": "properties.recommendationImpact",
              "equals": "Medium"
            },
            {
              "field": "operationName",
              "equals": "Microsoft.Advisor/recommendations/available/action"
            }
          ]
        },
        "actions": {
          "actionGroups": [
            {
              "actionGroupId": "[resourceId('microsoft.insights/actionGroups', parameters('actionGroups_name'))]",
              "webhookProperties": {}
            }
          ]
        },
        "enabled": true,
        "description": ""
      },
      "dependsOn": [
        "[resourceId('microsoft.insights/actionGroups', parameters('actionGroups_name'))]"
      ]
    }
  ]
}
  ```

## <a name="configure-recommendation-alerts-to-use-a-webhook"></a>Konfigurieren von Empfehlungswarnungen für die Verwendung eines Webhooks
In diesem Abschnitt wird beschrieben, wie Sie Azure Advisor-Warnungen zum Senden von Empfehlungsdaten über Webhooks an Ihr vorhandenes System konfigurieren. 

Sie können Warnungen so einrichten, dass Sie benachrichtigt werden, wenn für eine Ihrer Ressourcen eine neue Advisor-Empfehlung vorliegt. Bei diesen Warnungen können Sie per E-Mail oder SMS benachrichtigen werden. Die Warnungen können aber auch über einen Webhook in Ihre vorhandenen Systeme integriert werden. 


### <a name="using-the-advisor-recommendation-alert-payload"></a>Verwenden der Nutzlast für Advisor-Empfehlungswarnungen
Wenn Sie Advisor-Warnungen mithilfe eines Webhooks in Ihre eigenen Systeme integrieren möchten, müssen Sie die JSON-Nutzlast analysieren, die von der Benachrichtigung gesendet wird. 

Wenn Sie die Aktionsgruppe für diese Warnung einrichten, wählen Sie aus, ob Sie das allgemeine Warnungsschema verwenden möchten. Bei Auswahl des allgemeinen Warnungsschemas sieht Ihre Nutzlast wie folgt aus: 

```json
{  
   "schemaId":"azureMonitorCommonAlertSchema",
   "data":{  
      "essentials":{  
         "alertId":"/subscriptions/<subid>/providers/Microsoft.AlertsManagement/alerts/<alerted>",
         "alertRule":"Webhhook-test",
         "severity":"Sev4",
         "signalType":"Activity Log",
         "monitorCondition":"Fired",
         "monitoringService":"Activity Log - Recommendation",
         "alertTargetIDs":[  
            "/subscriptions/<subid>/resourcegroups/<resource group name>/providers/microsoft.dbformariadb/servers/<resource name>"
         ],
         "originAlertId":"001d8b40-5d41-4310-afd7-d65c9d4428ed",
         "firedDateTime":"2019-07-17T23:00:57.3858656Z",
         "description":"A new recommendation is available.",
         "essentialsVersion":"1.0",
         "alertContextVersion":"1.0"
      },
      "alertContext":{  
         "channels":"Operation",
         "claims":"{\"http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress\":\"Microsoft.Advisor\"}",
         "caller":"Microsoft.Advisor",
         "correlationId":"8554b847-2a72-48ef-9776-600aca3c3aab",
         "eventSource":"Recommendation",
         "eventTimestamp":"2019-07-17T22:28:54.1566942+00:00",
         "httpRequest":"{\"clientIpAddress\":\"0.0.0.0\"}",
         "eventDataId":"001d8b40-5d41-4310-afd7-d65c9d4428ed",
         "level":"Informational",
         "operationName":"Microsoft.Advisor/recommendations/available/action",
         "properties":{  
            "recommendationSchemaVersion":"1.0",
            "recommendationCategory":"Performance",
            "recommendationImpact":"Medium",
            "recommendationName":"Increase the MariaDB server vCores",
            "recommendationResourceLink":"https://portal.azure.com/#blade/Microsoft_Azure_Expert/RecommendationListBlade/source/ActivityLog/recommendationTypeId/a5f888e3-8cf4-4491-b2ba-b120e14eb7ce/resourceId/%2Fsubscriptions%<subscription id>%2FresourceGroups%2<resource group name>%2Fproviders%2FMicrosoft.DBforMariaDB%2Fservers%2F<resource name>",
            "recommendationType":"a5f888e3-8cf4-4491-b2ba-b120e14eb7ce"
         },
         "status":"Active",
         "subStatus":"",
         "submissionTimestamp":"2019-07-17T22:28:54.1566942+00:00"
      }
   }
}
  ```

Wenn Sie das allgemeine Schema nicht verwenden, sieht Ihre Nutzlast folgendermaßen aus: 

```json
{  
   "schemaId":"Microsoft.Insights/activityLogs",
   "data":{  
      "status":"Activated",
      "context":{  
         "activityLog":{  
            "channels":"Operation",
            "claims":"{\"http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress\":\"Microsoft.Advisor\"}",
            "caller":"Microsoft.Advisor",
            "correlationId":"3ea7320f-c002-4062-adb8-96d3bd92a5f4",
            "description":"A new recommendation is available.",
            "eventSource":"Recommendation",
            "eventTimestamp":"2019-07-17T20:36:39.3966926+00:00",
            "httpRequest":"{\"clientIpAddress\":\"0.0.0.0\"}",
            "eventDataId":"a12b8e59-0b1d-4003-bfdc-3d8152922e59",
            "level":"Informational",
            "operationName":"Microsoft.Advisor/recommendations/available/action",
            "properties":{  
               "recommendationSchemaVersion":"1.0",
               "recommendationCategory":"Performance",
               "recommendationImpact":"Medium",
               "recommendationName":"Increase the MariaDB server vCores",
               "recommendationResourceLink":"https://portal.azure.com/#blade/Microsoft_Azure_Expert/RecommendationListBlade/source/ActivityLog/recommendationTypeId/a5f888e3-8cf4-4491-b2ba-b120e14eb7ce/resourceId/%2Fsubscriptions%2F<subscription id>%2FresourceGroups%2F<resource group name>%2Fproviders%2FMicrosoft.DBforMariaDB%2Fservers%2F<resource name>",
               "recommendationType":"a5f888e3-8cf4-4491-b2ba-b120e14eb7ce"
            },
            "resourceId":"/subscriptions/<subscription id>/resourcegroups/<resource group name>/providers/microsoft.dbformariadb/servers/<resource name>",
            "resourceGroupName":"<resource group name>",
            "resourceProviderName":"MICROSOFT.DBFORMARIADB",
            "status":"Active",
            "subStatus":"",
            "subscriptionId":"<subscription id>",
            "submissionTimestamp":"2019-07-17T20:36:39.3966926+00:00",
            "resourceType":"MICROSOFT.DBFORMARIADB/SERVERS"
         }
      },
      "properties":{  
 
      }
   }
}
```

In beiden Schemas können Sie Advisor-Empfehlungsereignisse ermitteln, indem Sie nach **eventSource** ist `Recommendation` und **operationName** ist `Microsoft.Advisor/recommendations/available/action` suchen.

Sie können auch einige der anderen wichtigen Felder verwenden. Dies sind: 

* *alertTargetIDs* (im allgemeinen Schema) oder *resourceId* (älteres Schema)
* *recommendationType*
* *recommendationName*
* *recommendationCategory*
* *recommendationImpact*
* *recommendationResourceLink*


## <a name="manage-your-alerts"></a>Verwalten Ihrer Warnungen 

In Azure Advisor können Sie Empfehlungswarnungen bearbeiten, löschen oder deaktivieren und aktivieren. 

1. Wählen Sie im **Portal** die Option für den **Azure Advisor** aus.

    ![Azure Advisor-Banner](./media/advisor-alerts/create1.png)

2. Wählen Sie im linken Menü im Abschnitt **Überwachung** die Option **Warnungen** aus.

    ![Azure Advisor-Banner](./media/advisor-alerts/create2.png)

3. Wenn Sie eine Warnung bearbeiten möchten, klicken Sie auf den Warnungsnamen, um die Warnung zu öffnen, und bearbeiten Sie die entsprechenden Felder.

4. Wenn Sie eine Warnung löschen, aktivieren oder deaktivieren möchten, klicken Sie auf die Auslassungszeichen am Ende der Zeile, und wählen Sie dann die Aktion aus, die Sie durchführen möchten.
 

