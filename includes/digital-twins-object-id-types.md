---
title: include file
description: include file
services: digital-twins
author: kingdomofends
ms.service: digital-twins
ms.topic: include
ms.date: 09/30/2019
ms.author: v-adgera
ms.custom: include file
ms.openlocfilehash: 1f4cdf45fb643499f7d6ab717c6f36395ad292c8
ms.sourcegitcommit: 7c2dba9bd9ef700b1ea4799260f0ad7ee919ff3b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2019
ms.locfileid: "71827696"
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