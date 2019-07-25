---
title: Migrieren von lokalen VMware-VMs zu Azure per Azure Migrate-Servermigration ohne Agent | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie die Migration von lokalen VMware-VMs zu Azure ohne Agent durchführen, indem Sie Azure Migrate verwenden.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 07/08/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 3510c0505a5a3c1353642baf5060a83d13fdd43a
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67808110"
---
# <a name="migrate-vmware-vms-to-azure-agentless"></a>Migrieren von VMware-VMs zu Azure (ohne Agent)

In diesem Artikel wird veranschaulicht, wie Sie lokale VMware-VMs zu Azure migrieren, indem Sie die Migration ohne Agent mit dem Tool für die Azure Migrate-Servermigration durchführen.

[Azure Migrate](migrate-services-overview.md) ist ein zentraler Hub zum Nachverfolgen der Ermittlung, Bewertung und Migration Ihrer lokalen Apps und Workloads sowie von AWS/GCP-VM-Instanzen zu Azure. Der Hub stellt Azure Migrate-Tools für die Bewertung und Migration sowie unabhängige Drittanbietertools bereit.

Dieses Tutorial ist das dritte in einer Reihe, mit der veranschaulicht wird, wie Sie VMware-VMs bewerten und zu Azure migrieren, indem Sie die Azure Migrate-Serverbewertung und -migration nutzen. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Vorbereiten von virtuellen Computern für die Migration
> * Hinzufügen des Tools für die Azure Migrate-Servermigration
> * Ermitteln der zu migrierenden VMs
> * Beginnen mit der Replikation von VMs
> * Durchführen einer Testmigration, um sicherzustellen, dass alles wie erwartet funktioniert
> * Durchführen einer vollständigen VM-Migration

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) erstellen, bevor Sie beginnen.

## <a name="migration-methods"></a>Migrationsmethoden

Sie können VMware-VMs zu Azure migrieren, indem Sie das Tool für die Azure Migrate-Servermigration verwenden. Dieses Tool verfügt über Optionen für die Migration von VMware-VMs:

- Durchführen einer Migration per Replikation ohne Agent Migrieren von VMs, ohne dass darauf Software installiert werden muss
- Durchführen einer Migration mit einem Agent für die Replikation Installieren eines Agents für die Replikation auf der VM

Verwenden Sie die folgenden Artikel beim Treffen der Entscheidung, ob Sie die Migration mit oder ohne Agent durchführen möchten:

