---
title: Abrufen von Ressourcenänderungen
description: Erfahren Sie, wie Sie feststellen können, wann eine Ressource geändert wurde, und wie Sie eine Liste der geänderten Eigenschaften abrufen.
services: resource-graph
author: DCtheGeek
ms.author: dacoulte
ms.date: 10/09/2019
ms.topic: conceptual
ms.service: resource-graph
ms.openlocfilehash: 13e2a848f9d178fc6554062c324c951102e1343b
ms.sourcegitcommit: 824e3d971490b0272e06f2b8b3fe98bbf7bfcb7f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2019
ms.locfileid: "72244116"
---
# <a name="get-resource-changes"></a>Abrufen von Ressourcenänderungen

Ressourcen werden während der alltäglichen Verwendung, Neukonfiguration und sogar der erneuten Bereitstellung geändert.
Änderungen können durch eine Person oder einen automatisierten Prozess herbeigeführt werden. Die meisten, jedoch nicht alle Änderungen sind beabsichtigt. Azure Resource Graph ermöglicht innerhalb der letzten 14 Tage des Änderungsverlaufs Folgendes:

- Ermitteln, wann Änderungen an einer Azure Resource Manager-Eigenschaft festgestellt wurden
- Anzeigen der Änderungsdetails für Eigenschaften für jede Ressourcenänderung
- Anzeigen eines vollständigen Vergleichs der Ressource vor und nach der erkannten Änderung

Die Erkennung und die Details von Änderungen sind in folgenden Beispielszenarien nützlich:

- Bei der Verwaltung von Incidents zum Suchen _potenziell_ ähnlicher Änderungen. Sie können Änderungsereignisse in einem bestimmten Zeitfenster abfragen und die Änderungsdetails auswerten.
- Wenn eine Datenbank für die Konfigurationsverwaltung (Configuration Management Database, CMDB) auf dem neuesten Stand gehalten werden soll. Anstatt alle Ressourcen und die zugehörigen vollständigen Eigenschaftensätze in einem festgelegten Intervall zu aktualisieren, können Sie nur die Änderungen abrufen.
- Ermitteln, welche anderen Eigenschaften sich unter Umständen geändert haben, wenn der Konformitätszustand einer Ressource geändert wurde. Die Auswertung dieser zusätzlichen Eigenschaften kann Einblicke in andere Eigenschaften bieten, die möglicherweise über eine Azure Policy-Definition verwaltet werden müssen.

