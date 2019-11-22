---
title: Erstellen von Arbeitsbereichen für das Portal „Meine Apps“ in Azure Active Directory | Microsoft-Dokumentation
description: Verwenden Sie die Arbeitsbereiche von „Meine Apps“, um die Seiten von „Meine Apps“ anzupassen und die „Meine Apps“-Umgebung für Ihre Endbenutzer zu vereinfachen. Fassen Sie Anwendungen in Gruppen mit separaten Registerkarten zusammen.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/29/2019
ms.author: mimart
ms.reviewer: kasimpso
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5a986c2032fc41bbd34230c5b0ead745f88b000f
ms.sourcegitcommit: fa5ce8924930f56bcac17f6c2a359c1a5b9660c9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2019
ms.locfileid: "73199826"
---
# <a name="create-workspaces-on-the-my-apps-preview-portal"></a>Erstellen von Arbeitsbereichen im Portal „Meine Apps“ (Vorschauversion)

Ihre Benutzer können das Portal „Meine Apps“ (Vorschau) verwenden, um die cloudbasierten Anwendungen, auf die sie zugreifen können, anzuzeigen und zu starten. Standardmäßig werden alle Anwendungen, auf die ein Benutzer zugreifen kann, auf einer einzigen Seite aufgelistet. Wenn Sie über eine Azure AD Premium P1- oder P2-Lizenz verfügen, können Sie Arbeitsbereiche einrichten, um diese Seite für Ihre Benutzer übersichtlicher zu gestalten. Mit einem Arbeitsbereich können Sie verwandte Anwendungen gruppieren (z.B. nach Auftragsrolle, Aufgabe oder Projekt) und auf separaten Registerkarten anzeigen. Ein Arbeitsbereich wendet im Wesentlichen einen Filter auf die Anwendungen an, auf die ein Benutzer bereits zugreifen kann. Dem Benutzer werden somit nur die ihm zugewiesenen Anwendungen im Arbeitsbereich angezeigt.

