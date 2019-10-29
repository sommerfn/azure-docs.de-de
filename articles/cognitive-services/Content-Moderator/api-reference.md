---
title: API-Referenz – Content Moderator
titleSuffix: Azure Cognitive Services
description: Erfahren Sie mehr über die verschiedenen Inhaltsmoderations- und die Überprüfungs-APIs für Content Moderator.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: reference
ms.date: 05/29/2019
ms.author: pafarley
ms.openlocfilehash: 7fc46d06b68dca074da060b4866186a6242ffad2
ms.sourcegitcommit: 8074f482fcd1f61442b3b8101f153adb52cf35c9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/22/2019
ms.locfileid: "72757374"
---
# <a name="content-moderator-api-reference"></a>Content Moderator-API-Referenz

Sie können die ersten Schritte mit Azure Content Moderator-APIs folgendermaßen unternehmen:

- [Abonnieren Sie die Content Moderator-API](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesContentModerator) im Azure-Portal.
- Unter [Testen von Content Moderator im Web](quick-start.md) erfahren Sie, wie Sie sich für das [Content Moderator-Prüfungstool](https://contentmoderator.cognitive.microsoft.com/) registrieren.

## <a name="moderation-apis"></a>Moderations-APIs

Sie können die folgenden Content Moderator-APIs zum Einrichten Ihrer Workflows für die Beitragsmoderation verwenden.

| BESCHREIBUNG | Verweis |
| -------------------- |-------------|
| **Bildmoderations-API**<br /><br />Durchsuchen Sie Bilder und erkennen Sie anzügliche sowie nur für Erwachsene geeignete Inhalte mithilfe von Tags, Zuverlässigkeitsbewertungen und anderen extrahierten Informationen. <br /><br />Anhand dieser Informationen können Sie den Inhalt in Ihrem Workflow für die Beitragsmoderation veröffentlichen, ablehnen oder überprüfen. <br /><br />| [Referenz für die Bildmoderations-API](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66c "Referenz für die Bildmoderations-API")   |
| **Textmoderations-API**<br /><br />Durchsuchen Sie Textinhalt. Es werden anstößige Ausdrücke und persönliche Daten zurückgegeben. <br /><br />Anhand dieser Informationen können Sie den Inhalt in Ihrem Workflow für die Beitragsmoderation veröffentlichen, ablehnen oder überprüfen.<br /><br /> | [Referenz für die Textmoderations-API](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66f "Referenz für die Textmoderations-API")   |
| **Videomoderations-API**<br /><br />Durchsuchen Sie Videos und erkennen Sie anzügliche sowie nur für Erwachsene geeignete Inhalte. <br /><br />Anhand dieser Informationen können Sie den Inhalt in Ihrem Workflow für die Beitragsmoderation veröffentlichen, ablehnen oder überprüfen.<br /><br /> | [Übersicht der Videomoderations-API](video-moderation-api.md "Übersicht der Videomoderations-API")   |
| **Listenverwaltungs-API**<br /><br />Erstellen und verwalten Sie benutzerdefinierte Ausschluss- und Aufnahmeliste von Bildern und Text. Nach der Aktivierung überprüfen die Vorgänge **Image - Match** und **Text - Screen**, ob eine Fuzzyübereinstimmung zwischen dem übermittelten Inhalt und Ihren benutzerdefinierten Listen besteht. <br /><br />Aus Effizienzgründen können Sie den auf maschinellen Lernen basierenden Moderationsschritt überspringen.<br /><br /> | [Referenz für die Listenverwaltungs-API](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f675 "Referenz für die Listenverwaltungs-API")   |

## <a name="review-apis"></a>Überprüfen von APIs

Die Überprüfungs-APIs enthalten die folgenden Komponenten:

| BESCHREIBUNG | Verweis |
| -------------------- |-------------|
| **Aufträge**<br /><br /> Starten Sie Moderationsworkflows zum Durchsuchen und Überprüfen von Bild- und Textinhalten. Ein Moderationsauftrag durchsucht Ihre Inhalte mithilfe der Bildmoderations-API und der Textmoderations-API. Moderationsaufträge verwenden definierte und Standardworkflows, um Überprüfungen zu generieren. <br /><br />Nachdem ein menschlicher Moderator die automatisch zugewiesenen Tags und Vorhersagedaten überprüft und eine Entscheidung zur Inhaltsmoderation getroffen hat, sendet die Überprüfungs-API alle Informationen an Ihren API-Endpunkt.<br /><br /> | [Auftragsreferenz](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c5 "Auftragsreferenz")   |
| **Überprüfungen**<br /><br />Verwenden Sie das Prüfungstool, um direkt Bild- oder Textüberprüfungen für menschliche Moderatoren zu erstellen.<br /><br /> Nachdem ein menschlicher Moderator die automatisch zugewiesenen Tags und Vorhersagedaten überprüft und eine Entscheidung zur Inhaltsmoderation getroffen hat, sendet die Überprüfungs-API alle Informationen an Ihren API-Endpunkt.<br /><br /> | [Überprüfungsreferenz](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c4 "Überprüfungsreferenz")   |
| **Workflows**<br /><br />Erstellen, aktualisieren und erhalten Sie Details zu den benutzerdefinierten Workflows, die Ihr Team erstellt. Mithilfe des Prüfungstools können Sie Workflows definieren. <br /> <br />Workflows verwenden normalerweise Content Moderator, können aber auch bestimmte andere APIs verwenden, die als Konnektoren im Prüfungstool verfügbar sind.<br /><br /> | [Workflowreferenz](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b46b3f9b0711b43c4c59 "Workflowreferenz")   |