- Informieren Sie sich über die [Migration ohne Agent](server-migrate-overview.md) und die [Einschränkungen](server-migrate-overview.md#agentless-migration-limitations).
- [Lesen Sie diesen Artikel](tutorial-migrate-vmware-agent.md), wenn Sie das Verfahren mit Agent nutzen möchten.

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie mit diesem Tutorial beginnen, sollten folgende Voraussetzungen erfüllt sein:

1. [Machen Sie sich mit der Architektur der VMware-Migration vertraut](migrate-architecture.md).
2. Arbeiten Sie das [erste Tutorial dieser Reihe](tutorial-prepare-vmware.md) durch, um Azure und VMware für die Migration einzurichten. In diesem Tutorial geht es vor allem um Folgendes:
    - [Vorbereiten von Azure](tutorial-prepare-vmware.md#prepare-azure) für die Migration
    - [Vorbereiten der lokalen Umgebung](tutorial-prepare-vmware.md#prepare-for-agentless-vmware-migration) für die Migration
    
3. Wir empfehlen Ihnen, VMware-VMs nach Möglichkeit mit der Azure Migrate-Serverbewertung zu bewerten, bevor Sie diese zu Azure migrieren. Arbeiten Sie das [zweite Tutorial der Reihe](tutorial-assess-vmware.md) durch, um die Bewertung einzurichten. Falls Sie die VMs nicht bewerten möchten, können Sie dieses Tutorial überspringen. Wir empfehlen Ihnen zwar, die Bewertung auszuprobieren, aber dies ist nicht unbedingt erforderlich, bevor Sie die Migration testen.



## <a name="add-the-azure-migrate-server-migration-tool"></a>Hinzufügen des Tools für die Azure Migrate-Servermigration

Falls Sie das zweite Tutorial zur Bewertung von VMware-VMs nicht durchgearbeitet haben, müssen Sie [diese Anleitung befolgen](how-to-add-tool-first-time.md), um ein Azure Migrate-Projekt einzurichten, und dann das Tool für die Azure Migrate-Servermigration auswählen. 

Wenn Sie das zweite Tutorial durchgearbeitet und bereits ein Azure Migrate-Projekt eingerichtet haben, können Sie das Tool für die Azure Migrate-Servermigration wie folgt hinzufügen:

1. Klicken Sie im Azure Migrate-Projekt auf **Übersicht**. 
2. Klicken Sie unter **Server ermitteln, bewerten und migrieren** auf **Server bewerten und migrieren**.

     ![Bewerten und Migrieren von Servern](./media/tutorial-migrate-vmware/assess-migrate.png)

3. Wählen Sie unter **Migrationstools** die Option **Click here to add a migration tool when you are ready to migrate** (Klicken Sie hier, um ein Migrationstool hinzuzufügen, wenn Sie bereit für die Migration sind).

    ![Auswählen eines Tools](./media/tutorial-migrate-vmware/select-migration-tool.png)

4. Wählen Sie in der Liste mit den Tools die Option **Azure Migrate: Servermigration** > **Tool hinzufügen**.

    ![Tool für die Servermigration](./media/tutorial-migrate-vmware/server-migration-tool.png)

## <a name="set-up-the-azure-migrate-appliance"></a>Einrichten der Azure Migrate-Appliance

Für die Azure Migrate-Servermigration wird eine einfache VMware-VM-Appliance ausgeführt. Die Appliance ermittelt virtuelle Computer und sendet Meta- und Leistungsdaten zu virtuellen Computern an die Azure Migrate-Servermigration. Die gleiche Appliance wird auch vom Tool für die Azure Migrate-Serverbewertung verwendet.

Wenn Sie das zweite Tutorial zum Bewerten von VMware-VMs durchgearbeitet haben, haben Sie die Appliance dabei bereits eingerichtet. Wenn nicht, müssen Sie die Appliance jetzt einrichten. Führen Sie dazu die folgenden Schritte aus: 

- Herunterladen einer OVA-Vorlagendatei und Importieren der Datei in vCenter Server
- Erstellen der Appliance und Durchführen einer Überprüfung, ob damit eine Verbindung mit der Azure Migrate-Serverbewertung hergestellt werden kann 
- Durchführen der Erstkonfiguration für die Appliance und Registrieren der Appliance beim Azure Migrate-Projekt

Befolgen Sie die Anleitung zum Einrichten der Appliance in [diesem Artikel](how-to-set-up-appliance-vmware.md).


## <a name="prepare-vms-for-migration"></a>Vorbereiten von virtuellen Computern für die Migration

Für Azure Migrate müssen einige VM-Änderungen durchgeführt werden, um sicherzustellen, dass VMs zu Azure migriert werden können.

- Bei einigen [Betriebssystemen](server-migrate-overview.md#agentless-migration-limitations) führt Azure Migrate diese Änderungen automatisch durch.
- Befolgen Sie die Anleitung zum Vorbereiten des virtuellen Computers, wenn Sie einen virtuellen Computer migrieren, der nicht über eines dieser Betriebssysteme verfügt.
- Es ist wichtig, diese Änderungen vorzunehmen, bevor Sie mit der Migration beginnen. Wenn Sie den virtuellen Computer migrieren, bevor Sie die Änderung vorgenommen haben, wird der virtuelle Computer in Azure unter Umständen nicht gestartet.
- Konfigurationsänderungen, die Sie auf lokalen virtuellen Computern vornehmen, werden nach dem Aktivieren der Replikation für den virtuellen Computer in Azure repliziert. Gehen Sie wie folgt vor, um sicherzustellen, dass die Änderungen repliziert wurden: Vergewissern Sie sich, dass der Wiederherstellungspunkt der Migration nach dem Zeitpunkt liegt, zu dem die Konfigurationsänderungen lokal vorgenommen wurden.


### <a name="prepare-windows-server-vms"></a>Vorbereiten von Windows Server-VMs

**Aktion** | **Details** | **Anleitung**
--- | --- | ---
Stellen Sie sicher, dass für Windows-Volumes auf der Azure-VM die gleichen Laufwerkbuchstaben wie auf der lokalen VM verwendet werden. | Legen Sie die SAN-Richtlinie auf „Online – Alle“ fest. | 1. Melden Sie sich an der VM mit einem Administratorkonto an, und öffnen Sie ein Befehlsfenster.<br/> 2. Geben Sie **diskpart** ein, um das Hilfsprogramm DiskPart auszuführen.<br/> 3. Geben Sie **SAN POLICY=OnlineAll** ein.<br/> 4. Geben Sie „Exit“ ein, um DiskPart zu verlassen, und schließen Sie die Eingabeaufforderung.
Aktivieren der Azure-Konsole für den seriellen Zugriff für die Azure-VM | Dies dient Ihnen als Hilfe bei der Problembehandlung. Sie müssen den virtuellen Computer nicht neu starten. Die Azure-VM wird mit dem Datenträgerimage gestartet, und dies entspricht einem Neustart der neuen VM. | Befolgen Sie zur Aktivierung [diese Anleitung](https://docs.microsoft.com/azure/virtual-machines/windows/serial-console#enable-serial-console-in-custom-or-older-images).
Installieren der Hyper-V-Gastintegration | Wenn Sie Computer migrieren, auf denen Windows Server 2003 ausgeführt wird, müssen Sie die Dienste für die Hyper-V-Gastintegration unter dem Betriebssystem der VM installieren. | [Weitere Informationen](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-integration-services#install-or-update-integration-services)
Remotedesktop | Aktivieren Sie Remotedesktop auf der VM, und vergewissern Sie sich, dass die Windows-Firewall den Remotedesktopzugriff für die Netzwerkprofile nicht blockiert. | [Weitere Informationen](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/remote-desktop-allow-access)

### <a name="prepare-linux-vms"></a>Vorbereiten von Linux-VMs

**Aktion** | **Details** 
--- | --- | ---
Installieren von Linux Integration Services für Hyper-V | In den meisten neuen Versionen von Linux-Distributionen ist dies standardmäßig enthalten.
Neuerstellen des Linux-Initialisierungsimages, damit es die erforderlichen Hyper-V-Treiber enthält | Hiermit wird sichergestellt, dass der virtuelle Computer in Azure gestartet wird. Dies ist nur in einigen Distributionen erforderlich.
Aktivieren der Protokollierung für die serielle Azure-Konsole | Dies dient Ihnen als Hilfe bei der Problembehandlung. Sie müssen den virtuellen Computer nicht neu starten. Die Azure-VM wird mit dem Datenträgerimage gestartet, und dies entspricht einem Neustart der neuen VM.<br/> Befolgen Sie zur Aktivierung [diese Anleitung](https://docs.microsoft.com/azure/virtual-machines/linux/serial-console).
Aktualisieren der Gerätezuordnungsdatei | Aktualisieren Sie die Gerätezuordnungsdatei, die den Gerätenamen enthält, auf Volumezuordnungen, damit persistente Gerätebezeichner verwendet werden.
Aktualisieren von fstab-Einträgen | Aktualisieren Sie die Einträge so, dass persistente Volumebezeichner verwendet werden.
Entfernen der udev-Regel | Entfernen Sie alle udev-Regeln, mit denen Schnittstellennamen basierend auf der MAC-Adresse usw. reserviert werden.
Aktualisieren von Netzwerkschnittstellen | Aktualisieren Sie die Netzwerkschnittstellen so, dass IP-Adressen basierend auf DHCP empfangen werden.
Aktivieren von SSH | Stellen Sie sicher, dass SSH aktiviert und für den SSHD-Dienst das Starten während des Neustartvorgangs festgelegt ist.<br/> Stellen Sie sicher, dass eingehende SSH-Verbindungsanforderungen nicht durch die Firewall des Betriebssystems oder skriptfähige Regeln blockiert werden.

[Befolgen Sie die Anleitung in diesem Artikel](https://docs.microsoft.com/azure/virtual-machines/linux/create-upload-generic), in dem die Schritte zum Ausführen einer Linux-VM in Azure beschrieben sind. Außerdem sind Anleitungen für einige beliebte Linux-Distributionen enthalten.  


## <a name="replicate-vms"></a>Replizieren von VMs

Nachdem die Ermittlung abgeschlossen ist, können Sie mit der Replikation von VMware-VMs in Azure beginnen.

1. Klicken Sie im Azure Migrate-Projekt unter **Server** > **Azure Migrate: Servermigration** auf **Replizieren**.

    ![Replizieren von VMs](./media/tutorial-migrate-vmware/select-replicate.png)

2. Wählen Sie unter **Replizieren** > **Quelleinstellungen** > **Sind Ihre Computer virtualisiert?** die Option **Ja, mit VMware vSphere-Hypervisor**.
3. Wählen Sie unter **Lokale Appliance** den Namen der Azure Migrate-Appliance aus, die Sie eingerichtet haben, und dann **OK**. 

    ![Quelleinstellungen](./media/tutorial-migrate-vmware/source-settings.png)

    - In diesem Schritt wird davon ausgegangen, dass Sie beim Durcharbeiten des Tutorials bereits eine Appliance eingerichtet haben.
    - Falls Sie die Einrichtung einer Appliance noch nicht durchgeführt haben, sollten Sie die Anleitung in [diesem Artikel](how-to-set-up-appliance-vmware.md) befolgen.

4. Wählen Sie unter **Virtuelle Computer** die Computer aus, die Sie replizieren möchten.
    - Wenn Sie eine Bewertung für die virtuellen Computer durchgeführt haben, können Sie die Empfehlungen zur VM-Größenanpassung und zum Datenträgertyp (Premium/Standard) aus den Bewertungsergebnissen anwenden. Wählen Sie hierzu unter **Migrationseinstellungen aus einer Azure Migrate-Bewertung importieren?** die Option **Ja**.
    - Wählen Sie **Nein**, wenn Sie keine Bewertung durchgeführt haben oder die Bewertungseinstellungen nicht verwenden möchten.
    - Wählen Sie die VM-Gruppe und den Bewertungsnamen aus, wenn Sie die Bewertung gewählt haben.

    ![Auswählen der Bewertung](./media/tutorial-migrate-vmware/select-assessment.png)

5. Suchen Sie unter **Virtuelle Computer** je nach Bedarf nach VMs, und aktivieren Sie alle VMs, die Sie migrieren möchten. Klicken Sie anschließend auf **Next: Zieleinstellungen**.

    ![Auswählen von VMs](./media/tutorial-migrate-vmware/select-vms.png)

6. Wählen Sie unter **Zieleinstellungen** das Abonnement und die Zielregion für die Migration aus, und geben Sie die Ressourcengruppe an, in der sich die Azure-VMs nach der Migration befinden. Wählen Sie unter **Virtuelles Netzwerk** das Azure-VNET/-Subnetz aus, in das die Azure-VMs nach der Migration eingebunden werden.
7. Wählen Sie unter **Azure-Hybridvorteil**

    - die Option **Nein**, falls Sie den Azure-Hybridvorteil nicht anwenden möchten. Klicken Sie auf **Weiter**.
    - Wählen Sie **Ja**, wenn Sie über Windows Server-Computer verfügen, die durch aktive Software Assurance- oder Windows Server-Abonnements abgedeckt sind, und den Vorteil auf die zu migrierenden Computer anwenden möchten. Klicken Sie auf **Weiter**.

    ![Zieleinstellungen](./media/tutorial-migrate-vmware/target-settings.png)

8. Überprüfen Sie unter **Compute** den VM-Namen, die Größe, den Typ des Betriebssystemdatenträgers und die Verfügbarkeitsgruppe. Für die VMs müssen die [Azure-Anforderungen](migrate-support-matrix-vmware.md#agentless-migration-vmware-vm-requirements) erfüllt werden.

    - **VM-Größe**: Wenn Sie Bewertungsempfehlungen verwenden, enthält die Dropdownliste der VM die empfohlene Größe. Andernfalls wählt Azure Migrate eine Größe basierend auf der höchsten Übereinstimmung im Azure-Abonnement aus. Alternativ können Sie unter **Azure-VM-Größe** manuell eine Größe auswählen. 
    - **Betriebssystemdatenträger**: Geben Sie den Betriebssystemdatenträger (Datenträger für Bootvorgang) für die VM an. Der Betriebssystemdatenträger enthält den Bootloader und das Installationsprogramm des Betriebssystems. 
    - **Verfügbarkeitsgruppe**: Geben Sie die Gruppe an, falls die VM nach der Migration in einer Azure-Verfügbarkeitsgruppe enthalten sein soll. Die Gruppe muss Teil der Zielressourcengruppe sein, die Sie für die Migration angeben.

    ![VM-Computeeinstellungen](./media/tutorial-migrate-vmware/compute-settings.png)

9. Geben Sie unter **Datenträger** an, ob die VM-Datenträger in Azure repliziert werden sollen, und wählen Sie in Azure den Datenträgertyp aus (SSD Standard/HDD Standard oder Managed Disks Premium). Klicken Sie auf **Weiter**.
    - Sie können Datenträger von der Replikation ausschließen.
    - Wenn Sie Datenträger ausschließen, sind diese nach der Migration nicht auf dem virtuellen Azure-Computer vorhanden. 

    ![Datenträger](./media/tutorial-migrate-vmware/disks.png)

10. Überprüfen Sie unter **Replikation prüfen und starten** die Einstellungen, und klicken Sie auf **Replizieren**, um die erste Replikation für die Server zu starten.

> [!NOTE]
> Sie können die Replikationseinstellungen vor Beginn der Replikation jederzeit unter **Verwalten** > **Aktuell replizierte Computer** aktualisieren. Die Einstellungen können nach dem Beginn der Replikation nicht mehr geändert werden.

### <a name="provisioning-for-the-first-time"></a>Erste Bereitstellung

Wenn dies die erste VM ist, die Sie im Rahmen des Azure Migrate-Projekts replizieren, werden diese Ressourcen von der Azure Migrate-Servermigration automatisch in derselben Ressourcengruppe wie das Projekt bereitgestellt.

- **Service Bus**: Für die Azure Migrate-Servermigration wird Service Bus verwendet, um Nachrichten zur Replikationsorchestrierung an die Appliance zu senden.
- **Gatewayspeicherkonto**: Bei der Servermigration wird das Gatewayspeicherkonto verwendet, um Statusinformationen zu den replizierten VMs zu speichern.
- **Protokollspeicherkonto**: Die Azure Migrate-Appliance lädt Replikationsprotokolle für VMs in ein Protokollspeicherkonto hoch. Azure Migrate wendet die Replikationsinformationen auf die verwalteten Replikatdatenträger an.
- **Schlüsseltresor**: Von der Azure Migrate-Appliance wird der Schlüsseltresor verwendet, um Verbindungszeichenfolgen für den Service Bus zu verwalten, und Zugriffsschlüssel werden für die Speicherkonten genutzt, die bei der Replikation eingesetzt werden. Sie sollten die Berechtigungen, die vom Schlüsseltresor für den Zugriff auf das Speicherkonto benötigt werden, während der Vorbereitung eingerichtet haben. [Sehen Sie sich diese Berechtigungen an](tutorial-prepare-vmware.md#assign-role-assignment-permissions).   


## <a name="track-and-monitor"></a>Nachverfolgen und Überwachen


- Wenn Sie auf **Replizieren** klicken, beginnt die Durchführung des Auftrags „Replikation starten“. 
- Nachdem der Auftrag „Replikation starten“ erfolgreich abgeschlossen wurde, beginnen die Computer mit der ersten Replikation in Azure.
- Während der ersten Replikation wird eine VM-Momentaufnahme erstellt. Die Datenträgerdaten der Momentaufnahme werden auf verwalteten Replikatdatenträgern in Azure repliziert.
- Nachdem die erste Replikation abgeschlossen ist, beginnt die Deltareplikation. Inkrementelle Änderungen an lokalen Datenträgern werden regelmäßig auf den Replikatdatenträgern in Azure repliziert.

Sie haben die Möglichkeit, den Auftragsstatus über die Portalbenachrichtigungen nachzuverfolgen.

Sie können den Replikationsstatus überwachen, indem Sie auf **Server werden repliziert** klicken (unter **Azure Migrate: Servermigration**).
![Überwachen der Replikation](./media/tutorial-migrate-vmware/replicating-servers.png)




## <a name="run-a-test-migration"></a>Ausführen einer Testmigration


Wenn die Deltareplikation beginnt, können Sie zunächst eine Testmigration für die VMs durchführen, bevor eine vollständige Migration zu Azure erfolgt. Wir empfehlen Ihnen dringend, dies vor dem Migrieren für jeden Computer mindestens einmal durchzuführen.

- Bei einer Testmigration wird überprüft, ob die Migration wie erwartet funktioniert, ohne die lokalen Computer zu beeinträchtigen, und ob diese während der Replikation betriebsbereit bleiben. 
- Mit der Testmigration wird die Migration simuliert, indem eine Azure-VM mit replizierten Daten erstellt wird (normalerweise per Migration zu einem nicht für die Produktion bestimmten VNET unter Ihrem Azure-Abonnement).
- Sie können den replizierten virtuellen Azure-Testcomputer verwenden, um die Migration zu überprüfen, App-Tests durchzuführen und Probleme zu beheben, bevor die vollständige Migration erfolgt.

Führen Sie die Testmigration wie folgt durch:


1. Klicken Sie unter **Migrationsziele** > **Server** > **Azure Migrate: Servermigration** auf **Migrierte Server testen**.

     ![Migrierte Server testen](./media/tutorial-migrate-vmware/test-migrated-servers.png)

2. Klicken Sie mit der rechten Maustaste auf den zu testenden virtuellen Computer, und klicken Sie dann auf **Testmigration**.

    ![Testmigration](./media/tutorial-migrate-vmware/test-migrate.png)

3. Wählen Sie unter **Testmigration** das Azure VNET aus, in dem sich die Azure-VM nach der Migration befinden soll. Wir empfehlen Ihnen, ein nicht für die Produktion bestimmtes VNET zu verwenden.
4. Der Auftrag **Testmigration** wird gestartet. Überwachen Sie den Auftrag in den Portalbenachrichtigungen.
5. Zeigen Sie die migrierte Azure-VM nach Abschluss der Migration im Azure-Portal unter **Virtuelle Computer** an. Der Computername enthält das Suffix **-Test**.
6. Klicken Sie nach Abschluss des Tests mit der rechten Maustaste unter **Aktuell replizierte Computer** auf die Azure-VM, und klicken Sie anschließend auf **Testmigration bereinigen**.

    ![Testmigration bereinigen](./media/tutorial-migrate-vmware/clean-up.png)


## <a name="migrate-vms"></a>Migrieren virtueller Computer

Nachdem Sie sich vergewissert haben, dass die Testmigration wie erwartet funktioniert, können Sie die lokalen Computer migrieren.

1. Klicken Sie im Azure Migrate-Projekt unter **Server** > **Azure Migrate: Servermigration** auf **Server werden repliziert**.

    ![Server werden repliziert](./media/tutorial-migrate-vmware/replicate-servers.png)

2. Klicken Sie unter **Aktuell replizierte Computer** mit der rechten Maustaste auf die VM und dann auf **Migrieren**.
3. Wählen Sie unter **Migrieren** > **Virtuelle Computer herunterfahren und eine geplante Migration ohne Datenverlust durchführen?** die Option **Ja** > **OK**.
    - Azure Migrate fährt die lokale VM standardmäßig herunter und führt eine bedarfsabhängige Replikation durch, um alle VM-Änderungen zu synchronisieren, die seit der letzten Replikation vorgenommen wurden. So wird sichergestellt, dass es nicht zu Datenverlust kommt.
    - Wählen Sie **Nein**, wenn Sie die VM nicht herunterfahren möchten.
4. Ein Migrationsauftrag wird für den virtuellen Computer gestartet. Verfolgen Sie den Auftrag unter den Azure-Benachrichtigungen nach.
5. Nach Abschluss des Auftrags können Sie die VM auf der Seite **Virtuelle Computer** anzeigen und verwalten.

## <a name="complete-the-migration"></a>Fertigstellen der Migration

1. Klicken Sie nach Abschluss der Migration mit der rechten Maustaste auf die VM und dann auf **Migration beenden**. Die Replikation für den lokalen Computer wird beendet, und die Informationen zum Replikationsstatus der VM werden bereinigt.
2. Installieren Sie den [Windows](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-windows)- oder [Linux](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux)-Agent für die Azure-VM auf den migrierten Computern.
3. Führen Sie App-Anpassungen nach der Migration durch, z.B. die Aktualisierung von Datenbankverbindungszeichenfolgen und Webserverkonfigurationen.
4. Führen Sie endgültige Anwendungs- und Migrationsakzeptanztests für die migrierte Anwendung durch, die nun in Azure ausgeführt wird.
5. Leiten Sie den Datenverkehr auf die migrierte Instanz der Azure-VM um.
6. Entfernen Sie die lokalen VMs aus Ihrem lokalen VM-Bestand.
7. Entfernen Sie die lokalen VMs aus lokalen Sicherungen.
8. Aktualisieren Sie die interne Dokumentation zum Anzeigen des neuen Speicherorts und der IP-Adresse der Azure-VMs. 

## <a name="post-migration-best-practices"></a>Bewährte Methoden nach der Migration

- Beachten Sie zur Steigerung der Resilienz Folgendes:
    - Schützen Sie Daten, indem Sie Azure-VMs mit dem Azure Backup-Dienst sichern. [Weitere Informationen](../backup/quick-backup-vm-portal.md)
    - Sorgen Sie für die kontinuierliche Ausführung und Verfügbarkeit von Workloads, indem Sie Azure-VMs mithilfe von Site Recovery in eine sekundäre Region replizieren. [Weitere Informationen](../site-recovery/azure-to-azure-tutorial-enable-replication.md)
- Beachten Sie zur Steigerung der Sicherheit Folgendes:
    - Sperren und beschränken Sie den Zugriff von eingehendem Datenverkehr mit der [Just-in-Time-Verwaltung im Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-just-in-time).
    - Beschränken Sie den Netzwerkdatenverkehr mithilfe von [Netzwerksicherheitsgruppen](https://docs.microsoft.com/azure/virtual-network/security-overview) auf Verwaltungsendpunkte.
    - Stellen Sie [Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption-overview) bereit, um Datenträger und Daten vor Diebstahl und unbefugtem Zugriff zu schützen.
    - Erfahren Sie mehr über das [Sichern von IaaS-Ressourcen](https://azure.microsoft.com/services/virtual-machines/secure-well-managed-iaas/), und besuchen Sie die Website [Azure Security Center](https://azure.microsoft.com/services/security-center/).
- Beachten Sie zur Überwachung und Verwaltung Folgendes:
-  Ziehen Sie die Bereitstellung von [Azure Cost Management](https://docs.microsoft.com/azure/cost-management/overview) in Erwägung, um den Ressourceneinsatz und die Ausgaben zu überwachen.


## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich den Artikel zur [Cloudmigration](https://docs.microsoft.com/azure/architecture/cloud-adoption/getting-started/migrate) des Frameworks für die Cloudeinführung (Cloud Adoption Framework) an.
