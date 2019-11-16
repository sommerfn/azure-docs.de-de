---
title: Behandeln von Problemen mit der Azure Automation-Konfiguration des gewünschten Zustands (Desired State Configuration, DSC)
description: Dieser Artikel enthält Informationen zur Behandlung von Problemen mit der Konfiguration des gewünschten Zustands (Desired State Configuration, DSC).
services: automation
ms.service: automation
ms.subservice: ''
author: bobbytreed
ms.author: robreed
ms.date: 04/16/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 7be5e814d8092b523fa69fdd84f0e1476736fda2
ms.sourcegitcommit: cf36df8406d94c7b7b78a3aabc8c0b163226e1bc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/09/2019
ms.locfileid: "73887712"
---
# <a name="troubleshoot-desired-state-configuration-dsc"></a>Behandeln von Problemen mit Konfiguration des gewünschten Zustands (Desired State Configuration, DSC)

Dieser Artikel enthält Informationen zur Behandlung von Problemen mit der Konfiguration des gewünschten Zustands (Desired State Configuration, DSC).

## <a name="steps-to-troubleshoot-desired-state-configuration-dsc"></a>Schritte zum Behandeln von Problemen mit der DSC-Konfiguration (Desired State Configuration)

Wenn es beim Kompilieren oder Bereitstellen von Konfigurationen in Azure State Configuration zu Fehlern kommt, finden Sie hier einige Schritte, um das Problem zu diagnostizieren.

