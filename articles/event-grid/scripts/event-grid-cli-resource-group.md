---
title: 'Azure CLI-Skriptbeispiel: Abonnieren einer Ressourcengruppe | Microsoft-Dokumentation'
description: 'Azure CLI-Skriptbeispiel: Abonnieren einer Ressourcengruppe'
services: event-grid
documentationcenter: na
author: tfitzmac
ms.service: event-grid
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2018
ms.author: tomfitz
ms.openlocfilehash: 7bc07ec294e341c7f96c60fd2c9916b0c6b9f215
ms.sourcegitcommit: f0c2758fb8ccfaba76ce0b17833ca019a8a09d46
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2018
ms.locfileid: "51035835"
---
# <a name="subscribe-to-events-for-a-resource-group-with-azure-cli"></a>Abonnieren von Ereignissen für eine Ressourcengruppe mit der Azure CLI

Dieses Skript erstellt ein Event Grid-Abonnement für die Ereignisse für eine Ressourcengruppe. 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

Für das Vorschaubeispielskript ist die Event Grid-Erweiterung erforderlich. Führen Sie zum Installieren `az extension add --name eventgrid` aus.

## <a name="sample-script---stable"></a>Beispielskript: stabil

[!code-azurecli[main](../../../cli_scripts/event-grid/subscribe-to-resource-group/subscribe-to-resource-group.sh "Subscribe to resource group")]

## <a name="sample-script---preview-extension"></a>Beispielskript: Vorschauerweiterung

[!code-azurecli[main](../../../cli_scripts/event-grid/subscribe-to-resource-group-preview/subscribe-to-resource-group-preview.sh "Subscribe to resource group")]

## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet den folgenden Befehl, um das Ereignisabonnement zu erstellen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az eventgrid event-subscription create](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription#az-eventgrid-event-subscription-create) | Erstellen Sie ein Event Grid-Abonnement. |
| [az eventgrid event-subscription create](/cli/azure/ext/eventgrid/eventgrid/event-subscription#ext-eventgrid-az-eventgrid-event-subscription-create) - extension version | Erstellen Sie ein Event Grid-Abonnement. |

## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Abfragen von Abonnements finden Sie unter [Abfragen von Event Grid-Abonnements](../query-event-subscriptions.md).
* Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure).
