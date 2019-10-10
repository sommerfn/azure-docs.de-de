---
title: Verwenden integrierter Notebookbefehle und -features in Azure Cosmos DB
description: Erfahren Sie, wie Sie integrierte Befehle und Features verwenden, um allgemeine Vorgänge unter Verwendung der in Azure Cosmos DB integrierten Notebooks auszuführen.
author: deborahc
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/23/2019
ms.author: dech
ms.openlocfilehash: 4a9bd554e0858024d656dbf35d6fb00995e6f4bd
ms.sourcegitcommit: 2d9a9079dd0a701b4bbe7289e8126a167cfcb450
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/29/2019
ms.locfileid: "71672487"
---
# <a name="use-built-in-notebook-commands-and-features-in-azure-cosmos-db"></a>Verwenden integrierter Notebookbefehle und -features in Azure Cosmos DB

Integrierte Jupyter Notebooks in Azure Cosmos DB ermöglichen es Ihnen, Ihre Daten über das Azure-Portal zu analysieren und zu visualisieren. In diesem Artikel wird beschrieben, wie Sie integrierte Notebookbefehle und -features verwenden, um allgemeine Vorgänge auszuführen.

## <a name="install-a-new-package"></a>Installieren eines neuen Pakets
Nachdem Sie die Notebookunterstützung für Ihre Azure Cosmos-Konten aktiviert haben, können Sie ein neues Notebook öffnen und ein Paket installieren.

Fügen Sie in einer neuen Codezelle den folgenden Code ein, und führen Sie ihn aus. Ersetzen Sie dabei ``PackageToBeInstalled`` durch das gewünschte Python-Paket.
```python
import sys
!{sys.executable} -m pip install PackageToBeInstalled –user
```
Dieses Paket kann von einem beliebigen Notebook im Arbeitsbereich des Azure Cosmos-Kontos verwendet werden. 

