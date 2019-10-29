---
title: 'Tutorial: Erstellen einer Azure Active Directory Domain Services-Instanz | Microsoft-Dokumentation'
description: In diesem Tutorial erfahren Sie, wie Sie im Azure-Portal eine Azure Active Directory Domain Services-Instanz erstellen und konfigurieren.
author: iainfoulds
manager: daveba
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: tutorial
ms.date: 10/18/2019
ms.author: iainfou
ms.openlocfilehash: b99eafeae60e81fd7d902289a47190a2cbe1daa3
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72786987"
---
# <a name="tutorial-create-and-configure-an-azure-active-directory-domain-services-instance"></a>Tutorial: Erstellen und Konfigurieren einer Azure Active Directory Domain Services-Instanz

Azure Active Directory Domain Services (Azure AD DS) stellt verwaltete Domänendienste bereit, z. B. Domänenbeitritt, Gruppenrichtlinie, LDAP und Kerberos-/NTLM-Authentifizierung, die mit Windows Server Active Directory vollständig kompatibel sind. Sie können diese Domänendienste nutzen, ohne selbst Domänencontroller bereitstellen, verwalten und patchen zu müssen. Azure AD DS lässt sich in Ihren vorhandenen Azure AD-Mandanten integrieren. Dank dieser Integration können Benutzer sich mit ihren Unternehmensanmeldeinformationen anmelden, und Sie können vorhandene Gruppen und Benutzerkonten verwenden, um den Zugriff auf Ressourcen zu sichern.

Sie können eine verwaltete Domäne mit den Standardkonfigurationsoptionen für Netzwerk und Synchronisierung erstellen oder [diese Einstellungen manuell festlegen][tutorial-create-instance-advanced]. Dieses Tutorial zeigt Ihnen, wie Sie die Standardoptionen zum Erstellen und Konfigurieren einer Azure AD DS-Instanz im Azure-Portal verwenden.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Grundlegendes zu den DNS-Anforderungen für eine verwaltete Domäne
> * Erstellen einer Azure AD DS-Instanz
> * Aktivieren der Kennworthashsynchronisierung

