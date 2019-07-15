---
title: Erstellen und Freigeben von Dashboards im Azure-Portal | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie Dashboards im Azure-Portal erstellen, anpassen, veröffentlichen und freigeben.
services: azure-portal
documentationcenter: ''
author: sewatson
manager: mtillman
editor: tysonn
ms.assetid: ff422f36-47d2-409b-8a19-02e24b03ffe7
ms.service: azure-portal
ms.devlang: NA
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 07/01/2019
ms.author: kfollis
ms.openlocfilehash: 8dd1349ca9ab62484eb6693291e3b869ff079dc1
ms.sourcegitcommit: 084630bb22ae4cf037794923a1ef602d84831c57
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2019
ms.locfileid: "67537424"
---
# <a name="create-and-share-dashboards-in-the-azure-portal"></a>Erstellen und Freigeben von Dashboards im Azure-Portal

Dashboards bieten Ihnen die Möglichkeit, eine organisierte Ansicht Ihrer Cloudressourcen im Azure-Portal zu erstellen. Sie können Dashboards als Arbeitsbereich einsetzen, indem sie Aufgaben für alltägliche Vorgänge und zur Überwachung von Ressourcen schnell und einfach erstellen können.  Sie können z. B. eigene Dashboards für Projekte, Aufgabe oder Benutzerrollen erstellen.  Im Azure-Portal wartet ein Standarddashboard auf Sie, mit dem Sie beginnen können. Sie können das Standarddashboard bearbeiten, zusätzliche Dashboards erstellen und anpassen, und diese dann veröffentlichen oder freigeben, um sie anderen Benutzern bereitzustellen. In diesem Artikel erfahren Sie mehr darüber, wie Sie ein neues Dashboard erstellen, die Schnittstelle anpassen und Dashboards veröffentlichen und freigeben.

## <a name="create-a-new-dashboard"></a>Erstellen eines neuen Dashboards

In diesem Beispiel werden wir ein neues, privates Dashboard erstellen und ihm einen Namen zuweisen. Führen Sie zum Einstieg die folgenden Schritte aus:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie oben in der Seitenleiste **„Dashboard“** aus. In Ihrer Standardansicht kann das Dashboard bereits ausgewählt sein.
1. Wählen Sie **„+ Neues Dashboard“** .

    ![Screenshot des Standarddashboards](./media/azure-portal-dashboards/dashboard-new.png)

4. Diese Aktion öffnet den **Kachelkatalog**, aus dem Sie Kacheln auswählen können, sowie ein leeres Raster, wo Sie sie anordnen können.

    ![Screenshot des Kachelkatalogs und des leeren Rasters](./media/azure-portal-dashboards/dashboard-name.png)

5. Wählen Sie den Text **„Mein Dashboard“** in der Dashboardbezeichnung und geben Sie einen Namen ein, der Ihnen dabei hilft, das benutzerdefinierte Dashboard leichter zu finden.
1. Wählen Sie im Seitenheader **„Anpassung abgeschlossen“** , um den Bearbeitungsmodus zu verlassen.

Die Dashboardansicht zeigt jetzt Ihr leeres Dashboard. Wählen Sie die Dropdownliste neben dem Dashboardnamen, um die verfügbaren Dashboards zu sehen. In der Liste können Sie auch Dashboards finden, die andere Benutzer erstellt und freigegeben haben.

## <a name="edit-a-dashboard"></a>Bearbeiten eines Dashboards

Bearbeiten wir jetzt das Dashboard, um Kacheln hinzuzufügen, ihre Größe zu ändern und sie neu anzuordnen. Diese Kacheln stehen für Ihre Azure-Ressourcen.

### <a name="add-tiles"></a>Hinzufügen von Kacheln

Mit den folgenden Schritten können Sie einem Dashboard Kacheln hinzufügen:
1. Wählen Sie das ![Bearbeitungssymbol](./media/azure-portal-dashboards/dashboard-edit-icon.png) **„Bearbeiten“** aus dem Seitenheader.

    ![Screenshot des Dashboards mit „Bearbeiten“ hervorgehoben](./media/azure-portal-dashboards/dashboard-edit.png)