1. **Stellen Sie sicher, dass die Konfiguration auf Ihrem lokalen Computer erfolgreich kompiliert wird:**  Azure State Configuration ist in PowerShell DSC integriert. Die Dokumentation zur DSC-Sprache und -Syntax finden Sie in der [PowerShell DSC-Dokumentation](https://docs.microsoft.com/powershell/scripting/overview).

   Durch das Kompilieren der DSC-Konfiguration auf Ihrem lokalen Computer können Sie unter anderem die folgenden allgemeinen Fehler ermitteln und beheben:

   - **Fehlende Module**
   - **Syntaxfehler**
   - **Logikfehler**

2. **Zeigen Sie die DSC-Protokolle auf Ihrem Knoten an:** Wenn Ihre Konfiguration erfolgreich kompiliert wird, aber nicht auf einen Knoten angewendet werden kann, finden Sie in den Protokollen ausführliche Informationen. Informationen zum Speicherort der DSC-Protokolle finden Sie unter [Wo befinden sich die DSC-Ereignisprotokolle?](/powershell/dsc/troubleshooting/troubleshooting#where-are-dsc-event-logs).

   Darüber hinaus können Sie mithilfe von [xDscDiagnostics](https://github.com/PowerShell/xDscDiagnostics) die Informationen in den DSC-Protokollen analysieren. Wenn Sie sich an den Support wenden, werden diese Protokolle zur Diagnose des Problems benötigt.

   Sie können **xDscDiagnostics** auf Ihrem lokalen Computer installieren. Folgen Sie hierzu den Anweisungen zur [Installation der stabilen Modulversion](https://github.com/PowerShell/xDscDiagnostics#install-the-stable-version-module).

   Um **xDscDiagnostics** auf Ihrem Azure-Computer zu installieren, können Sie [az vm run-command](/cli/azure/vm/run-command) oder [Invoke-AzVMRunCommand](/powershell/module/azurerm.compute/invoke-azurermvmruncommand) verwenden. Es ist auch möglich, die Option **Befehl ausführen** aus dem Portal zu verwenden. Folgen Sie hierzu den Schritten unter [Ausführen von PowerShell-Skripts in Ihrer Windows-VM mit „Befehl ausführen“](../../virtual-machines/windows/run-command.md).

   Informationen zur Verwendung von **xDscDiagnostics** finden Sie unter [Verwenden von „xDscDiagnostics“ zum Analysieren von DSC-Protokollen](/powershell/dsc/troubleshooting/troubleshooting#using-xdscdiagnostics-to-analyze-dsc-logs) sowie im Abschnitt zu den [xDscDiagnostics-Cmdlets](https://github.com/PowerShell/xDscDiagnostics#cmdlets).
3. **Stellen Sie sicher, dass Ihre Knoten und der Automation-Arbeitsbereich über die erforderlichen Module verfügen:** DSC hängt von den Modulen ab, die auf dem Knoten installiert sind.  Wenn Sie Azure Automation State Configuration verwenden, importieren Sie alle benötigten Module in Ihr Automation-Konto. Die hierzu erforderlichen Schritte finden Sie unter [Importieren von Modulen](../shared-resources/modules.md#import-modules). Konfigurationen können auch von bestimmten Modulversionen abhängen.  Weitere Informationen finden Sie unter [Beheben von Fehlern bei freigegebenen Ressourcen](shared-resources.md#modules).

## <a name="common-errors-when-working-with-desired-state-configuration-dsc"></a>Häufige Fehler beim Verwenden der Konfiguration des gewünschten Zustands (Desired State Configuration, DSC)

### <a name="unsupported-characters"></a>Szenario: Eine Konfiguration mit speziellen Zeichen kann nicht aus dem Portal gelöscht werden

#### <a name="issue"></a>Problem

Beim Versuch, eine DSC-Konfiguration aus dem Portal zu löschen, wird der folgende Fehler angezeigt:

```error
An error occurred while deleting the DSC configuration '<name>'.  Error-details: The argument configurationName with the value <name> is not valid.  Valid configuration names can contain only letters,  numbers, and underscores.  The name must start with a letter.  The length of the name must be between 1 and 64 characters.
```

#### <a name="cause"></a>Ursache

Der Fehler ist ein vorübergehendes Problem, das gelöst werden soll.

#### <a name="resolution"></a>Lösung

* Verwenden Sie das Azure-Cmdlet „Remove-AzAutomationDscConfiguration“, um die Konfiguration zu löschen.
* Die Dokumentation für dieses Cmdlet wurde noch nicht aktualisiert.  Nutzen Sie bis dahin bitte die Dokumentation für das AzureRM-Modul.
  * [Remove-AzureRmAutomationDSCConfiguration](/powershell/module/azurerm.automation/Remove-AzureRmAutomationDscConfiguration)

### <a name="failed-to-register-agent"></a>Szenario: Fehler beim Registrieren des DSC-Agents

#### <a name="issue"></a>Problem

Bei der Ausführung von `Set-DscLocalConfigurationManager` oder eines anderen DSC-Cmdlets tritt der folgende Fehler auf:

```error
Registration of the Dsc Agent with the server
https://<location>-agentservice-prod-1.azure-automation.net/accounts/00000000-0000-0000-0000-000000000000 failed. The
underlying error is: Failed to register Dsc Agent with AgentId 00000000-0000-0000-0000-000000000000 with the server htt
ps://<location>-agentservice-prod-1.azure-automation.net/accounts/00000000-0000-0000-0000-000000000000/Nodes(AgentId='00000000-0000-0000-0000-000000000000'). .
    + CategoryInfo          : InvalidResult: (root/Microsoft/...gurationManager:String) [], CimException
    + FullyQualifiedErrorId : RegisterDscAgentCommandFailed,Microsoft.PowerShell.DesiredStateConfiguration.Commands.Re
   gisterDscAgentCommand
    + PSComputerName        : <computerName>
```

#### <a name="cause"></a>Ursache

Dieser Fehler wird normalerweise durch eine Firewall, durch einen Computer hinter einem Proxyserver oder durch andere Netzwerkfehler verursacht.

#### <a name="resolution"></a>Lösung

Stellen Sie sicher, dass Ihr Computer Zugriff auf die richtigen Endpunkte für Azure Automation DSC hat, und wiederholen Sie den Vorgang. Eine Liste der erforderlichen Ports und Adressen finden Sie unter [Übersicht über Azure Automation State Configuration](../automation-dsc-overview.md#network-planning).

### <a name="failed-not-found"></a>Szenario: Knoten hat den Fehlerstatus „Nicht gefunden“

#### <a name="issue"></a>Problem

Der Knoten hat den Status **Fehler** mit der folgenden Meldung zurückgegeben:

```error
The attempt to get the action from server https://<url>//accounts/<account-id>/Nodes(AgentId=<agent-id>)/GetDscAction failed because a valid configuration <guid> cannot be found.
```

#### <a name="cause"></a>Ursache

Dieser Fehler tritt normalerweise auf, wenn der Knoten einem Konfigurationsnamen (z.B. ABC) anstatt einem Knotenkonfigurationsnamen (z.B. ABC.WebServer) zugewiesen ist.

#### <a name="resolution"></a>Lösung

* Stellen Sie sicher, dass Sie den Knoten mit dem „Knotenkonfigurationsnamen“ und nicht mit dem „Konfigurationsnamen“ zuweisen.
* Einem Knoten können Sie über das Azure-Portal oder mit einem PowerShell-Cmdlet eine Knotenkonfiguration zuweisen.

  * Um über das Azure-Portal einem Knoten eine Knotenkonfiguration zuzuweisen, öffnen Sie die Seite **DSC-Knoten**, wählen Sie dann einen Knoten aus, und klicken Sie auf die Schaltfläche **Knotenkonfiguration zuweisen**.
  * Um einem Knoten mit einem PowerShell-Cmdlet eine Knotenkonfiguration zuzuweisen, verwenden Sie das Cmdlet **Set-AzureRmAutomationDscNode**.

### <a name="no-mof-files"></a>Szenario: Bei der Kompilierung einer Konfiguration wurden keine Knotenkonfigurationen (MOF-Dateien) erstellt

#### <a name="issue"></a>Problem

Ihr DSC-Kompilierungsauftrag wird mit folgendem Fehler abgebrochen:

```error
Compilation completed successfully, but no node configuration.mofs were generated.
```

#### <a name="cause"></a>Ursache

Wenn der Ausdruck nach dem Schlüsselwort **Node** in der DSC-Konfiguration mit `$null` ausgewertet wird, werden keine Knotenkonfigurationen erstellt.

#### <a name="resolution"></a>Lösung

Sie können dieses Problem mit jeder der folgenden Lösungen beheben:

* Stellen Sie sicher, dass der Ausdruck neben dem Schlüsselwort **Node** in der Konfigurationsdefinition nicht mit „$null“ ausgewertet wird.
* Wenn Sie bei der Kompilierung der Konfiguration ConfigurationData übergeben, stellen Sie sicher, dass Sie die erwarteten Werte übergeben, die für die Konfiguration aus [ConfigurationData](../automation-dsc-compile.md)erforderlich sind.

### <a name="dsc-in-progress"></a>Szenario: Der DSC-Knotenbericht bleibt mit dem Status „In Bearbeitung“ hängen

#### <a name="issue"></a>Problem

Der DSC-Agent gibt Folgendes aus:

```error
No instance found with given property values
```

#### <a name="cause"></a>Ursache

Sie haben Ihre WMF-Version aktualisiert und WMI beschädigt.

#### <a name="resolution"></a>Lösung

Befolgen Sie zum Beheben des Problems die Anweisungen im Artikel [Bekannte Probleme und Einschränkungen bei DSC](https://docs.microsoft.com/powershell/scripting/wmf/known-issues/known-issues-dsc).

### <a name="issue-using-credential"></a>Szenario: In einer DSC-Konfiguration können keine Anmeldeinformationen verwendet werden

#### <a name="issue"></a>Problem

Ihr DSC-Kompilierungsauftrag wurde mit folgendem Fehler abgebrochen:

```error
System.InvalidOperationException error processing property 'Credential' of type <some resource name>: Converting and storing an encrypted password as plaintext is allowed only if PSDscAllowPlainTextPassword is set to true.
```

#### <a name="cause"></a>Ursache

Sie haben Anmeldeinformationen in einer Konfiguration verwendet, aber keine ordnungsgemäßen **ConfigurationData** angegeben, über die **PSDscAllowPlainTextPassword** für jede Knotenkonfiguration auf TRUE festgelegt wird.

#### <a name="resolution"></a>Lösung

* Stellen Sie sicher, dass Sie die ordnungsgemäßen **ConfigurationData** übergeben, über die **PSDscAllowPlainTextPassword** für jede Knotenkonfiguration auf „true“ festgelegt wird. Weitere Informationen finden Sie unter [Assets in Azure Automation DSC](../automation-dsc-compile.md#working-with-assets-in-azure-automation-during-compilation).

### <a name="failure-processing-extension"></a>Szenario: Onboarding über DSC-Erweiterung, Fehler beim Verarbeiten der Erweiterung

#### <a name="issue"></a>Problem

Beim Onboarding mithilfe der DSC-Erweiterung tritt der folgende Fehler auf:

```error
VM has reported a failure when processing extension 'Microsoft.Powershell.DSC'. Error message: \"DSC COnfiguration 'RegistrationMetaConfigV2' completed with error(s). Following are the first few: Registration of the Dsc Agent with the server <url> failed. The underlying error is: The attempt to register Dsc Agent with Agent Id <ID> with the server <url> return unexpected response code BadRequest. .\".
```

#### <a name="cause"></a>Ursache

Dieser Fehler tritt in der Regel auf, wenn dem Knoten ein Knotenkonfigurationsname zugewiesen wurde, der im Dienst nicht vorhanden ist.

#### <a name="resolution"></a>Lösung

* Stellen Sie sicher, dass Sie dem Knoten einen Knotenkonfigurationsnamen zuweisen, der genau mit dem im Dienst vorhandenen Namen übereinstimmt.
* Sie können festlegen, dass kein Knotenkonfigurationsname angegeben werden soll. In dem Fall erfolgt ein Onboarding des Knotens, aber keine Zuweisung einer Knotenkonfiguration.

### <a name="failure-linux-temp-noexec"></a>Szenario: Anwenden einer Konfiguration in Linux, dabei tritt allgemeiner Fehler auf

#### <a name="issue"></a>Problem

Wenn Sie eine Konfiguration in Linux anwenden, bei der der folgende Fehler auftritt:

```error
This event indicates that failure happens when LCM is processing the configuration. ErrorId is 1. ErrorDetail is The SendConfigurationApply function did not succeed.. ResourceId is [resource]name and SourceInfo is ::nnn::n::resource. ErrorMessage is A general error occurred, not covered by a more specific error code..
```

#### <a name="cause"></a>Ursache

Kunden haben festgestellt, dass die aktuelle Version von DSC Konfigurationen nicht übernimmt, wenn der Speicherort `/tmp` auf `noexec` festgelegt ist.

#### <a name="resolution"></a>Lösung

* Entfernen Sie die Option `noexec` aus dem Speicherort `/tmp`.

### <a name="compilation-node-name-overlap"></a>Szenario: Sich überschneidende Knotenkonfigurationsnamen können zu einem fehlerhaften Release führen.

#### <a name="issue"></a>Problem

Wenn zum Generieren mehrerer Knotenkonfigurationen ein einzelnes Skript verwendet wird und einige der Knotenkonfigurationen einen Namen besitzen, der in anderen enthalten ist, kann ein Problem beim Kompilierungsdienst dazu führen, dass die falsche Konfiguration zugewiesen wird.  Dies ist nur der Fall, wenn ein einzelnes Skript zum Generieren von Konfigurationen mit Konfigurationsdaten pro Knoten verwendet wird und die Namensüberschneidung am Anfang der Zeichenfolge auftritt.

Beispiel: Ein einzelnes Konfigurationsskript wird zum Generieren von Konfigurationen auf der Grundlage von Knotendaten verwendet, die unter Verwendung von Cmdlets als Hashtabelle übergeben werden, und die Knotendaten enthalten einen Server namens „server“ und „1server“.

#### <a name="cause"></a>Ursache

Bekanntes Problem beim Kompilierungsdienst

#### <a name="resolution"></a>Lösung

Sie umgehen das Problem am besten, indem Sie die Kompilierung lokal oder in einer CI/CD-Pipeline ausführen und die MOF-Dateien anschließend direkt in den Dienst hochladen.  Muss die Kompilierung im Dienst erfolgen, besteht die zweitbeste Problemumgehung darin, die Kompilierungsaufträge aufzuteilen, damit es keine Überschneidungen bei den Namen gibt.

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Erhalten Sie Antworten von Azure-Experten über [Azure-Foren](https://azure.microsoft.com/support/forums/).
* Mit [@AzureSupport](https://twitter.com/azuresupport) verbinden – das offizielle Microsoft Azure-Konto zur Verbesserung der Benutzerfreundlichkeit durch Verbinden der Azure-Community mit den richtigen Ressourcen: Antworten, Support und Experten.
* Wenn Sie weitere Hilfe benötigen, können Sie einen Azure-Supportvorgang anlegen. Rufen Sie die [Azure-Support-Website](https://azure.microsoft.com/support/options/) auf, und wählen Sie **Support erhalten**aus.
