---
title: Verwenden von Aufträgen für SQL-Datenbank-DAC-Pakete und Stream Analytics mit Azure SQL Database Edge | Microsoft-Dokumentation
description: Informationen zur Verwendung von Stream Analytics-Aufträgen in SQL Database Edge
keywords: sql database edge, stream analytics, sqlpackage
services: sql-database-edge
ms.service: sql-database-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 11/04/2019
ms.openlocfilehash: 1b6b3f4e4be9d056bc53fac2eb2f1f3fcd768085
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73510947"
---
# <a name="using-sql-database-dac-package-and-stream-analytics-job-with-sql-database-edge"></a>Verwenden von Aufträgen für SQL-Datenbank-DAC-Pakete und Stream Analytics mit SQL Database Edge

Azure SQL Database Edge (Vorschau) ist eine optimierte relationale Datenbank-Engine für IoT- und Edgebereitstellungen. Sie basiert auf den aktuellen Versionen der Microsoft SQL Server-Datenbank-Engine, die branchenführende Leistung, Sicherheit und Funktionen für die Abfrageverarbeitung bietet. Zusammen mit den branchenführenden Funktionen für die Verwaltung relationaler Datenbanken von SQL Server bietet Azure SQL Database Edge integrierte Streamingfunktionen für Echtzeitanalysen und komplexe Ereignisverarbeitung.

Azure SQL Database Edge bietet auch eine native Implementierung von SQLPackage.exe, mit der Benutzer während der Bereitstellung von SQL Database Edge ein [SQL-Datenbank-DAC](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications)-Paket bereitstellen können.

Über die IoT Edge-Moduloption *Gewünschte Eigenschaften für Modulzwilling* macht Azure SQL Database Edge zwei optionale Parameter verfügbar.

```json
{
    "properties.desired":
    {
        "SqlPackage": "<Optional_DACPAC_ZIP_SAS_URL>",
        "ASAJobInfo": "<Optional_ASA_Job_ZIP_SAS_URL>"
    }
}
```

|Feld | BESCHREIBUNG |
|------|-------------|
| SQLPackage | Azure Blob Storage-URI für die ZIP-Datei mit dem SQL-Datenbank-DAC-Paket.
| ASAJobInfo | Azure Blob Storage-URI für den ASA Edge-Auftrag. Weitere Informationen zum Veröffentlichen des ASA Edge-Auftrags finden Sie unter [Veröffentlichen eines ASA Edge-Auftrags für SQL Database Edge]().

## <a name="using-sql-database-dac-packages-with-sql-database-edge"></a>Verwenden von SQL-Datenbank-DAC-Paketen mit SQL Database Edge

Zum Verwenden eines SQL-Datenbank-DAC-Pakets (*.dacpac) mit SQL Database Edge führen Sie die unten beschriebenen Schritte aus.

1. Erstellen oder extrahieren Sie ein SQL-Datenbank-DAC-Paket. Sie können die in [Extrahieren einer DAC aus einer vorhandenen Datenbank](/sql/relational-databases/data-tier-applications/extract-a-dac-from-a-database/) erläuterten Konzepte nutzen, um ein DAC-Paket für eine vorhandene SQL-Datenbank zu generieren.

2. Komprimieren Sie die * *.dacpac*-Datei, und laden Sie sie in ein Azure Blob Storage-Konto hoch. Weitere Informationen zum Hochladen von Dateien in Azure Blob Storage finden Sie unter [Hochladen, Herunterladen und Auflisten von Blobs mit dem Azure-Portal](../storage/blobs/storage-quickstart-blobs-portal.md).

3. Generieren Sie im Azure-Portal eine SAS für die ZIP-Datei. Weitere Informationen finden Sie unter [Delegieren des Zugriffs mit Shared Access Signatures (SAS)](../storage/common/storage-sas-overview.md).

