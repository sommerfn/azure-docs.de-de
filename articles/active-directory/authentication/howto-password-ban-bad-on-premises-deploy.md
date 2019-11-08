---
title: Bereitstellen des Azure AD-Kennwortschutzes – Azure Active Directory
description: Bereitstellen des Kennwortschutzes für Azure AD zum lokalen Sperren ungültiger Kennwörter
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: article
ms.date: 02/01/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jsimmons
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5e2328bcd2b2d9fe957df82c46730091ffdf9366
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73474285"
---
# <a name="deploy-azure-ad-password-protection"></a>Bereitstellen des Kennwortschutzes für Azure AD

Nachdem Sie nun erfahren haben, [wie Sie den Azure AD-Kennwortschutz für Windows Server Active Directory durchsetzen können](concept-password-ban-bad-on-premises.md), ist der nächste Schritt die Planung und Ausführung der Bereitstellung.

## <a name="deployment-strategy"></a>Bereitstellungsstrategie

Es wird empfohlen, Bereitstellungen im Überwachungsmodus zu starten. Der Überwachungsmodus ist die anfängliche Standardeinstellung, bei der Kennwörter weiterhin festgelegt werden können. Kennwörter, die gesperrt würden, werden im Ereignisprotokoll aufgezeichnet. Nachdem Sie die Proxyserver und DC-Agents im Überwachungsmodus bereitgestellt haben, sollten Sie die Auswirkungen überwachen, die die Kennwortrichtlinie auf Benutzer und die Umgebung hat, wenn die Richtlinie durchgesetzt wird.

Während der Überwachungsphase erlangen viele Organisationen die folgenden Erkenntnisse:

* Sie müssen bestehende Betriebsprozesse verbessern, um sicherere Kennwörter zu verwenden.
* Benutzer verwenden häufig unsichere Kennwörter.
* Sie müssen Benutzer über bevorstehende Änderungen der Sicherheitsmaßnahmen und deren mögliche Auswirkungen auf sie informieren und aufklären, wie sicherere Kennwörter ausgewählt werden können.

Es ist auch möglich, dass eine stärkere Kennwortüberprüfung die vorhandene Active Directory-Domänencontroller-Bereitstellungsautomatisierung beeinträchtigt. Es wird empfohlen, während der Auswertung des Überwachungszeitraums mindestens eine DC-Heraufstufung und eine DC-Herabstufung durchzuführen, um solche Probleme im Voraus erkennen zu können.  Weitere Informationen finden Sie unter

