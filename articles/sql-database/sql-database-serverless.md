---
title: Azure SQL-Datenbank – serverlos (Vorschau) | Microsoft-Dokumentation
description: In diesem Artikel wird die neue serverlose Computeebene beschrieben und mit der vorhandenen bereitgestellten Computeebene verglichen.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: oslake
ms.author: moslake
ms.reviewer: sstein, carlrab
manager: craigg
ms.date: 05/20/2019
ms.openlocfilehash: 57f2c38ce0479f43d7f24de8d1feb554517bcc69
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2019
ms.locfileid: "65951486"
---
# <a name="sql-database-serverless-preview"></a>SQL-Datenbank – serverlos (Vorschau)

## <a name="what-is-the-serverless-compute-tier"></a>Was ist die serverlose Computeebene?

SQL-Datenbank – serverlos (Vorschau) ist eine Computeebene, bei der die Nutzung von Computeressourcen sekundengenau abgerechnet wird. Serverlos ist preis-/leistungsoptimiert für Einzeldatenbanken mit zeitweiligen, unvorhersehbaren Nutzungsmustern, bei denen eine gewisse Verzögerung in der Compute-Aufwärmphase nach Leerlaufzeiträumen ohne Nutzung akzeptabel ist.

Eine Datenbank in der serverlosen Computeebene wird durch den Computebereich parametrisiert, den sie nutzen kann, sowie durch eine Verzögerung durch automatisches Anhalten.

![Abrechnung – serverlos](./media/sql-database-serverless/serverless-billing.png)

### <a name="performance"></a>Leistung

- `MinVcore` und `MaxVcore` sind konfigurierbare Parametern, die den Bereich der Computekapazität definieren, die für die Datenbank verfügbar ist. Arbeitsspeicher- und E/A-Limits sind proportional zum angegebenen V-Kern-Bereich.  
- Die Verzögerung durch automatisches Anhalten ist ein konfigurierbarer Parameter, der die Zeitspanne definiert, für welche die Datenbank inaktiv sein muss, bevor sie automatisch pausiert wird. Bei der nächsten Anmeldung wird die Ausführung der Datenbank automatisch fortgesetzt.

### <a name="pricing"></a>Preise

- Die Gesamtrechnung für eine serverlose Datenbank setzt sich aus der Summe der Computerechnung und der Speicherrechnung zusammen.
Die Abrechnung für die Nutzung von Computeressourcen basiert auf der Menge der genutzten V-Kerne und des genutzten Speichers (pro Sekunde).
- Die mindestens berechneten Computeressourcen basieren auf den Minimalwerten für V-Kerne und Speicher.
- Bei angehaltener Datenbank wird lediglich Speicher in Rechnung gestellt.

## <a name="scenarios"></a>Szenarien

Serverlos ist preis-/leistungsoptimiert für Einzeldatenbanken mit zeitweiligen, unvorhersehbaren Nutzungsmustern, bei denen eine gewisse Verzögerung in der Compute-Aufwärmphase nach Leerlaufzeiträumen ohne Nutzung akzeptabel ist. Die bereitgestellte Computeebene ist dagegen preis-/leistungsoptimiert für Einzel- oder Pooldatenbanken mit höherer durchschnittlicher Nutzung, bei denen eine Verzögerung in der Compute-Aufwärmphase nicht akzeptabel ist.

### <a name="scenarios-well-suited-for-serverless-compute"></a>Ideal geeignete Szenarien für serverloses Computing

- Einzeldatenbanken mit zeitweiligen, unvorhersehbaren Nutzungsmustern, die sich mit Inaktivitätszeiträumen abwechseln, können von Kosteneinsparungen profitieren, die sich aus der sekundengenauen Abrechnung der Nutzung von Computeressourcen ergeben.
- Einzelne Datenbanken mit einem schwer prognostizierbaren Ressourcenbedarf und Kunden, die die Computeskalierung an den Dienst delegieren möchten.
- Einzelne Datenbanken in der bereitgestellten Computeebene mit laufend schwankendem Leistungspegel.

### <a name="scenarios-well-suited-for-provisioned-compute"></a>Ideal geeignete Szenarien für bereitgestelltes Computing

