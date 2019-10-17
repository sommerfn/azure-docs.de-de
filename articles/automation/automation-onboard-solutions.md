---
title: Integrieren von Update- und Änderungsnachverfolgungslösungen in Azure Automation
description: Es wird beschrieben, wie Sie Update- und Änderungsnachverfolgungslösungen in Azure Automation integrieren.
services: automation
ms.service: automation
author: eamonoreilly
ms.author: eamono
manager: carmonm
ms.topic: tutorial
ms.date: 05/10/2018
ms.custom: mvc
ms.openlocfilehash: cacd9206eb0213d410a11ee3a2dcf5c0e28989c6
ms.sourcegitcommit: 0576bcb894031eb9e7ddb919e241e2e3c42f291d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "72372828"
---
# <a name="onboard-update-and-change-tracking-solutions-to-azure-automation"></a>Integrieren von Update- und Änderungsnachverfolgungslösungen in Azure Automation

In diesem Tutorial wird beschrieben, wie Sie Update-, Änderungsnachverfolgungs- und Inventarlösungen für VMs in Azure Automation integrieren:

> [!div class="checklist"]
> * Integrieren einer Azure-VM
> * Aktivieren von Lösungen
> * Installieren und Aktualisieren von Modulen
> * Importieren des Runbooks für die Integration
> * Starten des Runbooks

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorials müssen folgende Voraussetzungen erfüllt sein:

* Azure-Abonnement. Wenn Sie noch kein Abonnement haben, können Sie Ihre [MSDN-Abonnentenvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oder sich für ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) registrieren.
* [Automation-Konto](automation-offering-get-started.md) zum Verwalten von Computern.
* Einen [virtuellen Computer](../virtual-machines/windows/quick-create-portal.md), der integriert werden soll.

## <a name="onboard-an-azure-vm"></a>Integrieren einer Azure-VM

Es gibt mehrere Möglichkeiten, Computer zu integrieren. Sie können die Lösung [über einen virtuellen Computer](automation-onboard-solutions-from-vm.md), [durch Durchsuchen mehrerer Computer](automation-onboard-solutions-from-browse.md), [über Ihr Automation-Konto](automation-onboard-solutions-from-automation-account.md) oder per Runbook integrieren. In diesem Tutorial wird das Aktivieren der Updateverwaltung über ein Runbook Schritt für Schritt beschrieben. Um virtuelle Azure-Computer bedarfsgesteuert zu integrieren, muss eine vorhandene VM mit der Lösung für die Änderungsnachverfolgung oder Updateverwaltung integriert werden. In diesem Schritt integrieren Sie eine VM mithilfe der Updateverwaltung und Änderungsnachverfolgung.

### <a name="enable-change-tracking-and-inventory"></a>Aktivieren der Lösung für Änderungsnachverfolgung und Bestand

Die Lösung für Änderungsnachverfolgung und Bestand bietet die Möglichkeit zum Nachverfolgen von [Änderungen](automation-vm-change-tracking.md) und [Bestand](automation-vm-inventory.md) in Ihren virtuellen Computern. In diesem Schritt aktivieren Sie die Lösung auf einem virtuellen Computer.

1. Klicken Sie im Menü links auf die Option **Automation-Konten**, und wählen Sie dann Ihr Automation-Konto in der Liste aus.
1. Klicken Sie unter **KONFIGURATIONSVERWALTUNG** auf **Bestand**.
1. Wählen Sie einen vorhandenen Log Analytics-Arbeitsbereich, oder erstellen Sie einen neuen. Klicken Sie auf die Schaltfläche **Aktivieren**.

![Integrieren der Updatelösung](media/automation-onboard-solutions/inventory-onboard.png)

Wenn Sie die Benachrichtigung erhalten haben, dass die Lösung für Änderungsnachverfolgung und Bestand integriert wurde, klicken Sie unter **KONFIGURATIONSVERWALTUNG** auf **Updateverwaltung**.

### <a name="enable-update-management"></a>Aktivieren der Updateverwaltung

Mithilfe der Lösung zur Updateverwaltung können Sie Updates und Patches für Ihre Azure-Windows-VMs verwalten. Sie können den Status der verfügbaren Updates bewerten, die Installation der erforderlichen Updates planen und Bereitstellungsergebnisse überprüfen, um sicherzustellen, dass Updates erfolgreich auf die VM angewendet wurden. In diesem Schritt aktivieren Sie die Lösung für Ihre VM.

