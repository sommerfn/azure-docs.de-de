---
title: Behandlung von Konnektivitätsproblemen bei der Azure-zu-Azure-Notfallwiederherstellung mit Azure Site Recovery
description: Hier finden Sie Informationen zum Beheben von Problemen und Fehlern beim Replizieren von virtuellen Azure-Computern für die Notfallwiederherstellung.
services: site-recovery
author: asgang
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 08/05/2019
ms.author: asgang
ms.openlocfilehash: 4995a4086c471a06fe859febfd2d1af7fbb22a76
ms.sourcegitcommit: 6c2c97445f5d44c5b5974a5beb51a8733b0c2be7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2019
ms.locfileid: "73622454"
---
# <a name="troubleshoot-azure-to-azure-vm-network-connectivity-issues"></a>Problembehandlung für Azure-zu-Azure-VM-Netzwerkkonnektivitätsprobleme

In diesem Artikel werden allgemeine Probleme im Zusammenhang mit der Netzwerkkonnektivität beim Replizieren und Wiederherstellen virtueller Azure-Computer aus einer Region in einer anderen Region beschrieben. Weitere Informationen zu den Netzwerkanforderungen finden Sie unter [Netzwerkkonzepte für die Replikation zwischen Azure-Standorten](azure-to-azure-about-networking.md).

Damit die Site Recovery-Replikation funktioniert, ist für die VM die ausgehende Konnektivität zu bestimmten URLs oder IP-Bereichen erforderlich. Wenn sich Ihr virtueller Computer hinter einer Firewall befindet oder Netzwerksicherheitsgruppen-Regeln (NSG-Regeln) zum Steuern der ausgehenden Konnektivität verwendet werden, wird ggf. eine dieser Fehlermeldungen angezeigt.

**URL** | **Details**  
--- | ---
*.blob.core.windows.net | Erforderlich, damit Daten in das Cachespeicherkonto in der Quellregion über die VM geschrieben werden können. Wenn Sie alle Cachespeicherkonten für Ihre virtuellen Computer kennen, können Sie die spezifischen Speicherkonten-URLs (z. B. cache1.blob.core.windows.net and cache2.blob.core.windows.net) anstelle von „*.blob.core.windows.net“ in die Zulassungsliste aufnehmen.
login.microsoftonline.com | Erforderlich für die Autorisierung und Authentifizierung bei den Site Recovery-Dienst-URLs.
*.hypervrecoverymanager.windowsazure.com | Erforderlich, um die Kommunikation mit dem Site Recovery-Dienst über die VM zu ermöglichen. Sie können die entsprechende Site Recovery-IP verwenden, wenn Ihr Firewallproxy IPs unterstützt.
*.servicebus.windows.net | Erforderlich, damit die Site Recovery-Überwachungs- und -Diagnosedaten über die VM geschrieben werden können. Sie können die entsprechende Site Recovery-Überwachungs-IP verwenden, wenn Ihr Firewallproxy IPs unterstützt.

## <a name="outbound-connectivity-for-site-recovery-urls-or-ip-ranges-error-code-151037-or-151072"></a>Ausgehende Konnektivität für Site Recovery-URLs oder IP-Bereiche (Fehlercode 151037 oder 151072)

## <a name="issue-1-failed-to-register-azure-virtual-machine-with-site-recovery-151195-br"></a>Problem 1: Fehler beim Registrieren des virtuellen Azure-Computers bei Site Recovery (151195) </br>
- **Mögliche Ursache** </br>
  - Die Verbindung mit Site Recovery-Endpunkten kann aufgrund eines Fehlers bei der DNS-Auflösung nicht hergestellt werden.
  - Dies ist häufiger der Fall beim erneuten Schutz, wenn Sie für den virtuellen Computer ein Failover ausgeführt haben, der DNS-Server in der Notfallwiederherstellungsregion jedoch nicht erreichbar ist.