- Einzelne Datenbanken mit regelmäßigerer und erheblicher Nutzung von Computeressourcen über längere Zeiträume.
- Datenbanken, die keine Leistungskompromisse durch häufigeres Begrenzen des Speichers oder Verzögerung beim automatischen Fortsetzen aus dem angehaltenen Zustand tolerieren können.
- Mehrere Datenbanken mit zeitweiligen, unvorhersehbaren Nutzungsmustern, die auf einem einzigen Server konsolidiert werden können und Pools für elastische Datenbanken für bessere Preisoptimierung nutzen können.

## <a name="comparison-with-provisioned-compute-tier"></a>Vergleich mit der bereitgestellten Computeebene

Die folgende Tabelle enthält eine Zusammenfassung der Unterschiede zwischen der serverlosen Computeebene und der bereitgestellten Computeebene:

| | **Serverloses Computing**: | **Bereitgestelltes Computing** |
|:---|:---|:---|
|**Szenario für typische Verwendung**| Datenbanken mit zeitweiliger, unvorhersehbarer Nutzung, bei der inaktive Zeiträume auftreten | Datenbanken oder Pools für elastische Datenbanken mit regelmäßiger Nutzung|
| **Aufwand bei der Leistungsverwaltung** |Geringer|Höher|
|**Compute-Skalierung**|Automatisch|Manuell|
|**Compute-Reaktionsfähigkeit**|Geringer nach Inaktivitätszeiträumen|Unmittelbar|
|**Granularität bei der Abrechnung**|Pro Sekunde|Pro Stunde|

## <a name="purchasing-model-and-service-tier"></a>Kaufmodell und Dienstebene

„SQL-Datenbank – serverlos“ wird derzeit nur von der Ebene „Universell“ auf Hardware der Generation 5 im vCore-basierten Kaufmodell unterstützt.

## <a name="autoscaling"></a>Automatische Skalierung

### <a name="scaling-responsiveness"></a>Reaktionsfähigkeit hinsichtlich der Skalierung

Im Allgemeinen werden Datenbanken auf einem Computer mit ausreichender Kapazität zum unterbrechungsfreien Erfüllen des Ressourcenbedarfs für beliebige Volumen von angeforderten Computeressourcen innerhalb der Grenzen unterstützt, die durch den `maxVcores`-Wert festgelegt sind. Gelegentlich tritt automatisch ein Lastenausgleich auf, wenn der Computer den Ressourcenbedarf nicht innerhalb weniger Minuten erfüllen kann. Die Datenbank bleibt während des Lastenausgleichs online, mit Ausnahme einer kurzen Zeitspanne am Schluss des Vorgangs, wenn Verbindungen verworfen werden.

### <a name="memory-management"></a>Speicherverwaltung

Arbeitsspeicher für serverlose Datenbanken wird häufiger als bei bereitgestellten Computedatenbanken freigegeben. Dieses Verhalten ist wichtig, um für serverlose Datenbanken die Kosten kontrollieren zu können, und es kann mit einer Beeinträchtigung der Leistung verbunden sein.

#### <a name="cache-reclaiming"></a>Freigeben des Cachespeichers

Im Gegensatz zu bereitgestellten Computedatenbanken wird der Speicher aus dem SQL-Cache von einer serverlosen Datenbank freigegeben, wenn eine geringe CPU- oder Cacheauslastung vorliegt.

- Die Cacheauslastung wird als niedrig angesehen, wenn die Gesamtgröße der zuletzt verwendeten Cacheeinträge für einen bestimmten Zeitraum unter einen Schwellenwert fällt.
- Beim Auslösen der Cachefreigabe wird die Größe des Zielspeichers inkrementell auf einen Bruchteil der vorherigen Größe reduziert, und der Freigabevorgang wird nur fortgesetzt, wenn die Auslastung niedrig bleibt.
- Während der Cachefreigabe entspricht die Richtlinie für die Auswahl der zu entfernenden Cacheeinträge der Auswahlrichtlinie für bereitgestellte Computedatenbanken bei hoher Speicherauslastung.
- Der Cache wird niemals auf eine Größe verkleinert, die unter der Mindestgröße für den Arbeitsspeicher liegt. Dies wird über die Mindestanzahl von virtuellen Kernen definiert und kann entsprechend konfiguriert werden.

