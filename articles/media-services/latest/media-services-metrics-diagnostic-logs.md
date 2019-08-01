---
title: Überwachen von Azure Media Services-Metriken und -Diagnoseprotokollen über Azure Monitor | Microsoft-Dokumentation
description: Dieser Artikel bietet eine Übersicht darüber, wie Sie Metriken und Diagnoseprotokolle von Azure Media Services über Azure Monitor überwachen können.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2019
ms.author: juliako
ms.openlocfilehash: 6d26cd809d78bf05f66c9fa03be5063ca4d2d5e4
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67805996"
---
# <a name="monitor-media-services-metrics-and-diagnostic-logs"></a>Überwachen von Media Services-Metriken und -Diagnoseprotokollen

[Azure Monitor](../../azure-monitor/overview.md) ermöglicht Ihnen die Überwachung von Metriken und Diagnoseprotokollen, die Ihnen zu verstehen helfen, wie sich Ihre Anwendungen verhalten. Alle von Azure Monitor gesammelten Daten gehören einem von zwei Grundtypen an: Metriken und Protokollen. Sie können Media Services-Diagnoseprotokolle überwachen sowie Warnungen und Benachrichtigungen für die gesammelten Metriken und Protokolle erstellen. Sie können die Metrikdaten mit dem [Metrik-Explorer](../../azure-monitor/platform/metrics-getting-started.md) visualisieren und analysieren. Sie können Protokolle an [Azure Storage](https://azure.microsoft.com/services/storage/) senden, sie an [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) streamen und in [Log Analytics](https://azure.microsoft.com/services/log-analytics/) exportieren oder Dienste von Drittanbietern nutzen.

Eine ausführliche Übersicht finden Sie unter [Azure Monitor-Metrik](../../azure-monitor/platform/data-platform.md) und [Azure Monitor-Diagnoseprotokolle](../../azure-monitor/platform/diagnostic-logs-overview.md).

In diesem Thema werden die unterstützten [Media Services-Metriken](#media-services-metrics) und [Media Services-Diagnoseprotokolle](#media-services-diagnostic-logs) behandelt.

## <a name="media-services-metrics"></a>Media Services-Metriken

Metriken werden in regelmäßigen Abständen gesammelt, unabhängig davon, ob sich der Wert ändert oder nicht. Sie sind für Warnmeldungen gut geeignet, da sie häufig abgetastet werden können und eine Warnung mit relativ einfacher Logik schnell ausgelöst werden kann. Informationen zum Erstellen von Metrikwarnungen finden Sie unter [Erstellen, Anzeigen und Verwalten von Metrikwarnungen mit Azure Monitor](../../azure-monitor/platform/alerts-metric.md).

Media Services unterstützt das Überwachen von Metriken für die folgenden Ressourcen:

* Konto
* Streamingendpunkt
 
### <a name="account"></a>Konto

Sie können die folgenden Kontometriken überwachen. 

|Metrikname|`Display name`|BESCHREIBUNG|
|---|---|---|
|AssetCount|Anzahl der Medienobjekte|Die Medienobjekte in Ihrem Konto.|
|AssetQuota|Kontingent der Medienobjekte|Das Kontingent der Medienobjekte in Ihrem Konto.|
|AssetQuotaUsedPercentage|Prozentsatz des verwendeten Kontingents der Medienobjekte|Der Prozentsatz des bereits verwendeten Kontingents der Medienobjekte.|
|ContentKeyPolicyCount|Anzahl der Richtlinien für Inhaltsschlüssel|Die Richtlinien für Inhaltsschlüssel in Ihrem Konto.|
|ContentKeyPolicyQuota|Kontingent der Richtlinien für Inhaltsschlüssel|Das Kontingent der Richtlinien für Inhaltsschlüssel in Ihrem Konto.|
|ContentKeyPolicyQuotaUsedPercentage|Prozentsatz des Kontingents der Richtlinien für Inhaltsschlüssel|Der Prozentsatz des bereits verwendeten Kontingents der Richtlinien für Inhaltsschlüssel.|
|StreamingPolicyCount|Anzahl der Streamingrichtlinien|Die Streamingrichtlinien in Ihrem Konto.|
|StreamingPolicyQuota|Kontingent der Streamingrichtlinien|Das Kontingent der Streamingrichtlinien in Ihrem Konto.|
|StreamingPolicyQuotaUsedPercentage|Prozentsatz des verwendeten Kontingents der Streamingrichtlinien|Der Prozentsatz des bereits verwendeten Kontingents der Streamingrichtlinien.|
 
Informieren Sie sich außerdem über [Kontingente und Einschränkungen für Konten](limits-quotas-constraints.md).

### <a name="streaming-endpoint"></a>Streamingendpunkt

Die folgenden [Streamingendpunkte](https://docs.microsoft.com/rest/api/media/streamingendpoints) von Media Services-Metriken werden unterstützt:

|Metrikname|`Display name`|BESCHREIBUNG|
|---|---|---|
|Requests|Requests|Gibt die Gesamtzahl der HTTP-Anforderungen an, die vom Streamingendpunkt behandelt werden.|
|Ausgehende Daten|Ausgehende Daten|Die Gesamtzahl der ausgehenden Bytes. Beispiel: Vom Streamingendpunkt gestreamte Bytes.|
|SuccessE2ELatency|End-to-End-Wartezeit bei Erfolg|Die Zeitspanne zwischen dem Empfang der Anforderung durch den Streamingendpunkt und dem Senden des letzten Bytes der Antwort.|

### <a name="why-would-i-want-to-use-metrics"></a>Warum sollte ich Metriken verwenden? 

Hier sind Beispiele dafür, wie Sie durch das Überwachen von Media Services-Metriken Informationen über die Leistung Ihrer Anwendungen erhalten. Media Services-Metriken können beispielsweise für folgende Fragen genutzt werden:

* Wie überwache ich meinen Standardstreamingendpunkt, um zu wissen, wann die Grenzwerte überschritten wurden?
* Wie erkenne ich, ob ich über genügend Premium-Streamingendpunkt-Skalierungseinheiten verfüge? 
* Wie kann ich eine Warnung festlegen, um zu wissen, wann die Streamingendpunkte zentral hochskaliert werden müssen?
* Wie lege ich eine Warnung fest, um zu wissen, wann der für das Konto konfigurierte maximale ausgehende Datenverkehr erreicht wurde?
* Wie kann ich die Aufschlüsselung der fehlgeschlagenen Anforderungen und die Fehlerursachen anzeigen?
* Wie kann ich erkennen, wie viele HLS- oder DASH-Anforderungen aus dem Objekt-Manager abgerufen werden?
* Wie lege ich eine Warnung fest, um zu wissen, wann der Schwellenwert für die Anzahl fehlgeschlagener Anforderungen erreicht wurde? 

### <a name="example"></a>Beispiel

Siehe [Überwachen von Media Services-Metriken](media-services-metrics-howto.md)

## <a name="media-services-diagnostic-logs"></a>Media Services-Diagnoseprotokolle

Diagnoseprotokolle liefern regelmäßig umfassende Daten zum Betrieb einer Azure-Ressource. Weitere Informationen finden Sie unter [Erfassen und Nutzen von Protokolldaten aus Ihren Azure-Ressourcen](../../azure-monitor/platform/diagnostic-logs-overview.md).

Media Services unterstützt die folgenden Diagnoseprotokolle:

* Schlüsselübermittlung

### <a name="key-delivery"></a>Schlüsselübermittlung

|NAME|BESCHREIBUNG|
|---|---|
|Dienstanforderung zur Schlüsselübermittlung|Protokolle, die die wichtigsten Informationen zur Dienstanforderung für die Schlüsselübermittlung anzeigen. Weitere Informationen finden Sie unter [Schemas](media-services-diagnostic-logs-schema.md).|

### <a name="why-would-i-want-to-use-diagnostics-logs"></a>Warum sollte ich Diagnoseprotokolle verwenden? 

Mit Diagnoseprotokollen für die Schlüsselübermittlung können Sie beispielsweise Folgendes untersuchen:

* Anzeigen der Anzahl bereitgestellter Lizenzen nach DRM-Typ
* Anzeigen der Anzahl bereitgestellter Lizenzen nach Richtlinie 
* Anzeigen von Fehlern nach DRM- oder Richtlinientyp
* Anzeigen der Anzahl nicht autorisierter Lizenzanforderungen von Clients

### <a name="example"></a>Beispiel

Siehe [Überwachen von Media Services-Diagnoseprotokollen](media-services-diagnostic-logs-howto.md)

## <a name="next-steps"></a>Nächste Schritte 

* [Erfassen und Nutzen von Protokolldaten aus Ihren Azure-Ressourcen](../../azure-monitor/platform/diagnostic-logs-overview.md)
* [Erstellen, Anzeigen und Verwalten von Metrikwarnungen mit Azure Monitor](../../azure-monitor/platform/alerts-metric.md)
* [Überwachen von Media Services-Metriken](media-services-metrics-howto.md)
* [Überwachen von Media Services-Diagnoseprotokollen](media-services-diagnostic-logs-howto.md)
