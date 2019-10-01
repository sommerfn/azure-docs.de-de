---
title: include file
description: include file
services: active-directory
author: curtand
ms.service: active-directory
ms.topic: include
ms.date: 05/22/2019
ms.author: curtand
ms.custom: include file
ms.openlocfilehash: c58977798fabc120f3a8647774f575bc32515dcd
ms.sourcegitcommit: 3fa4384af35c64f6674f40e0d4128e1274083487
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2019
ms.locfileid: "71219031"
---
Nachstehend finden Sie die Verwendungs- und andere Diensteinschränkungen für den Azure Active Directory-Dienst (Azure AD).

| Category | Grenzen |
| --- | --- |
| Verzeichnisse | Ein einzelner Benutzer kann als Mitglied oder Gast bis zu 500 Azure AD-Verzeichnissen angehören.<br/>Ein einzelner Benutzer kann maximal 20 Verzeichnisse erstellen. |
| Domänen | Sie können nicht mehr als 900 verwaltete Domänennamen hinzufügen. Wenn Sie alle Ihre Domänen für den Verbund mit der lokalen Active Directory-Instanz einrichten möchten, können Sie in jedem Verzeichnis maximal 450 Domänennamen hinzufügen. |
|Ressourcen |<ul><li>Bei der Free Edition von Azure Active Directory können Benutzer standardmäßig in einem Verzeichnis maximal 50.000 Azure AD-Ressourcen erstellen. Wenn Sie mindestens eine verifizierte Domäne haben, wird das Standardkontingent für den Verzeichnisdienst in Azure AD auf 300.000 Azure AD-Ressourcen erweitert. </li><li>Ein Benutzer ohne Administratorrechte kann maximal 250 Azure AD-Ressourcen erstellen. Zu diesem Kontingent zählen sowohl aktive Ressourcen als auch gelöschte Ressourcen, die zum Wiederherstellen verfügbar sind. Nur gelöschte Azure AD-Ressourcen, die vor weniger als 30 Tagen gelöscht wurden, stehen für die Wiederherstellung bereit. Gelöschte Azure AD-Ressourcen, die nicht mehr für die Wiederherstellung verfügbar sind, zählen für 30 Tage mit einem Viertelwert zu diesem Kontingent. Wenn Sie Entwickler haben, die dieses Kontingent wahrscheinlich wiederholt im Rahmen ihrer regulären Aufgaben überschreiten, können Sie [eine benutzerdefinierte Rolle erstellen und zuweisen](../articles/active-directory/users-groups-roles/roles-quickstart-app-registration-limits.md), die die Berechtigung hat, eine unbegrenzte Anzahl von App-Registrierungen zu erstellen.</li></ul> |
| Schemaerweiterungen |<ul><li>Erweiterungen des Typs „String“ sind auf maximal 256 Zeichen begrenzt. </li><li>Erweiterungen des Typs „Binary“ sind auf 256 Byte beschränkt.</li><li>Es können maximal 100 Erweiterungswerte (für *alle* Typen und *alle* Anwendungen) in jede einzelne Azure AD-Ressource geschrieben werden.</li><li>Nur die Entitäten „User“, „Group“, „TenantDetail“, „Device“, „Application“ und „ServicePrincipal“ mit dem Typ „String“ oder „Binary“ können mit Einzelwertattributen erweitert werden.</li><li>Schemaerweiterungen sind nur in der Graph-API-Version „1.21-preview“ verfügbar. Der Anwendung muss Schreibzugriff zum Registrieren einer Erweiterung gewährt werden.</li></ul> |
| ANWENDUNGEN |Maximal 100 Benutzer können Besitzer einer einzelnen Anwendung sein. |
| Gruppen |<ul><li>Maximal 100 Benutzer können Besitzer einer einzelnen Gruppe sein.</li><li>Eine beliebige Anzahl von Azure AD-Ressourcen kann einer einzelnen Gruppe angehören.</li><li>Ein Benutzer kann ein Mitglied einer beliebigen Anzahl von Gruppen sein.</li><li>Die Anzahl von Mitgliedern einer Gruppe, die Sie über Ihre lokale Active Directory-Instanz mit Azure Active Directory synchronisieren können, ist bei Verwendung von Azure AD Connect auf 50.000 beschränkt.</li></ul> |
| Anwendungsproxy | <ul><li>Maximal 500 Transaktionen pro Sekunde pro App-Proxy-Anwendung</li><li>Maximal 750 Transaktionen pro Sekunde für den Mandanten</li></ul><br/>Eine Transaktion wird als einzelne HTTP-Anforderung und -Antwort für eine eindeutige Ressource definiert. Bei einer Drosselung erhalten Clients die Antwort 429 (zu viele Anforderungen). |
| Anpassung des Zugriffsbereichs |<ul><li>Es gibt keine Beschränkung für die Anzahl der Anwendungen, die im Zugriffsbereich pro Benutzer angezeigt werden können. Dies gilt für Benutzer mit zugewiesenen Lizenzen für Azure AD Premium oder die Enterprise Mobility Suite.</li><li>Maximal 10 App-Kacheln können für jeden Benutzer im Zugriffsbereich angezeigt werden. Dieser Grenzwert gilt für Benutzer, denen Lizenzen für den Azure AD Free-Lizenzplan zugewiesen sind. Beispiele für App-Kacheln sind Box, Salesforce oder Dropbox. Diese Beschränkung gilt nicht für Administratorkonten.</li></ul> |
| Berichte | In einem Bericht können maximal 1.000 Zeilen angezeigt oder heruntergeladen werden. Weitere Daten werden abgeschnitten. |
| Verwaltungseinheiten | Eine Azure AD-Ressource kann zu höchstens 30 Verwaltungseinheiten gehören. |
| Administratorrollen und -berechtigungen | <ul><li>Eine Gruppe kann nicht als [Besitzer](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context#object-ownership) hinzugefügt werden.</li><li>Eine Gruppe kann nicht zu einer [Rolle](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) zugewiesen werden.</li><li>Die Fähigkeit von Benutzern, Verzeichnisinformationen anderer Benutzer lesen zu können, kann nicht außerhalb des mandantenweiten Switches eingeschränkt werden, um den Zugriff für alle Nicht-Administratorbenutzer auf alle Verzeichnisinformationen zu deaktivieren (nicht empfohlen). Weitere Informationen zu den Standardberechtigungen finden Sie [hier](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context#to-restrict-the-default-permissions-for-member-users).</li><li>Es kann bis zu 15 Minuten für Abmelden/Anmelden dauern, bevor Ergänzungen und Widerrufe für eine Mitgliedschaft in einer Administratorrolle wirksam werden.</li></ul> |
