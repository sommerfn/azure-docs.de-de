---
title: Azure-Schnellstart – Erstellen eines Azure Automation-Kontos | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie ein Azure Automation-Konto erstellen und ein Runbook ausführen.
services: automation
author: csand-msft
ms.author: csand
ms.date: 04/04/2019
ms.topic: quickstart
ms.service: automation
ms.subservice: process-automation
ms.custom: mvc
ms.openlocfilehash: 89f32499d02978294398aae7be21f63b5d6aa028
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/30/2019
ms.locfileid: "64919811"
---
# <a name="create-an-azure-automation-account"></a>Erstellen eines Azure Automation-Kontos

Azure Automation-Konten können über Azure erstellt werden. Diese Methode bietet eine browserbasierte Benutzeroberfläche zum Erstellen und Konfigurieren von Automation-Konten und den dazugehörigen Ressourcen. In dieser Schnellstartanleitung werden die Erstellung eines Automation-Kontos und die Ausführung eines Runbooks im Konto Schritt für Schritt beschrieben.

Wenn Sie über kein Azure-Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter https://portal.azure.com bei Azure an.

## <a name="create-automation-account"></a>Erstellen eines Automation-Kontos

1. Klicken Sie in der linken oberen Ecke von Azure auf die Schaltfläche **Ressource erstellen**.

1. Wählen Sie **Verwaltungsprogramme** und dann **Automation**.

1. Geben Sie die Kontoinformationen ein. Wählen Sie für **Ausführendes Azure-Konto erstellen** die Option **Ja**, damit die Artefakte zum Vereinfachen der Authentifizierung für Azure automatisch aktiviert werden. Beachten Sie beim Erstellen eines Automation-Kontos unbedingt, dass der Name nach dem Auswählen nicht geändert werden kann. *Automation-Kontonamen sind für jede Region und Ressourcengruppe eindeutig. Namen für Automation-Konten, die gelöscht wurden, sind möglicherweise nicht sofort verfügbar.* Ein Automation-Konto kann Ressourcen in allen Regionen und Abonnements für einen bestimmten Mandanten verwalten. Klicken Sie nach Abschluss des Vorgangs auf **Erstellen**, um die Bereitstellung des Automation-Kontos zu starten.

    ![Eingeben von Informationen zu Ihrem Automation-Konto auf der Seite](./media/automation-quickstart-create-account/create-automation-account-portal-blade.png)  

    > [!NOTE]
    > Eine aktualisierte Liste mit Standorten, an denen Sie ein Automation-Konto bereitstellen können, finden Sie unter [Verfügbare Produkte nach Region](https://azure.microsoft.com/global-infrastructure/services/?products=automation&regions=all).

1. Klicken Sie nach Abschluss der Bereitstellung auf **Alle Dienste**, und wählen Sie **Automation-Konten** und anschließend das von Ihnen erstellte Automation-Konto aus.

    ![Automation-Konto – Übersicht](./media/automation-quickstart-create-account/automation-account-overview.png)

## <a name="run-a-runbook"></a>Ausführen eines Runbooks

Führen Sie ein Runbook des Tutorials aus.

1. Klicken Sie unter **PROZESSAUTOMATISIERUNG** auf **Runbooks**. Die Liste mit den Runbooks wird angezeigt. Standardmäßig sind mehrere Tutorial-Runbooks im Konto aktiviert.

    ![Liste mit Runbooks des Automation-Kontos](./media/automation-quickstart-create-account/automation-runbooks-overview.png)

1. Wählen Sie das Runbook **AzureAutomationTutorialScript** aus. Die Seite mit der Runbookübersicht wird geöffnet.

    ![Runbookübersicht](./media/automation-quickstart-create-account/automation-tutorial-script-runbook-overview.png)

1. Klicken Sie auf **Starten** und auf der Seite **Runbook starten** dann auf **OK**, um das Runbook zu starten.

    ![Seite für Runbookauftrag](./media/automation-quickstart-create-account/automation-tutorial-script-job.png)

1. Nachdem sich der **Auftragsstatus** in **Wird ausgeführt** geändert hat, können Sie auf **Ausgabe** oder **Alle Protokolle** klicken, um die Ausgabe des Runbookauftrags anzuzeigen. Für dieses Tutorial-Runbook ist die Ausgabe eine Liste mit Ihren Azure-Ressourcen.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie ein Automation-Konto bereitgestellt, einen Runbookauftrag gestartet und die Auftragsergebnisse angezeigt. Weitere Informationen zu Azure Automation finden Sie in der Schnellstartanleitung zum Erstellen Ihres ersten Runbooks.

> [!div class="nextstepaction"]
> [Automation-Schnellstart – Erstellen eines Runbooks](./automation-quickstart-create-runbook.md)

