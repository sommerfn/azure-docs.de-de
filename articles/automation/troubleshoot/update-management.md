---
title: Beheben von Fehlern mit Updateverwaltung
description: Erfahren Sie, wie Sie Fehler mit der Updateverwaltung beheben können.
services: automation
author: bobbytreed
ms.author: robreed
ms.date: 05/31/2019
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: 2aebcf05cbc818997943ed3bab19fb1fd8a83592
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72786059"
---
# <a name="troubleshooting-issues-with-update-management"></a>Behandeln von Problemen mit Updateverwaltung

In diesem Artikel werden Lösungen für Probleme beschrieben, die bei Verwendung der Updateverwaltung auftreten können.

Es gibt eine Agent-Problembehandlung für den Hybrid Worker-Agent, mit dem das zugrunde liegende Problem bestimmt werden kann. Informationen zur Problembehandlung finden Sie unter [Beheben von Problemen mit dem Update-Agent](update-agent-issues.md). Verwenden Sie für alle anderen Probleme die folgende Anleitung zur Problembehandlung.

Sollten Sie Probleme beim Integrieren der Lösung in einen virtuellen Computer haben, suchen Sie auf dem lokalen Computer im Protokoll **Operations Manager** unter **Anwendungs- und Dienstprotokolle** nach Ereignissen mit der Ereignis-ID 4502 und Ereignisdetails, die **Microsoft.EnterpriseManagement.HealthService.AzureAutomation.HybridAgent** enthalten.

Der folgende Abschnitt enthält spezifische Fehlermeldungen und passende Lösungsvorschläge. Informationen zu anderen Onboardingproblemen finden Sie unter [Problembehandlung bei der Integration von Lösungen](onboarding.md).

## <a name="nologs"></a>Szenario: Im Portal werden unter „Updateverwaltung“ keine Computer angezeigt

### <a name="issue"></a>Problem

Es treten möglicherweise die folgenden Probleme auf:

* In der Ansicht „Updateverwaltung“ einer VM wird Ihr Computer als **Nicht konfiguriert** angezeigt.

* Ihre Computer fehlen in der Ansicht „Updateverwaltung“ Ihres Azure Automation-Kontos.

* Es sind Computer vorhanden, die unter **Konformität** als **Nicht bewertet** gezeigt werden. In Azure Monitor-Protokollen werden Heartbeatdaten für den Hybrid Runbook Worker, nicht jedoch für die Updateverwaltung angezeigt.

### <a name="cause"></a>Ursache

Dieses Problem kann durch lokale Konfigurationsprobleme oder eine falsch konfigurierte Bereichskonfiguration verursacht werden.

Möglicherweise müssen Sie den Hybrid Runbook Worker erneut registrieren und installieren.

Möglicherweise wurde ein in Ihrem Arbeitsbereich definiertes Kontingent erreicht, sodass eine weitere Datenspeicherung verhindert wird.

### <a name="resolution"></a>Lösung

