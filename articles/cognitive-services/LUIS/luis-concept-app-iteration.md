---
title: Iteratives App-Design – LUIS
titleSuffix: Azure Cognitive Services
description: LUIS lernt am besten in einem iterativen Zyklus aus Modelländerungen, Äußerungsbeispielen, Veröffentlichungen und dem Sammeln von Daten aus Endpunktabfragen.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 10/25/2019
ms.author: diberry
ms.openlocfilehash: 12a1f2304e4255eb9abd04ab2e2d0726066dd1e6
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73487777"
---
# <a name="authoring-cycles-and-versions"></a>Erstellungszyklen und Versionen

Ihre LUIS-App lernt am besten in einem iterativen Zyklus aus:

* Erstellen einer neuen Version
* Bearbeiten des App-Schemas
    * Absichten mit Beispieläußerungen
    * entities
    * Features
* Training
* test
* Veröffentlichen
    * Testen am Vorhersageendpunkt für aktives Lernen
* Sammeln von Daten aus Endpunktabfragen

![Erstellungszyklus](./media/luis-concept-app-iteration/iteration.png)

## <a name="building-a-luis-schema"></a>Erstellen eines LUIS-Schemas

Mithilfe des App-Schemas soll definiert werden, was der Benutzer fragt (Absicht) und welche Teile der Frage Details (Entitäten) bereitstellen, mit deren Hilfe die Antwort bestimmt werden kann. 

Das App-Schema muss spezifisch für die App-Domäne sein, um Wörter und Ausdrücke zu ermitteln, die relevant sind und in einer typischen Reihenfolge vorliegen. 

Beispieläußerungen stellen Benutzereingaben dar, die von der App zur Laufzeit erwartet werden. 

Das Schema erfordert Absichten und _sollte über Entitäten verfügen_. 

### <a name="example-schema-of-intents"></a>Beispielschema von Absichten

Das gängigste Schema ist ein Absichtsschema, das nach Absichten strukturiert ist. Diese Art Schema ist darauf angewiesen, dass LUIS die Absicht eines Benutzers bestimmt. 

Dieser Schematyp darf über Entitäten verfügen, wenn dies LUIS beim Bestimmen der Absicht nützt. Beispielsweise hilft eine Versandentität (als Deskriptor für eine Absicht) LUIS, eine Versandabsicht zu erkennen. 

### <a name="example-schema-of-entities"></a>Beispielschema von Entitäten

Ein Entitätsschema konzentriert sich auf die Entitäten, die ihrerseits die Daten darstellen, die aus den Äußerungen extrahiert werden sollen. 

Die Absicht der Äußerung ist für die Clientanwendung weniger oder gar nicht wichtig. 

Eine gängige Methode zum Organisieren eines Entitätsschemas besteht darin, alle Beispieläußerungen der Absicht „None“ hinzuzufügen. 

### <a name="example-of-a-mixed-schema"></a>Beispiel für ein gemischtes Schema

Das leistungsfähigste und ausgereifteste Schema ist ein Absichtsschema mit einer umfassenden Palette an Entitäten und Features. Dieses Schema kann entweder als Absichts- oder als Entitätsschema beginnen und dann im weiteren Wachstum Konzepte von beiden einschließen, in dem Maß, wie die Clientanwendung diese Informationseinheiten benötigt. 

## <a name="add-example-utterances-to-intents"></a>Hinzufügen von Beispieläußerungen zu Absichten

LUIS benötigt einige Beispieläußerungen in jeder **Absicht**. Für die Beispieläußerungen sind ausreichend viele Variationen bei der Wortauswahl und Wortreihenfolge erforderlich, um die Absicht der Äußerung zu bestimmen. 

> [!CAUTION]
> Fügen Sie nicht massenhaft Beispieläußerungen hinzu. Beginnen Sie mit 15 bis 30 spezifischen und unterschiedlichen Beispielen. 

Für jede Beispieläußerung müssen alle **erforderlichen zu extrahierenden Daten** mit **Entitäten** entworfen und bezeichnet sein. 

