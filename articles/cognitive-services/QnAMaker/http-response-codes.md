---
title: API-HTTP-Antwortcodes – QnA Maker
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, welche HTTP-Antwortcodes über die QnA Maker-APIs zurückgegeben werden. Dies ist beim Beheben von Fehlern hilfreich.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 02/20/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: 0416b282a869536b0cea591ba39d7a3804697bac
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2019
ms.locfileid: "67446522"
---
# <a name="qna-maker-api-http-response-codes"></a>QnA Maker-API – HTTP-Antwortcodes
Die APIs für die [Verwaltung](https://go.microsoft.com/fwlink/?linkid=2092179) und Vorhersage geben HTTP-Antwortcodes zurück. Antwortnachrichten enthalten spezifische Informationen zu einer Anforderung, aber es wird ein allgemeiner HTTP-Antwortstatuscode verwendet. 

### <a name="management"></a>Verwaltung

|Code|Erklärung|
|:--|--|
|2xx|Erfolgreich|
|400|Die Parameter der Anforderung sind fehlerhaft, da die erforderlichen Parameter nicht vorhanden, falsch formatiert oder zu groß sind|
|400|Der Text der Anforderung ist fehlerhaft, da der JSON-Code nicht vorhanden, falsch formatiert oder zu groß ist|
|401|Ungültiger Schlüssel|
|403|Verboten – Sie verfügen nicht über die richtigen Berechtigungen|
|404|KB nicht vorhanden|
|410|Diese API ist veraltet und nicht mehr verfügbar.|
