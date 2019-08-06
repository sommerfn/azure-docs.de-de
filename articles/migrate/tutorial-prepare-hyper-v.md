---
title: Vorbereiten von virtuellen Hyper-V-Computern auf die Bewertung und Migration zu Azure mit Azure Migrate | Microsoft-Dokumentation
description: Beschreibt, wie Sie Vorbereitungen für die Bewertung und Migration von virtuellen Hyper-V-Computern zu Azure mit Azure Migrate treffen.
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 07/24/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 514905bf2db1c0c58faa131eeb916af033b2c830
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68640844"
---
# <a name="prepare-for-assessment-and-migration-of-hyper-v-vms-to-azure"></a>Vorbereiten auf die Bewertung und Migration von virtuellen Hyper-V-Computern zu Azure

In diesem Artikel wird beschrieben, wie Sie Vorbereitungen für die Bewertung und Migration von lokalen virtuellen Hyper-V-Computern zu Azure mit [Azure Migrate](migrate-services-overview.md) treffen.

[Azure Migrate](migrate-overview.md) stellt einen Hub mit Tools bereit, die Ihnen helfen, Apps, Infrastrukturen und Workloads zu ermitteln, zu bewerten und zu Microsoft Azure zu migrieren. Der Hub umfasst Azure Migrate-Tools sowie unabhängige Drittanbietertools bereit. 

Dieses Tutorial ist das erste in einer Reihe, die Ihnen zeigt, wie Sie virtuelle Hyper-V-Computer bewerten und zu Azure migrieren. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Bereiten Sie Azure vor. Einrichten von Berechtigungen für Ihr Azure-Konto und Ihre Ressourcen, um mit Azure Migrate zu arbeiten.
> * Vorbereiten lokaler Hyper-V-Hosts und VMs für die Serverbewertung.
> * Vorbereiten lokaler Hyper-V-Hosts und VMs für die Servermigration.


> [!NOTE]
> In den Tutorials wird der einfachste Bereitstellungspfad für ein Szenario erläutert, damit Sie schnell ein Proof of Concept einrichten können. Die Tutorials verwenden nach Möglichkeit Standardoptionen und zeigen nicht alle möglichen Einstellungen und Pfade. Ausführliche Anweisungen finden Sie in den Vorgehensweisen für die Hyper-V-Bewertung und -Migration.


Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) erstellen, bevor Sie beginnen.


## <a name="prepare-azure"></a>Vorbereiten von Azure

### <a name="azure-permissions"></a>Azure-Berechtigungen

Sie müssen Berechtigungen für die Azure Migrate-Bereitstellung einrichten:

- Berechtigungen für Ihr Azure-Konto zum Erstellen eines Azure Migrate-Projekts 
- Berechtigungen für Ihr Konto zum Registrieren der Azure Migrate-Appliance. Die Appliance wird für die Hyper-V-Ermittlung und -Migration verwendet. Während der Appliance-Registrierung erstellt Azure Migrate zwei Azure Active Directory-Apps (Azure AD), die die Appliance eindeutig identifizieren:
    - Die erste App kommuniziert mit Azure Migrate-Dienstendpunkten.
    - Die zweite App greift auf eine Azure Key Vault-Instanz zu, die während der Registrierung erstellt wurde, um Azure AD-App-Informationen und Appliancekonfigurationseinstellungen zu speichern.



### <a name="assign-permissions-to-create-project"></a>Zuweisen von Berechtigungen für die Projekterstellung

Überprüfen Sie, ob Sie die Berechtigung zum Erstellen eines Azure Migrate-Projekts besitzen.

1. Öffnen Sie im Azure-Portal das Abonnement, und wählen Sie **Zugriffssteuerung (IAM)** aus.
2. Suchen Sie unter **Zugriff überprüfen** das relevante Konto, und klicken Sie darauf, um Berechtigungen anzuzeigen.
3. Sie sollten über die Berechtigung **Mitwirkender** oder **Besitzer** verfügen.
    - Wenn Sie gerade erst ein kostenloses Azure-Konto erstellt haben, sind Sie der Besitzer Ihres Abonnements.
    - Wenn Sie nicht der Besitzer des Abonnements sind, bitten Sie den Besitzer, Ihnen die Rolle zuzuweisen.


