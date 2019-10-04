---
title: 'LiveEvent: Zustandswerte und Abrechnung in Azure Media Services | Microsoft-Dokumentation'
description: Dieses Thema bietet eine Übersicht über LiveEvent-Zustandswerte und die entsprechende Abrechnung in Azure Media Services.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 01/28/2019
ms.author: juliako
ms.openlocfilehash: 2907b5be7f8d5fda3d510484179e80b065ab64b0
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "67074891"
---
# <a name="live-event-states-and-billing"></a>Zustandswerte von Liveereignissen und Abrechnung

In Azure Media Services fallen für ein Liveereignis Gebühren an, sobald der Zustand in **Wird ausgeführt** geändert wird. Damit für das Liveereignis keine Gebühren mehr anfallen, müssen Sie es beenden.

Wenn **LiveEventEncodingType** für Ihr [Liveereignis](https://docs.microsoft.com/rest/api/media/liveevents) auf „Standard“ oder „Premium1080p“ festgelegt ist, schaltet Media Services jedes Liveereignis, das sich noch im Zustand **Wird ausgeführt** befindet, 12 Stunden nach dem Verlust des Eingangsfeeds automatisch ab. Es werden keine **Liveausgaben** mehr ausgeführt. Allerdings wird Ihnen die Zeit in Rechnung gestellt, in der das Liveereignis den Zustand **Wird ausgeführt** hatte.

## <a name="states"></a>Zustände

Das Liveereignis kann folgende Zustandswerte aufweisen:

|Zustand|BESCHREIBUNG|
|---|---|
|**Beendet**| Dies ist der anfängliche Zustand des Liveereignisses nach der Erstellung (es sei denn, der automatische Start wurde auf „true“ festgelegt). In diesem Status werden keine Gebühren berechnet. In diesem Zustand können die Eigenschaften des Liveereignisses aktualisiert werden, Streaming ist jedoch nicht zulässig.|
|**Wird gestartet**| Das Liveereignis wird gestartet, und Ressourcen werden zugewiesen. In diesem Status werden keine Gebühren berechnet. In diesem Zustand sind weder Updates noch Streaming zulässig. Wenn ein Fehler auftritt, wird das Liveereignis in den Zustand „Stopped“ (Angehalten) zurückgesetzt.|
|**Wird ausgeführt**| Die Liveereignisressourcen wurden zugewiesen, Erfassungs- und Vorschau-URLs wurden generiert, und Livestreams können empfangen werden. Zu diesem Zeitpunkt ist die Abrechnung aktiv. Sie müssen für die Liveereignisressource explizit „Beenden“ auswählen, damit keine Gebühren mehr anfallen.|
|**Wird beendet**| Das Liveereignis wird beendet und die Bereitstellung der Ressourcen aufgehoben. In diesem Übergangszustand erfolgt keine Berechnung. In diesem Zustand sind weder Updates noch Streaming zulässig.|
|**Wird gelöscht**.| Das Liveereignis wird gelöscht. In diesem Übergangszustand erfolgt keine Berechnung. In diesem Zustand sind weder Updates noch Streaming zulässig.|

## <a name="next-steps"></a>Nächste Schritte

- [Übersicht über das Livestreaming](live-streaming-overview.md)
- [Tutorial zu Livestreaming](stream-live-tutorial-with-api.md)