2. Durchsuchen Sie den **Kachelkatalog**oder verwenden Sie das Suchfeld, um die gewünschte Kachel zu finden.
1. Wählen Sie **„Hinzufügen“** aus, um die Kachel dem Dashboard automatisch mit einer Standardgröße und einem Standardort hinzuzufügen. Oder ziehen Sie die Kachel in das Raster und platzieren Sie sie dort, wo Sie sie haben möchten.

Viele Ressourcenseiten (auch „Blätter“ genannt) enthalten eine Ortsmarke in der Befehlsleiste. Wenn Sie das Symbol auswählen, dann wird eine Kachel, die für die Quellseite steht, in das derzeit aktive Dashboard geheftet. Dies ist eine alternative Methode zum Hinzufügen von Kacheln.

![Screenshot der Befehlsleiste der Seite mit dem Stecknadelsymbol](./media/azure-portal-dashboards/dashboard-pin-blade.png)

> [!TIP]
> Wenn Sie mit mehr als einer Organisation zusammenarbeiten, können Sie die Kachel **„Organisationsidentität“** hinzufügen, um klar anzuzeigen, zu welcher Organisation die Ressource gehört.
>
>
### <a name="resize-or-rearrange-tiles"></a>Änderung der Größe oder des Platzes von Kacheln
Wenn Sie die Größe einer Kachel ändern oder sie im Dashboard an einen anderen Platz verschieben wollen, dann folgen Sie diesen Schritten:

1. Wählen Sie das ![Bearbeitungssymbol](./media/azure-portal-dashboards/dashboard-edit-icon.png) **„Bearbeiten“** aus dem Seitenheader.
1. Wählen Sie das Kontextmenü oben rechts in der Ecke einer Kachel. Wählen Sie dann eine Kachelgröße. Kacheln, die in beliebiger Größe verfügbar sind, haben einen Ziehpunkt unten rechts in der Ecke, mit dem Sie die Größe durch Ziehen anpassen können.

   ![Screenshot des Dashboards mit offenem Kachelgrößenmenü](./media/azure-portal-dashboards/dashboard-tile-resize.png)

3. Wählen Sie eine Kachel aus und ziehen Sie sie an einen neuen Platz im Raster, um Ihr Dashboard anzupassen.

### <a name="additional-tile-configuration"></a>Zusätzliche Kachelkonfiguration

Es kann sein, dass Sie einige Kacheln weiter konfigurieren müssen, damit sie die von Ihnen gewünschten Informationen anzeigen. So muss die Kachel **„Metrikdiagramm“** eingerichtet werden, damit sie eine Metrik aus **Azure Monitor** anzeigt. Sie können Kacheldaten auch so anpassen, dass sie die Standardzeiteinstellungen des Dashboard außer Kraft setzen.

Jede Kachel, die eingerichtet werden muss, zeigt das Banner **„Kachel konfigurieren“** , bis Sie sie angepasst haben. Wählen Sie das Banner aus und führen Sie dann die erforderliche Einrichtung durch.

![Screenshot einer Kachel, die konfiguriert werden muss](./media/azure-portal-dashboards/dashboard-configure-tile.png)

> [!NOTE]
> Mit einer Markdown-Kachel können Sie benutzerdefinierte, statische Inhalte auf Ihrem Dashboard anzeigen. Das können einfache Anweisungen sein, ein Bild, eine Reihe Links oder sogar Kontaktinformationen. Weitere Informationen über die Verwendung einer Markdown-Kachel finden Sie unter [„Verwenden einer benutzerdefinierten Markdown-Kachel“](azure-portal-markdown-tile.md).
>
>
### <a name="customize-tile-data"></a>Kacheldaten anpassen

Die Daten auf dem Dashboard zeigen automatisch die Aktivitäten der letzten 24 Stunden an. Folgen Sie diesen Schritten, wenn Sie eine andere Zeitspanne nur für diese Kachel sehen möchten:

1. Wählen Sie das ![Filtersymbol](./media/azure-portal-dashboards/dashboard-filter.png) oben links in der Kachel aus oder **„Kacheldaten anpassen“** im Kontextmenü.

   ![Screenshot des Kachelkontextmenüs](./media/azure-portal-dashboards/dashboard-customize-tile-data.png)

