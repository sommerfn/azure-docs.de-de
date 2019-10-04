---
title: 'Azure CLI-Skript: Skalieren eines Azure Database for MariaDB-Servers'
description: Dieses CLI-Beispielskript skaliert einen Azure Database for MariaDB-Server nach Abfragen der Metriken auf eine andere Leistungsstufe.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.devlang: azurecli
ms.topic: sample
ms.custom: mvc
ms.date: 08/07/2019
ms.openlocfilehash: 772c4de9c53f618d0c7c5d8b8d6acc2830595f55
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/09/2019
ms.locfileid: "68884143"
---
# <a name="monitor-and-scale-an-azure-database-for-mariadb-server-using-azure-cli"></a>Überwachen und Skalieren eines Azure Database for MariaDB-Servers mithilfe der Azure CLI
Dieses CLI-Beispielskript skaliert Compute- und Speicherressourcen für einen einzelnen Azure Database for MariaDB-Server nach Abfragen der Metriken. Computeressourcen können zentral hoch- oder herunterskaliert werden. Speicherressourcen können nur zentral hochskaliert werden.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Wenn Sie die Befehlszeilenschnittelle lokal ausführen, ist für diesen Artikel Azure CLI Version 2.0 oder höher erforderlich. Überprüfen Sie die Version, indem Sie `az --version` ausführen. Informationen zum Installieren oder Aktualisieren Ihrer Version der Azure CLI finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Beispielskript
Aktualisieren Sie das Skript mit Ihrer Abonnement-ID.
[!code-azurecli-interactive[main](../../../cli_scripts/mariadb/scale-mariadb-server/scale-mariadb-server.sh "Create and scale Azure Database for MariaDB.")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung
Verwenden Sie den folgenden Befehl, um die Ressourcengruppe und alle damit verbundenen Ressourcen zu entfernen, nachdem das Skript ausgeführt wurde. 
[!code-azurecli-interactive[main](../../../cli_scripts/mariadb/scale-mariadb-server/delete-mariadb.sh  "Delete the resource group.")]

## <a name="script-explanation"></a>Erläuterung des Skripts
In diesem Skript werden die Befehle verwendet, die in der folgenden Tabelle aufgeführt sind:

| **Befehl** | **Hinweise** |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az mariadb server create](/cli/azure/mariadb/server#az-mariadb-server-create) | Erstellt einen MariaDB-Server, der die Datenbanken hostet. |
| [az mariadb server update](/cli/azure/mariadb/server#az-mariadb-server-update) | Aktualisiert die Eigenschaften des MariaDB-Servers. |
| [az monitor metrics list](/cli/azure/monitor/metrics#az-monitor-metrics-list) | Listet den Metrikwert für die Ressourcen auf. |
| [az group delete](/cli/azure/group#az-group-delete) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte
- Informieren Sie sich ausführlicher über [Compute- und Speicherressourcen in Azure Database for MariaDB](../concepts-pricing-tiers.md).
- Probieren Sie weitere Skripts aus: [Azure CLI-Beispiele für Azure Database for MariaDB](../sample-scripts-azure-cli.md)
- Lesen Sie weitere Informationen zur [Azure-Befehlszeilenschnittstelle](/cli/azure).