4. Aktualisieren Sie die Konfiguration des SQL Database Edge-Moduls, und schließen Sie den SAS-URI für das DAC-Paket ein. So aktualisieren Sie das SQL Database Edge-Modul:

    1. Navigieren Sie im Azure-Portal zu Ihrer IoT Hub-Bereitstellung.

    2. Klicken Sie im linken Bereich auf **IoT Edge**.

    3. Suchen Sie auf der Seite **IoT Edge** nach der IoT Edge-Instanz, in der das SQL Database Edge-Modul bereitgestellt ist, und klicken Sie darauf.

    4. Klicken Sie auf der Geräteseite *IoT Edge-Gerät* auf **Module festlegen**. 

    5. Klicken Sie auf der Seite **Module festlegen** beim SQL Database Edge-Modul auf *Konfigurieren*. 

    6. Wählen Sie auf der Seite **Benutzerdefinierte IoT Edge-Module** die Option *Gewünschte Eigenschaften für Modulzwilling festlegen* aus. Aktualisieren Sie die gewünschten Eigenschaften, und schließen Sie den URI für die SQLPackage-Option ein, wie im Beispiel unten gezeigt. 

        > [!NOTE]
        > Der unten gezeigte SAS-URI dient nur zur Veranschaulichung. Ersetzen Sie diesen URI durch den tatsächlichen URI Ihrer Bereitstellung.

        ```json
            {
                "properties.desired":
                {
                    "SqlPackage": "https://StorageAccountName.blob.core.windows.net/SQLpackageFiles/MeasurementsDB.zip?sp=r&st=2019-09-01T00:00:00Z&se=2019-09-30T00:00:00Z&spr=https&sv=2019-02-02&sr=b&sig=Uh8X0E50qzlxkiKVBJKU3ccVQYwF235qTz7AXp4R3gI%3D",
                }
            }
        ```

    7. Klicken Sie auf **Speichern**.

    8. Klicken Sie auf der Seite **Module festlegen** auf *Weiter*.

    9. Klicken Sie auf der Seite **Module festlegen** auf *Weiter* und dann auf **Senden**.

5. Nach dem Modulupdate wird die DAC-Paketdatei heruntergeladen, dekomprimiert und für die SQL Database Edge-Instanz bereitgestellt.

## <a name="using-streaming-jobs-with-sql-database-edge"></a>Verwenden von Streamingaufträgen mit SQL Database Edge

Azure SQL Database Edge verfügt über eine native Implementierung der Stream Analytics-Runtime. Damit können Benutzer einen Azure Stream Analytics-Edgeauftrag erstellen und diesen als SQL Database Edge-Streamingauftrag bereitstellen. Um einen Stream Analytics-Edgeauftrag zu erstellen, führen Sie die folgenden Schritte aus.

