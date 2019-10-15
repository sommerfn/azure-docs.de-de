---
title: Zuweisen oder Entfernen von Lizenzen – Azure Active Directory | Microsoft-Dokumentation
description: Hier finden Sie Anweisungen zum Zuweisen oder Entfernen von Azure Active Directory-Lizenzen für Ihre Benutzer oder Gruppen.
services: active-directory
author: msaburnley
manager: daveba
ms.assetid: f8b932bc-8b4f-42b5-a2d3-f2c076234a78
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 09/18/2018
ms.author: ajburnle
ms.reviewer: jeffsta
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 869713967a6a89611ab2ea9ca7b7516f090a5f92
ms.sourcegitcommit: f9e81b39693206b824e40d7657d0466246aadd6e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/08/2019
ms.locfileid: "72034287"
---
# <a name="assign-or-remove-licenses-in-the-azure-active-directory-portal"></a>Zuweisen oder Entfernen von Lizenzen im Azure Active Directory-Portal

Viele Dienste von Azure Active Directory (Azure AD) erfordern, dass Sie Ihre einzelnen Benutzer oder Gruppen (und die zugehörigen Mitglieder) für den jeweiligen Dienst lizenzieren. Nur Benutzer mit aktiven Lizenzen können auf die lizenzierten Azure AD-Dienste zugreifen und diese verwenden, auf die das zutrifft.

## <a name="available-license-plans"></a>Verfügbare Lizenzpläne

Für den Azure AD-Dienst sind mehrere Lizenzpläne verfügbar, einschließlich der folgenden:

- Azure AD Free

- Azure AD Premium P1

- Azure AD Premium P2

