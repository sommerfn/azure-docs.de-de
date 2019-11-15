---
title: Übersicht über benutzerdefinierte Rollendefinitionen in Azure Managed Applications | Microsoft-Dokumentation
description: Beschreibt das Konzept der Erstellung von benutzerdefinierten Rollendefinitionen für verwaltete Anwendungen.
services: managed-applications
ms.service: managed-applications
ms.topic: conceptual
ms.author: jobreen
author: jjbfour
ms.date: 09/16/2019
ms.openlocfilehash: 7f4371bea467d6d4c99a776e03cdf13070d77ac6
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73818398"
---
# <a name="custom-role-definition-artifact-in-azure-managed-applications"></a>Artefakt für benutzerdefinierte Rollendefinitionen in Azure Managed Applications

Benutzerdefinierte Rollendefinitionen sind ein optionales Artefakt in Managed Applications. Sie werden verwendet, um festzulegen, welche Berechtigungen die verwaltete Anwendung für ihre Funktionen benötigt.

Dieser Artikel bietet eine Übersicht über das Artefakt für benutzerdefinierte Rollendefinitionen und seine Funktionen.

## <a name="custom-role-definition-artifact"></a>Artefakt für benutzerdefinierte Rollendefinitionen

Sie müssen dem Artefakt für benutzerdefinierte Rollendefinitionen den Namen „customRoleDefinition.json“ geben. Platzieren Sie es in dem ZIP-Paket, das eine Definition eine verwalteten Anwendung erstellt, auf derselben Ebene wie „createUiDefinition.json“ und „mainTemplate.json“. Unter [Veröffentlichen einer Definition für eine verwaltete Anwendung](publish-managed-app-definition-quickstart.md) erfahren Sie, wie Sie das ZIP-Paket erstellen und eine Definition für eine verwaltete Anwendung veröffentlichen.

## <a name="custom-role-definition-schema"></a>Schema einer benutzerdefinierten Rollendefinition

Die Datei „customRoleDefinition.json“ weist eine `roles`-Eigenschaft der obersten Ebene auf, die ein Array von Rollen ist. Diese Rollen sind die erforderlichen Berechtigungen für die verwaltete Anwendung. Derzeit sind nur integrierte Rollen zulässig, Sie können jedoch mehrere Rollen angeben. Auf eine Rolle kann durch die ID der Rollendefinition oder durch den Rollennamen verwiesen werden.

JSON-Beispiel für eine benutzerdefinierte Rollendefinition:

```json
{
    "contentVersion": "0.0.0.1",
    "roles": [
        {
            "properties": {
                "roleName": "Contributor"
            }
        },
        {
            "id": "acdd72a7-3385-48ef-bd42-f606fba81ae7"
        },
        {
            "id": "/providers/Microsoft.Authorization/roledefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c"
        }
    ]
}
```

## <a name="roles"></a>Rollen

Eine Rolle umfasst eines der Elemente `$.properties.roleName` oder `id`:

```json
{
    "id": null,
    "properties": {
        "roleName": "Contributor"
    }
}
```

> [!NOTE]
> Sie können das Feld `id` oder das Feld `roleName` verwenden. Nur eines ist erforderlich. Mithilfe dieser Felder wird ermittelt, welche Rollendefinition angewandt werden soll. Wenn beide bereitgestellt werden, wird das Feld `id` verwendet.

|Eigenschaft|Erforderlich?|BESCHREIBUNG|
|---------|---------|---------|
|id|Ja|Die ID der integrierten Rolle. Sie können die vollständige ID oder nur die GUID verwenden.|
|roleName|Ja|Der Name der integrierten Rolle.|