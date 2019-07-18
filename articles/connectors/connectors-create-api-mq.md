---
title: Herstellen einer Verbindung mit dem IBM MQ-Server – Azure Logic Apps
description: Senden und Empfangen von Nachrichten mit einem Azure-basierten oder lokalen IBM MQ-Server und Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: valrobb
ms.author: valthom
ms.reviewer: chrishou, LADocs
ms.topic: article
ms.date: 06/19/2019
tags: connectors
ms.openlocfilehash: a2894799946d069916b27a4f5bcc7bd3244705b2
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2019
ms.locfileid: "67273124"
---
# <a name="connect-to-an-ibm-mq-server-from-azure-logic-apps"></a>Herstellen einer Verbindung mit dem IBM MQ-Server über Azure Logic Apps

Der IBM MQ-Connector sendet und empfängt Nachrichten, die lokal oder in Azure auf einem IBM MQ-Server gespeichert sind. Dieser Connector umfasst einen Microsoft-Client zum Kommunizieren mit einem IBM MQ-Remoteserver über ein TCP/IP-Netzwerk. Der vorliegende Artikel ist ein Leitfaden für den Einstieg in die Verwendung des MQ-Connectors. Sie können zuerst eine einzelne Nachricht in einer Warteschlange suchen und anschließend andere Aktionen ausprobieren.

Der IBM MQ-Connector stellt die folgenden Aktionen, jedoch keine Trigger bereit:

- Suchen nach einer einzelnen Nachricht ohne Löschen der Nachricht vom IBM MQ-Server
- Suchen nach einem Batch von Nachrichten ohne Löschen der Nachrichten vom IBM MQ-Server
- Empfangen einer einzelnen Nachricht und Löschen der Nachricht vom IBM MQ-Server
- Empfangen eines Batchs von Nachrichten und Löschen der Nachrichten vom IBM MQ-Server
- Senden einer einzelnen Nachricht an den IBM MQ-Server

## <a name="prerequisites"></a>Voraussetzungen

* Wenn Sie einen lokalen MQ-Server verwenden, [installieren Sie das lokale Datengateway](../logic-apps/logic-apps-gateway-install.md) auf einem Server in Ihrem Netzwerk. Der Server, auf dem das lokale Datengateway installiert ist, muss über .NET Framework 4.6 verfügen, damit der MQ-Connector funktioniert. Außerdem müssen Sie in Azure eine Ressource für das lokale Datengateway erstellen. Weitere Informationen finden Sie unter [Einrichten der Datengatewayverbindung](../logic-apps/logic-apps-gateway-connection.md).

  Wenn Ihr MQ-Server jedoch öffentlich oder in Azure verfügbar ist, müssen Sie das Datengateway nicht verwenden.

* Offiziell unterstützte IBM WebSphere MQ-Versionen:

  * MQ 7.5
  * MQ 8.0
  * MQ 9.0

* Die Logik-App, der Sie die MQ-Aktion hinzufügen möchten. Für diese Logik-App müssen derselbe Standort wie für die Verbindung mit dem lokalen Datengateway und ein bereits vorhandener Trigger verwendet werden, der den Workflow startet. 

  Da der MQ-Connector keine Trigger bereitstellt, müssen Sie Ihrer Logik-App zuerst einen hinzufügen. Sie können beispielsweise den Wiederholungstrigger verwenden. Falls Sie noch nicht mit Logik-Apps gearbeitet haben, lesen Sie zuerst den [Schnellstart zum Erstellen Ihrer ersten Logik-App](../logic-apps/quickstart-create-first-logic-app-workflow.md). 

## <a name="browse-a-single-message"></a>Suchen einer einzelnen Nachricht

1. Klicken Sie in Ihrer Logik-App unter dem Trigger oder einer anderen Aktion auf **Neuer Schritt**. 

1. Geben Sie im Suchfeld „mq“ ein, und klicken Sie auf die folgende Aktion: **Nachricht suchen**

   ![Nachricht suchen](media/connectors-create-api-mq/Browse_message.png)

1. Wenn noch keine MQ-Verbindung besteht, müssen Sie diese zuerst herstellen:  

   1. Klicken Sie innerhalb der Aktion auf **Über lokales Datengateway verbinden**.
   
   1. Geben Sie die Eigenschaften für Ihren MQ-Server ein.  

      Für **Server** können Sie den Namen des MQ-Server oder die IP-Adresse gefolgt von einem Doppelpunkt und der Portnummer eingeben.
    
   1. Öffnen Sie die Liste **Gateway**. Diese enthält alle bereits konfigurierten Gatewayverbindungen. Wählen Sie Ihr Gateway aus.
    
   1. Wenn Sie fertig sind, wählen Sie **Erstellen** aus. 
   
      Nun sieht Ihre Verbindungskonfiguration wie im folgendem Beispiel aus:

      ![Verbindungseigenschaften](media/connectors-create-api-mq/Connection_Properties.png)