* Führen Sie je nach Betriebssystem die Problembehandlung für [Windows](update-agent-issues.md#troubleshoot-offline) oder [Linux](update-agent-issues-linux.md#troubleshoot-offline) aus.

* Vergewissern Sie sich, dass Ihr Computer Meldungen an den richtigen Arbeitsbereich zurückgibt. Anweisungen zum Überprüfen dieses Punktes finden Sie unter [Überprüfen der Agentkonnektivität mit Log Analytics](../../azure-monitor/platform/agent-windows.md#verify-agent-connectivity-to-log-analytics). Stellen Sie außerdem sicher, dass dieser Arbeitsbereich mit Ihrem Azure Automation-Konto verknüpft ist. Rufen Sie dafür Ihr Automation-Konto auf, und wählen Sie unter **Zugehörige Ressourcen** die Option **Verknüpfter Arbeitsbereich** aus.

* Stellen Sie sicher, dass die Computer in Ihrem Log Analytics-Arbeitsbereich angezeigt werden. Führen Sie im Log Analytics-Arbeitsbereich, der mit Ihrem Automation-Konto verknüpft ist, die folgende Abfrage aus.

  ```loganalytics
  Heartbeat
  | summarize by Computer, Solutions
  ```
  Wenn Ihr Computer nicht in den Suchergebnissen angezeigt wird, wurde er in letzter Zeit nicht eingecheckt. Das bedeutet, dass wahrscheinlich ein lokales Konfigurationsproblem vorliegt und Sie den [Agent neu installieren müssen](../../azure-monitor/learn/quick-collect-windows-computer.md#install-the-agent-for-windows). Wenn Ihr Computer in den Suchergebnissen angezeigt wird, müssen Sie die Bereichskonfiguration überprüfen (siehe nächster Listenpunkt).

* Suchen Sie nach Bereichskonfigurationsproblemen. Die [Bereichskonfiguration](../automation-onboard-solutions-from-automation-account.md#scope-configuration) bestimmt, welche Computer für die Lösung konfiguriert werden. Wenn Ihr Computer zwar in Ihrem Arbeitsbereich, aber nicht im **Updateverwaltungsportal** angezeigt wird, müssen Sie die Bereichskonfiguration so konfigurieren, dass sie auf die Computer ausgerichtet ist. Informationen hierzu finden Sie unter [Integrieren von Computern in den Arbeitsbereich](../automation-onboard-solutions-from-automation-account.md#onboard-machines-in-the-workspace).

* Führen Sie in Ihrem Arbeitsbereich die folgende Abfrage aus:

  ```loganalytics
  Operation
  | where OperationCategory == 'Data Collection Status'
  | sort by TimeGenerated desc
  ```
  Wenn das Ergebnis `Data collection stopped due to daily limit of free data reached. Ingestion status = OverQuota` angezeigt wird, wurde ein für Ihren Arbeitsbereich definiertes Kontingent erreicht, wodurch das Speichern von Daten verhindert wird. Wechseln Sie in Ihrem Arbeitsbereich zu **Nutzung und geschätzte Kosten** > **Datenvolumenverwaltung**. Überprüfen Sie dort das Kontingent, oder heben Sie es auf.

* Wenn das Problem dadurch nicht gelöst wird, führen Sie die Schritte unter [Bereitstellen eines Windows Hybrid Runbook Workers](../automation-windows-hrw-install.md) aus, um den Hybrid Worker für Windows neu zu installieren. Informationen zu Linux finden Sie unter [Bereitstellen eines Linux Hybrid Runbook Workers](../automation-linux-hrw-install.md).

## <a name="rp-register"></a>Szenario: Der Automation-Ressourcenanbieter für Abonnements kann nicht registriert werden.

### <a name="issue"></a>Problem

Beim Arbeiten mit Lösungen in Ihrem Automation-Konto können die folgenden Fehler auftreten:

```error
Error details: Unable to register Automation Resource Provider for subscriptions:
```

### <a name="cause"></a>Ursache

Der Automation-Ressourcenanbieter ist nicht im Abonnement registriert.

### <a name="resolution"></a>Lösung

Führen Sie im Azure-Portal die folgenden Schritte aus, um den Automation-Ressourcenanbieter zu registrieren:

1. Wählen Sie unten im Portal in der Azure-Dienstliste die Option **Alle Dienste** und anschließend in der Dienstgruppe „Allgemein“ die Option **Abonnements** aus.
2. Wählen Sie Ihr Abonnement aus.
3. Wählen Sie unter **Einstellungen** die Option **Ressourcenanbieter** aus.
4. Überprüfen Sie in der Liste der Ressourcenanbieter, ob der Ressourcenanbieter **Microsoft.Automation** registriert ist.
5. Wenn dieser nicht aufgeführt ist, registrieren Sie den Anbieter **Microsoft.Automation** mithilfe der Schritte unter [Beheben von Fehlern bei der Ressourcenanbieterregistrierung](/azure/azure-resource-manager/resource-manager-register-provider-errors).

## <a name="components-enabled-not-working"></a>Szenario: Die Komponenten für die Lösung „Updateverwaltung“ wurden aktiviert, und diese VM wird nun konfiguriert.

### <a name="issue"></a>Problem

Die folgende Meldung wird 15 Minuten nach dem Onboarding auf einer VM angezeigt:

```error
The components for the 'Update Management' solution have been enabled, and now this virtual machine is being configured. Please be patient, as this can sometimes take up to 15 minutes.
```

### <a name="cause"></a>Ursache

Dieser Fehler kann aus den folgenden Gründe auftreten:

- Die Kommunikation mit dem Automation-Konto wird blockiert.
- Die zu integrierende VM stammt möglicherweise von einem geklonten Computer, der nicht über Sysprep mit MMA-Installation (Microsoft Monitoring Agent) vorbereitet wurde.

### <a name="resolution"></a>Lösung

1. Rufen Sie den Abschnitt [Netzwerkplanung](../automation-hybrid-runbook-worker.md#network-planning) auf, um zu ermitteln, welche Adressen und Ports zugelassen werden müssen, damit die Updateverwaltung funktioniert.
2. Wenn Sie ein geklontes Image verwenden:
   1. Entfernen Sie in Ihrem Log Analytics-Arbeitsbereich den virtuellen Computer aus der gespeicherten Suche für die Bereichskonfiguration `MicrosoftDefaultScopeConfig-Updates`, sofern er angezeigt wird. Gespeicherte Suchen finden Sie unter **Allgemein** in Ihrem Arbeitsbereich.
   2. Führen Sie `Remove-Item -Path "HKLM:\software\microsoft\hybridrunbookworker" -Recurse -Force`aus.
   3. Führen Sie `Restart-Service HealthService` aus, um `HealthService` neu zu starten. Dadurch wird der Schlüssel neu erstellt und eine neue UUID generiert.
   4. Wenn dieser Ansatz nicht funktioniert, führen Sie zuerst Sysprep auf dem Image aus, und installieren Sie dann den MMA.

## <a name="multi-tenant"></a>Szenario: Sie erhalten einen Fehler aufgrund eines verknüpften Abonnements, wenn Sie eine Updatebereitstellung für Computer in einem anderen Azure-Mandanten erstellen.

### <a name="issue"></a>Problem

Sie erhalten beim Versuch, eine Updatebereitstellung für Computer in einem anderen Azure-Mandanten zu erstellen, die folgende Fehlermeldung:

```error
The client has permission to perform action 'Microsoft.Compute/virtualMachines/write' on scope '/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resourceGroupName/providers/Microsoft.Automation/automationAccounts/automationAccountName/softwareUpdateConfigurations/updateDeploymentName', however the current tenant '00000000-0000-0000-0000-000000000000' is not authorized to access linked subscription '00000000-0000-0000-0000-000000000000'.
```

### <a name="cause"></a>Ursache

Dieser Fehler tritt bei der Erstellung einer Updatebereitstellung auf, bei der Azure-VMs eines anderen Mandanten in eine Updatebereitstellung einbezogen werden.

### <a name="resolution"></a>Lösung

Verwenden Sie die folgende Problemumgehung, um die Planung durchzuführen. Sie können das Cmdlet [New-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/new-azurermautomationschedule) mit dem Schalter `-ForUpdate` verwenden, um einen Zeitplan zu erstellen. Verwenden Sie dann das Cmdlet [New-AzureRmAutomationSoftwareUpdateConfiguration](/powershell/module/azurerm.automation/new-azurermautomationsoftwareupdateconfiguration
), und übergeben Sie die Computer im anderen Mandanten an den Parameter `-NonAzureComputer`. Dies wird anhand des folgenden Beispiels veranschaulicht:

```azurepowershell-interactive
$nonAzurecomputers = @("server-01", "server-02")

$startTime = ([DateTime]::Now).AddMinutes(10)

$s = New-AzureRmAutomationSchedule -ResourceGroupName mygroup -AutomationAccountName myaccount -Name myupdateconfig -Description test-OneTime -OneTime -StartTime $startTime -ForUpdate

New-AzureRmAutomationSoftwareUpdateConfiguration  -ResourceGroupName $rg -AutomationAccountName $aa -Schedule $s -Windows -AzureVMResourceId $azureVMIdsW -NonAzureComputer $nonAzurecomputers -Duration (New-TimeSpan -Hours 2) -IncludedUpdateClassification Security,UpdateRollup -ExcludedKbNumber KB01,KB02 -IncludedKbNumber KB100
```

## <a name="node-reboots"></a>Szenario: Unerklärliche Neustarts

### <a name="issue"></a>Problem

Auch wenn Sie die Option **Steuerelement neu starten** auf **Nie neu starten** festgelegt haben, werden die Computer nach der Installation von Updates immer noch neu gestartet.

### <a name="cause"></a>Ursache

Windows Update lässt sich über mehrere Registrierungsschlüssel modifizieren, mit denen das Neustartverhalten geändert werden kann.

### <a name="resolution"></a>Lösung

Überprüfen Sie die unter [Konfigurieren automatischer Updates durch Bearbeiten der Registrierung](/windows/deployment/update/waas-wu-settings#configuring-automatic-updates-by-editing-the-registry) und [Zum Verwalten des Neustarts verwendete Registrierungsschlüssel](/windows/deployment/update/waas-restart#registry-keys-used-to-manage-restart) aufgeführten Registrierungsschlüssel, um sicherzustellen, dass Ihre Computer ordnungsgemäß konfiguriert sind.

## <a name="failed-to-start"></a>Szenario: Für einen Computer in einer Updatebereitstellung wird die Fehlermeldung „Fehler beim Starten“ angezeigt.

### <a name="issue"></a>Problem

Ein Computer zeigt den Status **Fehler beim Starten** an. Wenn Sie sich die Detailinformationen für den Computer ansehen, wird der folgende Fehler angezeigt:

```error
Failed to start the runbook. Check the parameters passed. RunbookName Patch-MicrosoftOMSComputer. Exception You have requested to create a runbook job on a hybrid worker group that does not exist.
```

### <a name="cause"></a>Ursache

Dieser Fehler kann aus einem der folgenden Gründe auftreten:

* Der Computer ist nicht mehr vorhanden.
* Der Computer ist ausgeschaltet und nicht erreichbar.
* Für den Computer besteht ein Netzwerkverbindungsproblem. Daher ist der Hybrid Worker auf dem Computer nicht erreichbar.
* Ein Update des MMA hat zu einer Änderung der SourceComputerId geführt.
* Die Ausführung des Updates wurde möglicherweise gedrosselt, wenn das Limit von 2.000 gleichzeitigen Aufträgen in einem Automation-Konto erreicht wurde. Jede Bereitstellung kann als Auftrag betrachtet werden, und jeder Computer in einer Updatebereitstellung zählt als Auftrag. Jeder andere Automatisierungsauftrag oder jede Updatebereitstellung, der bzw. die in Ihrem Automation-Konto ausgeführt wird, wird auf das Limit für gleichzeitige Aufträge angerechnet.

### <a name="resolution"></a>Lösung

Verwenden Sie [dynamische Gruppen](../automation-update-management-groups.md) (falls vorhanden) für Ihre Updatebereitstellungen. Außerdem zu beachten:

* Stellen Sie sicher, dass der Computer noch vorhanden und erreichbar ist. Wenn er nicht mehr vorhanden ist, bearbeiten Sie Ihre Bereitstellung, und entfernen Sie den Computer.
* Im Abschnitt [Netzwerkplanung](../automation-update-management.md#ports) finden Sie eine Liste der Ports und Adressen, die für die Updateverwaltung erforderlich sind. Stellen Sie sicher, dass Ihr Computer diese Anforderungen erfüllt.
* Führen Sie die folgende Abfrage in Log Analytics aus, um nach Computern in Ihrer Umgebung zu suchen, bei denen sich die `SourceComputerId` geändert hat. Suchen Sie nach Computern, die denselben `Computer`-Wert, aber einen anderen `SourceComputerId`-Wert aufweisen. 

   ```loganalytics
   Heartbeat | where TimeGenerated > ago(30d) | distinct SourceComputerId, Computer, ComputerIP
   ```
   Nachdem Sie die betroffenen Computer gefunden haben, bearbeiten Sie die Updatebereitstellungen für diese Computer. Anschließend müssen Sie die Computer entfernen und dann erneut hinzufügen, damit die `SourceComputerId` den richtigen Wert aufweist.

## <a name="updates-nodeployment"></a>Szenario: Updates werden ohne Bereitstellung installiert.

### <a name="issue"></a>Problem

Wenn Sie einen Windows-Computer in der Updateverwaltung registrieren, sehen Sie Updates, die ohne Bereitstellung installiert werden.

### <a name="cause"></a>Ursache

Unter Windows werden Updates automatisch installiert, sobald sie verfügbar sind. Dieses Verhalten kann verwirrend sein, wenn Sie ein Update nicht für die Bereitstellung auf dem Computer eingeplant haben.

### <a name="resolution"></a>Lösung

Der `HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU`-Registrierungsschlüssel hat die Standardeinstellung „4“: **Automatisches Herunterladen und Installieren**.

Für Clients für die Updateverwaltung empfehlen wir Ihnen, diesen Schlüssel auf „3“ festzulegen: **Automatisches Herunterladen, keine automatische Installation**.

Weitere Informationen finden Sie unter [Automatische Updates konfigurieren](https://docs.microsoft.com/windows/deployment/update/waas-wu-settings#configure-automatic-updates).

## <a name="machine-already-registered"></a>Szenario: Computer ist bereits bei einem anderen Konto registriert.

### <a name="issue"></a>Problem

Sie erhalten die folgende Fehlermeldung:

```error
Unable to Register Machine for Patch Management, Registration Failed with Exception System.InvalidOperationException: {"Message":"Machine is already registered to a different account."}
```

### <a name="cause"></a>Ursache

Der Computer wurde bereits in einen anderen Arbeitsbereich für die Updateverwaltung integriert.

### <a name="resolution"></a>Lösung

1. Befolgen Sie die Schritte unter [Im Portal werden unter „Updateverwaltung“ keine Computer angezeigt](#nologs), um sicherzustellen, dass der Computer Meldungen an den richtigen Arbeitsbereich zurückgibt.
2. Bereinigen Sie alte Artefakte auf dem Computer durch [Löschen der Hybrid Runbook-Gruppe](../automation-hybrid-runbook-worker.md#remove-a-hybrid-worker-group), und wiederholen Sie den Vorgang.

## <a name="machine-unable-to-communicate"></a>Szenario: Der Computer kann nicht mit dem Dienst kommunizieren.

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

Möglicherweise wird die Netzwerkkommunikation durch einen Proxy, ein Gateway oder eine Firewall blockiert.

### <a name="resolution"></a>Lösung

Überprüfen Sie Ihr Netzwerk, und stellen Sie sicher, dass die entsprechenden Ports und Adressen zugelassen sind. Unter [Netzwerkanforderungen](../automation-hybrid-runbook-worker.md#network-planning) finden Sie eine Liste der Ports und Adressen, die von Updateverwaltung und Hybrid Runbook Workers benötigt werden.

## <a name="unable-to-create-selfsigned-cert"></a>Szenario: Selbstsigniertes Zertifikat kann nicht erstellt werden.

### <a name="issue"></a>Problem

Sie erhalten eine der folgenden Fehlermeldungen:

```error
Unable to Register Machine for Patch Management, Registration Failed with Exception AgentService.HybridRegistration. PowerShell.Certificates.CertificateCreationException: Failed to create a self-signed certificate. ---> System.UnauthorizedAccessException: Access is denied.
```

### <a name="cause"></a>Ursache

Der Hybrid Runbook Worker konnte kein selbstsigniertes Zertifikat generieren.

### <a name="resolution"></a>Lösung

Überprüfen Sie, ob das Systemkonto über Lesezugriff auf den Ordner **C:\ProgramData\Microsoft\Crypto\RSA** verfügt, und versuchen Sie es noch mal.

## <a name="mw-exceeded"></a>Szenario: Beim geplanten Update ist ein MaintenanceWindowExceeded-Fehler aufgetreten.

### <a name="issue"></a>Problem

Das Standardwartungsfenster für Updates beträgt 120 Minuten. Sie können das Wartungsfenster auf maximal sechs Stunden oder 360 Minuten erhöhen.

### <a name="resolution"></a>Lösung

Bearbeiten Sie alle fehlgeschlagenen, geplanten Updatebereitstellungen, und vergrößern Sie das Wartungsfenster.

Weitere Informationen zu Wartungsfenstern finden Sie unter [Installieren von Updates](../automation-tutorial-update-management.md#schedule-an-update-deployment).

## <a name="hresult"></a>Szenario: Computer wird als „Nicht bewertet“ mit einer HResult-Ausnahme angezeigt.

### <a name="issue"></a>Problem

* Für Ihren Computer wird **Nicht bewertet** unter **Konformität** und darunter wird eine Ausnahmemeldung angezeigt.
* Es sind Computer vorhanden, die als nicht bewertet angezeigt werden.
* Im Portal wird ein HRESULT-Fehlercode angezeigt.

### <a name="cause"></a>Ursache

Der Update-Agent (Windows Update-Agent unter Windows, der Paket-Manager für eine Linux-Distribution) ist nicht ordnungsgemäß konfiguriert. Für die Updateverwaltung wird der Update-Agent des Computers benötigt, um die erforderlichen Updates, den Status des Patches und die Ergebnisse der bereitgestellten Patches bereitzustellen. Ohne diese Informationen kann die Updateverwaltung keine richtigen Meldungen für die Patches zurückgeben, die benötigt bzw. installiert werden.

### <a name="resolution"></a>Lösung

Versuchen Sie, Updates lokal auf dem Computer auszuführen. Wenn dabei ein Fehler auftritt, liegt in der Regel ein Konfigurationsfehler beim Update-Agent vor.

Dieses Problem wird häufig durch Probleme mit der Netzwerkkonfiguration und der Firewall verursacht. Probieren Sie Folgendes aus:

* Lesen Sie für Linux die entsprechende Dokumentation, um sicherzustellen, dass Sie den Netzwerkendpunkt Ihres Paketrepositorys erreichen können.
* Überprüfen Sie bei Windows Ihre Agent-Konfiguration, wie unter [Updates werden nicht vom Intranetendpunkt heruntergeladen (WSUS/SCCM) ](/windows/deployment/update/windows-update-troubleshooting#updates-arent-downloading-from-the-intranet-endpoint-wsussccm) aufgeführt.
  * Wenn die Computer für Windows Update konfiguriert sind, stellen Sie sicher, dass Sie die in [Probleme im Zusammenhang mit HTTP/Proxy](/windows/deployment/update/windows-update-troubleshooting#issues-related-to-httpproxy) beschriebenen Endpunkte erreichen können.
  * Wenn die Computer für Windows Server Update Services (WSUS) konfiguriert sind, stellen Sie sicher, dass Sie den über den [WUServer-Registrierungsschlüssel](/windows/deployment/update/waas-wu-settings) konfigurierten WSUS-Server erreichen können.

Wird ein HRESULT angezeigt, doppelklicken Sie auf die rot angezeigte Ausnahme, um die vollständige Ausnahmemeldung anzuzeigen. Suchen Sie in der folgenden Tabelle nach möglichen Lösungen oder empfohlenen Aktionen:

|Ausnahme  |Lösung oder Aktion  |
|---------|---------|
|`Exception from HRESULT: 0x……C`     | Suchen Sie den entsprechenden Fehlercode in der [Windows Update-Fehlercodeliste](https://support.microsoft.com/help/938205/windows-update-error-code-list), um weitere Details zur Ursache der Ausnahme zu erfahren.        |
|`0x8024402C`</br>`0x8024401C`</br>`0x8024402F`      | Diese Fehler deuten auf Netzwerkkonnektivitätsprobleme hin. Stellen Sie sicher, dass Ihr Computer über Netzwerkkonnektivität zur Updateverwaltung verfügt. Im Abschnitt [Netzwerkplanung](../automation-update-management.md#ports) finden Sie eine Liste der erforderlichen Ports und Adressen.        |
|`0x8024001E`| Der Updatevorgang konnte nicht abgeschlossen werden, weil der Dienst oder das System heruntergefahren wurden.|
|`0x8024002E`| Der Windows Update-Dienst ist deaktiviert.|
|`0x8024402C`     | Wenn Sie einen WSUS-Server verwenden, stellen Sie sicher, dass die Registrierungswerte für `WUServer` und `WUStatusServer` unter dem Registrierungsschlüssel `HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate` den richtigen WSUS-Server angeben.        |
|`0x80072EE2`|Es liegt ein Netzwerkkonnektivitätsproblem oder ein Problem bei der Kommunikation mit einem konfigurierten WSUS-Server vor. Überprüfen Sie die WSUS-Einstellungen, und stellen Sie sicher, dass der Client auf den Dienst zugreifen kann.|
|`The service cannot be started, either because it is disabled or because it has no enabled devices associated with it. (Exception from HRESULT: 0x80070422)`     | Stellen Sie sicher, dass der Windows Update-Dienst (wuauserv) ausgeführt wird und nicht deaktiviert ist.        |
|Weitere generische Ausnahmen     | Suchen Sie im Internet nach möglichen Lösungen, und arbeiten Sie mit Ihrem lokalen IT-Support zusammen.         |

In der Datei „Windowsupdate.log“ können Sie ebenfalls mögliche Ursachen finden. Weitere Informationen zum Lesen des Protokolls finden Sie unter [Lesen der Datei „Windowsupdate.log“](https://support.microsoft.com/en-ca/help/902093/how-to-read-the-windowsupdate-log-file).

Darüber hinaus können Sie die [Windows Update-Problembehandlung](https://support.microsoft.com/help/4027322/windows-update-troubleshooter) herunterladen und ausführen, um zu überprüfen, ob auf dem Computer Probleme mit Windows Update bestehen.

> [!NOTE]
> In der Dokumentation zur [Windows Update-Problembehandlung](https://support.microsoft.com/help/4027322/windows-update-troubleshooter) ist angegeben, dass sie für Windows-Clients verwendet wird. Sie funktioniert aber auch auf Windows Server.

## <a name="scenario-update-run-returns-failed-status-linux"></a>Szenario: Updateausführung gibt den Status „Fehler“ zurück (Linux).

### <a name="issue"></a>Problem

Die Ausführung eines Updates wird gestartet, wobei jedoch Fehler auftreten.

### <a name="cause"></a>Ursache

Mögliche Ursachen:

* Der Paket-Manager ist fehlerhaft.
* Der Update-Agent (WUA für Windows, distributionsspezifischer Paket-Manager für Linux) ist falsch konfiguriert.
* Bestimmte Pakete haben Probleme mit cloudbasiertem Patching.
* Der Computer ist nicht erreichbar.
* Updates enthielten Abhängigkeiten, die nicht aufgelöst wurden.

### <a name="resolution"></a>Lösung

Wenn während der Ausführung eines Updates nach dem erfolgreichen Start Fehler auftreten, [überprüfen Sie die Auftragsausgabe](../manage-update-multi.md#view-results-of-an-update-deployment) des von der Ausführung betroffenen Computers. Möglicherweise finden Sie spezifische Fehlermeldungen von Ihren Computern, die Sie untersuchen können, um dann Maßnahmen zu ergreifen. Updateverwaltung setzt voraus, dass der Paket-Manager für eine erfolgreiche Bereitstellung von Updates fehlerfrei ist.

Wenn bestimmte Patches, Pakete oder Updates unmittelbar vor dem Fehlschlagen des Auftrags angezeigt werden, können Sie versuchen, diese aus der nächsten Updatebereitstellung [auszuschließen](../automation-tutorial-update-management.md#schedule-an-update-deployment). Informationen zum Sammeln von Protokollinformationen aus Windows Update finden Sie unter [Windows Update-Protokolldateien](/windows/deployment/update/windows-update-logs).

Wenn Sie ein Patchproblem nicht beheben können, erstellen Sie eine Kopie der folgenden Protokolldatei, und bewahren Sie sie zu Problembehandungszwecken auf, *bevor* die nächste Bereitstellung von Updates beginnt:

```bash
/var/opt/microsoft/omsagent/run/automationworker/omsupdatemgmt.log
```

## <a name="patches-arent-installed"></a>Patches sind nicht installiert.

### <a name="machines-dont-install-updates"></a>Computer installieren keine Updates.

* Versuchen Sie, Updates direkt auf dem Computer auszuführen. Wenn der Computer die Updates nicht anwenden kann, sehen Sie sich die [Liste mit potenziellen Fehlern im Handbuch zur Problembehandlung](https://docs.microsoft.com/azure/automation/troubleshoot/update-management#hresult) an.
* Wenn Updates lokal ausgeführt werden, versuchen Sie, den Agent auf dem Computer zu entfernen und neu zu installieren, bevor Sie die Anweisungen unter [Entfernen einer VM für die Updateverwaltung](https://docs.microsoft.com/azure/automation/automation-onboard-solutions-from-browse#clean-up-resources) ausführen.

### <a name="i-know-updates-are-available-but-they-dont-show-as-available-on-my-machines"></a>Ich weiß, dass Updates verfügbar sind, diese werden auf meinen Computern aber nicht als verfügbar angezeigt.

* Dies geschieht häufig, wenn Computer so konfiguriert sind, dass sie Updates von WSUS oder System Center Configuration Manager (SCCM) erhalten, aber WSUS und SCCM die Updates nicht genehmigt haben.
* Sie können überprüfen, ob Computer für WSUS und SCCM konfiguriert sind. [Vergleichen Sie dazu den UseWUServer-Registrierungsschlüssel mit den Registrierungsschlüsseln im Abschnitt „Konfigurieren von automatischen Updates durch Bearbeiten der Registrierung“ in diesem Artikel](https://support.microsoft.com/help/328010/how-to-configure-automatic-updates-by-using-group-policy-or-registry-s).
* Wenn Updates in WSUS nicht genehmigt sind, werden sie nicht installiert. Sie können durch die Ausführung der folgenden Abfrage in Log Analytics nach nicht genehmigten Updates suchen:

  ```loganalytics
  Update | where UpdateState == "Needed" and ApprovalSource == "WSUS" and Approved == "False" | summarize max(TimeGenerated) by Computer, KBID, Title
  ```

### <a name="updates-show-as-installed-but-i-cant-find-them-on-my-machine"></a>Updates werden als installiert angezeigt, ich kann sie aber nicht auf meinem Computer finden.

* Updates werden oft durch andere Updates ersetzt. Weitere Informationen finden Sie unter [Update wird ersetzt](https://docs.microsoft.com/windows/deployment/update/windows-update-troubleshooting#the-update-is-not-applicable-to-your-computer) in der Anleitung für die Windows Update-Problembehandlung.

### <a name="installing-updates-by-classification-on-linux"></a>Installieren von Updates durch Klassifizierung unter Linux

* Die Bereitstellung von Updates für Linux durch Klassifizierung („Kritische und Sicherheitsupdates“) hat wichtige Einschränkungen, insbesondere für CentOS. Diese Einschränkungen sind auf der [Übersichtsseite für die Updateverwaltung](https://docs.microsoft.com/azure/automation/automation-update-management#linux-2) dokumentiert.

### <a name="kb2267602-is-consistently-missing"></a>KB2267602 fehlt dauerhaft.

* KB2267602 ist das [Windows Defender-Definitionsupdate](https://www.microsoft.com/wdsi/definitions). Es wird täglich aktualisiert.

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Erhalten Sie Antworten von Azure-Experten über [Azure-Foren](https://azure.microsoft.com/support/forums/).
* Nutzen Sie [@AzureSupport](https://twitter.com/azuresupport) – das offizielle Microsoft Azure-Konto zur Verbesserung der Benutzerfreundlichkeit. Hierüber hat die Azure-Community Zugriff auf die richtigen Ressourcen: Antworten, Support und Experten.
* Erstellen Sie einen Azure-Supportfall. Rufen Sie die [Azure-Support-Website](https://azure.microsoft.com/support/options/) auf, und wählen Sie **Support erhalten**aus.