- **Lösung**
   - Stellen Sie bei Verwendung von benutzerdefiniertem DNS sicher, dass der DNS-Server in der Notfallwiederherstellungsregion erreichbar ist. Um zu überprüfen, ob Sie benutzerdefiniertes DNS verwenden, navigieren Sie zum virtuellen Computer > Notfallwiederherstellungsnetzwerk > DNS-Server. Versuchen Sie, über den virtuellen Computer auf den DNS-Server zuzugreifen. Wenn dies nicht möglich ist, können Sie ein Failover für den DNS-Server ausführen oder eine Verbindung zwischen dem Notfallwiederherstellungsnetzwerk und dem DNS herstellen.

    ![com-error](./media/azure-to-azure-troubleshoot-errors/custom_dns.png)


## <a name="issue-2-site-recovery-configuration-failed-151196"></a>Problem 2: Fehler bei der Site Recovery-Konfiguration (151196)

> [!NOTE]
> Wenn sich die virtuellen Computer hinter dem internen **Standard**-Lastenausgleich befinden, hat dieser standardmäßig keinen Zugriff auf O365-IP-Adressen (d. h. login.microsoftonline.com). Ändern Sie ihn entweder in einen internen Lastenausgleich des Typs **Basic**, oder erstellen Sie ausgehenden Zugriff wie in [diesem Artikel](https://aka.ms/lboutboundrulescli) erwähnt.

- **Mögliche Ursache** </br>
  - Die Verbindung mit IP4-Endpunkten für die Authentifizierung und Identität von Office 365 kann nicht hergestellt werden.

- **Lösung**
  - Azure Site Recovery benötigt zur Authentifizierung Zugriff auf Office 365-IP-Adressbereiche.
    Wenn Sie zum Steuern der ausgehenden Netzwerkkonnektivität auf dem virtuellen Computer Azure-Netzwerksicherheitsgruppen-Regeln (NSG-Regeln) oder einen Firewallproxy verwenden, müssen Sie die Kommunikation mit Office 365-IP-Adressbereichen zulassen. Erstellen Sie basierend auf der NSG-Regel ein [Azure Active Directory-Diensttag (Azure AD)](../virtual-network/security-overview.md#service-tags) für den Zugriff auf alle IP-Adressen für Azure AD.
      - Wenn Azure AD später neue Adressen hinzugefügt werden, müssen Sie neue NSG-Regeln erstellen.

### <a name="example-nsg-configuration"></a>Beispielkonfiguration für eine Netzwerksicherheitsgruppe

Dieses Beispiel zeigt, wie NSG-Regeln für eine zu replizierende VM konfiguriert werden.

- Wenn Sie die ausgehende Konnektivität mit NSG-Regeln steuern, verwenden Sie für alle erforderlichen IP-Adressbereiche Regeln zum Zulassen ausgehender HTTPS-Verbindungen für „port:443“.
- Im Beispiel wird davon ausgegangen, dass der VM-Quellstandort „USA, Osten“ ist und der Zielstandort „USA, Mitte“.

### <a name="nsg-rules---east-us"></a>NSG-Regeln – USA, Osten

1. Erstellen Sie eine NSG-Sicherheitsregel für ausgehende HTTPS-Verbindungen (443) für „Storage.EastUS“, wie im Screenshot unten gezeigt.

      ![storage-tag](./media/azure-to-azure-about-networking/storage-tag.png)

2. Erstellen Sie eine NSG-Sicherheitsregel für ausgehende HTTPS-Verbindungen (443) für „AzureActiveDirectory“, wie im Screenshot unten gezeigt.

      ![aad-tag](./media/azure-to-azure-about-networking/aad-tag.png)

3. Erstellen Sie Regeln für ausgehende HTTPS-Verbindungen (443) für die Site Recovery-IP-Adressen, die dem Zielstandort entsprechen:

   **Location** | **IP-Adressen für Site Recovery** |  **IP-Adressen zur Site Recovery-Überwachung**
    --- | --- | ---
   USA (Mitte) | 40.69.144.231 | 52.165.34.144

### <a name="nsg-rules---central-us"></a>NSG-Regeln – USA, Mitte

Diese Regeln sind erforderlich, damit nach dem Failover die Replikation von der Zielregion zur Quellregion aktiviert werden kann:

1. Erstellen Sie eine NSG-Sicherheitsregel für ausgehende HTTPS-Verbindungen (443) für „Storage.CentralUS“.

2. Erstellen Sie eine NSG-Sicherheitsregel für ausgehende HTTPS-Verbindungen (443) für „AzureActiveDirectory“.

3. Erstellen Sie Regeln für ausgehende HTTPS-Verbindungen (443) für die Site Recovery-IP-Adressen, die dem Quellstandort entsprechen:

   **Location** | **IP-Adressen für Site Recovery** |  **IP-Adressen zur Site Recovery-Überwachung**
    --- | --- | ---
   USA (Mitte) | 13.82.88.226 | 104.45.147.24
## <a name="issue-3-site-recovery-configuration-failed-151197"></a>Problem 3: Fehler bei der Site Recovery-Konfiguration (151197)
- **Mögliche Ursache** </br>
  - Die Verbindung mit Azure Site Recovery-Dienstendpunkten kann nicht hergestellt werden.

- **Lösung**
  - Azure Site Recovery benötigt abhängig von der Region Zugriff auf [Site Recovery-IP-Adressbereiche](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-about-networking#outbound-connectivity-for-ip-address-ranges). Stellen Sie sicher, dass über den virtuellen Computer auf die erforderlichen IP-Bereiche zugegriffen werden kann.


## <a name="issue-4-a2a-replication-failed-when-the-network-traffic-goes-through-on-premises-proxy-server-151072"></a>Problem 4: Fehler bei der A2A-Replikation, wenn der Netzwerkdatenverkehr lokale Proxyserver durchläuft (151072)
- **Mögliche Ursache** </br>
  - Die benutzerdefinierten Proxyeinstellungen sind ungültig, und der Mobility Service-Agent von Azure Site Recovery hat die Proxyeinstellungen von Internet Explorer nicht automatisch erkannt.


- **Lösung**
  1. Der Mobilitätsdienst-Agent erkennt die Proxyeinstellungen von Internet Explorer unter Windows und /etc/environment unter Linux.
  2. Wenn nur für Mobility Service von Azure Site Recovery-Mobilitätsdienst ein Proxy festgelegt werden soll, können Sie die Proxydetails in „ProxyInfo.conf“ angeben, die sich hier befinden:</br>
     - ``/usr/local/InMage/config/`` unter ***Linux***
     - ``C:\ProgramData\Microsoft Azure Site Recovery\Config`` unter ***Windows***
  3. ProxyInfo.conf sollte die Proxyeinstellungen im folgenden INI-Format aufweisen.</br>
                *[proxy]*</br>
                *Address=http://1.2.3.4*</br>
                *Port=567*</br>
  4. Der Agent des Azure Site Recovery-Mobilitätsdiensts unterstützt nur ***nicht authentifizierte Proxys***.

### <a name="fix-the-problem"></a>Beheben des Problems
Führen Sie die Schritte im [Dokument mit der Netzwerkanleitung](site-recovery-azure-to-azure-networking-guidance.md) aus, um die [erforderlichen URLs](azure-to-azure-about-networking.md#outbound-connectivity-for-urls) bzw. die [erforderlichen IP-Bereiche](azure-to-azure-about-networking.md#outbound-connectivity-for-ip-address-ranges) zuzulassen.


## <a name="next-steps"></a>Nächste Schritte
[Replizieren virtueller Azure-Computer](site-recovery-replicate-azure-to-azure.md)
