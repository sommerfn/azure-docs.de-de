---
title: Lösung für die Updateverwaltung in Azure
description: In diesem Artikel soll vermittelt werden, wie Sie die Lösung für die Azure-Updateverwaltung zum Verwalten von Updates für Ihre Windows- und Linux-Computer verwenden können.
services: automation
ms.service: automation
ms.subservice: update-management
author: bobbytreed
ms.author: robreed
ms.date: 05/22/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 1751e8d67f59285d011df33a2d4d1d6d8abcec6a
ms.sourcegitcommit: 0576bcb894031eb9e7ddb919e241e2e3c42f291d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "72376052"
---
# <a name="update-management-solution-in-azure"></a>Lösung für die Updateverwaltung in Azure

Sie können die Lösung für die Updateverwaltung in Azure Automation für Betriebssystemupdates für Ihre Windows- und Linux-Computer in Azure, in lokalen Umgebungen oder bei anderen Cloudanbietern verwalten. Sie können den Status der verfügbaren Updates auf allen Agent-Computern schnell auswerten und die Installation der für den Server erforderlichen Updates initiieren.

Die Updateverwaltung für virtuelle Computer kann direkt in Ihrem Azure Automation-Konto aktiviert werden. Informationen zum Aktivieren der Updateverwaltung für virtuelle Computer über das Automation-Konto finden Sie unter [Verwalten von Updates für mehrere virtuelle Azure-Computer](manage-update-multi.md). Sie können die Updateverwaltung für einen virtuellen Computer auch im Azure-Portal auf der Seite des virtuellen Computers aktivieren. Dieses Szenario ist für virtuelle [Linux](../virtual-machines/linux/tutorial-config-management.md#enable-update-management)- und [Windows](../virtual-machines/windows/tutorial-config-management.md#enable-update-management)-Computer verfügbar.

> [!NOTE]
> Für die Lösung zur Updateverwaltung muss ein Log Analytics-Arbeitsbereich mit Ihrem Automation-Konto verknüpft werden. Eine aktuelle Liste der unterstützten Regionen finden Sie unter [Arbeitsbereichzuordnungen in Azure](./how-to/region-mappings.md). Die Zuordnung von Regionen wirkt sich nicht auf die Möglichkeit aus, virtuelle Computer in einer anderen Region als der Ihres Automation-Kontos zu verwalten.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="solution-overview"></a>Lösungsübersicht

Verwenden Sie für Computer, die mit der Updateverwaltung verwaltet werden, die folgenden Konfigurationen, um Bewertungen und Updatebereitstellungen durchzuführen:

* Microsoft Monitoring Agent (MMA) für Windows oder Linux
* PowerShell Desired State Configuration (DSC) für Linux
* Automation Hybrid Runbook Worker
* Microsoft Update oder Windows Server Update Services (WSUS) für Windows-Computer

Das folgende Diagramm enthält eine konzeptionelle Darstellung des Verhaltens und Datenflusses und zeigt, wie die Lösung alle verbundenen Windows Server- und Linux-Computer eines Arbeitsbereichs bewertet und Sicherheitsupdates darauf anwendet:

![Ablauf des Updateverwaltungsprozesses](./media/automation-update-management/update-mgmt-updateworkflow.png)

Die Updateverwaltung kann für das systeminterne Integrieren von Computern in mehrere Abonnements im selben Mandanten verwendet werden.

Nachdem ein Paket veröffentlicht wurde, dauert es 2 bis 3 Stunden, bis der Patch für Linux-Computer für die Bewertung angezeigt wird. Bei Windows-Computern dauert es 12 bis 15 Stunden, bis der Patch nach der Veröffentlichung für die Bewertung angezeigt wird.

Nachdem ein Computer einen Scanvorgang abgeschlossen hat, um die Konformität für das Update zu überprüfen, leitet der Agent die Informationen gesammelt an Azure Monitor-Protokolle weiter. Auf einem Windows-Computer wird der Konformitätsscan standardmäßig alle 12 Stunden ausgeführt.

Darüber hinaus wird der Update-Konformitätsscan innerhalb von 15 Minuten nach dem MMA-Neustart sowie vor und nach der Updateinstallation initiiert.

Für einen Linux-Computer wird der Konformitätsscan standardmäßig jede Stunde durchgeführt. Wenn der MMA-Agent neu gestartet wird, wird ein Konformitätsscan innerhalb von 15 Minuten eingeleitet.

Die Lösung meldet basierend auf der für die Synchronisierung konfigurierten Quelle, wie aktuell der Computer ist. Wenn der Windows-Computer für das Senden von Meldungen an WSUS konfiguriert ist, können sich die Ergebnisse von den angezeigten Microsoft Update-Ergebnissen unterscheiden. Dies hängt davon ab, wann WSUS zuletzt mit Microsoft Update synchronisiert wurde. Dasselbe gilt für Linux-Computer, die für Meldungen an ein lokales Repository konfiguriert sind (anstatt an ein öffentliches Repository).

> [!NOTE]
> Damit Meldungen an den Dienst ordnungsgemäß erfolgen können, erfordert die Updateverwaltung, dass bestimmte URLs und Ports aktiviert werden. Weitere Informationen zu diesen Anforderungen finden Sie unter [Netzwerkplanung für Hybrid Worker](automation-hybrid-runbook-worker.md#network-planning).

Sie können Softwareupdates auf Computern bereitstellen und installieren, für die die Updates erforderlich sind, indem Sie einen geplante Bereitstellung erstellen. Updates, die als *Optional* klassifiziert sind, sind im Bereitstellungsumfang von Windows-Computern nicht enthalten. Nur erforderliche Updates sind im Bereitstellungsumfang enthalten.

Die geplante Bereitstellung definiert, welche Zielcomputer die entsprechenden Updates erhalten. Dies erfolgt entweder durch explizite Angabe von Computern oder durch Auswahl einer [Computergruppe](../azure-monitor/platform/computer-groups.md), die auf Protokollsuchvorgängen einer bestimmten Reihe von Computern oder auf einer [Azure-Abfrage](automation-update-management-query-logs.md) basiert, die Azure-VMs basierend auf bestimmten Kriterien dynamisch auswählt. Diese Gruppen unterscheiden sich von der [Bereichskonfiguration](../azure-monitor/insights/solution-targeting.md), die nur zur Ermittlung der Computer verwendet wird, die die Verwaltungspakete erhalten, die die Lösung ermöglichen.

Außerdem geben Sie einen Zeitplan an, um einen Zeitraum zu genehmigen und festzulegen, in dem Updates installiert werden dürfen. Dieser Zeitraum wird das Wartungsfenster bezeichnet. Zwanzig Minuten des Wartungsfensters sind für Neustarts reserviert, wenn ein Neustart erforderlich ist und Sie die entsprechende Neustartoption ausgewählt haben. Wenn das Patchen länger als erwartet dauert und im Wartungsfenster weniger als zwanzig Minuten verbleiben, wird kein Neustart durchgeführt.

Updates werden mit Runbooks in Azure Automation installiert. Sie können diese Runbooks nicht anzeigen, und für die Runbooks ist keine Konfiguration erforderlich. Wenn eine Updatebereitstellung erstellt wird, erstellt die Updatebereitstellung einen Zeitplan, nach dem für die einbezogenen Computer zur angegebenen Zeit ein Masterrunbook für das Update gestartet wird. Das Masterrunbook startet ein untergeordnetes Runbook für jeden Agent, um die erforderlichen Updates zu installieren.

Wenn die Datums- bzw. Uhrzeitangabe der Updatebereitstellung erreicht ist, führen die Zielcomputer die Bereitstellung parallel aus. Vor der Installation wird ein Scan ausgeführt, um sicherzustellen, dass die Updates weiterhin erforderlich sind. Für WSUS-Clientcomputer tritt ein Fehler bei der Updatebereitstellung auf, wenn die Updates in WSUS nicht genehmigt sind.

Das Registrieren eines Computers für die Updateverwaltung in mehreren Log Analytics-Arbeitsbereichen (Multihosting) wird nicht unterstützt.

## <a name="clients"></a>Clients

### <a name="supported-client-types"></a>Unterstützte Clienttypen

In der folgenden Tabelle sind die unterstützten Betriebssysteme für Updatebewertungen aufgeführt. Patchen erfordert einen Hybrid Runbook Worker. Informationen zu den Anforderungen für Hybrid Runbook Worker finden Sie in den Installationsanleitungen für [Windows HRW](automation-windows-hrw-install.md#installing-the-windows-hybrid-runbook-worker)und [Linux HRW](automation-linux-hrw-install.md#installing-a-linux-hybrid-runbook-worker).

|Betriebssystem  |Notizen  |
|---------|---------|
|Windows Server 2019 (Datacenter/Datacenter Core/Standard)<br><br>Windows Server 2016 (Datacenter/Datacenter Core/Standard)<br><br>Windows Server 2012 R2 (Datacenter/Standard)<br><br>Windows Server 2012<br><br>Windows Server 2008 R2 (RTM und SP1 Standard)||
|CentOS 6 (x86/x64) und 7 (x64)      | Für Linux-Agents muss Zugriff auf ein Updaterepository bestehen. Für klassifizierungsbasiertes Patchen muss yum Sicherheitsdaten zurückgeben, über die CentOS nicht standardmäßig verfügt. Weitere Informationen zu klassifizierungsbasiertem Patching unter CentOS finden Sie unter [Lösung für die Updateverwaltung in Azure](#linux-2).          |
|Red Hat Enterprise 6 (x86/x64) und 7 (x64)     | Für Linux-Agents muss Zugriff auf ein Updaterepository bestehen.        |
|SUSE Linux Enterprise Server 11 (x86/x64) und 12 (x64)     | Für Linux-Agents muss Zugriff auf ein Updaterepository bestehen.        |
|Ubuntu 14.04 LTS, 16.04 LTS und 18.04 (x86/x64)      |Für Linux-Agents muss Zugriff auf ein Updaterepository bestehen.         |

> [!NOTE]
> VM-Skalierungsgruppen von Azure können über die Updateverwaltung verwaltet werden. Die Updateverwaltung arbeitet mit den Instanzen selbst und nicht mit dem Basisimage. Sie müssen die Updates inkrementell planen, um nicht alle VM-Instanzen auf einmal zu aktualisieren.
> VMSS-Knoten können hinzugefügt werden, indem Sie die Schritte unter [Onboarding eines Nicht-Azure-Computers](automation-tutorial-installed-software.md#onboard-a-non-azure-machine) ausführen.

### <a name="unsupported-client-types"></a>Nicht unterstützte Clienttypen

In der folgenden Tabelle werden die Betriebssysteme aufgelistet, die nicht unterstützt werden:

|Betriebssystem  |Notizen  |
|---------|---------|
|Windows-Client     | Clientbetriebssysteme (z.B. Windows 7 und Windows 10) werden nicht unterstützt.        |
|Windows Server 2016 Nano Server     | Nicht unterstützt.       |
|Azure Kubernetes Service-Knoten | Nicht unterstützt. Verwenden Sie den Patchprozess, dargelegt unter [Anwenden von Sicherheits- und Kernelupdates auf Linux-Knoten in Azure Kubernetes Service (AKS)](../aks/node-updates-kured.md).|

### <a name="client-requirements"></a>Clientanforderungen

Die folgenden Informationen beschreiben betriebssystemspezifische Clientanforderungen.  Weitere Informationen finden Sie auch im Artikel zur [Netzwerkplanung](#ports).

#### <a name="windows"></a>Windows

Windows-Agents müssen für die Kommunikation mit einem WSUS-Server konfiguriert sein oder über Zugriff auf Microsoft Update verfügen.

Sie können die Updateverwaltung mit System Center Configuration Manager verwenden. Weitere Informationen zu den Integrationsszenarien finden Sie unter [Integrieren von System Center Configuration Manager in die Updateverwaltung](oms-solution-updatemgmt-sccmintegration.md#configuration). Der [Windows-Agent](../azure-monitor/platform/agent-windows.md) ist erforderlich. Dieser Agent wird automatisch installiert, wenn Sie das Onboarding eines virtuellen Azure-Computer ausführen.

Über Azure Marketplace bereitgestellte virtuelle Windows-Computer sind standardmäßig so konfiguriert, dass sie automatisch Updates von Windows Update Service erhalten. Dieses Verhalten ändert sich nicht, wenn Sie diese Lösung hinzufügen oder Ihrem Arbeitsbereich virtuelle Windows-Computer hinzufügen. Wenn Sie Updates mithilfe dieser Lösung nicht aktiv verwalten, gilt das Standardverhalten (Updates werden automatisch angewendet).

> [!NOTE]
> Ein Benutzer kann eine Gruppenrichtlinie so ändern, dass Computerneustarts nur vom Benutzer ausgeführt werden können, nicht aber vom System. Verwaltete Computer können hängen bleiben, wenn die Updateverwaltung nicht über die Berechtigungen zum Neustarten des Computers ohne manuelle Interaktion des Benutzers verfügt.
>
> Weitere Informationen finden Sie unter [Konfigurieren der Gruppenrichtlinieneinstellungen für automatische Updates](https://docs.microsoft.com/en-us/windows-server/administration/windows-server-update-services/deploy/4-configure-group-policy-settings-for-automatic-updates).

#### <a name="linux"></a>Linux

Für Linux muss der Computer über Zugriff auf ein Updaterepository verfügen. Das Updaterepository kann privat oder öffentlich sein. Für die Interaktion mit der Updateverwaltung ist TLS 1.1 oder TLS 1.2 erforderlich. Diese Lösung unterstützt keinen Log Analytics-Agent für Linux, der für die Berichterstattung an mehrere Log Analytics-Arbeitsbereiche konfiguriert ist.  Auf dem Computer muss außerdem Python 2.x installiert sein.

Weitere Informationen zum Installieren des Log Analytics-Agents für Linux und zum Herunterladen der aktuellen Version finden Sie unter [Log Analytics Agent for Linux (Log Analytics-Agent für Linux)](https://github.com/microsoft/oms-agent-for-linux). Informationen zur Installation des Log Analytics-Agents für Windows finden Sie unter [Verbinden von Windows-Computern mit dem Log Analytics-Dienst in Azure](../log-analytics/log-analytics-windows-agent.md).

Virtuelle Computer, die auf der Grundlage der über Azure Marketplace erhältlichen On-Demand-RHEL-Images (Red Hat Enterprise Linux) erstellt werden, werden für den Zugriff auf die in Azure bereitgestellte [Red Hat-Updateinfrastruktur (RHUI)](../virtual-machines/virtual-machines-linux-update-infrastructure-redhat.md) registriert. Alle anderen Linux-Distributionen müssen über das Onlinedateirepository der Distributionen gemäß den unterstützten Methoden der jeweiligen Distribution aktualisiert werden.

## <a name="permissions"></a>Berechtigungen

Zum Erstellen und Verwalten von Updatebereitstellungen benötigen Sie bestimmte Berechtigungen. Weitere Informationen zu diesen Berechtigungen finden Sie unter [Rollenbasierter Zugriff: Updateverwaltung](automation-role-based-access-control.md#update-management).

## <a name="solution-components"></a>Lösungskomponenten

Die Lösung besteht aus den folgenden Ressourcen. Die Ressourcen werden Ihrem Automation-Konto hinzugefügt. Es handelt sich um direkt verbundene Agents oder um Agents in einer durch Operations Manager verbundenen Verwaltungsgruppe.

### <a name="hybrid-worker-groups"></a>Hybrid Worker-Gruppen

Nachdem Sie diese Lösung aktiviert haben, werden alle direkt mit dem Log Analytics-Arbeitsbereich verbundenen Windows-Computer automatisch als Hybrid Runbook Worker konfiguriert, um die in dieser Lösung enthaltenen Runbooks zu unterstützen.

Jeder von der Lösung verwaltete Windows-Computer wird im Bereich **Hybrid Worker-Gruppen** als **Hybrid Worker-Systemgruppe** für das Automation-Konto aufgeführt. Die Lösungen verwenden die Benennungskonvention *Hostname FQDN_GUID*. Es ist nicht möglich, diese Gruppen mit Runbooks in Ihrem Konto zu erreichen. Dieser Versuch führt zu einem Fehler. Diese Gruppen sind nur für die Unterstützung der Verwaltungslösung bestimmt.

Sie können die Windows-Computer einer Hybrid Runbook Worker-Gruppe in Ihrem Automation-Konto hinzufügen, um Automation-Runbooks zu unterstützen, wenn Sie für die Lösung und die Mitgliedschaft in der Hybrid Runbook Worker-Gruppe dasselbe Konto verwenden. Diese Funktionalität wurde in Version 7.2.12024.0 des Hybrid Runbook Worker hinzugefügt.

### <a name="management-packs"></a>Management Packs

Wenn Ihre System Center Operations Manager-Verwaltungsgruppe mit einem Log Analytics-Arbeitsbereich verbunden ist, werden in Operations Manager die folgenden Management Packs installiert. Diese Management Packs werden nach dem Hinzufügen dieser Lösung auch auf direkt verbundenen Windows-Computern installiert. Für diese Management Packs fällt kein Konfigurations- oder Verwaltungsaufwand an.

* Microsoft System Center Advisor Update Assessment Intelligence Pack (Microsoft.IntelligencePacks.UpdateAssessment)
* Microsoft.IntelligencePack.UpdateAssessment.Configuration (Microsoft.IntelligencePack.UpdateAssessment.Configuration)
* Update Deployment MP

> [!NOTE]
> Wenn Sie eine Operations Manager 1807- oder 2019-Verwaltungsgruppe mit auf Verwaltungsgruppenebene konfigurierten Agenten einem Arbeitsbereich zuordnen, besteht die Problemumgehung, damit diese angezeigt werden, momentan darin, **IsAutoRegistrationEnabled** in der **Microsoft.IntelligencePacks.AzureAutomation.HybridAgent.Init**-Regel mit **True** zu überschreiben.

Weitere Informationen zur Aktualisierung von Management Packs finden Sie unter [Herstellen einer Verbindung zwischen Operations Manager und Log Analytics](../azure-monitor/platform/om-agents.md).

> [!NOTE]
> Damit Systeme mit dem Operations Manager-Agent vollständig durch die Updateverwaltung verwaltet werden können, muss der Agent auf den Microsoft Monitoring Agent aktualisiert werden. Informationen zum Aktualisieren des Agents finden Sie unter [Aktualisieren eines Operations Manager-Agents](https://docs.microsoft.com/system-center/scom/deploy-upgrade-agents). Für Umgebungen, die Operations Manager verwenden, ist es erforderlich, dass Sie System Center Operations Manager 2012 R2 UR14 oder höher ausführen.

## <a name="data-collection"></a>Datensammlung

### <a name="supported-agents"></a>Unterstützte Agents

In der folgenden Tabelle sind die verbundenen Quellen beschrieben, die von der Lösung unterstützt werden:

| Verbundene Quelle | Unterstützt | BESCHREIBUNG |
| --- | --- | --- |
| Windows-Agents |Ja |Die Lösung sammelt Informationen zu Systemupdates aus Windows-Agents und initiiert dann die Installation von erforderlichen Updates. |
| Linux-Agents |Ja |Die Lösung sammelt Informationen zu Systemupdates von Linux-Agents und initiiert dann die Installation von erforderlichen Updates für unterstützte Distributionen. |
| Operations Manager-Verwaltungsgruppe |Ja |Die Lösung sammelt Informationen zu Systemupdates von Agents in einer verbundenen Verwaltungsgruppe.<br/>Es ist keine direkte Verbindung zwischen dem Operations Manager-Agent und Azure Monitor-Protokollen erforderlich. Daten werden von der Verwaltungsgruppe an den Log Analytics-Arbeitsbereich weitergeleitet. |

### <a name="collection-frequency"></a>Sammlungshäufigkeit

Für jeden verwalteten Windows-Computer wird zwei Mal pro Tag ein Scanvorgang ausgeführt. Alle 15 Minuten wird die Windows-API aufgerufen, um den letzten Updatezeitpunkt abzufragen und zu ermitteln, ob sich der Status geändert hat. Wenn sich der Status geändert hat, wird eine Konformitätsprüfung eingeleitet.

Für jeden verwalteten Linux-Computer wird jede Stunde ein Scanvorgang ausgeführt.

Es kann zwischen 30 Minuten und sechs Stunden dauern, bis im Dashboard aktualisierte Daten von verwalteten Computern angezeigt werden.

Die durchschnittliche Nutzung von Azure Monitor-Protokolldaten für einen Computer mithilfe der Updateverwaltung beträgt ungefähr 25 MB pro Monat. Dieser Wert ist nur ein Näherungswert und kann sich abhängig von Ihrer Umgebung ändern. Es wird empfohlen, Ihre Umgebung zu überwachen, um die genaue Nutzung zu ermitteln.

## <a name="ports"></a>Netzwerkplanung

Die folgenden Adressen sind speziell für die Updateverwaltung erforderlich. Die Kommunikation mit diesen Adressen erfolgt über Port 443.

|Azure – Öffentlich  |Azure Government  |
|---------|---------|
|*.ods.opinsights.azure.com     |*.ods.opinsights.azure.us         |
|*.oms.opinsights.azure.com     | *.oms.opinsights.azure.us        |
|*.blob.core.windows.net|*.blob.core.usgovcloudapi.net|
|*.azure-automation.net|*.azure-automation.us|

Bei Windows-Computern müssen Sie auch Datenverkehr zu allen Endpunkten zulassen, die für Windows Update erforderlich sind.  Sie finden eine aktualisierte Liste der erforderlichen Endpunkte unter [Probleme im Zusammenhang mit HTTP/Proxy](/windows/deployment/update/windows-update-troubleshooting#issues-related-to-httpproxy). Wenn Sie über einen lokalen [Windows Update Server](/windows-server/administration/windows-server-update-services/plan/plan-your-wsus-deployment) verfügen, müssen Sie auch Datenverkehr zu dem in Ihrem [WSUS-Schlüssel](/windows/deployment/update/waas-wu-settings#configuring-automatic-updates-by-editing-the-registry) angegebenen Server zulassen.

Informationen zu den erforderlichen Endpunkten für Red Hat Linux-Computer finden Sie unter [Die IPs für die RHUI-Inhaltsübermittlungsserver](../virtual-machines/linux/update-infrastructure-redhat.md#the-ips-for-the-rhui-content-delivery-servers). Informationen zu anderen Linux-Distributionen finden Sie in der jeweiligen Dokumentation des Anbieters.

Weitere Informationen zu Ports, die für den Hybrid Runbook Worker erforderlich sind, finden Sie unter [Ports für Hybrid Worker-Rollen](automation-hybrid-runbook-worker.md#hybrid-worker-role).

Es wird empfohlen, beim Definieren von Ausnahmen die aufgeführten Adressen zu verwenden. Für IP-Adressen können Sie die [IP-Bereiche des Microsoft Azure-Rechenzentrums](https://www.microsoft.com/download/details.aspx?id=41653) herunterladen. Diese Datei mit den jeweils aktuellen bereitgestellten Bereichen und allen anstehenden Änderungen an den IP-Adressbereichen wird wöchentlich veröffentlicht.

Befolgen Sie die Anweisungen in [Verbinden von Computern ohne Internetzugang](../azure-monitor/platform/gateway.md), um Computer zu konfigurieren, die keinen Internetzugang besitzen.

## <a name="view-update-assessments"></a>Anzeigen der Updatebewertungen

Wählen Sie in Ihrem Automation-Konto **Updateverwaltung** aus, um den Status Ihrer Computer anzuzeigen.

Diese Ansicht enthält Informationen zu Ihren Computern, zu fehlenden Updates, zu Updatebereitstellungen und geplanten Updatebereitstellungen. In der Spalte **KONFORMITÄT** können Sie sehen, wann der Computer zuletzt bewertet wurde. In der Spalte **UPDATE-AGENT-BEREITSCHAFT** können Sie die Integrität des Update-Agents anzeigen. Wenn ein Problem vorliegt, wählen Sie den Link aus, um zur Dokumentation für die Problembehandlung zu navigieren und zu erfahren, welche Schritte zum Beheben des Problems erforderlich sind.

Um eine Protokollsuche auszuführen, die Informationen zum Computer, zu Updates oder zu Bereitstellungen zurückgibt, wählen Sie das Element in der Liste aus. Der Bereich **Protokollsuche** wird mit einer Abfrage für das ausgewählte Element geöffnet:

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

Für Linux kann die Updateverwaltung zwischen kritischen und Sicherheitsupdates in der Cloud unterscheiden. Sie zeigt Bewertungsdaten aufgrund der Datenanreicherung in der Cloud an. Für das Patchen verwendet die Updateverwaltung Klassifizierungsdaten, die auf dem Computer verfügbar sind. Im Gegensatz zu anderen Distributionen verfügt CentOS nicht standardmäßig über diese Informationen. Wenn Sie CentOS-Computer für das Zurückgeben von Sicherheitsdaten für den folgenden Befehl konfiguriert haben, kann die Updateverwaltung basierend auf Klassifizierungen Patchvorgänge ausführen.

```bash
sudo yum -q --security check-update
```

Es gibt derzeit keine unterstützte Methode zum Aktivieren einer nativen Klassifizierungsdatenverfügbarkeit unter CentOS. Zu diesem Zeitpunkt wird nur Unterstützung nach bestem Wissen für Kunden bereitgestellt, die dies möglicherweise auf eigene Weise aktiviert haben.

## <a name="integrate-with-system-center-configuration-manager"></a>Integrieren in System Center Configuration Manager

Kunden, die in System Center Configuration Manager investiert haben, um PCs, Server und mobile Geräte zu verwalten, profitieren auch von der Leistungsstärke und dem Funktionsumfang von Configuration Manager bei der Verwaltung von Softwareupdates. Configuration Manager ist Teil ihres Softwareupdateverwaltungs-Zyklus.

Wie Sie die Verwaltungslösung in System Center Configuration Manager integrieren, erfahren Sie unter [Integrieren von System Center Configuration Manager und Updateverwaltung](oms-solution-updatemgmt-sccmintegration.md).

### <a name="third-party-patches-on-windows"></a>Drittanbieterpatches unter Windows

In der Updateverwaltung wird das lokal konfigurierte Updaterepository verwendet, um unterstützte Windows-Systeme zu patchen. Dabei handelt es sich entweder um WSUS oder um Windows Update. Mit Tools wie [System Center Updates Publisher](/sccm/sum/tools/updates-publisher) (Updates Publisher) können Sie benutzerdefinierte Updates in WSUS veröffentlichen. Dadurch kann die Updateverwaltung Computer, auf denen System Center Configuration Manager als Updaterepository verwendet wird, mit Software von Drittanbietern patchen. Informationen zum Konfigurieren von Updates Publisher finden Sie unter [Installieren von Updates Publisher](/sccm/sum/tools/install-updates-publisher).

## <a name="patch-linux-machines"></a>Patchen von Linux-Computern

In den folgenden Abschnitten werden mögliche Probleme beim Patchen von Linux-Computern erläutert.

### <a name="unexpected-os-level-upgrades"></a>Unerwartete Upgrades auf Betriebssystemebene

Bei einigen Linux-Varianten (z.B. Red Hat Enterprise Linux) können Upgrades auf Betriebssystemebene über Pakete erfolgen. Dies kann ggf. zur Ausführung der Updateverwaltung führen, wodurch sich die Versionsnummer des Betriebssystems ändert. Dieses Verhalten ist beabsichtigt, da die Updateverwaltung die gleichen Methoden für Updatepakete verwendet, die auch ein Administrator lokal auf dem Linux-Computer nutzt.

Um zu vermeiden, dass die Betriebssystemversion durch die Ausführung der Updateverwaltung aktualisiert wird, verwenden Sie das **Ausschluss**-Feature.

In Red Hat Enterprise Linux lautet der Name des auszuschließenden Pakets „redhat-release-server.x86_64“.

![Auszuschließende Pakete für Linux](./media/automation-update-management/linuxpatches.png)

### <a name="critical--security-patches-arent-applied"></a>Kritische/Sicherheitspatches werden nicht angewendet

Beim Bereitstellen von Updates für einen Linux-Computer können Sie Updateklassifizierungen auswählen. Dadurch werden die angewandten Updates auf den Computer gefiltert, der die definierten Kriterien erfüllt. Dieser Filter wird lokal auf dem Computer angewendet, wenn das Update bereitgestellt wird.

Da die Updateverwaltung die Updateergänzung in der Cloud ausführt, werden einige Updates in der Updateverwaltung möglicherweise als Update mit Sicherheitsauswirkung gekennzeichnet, obwohl der lokale Computer nicht über diese Informationen verfügt. Daher gibt es beim Anwenden kritischer Updates auf einem Linux-Computer möglicherweise Updates, die nicht als Update mit Sicherheitsauswirkung auf diesen Computer gekennzeichnet sind. Diese Updates werden dann nicht angewendet.

Allerdings wird die Updateverwaltung diesen Computer wahrscheinlich weiterhin als nicht kompatibel melden, da sie über zusätzliche Informationen zum relevanten Update verfügt.

Das Bereitstellen von Updates nach einer Updateklassifizierung funktioniert unter CentOS nicht standardmäßig. Wählen Sie zum ordnungsgemäßen Bereitstellen von Updates für CentOS alle Klassifizierungen aus, um sicherzustellen, dass die Updates angewendet werden. Für SUSE kann die Auswahl *nur* von „Other Updates“ als Klassifizierung möglicherweise dazu führen, dass einige Sicherheitsupdates installiert werden, wenn zuerst Sicherheitsupdates im Zusammenhang mit zypper (Paket-Manager) oder dessen Abhängigkeiten erforderlich sind. Dieses Verhalten ist eine Einschränkung von zypper. In einigen Fällen müssen Sie die Updatebereitstellung ggf. erneut ausführen. Überprüfen Sie das Updateprotokoll.

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

Fahren Sie mit dem Tutorial fort, um zu erfahren, wie Updates für Ihre virtuellen Windows-Computer verwaltet werden.

> [!div class="nextstepaction"]
> [Verwalten von Updates und Patches für Ihre virtuellen Azure Windows-Computer](automation-tutorial-update-management.md)

* Verwenden Sie Protokollsuchen in [Azure Monitor-Protokollen](../log-analytics/log-analytics-log-searches.md), um ausführliche Aktualisierungsdaten anzuzeigen.
* [Erstellen Sie Warnungen](automation-tutorial-update-management.md#configure-alerts) für den Status der Updatebereitstellung.

* Weitere Informationen zur Interaktion mit der Updateverwaltung über die REST-API finden Sie unter [Softwareupdatekonfigurationen](/rest/api/automation/softwareupdateconfigurations).
* Informationen zur Problembehandlung der Updateverwaltung finden Sie unter [Problembehandlung der Updateverwaltung](troubleshoot/update-management.md).
