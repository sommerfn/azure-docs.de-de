---
title: Migrieren von lokalen physischen oder virtualisierten Computern zu Azure per Azure Migrate-Servermigration | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie lokale physische oder virtualisierte Computer per Azure Migrate-Servermigration zu Azure migrieren.
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 11/04/2019
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: ffa072f593a06a38e6e3d121fb76806e41bf568e
ms.sourcegitcommit: c62a68ed80289d0daada860b837c31625b0fa0f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2019
ms.locfileid: "73605552"
---
# <a name="migrate-physical-or-virtualized-servers-to-azure"></a>Migrieren von physischen oder virtualisierten Servern zu Azure 

In diesem Artikel wird beschrieben, wie Sie physische oder virtualisierte Server zu Azure migrieren. Das Tool für die Azure Migrate-Servermigration ermöglicht die Migration von physischen und virtualisierten Servern, indem eine Agent-basierte Replikation durchgeführt wird. Mit diesem Tool können Sie viele verschiedene Computer zu Azure migrieren:

- Migrieren von lokalen physischen Servern
- Migrieren von VMs, die über eine Plattform, z. B. Xen oder KVM, virtualisiert wurden
- Migrieren von Hyper-V- oder VMware-VMs. Dies ist hilfreich, falls Sie aus einem bestimmten Grund nicht den Standardmigrationsablauf nutzen können, der von der Azure Migrate-Servermigration für die [Hyper-V-Migration](tutorial-migrate-hyper-v.md), die [VMware-Migration ohne Agent](tutorial-migrate-vmware.md) oder die [Agent-basierte VMware-Migration](tutorial-migrate-vmware-agent.md) angeboten wird.
- Migrieren von VMs, die in privaten Clouds ausgeführt werden
- Migrieren von VMs, die in öffentlichen Clouds ausgeführt werden, z. B. Amazon Web Services (AWS) oder Google Cloud Platform (GCP)


[Azure Migrate](migrate-services-overview.md) ist ein zentraler Hub zum Nachverfolgen der Ermittlung, Bewertung und Migration Ihrer lokalen Apps und Workloads sowie von VM-Cloudinstanzen zu Azure. Der Hub stellt Azure Migrate-Tools für die Bewertung und Migration sowie unabhängige Drittanbietertools bereit.


In diesem Tutorial lernen Sie Folgendes:
> [!div class="checklist"]
> * Vorbereiten von Azure für die Migration mit dem Tool für die Azure Migrate-Servermigration
> * Überprüfen der Anforderungen für zu migrierende Computer und Vorbereiten eines Computers für die Azure Migrate-Replikationsappliance, die zum Ermitteln und Migrieren von Computern zu Azure verwendet wird
> * Hinzufügen des Tools für die Azure Migrate-Servermigration im Azure Migrate-Hub
> * Einrichten der Replikationsappliance
> * Installieren des Mobility Service auf Computern, die Sie migrieren möchten
> * Aktivieren Sie die Replikation.
> * Ausführen einer Testmigration, um sicherzustellen, dass alles wie erwartet funktioniert
> * Durchführen einer vollständigen Migration zu Azure

> [!NOTE]
> In den Tutorials wird der einfachste Bereitstellungspfad für ein Szenario erläutert, damit Sie schnell ein Proof of Concept einrichten können. In den Tutorials werden nach Möglichkeit Standardoptionen verwendet, und es werden nicht alle möglichen Einstellungen und Pfade angezeigt. Ausführliche Beschreibungen finden Sie in den Anleitungen zu Azure Migrate.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) erstellen, bevor Sie beginnen.


## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie mit diesem Tutorial beginnen, sollten folgende Voraussetzungen erfüllt sein:

