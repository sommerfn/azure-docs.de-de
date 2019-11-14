---
title: Migrieren lokaler VMware-VMs zu Azure mit Agent-basierter Azure Migrate-Servermigration | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie für lokale Computer mithilfe der Azure Migrate-Servermigration eine Agent-basierte Migration zu Azure durchführen.
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 09/04/2019
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 07e91abc1130505abc84f6687be7edd04522fa76
ms.sourcegitcommit: bc7725874a1502aa4c069fc1804f1f249f4fa5f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73720178"
---
# <a name="migrate-vmware-vms-to-azure-agent-based"></a>Migrieren von VMware-VMs zu Azure (Agent-basiert)

In diesem Artikel erfahren Sie, wie Sie lokale VMware-VMs mithilfe der Agent-basierten Migration des Tools für die Azure Migrate-Servermigration zu Azure migrieren.

[Azure Migrate](migrate-services-overview.md) ist ein zentraler Hub zum Nachverfolgen der Ermittlung, Bewertung und Migration Ihrer lokalen Apps und Workloads sowie von AWS/GCP-VM-Instanzen zu Azure. Der Hub stellt Azure Migrate-Tools für die Bewertung und Migration sowie Angebote von unabhängigen Drittanbietern (Independent Software Vendors, ISVs) bereit.


In diesem Tutorial lernen Sie Folgendes:
> [!div class="checklist"]
> * Einrichten der Quellumgebung und Bereitstellen einer Azure Migrate-Replikationsappliance für die Agent-basierte Migration
> * Einrichten der Zielumgebung für die Migration
> * Richten Sie eine Replikationsrichtlinie ein.
> * Aktivieren Sie die Replikation.
> * Ausführen einer Testmigration, um sicherzustellen, dass alles wie erwartet funktioniert
> * Durchführen einer vollständigen Migration zu Azure

> [!NOTE]
> In den Tutorials wird der einfachste Bereitstellungspfad für ein Szenario erläutert, damit Sie schnell ein Proof of Concept einrichten können. Die Tutorials verwenden nach Möglichkeit Standardoptionen und zeigen nicht alle möglichen Einstellungen und Pfade. Ausführliche Anweisungen finden Sie in den Bewertungs- und Migrationsanleitungen für VMware.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) erstellen, bevor Sie beginnen.

## <a name="before-you-begin"></a>Voraussetzungen

Es empfiehlt sich, VMware-VMs mit der Azure Migrate-Serverbewertung zu bewerten, bevor Sie die VMs zu Azure migrieren. Gehen Sie zum Erstellen einer Bewertung wie folgt vor:

1. Führen Sie die Schritte des Tutorials [Vorbereiten von VMware-VMs für die Bewertung und die Migration zu Azure](tutorial-prepare-vmware.md) aus.
2. Führen Sie anschließend die Schritte [dieses Tutorials](tutorial-assess-vmware.md) aus, um eine Azure Migrate-Appliance für die Bewertung einzurichten und die VMs zu ermitteln und zu bewerten.


Wir empfehlen zwar, vor der VM-Migration eine Bewertung auszuprobieren, dies ist jedoch nicht zwingend erforderlich.

## <a name="migration-methods"></a>Migrationsmethoden

Sie können VMware-VMs zu Azure migrieren, indem Sie das Tool für die Azure Migrate-Servermigration verwenden. Dieses Tool bietet verschiedene Optionen für die Migration von VMware-VMs:

- Replikation ohne Agent: Migrieren von VMs, ohne dass auf ihnen Software installiert werden muss
- Agent-basierte Migration oder Replikation: Installieren eines Agents (genauer gesagt: des Mobility Services-Agents) auf der zu replizierenden VM

Orientieren Sie sich bei der Entscheidung für eine Migration mit oder ohne Agent an den folgenden Artikeln:

- Informationen zu den VMware-Migrationsoptionen finden Sie [hier](server-migrate-overview.md).
- [Vergleichen von Migrationsmethoden](server-migrate-overview.md#compare-migration-methods)
- Eine Anleitung zum Ausprobieren der Migration ohne Agent finden Sie [hier](tutorial-migrate-vmware.md).



## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie mit diesem Tutorial beginnen, sollten folgende Voraussetzungen erfüllt sein:

1. [Machen Sie sich mit der Architektur der VMware-Migration vertraut.](migrate-architecture.md)
2. Vergewissern Sie sich, dass Ihrem Azure-Konto die Rolle „Mitwirkender für virtuelle Computer“ zugewiesen ist, damit Sie über folgende Berechtigungen verfügen:

    - Erstellen einer VM in der ausgewählten Ressourcengruppe
    - Erstellen einer VM im ausgewählten virtuellen Netzwerk
    - Schreiben auf einen verwalteten Azure-Datenträger 

3. [Richten Sie ein Azure-Netzwerk ein](../virtual-network/manage-virtual-network.md#create-a-virtual-network). Lokale Computer werden auf verwalteten Azure-Datenträgern repliziert. Wenn Sie für die Migration ein Failover auf Azure durchführen, werden auf der Grundlage dieser verwalteten Datenträger Azure-VMs erstellt und mit einem Azure-Netzwerk verknüpft, das Sie beim Einrichten der Migration angeben.


## <a name="prepare-azure"></a>Vorbereiten von Azure

Wenn Sie bereits eine Bewertung mit der Azure Migrate-Serverbewertung ausgeführt haben, können Sie die Anweisungen in diesem Abschnitt überspringen, da Sie diese Schritte bereits ausgeführt haben. 

Falls Sie noch keine Bewertung ausgeführt haben, müssen Sie zunächst Azure-Berechtigungen einrichten, um eine Migration mit der Azure Migrate-Servermigration durchführen zu können.

- **Erstellen eines Projekts:** Ihr Azure-Konto benötigt Berechtigungen zum Erstellen eines Azure Migrate-Projekts. 
- **Registrieren der Azure Migrate-Replikationsappliance:** Die Replikationsappliance erstellt und registriert eine Azure Active Directory-App in Ihrem Azure-Konto. Hierfür müssen Berechtigungen delegiert werden.
- **Erstellen einer Key Vault-Instanz:** Für die Migration von VMware-VMs mithilfe der Azure Migrate-Servermigration erstellt Azure Migrate eine Key Vault-Instanz in der Ressourcengruppe, um Zugriffsschlüssel für das Replikationsspeicherkonto in Ihrem Abonnement zu verwalten. Für die Tresorerstellung benötigen Sie Rollenzuweisungsberechtigungen für die Ressourcengruppe, in der sich das Azure Migrate-Projekt befindet. 


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

    ![Azure AD-Berechtigungen](./media/tutorial-prepare-vmware/aad.png)

> [!NOTE]
> Dies ist eine Standardeinstellung, die nicht vertraulich ist. [Weitere Informationen](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-applications-are-added#who-has-permission-to-add-applications-to-my-azure-ad-instance)

#### <a name="assign-application-developer-role"></a>Zuweisen der Rolle „Anwendungsentwickler“ 

Der Mandantenadministrator/globale Administrator kann einem Konto die Rolle „Anwendungsentwickler“ zuweisen. [Weitere Informationen](../active-directory/fundamentals/active-directory-users-assign-role-azure-portal.md)

## <a name="assign-permissions-to-create-key-vault"></a>Zuweisen von Berechtigungen für die Key Vault-Erstellung

Gehen Sie wie folgt vor, um Rollenzuweisungsberechtigungen für die Ressourcengruppe zuzuweisen, in der sich das Azure Migrate-Projekt befindet:

1. Wählen Sie im Azure-Portal unter der Ressourcengruppe die Option **Zugriffssteuerung (IAM)** aus.
2. Suchen Sie unter **Zugriff überprüfen** nach dem relevanten Konto, und klicken Sie darauf, um Berechtigungen anzuzeigen. Sie benötigen die Berechtigung **Besitzer** (oder die Berechtigungen **Mitwirkender** und **Benutzerzugriffsadministrator**).
3. Sollten Sie nicht über die erforderlichen Berechtigungen verfügen, fordern Sie sie beim Besitzer der Ressourcengruppe an. 


## <a name="prepare-on-premises-vmware"></a>Lokales Vorbereiten von VMware

### <a name="prepare-an-account-for-automatic-discovery"></a>Vorbereiten eines Kontos für die automatische Ermittlung

Die Azure Migrate-Servermigration muss auf VMware-Server zugreifen können, um Folgendes zu ermöglichen:

- Automatisches Ermitteln von VMs. Dafür ist mindestens ein Konto mit Lesezugriff erforderlich.
- Orchestrieren von Replikation, Failover und Failback. Sie benötigen ein Konto, das berechtigt ist, Vorgänge wie das Erstellen und Entfernen von Datenträgern sowie das Einschalten virtueller Computer durchzuführen.

Erstellen Sie das Konto wie folgt:

1. Erstellen Sie zum Verwenden eines dedizierten Kontos eine Rolle auf vCenter-Ebene. Geben Sie der Rolle einen Namen wie **Azure_Site_Recovery**.
2. Weisen Sie der Rolle die Berechtigungen, die in der folgenden Tabelle aufgeführt sind, zu.
3. Erstellen Sie einen Benutzer auf dem vCenter-Server oder vSphere-Host. Weisen Sie dem Benutzer die Rolle zu.

#### <a name="vmware-account-permissions"></a>VMware-Kontoberechtigungen

**Aufgabe** | **Rolle/Berechtigungen** | **Details**
--- | --- | ---
**VM-Ermittlung** | Mindestens ein Benutzer mit Lesezugriff<br/><br/> Data Center object (Rechenzentrenobjekt) –> Propagate to Child Object (An untergeordnetes Objekt weitergeben), role=Read-only (Rolle=schreibgeschützt) | Der Benutzer wird auf Datencenterebene zugewiesen und hat Zugriff auf alle Objekte im Datencenter.<br/><br/> Um den Zugriff einzuschränken, weisen Sie den untergeordneten Objekten (vSphere-Hosts, Datenspeicher, VMs und Netzwerke) die Rolle **No access** (Kein Zugriff) mit **Propagate to child object** (Auf untergeordnetes Objekt übertragen) zu.
**Vollständige Replikation, Failover, Failback** |  Erstellen Sie eine Rolle („Azure_Site_Recovery“) mit den erforderlichen Berechtigungen, und weisen Sie die Rolle dann einem VMware-Benutzer oder einer VMware-Gruppe zu.<br/><br/> Rechenzentrumsobjekt -> An untergeordnetes Objekt weitergeben, Rolle=Azure_Site_Recovery<br/><br/> Datenspeicher -> Speicherplatz zuordnen, Datenspeicher durchsuchen, Low-Level-Dateivorgänge, Datei entfernen, Dateien virtueller Computer aktualisieren<br/><br/> Netzwerk -> Netzwerk zuweisen<br/><br/> Ressource -> Zuweisen der VM zu einem Ressourcenpool, ausgeschaltete VM migrieren, eingeschaltete VM migrieren<br/><br/> Tasks (Aufgaben) -> Create task (Aufgabe erstellen), update task (Aufgabe aktualisieren)<br/><br/> Virtueller Computer -> Konfiguration<br/><br/> Virtueller Computer -> Interagieren -> Frage beantworten, Geräteverbindung, CD-Medien konfigurieren, Diskettenmedien konfigurieren, Ausschalten, Einschalten, VMware-Tools installieren<br/><br/> Virtueller Computer -> Inventar -> Erstellen, Registrieren, Registrierung aufheben<br/><br/> Virtueller Computer -> Bereitstellung -> Download virtueller Computer zulassen, Upload von Dateien virtueller Computer zulassen<br/><br/> Virtual machine -> Snapshots -> Remove snapshots | Der Benutzer wird auf Datencenterebene zugewiesen und hat Zugriff auf alle Objekte im Datencenter.<br/><br/> Um den Zugriff einzuschränken, weisen Sie den untergeordneten Objekten (vSphere-Hosts, Datenspeicher, VMs und Netzwerke) die Rolle **No access** (Kein Zugriff) mit **Propagate to child object** (Auf untergeordnetes Objekt übertragen) zu.

### <a name="prepare-an-account-for-mobility-service-installation"></a>Vorbereiten eines Kontos für die Installation des Mobility Services

Der Mobilitätsdienst muss auf Computern installiert sein, die Sie replizieren möchten.

- Die Azure Migrate-Replikationsappliance kann eine Pushinstallation dieses Diensts ausführen, wenn Sie die Replikation für einen Computer aktivieren. Sie können den Dienst aber auch manuell oder mithilfe von Installationstools installieren.
- In diesem Tutorial installieren wir den Mobilitätsdienst mit der Pushinstallation.
- Für die Pushinstallation müssen Sie ein Konto vorbereiten, über das die Azure Migrate-Servermigration auf die VM zugreifen kann.

Bereiten Sie das Konto wie folgt vor:

1. Bereiten Sie ein Domänen- oder lokales Konto mit den Berechtigungen zum Installieren auf dem virtuellen Computer vor.
2. Für Windows-VMs gilt: Wenn Sie kein Domänenkonto verwenden, deaktivieren Sie die Remote-Benutzerzugriffssteuerung auf dem lokalen Computer, indem Sie in der Registrierung unter **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System** den DWORD-Eintrag **LocalAccountTokenFilterPolicy** mit dem Wert „1“ hinzufügen.
3. Bereiten Sie für Linux-VMs ein root-Konto auf dem Linux-Quellserver vor.


### <a name="check-vmware-requirements"></a>Überprüfen der VMware-Anforderungen

Vergewissern Sie sich, dass die VMware-Server und -VMs die Anforderungen für die Migration zu Azure erfüllen. 


> [!NOTE]
> Die Agent-basierte Migration mit der Azure Migrate-Servermigration basiert auf Features des Azure Site Recovery-Diensts. Einige Anforderungen sind daher ggf. mit der Site Recovery-Dokumentation verknüpft.

1. [Überprüfen](migrate-support-matrix-vmware.md#agent-based-migration-vmware-server-requirements) Sie die Anforderungen für VMware-Server.
2. [Überprüfen](migrate-support-matrix-vmware.md#agent-based-migration-vmware-vm-requirements) Sie die VM-Unterstützungsanforderungen für die Migration.
3. Überprüfen Sie die VM-Einstellungen. Lokale VMs, die Sie zu Azure replizieren möchten, müssen die [Azure-VM-Anforderungen](migrate-support-matrix-vmware.md#azure-vm-requirements) erfüllen.



## <a name="add-the-azure-migrate-server-migration-tool"></a>Hinzufügen des Tools für die Azure Migrate-Servermigration

Falls Sie das Tutorial zur Bewertung von VMware-VMs noch nicht absolviert haben, richten Sie ein Azure Migrate-Projekt ein, und fügen Sie anschließend das Tool für die Azure Migrate-Servermigration hinzu:

1. Wählen Sie im Azure-Portal **Alle Dienste** aus, und suchen Sie nach **Azure Migrate**.
2. Wählen Sie unter **Dienste** die Option **Azure Migrate** aus.

    ![Einrichten von Azure Migrate](./media/tutorial-migrate-vmware-agent/azure-migrate-search.png)

3. Klicken Sie in der **Übersicht** auf **Server bewerten und migrieren**.
4. Klicken Sie unter **Server ermitteln, bewerten und migrieren** auf **Server bewerten und migrieren**.

    ![Ermitteln und Bewerten von Servern](./media/tutorial-migrate-vmware-agent/assess-migrate.png)

1. Klicken Sie unter **Server ermitteln, bewerten und migrieren** auf **Tools hinzufügen**.
2. Wählen Sie unter **Projekt migrieren** Ihr Azure-Abonnement aus, und erstellen Sie bei Bedarf eine Ressourcengruppe.
3. Geben Sie unter **Projektdetails** den Projektnamen und die geografische Region an, in der Sie das Projekt erstellen möchten. Klicken Sie anschließend auf **Weiter**.

    ![Erstellen eines Azure Migrate-Projekts](./media/tutorial-migrate-vmware-agent/migrate-project.png)

    Sie können Azure Migrate-Projekte in den unten angegebenen geografischen Regionen erstellen.

    **Geografie** | **Region**
    --- | ---
    Asien | Asien, Südosten
    Europa | „Europa, Norden“ oder „Europa, Westen“
    USA | „USA, Osten“ oder „USA, Westen-Mitte“

    Die für das Projekt angegebene Region wird nur zum Speichern der Metadaten verwendet, die von den lokalen VMs erfasst werden. Bei der tatsächlichen Migration kann eine beliebige Zielregion ausgewählt werden.
4. Wählen Sie unter **Bewertungstool auswählen** die Option **Hinzufügen eines Bewertungstools vorerst überspringen** >  und anschließend **Weiter** aus.
5. Wählen Sie unter **Migrationstool auswählen** Folgendes aus: **Azure Migrate: Servermigration** > **Weiter**.
6. Überprüfen Sie die Einstellungen unter **Review + add tools** (Überprüfen und Tools hinzufügen), und klicken Sie auf **Tools hinzufügen**.
7. Nachdem Sie das Tool hinzugefügt haben, wird es im Azure Migrate-Projekt unter **Server** > **Migrationstools** angezeigt.

## <a name="set-up-the-replication-appliance"></a>Einrichten der Replikationsappliance

Der erste Schritt bei der Migration besteht darin, die Replikationsappliance einzurichten. Bei der Replikationsappliance handelt es sich um eine einzelne hochverfügbare lokale VMware-VM, auf der folgende Komponenten gehostet werden:

- **Konfigurationsserver**: Der Konfigurationsserver koordiniert die Kommunikation zwischen der lokalen Umgebung und Azure und verwaltet die Datenreplikation.
- **Prozessserver** Der Prozessserver fungiert als Replikationsgateway. Er empfängt Replikationsdaten, optimiert sie durch Zwischenspeicherung, Komprimierung und Verschlüsselung und sendet sie an ein Cachespeicherkonto in Azure. Der Prozessserver installiert auch den Mobility Service-Agent auf VMs, die Sie replizieren möchten, und führt eine automatische Ermittlung der lokalen VMware-VMs durch.


Für die Einrichtung der Replikationsappliance laden Sie eine vorbereitete OVA-Vorlage (Open Virtualization Application) herunter. Anschließend importieren Sie die Vorlage in VMware und erstellen die Replikationsappliance-VM. 

### <a name="download-the-replication-appliance-template"></a>Herunterladen der Vorlage für die Replikationsappliance

Gehen Sie wie folgt vor, um die Vorlage herunterzuladen:

1. Klicken Sie im Azure Migrate-Projekt unter **Migrationsziele** auf **Server**.
2. Klicken Sie unter **Azure Migrate – Server** > **Azure Migrate: Servermigration** auf **Ermitteln**.

    ![VMs ermitteln](./media/tutorial-migrate-vmware-agent/migrate-discover.png)

3. Wählen Sie unter **Computer ermitteln** > **Sind Ihre Computer virtualisiert?** die Option **Ja, mit VMware vSphere Hypervisor** aus.
4. Wählen Sie unter **Wie möchten Sie die Migration durchführen?** die Option **Mit der Agent-basierten Replikation** aus.
5. Wählen Sie unter **Zielregion** die Azure-Region aus, zu der Sie die Computer migrieren möchten.
6. Aktivieren Sie das Kontrollkästchen **Bestätigen Sie, dass die Zielregion für die Migration „<Name der Region>“ lautet**.
7. Klicken Sie auf **Ressourcen erstellen**. Daraufhin wird im Hintergrund ein Azure Site Recovery-Tresor erstellt.
    - Nach dem Klicken auf diese Schaltfläche kann die Zielregion für dieses Projekt nicht mehr geändert werden.
    - Alle anschließenden Migrationen erfolgen zu dieser Region.

    ![Erstellen eines Recovery Services-Tresors](./media/tutorial-migrate-vmware-agent/create-resources.png)

8. Wählen Sie unter **Möchten Sie eine neue Replikationsappliance installieren oder ein vorhandenes Setup horizontal hochskalieren?** die Option **Replikationsappliance installieren** aus.
9. Klicken Sie auf **Herunterladen**, um die Replikationsappliance herunterzuladen. Daraufhin wird eine OVF-Vorlage heruntergeladen, auf deren Grundlage Sie eine neue VMware-VM erstellen, die die Appliance ausführt.
    ![Herunterladen der OVA-Vorlage](./media/tutorial-migrate-vmware-agent/download-ova.png)
10. Notieren Sie sich den Namen der Ressourcengruppe und des Recovery Services-Tresors. Sie werden bei der Appliancebereitstellung benötigt.


### <a name="import-the-template-in-vmware"></a>Importieren der Vorlage in VMware

Die heruntergeladene OVF-Vorlage muss in VMware importiert werden, um die Replikationsappliance auf einer VMware-VM unter Windows Server 2016 zu erstellen.

1. Melden Sie sich mit dem VMWare vSphere-Client beim VMware vCenter-Server oder beim vSphere ESXi-Host an.
2. Wählen Sie im Menü **Datei** die Option **OVF-Vorlage bereitstellen** aus, um den **Assistenten zum Bereitstellen von OVF-Vorlagen** zu starten. 
3. Geben Sie unter **Quelle auswählen** den Speicherort der heruntergeladenen OVF-Vorlage ein.
4. Klicken Sie unter **Bewertungsdetails** auf **Weiter**.
5. Übernehmen Sie in **Name und Ordner auswählen** und **Konfiguration auswählen** die Standardeinstellungen.
6. Wählen Sie unter **Speicher auswählen** > **Select virtual disk format** (Format für virtuellen Datenträger auswählen) die Option **Thick Provision Eager Zeroed** (Breite Bereitstellung gegen null) aus, um eine optimale Leistung zu erzielen.
7. Übernehmen Sie auf den übrigen Seiten des Assistenten die Standardeinstellungen.
8. Wählen Sie unter **Zum Abschluss bereit** zum Einrichten des virtuellen Computers mit den Standardeinstellungen **Power on after deployment** (Nach Bereitstellung einschalten) > **Fertig stellen** aus.

   > [!TIP]
   > Wenn Sie einen weiteren Netzwerkadapter hinzufügen möchten, deaktivieren Sie **Power on after deployment** (Nach Bereitstellung einschalten) > **Fertig stellen**. Standardmäßig enthält die Vorlage einen einzelnen Netzwerkadapter. Weitere NICs können nach der Bereitstellung hinzugefügt werden.

### <a name="kick-off-replication-appliance-setup"></a>Starten der Einrichtung der Replikationsappliance

1. Schalten Sie den virtuellen Computer über die Konsole des VMware vSphere-Clients an.
2. Der virtuelle Computer wird mit der Benutzeroberfläche für die Installation von Windows Server 2016 hochgefahren. Akzeptieren Sie den Lizenzvertrag, und geben Sie ein Administratorkennwort ein.
3. Melden Sie sich nach Abschluss der Installation unter Verwendung des Administratorkennworts als Administrator bei der VM an.
4. Bei der ersten Anmeldung wird innerhalb weniger Sekunden das Setuptool für die Replikationsappliance (Azure Site Recovery-Konfigurationstool) gestartet.
5. Geben Sie einen Namen ein, um die Appliance bei der Azure Migrate-Servermigration zu registrieren. Klicken Sie auf **Weiter**.
6. Das Tool überprüft, ob der virtuelle Computer eine Verbindung mit Azure herstellen kann. Klicken Sie nach der Verbindungsherstellung auf **Anmelden**, um sich bei Ihrem Azure-Abonnement anzumelden.
7. Warten Sie, bis das Tool eine Azure AD-App zur Identifizierung der Appliance registriert hat. Die Appliance wird neu gestartet.
1. Melden Sie sich erneut am Computer an. Der Assistent für die Konfigurationsserververwaltung wird innerhalb weniger Sekunden automatisch gestartet.

### <a name="register-the-replication-appliance"></a>Registrieren der Replikationsappliance

Schließen Sie die Einrichtung und Registrierung der Replikationsappliance ab.

1. Wählen Sie im Assistenten für die Konfigurationsserververwaltung die Option **Konnektivität einrichten** aus.
2. Wählen Sie die NIC aus, die die Replikationsappliance für die VM-Ermittlung sowie für eine Pushinstallation des Mobility-Diensts auf Quellcomputern verwendet. (Standardmäßig ist nur eine NIC verfügbar.)
3. Wählen Sie die NIC aus, die die Replikationsappliance für die Konnektivität mit Azure verwendet. Klicken Sie dann auf **Speichern**. Diese Einstellung kann nach der Konfiguration nicht mehr geändert werden.
4. Falls sich die Appliance hinter einem Proxyserver befindet, müssen Proxyeinstellungen angegeben werden.
    - Geben Sie den Proxynamen im Format **http://ip-address** oder **http://FQDN** an. HTTPS-Proxyserver werden nicht unterstützt.
5. Wenn Sie zur Eingabe des Abonnements, der Ressourcengruppen und der Tresordetails aufgefordert werden, fügen Sie die Angaben hinzu, die Sie sich beim Herunterladen der Appliancevorlage notiert haben.
6. Akzeptieren Sie unter **Drittanbietersoftware installieren** den Lizenzvertrag. Klicken Sie auf **Herunterladen und installieren**, um MySQL Server zu installieren.
7. Klicken Sie auf **VMware PowerCLI installieren**. Stellen Sie sicher, dass alle Browserfenster geschlossen sind, bevor Sie diesen Schritt durchführen. Klicken Sie anschließend auf **Weiter**.
8. Unter **Anwendungskonfiguration überprüfen** werden die Voraussetzungen überprüft, bevor der Vorgang fortgesetzt wird.
9. Geben Sie unter **vCenter-Server/vSphere ESXi-Server konfigurieren** den FQDN oder die IP-Adresse des vCenter-Servers oder vSphere-Hosts ein, auf dem sich die virtuellen Computer befinden, die repliziert werden sollen. Geben Sie den Port ein, über den der Server lauscht. Geben Sie einen Anzeigenamen ein, der für den VMware-Server im Tresor verwendet werden soll.
10. Geben Sie die Anmeldeinformationen für das Konto ein, das Sie für die VMware-Ermittlung [erstellt](#prepare-an-account-for-automatic-discovery) haben. Wählen Sie **Hinzufügen** > **Weiter** aus.
11. Geben Sie beim Aktivieren der VM-Replikation unter **Anmeldeinformationen für virtuelle Computer konfigurieren** die Anmeldeinformationen ein, die Sie für die Pushinstallation des Mobility-Diensts [erstellt](#prepare-an-account-for-mobility-service-installation) haben.  
    - Für Windows-Computer benötigt das Konto lokale Administratorrechte auf den Computern, die Sie replizieren möchten.
    - Bei Linux geben Sie die Anmeldeinformationen für das root-Konto an.
12. Klicken Sie auf **Konfiguration abschließen**, um die Registrierung abzuschließen.


Nachdem die Replikationsappliance registriert wurde, stellt die Azure Migrate-Serverbewertung unter Verwendung der angegebenen Einstellungen eine Verbindung mit VMware-Servern her und ermittelt VMs. Die ermittelten VMs werden unter **Verwalten** > **Ermittelte Elemente** auf der Registerkarte **Sonstiges** angezeigt.


## <a name="replicate-vms"></a>Replizieren von VMs

Wählen Sie nun virtuelle Computer für die Migration aus.

> [!NOTE]
> Sie können bis zu zehn Computer gleichzeitig replizieren. Müssen Sie mehr Computer replizieren, führen Sie die Replikation in Batches mit jeweils zehn Computern durch.

1. Klicken Sie im Azure Migrate-Projekt unter **Server** > **Azure Migrate: Servermigration** auf **Replizieren**.

    ![Replizieren von VMs](./media/tutorial-migrate-vmware-agent/select-replicate.png)

2. Wählen Sie unter **Replizieren** > **Quelleinstellungen** > **Sind Ihre Computer virtualisiert?** die Option **Ja, mit VMware vSphere-Hypervisor** aus.
3. Wählen Sie unter **Lokale Appliance** den Namen der Azure Migrate-Appliance aus, die Sie eingerichtet haben.
4. Geben Sie unter **vCenter Server** den Namen des vCenter-Servers an, der die virtuellen Computer verwaltet, oder den vSphere-Server, auf dem die VMs gehostet werden.
5. Wählen Sie unter **Prozessserver** den Namen der Replikationsappliance aus.
6. Geben Sie unter **Gastanmeldeinformationen** das VM-Administratorkonto an, das für die Pushinstallation des Mobility-Diensts verwendet wird. Klicken Sie anschließend auf **Next: Virtuelle Computer**.

    ![Replizieren von VMs](./media/tutorial-migrate-vmware-agent/source-settings.png)

7. Wählen Sie unter **Virtuelle Computer** die Computer aus, die Sie replizieren möchten.

    - Wenn Sie eine Bewertung für die VMs ausgeführt haben, können Sie die Empfehlungen zur VM-Größenanpassung und zum Datenträgertyp (Premium/Standard) aus den Bewertungsergebnissen anwenden. Wählen Sie hierzu unter **Migrationseinstellungen aus einer Azure Migrate-Bewertung importieren?** die Option **Ja** aus.
    - Wählen Sie **Nein** aus, wenn Sie keine Bewertung ausgeführt haben oder die Bewertungseinstellungen nicht verwenden möchten.
    - Falls Sie sich für die Verwendung der Bewertung entschieden haben, wählen Sie die VM-Gruppe und den Bewertungsnamen aus.

8. Wählen Sie alle VMs aus, die Sie migrieren möchten. Klicken Sie anschließend auf **Next: Zieleinstellungen**.
9. Wählen Sie unter **Zieleinstellungen** das Abonnement und die Zielregion für die Migration aus, und geben Sie die Ressourcengruppe an, in der sich die Azure-VMs nach der Migration befinden.
10. Wählen Sie unter **Virtuelles Netzwerk** das Azure-VNET/-Subnetz aus, in das die Azure-VMs nach der Migration eingebunden werden.
11. Wählen Sie unter **Azure-Hybridvorteil**

    - die Option **Nein** aus, falls Sie den Azure-Hybridvorteil nicht anwenden möchten. Klicken Sie auf **Weiter**.
    - Wählen Sie **Ja** aus, wenn Sie über Windows Server-Computer verfügen, die durch aktive Software Assurance- oder Windows Server-Abonnements abgedeckt sind, und den Vorteil auf die zu migrierenden Computer anwenden möchten. Klicken Sie auf **Weiter**.

12. Überprüfen Sie unter **Compute** den VM-Namen, die Größe, den Typ des Betriebssystemdatenträgers und die Verfügbarkeitsgruppe. Die VMs müssen die [Azure-Anforderungen](migrate-support-matrix-vmware.md#agentless-migration-vmware-vm-requirements) erfüllen.

    - **VM-Größe**: Bei Verwendung von Bewertungsempfehlungen enthält die Dropdownliste für die VM-Größe die empfohlene Größe. Andernfalls wählt Azure Migrate eine Größe basierend auf der höchsten Übereinstimmung im Azure-Abonnement aus. Alternativ können Sie unter **Azure-VM-Größe** manuell eine Größe auswählen. 
    - **Betriebssystemdatenträger**: Geben Sie den Betriebssystemdatenträger (Startdatenträger) für die VM an. Der Betriebssystemdatenträger enthält den Bootloader und das Installationsprogramm des Betriebssystems. 
    - **Verfügbarkeitsgruppe**: Wenn die VM nach der Migration in einer Azure-Verfügbarkeitsgruppe enthalten sein soll, geben Sie die Gruppe an. Die Gruppe muss Teil der Zielressourcengruppe sein, die Sie für die Migration angeben.

13. Geben Sie unter **Datenträger** an, ob die VM-Datenträger in Azure repliziert werden sollen, und wählen Sie den Datenträgertyp (SSD Standard/HDD Standard oder Managed Disks Premium) in Azure aus. Klicken Sie auf **Weiter**.
    - Sie können Datenträger von der Replikation ausschließen.
    - Wenn Sie Datenträger ausschließen, sind diese nach der Migration nicht auf der Azure-VM vorhanden. 

14. Überprüfen Sie unter **Replikation prüfen und starten** die Einstellungen, und klicken Sie auf **Replizieren**, um die erste Replikation für die Server zu starten.

> [!NOTE]
> Sie können die Replikationseinstellungen vor Beginn der Replikation jederzeit unter **Verwalten** > **Aktuell replizierte Computer** aktualisieren. Die Einstellungen können nach dem Beginn der Replikation nicht mehr geändert werden.




## <a name="track-and-monitor"></a>Nachverfolgen und Überwachen

- Wenn Sie auf **Replizieren** klicken, beginnt die Durchführung des Auftrags „Replikation starten“. 
- Nachdem der Auftrag „Replikation starten“ erfolgreich abgeschlossen wurde, beginnen die Computer mit der ersten Replikation in Azure.
- Nach Abschluss der ersten Replikation beginnt die Deltareplikation. Inkrementelle Änderungen an lokalen Datenträgern werden regelmäßig auf den Replikatdatenträgern in Azure repliziert.


Sie können den Auftragsstatus über die Portalbenachrichtigungen nachverfolgen.

![Nachverfolgen des Auftrags](./media/tutorial-migrate-vmware-agent/jobs.png)

Sie können den Replikationsstatus überwachen, indem Sie auf **Server werden repliziert** klicken (unter **Azure Migrate: Servermigration**).
![Überwachen der Replikation](./media/tutorial-migrate-vmware-agent/replicate-servers.png)

## <a name="run-a-test-migration"></a>Ausführen einer Testmigration


Wenn die Deltareplikation beginnt, können Sie zunächst eine Testmigration für die VMs ausführen, bevor eine vollständige Migration zu Azure erfolgt. Dieser Schritt sollte vor der Migration mindestens einmal für jeden Computer ausgeführt werden.

- Bei einer Testmigration wird überprüft, ob die Migration wie erwartet funktioniert, ohne die lokalen Computer zu beeinträchtigen, und ob diese während der Replikation betriebsbereit bleiben. 
- Mit der Testmigration wird die Migration simuliert, indem eine Azure-VM mit replizierten Daten erstellt wird (normalerweise per Migration zu einem nicht für die Produktion bestimmten VNET unter Ihrem Azure-Abonnement).
- Sie können den replizierten virtuellen Azure-Testcomputer verwenden, um die Migration zu überprüfen, App-Tests durchzuführen und Probleme zu beheben, bevor die vollständige Migration erfolgt.

Führen Sie die Testmigration wie folgt durch:


1. Klicken Sie unter **Migrationsziele** > **Server** > **Azure Migrate: Servermigration** auf **Migrierte Server testen**.

     ![Testen der migrierten Server](./media/tutorial-migrate-vmware-agent/test-migrated-servers.png)

2. Klicken Sie mit der rechten Maustaste auf die zu testende VM, und klicken Sie anschließend auf **Testmigration**.

    ![Testmigration](./media/tutorial-migrate-vmware-agent/test-migrate.png)

3. Wählen Sie unter **Testmigration** das Azure VNET aus, in dem sich die Azure-VM nach der Migration befindet. Es empfiehlt sich, ein nicht für die Produktion bestimmtes VNET zu verwenden.
4. Der Auftrag **Testmigration** wird gestartet. Überwachen Sie den Auftrag anhand der Portalbenachrichtigungen.
5. Zeigen Sie die migrierte Azure-VM nach Abschluss der Migration im Azure-Portal unter **Virtuelle Computer** an. Der Computername enthält das Suffix **-Test**.
6. Klicken Sie nach Abschluss des Tests mit der rechten Maustaste unter **Aktuell replizierte Computer** auf die Azure-VM, und klicken Sie anschließend auf **Testmigration bereinigen**.

    ![Bereinigen der Migration](./media/tutorial-migrate-vmware-agent/clean-up.png)


## <a name="migrate-vms"></a>Migrieren virtueller Computer

Nachdem Sie sich vergewissert haben, dass die Testmigration wie erwartet funktioniert, können Sie die lokalen Computer migrieren.

1. Klicken Sie im Azure Migrate-Projekt unter **Server** > **Azure Migrate: Servermigration** auf **Server werden repliziert**.

    ![Replizieren der Server](./media/tutorial-migrate-vmware-agent/replicate-servers.png)

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

- Lokal
    - Migrieren Sie den App-Datenverkehr zur App, die auf der migrierten Azure-VM-Instanz ausgeführt wird.
    - Entfernen Sie die lokalen VMs aus Ihrem lokalen VM-Bestand.
    - Entfernen Sie die lokalen VMs aus lokalen Sicherungen.
    - Aktualisieren Sie die interne Dokumentation zum Anzeigen des neuen Speicherorts und der IP-Adresse der Azure-VMs.
- Optimieren der Azure-VM-Einstellungen nach der Migration:
    - Der [Azure-VM-Agent](../virtual-machines/extensions/agent-windows.md) verwaltet VM-Interaktionen mit dem Azure Fabric Controller. Dieser ist für einige Azure-Dienste wie Azure Backup, Site Recovery und das Azure Security Center erforderlich. Beim Migrieren von virtuellen VMare-Computern mit Agent-basierter Migration installiert das Mobility Service-Installationsprogramm den Azure-VM-Agent auf Windows-Computern. Auf virtuellen Linux-Computern empfehlen wir, den Agent nach der Migration zu installieren.
    - Deinstallieren Sie den Mobility Service nach der Migration manuell von dem virtuellen Azure-Computer.
    - Deinstallieren Sie VMware-Tools nach der Migration manuell.
- In Azure:
    - Führen Sie App-Anpassungen nach der Migration durch, z.B. die Aktualisierung von Datenbankverbindungszeichenfolgen und Webserverkonfigurationen.
    - Führen Sie endgültige Anwendungs- und Migrationsakzeptanztests für die migrierte Anwendung durch, die nun in Azure ausgeführt wird.
- Geschäftskontinuität/Notfallwiederherstellung
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
