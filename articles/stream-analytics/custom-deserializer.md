---
title: Benutzerdefinierte .NET-Deserialisierer für Azure Stream Analytics-Cloudaufträge
description: In diesem Tutorial erfahren Sie, wie Sie mithilfe von Visual Studio einen benutzerdefinierten .NET-Deserialisierer für einen Azure Stream Analytics-Cloudauftrag erstellen.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: tutorial
ms.date: 05/06/2019
ms.openlocfilehash: 56d0927bff928929ca55433a812dfe6c1405a738
ms.sourcegitcommit: 92d42c04e0585a353668067910b1a6afaf07c709
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/28/2019
ms.locfileid: "72991644"
---
# <a name="tutorial-custom-net-deserializers-for-azure-stream-analytics"></a>Tutorial: Benutzerdefinierte .NET-Deserialisierer für Azure Stream Analytics

Azure Stream Analytics verfügt über [integrierte Unterstützung von drei Datenformaten](stream-analytics-parsing-json.md): JSON, CSV und Avro. Mit benutzerdefinierten .NET-Deserialisierern können Sie Daten aus anderen Formaten lesen. Hierzu zählen unter anderem [Protokollpuffer](https://developers.google.com/protocol-buffers/), [Bond](https://github.com/Microsoft/bond) und andere benutzerdefinierte Formate für Cloud- und Edgeaufträge.

In diesem Tutorial erfahren Sie, wie Sie mithilfe von Visual Studio einen benutzerdefinierten .NET-Deserialisierer für einen Azure Stream Analytics-Cloudauftrag erstellen. 

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines benutzerdefinierten Deserialisierers für Protokollpuffer
> * Erstellen eines Azure Stream Analytics-Auftrags in Visual Studio
> * Konfigurieren Ihres Stream Analytics-Auftrags für die Verwendung des benutzerdefinierten Deserialisierers
> * Lokales Ausführen Ihres Stream Analytics-Auftrags, um den benutzerdefinierten Deserialisierer zu testen

## <a name="prerequisites"></a>Voraussetzungen

* Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen.

* Installieren Sie [Visual Studio 2017](https://www.visualstudio.com/downloads/) oder [Visual Studio 2015](https://www.visualstudio.com/vs/older-downloads/). Die Editionen Enterprise (Ultimate/Premium), Professional und Community werden unterstützt. Die Express-Edition wird nicht unterstützt.

* [Installieren Sie die Stream Analytics-Tools für Visual Studio](stream-analytics-tools-for-visual-studio-install.md), oder führen Sie ein Update auf die neueste Version durch. Die folgenden Versionen von Visual Studio werden unterstützt:
   * Visual Studio 2015
   * Visual Studio 2017

* Öffnen Sie **Cloud-Explorer** in Visual Studio, und melden Sie sich bei Ihrem Azure-Abonnement an.

* Erstellen Sie einen Container in Ihrem Azure Storage-Konto.
Der Container, den Sie erstellen, dient zum Speichern von Objekten im Zusammenhang mit Ihrem Stream Analytics-Auftrag. Wenn Sie bereits über ein Speicherkonto mit vorhandenen Containern verfügen, können Sie diese verwenden. Wenn nicht, müssen Sie einen [neuen Container erstellen](../storage/blobs/storage-quickstart-blobs-portal.md).

## <a name="create-a-custom-deserializer"></a>Erstellen eines benutzerdefinierten Deserialisierers

1. Öffnen Sie Visual Studio, und wählen Sie **Datei > Neu > Projekt** aus. Suchen Sie nach **Stream Analytics**, und wählen Sie **Azure Stream Analytics Custom Deserializer Project (.NET)** (Azure Stream Analytics: Projekt für benutzerdefinierten Deserialisierer (.NET)) aus. Geben Sie dem Projekt einen Namen (beispielsweise **Protobuf Deserializer**).

   ![Erstellen eines .NET-Standardklassenbibliothek-Projekts in Visual Studio](./media/custom-deserializer/create-dotnet-library-project.png)

2. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt **Protobuf Deserializer**, und wählen Sie im Menü die Option **NuGet-Pakete verwalten** aus. Installieren Sie anschließend die NuGet-Pakete **Microsoft.Azure.StreamAnalytics** und **Google.Protobuf**.

3. Fügen Sie Ihrem Projekt die Klassen [MessageBodyProto](https://github.com/Azure/azure-stream-analytics/blob/master/CustomDeserializers/Protobuf/MessageBodyProto.cs) und [MessageBodyDeserializer](https://github.com/Azure/azure-stream-analytics/blob/master/CustomDeserializers/Protobuf/MessageBodyDeserializer.cs) hinzu.

4. Erstellen Sie das Projekt **Protobuf Deserializer**.

## <a name="add-an-azure-stream-analytics-project"></a>Hinzufügen eines Azure Stream Analytics-Projekts

1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf die Projektmappe **Protobuf Deserializer**, und wählen Sie **Hinzufügen > Neues Projekt** aus. Wählen Sie unter **Azure Stream Analytics > Stream Analytics** die Option **Azure Stream Analytics Application** (Azure Stream Analytics-Anwendung) aus. Nennen Sie sie **ProtobufCloudDeserializer**, und wählen Sie **OK** aus. 

2. Klicken Sie unter dem Azure Stream Analytics-Projekt **ProtobufCloudDeserializer** mit der rechten Maustaste auf **Verweise**. Fügen Sie unter **Projekte** das Projekt **Protobuf Deserializer** hinzu. Es sollte automatisch für Sie aufgefüllt werden.

## <a name="configure-a-stream-analytics-job"></a>Konfigurieren eines Stream Analytics-Auftrags

1. Doppelklicken Sie auf **JobConfig.json**. Verwenden Sie die folgenden Einstellungen, und behalten Sie ansonsten die Standardkonfiguration bei:

   |Einstellung|Empfohlener Wert|
   |-------|---------------|
   |Global Storage Settings Resource (Ressource für globale Speichereinstellungen)|Datenquelle aus aktuellem Konto wählen|
   |Global Storage Settings Subscription (Abonnement für globale Speichereinstellungen)| <Ihr Abonnement>|
   |Global Storage Settings Storage Account (Speicherkonto für globale Speichereinstellungen)| <Ihr Speicherkonto>|
   |Custom Code Storage Settings Resource (Ressource für Speichereinstellungen für benutzerdefinierten Code)|Datenquelle aus aktuellem Konto wählen|
   |Custom Code Storage Settings Storage Account (Speicherkonto für Speichereinstellungen für benutzerdefinierten Code)|<Ihr Speicherkonto>|
   |Custom Code Storage Settings Container (Container für Speichereinstellungen für benutzerdefinierten Code)|<Ihr Speichercontainer>|

2. Doppelklicken Sie unter **Eingaben** auf **Input.json**. Verwenden Sie die folgenden Einstellungen, und behalten Sie ansonsten die Standardkonfiguration bei:

   |Einstellung|Empfohlener Wert|
   |-------|---------------|
   |`Source`|Blob Storage|
   |Resource|Datenquelle aus aktuellem Konto wählen|
   |Subscription|<Ihr Abonnement>|
   |Speicherkonto|<Ihr Speicherkonto>|
   |Container|<Ihr Speichercontainer>|
   |Ereignisserialisierungsformat|Sonstige (Protobuf, XML, Proprietär...)|
   |Resource|Aus ASA-Projektverweis oder CodeBehind laden|
   |CSharp-Assemblyname|ProtobufDeserializer.dll|
   |Klassenname|MessageBodyProto.MessageBodyDeserializer|
   |Typ der Ereigniskomprimierung|Keine|

3. Fügen Sie der Datei **Script.asaql** die folgende Abfrage hinzu:

   ```sql
   SELECT * FROM Input
   ```

4. Laden Sie die [exemplarische Protobuf-Eingabedatei](https://github.com/Azure/azure-stream-analytics/blob/master/CustomDeserializers/Protobuf/SimulatedTemperatureEvents.protobuf) herunter. Klicken Sie im Ordner **Inputs** mit der rechten Maustaste auf **Input.json**, und wählen Sie **Lokale Eingabe hinzufügen** aus. Doppelklicken Sie anschließend auf **local_Input.json**, und konfigurieren Sie die folgenden Einstellungen:

   |Einstellung|Empfohlener Wert|
   |-------|---------------|
   |Eingabealias|Eingabe|
   |Quellentyp|Datenstrom|
   |Ereignisserialisierungsformat|Sonstige (Protobuf, XML, Proprietär...)|
   |CSharp-Assemblyname|ProtobufDeserializer.dll|
   |Klassenname|MessageBodyProto.MessageBodyDeserializer|
   |Pfad für lokale Eingabedatei|<Dateipfad für die heruntergeladene exemplarische Protobuf-Eingabedatei>|

## <a name="execute-the-stream-analytics-job"></a>Ausführen des Stream Analytics-Auftrags

1. Öffnen Sie **Script.asaql**, und wählen Sie **Lokal ausführen** aus.

2. Sehen Sie sich unter **Ergebnisse der lokalen Ausführung von Stream Analytics** die Ergebnisse an.

Sie haben erfolgreich einen benutzerdefinierten Deserialisierer für Ihren Stream Analytics-Auftrag implementiert. In diesem Tutorial haben Sie den benutzerdefinierten Deserialisierer lokal getestet. Für Ihre tatsächlichen Daten müssen Sie die Ein- und Ausgabe ordnungsgemäß konfigurieren. Anschließend müssen Sie den Auftrag von Visual Studio aus an Azure übermitteln, um ihn in der Cloud unter Verwendung des soeben implementierten benutzerdefinierten Deserialisierers auszuführen.

## <a name="debug-your-deserializer"></a>Debuggen Ihres Deserialisierers

Sie können Ihren benutzerdefinierten .NET-Deserialisierer genau wie .NET-Standardcode lokal debuggen. 

1. Fügen Sie Ihrer Funktion Breakpoints hinzu.

2. Drücken Sie **F5**, um das Debuggen zu starten. Das Programm hält wie erwartet an den von Ihnen festgelegten Breakpoints an.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie die Ressourcengruppe, den Streamingauftrag und alle dazugehörigen Ressourcen, wenn Sie sie nicht mehr benötigen. Durch das Löschen des Auftrags verhindern Sie, dass Kosten für die vom Auftrag verbrauchten Streamingeinheiten anfallen. Wenn Sie den Auftrag in Zukunft verwenden möchten, können Sie ihn beenden und später bei Bedarf neu starten. Wenn Sie diesen Auftrag nicht mehr verwenden möchten, löschen Sie alle Ressourcen, die im Rahmen dieses Tutorials erstellt wurden, mithilfe der folgenden Schritte:

1. Klicken Sie im Azure-Portal im Menü auf der linken Seite auf **Ressourcengruppen** und dann auf den Namen der erstellten Ressource.  

2. Klicken Sie auf der Seite mit Ihrer Ressourcengruppe auf **Löschen**, geben Sie im Textfeld den Namen der zu löschenden Ressource ein, und klicken Sie dann auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie gelernt, wie Sie einen benutzerdefinierten .NET-Deserialisierer für die Protokollpuffer-Eingabeserialisierung implementieren. Weitere Informationen zur Erstellung benutzerdefinierter Deserialisierer finden Sie im folgenden Artikel:

> [!div class="nextstepaction"]
> [Verwenden von .NET-Deserialisierern für Azure Stream Analytics-Aufträge](custom-deserializer-examples.md)

