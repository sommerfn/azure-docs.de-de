---
title: Übersicht über Benutzer, Gruppen, Lizenzierung und Rollen – Azure AD | Microsoft-Dokumentation
description: Enthält eine Beschreibung der Beziehung zwischen zugewiesenen Benutzern und Lizenzen, Administratorrollen und Gruppenmitgliedschaft in Azure Active Directory.
keywords: ''
author: curtand
manager: daveba
ms.author: curtand
ms.reviewer: vincesm
ms.date: 11/08/2019
ms.topic: overview
ms.service: active-directory
ms.subservice: users-groups-roles
ms.workload: identity
services: active-directory
ms.custom: it-pro;seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3a647f4a5f051ec89bb82b6e7f89dcb677c95371
ms.sourcegitcommit: 49cf9786d3134517727ff1e656c4d8531bbbd332
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2019
ms.locfileid: "74027736"
---
# <a name="users-groups-licensing-and-roles-for-large-organizations"></a>Benutzer, Gruppen, Lizenzierung und Rollen für große Organisationen

Dieser Artikel enthält für Azure AD-Administratoren eine Einführung in die Beziehung zwischen den wichtigsten Aufgaben der [Identitätsverwaltung](/azure/active-directory/fundamentals/identity-fundamentals?context=azure/active-directory/users-groups-roles/context/ugr-context) für Benutzer in Bezug auf deren Gruppen, Lizenzen, bereitgestellte Unternehmens-Apps und Administratorrollen. Wenn Ihre Organisation wächst, können Sie Azure AD-Gruppen und -Administratorrollen für folgende Zwecke nutzen:

* Zuweisen von Lizenzen zu Gruppen anstelle von einzelnen Benutzern
* Delegieren von Berechtigungen zum Verteilen der Azure AD-Verwaltungsarbeit auf Rollen mit weniger Rechten
* Zuweisen des Zugriffs auf Unternehmens-Apps zu Gruppen

## <a name="assign-users-to-groups"></a>Zuweisen von Benutzern zu Gruppen

Sie können Gruppen in Azure AD verwenden, um Lizenzen einer großen Zahl von Benutzern zuzuweisen oder den Benutzerzugriff auf bereitgestellte Unternehmens-Apps zuzuweisen. Sie können Gruppen verwenden, um Administratorrollen in Azure zuzuweisen, oder Sie können den Zugriff auf externe Ressourcen gewähren, z.B. SaaS-Anwendungen oder SharePoint-Websites.

Wenn Sie für zusätzliche Flexibilität sorgen und den Aufwand für die Verwaltung der Gruppenmitgliedschaft reduzieren möchten, können Sie in Azure AD [dynamische Gruppen](groups-create-rule.md) nutzen, um die Gruppenmitgliedschaft automatisch zu verlängern und zu begrenzen. Sie benötigen für jeden eindeutigen Benutzer, der Mitglied mindestens einer dynamischen Gruppe ist, eine Azure AD Premium P1-Lizenz.

## <a name="assign-licenses-to-groups"></a>Zuweisen von Lizenzen zu Gruppen

Das einzelne Zuweisen und Entfernen von Lizenzen für Benutzer kann relativ viel Zeit und Aufwand erfordern. Wenn Sie stattdessen [Gruppen Lizenzen zuweisen](/azure/active-directory/fundamentals/license-users-groups?context=azure/active-directory/users-groups-roles/context/ugr-context), können Sie Ihre umfangreiche Lizenzverwaltung vereinfachen.

Wenn Benutzer in Azure AD einer lizenzierten Gruppe beitreten, werden ihnen automatisch die richtigen Lizenzen zugewiesen. Wenn Benutzer die Gruppe verlassen, entfernt Azure AD ihre Lizenzzuweisungen. Ohne Azure AD-Gruppen müssen Sie ein PowerShell-Skript schreiben oder eine Graph-API verwenden, um Lizenzen für Benutzer, die der Organisation beitreten oder diese verlassen, per Massenvorgang hinzuzufügen oder zu entfernen.

Wenn nicht genügend verfügbare Lizenzen vorhanden sind oder ein Problem auftritt (z.B. Dienstpläne, die nicht gleichzeitig zugewiesen werden können), können Sie den Status der Lizenzprobleme für die Gruppe im Azure-Portal anzeigen.

>[!NOTE]
>Das Feature für die gruppenbasierte Lizenzierung befindet sich derzeit in der öffentlichen Vorschauphase. Während der Vorschauphase ist die Funktion in jedem Azure Active Directory-Lizenzplan (Azure AD) bzw. jeder Testversion verfügbar.

## <a name="delegate-administrator-roles"></a>Delegieren von Administratorrollen

