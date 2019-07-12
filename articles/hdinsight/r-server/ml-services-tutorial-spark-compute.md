---
title: 'Tutorial: Verwenden von R in einem Spark-Computekontext in Azure HDInsight'
description: 'Tutorial: Erste Schritte mit R und Spark in ML-Diensten.'
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: tutorial
ms.date: 06/21/2019
ms.openlocfilehash: 244c62467f187417bbb9f0e54173aad5a7d26d0a
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2019
ms.locfileid: "67451741"
---
# <a name="tutorial-use-r-in-a-spark-compute-context-in-azure-hdinsight"></a>Tutorial: Verwenden von R in einem Spark-Computekontext in Azure HDInsight

Dieses Tutorial bietet eine schrittweise Einführung in die Verwendung der R-Funktionen in Apache Spark bei der Ausführung auf einem ML Services-Cluster in Azure HDInsight.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Herunterladen von Beispieldaten in den lokalen Speicher
> * Kopieren von Daten in den Standardspeicher
> * Einrichten eines Datasets
> * Erstellen der Datenquelle
> * Erstellen von Computekontext für Spark
> * Anpassen eines linearen Modells
> * Verwenden von XDF-Verbunddateien
> * Konvertieren von XDF in CSV

## <a name="prerequisites"></a>Voraussetzungen

* Ein ML Services-Cluster in HDInsight. Siehe [Erstellen von Apache Hadoop-Clustern im Azure-Portal](../hdinsight-hadoop-create-linux-clusters-portal.md), und wählen Sie **ML Services** für **Clustertyp** aus.

## <a name="connect-to-rstudio-server"></a>Herstellen einer Verbindung mit RStudio Server

RStudio Server wird auf dem Edgeknoten des Clusters ausgeführt. Navigieren Sie zur folgenden URL, wobei `CLUSTERNAME` der Name des von Ihnen erstellten ML Services-Clusters ist:

```
https://CLUSTERNAME.azurehdinsight.net/rstudio/
```

Bei der ersten Anmeldung müssen Sie sich zweimal authentifizieren. Geben Sie für die erste Authentifizierungseingabeaufforderung die Benutzer-ID und das Kennwort für den Clusteradministrator an. Der Standardwert ist `admin`. Geben Sie für die zweite Authentifizierungseingabeaufforderung die Benutzer-ID und das Kennwort für SSH an. Der Standardwert ist `sshuser`. Für nachfolgende Anmeldungen sind dann nur noch die SSH-Anmeldeinformationen erforderlich.

## <a name="download-sample-data"></a>Herunterladen von Beispieldaten

Das *Dataset „Airline 2012 On-Time“* besteht aus 12 CSV-Dateien mit Informationen über Ankunft und Abflug für alle kommerziellen Flüge innerhalb der USA für das Jahr 2012. Dies ist ein großes Dataset mit über sechs Millionen Beobachtungen.

1. Initialisieren Sie einige Umgebungsvariablen. Geben Sie den folgenden Code in die RStudio Server-Konsole ein:

    ```R
    bigDataDirRoot <- "/tutorial/data" # root directory on cluster default storage
    localDir <- "/tmp/AirOnTimeCSV2012" # directory on edge node
    remoteDir <- "https://packages.revolutionanalytics.com/datasets/AirOnTimeCSV2012" # location of data
    ```

    Die Variablen werden auf der rechten Seite des Bildschirms unter der Registerkarte **Umgebung** angezeigt.

    ![RStudio](./media/ml-services-tutorial-spark-compute/rstudio.png)

2.  Erstellen Sie ein lokales Verzeichnis, und laden Sie Beispieldaten herunter. Geben Sie den folgenden Code in RStudio ein:

    ```R
    # Create local directory
    dir.create(localDir)
    
    # Download data to the tmp folder(local)
    download.file(file.path(remoteDir, "airOT201201.csv"), file.path(localDir, "airOT201201.csv"))
    download.file(file.path(remoteDir, "airOT201202.csv"), file.path(localDir, "airOT201202.csv"))
    download.file(file.path(remoteDir, "airOT201203.csv"), file.path(localDir, "airOT201203.csv"))
    download.file(file.path(remoteDir, "airOT201204.csv"), file.path(localDir, "airOT201204.csv"))
    download.file(file.path(remoteDir, "airOT201205.csv"), file.path(localDir, "airOT201205.csv"))
    download.file(file.path(remoteDir, "airOT201206.csv"), file.path(localDir, "airOT201206.csv"))
    download.file(file.path(remoteDir, "airOT201207.csv"), file.path(localDir, "airOT201207.csv"))
    download.file(file.path(remoteDir, "airOT201208.csv"), file.path(localDir, "airOT201208.csv"))
    download.file(file.path(remoteDir, "airOT201209.csv"), file.path(localDir, "airOT201209.csv"))
    download.file(file.path(remoteDir, "airOT201210.csv"), file.path(localDir, "airOT201210.csv"))
    download.file(file.path(remoteDir, "airOT201211.csv"), file.path(localDir, "airOT201211.csv"))
    download.file(file.path(remoteDir, "airOT201212.csv"), file.path(localDir, "airOT201212.csv"))
    ```

    Der Download sollte in ca. 9,5 Minuten abgeschlossen sein.

