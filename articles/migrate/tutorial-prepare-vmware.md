---
title: Vorbereiten von VMware-VMs für die Bewertung und die Migration zu Azure mit Azure Migrate | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie VMware-VMs mit Azure Migrate für die Bewertung und die Migration zu Azure vorbereiten.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 10/23/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 4cc04e9ab0acdc9d0cdff77ed1de7bea1c1362d4
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73498473"
---
# <a name="prepare-vmware-vms-for-assessment-and-migration-to-azure"></a>Vorbereiten von VMware-VMs für die Bewertung und die Migration zu Azure

In diesem Artikel wird erläutert, wie Sie VMware-VMs mit [Azure Migrate](migrate-services-overview.md) für die Bewertung und die Migration zu Azure vorbereiten.

[Azure Migrate](migrate-overview.md) stellt einen Hub mit Tools bereit, die Ihnen dabei helfen, Apps, Infrastrukturen und Workloads zu ermitteln, zu bewerten und zu Microsoft Azure zu migrieren. Der Hub umfasst Azure Migrate-Tools sowie Angebote von unabhängigen Drittanbietern (Independent Software Vendors, ISVs). 


Dieses Tutorial ist das erste in einer Reihe zur Bewertung und Migration von VMware-VMs. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Vorbereiten von Azure auf die Arbeit mit Azure Migrate
> * Vorbereiten von VMware auf die VM-Bewertung
> * Vorbereiten von VMware auf die VM-Migration

> [!NOTE]
> In den Tutorials wird der einfachste Bereitstellungspfad für ein Szenario erläutert. Sie eignen sich als nützliche Einführung in die Einrichtung einer Bereitstellung und als schnelles Proof of Concept. Die Tutorials verwenden nach Möglichkeit Standardoptionen und zeigen nicht alle möglichen Einstellungen und Pfade. Ausführliche Anweisungen finden Sie in den Bewertungs- und Migrationsanleitungen für VMware.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) erstellen, bevor Sie beginnen.


## <a name="prepare-azure"></a>Vorbereiten von Azure

Sie benötigen folgende Berechtigungen:

**Aufgabe** | **Berechtigungen** 
--- | --- | ---
**Erstellen eines Azure Migrate-Projekts** | Ihr Azure-Konto benötigt Berechtigungen zum Erstellen eines Projekts. 
**Registrieren der Azure Migrate-Appliance** | Azure Migrate verwendet eine schlanke Azure Migrate- Appliance, um VMware-VMs mit der Azure Migrate-Serverbewertung zu bewerten und für die VMware-VMs mit der Azure Migrate-Servermigration eine [Migration ohne Agents](server-migrate-overview.md) durchzuführen. Diese Appliance ermittelt VMs und sendet Meta- und Leistungsdaten zu den VMs an Azure Migrate.<br/><br/>Während der Registrierung erstellt Azure Migrate zwei Azure AD-Apps (Active Directory), die die Appliance eindeutig identifizieren. Azure Migrate benötigt Berechtigungen zum Erstellen dieser Apps.<br/> – Die erste App kommuniziert mit Azure Migrate-Dienstendpunkten.<br/> – Die zweite App greift auf eine Azure Key Vault-Instanz zu, die während der Registrierung erstellt wurde, um Azure AD-App-Informationen und Appliancekonfigurationseinstellungen zu speichern.
**Erstellen einer Key Vault-Instanz** | Für die Migration von VMware-VMs mithilfe der Azure Migrate-Servermigration erstellt Azure Migrate eine Key Vault-Instanz, um Zugriffsschlüssel für das Replikationsspeicherkonto in Ihrem Abonnement zu verwalten. Für die Tresorerstellung benötigen Sie Rollenzuweisungsberechtigungen für die Ressourcengruppe, in der sich das Azure Migrate-Projekt befindet.


### <a name="assign-permissions-to-create-project"></a>Zuweisen von Berechtigungen für die Projekterstellung

1. Öffnen Sie im Azure-Portal das Abonnement, und wählen Sie **Zugriffssteuerung (IAM)** aus.
2. Suchen Sie unter **Zugriff überprüfen** das relevante Konto, und klicken Sie darauf, um Berechtigungen anzuzeigen.
3. Sie sollten über die Berechtigung **Mitwirkender** oder **Besitzer** verfügen.
    - Wenn Sie gerade erst ein kostenloses Azure-Konto erstellt haben, sind Sie der Besitzer Ihres Abonnements.
    - Wenn Sie nicht der Besitzer des Abonnements sind, bitten Sie den Besitzer, Ihnen die Rolle zuzuweisen.

### <a name="assign-permissions-to-register-the-appliance"></a>Zuweisen von Berechtigungen zum Registrieren der Appliance

Um die Appliance zu registrieren, weisen Sie Berechtigungen für Azure Migrate zu, um die Azure AD-Apps während der Applianceregistrierung zu erstellen. Die Berechtigungen können über eine der folgenden Methoden zugewiesen werden:

