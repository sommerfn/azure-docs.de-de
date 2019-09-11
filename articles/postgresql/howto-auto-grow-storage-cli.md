---
title: 'Automatisches Vergrößern des Azure Database for PostgreSQL-Speichers: Einzelserver über die Azure CLI'
description: In diesem Artikel wird beschrieben, wie Sie das automatische Vergrößern von Speicher über die Azure CLI in Azure Database for PostgreSQL für einen Einzelserver aktivieren.
author: ambhatna
ms.author: ambhatna
ms.service: postgresql
ms.topic: conceptual
ms.date: 8/7/2019
ms.openlocfilehash: b51158ca9ca54f5157e47e2fcbbada8890104105
ms.sourcegitcommit: d200cd7f4de113291fbd57e573ada042a393e545
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/29/2019
ms.locfileid: "70143297"
---
# <a name="auto-grow-azure-database-for-postgresql-storage---single-server-using-the-azure-cli"></a>Automatisches Vergrößern des Azure Database for PostgreSQL-Speichers: Einzelserver über die Azure CLI
In diesem Artikel wird beschrieben, wie Sie die Speichervergrößerung für einen Azure Database for PostgreSQL-Server konfigurieren können, ohne die Workload zu beeinträchtigen.

Der Server, für den die [Speicherbegrenzung erreicht wird](https://docs.microsoft.com/azure/postgresql/concepts-pricing-tiers#reaching-the-storage-limit), ist auf „Schreibgeschützt“ festgelegt. Wenn die automatische Speichervergrößerung dann für Server mit weniger als 100 GB bereitgestelltem Speicher aktiviert wird, wird die bereitgestellte Speichergröße um 5 GB erhöht, sobald der freie Speicher unter 1 GB oder zehn Prozent des bereitgestellten Speichers sinkt. Bei Servern mit mehr als 100 GB bereitgestelltem Speicher wird die bereitgestellte Speichergröße um fünf Prozent erhöht, sobald der freie Speicherplatz unter fünf Prozent der bereitgestellten Speichergröße sinkt. Dabei gelten die [hier](https://docs.microsoft.com/azure/postgresql/concepts-pricing-tiers#storage) beschriebenen Grenzwerte für maximalen Speicher.

## <a name="prerequisites"></a>Voraussetzungen
Zum Durcharbeiten dieses Leitfadens benötigen Sie Folgendes:
- Einen [Azure-Datenbank für PostgreSQL-Server](quickstart-create-server-database-azure-cli.md)

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

> [!IMPORTANT]
> Diese Anleitung setzt die Verwendung von Azure CLI-Version 2.0 oder höher voraus. Geben Sie zum Bestätigen der Version an der Eingabeaufforderung von Azure CLI `az --version` ein. Informationen zum Ausführen einer Installation oder eines Upgrades finden Sie unter [Installieren der Azure CLI]( /cli/azure/install-azure-cli).

## <a name="enable-postgresql-server-storage-auto-grow"></a>Aktivieren der automatischen Speichervergrößerung für den PostgreSQL-Server

Aktivieren Sie die automatische Speichervergrößerung auf einem vorhandenen Server mit dem folgenden Befehl:

```azurecli-interactive
az postgres server update --name mydemoserver --resource-group myresourcegroup --auto-grow Enabled
```

Aktivieren Sie die automatische Speichervergrößerung beim Erstellen eines neuen Servers mit dem folgenden Befehl:

```azurecli-interactive
az postgres server create --resource-group myresourcegroup --name mydemoserver  --auto-grow Enabled --location westus --admin-user myadmin --admin-password <server_admin_password> --sku-name GP_Gen4_2 --version 9.6
```

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie [Warnungen zu Metriken erstellen](howto-alert-on-metric.md).