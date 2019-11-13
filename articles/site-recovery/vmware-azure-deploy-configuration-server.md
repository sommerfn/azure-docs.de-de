---
title: Bereitstellen des Konfigurationsservers für die VMware-Notfallwiederherstellung mit Azure Site Recovery | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie einen Konfigurationsserver für die VMware-Notfallwiederherstellung in Azure Site Recovery bereitstellen.
services: site-recovery
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 10/15/2019
ms.author: ramamill
ms.openlocfilehash: 92b51b3955833bac6f87457a19e4d6359600a25a
ms.sourcegitcommit: 827248fa609243839aac3ff01ff40200c8c46966
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73747835"
---
# <a name="deploy-a-configuration-server"></a>Bereitstellen eines Konfigurationsservers

Sie stellen einen lokalen Konfigurationsserver bereit, wenn Sie [Azure Site Recovery](site-recovery-overview.md) für die Notfallwiederherstellung von VMware-VMs und physischen Servern in Azure verwenden. Der Konfigurationsserver koordiniert die Kommunikation zwischen der lokalen VMware-Umgebung und Azure. Darüber hinaus verwaltet er die Datenreplikation. In diesem Artikel werden die erforderlichen Schritte zum Bereitstellen des Konfigurationsservers beim Replizieren virtueller VMware-Computer in Azure beschrieben. Wenn Sie einen Konfigurationsserver für die Replikation physischer Server einrichten müssen, finden Sie weitere Informationen unter [Einrichten des Konfigurationsservers für die Notfallwiederherstellung von physischen Servern in Azure](physical-azure-set-up-source.md).

> [!TIP]
> Weitere Informationen zur Rolle eines Konfigurationsservers als Bestandteil der Azure Site Recovery-Architektur finden Sie unter [Architektur der Notfallwiederherstellung von VMware zu Azure](vmware-azure-architecture.md).

## <a name="deploy-a-configuration-server-through-an-ova-template"></a>Bereitstellung eines Konfigurationsservers über eine OVA-Vorlage

Der Konfigurationsserver muss als hochverfügbare VMware-VM mit bestimmten Mindestanforderungen an die Hardware und Größenanpassung eingerichtet werden. Um eine unkomplizierte und einfache Bereitstellung zu ermöglichen, bietet Site Recovery eine zum Download verfügbare OVA-Vorlage (Open Virtualization Application), mit der der Konfigurationsserver entsprechend aller hier aufgeführten obligatorischen Anforderungen eingerichtet werden kann.

## <a name="prerequisites"></a>Voraussetzungen

Die Mindesthardwareanforderungen für einen Konfigurationsserver werden in den folgenden Abschnitten zusammengefasst.

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

## <a name="azure-active-directory-permission-requirements"></a>Anforderungen für Azure Active Directory-Berechtigungen

Sie benötigen einen Benutzer mit einem der folgenden Berechtigungssätze in Azure Active Directory (Azure AD), um den Konfigurationsserver mit Azure Site Recovery-Diensten registrieren zu können.

