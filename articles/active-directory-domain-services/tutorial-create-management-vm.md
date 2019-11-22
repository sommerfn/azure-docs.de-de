---
title: 'Tutorial: Erstellen einer Verwaltungs-VM für Azure Active Directory Domain Services | Microsoft-Dokumentation'
description: In diesem Tutorial erfahren Sie, wie Sie eine Windows-VM erstellen und konfigurieren, die Sie zum Verwalten einer Azure Active Directory Domain Services-Instanz verwenden.
author: iainfoulds
manager: daveba
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: tutorial
ms.date: 10/30/2019
ms.author: iainfou
ms.openlocfilehash: 2aec10ab07b78aaacf34340b268f9b7dfbe69eb5
ms.sourcegitcommit: 98ce5583e376943aaa9773bf8efe0b324a55e58c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73172332"
---
# <a name="tutorial-create-a-management-vm-to-configure-and-administer-an-azure-active-directory-domain-services-managed-domain"></a>Tutorial: Erstellen einer Verwaltungs-VM zum Konfigurieren und Verwalten einer verwalteten Azure Active Directory Domain Services-Domäne

Azure Active Directory Domain Services (AD DS) stellt verwaltete Domänendienste wie Domänenbeitritt, Gruppenrichtlinie, LDAP und Kerberos-/NTLM-Authentifizierung bereit, die mit Windows Server Active Directory vollständig kompatibel sind. Sie verwalten diese verwaltete Domäne mit den gleichen Remoteserver-Verwaltungstools (Remote Server Administration Tools, RSAT) wie eine lokale Active Directory Domain Services-Domäne. Da Azure AD DS ein verwalteter Dienst ist, können Sie einige administrative Aufgaben nicht ausführen. Beispielsweise können Sie mit dem Remotedesktopprotokoll (RDP) keine Verbindung mit den Domänencontrollern (DCs) herstellen.

In diesem Tutorial wird gezeigt, wie Sie eine Windows Server-VM in Azure erstellen und die erforderlichen Tools zum Verwalten einer verwalteten Azure AD DS-Domäne installieren.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Grundlegendes zu den verfügbaren administrativen Aufgaben in einer verwalteten Azure AD DS-Domäne
> * Installieren der Active Directory-Verwaltungstools auf einer Windows Server-VM
> * Verwenden des Active Directory-Verwaltungscenters zum Ausführen allgemeiner Aufgaben