> [!TIP]
> Wenn für Ihr Notebook ein benutzerdefiniertes Paket erforderlich ist, wird empfohlen, im Notebook eine Zelle hinzuzufügen, um das Paket zu installieren, da Pakete entfernt werden, wenn Sie den [Arbeitsbereich zurücksetzen](#reset-notebooks-workspace).  

## <a name="run-a-sql-query"></a>Ausführen einer SQL-Abfrage

Sie können den Magic-Befehl ``%%sql`` verwenden, um eine [SQL-Abfrage](sql-query-getting-started.md) für einen beliebigen Container in Ihrem Konto auszuführen. Verwenden Sie die folgende Syntax:

```bash
%%sql --database {database_id} --container {container_id}
{Query text}
```

- Ersetzen Sie ``{database_id}`` und ``{container_id}`` durch den Namen der Datenbank und des Containers in Ihrem Cosmos-Konto. Wenn die Argumente ``--database`` und ``--container`` nicht angegeben werden, wird die Abfrage für die [Standarddatenbank und den Standardcontainer](#set-default-database-for-queries) ausgeführt.
- Sie können eine beliebige, in Azure Cosmos DB gültige SQL-Abfrage ausführen. Der Abfragetext muss in einer neuen Zeile enthalten sein.

Beispiel: 
```bash
%%sql --database RetailDemo --container WebsiteData
SELECT c.Action, c.Price as ItemRevenue, c.Country, c.Item FROM c
```
Führen Sie ```%%sql?``` in einer Zelle aus, um die Hilfedokumentation zum Magic-SQL-Befehl im Notebook anzuzeigen.

## <a name="run-a-sql-query-and-output-to-a-pandas-dataframe"></a>Ausführen einer SQL-Abfrage und Ausgeben des Ergebnisses in einen Pandas DataFrame

Sie können die Ergebnisse einer ``%%sql``-Abfrage in einen [Pandas DataFrame](https://pandas.pydata.org/pandas-docs/stable/getting_started/dsintro.html#dataframe) ausgeben. Verwenden Sie die folgende Syntax: 

```bash
%%sql --database {database_id} --container {container_id} --output {outputDataFrameVar}
{Query text}
```
- Ersetzen Sie ``{database_id}`` und ``{container_id}`` durch den Namen der Datenbank und des Containers in Ihrem Cosmos-Konto. Wenn die Argumente ``--database`` und ``--container`` nicht angegeben werden, wird die Abfrage für die [Standarddatenbank und den Standardcontainer](#set-default-database-for-queries) ausgeführt.
- Ersetzen Sie ``{outputDataFrameVar}`` durch den Namen der DataFrame-Variablen, die die Ergebnisse enthalten soll.
- Sie können eine beliebige, in Azure Cosmos DB gültige SQL-Abfrage ausführen. Der Abfragetext muss in einer neuen Zeile enthalten sein. 

Beispiel:

```bash
%%sql --database RetailDemo --container WebsiteData --output df_cosmos
SELECT c.Action, c.Price as ItemRevenue, c.Country, c.Item FROM c
```
```bash
df_cosmos.head(10)

    Action  ItemRevenue Country Item
0   Viewed  9.00    Tunisia Black Tee
1   Viewed  19.99   Antigua and Barbuda Flannel Shirt
2   Added   3.75    Guinea-Bissau   Socks
3   Viewed  3.75    Guinea-Bissau   Socks
4   Viewed  55.00   Czech Republic  Rainjacket
5   Viewed  350.00  Iceland Cosmos T-shirt
6   Added   19.99   Syrian Arab Republic    Button-Up Shirt
7   Viewed  19.99   Syrian Arab Republic    Button-Up Shirt
8   Viewed  33.00   Tuvalu  Red Top
9   Viewed  14.00   Cape Verde  Flip Flop Shoes
```

## <a name="set-default-database-for-queries"></a>Festlegen der Standarddatenbank für Abfragen
Sie können die Standarddatenbank festlegen, die von ```%%sql```-Befehlen für das Notebook verwendet wird. Ersetzen Sie ```{database_id}``` durch den Namen Ihrer Datenbank.

```bash
%database {database_id}
```
Führen Sie ```%database?``` in einer Zelle aus, um die Dokumentation im Notebook anzuzeigen.

## <a name="set-default-container-for-queries"></a>Festlegen des Standardcontainers für Abfragen
Sie können den Standardcontainer festlegen, der von ```%%sql```-Befehlen für das Notebook verwendet wird. Ersetzen Sie ```{container_id}``` durch den Namen Ihres Containers.

```bash
%container {container_id}
```
Führen Sie ```%container?``` in einer Zelle aus, um die Dokumentation im Notebook anzuzeigen.

## <a name="use-the-built-in-python-sdk"></a>Verwenden des integrierten Python SDKs
In der Notebookumgebung für das Cosmos-Konto ist Version 4 des [Azure Cosmos DB Python SDKs für die SQL-API](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cosmos/azure-cosmos) installiert und enthalten.

Verwenden Sie die integrierte ``cosmos_client``-Instanz, um SDK-Vorgänge auszuführen. 

Beispiel:

```python
## Import modules as needed
from azure.cosmos.partition_key import PartitionKey

## Create a new database if it doesn't exist
database = cosmos_client.create_database_if_not_exists('RetailDemo')

## Create a new container if it doesn't exist
container = database.create_container_if_not_exists(id='WebsiteData', partition_key=PartitionKey(path='/CartID'))
```
Siehe [Python SDK-Beispiele](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cosmos/azure-cosmos/samples). 

> [!IMPORTANT]
> Das integrierte Python SDK wird nur für SQL (Core)-API-Konten unterstützt. Bei anderen APIs müssen Sie [den relevanten Python-Treiber installieren](#install-a-new-package), der der API entspricht. 

## <a name="create-a-custom-instance-of-cosmos_client"></a>Erstellen einer benutzerdefinierten Instanz von ``cosmos_client``
Sie können eine benutzerdefinierte Instanz von ``cosmos_client`` erstellen, um mehr Flexibilität zu erhalten und:

- die [Verbindungsrichtlinie](https://docs.microsoft.com/python/api/azure-cosmos/azure.cosmos.documents.connectionpolicy?view=azure-python-preview) anzupassen.
- Vorgänge für ein anderes Cosmos-Konto als das auszuführen, das Sie gerade verwenden.

Auf die Verbindungszeichenfolge und den Primärschlüssel des aktuellen Kontos können Sie über die [Umgebungsvariablen](#access-the-account-endpoint-and-primary-key-env-variables) zugreifen. 

```python
import os
import azure.cosmos.cosmos_client as cosmos
import azure.cosmos.documents as documents

# These should be set to a region you've added for Cosmos DB
region_1 = "Central US" 
region_2 = "East US 2"

custom_connection_policy = documents.ConnectionPolicy()
custom_connection_policy.PreferredLocations = [region_1, region_2] # Set the order of regions the SDK will route requests to. The regions should be regions you've added for Cosmos, otherwise this will error.

# Create a new instance of CosmosClient, getting the endpoint and key from the environment variables
custom_client = cosmos.CosmosClient(os.environ["COSMOS_ENDPOINT"], {'masterKey': os.environ["COSMOS_KEY"]}, connection_policy=custom_connection_policy)
```
## <a name="access-the-account-endpoint-and-primary-key-env-variables"></a>Zugreifen auf die Umgebungsvariablen für den Kontoendpunkt und Primärschlüssel
```python
import os

endpoint = os.environ["COSMOS_ENDPOINT"]
primary_key = os.environ["COSMOS_KEY"]
```
> [!IMPORTANT]
> Die Umgebungsvariablen ``COSMOS_ENDPOINT`` und ``COSMOS_KEY`` gelten nur für die SQL-API. Bei anderen APIs finden Sie den Endpunkt und den Schlüssel auf dem Blatt **Verbindungszeichenfolgen** oder **Schlüssel** in Ihrem Cosmos-Konto.  

## <a name="reset-notebooks-workspace"></a>Zurücksetzen des Notebookarbeitsbereichs
Wählen Sie in der Befehlsleiste den Befehl zum **Zurücksetzen des Arbeitsbereichs** aus, um den Notebookarbeitsbereich auf die Standardeinstellungen zurückzusetzen. Dadurch werden alle installierten benutzerdefinierten Pakete entfernt und der Jupyter-Server neu gestartet. Ihre Notebooks, Dateien und Cosmos-Ressourcen sind nicht betroffen.  

![Zurücksetzen des Notebookarbeitsbereichs](media/use-notebook-features-and-commands/reset-workspace.png)

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu den Vorteilen von [Jupyter Notebooks in Azure Cosmos DB](cosmosdb-jupyter-notebooks.md)
- Weitere Informationen zum [Azure Cosmos DB Python SDK für die SQL-API](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cosmos/azure-cosmos)
