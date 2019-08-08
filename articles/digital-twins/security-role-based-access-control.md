---
title: Grundlegendes zu rollenbasierter Zugriffssteuerung in Azure Digital Twins | Microsoft-Dokumentation
description: Informationen zur Authentifizierung in Digital Twins mit rollenbasierter Zugriffssteuerung.
author: lyrana
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 07/29/2019
ms.author: lyhughes
ms.openlocfilehash: 6602a4df2c6e313156010102780240ba19fbac95
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2019
ms.locfileid: "68815570"
---
# <a name="role-based-access-control-in-azure-digital-twins"></a>Rollenbasierte Zugriffssteuerung in Azure Digital Twins

Azure Digital Twins ermöglicht präzise Zugriffssteuerung für bestimmte Daten, Ressourcen und Aktionen in Ihrem Raumgraphen. Dies geschieht durch eine granulare Rollen- und Berechtigungsverwaltung namens „Rollenbasierte Zugriffssteuerung“ (Role-Based Access Control, RBAC). RBAC besteht aus _Rollen_ und _Rollenzuweisungen_. Rollen bestimmen die Berechtigungsebene. Rollenzuweisungen ordnen einem Benutzer oder Gerät eine Rolle zu.

Mit RBAC können Berechtigungen zugewiesen werden für:

- Einen Benutzer
- Ein Gerät
- Ein Dienstprinzipal
- Eine benutzerdefinierte Funktion
- Alle Benutzer, die einer Domäne angehören.
- Einen Mandanten

Der Grad des Zugriffs kann darüber hinaus angepasst werden.

RBAC ist besonders, weil Berechtigungen im Raumgraphen von oben nach vererbt werden.

## <a name="what-can-i-do-with-rbac"></a>Welche Möglichkeiten bietet RBAC?

Ein Entwickler kann RBAC für folgende Aufgaben verwenden:

- Einem Benutzer die Berechtigung gewähren, Geräte für ein ganzes Gebäude oder nur für einen bestimmten Raum oder eine bestimmte Etage zu verwalten
- Einem Administrator globalen Zugriff auf alle Raumgraphknoten für einen gesamten Graphen oder nur für einen Abschnitt des Graphen gewähren
- Einem Supportspezialisten Lesezugriff auf den Graphen gewähren, wobei Zugriffsschlüssel ausgenommen sind
- Jedem Mitglied einer Domäne Lesezugriff auf alle Graphobjekte gewähren

## <a name="rbac-best-practices"></a>Best Practices für RBAC

[!INCLUDE [digital-twins-permissions](../../includes/digital-twins-rbac-best-practices.md)]

## <a name="roles"></a>Rollen

### <a name="role-definitions"></a>Rollendefinitionen

Eine Rollendefinition ist eine Sammlung von Berechtigungen und anderen Attributen, die sich zu einer Rolle zusammensetzen. Eine Rollendefinition enthält die zulässigen Vorgänge, wozu *ERSTELLEN*, *LESEN*, *AKTUALISIEREN* und *LÖSCHEN* gehören, die von jedem Objekt mit dieser Rolle ausgeführt werden können. In ihr ist auch angegeben, für welche Objekte die Berechtigungen gelten.

[!INCLUDE [digital-twins-roles](../../includes/digital-twins-roles.md)]

>[!NOTE]
> Um die vollständigen Definitionen für die vorherigen Rollen abzurufen, fragen Sie die System-/Rollen-API ab.
> Weitere Informationen finden Sie unter [Erstellen und Verwalten von Rollenzuweisungen](./security-create-manage-role-assignments.md#all).

### <a name="object-identifier-types"></a>Objektbezeichnertypen

[!INCLUDE [digital-twins-object-types](../../includes/digital-twins-object-id-types.md)]

>[!TIP]
> Weitere Informationen zum Gewähren von Berechtigungen für Ihren Dienstprinzipal finden Sie unter [Erstellen und Verwalten von Rollenzuweisungen](./security-create-manage-role-assignments.md#grant).

In den folgenden Referenzartikeln werden die nachfolgenden Themen beschrieben:

- [Abfragen der Objekt-ID eines Benutzers](https://docs.microsoft.com/powershell/module/azuread/get-azureaduser?view=azureadps-2.0)
- [Abrufen der Objekt-ID eines Dienstprinzipals](https://docs.microsoft.com/powershell/module/az.resources/get-azadserviceprincipal)
- [Abrufen der Objekt-ID eines Azure AD-Mandanten](../active-directory/develop/quickstart-create-new-tenant.md)

## <a name="role-assignments"></a>Rollenzuweisungen

Eine Azure Digital Twins-Rollenzuweisung verknüpft ein Objekt, z.B. einen Benutzer oder einen Azure AD-Mandant, mit einer Rolle und einem Raum. Berechtigungen werden für alle Objekte gewährt, die zu diesem Raum gehören. Der Raum umfasst den gesamten Raumgraphen darunter.

Angenommen, ein Benutzer hat eine Rollenzuweisung mit der Rolle `DeviceInstaller` für den Stammknoten eines Raumgraphen, der einem Gebäude entspricht. Der Benutzer kann dann Geräte für diesen Knoten und alle anderen untergeordneten Räume im Gebäude lesen und aktualisieren.

Um einem Empfänger Berechtigungen zuzuweisen, erstellen Sie eine Rollenzuweisung. Um die Berechtigung zu widerrufen, entfernen Sie die Rollenzuweisung.

>[!IMPORTANT]
> Weitere Informationen zur Rollenzuweisung finden Sie unter [Erstellen und Verwalten von Rollenzuweisungen](./security-create-manage-role-assignments.md).

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum Erstellen und Verwalten von Rollenzuweisungen in Azure Digital Twins finden Sie unter [Erstellen und Verwalten von Rollenzuweisungen](./security-create-manage-role-assignments.md).