|Schlüsselelement|Zweck|
|--|--|
|Absicht|**Klassifizieren** von Benutzeräußerungen in einer einzelnen Absicht oder Aktion. Beispiele sind `BookFlight` und `GetWeather`.|
|Entität|**Extrahieren** von Daten aus Äußerungen, was für die Vervollständigung der Absicht erforderlich ist. Beispiele hierfür sind Datum und Uhrzeit der Reise und der Standort.|

Sie entwerfen Ihre LUIS-App so, dass sie Äußerungen ignoriert, die für Ihre App-Domäne nicht relevant sind, indem Sie die Äußerung der Absicht **None** zuweisen. 

## <a name="test-and-train-your-app"></a>Testen und Trainieren Ihrer App

Wenn Sie zu jeder Absicht 15 bis 30 verschiedene Beispieläußerungen haben und die erforderlichen Entitäten mit Bezeichnungen versehen sind, müssen Sie das Testen und anschließend das [Training](luis-how-to-train.md) durchführen. 

## <a name="publish-to-a-prediction-endpoint"></a>Veröffentlichung an einem Vorhersageendpunkt

Achten Sie darauf, Ihre App zu veröffentlichen, sodass sie in den benötigten [Vorhersageendpunktregionen](luis-reference-regions.md) verfügbar ist. 

## <a name="test-your-published-app"></a>Testen der veröffentlichten App

Sie können Ihre veröffentlichte LUIS-App vom HTTPS-Vorhersageendpunkt aus testen. Durch Tests vom Vorhersageendpunkt kann LUIS alle Äußerungen mit niedrigen Zuverlässigkeitswerten zur [Überprüfung](luis-how-to-review-endpoint-utterances.md) auswählen.  

## <a name="create-a-new-version-for-each-cycle"></a>Erstellen einer neuen Version für jeden Zyklus

Versionen in LUIS sind mit den aus der herkömmlichen Programmierung bekannten Versionen vergleichbar. Jede Version ist eine Momentaufnahme der App. Bevor Sie Änderungen an der App vornehmen, erstellen Sie eine neue Version. Es ist einfacher, zu einer älteren Version zurückzukehren, als Absichten und Äußerungen zu entfernen, um einen früheren Zustand zu erhalten.

Die Versions-ID besteht aus Zeichen, Ziffern oder „.“ und darf nicht länger als 10 Zeichen sein.

Die ursprüngliche Version (0.1) ist standardmäßig die aktive Version. 

### <a name="begin-by-cloning-an-existing-version"></a>Klonen Sie zunächst eine vorhandene Version.

Klonen Sie eine vorhandene Version, um sie als Ausgangspunkt für die neue Version zu verwenden. Nachdem Sie eine Version geklont haben, wird die neue Version die **aktive** Version. 

### <a name="publishing-slots"></a>Veröffentlichungsslots
Sie veröffentlichen entweder im Staging- oder im Produktionsslot. Jeder Slot kann eine andere Version oder die gleiche Version aufweisen. Das ist nützlich, um Änderungen vor der Veröffentlichung in der Produktion zu überprüfen, die für Bots oder andere Anwendungen verfügbar ist, die LUIS aufrufen. 

