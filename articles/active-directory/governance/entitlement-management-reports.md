---
title: Anzeigen von Berichten und Protokollen in der Azure AD-Berechtigungsverwaltung – Azure Active Directory
description: Erfahren Sie, wie Sie den Bericht zu Benutzerzuweisungen und Überwachungsprotokolle in der Azure Active Directory-Berechtigungsverwaltung anzeigen.
services: active-directory
documentationCenter: ''
author: msaburnley
manager: daveba
editor: jocastel-MSFT
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 10/28/2019
ms.author: ajburnle
ms.reviewer: jocastel
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3302fc3b2513794cd66d1ebf6db2cbcdb0f713dd
ms.sourcegitcommit: 98ce5583e376943aaa9773bf8efe0b324a55e58c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73173879"
---
# <a name="view-reports-and-logs-in-azure-ad-entitlement-management"></a>Anzeigen von Berichten und Protokollen in der Azure AD-Berechtigungsverwaltung

Die Berichte der Azure AD-Berechtigungsverwaltung und das Azure AD-Überwachungsprotokoll enthalten zusätzliche Details zu den Ressourcen, auf die Benutzer Zugriff haben. Als Administrator können Sie die Zugriffspakete und Ressourcenzuweisungen für einen Benutzer sowie die Anforderungsprotokolle zu Überprüfungszwecken oder zum Ermitteln des Status einer Benutzeranforderung anzeigen. In diesem Artikel wird die Verwendung von Berichten der Berechtigungsverwaltung und von Azure AD-Überwachungsprotokollen beschrieben.

Sehen Sie sich das folgende Video an, um zu erfahren, wie Sie in der Berechtigungsverwaltung anzeigen können, auf welche Ressourcen Benutzer Zugriff haben:

>[!VIDEO https://www.youtube.com/embed/omtNJ7ySjS0]

## <a name="view-access-packages-for-a-user"></a>Anzeigen von Zugriffspaketen für einen Benutzer

Mit diesem Bericht können Sie alle Zugriffspakete auflisten, die ein Benutzer anfordern kann und die dem Benutzer derzeit zugewiesen sind.

**Erforderliche Rolle:** Globaler Administrator oder Benutzeradministrator

1. Klicken Sie auf **Azure Active Directory**, und klicken Sie dann auf **Identity Governance**.

1. Klicken Sie im linken Menü auf **Berichte**.

1. Klicken Sie auf **Zugriffspakete für einen Benutzer**.

1. Klicken Sie auf **Benutzer auswählen**, um den Bereich „Benutzer auswählen“ zu öffnen.

1. Suchen Sie in der Liste nach dem Benutzer, und klicken Sie dann auf **Auswählen**.

    Auf der Registerkarte **Kann anfordern** wird eine Liste mit den Zugriffspaketen angezeigt, die der Benutzer anfordern kann. Diese Liste beruht auf den [Anforderungsrichtlinien](entitlement-management-access-package-request-policy.md#for-users-in-your-directory), die für die Zugriffspakete definiert sind. 

    ![Zugriffspakete für einen Benutzer](./media/entitlement-management-reports/access-packages-report.png)

1. Wenn es mehr als eine Ressourcenrolle oder Richtlinie für ein Zugriffspaket gibt, klicken Sie auf den Eintrag für die Ressourcenrollen bzw. Richtlinien, um die Auswahldetails anzuzeigen.

1. Klicken Sie auf die Registerkarte **Zugewiesen**, um eine Liste der Zugriffspakete anzuzeigen, die dem Benutzer derzeit zugewiesen sind. Wenn einem Benutzer ein Zugriffspaket zugewiesen ist, bedeutet dies, dass der Benutzer Zugriff auf alle Ressourcenrollen im Zugriffspaket hat.

## <a name="view-resource-assignments-for-a-user"></a>Anzeigen der Ressourcenzuweisungen für einen Benutzer

Mit diesem Bericht können Sie die Ressourcen auflisten, die einem Benutzer derzeit in der Berechtigungsverwaltung zugewiesen sind. Beachten Sie, dass dieser Bericht für mit der Berechtigungsverwaltung verwaltete Ressourcen gilt. Der Benutzer hat möglicherweise Zugriff auf andere Ressourcen in Ihrem Verzeichnis außerhalb der Berechtigungsverwaltung.

**Erforderliche Rolle:** Globaler Administrator oder Benutzeradministrator

1. Klicken Sie auf **Azure Active Directory**, und klicken Sie dann auf **Identity Governance**.

1. Klicken Sie im linken Menü auf **Berichte**.

1. Klicken Sie auf **Ressourcenzuweisungen für einen Benutzer**.

1. Klicken Sie auf **Benutzer auswählen**, um den Bereich „Benutzer auswählen“ zu öffnen.

1. Suchen Sie in der Liste nach dem Benutzer, und klicken Sie dann auf **Auswählen**.

    Es wird eine Liste der Ressourcen angezeigt, die dem Benutzer aktuell zugewiesen sind. In der Liste werden auch das Zugriffspaket und die Richtlinie angezeigt, von denen die Ressourcenrolle stammt, sowie das Start-und Enddatum für den Zugriff.
    
    Wenn ein Benutzer in zwei oder mehr Paketen Zugriff auf dieselbe Ressource hat, können Sie auf einen Pfeil klicken und die einzelnen Pakete und Richtlinien anzeigen.

    ![Ressourcenzuweisungen für einen Benutzer](./media/entitlement-management-reports/resource-assignments-report.png)

## <a name="determine-the-status-of-a-users-request"></a>Ermitteln des Status einer Benutzeranforderung

Sie können das Azure AD-Überwachungsprotokoll verwenden, um zusätzliche Details darüber abzurufen, wie ein Benutzer Zugriff auf ein Zugriffspaket angefordert und erhalten hat. Insbesondere können Sie die Protokolleinträge in den Kategorien `EntitlementManagement` und `UserManagement` nutzen, um zusätzliche Details zu den Verarbeitungsschritten für jede Anforderung zu erhalten.  

1. Klicken Sie auf **Azure Active Directory**, und klicken Sie dann auf **Überwachungsprotokolle**.

1. Ändern Sie im oberen Bereich die **Kategorie** abhängig vom gewünschten Überwachungsdatensatz in `EntitlementManagement` oder `UserManagement`.  

1. Klicken Sie auf **Anwenden**.

1. Klicken Sie zum Herunterladen der Protokolle auf **Herunterladen**.

Wenn Azure AD eine neue Anforderung empfängt, wird ein Überwachungsdatensatz geschrieben, in dem die **Kategorie** `EntitlementManagement` und die **Aktivität** in der Regel `User requests access package assignment` lautet.  Bei einer im Azure-Portal erstellten direkten Zuweisung heißt das Feld **Aktivität** des Überwachungsdatensatzes `Administrator directly assigns user to access package`, und der Benutzer, der die Zuweisung vornimmt, wird durch das Feld **ActorUserPrincipalName** identifiziert.

Azure AD schreibt zusätzliche Überwachungsdatensätze, während die Anforderung ausgeführt wird, die Folgendes umfassen:

| Category | Aktivität | Anfragestatus |
| :---- | :------------ | :------------ |
| `EntitlementManagement` | `Auto approve access package assignment request` | Anforderung muss nicht genehmigt werden |
| `UserManagement` | `Create request approval` | Anforderung muss genehmigt werden |
| `UserManagement` | `Add approver to request approval` | Anforderung muss genehmigt werden |
| `EntitlementManagement` | `Approve access package assignment request` | Anforderung genehmigt |
| `EntitlementManagement` | `Ready to fulfill access package assignment request` |Anforderung wurde genehmigt oder muss nicht genehmigt werden |

Wenn einem Benutzer Zugriff zugewiesen wird, schreibt Azure AD einen Überwachungsdatensatz für die Kategorie `EntitlementManagement` mit der **Aktivität** `Fulfill access package assignment`.  Der Benutzer, der Zugriff erhalten hat, wird durch das Feld **ActorUserPrincipalName** identifiziert.

Wenn kein Zugriff zugewiesen wurde, schreibt Azure AD einen Überwachungsdatensatz für die Kategorie `EntitlementManagement`, wobei die **Aktivität** entweder `Deny access package assignment request` lautet, wenn die Anforderung von einer genehmigenden Person abgelehnt wurde, oder `Access package assignment request timed out (no approver action taken)`, wenn bei der Anforderung ein Timeout aufgetreten ist, bevor sie von einer genehmigenden Person genehmigt werden konnte.

Wenn die Zugriffspaketzuweisung des Benutzers abläuft, vom Benutzer abgebrochen oder von einem Administrator entfernt wird, schreibt Azure AD einen Überwachungsdatensatz für die Kategorie `EntitlementManagement` mit der **Aktivität** `Remove access package assignment`.

## <a name="next-steps"></a>Nächste Schritte

- [Problembehandlung bei der Azure AD-Berechtigungsverwaltung](entitlement-management-troubleshoot.md)
- [Gängige Szenarios](entitlement-management-scenarios.md)
