---
title: Herstellen einer Verbindung mit OneDrive – Azure Logic Apps | Microsoft-Dokumentation
description: Hochladen und Verwalten von Dateien mit OneDrive-REST-APIs und Azure Logic Apps
author: ecfan
manager: jeconnoc
ms.author: estfan
ms.date: 10/18/2016
ms.topic: article
ms.service: logic-apps
services: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
tags: connectors
ms.openlocfilehash: 840a010f8606387a250552d884621a96d0031f90
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "62106226"
---
# <a name="get-started-with-the-onedrive-connector"></a>Erste Schritte mit dem OneDrive-Connector
Stellen Sie eine Verbindung mit OneDrive her, um Ihre Dateien zu verwalten, d. h. diese hochzuladen, abzurufen, zu löschen usw. 

OneDrive ermöglicht Folgendes: 

* Erstellen Ihres Workflows durch Speichern von Dateien in OneDrive oder Aktualisieren vorhandener Dateien in OneDrive. 
* Verwenden von Triggern, um den Workflow zu starten, wenn eine Datei in OneDrive erstellt oder aktualisiert wird.
* Verwenden von Aktionen, um z. B. eine Datei zu kopieren oder zu löschen. So kann beispielsweise bei Eingang einer neuen Office 365-E-Mail mit einer Anlage (Trigger) eine neue Datei in OneDrive erstellt werden (Aktion).

Dieser Artikel beschreibt, wie Sie den OneDrive-Connector in einer Logik-App verwenden, und enthält eine Liste mit den Triggern und Aktionen.

