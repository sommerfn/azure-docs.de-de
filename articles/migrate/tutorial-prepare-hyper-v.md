---
title: Vorbereiten von virtuellen Hyper-V-Computern auf die Bewertung und Migration zu Azure mit Azure Migrate | Microsoft-Dokumentation
description: Beschreibt, wie Sie Vorbereitungen für die Bewertung und Migration von virtuellen Hyper-V-Computern zu Azure mit Azure Migrate treffen.
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 07/11/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 9e0d29770aa36f8e79bf08b7c5435ea2dbc4ae38
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67840373"
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
> In den Tutorials wird der einfachste Bereitstellungspfad für ein Szenario erläutert, damit Sie schnell ein Proof of Concept einrichten können. Die Tutorials verwenden nach Möglichkeit Standardoptionen und zeigen nicht alle möglichen Einstellungen und Pfade. Ausführliche Anweisungen finden Sie in den „Vorgehensweisen für die Hyper-V-Bewertung und-Migration“.


Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) erstellen, bevor Sie beginnen.


## <a name="prepare-azure"></a>Vorbereiten von Azure

### <a name="azure-permissions"></a>Azure-Berechtigungen

Sie benötigen ein paar Berechtigungen zum Bereitstellen von Azure Migrate:

- Ihr Azure-Konto benötigt Berechtigungen zum Erstellen eines Azure Migrate-Projekts für die Bewertung und Migration. 
- Ihr Azure-Konto benötigt Berechtigungen zum Registrieren der Azure Migrate-Appliance.
    - Für Bewertung führt Azure Migrate eine einfache Appliance aus, die virtuelle Hyper-V-Computer ermittelt und VM-Metadaten sowie Leistungsdaten an Azure Migrate sendet.
    - Während der Appliance-Registrierung erstellt Azure Migrate zwei Azure Active Directory-Apps (Azure AD), die die Appliance eindeutig identifizieren:
        - Die erste App kommuniziert mit Azure Migrate-Dienstendpunkten.
        - Die zweite App greift auf einen Azure Key Vault zu, der während der Registrierung erstellt wurde, um Azure AD-App-Informationen und Appliance-Konfigurationseinstellungen zu speichern.
    - Sie können Berechtigungen für Azure Migrate zuweisen, um diese Azure AD-Apps mit einer der folgenden Methoden zu erstellen:
        - Ein Mandanten-/globaler Administrator kann Benutzern im Mandanten Berechtigungen erteilen, um Azure AD-Apps zu erstellen und zu registrieren.
        - Ein Mandanten-/globaler Administrator kann dem Konto die Rolle „Anwendungsentwickler“ (die über die Berechtigungen verfügt) zuweisen.
    - Beachten Sie Folgendes:
        - Die Apps verfügen über keine anderen Zugriffsberechtigungen für das Abonnement, außer den oben beschriebenen.
        - Sie benötigen diese Berechtigungen nur, wenn Sie eine neue Appliance registrieren. Sie können die Berechtigungen nach der Einrichtung der Appliance entfernen. 


### <a name="assign-permissions-to-create-project"></a>Zuweisen von Berechtigungen zum Erstellen eines Projekts

Überprüfen Sie, ob Sie die Berechtigung zum Erstellen eines Azure Migrate-Projekts besitzen.

1. Öffnen Sie im Azure-Portal das Abonnement, und wählen Sie **Zugriffssteuerung (IAM)** aus.
2. Suchen Sie unter **Zugriff überprüfen** das relevante Konto, und klicken Sie darauf, um Berechtigungen anzuzeigen.
3. Sie sollten über die Berechtigung **Mitwirkender** oder **Besitzer** verfügen.
    - Wenn Sie gerade erst ein kostenloses Azure-Konto erstellt haben, sind Sie der Besitzer Ihres Abonnements.
    - Wenn Sie nicht der Besitzer des Abonnements sind, arbeiten Sie gemeinsam mit dem Besitzer, um Ihnen die Rolle zuzuweisen.


### <a name="assign-permissions-to-register-the-appliance"></a>Zuweisen von Berechtigungen zum Registrieren der Appliance

Wenn Sie die Azure Migrate-Appliance bereitstellen, um VMs zu bewerten, müssen Sie sie registrieren.

- Während der Appliance-Registrierung erstellt Azure Migrate zwei Azure Active Directory-Apps (Azure AD), die die Appliance eindeutig identifizieren.
    - Die erste App kommuniziert mit Azure Migrate-Dienstendpunkten.
    - Die zweite App greift auf einen Azure Key Vault zu, der während der Registrierung erstellt wurde, um Azure AD-App-Informationen und Appliance-Konfigurationseinstellungen zu speichern.
