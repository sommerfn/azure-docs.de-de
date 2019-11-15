---
title: Bedarfsorientiertes Sichern von virtuellen Azure-Computern
description: In diesem Tutorial erfahren Sie, wie Sie einen Recovery Services-Tresor erstellen, eine Sicherungsrichtlinie definieren und mehrere virtuelle Computer gleichzeitig sichern.
keywords: Sicherung virtueller Computer; virtuelle Computer sichern; VM-Sicherung; VM sichern; Azure-VM sichern; Sicherung und Notfallwiederherstellung
author: dcurwin
manager: carmonm
ms.author: dacurwin
ms.date: 01/31/2019
ms.topic: tutorial
ms.service: backup
ms.custom: mvc
ms.openlocfilehash: 99a842704325e38cbf1ab9203a56a25bc2273827
ms.sourcegitcommit: 827248fa609243839aac3ff01ff40200c8c46966
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73747019"
---
# <a name="use-azure-portal-to-back-up-multiple-virtual-machines"></a>Verwenden des Azure-Portals zum Sichern mehrerer virtueller Computer

Wenn Sie Daten in Azure sichern, speichern Sie diese Daten in einer Azure-Ressource mit der Bezeichnung Recovery Services-Tresor. Sie finden den Recovery Services-Tresor in den meisten Azure-Diensten im Einstellungsmenü. Die Integration des Recovery Services-Tresors in das Einstellungsmenü der meisten Azure-Dienste vereinfacht das Sichern von Daten. Die Arbeit mit den einzelnen Datenbanken oder virtuellen Computern in Ihrem Unternehmen ist jedoch aufwendig. Was geschieht, wenn Sie die Daten für alle virtuellen Computer in einer Abteilung oder an einem Standort sichern möchten? Es ist einfach, mehrere virtuelle Computer zu sichern, indem Sie eine Sicherungsrichtlinie erstellen und diese Richtlinie auf die gewünschten virtuellen Computer anwenden. In diesem Tutorial werden folgende Punkte erläutert:

> [!div class="checklist"]
>
> * Erstellen eines Recovery Services-Tresors
> * Definieren einer Sicherungsrichtlinie
> * Anwenden der Sicherungsrichtlinie zum Schützen mehrerer virtueller Computer
> * Auslösen eines bedarfsgesteuerten Sicherungsauftrags für die geschützten virtuellen Computer

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

## <a name="create-a-recovery-services-vault"></a>Erstellen eines Recovery Services-Tresors

Der Recovery Services-Tresor enthält die Sicherungsdaten, und Sicherungsrichtlinie, die auf die geschützten virtuellen Computer angewendet wird. Das Sichern virtueller Computer ist ein örtlich gebundener Vorgang. Es ist nicht möglich, einen virtuellen Computer eines Standorts in einem Recovery Services-Tresor an einem anderen Standort zu sichern. An jedem Azure-Standort, der über zu sichernde virtuelle Computer verfügt, muss also mindestens ein Recovery Services-Tresor vorhanden sein.

1. Wählen Sie im linken Menü **Alle Dienste** aus, und geben Sie in der Dienstliste *Recovery Services* ein. Während der Eingabe wird die Liste mit den Ressourcen gefiltert. Wenn in der Liste die Option „Recovery Services-Tresore“ angezeigt wird, wählen Sie sie aus, um das Menü „Recovery Services-Tresore“ zu öffnen.

    ![Öffnen des Menüs „Recovery Services-Tresor“](./media/tutorial-backup-vm-at-scale/full-browser-open-rs-vault.png) <br/>

2. Klicken Sie im Menü **Recovery Services-Tresore** auf **Hinzufügen**, um das Menü „Recovery Services-Tresor“ zu öffnen.

    ![Tresormenü öffnen](./media/tutorial-backup-vm-at-scale/provide-vault-detail-2.png)

