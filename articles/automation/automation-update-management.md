---
title: Lösung für die Updateverwaltung in Azure
description: In diesem Artikel erfahren Sie, wie Sie Updates für Ihre Windows- und Linux-Computer mithilfe der Azure-Updateverwaltung verwalten.
services: automation
ms.service: automation
ms.subservice: update-management
author: bobbytreed
ms.author: robreed
ms.date: 05/22/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 66acb1284f0814eec91715284259272a065dbae2
ms.sourcegitcommit: e0e6663a2d6672a9d916d64d14d63633934d2952
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "72690895"
---
# <a name="update-management-solution-in-azure"></a>Lösung für die Updateverwaltung in Azure

Mithilfe der Updateverwaltung in Azure Automation können Sie Betriebssystemupdates für Ihre Windows- und Linux-Computer in Azure, in lokalen Umgebungen und bei anderen Cloudanbietern verwalten. Sie können den Status der verfügbaren Updates auf allen Agent-Computern schnell auswerten und die Installation der für den Server erforderlichen Updates initiieren.

Die Updateverwaltung für virtuelle Computer (virtual machines, VMs) kann direkt über Ihr Azure Automation-Konto aktiviert werden. Eine entsprechende Anleitung finden Sie unter [Verwalten von Updates für mehrere Computer](manage-update-multi.md). Sie können die Updateverwaltung für einen virtuellen Computer auch im Azure-Portal auf der Seite des virtuellen Computers aktivieren. Dieses Szenario steht für virtuelle Computer unter [Linux](../virtual-machines/linux/tutorial-config-management.md#enable-update-management) oder [Windows](../virtual-machines/windows/tutorial-config-management.md#enable-update-management) zur Verfügung.

> [!NOTE]
> Für die Lösung zur Updateverwaltung muss ein Log Analytics-Arbeitsbereich mit Ihrem Automation-Konto verknüpft werden. Eine aktuelle Liste der unterstützten Regionen finden Sie unter [Arbeitsbereichzuordnungen in Azure](./how-to/region-mappings.md). Die Regionszuordnungen haben keine Auswirkung auf die Möglichkeit, virtuelle Computer in einer anderen Region als der Ihres Automation-Kontos zu verwalten.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="solution-overview"></a>Lösungsübersicht

Computer, die mit der Updateverwaltung verwaltet werden, verwenden folgende Konfigurationen, um Bewertungen und Updatebereitstellungen durchzuführen:

* Microsoft Monitoring Agent (MMA) für Windows oder Linux
* PowerShell Desired State Configuration (DSC) für Linux
* Automation Hybrid Runbook Worker
* Microsoft Update oder Windows Server Update Services (WSUS) für Windows-Computer

Das folgende Diagramm veranschaulicht, wie die Lösung alle verbundenen Windows Server- und Linux-Computer eines Arbeitsbereichs bewertet und Sicherheitsupdates darauf anwendet:

![Ablauf des Updateverwaltungsprozesses](./media/automation-update-management/update-mgmt-updateworkflow.png)

Die Updateverwaltung kann für das systeminterne Integrieren von Computern in mehrere Abonnements im selben Mandanten verwendet werden.

Nachdem ein Paket veröffentlicht wurde, dauert es zwei bis drei Stunden, bis der Patch für Linux-Computer für die Bewertung angezeigt wird. Bei Windows-Computern dauert es 12 bis 15 Stunden, bis der Patch nach der Veröffentlichung für die Bewertung angezeigt wird.

Nachdem ein Computer einen Scanvorgang abgeschlossen hat, um die Konformität für das Update zu überprüfen, leitet der Agent die Informationen gesammelt an Azure Monitor-Protokolle weiter. Auf einem Windows-Computer wird der Konformitätsscan standardmäßig alle 12 Stunden ausgeführt.

Darüber hinaus wird der Update-Konformitätsscan innerhalb von 15 Minuten nach dem MMA-Neustart sowie vor und nach der Updateinstallation initiiert.

Für einen Linux-Computer wird der Konformitätsscan standardmäßig jede Stunde durchgeführt. Wenn der MMA-Agent neu gestartet wird, wird ein Konformitätsscan innerhalb von 15 Minuten eingeleitet.

Die Lösung meldet basierend auf der für die Synchronisierung konfigurierten Quelle, wie aktuell der Computer ist. Wenn der Windows-Computer für das Senden von Meldungen an WSUS konfiguriert ist, können sich die Ergebnisse von den angezeigten Microsoft Update-Ergebnissen unterscheiden. Dies hängt davon ab, wann WSUS zuletzt mit Microsoft Update synchronisiert wurde. Dasselbe gilt für Linux-Computer, die für Meldungen an ein lokales Repository konfiguriert sind (anstatt an ein öffentliches Repository).

> [!NOTE]
> Damit Meldungen an den Dienst ordnungsgemäß erfolgen können, erfordert die Updateverwaltung, dass bestimmte URLs und Ports aktiviert werden. Weitere Informationen zu diesen Anforderungen finden Sie unter [Netzwerkplanung für Hybrid Worker](automation-hybrid-runbook-worker.md#network-planning).

Sie können Softwareupdates auf Computern bereitstellen und installieren, für die die Updates erforderlich sind, indem Sie einen geplante Bereitstellung erstellen. Updates, die als *Optional* klassifiziert sind, sind im Bereitstellungsumfang von Windows-Computern nicht enthalten. Nur erforderliche Updates sind im Bereitstellungsumfang enthalten.

Die geplante Bereitstellung definiert, welche Zielcomputer die anwendbaren Updates erhalten. Hierzu werden entweder explizit bestimmte Computer angegeben, oder es wird eine [Computergruppe](../azure-monitor/platform/computer-groups.md) ausgewählt, die auf Protokollsuchvorgängen einer bestimmten Gruppe von Computern basiert (oder auf einer [Azure-Abfrage](automation-update-management-query-logs.md), die virtuelle Azure-Computer auf der Grundlage bestimmter Kriterien dynamisch auswählt). Diese Gruppen unterscheiden sich von der [Bereichskonfiguration](../azure-monitor/insights/solution-targeting.md), die nur zur Ermittlung der Computer dient, die die Verwaltungspakete zur Ermöglichung der Lösung erhalten.

Darüber hinaus geben Sie einen Zeitplan an, um einen Zeitraum zu genehmigen und festzulegen, in dem Updates installiert werden dürfen. Dieser Zeitraum wird das Wartungsfenster bezeichnet. 20 Minuten des Wartungsfensters sind für Neustarts reserviert (sofern ein Neustart erforderlich ist und Sie die entsprechende Neustartoption ausgewählt haben). Wenn das Patchen länger dauert als erwartet und im Wartungsfenster weniger als 20 Minuten verbleiben, wird kein Neustart durchgeführt.

Updates werden mit Runbooks in Azure Automation installiert. Sie können diese Runbooks nicht anzeigen, und für diese Runbooks ist keine Konfiguration erforderlich. Bei der Erstellung einer Updatebereitstellung wird ein Zeitplan erstellt, nach dem für die einbezogenen Computer zur angegebenen Zeit ein Masterrunbook für das Update gestartet wird. Das Masterrunbook startet ein untergeordnetes Runbook für jeden Agent, um die erforderlichen Updates zu installieren.

Wenn die Datums- bzw. Uhrzeitangabe der Updatebereitstellung erreicht ist, führen die Zielcomputer die Bereitstellung parallel aus. Vor der Installation wird ein Scan ausgeführt, um sicherzustellen, dass die Updates weiterhin erforderlich sind. Für WSUS-Clientcomputer tritt ein Fehler bei der Updatebereitstellung auf, wenn die Updates in WSUS nicht genehmigt wurden.

Die Registrierung eines Computers für die Updateverwaltung in mehreren Log Analytics-Arbeitsbereichen (Multi-Homing) wird nicht unterstützt.

## <a name="clients"></a>Clients

### <a name="supported-client-types"></a>Unterstützte Clienttypen

Die folgende Tabelle enthält die unterstützten Betriebssysteme für Updatebewertungen. Patchen erfordert einen Hybrid Runbook Worker. Informationen zu den Anforderungen für Hybrid Runbook Worker finden Sie in den Installationsleitfäden für [Windows Hybrid Runbook Worker](automation-windows-hrw-install.md#installing-the-windows-hybrid-runbook-worker) und [Linux Hybrid Runbook Worker](automation-linux-hrw-install.md#installing-a-linux-hybrid-runbook-worker).

|Betriebssystem  |Notizen  |
|---------|---------|
|Windows Server 2019 (Datacenter/Datacenter Core/Standard)<br><br>Windows Server 2016 (Datacenter/Datacenter Core/Standard)<br><br>Windows Server 2012 R2 (Datacenter/Standard)<br><br>Windows Server 2012<br><br>Windows Server 2008 R2 (RTM und SP1 Standard)||
|CentOS 6 (x86/x64) und 7 (x64)      | Für Linux-Agents muss Zugriff auf ein Updaterepository bestehen. Für klassifizierungsbasiertes Patchen muss `yum` Sicherheitsdaten zurückgeben, über die CentOS in den RTM-Releases nicht verfügt. Weitere Informationen zu klassifizierungsbasiertem Patching unter CentOS finden Sie unter [Lösung für die Updateverwaltung in Azure](#linux-2).          |
|Red Hat Enterprise 6 (x86/x64) und 7 (x64)     | Für Linux-Agents muss Zugriff auf ein Updaterepository bestehen.        |
|SUSE Linux Enterprise Server 11 (x86/x64) und 12 (x64)     | Für Linux-Agents muss Zugriff auf ein Updaterepository bestehen.        |
|Ubuntu 14.04 LTS, 16.04 LTS und 18.04 (x86/x64)      |Für Linux-Agents muss Zugriff auf ein Updaterepository bestehen.         |

> [!NOTE]
> VM-Skalierungsgruppen von Azure können über die Updateverwaltung verwaltet werden. Die Updateverwaltung arbeitet nicht mit dem Basisimage, sondern mit den Instanzen selbst. Die Updates müssen inkrementell geplant werden, damit nicht alle VM-Instanzen auf einmal aktualisiert werden.
> Sie können Knoten für VM-Skalierungsgruppen hinzufügen. Eine entsprechende Anleitung finden Sie unter [Integrieren eines Nicht-Azure-Computers](automation-tutorial-installed-software.md#onboard-a-non-azure-machine).

### <a name="unsupported-client-types"></a>Nicht unterstützte Clienttypen

Die folgende Tabelle enthält nicht unterstützte Betriebssysteme:

|Betriebssystem  |Notizen  |
|---------|---------|
|Windows-Client     | Clientbetriebssysteme (z.B. Windows 7 und Windows 10) werden nicht unterstützt.        |
|Windows Server 2016 Nano Server     | Nicht unterstützt.       |
|Azure Kubernetes Service-Knoten | Nicht unterstützt. Verwenden Sie den unter [Anwenden von Sicherheits- und Kernelupdates auf Linux-Knoten in Azure Kubernetes Service (AKS)](../aks/node-updates-kured.md) beschriebenen Patchprozess.|

### <a name="client-requirements"></a>Clientanforderungen

Der folgende Abschnitt enthält Informationen zu betriebssystemspezifischen Clientanforderungen. Weitere Informationen finden Sie unter [Netzwerkplanung](#ports).

#### <a name="windows"></a>Windows

Windows-Agents müssen für die Kommunikation mit einem WSUS-Server konfiguriert sein oder über Zugriff auf Microsoft Update verfügen.

Sie können die Updateverwaltung mit System Center Configuration Manager verwenden. Weitere Informationen zu den Integrationsszenarien finden Sie unter [Integrieren von System Center Configuration Manager in die Updateverwaltung](oms-solution-updatemgmt-sccmintegration.md#configuration). Der [Windows-Agent](../azure-monitor/platform/agent-windows.md) ist erforderlich. Der Agent wird automatisch installiert, wenn Sie einen virtuellen Azure-Computer integrieren.

Über den Azure Marketplace bereitgestellte virtuelle Windows-Computer sind standardmäßig so konfiguriert, dass sie automatisch Updates vom Windows Update-Dienst erhalten. Dieses Verhalten ändert sich nicht, wenn Sie diese Lösung hinzufügen oder Ihrem Arbeitsbereich virtuelle Windows-Computer hinzufügen. Wenn Sie Updates mithilfe dieser Lösung nicht aktiv verwalten, gilt das Standardverhalten (Updates werden automatisch angewendet).

> [!NOTE]
> Ein Benutzer kann die Gruppenrichtlinie so ändern, dass Computerneustarts nur vom Benutzer durchgeführt werden können, nicht aber vom System. Verwaltete Computer bleiben ggf. hängen, wenn die Updateverwaltung nicht berechtigt ist, den Computer ohne manuelle Benutzerinteraktion neu zu starten.
>
> Weitere Informationen finden Sie unter [Konfigurieren der Gruppenrichtlinieneinstellungen für automatische Updates](https://docs.microsoft.com/windows-server/administration/windows-server-update-services/deploy/4-configure-group-policy-settings-for-automatic-updates).

#### <a name="linux"></a>Linux

Für Linux muss der Computer über Zugriff auf ein Updaterepository verfügen. Das Updaterepository kann privat oder öffentlich sein. Für die Interaktion mit der Updateverwaltung ist TLS 1.1 oder TLS 1.2 erforderlich. Diese Lösung unterstützt keinen Log Analytics-Agent für Linux, der für die Berichterstattung an mehrere Log Analytics-Arbeitsbereiche konfiguriert ist. Auf dem Computer muss außerdem Python 2.x installiert sein.

Weitere Informationen zum Installieren des Log Analytics-Agents für Linux und zum Herunterladen der aktuellen Version finden Sie unter [Log Analytics Agent for Linux (Log Analytics-Agent für Linux)](https://github.com/microsoft/oms-agent-for-linux). Informationen zur Installation des Log Analytics-Agents für Windows finden Sie unter [Verbinden von Windows-Computern mit Azure Monitor](../log-analytics/log-analytics-windows-agent.md).

Virtuelle Computer, die auf der Grundlage der über Azure Marketplace erhältlichen On-Demand-RHEL-Images (Red Hat Enterprise Linux) erstellt werden, werden für den Zugriff auf die in Azure bereitgestellte [Red Hat-Updateinfrastruktur (RHUI)](../virtual-machines/virtual-machines-linux-update-infrastructure-redhat.md) registriert. Alle anderen Linux-Distributionen müssen über das Onlinedateirepository der Distributionen mithilfe der unterstützten Methoden der jeweiligen Distribution aktualisiert werden.

## <a name="permissions"></a>Berechtigungen

Zum Erstellen und Verwalten von Updatebereitstellungen benötigen Sie bestimmte Berechtigungen. Weitere Informationen zu diesen Berechtigungen finden Sie unter [Rollenbasierter Zugriff: Updateverwaltung](automation-role-based-access-control.md#update-management).

## <a name="solution-components"></a>Lösungskomponenten

Die Lösung besteht aus den folgenden Ressourcen. Die Ressourcen werden Ihrem Automation-Konto hinzugefügt. Es handelt sich um direkt verbundene Agents oder um Agents in einer durch Operations Manager verbundenen Verwaltungsgruppe.

### <a name="hybrid-worker-groups"></a>Hybrid Worker-Gruppen

Nachdem Sie diese Lösung aktiviert haben, werden alle direkt mit dem Log Analytics-Arbeitsbereich verbundenen Windows-Computer automatisch als Hybrid Runbook Worker konfiguriert, um die in dieser Lösung enthaltenen Runbooks zu unterstützen.

Jeder von der Lösung verwaltete Windows-Computer wird im Bereich **Hybrid Worker-Gruppen** als **Hybrid Worker-Systemgruppe** für das Automation-Konto aufgeführt. Die Lösungen verwenden die Benennungskonvention *Hostname FQDN_GUID*. Es ist nicht möglich, diese Gruppen mit Runbooks in Ihrem Konto zu erreichen. Entsprechende Versuche sind nicht erfolgreich. Diese Gruppen sind nur für die Unterstützung der Verwaltungslösung bestimmt.

Sie können die Windows-Computer einer Hybrid Runbook Worker-Gruppe in Ihrem Automation-Konto hinzufügen, um Automation-Runbooks zu unterstützen, wenn Sie für die Lösung und die Mitgliedschaft in der Hybrid Runbook Worker-Gruppe dasselbe Konto verwenden. Diese Funktionalität wurde in Version 7.2.12024.0 des Hybrid Runbook Worker hinzugefügt.

### <a name="management-packs"></a>Management Packs

Wenn Ihre System Center Operations Manager-Verwaltungsgruppe mit einem Log Analytics-Arbeitsbereich verbunden ist, werden in Operations Manager die folgenden Management Packs installiert. Diese Management Packs werden nach dem Hinzufügen dieser Lösung auch auf direkt verbundenen Windows-Computern installiert. Für diese Management Packs fällt kein Konfigurations- oder Verwaltungsaufwand an.

* Microsoft System Center Advisor Update Assessment Intelligence Pack (Microsoft.IntelligencePacks.UpdateAssessment)
* Microsoft.IntelligencePack.UpdateAssessment.Configuration (Microsoft.IntelligencePack.UpdateAssessment.Configuration)
* Update Deployment MP

> [!NOTE]
> Angenommen, Sie verfügen über eine Operations Manager-Verwaltungsgruppe (1807 oder 2019) mit auf Verwaltungsgruppenebene konfigurierten Agents, um sie einem Arbeitsbereich zuzuordnen. In diesem Fall können Sie **IsAutoRegistrationEnabled** in der Regel **Microsoft.IntelligencePacks.AzureAutomation.HybridAgent.Init** mit **True** überschreiben, um das Problem mit der Anzeige zu umgehen.

Weitere Informationen zur Aktualisierung von Management Packs finden Sie unter [Herstellen einer Verbindung zwischen Operations Manager und Log Analytics](../azure-monitor/platform/om-agents.md).

> [!NOTE]
> Bei Systemen mit Operations Manager-Agent gilt Folgendes: Damit ein Agent vollständig von der Updateverwaltung verwaltet werden kann, muss er auf den MMA aktualisiert werden. Informationen zum Aktualisieren des Agents finden Sie unter [Aktualisieren eines Operations Manager-Agents](https://docs.microsoft.com/system-center/scom/deploy-upgrade-agents). In Umgebungen, in denen Operations Manager verwendet wird, muss mindestens System Center Operations Manager 2012 R2 UR14 ausgeführt werden.

## <a name="data-collection"></a>Datensammlung

### <a name="supported-agents"></a>Unterstützte Agents

In der folgenden Tabelle sind die verbundenen Quellen beschrieben, die von dieser Lösung unterstützt werden:

| Verbundene Quelle | Unterstützt | BESCHREIBUNG |
| --- | --- | --- |
| Windows-Agents |Ja |Die Lösung sammelt Informationen zu Systemupdates aus Windows-Agents und initiiert dann die Installation von erforderlichen Updates. |
| Linux-Agents |Ja |Die Lösung sammelt Informationen zu Systemupdates von Linux-Agents und initiiert dann die Installation von erforderlichen Updates für unterstützte Distributionen. |
| Operations Manager-Verwaltungsgruppe |Ja |Die Lösung sammelt Informationen zu Systemupdates von Agents in einer verbundenen Verwaltungsgruppe.<br/><br/>Es ist keine direkte Verbindung zwischen dem Operations Manager-Agent und Azure Monitor-Protokollen erforderlich. Daten werden von der Verwaltungsgruppe an den Log Analytics-Arbeitsbereich weitergeleitet. |

### <a name="collection-frequency"></a>Sammlungshäufigkeit

Für jeden verwalteten Windows-Computer wird zwei Mal pro Tag ein Scanvorgang ausgeführt. Alle 15 Minuten wird die Windows-API aufgerufen, um den letzten Updatezeitpunkt abzufragen und zu ermitteln, ob sich der Status geändert hat. Wenn sich der Status geändert hat, wird eine Konformitätsprüfung eingeleitet.

Für jeden verwalteten Linux-Computer wird jede Stunde ein Scanvorgang ausgeführt.

Es kann zwischen 30 Minuten und sechs Stunden dauern, bis im Dashboard aktualisierte Daten von verwalteten Computern angezeigt werden.

Die durchschnittliche Datennutzung von Azure Monitor-Protokollen für einen Computer mit Updateverwaltung beträgt etwa 25 MB pro Monat. Dieser Wert ist lediglich eine Schätzung und kann sich abhängig von Ihrer Umgebung ändern. Es empfiehlt sich, die Umgebung zu überwachen, um die exakte Nutzung nachzuverfolgen.

## <a name="ports"></a>Netzwerkplanung

Die folgenden Adressen sind speziell für die Updateverwaltung erforderlich. Die Kommunikation mit diesen Adressen erfolgt über Port 443.

|Azure – Öffentlich  |Azure Government  |
|---------|---------|
|*.ods.opinsights.azure.com     |*.ods.opinsights.azure.us         |
|*.oms.opinsights.azure.com     | *.oms.opinsights.azure.us        |
|*.blob.core.windows.net|*.blob.core.usgovcloudapi.net|
|*.azure-automation.net|*.azure-automation.us|

Bei Windows-Computern müssen Sie auch Datenverkehr zu allen Endpunkten zulassen, die für Windows Update erforderlich sind. Sie finden eine aktualisierte Liste der erforderlichen Endpunkte unter [Probleme im Zusammenhang mit HTTP/Proxy](/windows/deployment/update/windows-update-troubleshooting#issues-related-to-httpproxy). Wenn Sie über einen lokalen [Windows Update-Server](/windows-server/administration/windows-server-update-services/plan/plan-your-wsus-deployment) verfügen, müssen Sie auch Datenverkehr zu dem in Ihrem [WSUS-Schlüssel](/windows/deployment/update/waas-wu-settings#configuring-automatic-updates-by-editing-the-registry) angegebenen Server zulassen.

Informationen zu den erforderlichen Endpunkten für Red Hat Linux-Computer finden Sie unter [IP-Adressen der Server für die RHUI-Inhaltsübermittlung](../virtual-machines/linux/update-infrastructure-redhat.md#the-ips-for-the-rhui-content-delivery-servers). Informationen zu anderen Linux-Distributionen finden Sie in der jeweiligen Dokumentation des Anbieters.

Weitere Informationen zu Ports, die für den Hybrid Runbook Worker erforderlich sind, finden Sie unter [Ports für Hybrid Worker-Rollen](automation-hybrid-runbook-worker.md#hybrid-worker-role).

Es wird empfohlen, beim Definieren von Ausnahmen die aufgeführten Adressen zu verwenden. Für IP-Adressen können Sie [IP-Bereiche des Microsoft Azure-Rechenzentrums](https://www.microsoft.com/download/details.aspx?id=41653) herunterladen. Diese Datei wird wöchentlich aktualisiert und enthält die aktuell bereitgestellten Bereiche sowie alle anstehenden Änderungen für die IP-Adressbereiche.

Gehen Sie wie unter [Verbinden von Computern ohne Internetzugriff über das Log Analytics-Gateway in Azure Monitor](../azure-monitor/platform/gateway.md) beschrieben vor, um Computer ohne Internetzugriff zu konfigurieren.

## <a name="view-update-assessments"></a>Anzeigen der Updatebewertungen

Wählen Sie in Ihrem Automation-Konto **Updateverwaltung** aus, um den Status Ihrer Computer anzuzeigen.

Diese Ansicht enthält Informationen zu Ihren Computern, zu fehlenden Updates, zu Updatebereitstellungen und geplanten Updatebereitstellungen. In der Spalte **KONFORMITÄT** sehen Sie, wann der Computer zuletzt bewertet wurde. In der Spalte **UPDATE-AGENT-BEREITSCHAFT** können Sie die Integrität des Update-Agents überprüfen. Liegt ein Problem vor, wählen Sie den Link aus, um zur Dokumentation für die Problembehandlung zu navigieren und Informationen zur Behebung des Problems zu erhalten.

Wenn Sie eine Protokollsuche ausführen möchten, die Informationen zum Computer, zu Updates oder zu Bereitstellungen zurückgibt, wählen Sie das entsprechende Element in der Liste aus. Der Bereich **Protokollsuche** wird mit einer Abfrage für das ausgewählte Element geöffnet:

![Standardansicht der Updateverwaltung](media/automation-update-management/update-management-view.png)

## <a name="view-missing-updates"></a>Anzeigen fehlender Updates

Wählen Sie **Fehlende Updates** aus, um eine Liste der Updates anzuzeigen, die auf Ihrem Computer nicht vorhanden sind. Jedes Update wird aufgeführt und kann ausgewählt werden. Es werden Informationen zur Anzahl der Computer, die das Update benötigen, und zum Betriebssystem sowie ein Link zu weiteren Informationen angezeigt. Der Bereich **Protokollsuche** zeigt weitere Details zu den Updates an.

![Fehlende Updates](./media/automation-view-update-assessments/automation-view-update-assessments-missing-updates.png)

## <a name="update-classifications"></a>Updateklassifizierungen

Die folgenden Tabellen enthalten eine Liste der Updateklassifizierungen in der Updateverwaltung sowie eine Definition für jede Klassifikation.

### <a name="windows"></a>Windows

|Classification  |BESCHREIBUNG  |
|---------|---------|
|Kritische Updates     | Ein Update für ein bestimmtes Problem, mit dem ein entscheidender, nicht sicherheitsrelevanter Fehler behoben wird.        |
|Sicherheitsupdates     | Ein Update für ein produktspezifisches, sicherheitsrelevantes Problem.        |
|Updaterollups     | Eine kumulative Gruppe von Hotfixes, die zur Vereinfachung der Bereitstellung gebündelt sind.        |
|Feature Packs     | Neue Produktfeatures, die nicht im Rahmen eines Produktreleases verteilt werden.        |
|Service Packs     | Eine kumulative Gruppe von Hotfixes, die auf eine Anwendung angewendet werden.        |
|Definitionsupdates     | Ein Update für virenbehaftete oder andere Definitionsdateien.        |
|Tools     | Ein Hilfsprogramm oder Feature, mit dem mindestens eine Aufgabe ausgeführt werden kann.        |
|Aktualisierungen     | Ein Update für eine Anwendung oder Datei, die zurzeit installiert ist.        |

### <a name="linux-2"></a>Linux

|Classification  |BESCHREIBUNG  |
|---------|---------|
|Kritische Updates und Sicherheitsupdates     | Updates für ein spezielles oder produktspezifisches, sicherheitsrelevantes Problem.         |
|Andere Updates     | Alle anderen Updates, die nicht kritisch sind oder bei denen es sich nicht um Sicherheitsupdates handelt.        |

Für Linux kann die Updateverwaltung bei der Anzeige von Bewertungsdaten dank der Datenanreicherung in der Cloud zwischen kritischen Updates und Sicherheitsupdates in der Cloud unterscheiden. Für das Patchen verwendet die Updateverwaltung Klassifizierungsdaten, die auf dem Computer verfügbar sind. Im Gegensatz zu anderen Distributionen verfügt CentOS in der RTM-Version standardmäßig nicht über diese Informationen. Wenn Sie CentOS-Computer so konfiguriert haben, dass für den folgenden Befehl Sicherheitsdaten zurückgegeben werden, kann die Updateverwaltung basierend auf Klassifizierungen Patchvorgänge ausführen.

```bash
sudo yum -q --security check-update
```

Aktuell steht keine unterstützte Methode zur Verfügung, mit der unter CentOS eine native Verfügbarkeit von Klassifizierungsdaten implementiert werden kann. Für Kunden, die dies möglicherweise selbst implementiert haben, wird derzeit nur Support nach bestem Wissen bereitgestellt.

## <a name="integrate-with-system-center-configuration-manager"></a>Integrieren in System Center Configuration Manager

Kunden, die in System Center Configuration Manager investiert haben, um PCs, Server und mobile Geräte zu verwalten, profitieren auch von der Leistungsstärke und dem Funktionsumfang von Configuration Manager bei der Verwaltung von Softwareupdates. Configuration Manager ist Teil ihres Softwareupdateverwaltungs-Zyklus.

Wie Sie die Verwaltungslösung in System Center Configuration Manager integrieren, erfahren Sie unter [Integrieren von System Center Configuration Manager und Updateverwaltung](oms-solution-updatemgmt-sccmintegration.md).

### <a name="third-party-patches-on-windows"></a>Drittanbieterpatches unter Windows

In der Updateverwaltung wird das lokal konfigurierte Updaterepository verwendet, um unterstützte Windows-Systeme zu patchen. Dabei handelt es sich entweder um WSUS oder um Windows Update. Mit Tools wie [System Center Updates Publisher](/sccm/sum/tools/updates-publisher) (Updates Publisher) können Sie benutzerdefinierte Updates in WSUS veröffentlichen. Dadurch kann die Updateverwaltung Computer, auf denen System Center Configuration Manager als Updaterepository verwendet wird, mit Software von Drittanbietern patchen. Informationen zum Konfigurieren von Updates Publisher finden Sie unter [Installieren von Updates Publisher](/sccm/sum/tools/install-updates-publisher).

## <a name="patch-linux-machines"></a>Patchen von Linux-Computern

In den folgenden Abschnitten werden mögliche Probleme beim Patchen von Linux-Computern erläutert.

### <a name="unexpected-os-level-upgrades"></a>Unerwartete Upgrades auf Betriebssystemebene

Bei einigen Linux-Varianten (z. B. Red Hat Enterprise Linux) werden unter Umständen Upgrades auf Betriebssystemebene über Pakete durchgeführt. Dies kann ggf. zur Ausführung der Updateverwaltung führen, wodurch sich die Versionsnummer des Betriebssystems ändert. Dieses Verhalten ist beabsichtigt, da die Updateverwaltung die gleichen Methoden für Updatepakete verwendet, die auch ein Administrator lokal auf dem Linux-Computer nutzt.

Um zu vermeiden, dass die Betriebssystemversion durch die Ausführung der Updateverwaltung aktualisiert wird, verwenden Sie das Feature **Ausschluss**.

In Red Hat Enterprise Linux lautet der Name des auszuschließenden Pakets „redhat-release-server.x86_64“.

![Auszuschließende Pakete für Linux](./media/automation-update-management/linuxpatches.png)

### <a name="criticalsecurity-patches-arent-applied"></a>Nicht angewendete kritische Patches/Sicherheitspatches

Beim Bereitstellen von Updates für einen Linux-Computer können Sie Updateklassifizierungen auswählen. Durch diese Option werden die auf den Computer angewendeten Updates gefiltert, die die definierten Kriterien erfüllen. Dieser Filter wird lokal auf dem Computer angewendet, wenn das Update bereitgestellt wird.

Da die Updateverwaltung die Updateanreicherung in der Cloud durchführt, werden einige Updates in der Updateverwaltung möglicherweise als Update mit Sicherheitsauswirkung gekennzeichnet, obwohl der lokale Computer nicht über diese Information verfügt. Daher gibt es beim Anwenden kritischer Updates auf einem Linux-Computer möglicherweise Updates, die für diesen Computer nicht als Update mit Sicherheitsauswirkung gekennzeichnet sind und somit nicht angewendet werden. Es kann allerdings sein, dass der Computer von der Updateverwaltung weiterhin als nicht konform gemeldet wird, da sie über zusätzliche Informationen zum relevanten Update verfügt.

Das Bereitstellen von Updates nach Updateklassifizierung funktioniert unter RTM-Versionen von CentOS nicht. Wählen Sie zum ordnungsgemäßen Bereitstellen von Updates für CentOS alle Klassifizierungen aus, um sicherzustellen, dass die Updates angewendet werden. Wenn für SUSE als Klassifizierung *nur* die Option **Weitere Updates** ausgewählt wird, kann dies dazu führen, dass auch einige Sicherheitsupdates installiert werden, falls zuerst Sicherheitsupdates im Zusammenhang mit zypper (Paket-Manager) oder dessen Abhängigkeiten erforderlich sind. Dieses Verhalten ist eine Einschränkung von zypper. In einigen Fällen muss die Updatebereitstellung ggf. erneut ausgeführt werden. Überprüfen Sie das Updateprotokoll.

### <a name="multi-tenant"></a>Mandantenübergreifende Updatebereitstellungen

Wenn Sie Computer patchen müssen, die einem anderen Azure-Mandanten angehören, der der Updateverwaltung unterliegt, müssen Sie wie folgt vorgehen, um sie in die Planung aufzunehmen. Verwenden Sie das Cmdlet [New-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/new-azurermautomationschedule) mit dem Schalter `-ForUpdate`, um einen Zeitplan zu erstellen, und das Cmdlet [New-AzureRmAutomationSoftwareUpdateConfiguration](/powershell/module/azurerm.automation/new-azurermautomationsoftwareupdateconfiguration
), um die Computer aus dem anderen Mandanten an den Parameter `-NonAzureComputer` zu übergeben. Dies wird anhand des folgenden Beispiels veranschaulicht:

```azurepowershell-interactive
$nonAzurecomputers = @("server-01", "server-02")

$startTime = ([DateTime]::Now).AddMinutes(10)

$sched = New-AzureRmAutomationSchedule -ResourceGroupName mygroup -AutomationAccountName myaccount -Name myupdateconfig -Description test-OneTime -OneTime -StartTime $startTime -ForUpdate

New-AzureRmAutomationSoftwareUpdateConfiguration  -ResourceGroupName $rg -AutomationAccountName <automationAccountName> -Schedule $sched -Windows -NonAzureComputer $nonAzurecomputers -Duration (New-TimeSpan -Hours 2) -IncludedUpdateClassification Security,UpdateRollup -ExcludedKbNumber KB01,KB02 -IncludedKbNumber KB100
```

## <a name="onboard"></a>Aktivieren der Updateverwaltung

Um mit dem Patchen von Systemen zu beginnen, müssen Sie die Updateverwaltungslösung aktivieren. Es gibt viele Methoden, Computer in die Updateverwaltung zu integrieren. Die folgenden Methoden zum Integrieren der Lösung werden empfohlenen und unterstützt:

* [Über einen virtuellen Computer](automation-onboard-solutions-from-vm.md)
* [Über Durchsuchen mehrerer Computer](automation-onboard-solutions-from-browse.md)
* [Über Ihr Automation-Konto](automation-onboard-solutions-from-automation-account.md)
* [Über ein Azure Automation-Runbook](automation-onboard-solutions.md)

## <a name="next-steps"></a>Nächste Schritte

Im folgenden Tutorial erfahren Sie, wie Sie Updates für Ihre virtuellen Windows-Computer verwalten:

> [!div class="nextstepaction"]
> [Verwalten von Updates und Patches für Ihre virtuellen Azure Windows-Computer](automation-tutorial-update-management.md)

* Verwenden Sie Protokollsuchen in [Azure Monitor-Protokollen](../log-analytics/log-analytics-log-searches.md), um ausführliche Aktualisierungsdaten anzuzeigen.
* [Erstellen Sie Warnungen](automation-tutorial-update-management.md#configure-alerts) für den Status der Updatebereitstellung.

* Informationen zur Interaktion mit der Updateverwaltung über die REST-API finden Sie unter [Softwareupdatekonfigurationen](/rest/api/automation/softwareupdateconfigurations).
* Informationen zur Problembehandlung für die Updateverwaltung finden Sie unter [Behandeln von Problemen mit Updateverwaltung](troubleshoot/update-management.md).
