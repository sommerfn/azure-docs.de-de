---
title: Entwickeln einer sicheren Azure AD-Webanwendung | Microsoft-Dokumentation
description: Diese einfache Beispiel-App implementiert bewährte Sicherheitsmethoden, die den Sicherheitsstatus Ihrer Anwendung und Ihres Unternehmens bei der Entwicklung in Azure verbessern.
keywords: na
services: security
documentationcenter: na
author: fehase
manager: alclabo
editor: ''
ms.assetid: cd906856-f4f9-4ddc-9249-c998386f4085
ms.service: security
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/12/2019
ms.author: v-fehase
ms.openlocfilehash: 88ef0874d760fb87700eac83c0d615be5887ddee
ms.sourcegitcommit: 0b1a4101d575e28af0f0d161852b57d82c9b2a7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73159840"
---
# <a name="develop-secure-app-for-an-azure-ad-app"></a>Entwickeln einer sicheren App für Azure AD-Apps
## <a name="overview"></a>Übersicht

Bei diesem Beispiel handelt es sich um eine einfache Azure Active Directory-Web-App mit Verknüpfungen zu Sicherheitsressourcen für die Entwicklung von Apps in Azure. Diese App implementiert bewährte Sicherheitsmethoden, die den Sicherheitsstatus Ihrer Anwendung und Ihres Unternehmens bei der Entwicklung in Azure verbessern können.

Die Bereitstellungsskripts richten die Infrastruktur ein. Nachdem Sie die Bereitstellungsskripts ausgeführt haben, müssen Sie einige manuelle Konfigurationen im Azure-Portal durchführen, um die Komponenten und Dienste zu verknüpfen. Dieses Beispiel richtet sich an erfahrene Azure-Entwickler, die in der Einzelhandelsbranche tätig sind und ein geschütztes Azure Active Directory mit sicherer Azure-Infrastruktur erstellen möchten. 


Bei der Entwicklung und Bereitstellung dieser App lernen Sie Folgendes: 
- Erstellen einer Azure Key Vault-Instanz und Speichern und Abrufen von Geheimnissen
- Bereitstellen der Azure-Web-App (dedizierte, isolierte App mit Front-End-Firewallzugriff) 
- Erstellen und Konfigurieren einer Azure Application Gateway-Instanz mit einer Firewall, die das Top 10 OWASP-Regelwerk anwendet 
- Aktivieren der Verschlüsselung von Daten während der Übertragung und im Ruhezustand mithilfe von Azure-Diensten 
- Einrichten der Azure-Richtlinie und des Azure Security Center zur Evaluierung der Compliance 

Sobald Sie diese App entwickelt und bereitgestellt haben, haben Sie die folgende Beispiel-Web-App sowie die beschriebene Konfiguration und Sicherheitsmaßnahmen eingerichtet.

## <a name="architecture"></a>Architecture
Bei der App handelt es sich um eine n-schichtige Anwendung mit drei Schichten. Die Front-End-, Back-End- und Datenbankebenen mit integrierten Überwachungs- und Geheimnisverwaltungskomponenten werden in der folgenden Abbildung veranschaulicht:

![App-Architektur](./media/secure-aad-app/architecture.png)

Diese Lösung verwendet die folgenden Azure-Dienste. Details zur Bereitstellungsarchitektur finden Sie im Abschnitt „Bereitstellungsarchitektur“. 

Die Architektur umfasst die folgenden Komponenten

- [Azure Application Gateway:](../../application-gateway/index.yml) Stellt das Gateway und die Firewall für die Anwendungsarchitektur zur Verfügung.
- [Application Insights:](../../azure-monitor/app/app-insights-overview.md) Stellt einen erweiterbaren APM-Dienst (Application Performance Management) auf mehreren Plattformen bereit.
- [Azure Key Vault](../../key-vault/index.yml). Speichert und verschlüsselt die Geheimnisse der App und verwaltet die Erstellung von Zugriffsrichtlinien.
- [Azure Active Directory](../../active-directory/fundamentals/active-directory-whatis.md). Stellt einen cloudbasierten Identitäts- und Zugriffsverwaltungsdienst sowie Anmeldefunktionen und Zugriffsressourcen bereit.
- [Azure Domain Name System](../../dns/dns-overview.md). Stellt den Dienst zum Hosten der Domäne bereit.
- [Azure Load Balancer:](../../load-balancer/load-balancer-overview.md) Sorgt für die Skalierung Ihrer Anwendungen und bietet Hochverfügbarkeit für Ihre Dienste.
- [Azure-Web-App](../../app-service/overview.md).  Stellt einen HTTP-basierten Dienst zum Hosten von Webanwendungen bereit.
- [Azure Security Center](../../security-center/index.yml). Bietet erweiterten übergreifenden Bedrohungsschutz für die Hybridworkloads in der Cloud.
- [Azure Policy](../../governance/policy/overview.md). Ermöglicht die Überprüfung Ihrer Ressourcen auf Verstöße gegen zugewiesene Richtlinien.

## <a name="threat-model"></a>Bedrohungsmodell
Die Bedrohungsmodellierung ist der Prozess, bei dem potenzielle Sicherheitsbedrohungen für Ihr Unternehmen und Ihre Anwendung identifiziert werden und dann sichergestellt wird, dass geeignete vorbeugende Maßnahmen wirksam sind.

