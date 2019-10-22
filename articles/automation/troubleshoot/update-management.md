---
title: Beheben von Fehlern mit Updateverwaltung
description: Erfahren Sie, wie Sie Fehler mit Updateverwaltung beheben können.
services: automation
author: bobbytreed
ms.author: robreed
ms.date: 05/31/2019
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: 89c5c849ffdbe70ae449f41ac08471c84f2fb5bc
ms.sourcegitcommit: 0576bcb894031eb9e7ddb919e241e2e3c42f291d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "72372624"
---
# <a name="troubleshooting-issues-with-update-management"></a>Behandeln von Problemen mit Updateverwaltung

In diesem Artikel werden Lösungen zum Beheben von Problemen beschrieben, die bei Verwendung der Updateverwaltung auftreten können.

Es gibt eine Agent-Problembehandlung für den Hybrid Worker-Agent, mit dem das zugrunde liegende Problem ermittelt werden kann. Informationen zur Problembehandlung finden Sie unter [Beheben von Problemen mit dem Update-Agent](update-agent-issues.md). Bei allen anderen Problemen finden Sie weiter unten ausführliche Informationen zu möglichen Problemen.

Sollten Sie Probleme beim Integrieren der Lösung in einen virtuellen Computers haben, suchen Sie auf dem lokalen Computer im Ereignisprotokoll **Operations Manager** unter **Anwendungs- und Dienstprotokolle** nach Ereignissen mit der Ereignis-ID **4502** und einer Ereignismeldung, die **Microsoft.EnterpriseManagement.HealthService.AzureAutomation.HybridAgent** enthält.

Der folgende Abschnitt enthält spezifische Fehlermeldungen und passende Lösungsvorschläge. Informationen zu anderen Integrationsproblemen finden Sie unter [Problembehandlung bei der Integration von Lösungen](onboarding.md).

## <a name="nologs"></a>Szenario: Im Portal werden unter „Updateverwaltung“ keine Computer angezeigt

### <a name="issue"></a>Problem

Sie können auf die folgenden Szenarien stoßen:

* In der Ansicht „Updateverwaltung“ einer VM wird Ihr Computer als **Nicht konfiguriert** angezeigt

* Ihre Computer fehlen in der Ansicht „Updateverwaltung“ Ihres Automation-Kontos

* Bei einigen Ihrer Computer wird **Nicht bewertet** unter **Compliance** angezeigt, es werden aber Heartbeatdaten in Azure Monitor-Protokolle für den Hybrid Runbook Worker, jedoch nicht für die Updateverwaltung angezeigt.

### <a name="cause"></a>Ursache

Dies kann durch potenzielle lokale Konfigurationsprobleme oder eine falsch konfigurierte Bereichskonfiguration verursacht werden.

Der Hybrid Runbook Worker muss eventuell erneut registriert und installiert werden.

Möglicherweise haben Sie in Ihrem Arbeitsbereich ein Kontingent festgelegt, das erreicht ist und verhindert, dass Daten gespeichert werden.

### <a name="resolution"></a>Lösung