1. Wählen Sie in Ihrem Automation-Konto unter **UPDATEVERWALTUNG** die Option **Updateverwaltung** aus.
1. Der ausgewählte Log Analytics-Arbeitsbereich ist derselbe, der im vorherigen Schritt verwendet wurde. Klicken Sie auf **Aktivieren**, um die Lösung für die Updateverwaltung zu integrieren.

![Integrieren der Updatelösung](media/automation-onboard-solutions/update-onboard.png)

Während die Lösung zur Updateverwaltung installiert wird, wird ein blaues Banner angezeigt. Wenn die Lösung aktiviert wurde, klicken Sie unter **KONFIGURATIONSVERWALTUNG** auf **Änderungsnachverfolgung**, um zum nächsten Schritt zu wechseln.

### <a name="select-azure-vm-to-be-managed"></a>Auswählen der zu verwaltenden VM

Nachdem die Lösungen aktiviert wurden, können Sie eine Azure-VM hinzufügen, die in diese Lösungen integriert werden soll.

1. Wählen Sie in Ihrem Automation-Konto auf der Seite **Änderungsnachverfolgung** die Option **+ Azure-VM hinzufügen** aus, um Ihren virtuellen Computer hinzuzufügen.

1. Wählen Sie Ihre VM aus der Liste aus, und klicken Sie auf **Aktivieren**. Hierdurch wird die Lösung für Änderungsnachverfolgung und Bestand für den virtuellen Computer aktiviert.

   ![Aktivieren der Updatelösung für die VM](media/automation-onboard-solutions/enable-change-tracking.png)

1. Wenn Sie die Benachrichtigung erhalten haben, dass die VM integriert wurde, klicken Sie in Ihrem Automation-Konto unter **UPDATEVERWALTUNG** auf **Updateverwaltung**.

1. Wählen Sie **+ Azure-VM hinzufügen** aus, um Ihren virtuellen Computer hinzuzufügen.

1. Wählen Sie Ihre VM aus der Liste aus, und klicken Sie auf **Aktivieren**. Hierdurch wird die Lösung zur Updateverwaltung für den virtuellen Computer aktiviert.

   ![Aktivieren der Updatelösung für die VM](media/automation-onboard-solutions/enable-update.png)

> [!NOTE]
> Wenn Sie nicht auf den Abschluss der anderen Lösung warten, erhalten Sie beim Aktivieren der nächsten Lösung eine Nachricht folgenden Inhalts: *Eine andere Lösung wird zurzeit auf diesem oder einem anderen virtuellen Computer installiert. Nach Abschluss dieser Installation wird die Schaltfläche „Aktivieren“ aktiviert, und Sie können die Installation der Lösung auf diesem virtuellen Computer anfordern.*

## <a name="install-and-update-modules"></a>Installieren und Aktualisieren von Modulen

Es ist erforderlich, dass Sie ein Update auf die aktuellen Azure-Module durchführen und `AzureRM.OperationalInsights` importieren, um die Lösungsintegration erfolgreich zu automatisieren.

## <a name="update-azure-modules"></a>Aktualisieren von Azure-Modulen

Wählen Sie in Ihrem Automation-Konto unter **FREIGEGEBENE RESSOURCEN** die Option **Module** aus. Klicken Sie auf **Azure-Module aktualisieren**, um die Azure-Module auf die neueste Version zu aktualisieren. Klicken Sie bei Aufforderung auf **Ja**, um alle vorhandenen Azure-Module auf die aktuelle Version zu aktualisieren.

![Aktualisieren von Modulen](media/automation-onboard-solutions/update-modules.png)

### <a name="install-azurermoperationalinsights-module"></a>Installieren des AzureRM.OperationalInsights-Moduls

Wählen Sie auf der Seite **Module** die Option **Katalog durchsuchen** aus, um den Modulkatalog zu öffnen. Suchen Sie nach AzureRM.OperationalInsights, und importieren Sie dieses Modul in das Automation-Konto.

![Importieren des OperationalInsights-Moduls](media/automation-onboard-solutions/import-operational-insights-module.png)

