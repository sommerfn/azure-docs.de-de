---
title: Tutorial für das Masseneinladen von Benutzern für die B2B-Zusammenarbeit – Azure Active Directory | Microsoft-Dokumentation
description: In diesem Tutorial erfahren Sie, wie Sie mit PowerShell und einer CSV-Datei Masseneinladungen an externe Benutzer für die Azure AD B2B-Zusammenarbeit senden.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: tutorial
ms.date: 9/19/2019
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.collection: M365-identity-device-management
ms.openlocfilehash: ec1a6ea8f363f2ddd4a9568700d5bff3330443c0
ms.sourcegitcommit: 2ed6e731ffc614f1691f1578ed26a67de46ed9c2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/19/2019
ms.locfileid: "71128726"
---
# <a name="tutorial-bulk-invite-azure-ad-b2b-collaboration-users-preview"></a>Tutorial: Masseneinladen von Benutzern für die Azure AD B2B-Zusammenarbeit (Vorschau)

|     |
| --- |
| In diesem Artikel wird eine öffentliche Previewfunktion von Azure Active Directory beschrieben. Weitere Informationen zu Vorschauversionen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).|
|     |


Wenn Sie Azure Active Directory (Azure AD) für die B2B-Zusammenarbeit mit externen Partnern verwenden, können Sie mehrere Gastbenutzer gleichzeitig in Ihre Organisation einladen. In diesem Tutorial erfahren Sie, wie Sie mit dem Azure-Portal Masseneinladungen an externe Benutzer senden. Das Tutorial umfasst vor allem die folgenden Aufgaben:

> [!div class="checklist"]
> * Vorbereiten einer CSV-Datei mit den Benutzerinformationen und Einladungseinstellungen mithilfe von **Bulk invite users (Preview)** (Massenbenutzereinladung (Vorschau))
> * Hochladen der CSV-Datei in Azure AD
> * Überprüfen, ob Benutzer dem Verzeichnis hinzugefügt wurden

Sollten Sie keine Azure Active Directory-Instanz haben, erstellen Sie zunächst ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F). 

## <a name="prerequisites"></a>Voraussetzungen

Sie benötigen mindestens zwei Test-E-Mail-Konten, an die Sie die Einladungen senden können. Die Konten müssen sich außerhalb Ihrer Organisation befinden. Sie können jede Art von Konto verwenden, einschließlich Konten bei sozialen Netzwerken wie „gmail.com“- oder „outlook.com“-Adressen.

## <a name="invite-guest-users-in-bulk"></a>Masseneinladung von Gastbenutzern

1. Melden Sie sich beim Azure-Portal mit einem Konto an, das über Benutzeradministratorberechtigungen in der Organisation verfügt.
2. Klicken Sie im Navigationsbereich auf **Azure Active Directory**.
3. Wählen Sie unter **Verwalten** die Optionen **Benutzer** > **Masseneinladung** aus.
4. Wählen Sie auf der Seite **Bulk invite users (Preview)** (Massenbenutzereinladung (Vorschau)) die Option **Herunterladen** aus, um eine gültige CSV-Datei mit Einladungseigenschaften zu erhalten.

    ![Downloadschaltfläche für Masseneinladungen](media/tutorial-bulk-invite/bulk-invite-button.png)

5. Öffnen Sie die CSV-Datei, und fügen Sie eine Zeile für jeden Gastbenutzer hinzu. Erforderliche Werte:

   * **E-Mail-Adresse für Einladung**: Der Benutzer, der eine Einladung erhält

   * **Umleitungs-URL**: Die URL, an die der eingeladene Benutzer nach dem Akzeptieren der Einladung weitergeleitet wird

    ![Beispiel für eine CSV-Datei mit eingegebenen Gastbenutzern](media/tutorial-bulk-invite/bulk-invite-csv.png)

   > [!NOTE]
   > Verwenden Sie unter **Benutzerdefinierte Einladungsnachricht** keine Kommas, da die Nachricht sonst nicht erfolgreich analysiert werden kann.