### <a name="assign-permissions-to-register-the-appliance"></a>Zuweisen von Berechtigungen zum Registrieren der Appliance

Sie können wie folgt Berechtigungen für Azure Migrate zuweisen, um die während der Applianceregistrierung erstellten Azure AD-Apps zu erstellen:

- Ein Mandantenadministrator/globaler Administrator kann Benutzern unter dem Mandanten Berechtigungen zum Erstellen und Registrieren von Azure AD-Apps erteilen.
- Ein Mandantenadministrator/globaler Administrator kann dem Konto die Rolle „Anwendungsentwickler“ (die über die Berechtigungen verfügt) zuweisen.

Beachten Sie Folgendes:

- Die Apps verfügen nur über die oben beschriebenen Zugriffsberechtigungen für das Abonnement.
- Sie benötigen diese Berechtigungen nur, wenn Sie eine neue Appliance registrieren. Nach Einrichtung der Appliance können die Berechtigungen wieder entfernt werden. 


#### <a name="grant-account-permissions"></a>Erteilen von Kontoberechtigungen

Der Mandanten-/globale Administrator kann wie folgt Berechtigungen erteilen:

1. In Azure AD muss der globale oder der Mandantenadministrator zu **Azure Active Directory** > **Benutzer** > **Benutzereinstellungen** navigieren.
2. Der Administrator muss **App-Registrierungen** auf **Ja** festlegen.

    ![Azure AD-Berechtigungen](./media/tutorial-prepare-hyper-v/aad.png)

