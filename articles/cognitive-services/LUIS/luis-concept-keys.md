---
title: 'Erstellungs- und Laufzeitschlüssel: LUIS'
titleSuffix: Azure Cognitive Services
description: 'In LUIS werden zwei Schlüssel verwendet: der Erstellungsschlüssel, um Ihr Modell zu erstellen, und der Endpunktschlüssel zum Abfragen des Vorhersageendpunkts mit Benutzeräußerungen.'
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 09/27/2019
ms.author: diberry
ms.openlocfilehash: 70e58077fa40ce685324cd24b447886ec3411034
ms.sourcegitcommit: 6fe40d080bd1561286093b488609590ba355c261
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2019
ms.locfileid: "71703185"
---
# <a name="authoring-and-runtime-keys"></a>Erstellungs- und Laufzeitschlüssel


>[!NOTE]
>[Migrieren](luis-migration-authoring.md) Sie alle Apps, in denen die Azure-Erstellungsressource nicht verwendet wird, bevor Sie die weiteren Schritte ausführen.

In LUIS werden zwei Typen von Azure-Ressourcen verwendet, wobei jeder Typ Schlüssel hat: 
 
* [Erstellung](#programmatic-key), um Absichten und Entitäten zu erstellen, Äußerungen zu bezeichnen sowie zu trainieren und zu veröffentlichen. Wenn Sie Ihre LUIS-App veröffentlichen möchten, benötigen Sie einen [Vorhersageendpunktschlüssel für die Laufzeit](luis-how-to-azure-subscription.md), der der APP zugewiesen ist.
* [Vorhersageendpunktschlüssel für die Laufzeit](#prediction-endpoint-runtime-key). Clientanwendungen, etwa ein Chatbot, benötigen zur Laufzeit Zugriff auf den **Abfragevorhersageendpunkt** über diesen Schlüssel. 

|Schlüssel|Zweck|Cognitive Service `kind`|Cognitive Service `type`|
|--|--|--|--|
|[Erstellungsschlüssel](#programmatic-key)|Erstellung, Training, Veröffentlichung, Testen|`LUIS.Authoring`|`Cognitive Services`|
|[Laufzeitschlüssel für Vorhersageendpunkt](#prediction-endpoint-runtime-key)| Abfragevorhersageendpunkt zur Laufzeit mit einer Benutzeräußerung, um Absichten und Entitäten zu ermitteln|`LUIS`|`Cognitive Services`|

Außerdem stellt LUIS einen [Startschlüssel](luis-how-to-azure-subscription.md#starter-key) mit einem Vorhersageendpunktkontingent von 1000 Transaktionen pro Monat bereit. 

Sie müssen zwar nicht beide Schlüssel gleichzeitig erstellen, es ist aber viel einfacher, wenn Sie dies tun.

Es ist wichtig, LUIS-Apps in den [Regionen](luis-reference-regions.md#publishing-regions) zu erstellen, in denen sie veröffentlicht und abgefragt werden sollen.

<a name="programmatic-key" ></a>

## <a name="authoring-key"></a>Erstellungsschlüssel

Ein Erstellungsschlüssel wird automatisch erstellt, wenn Sie ein LUIS-Konto erstellen. Er ist kostenlos. Wenn Sie mit LUIS beginnen, haben Sie einen Erstellungsschlüssel für sämtliche Ihrer LUIS-Apps für jede [Erstellungsregion](luis-reference-regions.md). Der Zweck des Erstellungsschlüssels besteht darin, Authentifizierung bereitzustellen, damit Sie Ihre LUIS-App verwalten oder Ihre Vorhersageendpunktabfragen testen können. 

Durch Erstellen von Erstellungsschlüsseln im Azure-Portal können Sie die Berechtigungen für die Erstellungsressource steuern, indem Sie Personen [die Rolle „Mitwirkender“](#contributions-from-other-authors) zuweisen. Sie benötigen Berechtigung auf der Azure-Abonnementebene, damit Sie Mitwirkende hinzufügen können. 

Sie finden den Erstellungsschlüssel, indem Sie sich bei [LUIS](luis-reference-regions.md#luis-website) anmelden. Klicken Sie auf der Navigationsleiste rechts oben auf den Kontonamen, um die **Kontoeinstellungen** zu öffnen.

![Erstellungsschlüssel](./media/luis-concept-keys/authoring-key.png)

Wenn Sie **Laufzeitabfragen** ausführen möchten, erstellen Sie die Azure-[LUIS-Ressource](https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/). 

> [!CAUTION]
> Der Einfachheit halber wird in vielen der Beispiele der [Startschlüssel](#starter-prediction-endpoint-runtime-key) verwendet, weil sein [Kontingent](luis-boundaries.md#key-limits) einige kostenlose Vorhersageendpunktaufrufe umfasst.  

<a name="endpoint-key"></a>

## <a name="prediction-endpoint-runtime-key"></a>Laufzeitschlüssel für Vorhersageendpunkt 

Wenn Sie **Laufzeitendpunktabfragen** benötigen, erstellen Sie eine Language Understanding-Ressource (LUIS-Ressource), und weisen Sie diese anschließend der LUIS-App. 

[!INCLUDE [Azure runtime resource creation for Language Understanding and Cognitive Service resources](../../../includes/cognitive-services-luis-azure-resource-instructions.md)]

Nach Abschluss der Erstellung der Ressource müssen Sie der App [den Schlüssel zuweisen](luis-how-to-azure-subscription.md). 

* Der Laufzeitschlüssel (Schlüssel für Abfragevorhersageendpunkt) umfasst ein Kontingent an Endpunkttreffern basierend auf dem Nutzungsplan, den Sie beim Erstellen des Laufzeitschlüssels angegeben haben. Weitere Informationen zu den Preisen finden Sie unter [Cognitive Services-Preise](https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/?v=17.23h).

* Der Laufzeitschlüssel kann für alle LUIS-Apps oder für bestimmte LUIS-Apps verwendet werden. 
* Verwenden Sie die Laufzeitschlüssel nicht für das Erstellen von LUIS-Apps. 

### <a name="starter-prediction-endpoint-runtime-key"></a>Startlaufzeitschlüssel für Vorhersageendpunkt

Der **Startschlüssel** für einen Vorhersageendpunkt ist kostenlos und umfasst 1000 Vorhersageendpunktabfragen. Nachdem diese Abfragen verwendet wurden, sollten Sie eine eigene Vorhersageendpunktressource für Language Understanding (LUIS) erstellen.  

Dies ist eine spezielle Ressource, die für Sie erstellt wurde. Sie wird nicht in der Liste der Azure-Ressourcen angezeigt, da sie als temporärer Anfangsschlüssel gedacht ist. 

<a name="use-endpoint-key-in-query"></a>

### <a name="use-runtime-key-in-query"></a>Verwenden des Laufzeitschlüssels in einer Abfrage
Der LUIS-Laufzeitendpunkt akzeptiert zwei Arten von Abfragen, die beide den Vorhersageendpunkt-Laufzeitschlüssel verwenden, jedoch an verschiedenen Positionen.

Für den Endpunkt, der für den Zugriff zur Laufzeit verwendet wird, wird eine Unterdomäne verwendet, die für die Region Ihrer Ressource eindeutig ist. In der folgenden Tabelle ist diese Region durch `{region}` gekennzeichnet. 


#### <a name="v2-prediction-endpointtabv2"></a>[V2-Vorhersageendpunkt](#tab/V2)

|Verb|Beispiel-URL und Schlüsselposition|
|--|--|
|[GET](https://westus.dev.cognitive.microsoft.com/docs/services/5819c76f40a6350ce09de1ac/operations/5819c77140a63516d81aee78)|`https://{region}.api.cognitive.microsoft.com/luis/v2.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2?runtime-key=your-endpoint-key-here&verbose=true&timezoneOffset=0&q=turn%20on%20the%20lights`|
|[POST](https://westus.dev.cognitive.microsoft.com/docs/services/5819c76f40a6350ce09de1ac/operations/5819c77140a63516d81aee79)| `https://{region}.api.cognitive.microsoft.com/luis/v2.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2`|

#### <a name="v3-prediction-endpointtabv3"></a>[V3-Vorhersageendpunkt](#tab/V3)

|Verb|Beispiel-URL und Schlüsselposition|
|--|--|
|[GET](https://westcentralus.dev.cognitive.microsoft.com/docs/services/luis-endpoint-api-v3-0-preview/operations/5cb0a91e54c9db63d589f433)|`https://{region}.api.cognitive.microsoft.com/luis/v3.0-preview/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2/slots/production/predict?runtime-key=your-endpoint-key-here&query=turn%20on%20the%20lights`|
|[POST](https://westcentralus.dev.cognitive.microsoft.com/docs/services/luis-endpoint-api-v3-0-preview/operations/5cb0a5830f741b27cd03a061)| `https://{region}.api.cognitive.microsoft.com/luis/v3.0-preview/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2/slots/production/predict`| 

Erfahren Sie mehr über den [V3-Vorhersageendpunkt](luis-migration-api-v3.md).

* * * 

**GET**: Ändern Sie den Wert Ihrer Endpunktabfrage für den `runtime-key` vom Erstellungsschlüssel (Startschlüssel) zum neuen Endpunktschlüssel, um das Schlüsselkontingent des LUIS-Endpunkts zu verwenden. Wenn Sie den Schlüssel erstellen und zuweisen, aber nicht den Wert in der Endpunktabfrage für `runtime-key` ändern, nutzen Sie nicht das Kontingent Ihres Endpunktschlüssels.

**POST**: Ändern des Headerwerts für `Ocp-Apim-Subscription-Key`<br>Wenn Sie den Laufzeitschlüssel erstellen und zuweisen, aber nicht den Endpunktabfragewert für `Ocp-Apim-Subscription-Key` ändern, nutzen Sie nicht Ihren Laufzeitschlüssel.

Die App-ID, `df67dcdb-c37d-46af-88e1-8b97951ca1c2`, die in den vorherigen URLs verwendet wurde, ist die öffentliche IoT-App für die [interaktive Demo](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/). 

## <a name="assignment-of-the-runtime-key"></a>Zuweisen des Laufzeitschlüssels

Sie können den Laufzeitschlüssel im [LUIS-Portal](https://www.luis.ai) oder über die entsprechenden APIs [zuweisen](luis-how-to-azure-subscription.md). 

## <a name="key-limits"></a>Schlüsselgrenzwerte

Sie können bis zu 10 Erstellungsschlüssel pro Region und Abonnement erstellen. 

Lesen Sie unter [Schlüsselgrenzwerte](luis-boundaries.md#key-limits) und [Azure-Regionen](luis-reference-regions.md) nach. 

Veröffentlichungsregionen unterscheiden sich von Erstellungsregionen. Stellen Sie sicher, dass Sie eine App in der Erstellungsregion erstellen, die der Veröffentlichungsregion entspricht, in der sich Ihre Clientanwendung befinden soll.

## <a name="key-limit-errors"></a>Fehler für Schlüsselgrenzwerte
Wenn Sie Ihr TPS-Kontingent (Transaktionen pro Sekunde) überschreiten, erhalten Sie den HTTP-Fehler 429. Wenn Sie Ihr TPM-Kontingent (Transaktionen pro Monat) überschreiten, erhalten Sie den HTTP-Fehler 403. 

## <a name="contributions-from-other-authors"></a>Beiträge von anderen Autoren



Die Verwaltung von Beiträgen von Projektmitarbeitern ist abhängig vom aktuellen Status der App.

**Für Apps, die zur [Erstellungsressource migriert](luis-migration-authoring.md) wurden**: _Mitwirkende_ werden im Azure-Portal für die Erstellungsressource über die Seite **Zugriffssteuerung (IAM)** verwaltet. Erfahren Sie, [wie Sie einen Benutzer hinzufügen](luis-how-to-collaborate.md), indem Sie die E-Mail-Adresse des Projektmitarbeiters und die Rolle _Mitwirkender_ verwenden. 

**Für Apps, die noch nicht migriert wurden**: Alle _Projektmitarbeiter_ werden im LUIS-Portal über die Seite **Verwalten –> Projektmitarbeiter** App verwaltet.

### <a name="contributor-roles-vs-entity-roles"></a>Mitwirkender-Rollen im Vergleich zu Entitätsrollen

[Entitätsrollen](luis-concept-roles.md) gelten für das Datenmodell der LUIS-App. Die Projektmitarbeiter/Mitwirkender-Rollen gelten für die Ebenen des Erstellungszugriffs. 

## <a name="moving-or-changing-ownership"></a>Verschieben oder Ändern des Besitzes

Eine App ist durch ihre Azure-Ressourcen definiert, die durch das Abonnement des Besitzers bestimmt sind. 

Sie können Ihre LUIS-App verschieben. Verwenden Sie die folgenden Dokumentationsressourcen im Azure-Portal oder in der Azure-Befehlszeilenschnittstelle:

* [Move app to another LUIS authoring Azure resource](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/apps-move-app-to-another-luis-authoring-azure-resource)
* [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](../../azure-resource-manager/resource-group-move-resources.md)
* [Anleitung zum Verschieben von App Service-Ressourcen](../../azure-resource-manager/move-limitations/app-service-move-limitations.md)
* [Übertragen des Besitzes](../../billing/billing-subscription-transfer.md) Ihres Abonnements 

## <a name="access-for-private-and-public-apps"></a>Zugriff für private und öffentliche Apps

Für eine **private** App ist der Laufzeitzugriff für Besitzer und Mitwirkende möglich. Für eine **öffentliche** App ist der Laufzeitzugriff für jede Person möglich, die über eine eigene Azure [Cognitive Services](../cognitive-services-apis-create-account.md)- oder [LUIS](luis-how-to-azure-subscription.md#create-resources-in-the-azure-portal)-Laufzeitressource und die ID der öffentlichen App verfügt. 

Zurzeit gibt es keinen Katalog mit öffentlichen Apps.

### <a name="authoring-access"></a>Erstellungszugriff
Der Zugriff auf die App aus dem [LUIS](luis-reference-regions.md#luis-website)-Portal oder über die [Erstellungs-APIs](https://go.microsoft.com/fwlink/?linkid=2092087) wird von der Azure-Erstellungsressource gesteuert. 

Der Besitzer und alle Mitwirkenden haben Zugriff für die Erstellung der App. 

|Der Zugriff für die Erstellung umfasst|Notizen|
|--|--|
|Hinzufügen oder Entfernen von Endpunktschlüsseln||
|Exportieren von Versionen||
|Exportieren von Endpunktprotokollen||
|Importieren von Versionen||
|Öffentlichmachen von Apps|Wenn eine App öffentlich ist, kann jede Person mit einem Erstellungs- oder Endpunktschlüssel die App abfragen.|
|Ändern von Modellen|
|Veröffentlichen|
|Überprüfen von Endpunktäußerungen für das [aktive Lernen](luis-how-to-review-endpoint-utterances.md)|
|Trainieren|

### <a name="prediction-endpoint-runtime-access"></a>Laufzeitzugriff auf Vorhersageendpunkt

Der Zugriff zum Abfragen des Vorhersageendpunkts wird von einer Einstellung auf der Seite **Anwendungsinformationen** im Abschnitt **Verwalten** gesteuert. 

![Festlegen der App als öffentlich](./media/luis-concept-security/set-application-as-public.png)

|[Privater Endpunkt](#runtime-security-for-private-apps)|[Öffentlicher Endpunkt](#runtime-security-for-public-apps)|
|:--|:--|
|Für Besitzer und Mitwirkende verfügbar|Für Besitzer, Mitwirkende und alle Personen verfügbar, denen die App-ID bekannt ist|

Sie können steuern, wer Ihren LUIS-Laufzeitschlüssel sieht, indem Sie ihn in einer Server-zu-Server-Umgebung aufrufen. Wenn Sie LUIS über einen Bot verwenden, ist die Verbindung zwischen dem Bot und LUIS bereits sicher. Wenn Sie den LUIS-Endpunkt direkt aufrufen, sollten Sie eine serverseitige API (z.B. als Azure-[Funktion](https://azure.microsoft.com/services/functions/)) mit kontrolliertem Zugang (z.B. [AAD](https://azure.microsoft.com/services/active-directory/)) erstellen. Wenn die serverseitige API aufgerufen und authentifiziert und die Autorisierung überprüft wurde, übergeben Sie den Aufruf an LUIS. Auch wenn diese Strategie Man-in-the-Middle-Angriffe nicht verhindert, verbirgt sie Ihre Schlüssel- und Endpunkt-URL vor Ihren Benutzern. Außerdem ermöglicht sie das Nachverfolgen des Zugriffs und das Hinzufügen einer Protokollierung von Endpunktantworten (z.B. [Application Insights](https://azure.microsoft.com/services/application-insights/)).

#### <a name="runtime-security-for-private-apps"></a>Laufzeitsicherheit für private Apps

Die Laufzeitumgebung einer privaten App ist nur für Folgendes verfügbar:

|Schlüssel und Benutzer|Erklärung|
|--|--|
|Erstellungsschlüssel des Besitzers| Bis zu 1.000 Endpunkttreffer|
|Erstellungsschlüssel für Projektmitarbeiter/Mitwirkende| Bis zu 1.000 Endpunkttreffer|
|Jeder Schlüssel, der LUIS von einem Ersteller oder Projektmitarbeiter/Mitwirkenden zugewiesen wurde|Basierend auf dem Tarif für die Schlüsselverwendung|

#### <a name="runtime-security-for-public-apps"></a>Laufzeitsicherheit für öffentliche Apps

Wenn eine App als öffentlich konfiguriert ist, können _alle_ gültigen LUIS-Erstellungsschlüssel oder LUIS-Endpunktschlüssel Ihre App abfragen, solange der Schlüssel nicht das gesamte Endpunktkontingent verbraucht hat.

Ein Benutzer, der nicht Besitzer oder Mitwirkender ist, hat nur dann Zugriff auf die Laufzeitumgebung einer öffentlichen App, wenn er die App-ID hat. LUIS verfügt nicht über einen öffentlichen _Markt_ oder eine andere Möglichkeit der Suche nach einer öffentlichen App.  

Eine öffentliche App wird in allen Regionen veröffentlicht, so dass ein Benutzer mit einem regionsbasierten LUIS-Ressourcenschlüssel auf die App in jeder Region zugreifen kann, die dem Ressourcenschlüssel zugeordnet ist.

## <a name="transfer-of-ownership"></a>Übertragen des Besitzes

**Für Apps, die zur [Erstellungsressource migriert](luis-migration-authoring.md) wurden**: Als Besitzer der Ressource können Sie ein `contributor` hinzufügen.

**Für Apps, die noch nicht migriert wurden**: Exportieren Sie Ihre App als JSON-Datei. Ein anderer LUIS-Benutzer kann die App importieren, wodurch er der App-Besitzer wird. Die neue App hat eine andere App-ID.  

## <a name="securing-the-endpoint"></a>Schützen von Endpunkten 

Sie können steuern, wer Ihren LUIS-Laufzeitschlüssel für den Vorhersageendpunkt sehen kann, indem Sie ihn in einer Server-zu-Server-Umgebung aufrufen. Wenn Sie LUIS über einen Bot verwenden, ist die Verbindung zwischen dem Bot und LUIS bereits sicher. Wenn Sie den LUIS-Endpunkt direkt aufrufen, sollten Sie eine serverseitige API (z.B. als Azure-[Funktion](https://azure.microsoft.com/services/functions/)) mit kontrolliertem Zugang (z.B. [AAD](https://azure.microsoft.com/services/active-directory/)) erstellen. Wenn die serverseitige API aufgerufen und die Authentifizierung und Autorisierung überprüft wurden, übergeben Sie den Aufruf an LUIS. Auch wenn diese Strategie Man-in-the-Middle-Angriffe nicht verhindert, verbirgt sie Ihren Endpunkt vor Ihren Benutzern. Außerdem ermöglicht sie das Nachverfolgen des Zugriffs und das Hinzufügen einer Protokollierung von Endpunktantworten (z.B. [Application Insights](https://azure.microsoft.com/services/application-insights/)).  

## <a name="next-steps"></a>Nächste Schritte

* Grundlegendes zu den Konzepten der [Versionsverwaltung](luis-concept-version.md). 
* Erfahren Sie, [wie Sie Schlüssel erstellen](luis-how-to-azure-subscription.md).
