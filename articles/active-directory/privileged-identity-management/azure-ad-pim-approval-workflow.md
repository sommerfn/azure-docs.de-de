---
title: Genehmigen oder Ablehnen von Anforderungen für Azure AD-Rollen in PIM – Azure Active Directory | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Anforderungen für Azure AD-Rollen in Azure AD Privileged Identity Management (PIM) genehmigen oder ablehnen.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: pim
ms.date: 04/09/2019
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: f83cb38567feb51ba7959ada7730d66ded677bf9
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/29/2019
ms.locfileid: "67476539"
---
# <a name="approve-or-deny-requests-for-azure-ad-roles-in-pim"></a>Genehmigen oder Ablehnen von Anforderungen für Azure AD-Rollen in PIM

Mit Azure Active Directory (Azure AD) Privileged Identity Management (PIM) können Sie Rollen konfigurieren, um die Genehmigung für die Aktivierung anzufordern, und einen oder mehrere Benutzer oder Gruppen als delegierte genehmigende Personen auswählen. Delegierte genehmigende Personen haben zum Genehmigen von Anforderungen 24 Stunden Zeit. Wenn eine Anforderung nicht innerhalb von 24 Stunden genehmigt wird, muss der berechtigte Benutzer eine neue Anforderung senden. Das 24-Stunden-Zeitfenster für die Genehmigung ist nicht konfigurierbar.

Führen Sie die in diesem Artikel beschriebenen Schritte aus, um Anforderungen für Azure AD-Rollen zu genehmigen oder abzulehnen.

## <a name="view-pending-requests"></a>Anzeigen ausstehender Anforderungen

Als delegierte genehmigende Person erhalten Sie eine E-Mail-Benachrichtigung, wenn Ihre Genehmigung einer Anforderung für eine Azure AD-Rolle aussteht. Sie können diese ausstehenden Anforderungen in PIM anzeigen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

1. Öffnen Sie **Azure AD Privileged Identity Management**.

1. Klicken Sie auf **Azure AD-Rollen**.

1. Klicken Sie auf **Anforderungen genehmigen**.

    ![Azure AD-Rollen – Anforderungen genehmigen](./media/azure-ad-pim-approval-workflow/pim-directory-roles-approve-requests.png)

    Es wird eine Liste der Anforderungen angezeigt, die Sie noch genehmigen müssen.

## <a name="approve-requests"></a>Genehmigen von Anforderungen

1. Wählen Sie die Anforderungen aus, die Sie genehmigen möchten, und klicken Sie dann auf **Genehmigen**, um den Bereich „Ausgewählte Anforderungen genehmigen“ zu öffnen.

    ![Liste „Anforderungen genehmigen“ mit hervorgehobener Option „Genehmigen“](./media/azure-ad-pim-approval-workflow/pim-approve-requests-list.png)

1. Geben Sie im Feld **Genehmigungsgrund** einen Grund ein.

    ![Bereich zum Genehmigen ausgewählter Anforderungen mit einem Genehmigungsgrund](./media/azure-ad-pim-approval-workflow/pim-approve-selected-requests.png)

1. Klicken Sie auf **Approve**.

    Das Statussymbol wird mit Ihrer Genehmigung aktualisiert.

    ![Bereich zum Genehmigen ausgewählter Anforderungen nach Klicken auf die Schaltfläche „Genehmigen“](./media/azure-ad-pim-approval-workflow/pim-approve-status.png)

## <a name="deny-requests"></a>Ablehnen von Anforderungen

1. Wählen Sie die Anforderungen aus, die Sie ablehnen möchten, und klicken Sie dann auf **Ablehnen**, um den Bereich „Ausgewählte Anforderungen ablehnen“ zu öffnen.

    ![Liste „Anforderungen genehmigen“ mit hervorgehobener Option „Ablehnen“](./media/azure-ad-pim-approval-workflow/pim-deny-requests-list.png)

1. Geben Sie im Feld **Ablehnungsgrund** einen Grund ein.

    ![Bereich zum Ablehnen ausgewählter Anforderungen mit einem Ablehnungsgrund](./media/azure-ad-pim-approval-workflow/pim-deny-selected-requests.png)

1. Klicken Sie auf **Ablehnen**.

    Das Statussymbol wird mit Ihrer Ablehnung aktualisiert.

## <a name="next-steps"></a>Nächste Schritte

- [E-Mail-Benachrichtigungen in PIM](pim-email-notifications.md)
- [Genehmigen oder Ablehnen von Anforderungen für Azure-Ressourcenrollen in PIM](pim-resource-roles-approval-workflow.md)
