---
title: Gruppieren von Computern mithilfe von Computerabhängigkeiten per Azure Migrate | Microsoft-Dokumentation
description: Hier wird beschrieben, wie Sie mithilfe von Computerabhängigkeiten mit dem Azure Migrate-Dienst eine Bewertung erstellen.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 10/01/2019
ms.author: hamusa
ms.openlocfilehash: 1cd1ff83fd706e3474426f6cc2ac99d39e74dc22
ms.sourcegitcommit: 42748f80351b336b7a5b6335786096da49febf6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2019
ms.locfileid: "72177945"
---
# <a name="set-up-dependency-visualization-for-assessment"></a>Einrichten einer Visualisierung der Abhängigkeit für die Bewertung

Dieser Artikel beschreibt, wie Sie die Zuordnung von Abhängigkeiten in Azure Migrate: Server Assessment“ (Azure Migrate-Serverbewertung) erstellen.

Mit der Abhängigkeitszuordnung können Sie Abhängigkeiten computerübergreifend visualisieren, die Sie bewerten und migrieren möchten.

- In Azure Migrate: Serverbewertung die Computer, die Sie bewerten möchten. In der Regel sind das Computer, die Sie gemeinsam migrieren möchten.
- Sie verwenden die Abhängigkeitszuordnung normalerweise, wenn Sie auf Gruppen mit höheren Zuverlässigkeitsgraden zugreifen möchten.
- Die Zuordnung der Abhängigkeiten ist hilfreich, die Abhängigkeiten zwischen Computern zu überprüfen, bevor Sie eine Bewertung durchführen.
- Die Zuordnung und Visualisierung von Abhängigkeiten hilft Ihnen, Ihre Migration nach Azure effektiv zu planen. Dies trägt dazu bei, dass nichts zurückbleibt und vermeidet Überraschungsausfälle während der Migration.
- Mit der Zuordnung können Sie alle unabhängigen Systeme ermitteln, die zusammen migriert werden müssen. Sie können bestimmen, ob ein ausgeführtes System noch benötigt wird oder eher außer Betrieb gesetzt als migriert werden sollte.

