---
title: 'Tutorial: Erstellen Ihres ersten Zugriffspakets in der Azure AD-Berechtigungsverwaltung – Azure Active Directory'
description: Schritt-für-Schritt-Tutorial zum Erstellen Ihres ersten Zugriffspakets in der Azure Active Directory-Berechtigungsverwaltung
services: active-directory
documentationCenter: ''
author: msaburnley
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.subservice: compliance
ms.date: 10/22/2019
ms.author: ajburnle
ms.reviewer: markwahl-msft
ms.collection: M365-identity-device-management
ms.openlocfilehash: de4d4a1825149a512d7abdb192d8fb9d49e85a20
ms.sourcegitcommit: 98ce5583e376943aaa9773bf8efe0b324a55e58c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73174878"
---
# <a name="tutorial-create-your-first-access-package-in-azure-ad-entitlement-management"></a>Tutorial: Erstellen Ihres ersten Zugriffspakets in der Azure AD-Berechtigungsverwaltung

Das Verwalten des Zugriffs auf alle Ressourcen, die von Mitarbeitern benötigt werden, etwa Gruppen, Anwendungen und Websites, ist eine wichtige Aufgabe für Organisationen. Sie möchten Mitarbeitern den richtigen Umfang an Zugriff gewähren, den sie benötigen, um produktiv zu sein, und Sie möchten deren Zugriff entfernen, wenn er nicht mehr benötigt wird.

In diesem Tutorial arbeiten Sie als IT-Administrator für die Woodgrove Bank. Sie wurden gebeten, ein Paket von Ressourcen für eine Marketingkampagne zu erstellen, das interne Benutzern selbst anfordern können. Anforderungen bedürfen keiner Genehmigung, und der Zugriff eines Benutzer läuft nach 30 Tagen ab. Für dieses Tutorial gehören die Ressourcen der Marketingkampagne nur zu einer einzelnen Gruppe, sie könnten aber auch einer Sammlung von Gruppen, Anwendungen oder SharePoint Online-Websites angehören.

![Übersicht über das Szenario](./media/entitlement-management-access-package-first/elm-scenario-overview.png)

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines Zugriffspakets mit einer Gruppe als Ressource
> * Zulassen von Zugriffsanforderungen durch Benutzer in Ihrem Verzeichnis
> * Vorgehen, wie ein interner Benutzer das Zugriffspaket anfordern kann

Eine detaillierte Demonstration der Bereitstellung der Azure Active Directory-Berechtigungsverwaltung, einschließlich der Erstellung Ihres ersten Zugriffspakets, finden Sie in folgendem Video:

>[!VIDEO https://www.youtube.com/embed/zaaKvaaYwI4]

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Berechtigungsverwaltung verwenden zu können, benötigen Sie eine der folgenden Lizenzen:

- Azure AD Premium P2
- Enterprise Mobility + Security (EMS) E5-Lizenz

Weitere Informationen finden Sie unter [Lizenzanforderungen](entitlement-management-overview.md#license-requirements).

## <a name="step-1-set-up-users-and-group"></a>Schritt 1: Einrichten von Benutzern und Gruppen

Ein Ressourcenverzeichnis hat mindestens eine Ressource, die freigegeben (geteilt) werden soll. In diesem Schritt erstellen Sie eine Gruppe namens **Marketing resources** im Verzeichnis „Woodgrove Bank“ – der Zielressource für die Berechtigungsverwaltung. Außerdem richten Sie einen internen Anforderer ein.

**Erforderliche Rolle:** Globaler Administrator oder Benutzeradministrator

![Erstellen von Benutzern und Gruppen](./media/entitlement-management-access-package-first/elm-users-groups.png)

1. Melden Sie sich als globaler Administrator oder Benutzeradministrator beim [Azure-Portal](https://portal.azure.com) an.  

1. Klicken Sie im linken Navigationsmenü auf **Azure Active Directory**.

1. Erstellen Sie konfigurieren Sie die beiden folgenden Benutzer. Sie können diese Namen oder eine andere Namen verwenden. **Admin1** kann der Benutzer sein, als der Sie derzeit angemeldet sind.

    | NAME | Verzeichnisrolle |
    | --- | --- |
    | **Admin1** | Globaler Administrator<br/>Oder<br/>Benutzeradministrator |
    | **Anforderer1** | Benutzer |

1. Erstellen Sie eine Azure AD-Sicherheitsgruppe namens **Marketing resources** mit dem Mitgliedschaftstyp **Zugewiesen**.

    Diese Gruppe ist die Zielressource für die Berechtigungsverwaltung. Die Gruppe darf zu Beginn keine Mitglieder haben.

## <a name="step-2-create-an-access-package"></a>Schritt 2: Erstellen eines Zugriffspakets

Bei einem *Zugriffspaket* handelt es sich um eine Gruppe von Ressourcen, die von einem Team oder Projekt benötigt wird und Richtlinien unterliegt. Zugriffspakete sind in Containern definiert, die als *Kataloge* bezeichnet werden. In diesem Schritt erstellen Sie ein Zugriffspaket namens **Marketing Campaign** im Katalog **Allgemein**.

**Erforderliche Rolle:** Globaler Administrator, Benutzeradministrator, Katalogbesitzer oder Zugriffspaket-Manager

![Erstellen eines Zugriffspakets](./media/entitlement-management-access-package-first/elm-access-package.png)

1. Klicken Sie im Azure-Portal im linken Navigationsbereich auf **Azure Active Directory**.

1. Klicken Sie im linken Menü auf **Identity Governance**.

1. Klicken Sie im linken Menü auf **Zugriffspakete**.  Sollte **Zugriff verweigert** angezeigt werden, prüfen Sie, vergewissern Sie sich, dass in Ihrem Verzeichnis eine Azure AD Premium P2-Lizenz vorhanden ist.

1. Klicken Sie auf **Neues Zugriffspaket**.

    ![Berechtigungsverwaltung im Azure-Portal](./media/entitlement-management-shared/access-packages-list.png)

1. Geben Sie auf der Registerkarte **Grundeinstellungen** den Namen **Marketing Campaign** und die Beschreibung **Access to resources for the campaign** ein.

1. Übernehmen Sie für die Dropdownliste **Katalog** die Einstellung **Allgemein**.

    ![Neues Zugriffspaket, Registerkarte „Grundeinstellungen“](./media/entitlement-management-access-package-first/basics.png)

1. Klicken Sie auf **Weiter**, um die Registerkarte **Ressourcenrollen** zu öffnen.

    Wählen Sie auf dieser Registerkarte die Ressourcen und die Ressourcenrolle aus, die in das Zugriffspaket aufgenommen werden sollen.

1. Klicken Sie auf **Gruppen und Teams**.

1. Suchen Sie im Bereich „Gruppen auswählen“ nach der zuvor erstellten Gruppe **Marketing resources**, und wählen Sie sie aus.

    Standardmäßig werden Gruppen angezeigt, die sich innerhalb und außerhalb des Katalogs **Allgemein** befinden. Wenn Sie eine Gruppe auswählen, die sich außerhalb des Katalogs **Allgemein** befindet, wird sie dem Katalog **Allgemein** hinzugefügt.

    ![Neues Zugriffspaket, Registerkarte „Ressourcenrollen“](./media/entitlement-management-access-package-first/resource-roles-select-groups.png)

1. Klicken Sie auf **Auswählen**, um die Gruppe zur Liste hinzuzufügen.

1. Wählen Sie in der Dropdownliste **Rolle** den Eintrag **Mitglied** aus.

    ![Neues Zugriffspaket, Registerkarte „Ressourcenrollen“](./media/entitlement-management-access-package-first/resource-roles.png)

1. Klicken Sie auf **Weiter**, um die Registerkarte **Anforderungen** zu öffnen.

    Diese Registerkarte dient zum Erstellen einer Anforderungsrichtlinie. In einer *Richtlinie* sind die Regeln oder Leitlinien für den Zugriff auf ein Zugriffspaket definiert. Sie erstellen eine Richtlinie, die es einem bestimmten Benutzer im Ressourcenverzeichnis ermöglicht, dieses Zugriffspaket anzufordern.

1. Klicken Sie im Abschnitt **Benutzer, die Zugriff anfordern können** auf **Für in Ihrem Verzeichnis befindliche Benutzer** und anschließend auf **Bestimmte Benutzer und Gruppen**.

    ![Neues Zugriffspaket, Registerkarte „Anforderungen“](./media/entitlement-management-access-package-first/requests.png)

1. Klicken Sie auf **Benutzer und Gruppen hinzufügen**.

1. Wählen Sie im Bereich „Benutzer und Gruppen auswählen“ den zuvor erstellten Benutzer **Anforderer1** aus.

    ![Neues Zugriffspaket, Registerkarte „Anforderungen“: „Benutzer und Gruppen auswählen“](./media/entitlement-management-access-package-first/requests-select-users-groups.png)

1. Klicken Sie auf **Auswählen**.

1. Scrollen Sie nach unten zu den Abschnitten **Genehmigung** und **Enable requests** (Anforderungen aktivieren).

1. Behalten Sie für **Genehmigung erforderlich** die Einstellung **Nein** bei.

1. Klicken Sie für **Enable requests** (Anforderungen aktivieren) auf **Ja**, damit dieses Zugriffspaket angefordert werden kann, sobald es erstellt wurde.

    ![Neues Zugriffspaket, Registerkarte „Anforderungen“: „Genehmigung“ und „Enable requests“ (Anforderungen aktivieren)](./media/entitlement-management-access-package-first/requests-approval-enable.png)

1. Klicken Sie auf **Weiter**, um die Registerkarte **Lebenszyklus** zu öffnen.

1. Legen Sie im Abschnitt **Ablauf** die Option **Die Zugriffspaketzuweisungen laufen ab.** auf **Anzahl Tage** fest.

1. Legen Sie **Zuweisungen laufen ab nach:** auf **30** Tage fest.

    ![Neues Zugriffspaket, Registerkarte „Lebenszyklus“](./media/entitlement-management-access-package-first/lifecycle.png)

1. Klicken Sie auf **Weiter**, um die Registerkarte **Überprüfen + erstellen** zu öffnen.

    ![Neues Zugriffspaket, Registerkarte „Überprüfen + erstellen“](./media/entitlement-management-access-package-first/review-create.png)

    Nach einigen Augenblicken sollte eine Benachrichtigung angezeigt werden, dass das Zugriffspaket erfolgreich erstellt wurde.

1. Klicken Sie im linken Menü des Zugriffspakets „Marketing Campaign“ auf **Übersicht**.

1. Kopieren Sie den Wert von **Link zum Portal "Mein Zugriff"** .

    Sie verwenden diesen Link im nächsten Schritt.

    ![Zugriffspaket (Übersicht): Link zum Portal „Mein Zugriff“](./media/entitlement-management-shared/my-access-portal-link.png)

## <a name="step-3-request-access"></a>Schritt 3: Zugriff anfordern

In diesem Schritt führen Sie die Schritte als **interner Anforderer** aus und fordern Zugriff auf das Paket an. Anforderer senden ihre Anforderungen über eine Website, die als Portal „Eigener Zugriff“ (Mein Zugriff) bezeichnet wird. Über das Portal „Mein Zugriff“ können Anforderer Anforderungen für Zugriffspakete senden, die Zugriffspakete sehen, auf die sie bereits Zugriff haben, und ihre Anforderungsverläufe anzeigen.

**Erforderliche Rolle:** Interner Anforderer

1. Melden Sie sich vom Azure-Portal ab.

1. Navigieren Sie in einem neuen Browserfenster zu dem „Link zum Portal "Mein Zugriff"“, den Sie im vorherigen Schritt kopiert haben.

1. Melden Sie beim Portal „Mein Zugriff“ als **Anforderer1** an.

    Das Zugriffspaket **Marketing Campaign** sollte angezeigt werden.

1. Klicken Sie ggf. in der Spalte **Beschreibung** auf den Pfeil, um die Details zu dem Zugriffspaket anzuzeigen.

    ![Portal „Mein Zugriff“ – Zugriffspakete](./media/entitlement-management-shared/my-access-access-packages.png)

1. Klicken Sie auf das Häkchen, um das Paket auszuwählen.

1. Klicken Sie auf **Zugriff anfordern**, um den Bereich „Zugriff anfordern“ zu öffnen.

    ![Portal „Mein Zugriff“, Schaltfläche „Zugriff anfordern“](./media/entitlement-management-access-package-first/my-access-request-access-button.png)

1. Geben Sie in das Feld **Geschäftliche Begründung** die Begründung **I am working on the new marketing campaign** ein.

    ![Portal „Mein Zugriff“ – Zugriff anfordern](./media/entitlement-management-shared/my-access-request-access.png)

1. Klicken Sie auf **Submit**.

1. Klicken Sie im linken Menü auf **Anforderungsverlauf**, um zu überprüfen, ob Ihre Anforderung übermittelt wurde.

## <a name="step-4-validate-that-access-has-been-assigned"></a>Schritt 4: Überprüfen, ob der Zugriff zugewiesen wurde

In diesem Schritt vergewissern Sie sich, dass das Zugriffspaket dem **internen Anforderer** zugewiesen wurde und er nun der Gruppe **Marketing resources** angehört.

**Erforderliche Rolle:** Globaler Administrator, Benutzeradministrator, Katalogbesitzer oder Zugriffspaket-Manager

1. Melden Sie sich vom Portal „Mein Zugriff“ ab.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) als **Admin1** an.

1. Klicken Sie auf **Azure Active Directory**, und klicken Sie dann auf **Identity Governance**.

1. Klicken Sie im linken Menü auf **Zugriffspakete**.

1. Suchen Sie nach dem Zugriffspaket **Marketing Campaign**, und klicken Sie darauf.

1. Klicken Sie im linken Menü auf **Anforderungen**.

    „Anforderer1“, die anfängliche Richtlinie und der Status **Übermittelt** sollten angezeigt werden.

1. Klicken Sie auf die Anforderung, um die Anforderungsdetails anzuzeigen.

    ![Zugriffspaket – Anforderungsdetails](./media/entitlement-management-access-package-first/request-details.png)

1. Klicken Sie im linken Navigationsmenü auf **Azure Active Directory**.

1. Klicken Sie auf **Gruppen**, und öffnen Sie die Gruppe **Marketing resources**.

1. Klicken Sie auf **Mitglieder**.

    Sie sollten sehen, dass **Anforderer1** als Mitglied (Member) aufgelistet wird.

    ![Mitglieder von „Marketing resources“](./media/entitlement-management-access-package-first/group-members.png)

## <a name="step-5-clean-up-resources"></a>Schritt 5: Bereinigen von Ressourcen

In diesem Schritt werden die von Ihnen vorgenommenen Änderungen entfernt, und das Zugriffspaket **Marketing Campaign** wird gelöscht.

**Erforderliche Rolle:**  Globaler Administrator oder Benutzeradministrator

1. Klicken Sie im Azure-Portal auf **Azure Active Directory** und dann auf **Identity Governance**.

1. Öffnen Sie das Zugriffspaket **Marketing Campaign**.

1. Klicken Sie auf **Zuweisungen:** .

1. Klicken Sie für **Anforderer1** auf die Auslassungspunkte ( **...** ), und klicken Sie dann auf **Zugriff entfernen**. Klicken Sie in der angezeigten Meldung auf **Ja**.

    Kurz darauf ändert sich der Status von „Übermittelt“ in „Abgelaufen“.

1. Klicken Sie auf **Ressourcenrollen**.

1. Klicken Sie für **Marketing resources** auf die Auslassungspunkte ( **...** ) und anschließend auf **Ressourcenrolle entfernen**. Klicken Sie in der angezeigten Meldung auf **Ja**.

1. Öffnen Sie die Liste der Zugriffspakete.

1. Klicken Sie für **Marketing Campaign** auf die Auslassungspunkte ( **...** ) und anschließend auf **Löschen**. Klicken Sie in der angezeigten Meldung auf **Ja**.

1. Löschen Sie in Azure Active Directory alle Benutzer, die Sie erstellt, z. B. **Anforderer1** und **Admin1**.

1. Löschen Sie die Gruppe **Marketing resources**.

## <a name="next-steps"></a>Nächste Schritte

Wechseln Sie zum nächsten Artikel, um mehr über gängige Szenarioschritte in der Berechtigungsverwaltung zu erfahren.
> [!div class="nextstepaction"]
> [Gängige Szenarios](entitlement-management-scenarios.md)