Viele große Organisationen wünschen sich Optionen für ihre Benutzer, mit denen ausreichende Berechtigungen für ihre Arbeitsaufgaben gewährt werden können, ohne dass die umfassende Rolle „Globaler Administrator“ beispielsweise Benutzern zugewiesen werden muss, die Anwendungen registrieren müssen. Hier ist ein Beispiel für neue Azure AD-Administratorrollen angegeben, mit denen Sie die Aufgaben der Anwendungsverwaltung präziser verteilen können:

 Rollenname | Zusammenfassung der Berechtigungen
 --------- | -------------------
 **Anwendungsadministrator** | Kann Unternehmensanwendungen und Anwendungsregistrierungen hinzufügen und verwalten und Anwendungsproxyeinstellungen konfigurieren. Anwendungsadministratoren können Richtlinien und Geräte für bedingten Zugriff anzeigen, aber nicht verwalten.
 **Cloudanwendungsadministrator** | Kann Unternehmensanwendungen und Registrierungen für Unternehmens-Apps hinzufügen und verwalten. Diese Rolle verfügt über alle Berechtigungen des Anwendungsadministrators, mit Ausnahme der Verwaltung von Anwendungsproxyeinstellungen.
**Anwendungsentwickler** | Kann Anwendungsregistrierungen hinzufügen und aktualisieren, aber keine Unternehmensanwendungen verwalten oder einen Anwendungsproxy konfigurieren.

Neue Azure AD-Administratorrollen werden derzeit hinzugefügt. Im Azure-Portal oder in der [Referenz zu den Berechtigungen der Administratorrolle ](directory-assign-admin-roles.md) können Sie sich die derzeit verfügbaren Rollen ansehen.

## <a name="assign-app-access"></a>Zuweisen des App-Zugriffs

Sie können Azure AD nutzen, um Gruppenzugriff auf die [Unternehmens-Apps zuzuweisen, die in Ihrem Azure AD-Mandanten bereitgestellt werden](/azure/active-directory/manage-apps/methods-for-assigning-users-and-groups?context=azure/active-directory/users-groups-roles/context/ugr-context). Wenn Sie dynamische Gruppen mit der Gruppenzuweisung zu Apps kombinieren, können Sie Ihre Zuweisungen des Benutzerzugriffs auf Apps automatisieren, wenn Ihre Organisation wächst. Sie benötigen eine Lizenz vom Typ Azure Active Directory Premium P1 oder Premium P2, um den Zugriff auf Unternehmens-Apps zuweisen zu können.

Mit Azure AD können Sie auch die Daten präzise steuern, die zwischen der App und den Gruppen ausgetauscht werden, denen Sie die Zugriffsberechtigung zuweisen. Öffnen Sie in [Unternehmensanwendungen](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/AllApps) eine App, und wählen Sie **Bereitstellung**, um Folgendes durchzuführen:

* Einrichten der automatischen Bereitstellung für Apps, die dies unterstützen
* Bereitstellen von Anmeldeinformationen zum Herstellen einer Verbindung mit der Benutzerverwaltungs-API der App
* Einrichten der Zuordnungen, mit denen gesteuert wird, welche Benutzerattribute zwischen Azure AD und der App fließen, wenn Benutzerkonten bereitgestellt oder aktualisiert werden
* Starten und Beenden des Azure AD-Bereitstellungsdiensts für eine App, Löschen des Bereitstellungscaches oder Neustarten des Diensts
* Anzeigen des **Bereitstellungsaktivitätsberichts** mit einem Protokoll aller Benutzer und Gruppen, die zwischen Azure AD und der App erstellt, aktualisiert und entfernt wurden, und des **Fehlerberichts zur Bereitstellung** mit ausführlicheren Fehlermeldungen

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie noch nicht lange als Azure AD-Administrator arbeiten, hilft Ihnen die [Dokumentation mit grundlegenden Informationen zu Azure Active Directory](https://docs.microsoft.com/azure/active-directory/fundamentals/index) weiter.

Sie können auch mit dem [Erstellen von Gruppen](/azure/active-directory/fundamentals/active-directory-groups-create-azure-portal?context=azure/active-directory/users-groups-roles/context/ugr-context), [Zuweisen von Lizenzen](/azure/active-directory/fundamentals/license-users-groups?context=azure/active-directory/users-groups-roles/context/ugr-context), [Zuweisen des App-Zugriffs](/azure/active-directory/manage-apps/methods-for-assigning-users-and-groups?context=azure/active-directory/users-groups-roles/context/ugr-context) oder [Zuweisen von Administratorrollen](directory-assign-admin-roles.md) beginnen.
