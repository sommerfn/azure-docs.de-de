---
title: Verwalten von DNS für Azure AD Domain Services | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die DNS-Servertools zur Verwaltung von DNS für eine von Azure Active Directory Domain Services verwaltete Domäne installieren.
author: iainfoulds
manager: daveba
ms.assetid: 938a5fbc-2dd1-4759-bcce-628a6e19ab9d
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: conceptual
ms.date: 10/31/2019
ms.author: iainfou
ms.openlocfilehash: c225be5a1123c89d8a470a8dea48b3c57eb893b5
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73474577"
---
# <a name="administer-dns-in-an-azure-ad-domain-services-managed-domain"></a>Verwalten von DNS in einer durch Azure AD Domain Services verwalteten Domäne

In Azure Active Directory Domain Services (Azure AD DS) ist DNS eine Schlüsselkomponente. Azure AD DS umfasst einen DNS-Server, der die Namensauflösung für die verwaltete Domäne bereitstellt. Dieser DNS-Server enthält integrierte DNS-Einträge und Updates für die wichtigsten Komponenten, die die Ausführung des Dienstes ermöglichen.

Wenn Sie Ihre eigenen Anwendungen und Dienste ausführen, müssen Sie unter Umständen DNS-Einträge für nicht in die Domäne eingebundene Computer erstellen, virtuelle IP-Adressen für Load Balancer konfigurieren oder externe DNS-Weiterleitungen einrichten. Benutzern, die zur Gruppe *AAD DC-Administratoren* gehören, werden DNS-Administratorrechte in der von Azure AD DS verwalteten Domäne gewährt und sie können benutzerdefinierte DNS-Einträge erstellen und bearbeiten.

In einer Hybridumgebung werden in einer lokalen AD DS-Umgebung konfigurierte DNS-Zonen und DNS-Einträge nicht mit Azure AD DS synchronisiert. Um Ihre eigenen DNS-Einträge zu definieren und zu verwenden, erstellen Sie Einträge im Azure AD DS DNS-Server, oder verwenden Sie bedingte Weiterleitungen, die auf vorhandene DNS-Server in Ihrer Umgebung verweisen.

In diesem Artikel erfahren Sie, wie Sie die DNS-Servertools installieren und dann die DNS-Konsole zur Verwaltung von Einträgen in Azure AD DS verwenden.

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
    * Führen Sie bei Bedarf die [Schritte im Tutorial zum Erstellen einer Windows Server-VM und Einbinden der VM in eine verwaltete Domäne][create-join-windows-vm] aus.
* Ein Benutzerkonto, das Mitglied der *Administratorengruppe für Azure AD-Domänencontroller* (AAD-DC-Administratoren) in Ihrem Azure AD-Mandanten ist.

## <a name="install-dns-server-tools"></a>Installieren von DNS-Servertools

Zum Erstellen und Ändern von DNS-Einträgen in Azure AD DS müssen Sie die DNS-Servertools installieren. Diese Tools können als Feature in Windows Server installiert werden. Weitere Informationen zum Installieren der Verwaltungstools auf einem Windows-Client finden Sie unter [Installieren der Remoteserver-Verwaltungstools (RSAT)][install-rsat].

1. Melden Sie sich bei Ihrer Verwaltungs-VM an. Weitere Informationen zu den Schritten zum Herstellen einer Verbindung mithilfe des Azure-Portals finden Sie unter [Herstellen einer Verbindung mit einer Windows Server-VM][connect-windows-server-vm].
1. Wenn **Server-Manager** bei der Anmeldung beim virtuellen Computer nicht standardmäßig geöffnet wird, wählen Sie das **Startmenü** und dann **Server-Manager** aus.
1. Wählen Sie im Bereich *Dashboard* des Fensters **Server-Manager** die Option **Rollen und Features hinzufügen** aus.
1. Klicken Sie auf der Seite **Vorbereitung** des *Assistenten zum Hinzufügen von Rollen und Features* auf **Weiter**.
1. Lassen Sie für *Installationstyp* die Option **Rollenbasierte oder featurebasierte Installation** aktiviert, und wählen Sie **Weiter** aus.
1. Wählen Sie auf der Seite **Serverauswahl** die aktuelle VM aus dem Serverpool aus (z. B. *myvm.contoso.com*), und klicken Sie dann auf **Weiter**.
1. Klicken Sie auf der Seite **Serverrollen** auf **Weiter**.
1. Erweitern Sie auf der Seite **Features** den Knoten **Remote Server-Verwaltungstools** und anschließend den Knoten **Rollenverwaltungstools**. Wählen Sie das Feature **DNS-Servertools** in der Liste der Rollenverwaltungstools aus.

    ![Wählen Sie aus der Liste der verfügbaren Tools zur Rollenverwaltung die Installation der DNS-Servertools aus.](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-dns-tools.png)

