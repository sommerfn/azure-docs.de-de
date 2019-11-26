---
title: Bewerten von Hyper-V-VMs für die Migration zu Azure mit Azure Migrate | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie lokale Hyper-V-VMs mithilfe von Azure Migrate für die Migration zu Azure bewerten.
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 08/11/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 1ff46ec437de671d5804eeb31a5d1fb0284a8946
ms.sourcegitcommit: 5a8c65d7420daee9667660d560be9d77fa93e9c9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2019
ms.locfileid: "74123519"
---
# <a name="assess-hyper-v-vms-with-azure-migrate-server-assessment"></a>Bewerten von Hyper-V-VMs mit der Azure Migrate-Serverbewertung

In diesem Artikel erfahren Sie, wie Sie lokale Hyper-V-VMs mithilfe der Azure Migrate-Serverbewertung bewerten.

[Azure Migrate](migrate-services-overview.md) stellt einen Hub mit Tools bereit, die Ihnen dabei helfen, Apps, Infrastrukturen und Workloads zu ermitteln, zu bewerten und zu Microsoft Azure zu migrieren. Der Hub umfasst Azure Migrate-Tools sowie Angebote von unabhängigen Drittanbietern (Independent Software Vendors, ISVs).



Dieses Tutorial ist das zweite in einer Reihe zur Bewertung und Migration von Hyper-V-VMs zu Azure. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Einrichten eines Azure Migrate-Projekts
> * Einrichten und Registrieren einer Azure Migrate-Appliance
> * Starten der kontinuierlichen Ermittlung lokaler VMs
> * Gruppieren erkannter VMs und Bewerten der Gruppe
> * Überprüfen der Bewertung

> [!NOTE]
> In den Tutorials wird der einfachste Bereitstellungspfad für ein Szenario erläutert, damit Sie schnell einen Proof of Concept einrichten können. Die Tutorials verwenden nach Möglichkeit Standardoptionen und zeigen nicht alle möglichen Einstellungen und Pfade. Ausführliche Anweisungen finden Sie in den Anleitungsartikeln.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) erstellen, bevor Sie beginnen.


## <a name="prerequisites"></a>Voraussetzungen

