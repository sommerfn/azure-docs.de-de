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
ms.openlocfilehash: d392ae97a8325dd4a56acd807ebfb2b951216eae
ms.sourcegitcommit: 42748f80351b336b7a5b6335786096da49febf6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2019
ms.locfileid: "72174245"
---
# <a name="bulk-restore-deleted-users-preview-in-azure-active-directory"></a>Massenwiederherstellung gelöschter Benutzer (Vorschau) in Azure Active Directory

Azure Active Directory (Azure AD) unterstützt Vorgänge der Massenerstellung und Massenlöschung von Benutzern, Masseneinladung für Gäste sowie das Herunterladen von Listen von Benutzern, Gruppen und Gruppenmitgliedern.

## <a name="to-bulk-restore-users"></a>So stellen Sie Benutzer in einem Massenvorgang wieder her

1. [Melden Sie sich bei ihrer Azure AD-Organisation](https://aad.portal.azure.com) mit einem Konto an, das über Benutzeradministratorberechtigungen in der Azure AD-Organisation verfügt.
1. Wählen Sie in Azure AD **Benutzer** > **Gelöscht** aus.
1. Wählen Sie auf der Seite **Gelöschte Benutzer** die Option **Massenwiederherstellung** aus, um eine gültige CSV-Datei mit den Eigenschaften der wiederherzustellenden Benutzer hochzuladen.

   ![Auf der Seite „Gelöschte Benutzer“ den Benutzer „Massenwiederherstellung“ auswählen](./media/users-bulk-restore/bulk-restore.png)

1. Öffnen Sie die CSV-Datei, und fügen Sie eine Zeile für jeden Benutzer hinzu, den Sie wiederherstellen möchten. Der einzige erforderliche Wert ist **ObjectID**. Speichern Sie dann die Datei.

   ![Wählen Sie eine lokale CSV-Datei aus, in der Sie die Benutzer auflisten, die Sie hinzufügen möchten.](./media/users-bulk-restore/upload-button.png)

1. Navigieren Sie auf der Seite **Massenwiederherstellung von Benutzern (Vorschau)** unter **CSV-Datei hochladen** zur entsprechenden Datei. Wenn Sie die Datei auswählen und auf **Senden** klicken, wird mit der Überprüfung der CSV-Datei begonnen.
1. Nach der Überprüfung des Dateiinhalts wird die Meldung **Datei erfolgreich hochgeladen** angezeigt. Wenn Fehler vorliegen, müssen Sie diese beheben, bevor Sie den Auftrag übermitteln können.
1. Wenn Ihre Datei die Überprüfung bestanden hat, wählen Sie **Senden** aus, um den Azure-Massenvorgang zum Wiederherstellen der Benutzer zu starten.
1. Nach Abschluss des Wiederherstellungsvorgangs wird eine Benachrichtigung angezeigt, dass der Massenvorgang erfolgreich abgeschlossen wurde.

Wenn Fehler auftreten, können Sie die Ergebnisdatei auf der Seite **Ergebnisse von Massenvorgängen** herunterladen und anzeigen. Die Datei enthält den Grund für die einzelnen Fehler.

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
