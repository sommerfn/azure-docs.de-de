---
title: Includedatei
description: Includedatei
services: azure-policy
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 05/30/2019
ms.author: dacoulte
ms.custom: include file
ms.openlocfilehash: 7907504401f4b47aafe6032ea895d9647e6c303c
ms.sourcegitcommit: c05618a257787af6f9a2751c549c9a3634832c90
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/30/2019
ms.locfileid: "66420799"
---
Für jeden Objekttyp für Azure Policy gilt eine maximale Anzahl. Ein _Scope_-Eintrag (Bereich) gilt entweder für das Abonnement oder die [Verwaltungsgruppe](../articles/governance/management-groups/overview.md).

| Hierbei gilt: | Was | Maximale Anzahl |
|---|---|---|
| `Scope` | Richtliniendefinitionen | 250 |
| `Scope` | Initiativdefinitionen | 100 |
| Tenant | Initiativdefinitionen | 1.000 |
| `Scope` | Richtlinien- oder Initiativenzuweisungen | 100 |
| Richtliniendefinition | Parameter | 20 |
| Initiativdefinition | Richtlinien | 100 |
| Initiativdefinition | Parameter | 100 |
| Richtlinien- oder Initiativenzuweisungen | Ausschlüsse (notScopes) | 400 |
| Richtlinienregel | Geschachtelte konditionelle Abschnitte | 512 |
