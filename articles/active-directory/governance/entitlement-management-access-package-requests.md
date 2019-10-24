---
title: Anzeigen und Verwalten von Anforderungen für ein Zugriffspaket in der Azure AD-Berechtigungsverwaltung (Vorschauversion) – Azure Active Directory
description: Hier erfahren Sie, wie Sie Anforderungen für ein Zugriffspaket in der Azure Active Directory-Berechtigungsverwaltung (Vorschauversion) anzeigen, erneut verarbeiten und abbrechen.
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
ms.openlocfilehash: f7211fea4781904b9d97428ecf00b57970e4b57d
ms.sourcegitcommit: 77bfc067c8cdc856f0ee4bfde9f84437c73a6141
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2019
ms.locfileid: "72430280"
---
# <a name="view-and-manage-requests-for-an-access-package-in-azure-ad-entitlement-management-preview"></a>Anzeigen und Verwalten von Anforderungen für ein Zugriffspaket in der Azure AD-Berechtigungsverwaltung (Vorschauversion)

> [!IMPORTANT]
> Die Berechtigungsverwaltung von Azure Active Directory (Azure AD) befindet sich derzeit in der öffentlichen Vorschau.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

In der Azure AD-Berechtigungsverwaltung können Sie anzeigen, wer Zugriffspakete angefordert hat. Außerdem können Sie die jeweilige Richtlinie und den Status anzeigen. In diesem Artikel erfahren Sie, wie Sie Anforderungen für ein Zugriffspaket anzeigen, erneut verarbeiten und abbrechen.

## <a name="view-requests"></a>Anzeigen von Anforderungen

**Erforderliche Rolle:** Globaler Administrator, Benutzeradministrator, Katalogbesitzer oder Zugriffspaket-Manager

1. Klicken Sie im Azure-Portal auf **Azure Active Directory** und dann auf **Identity Governance**.

1. Klicken Sie im Menü auf der linken Seite auf **Zugriffspakete**, und öffnen Sie das Zugriffspaket.

1. Klicken Sie auf **Anforderungen**.

1. Klicken Sie auf eine bestimmte Anforderung, wenn Sie zusätzliche Details anzeigen möchten.

    ![Anforderungsliste für ein Zugriffspaket](./media/entitlement-management-access-package-requests/requests-list.png)

## <a name="view-a-requests-delivery-errors"></a>Anzeigen der Fehler bei der Übermittlung einer Anforderung

**Erforderliche Rolle:** Globaler Administrator, Benutzeradministrator, Katalogbesitzer oder Zugriffspaket-Manager

1. Klicken Sie im Azure-Portal auf **Azure Active Directory** und dann auf **Identity Governance**.

1. Klicken Sie im Menü auf der linken Seite auf **Zugriffspakete**, und öffnen Sie das Zugriffspaket.

1. Klicken Sie auf **Anforderungen**.

1. Wählen Sie die Anforderung aus, die angezeigt werden soll.

    Sind bei der Übermittlung der Anforderung Fehler aufgetreten, lautet der Status der Anforderung **Nicht übermittelt** oder **Teilweise übermittelt**.

    Sind Übermittlungsfehler aufgetreten, wird deren Anzahl im Detailbereich der Anforderung angezeigt.

1. Klicken Sie auf die Anzahl, um alle Fehler bei der Übermittlung der Anforderung anzuzeigen.

## <a name="reprocess-a-request"></a>Erneutes Verarbeiten einer Anforderung

Sollte bei einer Anforderung ein Fehler auftreten, können Sie die Anforderung erneut verarbeiten, um den Vorgang zu wiederholen. Die erneute Verarbeitung ist nur für Anforderungen mit dem Status **Übermittlungsfehler** oder **Teilweise übermittelt** und einem Abschlussdatum möglich, das weniger eine Woche zurückliegt.

**Erforderliche Rolle:** Globaler Administrator, Benutzeradministrator, Katalogbesitzer oder Zugriffspaket-Manager

1. Klicken Sie im Azure-Portal auf **Azure Active Directory** und dann auf **Identity Governance**.

1. Klicken Sie im Menü auf der linken Seite auf **Zugriffspakete**, und öffnen Sie das Zugriffspaket.

1. Klicken Sie auf **Anforderungen**.

1. Wählen Sie die Anforderung aus, die Sie erneut verarbeiten möchten.

1. Klicken Sie im Detailbereich der Anforderung auf **Anforderung erneut verarbeiten**.

    ![Erneutes Verarbeiten einer nicht erfolgreichen Anforderung](./media/entitlement-management-access-package-requests/reprocess-request.png)

## <a name="cancel-a-pending-request"></a>Abbrechen einer ausstehenden Anforderung

Es können nur ausstehende Anforderungen abgebrochen werden, die noch nicht übermittelt wurden oder bei deren Übermittlung ein Fehler aufgetreten ist.

**Erforderliche Rolle:** Globaler Administrator, Benutzeradministrator, Katalogbesitzer oder Zugriffspaket-Manager

1. Klicken Sie im Azure-Portal auf **Azure Active Directory** und dann auf **Identity Governance**.

1. Klicken Sie im Menü auf der linken Seite auf **Zugriffspakete**, und öffnen Sie das Zugriffspaket.

1. Klicken Sie auf **Anforderungen**.

1. Wählen Sie die Anforderung aus, die Sie abbrechen möchten.

1. Klicken Sie im Detailbereich der Anforderung auf **Anforderung abbrechen**.

## <a name="next-steps"></a>Nächste Schritte

- [Ändern der Anforderungs- und Genehmigungseinstellungen für ein Zugriffspaket](entitlement-management-access-package-request-policy.md)
- [Anzeigen, Hinzufügen und Entfernen von Zuweisungen für ein Zugriffspaket](entitlement-management-access-package-assignments.md)