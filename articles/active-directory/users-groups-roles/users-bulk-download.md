---
title: Herunterladen einer Liste von Benutzern (Vorschau) im Azure Active Directory-Portal | Microsoft-Dokumentation
description: Laden Sie Benutzerdatensätze in einem Massenvorgang im Azure Admin Center in Azure Active Directory herunter.
services: active-directory
author: curtand
ms.author: curtand
manager: mtillman
ms.date: 07/15/2019
ms.topic: conceptual
ms.service: active-directory
ms.subservice: users-groups-roles
ms.workload: identity
ms.custom: it-pro
ms.reviewer: jeffsta
ms.collection: M365-identity-device-management
ms.openlocfilehash: c59a0b40065be1554e79186227cd2e90d68e7222
ms.sourcegitcommit: 42748f80351b336b7a5b6335786096da49febf6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2019
ms.locfileid: "72174208"
---
# <a name="download-a-list-of-users-preview-in-azure-active-directory-portal"></a>Herunterladen einer Liste von Benutzern (Vorschau) im Azure Active Directory-Portal

Azure Active Directory (Azure AD) unterstützt Massenvorgänge zum Importieren (Erstellen) von Benutzern.

## <a name="required-permissions"></a>Erforderliche Berechtigungen

Um die Liste der Benutzer aus dem Azure AD Admin Center herunterzuladen, müssen Sie als Benutzer angemeldet sein, der einer oder mehreren Administratorrollen auf Organisationsebene in Azure AD zugewiesen ist. Gasteinladender und Anwendungsentwickler werden nicht als Administratorrollen betrachtet.

## <a name="to-download-a-list-of-users"></a>So laden Sie eine Liste von Benutzern herunter

1. [Melden Sie sich bei Ihrer Azure AD-Organisation](https://aad.portal.azure.com) mit einem Benutzeradministratorkonto an.
1. Wählen Sie in Azure AD **Benutzer** > **Benutzer herunterladen** aus.
1. Wählen Sie auf der Seite **Benutzer herunterladen** die Option **Start** aus, um eine CSV-Datei mit der Auflistung von Benutzerprofileigenschaften zu erhalten. Wenn Fehler auftreten, können Sie die Ergebnisdatei auf der Seite „Ergebnisse von Massenvorgängen“ herunterladen und anzeigen. Die Datei enthält den Grund für die einzelnen Fehler.

   ![Wählen Sie den Speicherort aus, wo Sie die Liste der Benutzer herunterladen möchten.](./media/users-bulk-download/bulk-download.png)

## <a name="check-status"></a>Status überprüfen

Sie können den Status Ihrer ausstehenden Massenanforderungen auf der Seite **Ergebnisse von Massenvorgängen (Vorschau)** anzeigen.

   ![Überprüfen des Uploadstatus auf der Seite „Ergebnisse von Massenvorgängen“](./media/users-bulk-download/bulk-center.png)

## <a name="bulk-download-service-limits"></a>Grenzwerte für den Massendownloaddienst

Jede Massenaktivität zum Erstellen einer Liste von Benutzern kann bis zu einer Stunde dauern. Dies ermöglicht das Erstellen und den Download einer Liste von mindestens 500.000 Benutzern.

## <a name="next-steps"></a>Nächste Schritte

- [Benutzer per Massenvorgang hinzufügen](users-bulk-add.md)
- [Massenlöschung von Benutzern](users-bulk-delete.md)
- [Massenwiederherstellung von Benutzern](users-bulk-restore.md)
