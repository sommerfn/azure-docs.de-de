---
title: Sichern von Azure Files-Dateifreigaben mit dem Azure Backup-Dienst
description: In diesem Tutorial erfahren Sie, wie Sie im Azure-Portal einen Recovery Services-Tresors konfigurieren und Azure-Dateifreigaben sichern.
author: dcurwin
ms.author: dacurwin
ms.date: 06/10/2019
ms.topic: tutorial
ms.service: backup
manager: carmonm
ms.openlocfilehash: a8b08f87441f9b4c67f718dfe9f0c894d0730a5f
ms.sourcegitcommit: 827248fa609243839aac3ff01ff40200c8c46966
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73747053"
---
# <a name="back-up-azure-file-shares-in-the-azure-portal"></a>Sichern von Azure-Dateifreigaben im Azure-Portal

In diesem Tutorial erfahren Sie, wie Sie mithilfe des Azure-Portals [Azure Dateifreigaben](../storage/files/storage-files-introduction.md) sichern.

In diesem Artikel lernen Sie Folgendes:
> [!div class="checklist"]
>
> * Konfigurieren eines Recovery Services-Tresors zum Sichern von Azure Files
> * Ausführen eines bedarfsbasierten Sicherungsauftrags zum Erstellen eines Wiederherstellungspunkts

## <a name="prerequisites"></a>Voraussetzungen

