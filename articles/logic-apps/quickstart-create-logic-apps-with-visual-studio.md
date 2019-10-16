---
title: Erstellen von automatisierten Workflows mit Visual Studio – Azure Logic Apps
description: Automatisieren von Aufgaben, Geschäftsprozessen und Workflows für die Integration in Unternehmen mit Azure Logic Apps und Visual Studio
services: logic-apps
ms.service: logic-apps
ms.suite: integration
ms.workload: azure-vs
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.manager: carmonm
ms.topic: quickstart
ms.custom: mvc
ms.date: 04/25/2019
ms.openlocfilehash: 47b7609fe111ecbe41a161bfbff1f7225ad66357
ms.sourcegitcommit: aef6040b1321881a7eb21348b4fd5cd6a5a1e8d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2019
ms.locfileid: "72165914"
---
# <a name="quickstart-create-automated-tasks-processes-and-workflows-with-azure-logic-apps---visual-studio"></a>Schnellstart: Erstellen von automatisierten Aufgaben, Prozessen und Workflows mit Azure Logic Apps – Visual Studio

Mit [Azure Logic Apps](../logic-apps/logic-apps-overview.md) und Visual Studio können Sie Workflows zur Automatisierung von Aufgaben und Prozessen für die Integration von Apps, Daten, Systemen und Diensten in Unternehmen und Organisationen erstellen. In diesem Schnellstart wird veranschaulicht, wie Sie diese Workflows entwerfen und erstellen, indem Sie in Visual Studio Logik-Apps erstellen und diese Apps über Azure bereitstellen. Sie können diese Aufgaben zwar auch im Azure-Portal durchführen, aber mit Visual Studio können Sie Ihre Logik-Apps der Quellcodeverwaltung hinzufügen, unterschiedliche Versionen veröffentlichen und Azure Resource Manager-Vorlagen für verschiedene Bereitstellungsumgebungen erstellen.

Wenn Sie mit Azure Logic Apps noch nicht vertraut sind und sich nur über die grundlegenden Konzepte informieren möchten, hilft Ihnen der [Schnellstart zur Erstellung einer Logik-App im Azure-Portal](../logic-apps/quickstart-create-first-logic-app-workflow.md) weiter. Der Logik-App-Designer funktioniert im Azure-Portal und in Visual Studio ähnlich.

In diesem Schnellstart erstellen Sie die gleiche Logik-App wie im Schnellstart zum Azure-Portal, allerdings mit Visual Studio. Mit dieser Logik-App wird der RSS-Feed einer Website überwacht und eine E-Mail für jedes neue Element in diesem Feed gesendet. Ihre fertige Logik-App ähnelt diesem allgemeinen Workflow:

![Allgemeine Übersicht über den Logik-App-Workflow](./media/quickstart-create-logic-apps-with-visual-studio/high-level-workflow-overview.png)

