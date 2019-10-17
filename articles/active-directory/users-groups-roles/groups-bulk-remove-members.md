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
ms.openlocfilehash: 9d384ea4749e2d0bc7edf8df7ac0508566f2f76b
ms.sourcegitcommit: 12de9c927bc63868168056c39ccaa16d44cdc646
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "72517091"
---
# <a name="bulk-remove-group-members-preview-in-azure-active-directory"></a>Massenentfernung von Gruppenmitgliedern (Vorschau) in Azure Active Directory

Über das Azure AD-Portal (Azure Active Directory) können Sie eine große Anzahl von Mitgliedern aus einer Gruppe entfernen, indem Sie mithilfe einer CSV-Datei (Comma-Separated Values, durch Trennzeichen getrennte Werte) eine Massenentfernung von Gruppenmitgliedern ausführen.

## <a name="to-bulk-remove-group-members"></a>So entfernen Sie Gruppenmitglieder in einem Massenvorgang

1. Melden Sie sich mit dem Konto eines Benutzeradministrators in der Organisation beim [Azure-Portal](https://portal.azure.com) an. Gruppenbesitzer können Mitglieder von Gruppen, die ihnen gehören, ebenfalls in einem Massenvorgang entfernen.
1. Wählen Sie in Azure AD **Gruppen** > **Alle Gruppen** aus.
1. Öffnen Sie die Gruppe, aus der Sie Mitglieder entfernen möchten, und wählen Sie **Mitglieder** aus.
1. Wählen Sie auf der Seite **Mitglieder** die Option **Mitglieder entfernen** aus.
1. Wählen Sie auf der Seite **Massenentfernung von Gruppenmitgliedern (Vorschau)** die Option **Herunterladen** aus, um die CSV-Dateivorlage mit den erforderlichen Gruppenmitgliedseigenschaften zu erhalten.

   ![Der Befehl „Mitglieder entfernen“ ist auf der Profilseite für die Gruppe zu finden.](./media/groups-bulk-remove-members/remove-panel.png)

1. Öffnen Sie die CSV-Datei, und fügen Sie für jedes aus der Gruppe zu entfernende Gruppenmitglied eine Zeile hinzu (die erforderlichen Werte sind „Mitgliedsobjekt-ID“ oder „Benutzerprinzipalname“). Speichern Sie dann die Datei.

   ![Die CSV-Datei enthält Namen und IDs der zu entfernenden Mitglieder.](./media/groups-bulk-remove-members/csv-file.png)

1. Navigieren Sie auf der Seite **Massenentfernung von Gruppenmitgliedern (Vorschau)** unter **CSV-Datei hochladen** zur entsprechenden Datei. Wenn Sie die Datei auswählen, wird mit der Überprüfung der CSV-Datei begonnen.
1. Nach der Überprüfung des Dateiinhalts wird auf der Seite für den Massenimport die Meldung **Datei erfolgreich hochgeladen** angezeigt. Wenn Fehler vorliegen, müssen Sie diese beheben, bevor Sie den Auftrag übermitteln können.
1. Wenn die Datei die Überprüfung bestanden hat, wählen Sie **Senden** aus, um den Azure-Massenvorgang zu starten, der die Gruppenmitglieder aus der Gruppe entfernt.
1. Nach Abschluss des Entfernungsvorgangs wird eine Benachrichtigung angezeigt, dass der Massenvorgang erfolgreich abgeschlossen wurde.

## <a name="check-removal-status"></a>Überprüfen des Status für Entfernen

Sie können den Status aller Ihrer ausstehenden Massenanforderungen auf der Seite **Ergebnisse von Massenvorgängen (Vorschau)** anzeigen.

   ![Die Seite „Ergebnisse von Massenvorgängen“ zeigt Ihnen den Status von Massenanforderungen.](./media/groups-bulk-remove-members/bulk-center.png)

Wenn Sie ausführliche Informationen zu jedem Zeilenelement des Massenvorgangs erhalten möchten, wählen Sie die Werte unter der Spalte **# Erfolg**, **# Fehler** oder **Anforderungen insgesamt** aus. Wenn Fehler aufgetreten sind, werden die Fehlerursachen aufgeführt.

## <a name="bulk-removal-service-limits"></a>Diensteinschränkungen bei Massenentfernung

Jede Massenaktivität zum Entfernen einer Liste von Gruppenmitgliedern kann bis zu einer Stunde dauern. Dadurch wird das Entfernen einer Liste von mindestens 40.000 Mitgliedern ermöglicht.

## <a name="next-steps"></a>Nächste Schritte

- [Massenimport von Gruppenmitgliedern ausführen](groups-bulk-import-members.md)
- [Mitglieder einer Gruppe herunterladen](groups-bulk-download-members.md)
- [Herunterladen einer Liste aller Gruppen](groups-bulk-download.md)
