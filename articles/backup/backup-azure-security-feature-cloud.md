---
title: Sicherheitsfeatures für den Schutz von Cloudworkloads mit Azure Backup
description: In diesem Artikel wird erläutert, wie Sie mit den Azure Backup-Sicherheitsfunktionen für mehr Sicherheit für Ihre Sicherungen sorgen können.
author: dcurwin
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: dacurwin
ms.openlocfilehash: f0e4540f3f5ab3fdbb5953cbf100c5fdc2b2542a
ms.sourcegitcommit: 6c2c97445f5d44c5b5974a5beb51a8733b0c2be7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2019
ms.locfileid: "73622012"
---
# <a name="security-features-to-help-protect-cloud-workloads-that-use-azure-backup"></a>Sicherheitsfeatures für den Schutz von Cloudworkloads mit Azure Backup

Die Sorgen bezüglich Sicherheitsproblemen wie Schadsoftware, Ransomware und Eindringlingen werden immer größer. Diese Sicherheitsprobleme können erhebliche Daten- und finanzielle Verluste mit sich bringen. Zum Schutz gegen solche Angriffe verfügt Azure Backup jetzt über Sicherheitsfeatures für den Schutz von Sicherungsdaten auch nach dem Löschen. Eines dieser Features ist das vorläufige Löschen. Beim vorläufigen Löschen werden die Sicherungsdaten 14 Tage länger aufbewahrt, damit das jeweilige Sicherungselement auch dann ohne Datenverluste wiederhergestellt werden kann, wenn ein böswilliger Akteur die Sicherung einer VM löscht oder die Sicherungsdaten versehentlich gelöscht werden. Für diese zusätzlichen 14 Tage der Aufbewahrung von Sicherungsdaten mit dem Status „Vorläufiges Löschen“ fallen für Kunden keine Kosten an.

> [!NOTE]
> Mit dem vorläufigen Löschen werden nur gelöschte Sicherungsdaten geschützt. Wenn eine VM ohne Sicherung gelöscht wird, bewirkt das Feature „Vorläufiges Löschen“ nicht, dass die Daten erhalten bleiben. Alle Ressourcen sollten per Azure Backup geschützt werden, um die vollständige Resilienz sicherzustellen.
>

## <a name="soft-delete"></a>Vorläufiges Löschen

### <a name="supported-regions"></a>Unterstützte Regionen

Vorläufiges Löschen wird derzeit in unterstützt in USA, Westen-Mitte; Asien, Osten; Kanada, Mitte; Kanada, Osten; Frankreich, Mitte; Frankreich, Süden; Südkorea, Mitte; Südkorea, Süden; Vereinigtes Königreich, Süden; Vereinigtes Königreich, Westen; Australien, Osten; Australien, Südosten; Europa, Norden; USA, Westen; USA, Westen 2; USA, Mitte; Asien, Südosten; USA, Norden-Mitte, USA, Süden-Mitte; Japan, Osten; Japan, Westen; Indien, Süden; Indien, Mitte; Indien, Westen; USA, Osten 2; Schweiz, Norden; Schweiz, Westen und allen nationalen Regionen.

### <a name="soft-delete-for-vms"></a>Vorläufiges Löschen für VMs

1. Der Sicherungsvorgang muss beendet werden, um die Sicherungsdaten einer VM zu löschen. Navigieren Sie im Azure-Portal zu Ihrem Recovery Services-Tresor, klicken Sie mit der rechten Maustaste auf das Sicherungselement, und wählen Sie die Option **Sicherung beenden** aus.

   ![Screenshot: Sicherungselemente im Azure-Portal](./media/backup-azure-security-feature-cloud/backup-stopped.png)

