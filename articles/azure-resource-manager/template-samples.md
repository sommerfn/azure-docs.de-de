---
title: Beispiele für Azure Resource Manager-Vorlagen
description: Azure Resource Manager-Vorlagenbeispiele zum Bereitstellen von Verwaltungsfunktionen wie Rollen und Sperren.
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.date: 11/16/2018
ms.author: tomfitz
ms.openlocfilehash: e342507b584a405637fc6728dfcd6e49199a154f
ms.sourcegitcommit: bb65043d5e49b8af94bba0e96c36796987f5a2be
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2019
ms.locfileid: "72390177"
---
# <a name="azure-resource-manager-templates-for-management-features"></a>Azure Resource Manager-Vorlagen für Verwaltungsfunktionen

Die folgende Tabelle enthält Links zu Azure Resource Manager-Vorlagen für Funktionen, die im Resource Manager verfügbar sind.

| | |
|-|-|
|**Rollenzuweisungen**||
| [Rolle für Ressourcengruppe zuweisen](https://github.com/Azure/azure-quickstart-templates/tree/master/101-rbac-builtinrole-resourcegroup)| Weist einem Benutzer eine integrierte Rolle für eine vorhandene Ressourcengruppe zu. |
| [Rolle für vorhandene VM zuweisen](https://github.com/Azure/azure-quickstart-templates/tree/master/101-rbac-builtinrole-virtualmachine)| Weist einem Benutzer eine integrierte Rolle für eine vorhandene VM zu. |
| [Rolle für mehrere VMs zuweisen](https://github.com/Azure/azure-quickstart-templates/tree/master/201-rbac-builtinrole-multipleVMs)| Weist Benutzern integrierte Rollen für mehrere VMs zu. |
| [Rolle für Azure-Abonnement zuweisen](https://github.com/Azure/azure-quickstart-templates/tree/master/subscription-level-deployments/subscription-role-assigment)| Weist einem Benutzer eine Rolle für ein Azure-Abonnement zu. |
|**Rollendefinition**||
| [Benutzerdefinierte Rollendefinition erstellen](https://github.com/Azure/azure-quickstart-templates/tree/master/subscription-level-deployments/create-role-def)| Erstellt eine neue Rollendefinition in einem Azure-Abonnement. |
|**Ressourcensperre**||
| [Ressourcengruppe sperren](https://github.com/Azure/azure-quickstart-templates/tree/master/subscription-level-deployments/create-rg-lock-role-assignment)| Erstellt eine Ressourcengruppe und wendet eine **DoNotDelete**-Sperre auf die Ressourcengruppe an. Weist einem Benutzer die Rolle „Mitwirkender“ zu. |
| | |
