---
title: Livestreaming mit Media Services v3
titleSuffix: Azure Media Services
description: Informationen zum Livestreaming mit Azure Media Services v3
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 06/13/2019
ms.author: juliako
ms.openlocfilehash: 47d526ea410bc449c91ae4fb10913850c447f1b3
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73582646"
---
# <a name="tutorial-stream-live-with-media-services"></a>Tutorial: Livestreaming mit Media Services

> [!NOTE]
> Auch wenn in diesem Tutorial die [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.models.liveevent?view=azure-dotnet)-Beispiele verwendet werden, sind die allgemeinen Schritte für die [REST-API](https://docs.microsoft.com/rest/api/media/liveevents), die [CLI](https://docs.microsoft.com/cli/azure/ams/live-event?view=azure-cli-latest) oder für andere unterstützte [SDKs](media-services-apis-overview.md#sdks) identisch.

In Azure Media Services sind [Liveereignisse](https://docs.microsoft.com/rest/api/media/liveevents) für die Verarbeitung von Livestreaminginhalten zuständig. Ein Liveereignis stellt einen Eingabeendpunkt (Erfassungs-URL) bereit, den Sie dann für einen Liveencoder bereitstellen. Das Liveereignis empfängt Live-Eingabestreams aus dem Liveencoder und stellt diese zum Streamen durch einen oder mehrere [Streamingendpunkte](https://docs.microsoft.com/rest/api/media/streamingendpoints) zur Verfügung. Zudem stellen Liveereignisse einen Vorschauendpunkt (Vorschau-URL) bereit, mit dem Sie eine Vorschau des Streams anzeigen und überprüfen können, bevor Sie ihn weiter verarbeiten und übermitteln. In diesem Tutorial erfahren Sie, wie Sie unter Verwendung von .NET Core ein Liveereignis vom Typ **Pass-Through** erstellen.

Das Tutorial veranschaulicht folgende Vorgehensweisen:

> [!div class="checklist"]
> * Herunterladen der in diesem Thema beschriebenen Beispiel-App
> * Untersuchen des Codes für das Livestreaming
> * Wiedergabe des Ereignisses mit [Azure Media Player](https://amp.azure.net/libs/amp/latest/docs/index.html) unter https://ampdemo.azureedge.net
> * Bereinigen der Ressourcen

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

- Installieren Sie Visual Studio Code oder Visual Studio.
- [Erstellen Sie ein Media Services-Konto.](create-account-cli-how-to.md)<br/>Merken Sie sich die Werte, die Sie für die Namen von Ressourcengruppe und Media Services-Konto verwendet haben.
- Führen Sie die Schritte unter [Zugreifen auf die Azure Media Services-API mit der Azure CLI](access-api-cli-how-to.md) aus, und speichern Sie die Anmeldeinformationen. Sie benötigen sie für den Zugriff auf die API.
- Eine Kamera oder ein Gerät (beispielsweise ein Laptop) zum Übertragen eines Ereignisses.
- Ein lokaler Liveencoder, der Signale von der Kamera in Datenströme konvertiert, die an den Media Services-Livestreamingdienst gesendet werden. Der Datenstrom muss das Format **RTMP** oder **Smooth Streaming** haben.

> [!TIP]
> Lesen Sie [Live streaming with Azure Media Services v3](live-streaming-overview.md) (Livestreaming mit Azure Media Services v3), bevor Sie mit diesem Tutorial fortfahren. 

## <a name="download-and-configure-the-sample"></a>Herunterladen und Konfigurieren des Beispiels

Klonen Sie ein GitHub-Repository auf Ihren Computer, das das .NET-Streamingbeispiel enthält, indem Sie den folgenden Befehl verwenden:  

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials.git
 ```

Das Livestreamingbeispiel befindet sich im Ordner [Live](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/tree/master/NETCore/Live/MediaV3LiveApp).

Öffnen Sie in Ihrem heruntergeladenen Projekt die Datei [appsettings.json](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/blob/master/NETCore/Live/MediaV3LiveApp/appsettings.json). Ersetzen Sie die Werte durch die Anmeldeinformationen, die Sie per [API-Zugriff](access-api-cli-how-to.md) abgerufen haben.

> [!IMPORTANT]
> In diesem Beispiel wird für jede Ressourcen ein eindeutiges Suffix verwendet. Wenn Sie das Debuggen abbrechen oder die App beenden, ohne das Beispiel vollständig zu durchlaufen, werden in Ihrem Konto mehrere Liveereignisse generiert. <br/>Die aktiven Liveereignisse müssen unbedingt beendet werden. Andernfalls **fallen für die Ereignisse Kosten an**!

## <a name="examine-the-code-that-performs-live-streaming"></a>Untersuchen des Codes für Livestreaming

In diesem Abschnitt werden die Funktionen untersucht, die in der Datei [Program.cs](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/blob/master/NETCore/Live/MediaV3LiveApp/Program.cs) des Projekts *MediaV3LiveApp* definiert werden.

Das Beispiel erstellt für jede Ressource ein eindeutiges Suffix, damit keine Namenskonflikte auftreten, wenn Sie das Beispiel ohne Bereinigung der Ressourcen mehrmals ausführen.

> [!IMPORTANT]
> In diesem Beispiel wird für jede Ressourcen ein eindeutiges Suffix verwendet. Wenn Sie das Debuggen abbrechen oder die App beenden, ohne das Beispiel vollständig zu durchlaufen, werden in Ihrem Konto mehrere Liveereignisse generiert. <br/>
> Die aktiven Liveereignisse müssen unbedingt beendet werden. Andernfalls **fallen für die Ereignisse Kosten an**!

### <a name="start-using-media-services-apis-with-net-sdk"></a>Starten der Verwendung von Media Services-APIs mit dem .NET SDK

Um mit der Verwendung von Media Services-APIs in .NET zu beginnen, müssen Sie ein **AzureMediaServicesClient**-Objekt erstellen. Zum Erstellen des Objekts müssen Sie Anmeldeinformationen bereitstellen, die für den Client zum Herstellen einer Verbindung mit Azure mithilfe von Azure AD erforderlich sind. In dem Code, den Sie zu Beginn des Artikels geklont haben, erstellt die Funktion **GetCredentialsAsync** das ServiceClientCredentials-Objekt basierend auf den in der lokalen Konfigurationsdatei angegebenen Anmeldeinformationen. 

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CreateMediaServicesClient)]

### <a name="create-a-live-event"></a>Erstellen eines Liveereignisses

In diesem Abschnitt erfahren Sie, wie Sie ein Liveereignis vom Typ **Pass-Through** erstellen. („LiveEventEncodingType“ ist in diesem Fall auf „None“ festgelegt.) Weitere Informationen zu den verfügbaren Typen von Liveereignissen finden Sie unter [Liveereignisse und Liveausgaben](live-events-outputs-concept.md#live-event-types). 
 
Beim Erstellen des Liveereignisses können Sie folgende Punkte angeben:

* Den Media Services-Standort.
* Streamingprotokoll für das Liveereignis (momentan unterstützte Protokolle: RTMP und Smooth Streaming).<br/>Die Protokolloption kann nicht geändert werden, während das Liveereignis oder die zugehörigen Liveausgaben aktiv sind. Sollten Sie verschiedene Protokolle benötigen, erstellen Sie für jedes Streamingprotokoll ein separates Liveereignis.  
* IP-Einschränkungen für Erfassung und Vorschau. Sie können die IP-Adressen definieren, die ein Video für dieses Liveereignis erfassen dürfen. Zulässige IP-Adressen können als einzelne IP-Adresse (Beispiel: 10.0.0.1), als IP-Adressbereiche mit einer IP-Adresse und einer CIDR-Subnetzmaske (Beispiel: 10.0.0.1/22) oder als IP-Adressbereiche mit einer IP-Adresse und einer Subnetzmaske in Punkt-Dezimalschreibweise (Beispiel: 10.0.0.1(255.255.252.0)) angegeben werden.<br/>Wenn keine IP-Adressen angegeben sind und es keine Regeldefinition gibt, sind keine IP-Adressen zulässig. Um alle IP-Adressen zuzulassen, erstellen Sie eine Regel und legen 0.0.0.0/0 fest.<br/>Die IP-Adressen müssen in einem der folgenden Formate vorliegen: IPv4-Adresse mit vier Ziffern oder CIDR-Adressbereich
* Bei der Ereigniserstellung können Sie angeben, dass das Ereignis automatisch gestartet werden soll. <br/>Wenn für den automatischen Start „true“ festgelegt ist, wird das Liveereignis nach der Erstellung gestartet. Dies bedeutet, dass die Abrechnung beginnt, sobald das Liveereignis startet. Sie müssen für die Liveereignisressource explizit „Beenden“ auswählen, damit keine Gebühren mehr anfallen. Weitere Informationen finden Sie im Abschnitt [LiveEvent-Zustandswerte und Abrechnung](live-event-states-billing.md).
* Damit eine Erfassungs-URL vorhersagbar ist, legen Sie den Vanitymodus fest. Ausführliche Informationen finden Sie unter [Erfassungs-URLs für Liveereignisse](live-events-outputs-concept.md#live-event-ingest-urls).

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CreateLiveEvent)]

### <a name="get-ingest-urls"></a>Abrufen von Erfassungs-URLs

Sobald das Liveereignis erstellt wurde, können Sie Erfassungs-URLs abrufen, die Sie dem Liveencoder bereitstellen. Diese URLs werden vom Encoder zur Eingabe eines Livedatenstroms verwendet.

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#GetIngestURL)]

### <a name="get-the-preview-url"></a>Abrufen der Vorschau-URL

Verwenden Sie den Vorschauendpunkt (previewEndpoint), um eine Vorschau anzuzeigen und sich zu vergewissern, dass die Eingabe des Encoders auch tatsächlich empfangen wird.

> [!IMPORTANT]
> Vergewissern Sie sich, dass das Video an die Vorschau-URL übertragen wird, bevor Sie fortfahren.

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#GetPreviewURLs)]

### <a name="create-and-manage-live-events-and-live-outputs"></a>Erstellen und Verwalten von Liveereignissen und Liveausgaben

Wenn der Stream an das Liveereignis übertragen wird, können Sie das Streamingereignis starten, indem Sie ein Medienobjekt, eine Liveausgabe und einen Streaminglocator erstellen. Dadurch wird der Datenstrom archiviert und über den Streamingendpunkt für die Zuschauer verfügbar gemacht.

#### <a name="create-an-asset"></a>Erstellen eines Medienobjekts

Erstellen Sie ein Medienobjekt, das von der Liveausgabe verwendet werden kann.

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CreateAsset)]

#### <a name="create-a-live-output"></a>Erstellen einer Liveausgabe

Liveausgaben werden bei der Erstellung gestartet und beim Löschen beendet. Wenn Sie die Liveausgabe löschen, werden das zugrunde liegende Medienobjekt und dessen Inhalt nicht gelöscht.

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CreateLiveOutput)]

#### <a name="create-a-streaming-locator"></a>Erstellen eines Streaminglocators

> [!NOTE]
> Beim Erstellen Ihres Media Services-Kontos wird dem Konto ein **Standard**-Streamingendpunkt im Zustand **Beendet** hinzugefügt. Um mit dem Streamen Ihrer Inhalte zu beginnen und die [dynamische Paketerstellung](dynamic-packaging-overview.md) und dynamische Verschlüsselung zu nutzen, muss der Streamingendpunkt, von dem Sie Inhalte streamen möchten, den Zustand **Wird ausgeführt** aufweisen.

Wenn Sie das Liveausgabe-Medienobjekt mit einem Streaminglocator veröffentlicht haben, ist das Liveereignis (bis zur DVR-Fensterlänge) weiterhin bis zu Ablauf oder Löschung des Streaminglocators sichtbar, je nachdem, was zuerst eintritt.

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CreateStreamingLocator)]

```csharp

// Get the url to stream the output
ListPathsResponse paths = await client.StreamingLocators.ListPathsAsync(resourceGroupName, accountName, locatorName);

foreach (StreamingPath path in paths.StreamingPaths)
{
    UriBuilder uriBuilder = new UriBuilder();
    uriBuilder.Scheme = "https";
    uriBuilder.Host = streamingEndpoint.HostName;

    uriBuilder.Path = path.Paths[0];
    // Get the URL from the uriBuilder: uriBuilder.ToString()
}
```

### <a name="cleaning-up-resources-in-your-media-services-account"></a>Bereinigen der Ressourcen in Ihrem Media Services-Konto

Wenn die Streamingereignisse abgeschlossen sind und Sie die zuvor bereitgestellten Ressourcen bereinigen möchten, führen Sie die folgenden Schritte aus:

* Beenden Sie die Datenstromeingabe vom Encoder.
* Beenden Sie das Liveereignis. Nach Beendigung des Liveereignisses fallen keine weiteren Kosten an. Wenn Sie den Kanal erneut starten, weist er die gleiche Erfassungs-URL auf, damit Sie den Encoder nicht erneut konfigurieren müssen.
* Sie können Ihren Streamingendpunkt beenden, sofern Sie das Archiv Ihres Liveereignisses nicht als bedarfsgesteuerten Datenstrom bereitstellen möchten. Wenn sich das Liveereignis im Zustand „Beendet“ befindet, fallen keine weiteren Kosten an.

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CleanupLiveEventAndOutput)]

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CleanupLocatorAssetAndStreamingEndpoint)]

## <a name="watch-the-event"></a>Ansehen des Ereignisses

Kopieren Sie zur Wiedergabe des Ereignisses die Streaming-URL, die Sie beim Ausführen des Codes in „Erstellen eines Streaminglocators“ erhalten haben. Sie können einen Medienplayer Ihrer Wahl verwenden. Um Ihren Stream zu testen, können Sie den [Azure Media Player](https://amp.azure.net/libs/amp/latest/docs/index.html) unter https://ampdemo.azureedge.net verwenden.

Das Liveereignis konvertiert Ereignisse automatisch in On-Demand-Inhalt, wenn es beendet wird. Auch nach dem Beenden und Löschen des Ereignisses können die Benutzer archivierte Inhalte als Video auf Abruf streamen, solange Sie das Medienobjekt nicht löschen. Ein Medienobjekt kann nicht gelöscht werden, wenn es von einem Ereignis verwendet wird. Zuerst muss das betreffende Ereignis gelöscht werden.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie keine Ressourcen in Ihrer Ressourcengruppe mehr benötigen, einschließlich der Media Services und Speicherkonten, die Sie für dieses Tutorial erstellt haben, löschen Sie die zuvor erstellte Ressourcengruppe.

Führen Sie den folgenden CLI-Befehl aus:

```azurecli-interactive
az group delete --name amsResourceGroup
```

> [!IMPORTANT]
> Wenn Sie das Liveereignis nicht beenden, fallen weiter Kosten dafür an. Falls das Projekt/Programm abstürzt oder aus einem anderen Grund geschlossen wird, bleibt das Liveereignis unter Umständen aktiv und verursacht weitere Kosten.

## <a name="ask-questions-give-feedback-get-updates"></a>Fragen stellen, Feedback geben, Updates abrufen

Im Artikel [Azure Media Services-Community](media-services-community.md) finden Sie verschiedene Möglichkeiten, Fragen zu stellen, Feedback zu geben und Updates zu Media Services zu bekommen.

## <a name="next-steps"></a>Nächste Schritte

[Streamen von Dateien](stream-files-tutorial-with-api.md)
 
