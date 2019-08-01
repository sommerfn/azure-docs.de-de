---
title: Einrichten einer Appliance für die Azure Migrate-Serverbewertung/-migration für VMware-VMs | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie eine Appliance unter Verwendung der Azure Migrate-Serverbewertung/-migration für die Ermittlung, Bewertung und Agent-lose Migration von VMware-VMs einrichten.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 07/08/2019
ms.author: raynew
ms.openlocfilehash: fe190381df346278e75a3e6fd9876b80c33bd86b
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67810197"
---
# <a name="set-up-an-appliance-for-vmware-vms"></a>Einrichten einer Appliance für VMware-VMs

In diesem Artikel erfahren Sie, wie Sie eine Azure Migrate-Appliance einrichten, wenn Sie VMware-VMs mithilfe des Azure Migrate-Serverbewertungstools bewerten oder VMware-VMs mithilfe des Azure Migrate-Servermigrationstools ohne Agent zu Azure migrieren.

Bei der VMware-VM-Appliance handelt es sich um eine einfache Appliance, die von der Azure Migrate-Serverbewertung/-migration für Folgendes verwendet wird:

- Ermitteln Sie lokale virtuelle VMware-Computer.
- Senden von Meta- und Leistungsdaten für ermittelte VMs an die Azure Migrate-Serverbewertung/-migration.

Weitere Informationen zur Azure Migrate-Appliance finden Sie [hier](migrate-appliance.md).


## <a name="appliance-deployment-steps"></a>Schritte für die Appliancebereitstellung

Die Einrichtung der Appliance umfasst Folgendes:
- Herunterladen einer OVA-Vorlagendatei und Importieren der Datei in vCenter Server.
- Erstellen der Appliance und Überprüfen der Verbindungsherstellung mit der Azure Migrate-Serverbewertung. 
- Durchführen der Erstkonfiguration für die Appliance und Registrieren der Appliance beim Azure Migrate-Projekt.

## <a name="download-the-ova-template"></a>Herunterladen der OVA-Vorlage

1. Klicken Sie unter **Migrationsziele** > **Server** > **Azure Migrate: Server Assessment** (Azure Migrate-Serverbewertung) auf **Ermitteln**.
2. Wählen Sie unter **Computer ermitteln** > **Sind Ihre Computer virtualisiert?** die Option **Ja, mit VMware vSphere Hypervisor** aus.
3. Klicken Sie auf **Herunterladen**, um die OVA-Vorlagendatei herunterzuladen.



### <a name="verify-security"></a>Überprüfen der Sicherheit

Vergewissern Sie sich vor der Bereitstellung, dass die OVA-Datei sicher ist.

1. Öffnen Sie auf dem Computer, auf den Sie die Datei heruntergeladen haben, ein Administratorbefehlsfenster.
2. Führen Sie den folgenden Befehl aus, um den Hash für die OVA-Datei zu generieren:
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Beispielverwendung: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3. Für die Applianceversion 1.0.0.5 muss der generierte Hash den folgenden Einstellungen entsprechen: 

  **Algorithmus** | **Hashwert**
  --- | ---
  MD5 | ddfdf21c64af02a222ed517ce300c977


## <a name="create-the-appliance-vm"></a>Erstellen der Appliance-VM

Importieren Sie die heruntergeladene Datei, und erstellen Sie eine VM.

1. Klicken Sie in der vSphere-Clientkonsole auf **Datei** > **Deploy OVF Template** (OVF-Vorlage bereitstellen).
2. Geben Sie im Assistenten zum Bereitstellen der OVF-Vorlage unter **Quelle** den Speicherort der OVA-Datei an.
3. Geben Sie unter **Name** und **Standort** einen Anzeigenamen für die VM an. Wählen Sie das Inventarobjekt aus, in dem die VM gehostet wird.
5. Geben Sie unter **Host/Cluster** den Host oder Cluster an, auf bzw. in dem die VM ausgeführt wird.
6. Geben Sie unter **Speicher** das Speicherziel für die VM an.
7. Geben Sie unter **Datenträgerformat** den Typ und die Größe des Datenträgers an.
8. Geben Sie unter **Netzwerkzuordnung** das Netzwerk an, mit dem die VM eine Verbindung herstellt. Das Netzwerk muss über eine Internetverbindung verfügen, um Metadaten an die Azure Migrate-Serverbewertung senden zu können.
9. Überprüfen Sie die Einstellungen, und klicken Sie dann auf **Fertig stellen**.