Unter [Welche Lizenz benötige ich?](https://azure.microsoft.com/pricing/details/active-directory/) finden Sie spezifische Informationen zu jedem Lizenzplan sowie die zugehörigen Lizenzdetails.

Nicht alle Microsoft-Dienste sind an allen Standorten verfügbar. Bevor einer Gruppe eine Lizenz zugewiesen werden kann, müssen Sie den **Nutzungsspeicherort** für alle Mitglieder angeben. Sie können diesen Wert im Bereich **Azure Active Directory &gt; Benutzer &gt; Profil &gt; Einstellungen** in Azure AD festlegen. Jeder Benutzer, dessen Verwendungsstandort nicht angegeben ist, erbt den Standort der Azure AD-Organisation.

## <a name="view-license-plans-and-plan-details"></a>Anzeigen von Lizenzplänen und Plandetails

Sie können Ihre verfügbaren Dienstpläne einschließlich der einzelnen Lizenzen anzeigen, die bevorstehenden Ablaufdaten überprüfen und die Anzahl der verfügbaren Zuweisungen anzeigen.

### <a name="to-find-your-service-plan-and-plan-details"></a>So finden Sie Ihren Serviceplan und die Plandetails

1. Melden Sie sich mit dem Konto eines Lizenzadministrators Ihrer Azure AD-Organisation beim [Azure-Portal](https://portal.azure.com/) an.

1. Wählen Sie **Azure Active Directory** und dann **Lizenzen** aus.

    ![Seite „Lizenzen“ mit der Anzahl der erworbenen Dienste und zugewiesenen Lizenzen](media/license-users-groups/license-details-blade.png)

1. Wählen Sie den Link **Erworben** aus, um die Seite **Produkte** und die Angaben **Zugewiesen**, **Verfügbar** und **Läuft bald ab** für Ihre Lizenzpläne anzuzeigen.

    ![Dienstseite mit Dienstlizenzplänen und den zugehörigen Lizenzinformationen](media/license-users-groups/license-products-blade-with-products.png)

1. Wählen Sie den Namen eines Plans aus, um die dafür lizenzierten Benutzer und Gruppen anzuzeigen.

## <a name="assign-licenses-to-users-or-groups"></a>Zuweisen von Lizenzen zu Benutzern oder Gruppen

Stellen Sie sicher, dass jeder, der einen lizenzierten Azure AD-Dienst nutzen muss, über die entsprechende Lizenz verfügt. Sie können den Benutzern oder einer gesamten Gruppe die Lizenzierungsrechte hinzufügen.

### <a name="to-assign-a-license-to-a-user"></a>So weisen Sie einem Benutzer eine Lizenz zu

1. Wählen Sie auf der Seite **Produkte** den Namen des Lizenzplans aus, den Sie dem Benutzer zuweisen möchten.

    ![Dienstseite mit hervorgehobenem Dienstlizenzplan](media/license-users-groups/license-products-blade-with-product-highlight.png)

1. Wählen Sie auf der Übersichtsseite des Lizenzplans die Option **Zuweisen** aus.

    ![Dienstseite mit hervorgehobener Option „Zuweisen“](media/license-users-groups/license-products-blade-with-assign-option-highlight.png)

1. Wählen Sie auf der Seite **Zuweisen** **Benutzer und Gruppen**, und suchen und wählen Sie dann den Benutzer, dem Sie die Lizenz zuweisen.

    ![Seite „Lizenz hinzufügen“ mit den hervorgehobenen Optionen „Suchen“ und „Auswählen“](media/license-users-groups/assign-license-blade-with-highlight.png)

1. Wählen Sie **Zuordnungsoptionen**, stellen Sie sicher, dass die entsprechenden Lizenzoptionen aktiviert sind, und wählen Sie dann **OK**.

    ![Seite „Lizenzoptionen“ mit allen im Lizenzplan verfügbaren Optionen](media/license-users-groups/license-option-blade-assignments.png)

    Die Seite **Lizenz zuweisen** wird aktualisiert, um anzuzeigen, dass ein Benutzer ausgewählt ist und dass die Zuweisungen konfiguriert sind.

    > [!NOTE]
    > Nicht alle Microsoft-Dienste sind an allen Standorten verfügbar. Bevor einem Benutzer eine Lizenz zugewiesen werden kann, müssen Sie den **Nutzungsspeicherort** angeben. Sie können diesen Wert im Bereich **Azure Active Directory &gt; Benutzer &gt; Profil &gt; Einstellungen** in Azure AD festlegen. Jeder Benutzer, dessen Verwendungsstandort nicht angegeben ist, erbt den Standort der Azure AD-Organisation.

1. Wählen Sie **Zuweisen** aus.

    Der Benutzer wird der Liste der lizenzierten Benutzer hinzugefügt und hat Zugriff auf die enthaltenen Azure AD-Dienste.

### <a name="to-assign-a-license-to-a-group"></a>So weisen Sie einer Gruppe eine Lizenz zu

1. Wählen Sie auf der Seite **Produkte** den Namen des Lizenzplans aus, den Sie dem Benutzer zuweisen möchten.

    ![Blatt „Produkte“ mit hervorgehobenem Produktlizenzplan](media/license-users-groups/license-products-blade-with-product-highlight.png)

1. Wählen Sie auf der Seite **Azure Active Directory Premium Plan 2** die Option **Zuweisen**.

    ![Seite „Produkte“ mit hervorgehobener Option „Zuweisen“](media/license-users-groups/license-products-blade-with-assign-option-highlight.png)

1. Wählen Sie auf der Seite **Zuweisen** **Benutzer und Gruppen**, und suchen und wählen Sie dann die Gruppe, der Sie die Lizenz zuweisen.

    ![Seite „Lizenz hinzufügen“ mit den hervorgehobenen Optionen „Suchen“ und „Auswählen“](media/license-users-groups/assign-group-license-blade-with-highlight.png)

1. Wählen Sie **Zuordnungsoptionen**, stellen Sie sicher, dass die entsprechenden Lizenzoptionen aktiviert sind, und wählen Sie dann **OK**.

    ![Seite „Lizenzoptionen“ mit allen im Lizenzplan verfügbaren Optionen](media/license-users-groups/license-option-blade-group-assignments.png)

    Die Seite **Lizenz zuweisen** wird aktualisiert, um anzuzeigen, dass ein Benutzer ausgewählt ist und dass die Zuweisungen konfiguriert sind.

1. Wählen Sie **Zuweisen** aus.

    Die Gruppe wird der Liste der lizenzierten Gruppen hinzugefügt und alle Mitglieder haben Zugriff auf die enthaltenen Azure AD-Dienste.

## <a name="remove-a-license"></a>Entfernen einer Lizenz

Sie können eine Lizenz auf der Azure AD-Benutzerseite eines Benutzers und auf der Gruppenübersichtsseite für eine Gruppenzuweisung entfernen. Oder Sie beginnen auf der Azure AD-Seite **Lizenzen**, um die Benutzer und Gruppen für eine Lizenz anzuzeigen.

### <a name="to-remove-a-license-from-a-user"></a>So entfernen Sie eine Lizenz von einem Benutzer

1. Wählen Sie auf der Seite **Lizenzierte Benutzer** für den Dienstplan den Benutzer aus, der nicht mehr über die Lizenz verfügen soll. Beispielsweise _Alain Charon_.

1. Wählen Sie **Lizenz entfernen** aus.

    ![Seite „Lizenzierte Benutzer“ mit hervorgehobener Option „Lizenz entfernen“](media/license-users-groups/license-products-user-blade-with-remove-option-highlight.png)

> [!IMPORTANT]
> Lizenzen, die ein Benutzer von einer Gruppe erbt, können nicht direkt entfernt werden. Stattdessen müssen Sie den Benutzer aus der Gruppe entfernen, aus der er die Lizenz erbt.

### <a name="to-remove-a-license-from-a-group"></a>Entfernen einer Lizenz aus einer Gruppe

1. Wählen Sie auf der Seite **Lizenzierte Gruppen** für den Lizenzplan die Gruppe aus, die nicht mehr über die Lizenz verfügen soll.

1. Wählen Sie **Lizenz entfernen** aus.

    ![Seite „Lizenzierte Gruppe“ mit hervorgehobener Option „Lizenz entfernen“](media/license-users-groups/license-products-group-blade-with-remove-option-highlight.png)

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie Ihre Lizenzen zugewiesen haben, können Sie die folgenden Prozesse ausführen:

- [Bestimmen und Beheben von Lizenzzuweisungsproblemen](../users-groups-roles/licensing-groups-resolve-problems.md)

- [Hinzufügen von lizenzierten Benutzern zur Lizenzierung zu einer Gruppe](../users-groups-roles/licensing-groups-migrate-users.md)

- [Szenarien, Einschränkungen und bekannte Probleme mit der Verwendung von Gruppen zum Verwalten der Lizenzierung in Azure Active Directory](../users-groups-roles/licensing-group-advanced.md)

- [Hinzufügen oder Ändern von Profilinformationen](active-directory-users-profile-azure-portal.md)
