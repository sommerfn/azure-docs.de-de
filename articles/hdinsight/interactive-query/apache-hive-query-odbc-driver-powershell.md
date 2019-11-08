---
title: Abfragen von Apache Hive mit ODBC-Treiber und PowerShell – Azure HDInsight
description: Verwenden Sie den Microsoft Hive ODBC-Treiber und PowerShell zum Abfragen von Apache Hive-Clustern in Azure HDInsight.
keywords: Hive, Hive ODBC, PowerShell
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: tutorial
ms.date: 06/27/2019
ms.openlocfilehash: f6139bb98fa0272e43c8e180d4ec029f7a7538bb
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73494317"
---
# <a name="tutorial-query-apache-hive-with-odbc-and-powershell"></a>Tutorial: Abfragen von Apache Hive mit ODBC und PowerShell

Microsoft ODBC-Treiber bieten eine flexible Möglichkeit zur Interaktion mit verschiedenen Arten von Datenquellen, einschließlich Apache Hive. Sie können Code in Skriptsprachen wie PowerShell schreiben, die die ODBC-Treiber verwenden, um eine Verbindung mit Ihrem Hive-Cluster herzustellen, eine Abfrage Ihrer Wahl zu übergeben und die Ergebnisse anzuzeigen.

In diesem Tutorial führen Sie die folgenden Aufgaben aus:

> [!div class="checklist"]
> * Herunterladen und Installieren des Microsoft Hive ODBC-Treibers
> * Erstellen einer ODBC-Datenquelle für Apache Hive, die mit Ihrem Cluster verknüpft ist
> * Abfragen von Beispielinformationen aus Ihrem Cluster mit PowerShell

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie mit diesem Tutorial beginnen können, benötigen Sie Folgendes:

* Einen Interactive Query-Cluster in HDInsight. Hinweise zum Erstellen finden Sie unter [Erste Schritte mit Azure HDInsight](../hdinsight-hadoop-provision-linux-clusters.md). Wählen Sie als Clustertyp **Interactive Query** aus.

## <a name="install-microsoft-hive-odbc-driver"></a>Installieren des Microsoft Hive ODBC-Treibers