Trainierte Versionen stehen nicht automatisch am [Endpunkt](luis-glossary.md#endpoint) Ihrer App zur Verfügung. Sie müssen eine Version [veröffentlichen](luis-how-to-publish-app.md) oder erneut veröffentlichen, damit sie am App-Endpunkt verfügbar ist. Sie können für das **Staging** und die **Produktion** veröffentlichen. Damit stehen Ihnen am Endpunkt zwei Versionen zur Verfügung. Wenn Sie weitere Versionen der App an einem Endpunkt benötigen, sollten Sie die Version exportieren und in eine neue App importieren. Die neue App hat eine andere App-ID.

### <a name="import-and-export-a-version"></a>Importieren und Exportieren von Versionen
Sie können eine Version auf App-Ebene importieren. Diese Version wird die aktive Version und verwendet die Versions-ID in der `versionId`-Eigenschaft der App-Datei. Sie können auch auf Versionsebene in eine vorhandene App importieren. Die neue Version wird die aktive Version. 

Sie können eine Version auf App- oder auf Versionsebene exportieren. Der einzige Unterschied besteht darin, dass die auf App-Ebene exportierte Version die derzeit aktive Version ist, während Sie beim Exportieren auf Versionsebene auf der Seite **[Einstellungen](luis-how-to-manage-versions.md)** die zu exportierende Version auswählen können. 

Die exportierte Datei enthält nicht:

* maschinell erlernte Informationen, da die App nach dem Importieren neu trainiert wird
* Informationen zu Mitwirkenden

Um Ihr LUIS-App-Schema zu sichern, exportieren Sie eine Version aus dem LUIS-Portal.

## <a name="manage-contributor-changes-with-versions-and-apps"></a>Verwalten von Änderungen durch Mitwirkende mit Versionen und Apps

LUIS bietet das Konzept der Mitwirkenden an einer App und stellt dazu Berechtigungen auf Azure-Ressourcenebene zur Verfügung. Kombinieren Sie dieses Konzept mit Versionsverwaltung, um gezielte Zusammenarbeit zu ermöglichen. 

Verwenden Sie die folgenden Techniken zur Verwaltung von Änderungen durch Mitwirkende an ihrer App.

### <a name="manage-multiple-versions-inside-the-same-app"></a>Verwalten mehrerer Versionen in derselben App
Zunächst [klonen](luis-how-to-manage-versions.md#clone-a-version) Sie für jeden Ersteller eine Version aus einer Basisversion. 

Jeder Ersteller ändert seine eigene Version der App. Sobald jeder Ersteller mit seinem Modell zufrieden ist, werden die neuen Versionen in JSON-Dateien exportiert.  

Exportierte Apps, JSON- oder LU-Dateien können auf hin Änderungen verglichen werden. Kombinieren Sie die Dateien, um eine einzelne Datei der neuen Version zu erstellen. Ändern Sie die **versionId**-Eigenschaft, um die neue zusammengeführte Version zu kennzeichnen. Importieren Sie diese Version in die ursprüngliche App. 

Diese Methode erlaubt eine aktive Version, eine Stagingversion und eine veröffentlichte Version. Sie können die Ergebnisse der aktiven Version mit einer veröffentlichten Version (Staging oder Produktion) im [Bereich für interaktives Testen](luis-interactive-test.md) vergleichen.

### <a name="manage-multiple-versions-as-apps"></a>Verwalten mehrerer Versionen als Apps
[Exportieren](luis-how-to-manage-versions.md#export-version) Sie die Basisversion. Jeder Ersteller importiert die Version. Die Person, die die App importiert, ist auch der Besitzer dieser Version. Wenn sie das Bearbeiten der App abgeschlossen hat, exportiert sie ihre Version. 

Die exportierten Apps sind JSON-formatierte Dateien, die mit dem Basisexport auf Änderungen verglichen werden können. Kombinieren Sie die Dateien, um eine einzelne JSON-Datei der neuen Version zu erstellen. Ändern Sie die **versionId**-Eigenschaft in der JSON-Datei, um die neue zusammengeführte Version zu kennzeichnen. Importieren Sie diese Version in die ursprüngliche App.

Erfahren Sie mehr über das Erstellen von Beiträgen durch [Projektmitarbeiter](luis-how-to-collaborate.md).

## <a name="review-endpoint-utterances-to-begin-the-new-authoring-cycle"></a>Überprüfen von Endpunktäußerungen zum Einleiten des neuen Erstellungszyklus

Wenn Sie einen Erstellungszyklus abgeschlossen haben, können Sie erneut beginnen. Beginnen Sie mit dem [Überprüfen der Vorhersageendpunktäußerungen](luis-how-to-review-endpoint-utterances.md), die LUIS mit niedrigen Zuverlässigkeitswerten gekennzeichnet hat. Überprüfen Sie diese Äußerungen sowohl auf richtig vorhergesagte Absicht als auch auf richtig und vollständig extrahierte Entitäten. Nach dem Überprüfen und Akzeptieren der Änderungen sollte die Prüfliste leer sein.  

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich über die Konzepte der [Zusammenarbeit](luis-concept-keys.md).