3. Führen Sie im Menü „Recovery Services-Tresor“ folgende Aktionen durch:

    * Geben Sie *myRecoveryServicesVault* in **Name** ein.
    * Die aktuelle Abonnement-ID wird unter **Abonnement** angezeigt. Wenn Sie über weitere Abonnements verfügen, können Sie ein anderes Abonnement für den neuen Tresor auswählen.
    * Wählen Sie für **Ressourcengruppe** die Option **Vorhandene verwenden** und dann *myResourceGroup* aus. Wenn *myResourceGroup* nicht vorhanden ist, wählen Sie **Neu erstellen** aus und geben *myResourceGroup* ein.
    * Wählen Sie im Dropdownmenü **Standort** die Region *Europa, Westen* aus.
    * Klicken Sie auf **Erstellen**, um den Recovery Services-Tresor zu erstellen.

Recovery Services-Tresore müssen sich am gleichen Standort wie die zu schützenden virtuellen Computer befinden. Falls Sie über virtuelle Computer in mehreren Regionen verfügen, erstellen Sie in jeder Region einen Recovery Services-Tresor. In diesem Tutorial wird ein Recovery Services-Tresor in *Europa, Westen* erstellt, da dort auch *myVM* (der im Schnellstart erstellte virtuelle Computer) erstellt wurde.

Die Erstellung des Recovery Services-Tresors kann mehrere Minuten dauern. Verfolgen Sie die Statusbenachrichtigungen rechts oben im Portal. Nach Abschluss des Erstellungsvorgangs wird der Tresor in der Liste mit den Recovery Services-Tresoren angezeigt.

Wenn Sie einen Recovery Services-Tresor erstellen, verfügt dieser standardmäßig über georedundanten Speicher. Für mehr Datenresilienz repliziert der georedundante Speicher die Daten mehrmals zwischen zwei Azure-Regionen.

## <a name="set-backup-policy-to-protect-vms"></a>Festlegen der Sicherungsrichtlinie zum Schutz von virtuellen Computern

Nach dem Erstellen des Recovery Services-Tresors konfigurieren Sie im nächsten Schritt den Tresor für den Datentyp und legen die Sicherungsrichtlinie fest. Die Sicherungsrichtlinie gibt die Häufigkeit und den Zeitpunkt für die Erstellung von Wiederherstellungspunkten an. Darüber hinaus enthält die Richtlinie die Beibehaltungsdauer für die Wiederherstellungspunkte. Für dieses Tutorial wird davon ausgegangen, dass Ihr Unternehmen ein Sportkomplex mit Hotel, Stadion, Restaurants und Konzessionen ist und Sie die Daten auf den virtuellen Computern schützen möchten. Anhand der folgenden Schritte erstellen Sie eine Sicherungsrichtlinie für Finanzdaten.

1. Wählen Sie in der Liste mit den Recovery Services-Tresoren **myRecoveryServicesVault** aus, um das zugehörige Dashboard zu öffnen.

   ![Menü „Szenario“ öffnen](./media/tutorial-backup-vm-at-scale/open-vault-from-list.png)

2. Klicken Sie im Menü des Tresordashboards auf **Sicherung**, um das Menü „Sicherung“ zu öffnen.

3. Wählen Sie im Menü „Sicherungsziel“ im Dropdownmenü **Wo wird Ihre Workload ausgeführt?** die Option *Azure* aus. Wählen Sie im Dropdownmenü **Was möchten Sie sichern?** die Option *Virtueller Computer* aus, und klicken Sie dann auf **Sichern**.

    Diese Aktionen bereiten den Recovery Services-Tresor auf die Interaktion mit einem virtuellen Computer vor. Recovery Services-Tresore haben eine Standardrichtlinie, die jeden Tag einen Wiederherstellungspunkt erstellt und die Wiederherstellungspunkte für 30 Tage aufbewahrt.

    ![Menü „Szenario“ öffnen](./media/tutorial-backup-vm-at-scale/backup-goal.png)

4. Zum Erstellen einer neuen Richtlinie wählen Sie im Menü „Sicherungsrichtlinie“ im Dropdownmenü **Sicherungsrichtlinie auswählen** die Option *Neu erstellen* aus.

    ![Workload auswählen](./media/tutorial-backup-vm-at-scale/create-new-policy.png)

