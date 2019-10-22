---
title: Ändern der Lebenszykluseinstellungen für ein Zugriffspaket in der Azure AD-Berechtigungsverwaltung (Vorschauversion) – Azure Active Directory
description: Erfahren Sie, wie Sie die Lebenszykluseinstellungen für ein Zugriffspaket in der Azure Active Directory-Berechtigungsverwaltung (Vorschauversion) ändern.
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
ms.openlocfilehash: 46d63a12443edf04db3570fa43fbd8f20619122e
ms.sourcegitcommit: bb65043d5e49b8af94bba0e96c36796987f5a2be
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2019
ms.locfileid: "72391973"
---
# <a name="change-lifecycle-settings-for-an-access-package-in-azure-ad-entitlement-management-preview"></a>Ändern der Lebenszykluseinstellungen für ein Zugriffspaket in der Azure AD-Berechtigungsverwaltung (Vorschauversion)

> [!IMPORTANT]
> Die Berechtigungsverwaltung von Azure Active Directory (Azure AD) befindet sich derzeit in der öffentlichen Vorschau.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Als Zugriffspaket-Manager können Sie die Lebenszykluseinstellungen für ein Zugriffspaket jederzeit ändern, indem Sie eine vorhandene Richtlinie bearbeiten. Wenn Sie das Ablaufdatum für eine Richtlinie ändern, ändert sich das Ablaufdatum für Anforderungen mit ausstehender Genehmigung oder genehmigte Anforderungen nicht.

In diesem Artikel wird beschrieben, wie die Lebenszykluseinstellungen für ein vorhandenes Zugriffspaket geändert werden.

## <a name="open-lifecycle-settings"></a>Öffnen der Lebenszykluseinstellungen

Wenn Sie die Lebenszykluseinstellungen für ein Zugriffspaket ändern möchten, müssen Sie die entsprechende Richtlinie öffnen. Führen Sie die folgenden Schritte aus, um die Lebenszykluseinstellungen für ein Zugriffspaket zu öffnen.

**Erforderliche Rolle:** Globaler Administrator, Benutzeradministrator, Katalogbesitzer oder Zugriffspaket-Manager

1. Klicken Sie im Azure-Portal auf **Azure Active Directory** und dann auf **Identity Governance**.

1. Klicken Sie im Menü auf der linken Seite auf **Zugriffspakete**, und öffnen Sie das Zugriffspaket.

1. Klicken Sie auf **Richtlinien**, und klicken Sie dann auf die Richtlinie mit den Lebenszykluseinstellungen, die Sie bearbeiten möchten.

    Der Bereich „Richtliniendetails“ wird unten auf der Seite geöffnet.

    ![Zugriffspaket: Bereich „Richtliniendetails“](./media/entitlement-management-shared/policy-details.png)

1. Klicken Sie auf **Bearbeiten**, um die Richtlinie zu bearbeiten.

    ![Zugriffspaket: Richtlinie bearbeiten](./media/entitlement-management-shared/policy-edit.png)

1. Klicken Sie auf die Registerkarte **Lebenszyklus**, um die Lebenszykluseinstellungen zu öffnen.

[!INCLUDE [Entitlement management lifecycle policy](../../../includes/active-directory-entitlement-management-lifecycle-policy.md)]

## <a name="next-steps"></a>Nächste Schritte

- [Ändern der Anforderungs- und Genehmigungseinstellungen für ein Zugriffspaket](entitlement-management-access-package-request-policy.md)