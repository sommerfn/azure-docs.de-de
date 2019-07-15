---
title: Abfragen von Daten in Azure Data Lake mit Azure Data Explorer
description: Erfahren Sie, wie Sie Daten in Azure Data Lake mit Azure Data Explorer abfragen.
author: orspod
ms.author: orspodek
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: conceptual
ms.date: 06/25/2019
ms.openlocfilehash: d6a58d144482e17f7e4b615134115d1da46af6f0
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2019
ms.locfileid: "67454389"
---
# <a name="query-data-in-azure-data-lake-using-azure-data-explorer-preview"></a>Abfragen von Daten in Azure Data Lake mit Azure Data Explorer (Vorschau)

Azure Data Lake Storage ist eine hochgradig skalierbare und kostengünstige Data Lake-Lösung für Big Data-Analysen. Dank des leistungsstarken Dateisystems kombiniert mit immenser Skalierbarkeit und Profitabilität können Sie in kurzer Zeit aufschlussreiche Erkenntnisse gewinnen. Data Lake Storage Gen2 erweitert die Funktionen von Azure Blob Storage und ist für Analyseworkloads optimiert.
 
Azure Data Explorer ist in Azure Blob Storage und Azure Data Lake Storage Gen2 integriert und bietet schnellen, zwischengespeicherten und indizieren Zugriff auf Daten im Lake. Sie können Daten im Lake analysieren und abfragen, ohne sie vorher in Azure Data Explorer erfassen zu müssen. Sie können auch erfasste und nicht erfasste native Lake-Daten gleichzeitig abfragen.  

> [!TIP]
> Zur Erzielung der besten Abfrageleistung ist die Datenerfassung in Azure Data Explorer erforderlich. Die Möglichkeit des Abfragens von Daten in Azure Data Lake Storage Gen2 ohne vorherige Erfassung sollte nur für Verlaufsdaten oder für Daten genutzt werden, die nur selten abgefragt werden.
 
## <a name="optimize-query-performance-in-the-lake"></a>Optimieren der Abfrageleistung im Lake 

* Partitionieren Sie Daten, um die Leistung zu verbessern und die Ausführungszeit für Abfragen zu optimieren.
* Komprimieren Sie Daten, um die Leistung zu verbessern (mit gzip erzielen Sie die beste Komprimierung, mit LZ4 die beste Leistung).
* Verwenden Sie für Azure Blob Storage oder Azure Data Lake Storage Gen2 die gleiche Region wie für Ihren Azure Data Explorer-Cluster. 

## <a name="create-an-external-table"></a>Erstellen einer externen Tabelle

1. Verwenden Sie den Befehl `.create external table`, um eine externe Tabelle in Azure Data Explorer zu erstellen. Weitere Befehle für externe Tabellen wie `.show`, `.drop` und `.alter` sind unter [Befehle für externe Tabellen](/azure/kusto/management/externaltables) dokumentiert.

    ```Kusto
    .create external table ArchivedProducts(
    Timestamp:datetime,
    ProductId:long, ProductDescription:string) 
    kind=blob
    partition by bin(Timestamp, 1d) 
    dataformat=csv (h@'http://storageaccount.blob.core.windows.net/container1;secretKey') 
    with (compressed = true)  
    ```

    Diese Abfrage erstellt tägliche Partitionen (*container1/yyyy/MM/dd/all_exported_blobs.csv*). Durch eine differenziertere Partitionierung können Sie eine höhere Leistung erwarten. Beispielsweise verzeichnen Abfragen über externe Tabellen mit täglichen Partitionen (wie die oben dargestellte Abfrage) eine bessere Leistung als Abfragen mit monatlich partitionierten Tabellen.

    > [!NOTE]
    > Derzeit werden die Speicherkonten Azure Blob Storage und Azure Data Lake Storage Gen2 unterstützt. Die derzeit unterstützten Datenformate lauten CSV, TSV und TXT.

1. Die externe Tabelle ist im linken Bereich der Webbenutzeroberfläche sichtbar.

    ![Externe Tabelle auf der Webbenutzeroberfläche](media/data-lake-query-data/external-tables-web-ui.png)
 
### <a name="external-table-permissions"></a>Berechtigungen für externe Tabellen
 
