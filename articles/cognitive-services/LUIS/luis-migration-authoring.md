---
title: Migrieren zu Azure-Ressource für Erstellung
titleSuffix: Azure Cognitive Services
description: Informationen zum Migrieren zu einem Schlüssel einer Azure-Erstellungsressource
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 10/04/2019
ms.author: diberry
ms.openlocfilehash: 19852fe3a6925ada3dea141a1472683ee264f6d5
ms.sourcegitcommit: c2e7595a2966e84dc10afb9a22b74400c4b500ed
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2019
ms.locfileid: "71973291"
---
# <a name="migrate-to-an-azure-resource-authoring-key"></a>Migrieren zu einem Schlüssel einer Azure-Erstellungsressource

Die Erstellungsauthentifizierung von Language Understanding (LUIS) wurde von einem E-Mail-Konto in eine Azure-Ressource geändert. 

## <a name="why-migrate"></a>Vorteile der Migration

Wenn Sie für die Erstellung eine Azure-Ressource verwenden, können Sie als Besitzer der Ressource den Zugriff auf die Erstellung steuern. Sie können Erstellungsressourcen erstellen und benennen, um verschiedene Autorengruppen zu verwalten. 

Wenn Sie beispielsweise zwei Arten von LUIS-Apps haben, die Sie mit unterschiedlichen Mitgliedern erstellen, können Sie zwei verschiedene Erstellungsressourcen erstellen und Mitwirkende zuweisen. Die Autorisierung wird von der Azure-Erstellungsressource gesteuert. 

> [!Note]
> Vor der Migration werden Mitautoren als _Projektmitarbeiter_ bezeichnet. Nach der Migration wird die Azure-Rolle _Mitwirkender_ für dieselbe Funktionalität verwendet.

## <a name="what-is-migrating"></a>Was ist hier mit Migration gemeint?

Migration umfasst:

* Alle Benutzer von LUIS, Besitzer und Mitwirkende.
* **Alle** Apps.
* Eine **unidirektionale** Migration

Der Besitzer kann keine Teilmenge zu migrierender Apps auswählen, und der Prozess kann nicht rückgängig gemacht werden. 

Folgendes ist die Migration nicht: 

