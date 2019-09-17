---
title: Massenimport und -upload zum Hinzufügen von Mitgliedern zu einer Gruppe – Azure Active Directory | Microsoft-Dokumentation
description: Fügen Sie Gruppenmitglieder in einem Massenvorgang im Azure Active Directory Admin Center hinzu.
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
ms.openlocfilehash: 0335522b398c22fb395305b33bac5a56ba565ee2
ms.sourcegitcommit: d70c74e11fa95f70077620b4613bb35d9bf78484
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2019
ms.locfileid: "70910690"
---
# <a name="bulk-import-group-members-preview-in-azure-active-directory"></a>Massenimport von Gruppenmitgliedern (Vorschau) in Azure Active Directory

Über das Azure AD-Portal (Azure Active Directory) können Sie eine große Anzahl von Mitgliedern zu einer Gruppe hinzufügen, indem Sie mithilfe einer CSV-Datei (Comma-Separated Values, durch Trennzeichen getrennte Werte) einen Massenimport von Gruppenmitgliedern ausführen.

> [!NOTE]
> Azure AD-Massenvorgänge sind eine öffentliche Previewfunktion von Azure AD und bei jedem kostenpflichtigen Azure AD-Lizenzplan verfügbar. Weitere Informationen zu Nutzungsbedingungen für Vorschauversionen finden Sie unter [Zusätzliche Nutzungsbedingungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="bulk-import-service-limits"></a>Diensteinschränkungen beim Massenimport

Jede Massenaktivität zum Importieren einer Liste von Gruppenmitgliedern kann bis zu einer Stunde dauern. Dadurch wird der Import einer Liste von mindestens 40.000 Mitgliedern ermöglicht.

## <a name="to-bulk-import-group-members"></a>So führen Sie den Massenimport von Gruppenmitgliedern aus

1. Melden Sie sich mit dem Konto eines Benutzeradministrators in der Organisation beim [Azure-Portal](https://portal.azure.com) an. Gruppenbesitzer können Mitglieder von Gruppen, die ihnen gehören, ebenfalls in einem Massenvorgang importieren.
1. Wählen Sie in Azure AD **Gruppen** > **Alle Gruppen** aus.
1. Öffnen Sie die Gruppe, der Sie Mitglieder hinzufügen möchten, und wählen Sie **Mitglieder** aus.
1. Wählen Sie auf der Seite **Mitglieder** die Option **Mitglieder importieren** aus, um eine CSV-Datei mit einer Liste der Mitglieder, die Sie in die Gruppe importieren möchten, herunterzuladen, zu aktualisieren und hochzuladen.

   ![Der Befehl „Mitglieder importieren“ ist auf der Profilseite für die Gruppe zu finden.](./media/groups-bulk-import-members/import-panel.png)

## <a name="check-import-status"></a>Überprüfen des Importstatus

Sie können den Status aller Ihrer ausstehenden Massenanforderungen auf der Seite **Ergebnisse von Massenvorgängen (Vorschau)** anzeigen.

   ![Die Seite „Ergebnisse von Massenvorgängen“ zeigt Ihnen den Status von Massenanforderungen.](./media/groups-bulk-import-members/bulk-center.png)

## <a name="next-steps"></a>Nächste Schritte

- [Massenentfernung von Gruppenmitgliedern](groups-bulk-remove-members.md)
- [Mitglieder einer Gruppe herunterladen](groups-bulk-download-members.md)