> [!NOTE]
> In diesem Artikel wird beschrieben, wie ein Administrator Arbeitsbereiche aktivieren und erstellen kann. Informationen für Endbenutzer zur Verwendung des Portals „Meine Apps“ und der Arbeitsbereiche finden Sie unter [Zugreifen auf und Verwenden von Arbeitsbereichen](https://docs.microsoft.com/azure/active-directory/user-help/my-applications-portal-workspaces).

## <a name="enable-my-apps-preview-features"></a>Aktivieren von „Meine Apps“-Previewfunktionen

1. Öffnen Sie das [**Azure-Portal**](https://portal.azure.com/), und melden Sie sich als Benutzeradministrator oder globaler Administrator an.

2. Navigieren Sie zu **Azure Active Directory** > **Benutzereinstellungen**.

3. Wählen Sie unter **Vorschauen für Benutzerfeatures** die Option **Vorschaueinstellungen für Benutzerfeatures verwalten** aus.

4. Wählen Sie unter **Benutzer können Previewfunktionen für „Meine Apps“ verwenden**, eine der folgenden Optionen aus:
   * **Ausgewählt**: Aktiviert Previewfunktionen für eine bestimmte Gruppe. Verwenden Sie die Option **Eine Gruppe auswählen**, um die Gruppe auszuwählen, für die Sie Previewfunktionen aktivieren möchten.  
   * **Alle**: Aktiviert Previewfunktionen für alle Benutzer.

   ![Previewfunktionen für Benutzer](media/access-panel-workspaces/user-preview-features.png)

> [!NOTE]
> Benutzer können das Portal „Meine Apps“ über den Link `https://myapps.microsoft.com` oder den angepassten Link für Ihre Organisation (z.B. `https://myapps.microsoft.com/contoso.com`) öffnen. Wenn Benutzer nicht zur Vorschauversion von „Meine Apps“ weitergeleitet werden, sollten sie `https://myapplications.microsoft.com` oder `https://myapplications.microsoft.com/contoso.com` ausprobieren.

## <a name="create-a-workspace"></a>Erstellen eines Arbeitsbereichs

Zum Erstellen eines Arbeitsbereichs müssen Sie über eine Azure AD Premium P1- oder P2-Lizenz verfügen.

1. Öffnen Sie das [**Azure-Portal**](https://portal.azure.com/), und melden Sie sich als Administrator mit einer Azure AD Premium P1- oder P2-Lizenz an.

2. Wechseln Sie zu **Azure Active Directory** > **Unternehmensanwendungen**.

3. Wählen Sie unter **Verwalten** die Option **Arbeitsbereiche (Vorschau)** aus.

4. Wählen Sie **Neuer Arbeitsbereich** aus. Geben Sie auf der Seite **Neuer Arbeitsbereich** einen **Namen** für den Arbeitsbereich ein. (Es wird empfohlen, im Namen nicht die Zeichenfolge „Arbeitsbereich“ zu verwenden.) Geben Sie dann eine **Beschreibung** ein.

   ![Erstellen eines neuen Arbeitsbereichs](media/access-panel-workspaces/new-workspace.png)

5. Klicken Sie auf **Überprüfen + erstellen**. Die Eigenschaften für den neuen Arbeitsbereich werden angezeigt.

6. Wählen Sie die Registerkarte **Anwendungen** aus. Wählen Sie unter **Anwendungen hinzufügen** alle Anwendungen aus, die Sie dem Arbeitsbereich hinzufügen möchten, oder verwenden Sie das Feld **Suchen**, um nach Anwendungen zu suchen. 

   ![Hinzufügen von Anwendungen zum Arbeitsbereich](media/access-panel-workspaces/add-applications.png)

7. Wählen Sie **Hinzufügen**. Die Liste der ausgewählten Anwendungen wird angezeigt. Mithilfe der Aufwärts- und Abwärtspfeile können Sie die Reihenfolge der Anwendungen in der Liste ändern.

   ![Liste der Anwendungen im Arbeitsbereich](media/access-panel-workspaces/add-applications-list.png)

8. Wählen Sie die Registerkarte **Benutzer und Gruppen**. Wählen Sie **Benutzer hinzufügen**, um einen Benutzer oder eine Gruppe hinzuzufügen. 

9. Wählen Sie auf der Seite **Mitglieder auswählen** die Benutzer oder Gruppen aus, denen Sie den Arbeitsbereich zuweisen möchten. Oder verwenden Sie das Feld **Suchen**, um nach Benutzern oder Gruppen zu suchen.

   ![Zuweisen von Benutzern und Gruppen zum Arbeitsbereich](media/access-panel-workspaces/add-users-and-groups.png)

10. Wenn Sie mit der Auswahl der Benutzer und Gruppen fertig sind, wählen Sie **Auswählen** aus.

11. Um die Rolle eines Benutzers von **Lesezugriffs** auf **Besitzer** zu ändern oder umgekehrt, klicken Sie auf die aktuelle Rolle, und wählen Sie eine neue Rolle aus.

    ![Zuweisen von Rollen zu Benutzern und Gruppen](media/access-panel-workspaces/users-groups-list-role.png)

## <a name="view-audit-logs"></a>Anzeigen von Überwachungsprotokollen

In den Überwachungsprotokollen werden Vorgänge des Arbeitsbereichs „Meine Apps“ aufgezeichnet, einschließlich Endbenutzeraktionen der Arbeitsbereichserstellung. Die folgenden Ereignisse werden von „Meine Apps“ generiert:

* Arbeitsbereich erstellen
* Arbeitsbereich bearbeiten
* Arbeitsbereich löschen
* Anwendung starten (Endbenutzer)
* Self-Service-Hinzufügung von Anwendungen (Endbenutzer)
* Self-Service-Löschung von Anwendungen (Endbenutzer)

Sie können im [Azure-Portal](https://portal.azure.com) auf die Überwachungsprotokolle zugreifen. Wählen Sie dazu **Azure Active Directory** > **Unternehmensanwendungen** > **Überwachungsprotokolle** im Abschnitt „Aktivität“ aus. Wählen Sie **Meine Apps** für **Dienst** aus.

   ![Zuweisen von Rollen zu Benutzern und Gruppen](media/access-panel-workspaces/audit-log-myapps.png)


## <a name="next-steps"></a>Nächste Schritte
[Endbenutzerumgebungen für Anwendungen in Azure Active Directory](end-user-experiences.md)