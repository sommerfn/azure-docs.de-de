---
title: Bewerten physischer Server für die Migration mit der Azure Migrate-Serverbewertung
description: Es wird beschrieben, wie Sie lokale physische Server mit der Azure Migrate-Serverbewertung für die Migration zu Azure bewerten.
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 10/23/2019
ms.author: raynew
ms.openlocfilehash: 7574e80101784961448ff3c3b5a49d9e2c2f9807
ms.sourcegitcommit: bc7725874a1502aa4c069fc1804f1f249f4fa5f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73720226"
---
# <a name="assess-physical-servers-with-azure-migrate-server-assessment"></a>Bewerten physischer Server mit Azure Migrate: Migrate-Serverbewertung

> [!NOTE]
> Falls Sie dieses Feature noch nicht im Azure Migrate-Portal sehen, bitten wir Sie um etwas Geduld. Es wird voraussichtlich im Laufe der nächsten Woche verfügbar.
 

In diesem Artikel erfahren Sie, wie Sie lokale physische Server mit Azure Migrate bewerten: Migrate-Serverbewertung bewerten.

[Azure Migrate](migrate-services-overview.md) stellt einen Hub mit Tools bereit, die Ihnen dabei helfen, Apps, Infrastrukturen und Workloads zu ermitteln, zu bewerten und zu Microsoft Azure zu migrieren. Der Hub umfasst Azure Migrate-Tools sowie Angebote von unabhängigen Drittanbietern (Independent Software Vendors, ISVs).

Dieses Tutorial ist das zweite in einer Reihe zur Bewertung physischer Server und deren Migration zu Azure. In diesem Tutorial lernen Sie Folgendes:
> [!div class="checklist"]
> * Einrichten eines Azure Migrate-Projekts
> * Einrichten einer lokal zur Bewertung physischer Server ausgeführten Azure Migrate-Appliance.
> * Starten der kontinuierlichen Ermittlung lokaler physischer Server. Die Appliance sendet Konfigurations- und Leistungsdaten für ermittelte Server an Azure.
> * Gruppieren Sie die ermittelten Server, und bewerten Sie die Servergruppe.
> * Überprüfen der Bewertung

> [!NOTE]
> In den Tutorials wird der einfachste Bereitstellungspfad für ein Szenario erläutert, damit Sie schnell einen Proof of Concept einrichten können. Die Tutorials verwenden nach Möglichkeit Standardoptionen und zeigen nicht alle möglichen Einstellungen und Pfade. Ausführliche Anweisungen finden Sie in den Anleitungsartikeln.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) erstellen, bevor Sie beginnen.


## <a name="prerequisites"></a>Voraussetzungen

