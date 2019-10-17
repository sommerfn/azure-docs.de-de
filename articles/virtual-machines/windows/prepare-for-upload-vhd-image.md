---
title: Vorbereiten einer Windows-VHD für das Hochladen in Azure | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine Windows-VHD oder -VHDX für das Hochladen in Azure vorbereiten.
services: virtual-machines-windows
documentationcenter: ''
author: glimoli
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.assetid: 7802489d-33ec-4302-82a4-91463d03887a
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 05/11/2019
ms.author: genli
ms.openlocfilehash: 555b250f211cf22e766e64960b3359692f73c843
ms.sourcegitcommit: e0a1a9e4a5c92d57deb168580e8aa1306bd94723
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2019
ms.locfileid: "72285717"
---
# <a name="prepare-a-windows-vhd-or-vhdx-to-upload-to-azure"></a>Vorbereiten einer Windows-VHD oder -VHDX zum Hochladen in Azure

Bevor Sie einen virtuellen Windows-Computer aus einem lokalen Speicherort in Azure hochladen können, müssen Sie die virtuelle Festplatte (Virtual Hard Disk, VHD oder VHDX) vorbereiten. Azure unterstützt VMs der Generationen 1 und 2, die das VHD-Dateiformat und einen Datenträger mit fester Größe aufweisen. Die maximal zulässige Größe für die virtuelle Festplatte beträgt 1.023 GB. 