2. Im folgenden Fenster erhalten Sie die Möglichkeit, die Sicherungsdaten zu löschen oder zu speichern. Wenn Sie die Option **Sicherungsdaten löschen** und dann **Sicherung beenden** wählen, wird die VM-Sicherung nicht endgültig gelöscht. Stattdessen werden die Sicherungsdaten 14 Tage lang im Zustand „Vorläufig gelöscht“ aufbewahrt. Wenn Sie die Option **Sicherungsdaten löschen** auswählen, wird eine E-Mail-Benachrichtigung an die konfigurierte E-Mail-ID gesendet. Hiermit wird der Benutzer darüber informiert, dass die Aufbewahrungsdauer für die Sicherungsdaten um 14 Tage verlängert wurde. Darüber hinaus wird am 12. Tag eine E-Mail-Benachrichtigung mit dem Hinweis gesendet, dass zwei weitere Tage verbleiben, um die gelöschten Daten wiederherzustellen. Das endgültige Löschen wird erst am 15. Tag durchgeführt, und es wird eine abschließende E-Mail-Benachrichtigung gesendet, um den Benutzer über die endgültige Löschung der Daten zu informieren.

   ![Screenshot: „Sicherung beenden“ im Azure-Portal](./media/backup-azure-security-feature-cloud/delete-backup-data.png)

3. Während dieser 14 Tage wird die vorläufig gelöschte VM im Recovery Services-Tresor mit einem roten Symbol gekennzeichnet, mit dem der Status „Vorläufig gelöscht“ angezeigt wird.

   ![Screenshot: Vorläufig gelöschte VM im Azure-Portal](./media/backup-azure-security-feature-cloud/vm-soft-delete.png)

   > [!NOTE]
   > Wenn im Tresor vorläufig gelöschte Sicherungselemente vorhanden sind, kann der Tresor nicht gelöscht werden. Versuchen Sie erst, den Tresor zu löschen, nachdem die Sicherungselemente endgültig gelöscht wurden und im Tresor kein Element mit dem Status „Vorläufig gelöscht“ mehr vorhanden ist.

4. Das Löschen muss zuerst wieder rückgängig gemacht werden, um die vorläufig gelöschte VM wiederherstellen zu können. Wählen Sie zum Rückgängigmachen des Löschvorgangs die vorläufig gelöschte VM aus, und klicken Sie anschließend auf die Option **Wiederherstellen**.

   ![Screenshot: Wiederherstellen der VM im Azure-Portal](./media/backup-azure-security-feature-cloud/choose-undelete.png)

   Es wird ein Fenster mit einer Warnung angezeigt, dass bei Auswahl der Option „Wiederherstellen“ alle Wiederherstellungspunkte für die VM wiederhergestellt werden und für die Durchführung eines Wiederherstellungsvorgangs verfügbar sind. Für die VM gilt der Status „Beendigung des Schutzes mit Beibehaltung der Daten“. Die Sicherungen sind angehalten, und die Sicherungsdaten werden ohne zeitliche Beschränkung aufbewahrt, ohne dass eine Sicherungsrichtlinie gilt.

   ![Screenshot: Bestätigung der VM-Wiederherstellung im Azure-Portal](./media/backup-azure-security-feature-cloud/undelete-vm.png)

   An diesem Punkt können Sie die VM auch wiederherstellen, indem Sie für den ausgewählten Wiederherstellungspunkt die Option **Virtuellen Computer wiederherstellen** auswählen.  

   ![Screenshot: Option „Virtuellen Computer wiederherstellen“ im Azure-Portal](./media/backup-azure-security-feature-cloud/restore-vm.png)

   > [!NOTE]
   > Der Garbage Collector zum Bereinigen abgelaufener Wiederherstellungspunkte wird erst ausgeführt, nachdem der Benutzer den Vorgang **Sicherung fortsetzen** durchgeführt hat.

5. Nachdem der Wiederherstellungsprozess abgeschlossen ist, wird der Status auf „Sicherung beenden mit Daten beibehalten“ zurückgesetzt, und anschließend können Sie die Option **Sicherung fortsetzen** auswählen. Mit **Sicherung fortsetzen** wird das Sicherungselement wieder in den aktiven Zustand versetzt, und es wird eine vom Benutzer ausgewählte Sicherungsrichtlinie zugeordnet, mit der die Sicherungs- und Aufbewahrungszeitpläne definiert werden.

   ![Screenshot: Option „Sicherung fortsetzen“ im Azure-Portal](./media/backup-azure-security-feature-cloud/resume-backup.png)

