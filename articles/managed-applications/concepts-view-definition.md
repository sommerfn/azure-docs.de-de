---
title: Übersicht der Ansichtsdefinition in Azure Managed Applications| Microsoft-Dokumentation
description: Beschreibt das Konzept der Erstellung einer Ansichtsdefinition für Azure Managed Applications.
services: managed-applications
ms.service: managed-applications
ms.topic: conceptual
ms.author: lazinnat
author: lazinnat
ms.date: 06/12/2019
ms.openlocfilehash: 6735787f9b43f98ab611584f3c7191c9f927dbc2
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/29/2019
ms.locfileid: "67479055"
---
# <a name="view-definition-artifact-in-azure-managed-applications"></a>Ansichtsdefinitionsartefakt in Azure Managed Applications

Die Ansichtsdefinition ist ein optionales Artefakt in Azure Managed Applications. Es ermöglicht die Anpassung der Übersichtsseite und das Hinzufügen weiterer Ansichten wie Metriken und benutzerdefinierte Ressourcen.

Dieser Artikel gibt eine Übersicht über das Ansichtsdefinitionsartefakt und seine Funktionen.

## <a name="view-definition-artifact"></a>Anzeigen des Definitionsartefakts

Das Ansichtsdefinitionsartefakt muss **viewDefinition.json** genannt werden und auf der gleichen Ebene wie **createUiDefinition.json** und **mainTemplate.json** im ZIP-Paket platziert sein, das eine Definition einer verwalteten Anwendung erstellt. Unter [Veröffentlichen einer Definition für eine verwaltete Azure-Anwendung](publish-managed-app-definition-quickstart.md) erfahren Sie, wie Sie das ZIP-Paket erstellen und eine Definition für eine verwaltete Anwendung veröffentlichen.

## <a name="view-definition-schema"></a>Ansichtsdefinitionsschema

