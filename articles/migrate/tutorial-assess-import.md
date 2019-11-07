---
title: Bewerten von Servern anhand von importierten Serverdaten mit der Azure Migrate-Serverbewertung
description: Es wird beschrieben, wie Sie lokale Server für die Migration zu Azure bewerten, indem Sie die Azure Migrate-Serverbewertung und importierte Daten verwenden.
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 10/23/2019
ms.author: raynew
ms.openlocfilehash: 86d61dbd75363f68ee4651bfb36f940810909c00
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73509944"
---
# <a name="assess-servers-using-imported-data"></a>Bewerten von Servern mit importierten Daten

In diesem Artikel wird beschrieben, wie Sie lokale Server mit der [ Azure Migrate-Serverbewertung](migrate-services-overview.md#azure-migrate-server-assessment-tool) bewerten, indem Sie Servermetadaten im CSV-Format importieren. Mit dieser Bewertungsmethode müssen Sie keine Azure Migrate-Appliance einrichten, um eine Bewertung zu erstellen. Dies ist in folgenden Situationen nützlich: 

- Sie möchten schnell eine anfängliche Bewertung erstellen, bevor Sie die Appliance bereitstellen.
- Sie können die Azure Migrate-Appliance nicht in Ihrer Organisation bereitstellen.
- Sie können keine Anmeldeinformationen freigeben, die den Zugriff auf lokale Server ermöglichen.
- Aufgrund von Sicherheitsbeschränkungen wird verhindert, dass Sie von der Appliance erfasste Daten sammeln und an Azure senden. Mit einer importierten Datei können Sie die von Ihnen freigegebenen Daten steuern, und viele Daten sind optional (z. B. die Bereitstellung von IP-Adressen).


## <a name="before-you-start"></a>Vorbereitung

Beachten Sie Folgendes:

- Sie können in einer einzelnen CSV-Datei maximal 20.000 Server hinzufügen.
- Sie können in einem Azure Migrate-Projekt per CSV-Datei maximal 20.000 Server hinzufügen.
- Es ist möglich, Serverinformationen per CSV-Datei mehrfach für die Azure Migrate-Serverbewertung hochzuladen.
- Das Sammeln von Anwendungsinformationen ist nützlich, wenn Sie Ihre lokale Umgebung für die Migration auswerten. Mit der Azure Migrate-Serverbewertung werden derzeit aber keine Bewertungen auf Anwendungsebene durchgeführt, und beim Erstellen einer Bewertung werden keine Anwendungen berücksichtigt.

In diesem Tutorial lernen Sie Folgendes:
> [!div class="checklist"]
> * Einrichten eines Azure Migrate-Projekts
> * Fügen Sie die Serverinformationen in eine CSV-Datei ein.
> * Importieren Sie die Datei, um der Azure Migrate-Serverbewertung Serverinformationen hinzuzufügen.
> * Erstellen und überprüfen Sie eine Bewertung.

> [!NOTE]
> In den Tutorials wird der einfachste Bereitstellungspfad für ein Szenario erläutert, damit Sie schnell einen Proof of Concept einrichten können. Die Tutorials verwenden nach Möglichkeit Standardoptionen und zeigen nicht alle möglichen Einstellungen und Pfade. Ausführliche Anweisungen finden Sie in den Anleitungsartikeln.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) erstellen, bevor Sie beginnen.


## <a name="set-azure-permissions-for-azure-migrate"></a>Festlegen von Azure-Berechtigungen für Azure Migrate 

Ihr Azure-Konto benötigt Berechtigungen zum Erstellen eines Azure Migrate-Projekts.

1. Öffnen Sie im Azure-Portal das Abonnement, und wählen Sie **Zugriffssteuerung (IAM)** aus.
2. Suchen Sie unter **Zugriff überprüfen** das relevante Konto, und klicken Sie darauf, um Berechtigungen anzuzeigen.
3. Sie sollten über die Berechtigung **Mitwirkender** oder **Besitzer** verfügen.
    - Wenn Sie gerade erst ein kostenloses Azure-Konto erstellt haben, sind Sie der Besitzer Ihres Abonnements.
    - Wenn Sie nicht der Besitzer des Abonnements sind, müssen Sie mit dem Besitzer zusammenarbeiten, um die Rolle zuzuweisen.


