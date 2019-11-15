---
title: Streamen von Daten mithilfe der Stream Analytics-Integration (Vorschau)
description: Verwenden Sie Azure Stream Analytics zum Streamen von Daten in eine Azure SQL-Datenbank.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: ajetasin
ms.author: ajetasi
ms.reviewer: sstein
ms.date: 11/04/2019
ms.openlocfilehash: d233d3c98cc495e4b9e84142781f5eb9faa7eec8
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73820843"
---
# <a name="stream-data-by-using-azure-sql-database-stream-analytics-integration-preview"></a>Streamen von Daten mithilfe der Stream Analytics Integration in Azure SQL-Datenbank (Vorschau)

Benutzer können jetzt mithilfe von [Azure Stream Analytics](../stream-analytics/stream-analytics-introduction.md) Streamingdaten in Echtzeit direkt aus einer SQL-Datenbank im Azure-Portal in einer Tabelle erfassen, verarbeiten, anzeigen und analysieren. Dies ermöglicht eine Vielzahl von Szenarien, wie z.B. vernetzte Fahrzeuge, Remoteüberwachung, Betrugserkennung und vieles mehr. Im Azure-Portal können Sie eine Ereignisquelle (Event Hub/IOT Hub) auswählen, eingehende Echtzeitereignisse anzeigen und eine Tabelle zum Speichern von Ereignissen auswählen. Sie können im Portal auch Abfragen in der Stream Analytics-Abfragesprache schreiben, um eingehende Ereignisse zu transformieren und in der ausgewählten Tabelle zu speichern. Dieser neue Einstiegspunkt wird zusätzlich zu den Erstellungs- und Konfigurationsfunktionen geboten, die bereits in Stream Analytics vorhanden sind. Diese Funktionalität beginnt im Kontext Ihrer Datenbank, sodass Sie schnell einen Stream Analytics-Auftrag einrichten und nahtlos zwischen der Azure SQL-Datenbank und Stream Analytics navigieren können.

![Ablauf in Stream Analytics](media/sql-database-stream-analytics/stream-analytics-flow.png)

## <a name="key-benefits"></a>Hauptvorteile

- Minimaler Kontextwechsel: Sie können von einer SQL-Datenbank im Portal aus starten und mit der Erfassung von Echtzeitdaten in einer Tabelle beginnen, ohne zu einem anderen Dienst zu wechseln. 
- Reduzierte Anzahl von Schritten: Der Kontext Ihrer Datenbank und Tabelle wird verwendet, um einen Stream Analytics-Auftrag vorab zu konfigurieren.
- Zusätzliche Benutzerfreundlichkeit mit Vorschau von Daten: Sie können eine Vorschau eingehender Daten aus der Ereignisquelle (Event Hub/IOT Hub) im Kontext der ausgewählten Tabelle anzeigen. 


## <a name="prerequisites"></a>Voraussetzungen

Zum Ausführen der Schritte in diesem Artikel benötigen Sie folgende Ressourcen:

- Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto erstellen](https://azure.microsoft.com/free/). 
- Eine SQL-Datenbank. Ausführliche Informationen finden Sie unter [Erstellen einer Einzeldatenbank in Azure SQL-Datenbank](sql-database-single-database-get-started.md).
- Eine Firewallregel, die dem Computer das Herstellen einer Verbindung mit dem Azure SQL-Server ermöglicht. Ausführliche Informationen finden Sie unter [Erstellen einer Firewallregel auf Serverebene](sql-database-server-level-firewall-rule.md).


## <a name="configure-stream-analytics-integration"></a>Konfigurieren der Stream Analytics-Integration

1. Melden Sie sich beim Azure-Portal an. 
2. Navigieren Sie zu Ihrer SQL-Datenbank, in der Sie Streamingdaten erfassen möchten. Wählen Sie **Stream Analytics (Vorschau)** aus. 

    ![Stream Analytics](media/sql-database-stream-analytics/stream-analytics.png)

3. Um mit dem Erfassen Ihrer Streamingdaten in dieser SQL-Datenbank zu beginnen, wählen Sie **Erstellen** aus. Geben Sie dem Streamingauftrag einen Namen, und wählen Sie dann **Weiter: Eingabe** aus. 

    ![Erstellen eines Stream Analytics-Auftrags](media/sql-database-stream-analytics/create-job.png)

4. Geben Sie Informationen zu Ihrer Ereignisquelle ein, und wählen Sie dann **Weiter: Ausgabe** aus.

   - **Eingabetyp**: Event Hub/IoT Hub
   - **Eingabealias**: Geben Sie einen Namen zum Identifizieren Ihrer Ereignisquelle ein. 
   - **Abonnement**: Identisch mit SQL-Datenbank-Abonnement. 
   - **Event Hub-Namespace**: Der Name für den Namespace. 
   - **Event Hub-Name**: Der Name des Event Hubs innerhalb des ausgewählten Namespace. 
   - **Event Hub-Richtlinienname** (Standardeinstellung: Neu erstellen): Geben Sie einen Richtliniennamen an. 
   - **Event Hub-Consumergruppe** (Standardeinstellung: Neu erstellen): Geben Sie einen Consumergruppennamen an.  
     - Es wird empfohlen, dass Sie eine Consumergruppe und eine Richtlinie für jeden neuen Azure Stream Analytics-Auftrag erstellen, den Sie von hier aus erstellen. Consumergruppen erlauben nur fünf Leser gleichzeitig. Wenn Sie also für jeden Auftrag eine dedizierte Consumergruppe bereitstellen, vermeiden Sie mögliche Fehler durch das Überschreiten dieses Grenzwerts. Eine dedizierte Richtlinie ermöglicht das Rotieren Ihres Schlüssels oder das Widerrufen von Berechtigungen ohne Auswirkungen auf andere Ressourcen.

    ![Erstellen eines Stream Analytics-Auftrags](media/sql-database-stream-analytics/create-job-output.png)

5. Wählen Sie aus, in welcher Tabelle die Streamingdaten erfasst werden sollen. Wählen Sie anschließend die Option **Erstellen** aus.
   - **Benutzername**, **Kennwort**: Geben Sie Ihre Anmeldeinformationen für die SQL Server-Authentifizierung ein. Wählen Sie **Überprüfen**.
   - **Tabelle**: Wählen Sie **Neu erstellen** oder **Vorhandene verwenden** aus. Bei diesem Ablauf wählen Sie **Erstellen**. Dadurch wird eine neue Tabelle erstellt, wenn Sie den Stream Analytics-Auftrag starten.

    ![Erstellen eines Stream Analytics-Auftrags](media/sql-database-stream-analytics/create.png)

6. Es wird eine Abfrageseite mit folgenden Details geöffnet:

   - Ihre **Eingabe** (Eingabeereignisquelle), aus der Daten erfasst werden.  
   - Ihre **Ausgabe** (Ausgabetabelle), in der transformierte Daten gespeichert werden. 
   - Eine [SAQL-Beispielabfrage](../stream-analytics/stream-analytics-stream-analytics-query-patterns.md) mit SELECT-Anweisung. 
   - **Eingabevorschau**: Zeigt eine Momentaufnahme der neuesten eingehenden Daten aus der Eingabeereignisquelle.  
     - Der Serialisierungstyp in Ihren Daten wird automatisch erkannt (JSON/CSV). Sie können ihn manuell in JSON, CSV oder AVRO ändern. 
     - Sie können eine Vorschau der eingehenden Daten im Tabellenformat oder Rohformat anzeigen. 
     - Wenn die angezeigten Daten nicht aktuell sind, wählen Sie **Aktualisieren** aus, um die neuesten Ereignisse anzuzeigen. 
     - Wählen Sie **Zeitbereich auswählen**, um Ihre Abfrage mit einem bestimmten Zeitraum eingehender Ereignisse zu testen. 
     - Wählen Sie **Beispieleingabe hochladen**, um Ihre Abfrage durch Hochladen einer JSON/CSV-Beispieldatei zu testen. Weitere Informationen zum Testen einer SAQL-Abfrage finden Sie unter [Testen eines Azure Stream Analytics-Auftrags mit Beispieldaten](../stream-analytics/stream-analytics-test-query.md). 

    ![Testabfrage](media/sql-database-stream-analytics/test-query.png)


   - **Testergebnisse**: Wählen Sie **Testabfrage** aus, und die Ergebnisse Ihrer Streamingabfrage werden angezeigt 

    ![Testergebnisse](media/sql-database-stream-analytics/test-results.png)

   - **Testergebnisschema**: Zeigt das Schema der Ergebnisse Ihrer Streamingabfrage nach dem Testen. Stellen Sie sicher, dass das Testergebnisschema mit dem Ausgabeschema übereinstimmt. 

    ![Testergebnisschema](media/sql-database-stream-analytics/test-results-schema.png)


   - **Ausgabeschema**: Hier ist das Schema der Tabelle enthalten, die Sie in Schritt 5 (neu oder vorhanden) ausgewählt haben.
     - Neu erstellen: Wenn Sie diese Option in Schritt 5 ausgewählt haben, wird das Schema erst angezeigt, wenn Sie den Streamingauftrag starten. Beim Erstellen einer neuen Tabelle wählen Sie den entsprechenden Tabellenindex aus. Weitere Informationen zur Tabellenindizierung finden Sie unter [Beschreibung von gruppierten und nicht gruppierten Indizes](/sql/relational-databases/indexes/clustered-and-nonclustered-indexes-described/).
    - Vorhandene verwenden: Wenn Sie diese Option in Schritt 5 ausgewählt haben, wird das Schema der ausgewählten Tabelle angezeigt. 
 
7. Wenn Sie mit dem Erstellen und Testen der Abfrage fertig sind, wählen Sie **Abfrage speichern** aus. Wählen Sie **Stream Analytics-Auftrag starten** aus, um mit dem Erfassen transformierter Daten in der SQL-Tabelle zu beginnen. Nachdem Sie die folgenden Felder fertiggestellt haben, **starten** Sie den Auftrag. 
   - **Startzeitpunkt der Ausgabe**: Hiermit wird der Zeitpunkt der ersten Ausgabe des Auftrags definiert.  
     - Jetzt: Der Auftrag wird jetzt gestartet, und neue eingehende Daten werden verarbeitet.
     - Benutzerdefiniert: Der Auftrag wird jetzt gestartet, verarbeitet jedoch Daten ab einem bestimmten Zeitpunkt (der in der Vergangenheit oder der Zukunft liegen kann). Weitere Informationen finden Sie unter [Starten eines Azure Stream Analytics-Auftrags](../stream-analytics/start-job.md).
   - **Streamingeinheiten**: Die Kosten für Azure Stream Analytics werden nach der Anzahl von Streamingeinheiten berechnet, die für die Verarbeitung der Daten im Dienst erforderlich sind. Weitere Informationen finden Sie unter [Azure Stream Analytics – Preise](https://azure.microsoft.com/pricing/details/stream-analytics/). 
   - **Fehlerbehandlung für Ausgabedaten**:  
     - Wiederholung: Wenn ein Fehler auftritt, wiederholt Azure Stream Analytics den Schreibvorgang für das Ereignis unbegrenzt, bis er erfolgreich ist. Für Wiederholungen gilt kein Timeout. Letztendlich wird die Verarbeitung aller nachfolgenden Ereignisse durch das Ereignis blockiert, für das Wiederholungen ausgeführt werden. Diese Option ist die Standardrichtlinie für die Ausgabefehlerbehandlung. 
     - Verwerfen: Azure Stream Analytics verwirft jedes Ausgabeereignis, das zu einem Konvertierungsfehler führt. Die verworfenen Ereignisse können nicht zur späteren erneuten Verarbeitung wiederhergestellt werden. Alle vorübergehenden Fehler (z.B. Netzwerkfehler) werden unabhängig von der Konfiguration der Richtlinien für die Ausgabefehlerbehandlung wiederholt. 
   - **Ausgabeeinstellungen für SQL-Datenbank**: Eine Option zum Erben des Partitionierungsschemas Ihres vorherigen Abfrageschrittes, um die vollständig parallele Topologie mit mehreren in die Tabelle Schreibenden zu aktivieren. Weitere Informationen finden Sie unter [Azure Stream Analytics-Ausgabe an Azure SQL-Datenbank](../stream-analytics/stream-analytics-sql-output-perf.md).
   - **Maximal zulässige Batchanzahl**: Der empfohlene obere Grenzwert für die Anzahl der Sätze, die mit jeder Transaktion zum Masseneinfügen gesendet werden.  
    Weitere Informationen zur Behandlung von Ausgabefehlern finden Sie unter [Ausgabefehlerrichtlinien in Azure Stream Analytics](../stream-analytics/stream-analytics-output-error-policy.md).  

    ![Starten des Auftrags](media/sql-database-stream-analytics/start-job.png)

8. Nachdem Sie den Auftrag gestartet haben, wird der ausgeführte Auftrag in der Liste angezeigt, und Sie können die folgenden Aktionen ausführen: 
   - **Auftrag starten/beenden**: Wenn der Auftrag ausgeführt wird, können Sie ihn beenden. Wenn der Auftrag beendet wurde, können Sie ihn starten. 
   - **Auftrag bearbeiten**: Sie können die Abfrage bearbeiten. Wenn Sie weitere Änderungen am Auftrag vornehmen möchten, fügen Sie weitere Eingaben/Ausgaben hinzu, und öffnen Sie dann den Auftrag in Stream Analytics. Bei Ausführung des Auftrags ist die Bearbeitungsoption deaktiviert. 
   - **Vorschau der Ausgabetabelle**: Sie können eine Vorschau der Tabelle im SQL-Abfrage-Editor anzeigen. 
   - **In Stream Analytics öffnen**: Öffnen Sie den Auftrag im Stream Analytics-Dienst, um Details zur Überwachung und zum Debuggen des Auftrags anzuzeigen. 


    ![Stream Analytics-Aufträge](media/sql-database-stream-analytics/jobs.png)






## <a name="next-steps"></a>Nächste Schritte

- [Azure Stream Analytics-Dokumentation](https://docs.microsoft.com/azure/stream-analytics/)
- [Lösungsmuster in Azure Stream Analytics](../stream-analytics/stream-analytics-solution-patterns.md)
