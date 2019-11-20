---
title: Batchverarbeitung von Nachrichten als Gruppe – Azure Logic Apps
description: Senden und Empfangen von Nachrichten als Batches in Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: estfan, jonfan, LADocs
ms.topic: article
ms.date: 01/16/2019
ms.openlocfilehash: b56a50fceec8ac6be966c0c58a82e94e0c977143
ms.sourcegitcommit: d37991ce965b3ee3c4c7f685871f8bae5b56adfa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "72680438"
---
# <a name="send-receive-and-batch-process-messages-in-azure-logic-apps"></a>Versand, Empfang und Batchverarbeitung von Nachrichten in Azure Logic Apps

Wenn Sie Nachrichten zusammen als Gruppen senden und verarbeiten möchten, können Sie eine Batchverarbeitungslösung erstellen, die Nachrichten in einem *Batch* sammelt, bis die angegebenen Kriterien zur Freigabe und Verarbeitung der Nachrichten im Batch erfüllt sind. Mittels Batchverarbeitung können Sie erreichen, dass Ihre Logik-App Nachrichten weniger häufig verarbeitet. In diesem Artikel wird gezeigt, wie Sie eine Batchverarbeitungslösung erstellen, indem Sie zwei Logik-Apps im gleichen Azure-Abonnement und der gleichen Azure-Region in folgender bestimmter Reihenfolge erstellen: 

