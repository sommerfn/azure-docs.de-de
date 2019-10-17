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
ms.openlocfilehash: 8ff4234d9fd28e655e868108e37b09c5afe2767c
ms.sourcegitcommit: 12de9c927bc63868168056c39ccaa16d44cdc646
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "72517128"
---
# <a name="bulk-import-group-members-preview-in-azure-active-directory"></a>Massenimport von Gruppenmitgliedern (Vorschau) in Azure Active Directory

Über das Azure AD-Portal (Azure Active Directory) können Sie eine große Anzahl von Mitgliedern zu einer Gruppe hinzufügen, indem Sie mithilfe einer CSV-Datei (Comma-Separated Values, durch Trennzeichen getrennte Werte) einen Massenimport von Gruppenmitgliedern ausführen.

## <a name="to-bulk-import-group-members"></a>So führen Sie den Massenimport von Gruppenmitgliedern aus

1. Melden Sie sich mit dem Konto eines Benutzeradministrators in der Organisation beim [Azure-Portal](https://portal.azure.com) an. Gruppenbesitzer können Mitglieder von Gruppen, die ihnen gehören, ebenfalls in einem Massenvorgang importieren.
1. Wählen Sie in Azure AD **Gruppen** > **Alle Gruppen** aus.
1. Öffnen Sie die Gruppe, der Sie Mitglieder hinzufügen möchten, und wählen Sie **Mitglieder** aus.
1. Wählen Sie auf der Seite **Mitglieder** die Option **Mitglieder importieren** aus.
1. Wählen Sie auf der Seite **Massenimport von Gruppenmitgliedern (Vorschau)** die Option **Herunterladen** aus, um die CSV-Dateivorlage mit den erforderlichen Gruppenmitgliedseigenschaften zu erhalten.

    ![Der Befehl „Mitglieder importieren“ ist auf der Profilseite für die Gruppe zu finden.](./media/groups-bulk-import-members/import-panel.png)

1. Öffnen Sie die CSV-Datei, und fügen Sie für jedes in die Gruppe zu importierende Gruppenmitglied eine Zeile hinzu (die erforderlichen Werte sind entweder **Mitgliedsobjekt-ID** oder **Benutzerprinzipalname**). Speichern Sie dann die Datei.

   ![Die CSV-Datei enthält Namen und IDs der zu importierenden Mitglieder.](./media/groups-bulk-import-members/csv-file.png)

1. Navigieren Sie auf der Seite **Massenimport von Gruppenmitgliedern (Vorschau)** unter **CSV-Datei hochladen** zur entsprechenden Datei. Wenn Sie die Datei auswählen, wird mit der Überprüfung der CSV-Datei begonnen.
1. Nach der Überprüfung des Dateiinhalts wird auf der Seite für den Massenimport die Meldung **Datei erfolgreich hochgeladen** angezeigt. Wenn Fehler vorliegen, müssen Sie diese beheben, bevor Sie den Auftrag übermitteln können.
1. Wenn die Datei die Überprüfung bestanden hat, wählen Sie **Senden** aus, um den Azure-Massenvorgang zu starten, der die Gruppenmitglieder in die Gruppe importiert.
1. Nach Abschluss des Importvorgangs wird eine Benachrichtigung angezeigt, dass der Massenvorgang erfolgreich abgeschlossen wurde.

## <a name="check-import-status"></a>Überprüfen des Importstatus

Sie können den Status aller Ihrer ausstehenden Massenanforderungen auf der Seite **Ergebnisse von Massenvorgängen (Vorschau)** anzeigen.

   ![Die Seite „Ergebnisse von Massenvorgängen“ zeigt Ihnen den Status von Massenanforderungen.](./media/groups-bulk-import-members/bulk-center.png)

Wenn Sie ausführliche Informationen zu jedem Zeilenelement des Massenvorgangs erhalten möchten, wählen Sie die Werte unter der Spalte **# Erfolg**, **# Fehler** oder **Anforderungen insgesamt** aus. Wenn Fehler aufgetreten sind, werden die Fehlerursachen aufgeführt.

## <a name="bulk-import-service-limits"></a>Diensteinschränkungen beim Massenimport

Jede Massenaktivität zum Importieren einer Liste von Gruppenmitgliedern kann bis zu einer Stunde dauern. Dadurch wird der Import einer Liste von mindestens 40.000 Mitgliedern ermöglicht.

## <a name="next-steps"></a>Nächste Schritte

- [Massenentfernung von Gruppenmitgliedern](groups-bulk-remove-members.md)
- [Mitglieder einer Gruppe herunterladen](groups-bulk-download-members.md)
- [Herunterladen einer Liste aller Gruppen](groups-bulk-download.md)
