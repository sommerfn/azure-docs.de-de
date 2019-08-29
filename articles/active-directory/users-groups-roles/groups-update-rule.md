---
title: Aktualisieren und Verwalten einer dynamischen Gruppenregel und Problembehandlung bei der Mitgliedschaft – Azure Active Directory | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie im Azure-Portal eine Regel für die Gruppenmitgliedschaft erstellen und den Status überprüfen.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 08/12/2019
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: ce3bce5e2656efea0a4fc3d7aa6be46f1e6926ec
ms.sourcegitcommit: bb8e9f22db4b6f848c7db0ebdfc10e547779cccc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2019
ms.locfileid: "69656925"
---
# <a name="update-a-dynamic-group-to-manage-membership-in-azure-active-directory"></a>Aktualisieren und Verwalten einer dynamischen Gruppenregel und Problembehandlung bei der Mitgliedschaft in Azure Active Directory

In Azure Active Directory (Azure AD) können Sie mithilfe von Regeln die Gruppenmitgliedschaft auf der Grundlage von Benutzer- oder Geräteeigenschaften ermitteln. In diesem Artikel erfahren Sie, wie Sie im Azure-Portal eine Regel für eine dynamische Gruppe einrichten.
Die dynamische Mitgliedschaft wird für Sicherheitsgruppen und Office 365-Gruppen unterstützt. Wenn eine Regel für die Gruppenmitgliedschaft angewendet wird, werden Benutzer- und Geräteattribute auf Übereinstimmungen mit der Mitgliedschaftsregel geprüft. Wenn sich ein Attribut für einen Benutzer oder ein Gerät ändert, werden alle dynamischen Gruppenregeln in der Organisation verarbeitet, um Mitgliedschaftsänderungen zu berücksichtigen. Benutzer und Geräte werden hinzugefügt oder entfernt, wenn sie die Bedingungen für eine Gruppe erfüllen.

Beispiele für Syntax, unterstützte Eigenschaften, Operatoren und Werte für eine Mitgliedschaftsregel finden Sie unter [Regeln für eine dynamische Mitgliedschaft für Gruppen in Azure Active Directory](groups-dynamic-membership.md).

## <a name="to-update-a-group-membership-rule"></a>So aktualisieren Sie eine Regel für die Gruppenmitgliedschaft

1. Melden Sie sich beim [Azure AD Admin Center](https://aad.portal.azure.com) mit einem Konto an, das der Rolle des globalen Administrators, Intune-Administrators oder Benutzeradministrators in dem Mandanten angehört.
1. Wählen Sie **Gruppen** > **Alle Gruppen** aus.
1. Wählen Sie eine Gruppe aus, um ihr Profil zu öffnen.
1. Wählen Sie auf der Profilseite für die Gruppe **Dynamische Mitgliedschaftsregeln** aus. Der Regel-Generator unterstützt bis zu fünf Ausdrücke. Um weitere Ausdrücke hinzuzufügen, müssen Sie das Textfeld verwenden.

   ![Hinzufügen einer Mitgliedschaftsregel für eine dynamische Gruppe](./media/groups-update-rule/update-dynamic-group-rule.png)

1. So zeigen Sie die benutzerdefinierten Erweiterungseigenschaften für Ihre Mitgliedschaftsregel an
   1. Wählen Sie **Benutzerdefinierte Erweiterungseigenschaften abrufen** aus.
   2. Geben Sie die Anwendungs-ID ein, und wählen Sie anschließend **Eigenschaften aktualisieren** aus.
1. Klicken Sie nach dem Aktualisieren der Regel auf **Speichern**.

Sollte die eingegebene Regel ungültig sein, wird durch eine Azure-Benachrichtigung im Portal angezeigt, warum die Regel nicht verarbeitet werden konnte. Lesen Sie sich die Erklärung aufmerksam durch, um die Regel korrigieren zu können.

## <a name="check-processing-status-for-a-rule"></a>Überprüfen des Verarbeitungsstatus für eine Regel

Auf der Seite **Übersicht** für die Gruppe sehen Sie den Verarbeitungsstatus der Mitgliedschaft und das zuletzt geänderte Datum.
  
  ![Anzeigen des Status der dynamischen Gruppe](./media/groups-create-rule/group-status.png)

Für **Verarbeitungsstatus der Mitgliedschaft** können die folgenden Statusmeldungen angezeigt werden:

* **Auswertung wird durchgeführt:**  Die Gruppenänderung wurde empfangen, und die Aktualisierungen werden ausgewertet.
* **Verarbeitung:** Die Updates werden verarbeitet.
* **Aktualisierung abgeschlossen:** Die Verarbeitung wurde abgeschlossen, alle anwendbaren Aktualisierungen wurden vorgenommen.
* **Verarbeitungsfehler:**  Die Verarbeitung konnte aufgrund eines Fehlers beim Auswerten der Mitgliedschaftsregel nicht abgeschlossen werden.
* **Aktualisierung angehalten:** Aktualisierungen der Regeln für die dynamische Mitgliedschaft wurden vom Administrator angehalten. „MembershipRuleProcessingState“ ist auf „Paused“ festgelegt.

Für **Letzte Aktualisierung der Mitgliedschaft** können die folgenden Statusmeldungen angezeigt werden:

* **Datum und Uhrzeit**: Der Zeitpunkt der letzten Aktualisierung der Mitgliedschaft.
* **In Bearbeitung**: Aktualisierungen sind derzeit in Bearbeitung.
* **Unbekannt:** Der Zeitpunkt der letzten Aktualisierung kann nicht abgerufen werden. Die Gruppe ist möglicherweise neu.

Wenn bei der Verarbeitung der Mitgliedschaftsregel für eine bestimmte Gruppe ein Fehler auftritt, wird oben auf der Seite **Übersicht** der Gruppe eine Warnung angezeigt. Wenn für alle Gruppen innerhalb des Mandanten für mehr als 24 Stunden keine ausstehenden Aktualisierungen der dynamischen Mitgliedschaft verarbeitet werden können, wird oben in **Alle Gruppen** eine Warnung angezeigt.

![Warnungen aufgrund von Verarbeitungsfehlern](./media/groups-create-rule/processing-error.png)

## <a name="next-steps"></a>Nächste Schritte

Diese Artikel bieten zusätzliche Informationen zur Arbeit mit dynamischen Gruppen in Azure AD.

* Eine vollständige Referenz zur dynamischen Regelstruktur finden Sie unter [Syntax der Regel für dynamische Mitgliedschaft](groups-dynamic-membership.md).
* [Erstellen einer statischen Mitgliedschaftsgruppe und Hinzufügen von Mitgliedern](../fundamentals/active-directory-groups-create-azure-portal.md).
