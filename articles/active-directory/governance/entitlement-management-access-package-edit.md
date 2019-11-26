---
title: Ausblenden oder Löschen eines Zugriffspakets in der Azure AD-Berechtigungsverwaltung – Azure Active Directory
description: Erfahren Sie, wie Sie ein Zugriffspaket in der Azure Active Directory-Berechtigungsverwaltung ausblenden oder löschen.
services: active-directory
documentationCenter: ''
author: msaburnley
manager: daveba
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 10/15/2019
ms.author: ajburnle
ms.reviewer: ''
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4391cd20f3919e8def28fa3c1d5007f91b2d9f0b
ms.sourcegitcommit: 98ce5583e376943aaa9773bf8efe0b324a55e58c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73174746"
---
# <a name="hide-or-delete-an-access-package-in-azure-ad-entitlement-management"></a>Ausblenden oder Löschen eines Zugriffspakets in der Azure AD-Berechtigungsverwaltung

Zugriffspakete sind standardmäßig sichtbar. Dies bedeutet, dass einem Benutzer ein Zugriffspaket im Portal „Mein Zugriff“ automatisch aufgeführt wird, wenn ihm das Anfordern des Zugriffspakets in einer Richtlinie erlaubt wird. Sie können jedoch die Einstellung **Ausgeblendet** ändern, damit das Zugriffspaket im Portal „Mein Zugriff“ des Benutzers nicht aufgeführt wird.

In diesem Artikel wird beschrieben, wie Sie ein Zugriffspaket ausblenden oder löschen können.

## <a name="change-the-hidden-setting"></a>Ändern der Einstellung „Ausgeblendet“

Führen Sie die folgenden Schritte aus, um die Einstellung **Ausgeblendet** für ein Zugriffspaket zu ändern.

**Erforderliche Rolle:** Globaler Administrator, Benutzeradministrator, Katalogbesitzer oder Zugriffspaket-Manager

1. Klicken Sie im Azure-Portal auf **Azure Active Directory** und dann auf **Identity Governance**.

1. Klicken Sie im Menü auf der linken Seite auf **Zugriffspakete**, und öffnen Sie das Zugriffspaket.

1. Klicken Sie auf der Seite „Übersicht“ auf **Bearbeiten**.

1. Legen Sie die Einstellung **Ausgeblendet** fest:

    Bei **Nein** wird das Zugriffspaket im Portal „Mein Zugriff“ des Benutzers aufgeführt.

    Bei **Ja** wird das Zugriffspaket im Portal „Mein Zugriff“ des Benutzers nicht aufgeführt. In diesem Fall kann der Benutzer das Zugriffspaket nur über den direkten Link auf das Zugriffspaket im **Portal „Mein Zugriff“** anzeigen. Weitere Informationen finden Sie unter [Teilen des Links zum Anfordern eines Zugriffspakets](entitlement-management-access-package-settings.md).

## <a name="delete-an-access-package"></a>Löschen eines Zugriffspakets

Es können nur Zugriffspakete ohne aktive Benutzerzuweisungen gelöscht werden. Führen Sie die folgenden Schritte aus, um ein Zugriffspaket zu löschen.

**Erforderliche Rolle:** Globaler Administrator, Benutzeradministrator, Katalogbesitzer oder Zugriffspaket-Manager

1. Klicken Sie im Azure-Portal auf **Azure Active Directory** und dann auf **Identity Governance**.

1. Klicken Sie im Menü auf der linken Seite auf **Zugriffspakete**, und öffnen Sie das Zugriffspaket.

1. Klicken Sie im Menü auf der linken Seite auf **Zuweisungen**, und entfernen Sie für alle Benutzer den Zugriff.

1. Klicken Sie im Menü auf der linken Seite auf **Übersicht** und dann auf **Löschen**.

1. Klicken Sie im angezeigten Meldungsfeld auf **Ja**.

## <a name="next-steps"></a>Nächste Schritte

- [Anzeigen, Hinzufügen und Entfernen von Zuweisungen für ein Zugriffspaket](entitlement-management-access-package-assignments.md)
- [Anzeigen von Berichten und Protokollen](entitlement-management-reports.md)