## <a name="copy-data-to-default-storage"></a>Kopieren von Daten in den Standardspeicher

Der HDFS-Speicherort wird mit der `airDataDir`-Variablen angegeben. Geben Sie den folgenden Code in RStudio ein:

```R
# Set directory in bigDataDirRoot to load the data into
airDataDir <- file.path(bigDataDirRoot,"AirOnTimeCSV2012")

# Create directory (default storage)
rxHadoopMakeDir(airDataDir)

# Copy data from local storage to default storage
rxHadoopCopyFromLocal(localDir, bigDataDirRoot)
    
# Optional. Verify files
rxHadoopListFiles(airDataDir)
```

Der Schritt sollte in etwa 10 Sekunden abgeschlossen sein.

## <a name="set-up-data-set"></a>Einrichten eines Datasets

1. Erstellen Sie ein Dateisystemobjekt, das die Standardwerte verwendet. Geben Sie den folgenden Code in RStudio ein:

    ```R
    # Define the HDFS (WASB) file system
    hdfsFS <- RxHdfsFileSystem()
    ```

2. Die ursprünglichen CSV-Dateien haben ziemlich sperrige Variablennamen, daher stellen wir eine `colInfo`-Liste, damit sie besser zu verwalten sind. Geben Sie den folgenden Code in RStudio ein:

    ```R
    airlineColInfo <- list(
         MONTH = list(newName = "Month", type = "integer"),
        DAY_OF_WEEK = list(newName = "DayOfWeek", type = "factor",
            levels = as.character(1:7),
            newLevels = c("Mon", "Tues", "Wed", "Thur", "Fri", "Sat",
                          "Sun")),
        UNIQUE_CARRIER = list(newName = "UniqueCarrier", type =
                                "factor"),
        ORIGIN = list(newName = "Origin", type = "factor"),
        DEST = list(newName = "Dest", type = "factor"),
        CRS_DEP_TIME = list(newName = "CRSDepTime", type = "integer"),
        DEP_TIME = list(newName = "DepTime", type = "integer"),
        DEP_DELAY = list(newName = "DepDelay", type = "integer"),
        DEP_DELAY_NEW = list(newName = "DepDelayMinutes", type =
                             "integer"),
        DEP_DEL15 = list(newName = "DepDel15", type = "logical"),
        DEP_DELAY_GROUP = list(newName = "DepDelayGroups", type =
                               "factor",
           levels = as.character(-2:12),
           newLevels = c("< -15", "-15 to -1","0 to 14", "15 to 29",
                         "30 to 44", "45 to 59", "60 to 74",
                         "75 to 89", "90 to 104", "105 to 119",
                         "120 to 134", "135 to 149", "150 to 164",
                         "165 to 179", ">= 180")),
        ARR_DELAY = list(newName = "ArrDelay", type = "integer"),
        ARR_DELAY_NEW = list(newName = "ArrDelayMinutes", type =
                             "integer"),  
        ARR_DEL15 = list(newName = "ArrDel15", type = "logical"),
        AIR_TIME = list(newName = "AirTime", type =  "integer"),
        DISTANCE = list(newName = "Distance", type = "integer"),
        DISTANCE_GROUP = list(newName = "DistanceGroup", type =
                             "factor",
         levels = as.character(1:11),
         newLevels = c("< 250", "250-499", "500-749", "750-999",
             "1000-1249", "1250-1499", "1500-1749", "1750-1999",
             "2000-2249", "2250-2499", ">= 2500")))
    
    varNames <- names(airlineColInfo)
    ```

## <a name="create-data-source"></a>Erstellen der Datenquelle

In einem Spark-Computekontext können Sie Datenquellen mit den folgenden Funktionen erstellen:

|Funktion | BESCHREIBUNG |
|---------|-------------|
|`RxTextData` | Eine durch Trennzeichen getrennte Textdatenquelle. |
|`RxXdfData` | Daten im XDF-Datendateiformat. In RevoScaleR wird das XDF-Dateiformat so geändert, dass Hadoop Daten in einem Verbundsatz von Dateien anstatt in einer einzigen Datei speichert. |
|`RxHiveData` | Generiert ein Hive-Datenquellenobjekt.|
|`RxParquetData` | Generiert ein Parquet-Datenquellenobjekt.|
|`RxOrcData` | Generiert ein Orc-Datenquellenobjekt.|

Erstellen Sie mit den Dateien, die Sie in HDFS kopiert haben, ein [RxTextData](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxtextdata)-Objekt. Geben Sie den folgenden Code in RStudio ein:

```R
airDS <- RxTextData( airDataDir,
                        colInfo = airlineColInfo,
                        varsToKeep = varNames,
                        fileSystem = hdfsFS ) 
```

## <a name="create-compute-context-for-spark"></a>Erstellen von Computekontext für Spark

Zum Laden von Daten und Ausführen von Analysen auf Workerknoten legen Sie den Computekontext in Ihrem Skript auf [RxSpark](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxspark) fest. In diesem Kontext verteilen R-Funktionen die Arbeitsauslastung automatisch auf alle Workerknoten, ohne dass eine integrierte Anforderung für die Verwaltung Einzelvorgängen oder der Warteschlange besteht. Der Spark-Computekontext wird durch `RxSpark` oder `rxSparkConnect()` eingerichtet, um den Spark-Computekontext zu erstellen, und verwendet `rxSparkDisconnect()`, um zu einem lokalen Computekontext zurückzukehren. Geben Sie den folgenden Code in RStudio ein:

```R
# Define the Spark compute context
mySparkCluster <- RxSpark()

# Set the compute context
rxSetComputeContext(mySparkCluster)
```

## <a name="fit-a-linear-model"></a>Anpassen eines linearen Modells

1. Verwenden Sie die [RxLinMod](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxlinmod)-Funktion, um mithilfe Ihrer `airDS`-Datenquelle ein lineares Modell anzupassen. Geben Sie den folgenden Code in RStudio ein:

    ```R
    system.time(
         delayArr <- rxLinMod(ArrDelay ~ DayOfWeek, data = airDS,
              cube = TRUE)
    )
    ```
    
    Dieser Schritt sollte zwischen 2 und 3 Minuten dauern.

1. Zeigen Sie die Ergebnisse an. Geben Sie den folgenden Code in RStudio ein:

    ```R
    summary(delayArr)
    ```

    Die Ergebnisse sollten wie folgt aussehen:

    ```output
    Call:
    rxLinMod(formula = ArrDelay ~ DayOfWeek, data = airDS, cube = TRUE)
    
    Cube Linear Regression Results for: ArrDelay ~ DayOfWeek
    Data: airDataXdf (RxXdfData Data Source)
    File name: /tutorial/data/AirOnTimeCSV2012
    Dependent variable(s): ArrDelay
    Total independent variables: 7 
    Number of valid observations: 6005381
    Number of missing observations: 91381 
     
    Coefficients:
                   Estimate Std. Error t value Pr(>|t|)     | Counts
    DayOfWeek=Mon   3.54210    0.03736   94.80 2.22e-16 *** | 901592
    DayOfWeek=Tues  1.80696    0.03835   47.12 2.22e-16 *** | 855805
    DayOfWeek=Wed   2.19424    0.03807   57.64 2.22e-16 *** | 868505
    DayOfWeek=Thur  4.65502    0.03757  123.90 2.22e-16 *** | 891674
    DayOfWeek=Fri   5.64402    0.03747  150.62 2.22e-16 *** | 896495
    DayOfWeek=Sat   0.91008    0.04144   21.96 2.22e-16 *** | 732944
    DayOfWeek=Sun   2.82780    0.03829   73.84 2.22e-16 *** | 858366
    ---
    Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
    
    Residual standard error: 35.48 on 6005374 degrees of freedom
    Multiple R-squared: 0.001827 (as if intercept included)
    Adjusted R-squared: 0.001826 
    F-statistic:  1832 on 6 and 6005374 DF,  p-value: < 2.2e-16 
    Condition number: 1 
    ```

    Beachten Sie, dass die Ergebnisse zeigen, dass wir alle Daten – sechs Millionen Beobachtungen – mit allen CSV-Dateien im angegebenen Verzeichnis verarbeitet haben. Beachten Sie außerdem, dass wir einen geschätzten Koeffizienten für jeden Tag der Woche (und nicht für das konstante Glied) haben, da wir `cube = TRUE` angegeben haben.

