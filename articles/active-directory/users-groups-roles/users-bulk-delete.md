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
ms.openlocfilehash: d7c47887c12c8bf9be7a0c5b11dfb3f099965cb7
ms.sourcegitcommit: 42748f80351b336b7a5b6335786096da49febf6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2019
ms.locfileid: "72174384"
---
# <a name="bulk-delete-users-preview-in-azure-active-directory"></a>Massenlöschung von Benutzern (Vorschau) in Azure Active Directory

Über das Azure Active Directory-Portal (Azure AD-Portal) können Sie eine große Anzahl von Mitgliedern aus einer Gruppe entfernen, indem Sie mithilfe einer CSV-Datei (Comma-Separated Values, durch Trennzeichen getrennte Werte) eine Massenlöschung von Benutzern ausführen.

## <a name="to-bulk-delete-users"></a>So löschen Sie Benutzer in einem Massenvorgang

1. [Melden Sie sich bei ihrer Azure AD-Organisation](https://aad.portal.azure.com) mit einem Konto an, das über Benutzeradministratorberechtigungen in der Organisation verfügt.
1. Wählen Sie in Azure AD **Benutzer** > **Massenlöschung** aus.
1. Wählen Sie auf der Seite **Massenlöschung von Benutzern** die Option **Download** aus, um eine gültige CSV-Datei mit Benutzereigenschaften herunterzuladen.

   ![Wählen Sie eine lokale CSV-Datei aus, in der Sie die Benutzer auflisten, die Sie löschen möchten.](./media/users-bulk-delete/bulk-delete.png)

1. Öffnen Sie die CSV-Datei, und fügen Sie eine Zeile für jeden Benutzer hinzu, den Sie löschen möchten. Der einzige erforderliche Wert ist **Benutzerprinzipalname**. Speichern Sie dann die Datei.

   ![Die CSV-Datei enthält Namen und IDs der zu löschenden Benutzer.](./media/users-bulk-delete/delete-csv-file.png)

1. Navigieren Sie auf der Seite **Massenlöschung von Benutzern (Vorschau)** unter **CSV-Datei hochladen** zur entsprechenden Datei. Wenn Sie die Datei auswählen und auf „Senden“ klicken, wird mit der Überprüfung der CSV-Datei begonnen.
1. Nach der Überprüfung des Dateiinhalts wird die Meldung **Datei erfolgreich hochgeladen** angezeigt. Wenn Fehler vorliegen, müssen Sie diese beheben, bevor Sie den Auftrag übermitteln können.
1. Wenn Ihre Datei die Überprüfung bestanden hat, wählen Sie **Senden** aus, um den Azure-Massenvorgang zum Löschen der Benutzer zu starten.
1. Nach Abschluss des Löschvorgangs wird eine Benachrichtigung angezeigt, dass der Massenvorgang erfolgreich abgeschlossen wurde.

Wenn Fehler auftreten, können Sie die Ergebnisdatei auf der Seite **Ergebnisse von Massenvorgängen** herunterladen und anzeigen. Die Datei enthält den Grund für die einzelnen Fehler.

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