Laden Sie den [Microsoft Hive ODBC-Treiber](https://www.microsoft.com/download/details.aspx?id=40886) herunter, und installieren Sie ihn.

## <a name="create-apache-hive-odbc-data-source"></a>Erstellen einer Apache Hive ODBC-Datenquelle

Die folgenden Schritte zeigen Ihnen, wie Sie eine Apache Hive-ODBC-Datenquelle erstellen.

1. Navigieren Sie in Windows zu **Start** > **Windows-Verwaltungsprogramme** > **ODBC-Datenquellen (32-Bit)/(64-Bit)** .  Das Fenster **ODBC-Datenquellen-Administrator** wird geöffnet.

    ![ODBC-Datenquellen-Administrator](./media/apache-hive-query-odbc-driver-powershell/hive-odbc-driver-dsn-setup.png "Konfigurieren eines DSN mithilfe des ODBC-Datenquellen-Administrators")

1. Wählen Sie auf der Registerkarte **Benutzer-DSN** die Option **Hinzufügen** aus, um das Fenster **Neue Datenquelle erstellen** zu öffnen.

1. Wählen Sie **Microsoft Hive ODBC-Treiber** und dann **Fertig stellen** aus, um das Fenster **DSN-Setup für Microsoft Hive ODBC-Treiber** zu öffnen.

1. Geben Sie folgende Werte ein bzw. wählen diese aus:

   | Eigenschaft | BESCHREIBUNG |
   | --- | --- |
   |  Name der Datenquelle |Geben Sie einen Namen für die Datenquelle an. |
   |  Host(s) |Geben Sie `CLUSTERNAME.azurehdinsight.net` ein. Zum Beispiel, `myHDICluster.azurehdinsight.net` |
   |  Port |Verwenden Sie **443**.|
   |  Datenbank |Verwenden Sie **default**. |
   |  Mechanismus |Wählen Sie **Microsoft Azure HDInsight Service**. |
   |  Benutzername |Geben Sie Ihren HTTP-Benutzernamen für den HDInsight-Cluster an. Der Standard-Benutzername lautet **admin**. |
   |  Kennwort |Geben Sie Ihr Benutzerkennwort für den HDInsight-Cluster an. Aktivieren Sie das Kontrollkästchen **Kennwort speichern (verschlüsselt)** .|

1. Optional: Wählen Sie **Erweiterte Optionen** aus.  

   | Parameter | BESCHREIBUNG |
   | --- | --- |
   |  Use Native Query |Wenn diese Option ausgewählt ist, versucht der ODBC-Treiber NICHT, TSQL in HiveQL zu konvertieren. Verwenden Sie diese Option nur, wenn Sie absolut sicher sind, dass Sie reine HiveQL-Anweisungen übermitteln. Wenn Sie eine Verbindung mit SQL Server oder Azure SQL-Datenbank herstellen, sollten Sie die Option nicht aktivieren. |
   |  Rows fetched per block |Wenn Sie viele Datensätze abrufen, ist es möglicherweise erforderlich, diesen Parameter zu optimieren, um optimale Leistung zu garantieren. |
   |  Default string column length, Binary column length, Decimal column scale |Längen und Genauigkeiten der Datentypen können beeinflussen, wie die Daten zurückgegeben werden. Aufgrund zu geringer Genauigkeit und von Abschneidevorgängen werden falsche Informationen zurückgegeben. |

    ![Erweiterte DSN-Konfigurationsoptionen](./media/apache-hive-query-odbc-driver-powershell/odbc-data-source-advanced-options.png "Erweiterte DSN-Konfigurationsoptionen")

1. Wählen Sie **Testen** aus, um die Datenquelle zu testen. Wenn die Datenquelle richtig konfiguriert wurde, wird als Testergebnis **SUCCESS** angezeigt.  

1. Wählen Sie **OK** aus, um das Testfenster zu schließen.  

1. Wählen Sie **OK** aus, um das Fenster **DSN-Setup für Microsoft Hive ODBC-Treiber** zu schließen.  

1. Wählen Sie **OK** aus, um das Fenster **ODBC-Datenquellen-Administrator** zu schließen.  

## <a name="query-data-with-powershell"></a>Abfragen von Daten mit PowerShell

Das folgende PowerShell-Skript ist eine Funktion, die von ODBC zum Abfragen einer Hive-Clusters verwendet wird.

```powershell
function Get-ODBC-Data {

   param(
   [string]$query=$(throw 'query is required.'),
   [string]$dsn,  
   [PSCredential] $cred = (Get-Credential)  
   )

   $conn = New-Object System.Data.Odbc.OdbcConnection
   $uname = $cred.UserName

   $pswd = (New-Object System.Net.NetworkCredential -ArgumentList "", $cred.Password).Password
   $conn.ConnectionString = "DSN=$dsn;Uid=$uname;Pwd=$pswd;"
   $conn.open()
   $cmd = New-object System.Data.Odbc.OdbcCommand($query,$conn)

   $ds = New-Object system.Data.DataSet

   (New-Object system.Data.odbc.odbcDataAdapter($cmd)).fill($ds) #| out-null
   $conn.close()
   $ds.Tables
}
```

Im folgenden Codeausschnitt wird mit der obigen Funktion eine Abfrage auf den Interactive Query-Cluster angewendet, den Sie zu Beginn des Tutorials angelegt haben. Ersetzen Sie `DATASOURCENAME` durch den **Datenquellennamen**, den Sie auf dem Bildschirm **Microsoft Hive ODBC-Treiber – DNS-Setup** angegeben haben. Wenn Sie zur Eingabe von Anmeldeinformationen aufgefordert werden, geben Sie den Benutzernamen und das Kennwort entsprechend den Angaben ein, die Sie bei der Erstellung des Clusters unter **Benutzername für Clusteranmeldung** und **Kennwort für Clusteranmeldung** eingegeben haben.

```powershell

$dsn = "DATASOURCENAME"

$query = "select count(distinct clientid) AS total_clients from hivesampletable"

Get-ODBC-Data -query $query -dsn $dsn
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie die Ressourcengruppe, den HDInsight-Cluster und das Speicherkonto, wenn diese Elemente nicht mehr benötigt werden. Wählen Sie dazu die Ressourcengruppe aus, in der der Cluster erstellt wurde, und klicken Sie auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie gelernt, wie Sie den Microsoft Hive ODBC-Treiber und PowerShell verwenden, um Daten aus Ihrem Azure HDInsight-Cluster des Typs „Interactive Query“ abzurufen.

> [!div class="nextstepaction"]
> [Verbinden von Excel mit Apache Hive mithilfe von ODBC](../hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md)
