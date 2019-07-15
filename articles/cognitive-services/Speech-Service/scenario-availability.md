---
title: 'Szenarioverfügbarkeit: Speech-Dienste'
titlesuffix: Azure Cognitive Services
description: Referenz zu den Regionen des Speech-Diensts.
services: cognitive-services
author: chrisbasoglu
manager: xdh
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/07/2019
ms.author: cbasoglu
ms.openlocfilehash: 7aa2c72a01f1887ea9680f8d5706b825a49039a1
ms.sourcegitcommit: d3b1f89edceb9bff1870f562bc2c2fd52636fc21
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/04/2019
ms.locfileid: "67561284"
---
# <a name="scenario-availability"></a>Szenarioverfügbarkeit

Das Speech-Dienst-SDK bietet zahlreiche Szenarien für eine Vielzahl von Programmiersprachen und Umgebungen.  Nicht alle Szenarien sind derzeit in allen Programmiersprachen oder in allen Umgebungen verfügbar.  Die Verfügbarkeit der einzelnen Szenarien ist nachfolgend aufgeführt.

- **Spracherkennung (SR), Begriffsliste, Absicht, Übersetzung und lokale Container**
  - Alle Programmiersprachen/Umgebungen mit einem Pfeillink <img src="media/index/link.jpg" height="15" width="15"></img> in der Schnellstarttabelle [hier](https://aka.ms/csspeech).
- **Sprachsynthese (TTS)**
  - C++/Windows und Linux
  - C#/Windows & UWP & Unity
  - Die TTS-REST-API kann in allen anderen Situationen verwendet werden.
- **Aktivierungswort (Keyword Spotter/KWS)**
  - C++/Windows und Linux
  - C#/Windows und Linux
  - Python/Windows und Linux
  - Java/Windows und Linux und Android (Speech Devices SDK)
  - Die Funktionalität für das Aktivierungswort (Keyword Spotter/KWS) kann u. U. mit jedem Mikrofontyp verwendet werden, offiziell wird KWS derzeit jedoch nur für die Mikrofonarrays in der Azure Kinect DK-Hardware oder im Speech Devices SDK unterstützt.
- **Virtueller Voice-First-Assistent**
  - C++/Windows und Linux und macOS
  - C#/Windows
  - Java/Windows und Linux und macOS und Android (Speech Devices SDK)
- **Unterhaltungstranskription**
  - C++/Windows und Linux
  - C# (Framework und .NET Core)/Windows und UWP und Linux
  - Java/Windows und Linux und Android (Speech Devices SDK)
- **Callcentertranskription**
  - REST-API und kann in jeder Situation verwendet werden
- **Per Codec komprimierte Audioeingabe**
  - C++/Linux
  - C#/Linux
  - Java/Linux und Android
