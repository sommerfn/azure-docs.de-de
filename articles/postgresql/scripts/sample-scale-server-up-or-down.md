---
title: 'Azure CLI-Skript: Skalieren und Überwachen einer Azure Database for PostgreSQL-Instanz'
description: 'Azure CLI-Skriptbeispiel: Skalieren eines Azure-Datenbank für PostgreSQL-Servers auf eine andere Leistungsstufe nach Abfragen der Metriken.'
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.devlang: azurecli
ms.custom: mvc
ms.topic: sample
ms.date: 08/07/2019
ms.openlocfilehash: 24aaf461576e6e043979660f9de968358763e003
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/09/2019
ms.locfileid: "68882990"
---
# <a name="monitor-and-scale-a-single-postgresql-server-using-azure-cli"></a>Überwachen und Skalieren eines einzelnen PostgreSQL-Servers mit der Azure CLI
Dieses CLI-Beispielskript skaliert Compute- und Speicherressourcen für einen einzelnen Azure Database for PostgreSQL-Server nach Abfragen der Metriken. Computeressourcen können zentral hoch- oder herunterskaliert werden. Speicherressourcen können nur zentral hochskaliert werden. 

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Wenn Sie die Befehlszeilenschnittelle lokal ausführen, ist für diesen Artikel Azure CLI Version 2.0 oder höher erforderlich. Überprüfen Sie die Version, indem Sie `az --version` ausführen. Informationen zum Installieren oder Aktualisieren Ihrer Version der Azure CLI finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli).

## <a name="sample-script"></a>Beispielskript
Aktualisieren Sie das Skript mit Ihrer Abonnement-ID.
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/scale-postgresql-server/scale-postgresql-server.sh "Create and scale Azure Database for PostgreSQL.")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung
Verwenden Sie den folgenden Befehl, um die Ressourcengruppe und alle damit verbundenen Ressourcen zu entfernen, nachdem das Skript ausgeführt wurde. 
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/scale-postgresql-server/delete-postgresql.sh "Delete the resource group.")]

## <a name="script-explanation"></a>Erläuterung des Skripts
In diesem Skript werden die Befehle verwendet, die in der folgenden Tabelle aufgeführt sind:

| **Befehl** | **Hinweise** |
|---|---|
| [az group create](/cli/azure/group) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az postgres server create](/cli/azure/postgres/server#az-postgres-server-create) | Erstellt einen PostgreSQL-Server, der die Datenbanken hostet. |
| [az postgres server update](/cli/azure/postgres/server#az-postgres-server-update) | Aktualisiert die Eigenschaften des PostgreSQL-Servers. |
| [az monitor metrics list](/cli/azure/monitor/metrics) | Listet den Metrikwert für die Ressourcen auf. |
| [az group delete](/cli/azure/group) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte
- Informieren Sie sich ausführlicher über [Compute- und Speicherressourcen in Azure Database for PostgreSQL](../concepts-pricing-tiers.md).
- Probieren Sie weitere Skripts aus: [Azure CLI-Beispiele für Azure-Datenbank für PostgreSQL](../sample-scripts-azure-cli.md)
- Lesen Sie weitere Informationen zur [Azure-Befehlszeilenschnittstelle](/cli/azure).
