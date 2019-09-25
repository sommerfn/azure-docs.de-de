---
title: Verwalten von Lesereplikaten für Azure Database for PostgreSQL (Einzelserver) über die Azure CLI, REST-API
description: Erfahren Sie, wie Sie Lesereplikate für Azure Database for PostgreSQL verwalten – Einzelserver über die Azure CLI und die REST-API.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 09/12/2019
ms.openlocfilehash: e146a0f42b6487545321ebfbc9ec523da5e78c8a
ms.sourcegitcommit: 1752581945226a748b3c7141bffeb1c0616ad720
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/14/2019
ms.locfileid: "70995351"
---
# <a name="create-and-manage-read-replicas-from-the-azure-cli-rest-api"></a>Erstellen und Verwalten von Lesereplikaten über die Azure CLI und die REST-API

In diesem Artikel erfahren Sie, wie Sie Lesereplikate in Azure Database for PostgreSQL über die Azure CLI und die REST-API erstellen und verwalten. Weitere Informationen zu Lesereplikaten finden Sie in der [Übersicht](concepts-read-replicas.md).

## <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle
Sie können Lesereplikate mithilfe der Azure CLI erstellen und verwalten.

### <a name="prerequisites"></a>Voraussetzungen

- [Installieren der Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)
- Ein [Azure Database for PostgreSQL-Server](quickstart-create-server-up-azure-cli.md), der als Masterserver verwendet wird


### <a name="prepare-the-master-server"></a>Vorbereiten des Masterservers
Diese Schritte müssen durchgeführt werden, um einen Masterserver in den Tarifen „Universell“ und „Arbeitsspeicheroptimiert“ vorzubereiten.

Auf dem Masterserver muss der Parameter `azure.replication_support` auf **REPLICA** festgelegt werden. Bei Änderung dieses statischen Parameters ist ein Neustart des Servers erforderlich, damit die Änderung wirksam wird.

1. Legen Sie `azure.replication_support` auf „REPLICA“ fest.

   ```azurecli-interactive
   az postgres server configuration set --resource-group myresourcegroup --server-name mydemoserver --name azure.replication_support --value REPLICA
   ```

2. Starten Sie den Server neu, um die Änderung zu übernehmen.

   ```azurecli-interactive
   az postgres server restart --name mydemoserver --resource-group myresourcegroup
   ```

### <a name="create-a-read-replica"></a>Erstellen eines Lesereplikats

