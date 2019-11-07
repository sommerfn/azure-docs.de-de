---
title: Erstellen einer Organisationseinheit (OE) in Azure AD Domain Services | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine benutzerdefinierte Organisationseinheit (OU) in einer von Azure AD Domain Services verwalteten Domäne erstellen und verwalten.
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.assetid: 52602ad8-2b93-4082-8487-427bdcfa8126
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: conceptual
ms.date: 10/31/2019
ms.author: iainfou
ms.openlocfilehash: 7d651849f5c8d930d99e87931eed5b823e90113c
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73474766"
---
# <a name="create-an-organizational-unit-ou-in-an-azure-ad-domain-services-managed-domain"></a>Erstellen einer Organisationseinheit (OE) in einer durch Azure AD Domain Services verwalteten Domäne

Mit Organisationseinheiten (OEs) in Active Directory Domain Services (AD DS) können Sie Objekte wie Benutzerkonten, Dienstkonten oder Computerkonten logisch gruppieren. Sie können dann Administratoren bestimmten Organisationseinheiten zuordnen und Gruppenrichtlinien anwenden, um gezielte Konfigurationseinstellungen zu erzwingen.

Von Azure AD DS verwaltete Domänen umfassen zwei integrierte Organisationseinheiten: *AADDC-Computer* und *AADDC-Benutzer*. Die Organisationseinheit *AADDC-Computer* enthält Computerobjekte für alle Computer, die in die verwaltete Domäne eingebunden sind. Die Organisationseinheit *AADDC-Benutzer* enthält Benutzer und Gruppen, die über den Azure AD-Mandanten synchronisiert wurden. Wenn Sie Workloads erstellen und ausführen, die Azure AD DS verwenden, müssen Sie möglicherweise Dienstkonten für Anwendungen erstellen, damit diese sich selbst authentifizieren können. Um diese Dienstkonten zu organisieren, erstellen Sie häufig eine benutzerdefinierte Organisationseinheit in der von Azure AD DS verwalteten Domäne und erstellen dann Dienstkonten innerhalb dieser Organisationseinheit.

In einer Hybridumgebung werden in einer lokalen AD DS-Umgebung erstellte Organisationseinheiten nicht mit Azure AD DS synchronisiert. Verwaltete Azure AD DS-Domänen weisen eine flache OE-Struktur auf. Alle Benutzerkonten und -gruppen werden ungeachtet der Synchronisierung aus verschiedenen lokalen Domänen oder Gesamtstrukturen im Container *AADDC Users* gespeichert, auch wenn Sie eine hierarchische OE-Struktur konfiguriert haben.

In diesem Artikel wird gezeigt, wie Sie in der von Azure AD DS verwalteten Domäne eine Organisationseinheit erstellen.

[!INCLUDE [active-directory-ds-prerequisites.md](../../includes/active-directory-ds-prerequisites.md)]

## <a name="before-you-begin"></a>Voraussetzungen

Für diesen Artikel benötigen Sie die folgenden Ressourcen und Berechtigungen:

* Ein aktives Azure-Abonnement.
    * Wenn Sie kein Azure-Abonnement besitzen, [erstellen Sie ein Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Einen mit Ihrem Abonnement verknüpften Azure Active Directory-Mandanten, der entweder mit einem lokalen Verzeichnis synchronisiert oder ein reines Cloudverzeichnis ist.
    * [Erstellen Sie einen Azure Active Directory-Mandanten][create-azure-ad-tenant], oder [verknüpfen Sie ein Azure-Abonnement mit Ihrem Konto][associate-azure-ad-tenant], sofern erforderlich.
* Eine verwaltete Azure Active Directory Domain Services-Domäne, die in Ihrem Azure AD-Mandanten aktiviert und konfiguriert ist.
    * Führen Sie bei Bedarf das Tutorial zum [Erstellen und Konfigurieren einer Azure Active Directory Domain Services-Instanz][create-azure-ad-ds-instance] aus.
* Eine Windows Server-Verwaltungs-VM, die in die verwaltete Azure AD DS-Domäne eingebunden ist.
    * Führen Sie bei Bedarf das Tutorial zum [Erstellen eines virtuellen Verwaltungscomputers][tutorial-create-management-vm] aus.
* Ein Benutzerkonto, das Mitglied der *Administratorengruppe für Azure AD-Domänencontroller* (AAD-DC-Administratoren) in Ihrem Azure AD-Mandanten ist.

## <a name="custom-ou-considerations-and-limitations"></a>Überlegungen und Einschränkungen zu benutzerdefinierten Organisationseinheiten

Wenn Sie benutzerdefinierte Organisationseinheiten in einer von Azure AD DS verwalteten Domäne erstellen, erhalten Sie zusätzliche Flexibilität für die Benutzerverwaltung und die Anwendung von Gruppenrichtlinien. Im Vergleich zu einer lokalen AD DS-Umgebung gibt es einige Einschränkungen und Überlegungen bei der Erstellung und Verwaltung einer benutzerdefinierten Organisationseinheitsstruktur in Azure AD DS:

* Um benutzerdefinierte Organisationseinheiten erstellen zu können, müssen Benutzer Mitglied der Gruppe *AAD DC Administrators* sein.
* Ein Benutzer, der eine benutzerdefinierte Organisationseinheit erstellt, erhält Administratorrechte (Vollzugriff) über diese Organisationseinheit und ist der Ressourcenbesitzer.
    * Standardmäßig hat die Gruppe *AAD DC Administrators* auch Vollzugriff auf die benutzerdefinierte Organisationseinheit.
* Es wird eine Standardorganisationseinheit für *AADDC-Benutzer* erstellt, die alle synchronisierten Benutzerkonten Ihres Azure AD-Mandanten enthält.
    * Sie können keine Benutzer oder Gruppen der Organisationseinheit *AADDC-Benutzer* in benutzerdefinierte Organisationseinheiten verschieben, die Sie erstellen. Nur Benutzerkonten oder Ressourcen, die in der von Azure AD DS verwalteten Domäne erstellt wurden, können in benutzerdefinierte Organisationseinheiten verschoben werden.
* Benutzerkonten, Gruppen, Dienstkonten und Computerobjekte, die Sie in benutzerdefinierten Organisationseinheiten erstellen, stehen in Ihrem Azure AD-Mandanten nicht zur Verfügung.
    * Diese Objekte werden nicht über die Azure AD Graph-API oder auf der Azure AD-Benutzeroberfläche angezeigt. Sie sind nur in Ihrer von Azure AD DS verwalteten Domäne verfügbar.

## <a name="create-a-custom-ou"></a>Erstellen einer benutzerdefinierten Organisationseinheit

Um eine benutzerdefinierte Organisationseinheit zu erstellen, verwenden Sie die Active Directory-Verwaltungstools auf einer in die Domäne eingebundenen VM. Mit dem Active Directory-Verwaltungscenter können Sie Ressourcen in einer verwalteten Azure AD DS-Domäne anzeigen, bearbeiten und erstellen, einschließlich Organisationseinheiten.

> [!NOTE]
> Zum Erstellen einer benutzerdefinierten Organisationseinheit in einer verwalteten Azure AD DS-Domäne müssen Sie bei einem Benutzerkonto angemeldet sein, das Mitglied der Gruppe *AAD DC-Administrators* ist.

1. Melden Sie sich bei Ihrer Verwaltungs-VM an. Weitere Informationen zu den Schritten zum Herstellen einer Verbindung mithilfe des Azure-Portals finden Sie unter [Herstellen einer Verbindung mit einer Windows Server-VM][connect-windows-server-vm].
1. Klicken Sie auf dem Startbildschirm auf **Verwaltung**. Es wird eine Liste der verfügbaren Verwaltungstools angezeigt, die im Tutorial zum [Erstellen eines virtuellen Verwaltungscomputers][tutorial-create-management-vm] installiert wurden.
1. Wählen Sie zum Erstellen und Verwalten von Organisationseinheiten **Active Directory-Verwaltungscenter** aus der Liste der Verwaltungstools aus.
1. Wählen Sie im linken Bereich die verwaltete Azure AD DS-Domäne aus, z. B. *contoso.com*. Eine Liste der vorhandenen Organisationseinheiten und Ressourcen wird angezeigt:

    ![Wählen Sie Ihre von Azure AD DS verwaltete Domäne im Active Directory-Verwaltungscenter aus.](./media/active-directory-domain-services-admin-guide/create-ou-adac-overview.png)

1. Der Bereich **Aufgaben** wird auf der rechten Seite des Active Directory-Verwaltungscenters angezeigt. Wählen Sie unter der Domäne wie *contoso.com* die Option **Neu > Organisationseinheit** aus.

    ![Wählen Sie die Option aus, um eine neue Organisationseinheit im Active Directory-Verwaltungscenter zu erstellen.](./media/active-directory-domain-services-admin-guide/create-ou-adac-new-ou.png)

1. Geben Sie im Dialogfeld **Organisationseinheit erstellen** einen **Namen** für die neue Organisationseinheit an, z. B. *MyCustomOu*. Geben Sie eine kurze Beschreibung für die Organisationseinheit an, z. B. *Benutzerdefinierte Organisationseinheit für Dienstkonten*. Bei Bedarf können Sie auch das Feld **Verwaltet von** für die Organisationseinheit festlegen. Wählen Sie **OK** aus, um die benutzerdefinierte Organisationseinheit zu erstellen.

    ![Erstellen einer benutzerdefinierten Organisationseinheit im Active Directory-Verwaltungscenter](./media/active-directory-domain-services-admin-guide/create-ou-dialog.png)

1. Im Active Directory-Verwaltungscenter ist jetzt die benutzerdefinierte Organisationseinheit aufgeführt und kann verwendet werden:

    ![Zur Verwendung verfügbare benutzerdefinierte Organisationseinheit im Active Directory-Verwaltungscenter](./media/active-directory-domain-services-admin-guide/create-ou-done.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Verwendung der Verwaltungstools oder zum Erstellen und Verwenden von Dienstkonten finden Sie in den folgenden Artikeln:

* [Active Directory-Verwaltungscenter: Erste Schritte](https://technet.microsoft.com/library/dd560651.aspx)
* [Schrittweise Anleitung zu Dienstkonten](https://technet.microsoft.com/library/dd548356.aspx)

<!-- INTERNAL LINKS -->
[create-azure-ad-tenant]: ../active-directory/fundamentals/sign-up-organization.md
[associate-azure-ad-tenant]: ../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md
[create-azure-ad-ds-instance]: tutorial-create-instance.md
[tutorial-create-management-vm]: tutorial-create-management-vm.md
[connect-windows-server-vm]: join-windows-vm.md#connect-to-the-windows-server-vm