* [„Ntdsutil.exe“ kann kein unsicheres DSRM-Kennwort festlegen](howto-password-ban-bad-on-premises-troubleshoot.md##ntdsutilexe-fails-to-set-a-weak-dsrm-password)
* [Fehler beim Höherstufen des Domänencontrollerreplikats aufgrund eines unsicheren DSRM-Kennworts](howto-password-ban-bad-on-premises-troubleshoot.md#domain-controller-replica-promotion-fails-because-of-a-weak-dsrm-password)
* [Fehler beim Herabstufen des Domänencontrollers aufgrund eines unsicheren lokalen Administratorkennworts](howto-password-ban-bad-on-premises-troubleshoot.md#domain-controller-demotion-fails-due-to-a-weak-local-administrator-password)

Nachdem das Feature für einen angemessenen Zeitraum im Überwachungsmodus ausgeführt wurde, können Sie die Konfiguration von *Überwachen* auf *Erzwingen* umstellen, um sicherere Kennwörter zu verlangen. Eine gezielte Überwachung während dieser Zeit wird empfohlen.

## <a name="deployment-requirements"></a>Bereitstellungsanforderungen

* Lizenzanforderungen für den Azure AD-Kennwortschutz finden Sie im Artikel [Beseitigen falscher Kennwörter in Ihrer Organisation](concept-password-ban-bad.md#license-requirements).
* Alle Computer, auf denen die DC Agent-Software für Azure AD-Kennwortschutz installiert werden soll, müssen Windows Server 2012 oder höher ausführen. Diese Anforderung bedeutet nicht, dass sich die Active Directory-Domäne oder die Gesamtstruktur ebenfalls in einer Windows Server 2012-Domäne oder auf einer Funktionsebene der Gesamtstruktur befinden muss. Wie unter [Entwurfsprinzipien](concept-password-ban-bad-on-premises.md#design-principles) ausgeführt, gibt es für das Ausführen des DC-Agents oder der Proxy-Software keine Mindestanforderungen an die Domänenfunktionsebene (DFL) oder die Funktionsebene der Gesamtstruktur (FFL).
* Auf allen Computern, auf denen der DC-Agent-Dienst installiert werden soll, muss .NET 4.5 installiert sein.
* Alle Computer, auf denen der Azure AD-Kennwortschutz-Proxydienst installiert werden soll, müssen Windows Server 2012 R2 oder höher ausführen.
   > [!NOTE]
   > Die Bereitstellung von Proxydiensten ist eine obligatorische Voraussetzung für das Bereitstellen des Kennwortschutzes von Azure AD, auch wenn der Domänencontroller möglicherweise über eine direkte ausgehende Internetverbindung verfügt. 
   >
* Auf allen Computern, auf denen der Azure AD-Kennwortschutz-Proxydienst installiert werden soll, muss .NET 4.7 installiert sein.
  .NET 4.7 sollte bereits auf einem vollständig aktualisierten Windows-Server installiert sein. Laden Sie bei Bedarf das Installationsprogramm unter [.NET Framework 4.7-Offlineinstallationsprogramm für Windows](https://support.microsoft.com/help/3186497/the-net-framework-4-7-offline-installer-for-windows) herunter, und führen Sie es aus.
* Alle Computer (auch Domänencontroller), auf denen Azure AD-Kennwortschutzkomponenten installiert sind, müssen über eine Installation der Universal C-Runtime verfügen. Sie können die Runtime abrufen, indem Sie sicherstellen, dass Sie über alle Updates von Windows Update verfügen. Sie können sie auch in einem betriebssystemspezifischen Updatepaket abrufen. Weitere Informationen finden Sie unter [Update für die Universal C-Runtime in Windows](https://support.microsoft.com/help/2999226/update-for-uniersal-c-runtime-in-windows).
* Netzwerkkonnektivität muss zwischen mindestens einem Domänencontroller in jeder Domäne und mindestens einem Server bestehen, der den Proxydienst für Kennwortschutz hostet. Diese Konnektivität muss es dem Domänencontroller gestatten, auf den RPC-Endpunktzuordnungsport 135 und den RPC-Serverport für den Proxydienst zuzugreifen. Der RPC-Serverport ist standardmäßig ein dynamischer RPC-Port. Er kann jedoch so konfiguriert werden, dass er einen [statischen Port verwendet](#static).
* Alle Computer, auf denen der Proxydienst für den AD-Kennwortschutz installiert werden soll, müssen Netzwerkzugriff auf die folgenden Endpunkte besitzen:

    |**Endpunkt**|**Zweck**|
    | --- | --- |
    |`https://login.microsoftonline.com`|Authentifizierungsanforderungen|
    |`https://enterpriseregistration.windows.net`|Funktion für Azure AD-Kennwortschutz|
 
* Voraussetzungen für Microsoft Azure AD Connect Agent Updater

  Der Microsoft Azure AD Connect Agent Updater-Dienst wird zusammen mit dem Azure AD-Kennwortschutz-Proxydienst installiert. Damit der Microsoft Azure AD Connect Agent Updater-Dienst funktionieren kann, ist eine zusätzliche Konfiguration erforderlich:

  Wenn in Ihrer Umgebung ein HTTP-Proxyserver verwendet wird, müssen Sie die im Artikel [Verwenden von vorhandenen lokalen Proxyservern](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-configure-connectors-with-proxy-servers) angegebenen Richtlinien befolgen.

  Der Microsoft Azure AD Connect Agent Updater-Dienst erfordert auch die TLS 1.2-Schritte, die in den [TLS-Anforderungen](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-add-on-premises-application#tls-requirements) angegeben werden.

  Außerdem müssen Sie Netzwerkzugriff für die Ports und URLs aktivieren, die im Artikel [Setupprozeduren für die Anwendungsproxyumgebung](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-add-on-premises-application#prepare-your-on-premises-environment) angegeben sind.

  > [!WARNING]
  > Für den Azure AD-Kennwortschutz-Proxy und den Anwendungsproxy werden unterschiedliche Versionen des Microsoft Azure AD Connect Agent Updater-Diensts installiert. Daher beziehen sich die Anweisungen auf Anwendungsproxy-Inhalte. Diese verschiedenen Versionen sind nicht kompatibel, wenn sie parallel installiert werden. Es empfiehlt sich daher nicht, den Azure AD-Kennwortschutz-Proxy und den Anwendungsproxy zusammen auf demselben Computer zu installieren.

* Alle Computer, auf denen der Proxydienst für den Kennwortschutz gehostet wird, müssen so konfiguriert werden, dass Domänencontrollern die Anmeldung beim Proxydienst ermöglicht wird. Dies wird über die Zuweisung der Berechtigung „Auf diesen Computer vom Netzwerk aus zugreifen“ gesteuert.
* Alle Computer, die den Proxydienst für Kennwortschutz hosten, müssen so konfiguriert sein, dass sie ausgehenden HTTP-Datenverkehr mit TLS 1.2 zulassen.
* Ein globales Administratorkonto zum Registrieren des Proxydiensts für Kennwortschutz und der Gesamtstruktur bei Azure AD.
* Ein Konto mit Active Directory-Domänenadministratorrechten in der Stammdomäne der Gesamtstruktur, um die Windows Server Active Directory-Gesamtstruktur bei Azure AD zu registrieren.
* Alle Active Directory-Domänen, die die DC-Agent-Dienstsoftware ausführen, müssen DFSR (Distributed File System Replication) für die SYSVOL-Replikation verwenden.

  Sollte DFSR von Ihrer Domäne noch nicht verwendet werden, muss die Domäne vor der Installation des Azure AD-Kennwortschutzes für die Verwendung von DFSR migriert werden. Weitere Informationen finden Sie unter dem folgenden Link:

  [Migrationshandbuch für die SYSVOL-Replikation: Replikation von FRS zu DFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640019(v=ws.10))

  > [!WARNING]
  > Die DC-Agent-Software für den Azure AD-Kennwortschutz wird derzeit auf Domänencontrollern in Domänen installiert, von denen noch FRS (die Vorgängertechnologie zu DFSR) für die SYSVOL-Replikation verwendet wird. Die Software funktioniert in dieser Umgebung allerdings NICHT ordnungsgemäß. Dies macht sich unter anderem durch nicht erfolgreich replizierte Einzeldateien sowie durch scheinbar erfolgreiche SYSVOL-Wiederherstellungsprozeduren bemerkbar, bei denen jedoch nicht alle Dateien repliziert werden. Es empfiehlt sich, die Domäne baldmöglichst für die Verwendung von DFSR zu migrieren, um von den DFSR-Vorteilen zu profitieren und die Blockierung der Bereitstellung des Azure AD-Kennwortschutzes aufzuheben. Zukünftige Versionen der Software werden automatisch deaktiviert, wenn sie in einer Domäne ausgeführt werden, die noch FRS verwendet.

* Den Schlüsselverteilungsdienst muss auf allen Domänencontrollern in der Domäne aktiviert sein, auf denen Windows Server 2012 ausgeführt wird. Standardmäßig wird dieser Dienst über das Starten eines manuellen Triggers aktiviert.

## <a name="single-forest-deployment"></a>Bereitstellung in einer einzelnen Gesamtstruktur

Die folgende Abbildung zeigt, wie die grundlegenden Komponenten des Azure AD-Kennwortschutzes in einer lokalen Active Directory-Umgebung zusammenarbeiten.

![Zusammenspiel der Komponenten des Azure AD-Kennwortschutzes](./media/concept-password-ban-bad-on-premises/azure-ad-password-protection.png)

Es empfiehlt sich, sich vor der Bereitstellung mit der Funktionsweise der Software vertraut zu machen. Weitere Informationen finden Sie unter [Konzeptionelle Übersicht über den Azure AD-Kennwortschutz](concept-password-ban-bad-on-premises.md).

### <a name="download-the-software"></a>Herunterladen der Software

Es gibt zwei erforderliche Installationsprogramme für den Azure AD-Kennwortschutz. Sie sind im [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=57071) verfügbar.

### <a name="install-and-configure-the-proxy-service-for-password-protection"></a>Installieren und Konfigurieren des Proxydiensts für Kennwortschutz

1. Wählen Sie mindestens einen Server aus, auf dem der Proxydienst für den Kennwortschutz gehostet werden soll.
   * Jeder dieser Dienste kann nur Kennwortrichtlinien für eine einzelne Gesamtstruktur bereitstellen. Der Hostcomputer muss einer Domäne in dieser Gesamtstruktur angehören. Stamm- und untergeordnete Domänen werden unterstützt. Sie benötigen eine Netzwerkverbindung zwischen mindestens einem Domänencontroller in jeder Domäne der Gesamtstruktur und dem Kennwortschutzcomputer.
   * Sie können den Proxydienst zu Testzwecken auf einem Domänencontroller ausführen. Aber dieses Domänencontrollers muss dann über eine Internetverbindung verfügen, was möglicherweise ein Sicherheitsrisiko darstellt. Es wird empfohlen, diese Konfiguration nur zu Testzwecken zu verwenden.
   * Es wird empfohlen, mindestens zwei Proxyserver aus Gründen der Redundanz einzusetzen. Siehe [Hochverfügbarkeit](howto-password-ban-bad-on-premises-deploy.md#high-availability).

1. Installieren Sie den Azure AD-Kennwortschutz-Proxydienst mithilfe des Softwareinstallationsprogramms `AzureADPasswordProtectionProxySetup.exe`.
   * Nach der Softwareinstallation ist kein Neustart erforderlich. Die Softwareinstallation lässt sich mit Standard-MSI-Prozeduren automatisieren, z.B.:

      `AzureADPasswordProtectionProxySetup.exe /quiet`

      > [!NOTE]
      > Der Windows-Firewalldienst muss vor der Installation des Pakets „AzureADPasswordProtectionProxySetup.msi“ ausgeführt werden, um einen Installationsfehler zu vermeiden. Wenn die Windows-Firewall dafür konfiguriert ist, nicht ausgeführt zu werden, besteht die Problemumgehung im vorübergehenden Aktivieren und Ausführen des Firewalldiensts während des Installationsvorgangs. Die Proxysoftware weist nach der Installation keine Abhängigkeit von der Windows-Firewall auf. Wenn Sie eine Firewall eines Drittanbieters verwenden, muss auch diese zum Erfüllen der Anforderungen für die Bereitstellung konfiguriert sein. Dazu gehören Zugriff auf Port 135 für eingehenden Datenverkehr und auf den Proxy-RPC-Serverport. Siehe [Bereitstellungsanforderungen](howto-password-ban-bad-on-premises-deploy.md#deployment-requirements).

1. Öffnen Sie ein PowerShell-Fenster als Administrator.
   * Die Software für den Kennwortschutzproxy enthält ein neues PowerShell-Modul namens *AzureADPasswordProtection*. Die folgenden Schritte führen verschiedene Cmdlets aus diesem PowerShell-Modul aus. Importieren Sie das neue Modul wie folgt:

      ```powershell
      Import-Module AzureADPasswordProtection
      ```

   * Um sicherzustellen, dass der Dienst ausgeführt wird, verwenden Sie den folgenden PowerShell-Befehl:

      `Get-Service AzureADPasswordProtectionProxy | fl`.

     Das Ergebnis sollte einen **Status** „Wird ausgeführt“ anzeigen.

1. Registrieren Sie den Proxy.
   * Nachdem Schritt 3 abgeschlossen ist, wird der Proxydienst auf dem Computer ausgeführt. Aber der Dienst verfügt noch nicht über die erforderlichem Anmeldeinformationen für die Kommunikation mit Azure AD. Die Registrierung bei Azure AD ist erforderlich:

     `Register-AzureADPasswordProtectionProxy`

     Dieses Cmdlet erfordert die Anmeldeinformationen des globalen Administrators für Ihren Azure AD-Mandanten. Sie benötigen außerdem ein Konto mit lokalen Active Directory-Domänenadministratorberechtigungen in der Stammdomäne der Gesamtstruktur. Nachdem dieser Befehl ein Mal für einen Proxydienst erfolgreich war, sind weitere Aufrufe erfolgreich, aber nicht erforderlich.

      Das Cmdlet `Register-AzureADPasswordProtectionProxy` unterstützt die folgenden drei Authentifizierungsmodi.

     * Interaktiver Authentifizierungsmodus:

        ```powershell
        Register-AzureADPasswordProtectionProxy -AccountUpn 'yourglobaladmin@yourtenant.onmicrosoft.com'
        ```

        > [!NOTE]
        > Dieser Modus funktioniert nicht auf Server Core-Betriebssystemen. Verwenden Sie stattdessen einen der folgenden Authentifizierungsmechanismen. Dieser Modus kann fehlschlagen, wenn verstärkte Sicherheitskonfiguration für Internet Explorer aktiviert ist. Die Problemumgehung besteht darin, diese Konfiguration zu deaktivieren, den Proxy zu registrieren und die Konfiguration dann erneut zu aktivieren.

     * Wählen Sie den Gerätecode-Authentifizierungsmodus aus:

        ```powershell
        Register-AzureADPasswordProtectionProxy -AccountUpn 'yourglobaladmin@yourtenant.onmicrosoft.com' -AuthenticateUsingDeviceCode
        To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code XYZABC123 to authenticate.
        ```

        Anschließend schließen Sie die Authentifizierung mithilfe der auf einem anderen Gerät angezeigten Anweisungen ab.

     * Automatischer (kennwortbasierter) Authentifizierungsmodus:

        ```powershell
        $globalAdminCredentials = Get-Credential
        Register-AzureADPasswordProtectionProxy -AzureCredential $globalAdminCredentials
        ```

        > [!NOTE]
        > Diese Modus ist nicht erfolgreich, wenn Azure Multi-Factor Authentication für Ihr Konto erforderlich ist. Verwenden Sie in diesem Fall einen der beiden vorherigen Authentifizierungsmodi oder ein anderes Konto, für das keine mehrstufige Authentifizierung erforderlich ist.
        >
        > Eine mehrstufige Authentifizierung kann auch erforderlich sein, wenn der (im Hintergrund durch den Azure AD-Kennwortschutz verwendete) Azure-Geräteregistrierungsdienst so konfiguriert wurde, dass global eine mehrstufige Authentifizierung erzwungen wird. Zur Umgehung dieses Problems können Sie ein anderes Konto verwenden, das die mehrstufige Authentifizierung mit einem der beiden vorherigen Authentifizierungsmodi unterstützt, oder die MFA-Anforderung des Azure-Geräteregistrierungsdients vorübergehend lockern. Navigieren Sie hierzu im Azure-Verwaltungsportal zu „Azure Active Directory“ > „Geräte“ > „Geräteeinstellungen“, und legen Sie die Einstellung „Mehrstufige Authentifizierung zum Hinzufügen von Geräten erforderlich“ auf „Nein“ fest. Denken Sie daran, diese Einstellung nach Abschluss der Registrierung wieder auf „Ja“ zurücksetzen.
        >
        > MFA-Anforderungen sollten ausschließlich zu Testzwecken umgangen werden.

       Sie müssen zurzeit den Parameter *-ForestCredential* nicht angeben, der für zukünftige Funktionalität reserviert ist.

   Die Registrierung des Proxydiensts für den Kennwortschutz ist nur ein Mal während der Lebensdauer des Diensts erforderlich. Danach führt der Proxydienst automatisch alle weiteren notwendigen Wartungen durch.

   > [!TIP]
   > Es kann zu einer spürbaren Verzögerung bis zum Abschluss kommen, wenn dieses Cmdlet zum ersten Mal für einen bestimmten Azure-Mandanten ausgeführt wird. Wenn kein Fehler gemeldet wird, müssen Sie sich keine Gedanken über diese Verzögerung machen.

1. Registrieren Sie die Gesamtstruktur.
   * Sie müssen die lokale Active Directory-Gesamtstruktur mit den erforderlichen Anmeldeinformationen für die Kommunikation mit Azure unter Verwendung des PowerShell-Cmdlets `Register-AzureADPasswordProtectionForest` initialisieren. Das Cmdlet erfordert die Anmeldeinformationen des globalen Administrators für Ihren Azure AD-Mandanten. Es benötigt außerdem lokale Active Directory-Unternehmensadministratorrechte. Dieser Schritt wird pro Gesamtstruktur einmal ausgeführt.

      Das Cmdlet `Register-AzureADPasswordProtectionForest` unterstützt die folgenden drei Authentifizierungsmodi.

     * Interaktiver Authentifizierungsmodus:

        ```powershell
        Register-AzureADPasswordProtectionForest -AccountUpn 'yourglobaladmin@yourtenant.onmicrosoft.com'
        ```

        > [!NOTE]
        > Dieser Modus funktioniert nicht auf Server Core-Betriebssystemen. Verwenden Sie stattdessen einen der folgenden zwei Authentifizierungsmechanismen. Dieser Modus kann fehlschlagen, wenn verstärkte Sicherheitskonfiguration für Internet Explorer aktiviert ist. Die Problemumgehung besteht darin, diese Konfiguration zu deaktivieren, die Gesamtstruktur zu registrieren und die Konfiguration dann wieder zu aktivieren.  

     * Wählen Sie den Gerätecode-Authentifizierungsmodus aus:

        ```powershell
        Register-AzureADPasswordProtectionForest -AccountUpn 'yourglobaladmin@yourtenant.onmicrosoft.com' -AuthenticateUsingDeviceCode
        To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code XYZABC123 to authenticate.
        ```

        Anschließend schließen Sie die Authentifizierung mithilfe der auf einem anderen Gerät angezeigten Anweisungen ab.

     * Automatischer (kennwortbasierter) Authentifizierungsmodus:

        ```powershell
        $globalAdminCredentials = Get-Credential
        Register-AzureADPasswordProtectionForest -AzureCredential $globalAdminCredentials
        ```

        > [!NOTE]
        > Diese Modus ist nicht erfolgreich, wenn Azure Multi-Factor Authentication für Ihr Konto erforderlich ist. Verwenden Sie in diesem Fall einen der beiden vorherigen Authentifizierungsmodi oder ein anderes Konto, für das keine mehrstufige Authentifizierung erforderlich ist.
        >
        > Eine mehrstufige Authentifizierung kann auch erforderlich sein, wenn der (im Hintergrund durch den Azure AD-Kennwortschutz verwendete) Azure-Geräteregistrierungsdienst so konfiguriert wurde, dass global eine mehrstufige Authentifizierung erzwungen wird. Zur Umgehung dieses Problems können Sie ein anderes Konto verwenden, das die mehrstufige Authentifizierung mit einem der beiden vorherigen Authentifizierungsmodi unterstützt, oder die MFA-Anforderung des Azure-Geräteregistrierungsdients vorübergehend lockern. Navigieren Sie hierzu im Azure-Verwaltungsportal zu „Azure Active Directory“ > „Geräte“ > „Geräteeinstellungen“, und legen Sie die Einstellung „Mehrstufige Authentifizierung zum Hinzufügen von Geräten erforderlich“ auf „Nein“ fest. Denken Sie daran, diese Einstellung nach Abschluss der Registrierung wieder auf „Ja“ zurücksetzen.
        >
        > MFA-Anforderungen sollten ausschließlich zu Testzwecken umgangen werden.

       Diese Beispiele funktionieren nur, wenn der aktuell angemeldete Benutzer auch ein Active Directory-Domänenadministrator für die Stammdomäne ist. Wenn dies nicht der Fall ist, können die alternativen Anmeldeinformationen für die Domäne auch über den Parameter *-ForestCredential* angegeben werden.

   > [!NOTE]
   > Wenn mehrere Proxyserver in Ihrer Umgebung installiert sind, spielt es keine Rolle, welcher Proxyserver zum Registrieren der Gesamtstruktur verwendet wird.
   >
   > [!TIP]
   > Es kann zu einer spürbaren Verzögerung bis zum Abschluss kommen, wenn dieses Cmdlet zum ersten Mal für einen bestimmten Azure-Mandanten ausgeführt wird. Wenn kein Fehler gemeldet wird, müssen Sie sich keine Gedanken über diese Verzögerung machen.

   Die Registrierung der Active Directory-Gesamtstruktur ist nur ein Mal während der Lebensdauer der Gesamtstruktur erforderlich. Danach führen die Domänencontroller-Agents in der Gesamtstruktur automatisch alle weiteren notwendigen Wartungen durch. Nachdem `Register-AzureADPasswordProtectionForest` erfolgreich für eine Gesamtstruktur ausgeführt wurde, sind weitere Aufrufe des Cmdlets erfolgreich, aber nicht erforderlich.

   Damit `Register-AzureADPasswordProtectionForest` erfolgreich ist, muss mindestens ein Domänencontroller, der Windows Server 2012 oder höher ausführt, in der Domäne des Proxyservers verfügbar sein. Die DC-Agent-Software muss vor diesem Schritt nicht auf einem Domänencontroller installiert sein.

1. Konfigurieren Sie den Proxydienst für Kennwortschutz so, dass er über einen HTTP-Proxy kommuniziert.

   Wenn Ihre Umgebung die Verwendung eines bestimmten HTTP-Proxys für die Kommunikation mit Azure erfordert, verwenden Sie die folgende Methode: Erstellen Sie eine Datei *AzureADPasswordProtectionProxy.exe.config* im Ordner „%Programme%\Azure AD Password Protection Proxy\Service“. Beziehen Sie die folgenden Inhalte ein:

      ```xml
      <configuration>
        <system.net>
          <defaultProxy enabled="true">
           <proxy bypassonlocal="true"
               proxyaddress="http://yourhttpproxy.com:8080" />
          </defaultProxy>
        </system.net>
      </configuration>
      ```

   Wenn Ihr HTTP-Proxy Authentifizierung erfordert, fügen Sie das *useDefaultCredentials*-Tag hinzu:

      ```xml
      <configuration>
        <system.net>
          <defaultProxy enabled="true" useDefaultCredentials="true">
           <proxy bypassonlocal="true"
               proxyaddress="http://yourhttpproxy.com:8080" />
          </defaultProxy>
        </system.net>
      </configuration>
      ```

   In beiden Fällen ersetzen Sie `http://yourhttpproxy.com:8080` durch die Adresse und den Port Ihres jeweiligen HTTP-Proxyservers.

   Wenn Ihr HTTP-Proxy für die Verwendung einer Autorisierungsrichtlinie konfiguriert ist, müssen Sie den Zugriff auf das Active Directory-Computerkonto des Computers gewähren, der den Proxydienst für den Kennwortschutz hostet.

   Es wird empfohlen, den Proxydienst zu beenden und neu zu starten, nachdem Sie die Datei *AzureADPasswordProtectionProxy.exe.config* erstellt oder aktualisiert haben.

   Der Proxydienst unterstützt nicht die Verwendung von spezifischen Anmeldeinformationen für das Herstellen einer Verbindung mit einem HTTP-Proxy.

1. Optional: Konfigurieren Sie den Proxydienst für Kennwortschutz so, dass er an einem bestimmten Port lauscht.
   * Die DC-Agent-Software für Kennwortschutz auf den Domänencontrollern verwendet RPC über TCP für die Kommunikation mit dem Proxydienst. Standardmäßig lauscht der Proxydienst an jedem verfügbaren dynamischen RPC-Endpunkt. Sie können den Dienst jedoch so konfigurieren, dass er an einem bestimmten TCP-Port lauscht, wenn dies aufgrund von Netzwerktopologie- oder Firewallanforderungen in Ihrer Umgebung erforderlich ist.
      * <a id="static" /></a>Verwenden Sie das Cmdlet `Set-AzureADPasswordProtectionProxyConfiguration`, um den Dienst für die Ausführung unter einem statischen Port zu konfigurieren.

         ```powershell
         Set-AzureADPasswordProtectionProxyConfiguration –StaticPort <portnumber>
         ```

         > [!WARNING]
         > Sie müssen den Dienst beenden und neu starten, damit die Änderungen wirksam werden.

      * Um den Dienst so zu konfigurieren, dass er unter einem dynamischen Port ausgeführt wird, verwenden Sie die gleiche Prozedur, legen *StaticPort* dabei aber wieder auf Null fest:

         ```powershell
         Set-AzureADPasswordProtectionProxyConfiguration –StaticPort 0
         ```

         > [!WARNING]
         > Sie müssen den Dienst beenden und neu starten, damit die Änderungen wirksam werden.

   > [!NOTE]
   > Der Proxydienst für Kennwortschutz erfordert einen manuellen Neustart nach jeder Änderung der Portkonfiguration. Sie müssen jedoch die DC-Agent-Dienstsoftware auf Domänencontrollern nicht neu starten, nachdem Sie diese Konfigurationsänderungen vorgenommen haben.

   * Um die aktuelle Konfiguration des Diensts abzufragen, verwenden Sie das Cmdlet `Get-AzureADPasswordProtectionProxyConfiguration`:

      ```powershell
      Get-AzureADPasswordProtectionProxyConfiguration | fl

      ServiceName : AzureADPasswordProtectionProxy
      DisplayName : Azure AD password protection Proxy
      StaticPort  : 0
      ```

### <a name="install-the-dc-agent-service"></a>Installieren des DC-Agent-Diensts

   Installieren Sie den DC-Agent-Dienst für Kennwortschutz mithilfe des `AzureADPasswordProtectionDCAgentSetup.msi`-Pakets.

   Die Installation oder Deinstallation der Software erfordert einen Neustart. Dies liegt daran, dass Kennwortfilter-DLLs nur durch einen Neustart geladen oder entladen werden.

   Sie können den DC-Agent-Dienst auf einem Computer installieren, der noch kein Domänencontroller ist. In diesem Fall wird der Dienst gestartet und ausgeführt, bleibt aber inaktiv, bis der Computer zum Domänencontroller höher gestuft wird.

   Die Softwareinstallation kann mit MSI-Standardprozeduren automatisiert werden. Beispiel:

   `msiexec.exe /i AzureADPasswordProtectionDCAgentSetup.msi /quiet /qn /norestart`

   Sie können das `/norestart`-Flag auslassen, wenn Sie es vorziehen, dass der Computer vom Installationsprogramm automatisch neu gestartet wird.

Die Installation ist abgeschlossen, nachdem die DC-Agent-Software auf einem Domänencontroller installiert und dieser Computer neu gestartet wurde. Eine andere Konfiguration ist weder erforderlich noch möglich.

## <a name="upgrading-the-proxy-agent"></a>Upgrade des Proxy-Agents

Wenn eine neuere Version der Azure AD-Kennwortschutz-Proxysoftware verfügbar ist, wird das Upgrade durch Ausführen der neuesten Version des Softwareinstallationsprogramms `AzureADPasswordProtectionProxySetup.exe` durchgeführt. Die aktuelle Version der Software ist im [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=57071) verfügbar.

Es ist nicht erforderlich, die aktuelle Version der Proxysoftware zu deinstallieren. Das Installationsprogramm führt ein direktes Upgrade durch. Beim Upgrade der Proxysoftware sollte kein Neustart erforderlich sein. Das Softwareupgrade kann mit MSI-Standardprozeduren automatisiert werden, z.B. `AzureADPasswordProtectionProxySetup.exe /quiet`.

Der Proxy-Agent unterstützt das automatische Upgrade. Für das automatische Upgrade wird der Microsoft Azure AD Connect Agent Updater-Dienst verwendet, der zusammen mit dem Proxydienst installiert wird. Das automatische Upgrade ist standardmäßig aktiviert und kann mit dem Cmdlet `Set-AzureADPasswordProtectionProxyConfiguration` aktiviert oder deaktiviert werden. Die aktuelle Einstellung kann mit dem Cmdlet `Get-AzureADPasswordProtectionProxyConfiguration` abgefragt werden. Microsoft empfiehlt, die Einstellung für das automatische Upgrade immer aktiviert zu lassen.

Mit dem Cmdlet `Get-AzureADPasswordProtectionProxy` kann die Softwareversion aller aktuell installierten Proxy-Agents in einer Gesamtstruktur abgefragt werden.

## <a name="upgrading-the-dc-agent"></a>Upgrade des DC-Agents

Wenn eine neuere Version der DC-Agent-Software für den Azure AD-Kennwortschutz verfügbar ist, wird das Upgrade durch Ausführen der neuesten Version des Softwarepakets `AzureADPasswordProtectionDCAgentSetup.msi` durchgeführt. Die aktuelle Version der Software ist im [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=57071) verfügbar.

Es ist nicht erforderlich, die aktuelle Version der DC-Agent-Software zu deinstallieren. Das Installationsprogramm führt ein direktes Upgrade durch. Beim Upgrade der DC-Agent-Software ist immer ein Neustart erforderlich. Diese Anforderung wird durch das Kernverhalten von Windows verursacht. 

Das Softwareupgrade kann mit MSI-Standardprozeduren automatisiert werden, z.B. `msiexec.exe /i AzureADPasswordProtectionDCAgentSetup.msi /quiet /qn /norestart`.

Sie können das `/norestart`-Flag auslassen, wenn Sie es vorziehen, dass der Computer vom Installationsprogramm automatisch neu gestartet wird.

Mit dem Cmdlet `Get-AzureADPasswordProtectionDCAgent` kann die Softwareversion aller aktuell installierten DC-Agents in einer Gesamtstruktur abgefragt werden.

## <a name="multiple-forest-deployments"></a>Bereitstellungen in mehreren Gesamtstrukturen

Für die Bereitstellung des Azure AD-Kennwortschutzes in mehreren Gesamtstrukturen gibt es keine zusätzlichen Anforderungen. Jede Gesamtstruktur wird unabhängig voneinander konfiguriert, wie im Abschnitt „Bereitstellung in einer einzelnen Gesamtstruktur“ beschrieben wird. Jeder Kennwortschutzproxy kann nur Domänencontroller aus der Gesamtstruktur unterstützen, mit der er verknüpft ist. Unabhängig von der in Active Directory konfigurierten Vertrauensstellung besitzt die Kennwortschutzsoftware in einer bestimmten Gesamtstruktur keine Informationen über eine Azure AD-Kennwortschutzsoftware, die in einer anderen Gesamtstruktur bereitgestellt ist.

## <a name="read-only-domain-controllers"></a>Schreibgeschützte Domänencontroller

Änderungen/Festlegungen von Kennwörtern werden auf schreibgeschützten Domänencontrollern (RODCs) nicht verarbeitet und gespeichert. Sie werden an nicht schreibgeschützte Domänencontroller weitergeleitet. Eine Installation der DC-Agent-Software auf RODCs ist daher nicht erforderlich.

## <a name="high-availability"></a>Hochverfügbarkeit

Bei der Hochverfügbarkeit des Kennwortschutzes geht es hauptsächlich darum, die Verfügbarkeit der Proxyserver zu gewährleisten, wenn Domänencontroller in einer Gesamtstruktur versuchen, neue Richtlinien oder andere Daten aus Azure herunterzuladen. Jeder DC-Agent verwendet bei der Entscheidung, welcher Proxyserver aufgerufen werden soll, einen einfachen Roundrobin-Algorithmus. Der Agent überspringt Proxyserver, die nicht antworten. Für die meisten vollständig verbundenen Active Directory-Bereitstellungen, die eine integre Replikation des Verzeichnisses und SYSVOL-Ordnerstatus aufweisen, reichen zwei Proxyserver aus, um die Verfügbarkeit sicherzustellen. Dies führt zum rechtzeitigen Download neuer Richtlinien und anderer Daten. Aber Sie können zusätzliche Proxyserver bereitstellen.

Die üblichen Probleme, die mit Hochverfügbarkeit einhergehen, werden durch den Entwurf der DC-Agent-Software verringert. Der DC-Agent verwaltet einen lokalen Cache, in dem sich die zuletzt heruntergeladene Kennwortrichtlinie befindet. Selbst wenn alle registrierten Proxyserver nicht mehr verfügbar sind, setzen die DC-Agents ihre zwischengespeicherten Kennwortrichtlinien durch. Eine angemessene Aktualisierungshäufigkeit für Kennwortrichtlinien in einer umfangreichen Bereitstellung liegt für gewöhnlich in der Größenordnung von Tagen, nicht Stunden oder weniger. Kurze Ausfälle der Proxyserver haben daher keinen wesentlichen Einfluss auf den Azure AD-Kennwortschutz.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun die für den Azure AD-Kennwortschutz erforderlichen Dienste auf Ihren lokalen Servern installiert haben, lesen Sie [Ausführen von Konfigurationsaufgaben nach der Installation und Sammeln von Berichtsinformationen](howto-password-ban-bad-on-premises-operations.md), um Ihre Bereitstellung abzuschließen.

[Konzeptionelle Übersicht über den Azure AD-Kennwortschutz](concept-password-ban-bad-on-premises.md)
