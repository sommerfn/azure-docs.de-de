---
title: Delegieren von Aufgaben in der Azure AD-Berechtigungsverwaltung (Vorschauversion) – Azure Active Directory
description: Erfahren Sie mehr über die Rollen, die Sie zum Delegieren von Aufgaben in der Azure Active Directory-Berechtigungsverwaltung zuweisen können.
services: active-directory
documentationCenter: ''
author: msaburnley
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 07/10/2019
ms.author: ajburnle
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 064724b3c6a5faa485850ecdfa3d3759d3631be0
ms.sourcegitcommit: 07700392dd52071f31f0571ec847925e467d6795
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2019
ms.locfileid: "70124905"
---
# <a name="delegate-tasks-in-azure-ad-entitlement-management-preview"></a>Delegieren von Aufgaben in der Azure AD-Berechtigungsverwaltung (Vorschauversion)

> [!IMPORTANT]
> Die Berechtigungsverwaltung von Azure Active Directory (Azure AD) befindet sich derzeit in der öffentlichen Vorschau.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Standardmäßig können globale Administratoren und Benutzeradministratoren alle Aspekte der Azure AD-Berechtigungsverwaltung erstellen und verwalten. Die Benutzer in diesen Rollen kennen jedoch möglicherweise nicht alle Szenarien, in denen Zugriffspakete erforderlich sind. In der Regel wissen Benutzer in Abteilungen, wer zusammenarbeiten muss. 

Anstatt Nicht-Administratoren uneingeschränkte Berechtigungen zu gewähren, können Sie Benutzern die Mindestberechtigungen erteilen, die sie für die Ausführung ihrer Arbeit benötigen, und vermeiden, widersprüchliche oder unangemessene Zugriffsrechte zu erstellen. Dieser Artikel beschreibt die Rollen, die Sie zuweisen können, um verschiedene Aufgaben in der Berechtigungsverwaltung zu delegieren. 

## <a name="delegate-example-for-departmental-adoption"></a>Delegierungsbeispiel für die Einführung in Abteilungen

Anhand eines Beispiels erfahren Sie, wie Sie Aufgaben in der Berechtigungsverwaltung delegieren könnten. 

Nehmen wir an, dass zu Ihrer Organisation die folgenden fünf Benutzer gehören:

| Benutzer | Department | Notizen |
| --- | --- | --- |
| Alina | IT | Globaler Administrator |
| Bernd | Research | Bernd ist außerdem Besitzer der Gruppe Research. |
| Carola | Research |  |
| David | Marketing |  |
| Elisa | Marketing | Elisa ist auch Besitzerin einer Marketing-Anwendung. |

Die Abteilungen Research und Marketing möchten die Berechtigungsverwaltung für ihre Benutzer verwenden. Alina ist noch nicht dazu bereit, dass andere Abteilungen die Berechtigungsverwaltung nutzen können. Hier ist eine Möglichkeit, wie Alina Aufgaben an die Abteilungen Marketing und Research delegieren könnte.

1. Alina erstellt eine neue Azure AD-Sicherheitsgruppe für Katalogersteller und fügt Bernd, Carola, David und Elisa dieser Gruppe als Mitglieder hinzu.

1. Alina verwendet die Berechtigungsverwaltungseinstellungen, um diese Gruppe der Katalogerstellerrolle hinzuzufügen.

1. Carola erstellt einen **Research**-Katalog und fügt Bernd als Mitbesitzer des betreffenden Katalogs hinzu. Bernd fügt die Research-Gruppe, die er besitzt, als Ressource dem Katalog hinzu, sodass sie in einem Zugriffspaket für die Research-Zusammenarbeit verwendet werden kann.

1. David erstellt einen **Marketing**-Katalog und fügt Elisa als Mitbesitzerin des betreffenden Katalogs hinzu. Elisa fügt die Marketing-Anwendung, die sie besitzt, als Ressource dem Katalog hinzu, sodass sie in einem Zugriffspaket für die Marketing-Zusammenarbeit verwendet werden kann.

Jetzt können die Abteilungen Research und Marketing die Berechtigungsverwaltung nutzen. Bernd, Carola, David und Elisa können Zugriffspakete in ihren jeweiligen Katalogen erstellen und verwalten.

![Delegierungsbeispiel für die Berechtigungsverwaltung](./media/entitlement-management-delegate/elm-delegate.png)


## <a name="entitlement-management-roles"></a>Berechtigungsverwaltungsrollen

Die Berechtigungsverwaltung hat die folgenden Rollen, die für die Berechtigungsverwaltung spezifisch sind.

