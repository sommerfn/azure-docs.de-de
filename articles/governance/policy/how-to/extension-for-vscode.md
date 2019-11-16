---
title: Azure Policy-Erweiterung für Visual Studio Code
description: Erfahren Sie, wie Sie die Azure Policy-Erweiterung für Visual Studio Code zum Suchen nach Resource Manager-Aliasen verwenden.
ms.date: 11/04/2019
ms.topic: conceptual
ms.openlocfilehash: cbc0e14ed70fbacc297126aef58cd722d2235a3d
ms.sourcegitcommit: a107430549622028fcd7730db84f61b0064bf52f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2019
ms.locfileid: "74072039"
---
# <a name="use-azure-policy-extension-for-visual-studio-code"></a>Verwenden der Azure Policy-Erweiterung für Visual Studio Code

> Gilt für Version **0.0.21** und höher der Azure Policy-Erweiterung.

Erfahren Sie, wie Sie die Azure Policy-Erweiterung für Visual Studio Code verwenden, um nach [Aliasen](../concepts/definition-structure.md#aliases) zu suchen und Ressourcen und Richtlinien zu überprüfen. Zunächst wird beschrieben, wie Sie die Azure Policy-Erweiterung in Visual Studio Code installieren. Anschließend erfahren Sie, wie Sie Aliase suchen.

Die Azure Policy-Erweiterung für Visual Studio Code kann auf allen Plattformen installiert werden, die von Visual Studio Code unterstützt werden. Diese Unterstützung umfasst Windows, Linux und macOS.

> [!NOTE]
> Lokal an Richtlinien vorgenommene Änderungen, die in der Azure Policy-Erweiterung für Visual Studio Code angezeigt werden, werden nicht mit Azure synchronisiert.

## <a name="prerequisites"></a>Voraussetzungen

Für die Schritte in diesem Artikel ist Folgendes erforderlich:

- Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.
- [Visual Studio Code](https://code.visualstudio.com).

## <a name="install-azure-policy-extension"></a>Installieren der Azure Policy-Erweiterung

Wenn die Voraussetzungen erfüllt sind, können Sie die Azure Policy-Erweiterung für Visual Studio Code mithilfe der folgenden Schritte installieren:

1. Öffnen Sie Visual Studio Code.

1. Navigieren Sie auf der Menüleiste zu **Ansicht** > **Erweiterungen**.

1. Geben Sie im Suchfeld **Azure Policy** ein.

1. Wählen Sie in den Suchergebnissen **Azure Policy** aus, und wählen Sie dann **Installieren**.

1. Wählen Sie bei Bedarf **Erneut laden** aus.

## <a name="set-the-azure-environment"></a>Einrichten der Azure-Umgebung

Führen Sie für Benutzer der nationalen Cloud die folgenden Schritte aus, um zunächst die Azure-Umgebung festzulegen:

1. Wählen Sie **Datei > Einstellungen > Einstellungen** aus.

1. Suchen Sie nach folgender Zeichenfolge: _Azure: Cloud_.

1. Wählen Sie in der Liste die nationale Cloud aus:

   ![Festlegen der Anmeldung bei der Azure-Standardcloud für Visual Studio Code](../media/extension-for-vscode/set-default-azure-cloud-sign-in.png)

## <a name="connect-to-an-azure-account"></a>Verbinden mit einem Azure-Konto

Zum Auswerten von Ressourcen und Suchen nach Aliasen müssen Sie eine Verbindung mit Ihrem Azure-Konto herstellen. Führen Sie die folgenden Schritte aus, um eine Verbindung mit Azure aus Visual Studio Code herzustellen:

1. Melden Sie sich über die Azure Policy-Erweiterung oder die Befehlspalette bei Azure an.

   - Azure Policy-Erweiterung

     Wählen Sie in der Azure Policy-Erweiterung die Option **Bei Azure anmelden** aus.

     ![Azure Cloud-Anmeldung für Visual Studio Code über die Azure Policy-Erweiterung](../media/extension-for-vscode/azure-cloud-sign-in-policy-extension.png)

   - Befehlspalette

     Navigieren Sie auf der Menüleiste zu **Ansicht** > **Befehlspalette**, und geben Sie **Azure: Anmelden** ein.

     ![Azure Cloud-Anmeldung für Visual Studio Code über die Befehlspalette](../media/extension-for-vscode/azure-cloud-sign-in-command-palette.png)

1. Befolgen Sie die Anleitung für die Anmeldung, um sich bei Azure anzumelden. Nach dem Herstellen der Verbindung wird Ihr Azure-Kontoname unten im Visual Studio Code-Fenster in der Statusleiste angezeigt.

## <a name="select-subscriptions"></a>Auswählen von Abonnements

Bei der ersten Anmeldung werden nur die Ressourcen und Richtlinien des Standardabonnements von der Azure Policy-Erweiterung geladen. Gehen Sie folgendermaßen vor, um Abonnements für die Anzeige von Ressourcen und Richtlinien hinzuzufügen oder zu entfernen:

1. Starten Sie den Abonnementbefehl über die Befehlspalette oder die Fensterfußzeile.

   - Befehlspalette: 

     Navigieren Sie auf der Menüleiste zu **Ansicht** > **Befehlspalette**, und geben Sie **Azure Policy: Abonnements auswählen** ein.

   - Fensterfußzeile

     Wählen Sie in der Fensterfußzeile am unteren Bildschirmrand das Segment aus, das **Azure: \<Ihr Konto\>** entspricht.

1. Verwenden Sie das Filterfeld, um Abonnements schnell nach Namen zu finden. Dann aktivieren bzw. deaktivieren Sie die einzelnen Abonnements, um die von der Azure Policy-Erweiterung angezeigten Abonnements festzulegen. Nachdem Sie Abonnements für die Anzeige hinzugefügt oder entfernt haben, klicken Sie auf **OK**.

## <a name="search-for-and-view-resources"></a>Suchen nach und Anzeigen von Ressourcen

Die Azure Policy-Erweiterung listet Ressourcen in den ausgewählten Abonnements nach Ressourcenanbieter und nach Ressourcengruppe im Bereich **Ressourcen** auf. Die Strukturansicht enthält die folgenden Gruppierungen von Ressourcen innerhalb des ausgewählten Abonnements oder auf Abonnementebene:

- **Ressourcenanbieter**
  - Jeder registrierte Ressourcenanbieter mit Ressourcen und zugehörigen untergeordneten Ressourcen, die Richtlinienaliase aufweisen
- **Ressourcengruppen**
  - Alle Ressourcen nach der Ressourcengruppe, in der sie enthalten sind

Standardmäßig filtert die Erweiterung den Abschnitt „Ressourcenanbieter“ nach vorhandenen Ressourcen und Ressourcen mit Richtlinienaliasen. Ändern Sie dieses Verhalten unter **Einstellungen** > **Erweiterungen** > **Azure Policy**, um alle Ressourcenanbieter ohne Filter anzuzeigen.

Kunden mit Hunderten oder Tausenden von Ressourcen in einem einzelnen Abonnement bevorzugen möglicherweise eine Suchmethode zum Auffinden ihrer Ressourcen. Die Azure Policy-Erweiterung ermöglicht die Suche nach einer bestimmten Ressource mit den folgenden Schritten:

1. Starten Sie die Suchschnittstelle über die Azure Policy-Erweiterung oder die Befehlspalette.

   - Azure Policy-Erweiterung

     Zeigen Sie in der Azure Policy-Erweiterung auf den Bereich **Ressourcen**, und wählen Sie die Auslassungspunkte und dann **Ressourcen suchen** aus.

   - Befehlspalette:

     Navigieren Sie auf der Menüleiste zu **Ansicht** > **Befehlspalette**, und geben Sie **Ressourcen: Ressourcen suchen** ein.

1. Wenn mehr als ein Abonnement für die Anzeige ausgewählt ist, wählen Sie mit dem Filter das zu durchsuchende Abonnement aus.

1. Wählen Sie mit dem Filter aus, welche Ressourcengruppe durchsucht werden soll, die Teil des zuvor ausgewählten Abonnements ist.

1. Wählen Sie mit dem Filter aus, welche Ressource angezeigt werden soll. Der Filter kann sowohl für den Ressourcennamen als auch den Ressourcentyp verwendet werden.

## <a name="discover-aliases-for-resource-properties"></a>Ermitteln von Aliasen für Ressourceneigenschaften

Bei Auswahl einer Ressource (unabhängig davon, ob über die Suchschnittstelle oder durch Auswahl in der Strukturansicht) öffnet die Azure Policy-Erweiterung die JSON-Datei, die diese Ressource und alle zugehörigen Resource Manager-Eigenschaftswerte darstellt.

Sobald eine Ressource geöffnet ist, wird beim Zeigen auf den Namen oder den Wert der Resource Manager-Eigenschaft der Azure Policy-Alias angezeigt (sofern vorhanden). In diesem Beispiel ist die Ressource vom Typ `Microsoft.Compute/virtualMachines`, und es wird auf die Eigenschaft **properties.storageProfile.imageReference.offer** gezeigt. Beim Zeigen werden die entsprechenden Aliase eingeblendet.

![Zeigen in der Azure Policy-Erweiterung zum Einblenden des Resource Manager-Eigenschaftenalias](../media/extension-for-vscode/extension-hover-shows-property-alias.png)

## <a name="search-for-and-view-policies-and-assignments"></a>Suchen nach und Anzeigen von Richtlinien und Zuweisungen

Die Azure Policy-Erweiterung listet Richtlinientypen und Richtlinienzuweisungen in Form einer Strukturansicht für die Abonnements auf, die zur Anzeige im Bereich **Richtlinien** ausgewählt wurden. Kunden mit Hunderten oder Tausenden von Richtlinien oder Zuweisungen in einem einzelnen Abonnement bevorzugen möglicherweise eine Suchmethode zum Auffinden ihrer Richtlinien oder Zuweisungen. Die Azure Policy-Erweiterung ermöglicht die Suche nach einer bestimmten Richtlinie oder Zuweisung mit den folgenden Schritten:

1. Starten Sie die Suchschnittstelle über die Azure Policy-Erweiterung oder die Befehlspalette.

   - Azure Policy-Erweiterung

     Zeigen Sie in der Azure Policy-Erweiterung auf den Bereich **Richtlinien**, und wählen Sie die Auslassungspunkte und dann **Richtlinien suchen** aus.

   - Befehlspalette:

     Navigieren Sie auf der Menüleiste zu **Ansicht** > **Befehlspalette**, und geben Sie **Richtlinien: Richtlinien suchen** ein.

1. Wenn mehr als ein Abonnement für die Anzeige ausgewählt ist, wählen Sie mit dem Filter das zu durchsuchende Abonnement aus.

1. Wählen Sie mit dem Filter aus, welcher Richtlinientyp oder welche Richtlinienzuweisung durchsucht werden soll, die Teil des zuvor ausgewählten Abonnements ist.

1. Wählen Sie mit dem Filter aus, welche Richtlinie angezeigt werden soll. Der Filter kann für _displayName_ für die Richtliniendefinition oder die Richtlinienzuweisung verwendet werden.

Bei Auswahl einer Richtlinie oder Zuweisung (unabhängig davon, ob über die Suchschnittstelle oder durch Auswahl in der Strukturansicht) öffnet die Azure Policy-Erweiterung die JSON-Datei, die diese Richtlinie oder Zuweisung und alle zugehörigen Resource Manager-Eigenschaftswerte darstellt. Die Erweiterung kann das geöffnete Azure Policy-JSON-Schema überprüfen.

## <a name="sign-out"></a>Abmelden

Navigieren Sie auf der Menüleiste zu **Ansicht** > **Befehlspalette**, und geben Sie dann **Azure: Abmelden** ein.

## <a name="next-steps"></a>Nächste Schritte

- Sehen Sie sich die Beispiele unter [Azure Policy-Beispiele](../samples/index.md) an.
- Lesen Sie die Informationen unter [Struktur von Azure Policy-Definitionen](../concepts/definition-structure.md).
- Lesen Sie [Grundlegendes zu Richtlinienauswirkungen](../concepts/effects.md).
- Informieren Sie sich über das [programmgesteuerte Erstellen von Richtlinien](programmatically-create.md).
- Erfahren Sie, wie Sie [nicht konforme Ressourcen korrigieren](remediate-resources.md) können.
- Weitere Informationen zu Verwaltungsgruppen finden Sie unter [Organisieren Ihrer Ressourcen mit Azure-Verwaltungsgruppen](../../management-groups/overview.md).