1. [Überprüfen Sie die Migrationsarchitektur](migrate-architecture.md).
2. Vergewissern Sie sich, dass Ihrem Azure-Konto die Rolle „Mitwirkender für virtuelle Computer“ zugewiesen ist, damit Sie über folgende Berechtigungen verfügen:

    - Erstellen einer VM in der ausgewählten Ressourcengruppe
    - Erstellen einer VM im ausgewählten virtuellen Netzwerk
    - Schreiben auf einen verwalteten Azure-Datenträger 

3. [Richten Sie ein Azure-Netzwerk ein](../virtual-network/manage-virtual-network.md#create-a-virtual-network). Wenn Sie die Replikation in Azure durchführen, werden Azure-VMs erstellt und in ein Azure-Netzwerk eingebunden, das Sie beim Einrichten der Migration angeben.


## <a name="prepare-azure"></a>Vorbereiten von Azure

Sie müssen Azure-Berechtigungen einrichten, bevor Sie die Migration per Azure Migrate-Servermigration durchführen können.

- **Erstellen eines Projekts:** Ihr Azure-Konto benötigt Berechtigungen zum Erstellen eines Azure Migrate-Projekts. 
- **Registrieren der Azure Migrate-Replikationsappliance:** Die Replikationsappliance erstellt und registriert eine Azure Active Directory-App in Ihrem Azure-Konto. Delegieren Sie hierfür die Berechtigungen.
- **Erstellen eines Schlüsseltresors:** Für die Migration von Computern erstellt Azure Migrate einen Schlüsseltresor in der Ressourcengruppe, mit dem Zugriffsschlüssel für das Replikationsspeicherkonto in Ihrem Abonnement verwaltet werden können. Für die Tresorerstellung benötigen Sie Rollenzuweisungsberechtigungen für die Ressourcengruppe, in der sich das Azure Migrate-Projekt befindet. 


### <a name="assign-permissions-to-create-project"></a>Zuweisen von Berechtigungen für die Projekterstellung

1. Öffnen Sie im Azure-Portal das Abonnement, und wählen Sie **Zugriffssteuerung (IAM)** aus.
2. Suchen Sie unter **Zugriff überprüfen** das relevante Konto, und klicken Sie darauf, um Berechtigungen anzuzeigen.
3. Sie sollten über die Berechtigung **Mitwirkender** oder **Besitzer** verfügen.
    - Wenn Sie gerade erst ein kostenloses Azure-Konto erstellt haben, sind Sie der Besitzer Ihres Abonnements.
    - Wenn Sie nicht der Besitzer des Abonnements sind, müssen Sie mit dem Besitzer zusammenarbeiten, um die Rolle zuzuweisen.

### <a name="assign-permissions-to-register-the-replication-appliance"></a>Zuweisen von Berechtigungen zum Registrieren der Replikationsappliance

Delegieren Sie bei der Agent-basierten Migration Berechtigungen für die Azure Migrate-Servermigration, um die Erstellung und Registrierung einer Azure AD-App in Ihrem Konto zu ermöglichen. Berechtigungen können wie folgt zugewiesen werden:

- Ein Mandantenadministrator/globaler Administrator kann Benutzern unter dem Mandanten Berechtigungen zum Erstellen und Registrieren von Azure AD-Apps erteilen.
- Ein Mandantenadministrator/globaler Administrator kann dem Konto die Rolle „Anwendungsentwickler“ (die über die Berechtigungen verfügt) zuweisen.

Beachten Sie Folgendes:

- Die Apps verfügen nur über die oben beschriebenen Zugriffsberechtigungen für das Abonnement.
- Sie benötigen diese Berechtigungen nur, wenn Sie eine neue Replikationsappliance registrieren. Nach Einrichtung der Replikationsappliance können die Berechtigungen wieder entfernt werden. 


#### <a name="grant-account-permissions"></a>Erteilen von Kontoberechtigungen

Der Mandantenadministrator/globale Administrator kann Berechtigungen wie folgt erteilen:

1. In Azure AD muss der globale oder der Mandantenadministrator zu **Azure Active Directory** > **Benutzer** > **Benutzereinstellungen** navigieren.
2. Der Administrator muss **App-Registrierungen** auf **Ja** festlegen.

    ![Azure AD-Berechtigungen](./media/tutorial-migrate-physical-virtual-machines/aad.png)

> [!NOTE]
> Dies ist eine Standardeinstellung, die nicht vertraulich ist. [Weitere Informationen](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-applications-are-added#who-has-permission-to-add-applications-to-my-azure-ad-instance)

#### <a name="assign-application-developer-role"></a>Zuweisen der Rolle „Anwendungsentwickler“ 

Der Mandantenadministrator/globale Administrator kann einem Konto die Rolle „Anwendungsentwickler“ zuweisen. [Weitere Informationen](../active-directory/fundamentals/active-directory-users-assign-role-azure-portal.md)

## <a name="assign-permissions-to-create-key-vault"></a>Zuweisen von Berechtigungen für die Key Vault-Erstellung

Gehen Sie wie folgt vor, um Rollenzuweisungsberechtigungen für die Ressourcengruppe zuzuweisen, in der sich das Azure Migrate-Projekt befindet:

1. Wählen Sie im Azure-Portal unter der Ressourcengruppe die Option **Zugriffssteuerung (IAM)** aus.
2. Suchen Sie unter **Zugriff überprüfen** nach dem relevanten Konto, und klicken Sie darauf, um Berechtigungen anzuzeigen. Sie benötigen die Berechtigung **Besitzer** (oder die Berechtigungen **Mitwirkender** und **Benutzerzugriffsadministrator**).
3. Sollten Sie nicht über die erforderlichen Berechtigungen verfügen, müssen Sie sie beim Besitzer der Ressourcengruppe anfordern. 

## <a name="prepare-for-migration"></a>Vorbereiten der Migration

### <a name="check-machine-requirements-for-migration"></a>Überprüfen der Computeranforderungen für die Migration

Stellen Sie sicher, dass die Computer die Anforderungen für die Migration zu Azure erfüllen. 

> [!NOTE]
> Die Agent-basierte Migration mit der Azure Migrate-Servermigration basiert auf Features des Azure Site Recovery-Diensts. Einige Anforderungen sind daher ggf. mit der Site Recovery-Dokumentation verknüpft.

1. [Überprüfen](migrate-support-matrix-vmware.md#agent-based-migration-vmware-server-requirements) Sie die Anforderungen für VMware-Server.
2. [Überprüfen](migrate-support-matrix-vmware.md#agent-based-migration-vmware-vm-requirements) Sie die VM-Unterstützungsanforderungen für die Migration.
3. Überprüfen Sie die VM-Einstellungen. Lokale VMs, die Sie zu Azure replizieren möchten, müssen die [Azure-VM-Anforderungen](migrate-support-matrix-vmware.md#azure-vm-requirements) erfüllen.


### <a name="prepare-a-machine-for-the-replication-appliance"></a>Vorbereiten eines Computers für die Replikationsappliance

Bei der Azure Migrate-Servermigration wird eine Replikationsappliance verwendet, um Computer in Azure zu replizieren. Die Replikationsappliance führt die hier angegebenen Komponenten aus.

- **Konfigurationsserver**: Der Konfigurationsserver koordiniert die Kommunikation zwischen der lokalen Umgebung und Azure und verwaltet die Datenreplikation.
- **Prozessserver** Der Prozessserver fungiert als Replikationsgateway. Er empfängt Replikationsdaten, optimiert sie durch Zwischenspeicherung, Komprimierung und Verschlüsselung und sendet sie an ein Cachespeicherkonto in Azure. 

Bevor Sie beginnen, müssen Sie einen Windows Server 2016-Computer für das Hosten der Replikationsappliance vorbereiten. Der Computer sollte [diese Anforderungen](migrate-support-matrix-vmware.md#agent-based-migration-replication-appliance-requirements) erfüllen. Die Appliance sollte nicht auf einem Quellcomputer installiert werden, den Sie schützen möchten.


## <a name="add-the-azure-migrate-server-migration-tool"></a>Hinzufügen des Tools für die Azure Migrate-Servermigration

Richten Sie ein Azure Migrate-Projekt ein, und fügen Sie diesem dann das Tool für die Azure Migrate-Servermigration hinzu.

1. Wählen Sie im Azure-Portal die Option **Alle Dienste**, und suchen Sie nach **Azure Migrate**.
2. Wählen Sie unter **Dienste** die Option **Azure Migrate**.
3. Klicken Sie in der **Übersicht** auf **Server bewerten und migrieren**.
4. Klicken Sie unter **Server ermitteln, bewerten und migrieren** auf **Server bewerten und migrieren**.

    ![Ermitteln und Bewerten von Servern](./media/tutorial-migrate-physical-virtual-machines/assess-migrate.png)

5. Klicken Sie unter **Server ermitteln, bewerten und migrieren** auf **Tools hinzufügen**.
6. Wählen Sie unter **Projekt migrieren** Ihr Azure-Abonnement aus, und erstellen Sie bei Bedarf eine Ressourcengruppe.
7. Geben Sie unter **Projektdetails** den Projektnamen und die geografische Region an, in der Sie das Projekt erstellen möchten. Klicken Sie anschließend auf **Weiter**.

    ![Erstellen eines Azure Migrate-Projekts](./media/tutorial-migrate-physical-virtual-machines/migrate-project.png)

    Sie können Azure Migrate-Projekte in den unten angegebenen geografischen Regionen erstellen.

    **Geografie** | **Region**
    --- | ---
    Asien | Asien, Südosten
    Europa | „Europa, Norden“ oder „Europa, Westen“
    USA | „USA, Osten“ oder „USA, Westen-Mitte“

    Die für das Projekt angegebene Region wird nur zum Speichern der Metadaten verwendet, die von den lokalen VMs erfasst werden. Bei der tatsächlichen Migration kann eine beliebige Zielregion ausgewählt werden.
8. Wählen Sie unter **Bewertungstool auswählen** die Option **Hinzufügen eines Bewertungstools vorerst überspringen** und anschließend **Weiter** aus.
9. Wählen Sie unter **Migrationstool auswählen** Folgendes aus: **Azure Migrate: Servermigration** > **Weiter**.
10. Überprüfen Sie die Einstellungen unter **Überprüfen + Tools hinzufügen**, und klicken Sie auf **Tools hinzufügen**.
11. Nachdem Sie das Tool hinzugefügt haben, wird es im Azure Migrate-Projekt unter **Server** > **Migrationstools** angezeigt.

## <a name="set-up-the-replication-appliance"></a>Einrichten der Replikationsappliance

Der erste Schritt bei der Migration besteht darin, die Replikationsappliance einzurichten. Sie laden die Installationsprogrammdatei für die Appliance herunter und führen sie auf dem [vorbereiteten Computer](#prepare-a-machine-for-the-replication-appliance) aus. Nachdem Sie die Appliance installiert haben, registrieren Sie sie bei Azure Migrate-Servermigration.


### <a name="download-the-replication-appliance-installer"></a>Herunterladen des Installationsprogramms für die Replikationsappliance

1. Klicken Sie im Azure Migrate-Projekt unter **Server** > **Azure Migrate: Servermigration** auf **Ermitteln**.

    ![VMs ermitteln](./media/tutorial-migrate-physical-virtual-machines/migrate-discover.png)

3. Klicken Sie unter **Computer ermitteln** > **Sind Ihre Computer virtualisiert?** auf **Nicht virtualisiert/Andere**.
4. Wählen Sie unter **Zielregion** die Azure-Region aus, zu der Sie die Computer migrieren möchten.
5. Aktivieren Sie das Kontrollkästchen **Bestätigen Sie, dass die Zielregion für die Migration „<Name der Region>“ lautet**.
6. Klicken Sie auf **Ressourcen erstellen**. Daraufhin wird im Hintergrund ein Azure Site Recovery-Tresor erstellt.
    - Falls Sie die Migration bereits mit dem Tool für die Azure Migrate-Servermigration eingerichtet haben, kann die Zieloption nicht konfiguriert werden, da die Ressourcen bereits vorher eingerichtet wurden.
    - Nach dem Klicken auf diese Schaltfläche kann die Zielregion für dieses Projekt nicht mehr geändert werden.
    - Alle anschließenden Migrationen erfolgen zu dieser Region.

7. Wählen Sie unter **Möchten Sie eine neue Replikationsappliance installieren oder ein vorhandenes Setup horizontal hochskalieren?** die Option **Replikationsappliance installieren**.
9. Führen Sie unter **Laden Sie die Software für die Replikationsappliance herunter, und installieren Sie sie** den Download des Installationsprogramms für die Appliance und des Registrierungsschlüssels durch. Sie benötigen den Schlüssel zum Registrieren der Appliance. Der Schlüssel ist nach dem Herunterladen fünf Tage lang gültig.

    ![Anbieter herunterladen](media/tutorial-migrate-physical-virtual-machines/download-provider.png)

10. Kopieren Sie die Setupdatei für die Appliance und die Schlüsseldatei auf den Windows Server 2016-Computer, den Sie für die Appliance erstellt haben.
11. Führen Sie die Setupdatei für die Replikationsappliance aus, wie dies im nächsten Schritt beschrieben ist.
12. Nachdem die Appliance nach Abschluss des Setupvorgangs neu gestartet wurde, wählen Sie im Bereich **Computer ermitteln** die neue Appliance unter **Konfigurationsserver auswählen** aus. Klicken Sie anschließend auf **Finalize registration** (Registrierung abschließen). Beim Abschließen der Registrierung werden einige letzte Schritte ausgeführt, um die Replikationsappliance vorzubereiten.

    ![Registrierung abschließen](./media/tutorial-migrate-physical-virtual-machines/finalize-registration.png)

Es kann nach dem Abschluss der Registrierung bis zu 15 Minuten dauern, bis ermittelte Computer in „Azure Migrate-Servermigration“ angezeigt werden. Wenn VMs ermittelt werden, erhöht sich die Anzahl unter **Ermittelte Server**.

![Ermittelte Server](./media/tutorial-migrate-physical-virtual-machines/discovered-servers.png)


## <a name="install-the-mobility-service"></a>Installieren des Mobilitätsdiensts

Auf Computern, die Sie migrieren möchten, müssen Sie den Mobility Service-Agent installieren. Die Installationsprogramme für den Agent sind auf der Replikationsappliance verfügbar. Sie können nach dem richtigen Installationsprogramm suchen und den Agent auf jedem Computer installieren, den Sie migrieren möchten. Gehen Sie hierzu wie folgt vor:

1. Melden Sie sich an der Replikationsappliance an.
2. Navigieren Sie zu **%ProgramData%\ASR\home\svsystems\pushinstallsvc\repository**.
3. Suchen Sie nach dem richtigen Installationsprogramm für das Betriebssystem und die Version des Computers. Lesen Sie den Artikel zu den [unterstützten Betriebssystemen](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#replicated-machines). 
4. Kopieren Sie die Installationsdatei auf den Computer, den Sie migrieren möchten.
5. Stellen Sie sicher, dass Sie über die Passphrase verfügen, die bei der Bereitstellung des Geräts generiert wurde.
    - Speichern Sie die Datei in einer temporären Textdatei auf dem Computer.
    - Sie können die Passphrase über die Replikationsappliance abrufen. Führen Sie in der Befehlszeile **C:\ProgramData\ASR\home\svsystems\bin\genpassphrase.exe -v** aus, um die aktuelle Passphrase anzuzeigen.
    - Generieren Sie die Passphrase nicht neu. Hierbei wird die Verbindung getrennt, und Sie müssen die Replikationsappliance erneut registrieren.


### <a name="install-on-windows"></a>Installieren unter Windows

1. Extrahieren Sie den Inhalt der Installationsprogrammdatei wie folgt in einen lokalen Ordner des Computers (z. B. „C:\Temp“):

    ```
    ren Microsoft-ASR_UA*Windows*release.exe MobilityServiceInstaller.exe
    MobilityServiceInstaller.exe /q /x:C:\Temp\Extracted
    cd C:\Temp\Extracted
    ```
2. Führen Sie das Mobility Service-Installationsprogramm aus:
    ```
   UnifiedAgent.exe /Role "MS" /Silent
    ```
3. Registrieren Sie den Agent bei der Replikationsappliance:
    ```
    cd C:\Program Files (x86)\Microsoft Azure Site Recovery\agent
    UnifiedAgentConfigurator.exe  /CSEndPoint <replication appliance IP address> /PassphraseFilePath <Passphrase File Path>
    ```

### <a name="install-on-linux"></a>Installieren unter Linux

1. Extrahieren Sie den Inhalt des Tarball-Installationsprogramms wie folgt in einen lokalen Ordner des Computers (z. B. „/tmp/MobSvcInstaller“):
    ```
    mkdir /tmp/MobSvcInstaller
    tar -C /tmp/MobSvcInstaller -xvf <Installer tarball>
    cd /tmp/MobSvcInstaller
    ```
2. Führen Sie das Installationsskript aus:
    ```
    sudo ./install -r MS -q
    ```
3. Registrieren Sie den Agent bei der Replikationsappliance:
    ```
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <replication appliance IP address> -P <Passphrase File Path>
    ```

## <a name="replicate-machines"></a>Replizieren von Computern

Wählen Sie nun Computer für die Migration aus. 

> [!NOTE]
> Sie können bis zu zehn Computer gleichzeitig replizieren. Müssen Sie mehr Computer replizieren, führen Sie die Replikation in Batches mit jeweils zehn Computern durch.

1. Klicken Sie im Azure Migrate-Projekt unter **Server** > **Azure Migrate: Servermigration** auf **Replizieren**.

    ![Replizieren von VMs](./media/tutorial-migrate-physical-virtual-machines/select-replicate.png)

2. Wählen Sie unter **Replizieren** > **Quelleinstellungen** > **Sind Ihre Computer virtualisiert?** die Option **Ja, mit VMware vSphere-Hypervisor** aus.
3. Wählen Sie unter **Lokale Appliance** den Namen der Azure Migrate-Appliance aus, die Sie eingerichtet haben.
4. Geben Sie unter **vCenter Server** den Namen des vCenter-Servers an, der die virtuellen Computer verwaltet, oder den vSphere-Server, auf dem die VMs gehostet werden.
5. Wählen Sie unter **Prozessserver** den Namen der Replikationsappliance aus.
6. Geben Sie unter **Gastanmeldeinformationen** ein VM-Administratorkonto an, das für die Pushinstallation des Mobility Service-Diensts verwendet wird. Da wir den Mobility Service in diesem Tutorial manuell installieren, können Sie ein beliebiges Dummy-Konto hinzufügen. Klicken Sie anschließend auf **Next: Virtuelle Computer**.

    ![Replizieren von VMs](./media/tutorial-migrate-physical-virtual-machines/source-settings.png)

7. Übernehmen Sie im Bereich **Virtuelle Computer** unter **Migrationseinstellungen aus einer Bewertung importieren?** die Standardeinstellung **Nein, ich gebe die Migrationseinstellungen manuell an**.
8. Wählen Sie alle VMs aus, die Sie migrieren möchten. Klicken Sie anschließend auf **Next: Zieleinstellungen**.

    ![Auswählen von VMs](./media/tutorial-migrate-physical-virtual-machines/select-vms.png)


9. Wählen Sie unter **Zieleinstellungen** das Abonnement und die Zielregion für die Migration aus, und geben Sie die Ressourcengruppe an, in der sich die Azure-VMs nach der Migration befinden.
10. Wählen Sie unter **Virtuelles Netzwerk** das Azure-VNET/-Subnetz aus, in das die Azure-VMs nach der Migration eingebunden werden.
11. Wählen Sie unter **Azure-Hybridvorteil**

    - die Option **Nein** aus, falls Sie den Azure-Hybridvorteil nicht anwenden möchten. Klicken Sie auf **Weiter**.
    - Wählen Sie **Ja** aus, wenn Sie über Windows Server-Computer verfügen, die durch aktive Software Assurance- oder Windows Server-Abonnements abgedeckt sind, und den Vorteil auf die zu migrierenden Computer anwenden möchten. Klicken Sie auf **Weiter**.

    ![Zieleinstellungen](./media/tutorial-migrate-physical-virtual-machines/target-settings.png)

12. Überprüfen Sie unter **Compute** den VM-Namen, die Größe, den Typ des Betriebssystemdatenträgers und die Verfügbarkeitsgruppe. Die VMs müssen die [Azure-Anforderungen](migrate-support-matrix-vmware.md#azure-vm-requirements) erfüllen.

    - **VM-Größe**: Standardmäßig wird von Azure Migrate-Servermigration eine Größe ausgewählt, die die höchste Übereinstimmung mit der Angabe im Azure-Abonnement aufweist. Alternativ können Sie unter **Azure-VM-Größe** manuell eine Größe auswählen. 
    - **Betriebssystemdatenträger**: Geben Sie den Betriebssystemdatenträger (Startdatenträger) für die VM an. Der Betriebssystemdatenträger enthält den Bootloader und das Installationsprogramm des Betriebssystems. 
    - **Verfügbarkeitsgruppe**: Wenn die VM nach der Migration in einer Azure-Verfügbarkeitsgruppe enthalten sein soll, geben Sie die Gruppe an. Die Gruppe muss Teil der Zielressourcengruppe sein, die Sie für die Migration angeben.

    ![Computeeinstellungen](./media/tutorial-migrate-physical-virtual-machines/compute-settings.png)

13. Geben Sie unter **Datenträger** an, ob die VM-Datenträger in Azure repliziert werden sollen, und wählen Sie den Datenträgertyp (SSD Standard/HDD Standard oder Managed Disks Premium) in Azure aus. Klicken Sie auf **Weiter**.
    - Sie können Datenträger von der Replikation ausschließen.
    - Wenn Sie Datenträger ausschließen, sind diese nach der Migration nicht auf dem virtuellen Azure-Computer vorhanden. 

    ![Datenträgereinstellungen](./media/tutorial-migrate-physical-virtual-machines/disks.png)


14. Überprüfen Sie unter **Replikation prüfen und starten** die Einstellungen, und klicken Sie auf **Replizieren**, um die erste Replikation für die Server zu starten.

> [!NOTE]
> Sie können die Replikationseinstellungen vor Beginn der Replikation jederzeit unter **Verwalten** > **Aktuell replizierte Computer** aktualisieren. Die Einstellungen können nach dem Beginn der Replikation nicht mehr geändert werden.



## <a name="track-and-monitor"></a>Nachverfolgen und Überwachen

- Wenn Sie auf **Replizieren** klicken, beginnt die Durchführung des Auftrags „Replikation starten“. 
- Nachdem der Auftrag „Replikation starten“ erfolgreich abgeschlossen wurde, beginnen die Computer mit der ersten Replikation in Azure.
- Nach Abschluss der ersten Replikation beginnt die Deltareplikation. Inkrementelle Änderungen an lokalen Datenträgern werden regelmäßig auf den Replikatdatenträgern in Azure repliziert.


Sie haben die Möglichkeit, den Auftragsstatus über die Portalbenachrichtigungen nachzuverfolgen.

Sie können den Replikationsstatus überwachen, indem Sie auf **Server werden repliziert** klicken (unter **Azure Migrate: Servermigration**).
![Überwachen der Replikation](./media/tutorial-migrate-physical-virtual-machines/replicating-servers.png)

## <a name="run-a-test-migration"></a>Ausführen einer Testmigration


Wenn die Deltareplikation beginnt, können Sie zunächst eine Testmigration für die VMs ausführen, bevor eine vollständige Migration zu Azure erfolgt. Dieser Schritt sollte vor der Migration mindestens einmal für jeden Computer ausgeführt werden.

- Bei einer Testmigration wird überprüft, ob die Migration wie erwartet funktioniert, ohne die lokalen Computer zu beeinträchtigen, und ob diese während der Replikation betriebsbereit bleiben. 
- Mit der Testmigration wird die Migration simuliert, indem eine Azure-VM mit replizierten Daten erstellt wird (normalerweise per Migration zu einem nicht für die Produktion bestimmten VNET unter Ihrem Azure-Abonnement).
- Sie können den replizierten virtuellen Azure-Testcomputer verwenden, um die Migration zu überprüfen, App-Tests durchzuführen und Probleme zu beheben, bevor die vollständige Migration erfolgt.

Führen Sie die Testmigration wie folgt durch:


1. Klicken Sie unter **Migrationsziele** > **Server** > **Azure Migrate: Servermigration** auf **Migrierte Server testen**.

     ![Testen der migrierten Server](./media/tutorial-migrate-physical-virtual-machines/test-migrated-servers.png)

2. Klicken Sie mit der rechten Maustaste auf die zu testende VM, und klicken Sie anschließend auf **Testmigration**.

    ![Testmigration](./media/tutorial-migrate-physical-virtual-machines/test-migrate.png)

3. Wählen Sie unter **Testmigration** das Azure VNET aus, in dem sich die Azure-VM nach der Migration befindet. Es empfiehlt sich, ein nicht für die Produktion bestimmtes VNET zu verwenden.
4. Der Auftrag **Testmigration** wird gestartet. Überwachen Sie den Auftrag anhand der Portalbenachrichtigungen.
5. Zeigen Sie die migrierte Azure-VM nach Abschluss der Migration im Azure-Portal unter **Virtuelle Computer** an. Der Computername enthält das Suffix **-Test**.
6. Klicken Sie nach Abschluss des Tests mit der rechten Maustaste unter **Aktuell replizierte Computer** auf die Azure-VM, und klicken Sie anschließend auf **Testmigration bereinigen**.

    ![Bereinigen der Migration](./media/tutorial-migrate-physical-virtual-machines/clean-up.png)


## <a name="migrate-vms"></a>Migrieren virtueller Computer

Nachdem Sie sich vergewissert haben, dass die Testmigration wie erwartet funktioniert, können Sie die lokalen Computer migrieren.

1. Klicken Sie im Azure Migrate-Projekt unter **Server** > **Azure Migrate: Servermigration** auf **Server werden repliziert**.

    ![Replizieren der Server](./media/tutorial-migrate-physical-virtual-machines/replicate-servers.png)

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
    - Ziehen Sie die Bereitstellung von [Azure Cost Management](https://docs.microsoft.com/azure/cost-management/overview) in Erwägung, um den Ressourceneinsatz und die Ausgaben zu überwachen.


## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich den Artikel zur [Cloudmigration](https://docs.microsoft.com/azure/architecture/cloud-adoption/getting-started/migrate) des Frameworks für die Cloudeinführung (Cloud Adoption Framework) an.