## <a name="import-the-onboarding-runbook"></a>Importieren des Runbooks für die Integration

1. Wählen Sie in Ihrem Automation-Konto unter **PROZESSAUTOMATISIERUNG** die Option **Runbooks** aus.
1. Klicken Sie auf **Katalog durchsuchen**.
1. Suchen Sie nach **Update und Änderungsnachverfolgung**, klicken Sie auf das Runbook, und wählen Sie auf der Seite **Quelle anzeigen** die Option **Importieren** aus. Klicken Sie auf **OK**, um das Runbook in das Automation-Konto zu importieren.

   ![Importieren des Runbooks für die Integration](media/automation-onboard-solutions/import-from-gallery.png)

1. Klicken Sie auf der Seite **Runbook** auf **Bearbeiten**, und wählen Sie **Veröffentlichen** aus. Klicken Sie im Dialogfeld **Runbook veröffentlichen** auf **Ja**, um das Runbook zu veröffentlichen.

## <a name="start-the-runbook"></a>Starten des Runbooks

Sie müssen entweder eine Änderungsnachverfolgungs- oder Updatelösung in eine Azure-VM integriert haben, um dieses Runbook starten zu können. Hierfür sind ein vorhandener virtueller Computer und eine Ressourcengruppe sowie die integrierte Lösung für Parameter erforderlich.

1. Öffnen Sie das Runbook „Enable-MultipleSolution“.

   ![Runbooks für mehrere Lösungen](media/automation-onboard-solutions/runbook-overview.png)

1. Klicken Sie auf die Schaltfläche „Start“, und geben Sie die folgenden Werte für die Parameter ein:

   * **VMNAME**: Lassen Sie diesen Wert leer. Der Name einer vorhandenen VM für die Integration der Update- oder Änderungsnachverfolgungslösung. Wenn Sie diesen Wert leer lassen, werden alle VMs der Ressourcengruppe integriert.
   * **VMRESOURCEGROUP**: Der Name der Ressourcengruppe für die VMs, die integriert werden sollen.
   * **SUBSCRIPTIONID**: Lassen Sie diesen Wert leer. Die Abonnement-ID der neuen VM, die integriert werden soll. Wenn Sie diesen Wert leer lassen, wird das Abonnement des Arbeitsbereichs verwendet. Wenn eine andere Abonnement-ID angegeben wird, sollte das RunAs-Konto für das Automation-Konto auch als Mitwirkender für das Abonnement hinzugefügt werden.
   * **ALREADYONBOARDEDVM**: Der Name der VM, die für die Update- oder Änderungsnachverfolgungslösung manuell integriert wurde.
   * **ALREADYONBOARDEDVMRESOURCEGROUP**: Der Name der Ressourcengruppe, der die VM angehört.
   * **SOLUTIONTYPE**: Geben Sie **Updates** oder **ChangeTracking** ein.

   ![Parameter für das Enable-MultipleSolution-Runbook](media/automation-onboard-solutions/runbook-parameters.png)

1. Klicken Sie auf **OK**, um den Runbookauftrag zu starten.
1. Überwachen Sie den Status und etwaige Fehler auf der Seite für den Runbookauftrag.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

So entfernen Sie einen virtuellen Computer aus der Updateverwaltung:

* Entfernen Sie in Ihrem Log Analytics-Arbeitsbereich den virtuellen Computer aus der gespeicherten Suche für die Bereichskonfiguration `MicrosoftDefaultScopeConfig-Updates`. Gespeicherte Suchen finden Sie unter **Allgemein** in Ihrem Arbeitsbereich.
* Entfernen Sie den [Microsoft Monitoring Agent](../azure-monitor/learn/quick-collect-windows-computer.md#clean-up-resources) oder den [Log Analytics Agent für Linux](../azure-monitor/learn/quick-collect-linux-computer.md#clean-up-resources).

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Manuelles Integrieren eines virtuellen Azure-Computers
> * Installieren und Aktualisieren der erforderlichen Azure-Module
> * Importieren eines Runbooks für die Integration von Azure-VMs
> * Starten des Runbooks für die automatische Integration von Azure-VMs

Klicken Sie auf diesen Link, um weitere Informationen zum Planen von Runbooks zu erhalten:

> [!div class="nextstepaction"]
> [Planen von RunBooks](automation-schedules.md)
