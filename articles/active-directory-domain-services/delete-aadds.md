---
title: Deaktivieren von Azure Active Directory Domain Services | Microsoft-Dokumentation
description: Deaktivieren von Azure Active Directory Domain Services mithilfe des Azure-Portals
services: active-directory-ds
documentationcenter: ''
author: iainfoulds
manager: daveba
editor: curtand
ms.assetid: 89e407e1-e1e0-49d1-8b89-de11484eee46
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/14/2019
ms.author: iainfou
ms.openlocfilehash: d5917ad94212c8b18d4362528bdfbafb02aec808
ms.sourcegitcommit: ee61ec9b09c8c87e7dfc72ef47175d934e6019cc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/30/2019
ms.locfileid: "70171969"
---
# <a name="disable-azure-active-directory-domain-services-using-the-azure-portal"></a>Deaktivieren von Azure Active Directory Domain Services mithilfe des Azure-Portals
In diesem Artikel wird gezeigt, wie Sie mit dem Azure-Portal Azure Active Directory Domain Services (AD) für Ihr Azure AD-Verzeichnis deaktivieren.

> [!WARNING]
> **Verzeichnisse werden dauerhaft gelöscht, und dieser Vorgang kann nicht rückgängig gemacht werden.**
> Lassen Sie daher Vorsicht walten. Beim Löschen der verwalteten Domäne geschieht Folgendes:
>   * Die Bereitstellung der Domänencontroller für die verwaltete Domäne wird aufgehoben, und die Domänencontroller werden aus dem virtuellen Netzwerk entfernt.
>   * Die Daten in der verwalteten Domäne werden endgültig gelöscht. Dazu gehören u.a. benutzerdefinierte Organisationseinheiten, GPOs, benutzerdefinierte DNS-Einträge, Dienstprinzipale und gMSAs, die Sie in der verwalteten Domäne erstellt haben.
>   * Computer, die in die verwaltete Domäne eingebunden sind, verlieren ihre Vertrauensstellung mit der Domäne und müssen aus der Domäne entfernt werden.
>   * Bei diesen Computern können Sie sich nicht mit Unternehmens-AD-Anmeldeinformationen anmelden. Verwenden Sie stattdessen die Anmeldeinformationen für den lokalen Administrator des Computers.
> Durch das Löschen der verwalteten Domäne wird Ihr Azure AD-Verzeichnis weder gelöscht noch anderweitig beeinträchtigt.

Führen Sie zum Löschen der verwalteten Azure AD Domain Services-Domäne die folgenden Schritte aus:
1. Navigieren Sie im Azure-Portal zur [Azure AD Domain Services-Erweiterung](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.AAD%2FdomainServices).
2. Klicken Sie auf den Namen der verwalteten Domäne.

    ![Auswählen der zu löschenden Domäne](./media/getting-started/domain-services-delete-select-domain.png)

3. Klicken Sie auf der Seite **Übersicht** auf die Schaltfläche **Löschen**.

    ![Löschen der Domäne](./media/getting-started/domain-services-delete-domain.png)

4. Geben Sie zum Bestätigen des Löschvorgangs den DNS-Domänennamen der verwalteten Domäne ein. Klicken Sie anschließend auf die Schaltfläche **Löschen**.

    ![Bestätigung zum Löschen der Domäne](./media/getting-started/domain-services-delete-domain-confirm.png)

Das Löschen der verwalteten Domäne kann zwischen 15 und 20 Minuten (oder länger) dauern.

Wir würden uns über [Ihr Feedback](contact-us.md) freuen, damit wir besser verstehen, welche Features Sie benötigen, um sich in Zukunft für die Azure AD Domain Services zu entscheiden. Dieses Feedback hilft uns dabei, den Dienst weiter zu verbessern und an die Anforderungen Ihrer Bereitstellungen und Anwendungsfälle anzupassen.
