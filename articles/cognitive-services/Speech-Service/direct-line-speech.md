---
title: Direct Line Speech – Spracherkennungsdienst
titleSuffix: Azure Cognitive Services
description: Ein Überblick über die Features, Funktionen und Einschränkungen für Sprachassistenten, die Direct Line Speech mit dem Speech Software Development Kit (SDK) verwenden.
services: cognitive-services
author: trrwilson
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 11/05/2019
ms.author: travisw
ms.openlocfilehash: 72029d1d56c9d561e629cc750244fd7de8ec113a
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73506565"
---
# <a name="about-direct-line-speech"></a>Informationen zu Direct Line Speech

[Sprachassistenten](voice-assistants.md) hören den Benutzern zu und ergreifen als Reaktion eine Aktion, wobei sie dabei häufig antworten. Sie verwenden die [Spracherkennung](speech-to-text.md), um die Sprache des Benutzers zu erfassen, und ergreifen dann Maßnahmen zum in natürlicher Sprache verstandenen Text. Diese Aktion umfasst häufig die Sprachausgabe des Assistenten, die mit [Sprachsynthese](text-to-speech.md) generiert wurde. Geräte stellen eine Verbindung mit Assistenten über das `DialogServiceConnector`-Objekt des Speech SDKs her.

**Direct Line Speech** ist eine stabile, End-to-End-Lösung zum Erstellen eines flexiblen, erweiterbaren Sprachassistenten, der auf dem Bot Framework und seinem Direct Line Speech-Kanal basiert und für die Spracheingabe- sowie Sprachausgabe-Interaktion mit Bots optimiert ist.

Direct Line Speech bietet ein Höchstmaß an Anpassungsmöglichkeiten und Vielseitigkeit für Sprachassistenten. Es eignet sich hervorragend für interaktive Szenarien, die offen, natürlich oder eine Kombination aus beidem sind und Aufgabenerledigung oder die Verwendung von Befehl-und-Steuerung (Command-and-Control) bieten. Dieser hohe Grad an Flexibilität ist mit einer höheren Komplexität verbunden, und Szenarien, die auf klar definierte Aufgaben unter Verwendung natürlicher Sprache ausgerichtet sind, können [Benutzerdefinierte Befehle (Vorschau)](custom-commands.md) für eine optimierte Benutzererfahrung berücksichtigen.

## <a name="getting-started-with-direct-line-speech"></a>Erste Schritte mit Direct Line Speech

Die ersten Schritte zum Erstellen eines Sprachassistenten mit Direct Line Speech sind das [Abrufen eines Speech-Abonnementschlüssels](get-started.md), das Erstellen eines neuen, diesem Abonnement zugeordneten Bots und das Verbinden des Bot mit dem Direct Line Speech-Kanal.

   ![Konzeptionelle Darstellung des Orchestrierungsdienst-Flows](media/voice-assistants/overview-directlinespeech.png "Flow des Speech-Kanals")

Eine vollständige, schrittweise Anleitung zur Erstellung eines einfachen Sprachassistenten mit Direct Line Speech finden Sie in dem [Tutorial zur Sprachaktivierung Ihres Bot mit dem Speech SDK und dem Direct Line Speech-Kanal](tutorial-voice-enable-your-bot-speech-sdk.md).

Wir bieten außerdem Schnellstarts an, die so konzipiert sind, dass Sie in weniger als 10 Minuten Code ausführen können. In dieser Tabelle werden Schnellstarts für Sprachassistenten nach Sprache aufgelistet.

| Schnellstart | Plattform | API-Referenz |
|------------|----------|---------------|
| C#, UWP | Windows | [Browse](https://aka.ms/csspeech/csharpref) |
| Java | Windows, macOS, Linux | [Browse](https://aka.ms/csspeech/javaref) |
| Java | Android | [Browse](https://aka.ms/csspeech/javaref) |

## <a name="sample-code"></a>Beispielcode

Beispielcode für die Erstellung eines Sprachassistenten ist auf GitHub verfügbar. Diese Beispiele decken die Clientanwendung für die Verbindung mit Ihrem Assistenten in mehreren gängigen Programmiersprachen ab.

* [Beispiele für Sprachassistenten (SDK)](https://aka.ms/csspeech/samples)
* [Tutorial: Ermöglichen der Verwendung von Sprache in Ihrem Assistenten mit dem Speech SDK, C#](tutorial-voice-enable-your-bot-speech-sdk.md)

## <a name="customization"></a>Anpassung

Sprachassistenten, die mithilfe von Azure Speech-Diensten erstellt wurden, können die gesamte Bandbreite der Anpassungsoptionen nutzen, die für die [Spracherkennung](speech-to-text.md), [Sprachsynthese](text-to-speech.md) und [Auswahl benutzerdefinierter Stichwörter](speech-devices-sdk-create-kws.md) verfügbar sind.

> [!NOTE]
> Die Anpassungsoptionen unterscheiden sich je nach Sprache/Gebietsschema. Weitere Informationen erhalten Sie unter [Unterstützte Sprachen](supported-languages.md).

Direct Line Speech und die damit verbundene Funktionalität für Sprachassistenten sind eine ideale Ergänzung zur [Lösung für virtuelle Assistenten](https://docs.microsoft.com/azure/bot-service/bot-builder-enterprise-template-overview) und der Unternehmensvorlage. Direct Line Speech kann zwar mit jedem kompatiblen Bot verwendet werden, diese Ressourcen bieten jedoch eine wiederverwendbare Baseline für qualitativ hochwertige Konversationsumgebungen sowie gemeinsame unterstützende Fähigkeiten und Modelle für einen schnellen Einstieg.

## <a name="reference-docs"></a>Referenz

* [Speech SDK](speech-sdk-reference.md)
* [Azure Bot Service](https://docs.microsoft.com/azure/bot-service/?view=azure-bot-service-4.0)

## <a name="next-steps"></a>Nächste Schritte

* [Beziehen eines kostenlosen Abonnementschlüssels für die Spracherkennungsdienste](get-started.md)
* [Abrufen des Speech SDK](speech-sdk.md)
* [Erstellen und Bereitstellen eines Basisbots](https://docs.microsoft.com/azure/bot-service/bot-builder-tutorial-basic-deploy?view=azure-bot-service-4.0)
* [Abrufen der Lösung für virtuelle Assistenten und der Unternehmensvorlage](https://github.com/Microsoft/AI)