In diesem Beispiel wird das [Microsoft Threat Modeling Tool](https://docs.microsoft.com/azure/security/azure-security-threat-modeling-tool) zum Implementieren der Bedrohungsmodellierung für die sichere Beispiel-App verwendet. Mithilfe von Diagrammen der Komponenten und Datenflüsse können Sie Probleme und Bedrohungen frühzeitig im Entwicklungsprozess identifizieren. Dies sorgt für zukünftige Einsparungen an Zeit und Kosten.

Das Bedrohungsmodell für die App sieht wie folgt aus.

![Bedrohungsmodell](./media/secure-aad-app/threat-model.png)

Im folgenden Screenshot werden einige Beispielbedrohungen und potenzielle Sicherheitsrisiken veranschaulicht, die das Threat Modeling Tool generiert. Das Bedrohungsmodell verschafft Ihnen einen Überblick über die freigestellte Angriffsfläche und fordert die Entwickler auf, sich Gedanken über die Problembehebung zu machen.

![Ausgabe des Bedrohungsmodells](./media/secure-aad-app/threat-model-output.png)

### <a name="prerequisites"></a>Voraussetzungen
Sie müssen die folgenden Tools installieren, um die Anwendung ausführen zu können:

- Ein Code-Editor zum Ändern und Anzeigen des Anwendungscodes. [Visual Studio Code](https://code.visualstudio.com/) bietet sich hier als Open-Source-Option an.
- Die [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest&viewFallbackFrom=azure-cli-latest,) auf Ihrem Entwicklungscomputer.
- [Git](https://git-scm.com/) muss auf Ihrem System installiert sein. Git wird verwendet, um den Quellcode lokal zu klonen.
- Sie benötigen auch [jq](https://stedolan.github.io/jq/). Dies ist ein benutzerfreundliches UNIX-Tool für das Abfragen von JSON.

Außerdem benötigen Sie ein Azure-Abonnement für die Bereitstellung der Ressourcen der Beispiel-App. Wenn Sie über kein Azure-Abonnement verfügen, können Sie ein [kostenloses Konto erstellen](https://azure.microsoft.com/free/), um die Beispiel-App zu testen.

Sobald Sie diese Tools installiert haben, sind Sie bereit, die App in Azure bereitzustellen.

### <a name="implementation-guidance"></a>Implementierungsleitfaden
Das Bereitstellungsskript kann in vier Phasen unterteilt werden. In jeder Phase wird eine Azure-Ressource bereitgestellt und konfiguriert, die im [Architekturdiagramm](#architecture) veranschaulicht wird.

Die vier Phasen lauten wie folgt.

- Bereitstellen von Azure Key Vault
- Bereitstellen von Azure-Web-Apps
- Bereitstellen von Application Gateway mit Web Application Firewall
- Konfigurieren von Azure AD mit der bereitgestellten App

Jede Phase baut auf der vorherigen auf, indem die Konfiguration der zuvor bereitgestellten Ressourcen verwendet wird.

Stellen Sie sicher, dass Sie die unter [Voraussetzungen](#prerequisites) aufgeführten Tools installiert haben, um diese Implementierungsschritte durchführen zu können.

#### <a name="deploy-azure-key-vault"></a>Bereitstellen von Azure Key Vault
In diesem Abschnitt erstellen Sie eine Azure Key Vault-Instanz, die zum Speichen von Geheimnissen und Zertifikaten verwendet wird, und stellen diese bereit.

Sobald die Bereitstellung abgeschlossen ist, verfügen Sie über eine in Azure bereitgestellte Azure Key Vault-Instanz.

So stellen Sie Azure Key Vault mithilfe von PowerShell bereit
 
1. Deklarieren Sie die Variablen für Azure Key Vault.
2. Registrieren Sie den Azure Key Vault-Anbieter.
3. Erstellen Sie die Ressourcengruppe für die Instanz.
4. Erstellen Sie die Azure Key Vault-Instanz in der Ressourcengruppe, die Sie in Schritt 3 erstellt haben.

#### <a name="the-below-azure-ad-user-will-have-admin-permissions-to-the-key-vault"></a>Der unten dargestellte Azure AD-Benutzer verfügt über Administratorberechtigungen für den Key Vault.
    $keyVaultAdminUsers = @($user1,user2)

#### <a name="register-the-az-providers"></a>Registrieren der Azure Key Vault-Anbieter
    Register-AzResourceProvider -ProviderNamespace Microsoft.KeyVault

#### <a name="create-the-azure-key-vault-instance"></a>Erstellen der Azure Key Vault-Instanz
    New-AzKeyVault -Name $kvName 
                -ResourceGroupName $ResourceGroup 
                -Location 'East US'
                -EnabledForDiskEncryption

#### <a name="add-the-administrator-policies-to-the-key-vault"></a>Hinzufügen der Administratorrichtlinien zum Key Vault
    foreach ($keyVaultAdminUser in $keyVaultAdminUsers) {
    $UserObjectId = (Get-AzADUser -SearchString $keyVaultAdminUser).Id
    Set-AzKeyVaultAccessPolicy -VaultName $keyVaultName -ResourceGroupName $resourceGroupName -ObjectId $UserObjectId 
    -PermissionsToKeys all -PermissionsToSecrets all -PermissionsToCertificates all
    }

#### <a name="to-create-an-access-policy-to-allow-a-user-to-get-and-list-cryptographic-keys-certificates-and-secrets-if-you-know-the-user-principal-name"></a>So erstellen Sie eine Zugriffsrichtlinie, damit ein Benutzer kryptografische Schlüssel, Zertifikate und Geheimnisse abrufen und auflisten kann, wenn Sie den Benutzerprinzipalnamen kennen:
    Set-AzKeyVaultAccessPolicy -VaultName $keyVaultName 
                           -ResourceGroupName $resourceGroupName 
                           -UserPrincipalName 'user1@contoso.com 
                           -PermissionsToCertificates list, get 
                           -PermissionsToKeys list, get 
                           -PermissionsToSecrets list, get 

Die Verwendung verwalteter Identitäten für Azure-Ressourcen in Apps, die Azure Key Vault für den Zugriff auf Ressourcen verwenden, ist eine bewährte Methode. Ihr Sicherheitsstatus verbessert sich, wenn Zugriffsschlüssel für Azure Key Vault weder im Code noch in der Konfiguration gespeichert werden.

Im Container ist ein Stammzertifikat enthalten. Die Schritte zum Abrufen des Zertifikats lauten wie folgt:

1. Laden Sie die Zertifikatdatei von der [Zertifizierungsstelle](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt) herunter.
2. Decodieren Sie die Zertifikatdatei:

   ```powershell
   openssl x509 -inform DER -in BaltimoreCyberTrustRoot.crt -text -out root.crt
   ```
Dieses Skript erstellt eine zugewiesene Identität für die App Service-Instanz, die mit MSI für die Interaktion mit Azure Key Vault verwendet werden kann, ohne eine Hartcodierung für Geheimnisse im Code oder in der Konfiguration vorzunehmen.

Navigieren Sie im Portal zur Azure Key Vault-Instanz, um die zugewiesene Identität auf der Registerkarte „Zugriffsrichtlinie“ zu autorisieren. Klicken Sie auf **Neue Zugriffsrichtlinie hinzufügen**. Suchen Sie unter **Prinzipal auswählen** nach dem Anwendungsnamen, der dem Namen der erstellten App Service-Instanz ähnelt.
Sie sollten einen Dienstprinzipal sehen können, der der Anwendung angefügt ist. Wählen Sie ihn aus, und speichern Sie die Zugriffsrichtlinienseite wie im folgenden Screenshot gezeigt.

Da die Anwendung nur Schlüssel abrufen muss, wählen Sie die **Get**-Berechtigung in den Geheimnisoptionen aus, um den Zugriff zu gewähren und die gewährten Berechtigungen zu verringern.

![Schlüsseltresor-Zugriffsrichtlinie](./media/secure-aad-app/kv-access-policy.png)

*Erstellen einer Schlüsseltresor-Zugriffsrichtlinie*

Speichern Sie die Zugriffsrichtlinie, und speichern Sie dann die neue Änderung auf der Registerkarte **Zugriffsrichtlinien**, um die Richtlinien zu aktualisieren.

#### <a name="deploy-application-gateway-with-web-application-firewall-enabled"></a>Bereitstellen von Application Gateway mit aktivierter Web Application Firewall
Es wird davon abgeraten, Dienste in Web-Apps direkt für Außenstehende im Internet zur Verfügung zu stellten.
Lastenausgleiche und Firewallregeln stellen mehr Sicherheit und Kontrolle über eingehenden Datenverkehr bereit und helfen Ihnen bei der Verwaltung.

So stellen Sie eine Application Gateway-Instanz bereit

1. Erstellen Sie die Ressourcengruppe, die die Application Gateway-Instanz enthalten soll.
2. Stellen Sie ei virtuelles Netzwerk bereit, das an das Gateway angefügt wird.
3. Erstellen Sie ein Subnetz für das Gateway im virtuellen Netzwerk.
4. Stellen Sie eine öffentliche IP-Adresse bereit.
5. Stellen Sie das Anwendungsgateway bereit.
6. Aktivieren Sie die Web Application Firewall für das Gateway.

```
Connect-AzAccount
Select-AzSubscription -SubscriptionId '$SubscriptionId'
New-AzResourceGroup -Name appgw-rg -Location "East US"

#Create a virtual network and a subnet for the application gateway

#Assign an address range for the subnet to be used for the application gateway.

$gwSubnet = New-AzVirtualNetworkSubnetConfig -Name 'appgwsubnet' -AddressPrefix 10.0.0.0/24

#Assign an address range to be used for the back-end address pool.

$nicSubnet = New-AzVirtualNetworkSubnetConfig  -Name 'appsubnet' -AddressPrefix 10.0.0.0/24

#Create a virtual network with the subnets defined in the preceding steps.

$vnet = New-AzvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg -Location "East US" -AddressPrefix 10.0.0.0/16 -Subnet $gwSubnet, $nicSubnet

#Retrieve the virtual network resource and subnet resources to be used in the steps that follow.

$vnet = Get-AzvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg
$gwSubnet = Get-AzVirtualNetworkSubnetConfig -Name 'appgwsubnet' -VirtualNetwork $vnet
$nicSubnet = Get-AzVirtualNetworkSubnetConfig -Name 'appsubnet' -VirtualNetwork $vnet


#Create a public IP address for the front-end configuration

$publicip = New-AzPublicIpAddress -ResourceGroupName appgw-rg -Name 'publicIP01' -Location "East US" -AllocationMethod Dynamic

#Create an application gateway configuration object

$gipconfig = New-AzApplicationGatewayIPConfiguration -Name 'gwconfig' -Subnet $gwSubnet

#Create a front-end IP configuration

$fipconfig = New-AzApplicationGatewayFrontendIPConfig -Name 'fip01' -PublicIPAddress $publicip

#Configure the back-end IP address pool with the IP addresses of the back-end web servers

$pool = New-AzApplicationGatewayBackendAddressPool -Name 'pool01' -BackendIPAddresses 10.0.0.0

#Configure the front-end IP port for the public IP endpoint

$fp = New-AzApplicationGatewayFrontendPort -Name 'port01'  -Port 443

#Configure the certificate for the application gateway. This certificate is used to decrypt and reencrypt the traffic on the application gateway

$passwd = ConvertTo-SecureString  "P@ssword!1" -AsPlainText -Force 
$cert = New-AzApplicationGatewaySSLCertificate -Name cert01 -CertificateFile "C:\AAD\Securities\Certificates\sslcert.com.cer" -Password $passwd 

#Create the HTTP listener for the application gateway

$listener = New-AzApplicationGatewayHttpListener -Name listener01 -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SSLCertificate $cert

#Upload the certificate to be used on the SSL-enabled back-end pool resources

#$authcert = New-AzApplicationGatewayAuthenticationCertificate -Name 'allowlistcert1' -CertificateFile C:\cert.cer

$trustedRootCert01 = New-AzApplicationGatewayTrustedRootCertificate -Name "test1" -CertificateFile "C:\AAD\Securities\Certificates\sslcert.com.cer"

#Configure the HTTP settings for the application gateway back end

$poolSetting01 = New-AzApplicationGatewayBackendHttpSettings -Name "setting01" -Port 443 -Protocol Https -CookieBasedAffinity Disabled -TrustedRootCertificate $trustedRootCert01 -HostName "test1"

#Create a load-balancer routing rule that configures the load balancer

$rule = New-AzApplicationGatewayRequestRoutingRule -Name 'rule01' -RuleType basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

#Configure the instance size of the application gateway

$sku = New-AzApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

#Configure the SSL policy to be used on the application gateway

$SSLPolicy = New-AzApplicationGatewaySSLPolicy -MinProtocolVersion TLSv1_2 -CipherSuite "TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256", "TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384", "TLS_RSA_WITH_AES_128_GCM_SHA256" -PolicyType Custom

$appgw = New-AzApplicationGateway -Name appgateway -SSLCertificates $cert -ResourceGroupName "appgw-rg" -Location "East US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting01 -FrontendIpConfigurations $fipconfig -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -SSLPolicy $SSLPolicy -TrustedRootCertificate $trustedRootCert01 -Verbose

```

#### <a name="deploy-azure-web-apps"></a>Bereitstellen von Azure-Web-Apps
Der Azure App Service ermöglicht Ihnen das Erstellen von Web-Apps mithilfe der Sprachen Python C#, Ruby und Java. Azure unterstützt außerdem benutzerdefinierte Container, die praktisch alle Programmiersprachen auf der Azure App Service-Plattform zulassen.

#### <a name="create-an-app-service-plan-in-free-tier"></a>Erstellen eines App Service-Plans im Free-Tarif
    New-AzAppServicePlan -Name $webappname -Location $location -ResourceGroupName $webappname -Tier Free

#### <a name="create-a-web-app"></a>Erstellen einer Web-App
    New-AzWebApp -Name $webappname -Location $location -AppServicePlan $webappname -ResourceGroupName $webappname

    Write-Host "Configure a CNAME record that maps $fqdn to $webappname.azurewebsites.net"
    Read-Host "Press [Enter] key when ready ..."

#### <a name="before-continuing-go-to-your-azure-domain-name-system-configuration-ui-for-your-custom-domain-and-follow-the-instructions-at-httpsakamsappservicecustomdns-to-configure-a-cname-record-for-the-hostname-www-and-point-it-your-web-apps-default-domain-name"></a>Bevor Sie fortfahren, wechseln Sie zur Azure Domain Name System-Konfigurationsbenutzeroberfläche für Ihre benutzerdefinierte Domäne, und befolgen Sie die Anweisungen unter https://aka.ms/appservicecustomdns , um einen CNAME-Eintrag für den Hostnamen „www“ zu konfigurieren. Verweisen Sie von ihm auf den Standarddomänennamen Ihrer Web-App.

#### <a name="upgrade-app-service-plan-to-shared-tier-minimum-required-by-custom-domains"></a>Führen Sie ein Upgrade des App Service-Plan auf den Tarif „Shared“ (Mindestanforderung für benutzerdefinierte Domänen) durch.
    Set-AzAppServicePlan -Name $webappname -ResourceGroupName $webappname -Tier Shared

#### <a name="add-a-custom-domain-name-to-the-web-app"></a>Fügen Sie der Web-App einen benutzerdefinierten Domänennamen hinzu.
    Set-AzWebApp -Name $webappname -ResourceGroupName $webappname `-HostNames @($fqdn,"$webappname.azurewebsites.net")

## <a name="guidance-and-recommendations"></a>Anleitungen und Empfehlungen

### <a name="network"></a>Netzwerk
Nach Abschluss der Bereitstellung verfügen Sie über ein Anwendungsgateway mit aktivierter Web Application Firewall.

Die Application Gateway-Instanz stellt den Port 443 für HTTPS zur Verfügung. Mit dieser Konfiguration wird sichergestellt, dass die App nur über Port 443 per HTTPS zugänglich ist.

Das Sperren nicht verwendeter Ports und Einschränken der Angriffsfläche stellt eine bewährte Sicherheitsmaßnahme dar.

#### <a name="add-network-security-groups-to-the-app-service-instance"></a>Hinzufügen von Netzwerksicherheitsgruppen zur App Service-Instanz

App Service-Instanzen können in virtuelle Netzwerke integriert werden. Diese Integration ermöglicht die Konfiguration mit Richtlinien für Netzwerksicherheitsgruppen, die den eingehenden und ausgehenden Datenverkehr der App verwalten.

1. Wählen Sie auf dem Blatt der Azure App Service-Instanz unter **Einstellungen** die Option **Netzwerk** aus, um dieses Feature zu aktivieren. Führen Sie im rechten Bereich die Konfiguration unter **VNET-Integration** durch.

   ![Neue VNET-Integration](./media/secure-web-app/app-vnet-menu.png)

    *Neue VNET-Integration für App Service*
1. Klicken Sie auf der nächsten Seite auf **VNET hinzufügen (Vorschau)** .

1. Wählen Sie im nächsten Menü das in der Bereitstellung erstellte virtuelle Netzwerk aus, das mit `aad-vnet` beginnt. Sie können entweder ein neues Subnetz erstellen oder ein vorhandenes auswählen.
   Erstellen Sie in diesem Fall ein neues Subnetz. Legen Sie den **Adressbereich** auf **10.0.3.0/24** fest, und nennen Sie das Subnetz **app-subnet**.

   ![Konfiguration des virtuellen App Service-Netzwerks](./media/secure-web-app/app-vnet-config.png)

    *Konfiguration des virtuellen Netzwerks für App Service*

Da Sie die VNET-Integration nun aktiviert haben, können Sie die Netzwerksicherheitsgruppen zur App hinzufügen.

1. Suchen Sie mithilfe des Suchfelds nach **Netzwerksicherheitsgruppen**. Wählen Sie **Netzwerksicherheitsgruppen** aus den Suchergebnissen aus.

    ![Netzwerksicherheitsgruppen suchen](./media/secure-web-app/nsg-search-menu.png)

    *Suche nach Netzwerksicherheitsgruppen*

2. Klicken Sie im nächsten Menü auf **Hinzufügen**. Geben Sie die **Namen** der Netzwerksicherheitsgruppe und der **Ressourcengruppe** ein, in der sie enthalten ist. Diese Netzwerksicherheitsgruppe wird auf das Subnetz des Anwendungsgateways angewendet.

    ![Erstellen einer NSG](./media/secure-web-app/nsg-create-new.png)

    *Erstellen einer Netzwerksicherheitsgruppe*

3. Nachdem die Netzwerksicherheitsgruppe erstellt wurde, wählen Sie sie aus. Klicken Sie auf dem Blatt unter **Einstellungen** auf **Eingangssicherheitsregeln**. Konfigurieren Sie diese Einstellungen, um eingehende Verbindungen mit dem Anwendungsgateway über Port 443 zuzulassen.

   ![Konfigurieren der Netzwerksicherheitsgruppe](./media/secure-web-app/nsg-gateway-config.png)

   *Konfigurieren der Netzwerksicherheitsgruppe*

4. Fügen Sie in den Ausgangsregeln für die Netzwerksicherheitsgruppe des Gateways eine Regel hinzu, die ausgehende Verbindungen mit der App Service-Instanz zulässt, indem Sie eine Regel für das Diensttag `AppService` erstellen.

   ![Hinzufügen von Ausgangsregeln für die Netzwerksicherheitsgruppe](./media/secure-web-app/nsg-outbound-allowappserviceout.png)

   *Hinzufügen von Ausgangsregeln für die Netzwerksicherheitsgruppe*

    Fügen Sie eine weitere Ausgangsregel hinzu, um dem Gateway das Senden von Ausgangsregeln an ein virtuelles Netzwerk zu erlauben.

   ![Hinzufügen einer weiteren Ausgangsregel](./media/secure-web-app/nsg-outbound-vnet.png)

    *Hinzufügen einer weiteren Ausgangsregel*

5. Klicken Sie auf dem Subnetzblatt der Netzwerksicherheitsgruppe auf **Zuordnen**, und wählen Sie das in der Bereitstellung erstellte virtuelle Netzwerk und das Gatewaysubnetz namens **gw-subnet** aus. Die Netzwerksicherheitsgruppe wird auf das Subnetz angewendet.

6. Erstellen Sie eine weitere Netzwerksicherheitsgruppe wie in einem der vorherigen Schritte, dieses Mal für die App Service-Instanz. Geben Sie ihr einen Namen. Fügen Sie die Eingangsregel für Port 443 hinzu, wie Sie es bereits für die Netzwerksicherheitsgruppe für das Anwendungsgateway getan haben.

   Wenn Sie eine App Service-Instanz in einer App Service-Umgebung bereitgestellt haben, was bei dieser App nicht der Fall ist, können Sie Eingangsregeln hinzufügen, um Azure Service Health-Tests zuzulassen, indem Sie die Ports 454-455 in den Eingangssicherheitsgruppen Ihrer App Service-Netzwerksicherheitsgruppe öffnen. Die Konfiguration sieht wie folgt aus:

   ![Hinzufügen von Regeln für Azure Service Health-Tests](./media/secure-web-app/nsg-create-healthprobes.png)

    *Hinzufügen von Regeln für Azure Service Health-Tests (Nur für App Service-Umgebungen)*

Ändern Sie die App Service-Netzwerkeinstellungen so, dass nur das Anwendungsgateway auf die Anwendung zugreifen kann, um die Angriffsfläche einzuschränken.
Öffnen Sie zum Anwenden der Einstellungen die App Service-Netzwerkregisterkarte, wählen Sie die Registerkarte **IP-Einschränkungen** aus, und erstellen Sie eine Zulassungsregel, die nur der IP-Adresse des Anwendungsgateways den Direktzugriff auf den Dienst erlaubt. Sie können die IP-Adresse des Gateways auf der Übersichtsseite abrufen. Geben Sie die IP-Adresse auf der Registerkarte **IP Address CIDR (IP-Adressen-CIDR)** im folgenden Format ein: `<GATEWAY_IP_ADDRESS>/32`.

![Nur Zugriff des Gateways zulassen](./media/secure-web-app/app-allow-gw-only.png)

*Zulassen des Zugriffs auf App Service nur für die IP-Adresse des Gateways*

### <a name="azure-domain-name-system"></a>Azure Domain Name System 
Das Azure Domain Name System ist für die Übersetzung (oder Auflösung) eines Website- oder Dienstnamens in die IP-Adresse verantwortlich. Azure Domain Name System(https://docs.microsoft.com/azure/dns/dns-overview) ist ein Hostingdienst für Domain Name System-Domänen, der die Namensauflösung mithilfe der Azure-Infrastruktur durchführt. Durch das Hosten von Domänen in Azure können Benutzer Domain Name System-Einträge mithilfe der gleichen Anmeldeinformationen, APIs, Tools und Abrechnungsschritte wie für die anderen Azure-Dienste verwalten. Azure Domain Name System unterstützt auch private Domain Name System-Domänen.

### <a name="azure-disk-encryption"></a>Azure Disk Encryption
Azure Disk Encryption nutzt das BitLocker-Feature von Windows, um eine Volumeverschlüsselung für Datenträger bereitzustellen. Die Lösung ist in Azure Key Vault integriert, um die Steuerung und Verwaltung der Datenträgerverschlüsselungsschlüssel zu erleichtern.

### <a name="identity-management"></a>Identitätsverwaltung
Die folgenden Technologien enthalten Funktionen zum Verwalten des Zugriffs auf Karteninhaberdaten in der Azure-Umgebung.
- Azure Active Directory ist der mehrinstanzenfähige cloudbasierte Verzeichnis- und Identitätsverwaltungsdienst von Microsoft. Alle Benutzer für diese Lösung werden in Azure Active Directory erstellt, so auch die Benutzer, die auf die Azure-Web-App zugreifen.
- Die rollenbasierte Zugriffssteuerung in Azure ermöglicht Administratoren das Definieren präziser Zugriffsberechtigungen, um jeweils nur den Zugriff zu gewähren, den Benutzer für ihre Aufgaben benötigen. Anstatt jedem Benutzer uneingeschränkte Benutzerberechtigungen für Azure-Ressourcen zu gewähren, können Administratoren auch nur bestimmte Aktionen für den Zugriff auf Karteninhaberdaten erlauben. Der Abonnementzugriff ist auf den Abonnementadministrator beschränkt.
- Mit Azure Active Directory Privileged Identity Management können Kunden die Anzahl von Benutzern mit Zugriff auf bestimmte Informationen (etwa Karteninhaberdaten) einschränken. Administratoren können mit Azure Active Directory Privileged Identity Management privilegierte Identitäten und den damit verbundenen Zugriff auf Ressourcen erkennen, einschränken und überwachen. Diese Funktionalität kann auch verwendet werden, um einen bedarfsgesteuerten Just-in-Time-Administratorzugriff zu erzwingen.
- Azure Active Directory Identity Protection erkennt potenzielle Sicherheitsrisiken, die sich auf die Identitäten einer Organisation auswirken, konfiguriert automatisierte Reaktionen auf erkannte verdächtige Aktionen, die im Zusammenhang mit Identitäten der Organisation stehen, untersucht verdächtige Vorfälle und führt die entsprechenden Aktionen zu deren Behebung aus.
### <a name="secrets-management"></a>Verwaltung geheimer Schlüssel
Die Lösung verwendet Azure Key Vault für die Verwaltung von Schlüsseln und Geheimnissen. Azure Key Vault unterstützt Sie dabei, kryptografische Schlüssel und Geheimnisse zu schützen, die von Cloudanwendungen und -diensten verwendet werden. Die folgenden Funktionen von Azure Key Vault unterstützen Kunden beim Schützen von und Zugreifen auf diese Daten.
   - Erweiterte Zugriffsrichtlinien werden auf Bedarfsbasis konfiguriert.
   - Key Vault-Zugriffsrichtlinien werden mit minimalen erforderlichen Berechtigungen für Schlüssel und Geheimnisse definiert.
   - Alle Schlüssel und Geheimnisse in Key Vault besitzen ein Ablaufdatum.
   - Alle Schlüssel in Key Vault werden durch spezielle Hardwaresicherheitsmodule geschützt. Der Schlüsseltyp ist ein Hardwaresicherheitsmodul (HSM), das mit einem 2048-Bit-RSA-Schlüssel geschützt wird.
   - Mit Key Vault können Sie Schlüssel und Geheimnisse (beispielsweise Authentifizierungsschlüssel, Schlüssel für Speicherkonten, Datenverschlüsselungsschlüssel, PFX-Dateien und Kennwörter) verschlüsseln, indem Sie Schlüssel verwenden, die durch Hardwaresicherheitsmodule (HSMs) geschützt werden. 
   - Verwenden Sie die rollenbasierte Zugriffssteuerung (RBAC), um Benutzern, Gruppen und Anwendungen Berechtigungen für einen bestimmten Bereich zu erteilen.     
   - Verwenden Sie Key Vault, um Ihre TLS-Zertifikate mit automatischer Verlängerung zu verwalten. 
   - Diagnoseprotokolle für Key Vault werden mit einer Aufbewahrungsdauer von mindestens 365 Tagen aktiviert.
   - Zulässige kryptographische Vorgänge für Schlüssel sind auf die erforderlichen Vorgänge beschränkt.
### <a name="azure-security-center"></a>Azure Security Center
Mit Azure Security Center können Kunden zentral workloadübergreifende Sicherheitsrichtlinien anwenden und verwalten, die Angriffsfläche verringern sowie Angriffe erkennen und darauf reagieren. Außerdem greift 
   - Azure Security Center auf die vorhandenen Konfigurationen der Azure-Dienste zu, um Empfehlungen für die Konfiguration und den Dienst bereitzustellen, die zur Verbesserung des Sicherheitsstatus und zum Schutz von Daten beitragen sollen.
   - Azure Security Center verwendet eine Vielzahl von Erkennungsfunktionen, um Kunden vor potenziellen Angriffen auf ihre Umgebungen zu warnen. Diese Warnungen enthalten wichtige Informationen zum Auslöser der Warnung, zu den möglicherweise betroffenen Ressourcen und zur Quelle des Angriffs. In Azure Security Center steht eine Reihe vordefinierter Sicherheitswarnungen zur Verfügung, die ausgelöst werden, wenn eine Bedrohung oder eine verdächtige Aktivität erkannt wird. Mithilfe benutzerdefinierter Warnungsregeln in Azure Security Center können Kunden neue Sicherheitswarnungen auf der Grundlage von Daten definieren, die bereits in ihrer Umgebung gesammelt werden.
   - Azure Security Center bietet priorisierte Sicherheitswarnungen und Incidents, sodass es für Kunden einfacher wird, potenzielle Sicherheitsprobleme zu erkennen und zu beheben. Für jede erkannte Bedrohung wird ein Bericht mit Bedrohungsinformationen generiert, der die zuständigen Teams bei der Untersuchung und Beseitigung von Bedrohungen unterstützt.
### <a name="azure-application-gateway"></a>Azure Application Gateway 
   Die Architektur verringert die Gefahr von Sicherheitsrisiken durch ein Azure Application Gateway mit einer konfigurierten Web Application Firewall und aktiviertem OWASP-Regelsatz. Weitere Funktionen:
   - End-to-End-SSL
   - Deaktivieren von TLS v1.0 und v1.1
   - Aktivieren von TLS v1.2
   - Web Application Firewall (Schutzmodus)
   - Schutzmodus mit OWASP 3.0-Regelsatz
   - Aktivieren der Diagnoseprotokollierung
   - Benutzerdefinierte Integritätstests
   - Azure Security Center und Azure Advisor bieten zusätzlichen Schutz und zusätzliche Benachrichtigungen. Azure Security Center stellt außerdem ein Reputationssystem bereit.
### <a name="logging-and-auditing"></a>Protokollierung und Überwachung
Azure-Dienste protokollieren umfassend die System- und Benutzeraktivitäten sowie die Systemintegrität:
   - Aktivitätsprotokolle: [Aktivitätsprotokolle](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) bieten Einblicke in Vorgänge, die für Ressourcen in einem Abonnement ausgeführt wurden. Aktivitätsprotokolle können dabei helfen, den Initiator eines Vorgangs, die Zeit des Auftretens und den Status zu bestimmen.
   - Diagnoseprotokolle: [Diagnoseprotokolle](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) umfassen sämtliche Protokolle, die von jeder Ressource ausgegeben werden. Zu diesen Protokollen gehören Windows-Ereignissystemprotokolle, Azure Storage-Protokolle, Key Vault-Überwachungsprotokolle sowie Application Gateway-Zugriffs- und -Firewallprotokolle. Alle Diagnoseprotokolle werden für die Archivierung in ein zentrales und verschlüsseltes Azure Storage-Konto geschrieben. Die Aufbewahrung kann vom Benutzer konfiguriert werden (bis zu 730 Tage), um den unternehmensspezifischen Aufbewahrungsanforderungen gerecht zu werden.
### <a name="azure-monitor-logs"></a>Azure Monitor-Protokolle
   Diese Protokolle werden dann zur Verarbeitung, Speicherung und Dashboardanzeige in [Azure Monitor-Protokolle](https://azure.microsoft.com/services/log-analytics/) konsolidiert. Nachdem die Daten gesammelt wurden, werden sie in separaten Tabellen für die einzelnen Datentypen in Log Analytics-Arbeitsbereichen angeordnet, damit alle Daten unabhängig von der ursprünglichen Quelle zusammen analysiert werden können. Darüber hinaus ist das Azure Security Center in Azure Monitor-Protokolle integriert, sodass Kunden mit Kusto-Abfragen auf ihre Sicherheitsereignisdaten zugreifen und diese mit Daten aus anderen Diensten kombinieren können.

   In dieser Architektur sind die folgenden Azure-[Überwachungslösungen](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions) enthalten.

   - [Active Directory-Bewertung](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): Die Lösung „Active Directory-Integritätsüberprüfung“ bewertet die Risiken und die Integrität von Serverumgebungen in regelmäßigen Abständen und erstellt eine priorisierte Liste mit spezifischen Empfehlungen für die bereitgestellte Serverinfrastruktur.
   - [Agent-Integritätsdiagnose](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): Die Lösung „Agent-Integritätsdiagnose“ meldet die Anzahl bereitgestellter Agents sowie deren geografische Verteilung. Außerdem meldet sie, wie viele Agents nicht mehr reagieren und wie viele Agents Betriebsdaten übermitteln.
   - [Aktivitätsprotokollanalyse](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): Die Lösung „Aktivitätsprotokollanalyse“ hilft bei der Analyse der Azure-Aktivitätsprotokolle in allen Azure-Abonnements eines Kunden.
### <a name="azure-monitor"></a>Azure Monitor
   [Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) unterstützt Benutzer beim Nachverfolgen der Leistung, Aufrechterhalten der Sicherheit und Identifizieren von Trends, indem Organisationen das Überwachen, Erstellen von Warnungen und Archivieren von Daten ermöglicht wird, z. B. die Nachverfolgung von API-Aufrufen in ihren Azure-Ressourcen.
### <a name="application-insights"></a>Application Insights 
   [Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) ist ein erweiterbarer, für Webentwickler konzipierter Dienst zur Verwaltung der Anwendungsleistung auf mehreren Plattformen. Mit Application Insights werden Leistungsanomalien erkannt, und Kunden können die Lösung für die Liveüberwachung der Webanwendung nutzen. Sie verfügt über leistungsstarke Analysetools, mit denen Kunden Probleme diagnostizieren und nachvollziehen können, wie ihre App von den Benutzern verwendet wird. Der Dienst unterstützt Kunden bei der kontinuierlichen Verbesserung der Leistung und Benutzerfreundlichkeit.

### <a name="azure-key-vault"></a>Azure Key Vault
   Erstellen Sie einen Tresor für die Schlüsselspeicherung der Organisation, und gewährleisten Sie die Verantwortlichkeit für z. B. folgende operative Aufgaben.

   - In Key Vault gespeicherte Daten:   
   - Application Insights-Schlüssel
   - Datenspeicher-Zugriffsschlüssel
   - Verbindungszeichenfolge
   - Datentabellenname
   - Benutzeranmeldeinformationen
   - Erweiterte Zugriffsrichtlinien werden auf Bedarfsbasis konfiguriert.
   - Key Vault-Zugriffsrichtlinien werden mit minimalen erforderlichen Berechtigungen für Schlüssel und Geheimnisse definiert.
   - Alle Schlüssel und Geheimnisse in Key Vault besitzen ein Ablaufdatum.
   - Alle Schlüssel in Key Vault werden durch spezielle Hardwaresicherheitsmodule (HSMs) geschützt. [Der Schlüsseltyp ist ein Hardwaresicherheitsmodul, das mit einem       
     2048-Bit-RSA-Schlüssel geschützt wird.]
   - Allen Benutzern/Identitäten werden mithilfe der rollenbasierten Zugriffssteuerung (Role Based Access Control, RBAC) die mindestens erforderlichen Berechtigungen erteilt.
   - Anwendungen teilen sich nur dann einen Key Vault, wenn sie sich gegenseitig vertrauen und zur Laufzeit Zugriff auf dieselben Geheimnisse benötigen.
   - Diagnoseprotokolle für Key Vault werden mit einer Aufbewahrungsdauer von mindestens 365 Tagen aktiviert.
   - Zulässige kryptographische Vorgänge für Schlüssel sind auf die erforderlichen Vorgänge beschränkt.

### <a name="vpn-and-expressroute"></a>VPN und ExpressRoute
   Zur Erzielung von sicheren Verbindungen mit Ressourcen, die als Teil dieser Referenzarchitektur einer PaaS-Webanwendung bereitgestellt werden, muss ein sicherer VPN-Tunnel oder eine [ExpressRoute-Verbindung](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) konfiguriert werden. Durch die korrekte Einrichtung von VPN oder ExpressRoute können Kunden eine zusätzliche Sicherheitsebene für Daten während der Übertragung hinzufügen.

   Mit der Implementierung eines sicheren VPN-Tunnels mit Azure kann eine virtuelle private Verbindung zwischen einem lokalen Netzwerk und einem virtuellen Azure-Netzwerk hergestellt werden. Diese Verbindung wird über das Internet hergestellt und ermöglicht Kunden das Übertragen von Informationen über einen „Tunnel“ innerhalb eines verschlüsselten Links zwischen dem Kundennetzwerk und Azure. Eine Site-to-Site-VPN-Verbindung ist eine sichere, ausgereifte Technologie, die seit Jahrzehnten von Unternehmen aller Größen eingesetzt wird. Bei dieser Option wird der IPsec-Tunnelmodus als Verschlüsselungsmechanismus verwendet.

   Da Datenverkehr im VPN-Tunnel über ein Site-to-Site-VPN über das Internet übertragen wird, bietet Microsoft eine weitere Verbindungsoption an, die noch sicherer ist. Azure ExpressRoute ist ein dedizierter WAN-Link zwischen Azure und einem lokalen Standort oder einem Exchange-Hostinganbieter. Da ExpressRoute-Verbindungen nicht über das Internet verlaufen, bieten sie eine höhere Sicherheit, größere Zuverlässigkeit und schnellere Geschwindigkeit bei geringerer Latenz als herkömmliche Verbindungen über das Internet. Da es sich außerdem um eine direkte Verbindung vom Telekommunikationsanbieter des Kunden handelt, werden die Daten nicht über das Internet übertragen und dort nicht verfügbar gemacht.

   Bewährte Methoden für das Implementieren eines sicheren hybriden Netzwerks, mit dem ein lokales Netzwerk auf Azure erweitert wird, finden Sie [hier](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid).

#### <a name="implement-azure-active-directory-oidc"></a>Implementieren von Azure Active Directory OIDC

1. Verwenden Sie den folgenden Git-Befehl, um das Quellcoderepository zu klonen.

 ``` git
 git clone https://github.com/Azure-Samples/AAD-Security
   ```
## <a name="update-the-redirect-urls"></a>Aktualisieren von Umleitungs-URLs
1.  Navigieren Sie zurück zum Azure-Portal. Wählen Sie im linken Navigationsbereich den Dienst „Azure Active Directory“ und anschließend „App-Registrierungen“ aus.
2.  Wählen Sie im daraufhin angezeigten Bildschirm die Anwendung „WebApp-OpenIDConnect-DotNet-code-v2“ aus.
3.  Auf der Registerkarte „Authentifizierung“ o   Wählen Sie im Abschnitt „Umleitungs-URIs“ im Kombinationsfeld die Option „Web“ aus, und fügen Sie die folgenden Umleitungs-URIs hinzu.
    [https://WebApp-OpenIDConnect-DotNet-code-v2-contoso.azurewebsites.net](https://WebApp-OpenIDConnect-DotNet-code-v2-contoso.azurewebsites.net ) https://WebApp-OpenIDConnect-DotNet-code-v2-contoso.azurewebsites.net/signin-oidc o   Legen Sie im Abschnitt „Erweiterte Einstellungen“ die Option „Abmelde-URL“ auf https://WebApp-OpenIDConnect-DotNet-code-v2-contoso.azurewebsites.net/signout-oidc fest.
4.  Auf der Registerkarte „Branding“ o   Aktualisieren Sie die URL der Startseite auf die Adresse des App Service, z. B. https://WebApp-OpenIDConnect-DotNet-code-v2-contoso.azurewebsites.net.
        o   Speichern Sie die Konfiguration.
5.  Wenn Ihre Anwendung eine Web-API aufruft, stellen Sie sicher, dass Sie die erforderlichen Änderungen an der Datei „appsettings.json“ des Projekts vornehmen, damit anstelle von „localhost“ die veröffentlichte API-URL aufgerufen wird.
Veröffentlichen des Beispiels
    1.  Laden Sie über die Registerkarte „Übersicht“ des App Service das Veröffentlichungsprofil herunter, indem Sie auf den Link „Veröffentlichungsprofil abrufen“ klicken, und speichern Sie es. Es können auch andere Bereitstellungsmechanismen, z. B. über die Quellcodeverwaltung, verwendet werden.
    2.  Wechseln Sie zu Visual Studio, und navigieren Sie zum Projekt „WebApp-OpenIDConnect-DotNet-code-v2“. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, und wählen Sie „Veröffentlichen“ aus. Klicken Sie in der unteren Leiste auf „Profil importieren“, und importieren Sie das Veröffentlichungsprofil, das Sie zuvor heruntergeladen haben.
    3.  Klicken Sie auf „Konfigurieren“, und aktualisieren Sie auf der Registerkarte „Verbindung“ die Ziel-URL, sodass die URL der Startseite „https“ enthält, z. B. https://WebApp-OpenIDConnect-DotNet-code-v2-contoso.azurewebsites.net. Klicken Sie auf Weiter.
    4.  Stellen Sie sicher, dass auf der Registerkarte „Einstellungen“ die Option „Organisationsauthentifizierung aktivieren“ NICHT ausgewählt ist. Klicken Sie auf Speichern. Klicken Sie im Hauptbildschirm auf „Veröffentlichen“.
    5.  Visual Studio veröffentlicht das Projekt und öffnet automatisch einen Browser mit der URL des Projekts. Wenn die Standardwebseite des Projekts angezeigt wird, war die Veröffentlichung erfolgreich.
#### <a name="implement-multi-factor-authentication-for-azure-active-directory"></a>Implementieren der mehrstufigen Authentifizierung für Azure Active Directory
   Administratoren müssen sicherstellen, dass die Abonnementkonten im Portal geschützt sind. Das Abonnement ist für Angriffe anfällig, da es die von Ihnen erstellten Ressourcen verwaltet. Aktivieren Sie die mehrstufige Authentifizierung über die Registerkarte **Azure Active Directory** des Abonnements, um das Abonnement zu schützen.

   Azure AD funktioniert auf Basis von Richtlinien, die auf Benutzer oder Benutzergruppen angewendet werden, die bestimmte Kriterien erfüllen.
Azure erstellt eine Standardrichtlinie, die angibt, dass Administratoren die zweistufige Authentifizierung benötigen, um sich beim Portal anzumelden.
Sobald diese Richtlinie aktiviert ist, werden Sie möglicherweise dazu aufgefordert, sich beim Azure-Portal abzumelden und sich wieder anzumelden.

So aktivieren Sie die mehrstufige Authentifizierung für Administratoranmeldungen

   1. Öffnen Sie im Azure-Portal die Registerkarte **Azure Active Directory**.
   2. Wählen Sie in der Kategorie „Sicherheit“ die Option „Bedingter Zugriff“ aus. Jetzt wird der folgende Bildschirm angezeigt.

       ![Bedingter Zugriff – Richtlinien](./media/secure-aad-app/ad-mfa-conditional-add.png)

Gehen Sie wie folgt vor, wenn Sie keine neue Richtlinie erstellen können:

   1. Öffnen Sie die Registerkarte **Mehrstufige Authentifizierung**.
   2. Klicken Sie auf den Link zur **kostenlosen Testversion** von Azure AD Premium, und abonnieren Sie sie.

   ![Kostenlose Azure AD Premium-Testversion](./media/secure-aad-app/ad-trial-premium.png)

Kehren Sie zur Anzeige „Bedingter Zugriff“ zurück.

   1. Wählen Sie die Registerkarte „Neue Richtlinie“ aus.
   2. Geben Sie den Namen der Richtlinie ein.
   3. Wählen Sie die Benutzer oder Gruppen aus, für die die mehrstufige Authentifizierung aktiviert werden soll.
   4. Wählen Sie unter **Zugriffssteuerung** die Registerkarte **Zuweisung** aus, und wählen Sie dann **Require multi-factor authentication** (Mehrstufige Authentifizierung anfordern) (und gegebenenfalls andere Einstellungen) aus.

   ![Anfordern von MFA](./media/secure-aad-app/ad-mfa-conditional-add.png)

   Sie können die Richtlinie aktivieren, indem Sie das Kontrollkästchen in der Anzeige oben oder auf der Registerkarte **Bedingter Zugriff** aktivieren. Wenn die Richtlinie aktiviert ist, benötigen Benutzer eine mehrstufige Authentifizierung, um sich beim Portal anzumelden.

   Es gibt eine Basisrichtlinie, die mehrstufige Authentifizierung für alle Azure-Administratoren erfordert. Sie können diese im Portal direkt aktivieren. Wenn Sie diese Richtlinie aktivieren, wird Ihre aktuelle Sitzung möglicherweise ungültig und erfordert eine erneute Anmeldung.

   Gehen Sie wie folgt vor, wenn die Basisrichtlinie nicht aktiviert ist:
   1.   Klicken Sie auf **Require MFA for admins** (Mehrstufige Authentifizierung für Administratoren anfordern).
   2.   Wählen Sie dann **Richtlinie sofort verwenden** aus.

   ![Auswahl von „Richtlinie sofort verwenden“](./media/secure-aad-app/ad-mfa-conditional-enable.png)

#### <a name="use-azure-sentinel-to-monitor-apps-and-resources"></a>Verwenden von Azure Sentinel zum Überwachen von Apps und Ressourcen

   Mit dem Wachstum einer Anwendung wird es schwerer, alle von Ressourcen empfangenen Sicherheitssignale und Metriken zu aggregieren und auf aktionsorientierte Weise zu nutzen.

   Azure Sentinel wurde zum Erfassen von Daten, Ermitteln möglicher Bedrohungstypen und zur Bereitstellung eines Überblicks über Sicherheitsvorfälle entworfen.
Während auf einen manuellen Eingriff gewartet wird, kann Azure Sentinel auf vorgeschriebene Playbooks zurückgreifen, um Warnungen und Prozesse für die Vorfallverwaltung auszulösen.

   Die Beispiel-App besteht aus mehreren Ressourcen, die von Azure Sentinel überwacht werden können.
Sie müssen zunächst einen Log Analytics-Arbeitsbereich erstellen, der alle von den verschiedenen Ressourcen erfassten Daten speichert, um Azure Sentinel einzurichten.

So erstellen Sie diesen Arbeitsbereich

   1. Suchen Sie im Suchfeld des Azure-Portals nach **Log Analytics**. Wählen Sie **Log Analytics-Arbeitsbereiche** aus.

   ![Suche nach Log Analytics-Arbeitsbereichen](./media/secure-aad-app/sentinel-log-analytics.png)

   *Suche nach Log Analytics-Arbeitsbereichen*

   2. Klicken Sie auf der nächsten Seite auf **Hinzufügen**, und geben Sie dann einen Namen, eine Ressourcengruppe und einen Speicherort für den Arbeitsbereich an.
   ![Erstellen eines Log Analytics-Arbeitsbereichs](./media/secure-aad-app/sentinel-log-analytics-create.png)

   *Erstellen eines Log Analytics-Arbeitsbereichs*

   3. Suchen Sie über das Suchfeld nach **Azure Sentinel**.

   ![Suche nach Azure Sentinel](./media/secure-aad-app/sentinel-add.png)

   *Suche nach Azure Sentinel*

   4. Klicken Sie auf **Hinzufügen**, und wählen Sie dann den zuvor erstellten Log Analytics-Arbeitsbereich aus.

   ![Hinzufügen eines Log Analytics-Arbeitsbereichs](./media/secure-aad-app/sentinel-workspace-add.png)

   *Hinzufügen eines Log Analytics-Arbeitsbereichs*

   5. Wählen Sie auf der Seite **Azure Sentinel – Data connectors** (Azure Sentinel – Datenconnectors) unter **Konfiguration** die Option **Data connectors** (Datenconnectors) aus. Daraufhin werden Ihnen Azure-Dienste angezeigt, die Sie für die Analyse in Azure Sentinel mit der Log Analytics-Speicherinstanz verknüpfen können.

   ![Log Analytics-Datenconnectors](./media/secure-aad-app/sentinel-connectors.png)

      *Hinzufügen eines Datenconnectors zu Azure Sentinel*

   Führen Sie die folgenden Schritte aus, um beispielsweise das Anwendungsgateway zu verknüpfen:

   1. Öffnen Sie das Blatt der Azure Application Gateway-Instanz.
   2. Wählen Sie unter **Überwachung** die Option **Diagnoseeinstellungen** aus.
   3. Klicken Sie auf **Diagnoseeinstellung hinzufügen**.

   ![Hinzufügen der Application Gateway-Diagnose](./media/secure-aad-app/sentinel-gateway-connector.png)
         
   *Hinzufügen der Application Gateway-Diagnose*

   4. Wählen Sie auf der Seite **Diagnoseeinstellungen** den Log Analytics-Arbeitsbereich aus, den Sie erstellt haben, und wählen Sie dann alle Metriken aus, die Sie erfassen und an Azure Sentinel senden möchten. Wählen Sie **Speichern** aus.

   ![Einstellungen für den Azure Sentinel-Connector](./media/secure-aad-app/sentinel-connector-settings.png)



## <a name="cost-considerations"></a>Kostenbetrachtung
   Wenn Sie über kein Azure-Konto verfügen, können Sie ein kostenloses Konto erstellen. Rufen Sie zunächst die [Seite zur Erstellung eines kostenlosen Kontos](https://azure.microsoft.com/free/) auf, und sehen Sie sich an, was Sie mit einem kostenlosen Azure-Konto tun können und welche Produkte 12 Monate lang kostenlos genutzt werden können.

   Sie müssen für einige Premium-Features zahlen, die Sie zur Bereitstellung der Ressourcen für die Beispiel-App mit Sicherheitsfeatures benötigen. Mit der Skalierung der App müssen die kostenlosen Tarife und Testversionen von Azure aktualisiert werden, um die Anwendungsanforderungen zu erfüllen. Dadurch können die Kosten steigen. Verwenden Sie den [Azure-Preisrechner](https://azure.microsoft.com/pricing/calculator/), um Ihre Kosten zu ermitteln.

## <a name="next-steps"></a>Nächste Schritte
   Weitere Informationen zum Entwerfen, Entwickeln und Bereitstellen sicherer Anwendungen finden Sie in den folgenden Artikeln.

- [Entwurf](secure-design.md)
- [Entwickeln](secure-develop.md)
- [Bereitstellen](secure-deploy.md)
