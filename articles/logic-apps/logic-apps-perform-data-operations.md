---
title: Durchführen von Vorgängen für Daten – Azure Logic Apps | Microsoft-Dokumentation
description: Konvertieren, Verwalten und Bearbeiten von Datenausgaben und -formaten in Azure Logic Apps
services: logic-apps
ms.service: logic-apps
author: ecfan
ms.author: estfan
manager: jeconnoc
ms.topic: article
ms.date: 07/30/2018
ms.reviewer: klam, LADocs
ms.suite: integration
ms.openlocfilehash: 93c24f88fcd6a002493933ef71c5c80bd2ff8c10
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2019
ms.locfileid: "67206212"
---
# <a name="perform-data-operations-in-azure-logic-apps"></a>Durchführen von Datenvorgängen in Azure Logic Apps

In diesem Artikel wird veranschaulicht, wie Sie in Ihren Logik-Apps mit Daten arbeiten können, indem Sie Aktionen für diese und andere Aufgaben hinzufügen:

* Erstellen von Tabellen aus Arrays
* Erstellen von Arrays aus anderen Arrays basierend auf einer Bedingung
* Erstellen von benutzerfreundlichen Token aus JSON-Objekteigenschaften (JavaScript Object Notation), damit Sie diese Eigenschaften leicht in Ihrem Workflow nutzen können

Wenn Sie hier nicht die gewünschte Aktion finden, können Sie die vielen verschiedenen [Funktionen zur Bearbeitung von Daten](../logic-apps/workflow-definition-language-functions-reference.md) durchsuchen, die von Logic Apps bereitgestellt werden. 

In diesen Tabellen sind die Datenvorgänge zusammengefasst, die Sie nutzen können. Sie sind anhand des Quelldatentyps organisiert, der von den Vorgängen verwendet wird, aber die Beschreibungen sind alphabetisch angegeben.

**Arrayaktionen** 

Diese Aktionen erleichtern Ihnen die Arbeit mit Daten in Arrays.

