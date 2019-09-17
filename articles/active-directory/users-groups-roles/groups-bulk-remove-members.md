---
title: Massenentfernung von Gruppenmitgliedern durch Hochladen einer CSV-Datei – Azure Active Directory | Microsoft-Dokumentation
description: Fügen Sie Benutzer in einem Massenvorgang im Azure Admin Center hinzu.
services: active-directory
author: curtand
ms.author: curtand
manager: mtillman
ms.date: 09/11/2019
ms.topic: conceptual
ms.service: active-directory
ms.subservice: users-groups-roles
ms.workload: identity
ms.custom: it-pro
ms.reviewer: jeffsta
ms.collection: M365-identity-device-management
ms.openlocfilehash: b153db3570f10ad5ad130dedd0bd20fe22776ed6
ms.sourcegitcommit: d70c74e11fa95f70077620b4613bb35d9bf78484
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2019
ms.locfileid: "70910685"
---
# <a name="bulk-remove-group-members-preview-in-azure-active-directory"></a>Massenentfernung von Gruppenmitgliedern (Vorschau) in Azure Active Directory

Über das Azure AD-Portal (Azure Active Directory) können Sie eine große Anzahl von Mitgliedern aus einer Gruppe entfernen, indem Sie mithilfe einer CSV-Datei (Comma-Separated Values, durch Trennzeichen getrennte Werte) eine Massenentfernung von Gruppenmitgliedern ausführen.

> [!NOTE]
> Azure AD-Massenvorgänge sind eine öffentliche Previewfunktion von Azure AD und bei jedem kostenpflichtigen Azure AD-Lizenzplan verfügbar. Weitere Informationen zu Nutzungsbedingungen für Vorschauversionen finden Sie unter [Zusätzliche Nutzungsbedingungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="bulk-removal-service-limits"></a>Diensteinschränkungen bei Massenentfernung

Jede Massenaktivität zum Entfernen einer Liste von Gruppenmitgliedern kann bis zu einer Stunde dauern. Dadurch wird das Entfernen einer Liste von mindestens 40.000 Mitgliedern ermöglicht.

## <a name="to-bulk-remove-group-members"></a>So entfernen Sie Gruppenmitglieder in einem Massenvorgang

1. Melden Sie sich mit dem Konto eines Benutzeradministrators in der Organisation beim [Azure-Portal](https://portal.azure.com) an. Gruppenbesitzer können Mitglieder von Gruppen, die ihnen gehören, ebenfalls in einem Massenvorgang entfernen.
1. Wählen Sie in Azure AD **Gruppen** > **Alle Gruppen** aus.
1. Öffnen Sie die Gruppe, aus der Sie Mitglieder entfernen möchten, und wählen Sie **Mitglieder** aus.
1. Wählen Sie auf der Seite **Mitglieder** die Option **Mitglieder entfernen** aus, um eine CSV-Datei mit einer Liste der Mitglieder, die Sie aus der Gruppe entfernen möchten, herunterzuladen, zu aktualisieren und hochzuladen.

   ![Der Befehl „Mitglieder entfernen“ ist auf der Profilseite für die Gruppe zu finden.](./media/groups-bulk-remove-members/remove-panel.png)

## <a name="check-removal-status"></a>Überprüfen des Status für Entfernen

Sie können den Status aller Ihrer ausstehenden Massenanforderungen auf der Seite **Ergebnisse von Massenvorgängen (Vorschau)** anzeigen.

   ![Die Seite „Ergebnisse von Massenvorgängen“ zeigt Ihnen den Status von Massenanforderungen.](./media/groups-bulk-remove-members/bulk-center.png)

## <a name="next-steps"></a>Nächste Schritte

- [Massenimport von Gruppenmitgliedern ausführen](groups-bulk-import-members.md)
- [Mitglieder einer Gruppe herunterladen](groups-bulk-download-members.md)
