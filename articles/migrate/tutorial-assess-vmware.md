---
title: Bewerten von VMware-VMs für die Migration zu Azure mit Azure Migrate
description: Hier erfahren Sie, wie Sie lokale VMware-VMs mithilfe von Azure Migrate für die Migration zu Azure bewerten.
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 10/11/2019
ms.author: hamusa
ms.openlocfilehash: 46bf756a729441bd3bc4b2b00aaa2c79fa06c0b8
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73521221"
---
# <a name="assess-vmware-vms-with-azure-migrate-server-assessment"></a>Bewerten von VMware-VMs mit der Azure Migrate-Serverbewertung

In diesem Artikel erfahren Sie, wie Sie lokale VMware-VMs mithilfe der Azure Migrate-Serverbewertung bewerten.

[Azure Migrate](migrate-services-overview.md) stellt einen Hub mit Tools bereit, die Ihnen dabei helfen, Apps, Infrastrukturen und Workloads zu ermitteln, zu bewerten und zu Microsoft Azure zu migrieren. Der Hub umfasst Azure Migrate-Tools sowie Angebote von unabhängigen Drittanbietern (Independent Software Vendors, ISVs).



Dieses Tutorial ist das zweite in einer Reihe zur Bewertung und Migration von VMware-VMs zu Azure. In diesem Tutorial lernen Sie Folgendes:
> [!div class="checklist"]
> * Einrichten eines Azure Migrate-Projekts
> * Einrichten einer lokal ausgeführten Azure Migrate-Appliance zur Bewertung von VMs
> * Starten der kontinuierlichen Ermittlung lokaler VMs. Die Appliance sendet Konfigurations- und Leistungsdaten für erkannte VMs an Azure.
> * Gruppieren erkannter VMs und Bewerten der VM-Gruppe
> * Überprüfen der Bewertung



> [!NOTE]
> In den Tutorials wird der einfachste Bereitstellungspfad für ein Szenario erläutert, damit Sie schnell einen Proof of Concept einrichten können. Die Tutorials verwenden nach Möglichkeit Standardoptionen und zeigen nicht alle möglichen Einstellungen und Pfade. Ausführliche Anweisungen finden Sie in den Anleitungsartikeln.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) erstellen, bevor Sie beginnen.


## <a name="prerequisites"></a>Voraussetzungen

