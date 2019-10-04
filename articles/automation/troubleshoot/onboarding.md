---
title: Beheben von Fehlern beim Integrieren von Lösungen für Updateverwaltung, Änderungsnachverfolgung und Bestand
description: Erfahren Sie, wie Sie Fehler beim Integrieren von Lösungen für Updateverwaltung, Änderungsnachverfolgung und Bestand beheben.
services: automation
author: bobbytreed
ms.author: robreed
ms.date: 05/22/2019
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: 8b4ee999bb23abdcea3411720bde244b2da4e89f
ms.sourcegitcommit: f5cc71cbb9969c681a991aa4a39f1120571a6c2e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2019
ms.locfileid: "68516395"
---
# <a name="troubleshoot-errors-when-onboarding-solutions"></a>Beheben von Fehlern beim Integrieren von Lösungen

Beim Integrieren von Lösungen wie Updateverwaltung oder Änderungsnachverfolgung können Fehler auftreten. In diesem Artikel wird beschrieben, welche Fehler auftreten und wie diese behoben werden können.

## <a name="known-issues"></a>Bekannte Probleme

### <a name="node-rename"></a>Szenario: Das Umbenennen eines registrierten Knotens erfordert das Aufheben der Registrierung und das erneute Registrieren.

#### <a name="issue"></a>Problem

Ein Knoten ist für Azure Automation registriert, und dann wird der Computername des im Betriebssystem geändert.  Berichte von diesem Knoten werden weiterhin mit dem ursprünglichen Namen angezeigt.

#### <a name="cause"></a>Ursache

Beim Umbenennen registrierter Knoten wird der Knotenname in Azure Automation nicht aktualisiert.

#### <a name="resolution"></a>Lösung

Heben Sie die Registrierung des Knotens in Azure Automation State Configuration auf, und registrieren Sie ihn erneut.  Berichte, die vor diesem Zeitpunkt im Dienst veröffentlicht wurden, sind anschließend nicht mehr verfügbar.


### <a name="resigning-cert"></a>Szenario: Das erneute Signieren von Zertifikaten über den HTTPS-Proxy wird nicht unterstützt.

#### <a name="issue"></a>Problem

Kunden haben gemeldet, dass beim Herstellen einer Verbindung über eine Proxylösung, die den HTTPS-Datenverkehr beendet und dann den Datenverkehr mit einem neuen Zertifikat erneut verschlüsselt, der Dienst die Verbindung nicht zulässt.

#### <a name="cause"></a>Ursache

Azure Automation unterstützt keine Neusignierung von Zertifikaten, die zum Verschlüsseln des Datenverkehrs verwendet werden.

#### <a name="resolution"></a>Lösung

Für dieses Szenario gibt es keine Problemumgehung.

## <a name="general-errors"></a>Allgemeine Fehler

### <a name="missing-write-permissions"></a>Szenario: Fehler beim Onboarding mit der Meldung, dass die Lösung nicht aktiviert werden kann

#### <a name="issue"></a>Problem

Eine der folgenden Meldungen wird angezeigt, wenn Sie versuchen, einen virtuellen Computer in eine Lösung zu integrieren (Onboarding):

```error
The solution cannot be enabled due to missing permissions for the virtual machine or deployments
```

```error
The solution cannot be enabled on this VM because the permission to read the workspace is missing
```

#### <a name="cause"></a>Ursache

Dieser Fehler wird durch falsche oder fehlende Berechtigungen auf dem virtuellen Computer, im Arbeitsbereich oder für den Benutzer verursacht.

#### <a name="resolution"></a>Lösung

