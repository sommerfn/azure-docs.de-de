---
title: Herstellen einer Verbindung mit Azure-Blobspeicher – Azure Logic Apps
description: Erstellen und Verwalten von Blobs in Azure-Speicher mit Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
manager: carmonm
ms.reviewer: klam, LADocs
ms.topic: conceptual
ms.date: 06/20/2019
tags: connectors
ms.openlocfilehash: 98a811508d5fa65135c224536b668145ea0808d0
ms.sourcegitcommit: 42748f80351b336b7a5b6335786096da49febf6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2019
ms.locfileid: "72176066"
---
# <a name="create-and-manage-blobs-in-azure-blob-storage-with-azure-logic-apps"></a>Erstellen und Verwalten von Blobs in Azure-Blobspeicher mit Azure Logic Apps

In diesem Artikel wird veranschaulicht, wie Sie auf Dateien zugreifen und diese verwalten können, die als Blobs in Ihrem Azure-Speicherkonto gespeichert sind, indem Sie eine Logik-App und den Azure Blob Storage-Connector verwenden. Auf diese Weise können Sie Logik-Apps erstellen, mit denen Aufgaben und Workflows für das Verwalten Ihrer Dateien automatisiert werden. Beispielsweise können Sie Logik-Apps erstellen, mit denen Dateien in Ihrem Speicherkonto erstellt, abgerufen, aktualisiert und gelöscht werden.

Angenommen, Sie verfügen über ein Tool, das auf einer Azure-Website aktualisiert wird, die als Trigger für Ihre Logik-App fungiert. Wenn dieses Ereignis eintritt, können Sie über Ihre Logik-App eine Datei in Ihrem Blobspeichercontainer aktualisieren. Hierbei handelt es sich um eine Aktion in Ihrer Logik-App.