- [Absolvieren Sie das erste Tutorial dieser Reihe.](tutorial-prepare-hyper-v.md) Andernfalls funktionieren die Anweisungen in diesem Tutorial nicht.
- Im ersten Tutorial müssen folgende Schritte ausgeführt werden:
    - [Einrichten von Azure-Berechtigungen](tutorial-prepare-hyper-v.md#prepare-azure) für Azure Migrate
    - [Vorbereiten von Hyper-V](tutorial-prepare-hyper-v.md#prepare-for-hyper-v-assessment) (Cluster, Hosts und VMs) für die Bewertung

## <a name="set-up-an-azure-migrate-project"></a>Einrichten eines Azure Migrate-Projekts

1. Wählen Sie im Azure-Portal **Alle Dienste** aus, und suchen Sie nach **Azure Migrate**.
2. Wählen Sie in den Suchergebnissen die Option **Azure Migrate** aus.
3. Klicken Sie in der **Übersicht** unter **Server ermitteln, bewerten und migrieren** auf **Server bewerten und migrieren**.

    ![Ermitteln und Bewerten von Servern](./media/tutorial-assess-hyper-v/assess-migrate.png)

4. Klicken Sie unter **Erste Schritte** auf **Tools hinzufügen**.
5. Wählen Sie auf der Registerkarte **Projekt migrieren** Ihr Azure-Abonnement aus, und erstellen Sie bei Bedarf eine Ressourcengruppe.
6. Geben Sie unter **Projektdetails** den Projektnamen und die Region an, in der Sie das Projekt erstellen möchten.


    ![Erstellen eines Azure Migrate-Projekts](./media/tutorial-assess-hyper-v/migrate-project.png)

    Azure Migrate-Projekte können in folgenden Regionen erstellt werden:

    **Geografie** | **Region**
    --- | ---
    Asien  | Asien, Südosten
    Europa | „Europa, Norden“ oder „Europa, Westen“
    Vereinigtes Königreich |  „Vereinigtes Königreich, Süden“ oder „Vereinigtes Königreich, Westen“
    USA | „USA, Osten“, „USA, Westen 2“ oder „USA, Westen-Mitte“

    - Die Projektregion dient nur zum Speichern der Metadaten, die von den lokalen VMs erfasst werden.
    - Bei der Migration der VMs kann eine andere Azure-Zielregion ausgewählt werden. Alle Azure-Regionen werden als Migrationsziel unterstützt.

7. Klicken Sie auf **Weiter**.
8. Wählen Sie unter **Bewertungstool auswählen** Folgendes aus: **Azure Migrate: Server Assessment** (Azure Migrate-Serverbewertung) > **Weiter**.

    ![Erstellen eines Azure Migrate-Projekts](./media/tutorial-assess-hyper-v/assessment-tool.png)

9. Wählen Sie unter **Migrationstool auswählen** die Option **Hinzufügen eines Migrationstools vorerst überspringen** >  und anschließend **Weiter** aus.
10. Überprüfen Sie die Einstellungen unter **Review + add tools** (Überprüfen und Tools hinzufügen), und klicken Sie auf **Tools hinzufügen**.
11. Warten Sie einige Minuten, bis das Azure Migrate-Projekt bereitgestellt wurde. Sie werden zur Projektseite weitergeleitet. Sollte das Projekt nicht angezeigt werden, können Sie auf dem Azure Migrate-Dashboard unter **Server** darauf zugreifen.




## <a name="set-up-the-appliance-vm"></a>Einrichten der Appliance-VM

Für die Azure Migrate-Serverbewertung wird eine einfache Hyper-V-VM-Appliance ausgeführt.

- Diese Appliance ermittelt VMs und sendet Meta- und Leistungsdaten zu VMs an die Azure Migrate-Serverbewertung.
- Die Einrichtung der Appliance umfasst Folgendes:
    - Herunterladen einer komprimierten Hyper-V-VHD über das Azure-Portal
    - Erstellen der Appliance und Überprüfen der Verbindungsherstellung mit der Azure Migrate-Serverbewertung
    - Durchführen der Erstkonfiguration für die Appliance und Registrieren der Appliance beim Azure Migrate-Projekt

### <a name="download-the-vhd"></a>Herunterladen der VHD

Laden Sie die gezippte VHD-Vorlage für die Appliance herunter.

1. Klicken Sie unter **Migrationsziele** > **Server** > **Azure Migrate: Server Assessment** (Azure Migrate-Serverbewertung) auf **Ermitteln**.
2. Klicken Sie unter **Computer ermitteln** > **Sind Ihre Computer virtualisiert?** auf **Ja, mit Hyper-V**.
3. Klicken Sie auf **Herunterladen**, um die VHD-Datei herunterzuladen.

    ![Herunterladen der VM](./media/tutorial-assess-hyper-v/download-appliance-hyperv.png)


### <a name="verify-security"></a>Überprüfen der Sicherheit

Vergewissern Sie sich vor der Bereitstellung, dass die gezippte Datei sicher ist.

1. Öffnen Sie auf dem Computer, auf den Sie die Datei heruntergeladen haben, ein Administratorbefehlsfenster.

2. Führen Sie den folgenden PowerShell-Befehl aus, um den Hash für die ZIP-Datei zu generieren:
    - ```C:\>Get-FileHash -Path <file_location> -Algorithm [Hashing Algorithm]```
    - Beispielverwendung: ```C:\>Get-FileHash -Path ./AzureMigrateAppliance_v1.19.06.27.zip -Algorithm SHA256```

3.  Für die Applianceversion 2.19.07.30 muss der generierte Hash den folgenden Einstellungen entsprechen:

  **Algorithmus** | **Hashwert**
  --- | ---
  MD5 | d0a68e76ea24ba4e4a494c0dab95e90e
  SHA256 | 0551221d2a9de75c352c201ccc88f7f10e87e5df1ecda42bfd4ec6c8defc57c0



### <a name="create-the-appliance-vm"></a>Erstellen der Appliance-VM

Importieren Sie die heruntergeladene Datei, und erstellen Sie die VM.

1. Laden Sie die gezippte VHD-Datei auf den Hyper-V-Host herunter, auf dem sich die Appliance-VM befinden wird, und extrahieren Sie sie anschließend.
    - Am Speicherort für das Extrahieren wird die Datei in einem Ordner namens **AzureMigrateAppliance_VersionNumber** entpackt.
    - Dieser Ordner enthält einen Unterordner, der ebenfalls **AzureMigrateAppliance_VersionNumber** heißt.
    - Dieser Unterordner enthält drei weitere Unterordner: **Momentaufnahmen** (Snapshots), **VHDs** (Virtual Hard Disks) und **Virtual Machines**.

2. Öffnen Sie den Hyper-V-Manager. Klicken Sie unter **Aktionen** auf **Virtuellen Computer importieren**.

    ![Bereitstellen der VHD](./media/tutorial-assess-hyper-v/deploy-vhd.png)

2. Klicken Sie im Assistenten zum Importieren virtueller Computer unter **Vorbereitung** auf **Weiter**.
3. Wählen Sie unter **Ordner suchen** den Ordner **Virtual Machines** aus. Klicken Sie auf **Weiter**.
1. Klicken Sie unter **Virtuellen Computer auswählen** auf **Weiter**.
2. Klicken Sie unter **Importtyp auswählen** auf **Virtuellen Computer kopieren (neue eindeutige ID erstellen)** . Klicken Sie auf **Weiter**.
3. Behalten Sie unter **Ziel auswählen** die Standardeinstellung bei. Klicken Sie auf **Weiter**.
4. Behalten Sie unter **Speicherordner** die Standardeinstellung bei. Klicken Sie auf **Weiter**.
5. Geben Sie unter **Netzwerk auswählen** den virtuellen Switch an, der von der VM verwendet wird. Der Switch benötigt Internetkonnektivität, um Daten an Azure senden zu können.
6. Überprüfen Sie die Einstellungen unter **Zusammenfassung**. Klicken Sie auf **Fertig stellen**.
7. Starten Sie die VM im Hyper-V-Manager unter **Virtuelle Computer**.


### <a name="verify-appliance-access-to-azure"></a>Überprüfen des Appliancezugriffs auf Azure

Vergewissern Sie sich, dass die Appliance-VM eine Verbindung mit [Azure-URLs](migrate-support-matrix-hyper-v.md#assessment-appliance-url-access) herstellen kann.

### <a name="configure-the-appliance"></a>Konfigurieren der Appliance

Führen Sie die Ersteinrichtung der Appliance durch.

1. Klicken Sie im Hyper-V-Manager unter **Virtuelle Computer** mit der rechten Maustaste auf die VM, und klicken Sie anschließend auf **Verbinden**.
2. Geben Sie die Sprache, die Zeitzone und das Kennwort für die Appliance an.
3. Öffnen Sie in einem Browser auf einem beliebigen Computer, der eine Verbindung mit der VM herstellen kann, die URL der Appliance-Web-App: **https://*Name oder IP-Adresse der Appliance*: 44368**.

   Alternativ können Sie auch auf dem Appliancedesktop auf die App-Verknüpfung klicken, um die App zu öffnen.
1. Gehen Sie in der Web-App unter **Erforderliche Komponenten einrichten** wie folgt vor:
    - **Lizenz**: Akzeptieren Sie die Lizenzbedingungen, und lesen Sie die Drittanbieterinformationen.
    - **Konnektivität**: Die App überprüft, ob die VM über Internetzugriff verfügt. Falls die VM einen Proxy verwendet, gehen Sie wie folgt vor:
      - Klicken Sie auf **Proxyeinstellungen**, und geben Sie die Proxyadresse und den Lauschport an (im Format http://ProxyIPAddress oder http://ProxyFQDN ).
      - Geben Sie die Anmeldeinformationen an, wenn der Proxy eine Authentifizierung erfordert.
      - Es werden nur HTTP-Proxys unterstützt.
    - **Uhrzeitsynchronisierung**: Die Uhrzeit wird überprüft. Die Uhrzeit der Appliance muss mit der Internetzeit synchronisiert werden, damit die VM-Ermittlung ordnungsgemäß funktioniert.
    - **Updates installieren**: Die Azure Migrate-Serverbewertung überprüft, ob auf der Appliance die neuesten Updates installiert sind.

### <a name="register-the-appliance-with-azure-migrate"></a>Registrieren der Appliance bei Azure Migrate

1. Klicken Sie auf **Anmelden**. Sollte keine Anmeldung angezeigt werden, vergewissern Sie sich, dass Sie den Popupblocker im Browser deaktiviert haben.
2. Melden Sie sich auf der neuen Registerkarte mit Ihren Azure-Anmeldeinformationen an.
    - Melden Sie sich mit Ihrem Benutzernamen und Ihrem Kennwort an.
    - Die Anmeldung mit einer PIN wird nicht unterstützt.
3. Kehren Sie nach erfolgreicher Anmeldung zur Web-App zurück.
4. Wählen Sie das Abonnement aus, in dem das Azure Migrate-Projekt erstellt wurde. Wählen Sie anschließend das Projekt aus.
5. Geben Sie einen Namen für die Appliance an. Für den Namen können bis zu 14 alphanumerische Zeichen angegeben werden.
6. Klicken Sie auf **Registrieren**.


### <a name="delegate-credentials-for-smb-vhds"></a>Delegieren von Anmeldeinformationen für SMB-VHDs

Wenn Sie VHDs in SMBs ausführen, müssen Sie die Delegierung von Anmeldeinformationen von der Appliance an die Hyper-V-Hosts aktivieren. Dafür ist Folgendes erforderlich:

- Sie ermöglichen es jedem Host, als Delegat für die Appliance zu fungieren. Diesen Schritte sollten Sie im vorherigen Tutorial ausgeführt haben, als Sie Hyper-V auf die Bewertung und Migration vorbereitet haben. Sie müssen CredSSP für die Hosts entweder [manuell](tutorial-prepare-hyper-v.md#enable-credssp-on-hosts) oder durch [Ausführung des Konfigurationsskripts für Hyper-V-Voraussetzungen](tutorial-prepare-hyper-v.md#hyper-v-prerequisites-configuration-script) eingerichtet haben.
- Aktivieren Sie die CredSSP-Delegierung, sodass die Azure Migrate-Appliance als Client fungieren und die Anmeldeinformationen an einen Host delegieren kann.

Gehen Sie zur Aktivierung für die Appliance wie folgt vor:

#### <a name="option-1"></a>Option 1:

Führen Sie auf der Appliance-VM den folgenden Befehl aus. „HyperVHost1“ und „HyperVHost2“ sind Beispielhostnamen.

```
Enable-WSManCredSSP -Role Client -DelegateComputer HyperVHost1.contoso.com HyperVHost2.contoso.com -Force
```

Beispiel: ` Enable-WSManCredSSP -Role Client -DelegateComputer HyperVHost1.contoso.com HyperVHost2.contoso.com -Force `

#### <a name="option-2"></a>Option 2:

Gehen Sie alternativ im Editor für lokale Gruppenrichtlinien auf der Appliance wie folgt vor:

1. Klicken Sie unter **Richtlinie für "Lokaler Computer"**  > **Computerkonfiguration** auf **Administrative Vorlagen** > **System** > **Delegierung von Anmeldeinformationen**.
2. Doppelklicken Sie auf **Delegierung von aktuellen Anmeldeinformationen zulassen**, und wählen Sie **Aktiviert** aus.
3. Klicken Sie unter **Optionen** auf **Anzeigen**, und fügen Sie jeden Hyper-V-Host, den Sie ermitteln möchten, mit dem Präfix **wsman/** der Liste hinzu.
4. Doppelklicken Sie dann unter **Delegierung von Anmeldeinformationen** auf **Delegierung von aktuellen Anmeldeinformationen mit reiner NTLM-Serverauthentifizierung zulassen**. Fügen Sie auch hier wieder jeden Hyper-V-Host, den Sie ermitteln möchten, mit dem Präfix **wsman/** der Liste hinzu.

## <a name="start-continuous-discovery"></a>Starten der kontinuierlichen Ermittlung

Stellen Sie von der Appliance aus eine Verbindung mit Hyper-V-Hosts oder -Clustern her, und starten Sie VM-Ermittlung.

1. Geben Sie unter **Benutzername** und **Kennwort** die Kontoanmeldeinformationen an, die die Appliance für die VM-Ermittlung verwendet. Geben Sie einen Anzeigenamen für die Anmeldeinformationen an, und klicken Sie auf **Details speichern**.
2. Klicken Sie auf **Host hinzufügen**, und geben Sie Details zum Hyper-V-Host/-Cluster an.
3. Klicken Sie auf **Überprüfen**. Nach der Überprüfung wird die Anzahl von VMs angezeigt, die auf dem jeweiligen Host/im jeweiligen Cluster ermittelt werden können.
    - Sollte bei der Überprüfung eines Hosts ein Fehler auftreten, sehen Sie sich den Fehler an, indem Sie mit dem Mauszeiger auf das Symbol in der Spalte **Status** zeigen. Beheben Sie mögliche Probleme, und wiederholen Sie die Überprüfung.
    - Wenn Sie Hosts oder Cluster entfernen möchten, wählen Sie **Löschen** aus.
    - Es ist nicht möglich, einen bestimmten Host aus einem Cluster zu entfernen. Sie können nur den gesamten Cluster entfernen.
    - Ein Cluster kann auch dann hinzugefügt werden, wenn Probleme mit bestimmten, in dem Cluster enthaltenen Hosts vorliegen.
4. Klicken Sie nach der Überprüfung auf **Speichern und Ermittlung starten**, um mit der Ermittlung zu beginnen.

Daraufhin wird die Ermittlung gestartet. Es dauert ca. 1,5 Minuten pro Host, bis Metadaten von ermittelten Servern im Azure-Portal angezeigt werden. 

### <a name="verify-vms-in-the-portal"></a>Überprüfen virtueller Computer im Portal

Nach Abschluss der Ermittlung können Sie überprüfen, ob die VMs im Portal angezeigt werden.

1. Öffnen Sie das Azure Migrate-Dashboard.
2. Klicken Sie unter **Azure Migrate – Server** > **Azure Migrate: Server Assessment** (Azure Migrate-Serverbewertung) auf das Symbol mit der Anzahl für **Ermittelte Server**.

## <a name="set-up-an-assessment"></a>Einrichten einer Bewertung

Mit der Azure Migrate-Serverbewertung können zwei Arten von Bewertungen ausgeführt werden:

**Bewertung** | **Details** | **Daten**
--- | --- | ---
**Leistungsbasiert** | Bewertungen basierend auf gesammelten Leistungsdaten | **Empfohlene VM-Größe**: Basierend auf CPU- und Arbeitsspeicher-Nutzungsdaten<br/><br/> **Empfohlener Datenträgertyp (Verwalteter Datenträger vom Typ Standard oder Premium)** : Basierend auf IOPS und Durchsatz der lokalen Datenträger
**Wie lokal** | Bewertungen basierend auf lokaler Größenanpassung | **Empfohlene VM-Größe**: Basierend auf der Größe der lokalen VM<br/><br> **Empfohlener Datenträgertyp**: Basierend auf der für die Bewertung ausgewählten Speichertypeinstellung



### <a name="run-an-assessment"></a>Durchführen einer Bewertung

Führen Sie eine Bewertung wie folgt aus:

1. Machen Sie sich mit den [bewährten Methoden](best-practices-assessment.md) für die Bewertungserstellung vertraut.
2. Klicken Sie unter **Server** > **Azure Migrate: Server Assessment** (Azure Migrate-Serverbewertung) auf **Bewerten**.

    ![Bewerten](./media/tutorial-assess-hyper-v/assess.png)

3. Geben Sie unter **Server bewerten** einen Namen für die Bewertung an.
4. Klicken Sie auf **Alle anzeigen**, um die Eigenschaften für die Bewertung zu überprüfen.

    ![Bewertungseigenschaften](./media/tutorial-assess-hyper-v/assessment-properties.png)

3. Wählen Sie unter **Gruppe auswählen oder erstellen** die Option **Neu erstellen** aus, und geben Sie einen Namen für die Gruppe ein. Eine Gruppe enthält mindestens eine zu bewertende VM.
4. Wählen Sie unter **Computer zur Gruppe hinzufügen** die VMs aus, die der Gruppe hinzugefügt werden sollen.
5. Klicken Sie auf **Bewertung erstellen**, um die Gruppe zu erstellen und die Bewertung auszuführen.

    ![Erstellen einer Bewertung](./media/tutorial-assess-hyper-v/assessment-create.png)

6. Zeigen Sie die erstellte Bewertung unter **Server** > **Azure Migrate: Serverbewertung** aus.
7. Klicken Sie auf **Bewertung exportieren**, um sie als Excel-Datei herunterzuladen.


## <a name="review-an-assessment"></a>Überprüfen einer Bewertung

Eine Bewertung beschreibt Folgendes:

- **Azure-Bereitschaft**: Gibt an, ob VMs für die Migration zu Azure geeignet sind.
- **Geschätzte monatliche Kosten**: Die geschätzten monatlichen Compute- und Speicherkosten für die Ausführung der VMs in Azure.
- **Geschätzte monatliche Speicherkosten**: Die geschätzten Kosten für den Datenträgerspeicher nach der Migration.


### <a name="view-an-assessment"></a>Anzeigen einer Bewertung

1. Klicken Sie unter **Migrationsziele** >  **Server** > **Azure Migrate: Server Assessment** (Azure Migrate-Serverbewertung) auf **Bewertungen**.
2. Klicken Sie unter **Bewertungen** auf eine Bewertung, um sie zu öffnen.

    ![Zusammenfassung der Bewertung](./media/tutorial-assess-hyper-v/assessment-summary.png)


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

![Zuverlässigkeitsstufe](./media/tutorial-assess-hyper-v/confidence-rating.png)

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

Im dritten Tutorial der Reihe erfahren Sie, wie Sie Hyper-V-VMs mithilfe der Azure Migrate-Servermigration zu Azure migrieren.

> [!div class="nextstepaction"]
> [Migrieren von virtuellen Hyper-V-Computern zu Azure](./tutorial-migrate-hyper-v.md)