* Der Datenbankbenutzer kann eine externe Tabelle erstellen. Der Ersteller der Tabelle wird automatisch zum Tabellenadministrator.
* Der Cluster-, Datenbank- oder Tabellenadministrator kann eine vorhandene Tabelle bearbeiten.
* Alle Datenbankbenutzer oder -leser können eine externe Tabelle abfragen.
 
## <a name="query-an-external-table"></a>Abfragen einer externen Tabelle
 
Wenn Sie eine externe Tabelle abfragen möchten, verwenden Sie die Funktion `external_table()`, und geben Sie den Tabellennamen als Funktionsargument an. Bei dem Rest der Abfrage handelt es sich um die Kusto-Standardabfragesprache.

```Kusto
external_table("ArchivedProducts") | take 100
```

> [!TIP]
> IntelliSense wird derzeit in Abfragen für externe Tabellen nicht unterstützt.

## <a name="query-external-and-ingested-data-together"></a>Gemeinsames Abfragen von externen und erfassten Daten

Externe Tabellen und Tabellen mit erfassten Daten können in derselben Abfrage abgefragt werden. Mit [`join`](/azure/kusto/query/joinoperator) oder [`union`](/azure/kusto/query/unionoperator) können Sie die externe Tabelle mit zusätzlichen Daten aus Azure Data Explorer, SQL Server-Instanzen oder anderen Quellen verknüpfen. Verwenden Sie eine [`let( ) statement`](/azure/kusto/query/letstatement), um einem externen Tabellenverweis einen Kurznamen zuzuweisen.

Im folgenden Beispiel ist *Products* eine Tabelle mit erfassten Daten, und *ArchivedProducts* ist eine externe Tabelle, die Daten aus Azure Data Lake Storage Gen2 enthält:

```kusto
let T1 = external_table("ArchivedProducts") |  where TimeStamp > ago(100d);
let T = Products; //T is an internal table
T1 | join T on ProductId | take 10
```

## <a name="query-taxirides-external-table-in-the-help-cluster"></a>Abfragen der externen Tabelle *TaxiRides* im Cluster „help“