### <a name="verify-appliance-access-to-azure"></a>Überprüfen des Appliancezugriffs auf Azure

Vergewissern Sie sich, dass die Appliance-VM eine Verbindung mit [Azure-URLs](migrate-support-matrix-vmware.md#assessment-url-access-requirements) herstellen kann.


## <a name="configure-the-appliance"></a>Konfigurieren der Appliance

Führen Sie die Ersteinrichtung der Appliance durch.

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
    - **Uhrzeitsynchronisierung**: Die Uhrzeit wird überprüft. Die Uhrzeit der Appliance muss mit der Internetzeit synchronisiert werden, damit die Ermittlung ordnungsgemäß funktioniert.
    - **Updates installieren**: Azure Migrate überprüft, ob die neusten Appliance-Updates installiert sind.
    - **Install VDDK** (VDDK installieren): Azure Migrate überprüft, ob das VMware vSphere-VDDK (Virtual Disk Development Kit) installiert ist.
        - Azure Migrate verwendet das VDDK, um Computer bei der Migration zu Azure zu replizieren.
        - Laden Sie VDDK 6.7 von VMware herunter, und extrahieren Sie den Inhalt der heruntergeladenen ZIP-Datei am angegebenen Ort auf der Appliance.

## <a name="register-the-appliance-with-azure-migrate"></a>Registrieren der Appliance bei Azure Migrate

1. Klicken Sie auf **Anmelden**. Sollte keine Anmeldung angezeigt werden, vergewissern Sie sich, dass Sie den Popupblocker im Browser deaktiviert haben.
2. Melden Sie sich auf der neuen Registerkarte mit Ihren Azure-Anmeldeinformationen an. 
    - Melden Sie sich mit Ihrem Benutzernamen und Ihrem Kennwort an.
    - Die Anmeldung mit einer PIN wird nicht unterstützt.
3. Kehren Sie nach erfolgreicher Anmeldung zur Web-App zurück.
2. Wählen Sie das Abonnement aus, in dem das Azure Migrate-Projekt erstellt wurde. Wählen Sie anschließend das Projekt aus.
3. Geben Sie einen Namen für die Appliance an. Für den Namen können bis zu 14 alphanumerische Zeichen angegeben werden.
4. Klicken Sie auf **Registrieren**.


## <a name="start-continuous-discovery"></a>Starten der kontinuierlichen Ermittlung

Stellen Sie als Nächstes über die Appliance eine Verbindung mit vCenter Server her, und starten Sie die VM-Ermittlung. 

1. Geben Sie unter **vCenter Server-Details angeben** den Namen (FQDN) oder die IP-Adresse der vCenter Server-Instanz an. Sie können den Standardport beibehalten oder einen benutzerdefinierten Port angeben, an dem Ihre vCenter Server-Instanz lauscht.
2. Geben Sie unter **Benutzername** und **Kennwort** die Anmeldeinformationen für das schreibgeschützte Konto an, über das die Appliance VMs in der vCenter Server-Instanz ermittelt. Vergewissern Sie sich, dass das Konto über die [erforderlichen Berechtigungen für die Ermittlung](migrate-support-matrix-vmware.md#assessment-vcenter-server-permissions) verfügt.
3. Vergewissern Sie sich durch Klicken auf **Verbindung überprüfen**, dass die Appliance eine Verbindung mit vCenter Server herstellen kann.
4. Klicken Sie nach der Verbindungsherstellung auf **Speichern und Ermittlung starten**.


Daraufhin wird die Ermittlung gestartet. Es dauert etwa 15 Minuten, bis Metadaten von ermittelten VMs im Portal angezeigt werden. 


## <a name="next-steps"></a>Nächste Schritte

Lesen Sie die Tutorials zur [VMware-Bewertung](tutorial-assess-vmware.md) und zur [Migration ohne Agent](tutorial-migrate-vmware.md).
