---
title: Vorbereiten physischer Server auf die Bewertung mit der Azure Migrate-Serverbewertung
description: Hier erfahren Sie, wie Sie sich mithilfe der Azure Migrate-Serverbewertung auf die Bewertung und Migration physischer Server zu Azure vorbereiten.
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 11/07/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: df425f723aa6a5e261ed6dcd15abfe87b367ad68
ms.sourcegitcommit: 827248fa609243839aac3ff01ff40200c8c46966
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73747972"
---
# <a name="prepare-for-assessment-and-migration-of-physical-servers-to-azure"></a>Vorbereiten auf die Bewertung und Migration physischer Server zu Azure

In diesem Artikel wird beschrieben, wie Sie sich mit [Azure Migrate](migrate-services-overview.md) auf die Bewertung und Migration physischer zu Azure vorbereiten.


> [!NOTE]
> Falls Sie einige dieser Features noch nicht im Azure Migrate-Portal sehen, bitten wir Sie um etwas Geduld. Sie werden voraussichtlich im Laufe der nächsten Woche verfügbar.

[Azure Migrate](migrate-overview.md) stellt einen Hub mit Tools bereit, die Ihnen dabei helfen, Apps, Infrastrukturen und Workloads zu ermitteln, zu bewerten und zu Microsoft Azure zu migrieren. Der Hub umfasst Azure Migrate-Tools sowie Angebote von unabhängigen Drittanbietern (Independent Software Vendors, ISVs). 

Dieses Tutorial ist das erste in einer Reihe und zeigt Ihnen, wie Sie physische Server mit Azure Migrate bewerten. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Bereiten Sie Azure vor. Richten sie Berechtigungen für Ihr Azure-Konto und Ihre Ressourcen ein, um Azure Migrate verwenden zu können.
> * Bereiten Sie lokale physischer Server auf die Serverbewertung vor.


> [!NOTE]
> In den Tutorials wird der einfachste Bereitstellungspfad für ein Szenario erläutert, damit Sie schnell einen Proof of Concept einrichten können. Die Tutorials verwenden nach Möglichkeit Standardoptionen und zeigen nicht alle möglichen Einstellungen und Pfade. Ausführliche Anweisungen finden Sie Anleitungen zur Bewertung physischer Server.


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


## <a name="prepare-for-physical-server-assessment"></a>Vorbereiten auf die Bewertung physischer Server

Zur Vorbereitung auf die Bewertung physischer Server müssen Sie Einstellungen des physischen Servers sowie Einstellungen für die Appliancebereitstellung überprüfen:

### <a name="verify-physical-server-settings"></a>Überprüfen der Einstellungen des physischen Servers

1. Überprüfen Sie für die Serverbewertung die [Anforderungen an physische Server](migrate-support-matrix-hyper-v.md#assessment-hyper-v-host-requirements).
2. Stellen Sie sicher, dass auf physischen Servern die [erforderlichen Ports](migrate-support-matrix-hyper-v.md#assessment-port-requirements) geöffnet sind.


### <a name="verify-appliance-settings"></a>Überprüfen von Appliance-Einstellungen

Bevor Sie im nächsten Tut die Azure Migrate-Appliance einrichten und mit der Bewertung beginnen, bereiten Sie die Appliance-Bereitstellung vor.

1. [Überprüfen](migrate-support-matrix-hyper-v.md#assessment-appliance-requirements) Sie Appliance-Anforderungen.
2. [Überprüfen](migrate-support-matrix-hyper-v.md#assessment-appliance-url-access) Sie die Azure-URLs, auf die die Appliance zugreifen muss.
3. Überprüfen Sie die Daten, die die Appliance während der Ermittlung und Bewertung sammelt.
4. [Beachten](migrate-support-matrix-hyper-v.md#assessment-port-requirements) Sie die Portzugriffsanforderungen für die Appliance.


### <a name="set-up-an-account-for-physical-server-discovery"></a>Einrichten eines Kontos für die Ermittlung physischer Server

Azure Migrate benötigt Berechtigungen zum Ermitteln lokaler Server.

- **Windows:** Richten Sie auf allen Windows-Servern, die Sie in die Ermittlung einschließen möchten, ein lokales Benutzerkonto ein. Das Benutzerkonto muss den folgenden Gruppen hinzugefügt werden: Remotedesktopbenutzer, Leistungsüberwachungsbenutzer und Leistungsprotokollbenutzer.
- **Linux:** Sie benötigen ein root-Konto auf den Linux-Servern, die Sie ermitteln möchten.


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial führen Sie Folgendes durch:
 
> [!div class="checklist"] 
> * Einrichten von Azure-Kontoberechtigungen.
> * Physische Server auf die Bewertung vorbereitet.

Fahren Sie mit dem nächsten Tutorial fort, um ein Azure Migrate-Projekt zu erstellen und physische Server für die Migration zu Azure zu bewerten.

> [!div class="nextstepaction"] 
> [Bewerten physischer Server](./tutorial-assess-physical.md) 