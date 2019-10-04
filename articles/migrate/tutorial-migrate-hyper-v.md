---
title: Migrieren von lokalen virtuellen Hyper-V-Computern zu Azure per Azure Migrate-Servermigration | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie lokale Hyper-V-VMs per Azure Migrate-Servermigration zu Azure migrieren.
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 09/04/2019
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 755bb6d019418cf9dae22ebf7ee6a3c94af3c750
ms.sourcegitcommit: f176e5bb926476ec8f9e2a2829bda48d510fbed7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70309446"
---
# <a name="migrate-hyper-v-vms-to-azure"></a>Migrieren von virtuellen Hyper-V-Computern zu Azure 

In diesem Artikel wird veranschaulicht, wie Sie lokale Hyper-V-VMs zu Azure migrieren, indem Sie die Migration ohne Agent mit dem Tool für die Azure Migrate-Servermigration durchführen.

[Azure Migrate](migrate-services-overview.md) ist ein zentraler Hub zum Nachverfolgen der Ermittlung, Bewertung und Migration Ihrer lokalen Apps und Workloads sowie von VMs in der privaten/öffentlichen Cloud zu Azure. Der Hub stellt Azure Migrate-Tools für die Bewertung und Migration sowie unabhängige Drittanbietertools bereit.

Dieses Tutorial ist das dritte in einer Reihe, mit der veranschaulicht wird, wie Sie Hyper-V-VMs bewerten und zu Azure migrieren, indem Sie die Azure Migrate-Serverbewertung und -migration nutzen. In diesem Tutorial lernen Sie Folgendes:


> [!div class="checklist"]
> * Vorbereiten von Azure und Ihrer lokalen Hyper-V-Umgebung
> * Einrichten der Quellumgebung und Bereitstellen einer Replikationsappliance
> * Einrichten der Zielumgebung
> * Aktivieren Sie die Replikation.
> * Ausführen einer Testmigration, um sicherzustellen, dass alles wie erwartet funktioniert
> * Durchführen einer vollständigen Migration zu Azure

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) erstellen, bevor Sie beginnen.


## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie mit diesem Tutorial beginnen, sollten folgende Voraussetzungen erfüllt sein:

1. [Lesen Sie den Artikel zur Hyper-V-Migrationsarchitektur](migrate-architecture.md).
2. Arbeiten Sie das [erste Tutorial dieser Reihe](tutorial-prepare-hyper-v.md) durch, um Azure und Hyper-V für die Migration einzurichten. Im ersten Tutorial führen Sie Folgendes durch:
    - [Vorbereiten von Azure](tutorial-prepare-hyper-v.md#prepare-azure) für die Migration
    - [Vorbereiten der lokalen Umgebung](tutorial-prepare-hyper-v.md#prepare-for-hyper-v-migration) für die Migration
3. Wir empfehlen Ihnen, Hyper-V-VMs nach Möglichkeit mit der Azure Migrate-Serverbewertung zu bewerten, bevor Sie diese zu Azure migrieren. Arbeiten Sie hierzu das [zweite Tutorial](tutorial-assess-hyper-v.md) dieser Reihe durch. Wir empfehlen Ihnen zwar, die Bewertung auszuprobieren, aber dies ist nicht unbedingt erforderlich, bevor Sie VMs migrieren.
4. Vergewissern Sie sich, dass Ihrem Azure-Konto die Rolle „Mitwirkender für virtuelle Computer“ zugewiesen ist, damit Sie über folgende Berechtigungen verfügen:

    - Erstellen einer VM in der ausgewählten Ressourcengruppe
    - Erstellen einer VM im ausgewählten virtuellen Netzwerk
    - Schreiben auf einen verwalteten Azure-Datenträger   
5. [Richten Sie ein Azure-Netzwerk ein](../virtual-network/manage-virtual-network.md#create-a-virtual-network). Wenn Sie die Migration zu Azure durchführen, werden die erstellten Azure-VMs in ein Azure-Netzwerk eingebunden, das Sie beim Einrichten der Migration angeben.


## <a name="add-the-azure-migrate-server-migration-tool"></a>Hinzufügen des Tools für die Azure Migrate-Servermigration

Falls Sie das zweite Tutorial zur Bewertung von Hyper-V-VMs nicht durchgearbeitet haben, müssen Sie [diese Anleitung befolgen](how-to-add-tool-first-time.md), um ein Azure Migrate-Projekt einzurichten, und dem Projekt dann das Tool für die Azure Migrate-Servermigration hinzufügen.

Wenn Sie das zweite Tutorial durchgearbeitet und bereits ein Azure Migrate-Projekt eingerichtet haben, können Sie das Tool für die Azure Migrate-Servermigration wie folgt hinzufügen:

1. Klicken Sie im Azure Migrate-Projekt auf **Übersicht**. 
2. Klicken Sie unter **Server ermitteln, bewerten und migrieren** auf **Server bewerten und migrieren**.
3. Wählen Sie unter **Migrationstools** die Option **Click here to add a migration tool when you are ready to migrate** (Klicken Sie hier, um ein Migrationstool hinzuzufügen, wenn Sie bereit für die Migration sind).

    ![Auswählen eines Tools](./media/tutorial-migrate-hyper-v/select-migration-tool.png)

4. Wählen Sie in der Liste mit den Tools die Option **Azure Migrate: Servermigration** > **Tool hinzufügen**.

    ![Tool für die Servermigration](./media/tutorial-migrate-hyper-v/server-migration-tool.png)


## <a name="set-up-the-azure-migrate-appliance"></a>Einrichten der Azure Migrate-Appliance

Für die Azure Migrate-Servermigration wird eine einfache Hyper-V-VM-Appliance ausgeführt.

- Die Appliance ermittelt virtuelle Computer und sendet Meta- und Leistungsdaten zu virtuellen Computern an die Azure Migrate-Servermigration.
- Die gleiche Appliance wird auch vom Tool für die Azure Migrate-Serverbewertung verwendet.

Richten Sie die Appliance wie folgt ein:
- Wenn Sie das zweite Tutorial zum Bewerten von Hyper-V-VMs durchgearbeitet haben, haben Sie die Appliance bereits während dieses Tutorials eingerichtet.
- Wenn nicht, müssen Sie die Appliance jetzt einrichten. Führen Sie dazu die folgenden Schritte aus: 

    - Laden Sie über das Azure-Portal eine komprimierte Hyper-V-VHD herunter.
    - Erstellen der Appliance und Überprüfen der Verbindungsherstellung mit der Azure Migrate-Serverbewertung 
    - Durchführen der Erstkonfiguration für die Appliance und Registrieren der Appliance beim Azure Migrate-Projekt

    Befolgen Sie die Anleitung zum Einrichten der Appliance in [diesem Artikel](how-to-set-up-appliance-hyper-v.md).

## <a name="prepare-hyper-v-hosts"></a>Vorbereiten der Hyper-V-Hosts

1. Klicken Sie im Azure Migrate-Projekt unter **Server** > **Azure Migrate: Servermigration** auf **Ermitteln**.
2. Wählen Sie unter **Computer ermitteln** > **Sind Ihre Computer virtualisiert?** die Option **Ja, mit Hyper-V**.
3. Wählen Sie unter **Zielregion** die Azure-Region aus, zu der Sie die Computer migrieren möchten.
6. Aktivieren Sie das Kontrollkästchen **Bestätigen Sie, dass die Zielregion für die Migration „<Name der Region>“ lautet**.
7. Klicken Sie auf **Ressourcen erstellen**. Daraufhin wird im Hintergrund ein Azure Site Recovery-Tresor erstellt.
    - Falls Sie die Migration bereits mit Azure Migrate-Servermigration eingerichtet haben, wird diese Option nicht angezeigt, da die Ressourcen bereits vorher eingerichtet wurden.
    - Nach dem Klicken auf diese Schaltfläche kann die Zielregion für dieses Projekt nicht mehr geändert werden.
    - Alle anschließenden Migrationen erfolgen zu dieser Region.
    
8. Laden Sie unter **Bereiten Sie die Hyper-V-Hostserver vor** den Hyper-V-Replikationsanbieter und die Datei mit dem Registrierungsschlüssel herunter.
    - Der Registrierungsschlüssel ist erforderlich, um den Hyper-V-Host bei „Azure Migrate-Servermigration“ zu registrieren.
    - Der Schlüssel ist nach der Erstellung fünf Tage lang gültig.

    ![Anbieter und Schlüssel herunterladen](./media/tutorial-migrate-hyper-v/download-provider-hyper-v.png)

4. Kopieren Sie die Setupdatei für den Anbieter und die Datei mit dem Registrierungsschlüssel auf alle Hyper-V-Hosts (oder Clusterknoten), auf denen zu replizierende VMs ausgeführt werden.
5. Führen Sie die Setupdatei für den Anbieter auf jedem Host aus, wie dies im nächsten Schritt beschrieben ist.
6. Nachdem der Anbieter auf den Hosts installiert wurde, klicken Sie unter **Computer ermitteln** auf **Finalize registration** (Registrierung abschließen).

    ![Registrierung abschließen](./media/tutorial-migrate-hyper-v/finalize-registration.png)

Es kann nach dem Abschluss der Registrierung bis zu 15 Minuten dauern, bis ermittelte VMs in „Azure Migrate-Servermigration“ angezeigt werden. Wenn VMs ermittelt werden, erhöht sich die Anzahl unter **Ermittelte Server**.

![Ermittelte Server](./media/tutorial-migrate-hyper-v/discovered-servers.png)

### <a name="register-hyper-v-hosts"></a>Registrieren von Hyper-V-Hosts

Installieren Sie die heruntergeladene Setupdatei (AzureSiteRecoveryProvider.exe) auf allen relevanten Hyper-V-Hosts.

1. Führen Sie die Setupdatei für den Anbieter auf jedem Host- bzw. Clusterknoten aus.
2. Wählen Sie im Setup-Assistenten für den Anbieter die Option **Microsoft Update**, um über Microsoft Update nach Anbieterupdates zu suchen.
3. Akzeptieren Sie unter **Installation** den standardmäßigen Installationsspeicherort für den Anbieter und den Agent, und wählen Sie **Installieren** aus.
4. Wählen Sie nach der Installation im Registrierungs-Assistenten unter **Tresoreinstellungen** die Option **Durchsuchen** und unter **Schlüsseldatei** dann die Tresorschlüsseldatei aus, die Sie heruntergeladen haben.
5. Geben Sie unter **Proxyeinstellungen** an, wie für den Anbieter, der auf dem Host ausgeführt wird, eine Internetverbindung hergestellt wird.
    - Falls sich die Appliance hinter einem Proxyserver befindet, müssen Proxyeinstellungen angegeben werden.
    - Geben Sie den Proxynamen im Format **http://ip-address** oder **http://FQDN** an. HTTPS-Proxyserver werden nicht unterstützt.
   

6. Stellen Sie sicher, dass die [erforderlichen URLs](migrate-support-matrix-hyper-v.md#migration-hyper-v-host-url-access) für den Anbieter erreichbar sind.
7. Klicken Sie nach dem Registrieren des Hosts unter **Registrierung** auf **Fertig stellen**.

## <a name="replicate-hyper-v-vms"></a>Replizieren von Hyper-V-VMs

Nachdem die Ermittlung abgeschlossen ist, können Sie mit der Replikation von Hyper-V-VMs in Azure beginnen.

> [!NOTE]
> Sie können bis zu zehn Computer gleichzeitig replizieren. Müssen Sie mehr Computer replizieren, führen Sie die Replikation in Batches mit jeweils zehn Computern durch.

1. Klicken Sie im Azure Migrate-Projekt unter **Server** > **Azure Migrate: Servermigration** auf **Replizieren**.
2. Wählen Sie unter **Replizieren** > **Quelleinstellungen** > **Sind Ihre Computer virtualisiert?** die Option **Ja, mit Hyper-V**. Klicken Sie anschließend auf **Next: Virtuelle Computer**.
3. Wählen Sie unter **Virtuelle Computer** die Computer aus, die Sie replizieren möchten.
    - Wenn Sie eine Bewertung für die virtuellen Computer durchgeführt haben, können Sie die Empfehlungen zur VM-Größenanpassung und zum Datenträgertyp (Premium/Standard) aus den Bewertungsergebnissen anwenden. Wählen Sie hierzu unter **Migrationseinstellungen aus einer Azure Migrate-Bewertung importieren?** die Option **Ja** aus.
    - Wählen Sie **Nein** aus, wenn Sie keine Bewertung ausgeführt haben oder die Bewertungseinstellungen nicht verwenden möchten.
    - Wählen Sie die VM-Gruppe und den Bewertungsnamen aus, wenn Sie die Bewertung gewählt haben.

        ![Auswählen der Bewertung](./media/tutorial-migrate-hyper-v/select-assessment.png)

4. Suchen Sie unter **Virtuelle Computer** je nach Bedarf nach VMs, und aktivieren Sie alle VMs, die Sie migrieren möchten. Klicken Sie anschließend auf **Weiter: Zieleinstellungen**.

    ![Auswählen von VMs](./media/tutorial-migrate-hyper-v/select-vms.png)

5. Wählen Sie unter **Zieleinstellungen** die Zielregion für die Migration, das Abonnement und die Ressourcengruppe aus, in der sich die Azure-VMs nach der Migration befinden.
7. Wählen Sie unter **Replikationsspeicherkonto** das Azure-Speicherkonto aus, in dem replizierte Daten in Azure gespeichert werden.
8. Wählen Sie unter **Virtuelles Netzwerk** das Azure-VNET/-Subnetz aus, in das die Azure-VMs nach der Migration eingebunden werden.
9. Wählen Sie unter **Azure-Hybridvorteil**

    - die Option **Nein**, falls Sie den Azure-Hybridvorteil nicht anwenden möchten. Klicken Sie auf **Weiter**.
    - Wählen Sie **Ja**, wenn Sie über Windows Server-Computer verfügen, die durch aktive Software Assurance- oder Windows Server-Abonnements abgedeckt sind, und den Vorteil auf die zu migrierenden Computer anwenden möchten. Klicken Sie auf **Weiter**.

    ![Zieleinstellungen](./media/tutorial-migrate-hyper-v/target-settings.png)

10. Überprüfen Sie unter **Compute** den VM-Namen, die Größe, den Typ des Betriebssystemdatenträgers und die Verfügbarkeitsgruppe. Die VMs müssen die [Azure-Anforderungen](migrate-support-matrix-vmware.md#agentless-migration-vmware-vm-requirements) erfüllen.

    - **VM-Größe**: Bei Verwendung von Bewertungsempfehlungen enthält die Dropdownliste für die VM-Größe die empfohlene Größe. Andernfalls wählt Azure Migrate eine Größe basierend auf der höchsten Übereinstimmung im Azure-Abonnement aus. Alternativ können Sie unter **Azure-VM-Größe** manuell eine Größe auswählen. 
    - **Betriebssystemdatenträger**: Geben Sie den Betriebssystemdatenträger (Startdatenträger) für die VM an. Der Betriebssystemdatenträger enthält den Bootloader und das Installationsprogramm des Betriebssystems. 
    - **Verfügbarkeitsgruppe**: Wenn die VM nach der Migration in einer Azure-Verfügbarkeitsgruppe enthalten sein soll, geben Sie die Gruppe an. Die Gruppe muss Teil der Zielressourcengruppe sein, die Sie für die Migration angeben.

    ![VM-Computeeinstellungen](./media/tutorial-migrate-hyper-v/compute-settings.png)

11. Geben Sie unter **Datenträger** an, ob die VM-Datenträger in Azure repliziert werden sollen, und wählen Sie in Azure den Datenträgertyp aus (SSD Standard/HDD Standard oder Managed Disks Premium). Klicken Sie auf **Weiter**.
    - Sie können Datenträger von der Replikation ausschließen.
    - Wenn Sie Datenträger ausschließen, sind diese nach der Migration nicht auf dem virtuellen Azure-Computer vorhanden. 

    ![Datenträger](./media/tutorial-migrate-hyper-v/disks.png)

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
- Nachdem die erste Replikation abgeschlossen ist, beginnt die Deltareplikation. Inkrementelle Änderungen an lokalen Datenträgern werden regelmäßig in Azure repliziert.

Sie haben die Möglichkeit, den Auftragsstatus über die Portalbenachrichtigungen nachzuverfolgen.

Sie können den Replikationsstatus überwachen, indem Sie auf **Server werden repliziert** klicken (unter **Azure Migrate: Servermigration**).
![Überwachen der Replikation](./media/tutorial-migrate-hyper-v/replicating-servers.png)



## <a name="run-a-test-migration"></a>Ausführen einer Testmigration


Wenn die Deltareplikation beginnt, können Sie zunächst eine Testmigration für die VMs ausführen, bevor eine vollständige Migration zu Azure erfolgt. Dieser Schritt sollte vor der Migration mindestens einmal für jeden Computer ausgeführt werden.

- Bei einer Testmigration wird überprüft, ob die Migration wie erwartet funktioniert, ohne die lokalen Computer zu beeinträchtigen, und ob diese während der Replikation betriebsbereit bleiben. 
- Mit der Testmigration wird die Migration simuliert, indem eine Azure-VM mit replizierten Daten erstellt wird (normalerweise per Migration zu einem nicht für die Produktion bestimmten VNET unter Ihrem Azure-Abonnement).
- Sie können den replizierten virtuellen Azure-Testcomputer verwenden, um die Migration zu überprüfen, App-Tests durchzuführen und Probleme zu beheben, bevor die vollständige Migration erfolgt.

Führen Sie die Testmigration wie folgt durch:


1. Klicken Sie unter **Migrationsziele** > **Server** > **Azure Migrate: Servermigration** auf **Migrierte Server testen**.

     ![Testen der migrierten Server](./media/tutorial-migrate-hyper-v/test-migrated-servers.png)

2. Klicken Sie mit der rechten Maustaste auf die zu testende VM, und klicken Sie anschließend auf **Testmigration**.

    ![Testmigration](./media/tutorial-migrate-hyper-v/test-migrate.png)

3. Wählen Sie unter **Testmigration** das Azure VNET aus, in dem sich die Azure-VM nach der Migration befindet. Es empfiehlt sich, ein nicht für die Produktion bestimmtes VNET zu verwenden.
4. Der Auftrag **Testmigration** wird gestartet. Überwachen Sie den Auftrag anhand der Portalbenachrichtigungen.
5. Zeigen Sie die migrierte Azure-VM nach Abschluss der Migration im Azure-Portal unter **Virtuelle Computer** an. Der Computername enthält das Suffix **-Test**.
6. Klicken Sie nach Abschluss des Tests mit der rechten Maustaste unter **Aktuell replizierte Computer** auf die Azure-VM, und klicken Sie anschließend auf **Testmigration bereinigen**.

    ![Bereinigen der Migration](./media/tutorial-migrate-hyper-v/clean-up.png)


## <a name="migrate-vms"></a>Migrieren virtueller Computer

Nachdem Sie sich vergewissert haben, dass die Testmigration wie erwartet funktioniert, können Sie die lokalen Computer migrieren.

1. Klicken Sie im Azure Migrate-Projekt unter **Server** > **Azure Migrate: Servermigration** auf **Server werden repliziert**.

    ![Replizieren der Server](./media/tutorial-migrate-hyper-v/replicate-servers.png)

2. Klicken Sie unter **Aktuell replizierte Computer** mit der rechten Maustaste auf die VM und dann auf **Migrieren**.
3. Wählen Sie unter **Migrieren** > **Virtuelle Computer herunterfahren und eine geplante Migration ohne Datenverlust durchführen?** die Option **Ja** >  und anschließend **OK** aus.
    - Azure Migrate fährt die lokale VM standardmäßig herunter und führt eine bedarfsabhängige Replikation aus, um alle VM-Änderungen zu synchronisieren, die seit der letzten Replikation vorgenommen wurden. So wird sichergestellt, dass keine Daten verloren gehen.
    - Falls Sie die VM nicht herunterfahren möchten, wählen Sie **Nein** aus.
4. Für den virtuellen Computer wird ein Migrationsauftrag gestartet. Verfolgen Sie den Auftrag anhand der Azure-Benachrichtigungen nach.
5. Nach Abschluss des Auftrags können Sie die VM auf der Seite **Virtuelle Computer** anzeigen und verwalten.

## <a name="complete-the-migration"></a>Fertigstellen der Migration

1. Klicken Sie nach Abschluss der Migration mit der rechten Maustaste auf die VM und dann auf **Migration beenden**. Daraufhin wird Replikation für den lokalen Computer beendet, und die Informationen zum Replikationsstatus der VM werden bereinigt.
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
