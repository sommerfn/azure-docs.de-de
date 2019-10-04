---
title: Azure AD – Aktivieren des Kennwortrückschreibens
description: In diesem Tutorial aktivieren Sie das Kennwortrückschreiben, um im Rahmen von Azure AD Connect über die Cloud initiierte Kennwortänderungen zurück in die lokale AD-Instanz zu schreiben.
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: tutorial
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1efb67df6c31a3b03fdc45fffc0564fb09e39faf
ms.sourcegitcommit: 470041c681719df2d4ee9b81c9be6104befffcea
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2019
ms.locfileid: "67853040"
---
# <a name="tutorial-enabling-password-writeback"></a>Tutorial: Aktivieren des Kennwortrückschreibens

In diesem Tutorial aktivieren Sie das Kennwortrückschreiben für Ihre Hybridumgebung. Das Kennwortrückschreiben wird genutzt, um Kennwortänderungen in Azure Active Directory (Azure AD) mit Ihrer lokalen Active Directory Domain Services-Umgebung (AD DS) zu synchronisieren. Das Kennwortrückschreiben wird im Rahmen von Azure AD Connect aktiviert, um einen sicheren Mechanismus zum Zurücksenden von Kennwortänderungen in ein vorhandenes lokales Verzeichnis aus Azure AD bereitzustellen. Ausführlichere Informationen zur Funktionsweise des Kennwortrückschreibens finden Sie im Artikel [Was ist die Kennwortrückschreibung?](concept-sspr-writeback.md).

> [!div class="checklist"]
> * Aktivieren der Option für das Kennwortrückschreiben in Azure AD Connect
> * Aktivieren der Option für das Kennwortrückschreiben bei der Self-Service-Kennwortzurücksetzung (Self-Service Password Reset, SSPR)

## <a name="prerequisites"></a>Voraussetzungen

* Zugriff auf einen funktionierenden Azure AD-Mandanten mit mindestens einer zugewiesenen Testlizenz.
* Eine Konto mit Rechten vom Typ „Globaler Administrator“ in Ihrem Azure AD-Mandanten.
* Ein vorhandener Server, der für die Ausführung einer aktuellen Version von [Azure AD Connect](../hybrid/how-to-connect-install-express.md) konfiguriert ist.
* Die vorherigen Tutorials zur Self-Service-Kennwortzurücksetzung wurden abgeschlossen.

## <a name="enable-password-writeback-option-in-azure-ad-connect"></a>Aktivieren der Option für das Kennwortrückschreiben in Azure AD Connect

Zum Ermöglichen des Kennwortrückschreibens müssen wir zuerst das Feature auf dem Server aktivieren, auf dem Sie Azure AD Connect installiert haben.

1. Um das Kennwortrückschreiben zu konfigurieren und zu aktivieren, melden Sie sich bei Ihrem Azure AD Connect-Server an, und starten Sie den **Azure AD Connect**-Konfigurations-Assistenten.
2. Wählen Sie auf der Seite **Willkommen** die Option **Konfigurieren** aus.
3. Wählen Sie auf der Seite **Weitere Aufgaben** die Option **Synchronisierungsoptionen anpassen** aus, und wählen Sie dann **Weiter** aus.
4. Geben Sie auf der Seite **Mit Azure AD verbinden** die Anmeldeinformationen eines globalen Administrators ein, und wählen Sie dann **Weiter** aus.
5. Wählen Sie auf den Seiten **Verzeichnisse verbinden** und **Domänen-/OE-Filterung** die Schaltfläche **Weiter** aus.
6. Aktivieren Sie auf der Seite **Optionale Features** das Kontrollkästchen neben **Kennwortrückschreiben**, und wählen Sie **Weiter** aus.
7. Wählen Sie auf der Seite **Bereit zur Konfiguration** die Option **Konfigurieren** aus, und warten Sie, bis der Vorgang abgeschlossen ist.
8. Wenn Sie sehen, dass die Konfiguration beendet ist, wählen Sie **Beenden** aus.

## <a name="enable-password-writeback-option-in-sspr"></a>Aktivieren der Option für das Kennwortrückschreiben bei SSPR

Die Aktivierung des Features für das Kennwortrückschreiben in Azure AD Connect ist nur die eine Seite. Wenn für SSPR die Nutzung des Kennwortrückschreibens zugelassen wird, schließt sich der Kreis. Auf diese Weise können Benutzer, die ihr Kennwort ändern oder zurücksetzen, dieses Kennwort auch lokal festlegen lassen.

1. Melden Sie sich mit dem Konto eines globalen Administrators am [Azure-Portal](https://portal.azure.com) an.
2. Navigieren Sie zu **Azure Active Directory**, klicken Sie auf **Zurücksetzen des Kennworts**, und wählen Sie dann **Lokale Integration**.
3. Legen Sie die Option **Kennwörter in Ihr lokales Verzeichnis zurückschreiben?** auf **Ja** fest.
4. Legen Sie die Option **Benutzern das Entsperren von Konten ohne Zurücksetzen des Kennworts erlauben** auf **Ja** fest.
5. Klicken Sie unten auf der Seite auf **Speichern**.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie das Kennwortrückschreiben für die Self-Service-Kennwortzurücksetzung aktiviert. Lassen Sie das Fenster des Azure-Portals geöffnet, und fahren Sie mit dem nächsten Tutorial fort, um zusätzliche Einstellungen zur Self-Service-Kennwortzurücksetzung zu konfigurieren, bevor Sie für die Lösung den Rollout in einem Pilotprojekt durchführen.

> [!div class="nextstepaction"]
> [Tutorial: Use risk events to trigger Multi-Factor Authentication and password changes](tutorial-risk-based-sspr-mfa.md) (Tutorial: Auslösen von Multi-Factor Authentication und Kennwortänderungen mithilfe von Risikoereignissen)
