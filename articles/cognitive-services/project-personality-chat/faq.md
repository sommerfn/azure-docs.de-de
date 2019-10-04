---
title: Häufig gestellte Fragen (FAQ) – Individualisierter Chat
titlesuffix: Azure Cognitive Services
description: Häufig gestellte Fragen zu Personality Chat
services: cognitive-services
author: noellelacharite
manager: nitinme
ms.service: cognitive-services
ms.subservice: personality-chat
ms.topic: conceptual
ms.date: 05/07/2018
ms.author: nitinme
comment: As a bot developer, I want my bot to be able to handle small talk in a consistent tone so that my bot appears more complete and conversational.
ROBOTS: NOINDEX
ms.openlocfilehash: d5f144fa0bf5325bfe2922fdc00c6689d894ff7e
ms.sourcegitcommit: 267a9f62af9795698e1958a038feb7ff79e77909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70258572"
---
# <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

## <a name="why-doesnt-this-answer-every-question-i-ask-it-like-other-chat-bots"></a>Warum wird nicht wie bei anderen Chatbots jede Frage beantwortet, die ich stelle?

Das Projekt „Personality Chat“ erweitert einen Bot um allgemeinen Smalltalk, der Persönlichkeit simuliert und eine vollständigere Benutzererfahrung ermöglicht. Der Bot wurde nicht für lange Gespräche über Themen entworfen, die nicht für seine primäre Funktion relevant sind. Er könnte auf alle Gespräche antworten, ist jedoch in der Betaversion auf allgemeinen Smalltalk beschränkt.

## <a name="how-can-i-customize-the-personality-to-suit-my-brand"></a>Wie kann ich die Persönlichkeit an meine Marke anpassen?

Wählen Sie die passendste Persona aus den verfügbaren Standardpersonas aus. Heute können Sie in der Redaktionsbibliothek die Antworten an Ihre Marke anpassen. In Zukunft können Sie eine Stichprobenmenge der Antworten der ausgewählten Persönlichkeit hochladen und die passendste Version der Persona-ID suchen. Es gibt auch Methoden, um das Modell zu trainieren und anzupassen.

## <a name="is-this-service-powering-existing-intelligent-agents-such-aszo"></a>Unterstützt dieser Dienst vorhandene intelligente Agents wie Zo?

Die Dienste, die Zo, Cortana und das Projekt „Personality Chat“ unterstützen, verwenden ähnliche Techniken, es gibt jedoch Unterschiede. Es wurden Lernalgorithmen verwendet, die auf den Erfahrungen mit Zo und Cortana basieren.

## <a name="can-this-service-lead-to-bad-customer-experiences"></a>Kann dieser Dienst die Kundenerfahrung verschlechtern?

Für eine bessere Benutzerfreundlichkeit kann Personality Chat Antworten generieren, die über die Antworten im Redaktionsdataset hinausgehen. Außerdem wird versucht, alle Benutzereingaben zu interpretieren. Es ist also möglich, dass eine Antwort nicht zum Kontext zu passen scheint. Mehrere Steuerelement wurden verwendet, um unpassende Antworten zu vermeiden. Diese basieren auf den Erfahrungen mit intelligenten Agents wie Zo. Standardmäßig ist das Projekt „Personality Chat“ darauf festgelegt, nur auf erkannte Absichten des Benutzers zu antworten. Sie sollten testen, ob das Projekt „Personality Chat“ für Ihre Situation geeignet ist. Wir freuen uns auf Ihr Feedback, wenn Sie feststellen, dass Verbesserungen vorgenommen werden können. Wenn Sie diesen Dienst in Zukunft mit Ihren Kunden verwenden, wird empfohlen, eine anonymisierte Protokollierung zu verwenden, damit Probleme mit Live-Benutzerinteraktionen erkannt werden können.
