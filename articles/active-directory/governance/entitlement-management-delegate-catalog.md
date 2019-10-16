---
title: Delegieren der Zugriffssteuerung an Katalogersteller in der Azure AD-Berechtigungsverwaltung (Vorschauversion) – Azure Active Directory
description: Erfahren Sie, wie Sie die Zugriffssteuerung von IT-Administratoren an Katalogersteller und Projektmanager delegieren, damit sie den Zugriff selbst verwalten können.
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
ms.date: 10/07/2019
ms.author: ajburnle
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: da6183599f2dacd1a98d8fe359edda741b903b50
ms.sourcegitcommit: aef6040b1321881a7eb21348b4fd5cd6a5a1e8d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2019
ms.locfileid: "72170880"
---
# <a name="delegate-access-governance-to-catalog-creators-in-azure-ad-entitlement-management-preview"></a>Delegieren der Zugriffssteuerung an Katalogersteller in der Azure AD-Berechtigungsverwaltung (Vorschauversion)

> [!IMPORTANT]
> Die Berechtigungsverwaltung von Azure Active Directory (Azure AD) befindet sich derzeit in der öffentlichen Vorschau.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Wenn Sie an Benutzer delegieren möchten, die keine Administratoren sind, damit sie ihre eigenen Kataloge erstellen können, fügen Sie diese Benutzer der in der Azure AD-Berechtigungsverwaltung definierten Rolle Katalogersteller hinzu. Sie können einzelne Benutzer oder eine ganze Gruppe hinzufügen, deren Mitglieder dann Kataloge erstellen können.

## <a name="as-an-it-administrator-delegate-to-a-catalog-creator"></a>Delegieren an einen Katalogersteller als IT-Administrator

Führen Sie diese Schritte aus, um einen Benutzer der Katalogerstellerrolle zuzuordnen.

**Erforderliche Rolle:** Globaler Administrator oder Benutzeradministrator

1. Klicken Sie im Azure-Portal auf **Azure Active Directory** und dann auf **Identity Governance**.

1. Klicken Sie im linken Menü im Abschnitt **Berechtigungsverwaltung** auf **Einstellungen**.

1. Klicken Sie auf **Edit**.

    ![Einstellungen zum Hinzufügen von Katalogerstellern](./media/entitlement-management-delegate/settings-delegate.png)

1. Klicken Sie im Abschnitt **Berechtigungsverwaltung delegieren** auf **Katalogersteller hinzufügen**, um die Benutzer oder Gruppen auszuwählen, an die Sie die Berechtigungsverwaltungsrolle delegieren möchten.

1. Klicken Sie auf **Auswählen**.

1. Klicken Sie auf **Speichern**.

## <a name="next-steps"></a>Nächste Schritte

- [Erstellen und Verwalten eines Katalogs von Ressourcen](entitlement-management-catalog-create.md)
- [Delegieren der Zugriffssteuerung an Zugriffspaket-Manager](entitlement-management-delegate-managers.md)