- Ein Mandantenadministrator/globaler Administrator kann Benutzern unter dem Mandanten Berechtigungen zum Erstellen und Registrieren von Azure AD-Apps erteilen.
- Ein Mandantenadministrator/globaler Administrator kann dem Konto die Rolle „Anwendungsentwickler“ (die über die Berechtigungen verfügt) zuweisen.

> [!NOTE]
> - Die Apps verfügen nur über die oben beschriebenen Zugriffsberechtigungen für das Abonnement.
> - Sie benötigen diese Berechtigungen nur, wenn Sie eine neue Appliance registrieren. Nach Einrichtung der Appliance können die Berechtigungen wieder entfernt werden. 


#### <a name="grant-account-permissions"></a>Erteilen von Kontoberechtigungen

Der Mandantenadministrator/globale Administrator kann Berechtigungen wie folgt erteilen:

1. In Azure AD muss der globale oder der Mandantenadministrator zu **Azure Active Directory** > **Benutzer** > **Benutzereinstellungen** navigieren.
2. Der Administrator muss **App-Registrierungen** auf **Ja** festlegen. Dies ist eine Standardeinstellung, die nicht vertraulich ist. [Weitere Informationen](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-applications-are-added#who-has-permission-to-add-applications-to-my-azure-ad-instance)

    ![Azure AD-Berechtigungen](./media/tutorial-prepare-vmware/aad.png)



#### <a name="assign-application-developer-role"></a>Zuweisen der Rolle „Anwendungsentwickler“ 

Der Mandantenadministrator/globale Administrator kann einem Konto die Rolle „Anwendungsentwickler“ zuweisen. [Weitere Informationen](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-users-assign-role-azure-portal)

### <a name="assign-role-assignment-permissions"></a>Zuweisen von Rollenzuweisungsberechtigungen

Um das Erstellen einer Key Vault-Instanz durch Azure Migrate zu ermöglichen, weisen Sie die folgenden Rollenzuweisungsberechtigungen zu:

1. Wählen Sie im Azure-Portal unter der Ressourcengruppe die Option **Zugriffssteuerung (IAM)** aus.
2. Suchen Sie unter **Zugriff überprüfen** das relevante Konto, und klicken Sie darauf, um Berechtigungen anzuzeigen.

    - Zum Ausführen der Serverbewertung ist die Berechtigung **Mitwirkender** ausreichend.
    - Wenn Sie eine Servermigration ohne Agent ausführen möchten, müssen Sie über die Berechtigung **Besitzer** (oder über die Berechtigungen **Mitwirkender** und **Benutzerzugriffsadministrator**) verfügen.

3. Sollten Sie nicht über die erforderlichen Berechtigungen verfügen, fordern Sie sie beim Besitzer der Ressourcengruppe an. 



## <a name="prepare-for-vmware-vm-assessment"></a>Vorbereiten der Bewertung von VMware-VMs

Führen Sie die folgenden Schritte aus, um die Bewertung von VMware-VMs vorzubereiten:

- **Überprüfen Sie die VMware-Einstellungen**. Stellen Sie sicher, dass die vCenter Server-Instanz und die zu migrierenden VMs die geltenden Anforderungen erfüllen.
- **Richten Sie ein Bewertungskonto ein**. Azure Migrate muss auf die vCenter Server-Instanz zugreifen können, um VMs für die Bewertung zu ermitteln. Sie benötigen ein Konto mit Lesezugriff für den Azure Migrate-Zugriff.
- **Überprüfen Sie die Appliance-Anforderungen**. Überprüfen Sie die Bereitstellungsanforderungen für die Azure Migrate-Appliance, die für die Bewertung verwendet wird.

### <a name="verify-vmware-settings"></a>Überprüfen der VMware-Einstellungen

1. [Überprüfen](migrate-support-matrix-vmware.md#assessment-vcenter-server-requirements) Sie die VMware-Serveranforderungen für die Bewertung.
2. [Vergewissern Sie sich](migrate-support-matrix-vmware.md#assessment-port-requirements), dass auf der vCenter Server-Instanz die benötigten Ports geöffnet sind.


### <a name="set-up-an-account-for-assessment"></a>Einrichten eines Kontos für die Bewertung

Azure Migrate muss auf die vCenter Server-Instanz zugreifen, um VMs für die Bewertung und die Migration ohne Agent zu ermitteln. Wenn Sie nur eine Bewertung durchführen möchten, richten Sie ein Konto mit Lesezugriff für die vCenter Server-Instanz ein.

### <a name="verify-appliance-settings-for-assessment"></a>Überprüfen der Applianceeinstellungen für die Bewertung

Überprüfen Sie die Applianceanforderungen, bevor Sie die Appliance bereitstellen.

1. [Überprüfen](migrate-support-matrix-vmware.md#assessment-appliance-requirements) Sie Applianceanforderungen und -einschränkungen.
2. Wenn Sie einen URL-basierten Firewallproxy verwenden, [überprüfen](migrate-support-matrix-vmware.md#assessment-url-access-requirements) Sie die Azure-URLs, auf die die Appliance Zugriff benötigt. Stellen Sie sicher, dass der Proxy alle CNAME-Einträge auflöst, die ggf. bei der Suche nach den URLs empfangen werden.
3. Überprüfen Sie die [Leistungsdaten](migrate-appliance.md#collected-performance-data-vmware) und [Metadaten](migrate-appliance.md#collected-metadata-vmware), die die Appliance während der Ermittlung und Bewertung sammelt.
4. [Notieren](migrate-support-matrix-vmware.md#assessment-port-requirements) Sie sich die Ports, auf die die Appliance zugreift.
5. Vergewissern Sie sich in vCenter Server, dass Ihr Konto über Berechtigungen zum Erstellen einer VM mit einer OVA-Datei verfügt. Die Azure Migrate-Appliance wird unter Verwendung einer OVA-Datei als VMware-VM bereitgestellt. 

Sollten Sie einen URL-basierten Firewallproxy verwenden, lassen Sie den Zugriff auf die erforderlichen [Azure-URLs](migrate-support-matrix-vmware.md#assessment-url-access-requirements) zu.




## <a name="prepare-for-agentless-vmware-migration"></a>Vorbereiten der VMware-Migration ohne Agent

Überprüfen Sie die Anforderungen für die Migration von VMware-VMs ohne Agent.

1. [Überprüfen](migrate-support-matrix-vmware.md#agentless-migration-vmware-server-requirements) Sie die Anforderungen für den VMware-Server.
2. Richten Sie ein Konto mit den [erforderlichen Berechtigungen](migrate-support-matrix-vmware.md#agentless-migration-vcenter-server-permissions) ein, damit Azure Migrate auf die vCenter Server-Instanz zugreifen kann, wenn Sie die Migration ohne Agents mithilfe der Azure Migrate-Servermigration durchführen.
3. [Überprüfen](migrate-support-matrix-vmware.md#agentless-migration-vmware-vm-requirements) Sie die Anforderungen für VMware-VMs, die Sie mithilfe der Migration ohne Agents zu Azure migrieren möchten.
4. [Überprüfen](migrate-support-matrix-vmware.md#agentless-migration-appliance-requirements) Sie die Anforderungen zur Verwendung der Azure Migrate-Appliance für die Migration ohne Agents.
5. Beachten Sie den [URL-Zugriff](migrate-support-matrix-vmware.md#agentless-migration-url-access-requirements) und den [Portzugriff](migrate-support-matrix-vmware.md#agentless-migration-port-requirements), den die Azure Migrate-Appliance für die Migration ohne Agents benötigt.


## <a name="prepare-for-agent-based-vmware-migration"></a>Vorbereiten der Agent-basierten VMware-Migration

Überprüfen Sie die Anforderungen für die [Agent-basierte Migration](server-migrate-overview.md) von VMware-VMs.

1. [Überprüfen](migrate-support-matrix-vmware.md#agent-based-migration-vmware-server-requirements) Sie die Anforderungen für VMware-Server. 
2. Richten Sie ein Konto mit den [erforderlichen Berechtigungen](migrate-support-matrix-vmware.md#agent-based-migration-vcenter-server-permissions) ein. Dies ist erforderlich, damit Azure Migrate auf die vCenter Server-Instanz zugreifen kann, wenn Sie eine Agent-basierte Migration mithilfe der Azure Migrate-Servermigration durchführen.
3. [Überprüfen](migrate-support-matrix-vmware.md#agent-based-migration-vmware-vm-requirements) Sie die Anforderungen für VMware-VMs, die Sie mithilfe der Agent-basierten Migration zu Azure migrieren möchten – einschließlich der Installation des Mobilitätsdiensts auf jeder VM, die Sie migrieren möchten.
4. Beachten Sie den [URL-Zugriff](migrate-support-matrix-vmware.md#agent-based-migration-url-access-requirements).
5. Überprüfen Sie den [Portzugriff](migrate-support-matrix-vmware.md#agent-based-migration-port-requirements), den Azure Migrate-Komponenten für den Agent-basierten Zugriff benötigen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial führen Sie Folgendes durch:
 
> [!div class="checklist"] 
> * Einrichten von Azure-Berechtigungen
> * Vorbereiten von VMware für die Bewertung und Migration


Im nächsten Tutorial erfahren Sie, wie Sie ein Azure Migrate-Projekt einrichten und VMware-VMs bewerten, die zu Azure migriert werden sollen.

> [!div class="nextstepaction"] 
> [Migrieren von VMware-VMs zu Azure (ohne Agent)](./tutorial-assess-vmware.md) 

