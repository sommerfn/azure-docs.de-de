---
title: Massenlöschung von Benutzern (Vorschau) im Azure Active Directory-Portal | Microsoft-Dokumentation
description: Löschen Sie Benutzer in einem Massenvorgang im Azure Admin Center in Azure Active Directory.
services: active-directory
author: curtand
ms.author: curtand
manager: mtillman
ms.date: 08/15/2019
ms.topic: conceptual
ms.service: active-directory
ms.subservice: users-groups-roles
ms.workload: identity
ms.custom: it-pro
ms.reviewer: jeffsta
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6c2204c572ca1f74f8060d0b6176df69359fe69a
ms.sourcegitcommit: 3e7646d60e0f3d68e4eff246b3c17711fb41eeda
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2019
ms.locfileid: "70901378"
---
# <a name="bulk-delete-users-preview-in-azure-active-directory"></a>Massenlöschung von Benutzern (Vorschau) in Azure Active Directory

Azure Active Directory (Azure AD) unterstützt Vorgänge der Massenerstellung und Massenlöschung von Benutzern, Masseneinladung für Gäste sowie das Herunterladen von Listen von Benutzern, Gruppen und Gruppenmitgliedern.

## <a name="to-bulk-delete-users"></a>So löschen Sie Benutzer in einem Massenvorgang

1. Melden Sie sich bei ihrer Azure AD-Organisation mit einem Konto an, das über Benutzeradministratorberechtigungen in der Organisation verfügt.
1. Wählen Sie in Azure AD **Benutzer** > **Massenlöschung** aus.
1. Wählen Sie auf der Seite **Benutzer für Massenlöschung** die Option **Download** aus, um eine gültige CSV-Datei mit Benutzereigenschaften herunterzuladen. Fügen Sie dann die Benutzer hinzu, die Sie löschen möchten.
1. Wenn Sie die Bearbeitung der CSV-Datei abgeschlossen haben oder eine Ihrer eigenen Dateien hochladen möchten, wählen Sie die Datei unter **Ihre CSV-Datei hochladen** zur Überprüfung aus.

   ![Wählen Sie eine lokale CSV-Datei aus, in der Sie die Benutzer auflisten, die Sie löschen möchten.](./media/users-bulk-delete/bulk-delete.png)

1. Wenn der Dateiinhalt überprüft wird, müssen Sie alle Fehler beheben, bevor der Auftrag gesendet wird.
1. Wenn Ihre Datei die Überprüfung bestanden hat, wählen Sie **Senden** aus, um den Azure-Batchauftrag zum Löschen der Benutzer zu starten. Wenn Fehler auftreten, können Sie die Ergebnisdatei auf der Seite „Ergebnisse von Massenvorgängen“ herunterladen und anzeigen. Die Datei enthält den Grund für die einzelnen Fehler.

## <a name="check-status"></a>Status überprüfen

Sie können den Status aller Ihrer ausstehenden Massenanforderungen auf der Seite **Ergebnisse von Massenvorgängen (Vorschau)** anzeigen.

   ![Überprüfen des Uploadstatus auf der Seite „Ergebnisse von Massenvorgängen“](./media/users-bulk-delete/bulk-center.png)

Als Nächstes können Sie entweder im Azure-Portal oder mithilfe von PowerShell überprüfen, ob die gelöschten Benutzer in der Azure AD-Organisation vorhanden sind.

## <a name="verify-deleted-users-in-the-azure-portal"></a>Überprüfen gelöschter Benutzer im Azure-Portal

1. Melden Sie sich beim Azure-Portal mit einem Konto an, das über Benutzeradministratorberechtigungen in der Organisation verfügt.
1. Klicken Sie im Navigationsbereich auf **Azure Active Directory**.
1. Wählen Sie unter **Verwalten** die Option **Benutzer** aus.
1. Wählen Sie unter **Anzeigen** nur **Alle Benutzer** aus, und vergewissern Sie sich, dass die gelöschten Benutzer nicht mehr aufgelistet werden.

### <a name="verify-deleted-users-with-powershell"></a>Überprüfen gelöschter Benutzer mit PowerShell

Führen Sie den folgenden Befehl aus:

``` PowerShell
Get-AzureADUser -Filter "UserType eq 'Member'"
```

Vergewissern Sie sich, dass die gelöschten Benutzer nicht mehr aufgelistet werden.

## <a name="next-steps"></a>Nächste Schritte

- [Benutzer per Massenvorgang hinzufügen](users-bulk-add.md)
- [Herunterladen einer Benutzerliste](users-bulk-download.md)
- [Massenwiederherstellung von Benutzern](users-bulk-restore.md)