Sowohl in serverlosen als auch in bereitgestellten Computedatenbanken können Cacheeinträge entfernt werden, wenn der gesamte verfügbare Arbeitsspeicher verwendet wird.

#### <a name="cache-hydration"></a>Cachehydration

Der SQL-Cache wächst an, während Daten auf die gleiche Weise und mit der gleichen Geschwindigkeit wie für bereitgestellte Datenbanken vom Datenträger abgerufen werden. Wenn die Datenbank ausgelastet ist, kann die Größe des Caches uneingeschränkt bis zum maximalen Arbeitsspeichergrenzwert zunehmen.

## <a name="autopause-and-autoresume"></a>Automatisches Anhalten und automatisches Fortsetzen

### <a name="autopause"></a>Automatisches Anhalten

Das automatische Anhalten wird ausgelöst, wenn die folgenden Bedingungen für die Dauer der Verzögerung für automatisches Anhalten erfüllt sind:

- Anzahl der Sitzungen = 0
- CPU = 0 (für Benutzerworkload im Benutzerpool)

Es ist eine Option verfügbar, mit der das automatische Anhalten ggf. deaktiviert werden kann.

### <a name="autoresume"></a>Automatisches Fortsetzen

Das automatische Fortsetzen wird ausgelöst, wenn eine der folgenden Bedingungen erfüllt ist:

|Feature|Trigger für automatisches Fortsetzen|
|---|---|
|Authentifizierung und Autorisierung|Anmeldung|
|Bedrohungserkennung|Aktivieren/Deaktivieren von Einstellungen für die Bedrohungserkennung auf der Datenbank- oder Serverebene<br>Ändern von Einstellungen für die Bedrohungserkennung auf der Datenbank- oder Serverebene|
|Datenermittlung und -klassifizierung|Hinzufügen, Ändern, Löschen oder Anzeigen von Vertraulichkeitsbezeichnungen|
|Überwachung|Anzeigen von Überwachungsdatensätzen.<br>Aktualisieren oder Anzeigen von Überwachungsrichtlinien|
|Datenmaskierung|Hinzufügen, Ändern, Löschen oder Anzeigen von Datenmaskierungsregeln|
|Transparent Data Encryption|Anzeigezustand oder Status der transparenten Datenverschlüsselung|
|Abfragedatenspeicher (Leistung)|Ändern oder Anzeigen von Abfragespeichereinstellungen; automatische Optimierung|
|Automatische Optimierung|Anwendung und Überprüfung von Empfehlungen für die automatische Optimierung, z.B. die automatische Indizierung|
|Kopieren von Datenbanken|Erstellen von Datenbanken als Kopie<br>Exportieren in eine BACPAC-Datei|
|SQL-Datensynchronisierung|Die Synchronisierung zwischen Hub- und Mitgliedsdatenbanken, die nach einem konfigurierbaren Zeitplan oder manuell ausgeführt werden|
|Ändern bestimmter Datenbankmetadaten|Hinzufügen von neuen Datenbanktags<br>Ändern der Maximal- und Minimalwerte für V-Kerne und der Verzögerung für das automatische Anhalten|
|SQL Server Management Studio (SSMS)|Durch Verwendung von SSMS Version 18 und Öffnen eines neuen Abfragefensters für eine Datenbank auf dem Server wird jede automatisch angehaltene Datenbank auf dem betreffenden Server fortgesetzt. Dieses Verhalten tritt nicht auf, wenn SSMS Version 17.9.1 verwendet wird und IntelliSense deaktiviert ist.|

### <a name="connectivity"></a>Konnektivität

Wenn eine serverlose Datenbank angehalten wird, wird die Datenbank bei der ersten Anmeldung fortgesetzt, und es wird ein Fehler (Fehlercode 40613) mit dem Hinweis zurückgegeben, dass die Datenbank nicht verfügbar ist. Sobald die Datenbank fortgesetzt wird, muss die Anmeldung wiederholt werden, um die Verbindung herzustellen. Datenbankclients mit Wiederholungslogik für Verbindungen dürfen nicht geändert werden.

### <a name="latency"></a>Latency

Die Latenz für das automatische Anhalten oder das automatische Fortsetzen einer serverlosen Datenbank bewegt sich generell in der Größenordnung von 1 Minute.

### <a name="feature-support"></a>Featureunterstützung

