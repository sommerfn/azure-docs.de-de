---
title: Einmaliges Anmelden über Link für Azure AD-Anwendungen – Microsoft Identity Platform | Microsoft-Dokumentation
description: Konfigurieren des einmaligen Anmeldens (Single Sign-On, SSO) über Link für Ihre Azure AD-Unternehmensanwendungen auf der Microsoft Identity Platform (Azure AD)
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: article
ms.workload: identity
ms.date: 05/08/2019
ms.author: msmimart
ms.reviewer: arvinh,luleon
ms.collection: M365-identity-device-management
ms.openlocfilehash: a2d9035adb70ab4a5877f3e55dc4115a5ec1f7f5
ms.sourcegitcommit: 198c3a585dd2d6f6809a1a25b9a732c0ad4a704f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2019
ms.locfileid: "68425849"
---
# <a name="configure-linked-sign-on"></a>Konfigurieren der Anmeldung über Link

Wenn Sie eine Katalog- oder Nicht-Katalogwebanwendung hinzufügen, ist [einmaliges Anmelden über Link](what-is-single-sign-on.md) eine der Optionen für einmaliges Anmelden, die Ihnen zur Verfügung stehen. Wählen Sie diese Option aus, um im Azure AD-Zugriffsbereich oder Office 365-Portal Ihrer Organisation einen Link zu einer Anwendung hinzufügen. Mit dieser Methode können Sie Links zu benutzerdefinierten Webanwendungen hinzufügen, die derzeit für die Authentifizierung Active Directory-Verbunddienste (oder andere Verbunddienste) anstelle von Azure AD verwenden. Oder Sie können Deep-Links auf spezifische SharePoint-Seiten oder andere Webseiten hinzufügen, die in den Zugriffsbereichen der betreffenden Benutzer angezeigt werden sollen.

## <a name="before-you-begin"></a>Voraussetzungen

Falls die Anwendung Ihrem Azure AD-Mandanten noch nicht hinzugefügt wurde, helfen Ihnen die Artikel [Hinzufügen einer Katalog-App zu Ihrer Azure AD-Organisation](add-gallery-app.md) bzw. [Hinzufügen einer nicht aufgeführten Anwendung (Nicht-Kataloganwendung) zu Ihrer Azure AD-Organisation](add-non-gallery-app.md) weiter.

### <a name="open-the-app-and-select-linked-sign-on"></a>Öffnen der App und Auswählen des einmaligen Anmeldens über Link

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) als Cloudanwendungsadministrator oder Anwendungsadministrator für Ihren Azure AD-Mandanten an.

1. Navigieren Sie zu **Azure Active Directory** > **Unternehmensanwendungen**. Eine zufällige Stichprobe von Anwendungen in Ihrem Azure AD-Mandanten wird angezeigt. 

1. Wählen Sie im Menü **Anwendungstyp** die Option **Alle Anwendungen** und dann **Übernehmen** aus.

1. Geben Sie den Namen Ihrer Anwendung in das Suchfeld ein, und wählen Sie in den Ergebnissen Ihre Anwendung aus.

1. Wählen Sie im Abschnitt **Verwalten** die Option **Einmaliges Anmelden** aus. 

1. Wählen Sie **Mit Link** aus.

1. Geben Sie die URL der Anwendung ein, mit der eine Verknüpfung hergestellt werden soll. Geben Sie die URL ein, und klicken Sie auf **Speichern**. 
 
1. Sie können der Anwendung Benutzer und Gruppen zuweisen. Dadurch wird die Anwendung im [Office 365-App-Startfeld](https://blogs.office.com/2014/10/16/organize-office-365-new-app-launcher-2/) oder im [Azure AD-Zugriffsbereich](end-user-experiences.md) für die betreffenden Benutzer angezeigt.

1. Wählen Sie **Speichern** aus.

## <a name="next-steps"></a>Nächste Schritte

- [Zuweisen von Benutzern und Gruppen zu einer Anwendung in Azure Active Directory](methods-for-assigning-users-and-groups.md)
- [Verwalten der Benutzerkontobereitstellung für Unternehmens-Apps im Azure-Portal](configure-automatic-user-provisioning-portal.md)