In diesem Flussdiagramm sind die unterschiedlichen Schritte und Status eines Sicherungselements dargestellt:

![Lebenszyklus eines vorläufig gelöschten Sicherungselements](./media/backup-azure-security-feature-cloud/lifecycle.png)

Weitere Informationen finden Sie unten im Abschnitt [Häufig gestellte Fragen](backup-azure-security-feature-cloud.md#frequently-asked-questions).

## <a name="disabling-soft-delete"></a>Deaktivieren des vorläufigen Löschens

Das vorläufige Löschen ist bei neu erstellten Tresoren standardmäßig aktiviert. Wenn das Sicherheitsfeature für vorläufiges Löschen deaktiviert ist, sind die Sicherungsdaten nicht vor versehentlichen oder böswilligen Löschvorgängen geschützt. Ohne das Feature für vorläufiges Löschen führen alle Löschvorgänge von geschützten Elementen zu einer sofortigen Entfernung, ohne die Möglichkeit zur Wiederherstellung. Da für den Kunden keine Kosten für die Sicherung von Daten im Zustand „Vorläufiges Löschen“ anfallen, wird die Deaktivierung dieses Features nicht empfohlen. Sie sollten das vorläufige Löschen nur dann deaktivieren, wenn Sie planen, Ihre geschützten Elemente in einen neuen Tresor zu verschieben, und nicht die 14 Tage warten können, die für das Löschen und erneute Schützen erforderlich sind (z. B. in einer Testumgebung).

### <a name="prerequisites-for-disabling-soft-delete"></a>Voraussetzungen für das Deaktivieren des vorläufigen Löschens

- Das Aktivieren oder Deaktivieren des vorläufigen Löschens für Tresore (ohne geschützte Elemente) kann nur über das Azure-Portal erfolgen. Dies gilt für:
  - Neu erstellte Tresore, die keine geschützten Elemente enthalten.
  - Vorhandene Tresore, deren geschützte Objekte gelöscht und abgelaufen sind (über den festgelegten 14-tägigen Aufbewahrungszeitraum hinaus).
- Wenn das Feature für vorläufiges Löschen für den Tresor deaktiviert ist, können Sie es erneut aktivieren, aber Sie können diese Option nicht rückgängig machen und wieder deaktivieren, wenn der Tresor geschützte Elemente enthält.
- Sie können das Feature für vorläufiges Löschen für Tresore, die geschützte Elemente enthalten oder sich im Zustand „vorläufig gelöscht“ befinden, nicht deaktivieren. Führen Sie in diesem Fall die folgenden Schritte aus:
  - Beenden Sie den Schutz der gelöschten Daten für alle geschützten Elemente.
  - Warten Sie, bis die 14 Tage des Sicherheitsaufbewahrungszeitraums abgelaufen sind.
  - Deaktivieren Sie das vorläufige Löschen.

Um das vorläufige Löschen zu deaktivieren, stellen Sie sicher, dass die Voraussetzungen erfüllt sind, und führen Sie dann die folgenden Schritte aus:

1. Wechseln Sie im Azure-Portal zu Ihrem Tresor und dann zu **Einstellungen** -> **Eigenschaften**.
2. Wählen Sie im Eigenschaftenbereich **Sicherheitseinstellungen** -> **Aktualisieren** aus.
3. Wählen Sie im Bereich „Sicherheitseinstellungen“ unter „Vorläufiges Löschen“ die Option **Deaktivieren** aus.

![Deaktivieren des vorläufigen Löschens](./media/backup-azure-security-feature-cloud/disable-soft-delete.png)

## <a name="other-security-features"></a>Weitere Sicherheitsfeatures

### <a name="storage-side-encryption"></a>Speicherseitige Verschlüsselung

Azure Storage verschlüsselt Ihre Daten beim Speichern in der Cloud automatisch. Die Verschlüsselung schützt Ihre Daten und unterstützt Sie beim Einhalten der Sicherheits- und Complianceanforderungen Ihrer Organisation. Daten in Azure Storage werden auf transparente Weise mit der AES-256-Verschlüsselung – einer der stärksten verfügbaren Blockchiffren – ver- und entschlüsselt und sind mit dem FIPS 140-2-Standard konform. Die Azure Storage-Verschlüsselung ähnelt der BitLocker-Verschlüsselung unter Windows. Mit Azure Backup werden Daten vor dem Speichern automatisch verschlüsselt. Azure Storage entschlüsselt Daten vor dem Abrufen.  

In Azure werden Daten bei der Übertragung zwischen Azure Storage und dem Tresor per HTTPS geschützt. Diese Daten bleiben im Azure-Backbone-Netzwerk.

Weitere Informationen finden Sie unter [Azure Storage-Verschlüsselung für ruhende Daten](https://docs.microsoft.com/azure/storage/common/storage-service-encryption).

### <a name="vm-encryption"></a>VM-Verschlüsselung

Sie können virtuelle Azure-Computer (VMs) unter Windows oder Linux mit verschlüsselten Datenträgern mithilfe des Diensts Azure Backup sichern und wiederherstellen. Anleitungen finden Sie unter [Sichern und Wiederherstellen verschlüsselter virtueller Computer mit Azure Backup](https://docs.microsoft.com/azure/backup/backup-azure-vms-encryption).

### <a name="protection-of-azure-backup-recovery-points"></a>Schutz von Azure Backup-Wiederherstellungspunkten

Von Recovery Services-Tresoren verwendete Speicherkonten sind isoliert, sodass böswillige Akteure keinen Zugriff darauf haben. Der Zugriff ist nur über Azure Backup-Verwaltungsvorgänge, z. B. eine Wiederherstellung, zulässig. Diese Verwaltungsvorgänge werden per rollenbasierter Zugriffssteuerung (Role-Based Access Control, RBAC) gesteuert.

Weitere Informationen finden Sie unter [Verwenden der rollenbasierten Zugriffssteuerung zum Verwalten von Azure Backup-Wiederherstellungspunkten](https://docs.microsoft.com/azure/backup/backup-rbac-rs-vault).

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

### <a name="soft-delete"></a>Vorläufiges Löschen

#### <a name="do-i-need-to-enable-the-soft-delete-feature-on-every-vault"></a>Muss ich die Funktion für vorläufiges Löschen in jedem Tresor aktivieren?

Nein. Sie wird standardmäßig für alle Recovery Services-Tresore erstellt und aktiviert.

#### <a name="can-i-configure-the-number-of-days-for-which-my-data-will-be-retained-in-soft-deleted-state-after-delete-operation-is-complete"></a>Kann ich konfigurieren, wie lange meine Daten nach Abschluss des Löschvorgangs im Status „Vorläufig gelöscht“ aufbewahrt werden?

Nein. Der Zeitraum ist auf eine um 14 Tage verlängerte Aufbewahrung nach dem Löschvorgang festgelegt.

#### <a name="do-i-need-to-pay-the-cost-for-this-additional-14-day-retention"></a>Muss ich die Kosten für diesen zusätzlichen Aufbewahrungszeitraum von 14 Tagen bezahlen?

Nein. Dieser zusätzliche Aufbewahrungszeitraum von 14 Tagen ist im Rahmen der Funktion „Vorläufiges Löschen“ kostenlos.

#### <a name="can-i-perform-a-restore-operation-when-my-data-is-in-soft-delete-state"></a>Kann ich einen Wiederherstellungsvorgang durchführen, wenn sich meine Daten im Status „Vorläufig gelöscht“ befinden?

Nein. Sie müssen das Löschen der vorläufig gelöschten Ressource rückgängig machen, um sie wiederherzustellen. Mit dem Vorgang „Wiederherstellen“ wird die Ressource zurück in den Status **Beendigung des Schutzes mit Beibehaltung der Daten** versetzt, und Sie können beliebige Wiederherstellungspunkte wiederherstellen. Der Garbage Collector bleibt in diesem Status angehalten.

#### <a name="will-my-snapshots-follow-the-same-lifecycle-as-my-recovery-points-in-the-vault"></a>Gilt für meine Momentaufnahmen der gleiche Lebenszyklus wie für meine Wiederherstellungspunkte im Tresor?

Ja.

#### <a name="how-can-i-trigger-the-scheduled-backups-again-for-a-soft-deleted-resource"></a>Wie kann ich die geplanten Sicherungen für eine vorläufig gelöschte Ressource erneut auslösen?

Wenn Sie nach dem Wiederherstellen den Vorgang „Fortsetzen“ ausführen, ist die Ressource wieder geschützt. Beim Vorgang „Fortsetzen“ wird eine Sicherungsrichtlinie zugeordnet, um die geplanten Sicherungen mit dem ausgewählten Aufbewahrungszeitraum auszulösen. Darüber hinaus wird der Garbage Collector ausgeführt, sobald der Vorgang „Fortsetzen“ abgeschlossen ist. Falls Sie eine Wiederherstellung für einen Wiederherstellungspunkt durchführen möchten, für den das Ablaufdatum überschritten wurde, sollten Sie dies tun, bevor Sie den Wiederherstellungsvorgang auslösen.

#### <a name="can-i-delete-my-vault-if-there-are-soft-deleted-items-in-the-vault"></a>Kann ich meinen Tresor löschen, wenn er vorläufig gelöschte Elemente enthält?

Der Recovery Services-Tresor kann nicht gelöscht werden, wenn sich darin Sicherungselemente mit dem Status „Vorläufig gelöscht“ befinden. Die vorläufig gelöschten Elemente werden 14 Tage nach dem ersten Löschvorgang endgültig gelöscht. Sie können den Tresor erst löschen, nachdem alle vorläufig gelöschten Elemente entfernt wurden.  

#### <a name="can-i-delete-the-data-earlier-than-the-14-days-soft-delete-period-after-deletion"></a>Kann ich die Daten löschen, bevor die 14 Tage im Status „Vorläufig gelöscht“ abgelaufen sind?

Nein. Das Löschen der vorläufig gelöschten Elemente kann nicht erzwungen werden; sie werden nach 14 Tagen automatisch gelöscht. Diese Sicherheitsfunktion ist aktiviert, um die gesicherten Daten vor versehentlichen oder böswilligen Löschvorgängen zu schützen.  Sie sollten 14 Tage warten, bevor Sie eine andere Aktion auf dem virtuellen Computer ausführen.  Vorläufig gelöschte Elemente werden nicht in Rechnung gestellt.  Wenn Sie die zum vorläufigen Löschen gekennzeichneten VMs innerhalb von 14 Tagen in einem neuen Tresor erneut schützen müssen, wenden Sie sich an den Microsoft-Support.

#### <a name="can-soft-delete-operations-be-performed-in-powershell-or-cli"></a>Kann das vorläufige Löschen in PowerShell oder per CLI durchgeführt werden?

Nein. Derzeit ist keine Unterstützung für PowerShell oder die CLI vorhanden.

#### <a name="is-soft-delete-supported-for-other-cloud-workloads-like-sql-server-in-azure-vms-and-sap-hana-in-azure-vms"></a>Wird das vorläufige Löschen für andere Cloudworkloads unterstützt, z. B. SQL Server auf Azure-VMs und SAP HANA auf Azure-VMs?

Nein. Das vorläufige Löschen wird derzeit nur für virtuelle Azure-Computer unterstützt.

## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich über [Sicherheitskontrollen für Azure Backup](backup-security-controls.md).