1. Navigieren Sie mit der Vorschau-[URL](https://portal.azure.com/?microsoft_azure_streamanalytics_edgeadapterspreview=true) zum Azure-Portal. Diese Vorschau-URL ermöglicht es Benutzern, eine SQL-Datenbank-Ausgabe für einen Stream Analytics-Edgeauftrag zu konfigurieren.

2. Erstellen Sie einen neuen **Azure Stream Analytics auf IoT Edge**-Auftrag, und wählen Sie für die Hostingumgebung **Edge** aus.

3. Definieren Sie die *Eingabe* und die *Ausgabe* für den Azure Stream Analytics-Auftrag. Jede SQL-Ausgabe (wie unten konfiguriert) ist an eine einzelne Tabelle innerhalb der Datenbank gebunden. Wenn ein Streaming von Daten an mehrere Tabellen erforderlich ist, müssen Sie mehrere SQL-Datenbank-Ausgaben erstellen. Die SQL-Ausgaben können so konfiguriert werden, dass sie auf verschiedene Datenbanken verweisen.

    *Eingabe: Wählen Sie Edge Hub als Eingabe für den Edgeauftrag aus, und geben Sie die notwendigen Ressourceninformationen an.*

    *Ausgabe: Wählen Sie SQL-Datenbank als Ausgabe aus, aktivieren Sie die Option „SQL-Datenbank-Einstellungen manuell angeben“, und geben Sie die Konfigurationsdetails für die Datenbank und die Tabelle an.*

    |Feld      | BESCHREIBUNG |
    |---------------|-------------|
    |Ausgabealias | Der Name des Ausgabealias.|
    |Datenbank | Der Name der SQL-Datenbank. Hierbei muss es sich um einen gültigen Datenbanknamen handeln, der in der SQL Database Edge-Instanz vorhanden ist.|
    |Servername | Name (oder IP-Adresse) und Portnummer für die SQL Server-Instanz. Bei einer SQL Database Edge-Bereitstellung können Sie **tcp:.,1433** als Servernamen verwenden.|
    |Username | SQL-Anmeldekonto, das über Zugriff als Datenleser und Datenschreiber auf die oben genannte Datenbank verfügt.|
    |Kennwort | Kennwort für das oben genannte SQL-Anmeldekonto.|
    |Table | Name der Tabelle, die für den Streamingauftrag ausgegeben wird.|
    |Partitionierung erben| Diese Konfigurationsoption für die SQL-Ausgabe ermöglicht das Erben des Partitionierungsschemas des vorherigen Abfrageschritts oder der vorherigen Eingabe. Wenn diese Option aktiviert ist, sind beim Schreiben in eine datenträgerbasierte Tabelle und beim Verwenden einer vollständig parallelen Topologie für Ihren Auftrag bessere Durchsatzwerte zu erwarten.|
    |Batchgröße| Die Batchgröße ist die maximale Anzahl von Datensätzen, die bei jeder Masseneinfügungstransaktion gesendet werden.|

    Beispielkonfiguration für die Ein- und Ausgabe:

    ```txt
        Input:
            Input from EdgeHub
            Input alias: input
            Event serialization format: JSON
            Encoding: UTF-8
            Event compression type: None

        Output :
            Output alias: output
            Database:  MeasurementsDB
            Server name: tcp:.,1433
            Username: sa
            Password: <Password>
            Table: TemperatureMeasurements
            Inherit Partitioning: Merge all input partitions into a single writer
            Max batch count: 10000
    ```

    > [!NOTE]
    > Weitere Informationen zum SQL-Ausgabeadapter für Azure Stream Analytics finden Sie unter [Azure Stream Analytics-Ausgabe an Azure SQL-Datenbank](../stream-analytics/stream-analytics-sql-output-perf.md).

4. Definieren Sie die ASA-Auftragsabfrage für den Edgeauftrag. Diese Abfrage muss die definierten Eingabe- und Ausgabealiase als Eingabe- und Ausgabenamen verwenden. Weitere Informationen finden Sie in der [Referenz zur Stream Analytics-Abfragesprache](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference).

5. Legen Sie die Speicherkontoeinstellungen für den Edgeauftrag fest. Das Speicherkonto wird als Veröffentlichungsziel für den Edgeauftrag verwendet.

6. Wählen Sie unter „Konfigurieren“ die Option „Veröffentlichen“ aus, und klicken Sie auf die Schaltfläche „Veröffentlichen“. Speichern Sie den SAS-URL zur Verwendung mit dem SQL Database Edge-Modul.

### <a name="deploy-the-stream-analytics-edge-job-to-the-sql-database-edge"></a>Bereitstellen des Stream Analytics-Edgeauftrags im SQL Database Edge

Um den Streamingauftrag im SQL Database Edge-Modul bereitzustellen, aktualisieren Sie die Konfiguration des SQL Database Edge-Moduls, und schließen Sie den SAS-URI für den Streamingauftrag aus dem oben genannten Schritt ein. So aktualisieren Sie das SQL Database Edge-Modul:

1. Navigieren Sie im Azure-Portal zu Ihrer IoT Hub-Bereitstellung.

2. Klicken Sie im linken Bereich auf **IoT Edge**.

3. Suchen Sie auf der Seite **IoT Edge** nach der IoT Edge-Instanz, in der das SQL Database Edge-Modul bereitgestellt ist, und klicken Sie darauf.

4. Klicken Sie auf der Geräteseite *IoT Edge-Gerät* auf **Module festlegen**. 

5. Klicken Sie auf der Seite **Module festlegen** beim SQL Database Edge-Modul auf *Konfigurieren*. 

6. Wählen Sie auf der Seite **Benutzerdefinierte IoT Edge-Module** die Option *Gewünschte Eigenschaften für Modulzwilling festlegen* aus. Aktualisieren Sie die gewünschten Eigenschaften, und schließen Sie den URI für die ASAJobInfo-Option ein, wie im Beispiel unten gezeigt. 

    > [!NOTE]
    > Der unten gezeigte SAS-URI dient nur zur Veranschaulichung. Ersetzen Sie diesen URI durch den tatsächlichen URI Ihrer Bereitstellung.

    ```json
        {
            "properties.desired":
            {
                "ASAJobInfo": "https://storageAccountName.blob.core.windows.net/asajobs/ASAEdgeJobs/5a9b34db-7a5c-4606-adae-4c8609eaa1c7/d85b5aa6-4d38-4703-bb34-af7f0bd7916d/ASAEdgeJobDefinition.zip?sv=2018-03-28&sr=b&sig=HH%2BFMsEy378RaTxIy2Xo6rM6wDaqoBaZ5zFDBqdZiS0%3D&st=2019-09-01T22%3A24%3A34Z&se=2019-09-30T22%3A34%3A34Z&sp=r"   
            }
        }
    ```

7. Klicken Sie auf **Speichern**.

8. Klicken Sie auf der Seite **Module festlegen** auf *Weiter*.

9. Klicken Sie auf der Seite **Module festlegen** auf *Weiter* und dann auf **Senden**.

10. Nach dem Modulupdate wird die Stream Analytics-Auftragsdatei heruntergeladen, dekomprimiert und für die SQL Database Edge-Instanz bereitgestellt.

## <a name="next-steps"></a>Nächste Schritte

- Details zu Preisen und Verfügbarkeit finden Sie unter [Azure SQL Database Edge](https://azure.microsoft.com/services/sql-database-edge/).
- Fordern Sie eine Aktivierung von Azure SQL Database Edge für Ihr Abonnement an.
- Hinweise zu den ersten Schritten finden Sie hier:
  - [Bereitstellen von SQL Database Edge über das Azure-Portal](deploy-portal.md)
