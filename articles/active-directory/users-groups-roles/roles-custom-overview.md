---
title: 'Vorschau: Azure-Administratorrollen mit anpassbaren Berechtigungen – Azure Active Directory | Microsoft-Dokumentation'
description: 'Vorschau: Benutzerdefinierte Azure AD-Rollen zur Delegierung der Identitätsverwaltung. Verwalten von Azure-Rollen im Azure-Portal, mit PowerShell oder über die Graph-API.'
services: active-directory
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 07/31/2019
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 82638e3e102f7b8e39cd797960a11f3193132bc1
ms.sourcegitcommit: 6cbf5cc35840a30a6b918cb3630af68f5a2beead
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/05/2019
ms.locfileid: "68779389"
---
# <a name="custom-administrator-roles-in-azure-active-directory-preview"></a>Benutzerdefinierte Administratorrollen in Azure Active Directory (Vorschau)

In diesem Artikel werden die Grundlagen der neuen benutzerdefinierten rollenbasierten Zugriffssteuerung (RBAC) und Ressourcenbereiche in Azure Active Directory (Azure AD) beschrieben. Durch benutzerdefinierte RBAC-Rollen werden die zugrunde liegenden Berechtigungen der [integrierten Rollen](directory-assign-admin-roles.md) angezeigt, sodass Sie eigene benutzerdefinierte Rollen erstellen und organisieren können. Ressourcenbereiche bieten Ihnen die Möglichkeit, die benutzerdefinierte Rolle zur Verwaltung einiger Ressourcen (z. B. einer Anwendung) zuzuweisen, ohne Zugriff auf alle Ressourcen (alle Anwendungen) zu gewähren.

Die Erteilung von Berechtigungen mithilfe benutzerdefinierter RBAC-Rollen erfolgt in zwei Schritten. Zunächst erstellen Sie eine benutzerdefinierte Rollendefinition und fügen ihr Berechtigungen aus der vordefinierten Liste hinzu. Dabei handelt es sich um die gleichen Berechtigungen, die in den integrierten Rollen verwendet werden. Nach dem Erstellen der Rolle weisen Sie sie durch Erstellen einer Rollenzuweisung einem Benutzer zu. Durch diesen zweistufigen Vorgang können Sie eine Rolle erstellen und dann mehrmals für verschiedene Bereiche zuweisen. Eine benutzerdefinierte Rolle kann im Verzeichnisbereich oder in einem Objektbereich zugewiesen werden. Ein Beispiel für einen Objektbereich ist eine einzelne Anwendung. Auf diese Weise kann die gleiche Rolle Sally für alle Anwendungen im Verzeichnis und dann Naveen nur für die App der Contoso-Spesenabrechnungen zugewiesen werden.

Dieses erste Release der benutzerdefinierten RBAC-Rollen umfasst die Funktion, eine Rolle zum Zuweisen von Berechtigungen zum Verwalten von Anwendungsregistrierungen zu erstellen. Im Laufe der Zeit werden weitere Berechtigungen für Organisationsressourcen wie Unternehmensanwendungen, Benutzer und Geräte hinzugefügt.

Previewfunktionen:

- Aktualisierungen der Portalbenutzeroberfläche zum Erstellen und Verwalten von benutzerdefinierten Rollen und zum Zuweisen dieser Rollen zu Benutzern im organisationsweiten Bereich
- PowerShell-Vorschaumodul mit neuen Cmdlets für Folgendes:
  - Erstellen und Verwalten von benutzerdefinierten Rollen
  - Zuweisen von benutzerdefinierten Rollen mit einem organisationsweiten oder einem anwendungsregistrierungsspezifischen Bereich
  - Zuweisen von integrierten Rollen im organisationsweiten Bereich (Gleichstellung mit allgemein verfügbaren Cmdlets)
  - Unterstützung der Azure AD Graph-API

Die rollenbasierte Zugriffssteuerung ist eine öffentliche Previewfunktion von Azure AD und mit jedem kostenpflichtigen Azure AD-Lizenzplan verfügbar. Weitere Informationen zu Vorschauversionen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="understand-azure-ad-role-based-access-control"></a>Grundlegendes zur rollenbasierten Zugriffssteuerung von Azure AD

Mit der rollenbasierten Zugriffssteuerung von Azure AD können Sie Rollen zuweisen, die so angepasst sind, dass zulässige Aktionen nur für einen Typ von Azure AD-Ressource möglich sind. Die rollenbasierte Zugriffssteuerung von Azure AD funktioniert auf ähnliche Weise wie die rollenbasierte Zugriffssteuerung in Azure ([Azure RBAC](../../role-based-access-control/overview.md)) für den Zugriff auf Azure-Ressourcen. Die rollenbasierte Zugriffssteuerung von Azure AD basiert jedoch auf Microsoft Graph, während Azure RBAC auf Azure Resource Manager basiert. Die Funktionen beider Systeme beruhen aber auf Rollenzuweisungen.