| Aktion | BESCHREIBUNG | 
|--------|-------------| 
| [**Create CSV table**](#create-csv-table-action) | Dient zum Erstellen einer CSV-Tabelle (durch Trennzeichen getrennt) aus einem Array. | 
| [**Create HTML table**](#create-html-table-action) | Dient zum Erstellen einer HTML-Tabelle aus einem Array. | 
| [**Filter array**](#filter-array-action) | Dient zum Erstellen einer Arrayteilmenge aus einem Array basierend auf dem angegebenen Filter bzw. der Bedingung. | 
| [**Join**](#join-action) | Dient zum Erstellen einer Zeichenfolge aus allen Elementen eines Arrays und Trennen der Elemente mit dem angegebenen Zeichen. | 
| [**Select**](#select-action) | Dient zum Erstellen eines Arrays aus den angegebenen Eigenschaften für alle Elemente eines anderen Arrays. | 
||| 

**JSON-Aktionen**

Diese Aktionen erleichtern Ihnen die Arbeit mit Daten im JSON-Format (JavaScript Object Notation).

| Aktion | BESCHREIBUNG | 
|--------|-------------| 
| [**Compose**](#compose-action) | Dient zum Erstellen einer Nachricht oder Zeichenfolge aus mehreren Eingaben, die unterschiedliche Datentypen aufweisen können. Sie können diese Zeichenfolge dann als einzelne Eingabe verwenden, anstatt immer wieder die gleichen Eingaben vornehmen zu müssen. Beispielsweise können Sie eine einzelne JSON-Nachricht aus unterschiedlichen Eingaben erstellen. | 
| [**Parse JSON**](#parse-json-action) | Dient zum Erstellen von benutzerfreundlichen Datentoken für Eigenschaften in JSON-Inhalten, damit Sie die Eigenschaften leichter in Ihren Logik-Apps nutzen können. | 
||| 

Informationen zur Erstellung von komplexeren JSON-Transformationen finden Sie unter [Perform advanced JSON transformations with Liquid templates](../logic-apps/logic-apps-enterprise-integration-liquid-transform.md) (Durchführen von erweiterten JSON-Transformationen mit Liquid-Vorlagen).

## <a name="prerequisites"></a>Voraussetzungen

Sie benötigen Folgendes, um die Beispiele in diesem Artikel verwenden zu können:

* Ein Azure-Abonnement. Wenn Sie noch kein Azure-Abonnement haben, <a href="https://azure.microsoft.com/free/" target="_blank">melden Sie sich für ein kostenloses Azure-Konto an</a>.

* Die Logik-App, für die Sie den Vorgang für die Arbeit mit den Daten benötigen. 

  Falls Sie noch nicht mit Logik-Apps vertraut sind, finden Sie weitere Informationen unter [Was ist Azure Logic Apps?](../logic-apps/logic-apps-overview.md) und [Schnellstart: Erstellen Ihres ersten automatisierten Workflows mit Azure Logic Apps – Azure-Portal](../logic-apps/quickstart-create-first-logic-app-workflow.md).

* Ein [Trigger](../logic-apps/logic-apps-overview.md#logic-app-concepts), der als erster Schritt in Ihrer Logik-App verwendet wird. 

  Datenvorgänge sind nur als Aktionen verfügbar. Bevor Sie diese Aktionen nutzen können, müssen Sie also Ihre Logik-App mit einem Trigger starten und alle anderen Aktionen einbinden, die zum Erstellen der gewünschten Ausgaben erforderlich sind.

<a name="compose-action"></a>

## <a name="compose-action"></a>Compose-Aktion

Zum Erstellen eines einzelnen Ausgabe, z.B. eines JSON-Objekts aus mehreren Eingaben, können Sie die Aktion **Datenvorgänge – Compose** verwenden. Ihre Eingaben können unterschiedliche Typen aufweisen, z.B. Integer, boolesche Wert, Arrays, JSON-Objekte und andere native Typen, die von Azure Logic Apps unterstützt werden. Beispiele hierfür sind Binärdateien und XML. Anschließend können Sie die Ausgabe in Aktionen verwenden, die nach der Aktion **Compose** folgen. Die Aktion **Compose** kann Ihnen beim Erstellen des Workflows Ihrer Logik-App auch das wiederholte Eingeben der gleichen Angaben ersparen. 

Sie können beispielsweise eine JSON-Nachricht aus mehreren Variablen erstellen, z.B. Zeichenfolgenvariablen, in denen die Vor- und Nachnamen von Benutzern gespeichert werden, und einer Integer-Variablen zum Speichern des Alters der Benutzer. Hier akzeptiert die Aktion **Compose** diese Eingaben:

`{ "age": <ageVar>, "fullName": "<lastNameVar>, <firstNameVar>" }`

Anschließend erstellt sie diese Ausgabe:

`{"age":35,"fullName":"Owens,Sophie"}`

Führen Sie diese Schritte mit dem Logik-App-Designer aus, wenn Sie ein Beispiel ausprobieren möchten. Falls Sie den Codeansichts-Editor bevorzugen, können Sie die Aktionsdefinitionen des Beispiels für **Compose** und **Initialize variable** auch aus diesem Artikel in die zugrunde liegende Workflowdefinition Ihrer eigenen Logik-App kopieren: [Codebeispiele für Datenvorgänge – Compose](../logic-apps/logic-apps-data-operations-code-samples.md#compose-action-example) 

1. Öffnen Sie im <a href="https://portal.azure.com" target="_blank">Azure-Portal</a> oder in Visual Studio Ihre Logik-App im Logik-App-Designer. 

   In diesem Beispiel werden das Azure-Portal und eine Logik-App mit einem **Recurrence**-Trigger und mehrere Aktionen vom Typ **Initialize variable** verwendet. 
   Diese Aktionen sind für die Erstellung von zwei Zeichenfolgenvariablen und einer Integer-Variablen eingerichtet. Wenn Sie Ihre Logik-App zu einem späteren Zeitpunkt testen, können Sie Ihre App manuell ausführen, ohne auf das Auslösen des Triggers zu warten.

   ![Starten des Beispiels für die Logik-App](./media/logic-apps-perform-data-operations/sample-starting-logic-app-compose-action.png)

2. Führen Sie in Ihrer Logik-App, in der Sie die Ausgabe erstellen möchten, einen dieser Schritte aus: 

   * Zum Hinzufügen einer Aktion unter dem letzten Schritt wählen Sie **Neuer Schritt** > **Aktion hinzufügen**.

     ![Hinzufügen einer Aktion](./media/logic-apps-perform-data-operations/add-compose-action.png)

   * Zum Hinzufügen einer Aktion zwischen Schritten bewegen Sie den Mauszeiger über den Verbindungspfeil, sodass das Pluszeichen (+) angezeigt wird. 
   Wählen Sie das Pluszeichen und dann die Option **Aktion hinzufügen**.

3. Geben Sie im Suchfeld den Begriff „compose“ als Filter ein. Wählen Sie in der Liste mit den Aktionen diese Aktion aus: **Verfassen**

   ![Auswählen der Aktion „Compose“](./media/logic-apps-perform-data-operations/select-compose-action.png)

4. Nehmen Sie im Feld **Eingaben** die gewünschten Eingaben für die Erstellung der Ausgabe vor. 

   In diesem Beispiel wird beim Klicken in das Feld **Eingaben** die Liste mit den dynamischen Inhalten angezeigt, damit Sie die zuvor erstellten Variablen auswählen können:

   ![Auswählen von zu verfassenden Eingaben](./media/logic-apps-perform-data-operations/configure-compose-action.png)

   Hier ist das fertige Beispiel für die Aktion **Compose** angegeben: 

   ![Fertige Aktion „Compose“](./media/logic-apps-perform-data-operations/finished-compose-action.png)

5. Speichern Sie Ihre Logik-App. Wählen Sie auf der Symbolleiste des Designers **Speichern**.

Weitere Informationen zu dieser Aktion in Ihrer zugrunde liegenden Workflowdefinition finden Sie unter [Aktion „Compose“](../logic-apps/logic-apps-workflow-actions-triggers.md#compose-action). 

### <a name="test-your-logic-app"></a>Testen Ihrer Logik-App

Sie können sich vergewissern, dass mit der Aktion **Compose** die erwarteten Ergebnisse erstellt werden, indem Sie an sich selbst eine Benachrichtigung senden, die die Ausgabe der Aktion **Compose** enthält.

1. Fügen Sie in Ihrer Logik-App eine Aktion hinzu, über die die Ergebnisse der Aktion **Compose** an Sie gesendet werden können.

2. Klicken Sie in dieser Aktion auf eine beliebige Position, an der die Ergebnisse angezeigt werden sollen. Wählen Sie nach dem Öffnen der Liste mit den dynamischen Inhalten unter der Aktion **Compose** die Option **Ausgabe**. 

   In diesem Beispiel wird die Aktion **Office 365 Outlook – Send an email** verwendet. Sie enthält das Feld **Ausgabe** im Text und Betreff der E-Mail:

   ![Feld „Ausgabe“ in der Aktion „Send an email“](./media/logic-apps-perform-data-operations/send-email-compose-action.png)

3. Führen Sie Ihre Logik-App jetzt manuell aus. Wählen Sie auf der Symbolleiste des Designers die Option **Ausführen**. 

   Hier sind die Ergebnisse angegeben, die Sie basierend auf dem verwendeten E-Mail-Connector erhalten:

   ![E-Mail mit Ergebnissen der Aktion „Compose“](./media/logic-apps-perform-data-operations/compose-email-results.png)

<a name="create-csv-table-action"></a>

## <a name="create-csv-table-action"></a>Aktion „Create CSV table“

Verwenden Sie die Aktion **Datenvorgänge – Create CSV table**, um eine CSV-Tabelle (durch Trennzeichen getrennte Werte) zu erstellen, die die Eigenschaften und Werte aus JSON-Objekten (JavaScript Object Notation) in einem Array enthält. Sie können die sich ergebende Tabelle dann in Aktionen verwenden, die auf die Aktion **Create CSV table** folgen. 

Falls Sie den Codeansichts-Editor bevorzugen, können Sie die Aktionsdefinitionen des Beispiels für **Create CSV table** und **Initialize variable** aus diesem Artikel in die zugrunde liegende Workflowdefinition Ihrer eigenen Logik-App kopieren: [Codebeispiele für Datenvorgänge – Create CSV table](../logic-apps/logic-apps-data-operations-code-samples.md#create-csv-table-action-example) 

1. Öffnen Sie im <a href="https://portal.azure.com" target="_blank">Azure-Portal</a> oder in Visual Studio Ihre Logik-App im Logik-App-Designer. 

   In diesem Beispiel werden das Azure-Portal und eine Logik-App mit einem **Recurrence**-Trigger und der Aktion **Initialize variable** verwendet. 
   Die Aktion ist auf die Erstellung einer Variablen ausgelegt, deren anfänglicher Wert ein Array mit einigen Eigenschaften und Werten im JSON-Format ist. 
   Wenn Sie Ihre Logik-App zu einem späteren Zeitpunkt testen, können Sie Ihre App manuell ausführen, ohne auf das Auslösen des Triggers zu warten.

   ![Starten des Beispiels für die Logik-App](./media/logic-apps-perform-data-operations/sample-starting-logic-app-create-table-action.png)

2. Führen Sie in Ihrer Logik-App, in der Sie die CSV-Tabelle erstellen möchten, einen dieser Schritte aus: 

   * Zum Hinzufügen einer Aktion unter dem letzten Schritt wählen Sie **Neuer Schritt** > **Aktion hinzufügen**.

     ![Hinzufügen einer Aktion](./media/logic-apps-perform-data-operations/add-create-table-action.png)

   * Zum Hinzufügen einer Aktion zwischen Schritten bewegen Sie den Mauszeiger über den Verbindungspfeil, sodass das Pluszeichen (+) angezeigt wird. 
   Wählen Sie das Pluszeichen und dann die Option **Aktion hinzufügen**.

3. Geben Sie im Suchfeld den Begriff „create csv table“ als Filter ein. Wählen Sie in der Liste mit den Aktionen diese Aktion aus: **CSV-Tabelle erstellen**

   ![Auswählen der Aktion „Create CSV table“](./media/logic-apps-perform-data-operations/select-create-csv-table-action.png)

4. Geben Sie im Feld **Von** das Array oder den Ausdruck ein, das bzw. den Sie zum Erstellen der Tabelle verwenden möchten. 

   In diesem Beispiel wird beim Klicken in das Feld **Von** die Liste mit den dynamischen Inhalten angezeigt, damit Sie die zuvor erstellte Variable auswählen können:

   ![Auswählen der Arrayausgabe für die Erstellung der CSV-Tabelle](./media/logic-apps-perform-data-operations/configure-create-csv-table-action.png)

   Hier ist die fertige Beispielaktion **Create CSV table** angegeben: 

   ![Fertige Aktion „Create CSV table“](./media/logic-apps-perform-data-operations/finished-create-csv-table-action.png)

   Standardmäßig werden mit dieser Aktion die Spalten automatisch basierend auf den Arrayelementen erstellt. 
   Wählen Sie **Erweiterte Optionen anzeigen**, um die Spaltenüberschriften und -werte manuell zu erstellen. 
   Um nur benutzerdefinierte Werte anzugeben, ändern Sie **Spalten** in **Benutzerdefiniert**. 
   Um auch benutzerdefinierte Spaltenüberschriften anzugeben, ändern Sie **Header einschließen** in **Ja**. 

   > [!TIP]
   > Verwenden Sie die Aktion [Parse JSON](#parse-json-action), bevor Sie die Aktion **Create CSV table** aufrufen, um benutzerfreundliche Token für die Eigenschaften in JSON-Objekten zu erstellen. Sie können diese Eigenschaften dann als Eingaben auswählen.

5. Speichern Sie Ihre Logik-App. Wählen Sie auf der Symbolleiste des Designers **Speichern**.

Weitere Informationen zu dieser Aktion in Ihrer zugrunde liegenden Workflowdefinition finden Sie unter [Aktion „Table“](../logic-apps/logic-apps-workflow-actions-triggers.md#table-action).

### <a name="test-your-logic-app"></a>Testen Ihrer Logik-App

Sie können sich vergewissern, dass mit der Aktion **Create CSV table** die erwarteten Ergebnisse erstellt werden, indem Sie an sich selbst eine Benachrichtigung senden, die die Ausgabe der Aktion **Create CSV table** enthält.

1. Fügen Sie in Ihrer Logik-App eine Aktion hinzu, über die die Ergebnisse der Aktion **Create CSV table** an Sie gesendet werden können.

2. Klicken Sie in dieser Aktion auf eine beliebige Position, an der die Ergebnisse angezeigt werden sollen. Wählen Sie nach dem Öffnen der Liste mit den dynamischen Inhalten unter der Aktion **Create CSV table** die Option **Ausgabe**. 

   In diesem Beispiel wird die Aktion **Office 365 Outlook – Send an email** verwendet. Sie enthält das Feld **Ausgabe** im Text der E-Mail:

   ![Feld „Ausgabe“ in der Aktion „Send an email“](./media/logic-apps-perform-data-operations/send-email-create-csv-table-action.png)

3. Führen Sie Ihre Logik-App jetzt manuell aus. Wählen Sie auf der Symbolleiste des Designers die Option **Ausführen**. 

   Hier sind die Ergebnisse angegeben, die Sie basierend auf dem verwendeten E-Mail-Connector erhalten:

   ![E-Mail mit den Ergebnissen der Aktion „Create CSV table“](./media/logic-apps-perform-data-operations/create-csv-table-email-results.png)

<a name="create-html-table-action"></a>

## <a name="create-html-table-action"></a>Aktion „Create HTML table“

Verwenden Sie die Aktion **Datenvorgänge – Create HTML table**, um eine HTML-Tabelle zu erstellen, die die Eigenschaften und Werte aus JSON-Objekten (JavaScript Object Notation) in einem Array enthält. Sie können die sich ergebende Tabelle dann in Aktionen verwenden, die auf die Aktion **Create HTML table** folgen.

Falls Sie den Codeansichts-Editor bevorzugen, können Sie die Aktionsdefinitionen des Beispiels für **Create HTML table** und **Initialize variable** aus diesem Artikel in die zugrunde liegende Workflowdefinition Ihrer eigenen Logik-App kopieren: [Codebeispiele für Datenvorgänge – Create HTML table](../logic-apps/logic-apps-data-operations-code-samples.md#create-html-table-action-example) 

1. Öffnen Sie im <a href="https://portal.azure.com" target="_blank">Azure-Portal</a> oder in Visual Studio Ihre Logik-App im Logik-App-Designer. 

   In diesem Beispiel werden das Azure-Portal und eine Logik-App mit einem **Recurrence**-Trigger und der Aktion **Initialize variable** verwendet. 
   Die Aktion ist auf die Erstellung einer Variablen ausgelegt, deren anfänglicher Wert ein Array mit einigen Eigenschaften und Werten im JSON-Format ist. 
   Wenn Sie Ihre Logik-App zu einem späteren Zeitpunkt testen, können Sie Ihre App manuell ausführen, ohne auf das Auslösen des Triggers zu warten.

   ![Starten des Beispiels für die Logik-App](./media/logic-apps-perform-data-operations/sample-starting-logic-app-create-table-action.png)

2. Führen Sie in Ihrer Logik-App, in der Sie eine HTML-Tabelle erstellen möchten, einen dieser Schritte aus: 

   * Zum Hinzufügen einer Aktion unter dem letzten Schritt wählen Sie **Neuer Schritt** > **Aktion hinzufügen**.

     ![Hinzufügen einer Aktion](./media/logic-apps-perform-data-operations/add-create-table-action.png)

   * Zum Hinzufügen einer Aktion zwischen Schritten bewegen Sie den Mauszeiger über den Verbindungspfeil, sodass das Pluszeichen (+) angezeigt wird. 
   Wählen Sie das Pluszeichen und dann die Option **Aktion hinzufügen**.

3. Geben Sie im Suchfeld den Begriff „create html table“ als Filter ein. Wählen Sie in der Liste mit den Aktionen diese Aktion aus: **HTML-Tabelle erstellen**

   ![Auswählen der Aktion „Create HTML table“](./media/logic-apps-perform-data-operations/select-create-html-table-action.png)

4. Geben Sie im Feld **Von** das Array oder den Ausdruck ein, das bzw. den Sie zum Erstellen der Tabelle verwenden möchten. 

   In diesem Beispiel wird beim Klicken in das Feld **Von** die Liste mit den dynamischen Inhalten angezeigt, damit Sie die zuvor erstellte Variable auswählen können:

   ![Auswählen der Arrayausgabe für die Erstellung der HTML-Tabelle](./media/logic-apps-perform-data-operations/configure-create-html-table-action.png)

   Hier ist die fertige Beispielaktion **Create HTML table** angegeben: 

   ![Fertige Aktion „Create HTML table“](./media/logic-apps-perform-data-operations/finished-create-html-table-action.png)

   Standardmäßig werden mit dieser Aktion die Spalten automatisch basierend auf den Arrayelementen erstellt. 
   Wählen Sie **Erweiterte Optionen anzeigen**, um die Spaltenüberschriften und -werte manuell zu erstellen. 
   Um nur benutzerdefinierte Werte anzugeben, ändern Sie **Spalten** in **Benutzerdefiniert**. 
   Um auch benutzerdefinierte Spaltenüberschriften anzugeben, ändern Sie **Header einschließen** in **Ja**. 

   > [!TIP]
   > Verwenden Sie die Aktion [Parse JSON](#parse-json-action), bevor Sie die Aktion **Create HTML table** aufrufen, um benutzerfreundliche Token für die Eigenschaften in JSON-Objekten zu erstellen. Sie können diese Eigenschaften dann als Eingaben auswählen.

5. Speichern Sie Ihre Logik-App. Wählen Sie auf der Symbolleiste des Designers **Speichern**.

Weitere Informationen zu dieser Aktion in Ihrer zugrunde liegenden Workflowdefinition finden Sie unter [Aktion „Table“](../logic-apps/logic-apps-workflow-actions-triggers.md#table-action).

### <a name="test-your-logic-app"></a>Testen Ihrer Logik-App

Sie können sich vergewissern, dass mit der Aktion **Create HTML table** die erwarteten Ergebnisse erstellt werden, indem Sie an sich selbst eine Benachrichtigung senden, die die Ausgabe der Aktion **Create HTML table** enthält.

1. Fügen Sie in Ihrer Logik-App eine Aktion hinzu, über die die Ergebnisse der Aktion **Create HTML table** an Sie gesendet werden können.

2. Klicken Sie in dieser Aktion auf eine beliebige Position, an der die Ergebnisse angezeigt werden sollen. Wählen Sie nach dem Öffnen der Liste mit den dynamischen Inhalten unter der Aktion **Create HTML table** die Option **Ausgabe**. 

   In diesem Beispiel wird die Aktion **Office 365 Outlook – Send an email** verwendet. Sie enthält das Feld **Ausgabe** im Text der E-Mail:

   ![Feld „Ausgabe“ in der Aktion „Send an email“](./media/logic-apps-perform-data-operations/send-email-create-html-table-action.png)
   
   > [!NOTE]
   > Stellen Sie beim Einbinden der Ausgabe der HTML-Tabelle in eine E-Mail-Aktion sicher, dass Sie die Eigenschaft **Ist HTML** in den erweiterten Optionen der E-Mail-Aktion auf **Ja** festlegen. Die HTML-Tabelle wird dann von der E-Mail-Aktion richtig formatiert.

3. Führen Sie Ihre Logik-App jetzt manuell aus. Wählen Sie auf der Symbolleiste des Designers die Option **Ausführen**. 

   Hier sind die Ergebnisse angegeben, die Sie basierend auf dem verwendeten E-Mail-Connector erhalten:

   ![E-Mail mit den Ergebnissen der Aktion „Create HTML table“](./media/logic-apps-perform-data-operations/create-html-table-email-results.png)

<a name="filter-array-action"></a>

## <a name="filter-array-action"></a>Aktion „Filter array“

Verwenden Sie die Aktion **Datenvorgänge – Filter array**, um aus einem vorhandenen Array ein kleineres Array mit Elementen zu erstellen, die bestimmte Kriterien erfüllen. Anschließend können Sie das gefilterte Array in Aktionen verwenden, die auf die Aktion **Filter array** folgen. 

> [!NOTE]
> Für den Filtertext, den Sie in Ihrer Bedingung verwenden, wird die Groß-/Kleinschreibung berücksichtigt. Mit dieser Aktion können außerdem keine Formate oder Komponenten von Elementen im Array geändert werden. 
> 
> Wenn die Arrayausgabe der Aktion **Filter array** für Aktionen verwendet werden soll, müssen diese Aktionen entweder Arrays als Eingabe akzeptieren, oder Sie müssen die Arrayausgabe ggf. in ein anderes kompatibles Format konvertieren. 

Falls Sie den Codeansichts-Editor bevorzugen, können Sie die Aktionsdefinitionen des Beispiels für **Filter array** und **Initialize variable** aus diesem Artikel in die zugrunde liegende Workflowdefinition Ihrer eigenen Logik-App kopieren: [Codebeispiele für Datenvorgänge – Filter array](../logic-apps/logic-apps-data-operations-code-samples.md#filter-array-action-example) 

1. Öffnen Sie im <a href="https://portal.azure.com" target="_blank">Azure-Portal</a> oder in Visual Studio Ihre Logik-App im Logik-App-Designer. 

   In diesem Beispiel werden das Azure-Portal und eine Logik-App mit einem **Recurrence**-Trigger und der Aktion **Initialize variable** verwendet. 
   Die Aktion ist auf die Erstellung einer Variablen ausgelegt, deren anfänglicher Wert ein Array mit einigen Integer-Beispielen ist. Wenn Sie Ihre Logik-App zu einem späteren Zeitpunkt testen, können Sie Ihre App manuell ausführen, ohne auf das Auslösen des Triggers zu warten.

   > [!NOTE]
   > In diesem Beispiel wird zwar ein einfaches Integer-Array verwendet, aber diese Aktion ist besonders nützlich für JSON-Objektarrays, bei denen Sie basierend auf den Eigenschaften und Werten der Objekte filtern können.

   ![Starten des Beispiels für die Logik-App](./media/logic-apps-perform-data-operations/sample-starting-logic-app-filter-array-action.png)

2. Führen Sie in Ihrer Logik-App, in der Sie das gefilterte Array erstellen möchten, einen dieser Schritte aus: 

   * Zum Hinzufügen einer Aktion unter dem letzten Schritt wählen Sie **Neuer Schritt** > **Aktion hinzufügen**.

     ![Hinzufügen einer Aktion](./media/logic-apps-perform-data-operations/add-filter-array-action.png)

   * Zum Hinzufügen einer Aktion zwischen Schritten bewegen Sie den Mauszeiger über den Verbindungspfeil, sodass das Pluszeichen (+) angezeigt wird. 
   Wählen Sie das Pluszeichen und dann die Option **Aktion hinzufügen**.

3. Geben Sie im Suchfeld „filter array“ als Filter ein. Wählen Sie in der Liste mit den Aktionen diese Aktion aus: **Filter array**

   ![Auswählen der Aktion „Filter array“](./media/logic-apps-perform-data-operations/select-filter-array-action.png)

4. Geben Sie im Feld **Von** das Array oder den Ausdruck an, das bzw. den Sie filtern möchten. 

   In diesem Beispiel wird beim Klicken in das Feld **Von** die Liste mit den dynamischen Inhalten angezeigt, damit Sie die zuvor erstellte Variable auswählen können:

   ![Auswählen der Arrayausgabe für die Erstellung eines gefilterten Arrays](./media/logic-apps-perform-data-operations/configure-filter-array-action.png)

5. Geben Sie für die Bedingung die zu vergleichenden Arrayelemente an, wählen Sie den Vergleichsoperator aus, und geben Sie den Vergleichswert an.

   In diesem Beispiel wird die Funktion **item()** zum Zugreifen auf die einzelnen Elemente im Array verwendet, während mit der Aktion **Filter array** nach Arrayelementen gesucht wird, deren Wert größer als 1 ist:
   
   ![Fertige Aktion „Filter array“](./media/logic-apps-perform-data-operations/finished-filter-array-action.png)

6. Speichern Sie Ihre Logik-App. Wählen Sie auf der Symbolleiste des Designers **Speichern**.

Weitere Informationen zu dieser Aktion in Ihrer zugrunde liegenden Workflowdefinition finden Sie unter [Aktion „Query“](../logic-apps/logic-apps-workflow-actions-triggers.md#query-action).

### <a name="test-your-logic-app"></a>Testen Ihrer Logik-App

Sie können sich vergewissern, dass mit der Aktion **Filter array** die erwarteten Ergebnisse erstellt werden, indem Sie an sich selbst eine Benachrichtigung senden, die die Ausgabe der Aktion **Filter array** enthält.

1. Fügen Sie in Ihrer Logik-App eine Aktion hinzu, über die die Ergebnisse der Aktion **Filter array** an Sie gesendet werden können.

2. Klicken Sie in dieser Aktion auf eine beliebige Position, an der die Ergebnisse angezeigt werden sollen. Wählen Sie **Ausdruck**, wenn die Liste mit den dynamischen Inhalten geöffnet wird. Geben Sie diesen Ausdruck ein, der den Namen der Aktion **Filter array** enthält, um die Arrayausgabe der Aktion **Filter array** zu erhalten:

   ```
   @actionBody('Filter_array')
   ```

   In diesem Beispiel wird die Aktion **Office 365 Outlook – Send an email** verwendet, und die Ausgaben des Ausdrucks **actionBody('Filter_array')** werden in den Text der E-Mail eingebunden:

   ![Aktionsausgaben der Aktion „Send an email“](./media/logic-apps-perform-data-operations/send-email-filter-array-action.png)

3. Führen Sie Ihre Logik-App jetzt manuell aus. Wählen Sie auf der Symbolleiste des Designers die Option **Ausführen**. 

   Hier sind die Ergebnisse angegeben, die Sie basierend auf dem verwendeten E-Mail-Connector erhalten:

   ![E-Mail mit Ergebnissen der Aktion „Filter array“](./media/logic-apps-perform-data-operations/filter-array-email-results.png)

<a name="join-action"></a>

## <a name="join-action"></a>Aktion „Join“

Verwenden Sie die Aktion **Datenvorgänge – Join**, um eine Zeichenfolge zu erstellen, die alle Elemente eines Arrays enthält. Die Elemente sind jeweils mit einem spezifischen Trennzeichen voneinander getrennt. Anschließend können Sie die Zeichenfolge in Aktionen verwenden, die nach der Aktion **Join** folgen.

Falls Sie den Codeansichts-Editor bevorzugen, können Sie die Aktionsdefinitionen des Beispiels für **Join** und **Initialize variable** aus diesem Artikel in die zugrunde liegende Workflowdefinition Ihrer eigenen Logik-App kopieren: [Codebeispiele für Datenvorgänge – Join](../logic-apps/logic-apps-data-operations-code-samples.md#join-action-example) 

1. Öffnen Sie im <a href="https://portal.azure.com" target="_blank">Azure-Portal</a> oder in Visual Studio Ihre Logik-App im Logik-App-Designer. 

   In diesem Beispiel werden das Azure-Portal und eine Logik-App mit einem **Recurrence**-Trigger und der Aktion **Initialize variable** verwendet. 
   Diese Aktion ist auf die Erstellung einer Variablen ausgelegt, deren anfänglicher Wert ein Array mit einigen Integer-Beispielen ist. 
   Wenn Sie Ihre Logik-App zu einem späteren Zeitpunkt testen, können Sie Ihre App manuell ausführen, ohne auf das Auslösen des Triggers zu warten.

   ![Starten des Beispiels für die Logik-App](./media/logic-apps-perform-data-operations/sample-starting-logic-app-join-action.png)

2. Führen Sie in Ihrer Logik-App, in der Sie die Zeichenfolge aus einem Array erstellen möchten, einen dieser Schritte aus: 

   * Zum Hinzufügen einer Aktion unter dem letzten Schritt wählen Sie **Neuer Schritt** > **Aktion hinzufügen**.

     ![Hinzufügen einer Aktion](./media/logic-apps-perform-data-operations/add-join-action.png)

   * Zum Hinzufügen einer Aktion zwischen Schritten bewegen Sie den Mauszeiger über den Verbindungspfeil, sodass das Pluszeichen (+) angezeigt wird. 
   Wählen Sie das Pluszeichen und dann die Option **Aktion hinzufügen**.

3. Geben Sie im Suchfeld den Begriff „join“ als Filter ein. Wählen Sie in der Liste mit den Aktionen diese Aktion aus: **Join**

   ![Auswählen der Aktion „Datenvorgänge – Join“](./media/logic-apps-perform-data-operations/select-join-action.png)

4. Geben Sie im Feld **Von** das Array mit den Elementen an, die Sie als Zeichenfolge verknüpfen möchten. 

   In diesem Beispiel wird beim Klicken in das Feld **Von** die Liste mit den dynamischen Inhalten angezeigt, damit Sie die zuvor erstellte Variable auswählen können:  

   ![Auswählen der Arrayausgabe für die Erstellung der Zeichenfolge](./media/logic-apps-perform-data-operations/configure-join-action.png)

5. Geben Sie im Feld **Verknüpfen mit** das Zeichen ein, das Sie zum Trennen der einzelnen Arrayelemente verwenden möchten. 

   In diesem Beispiel wird ein Doppelpunkt (:) als Trennzeichen verwendet.

   ![Angeben des Trennzeichens](./media/logic-apps-perform-data-operations/finished-join-action.png)

6. Speichern Sie Ihre Logik-App. Wählen Sie auf der Symbolleiste des Designers **Speichern**.

Weitere Informationen zu dieser Aktion in Ihrer zugrunde liegenden Workflowdefinition finden Sie unter [Aktion „Join“](../logic-apps/logic-apps-workflow-actions-triggers.md#join-action).

### <a name="test-your-logic-app"></a>Testen Ihrer Logik-App

Sie können sich vergewissern, dass mit der Aktion **Join** die erwarteten Ergebnisse erstellt werden, indem Sie an sich selbst eine Benachrichtigung senden, die die Ausgabe der Aktion **Join** enthält. 

1. Fügen Sie in Ihrer Logik-App eine Aktion hinzu, über die die Ergebnisse der Aktion **Join** an Sie gesendet werden können.

2. Klicken Sie in dieser Aktion auf eine beliebige Position, an der die Ergebnisse angezeigt werden sollen. Wählen Sie nach dem Öffnen der Liste mit den dynamischen Inhalten unter der Aktion **Join** die Option **Ausgabe**. 

   In diesem Beispiel wird die Aktion **Office 365 Outlook – Send an email** verwendet. Sie enthält das Feld **Ausgabe** im Text der E-Mail:

   ![Feld „Ausgabe“ in der Aktion „Send an email“](./media/logic-apps-perform-data-operations/send-email-join-action.png)

3. Führen Sie Ihre Logik-App jetzt manuell aus. Wählen Sie auf der Symbolleiste des Designers die Option **Ausführen**. 

   Hier sind die Ergebnisse angegeben, die Sie basierend auf dem verwendeten E-Mail-Connector erhalten:

   ![E-Mail mit Ergebnissen der Aktion „Join“](./media/logic-apps-perform-data-operations/join-email-results.png)

<a name="parse-json-action"></a>

## <a name="parse-json-action"></a>Aktion „Parse JSON“

Um auf Eigenschaften in JSON-Inhalten (JavaScript Object Notation) zu verweisen oder darauf zuzugreifen, können Sie benutzerfreundliche Felder oder Token für diese Eigenschaften erstellen, indem Sie die Aktion **Datenvorgänge – Parse JSON** verwenden.
Auf diese Weise können Sie diese Eigenschaften in der Liste mit den dynamischen Inhalten auswählen, wenn Sie die Eingaben für Ihre Logik-App angeben. Für diese Aktion können Sie entweder ein JSON-Schema angeben oder aus dem JSON-Inhalt oder der Nutzlast Ihres Beispiels generieren.

Falls Sie den Codeansichts-Editor bevorzugen, können Sie die Aktionsdefinitionen des Beispiels für **Parse JSON** und **Initialize variable** aus diesem Artikel in die zugrunde liegende Workflowdefinition Ihrer eigenen Logik-App kopieren: [Codebeispiele für Datenvorgänge – Parse JSON](../logic-apps/logic-apps-data-operations-code-samples.md#parse-json-action-example) 

1. Öffnen Sie im <a href="https://portal.azure.com" target="_blank">Azure-Portal</a> oder in Visual Studio Ihre Logik-App im Logik-App-Designer. 

   In diesem Beispiel werden das Azure-Portal und eine Logik-App mit einem **Recurrence**-Trigger und der Aktion **Initialize variable** verwendet. 
   Die Aktion ist auf die Erstellung einer Variablen ausgelegt, deren anfänglicher Wert ein JSON-Objekt mit Eigenschaften und Werten ist. 
   Wenn Sie Ihre Logik-App zu einem späteren Zeitpunkt testen, können Sie Ihre App manuell ausführen, ohne auf das Auslösen des Triggers zu warten.

   ![Starten des Beispiels für die Logik-App](./media/logic-apps-perform-data-operations/sample-starting-logic-app-parse-json-action.png)

2. Führen Sie einen dieser Schritte in Ihrer Logik-App aus, in der Sie die JSON-Inhalte analysieren möchten: 

   * Zum Hinzufügen einer Aktion unter dem letzten Schritt wählen Sie **Neuer Schritt** > **Aktion hinzufügen**.

     ![Hinzufügen einer Aktion](./media/logic-apps-perform-data-operations/add-parse-json-action.png)

   * Zum Hinzufügen einer Aktion zwischen Schritten bewegen Sie den Mauszeiger über den Verbindungspfeil, sodass das Pluszeichen (+) angezeigt wird. 
   Wählen Sie das Pluszeichen und dann die Option **Aktion hinzufügen**.

3. Geben Sie im Suchfeld den Begriff „parse json“ als Filter ein. Wählen Sie in der Liste mit den Aktionen diese Aktion aus: **JSON analysieren**

   ![Auswählen der Aktion „Parse JSON“](./media/logic-apps-perform-data-operations/select-parse-json-action.png)

4. Geben Sie im Feld **Inhalt** die JSON-Inhalte an, die Sie analysieren möchten. 

   In diesem Beispiel wird beim Klicken in das Feld **Inhalt** die Liste mit den dynamischen Inhalten angezeigt, damit Sie die zuvor erstellte Variable auswählen können:

   ![Auswählen des JSON-Objekts für die Aktion „Parse JSON“](./media/logic-apps-perform-data-operations/configure-parse-json-action.png)

5. Geben Sie das JSON-Schema ein, mit dem die von Ihnen analysierten JSON-Inhalte beschrieben werden. 

   Hier ist das JSON-Schema für dieses Beispiel angegeben:

   ![Angeben des JSON-Schemas für das zu analysierende JSON-Objekt](./media/logic-apps-perform-data-operations/provide-schema-parse-json-action.png)

   Falls Sie nicht über dieses Schema verfügen, können Sie es aus den JSON-Inhalten oder der *Nutzlast* generieren, die Sie analysieren. 
   
   1. Wählen Sie in der Aktion **Parse JSON** die Option **Beispielnutzlast zum Generieren eines Schemas verwenden**.

   2. Geben Sie unter **Geben oder fügen Sie eine JSON-Beispielnutzlast ein** die JSON-Inhalte an, und wählen Sie anschließend **Fertig**.

      ![Eingeben der JSON-Inhalte zum Generieren des Schemas](./media/logic-apps-perform-data-operations/generate-schema-parse-json-action.png)

6. Speichern Sie Ihre Logik-App. Wählen Sie auf der Symbolleiste des Designers **Speichern**.

Weitere Informationen zu dieser Aktion in Ihrer zugrunde liegenden Workflowdefinition finden Sie unter [Aktion „Parse JSON“](../logic-apps/logic-apps-workflow-actions-triggers.md).

### <a name="test-your-logic-app"></a>Testen Ihrer Logik-App

Sie können sich vergewissern, dass mit der Aktion **Parse JSON** die erwarteten Ergebnisse erstellt werden, indem Sie an sich selbst eine Benachrichtigung senden, die die Ausgabe der Aktion **Parse JSON** enthält.

1. Fügen Sie in Ihrer Logik-App eine Aktion hinzu, über die die Ergebnisse der Aktion **Parse JSON** an Sie gesendet werden können.

2. Klicken Sie in dieser Aktion auf eine beliebige Position, an der die Ergebnisse angezeigt werden sollen. Wenn die Liste mit den dynamischen Inhalten geöffnet wird, können Sie unter der Aktion **Parse JSON** nun die Eigenschaften aus den analysierten JSON-Inhalten auswählen.

   In diesem Beispiel wird die Aktion **Office 365 Outlook – Send an email** verwendet. Sie enthält die Felder **FirstName** (Vorname), **LastName** (Nachname) und **Email** (E-Mail) im Text der E-Mail:

   ![JSON-Eigenschaften in der Aktion „Send an email“](./media/logic-apps-perform-data-operations/send-email-parse-json-action.png)

   Hier ist die fertige E-Mail-Aktion angegeben:

   ![Fertige E-Mail-Aktion](./media/logic-apps-perform-data-operations/send-email-parse-json-action-2.png)

3. Führen Sie Ihre Logik-App jetzt manuell aus. Wählen Sie auf der Symbolleiste des Designers die Option **Ausführen**. 

   Hier sind die Ergebnisse angegeben, die Sie basierend auf dem verwendeten E-Mail-Connector erhalten:

   ![E-Mail mit Ergebnissen der Aktion „Join“](./media/logic-apps-perform-data-operations/parse-json-email-results.png)

<a name="select-action"></a>

## <a name="select-action"></a>Aktion select

Verwenden Sie die Aktion **Datenvorgänge – Select**, um ein Array mit JSON-Objekten zu erstellen, die aus Werten eines vorhandenen Arrays erstellt wurden. Sie können beispielsweise ein JSON-Objekt für jeden Wert eines Integer-Arrays erstellen, indem Sie Folgendes angeben: die Eigenschaften, über die jedes JSON-Objekt verfügen muss, und die Vorgehensweise beim Zuordnen der Werte im Quellarray zu diesen Eigenschaften. Sie können die Komponenten in diesen JSON-Objekten zwar ändern, aber das Ausgabearray weist immer die gleiche Anzahl von Elementen wie das Quellarray auf.

> [!NOTE]
> Wenn die Arrayausgabe der Aktion **Select** für Aktionen verwendet werden soll, müssen diese Aktionen entweder Arrays als Eingabe akzeptieren, oder Sie müssen die Arrayausgabe ggf. in ein anderes kompatibles Format konvertieren. 

Falls Sie den Codeansichts-Editor bevorzugen, können Sie die Aktionsdefinitionen des Beispiels für **Select** und **Initialize variable** aus diesem Artikel in die zugrunde liegende Workflowdefinition Ihrer eigenen Logik-App kopieren: [Codebeispiele für Datenvorgänge – Select](../logic-apps/logic-apps-data-operations-code-samples.md#select-action-example) 

1. Öffnen Sie im <a href="https://portal.azure.com" target="_blank">Azure-Portal</a> oder in Visual Studio Ihre Logik-App im Logik-App-Designer. 

   In diesem Beispiel werden das Azure-Portal und eine Logik-App mit einem **Recurrence**-Trigger und der Aktion **Initialize variable** verwendet. 
   Die Aktion ist auf die Erstellung einer Variablen ausgelegt, deren anfänglicher Wert ein Array mit einigen Integer-Beispielen ist. 
   Wenn Sie Ihre Logik-App zu einem späteren Zeitpunkt testen, können Sie Ihre App manuell ausführen, ohne auf das Auslösen des Triggers zu warten.

   ![Starten des Beispiels für die Logik-App](./media/logic-apps-perform-data-operations/sample-starting-logic-app-select-action.png)

2. Führen Sie in Ihrer Logik-App, in der Sie das Array erstellen möchten, einen dieser Schritte aus: 

   * Zum Hinzufügen einer Aktion unter dem letzten Schritt wählen Sie **Neuer Schritt** > **Aktion hinzufügen**.

     ![Hinzufügen einer Aktion](./media/logic-apps-perform-data-operations/add-select-action.png)

   * Zum Hinzufügen einer Aktion zwischen Schritten bewegen Sie den Mauszeiger über den Verbindungspfeil, sodass das Pluszeichen (+) angezeigt wird. 
   Wählen Sie das Pluszeichen und dann die Option **Aktion hinzufügen**.

3. Geben Sie im Suchfeld den Begriff „select“ als Filter ein. Wählen Sie in der Liste mit den Aktionen diese Aktion aus: **Auswählen**

   ![Auswählen der Aktion „Select“](./media/logic-apps-perform-data-operations/select-select-action.png)

4. Geben Sie im Feld **Von** das gewünschte Quellarray an.

   In diesem Beispiel wird beim Klicken in das Feld **Von** die Liste mit den dynamischen Inhalten angezeigt, damit Sie die zuvor erstellte Variable auswählen können:

   ![Auswählen des Quellarrays für die Aktion „Select“](./media/logic-apps-perform-data-operations/configure-select-action.png)

5. Geben Sie in der linken Spalte des Felds **Map** (Zuordnen) den Eigenschaftennamen an, dem Sie die einzelnen Werte im Quellarray zuweisen möchten. Geben Sie in der rechten Spalte einen Ausdruck an, der für den Wert steht, den Sie der Eigenschaft zuweisen möchten.

   In diesem Beispiel wird „Product_ID“ als Eigenschaftenname angegeben, um die einzelnen Werte im Integer-Array zuzuweisen. Hierfür wird die Funktion **item()** in einem Ausdruck verwendet, mit dem auf die einzelnen Arrays zugegriffen wird. 

   ![Angeben der JSON-Objekteigenschaft und Werte für das zu erstellende Array](./media/logic-apps-perform-data-operations/configure-select-action-2.png)

   Hier ist die fertige Aktion angegeben:

   ![Fertige Aktion „Select“](./media/logic-apps-perform-data-operations/finished-select-action.png)

6. Speichern Sie Ihre Logik-App. Wählen Sie auf der Symbolleiste des Designers **Speichern**.

Weitere Informationen zu dieser Aktion in Ihrer zugrunde liegenden Workflowdefinition finden Sie unter [Aktion „Select“](../logic-apps/logic-apps-workflow-actions-triggers.md).

### <a name="test-your-logic-app"></a>Testen Ihrer Logik-App

Sie können sich vergewissern, dass mit der Aktion **Select** die erwarteten Ergebnisse erstellt werden, indem Sie an sich selbst eine Benachrichtigung senden, die die Ausgabe der Aktion **Select** enthält.

1. Fügen Sie in Ihrer Logik-App eine Aktion hinzu, über die die Ergebnisse der Aktion **Select** an Sie gesendet werden können.

2. Klicken Sie in dieser Aktion auf eine beliebige Position, an der die Ergebnisse angezeigt werden sollen. Wählen Sie **Ausdruck**, wenn die Liste mit den dynamischen Inhalten geöffnet wird. Geben Sie diesen Ausdruck mit dem Namen der Aktion **Select** ein, um die Arrayausgabe der Aktion **Select** zu erhalten:

   ```
   @actionBody('Select')
   ```

   In diesem Beispiel wird die Aktion **Office 365 Outlook – Send an email** verwendet, und die Ausgaben des Ausdrucks **actionBody('Select')** werden in den Text der E-Mail eingebunden:

   ![Aktionsausgaben der Aktion „Send an email“](./media/logic-apps-perform-data-operations/send-email-select-action.png)

3. Führen Sie Ihre Logik-App jetzt manuell aus. Wählen Sie auf der Symbolleiste des Designers die Option **Ausführen**. 

   Hier sind die Ergebnisse angegeben, die Sie basierend auf dem verwendeten E-Mail-Connector erhalten:

   ![E-Mail mit Ergebnissen der Aktion „Select“](./media/logic-apps-perform-data-operations/select-email-results.png)

## <a name="get-support"></a>Support

* Sollten Sie Fragen haben, besuchen Sie das [Azure Logic Apps-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Wenn Sie Features vorschlagen oder für Vorschläge abstimmen möchten, besuchen Sie die [Website für Logic Apps-Benutzerfeedback](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über [Logic Apps-Connectors](../connectors/apis-list.md).
