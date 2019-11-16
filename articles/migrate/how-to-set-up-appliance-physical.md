---
title: Einrichten einer Appliance für die Bewertung physischer Server mit der Azure Migrate-Serverbewertung
description: Hier wird beschrieben, wie Sie eine Appliance für die Bewertung physischer Server mit der Azure Migrate-Serverbewertung einrichten.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 11/11/2019
ms.author: raynew
ms.openlocfilehash: a3212e4dac6856a5fd032c731d877453965584ae
ms.sourcegitcommit: 6dec090a6820fb68ac7648cf5fa4a70f45f87e1a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/11/2019
ms.locfileid: "73907166"
---
# <a name="set-up-an-appliance-for-physical-servers"></a>Einrichten einer Appliance für physische Server

In diesem Artikel wird beschrieben, wie Sie die Azure Migrate-Appliance einrichten, wenn Sie physische Server mithilfe der Azure Migrate-Serverbewertung bewerten.

> [!NOTE]
> Werden hier Features erwähnt, die Sie noch nicht im Azure Migrate-Portal sehen, bitten wir Sie um etwas Geduld. Sie werden voraussichtlich im Laufe der nächsten Woche verfügbar.

Die Azure Migrate-Appliance ist eine einfache Appliance, die von der Azure Migrate-Serverbewertung für folgende Aufgaben verwendet wird:

- Ermitteln lokaler Server
- Senden von Meta- und Leistungsdaten für ermittelte Server an die Azure Migrate-Serverbewertung

[Erfahren Sie mehr](migrate-appliance.md) über die Azure Migrate-Appliance.


## <a name="appliance-deployment-steps"></a>Schritte für die Appliancebereitstellung

Die Einrichtung der Appliance umfasst Folgendes:
- Herunterladen einer gezippten Datei mit dem Azure Migrate-Installationsskript aus dem Azure-Portal.
- Extrahieren der Inhalte aus der gezippten Datei. Starten der PowerShell-Konsole mit Administratorrechten.
- Ausführen des PowerShell-Skripts zum Starten der Appliancewebanwendung.
- Durchführen der Erstkonfiguration für die Appliance und Registrieren der Appliance beim Azure Migrate-Projekt

## <a name="download-the-installer-script"></a>Herunterladen des Installationsskripts

Laden Sie gezippte Datei für die Appliance herunter.

1. Klicken Sie unter **Migrationsziele** > **Server** > **Azure Migrate: Server Assessment** (Azure Migrate-Serverbewertung) auf **Ermitteln**.
2. Klicken Sie unter **Computer ermitteln** > **Sind Ihre Computer virtualisiert?** auf **Nicht virtualisiert/Andere**.
3. Klicken Sie auf **Herunterladen**, um die gezippte Datei herunterzuladen.

    ![Herunterladen der VM](./media/how-to-set-up-appliance-hyper-v/download-appliance-hyperv.png)


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


  
## <a name="run-the-azure-migrate-installer-script"></a>Ausführen des Azure Migrate-Installationsskripts
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

Vergewissern Sie sich, dass die Appliance-VM eine Verbindung mit den erforderlichen [Azure-URLs](migrate-support-matrix-hyper-v.md#assessment-appliance-url-access) herstellen kann.

## <a name="configure-the-appliance"></a>Konfigurieren der Appliance

Führen Sie die Ersteinrichtung der Appliance durch.

1. Öffnen Sie in einem Browser auf einem beliebigen Computer, der eine Verbindung mit der VM herstellen kann, und öffnen Sie die URL der Appliance-Web-App: **https://*Appliancename oder IP-Adresse*: 44368**.

   Alternativ können Sie auch auf dem Desktop auf die App-Verknüpfung klicken, um die App zu öffnen.
2. Gehen Sie in der Web-App unter **Erforderliche Komponenten einrichten** wie folgt vor:
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


## <a name="start-continuous-discovery"></a>Starten der kontinuierlichen Ermittlung

Stellen Sie eine Verbindung zwischen der Appliance und den physischen Servern her, und starten Sie die Ermittlung.

1. Klicken Sie auf **Anmeldeinformationen hinzufügen**, um die Kontoanmeldeinformationen anzugeben, die die Appliance für die Serverermittlung verwendet.  
2. Geben Sie das **Betriebssystem**, einen Anzeigenamen für die Anmeldeinformationen, **Benutzername** und **Kennwort** ein, und klicken Sie auf **Hinzufügen**.
Sie können für Windows- und Linux-Server je einen Satz Anmeldeinformationen angeben.
4. Klicken Sie auf **Server hinzufügen**, und geben Sie für die Verbindungsherstellung mit dem Server Details zum Server an: FQDN/IP-Adresse und einen Anzeigenamen für die Anmeldeinformationen (ein Eintrag pro Zeile).
3. Klicken Sie auf **Überprüfen**. Nach der Überprüfung wird die Liste der Server angezeigt, die ermittelt werden können.
    - Sollte bei der Überprüfung eines Servers ein Fehler auftreten, sehen Sie sich den Fehler an, indem Sie mit dem Mauszeiger auf das Symbol in der Spalte **Status** zeigen. Beheben Sie mögliche Probleme, und wiederholen Sie die Überprüfung.
    - Um einen Server zu entfernen, wählen Sie **Löschen** aus.
4. Klicken Sie nach der Überprüfung auf **Speichern und Ermittlung starten**, um mit der Ermittlung zu beginnen.

Daraufhin wird die Ermittlung gestartet. Es dauert etwa 15 Minuten, bis Metadaten von ermittelten VMs im Azure-Portal angezeigt werden. 

## <a name="verify-servers-in-the-portal"></a>Überprüfen von Servern im Portal

Nach Abschluss der Ermittlung können Sie überprüfen, ob die Server im Portal angezeigt werden.

1. Öffnen Sie das Azure Migrate-Dashboard.
2. Klicken Sie unter **Azure Migrate – Server** > **Azure Migrate: Server Assessment** (Azure Migrate-Serverbewertung) auf das Symbol mit der Anzahl für **Ermittelte Server**. 


## <a name="next-steps"></a>Nächste Schritte

Testen Sie die [Bewertung physischer Server](tutorial-assess-physical.md) mit der Azure Migrate-Serverbewertung.
