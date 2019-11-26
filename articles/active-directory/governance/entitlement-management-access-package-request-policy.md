---
title: Ändern der Anforderungs- und Genehmigungseinstellungen für ein Zugriffspaket in der Azure AD-Berechtigungsverwaltung – Azure Active Directory
description: Erfahren Sie, wie Sie die Anforderungs- und Genehmigungseinstellungen für ein Zugriffspaket in der Azure Active Directory-Berechtigungsverwaltung ändern.
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
ms.openlocfilehash: 649a4a50766d26f73584dae4481652c8007b2e9e
ms.sourcegitcommit: 98ce5583e376943aaa9773bf8efe0b324a55e58c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73174675"
---
# <a name="change-request-and-approval-settings-for-an-access-package-in-azure-ad-entitlement-management"></a>Ändern der Anforderungs- und Genehmigungseinstellungen für ein Zugriffspaket in der Azure AD-Berechtigungsverwaltung

Als Zugriffspaket-Manager können Sie jederzeit die Benutzer ändern, die ein Zugriffspaket anfordern können, indem Sie die Richtlinie bearbeiten oder eine neue Richtlinie hinzufügen. Sie können auch die Genehmigungseinstellungen ändern.

In diesem Artikel wird beschrieben, wie die Anforderungs- und Genehmigungseinstellungen für ein vorhandenes Zugriffspaket geändert werden.

## <a name="choose-between-one-or-multiple-polices"></a>Auswählen zwischen einer oder mehreren Richtlinien

Mit einer Richtlinie legen Sie fest, wer ein Zugriffspaket anfordern kann. Beim Erstellen eines Zugriffspakets geben Sie die Anforderungs- und Genehmigungseinstellung an, die eine Richtlinie erstellt. Die meisten Zugriffspakete enthalten eine einzige Richtlinie, aber ein einzelnes Zugriffspaket kann über mehrere Richtlinien verfügen. Sie erstellen mehrere Richtlinien für ein Zugriffspaket, wenn Sie unterschiedlichen Benutzergruppen Zuweisungen mit unterschiedlichen Anforderungs- und Genehmigungseinstellungen erteilen möchten. Beispielsweise ist es mit einer einzelnen Richtlinie nicht möglich, einem Zugriffspaket interne und externe Benutzer zuzuweisen. Erstellen Sie hierfür in einem Zugriffspaket zwei separate Richtlinien für interne bzw. externe Benutzer. Gelten für einen Benutzer mehrere Richtlinien, wird er beim Anfordern dazu aufgefordert, die Richtlinie auszuwählen, der er zugewiesen werden möchte. Im folgenden Diagramm ist ein Zugriffspaket mit zwei Richtlinien dargestellt.

![Mehrere Richtlinien in einem Zugriffspaket](./media/entitlement-management-access-package-request-policy/access-package-policy.png)

| Szenario | Anzahl von Richtlinien |
| --- | --- |
| Für alle Benutzer in meinem Verzeichnis sollen die gleichen Anforderungs- und Genehmigungseinstellungen für ein Zugriffspaket gelten. | Eine |
| Alle Benutzer in bestimmten verbundenen Organisationen sollen ein Zugriffspaket anfordern können. | Eine |
| Sowohl die Benutzer in meinem Verzeichnis als auch Benutzer außerhalb meines Verzeichnisses sollen ein Zugriffspaket anfordern können. | Mehrere |
| Ich möchte für einige Benutzer andere Genehmigungseinstellungen angeben. | Mehrere |
| Ich möchte, dass die Zugriffspaketzuweisungen einiger Benutzer ablaufen, während andere Benutzer ihren Zugriff verlängern können. | Mehrere |

### <a name="open-an-existing-policy-of-request-and-approval-settings"></a>Öffnen einer vorhandenen Richtlinie mit Anforderungs- und Genehmigungseinstellungen

Wenn Sie die Anforderungs- und Genehmigungseinstellungen für ein Zugriffspaket ändern möchten, müssen Sie die entsprechende Richtlinie öffnen. Führen Sie die folgenden Schritte aus, um die Anforderungs- und Genehmigungseinstellungen für ein Zugriffspaket zu öffnen.

**Erforderliche Rolle:** Globaler Administrator, Benutzeradministrator, Katalogbesitzer oder Zugriffspaket-Manager

1. Klicken Sie im Azure-Portal auf **Azure Active Directory** und dann auf **Identity Governance**.

1. Klicken Sie im Menü auf der linken Seite auf **Zugriffspakete**, und öffnen Sie das Zugriffspaket.

1. Klicken Sie auf **Richtlinien** und anschließend auf die Richtline, die Sie bearbeiten möchten.

    Der Bereich „Richtliniendetails“ wird unten auf der Seite geöffnet.

    ![Zugriffspaket: Bereich „Richtliniendetails“](./media/entitlement-management-shared/policy-details.png)

1. Klicken Sie auf **Bearbeiten**, um die Richtlinie zu bearbeiten.

    ![Zugriffspaket: Richtlinie bearbeiten](./media/entitlement-management-shared/policy-edit.png)

1. Klicken Sie auf die Registerkarte **Anforderungen**, um die Anforderungs- und Genehmigungseinstellungen zu öffnen.

1. Führen Sie die Schritte in einem der folgenden Abschnitte zu Anforderungen aus.

### <a name="add-a-new-policy-of-request-and-approval-settings"></a>Hinzufügen einer neuen Richtlinie mit Anforderungs- und Genehmigungseinstellungen

Wenn Sie über eine Gruppe von Benutzern verfügen, für die unterschiedliche Anforderungs- und Genehmigungseinstellungen gelten sollen, müssen Sie wahrscheinlich eine neue Richtlinie erstellen. Führen Sie die folgenden Schritte aus, um einem vorhandenen Zugriffspaket eine neue Richtlinie hinzuzufügen.

**Erforderliche Rolle:** Globaler Administrator, Benutzeradministrator, Katalogbesitzer oder Zugriffspaket-Manager

1. Klicken Sie im Azure-Portal auf **Azure Active Directory** und dann auf **Identity Governance**.

1. Klicken Sie im Menü auf der linken Seite auf **Zugriffspakete**, und öffnen Sie das Zugriffspaket.

1. Klicken Sie auf **Richtlinien** und dann auf **Richtlinie hinzufügen**.

1. Geben Sie einen Namen und eine Beschreibung für die Richtlinie ein.

    ![Erstellen der Richtlinie mit Name und Beschreibung](./media/entitlement-management-access-package-request-policy/policy-name-description.png)

1. Klicken Sie auf **Weiter**, um die Registerkarte **Anforderungen** zu öffnen.

1. Führen Sie die Schritte in einem der folgenden Abschnitte zu Anforderungen aus.

[!INCLUDE [Entitlement management request policy](../../../includes/active-directory-entitlement-management-request-policy.md)]

Wenn Sie eine Richtlinie bearbeiten, klicken Sie auf **Aktualisieren**. Wenn Sie eine neue Richtlinie hinzufügen, klicken Sie auf **Erstellen**.

## <a name="next-steps"></a>Nächste Schritte

- [Ändern der Lebenszykluseinstellungen für ein Zugriffspaket](entitlement-management-access-package-lifecycle-policy.md)
- [Anzeigen der Anforderungen für ein Zugriffspaket](entitlement-management-access-package-requests.md)