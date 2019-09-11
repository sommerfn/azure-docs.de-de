---
title: 'Herstellen einer Verbindung zu SAP-Systemen: Azure Logic Apps'
description: Zugreifen auf und Verwalten von SAP-Ressourcen durch das Automatisieren von Workflows mit Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: divswa, LADocs
ms.topic: article
ms.date: 08/30/2019
tags: connectors
ms.openlocfilehash: 8712af60df2454b29c0691602260c8b826eae75c
ms.sourcegitcommit: 19a821fc95da830437873d9d8e6626ffc5e0e9d6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/29/2019
ms.locfileid: "70164983"
---
# <a name="connect-to-sap-systems-from-azure-logic-apps"></a>Herstellen einer Verbindung zu SAP-Systemen: Azure Logic Apps

> [!IMPORTANT]
> Die älteren Connectors für SAP-Anwendungsserver und SAP-Nachrichtenserver sind als veraltet markiert. Der aktuelle SAP-Connector fasst diese älteren SAP-Connectors zusammen, damit Sie den Verbindungstyp nicht ändern müssen. Darüber hinaus ist er vollständig mit vorherigen Connectors kompatibel, bietet zahlreiche Zusatzfunktionen und verwendet weiterhin die SAP-.NET-Connectorbibliothek (SAP NCo).
>
> Für Logik-Apps, die die älteren Connectors verwenden, muss vor dem Veraltungsdatum eine [Migration zum neuesten Connector](#migrate) durchgeführt werden. Andernfalls treten bei diesen Logik-Apps Ausführungsfehler auf, und es können keine Nachrichten an Ihr SAP-System gesendet werden.

In diesem Artikel wird gezeigt, wie Sie aus einer Logik-App auf Ihre lokalen SAP-Ressourcen zugreifen, indem Sie den SAP-Connector verwenden. Der Connector arbeitet mit den klassischen SAP-Releases wie R/3- und ECC-Systemen lokal zusammen. Der Connector ermöglicht auch die Integration mit den neueren HANA-basierten SAP-Systemen wie S/4 HANA, unabhängig davon, ob sie lokal oder in der Cloud gehostet werden. Der SAP-Connector unterstützt die Nachrichten- oder Datenintegration in SAP NetWeaver-basierten Systemen über Intermediate Document (IDoc), Business Application Programming Interface (BAPI) oder Remote Function Call (RFC).

Der SAP-Connector verwendet die [SAP-NCo-Bibliothek (.NET-Connector)](https://support.sap.com/en/product/connectors/msnet.html) und stellt diese Vorgänge oder Aktionen bereit:

* **An SAP senden:** Senden von IDoc über tRFC, Abrufen von BAPI-Funktionen über RFC und Abrufen von RFC/tRFC in SAP-Systemen
* **Von SAP empfangen:** Empfangen von IDoc über tRFC, Abrufen von BAPI-Funktionen über tRFC und Abrufen von RFC/tRFC in SAP-Systemen
* **Schemas generieren:** Generieren von Schemas für die SAP-Artefakte für IDoc, BAPI oder RFC.

Für diese Vorgänge unterstützt der SAP-Connector die Basisauthentifizierung über Benutzername und Kennwort. Der Connector unterstützt auch [Secure Network Communications (SNC)](https://help.sap.com/doc/saphelp_nw70/7.0.31/e6/56f466e99a11d1a5b00000e835363f/content.htm?no_cache=true). SNC kann für SAP NetWeaver-SSO (einmaliges Anmelden) oder für zusätzliche Sicherheitsfunktionen eines externen Sicherheitsprodukts verwendet werden.

Der SAP-Connector kann über das [lokale Datengateway](../logic-apps/logic-apps-gateway-connection.md) in lokale SAP-Systeme integriert werden. In Sendeszenarios, in denen beispielsweise eine Nachricht von Logik-Apps an ein SAP-System gesendet wird, agiert das Datengateway als RFC-Client und leitet die von der Logik-App erhaltenen Anforderungen an SAP weiter. Ebenso fungiert das Datengateway in Empfangsszenarios als RFC-Server, der Anforderungen von SAP empfängt und an die Logik-App weiterleitet.

In diesem Artikel wird das Erstellen von in SAP integrierbaren Beispiel-Logik-Apps gezeigt. Gleichzeitig werden die zuvor beschriebenen Integrationsszenarien veranschaulicht. Für Logik-Apps, die die älteren SAP-Connectors verwenden, wird in diesem Artikel erläutert, wie Sie Ihre Logik-Apps zum neuesten SAP-Connector migrieren.

<a name="pre-reqs"></a>

## <a name="prerequisites"></a>Voraussetzungen

Um diesem Artikel weiter folgen zu können, benötigen Sie Folgendes:

* Ein Azure-Abonnement. Wenn Sie noch kein Azure-Abonnement haben, [melden Sie sich für ein kostenloses Azure-Konto an](https://azure.microsoft.com/free/).

* Die Logik-App, von der aus Sie auf Ihr SAP-System zugreifen möchten, und einen Trigger, der Ihren Logik-App-Workflow startet. Falls Sie noch nicht mit Logik-Apps vertraut sind, finden Sie weitere Informationen unter [Was ist Azure Logic Apps?](../logic-apps/logic-apps-overview.md) und [Schnellstart: Erstellen Ihres ersten automatisierten Workflows mit Azure Logic Apps – Azure-Portal](../logic-apps/quickstart-create-first-logic-app-workflow.md).

* Ihr [SAP-Anwendungsserver](https://wiki.scn.sap.com/wiki/display/ABAP/ABAP+Application+Server) oder der [SAP-Nachrichtenserver](https://help.sap.com/saphelp_nw70/helpdata/en/40/c235c15ab7468bb31599cc759179ef/frameset.htm)

* Laden Sie das neueste [lokalen Datengateway](https://www.microsoft.com/download/details.aspx?id=53127) auf einem lokalen Computer herunter, und installieren Sie es. Bevor Sie fortfahren, sollten Sie prüfen, ob Ihr Gateway im Azure-Portal eingerichtet ist. Das Gateway hilft Ihnen, sicher auf lokale Daten und Ressourcen zuzugreifen. Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit einem lokalen SAP-System in Logik-Apps mit dem SAP-Connector](../logic-apps/logic-apps-gateway-install.md).

* Stellen Sie sicher, dass das Gateway als Benutzer ausgeführt wird, der dem SAP-Benutzer zugeordnet ist, wenn Sie SNC mit SSO (Einmaliges Anmelden) verwenden. Wählen Sie **Konto ändern** aus, und geben Sie die Anmeldeinformationen des Benutzers ein, um das Standardkonto zu ändern.

  ![Ändern des Gatewaykontos](./media/logic-apps-using-sap-connector/gateway-account.png)

* Wenn Sie SNC mit einem externen Sicherheitsprodukt aktivieren, kopieren Sie die SNC-Bibliothek oder -Dateien auf den gleichen Computer, auf dem das Gateway installiert ist. Einige Beispiele für SNC-Produkte sind [sapseculib](https://help.sap.com/saphelp_nw74/helpdata/en/7a/0755dc6ef84f76890a77ad6eb13b13/frameset.htm), Kerberos und NTLM.

* Laden Sie die neueste SAP-Clientbibliothek herunter (aktuell [SAP-Connector (NCo 3.0) für Microsoft .NET 3.0.22.0, kompiliert mit .NET Framework 4.0 – 64-Bit-Version von Windows (x64)](https://softwaredownloads.sap.com/file/0020000001000932019)), und installieren Sie sie auf dem Computer, auf dem sich auch das lokale Datengateway befindet. Installieren Sie diese Version oder eine höhere aus folgenden Gründen:

  * Bei früheren SAP-NCo-Versionen konnte es zu einem Deadlock kommen, wenn mehrere IDoc-Nachrichten gleichzeitig gesendet werden. Dadurch werden auch alle späteren Nachrichten blockiert, die an das SAP-Ziel gesendet werden, und es kommt zu einem Timeout der Nachricht.
  
  * Über das lokale Datengateway können nur 64-Bit-Systemen ausgeführt werden. Andernfalls erhalten Sie einen Fehler „Ungültige Image“, weil der Gateway-Hostdienst 32-Bit-Assemblys nicht unterstützt.
  
  * Sowohl der Datengateway-Hostdienst als auch der Microsoft-SAP-Adapter verwenden .NET Framework 4.5. Der SAP .NET-Connector für .NET Framework 4.0 funktioniert mit Prozessen, die .NET Common Language Runtime 4.0 bis 4.7.1 verwenden. Der SAP .NET-Connector für .NET Framework 2.0 funktioniert mit Prozessen, die .NET Runtime 2.0 bis 3.5 verwenden. Er kann jedoch nicht mehr mit dem aktuellen lokalen Datengateway verwendet werden.

* Nachrichteninhalte, die Sie an Ihren SAP-Server senden können, z. B. eine IDoc-Beispieldatei, müssen im XML-Format vorliegen und den Namespace für die SAP-Aktion enthalten, die Sie verwenden möchten.

<a name="migrate"></a>

## <a name="migrate-to-current-connector"></a>Migrieren zum aktuellen Connector

1. Aktualisieren Sie bei Bedarf Ihr [lokales Datengateway](https://www.microsoft.com/download/details.aspx?id=53127), damit Sie über die neueste Version verfügen. Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit einem lokalen SAP-System in Logik-Apps mit dem SAP-Connector](../logic-apps/logic-apps-gateway-install.md).

1. Löschen Sie in der Logik-App, die den älteren SAP-Connector verwendet, die Aktion **Send to SAP** (An SAP senden).

1. Fügen Sie über den aktuellen SAP-Connector die Aktion **Send to SAP** (An SAP senden) hinzu. Diese Aktion kann erst nach erneuter Erstellung der Verbindung mit Ihrem SAP-System verwendet werden.

1. Wenn Sie fertig sind, speichern Sie Ihre Logik-App.

<a name="add-trigger"></a>

## <a name="send-to-sap"></a>Senden an SAP

In diesem Beispiel wird eine Logik-App verwendet, die Sie mit einer HTTP-Anforderung auslösen können. Die Logik-App sendet eine IDoc-Datei an einen SAP-Server und gibt eine Antwort an den Anforderer zurück, der die Logik-App aufgerufen hat.

### <a name="add-an-http-request-trigger"></a>Hinzufügen eines HTTP-Anforderungstriggers

In Azure Logic Apps muss jede Logik-App mit einem [Trigger](../logic-apps/logic-apps-overview.md#logic-app-concepts) beginnen, der ausgelöst wird, wenn ein bestimmtes Ereignis eintritt oder eine bestimmte Bedingung erfüllt wird. Bei jeder Auslösung des Triggers erstellt das Logic Apps-Modul eine Logik-App-Instanz und startet die Ausführung des Workflows Ihrer App.

In diesem Beispiel erstellen Sie eine Logik-App mit einem Endpunkt in Azure, damit Sie *HTTP POST-Anforderungen* an Ihre Logik-App senden können. Wenn Ihre Logik-App diese HTTP-Anforderungen empfängt, wird der Trigger ausgelöst und führt den nächsten Schritt im Workflow aus.

1. Erstellen Sie im [Azure-Portal](https://portal.azure.com) eine leere Logik-App, die den Logik-App-Designer öffnet.

1. Geben Sie im Suchfeld den Begriff „http request“ als Filter ein. Wählen Sie in der Liste der **Trigger** die Option **Beim Empfang einer HTTP-Anforderung** aus.

   ![Hinzufügen eines HTTP-Anforderungstriggers](./media/logic-apps-using-sap-connector/add-trigger.png)

1. Speichern Sie jetzt Ihre Logik-App, damit Sie eine Endpunkt-URL dafür erstellen können. Wählen Sie auf der Symbolleiste des Designers **Speichern** aus.

   Die Endpunkt-URL wird nun im Trigger angezeigt, z.B.:

   ![URL für den Endpunkt generieren](./media/logic-apps-using-sap-connector/generate-http-endpoint-url.png)

<a name="add-action"></a>

### <a name="add-an-sap-action"></a>Hinzufügen einer SAP-Aktion

In Azure Logic Apps handelt es sich bei einer [Aktion](../logic-apps/logic-apps-overview.md#logic-app-concepts) um einen Schritt in Ihrem Workflow, der einem Trigger oder einer anderen Aktion folgt. Wenn Sie Ihrer Logik-App noch keinen Trigger hinzugefügt haben und mit diesem Beispiel weitermachen möchten, [fügen Sie den Trigger wie in diesem Abschnitt beschrieben hinzu](#add-trigger).

1. Wählen Sie im Logik-App-Designer unter dem Trigger die Option **Neuer Schritt** aus.

   ![Wählen Sie „Neuer Schritt“ aus.](./media/logic-apps-using-sap-connector/add-action.png)

1. Geben Sie im Suchfeld den Begriff „sap“ als Filter ein. Wählen Sie in der Liste **Aktionen** die Option **Nachricht an SAP senden** aus.
  
   ![SAP-Sendeaktion auswählen](media/logic-apps-using-sap-connector/select-sap-send-action.png)

   Anstelle der Suchaktion können Sie alternativ die Registerkarte **Unternehmen** und dann die SAP-Aktion auswählen.

   ![SAP-Sendeaktion auf der Registerkarte „Unternehmen“ auswählen](media/logic-apps-using-sap-connector/select-sap-send-action-ent-tab.png)

1. Wenn Sie zur Angabe von Verbindungsdetails aufgefordert werden, erstellen Sie jetzt Ihre SAP-Verbindung. Sollte die Verbindung bereits bestehen, fahren Sie mit dem nächsten Schritt fort, damit Sie Ihre SAP-Aktion einrichten können.

   **Erstellen einer lokalen SAP-Verbindung**

   Stellen Sie die Verbindungsinformationen für Ihren SAP-Server bereit. Wählen Sie für die Eigenschaft **Datengateway** das Datengateway aus, das Sie im Azure-Portal für Ihre Gatewayinstallation erstellt haben. Wählen Sie **Erstellen** aus, wenn Sie fertig sind. Die Verbindung wird in Logic Apps eingerichtet und getestet, sodass sichergestellt ist, dass sie ordnungsgemäß funktioniert.

   * Wenn die Eigenschaft **Anmeldetyp** auf **Anwendungsserver** festgelegt ist, sind diese (in der Regel optionalen) Eigenschaften erforderlich:

     ![Verbindung zum SAP-Anwendungsserver herstellen](media/logic-apps-using-sap-connector/create-SAP-application-server-connection.png)

   * Wenn die Eigenschaft **Anmeldetyp** auf **Gruppe** festgelegt ist, sind diese (in der Regel optionalen) Eigenschaften erforderlich:

     ![Verbindung zum SAP-Nachrichtenserver herstellen](media/logic-apps-using-sap-connector/create-SAP-message-server-connection.png)

   Standardmäßig wird starke Typisierung verwendet, um nach ungültigen Werten zu suchen, indem eine XML-Validierung anhand des Schemas durchgeführt wird. Durch dieses Verhalten können Sie Probleme früher feststellen. Die Option **Sichere Typisierung** ist aus Gründen der Abwärtskompatibilität verfügbar und überprüft nur die Länge der Zeichenfolge. Erfahren Sie mehr über die Option [Sichere Typisierung](#safe-typing).

1. Suchen Sie nun eine Aktion für Ihren SAP-Server, und wählen Sie sie aus.

   1. Wählen Sie im Feld **SAP-Aktion** das Ordnersymbol aus. Suchen Sie in der Dateiliste nach der gewünschten SAP-Nachricht, und wählen Sie sie aus. Verwenden Sie die Pfeile zum Durchlaufen der Liste.

      In diesem Beispiel wird ein IDoc mit dem Typ **Aufträge** ausgewählt.

      ![Aktion „IDoc“ suchen und auswählen](./media/logic-apps-using-sap-connector/SAP-app-server-find-action.png)

      Wenn Sie die Aktion, die Sie verwenden möchten, nicht finden können, können Sie einen Pfad auch manuell eingeben:

      ![Pfad zur Aktion „IDoc“ manuell bereitstellen](./media/logic-apps-using-sap-connector/SAP-app-server-manually-enter-action.png)

      > [!TIP]
      > Geben Sie den Wert für die **SAP-Aktion** über den Ausdrucks-Editor ein. Auf diese Weise können Sie dieselbe Aktion für verschiedene Nachrichtentypen verwenden.

      Weitere Informationen zu IDoc-Vorgängen finden Sie unter [Nachrichtenschemas für IDOC-Vorgänge](https://docs.microsoft.com/biztalk/adapters-and-accelerators/adapter-sap/message-schemas-for-idoc-operations).

   1. Klicken Sie in das Feld **Input Message** (Eingabenachricht), damit die dynamische Inhaltsliste angezeigt wird. Klicken Sie in dieser Liste unter **Beim Empfang einer HTTP-Anforderung** auf das Feld **Hauptteil**.

      Dadurch wird der Hauptteil des HTTP-Anforderungstriggers eingefügt und diese Ausgabe an Ihren SAP-Server gesendet.

      ![Feld „Hauptteil“ auswählen](./media/logic-apps-using-sap-connector/SAP-app-server-action-select-body.png)

      Ihre SAP-Aktion sieht dann wie im folgenden Beispiel aus:

      ![Vollständige SAP-Aktion](./media/logic-apps-using-sap-connector/SAP-app-server-complete-action.png)

1. Speichern Sie Ihre Logik-App. Wählen Sie auf der Symbolleiste des Designers **Speichern** aus.

<a name="add-response"></a>

### <a name="add-an-http-response-action"></a>Hinzufügen einer HTTP-Antwortaktion

Fügen Sie nun dem Workflow Ihrer Logik-App eine Antwortaktion hinzu, und fügen Sie die Ausgabe der SAP-Aktion ein. So gibt die Logik-App die Ergebnisse Ihres SAP-Servers an den ursprünglichen Anforderer zurück.

1. Wählen Sie im Logik-App-Designer unter der SAP-Aktion die Option **Neuer Schritt** aus.

1. Geben Sie im Suchfeld den Begriff „Antwort“ als Filter ein. Wählen Sie in der Liste **Aktionen** die Option **Antwort** aus.

1. Klicken Sie in das Feld **Hauptteil**, damit die dynamische Inhaltsliste angezeigt wird. Wählen Sie aus dieser Liste unter **Send message to SAP** (Nachricht an SAP senden) das Feld **Hauptteil** aus.

   ![Vollständige SAP-Aktion](./media/logic-apps-using-sap-connector/select-sap-body-for-response-action.png)

1. Speichern Sie Ihre Logik-App.

### <a name="test-your-logic-app"></a>Testen Ihrer Logik-App

1. Wenn Ihre Logik-App nicht bereits aktiviert ist, wählen Sie im Menü der Logik-App die Option **Überblick** aus. Wählen Sie auf der Symbolleiste **Aktivieren** aus.

1. Wählen Sie auf der Symbolleiste des Designers **Ausführen** aus. Dadurch wird die Logik-App manuell gestartet.

1. Lösen Sie Ihre Logik-App aus, indem Sie eine HTTP POST-Anforderung an die URL im HTTP-Anforderungstrigger senden.
Fügen Sie den Nachrichteninhalt in Ihre Anforderung ein. Um die Anforderung zu senden, können Sie ein Tool wie [Postman](https://www.getpostman.com/apps) nutzen.

   In diesem Artikel sendet die Anforderung eine IDoc-Datei, die im XML-Format vorliegen muss, und enthält den Namespace der SAP-Aktion, die Sie verwenden möchten, z.B.:

   ``` xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <Send xmlns="http://Microsoft.LobServices.Sap/2007/03/Idoc/2/ORDERS05//720/Send">
      <idocData>
         <...>
      </idocData>
   </Send>
   ```

1. Nachdem Sie die HTTP-Anforderung gesendet haben, warten Sie auf die Antwort der Logik-App.

   > [!NOTE]
   > Es kann ein Timeout auftreten, wenn nicht alle für die Antwort erforderlichen Schritte innerhalb des [Timeoutlimits der Anforderung](./logic-apps-limits-and-config.md) abgeschlossen wurden. Wenn dies passiert, können Anforderungen blockiert werden. Um die Problemdiagnose zu vereinfachen, sollten Sie sich mit dem [Überprüfen und Überwachen Ihrer Logik-Apps](../logic-apps/logic-apps-monitor-your-logic-apps.md) vertraut machen.

Sie haben eine Logik-App erstellt, die mit dem SAP-Server kommunizieren kann. Da Sie eine SAP-Verbindung für die Logik-App eingerichtet haben, können Sie nun andere SAP Aktionen kennenlernen, z.B. BAPI und RFC.

## <a name="receive-from-sap"></a>Empfangen von SAP

Dieses Beispiel verwendet eine Logik-App, die bei Empfang einer Nachricht von einem SAP-System ausgelöst wird.

### <a name="add-an-sap-trigger"></a>Hinzufügen eines SAP-Triggers

1. Erstellen Sie im Azure-Portal eine leere Logik-App, die den Logik-App-Designer öffnet.

1. Geben Sie im Suchfeld den Begriff „sap“ als Filter ein. Wählen Sie in der Liste der **Trigger** die Option **Beim Empfang einer Nachricht von SAP** aus.

   ![Hinzufügen eines SAP-Triggers](./media/logic-apps-using-sap-connector/add-sap-trigger.png)

   Alternativ können Sie zur Registerkarte **Unternehmen** wechseln und folgenden Trigger auswählen:

   ![Hinzufügen eines SAP-Triggers auf der Registerkarte „Unternehmen“](./media/logic-apps-using-sap-connector/add-sap-trigger-ent-tab.png)

1. Wenn Sie zur Angabe von Verbindungsdetails aufgefordert werden, erstellen Sie jetzt Ihre SAP-Verbindung. Sollte die Verbindung bereits bestehen, fahren Sie mit dem nächsten Schritt fort, damit Sie Ihre SAP-Aktion einrichten können.

   **Erstellen einer lokalen SAP-Verbindung**

   Stellen Sie die Verbindungsinformationen für Ihren SAP-Server bereit. Wählen Sie für die Eigenschaft **Datengateway** das Datengateway aus, das Sie im Azure-Portal für Ihre Gatewayinstallation erstellt haben. Wählen Sie **Erstellen** aus, wenn Sie fertig sind. Die Verbindung wird in Logic Apps eingerichtet und getestet, sodass sichergestellt ist, dass sie ordnungsgemäß funktioniert.

   * Wenn die Eigenschaft **Anmeldetyp** auf **Anwendungsserver** festgelegt ist, sind diese (in der Regel optionalen) Eigenschaften erforderlich:

     ![Verbindung zum SAP-Anwendungsserver herstellen](media/logic-apps-using-sap-connector/create-SAP-application-server-connection.png)

   * Wenn die Eigenschaft **Anmeldetyp** auf **Gruppe** festgelegt ist, sind diese (in der Regel optionalen) Eigenschaften erforderlich:

     ![Verbindung zum SAP-Nachrichtenserver herstellen](media/logic-apps-using-sap-connector/create-SAP-message-server-connection.png)  

   Standardmäßig wird starke Typisierung verwendet, um nach ungültigen Werten zu suchen, indem eine XML-Validierung anhand des Schemas durchgeführt wird. Durch dieses Verhalten können Sie Probleme früher feststellen. Die Option **Sichere Typisierung** ist aus Gründen der Abwärtskompatibilität verfügbar und überprüft nur die Länge der Zeichenfolge. Erfahren Sie mehr über die Option [Sichere Typisierung](#safe-typing).

1. Geben Sie die erforderlichen Parameter basierend auf der Konfiguration Ihres SAP-Systems an.

   Optional können Sie eine oder mehrere SAP-Aktionen angeben. Diese Liste von Aktionen gibt die Nachrichten an, die der Trigger über das Datengateway von Ihrem SAP-Server empfängt. Eine leere Liste gibt an, dass der Trigger alle Nachrichten empfängt. Wenn die Liste mehrere Nachrichten enthält, empfängt der Trigger nur die Nachrichten, die in der Liste angegeben wurden. Alle anderen von Ihrem SAP-Server gesendeten Nachrichten werden vom Gateway abgelehnt.

   Sie können eine SAP-Aktion aus der Dateiauswahl auswählen:

   ![SAP-Aktion auswählen](media/logic-apps-using-sap-connector/select-SAP-action-trigger.png)  

   Sie können eine Aktion auch manuell angeben:

   ![SAP-Aktion manuell eingeben](media/logic-apps-using-sap-connector/manual-enter-SAP-action-trigger.png)

   Dieses Beispiel stellt dar, wie die Aktion angezeigt wird, wenn Sie den Trigger für den Empfang mehrerer Nachrichten einrichten.

   ![Triggerbeispiel](media/logic-apps-using-sap-connector/example-trigger.png)  

   Weitere Informationen zur SAP-Aktion finden Sie unter [Nachrichtenschemas für IDOC-Vorgänge](https://docs.microsoft.com/biztalk/adapters-and-accelerators/adapter-sap/message-schemas-for-idoc-operations).

1. Speichern Sie jetzt Ihre Logik-App, um den Empfang von Nachrichten von Ihrem SAP-System zu starten.
Wählen Sie auf der Symbolleiste des Designers **Speichern** aus.

Ihre Logik-App kann jetzt Nachrichten von Ihrem SAP-System empfangen.

> [!NOTE]
> Der SAP-Trigger ist kein Abfragetrigger, sondern er basiert auf Webhooks. Der Trigger wird nur dann vom Gateway aufgerufen, wenn eine Nachricht vorhanden ist. Abfragen sind daher nicht erforderlich.

### <a name="test-your-logic-app"></a>Testen Ihrer Logik-App

1. Um Ihre Logik-App auszulösen, senden Sie eine Nachricht von Ihrem SAP-System.

1. Wählen Sie in der Logik-App-Menü **Übersicht** aus. Überprüfen Sie den **Ausführungsverlauf** für alle neuen Ausführungen für Ihre Logik-App.

1. Öffnen Sie die letzte Ausführung, in der die von Ihrem SAP-System gesendete Nachricht im Abschnitt mit den Triggerausgaben angezeigt wird.

## <a name="receive-idoc-packets-from-sap"></a>Empfangen von IDOC-Paketen von SAP

Sie können SAP für das [Senden von IDOCs in Paketen](https://help.sap.com/viewer/8f3819b0c24149b5959ab31070b64058/7.4.16/en-US/4ab38886549a6d8ce10000000a42189c.html) einrichten, bei denen es sich um Batches oder Gruppen von IDOCs handelt. Zum Empfangen von IDOC-Paketen ist für den SAP-Connector und insbesondere für den Trigger keine zusätzliche Konfiguration erforderlich. Damit Sie jedoch jedes Element in einem IDOC-Paket verarbeiten können, nachdem der Trigger das Paket empfangen hat, sind einige zusätzliche Schritte erforderlich, um das Paket in einzelne IDOCs aufzuteilen.

Im Folgenden sehen Sie ein Beispiel, das veranschaulicht, wie einzelne IDOCs mithilfe der [`xpath()`-Funktion](./workflow-definition-language-functions-reference.md#xpath) aus einem Paket extrahiert werden:

1. Zunächst benötigen Sie eine Logik-App mit einem SAP-Trigger. Wenn Sie diese Logik-App noch nicht besitzen, führen Sie die vorherigen Schritte in diesem Thema aus, um [eine Logik-App mit einem SAP-Trigger](#receive-from-sap) einzurichten.

   Beispiel:

   ![SAP-Trigger](./media/logic-apps-using-sap-connector/first-step-trigger.png)

1. Rufen Sie den Stammnamespace aus dem XML-IDOC ab, das Ihre Logik-App von SAP erhält. Fügen Sie zum Extrahieren dieses Namespace aus dem XML-Dokument einen Schritt hinzu, der eine lokale Zeichenfolgenvariable erstellt und diesen Namespace mithilfe eines `xpath()`-Ausdrucks speichert:

   `xpath(xml(triggerBody()?['Content']), 'namespace-uri(/*)')`

   ![Abrufen des Namespace](./media/logic-apps-using-sap-connector/get-namespace.png)

1. Fügen Sie zum Extrahieren eines einzelnen IDOC einen Schritt hinzu, der eine Arrayvariable erstellt und die IDOC-Sammlung mithilfe eines anderen `xpath()`-Ausdrucks speichert:

   `xpath(xml(triggerBody()?['Content']), '/*[local-name()="Receive"]/*[local-name()="idocData"]')`

   ![Abrufen eines Arrays von Elementen](./media/logic-apps-using-sap-connector/get-array.png)

   Die Arrayvariable macht jedes IDOC verfügbar, damit Ihre Logik-App diese einzeln verarbeiten kann, indem sie sie in der Sammlung auflistet. In diesem Beispiel überträgt die Logik-App jedes IDOC mithilfe einer Schleife an einen SFTP-Server:

   ![Versenden des IDOC](./media/logic-apps-using-sap-connector/loop-batch.png)

   Jedes IDOC muss den Stammnamespace enthalten. Daher wird der Inhalt der Datei zusammen mit dem Stammnamespace in ein `<Receive></Receive`-Element gepackt, bevor das IDOC an die Downstream-App oder in diesem Fall an den SFTP-Server gesendet wird.

> [!TIP]
> Sie können die Schnellstartvorlage für dieses Muster verwenden, indem Sie diese Vorlage im Logik-App-Designer auswählen, wenn Sie eine neue Logik-App erstellen.
>
> ![Batchvorlage](./media/logic-apps-using-sap-connector/batch-template.png)

## <a name="generate-schemas-for-artifacts-in-sap"></a>Generieren von Schemas für Artefakte in SAP

In diesem Beispiel wird eine Logik-App verwendet, die Sie mit einer HTTP-Anforderung auslösen können. Die SAP-Aktion sendet eine Anforderung an ein SAP-System, um die Schemas für die IDoc- und BAPI-Angabe zu generieren. Schemas, die in der Antwort zurückgegeben werden, werden über den Azure Resource Manager-Connector in ein Integrationskonto hochgeladen.

### <a name="add-an-http-request-trigger"></a>Hinzufügen eines HTTP-Anforderungstriggers

1. Erstellen Sie im Azure-Portal eine leere Logik-App, die den Logik-App-Designer öffnet.

1. Geben Sie im Suchfeld den Begriff „http request“ als Filter ein. Wählen Sie in der Liste der **Trigger** die Option **Beim Empfang einer HTTP-Anforderung** aus.

   ![Hinzufügen eines HTTP-Anforderungstriggers](./media/logic-apps-using-sap-connector/add-trigger.png)

1. Speichern Sie jetzt Ihre Logik-App, damit Sie eine Endpunkt-URL dafür erstellen können.
Wählen Sie auf der Symbolleiste des Designers **Speichern** aus.

   Die Endpunkt-URL wird nun im Trigger angezeigt, z.B.:

   ![URL für den Endpunkt generieren](./media/logic-apps-using-sap-connector/generate-http-endpoint-url.png)

### <a name="add-an-sap-action-to-generate-schemas"></a>Hinzufügen einer SAP-Aktion zum Generieren von Schemas

1. Wählen Sie im Logik-App-Designer unter dem Trigger die Option **Neuer Schritt** aus.

   ![Wählen Sie „Neuer Schritt“ aus.](./media/logic-apps-using-sap-connector/add-action.png)

1. Geben Sie im Suchfeld den Begriff „sap“ als Filter ein. Wählen Sie in der Liste mit den **Aktionen** die Aktion **Schemas generieren** aus.
  
   ![SAP-Sendeaktion auswählen](media/logic-apps-using-sap-connector/select-sap-schema-generator-action.png)

   Alternativ dazu wählen Sie die Registerkarte **Unternehmen** und dann die SAP-Aktion aus.

   ![SAP-Sendeaktion auf der Registerkarte „Unternehmen“ auswählen](media/logic-apps-using-sap-connector/select-sap-schema-generator-ent-tab.png)

1. Wenn Sie zur Angabe von Verbindungsdetails aufgefordert werden, erstellen Sie jetzt Ihre SAP-Verbindung. Sollte die Verbindung bereits bestehen, fahren Sie mit dem nächsten Schritt fort, damit Sie Ihre SAP-Aktion einrichten können.

   **Erstellen einer lokalen SAP-Verbindung**

   1. Stellen Sie die Verbindungsinformationen für Ihren SAP-Server bereit. Wählen Sie für die Eigenschaft **Datengateway** das Datengateway aus, das Sie im Azure-Portal für Ihre Gatewayinstallation erstellt haben.

      * Wenn die Eigenschaft **Anmeldetyp** auf **Anwendungsserver** festgelegt ist, sind diese (in der Regel optionalen) Eigenschaften erforderlich:

        ![Verbindung zum SAP-Anwendungsserver herstellen](media/logic-apps-using-sap-connector/create-SAP-application-server-connection.png)

      * Wenn die Eigenschaft **Anmeldetyp** auf **Gruppe** festgelegt ist, sind diese (in der Regel optionalen) Eigenschaften erforderlich:

        ![Verbindung zum SAP-Nachrichtenserver herstellen](media/logic-apps-using-sap-connector/create-SAP-message-server-connection.png)

      Standardmäßig wird starke Typisierung verwendet, um nach ungültigen Werten zu suchen, indem eine XML-Validierung anhand des Schemas durchgeführt wird. Durch dieses Verhalten können Sie Probleme früher feststellen. Die Option **Sichere Typisierung** ist aus Gründen der Abwärtskompatibilität verfügbar und überprüft nur die Länge der Zeichenfolge. Erfahren Sie mehr über die Option [Sichere Typisierung](#safe-typing).

   1. Wählen Sie **Erstellen** aus, wenn Sie fertig sind.

      Die Verbindung wird in Logic Apps eingerichtet und getestet, sodass sichergestellt ist, dass sie ordnungsgemäß funktioniert.

1. Geben Sie den Pfad zu dem Artefakt an, für das Sie das Schema generieren möchten.

   Sie können die SAP-Aktion über die Dateiauswahl auswählen:

   ![SAP-Aktion auswählen](media/logic-apps-using-sap-connector/select-SAP-action-schema-generator.png)  

   Alternativ dazu können Sie die Aktion manuell eingeben:

   ![SAP-Aktion manuell eingeben](media/logic-apps-using-sap-connector/manual-enter-SAP-action-schema-generator.png)

   Um Schemas für mehrere Artefakte zu generieren, geben Sie die Details der SAP-Aktion für jedes Artefakt an. Beispiel:

   ![„Neues Element hinzufügen“ auswählen](media/logic-apps-using-sap-connector/schema-generator-array-pick.png)

   ![Zwei Elemente anzeigen](media/logic-apps-using-sap-connector/schema-generator-example.png)

   Weitere Informationen zur SAP-Aktion finden Sie unter [Nachrichtenschemas für IDOC-Vorgänge](https://docs.microsoft.com/biztalk/adapters-and-accelerators/adapter-sap/message-schemas-for-idoc-operations).

1. Speichern Sie Ihre Logik-App. Wählen Sie auf der Symbolleiste des Designers **Speichern** aus.

### <a name="test-your-logic-app"></a>Testen Ihrer Logik-App

1. Wählen Sie in der Designer-Symbolleiste die Option **Ausführen**, um eine Ausführung für Ihre Logik-App auszulösen.

1. Öffnen Sie die Ausführung, und überprüfen Sie die Ausgaben für die Aktion **Schemas generieren**.

   Die Ausgaben zeigen die generierten Schemas für die angegebene Liste von Nachrichten.

### <a name="upload-schemas-to-an-integration-account"></a>Hochladen von Schemas in ein Integrationskonto

Optional können Sie die generierten Schemas in Repositorys wie ein Blob-, Speicher- oder Integrationskonto herunterladen oder speichern. Integrationskonten eignen sich hervorragend für die Verwendung mit anderen XML-Aktionen. Daher zeigt dieses Beispiel, wie Sie Schemas mithilfe des Azure Resource Manager-Connectors in ein Integrationskonto für die gleiche Logik-App hochladen.

1. Wählen Sie im Logik-App-Designer unter dem Trigger die Option **Neuer Schritt** aus.

1. Geben Sie im Suchfeld den Begriff „Resource Manager“ als Filter ein. Wählen Sie **Ressource erstellen oder aktualisieren** aus.

   ![Azure Resource Manager-Aktion auswählen](media/logic-apps-using-sap-connector/select-azure-resource-manager-action.png)

1. Geben Sie die Details für die Aktion einschließlich des Azure-Abonnements, der Azure-Ressourcengruppe und des Integrationskontos ein. Klicken Sie in die Felder für diese Felder, und wählen Sie aus der angezeigten dynamischen Inhaltsliste die SAP-Token aus, die den Feldern hinzugefügt werden sollen.

   1. Öffnen Sie die Liste **Neuen Parameter hinzufügen**, und wählen Sie die Felder **Speicherort** und **Eigenschaften** aus.

   1. Geben Sie wie in diesem Beispiel veranschaulicht Details für diese neuen Felder an.

      ![Details für Azure Resource Manager-Aktion eingeben](media/logic-apps-using-sap-connector/azure-resource-manager-action.png)

   Die SAP-Aktion **Schemas generieren** generiert Schemas als Sammlung, daher fügt der Designer der Aktion automatisch eine **For each**-Schleife hinzu. In diesem Beispiel wird die Darstellung dieser Aktion gezeigt:

   ![Azure Resource Manager-Aktion mit „for each“-Schleife](media/logic-apps-using-sap-connector/azure-resource-manager-action-foreach.png)  
   > [!NOTE]
   > Die Schemas verwenden das Base64-codierte Format. Damit die Schemas in ein Integrationskonto hochgeladen werden können, müssen sie mithilfe der Funktion `base64ToString()` decodiert werden. Das folgende Beispiel zeigt den Code für das Element `"properties"`:
   >
   > ```json
   > "properties": {
   >    "Content": "@base64ToString(items('For_each')?['Content'])",
   >    "ContentType": "application/xml",
   >    "SchemaType": "Xml"
   > }
   > ```

1. Speichern Sie Ihre Logik-App. Wählen Sie auf der Symbolleiste des Designers **Speichern** aus.

### <a name="test-your-logic-app"></a>Testen Ihrer Logik-App

1. Wählen Sie in der Designer-Symbolleiste die Option **Ausführen**, um Ihre Logik-App manuell auszulösen.

1. Wechseln Sie nach einer erfolgreichen Ausführung zum Integrationskonto, und überprüfen Sie, ob die generierten Schemas vorhanden sind.

## <a name="enable-secure-network-communications"></a>Aktivieren von Secure Network Communications (SNC)

Bevor Sie beginnen, stellen Sie sicher, dass Sie die zuvor aufgeführten [Voraussetzungen](#pre-reqs) erfüllt haben:

* Das lokale Datengateway ist auf einem Computer installiert, der sich im selben Netzwerk wie Ihr SAP-System befindet.
* Für SSO wird das Gateway als Benutzer ausgeführt, der dem SAP-Benutzer zugeordnet ist.
* Die SNC-Bibliothek, die die zusätzlichen Sicherheitsfunktionen bereitstellt, wurde auf demselben Computer wie das Datengateway installiert. Einige Beispiele sind [sapseculib](https://help.sap.com/saphelp_nw74/helpdata/en/7a/0755dc6ef84f76890a77ad6eb13b13/frameset.htm), Kerberos und NTLM.

   Um SNC für Ihre Anforderungen an oder aus dem SAP-System zu aktivieren, aktivieren Sie das Kontrollkästchen **SNC verwenden** in der SAP-Verbindung, und geben Sie diese Eigenschaften an:

   ![Konfigurieren von SAP-SNC in der Verbindung](media/logic-apps-using-sap-connector/configure-sapsnc.png)

   | Eigenschaft | BESCHREIBUNG |
   |----------| ------------|
   | **SNC-Bibliothekspfad** | Der Name oder Pfad der SNC-Bibliothek, der relativ zum NCo-Installationspfad oder absoluten Pfad ist. Bespiele sind `sapsnc.dll`, `.\security\sapsnc.dll` oder `c:\security\sapsnc.dll`. |
   | **SNC SSO** | Bei einer Verbindung über SNC wird die SNC-Identität typischerweise zur Authentifizierung des Aufrufers verwendet. Eine weitere Möglichkeit besteht darin, die Benutzer- und Kennwortinformationen so zu überschreiben, dass sie für die Authentifizierung des Aufrufers verwendet werden können, die Verbindung aber dennoch verschlüsselt ist. |
   | **SNC Mein Name** | In den meisten Fällen kann diese Eigenschaft ausgelassen werden. Die installierte SNC-Lösung kennt in der Regel den eigenen SNC-Namen. Nur bei Lösungen, die mehrere Identitäten unterstützen, müssen Sie möglicherweise die Identität angeben, die für dieses spezielle Ziel oder diesen Server verwendet werden soll. |
   | **Name des SNC-Partners** | Name für das Back-End-SNC |
   | **SNC-Qualität des Schutzes** | Die Servicequalität, die für die SNC-Kommunikation dieses bestimmten Ziels oder Servers verwendet werden soll. Der Standardwert wird durch das Back-End-System definiert. Der Maximalwert wird durch das für SNC verwendete Sicherheitsprodukt definiert. |
   |||

   > [!NOTE]
   > Legen Sie die Umgebungsvariablen SNC_LIB und SNC_LIB_64 nicht auf dem Computer fest, auf dem Sie das Datengateway und die SNC-Bibliothek betreiben. Wenn sie festgelegt werden, haben sie Vorrang vor dem Wert der SNC-Bibliothek, der über den Connector übergeben wird.

<a name="safe-typing"></a>

## <a name="safe-typing"></a>Sichere Typisierung

Standardmäßig wird beim Erstellen der SAP-Verbindung starke Typisierung verwendet, um nach ungültigen Werten zu suchen, indem eine XML-Validierung anhand des Schemas durchgeführt wird. Durch dieses Verhalten können Sie Probleme früher feststellen. Die Option **Sichere Typisierung** ist aus Gründen der Abwärtskompatibilität verfügbar und überprüft nur die Länge der Zeichenfolge. Wenn Sie **Sichere Typisierung** auswählen, werden die DATS- und TIMS-Typen in SAP als Zeichenfolgen und nicht als ihre XML-Entsprechungen (`xs:date` und `xs:time`, `xmlns:xs="http://www.w3.org/2001/XMLSchema"`) behandelt. Die sichere Typisierung beeinflusst das Verhalten für die gesamte Schemagenerierung, die Sendenachricht für die Nutzlast „wurde gesendet“ und die Antwort „wurde empfangen“ sowie den Trigger. 

Wenn starke Typisierung verwendet wird (**Sichere Typisierung** ist nicht aktiviert) ordnet das Schema die DATS-und TIMS-Typen einfacheren XML-Typen zu:

```xml
<xs:element minOccurs="0" maxOccurs="1" name="UPDDAT" nillable="true" type="xs:date"/>
<xs:element minOccurs="0" maxOccurs="1" name="UPDTIM" nillable="true" type="xs:time"/>
```

Beim Senden von Nachrichten mit starker Typisierung entspricht die DATS- und TIMS-Antwort dem entsprechenden XML-Typformat:

```xml
<DATE>9999-12-31</DATE>
<TIME>23:59:59</TIME>
```

Wenn **Sichere Typisierung** aktiviert ist, ordnet das Schema die DATS- und TIMS-Typen nur XML-Zeichenfolgenfeldern mit Längeneinschränkungen zu, z.B.:

```xml
<xs:element minOccurs="0" maxOccurs="1" name="UPDDAT" nillable="true">
  <xs:simpleType>
    <xs:restriction base="xs:string">
      <xs:maxLength value="8" />
    </xs:restriction>
  </xs:simpleType>
</xs:element>
<xs:element minOccurs="0" maxOccurs="1" name="UPDTIM" nillable="true">
  <xs:simpleType>
    <xs:restriction base="xs:string">
      <xs:maxLength value="6" />
    </xs:restriction>
  </xs:simpleType>
</xs:element>
```

Beim Senden von Nachrichten mit aktivierter **Sicherer Typisierung** sieht die DATS- und TIMS-Antwort wie im folgenden Beispiel aus:

```xml
<DATE>99991231</DATE>
<TIME>235959</TIME>
```

## <a name="advanced-scenarios"></a>Erweiterte Szenarien

### <a name="confirm-transaction-explicitly"></a>Explizites Bestätigen der Transaktion

Wenn Sie Transaktionen von Logic Apps an SAP senden, erfolgt dieser Austausch in zwei Schritten, wie im SAP-Dokument [Transactional RFC Server Programs](https://help.sap.com/doc/saphelp_nwpi71/7.1/en-US/22/042ad7488911d189490000e829fbbd/content.htm?no_cache=true) (Transaktionsbezogene RFC-Serverprogramme) beschrieben. Von der Aktion **Send to SAP** (An SAP senden) werden standardmäßig sowohl die Schritte für die Funktionsübertragung als auch die Schritte für die Transaktionsbestätigung in einem einzelnen Aufruf abgewickelt. Der SAP-Connector ermöglicht die Entkoppelung dieser Schritte. Sie können ein IDOC-Element senden und anstatt der automatischen Transaktionsbestätigung die explizite Aktion **Confirm transaction ID** (Transaktions-ID bestätigen) verwenden.

Diese Entkoppelung der Transaktions-ID-Bestätigung ist hilfreich, wenn Sie in SAP keine Transaktionen duplizieren möchten (etwa in Szenarien, in denen beispielsweise Fehler aufgrund von Netzwerkproblemen auftreten können). Durch die separate Bestätigung der Transaktions-ID wird die Transaktion nur einmal in Ihrem SAP-System durchgeführt.

Hier sehen Sie ein Beispiel für dieses Muster:

1. Erstellen Sie eine leere Logik-App, und fügen Sie einen HTTP-Trigger hinzu.

1. Fügen Sie über den SAP-Connector die Aktion **Send IDOC** (IDOC senden) hinzu. Geben Sie die Details für das IDOC-Element an, das Sie an Ihr SAP-System senden.

1. Soll die Transaktions-ID explizit in einem separaten Schritt bestätigt werden, wählen Sie unter der Eigenschaft **Confirm TID** (Transaktions-ID bestätigen) die Option **Nein** aus. Für die optionale Eigenschaft **Transaction ID GUID** (GUID der Transaktions-ID) können Sie den Wert entweder manuell angeben oder diese GUID automatisch durch den Connector generieren und in der Antwort auf die Aktion „Send IDOC“ (IDOC senden) zurückgeben lassen.

   ![Eigenschaften der Aktion „Send IDOC“ (IDOC senden)](./media/logic-apps-using-sap-connector/send-idoc-action-details.png)

1. Fügen Sie zur expliziten Bestätigung der Transaktions-ID die Aktion **Confirm transaction ID** (Transaktions-ID bestätigen) hinzu. Klicken Sie auf das Feld **Transaction ID** (Transaktions-ID), um die dynamische Inhaltsliste anzuzeigen. Wählen Sie in dieser Liste den **Transaktions-ID-Wert** aus, der von der Aktion **Send IDOC** (IDOC senden) zurückgegeben wurde.

   ![Aktion zum Bestätigen der Transaktions-ID](./media/logic-apps-using-sap-connector/explicit-transaction-id.png)

   Im Anschluss an diesen Schritt wird die aktuelle Transaktion auf beiden Seiten (also aufseiten des SAP-Connectors und aufseiten des SAP-Systems) als abgeschlossen markiert.

## <a name="known-issues-and-limitations"></a>Bekannte Probleme und Einschränkungen

Zurzeit sind für den SAP-Connector folgende Probleme und Einschränkungen bekannt:

* Der SAP-Trigger unterstützt keine Datengatewaycluster. In einigen Failoverfällen unterscheidet sich der Datengatewayknoten, der mit dem SAP-System kommuniziert, von dem aktiven Knoten. Dies führt zu unerwartetem Verhalten. In Sendeszenarien werden Datengatewaycluster unterstützt.

* Der SAP-Connector unterstützt derzeit keine SAP-Router-Zeichenfolgen. Das lokale Datengateway muss in demselben LAN wie das SAP-System vorhanden sein, mit dem eine Verbindung hergestellt werden soll.

## <a name="connector-reference"></a>Connector-Referenz

Technische Details zu Triggern, Aktionen und Beschränkungen aus der OpenAPI-Beschreibung (ehemals Swagger) des Connectors finden Sie auf der [Referenzseite des Connectors](/connectors/sap/).

## <a name="next-steps"></a>Nächste Schritte

* [Herstellen einer Verbindung von Azure Logic Apps zu lokalen Systemen](../logic-apps/logic-apps-gateway-connection.md)
* In diesem Artikel lernen Sie die Überprüfung, die Transformation und die Verwendung anderer Nachrichtenvorgänge im [Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md) kennen.
* Informationen zu anderen [Logic Apps-Connectors](../connectors/apis-list.md)