* Ein Prozess, in dem Projektmitarbeiter gesammelt und automatisch in die Azure-Erstellungsressource verschoben oder in dieser hinzugefügt werden. Diesen Schritt müssen Sie als App-Besitzer ausführen. Dieser Schritt erfordert Berechtigungen für die entsprechende Ressource.
* Ein Prozess zum Erstellen und Zuweisen einer Vorhersagelaufzeitressource. Wenn Sie eine Vorhersagelaufzeitressource benötigen, müssen Sie den [gesonderten Prozess](luis-how-to-azure-subscription.md#create-resources-in-the-azure-portal) ausführen, der unverändert ist. 

## <a name="how-are-the-apps-migrating"></a>Wie werden die Apps migriert?

Der Migrationsprozess wird im [LUIS-Portal](https://www.luis.ai) bereitgestellt. 

Sie werden zu einer Migration aufgefordert, wenn Folgendes zutrifft:

* Sie haben Apps im E-Mail-Authentifizierungssystem für Erstellung.
* Und Sie sind der App-Besitzer. 

Durch Abbrechen des Fensters können Sie den Migrationsprozess hinauszögern. Sie werden regelmäßig zu einer Migration aufgefordert, bis Sie die Migration ausgeführt haben oder bis der Stichtag für die Migration überschritten ist. Sie können den Migrationsprozess über das Schlosssymbol auf der oberen Navigationsleiste starten.

## <a name="migration-for-the-app-owner"></a>Migration für den App-Besitzer

### <a name="before-you-migrate"></a>Vor der Migration

* **Optional**: Sichern Sie die Apps, die im LUIS-Portal in der Liste der Apps aufgeführt sind, indem Sie jede App exportieren oder die Export-[API](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c40) verwenden.
* **Optional**: Speichern Sie für jede App deren Liste der Projektmitarbeiter. Diese E-Mail-Liste wird als Bestandteil des Migrationsprozesses bereitgestellt.
* **Erforderlich**: Sie benötigen ein [Azure-Abonnement](https://azure.microsoft.com/free/). In einem Schritt des Abonnementprozesses sind Abrechnungsinformationen erforderlich. Sie können jedoch den Free-Tarif (`F0`) verwenden, wenn Sie LUIS verwenden. 

**Das Erstellen Ihrer LUIS-App ist kostenlos**, wie der `F0`-Tarif zeigt. Hier finden Sie [weitere Informationen zu Tarifen](luis-boundaries.md#key-limits).

Wenn Sie kein Azure-Abonnement haben, [registrieren Sie sich](https://azure.microsoft.com/free/). 

### <a name="migration-steps"></a>Schritte bei der Migration

Führen Sie [diese Migrationsschritte](luis-migration-authoring-steps.md) aus.

### <a name="after-you-migrate"></a>Nach der Migration 

Nach dem Migrationsprozess sind Ihre sämtlichen LUIS-Apps einer einzigen LUIS-Erstellungsressource zugewiesen.

Sie können weitere Erstellungsressourcen erstellen und diese im _LUIS-Portal_ über die Seite **Verwalten -> Azure-Ressourcen** zuweisen. 

Sie können einer Erstellungsressource Mitwirkende hinzufügen, indem Sie im _Azure-Portal_ zur **Zugriffssteuerung (IAM)** -Seite für diese Ressource wechseln. Weitere Informationen finden Sie unter [Nach dem Migrationsprozess: Hinzufügen von Mitwirkenden zu Ihrer Erstellungsressource](luis-migration-authoring-steps.md#after-the-migration-process-add-contributors-to-your-authoring-resource).

|Portal|Zweck|
|--|--|
|[Azure](https://azure.microsoft.com/free/)|* Erstellen von Vorhersage-und Erstellungsressourcen<br>* Zuweisen von Mitwirkenden|
|[LUIS](https://www.luis.ai)|* Migrieren zu neuen Erstellungsressourcen<br>* Zuweisen oder Aufheben der Zuweisung von Vorhersage- und Erstellungsressourcen zu Apps über die Seite **Verwalten -> Azure-Ressourcen**| 

## <a name="migration-for-the-app-contributor"></a>Migration für den App-Mitwirkenden

Jeder Benutzer von LUIS muss die Migration durchführen, einschließlich der Projektmitarbeiter/Mitwirkenden. 

### <a name="before-the-app-is-migrated"></a>Vor dem Migrieren der App

Sie können so vorgehen, dass Sie eine App, für die Sie ein Projektmitarbeiter sind, exportieren und dann wieder in LUIS importieren. Beim Importvorgang wird eine neue App mit einer neuen App-ID erstellt, deren Besitzer Sie sind.

### <a name="after-the-app-is-migrated"></a>Nach dem Migrieren der App

Der App-Besitzer muss [Ihre E-Mail-Adresse als Projektmitarbeiter zur Azure-Erstellungsressource hinzufügen](luis-how-to-collaborate.md#add-contributor-to-azure-authoring-resource). 

Nach dem Migrationsvorgang sind alle Apps, die Sie besitzen, auf der Seite **Meine Apps** des LUIS-Portals verfügbar.  

## <a name="troubleshooting"></a>Problembehandlung

LUIS-Erstellungsschlüssel sind im LUIS-Portal erst sichtbar, nachdem der Migrationsvorgang abgeschlossen wurde. Wenn Sie die Erstellungsschlüssel erstellen, beispielsweise mit der LUIS-Befehlszeilenschnittstelle, muss der Benutzer den Migrationsvorgang trotzdem abschließen. 

## <a name="next-steps"></a>Nächste Schritte

* [Schritte zum Migrieren zur Azure-Erstellungsressource](luis-migration-authoring-steps.md)