In diesem Artikel wird erläutert, wie diese Informationen über das Resource Graph-SDK erfasst werden. Beachten Sie für das Anzeigen dieser Informationen im Azure-Portal die Beschreibung unter [Änderungsverlauf](../../policy/how-to/determine-non-compliance.md#change-history-preview) von Azure Policy bzw. [Änderungsverlauf](../../../azure-monitor/platform/activity-log-view.md#azure-portal) des Azure-Aktivitätsprotokolls.

> [!NOTE]
> Die Änderungsdetails in Resource Graph beziehen sich auf Resource Manager-Eigenschaften. Informationen zur Nachverfolgung von Änderungen in einem virtuellen Computer finden Sie unter [Änderungsnachverfolgung](../../../automation/automation-change-tracking.md) für Azure Automation oder unter [Guest Configuration für virtuelle Computer](../../policy/concepts/guest-configuration.md) für Azure Policy.

> [!IMPORTANT]
> Der Änderungsverlauf in Azure Resource Graph befindet sich in der öffentlichen Vorschau.

## <a name="find-detected-change-events-and-view-change-details"></a>Suchen von erkannten Änderungsereignissen und Anzeigen von Änderungsdetails

Der erste Schritt beim Ermitteln, was sich in einer Ressource geändert hat, besteht darin, die Änderungsereignisse in dieser Ressource innerhalb eines Zeitfensters zu suchen. Jedes Änderungsereignis schließt auch Details dazu ein, was sich in der Ressource geändert hat. Dieser Schritt erfolgt über den REST-Endpunkt **resourceChanges**.

Der Endpunkt **resourceChanges** akzeptiert die folgenden Parameter im Anforderungstext:

- **resourceId** \[erforderlich\]: Die Azure-Ressource, in der nach Änderungen gesucht werden soll.
- **interval** \[erforderlich\]: Eine Eigenschaft mit dem _Start-_ und _Enddatum_, innerhalb derer in der **Zulu-Zeitzone (Z)** nach einem Änderungsereignis gesucht werden soll.
- **fetchPropertyChanges** (optional): Eine boolesche Eigenschaft, die festlegt, ob das Antwortobjekt Eigenschaftenänderungen einschließt.

Beispiel für Anforderungstext:

```json
{
    "resourceId": "/subscriptions/{subscriptionId}/resourceGroups/MyResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
    "interval": {
        "start": "2019-09-28T00:00:00.000Z",
        "end": "2019-09-29T00:00:00.000Z"
    },
    "fetchPropertyChanges": true
}
```

Mit dem Anforderungstext oben lautet der REST-API-URI für **resourceChanges** wie folgt:

```http
POST https://management.azure.com/providers/Microsoft.ResourceGraph/resourceChanges?api-version=2018-09-01-preview
```

Die Antwort ähnelt der im folgenden Beispiel:

```json
{
    "changes": [
        {
            "changeId": "{\"beforeId\":\"3262e382-9f73-4866-a2e9-9d9dbee6a796\",\"beforeTime\":\"2019-09-28T00:45:35.012Z\",\"afterId\":\"6178968e-981e-4dac-ac37-340ee73eb577\",\"afterTime\":\"2019-09-28T00:52:53.371Z\"}",
            "beforeSnapshot": {
                "snapshotId": "3262e382-9f73-4866-a2e9-9d9dbee6a796",
                "timestamp": "2019-09-28T00:45:35.012Z"
            },
            "afterSnapshot": {
                "snapshotId": "6178968e-981e-4dac-ac37-340ee73eb577",
                "timestamp": "2019-09-28T00:52:53.371Z"
            },
            "changeType": "Create"
        },
        {
            "changeId": "{\"beforeId\":\"a00f5dac-86a1-4d86-a1c5-a9f7c8147b7c\",\"beforeTime\":\"2019-09-28T00:43:38.366Z\",\"afterId\":\"3262e382-9f73-4866-a2e9-9d9dbee6a796\",\"afterTime\":\"2019-09-28T00:45:35.012Z\"}",
            "beforeSnapshot": {
                "snapshotId": "a00f5dac-86a1-4d86-a1c5-a9f7c8147b7c",
                "timestamp": "2019-09-28T00:43:38.366Z"
            },
            "afterSnapshot": {
                "snapshotId": "3262e382-9f73-4866-a2e9-9d9dbee6a796",
                "timestamp": "2019-09-28T00:45:35.012Z"
            },
            "changeType": "Delete"
        },
        {
            "changeId": "{\"beforeId\":\"b37a90d1-7ebf-41cd-8766-eb95e7ee4f1c\",\"beforeTime\":\"2019-09-28T00:43:15.518Z\",\"afterId\":\"a00f5dac-86a1-4d86-a1c5-a9f7c8147b7c\",\"afterTime\":\"2019-09-28T00:43:38.366Z\"}",
            "beforeSnapshot": {
                "snapshotId": "b37a90d1-7ebf-41cd-8766-eb95e7ee4f1c",
                "timestamp": "2019-09-28T00:43:15.518Z"
            },
            "afterSnapshot": {
                "snapshotId": "a00f5dac-86a1-4d86-a1c5-a9f7c8147b7c",
                "timestamp": "2019-09-28T00:43:38.366Z"
            },
            "propertyChanges": [
                {
                    "propertyName": "tags.org",
                    "afterValue": "compute",
                    "changeCategory": "User",
                    "changeType": "Insert"
                },
                {
                    "propertyName": "tags.team",
                    "afterValue": "ARG",
                    "changeCategory": "User",
                    "changeType": "Insert"
                }
            ],
            "changeType": "Update"
        },
        {
            "changeId": "{\"beforeId\":\"19d12ab1-6ac6-4cd7-a2fe-d453a8e5b268\",\"beforeTime\":\"2019-09-28T00:42:46.839Z\",\"afterId\":\"b37a90d1-7ebf-41cd-8766-eb95e7ee4f1c\",\"afterTime\":\"2019-09-28T00:43:15.518Z\"}",
            "beforeSnapshot": {
                "snapshotId": "19d12ab1-6ac6-4cd7-a2fe-d453a8e5b268",
                "timestamp": "2019-09-28T00:42:46.839Z"
            },
            "afterSnapshot": {
                "snapshotId": "b37a90d1-7ebf-41cd-8766-eb95e7ee4f1c",
                "timestamp": "2019-09-28T00:43:15.518Z"
            },
            "propertyChanges": [{
                "propertyName": "tags.cgtest",
                "afterValue": "hello",
                "changeCategory": "User",
                "changeType": "Insert"
            }],
            "changeType": "Update"
        }
    ]
}
```

Jedes erkannte Änderungsereignis für die **resourceId** weist die folgenden Eigenschaften auf:

- **changeId**: Dieser Wert ist eindeutig für die betreffende Ressource. Die Zeichenfolge **changeId** kann auch andere Eigenschaften enthalten, jedoch ist nur ihre Eindeutigkeit garantiert.
- **beforeSnapshot**: Enthält die **snapshotId** und den **timestamp** der Ressourcenmomentaufnahme, die vor dem Erkennen einer Änderung aufgenommen wurde.
- **afterSnapshot**: Enthält die **snapshotId** und den **timestamp** der Ressourcenmomentaufnahme, die nach dem Erkennen einer Änderung aufgenommen wurde.
- **changeType**: Beschreibt die Art der Änderung, die für den gesamten Änderungsdatensatz zwischen **beforeSnapshot** und **afterSnapshot** erkannt wurde. Werte: _Create_, _Update_ und _Delete_. Das **propertyChanges**-Eigenschaftenarray ist nur enthalten, wenn **changeType** gleich _Update_ ist.
- **propertyChanges**: In diesem Array von Eigenschaften werden alle Ressourceneigenschaften aufgeführt, die zwischen **beforeSnapshot** und **afterSnapshot** aktualisiert wurden:
  - **propertyName**: Der Name der Ressourceneigenschaft, die geändert wurde.
  - **changeCategory**: Eine Beschreibung der vorgenommenen Änderung. Werte: _System_ und _User_.
  - **changeType**: Beschreibt die Art der Änderung, die für die betreffende Ressourceneigenschaft erkannt wurde.
    Werte: _Insert_, _Update_, _Remove_.
  - **beforeValue**: Der Wert der Ressourceneigenschaft im **beforeSnapshot**. Dieser wird nicht angezeigt, wenn **changeType** gleich _Insert_ ist.
  - **afterValue**: Der Wert der Ressourceneigenschaft im **afterSnapshot**. Dieser wird nicht angezeigt, wenn **changeType** gleich _Remove_ ist.

## <a name="compare-resource-changes"></a>Vergleichen von Ressourcenänderungen

Mit der **changeId** des Endpunkts **resourceChanges** werden dann anhand des REST-Endpunkts **resourceChangeDetails** die Vorher- und Nachher-Momentaufnahmen der geänderten Ressource abgerufen.

Der Endpunkt **resourceChangeDetails** erfordert zwei Parameter im Anforderungstext:

- **resourceId:** Die Azure-Ressource, in der ein Vergleich in Bezug auf Änderungen erfolgen soll.
- **changeId:** Das eindeutige Änderungsereignis für **resourceId**, das aus **resourceChanges** erfasst wird.

Beispiel für Anforderungstext:

```json
{
    "resourceId": "/subscriptions/{subscriptionId}/resourceGroups/MyResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
    "changeId": "{\"beforeId\":\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\",\"beforeTime\":'2019-05-09T00:00:00.000Z\",\"afterId\":\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\",\"beforeTime\":'2019-05-10T00:00:00.000Z\"}"
}
```

Mit dem Anforderungstext oben lautet der REST-API-URI für **resourceChangeDetails** wie folgt:

```http
POST https://management.azure.com/providers/Microsoft.ResourceGraph/resourceChangeDetails?api-version=2018-09-01-preview
```

Die Antwort ähnelt der im folgenden Beispiel:

```json
{
    "changeId": "{\"beforeId\":\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\",\"beforeTime\":'2019-05-09T00:00:00.000Z\",\"afterId\":\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\",\"beforeTime\":'2019-05-10T00:00:00.000Z\"}",
    "beforeSnapshot": {
        "timestamp": "2019-03-29T01:32:05.993Z",
        "content": {
            "sku": {
                "name": "Standard_LRS",
                "tier": "Standard"
            },
            "kind": "Storage",
            "id": "/subscriptions/{subscriptionId}/resourceGroups/MyResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
            "name": "mystorageaccount",
            "type": "Microsoft.Storage/storageAccounts",
            "location": "westus",
            "tags": {},
            "properties": {
                "networkAcls": {
                    "bypass": "AzureServices",
                    "virtualNetworkRules": [],
                    "ipRules": [],
                    "defaultAction": "Allow"
                },
                "supportsHttpsTrafficOnly": false,
                "encryption": {
                    "services": {
                        "file": {
                            "enabled": true,
                            "lastEnabledTime": "2018-07-27T18:37:21.8333895Z"
                        },
                        "blob": {
                            "enabled": true,
                            "lastEnabledTime": "2018-07-27T18:37:21.8333895Z"
                        }
                    },
                    "keySource": "Microsoft.Storage"
                },
                "provisioningState": "Succeeded",
                "creationTime": "2018-07-27T18:37:21.7708872Z",
                "primaryEndpoints": {
                    "blob": "https://mystorageaccount.blob.core.windows.net/",
                    "queue": "https://mystorageaccount.queue.core.windows.net/",
                    "table": "https://mystorageaccount.table.core.windows.net/",
                    "file": "https://mystorageaccount.file.core.windows.net/"
                },
                "primaryLocation": "westus",
                "statusOfPrimary": "available"
            }
        }
    },
    "afterSnapshot": {
        "timestamp": "2019-03-29T01:54:24.42Z",
        "content": {
            "sku": {
                "name": "Standard_LRS",
                "tier": "Standard"
            },
            "kind": "Storage",
            "id": "/subscriptions/{subscriptionId}/resourceGroups/MyResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
            "name": "mystorageaccount",
            "type": "Microsoft.Storage/storageAccounts",
            "location": "westus",
            "tags": {},
            "properties": {
                "networkAcls": {
                    "bypass": "AzureServices",
                    "virtualNetworkRules": [],
                    "ipRules": [],
                    "defaultAction": "Allow"
                },
                "supportsHttpsTrafficOnly": true,
                "encryption": {
                    "services": {
                        "file": {
                            "enabled": true,
                            "lastEnabledTime": "2018-07-27T18:37:21.8333895Z"
                        },
                        "blob": {
                            "enabled": true,
                            "lastEnabledTime": "2018-07-27T18:37:21.8333895Z"
                        }
                    },
                    "keySource": "Microsoft.Storage"
                },
                "provisioningState": "Succeeded",
                "creationTime": "2018-07-27T18:37:21.7708872Z",
                "primaryEndpoints": {
                    "blob": "https://mystorageaccount.blob.core.windows.net/",
                    "queue": "https://mystorageaccount.queue.core.windows.net/",
                    "table": "https://mystorageaccount.table.core.windows.net/",
                    "file": "https://mystorageaccount.file.core.windows.net/"
                },
                "primaryLocation": "westus",
                "statusOfPrimary": "available"
            }
        }
    }
}
```

**beforeSnapshot** und **afterSnapshot** geben jeweils den Zeitpunkt, an dem die Momentaufnahme erstellt wurde, und die Eigenschaften zu diesem Zeitpunkt an. Die Änderung wurde zu einem Zeitpunkt zwischen diesen Momentaufnahmen vorgenommen. Im Beispiel oben können Sie sehen, dass es sich bei der geänderten Eigenschaft um **supportsHttpsTrafficOnly** handelt.

Verwenden Sie zum Vergleichen der Ergebnisse die **changes**-Eigenschaft in **resourceChanges**, oder untersuchen Sie den **content**-Abschnitt in jeder Momentaufnahme in **resourceChangeDetails**, um den Unterschied zu ermitteln. Wenn Sie die Momentaufnahmen vergleichen, wird **timestamp** entgegen der Erwartung immer als Unterschied angezeigt.

## <a name="next-steps"></a>Nächste Schritte

- Informationen zur verwendeten Sprache finden Sie unter [Einfache Abfragen](../samples/starter.md).
- Informationen zur anspruchsvolleren Nutzung finden Sie unter [Erweiterte Abfragen](../samples/advanced.md).
- Erfahren Sie, wie Sie [Ressourcen untersuchen](../concepts/explore-resources.md).