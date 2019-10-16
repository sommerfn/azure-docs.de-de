---
title: Erstellen einer IoT Hub-Datenverbindung für Azure Data Explorer mit Python
description: In diesem Artikel erfahren Sie, wie Sie eine IoT Hub-Datenverbindung für Azure Data Explorer mit Python erstellen.
author: lucygoldbergmicrosoft
ms.author: lugoldbe
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 10/07/2019
ms.openlocfilehash: 1e6cd94e18382b3e66fc6a9c284539bc178e199c
ms.sourcegitcommit: 11265f4ff9f8e727a0cbf2af20a8057f5923ccda
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/08/2019
ms.locfileid: "72031043"
---
# <a name="create-an-iot-hub-data-connection-for-azure-data-explorer-by-using-python-preview"></a>Erstellen einer IoT Hub-Datenverbindung für Azure Data Explorer mit Python (Vorschau)

> [!div class="op_single_selector"]
> * [Portal](ingest-data-iot-hub.md)
> * [C#](data-connection-iot-hub-csharp.md)
> * [Python](data-connection-iot-hub-python.md)

Azure-Daten-Explorer ist ein schneller und hochgradig skalierbarer Dienst zur Untersuchung von Daten (Protokoll- und Telemetriedaten). Azure Data Explorer bietet Erfassung (Laden von Daten) aus Event Hubs, IoT Hubs und Blobs, die in Blobcontainer geschrieben werden. In diesem Artikel erstellen Sie eine IoT Hub-Datenverbindung für Azure Data Explorer mit Python.

## <a name="prerequisites"></a>Voraussetzungen

* Wenn Sie über kein Azure-Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

* Erstellen eines [Clusters und einer Datenbank](create-cluster-database-csharp.md)

* Erstellen einer [Tabelle und Spaltenzuordnung](net-standard-ingest-data.md#create-a-table-on-your-test-cluster)

* Festlegen von [Datenbank- und Tabellenrichtlinien](database-table-policies-csharp.md) (optional)

* Erstellen Sie einen [IoT-Hub mit einer konfigurierten SAS-Richtlinie](ingest-data-iot-hub.md#create-an-iot-hub).

[!INCLUDE [data-explorer-data-connection-install-package-python](../../includes/data-explorer-data-connection-install-package-python.md)]

[!INCLUDE [data-explorer-authentication](../../includes/data-explorer-authentication.md)]

## <a name="add-an-iot-hub-data-connection"></a>Hinzufügen einer IoT Hub-Datenverbindung 

Im folgenden Beispiel wird gezeigt, wie eine IoT Hub-Datenverbindung programmgesteuert hinzugefügt wird. Weitere Informationen zum Hinzufügen einer IoT Hub-Datenverbindung mithilfe des Azure-Portals finden Sie unter [Herstellen einer Verbindung zwischen der Azure Data Explorer-Tabelle und IoT Hub](ingest-data-iot-hub.md#connect-azure-data-explorer-table-to-iot-hub).

```Python
from azure.mgmt.kusto import KustoManagementClient
from azure.mgmt.kusto.models import IotHubDataConnection
from azure.common.credentials import ServicePrincipalCredentials

#Directory (tenant) ID
tenant_id = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx"
#Application ID
client_id = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx"
#Client Secret
client_secret = "xxxxxxxxxxxxxx"
subscription_id = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx"
credentials = ServicePrincipalCredentials(
        client_id=client_id,
        secret=client_secret,
        tenant=tenant_id
    )
kusto_management_client = KustoManagementClient(credentials, subscription_id)

resource_group_name = "testrg";
#The cluster and database that are created as part of the Prerequisites
cluster_name = "mykustocluster";
database_name = "mykustodatabase";
data_connection_name = "myeventhubconnect";
#The IoT hub that is created as part of the Prerequisites
iot_hub_resource_id = "/subscriptions/xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx/resourceGroups/xxxxxx/providers/Microsoft.Devices/IotHubs/xxxxxx";
shared_access_policy_name = "iothubforread";
consumer_group = "$Default";
location = "Central US";
#The table and column mapping that are created as part of the Prerequisites
table_name = "StormEvents";
mapping_rule_name = "StormEvents_CSV_Mapping";
data_format = "csv";

#Returns an instance of LROPoller, check https://docs.microsoft.com/python/api/msrest/msrest.polling.lropoller?view=azure-python
poller = kusto_management_client.data_connections.create_or_update(resource_group_name=resource_group_name, cluster_name=cluster_name, database_name=database_name, data_connection_name=data_connection_name,
                                            parameters=IotHubDataConnection(iot_hub_resource_id=iot_hub_resource_id, shared_access_policy_name=shared_access_policy_name, 
                                                                                consumer_group=consumer_group, table_name=table_name, location=location, mapping_rule_name=mapping_rule_name, data_format=data_format))
```

|**Einstellung** | **Empfohlener Wert** | **Feldbeschreibung**|
|---|---|---|
| tenant_id | *xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx* | Ihre Mandanten-ID. Wird auch als Verzeichnis-ID bezeichnet.|
| subscriptionId | *xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx* | Die Abonnement-ID, die Sie für die Ressourcenerstellung verwenden.|
| client_id | *xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx* | Die Client-ID der Anwendung, die auf Ressourcen in Ihrem Mandanten zugreifen kann.|
| client_secret | *xxxxxxxxxxxxxx* | Das Clientgeheimnis der Anwendung, die auf Ressourcen in Ihrem Mandanten zugreifen kann. |
| resource_group_name | *testrg* | Der Name der Ressourcengruppe, die Ihren Cluster enthält.|
| cluster_name | *mykustocluster* | Der Name Ihres Clusters.|
| database_name | *mykustodatabase* | Der Name der Zieldatenbank in Ihrem Cluster.|
| data_connection_name | *myeventhubconnect* | Der gewünschte Name Ihrer Datenverbindung.|
| table_name | *StormEvents* | Der Name der Zieltabelle in der Zieldatenbank.|
| mapping_rule_name | *StormEvents_CSV_Mapping* | Der Name der Spaltenzuordnung, die mit der Zieltabelle verknüpft ist.|
| data_format | *csv* | Das Datenformat der Nachricht.|
| iot_hub_resource_id | *Ressourcen-ID* | Die Ressourcen-ID Ihres IoT-Hubs mit den Daten für die Erfassung.|
| shared_access_policy_name | *iothubforread* | Der Name der SAS-Richtlinie, die Berechtigungen für Geräte und Dienste zum Herstellen einer Verbindung mit IoT Hub definiert. |
| consumer_group | *$Default* | Die Consumergruppe Ihres Event Hubs.|
| location | *USA (Mitte)* | Der Speicherort der Datenverbindungsressource.|

[!INCLUDE [data-explorer-data-connection-clean-resources-python](../../includes/data-explorer-data-connection-clean-resources-python.md)]