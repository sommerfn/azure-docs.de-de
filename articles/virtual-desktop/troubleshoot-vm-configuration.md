---
title: Konfiguration des virtuellen Sitzungshostcomputers – Azure
description: Beheben von Problemen beim Konfigurieren von virtuellen Hostcomputern in einer Windows Virtual Desktop-Sitzung.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: troubleshooting
ms.date: 10/02/2019
ms.author: helohr
ms.openlocfilehash: 4c684a2db02b7587b6d81eaf2f034540250fc001
ms.sourcegitcommit: 15e3bfbde9d0d7ad00b5d186867ec933c60cebe6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/03/2019
ms.locfileid: "71841290"
---
# <a name="session-host-virtual-machine-configuration"></a>Konfiguration des virtuellen Sitzungshostcomputers

Verwenden Sie diesen Artikel, um Probleme zu behandeln, die bei der Konfiguration von virtuellen Computern (VMs) der Windows Virtual Desktop-Sitzung auftreten.

## <a name="provide-feedback"></a>Feedback geben

In der [Windows Virtual Desktop Tech Community](https://techcommunity.microsoft.com/t5/Windows-Virtual-Desktop/bd-p/WindowsVirtualDesktop) können Sie sich mit dem Produktteam und aktiven Communitymitgliedern über den Windows Virtual Desktop-Dienst austauschen.

## <a name="vms-are-not-joined-to-the-domain"></a>Virtuelle Computer werden der Domäne nicht hinzugefügt

Befolgen Sie diese Anweisungen, wenn Probleme beim Hinzufügen virtueller Computer zur Domäne auftreten.

- Fügen Sie die VM manuell über den unter [Hinzufügen eines virtuellen Windows Server-Computers zu einer verwalteten Domäne](https://docs.microsoft.com/azure/active-directory-domain-services/Active-directory-ds-admin-guide-join-windows-vm-portal) beschriebenen Prozess hinzu, oder verwenden Sie die [Vorlage für den Domänenbeitritt](https://azure.microsoft.com/resources/templates/201-vm-domain-join-existing/).
- Versuchen Sie, den Domänennamen über die Befehlszeile auf dem virtuellen Computer zu pingen.
- Überprüfen Sie die Liste der Fehlermeldungen zum Domänenbeitritt unter [Fehlermeldungen bei der Problembehandlung von Domänenbeitritten](https://social.technet.microsoft.com/wiki/contents/articles/1935.troubleshooting-domain-join-error-messages.aspx).

### <a name="error-incorrect-credentials"></a>Fehler Falsche Anmeldeinformationen

**Ursache:** Es gab einen Schreibfehler, als die Anmeldeinformationen in die Schnittstellenkorrekturen der Azure Resource Manager-Vorlage eingegeben wurden.

**Behebung:** Führen Sie eine der folgenden Aktionen aus, um dieses Problem zu beheben.

- Fügen Sie die virtuellen Computer einer Domäne manuell hinzu.
- Stellen Sie die Vorlage erneut bereit, nachdem die Anmeldeinformationen bestätigt wurden. Siehe [Erstellen eines Hostpools mit PowerShell](https://docs.microsoft.com/azure/virtual-desktop/create-host-pools-powershell).
- Fügen Sie einer Domäne VMs mithilfe einer Vorlage zum [Hinzufügen eines vorhandenen virtuellen Windows-Computers zu einer AD-Domäne](https://azure.microsoft.com/resources/templates/201-vm-domain-join-existing/) hinzu.

### <a name="error-timeout-waiting-for-user-input"></a>Fehler Timeout beim Warten auf eine Benutzereingabe.

**Ursache:** Das Konto, dar zum Abschließen des Domänenbeitritts verwendet wird, nutzt möglicherweise Multi-Factor Authentication (MFA).

**Behebung:** Führen Sie eine der folgenden Aktionen aus, um dieses Problem zu beheben.

- Entfernen Sie vorübergehend MFA für das Konto.
- Verwenden Sie ein Dienstkonto.

### <a name="error-the-account-used-during-provisioning-doesnt-have-permissions-to-complete-the-operation"></a>Fehler Das während der Bereitstellung verwendete Konto ist nicht berechtigt, den Vorgang abzuschließen.

**Ursache:** Das verwendete Konto besitzt aufgrund von Compliance und Vorschriften keine Berechtigungen, der Domäne VMs hinzuzufügen.

**Behebung:** Führen Sie eine der folgenden Aktionen aus, um dieses Problem zu beheben.

- Verwenden Sie ein Konto, das Mitglied der Gruppe „Administratoren“ ist.
- Erteilen Sie dem verwendeten Konto die erforderlichen Berechtigungen.

### <a name="error-domain-name-doesnt-resolve"></a>Fehler Der Domänenname wird nicht aufgelöst.

**Ursache 1:** VMs befinden sich in einem virtuellen Netzwerk, das nicht dem virtuellen Netzwerk (VNET) zugeordnet ist, in dem sich die Domäne befindet.

**Behebung 1:** Erstellen Sie VNET-Peering zwischen dem VNET, in dem VMs bereitgestellt wurden, und dem VNET, in dem der Domänencontroller (DC) ausgeführt wird. Siehe [Erstellen eines Peerings in virtuellen Netzwerken: Resource Manager, verschiedene Abonnements](https://docs.microsoft.com/azure/virtual-network/create-peering-different-subscriptions).

**Ursache 2:** Bei Verwendung von Azure Active Directory Domain Services (Azure AD DS) werden die DNS-Servereinstellungen für das virtuelle Netzwerk nicht so aktualisiert, dass sie auf die verwalteten Domänencontroller verweisen.

**Behebung 2:** Informationen zum Aktualisieren der DNS-Einstellungen für das virtuelle Netzwerk mit Azure AD DS finden Sie unter [Aktualisieren der DNS-Einstellungen für das virtuelle Azure-Netzwerk](https://docs.microsoft.com/azure/active-directory-domain-services/tutorial-create-instance#update-dns-settings-for-the-azure-virtual-network).

**Ursache 3:** Die DNS-Servereinstellungen der Netzwerkschnittstelle zeigen nicht auf den entsprechenden DNS-Server im virtuellen Netzwerk.

**Behebung 3:** Führen Sie eine der folgenden Aktionen aus, um das Problem zu beheben, und befolgen Sie dabei die Schritte unter [Ändern von DNS-Servern].
- Ändern Sie die DNS-Servereinstellungen der Netzwerkschnittstelle anhand der Schritte unter [Ändern von DNS-Servern](https://docs.microsoft.com/azure/virtual-network/virtual-network-network-interface#change-dns-servers) in **Benutzerdefiniert**, und geben Sie die privaten IP-Adressen der DNS-Server im virtuellen Netzwerk an.
- Ändern Sie die DNS-Servereinstellungen der Netzwerkschnittstelle über die Schritte unter [Ändern von DNS-Servern](https://docs.microsoft.com/azure/virtual-network/virtual-network-network-interface#change-dns-servers) so, dass sie **vom virtuellen Netzwerk geerbt werden**, und ändern Sie dann die DNS-Servereinstellungen des virtuellen Netzwerks anhand der Schritte unter [Ändern von DNS-Servern](https://docs.microsoft.com/azure/virtual-network/manage-virtual-network#change-dns-servers).

## <a name="windows-virtual-desktop-agent-and-windows-virtual-desktop-boot-loader-are-not-installed"></a>Der Windows Virtual Desktop-Agent und das Windows Virtual Desktop-Startladeprogramm sind nicht installiert.

Die empfohlene Methode zur Bereitstellung von VMs ist die Verwendung der Azure Resource Manager-Vorlage **Erstellen und Bereitstellen von Windows Virtual Desktop-Hostpools**. Die Vorlage installiert automatisch den Windows Virtual Desktop-Agent und das Windows Virtual Desktop-Agent-Startladeprogramm.

Befolgen Sie diese Anweisungen, um die Installation der Komponenten zu bestätigen und nach Fehlermeldungen zu suchen.

1. Vergewissern Sie sich, dass die beiden Komponenten installiert sind, indem Sie **Systemsteuerung** > **Programme** > **Programme und Funktionen** auswählen. Wenn **Windows Virtual Desktop-Agent** und das **Windows Virtual Desktop-Agent-Startladeprogramm** nicht angezeigt werden, sind sie auf der VM nicht installiert.
2. Öffnen Sie den **Datei-Explorer**, und navigieren Sie zu **C:\Windows\Temp\scriptlogs.log**. Wenn die Datei nicht vorhanden ist, zeigt dies an, dass die PowerShell-DSC, die die beiden Komponenten installiert hat, nicht in der Lage war, im bereitgestellten Sicherheitskontext ausgeführt zu werden.
3. Wenn die Datei **C:\Windows\Temp\scriptlogs.log** vorhanden ist, öffnen Sie sie, und suchen Sie nach Fehlermeldungen.

### <a name="error-windows-virtual-desktop-agent-and-windows-virtual-desktop-agent-boot-loader-are-missing-cwindowstempscriptlogslog-is-also-missing"></a>Fehler Der Windows Virtual Desktop-Agent und das Windows Virtual Desktop-Agent--Startladeprogramm sind nicht vorhanden. „C:\Windows\Temp\scriptlogs.log“ fehlt ebenfalls.

**Ursache 1:** Die bei der Eingabe für die Azure Resource Manager-Vorlage angegebenen Anmeldeinformationen waren falsch, oder die Berechtigungen waren unzureichend.

**Behebung 1:** Fügen Sie den VMs die fehlenden Komponenten manuell mithilfe von [Erstellen eines Hostpools mit PowerShell](https://docs.microsoft.com/azure/virtual-desktop/create-host-pools-powershell) hinzu.

**Ursache 2:** Die PowerShell-DSC konnte gestartet und ausgeführt, aber nicht abgeschlossen werden, da sie sich nicht bei Windows Virtual Desktop anmelden und die erforderlichen Informationen abrufen kann.

**Behebung 2:** Bestätigen Sie die einzelnen Punkte in der folgenden Liste.

- Stellen Sie sicher, dass das Konto keine MFA verwendet.
- Bestätigen Sie, dass der Mandantenname richtig und der Mandant in Windows Virtual Desktop vorhanden ist.
- Bestätigen Sie, dass das Konto mindestens über die Berechtigung „RDS-Mitwirkender“ verfügt.

### <a name="error-authentication-failed-error-in-cwindowstempscriptlogslog"></a>Fehler Authentifizierungsfehler, Fehler in „C:\Windows\Temp\scriptlogs.log“.

**Ursache:** Die PowerShell-DSC konnte ausgeführt werden, aber keine Verbindung mit Windows Virtual Desktop herstellen.

**Behebung:** Bestätigen Sie die einzelnen Punkte in der folgenden Liste.

- Registrieren Sie die virtuellen Computer manuell beim Windows Virtual Desktop-Dienst.
- Bestätigen Sie, dass das Konto, das für die Verbindung mit Windows Virtual Desktop verwendet wird, auf dem Mandanten über die Berechtigung verfügt, Hostpools zu erstellen.
- Bestätigen Sie, dass das Konto keine MFA verwendet.

## <a name="windows-virtual-desktop-agent-is-not-registering-with-the-windows-virtual-desktop-service"></a>Der Windows Virtual Desktop-Agent ist nicht beim Windows Virtual Desktop-Dienst registriert.

Wenn der Windows Virtual Desktop-Agent zum ersten Mal auf Sitzungshost-VMs installiert wird (entweder manuell oder über die Azure Resource Manager-Vorlage und PowerShell-DSC), stellt er ein Registrierungstoken bereit. Der folgende Abschnitt beschreibt die Behandlung von Problemen, die für den Windows Virtual Desktop-Agent und das Token gelten.

### <a name="error-the-status-filed-in-get-rdssessionhost-cmdlet-shows-status-as-unavailable"></a>Fehler Der Status im Cmdlet Get-RdsSessionHost wird als „Nicht verfügbar“ angegeben.

![Das Cmdlet Get-RdsSessionHost zeigt den Status als „Nicht verfügbar“ an.](media/23b8e5f525bb4e24494ab7f159fa6b62.png)

**Ursache:** Der Agent kann sich nicht selbst auf eine neue Version aktualisieren.

**Behebung:** Führen Sie diese Anweisungen manuell aus, um den Agent zu aktualisieren.

1. Laden Sie eine neue Version des Agents auf die Sitzungshost-VM herunter.
2. Starten Sie den Task-Manager, und beenden Sie auf der Registerkarte „Dienst“ den RDAgentBootLoader-Dienst.
3. Führen Sie den Installer für die neue Version des Windows Virtual Desktop-Agents aus.
4. Wenn Sie zur Eingabe des Registrierungstokens aufgefordert werden, entfernen Sie den Eintrag INVALID_TOKEN, und klicken Sie auf „Weiter“ (ein neues Token ist nicht erforderlich).
5. Führen Sie den Installations-Assistenten aus.
6. Öffnen Sie den Task-Manager, und starten Sie den RDAgentBootLoader-Dienst.

## <a name="error--windows-virtual-desktop-agent-registry-entry-isregistered-shows-a-value-of-0"></a>Fehler  Der Registrierungseintrag IsRegistered des Windows Virtual Desktop-Agents zeigt einen Wert von 0 an.

**Ursache:** Das Registrierungstoken ist abgelaufen oder wurde mit einem Ablaufwert von 999999 generiert.

**Behebung:** Befolgen Sie diese Anweisungen, um den Fehler in der Agent-Registrierung zu beheben.

1. Wenn bereits ein Registrierungstoken vorhanden ist, entfernen Sie es mit Remove-RDSRegistrationInfo.
2. Generieren Sie ein neues Token mit Rds-NewRegistrationInfo.
3. Bestätigen Sie, dass der Parameter -ExpriationHours auf 72 festgelegt ist (der Maximalwert ist 99999).

### <a name="error-windows-virtual-desktop-agent-isnt-reporting-a-heartbeat-when-running-get-rdssessionhost"></a>Fehler Der Windows Virtual Desktop-Agent meldet bei der Ausführung von Get-RdsSessionHost keinen Heartbeat.

**Ursache 1:** Der RDAgentBootLoader-Dienst wurde beendet.

**Behebung 1:** Starten Sie den Task-Manager, und starten Sie den Dienst, wenn die Registerkarte „Dienst“ einen beendeten Status für den RDAgentBootLoader-Dienst meldet.

**Ursache 2:** Port 443 ist ggf. geschlossen.

**Behebung 2:** Befolgen Sie diese Anweisungen, um Port 443 zu öffnen.

1. Bestätigen Sie, dass Port 443 geöffnet ist, indem Sie das PSPing-Tool von [Sysinternal Tools](https://docs.microsoft.com/sysinternals/downloads/psping) herunterladen.
2. Installieren Sie PSPing auf der Sitzungshost-VM, auf der der Agent ausgeführt wird.
3. Öffnen Sie die Eingabeaufforderung als Administrator, und geben Sie den unten gezeigten Befehl aus:

    ```cmd
    psping rdbroker.wvdselfhost.microsoft.com:443
    ```

4. Bestätigen Sie, das PSPing Informationen vom RDBroker zurückerhalten hat:

    ```
    PsPing v2.10 - PsPing - ping, latency, bandwidth measurement utility
    Copyright (C) 2012-2016 Mark Russinovich
    Sysinternals - www.sysinternals.com
    TCP connect to 13.77.160.237:443:
    5 iterations (warmup 1) ping test:
    Connecting to 13.77.160.237:443 (warmup): from 172.20.17.140:60649: 2.00ms
    Connecting to 13.77.160.237:443: from 172.20.17.140:60650: 3.83ms
    Connecting to 13.77.160.237:443: from 172.20.17.140:60652: 2.21ms
    Connecting to 13.77.160.237:443: from 172.20.17.140:60653: 2.14ms
    Connecting to 13.77.160.237:443: from 172.20.17.140:60654: 2.12ms
    TCP connect statistics for 13.77.160.237:443:
    Sent = 4, Received = 4, Lost = 0 (0% loss),
    Minimum = 2.12ms, Maximum = 3.83ms, Average = 2.58ms
    ```

## <a name="troubleshooting-issues-with-the-windows-virtual-desktop-side-by-side-stack"></a>Problembehandlung mit dem parallelen Stapel für Windows Virtual Desktop

Der parallele Stapel für Windows Virtual Desktop wird automatisch mit Windows Server 2019 installiert. Verwenden Sie Microsoft Installer (MSI), um den parallelen Stapel unter Microsoft Windows Server 2016 oder Windows Server 2012 R2 zu installieren. Für Microsoft Windows 10 wird der parallele Stapel für Windows Virtual Desktop mit **enablesxstackrs.ps1** aktiviert.

Es gibt drei Hauptmethoden, wie der parallele Stapel auf Sitzungshostpool-VMs installiert oder aktiviert wird:

- Mit der Azure Resource Manager-Vorlage **Erstellen und Bereitstellen von Windows Virtual Desktop-Hostpools**.
- Durch Einbinden in das und Aktivieren im Masterimage.
- Durch manuelles Installieren oder Aktivieren auf jeder VM (oder mit Erweiterungen/PowerShell).

Wenn Sie Probleme mit dem parallelen Stapel von Windows Virtual Desktop haben, geben Sie den Befehl **qwinsta** an der Eingabeaufforderung ein, um zu bestätigen, dass der parallele Stapel installiert oder aktiviert ist.

Die Ausgabe von **qwinsta** listet **rdp-sxs** in der Ausgabe auf, wenn der parallele Stapel installiert und aktiviert ist.

![Paralleler Stapel installiert oder aktiviert, qwinsta-Ausgabe mit rdp-sxs.](media/23b8e5f525bb4e24494ab7f159fa6b62.png)

Untersuchen Sie die unten aufgeführten Registrierungseinträge und bestätigen Sie, dass ihre Werte übereinstimmen. Wenn Registrierungsschlüssel fehlen oder Werte nicht übereinstimmen, befolgen Sie die Anweisungen unter [Erstellen eines Hostpools mit PowerShell](https://docs.microsoft.com/azure/virtual-desktop/create-host-pools-powershell) zum erneuten Installieren des parallelen Stapels.

```registry
    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal
    Server\WinStations\rds-sxs\"fEnableWinstation":DWORD=1

    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal
    Server\ClusterSettings\"SessionDirectoryListener":rdp-sxs
```

### <a name="error-o_reverse_connect_stack_failure"></a>Fehler O_REVERSE_CONNECT_STACK_FAILURE

![O_REVERSE_CONNECT_STACK_FAILURE-Fehlercode.](media/23b8e5f525bb4e24494ab7f159fa6b62.png)

**Ursache:** Der parallele Stapel ist nicht auf der Sitzungshost-VM installiert.

**Behebung:** Befolgen Sie diese Anweisungen, um den parallelen Stapel auf der Sitzungshost-VM zu installieren.

1. Verwenden Sie RDP (Remote Desktop Protocol), um als lokaler Administrator direkt in die Sitzungshost-VM zu gelangen.
2. Laden Sie bei Bedarf das [Windows Virtual Desktop-PowerShell-Modul](https://docs.microsoft.com/powershell/windows-virtual-desktop/overview) herunter, und importieren Sie es, um es in Ihrer PowerShell-Sitzung verwenden zu können. Führen Sie anschließend das folgende Cmdlet aus, um sich bei Ihrem Konto anzumelden:
    
    ```powershell
    Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
    ```
    
3. Installieren Sie den parallelen Stapel mithilfe von [Erstellen eines Hostpools mit PowerShell](https://docs.microsoft.com/azure/virtual-desktop/create-host-pools-powershell).

## <a name="how-to-fix-a-windows-virtual-desktop-side-by-side-stack-that-malfunctions"></a>Beheben der Fehlfunktion eines parallelen Stapels für Windows Virtual Desktop

Es sind Umstände bekannt, die zu einer Fehlfunktion des parallelen Stapels führen können:

- Nichtbefolgen der richtigen Reihenfolge der Schritte zum Aktivieren des parallelen Stapels
- Automatisches Update auf Windows 10 Enhanced Versatile Disc (EVD)
- Fehlende RDSH-Rolle (Remotedesktop-Sitzungshost)
- Mehrmaliges Ausführen von Enablesxsstackrc.ps1
- Ausführen von Enablesxsstackrc.ps1 in einem Konto, das nicht über lokale Administratorrechte verfügt

Die Anweisungen in diesem Abschnitt können Ihnen helfen, den parallelen Stapel für Windows Virtual Desktop zu deinstallieren. Sobald Sie den parallelen Stapel deinstalliert haben, navigieren Sie zu „Registrieren der VM beim Windows Virtual Desktop-Hostpool“ in [Erstellen eines Hostpools mit PowerShell](https://docs.microsoft.com/azure/virtual-desktop/create-host-pools-powershell), um den parallelen Stapel erneut zu installieren.

Die VM, mit der die Wiederherstellung durchgeführt wird, muss sich im gleichen Subnetz und in derselben Domäne befinden wie die VM mit dem fehlerhaften parallelen Stapel.

Befolgen Sie diese Anweisungen, um die Fehlerbehebung aus demselben Subnetz und derselben Domäne durchzuführen:

1. Stellen Sie eine Verbindung mit der VM mit dem RDP-Standardprotokoll (Remote Desktop Protocol) her, von der aus die Fehlerkorrektur angewendet wird.
2. Laden Sie PsExec von https://docs.microsoft.com/sysinternals/downloads/psexec herunter.
3. Extrahieren Sie die heruntergeladene Datei.
4. Starten Sie eine Eingabeaufforderung als lokaler Administrator.
5. Navigieren Sie zu dem Ordner, in den PsExec extrahiert wurde.
6. Verwenden Sie in der Eingabeaufforderung den folgenden Befehl:

    ```cmd
            psexec.exe \\<VMname> cmd
    ```

    >[!Note]
    >VMname ist der Computername des virtuellen Computers mit dem fehlerhaften parallelen Stapel.

7. Stimmen Sie dem PsExec-Lizenzvertrag zu, indem Sie auf „Agree“ (Ich stimme zu) klicken.

    ![Screenshot: Softwarelizenzvereinbarung.](media/SoftwareLicenseTerms.png)

    >[!Note]
    >Dieses Dialogfeld wird nur bei der ersten Ausführung von PsExec angezeigt.

8. Nachdem die Eingabeaufforderungssitzung auf der VM mit dem fehlerhaften parallelen Stapel geöffnet wurde, führen Sie qwinsta aus und bestätigen, dass ein Eintrag namens rdp-sxs verfügbar ist. Wenn dies nicht der Fall ist, ist auf der VM kein paralleler Stapel vorhanden, sodass das Problem nicht durch den parallelen Stapel verursacht wird.

    ![Administratoreingabeaufforderung](media/AdministratorCommandPrompt.png)

9. Führen Sie den folgenden Befehl aus, der die auf der VM installierten Microsoft-Komponenten mit dem fehlerhaften parallelen Stapel auflistet.

    ```cmd
        wmic product get name
    ```

10. Führen Sie den folgenden Befehl mit Produktnamen aus dem vorherigen Schritt aus.

    ```cmd
        wmic product where name="<Remote Desktop Services Infrastructure Agent>" call uninstall
    ```

11. Deinstallieren Sie alle Produkte, die mit „Remotedesktop“ beginnen.

12. Nachdem alle Windows Virtual Desktop-Komponenten deinstalliert wurden, führen Sie die Anweisungen für Ihr Betriebssystem aus:

13. Wenn Ihr Betriebssystem Windows Server ist, starten Sie die VM neu, die den fehlerhaften parallelen Stapel aufwies (entweder mit dem Azure-Portal oder mit dem PsExec-Tool).

Wenn das Betriebssystem Microsoft Windows 10 ist, fahren Sie mit den folgenden Anweisungen fort:

14. Öffnen Sie auf der VM, die PsExec ausführt, den Datei-Explorer, und kopieren Sie „disablesxsstackrc.ps1“ auf das Systemlaufwerk der VM mit dem fehlerhaften parallelen Stapel.

    ```cmd
        \\<VMname>\c$\
    ```

    >[!NOTE]
    >VMname ist der Computername des virtuellen Computers mit dem fehlerhaften parallelen Stapel.

15. Empfohlene Vorgehensweise: Starten Sie PowerShell aus dem PsExec-Tool, und navigieren Sie zum Ordner aus dem vorherigen Schritt. Führen Sie dann „disablesxsstackrc.ps1“ aus. Alternativ können Sie die folgenden Cmdlets ausführen:

    ```PowerShell
    Remove-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\ClusterSettings" -Name "SessionDirectoryListener" -Force
    Remove-Item -Path "HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\rdp-sxs" -Recurse -Force
    Remove-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations" -Name "ReverseConnectionListener" -Force
    ```

16. Wenn die Ausführung der Cmdlets abgeschlossen ist, starten Sie die VM mit dem fehlerhaften parallelen Stapel neu.

## <a name="remote-licensing-model-isnt-configured"></a>Remotelizenzierungsmodell ist nicht konfiguriert

Wenn Sie sich bei Windows 10 Enterprise (mehrere Sitzungen) mit einem Administratorkonto anmelden, erhalten Sie möglicherweise folgende Benachrichtigung: „Der Remotedesktop-Lizenzierungsmodus ist nicht konfiguriert. Die Remotedesktopdienste können in X Tagen nicht mehr ausgeführt werden. Geben Sie mit dem Server-Manager auf dem Verbindungsbrokerserver einen Remotedesktop-Lizenzierungsmodus an.“

Wenn das Zeitlimit abgelaufen ist, wird die folgende Fehlermeldung angezeigt: „Die Verbindung mit der Remotesitzung wurde getrennt, da keine Remotedesktop-Clientzugriffslizenzen für diesen Computer vorhanden sind.“

Wenn eine dieser Meldungen angezeigt wird, müssen Sie den Gruppenrichtlinien-Editor öffnen und den Lizenzierungsmodus manuell auf **Pro Benutzer** festlegen. Der manuelle Konfigurationsprozess ist je nachdem, welche Version von Windows 10 Enterprise (mehrere Sitzungen) Sie verwenden, unterschiedlich. In den folgenden Abschnitten wird erläutert, wie Sie die Versionsnummer überprüfen, und welche Aktionen für jede Version auszuführen sind.

>[!NOTE]
>Windows Virtual Desktop erfordert nur dann eine RDS-Clientzugriffslizenz (Client Access License, CAL), wenn Ihr Hostpool Windows Server-Sitzungshosts enthält. Informationen zum Konfigurieren einer RDS-CAL finden Sie unter [Lizenzieren deiner RDS-Bereitstellung mit Clientzugriffslizenzen (CALs)](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/rds-client-access-license).

### <a name="identify-which-version-of-windows-10-enterprise-multi-session-youre-using"></a>Ermitteln, welche Version von Windows 10 Enterprise (mehrere Sitzungen) verwendet wird

Gehen Sie wie folgt vor, um zu ermitteln, mit welcher Version von Windows 10 Enterprise (mehrere Sitzungen) Sie arbeiten:

1. Melden Sie sich mit Ihrem Administratorkonto an.
2. Geben Sie „Info“ in die Suchleiste neben dem Startmenü ein.
3. Wählen Sie **PC-Infos** aus.
4. Überprüfen Sie die Nummer neben „Version“. Die Nummer muss entweder „1809“ oder „1903“ lauten, wie in der folgenden Abbildung dargestellt.
   
    ![Screenshot des Fensters für Windows-Spezifikationen Die Versionsnummer ist blau hervorgehoben.](media/windows-specifications.png)

Nun, da Sie die Versionsnummer wissen, wechseln Sie zu dem entsprechenden Abschnitt.

### <a name="version-1809"></a>Version 1809

Wenn die Versionsnummer „1809“ lautet, können Sie entweder auf Windows 10 Enterprise (mehrere Sitzungen), Version 1903, aktualisieren oder den Hostpool mit dem neuesten Image erneut bereitstellen.

So aktualisieren Sie auf Windows 10, Version 1903

1. Sofern noch nicht geschehen, laden Sie das [Windows-Update vom 10. Mai 2019](https://support.microsoft.com/help/4028685/windows-10-get-the-update) herunter, und installieren Sie es.
2. Melden Sie sich mit Ihrem Administratorkonto bei Ihrem Computer an.
3. Führen Sie **gpedit.msc** aus, um den Gruppenrichtlinien-Editor zu öffnen.
4. Navigieren Sie unter „Computerkonfiguration“ zu **Administrative Vorlagen** > **Windows-Komponenten** > **Remotedesktopdienste** > **Remotedesktopsitzungs-Host** > **Lizenzierung**.
5. Wählen Sie **Remotedesktop-Lizenzierungsmodus festlegen** aus.
6. Wählen Sie im daraufhin angezeigten Fenster zuerst die Option **Aktiviert** aus, und geben Sie dann unter „Optionen“ den Lizenzierungsmodus für den RD-Sitzungshostserver als **Pro Benutzer** an, wie in der folgenden Abbildung dargestellt.
    
    ![Screenshot des Fensters „Remotedesktop-Lizenzierungsmodus festlegen“ mit Konfigurationseinstellungen gemäß Schritt 6](media/group-policy-editor-per-user.png)

7. Wählen Sie **Übernehmen**.
8. Klicken Sie auf **OK**.
9.  Starten Sie den Computer neu.

So stellen Sie den Hostpool mit dem neuesten Image erneut bereit

1. Führen Sie die unter [Erstellen eines Hostpools mit dem Azure Marketplace](create-host-pools-azure-marketplace.md) aufgeführten Schritte aus, bis Sie zur Auswahl der Betriebssystemversion des Image aufgefordert werden. Sie können Windows 10 Enterprise (mehrere Sitzungen) mit oder ohne Office365 ProPlus auswählen.
2. Melden Sie sich mit Ihrem Administratorkonto bei Ihrem Computer an.
3. Führen Sie **gpedit.msc** aus, um den Gruppenrichtlinien-Editor zu öffnen.
4. Navigieren Sie unter „Computerkonfiguration“ zu **Administrative Vorlagen** > **Windows-Komponenten** > **Remotedesktopdienste** > **Remotedesktopsitzungs-Host** > **Lizenzierung**.
5. Wählen Sie **Remotedesktop-Lizenzierungsmodus festlegen** aus.
6. Wählen Sie im daraufhin angezeigten Fenster zuerst die Option **Aktiviert** aus, und geben Sie dann unter „Optionen“ den Lizenzierungsmodus für den RD-Sitzungshostserver als **Pro Benutzer** an.
7. Wählen Sie **Übernehmen**.
8. Klicken Sie auf **OK**.
9.  Starten Sie den Computer neu.

### <a name="version-1903"></a>Version 1903

Wenn die Versionsnummer „1903“ lautet, gehen Sie wie folgt vor:

1. Melden Sie sich mit Ihrem Administratorkonto bei Ihrem Computer an.
2. Führen Sie **gpedit.msc** aus, um den Gruppenrichtlinien-Editor zu öffnen.
3. Navigieren Sie unter „Computerkonfiguration“ zu **Administrative Vorlagen** > **Windows-Komponenten** > **Remotedesktopdienste** > **Remotedesktopsitzungs-Host** > **Lizenzierung**.
4. Wählen Sie **Remotedesktop-Lizenzierungsmodus festlegen** aus.
6. Wählen Sie im daraufhin angezeigten Fenster zuerst die Option **Aktiviert** aus, und geben Sie dann unter „Optionen“ den Lizenzierungsmodus für den RD-Sitzungshostserver als **Pro Benutzer** an, wie in der folgenden Abbildung dargestellt.
    
    ![Screenshot des Fensters „Remotedesktop-Lizenzierungsmodus festlegen“ mit Konfigurationseinstellungen gemäß Schritt 6](media/group-policy-editor-per-user.png)

7. Wählen Sie **Übernehmen**.
8. Klicken Sie auf **OK**.
9.  Starten Sie den Computer neu.

## <a name="next-steps"></a>Nächste Schritte

- Eine Übersicht über die Problembehandlung von Windows Virtual Desktop und die Eskalationspfade finden Sie unter [Überblick über Problembehandlung, Feedback und Support](troubleshoot-set-up-overview.md).
- Informationen zur Problembehandlung beim Erstellen eines Mandanten- und Hostpools in einer Windows Virtual Desktop-Umgebung finden Sie unter [Mandanten- und Hostpoolerstellung](troubleshoot-set-up-issues.md).
- Informationen zur Problembehandlung bei der Konfiguration eines virtuellen Computers (VM) in Windows Virtual Desktop finden Sie unter [Konfiguration des virtuellen Sitzungshostcomputers](troubleshoot-vm-configuration.md).
- Informationen zur Problembehandlung bei Problemen mit Windows Virtual Desktop-Clientverbindungen finden Sie unter [Remotedesktop-Clientverbindungen](troubleshoot-client-connection.md).
- Informationen zur Problembehandlung bei der Verwendung von PowerShell mit Windows Virtual Desktop finden Sie unter [Windows Virtual Desktop – PowerShell](troubleshoot-powershell.md).
- Weitere Informationen zum Dienst finden Sie unter [Windows Virtual Desktop-Umgebung](https://docs.microsoft.com/azure/virtual-desktop/environment-setup).
- Ein Tutorial zur Problembehandlung finden Sie unter [Tutorial: Problembehandlung von Bereitstellungen der Resource Manager-Vorlage](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-tutorial-troubleshoot).
- Informationen zur Überwachung von Aktionen finden Sie unter [Überwachen von Vorgängen mit Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-audit).
- Weitere Informationen zu Aktionen zum Bestimmen von Fehlern während der Bereitstellung finden Sie unter [Anzeigen von Bereitstellungsvorgängen mit dem Azure-Portal](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-operations).