* Führen Sie je nach Betriebssystem die Problembehandlung für [Windows](update-agent-issues.md#troubleshoot-offline) oder [Linux](update-agent-issues-linux.md#troubleshoot-offline) aus.

* Vergewissern Sie sich, dass Ihr Computer dem richtigen Arbeitsbereich Bericht erstattet. Überprüfen Sie, welchem Arbeitsbereich Ihr Computer Berichte erstattet. Entsprechende Anweisungen dazu finden Sie unter [Überprüfen der Agent-Konnektivität mit Log Analytics](../../azure-monitor/platform/agent-windows.md#verify-agent-connectivity-to-log-analytics). Vergewissern Sie sich anschließend, dass dies der Arbeitsbereich ist, der mit Ihrem Azure Automation-Konto verknüpft ist. Um dies zu bestätigen, navigieren Sie zu Ihrem Automation-Konto und klicken Sie unter **Verwandte Ressourcen** auf **Verknüpfter Arbeitsbereich**.

* Prüfen Sie, ob die Computer in Ihrem Log Analytics-Arbeitsbereich angezeigt werden. Führen Sie in Ihrem Log Analytics-Arbeitsbereich, der mit Ihrem Automation-Konto verknüpft ist, die folgende Abfrage aus. Wenn Ihr Computer nicht in den Suchergebnissen angezeigt wird, hat sich Ihr Computer nicht kürzlich eingecheckt, was bedeutet, dass höchstwahrscheinlich ein lokales Konfigurationsproblem vorliegt und dass Sie [Agent neu installieren können](../../azure-monitor/learn/quick-collect-windows-computer.md#install-the-agent-for-windows). Wenn Ihr Computer in den Suchergebnissen aufgeführt ist, müssen Sie die im folgenden Aufzählungspunkt angegebene Bereichskonfiguration überprüfen.

  ```loganalytics
  Heartbeat
  | summarize by Computer, Solutions
  ```

* Suchen Sie nach Bereichskonfigurationsproblemen. Die [Bereichskonfiguration](../automation-onboard-solutions-from-automation-account.md#scope-configuration) bestimmt, welche Computer für die Lösung konfiguriert werden. Wenn Ihr Computer zwar in Ihrem Arbeitsbereich, aber nicht im **Updateverwaltungs**-Portal angezeigt wird, müssen Sie die Bereichskonfiguration auf die Computer konfigurieren. Informationen hierzu finden Sie unter [Integrieren von Computern in den Arbeitsbereich](../automation-onboard-solutions-from-automation-account.md#onboard-machines-in-the-workspace).

* Führen Sie in Ihrem Arbeitsbereich die folgende Abfrage aus. Wenn Sie das Ergebnis `Data collection stopped due to daily limit of free data reached. Ingestion status = OverQuota` sehen, haben Sie ein Kontingent für Ihren Arbeitsbereich festgelegt, das erreicht ist, wodurch das Speichern von Daten verhindert wird. Wechseln Sie in Ihrem Arbeitsbereich zu **Nutzung und geschätzte Kosten** > **Datenvolumenverwaltung**. Überprüfen Sie das eingerichtete Kontingent, oder heben Sie es auf.

  ```loganalytics
  Operation
  | where OperationCategory == 'Data Collection Status'
  | sort by TimeGenerated desc
  ```

* Wenn Sie das Problem nicht anhand der obigen Schritte beheben können, führen Sie die Schritte unter [Bereitstellen eines Windows Hybrid Runbook Workers](../automation-windows-hrw-install.md) aus, um den Hybrid Worker für Windows neu zu installieren (bzw. [Bereitstellen eines Linux Hybrid Runbook Workers](../automation-linux-hrw-install.md) für Linux).

## <a name="rp-register"></a>Szenario: Der Automation-Ressourcenanbieter für Abonnements kann nicht registriert werden.

### <a name="issue"></a>Problem

Beim Arbeiten mit Lösungen in Ihrem Automation-Konto erhalten Sie eventuell folgenden Fehler.

```error
Error details: Unable to register Automation Resource Provider for subscriptions:
```

### <a name="cause"></a>Ursache

Der Automation-Ressourcenanbieter ist im Abonnement nicht registriert.

### <a name="resolution"></a>Lösung

Sie können die Automation-Ressourcenanbieter mit den folgenden Schritten im Azure-Portal registrieren:

1. Klicken Sie unten in der Azure-Dienstliste auf **Alle Dienste**, und wählen Sie dann in der Dienstgruppe _Allgemein_ die Option **Abonnements** aus.
2. Wählen Sie Ihr Abonnement aus.
3. Klicken Sie unter _Einstellungen_ auf **Ressourcenanbieter**.
4. Überprüfen Sie in der Liste der Ressourcenanbieter, ob der Ressourcenanbieter **Microsoft.Automation** registriert ist.
5. Wenn der Anbieter nicht aufgeführt ist, registrieren Sie den Anbieter **Microsoft.Automation** mit den unter [](/azure/azure-resource-manager/resource-manager-register-provider-errors) aufgeführten Schritten.

## <a name="components-enabled-not-working"></a>Szenario: Die Komponenten für die Lösung „Updateverwaltung“ wurden aktiviert, und diese VM wird nun konfiguriert.

### <a name="issue"></a>Problem

Die folgende Meldung wird 15 Minuten nach dem Onboarding auf einer VM angezeigt:

```error
The components for the 'Update Management' solution have been enabled, and now this virtual machine is being configured. Please be patient, as this can sometimes take up to 15 minutes.
```

### <a name="cause"></a>Ursache

Dieser Fehler kann die folgenden Gründe haben:

1. Die Kommunikation an das Automation-Konto wird blockiert.
2. Die VM, die integriert wird, stammt möglicherweise von einem geklonten Cloudcomputer, dessen System nicht mit dem installierten Microsoft Monitoring Agent vorbereitet wurde.

### <a name="resolution"></a>Lösung

1. Sehen Sie sich den Abschnitt [Konfigurieren des Netzwerks](../automation-hybrid-runbook-worker.md#network-planning) an, um zu erfahren, welche Adressen und Ports zugelassen werden müssen, damit die Updateverwaltung funktioniert.
2. Wenn Sie ein geklontes Image verwenden:
   1. Entfernen Sie in Ihrem Log Analytics-Arbeitsbereich den virtuellen Computer aus der gespeicherten Suche für die Bereichskonfiguration `MicrosoftDefaultScopeConfig-Updates`, sofern er angezeigt wird. Gespeicherte Suchen finden Sie unter **Allgemein** in Ihrem Arbeitsbereich.
   2. Führen Sie `Remove-Item -Path "HKLM:\software\microsoft\hybridrunbookworker" -Recurse -Force` aus.
   3. Führen Sie `Restart-Service HealthService` aus, um `HealthService` neu zu starten. Der Schlüssel wird erneut erstellt, und eine neue UUID wird generiert.
   4. Wenn dies nicht funktioniert, bereiten Sie mit Sysprep das Image zunächst vor, und installieren Sie den MMA-Agent anschließend.

## <a name="multi-tenant"></a>Szenario: Sie erhalten einen Fehler aufgrund eines verknüpften Abonnements, wenn Sie eine Updatebereitstellung für Computer auf einem anderen Azure-Mandanten erstellen.

### <a name="issue"></a>Problem

Sie erhalten beim Versuch, eine Updatebereitstellung für Computer auf einem anderen Azure-Mandanten zu erstellen, die folgende Fehlermeldung:

```error
The client has permission to perform action 'Microsoft.Compute/virtualMachines/write' on scope '/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resourceGroupName/providers/Microsoft.Automation/automationAccounts/automationAccountName/softwareUpdateConfigurations/updateDeploymentName', however the current tenant '00000000-0000-0000-0000-000000000000' is not authorized to access linked subscription '00000000-0000-0000-0000-000000000000'.
```

### <a name="cause"></a>Ursache

Dieser Fehler tritt bei der Erstellung einer Updatebereitstellung auf, bei der virtuelle Azure-Computer eines anderen Mandanten in eine Updatebereitstellung einbezogen werden.

### <a name="resolution"></a>Lösung

Sie müssen die folgende Problemumgehung nutzen, um die Planung durchzuführen. Verwenden Sie das Cmdlet [New-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/new-azurermautomationschedule) mit dem Schalter `-ForUpdate`, um einen Zeitplan zu erstellen, und das Cmdlet [New-AzureRmAutomationSoftwareUpdateConfiguration](/powershell/module/azurerm.automation/new-azurermautomationsoftwareupdateconfiguration
), um die Computer aus dem anderen Mandanten an den Parameter `-NonAzureComputer` zu übergeben. Dies wird anhand des folgenden Beispiels veranschaulicht:

```azurepowershell-interactive
$nonAzurecomputers = @("server-01", "server-02")

$startTime = ([DateTime]::Now).AddMinutes(10)

$s = New-AzureRmAutomationSchedule -ResourceGroupName mygroup -AutomationAccountName myaccount -Name myupdateconfig -Description test-OneTime -OneTime -StartTime $startTime -ForUpdate

New-AzureRmAutomationSoftwareUpdateConfiguration  -ResourceGroupName $rg -AutomationAccountName $aa -Schedule $s -Windows -AzureVMResourceId $azureVMIdsW -NonAzureComputer $nonAzurecomputers -Duration (New-TimeSpan -Hours 2) -IncludedUpdateClassification Security,UpdateRollup -ExcludedKbNumber KB01,KB02 -IncludedKbNumber KB100
```

## <a name="node-reboots"></a>Szenario: Unerklärliche Neustarts

### <a name="issue"></a>Problem

Sie haben **Neustartsteuerung** mit **Nie neu starten** konfiguriert, aber Computer werden nach der Installation von Updates weiterhin neu gestartet.

### <a name="cause"></a>Ursache

Das Verhalten von Windows Update lässt sich über mehrere Registrierungsschlüssel ändern, mit denen das Neustartverhalten geändert werden kann.

### <a name="resolution"></a>Lösung

Überprüfen Sie die unter [Konfigurieren automatischer Updates durch Bearbeiten der Registrierung](/windows/deployment/update/waas-wu-settings#configuring-automatic-updates-by-editing-the-rej7uijui7jgistry) und [Zum Verwalten des Neustarts verwendete Registrierungsschlüssel](/windows/deployment/update/waas-restart#registry-keys-used-to-manage-restart) aufgeführten Registrierungsschlüssel, um sicherzustellen, dass Ihre Computer ordnungsgemäß konfiguriert sind.

## <a name="failed-to-start"></a>Szenario: Beim Starten eines Computers in einer Updatebereitstellung wird die Fehlermeldung „Fehler beim Starten“ zurückgegeben

### <a name="issue"></a>Problem

Für einen Computer wird der Status **Fehler beim Starten** angezeigt. Wenn Sie sich die Detailinformationen für den Computer ansehen, wird der folgende Fehler angezeigt:

```error
Failed to start the runbook. Check the parameters passed. RunbookName Patch-MicrosoftOMSComputer. Exception You have requested to create a runbook job on a hybrid worker group that does not exist.
```

### <a name="cause"></a>Ursache

Dieser Fehler kann folgende Ursachen haben:

* Der Computer ist nicht mehr vorhanden.
* Der Computer ist ausgeschaltet und nicht erreichbar.
* Für den Computer besteht ein Netzwerkverbindungsproblem, und der Hybrid Worker auf dem Computer ist nicht erreichbar.
* Ein Update von Microsoft Monitoring Agent hat zu einer Änderung von SourceComputerId geführt.
* Die Ausführung des Updates wurde möglicherweise gedrosselt, wenn das Limit von 2.000 gleichzeitigen Aufträgen in einem Automation-Konto erreicht wurde. Jede Bereitstellung kann als Auftrag betrachtet werden, und jeder Computer in einer Updatebereitstellung zählt als Auftrag. Jeder andere Automatisierungsauftrag oder jede Updatebereitstellung, der bzw. die in Ihrem Automation-Konto ausgeführt wird, wird auf das Limit für gleichzeitige Aufträge angerechnet.

### <a name="resolution"></a>Lösung

Verwenden Sie [dynamische Gruppen](../automation-update-management-groups.md) (falls vorhanden) für Ihre Updatebereitstellungen.

* Stellen Sie sicher, dass der Computer noch vorhanden und erreichbar ist. Wenn er nicht mehr vorhanden ist, bearbeiten Sie Ihre Bereitstellung und entfernen Sie den Computer.
* Im Abschnitt zu [Netzwerkplanung](../automation-update-management.md#ports) finden Sie eine Liste der Ports und Adressen, die für die Updateverwaltung erforderlich sind. Stellen Sie sicher, dass Ihr Computer diese Anforderungen erfüllt.
* Führen Sie die folgende Abfrage in Log Analytics aus, um nach Computern in Ihrer Umgebung zu suchen, bei denen sich `SourceComputerId` geändert hat. Suchen Sie nach Computern, die denselben `Computer`-Wert, aber einen anderen `SourceComputerId`-Wert haben. Sobald Sie die betroffenen Computer gefunden haben, müssen Sie die Updatebereitstellungen für diese Computer bearbeiten und die Computer entfernen und dann erneut hinzufügen, damit `SourceComputerId` den richtigen Wert aufweist.

   ```loganalytics
   Heartbeat | where TimeGenerated > ago(30d) | distinct SourceComputerId, Computer, ComputerIP
   ```

## <a name="updates-nodeployment"></a>Szenario: Installation von Updates ohne Bereitstellung

### <a name="issue"></a>Problem

Wenn Sie einen Windows-Computer in der Updateverwaltung registrieren, kann es sein, dass Updates ohne Bereitstellung installiert werden.

### <a name="cause"></a>Ursache

Unter Windows werden Updates automatisch installiert, sobald sie verfügbar sind. Dies kann verwirrend sein, wenn Sie ein Update nicht für die Bereitstellung auf dem Computer eingeplant haben.

### <a name="resolution"></a>Lösung

Der Windows-Registrierungsschlüssel `HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU` hat die Standardeinstellung „4“: **Automatisches Herunterladen und Installieren**.

Für Clients für die Updateverwaltung empfehlen wir Ihnen, diesen Schlüssel auf „3“ festzulegen: **Automatisches Herunterladen, keine automatische Installation**.

Weitere Informationen finden Sie unter [Automatische Updates konfigurieren](https://docs.microsoft.com/windows/deployment/update/waas-wu-settings#configure-automatic-updates).

## <a name="machine-already-registered"></a>Szenario: Computer ist bereits bei einem anderen Konto registriert.

### <a name="issue"></a>Problem

Sie erhalten die folgende Fehlermeldung:

```error
Unable to Register Machine for Patch Management, Registration Failed with Exception System.InvalidOperationException: {"Message":"Machine is already registered to a different account."}
```

### <a name="cause"></a>Ursache

Der Computer ist bereits in einen anderen Arbeitsbereich für Updateverwaltung integriert.

### <a name="resolution"></a>Lösung

1. Befolgen Sie die Schritte unter [Im Portal werden unter „Updateverwaltung“ keine Computer angezeigt](#nologs), um sicherzustellen, dass der Computer im richtigen Arbeitsbereich meldet.
2. Bereinigen Sie alte Artefakte auf dem Computer durch [Löschen der Hybrid-Runbook-Gruppe](../automation-hybrid-runbook-worker.md#remove-a-hybrid-worker-group), und wiederholen Sie den Vorgang.

## <a name="machine-unable-to-communicate"></a>Szenario: Computer kann nicht mit dem Dienst kommunizieren.

### <a name="issue"></a>Problem

Sie erhalten eine der folgenden Fehlermeldungen:

```error
Unable to Register Machine for Patch Management, Registration Failed with Exception System.Net.Http.HttpRequestException: An error occurred while sending the request. ---> System.Net.WebException: The underlying connection was closed: An unexpected error occurred on a receive. ---> System.ComponentModel.Win32Exception: The client and server can't communicate, because they do not possess a common algorithm
```

```error
Unable to Register Machine for Patch Management, Registration Failed with Exception Newtonsoft.Json.JsonReaderException: Error parsing positive infinity value.
```

```error
The certificate presented by the service <wsid>.oms.opinsights.azure.com was not issued by a certificate authority used for Microsoft services. Contact your network administrator to see if they are running a proxy that intercepts TLS/SSL communication.
```

### <a name="cause"></a>Ursache

Es gibt möglicherweise einen Proxy, ein Gateway oder eine Firewall, durch den bzw. die die Netzwerkkommunikation blockiert wird.

### <a name="resolution"></a>Lösung

Überprüfen Sie Ihr Netzwerk, und stellen Sie sicher, dass die entsprechenden Ports und Adressen zulässig sind. Unter [Netzwerkanforderungen](../automation-hybrid-runbook-worker.md#network-planning) finden Sie eine Liste der Ports und Adressen, die von Updateverwaltung und Hybrid Runbook Workers benötigt werden.

## <a name="unable-to-create-selfsigned-cert"></a>Szenario: Selbstsigniertes Zertifikat kann nicht erstellt werden.

### <a name="issue"></a>Problem

Sie erhalten eine der folgenden Fehlermeldungen:

```error
Unable to Register Machine for Patch Management, Registration Failed with Exception AgentService.HybridRegistration. PowerShell.Certificates.CertificateCreationException: Failed to create a self-signed certificate. ---> System.UnauthorizedAccessException: Access is denied.
```

### <a name="cause"></a>Ursache

Der Hybrid Runbook Worker konnte kein selbstsigniertes Zertifikat generieren.

### <a name="resolution"></a>Lösung

Überprüfen Sie, ob das Systemkonto über Lesezugriff auf den Ordner **C:\ProgramData\Microsoft\Crypto\RSA** verfügt, und versuchen Sie es erneut.

## <a name="mw-exceeded"></a>Szenario: Die geplante Updateverwaltung ist mit dem Fehler „MaintenanceWindowExceeded“ fehlgeschlagen.

### <a name="issue"></a>Problem

Das Standardwartungsfenster für Updates beträgt 120 Minuten. Sie können das Wartungsfenster auf maximal sechs (6) Stunden oder 360 Minuten erhöhen.

### <a name="resolution"></a>Lösung

Bearbeiten Sie alle fehlgeschlagenen, geplanten Updatebereitstellungen, und vergrößern Sie das Wartungsfenster.

Weitere Informationen zu Wartungsfenstern finden Sie unter [Installieren von Updates](../automation-tutorial-update-management.md#schedule-an-update-deployment).

## <a name="hresult"></a>Szenario: Computer wird als nicht bewertet mit einer HResult-Ausnahme angezeigt.

### <a name="issue"></a>Problem

* Für Ihren Computer wird **Nicht bewertet** unter **Konformität** und darunter wird eine Ausnahmemeldung angezeigt.
* Sie haben Computer, die als nicht bewertet angezeigt werden.
* Im Portal wird ein HRESULT-Fehlercode angezeigt.

### <a name="cause"></a>Ursache

Der Update-Agent (Windows Update-Agent unter Windows, der Paket-Manager für Ihre Linux-Distribution) ist nicht ordnungsgemäß konfiguriert. Für die Updateverwaltung wird der Update-Agent des Computers benötigt, um die erforderlichen Updates, den Status des Patches und die Ergebnisse der bereitgestellten Patches bereitzustellen. Ohne diese Informationen kann die Updateverwaltung keine richtigen Meldungen für die Patches zurückgeben, die benötigt bzw. installiert werden.

### <a name="resolution"></a>Lösung

Versuchen Sie, Updates lokal auf dem Computer auszuführen. Wenn dies fehlschlägt, bedeutet dies in der Regel einen Konfigurationsfehler beim Update-Agent.

Häufige Ursachen für Fehler sind:

* Netzwerkkonfiguration und Firewalls.
* Lesen Sie bei Linux die entsprechende Dokumentation, um sicherzustellen, dass Sie den Netzwerkendpunkt Ihres Paketrepositorys erreichen können.
* Überprüfen Sie bei Windows Ihre Agent-Konfiguration, wie unter [Updates werden nicht vom Intranetendpunkt heruntergeladen (WSUS/SCCM) ](/windows/deployment/update/windows-update-troubleshooting#updates-arent-downloading-from-the-intranet-endpoint-wsussccm) aufgeführt.
  * Wenn die Computer für Windows Update konfiguriert sind, stellen Sie sicher, dass Sie die in [](/windows/deployment/update/windows-update-troubleshooting#issues-related-to-httpproxy) aufgeführten Endpunkte erreichen können.
  * Wenn die Computer für WSUS konfiguriert sind, stellen Sie sicher, dass Sie den über den [WUServer-Registrierungsschlüssel](/windows/deployment/update/waas-wu-settings) konfigurierten WSUS-Server erreichen können.

Wird ein HRESULT angezeigt, doppelklicken Sie auf die rot angezeigte Ausnahme, um die vollständige Ausnahmemeldung anzuzeigen. Suchen Sie in der folgenden Tabelle nach möglichen Lösungen oder Aktionen:

|Ausnahme  |Lösung oder Aktion  |
|---------|---------|
|`Exception from HRESULT: 0x……C`     | Suchen Sie den entsprechenden Fehlercode in der [Windows Update-Fehlercodeliste](https://support.microsoft.com/help/938205/windows-update-error-code-list), um weitere Details zur Ursache der Ausnahme zu erfahren.        |
|`0x8024402C`</br>`0x8024401C`</br>`0x8024402F`      | Diese Fehler deuten auf Netzwerkkonnektivitätsprobleme hin. Stellen Sie sicher, dass Ihr Computer über die richtige Netzwerkkonnektivität für die Updateverwaltung verfügt. Im Abschnitt [Netzwerkplanung](../automation-update-management.md#ports) finden Sie eine Liste der erforderlichen Ports und Adressen.        |
|`0x8024001E`| Der Updatevorgang konnte nicht abgeschlossen werden, da der Dienst oder das System heruntergefahren wurden.|
|`0x8024002E`| Der Windows Update-Dienst ist deaktiviert.|
|`0x8024402C`     | Wenn Sie einen WSUS-Server verwenden, stellen Sie sicher, dass die Registrierungswerte für `WUServer` und `WUStatusServer` unter dem Registrierungsschlüssel `HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate` den richtigen WSUS-Server aufweisen.        |
|`0x80072EE2`|Netzwerkkonnektivitätsproblem oder Problem bei der Kommunikation mit einem konfigurierten WSUS-Server. Überprüfen Sie die WSUS-Einstellungen, und stellen Sie sicher, dass der Client darauf zugreifen kann.|
|`The service cannot be started, either because it is disabled or because it has no enabled devices associated with it. (Exception from HRESULT: 0x80070422)`     | Stellen Sie sicher, dass der Windows Update-Dienst (wuauserv) ausgeführt wird und nicht deaktiviert ist.        |
|Weitere generische Ausnahmen     | Suchen Sie im Internet nach möglichen Lösungen, und arbeiten Sie mit Ihrem lokalen IT-Support zusammen.         |

Die Überprüfung von `windowsupdate.log` kann Ihnen dabei helfen, auch die mögliche Ursache zu ermitteln. Weitere Informationen zum Lesen des Protokolls finden Sie unter [Lesen der Datei „Windowsupdate.log“](https://support.microsoft.com/en-ca/help/902093/how-to-read-the-windowsupdate-log-file).

Darüber hinaus können Sie die [Windows Update-Problembehandlung](https://support.microsoft.com/help/4027322/windows-update-troubleshooter) herunterladen und ausführen, um zu überprüfen, ob auf dem Computer Probleme mit Windows Update bestehen.

> [!NOTE]
> Die [Windows Update-Problembehandlung](https://support.microsoft.com/help/4027322/windows-update-troubleshooter) ist für Windows-Clients vorgesehen, sie funktioniert aber auch unter Windows Server.

## <a name="scenario-update-run-returns-status-failed"></a>Szenario: Die Updateausführung gibt den Status „Fehler“ zurück.

### <a name="issue"></a>Problem

Die Ausführung eines Updates wird gestartet, wobei jedoch Fehler auftreten.

### <a name="cause"></a>Ursache

Mögliche Ursachen:

* Der Paket-Manager ist fehlerhaft.
* Der Update-Agent (WUA für Windows, distributionsspezifischer Paket-Manager für Linux) ist falsch konfiguriert.
* Bestimmte Pakete können Probleme mit cloudbasiertem Patching haben.
* Computer nicht erreichbar.
* Updates enthielten Abhängigkeiten, die nicht aufgelöst wurden.

### <a name="resolution"></a>Lösung

Wenn während der Ausführung eines Updates nach dem erfolgreichen Start Fehler auftreten, [überprüfen Sie die Auftragsausgabe](../manage-update-multi.md#view-results-of-an-update-deployment) des von der Ausführung betroffenen Computers. Möglicherweise finden Sie spezifische Fehlermeldungen von Ihrem Computer, die Sie untersuchen können, um dann Maßnahmen zu ergreifen. Updateverwaltung setzt voraus, dass der Paket-Manager für eine erfolgreiche Bereitstellung von Updates fehlerfrei ist.

Wenn bestimmte Patches, Pakete oder Updates unmittelbar vor dem Fehlschlagen des Auftrags angezeigt werden, können Sie versuchen, diese aus der nächsten Updatebereitstellung [auszuschließen](../automation-tutorial-update-management.md#schedule-an-update-deployment). Informationen zum Sammeln von Protokollinformationen aus Windows Update finden Sie unter [Windows Update-Protokolldateien](/windows/deployment/update/windows-update-logs).

Wenn Sie ein Patchproblem nicht beheben können, erstellen Sie eine Kopie der folgenden Protokolldatei, und bewahren Sie sie zu Problembehandungszwecken auf, **bevor** die nächste Bereitstellung von Updates beginnt:

```bash
/var/opt/microsoft/omsagent/run/automationworker/omsupdatemgmt.log
```

## <a name="patches-are-not-installed"></a>Patches sind nicht installiert.

### <a name="machines-do-not-install-updates"></a>Computer installieren keine Updates.

* Versuchen Sie, Updates direkt auf dem Computer auszuführen. Kann der Computer nicht aktualisiert werden, sehen Sie sich die [Liste mit potenziellen Fehlern im Handbuch zur Problembehandlung](https://docs.microsoft.com/azure/automation/troubleshoot/update-management#hresult) an.
* Wenn Updates lokal ausgeführt werden, versuchen Sie, den Agent auf dem Computer zu entfernen und neu zu installieren, bevor Sie die Anweisungen unter [Entfernen eines virtuellen Computers für die Updateverwaltung](https://docs.microsoft.com/azure/automation/automation-update-management#remove-a-vm-from-update-management) befolgen.

### <a name="i-know-updates-are-available-but-they-dont-show-as-needed-on-my-machines"></a>Ich weiß, dass Updates verfügbar sind, diese werden auf meinen Computern aber nicht als erforderlich angezeigt.

* Dies geschieht häufig, wenn Computer so konfiguriert sind, dass sie Updates von WSUS/SCCM erhalten, aber WSUS/SCCM die Updates nicht genehmigt haben.
* Sie können überprüfen, ob Computer für WSUS/SCCM konfiguriert sind. [Vergleichen Sie dazu den Registrierungsschlüssel „UseWUServer“ mit den Registrierungsschlüsseln im Abschnitt „Konfigurieren der Funktion „Automatische Updates“ in diesem Dokument.](https://support.microsoft.com/help/328010/how-to-configure-automatic-updates-by-using-group-policy-or-registry-s)
* Wenn Updates in WSUS nicht genehmigt sind, werden Sie nicht installiert. Sie können mit der folgenden Abfrage in Log Analytics nach nicht genehmigten Updates suchen.

  ```loganalytics
  Update | where UpdateState == "Needed" and ApprovalSource == "WSUS" and Approved == "False" | summarize max(TimeGenerated) by Computer, KBID, Title
  ```

### <a name="updates-show-as-installed-but-i-cant-find-them-on-my-machine"></a>Updates werden als installiert angezeigt, ich kann sie aber nicht auf meinem Computer finden.

* Updates werden oft durch andere Updates ersetzt. Weitere Informationen finden Sie unter [„Das Update ist nicht auf Ihrem Computer anwendbar“ im Dokument „Windows Update-Problembehandlung“](https://docs.microsoft.com/windows/deployment/update/windows-update-troubleshooting#the-update-is-not-applicable-to-your-computer).

### <a name="installing-updates-by-classification-on-linux"></a>Installieren von Updates durch Klassifizierung unter Linux

* Die Bereitstellung von Updates für Linux durch Klassifizierung („Kritische und Sicherheitsupdates“) hat wichtige Einschränkungen, insbesondere für CentOS. Diese [Einschränkungen sind auf der Übersichtsseite für die Updateverwaltung dokumentiert](https://docs.microsoft.com/azure/automation/automation-update-management#linux-2).

### <a name="kb2267602-is-consistently--missing"></a>KB2267602 fehlt konsistent.

* KB2267602 ist das [Windows Defender-Definitionsupdate](https://www.microsoft.com/wdsi/definitions). Es wird täglich aktualisiert.

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Erhalten Sie Antworten von Azure-Experten über [Azure-Foren](https://azure.microsoft.com/support/forums/).
* Mit [@AzureSupport](https://twitter.com/azuresupport) verbinden – das offizielle Microsoft Azure-Konto zur Verbesserung der Benutzerfreundlichkeit durch Verbinden der Azure-Community mit den richtigen Ressourcen: Antworten, Support und Experten.
* Wenn Sie weitere Hilfe benötigen, können Sie einen Azure-Supportvorgang anlegen. Rufen Sie die [Azure-Support-Website](https://azure.microsoft.com/support/options/) auf, und wählen Sie **Support erhalten**aus.
