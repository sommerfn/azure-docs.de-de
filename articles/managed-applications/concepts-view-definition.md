---
title: Übersicht der Ansichtsdefinition in Azure Managed Applications| Microsoft-Dokumentation
description: Beschreibt das Konzept der Erstellung einer Ansichtsdefinition für Azure Managed Applications.
services: managed-applications
ms.service: managed-applications
ms.topic: conceptual
ms.author: lazinnat
author: lazinnat
ms.date: 06/12/2019
ms.openlocfilehash: b23e844cb550a98328951bc6efae3c5039ff73bf
ms.sourcegitcommit: c62a68ed80289d0daada860b837c31625b0fa0f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2019
ms.locfileid: "73607537"
---
# <a name="view-definition-artifact-in-azure-managed-applications"></a>Ansichtsdefinitionsartefakt in Azure Managed Applications

Die Ansichtsdefinition ist ein optionales Artefakt in Azure Managed Applications. Es ermöglicht die Anpassung der Übersichtsseite und das Hinzufügen weiterer Ansichten wie Metriken und benutzerdefinierte Ressourcen.

Dieser Artikel gibt eine Übersicht über das Ansichtsdefinitionsartefakt und seine Funktionen.

## <a name="view-definition-artifact"></a>Anzeigen des Definitionsartefakts

Das Ansichtsdefinitionsartefakt muss **viewDefinition.json** genannt werden und auf der gleichen Ebene wie **createUiDefinition.json** und **mainTemplate.json** im ZIP-Paket platziert sein, das eine Definition einer verwalteten Anwendung erstellt. Unter [Veröffentlichen einer Definition für eine verwaltete Azure-Anwendung](publish-managed-app-definition-quickstart.md) erfahren Sie, wie Sie das ZIP-Paket erstellen und eine Definition für eine verwaltete Anwendung veröffentlichen.

## <a name="view-definition-schema"></a>Ansichtsdefinitionsschema

