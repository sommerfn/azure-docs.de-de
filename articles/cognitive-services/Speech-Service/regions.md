---
title: 'Regionen: Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: Referenz zu den Regionen des Speech-Diensts.
services: cognitive-services
author: mahilleb-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/12/2019
ms.author: panosper
ms.custom: seodec18
ms.openlocfilehash: c0414277b4851891911908ba4f42e92abedc86e4
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2019
ms.locfileid: "68553188"
---
# <a name="speech-service-supported-regions"></a>Vom Speech-Dienst unterstützte Regionen

Mithilfe des Speech-Diensts kann Ihre Anwendung Sprache in Text konvertieren, Sprachübersetzungen ausführen und Text in Sprache konvertieren. Der Dienst ist in mehreren Regionen mit eindeutigen Endpunkten für das Speech SDK und REST-APIs verfügbar.

Stellen Sie sicher, dass Sie den Endpunkt verwenden, der mit der Region für Ihr Abonnement übereinstimmt.

## <a name="speech-sdk"></a>Sprach-SDK

Im [Speech SDK](speech-sdk.md) werden Regionen als Zeichenfolge angegeben (z.B. als Parameter für `SpeechConfig.FromSubscription` im Speech SDK für C#).

### <a name="speech-to-text-text-to-speech-and-translation"></a>Spracherkennung, Sprachsynthese und Übersetzung

Das Speech SDK ist in den folgenden Regionen für **Spracherkennung**, **Sprachsynthese** und **Übersetzung** verfügbar:

  Region | Speech SDK-Parameter | Speech-Anpassungsportal
 ------|-------|--------
 USA (Westen) | `westus` | https://westus.cris.ai
 USA, Westen 2 | `westus2` | https://westus2.cris.ai
 East US | `eastus` | https://eastus.cris.ai
 USA (Ost) 2 | `eastus2` | https://eastus2.cris.ai
 USA (Mitte) | `centralus` | https://centralus.cris.ai
 USA Nord Mitte | `northcentralus` | https://northcentralus.cris.ai
 USA Süd Mitte | `southcentralus` | https://southcentralus.cris.ai
 Indien, Mitte | `centralindia` | https://centralindia.cris.ai
 Asien, Osten | `eastasia` | https://eastasia.cris.ai
 Asien, Südosten | `southeastasia` | https://southeastasia.cris.ai
 Japan, Osten | `japaneast` | https://japaneast.cris.ai
 Korea, Mitte | `koreacentral` | https://koreacentral.cris.ai
 Australien (Osten) | `australiaeast` | https://australiaeast.cris.ai
 Kanada, Mitte | `canadacentral` | https://canadacentral.cris.ai
 Nordeuropa | `northeurope` | https://northeurope.cris.ai
 Europa, Westen | `westeurope` | https://westeurope.cris.ai
 UK, Süden | `uksouth` | https://uksouth.cris.ai
 Frankreich, Mitte | `francecentral` | https://francecentral.cris.ai

### <a name="intent-recognition"></a>Absichtserkennung

Verfügbare Regionen für **Absichtserkennung** über das Speech SDK sind die folgenden:

 Globale Region | Region | Speech SDK-Parameter
 ------|-------|--------
 Asien | Asien, Osten | `eastasia`
 Asien | Asien, Südosten | `southeastasia`
 Australien | Australien (Osten) | `australiaeast`
 Europa | Nordeuropa | `northeurope`
 Europa | Europa, Westen | `westeurope`
 Nordamerika | East US | `eastus`
 Nordamerika | USA (Ost) 2 | `eastus2`
 Nordamerika | USA Süd Mitte | `southcentralus`
 Nordamerika | USA, Westen-Mitte | `westcentralus`
 Nordamerika | USA (Westen) | `westus`
 Nordamerika | USA, Westen 2 | `westus2`
 Südamerika | Brasilien Süd | `brazilsouth`

Dies ist eine Teilmenge der Veröffentlichungsregionen, die vom [Language Understanding Intelligent Service (LUIS)](/azure/cognitive-services/luis/luis-reference-regions) unterstützt werden.

### <a name="voice-first-virtual-assistants"></a>Virtuelle Voice-First-Assistenten

Das [Speech SDK](speech-sdk.md) unterstützt Funktionen des **virtuellen Voice-First-Assistenten** in den folgenden Regionen:

Region | Speech SDK-Parameter
-------|---------------------
USA (Westen) | `westus`
USA, Westen 2 | `westus2`
East US | `eastus`
USA (Ost) 2 | `eastus2`
Europa, Westen | `westeurope`
Nordeuropa | `northeurope`
Asien, Südosten | `southeastasia`

## <a name="rest-apis"></a>REST-APIs

Der Speech-Dienst macht auch REST-Endpunkte für Spracherkennungs- und Sprachsyntheseanforderungen verfügbar.

### <a name="speech-to-text"></a>Spracherkennung

Die Referenzdokumentation zur Spracherkennung finden Sie unter [Spracherkennungs-REST-API](rest-speech-to-text.md).

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-speech-to-text.md)]

### <a name="text-to-speech"></a>Text-zu-Sprache

Die Referenzdokumentation zur Sprachsynthese finden Sie unter [Text-to-Speech-REST-API](rest-text-to-speech.md).

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-text-to-speech.md)]