5. Geben Sie im Menü **Sicherungsrichtlinie** für **Richtlinienname** den Namen *Finance* ein. Geben Sie die folgenden Änderungen für die Sicherungsrichtlinie ein:
   * Legen Sie für **Sicherungshäufigkeit** die Zeitzone auf *Central Time* fest. Der Sportkomplex befindet sich in Texas, und der Besitzer möchte die lokale Zeit verwenden. Lassen Sie die Sicherungshäufigkeit auf täglich um 3:30 Uhr festgelegt.
   * Legen Sie für **Aufbewahrung für täglichen Sicherungspunkt** den Zeitraum auf 90 Tage fest.
   * Verwenden Sie für **Aufbewahrung für wöchentlichen Sicherungspunkt** den Wiederherstellungspunkt *Monday*, und legen Sie als Aufbewahrungszeit 52 Wochen fest.
   * Verwenden Sie für **Aufbewahrung für monatlichen Sicherungspunkt** den Wiederherstellungspunkt vom ersten Sonntag des Monats, und legen Sie als Aufbewahrungszeit 36 Monate fest.
   * Deaktivieren Sie die Option **Aufbewahrung für jährlichen Sicherungspunkt**. Der Leiter der Finanzabteilung möchte die Daten nicht länger als 36 Monate aufbewahren.
   * Klicken Sie auf **OK**, um die Sicherungsrichtlinie zu erstellen.

     ![Workload auswählen](./media/tutorial-backup-vm-at-scale/set-new-policy.png)

     Nach ihrer Erstellung weisen Sie die Sicherungsrichtlinie den virtuellen Computern zu.

6. Wählen Sie im Dialogfeld **Virtuelle Computer auswählen** den virtuellen Computer *myVM* aus, und klicken Sie auf **OK**, um die Sicherungsrichtlinie auf die virtuellen Computer anzuwenden.

    Es werden alle virtuellen Computer am gleichen Standort angezeigt, die noch keiner Sicherungsrichtlinie zugeordnet sind. *myVMH1* und *myVMR1* wurden für die Richtlinie *Finance* ausgewählt.

    ![Workload auswählen](./media/tutorial-backup-vm-at-scale/choose-vm-to-protect.png)

    Wenn die Bereitstellung abgeschlossen ist, erhalten Sie eine Benachrichtigung darüber, dass die Bereitstellung erfolgreich abgeschlossen wurde.

## <a name="initial-backup"></a>Erste Sicherung

Sie haben zwar die Sicherung für die Recovery Services-Tresore aktiviert, aber noch keine erste Sicherung erstellt. Es wird für die Notfallwiederherstellung empfohlen, eine erste Sicherung auszulösen, damit Ihre Daten geschützt sind.

So führen Sie einen bedarfsgesteuerten Sicherungsauftrag aus

1. Klicken Sie im Tresordashboard auf die **3** unter **Sicherungselemente**, um das Menü „Sicherungselemente“ zu öffnen.

    ![Symbol „Einstellungen“](./media/tutorial-backup-vm-at-scale/tutorial-vm-back-up-now.png)

    Das Menü **Sicherungselemente** wird geöffnet.

2. Klicken Sie im Menü **Sicherungselemente** auf **Virtueller Azure-Computer**, um die Liste der virtuellen Computer, die dem Tresor zugeordnet sind, zu öffnen.

    ![Symbol „Einstellungen“](./media/tutorial-backup-vm-at-scale/three-virtual-machines.png)

    Die Liste **Sicherungselemente** wird geöffnet.

    ![Sicherungsauftrag ausgelöst](./media/tutorial-backup-vm-at-scale/initial-backup-context-menu.png)

3. Klicken Sie in der Liste **Sicherungselemente** auf die Auslassungszeichen ( **...** ), um das Kontextmenü zu öffnen.

4. Wählen Sie im Kontextmenü **Jetzt sichern** aus.

    ![Kontextmenü](./media/tutorial-backup-vm-at-scale/context-menu.png)

    Das Menü „Jetzt sichern“ wird geöffnet.

