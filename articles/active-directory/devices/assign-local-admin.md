---
title: Verwalten der lokalen Administratorgruppe auf in Azure AD eingebundenen Geräten | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Azure-Rollen zur lokalen Administratorgruppe eines Windows-Geräts hinzufügen.
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: conceptual
ms.date: 06/28/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: ravenn
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5372bf853092ea55952b45a1300d0f979f1b086c
ms.sourcegitcommit: f7f70c9bd6c2253860e346245d6e2d8a85e8a91b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73062256"
---
# <a name="how-to-manage-the-local-administrators-group-on-azure-ad-joined-devices"></a>Verwalten der lokalen Administratorgruppe auf in Azure AD eingebundenen Geräten

Um ein Windows-Gerät verwalten zu können, müssen Sie Mitglied der lokalen Administratorgruppe sein. Im Rahmen der Einbindung in Azure Active Directory (Azure AD) wird die Mitgliedschaft dieser Gruppe auf einem Gerät von Azure AD aktualisiert. Sie können die Mitgliedschaftsaktualisierung Ihren geschäftlichen Anforderungen entsprechend anpassen. Eine Mitgliedschaftsaktualisierung ist beispielsweise hilfreich, wenn Sie Helpdeskmitarbeitern die Ausführung von Aufgaben ermöglichen möchten, für die Administratorrechte erforderlich sind.

In diesem Artikel wird erläutert, wie die Aktualisierung der Mitgliedschaft funktioniert und wie Sie sie während der Einbindung in Azure AD anpassen können. Der Inhalt dieses Artikels gilt nicht für eine **hybride** Azure AD-Einbindung.

## <a name="how-it-works"></a>So funktioniert's

Wenn Sie mithilfe einer Azure AD-Einbindung eine Verbindung zwischen einem Windows-Gerät und Azure AD herstellen, fügt Azure AD die folgenden Sicherheitsprinzipale zur lokalen Administratorgruppe auf dem Gerät hinzu:

- Globale Azure AD-Administratorrolle
- Azure AD-Geräteadministratorrolle 
- Benutzer, der die Azure AD-Einbindung ausführt   

Indem Sie Azure AD-Rollen zur lokalen Administratorgruppe hinzufügen, können Sie die Benutzer, die ein Gerät verwalten können, jederzeit in Azure AD aktualisieren, ohne Änderungen auf dem Gerät vornehmen zu müssen. Derzeit können Sie einer Administratorrolle keine Gruppen zuweisen.
Azure AD fügt der lokalen Administratorgruppe darüber hinaus die Rolle des Azure AD-Geräteadministrators hinzu, um das Prinzip der geringsten Berechtigung (Principle of Least Privilege, PoLP) zu unterstützen. Zusätzlich zu den globalen Administratoren können Sie auch Benutzern, denen *nur* die Geräteadministratorrolle zugewiesen wurde, die Verwaltung eines Geräts ermöglichen. 

## <a name="manage-the-global-administrators-role"></a>Verwalten der globalen Administratorrolle

Informationen zum Anzeigen und Aktualisieren der Mitgliedschaft in der globalen Administratorrolle finden Sie in den folgenden Artikeln:

- [Anzeigen von Mitgliedern und Beschreibungen von Administratorrollen in Azure Active Directory](../users-groups-roles/directory-manage-roles-portal.md)
- [Zuweisen eines Benutzers zu Administratorrollen in Azure Active Directory](../fundamentals/active-directory-users-assign-role-azure-portal.md)


## <a name="manage-the-device-administrator-role"></a>Verwalten der Geräteadministratorrolle 

Die Geräteadministratorrolle kann im Azure-Portal auf der Seite **Geräte** verwaltet werden. So öffnen Sie die Seite **Geräte**:

1. Melden Sie sich als globaler Administrator oder Geräteadministrator beim [Azure-Portal](https://portal.azure.com) an.
1. Klicken Sie auf der linken Navigationsleiste auf **Azure Active Directory**. 
1. Klicken Sie im Bereich **Verwalten** auf **Geräte**.
1. Klicken Sie auf der Seite **Geräte** auf **Geräteeinstellungen**.

Konfigurieren Sie die Einstellung **Weitere lokale Administratoren für in Azure AD eingebundene Geräte**, um die Geräteadministratorrolle anzupassen.  

![Weitere lokale Administratoren](./media/assign-local-admin/10.png)

>[!NOTE]
> Für diese Option ist ein Azure AD Premium-Mandant erforderlich. 

Geräteadministratoren werden allen in Azure AD eingebundenen Geräten zugewiesen. Sie können Geräteadministratoren nicht auf eine bestimmte Gerätegruppe beschränken. Die Aktualisierung der Geräteadministratorrolle wirkt sich nicht unbedingt unmittelbar auf die betroffenen Benutzer aus. Auf Geräten, auf denen bereits ein Benutzer angemeldet ist, wird die Berechtigungsaktualisierung durchgeführt, wenn die *beiden* folgenden Voraussetzungen erfüllt sind:

- Azure AD hatte vier Stunden Zeit, ein neues primäres Aktualisierungstoken mit den entsprechenden Berechtigungen auszustellen. 
- Die Benutzer melden sich ab und wieder an (kein Sperren/Entsperren), um ihr Profil zu aktualisieren.

## <a name="manage-regular-users"></a>Verwalten der regulären Benutzer

Azure AD fügt den Benutzer, der die Azure AD-Einbindung durchführt, der Administratorgruppe auf dem Gerät hinzu. Wenn Sie verhindern möchten, dass reguläre Benutzer lokale Administratoren werden, haben Sie folgende Optionen:

- [Windows Autopilot:](https://docs.microsoft.com/windows/deployment/windows-autopilot/windows-10-autopilot) Mit Windows Autopilot können Sie verhindern, dass ein primärer Benutzer, der die Einbindung ausführt, lokaler Administrator wird. Das können Sie durch [Erstellen eines Autopilot-Profils](https://docs.microsoft.com/intune/enrollment-autopilot#create-an-autopilot-deployment-profile) erreichen.
- [Massenregistrierung:](https://docs.microsoft.com/intune/windows-bulk-enroll) Eine im Kontext einer Massenregistrierung durchgeführte Azure AD-Einbindung erfolgt im Kontext eines automatisch erstellten Benutzers. Benutzer, die sich nach der Einbindung eines Geräts anmelden, werden nicht zur Administratorgruppe hinzugefügt.   

## <a name="manually-elevate-a-user-on-a-device"></a>Manuelles Erhöhen der Berechtigungen eines Benutzers auf einem Gerät 

Sie können nicht nur den Prozess zur Azure AD-Einbindung nutzen, sondern auch die Berechtigungen eines regulären Benutzers manuell erhöhen, sodass er lokaler Administrator auf einem bestimmten Gerät wird. Um diesen Schritt ausführen zu können, müssen Sie bereits Mitglied der lokalen Administratorgruppe sein. 

Ab dem Release **Windows 10 1709** können Sie diese Aufgabe über **Einstellungen > Konten > Andere Benutzer** ausführen. Wählen Sie **Add a work or school user** (Geschäfts-, Schul- oder Unibenutzer hinzufügen) aus, und geben Sie den Benutzerprinzipalnamen (UPN) des Benutzers unter **Benutzerkonto** ein. Wählen Sie unter *Kontotyp* die Option **Administrator** aus.  
 
Darüber hinaus können Sie Benutzer auch über die Eingabeaufforderung hinzufügen:

- Wenn Ihre Mandantenbenutzer aus der lokalen Active Directory-Umgebung synchronisiert werden, verwenden Sie `net localgroup administrators /add "Contoso\username"`.
- Wenn Ihre Mandantenbenutzer in Azure AD erstellt werden, verwenden Sie `net localgroup administrators /add "AzureAD\UserUpn"`.

## <a name="considerations"></a>Überlegungen 

Sie können der Geräteadministratorrolle nur einzelne Benutzer, aber keine Gruppen zuweisen.

Geräteadministratoren werden allen in Azure AD eingebundenen Geräten zugewiesen. Sie können sie nicht auf eine bestimmte Gruppe von Geräten begrenzen.

Wenn Sie Benutzer aus der Geräteadministratorrolle entfernen, verfügen sie weiterhin über die Berechtigung eines lokalen Administrators auf einem Gerät, solange sie bei ihm angemeldet sind. Die Berechtigung wird bei der nächsten Anmeldung oder nach vier Stunden bei Ausgabe eines neuen primären Aktualisierungstokens aufgehoben.

## <a name="next-steps"></a>Nächste Schritte

- Einen Überblick über die Verwaltung von Geräten im Azure-Portal finden Sie unter [Managing devices using the Azure portal - preview](device-management-azure-portal.md) (Verwalten von Geräten mit dem Azure-Portal – Vorschauversion).
- Weitere Informationen zum gerätebasierten bedingten Zugriff finden Sie unter [Konfigurieren des gerätebasierten bedingten Zugriffs für Azure Active Directory](../conditional-access/require-managed-devices.md).