6. Speichern Sie die Datei .
7. Navigieren Sie auf der Seite **Bulk invite users (Preview)** (Massenbenutzereinladung (Vorschau)) unter **CSV-Datei hochladen** zur entsprechenden Datei. Wenn Sie die Datei auswählen, wird mit der Überprüfung der CSV-Datei begonnen. 
8. Nach der Überprüfung des Dateiinhalts wird die Meldung **Datei erfolgreich hochgeladen** angezeigt. Wenn Fehler vorliegen, müssen Sie diese beheben, bevor Sie den Auftrag übermitteln können.
9. Wenn Ihre Datei die Überprüfung bestanden hat, wählen Sie **Senden** aus, um den Azure-Massenvorgang zum Hinzufügen der Einladungen zu starten. 
10. Wählen Sie zum Anzeigen des Auftragsstatus **Klicken Sie hier, um den Status für jeden Vorgang anzuzeigen.** aus. Alternativ können Sie im Abschnitt **Aktivität** die Option **Ergebnisse von Massenvorgängen (Vorschau)** auswählen. Wenn Sie ausführliche Informationen zu jedem Zeilenelement des Massenvorgangs erhalten möchten, wählen Sie die Werte in der Spalte **# Erfolg**, **# Fehler** oder **Anforderungen insgesamt** aus. Wenn Fehler aufgetreten sind, werden die Fehlerursachen aufgeführt.

    ![Beispiel für Ergebnisse von Massenvorgängen](media/tutorial-bulk-invite/bulk-operation-results.png)

11. Nach Abschluss des Vorgangs wird eine Benachrichtigung angezeigt, dass der Massenvorgang erfolgreich abgeschlossen wurde.

## <a name="verify-guest-users-in-the-directory"></a>Überprüfen von Gastbenutzern im Verzeichnis

Überprüfen Sie entweder im Azure-Portal oder mithilfe von PowerShell, ob die hinzugefügten Gastbenutzer im Verzeichnis vorhanden sind.

### <a name="view-guest-users-in-the-azure-portal"></a>Anzeigen von Gastbenutzern im Azure-Portal

1. Melden Sie sich beim Azure-Portal mit einem Konto an, das über Benutzeradministratorberechtigungen in der Organisation verfügt.
2. Klicken Sie im Navigationsbereich auf **Azure Active Directory**.
3. Wählen Sie unter **Verwalten** die Option **Benutzer** aus.
4. Wählen Sie unter **Anzeigen** die Option **Nur Gastbenutzer** aus, und überprüfen Sie, ob die hinzugefügten Benutzer aufgelistet werden.

### <a name="view-guest-users-with-powershell"></a>Anzeigen von Gastbenutzern mit PowerShell

Führen Sie den folgenden Befehl aus:

```powershell
 Get-AzureADUser -Filter "UserType eq 'Guest'"
```

Die von Ihnen eingeladenen Benutzer sollten mit einem Benutzerprinzipalnamen (UPN) im Format *E-Mail-Adresse*#EXT#\@*Domäne* angezeigt werden. Beispiel: *lstokes_fabrikam.com#EXT#\@contoso.onmicrosoft.com*, wobei „contoso.onmicrosoft.com“ der Organisation entspricht, für die Sie die Einladungen gesendet haben.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Testbenutzerkonten nicht mehr benötigen, können Sie sie im Azure-Portal auf der Seite „Benutzer“ aus dem Verzeichnis löschen. Aktivieren Sie dazu das Kontrollkästchen neben dem Gastbenutzer, und wählen Sie dann **Löschen** aus. 

Sie können aber auch den folgenden PowerShell-Befehl ausführen, um ein Benutzerkonto zu löschen:

```powershell
 Remove-AzureADUser -ObjectId "<UPN>"
```

Beispiel: `Remove-AzureADUser -ObjectId "lstokes_fabrikam.com#EXT#@contoso.onmicrosoft.com"`

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Masseneinladungen an Gastbenutzer außerhalb Ihrer Organisation gesendet. Als Nächstes erfahren Sie, wie Einladungen eingelöst werden.

> [!div class="nextstepaction"]
> [Azure Active Directory B2B-Zusammenarbeit: Einlösen von Einladungen](redemption-experience.md)
