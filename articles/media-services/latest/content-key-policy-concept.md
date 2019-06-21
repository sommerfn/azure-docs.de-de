---
title: 'Richtlinien für Inhaltsschlüssel in Media Services: Azure | Microsoft-Dokumentation'
description: In diesem Artikel wird erläutert, was Richtlinien für Inhaltsschlüssel sind und wie sie in Azure Media Services verwendet werden.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 05/28/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: a597ab3519f4ba1696e111622541bcab89488558
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "66425435"
---
# <a name="content-key-policies"></a>Richtlinien für Inhaltsschlüssel

Mit Media Services können Sie Ihre zu übermittelnden Live- und On-Demand-Inhalte dynamisch mit Advanced Encryption Standard (AES-128) oder einem der drei wichtigsten DRM-Systeme verschlüsseln: Microsoft PlayReady, Google Widevine und Apple FairPlay. Media Services bietet auch einen Dienst für die Übermittlung von AES-Schlüsseln und DRM-Lizenzen (PlayReady, Widevine und FairPlay) an autorisierte Clients. 

Um Verschlüsselungsoptionen für Ihren Datenstrom festzulegen, müssen Sie die [Streamingrichtlinie](streaming-policy-concept.md) erstellen und sie mit Ihrem [Streaminglocator](streaming-locators-concept.md) verknüpfen. Sie erstellen die [Richtlinie für den Inhaltsschlüssel](https://docs.microsoft.com/rest/api/media/contentkeypolicies), um zu konfigurieren, wie der Inhaltsschlüssel (der den sicheren Zugriff auf Ihre [Medienobjekte](assets-concept.md) ermöglicht) an Endclients übermittelt wird. Sie müssen die Anforderungen (Einschränkungen) für die Richtlinie für den Inhaltsschlüssel festlegen, die erfüllt sein müssen, damit Schlüssel mit der angegebenen Konfiguration an Clients übermittelt werden. Diese Richtlinie für den Inhaltsschlüssel ist zum Streamen oder Herunterladen als Klartext nicht erforderlich. 

Normalerweise ordnen Sie Ihre **Richtlinie für Inhaltsschlüssel** Ihrem [Streaminglocator](streaming-locators-concept.md) zu. Alternativ hierzu können Sie die Richtlinie für Inhaltsschlüssel auch in einer [Streamingrichtlinie](streaming-policy-concept.md) angeben (beim Erstellen einer benutzerdefinierten Streamingrichtlinie für erweiterte Szenarien). 

Es wird empfohlen, Media Services Inhaltsschlüssel automatisch generieren zu lassen. Normalerweise würde ein langlebiger Schlüssel verwendet und das Vorhandensein der Richtlinie mithilfe von **Get** überprüft. Um den Schlüssel abzurufen, müssen Sie eine separate Aktionsmethode aufrufen, um geheime Schlüssel oder Anmeldeinformationen abzurufen, wie im folgenden Beispiel gezeigt.

**Richtlinien für Inhaltsschlüssel** sind aktualisierbar. Es dauert bis zu 15 Minuten, die Schlüsselbereitstellungscaches zu aktualisieren und die aktualisierte Richtlinie zu übernehmen. 

> [!IMPORTANT]
> * Eigenschaften von **Inhaltssschlüsselrichtlinien** vom Datetime-Typ liegen immer im UTC-Format vor.
> * Sie sollten eine begrenzte Sammlung von Richtlinien für Ihr Media Services-Konto erstellen und diese für Ihre Streaminglocators wiederverwenden, wenn dieselben Optionen benötigt werden. Weitere Informationen finden Sie unter [Kontingente und Einschränkungen](limits-quotas-constraints.md).

### <a name="example"></a>Beispiel

Verwenden Sie **GetPolicyPropertiesWithSecretsAsync**, um den Schlüssel abzurufen. Dies wird im Beispiel unter [Abrufen eines Signaturschlüssels aus der vorhandenen Richtlinie](get-content-key-policy-dotnet-howto.md#get-contentkeypolicy-with-secrets) veranschaulicht.

## <a name="filtering-ordering-paging"></a>Filterung, Sortierung, Paging

Informationen finden Sie unter [Filterung, Sortierung, Paginierung von Media Services-Entitäten](entities-overview.md).

## <a name="next-steps"></a>Nächste Schritte

* [Verwenden der dynamischen AES-128-Verschlüsselung und des Schlüsselübermittlungsdiensts](protect-with-aes128.md)
* [Verwenden der dynamischen DRM-Verschlüsselung und des Lizenzbereitstellungsdiensts](protect-with-drm.md)
* [EncodeHTTPAndPublishAESEncrypted](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/tree/master/NETCore/EncodeHTTPAndPublishAESEncrypted)
