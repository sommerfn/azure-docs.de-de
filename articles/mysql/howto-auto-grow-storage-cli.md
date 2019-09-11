---
title: Automatisches Vergrößern des Azure Database for MySQL-Speichers per Azure CLI
description: In diesem Artikel wird beschrieben, wie Sie die automatische Speichervergrößerung für Azure Database for MySQL per Azure CLI aktivieren können.
author: ambhatna
ms.author: ambhatna
ms.service: mysql
ms.topic: conceptual
ms.date: 8/7/2019
ms.openlocfilehash: f9625d8b7d27fd47bf2088d552c0a2fc64b141c9
ms.sourcegitcommit: d200cd7f4de113291fbd57e573ada042a393e545
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/29/2019
ms.locfileid: "70143289"
---
# <a name="auto-grow-azure-database-for-mysql-storage-using-the-azure-cli"></a>Automatisches Vergrößern des Azure Database for MySQL-Speichers per Azure CLI
In diesem Artikel ist beschrieben, wie Sie die Speichervergrößerung für einen Azure Database for MySQL-Server konfigurieren können, ohne die Workload zu beeinträchtigen.

Der Server, für den die [Speicherbegrenzung erreicht wird](https://docs.microsoft.com/azure/mysql/concepts-pricing-tiers#reaching-the-storage-limit), ist auf „Schreibgeschützt“ festgelegt. Wenn die automatische Speichervergrößerung dann für Server mit weniger als 100 GB bereitgestelltem Speicher aktiviert wird, wird die bereitgestellte Speichergröße um 5 GB erhöht, sobald der freie Speicher unter 1 GB oder zehn Prozent des bereitgestellten Speichers sinkt. Bei Servern mit mehr als 100 GB bereitgestelltem Speicher wird die bereitgestellte Speichergröße um fünf Prozent erhöht, sobald der freie Speicherplatz unter fünf Prozent der bereitgestellten Speichergröße sinkt. Dabei gelten die [hier](https://docs.microsoft.com/azure/mysql/concepts-pricing-tiers#storage) beschriebenen Grenzwerte für maximalen Speicher.

## <a name="prerequisites"></a>Voraussetzungen
Zum Durcharbeiten dieses Leitfadens benötigen Sie Folgendes:
- Eine [Serverinstanz von Azure Database for MySQL](quickstart-create-mysql-server-database-using-azure-cli.md)

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

> [!IMPORTANT]
> Diese Anleitung setzt die Verwendung von Azure CLI-Version 2.0 oder höher voraus. Geben Sie zum Bestätigen der Version an der Eingabeaufforderung von Azure CLI `az --version` ein. Informationen zum Ausführen einer Installation oder eines Upgrades finden Sie unter [Installieren der Azure CLI]( /cli/azure/install-azure-cli).

## <a name="enable-mysql-server-storage-auto-grow"></a>Aktivieren der automatischen Speichervergrößerung für den MySQL-Server

Aktivieren Sie die automatische Speichervergrößerung auf einem vorhandenen Server mit dem folgenden Befehl:

```azurecli-interactive
az mysql server update --name mydemoserver --resource-group myresourcegroup --auto-grow Enabled
```

Aktivieren Sie die automatische Speichervergrößerung beim Erstellen eines neuen Servers mit dem folgenden Befehl:

```azurecli-interactive
az mysql server create --resource-group myresourcegroup --name mydemoserver  --auto-grow Enabled --location westus --admin-user myadmin --admin-password <server_admin_password> --sku-name GP_Gen4_2 --version 5.7
```

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie [Warnungen zu Metriken erstellen](howto-alert-on-metric.md).