Wenn Sie kein Azure-Abonnement haben, [erstellen Sie ein Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie die folgenden Ressourcen und Berechtigungen:

* Ein aktives Azure-Abonnement.
    * Wenn Sie kein Azure-Abonnement besitzen, [erstellen Sie ein Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Einen mit Ihrem Abonnement verknüpften Azure Active Directory-Mandanten, der entweder mit einem lokalen Verzeichnis synchronisiert oder ein reines Cloudverzeichnis ist.
    * [Erstellen Sie einen Azure Active Directory-Mandanten][create-azure-ad-tenant], oder [verknüpfen Sie ein Azure-Abonnement mit Ihrem Konto][associate-azure-ad-tenant], sofern erforderlich.
* Eine verwaltete Azure Active Directory Domain Services-Domäne, die in Ihrem Azure AD-Mandanten aktiviert und konfiguriert ist.
    * Falls Sie keine solche Domäne haben, gehen Sie wie im ersten Tutorial beschrieben vor, um eine [Azure Active Directory Domain Services-Instanz zu erstellen und zu konfigurieren][create-azure-ad-ds-instance].
* Eine Windows Server-VM, die in die verwaltete Azure AD DS-Domäne eingebunden ist.
    * Falls Sie keine solche VM haben, gehen Sie wie im vorherigen Tutorial beschrieben vor, um eine [Windows Server-VM zu erstellen und in eine verwaltete Domäne einzubinden][create-join-windows-vm].
* Ein Benutzerkonto, das Mitglied der *Administratorengruppe für Azure AD-Domänencontroller* (AAD-DC-Administratoren) in Ihrem Azure AD-Mandanten ist.

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

In diesem Tutorial erstellen und konfigurieren Sie eine Verwaltungs-VM mithilfe des Azure-Portals. Melden Sie sich als Erstes beim [Azure-Portal](https://portal.azure.com) an.

## <a name="available-administrative-tasks-in-azure-ad-ds"></a>Verfügbare administrative Aufgaben in Azure AD DS

Azure AD DS stellt eine verwaltete Domäne bereit, die von Ihren Benutzern, Anwendungen und Diensten genutzt werden kann. Bei diesem Ansatz ändern sich einige der für Sie verfügbaren Verwaltungsaufgaben und die Berechtigungen, die Sie in der verwalteten Domäne haben. Diese Aufgaben und Berechtigungen unterscheiden sich möglicherweise von denen in einer normalen lokalen Active Directory Domain Services-Umgebung. Zudem ist es nicht möglich, über Remotedesktop eine Verbindung mit Domänencontrollern in der verwalteten Azure AD DS-Domäne herzustellen.

### <a name="administrative-tasks-you-can-perform-on-an-azure-ad-ds-managed-domain"></a>Administrative Aufgaben, die Sie in einer verwalteten Azure AD DS-Domäne ausführen können

Mitgliedern der Gruppe der *AAD-DC-Administratoren* werden Berechtigungen für die verwaltete Azure AD DS-Domäne gewährt, mit denen sie Aufgaben wie die folgenden ausführen können:

* Hinzufügen von Computern zur verwalteten Domäne
* Konfigurieren des integrierten Gruppenrichtlinienobjekts (Group Policy Object, GPO) für die Container *AADDC Computers* (Azure AD-DC-Computer) und *AADDC Users* (Azure AD-DC-Benutzer) in der verwalteten Domäne
* Verwalten von DNS in der verwalteten Domäne
* Erstellen und Verwalten benutzerdefinierter Organisationseinheiten (OEs) in der verwalteten Domäne
* Administrativer Zugriff auf Computer, die der verwalteten Domäne beigetreten sind

### <a name="administrative-privileges-you-dont-have-on-an-azure-ad-ds-managed-domain"></a>Administratorberechtigungen, die Sie in einer verwalteten Azure AD DS-Domäne nicht haben

Die verwaltete Azure AD DS-Domäne ist gesperrt, sodass Sie für bestimmte administrative Aufgaben in der Domäne keine Berechtigungen besitzen. Folgende Aufgaben können Sie beispielsweise nicht ausführen:

* Sie können das Schema der verwalteten Domäne nicht erweitern.
* Sie können keine Verbindung über Remotedesktop mit Domänencontrollern für die verwaltete Domäne herstellen.
* Sie können der verwalteten Domäne keine Domänencontroller hinzufügen.
* Sie besitzen keine *Domänenadministrator*- oder *Unternehmensadministrator*-Berechtigungen für die verwaltete Domäne.

## <a name="sign-in-to-the-windows-server-vm"></a>Anmelden bei der Windows-VM

Im vorherigen Tutorial wurde eine Windows Server-VM erstellt und in die verwaltete Azure AD DS-Domäne eingebunden. Diese VM verwenden Sie nun, um die Verwaltungstools zu installieren. Führen Sie ggf. die [Schritte im Tutorial zum Erstellen einer Windows Server-VM und Einbinden der VM in eine verwaltete Domäne][create-join-windows-vm] aus.

> [!NOTE]
> In diesem Tutorial verwenden Sie eine Windows Server-VM in Azure, die in die verwaltete Azure AD DS-Domäne eingebunden ist. Sie können auch einen in die verwaltete Domäne eingebundenen Windows-Client (z. B. Windows 10) verwenden.
>
> Weitere Informationen zum Installieren der Verwaltungstools auf einem Windows-Client finden Sie unter [Install Remote Server Administration Tools (RSAT)](https://social.technet.microsoft.com/wiki/contents/articles/2202.remote-server-administration-tools-rsat-for-windows-client-and-windows-server-dsforum2wiki.aspx) (Installieren der Remoteserver-Verwaltungstools (RSAT)).

Stellen Sie zunächst wie folgt eine Verbindung mit der Windows Server-VM her:

1. Wählen Sie im Azure-Portal auf der linken Seite **Ressourcengruppen** aus. Wählen Sie die Ressourcengruppe, in der Ihre VM erstellt wurde (z. B. *myResourceGroup*), und dann die VM (z. B. *myVM*) aus.
1. Wählen Sie im Fenster **Übersicht** der VM **Verbinden** aus.

    ![Herstellen einer Verbindung mit der Windows-VM im Azure-Portal](./media/tutorial-create-management-vm/connect-vm.png)

    Sie können auch einen [Azure Bastion-Host erstellen und verwenden (zurzeit als Vorschauversion verfügbar)][azure-bastion], um den Zugriff auf das Azure-Portal nur über SSL zuzulassen.

1. Wählen Sie die Option *RDP-Datei herunterladen* aus. Speichern Sie die RDP-Datei in Ihrem Webbrowser.
1. Öffnen Sie die heruntergeladene RDP-Datei, um eine Verbindung mit Ihrem virtuellen Computer herzustellen. Wenn Sie dazu aufgefordert werden, wählen Sie **Verbinden** aus.
1. Geben Sie die Anmeldeinformationen eines Benutzers ein, der Mitglied der Gruppe der *Azure AD DC-Administratoren* ist (z. B. *contoso\dee*).
1. Wenn während des Anmeldevorgangs eine Zertifikatwarnung angezeigt wird, klicken Sie auf **Ja** oder **Weiter**, um eine Verbindung herzustellen.

## <a name="install-active-directory-administrative-tools"></a>Installieren der Active Directory-Verwaltungstools

Verwaltete Azure AD DS-Domänen werden mit den gleichen Verwaltungstools verwaltet wie lokale AD DS-Umgebungen, beispielsweise mit dem Active Directory-Verwaltungscenter (Active Directory Administrative Center, ADAC) oder AD PowerShell. Diese Tools können als Teil des RSAT-Features auf Windows Server- und Windows-Clientcomputern installiert werden. Mitglieder der Gruppe der *AAD-DC-Administratoren* können verwaltete Azure AD DS-Domänen dann auf einem in die verwaltete Domäne eingebundenen Computer remote mit den AD-Verwaltungstools verwalten.

Führen Sie die folgenden Schritte aus, um die Active Directory-Verwaltungstools auf einer in die Domäne eingebundenen VM zu installieren:

1. Wenn **Server-Manager** bei der Anmeldung beim virtuellen Computer nicht standardmäßig geöffnet wird, wählen Sie das **Startmenü** und dann **Server-Manager** aus.
1. Wählen Sie im Bereich *Dashboard* des Fensters **Server-Manager** die Option **Rollen und Features hinzufügen** aus.
1. Klicken Sie auf der Seite **Vorbereitung** des *Assistenten zum Hinzufügen von Rollen und Features* auf **Weiter**.
1. Lassen Sie für *Installationstyp* die Option **Rollenbasierte oder featurebasierte Installation** aktiviert, und wählen Sie **Weiter** aus.
1. Wählen Sie auf der Seite **Serverauswahl** die aktuelle VM aus dem Serverpool aus (z. B. *myvm.contoso.com*), und klicken Sie dann auf **Weiter**.
1. Klicken Sie auf der Seite **Serverrollen** auf **Weiter**.
1. Erweitern Sie auf der Seite **Features** den Knoten **Remote Server-Verwaltungstools** und anschließend den Knoten **Rollenverwaltungstools**.

    Wählen Sie in der Liste mit den Rollenverwaltungstools das Feature **AD DS- und AD LDS-Tools** aus, und klicken Sie dann auf **Weiter**.

    ![Installieren von „AD DS- und AD LDS-Tools“ auf der Seite „Features“](./media/tutorial-create-management-vm/install-features.png)

1. Wählen Sie auf der Seite **Bestätigung** die Option **Installieren** aus. Die Installation der Verwaltungstools kann ein oder zwei Minuten dauern.
1. Wenn die Installation des Features abgeschlossen ist, wählen Sie **Schließen** aus, um den **Assistenten zum Hinzufügen von Rollen und Features** zu beenden.

## <a name="use-active-directory-administrative-tools"></a>Verwenden der Active Directory-Verwaltungstools

Nachdem Sie die Verwaltungstools installiert haben, sehen wir uns nun an, wie Sie die verwaltete Azure AD DS-Domäne damit verwalten. Stellen Sie sicher, dass Sie bei der VM mit einem Benutzerkonto angemeldet sind, das Mitglied der Gruppe der *AAD-DC-Administratoren* ist.

1. Wählen Sie im Startmenü die Option **Windows-Verwaltungsprogramme** aus **.** Die im vorherigen Schritt installierten AD-Verwaltungstools werden aufgelistet.

    ![Liste der auf dem Server installierten Verwaltungstools](./media/tutorial-create-management-vm/list-admin-tools.png)

1. Wählen Sie **Active Directory-Verwaltungscenter** aus.
1. Wählen Sie zum Durchsuchen der verwalteten Azure AD DS-Domäne im linken Bereich den Domänennamen aus (z. B. *contoso.com*). Am Anfang der Liste sehen Sie zwei Container mit den Namen *AADDC Computers* (Azure AD-DC-Computer) und *AADDC Users* (Azure AD-DC-Benutzer).

    ![Liste der verfügbaren Container, die Teil der verwalteten Azure AD DS-Domäne sind](./media/tutorial-create-management-vm/active-directory-administrative-center.png)

1. Wählen Sie den Container **AADDC Users** (Azure AD-DC-Benutzer) aus, um die Benutzer und Gruppen anzuzeigen, die der verwalteten Azure AD DS-Domäne angehören. In diesem Container werden die Benutzerkonten und -gruppen aus Ihrem Azure AD-Mandanten aufgelistet.

    In der folgenden Beispielausgabe werden in diesem Container ein Benutzerkonto namens *Contoso Admin* und eine Gruppe für *AAD DC-Administratoren* angezeigt.

    ![Anzeigen der Liste der Azure AD DS-Domänenbenutzer im Active Directory-Verwaltungscenter](./media/tutorial-create-management-vm/list-azure-ad-users.png)

1. Wählen Sie den Container **AADDC Computers** (Azure AD-DC-Computer) aus, um die Computer anzuzeigen, die in die verwaltete Azure AD DS-Domäne eingebunden sind. Ein Eintrag für die aktuelle VM (z. B. *myVM*) wird angezeigt. Computerkonten für alle in die verwaltete Azure AD DS-Domäne eingebundenen Computer werden im Container *AADDC Computers* (Azure AD-DC-Computer) gespeichert.

Allgemeine Aktionen im Active Directory-Verwaltungscenter wie das Zurücksetzen des Kennworts für ein Benutzerkonto oder Verwalten der Gruppenmitgliedschaft sind verfügbar. Diese Aktionen können nur für Benutzer und Gruppen ausgeführt werden, die direkt in der verwalteten Azure AD DS-Domäne erstellt wurden. Identitätsinformationen werden nur *aus* Azure AD mit Azure AD DS synchronisiert. Es werden keine Daten aus Azure AD DS in Azure AD zurückgeschrieben. Sie können Kennwörter oder die Mitgliedschaft in verwalteten Gruppen für Benutzer, die aus Azure AD synchronisiert wurden, nicht ändern und die Änderungen wieder mit Azure AD synchronisieren.

Für allgemeine Verwaltungsaktionen in Ihrer verwalteten Azure AD DS-Domäne können Sie auch das *Active Directory-Modul für Windows PowerShell* verwenden, das als Teil der Verwaltungstools installiert wird.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Grundlegendes zu den verfügbaren administrativen Aufgaben in einer verwalteten Azure AD DS-Domäne
> * Installieren der Active Directory-Verwaltungstools auf einer Windows Server-VM
> * Verwenden des Active Directory-Verwaltungscenters zum Ausführen allgemeiner Aufgaben

Aktivieren Sie zur sicheren Interaktion mit Ihrer verwalteten Azure AD DS-Domäne sicheres LDAP (LDAPS).

> [!div class="nextstepaction"]
> [Configure secure LDAP for your managed domain](tutorial-configure-ldaps.md) (Konfigurieren von sicherem LDAP für eine verwaltete Domäne)

<!-- INTERNAL LINKS -->
[create-azure-ad-tenant]: ../active-directory/fundamentals/sign-up-organization.md
[associate-azure-ad-tenant]: ../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md
[create-azure-ad-ds-instance]: tutorial-create-instance.md
[create-join-windows-vm]: join-windows-vm.md
[azure-bastion]: ../bastion/bastion-create-host-portal.md