| Role | BESCHREIBUNG |
| --- | --- |
| Katalogersteller | Erstellen und Verwalten von Katalogen. In der Regel ein IT-Administrator, der kein globaler Administrator ist, oder ein Ressourcenbesitzer für eine Sammlung von Ressourcen. Die Person, die einen Katalog erstellt, ist automatisch der erste Besitzer des Katalogs und kann zusätzliche Katalogbesitzer hinzufügen. |
| Katalogbesitzer | Bearbeiten und Verwalten von vorhandenen Katalogen. In der Regel ein IT-Administrator oder Ressourcenbesitzer bzw. ein Benutzer, den der Katalogbesitzer bestimmt hat. |
| Zugriffspaketmanager | Bearbeiten und Verwalten aller in einem Katalog vorhandenen Zugriffspakete. |

Darüber hinaus haben auch eine designierte genehmigende Person und ein Antragsteller eines Zugriffspakets Rechte, obwohl es sich nicht um Rollen handelt.
 
* Genehmigende Person: Autorisiert durch eine Richtlinie zur Genehmigung oder Ablehnung von Anforderungen zum Zugriff auf Pakete, obwohl sie die Zugriffspaketdefinitionen nicht ändern können.
* Anforderer: Autorisiert durch eine Richtlinie eines Zugriffspakets zum Anfordern dieses Zugriffspakets.

Die folgende Tabelle enthält die Aufgaben, die diese Rollen ausführen können.

