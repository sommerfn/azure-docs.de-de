---
title: Bereitstellen der Azure-Zugriffsbereichserweiterung für IE mithilfe eines Gruppenrichtlinienobjekts | Microsoft-Dokumentation
description: So stellen Sie das Internet Explorer-Add-On für das Portal "Meine Apps" mithilfe von Gruppenrichtlinien bereit
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/08/2018
ms.author: mimart
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: 71c342ede77349b3f6c22093e5877ad5f5ce6549
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67807687"
---
# <a name="how-to-deploy-the-access-panel-extension-for-internet-explorer-using-group-policy"></a>Gewusst wie: Bereitstellen der Zugriffsbereichserweiterung für Internet Explorer mit der Gruppenrichtlinie

In diesem Tutorial wird erläutert, wie Sie mithilfe von Gruppenrichtlinien die Zugriffsbereichserweiterung für Internet Explorer per Remotezugriff auf den Computern Ihrer Benutzer installieren. Diese Erweiterung ist für Benutzer von Internet Explorer erforderlich, die sich bei Apps anmelden müssen, die mit der [kennwortbasierten einmaligen Anmeldung](what-is-single-sign-on.md#password-based-sso)konfiguriert wurden.

Administratoren wird empfohlen, die Bereitstellung dieser Erweiterung zu automatisieren. Andernfalls müssen Benutzer die Erweiterung selbst herunterladen und installieren. Diese Vorgehensweise ist jedoch fehleranfällig; zudem sind Administratorberechtigungen erforderlich. In diesem Tutorial wird eine Methode zum Automatisieren von Softwarebereitstellungen mithilfe von Gruppenrichtlinien erläutert. [Weitere Informationen zu Gruppenrichtlinien](https://technet.microsoft.com/windowsserver/bb310732.aspx)

Die Zugriffsbereichserweiterung ist auch für [Chrome](https://go.microsoft.com/fwLink/?LinkID=311859) und [Firefox](https://go.microsoft.com/fwLink/?LinkID=626998) verfügbar. Bei diesen Browser sind für die Installation keine Administratorberechtigungen erforderlich.

## <a name="prerequisites"></a>Voraussetzungen

* Sie haben [Active Directory-Domänendienste](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx)eingerichtet und die Computer Ihrer Benutzer Ihrer Domäne hinzugefügt.
* Zum Bearbeiten des Gruppenrichtlinienobjekts benötigen Sie die Berechtigung „Einstellungen bearbeiten“. Standardmäßig verfügen Mitglieder der folgenden Sicherheitsgruppen über diese Berechtigung: Domänenadministratoren, Organisationsadministratoren und Ersteller/Besitzer von Gruppenrichtlinien. [Weitere Informationen.](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx)

## <a name="step-1-create-the-distribution-point"></a>Schritt 1: Erstellen des Verteilungspunkts

Zunächst müssen Sie das Installationspaket an einem Speicherort im Netzwerk ablegen, auf den über die Computer zugegriffen werden kann, auf denen Sie die Erweiterung per Remotezugriff installieren möchten. Gehen Sie dazu folgendermaßen vor:

1. Melden Sie sich beim Server als Administrator an.
1. Wechseln Sie im Fenster **Server-Manager** zu **Dateien und Speicherdienste**.

    ![Dateien und Speicherdienste öffnen](./media/deploy-access-panel-browser-extension/files-services.png)

1. Klicken Sie auf die Registerkarte **Freigaben** . Klicken Sie dann auf **Aufgaben** > **Neue Freigabe**.

    ![Screenshot der Option „Neue Freigabe“ auf dem Bildschirm „Aufgaben“](./media/deploy-access-panel-browser-extension/shares.png)

1. Schließen Sie den **Assistenten für neue Freigaben** , und legen Sie Berechtigungen fest, um sicherzustellen, dass von den Computern Ihrer Benutzer aus darauf zugegriffen werden kann. [Weitere Informationen zu Freigaben](https://technet.microsoft.com/library/cc753175.aspx)
1. Laden Sie das folgende Microsoft Windows Installer-Paket (MSI-Datei) herunter: [Access Panel Extension.msi](https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access%20Panel%20Extension.msi)
1. Kopieren Sie das Installationspaket an den gewünschten Speicherort auf der Freigabe.

    ![Kopieren Sie die .msi-Datei in die Freigabe](./media/deploy-access-panel-browser-extension/copy-package.png)

1. Stellen Sie sicher, dass von den Clientcomputern aus auf das Installationspaket auf der Freigabe zugegriffen werden kann.

## <a name="step-2-create-the-group-policy-object"></a>Schritt 2: Erstellen des Gruppenrichtlinienobjekts

1. Melden Sie sich bei dem Server mit den Active Directory-Domänendiensten (AD DS) an.
1. Navigieren Sie im Server-Manager zu **Extras** > **Gruppenrichtlinienverwaltung**.

    ![Navigieren Sie zu „Tools“ > „Gruppenrichtlinienverwaltung“.](./media/deploy-access-panel-browser-extension/tools-gpm.png)

1. Zeigen Sie im linken Bereich des Fensters **Gruppenrichtlinienverwaltung** die Hierarchie Ihrer Organisationseinheiten (OE) an, und legen Sie fest, in welchem Umfang die Gruppenrichtlinie angewendet werden soll. Sie können beispielsweise eine kleine Organisationseinheit auswählen, die einigen wenigen Benutzern zu Testzwecken bereitgestellt wird, oder eine Organisationseinheit der obersten Ebene festlegen, die im gesamten Unternehmen bereitgestellt wird.

   > [!NOTE]
   > Wenn Sie Organisationseinheiten erstellen oder bearbeiten möchten, wechseln Sie zurück zum Server-Manager, und wählen Sie **Extras** > **Active Directory-Benutzer und -Computer** aus.

1. Nachdem Sie eine Organisationseinheit ausgewählt haben, klicken Sie mit der rechten Maustaste darauf, und wählen Sie **Gruppenrichtlinienobjekt hier erstellen und verknüpfen** aus.

    ![Der Screenshot zeigt Ihnen die Option zur Erstellung eines neuen GPOs](./media/deploy-access-panel-browser-extension/create-gpo.png)

1. Geben Sie in der Eingabeaufforderung **Neues Gruppenrichtlinienobjekt** einen Namen für das neue Gruppenrichtlinienobjekt ein.
1. Klicken Sie mit der rechten Maustaste auf das Gruppenrichtlinienobjekt, das Sie gerade erstellt haben, und wählen Sie **Bearbeiten** aus.

## <a name="step-3-assign-the-installation-package"></a>Schritt 3: Zuweisen des Installationspakets

1. Legen Sie fest, ob Sie die Erweiterung basierend auf der **Computerkonfiguration** oder basierend auf der **Benutzerkonfiguration** bereitstellen möchten. Bei Verwendung der [Computerkonfiguration](https://technet.microsoft.com/library/cc736413%28v=ws.10%29.aspx) wird die Erweiterung auf dem Computer installiert, und zwar unabhängig davon, welche Benutzer sich bei ihm anmelden. Bei Verwendung der [Benutzerkonfiguration](https://technet.microsoft.com/library/cc781953%28v=ws.10%29.aspx) wird die Erweiterung für die Benutzer installiert, und zwar unabhängig davon, bei welchem Computer sie sich anmelden.
1. Folgen Sie abhängig vom gewählten Konfigurationstyp im linken Bereich des Fensters **Gruppenrichtlinienverwaltungs-Editor** einem der folgenden Ordnerpfade:

   * `Computer Configuration/Policies/Software Settings/`
   * `User Configuration/Policies/Software Settings/`

1. Klicken Sie mit der rechten Maustaste auf **Softwareinstallation**, und wählen Sie dann **Neu** > **Paket** aus.
1. Wechseln Sie zum freigegebenen Ordner, der das Installerpaket aus [Schritt 1: Erstellen des Verteilungspunkts](#step-1-create-the-distribution-point) enthält, wählen Sie die MSI-Datei aus, und klicken Sie auf **Öffnen**.

   > [!IMPORTANT]
   > Wenn sich die Freigabe auf demselben Server befindet, vergewissern Sie sich, dass Sie nicht über den lokalen Dateipfad, sondern über den Netzwerkdateipfad auf die MSI-Datei zugreifen.

    ![Auswählen des Installationspakets im freigegebenen Ordner](./media/deploy-access-panel-browser-extension/select-package.png)

1. Wählen Sie in der Eingabeaufforderung **Software bereitstellen** als Bereitstellungsmethode die Option **Zugewiesen** aus. Klicken Sie dann auf **OK**.

Die Erweiterung wird nun für die ausgewählte Organisationseinheit bereitgestellt. [Weitere Informationen zur Gruppenrichtlinien-Softwareinstallation](https://technet.microsoft.com/library/cc738858%28v=ws.10%29.aspx)

## <a name="step-4-auto-enable-the-extension-for-internet-explorer"></a>Schritt 4: Automatisches Aktivieren der Erweiterung für Internet Explorer

Zusätzlich zur Ausführung des Installationsprogramms muss jede Erweiterung für Internet Explorer vor ihrer Verwendung explizit aktiviert werden. Gehen Sie folgendermaßen vor, um die Zugriffsbereichserweiterung mithilfe von Gruppenrichtlinien zu aktivieren:

1. Navigieren Sie im Fenster **Gruppenrichtlinienverwaltungs-Editor** zu einem der folgenden Ordnerpfade, je nachdem, welchen Konfigurationstyp Sie in [Schritt 3: Zuweisen des Installationspakets](#step-3-assign-the-installation-package) ausgewählt haben:

   * `Computer Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/Security Features/Add-on Management`
   * `User Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/Security Features/Add-on Management`

1. Klicken Sie mit der rechten Maustaste auf **Add-On-Liste**, und wählen Sie **Bearbeiten** aus.

    ![Klicken Sie mit der rechten Maustaste auf „Add-On-Liste“, und wählen Sie „Bearbeiten“ aus.](./media/deploy-access-panel-browser-extension/edit-add-on-list.png)

1. Wählen Sie im Fenster **Add-On-Liste** die Option **Aktiviert** aus. Klicken Sie im Abschnitt **Optionen** auf **Anzeigen...** .

    ![Klicken Sie auf „Aktivieren“ und anschließend auf „Anzeigen...“.](./media/deploy-access-panel-browser-extension/edit-add-on-list-window.png)

1. Führen Sie im Fenster **Inhalt anzeigen** die folgenden Schritte aus:

   1. Kopieren Sie für die erste Spalte (Feld **Wertname**) die folgende Klassen-ID: `{030E9A3F-7B18-4122-9A60-B87235E4F59E}`.
   1. Geben Sie für die zweite Spalte (Feld **Wert**) den folgenden Wert ein: `1`.
   1. Klicken Sie auf **OK**, um das Fenster **Inhalt anzeigen** zu schließen.

      ![Füllen Sie die Werte wie im vorherigen Schritt angegeben aus.](./media/deploy-access-panel-browser-extension/show-contents.png)

1. Klicken Sie auf **OK**, um die Änderungen zu übernehmen und das Fenster **Add-On-Liste** zu schließen.

Die Erweiterung sollte jetzt für die Computer in der ausgewählten Organisationseinheit aktiviert sein. [Erfahren Sie mehr über die Verwendung von Gruppenrichtlinien zum Aktivieren oder Deaktivieren von Add-Ons für Internet Explorer.](https://technet.microsoft.com/library/dn454941.aspx)

## <a name="step-5-optional-disable-remember-password-prompt"></a>Schritt 5 (Optional): Deaktivieren der Frage nach der Kennwortspeicherung

Wenn sich Benutzer unter Verwendung der Zugriffsbereichserweiterung bei Websites anmelden, kann Internet Explorer fragen, ob der Benutzer sein Kennwort speichern möchte.

![Zeigt Ihnen die „Möchten Sie Ihr Kennwort speichern?“-Eingabeaufforderung an.](./media/deploy-access-panel-browser-extension/remember-password-prompt.png)

Falls diese Frage nicht angezeigt werden soll, führen Sie die folgenden Schritte aus, um das Speichern von Kennwörtern durch AutoVervollständigen zu unterbinden:

1. Navigieren Sie im **Gruppenrichtlinienverwaltungs-Editor** zum unten angegebenen Pfad. Diese Konfigurationseinstellung ist nur unter **Benutzerkonfiguration** verfügbar.

   * `User Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/`
1. Suchen Sie die Einstellung **AutoVervollständigen für Benutzernamen und Kennwörter in Formularen aktivieren**.

   > [!NOTE]
   > In älteren Versionen von Active Directory heißt diese Einstellung unter Umständen noch **Kennwörter in AutoVervollständigen können nicht gespeichert werden**. Die Konfiguration für diese Einstellung unterscheidet sich von der in diesem Tutorial beschriebenen Einstellung.

    ![Diese finden Sie in den Benutzereinstellungen.](./media/deploy-access-panel-browser-extension/disable-auto-complete.png)

1. Klicken Sie mit der rechten Maustaste auf die oben genannte Einstellung, und wählen Sie **Bearbeiten** aus.
1. Wählen Sie im Fenster **AutoVervollständigen für Benutzernamen und Kennwörter in Formularen aktivieren** die Option **Deaktiviert** aus.

    ![Wählen Sie „Deaktiviert“ für AutoVervollständigen aus.](./media/deploy-access-panel-browser-extension/disable-passwords.png)

1. Klicken Sie auf **OK** , um die Änderungen zu übernehmen und das Fenster zu schließen.

Daraufhin können Benutzer ihre Anmeldeinformationen nicht mehr speichern oder mithilfe von AutoVervollständigen auf zuvor gespeicherte Anmeldeinformationen zugreifen. Für andere Arten von Formularfeldern (etwa Suchfelder) kann AutoVervollständigen dagegen weiter verwendet werden.

> [!WARNING]
> Wenn diese Richtlinie aktiviert wird, nachdem Benutzer bereits Anmeldeinformationen gespeichert haben, werden diese *nicht* gelöscht.

## <a name="step-6-testing-the-deployment"></a>Schritt 6: Testen der Bereitstellung

Gehen Sie folgendermaßen vor, um zu überprüfen, ob die Erweiterungsbereitstellung erfolgreich war:

1. Wenn Sie für die Bereitstellung die Option **Computerkonfiguration**verwendet haben, melden Sie sich bei einem Clientcomputer an, der zu der in [Schritt 2: Erstellen des Gruppenrichtlinienobjekts](#step-2-create-the-group-policy-object) ausgewählten Organisationseinheit gehört. Falls die Bereitstellung mit **Benutzerkonfiguration**erfolgt ist, melden Sie sich als Benutzer an, der zu dieser Organisationseinheit gehört.
1. Möglicherweise werden die Gruppenrichtlinienänderungen auf dem Computer erst durch mehrmaliges Anmelden vollständig aktualisiert. Öffnen Sie zum Erzwingen der Aktualisierung ein Fenster für die **Eingabeaufforderung**, und führen Sie den folgenden Befehl aus: `gpupdate /force`.
1. Der Computer muss neu gestartet werden, damit die Installation ausgeführt werden kann. Das Starten kann während der Installation der Erweiterung erheblich länger dauern als gewöhnlich.
1. Öffnen Sie nach dem Neustart **Internet Explorer**. Klicken Sie in der rechten oberen Ecke des Fensters auf **Extras** (das Zahnradsymbol), und wählen Sie dann **Add-Ons verwalten** aus.
1. Überprüfen Sie im Fenster **Add-Ons verwalten**, ob die **Zugriffsbereichserweiterung** installiert und ihr **Status** auf **Aktiviert** festgelegt wurde.

   ![Überprüfen von Installation und Aktivierung der Zugriffsbereichserweiterung](./media/deploy-access-panel-browser-extension/verify-install.png)

## <a name="learn-more"></a>Weitere Informationen

* [Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory](what-is-single-sign-on.md)
* [Problembehandlung in der Zugriffsbereichserweiterung für Internet Explorer](manage-access-panel-browser-extension.md)
