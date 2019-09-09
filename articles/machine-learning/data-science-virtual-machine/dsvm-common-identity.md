---
title: Einrichten einer gemeinsamen Identität
titleSuffix: Azure Data Science Virtual Machine
description: Erfahren Sie, wie Sie gemeinsame Benutzerkonten erstellen, die auf mehreren Data Science Virtual Machines verwendet werden können. Sie können Azure Active Directory oder ein lokales Active Directory verwenden, um die Authentifizierung von Benutzern mit der Data Science Virtual Machine auszuführen.
keywords: Deep Learning, KI, Data Science-Tools, virtueller Data Science-Computer, räumliche Analysen, Data Science-Teamprozess
services: machine-learning
ms.service: machine-learning
ms.subservice: data-science-vm
author: vijetajo
ms.author: vijetaj
ms.topic: conceptual
ms.date: 05/08/2018
ms.openlocfilehash: 44f1f7ae3b290e1dbf01877f3881e1d95a238446
ms.sourcegitcommit: 5f67772dac6a402bbaa8eb261f653a34b8672c3a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/01/2019
ms.locfileid: "70208140"
---
# <a name="set-up-a-common-identity-on-a-data-science-virtual-machine"></a>Einrichten einer gemeinsamen Identität für Data Science Virtual Machine

Auf einem virtuellen Microsoft Azure-Computer (VM) wie Data Science Virtual Machine (DSVM) erstellen Sie lokale Benutzerkonten bei der VM-Bereitstellung. Benutzer authentifizieren sich dann mit diesen Anmeldeinformationen bei der VM. Wenn Sie über mehrere VMS verfügen, auf die Benutzer zugreifen müssen, kann das Verwalten von Anmeldeinformationen sehr mühsam werden. Eine hervorragende Lösung besteht darin, gemeinsame Benutzerkonten und die Verwaltung über einen auf Standards basierenden Identitätsanbieter bereitzustellen. Durch diese Vorgehensweise können Sie einen einzelnen Satz von Anmeldeinformationen für den Zugriff auf mehrere Ressourcen in Azure verwenden, einschließlich mehrerer DSVMs.

Active Directory ist ein beliebter Identitätsanbieter und wird in Azure als Clouddienst sowie als lokales Verzeichnis unterstützt. Sie können Benutzer einer eigenständigen DSVM-Instanz oder in einem DSVM-Cluster in einer Azure-VM-Skalierungsgruppe mit Azure Active Directory (Azure AD) oder dem lokalen Active Directory-Verzeichnis authentifizieren. Dazu müssen die DSVM-Instanzen einer Active Directory-Domäne beitreten.

Wenn Sie bereits über Active Directory verfügen, können Sie Active Directory als gemeinsamen Identitätsanbieter verwenden. Für den Fall, dass Sie nicht über Active Directory verfügen, können Sie eine verwaltete Active Directory-Instanz in Azure über [Azure Active Directory Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/) (Azure AD DS) ausführen.