| Aufgabe | Katalogersteller | Katalogbesitzer | Zugriffspaketmanager | Genehmigende Person |
| --- | :---: | :---: | :---: | :---: |
| [Erstellen eines neuen Katalogs](entitlement-management-catalog-create.md) | :heavy_check_mark: |  |  |  |
| [Hinzufügen einer Ressource zu einem Katalog](entitlement-management-catalog-create.md#add-resources-to-a-catalog) | | :heavy_check_mark: | | |
| [Bearbeiten eines Katalogs](entitlement-management-catalog-create.md#edit-a-catalog) |  | :heavy_check_mark: |  |  |
| [Löschen eines Katalogs](entitlement-management-catalog-create.md#delete-a-catalog) |  | :heavy_check_mark: |  |  |
| [Hinzufügen eines Katalogbesitzers oder Zugriffspaket-Managers zu einem Katalog](#add-a-catalog-owner-or-an-access-package-manager) |  | :heavy_check_mark: |  |  |
| [Erstellen eines neuen Zugriffspakets in einem Katalog](entitlement-management-access-package-create.md) |  | :heavy_check_mark:  | :heavy_check_mark:  |  |
| [Verwalten von Ressourcenrollen in einem Zugriffspaket](entitlement-management-access-package-edit.md) |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Erstellen und Bearbeiten von Richtlinien](entitlement-management-access-package-edit.md#add-a-new-policy) |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Direktes Zuweisen eines Benutzers zu einem Zugriffspaket](entitlement-management-access-package-edit.md#directly-assign-a-user) |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Anzeigen, wem ein Zugriffspaket zugewiesen ist](entitlement-management-access-package-edit.md#view-who-has-an-assignment) |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Anzeigen der Anforderungen eines Zugriffspakets](entitlement-management-access-package-edit.md#view-requests) |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Anzeigen der Fehler bei der Übermittlung einer Anforderung](entitlement-management-access-package-edit.md#view-a-requests-delivery-errors) |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Abbrechen einer ausstehenden Anforderung](entitlement-management-access-package-edit.md#cancel-a-pending-request) |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Ausblenden eines Zugriffspakets](entitlement-management-access-package-edit.md#change-the-hidden-setting) |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Löschen eines Zugriffspakets](entitlement-management-access-package-edit.md#delete) |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Genehmigen einer Zugriffsanforderung](entitlement-management-request-approve.md) |  |  |  | :heavy_check_mark: |

## <a name="required-roles-to-add-resources-to-a-catalog"></a>Erforderliche Rollen, um einem Katalog Ressourcen hinzuzufügen

Ein globaler Administrator kann jede Gruppe (in der Cloud erstellte Sicherheitsgruppen oder Office 365-Gruppen), Anwendung oder SharePoint Online-Website einem Katalog hinzufügen oder daraus entfernen. Ein Benutzeradministrator kann jede Gruppe oder Anwendung einem Katalog hinzufügen oder daraus entfernen.

Damit ein Benutzer, der kein globaler Administrator oder Benutzeradministrator ist, Gruppen, Anwendungen oder SharePoint-Online-Websites einem Katalog hinzufügen kann, muss dieser Benutzer über die erforderliche Azure AD-Verzeichnisrolle *und* die Berechtigungsverwaltungsrolle für Katalogbesitzer verfügen. Die folgende Tabelle enthält die Rollenkombinationen, die erforderlich sind, um einem Katalog Ressourcen hinzuzufügen. Um Ressourcen aus einem Katalog zu entfernen, müssen Sie über die gleichen Rollen verfügen.

| Azure AD-Verzeichnisrolle | Berechtigungsverwaltungsrolle | Kann Sicherheitsgruppe hinzufügen | Kann Office 365-Gruppe hinzufügen | Kann App hinzufügen | Kann SharePoint Online-Website hinzufügen |
| --- | :---: | :---: | :---: | :---: | :---: |
| [Globaler Administrator](../users-groups-roles/directory-assign-admin-roles.md) | – |  :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Benutzeradministrator](../users-groups-roles/directory-assign-admin-roles.md) | – |  :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Intune-Administrator](../users-groups-roles/directory-assign-admin-roles.md) | Katalogbesitzer | :heavy_check_mark: | :heavy_check_mark: |  |  |
| [Exchange-Administrator](../users-groups-roles/directory-assign-admin-roles.md) | Katalogbesitzer |  | :heavy_check_mark: |  |  |
| [Teams-Dienstadministrator](../users-groups-roles/directory-assign-admin-roles.md) | Katalogbesitzer |  | :heavy_check_mark: |  |  |
| [SharePoint-Administrator](../users-groups-roles/directory-assign-admin-roles.md) | Katalogbesitzer |  | :heavy_check_mark: |  | :heavy_check_mark: |
| [Anwendungsadministrator](../users-groups-roles/directory-assign-admin-roles.md) | Katalogbesitzer |  |  | :heavy_check_mark: |  |
| [Cloudanwendungsadministrator](../users-groups-roles/directory-assign-admin-roles.md) | Katalogbesitzer |  |  | :heavy_check_mark: |  |
| Benutzer | Katalogbesitzer | Nur, wenn Gruppenbesitzer | Nur, wenn Gruppenbesitzer | Nur, wenn App-Besitzer |  |


## <a name="add-a-catalog-creator"></a>Hinzufügen eines Katalogerstellers

Wenn Sie das Erstellen des Katalogs delegieren möchten, fügen Sie Benutzer der Katalogerstellerrolle hinzu.  Sie können einzelne Benutzer oder der Einfachheit halber eine Gruppe hinzufügen, deren Mitglieder dann Kataloge erstellen können. Führen Sie diese Schritte aus, um einen Benutzer der Katalogerstellerrolle zuzuordnen.

**Erforderliche Rolle:** Globaler Administrator oder Benutzeradministrator

1. Klicken Sie im Azure-Portal auf **Azure Active Directory** und dann auf **Identity Governance**.

1. Klicken Sie im linken Menü im Abschnitt **Berechtigungsverwaltung** auf **Einstellungen**.

1. Klicken Sie auf **Edit**.

1. Klicken Sie im Abschnitt **Berechtigungsverwaltung delegieren** auf **Katalogersteller hinzufügen**, um die Benutzer oder Gruppen auszuwählen, die die Mitglieder für diese Berechtigungsverwaltungsrolle sein werden.

1. Klicken Sie auf **Auswählen**.

1. Klicken Sie auf **Speichern**.

## <a name="add-a-catalog-owner-or-an-access-package-manager"></a>Hinzufügen eines Katalogbesitzers oder Zugriffspaket-Managers

Wenn Sie die Verwaltung eines Katalogs oder von Zugriffspaketen im Katalog delegieren möchten, fügen Sie der Katalogbesitzer- oder Zugriffspaket-Manager-Rolle Benutzer hinzu. Der Benutzer, der einen Katalog erstellt, ist der erste Katalogbesitzer. Um einen Benutzer der Katalogbesitzer- oder Zugriffspaket-Manager-Rolle zuzuweisen, gehen Sie wie folgt vor.

**Erforderliche Rolle:** Globaler Administrator, Benutzeradministrator oder Katalogbesitzer

1. Klicken Sie im Azure-Portal auf **Azure Active Directory** und dann auf **Identity Governance**.

1. Klicken Sie im linken Menü auf **Kataloge**, und öffnen Sie dann den Katalog, dem Sie Administratoren hinzufügen möchten.

1. Klicken Sie im linken Menü auf **Rollen und Administratoren**.

1. Klicken Sie auf **Besitzer hinzufügen** oder **Zugriffspaketmanager hinzufügen**, um die Mitglieder für diese Rollen auszuwählen.

1. Klicken Sie auf **Auswählen**, um diese Mitglieder hinzuzufügen.

## <a name="next-steps"></a>Nächste Schritte

- [Hinzufügen genehmigender Personen](entitlement-management-access-package-edit.md#policy-request)
- [Hinzufügen von Ressourcen zu einem Katalog](entitlement-management-catalog-create.md#add-resources-to-a-catalog)