* Die [Logik-App für den Batchempfang](#batch-receiver), die Nachrichten in einem Batch annimmt und sammelt, bis die angegebenen Kriterien zur Freigabe und Verarbeitung der Nachrichten erfüllt sind

  Stellen Sie sicher, dass Sie zuerst die Logik-App für den Batchempfang erstellen, sodass Sie später beim Erstellen der Logik-App für den Batchversand das Batchziel auswählen können.

* Mindestens eine [Logik-App für den Batchversand](#batch-sender), die die Nachrichten an die zuvor erstellte Logik-App für den Batchempfang sendet 

   Darüber hinaus können Sie einen eindeutigen Schlüssel (beispielsweise eine Kundennummer) angeben, die den Zielbatch auf der Grundlage dieses Schlüssels in logische Untergruppen *partitioniert* (unterteilt). Auf diese Weise kann die App für den Empfang alle Elemente mit demselben Schlüssel sammeln und zusammen verarbeiten.

Stellen Sie sicher, dass sich die Logik-App für den Batchempfang und die Logik-App für den Batchversand im gleichen Azure-Abonnement *und* in der gleichen Azure-Region befinden. Wenn dies nicht der Fall ist, können Sie beim Erstellen der Logik-App für den Batchversand nicht die Logik-App für den Batchempfang auswählen, da sie gegenseitig nicht sichtbar sind.

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Beispiel benötigen Sie Folgendes:

* Ein Azure-Abonnement. Falls Sie über kein Abonnement verfügen, können Sie [mit einem kostenlosen Azure-Konto beginnen](https://azure.microsoft.com/free/). Oder [registrieren Sie sich für ein Abonnement mit nutzungsbasierter Bezahlung](https://azure.microsoft.com/pricing/purchase-options/).

* Ein E-Mail-Konto bei einem [von Azure Logic Apps unterstützten E-Mail-Anbieter](../connectors/apis-list.md)

* Grundlegende Kenntnisse über die [Erstellung von Logik-Apps](../logic-apps/quickstart-create-first-logic-app-workflow.md) 

* Um anstelle des Azure-Portals Visual Studio zu verwenden, müssen Sie [Visual Studio zur Verwendung mit Logik Apps einrichten](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md).

<a name="batch-receiver"></a>

## <a name="create-batch-receiver"></a>Erstellen der Logik-App für den Batchempfang

Um Nachrichten an einen Batch senden zu können, muss dieser Batch zunächst als Ziel vorhanden sein, an das die Nachrichten gesendet werden. Daher müssen Sie zunächst die Logik-App für den Batchempfang erstellen, die über den Trigger **Batch** gestartet wird. Dadurch können Sie die Logik-App für den Batchempfang auswählen, wenn Sie die Logik-App für den Batchversand erstellen. Die Logik-App für den Batchempfang sammelt Nachrichten, bis die angegebenen Kriterien zur Freigabe und Verarbeitung der Nachrichten erfüllt sind. Beim Batchempfang sind keine Angaben zum Batchversand erforderlich, beim Batchversand wird dagegen das Ziel benötigt, an das die Nachrichten gesendet werden. 

1. Erstellen Sie im [Azure-Portal](https://portal.azure.com) oder in Visual Studio eine Logik-App mit diesem Namen: „BatchReceiver“ 

2. Fügen Sie im Designer für Logik-Apps den Trigger **Batch** hinzu, um den Workflow Ihrer Logik-App zu starten. Geben Sie im Suchfeld den Begriff „Batch“ als Filter ein. Wählen Sie den folgenden Trigger aus: **Batchnachrichten**

   ![Hinzufügen des Triggers „Batchnachrichten“](./media/logic-apps-batch-process-send-receive-messages/add-batch-receiver-trigger.png)

3. Legen Sie diese Eigenschaften für den Batchempfang fest: 

   | Eigenschaft | BESCHREIBUNG | 
   |----------|-------------|
   | **Batchmodus** | - **Inline**: Zum Definieren von Freigabekriterien im Batchtrigger <br>- **Integrationskonto**: Zum Definieren mehrerer Konfigurationen für Freigabekriterien über ein [Integrationskonto](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md). Mit einem Integrationskonto können Sie diese Konfigurationen an zentraler Stelle statt in separaten Logik-Apps verwalten. | 
   | **Batchname** | Der Name für Ihren Batch (in diesem Beispiel „TestBatch“). Er gilt nur für den Batchmodus **Inline**. |  
   | **Freigabekriterien** | Diese Eigenschaft gilt nur für den Batchmodus **Inline** und gibt die Kriterien an, die vor der Verarbeitung der einzelnen Batches erfüllt sein müssen: <p>- **Basierend auf der Nachrichtenanzahl**: Den Batch auf Grundlage der Anzahl der vom Batch gesammelten Nachrichten freigeben. <br>- **Basierend auf Größe**: Den Batch auf Grundlage der Gesamtgröße aller von diesem Batch gesammelten Nachrichten in Bytes freigeben. <br>- **Zeitplan**: Den Batch auf Grundlage eines Wiederholungszeitplans freigeben, der ein Intervall und eine Häufigkeit angibt. In den erweiterten Optionen können Sie außerdem eine Zeitzone auswählen und ein Startdatum sowie eine Startzeit bereitstellen. <br>- **Alle auswählen**: Alle angegebenen Kriterien verwenden. | 
   | **Nachrichtenanzahl** | Die Anzahl von Nachrichten, die im Batch gesammelt werden sollen, z. B. 10 Nachrichten Der Grenzwert für einen Batch ist 8.000 Nachrichten. | 
   | **Batch Size** | Die Gesamtgröße in Bytes, die im Batch gesammelt werden soll, z. B. 10 MB. Das Größenlimit für einen Batch beträgt 80 MB. | 
   | **Zeitplan** | Das Intervall und die Häufigkeit zwischen Batchfreigaben, z. B. 10 Minuten. Der Mindestwert für die Wiederholungen beträgt 60 Sekunden bzw. 1 Minute. Minutenbruchteile werden auf 1 Minute aufgerundet. Um eine Zeitzone oder ein Startdatum und eine Startuhrzeit anzugeben, wählen Sie **Erweiterte Optionen anzeigen** aus. | 
   ||| 

   > [!NOTE]
   > 
   > Wenn Sie die Freigabekriterien ändern, während der Trigger noch über als Stapel zusammengefasste, aber nicht gesendete Nachrichten verfügt, verwendet er die aktualisierten Freigabekriterien, um die nicht gesendeten Nachrichten zu verarbeiten. 

   Dieses Beispiel zeigt alle Kriterien, doch probieren Sie für Ihre eigenen Tests nur ein Kriterium aus:

   ![Angeben von Details für den Batchtrigger](./media/logic-apps-batch-process-send-receive-messages/batch-receiver-criteria.png)

4. Fügen Sie nun eine oder mehrere Aktionen für die Verarbeitung der einzelnen Batches hinzu. 

   Fügen Sie für dieses Beispiel eine Aktion hinzu, die eine E-Mail sendet, wenn der Batchtrigger ausgelöst wird. 
   Der Trigger wird ausgeführt und sendet eine E-Mail, wenn der Batch 10 Nachrichten enthält oder 10 MB umfasst oder wenn 10 Minuten vergangen sind.

   1. Wählen Sie unter dem Batchtrigger die Option **+ Neuer Schritt** aus.

   2. Geben Sie im Suchfeld den Begriff „E-Mail senden“ als Filter ein.
   Wählen Sie basierend auf Ihrem E-Mail-Anbieter einen E-Mail-Connector aus.

      Beispiel: Wenn Sie ein persönliches Konto wie @outlook.com oder @hotmail.com besitzen, wählen Sie den Outlook.com-Connector aus. 
      Wenn Sie ein Gmail-Konto besitzen, wählen Sie den Gmail-Connector aus. 
      In diesem Beispiel wird Office 365 Outlook verwendet. 

   3. Wählen Sie diese Aktion aus: **E-Mail senden – <*E-Mail-Anbieter*>**

      Beispiel:

      ![Auswählen der Aktion „E-Mail senden“ für Ihren E-Mail-Anbieter](./media/logic-apps-batch-process-send-receive-messages/batch-receiver-send-email-action.png)

5. Melden Sie sich nach Aufforderung bei Ihrem E-Mail-Konto an. 

6. Legen Sie die Eigenschaften für die hinzugefügte Aktion fest.

   * Geben Sie im Feld **An** die E-Mail-Adresse des Empfängers ein. 
   Zu Testzwecken können Sie hier Ihre eigene E-Mail-Adresse angeben.

   * Wählen Sie im Feld **Betreff** in der angezeigten Liste mit dynamischem Inhalt das Feld **Partitionsname** aus.

     ![Auswählen von „Partitionsname“ in der Liste „Dynamischer Inhalt“](./media/logic-apps-batch-process-send-receive-messages/send-email-action-details.png)

     Später im Batchversand können Sie einen eindeutigen Partitionsschlüssel angeben, um den Zielbatch in logische Untergruppen zu unterteilen, an die Nachrichten gesendet werden können. 
     Jede Gruppe besitzt eine eindeutige Nummer, die von der Logik-App für den Batchversand generiert wird. 
     Dadurch können Sie einen einzelnen Batch mit mehreren Untergruppen verwenden und die einzelnen Untergruppen jeweils mit einem Namen Ihrer Wahl definieren.

     > [!IMPORTANT]
     > Eine Partition hat einen Grenzwert von 5.000 Nachrichten oder 80 MB. Wenn eine der Bedingungen erfüllt ist, gibt Logic Apps den Batch ggf. frei, auch wenn Ihre definierte Freigabebedingung nicht erfüllt ist.

   * Wählen Sie im Feld **Text** in der angezeigten Liste mit dynamischem Inhalt das Feld **Nachrichten-ID** aus. 

     Der Logic Apps-Designer fügt automatisch eine For Each-Schleife zur Aktion zum Senden von E-Mails hinzu, da diese Aktion die Ausgabe der vorherigen Aktion nicht als Batch, sondern als Sammlung behandelt. 

     ![Auswählen von „Nachrichten-ID“ unter „Text“](./media/logic-apps-batch-process-send-receive-messages/send-email-action-details-for-each.png)

7.  Speichern Sie Ihre Logik-App. Sie haben jetzt eine Logik-App für den Batchempfang erstellt.

    ![Speichern Ihrer Logik-App](./media/logic-apps-batch-process-send-receive-messages/save-batch-receiver-logic-app.png)

8. Stellen Sie bei Verwendung von Visual Studio sicher, dass Sie [die Logik-App für den Batchempfang in Azure bereitstellen](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md#deploy-logic-app-to-azure). Andernfalls können Sie beim Erstellen der Logik-App für den Batchversand nicht die Logik-App für den Batchempfang auswählen.

<a name="batch-sender"></a>

## <a name="create-batch-sender"></a>Erstellen der Logik-App für den Batchversand

Erstellen Sie nun eine oder mehrere Logik-Apps für den Batchversand, die Nachrichten an die Logik-App für den Batchempfang senden. In jeder Logik-App für den Batchversand geben Sie die Logik-App für den Batchempfang sowie den Batchnamen, den Nachrichteninhalt und gegebenenfalls weitere Einstellungen an. Optional können Sie einen eindeutigen Partitionsschlüssel angeben, um den Batch zum Sammeln von Nachrichten mit diesem Schlüssel in logische Untergruppen zu unterteilen. 

* Stellen Sie sicher, dass Sie bereits [die Logik-App für den Batchempfang erstellt haben](#batch-receiver), sodass Sie diese beim Erstellen der Logik-App für den Batchversand als Batchziel auswählen können. Beim Batchempfang sind keine Angaben zum Batchversand erforderlich, beim Batchversand wird dagegen das Ziel benötigt, an das die Nachrichten gesendet werden. 

* Stellen Sie sicher, dass sich die Logik-App für den Batchempfang und die Logik-App für den Batchversand in der gleichen Azure-Region *und* dem gleichen Azure-Abonnement befinden. Wenn dies nicht der Fall ist, können Sie beim Erstellen der Logik-App für den Batchversand nicht die Logik-App für den Batchempfang auswählen, da sie gegenseitig nicht sichtbar sind.

1. Erstellen Sie eine weitere Logik-App mit diesem Namen: „BatchSender“

   1. Geben Sie im Suchfeld den Begriff „Wiederholung“ als Filter ein. 
   Wählen Sie den folgenden Trigger aus: **Wiederholung – Zeitplan**

      ![Hinzufügen des Triggers „Wiederholung – Zeitplan“](./media/logic-apps-batch-process-send-receive-messages/add-schedule-trigger-batch-sender.png)

   2. Legen Sie die Häufigkeit und das Ausführungsintervall der Logik-App für den Versand auf einmal pro Minute fest.

      ![Festlegen von Häufigkeit und Intervall für den Wiederholungstrigger](./media/logic-apps-batch-process-send-receive-messages/recurrence-trigger-batch-sender-details.png)

2. Fügen Sie eine neue Aktion zum Senden von Nachrichten an einen Batch hinzu.

   1. Wählen Sie unter dem Wiederholungstrigger die Option **Neuer Schritt** aus.

   2. Geben Sie im Suchfeld den Begriff „Batch“ als Filter ein. 
   Wählen Sie die Liste **Aktionen** aus, und wählen Sie dann diese Aktion aus: **Logik-App-Workflow mit Batchtrigger auswählen – Nachrichten an Batch senden**

      ![Auswählen von „Logik-App-Workflow mit Batchtrigger auswählen“](./media/logic-apps-batch-process-send-receive-messages/send-messages-batch-action.png)

   3. Wählen Sie die zuvor erstellte Logik-App für den Batchempfang aus.

      ![Auswählen der Logik-App „BatchReceiver“](./media/logic-apps-batch-process-send-receive-messages/batch-sender-select-batch-receiver.png)

      > [!NOTE]
      > Die Liste enthält außerdem weitere Logik-Apps mit Batchtrigger. 
      > 
      > Wenn Sie Visual Studio verwenden und keine Logik-Apps für den Batchempfang zum Auswählen angezeigt werden, vergewissern Sie sich, dass Sie die Logik-App für den Batchempfang in Azure bereitgestellt haben. Ist dies nicht der Fall, lesen Sie die Informationen zum [Bereitstellen der Logik-App für den Batchempfang in Azure](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md#deploy-logic-app-to-azure). 

   4. Wählen Sie diese Aktion aus: **Batch_messages – <*Ihre Logik-App für den Batchempfang*>**

      ![Wählen Sie diese Aktion aus: „Batch_messages – <Ihre Logik-App>“](./media/logic-apps-batch-process-send-receive-messages/batch-sender-select-batch.png)

3. Legen Sie die Eigenschaften für den Batchversand fest.

   | Eigenschaft | BESCHREIBUNG | 
   |----------|-------------| 
   | **Batchname** | Der in der Logik-App für den Empfang definierte Batchname, in diesem Beispiel „TestBatch“. <p>**Wichtig**: Der Batchname wird zur Laufzeit überprüft und muss mit dem in der Logik-App für den Empfang angegebenen Namen übereinstimmen. Wenn Sie den Batchnamen ändern, funktioniert die Logik-App für den Batchversand nicht. | 
   | **Nachrichteninhalt** | Der Inhalt für die Nachricht, die Sie senden möchten | 
   ||| 

   Fügen Sie in diesem Beispiel den folgenden Ausdruck hinzu, um das aktuelle Datum und die aktuelle Uhrzeit in den Nachrichteninhalt einzufügen, den Sie an den Batch senden:

   1. Klicken Sie in das Feld **Nachrichteninhalt**. 

   2. Wenn die Liste „Dynamischer Inhalt“ angezeigt wird, wählen Sie **Ausdruck** aus. 

   3. Geben Sie den Ausdruck `utcnow()` ein, und wählen Sie **OK** aus. 

      ![Auswählen von „Ausdruck“ für „Nachrichteninhalt“, Eingeben von „utcnow()“ und Klicken auf „OK“](./media/logic-apps-batch-process-send-receive-messages/batch-sender-details.png)

4. Richten Sie nun eine Partition für den Batch ein. Wählen Sie in der Aktion „BatchReceiver“ die Option **Erweiterte Optionen anzeigen** aus, und legen Sie die folgenden Eigenschaften fest:

   | Eigenschaft | BESCHREIBUNG | 
   |----------|-------------| 
   | **Partitionsname** | Ein optionaler eindeutiger Partitionsschlüssel, der es ermöglicht, den Zielbatch in logische Untergruppen aufzuteilen und Nachrichten auf der Grundlage dieses Schlüssels zu sammeln | 
   | **Nachrichten-ID** | Eine optionale Nachrichten-ID, bei der es sich um einen global eindeutigen Bezeichner (Globally Unique Identifier, GUID) handelt, wenn nichts angegeben wird | 
   ||| 

   Fügen Sie in diesem Beispiel im Feld **Partitionsname** einen Ausdruck hinzu, der eine Zufallszahl zwischen eins und fünf generiert. Lassen Sie das Feld **Nachrichten-ID** leer.
   
   1. Klicken Sie in das Feld **Partitionsname**, damit die Liste mit dynamischem Inhalt angezeigt wird. 

   2. Wenn die Liste mit den dynamischen Inhalten erscheint, wählen Sie **Ausdruck** aus.
   
   3. Geben Sie den Ausdruck `rand(1,6)` ein, und wählen Sie **OK** aus.

      ![Einrichten einer Partition für den Zielbatch](./media/logic-apps-batch-process-send-receive-messages/batch-sender-partition-advanced-options.png)

      Diese Funktion vom Typ **rand** generiert eine Zahl zwischen eins und fünf. 
      Der Batch wird also in fünf nummerierte Partitionen unterteilt, die dynamisch durch diesen Ausdruck festgelegt werden.

5. Speichern Sie Ihre Logik-App. Ihre Logik-App für den Versand sieht nun in etwa wie folgt aus:

   ![Speichern Ihrer Logik-App für den Versand](./media/logic-apps-batch-process-send-receive-messages/batch-sender-finished.png)

## <a name="test-your-logic-apps"></a>Testen Ihrer Logik-Apps

Lassen Sie Ihre Logik-Apps zum Testen Ihrer Batchverarbeitungslösung einige Minuten laufen. In Kürze erhalten Sie E-Mails in Fünfergruppen, die jeweils über den gleichen Partitionsschlüssel verfügen.

Die Logik-App für den Batchversand wird einmal pro Minute ausgeführt. Sie generiert eine Zufallszahl zwischen eins und fünf und verwendet diese generierte Zahl als Partitionsschlüssel für den Zielbatch, an den Nachrichten gesendet werden. Sobald der Batch fünf Elemente mit dem gleichen Partitionsschlüssel enthält, wird die Logik-App für den Batchempfang ausgelöst und sendet für jede Nachricht eine E-Mail.

> [!IMPORTANT]
> Vergessen Sie am Ende Ihres Tests nicht, die Logik-App „BatchSender“ zu deaktivieren, um zu verhindern, dass die Logik-App weiterhin Nachrichten sendet und dadurch Ihren Posteingang überlädt.

## <a name="next-steps"></a>Nächste Schritte

* [Zusammenfassen von EDI-Nachrichten als Batch und Senden der Nachrichten](../logic-apps/logic-apps-scenario-edi-send-batch-messages.md)
* [Erstellen von Workflowdefinitionen für Logik-Apps per JSON-Code](../logic-apps/logic-apps-author-definitions.md)
* [Erstellen einer serverlosen App mit Azure Logic Apps und Functions](../logic-apps/logic-apps-serverless-get-started-vs.md)
* [Ausnahmebehandlung und Fehlerprotokollierung für Logik-Apps](../logic-apps/logic-apps-scenario-error-and-exception-handling.md)
