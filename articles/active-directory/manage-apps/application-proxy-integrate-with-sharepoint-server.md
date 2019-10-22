---
title: Aktivieren des Remotezugriffs auf SharePoint per Azure AD-Anwendungsproxy | Microsoft-Dokumentation
description: Behandelt die Grundlagen zur Integration eines lokalen SharePoint-Servers mit Azure AD-Anwendungsproxy.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/02/2019
ms.author: mimart
ms.reviewer: japere
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: a8f9cba1072866a3efdeb7b99981d0bfda22ab00
ms.sourcegitcommit: e0a1a9e4a5c92d57deb168580e8aa1306bd94723
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2019
ms.locfileid: "72286112"
---
# <a name="enable-remote-access-to-sharepoint-with-azure-ad-application-proxy"></a>Aktivieren des Remotezugriffs auf SharePoint per Azure AD-Anwendungsproxy

In dieser Schritt-für-Schritt-Anleitung wird beschrieben, wie Sie eine lokale SharePoint-Farm mit dem Anwendungsproxy von Azure Active Directory (Azure AD) integrieren.

## <a name="prerequisites"></a>Voraussetzungen

Um die Konfiguration vornehmen zu können, benötigen Sie die folgenden Ressourcen:
- Eine SharePoint 2013-Farm (oder neuer).
- Einen Azure Active Directory-Mandanten mit einem Plan, der den Anwendungsproxy enthält. Erfahren Sie mehr über die [Azure Active Directory-Pläne und -Preise](https://azure.microsoft.com/pricing/details/active-directory/).
- Eine [benutzerdefinierte überprüfte Domäne](../fundamentals/add-custom-domain.md) im Azure AD-Mandanten.
- Eine mit Azure AD Connect synchronisierte lokale Active Directory-Instanz mit Benutzern, die sich [bei Azure anmelden](../hybrid/plan-connect-user-signin.md) können.
- Einen Anwendungsproxyconnector, der auf einem Computer in der Unternehmensdomäne installiert ist und ausgeführt wird.

Für die Konfiguration von SharePoint mit dem Anwendungsproxy sind zwei URLs erforderlich:
- Eine externe URL, die für Endbenutzer sichtbar und in Azure Active Directory festgelegt ist. Diese URL kann eine benutzerdefinierte Domäne enthalten. Weitere Informationen finden Sie unter [Arbeiten mit benutzerdefinierten Domänen im Azure AD-Anwendungsproxy](application-proxy-configure-custom-domain.md).
- Eine interne URL, die nur innerhalb der Unternehmensdomäne bekannt ist und nie direkt verwendet wird.

> [!IMPORTANT]
> Befolgen Sie die folgenden Empfehlungen für die interne URL, um sicherzustellen, dass die Links ordnungsgemäß zugeordnet werden:
> - Verwenden Sie HTTPS.
> - Verwenden Sie keine benutzerdefinierten Ports.
> - Erstellen Sie im Unternehmens-DNS einen Host (A), der auf den SharePoint-WFE (oder Lastenausgleich) verweist, und keinen Alias (CName).

In diesem Artikel werden die folgenden Werte verwendet:
- Interne URL: `https://sharepoint`
- Externe URL: `https://spsites-demo1984.msappproxy.net/`
- Anwendungspoolkonto für die SharePoint-Webanwendung: `Contoso\spapppool`

## <a name="step-1-configure-an-application-in-azure-ad-that-uses-application-proxy"></a>Schritt 1: Konfigurieren einer Anwendung in Azure AD, die den Anwendungsproxy verwendet

In diesem Schritt erstellen Sie in Ihrem Azure Active Directory-Mandanten eine Anwendung, die den Anwendungsproxy verwendet. Sie legen die externe URL fest und geben die interne URL an, die beide später in SharePoint verwendet werden.

1. Erstellen Sie die App mit den folgenden Einstellungen gemäß der Beschreibung. Eine ausführliche Anleitung finden Sie unter [Veröffentlichen von Anwendungen mit Azure AD-Anwendungsproxy](application-proxy-add-on-premises-application.md#add-an-on-premises-app-to-azure-ad).
   * **Interne URL:** Interne SharePoint-URL, die später in SharePoint festgelegt wird, z. B. `https://sharepoint`.
   * **Methode für die Vorauthentifizierung**: Azure Active Directory
   * **URL in Headern übersetzen**: Nein
   * **URL im Hauptteil der Anwendung übersetzen**: Nein

   ![Veröffentlichen von SharePoint als Anwendung](./media/application-proxy-integrate-with-sharepoint-server/publish-app.png)

1. Nachdem die App veröffentlicht wurde, konfigurieren Sie die Einstellungen für einmaliges Anmelden anhand der folgenden Schritte:

   1. Wählen Sie auf der Anwendungsseite im Portal **Einmaliges Anmelden** aus.
   1. Legen Sie den **Modus für einmaliges Anmelden** auf **Integrierte Windows-Authentifizierung** fest.
   1. Legen Sie **Interner Anwendungs-SPN** auf den zuvor festgelegten Wert fest. In diesem Beispiel wird der Wert `HTTP/sharepoint` verwendet.
   1. Wählen Sie im Feld **Delegierte Identität für Anmeldung** die am besten geeignete Option für Ihre Active Directory-Gesamtstrukturkonfiguration aus. Wenn Sie beispielsweise in Ihrer Gesamtstruktur über eine einzelne AD-Domäne verfügen, wählen Sie **Name des lokalen SAM-Kontos** aus (wie unten dargestellt), aber wenn sich Ihre Benutzer nicht in derselben Domäne wie SharePoint und die Anwendungsproxy-Connectorserver befinden, wählen Sie **Lokaler Benutzerprinzipalname** aus (nicht dargestellt).

   ![Konfigurieren der integrierten Windows-Authentifizierung für einmaliges Anmelden](./media/application-proxy-integrate-with-sharepoint-server/configure-iwa.png)

1. Zum Abschluss der Anwendungseinrichtung wechseln Sie zum Abschnitt **Benutzer und Gruppen**. Weisen Sie dort Benutzer zu, die Zugriff auf diese Anwendung erhalten. 

## <a name="step-2-configure-the-sharepoint-web-application"></a>Schritt 2: Konfigurieren der SharePoint-Webanwendung

Die SharePoint-Webanwendung muss mit Kerberos und den entsprechenden alternativen Zugriffszuordnungen konfiguriert werden, damit sie ordnungsgemäß mit dem Azure AD-Anwendungsproxy funktioniert. Es gibt zwei mögliche Optionen:

1. Erstellen Sie eine neue Webanwendung, und verwenden Sie nur die Standardzone. Dies ist die bevorzugte Option für die optimale Verwendung von SharePoint (die Links in den von SharePoint generierten E-Mail-Benachrichtigungen verweisen beispielsweise immer auf die Standardzone).
1. Erweitern Sie eine vorhandene Webanwendung so, dass Kerberos für eine nicht standardmäßige Zone konfiguriert wird.

> [!IMPORTANT]
> Das Anwendungspoolkonto der SharePoint-Webanwendung muss unabhängig von der verwendeten Zone ein Domänenkonto sein, damit Kerberos ordnungsgemäß funktioniert.

### <a name="provision-the-sharepoint-web-application"></a>Bereitstellen der SharePoint-Webanwendung

- Gehen Sie wie folgt vor, wenn Sie eine neue Webanwendung erstellen und nur die Standardzone verwenden (bevorzugte Option):

    1. Starten Sie die **SharePoint-Verwaltungsshell**, und führen Sie das folgende Skript aus:

       ```powershell
       # This script creates a web application and configures the Default zone with the internal/external URL needed to work with Azure AD Application Proxy
       # Edit variables below to fit your environment. Note that the managed account must exist and it must be a domain account
       $internalUrl = "https://sharepoint"
       $externalUrl = "https://spsites-demo1984.msappproxy.net/"
       $applicationPoolManagedAccount = "Contoso\spapppool"
            
       $winAp = New-SPAuthenticationProvider -UseWindowsIntegratedAuthentication -DisableKerberos:$false
       $wa = New-SPWebApplication -Name "SharePoint - AAD Proxy" -Port 443 -SecureSocketsLayer -URL $externalUrl -ApplicationPool "SharePoint - AAD Proxy" -ApplicationPoolAccount (Get-SPManagedAccount $applicationPoolManagedAccount) -AuthenticationProvider $winAp
       New-SPAlternateURL -Url $internalUrl -WebApplication $wa -Zone Default -Internal
       ```

    1. Öffnen Sie die Website der **SharePoint-Zentraladministration**.
    1. Wählen Sie unter **Systemeinstellungen** die Option **Alternative Zugriffszuordnungen konfigurieren**. Das Feld „Alternative Zugriffszuordnungen“ wird geöffnet.
    1. Filtern Sie die Anzeige mit der neuen Webanwendung, und vergewissern Sie sich, dass die Anzeige in etwa wie folgt aussieht:

       ![Alternative Zugriffszuordnungen der Webanwendung](./media/application-proxy-integrate-with-sharepoint-server/new-webapp-aam.png)

- Gehen Sie wie folgt vor, wenn Sie eine vorhandene Webanwendung auf eine neue Zone erweitern (falls Sie die Standardzone nicht verwenden können):

    1. Starten Sie die **SharePoint-Verwaltungsshell**, und führen Sie das folgende Skript aus:

       ```powershell
       # This scripts extends an existing web application to Internet zone with the internal/external URL needed to work with Azure AD Application Proxy
       # Edit variables below to fit your environment
       $webAppUrl = "http://spsites/"
       $internalUrl = "https://sharepoint"
       $externalUrl = "https://spsites-demo1984.msappproxy.net/"
       
       $winAp = New-SPAuthenticationProvider -UseWindowsIntegratedAuthentication -DisableKerberos:$false
       $wa = Get-SPWebApplication $webAppUrl
       New-SPWebApplicationExtension -Name "SharePoint - AAD Proxy" -Identity $wa -SecureSocketsLayer -Zone Extranet -Url $externalUrl -AuthenticationProvider $winAp
       New-SPAlternateURL -Url $internalUrl -WebApplication $wa -Zone Extranet -Internal
       ```

    1. Öffnen Sie die Website der **SharePoint-Zentraladministration**.
    1. Wählen Sie unter **Systemeinstellungen** die Option **Alternative Zugriffszuordnungen konfigurieren**. Das Feld „Alternative Zugriffszuordnungen“ wird geöffnet.
    1. Filtern Sie die Anzeige mit der erweiterten Webanwendung, und vergewissern Sie sich, dass die Anzeige in etwa wie folgt aussieht:

        ![Alternative Zugriffszuordnungen der erweiterten Anwendung](./media/application-proxy-integrate-with-sharepoint-server/extend-webapp-aam.png)

### <a name="ensure-that-the-sharepoint-web-application-is-running-under-a-domain-account"></a>Sicherstellen, dass die SharePoint-Webanwendung unter einem Domänenkonto ausgeführt wird

Führen Sie die folgenden Schritte aus, um das Konto zu identifizieren, unter dem der Anwendungspool der SharePoint-Webanwendung ausgeführt wird, und sicherzustellen, dass es sich dabei um ein Domänenkonto handelt:

1. Öffnen Sie die Website der **SharePoint-Zentraladministration**.
1. Navigieren Sie zu **Sicherheit**, und wählen Sie die Option **Dienstkonten konfigurieren**.
1. Wählen Sie **Webanwendungspool – YourWebApplicationName** aus.

   ![Optionen für das Konfigurieren eines Dienstkontos](./media/application-proxy-integrate-with-sharepoint-server/service-web-application.png)

1. Vergewissern Sie sich, dass im Feld **Wählen Sie ein Konto für diese Komponente aus** ein Domänenkonto angezeigt wird, und merken Sie sich dieses Konto, weil es im nächsten Schritt benötigt wird.

### <a name="ensure-that-an-https-certificate-is-configured-for-the-iis-site-of-the-extranet-zone"></a>Sicherstellen, dass ein HTTPS-Zertifikat für die IIS-Website der Zone „Extranet“ konfiguriert ist

Weil die interne URL das HTTPS-Protokoll (`https://SharePoint/`) verwendet, muss auf der IIS-Website ein Zertifikat festgelegt werden.

1. Öffnen Sie die Windows PowerShell-Konsole.
1. Führen Sie das folgende Skript aus, um ein selbstsigniertes Zertifikat zu generieren und es dem Computerzertifikatspeicher „My“ hinzuzufügen:

   ```powershell
   # Replace "SharePoint" with the actual hostname of the Internal URL of your Azure AD proxy application
   New-SelfSignedCertificate -DnsName "SharePoint" -CertStoreLocation "cert:\LocalMachine\My"
   ```

   > [!IMPORTANT]
   > Selbstsignierte Zertifikate sind nur für Testzwecke geeignet. In Produktionsumgebungen wird dringend empfohlen, stattdessen von einer Zertifizierungsstelle ausgestellte Zertifikate zu verwenden.

1. Öffnen Sie die Konsole „Internetinformationsdienste-Manager“.
1. Erweitern Sie den Server in der Strukturansicht, erweitern Sie „Websites“, wählen Sie die Website „SharePoint – AAD-Proxy“ aus, und klicken Sie dann auf **Bindungen**.
1. Wählen Sie „HTTPS-Bindung“ aus, und klicken Sie auf **Bearbeiten**.
1. Wählen Sie im Feld „SSL-Zertifikat“ das **SharePoint**-Zertifikat aus, und klicken Sie auf „OK“.

Sie können auf die SharePoint-Website jetzt extern über den Azure AD-Anwendungsproxy zugreifen.

## <a name="step-3-configure-kerberos-constrained-delegation-kcd"></a>Schritt 3: Konfigurieren der eingeschränkten Kerberos-Delegierung (Kerberos Constrained Delegation, KCD)

Benutzer authentifizieren sich anfangs bei Azure Active Directory und dann bei SharePoint, wobei diese Authentifizierung mithilfe von Kerberos über den Azure AD-Proxyconnector erfolgt. Damit der Connector ein Kerberos-Token im Namen des Azure Active Directory-Benutzers abrufen kann, muss die eingeschränkte Kerberos-Delegierung mit Protokollübergang konfiguriert werden. Weitere Informationen zu KCD finden Sie unter [Eingeschränkte Kerberos-Delegierung: Übersicht](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj553400(v=ws.11)).

### <a name="set-the-service-principal-name-for-the-sharepoint-service-account"></a>Festlegen des Dienstprinzipalnamens für das SharePoint-Dienstkonto

In diesem Artikel wird die interne URL `https://sharepoint` verwendet. Daher lautet der SPN `HTTP/sharepoint`. Sie müssen diese Werte durch die Werte ersetzen, die Ihrer Umgebung entsprechen.
Um den SPN `HTTP/sharepoint` für das SharePoint-Anwendungspoolkonto `Contoso\spapppool` zu registrieren, führen Sie den folgenden Befehl als Administrator der Domäne an einer Eingabeaufforderung aus:

`setspn -S HTTP/sharepoint Contoso\spapppool`

Mit dem Befehl „setspn“ wird nach dem SPN gesucht, bevor er hinzugefügt wird. Falls er bereits vorhanden ist, wird der Fehler **Doppelter SPN-Wert** angezeigt. In diesem Fall sollten Sie den vorhandenen SPN entfernen, wenn er nicht unter dem richtigen Anwendungspoolkonto festgelegt ist.  
Sie können überprüfen, ob der SPN erfolgreich hinzugefügt wurde, indem Sie den Befehl „Setspn“ mit der Option „-L“ ausführen. Weitere Informationen zu diesem Befehl finden Sie unter [Setspn](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/cc731241(v=ws.11)).

### <a name="ensure-that-the-connector-is-trusted-for-delegation-to-the-spn-added-to-the-sharepoint-application-pool-account"></a>Sicherstellen, dass dem Connector bei Delegierungen des dem SharePoint-Anwendungspoolkonto hinzugefügten SPN vertraut wird

Konfigurieren Sie KCD so, dass der Azure AD-Anwendungsproxydienst Benutzeridentitäten an das SharePoint-Anwendungspoolkonto delegieren kann. Sie konfigurieren KCD, indem Sie den Anwendungsproxyconnector so einrichten, dass er Kerberos-Tickets für Benutzer abruft, die in Azure AD authentifiziert wurden. Anschließend übergibt dieser Server den Kontext an die Zielanwendung bzw. in diesem Fall an SharePoint.

Wiederholen Sie zum Konfigurieren von KCD die folgenden Schritte für jeden Connectorcomputer:

1. Melden Sie sich bei einem Domänencontroller als Domänenadministrator an, und öffnen Sie dann **Active Directory-Benutzer und -Computer**.
1. Suchen Sie nach dem Computer, auf dem der Azure AD-Proxyconnector ausgeführt wird. In diesem Beispiel ist das der SharePoint-Server.
1. Doppelklicken Sie auf den Computer, und klicken Sie anschließend auf die Registerkarte **Delegierung**.
1. Stellen Sie sicher, dass in den Delegierungseinstellungen **Computer bei Delegierungen angegebener Dienste vertrauen** festgelegt ist. Wählen Sie dann die Option **Beliebiges Authentifizierungsprotokoll verwenden** aus.
1. Klicken Sie auf die Schaltfläche **Hinzufügen**, klicken Sie auf **Benutzer oder Computer**, und suchen Sie nach dem SharePoint-Anwendungspoolkonto, z. B. `Contoso\spapppool`.
1. Wählen Sie in der Liste der SPNs denjenigen aus, den Sie zuvor für das Dienstkonto erstellt haben.
1. Klicken Sie auf **OK**. Klicken Sie erneut auf **OK**, um die Änderung zu speichern.
  
   ![Delegierungseinstellungen](./media/application-proxy-integrate-with-sharepoint-server/delegation-box2.png)

Sie können sich jetzt mit der externen URL bei SharePoint anmelden und bei Azure authentifizieren.

## <a name="troubleshoot-sign-in-errors"></a>Beheben von Anmeldefehlern

Wenn die Anmeldung bei der Website nicht funktioniert, können Sie weitere Informationen zum Problem in den Connectorprotokollen finden: Öffnen Sie die Ereignisanzeige auf dem Computer, auf dem der Connector ausgeführt wird, navigieren Sie zu **Anwendungs- und Dienstprotokolle** > **Microsoft** > **AadApplicationProxy** > **Connector**, und überprüfen Sie das Protokoll **Admin**.

## <a name="next-steps"></a>Nächste Schritte

* [Arbeiten mit benutzerdefinierten Domänen im Azure AD-Anwendungsproxy](application-proxy-configure-custom-domain.md)
* [Grundlegendes zu Azure AD-Anwendungsproxyconnectors](application-proxy-connectors.md)