### <a name="role-assignments"></a>Rollenzuweisungen

Bei der Steuerung des Zugriffs mithilfe der rollenbasierten Zugriffssteuerung von Azure AD werden **Rollenzuweisungen** erstellt, über die Berechtigungen erzwungen werden. Eine Rollenzuweisung besteht aus drei Elementen:

- Sicherheitsprinzipal
- Rollendefinition
- Ressourcenumfang

Der Zugriff wird durch Erstellen einer Rollenzuweisung erteilt und durch Entfernen einer Rollenzuweisung widerrufen. Sie können über das Azure-Portal, über Azure AD PowerShell und über die Graph-API [Rollenzuweisungen erstellen](roles-create-custom.md). Sie können die [Zuweisungen für eine benutzerdefinierte Rolle](roles-view-assignments.md#view-the-assignments-of-a-role-with-single-application-scope-using-the-azure-ad-portal-preview) separat anzeigen.

Das folgende Diagramm zeigt ein Beispiel für eine Rollenzuweisung. In diesem Beispiel wurde Chris Green die Rolle [Anwendungsadministrator](directory-assign-admin-roles.md#application-administrator) im Bereich der SalesForce-Anwendung zugewiesen. Chris hat keinen Zugriff auf die Verwaltung einer anderen Anwendung, es sei denn, die entsprechenden Anwendungen sind Teil einer anderen Rollenzuweisung.

![Die Rollenzuweisung definiert, wie Berechtigungen erzwungen werden. Sie besteht aus drei Teilen.](./media/roles-custom-overview/rbac-overview.png)

### <a name="security-principal"></a>Sicherheitsprinzipal

Ein Sicherheitsprinzipal stellt den Benutzer oder Dienstprinzipal dar, dem Zugriff auf Azure AD-Ressourcen zugewiesen werden soll. Ein *Benutzer* ist eine Person, die über ein Benutzerprofil in Azure Active Directory verfügt. Ein *Dienstprinzipal* ist eine Sicherheitsidentität, die in Anwendungen oder Diensten für den Zugriff auf bestimmte Azure AD-Ressourcen verwendet wird.

Ein Sicherheitsprinzipal ähnelt einer Benutzeridentität darin, dass er einen Benutzernamen und ein Kennwort oder Zertifikat darstellt, jedoch für eine Anwendung oder einen Dienst und nicht für einen Benutzer.

### <a name="role"></a>Role

Eine Rollendefinition oder Rolle ist eine Sammlung von Berechtigungen. Eine Rollendefinition listet die Vorgänge auf, die für Azure AD-Ressourcen ausgeführt werden können, z. B. Lesen, Schreiben, Aktualisieren und Löschen. Azure AD umfasst zwei Arten von Rollen:

- Von Microsoft erstellte integrierte Rollen, die nicht geändert werden können. Die integrierte Rolle „Globaler Administrator“ verfügt über alle Berechtigungen für alle Azure AD-Ressourcen.
- Von Ihrer Organisation erstellte und verwaltete benutzerdefinierte Rollen

### <a name="scope"></a>`Scope`

Ein Bereich ist die Beschränkung zulässiger Aktionen auf eine bestimmte Azure AD-Ressource. Beim Zuweisen einer Rolle können Sie einen Bereich angeben, der die zulässigen Aktionen des Administrators auf eine spezifische Ressource beschränkt. Wenn Sie z. B. einem Entwickler eine benutzerdefinierte Rolle zuweisen möchten, jedoch nur zum Verwalten einer spezifischen Anwendungsregistrierung, können Sie die entsprechende Anwendungsregistrierung als Bereich in die Rollenzuweisung einfügen.

  > [!Note]
  > Benutzerdefinierte Rollen können im Verzeichnisbereich und im Ressourcenbereich zugewiesen werden. Noch können sie nicht im Bereich von Verwaltungseinheiten zugewiesen werden.
  > Integrierte Rollen können im Verzeichnisbereich und in einigen Fällen im Bereich von Verwaltungseinheiten zugewiesen werden. Sie können noch nicht im Objektbereich zugewiesen werden.

## <a name="required-license-plan"></a>Erforderlicher Lizenzplan

[!INCLUDE [License requirement for using custom roles in Azure AD](../../../includes/active-directory-p1-license.md)]

## <a name="next-steps"></a>Nächste Schritte

- Erstellen von benutzerdefinierten Rollenzuweisungen über das [Azure-Portal, Azure AD PowerShell und die Graph-API](roles-create-custom.md)
- [Anzeigen der Zuweisungen für eine benutzerdefinierte Rolle](roles-view-assignments.md#view-the-assignments-of-a-role-with-single-application-scope-using-the-azure-ad-portal-preview)
