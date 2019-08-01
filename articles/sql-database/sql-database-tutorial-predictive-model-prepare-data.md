---
title: 'Tutorial: Vorbereiten von Daten zum Trainieren eines Vorhersagemodells in R'
titleSuffix: Azure SQL Database Machine Learning Services (preview)
description: In Teil 1 dieser dreiteiligen Reihe von Tutorials bereiten Sie die Daten aus einer Azure SQL-Datenbank zum Trainieren eines Vorhersagemodells in R mit Machine Learning Services (Vorschauversion) für Azure SQL-Datenbank vor.
services: sql-database
ms.service: sql-database
ms.subservice: machine-learning
ms.custom: ''
ms.devlang: r
ms.topic: tutorial
author: garyericson
ms.author: garye
ms.reviewer: davidph
manager: cgronlun
ms.date: 07/26/2019
ms.openlocfilehash: c1271d5b63fa796fe44b7a40c364953464a87539
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2019
ms.locfileid: "68596667"
---
# <a name="tutorial-prepare-data-to-train-a-predictive-model-in-r-with-azure-sql-database-machine-learning-services-preview"></a>Tutorial: Vorbereiten von Daten zum Trainieren eines Vorhersagemodells in R mit Machine Learning Services (Vorschauversion) für Azure SQL-Datenbank

In Teil 1 dieser dreiteiligen Tutorialreihe importieren Sie Daten aus einer Azure SQL-Datenbank in R und bereiten diese Daten vor. Später in dieser Reihe verwenden Sie die Daten zum Trainieren und Bereitstellen eines Predictive Machine Learning-Modells in R mit Machine Learning Services für Azure SQL-Datenbank (Vorschauversion).

Stellen Sie sich für diese Reihe von Tutorials vor, dass Sie einen Skiverleih betreiben und die Anzahl der Vermietungen an einem in der Zukunft liegenden Datum vorhersagen möchten. Diese Informationen helfen Ihnen, Ihren Lagerbestand, Ihre Mitarbeiter und Ihre Anlagen bereitzumachen.

In den Teilen 1 und 2 dieser Reihe entwickeln Sie einige R-Skripts in RStudio, um Ihre Daten vorzubereiten und ein Machine Learning-Modell zu trainieren. In Teil 3 führen Sie dann diese R-Skripts in einer SQL-Datenbank mithilfe gespeicherter Prozeduren aus.

In diesem Artikel lernen Sie Folgendes:

> [!div class="checklist"]
> * Importieren einer Beispieldatenbank in eine Azure SQL-Datenbank mithilfe von R
> * Laden der Daten aus der Azure SQL-Datenbank in einen R-Datenrahmen
> * Vorbereiten der Daten in R durch Identifizieren einiger Spalten als Kategorien

In [Teil 2](sql-database-tutorial-predictive-model-build-compare.md) erfahren Sie, wie Sie mehrere Machine Learning-Modelle in R erstellen und trainieren und dann das genaueste auswählen.

In [Teil 3](sql-database-tutorial-predictive-model-deploy.md) lernen Sie, wie Sie das Modell in einer Datenbank speichern und anschließend auf der Grundlage der in den Teilen 1 und 2 entwickelten R-Skripts gespeicherte Prozeduren erstellen. Die gespeicherten Prozeduren werden in einer SQL-Datenbank ausgeführt, um Vorhersagen basierend auf neuen Daten zu treffen.

