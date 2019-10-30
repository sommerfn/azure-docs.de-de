---
title: 'Execute R Script: Modulreferenz'
titleSuffix: Azure Machine Learning service
description: Erfahren Sie, wie das Modul „Execute R Script“ in Azure Machine Learning Service zum Ausführen von R-Code verwendet wird.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: peterlu
ms.date: 06/01/2019
ms.openlocfilehash: 01d4e3a06b8c6a95374b9ee246864167e6d2ac85
ms.sourcegitcommit: e0e6663a2d6672a9d916d64d14d63633934d2952
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "72693767"
---
# <a name="execute-r-script"></a>Execute R Script

In diesem Artikel wird beschrieben, wie Sie das Modul **Execute R Script** verwenden, um R-Code in Ihrer Pipeline auf der grafischen Benutzeroberfläche auszuführen.

Mit R können Sie Aufgaben ausführen, die von vorhandenen Modulen aktuell nicht unterstützt werden, beispielsweise: 
- Erstellen benutzerdefinierter Datentransformationen
- Verwenden eigener Metriken zum Auswerten von Vorhersagen
- Entwickeln von Modellen mit Algorithmen, die nicht als eigenständige Module auf der grafischen Benutzeroberfläche implementiert sind

## <a name="r-version-support"></a>Unterstützte Versionen von R

Die grafische Benutzeroberfläche von Azure Machine Learning Service verwendet die CRAN-Distribution (Comprehensive R Archive Network) von R. Die aktuell verwendete Version ist CRAN 3.5.1.

## <a name="supported-r-packages"></a>Unterstützte R-Pakete