- Sie können Berechtigungen für Azure Migrate zuweisen, um diese Azure AD-Apps mit einer der folgenden Methoden zu erstellen:
    - Ein Mandanten-/globaler Administrator kann Benutzern im Mandanten Berechtigungen erteilen, um Azure AD-Apps zu erstellen und zu registrieren.
    - Ein Mandanten-/globaler Administrator kann dem Konto die Rolle „Anwendungsentwickler“ (die über die Berechtigungen verfügt) zuweisen.

Beachten Sie Folgendes:

- Die Apps verfügen über keine anderen Zugriffsberechtigungen für das Abonnement, außer den oben beschriebenen.
- Sie benötigen diese Berechtigungen nur, wenn Sie eine neue Appliance registrieren. Sie können die Berechtigungen nach der Einrichtung der Appliance entfernen. 


#### <a name="grant-account-permissions"></a>Erteilen von Kontoberechtigungen

Der Mandanten-/globale Administrator kann wie folgt Berechtigungen erteilen:

1. In Azure AD muss der globale oder der Mandantenadministrator zu **Azure Active Directory** > **Benutzer** > **Benutzereinstellungen** navigieren.
2. Der Administrator muss **App-Registrierungen** auf **Ja** festlegen.

    ![Azure AD-Berechtigungen](./media/tutorial-prepare-hyper-v/aad.png)

> [!NOTE]
> Dies ist eine Standardeinstellung, die nicht vertraulich ist. [Weitere Informationen](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-applications-are-added#who-has-permission-to-add-applications-to-my-azure-ad-instance)



#### <a name="assign-application-developer-role"></a>Zuweisen der Rolle „Anwendungsentwickler“ 

Der Mandanten-/globale Administrator kann einem Konto die Rolle „Anwendungsentwickler“ zuweisen. [Weitere Informationen](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-users-assign-role-azure-portal)


## <a name="prepare-for-hyper-v-assessment"></a>Vorbereiten für die Hyper-V-Bewertung

Zum Vorbereiten der Hyper-V-Bewertung überprüfen Sie die Hyper-V-Host- und VM-Einstellungen sowie die Einstellungen für die Bereitstellung der Appliance.

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

Wenn sich VM-Datenträger auf SMB-Freigaben befinden, führen Sie diesen Schritt auf jedem relevanten Hyper-V-Host aus. Dieser Schritt dient zum Ermitteln von Konfigurationsinformationen für Hyper-V-VMs mit Datenträgern auf SMB-Freigaben. Wenn Sie über keine VM-Datenträger auf SMB-Freigaben verfügen, können Sie diesen Schritt überspringen.

1. Identifizieren Sie Hyper-V-Hosts, auf denen Hyper-V-VMs mit Datenträgern auf SMB-Freigaben ausgeführt werden.
2. Führen Sie den folgenden Befehl auf jedem identifizierten Hyper-V-Host aus:

    ```
    Enable-WSManCredSSP -Role Server -Force
    ```

- Die CredSSP-Authentifizierung gestattet es dem Hyper-V-Host, Anmeldeinformationen im Namen des Azure Migrate-Clients zu delegieren.
- Sie können diesen Befehl remote auf allen Hyper-V-Hosts ausführen.
- Wenn Sie in einem Cluster neue Hostknoten hinzufügen, werden diese automatisch zur Ermittlung hinzugefügt, aber Sie müssen CredSSP jedoch bei Bedarf manuell auf den neuen Knoten aktivieren.

### <a name="verify-appliance-settings"></a>Überprüfen von Appliance-Einstellungen

Bevor Sie im nächsten Tut die Azure Migrate-Appliance einrichten und mit der Bewertung beginnen, bereiten Sie die Appliance-Bereitstellung vor.

1. [Überprüfen](migrate-support-matrix-hyper-v.md#assessment-appliance-requirements) Sie Appliance-Anforderungen.
2. [Überprüfen](migrate-support-matrix-hyper-v.md#assessment-appliance-url-access) Sie die Azure-URLs, auf die die Appliance zugreifen muss.
3. Überprüfen Sie die Daten, die die Appliance während der Ermittlung und Bewertung sammeln wird.
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

- Aktivieren Sie auf virtuellen Computern, die Sie ermitteln und bewerten möchten, [Hyper-V-Integrationsdienste](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-integration-services) auf jedem virtuellen Computer. 

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
