---
title: Massenerstellung von Benutzern (Vorschau) im Azure Active Directory-Portal | Microsoft-Dokumentation
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
ms.openlocfilehash: a10dfffa69652ee2b75053c04b97f6492c46811e
ms.sourcegitcommit: 42748f80351b336b7a5b6335786096da49febf6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2019
ms.locfileid: "72174310"
---
# <a name="bulk-create-users-preview-in-azure-active-directory"></a>Massenerstellung von Benutzern (Vorschau) in Azure Active Directory

Azure Active Directory (Azure AD) unterstützt Vorgänge der Massenerstellung und Massenlöschung von Benutzern, Masseneinladung für Gäste sowie das Herunterladen von Listen von Benutzern, Gruppen und Gruppenmitgliedern.

## <a name="required-permissions"></a>Erforderliche Berechtigungen

Zur Massenerstellung von Benutzern im Verwaltungsportal müssen Sie als globaler Administrator oder Benutzeradministrator angemeldet sein.

## <a name="to-create-users-in-bulk"></a>So erstellen Sie Benutzer in einem Massenvorgang

1. [Melden Sie sich bei ihrer Azure AD-Organisation](https://aad.portal.azure.com) mit einem Konto an, das über Benutzeradministratorberechtigungen in der Organisation verfügt.
1. Wählen Sie in Azure AD **Benutzer** > **Massenerstellung** aus.
1. Wählen Sie auf der Seite **Massenerstellung von Benutzern** die Option **Herunterladen** aus, um eine gültige CSV-Datei mit Benutzereigenschaften herunterzuladen. Fügen Sie dann die Benutzer hinzu, die Sie erstellen möchten.

   ![Wählen Sie eine lokale CSV-Datei aus, in der Sie die Benutzer auflisten, die Sie hinzufügen möchten.](./media/users-bulk-add/upload-button.png)

1. Öffnen Sie die CSV-Datei, und fügen Sie eine Zeile für jeden Benutzer hinzu, den Sie erstellen möchten. Die einzigen erforderlichen Werte sind **Name**, **Benutzerprinzipalname**, **Anfängliches Kennwort** und **Anmeldung blockieren (Ja/Nein)** . Speichern Sie dann die Datei.

   ![Die CSV-Datei enthält Namen und IDs der zu erstellenden Benutzer.](./media/users-bulk-add/add-csv-file.png)

1. Navigieren Sie auf der Seite **Massenerstellung von Benutzern (Vorschau)** unter „CSV-Datei hochladen“ zur entsprechenden Datei. Wenn Sie die Datei auswählen und auf **Senden** klicken, wird mit der Überprüfung der CSV-Datei begonnen.
1. Nach der Überprüfung des Dateiinhalts wird die Meldung **Datei erfolgreich hochgeladen** angezeigt. Wenn Fehler vorliegen, müssen Sie diese beheben, bevor Sie den Auftrag übermitteln können.
1. Wenn Ihre Datei die Überprüfung bestanden hat, wählen Sie **Senden** aus, um den Azure-Massenvorgang zum Importieren der neuen Benutzer zu starten.
1. Nach Abschluss des Importvorgangs wird eine Benachrichtigung mit dem Auftragsstatus des Massenvorgangs angezeigt.

Wenn Fehler auftreten, können Sie die Ergebnisdatei auf der Seite **Ergebnisse von Massenvorgängen** herunterladen und anzeigen. Die Datei enthält den Grund für die einzelnen Fehler.

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

## <a name="bulk-import-service-limits"></a>Diensteinschränkungen beim Massenimport

Jede Massenaktivität zum Erstellen von Benutzern kann bis zu einer Stunde dauern. Dies ermöglicht die Massenerstellung von mindestens 50.000 Benutzern.

## <a name="next-steps"></a>Nächste Schritte

- [Massenlöschung von Benutzern](users-bulk-delete.md)
- [Herunterladen einer Benutzerliste](users-bulk-download.md)
- [Massenwiederherstellung von Benutzern](users-bulk-restore.md)
