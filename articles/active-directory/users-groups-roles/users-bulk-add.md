---
title: Massenimport zum Hinzufügen von Benutzern (Vorschau) im Azure Active Directory-Portal | Microsoft-Dokumentation
description: Fügen Sie Benutzer in einem Massenvorgang im Azure Admin Center in Azure Active Directory hinzu.
services: active-directory
author: curtand
ms.author: curtand
manager: mtillman
ms.date: 08/30/2019
ms.topic: article
ms.service: active-directory
ms.subservice: users-groups-roles
ms.workload: identity
ms.custom: it-pro
ms.reviewer: jeffsta
ms.collection: M365-identity-device-management
ms.openlocfilehash: a22052117448cdb71bbc16e1df7899e6d3eb764e
ms.sourcegitcommit: 3e7646d60e0f3d68e4eff246b3c17711fb41eeda
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2019
ms.locfileid: "70901310"
---
# <a name="bulk-import-users-preview-in-azure-active-directory"></a>Massenimport von Benutzern (Vorschau) in Azure Active Directory

Azure Active Directory (Azure AD) unterstützt Vorgänge der Massenerstellung und Massenlöschung von Benutzern, Masseneinladung für Gäste sowie das Herunterladen von Listen von Benutzern, Gruppen und Gruppenmitgliedern.

## <a name="bulk-import-service-limits"></a>Diensteinschränkungen beim Massenimport

Jede Massenaktivität zum Erstellen von Benutzern kann bis zu einer Stunde dauern. Dies ermöglicht die Massenerstellung von mindestens 50.000 Benutzern.

## <a name="required-permissions"></a>Erforderliche Berechtigungen

Zur Massenerstellung von Benutzern im Verwaltungsportal müssen Sie als globaler Administrator oder Benutzeradministrator angemeldet sein.

## <a name="to-bulk-import-users"></a>So führen Sie den Massenimport von Benutzern durch

1. [Melden Sie sich bei ihrer Azure AD-Organisation](https://aad.portal.azure.com) mit einem Konto an, das über Benutzeradministratorberechtigungen in der Organisation verfügt.
1. Wählen Sie in Azure AD **Benutzer** > **Massenerstellung** aus.
1. Wählen Sie auf der Seite **Benutzer für Massenerstellung** die Option **Herunterladen** aus, um eine gültige CSV-Datei mit Benutzereigenschaften herunterzuladen. Fügen Sie dann die neuen Benutzer hinzu.
1. Wenn Sie die Bearbeitung der CSV-Datei abgeschlossen haben oder eine Ihrer eigenen Dateien hochladen möchten, wählen Sie die Datei unter **Ihre CSV-Datei hochladen** zur Überprüfung aus.

   ![Wählen Sie eine lokale CSV-Datei aus, in der Sie die Benutzer auflisten, die Sie hinzufügen möchten.](./media/users-bulk-add/upload-button.png)

1. Wenn der Dateiinhalt überprüft wird, müssen Sie alle Fehler beheben, bevor Sie den Uploadauftrag starten können.
1. Wenn Ihre Datei die Überprüfung bestanden hat, wählen Sie **Senden** aus, um den Azure-Batchauftrag zum Hinzufügen der neuen Benutzerinformationen zu starten. Wenn Fehler auftreten, können Sie die Ergebnisdatei auf der Seite „Ergebnisse von Massenvorgängen“ herunterladen und anzeigen. Die Datei enthält den Grund für die einzelnen Fehler.

## <a name="check-status"></a>Status überprüfen

Sie können den Status aller Ihrer ausstehenden Massenanforderungen auf der Seite **Ergebnisse von Massenvorgängen (Vorschau)** anzeigen.

   ![Überprüfen des Uploadstatus auf der Seite „Ergebnisse von Massenvorgängen“](./media/users-bulk-add/bulk-center.png)

Als Nächstes können Sie entweder im Azure-Portal oder mithilfe von PowerShell überprüfen, ob die erstellten Benutzer in der Azure AD-Organisation vorhanden sind.

## <a name="verify-users-in-the-azure-portal"></a>Überprüfen von Benutzern im Azure-Portal

1. [Melden Sie sich beim Azure AD Admin Center](https://aad.portal.azure.com) mit einem Konto an, das über Benutzeradministratorberechtigungen in der Organisation verfügt.
1. Klicken Sie im Navigationsbereich auf **Azure Active Directory**.
1. Wählen Sie unter **Verwalten** die Option **Benutzer** aus.
1. Wählen Sie unter **Anzeigen** die Option **Alle Benutzer** aus, und überprüfen Sie, ob die erstellten Benutzer aufgelistet werden.

### <a name="verify-users-with-powershell"></a>Überprüfen von Benutzern mit PowerShell

Führen Sie den folgenden Befehl aus:

``` PowerShell
Get-AzureADUser -Filter "UserType eq 'Member'"
```

Jetzt sollten die erstellten Benutzer aufgelistet werden.

## <a name="next-steps"></a>Nächste Schritte

- [Massenlöschung von Benutzern](users-bulk-delete.md)
- [Herunterladen einer Benutzerliste](users-bulk-download.md)
- [Massenwiederherstellung von Benutzern](users-bulk-restore.md)