## <a name="use-composite-xdf-files"></a>Verwenden von XDF-Verbunddateien

Wie wir gesehen haben, können Sie CSV-Dateien direkt mit R in Hadoop analysieren, aber die Analyse kann schneller durchgeführt werden, wenn die Daten in einem effizienteren Format gespeichert werden. Das R-Format XDF ist äußerst effizient, wird aber für HDFS etwas verändert, so dass einzelne Dateien innerhalb eines einzigen HDFS-Blocks bleiben. (Die HDFS-Blockgröße variiert von Installation zu Installation, beträgt aber in der Regel 64 MB oder 128 MB.) Wenn Sie [rxImport](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rximport) in Hadoop verwenden, geben Sie eine `RxTextData`-Datenquelle wie `AirDS` als inData-Argument und eine `RxXdfData`-Datenquelle, bei der „fileSystem“ auf ein HDFS-Dateisystem festgelegt ist, als outFile-Argument an, um einen Verbundsatz von XDF-Dateien zu erstellen. Das `RxXdfData`-Objekt kann dann in nachfolgenden R-Analysen als Datenargument verwendet werden.

1. Definieren Sie ein `RxXdfData`-Objekt. Geben Sie den folgenden Code in RStudio ein:

    ```R
    airDataXdfDir <- file.path(bigDataDirRoot,"AirOnTimeXDF2012")
    
    airDataXdf <- RxXdfData( airDataXdfDir,
                            fileSystem = hdfsFS )
    ```

1. Legen Sie eine Blockgröße von 250.000 Zeilen fest, und geben Sie an, dass wir alle Daten gelesen haben. Geben Sie den folgenden Code in RStudio ein:

    ```R
    blockSize <- 250000
    numRowsToRead = -1
    ```

1. Importieren Sie die Daten mithilfe von `rxImport`. Geben Sie den folgenden Code in RStudio ein:

    ```R
    rxImport(inData = airDS,
             outFile = airDataXdf,
             rowsPerRead = blockSize,
             overwrite = TRUE,
             numRows = numRowsToRead )
    ```
    
    Dieser Schritt sollte nach einigen Minuten abgeschlossen sein.

1. Schätzen Sie das gleiche lineare Modell unter Verwendung der neuen, schnelleren Datenquelle neu. Geben Sie den folgenden Code in RStudio ein:

    ```R
    system.time(
         delayArr <- rxLinMod(ArrDelay ~ DayOfWeek, data = airDataXdf,
              cube = TRUE)
    )
    ```
    
    Der Schritt sollte in weniger als einer Minute abgeschlossen sein.

1. Zeigen Sie die Ergebnisse an. Die Ergebnisse sollten die gleichen sein wie bei den CSV-Dateien. Geben Sie den folgenden Code in RStudio ein:

    ```R
    summary(delayArr)
    ```

## <a name="convert-xdf-to-csv"></a>Konvertieren von XDF in CSV

### <a name="in-a-spark-context"></a>In einem Spark-Kontext

Wenn Sie Ihre CSV-Datei in XDF konvertiert haben, um die Effizienz bei der Durchführung von Analysen zu nutzen, aber nun Ihre Daten wieder in CSV konvertieren möchten, können [rxDataStep](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxdatastep) dazu verwenden.

Zum Erstellen eines Ordners mit CSV-Dateien erstellen Sie zunächst ein `RxTextData`-Objekt mit einem Verzeichnisnamen als Dateiargument. Dies steht für den Ordner, in dem die CSV-Dateien erstellt werden. Dieses Verzeichnis wird bei der Ausführung von `rxDataStep` erstellt. Zeigen Sie dann im `outFile`-Argument von `rxDataStep` auf dieses `RxTextData`-Objekt. Jede erstellte CSV-Datei erhält den Verzeichnisnamen gefolgt von einer Nummer.

Angenommen, wir möchten einen Ordner mit CSV-Dateien aus unserer XDF-Verbunddatei `airDataXdf` in HDFS schreiben, nachdem wir die logistische Regression und Vorhersage durchgeführt haben, sodass die neuen CSV-Dateien die vorhergesagten Werte und Restwerte enthalten. Geben Sie den folgenden Code in RStudio ein:

```R
airDataCsvDir <- file.path(bigDataDirRoot,"AirDataCSV2012")
airDataCsvDS <- RxTextData(airDataCsvDir,fileSystem=hdfsFS)
rxDataStep(inData=airDataXdf, outFile=airDataCsvDS)
```

Dieser Schritt sollte in ca. 2,5 Minuten abgeschlossen sein.

Sie werden feststellen, dass `rxDataStep` für jede XDFD-Datei in der XDF-Verbundeingabedatei eine CSV-Datei ausgegeben hat. Dies ist das Standardverhalten für das Schreiben von CSV-Dateien aus XDF-Verbunddateien in HDFS, wenn der Computekontext auf `RxSpark` festgelegt ist.

### <a name="in-a-local-context"></a>In einem lokalen Kontext

Alternativ können Sie Ihren Computekontext nach Abschluss der Analysen wieder auf `local` zurücksetzen und zwei Argumente innerhalb von `RxTextData` nutzen, die Ihnen etwas mehr Kontrolle beim Schreiben von CSV-Dateien in HDFS bieten: `createFileSet` und `rowsPerOutFile` geben. Wenn `createFileSet` auf `TRUE` festgelegt ist, wird ein Ordner mit CSV-Dateien in das von Ihnen angegebene Verzeichnis geschrieben. Wenn `createFileSet` auf `FALSE` festgelegt ist, wird eine einzelne CSV-Datei geschrieben. Das zweite Argument `rowsPerOutFile` kann auf einen Integer festgelegt werden, um anzugeben, wie viele Zeilen in jede CSV-Datei geschrieben werden sollen, wenn `createFileSet` `TRUE` ist.

Geben Sie den folgenden Code in RStudio ein:

```R
rxSetComputeContext("local")
airDataCsvRowsDir <- file.path(bigDataDirRoot,"AirDataCSVRows2012")
airDataCsvRowsDS <- RxTextData(airDataCsvRowsDir, fileSystem=hdfsFS, createFileSet=TRUE, rowsPerOutFile=1000000)
rxDataStep(inData=airDataXdf, outFile=airDataCsvRowsDS)
```

Dieser Schritt sollte nach ca. 10 Minuten abgeschlossen sein.

Bei Verwendung eines `RxSpark`-Computekontexts ist `createFileSet` standardmäßig auf `TRUE` festgelegt, und `rowsPerOutFile` hat keine Auswirkung. Wenn Sie also eine einzelne CSV-Datei erstellen oder die Anzahl der Zeilen pro Datei anpassen möchten, müssen Sie `rxDataStep` in einem `local`-Computekontext ausführen (Daten können sich dennoch in HDFS befinden).

## <a name="final-steps"></a>Letzte Schritte

1. Bereinigen Sie Daten. Geben Sie den folgenden Code in RStudio ein:

    ```R
    rxHadoopRemoveDir(airDataDir)
    rxHadoopRemoveDir(airDataXdfDir)
    rxHadoopRemoveDir(airDataCsvDir)
    rxHadoopRemoveDir(airDataCsvRowsDir)
    rxHadoopRemoveDir(bigDataDirRoot)
    ```

1. Beenden Sie die Spark-Remoteanwendung. Geben Sie den folgenden Code in RStudio ein:

    ```R
    rxStopEngine(mySparkCluster)
    ```

1. Beenden Sie die R-Sitzung. Geben Sie den folgenden Code in RStudio ein:

    ```R
    quit()
    ```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Nach Abschluss des Tutorials kann es ratsam sein, den Cluster zu löschen. Mit HDInsight werden Ihre Daten im Azure-Speicher gespeichert, sodass Sie einen Cluster problemlos löschen können, wenn er nicht verwendet wird. Für einen HDInsight-Cluster fallen auch dann Gebühren an, wenn er nicht verwendet wird. Da die Gebühren für den Cluster erheblich höher sind als die Kosten für den Speicher, ist es sinnvoll, nicht verwendete Cluster zu löschen.

Informationen zum Löschen eines Clusters finden Sie unter [Löschen eines HDInsight-Clusters mit Ihrem Browser, PowerShell oder der Azure CLI](../hdinsight-delete-cluster.md).

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie gelernt, wie R-Funktionen in Apache Spark bei der Ausführung auf einem ML Services-Cluster in Azure HDInsight verwendet werden. Weitere Informationen finden Sie in den folgenden Artikeln:

* [Computekontextoptionen für ML Services in HDInsight](r-server-compute-contexts.md)
* [R-Funktionen für Spark in Hadoop](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/revoscaler-hadoop-functions)