Die **viewDefinition.json**-Datei hat nur eine `views`-Eigenschaft der obersten Ebene, die ein Array von Ansichten ist. Jede Ansicht wird in der Benutzeroberfläche der verwalteten Anwendung als separates Menüelement im Inhaltsverzeichnis angezeigt. Jede Ansicht hat eine `kind`-Eigenschaft, die den Typ der Ansicht festlegt. Für sie muss einer der folgenden Werte festgelegt werden: [Overview](#overview), [Metrics](#metrics), [CustomResources](#custom-resources), [Associations](#associations). Weitere Informationen finden Sie im aktuellen [JSON-Schema für viewDefinition.json](https://schema.management.azure.com/schemas/viewdefinition/0.0.1-preview/ViewDefinition.json#).

JSON-Beispielcode für die Ansichtsdefinition:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/viewdefinition/0.0.1-preview/ViewDefinition.json#",
    "contentVersion": "0.0.0.1",
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
                        "displayName": "Custom Context Action",
                        "path": "testCustomResource/testContextAction",
                        "icon": "Stop",
                        "createUIDefinition": { }
                    }
                ],
                "columns": [
                    {"key": "name", "displayName": "Name"},
                    {"key": "properties.myProperty1", "displayName": "Property 1"},
                    {"key": "properties.myProperty2", "displayName": "Property 2", "optional": true}
                ]
            }
        },
        {
            "kind": "Associations",
            "properties": {
                "displayName": "Test association resource type",
                "version": "1.0.0",
                "targetResourceType": "Microsoft.Compute/virtualMachines",
                "createUIDefinition": { }
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

![Übersicht](./media/view-definition/overview.png)

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

![metrics](./media/view-definition/metrics.png)

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
        "icon": "Polychromatic.ResourceList",
        "createUIDefinition": { },
        "commands": [
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
|icon|Nein|Das Symbol der Ansicht. Die Liste der Beispielsymbole ist im [JSON-Schema](https://schema.management.azure.com/schemas/viewdefinition/0.0.1-preview/ViewDefinition.json#) definiert.|
|createUIDefinition|Nein|Schema zum Erstellen von Benutzeroberflächendefinitionen für den Befehl zum Erstellen benutzerdefinierter Ressourcen. Eine Einführung zum Erstellen von Benutzeroberflächendefinitionen finden Sie unter [Erste Schritte mit CreateUiDefinition](create-uidefinition-overview.md).|
|commands|Nein|Das Array der zusätzlichen Symbolleisten-Schaltflächen der Ansicht CustomResources finden Sie unter [commands](#commands).|
|columns|Nein|Das Array von Spalten der benutzerdefinierten Ressource. Wenn nicht definiert, wird standardmäßig die `name`-Spalte angezeigt. Die Spalte muss einen `"key"` und einen `"displayName"` besitzen. Geben Sie für „key“ den Schlüssel der Eigenschaft zur Anzeige in einer Ansicht ein. Wenn geschachtelt, verwenden Sie einen Punkt als Trennzeichen, z.B. `"key": "name"` oder `"key": "properties.property1"`. Geben Sie für „displayName“ den Anzeigenamen der Eigenschaft zur Anzeige in einer Ansicht ein. Sie können auch eine `"optional"`-Eigenschaft angeben. Bei Festlegung auf „True“ wird die Spalte in einer Ansicht standardmäßig ausgeblendet.|

![CustomResources](./media/view-definition/customresources.png)

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
|icon|Nein|Das Symbol der Befehlsschaltfläche. Die Liste der Beispielsymbole ist im [JSON-Schema](https://schema.management.azure.com/schemas/viewdefinition/0.0.1-preview/ViewDefinition.json#) definiert.|
|createUIDefinition|Nein|Schema zum Erstellen von Benutzeroberflächendefinitionen für den Befehl. Eine Einführung zum Erstellen von Benutzeroberflächendefinitionen finden Sie unter [Erste Schritte mit „CreateUiDefinition“](create-uidefinition-overview.md).|

## <a name="associations"></a>Associations

`"kind": "Associations"`

Sie können mehrere Ansichten dieses Typs definieren. In dieser Ansicht können Sie vorhandene Ressourcen mit der verwalteten Anwendung über den benutzerdefinierten Anbieter verknüpfen, den Sie in **mainTemplate.json** definiert haben. Eine Einführung in benutzerdefinierte Anbieter finden Sie unter [Azure Custom Providers (Vorschau): Übersicht](custom-providers-overview.md).

In dieser Ansicht können Sie vorhandene Azure-Ressourcen basierend auf dem `targetResourceType` erweitern. Wenn eine Ressource ausgewählt wird, wird eine Onboardinganforderung für den **öffentlichen** benutzerdefinierten Anbieter erstellt, der einen Nebeneffekt auf die Ressource anwenden kann. 

```json
{
    "kind": "Associations",
    "properties": {
        "displayName": "Test association resource type",
        "version": "1.0.0",
        "targetResourceType": "Microsoft.Compute/virtualMachines",
        "createUIDefinition": { }
    }
}
```

|Eigenschaft|Erforderlich|BESCHREIBUNG|
|---------|---------|---------|
|displayName|Ja|Der angezeigte Titel der Ansicht. Der Titel muss **eindeutig** sein für jede Associations-Ansicht in Ihrer **viewDefinition.json**-Datei.|
|version|Nein|Die Version der Plattform, die zum Rendern der Ansicht verwendet wird.|
|targetResourceType|Ja|Der Zielressourcentyp. Dies ist der Ressourcentyp, der für das Ressourcenonboarding angezeigt wird.|
|createUIDefinition|Nein|Schema zum Erstellen von Benutzeroberflächendefinitionen für den Befehl zum Erstellen von Zuordnungsressourcen (Assoziationsressourcen). Eine Einführung zum Erstellen von Benutzeroberflächendefinitionen finden Sie unter [Erste Schritte mit CreateUiDefinition](create-uidefinition-overview.md).|

## <a name="looking-for-help"></a>Wenn Hilfe benötigt wird

Fragen zu Azure Managed Applications können bei [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-managedapps) gestellt werden. Eine ähnliche Frage wurde möglicherweise bereits gestellt und beantwortet, weshalb Sie dies zunächst prüfen sollten, bevor Sie etwas posten. Fügen Sie das Tag `azure-managedapps` hinzu, um schnell eine Antwort zu erhalten!

## <a name="next-steps"></a>Nächste Schritte

- Eine Einführung in verwaltete Anwendungen finden Sie in der [Übersicht über verwaltete Azure-Anwendungen](overview.md).
- Eine Einführung in benutzerdefinierte Anbieter finden Sie unter [Azure Custom Providers: Übersicht](custom-providers-overview.md).
- Informationen zum Erstellen einer verwalteten Azure-Anwendung mit benutzerdefinierten Azure-Anbietern finden Sie unter [Tutorial: Erstellen einer verwalteten Anwendung mit benutzerdefinierten Aktionen und Ressourcen](tutorial-create-managed-app-with-custom-provider.md).
