---
title: 'Verwaltete Azure-Anwendung: Referenz für das Ansichtsdefinitionsartefakt'
description: Bei diesem Artikel handelt es sich um einen Referenzartikel für das Ansichtsdefinitionsartefakt.
services: managed-applications
ms.service: managed-applications
ms.topic: conceptual
ms.author: lazinnat
author: lazinnat
ms.date: 07/11/2019
ms.openlocfilehash: e60f26fe0a7144d768bac020d62c61cb92594914
ms.sourcegitcommit: e9c866e9dad4588f3a361ca6e2888aeef208fc35
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/19/2019
ms.locfileid: "68336652"
---
# <a name="reference-view-definition-artifact"></a>Referenz: Anzeigen des Definitionsartefakts

Bei diesem Artikel handelt es sich um einen Referenzartikel für ein Artefakt vom Typ *viewDefinition.json* in Azure Managed Applications. Weitere Informationen zum Erstellen von Ansichtskonfigurationen finden Sie unter [Ansichtsdefinitionsartefakt in Azure Managed Applications](concepts-view-definition.md).

## <a name="view-definition"></a>Anzeigen der Definition

Der folgende JSON-Code zeigt ein Beispiel für eine Datei vom Typ *viewDefinition.json* für Azure Managed Applications:

```json
{
  "views": [{
    "kind": "Overview",
    "properties": {
      "header": "Welcome to your Demo Azure Managed Application",
      "description": "This Managed application with Custom Provider is for demo purposes only.",
      "commands": [{
          "displayName": "Ping Action",
          "path": "/customping",
          "icon": "LaunchCurrent"
      }]
    }
  },
  {
    "kind": "CustomResources",
    "properties": {
      "displayName": "Users",
      "version": "1.0.0.0",
      "resourceType": "users",
      "createUIDefinition": {
        "parameters": {
          "steps": [{
            "name": "add",
            "label": "Add user",
            "elements": [{
              "name": "name",
              "label": "User's Full Name",
              "type": "Microsoft.Common.TextBox",
              "defaultValue": "",
              "toolTip": "Provide a full user name.",
              "constraints": { "required": true }
            },
            {
              "name": "location",
              "label": "User's Location",
              "type": "Microsoft.Common.TextBox",
              "defaultValue": "",
              "toolTip": "Provide a Location.",
              "constraints": { "required": true }
            }]
          }],
          "outputs": {
            "name": "[steps('add').name]",
            "properties": {
              "FullName": "[steps('add').name]",
              "Location": "[steps('add').location]"
            }
          }
        }
      },
      "commands": [{
        "displayName": "Custom Context Action",
        "path": "users/contextAction",
        "icon": "Start"
      }],
      "columns": [
        { "key": "properties.FullName", "displayName": "Full Name" },
        { "key": "properties.Location", "displayName": "Location", "optional": true }
      ]
    }
  }]
}
```

## <a name="next-steps"></a>Nächste Schritte

- [Tutorial: Erstellen einer verwalteten Anwendung mit benutzerdefinierten Aktionen und Ressourcen](tutorial-create-managed-app-with-custom-provider.md)
- [Referenz: Artefakt für Benutzeroberflächenelemente](reference-createuidefinition-artifact.md)
- [Referenz: Bereitstellungsvorlagenartefakt](reference-main-template-artifact.md)