## <a name="set-up-an-azure-migrate-project"></a>Einrichten eines Azure Migrate-Projekts

Richten Sie wie folgt ein neues Azure Migrate-Projekt ein:

1. Wählen Sie im Azure-Portal **Alle Dienste** aus, und suchen Sie nach **Azure Migrate**.
2. Wählen Sie unter **Dienste** die Option **Azure Migrate** aus.
3. Klicken Sie in der **Übersicht** unter **Server ermitteln, bewerten und migrieren** auf **Server bewerten und migrieren**.

    ![Ermitteln und Bewerten von Servern](./media/tutorial-assess-import/assess-migrate.png)

4. Klicken Sie unter **Erste Schritte** auf **Tools hinzufügen**.
5. Wählen Sie unter **Projekt migrieren** Ihr Azure-Abonnement aus, und erstellen Sie bei Bedarf eine Ressourcengruppe.     
6. Geben Sie unter **Projektdetails** den Projektnamen und die geografische Region an, in der Sie das Projekt erstellen möchten.

    - [Überprüfen](migrate-support-matrix.md#supported-geographies) Sie die unterstützten geografischen Regionen. Die geografische Region des Projekts dient nur zum Speichern der Metadaten, die von den lokalen VMs erfasst werden.
    - Beim Ausführen einer Migration kann eine beliebige Zielregion ausgewählt werden.

    ![Erstellen eines Azure Migrate-Projekts](./media/tutorial-assess-import/migrate-project.png)


7. Klicken Sie auf **Weiter**.
8. Wählen Sie unter **Bewertungstool auswählen** Folgendes aus: **Azure Migrate: Server Assessment** (Azure Migrate-Serverbewertung) > **Weiter**.

    ![Erstellen eines Azure Migrate-Projekts](./media/tutorial-assess-import/assessment-tool.png)

9. Wählen Sie unter **Migrationstool auswählen** die Option **Hinzufügen eines Migrationstools vorerst überspringen** und anschließend **Weiter** aus.
10. Überprüfen Sie die Einstellungen unter **Review + add tools** (Überprüfen und Tools hinzufügen), und klicken Sie auf **Tools hinzufügen**.
11. Warten Sie einige Minuten, bis das Azure Migrate-Projekt bereitgestellt wurde. Sie werden zur Projektseite weitergeleitet. Sollte das Projekt nicht angezeigt werden, können Sie auf dem Azure Migrate-Dashboard unter **Server** darauf zugreifen.


## <a name="prepare-the-csv"></a>Vorbereiten der CSV-Datei

Laden Sie die CSV-Vorlage herunter, und fügen Sie ihr Serverinformationen hinzu.


### <a name="download-the-template"></a>Herunterladen der Vorlage

1. Klicken Sie unter **Migrationsziele** > **Server** > **Azure Migrate: Server Assessment** (Azure Migrate-Serverbewertung) auf **Ermitteln**.
2. Wählen Sie unter **Computer ermitteln** die Option **Über CSV importieren** aus.
3. Klicken Sie auf **Herunterladen**, um die CSV-Vorlage herunterzuladen. Alternativ können Sie den [Download direkt durchführen](https://go.microsoft.com/fwlink/?linkid=2108404).

    ![Herunterladen der CSV-Vorlage](./media/tutorial-assess-import/download-template.png)


### <a name="add-server-information"></a>Hinzufügen von Serverinformationen

Sammeln Sie Serverdaten, und fügen Sie sie der CSV-Datei hinzu.

- Zum Sammeln von Daten können Sie diese aus Tools exportieren, die Sie für die lokale Serververwaltung nutzen, z. B. VMware vSphere oder Ihre Konfigurationsverwaltungsdatenbank (CMDB).
- Laden Sie unsere [Beispieldatei](https://go.microsoft.com/fwlink/?linkid=2108405) herunter, um sich die Beispieldaten anzusehen.


In der folgenden Tabelle sind die Felder zusammengefasst, die ausgefüllt werden müssen.

**Feldname** | **Obligatorisch** | **Details**
--- | --- | ---
**Servername** | Ja | Wir empfehlen Ihnen, den vollqualifizierten Domänennamen (FQDN) anzugeben. 
**IP-Adresse** | Nein | Serveradresse
**Number of cores** (Anzahl von Kernen) | Ja | Die Anzahl von Prozessorkernen, die dem Server zugeordnet sind.
**Memory** | Ja | Gesamter Arbeitsspeicher (MB), der dem Server zugeordnet ist.
**OS name** (Betriebssystemname) | Ja | Serverbetriebssystem
**Betriebssystemversion** | Nein | Betriebssystemversion des Servers
**Anzahl der Datenträger** | Nein | Nicht erforderlich, wenn Details zu den einzelnen Datenträgern angegeben werden.
**Disk 1 size** (Größe von Datenträger 1)  | Nein | Maximale Größe des Datenträgers (GB)<br/> Sie können Details zu weiteren Datenträgern hinzufügen, indem Sie in der Vorlage [Spalten hinzufügen](#add-multiple-disks). Es ist möglich, bis zu acht Datenträger hinzuzufügen.
**Disk 1 read ops** (Lesevorgänge für Datenträger 1) | Nein | Datenträger-Lesevorgänge pro Sekunde
**Disk 1 write ops** (Schreibvorgänge für Datenträger 1) | Nein | Datenträger-Schreibvorgänge pro Sekunde
**Disk 1 read throughput** (Lesedurchsatz für Datenträger 1) | Nein | Vom Datenträger pro Sekunde gelesene Daten (in MB pro Sekunde)
**Disk 1 write throughput** (Schreibdurchsatz für Datenträger 1) | Nein | Daten, die pro Sekunde auf den Datenträger geschrieben werden (in MB pro Sekunde)
**Prozentsatz der CPU-Auslastung** | Nein | Prozentuale Auslastung der CPU
**Memory utilization percentage** (Prozentuale Auslastung des Arbeitsspeichers) | Nein | Prozentuale RAM-Auslastung
**Total disks read ops** (Gesamte Lesevorgänge für Datenträger) | Nein | Datenträger-Lesevorgänge pro Sekunde
**Total disks write ops** (Gesamte Schreibvorgänge für Datenträger) | Nein | Datenträger-Schreibvorgänge pro Sekunde
**Total disks read throughput** (Gesamter Lesedurchsatz für Datenträger) | Nein | Vom Datenträger gelesene Daten (in MB pro Sekunde)
**Total disks write throughput** (Gesamter Schreibdurchsatz für Datenträger) | Nein | Auf Datenträger geschriebene Daten (in MB pro Sekunden)
**Network in throughput** (Eingehender Netzwerkdurchsatz) | Nein | Vom Server empfangene Daten (in MB pro Sekunde)
**Network out throughput** (Ausgehender Netzwerkdurchsatz) | Nein | Vom Server übertragene Daten (in MB pro Sekunde)
**Firmware type** (Firmwaretyp) | Nein | Serverfirmware. Die Werte können „BIOS“ oder „UEFI“ lauten.
**Servertyp** | Nein | Die Werte können „Physical“ (Physisch) oder „Virtual“ (Virtuell) lauten.
**Hypervisor** | Nein | Hypervisor, auf dem ein Computer ausgeführt wird. <br/> Die Werte können „VMware“, „Hyper-V“, „Xen“, „AWS“, „GCP“ oder „Other“ (Sonstiges) lauten.
**Hypervisor version number** (Hypervisor-Versionsnummer) | Nein | Hypervisor-Version
**Virtual machine ID** (ID des virtuellen Computers) | Nein | VM-Bezeichner. Dies ist die **InstanceUUid** für die VMware vCenter-VM bzw. die **ID des virtuellen Hyper-V-Computers** für Hyper-V.
**Virtual machine manager ID** (ID für Virtual Machine Manager) | Nein | Dies ist die **InstanceUUid** für VMWare vCenter. Für Hyper-V nicht erforderlich.
**MAC address** (MAC-Adresse)| Nein | MAC-Adresse des Servers
**BIOS ID** (BIOS-ID) | Nein | BIOS-ID des Servers
**Custom server ID** (Benutzerdefinierte Server-ID)| Nein | Lokale eindeutige Server-IDs für die lokale Umgebung. <br/> Nützlich zum Nachverfolgen des importierten Servers anhand der lokalen ID. 
**Application 1 name** (Name von Anwendung 1) | Nein | Name der Workloads, die auf dem Server ausgeführt werden.<br/> Sie können Details zu weiteren Apps hinzufügen, indem Sie in der Vorlage [Spalten hinzufügen](#add-multiple-applications). Sie können maximal fünf Anwendungen hinzufügen.
**Application 1 type** (Typ von Anwendung 1) | Nein | Typ der Workload, die auf dem Server ausgeführt wird
**Application 1 version** (Version von Anwendung 1) | Nein | Version der Workload, die auf dem Server ausgeführt wird
**Application 1 license expiry** (Lizenzablauf von Anwendung 1) | Nein | Lizenzablauf für die Workload (falls zutreffend)
**Business unit** (Geschäftseinheit) | Nein | Geschäftseinheit, zu der der Server gehört
**Business owner** (Geschäftsinhaber) | Nein | Besitzer der Geschäftseinheit
**Business application name** (Name der Geschäftsanwendung) | Nein | Name der Anwendung, zu der die App gehört
**Location** | Nein | Datencenter, in dem sich der Server befindet
**Server decommission date** (Datum der Außerbetriebnahme des Servers) | Nein | Datum der Außerbetriebnahme des physischen Servers oder des zugrunde liegenden physischen Servers des virtuellen Servers

### <a name="add-operating-systems"></a>Hinzufügen von Betriebssystemen

Bei der Bewertung werden bestimmte Betriebssystemnamen erkannt. Alle von Ihnen angegebenen Betriebssystemnamen müssen mit einer der Optionen in der Liste mit den [unterstützten Namen](#supported-operating-system-names) übereinstimmen.


### <a name="add-multiple-disks"></a>Hinzufügen mehrerer Datenträger

Die Vorlage enthält Standardfelder für den ersten Datenträger.  Sie können ähnliche Spalten für bis zu acht Datenträger hinzufügen. 

Fügen Sie beispielsweise diese Spalten hinzu, um alle Felder für einen zweiten Datenträger anzugeben:

Disk 2 size/Disk 2 read ops/Disk 2 write ops/Disk 2 read throughput/Disk 2 write throughput (Größe von Datenträger 2/Lesevorgänge für Datenträger 2/Schreibvorgänge für Datenträger 2/Lesedurchsatz für Datenträger 2/Schreibdurchsatz für Datenträger 2)

Optional können Sie auch spezifische Felder nur für einen Datenträger hinzufügen.


### <a name="add-multiple-applications"></a>Hinzufügen mehrerer Anwendungen

Die Vorlage enthält nur die Felder für eine Anwendung. Sie können ähnliche Spalten für bis zu fünf Apps hinzufügen.  

Fügen Sie beispielsweise diese Spalten hinzu, um alle Felder für eine zweite App anzugeben:

Application 2 name/Application 2 type/Application 2 version/Application 2 license expiry (Name von Anwendung 2/Typ von Anwendung 2/Version von Anwendung 2/Lizenzablauf von Anwendung 2)


Optional können Sie auch spezifische Felder nur für eine App hinzufügen.

> [!NOTE]
> App-Informationen sind nützlich, wenn Sie Ihre lokale Umgebung für die Migration auswerten. Mit der Azure Migrate-Serverbewertung wird derzeit aber keine Bewertung auf App-Ebene durchgeführt, und beim Erstellen einer Bewertung werden keine Apps berücksichtigt.


## <a name="upload-the-server-information"></a>Hochladen der Serverinformationen

Importieren Sie nach dem Hinzufügen der Informationen zur CSV-Vorlage die Server in die Azure Migrate-Serverbewertung.

1. Navigieren Sie in Azure Migrate zu **Computer ermitteln** und dann zur ausgefüllten Vorlage.
2. Klicken Sie auf **Importieren**.
3. Der Importstatus wird angezeigt. 
    - Falls in der Statusanzeige Warnungen aufgeführt sind, können Sie diese entweder beheben oder ohne Behebung fortfahren.
    - Durch die Verbesserung der Serverinformationen gemäß den Vorschlägen in den Warnungen wird die Genauigkeit der Bewertungen verbessert.
    - Klicken Sie auf **Download warning details .CSV** (CSV mit Warnungsdetails herunterladen), um Warnungen anzuzeigen und diese zu beheben. Die CSV-Datei mit den hinzugefügten Warnungen wird heruntergeladen. Sie können die Warnungen prüfen und je nach Bedarf beheben. 
    Falls in der Statusanzeige Fehler aufgeführt werden (Importstatus **Fehler**), müssen Sie diese beheben, bevor Sie den Importvorgang fortsetzen können. Laden Sie hierzu die CSV-Datei herunter, der jetzt die Fehlerdetails hinzugefügt wurden. Überprüfen und beheben Sie die Fehler je nach Bedarf. Laden Sie anschließend die geänderte Datei erneut hoch.
4. Wenn der Importstatus **Abgeschlossen** lautet, wurden die Serverinformationen importiert.


> [!NOTE]
> Laden Sie zum Aktualisieren der Serverinformationen, die in Azure Migrate hochgeladen wurden, die Daten für den Server erneut hoch, indem Sie den gleichen **Servernamen** verwenden. Beachten Sie, dass Sie das Feld **Servername** nicht mehr ändern können, nachdem die Vorlage importiert wurde. Das Löschen von Servern wird derzeit nicht unterstützt.

## <a name="updating-server-information"></a>Aktualisieren von Serverinformationen

Sie können Serverinformationen aktualisieren, indem Sie die Daten für den Server unter dem gleichen **Servernamen** erneut hochladen. Es ist nicht möglich, das Feld **Servername** zu ändern. 

Das Löschen von Servern wird derzeit nicht unterstützt.

### <a name="verify-servers-in-the-portal"></a>Überprüfen von Servern im Portal

Nach der Ermittlung können Sie überprüfen, ob die Server im Azure-Portal angezeigt werden.

1. Öffnen Sie das Azure Migrate-Dashboard.
2. Klicken Sie unter **Azure Migrate – Server** > **Azure Migrate: Server Assessment** (Azure Migrate-Serverbewertung) auf das Symbol mit der Anzahl für **Ermittelte Server**.
3. Klicken Sie auf die Registerkarte **Importbasiert**.

## <a name="set-up-an-assessment"></a>Einrichten einer Bewertung

Es gibt zwei Arten von Bewertungen, die Sie mit der Azure Migrate-Serverbewertung erstellen können:

**Bewertung** | **Details** | **Daten**
--- | --- | ---
**Leistungsbasiert** | Bewertungen anhand von angegebenen Leistungsdatenwerten | **Empfohlene VM-Größe**: Basierend auf CPU- und Arbeitsspeicher-Nutzungsdaten<br/><br/> **Empfohlener Datenträgertyp (Verwalteter Datenträger vom Typ Standard oder Premium)** : Basierend auf IOPS und Durchsatz der lokalen Datenträger
**Wie lokal** | Bewertungen basierend auf lokaler Größenanpassung | **Empfohlene VM-Größe**: Basierend auf der angegebenen Servergröße<br/><br> **Empfohlener Datenträgertyp**: Basierend auf der für die Bewertung ausgewählten Speichertypeinstellung


### <a name="run-an-assessment"></a>Durchführen einer Bewertung

Führen Sie eine Bewertung wie folgt aus:

1. Machen Sie sich mit den [bewährten Methoden](best-practices-assessment.md) für die Bewertungserstellung vertraut.
2. Klicken Sie auf der Registerkarte **Server** unter der Kachel **Azure Migrate: Server Assessment** (Azure Migrate-Serverbewertung) auf **Bewerten**.

    ![Bewerten](./media/tutorial-assess-physical/assess.png)

2. Geben Sie unter **Server bewerten** einen Namen für die Bewertung an.
3. Wählen Sie unter **Ermittlungsquelle** die Option **Machines added via import to Azure Migrate** (Per Azure Migrate-Import hinzugefügte Computer) aus.
3. Klicken Sie auf **Alle anzeigen**, um die Eigenschaften für die Bewertung zu überprüfen.

    ![Bewertungseigenschaften](./media/tutorial-assess-physical/view-all.png)

3. Wählen Sie unter **Gruppe auswählen oder erstellen** die Option **Neu erstellen** aus, und geben Sie einen Namen für die Gruppe ein. Eine Gruppe enthält mindestens eine zu bewertende VM.
4. Wählen Sie unter **Computer zur Gruppe hinzufügen** die Server aus, die der Gruppe hinzugefügt werden sollen.
5. Klicken Sie auf **Bewertung erstellen**, um die Gruppe zu erstellen und die Bewertung auszuführen.

    ![Erstellen einer Bewertung](./media/tutorial-assess-physical/assessment-create.png)

6. Zeigen Sie die erstellte Bewertung unter **Server** > **Azure Migrate: Server Assessment** (Azure Migrate-Serverbewertung) > **Bewertungen** an.
7. Klicken Sie auf **Bewertung exportieren**, um sie als Excel-Datei herunterzuladen.



## <a name="review-an-assessment"></a>Überprüfen einer Bewertung

Eine Bewertung beschreibt Folgendes:

- **Azure-Bereitschaft**: Gibt an, ob Server für die Migration zu Azure geeignet sind.
- **Geschätzte monatliche Kosten**: Die geschätzten monatlichen Compute- und Speicherkosten für die Ausführung der Server in Azure.
- **Geschätzte monatliche Speicherkosten**: Die geschätzten Kosten für den Datenträgerspeicher nach der Migration.

### <a name="view-an-assessment"></a>Anzeigen einer Bewertung

1. Klicken Sie unter **Migrationsziele** >  **Server** auf **Bewertungen** (unter **Azure Migrate: Serverbewertung** aus.
2. Klicken Sie unter **Bewertungen** auf eine Bewertung, um sie zu öffnen.

    ![Zusammenfassung der Bewertung](./media/tutorial-assess-physical/assessment-summary.png)

### <a name="review-azure-readiness"></a>Überprüfen der Azure-Bereitschaft

1. Überprüfen Sie unter **Azure-Bereitschaft**, ob die Server für die Migration zu Azure bereit sind.
2. Überprüfen Sie den Status:
    - **Bereit für Azure**: Azure Migrate empfiehlt in der Bewertung eine VM-Größe und gibt Kostenschätzungen für VMs ab.
    - **Bereit mit Bedingungen**: Zeigt Probleme und eine vorgeschlagene Abhilfe an.
    - **Nicht bereit für Azure**: Zeigt Probleme und eine vorgeschlagene Abhilfe an.
    - **Bereitschaft unbekannt**: Wird verwendet, wenn Azure Migrate die Bereitschaft aufgrund von Problemen mit der Datenverfügbarkeit nicht bewerten kann.

2. Klicken Sie auf einen **Azure-Bereitschaftsstatus**. Sie können Details zur Serverbereitschaft sowie Serverdetails wie Compute-, Speicher- und Netzwerkeinstellungen anzeigen.

### <a name="review-cost-details"></a>Überprüfen der Kostendetails

In dieser Ansicht werden die geschätzten Compute- und Speicherkosten für die Ausführung der VMs in Azure angezeigt.

1. Überprüfen Sie die monatlichen Compute- und Speicherkosten. Die Kosten für alle Server in der bewerteten Gruppe werden aggregiert.

    - Kostenschätzungen basieren auf den Größenempfehlungen für einen Computer sowie auf seinen Datenträgern und Eigenschaften.
    - Die geschätzten monatlichen Kosten für Compute und Speicher werden angezeigt.
    - Die Kostenschätzung gilt für die Ausführung der lokalen Server als IaaS-VMs. PaaS- oder SaaS-Kosten werden von der Azure Migrate-Serverbewertung nicht berücksichtigt.

2. Sie können die geschätzten monatlichen Speicherkosten überprüfen. Diese Ansicht zeigt aggregierte Speicherkosten für die bewertete Gruppe, aufgeschlüsselt nach verschiedenen Arten von Speicherdatenträgern.
3. Sie können einen Drilldown ausführen, um die Details für bestimmte VMs anzuzeigen.

> [!NOTE]
> Zuverlässigkeitsstufen werden keinen Bewertungen von Servern zugewiesen, die per CSV-Datei in die Azure Migrate-Serverbewertung importiert wurden.


## <a name="supported-operating-system-names"></a>Namen von unterstützten Betriebssystemen

NAME | NAME
--- | ---
**A - H** | 
Apple Mac OS X 10 | Asianux 3<br/>Asianux 4<br/>Asianux 5
CentOS<br/>CentOS 4/5 | CoreOS Linux 
Debian GNU/Linux 4<br/>Debian GNU/Linux 5<br/>Debian GNU/Linux 6<br/>Debian GNU/Linux 7<br/>Debian GNU/Linux 8 | FreeBSD 
**I - R** | 
IBM OS/2 | MS-DOS |
Novell NetWare 5<br/>Novell NetWare 6 | Oracle Linux<br/> Oracle Linux 4/5<br/>Oracle Solaris 10<br/> Oracle Solaris 11 
Red Hat Enterprise Linux 2<br/>Red Hat Enterprise Linux 3<br/>Red Hat Enterprise Linux 4<br/>Red Hat Enterprise Linux 5<br/>Red Hat Enterprise Linux 6<br/>Red Hat Enterprise Linux 7<br/>Red Hat Fedora | 
**S - T** | 
SCO OpenServer 5<br/>SCO OpenServer 6<br/>SCO UnixWare 7 | Serenity Systems eComStation 1<br/>Serenity Systems eComStation 2
Sun Microsystems Solaris 8<br/>Sun Microsystems Solaris 9 | SUSE Linux Enterprise 10<br/> SUSE Linux Enterprise 11<br/>SUSE Linux Enterprise 12<br/>SUSE Linux Enterprise 8/9<br/>SUSE Linux Enterprise 11<br/>SUSE openSUSE
**U - Z** | 
Ubuntu Linux | VMware ESXi 4<br/>VMware ESXi 5<br/>VMware ESXi 6
Windows 10<br/>Windows 2000<br/>Windows 3<br/>Windows 7<br/>Windows 8<br/>Windows 95<br/>Windows 98<br/>Windows NT<br/>Windows Server (R) 2008<br/>Windows Server 2003 | Windows Server 2008<br/>Windows Server 2008 R2<br/>Windows Server 2012<br/>Windows Server 2012 R2<br/>Windows Server 2016<br/>Windows Server 2019<br/>Windows Server Threshold<br/>Windows Vista<br/>Windows Web Server 2008 R2<br/>Windows XP Professional
    

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes durchgeführt:

> [!div class="checklist"]
> * Importieren von Servern in die Azure Migrate-Serverbewertung
> * Erstellen und Überprüfen einer Bewertung

[Stellen Sie nun eine Appliance bereit](./migrate-appliance.md), um genauere Bewertungen zu erhalten, und stellen Sie Server zu Gruppen zusammen, um per [Abhängigkeitsanalyse](./concepts-dependency-visualization.md) eingehendere Bewertungen durchzuführen.
