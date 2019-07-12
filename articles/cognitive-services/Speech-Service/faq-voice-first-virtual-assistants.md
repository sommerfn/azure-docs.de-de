---
title: Häufig gestellte Fragen zu Direct Line Speech
titleSuffix: Azure Cognitive Services
description: Hier erhalten Sie Antworten auf die häufigsten Fragen zu virtuellen Voice-First-Assistenten, die den Direct Linie Speech-Kanal verwenden.
services: cognitive-services
author: trrwilson
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: travisw
ms.openlocfilehash: 2c669f00ae65667f85976aca218ce51d630159ee
ms.sourcegitcommit: c63e5031aed4992d5adf45639addcef07c166224
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2019
ms.locfileid: "67465487"
---
# <a name="voice-first-virtual-assistants-preview-frequently-asked-questions"></a>Virtuelle Voice-First-Assistenten (Vorschauversion): Häufig gestellte Fragen

Wenn Sie in diesem Dokument keine Antworten auf Ihre Fragen finden, sehen Sie sich die [weiteren Supportoptionen](support.md) an.

## <a name="general"></a>Allgemein

**F: Was ist Direct Line Speech?**

**A:** Der `DialogServiceConnector` im Speech SDK stellt eine bidirektionale, asynchrone Kommunikation für Bots bereit, die mit dem Direct Linie Speech-Kanal in Bot Framework verbunden sind. Dieser Kanal ermöglicht den koordinierten Zugriff auf die Spracherkennung und Sprachsynthese der Azure Speech-Dienste, mit deren Hilfe Bots zu einer kompletten Voice-In/Voice-Out-Konversationserfahrung werden können. Dank der Unterstützung für Aktivierungswörter und die Überprüfung des Aktivierungsworts können Sie mit dieser Lösung hochgradig anpassbare virtuelle Voice-First-Assistenten für Ihre Marke oder Ihr Produkt erstellen.

**F: Wie fange ich an?**

**A:** Wenn Sie einen virtuellen Voice-First-Assistenten erstellen möchten, empfiehlt es sich, zum Einstieg einen [einfachen Bot Framework-Bot zu erstellen](https://docs.microsoft.com/azure/bot-service/bot-builder-tutorial-basic-deploy?view=azure-bot-service-4.0). Verbinden Sie als Nächstes Ihren Bot mit dem [Direct Line Speech-Kanal](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-directlinespeech).

## <a name="debugging"></a>Debuggen

**F: Ich erhalte bei der Verbindungsherstellung einen Fehler vom Typ 401, und nichts funktioniert. Ich weiß, dass mein Speech-Abonnementschlüssel gültig. Was geht da vor?**

**A:** In der Vorschauversion gelten für Direct Line Speech spezifische Einschränkungen dahingehend, welche Abonnements Sie verwenden können. Stellen Sie sicher, dass Sie die Ressource **Speech** (Microsoft.CognitiveServicesSpeechServices, „Speech“) und *nicht* die Ressource **Cognitive Services** (Microsoft.CognitiveServicesAllInOne, „Alle Cognitive Services“) verwenden. Nur [eine Teilmenge der Spracherkennungsdienste-Regionen](regions.md#voice-first-virtual-assistants) wird zurzeit für Direct Line Speech unterstütz.

![korrektes Abonnement für Direct Line Speech](media/voice-first-virtual-assistants/faq-supported-subscription.png "Beispiel eines kompatiblen Speech-Abonnements")

**F: Ich erhalte erkannten Text von Direct Line Speech, es wird aber ein Fehler vom Typ 1011 angezeigt, und mein Bot reagiert nicht. Warum?**

**A:** Dieser Fehler weist auf ein Kommunikationsproblem zwischen dem Bot und Direct Line Speech hin. Stellen Sie sicher, dass Sie eine [Verbindung mit dem Direct Line Speech-Kanal hergestellt](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-directlinespeech) und Ihrem Bot [Unterstützung für das Streaming-Protokoll hinzugefügt](https://aka.ms/botframework/addstreamingprotocolsupport) haben (mit der entsprechenden Websocketunterstützung). Überprüfen Sie dann, ob Ihr Bot auf eingehende Anforderungen vom Kanal reagiert.

**F: Dieser Code funktioniert trotzdem nicht, und/oder ich erhalte bei der Verwendung eines DialogServiceConnector einen anderen Fehler. Wie soll ich vorgehen?**

**A:** Die dateibasierte Protokollierung bietet deutlich mehr Details und kann Supportanfragen beschleunigen. Informationen zum Aktivieren dieser Funktion finden Sie unter [Verwenden der Dateiprotokollierung](how-to-use-logging.md).

## <a name="next-steps"></a>Nächste Schritte

* [Problembehandlung](troubleshooting.md)
* [Versionshinweise](releasenotes.md)
