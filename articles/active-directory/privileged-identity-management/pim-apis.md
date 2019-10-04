---
title: Microsoft Graph-APIs für PIM (Vorschauversion) – Azure Active Directory | Microsoft-Dokumentation
description: Hier erhalten Sie Informationen zur Verwendung der Microsoft Graph-APIs für Azure AD Privileged Identity Management (PIM) (Vorschauversion).
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.subservice: pim
ms.topic: overview
ms.date: 11/13/2018
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 45c7f42d536880f2578c62c6c4866b21be1cc9dc
ms.sourcegitcommit: 95b180c92673507ccaa06f5d4afe9568b38a92fb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/08/2019
ms.locfileid: "70804562"
---
# <a name="microsoft-graph-apis-for-pim-preview"></a>Microsoft Graph-APIs für PIM (Vorschauversion)

Die meisten Aufgaben, die Sie in Azure Active Directory (Azure AD) Privileged Identity Management (PIM) über das Azure-Portal ausführen können, können Sie auch mit den [Microsoft Graph-APIs](https://developer.microsoft.com/graph/docs/concepts/overview) ausführen. In diesem Artikel werden einige wichtige Konzepte für die Verwendung der Microsoft Graph-APIs für PIM beschrieben. Weitere Informationen zu den Microsoft Graph-APIs finden Sie in der [Referenz zur Azure AD Privileged Identity Management-API](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/privilegedidentitymanagement_root).

> [!IMPORTANT]
> APIs in der Betaversion in Microsoft Graph befinden sich in der Vorschauphase und ändern sich möglicherweise noch. Die Verwendung dieser APIs in Produktionsanwendungen wird nicht unterstützt.

## <a name="required-permissions"></a>Erforderliche Berechtigungen

Zum Aufrufen der Microsoft Graph-APIs für PIM benötigen Sie **mindestens eine** der folgenden Berechtigungen:

- `Directory.AccessAsUser.All`
- `Directory.Read.All`
- `Directory.ReadWrite.All`
- `PrivilegedAccess.ReadWrite.AzureAD`

### <a name="set-permissions"></a>Festlegen von Berechtigungen

Damit Anwendungen die Microsoft Graph-APIs für PIM aufrufen können, benötigen sie die erforderlichen Berechtigungen. Die erforderlichen Berechtigungen werden am einfachsten über das [Azure AD-Zustimmungsframework](../develop/consent-framework.md) angegeben.

### <a name="set-permissions-in-graph-explorer"></a>Festlegen von Berechtigungen in Graph-Tester

Beim Testen Ihrer Aufrufe mit Graph-Tester können Sie die Berechtigungen im Tool angeben.

1. Melden Sie sich bei [Graph-Tester](https://developer.microsoft.com/graph/graph-explorer) als globaler Administrator an.

1. Klicken Sie auf **Berechtigungen ändern**.

    ![Graph-Tester: Berechtigungen ändern](./media/pim-apis/graph-explorer.png)

1. Markieren Sie die gewünschten Berechtigungen. `PrivilegedAccess.ReadWrite.AzureAD` ist in Graph-Tester noch nicht verfügbar.

    ![Graph-Tester: Berechtigungen ändern](./media/pim-apis/graph-explorer-modify-permissions.png)

1. Klicken Sie auf **Berechtigungen ändern**, um die Änderungen der Berechtigungen zu übernehmen.

## <a name="next-steps"></a>Nächste Schritte

- [Referenz zur Azure AD Privileged Identity Management-API](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/privilegedidentitymanagement_root)
