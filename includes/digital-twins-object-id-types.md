---
title: include file
description: include file
services: digital-twins
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
ms.topic: include
ms.date: 09/30/2019
ms.custom: include file
ms.openlocfilehash: 12d4278171d43fdaf8613a1c91bdbffc269adc56
ms.sourcegitcommit: 4f7dce56b6e3e3c901ce91115e0c8b7aab26fb72
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/04/2019
ms.locfileid: "71949038"
---
`objectIdType` (oder **Objektbezeichnertyp**) bezieht sich auf den Typ der Identität, der einer Rolle zugewiesen wird. Mit Ausnahme der Typen `DeviceId` und `UserDefinedFunctionId` entsprechen Objektbezeichnertypen Eigenschaften von Azure Active Directory-Objekten.

Die folgende Tabelle enthält die unterstützten Objektbezeichnertypen in Azure Digital Twins:

| type | BESCHREIBUNG |
| --- | --- |
| UserId | Weist einem Benutzer eine Rolle zu. |
| deviceId | Weist einem Gerät eine Rolle zu. |
| DomainName | Weist einem Domänennamen eine Rolle zu. Jeder Benutzer mit dem angegebenen Domänennamen hat die Zugriffsrechte der entsprechenden Rolle. |
| TenantId | Weist einem Mandanten eine Rolle zu. Jeder Benutzer, der zu der angegebenen Azure AD-Mandanten-ID gehört, hat die Zugriffsrechte der entsprechenden Rolle. |
| ServicePrincipalId | Weist einer Dienstprinzipalobjekt-ID eine Rolle zu. |
| UserDefinedFunctionId | Weist einer benutzerdefinierten Funktion (User-defined Function, UDF) eine Rolle zu. |