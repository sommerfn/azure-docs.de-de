---
title: include file
description: include file
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 06/18/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: ff2ed5abbf2ce67a0b96a5da450b83832403db1f
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2019
ms.locfileid: "67269361"
---
[Azure Files](../articles/storage/files/storage-files-introduction.md) unterstützt identitätsbasierte Authentifizierung über SMB (Server Message Block) (Vorschau) über [Azure Active Directory (Azure AD) Domain Services](../articles/active-directory-domain-services/overview.md). Ihre in die Domäne eingebundenen virtuellen Windows-Computer (VMs) können mit [Azure AD](../articles/active-directory/fundamentals/active-directory-whatis.md)-Anmeldeinformationen auf Azure-Dateifreigaben zugreifen. 

Mit der [rollenbasierte Zugriffssteuerung (RBAC)](../articles/role-based-access-control/overview.md) können Sie die Azure Files-Freigabeebene für eine Identität wie einen Benutzer oder eine Gruppe in Azure AD festlegen. Sie können benutzerdefinierte RBAC-Rollen definieren, die gängige Berechtigungssätze für den Zugriff auf Azure Files enthalten. Wenn Sie Ihre benutzerdefinierte RBAC-Rolle einer Azure AD-Identität zuweisen, erhält diese Identität Zugriff auf eine Azure-Dateifreigabe gemäß diesen Berechtigungen.

Als Teil der Preview unterstützt Azure Files auch die Beibehaltung, Vererbung und Durchsetzung von [NTFS DACLs](https://technet.microsoft.com/library/2006.01.howitworksntfs.aspx) für alle Dateien und Verzeichnisse in einer Dateifreigabe. Wenn Sie Daten von einer Dateifreigabe in Azure Files kopieren, oder umgekehrt, können Sie angeben, dass NTFS DACLs beibehalten werden sollen. Auf diese Weise können Sie Sicherungsszenarien unter Verwendung von Azure Files implementieren, wobei Ihre NTFS DACLS zwischen Ihrer lokalen Dateifreigabe und Ihrer Clouddateifreigabe erhalten bleiben. 

> [!NOTE]
> - Die Azure AD Domain Services-Authentifizierung über SMB wird für Linux-VMs nicht unterstützt. Es werden nur virtuelle Windows-Computer unterstützt.
> - Die Azure AD Domain Services-Authentifizierung über SMB wird für Computer, die in eine Active Directory Domain Services-Domäne eingebunden sind, nicht unterstützt. In der Zwischenzeit können Sie mit der [Azure-Dateisynchronisierung](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning) Daten zu Azure Files migrieren und weiterhin die Zugriffssteuerung mit AD-Anmeldeinformationen Ihrer lokalen, in die AD-Domäne eingebundenen Computer erzwingen. 
> - Die Azure AD Domain Services-Authentifizierung über SMB ist nur für Speicherkonten verfügbar, die nach dem 24. September 2018 erstellt wurden.
> - Die Azure AD Domain Services-Authentifizierung über SMB und die persistente NTFS-DACL werden auf Azure-Dateifreigaben, die von der Azure-Dateisynchronisierung verwaltet werden, nicht unterstützt. 