Das automatische Anhalten und das automatische Fortsetzen werden von den folgenden Features nicht unterstützt. Das heißt, bei Verwendung eines der folgenden Features bleibt die Datenbank online, ungeachtet der Dauer der Inaktivität der Datenbank:

- Georeplikation (aktive Georeplikation und Gruppen für automatisches Failover)
- Langzeitaufbewahrung (Long-Term Retention, LTR) von Sicherungen
- In SQL-Datensynchronisierung verwendete Synchronisierungsdatenbank


## <a name="on-boarding-into-the-serverless-compute-tier"></a>Integration in die serverlose Computeebene

Beim Erstellen einer neuen Datenbank bzw. Verschieben einer vorhandenen Datenbank in eine serverlose Computeebene gilt dasselbe Muster wie beim Erstellen einer neuen Datenbank in der bereitgestellten Computeebene; dieser Vorgang umfasst die folgenden zwei Schritte:

1. Geben Sie den Namen des Dienstziels an. Das Dienstziel schreibt die Dienstebene, die Hardwaregeneration und die maximale Anzahl von V-Kernen vor. Die folgende Tabelle enthält die verschiedenen Optionen für Dienstziele:

   |Name des Dienstziels|Dienstebene|Hardwaregeneration|Maximale Anzahl von V-Kernen|
   |---|---|---|---|
   |GP_S_Gen5_1|Allgemeiner Zweck|Gen5|1|
   |GP_S_Gen5_2|Allgemeiner Zweck|Gen5|2|
   |GP_S_Gen5_4|Allgemeiner Zweck|Gen5|4|

2. Geben Sie optional die Mindestanzahl virtueller Kerne und die Verzögerung für das automatische Anhalten an, um deren Standardwerte zu ändern. In der folgenden Tabelle werden die verfügbaren Werte für diese Parameter aufgeführt.

   |Parameter|Auswahlmöglichkeiten für Werte|Standardwert|
   |---|---|---|---|
   |Mindestanzahl virtueller Kerne|Beliebiger Wert aus {0,5, 1, 2, 4}, darf Maximalwert für V-Kerne nicht überschreiten|0,5 V-Kerne|
   |Verzögerung für das automatische Anhalten|Min: 360 Minuten (sechs Stunden)<br>Max: 10.080 Minuten (sieben Tage)<br>Inkremente: 60 Minuten<br>Automatisches Anhalten deaktivieren: -1|360 Minuten|

> [!NOTE]
> Das Verschieben einer vorhandenen Datenbank in eine serverlose Computeebene oder das Ändern der Computegröße mithilfe von T-SQL wird derzeit nicht unterstützt. Diese Vorgänge können jedoch über das Azure-Portal oder PowerShell ausgeführt werden.

### <a name="create-new-database-using-the-azure-portal"></a>Erstellen einer neuen Datenbank über das Azure-Portal

Weitere Informationen finden Sie unter [Schnellstart: Erstellen einer Einzeldatenbank in Azure SQL-Datenbank über das Azure-Portal](sql-database-single-database-get-started.md).

### <a name="create-new-database-using-powershell"></a>Erstellen einer neuen Datenbank mithilfe von PowerShell

Im folgenden Beispiel wird eine neue Datenbank in der serverlosen Computeebene erstellt, die durch das Dienstziel GP_S_Gen5_4 definiert wird; es werden Standardwerte für die Mindestanzahl virtueller Kerne und die Verzögerung für das automatische Anhalten angegeben.

Für serverloses Computing ist eine neuere Version von PowerShell erforderlich, als derzeit im Katalog vorhanden ist; führen Sie daher `Update-Module Az.Sql` aus, um die aktuellen serverlos-fähigen Cmdlets abzurufen.

```powershell
New-AzSqlDatabase `
  -ResourceGroupName $resourceGroupName `
  -ServerName $serverName `
  -DatabaseName $databaseName `
  -ComputeModel Serverless `
  -Edition GeneralPurpose `
  -ComputeGeneration Gen5 `
  -MinVcore 0.5 `
  -MaxVcore 2 `
  -AutoPauseDelay 720
