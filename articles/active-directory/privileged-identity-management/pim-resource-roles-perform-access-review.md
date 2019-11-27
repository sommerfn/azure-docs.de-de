---
title: Überprüfen des Zugriffs auf Azure-Ressourcenrollen in PIM – Azure AD | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie den Zugriff auf Azure-Ressourcenrollen in Azure AD Privileged Identity Management (PIM) überprüfen.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: pim
ms.date: 11/08/2019
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7ddb4d0ff1339f1427c5041528cdbe464a345b37
ms.sourcegitcommit: 16c5374d7bcb086e417802b72d9383f8e65b24a7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73847001"
---
# <a name="review-access-to-azure-resource-roles-in-privileged-identity-management"></a>Überprüfen des Zugriffs auf Azure-Ressourcenrollen in Privileged Identity Management

Zugriffsüberprüfungen in Privileged Identity Management (PIM) können dabei helfen, den Zugriff auf privilegierte Rollen in Azure Active Directory (Azure AD) zu sichern. In diesem Artikel werden die Schritte beschrieben, mit denen Sie eine Überprüfung Ihrer privilegierten Rollenzuweisungen in einer Azure AD-Zugriffsüberprüfung überprüfen.

Wenn Ihnen eine Administratorrolle zugewiesen ist, müssen Sie möglicherweise eine Zugriffsüberprüfung durch Ihren Administrator durchführen lassen, um nachzuweisen, dass Sie eine Rolle wirklich benötigen. Die Bestätigungsanforderung kann als E-Mail mit einem Link eingehen, oder Sie können die Bestätigung im [Azure-Portal](https://portal.azure.com) vornehmen.

Wenn Sie Administrator für privilegierte Rollen sind und sich für die Zugriffsüberprüfungen interessieren, erhalten Sie weitere Informationen unter [Starten einer Zugriffsüberprüfung](pim-resource-roles-start-access-review.md).

## <a name="approve-or-deny-access"></a>Genehmigen oder Verweigern des Zugriffs

Sie können Zugriff in Abhängigkeit davon, ob Sie diese Rolle noch verwenden oder nicht, genehmigen oder verweigern. Wählen Sie **Genehmigen** aus, wenn Sie in der Rolle bleiben möchten, oder **Verweigern**, wenn Sie den Zugriff nicht mehr benötigen. Ihr Status ändert sich erst, nachdem der Prüfer die Ergebnisse angewendet hat.

Gehen Sie wie folgt vor, um die Zugriffsüberprüfung zu finden und abzuschließen:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
1. Wählen Sie **Azure Active Directory** aus, und öffnen Sie **Privileged Identity Management**.
1. Wählen Sie **Zugriff überprüfen** aus.

   ![Screenshot der Privileged Identity Management-Anwendung mit ausgewähltem Blatt „Zugriff überprüfen“](media/pim-resource-roles-perform-access-review/rbac-access-review-complete.png)

1. Wählen Sie die Überprüfung aus, die Sie abschließen möchten.
1. Wählen Sie **Genehmigen** oder **Ablehnen** aus. Geben Sie im Feld **Geben Sie eine Grund an** eine geschäftliche Begründung für Ihre Entscheidung ein, falls erforderlich.

   ![Screenshot der Seite „Details überprüfen“](media/pim-resource-roles-perform-access-review/rbac-access-review-choice.png)

## <a name="next-steps"></a>Nächste Schritte

- [Ausführen einer Zugriffsüberprüfung meiner Azure AD-Rollen in Privileged Identity Management](pim-how-to-perform-security-review.md)
