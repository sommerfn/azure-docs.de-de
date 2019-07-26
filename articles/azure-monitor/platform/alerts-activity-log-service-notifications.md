---
title: Empfangen von Aktivitätsprotokollwarnungen zu Azure-Dienstbenachrichtigungen
description: Lassen Sie sich per SMS, E-Mail oder Webhook benachrichtigen, wenn Ereignisse beim Azure-Dienst eintreten.
author: shawntabrizi
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 06/09/2018
ms.author: shtabriz
ms.openlocfilehash: 3157cef6952f01d689d3cb4a30c9d7c843095809
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "67071695"
---
# <a name="create-activity-log-alerts-on-service-notifications"></a>Erstellen von Aktivitätsprotokollwarnungen zu Dienstbenachrichtigungen
## <a name="overview"></a>Übersicht
Dieser Artikel zeigt, wie Sie Aktivitätsprotokollwarnungen für Dienstintegritätsbenachrichtigungen im Azure-Portal einrichten.  

Sie können eine Warnung erhalten, wenn Azure Benachrichtigungen zur Dienstintegrität an Ihr Azure-Abonnement sendet. Sie können die Warnung konfigurieren auf der Grundlage von:

- Der Klasse der Dienstintegritätsbenachrichtigung (Dientsprobleme, Geplante Wartung, Integritätsempfehlungen).
- Dem betroffenen Abonnement.
- Den betroffenen Diensten.
- Den betroffenen Regionen.

> [!NOTE]
> Dienstintegritätsbenachrichtigungen senden keine Warnung bezüglich Ereignissen der Ressourcenintegrität.

Sie können auch konfigurieren, an wen die Warnung gesendet werden soll:

- Wählen Sie eine vorhandene Aktionsgruppe aus.
- Erstellen Sie eine neue Aktionsgruppe (die für zukünftige Warnungen verwendet werden kann).

Weitere Informationen zu Aktionsgruppen finden Sie unter [Erstellen und Verwalten von Aktionsgruppen im Azure-Portal](../../azure-monitor/platform/action-groups.md).

Informationen zum Konfigurieren von Dienstintegritätsbenachrichtigungs-Warnungen mithilfe von Azure Resource Manager-Vorlagen siehe [Resource Manager-Vorlagen](alerts-activity-log.md).

### <a name="watch-a-video-on-setting-up-your-first-azure-service-health-alert"></a>Sehen Sie sich ein Video über das Einrichten der ersten Azure Service Health-Warnung an!

>[!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE2OaXt]

