---
title: Verstehen von Ablehnungszuweisungen für Azure-Ressourcen | Microsoft-Dokumentation
description: Informationen zu Ablehnungszuweisungen in der rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC) für Azure-Ressourcen
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: ''
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/13/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: ''
ms.openlocfilehash: f15d6fd81337aa4a859539e86f37a516848c9370
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/17/2019
ms.locfileid: "67165980"
---
# <a name="understand-deny-assignments-for-azure-resources"></a>Verstehen von Ablehnungszuweisungen für Azure-Ressourcen

Ähnlich wie eine Rollenzuweisung verknüpft eine *Ablehnungszuweisung* in einem bestimmten Bereich einen Satz von Aktionen mit einem Benutzer, einer Gruppe oder einem Dienstprinzipal, um den Zugriff zu verweigern. Ablehnungszuweisungen hindern Benutzer an der Ausführung bestimmter Aktionen für Azure-Ressourcen, auch wenn ihnen über eine Rollenzuweisung Zugriff erteilt wird.

In diesem Artikel wird die Definition von Ablehnungszuweisungen beschrieben.

## <a name="how-deny-assignments-are-created"></a>Wie werden Ablehnungszuweisungen erstellt?

Ablehnungszuweisungen werden von Azure erstellt und verwaltet, um Ressourcen zu schützen. Azure Blueprints und von Azure verwaltete Apps verwenden Ablehnungszuweisungen beispielsweise, um systemseitig verwaltete Ressourcen zu schützen. Weitere Informationen finden Sie unter [Schützen neuer Ressourcen mit Azure Blueprints-Ressourcensperren](../governance/blueprints/tutorials/protect-new-resources.md).

## <a name="compare-role-assignments-and-deny-assignments"></a>Vergleich zwischen Rollenzuweisungen und Ablehnungszuweisungen

Ablehnungszuweisungen folgen einem ähnlichen Muster wie Rollenzuweisungen, weisen aber auch einige Unterschiede auf.

| Funktion | Rollenzuweisung | Ablehnungszuweisung |
| --- | --- | --- |
| Gewähren von Zugriff | :heavy_check_mark: |  |
| Zugriff verweigern |  | :heavy_check_mark: |
| Kann direkt erstellt werden | :heavy_check_mark: |  |
| Gelten in einem Bereich | :heavy_check_mark: | :heavy_check_mark: |
| Schließen Prinzipale aus |  | :heavy_check_mark: |
| Verhindern die Vererbung auf untergeordnete Bereiche |  | :heavy_check_mark: |
| Werden auch auf Zuweisungen für [klassische Abonnementadministratoren](rbac-and-directory-admin-roles.md) angewendet. |  | :heavy_check_mark: |

## <a name="deny-assignment-properties"></a>Eigenschaften von Ablehnungszuweisungen

 Eine Ablehnungszuweisungen hat folgende Eigenschaften:

> [!div class="mx-tableFixed"]
> | Eigenschaft | Erforderlich | Typ | BESCHREIBUNG |
> | --- | --- | --- | --- |
> | `DenyAssignmentName` | Ja | string | Der Anzeigename der Ablehnungszuweisung. Namen müssen für einen bestimmten Bereich eindeutig sein. |
> | `Description` | Nein | string | Die Beschreibung der Ablehnungszuweisung. |
> | `Permissions.Actions` | Mindestens ein Actions- oder ein DataActions-Element | String[] | Ein Array von Zeichenfolgen, welche die Verwaltungsvorgänge angeben, auf die die Ablehnungszuweisung den Zugriff blockiert. |
> | `Permissions.NotActions` | Nein | String[] | Ein Array von Zeichenfolgen, welche die Verwaltungsvorgänge angeben, die von der Ablehnungszuweisung auszuschließen sind. |
> | `Permissions.DataActions` | Mindestens ein Actions- oder ein DataActions-Element | String[] | Ein Array von Zeichenfolgen, welche die Datenvorgänge angeben, auf die die Ablehnungszuweisung den Zugriff blockiert. |
> | `Permissions.NotDataActions` | Nein | String[] | Ein Array von Zeichenfolgen, welche die Datenvorgänge angeben, die von der Ablehnungszuweisung auszuschließen sind. |
> | `Scope` | Nein | string | Eine Zeichenfolge, die den Bereich festlegt, für den die Ablehnungszuweisung gilt. |
> | `DoNotApplyToChildScopes` | Nein | Boolean | Gibt an, ob die Ablehnungszuweisung für untergeordnete Bereiche gilt. Der Standardwert ist „false“. |
> | `Principals[i].Id` | Ja | String[] | Ein Array aus Azure AD-Prinzipalobjekt-IDs (Benutzer, Gruppe, Dienstprinzipal oder verwaltete Identität), für die die Ablehnungszuweisung gilt. Die Festlegung einer leeren GUID `00000000-0000-0000-0000-000000000000` repräsentiert alle Prinzipale. |
> | `Principals[i].Type` | Nein | String[] | Ein Array von Objekttypen, das durch „Principals[i].Id“ dargestellt wird. Eine leere GUID `SystemDefined` repräsentiert alle Prinzipale. |
> | `ExcludePrincipals[i].Id` | Nein | String[] | Ein Array aus Azure AD-Prinzipalobjekt-IDs (Benutzer, Gruppe, Dienstprinzipal oder verwaltete Identität), für die die Ablehnungszuweisung nicht gilt. |
> | `ExcludePrincipals[i].Type` | Nein | String[] | Ein Array von Objekttypen, das durch „ExcludePrincipals[i].Id“ dargestellt wird. |
> | `IsSystemProtected` | Nein | Boolean | Gibt an, ob diese Ablehnungszuweisung von Azure erstellt wurde und nicht bearbeitet oder gelöscht werden kann. Derzeit sind alle Ablehnungszuweisungen vom System geschützt. |

## <a name="the-all-principals-principal"></a>Der Prinzipal „Alle Prinzipale“

Zur Unterstützung von Ablehnungszuweisungen wurde ein systemseitig definierter Prinzipal namens *Alle Prinzipale* eingeführt. Dieser Prinzipal repräsentiert alle Benutzer, Gruppen, Dienstprinzipale und verwalteten Identitäten in einem Azure AD-Verzeichnis. Wenn die Prinzipal-ID eine aus Nullen bestehende GUID `00000000-0000-0000-0000-000000000000` ist und der Prinzipaltyp `SystemDefined` lautet, repräsentiert der Prinzipal alle Prinzipale. In der Azure PowerShell-Ausgabe sieht „Alle Prinzipale“ wie folgt aus:

```azurepowershell
Principals              : {
                          DisplayName:  All Principals
                          ObjectType:   SystemDefined
                          ObjectId:     00000000-0000-0000-0000-000000000000
                          }
```

„Alle Prinzipale“ kann mit `ExcludePrincipals` kombiniert werden, um den Zugriff für alle Prinzipale mit Ausnahme einiger Benutzer zu verweigern. „Alle Prinzipale“ hat die folgenden Einschränkungen:

- Kann nur in `Principals` verwendet werden, aber nicht in `ExcludePrincipals`.
- `Principals[i].Type` muss auf `SystemDefined` festgelegt werden.

## <a name="next-steps"></a>Nächste Schritte

* [Auflisten von Ablehnungszuweisungen für Azure-Ressourcen mit dem Azure-Portal](deny-assignments-portal.md)
* [Grundlegendes zu Rollendefinitionen für Azure-Ressourcen](role-definitions.md)
