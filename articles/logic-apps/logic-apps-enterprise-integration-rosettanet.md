---
title: RosettaNet-Nachrichten für die B2B-Integration – Azure Logic Apps
description: Austauschen von RosettaNet-Nachrichten in Azure Logic Apps mit dem Enterprise Integration Pack
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.date: 06/22/2019
ms.openlocfilehash: 570c7907f320b881e2db0bd45cdce311490f4f45
ms.sourcegitcommit: d37991ce965b3ee3c4c7f685871f8bae5b56adfa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "72680330"
---
# <a name="exchange-rosettanet-messages-for-b2b-enterprise-integration-in-azure-logic-apps"></a>Austauschen von RosettaNet-Nachrichten für die B2B-Unternehmensintegration in Azure Logic Apps

[RosettaNet](https://resources.gs1us.org) ist ein Non-Profit-Konsortium, das Standardprozesse zum Teilen von Unternehmensinformationen etabliert hat. Diese Standards sind weitverbreitet in der Halbleiter-, Elektronik- und Logistikindustrie und kommen dort am häufigsten zum Einsatz. Das RosettaNet-Konsortium erstellt und verwaltet sogenannte Partner Interface Processes (PIPs), die geläufige Geschäftsprozessdefinitionen für alle RosettaNet-Nachrichtenwechsel zur Verfügung stellen. RosettaNet basiert auf XML und definiert Nachrichtenrichtlinien, Schnittstellen für Geschäftsprozesse und Implementierungsframeworks für die Kommunikation zwischen Unternehmen.

In [Azure Logic Apps](../logic-apps/logic-apps-overview.md) können Sie mit dem RosettaNet-Connector Integrationslösungen erstellen, die RosettaNet-Standards unterstützen. Der Connector basiert auf dem RosettaNet Implementation Framework 2.0.01 (RNIF). RNIF ist ein offenes Framework für Netzwerkanwendungen, das es Geschäftspartnern ermöglicht, RosettaNet-PIPs gemeinsam auszuführen. Dieses Framework legt die Nachrichtenstruktur, die Notwendigkeit für Bestätigungen, die MIME-Codierung (Multipurpose Internet Mail Extensions) und die digitale Signatur fest.

Der Connector bietet insbesondere folgende Funktionen:

* Codieren oder Empfangen von RosettaNet-Nachrichten
* Decodieren oder Senden von RosettaNet-Nachrichten
* Warten auf Antworten und Generieren von Benachrichtigungen oder Fehlermeldungen

Damit diese Funktionen genutzt werden können, unterstützt der Connector alle PIPs, die durch RNIF 2.0.01 definiert werden. Die Kommunikation mit Partnern kann synchron oder asynchron erfolgen.

## <a name="rosettanet-concepts"></a>RosettaNet-Konzepte

Im Folgenden werden einige Konzepte und Begriffe vorgestellt, die nur für die RosettaNet-Spezifikation gelten und essenziell sind, wenn Sie auf RosettaNet basierende Integrationen erstellen:

* **PIP**

  Die Organisation RosettaNet erstellt und verwaltet sogenannte Partner Interface Processes (PIPs), um geläufige Geschäftsprozessdefinitionen für alle RosettaNet-Nachrichtenwechsel zur Verfügung zu stellen. Jede PIP-Spezifikation enthält eine DTD-Datei (Document Type Definition) und ein Dokument mit Nachrichtenrichtlinien. Die DTD-Datei definiert die Nachrichtenstruktur für Dienstinhalte. Das Dokument mit Nachrichtenrichtlinien ist eine lesbare HTML-Datei, in dem Einschränkungen auf Elementebene definiert werden. Diese Dateien ergeben zusammen eine vollständige Definition des Geschäftsprozesses.

   PIPs werden durch die allgemeine Geschäftsfunktion (Cluster) und eine Unterfunktion (Segment) definiert. So ist „3A4“ beispielsweise der PIP für die Bestellnummer, während „3“ für die Bestellungsverwaltung und „3A“ für die Angebots- und Auftragserfassung steht. Weitere Informationen finden Sie auf der Website von [RosettaNet](https://resources.gs1us.org).

* **Aktion**

  Im Rahmen eines PIP handelt es sich bei Aktionsnachrichten um Geschäftsnachrichten, die zwischen Partnern ausgetauscht werden.

* **Signal**

   Im Rahmen eines PIP handelt es sich bei Signalnachrichten um Bestätigungen, die als Reaktion auf Aktionsnachrichten gesendet werden.

* **Einzelaktion und Doppelaktion**

  Bei einem PIP mit Einzelaktionen besteht die einzige Antwort in Signalnachrichten zur Bestätigung. Bei einem PIP mit Doppelaktionen empfängt der Initiator eine Antwortnachricht und antwortet mit einer Bestätigung, die zusätzlich zum Nachrichtenfluss mit Einzelaktionen gesendet wird.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Wenn Sie noch kein Azure-Abonnement haben, [melden Sie sich für ein kostenloses Azure-Konto an](https://azure.microsoft.com/free/).

* Ein [Integrationskonto](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) zur Speicherung Ihrer Vereinbarung und anderer B2B-Artefakte. Dieses Integrationskonto muss mit Ihrem Azure-Abonnement verknüpft sein.

* Mindestens zwei [Partner](../logic-apps/logic-apps-enterprise-integration-partners.md), die in Ihrem Integrationskonto definiert und mit dem DUNS-Qualifizierer unter **Geschäftsidentitäten** konfiguriert sind

* Eine PIP-Prozesskonfiguration ist erforderlich, um RosettaNet-Nachrichten über Ihr Integrationskonto zu senden oder zu empfangen. Die Prozesskonfiguration speichert alle Eigenschaften der PIP-Konfiguration. Sie können auf diese Konfiguration verweisen, wenn Sie eine Vereinbarung mit dem Partner aushandeln. Weitere Informationen zum Erstellen einer PIP-Prozesskonfiguration in Ihrem Integrationskonto finden Sie unter [Add PIP process configuration (Hinzufügen einer PIP-Prozesskonfiguration)](#add-pip).

* [Zertifikate](../logic-apps/logic-apps-enterprise-integration-certificates.md) für die Verschlüsselung, Entschlüsselung oder Signatur der Nachrichten, die Sie in das Integrationskonto hochladen, sind optional. Sie benötigen diese nur, wenn Sie Signaturen oder Verschlüsselung verwenden.

<a name="add-pip"></a>

## <a name="add-pip-process-configuration"></a>Hinzufügen einer PIP-Prozesskonfiguration

Befolgen Sie diese Schritte, um eine PIP-Prozesskonfiguration zu Ihrem Integrationskonto hinzuzufügen:

1. Suchen Sie im [Azure-Portal](https://portal.azure.com) nach Ihrem Integrationskonto, und öffnen Sie es.

1. Wählen Sie im Bereich **Übersicht** die Kachel **RosettaNet-PIP** aus.

   ![RosettaNet-Kachel auswählen](media/logic-apps-enterprise-integration-rosettanet/select-rosettanet-tile.png)

1. Klicken Sie unter **RosettaNet-PIP** auf **Hinzufügen**. Geben Sie die PIP-Details ein.

   ![Details zum RosettaNet-PIP eingeben](media/logic-apps-enterprise-integration-rosettanet/add-rosettanet-pip.png)

   | Eigenschaft | Erforderlich | BESCHREIBUNG |
   |----------|----------|-------------|
   | **Name** | Ja | Ihr PIP-Name |
   | **PIP-Code** | Ja | Der dreistellige PIP-Code (weitere Informationen unter [RosettaNet-PIPs](https://docs.microsoft.com/biztalk/adapters-and-accelerators/accelerator-rosettanet/rosettanet-pips)) |
   | **PIP-Version** | Ja | Die PIP-Versionsnummer, die anhand des ausgewählten PIP-Codes bestimmt wird |
   ||||

   Weitere Informationen zu diesen PIP-Eigenschaften finden Sie auf der Website von [RosettaNet](https://resources.gs1us.org/RosettaNet-Standards/Standards-Library/PIP-Directory#1043208-pipsreg).

1. Klicken Sie auf **OK**, wenn Sie fertig sind, damit die PIP-Konfiguration erstellt wird.

1. Wählen Sie den PIP aus, und klicken Sie auf **Als JSON bearbeiten**, wenn Sie die Prozesskonfiguration anzeigen oder bearbeiten möchten.

   Alle Einstellungen für die Prozesskonfiguration stammen aus den PIP-Spezifikationen. Logic Apps füllt die meisten Einstellungen mit Standardwerten auf, die am häufigsten für diese Eigenschaften verwendeten werden.

   ![PIP-Konfiguration von RosettaNet bearbeiten](media/logic-apps-enterprise-integration-rosettanet/edit-rosettanet-pip.png)

1. Vergewissern Sie sich, dass die Einstellungen den Werten in der entsprechenden PIP-Spezifikation entsprechen und Ihre geschäftlichen Anforderungen erfüllen. Aktualisieren Sie bei Bedarf die Werte in der JSON-Datei, und speichern Sie die Änderungen.

## <a name="create-rosettanet-agreement"></a>Erstellen einer RosettaNet-Vereinbarung

1. Suchen Sie im [Azure-Portal](https://portal.azure.com) Ihr Integrationskonto, und öffnen Sie es, falls es noch nicht geöffnet ist.

1. Wählen Sie im Bereich **Übersicht** die Kachel **Vereinbarungen** aus.

   ![Kachel „Vereinbarungen“ auswählen](media/logic-apps-enterprise-integration-rosettanet/select-agreement-tile.png)

1. Wählen Sie unter **Vereinbarungen** die Option **Hinzufügen** aus. Geben Sie die Details zur Vereinbarung an.

   ![Details zur Vereinbarung angeben](media/logic-apps-enterprise-integration-rosettanet/add-agreement-details.png)

   | Eigenschaft | Erforderlich | BESCHREIBUNG |
   |----------|----------|-------------|
   | **Name** | Ja | Der Name der Vereinbarung |
   | **Vereinbarungstyp** | Ja | Wählen Sie **RosettaNet** aus. |
   | **Hostpartner** | Ja | Eine Vereinbarung erfordert einen Host- und einen Gastpartner. Der Hostpartner stellt die Organisation dar, die die Vereinbarung konfiguriert. |
   | **Hostidentität** | Ja | Ein Bezeichner für den Hostpartner. |
   | **Gastpartner** | Ja | Eine Vereinbarung erfordert einen Host- und einen Gastpartner. Der Gastpartner stellt die Organisation dar, die Geschäfte mit dem Hostpartner tätigt. |
   | **Gastidentität** | Ja | Ein Bezeichner für den Gastpartner. |
   | **Empfangseinstellungen** | Varies | Diese Eigenschaften gelten für alle Nachrichten, die vom Hostpartner empfangen werden. |
   | **Sendeeinstellungen** | Varies | Diese Eigenschaften gelten für alle Nachrichten, die vom Hostpartner gesendet werden. |  
   | **RosettaNet-PIP-Verweise** | Ja | Die PIP-Verweise für die Vereinbarung. Für alle RosettaNet-Nachrichten sind PIP-Konfigurationen erforderlich. |
   ||||

1. Klicken Sie auf **Empfangseinstellungen**, um eine Vereinbarung für den Empfang eingehender Nachrichten vom Gastpartner einzurichten.

   ![Empfangseinstellungen](media/logic-apps-enterprise-integration-rosettanet/add-agreement-receive-details.png)

   1. Wählen Sie unter **Nachrichten** die Option **Nachricht muss signiert sein** oder **Nachricht muss verschlüsselt sein** aus, um die Signatur oder Verschlüsselung für eingehende Nachrichten zu aktivieren.

      | Eigenschaft | Erforderlich | BESCHREIBUNG |
      |----------|----------|-------------|
      | **Nachricht muss signiert sein** | Nein | Eingehende Nachrichten werden mit dem ausgewählten Zertifikat signiert |
      | **Certificate** | Ja, wenn die Signatur aktiviert ist | Das Zertifikat, das für die Signatur verwendet werden soll |
      | **Nachrichtenverschlüsselung aktivieren** | Nein | Eingehende Nachrichten werden mit dem ausgewählten Zertifikat verschlüsselt |
      | **Certificate** | Ja, wenn die Verschlüsselung aktiviert ist | Das Zertifikat, das für die Verschlüsselung verwendet werden soll |
      ||||

   1. Wählen Sie unter jeder Auswahl das entsprechende [Zertifikat](./logic-apps-enterprise-integration-certificates.md) aus, das Sie zuvor zu Ihrem Integrationskonto hinzugefügt haben und das für die Signatur oder Verschlüsselung verwendet werden soll.

1. Klicken Sie auf **Sendeeinstellungen**, um eine Vereinbarung für das Senden von Nachrichten an Gastpartner einzurichten.

   ![Sendeeinstellungen](media/logic-apps-enterprise-integration-rosettanet/add-agreement-send-details.png)

   1. Wählen Sie unter **Nachrichten** die Option **Nachrichtensignatur aktivieren** oder **Nachrichtenverschlüsselung aktivieren** aus, um die Signatur oder Verschlüsselung für ausgehende Nachrichten zu aktivieren. Wählen Sie unter jeder Auswahl das entsprechende [Zertifikat](./logic-apps-enterprise-integration-certificates.md) oder den entsprechenden Algorithmus aus, das bzw. den Sie zuvor zu Ihrem Integrationskonto hinzugefügt haben und das bzw. der für die Signatur oder Verschlüsselung verwendet werden soll.

      | Eigenschaft | Erforderlich | BESCHREIBUNG |
      |----------|----------|-------------|
      | **Nachrichtensignatur aktivieren** | Nein | Signiert ausgehende Nachrichten mit dem ausgewählten Signaturalgorithmus und -zertifikat |
      | **Signaturalgorithmus** | Ja, wenn die Signatur aktiviert ist | Der Signaturalgorithmus, der auf Grundlage des ausgewählten Zertifikats verwendet werden soll |
      | **Certificate** | Ja, wenn die Signatur aktiviert ist | Das Zertifikat, das für die Signatur verwendet werden soll |
      | **Nachrichtenverschlüsselung aktivieren** | Nein | Verschlüsselt ausgehende Nachrichten mit dem ausgewählten Verschlüsselungsalgorithmus und -zertifikat |
      | **Verschlüsselungsalgorithmus** | Ja, wenn die Verschlüsselung aktiviert ist | Der Verschlüsselungsalgorithmus, der auf Grundlage des ausgewählten Zertifikats verwendet werden soll |
      | **Certificate** | Ja, wenn die Verschlüsselung aktiviert ist | Das Zertifikat, das für die Verschlüsselung verwendet werden soll |
      ||||

   1. Geben Sie unter **Endpunkte** die erforderlichen URLs ein, die für das Senden von Aktionsnachrichten und Bestätigungen verwendet werden sollen.

      | Eigenschaft | Erforderlich | BESCHREIBUNG |
      |----------|----------|-------------|
      | **Aktions-URL** |  Ja | Die URL, die zum Senden von Aktionsnachrichten verwendet werden soll – das Feld ist für synchrone und asynchrone Nachrichten erforderlich. |
      | **Bestätigung-URL** | Ja | Die URL, die zum Senden von Bestätigungsnachrichten verwendet werden soll – das Feld ist nur für asynchrone Nachrichten erforderlich. |
      ||||

1. Klicken Sie auf **RosettaNet-PIP-Verweise**, um eine Vereinbarung mit den RosettaNet-PIP-Verweisen für Partner einzurichten. Wählen Sie unter **PIP-Name** den Namen des zuvor erstellten PIP aus.

   ![PIP-Verweise](media/logic-apps-enterprise-integration-rosettanet/add-agreement-pip-details.png)

   Durch Ihre Auswahl werden die übrigen Eigenschaften aufgefüllt, die von dem PIP abhängen, den Sie in Ihrem Integrationskonto eingerichtet haben. Bei Bedarf können Sie die **PIP-Rolle** ändern.

   ![Ausgewählter PIP](media/logic-apps-enterprise-integration-rosettanet/add-agreement-selected-pip.png)

Wenn Sie diese Schritte durchgeführt haben, können Sie RosettaNet-Nachrichten senden oder empfangen.

## <a name="rosettanet-templates"></a>RosettaNet-Vorlagen

Sie können Vorlagen für Logik-Apps zum Decodieren und Codieren von RosettaNet-Nachrichten verwenden, um die Entwicklung zu beschleunigen und empfohlene Integrationsmuster zu nutzen. Wenn Sie eine Logik-App erstellen, können Sie eine Vorlage aus dem Vorlagenkatalog des Logik-App-Designers auswählen. Sie können diese Vorlagen auch über das [GitHub-Repository für Azure Logic Apps](https://github.com/Azure/logicapps) abrufen.

![RosettaNet-Vorlagen](media/logic-apps-enterprise-integration-rosettanet/decode-encode-rosettanet-templates.png)

## <a name="receive-or-decode-rosettanet-messages"></a>Empfangen oder decodieren von RosettaNet-Nachrichten

1. [Erstellen Sie eine leere Logik-App.](quickstart-create-first-logic-app-workflow.md)

1. [Verknüpfen Sie Ihr Integrationskonto](logic-apps-enterprise-integration-create-integration-account.md#link-account) mit der Logik-App.

1. Bevor Sie eine Aktion für die Decodierung der RosettaNet-Nachricht hinzufügen können, müssen Sie einen Trigger zum Starten der Logik-App (z. B. einen Anforderungstrigger) hinzufügen.

1. Klicken Sie auf **Neuer Schritt**, nachdem Sie den Trigger hinzugefügt haben.

   ![Anforderungstrigger hinzufügen](media/logic-apps-enterprise-integration-rosettanet/request-trigger.png)

1. Geben Sie im Suchfeld „rosettanet“ ein, und wählen Sie diese Aktion aus: **RosettaNet-Decodierung**

   ![Aktion „RosettaNet-Decodierung“ suchen und auswählen](media/logic-apps-enterprise-integration-rosettanet/select-decode-rosettanet-action.png)

1. Geben Sie die Informationen für die Eigenschaften der Aktion ein:

   ![Aktionsdetails angeben](media/logic-apps-enterprise-integration-rosettanet/decode-action-details.png)

   | Eigenschaft | Erforderlich | BESCHREIBUNG |
   |----------|----------|-------------|
   | **Meldung** | Ja | Die zu decodierende RosettaNet-Nachricht  |
   | **Header** | Ja | Die HTTP-Header, die die Werte für die Version (die RNIF-Version) und den Antworttyp (Kommunikationsart zwischen Partnern, kann synchron oder asynchron sein) angeben |
   | **Rolle** | Ja | Die Rolle des Hostpartners im PIP |
   ||||

   Die Ausgabe der Aktion „RosettaNet-Decodierung“ enthält **ausgehende Signale** und andere Eigenschaften, die Sie codieren und an den Partner zurücksenden können. Sie können jedoch auch andere Aktionen für diese Ausgabe durchführen.

## <a name="send-or-encode-rosettanet-messages"></a>Senden oder codieren von RosettaNet-Nachrichten

1. [Erstellen Sie eine leere Logik-App.](quickstart-create-first-logic-app-workflow.md)

1. [Verknüpfen Sie Ihr Integrationskonto](logic-apps-enterprise-integration-create-integration-account.md#link-account) mit der Logik-App.

1. Bevor Sie eine Aktion für die Codierung der RosettaNet-Nachricht hinzufügen können, müssen Sie einen Trigger zum Starten der Logik-App (z. B. einen Anforderungstrigger) hinzufügen.

1. Klicken Sie auf **Neuer Schritt**, nachdem Sie den Trigger hinzugefügt haben.

   ![Anforderungstrigger hinzufügen](media/logic-apps-enterprise-integration-rosettanet/request-trigger.png)

1. Geben Sie im Suchfeld „rosettanet“ ein, und wählen Sie diese Aktion aus: **RosettaNet-Codierung**

   ![Aktion „RosettaNet-Codierung“ suchen und auswählen](media/logic-apps-enterprise-integration-rosettanet/select-encode-rosettanet-action.png)

1. Geben Sie die Informationen für die Eigenschaften der Aktion ein:

   ![Aktionsdetails angeben](media/logic-apps-enterprise-integration-rosettanet/encode-action-details.png)

   | Eigenschaft | Erforderlich | BESCHREIBUNG |
   |----------|----------|-------------|
   | **Meldung** | Ja | Die zu codierende RosettaNet-Nachricht  |
   | **Hostpartner** | Ja | Der Name des Hostpartners |
   | **Gastpartner** | Ja | Der Name des Gastpartners |
   | **PIP-Code** | Ja | Der PIP-Code |
   | **PIP-Version** | Ja | Die PIP-Version |  
   | **Identität der PIP-Instanz** | Ja | Der eindeutige Bezeichner für die PIP-Nachricht |  
   | **Nachrichtentyp** | Ja | Der Nachrichtentyp, der codiert werden soll |  
   | **Rolle** | Ja | Die Rolle des Hostpartners |
   ||||

   Die codierte Nachricht kann nun an den Partner gesendet werden.

1. In diesem Beispiel wird die **HTTP-Aktion** verwendet, um die codierte Nachricht zu senden, die in „HTTP - Send encoded message to partner“ (HTTP – codierte Nachricht an Partner senden) umbenannt wurde.

   ![Hypertext Transfer-Protokoll für das Senden von RosettaNet-Nachrichten](media/logic-apps-enterprise-integration-rosettanet/send-rosettanet-message-to-partner.png)

   Gemäß den RosettaNet-Standards gilt eine Geschäftstransaktion nur dann als vollständig, wenn alle im PIP festgelegten Schritte abgeschlossen sind.

1. Nachdem der Host die codierte Nachricht an den Partner gesendet hat, wartet er auf das Signal und die Bestätigung. Fügen Sie die Aktion **RosettaNet-Wartevorgang auf Antwort** hinzu, damit dieser Task ausgeführt wird.

   ![Aktion „RosettaNet-Wartevorgang auf Antwort“ hinzufügen](media/logic-apps-enterprise-integration-rosettanet/rosettanet-wait-for-response-action.png)

   Die Wartezeit und die Anzahl der Wiederholungen hängen von der PIP-Konfiguration in Ihrem Integrationskonto ab. Wenn keine Antwort empfangen wird, generiert diese Aktion eine Fehlermeldung. Stellen Sie sicher, dass die Aktionen **Codierung** und **Wartevorgang auf Antwort** sich in einer **Bis-Schleife** befinden, damit Wiederholungen ordnungsgemäß verarbeitet werden.

   ![Bis-Schleife mit RosettaNet-Aktionen](media/logic-apps-enterprise-integration-rosettanet/rosettanet-loop.png)

## <a name="next-steps"></a>Nächste Schritte

* In diesem Artikel lernen Sie die Überprüfung, die Transformation und andere Nachrichtenvorgänge im [Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md) kennen.
* Informationen zu anderen [Logic Apps-Connectors](../connectors/apis-list.md)