[!INCLUDE[ml-preview-note](../../includes/sql-database-ml-preview-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement: Falls Sie nicht über ein Azure-Abonnement verfügen, [erstellen Sie ein Konto](https://azure.microsoft.com/free/), bevor Sie beginnen.

* Azure SQL-Datenbankserver mit aktivierten Machine Learning Services: Während der öffentlichen Vorschauphase führt Microsoft für Sie das Onboarding durch und aktiviert das maschinelle Lernen für Ihre vorhandene oder neue Datenbank. Führen Sie die Schritte in [Registrieren für die Vorschauversion](sql-database-machine-learning-services-overview.md#signup) aus.

* RevoScaleR-Paket: Unter [RevoScaleR](https://docs.microsoft.com/sql/advanced-analytics/r/ref-r-revoscaler?view=sql-server-2017#versions-and-platforms) finden Sie Optionen zum lokalen Installieren dieses Pakets.

* R-IDE: In diesem Tutorial wird [RStudio Desktop](https://www.rstudio.com/products/rstudio/download/) verwendet.

* SQL-Abfragetool: In diesem Tutorial wird davon ausgegangen, dass Sie [Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/what-is) oder [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms) (SSMS) verwenden.

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

## <a name="import-the-sample-database"></a>Importieren der Beispieldatenbank

Das in diesem Tutorial verwendete Beispieldataset wurde in einer **BACPAC**-Datenbank-Sicherungsdatei gespeichert, die Sie herunterladen und verwenden können.

1. Laden Sie die Datei [TutorialDB.bacpac](https://sqlchoice.blob.core.windows.net/sqlchoice/static/TutorialDB.bacpac) herunter.

1. Befolgen Sie die Anweisungen in [Importieren einer BACPAC-Datei in eine Datenbank in Azure SQL-Datenbank](https://docs.microsoft.com/azure/sql-database/sql-database-import), und verwenden Sie dazu diese Details:

   * Importieren Sie aus der heruntergeladenen **TutorialDB.bacpac**-Datei
   * Wählen Sie während der öffentlichen Vorschau die Konfiguration **Gen5/vCore** für die neue Datenbank
   * Benennen Sie die neue Datenbank "TutorialDB"

## <a name="load-the-data-into-a-data-frame"></a>Laden der Daten in einen Datenrahmen

Um die Daten in R zu verwenden, laden Sie die Daten aus der Azure SQL-Datenbank in einen Datenrahmen (`rentaldata`).

Erstellen Sie eine neue RScript-Datei in RStudio, und führen Sie das folgende Skript aus. Ersetzen Sie **Server**, **UID** und **PWD** durch Ihre eigenen Verbindungsinformationen.

```r
#Define the connection string to connect to the TutorialDB database
connStr <- paste("Driver=SQL Server",
               "; Server=", "<Azure SQL Database Server>",
               "; Database=TutorialDB",
               "; UID=", "<user>",
               "; PWD=", "<password>",
                  sep = "");

#Get the data from the table
SQL_rentaldata <- RxSqlServerData(table = "dbo.rental_data", connectionString = connStr, returnDataFrame = TRUE);

#Import the data into a data frame
rentaldata <- rxImport(SQL_rentaldata);

#Take a look at the structure of the data and the top rows
head(rentaldata);
str(rentaldata);
```

Die Ergebnisse sollten ähnlich wie hier aussehen:

```results
   Year  Month  Day  RentalCount  WeekDay  Holiday  Snow
1  2014    1     20      445         2        1      0
2  2014    2     13       40         5        0      0
3  2013    3     10      456         1        0      0
4  2014    3     31       38         2        0      0
5  2014    4     24       23         5        0      0
6  2015    2     11       42         4        0      0
'data.frame':       453 obs. of  7 variables:
$ Year       : int  2014 2014 2013 2014 2014 2015 2013 2014 2013 2015 ...
$ Month      : num  1 2 3 3 4 2 4 3 4 3 ...
$ Day        : num  20 13 10 31 24 11 28 8 5 29 ...
$ RentalCount: num  445 40 456 38 23 42 310 240 22 360 ...
$ WeekDay    : num  2 5 1 2 5 4 1 7 6 1 ...
$ Holiday    : int  1 0 0 0 0 0 0 0 0 0 ...
$ Snow       : num  0 0 0 0 0 0 0 0 0 0 ...
```

## <a name="prepare-the-data"></a>Vorbereiten der Daten

In dieser Beispieldatenbank wurde der größte Teil der Vorbereitung bereits erledigt, aber einen Vorbereitungsschritt müssen Sie hier ausführen.
Verwenden Sie das folgende R-Skript, um drei Spalten als *Kategorien* zu identifizieren, indem Sie den Datentyp in *factor* ändern.

```r
#Changing the three factor columns to factor types
rentaldata$Holiday <- factor(rentaldata$Holiday);
rentaldata$Snow    <- factor(rentaldata$Snow);
rentaldata$WeekDay <- factor(rentaldata$WeekDay);

#Visualize the dataset after the change
str(rentaldata);
```

Die Ergebnisse sollten ähnlich wie hier aussehen:

```results
data.frame':      453 obs. of  7 variables:
$ Year       : int  2014 2014 2013 2014 2014 2015 2013 2014 2013 2015 ...
$ Month      : num  1 2 3 3 4 2 4 3 4 3 ...
$ Day        : num  20 13 10 31 24 11 28 8 5 29 ...
$ RentalCount: num  445 40 456 38 23 42 310 240 22 360 ...
$ WeekDay    : Factor w/ 7 levels "1","2","3","4",..: 2 5 1 2 5 4 1 7 6 1 ...
$ Holiday    : Factor w/ 2 levels "0","1": 2 1 1 1 1 1 1 1 1 1 ...
$ Snow       : Factor w/ 2 levels "0","1": 1 1 1 1 1 1 1 1 1 1 ...
```

Die Daten sind jetzt für das Training vorbereitet.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie dieses Tutorial nicht fortsetzen möchten, löschen Sie die TutorialDB-Datenbank von Ihrem Azure SQL-Datenbankserver.

Führen Sie im Azure-Portal die folgenden Schritte aus:

1. Wählen Sie im Menü auf der linken Seite im Azure-Portal **Alle Ressourcen** oder **SQL-Datenbanken** aus.
1. Geben Sie im Feld **Nach Name filtern...** die **TutorialDB** ein, und wählen Sie Ihr Abonnement aus.
1. Wählen Sie Ihre TutorialDB-Datenbank aus.
1. Wählen Sie auf der Seite **Übersicht** die Option **Löschen** aus.

## <a name="next-steps"></a>Nächste Schritte

Im ersten Teil dieser Tutorialreihe haben Sie die folgenden Schritte ausgeführt:

* Importieren einer Beispieldatenbank in eine Azure SQL-Datenbank mithilfe von R
* Laden der Daten aus der Azure SQL-Datenbank in einen R-Datenrahmen
* Vorbereiten der Daten in R durch Identifizieren einiger Spalten als Kategorien

Um ein Machine Learning-Modell zu erstellen, das Daten aus der TutorialDB-Datenbank verwendet, verfolgen Sie Teil zwei dieser Tutorialreihe:

> [!div class="nextstepaction"]
> [Tutorial: Erstellen eines Vorhersagemodells in R mit Machine Learning Services (Vorschauversion) für Azure SQL-Datenbank](sql-database-tutorial-predictive-model-build-compare.md)