Die **viewDefinition.json**-Datei hat nur eine `views`-Eigenschaft der obersten Ebene, die ein Array von Ansichten ist. Jede Ansicht wird in der Benutzeroberfläche der verwalteten Anwendung als separates Menüelement im Inhaltsverzeichnis angezeigt. Jede Ansicht hat eine `kind`-Eigenschaft, die den Typ der Ansicht festlegt. Für sie muss einer der folgenden Werte festgelegt werden: [Overview](#overview), [Metrics](#metrics), [CustomResources](#custom-resources). Weitere Informationen finden Sie im aktuellen [JSON-Schema für viewDefinition.json](https://schema.management.azure.com/schemas/viewdefinition/0.0.1-preview/ViewDefinition.json#).

JSON-Beispielcode für die Ansichtsdefinition:

```json
{
    "views": [
        {
            "kind": "Overview",
            "properties": {
                "header": "Welcome to your Azure Managed Application",
                "description": "This managed application is for demo purposes only.",
                "commands": [
                    {
                        "displayName": "Test Action",
                        "path": "testAction"
                    }
                ]
            }
        },
        {
            "kind": "Metrics",
            "properties": {
                "displayName": "This is my metrics view",
                "version": "1.0.0",
                "charts": [
                    {
                        "displayName": "Sample chart",
                        "chartType": "Bar",
                        "metrics": [
                            {
                                "name": "Availability",
                                "aggregationType": "avg",
                                "resourceTagFilter": [ "tag1" ],
                                "resourceType": "Microsoft.Storage/storageAccounts",
                                "namespace": "Microsoft.Storage/storageAccounts"
                            }
                        ]
                    }
                ]
            }
        },
        {
            "kind": "CustomResources",
            "properties": {
                "displayName": "Test custom resource type",
                "version": "1.0.0",
                "resourceType": "testCustomResource",
                "createUIDefinition": { },
                "commands": [
                    {
                        "displayName": "Custom Test Action",
                        "path": "testAction"
                    },
                    {
                        "displayName": "Custom Context Action",
                        "path": "testCustomResource/testContextAction",
                        "icon": "Stop",
                        "createUIDefinition": { },
                    }
                ],
                "columns": [
                    {"key": "name", "displayName": "Name"},
                    {"key": "properties.myProperty1", "displayName": "Property 1"},
                    {"key": "properties.myProperty2", "displayName": "Property 2", "optional": true}
                ]
            }
        }
    ]
}

```

## <a name="overview"></a>Übersicht

`"kind": "Overview"`

Wenn Sie diese Ansicht in **viewDefinition.json** bereitstellen, überschreibt sie die Standardübersichtsseite in Ihrer verwalteten Anwendung.

```json
{
    "kind": "Overview",
    "properties": {
        "header": "Welcome to your Azure Managed Application",
        "description": "This managed application is for demo purposes only.",
        "commands": [
            {
                "displayName": "Test Action",
                "path": "testAction"
            }
        ]
    }
}
```

|Eigenschaft|Erforderlich|BESCHREIBUNG|
|---------|---------|---------|
|Header|Nein|Die Überschrift der Übersichtsseite.|
|description|Nein|Die Beschreibung Ihrer verwalteten Anwendung.|
|commands|Nein|Das Array der zusätzlichen Symbolleisten-Schaltflächen der Übersichtsseite finden Sie unter [commands](#commands).|

## <a name="metrics"></a>metrics

`"kind": "Metrics"`

Die Metrikenansicht ermöglicht Ihnen, Daten von Ihren verwalteten Anwendungsressourcen in [Azure Monitor-Metriken](../azure-monitor/platform/data-platform-metrics.md) zu sammeln und zu aggregieren.

```json
{
    "kind": "Metrics",
    "properties": {
        "displayName": "This is my metrics view",
        "version": "1.0.0",
        "charts": [
            {
                "displayName": "Sample chart",
                "chartType": "Bar",
                "metrics": [
                    {
                        "name": "Availability",
                        "aggregationType": "avg",
                        "resourceTagFilter": [ "tag1" ],
                        "resourceType": "Microsoft.Storage/storageAccounts",
                        "namespace": "Microsoft.Storage/storageAccounts"
                    }
                ]
            }
        ]
    }
}
```

|Eigenschaft|Erforderlich|BESCHREIBUNG|
|---------|---------|---------|
|displayName|Nein|Der angezeigte Titel der Ansicht.|
|version|Nein|Die Version der Plattform, die zum Rendern der Ansicht verwendet wird.|
|charts|Ja|Das Array von Diagrammen der Metrikenseite.|

### <a name="chart"></a>Diagramm

|Eigenschaft|Erforderlich|BESCHREIBUNG|
|---------|---------|---------|
|displayName|Ja|Der angezeigte Titel des Diagramms.|
|chartType|Nein|Die Visualisierung, die für dieses Diagramm verwendet werden soll. Standardmäßig wird ein Liniendiagramm dargestellt. Unterstützte Diagrammtypen: `Bar, Line, Area, Scatter`.|
|metrics|Ja|Das Array von Metriken, die in diesem Diagramm dargestellt werden sollen. Weitere Informationen zu unterstützten Metriken im Azure-Portal finden Sie unter [Unterstützte Metriken von Azure Monitor](../azure-monitor/platform/metrics-supported.md).|

### <a name="metric"></a>Metrik

|Eigenschaft|Erforderlich|BESCHREIBUNG|
|---------|---------|---------|
|name|Ja|Der Name der Metrik.|
|aggregationType|Ja|Der Aggregationstyp, der für diese Metrik verwendet werden soll. Unterstützte Aggregationstypen: `none, sum, min, max, avg, unique, percentile, count`|
|Namespace|Nein|Weitere Informationen, die beim Ermitteln der richtigen Metrikenanbieter verwendet werden.|
|resourceTagFilter|Nein|Das Ressourcentagsarray (wird mit dem `or`-Wort getrennt), für das Metriken angezeigt würden. Wird zusätzlich zum Ressourcentypfilter angewandt.|
|resourceType|Ja|Der Ressourcentyp, für den Metriken angezeigt würden.|

## <a name="custom-resources"></a>Benutzerdefinierte Ressourcen

`"kind": "CustomResources"`

Sie können mehrere Ansichten dieses Typs definieren. Jede Ansicht stellt einen **eindeutigen** benutzerdefinierten Ressourcentyp vom benutzerdefinierten Anbieter dar, den Sie die in **mainTemplate.json** definiert haben. Eine Einführung in benutzerdefinierte Anbieter finden Sie unter [Azure Custom Providers (Vorschau): Übersicht](custom-providers-overview.md).

In dieser Ansicht können Sie für Ihren benutzerdefinierten Ressourcentyp GET-, PUT-, DELETE- und POST-Operationen durchführen. POST-Operationen könnten globale benutzerdefinierte Aktionen oder benutzerdefinierte Aktionen in einem Kontext Ihres benutzerdefinierten Ressourcentyps sein.

```json
{
    "kind": "CustomResources",
    "properties": {
        "displayName": "Test custom resource type",
        "version": "1.0.0",
        "resourceType": "testCustomResource",
        "createUIDefinition": { },
        "commands": [
            {
                "displayName": "Custom Test Action",
                "path": "testAction"
            },
            {
                "displayName": "Custom Context Action",
                "path": "testCustomResource/testContextAction",
                "icon": "Stop",
                "createUIDefinition": { },
            }
        ],
        "columns": [
            {"key": "name", "displayName": "Name"},
            {"key": "properties.myProperty1", "displayName": "Property 1"},
            {"key": "properties.myProperty2", "displayName": "Property 2", "optional": true}
        ]
    }
}
```

|Eigenschaft|Erforderlich|BESCHREIBUNG|
|---------|---------|---------|
|displayName|Ja|Der angezeigte Titel der Ansicht. Der Titel muss **eindeutig** sein für jede CustomResources-Ansicht in Ihrer **viewDefinition.json**-Datei.|
|version|Nein|Die Version der Plattform, die zum Rendern der Ansicht verwendet wird.|
|resourceType|Ja|Der Name des benutzerdefinierten Ressourcentyps. Muss ein **eindeutiger** benutzerdefinierter Ressourcentyp des benutzerdefinierten Anbieters sein.|
|createUIDefinition|Nein|Schema zum Erstellen von Benutzeroberflächendefinitionen für den Befehl zum Erstellen benutzerdefinierter Ressourcen. Eine Einführung zum Erstellen von Benutzeroberflächendefinitionen finden Sie unter [Erste Schritte mit CreateUiDefinition](create-uidefinition-overview.md).|
|commands|Nein|Das Array der zusätzlichen Symbolleisten-Schaltflächen der Ansicht CustomResources finden Sie unter [commands](#commands).|
|columns|Nein|Das Array von Spalten der benutzerdefinierten Ressource. Wenn nicht definiert, wird standardmäßig die `name`-Spalte angezeigt. Die Spalte muss einen `"key"` und einen `"displayName"` besitzen. Geben Sie für „key“ den Schlüssel der Eigenschaft zur Anzeige in einer Ansicht ein. Wenn geschachtelt, verwenden Sie einen Punkt als Trennzeichen, z.B. `"key": "name"` oder `"key": "properties.property1"`. Geben Sie für „displayName“ den Anzeigenamen der Eigenschaft zur Anzeige in einer Ansicht ein. Sie können auch eine `"optional"`-Eigenschaft angeben. Bei Festlegung auf „True“ wird die Spalte in einer Ansicht standardmäßig ausgeblendet.|

## <a name="commands"></a>Befehle

„Commands“ ist ein Array von zusätzlichen Symbolleisten-Schaltflächen, die auf der Seite angezeigt werden. Jeder Befehl stellt eine POST-Aktion von Ihrem in **mainTemplate.json** definierten Azure Custom Provider dar. Eine Einführung in benutzerdefinierte Anbieter finden Sie unter [Azure Custom Providers: Übersicht](custom-providers-overview.md).

```json
{
    "commands": [
        {
            "displayName": "Start Test Action",
            "path": "testAction",
            "icon": "Start",
            "createUIDefinition": { }
        },
    ]
}
```

|Eigenschaft|Erforderlich|BESCHREIBUNG|
|---------|---------|---------|
|displayName|Ja|Der angezeigte Name der Befehlsschaltfläche.|
|path|Ja|Der Aktionsname des benutzerdefinierten Anbieters. Die Aktion muss in **mainTemplate.json** definiert werden.|
|icon|Nein|Das Symbol der Befehlsschaltfläche. Die Liste der unterstützten Symbole ist im [JSON-Schema](https://schema.management.azure.com/schemas/viewdefinition/0.0.1-preview/ViewDefinition.json#) definiert.|
|createUIDefinition|Nein|Schema zum Erstellen von Benutzeroberflächendefinitionen für den Befehl. Eine Einführung zum Erstellen von Benutzeroberflächendefinitionen finden Sie unter [Erste Schritte mit „CreateUiDefinition“](create-uidefinition-overview.md).|

## <a name="next-steps"></a>Nächste Schritte

- Eine Einführung in verwaltete Anwendungen finden Sie in der [Übersicht über verwaltete Azure-Anwendungen](overview.md).
- Eine Einführung in benutzerdefinierte Anbieter finden Sie unter [Azure Custom Providers: Übersicht](custom-providers-overview.md).
