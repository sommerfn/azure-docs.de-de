---
title: Sicherheitsfeatures für den Schutz von Cloudworkloads mit Azure Backup
description: In diesem Artikel wird erläutert, wie Sie mit den Azure Backup-Sicherheitsfunktionen für mehr Sicherheit für Ihre Sicherungen sorgen können.
author: dcurwin
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: dacurwin
ms.openlocfilehash: 4fb88cbed4e73a7cea2b0ccf01b1429a3ff321f3
ms.sourcegitcommit: 71db032bd5680c9287a7867b923bf6471ba8f6be
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/16/2019
ms.locfileid: "71018179"
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

## <a name="other-security-features"></a>Weitere Sicherheitsfeatures

### <a name="storage-side-encryption"></a>Speicherseitige Verschlüsselung

Azure Storage verschlüsselt Ihre Daten beim Speichern in der Cloud automatisch. Die Verschlüsselung schützt Ihre Daten und unterstützt Sie beim Einhalten der Sicherheits- und Complianceanforderungen Ihrer Organisation. Daten in Azure Storage werden auf transparente Weise mit der AES-256-Verschlüsselung – einer der stärksten verfügbaren Blockchiffren – ver- und entschlüsselt und sind mit dem FIPS 140-2-Standard konform. Die Azure Storage-Verschlüsselung ähnelt der BitLocker-Verschlüsselung unter Windows. Mit Azure Backup werden Daten vor dem Speichern automatisch verschlüsselt. Azure Storage entschlüsselt Daten vor dem Abrufen.  

In Azure werden Daten bei der Übertragung zwischen Azure Storage und dem Tresor per HTTPS geschützt. Diese Daten bleiben im Azure-Backbone-Netzwerk.

Weitere Informationen finden Sie unter [Azure Storage-Verschlüsselung für ruhende Daten](https://docs.microsoft.com/en-in/azure/storage/common/storage-service-encryption).

### <a name="vm-encryption"></a>VM-Verschlüsselung

Sie können virtuelle Azure-Computer (VMs) unter Windows oder Linux mit verschlüsselten Datenträgern mithilfe des Diensts Azure Backup sichern und wiederherstellen. Anleitungen finden Sie unter [Sichern und Wiederherstellen verschlüsselter virtueller Computer mit Azure Backup](https://docs.microsoft.com/en-us/azure/backup/backup-azure-vms-encryption).

### <a name="protection-of-azure-backup-recovery-points"></a>Schutz von Azure Backup-Wiederherstellungspunkten

Von Recovery Services-Tresoren verwendete Speicherkonten sind isoliert, sodass böswillige Akteure keinen Zugriff darauf haben. Der Zugriff ist nur über Azure Backup-Verwaltungsvorgänge, z. B. eine Wiederherstellung, zulässig. Diese Verwaltungsvorgänge werden per rollenbasierter Zugriffssteuerung (Role-Based Access Control, RBAC) gesteuert.

Weitere Informationen finden Sie unter [Verwenden der rollenbasierten Zugriffssteuerung zum Verwalten von Azure Backup-Wiederherstellungspunkten](https://docs.microsoft.com/en-us/azure/backup/backup-rbac-rs-vault).

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

#### <a name="how-can-i-delete-the-data-earlier-than-the-14-days-soft-delete-period-after-deletion"></a>Wie kann ich die Daten löschen, bevor die 14 Tage im Status „Vorläufig gelöscht“ abgelaufen sind?

Es gibt keine Möglichkeit, die Daten vor Ablauf der 14 Tage nach dem Löschvorgang endgültig zu löschen. Wenden Sie sich an den Microsoft-Support, wenn es sich um ein Blocker- oder Konformitätsproblem handelt.

#### <a name="can-soft-delete-operations-be-performed-in-powershell-or-cli"></a>Kann das vorläufige Löschen in PowerShell oder per CLI durchgeführt werden?

Nein. Derzeit ist keine Unterstützung für PowerShell oder die CLI vorhanden.

#### <a name="is-soft-delete-supported-for-other-cloud-workloads-like-sql-server-in-azure-vms-and-sap-hana-in-azure-vms"></a>Wird das vorläufige Löschen für andere Cloudworkloads unterstützt, z. B. SQL Server auf Azure-VMs und SAP HANA auf Azure-VMs?

Nein. Das vorläufige Löschen wird derzeit nur für virtuelle Azure-Computer unterstützt.

## <a name="next-steps"></a>Nächste Schritte

* Informieren Sie sich über [Sicherheitskontrollen für Azure Backup](backup-security-controls.md).
