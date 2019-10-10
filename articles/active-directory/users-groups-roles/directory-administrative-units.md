---
title: Verwalten von Verwaltungseinheiten (Vorschau) – Azure Active Directory | Microsoft-Dokumentation
description: Verwenden von Verwaltungseinheiten zum präziseren Delegieren von Berechtigungen in Azure Active Directory
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: article
ms.subservice: users-groups-roles
ms.workload: identity
ms.date: 08/01/2019
ms.author: curtand
ms.reviewer: elkuzmen
ms.custom: oldportal;it-pro;
ms.collection: M365-identity-device-management
ms.openlocfilehash: 58b61186a876af90c812ec7faf41fa9f5b14bf4e
ms.sourcegitcommit: e1b6a40a9c9341b33df384aa607ae359e4ab0f53
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71336924"
---
# <a name="administrative-units-management-in-azure-active-directory-preview"></a>Verwalten von Verwaltungseinheiten in Azure Active Directory (Vorschau)

In diesem Artikel werden Verwaltungseinheiten in Azure Active Directory (Azure AD) beschrieben. Eine Verwaltungseinheit ist eine Azure AD Ressource, bei der es sich um einen Container für andere Azure AD Ressourcen handeln kann. In dieser Vorschauversion können diese Ressourcen nur Benutzer sein. Beispielsweise kann ein einer Verwaltungseinheit zugeordneter Benutzerkontoadministrator nur in seiner Verwaltungseinheit Profilinformationen aktualisieren, Kennwörter zurücksetzen und Benutzern Lizenzen zuweisen.

Über Verwaltungseinheiten kann das Delegieren von administrativen Berechtigungen für Teilmengen von Benutzern und das Anwenden von Richtlinien auf eine Teilmenge der Benutzer erfolgen. Mithilfe von Verwaltungseinheiten können Sie Berechtigungen an regionale Administratoren delegieren oder Richtlinien auf granularer Ebene festlegen.

## <a name="deployment-scenario"></a>Bereitstellungsszenario

Verwaltungseinheiten können in Organisationen mit unabhängigen Abteilungen praktisch sein. Nehmen Sie als Beispiel eine große Universität, die aus vielen autonomen Fakultäten (Wirtschaftswissenschaften, Ingenieurwissenschaften usw.) besteht, die jeweils über eigene IT-Administratoren verfügen, die den Zugriff steuern, die Benutzer verwalten und Richtlinien für ihre Fakultät festlegen. Ein zentraler Administrator könnte für die Wirtschaftsfakultät eine Verwaltungseinheit erstellen und diese nur mit den Studenten und Mitarbeitern dieser Fakultät füllen. Dann kann der zentrale Administrator die IT-Mitarbeiter der Wirtschaftsfakultät einer bereichsbezogenen Rolle hinzufügen, die ihnen administrative Berechtigungen nur für die Azure AD-Benutzer in der Verwaltungseinheit „Wirtschaftsfakultät“ gewährt.

## <a name="license-requirements"></a>Lizenzanforderungen

Für die Verwendung von Verwaltungseinheiten ist eine Azure Active Directory Premium-Lizenz für jeden Administrator von Verwaltungseinheiten erforderlich. Weitere Informationen finden Sie unter [Erste Schritte mit Azure AD Premium](../fundamentals/active-directory-get-started-premium.md).

## <a name="managing-administrative-units"></a>Verwalten von Verwaltungseinheiten

In dieser Vorschauversion können Sie Verwaltungseinheiten nur mit dem Azure Active Directory-Modul für Windows PowerShell-Cmdlets erstellen und verwalten, wie unter [Verwenden von Verwaltungseinheiten](https://docs.microsoft.com/powershell/azure/active-directory/working-with-administrative-units?view=azureadps-2.0) beschrieben.

Weitere Informationen zu Softwareanforderungen und zur Installation des Azure AD-Moduls finden Sie unter [Verwenden von Verwaltungseinheiten](https://docs.microsoft.com/powershell/azure/active-directory/overview?view=azureadps-2.0). Hier finden Sie auch Referenzinformationen zu den Cmdlets des Azure AD-Moduls zum Verwalten von Verwaltungseinheiten, einschließlich Syntax, Parameterbeschreibung und Beispielen.

## <a name="next-steps"></a>Nächste Schritte

[Azure Active Directory-Editionen](../fundamentals/active-directory-whatis.md)