[Erfahren Sie mehr](concepts-dependency-visualization.md#how-does-it-work) über die Visualisierung von Abhängigkeiten.

## <a name="before-you-start"></a>Vorbereitung

- Stellen Sie sicher, dass Sie ein Azure Migrate-Projekt [erstellt](how-to-add-tool-first-time.md) haben.
- Wenn Sie bereits ein Projekt erstellt haben, vergewissern Sie sich, dass Sie das Tool Azure Migrate-Serverbewertung[hinzugefügt](how-to-assess.md): Migrate-Serverbewertung bewerten.
- Stellen Sie sicher, dass Sie Ihre Computer in Azure Migrate ermittelt haben. Sie können dies tun, indem Sie eine Azure Migrate-Appliance für [VMware](how-to-set-up-appliance-vmware.md) oder [Hyper-V](how-to-set-up-appliance-hyper-v.md) einrichten. Die Appliance ermittelt lokale Computer und sendet Metadaten und Leistungsdaten an Azure Migrate: Server Assessment“ (Azure Migrate-Serverbewertung) erstellen. [Weitere Informationen](migrate-appliance.md)


**Funktionen** | **Hinweis**
--- | ---
Verfügbarkeit | Abhängigkeitsvisualisierung ist in Azure Government nicht verfügbar.
Dienstzuordnung | Die Visualisierung von Abhängigkeiten verwendet die Dienstzuordnungslösung in Azure Monitor. [Dienstzuordnung](../azure-monitor/insights/service-map.md) erkennt Verbindungen zwischen Servern und zeigt sie automatisch an.
Agents | Um die Visualisierung von Abhängigkeiten zu verwenden, installieren Sie die folgenden Agents auf den Computern, die Sie zuordnen möchten:<br/> - [Log Analytics-Agent](../azure-monitor/platform/log-analytics-agent.md) (früher Microsoft Monitoring Agent (MMA) genannt).<br/> - [Abhängigkeits-Agent für Dienstzuordnung](../azure-monitor/insights/vminsights-enable-overview.md#the-microsoft-dependency-agent).<br/><br/> Um die Installation von Agents zu automatisieren, können Sie Bereitstellungstools wie System Center Configuration Manager verwenden, die eine Agent-Bereitstellungslösung für Azure Migrate bereitstellen.
Abhängigkeits-Agent | Überprüfen Sie die [Unterstützung des Abhängigkeits-Agents](../azure-monitor/insights/vminsights-enable-overview.md#the-microsoft-dependency-agent) für Windows und Linux.<br/><br/> [Erfahren Sie mehr](../azure-monitor/insights/vminsights-enable-hybrid-cloud.md#installation-script-examples) darüber, wie Sie den Abhängigkeits-Agent mithilfe von Skripts installieren können.
Log Analytics-Agent (MMA) | [Erfahren Sie mehr](../azure-monitor/platform/log-analytics-agent.md#install-and-configure-agent) über MMA-Installationsmethoden.<br/><br/> Bei Computern, die von System Center Operations Manager 2012 R2 oder höher überwacht werden, müssen Sie den MMA-Agent nicht installieren. Die Dienstzuordnung ist in den Operations Manager integriert. Sie können die Integration mithilfe der [hier](https://docs.microsoft.com/azure/azure-monitor/insights/service-map-scom#prerequisites) verfügbaren Anleitungen aktivieren. Beachten Sie aber, dass der Abhängigkeits-Agent auf diesen Computern installiert sein muss.<br/><br/> [Überprüfen](../azure-monitor/platform/log-analytics-agent.md#supported-linux-operating-systems) Sie die Linux-Betriebssysteme, die vom Log Analytics-Agent unterstützt werden.
Bewertungsgruppen | Gruppen, für die Sie Gruppenabhängigkeiten visualisieren möchten, sollten höchstens zehn Computer enthalten. Wenn Sie mehr als 10 Computer haben, teilen Sie diese in kleinere Gruppen auf, um Abhängigkeiten zu visualisieren.

## <a name="associate-a-log-analytics-workspace"></a>Zuordnen eines Log Analytics-Arbeitsbereichs

Sie müssen jedem Azure Migrate-Projekt einen neuen oder vorhandenen [Log Analytics-Arbeitsbereich](../azure-monitor/platform/manage-access.md) zuordnen, um die Abhängigkeitsvisualisierung nutzen zu können.

- Sie können einen Arbeitsbereich nur im Azure Migrate-Projektabonnement hinzufügen.
- Sie können einen bestehenden Arbeitsbereich anhängen oder einen neuen anlegen.
- Sie hängen den Arbeitsbereich bei der ersten Einrichtung der Visualisierung von Abhängigkeiten für einen Computer an.
- Sie können einen Arbeitsbereich erst dann anhängen, wenn Sie im Projekt Azure Migrate Computer ermittelt haben. Sie können dies tun, indem Sie eine Azure Migrate-Appliance für [VMware](how-to-set-up-appliance-vmware.md) oder [Hyper-V](how-to-set-up-appliance-hyper-v.md) einrichten. Die Appliance ermittelt lokale Computer und sendet Metadaten und Leistungsdaten an Azure Migrate: Server Assessment“ (Azure Migrate-Serverbewertung) erstellen. [Weitere Informationen](migrate-appliance.md)

Fügen Sie einen Arbeitsbereich wie folgt an:

1. Klicken Sie in **Azure Migrate: Serverbewertung** auf **Übersicht**. Wenn Sie das Serverbewertungs-Tool noch nicht hinzugefügt haben, [tun Sie dies zuerst](how-to-assess.md).
2. Klicken Sie in der **Übersicht** auf den Abwärtspfeil, um **Essentials** zu erweitern.
3. Klicken Sie im **OMS-Arbeitsbereich** auf **Konfiguration erforderlich**.
4. Geben Sie unter **Arbeitsbereich konfigurieren** an, ob Sie einen neuen Arbeitsbereich erstellen oder einen bestehenden verwenden möchten:

    ![Arbeitsbereich hinzufügen](./media/how-to-create-group-machine-dependencies/workspace.png)

    - Nachdem Sie einen Namen für einen neuen Arbeitsbereich angegeben haben, können Sie die [Region](https://azure.microsoft.com/global-infrastructure/regions/) auswählen, in der der Arbeitsbereich erstellt wird.
    - Wenn Sie einen vorhandenen Arbeitsbereich anfügen, können Sie zwischen allen verfügbaren Arbeitsbereichen im selben Abonnement wie das Migrationsprojekt auswählen.
    - Sie benötigen einen Lesezugriff zum Arbeitsbereich, um ihn anhängen zu können.
    - Sie können den Arbeitsbereich, der einem Projekt zugeordnet ist, nicht mehr ändern, nachdem er angehängt wurde.

## <a name="download-and-install-the-vm-agents"></a>Herunterladen und Installieren der VM-Agents

Laden Sie die Agents herunter und installieren Sie sie auf jedem lokalen Computer, den Sie mit dem Mapping von Abhängigkeiten visualisieren möchten.

1. Klicken Sie in **Azure Migrate: Serverbewertung** auf **Ermittelte Server**.
2. Klicken Sie für jeden Computer, für den Sie die Visualisierung von Abhängigkeiten verwenden möchten, auf **„Agent-Installation erforderlich“** .
3. Laden Sie auf der Seite **„Abhängigkeiten“** für einen Computer > **„MMA herunterladen und installieren“** den entsprechenden Agent herunter und installieren Sie ihn wie unten beschrieben.
4. Laden Sie unter **„Dependency-Agent herunterladen und installieren“** den entsprechenden Agent herunter und installieren Sie ihn wie unten beschrieben.
5. Kopieren Sie unter „**MMA-Agent konfigurieren“** die Arbeitsbereichs-ID und den Schlüssel. Sie benötigen diese Angaben für die Installation des MMA-Agents.

### <a name="install-the-mma"></a>Installieren des MMA

#### <a name="install-the-agent-on-a-windows-machine"></a>Installieren des Agent auf einem Windows-Computer

Gehen Sie wie folgt vor, um den Agent auf einem Windows-Computer zu installieren:

1. Doppelklicken Sie auf den heruntergeladenen Agent.
2. Klicken Sie auf der Seite **Willkommen**auf **Weiter**. Klicken Sie auf der Seite **Lizenzbedingungen** auf **Ich stimme zu**, um die Lizenzbedingungen zu akzeptieren.
3. Behalten Sie unter **Zielordner** den Standardinstallationsordner bei, oder ändern Sie ihn, und klicken Sie anschließend auf **Weiter**.
4. Wählen Sie unter **Agent-Setupoptionen** die Optionen **Azure Log Analytics** > **Weiter**.
5. Klicken Sie auf **Hinzufügen**, um einen neuen Log Analytics-Arbeitsbereich hinzuzufügen. Fügen Sie die Arbeitsbereichs-ID und den dazugehörigen Schlüssel ein, die bzw. den Sie im Portal kopiert haben. Klicken Sie auf **Weiter**.

Sie können den Agent über die Befehlszeile oder mithilfe einer automatisierten Methode wie System Center Configuration Manager oder [Intigua](https://go.microsoft.com/fwlink/?linkid=2104196) installieren. [Weitere Informationen](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#install-and-configure-agent) zur Verwendung dieser Methoden zum Installieren des MMA-Agent. Der MMA-Agent kann auch mit diesem [Skript](https://go.microsoft.com/fwlink/?linkid=2104394) installiert werden.

[Erfahren Sie mehr](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#supported-windows-operating-systems) über die durch den MMA unterstützten Windows-Betriebssysteme.

#### <a name="install-the-agent-on-a-linux-machine"></a>Installieren des Agent auf einem Linux-Computer

Gehen Sie wie folgt vor, um einen Agent auf einem Linux-Computer zu installieren:

1. Übertragen Sie das entsprechende Paket (x86 oder x64) mithilfe von scp/sftp auf Ihren Linux-Computer.
2. Installieren Sie das Bundle mit dem Argument „--install“.

    ```sudo sh ./omsagent-<version>.universal.x64.sh --install -w <workspace id> -s <workspace key>```

[Erfahren Sie mehr](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#supported-linux-operating-systems) über die Liste der Unterstützungen durch den MMA für Linux-Betriebssysteme. 

### <a name="install-the-dependency-agent"></a>Installieren des Abhängigkeits-Agents
1. Doppelklicken Sie zum Installieren des Abhängigkeits-Agents auf einem Windows-Computer auf die Setupdatei, und befolgen Sie die Schritte im Assistenten.
2. Installieren Sie den Abhängigkeits-Agent auf einem Linux-Computer als „root“, indem Sie den folgenden Befehl verwenden:

    ```sh InstallDependencyAgent-Linux64.bin```

[Erfahren Sie mehr](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-enable-hybrid-cloud#installation-script-examples) darüber, wie Sie den Dependency-Agent mithilfe von Skripts installieren können.

[Erfahren Sie mehr](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-enable-overview#supported-operating-systems) über die vom Abhängigkeits-Agent unterstützten Betriebssysteme.


## <a name="create-a-group-using-dependency-visualization"></a>Erstellen von Gruppen mit Abhängigkeitsvisualisierung

1. Klicken Sie in **Azure Migrate: Serverbewertung** auf **Ermittelte Server**.
2. Klicken Sie in der Spalte **„Abhängigkeiten“** auf **„Abhängigkeiten anzeigen“** für jeden Computer, den Sie überprüfen möchten.
3. Auf dem Abhängigkeitsdiagramm sehen Sie Folgendes:
    - Eingehende (Clients) und ausgehende (Server) TCP-Verbindungen vom und zum Computer.
    - Abhängige Computer, auf denen die Dependency-Agents installiert sind, werden nach Portnummer gruppiert.
    - Abhängige Computer mit installierten Dependency-Agents werden als separate Felder angezeigt.
    - Prozesse, die auf dem Computer ausgeführt werden. Sie können die einzelnen Computerfelder erweitern, um die Prozesse anzuzeigen.
    - Computereigenschaften (einschließlich FQDN, Betriebssystem, MAC-Adresse). Klicken Sie jeweils auf ein Computerfeld, um die Details anzuzeigen.

4. Sie können Abhängigkeiten für verschiedene Zeiträume anzeigen, indem Sie auf im Zeitbereich auf die Zeitdauer klicken. Standardmäßig ist ein Bereich von einer Stunde ausgewählt. Sie können den Zeitraum ändern oder das Start- und Enddatum und die Dauer angeben.

    > [!NOTE]
    > Der Zeitbereich kann maximal eine Stunde lang sein. Falls Sie einen längeren Bereich benötigen, können Sie Azure Monitor nutzen, um abhängige Daten für einen längeren Zeitraum abzufragen.

5. Nachdem Sie die abhängigen Computer identifiziert haben, die Sie gruppieren möchten, können Sie die gewünschten Computer mit Strg+Klick auswählen und auf **„Computer gruppieren“** klicken.
6. Geben Sie einen Gruppennamen an.
7. Stellen Sie sicher, dass die abhängigen Computer von Azure Migrate ermittelt werden.

    - Wenn ein abhängiger Computer nicht von Azure Migrate: Serverbewertung ermittelt wird, dann können Sie ihn der Gruppe nicht hinzufügen.
    - Um einen Computer hinzuzufügen, führen Sie die Ermittlung erneut durch, und vergewissern Sie sich, dass der Computer erkannt wird.

8. Wenn Sie eine Bewertung für diese Gruppe erstellen möchten, aktivieren Sie das Kontrollkästchen, um eine neue Bewertung für die Gruppe zu erstellen.
8. Klicken Sie auf **OK**, um die Gruppe zu speichern.

Nachdem Sie die Gruppe erstellt haben, empfehlen wir Ihnen, Agents auf allen Computern der Gruppe zu installieren und dann Abhängigkeiten für die gesamte Gruppe zu visualisieren.

## <a name="query-dependency-data-in-azure-monitor"></a>Abfragen von Abhängigkeitsdaten in Azure Monitor

Sie können von der Dienstzuordnung erfasste Abhängigkeitsdaten im Log Analytics-Arbeitsbereich abfragen, der Ihrem Azure Migrate-Projekt zugeordnet ist. Log Analytics wird zum Schreiben und Ausführen von Azure Monitor-Protokollabfragen verwendet.

- [Erfahren Sie](../azure-monitor/insights/service-map.md#log-analytics-records), wie Sie in Log Analytics nach Dienstzuordnungsdaten suchen.
- [Verschaffen Sie sich einen Überblick](../azure-monitor/log-query/get-started-queries.md) über das Schreiben von Protokollabfragen in [Log Analytics](../azure-monitor/log-query/get-started-portal.md).

Führen Sie eine Abfrage nach Abhängigkeitsdaten wie folgt durch:

1. Navigieren Sie nach der Installation des Agents zum Portal, und klicken Sie auf **Übersicht**.
2. Klicken Sie in **Azure Migrate: Serverbewertung** auf **Übersicht**. Klicken Sie auf den Abwärtspfeil, um **Essentials** zu erweitern.
3. Klicken Sie im **OMS-Arbeitsbereich** auf den Namen des Arbeitsbereichs.
3. Klicken Sie auf der Seite Log Analytics-Arbeitsbereich > **„Allgemein“** auf **„Protokolle“** .
4. Schreiben Sie Ihre Anfrage und klicken Sie auf **„Ausführen“** .

### <a name="sample-queries"></a>Beispielabfragen

Wir stellen Ihnen einige Beispielabfragen zur Verfügung, mit denen Sie Abhängigkeitsdaten extrahieren können.

- Sie können die Abfragen ändern, um Ihre bevorzugten Datenpunkte zu extrahieren.
- [Überprüfen](https://docs.microsoft.com/azure/azure-monitor/insights/service-map#log-analytics-records) Sie eine vollständige Liste der Datensätze der Abhängigkeiten.
- [Überprüfen](https://docs.microsoft.com/azure/azure-monitor/insights/service-map#sample-log-searches) Sie zusätzliche Beispielabfragen.

#### <a name="sample-review-inbound-connections"></a>Beispiel: Überprüfen eingehender Verbindungen

Überprüfen Sie eingehende Verbindungen für eine Gruppe von VMs.

- Die Datensätze in der Tabelle für Verbindungsmetriken (VMConnection) stellen keine einzelnen physischen Netzwerkverbindungen dar.
- Mehrere physische Netzwerkverbindungen werden in einer logischen Verbindung gruppiert.
- [Erfahren Sie mehr darüber](https://docs.microsoft.com/azure/azure-monitor/insights/service-map#connections), wie Daten von physischen Netzwerkverbindungen in VMConnection aggregiert werden.

```
// the machines of interest
let ips=materialize(ServiceMapComputer_CL
| summarize ips=makeset(todynamic(Ipv4Addresses_s)) by MonitoredMachine=ResourceName_s
| mvexpand ips to typeof(string));
let StartDateTime = datetime(2019-03-25T00:00:00Z);
let EndDateTime = datetime(2019-03-30T01:00:00Z);
VMConnection
| where Direction == 'inbound'
| where TimeGenerated > StartDateTime and TimeGenerated  < EndDateTime
| join kind=inner (ips) on $left.DestinationIp == $right.ips
| summarize sum(LinksEstablished) by Computer, Direction, SourceIp, DestinationIp, DestinationPort
```

#### <a name="sample-summarize-sent-and-received-data"></a>Beispiel: Zusammenfassen von gesendeten und empfangenen Daten

Dieses Beispiel zeigt eine Zusammenfassung des gesendeten und empfangenen Datenvolumens in eingehenden Verbindungen zwischen einer Gruppe von Computern an.

```
// the machines of interest
let ips=materialize(ServiceMapComputer_CL
| summarize ips=makeset(todynamic(Ipv4Addresses_s)) by MonitoredMachine=ResourceName_s
| mvexpand ips to typeof(string));
let StartDateTime = datetime(2019-03-25T00:00:00Z);
let EndDateTime = datetime(2019-03-30T01:00:00Z);
VMConnection
| where Direction == 'inbound'
| where TimeGenerated > StartDateTime and TimeGenerated  < EndDateTime
| join kind=inner (ips) on $left.DestinationIp == $right.ips
| summarize sum(BytesSent), sum(BytesReceived) by Computer, Direction, SourceIp, DestinationIp, DestinationPort
```

## <a name="next-steps"></a>Nächste Schritte

[Erstellen einer Bewertung](how-to-create-assessment.md) für eine Gruppe.
