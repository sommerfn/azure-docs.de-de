---
title: Zugreifen auf Protokolle für langsame Abfragen in Azure Database for MySQL mithilfe der Azure-Befehlszeilenschnittstelle
description: In diesem Artikel wird beschrieben, wie Sie über die Azure CLI auf die Protokolle für langsame Abfragen in Azure Database for MySQL zugreifen.
author: andrela
ms.author: ajlam
ms.service: mysql
ms.devlang: azurecli
ms.topic: conceptual
ms.date: 06/12/219
ms.openlocfilehash: 740dbce579fba6347b1a7f2cfc6bcae40d3503ab
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "67052706"
---
# <a name="configure-and-access-slow-query-logs-by-using-azure-cli"></a>Konfigurieren der und Zugreifen auf die Protokolle für langsame Abfragen mithilfe der Azure CLI
Sie können die Protokolle für langsame Abfragen von Azure Database for MySQL mithilfe der Azure-Befehlszeilenschnittstelle (Azure CLI, das Azure-Befehlszeilenhilfsprogramm) herunterladen.

## <a name="prerequisites"></a>Voraussetzungen
Zum Ausführen der Schritte in dieser Anleitung benötigen Sie Folgendes:
- [Azure Database for MySQL-Server](quickstart-create-mysql-server-database-using-azure-cli.md)
- Die [Azure CLI](/cli/azure/install-azure-cli) oder Azure Cloud Shell im Browser

## <a name="configure-logging"></a>Konfigurieren der Protokollierung
Sie können den Server mit folgenden Schritten für den Zugriff auf das Protokoll für langsame MySQL-Abfragen konfigurieren:
1. Aktivieren Sie die Protokollierung für langsame Abfragen, indem Sie den Parameter **slow\_query\_log** auf „ON“ festlegen.
2. Passen Sie andere Parameter an, z.B. **long\_query\_time** und **log\_slow\_admin\_statements**.

Unter [Konfigurieren von Serverparametern](howto-configure-server-parameters-using-cli.md) erfahren Sie, wie Sie den Wert dieser Parameter über die Azure-Befehlszeilenschnittstelle festlegen.

Beispielsweise aktiviert der folgende CLI-Befehl das Protokoll für langsame Abfragen, legt die Mindestlaufzeit für Protokollierung auf 10 Sekunden fest und deaktiviert die Protokollierung für die langsame Administratoranweisung. Schließlich werden die Konfigurationsoptionen für Ihre Überprüfung aufgelistet.
```azurecli-interactive
az mysql server configuration set --name slow_query_log --resource-group myresourcegroup --server mydemoserver --value ON
az mysql server configuration set --name long_query_time --resource-group myresourcegroup --server mydemoserver --value 10
az mysql server configuration set --name log_slow_admin_statements --resource-group myresourcegroup --server mydemoserver --value OFF
az mysql server configuration list --resource-group myresourcegroup --server mydemoserver
```

## <a name="list-logs-for-azure-database-for-mysql-server"></a>Auflisten der Protokolle für Server für Azure-Datenbank für MySQL
Um die verfügbaren Protokolldateien für langsame Abfragen für Ihren Server aufzulisten, führen Sie den Befehl [az mysql server-logs list](/cli/azure/mysql/server-logs#az-mysql-server-logs-list) aus.

Sie können die Protokolldateien für den Server **mydemoserver.mysql.database.azure.com** in der Ressourcengruppe **myresourcegroup** auflisten. Leiten Sie dann die Liste der Protokolldateien in eine Textdatei namens **log\_files\_list.txt** um.
```azurecli-interactive
az mysql server-logs list --resource-group myresourcegroup --server mydemoserver > log_files_list.txt
```
## <a name="download-logs-from-the-server"></a>Herunterladen von Protokollen vom Server
Mit dem Befehl [az mysql server-logs download](/cli/azure/mysql/server-logs#az-mysql-server-logs-download) können Sie einzelne Protokolldateien für Ihren Server herunterladen. 

Laden Sie dem folgenden Beispiel entsprechend die spezifische Protokolldatei für den Server **mydemoserver.mysql.database.azure.com** in der Ressourcengruppe **myresourcegroup** in Ihre lokale Umgebung herunter.
```azurecli-interactive
az mysql server-logs download --name 20170414-mydemoserver-mysql.log --resource-group myresourcegroup --server mydemoserver
```

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie mehr über [Protokolle für langsame Abfragen in Azure Database for MySQL](concepts-server-logs.md).