1. Der Benutzer muss über die Rolle „Anwendungsentwickler“ verfügen, um eine Anwendung erstellen zu können.
    - Zur Überprüfung melden Sie sich beim Azure-Portal an.</br>
    - Navigieren Sie zu **Azure Active Directory** > **Rollen und Administratoren**.</br>
    - Vergewissern Sie sich, dass dem Benutzer die Rolle „Anwendungsentwickler“ zugewiesen ist. Falls nicht, verwenden Sie einen Benutzer mit dieser Berechtigung, oder bitten Sie den [Administrator, die Berechtigung zu aktivieren](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-users-assign-role-azure-portal#assign-roles).
    
2. Wenn die Rolle „Anwendungsentwickler“ nicht zugewiesen werden kann, stellen Sie sicher, dass das Flag **Benutzer können Anwendungen registrieren** auf **true** festgelegt ist, damit der Benutzer eine Identität erstellen kann. So aktivieren Sie diese Berechtigungen
    - Melden Sie sich beim Azure-Portal an.
    - Navigieren Sie zu **Azure Active Directory** > **Benutzereinstellungen**.
    - Wählen Sie unter **App-Registrierungen**, **Benutzer können Anwendungen registrieren** die Option **Ja** aus.

      ![Azure AD_application_permission](media/vmware-azure-deploy-configuration-server/AAD_application_permission.png)

> [!NOTE]
> Active Directory-Verbunddienste (AD FS) *werden nicht unterstützt*. Verwenden Sie ein Konto, das mit [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-whatis) verwaltet wird.

## <a name="download-the-template"></a>Herunterladen der Vorlage

1. Navigieren Sie im Tresor zu **Infrastruktur vorbereiten** > **Quelle**.
2. Klicken Sie unter **Quelle vorbereiten** auf **+Konfigurationsserver**.
3. Überprüfen Sie unter **Server hinzufügen**, ob unter **Servertyp** die Option **Konfigurationsserver für VMware** angezeigt wird.
4. Laden Sie die OVA-Vorlage für den Konfigurationsserver herunter.

   > [!TIP]
   >Die neueste Version der Konfigurationsservervorlage können Sie auch direkt aus dem [Microsoft Download Center](https://aka.ms/asrconfigurationserver) herunterladen.

> [!NOTE]
> Bei der mit einer OVA-Vorlage bereitgestellten Lizenz handelt es sich um eine Evaluierungslizenz mit einer Gültigkeit von 180 Tagen. Nach diesem Zeitraum müssen Sie eine Lizenz erwerben.

## <a name="import-the-template-in-vmware"></a>Importieren der Vorlage in VMware

1. Melden Sie sich beim VMware vCenter-Server oder beim vSphere ESXi-Host über den VMWare vSphere-Client an.
2. Wählen Sie im Menü **Datei** die Option **OVF-Vorlage bereitstellen** aus, um den **Assistenten zum Bereitstellen von OVF-Vorlagen** zu starten.

     ![Bereitstellen der OVF-Vorlage](./media/vmware-azure-deploy-configuration-server/vcenter-wizard.png)

3. Geben Sie unter **Quelle auswählen** den Speicherort der heruntergeladenen OVF-Vorlage ein.
4. Klicken Sie unter **Bewertungsdetails** auf **Weiter**.
5. Übernehmen Sie unter **Name und Ordner auswählen** und **Konfiguration auswählen** die Standardeinstellungen.
6. Wählen Sie unter **Speicher auswählen** für **Select virtual disk format** (Format für virtuellen Datenträger auswählen) die Option **Thick Provision Eager Zeroed** (Breite Bereitstellung gegen null) aus, um eine optimale Leistung zu erzielen. Die Verwendung der schlanken Speicherzuweisung kann die Leistung des Konfigurationsservers beeinträchtigen.
7. Übernehmen Sie auf den übrigen Seiten des Assistenten die Standardeinstellungen.
8. Gehen Sie unter **Bereit für den Abschluss** wie folgt vor:

    * Wählen Sie zum Einrichten des virtuellen Computers mit den Standardeinstellungen **Power on after deployment** (Nach Bereitstellung einschalten) > **Fertig stellen** aus.
    * Wenn Sie eine weitere Netzwerkschnittstelle hinzufügen möchten, deaktivieren Sie **Power on after deployment** (Nach Bereitstellung einschalten), und wählen Sie dann **Fertig stellen** aus. Standardmäßig wird die Konfigurationsservervorlage mit einer einzelnen NIC bereitgestellt. Weitere NICs können nach der Bereitstellung hinzugefügt werden.

> [!IMPORTANT]
> Ändern Sie nach der Bereitstellung keine Ressourcenkonfigurationen wie Arbeitsspeicher, Kerne und CPU-Einschränkung, und ändern oder löschen Sie keine installierten Dienste oder Dateien auf dem Konfigurationsserver. Diese Arten von Änderungen wirken sich auf die Registrierung des Konfigurationsservers bei Azure-Diensten und die Leistung des Konfigurationsservers aus.

## <a name="add-an-additional-adapter"></a>Hinzufügen zusätzlicher Adapter

> [!NOTE]
> Wenn Sie die IP-Adressen der Quellcomputer bei einem Failover beibehalten und später ein Failback auf die lokale Umgebung durchführen möchten, sind zwei NICs erforderlich. Eine NIC wird mit Quellcomputern verbunden und die andere für die Azure-Konnektivität verwendet.

Wenn Sie dem Konfigurationsserver eine zusätzliche NIC hinzufügen möchten, führen Sie diesen Schritt aus, bevor Sie den Server im Tresor registrieren. Das Hinzufügen von zusätzlichen Adaptern nach der Registrierung wird nicht unterstützt.

1. Klicken Sie im vSphere-Client mit der rechten Maustaste auf den virtuellen Computer, und wählen Sie **Einstellungen bearbeiten** aus.
2. Klicken Sie unter **Hardware** auf **Hinzufügen** > **Ethernet-Adapter**. Klicken Sie anschließend auf **Weiter**.
3. Wählen Sie einen Adaptertyp und ein Netzwerk aus.
4. Um die virtuelle NIC zu verbinden, wenn die VM Computer eingeschaltet wird, wählen Sie **Connect at power-on** (Beim Einschalten verbinden) aus. Klicken Sie anschließend auf **Weiter** > **Fertig stellen** > **OK**.

## <a name="register-the-configuration-server-with-azure-site-recovery-services"></a>Registrieren des Konfigurationsservers bei Azure Site Recovery-Diensten

1. Schalten Sie den virtuellen Computer über die Konsole des VMware vSphere-Clients an.
2. Der virtuelle Computer wird mit der Benutzeroberfläche für die Installation von Windows Server 2016 hochgefahren. Akzeptieren Sie den Lizenzvertrag, und geben Sie ein Administratorkennwort ein.
3. Melden Sie sich nach Abschluss der Installation als Administrator am virtuellen Computer an.
4. Bei der ersten Anmeldung wird das Azure Site Recovery-Konfigurationstool innerhalb weniger Sekunden gestartet.
5. Geben Sie einen Namen ein, der für die Registrierung des Konfigurationsservers bei Site Recovery verwendet wird. Klicken Sie anschließend auf **Weiter**.
6. Das Tool überprüft, ob der virtuelle Computer eine Verbindung mit Azure herstellen kann. Klicken Sie nach der Verbindungsherstellung auf **Anmelden**, um sich bei Ihrem Azure-Abonnement anzumelden.</br>
    a. Die Anmeldeinformationen müssen über Zugriff auf den Tresor verfügen, in dem Sie den Konfigurationsserver registrieren möchten.</br>
    b. Stellen Sie sicher, dass das ausgewählte Benutzerkonto über die Berechtigung zum Erstellen einer Anwendung in Azure verfügt. Um die erforderlichen Berechtigungen zu aktivieren, befolgen Sie die Richtlinien im Abschnitt [Anforderungen für Azure Active Directory-Berechtigungen](#azure-active-directory-permission-requirements).
7. Das Tool führt einige Konfigurationsaufgaben und anschließend einen Neustart durch.
8. Melden Sie sich erneut am Computer an. Der Assistent für die Konfigurationsserververwaltung wird innerhalb weniger Sekunden automatisch gestartet.

### <a name="configure-settings"></a>Konfigurieren von Einstellungen

1. Wählen Sie im Assistent für die Konfigurationsserververwaltung die Option **Konnektivität einrichten**. Wählen Sie in den Dropdownfeldern zunächst den Netzwerkadapter aus, den der integrierte Prozessserver für die Ermittlung und die Pushinstallation des Mobilitätsdiensts auf Quellcomputern verwendet. Wählen Sie anschließend den Netzwerkadapter aus, den der Konfigurationsserver zum Herstellen einer Verbindung mit Azure nutzt. Wählen Sie **Speichern** aus. Diese Einstellung kann nach der Konfiguration nicht mehr geändert werden. Ändern Sie die IP-Adresse eines Konfigurationsservers nicht. Stellen Sie sicher, dass es sich bei der dem Konfigurationsserver zugewiesenen IP um eine statische IP und keine DHCP-IP handelt.
2. Melden Sie sich unter **Recovery Services-Tresor auswählen** bei Microsoft Azure mit den in Schritt 6 zum [Registrieren des Konfigurationsservers mit Azure Site Recovery Services](#register-the-configuration-server-with-azure-site-recovery-services) verwendeten Anmeldeinformationen an.
3. Wählen Sie nach der Anmeldung Ihr Azure-Abonnement, die entsprechende Ressourcengruppe und den Tresor aus.

    > [!NOTE]
    > Nach der Registrierung kann der Recovery Services-Tresor nicht geändert werden.
    > Das Ändern eines Recovery Services-Tresors erfordert eine Aufhebung der Zuordnung des Konfigurationsservers zum aktuellen Tresor und die Beendigung der Replikation aller geschützten virtuellen Computer unter dem Konfigurationsserver. Weitere Informationen finden Sie unter [Verwalten des Konfigurationsservers für die Notfallwiederherstellung von virtuellen VMware-Computern](vmware-azure-manage-configuration-server.md#register-a-configuration-server-with-a-different-vault).

4. Gehen Sie unter **Drittanbietersoftware installieren** wie folgt vor:

    |Szenario   |Vorgehensweise  |
    |---------|---------|
    |Kann ich MySQL manuell herunterladen und installieren?     |  Ja. Laden Sie die Anwendung MySQL herunter, legen Sie sie im Ordner **C:\Temp\ASRSetup** ab, und installieren Sie sie dann manuell. Nachdem Sie den Bedingungen zugestimmt haben, wählen Sie **Herunterladen und installieren** aus. Im Portal wird die Meldung *Bereits installiert* angezeigt. Sie können mit dem nächsten Schritt fortfahren.       |
    |Ist es möglich, das Herunterladen von MySQL über das Internet zu umgehen?     |   Ja. Platzieren Sie Ihr MySQL-Installationsprogramm im Ordner **C:\Temp\ASRSetup**. Stimmen Sie den Bedingungen zu, wählen Sie **Herunterladen und installieren** aus, und das Portal verwendet das Installationsprogramm, das Sie zum Installieren der Anwendung hinzugefügt haben. Fahren Sie nach Abschluss der Installation mit dem nächsten Schritt fort.    |
    |Ich möchte MySQL über Azure Site Recovery herunterladen und installieren.    |  Akzeptieren Sie den Lizenzvertrag, und wählen Sie **Herunterladen und installieren** aus. Fahren Sie nach Abschluss der Installation mit dem nächsten Schritt fort.       |

5. Unter **Anwendungskonfiguration überprüfen** werden die Voraussetzungen überprüft, bevor der Vorgang fortgesetzt wird.
6. Geben Sie unter **vCenter-Server/vSphere ESXi-Server konfigurieren** den FQDN oder die IP-Adresse des vCenter-Servers oder vSphere-Hosts ein, auf dem sich die virtuellen Computer befinden, die repliziert werden sollen. Geben Sie den Port ein, über den der Server lauscht. Geben Sie einen Anzeigenamen ein, der für den VMware-Server im Tresor verwendet werden soll.
7. Geben Sie die Anmeldeinformationen ein, die der Konfigurationsserver beim Herstellen der Verbindung mit dem VMware-Server verwenden soll. Site Recovery verwendet diese Anmeldeinformationen für die automatische Erkennung von VMware-VMs, die für die Replikation verfügbar sind. Wählen Sie **Hinzufügen** > **Weiter** aus. Die hier eingegebenen Anmeldeinformationen werden lokal gespeichert.
8. Geben Sie unter **Anmeldeinformationen für virtuelle Computer konfigurieren** den Benutzernamen und das Kennwort der virtuellen Computer an, um den Mobility Service während der Replikation automatisch installieren zu lassen. Für **Windows**-Computer benötigt das Konto lokale Administratorrechte auf den Computern, die Sie replizieren möchten. Bei **Linux** geben Sie die Anmeldeinformationen für das Stammkonto an.
9. Klicken Sie auf **Konfiguration abschließen**, um die Registrierung abzuschließen.
10. Nachdem die Registrierung abgeschlossen ist, öffnen Sie das Azure-Portal, und stellen Sie sicher, dass der Konfigurationsserver und der VMware-Server unter **Recovery Services-Tresor** > **Verwalten** > **Site Recovery-Infrastruktur** > **Konfigurationsserver** aufgeführt sind.

## <a name="upgrade-the-configuration-server"></a>Aktualisieren Sie den Konfigurationsserver

Informationen zum Aktualisieren des Konfigurationsservers auf die neueste Version finden Sie unter [Verwalten des Konfigurationsservers für die Notfallwiederherstellung von virtuellen VMware-Computern](vmware-azure-manage-configuration-server.md#upgrade-the-configuration-server). Anweisungen zum Upgrade aller Site Recovery-Komponenten finden Sie unter [Dienstupdates in Azure Site Recovery](service-updates-how-to.md).

## <a name="manage-the-configuration-server"></a>Verwalten des Konfigurationsservers

Um Unterbrechungen bei der laufenden Replikation zu vermeiden, stellen Sie sicher, dass sich die IP-Adresse des Konfigurationsservers nicht ändert, nachdem der Konfigurationsserver bei einem Tresor registriert wurde. Weitere Informationen zu allgemeinen Aufgaben zur Verwaltung des Konfigurationsservers finden Sie unter [Verwalten des Konfigurationsservers für die Notfallwiederherstellung von virtuellen VMware-Computern](vmware-azure-manage-configuration-server.md).

## <a name="faqs"></a>Häufig gestellte Fragen

* Wie lange ist die über OVF bereitgestellte Lizenz auf einem Konfigurationsserver gültig? Was geschieht, wenn ich die Lizenz nicht erneut aktiviere?

    Bei der mit einer OVA-Vorlage bereitgestellten Lizenz handelt es sich um eine Evaluierungslizenz mit einer Gültigkeit von 180 Tagen. Sie müssen die Lizenz vor dem Ablaufdatum aktivieren. Andernfalls kann dies zu einem häufigen Herunterfahren des Konfigurationsservers und damit zu einer Behinderung der Replikationsaktivitäten führen. Weitere Informationen finden Sie unter [Verwalten des Konfigurationsservers für die Notfallwiederherstellung von virtuellen VMware-Computern](vmware-azure-manage-configuration-server.md#update-windows-license).

* Kann ich die VM, auf der der Konfigurationsserver installiert ist, für andere Zwecke einsetzen?

    Nein. Verwenden Sie den virtuellen Computer nur für die Zwecke des Konfigurationsservers. Stellen Sie sicher, dass alle unter [Voraussetzungen](#prerequisites) genannten Spezifikationen für eine effiziente Verwaltung von Notfallwiederherstellungen eingehalten werden.
* Kann ich den Tresor, der bereits auf dem Konfigurationsserver registriert ist, mit einem neu erstellten Tresor austauschen?

    Nein. Nachdem ein Tresor für den Konfigurationsserver registriert wurde, sind daran keine Änderungen mehr möglich.
* Kann ich den gleichen Konfigurationsserver zum Schutz von physischen und virtuellen Computern verwenden?

    Ja. Der Konfigurationsserver kann zum Replizieren von physischen und virtuellen Computern verwendet werden. Allerdings kann der physische Computer nur auf eine VMware-VM zurückgesetzt werden.
* Welchen Zweck erfüllt ein Konfigurationsserver, und wo wird er eingesetzt?

    Weitere Informationen zum Konfigurationsserver und seinen Funktionen finden Sie unter [Architektur der Replikation von VMware zu Azure](vmware-azure-architecture.md).
* Wo erhalte ich die neueste Version des Konfigurationsservers?

    Schritte für ein Upgrade des Konfigurationsservers über das Portal finden Sie unter [Aktualisieren des Konfigurationsservers](vmware-azure-manage-configuration-server.md#upgrade-the-configuration-server). Anweisungen zum Upgrade aller Site Recovery-Komponenten finden Sie unter [Dienstupdates in Azure Site Recovery](https://aka.ms/asr_how_to_upgrade).
* Wo kann ich die Passphrase für den Konfigurationsserver herunterladen?

    Weitere Informationen zum Herunterladen der Passphrase finden Sie unter [Verwalten des Konfigurationsservers für die Notfallwiederherstellung von virtuellen VMware-Computern](vmware-azure-manage-configuration-server.md#generate-configuration-server-passphrase).
* Kann ich die Passphrase ändern?

    Nein. Ändern Sie die Passphrase des Konfigurationsservers nicht. Eine Änderung der Passphrase unterbricht die Replikation geschützter Computer und führt zu einem kritischen Integritätszustand.
* Wo kann ich die Tresorregistrierungsschlüssel herunterladen?

    Wählen Sie im **Recovery Services-Tresor** die Option **Verwalten** > **Site Recovery-Infrastruktur** > **Konfigurationsserver** aus. Klicken Sie unter **Server** auf **Registrierungsschlüssel herunterladen**, um die Datei mit den Tresoranmeldeinformationen herunterzuladen.
* Kann ich einen bestehenden Konfigurationsserver klonen und für die Replikationsorchestrierung verwenden?

    Nein. Die Verwendung einer geklonten Konfigurationsserverkomponente wird nicht unterstützt. Das Klonen des horizontal skalierten Prozessservers ist ebenfalls ein nicht unterstütztes Szenario. Das Klonen von Site Recovery-Komponenten wirkt sich auf die laufenden Replikationen aus.

* Kann ich die IP eines Konfigurationsservers ändern?

    Nein. Ändern Sie die IP-Adresse eines Konfigurationsservers nicht. Stellen Sie sicher, dass es sich bei allen dem Konfigurationsserver zugewiesenen IPs um statische IPs und keine DHCP-IPs handelt.
* Kann ich einen Konfigurationsserver in Azure einrichten?

    Richten Sie den Konfigurationsserver in der lokalen Umgebung mit einer direkten Sichtverbindung mit vCenter ein, um Wartezeiten bei der Datenübertragung zu minimieren. Sie können geplante Sicherungen des Konfigurationsservers für [Failbackzwecke](vmware-azure-manage-configuration-server.md#failback-requirements) erstellen.

Weitere FAQs zu Konfigurationsservern finden Sie unter [Häufig gestellte Fragen zu Konfigurationsservern](vmware-azure-common-questions.md#configuration-server).

## <a name="troubleshoot-deployment-issues"></a>Problembehandlung bei Bereitstellungsproblemen

[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]



## <a name="next-steps"></a>Nächste Schritte

[Einrichten der Notfallwiederherstellung in Azure für lokale VMware-VMs](vmware-azure-tutorial.md)
