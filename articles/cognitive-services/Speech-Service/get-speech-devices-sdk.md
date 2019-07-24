---
title: Abrufen des SDK für sprachaktivierte Geräte
titleSuffix: Azure Cognitive Services
description: Speech Services kann mit verschiedensten Geräten und Audioquellen verwendet werden. Jetzt können Sie einen Schritt weiter gehen und Ihre Sprachanwendungen mit angepasster Hardware und Software nutzen. In diesem Artikel erfahren Sie, wie Sie auf das Speech Devices SDK zugreifen und mit der Entwicklung beginnen.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: cff35398509c021a3d2ccf708e2705ab7e259d34
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2019
ms.locfileid: "67719798"
---
# <a name="get-the-cognitive-services-speech-devices-sdk"></a>Abrufen des Cognitive Services Speech-Geräte-SDK

Beim Speech Devices SDK handelt es sich um eine vorab optimierte Bibliothek für die Verwendung mit speziell entwickelten Development Kits und verschiedenen Mikrofonarraykonfigurationen.

## <a name="choose-a-development-kit"></a>Auswählen eines Development Kits

|Geräte|Spezifikation|BESCHREIBUNG|Szenarien|
|--|--|--|--|
|[Roobo Smart Audio Dev Kit](https://ddk.roobo.com)</br>[Setup](speech-devices-sdk-roobo-v1.md) / [Schnellstart](speech-devices-sdk-android-quickstart.md)![Roobo Smart Audio Dev Kit](media/speech-devices-sdk/device-roobo-v1.jpg)|Array mit 7 Mikrofonen, ARM SoC, WLAN, Audioausgang, EA </br>[Android](speech-devices-sdk-android-quickstart.md)|Das erste Speech Devices SDK zur Anpassung des Microsoft-Mikrofonarrays und Front Processing SDK für die Entwicklung hochwertiger Transkriptions- und Sprachszenarien|Unterhaltungstranskription, Smart Speaker, Sprach-Agent, Wearable|
|[Azure Kinect DK](https://azure.microsoft.com/services/kinect-dk/)</br>[Setup](https://docs.microsoft.com/azure/Kinect-dk/set-up-azure-kinect-dk) / [Schnellstart](speech-devices-sdk-windows-quickstart.md)![Azure Kinect DK](media/speech-devices-sdk/device-azure-kinect-dk.jpg)|Array mit 7 Mikrofonen, RGB und Tiefenkameras </br>[Windows](speech-devices-sdk-windows-quickstart.md)/[Linux](speech-devices-sdk-linux-quickstart.md)|Ein Entwicklerkit mit erweiterten KI-Sensoren (künstliche Intelligenz) für ausgereifte Modelle zum maschinellen Sehen und Sprachmodelle. Es kombiniert ein erstklassiges räumliches Mikrofonarray und eine Tiefenkamera mit einer Videokamera und einem Lagesensor – alles in einem kleinen Gerät mit mehreren Modi, Optionen und SDKs für verschiedene Computetypen.|Unterhaltungstranskription, Robotik, Smart Building|
|Roobo Smart Audio Dev Kit 2![Roobo Smart Audio Dev Kit 2](media/speech-devices-sdk/device-roobo-v2.jpg)|Array mit 7 Mikrofonen, ARM SoC, WLAN, Bluetooth, EA </br>Linux|Das Speech Devices SDK der 2. Generation, das ein alternatives Betriebssystem und mehr Features in einem kostengünstigen Referenzentwurf umfasst|Unterhaltungstranskription, Smart Speaker, Sprach-Agent, Wearable|
|URbetter T11 Development Board![URbetter DDK](media/speech-devices-sdk/device-urbetter.jpg)|Array mit 7 Mikrofonen, ARM SoC, WLAN, Ethernet, HDMI, USB-Kamera </br>Linux|Ein branchenübliches Speech Devices SDK zur Anpassung des Microsoft-Mikrofonarrays und zur Unterstützung erweiterter E/A wie HDMI/Ethernet und weiterer USB-Peripheriegeräte|Unterhaltungstranskription, Bildung, Kliniken, Roboter, OTT-Box, Sprach-Agent, Drive-through|

## <a name="download-the-speech-devices-sdk"></a>Herunterladen des Speech-Geräte-SDK

Laden Sie das [Speech Devices SDK](https://aka.ms/sdsdk-download) herunter.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erste Schritte mit dem Speech-Geräte-SDK](https://aka.ms/sdsdk-quickstart)
