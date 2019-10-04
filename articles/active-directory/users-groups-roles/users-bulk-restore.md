---
title: Massenwiederherstellung gelöschter Benutzer (Vorschau) im Azure Active Directory-Portal | Microsoft-Dokumentation
description: Gelöschte Benutzer in einem Massenvorgang im Azure AD Admin Center in Azure Active Directory wiederherstellen
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
ms.openlocfilehash: f53ade09c5e2e7db0499122526a1de482af9378f
ms.sourcegitcommit: 3e7646d60e0f3d68e4eff246b3c17711fb41eeda
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2019
ms.locfileid: "70901350"
---
# <a name="bulk-restore-deleted-users-preview-in-azure-active-directory"></a>Massenwiederherstellung gelöschter Benutzer (Vorschau) in Azure Active Directory

Azure Active Directory (Azure AD) unterstützt Vorgänge der Massenerstellung und Massenlöschung von Benutzern, Masseneinladung für Gäste sowie das Herunterladen von Listen von Benutzern, Gruppen und Gruppenmitgliedern.

## <a name="to-bulk-restore-users"></a>So stellen Sie Benutzer in einem Massenvorgang wieder her

1. [Melden Sie sich bei ihrer Azure AD-Organisation](https://aad.portal.azure.com) mit einem Konto an, das über Benutzeradministratorberechtigungen in der Organisation verfügt.
1. Wählen Sie in Azure AD **Benutzer** > **Gelöscht** aus.
1. Wählen Sie auf der Seite **Gelöschte Benutzer** die Option **Massenwiederherstellung** aus, um eine gültige CSV-Datei mit den Eigenschaften der wiederherzustellenden Benutzer hochzuladen.

   ![Auf der Seite „Gelöschte Benutzer“ den Benutzer „Massenwiederherstellung“ auswählen](./media/users-bulk-restore/bulk-restore.png)

1. Wenn Sie die Bearbeitung der CSV-Datei abgeschlossen haben oder eine Ihrer eigenen Dateien hochladen möchten, wählen Sie die Datei unter **Ihre CSV-Datei hochladen** zur Überprüfung aus.

   ![Wählen Sie eine lokale CSV-Datei aus, in der Sie die Benutzer auflisten, die Sie hinzufügen möchten.](./media/users-bulk-restore/upload-button.png)

1. Wenn der Dateiinhalt überprüft wird, korrigieren Sie die Dateiinformationen, und senden Sie die Datei erneut, falls Fehler auftreten. Durch das Senden einer gültigen Datei wird der Auftrag zum Hochladen von Daten automatisch gestartet.
1. Nachdem die CSV-Datei die Überprüfung bestanden hat, wählen Sie **Senden** aus, um den Azure-Batchauftrag zum Wiederherstellen der Benutzer zu starten. Wenn Fehler auftreten, können Sie die Ergebnisdatei auf der Seite „Ergebnisse von Massenvorgängen“ herunterladen und anzeigen. Die Datei enthält den Grund für die einzelnen Fehler.

## <a name="check-status"></a>Status überprüfen

Sie können den Status aller Ihrer ausstehenden Massenanforderungen auf der Seite **Ergebnisse von Massenvorgängen (Vorschau)** anzeigen.

   ![Überprüfen des Uploadstatus auf der Seite „Ergebnisse von Massenvorgängen“](./media/users-bulk-restore/bulk-center.png)

Als Nächstes können Sie entweder im Azure-Portal oder mithilfe von PowerShell überprüfen, ob die wiederhergestellten Benutzer in der Azure AD-Organisation vorhanden sind.

## <a name="view-restored-users-in-the-azure-portal"></a>Anzeigen wiederhergestellter Benutzer im Azure-Portal

1. [Melden Sie sich beim Azure AD Admin Center](https://aad.portal.azure.com) mit einem Konto an, das über Benutzeradministratorberechtigungen in der Organisation verfügt.
1. Klicken Sie im Navigationsbereich auf **Azure Active Directory**.
1. Wählen Sie unter **Verwalten** die Option **Benutzer** aus.
1. Wählen Sie unter **Anzeigen** die Option **Alle Benutzer** aus, und überprüfen Sie, ob die wiederhergestellten Benutzer aufgelistet werden.

### <a name="view-users-with-powershell"></a>Anzeigen von Benutzern mit PowerShell

Führen Sie den folgenden Befehl aus:

``` PowerShell
Get-AzureADUser -Filter "UserType eq 'Member'"
```

Jetzt sollten die wiederhergestellten Benutzer aufgelistet werden.

## <a name="next-steps"></a>Nächste Schritte

- [Massenimport von Benutzern](users-bulk-add.md)
- [Massenlöschung von Benutzern](users-bulk-delete.md)
- [Herunterladen einer Benutzerliste](users-bulk-download.md)