> [!IMPORTANT]
>
> Logik-Apps können nicht direkt auf Azure-Speicherkonten zugreifen, die über [Firewallregeln](../storage/common/storage-network-security.md) verfügen und in derselben Region vorhanden sind. Wenn Sie jedoch die [ausgehenden IP-Adressen für verwaltete Connectors in Ihrer Region](../logic-apps/logic-apps-limits-and-config.md#outbound) zulassen, können Logik-Apps auf Speicherkonten in einer anderen Region zugreifen, außer wenn Sie den Azure Table Storage-Connector oder den Azure Queue Storage-Connector verwenden. Um auf ihren Table Storage oder Queue Storage zuzugreifen, können Sie weiterhin HTTP-Trigger und -Aktionen verwenden. 
> Andernfalls können Sie auch hier die erweiterten Optionen verwenden:
> 
> * Erstellen Sie eine [Integrationsdienstumgebung](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md), die eine Verbindung mit Ressourcen in einem virtuellen Azure-Netzwerk herstellen kann.
>
> * Wenn Sie für API Management einen Dedicated-Tarif verwenden, können Sie die Speicher-API als Front-End verwenden, indem Sie API Management verwenden und dessen IP-Adressen das Passieren der Firewall gestatten. Im Grunde fügen Sie das virtuelle Azure-Netzwerk, das von API Management verwendet wird, der Firewalleinstellung des Speicherkontos hinzu. Sie können dann entweder die API Management-Aktion oder die HTTP-Aktion verwenden, um die Azure Storage-APIs aufzurufen. Wenn Sie diese Option auswählen, müssen Sie den Authentifizierungsprozess jedoch selbst handhaben. Weitere Informationen finden Sie unter [Einfache Unternehmensintegrationsarchitektur](https://aka.ms/aisarch).

Falls Sie noch nicht mit Logik-Apps vertraut sind, finden Sie weitere Informationen unter [Was ist Azure Logic Apps?](../logic-apps/logic-apps-overview.md) und [Schnellstart: Erstellen Ihres ersten automatisierten Workflows mit Azure Logic Apps – Azure-Portal](../logic-apps/quickstart-create-first-logic-app-workflow.md). Connectorspezifische technische Informationen finden Sie in der [Referenz zu Azure Blob Storage](/connectors/azureblobconnector/).

## <a name="limits"></a>Einschränkungen

* Standardmäßig können Azure Blob Storage-Aktionen Dateien mit einer Größe von *50 MB oder kleiner* lesen oder schreiben. Zum Verarbeiten von Dateien, die größer als 50 MB und maximal 1.024 MB groß sind, unterstützen Azure Blob Storage-Aktionen [Nachrichtensegmentierung](../logic-apps/logic-apps-handle-large-messages.md). Die Aktion **Get blob content** (Blobinhalt abrufen) verwendet implizit die Blockerstellung.

* Azure Blob Storage-Trigger unterstützen keine Segmentierung. Trigger wählen beim Anfordern von Dateiinhalten nur Dateien aus, die 50 MB oder kleiner sind. Befolgen Sie das folgende Muster, um Dateien abzurufen, die größer als 50 MB sind:

  * Verwenden Sie einen Azure Blob Storage-Trigger, der Dateieigenschaften zurückgibt, z.B. **When a blob is added or modified (properties only)** (Beim Hinzufügen oder Ändern eines Blobs (nur Eigenschaften)).

  * Lassen Sie auf den Trigger die Azure Blob Storage-Aktion **Get blob content** (Blobinhalt abrufen) folgen, die die vollständige Datei liest und implizit Segmentierung verwendet.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie sich [für ein kostenloses Azure-Konto registrieren](https://azure.microsoft.com/free/).

* Ein [Azure-Speicherkonto und -Speichercontainer](../storage/blobs/storage-quickstart-blobs-portal.md)

* Die Logik-App, für die Sie Zugriff auf Ihr Azure-Blobspeicherkonto benötigen. Um Ihre Logik-App mit einem Azure Blob Storage-Trigger starten zu können, benötigen Sie eine [leere Logik-App](../logic-apps/quickstart-create-first-logic-app-workflow.md).

<a name="add-trigger"></a>

## <a name="add-blob-storage-trigger"></a>Hinzufügen eines Blob Storage-Triggers

In Azure Logic Apps muss jede Logik-App mit einem [Trigger](../logic-apps/logic-apps-overview.md#logic-app-concepts) beginnen, der ausgelöst wird, wenn ein bestimmtes Ereignis eintritt oder eine bestimmte Bedingung erfüllt wird. Bei jeder Auslösung des Triggers erstellt das Logic Apps-Modul eine Logik-App-Instanz und startet die Ausführung des Workflows Ihrer App.

In diesem Beispiel wird veranschaulicht, wie Sie einen Logik-App-Workflow mit dem Trigger **When a blob is added or modified (properties only)** (Beim Hinzufügen oder Ändern eines Blobs (nur Eigenschaften)) starten können, wenn die Eigenschaften eines Blobs in Ihrem Speichercontainer hinzugefügt oder aktualisiert werden.

1. Erstellen Sie im [Azure-Portal](https://portal.azure.com) oder in Visual Studio eine leere Logik-App, die den Logik-App-Designer öffnet. In diesem Beispiel wird das Azure-Portal verwendet.

2. Geben Sie im Suchfeld „azure blob“ als Filter ein. Wählen Sie in der Triggerliste den gewünschten Trigger aus.

   In diesem Beispiel wird folgender Trigger verwendet: **When a blob is added or modified (properties only)** (Beim Hinzufügen oder Ändern eines Blobs (nur Eigenschaften))

   ![Trigger auswählen](./media/connectors-create-api-azureblobstorage/azure-blob-trigger.png)

3. Wenn Sie zum Angeben von Verbindungsdetails aufgefordert werden, können Sie [jetzt Ihre Blobspeicherverbindung erstellen](#create-connection). Falls die Verbindung bereits besteht, können Sie die erforderlichen Informationen für den Trigger angeben.

   Wählen Sie für dieses Beispiel den Container und Ordner aus, den Sie überwachen möchten.

   1. Wählen Sie im Feld **Container** das Ordnersymbol.

   2. Wählen Sie in der Ordnerliste den Pfeil nach rechts ( **>** ), und navigieren Sie zum gewünschten Ordner.

      ![Ordner auswählen](./media/connectors-create-api-azureblobstorage/trigger-select-folder.png)

   3. Wählen Sie das Intervall und die Häufigkeit aus, um anzugeben, wie oft der Trigger den Ordner auf Änderungen überprüfen soll.

4. Wenn Sie fertig sind, wählen Sie auf der Symbolleiste des Designers die Option **Speichern** aus.

5. Fahren Sie nun damit fort, der Logik-App weitere Aktionen für die Aufgaben hinzuzufügen, die anhand der Triggerergebnisse durchgeführt werden sollen.

<a name="add-action"></a>

## <a name="add-blob-storage-action"></a>Hinzufügen einer Blobspeicheraktion

In Azure Logic Apps handelt es sich bei einer [Aktion](../logic-apps/logic-apps-overview.md#logic-app-concepts) um einen Schritt in Ihrem Workflow, der einem Trigger oder einer anderen Aktion folgt. Bei diesem Beispiel beginnt die Logik-App mit dem [Trigger „Wiederholung“](../connectors/connectors-native-recurrence.md).

1. Öffnen Sie im [Azure-Portal](https://portal.azure.com) oder in Visual Studio Ihre Logik-App im Logik-App-Designer. In diesem Beispiel wird das Azure-Portal verwendet.

2. Wählen Sie im Logik-App-Designer unter dem Trigger oder der Aktion die Option **Neuer Schritt** aus.

   ![Hinzufügen einer Aktion](./media/connectors-create-api-azureblobstorage/add-action.png) 

   Um eine Aktion zwischen vorhandenen Schritten hinzuzufügen, bewegen Sie den Mauszeiger über den Verbindungspfeil. Wählen Sie das daraufhin angezeigte Pluszeichen ( **+** ) und dann **Aktion hinzufügen** aus.

3. Geben Sie im Suchfeld „azure blob“ als Filter ein. Wählen Sie in der Liste mit den Aktionen die gewünschte Aktion aus.

   In diesem Beispiel wird diese Aktion verwendet: **Get blob content** (Blobinhalt abrufen)

   ![Aktion select](./media/connectors-create-api-azureblobstorage/azure-blob-action.png)

4. Wenn Sie zum Angeben von Verbindungsdetails aufgefordert werden, können Sie [jetzt Ihre Azure Blob Storage-Verbindung erstellen](#create-connection).
Falls Ihre Verbindung bereits besteht, können Sie die erforderlichen Informationen für die Aktion angeben.

   Wählen Sie für dieses Beispiel die gewünschte Datei aus.

   1. Wählen Sie im Feld **Blob** das Ordnersymbol.
  
      ![Ordner auswählen](./media/connectors-create-api-azureblobstorage/action-select-folder.png)

   2. Suchen Sie anhand der **ID**-Nummer des Blobs nach der gewünschten Datei, und wählen Sie sie aus. Sie finden diese **ID**-Nummer in den Blobmetadaten, die vom oben beschriebenen Blobspeichertrigger zurückgegeben werden.

5. Wenn Sie fertig sind, wählen Sie auf der Symbolleiste des Designers die Option **Speichern** aus.
Stellen Sie zum Testen Ihrer Logik-App sicher, dass der ausgewählte Ordner ein Blob enthält.

In diesem Beispiel wird nur der Inhalt für ein Blob abgerufen. Fügen Sie zum Anzeigen des Inhalts eine weitere Aktion hinzu, die mit dem Blob eine Datei erstellt, indem ein anderer Connector verwendet wird. Fügen Sie beispielsweise eine OneDrive-Aktion hinzu, die basierend auf dem Blobinhalt eine Datei erstellt.

<a name="create-connection"></a>

## <a name="connect-to-storage-account"></a>Herstellen einer Verbindung mit dem Speicherkonto

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

[!INCLUDE [Create a connection to Azure blob storage](../../includes/connectors-create-api-azureblobstorage.md)]

## <a name="connector-reference"></a>Connector-Referenz

Technische Details wie Trigger, Aktionen und Limits, wie sie in der OpenAPI-Datei (ehemals Swagger) des Connectors beschrieben werden, finden Sie auf der [Referenzseite des Connectors](/connectors/azureblobconnector/).

## <a name="next-steps"></a>Nächste Schritte

* Informationen zu anderen [Logic Apps-Connectors](../connectors/apis-list.md)
