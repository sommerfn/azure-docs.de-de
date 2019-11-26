---
title: 'Azure CLI-Skriptbeispiel: Abrufen des Hostnamens, der Ports und Schlüssel für Azure Cache for Redis'
description: 'Azure CLI-Skriptbeispiel: Abrufen des Hostnamens, der Ports und Schlüssel für eine Azure Cache for Redis-Instanz'
author: yegu-ms
tags: azure-service-management
ms.service: cache
ms.devlang: azurecli
ms.topic: sample
ms.date: 08/30/2017
ms.author: yegu
ms.openlocfilehash: 604ec6de3b95a4bc289176d54d9c7b0a6c42eae6
ms.sourcegitcommit: 5a8c65d7420daee9667660d560be9d77fa93e9c9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2019
ms.locfileid: "74122517"
---
# <a name="get-the-hostname-ports-and-keys-for-azure-cache-for-redis"></a>Abrufen des Hostnamens, der Ports und Schlüssel für Azure Cache for Redis

In diesem Szenario erfahren Sie, wie Sie den Hostnamen, die Ports und Schlüssel abrufen, mit deren Hilfe eine Verbindung mit einer Azure Cache for Redis-Instanz hergestellt wird.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Beispielskript

[!code-azurecli[main](../../../cli_scripts/redis-cache/cache-keys-ports/cache-keys-ports.sh "Azure Cache for Redis")]


## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet die folgenden Befehle, um den Hostnamen, die Schlüssel und Ports einer Azure Cache for Redis-Instanz abzurufen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az redis show](https://docs.microsoft.com/cli/azure/redis) | Details zu einer Azure Cache for Redis-Instanz abrufen. |
| [az redis list-keys](https://docs.microsoft.com/cli/azure/redis) | Zugriffsschlüssel für eine Azure Cache for Redis-Instanz abrufen. |


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure).

Zusätzliche Azure Cache for Redis-CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Azure Cache for Redis](../cli-samples.md).
