---
title: Endpoint Protection-Empfehlungen in Azure Security Center
description: Erläutert, wie die Endpoint Protection-Lösungen ermittelt und als fehlerfrei identifiziert werden.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 2730a2f5-20bc-4027-a1c2-db9ed0539532
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2019
ms.author: memildin
ms.openlocfilehash: 140361b7ba3a6a618d4c416447525f8a73690b81
ms.sourcegitcommit: 827248fa609243839aac3ff01ff40200c8c46966
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73748431"
---
# <a name="endpoint-protection-assessment-and-recommendations-in-azure-security-center"></a>Endpoint Protection: Bewertung und Empfehlungen in Azure Security Center

Azure Security Center bietet Integritätsbewertungen von [unterstützten](https://docs.microsoft.com/azure/security-center/security-center-os-coverage) Versionen von Endpoint Protection-Lösungen. In diesem Artikel werden die Szenarien erläutert, die dazu führen, dass Security Center die folgenden beiden Empfehlungen generiert:

* **Endpoint Protection-Lösungen auf Ihrem virtuellen Computer installieren**
* **Endpoint Protection-Integritätsprobleme auf Ihren Computern beheben**

## <a name="windows-defender"></a>Windows Defender

* Security Center empfiehlt Ihnen **„Endpoint Protection-Lösungen auf Ihrem virtuellen Computer zu installieren“** , wenn [Get-MpComputerStatus](https://docs.microsoft.com/powershell/module/defender/get-mpcomputerstatus?view=win10-ps) ausgeführt wird und das Ergebnis **AMServiceEnabled: False** zurückgibt.

* Security Center empfiehlt Ihnen, **„Endpoint Protection-Integritätsprobleme auf Ihren Computern zu beheben“** , wenn [Get-MpComputerStatus](https://docs.microsoft.com/powershell/module/defender/get-mpcomputerstatus?view=win10-ps) ausgeführt wird und eine der folgenden Situationen eintritt:

  * Eine der folgenden Eigenschaften ist FALSE:

    **AMServiceEnabled**

    **AntispywareEnabled**

    **RealTimeProtectionEnabled**

    **BehaviorMonitorEnabled**

    **IoavProtectionEnabled**

    **OnAccessProtectionEnabled**

  * Mindestens eine der folgenden Eigenschaften ist größer oder gleich 7.

    **AntispywareSignatureAge**

    **AntivirusSignatureAge**

## <a name="microsoft-system-center-endpoint-protection"></a>Microsoft System Center Endpoint Protection

* Security Center empfiehlt, **„Endpoint Protection-Lösungen auf Ihrem virtuellen Computer zu installieren“** , wenn das Importieren von **SCEPMpModule ("$env:ProgramFiles\Microsoft Security Client\MpProvider\MpProvider.psd1")** und Ausführen von **Get-MProtComputerStatus** zum Ergebnis **AMServiceEnabled = false** führt.

* Security Center empfiehlt Ihnen, **„Endpoint Protection-Integritätsprobleme auf Ihren Computern zu beheben“** , wenn **Get-MprotComputerStatus** ausgeführt wird und eine der folgenden Situationen eintritt:

    * Mindestens eine der folgenden Eigenschaften ist „False“:

            **AMServiceEnabled**

            **AntispywareEnabled**
    
            **RealTimeProtectionEnabled**
    
            **BehaviorMonitorEnabled**
    
            **IoavProtectionEnabled**
    
            **OnAccessProtectionEnabled**
          
    * Mindestens eine oder beide folgenden Signaturaktualisierungen ist größer oder gleich 7. 

            **AntispywareSignatureAge**
    
            **AntivirusSignatureAge**

## <a name="trend-micro"></a>Trend Micro

* Security Center empfiehlt Ihnen, **„Endpoint Protection-Lösungen auf Ihrem virtuellen Computer zu installieren“** , wenn eine der folgenden Überprüfungen nicht bestanden wird:
    * **HKLM:\SOFTWARE\TrendMicro\Deep Security Agent** ist vorhanden
    * **HKLM:\SOFTWARE\TrendMicro\Deep Security Agent\InstallationFolder** ist vorhanden
    * Die Datei **dsq_query.cmd** befindet sich im Installationsordner.
    * Das Ausführen von **dsa_query.cmd** führt zum Ergebnis **Component.AM.mode: on – Trend Micro Deep Security-Agent erkannt**

## <a name="symantec-endpoint-protection"></a>Symantec Endpoint Protection
Security Center empfiehlt Ihnen, **„Endpoint Protection-Lösungen auf Ihrem virtuellen Computer zu installieren“** , wenn eine der folgenden Überprüfungen nicht bestanden wird:

* **HKLM:\Software\Symantec\Symantec Endpoint Protection\CurrentVersion\PRODUCTNAME = "Symantec Endpoint Protection"**

* **HKLM:\Software\Symantec\Symantec Endpoint Protection\CurrentVersion\public-opstate\ASRunningStatus = 1**

oder

* **HKLM:\Software\Wow6432Node\Symantec\Symantec Endpoint Protection\CurrentVersion\PRODUCTNAME = "Symantec Endpoint Protection"**

* **HKLM:\Software\Wow6432Node\Symantec\Symantec Endpoint Protection\CurrentVersion\public-opstate\ASRunningStatus = 1**

Security Center empfiehlt Ihnen, **„Endpoint Protection-Integritätsprobleme auf Ihren Computern zu beheben“** , wenn eine der folgenden Überprüfungen nicht bestanden wird:

* Überprüfen: Symantec-Version >= 12: Registrierungspfad: **HKLM:\Software\Symantec\Symantec Endpoint Protection\CurrentVersion" -Value "PRODUCTVERSION"**

* Echtzeitschutzstatus überprüfen: **HKLM:\Software\Wow6432Node\Symantec\Symantec Endpoint Protection\AV\Storages\Filesystem\RealTimeScan\OnOff == 1**

* Status der Signaturaktualisierung überprüfen: **HKLM\Software\Symantec\Symantec Endpoint Protection\CurrentVersion\public-opstate\LatestVirusDefsDate <= 7 Tage**

* Status der vollständigen Überprüfung überprüfen: **HKLM:\Software\Symantec\Symantec Endpoint Protection\CurrentVersion\public-opstate\LastSuccessfulScanDateTime <= 7 Tage**

* Pfad der Signaturversionsnummer zu Signaturversion für Symantec 12 suchen: **Registry Paths+ "CurrentVersion\SharedDefs" -Value "SRTSP"** 

* Pfad zur Signaturversion für Symantec-14: **Registry Paths+ "CurrentVersion\SharedDefs\SDSDefs" -Value "SRTSP"**

Registrierungspfade:

* **"HKLM:\Software\Symantec\Symantec Endpoint Protection" + $Path;**
* **"HKLM:\Software\Wow6432Node\Symantec\Symantec Endpoint Protection" + $Path**

## <a name="mcafee-endpoint-protection-for-windows"></a>McAfee Endpoint Protection für Windows

Security Center empfiehlt Ihnen, **„Endpoint Protection-Lösungen auf Ihrem virtuellen Computer zu installieren“** , wenn eine der folgenden Überprüfungen nicht bestanden wird:

* **HKLM:\SOFTWARE\McAfee\Endpoint\AV\ProductVersion** ist vorhanden

* **HKLM:\SOFTWARE\McAfee\AVSolution\MCSHIELDGLOBAL\GLOBAL\enableoas = 1**

Security Center empfiehlt Ihnen, **„Endpoint Protection-Integritätsprobleme auf Ihren Computern zu beheben“** , wenn eine der folgenden Überprüfungen nicht bestanden wird:

* McAfee-Version: **HKLM:\SOFTWARE\McAfee\Endpoint\AV\ProductVersion >= 10**

* Signaturversion suchen: **HKLM:\Software\McAfee\AVSolution\DS\DS -Value "dwContentMajorVersion"**

* Signaturdatum suchen: **HKLM:\Software\McAfee\AVSolution\DS\DS -Value "szContentCreationDate" >= 7 Tage**

* Überprüfungsdatum suchen: **HKLM:\Software\McAfee\Endpoint\AV\ODS -Value "LastFullScanOdsRunTime" >= 7 Tage**

## <a name="mcafee-endpoint-security-for-linux-threat-prevention"></a>McAfee Endpoint Security für Linux-Bedrohungsschutz 

Security Center empfiehlt Ihnen, **„Endpoint Protection-Lösungen auf Ihrem virtuellen Computer zu installieren“** , wenn eine der folgenden Überprüfungen nicht bestanden wird:

- Datei **/opt/isec/ens/threatprevention/bin/isecav** wird beendet 

- Ausgabe von **"/opt/isec/ens/threatprevention/bin/isecav --version"** lautet: **McAfee name = McAfee Endpoint Security for Linux Threat Prevention and McAfee version >= 10**

Security Center empfiehlt Ihnen, **„Endpoint Protection-Integritätsprobleme auf Ihren Computern zu beheben“** , wenn eine der folgenden Überprüfungen nicht bestanden wird:

- **"/opt/isec/ens/threatprevention/bin/isecav --listtask"** gibt **Quick scan, Full scan** zurück, und für beide Scans gilt „<= 7 Tage“

- **"/opt/isec/ens/threatprevention/bin/isecav --listtask"** gibt **DAT and engine Update time** zurück, und für beide gilt „<= 7 Tage“

- **"/opt/isec/ens/threatprevention/bin/isecav --getoasconfig --summary"** gibt den Status **On Access Scan** zurück

## <a name="sophos-antivirus-for-linux"></a>Sophos Antivirus für Linux 

Security Center empfiehlt Ihnen, **„Endpoint Protection-Lösungen auf Ihrem virtuellen Computer zu installieren“** , wenn eine der folgenden Überprüfungen nicht bestanden wird:

- Datei **/opt/sophos-av/bin/savdstatus** wird beendet, oder Suche nach benutzerdefiniertem Ort **"readlink $(which savscan)"** wird durchgeführt

- **"/opt/sophos-av/bin/savdstatus --version"** gibt **Sophos name = Sophos Anti-Virus and Sophos version >= 9** zurück

Security Center empfiehlt Ihnen, **„Endpoint Protection-Integritätsprobleme auf Ihren Computern zu beheben“** , wenn eine der folgenden Überprüfungen nicht bestanden wird:

- **"/opt/sophos-av/bin/savlog --maxage=7 | grep -i "Scheduled scan .\* completed" | tail -1"** gibt einen Wert zurück

- **"/opt/sophos-av/bin/savlog --maxage=7 | grep "scan finished"** | tail -1" gibt einen Wert zurück

- **"/opt/sophos-av/bin/savdstatus --lastupdate"** gibt „lastUpdate“ zurück. Der Wert sollte kleiner oder gleich 7 Tage sein 

- **"/opt/sophos-av/bin/savdstatus -v"** entspricht **"On-access scanning is running"** 

- **"/opt/sophos-av/bin/savconfig get LiveProtection"** gibt „enabled“ zurück

## <a name="troubleshoot-and-support"></a>Problembehandlung und Support

### <a name="troubleshoot"></a>Problembehandlung

Die Protokolle der Microsoft Antimalware-Erweiterung sind unter **%Systemdrive%\WindowsAzure\Logs\Plugins\Microsoft.Azure.Security.IaaSAntimalware(Or PaaSAntimalware)\1.5.5.x(version#)\CommandExecution.log** verfügbar.

### <a name="support"></a>Support

Wenn Sie weitere Hilfe benötigen, setzen Sie sich über das [MSDN Azure-Forum oder über das Stack Overflow-Forum](https://azure.microsoft.com/support/forums/) mit Azure-Experten in Verbindung. Sie können auch einen Azure-Supportfall erstellen. Rufen Sie die [Azure-Support-Website](https://azure.microsoft.com/support/options/) auf, und wählen Sie „Support erhalten“ aus. Informationen zur Nutzung von Azure-Support finden Sie unter [Microsoft Azure-Support-FAQ](https://azure.microsoft.com/support/faq/).