1. Richten Sie die folgenden Aktionseigenschaften ein:

   * **Queue:** Geben Sie eine Warteschlange an, die nicht für die Verbindung verwendet wird.

   * **MessageId**, **CorrelationId**, **GroupId** und weitere Eigenschaften: Legen Sie Werte für die verschiedenen MQ-Nachrichteneigenschaften fest, um nach Nachrichten mit diesen Eigenschaften zu suchen.

   * **IncludeInfo**: Legen Sie **True** fest, wenn Sie zusätzliche Nachrichteninformationen in die Ausgabe einschließen möchten. Legen Sie andernfalls **False** fest.

   * **Timeout**: Geben Sie einen Wert ein, um festzulegen, wie lange in einer leeren Warteschlange auf den Empfang einer Nachricht gewartet wird. Wenn hier kein Wert festgelegt ist, wird die erste Nachricht in der Warteschlange abgerufen, und es wird nicht gewartet, bis eine Nachricht eingeht.

     ![Eigenschaften für das Suchen nach Nachrichten](media/connectors-create-api-mq/Browse_message_Props.png)

1. **Speichern** Sie Ihre Änderungen, und klicken Sie dann auf **Ausführen**, um Ihre Logik-App auszuführen.

   ![Speichern und ausführen](media/connectors-create-api-mq/Save_Run.png)

   Anschließend werden die Ausführungsschritte angezeigt, und Sie können sich die Ausgabe ansehen.

1. Klicken Sie auf das grüne Häkchen, um sich Details zu einem Schritt anzeigen zu lassen. Wenn Sie weitere Informationen zu Ausgabedaten aufrufen möchten, klicken Sie auf **Unformatierte Ausgaben anzeigen**.

   ![Ausgabe für die Suche nach einer Nachricht](media/connectors-create-api-mq/Browse_message_output.png)  

   Im Folgenden sehen Sie ein Beispiel für eine unformatierte Ausgabe:

   ![Unformatierte Ausgabe für die Suche nach einer Nachricht](media/connectors-create-api-mq/Browse_message_raw_output.png)

1. Wenn Sie **IncludeInfo** auf „true“ festlegen, wird die folgende Ausgabe angezeigt:

   ![Informationen für die Suche nach einer Nachricht einschließen](media/connectors-create-api-mq/Browse_message_Include_Info.png)

## <a name="browse-multiple-messages"></a>Suche nach mehreren Nachrichten

Die Aktion **Nachrichten suchen** umfasst eine Option **BatchSize**, mit der Sie angeben können, wie viele Nachrichten aus der Warteschlange zurückgegeben werden sollen.  Wenn **BatchSize** keinen Wert enthält, werden alle Nachrichten zurückgegeben. Die zurückgegebene Ausgabe ist ein Array aus Nachrichten.

1. Wenn Sie die Aktion **Nachrichten suchen** hinzufügen, wird standardmäßig die zuletzt konfigurierte Verbindung ausgewählt. Klicken Sie auf **Verbindung ändern**, um eine neue Verbindung zu erstellen. Alternativ können Sie auch eine andere Verbindung auswählen.

1. Im Folgenden sehen Sie eine Beispielausgabe für die Aktion **Nachrichten suchen**. Die Ausgabe wird nach der Ausführung der Logik-App angezeigt:

   ![Ausgabe für die Suche nach Nachrichten](media/connectors-create-api-mq/Browse_messages_output.png)

## <a name="receive-single-message"></a>Empfangen einer einzelnen Nachricht

Die Aktion **Nachricht empfangen** umfasst dieselben Eingaben und Ausgaben wie die Aktion **Nachricht suchen**. Bei Verwendung von **Nachricht empfangen** wird die Nachricht aus der Warteschlange gelöscht.

## <a name="receive-multiple-messages"></a>Empfangen mehrerer Nachrichten

Die Aktion **Nachrichten empfangen** umfasst dieselben Eingaben und Ausgaben wie die Aktion **Nachrichten suchen**. Bei Verwendung von **Nachrichten empfangen** werden die Nachrichten aus der Warteschlange gelöscht.

Wenn beim Ausführen der Aktion „Nachrichten suchen“ oder „Nachrichten empfangen“ keine Nachrichten in der Warteschlange vorhanden sind, kommt es zu einem Fehler mit der folgenden Ausgabe:  

![MQ-Fehler, wenn keine Nachricht vorhanden ist](media/connectors-create-api-mq/MQ_No_Msg_Error.png)

## <a name="send-message"></a>Nachricht senden

Wenn Sie die Aktion **Nachrichten senden** hinzufügen, wird standardmäßig die zuletzt konfigurierte Verbindung ausgewählt. Klicken Sie auf **Verbindung ändern**, um eine neue Verbindung zu erstellen. Alternativ können Sie auch eine andere Verbindung auswählen.

1. Wählen Sie einen der folgenden gültigen Nachrichtentypen aus: **Datagramm**, **Antwort** oder **Anforderung**.  

   ![Nachrichteneigenschaften senden](media/connectors-create-api-mq/Send_Msg_Props.png)

1. Im Folgenden sehen Sie eine Beispielausgabe für die Aktion **Nachrichten senden**. Die Ausgabe wird nach der Ausführung der Logik-App angezeigt:

   ![Ausgabe von „Nachricht senden“](media/connectors-create-api-mq/Send_Msg_Output.png)

## <a name="connector-reference"></a>Connector-Referenz

Technische Details zu Aktionen und Einschränkungen, die in der OpenAPI-Beschreibung (ehemals Swagger-Beschreibung) des Connectors aufgeführt sind, finden Sie auf der [Referenzseite des Connectors](/connectors/mq/).

## <a name="next-steps"></a>Nächste Schritte

* Informationen zu anderen [Logic Apps-Connectors](../connectors/apis-list.md)