Um eine Azure-Dateifreigabe sichern zu können, muss sie sich unter einem der [unterstützten Speicherkontotypen](tutorial-backup-azure-files.md#limitations-for-azure-file-share-backup-during-preview) befinden. Sobald Sie dies überprüft haben, können Sie Ihre Dateifreigaben schützen.

## <a name="limitations-for-azure-file-share-backup-during-preview"></a>Einschränkungen beim Sichern von Azure-Dateifreigaben während der Vorschauphase

Die Sicherung für Azure-Dateifreigaben befindet sich in der Vorschauphase. Azure-Dateifreigaben in Speicherkonten vom Typ „Universell V1“ und „Universell V2“ werden unterstützt. Folgende Sicherungsszenarien werden für Azure-Dateifreigaben nicht unterstützt:

* Sie können Azure-Dateifreigaben in Speicherkonten mit aktivierten virtuellen Netzwerken oder aktivierter Firewall nicht schützen.
* Für den Schutz von Azure Files mit Azure Backup ist keine Befehlszeilenschnittstelle verfügbar.
* Die Anzahl geplanter Sicherungen ist auf eine Sicherung pro Tag begrenzt.
* Die Anzahl bedarfsgesteuerter Sicherungen ist auf vier Sicherungen pro Tag begrenzt.
* Verwenden Sie [Ressourcensperren](https://docs.microsoft.com/cli/azure/resource/lock?view=azure-cli-latest) für das Speicherkonto, um das versehentliche Löschen von Sicherungen in Ihrem Recovery Services-Tresor zu verhindern.
* Löschen Sie keine Momentaufnahmen, die mit Azure Backup erstellt wurden. Das Löschen von Momentaufnahmen kann zum Verlust von Wiederherstellungspunkten bzw. zu Wiederherstellungsfehlern führen.
* Löschen Sie keine Dateifreigaben, die durch Azure Backup geschützt sind. In der aktuellen Lösung werden nach dem Löschen der Dateifreigabe alle von Azure Backup erstellten Momentaufnahmen gelöscht, sodass alle Wiederherstellungspunkte verloren gehen.

Sicherungen für Azure-Dateifreigaben in Speicherkonten mit Replikation vom Typ [ZRS](../storage/common/storage-redundancy-zrs.md) (Zone Redundant Storage, zonenredundanter Speicher) steht momentan nur in den Regionen „USA, Mitte“ (CUS), „USA, Osten (EUS), „USA, Osten 2“ (EUS2), „Europa, Norden“ (NE), „Asien, Südosten“ (SEA), „Europa, Westen“ (WE) und „USA, Westen 2“ (WUS2) zur Verfügung.

## <a name="configuring-backup-for-an-azure-file-share"></a>Konfigurieren der Sicherung für eine Azure-Dateifreigabe

In diesem Tutorial wird davon ausgegangen, dass Sie bereits eine Azure-Dateifreigabe eingerichtet haben. So sichern Sie Ihre Azure-Dateifreigabe

1. Erstellen Sie einen Recovery Services-Tresor in der Region, in der sich auch die Dateifreigabe befindet. Wenn Sie bereits einen Tresor besitzen, öffnen Sie die Seite „Übersicht“ des Tresors, und klicken Sie auf **Sicherung**.

    ![Klicken Sie auf der Seite „Übersicht“ des Tresors auf „Sicherung“.](./media/backup-file-shares/overview-backup-page.png)

2. Wählen Sie im Menü **Sicherungsziel** unter **Was möchten Sie sichern?** die Option „Azure-Dateifreigabe“.

    ![Auswählen einer Azure-Dateifreigabe als Sicherungsziel](./media/backup-file-shares/choose-azure-fileshare-from-backup-goal.png)

3. Klicken Sie auf **Sicherung**, um die Azure-Dateifreigabe für Ihren Recovery Services-Tresor zu konfigurieren.

   ![Klicken auf „Sicherung“, um die Azure-Dateifreigabe mit dem Tresor zu verknüpfen](./media/backup-file-shares/set-backup-goal.png)

    Wenn der Tresor der Azure-Dateifreigabe zugeordnet wurde, wird das Menü „Sicherung“ geöffnet, und Sie werden zur Auswahl eines Speicherkontos aufgefordert. Im Menü werden alle unterstützten Speicherkonten in der Region des Tresors angezeigt, die noch keinem Recovery Services-Tresor zugeordnet sind.

   ![Wählen Sie Ihr Speicherkonto aus.](./media/backup-file-shares/list-of-storage-accounts.png)

4. Wählen Sie in der Liste der Speicherkonten ein Speicherkonto aus, und klicken Sie auf **OK**. Azure durchsucht das Speicherkonto nach Dateifreigaben, die gesichert werden können. Falls Sie die Dateifreigaben erst kürzlich hinzugefügt haben und diese nicht in der Liste angezeigt werden, warten Sie einen Moment, bis die Dateifreigaben angezeigt werden.

   ![Dateifreigaben werden ermittelt.](./media/backup-file-shares/discover-file-shares.png)

5. Wählen Sie in der Liste **Dateifreigaben** mindestens eine zu sichernde Dateifreigabe aus, und klicken Sie auf **OK**.

6. Nach dem Auswählen der Dateifreigaben wechselt das Menü „Sicherung“ zur **Sicherheitsrichtlinie**. Wählen Sie in diesem Menü entweder eine vorhandene Sicherheitsrichtlinie aus, oder erstellen Sie eine neue, und klicken Sie dann auf **Sicherung aktivieren**.

   ![Wählen Sie eine Sicherungsrichtlinie oder erstellen Sie eine neue](./media/backup-file-shares/apply-backup-policy.png)

    Nach dem Einrichten einer Sicherungsrichtlinie wird eine Momentaufnahme der Dateifreigaben zum geplanten Zeitpunkt erstellt, und der Wiederherstellungspunkt wird für den ausgewählten Zeitraum aufbewahrt.

## <a name="create-an-on-demand-backup"></a>Erstellen einer bedarfsgesteuerten Sicherung

Nachdem Sie die Sicherungsrichtlinie konfiguriert haben, sollten Sie eine bedarfsgesteuerte Sicherung erstellen, um zu gewährleisten, dass Ihre Daten bis zur nächsten geplanten Sicherung geschützt sind.

### <a name="to-create-an-on-demand-backup"></a>So erstellen Sie eine bedarfsgesteuerte Sicherung

1. Öffnen Sie den Recovery Services-Tresor, der die Wiederherstellungspunkte der Dateifreigaben enthält, und klicken Sie auf **Sicherungselemente**. Die Liste der Sicherungselementtypen wird angezeigt.

   ![Liste der Sicherungselemente](./media/backup-file-shares/list-of-backup-items.png)

2. Wählen Sie in der Liste die Option **Azure Storage (Azure Files)** aus. Die Liste der Azure-Dateifreigaben wird angezeigt.

   ![Liste der Azure-Dateifreigaben](./media/backup-file-shares/list-of-azure-files-backup-items.png)

3. Wählen Sie in der Liste der Azure-Dateifreigaben die gewünschte Dateifreigabe aus. Das Menü „Sicherungselemente“ wird für die ausgewählte Dateifreigabe geöffnet.

   ![Das Menü „Sicherungselemente“ für die ausgewählte Dateifreigabe](./media/backup-file-shares/backup-item-menu.png)

4. Klicken Sie im Menü „Sicherungselemente“ auf **Jetzt sichern**. Da es sich um einen bedarfsgesteuerten Sicherungsauftrag handelt, ist dem Wiederherstellungspunkt keine Aufbewahrungsrichtlinie zugeordnet. Das Dialogfeld **Jetzt sichern** wird geöffnet. Geben Sie den Tag an, bis zu dem der Wiederherstellungspunkt aufbewahrt werden soll.

   ![Wählen des Datums für die Aufbewahrung des Wiederherstellungspunkts](./media/backup-file-shares/backup-now-menu.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie das Azure-Portal zu folgenden Zwecken verwendet:

> [!div class="checklist"]
>
> * Konfigurieren eines Recovery Services-Tresors zum Sichern von Azure Files
> * Ausführen eines bedarfsbasierten Sicherungsauftrags zum Erstellen eines Wiederherstellungspunkts

Fahren Sie mit dem nächsten Artikel fort, in dem Sie erfahren, wie Sie eine Azure-Dateifreigabe mithilfe einer Sicherung wiederherstellen können.

> [!div class="nextstepaction"]
> [Wiederherstellen einer Azure-Dateifreigabe aus einer Sicherung](./backup-azure-files.md#restore-from-backup-of-azure-file-share)