- [Absolvieren Sie das erste Tutorial dieser Reihe.](tutorial-prepare-vmware.md) Andernfalls funktionieren die Anweisungen in diesem Tutorial nicht.
- Im ersten Tutorial müssen folgende Schritte ausgeführt werden:
    - [Einrichten von Azure-Berechtigungen](tutorial-prepare-vmware.md#prepare-azure) für Azure Migrate
    - [Vorbereiten von VMware](tutorial-prepare-vmware.md#prepare-for-vmware-vm-assessment) für die Bewertung. Die VMware-Einstellungen müssen überprüft worden sein, und Sie müssen über Berechtigungen zum Erstellen einer VMware-VM mit einer OVA-Vorlage verfügen. Außerdem muss ein Konto für die VM-Ermittlung eingerichtet worden sein. Die erforderlichen Ports müssen verfügbar sein, und Ihnen müssen die URLs bekannt sein, die für den Zugriff auf Azure benötigt werden.


## <a name="set-up-an-azure-migrate-project"></a>Einrichten eines Azure Migrate-Projekts

Richten Sie wie folgt ein neues Azure Migrate-Projekt ein:

1. Wählen Sie im Azure-Portal **Alle Dienste** aus, und suchen Sie nach **Azure Migrate**.
2. Wählen Sie unter **Dienste** die Option **Azure Migrate** aus.
3. Klicken Sie in der **Übersicht** unter **Server ermitteln, bewerten und migrieren** auf **Server bewerten und migrieren**.

    ![Ermitteln und Bewerten von Servern](./media/tutorial-assess-vmware/assess-migrate.png)

4. Klicken Sie unter **Erste Schritte** auf **Tools hinzufügen**.
5. Wählen Sie unter **Projekt migrieren** Ihr Azure-Abonnement aus, und erstellen Sie bei Bedarf eine Ressourcengruppe.     
6. Geben Sie unter **Projektdetails** den Projektnamen und die geografische Region an, in der Sie das Projekt erstellen möchten. Unterstützt werden Asien, Europa, das Vereinigte Königreich und die USA.

    - Die geografische Region des Projekts dient nur zum Speichern der Metadaten, die von den lokalen VMs erfasst werden.
    - Beim Ausführen einer Migration kann eine beliebige Zielregion ausgewählt werden.

    ![Erstellen eines Azure Migrate-Projekts](./media/tutorial-assess-vmware/migrate-project.png)


7. Klicken Sie auf **Weiter**.
8. Wählen Sie unter **Bewertungstool auswählen** Folgendes aus: **Azure Migrate: Server Assessment** (Azure Migrate-Serverbewertung) > **Weiter**.

    ![Erstellen eines Azure Migrate-Projekts](./media/tutorial-assess-vmware/assessment-tool.png)

9. Wählen Sie unter **Migrationstool auswählen** die Option **Hinzufügen eines Migrationstools vorerst überspringen** und anschließend **Weiter** aus.
10. Überprüfen Sie die Einstellungen unter **Review + add tools** (Überprüfen und Tools hinzufügen), und klicken Sie auf **Tools hinzufügen**.
11. Warten Sie einige Minuten, bis das Azure Migrate-Projekt bereitgestellt wurde. Sie werden zur Projektseite weitergeleitet. Sollte das Projekt nicht angezeigt werden, können Sie auf dem Azure Migrate-Dashboard unter **Server** darauf zugreifen.


## <a name="set-up-the-appliance-vm"></a>Einrichten der Appliance-VM

Von der Azure Von der Serverbewertung wird eine einfache VMware-VM-Appliance ausgeführt.

- Diese Appliance ermittelt VMs und sendet Meta- und Leistungsdaten zu VMs an die Azure Migrate-Serverbewertung.
- Die Einrichtung der Appliance umfasst Folgendes:
    - Herunterladen einer OVA-Vorlagendatei und Importieren der Datei in vCenter Server
    - Erstellen der Appliance und Überprüfen der Verbindungsherstellung mit der Azure Migrate-Serverbewertung
    - Durchführen der Erstkonfiguration für die Appliance und Registrieren der Appliance beim Azure Migrate-Projekt
- Für ein einzelnes Azure Migrate-Projekt können mehrere Appliances eingerichtet werden. Für alle Appliances können insgesamt bis zu 35.000 VMs ermittelt werden. Pro Appliance können maximal 10.000 Server ermittelt werden.

### <a name="download-the-ova-template"></a>Herunterladen der OVA-Vorlage

1. Klicken Sie unter **Migrationsziele** > **Server** > **Azure Migrate: Server Assessment** (Azure Migrate-Serverbewertung) auf **Ermitteln**.
2. Wählen Sie unter **Computer ermitteln** > **Sind Ihre Computer virtualisiert?** die Option **Ja, mit VMware vSphere Hypervisor** aus.
3. Klicken Sie auf **Herunterladen**, um die OVA-Vorlagendatei herunterzuladen.

    ![Herunterladen der OVA-Datei](./media/tutorial-assess-vmware/download-ova.png)


### <a name="verify-security"></a>Überprüfen der Sicherheit

Vergewissern Sie sich vor der Bereitstellung, dass die OVA-Datei sicher ist.

1. Öffnen Sie auf dem Computer, auf den Sie die Datei heruntergeladen haben, ein Administratorbefehlsfenster.
2. Führen Sie den folgenden Befehl aus, um den Hash für die OVA-Datei zu generieren:
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Beispielverwendung: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3. Für die Version 2.19.07.30 muss der generierte Hash den folgenden Werten entsprechen:

  **Algorithmus** | **Hashwert**
  --- | ---
  MD5 | 27230f3b012187860281b912ee661709
  SHA256 | c0a5b5998b7f38ac6e57ea9a808ecc4295795e18f9ca99c367585068883f06e7


### <a name="create-the-appliance-vm"></a>Erstellen der Appliance-VM

Importieren Sie die heruntergeladene Datei, und erstellen Sie eine VM.

1. Klicken Sie in der vSphere-Clientkonsole auf **Datei** > **Deploy OVF Template** (OVF-Vorlage bereitstellen).

    ![Bereitstellen der OVF-Vorlage](./media/tutorial-assess-vmware/deploy-ovf.png)

2. Geben Sie im Assistenten zum Bereitstellen der OVF-Vorlage unter **Quelle** den Speicherort der OVA-Datei an.
3. Geben Sie unter **Name** und **Standort** einen Anzeigenamen für die VM an. Wählen Sie das Inventarobjekt aus, in dem die VM gehostet wird.
5. Geben Sie unter **Host/Cluster** den Host oder Cluster an, auf bzw. in dem die VM ausgeführt wird.
6. Geben Sie unter **Speicher** das Speicherziel für die VM an.
7. Geben Sie unter **Datenträgerformat** den Typ und die Größe des Datenträgers an.
8. Geben Sie unter **Netzwerkzuordnung** das Netzwerk an, mit dem die VM eine Verbindung herstellt. Das Netzwerk muss über eine Internetverbindung verfügen, um Metadaten an die Azure Migrate-Serverbewertung senden zu können.
9. Überprüfen Sie die Einstellungen, und klicken Sie dann auf **Fertig stellen**.


### <a name="verify-appliance-access-to-azure"></a>Überprüfen des Appliancezugriffs auf Azure

Vergewissern Sie sich, dass die Appliance-VM eine Verbindung mit [Azure-URLs](migrate-support-matrix-vmware.md#assessment-url-access-requirements) herstellen kann.


### <a name="configure-the-appliance"></a>Konfigurieren der Appliance

Richten Sie die Appliance wie folgt ein:

1. Klicken Sie in der vSphere-Clientkonsole mit der rechten Maustaste auf den virtuellen Computer, und wählen Sie **Konsole öffnen** aus.
2. Geben Sie die Sprache, die Zeitzone und das Kennwort für die Appliance an.
3. Öffnen Sie in einem Browser auf einem beliebigen Computer, der eine Verbindung mit der VM herstellen kann, die URL der Appliance-Web-App: **https://*Name oder IP-Adresse der Appliance*: 44368**.

   Alternativ können Sie auch auf dem Appliancedesktop auf die App-Verknüpfung klicken, um die App zu öffnen.
4. Gehen Sie in der Web-App unter **Erforderliche Komponenten einrichten** wie folgt vor:
    - **Lizenz**: Akzeptieren Sie die Lizenzbedingungen, und lesen Sie die Drittanbieterinformationen.
    - **Konnektivität**: Die App überprüft, ob die VM über Internetzugriff verfügt. Sollte die VM einen Proxy verwenden, gehen Sie wie folgt vor:
        - Klicken Sie auf **Proxyeinstellungen**, und geben Sie die Proxyadresse und den Lauschport an (im Format http://ProxyIPAddress oder http://ProxyFQDN ).
        - Geben Sie die Anmeldeinformationen an, wenn der Proxy eine Authentifizierung erfordert.
        - Es werden nur HTTP-Proxys unterstützt.
    - **Uhrzeitsynchronisierung**: Die Uhrzeit der Appliance muss mit der Internetzeit synchronisiert werden, damit die Ermittlung ordnungsgemäß funktioniert.
    - **Updates installieren**: Die Appliance stellt sicher, dass die neuesten Updates installiert sind.
    - **Install VDDK** (VDDK installieren): Die Appliance überprüft, ob das VMware vSphere-VDDK (Virtual Disk Development Kit) installiert ist.
        - Azure Migrate: Server Migration verwendet das VDDK, um Computer bei der Migration zu Azure zu replizieren.
        - Laden Sie VDDK 6.7 von VMware herunter, und extrahieren Sie den Inhalt der heruntergeladenen ZIP-Datei am angegebenen Ort auf der Appliance.

### <a name="register-the-appliance-with-azure-migrate"></a>Registrieren der Appliance bei Azure Migrate

1. Klicken Sie auf **Anmelden**. Sollte keine Anmeldung angezeigt werden, vergewissern Sie sich, dass Sie den Popupblocker im Browser deaktiviert haben.
2. Melden Sie sich auf der neuen Registerkarte mit Ihren Azure-Anmeldeinformationen an.
    - Melden Sie sich mit Ihrem Benutzernamen und Ihrem Kennwort an.
    - Die Anmeldung mit einer PIN wird nicht unterstützt.
3. Kehren Sie nach erfolgreicher Anmeldung zur Web-App zurück.
2. Wählen Sie das Abonnement aus, in dem das Azure Migrate-Projekt erstellt wurde, und wählen Sie anschließend das Projekt aus.
3. Geben Sie einen Namen für die Appliance an. Für den Namen können bis zu 14 alphanumerische Zeichen angegeben werden.
4. Klicken Sie auf **Registrieren**.

## <a name="start-continuous-discovery"></a>Starten der kontinuierlichen Ermittlung

Die Appliance muss eine Verbindung mit der vCenter Server-Instanz herstellen, um die Konfigurations- und Leistungsdaten der VMs zu ermitteln.

### <a name="specify-vcenter-server-details"></a>vCenter Server-Details angeben
1. Geben Sie unter **vCenter Server-Details angeben** den Namen (FQDN) oder die IP-Adresse der vCenter Server-Instanz an. Sie können den Standardport beibehalten oder einen benutzerdefinierten Port angeben, an dem Ihre vCenter Server-Instanz lauscht.
2. Geben Sie unter **Benutzername** und **Kennwort** die Anmeldeinformationen für das schreibgeschützte Konto an, über das die Appliance VMs in der vCenter Server-Instanz ermittelt. Vergewissern Sie sich, dass das Konto über die [erforderlichen Berechtigungen für die Ermittlung](migrate-support-matrix-vmware.md#assessment-vcenter-server-permissions) verfügt. Sie können das Ermittlungsspektrum festlegen, indem Sie den Zugriff für das vCenter-Konto entsprechend beschränken. Weitere Informationen zur Beschränkung der Ermittlung finden Sie [hier](tutorial-assess-vmware.md#scoping-discovery).
3. Vergewissern Sie sich durch Klicken auf **Verbindung überprüfen**, dass die Appliance eine Verbindung mit vCenter Server herstellen kann.

### <a name="specify-vm-credentials"></a>Angeben der VM-Anmeldeinformationen
Zum Ermitteln von Anwendungen, Rollen und Features sowie zum Visualisieren von Abhängigkeiten der virtuellen Computer können Sie VM-Anmeldeinformationen bereitstellen, die Zugriff auf die virtuellen VMware-Computer ermöglichen. Sie können Anmeldeinformationen für virtuelle Windows-Computer und Anmeldeinformationen für virtuelle Linux-Computer hinzufügen. [Weitere Informationen](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware#assessment-vcenter-server-permissions) zu erforderlichen Zugriffsberechtigungen.

> [!NOTE]
> Diese Eingabe ist optional und wird benötigt, um die Anwendungsermittlung und die Visualisierung von Abhängigkeiten ohne Agent zu aktivieren.

1. Klicken Sie unter **Anwendungen und Abhängigkeiten auf VMs ermitteln** auf **Anmeldeinformationen hinzufügen**.
2. Wählen Sie das **Betriebssystem** aus.
3. Geben Sie einen Anzeigenamen für die Anmeldeinformationen an.
4. Geben Sie unter **Benutzername** und **Kennwort** ein Konto an, das mindestens über den Gastzugriff auf die virtuellen Computer verfügt.
5. Klicken Sie auf **Hinzufügen**.

Nachdem Sie die vCenter Server- und VM-Anmeldeinformationen angegeben haben (optional), klicken Sie auf **Speichern und Ermittlung starten**, um die Ermittlung der lokalen Umgebung zu starten.

Es dauert etwa 15 Minuten, bis Metadaten von ermittelten VMs im Portal angezeigt werden. Die Ermittlung von installierten Anwendungen, Rollen und Features dauert einige Zeit. Die Dauer hängt von der Anzahl der ermittelten VMS ab. Bei 500 VMs dauert es ungefähr 1 Stunde, bis der Anwendungsbestand im Azure Migrate-Portal angezeigt wird.

### <a name="scoping-discovery"></a>Beschränken der Ermittlung

Sie können das Ermittlungsspektrum festlegen, indem Sie den Zugriff des für die Ermittlung verwendeten vCenter-Kontos beschränken. Sie können das Spektrum auf vCenter Server-Datencenter, Cluster, Clusterordner, Hosts, Hostordner oder einzelne VMs festlegen.

Zum Festlegen des Bereichs müssen Sie die folgenden Schritte ausführen:
1.  Erstellen eines vCenter-Benutzerkontos
2.  Definieren einer neuen Rolle mit den erforderlichen Berechtigungen (<em>für Servermigration ohne Agent erforderlich</em>)
3.  Zuweisen von Berechtigungen zum Benutzerkonto auf vCenter-Objekten

**Erstellen eines vCenter-Benutzerkontos**
1.  Melden Sie sich als vCenter Server-Administrator am vSphere-Webclient an.
2.  Klicken Sie auf **Verwaltung** > **SSO Users and Groups** (SSO-Benutzer und -Gruppen) > Registerkarte **Benutzer**.
3.  Klicken Sie auf das Symbol **Neuer Benutzer**.
4.  Geben Sie die erforderlichen Informationen für die Erstellung des neuen Benutzers ein, und klicken Sie auf **OK**.

**Definieren einer neuen Rolle mit den erforderlichen Berechtigungen** (<em>für Servermigration ohne Agent erforderlich</em>)
1.  Melden Sie sich als vCenter Server-Administrator am vSphere-Webclient an.
2.  Navigieren Sie zu **Verwaltung** > **Rollen-Manager**.
3.  Wählen Sie im Dropdownmenü Ihre vCenter Server-Instanz aus.
4.  Klicken Sie auf die Aktion **Rolle erstellen**.
5.  Geben Sie einen Namen für die neue Rolle ein. (Beispiel: <em>Azure_Migrate</em>).
6.  Weisen Sie der neu definierten Rolle [diese Berechtigungen](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware#agentless-migration-vcenter-server-permissions) zu.
7.  Klicken Sie auf **OK**.

**Zuweisen von Berechtigungen für vCenter-Objekte**

Es gibt zwei Ansätze, um Berechtigungen in Inventarobjekten in vCenter dem vCenter-Benutzerkonto mit einer Rolle zuzuweisen.
- Für die Serverbewertung muss auf das vCenter-Benutzerkonto eine Rolle vom Typ **Schreibgeschützt** für alle übergeordneten Objekte angewendet werden, auf denen die zu ermittelnden VMs gehostet werden. Alle übergeordneten Objekte – Host, Hostordner, Cluster, Clusterordner – in der Hierarchie bis hinauf zum Rechenzentrum müssen eingebunden werden. Diese Berechtigungen müssen an die untergeordneten Objekte in der Hierarchie weitergegeben werden.

    Bei der Servermigration muss entsprechend eine benutzerdefinierte Rolle (kann den Namen <em>Azure_Migrate</em> haben) mit diesen zugewiesenen [Berechtigungen](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware#agentless-migration-vcenter-server-permissions) auf das vCenter-Benutzerkonto für alle übergeordneten Objekte angewendet werden, auf denen die zu migrierenden VMs gehostet werden.

![Zuweisen von Berechtigungen](./media/tutorial-assess-vmware/assign-perms.png)

- Die alternative Vorgehensweise besteht darin, das Benutzerkonto und die Rolle auf Rechenzentrumsebene zuzuweisen und diese an die untergeordneten Objekte weiterzugeben. Weisen Sie dem Konto dann eine Rolle vom Typ **Kein Zugriff** für jedes Objekt zu (z. B. VMs), das Sie nicht ermitteln bzw. migrieren möchten. Diese Konfiguration ist mühsam. Dabei besteht das Risiko einer versehentlichen Preisgabe von Zugangsdaten, da jedem neuen untergeordneten Objekt auch automatisch die vom übergeordneten Objekt geerbten Zugangsrechte erteilt werden. Aus diesem Grund wird empfohlen, die erste Methode zu verwenden.

> [!NOTE]
> Die Serverbewertung kann aktuell keine VMs ermitteln, wenn dem vCenter-Konto Zugriff auf der vCenter-VM-Ordnerebene gewährt wurde. Wenn Ihre Ermittlung auf VM-Ordnern basieren soll, muss das vCenter-Konto reinen Lesezugriff auf der VM-Ebene haben.  Im Anschluss finden Sie eine entsprechende Anleitung hierfür:
>
> 1. Weisen Sie reine Leseberechtigungen für alle VMs in den VM-Ordnern zu, auf denen die Ermittlung basieren soll.
> 2. Erteilen Sie überall dort, wo die VMs gehostet werden, reinen Lesezugriff auf alle übergeordneten Objekte. Alle übergeordneten Objekte – Host, Hostordner, Cluster, Clusterordner – in der Hierarchie bis hinauf zum Rechenzentrum müssen eingeschlossen werden. Sie brauchen die Berechtigungen nicht an alle untergeordneten Objekte weiterzugeben.
> 3. Verwenden Sie die Anmeldeinformationen für die Ermittlung, und wählen Sie das Rechenzentrum als *Sammlungsbereich* aus. Durch die eingerichtete rollenbasierte Zugriffssteuerung wird sichergestellt, dass der entsprechende vCenter-Benutzer nur Zugriff auf die mandantenspezifischen VMs hat.
>
> Beachten Sie, dass Host- und Clusterordner unterstützt werden.

### <a name="verify-vms-in-the-portal"></a>Überprüfen virtueller Computer im Portal

Nach der Ermittlung können Sie überprüfen, ob die VMs im Azure-Portal angezeigt werden.

1. Öffnen Sie das Azure Migrate-Dashboard.
2. Klicken Sie unter **Azure Migrate – Server** > **Azure Migrate: Server Assessment** (Azure Migrate-Serverbewertung) auf das Symbol mit der Anzahl für **Ermittelte Server**.

## <a name="set-up-an-assessment"></a>Einrichten einer Bewertung

Es gibt zwei Arten von Bewertungen, die Sie mit der Azure Migrate-Serverbewertung erstellen können:

**Bewertung** | **Details** | **Daten**
--- | --- | ---
**Leistungsbasiert** | Bewertungen basierend auf gesammelten Leistungsdaten | **Empfohlene VM-Größe**: Basierend auf CPU- und Arbeitsspeicher-Nutzungsdaten<br/><br/> **Empfohlener Datenträgertyp (Verwalteter Datenträger vom Typ Standard oder Premium)** : Basierend auf IOPS und Durchsatz der lokalen Datenträger
**Wie lokal** | Bewertungen basierend auf lokaler Größenanpassung | **Empfohlene VM-Größe**: Basierend auf der Größe der lokalen VM<br/><br> **Empfohlener Datenträgertyp**: Basierend auf der für die Bewertung ausgewählten Speichertypeinstellung


### <a name="run-an-assessment"></a>Durchführen einer Bewertung

Führen Sie eine Bewertung wie folgt aus:

1. Machen Sie sich mit den [bewährten Methoden](best-practices-assessment.md) für die Bewertungserstellung vertraut.
2. Klicken Sie auf der Registerkarte **Server** unter der Kachel **Azure Migrate: Server Assessment** (Azure Migrate-Serverbewertung) auf **Bewerten**.

    ![Bewerten](./media/tutorial-assess-vmware/assess.png)

2. Geben Sie unter **Server bewerten** einen Namen für die Bewertung an.
3. Klicken Sie auf **Alle anzeigen**, um die Eigenschaften für die Bewertung zu überprüfen.

    ![Bewertungseigenschaften](./media/tutorial-assess-vmware/view-all.png)

3. Wählen Sie unter **Gruppe auswählen oder erstellen** die Option **Neu erstellen** aus, und geben Sie einen Namen für die Gruppe ein. Eine Gruppe enthält mindestens eine zu bewertende VM.
4. Wählen Sie unter **Computer zur Gruppe hinzufügen** die VMs aus, die der Gruppe hinzugefügt werden sollen.
5. Klicken Sie auf **Bewertung erstellen**, um die Gruppe zu erstellen und die Bewertung auszuführen.

    ![Erstellen einer Bewertung](./media/tutorial-assess-vmware/assessment-create.png)

6. Zeigen Sie die erstellte Bewertung unter **Server** > **Azure Migrate: Server Assessment** (Azure Migrate-Serverbewertung) > **Bewertungen** an.
7. Klicken Sie auf **Bewertung exportieren**, um sie als Excel-Datei herunterzuladen.



## <a name="review-an-assessment"></a>Überprüfen einer Bewertung

Eine Bewertung beschreibt Folgendes:

- **Azure-Bereitschaft**: Gibt an, ob VMs für die Migration zu Azure geeignet sind.
- **Geschätzte monatliche Kosten**: Die geschätzten monatlichen Compute- und Speicherkosten für die Ausführung der VMs in Azure.
- **Geschätzte monatliche Speicherkosten**: Die geschätzten Kosten für den Datenträgerspeicher nach der Migration.

### <a name="view-an-assessment"></a>Anzeigen einer Bewertung

1. Klicken Sie unter **Migrationsziele** >  **Server** auf **Bewertungen** (unter **Azure Migrate: Serverbewertung** aus.
2. Klicken Sie unter **Bewertungen** auf eine Bewertung, um sie zu öffnen.

    ![Zusammenfassung der Bewertung](./media/tutorial-assess-vmware/assessment-summary.png)

### <a name="review-azure-readiness"></a>Überprüfen der Azure-Bereitschaft

1. Überprüfen Sie unter **Azure-Bereitschaft**, ob VMs für die Migration zu Azure bereit sind.
2. Überprüfen Sie den VM-Status:
    - **Bereit für Azure**: Azure Migrate empfiehlt in der Bewertung eine VM-Größe und gibt Kostenschätzungen für VMs ab.
    - **Bereit mit Bedingungen**: Zeigt Probleme und eine vorgeschlagene Abhilfe an.
    - **Nicht bereit für Azure**: Zeigt Probleme und eine vorgeschlagene Abhilfe an.
    - **Bereitschaft unbekannt**: Wird verwendet, wenn Azure Migrate die Bereitschaft aufgrund von Problemen mit der Datenverfügbarkeit nicht bewerten kann.

2. Klicken Sie auf einen **Azure-Bereitschaftsstatus**. Sie können Details zur VM-Bereitschaft sowie VM-Details wie Compute-, Speicher- und Netzwerkeinstellungen anzeigen.



### <a name="review-cost-details"></a>Überprüfen der Kostendetails

In dieser Ansicht werden die geschätzten Compute- und Speicherkosten für die Ausführung der VMs in Azure angezeigt.

1. Überprüfen Sie die monatlichen Compute- und Speicherkosten. Die Kosten für alle VMs in der bewerteten Gruppe werden aggregiert.

    - Kostenschätzungen basieren auf den Größenempfehlungen für einen Computer sowie auf seinen Datenträgern und Eigenschaften.
    - Die geschätzten monatlichen Kosten für Compute und Speicher werden angezeigt.
    - Die Kostenschätzung gilt für die Ausführung der lokalen VMs als IaaS-VMs. PaaS- oder SaaS-Kosten werden von der Azure Migrate-Serverbewertung nicht berücksichtigt.

2. Sie können die geschätzten monatlichen Speicherkosten überprüfen. Diese Ansicht zeigt aggregierte Speicherkosten für die bewertete Gruppe, aufgeschlüsselt nach verschiedenen Arten von Speicherdatenträgern.
3. Sie können einen Drilldown ausführen, um die Details für bestimmte VMs anzuzeigen.


### <a name="review-confidence-rating"></a>Prüfen der Zuverlässigkeitsstufe

Wenn Sie leistungsbasierte Bewertungen ausführen, wird der Bewertung eine Zuverlässigkeitsstufe zugewiesen.

![Zuverlässigkeitsstufe](./media/tutorial-assess-vmware/confidence-rating.png)

- Es wird eine Bewertung zwischen einem Stern (niedrigste Stufe) und fünf Sternen (höchste Stufe) vergeben.
- Anhand der Zuverlässigkeitsstufe können Sie die Zuverlässigkeit der von der Bewertung bereitgestellten Größenempfehlungen besser einschätzen.
- Die Zuverlässigkeitsstufe basiert auf der Verfügbarkeit von Datenpunkten, die zum Berechnen der Bewertung erforderlich sind.

Die Zuverlässigkeitsstufen für eine Bewertung sehen wie folgt aus:

**Verfügbarkeit von Datenpunkten** | **Zuverlässigkeitsstufe**
--- | ---
0 % bis 20 % | Ein Stern
21 % bis 40 % | Zwei Sterne
41 % bis 60 % | Drei Sterne
61 % bis 80 % | Vier Sterne
81 % bis 100 % | Fünf Sterne

Weitere Informationen zu bewährten Methoden für Zuverlässigkeitsstufen finden Sie [hier](best-practices-assessment.md#best-practices-for-confidence-ratings).


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial führen Sie Folgendes durch:

> [!div class="checklist"]
> * Einrichten einer Azure Migrate-Appliance
> * Erstellen und Überprüfen einer Bewertung

Im dritten Tutorial der Reihe erfahren Sie, wie Sie VMware-VMs mithilfe der Azure Migrate-Servermigration zu Azure migrieren.

> [!div class="nextstepaction"]
> [Migrieren von VMware-VMs zu Azure (ohne Agent)](./tutorial-migrate-vmware.md)