Weitere Informationen zu Logik-Apps finden Sie unter [Was sind Logik-Apps](../logic-apps/logic-apps-overview.md) sowie unter [Erstellen einer Logik-App zum Verbinden von SaaS-Diensten](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="connect-to-onedrive"></a>Herstellen einer Verbindung mit OneDrive
Damit Ihre Logik-App überhaupt auf einen Dienst zugreifen kann, muss zunächst eine *Verbindung* mit dem Dienst hergestellt werden. Eine Verbindung stellt den Kontakt zwischen einer Logik-App und einem anderen Dienst her. Wenn Sie also beispielsweise eine Verbindung mit OneDrive herstellen möchten, müssen Sie zunächst eine entsprechende *Verbindung* erstellen. Geben Sie zum Erstellen einer Verbindung die Anmeldeinformationen ein, mit denen Sie normalerweise auf den Dienst zugreifen, mit dem Sie eine Verbindung herstellen möchten. Geben Sie für OneDrive also die Anmeldeinformationen Ihres OneDrive-Kontos ein, um die Verbindung zu erstellen.

### <a name="create-the-connection"></a>Erstellen der Verbindung
> [!INCLUDE [Steps to create a connection to OneDrive](../../includes/connectors-create-api-onedrive.md)]
> 
> 

## <a name="use-a-trigger"></a>Verwenden eines Triggers
Ein Trigger ist ein Ereignis, mit dem ein in einer Logik-App definierter Workflow gestartet werden kann. Trigger fragen den Dienst im gewünschten Intervall und mit der gewünschten Häufigkeit ab. Weitere Informationen zu Triggern finden Sie [hier](../logic-apps/logic-apps-overview.md#logic-app-concepts).

1. Geben Sie in der Logik-App „onedrive“ ein, um eine Liste mit den Triggern abzurufen:  
   
    ![](./media/connectors-create-api-onedrive/onedrive-1.png)
2. Wählen Sie **When a file is modified** (Wenn eine Datei geändert wird) aus. Ist bereits eine Verbindung vorhanden, wählen Sie die Schaltfläche „Auswahl anzeigen“ aus, um einen Ordner auszuwählen.
   
    ![](./media/connectors-create-api-onedrive/sample-folder.png)
   
    Wenn Sie zur Anmeldung aufgefordert werden, geben Sie die Anmeldeinformationen ein, um die Verbindung zu erstellen. Die erforderlichen Schritte sind in diesem Artikel unter [Erstellen der Verbindung](connectors-create-api-onedrive.md#create-the-connection) aufgeführt. 
   
   > [!NOTE]
   > In diesem Beispiel wird die Logik-App ausgeführt, wenn eine Datei im ausgewählten Ordner aktualisiert wird. Fügen Sie zum Anzeigen der Ergebnisse dieses Triggers eine weitere Aktion hinzu, die Ihnen eine E-Mail sendet. Fügen Sie beispielsweise die Office 365 Outlook-Aktion *E-Mail senden* hinzu, um eine E-Mail zu erhalten, wenn eine Datei aktualisiert wird. 

3. Wählen Sie die Schaltfläche **Bearbeiten** aus, und legen Sie Werte für **Häufigkeit** und **Intervall** fest. Falls die Abfrage durch den Trigger also beispielsweise alle 15 Minuten erfolgen soll, legen Sie **Häufigkeit** auf **Minute** und **Intervall** auf **15** fest. 
   
    ![](./media/connectors-create-api-onedrive/trigger-properties.png)
4. Speichern Sie Ihre Änderungen. (Die Option **Speichern** befindet sich links oben auf der Symbolleiste.) Ihre Logik-App wird gespeichert und ggf. automatisch aktiviert.

## <a name="use-an-action"></a>Verwenden einer Aktion
Eine Aktion ist ein Vorgang, der durch den in einer Logik-App definierten Workflow ausgeführt wird. Weitere Informationen zu Aktionen finden Sie [hier](../logic-apps/logic-apps-overview.md#logic-app-concepts).

1. Wählen Sie das Pluszeichen aus. Sie sehen mehrere Auswahlmöglichkeiten: **Aktion hinzufügen**, **Bedingung hinzufügen** oder eine der Optionen unter **Mehr**.
   
    ![](./media/connectors-create-api-onedrive/add-action.png)
2. Wählen Sie **Aktion hinzufügen**aus.
3. Geben Sie im Textfeld die Zeichenfolge „onedrive“ ein, um eine Liste mit allen verfügbaren Aktionen zu erhalten.
   
    ![](./media/connectors-create-api-onedrive/onedrive-actions.png) 
4. In unserem Beispiel wählen wir **OneDrive - Create file** (OneDrive – Datei erstellen) aus. Falls bereits eine Verbindung vorhanden ist, wählen Sie den Pfad des **Zielordners** für die Datei aus, geben Sie den **Dateinamen** ein, und wählen Sie den gewünschten **Dateiinhalt** aus:  
   
    ![](./media/connectors-create-api-onedrive/sample-action.png)
   
    Wenn Sie zur Eingabe der Verbindungsinformationen aufgefordert werden, geben Sie die Details ein, um die Verbindung zu erstellen. Die Eigenschaften werden in diesem Artikel unter [Erstellen der Verbindung](connectors-create-api-onedrive.md#create-the-connection) beschrieben. 
   
   > [!NOTE]
   > In diesem Beispiel erstellen wir eine neue Datei in einem OneDrive-Ordner. Für die Erstellung der OneDrive-Datei kann die Ausgabe eines anderen Triggers verwendet werden. Fügen Sie beispielsweise den Office 365 Outlook-Trigger *When a new email arrives* (Wenn eine neue E-Mail eingeht) hinzu. Fügen Sie dann die OneDrive-Aktion *Datei erstellen* hinzu, die die Felder „Anlagen“ und „Inhaltstyp“ innerhalb einer ForEach-Schleife verwendet, um die neue Datei in OneDrive zu erstellen. 
   > 
   > ![](./media/connectors-create-api-onedrive/foreach-action.png)

5. Speichern Sie Ihre Änderungen. (Die Option **Speichern** befindet sich links oben auf der Symbolleiste.) Ihre Logik-App wird gespeichert und ggf. automatisch aktiviert.


## <a name="connector-specific-details"></a>Connectorspezifische Details

Zeigen Sie die in Swagger definierten Trigger und Aktionen sowie mögliche Beschränkungen in den [Connectordetails](/connectors/onedriveconnector/) an.

## <a name="more-connectors"></a>Weitere Connectors
Gehen Sie zur [Liste der APIs](apis-list.md)zurück.