2. Markieren Sie das Kontrollkästchen für **„Außerkraftsetzung der Zeiteinstellungen für das Dashboard auf Kachelebene“** .

   ![Screenshot des Dialogfelds zur Konfigurierung der Kachelzeiteinstellungen](./media/azure-portal-dashboards/dashboard-override-time-settings.png)

3. Wählen Sie die Zeitspanne für diese Kachel. Sie können sich für die letzten 30 Minuten bis hin zu den letzten 30 Tagen entscheiden, oder auch einen benutzerdefinierten Zeitraum angeben.
1. Wählen Sie die Zeitgranularität, die angezeigt werden soll. Sie können sich für Inkremente aus einer Minute bis hin zu einem Monat entscheiden.
1. Wählen Sie **Übernehmen**.

## <a name="delete-a-tile"></a>Löschen einer Kachel

Folgen Sie diesen Schritten, wenn Sie eine Kachel aus einem Dashboard löschen möchten:

* Wählen Sie das Kontextmenü oben rechts in der Kachel aus und wählen Sie dann **„Aus Dashboard entfernen“** . Oder
* Wählen Sie das ![Symbol „Bearbeiten“](./media/azure-portal-dashboards/dashboard-edit-icon.png) **„Bearbeiten“** , um den Anpassungsmodus aufzurufen. Fahren Sie über die obere rechte Ecke der Kachel und wählen sie dann das ![Löschsymbol](./media/azure-portal-dashboards/dashboard-delete-icon.png) aus, um die Kachel aus dem Dashboard zu entfernen.

   ![Screenshot der zeigt, wie eine Kachel aus dem Dashboard entfernt wird](./media/azure-portal-dashboards/dashboard-delete-tile.png)

## <a name="clone-a-dashboard"></a>Klonen eines Dashboards

Sie können ein vorhandenes Dashboard als Vorlage für ein neues verwenden, indem Sie diesen Schritten folgen:

1. Achten Sie darauf, dass die Dashboardansicht das Dashboard zeigt, das Sie kopieren möchten.
1. Wählen Sie im Seitenheader das ![Klonensymbol](./media/azure-portal-dashboards/dashboard-clone.png) **„Klonen“** .
1. Eine Kopie des Dashboards namens „Klon von *Ihre Dashboardname*“ wird im Bearbeitungsmodus geöffnet. Anhand der vorherigen Schritte in diesem Artikel können Sie das Dashboard umbenennen und anpassen.

## <a name="publish-and-share-a-dashboard"></a>Veröffentlichen und Freigeben eines Dashboards

Wenn Sie ein Dashboard erstellen, ist es standardmäßig ein privates Dashboard. Dies bedeutet, dass Sie die einzige Person sind, die es anzeigen kann. Sie können anderen Benutzern Dashboards zur Verfügung stellen, indem Sie sie freigeben. Zuerst müssen Sie das Dashboard als Azure-Ressource veröffentlichen. Folgen Sie diesen Schritten, um ein benutzerdefiniertes Dashboard zu veröffentlichen und freizugeben:

1. Wählen Sie das ![Freigabesymbol](./media/azure-portal-dashboards/dashboard-share-icon.png) **„Teilen“** im Seitenheader. Es erscheint das Formular **„Freigabe + Zugriffssteuerung“** .
1. Überprüfen Sie, ob der richtige Dashboardname angezeigt wird.
1. Wählen Sie einen **Abonnementnamen** aus. Benutzer mit Zugriff auf das Abonnement können das freigegebene Dashboard verwenden. Der Zugriff auf die Ressourcen, die anhand der einzelnen Kacheln dargestellt werden, wird durch die rollenbasierte Zugriffssteuerung von Azure dargestellt.
1. Markieren Sie das Kontrollkästchen, um dieses Dashboard in der Dashboard-Ressourcengruppe für das ausgewählte Abonnement zu veröffentlichen. Alternativ können Sie das Häkchen entfernen und das Dashboard in eine vorhandene Ressourcengruppe veröffentlichen.
1. Wählen Sie einen Ort für die Dashboardressource. Wir empfehlen, dass Sie das Dashboard bei den anderen Ressourcen speichern. Hinweis: Wenn Sie eine vorhandene Ressourcengruppe wählen, dann befindet sich das Dashboard automatisch bei der Ressourcengruppe.
1. Wählen Sie **Veröffentlichen**.

   ![Screenshot des Dialogfelds für die Dashboardveröffentlichung](./media/azure-portal-dashboards/dashboard-publish.png)