Stellen Sie sicher, dass Sie die erforderlichen Berechtigungen zum Integrieren des virtuellen Computers haben. Überprüfen Sie noch einmal [die Berechtigungen, die für das Onboarding von Computern erforderlich sind](../automation-role-based-access-control.md#onboarding), und wiederholen Sie das Onboarding für die Lösung. Wenn Sie den Fehler `The solution cannot be enabled on this VM because the permission to read the workspace is missing` erhalten, stellen Sie sicher, dass Sie über die Berechtigung `Microsoft.OperationalInsights/workspaces/read` verfügen, um herausfinden zu können, ob die VM in einen Arbeitsbereich integriert ist.

### <a name="diagnostic-logging"></a>Szenario: Onboarding-Fehler mit der Meldung „Fehler beim Konfigurieren eines Automation-Kontos für die Diagnoseprotokollierung“

#### <a name="issue"></a>Problem

Die folgende Meldung wird angezeigt, wenn Sie versuchen, einen virtuellen Computer in eine Lösung zu integrieren (Onboarding):

```error
Failed to configure automation account for diagnostic logging
```

#### <a name="cause"></a>Ursache

Dieser Fehler kann auftreten, wenn der Tarif nicht mit dem Abrechnungsmodell des Abonnements übereinstimmt. Weitere Informationen finden Sie unter [Überwachen der Nutzung und geschätzten Kosten in Azure Monitor](https://aka.ms/PricingTierWarning).

#### <a name="resolution"></a>Lösung

Erstellen Sie Ihren Log Analytics-Arbeitsbereich manuell, und wiederholen Sie den Onboardingprozess, um den erstellten Arbeitsbereich auszuwählen.

### <a name="computer-group-query-format-error"></a>Szenario: ComputerGroupQueryFormatError

#### <a name="issue"></a>Problem

Dieser Fehlercode bedeutet, dass die Abfrage einer gespeicherten Suchcomputergruppe für die Ziellösung nicht ordnungsgemäß formatiert war. 

#### <a name="cause"></a>Ursache

Möglicherweise haben Sie die Abfrage geändert, oder sie wurde vom System geändert.

#### <a name="resolution"></a>Lösung

Sie können die Abfrage für diese Lösung löschen und die Lösung erneut anwenden, wodurch die Abfrage neu erstellt wird. Die Abfrage finden Sie in Ihrem Arbeitsbereich unter **Gespeicherte Suchvorgänge**. Der Name der Abfrage lautet **MicrosoftDefaultComputerGroup**, und die Kategorie der Abfrage ist der Name der mit dieser Abfrage verknüpften Lösung. Wenn mehrere Lösungen aktiviert sind, wird **MicrosoftDefaultComputerGroup** unter **Speicherte Suchvorgänge** mehrfach angezeigt.

### <a name="policy-violation"></a>Szenario: PolicyViolation

#### <a name="issue"></a>Problem

Dieser Fehlercode bedeutet, dass bei der Bereitstellung ein Fehler aufgrund einer oder mehrerer Richtlinienverletzungen aufgetreten ist.

#### <a name="cause"></a>Ursache 

Eine Richtlinie ist vorhanden, die den Abschluss des Vorgangs blockiert.

#### <a name="resolution"></a>Lösung

Für eine erfolgreiche Bereitstellung der Lösung müssen Sie eine Änderung der angegebenen Richtlinie in Betracht ziehen. Da viele verschiedene Richtlinientypen definiert werden können, hängen die erforderlichen Änderungen von der Richtlinie ab, gegen die verstoßen wurde. Beispiel: Wenn eine Richtlinie für eine Ressourcengruppe festgelegt wurde, die die Berechtigung zum Ändern der Inhalte bestimmter Ressourcentypen in dieser Ressourcengruppe verweigert hat, können Sie beispielsweise einen der folgenden Schritte ausführen:

* Entfernen Sie die Richtlinie vollständig.
* Versuchen Sie, sie für eine andere Ressourcengruppe festzulegen.
* Überarbeiten Sie die Richtlinie etwa wie folgt:
  * Weisen Sie die Richtlinie einer bestimmten Ressource neu zu (z.B. einem bestimmten Automation-Konto).
  * Überarbeiten Sie die Gruppe der Ressourcen, zu deren Ablehnung diese Richtlinie konfiguriert wurde.

Lesen Sie die Benachrichtigungen in der oberen rechten Ecke des Azure-Portals, oder navigieren Sie zum Anzeigen der fehlerhaften Bereitstellung zu der Ressourcengruppe, die Ihr Automation-Konto enthält, und wählen Sie unter **Einstellungen** die Option **Bereitstellungen** aus. Weitere Informationen zu Azure Policy finden Sie hier: [Übersicht zu Azure Policy](../../governance/policy/overview.md?toc=%2fazure%2fautomation%2ftoc.json).

### <a name="unlink"></a>Szenario: Fehler beim Versuch, die Verknüpfung eines Arbeitsbereichs aufzuheben

#### <a name="issue"></a>Problem

Beim Versuch, die Verknüpfung eines Arbeitsbereichs aufzuheben, erhalten Sie die folgende Fehlermeldung:

```error
The link cannot be updated or deleted because it is linked to Update Management and/or ChangeTracking Solutions.
```

#### <a name="cause"></a>Ursache

Dieser Fehler tritt auf, wenn Sie noch über aktive Lösungen in Ihrem Log Analytics-Arbeitsbereich verfügen, die davon abhängig sind, dass Ihr Automation-Konto und Ihr Log Analytics-Arbeitsbereich verknüpft sind.

### <a name="resolution"></a>Lösung

Zum Beheben dieses Problems müssen Sie die folgenden Lösungen aus Ihrem Arbeitsbereich entfernen, sofern Sie sie verwenden:

* Updateverwaltung
* Change Tracking
* Starten/Beenden von VMs außerhalb der Kernzeit

Nachdem Sie die Lösungen entfernt haben, können Sie die Verknüpfung zu Ihrem Arbeitsbereich aufheben. Es ist wichtig, alle vorhandenen Artefakte dieser Lösungen auch aus Ihrem Arbeitsbereich und Ihrem Automation-Konto zu entfernen.  

* Updateverwaltung
  * Entfernen von Updatebereitstellungen (Zeitplänen) aus Ihrem Automation-Konto
* Starten/Beenden von VMs außerhalb der Kernzeit
  * Entfernen Sie alle Sperren für die Komponenten der Lösung in Ihrem Automation-Konto unter **Einstellungen** > **Sperren**.
  * Zusätzliche Schritte zum Entfernen der Lösung für das Starten/Beenden von VMs außerhalb der Geschäftszeiten finden Sie im Artikel [Starten/Beenden von VMs außerhalb der Geschäftszeiten – Entfernen der Lösung](../automation-solution-vm-management.md##remove-the-solution).

## <a name="mma-extension-failures"></a>MMA-Erweiterungsfehler

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)] 

Bei der Bereitstellung einer Lösung wird eine Vielzahl zugehöriger Ressourcen bereitgestellt. Eine dieser Ressourcen ist die Microsoft Monitoring Agent-Erweiterung oder der Log Analytics-Agent für Linux. Dies sind VM-Erweiterungen, die vom Gast-Agent des virtuellen Computers installiert werden. Dieser ist zuständig für die Kommunikation mit dem konfigurierten Log Analytics-Arbeitsbereich zum Zweck der späteren Koordination des Herunterladens von Binär- und anderen Dateien, von denen die von Ihnen integrierte Lösung abhängt, sobald die Ausführung beginnt.
Auf Fehler bei der Installation von MMA oder des Log Analytics-Agents für Linux werden Sie zumeist über eine Benachrichtigung im Notifications Hub aufmerksam. Wenn Sie auf diese Benachrichtigung klicken, erhalten Sie weitere Informationen zum jeweiligen Fehler. Durch Navigieren zur Ressource „Ressourcengruppen“ und dann zum darin enthaltenen Element „Bereitstellungen“ werden auch Details zu den aufgetretenen Bereitstellungsfehlern angezeigt.
Die Installation von MMA oder des Log Analytics-Agents für Linux kann aus verschiedenen Gründen fehlschlagen, wobei die Schritte zur Behebung dieser Fehler je nach Problem unterschiedlich sind. Es folgen spezifische Schritte zur Problembehandlung.

Im folgenden Abschnitt werden verschiedene Probleme beschrieben, die bei der Integration auftreten können und die einen Fehler bei der Bereitstellung der MMA-Erweiterung verursachen.

### <a name="webclient-exception"></a>Szenario: Ausnahme während einer WebClient-Anforderung

Die MMA-Erweiterung auf dem virtuellen Computer kann nicht mit externen Ressourcen kommunizieren, und die Bereitstellung schlägt fehl.

#### <a name="issue"></a>Problem

Im Folgenden finden Sie Beispiele für Fehlermeldungen, die zurückgegeben werden:

```error
Please verify the VM has a running VM agent, and can establish outbound connections to Azure storage.
```

```error
'Manifest download error from https://<endpoint>/<endpointId>/Microsoft.EnterpriseCloud.Monitoring_MicrosoftMonitoringAgent_australiaeast_manifest.xml. Error: UnknownError. An exception occurred during a WebClient request.
```

#### <a name="cause"></a>Ursache

Mögliche Ursachen für diesen Fehler:

* In der VM ist ein Proxy konfiguriert, der nur bestimmte Ports zulässt.

* Eine Firewalleinstellung hat den Zugriff auf die benötigten Ports und Adressen blockiert.

#### <a name="resolution"></a>Lösung

Stellen Sie sicher, dass die ordnungsgemäßen Ports und Adressen für die Kommunikation geöffnet sind. Eine Liste der Ports und Adressen finden Sie unter [Planen Ihres Netzwerks](../automation-hybrid-runbook-worker.md#network-planning).

### <a name="transient-environment-issue"></a>Szenario: Fehler bei der Installation aufgrund vorübergehender Umgebungsprobleme

Die Installation der Microsoft Monitoring Agent-Erweiterung ist während der Bereitstellung aufgrund einer anderen Installation oder Aktion fehlgeschlagen, die die Installation blockiert.

#### <a name="issue"></a>Problem

Im Folgenden finden Sie Beispiele für Fehlermeldungen, die ggf. zurückgegeben werden:

```error
The Microsoft Monitoring Agent failed to install on this machine. Please try to uninstall and reinstall the extension. If the issue persists, please contact support.
```

```error
'Install failed for plugin (name: Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent, version 1.0.11081.4) with exception Command C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\1.0.11081.4\MMAExtensionInstall.exe of Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent has exited with Exit code: 1618'
```

```error
'Install failed for plugin (name: Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent, version 1.0.11081.2) with exception Command C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\1.0.11081.2\MMAExtensionInstall.exe of Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent has exited with Exit code: 1601'
```

#### <a name="cause"></a>Ursache

Mögliche Ursachen für diesen Fehler:

* Eine weitere Installation erfolgt gerade.
* Auf dem System wird während der Vorlagenbereitstellung ein Neustart ausgelöst.

#### <a name="resolution"></a>Lösung

Dieser Fehler tritt nur vorübergehend auf. Wiederholen Sie die Bereitstellung, um die Erweiterung zu installieren.

### <a name="installation-timeout"></a>Szenario: Zeitlimit für Installation

Die Installation der MMA-Erweiterung wurde aufgrund eines Zeitlimits nicht abgeschlossen.

#### <a name="issue"></a>Problem

Im folgenden finden Sie ein Beispiel einer Fehlermeldung, die zurückgegeben werden kann:

```error
Install failed for plugin (name: Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent, version 1.0.11081.4) with exception Command C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\1.0.11081.4\MMAExtensionInstall.exe of Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent has exited with Exit code: 15614
```

#### <a name="cause"></a>Ursache

Dieser Fehler tritt auf, weil der virtuelle Computer während der Installation unter großer Last stand.

### <a name="resolution"></a>Lösung

Versuchen Sie, die MMA-Erweiterung zu installieren, wenn die VM unter geringerer Last steht.

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Erhalten Sie Antworten von Azure-Experten über [Azure-Foren](https://azure.microsoft.com/support/forums/).
* Mit [@AzureSupport](https://twitter.com/azuresupport) verbinden – das offizielle Microsoft Azure-Konto zur Verbesserung der Benutzerfreundlichkeit durch Verbinden der Azure-Community mit den richtigen Ressourcen: Antworten, Support und Experten.
* Wenn Sie weitere Hilfe benötigen, können Sie einen Azure-Supportvorgang anlegen. Rufen Sie die [Azure-Support-Website](https://azure.microsoft.com/support/options/) auf, und wählen Sie **Support erhalten**aus.