Der Befehl [az postgres server replica create](/cli/azure/postgres/server/replica?view=azure-cli-latest#az-postgres-server-replica-create) erfordert die folgenden Parameter:

| Einstellung | Beispielwert | BESCHREIBUNG  |
| --- | --- | --- |
| resource-group | myresourcegroup |  Die Ressourcengruppe, in der der Replikatserver erstellt wird.  |
| name | mydemoserver-replica | Der Name des neuen Replikatservers, der erstellt wird. |
| source-server | mydemoserver | Der Name oder die Ressourcen-ID des vorhandenen Masterservers, von dem die Replikation erfolgt. |

Im folgenden CLI-Beispiel wird das Replikat in der gleichen Region wie der Master erstellt.

```azurecli-interactive
az postgres server replica create --name mydemoserver-replica --source-server mydemoserver --resource-group myresourcegroup
```

Verwenden Sie den `--location`-Parameter, um ein regionsübergreifendes Lesereplikat zu erstellen. Im folgenden CLI-Beispiel wird das Replikat in der Region „USA, Westen“ erstellt.

```azurecli-interactive
az postgres server replica create --name mydemoserver-replica --source-server mydemoserver --resource-group myresourcegroup --location westus
```

> [!NOTE]
> Weitere Informationen zu den Regionen, in denen Sie ein Replikat erstellen können, finden Sie im [Konzeptartikel zu Lesereplikaten](concepts-read-replicas.md). 

Wenn Sie den Parameter `azure.replication_support` auf einem universellen oder arbeitsspeicheroptimierten Masterserver nicht auf **REPLICA** festgelegt und den Server nicht neu gestartet haben, erhalten Sie einen Fehler. Führen Sie diese beiden Schritte aus, bevor Sie ein Replikat erstellen.

Ein Replikat wird mit den gleichen Compute- und Speichereinstellungen erstellt wie der Master. Nachdem ein Replikat erstellt wurde, können mehrere Einstellungen unabhängig vom Masterserver geändert werden: die Computegeneration, die virtuellen Kerne, der Speicher und der Aufbewahrungszeitraum für Sicherungen. Auch der Tarif kann unabhängig geändert werden, allerdings nicht in den oder aus dem Tarif „Basic“.

> [!IMPORTANT]
> Bevor eine Masterservereinstellung auf einen neuen Wert aktualisiert wird, aktualisieren Sie die Replikateinstellung auf den gleichen oder einen größeren Wert. Diese Aktion sorgt dafür, dass das Replikat mit allen Änderungen auf dem Masterserver Schritt halten kann.

### <a name="list-replicas"></a>Auflisten von Replikaten
Sie können die Liste der Replikate eines Masterservers mit dem Befehl [az postgres server replica list](/cli/azure/postgres/server/replica?view=azure-cli-latest#az-postgres-server-replica-list) anzeigen.

```azurecli-interactive
az postgres server replica list --server-name mydemoserver --resource-group myresourcegroup 
```

### <a name="stop-replication-to-a-replica-server"></a>Beenden der Replikation auf einem Replikatserver
Sie können die Replikation zwischen einem Masterserver und einem Lesereplikat mit dem Befehl [az postgres server replica stop](/cli/azure/postgres/server/replica?view=azure-cli-latest#az-postgres-server-replica-stop) beenden.

Das Beenden der Replikation zwischen einem Masterserver und einem Lesereplikat kann nicht mehr rückgängig gemacht werden. Das Lesereplikat wird zu einem eigenständigen Server, der sowohl Lese- als auch Schreibvorgänge unterstützt. Der eigenständige Server kann nicht wieder in ein Replikat umgewandelt werden.

```azurecli-interactive
az postgres server replica stop --name mydemoserver-replica --resource-group myresourcegroup 
```

### <a name="delete-a-master-or-replica-server"></a>Löschen eines Master- oder Replikatservers
Wenn Sie einen Master- oder Replikatserver löschen möchten, verwenden Sie den Befehl [az postgres server delete](/cli/azure/postgres/server?view=azure-cli-latest#az-postgres-server-delete).

Wenn Sie einen Masterserver löschen, wird die Replikation auf allen Lesereplikaten beendet. Die Lesereplikate werden zu eigenständigen Servern, die nun Lese- und Schreibvorgänge unterstützen.

```azurecli-interactive
az postgres server delete --name myserver --resource-group myresourcegroup
```

## <a name="rest-api"></a>REST-API
Sie können Lesereplikate mithilfe der [ Azure-REST-API](/rest/api/azure/) erstellen und verwalten.

### <a name="prepare-the-master-server"></a>Vorbereiten des Masterservers
Diese Schritte müssen durchgeführt werden, um einen Masterserver in den Tarifen „Universell“ und „Arbeitsspeicheroptimiert“ vorzubereiten.

Auf dem Masterserver muss der Parameter `azure.replication_support` auf **REPLICA** festgelegt werden. Bei Änderung dieses statischen Parameters ist ein Neustart des Servers erforderlich, damit die Änderung wirksam wird.

1. Legen Sie `azure.replication_support` auf „REPLICA“ fest.

   ```http
   PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DBforPostgreSQL/servers/{masterServerName}/configurations/azure.replication_support?api-version=2017-12-01
   ```

   ```JSON
   {
    "properties": {
    "value": "replica"
    }
   }
   ```

2. [Starten Sie den Server neu](/rest/api/postgresql/servers/restart), um die Änderung zu übernehmen.

   ```http
   POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DBforPostgreSQL/servers/{masterServerName}/restart?api-version=2017-12-01
   ```

### <a name="create-a-read-replica"></a>Erstellen eines Lesereplikats
Sie können ein Lesereplikat mithilfe der [Create-API](/rest/api/postgresql/servers/create) erstellen:

```http
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DBforPostgreSQL/servers/{replicaName}?api-version=2017-12-01
```

```json
{
  "location": "southeastasia",
  "properties": {
    "createMode": "Replica",
    "sourceServerId": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DBforPostgreSQL/servers/{masterServerName}"
  }
}
```

> [!NOTE]
> Weitere Informationen zu den Regionen, in denen Sie ein Replikat erstellen können, finden Sie im [Konzeptartikel zu Lesereplikaten](concepts-read-replicas.md). 

Wenn Sie den Parameter `azure.replication_support` auf einem universellen oder arbeitsspeicheroptimierten Masterserver nicht auf **REPLICA** festgelegt und den Server nicht neu gestartet haben, erhalten Sie einen Fehler. Führen Sie diese beiden Schritte aus, bevor Sie ein Replikat erstellen.

Ein Replikat wird mit den gleichen Compute- und Speichereinstellungen erstellt wie der Master. Nachdem ein Replikat erstellt wurde, können mehrere Einstellungen unabhängig vom Masterserver geändert werden: die Computegeneration, die virtuellen Kerne, der Speicher und der Aufbewahrungszeitraum für Sicherungen. Auch der Tarif kann unabhängig geändert werden, allerdings nicht in den oder aus dem Tarif „Basic“.


> [!IMPORTANT]
> Bevor eine Masterservereinstellung auf einen neuen Wert aktualisiert wird, aktualisieren Sie die Replikateinstellung auf den gleichen oder einen größeren Wert. Diese Aktion sorgt dafür, dass das Replikat mit allen Änderungen auf dem Masterserver Schritt halten kann.

### <a name="list-replicas"></a>Auflisten von Replikaten
Sie können die Replikatliste eines Masterservers mithilfe der [Replica List-API](/rest/api/postgresql/replicas/listbyserver) anzeigen:

```http
GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DBforPostgreSQL/servers/{masterServerName}/Replicas?api-version=2017-12-01
```

### <a name="stop-replication-to-a-replica-server"></a>Beenden der Replikation auf einem Replikatserver
Sie können die Replikation zwischen einem Masterserver und einem Lesereplikat mithilfe der [Update-API](/rest/api/postgresql/servers/update) beenden.

Das Beenden der Replikation zwischen einem Masterserver und einem Lesereplikat kann nicht mehr rückgängig gemacht werden. Das Lesereplikat wird zu einem eigenständigen Server, der sowohl Lese- als auch Schreibvorgänge unterstützt. Der eigenständige Server kann nicht wieder in ein Replikat umgewandelt werden.

```http
PATCH https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DBforPostgreSQL/servers/{masterServerName}?api-version=2017-12-01
```

```json
{
  "properties": {
    "replicationRole":"None"  
   }
}
```

### <a name="delete-a-master-or-replica-server"></a>Löschen eines Master- oder Replikatservers
Zum Löschen eines Master- oder Replikatservers verwenden Sie die [Delete-API](/rest/api/postgresql/servers/delete):

Wenn Sie einen Masterserver löschen, wird die Replikation auf allen Lesereplikaten beendet. Die Lesereplikate werden zu eigenständigen Servern, die nun Lese- und Schreibvorgänge unterstützen.

```http
DELETE https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DBforPostgreSQL/servers/{serverName}?api-version=2017-12-01
```

## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie mehr über [Lesereplikate in Azure Database for PostgreSQL](concepts-read-replicas.md).
* Erhalten Sie Informationen zum [Erstellen und Verwalten von Lesereplikaten im Azure-Portal](howto-read-replicas-portal.md).