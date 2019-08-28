---
title: Vorbereiten von VMware-VMs für die Bewertung und die Migration zu Azure mit Azure Migrate | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie VMware-VMs mit Azure Migrate für die Bewertung und die Migration zu Azure vorbereiten.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 07/11/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 16c0354466d5c2a1207873a1e83e209da9339705
ms.sourcegitcommit: 0e59368513a495af0a93a5b8855fd65ef1c44aac
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/15/2019
ms.locfileid: "69509989"
---
# <a name="prepare-vmware-vms-for-assessment-and-migration-to-azure"></a>Vorbereiten von VMware-VMs für die Bewertung und die Migration zu Azure

In diesem Artikel erfahren Sie, wie Sie VMware-VMs mit [Azure Migrate](migrate-services-overview.md) für die Bewertung und die Migration zu Azure vorbereiten.

[Azure Migrate](migrate-overview.md) stellt einen Hub mit Tools bereit, die Ihnen dabei helfen, Apps, Infrastrukturen und Workloads zu ermitteln, zu bewerten und zu Microsoft Azure zu migrieren. Der Hub umfasst Azure Migrate-Tools sowie Angebote von unabhängigen Drittanbietern (Independent Software Vendors, ISVs). 


Dieses Tutorial ist das erste in einer Reihe zur Bewertung und Migration von VMware-VMs. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Bereiten Sie Azure vor. Richten sie Berechtigungen für Ihr Azure-Konto und Ihre Ressourcen ein, um Azure Migrate verwenden zu können.
> * Vorbereiten lokaler VMware-Server und VMs für die VM-Bewertung
> * Vorbereiten lokaler VMware-Server und VMs für die VM-Migration

> [!NOTE]
> In den Tutorials wird der einfachste Bereitstellungspfad für ein Szenario erläutert, damit Sie schnell einen Proof of Concept einrichten können. Die Tutorials verwenden nach Möglichkeit Standardoptionen und zeigen nicht alle möglichen Einstellungen und Pfade. Ausführliche Anweisungen finden Sie in den Bewertungs- und Migrationsanleitungen für VMware.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) erstellen, bevor Sie beginnen.


## <a name="prepare-azure"></a>Vorbereiten von Azure

Sie benötigen folgende Azure-Berechtigungen:

- Ihr Azure-Konto benötigt Berechtigungen zum Erstellen eines Azure Migrate-Projekts für die Bewertung und Migration. 
- Für die Bewertung und Agent-lose Migration von VMware-VMs führt Azure Migrate eine einfache Appliance aus, die VMs ermittelt und VM-Metadaten sowie Leistungsdaten an Azure Migrate sendet. In Azure benötigen Sie Berechtigungen zum Registrieren der Azure Migrate-Appliance.
- Für die Migration von VMware-VMs mithilfe der Azure Migrate-Servermigration erstellt Azure Migrate eine Key Vault-Instanz in der Ressourcengruppe, um Zugriffsschlüssel für das Replikationsspeicherkonto in Ihrem Abonnement zu verwalten. Für die Tresorerstellung benötigen Sie Rollenzuweisungsberechtigungen für die Ressourcengruppe, in der sich das Azure Migrate-Projekt befindet. 


### <a name="assign-permissions-to-create-project"></a>Zuweisen von Berechtigungen für die Projekterstellung

1. Öffnen Sie im Azure-Portal das Abonnement, und wählen Sie **Zugriffssteuerung (IAM)** aus.
2. Suchen Sie unter **Zugriff überprüfen** das relevante Konto, und klicken Sie darauf, um Berechtigungen anzuzeigen.
3. Sie sollten über die Berechtigung **Mitwirkender** oder **Besitzer** verfügen.
    - Wenn Sie gerade erst ein kostenloses Azure-Konto erstellt haben, sind Sie der Besitzer Ihres Abonnements.
    - Wenn Sie nicht der Besitzer des Abonnements sind, bitten Sie den Besitzer, Ihnen die Rolle zuzuweisen.

### <a name="assign-permissions-to-register-the-appliance"></a>Zuweisen von Berechtigungen zum Registrieren der Appliance

Wenn Sie die Azure Migrate-Appliance bereitstellen, um VMs zu bewerten oder ohne Agent zu migrieren, müssen Sie sie registrieren.

- Während der Appliance-Registrierung erstellt Azure Migrate zwei Azure AD-Apps (Active Directory), die die Appliance eindeutig identifizieren.
    - Die erste App kommuniziert mit Azure Migrate-Dienstendpunkten.
    - Die zweite App greift auf eine Azure Key Vault-Instanz zu, die während der Registrierung erstellt wurde, um Azure AD-App-Informationen und Appliancekonfigurationseinstellungen zu speichern.