<a name="prerequisites"></a>

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie sich [für ein kostenloses Azure-Konto registrieren](https://azure.microsoft.com/free/).

* Laden Sie diese Tools herunter, und installieren Sie sie, falls sie noch nicht vorhanden sind:

  * [Visual Studio 2019, 2017 oder 2015 – Community Edition oder höher](https://aka.ms/download-visual-studio). 
  In diesem Schnellstart wird Visual Studio Community 2017.

    > [!IMPORTANT]
    > Stellen Sie beim Installieren von Visual Studio 2019 oder 2017 sicher, dass Sie die Workload **Azure-Entwicklung** auswählen.

  * [Microsoft Azure SDK für .NET (2.9.1 oder höher)](https://azure.microsoft.com/downloads/). 
  Weitere Informationen zu [Azure SDK für .NET](https://docs.microsoft.com/dotnet/azure/dotnet-tools?view=azure-dotnet).

  * [Azure PowerShell](https://github.com/Azure/azure-powershell#installation)

  * Azure Logic Apps-Tools für die gewünschte Visual Studio-Version:

    * [Visual Studio 2019](https://aka.ms/download-azure-logic-apps-tools-visual-studio-2019)

    * [Visual Studio 2017](https://aka.ms/download-azure-logic-apps-tools-visual-studio-2017)

    * [Visual Studio 2015](https://aka.ms/download-azure-logic-apps-tools-visual-studio-2015)
  
    Sie können die Azure Logic Apps-Tools entweder direkt vom Visual Studio Marketplace herunterladen und installieren oder sich über das [Installieren dieser Erweiterung aus Visual Studio](https://docs.microsoft.com/visualstudio/ide/finding-and-using-visual-studio-extensions) informieren. 
    Achten Sie darauf, dass Sie Visual Studio nach Abschluss der Installation neu starten.

* Wenn Sie andere Azure-Umgebungen (etwa Azure Government) verwenden möchten, können Sie die Erweiterung [Azure Environment Selector](https://marketplace.visualstudio.com/items?itemName=SteveMichelotti.AzureEnvironmentSelector) (Azure-Umgebungsauswahl) installieren und nutzen, mit der Sie einfacher zwischen Umgebungen wechseln können. Weitere Informationen finden Sie unter [Einführung in die Visual Studio-Erweiterung „Azure Environment Selector“ (Azure-Umgebungsauswahl)](https://devblogs.microsoft.com/azuregov/introducing-the-azure-environment-selector-visual-studio-extension/).

* Internetzugriff bei Verwendung des eingebetteten Logik-App-Designers

  Für den Designer ist eine Internetverbindung zum Erstellen von Ressourcen in Azure und zum Lesen von Eigenschaften und Daten von Connectors in Ihrer Logik-App erforderlich. 
  Für Dynamics CRM Onlineverbindungen prüft der Designer Ihre CRM-Instanz auf standardmäßige und benutzerdefinierte Eigenschaften.

* Ein von Logic Apps unterstütztes E-Mail-Konto, z.B. Office 365 Outlook, Outlook.com oder Gmail. Informationen zu Connectors für andere Anbieter finden Sie in [dieser Liste](https://docs.microsoft.com/connectors/). In diesem Beispiel wird Office 365 Outlook verwendet. Bei Verwendung eines anderen Anbieters sind die Schritte im Großen und Ganzen identisch, aber die Benutzeroberfläche weicht ggf. etwas ab.

<a name="create-resource-group-project"></a>

## <a name="create-azure-resource-group-project"></a>Erstellen eines Azure-Ressourcengruppenprojekts

Erstellen Sie zunächst ein [Azure-Ressourcengruppenprojekt](../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md). Informieren Sie sich über [Azure-Ressourcengruppen und -Ressourcen](../azure-resource-manager/resource-group-overview.md).

1. Starten Sie Visual Studio. Melden Sie sich mit Ihrem Azure-Konto an.

1. Wählen Sie im Menü **Datei** die Option **Neu** > **Projekt**. (Tastatur: STRG+UMSCHALT+N)

   ![Erstellen eines neuen Visual Studio-Projekts](./media/quickstart-create-logic-apps-with-visual-studio/create-new-visual-studio-project.png)

1. Wählen Sie unter **Installiert** die Option **Visual C#** oder **Visual Basic**. Wählen Sie **Cloud** > **Azure-Ressourcengruppe**. Geben Sie Ihrem Projekt einen Namen, z.B.:

   ![Erstellen eines Azure-Ressourcengruppenprojekts](./media/quickstart-create-logic-apps-with-visual-studio/create-azure-cloud-service-project.png)

   > [!NOTE]
   > Ressourcengruppennamen dürfen nur Buchstaben, Ziffern, Punkte (`.`), Unterstriche (`_`), Bindestriche (`-`) und Klammern (`(`, `)`) enthalten, dürfen jedoch nicht auf einen Punkt (`.`) *enden*.
   >
   > Wenn **Cloud** oder **Azure-Ressourcengruppe** nicht angezeigt wird, vergewissern Sie sich, dass das Azure SDK für Visual Studio installiert ist.

   Führen Sie diese Schritte aus, wenn Sie Visual Studio 2019 verwenden:

   1. Wählen Sie im Feld **Neues Projekt erstellen** das Projekt **Azure-Ressourcengruppe** für Visual C# oder Visual Basic aus. Klicken Sie auf **Weiter**.

   1. Geben Sie einen Namen für die gewünschte Azure-Ressourcengruppe und andere Projektinformationen an. Wählen Sie **Erstellen**.

1. Wählen Sie in der Vorlagenliste die Vorlage **Logik-App** aus. Klicken Sie auf **OK**.

   ![Auswählen der Vorlage „Logik-App“ zum Erstellen des Projekts](./media/quickstart-create-logic-apps-with-visual-studio/select-logic-app-template.png)

   Nachdem Visual Studio Ihr Projekt erstellt hat, wird der Projektmappen-Explorer geöffnet, und Ihre Projektmappe wird angezeigt. 
   In Ihrer Projektmappe wird in der Datei **LogicApp.json** nicht nur die Logik-App-Definition gespeichert, sondern sie dient auch als Azure Resource Manager-Vorlage, die Sie für die Bereitstellung verwenden können.

   ![Neue Logik-App-Projektmappe und Bereitstellungsdatei im Projektmappen-Explorer](./media/quickstart-create-logic-apps-with-visual-studio/logic-app-solution-created.png)

## <a name="create-blank-logic-app"></a>Erstellen einer leeren Logik-App

Wenn Sie über Ihr Azure-Ressourcengruppenobjekt verfügen, erstellen Sie Ihre Logik-App mit der Vorlage **Leere Logik-App**.

1. Öffnen Sie im Projektmappen-Explorer das Kontextmenü der Datei **LogicApp.json**. Wählen Sie **Öffnen mit Logik-App-Designer** aus. (Tastatur: STRG+L)

   ![Öffnen der Datei „LogicApp.json“ mit dem Logik-App-Designer](./media/quickstart-create-logic-apps-with-visual-studio/open-logic-app-designer.png)

   > [!TIP]
   > Wenn Sie diesen Befehl in Visual Studio 2019 nicht finden, überprüfen Sie, ob Sie das letzte Update für Visual Studio installiert haben.

   Visual Studio fordert Sie zur Angabe Ihres Azure-Abonnements und einer Azure-Ressourcengruppe auf, um Ressourcen für Ihre Logik-App und Verbindungen zu erstellen und bereitzustellen.

1. Wählen Sie für **Abonnement** Ihr Azure-Abonnement aus. Wählen Sie für **Ressourcengruppe** die Option **Neu erstellen** aus, um eine neue Azure-Ressourcengruppe zu erstellen.

   ![Auswählen von Azure-Abonnement, Ressourcengruppe und Ressourcenstandort](./media/quickstart-create-logic-apps-with-visual-studio/select-azure-subscription-resource-group-location.png)

   | Einstellung | Beispielwert | BESCHREIBUNG |
   | ------- | ------------- | ----------- |
   | Benutzerprofilliste | Contoso <br> jamalhartnett@contoso.com | Standardmäßig das zum Anmelden verwendete Konto |
   | **Abonnement** | Nutzungsbasierte Bezahlung <br> (jamalhartnett@contoso.com) | Name für Ihr Azure-Abonnement und das zugeordnete Konto |
   | **Ressourcengruppe** | MyLogicApp-RG <br> (USA, Westen) | Azure-Ressourcengruppe und Standort zum Speichern und Bereitstellen der Ressourcen Ihrer Logik-App |
   | **Location** | MyLogicApp-RG2 <br> (USA, Westen) | Anderer Standort, falls Sie nicht den Ressourcengruppenstandort verwenden möchten |
   ||||

1. Der Logik-App-Designer öffnet eine Seite mit einem Einführungsvideo und häufig verwendeten Triggern. Scrollen Sie nach unten am Video und den Triggern vorbei zu **Vorlagen**, und wählen Sie **Leere Logik-App** aus.

   ![Auswählen von „Leere Logik-App“](./media/quickstart-create-logic-apps-with-visual-studio/choose-blank-logic-app-template.png)

## <a name="build-logic-app-workflow"></a>Erstellen eines Logik-App-Workflows

Fügen Sie als Nächstes einen RSS-[Trigger](../logic-apps/logic-apps-overview.md#logic-app-concepts) hinzu, der bei einem neuen Feedelement ausgelöst wird. Jede Logik-App beginnt mit einem Trigger, der ausgelöst wird, wenn bestimmte Kriterien erfüllt sind. Bei jeder Auslösung des Triggers erstellt die Logic Apps-Engine eine Logik-App-Instanz zur Ausführung Ihres Workflows.

1. Wählen Sie im Logik-App-Designer im Suchfeld **Alle** aus.
Geben Sie im Suchfeld „rss“ ein. Wählen Sie in der Triggerliste den folgenden Trigger aus: **Beim Veröffentlichen eines Feedelements: RSS**

   ![Erstellen Ihrer Logik-App durch das Hinzufügen eines Triggers und von Aktionen](./media/quickstart-create-logic-apps-with-visual-studio/add-trigger-logic-app.png)

1. Sobald der Trigger im Designer angezeigt wird, führen Sie zum Abschließen der Logik-App-Erstellung die Workflowschritte im [Schnellstart für das Azure-Portal](../logic-apps/quickstart-create-first-logic-app-workflow.md#add-rss-trigger) aus, und kehren Sie dann zu diesem Artikel zurück. Anschließend sieht Ihre Logik-App in etwa wie im folgenden Beispiel aus:

   ![Fertiges Beispiel für Logik-App-Workflow](./media/quickstart-create-logic-apps-with-visual-studio/finished-logic-app-workflow.png)

1. Speichern Sie Ihre Visual Studio-Projektmappe. (Tastatur: STRG+S)

<a name="deploy-to-Azure"></a>

## <a name="deploy-logic-app-to-azure"></a>Bereitstellen der Logik-App in Azure

Bevor Sie Ihre Logik-App ausführen und testen können, müssen Sie sie über Visual Studio in Azure bereitstellen.

1. Wählen Sie im Projektmappen-Explorer im Kontextmenü Ihres Projekts die Option **Bereitstellen** > **Neu**. Melden Sie sich nach Aufforderung mit Ihrem Azure-Konto an.

   ![Erstellen einer neuen Logik-App-Bereitstellung](./media/quickstart-create-logic-apps-with-visual-studio/create-logic-app-deployment.png)

1. Behalten Sie für diese Bereitstellung das Azure-Standardabonnement, die Ressourcengruppe und andere Einstellungen bei. Wählen Sie **Bereitstellen** aus.

   ![Bereitstellen der Logik-App in der Azure-Ressourcengruppe](./media/quickstart-create-logic-apps-with-visual-studio/select-azure-subscription-resource-group-deployment.png)

1. Wenn das Feld **Parameter bearbeiten** angezeigt wird, geben Sie einen Ressourcennamen für Ihre Logik-App an. Speichern Sie die Einstellungen.

   ![Angeben des Bereitstellungsnamens für die Logik-App](./media/quickstart-create-logic-apps-with-visual-studio/edit-parameters-deployment.png)

   Wenn die Bereitstellung gestartet wird, wird der Bereitstellungsstatus Ihrer App im Fenster **Ausgabe** von Visual Studio angezeigt. Falls der Status nicht angezeigt wird, können Sie die Liste **Ausgabe anzeigen von** öffnen und Ihre Azure-Ressourcengruppe auswählen.

   ![Bereitstellungsstatus im Visual Studio-Ausgabefenster](./media/quickstart-create-logic-apps-with-visual-studio/logic-app-output-window.png)

   Wenn Ihre ausgewählten Connectors eine Eingabe von Ihnen benötigen, wird im Hintergrund ein PowerShell-Fenster geöffnet, das zur Eingabe von erforderlichen Kennwörtern oder geheimen Schlüsseln auffordert. Nachdem Sie diese Informationen eingegeben haben, wird die Bereitstellung fortgesetzt.

   ![PowerShell-Eingabeaufforderung für Kennwörter oder geheime Schlüssel](./media/quickstart-create-logic-apps-with-visual-studio/logic-apps-powershell-window.png)

   Nach Abschluss der Bereitstellung befindet sich Ihre Logik-App im Azure-Portal im Livezustand und wird gemäß Ihrem angegebenen Zeitplan (jede Minute) ausgeführt. Wenn der Trigger neue Feedelemente erkennt, wird er ausgelöst, sodass eine Workflowinstanz erstellt wird, welche die Aktionen Ihrer Logik-App ausführt. Ihre Logik-App sendet für jedes neue Element eine E-Mail. Falls der Trigger keine neuen Elemente findet, wird er nicht ausgelöst und überspringt das Instanziieren des Workflows. Dann wartet Ihre Logik mit einer erneuten Prüfung bis zum nächsten Intervall.

   Hier sind Beispiele für E-Mails angegeben, die von dieser Logik-App gesendet werden. 
   Überprüfen Sie Ihren Ordner mit den Junk-E-Mails, falls Sie keine E-Mails erhalten.

   ![Outlook sendet E-Mails für jedes neue RSS-Element.](./media/quickstart-create-logic-apps-with-visual-studio/example-outlook-email.png)

Glückwunsch! Sie haben den Buildvorgang und die Bereitstellung Ihrer Logik-App mit Visual Studio erfolgreich durchgeführt. Informationen zur Verwaltung Ihrer Logik-App und des dazugehörigen Ausführungsverlaufs finden Sie unter [Verwalten von Logik-Apps mit Visual Studio](../logic-apps/manage-logic-apps-with-visual-studio.md).

## <a name="add-new-logic-app"></a>Hinzufügen einer neuen Logik-App

Wenn Sie über ein vorhandenes Azure-Ressourcengruppenprojekt verfügen, können Sie eine leere Logik-App im Fenster mit der JSON-Gliederung dem Projekt hinzufügen.

1. Öffnen Sie im Projektmappen-Explorer die Datei `<logic-app-name>.json`.

1. Wählen Sie im Menü **Ansicht** die Option **Weitere Fenster** > **JSON-Gliederung** aus.

1. Wählen Sie am oberen Rand des Fensters mit der JSON-Gliederung die Option **Ressource hinzufügen** aus, um der Vorlagendatei eine Ressource hinzuzufügen. Klicken Sie im Fenster mit der JSON-Gliederung mit der rechten Maustaste auf **Ressourcen**, und wählen Sie **Neue Ressource hinzufügen** aus.

   ![Hinzufügen neuer Ressource im JSON-Gliederungsfenster](./media/quickstart-create-logic-apps-with-visual-studio/json-outline-window-add-resource.png)

1. Wählen Sie im Dialogfeld **Ressource hinzufügen** die Option **Logik-App** aus. Benennen Sie Ihre Logik-App, und wählen Sie **Hinzufügen** aus.

   ![Hinzufügen einer neuen Logik-App-Ressource zum Projekt](./media/quickstart-create-logic-apps-with-visual-studio/add-logic-app-resource.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie mit Ihrer Logik-App fertig sind, löschen Sie die Ressourcengruppe mit Ihrer Logik-App und den dazugehörigen Ressourcen.

1. Melden Sie sich am [Azure-Portal](https://portal.azure.com) mit demselben Konto an, das Sie zum Erstellen Ihrer Logik-App verwendet haben.

1. Wählen Sie im Azure-Hauptmenü die Option **Ressourcengruppen**.
Wählen Sie die Ressourcengruppe Ihrer Logik-App und dann **Übersicht** aus.

1. Wählen Sie auf der Seite **Übersicht** die Option **Ressourcengruppe löschen** aus. Geben Sie zur Bestätigung den Ressourcengruppennamen ein, und klicken Sie auf **Löschen**.

   ![Löschen der Ressourcengruppe für die Logik-App](./media/quickstart-create-logic-apps-with-visual-studio/delete-resource-group.png)

1. Löschen Sie die Visual Studio-Projektmappe von Ihrem lokalen Computer.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie Ihre Logik-App mit Visual Studio erstellt, bereitgestellt und ausgeführt. Die folgenden Artikel enthalten Informationen zur Verwaltung und Durchführung der erweiterten Bereitstellung für Logik-Apps mit Visual Studio:

> [!div class="nextstepaction"]
> * [Verwalten von Logik-Apps mit Visual Studio](../logic-apps/manage-logic-apps-with-visual-studio.md)
