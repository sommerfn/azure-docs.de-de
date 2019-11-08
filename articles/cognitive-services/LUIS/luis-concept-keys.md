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
ms.date: 10/25/2019
ms.author: diberry
ms.openlocfilehash: 973a8dd56437506d907159f212164ff147ba975c
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73487494"
---
# <a name="authoring-and-runtime-keys"></a>Erstellungs- und Laufzeitschlüssel

Language Understanding (LUIS) verfügt über zwei Sätze von Diensten und APIs: 

* Erstellung (bisher als _programmgesteuert_ bezeichnet)
* Vorhersagelaufzeit

Abhängig von dem Service, mit dem Sie arbeiten möchten, und der Art seiner Verwendung, gibt es verschiedene Schlüsseltypen.

## <a name="non-azure-resources-for-luis"></a>Azure-fremde Ressourcen für LUIS

### <a name="starter-key"></a>Startschlüssel

Wenn Sie ganz neu in die Verwendung von LUIS einsteigen, wird ein **Startschlüssel** für Sie erstellt. Diese Ressource bietet:

* kostenlose Anforderungen an den Erstellungsdienst über das LUIS-Portal oder APIs (einschließlich SDKs)
* 1\.000 kostenlose Anforderungen an Vorhersageendpunkte über einen Browser, API oder SDKs

## <a name="azure-resources-for-luis"></a>Azure-Ressourcen für LUIS

<a name="programmatic-key" ></a>
<a name="endpoint-key"></a>
<a name="authoring-key"></a>

LUIS lässt drei Arten von Azure-Ressourcen zu: 
 
