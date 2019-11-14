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
ms.openlocfilehash: d106e803fdef4f054b7e4717ba59a43359d1d68a
ms.sourcegitcommit: bc7725874a1502aa4c069fc1804f1f249f4fa5f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73720288"
---
# <a name="download-a-list-of-users-preview-in-azure-active-directory-portal"></a>Herunterladen einer Liste von Benutzern (Vorschau) im Azure Active Directory-Portal

Azure Active Directory (Azure AD) unterstützt Massenvorgänge zum Importieren (Erstellen) von Benutzern.

## <a name="required-permissions"></a>Erforderliche Berechtigungen

Um die Liste der Benutzer aus dem Azure AD Admin Center herunterzuladen, müssen Sie als Benutzer angemeldet sein, der einer oder mehreren Administratorrollen auf Organisationsebene in Azure AD zugewiesen ist. Gasteinladender und Anwendungsentwickler werden nicht als Administratorrollen betrachtet.

## <a name="to-download-a-list-of-users"></a>So laden Sie eine Liste von Benutzern herunter

1. [Melden Sie sich bei Ihrer Azure AD-Organisation](https://aad.portal.azure.com) mit einem Benutzeradministratorkonto an.
2. Navigieren Sie zu Azure Active Directory > Benutzer. Wählen Sie dann die Benutzer aus, die Sie in den Download einbeziehen möchten, indem Sie in der linken Spalte neben den einzelnen Benutzern das Kontrollkästchen aktivieren. Hinweis: Derzeit gibt es keine Möglichkeit, alle Benutzer für den Export auszuwählen. Jeder muss einzeln ausgewählt werden.
3. Wählen Sie in Azure AD **Benutzer** > **Benutzer herunterladen** aus.
4. Wählen Sie auf der Seite **Benutzer herunterladen** die Option **Start** aus, um eine CSV-Datei mit der Auflistung von Benutzerprofileigenschaften zu erhalten. Wenn Fehler auftreten, können Sie die Ergebnisdatei auf der Seite „Ergebnisse von Massenvorgängen“ herunterladen und anzeigen. Die Datei enthält den Grund für die einzelnen Fehler.

   ![Wählen Sie den Speicherort aus, wo Sie die Liste der Benutzer herunterladen möchten.](./media/users-bulk-download/bulk-download.png)

   Die Downloaddatei enthält die gefilterte Benutzerliste.

   Enthalten sind die folgenden Benutzerattribute: 

   - userPrincipalName
   - displayName
   - surname
   - mail
   - givenName
   - objectId
   - userType
   - jobTitle
   - department
   - manager
   - accountEnabled
   - usageLocation
   - streetAddress
   - state
   - country
   - physicalDeliveryOfficeName
   - city
   - postalCode
   - telephoneNumber
   - mobile
   - authenticationPhoneNumber
   - authenticationAlternativePhoneNumber
   - AuthenticationEmail
   - alternateEmailAddress
   - ageGroup
   - consentProvidedForMinor
   - legalAgeGroupClassification

## <a name="check-status"></a>Status überprüfen

Sie können den Status Ihrer ausstehenden Massenanforderungen auf der Seite **Ergebnisse von Massenvorgängen (Vorschau)** anzeigen.

   ![Überprüfen des Uploadstatus auf der Seite „Ergebnisse von Massenvorgängen“](./media/users-bulk-download/bulk-center.png)

## <a name="bulk-download-service-limits"></a>Grenzwerte für den Massendownloaddienst

Jede Massenaktivität zum Erstellen einer Liste von Benutzern kann bis zu einer Stunde dauern. Dies ermöglicht das Erstellen und den Download einer Liste von mindestens 500.000 Benutzern.

## <a name="next-steps"></a>Nächste Schritte

- [Benutzer per Massenvorgang hinzufügen](users-bulk-add.md)
- [Massenlöschung von Benutzern](users-bulk-delete.md)
- [Massenwiederherstellung von Benutzern](users-bulk-restore.md)