Wenn Sie kein Azure-Abonnement besitzen, [erstellen Sie ein Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie die folgenden Ressourcen und Berechtigungen:

* Ein aktives Azure-Abonnement.
    * Wenn Sie kein Azure-Abonnement besitzen, [erstellen Sie ein Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Einen mit Ihrem Abonnement verknüpften Azure Active Directory-Mandanten, der entweder mit einem lokalen Verzeichnis synchronisiert oder ein reines Cloudverzeichnis ist.
    * [Erstellen Sie einen Azure Active Directory-Mandanten][create-azure-ad-tenant], oder [verknüpfen Sie ein Azure-Abonnement mit Ihrem Konto][associate-azure-ad-tenant], sofern erforderlich.
* Sie benötigen Berechtigungen als *globaler Administrator* in Ihrem Azure AD-Mandanten, um Azure AD DS zu aktivieren.
* Sie benötigen Berechtigungen als *Mitwirkender* in Ihrem Azure-Abonnement, um die erforderlichen Azure AD DS-Ressourcen zu erstellen.

Es ist bei Azure AD DS zwar nicht erforderlich, für den Azure AD-Mandanten die [Self-Service-Kennwortzurücksetzung (Self-Service Password Reset, SSPR) zu konfigurieren][configure-sspr], es wird jedoch empfohlen. Benutzer können Ihr Kennwort ohne SSPR ändern. SSPR ist jedoch hilfreich, wenn Benutzer ihr Kennwort vergessen haben und es zurücksetzen müssen.

> [!IMPORTANT]
> Nach der Erstellung einer verwalteten Azure AD DS-Domäne können Sie die Instanz nicht in eine andere Ressourcengruppe, ein anderes virtuelles Netzwerk, ein anderes Abonnement usw. verschieben. Wählen Sie bei der Bereitstellung der Azure AD DS-Instanz das am besten geeignete Abonnement, die am besten geeignete Ressourcengruppe und Region und das am besten geeignete virtuelle Netzwerk aus.

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

In diesem Tutorial erstellen und konfigurieren Sie eine Azure AD DS-Instanz im Azure-Portal. Melden Sie sich zunächst beim [Azure-Portal](https://portal.azure.com) an.

## <a name="create-an-instance"></a>Erstellen einer Instanz

Um den Assistenten zum **Aktivieren von Azure AD Domain Services** zu starten, führen Sie die folgenden Schritte aus:

1. Klicken Sie im Azure-Portal in der linken oberen Ecke auf **+ Ressource erstellen**.
1. Geben Sie *Domain Services* in die Suchleiste ein, und wählen Sie *Azure AD Domain Services* aus den Suchvorschlägen aus.
1. Klicken Sie auf der Seite „Azure AD Domain Services“ auf **Erstellen**. Der Assistent zum **Aktivieren von Azure AD Domain Services** wird gestartet.
1. Wählen Sie das **Azure-Abonnement**, in dem Sie die verwaltete Domäne erstellen möchten.
1. Wählen Sie die **Ressourcengruppe**, zu der die verwaltete Domäne gehören soll. Klicken Sie auf **Neu erstellen**, oder wählen Sie eine vorhandene Ressourcengruppe aus.

Beim Erstellen einer Azure AD DS-Instanz geben Sie einen DNS-Namen an. Bei der Auswahl dieses DNS-Namens sind folgende Aspekte zu berücksichtigen:

* **Integrierter Domänenname:** Standardmäßig wird der integrierte Domänenname des Verzeichnisses verwendet (das Suffix *.onmicrosoft.com*). Wenn Sie Secure LDAP für den Zugriff auf die verwaltete Domäne über das Internet aktivieren möchten, können Sie kein digitales Zertifikat erstellen, um die Verbindung mit dieser Standarddomäne zu sichern. Die Domäne *.onmicrosoft.com* ist im Besitz von Microsoft, daher stellt keine Zertifizierungsstelle ein Zertifikat aus.
* **Benutzerdefinierte Domänennamen:** Die gängigste Vorgehensweise besteht darin, einen benutzerdefinierten Domänennamen anzugeben – in der Regel den Namen einer Domäne, die Sie bereits besitzen und die routingfähig ist. Wenn Sie eine routingfähige benutzerdefinierte Domäne verwenden, kann der Datenverkehr ordnungsgemäß und bedarfsgerecht weitergeleitet werden, um Ihre Anwendungen zu unterstützen.
* **Nicht routingfähige Domänensuffixe:** Im Allgemeinen wird empfohlen, nicht routingfähige Domänennamesuffixe wie z. B. *contoso.local* zu vermeiden. Das Suffix *.local* ist nicht routingfähig und kann zu Problemen mit der DNS-Auflösung führen.

> [!TIP]
> Lassen Sie beim Erstellen eines benutzerdefinierten Domänennamens Vorsicht in Bezug auf DNS-Namespaces walten. Es wird empfohlen, ein eindeutiges Präfix für den Domänennamen einzuschließen. Lautet Ihr DNS-Stammname beispielsweise *contoso.com*, erstellen Sie eine verwaltete Azure AD DS-Domäne mit dem benutzerdefinierten Domänennamen *corp.contoso.com* oder *ds.contoso.com*. In einer Hybridumgebung mit einer lokalen AD DS-Umgebung werden diese Präfixe unter Umständen bereits verwendet. Verwenden Sie ein eindeutiges Präfix für Azure AD DS.
>
> Sie können den DNS-Stammnamen für Ihre verwaltete Azure AD DS-Domäne verwenden, müssen aber möglicherweise einige zusätzliche DNS-Einträge für andere Dienste in Ihrer Umgebung erstellen. Beispiel: Wenn Sie einen Webserver ausführen, der unter Verwendung des DNS-Stammnamens eine Website hostet, können Namenskonflikte auftreten, aufgrund derer zusätzliche DNS-Einträge erforderlich sind.
>
> In diesen Tutorials und Anleitungen wird die benutzerdefinierte Domäne *contoso.com* als kurzes Beispiel verwendet. Geben Sie in allen Befehlen Ihren eigenen Domänennamen an, der unter Umständen ein eindeutiges Präfix enthält.
>
> Weitere Informationen finden Sie unter [Auswählen eines Namenspräfixes für die Domäne][naming-prefix].

Es gelten außerdem die folgenden Einschränkungen für DNS-Namen:

* **Einschränkungen für Domänenpräfixe:** Sie können keine verwaltete Domäne mit einem Präfix erstellen, das länger ist als 15 Zeichen. Das Präfix des angegebenen Domänennamens (beispielsweise *contoso* im Domänennamen *contoso.com*) darf maximal 15 Zeichen lang sein.
* **Netzwerknamenskonflikte:** Der DNS-Domänenname für Ihre verwaltete Domäne darf im virtuellen Netzwerk noch nicht vorhanden sein. Achten Sie speziell auf die folgenden Szenarien, die zu einem Namenskonflikt führen würden:
    * Im virtuellen Azure-Netzwerk ist bereits eine Active Directory-Domäne mit dem gleichen DNS-Domänennamen vorhanden.
    * Das virtuelle Netzwerk, in dem Sie die verwaltete Domäne aktivieren möchten, verfügt über eine VPN-Verbindung mit Ihrem lokalen Netzwerk. In diesem Szenario stellen Sie sicher, dass Sie keine Domäne mit demselben DNS-Domänennamen in Ihrem lokalen Netzwerk haben.
    * Im virtuellen Azure-Netzwerk ist bereits ein Azure-Clouddienst mit diesem Namen vorhanden.

Füllen Sie die Felder im Fenster *Grundlagen* des Azure-Portals aus, um eine Azure AD DS-Instanz zu erstellen:

1. Geben Sie einen **DNS-Domänennamen** für Ihre verwaltete Domäne ein, und berücksichtigen Sie dabei die oben genannten Punkte.
1. Wählen Sie den **Azure-Speicherort**, in dem die verwaltete Domäne erstellt werden soll.

    ![Konfigurieren der grundlegenden Einstellungen für eine Azure AD Domain Services-Instanz](./media/tutorial-create-instance/basics-window.png)

Zum schnellen Erstellen einer verwalteten Azure AD DS-Domäne können Sie **Überprüfen + erstellen** auswählen, um zusätzliche Standardkonfigurationsoptionen zu akzeptieren. Bei der Auswahl dieser Erstellungsoption werden die folgenden Standardwerte konfiguriert:

* Ein virtuelles Netzwerk namens *aadds-vnet* wird erstellt, das den IP-Adressbereich *10.0.1.0/24* verwendet.
* Ein Subnetz namens *aadds-subnet* wird erstellt, das den IP-Adressbereich *10.0.1.0/24* verwendet.
* *Alle* Benutzer aus Azure AD werden mit der verwalteten Azure AD DS-Domäne synchronisiert.

1. Wählen Sie **Überprüfen + erstellen** aus, um diese Standardkonfigurationsoptionen zu akzeptieren.

## <a name="deploy-the-managed-domain"></a>Bereitstellen der verwalteten Domäne

Überprüfen Sie die Konfigurationseinstellungen für die verwaltete Domäne auf der Seite **Summary** (Zusammenfassung) des Assistenten. Sie können zu jedem Schritt des Assistenten zurückgehen, um Änderungen vorzunehmen. Sie können auch **eine Vorlage für die Automatisierung herunterladen**, um eine verwaltete Azure AD DS-Domäne mit diesen Konfigurationsoptionen auf konsistente Weise in einem anderen Azure AD-Mandanten erneut bereitzustellen.

1. Wählen Sie zum Erstellen der verwalteten Domäne **Erstellen** aus. Ein Hinweis wird angezeigt, dass bestimmte Konfigurationsoptionen, etwa der DNS-Name oder das virtuelle Netzwerk, nach der Erstellung der verwalteten Azure AD DS-Domäne nicht geändert werden können. Wählen Sie **OK** aus, um fortzufahren.
1. Der Prozess der Bereitstellung Ihrer verwalteten Domänen kann bis zu einer Stunde dauern. Es wird eine Benachrichtigung angezeigt, die Sie über den Status Ihrer Azure AD DS-Bereitstellung informiert. Wählen Sie die Benachrichtigung aus, um sich den detaillierten Fortschritt Ihrer Bereitstellung anzusehen.

    ![Benachrichtigung im Azure-Portal über den Fortschritt der Bereitstellung](./media/tutorial-create-instance/deployment-in-progress.png)

1. Die Seite wird mit Aktualisierungen zum Bereitstellungsvorgang geladen, u. a. mit der Erstellung neuer Ressourcen in Ihrem Verzeichnis.
1. Wählen Sie Ihre Ressourcengruppe aus (z. B. *myResourceGroup*), und wählen Sie dann aus der Liste der Azure-Ressourcen Ihre Azure AD DS-Instanz aus (z. B. *contoso.com*). Die Registerkarte **Übersicht** zeigt an, dass die verwaltete Domäne sich im Status *Wird bereitgestellt* befindet. Sie können die verwaltete Domäne erst dann konfigurieren, wenn sie vollständig bereitgestellt ist.

    ![Status „Wird bereitgestellt“ der Domäne](./media/tutorial-create-instance/provisioning-in-progress.png)

1. Wenn die verwaltete Domäne vollständig bereitgestellt ist, zeigt die Registerkarte **Overview** (Übersicht) den Domänenstatus als *Running* (Wird ausgeführt) an.

    ![Status der Domäne nach erfolgreicher Bereitstellung](./media/tutorial-create-instance/successfully-provisioned.png)

Azure AD Domain Services wird im Azure Active Directory-Mandanten bereitgestellt, und die Azure AD Domain Services-Ressource für den Dienst wird im zugehörigen Azure-Abonnement erstellt. Während des Bereitstellungsprozesses erstellt Azure AD DS zwei Unternehmensanwendungen mit den Namen *Domain Controller Services* und *AzureActiveDirectoryDomainControllerServices* in der Azure Active Directory-Instanz, in der Sie Azure AD Domain Services aktiviert haben. Diese Unternehmensanwendungen werden zur Unterstützung Ihrer verwalteten Domäne benötigt.  Diese Anwendungen dürfen unter keinen Umständen gelöscht werden.

## <a name="update-dns-settings-for-the-azure-virtual-network"></a>Aktualisieren der DNS-Einstellungen für das virtuelle Azure-Netzwerk

Nachdem Azure AD DS erfolgreich bereitgestellt wurde, konfigurieren Sie nun das virtuelle Netzwerk so, dass andere verbundene VMs und Anwendungen die verwaltete Domäne verwenden können. Um diese Konnektivität zu ermöglichen, aktualisieren Sie die DNS-Servereinstellungen für Ihr virtuelles Netzwerk so, dass diese auf die beiden IP-Adressen verweisen, unter denen Azure AD DS bereitgestellt ist.

1. Auf der Registerkarte **Übersicht** für Ihre verwaltete Domäne werden einige **erforderliche Konfigurationsschritte** angezeigt. Der erste Konfigurationsschritt besteht darin, die DNS-Servereinstellungen für Ihr virtuelles Netzwerk zu aktualisieren. Sobald die DNS-Einstellungen ordnungsgemäß konfiguriert sind, wird dieser Schritt nicht mehr angezeigt.

    Die aufgelisteten Adressen sind die Domänencontroller, die im virtuellen Netzwerk genutzt werden können. In diesem Beispiel lauten die Adressen *10.1.0.4* und *10.1.0.5*. Sie finden diese IP-Adressen später auf der Registerkarte **Eigenschaften**.

    ![Konfigurieren von DNS-Einstellungen für Ihr virtuelles Netzwerk mit den IP-Adressen der Azure AD Domain Services](./media/tutorial-create-instance/configure-dns.png)

1. Klicken Sie auf die Schaltfläche **Konfigurieren**, um die DNS-Servereinstellungen für das virtuelle Netzwerk zu aktualisieren. Die DNS-Einstellungen werden automatisch für Ihr virtuelles Netzwerk konfiguriert.

> [!TIP]
> Wenn Sie in den vorherigen Schritten ein virtuelles Netzwerk ausgewählt haben, erhalten alle VMs, die mit dem Netzwerk verbunden sind, die neuen DNS-Einstellungen erst nach einem Neustart. Sie können VMs über das Azure-Portal, mit Azure PowerShell oder mithilfe der Azure CLI neu starten.

## <a name="enable-user-accounts-for-azure-ad-ds"></a>Aktivieren von Benutzerkonten für Azure AD DS

Um Benutzer in der verwalteten Domäne authentifizieren zu können, benötigt Azure AD DS Kennworthashes in einem Format, das für die Authentifizierung über NT LAN Manager (NTLM) und Kerberos geeignet ist. Azure AD generiert oder speichert erst dann Kennworthashes in dem für die NTLM- oder Kerberos-Authentifizierung erforderlichen Format, wenn Sie Azure AD DS für Ihren Mandanten aktivieren. Aus Sicherheitsgründen speichert Azure AD Kennwörter nicht als Klartext. Daher kann Azure AD diese NTLM- oder Kerberos-Kennworthashes nicht automatisch auf der Grundlage bereits vorhandener Anmeldeinformationen von Benutzern generieren.

> [!NOTE]
> Nach entsprechender Konfiguration werden die verwendbaren Kennworthashes in der verwalteten Azure AD DS-Domäne gespeichert. Wenn Sie die verwaltete Azure AD DS-Domäne löschen, werden alle zu diesem Zeitpunkt gespeicherten Kennworthashes ebenfalls gelöscht. Synchronisierte Anmeldeinformationen in Azure AD können nicht wiederverwendet werden, wenn Sie später eine verwaltete Azure AD DS-Domäne erstellen. Sie müssen die Kennworthashsynchronisierung erneut konfigurieren, um die Kennworthashes wieder zu speichern. VMs oder Benutzer, die zuvor in eine Domäne eingebunden wurden, können sich nicht sofort authentifizieren, weil Azure AD die Kennworthashes in der neuen verwalteten Azure AD DS-Domäne generieren und speichern muss. Weitere Informationen finden Sie unter [Prozess der Kennworthashsynchronisierung für Azure AD DS und Azure AD Connect][password-hash-sync-process].

Zum Generieren und Speichern dieser Kennworthashes müssen für in Azure AD erstellte reine Cloudbenutzerkonten andere Schritte ausgeführt werden als für Benutzerkonten, die mit Azure AD Connect aus Ihrem lokalen Verzeichnis synchronisiert werden. Ein reines Cloudbenutzerkonto ist ein Konto, das in Ihrem Azure AD-Verzeichnis über das Azure-Portal oder mithilfe von Azure AD PowerShell-Cmdlets erstellt wurde. Diese Benutzerkonten werden nicht von einem lokalen Verzeichnis aus synchronisiert. In diesem Tutorial verwenden wir ein einfaches, rein cloudbasiertes Benutzerkonto. Weitere Informationen zu den zusätzlichen Schritten, die für die Verwendung von Azure AD Connect erforderlich sind, finden Sie unter [Synchronisieren von Kennworthashes für Benutzerkonten, die von Ihrem lokalen Active Directory aus in Ihrer verwalteten Domäne synchronisiert werden][on-prem-sync].

> [!TIP]
> Wenn Ihr Azure AD-Mandant über eine Kombination aus reinen Cloudbenutzern und Benutzern aus Ihrem lokalen Active Directory verfügt, müssen beide Prozeduren durchgeführt werden.

Bei reinen Cloudbenutzerkonten müssen Benutzer ihre Kennwörter ändern, bevor sie Azure AD DS verwenden können. Diese Kennwortänderung führt dazu, dass die Kennworthashes für die Kerberos- und NTLM-Authentifizierung in Azure AD generiert und gespeichert werden. Sie können die Kennwörter für alle Benutzer im Mandanten, die Azure AD DS verwenden müssen, als „abgelaufen“ markieren, wodurch bei der nächsten Anmeldung eine Kennwortänderung erzwungen wird. Alternativ dazu können Sie diese Benutzer anweisen, ihre Kennwörter manuell zu ändern. In diesem Tutorial ändern wir ein Benutzerkennwort manuell.

Bevor ein Benutzer sein Kennwort zurücksetzen kann, muss der Azure AD-Mandant für die [Self-Service-Kennwortzurücksetzung konfiguriert][configure-sspr] werden.

Wenn Sie reiner Cloudbenutzer sind, müssen Sie zum Ändern des Kennworts folgende Schritte ausführen:

1. Navigieren Sie zum Azure AD-Zugriffsbereich unter [https://myapps.microsoft.com](https://myapps.microsoft.com).
1. Klicken Sie in der oberen rechten Ecke auf Ihren Namen, und wählen Sie dann aus dem Dropdownmenü die Option **Profil** aus.

    ![Auswählen des Profils](./media/tutorial-create-instance/select-profile.png)

1. Klicken Sie auf der Seite **Profil** auf **Kennwort ändern**.
1. Geben Sie auf der Seite **Kennwort ändern** Ihr vorhandenes (altes) Kennwort ein. Geben Sie dann ein neues Kennwort ein und bestätigen dies.
1. Klicken Sie auf **Submit** (Senden).

Nach der Kennwortänderung dauert es einige Minuten, bis das neue Kennwort in Azure AD DS verwendet werden kann und Sie sich bei Computern anmelden können, die in die verwaltete Domäne eingebunden sind.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Grundlegendes zu den DNS-Anforderungen für eine verwaltete Domäne
> * Erstellen einer Azure AD DS-Instanz
> * Hinzufügen von Administratorbenutzern zur Domänenverwaltung
> * Aktivieren von Benutzerkonten für Azure AD DS und Generieren von Kennworthashes

Konfigurieren Sie ein virtuelles Azure-Netzwerk für Anwendungsworkloads, bevor Sie virtuelle Computer in eine Domäne einbinden und Anwendungen bereitstellen, die die verwaltete Azure AD DS-Domäne verwenden.

> [!div class="nextstepaction"]
> [Konfigurieren eines virtuellen Azure-Netzwerks für Anwendungsworkloads zur Verwendung der verwalteten Domäne](tutorial-configure-networking.md)

<!-- INTERNAL LINKS -->
[tutorial-create-instance-advanced]: tutorial-create-instance-advanced.md
[create-azure-ad-tenant]: ../active-directory/fundamentals/sign-up-organization.md
[associate-azure-ad-tenant]: ../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md
[network-considerations]: network-considerations.md
[create-dedicated-subnet]: ../virtual-network/virtual-network-manage-subnet.md#add-a-subnet
[scoped-sync]: scoped-synchronization.md
[on-prem-sync]: tutorial-configure-password-hash-sync.md
[configure-sspr]: ../active-directory/authentication/quickstart-sspr.md
[password-hash-sync-process]: ../active-directory/hybrid/how-to-connect-password-hash-synchronization.md#password-hash-sync-process-for-azure-ad-domain-services

<!-- EXTERNAL LINKS -->
[naming-prefix]: /windows-server/identity/ad-ds/plan/selecting-the-forest-root-domain#selecting-a-prefix