- [Absolvieren Sie das erste Tutorial dieser Reihe.](tutorial-prepare-physical.md) Andernfalls funktionieren die Anweisungen in diesem Tutorial nicht.
- Im ersten Tutorial müssen folgende Schritte ausgeführt werden:
    - [Einrichten von Azure-Berechtigungen](tutorial-prepare-physical.md#prepare-azure) für Azure Migrate
    - [Vorbereiten physischer Server](tutorial-prepare-physical.md#prepare-azure) auf die Bewertung. Applianceanforderungen müssen überprüft werden. Außerdem muss ein Konto für die Ermittlung physischer Server eingerichtet worden sein. Die erforderlichen Ports müssen verfügbar sein, und Ihnen müssen die URLs bekannt sein, die für den Zugriff auf Azure benötigt werden.


## <a name="set-up-an-azure-migrate-project"></a>Einrichten eines Azure Migrate-Projekts

Richten Sie wie folgt ein neues Azure Migrate-Projekt ein:

1. Wählen Sie im Azure-Portal **Alle Dienste** aus, und suchen Sie nach **Azure Migrate**.
2. Wählen Sie unter **Dienste** die Option **Azure Migrate** aus.
3. Klicken Sie in der **Übersicht** unter **Server ermitteln, bewerten und migrieren** auf **Server bewerten und migrieren**.

    ![Ermitteln und Bewerten von Servern](./media/tutorial-assess-physical/assess-migrate.png)

4. Klicken Sie unter **Erste Schritte** auf **Tools hinzufügen**.
5. Wählen Sie unter **Projekt migrieren** Ihr Azure-Abonnement aus, und erstellen Sie bei Bedarf eine Ressourcengruppe.     
6. Geben Sie unter **Projektdetails** den Projektnamen und die geografische Region an, in der Sie das Projekt erstellen möchten. Unterstützt werden Asien, Europa, das Vereinigte Königreich und die USA.

    - Die geografische Region des Projekts dient nur zum Speichern der Metadaten, die von lokalen Servern erfasst werden.
    - Beim Ausführen einer Migration kann eine beliebige Zielregion ausgewählt werden.

    ![Erstellen eines Azure Migrate-Projekts](./media/tutorial-assess-physical/migrate-project.png)


7. Klicken Sie auf **Weiter**.
8. Wählen Sie unter **Bewertungstool auswählen** Folgendes aus: **Azure Migrate: Server Assessment** (Azure Migrate-Serverbewertung) > **Weiter**.

    ![Erstellen eines Azure Migrate-Projekts](./media/tutorial-assess-physical/assessment-tool.png)

9. Wählen Sie unter **Migrationstool auswählen** die Option **Hinzufügen eines Migrationstools vorerst überspringen** >  und anschließend **Weiter** aus.
10. Überprüfen Sie die Einstellungen unter **Review + add tools** (Überprüfen und Tools hinzufügen), und klicken Sie auf **Tools hinzufügen**.
11. Warten Sie einige Minuten, bis das Azure Migrate-Projekt bereitgestellt wurde. Sie werden zur Projektseite weitergeleitet. Sollte das Projekt nicht angezeigt werden, können Sie auf dem Azure Migrate-Dashboard unter **Server** darauf zugreifen.


## <a name="set-up-the-appliance"></a>Einrichten der Appliance

Von der Azure Von der Serverbewertung wird eine einfache Appliance ausgeführt.

- Diese Appliance ermittelt physische Server und sendet Meta- und Leistungsdaten zu Servern an die Azure Migrate-Serverbewertung.
- Die Einrichtung der Appliance umfasst Folgendes:
    - Herunterladen einer gezippten Datei mit dem Azure Migrate-Installationsskript aus dem Azure-Portal.
    - Extrahieren der Inhalte aus der gezippten Datei. Starten der PowerShell-Konsole mit Administratorrechten.
    - Ausführen des PowerShell-Skripts zum Starten der Appliancewebanwendung.
    - Durchführen der Erstkonfiguration für die Appliance und Registrieren der Appliance beim Azure Migrate-Projekt
- Für ein einzelnes Azure Migrate-Projekt können mehrere Appliances eingerichtet werden. Auf allen Appliances können Sie beliebig viele physische Server ermitteln. Pro Appliance können maximal 250 Server ermittelt werden.

### <a name="download-the-installer-script"></a>Herunterladen des Installationsskripts

Laden Sie gezippte Datei für die Appliance herunter.

1. Klicken Sie unter **Migrationsziele** > **Server** > **Azure Migrate: Server Assessment** (Azure Migrate-Serverbewertung) auf **Ermitteln**.
2. Klicken Sie unter **Computer ermitteln** > **Sind Ihre Computer virtualisiert?** auf **Nicht virtualisiert/Andere**.
3. Klicken Sie auf **Herunterladen**, um die gezippte Datei herunterzuladen.

    ![Herunterladen des Installers](./media/tutorial-assess-physical/download-appliance.png)


### <a name="verify-security"></a>Überprüfen der Sicherheit

Vergewissern Sie sich vor der Bereitstellung, dass die gezippte Datei sicher ist.

1. Öffnen Sie auf dem Computer, auf den Sie die Datei heruntergeladen haben, ein Administratorbefehlsfenster.
2. Führen Sie den folgenden Befehl aus, um den Hash für die VHD zu generieren:
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Beispielverwendung: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3.  Für die Applianceversion 1.19.05.10 muss der generierte Hash den folgenden Einstellungen entsprechen:

  **Algorithmus** | **Hashwert**
  --- | ---
  SHA256 | 598d2e286f9c972bb7f7382885e79e768eddedfe8a3d3460d6b8a775af7d7f79

### <a name="run-the-azure-migrate-installer-script"></a>Ausführen des Azure Migrate-Installationsskripts
Das Installationsskript führt folgende Schritte aus:

- Installation der Agents und einer Webanwendung für die Ermittlung und Bewertung physischer Server.
- Installation von Windows-Rollen, darunter beispielsweise Windows-Aktivierungsdienst, IIS und PowerShell ISE.
- Download und Installation eines wiederbeschreibbaren IIS-Moduls. [Weitere Informationen](https://www.microsoft.com/download/details.aspx?id=7435)
- Aktualisierung eines Registrierungsschlüssels (HKLM) mit dauerhaften Einstellungsdetails für Azure Migrate.
- Erstellung der folgenden Dateien in diesem Pfad:
    - **Konfigurationsdateien**: %Programdata%\Microsoft Azure\Config
    - **Protokolldateien**: %Programdata%\Microsoft Azure\Logs

Führen Sie das Skript wie folgt aus:

1. Extrahieren Sie die gezippte Datei in einem Ordner auf dem Server, der die Appliance hostet.
2. Starten Sie PowerShell auf dem oben genannten Server mit Administratorberechtigungen (erhöhten Rechten).
3. Ändern Sie das PowerShell-Verzeichnis in den Ordner, in den die Inhalte der gezippten Datei extrahiert wurden, die Sie heruntergeladen haben.
4. Führen Sie das Skript mit folgendem Befehl aus:
    ```
    PS C:\Users\Administrators\Desktop> AzureMigrateInstaller-physical.ps1
    ```
Das Skript startet die Appliancewebanwendung, nachdem es erfolgreich ausgeführt wurde.


### <a name="verify-appliance-access-to-azure"></a>Überprüfen des Appliancezugriffs auf Azure

Vergewissern Sie sich, dass die Appliance eine Verbindung mit [Azure-URLs](migrate-support-matrix-physical.md#assessment-appliance-url-access) herstellen kann.


### <a name="configure-the-appliance"></a>Konfigurieren der Appliance

Führen Sie die Ersteinrichtung der Appliance durch.

1. Öffnen Sie in einem Browser auf einem beliebigen Computer, der eine Verbindung mit der Appliance herstellen kann, die URL der Appliance-Web-App: **https://*Name oder IP-Adresse der Appliance*: 44368**.

   Alternativ können Sie auch auf dem Desktop auf die App-Verknüpfung klicken, um die App zu öffnen.
2. Gehen Sie in der Web-App unter **Erforderliche Komponenten einrichten** wie folgt vor:
    - **Lizenz**: Akzeptieren Sie die Lizenzbedingungen, und lesen Sie die Drittanbieterinformationen.
    - **Konnektivität**: Die App überprüft, ob der Server über Internetzugriff verfügt. Wenn der Server einen Proxy verwendet:
        - Klicken Sie auf **Proxyeinstellungen**, und geben Sie die Proxyadresse und den Lauschport an (im Format http://ProxyIPAddress oder http://ProxyFQDN ).
        - Geben Sie die Anmeldeinformationen an, wenn der Proxy eine Authentifizierung erfordert.
        - Es werden nur HTTP-Proxys unterstützt.
    - **Uhrzeitsynchronisierung**: Die Uhrzeit wird überprüft. Die Uhrzeit der Appliance muss mit der Internetzeit synchronisiert werden, damit die Ermittlung ordnungsgemäß funktioniert.
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


## <a name="start-continuous-discovery"></a>Starten der kontinuierlichen Ermittlung

Stellen Sie nun eine Verbindung zwischen der Appliance und den zu ermittelnden physischen Servern her, und starten Sie die Ermittlung.

1. Klicken Sie auf **Anmeldeinformationen hinzufügen**, um die Kontoanmeldeinformationen anzugeben, die die Appliance für die Serverermittlung verwendet.  
2. Geben Sie das **Betriebssystem**, einen Anzeigenamen für die Anmeldeinformationen, **Benutzername** und **Kennwort** ein, und klicken Sie auf **Hinzufügen**.
Sie können für Windows- und Linux-Server je einen Satz Anmeldeinformationen angeben.
4. Klicken Sie auf **Server hinzufügen**, und geben Sie für die Verbindungsherstellung mit dem Server Details zum Server an: FQDN/IP-Adresse und einen Anzeigenamen für die Anmeldeinformationen (ein Eintrag pro Zeile).
3. Klicken Sie auf **Überprüfen**. Nach der Überprüfung wird die Liste der Server angezeigt, die ermittelt werden können.
    - Sollte bei der Überprüfung eines Servers ein Fehler auftreten, sehen Sie sich den Fehler an, indem Sie mit dem Mauszeiger auf das Symbol in der Spalte **Status** zeigen. Beheben Sie mögliche Probleme, und wiederholen Sie die Überprüfung.
    - Um einen Server zu entfernen, wählen Sie **Löschen** aus.
4. Klicken Sie nach der Überprüfung auf **Speichern und Ermittlung starten**, um mit der Ermittlung zu beginnen.

Daraufhin wird die Ermittlung gestartet. Es dauert etwa 15 Minuten, bis Metadaten von ermittelten Servern im Azure-Portal angezeigt werden. 

### <a name="verify-servers-in-the-portal"></a>Überprüfen von Servern im Portal

Nach der Ermittlung können Sie überprüfen, ob die Server im Azure-Portal angezeigt werden.

1. Öffnen Sie das Azure Migrate-Dashboard.
2. Klicken Sie unter **Azure Migrate – Server** > **Azure Migrate: Server Assessment** (Azure Migrate-Serverbewertung) auf das Symbol mit der Anzahl für **Ermittelte Server**.

## <a name="set-up-an-assessment"></a>Einrichten einer Bewertung

Es gibt zwei Arten von Bewertungen, die Sie mit der Azure Migrate-Serverbewertung erstellen können:

**Bewertung** | **Details** | **Daten**
--- | --- | ---
**Leistungsbasiert** | Bewertungen basierend auf gesammelten Leistungsdaten | **Empfohlene VM-Größe**: Basierend auf CPU- und Arbeitsspeicher-Nutzungsdaten<br/><br/> **Empfohlener Datenträgertyp (Verwalteter Datenträger vom Typ Standard oder Premium)** : Basierend auf IOPS und Durchsatz der lokalen Datenträger
**Wie lokal** | Bewertungen basierend auf lokaler Größenanpassung | **Empfohlene VM-Größe**: Basierend auf der Größe des lokalen Servers<br/><br> **Empfohlener Datenträgertyp**: Basierend auf der für die Bewertung ausgewählten Speichertypeinstellung


### <a name="run-an-assessment"></a>Durchführen einer Bewertung

Führen Sie eine Bewertung wie folgt aus:

1. Machen Sie sich mit den [bewährten Methoden](best-practices-assessment.md) für die Bewertungserstellung vertraut.
2. Klicken Sie auf der Registerkarte **Server** unter der Kachel **Azure Migrate: Server Assessment** (Azure Migrate-Serverbewertung) auf **Bewerten**.

    ![Bewerten](./media/tutorial-assess-physical/assess.png)

2. Geben Sie unter **Server bewerten** einen Namen für die Bewertung an.
3. Klicken Sie auf **Alle anzeigen**, um die Eigenschaften für die Bewertung zu überprüfen.

    ![Bewertungseigenschaften](./media/tutorial-assess-physical/view-all.png)

3. Wählen Sie unter **Gruppe auswählen oder erstellen** die Option **Neu erstellen** aus, und geben Sie einen Namen für die Gruppe ein. Eine Gruppe enthält mindestens einen zu bewertenden Server.
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
3. Sie können einen Drilldown ausführen, um die Details für bestimmte Server anzuzeigen.


### <a name="review-confidence-rating"></a>Prüfen der Zuverlässigkeitsstufe

Wenn Sie leistungsbasierte Bewertungen ausführen, wird der Bewertung eine Zuverlässigkeitsstufe zugewiesen.

![Zuverlässigkeitsstufe](./media/tutorial-assess-physical/confidence-rating.png)

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

Im dritten Tutorial der Reihe erfahren Sie, wie Sie physische Server mit Azure Migrate zu Azure migrieren: Servermigration.

> [!div class="nextstepaction"]
> [Migrieren physischer Server](./tutorial-migrate-physical-virtual-machines.md)