Bei einer VM der Generation 1 können Sie das VHDX-Dateisystem in VHD konvertieren. Sie können auch einen dynamisch erweiterbaren Datenträger in einen Datenträger mit fester Größe konvertieren. Aber die Generation eines virtuellen Computers kann nicht geändert werden. Weitere Informationen finden Sie unter [Sollte ich eine VM der Generation 1 oder 2 in Hyper-V erstellen?](https://technet.microsoft.com/windows-server-docs/compute/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v) und [Azure-Support für VMs der Generation 2 (Vorschauversion)](generation-2.md).

Weitere Informationen zur Supportrichtlinie für Azure-VMs finden Sie unter [Microsoft-Server-Software-Support für virtuelle Maschinen von Microsoft Azure](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines).

> [!NOTE]
> Die Anweisungen in diesem Artikel gelten für Folgendes:
>1. Die 64-Bit-Version von Windows Server 2008 R2 und höhere Versionen des Windows Server-Betriebssystems. Informationen zum Ausführen einer 32-Bit-Version des Betriebssystems in Azure finden Sie unter [Unterstützung für 32-Bit-Betriebssysteme in Azure-VMs](https://support.microsoft.com/help/4021388/support-for-32-bit-operating-systems-in-azure-virtual-machines).
>2. Wird die Workload mithilfe eines Notfallwiederherstellungstools wie Azure Site Recovery oder Azure Migrate migriert, muss dieser Prozess trotzdem für das Gastbetriebssystem ausgeführt werden, um das Image vor der Migration vorzubereiten.

## <a name="convert-the-virtual-disk-to-a-fixed-size-and-to-vhd"></a>Konvertieren des virtuellen Datenträgers in einen Datenträger mit fester Größe und eine VHD

Wenn Sie Ihren virtuellen Datenträger in das für Azure erforderliche Format konvertieren müssen, verwenden Sie eine der in diesem Abschnitt beschriebenen Methoden:

1. Sichern Sie den virtuellen Computer, bevor Sie den Prozess zum Konvertieren virtueller Datenträger ausführen.

1. Vergewissern Sie sich, dass die Windows-VHD auf dem lokalen Server ordnungsgemäß funktioniert. Beheben Sie alle Probleme auf dem virtuellen Computer selbst, bevor Sie versuchen, ihn zu konvertieren oder in Azure hochzuladen.

1. Beachten Sie im Zusammenhang mit der VHD-Größe Folgendes:

   1. Alle VHDs in Azure benötigen eine virtuelle Größe, die auf 1 MB ausgerichtet ist. Stellen Sie beim Konvertieren von einem RAW-Datenträger in VHD sicher, dass die Größe des RAW-Datenträgers vor der Konvertierung ein Vielfaches von 1 MB beträgt. Wenn keine vollen Megabytes verwendet werden, treten beim Erstellen von Images auf der Grundlage der hochgeladenen VHD Fehler auf.

   2. Die maximal zulässige Größe für die Betriebssystem-VHD beträgt 2 TB.


Nachdem Sie den Datenträger konvertiert haben, erstellen Sie einen virtuellen Computer, der den Datenträger verwendet. Starten Sie den virtuellen Computer, und melden Sie sich auf diesem an, um die Vorbereitung für den Upload abzuschließen.

### <a name="use-hyper-v-manager-to-convert-the-disk"></a>Verwenden von Hyper-V-Manager zum Konvertieren des Datenträgers 
1. Öffnen Sie den Hyper-V-Manager, und wählen Sie auf der linken Seite Ihren lokalen Computer aus. Wählen Sie im Menü über der Computerliste **Aktion** > **Datenträger bearbeiten** aus.
2. Wählen Sie auf der Seite **Virtuelle Festplatte suchen** Ihren virtuellen Datenträger aus.
3. Wählen Sie auf der Seite **Aktion auswählen** die Optionen **Konvertieren** > **Weiter** aus.
4. Wenn Sie eine VHDX konvertieren müssen, wählen Sie **VHD** > **Weiter** aus.
5. Wenn Sie einen dynamisch erweiterbaren Datenträger konvertieren müssen, wählen Sie **Feste Größe** > **Weiter** aus.
6. Navigieren Sie zu einem Pfad zum Speichern der neuen VHD-Datei, und wählen Sie ihn aus.
7. Wählen Sie **Fertig stellen** aus.

> [!NOTE]
> Die Befehle in diesem Artikel müssen in einer PowerShell-Sitzung mit erhöhten Rechten ausgeführt werden.

### <a name="use-powershell-to-convert-the-disk"></a>Verwenden von PowerShell zum Konvertieren des Datenträgers 
Sie können einen virtuellen Datenträger mithilfe des Befehls [Convert-VHD](https://technet.microsoft.com/library/hh848454.aspx) in Windows PowerShell konvertieren. Wählen Sie beim Starten von PowerShell **Als Administrator ausführen**. 

Im folgenden Beispiel wird der Datenträger von VHDX in VHD konvertiert. Der Befehl konvertiert auch einen dynamisch erweiterbaren Datenträger in einen Datenträger mit fester Größe.

```Powershell
Convert-VHD –Path c:\test\MY-VM.vhdx –DestinationPath c:\test\MY-NEW-VM.vhd -VHDType Fixed
```

Ersetzen Sie in diesem Befehl den Wert für `-Path` durch den Pfad zu der virtuellen Festplatte, die Sie konvertieren möchten. Ersetzen Sie den Wert für `-DestinationPath` durch den neuen Pfad und Namen für den konvertierten Datenträger.

### <a name="convert-from-vmware-vmdk-disk-format"></a>Konvertieren des VMware-VMDK-Datenträgerformats
Wenn Sie über ein Windows-VM-Image im [VMDK-Dateiformat](https://en.wikipedia.org/wiki/VMDK) verfügen, konvertieren Sie es mit dem [Microsoft Virtual Machine Converter](https://www.microsoft.com/download/details.aspx?id=42497) in das VHD-Format. Weitere Informationen finden Sie unter [How to Convert a VMWare VMDK to Hyper-V VHD](https://blogs.msdn.com/b/timomta/archive/2015/06/11/how-to-convert-a-vmware-vmdk-to-hyper-v-vhd.aspx) (Konvertieren von VMware-VMDK in Hyper-V-VHD).

## <a name="set-windows-configurations-for-azure"></a>Festlegen der Windows-Konfigurationen für Azure

Führen Sie die folgenden Befehle über eine [Eingabeaufforderung mit erhöhten Rechten](https://technet.microsoft.com/library/cc947813.aspx) auf dem virtuellen Computer aus, den Sie in Azure hochladen möchten:

1. Entfernen Sie alle statischen persistenten Routen aus der Routingtabelle:
   
   * Führen Sie zum Anzeigen der Routingtabelle `route print` über die Eingabeaufforderung aus.
   * Überprüfen Sie die `Persistence Routes`-Abschnitte. Wenn eine persistente Route vorhanden ist, verwenden Sie den Befehl `route delete`, um sie zu entfernen.
2. Entfernen Sie den WinHTTP-Proxy:
   
    ```PowerShell
    netsh winhttp reset proxy
    ```

    Wenn der virtuelle Computer mit einem bestimmten Proxy arbeiten muss, fügen Sie der Azure-IP-Adresse eine Proxyausnahme hinzu ([168.63.129.16](https://blogs.msdn.microsoft.com/mast/2015/05/18/what-is-the-ip-address-168-63-129-16/
)), damit der virtuelle Computer eine Verbindung mit Azure herstellen kann:
    ```
    $proxyAddress="<your proxy server>"
    $proxyBypassList="<your list of bypasses>;168.63.129.16"

    netsh winhttp set proxy $proxyAddress $proxyBypassList
    ```

3. Legen Sie für die Datenträger-SAN-Richtlinie [`Onlineall`](https://technet.microsoft.com/library/gg252636.aspx) fest:
   
    ```PowerShell
    diskpart 
    ```
    Geben Sie im geöffneten Eingabeaufforderungsfenster die folgenden Befehle ein:

     ```DISKPART
    san policy=onlineall
    exit   
    ```

4. Legen Sie für Windows die koordinierten Weltzeit (UTC) fest. Legen Sie außerdem den Starttyp des Windows-Zeitdiensts (`w32time`) auf `Automatic` fest:
   
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\TimeZoneInformation' -Name "RealTimeIsUniversal" -Value 1 -Type DWord -Force

    Set-Service -Name w32time -StartupType Automatic
    ```
5. Legen Sie das Energieprofil auf „Hohe Leistung“ fest:

    ```PowerShell
    powercfg /setactive SCHEME_MIN
    ```
6. Stellen Sie sicher, dass die Umgebungsvariablen `TEMP` und `TMP` auf ihre Standardwerte festgelegt sind:

    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\Environment' -Name "TEMP" -Value "%SystemRoot%\TEMP" -Type ExpandString -Force

    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\Environment' -Name "TMP" -Value "%SystemRoot%\TEMP" -Type ExpandString -Force
    ```

## <a name="check-the-windows-services"></a>Überprüfen der Windows-Dienste
Stellen Sie sicher, dass jeder der folgenden Windows-Dienste auf die Windows-Standardwerte festgelegt ist. Die Einrichtung dieser Dienste stellt die Mindestanforderung für VM-Konnektivität dar. Um die Starteinstellungen zurückzusetzen, führen Sie die folgenden Befehle aus:
   
```PowerShell
Get-Service -Name bfe | Where-Object { $_.StartType -ne 'Automatic' } | Set-Service -StartupType 'Automatic'
Get-Service -Name dhcp | Where-Object { $_.StartType -ne 'Automatic' } | Set-Service -StartupType 'Automatic'
Get-Service -Name dnscache | Where-Object { $_.StartType -ne 'Automatic' } | Set-Service -StartupType 'Automatic'
Get-Service -Name IKEEXT | Where-Object { $_.StartType -ne 'Automatic' } | Set-Service -StartupType 'Automatic'
Get-Service -Name iphlpsvc | Where-Object { $_.StartType -ne 'Automatic' } | Set-Service -StartupType 'Automatic'
Get-Service -Name netlogon | Where-Object { $_.StartType -ne 'Manual' } | Set-Service -StartupType 'Manual'
Get-Service -Name netman | Where-Object { $_.StartType -ne 'Manual' } | Set-Service -StartupType 'Manual'
Get-Service -Name nsi | Where-Object { $_.StartType -ne 'Automatic' } | Set-Service -StartupType 'Automatic'
Get-Service -Name TermService | Where-Object { $_.StartType -ne 'Manual' } | Set-Service -StartupType 'Manual'
Get-Service -Name MpsSvc | Where-Object { $_.StartType -ne 'Automatic' } | Set-Service -StartupType 'Automatic'
Get-Service -Name RemoteRegistry | Where-Object { $_.StartType -ne 'Automatic' } | Set-Service -StartupType 'Automatic'
```

## <a name="update-remote-desktop-registry-settings"></a>Aktualisieren der Remotedesktop-Registrierungseinstellungen
Stellen Sie sicher, dass die folgenden Einstellungen ordnungsgemäß für Remotezugriff konfiguriert sind:

>[!NOTE] 
>Sie erhalten möglicherweise eine Fehlermeldung, wenn Sie `Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services -Name <object name> -Value <value>` ausführen. Diese Meldung können Sie problemlos ignorieren. Sie gibt lediglich an, dass diese Konfiguration von der Domäne nicht über ein Gruppenrichtlinienobjekt gepusht wird.

1. Das Remotedesktopprotokoll (RDP) ist aktiviert:
   
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server' -Name "fDenyTSConnections" -Value 0 -Type DWord -Force

    Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services' -Name "fDenyTSConnections" -Value 0 -Type DWord -Force
    ```
   
2. Der RDP-Port ist ordnungsgemäß festgelegt. Der Standardport ist 3389:
   
    ```PowerShell
   Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -Name "PortNumber" -Value 3389 -Type DWord -Force
    ```
    Wenn Sie eine VM bereitstellen, werden die Standardregeln für Port 3389 erstellt. Wenn Sie die Portnummer ändern möchten, können Sie dies nach der Bereitstellung der VM in Azure vornehmen.

3. Der Listener lauscht auf allen Netzwerkschnittstellen:
   
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -Name "LanAdapter" -Value 0 -Type DWord -Force
   ```
4. Konfigurieren Sie den Modus zur Authentifizierung auf Netzwerkebene (Network Level Authentication, NLA) für die RDP-Verbindungen:
   
    ```PowerShell
   Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -Name "UserAuthentication" -Value 1 -Type DWord -Force

    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -Name "SecurityLayer" -Value 1 -Type DWord -Force

    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -Name "fAllowSecProtocolNegotiation" -Value 1 -Type DWord -Force
     ```

5. Legen Sie den Keep-Alive-Wert fest:
    
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services' -Name "KeepAliveEnable" -Value 1  -Type DWord -Force
    Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services' -Name "KeepAliveInterval" -Value 1  -Type DWord -Force
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -Name "KeepAliveTimeout" -Value 1 -Type DWord -Force
    ```
6. Erneute Verbindungsherstellung:
    
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services' -Name "fDisableAutoReconnect" -Value 0 -Type DWord -Force
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -Name "fInheritReconnectSame" -Value 1 -Type DWord -Force
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -Name "fReconnectSame" -Value 0 -Type DWord -Force
    ```
7. Beschränken Sie die Anzahl gleichzeitiger Verbindungen:
    
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -Name "MaxInstanceCount" -Value 4294967295 -Type DWord -Force
    ```
8. Entfernen Sie alle selbstsignierten Zertifikate, die an den RDP-Listener gebunden sind:
    
    ```PowerShell
    if ((Get-Item -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp').Property -contains "SSLCertificateSHA1Hash")
    {
        Remove-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -Name "SSLCertificateSHA1Hash" -Force
    }
    ```
    Durch diesen Code wird sichergestellt, dass Sie zu Beginn der Bereitstellung der VM eine Verbindung herstellen können. Wenn Sie dies später prüfen möchten, können Sie dies nach der Bereitstellung des virtuellen Computers in Azure durchführen.

9. Wenn die VM Teil einer Domäne sein wird, überprüfen Sie die folgenden Richtlinien, um sicherzustellen, dass die vorherigen Einstellungen nicht zurückgesetzt werden. 
    
    | Zielsetzung                                     | Richtlinie                                                                                                                                                       | Wert                                                                                    |
    |------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------|
    | RDP ist aktiviert                           | Computerkonfiguration\Richtlinien\Windows-Einstellungen\Administrative Vorlagen\Komponenten\Remotedesktopdienste\Remotedesktop-Sitzungshost\Verbindungen         | Remoteverbindungen für Benutzer mithilfe der Remotedesktopdienste zulassen                                  |
    | NLA-Gruppenrichtlinie                         | Einstellungen\Administrative Vorlagen\Komponenten\Remotedesktopdienste\Remotedesktop-Sitzungshost\Sicherheit                                                    | Erfordern der Benutzerauthentifizierung für den Remotezugriff mithilfe der NLA |
    | Keep-Alive-Einstellungen                      | Computerkonfiguration\Richtlinien\Windows-Einstellungen\Administrative Vorlagen\Windows-Komponenten\Remotedesktopdienste\Remotedesktop-Sitzungshost\Verbindungen | Keep-Alive-Verbindungsintervall konfigurieren                                                 |
    | Einstellungen zur erneuten Verbindungsherstellung                       | Computerkonfiguration\Richtlinien\Windows-Einstellungen\Administrative Vorlagen\Windows-Komponenten\Remotedesktopdienste\Remotedesktop-Sitzungshost\Verbindungen | Verbindung automatisch erneut herstellen                                                                   |
    | Begrenzte Anzahl von Verbindungseinstellungen | Computerkonfiguration\Richtlinien\Windows-Einstellungen\Administrative Vorlagen\Windows-Komponenten\Remotedesktopdienste\Remotedesktop-Sitzungshost\Verbindungen | Anzahl der Verbindungen einschränken                                                              |

## <a name="configure-windows-firewall-rules"></a>Konfigurieren von Windows-Firewallregeln
1. Aktivieren Sie die Windows-Firewall für die drei Profile („Domäne“, „Standard“ und „Öffentlich“):

   ```PowerShell
    Set-NetFirewallProfile -Profile Domain,Public,Private -Enabled True
   ```

2. Führen Sie den folgenden Befehl in PowerShell aus, um WinRM in den drei Firewallprofilen („Domäne“, „Privat“ und „Öffentlich“) zuzulassen, und aktivieren Sie den PowerShell-Remotedienst:
   
   ```PowerShell
    Enable-PSRemoting -Force

    Set-NetFirewallRule -DisplayName "Windows Remote Management (HTTP-In)" -Enabled True
   ```
3. Aktivieren Sie die folgenden Firewallregeln, um RDP-Datenverkehr zuzulassen:

   ```PowerShell
    Set-NetFirewallRule -DisplayGroup "Remote Desktop" -Enabled True
   ```   
4. Aktivieren Sie die Regel für die Datei- und Druckerfreigabe, damit die VM auf einen Ping-Befehl innerhalb des virtuellen Netzwerks antworten kann:

   ```PowerShell
   Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -Enabled True
   ``` 
5. Wenn die VM Teil einer Domäne sein wird, überprüfen Sie die folgenden Azure AD-Richtlinien, um sicherzustellen, dass die vorherigen Einstellungen nicht zurückgesetzt werden. 

    | Zielsetzung                                 | Richtlinie                                                                                                                                                  | Wert                                   |
    |--------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------|
    | Aktivieren der Windows-Firewallprofile | Computerkonfiguration\Richtlinien\Windows-Einstellungen\Administrative Vorlagen\Netzwerk\Netzwerkverbindung\Windows-Firewall\Domänenprofil\Windows-Firewall   | Schützen aller Netzwerkverbindungen         |
    | Aktivieren von RDP                           | Computerkonfiguration\Richtlinien\Windows-Einstellungen\Administrative Vorlagen\Netzwerk\Netzwerkverbindung\Windows-Firewall\Domänenprofil\Windows-Firewall   | Zulassen eingehender Remotedesktopausnahmen |
    |                                      | Computerkonfiguration\Richtlinien\Windows-Einstellungen\Administrative Vorlagen\Netzwerk\Netzwerkverbindung\Windows-Firewall\Standardprofil\Windows-Firewall | Zulassen eingehender Remotedesktopausnahmen |
    | Aktivieren von ICMP-V4                       | Computerkonfiguration\Richtlinien\Windows-Einstellungen\Administrative Vorlagen\Netzwerk\Netzwerkverbindung\Windows-Firewall\Domänenprofil\Windows-Firewall   | Zulassen von ICMP-Ausnahmen                   |
    |                                      | Computerkonfiguration\Richtlinien\Windows-Einstellungen\Administrative Vorlagen\Netzwerk\Netzwerkverbindung\Windows-Firewall\Standardprofil\Windows-Firewall | Zulassen von ICMP-Ausnahmen                   |

## <a name="verify-the-vm"></a>Überprüfen des virtuellen Computers 

Stellen Sie sicher, dass die VM fehlerfrei und sicher ist und dass per RDP darauf zugegriffen werden kann: 

1. Um sicherzustellen, dass der Datenträger fehlerfrei und konsistent ist, führen Sie beim nächsten VM-Neustart eine Datenträgerüberprüfung durch:

    ```PowerShell
    Chkdsk /f
    ```
    Stellen Sie sicher, dass der Bericht einen sauberen und fehlerfreien Datenträger anzeigt.

2. Legen Sie die Einstellungen für die Startkonfigurationsdaten (Boot Configuration Data, BCD) fest. 

    > [!NOTE]
    > Verwenden Sie für die Ausführung dieser Befehle ein PowerShell-Fenster mit erhöhten Rechten.
   
   ```powershell
    bcdedit /set "{bootmgr}" integrityservices enable
    bcdedit /set "{default}" device partition=C:
    bcdedit /set "{default}" integrityservices enable
    bcdedit /set "{default}" recoveryenabled Off
    bcdedit /set "{default}" osdevice partition=C:
    bcdedit /set "{default}" bootstatuspolicy IgnoreAllFailures

    #Enable Serial Console Feature
    bcdedit /set "{bootmgr}" displaybootmenu yes
    bcdedit /set "{bootmgr}" timeout 5
    bcdedit /set "{bootmgr}" bootems yes
    bcdedit /ems "{current}" ON
    bcdedit /emssettings EMSPORT:1 EMSBAUDRATE:115200
   ```
3. Das Abbildprotokoll kann bei der Problembehandlung nach Windows-Abstürzen hilfreich sein. Aktivieren Sie die Sammlung von Abbildprotokollen:

    ```powershell
    # Set up the guest OS to collect a kernel dump on an OS crash event
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\CrashControl' -Name CrashDumpEnabled -Type DWord -Force -Value 2
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\CrashControl' -Name DumpFile -Type ExpandString -Force -Value "%SystemRoot%\MEMORY.DMP"
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\CrashControl' -Name NMICrashDump -Type DWord -Force -Value 1

    # Set up the guest OS to collect user mode dumps on a service crash event
    $key = 'HKLM:\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps'
    if ((Test-Path -Path $key) -eq $false) {(New-Item -Path 'HKLM:\SOFTWARE\Microsoft\Windows\Windows Error Reporting' -Name LocalDumps)}
    New-ItemProperty -Path $key -Name DumpFolder -Type ExpandString -Force -Value "c:\CrashDumps"
    New-ItemProperty -Path $key -Name CrashCount -Type DWord -Force -Value 10
    New-ItemProperty -Path $key -Name DumpType -Type DWord -Force -Value 2
    Set-Service -Name WerSvc -StartupType Manual
    ```
4. Überprüfen Sie, ob das Repository für die Windows-Verwaltungsinstrumentation (Windows Management Instrumentation, WMI) konsistent ist:

    ```PowerShell
    winmgmt /verifyrepository
    ```
    Wenn das Repository beschädigt ist, informieren Sie sich in [WMI: Repository beschädigt oder nicht](https://blogs.technet.microsoft.com/askperf/2014/08/08/wmi-repository-corruption-or-not).

5. Stellen Sie sicher, dass keine andere Anwendung Port 3389 verwendet. Dieser Port wird für den RDP-Dienst in Azure verwendet. Um festzustellen, welche Ports auf dem virtuellen Computer verwendet werden, führen Sie `netstat -anob` aus:

    ```PowerShell
    netstat -anob
    ```

6. So laden Sie eine Windows-VHD hoch, die ein Domänencontroller ist

   * Führen Sie [diese zusätzlichen Schritte aus](https://support.microsoft.com/kb/2904015), um den Datenträger vorzubereiten.

   * Stellen Sie sicher, dass Sie das DSRM-Kennwort (Directory Services Restore Mode) kennen, falls Sie zu einem bestimmten Zeitpunkt die VM in DSRM starten müssen. Weitere Informationen finden Sie unter [set DSRM password](https://technet.microsoft.com/library/cc754363(v=ws.11).aspx) (Festlegen eines DSRM-Kennworts).

7. Stellen Sie sicher, dass Sie die Anmeldeinformationen für das integrierte Administratorkonto und das Kennwort kennen. Setzen Sie ggf. das aktuelle Kennwort des lokalen Administrators zurück, und stellen Sie sicher, dass Sie sich mit diesem Konto über die RDP-Verbindung bei Windows anmelden können. Diese Zugriffsberechtigung wird über das Gruppenrichtlinienobjekt „Anmelden über Remotedesktopdienste zulassen“ gesteuert. Sie können dieses Objekt im folgenden Pfad im Editor für lokale Gruppenrichtlinien anzeigen:

    Computerkonfiguration\Windows-Einstellungen\Sicherheitseinstellungen\Lokale Richtlinien\Zuweisen von Benutzerrechten

8. Überprüfen Sie die folgenden Azure AD-Richtlinien, um sicherzustellen, dass Sie den RDP-Zugriff weder über RDP noch aus dem Netzwerk blockieren:

    - Computerkonfiguration\Windows-Einstellungen\Sicherheitseinstellungen\Lokale Richtlinien\Zuweisen von Benutzerrechten\Zugriff vom Netzwerk auf diesen Computer verweigern

    - Computerkonfiguration\Windows-Einstellungen\Sicherheitseinstellungen\Lokale Richtlinien\Zuweisen von Benutzerrechten\Anmelden über Remotedesktopdienste verweigern


9. Überprüfen Sie die folgende Azure AD-Richtlinie, um sicherzustellen, dass Sie keines der erforderlichen Zugriffskonten entfernen:

   - Computerkonfiguration\Windows-Einstellungen\Sicherheitseinstellungen\Lokale Richtlinien\Zuweisen von Benutzerrechten\Zugriff vom Netzwerk auf diesen Computer

   Die Richtlinie sollten die folgenden Gruppen enthalten:

   - Administratoren

   - Sicherungsoperatoren

   - Jeder

   - Benutzer

10. Starten Sie den virtuellen Computer neu, um sicherzustellen, dass Windows weiterhin fehlerfrei ausgeführt wird und über die RDP-Verbindung erreichbar ist. An diesem Punkt empfiehlt es sich, einen virtuellen Computer in der lokalen Hyper-V-Instanz zu erstellen, um zu überprüfen, ob der virtuelle Computer vollständig gestartet wird. Testen Sie danach, ob Sie den virtuellen Computer über RDP erreichen können.

11. Entfernen Sie alle zusätzlichen TDI-Filter (Transport Driver Interface). Entfernen Sie beispielsweise Software, die TCP-Pakete analysiert, oder zusätzliche Firewalls. Wenn Sie dies später prüfen möchten, können Sie dies nach der Bereitstellung des virtuellen Computers in Azure durchführen.

12. Deinstallieren Sie jegliche Drittanbietersoftware und -treiber im Zusammenhang mit physischen Komponenten oder einer anderen Virtualisierungstechnologie.

### <a name="install-windows-updates"></a>Installieren von Windows-Updates
Im Idealfall sollten Sie den Computer auf dem Stand der aktuellen *Patchebene* halten. Wenn dies nicht möglich ist, stellen Sie sicher, dass die folgenden Updates installiert sind:

| Komponente               | Binary         | Windows 7 SP1, Windows Server 2008 R2 SP1 | Windows 8, Windows Server 2012               | Windows 8.1, Windows Server 2012 R2 | Windows 10 v1607, Windows Server 2016 v1607 | Windows 10 v1703    | Windows 10 v1709, Windows Server 2016 v1709 | Windows 10 v1803, Windows Server 2016 v1803 |
|-------------------------|----------------|-------------------------------------------|---------------------------------------------|------------------------------------|---------------------------------------------------------|----------------------------|-------------------------------------------------|-------------------------------------------------|
| Storage                 | disk.sys       | 6.1.7601.23403 – KB3125574                | 6.2.9200.17638 / 6.2.9200.21757 – KB3137061 | 6.3.9600.18203 – KB3137061         | -                                                       | -                          | -                                               | -                                               |
|                         | storport.sys   | 6.1.7601.23403 – KB3125574                | 6.2.9200.17188 / 6.2.9200.21306 – KB3018489 | 6.3.9600.18573 – KB4022726         | 10.0.14393.1358 – KB4022715                             | 10.0.15063.332             | -                                               | -                                               |
|                         | ntfs.sys       | 6.1.7601.23403 – KB3125574                | 6.2.9200.17623 / 6.2.9200.21743 – KB3121255 | 6.3.9600.18654 – KB4022726         | 10.0.14393.1198 – KB4022715                             | 10.0.15063.447             | -                                               | -                                               |
|                         | Iologmsg.dll   | 6.1.7601.23403 – KB3125574                | 6.2.9200.16384 – KB2995387                  | -                                  | -                                                       | -                          | -                                               | -                                               |
|                         | Classpnp.sys   | 6.1.7601.23403 – KB3125574                | 6.2.9200.17061 / 6.2.9200.21180 – KB2995387 | 6.3.9600.18334 – KB3172614         | 10.0.14393.953 – KB4022715                              | -                          | -                                               | -                                               |
|                         | Volsnap.sys    | 6.1.7601.23403 – KB3125574                | 6.2.9200.17047 / 6.2.9200.21165 – KB2975331 | 6.3.9600.18265 – KB3145384         | -                                                       | 10.0.15063.0               | -                                               | -                                               |
|                         | partmgr.sys    | 6.1.7601.23403 – KB3125574                | 6.2.9200.16681 – KB2877114                  | 6.3.9600.17401 – KB3000850         | 10.0.14393.953 – KB4022715                              | 10.0.15063.0               | -                                               | -                                               |
|                         | volmgr.sys     |                                           |                                             |                                    |                                                         | 10.0.15063.0               | -                                               | -                                               |
|                         | Volmgrx.sys    | 6.1.7601.23403 – KB3125574                | -                                           | -                                  | -                                                       | 10.0.15063.0               | -                                               | -                                               |
|                         | Msiscsi.sys    | 6.1.7601.23403 – KB3125574                | 6.2.9200.21006 – KB2955163                  | 6.3.9600.18624 – KB4022726         | 10.0.14393.1066 – KB4022715                             | 10.0.15063.447             | -                                               | -                                               |
|                         | Msdsm.sys      | 6.1.7601.23403 – KB3125574                | 6.2.9200.21474 – KB3046101                  | 6.3.9600.18592 – KB4022726         | -                                                       | -                          | -                                               | -                                               |
|                         | Mpio.sys       | 6.1.7601.23403 – KB3125574                | 6.2.9200.21190 – KB3046101                  | 6.3.9600.18616 – KB4022726         | 10.0.14393.1198 – KB4022715                             | -                          | -                                               | -                                               |
|                         | vmstorfl.sys   | 6.3.9600.18907 – KB4072650                | 6.3.9600.18080 – KB3063109                  | 6.3.9600.18907 – KB4072650         | 10.0.14393.2007 – KB4345418                             | 10.0.15063.850 – KB4345419 | 10.0.16299.371 – KB4345420                      | -                                               |
|                         | Fveapi.dll     | 6.1.7601.23311 – KB3125574                | 6.2.9200.20930 – KB2930244                  | 6.3.9600.18294 – KB3172614         | 10.0.14393.576 – KB4022715                              | -                          | -                                               | -                                               |
|                         | Fveapibase.dll | 6.1.7601.23403 – KB3125574                | 6.2.9200.20930 – KB2930244                  | 6.3.9600.17415 – KB3172614         | 10.0.14393.206 – KB4022715                              | -                          | -                                               | -                                               |
| Netzwerk                 | netvsc.sys     | -                                         | -                                           | -                                  | 10.0.14393.1198 – KB4022715                             | 10.0.15063.250 – KB4020001 | -                                               | -                                               |
|                         | mrxsmb10.sys   | 6.1.7601.23816 – KB4022722                | 6.2.9200.22108 – KB4022724                  | 6.3.9600.18603 – KB4022726         | 10.0.14393.479 – KB4022715                              | 10.0.15063.483             | -                                               | -                                               |
|                         | mrxsmb20.sys   | 6.1.7601.23816 – KB4022722                | 6.2.9200.21548 – KB4022724                  | 6.3.9600.18586 – KB4022726         | 10.0.14393.953 – KB4022715                              | 10.0.15063.483             | -                                               | -                                               |
|                         | mrxsmb.sys     | 6.1.7601.23816 – KB4022722                | 6.2.9200.22074 – KB4022724                  | 6.3.9600.18586 – KB4022726         | 10.0.14393.953 – KB4022715                              | 10.0.15063.0               | -                                               | -                                               |
|                         | tcpip.sys      | 6.1.7601.23761 – KB4022722                | 6.2.9200.22070 – KB4022724                  | 6.3.9600.18478 – KB4022726         | 10.0.14393.1358 – KB4022715                             | 10.0.15063.447             | -                                               | -                                               |
|                         | http.sys       | 6.1.7601.23403 – KB3125574                | 6.2.9200.17285 – KB3042553                  | 6.3.9600.18574 – KB4022726         | 10.0.14393.251 – KB4022715                              | 10.0.15063.483             | -                                               | -                                               |
|                         | vmswitch.sys   | 6.1.7601.23727 – KB4022719                | 6.2.9200.22117 – KB4022724                  | 6.3.9600.18654 – KB4022726         | 10.0.14393.1358 – KB4022715                             | 10.0.15063.138             | -                                               | -                                               |
| Core                    | ntoskrnl.exe   | 6.1.7601.23807 – KB4022719                | 6.2.9200.22170 – KB4022718                  | 6.3.9600.18696 – KB4022726         | 10.0.14393.1358 – KB4022715                             | 10.0.15063.483             | -                                               | -                                               |
| Remotedesktopdienste | rdpcorets.dll  | 6.2.9200.21506 – KB4022719                | 6.2.9200.22104 – KB4022724                  | 6.3.9600.18619 – KB4022726         | 10.0.14393.1198 – KB4022715                             | 10.0.15063.0               | -                                               | -                                               |
|                         | termsrv.dll    | 6.1.7601.23403 – KB3125574                | 6.2.9200.17048 – KB2973501                  | 6.3.9600.17415 – KB3000850         | 10.0.14393.0 – KB4022715                                | 10.0.15063.0               | -                                               | -                                               |
|                         | termdd.sys     | 6.1.7601.23403 – KB3125574                | -                                           | -                                  | -                                                       | -                          | -                                               | -                                               |
|                         | win32k.sys     | 6.1.7601.23807 – KB4022719                | 6.2.9200.22168 – KB4022718                  | 6.3.9600.18698 – KB4022726         | 10.0.14393.594 – KB4022715                              | -                          | -                                               | -                                               |
|                         | rdpdd.dll      | 6.1.7601.23403 – KB3125574                | -                                           | -                                  | -                                                       | -                          | -                                               | -                                               |
|                         | rdpwd.sys      | 6.1.7601.23403 – KB3125574                | -                                           | -                                  | -                                                       | -                          | -                                               | -                                               |
| Sicherheit                | MS17-010       | KB4012212                                 | KB4012213                                   | KB4012213                          | KB4012606                                               | KB4012606                  | -                                               | -                                               |
|                         |                |                                           | KB4012216                                   |                                    | KB4013198                                               | KB4013198                  | -                                               | -                                               |
|                         |                | KB4012215                                 | KB4012214                                   | KB4012216                          | KB4013429                                               | KB4013429                  | -                                               | -                                               |
|                         |                |                                           | KB4012217                                   |                                    | KB4013429                                               | KB4013429                  | -                                               | -                                               |
|                         | CVE-2018-0886  | KB4103718               | KB4103730                | KB4103725       | KB4103723                                               | KB4103731                  | KB4103727                                       | KB4103721                                       |
|                         |                | KB4103712          | KB4103726          | KB4103715|                                                         |                            |                                                 |                                                 |
       
> [!NOTE]
> Um einen versehentlichen Neustart bei der VM-Bereitstellung zu vermeiden, empfiehlt es sich, darauf zu achten, dass alle Windows Update-Installationen abgeschlossen sind und keine Updates ausstehen. Dies kann beispielsweise so erfolgen, dass alle möglichen Windows-Updates installiert werden und einmalig ein Neustart durchgeführt wird, bevor Sie den Sysprep-Befehl ausführen.

### Ermitteln von Einsatzszenarien für Sysprep <a id="step23"></a>    

Das Tool für die Systemvorbereitung (System Preparation Tool, Sysprep) ist ein Prozess, den Sie ausführen können, um eine Windows-Installation zurückzusetzen. Sysprep ermöglicht praktisch das Zurücksetzen auf Werkseinstellungen, bei dem alle personenbezogenen Daten entfernt und verschiedene Komponenten zurückgesetzt werden. 

Sie verwenden Sysprep üblicherweise, wenn Sie eine Vorlage erstellen, aus der Sie verschiedene weitere VMs mit einer spezifischen Konfiguration bereitstellen können. Diese Vorlage wird als *generalisiertes Image* bezeichnet.

Wenn Sie nur eine VM aus einem Datenträger erstellen möchten, ist die Verwendung von Sysprep nicht erforderlich. Sie können den virtuellen Computer stattdessen aus einem *spezialisierten Image* erstellen. Weitere Informationen zum Erstellen einer VM aus einem spezialisierten Datenträger finden Sie hier:

- [Erstellen eines virtuellen Computers von einem speziellen Datenträger](create-vm-specialized.md)
- [Create a VM from a specialized VHD disk](https://docs.microsoft.com/azure/virtual-machines/windows/create-vm-specialized-portal?branch=master) (Erstellen einer VM aus einem spezialisierten VHD-Datenträger)

Wenn Sie ein generalisiertes Image erstellen möchten, müssen Sie Sysprep ausführen. Weitere Informationen finden Sie unter [How to Use Sysprep: An introduction.](https://technet.microsoft.com/library/bb457073.aspx) (Verwenden von Sysprep: Einführung.) 

Nicht jede Rolle oder Anwendung, die auf einem Windows-basierten Computer installiert ist, unterstützt generalisierte Images. Stellen Sie daher vor dem Durchführen dieses Verfahrens sicher, dass Sysprep die Rolle des Computers unterstützt. Weitere Informationen finden Sie unter [Sysprep Support for Server Roles](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles) (Sysprep-Unterstützung für Serverrollen).

### <a name="generalize-a-vhd"></a>Generalisieren einer VHD

>[!NOTE]
> Schalten Sie die VM nach der Ausführung von `sysprep.exe` in den folgenden Schritten aus. Schalten Sie sie erst wieder ein, nachdem Sie ein Image davon in Azure erstellt haben.

1. Melden Sie sich bei der Windows-VM an.
1. Öffnen Sie als Administrator eine **Eingabeaufforderung**. 
1. Wechseln Sie zum Verzeichnis `%windir%\system32\sysprep`. Führen Sie dann `sysprep.exe` aus.
1. Wählen Sie unter **Systemvorbereitungsprogramm** die Option **Out-of-Box-Experience (OOBE) für System aktivieren**, und vergewissern Sie sich, dass das Kontrollkästchen **Verallgemeinern** aktiviert ist.

    ![Tool für die Systemvorbereitung](media/prepare-for-upload-vhd-image/syspre.png)
1. Wählen Sie unter **Optionen für Herunterfahren** die Option **Herunterfahren** aus.
1. Klicken Sie auf **OK**.
1. Fahren Sie die VM herunter, nachdem Sysprep abgeschlossen wurde. Verwenden Sie nicht **Neu starten**, um die VM herunterzufahren.

Jetzt kann die VHD hochgeladen werden. Weitere Informationen zum Erstellen einer VM aus einem generalisierten Datenträger finden Sie unter [Hochladen einer generalisierten VHD in Azure und Erstellen einer neuen VM](sa-upload-generalized.md).


>[!NOTE]
> Eine benutzerdefinierte Datei *unattend.xml* wird nicht unterstützt. Allerdings wird die `additionalUnattendContent`-Eigenschaft unterstützt, die nur eingeschränkte Unterstützung für das Hinzufügen von [microsoft-windows-shell-setup](https://docs.microsoft.com/windows-hardware/customize/desktop/unattend/microsoft-windows-shell-setup)-Optionen in der Datei *unattend.xml* bietet, die der Azure-Bereitstellungs-Agent verwendet. Sie können z. B. [additionalUnattendContent](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.compute.models.additionalunattendcontent?view=azure-dotnet) verwenden, um FirstLogonCommands und LogonCommands hinzuzufügen. Weitere Informationen finden Sie unter [Beispiel für additionalUnattendContent FirstLogonCommands](https://github.com/Azure/azure-quickstart-templates/issues/1407).


## <a name="complete-the-recommended-configurations"></a>Abschließen empfohlener Konfigurationen
Die folgenden Einstellungen wirken sich nicht auf das Hochladen von VHDs aus. Es wird jedoch dringend empfohlen, diese Einstellungen zu konfigurieren.

* Installieren Sie den [Azure-VM-Agent](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Anschließend können Sie VM-Erweiterungen aktivieren. Die VM-Erweiterungen implementieren die meisten der wichtigen Funktionen, die Sie für Ihre virtuellen Computer möglicherweise verwenden möchten. Sie benötigten die Erweiterungen z. B. zum Zurücksetzen von Kennwörtern und zum Konfigurieren von RDP. Weitere Informationen finden Sie unter [Übersicht über den Agent für virtuelle Azure-Computer](../extensions/agent-windows.md).
* Nachdem die VM in Azure erstellt wurde, sollten Sie die Auslagerungsdatei zur Verbesserung der Leistung auf dem *Volume für das temporäre Laufwerk* platzieren. Sie können die Ablage von Dateien wie folgt einrichten:

   ```PowerShell
   Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\Memory Management' -Name "PagingFiles" -Value "D:\pagefile.sys" -Type MultiString -Force
   ```
  Wenn an die VM ein Datenträger angefügt ist, lautet der Laufwerkbuchstabe des Volumes für das temporäre Laufwerk typischerweise *D*. Die Bezeichnung kann abhängig von Ihren Einstellungen und der Anzahl verfügbarer Laufwerke jedoch abweichen.
  * Es wird empfohlen, Skriptblocker zu deaktivieren, die möglicherweise von Antivirussoftware bereitgestellt werden. Diese können die beim Bereitstellen einer neuen VM aus Ihrem Image ausgeführten Skripts des Windows-Bereitstellungs-Agents beeinträchtigen oder blockieren.
  
## <a name="next-steps"></a>Nächste Schritte
* [Hochladen eines Windows-VM-Images an Azure für Resource Manager-Bereitstellungen](upload-generalized-managed.md)
* [Beheben von Problemen bei der Aktivierung virtueller Windows-Computer](troubleshoot-activation-problems.md)

