---
title: Advanced Data Security für IaaS in Azure Security Center | Microsoft-Dokumentation
description: " Erfahren Sie, wie Sie Advanced Data Security für IaaS in Azure Security Center aktivieren. "
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: ba46c460-6ba7-48b2-a6a7-ec802dd4eec2
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/04/2019
ms.author: memildin
ms.openlocfilehash: 93e52b393db288f5b19afde4a31e08d0bb91b471
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73571559"
---
# <a name="advanced-data-security-for-sql-servers-on-azure-virtual-machines-preview"></a>Erweiterte Datensicherheit für SQL Server-Instanzen in Azure Virtual Machines (Vorschau)
Advanced Data Security für SQL Server-Instanzen auf Azure Virtual Machines ist ein einheitliches Paket für erweiterte SQL-Sicherheitsfunktionen. Zu dieser Previewfunktion zählen aktuell die Funktionen zur Ermittlung und Verringerung potenzieller Datenbankschwachstellen und Erkennung ungewöhnlicher Aktivitäten, die Bedrohungen für Ihre Datenbank darstellen können. 

Dieses Sicherheitsangebot für SQL Server-Instanzen auf Azure-VMs basiert auf der gleichen grundlegenden Technologie, die auch im [Advanced Data Security-Paket für Azure SQL-Datenbank](https://docs.microsoft.com/azure/sql-database/sql-database-advanced-data-security) verwendet wird.


## <a name="overview"></a>Übersicht

Advanced Data Security stellt eine Reihe erweiterter SQL-Sicherheitsfunktionen bereit, die eine Sicherheitsrisikobewertung und Advanced Threat Protection umfassen.

* [Sicherheitsrisikobewertung](https://docs.microsoft.com/azure/sql-database/sql-vulnerability-assessment) ist ein einfach zu konfigurierender Dienst, mit dem potenzielle Schwachstellen in der Datenbank ermittelt, nachverfolgt und behoben werden können. Er bietet Einblicke in Ihren Sicherheitsstatus, enthält die Schritte zum Beheben von Sicherheitsproblemen und verbessert Ihre Datenbanksicherheit.
* [Advanced Threat Protection](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-overview) erkennt Anomalien bei Aktivitäten, die auf ungewöhnliche und potenziell schädliche Versuche hinweisen, auf SQL Server zuzugreifen oder diesen zu nutzen. Sie überwacht Ihre Datenbank fortlaufend auf verdächtige Aktivitäten und stellt handlungsorientierte Sicherheitswarnungen bei anomalen Datenbankzugriffsmustern bereit. Diese Warnungen enthalten Details zur verdächtigen Aktivität sowie empfohlene Maßnahmen zur Untersuchung und Abwehr der Bedrohung.

## <a name="get-started-with-advanced-data-security-for-sql-on-azure-vms"></a>Erste Schritte mit Advanced Data Security für SQL auf Azure Virtual Machines

Die folgenden Schritte dienen als Einstieg in Advanced Data Security für SQL für Azure-VMs (Public Preview).

### <a name="set-up-advanced-data-security-for-sql-on-azure-vms"></a>Einrichten von Advanced Data Security für SQL auf Azure Virtual Machines

Aktivieren von Advanced Data Security für SQL Server-Instanzen auf Azure Virtual Machines auf Abonnement-/Arbeitsbereichebene:
 
1. Öffnen Sie auf der Security Center-Randleiste die Seite **Preise und Einstellungen**.

1. Wählen Sie das Abonnement oder den Arbeitsbereich aus, für das bzw. den Sie Advanced Data Security für SQL auf Azure Virtual Machines aktivieren möchten.

1. Aktivieren Sie die Option **SQL Server-Instanzen auf VMs (Vorschau)** . 

    (Klicken Sie auf den Screenshot, um ihn zu erweitern.)

    [![Security Center-Empfehlungen und -Warnungen in Windows Admin Center](media/security-center-advanced-iaas-data/sql-servers-on-vms-in-pricing-small.png)](media/security-center-advanced-iaas-data/sql-servers-on-vms-in-pricing-large.png#lightbox)

    Advanced Data Security für SQL Server-Instanzen wird auf allen SQL Server-Instanzen aktiviert, die mit dem ausgewählten Arbeitsbereich oder dem Standardarbeitsbereich des ausgewählten Abonnements verbunden sind.

    >[!NOTE]
    > Die Lösung ist nach dem ersten Neustart der SQL Server-Instanz aktiv. 

Um einen neuen Arbeitsbereich zu erstellen, befolgen Sie die Anweisungen unter [Erstellen eines Log Analytics-Arbeitsbereichs](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace).

Um den SQL Server-Host mit einem Arbeitsbereich zu verbinden, befolgen Sie die Anweisungen unter [Verbinden von Windows-Computern mit Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/agent-windows).


## <a name="set-up-email-notification-for-atp-alerts"></a>Einrichten einer E-Mail-Benachrichtigung für ATP-Warnungen 

Sie können eine Liste von Empfängern festlegen, die eine E-Mail-Benachrichtigung erhalten sollen, wenn Security Center-Warnungen generiert werden. Die E-Mail enthält einen direkten Link zur Warnung in Azure Security Center mit allen relevanten Details. 

1. Navigieren Sie zu **Security Center** > **Preise und Einstellungen**, und klicken Sie auf das entsprechende Abonnement.

    ![Abonnementeinstellungen](./media/security-center-advanced-iaas-data/subscription-settings.png)

1. Klicken Sie im Menü „Einstellungen“ auf **E-Mail-Benachrichtigungen**. 
1. Geben Sie im Textfeld **E-Mail-Adresse** die E-Mail-Adressen ein, die Benachrichtigungen erhalten sollen. Sie können mehrere E-Mail-Adresse eingeben, indem Sie die einzelnen Adressen durch ein Komma (,) trennen.  Beispiel: admin1@mycompany.com,admin2@mycompany.com,admin3@mycompany.com

      ![E-Mail-Einstellungen](./media/security-center-advanced-iaas-data/email-settings.png)

1. Legen Sie unter den **Einstellungen für die E-Mail-Benachrichtigung** die folgenden Optionen fest:
  
    * **E-Mail-Benachrichtigung für Warnungen mit hohem Schweregrad senden**: Statt E-Mails für alle Warnungen zu senden, werden nur welche für Warnungen mit hohem Schweregrad gesendet.
    * **E-Mail-Benachrichtigung auch an Abonnementbesitzer senden**:  Es werden auch Benachrichtigungen an die Abonnementbesitzer gesendet.

1. Klicken Sie im oberen Bereich des Bildschirms **E-Mail-Benachrichtigungen** auf **Speichern**.

  > [!NOTE]
  > Achten Sie darauf, dass Sie vor dem Schließen des Fensters auf **Speichern** klicken, da sonst die neuen **Einstellungen für die E-Mail-Benachrichtigung** nicht gespeichert werden.

## <a name="explore-vulnerability-assessment-reports"></a>Untersuchen der Berichte zur Sicherheitsrisikobewertung

Das Dashboard zur Sicherheitsrisikobewertung bietet eine Übersicht über die Bewertungsergebnisse für alle Datenbanken. Sie können die Verteilung von Datenbanken nach SQL Server-Version sowie eine Zusammenfassung der fehlerhaften gegenüber einwandfreien Datenbanken und eine allgemeine Übersicht über fehlerhafte Prüfungen nach Risikoverteilung anzeigen.

Sie können die Ergebnisse und Berichte zur Sicherheitsrisikobewertung direkt aus Log Analytics heraus anzeigen.

1. Navigieren Sie zu Ihrem Log Analytics-Arbeitsbereich mit der Advanced Data Security-Lösung.
1. Navigieren Sie zu **Lösungen**, und wählen Sie die Lösung **SQL-Sicherheitsrisikobewertung** aus.
1. Klicken Sie im Bereich **Zusammenfassung** auf **Zusammenfassung anzeigen**, und wählen Sie Ihren **Bericht zur SQL-Sicherheitsrisikobewertung** aus.

    ![SQL-Bewertungsbericht](./media/security-center-advanced-iaas-data/ads-sql-server-1.png)

    Das Berichtsdashboard wird geladen. Stellen Sie sicher, dass das Zeitfenster mindestens auf **Letzte 7 Tage** festgelegt ist, da Sicherheitsrisikoüberprüfungen für Ihre Datenbanken nach einem festen Zeitplan einmal alle sieben Tage ausgeführt werden.

    ![Letzte 7 Tage festlegen](./media/security-center-advanced-iaas-data/ads-sql-server-2.png)

1. Um weitere Details anzuzeigen, klicken Sie auf eines der Dashboardelemente. Beispiel:

   1. Klicken Sie im Abschnitt mit der Zusammenfassung zu Überprüfungen mit Fehlern auf eine Sicherheitsüberprüfung, um eine Log Analytics-Tabelle mit den Ergebnissen für diese Überprüfung bei allen Datenbanken anzuzeigen **.** Die Überprüfungen mit Ergebnissen werden zuerst aufgeführt.

   1. Klicken Sie dann durch die Liste, um Details zu jedem Sicherheitsrisiko anzuzeigen. Dazu gehören die Beschreibung des Sicherheitsrisikos sowie Auswirkung, Status, verbundenes Risiko und tatsächliche Ergebnisse für diese Datenbank. Sie können auch die eigentliche Abfrage, die für diese Überprüfung ausgeführt wurde, und Informationen zur Beseitigung dieses Sicherheitsrisikos anzeigen.

    ![Arbeitsbereich auswählen](./media/security-center-advanced-iaas-data/ads-sql-server-3.png)

    ![Arbeitsbereich auswählen](./media/security-center-advanced-iaas-data/ads-sql-server-4.png)

1. Sie können beliebige Log Analytics-Abfragen für die Ergebnisdaten Ihrer Sicherheitsrisikobewertung ausführen, um die Daten entsprechend Ihren Anforderungen aufzuteilen.

## <a name="advanced-threat-protection-for-sql-servers-on-azure-vms-alerts"></a>Warnungen von Advanced Threat Protection für SQL Server-Instanzen auf Azure Virtual Machines
Warnungen werden bei ungewöhnlichen und potenziell schädlichen Zugriffsversuchen oder Exploit-Vorgängen für SQL Server-Instanzen generiert. Diese Ereignisse können die folgenden Warnungen auslösen:

### <a name="anomalous-access-pattern-alerts-preview"></a>Warnungen zu ungewöhnlichen Zugriffsmustern (Vorschau)

* **Zugriff von einem ungewöhnlichen Ort**: Diese Warnung wird ausgelöst, wenn eine Änderung des Zugriffsmusters für SQL Server erfolgt ist, weil sich eine Person von einem ungewöhnlichen Ort aus bei SQL Server angemeldet hat. Mögliche Ursachen:
    * Ein Angreifer oder böswilliger ehemaliger Mitarbeiter hat auf Ihren SQL Server zugegriffen.
    * Ein berechtigter Benutzer hat von einem neuen Ort aus auf Ihren SQL Server zugegriffen.
* **Zugriff über eine potenziell schädliche Anwendung:** Diese Warnung wird ausgelöst, wenn zum Zugreifen auf die Datenbank eine potenziell schädliche Anwendung verwendet wird. Mögliche Ursachen:
    * Ein Angreifer versucht, mit gebräuchlichen Angriffstools Zugriff auf Ihren SQL Server zu erhalten.
    * Ein legitimer Penetrationstest wird ausgeführt.
* **Zugriff über einen unbekannten Prinzipal:** Diese Warnung wird ausgelöst, wenn eine Änderung des Zugriffsmusters für SQL Server erfolgt ist, weil sich eine Person über einen ungewöhnlichen Prinzipal (SQL-Benutzer) bei SQL Server angemeldet hat. Mögliche Ursachen:
    * Ein Angreifer oder böswilliger ehemaliger Mitarbeiter hat auf Ihren SQL Server zugegriffen. 
    * Ein berechtigter Benutzer hat mit einem neuen Prinzipal auf Ihren SQL Server zugegriffen.
* **Brute-Force-Angriff auf SQL-Anmeldeinformationen:** Diese Warnung wird ausgelöst, wenn eine ungewöhnlich hohe Anzahl von fehlerhaften Anmeldungen mit unterschiedlichen Anmeldeinformationen vorliegt. Mögliche Ursachen:
    * Ein Angreifer versucht, mit einem Brute-Force-Angriff Zugriff auf Ihren SQL Server zu erhalten.
    * Ein legitimer Penetrationstest wird ausgeführt.

### <a name="potential-sql-injection-attacks-supported-in-sql-server-2019"></a>Mögliche Angriffe mit Einschleusung von SQL-Befehlen (unterstützt in SQL Server 2019)

* **Anfälligkeit für die Einschleusung von SQL-Befehlen**: Diese Warnung wird ausgelöst, wenn eine Anwendung in der Datenbank eine fehlerhafte SQL-Anweisung generiert. Diese Warnung kann auf ein mögliches Sicherheitsrisiko in Bezug auf Angriffe mit Einschleusung von SQL-Befehlen hinweisen. Mögliche Ursachen:
    * Ein Fehler im Anwendungscode, der zur fehlerhaften SQL-Anweisung führt
    * Anwendungscode oder gespeicherte Prozeduren führen bei der Erstellung der fehlerhaften SQL-Anweisung keine Bereinigung der Benutzereingabe durch, und dies kann für eine Einschleusung von SQL-Befehlen ausgenutzt werden
* **Potenzielle Einschleusung von SQL-Befehlen:** Diese Warnung wird ausgelöst, wenn ein aktiver Exploit für ein identifiziertes Anwendungssicherheitsrisiko in Bezug auf die Einschleusung von SQL-Befehlen besteht. Dies bedeutet, dass der Angreifer versucht, schädliche SQL-Anweisungen einzuschleusen, indem er den anfälligen Anwendungscode bzw. die gespeicherten Prozeduren verwendet.


### <a name="unsafe-command-supported-in-sql-server-2019"></a>Unsicherer Befehl (unterstützt in SQL Server 2019)

* **Potenziell unsichere Aktion**: Diese Warnung wird ausgelöst, wenn ein hoch privilegierter und potenziell unsicherer Befehl ausgeführt wird. Mögliche Ursachen:
    * Der Befehl, dessen Deaktivierung empfohlen wird, um einen besseren Sicherheitsstatus zu erreichen, ist aktiviert.
    * Ein Angreifer, der versucht, den SQL-Zugriff auszunutzen oder Berechtigungen zu eskalieren.   


## <a name="explore-and-investigate-security-alerts"></a>Erkunden und Untersuchen von Sicherheitswarnungen

Die Datensicherheitswarnungen sind auf der Security Center-Seite „Warnungen“, auf der Registerkarte „Sicherheit“ der Ressource oder über den direkten Link in den Benachrichtigungs-E-Mails verfügbar.

1. So zeigen Sie Warnungen an:

    * Im Security Center: Klicken Sie in der Randleiste auf **Sicherheitswarnungen**, und wählen Sie eine Warnung aus.
    * Im Ressourcenbereich: Öffnen Sie die relevante Ressourcenseite, und klicken Sie in der Randleiste auf **Sicherheit**. 

1. Warnungen sind eigenständig und enthalten jeweils ausführliche Schritte zur Bereinigung und Untersuchungsinformationen. Sie können weitere Untersuchungen durchführen, indem Sie andere Azure Security Center- und Azure Sentinel-Funktionen für eine umfassendere Ansicht verwenden:

    * Aktivieren Sie die Überwachungsfunktion von SQL Server, um weitere Untersuchungen durchzuführen. Wenn Sie Azure Sentinel-Benutzer sind, können Sie die SQL-Überwachungsprotokolle aus den Windows-Sicherheitsprotokollereignissen in Sentinel hochladen, wo Ihnen umfassende Untersuchungsfunktionen zur Verfügung stehen.
    * Um Ihren Sicherheitsstatus zu verbessern, verwenden Sie die in jeder Warnung aufgeführten Security Center-Empfehlungen für den Hostcomputer. Dadurch werden die Risiken zukünftiger Angriffe reduziert. 


## <a name="next-steps"></a>Nächste Schritte

Verwandte Informationen finden Sie im folgenden Artikel:

- [Umsetzen von Empfehlungen](security-center-remediate-recommendations.md)