```

### <a name="move-existing-database-into-the-serverless-compute-tier"></a>Verschieben einer vorhandenen Datenbank in die serverlose Computeebene

Im folgenden Beispiel wird eine vorhandene einzelne Datenbank aus der bereitgestellten Computeebene in die serverlose Computeebene verschoben. In diesem Beispiel werden die Mindestanzahl virtueller Kerne, die maximale Anzahl virtueller Kerne und die Verzögerung für das automatische Anhalten explizit angegeben.

```powershell
Set-AzSqlDatabase
  -ResourceGroupName $resourceGroupName `
  -ServerName $serverName `
  -DatabaseName $databaseName `
  -Edition GeneralPurpose `
  -ComputeModel Serverless `
  -ComputeGeneration Gen5 `
  -MinVcore 1 `
  -MaxVcore 4 `
  -AutoPauseDelay 1440
```

### <a name="move-a-database-out-of-the-serverless-compute-tier"></a>Verschieben einer Datenbank aus der serverlosen Computeebene

Eine serverlose Datenbank kann auf die gleiche Weise in eine bereitgestellte Computeebene verschoben werden wie eine bereitgestellte Datenbank in eine serverlose Computeebene.

## <a name="modify-serverless-configuration-parameters"></a>Ändern von Konfigurationsparametern für serverloses Computing

### <a name="maximum-vcores"></a>Maximale Anzahl virtueller Kerne

Das Ändern der maximalen Anzahl virtueller Kerne erfolgt durch Ausführen des Befehls [Set-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabase) in PowerShell mit dem `MaxVcore`-Argument.

### <a name="minimum-vcores"></a>Mindestanzahl virtueller Kerne

Das Ändern der minimalen Anzahl von virtuellen Kernen erfolgt durch Ausführen des Befehls [Set-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabase) in PowerShell mit dem `MinVcore`-Argument.

### <a name="autopause-delay"></a>Verzögerung für das automatische Anhalten

Das Ändern der Verzögerung für das automatische Anhalten erfolgt durch Ausführen des Befehls [Set-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabase) in PowerShell mit dem `AutoPauseDelay`-Argument.

## <a name="monitor-serverless-database"></a>Serverlose Datenbank überwachen

### <a name="resources-used-and-billed"></a>Genutzte und berechnete Ressourcen

Die Ressourcen einer serverlosen Datenbank werden durch die folgenden Einheiten gekapselt:

#### <a name="app-package"></a>App-Paket

Das App-Paket ist die „Außengrenze“ der Ressourcenverwaltung für eine Datenbank, wobei es keine Rolle spielt, ob sich die Datenbank auf einer serverlosen oder einer bereitgestellten Computeebene befindet. Das App-Paket enthält die SQL-Instanz und externe Dienste, die zusammen sämtliche Benutzer- und Systemressourcen umfassen, die von einer Datenbank in SQL-Datenbank genutzt werden. Beispiele für externe Dienste sind R und die Volltextsuche. Die SQL-Instanz bestimmt generell die allgemeine Ressourcennutzung für das gesamte App-Paket.

#### <a name="user-resource-pool"></a>Benutzerressourcenpool

Der Benutzerressourcenpool ist die „Innengrenze“ der Ressourcenverwaltung für eine Datenbank, wobei es keine Rolle spielt, ob sich die Datenbank auf einer serverlosen oder einer bereitgestellten Computeebene befindet. Der Benutzerressourcenpool beschränkt CPU und E/A für Benutzerworkload, die von DDL-Abfragen (z.B. CREATE, ALTER usw.) und DML-Abfragen (z.B. SELECT, INSERT, UPDATE, DELETE usw.) generiert wird. Diese Abfragen sind im Allgemeinen für den Großteil der Auslastung des App-Pakets verantwortlich.

### <a name="metrics"></a>metrics

|Entität|Metrik|BESCHREIBUNG|Units|
|---|---|---|---|
|App-Paket|app_cpu_percent|Prozentsatz der von der App genutzten virtuellen Kerne, bezogen auf die maximal zulässigen virtuellen Kerne für die App.|Prozentsatz|
|App-Paket|app_cpu_billed|Die Menge der Computeressourcen, die im Berichtszeitraum für die App abgerechnet wurden. Der während dieses Zeitraums zu zahlende Betrag ist das Produkt aus dieser Metrik und dem Einzelpreis für virtuelle Kerne. <br><br>Werte dieser Metrik werden bestimmt, indem der maximal genutzte Arbeitsspeicher und der pro Sekunde genutzte Speicher über einen Zeitraum aggregiert werden. Liegt die genutzte Menge unter der bereitgestellten Mindestmenge (festgelegt durch Mindestanzahl virtueller Kerne und Minimalwert für Speicher), wird die bereitgestellte Mindestmenge berechnet. Der Arbeitsspeicher wird in Einheiten aus virtuellen Kernen normalisiert, indem der Arbeitsspeicher in GB nach 3 GB pro virtuellem Kern neu skaliert wird. So kann die CPU bei der Abrechnung mit dem Arbeitsspeicher verglichen werden.|Virtueller Kern – Sekunden|
|App-Paket|app_memory_percent|Prozentsatz des von der App genutzten Speichers, bezogen auf den maximal zulässigen Speicher für die App.|Prozentsatz|
|Benutzerpool|cpu_percent|Prozentsatz der von der Benutzerworkload genutzten virtuellen Kerne, bezogen auf die maximal zulässigen virtuellen Kerne für die Benutzerworkload.|Prozentsatz|
|Benutzerpool|data_IO_percent|Prozentsatz der von der Benutzerworkload genutzten Daten-IOPS, bezogen auf die maximal zulässige Daten-IOPS für die Benutzerworkload.|Prozentsatz|
|Benutzerpool|log_IO_percent|Prozentsatz der von der Benutzerworkload genutzten Protokollrate (MB/s), bezogen auf die maximal zulässige Protokollrate (MB/s) für die Benutzerworkload.|Prozentsatz|
|Benutzerpool|workers_percent|Prozentsatz der von der Benutzerworkload genutzten Worker, bezogen auf die maximal zulässige Anzahl von Workern für die Benutzerworkload.|Prozentsatz|
|Benutzerpool|sessions_percent|Prozentsatz der von der Benutzerworkload genutzten Sitzungen, bezogen auf die maximal zulässige Anzahl von Sitzungen für die Benutzerworkload.|Prozentsatz|
____

> [!NOTE]
> Metriken sind im Azure-Portal im Datenbankbereich für eine einzelne Datenbank unter **Überwachung** verfügbar.

### <a name="pause-and-resume-status"></a>Status für Anhalten und Fortsetzen

Im Azure-Portal wird der Datenbankstatus im Übersichtsbereich des Servers angezeigt, in dem die enthaltenen Datenbanken aufgelistet werden. Der Status der Datenbank wird auch im Übersichtsbereich für die Datenbank angezeigt.

Fragen Sie den Status für Anhalten und Fortsetzen einer Datenbank mit dem folgenden PowerShell-Befehl ab:

```powershell
Get-AzSqlDatabase `
  -ResourceGroupName $resourcegroupname `
  -ServerName $servername `
  -DatabaseName $databasename `
  | Select -ExpandProperty "Status"