|Schlüssel|Zweck|Cognitive Service `kind`|Cognitive Service `type`|
|--|--|--|--|
|[Erstellungsschlüssel](#programmatic-key)|Sie greifen auf die Daten der Anwendung durch Erstellen, Trainieren, Veröffentlichen und Testen zu, und verwalten diese Daten auch so. Erstellen Sie einen LUIS-Erstellungsschlüssel, wenn Sie LUIS-Apps programmgesteuert erstellen möchten.<br><br>Der Zweck des `LUIS.Authoring`-Schlüssels besteht darin, Ihnen Folgendes zu ermöglichen:<br>* programmgesteuerte Verwaltung von Language Understanding-Apps und -Modellen, einschließlich Training und Veröffentlichung<br> * Steuern der Berechtigungen für die Erstellungsressource, indem Sie Personen [die Rolle „Mitwirkender“](#contributions-from-other-authors) zuweisen.|`LUIS.Authoring`|`Cognitive Services`|
|[Vorhersageschlüssel](#prediction-endpoint-runtime-key)| Anforderungen von Abfragen eines Vorhersageendpunkts. Erstellen Sie einen LUIS-Vorhersageschlüssel, bevor Ihre Client-App Vorhersagen über die 1.000 Anforderungen hinaus anfordert, die durch die Startressource zur Verfügung gestellt werden. |`LUIS`|`Cognitive Services`|
|[Cognitive Service-Ressourcenschlüssel für mehrere Dienste](../cognitive-services-apis-create-account-cli.md?tabs=windows#create-a-cognitive-services-resource)|Anforderungen von Abfragevorhersageendpunkten, die mit LUIS und anderen unterstützten Cognitive Services geteilt werden.|`CognitiveServices`|`Cognitive Services`|

Wenn der Vorgang der Ressourcenerstellung abgeschlossen ist, [weisen Sie den Schlüssel](luis-how-to-azure-subscription.md) im LUIS-Portal der App zu.

Es ist wichtig, LUIS-Apps in den [Regionen](luis-reference-regions.md#publishing-regions) zu erstellen, in denen sie veröffentlicht und abgefragt werden sollen.

> [!CAUTION]
> Der Einfachheit halber wird in vielen der Beispiele der [Startschlüssel](#starter-key) verwendet, weil sein [Kontingent](luis-boundaries.md#key-limits) einige kostenlose Vorhersageendpunktaufrufe umfasst.  


### <a name="query-prediction-resources"></a>Ressourcen zur Abfragevorhersage

* Der Laufzeitschlüssel kann für alle LUIS-Apps oder für bestimmte LUIS-Apps verwendet werden. 
* Verwenden Sie die Laufzeitschlüssel nicht für das Erstellen von LUIS-Apps. 

Der LUIS-Laufzeitendpunkt akzeptiert zwei Arten von Abfragen, die beide den Vorhersageendpunkt-Laufzeitschlüssel verwenden, jedoch an verschiedenen Positionen.

Für den Endpunkt, der für den Zugriff zur Laufzeit verwendet wird, wird eine Unterdomäne verwendet, die für die Region Ihrer Ressource eindeutig ist. In der folgenden Tabelle ist diese Region durch `{region}` gekennzeichnet. 

## <a name="assignment-of-the-key"></a>Zuweisen des Schlüssels

Sie können den Laufzeitschlüssel im [LUIS-Portal](https://www.luis.ai) oder über die entsprechenden APIs [zuweisen](luis-how-to-azure-subscription.md). 

## <a name="key-limits"></a>Schlüsselgrenzwerte

Sie können bis zu 10 Erstellungsschlüssel pro Region und Abonnement erstellen. 

Lesen Sie unter [Schlüsselgrenzwerte](luis-boundaries.md#key-limits) und [Azure-Regionen](luis-reference-regions.md) nach. 

Veröffentlichungsregionen unterscheiden sich von Erstellungsregionen. Stellen Sie sicher, dass Sie eine App in der Erstellungsregion erstellen, die der Veröffentlichungsregion entspricht, in der sich Ihre Clientanwendung befinden soll.

## <a name="key-limit-errors"></a>Fehler für Schlüsselgrenzwerte
Wenn Sie Ihr TPS-Kontingent (Transaktionen pro Sekunde) überschreiten, erhalten Sie den HTTP-Fehler 429. Wenn Sie Ihr TPM-Kontingent (Transaktionen pro Monat) überschreiten, erhalten Sie den HTTP-Fehler 403. 

## <a name="contributions-from-other-authors"></a>Beiträge von anderen Autoren

**Für Apps, die zur [Erstellungsressource migriert](luis-migration-authoring.md) wurden**: _Mitwirkende_ werden im Azure-Portal für die Erstellungsressource über die Seite **Zugriffssteuerung (IAM)** verwaltet. Erfahren Sie, [wie Sie einen Benutzer hinzufügen](luis-how-to-collaborate.md), indem Sie die E-Mail-Adresse des Projektmitarbeiters und die Rolle _Mitwirkender_ verwenden. 

**Für Apps, die noch nicht migriert wurden**: Alle _Projektmitarbeiter_ werden im LUIS-Portal über die Seite **Verwalten –> Projektmitarbeiter** App verwaltet.

## <a name="move-transfer-or-change-ownership"></a>Verschieben, Übertragen oder Ändern des Besitzes

Eine App ist durch ihre Azure-Ressourcen definiert, die durch das Abonnement des Besitzers bestimmt sind. 

Sie können Ihre LUIS-App verschieben. Verwenden Sie die folgenden Dokumentationsressourcen im Azure-Portal oder in der Azure-Befehlszeilenschnittstelle:

* [Move app to another LUIS authoring Azure resource](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/apps-move-app-to-another-luis-authoring-azure-resource)
* [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](../../azure-resource-manager/resource-group-move-resources.md)
* [Anleitung zum Verschieben von App Service-Ressourcen](../../azure-resource-manager/move-limitations/app-service-move-limitations.md)

So übertragen Sie den [Besitz](../../billing/billing-subscription-transfer.md) Ihres Abonnements: 

**Für migrierte Benutzer – [Erstellen von ressourcenmigrierten ](luis-migration-authoring.md) Apps**: Als Besitzer der Ressource können Sie ein `contributor` hinzufügen.

**Für Benutzer, die noch nicht migriert wurden**: Exportieren Sie Ihre App als JSON-Datei. Ein anderer LUIS-Benutzer kann die App importieren, wodurch er der App-Besitzer wird. Die neue App hat eine andere App-ID.  

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

<a name="prediction-endpoint-runtime-key"></a>

### <a name="prediction-endpoint-runtime-access"></a>Laufzeitzugriff auf Vorhersageendpunkt

Der Zugriff zum Abfragen des Vorhersageendpunkts wird von einer Einstellung auf der Seite **Anwendungsinformationen** im Abschnitt **Verwalten** gesteuert. 

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

LUIS kennt das Konzept der Übertragung des Ressourcenbesitzes nicht. 

## <a name="securing-the-endpoint"></a>Schützen von Endpunkten 

Sie können steuern, wer Ihren LUIS-Laufzeitschlüssel für den Vorhersageendpunkt sehen kann, indem Sie ihn in einer Server-zu-Server-Umgebung aufrufen. Wenn Sie LUIS über einen Bot verwenden, ist die Verbindung zwischen dem Bot und LUIS bereits sicher. Wenn Sie den LUIS-Endpunkt direkt aufrufen, sollten Sie eine serverseitige API (z.B. als Azure-[Funktion](https://azure.microsoft.com/services/functions/)) mit kontrolliertem Zugang (z.B. [AAD](https://azure.microsoft.com/services/active-directory/)) erstellen. Wenn die serverseitige API aufgerufen und die Authentifizierung und Autorisierung überprüft wurden, übergeben Sie den Aufruf an LUIS. Auch wenn diese Strategie Man-in-the-Middle-Angriffe nicht verhindert, verbirgt sie Ihren Endpunkt vor Ihren Benutzern. Außerdem ermöglicht sie das Nachverfolgen des Zugriffs und das Hinzufügen einer Protokollierung von Endpunktantworten (z.B. [Application Insights](https://azure.microsoft.com/services/application-insights/)).  

## <a name="next-steps"></a>Nächste Schritte

* Grundlegendes zu den Konzepten der [Versionsverwaltung](luis-concept-version.md). 
* Erfahren Sie, [wie Sie Schlüssel erstellen](luis-how-to-azure-subscription.md).