## <a name="alert-and-new-action-group-using-azure-portal"></a>Warnung und neue Aktionsgruppe mit dem Azure-Portal
1. Wählen Sie im [Portal](https://portal.azure.com) die Option **Dienstintegrität** aus.

    ![Der Dienst „Dienstintegrität“](media/alerts-activity-log-service-notifications/home-servicehealth.png)

1. Wählen Sie im Abschnitt **Warnungen** die Option **Integritätswarnungen** aus.

    ![Die Registerkarte „Integritätswarnungen“](media/alerts-activity-log-service-notifications/alerts-blades-sh.png)

1. Wählen Sie **Dienstintegritätswarnung erstellen** aus, und füllen Sie die Felder aus.

    ![Der Befehl „Dienstintegritätswarnung erstellen“](media/alerts-activity-log-service-notifications/service-health-alert.png)

1. Wählen Sie das **Abonnement**, die **Dienste** und die **Regionen** für Benachrichtigungen aus.

    ![Das Dialogfeld „Aktivitätsprotokollwarnung hinzufügen“](media/alerts-activity-log-service-notifications/activity-log-alert-new-ux.png)

> [!NOTE]
> Dieses Abonnement wird verwendet, um die Aktivitätsprotokollwarnung zu speichern. Die Warnungsressource wird für dieses Abonnement bereitgestellt und überwacht hierfür Ereignisse im Aktivitätsprotokoll.

1. Wählen Sie die **Ereignistypen** aus, bei denen Sie benachrichtigt werden möchten: *Dienstproblem*, *Geplante Wartung* und *Integritätsempfehlungen* 

1. Definieren Sie die Warnungsdetails durch Eingeben eines **Namens für die Warnungsregel** und einer **Beschreibung**.

1. Wählen Sie die **Ressourcengruppe** aus, in der die Warnung gespeichert werden soll.

1. Erstellen Sie eine neue Aktionsgruppe, indem Sie **Neue Aktionsgruppe** auswählen. Geben Sie jeweils einen Namen in die Felder **Aktionsgruppenname** und **Kurzname** ein. Auf den kurzen Namen wird in den Benachrichtigungen verwiesen, die gesendet werden, wenn diese Warnung ausgelöst wird.

    ![Erstellen einer neuen Aktionsgruppe](media/alerts-activity-log-service-notifications/action-group-creation.png)

1. Definieren Sie eine Liste von Empfängern, indem Sie folgende Daten der Empfänger angeben:

    a. **Name**: Geben Sie Name, Alias oder Bezeichner des Empfängers ein.

    b. **Aktionstyp**: Wählen Sie „SMS“, „E-Mail“, „Webhook“ „Azure-App“ usw. aus.

    c. **Details**: Geben Sie basierend auf dem ausgewählten Aktionstyp eine Telefonnummer, eine E-Mail-Adresse, einen Webhook-URI usw. ein.

1. Wählen Sie **OK** aus, um die Aktionsgruppe zu erstellen, und dann **Warnungsregel erstellen**, um die Warnung abzuschließen.

Innerhalb weniger Minuten wird die Warnung aktiv und entsprechend den Bedingungen ausgelöst, die Sie während der Erstellung angegeben haben.

Erfahren Sie, wie Sie [Webhookbenachrichtigungen für vorhandene Problemverwaltungssysteme konfigurieren](../../service-health/service-health-alert-webhook-guide.md). Informationen über das Webhookschema für Aktivitätsprotokollwarnungen finden Sie unter [Webhook für Azure-Aktivitätsprotokollwarnungen](../../azure-monitor/platform/activity-log-alerts-webhook.md).

>[!NOTE]
>Die in den folgenden Schritten definierte Aktionsgruppe kann als vorhandene Aktionsgruppe für alle zukünftigen Warnungsdefinitionen wiederverwendet werden.
>
>

## <a name="alert-with-existing-action-group-using-azure-portal"></a>Warnung mit vorhandener Aktionsgruppe mit dem Azure-Portal

1. Führen Sie die Schritte 1 bis 6 im vorherigen Abschnitt aus, um Ihre Dienstintegritätsbenachrichtigung zu erstellen. 

1. Klicken Sie unter **Aktionsgruppe definieren** auf die Schaltfläche **Aktionsgruppe auswählen**. Wählen Sie die entsprechende Aktionsgruppe aus.

1. Wählen Sie **Hinzufügen** aus, um die Aktionsgruppe hinzuzufügen, und dann **Warnungsregel erstellen**, um die Warnung abzuschließen.

Innerhalb weniger Minuten wird die Warnung aktiv und entsprechend den Bedingungen ausgelöst, die Sie während der Erstellung angegeben haben.

## <a name="alert-and-new-action-group-using-the-azure-resource-manager-templates"></a>Warnung und neue Aktionsgruppe mit Azure Resource Manager-Vorlagen

Nachfolgend sehen Sie ein Beispiel, das eine Aktionsgruppe mit einem E-Mail-Ziel erstellt und alle Dienstintegritätsbenachrichtigungen für das Zielabonnement aktiviert.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "actionGroups_name": {
            "defaultValue": "SubHealth",
            "type": "String"
        },
        "activityLogAlerts_name": {
            "defaultValue": "ServiceHealthActivityLogAlert",
            "type": "String"
        },
        "emailAddress":{
            "type":"string"
        }
    },
    "variables": {
        "alertScope":"[concat('/','subscriptions','/',subscription().subscriptionId)]"
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
            "comments": "Service Health Activity Log Alert",
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
                            "equals": "ServiceHealth"
                        },
                        {
                            "field": "properties.incidentType",
                            "equals": "Incident"
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

## <a name="manage-your-alerts"></a>Verwalten Ihrer Warnungen

Nachdem Sie eine Warnung erstellt haben, wird sie unter **Überwachen** im Abschnitt **Warnungen** angezeigt. Wählen Sie die Warnung, die Sie verwalten möchten, um Folgendes zu tun:

* Sie bearbeiten.
* Sie löschen.
* Deaktivieren oder aktivieren Sie sie, wenn Sie den Empfang von Benachrichtigungen zu dieser Warnung vorübergehend beenden oder fortsetzen möchten.

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie mehr über [bewährte Methoden zum Einrichten von Azure Service Health-Warnungen](https://www.microsoft.com/en-us/videoplayer/embed/RE2OtUa).
- Erfahren Sie, wie Sie [mobile Pushbenachrichtigungen für Azure Service Health einrichten](https://www.microsoft.com/en-us/videoplayer/embed/RE2OtUw).
- Erfahren Sie, wie Sie [Webhookbenachrichtigungen für vorhandene Problemverwaltungssysteme konfigurieren](../../service-health/service-health-alert-webhook-guide.md).
- Weitere Informationen zu [Dienstintegritätsbenachrichtigungen](../../azure-monitor/platform/service-notifications.md).
- Erfahren Sie mehr über [Ratenlimits für SMS, E-Mail-Nachrichten und Webhooks](../../azure-monitor/platform/alerts-rate-limiting.md).
- Weitere Informationen zum [Webhookschema für Aktivitätsprotokollwarnungen](../../azure-monitor/platform/activity-log-alerts-webhook.md).
- Verschaffen Sie sich eine [Übersicht über Aktivitätsprotokollwarnungen](../../azure-monitor/platform/alerts-overview.md), und erfahren Sie, wie Sie Warnungen empfangen können. 
- Weitere Informationen zu [Aktionsgruppen](../../azure-monitor/platform/action-groups.md).