```

## <a name="resource-limits"></a>Ressourceneinschränkungen

Ressourceneinschränkungen werden unter [Serverlose Computeebene](sql-database-vCore-resource-limits-single-databases.md#serverless-compute-tier) beschrieben.

## <a name="billing"></a>Abrechnung

Die abgerechnete Computeleistung basiert auf der maximal verwendeten CPU und dem verwendeten Arbeitsspeicher (pro Sekunde). Wenn die verwendete CPU und der verwendete Arbeitsspeicher kleiner als die bereitgestellte Mindestmenge sind, wird die bereitgestellte Menge abgerechnet. Der Arbeitsspeicher wird in Einheiten aus virtuellen Kernen normalisiert, indem der Arbeitsspeicher in GB nach 3 GB pro virtuellem Kern neu skaliert wird. So kann die CPU bei der Abrechnung mit dem Arbeitsspeicher verglichen werden.

- **Berechnete Ressource**: CPU und Arbeitsspeicher
- **Berechneter Betrag ($)** : Einzelpreis virtueller Kern * Max. (Min. virtuelle Kerne, genutzte virtuelle Kerne, Min. Speicher GB * 1/3, genutzter Speicher GB * 1/3) 
- **Fakturierungsintervall**: Pro Sekunde

Der Einzelpreis für virtuelle Kerne ergibt sich aus den Kosten pro virtuellem Kern pro Sekunde. Informationen zu Einzelpreisen in einer bestimmten Region finden Sie auf der Seite [Azure SQL-Datenbank – Preise ](https://azure.microsoft.com/pricing/details/sql-database/single/).

Die genutzte Computekapazität wird mit der folgenden Metrik angegeben:

- **Metrik**: app_cpu_billed (virtueller Kern – Sekunden)
- **Definition**: Max. (min. virtuelle Kerne, genutzte virtuelle Kerne, min. Speicher GB · 1/3, genutzter Speicher GB · 1/3)
- **Berichtsfrequenz**: Pro Minute

Diese Menge wird pro Sekunde berechnet und über eine Minute aggregiert.

Erwägen Sie die Verwendung einer serverlosen Datenbank, für die für virtuelle Kerne ein Mindestwert von 1 und ein Höchstwert von 4 konfiguriert ist.  Dies entspricht ungefähr einem Arbeitsspeicher von mindestens 3 GB und maximal 12 GB.  Angenommen, die Verzögerung für automatisches Anhalten ist auf sechs Stunden festgelegt, und die Datenbankworkload ist während der ersten beiden Stunden eines Zeitraums von 24 Stunden aktiv und ansonsten inaktiv.    

In diesem Fall werden für die Datenbank Compute- und Speicherkosten während der ersten acht Stunden berechnet.  Die Datenbank ist zwar nach der zweiten Stunde inaktiv, aber für die nachfolgenden sechs Stunden werden basierend auf der minimalen bereitgestellten Computeleistung trotzdem noch Computekosten berechnet, während die Datenbank online ist.  Während die Datenbank angehalten ist, werden in der restlichen Zeit des 24-Stunden-Zeitraums nur Kosten für den Speicher berechnet.

Die genaue Berechnung der Computekosten für dieses Beispiel lautet:

|Zeitintervall|Pro Sekunde verwendete virtuelle Kerne|Pro Sekunde verwendete GB|Berechnete Computedimension|Für Zeitintervall berechnete Sekunden für virtuelle Kerne|
|---|---|---|---|---|
|0:00 - 1:00|4|9|Verwendete virtuelle Kerne|4 virtuelle Kerne · 3.600 Sekunden = 14.400 Sekunden für virtuelle Kerne|
|1:00 - 2:00|1|12|Verwendeter Arbeitsspeicher|12 GB · 1/3 · 3.600 Sekunden = 14.400 Sekunden für virtuelle Kerne|
|2:00 - 8:00|0|0|Mindestens bereitgestellter Arbeitsspeicher|3 GB · 1/3 · 21.600 Sekunden = 21.600 Sekunden für virtuelle Kerne|
|8:00 - 24:00|0|0|Keine Berechnung von Computeleistung während des Anhaltens|0 Sekunden für virtuelle Kerne|
|Gesamte berechnete Sekunden für virtuelle Kerne in 24 Stunden||||50.400 Sekunden für virtuelle Kerne|

Angenommen, der Compute-Einzelpreis beträgt 0,000073 USD/V-Kern/Sekunde.  Die Computeleistung, die für diesen 24-Stunden-Zeitraum berechnet wird, ist dann das Produkt aus dem Preis der Compute-Einheit und den berechneten Sekunden für virtuelle Kerne: 0,000073 USD/V-Kern/Sekunde · 50.400 Sekunden für virtuelle Kerne = 3,68 USD

## <a name="available-regions"></a>Verfügbare Regionen

Die serverlose Computeebene ist in allen Regionen verfügbar, mit Ausnahme der folgenden Regionen: Australien, Mitte, China, Osten, China, Norden, Frankreich, Süden, Deutschland, Mitte, Deutschland, Nordosten, Indien, Westen, Südkorea, Süden, Südafrika, Westen, Vereinigtes Königreich, Norden, Vereinigtes Königreich, Süden, Vereinigtes Königreich, Westen und USA, Westen-Mitte

## <a name="next-steps"></a>Nächste Schritte

- Informationen zu den ersten Schritten finden Sie unter [Schnellstart: Erstellen einer Einzeldatenbank in Azure SQL-Datenbank über das Azure-Portal](sql-database-single-database-get-started.md).
- Ressourceneinschränkungen werden unter [Ressourceneinschränkungen für die serverlose Computeebene](sql-database-vCore-resource-limits-single-databases.md#serverless-compute-tier) beschrieben.
