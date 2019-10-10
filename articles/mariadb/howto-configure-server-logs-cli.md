---
title: Zugreifen auf Serverprotokolle in Azure Database for MariaDB mithilfe der Azure-Befehlszeilenschnittstelle
description: In diesem Artikel wird beschrieben, wie Sie mit der Azure CLI-Befehlszeile auf die Serverprotokolle in Azure Database for MariaDB zugreifen.
author: rachel-msft
ms.author: raagyema
ms.service: mariadb
ms.devlang: azurecli
ms.topic: conceptual
ms.date: 06/12/2019
ms.openlocfilehash: ffc724ef5133ee25643a966d2b6d8448a4c3a920
ms.sourcegitcommit: 11265f4ff9f8e727a0cbf2af20a8057f5923ccda
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/08/2019
ms.locfileid: "72023614"
---
# <a name="configure-and-access-server-logs-by-using-azure-cli"></a>Konfigurieren der und Zugreifen auf die Serverprotokolle mithilfe der Azure CLI
Sie können die Serverprotokolle von Azure Database for MariaDB mithilfe der Azure CLI, dem Azure-Befehlszeilenprogramm, herunterladen.

## <a name="prerequisites"></a>Voraussetzungen
Zum Ausführen der Schritte in dieser Anleitung benötigen Sie Folgendes:
- [Azure Database for MariaDB-Server](quickstart-create-mariadb-server-database-using-azure-cli.md)
- Die [Azure CLI](/cli/azure/install-azure-cli) oder Azure Cloud Shell im Browser

## <a name="configure-logging-for-azure-database-for-mariadb"></a>Konfigurieren der Protokollierung für Azure Database for MariaDB
Sie können den Server mit folgenden Schritten für den Zugriff auf das Protokoll für langsame MariaDB-Abfragen konfigurieren:
1. Aktivieren Sie die Protokollierung, indem Sie den Parameter **slow\_query\_log** auf „EIN“ festlegen.
2. Passen Sie andere Parameter an, z.B. **long\_query\_time** und **log\_slow\_admin\_statements**.

Unter [Konfigurieren von Serverparametern](howto-configure-server-parameters-cli.md) erfahren Sie, wie Sie den Wert dieser Parameter über die Azure-Befehlszeilenschnittstelle festlegen.

Beispielsweise aktiviert der folgende CLI-Befehl das Protokoll für langsame Abfragen, legt die Mindestlaufzeit für Protokollierung auf 10 Sekunden fest und deaktiviert die Protokollierung für die langsame Administratoranweisung. Schließlich werden die Konfigurationsoptionen für Ihre Überprüfung aufgelistet.
```azurecli-interactive
az mariadb server configuration set --name slow_query_log --resource-group myresourcegroup --server mydemoserver --value ON
az mariadb server configuration set --name long_query_time --resource-group myresourcegroup --server mydemoserver --value 10
az mariadb server configuration set --name log_slow_admin_statements --resource-group myresourcegroup --server mydemoserver --value OFF
az mariadb server configuration list --resource-group myresourcegroup --server mydemoserver
```

## <a name="list-logs-for-azure-database-for-mariadb-server"></a>Auflisten von Protokollen für Azure Database for MariaDB-Server
Um die verfügbaren Protokolldateien für langsame Abfragen für Ihren Server aufzulisten, führen Sie den Befehl [az mariadb server-logs list](/cli/azure/mariadb/server-logs#az-mariadb-server-logs-list) aus.

Sie können die Protokolldateien für den Server **mydemoserver.mariadb.database.azure.com** in der Ressourcengruppe **myresourcegroup** auflisten. Leiten Sie dann die Liste der Protokolldateien in eine Textdatei namens **log\_files\_list.txt** um.
```azurecli-interactive
az mariadb server-logs list --resource-group myresourcegroup --server mydemoserver > log_files_list.txt
```
## <a name="download-logs-from-the-server"></a>Herunterladen von Protokollen vom Server
Mit dem Befehl [az mariadb server-logs download](/cli/azure/mariadb/server-logs#az-mariadb-server-logs-download) können Sie einzelne Protokolldateien für Ihren Server herunterladen.

Laden Sie dem folgenden Beispiel entsprechend die spezifische Protokolldatei für den Server **mydemoserver.mariadb.database.azure.com** in der Ressourcengruppe **myresourcegroup** in Ihre lokale Umgebung herunter.
```azurecli-interactive
az mariadb server-logs download --name mysql-slow-mydemoserver-2018110800.log --resource-group myresourcegroup --server mydemoserver
```

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie mehr über [Protokolle für langsame Abfragen in Azure Database for MariaDB](concepts-server-logs.md).
