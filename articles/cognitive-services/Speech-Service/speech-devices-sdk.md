---
title: Informationen zum Speech Devices SDK – Spracherkennungsdienst
titleSuffix: Azure Cognitive Services
description: Erste Schritte mit dem Speech-Geräte-SDK Der Speech-Dienst kann mit verschiedensten Geräten und Audioquellen verwendet werden. Jetzt können Sie einen Schritt weiter gehen und Ihre Sprachanwendungen mit angepasster Hardware und Software nutzen. Beim Speech-Geräte-SDK handelt es sich um eine vorab optimierte Bibliothek, die mit spezifischen mikrofonfähigen Development Kits kombiniert wurde.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: 243072477c7d249d7066a7a448061c51a0bd2f34
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73468718"
---
# <a name="about-the-speech-devices-sdk"></a>Informationen zum Speech-Geräte-SDK

[Speech Services](overview.md) kann mit verschiedensten Geräten und Audioquellen verwendet werden. Jetzt können Sie einen Schritt weiter gehen und Ihre Sprachanwendungen mit angepasster Hardware und Software nutzen. Beim Speech-Geräte-SDK handelt es sich um eine vorab optimierte Bibliothek, die mit spezifischen mikrofonfähigen Development Kits kombiniert wurde.

Anwendungsmöglichkeiten für das Speech-Geräte-SDK:

* Testen Sie neue Sprachszenarien schnell.
* Integrieren Sie die cloudbasierten Speech Services einfacher in Ihr Gerät.
* Sorgen Sie für eine herausragende Benutzerfreundlichkeit für Ihre Kunden.

Das Speech-Geräte-SDK nutzt das [Speech-SDK](speech-sdk.md). Es verwendet das Speech-SDK zum Senden von Audioinhalten, die mit unserem Audioverarbeitungsalgorithmus bearbeitet wurden, vom Mikrofonarray des Geräts an die [Speech Services](overview.md). Es verwendet Mehrkanal-Audio für eine präzisere Fernfeld-[Spracherkennung](speech-to-text.md) mit Geräuschunterdrückung, Echounterdrückung, Beamforming und Hallunterdrückung.

Mit dem Speech Devices SDK können Sie auch Ambient Devices mit einem [benutzerdefinierten Schlüsselwort](speech-devices-sdk-create-kws.md) erstellen, sodass der Auslöser zum Initiieren einer Benutzerinteraktion in Bezug auf Ihre Marke einmalig ist.

Mit dem Speech Devices SDK lassen sich verschiedenste stimmaktivierte Szenarien entwickeln, z. B. [Sprachassistenten](https://aka.ms/bots/speech/va), Drive-through-Bestellsysteme, [Unterhaltungstranskription](conversation-transcription-service.md) und intelligente Lautsprecher. Es ist möglich, Benutzern mit Textausgaben oder mit Sprachausgaben in einer Standard- oder [benutzerdefinierten Stimme](how-to-customize-voice-font.md) zu antworten, Suchergebnisse bereitzustellen, [Übersetzungen](speech-translation.md) in andere Sprachen durchzuführen und vieles mehr. Wir freuen uns darauf, zu sehen, was Sie erstellen!

## <a name="get-the-speech-devices-sdk"></a>Abrufen des SDK für sprachaktivierte Geräte

### <a name="android"></a>Android

Für Android-Geräte laden Sie die neueste Version des [Speech-Geräte-SDK für Android](https://aka.ms/sdsdk-download-android) herunter.

### <a name="windows"></a>Windows

Für Windows wird die Beispielanwendung als plattformübergreifende Java-Anwendung bereitgestellt. Laden Sie die neueste Version des [Speech-Geräte-SDK für die JRE](https://aka.ms/sdsdk-download-JRE) herunter.
Die Anwendung wird mit dem Speech SDK-Paket und der Eclipse Java-IDE (v4) unter 64-Bit-Windows erstellt. Sie wird in einer 64-Bit Java 8-JRE (Runtime Environment) ausgeführt.

### <a name="linux"></a>Linux

Für Linux wird die Beispielanwendung als plattformübergreifende Java-Anwendung bereitgestellt. Laden Sie die neueste Version des [Speech-Geräte-SDK für die JRE](https://aka.ms/sdsdk-download-JRE) herunter.
Die Anwendung wird mit dem Speech SDK-Paket und der Eclipse Java-IDE (v4) unter 64-Bit-Linux (Ubuntu 16.04, Ubuntu 18.04, Debian 9) erstellt. Sie wird in einer 64-Bit Java 8-JRE (Runtime Environment) ausgeführt.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Auswählen Ihres Speech-Geräts](get-speech-devices-sdk.md)
>
> [!div class="nextstepaction"]
> [Beziehen eines kostenlosen Abonnementschlüssels für die Spracherkennungsdienste](get-started.md)