> [!NOTE]
> Dies ist eine Standardeinstellung, die nicht vertraulich ist. [Weitere Informationen](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-applications-are-added#who-has-permission-to-add-applications-to-my-azure-ad-instance)



#### <a name="assign-application-developer-role"></a>Zuweisen der Rolle „Anwendungsentwickler“ 

Der Mandantenadministrator/globale Administrator kann einem Konto die Rolle „Anwendungsentwickler“ zuweisen. [Weitere Informationen](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-users-assign-role-azure-portal)


## <a name="prepare-for-hyper-v-assessment"></a>Vorbereiten für die Hyper-V-Bewertung

Führen Sie zur Vorbereitung der Hyper-V-Bewertung die folgenden Schritte aus:

1. Überprüfen Sie die Hyper-V-Hosteinstellungen.
2. Richten Sie auf jedem Host PowerShell-Remoting ein, sodass die Azure Migrate-Appliance über eine WinRM-Verbindung PowerShell-Befehle auf dem Host ausführen kann.
3. Falls der SMB-Remotespeicher VM-Datenträger enthält, ist eine Delegierung von Anmeldeinformationen erforderlich. 
    - Aktivieren Sie die CredSSP-Delegierung, sodass die Azure Migrate-Appliance als Client fungieren und die Anmeldeinformationen an einen Host delegieren kann. T
    - Sie ermöglichen es jedem Host wie nachfolgend beschrieben, als Delegat für die Appliance zu fungieren.
    - Wenn Sie später die Appliance einrichten, aktivieren Sie die Delegierung für sie.
4. Überprüfen Sie die Applianceanforderungen und den für die Appliance erforderlichen URL-/Portzugriff.
5. Richten Sie ein Konto ein, das von der Appliance zum Ermitteln virtueller Computer verwendet wird.
6. Richten Sie auf jedem virtuellen Computer, den Sie ermitteln und bewerten möchten, Hyper-V-Integrationsdienste ein.


Sie können diese Einstellungen anhand der nachfolgenden Verfahren manuell konfigurieren. Führen Sie alternativ das Konfigurationsskript für Hyper-V-Voraussetzungen aus.

### <a name="hyper-v-prerequisites-configuration-script"></a>Konfigurationsskript für Hyper-V-Voraussetzungen

Das Skript überprüft Hyper-V-Hosts und konfiguriert die Einstellungen, die Sie zum Ermitteln und Bewerten virtueller Hyper-V-Computer benötigen. Dies funktioniert folgendermaßen:

- Das Skript überprüft, ob es unter einer unterstützten PowerShell-Version ausgeführt wird.
- Es wird überprüft, ob Sie (der Benutzer, der das Skript ausführt) über Administratorberechtigungen auf dem Hyper-V-Host verfügen.
- Das Skript ermöglicht die Erstellung eines lokalen Benutzerkontos (kein Administratorkonto), das vom Azure Migrate-Dienst für die Kommunikation mit dem Hyper-V-Host verwendet wird. Dieses Benutzerkonto wird den folgenden Gruppen auf dem Host hinzugefügt:
    - Remoteverwaltungsbenutzer
    - Hyper-V-Administratoren
    - Systemmonitorbenutzer
- Das Skript überprüft, ob auf dem Host eine unterstützte Hyper-V-Version und die Hyper-V-Rolle ausgeführt werden.
- Es aktiviert den WinRM-Dienst und öffnet die Ports 5985 (HTTP) und 5986 (HTTPS) auf dem Host (erforderlich für die Metadatensammlung).
- Das Skript aktiviert PowerShell-Remoting auf dem Host.
- Es überprüft, ob der Hyper-V-Integrationsdienst auf allen vom Host verwalteten virtuellen Computern aktiviert ist. 
- Es aktiviert bei Bedarf CredSSP auf dem Host.

Führen Sie das Skript wie folgt aus:

1. Vergewissern Sie sich, dass mindestens PowerShell-Version 4.0 auf dem Hyper-V-Host installiert ist.
2. Laden Sie das Skript aus dem [Microsoft Download Center](https://aka.ms/migrate/script/hyperv) herunter. Das Skript wird von Microsoft kryptografisch signiert.
3. Überprüfen Sie die Skriptintegrität mithilfe von MD5-oder SHA256-Hashdateien. Führen Sie den folgenden Befehl aus, um den Hash für das Skript zu generieren:
    ```
    C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]
    ```
    Beispielverwendung: 
    ```
    C:\>CertUtil -HashFile C:\Users\Administrators\Desktop\ MicrosoftAzureMigrate-Hyper-V.ps1
    SHA256
    ```
    
    Hashwerte:
    Hash | Wert
    --- | ---
    **MD5-Hash** | 0ef418f31915d01f896ac42a80dc414e
    **SHA256-Hash** | 0ef418f31915d01f896ac42a80dc414e0ad60e7299925eff4d1ae9f1c7db485dc9316ef45b0964148a3c07c80761ade2


4.  Führen Sie nach der Überprüfung der Skriptintegrität das Skript mit dem folgenden PowerShell-Befehl auf jedem Hyper-V-Host aus:
    ```
    PS C:\Users\Administrators\Desktop> MicrosoftAzureMigrate-Hyper-V.ps1
    ```


### <a name="verify-hyper-v-host-settings"></a>Überprüfen der Hyper-V-Hosteinstellungen

1. Überprüfen Sie die [Hyper-V-Hostanforderungen](migrate-support-matrix-hyper-v.md#assessment-hyper-v-host-requirements) für die Serverbewertung.
2. Stellen Sie sicher, dass die [erforderlichen Ports](migrate-support-matrix-hyper-v.md#assessment-port-requirements) auf den Hyper-V-Hosts geöffnet sind.

### <a name="enable-powershell-remoting-on-hosts"></a>Aktivieren von PowerShell-Remoting auf Hosts

Richten Sie auf jedem Host PowerShell-Remoting wie folgt ein:

1. Öffnen Sie auf jedem Host eine PowerShell-Konsole als Administrator.
2. Führen Sie den folgenden Befehl aus:

    ```
    Enable-PSRemoting -force
    ```

### <a name="enable-credssp-on-hosts"></a>Aktivieren von „CredSSP“ auf Hosts

Wenn sich auf dem Host VM-Datenträger auf SMB-Freigaben befinden, führen Sie diesen Schritt auf dem Host aus.

- Sie können diesen Befehl remote auf allen Hyper-V-Hosts ausführen.
- Wenn Sie in einem Cluster neue Hostknoten hinzufügen, werden diese automatisch zur Ermittlung hinzugefügt, aber Sie müssen CredSSP jedoch bei Bedarf manuell auf den neuen Knoten aktivieren.

Gehen Sie zur Aktivierung wie folgt vor:

1. Identifizieren Sie Hyper-V-Hosts, auf denen Hyper-V-VMs mit Datenträgern auf SMB-Freigaben ausgeführt werden.
2. Führen Sie den folgenden Befehl auf jedem identifizierten Hyper-V-Host aus:

    ```
    Enable-WSManCredSSP -Role Server -Force
    ```

Beim Einrichten der Appliance schließen Sie die Einrichtung von CredSSP durch [die Aktivierung für die Appliance](tutorial-assess-hyper-v.md#delegate-credentials-for-smb-vhds) ab. Dieser Vorgang wird im nächsten Tutorial dieser Reihe beschrieben.


### <a name="verify-appliance-settings"></a>Überprüfen von Appliance-Einstellungen

Bevor Sie im nächsten Tut die Azure Migrate-Appliance einrichten und mit der Bewertung beginnen, bereiten Sie die Appliance-Bereitstellung vor.

1. [Überprüfen](migrate-support-matrix-hyper-v.md#assessment-appliance-requirements) Sie Appliance-Anforderungen.
2. [Überprüfen](migrate-support-matrix-hyper-v.md#assessment-appliance-url-access) Sie die Azure-URLs, auf die die Appliance zugreifen muss.
3. Überprüfen Sie die Daten, die die Appliance während der Ermittlung und Bewertung sammelt.
4. [Beachten](migrate-support-matrix-hyper-v.md#assessment-port-requirements) Sie die Anforderungen an den Portzugriff für die Appliance.


### <a name="set-up-an-account-for-vm-discovery"></a>Einrichten eines Kontos für die VM-Ermittlung

Azure Migrate benötigt Berechtigungen zum Ermitteln lokaler VMs.

- Richten Sie ein Domänen- oder lokales Benutzerkonto mit Administratorberechtigungen auf dem Hyper-V-Host/-Cluster ein.

    - Sie benötigen ein einziges Konto für alle Hosts und Cluster, die Sie in die Ermittlung einschließen möchten.
    - Das Konto kann ein lokales oder ein Domänenkonto sein. Es wird empfohlen, dass es über Administratorberechtigungen für die Hyper-V-Hosts oder -Cluster verfügt.
    - Wenn Sie keine Administratorberechtigungen zuweisen möchten, sind alternativ die folgenden Berechtigungen erforderlich:
        - Remoteverwaltungsbenutzer
        - Hyper-V-Administratoren
        - Systemmonitorbenutzer

### <a name="enable-integration-services-on-vms"></a>Aktivieren von Integrationsdiensten auf VMs

Integrationsdienste sollten auf jedem virtuellen Computer aktiviert werden, damit Azure Migrate Betriebssysteminformationen auf dem virtuellen Computer erfassen kann.

Aktivieren Sie auf virtuellen Computern, die Sie ermitteln und bewerten möchten, [Hyper-V-Integrationsdienste](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-integration-services) auf jedem virtuellen Computer. 

## <a name="prepare-for-hyper-v-migration"></a>Vorbereiten der Hyper-V-Migration

1. [Überprüfen](migrate-support-matrix-hyper-v.md#migration-hyper-v-host-requirements) Sie die Hyper-V-Hostanforderungen für die Migration.
2. [Überprüfen](migrate-support-matrix-hyper-v.md#migration-hyper-v-vm-requirements) Sie die Anforderungen für Hyper-V-VMs, die Sie zu Azure migrieren möchten.
3. [Beachten](migrate-support-matrix-hyper-v.md#migration-hyper-v-host-url-access) Sie die Azure-URLs, auf die Hyper-V-Hosts und -Cluster Zugriff für die VM-Migration benötigen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial führen Sie Folgendes durch:
 
> [!div class="checklist"] 
> * Einrichten von Azure-Kontoberechtigungen.
> * Vorbereiten von virtuellen Hyper-V-Hosts für die Bewertung und Migration.

Fahren Sie mit dem nächsten Tutorial fort, um ein Azure Migrate-Projekt zu erstellen und Hyper-V-VMs für die Migration zu Azure zu bewerten.

> [!div class="nextstepaction"] 
> [Bewerten von Hyper-V-VMs](./tutorial-assess-hyper-v.md) 