### <a name="set-access-control-on-a-shared-dashboard"></a>Einrichten der Zugriffssteuerung bei einem freigegebenen Dashboard

Nachdem Sie das Dashboard veröffentlicht haben, können Sie anhand der folgenden Schritte verwalten, wer Zugriff darauf hat:

1. Wählen Sie im Feld **„Freigabe + Zugriffssteuerung“** **„Verwalten von Benutzern“** aus.

   ![Screenshot des Dialogfelds für die Freigabe und die Zugriffssteuerung des Dashboards](./media/azure-portal-dashboards/dashboard-share-access-control.png)

2. Die Seite für die **Zugriffssteuerung** wird geöffnet. Auf dieser Seite können Sie die Zugriffsebene für Benutzer überprüfen oder neue Rollenzuweisungen hinzufügen. Wenn Sie hier eine Rollenzuweisung hinzufügen, dann vergeben Sie Berechtigungen für das Dashboard.

> [!NOTE]
> Die Kacheln repräsentieren die Ressourcen in Ihrer Organisation. Der Zugriff auf die Ressourcen wird über eine rollenbasierte Zugriffssteuerung verwaltet. Die Berechtigung werden vom Abonnement weiter an die Ressource gegeben. Der Zugriff auf ein Dashboard bedeutet nicht automatisch, dass der Benutzer auch Berechtigungen für die Ressourcen hat, die auf dem Dashboard angezeigt werden. Weitere Informationen über Berechtigungen für freigegebene Dashboard und die rollenbasierte Zugriffssteuerung für Ressourcen finden Sie unter [„Freigeben von Dashboards per rollenbasierter Zugriffssteuerung“](azure-portal-dashboard-share-access.md).

### <a name="open-a-shared-dashboard"></a>Öffnen eines freigegebenen Dashboards

Folgen Sie diesen Schritten, um ein freigegebenes Dashboard zu finden und zu öffnen:

1. Wählen Sie die Dropdownliste neben dem Namen des Dashboards aus.
1. Wählen Sie ein Dashboard aus der Liste oder **durchsuchen Sie alle Dashboards**, wenn Ihres nicht aufgeführt ist.

   ![Screenshot des Dashboard-Auswahlmenüs](./media/azure-portal-dashboards/dashboard-browse.png)

3. Wählen Sie im Feld **„Typ“** **„Freigegebene Dashboards“** aus.
1. Wählen Sie ein oder mehrere Abonnements aus. Sie können auch Text verwenden, um die Dashboards nach Namen zu filtern.
1. Wählen Sie ein Dashboard aus der Liste der freigegebenen Dashboards.

## <a name="delete-a-dashboard"></a>Löschen eines Dashboards

Wenn Sie ein privates oder ein freigegebenes Dashboard dauerhaft löschen wollen, dann folgen Sie diesen Schritten:

1. Wählen Sie das Dashboard, das Sie löschen wollen, aus der Dropdownliste neben dem Namen des Dashboards.
1. Wählen Sie das ![Löschen-Symbol](./media/azure-portal-dashboards/dashboard-delete-icon.png) **„Löschen“** aus dem Seitenheader.
1. Bei einem privaten Dashboard müssen Sie im Bestätigungsdialogfeld **„OK“** wählen, um das Dashboard zu löschen. Bei einem freigegebenen Dashboard wählen Sie im Bestätigungsdialogfeld das Kontrollkästchen aus, um zu bestätigen, dass andere das veröffentlichte Dashboard nicht mehr sehen können. Wählen Sie anschließend **OK** aus.

   ![Screenshot der Löschbestätigung](./media/azure-portal-dashboards/dashboard-delete-dash.png)

## <a name="next-steps"></a>Nächste Schritte

* [Freigeben von Dashboards per rollenbasierter Zugriffssteuerung](azure-portal-dashboard-share-access.md)
* [Programmgesteuertes Erstellen von Azure-Dashboards](azure-portal-dashboards-create-programmatically.md)