- Sie können wie folgt Berechtigungen für Azure Migrate zuweisen, um diese Azure AD-Apps zu erstellen:
    - Ein Mandantenadministrator/globaler Administrator kann Benutzern in dem Mandanten Berechtigungen zum Erstellen und Registrieren von Azure AD-Apps erteilen.
    - Ein Mandantenadministrator/globaler Administrator kann dem Konto die Rolle „Anwendungsentwickler“ (die über die Berechtigungen verfügt) zuweisen.

Beachten Sie Folgendes:

- Die Apps verfügen nur über die oben beschriebenen Zugriffsberechtigungen für das Abonnement.
- Sie benötigen diese Berechtigungen nur, wenn Sie eine neue Appliance registrieren. Nach Einrichtung der Appliance können die Berechtigungen wieder entfernt werden. 


#### <a name="grant-account-permissions"></a>Erteilen von Kontoberechtigungen

Der Mandantenadministrator/globale Administrator kann Berechtigungen wie folgt erteilen:

1. In Azure AD muss der globale oder der Mandantenadministrator zu **Azure Active Directory** > **Benutzer** > **Benutzereinstellungen** navigieren.
2. Der Administrator muss **App-Registrierungen** auf **Ja** festlegen.

    ![Azure AD-Berechtigungen](./media/tutorial-prepare-vmware/aad.png)

