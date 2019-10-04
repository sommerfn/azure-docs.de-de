---
title: Aktualisieren von Azure-Modulen in Azure Automation
description: In diesem Artikel wird beschrieben, wie Sie jetzt häufig verwendete Azure PowerShell-Module, die standardmäßig in Azure Automation bereitgestellt werden, aktualisieren können.
services: automation
ms.service: automation
ms.subservice: process-automation
author: bobbytreed
ms.author: robreed
ms.date: 06/14/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 23475fb77210eeea0568bb996529c81458db9c6c
ms.sourcegitcommit: 49c4b9c797c09c92632d7cedfec0ac1cf783631b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/05/2019
ms.locfileid: "70382757"
---
# <a name="how-to-update-azure-powershell-modules-in-azure-automation"></a>Aktualisieren von Azure PowerShell-Modulen in Azure Automation

Zum Aktualisieren der Azure-Module in Ihrem Automation-Konto müssen Sie das [Runbook „Update Azure Modules“](https://github.com/Microsoft/AzureAutomation-Account-Modules-Update) verwenden, das als Open Source verfügbar ist. Um in die Verwendung des **Update-AutomationAzureModulesForAccount**-Runbooks zum Aktualisieren Ihrer Azure-Module einzusteigen, laden Sie es aus dem [Update Azure Modules-Runbook-Repository](https://github.com/Microsoft/AzureAutomation-Account-Modules-Update) auf GitHub herunter. Anschließend können Sie es in Ihr Automation-Konto importieren oder es als Skript ausführen. Informationen zum Importieren eines Runbooks in Ihr Automation-Konto finden Sie unter [Importieren eines Runbooks](manage-runbooks.md#import-a-runbook).

Die am häufigsten verwendeten AzureRM PowerShell-Module werden standardmäßig in jedem Automation-Konto bereitgestellt. Das Azure-Team aktualisiert die Azure-Module regelmäßig. Daher sollten Sie das Runbook [Update-AutomationAzureModulesForAccount](https://github.com/Microsoft/AzureAutomation-Account-Modules-Update) verwenden, um die Module in Ihren Automation-Konten so zu aktualisieren, dass sie auf dem neuesten Stand bleiben.

Da Module von der Produktgruppe regelmäßig aktualisiert werden, können in den enthaltenen Cmdlets Änderungen auftreten. Diese Aktion kann je nach Art der Änderung nachteilige Auswirkungen auf Ihre Runbooks haben, z.B. durch umbenannte Parameter oder ein vollständig eingestelltes Cmdlet.

Um Auswirkungen auf Ihre Runbooks und die Prozesse zu vermeiden, die sie automatisieren, führen Sie vor dem Fortsetzen des Vorgangs Tests und Überprüfungen aus. Wenn Sie für diesen Zweck kein dediziertes Automation-Konto haben, sollten Sie eines erstellen, damit Sie während der Entwicklung Ihrer Runbooks viele verschiedene Szenarios testen können. Diese Tests sollten iterative Änderungen wie das Aktualisieren der PowerShell-Module einschließen.

Wenn Sie Ihre Skripts lokal entwickeln, wird empfohlen, lokal die gleichen Modulversionen wie in Ihrem Automation-Konto beim Testen zur Verfügung zu haben, um sicherzustellen, dass Sie die gleichen Ergebnisse erhalten. Nach dem Überprüfen der Ergebnisse und dem Anwenden aller erforderlichen Änderungen können Sie die Änderungen in die Produktion übernehmen.

> [!NOTE]
> Ein neues Automation-Konto enthält ggf. nicht die neuesten Module.

> [!NOTE]
> Sie können keine globalen Module löschen, also Module, die im Lieferumfang der Automatisierung bereitstellt werden.

## <a name="considerations"></a>Überlegungen

Die folgenden Überlegungen sollten bei der Anwendung dieses Prozesses zum Aktualisieren Ihrer Azure-Module berücksichtigt werden:

* Dieses Runbook unterstützt standardmäßig die Aktualisierung der Module **Azure** und **AzureRM**. Dieses Runbook unterstützt auch die Aktualisierung der **Az**-Module. Weitere Informationen zum Aktualisieren von `Az`-Modulen mit diesem Runbook finden Sie in der [README-Datei zum Runbook zum Aktualisieren von Azure-Modulen](https://github.com/microsoft/AzureAutomation-Account-Modules-Update/blob/master/README.md). Es gibt weitere wichtige Faktoren, die Sie bei der Verwendung der `Az`-Module in Ihrem Automation-Konto berücksichtigen müssen. Weitere Informationen finden Sie unter [Verwenden von Az-Modulen in Ihrem Automation-Konto](az-modules.md).

* Bevor Sie dieses Runbook starten, vergewissern Sie sich, dass für Ihr Automation-Konto [Azure Run As-Kontoanmeldeinformationen](manage-runas-account.md) erstellt wurden.

* Sie können diesen Code als reguläres PowerShell-Skript statt als Runbook verwenden. Melden Sie sich einfach zuerst mit dem Befehl [Connect-AzureRmAccount](/powershell/module/azurerm.profile/connect-azurermaccount) bei Azure an, und übergeben Sie dann `-Login $false` an das Skript.

* Zur Ausführung dieses Runbooks in Sovereign Clouds verwenden Sie den `AzureRmEnvironment`-Parameter, um die richtige Umgebung an das Runbook zu übergeben.  Zulässige Werte sind **AzureCloud**, **AzureChinaCloud**, **AzureGermanCloud** und **AzureUSGovernment**. Diese Werte können mithilfe von `Get-AzureRmEnvironment | select Name` abgerufen werden. Wenn Sie keinen Wert an diesen Parameter übergeben, wird für das Runbook standardmäßig die öffentliche Azure-Cloud **AzureCloud** verwendet.

* Falls Sie eine bestimmte Azure PowerShell-Modulversion anstelle der neuesten im PowerShell-Katalog verfügbaren verwenden möchten, übergeben Sie diese Versionen im optionalen Parameter `ModuleVersionOverrides` des **Update-AutomationAzureModulesForAccount**-Runbooks. Beispiele finden Sie im [Update-AutomationAzureModulesForAccount.ps1](https://github.com/Microsoft/AzureAutomation-Account-Modules-Update/blob/master/Update-AutomationAzureModulesForAccount.ps1
)-Runbook. Azure PowerShell-Module, die im Parameter `ModuleVersionOverrides` nicht erwähnt sind, werden mit den aktuellen Modulversionen aus dem PowerShell-Katalog aktualisiert. Wenn Sie keine Argumente an den Parameter `ModuleVersionOverrides` übergeben, werden alle Module mit den aktuellen Modulversionen aus dem PowerShell-Katalog aktualisiert. Dieses Verhalten entspricht dem der Schaltfläche **Azure-Module aktualisieren**.

## <a name="next-steps"></a>Nächste Schritte

Besuchen Sie die Seite [Update Azure Modules-Runbook](https://github.com/Microsoft/AzureAutomation-Account-Modules-Update), um mehr darüber zu erfahren.