1. Wählen Sie auf der Seite **Bestätigung** die Option **Installieren** aus. Die Installation der Tools für die Gruppenrichtlinienverwaltung kann ein bis zwei Minuten dauern.
1. Wenn die Installation des Features abgeschlossen ist, wählen Sie **Schließen** aus, um den **Assistenten zum Hinzufügen von Rollen und Features** zu beenden.

## <a name="open-the-dns-management-console-to-administer-dns"></a>Öffnen der DNS-Verwaltungskonsole zum Verwalten von DNS

Wenn die DNS-Servertools installiert sind, können Sie DNS-Einträge auf der von Azure AD DS verwalteten Domäne verwalten.

> [!NOTE]
> Zum Verwalten von DNS in einer verwalteten Azure AD DS-Domäne müssen Sie bei einem Benutzerkonto angemeldet sein, das Mitglied der Gruppe *AAD DC-Administrators* ist.

1. Klicken Sie auf dem Startbildschirm auf **Verwaltung**. Es wird eine Liste der verfügbaren Verwaltungstools angezeigt, einschließlich **DNS**, das im vorherigen Abschnitt installiert wurde. Wählen Sie **DNS** aus, um die DNS-Verwaltungskonsole zu starten.
1. Wählen Sie im Dialogfeld **Verbindung mit DNS-Server herstellen** die Option **Folgender Computer** aus, und geben Sie dann den DNS-Domänennamen der verwalteten Domäne ein (z. B. *contoso.com*).

    ![Verbinden mit der von Azure AD DS verwalteten Domäne in der DNS-Konsole](./media/active-directory-domain-services-admin-guide/dns-console-connect-to-domain.png)

1. Die DNS-Konsole stellt eine Verbindung mit der von Azure AD DS verwalteten Domäne her. Erweitern Sie die **Forward-Lookup-Zonen** oder **Reverse-Lookup-Zonen**, um Ihre gewünschten DNS-Einträge zu erstellen oder bestehende Einträge nach Bedarf zu bearbeiten.

    ![DNS-Konsole – Domäne verwalten](./media/active-directory-domain-services-admin-guide/dns-console-managed-domain.png)

> [!WARNING]
> Wenn Sie Einträge mit den DNS-Servertools verwalten, stellen Sie sicher, dass Sie die integrierten DNS-Einträge, die von Azure AD DS verwendet werden, nicht löschen oder ändern. Zu den integrierten DNS-Einträgen zählen Domänen-DNS-Einträge, Namenservereinträge sowie weitere für den DC-Standort verwendete Einträge. Wenn Sie diese Einträge ändern, werden die Domänendienste im virtuellen Netzwerk unterbrochen.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur DNS-Verwaltung finden Sie im [Technet-Artikel zu DNS-Tools](https://technet.microsoft.com/library/cc753579.aspx).

<!-- INTERNAL LINKS -->
[create-azure-ad-tenant]: ../active-directory/fundamentals/sign-up-organization.md
[associate-azure-ad-tenant]: ../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md
[create-azure-ad-ds-instance]: tutorial-create-instance.md
[create-join-windows-vm]: join-windows-vm.md
[tutorial-create-management-vm]: tutorial-create-management-vm.md
[connect-windows-server-vm]: join-windows-vm.md#connect-to-the-windows-server-vm

<!-- EXTERNAL LINKS -->
[install-rsat]: /windows-server/remote/remote-server-administration-tools#BKMK_Thresh