Die Dokumentation für [Azure AD](https://docs.microsoft.com/azure/active-directory/) bietet detaillierte [Anweisungen für die Verwaltung](https://docs.microsoft.com/azure/active-directory/choose-hybrid-identity-solution), einschließlich Anleitungen zur Verbindung von Azure AD mit Ihrem lokalen Verzeichnis, sofern vorhanden.

Dieser Artikel beschreibt, wie Sie einen vollständig verwalteten Active Directory-Domänendienst in Azure mithilfe von Azure AD DS einrichten. Anschließend können Sie Ihre DSVMs der verwalteten Active Directory-Domäne hinzufügen. Dieser Ansatz ermöglicht es Benutzern, über ein gemeinsames Benutzerkonto und Anmeldeinformationen auf einen Pool von DSVMs (und andere Azure-Ressourcen) zuzugreifen.

## <a name="set-up-a-fully-managed-active-directory-domain-on-azure"></a>Einrichten einer vollständig verwalteten Active Directory-Domäne in Azure

Azure AD DS vereinfacht die Verwaltung Ihrer Identitäten, indem Ihnen ein vollständig verwalteter Dienst in Azure bereitgestellt wird. In dieser Active Directory-Domäne verwalten Sie Benutzer und Gruppen. Die folgenden Schritte sind zum Einrichten einer in Azure gehosteten Active Directory-Domäne und der Benutzerkonten in Ihrem Verzeichnis erforderlich:

1. Fügen Sie im Azure-Portal den Benutzer zu Active Directory hinzu: 

   1. Melden Sie sich beim [Azure Active Directory Admin Center](https://aad.portal.azure.com) mit einem Konto an, das als globaler Administrator für das Verzeichnis konfiguriert ist.
    
   1. Wählen Sie **Azure Active Directory** und dann **Benutzer und Gruppen** aus.
    
   1. Wählen Sie unter **Benutzer und Gruppen** die Option **Alle Benutzer** und dann **Neuer Benutzer** aus.
   
           The **User** pane opens:
      
      ![Der Bereich „Benutzer“](./media/add-user.png)
    
   1. Geben Sie Details zum Benutzer ein, beispielsweise **Name** und **Benutzername**. Der Domänennamensteil des Benutzernamens muss entweder der anfängliche Standarddomänenname „[Domänenname].onmicrosoft.com“ oder ein überprüfter, nicht im Verbund konfigurierter [benutzerdefinierter Domänenname](../../active-directory/add-custom-domain.md) wie z.B. „contoso.com“ sein.
    
   1. Kopieren oder notieren Sie sich das generierte Benutzerkennwort, damit sie es nach Abschluss des Vorgangs dem Benutzer mitteilen können.
    
   1. Optional können Sie die Informationen unter **Profil**, **Gruppen** oder **Verzeichnisrolle** für den Benutzer ausfüllen. 
    
   1. Wählen Sie unter **Benutzer** die Option **Erstellen** aus.
    
   1. Sorgen Sie für eine sichere Übermittlung des generierten Kennworts an den neuen Benutzer, damit sich dieser anmelden kann.

1. Erstellen Sie eine Azure AD DS-Instanz. Befolgen Sie die Anleitungen unter [Aktivieren von Azure Active Directory Domain Services mithilfe des Azure-Portals](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-getting-started) (Abschnitt „Erstellen einer Instanz und Konfigurieren grundlegender Einstellungen“). Es ist wichtig, die vorhandenen Benutzerkennwörter in Active Directory zu aktualisierten, damit das Kennwort in den Azure AD DS synchronisiert wird. Es ist auch wichtig, Azure AD DS DNS hinzuzufügen, wie unter „Vervollständigen der Felder im Fenster „Grundlagen“ des Azure-Portals zum Erstellen einer Azure AD DS-Instanz“ in diesem Abschnitt beschrieben.

1. Erstellen Sie ein separates DSVM-Subnetz im virtuellen Netzwerk, das im Abschnitt „Erstellen und Konfigurieren des virtuellen Netzwerks“ des vorherigen Schritts erstellt wurde.
1. Erstellen Sie mindestens eine DSVM-Instanz im DSVM-Subnetz.
1. Befolgen Sie die [Anweisungen](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-join-ubuntu-linux-vm ) zum Hinzufügen der DSVM zu Active Directory. 
1. Binden Sie als Nächstes eine Azure Files-Freigabe zum Hosten Ihres Stamm- oder Notebookverzeichnisses ein, damit Ihr Arbeitsbereich auf jedem Computer eingebunden werden kann. (Wenn Sie strenge Berechtigungen auf Dateiebene benötigen, muss NFS (Network File System) auf mindestens einer VM ausgeführt werden.)

   1. [Erstellen Sie eine Azure Files-Freigabe](../../storage/files/storage-how-to-create-file-share.md).
    
   2.  Binden Sie diese Freigabe auf der Linux-DSVM ein. Wenn Sie auf **Verbinden** für die Azure Files-Freigabe in Ihrem Speicherkonto im Azure-Portal klicken, wird der Befehl angezeigt, der in der Bash-Shell auf der Linux-DSVM ausgeführt werden soll. Der Befehl sieht wie folgt aus:
   
   ```
   sudo mount -t cifs //[STORAGEACCT].file.core.windows.net/workspace [Your mount point] -o vers=3.0,username=[STORAGEACCT],password=[Access Key or SAS],dir_mode=0777,file_mode=0777,sec=ntlmssp
   ```
1. Angenommen, Sie haben die Azure Files-Freigabe z.B. in „/data/workspace“ eingebunden. Erstellen Sie nun Verzeichnisse für jeden Benutzer in der Freigabe: „/data/workspace/user1“, „/data/workspace/user2“ usw. Erstellen Sie ein `notebooks`-Verzeichnis im Arbeitsbereich der einzelnen Benutzer. 
1. Erstellen Sie symbolische Verknüpfungen für `notebooks` in `$HOME/userx/notebooks/remote`.   

Jetzt sind die Benutzer in Ihrer Active Directory-Instanz enthalten, die in Azure gehostet wird. Mithilfe von Active Directory-Anmeldeinformationen können sich Benutzer bei einer beliebigen DSVM-Instanz (SSH oder JupyterHub) anmelden, die in Azure AD DS eingebunden ist. Da sich der Benutzerarbeitsbereich in einer Azure Files-Freigabe befindet, haben Benutzer über jede DSVM-Instanz mithilfe von JupyterHub Zugriff auf ihre Notebooks und auf sonstige Arbeiten.

Für die automatische Skalierung können Sie eine VM-Skalierungsgruppe verwenden, um einen Pool von VMs zu erstellen, die alle auf diese Weise zur Domäne hinzugefügt werden und bei denen der freigegebene Datenträger eingebunden ist. Benutzer können sich bei jedem verfügbaren Computer in der VM-Skalierungsgruppe anmelden und besitzen Zugriff auf den freigegebenen Datenträger, auf dem ihre Notebooks gespeichert sind. 

## <a name="next-steps"></a>Nächste Schritte

* [Sicheres Speichern von Anmeldeinformationen für den Zugriff auf Cloudressourcen](dsvm-secure-access-keys.md)