> [!NOTE]
> Dies ist eine Standardeinstellung, die nicht vertraulich ist. [Weitere Informationen](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-applications-are-added#who-has-permission-to-add-applications-to-my-azure-ad-instance)



#### <a name="assign-application-developer-role"></a>Zuweisen der Rolle „Anwendungsentwickler“ 

Der Mandantenadministrator/globale Administrator kann einem Konto die Rolle „Anwendungsentwickler“ zuweisen. [Weitere Informationen](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-users-assign-role-azure-portal)

## <a name="assign-role-assignment-permissions"></a>Zuweisen von Rollenzuweisungsberechtigungen

Gehen Sie wie folgt vor, um Rollenzuweisungsberechtigungen für die Ressourcengruppe zuzuweisen, in der sich das Azure Migrate-Projekt befindet:

1. Wählen Sie im Azure-Portal unter der Ressourcengruppe die Option **Zugriffssteuerung (IAM)** aus.
2. Suchen Sie unter **Zugriff überprüfen** das relevante Konto, und klicken Sie darauf, um Berechtigungen anzuzeigen.

    - Zum Ausführen der Serverbewertung ist die Berechtigung **Mitwirkender** ausreichend.
    - Wenn Sie eine Servermigration ohne Agent ausführen möchten, müssen Sie über die Berechtigung **Besitzer** (oder über die Berechtigungen **Mitwirkender** und **Benutzerzugriffsadministrator**) verfügen.

3. Sollten Sie nicht über die erforderlichen Berechtigungen verfügen, fordern Sie sie beim Besitzer der Ressourcengruppe an. 



## <a name="prepare-for-vmware-vm-assessment"></a>Vorbereiten der Bewertung von VMware-VMs

Zur Vorbereitung der Bewertung von VMware-VMs müssen Sie VmWare-Host- und VM-Einstellungen sowie Einstellungen für die Appliancebereitstellung überprüfen.

### <a name="verify-vmware-settings"></a>Überprüfen der VMware-Einstellungen

1. [Überprüfen](migrate-support-matrix-vmware.md#assessment-vcenter-server-requirements) Sie die VMware-Serveranforderungen für die VM-Bewertung.
2. [Vergewissern Sie sich](migrate-support-matrix-vmware.md#assessment-port-requirements), dass auf vCenter-Hosts die erforderlichen Ports geöffnet sind.


### <a name="set-up-an-account-for-assessment"></a>Einrichten eines Kontos für die Bewertung

Azure Migrate muss auf die vCenter Server-Instanz zugreifen, um VMs für die Bewertung und die Migration ohne Agent zu ermitteln. Für die Bewertung wird ein schreibgeschütztes Konto benötigt, um auf vCenter Server zugreifen zu können.

Sollten Sie einen URL-basierten Firewallproxy verwenden, lassen Sie den Zugriff auf die erforderlichen [Azure-URLs](migrate-support-matrix-vmware.md#assessment-url-access-requirements) zu.

Stellen Sie sicher, dass der Proxy alle CNAME-Einträge auflöst, die ggf. bei der Suche nach den URLs empfangen werden.


### <a name="verify-appliance-settings-for-assessment"></a>Überprüfen der Applianceeinstellungen für die Bewertung

Bevor Sie im nächsten Tutorial die Azure Migrate-Appliance einrichten und mit der Bewertung beginnen, müssen Sie die Appliance-Bereitstellung vorbereiten.

1. [Überprüfen](migrate-support-matrix-vmware.md#assessment-appliance-requirements) Sie die Anforderungen für die Einrichtung der Azure Migrate-Appliance in VMware.
2. [Überprüfen](migrate-support-matrix-vmware.md#assessment-url-access-requirements) Sie die Azure-URLs, auf die die Appliance zugreifen muss.
3. Überprüfen Sie die Daten, die die Appliance während der Ermittlung und Bewertung sammelt.
4. [Beachten](migrate-support-matrix-vmware.md#assessment-port-requirements) Sie die Portzugriffsanforderungen für die Appliance.
5. Die Azure Migrate-Appliance wird unter Verwendung einer OVA-Datei als VMware-VM bereitgestellt. Vergewissern Sie sich in vCenter Server, dass Ihr Konto über Berechtigungen zum Erstellen einer VM mit einer OVA-Datei verfügt.


## <a name="prepare-for-agentless-vmware-migration"></a>Vorbereiten der VMware-Migration ohne Agent

Überprüfen Sie die Anforderungen für die Migration von VMware-VMs ohne Agent.

1. [Überprüfen](migrate-support-matrix-vmware.md#agentless-migration-vmware-server-requirements) Sie die VMware-Serveranforderungen für die Migration ohne Agent.
2. Richten Sie ein Konto für den Zugriff auf vCenter Server mit den [erforderlichen Berechtigungen](migrate-support-matrix-vmware.md#agentless-migration-vcenter-server-permissions) für die Migration ohne Agent ein.
3. [Beachten](migrate-support-matrix-vmware.md#agentless-migration-vmware-vm-requirements) Sie die Anforderungen für VMware-VMs, die Sie mithilfe der Migration ohne Agent zu Azure migrieren möchten.
4. [Überprüfen](migrate-support-matrix-vmware.md#agentless-migration-appliance-requirements) Sie die Applianceanforderungen für die Migration ohne Agent.
5. Beachten Sie die Applianceanforderungen für [URL-Zugriff](migrate-support-matrix-vmware.md#agentless-migration-url-access-requirements) und [Portzugriff](migrate-support-matrix-vmware.md#agentless-migration-port-requirements) für die Migration ohne Agent.


## <a name="prepare-for-agent-based-vmware-migration"></a>Vorbereiten der Agent-basierten VMware-Migration

Überprüfen Sie die Anforderungen für die Agent-basierte Migration von VMware-VMs.

1. [Überprüfen](migrate-support-matrix-vmware.md#agent-based-migration-vmware-server-requirements) Sie die VMware-Serveranforderungen für die Migration ohne Agent. 
2. Richten Sie ein Konto für den Zugriff auf vCenter Server mit den [erforderlichen Berechtigungen](migrate-support-matrix-vmware.md#agent-based-migration-vcenter-server-permissions) für die Migration ohne Agent ein.
3. [Beachten](migrate-support-matrix-vmware.md#agent-based-migration-vmware-vm-requirements) Sie die Anforderungen für VMware-VMs, die Sie mithilfe der Agent-basierten Migration zu Azure migrieren möchten – einschließlich der Installation des Mobility-Diensts auf jeder VM, die Sie migrieren möchten.
4. Beachten Sie den [URL-Zugriff](migrate-support-matrix-vmware.md#agent-based-migration-url-access-requirements).
5. Überprüfen Sie die [Portzugriffsanforderungen](migrate-support-matrix-vmware.md#agent-based-migration-port-requirements) für den Mobility-Dienst, der auf den einzelnen VMs ausgeführt wird, sowie für den Azure Migrate-Konfigurationsserver.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial führen Sie Folgendes durch:
 
> [!div class="checklist"] 
> * Einrichten von Azure-Berechtigungen
> * Vorbereiten von VMware für die Bewertung und Migration


Im nächsten Tutorial erfahren Sie, wie Sie ein Azure Migrate-Projekt einrichten und VMware-VMs bewerten, die zu Azure migriert werden sollen.

> [!div class="nextstepaction"] 
> [Migrieren von VMware-VMs zu Azure (ohne Agent)](./tutorial-migrate-vmware.md) 

