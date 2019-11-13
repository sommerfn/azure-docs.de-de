---
title: Exportieren von Flows aus Power Automate in Azure Logic Apps
description: Migrieren von Flows aus Power Automate zu Azure Logic Apps durch Exportieren als Azure Resource Manager-Vorlagen
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: conceptual
ms.date: 07/10/2019
ms.openlocfilehash: e0dda5c2097243143d18851c47e7006c81769c87
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73583254"
---
# <a name="export-flows-from-power-automate-and-deploy-to-azure-logic-apps"></a>Exportieren von Flows aus Power Automate und Bereitstellen in Azure Logic Apps

Sie können diesen Flow aus [Microsoft Flow](https://flow.microsoft.com) zu [Power Automate](../logic-apps/logic-apps-overview.md) migrieren, um die Funktionen des Flows zu erweitern. Sie können den Flow als Azure Resource Manager-Vorlage für eine Logik-App exportieren, diese Logik-App-Vorlage in einer Azure-Ressourcengruppe bereitstellen und dann die Logik-App im Logik-App-Designer öffnen.

> [!NOTE]
> Nicht alle Power Automate-Connectors sind in Azure Logic Apps verfügbar. Sie können Flows importieren, die in Azure Logic Apps über [äquivalente Konnektoren](../connectors/apis-list.md) verfügen. Beispielsweise sind der Trigger „Schaltfläche“, der Connector „Genehmigung“ und der Connector „Benachrichtigung“ spezifisch für Power Automate.
>
> Auf OpenAPI basierende Flows, die aus Power Automate exportiert wurden, werden derzeit nicht für die Bereitstellung als Vorlagen für Logik-Apps unterstützt. 

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie sich [für ein kostenloses Azure-Konto registrieren](https://azure.microsoft.com/free/).

* Der Flow, den Sie aus Power Automate exportieren möchten.

## <a name="export-a-flow"></a>Exportieren eines Flows

1. Melden Sie sich bei [Power Automate](https://flow.microsoft.com) an, und wählen Sie **My Flows** aus. Suchen Sie den Flow und wählen Sie ihn aus. Klicken Sie auf der Symbolleiste auf die Schaltfläche mit den Auslassungszeichen ( **...** ). Klicken Sie auf **Exportieren** > **Logic Apps template (.json)** (Logic Apps-Vorlage (JSON)).

   ![Exportieren eines Flows](./media/export-from-microsoft-flow-logic-app-template/export-flow.png)

1. Speichern Sie die Vorlage am gewünschten Speicherort.

Weitere Informationen finden Sie unter [Verwenden von Azure Logic Apps](https://flow.microsoft.com/blog/grow-up-to-logic-apps/).

## <a name="deploy-template-by-using-the-azure-portal"></a>Bereitstellen einer Vorlage mithilfe des Azure-Portals

1. Melden Sie sich mit Ihrem Azure-Konto beim [Azure-Portal](https://portal.azure.com) an.

1. Wählen Sie im Azure-Menü **Ressource erstellen** aus. Geben Sie im Suchfeld „Vorlagenbereitstellung“ ein. Klicken Sie zunächst auf **Template deployment (deploy using custom templates)** (Vorlagenbereitstellung (Bereitstellen mithilfe benutzerdefinierter Vorlagen)) und anschließend auf **Erstellen**.

   ![Klicken Sie auf „Vorlagenbereitstellung“.](./media/export-from-microsoft-flow-logic-app-template/select-template-deployment.png)

1. Wählen Sie unter **Benutzerdefinierte Bereitstellung** die Option **Build your own template in the editor** (Eigene Vorlage im Editor erstellen) aus.

   ![Klicken Sie auf „Build your own template in the editor“ (Eigene Vorlage im Editor erstellen).](./media/export-from-microsoft-flow-logic-app-template/build-template-in-editor.png)

1. Klicken Sie auf der Symbolleiste **Vorlage bearbeiten** auf **Datei laden**. Suchen Sie die aus Power Automate exportierte JSON-Vorlage, wählen Sie sie aus, und klicken Sie dann auf **Öffnen**.

   ![Auswählen der Option „Datei laden“](./media/export-from-microsoft-flow-logic-app-template/load-file.png)

1. Nachdem der Editor die JSON-Datei, Parameter und Ressourcen in der Vorlage angezeigt hat, klicken Sie auf **Speichern**.
  
   ![Vorlage speichern](./media/export-from-microsoft-flow-logic-app-template/save-template.png)

1. Geben Sie nun die Eingabeparameter für die Vorlage an:

   * Azure-Abonnement für die Abrechnung
   * Azure-Ressourcengruppe
   * Speicherort für die Azure-Ressourcengruppe
   * Name für die Logik-App-Ressource
   * Speicherort für die Logik-App-Ressource, wenn sie sich von der Azure-Ressourcengruppe unterscheidet
   * Name für alle zuvor erstellten Verbindungen, die von der Logik-App verwendet werden können

      Wenn Sie Ihre erste Logik-App erstellen, werden alle Verbindungen neu erstellt, sodass Sie die Standardnamen übernehmen können. Andernfalls können Sie die Namen für zuvor erstellte Verbindungen angeben, die Sie in mehreren Logik-Apps verwenden können.

   Nachdem Sie diese Informationen für die Vorlage bereitgestellt haben, müssen Sie die Azure Marketplace-Geschäftsbedingungen zum Erstellen der erforderlichen Azure-Ressourcen und der Abrechnung des Azure-Abonnements lesen und diesen zustimmen. Klicken Sie dann auf **Kaufen**.
  
   ![Angeben der Eingabeparameter für die Vorlage](./media/export-from-microsoft-flow-logic-app-template/template-input-parameters.png)

   Azure stellt die Vorlage als Logik-App für die angegebene Ressourcengruppe bereit. Alle von Power Automate migrierten Logik-Apps werden in deaktiviertem Zustand bereitgestellt.

1. Autorisieren Sie vor dem Aktivieren der Logik-App alle neuen Verbindungen, indem Sie die folgenden Schritte ausführen:

   1. Öffnen Sie die erstellte Logik-App. Klicken Sie im Menü der Logik-App auf **Logic app designer** (Logik-App-Designer).

      Für jede Verbindung, die eine Autorisierung erfordert, wird ein Warnsymbol angezeigt:

      ![Symbol "Warnung"](./media/export-from-microsoft-flow-logic-app-template/authorize-connections.png)

   1. Erweitern Sie jeden Schritt, für den eine autorisierte Verbindung erforderlich ist, und klicken Sie auf **Neue hinzufügen**.

      ![Hinzufügen einer neuen Verbindung](./media/export-from-microsoft-flow-logic-app-template/add-new-connection.png)

   1. Melden Sie sich bei jedem Dienst an, oder geben Sie die erforderlichen Anmeldeinformationen zum Autorisieren der Verbindung an.

1. Speichern Sie Ihre Logik-App. Klicken Sie zum Aktivieren der Logik-App im Menü der Logik-App zunächst auf **Übersicht** und dann auf **Aktivieren**.

   ![Aktivieren der Logik-App](./media/export-from-microsoft-flow-logic-app-template/enable-logic-app.png)

1. Stellen Sie sicher, dass Sie den ursprünglichen Flow deaktiviert oder gelöscht haben, um die Ausführung doppelter Workflows zu vermeiden.

## <a name="deploy-template-by-using-visual-studio"></a>Bereitstellen einer Vorlage mithilfe von Visual Studio

Wenn Sie Visual Studio mit den [Voraussetzungen](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md#prerequisites) für das Erstellen von Logik-Apps eingerichtet haben, können Sie die exportierte Vorlage aus Visual Studio in Azure Logic Apps bereitstellen.

1. Öffnen Sie in Visual Studio die Vorlagendatei, die Sie aus Power Automate exportiert haben.

1. Erstellen Sie in Visual Studio ein Azure-Ressourcengruppenprojekt, und klicken Sie dann auf die **Logik-App**-Vorlage, indem Sie die in [Quickstart: Create automated tasks, processes, and workflows with Azure Logic Apps – Visual Studio](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md) (Schnellstart: Erstellen von automatisierten Aufgaben, Prozessen und Workflows mit Azure Logic Apps – Visual Studio) beschriebenen Schritte ausführen. Beispiel:

   ![Erstellen eines Azure-Ressourcengruppenprojekts](./media/export-from-microsoft-flow-logic-app-template/create-azure-resource-group-project.png)

1. Öffnen Sie im Projektmappen-Explorer die Datei **LogicApp.json**, sofern sie noch nicht geöffnet ist.

1. Kopieren Sie den Inhalt aus der exportierten Vorlage, und überschreiben Sie den Inhalt in der Datei **LogicApp.json**.

1. Autorisieren Sie vor dem Bereitstellen der Logik-App alle neuen Verbindungen, indem Sie die folgenden Schritte ausführen:

   1. Öffnen Sie das Kontextmenü der **LogicApp.json**-Datei, und klicken Sie dann auf **Open With Logic App Designer** (Mit Logik-App-Designer öffnen).

      ![Öffnen der Vorlage mit dem Logik-App-Designer](./media/export-from-microsoft-flow-logic-app-template/open-logic-app-designer.png)

   1. Wählen Sie das Azure-Abonnement und die Ressourcengruppe aus, die Sie für die Bereitstellung Ihrer Logik-App verwenden möchten, wenn Sie dazu aufgefordert werden.

      ![Auswählen des Azure-Abonnements und der Ressourcengruppe](./media/export-from-microsoft-flow-logic-app-template/select-azure-subscription-resource-group-deployment.png)

      Nachdem die Logik-App im Designer angezeigt wird, werden für alle Verbindungen, die eine Autorisierung erfordern, Warnsymbole angezeigt:

      ![Verbindungen mit Warnsymbolen](./media/export-from-microsoft-flow-logic-app-template/authorize-connections-vs.png)

   1. Erweitern Sie jeden Schritt, für den eine autorisierte Verbindung erforderlich ist, und klicken Sie auf **Neue hinzufügen**.

      ![Hinzufügen einer neuen Verbindung](./media/export-from-microsoft-flow-logic-app-template/add-new-connection-vs.png)

   1. Melden Sie sich bei jedem Dienst an, oder geben Sie die erforderlichen Anmeldeinformationen zum Autorisieren der Verbindung an.

   1. Speichern Sie die Projektmappe, bevor Sie die Logik-App bereitstellen.

1. Öffnen Sie im Projektmappen-Explorer das Kontextmenü für das Projekt, und klicken Sie dann auf **Bereitstellen** > **Neu**. Melden Sie sich nach Aufforderung mit Ihrem Azure-Konto an.

1. Bestätigen Sie, falls Sie dazu aufgefordert werden, das Azure-Abonnement, die Azure-Ressourcengruppe und die anderen Einstellungen, die Sie für die Bereitstellung verwenden möchten (beispielsweise eine [Parameterdatei](../azure-resource-manager/resource-group-template-deploy.md#parameter-files) zum Übergeben von Vorlagenparameterwerten), und klicken Sie dann auf **Bereitstellen**.

   ![Bestätigen der Bereitstellungseinstellungen](./media/export-from-microsoft-flow-logic-app-template/confirm-azure-subscription-resource-group-deployment.png)

1. Wenn das Feld **Parameter bearbeiten** angezeigt wird, geben Sie den Namen für Ihre Logik-App-Ressource an, und klicken Sie dann auf **Speichern**.  

   ![Bearbeiten von Bereitstellungsparametern](./media/export-from-microsoft-flow-logic-app-template/edit-parameters-deployment.png)

   Wenn die Bereitstellung gestartet wird, wird der Bereitstellungsstatus Ihrer App im Fenster **Ausgabe** von Visual Studio angezeigt. Falls der Status nicht angezeigt wird, können Sie die Liste **Ausgabe anzeigen von** öffnen und Ihre Azure-Ressourcengruppe auswählen. Beispiel:

   ![Fenster „Ausgabe“](./media/export-from-microsoft-flow-logic-app-template/output-window.png)

   Wenn Verbindungen in der Logik-App eine Eingabe von Ihnen benötigen, wird im Hintergrund ein PowerShell-Fenster geöffnet, das zur Eingabe von erforderlichen Kennwörtern oder geheimen Schlüsseln auffordert. Nachdem Sie diese Informationen eingegeben haben, wird die Bereitstellung fortgesetzt.

   ![Authentifizieren von Verbindungen](./media/export-from-microsoft-flow-logic-app-template/logic-apps-powershell-window.png)

   Nach Abschluss der Bereitstellung wird die Logik-App zwar veröffentlicht, ist allerdings noch nicht im Azure-Portal aktiviert.

1. Suchen Sie die Logik-App im Logik-App-Designer und öffnen Sie diese, wenn Sie die Logik-App im Azure-Portal aktivieren möchten. Klicken Sie im Menü der Logik-App zunächst auf **Übersicht** und dann auf **Aktivieren**.

1. Stellen Sie sicher, dass Sie den ursprünglichen Flow deaktiviert oder gelöscht haben, um die Ausführung doppelter Workflows zu vermeiden.

Weitere Informationen zu diesen Bereitstellungsschritten finden Sie unter [Schnellstart: Erstellen von automatisierten Aufgaben, Prozessen und Workflows mit Azure Logic Apps – Visual Studio](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md#deploy-to-Azure).

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu [Connectors für Azure Logic Apps](../connectors/apis-list.md)
* Weitere Informationen zu [Azure Logic Apps](../logic-apps/logic-apps-overview.md)