5. Geben Sie im Menü „Jetzt sichern“ den letzten Tag für die Aufbewahrung des Wiederherstellungspunkts ein, und klicken Sie auf **Sichern**.

    ![Festlegen des letzten Aufbewahrungstags für den Wiederherstellungspunkt für „Jetzt sichern“](./media/tutorial-backup-vm-at-scale/backup-now-short.png)

    Anhand von Bereitstellungsbenachrichtigungen werden Sie darüber informiert, dass der Sicherungsauftrag ausgelöst wurde und dass Sie den Status des Auftrags auf der Seite „Sicherungsaufträge“ überwachen können. Je nach Größe Ihres virtuellen Computers kann das Erstellen der ersten Sicherung einige Zeit dauern.

    Nach Abschluss des Auftrags für die erste Sicherung wird sein Status im Menü „Sicherungsauftrag“ angezeigt. Der manuell ausgelöste Sicherungsauftrag hat den ersten Wiederherstellungspunkt für *myVM* erstellt. Wenn Sie weitere virtuelle Computer sichern möchten, wiederholen Sie diese Schritte für jeden virtuellen Computer.

    ![Kachel „Sicherungsaufträge“](./media/tutorial-backup-vm-at-scale/initial-backup-complete.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie planen, mit den nachfolgenden Tutorials fortzufahren, sollten Sie die in diesem Tutorial erstellten Ressourcen nicht bereinigen. Falls Sie nicht fortfahren möchten, können Sie die folgenden Schritte durchführen, um alle in diesem Tutorial erstellten Ressourcen im Azure-Portal zu löschen.

1. Klicken Sie im Dashboard von **myRecoveryServicesVault** auf die **3** unter **Sicherungselemente**, um das Menü „Sicherungselemente“ zu öffnen.

    ![Symbol „Einstellungen“](./media/tutorial-backup-vm-at-scale/tutorial-vm-back-up-now.png)

2. Klicken Sie im Menü **Sicherungselemente** auf **Virtueller Azure-Computer**, um die Liste der virtuellen Computer, die dem Tresor zugeordnet sind, zu öffnen.

    ![Symbol „Einstellungen“](./media/tutorial-backup-vm-at-scale/three-virtual-machines.png)

    Die Liste **Sicherungselemente** wird geöffnet.

3. Klicken Sie im Menü **Sicherungselemente** auf die Auslassungszeichen (...), um das Kontextmenü zu öffnen.

    ![Symbol „Einstellungen“](./media/tutorial-backup-vm-at-scale/context-menu-to-delete-vm.png)

4. Wählen Sie im Kontextmenü **Sicherung beenden** aus, um das Menü „Sicherung beenden“ zu öffnen.

    ![Symbol „Einstellungen“](./media/tutorial-backup-vm-at-scale/context-menu-for-delete.png)

5. Wählen Sie im Menü **Sicherung beenden** das obere Dropdownmenü und dann **Sicherungsdaten löschen** aus.

6. Geben Sie im Dialogfeld **Geben Sie den Namen des Sicherungselements ein** den Namen *myVM* ein.

7. Nachdem das Sicherungselement überprüft wurde (ein Häkchen wird angezeigt), wird die Schaltfläche **Sicherung beenden** aktiviert. Klicken Sie auf **Sicherung beenden**, um die Richtlinie zu beenden und die Wiederherstellungspunkte zu löschen.

    ![Klicken Sie auf „Sicherung beenden“, um den Tresor zu löschen.](./media/tutorial-backup-vm-at-scale/provide-reason-for-delete.png)

8. Klicken Sie im Menü von **myRecoveryServicesVault** auf **Löschen**.

    ![Klicken Sie auf „Sicherung beenden“, um den Tresor zu löschen.](./media/tutorial-backup-vm-at-scale/deleting-the-vault.png)

    Nachdem der Tresor gelöscht wurde, wird wieder die Liste der Recovery Services-Tresore angezeigt.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie das Azure-Portal zu folgenden Zwecken verwendet:

> [!div class="checklist"]
>
> * Erstellen eines Recovery Services-Tresors
> * Festlegen des Tresors zum Schutz von virtuellen Computern
> * Erstellen einer benutzerdefinierte Sicherungs- und Aufbewahrungsrichtlinie
> * Zuweisen der Sicherungsrichtlinie zum Schützen mehrerer virtueller Computer
> * Auslösen einer bedarfsabhängigen Sicherung für virtuelle Computer

Im nächsten Tutorial stellen Sie einen virtuellen Azure-Computer von einem Datenträger wieder her.

> [!div class="nextstepaction"]
> [Verwalten von virtuellen Computern mithilfe der Befehlszeilenschnittstelle](./tutorial-restore-disk.md)
