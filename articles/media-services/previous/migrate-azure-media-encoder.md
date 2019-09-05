---
title: Migrieren von Azure Media Encoder zu Media Encoder Standard | Microsoft-Dokumentation
description: In diesem Thema wird erläutert, wie Sie von Azure Media Encoder zum Media Encoder Standard-Medienprozessor migrieren.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/21/2019
ms.author: juliako
ms.openlocfilehash: 645d40e51b69272f1883f5ad1fb73c425f7b4b8f
ms.sourcegitcommit: 3f78a6ffee0b83788d554959db7efc5d00130376
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/26/2019
ms.locfileid: "70019318"
---
# <a name="migrate-from-azure-media-encoder-to-media-encoder-standard"></a>Migrieren von Azure Media Encoder zu Media Encoder Standard

In diesem Artikel werden die Schritte für die Migration vom älteren AME-Medienprozessor (Azure Media Encoder), der am 30. November 2019 eingestellt wird, zum Media Encoder Standard-Medienprozessor erläutert.  

Beim Codieren von Dateien mit AME haben Kunden in der Regel eine benannte Voreinstellungszeichenfolge wie `H264 Adaptive Bitrate MP4 Set 1080p` verwendet. Für die Migration muss Ihr Code aktualisiert werden, um den **Media Encoder Standard**-Medienprozessor anstelle von AME und eine der entsprechenden [Systemvoreinstellungen](media-services-mes-presets-overview.md) wie `H264 Multiple Bitrate 1080p` zu verwenden. 

## <a name="migrating-to-media-encoder-standard"></a>Migrieren zu Media Encoder Standard

Hier folgt ein typisches C#-Codebeispiel, das den Legacy-Medienprozessor verwendet. 

```csharp
// Declare a new job. 
IJob job = _context.Jobs.Create("AME Job"); 
// Get a media processor reference, and pass to it the name of the  
// processor to use for the specific task. 
IMediaProcessor processor = GetLatestMediaProcessorByName("Azure Media Encoder"); 

// Create a task with the encoding details, using a string preset. 
// In this case " H264 Adaptive Bitrate MP4 Set 1080p" preset is used. 
ITask task = job.Tasks.AddNew("My encoding task", 
    processor, 
    " H264 Adaptive Bitrate MP4 Set 1080p", 
    TaskOptions.None); 
```

Hier ist die aktualisierte Version, die Media Encoder Standard verwendet.

```csharp
// Declare a new job. 
IJob job = _context.Jobs.Create("Media Encoder Standard Job"); 
// Get a media processor reference, and pass to it the name of the  
// processor to use for the specific task. 
IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard"); 

// Create a task with the encoding details, using a string preset. 
// In this case " H264 Multiple Bitrate 1080p" preset is used. 
ITask task = job.Tasks.AddNew("My encoding task", 
    processor, 
    " H264 Multiple Bitrate 1080p", 
    TaskOptions.None); 
```

### <a name="advanced-scenarios"></a>Erweiterte Szenarien 

Wenn Sie eine eigene Codierungsvoreinstellung für AME mit dessen Schema erstellt haben, gibt es ein [äquivalentes Schema für Media Encoder Standard](media-services-mes-schema.md). Wenn Sie Fragen zur Zuordnung der älteren Einstellungen zum neuen Encoder haben, können Sie uns über mailto:amshelp@microsoft.com erreichen.  
## <a name="known-differences"></a>Bekannte Unterschiede 

Media Encoder Standard ist robuster, zuverlässiger, hat eine höhere Leistung und liefert eine bessere Qualität als der herkömmliche AME-Encoder. Außerdem haben Sie folgende Möglichkeiten: 

* Media Encoder Standard erzeugt Ausgabedateien mit einer anderen Namenskonvention als AME.
* Media Encoder Standard erzeugt Artefakte wie Dateien, die die [Eingabedateimetadaten](media-services-input-metadata-schema.md) und die [Metadaten der Ausgabedatei(en)](media-services-output-metadata-schema.md) enthalten.

## <a name="next-steps"></a>Nächste Schritte

* [Legacy-Komponenten](legacy-components.md)
* [Seite mit der Preisübersicht](https://azure.microsoft.com/pricing/details/media-services/#encoding)
