---
title: include file
description: include file
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 07/30/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 5c45dbe29bb86150c76cf5bc136c4a7504270f86
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/08/2019
ms.locfileid: "68854581"
---
[Azure Files](../articles/storage/files/storage-files-introduction.md) unterstützt die identitätsbasierte Authentifizierung per SMB (Server Message Block) über [Azure Active Directory Domain Services (Azure AD DS)](../articles/active-directory-domain-services/overview.md). Ihre in die Domänen eingebundenen virtuellen Windows-Computer (VMs) können dann mit [Azure Active Directory](../articles/active-directory/fundamentals/active-directory-whatis.md)-Anmeldeinformationen (Azure AD) auf Azure-Dateifreigaben zugreifen.

Mit der [rollenbasierten Zugriffssteuerung (RBAC)](../articles/role-based-access-control/overview.md) können Sie die Azure Files-Freigabeebene für eine Identität wie einen Benutzer oder eine Gruppe in Azure AD festlegen. Sie können benutzerdefinierte RBAC-Rollen definieren, die gängige Berechtigungssätze für den Zugriff auf Azure Files enthalten. Wenn Sie Ihre benutzerdefinierte RBAC-Rolle einer Azure AD-Identität zuweisen, erhält diese Identität Zugriff auf eine Azure-Dateifreigabe gemäß diesen Berechtigungen.

Azure Files unterstützt auch die Beibehaltung, Vererbung und Durchsetzung von [NTFS-DACLs](https://technet.microsoft.com/library/2006.01.howitworksntfs.aspx) für alle Dateien und Verzeichnisse in einer Dateifreigabe. Wenn Sie Daten von einer Dateifreigabe in Azure Files kopieren, oder umgekehrt, können Sie angeben, dass NTFS DACLs beibehalten werden sollen. Auf diese Weise können Sie Sicherungsszenarien unter Verwendung von Azure Files implementieren, wobei Ihre NTFS-DACLs zwischen Ihrer lokalen Dateifreigabe und Ihrer Clouddateifreigabe erhalten bleiben. 

> [!NOTE]
> - Die Azure AD DS-Authentifizierung für den SMB-Zugriff (Server Message Block) wird für Linux-VMs nicht unterstützt. Es werden nur virtuelle Windows-Computer unterstützt.
> - Die Azure AD DS-Authentifizierung für den SMB-Zugriff wird für Computer, die in eine Active Directory-Domäne eingebunden sind, nicht unterstützt. In der Zwischenzeit sollten Sie die [Azure-Dateisynchronisierung](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning) verwenden, um mit der Migration Ihrer Daten zu Azure Files zu beginnen und die Zugriffssteuerung mit Active Directory-Anmeldeinformationen Ihrer lokalen in eine Active Directory-Domäne eingebundenen Computer zu erzwingen. 
> - Die Azure AD DS-Authentifizierung für den SMB-Zugriff ist nur für Speicherkonten verfügbar, die nach dem 24. September 2018 erstellt wurden.
> - Die Azure AD DS-Authentifizierung für den SMB-Zugriff und persistente NTFS-DACLs werden auf Azure-Dateifreigaben, die von der Azure-Dateisynchronisierung verwaltet werden, nicht unterstützt.
> - Die Azure AD DS-Authentifizierung unterstützt nicht die Authentifizierung für Computerkonten, die in Azure AD DS erstellt werden.