Das Beispiel-DataSet *TaxiRides* enthält New Yorker Taxidaten der [New York City Taxi and Limousine Commission (TLC)](https://www1.nyc.gov/site/tlc/about/tlc-trip-record-data.page).

### <a name="create-external-table-taxirides"></a>Erstellen der externen Tabelle *TaxiRides* 

> [!NOTE]
> In diesem Abschnitt wird die zum Erstellen der externen Tabelle *TaxiRides* im Cluster *help* verwendete Abfrage beschrieben. Da diese Tabelle bereits erstellt wurde, können Sie diesen Abschnitt überspringen und zum Abschnitt [Abfragen von Daten der externen Tabelle *TaxiRides*](#query-taxirides-external-table-data) wechseln. 

1. Die folgende Abfrage wurde verwendet, um die externe Tabelle *TaxiRides* im Cluster „help“ zu erstellen. 

    ```kusto
    .create external table TaxiRides
    (
    trip_id: long,
    vendor_id: string, 
    pickup_datetime: datetime,
    dropoff_datetime: datetime,
    store_and_fwd_flag: string,
    rate_code_id: int,
    pickup_longitude: real,
    pickup_latitude: real,
    dropoff_longitude: real,
    dropoff_latitude: real,
    passenger_count: int,
    trip_distance: real,
    fare_amount: real,
    extra: real,
    mta_tax: real,
    tip_amount: real,
    tolls_amount: real,
    ehail_fee: real,
    improvement_surcharge: real,
    total_amount: real,
    payment_type: string,
    trip_type: int,
    pickup: string,
    dropoff: string,
    cab_type: string,
    precipitation: int,
    snow_depth: int,
    snowfall: int,
    max_temperature: int,
    min_temperature: int,
    average_wind_speed: int,
    pickup_nyct2010_gid: int,
    pickup_ctlabel: string,
    pickup_borocode: int,
    pickup_boroname: string,
    pickup_ct2010: string,
    pickup_boroct2010: string,
    pickup_cdeligibil: string,
    pickup_ntacode: string,
    pickup_ntaname: string,
    pickup_puma: string,
    dropoff_nyct2010_gid: int,
    dropoff_ctlabel: string,
    dropoff_borocode: int,
    dropoff_boroname: string,
    dropoff_ct2010: string,
    dropoff_boroct2010: string,
    dropoff_cdeligibil: string,
    dropoff_ntacode: string,
    dropoff_ntaname: string,
    dropoff_puma: string
    )
    kind=blob 
    partition by bin(pickup_datetime, 1d)
    dataformat=csv
    ( 
    h@'https://externalkustosamples.blob.core.windows.net/taxiridesbyday?st=2019-06-18T14%3A59%3A00Z&se=2029-06-19T14%3A59%3A00Z&sp=rl&sv=2016-05-31&sr=c&sig=yEaO%2BrzFHzAq7lvd4d9PeQ%2BTi3AWnho8Rn8hGU0X30M%3D'
    )
    ```
1. Die daraus resultierende Tabelle wurde im Cluster *help* erstellt:

    ![Externe Tabelle „TaxiRides“](media/data-lake-query-data/taxirides-external-table.png) 

### <a name="query-taxirides-external-table-data"></a>Abfragen von Daten der externen Tabelle *TaxiRides* 

Melden Sie sich bei [https://dataexplorer.azure.com/clusters/help/databases/Samples](https://dataexplorer.azure.com/clusters/help/databases/Samples) an, um die externe Tabelle *TaxiRides* abzufragen. 

#### <a name="query-taxirides-external-table-without-partitioning"></a>Abfragen der externen Tabelle *TaxiRides* ohne Partitionierung

[Führen Sie diese Abfrage](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAAx3LSwqAMAwFwL3gHYKreh1xL7F9YrCtElP84OEV9zM4DZo5DsZjhGt6PqWTgL1p6+qhvaTEKjeI/FqyuZbGiwJf63QAi9vEL2UbAhtMEv6jyAH6+VhS9jOr1dULfUgAm2cAAAA=) für die externe Tabelle *TaxiRides* aus, um die Fahrten für jeden Wochentag für das gesamte DataSet darzustellen. 

```kusto
external_table("TaxiRides")
| summarize count() by dayofweek(pickup_datetime)
| render columnchart
```

Diese Abfrage zeigt den Wochentag mit den meisten Fahrten. Weil die Daten nicht partitioniert sind, kann es lange (mehrere Minuten) dauern, bis diese Abfrage Ergebnisse zurückgibt.

![Rendern einer nicht partitionierten Abfrage](media/data-lake-query-data/taxirides-no-partition.png)

#### <a name="query-taxirides-external-table-with-partitioning"></a>Abfragen der externen Tabelle „TaxiRides“ mit Partitionierung 

[Führen Sie diese Abfrage](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAA13NQQqDMBQE0L3gHT6ukkVF3fQepXv5SQYMNWmIP6ilh68WuinM6jHMYBPkyPMobGao5s6bv3mHpdF19aZ1QgYlbx8ljY4F4gPIQFYgkvqJGrr+eun6I5ralv58OP27t5QQOPsXiOyzRFGazE6WzSh7wtnIiA75uISdOEtdfQDLWmP+ogAAAA==) für die externe Tabelle *TaxiRides* aus, um die im Januar 2017 genutzten Taxiarten (gelb oder grün) darzustellen. 

```kusto
external_table("TaxiRides")
| where pickup_datetime between (datetime(2017-01-01) .. datetime(2017-02-01))
| summarize count() by cab_type
| render piechart
```

Diese Abfrage verwendet Partitionierung, wodurch Ausführungszeit und Leistung optimiert werden. Die Abfrage filtert eine partitionierte Spalte („pickup_datetime“) und gibt die Ergebnisse in wenigen Sekunden zurück.

![Rendern einer partitionierten Abfrage](media/data-lake-query-data/taxirides-with-partition.png)
  
Sie können weitere Abfragen zur Ausführung für die externe Tabelle *TaxiRides* schreiben und weitere Erkenntnisse über die Daten gewinnen. 

## <a name="next-steps"></a>Nächste Schritte

Fragen Sie Ihre Daten in Azure Data Lake mit Azure Data Explorer ab. Erfahren Sie, wie Sie [Abfragen schreiben](write-queries.md) und weitere Erkenntnisse aus Ihren Daten ableiten.