Die R-Umgebung ist mit mehr als 100 Paketen vorinstalliert. Eine vollständige Liste finden Sie im Abschnitt [Vorinstallierte R-Pakete](#pre-installed-r-packages).

Sie können auch jedem **Execute R Script**-Modul den folgenden Code hinzufügen, um die installierten Pakete anzuzeigen.

```R
azureml_main <- function(dataframe1, dataframe2){
  print("R script run.")
  dataframe1 <- data.frame(installed.packages())
  return(list(dataset1=dataframe1, dataset2=dataframe2))
}
```

## <a name="installing-r-packages"></a>Installieren von R-Paketen
Um zusätzliche R-Pakete zu installieren, verwenden Sie die `install.packages()`-Methode. Geben Sie unbedingt das CRAN-Repository an. Pakete werden für jedes **Execute R Script**-Modul installiert und nicht mit anderen **Execute R Script**-Modulen gemeinsam genutzt.

Dieses Beispiel zeigt die Installation von Zoo:
```R
# R version: 3.5.1
# The script MUST contain a function named azureml_main
# which is the entry point for this module.

# The entry point function can contain up to two input arguments:
#   Param<dataframe1>: a R DataFrame
#   Param<dataframe2>: a R DataFrame
azureml_main <- function(dataframe1, dataframe2){
  print("R script run.")
  
  if(!require(zoo)) install.packages("zoo",repos = "http://cran.us.r-project.org")
  library(zoo)
  # Return datasets as a Named List
  return(list(dataset1=dataframe1, dataset2=dataframe2))
}
```
 > [!NOTE]
  > Überprüfen Sie, ob das Paket bereits vorhanden ist, bevor Sie es installieren, um wiederholte Installationen zu vermeiden. Wie `  if(!require(zoo)) install.packages("zoo",repos = "http://cran.us.r-project.org")` im obigen Beispielcode. Eine wiederholte Installation kann zum Timeout der Webdienstanforderung führen.     

## <a name="how-to-configure-execute-r-script"></a>Konfigurieren von Execute R Script

Das Modul **Execute R Script** enthält R-Beispielcode, den Sie als Ausgangspunkt verwenden können. Stellen Sie zum Konfigurieren des Moduls **Execute R Script** die erforderlichen Eingaben und den auszuführenden Code bereit.

![R-Modul](media/module/execute-r-script.png)

Auf der grafischen Benutzeroberfläche gespeicherte Datasets werden automatisch in einen R-Datenrahmen konvertiert, wenn sie mit diesem Modul geladen werden.

1.  Fügen Sie Ihrer Pipeline das Modul **Execute R Script** hinzu.

  

1. Stellen Sie eine Verbindung mit allen Eingaben her, die vom Skript benötigt werden. Eingaben sind optional und können Daten sowie zusätzlichen R-Code enthalten.

    * **Dataset1**: Verweisen Sie auf die erste Eingabe als `dataframe1`. Das Eingangsdatenset muss das Format CSV, TSV oder ARFF haben, oder Sie können eine Verbindung mit einem Azure Machine Learning-Dataset herstellen.

    * **Dataset2**: Verweisen Sie auf die zweite Eingabe als `dataframe2`. Dieses Dataset muss auch das Format CSV, TSV oder ARFF haben oder ein Azure Machine Learning-Dataset sein.

    * **Script Bundle**: Die dritte Eingabe akzeptiert ZIP-Dateien. Die ZIP-Datei kann mehrere Dateien und Dateitypen enthalten.

1. Geben oder fügen Sie in das Textfeld **R script** (R-Skript) ein gültiges R-Skript ein.

    Um Ihnen den Einstieg zu erleichtern, ist das Textfeld **R Script** mit Beispielcode vorab ausgefüllt, den Sie bearbeiten oder ersetzen können.
    
```R
# R version: 3.5.1
# The script MUST contain a function named azureml_main
# which is the entry point for this module.

# The entry point function can contain up to two input arguments:
#   Param<dataframe1>: a R DataFrame
#   Param<dataframe2>: a R DataFrame
azureml_main <- function(dataframe1, dataframe2){
  print("R script run.")

  # If a zip file is connected to the third input port, it is
  # unzipped under "./Script Bundle". This directory is added
  # to sys.path.

  # Return datasets as a Named List
  return(list(dataset1=dataframe1, dataset2=dataframe2))
}
```

 * Das Skript muss eine Funktion mit dem Namen `azureml_main` als Einstiegspunkt für dieses Modul enthalten.

 * Die Einstiegspunktfunktion kann bis zu zwei Eingabeargumente enthalten: `Param<dataframe1>` und `Param<dataframe2>`
 
   > [!NOTE]
    > Auf die an das Modul **Execute R Script** übermittelten Daten wird als `dataframe1` und `dataframe2` verwiesen. Dies unterscheidet sich von Azure Machine Learning Studio (Studio-Referenzen `dataset1` und `dataset2`). Stellen Sie sicher, dass in Ihrem Skript ordnungsgemäß auf die Eingabedaten verwiesen wird.  
 
    > [!NOTE]
    >  Vorhandener R-Code kann kleinere Änderungen erfordern, um in einer Pipeline auf der grafischen Benutzeroberfläche ausgeführt zu werden. Beispielsweise müssen Eingabedaten, die Sie im CSV-Format bereitstellen, explizit in ein Dataset konvertiert werden, bevor Sie sie in Ihrem Code verwenden können. Daten- und Spaltentypen, die in der Sprache R verwendet werden, unterscheiden sich ebenfalls durch einige Besonderheiten von den Daten- und Spaltentypen, die auf der grafischen Benutzeroberfläche verwendet werden.

1.  **Random Seed** (Zufälliger Startwert): Geben Sie einen Wert ein, der in der R-Umgebung als zufälliger Startwert verwendet werden soll. Dieser Parameter entspricht dem Aufrufen von `set.seed(value)` im R-Code.  

1. Ausführen der Pipeline.  

## <a name="results"></a>Ergebnisse

Die **Execute R Script**-Module können mehrere Ausgaben zurückgeben, müssen aber als R-Datenrahmen bereitgestellt werden. Datenrahmen werden aus Kompatibilitätsgründen mit anderen Modulen automatisch in Datasets für grafische Benutzeroberflächen konvertiert.

Standardmeldungen und -fehler aus R werden im Protokoll des Moduls zurückgegeben.

## <a name="sample-scripts"></a>Beispielskripts

Es gibt viele Möglichkeiten, wie Sie Ihre Pipeline mithilfe eines benutzerdefinierten R-Skripts erweitern können.  Dieser Abschnitt enthält Beispielcode für allgemeine Aufgaben.


### <a name="add-r-script-as-an-input"></a>Hinzufügen eines R-Skripts als Eingabe

Das Modul **Execute R Script** unterstützt beliebige R-Skriptdateien als Eingaben. Dazu müssen sie als Teil der ZIP-Datei in Ihren Arbeitsbereich hochgeladen werden.

1. Um eine ZIP-Datei mit R-Code in Ihren Arbeitsbereich hochzuladen, klicken Sie auf **New** (Neu), dann auf **Dataset** und wählen anschließend **From local file** (Aus lokaler Datei) und die Option **Zip file** (ZIP-Datei) aus.  

1. Stellen Sie sicher, dass die ZIP-Datei in der Liste **Saved Datasets** (Gespeicherte Datasets) verfügbar ist.

1.  Verbinden Sie das Dataset mit dem Eingabeport **Script Bundle**.

1. Alle Dateien in der ZIP-Datei sind während der Laufzeit der Pipeline verfügbar. 

    Wenn die Script Bundle-Datei eine Verzeichnisstruktur enthält, bleibt die Struktur erhalten. Sie müssen jedoch Ihren Code so ändern, dass das Verzeichnis **./Script Bundle** dem Pfad vorangestellt wird.

### <a name="process-data"></a>Verarbeiten von Daten

Das folgende Beispiel zeigt, wie Eingabedaten skaliert und normalisiert werden:

```R
# R version: 3.5.1
# The script MUST contain a function named azureml_main
# which is the entry point for this module.
# The entry point function can contain up to two input arguments:
#   Param<dataframe1>: a R DataFrame
#   Param<dataframe2>: a R DataFrame
azureml_main <- function(dataframe1, dataframe2){
  print("R script run.")
  # If a zip file is connected to the third input port, it is
  # unzipped under "./Script Bundle". This directory is added
  # to sys.path.
  series <- dataframe1$width
  # find the maximum and minimum values of width column in dataframe1
  max_v <- max(series)
  min_v <- min(series)
  # calculate the scale and bias
  scale <- max_v - min_v
  bias <- min_v / dis
  # apply min-max normalizing
  dataframe1$width <- dataframe1$width / scale - bias
  dataframe2$width <- dataframe2$width / scale - bias
  # Return datasets as a Named List
  return(list(dataset1=dataframe1, dataset2=dataframe2))
}
 ```

### <a name="read-a-zip-file-as-input"></a>Lesen einer ZIP-Datei als Eingabe

Dieses Beispiel zeigt, wie Sie ein Dataset in einer ZIP-Datei als Eingabe für das Modul **Execute R Script** verwenden.

1. Erstellen Sie die Datendatei im CSV-Format, und nennen Sie sie „mydatafile.csv“.
1. Erstellen Sie eine ZIP-Datei, und fügen Sie die CSV-Datei dem Archiv hinzu.
1. Laden Sie die ZIP-Datei in Ihren Azure Machine Learning-Arbeitsbereich hoch. 
1. Verbinden Sie das resultierende Dataset mit der **ScriptBundle**-Eingabe Ihres Moduls **Execute R Script**.
1. Verwenden den folgenden Code, um die CSV-Daten aus der ZIP-Datei zu lesen.

```R
azureml_main <- function(dataframe1, dataframe2){
  print("R script run.")
  mydataset<-read.csv("./Script Bundle/mydatafile.csv",encoding="UTF-8");  
  # Return datasets as a Named List
  return(list(dataset1=mydataset, dataset2=dataframe2))
}
```

### <a name="replicate-rows"></a>Replizieren von Zeilen

Dieses Beispiel zeigt, wie positive Datensätze in einem Dataset repliziert werden können, um die Stichprobe auszugleichen:

```R
azureml_main <- function(dataframe1, dataframe2){
  data.set <- dataframe1[dataframe1[,1]==-1,]  
  # positions of the positive samples
  pos <- dataframe1[dataframe1[,1]==1,]
  # replicate the positive samples to balance the sample  
  for (i in 1:20) data.set <- rbind(data.set,pos)  
  row.names(data.set) <- NULL
  # Return datasets as a Named List
  return(list(dataset1=data.set, dataset2=dataframe2))
}
```

### <a name="pass-r-objects-between-execute-r-script-modules"></a>Übergeben von R-Objekten zwischen Execute R Script-Modulen

Sie können R-Objekte zwischen Instanzen des Moduls **Execute R Script** übergeben, indem Sie den internen Serialisierungsmechanismus verwenden. Bei diesem Beispiel wird davon ausgegangen, dass Sie das R-Objekt mit dem Namen `A` zwischen zwei **Execute R Script**-Modulen verschieben möchten.

1. Fügen Sie das erste Modul **Execute R Script** Ihrer Pipeline hinzu, und geben Sie den folgenden Code in das Textfeld **R Script** ein, um ein serialisiertes Objekt `A` als Spalte in der Ausgabedatentabelle des Moduls zu erstellen:  
  
    ```R
    azureml_main <- function(dataframe1, dataframe2){
      print("R script run.")
      # some codes generated A
      
      serialized <- as.integer(serialize(A,NULL))  
      data.set <- data.frame(serialized,stringsAsFactors=FALSE)

      return(list(dataset1=data.set, dataset2=dataframe2))
    }
    ```

    Die explizite Konvertierung in den Typ „Ganze Zahl“ erfolgt, da die Serialisierungsfunktion Daten im `Raw`-Format von R ausgibt, was von der grafischen Benutzeroberfläche nicht unterstützt wird.

1. Fügen Sie eine zweite Instanz des Moduls **Execute R Script** hinzu, und verbinden Sie sie mit dem Ausgangsport des vorherigen Moduls.

1. Geben Sie den folgenden Code in das Textfeld **R Script** ein, um das Objekt `A` aus der Eingabedatentabelle zu extrahieren. 

    ```R
    azureml_main <- function(dataframe1, dataframe2){
      print("R script run.")
      A <- unserialize(as.raw(dataframe1$serialized))  
      # Return datasets as a Named List
      return(list(dataset1=dataframe1, dataset2=dataframe2))
    }
    ```

## <a name="pre-installed-r-packages"></a>Bereits installierte R-Pakete

Die aktuelle Liste bereits installierter R-Pakete, die zur Verfügung stehen:

|              |            | 
|--------------|------------| 
| Paket      | Version    | 
| askpass      | 1.1        | 
| assertthat   | 0.2.1      | 
| backports    | 1.1.4      | 
| base         | 3.5.1      | 
| base64enc    | 0.1-3      | 
| BH           | 1.69.0-1   | 
| bindr        | 0.1.1      | 
| bindrcpp     | 0.2.2      | 
| bitops       | 1.0-6      | 
| starten         | 1.3-22     | 
| broom        | 0.5.2      | 
| callr        | 3.2.0      | 
| caret        | 6.0-84     | 
| caTools      | 1.17.1.2   | 
| cellranger   | 1.1.0      | 
| class        | 7.3-15     | 
| cli          | 1.1.0      | 
| clipr        | 0.6.0      | 
| cluster      | 2.0.7-1    | 
| codetools    | 0.2-16     | 
| colorspace   | 1.4-1      | 
| compiler     | 3.5.1      | 
| crayon       | 1.3.4      | 
| curl         | 3.3        | 
| data.table   | 1.12.2     | 
| datasets     | 3.5.1      | 
| DBI          | 1.0.0      | 
| dbplyr       | 1.4.1      | 
| digest       | 0.6.19     | 
| dplyr        | 0.7.6      | 
| e1071        | 1.7-2      | 
| Evaluieren     | 0.14       | 
| fansi        | 0.4.0      | 
| forcats      | 0.3.0      | 
| foreach      | 1.4.4      | 
| foreign      | 0.8-71     | 
| fs           | 1.3.1      | 
| gdata        | 2.18.0     | 
| generics     | 0.0.2      | 
| ggplot2      | 3.2.0      | 
| glmnet       | 2.0-18     | 
| glue         | 1.3.1      | 
| gower        | 0.2.1      | 
| gplots       | 3.0.1.1    | 
| graphics     | 3.5.1      | 
| grDevices    | 3.5.1      | 
| grid         | 3.5.1      | 
| gtable       | 0.3.0      | 
| gtools       | 3.8.1      | 
| haven        | 2.1.0      | 
| highr        | 0,8        | 
| hms          | 0.4.2      | 
| htmltools    | 0.3.6      | 
| httr         | 1.4.0      | 
| ipred        | 0.9-9      | 
| iterators    | 1.0.10     | 
| jsonlite     | 1.6        | 
| KernSmooth   | 2.23-15    | 
| knitr        | 1.23       | 
| labeling     | 0,3        | 
| lattice      | 0.20-38    | 
| lava         | 1.6.5      | 
| lazyeval     | 0.2.2      | 
| lubridate    | 1.7.4      | 
| magrittr     | 1.5        | 
| markdown     | 1          | 
| MASS         | 7.3-51.4   | 
| Matrix       | 1.2-17     | 
| methods      | 3.5.1      | 
| mgcv         | 1.8-28     | 
| mime         | 0,7        | 
| ModelMetrics | 1.2.2      | 
| modelr       | 0.1.4      | 
| munsell      | 0.5.0      | 
| nlme         | 3.1-140    | 
| nnet         | 7.3-12     | 
| numDeriv     | 2016.8-1.1 | 
| openssl      | 1.4        | 
| parallel     | 3.5.1      | 
| pillar       | 1.4.1      | 
| pkgconfig    | 2.0.2      | 
| plogr        | 0.2.0      | 
| plyr         | 1.8.4      | 
| prettyunits  | 1.0.2      | 
| processx     | 3.3.1      | 
| prodlim      | 2018.04.18 | 
| Fortschritt     | 1.2.2      | 
| ps           | 1.3.0      | 
| purrr        | 0.3.2      | 
| quadprog     | 1.5-7      | 
| quantmod     | 0.4-15     | 
| R6           | 2.4.0      | 
| randomForest | 4.6-14     | 
| RColorBrewer | 1.1-2      | 
| Rcpp         | 1.0.1      | 
| RcppRoll     | 0.3.0      | 
| readr        | 1.3.1      | 
| readxl       | 1.3.1      | 
| recipes      | 0.1.5      | 
| rematch      | 1.0.1      | 
| reprex       | 0.3.0      | 
| reshape2     | 1.4.3      | 
| reticulate   | 1.12       | 
| rlang        | 0.4.0      | 
| rmarkdown    | 1.13       | 
| ROCR         | 1.0-7      | 
| rpart        | 4.1-15     | 
| rstudioapi   | 0,1        | 
| rvest        | 0.3.4      | 
| scales       | 1.0.0      | 
| selectr      | 0.4-1      | 
| spatial      | 7.3-11     | 
| splines      | 3.5.1      | 
| SQUAREM      | 2017.10-1  | 
| stats        | 3.5.1      | 
| stats4       | 3.5.1      | 
| stringi      | 1.4.3      | 
| stringr      | 1.3.1      | 
| survival     | 2.44-1.1   | 
| sys          | 3.2        | 
| tcltk        | 3.5.1      | 
| tibble       | 2.1.3      | 
| tidyr        | 0.8.3      | 
| tidyselect   | 0.2.5      | 
| tidyverse    | 1.2.1      | 
| timeDate     | 3043.102   | 
| tinytex      | 0,13       | 
| Tools        | 3.5.1      | 
| tseries      | 0.10-47    | 
| TTR          | 0.23-4     | 
| utf8         | 1.1.4      | 
| utils        | 3.5.1      | 
| vctrs        | 0.1.0      | 
| viridisLite  | 0.3.0      | 
| whisker      | 0.3-2      | 
| withr        | 2.1.2      | 
| xfun         | 0,8        | 
| xml2         | 1.2.0      | 
| xts          | 0.11-2     | 
| yaml         | 2.2.0      | 
| zeallot      | 0.1.0      | 
| Zoo          | 1.8-6      | 

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich die [Gruppe der verfügbaren Module](module-reference.md) für Azure Machine Learning Service an. 
