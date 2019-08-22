---
title: Aufbau eines direkten Verbunds mit einem Identitätsanbieter für B2B-Azure Active Directory | Microsoft Docs
description: Direkter Verbund mit einem SAML- oder WS-Fed-Identitätsanbieter, damit sich Gäste bei Ihren Azure AD-Apps anmelden können
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 08/07/2019
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: f3aea108ed87debac56b18b5959d492f2bcb291d
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/08/2019
ms.locfileid: "68853599"
---
# <a name="direct-federation-with-ad-fs-and-third-party-providers-for-guest-users-preview"></a>Direkter Verbund mit AD FS und Drittanbietern für Gastnutzer (Vorschau)
|     |
| --- |
| Ein direkter Verbund ist eine öffentliche Vorschaufunktion von Azure Active Directory. Weitere Informationen zu Vorschauversionen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).|
|     |

Dieser Artikel beschreibt, wie Sie einen direkten Verbund mit einer anderen Organisation für die B2B-Zusammenarbeit einrichten können. Sie können auf einen direkten Verbund mit jeder beliebigen Organisation einrichten, dessen Identitätsanbieter (IdP) das SAML 2.0 oder WS-Fed-Protokoll unterstützt.
Wenn Sie einen direkten Verbund mit dem IdP eines Partners einrichten, können neue Gastbenutzer aus dieser Domäne ihr eigenes von IdP verwaltetes Organisationskonto verwenden, um sich bei Ihrem Azure AD Mandant anzumelden und mit Ihnen zusammenzuarbeiten. Es ist nicht erforderlich, dass der Gastbenutzer ein separates Azure AD-Konto erstellt.
> [!NOTE]
> Gastbenutzer des direkten Verbunds müssen sich mit einem Link anmelden, der den Mandantenkontext enthält (z. B. `https://myapps.microsoft.com/?tenantid=<tenant id>` oder `https://portal.azure.com/<tenant id>`, oder im Falle einer verifizierten Domain, `https://myapps.microsoft.com/\<verified domain>.onmicrosoft.com`). Direkte Links zu Anwendungen und Ressourcen funktionieren ebenfalls, sofern sie den Mandantenkontext enthalten. Benutzer des direkten Verbunds können sich derzeit nicht mit gemeinsamen Endpunkten anmelden, die keinen Mandantenkontext haben. Verwenden Sie beispielsweise `https://myapps.microsoft.com`, `https://portal.azure.com`, oder `https://teams.microsoft.com` führt zu einem Fehler.
 
## <a name="when-is-a-guest-user-authenticated-with-direct-federation"></a>Wann wird ein Gastbenutzer mit direktem Verbund authentifiziert?
Nachdem Sie den direkten Verbund mit einer Organisation eingerichtet haben, werden alle neuen Guest-Benutzer, die Sie einladen, mit einem direkten Verbund mit authentifiziert. Es ist wichtig zu beachten, dass die Einrichtung eines direkten Verbunds die Authentifizierungsmethode für Gastbenutzer, die bereits eine Einladung von Ihnen eingelöst haben, nicht ändert. Hier einige Beispiele:
 - Wenn Gastbenutzer bereits Einladungen von Ihnen eingelöst haben und Sie anschließend einen direkten Verbund mit ihrer Organisation einrichten, verwenden diese Gastbenutzer weiterhin die gleiche Authentifizierungsmethode, die sie vor der Einrichtung des direkten Verbunds verwendet haben.
 - Wenn Sie einen direkten Verbund mit einer Partnerorganisation einrichten und Gastbenutzer einladen und die Partnerorganisation später zu Azure AD wechselt, werden die Gastbenutzer, die bereits Einladungen eingelöst haben, weiterhin den direkten Verbund nutzen, solange die direkten Verbundsrichtlinien in Ihrem Mandant existiert.
 - Wenn Sie einen direkten Verbund mit einem Partnerunternehmen löschen, werden alle Gastbenutzer, die derzeit den direkten Verbund nutzen, sich nicht mehr anmelden können.

In jedem dieser Szenarien können Sie die Authentifizierungsmethode eines Gastbenutzers aktualisieren, indem Sie das Gastbenutzerkonto aus Ihrem Verzeichnis löschen und Sie wieder einladen.

Der direkte Verbund ist an Domain-Namensräume wie contoso.com und fabrikam.com gebunden. Wenn Unternehmen eine direkte Verbundskonfiguration mit AD FS oder einem IdP eines Drittanbieters einrichten, ordnen sie diesen IdPs einen oder mehrere Domain-Namensräume zu. 

## <a name="end-user-experience"></a>Endbenutzererfahrung 
Mit dem direkten Verbund melden sich Gastbenutzer mit ihrem eigenen Organisationskonto bei Ihrem Azure AD-Mandant an. Wenn sie auf gemeinsame Ressourcen zugreifen und zur Anmeldung aufgefordert werden, werden Benutzer des direkten Verbunds auf ihren IdP umgeleitet. Nach der erfolgreichen Anmeldung werden sie mit Azure AD zurückgeleitet, um Zugriff auf Ressourcen zu bekommen. Die Aktualisierungs-Token der Benutzer des direkten Verbunds sind 12 Stunden lang gültig, die [Standardlänge für die Weitergabe von Aktualisierungs-Token](../develop/active-directory-configurable-token-lifetimes.md#exceptions) in Azure AD. Wenn das verbundene IdP SSO aktiviert hat, wird der Benutzer SSO erleben und nach der ersten Authentifizierung keine Anmeldeaufforderung sehen.

## <a name="limitations"></a>Einschränkungen

### <a name="dns-verified-domains-in-azure-ad"></a>DNS-verfizierte Domänen in Azure AD
Die Domäne, mit der Sie einen Verbund einrichten möchten, darf in Azure AD ***nicht*** DNS-verifiziert werden. Es ist zulässig, einen direkten Verbund mit nicht verwalteten (E-Mail-verifizierten oder „viralen“) Azure AD-Mandanten einzurichten, da sie nicht DNS-verifiziert sind.
### <a name="authentication-url"></a>Authentifizierungs-URL
Ein direkter Verbund ist nur für Richtlinien zulässig, bei denen die Domäne die Authentifizierungs-URL mit der Zieldomäne übereinstimmt oder bei denen die Authentifizierungs-URL einer dieser zulässigen Identitätsanbieter ist (diese Liste kann sich ändern):
-   accounts.google.com
-   pingidentity.com
-   login.pingone.com
-   okta.com
-   my.salesforce.com
-   federation.exostar.com
-   federation.exostartest.com

Wenn Sie beispielsweise einen direkten Verbund für **fabrikam.com** einrichten, `https://fabrikam.com/adfs`wird die Authentifizierungs-URL die Validierung bestehen. Ein Host in der gleichen Domäne wird ebenfalls passieren, z.B. `https://sts.fabrikam.com/adfs`. Die Authentifizierungs-URL `https://fabrikamconglomerate.com/adfs` oder `https://fabrikam.com.uk/adfs` für die gleiche Domäne wird jedoch nicht passieren.

### <a name="signing-certificate-renewal"></a>Unterzeichnung der Zertifikatsverlängerung
Wenn Sie die Metadaten-URL in den Einstellungen des Identitätsanbieters angeben, erneuert Azure AD automatisch das Signaturzertifikat, wenn es abläuft. Wenn das Zertifikat jedoch aus irgendeinem Grund vor Ablauf der Gültigkeitsdauer gedreht wird oder wenn Sie keine Metadaten-URL angeben, kann Azure AD es nicht verlängern. In diesem Fall müssen Sie das Signaturzertifikat manuell aktualisieren.

### <a name="limit-on-federation-relationships"></a>Limit für Verbundbeziehungen
Derzeit werden maximal 1.000 Verbundbeziehungen unterstützt. Dieses Limit umfasst sowohl [interne Verbünde](https://docs.microsoft.com/powershell/module/msonline/set-msoldomainfederationsettings?view=azureadps-1.0) als auch direkte Verbünde.
## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen
### <a name="can-i-set-up-direct-federation-with-a-domain-for-which-an-unmanaged-email-verified-tenant-exists"></a>Kann ich einen direkten Verbund mit einer Domäne einrichten, für die ein nicht verwalteter (per E-Mail verifizierten) Mandant vorhanden ist? 
Ja. Wenn die Domäne nicht verifiziert wurde und der Mandant keine [Übernahme durch den Administrator](../users-groups-roles/domains-admin-takeover.md) erfahren hat, können Sie einen direkten Verbund mit dieser Domäne einrichten. Nicht verwaltete oder per E-Mail verifizierte Mandanten werden erstellt, wenn ein Benutzer eine B2B-Einladung einlöst oder eine Self-Service-Anmeldung für Azure AD über eine Domain durchführt, die derzeit nicht existiert. Sie können einen direkten Verbund für diese Domänen einrichten. Wenn Sie versuchen, einen direkten Verbund mit einer DNS-verifizierten Domäne einzurichten, entweder im Azure-Portal oder über PowerShell, wird eine Fehlermeldung angezeigt.
### <a name="if-direct-federation-and-email-one-time-passcode-authentication-are-both-enabled-which-method-takes-precedence"></a>Wenn sowohl der direkte Verbund als auch die Einmalpasswort-Authentifizierung per E-Mail aktiviert sind, welches Verfahren hat dann Vorrang?
Wenn ein direkter Verbund mit einer Partnerorganisation eingerichtet wird, hat sie Vorrang vor der Einmalpasswort-Authentifizierung per E-Mail für neue Gastbenutzer aus dieser Organisation. Wenn ein Gastbenutzer eine Einladung mit Einmalpasswort-Authentifizierung eingelöst hat, bevor Sie einen direkten Verbund einrichten, verwendet er weiterhin die Einmalpasswort-Authentifizierung. 
### <a name="does-direct-federation-address-sign-in-issues-due-to-a-partially-synced-tenancy"></a>Behebt der direkte Verband Probleme bei der Anmeldung aufgrund eines teilweise synchronisierten Mandantenverhältnisses?
Nein, in diesem Fall sollte die [E-Mail-Funktion für einmalige Passwörter](one-time-passcode.md) verwendet werden. Ein „teilweise synchronisiertes Mandantenverhältnis“ bezieht sich auf einen Partner Azure AD-Mandant, bei dem die Benutzeridentitäten vor Ort nicht vollständig mit der Cloud synchronisiert sind. Ein Gast, dessen Identität noch nicht in der Cloud existiert, der aber versucht, Ihre B2B-Einladung einzulösen, wird sich nicht anmelden können. Die Einmalkennung des Passworts würde es diesem Benutzer erlauben sich anzumelden. Die Funktion des direkten Verbunds befasst sich mit Situationen, in denen der Gast über ein eigenes IdP-verwaltetes Organisationskonto verfügt, aber das Unternehmen überhaupt keine Azure AD-Präsenz hat.

## <a name="step-1-configure-the-partner-organizations-identity-provider"></a>Schritt 1: Konfigurieren Sie den Identitätsanbieter der Partnerorganisation
Zunächst muss Ihre Partnerorganisation ihren Identitätsanbieter mit den erforderlichen Ansprüchen und Vertrauenspersonen vertraut machen. 

> [!NOTE]
> Um zu veranschaulichen, wie Sie einen Identitätsanbieter für den direkten Verbund konfigurieren, verwenden wir als Beispiel Active Directory Federation Services (AD FS). Lesen Sie den Artikel [Configure direct federation with AD FS](direct-federation-adfs.md), der Beispiele für die Konfiguration von AD FS als SAML 2.0 oder WS-Fed Identity Provider zur Vorbereitung auf die direkte Föderation enthält.

### <a name="saml-20-configuration"></a>SAML 2.0 Konfiguration

Azure AD B2B kann so konfiguriert werden, dass es mit Identitätsanbietern zusammenarbeitet, die das SAML-Protokoll mit den unten aufgeführten spezifischen Anforderungen verwenden. Weitere Informationen zum Einrichten eines Vertrauens zwischen Ihrem SAML-Identitätsanbieter und Azure AD finden Sie unter [Verwenden eines SAML 2.0 Identity Provider (IdP) für Single Sign-On](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-fed-saml-idp).  

> [!NOTE]
> HINWEIS Die Zieldomäne für die direkte Föderation darf auf Azure AD nicht DNS-verifiziert sein. Die Authentifizierungs-URL-Domäne muss mit der Zieldomäne übereinstimmen oder die Domäne eines zugelassenen Identitätsanbieters sein. Siehe Abschnitt [Einschränkungen](#limitations) für Details. 

#### <a name="required-saml-20-attributes-and-claims"></a>Erforderliche SAML 2.0 Attribute und Ansprüche
Die folgenden Tabellen zeigen die Anforderungen an bestimmte Attribute und Ansprüche, die beim Drittanbieter für Identitäten konfiguriert werden müssen. Um eine direkten Verbund einzurichten, müssen die folgenden Attribute in der SAML 2.0-Antwort vom Identitätsanbieter empfangen werden. Diese Attribute können durch Verlinkung mit der XML-Datei des Online-Sicherheitstoken-Service oder durch manuelle Eingabe konfiguriert werden.

Erforderliche Attribute für die SAML 2.0-Antwort vom IdP:

|Attribut  |Wert  |
|---------|---------|
|AssertionConsumerService     |`https://login.microsoftonline.com/login.srf`         |
|Zielgruppe     |`urn:federation:MicrosoftOnline`         |
|Issuer (Aussteller)     |Die Emittenten-URI des Partners IdP, z.B. `http://www.example.com/exk10l6w90DHM0yi...`         |


Erforderliche Ansprüche für das SAML 2.0-Token vom Identitätsanbieter ausgestellt:

|Attribut  |Wert  |
|---------|---------|
|NameID-Format     |`urn:oasis:names:tc:SAML:2.0:nameid-format:persistent`         |
|emailaddress     |`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`         |

### <a name="ws-fed-configuration"></a>WS-Fed Konfiguration 
Azure AD B2B kann so konfiguriert werden, dass es sich mit Identitätsanbietern verbindet, die das WS-Fed-Protokoll mit einigen spezifischen Anforderungen verwenden, wie unten aufgeführt. Derzeit wurden die beiden WS-Fed-Anbieter auf Kompatibilität mit Azure AD getestet, einschließlich AD FS und Shibboleth. Weitere Informationen zum Aufbau eines Vertrauens zwischen einem WS-Fed-kompatiblen Anbieter mit Azure AD finden Sie im "STS Integration Paper using WS Protocols", das in den [Kompatibilitätsdokumenten von Azure AD Identity Provider verfügbar ist](https://www.microsoft.com/download/details.aspx?id=56843).

> [!NOTE]
> Die Zieldomäne für den direkten Verbund darf auf Azure AD nicht DNS-verifiziert sein. Die Authentifizierungs-URL-Domäne muss entweder mit der Zieldomäne oder der Domäne eines zugelassenen Identitätsanbieters übereinstimmen. Siehe Abschnitt [Einschränkungen](#limitations) für Details. 

#### <a name="required-ws-fed-attributes-and-claims"></a>Erforderliche WS-Fed Attribute und Ansprüche

Die folgenden Tabellen zeigen Anforderungen für bestimmte Attribute und Ansprüche, die beim externen WS-Fed Identitätsanbieter konfiguriert werden müssen. Um eine direkten Verbund einzurichten, müssen die folgenden Attribute in der WS-Fed-Nachricht vom Identitätsanbieter empfangen werden. Diese Attribute können durch Verlinkung mit der XML-Datei des Online-Sicherheitstoken-Service oder durch manuelle Eingabe konfiguriert werden.

Erforderliche Attribute in der WS-Fed-Nachricht vom IdP:
 
|Attribut  |Wert  |
|---------|---------|
|PassiveRequestorEndpoint     |`https://login.microsoftonline.com/login.srf`         |
|Zielgruppe     |`urn:federation:MicrosoftOnline`         |
|Issuer (Aussteller)     |Die Emittenten-URI des Partners IdP, z.B. `http://www.example.com/exk10l6w90DHM0yi...`         |

Erforderliche Ansprüche für den WS-Fed-Token vom Identitätsanbieter ausgestellt:

|Attribut  |Wert  |
|---------|---------|
|ImmutableID     |`http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID`         |
|emailaddress     |`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`         |

## <a name="step-2-configure-direct-federation-in-azure-ad"></a>Schritt 2: Direkten Verbund in Azure AD konfigurieren 
Als nächstes konfigurieren Sie den direkten Verbund mit dem in Schritt 1 in Azure AD konfigurierten Identitätsanbieter. Sie können das Azure-Portal oder PowerShell verwenden. Es kann 5-10 Minuten dauern, bis die Richtlinien des direkten Verbunds in Kraft treten. Versuchen Sie in dieser Zeit nicht, eine Einladung für die direkte Verbunddomäne einzulösen. Die folgenden Attribute sind erforderlich:
- Aussteller-URI der Partner IdP
- Passiver Authentifizierungs-Endpunkt des Partner-IdP (nur https wird unterstützt)
- Zertifikat

### <a name="to-configure-direct-federation-in-the-azure-ad-portal"></a>So konfigurieren Sie den direkten Verbund im Azure AD-Portal

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com/). Wählen Sie im linken Bereich **Azure Active Directory** aus. 
2. Wählen Sie **Organisationsbeziehungen** aus.
3. Wählen Sie **Identitätsanbieter** und dann **Neue SAML/WS-Fed IdP** aus.

    ![Screenshot mit Schaltfläche zum Hinzufügen einer neuen SAML oder WS-Fed IdP](media/direct-federation/new-saml-wsfed-idp.png)

4. Wählen Sie auf der Seite **New SAML/WS-Fed IdP** unter **Identity Provider Protocol** **SAML** oder **WS-FED**.

    ![Screenshot mit Parse-Button auf der SAML- oder WS-Fed-IdP-Seite](media/direct-federation/new-saml-wsfed-idp-parse.png)

5. Geben Sie den Domainnamen Ihrer Partnerorganisation ein, der der Zieldomainname für den direkten Verbund sein wird
6. Sie können eine Metadatendatei hochladen, um Metadatendetails zu füllen. Wenn Sie sich für die manuelle Eingabe von Metadaten entscheiden, geben Sie die folgenden Informationen ein:
   - Domain-Name des Partners IdP
   - Entitäts-ID des Partners IdP
   - Passive Anforderung oder Endpunkt des Partners IdP
   - Zertifikat
   > [!NOTE]
   > Metadaten-URL ist optional, wird jedoch dringend empfohlen. Wenn Sie die Metadaten-URL angeben, kann Azure AD das Signaturzertifikat nach Ablauf automatisch verlängern. Wenn das Zertifikat aus irgendeinem Grund vor Ablauf der Gültigkeitsdauer gedreht wird oder wenn Sie keine Metadaten-URL angeben, kann Azure AD es nicht verlängern. In diesem Fall müssen Sie das Signaturzertifikat manuell aktualisieren.

7. Wählen Sie **Speichern** aus. 

### <a name="to-configure-direct-federation-in-azure-ad-using-powershell"></a>So konfigurieren Sie den direkten Verbund in Azure AD mit PowerShell

1. Installieren Sie die neueste Version des Azure AD PowerShell für Graph-Moduls ([AzureADPreview](https://www.powershellgallery.com/packages/AzureADPreview)). (Wenn Sie detaillierte Schritte benötigen, enthält der Schnellstart zum Hinzufügen eines Gastbenutzers den Abschnitt [Installation des neuesten AzureADPreview-Moduls](b2b-quickstart-invite-powershell.md#install-the-latest-azureadpreview-module).) 
2. Führen Sie den folgenden Befehl aus: 
   ```powershell
   Connect-AzureAD
   ```
1. Melden Sie sich an der Anmeldeaufforderung mit dem verwalteten globalen Administratorkonto an. 
2. Führen Sie die folgenden Befehle aus, und Ersetzen Sie dabei die Werte aus der Datei mit den Verbundmetadaten. Bei AD FS-Server und Okta, ist die Verbund-Datei "FederationMetadata.xml", z. B.: `https://sts.totheclouddemo.com/federationmetadata/2007-06/federationmetadata.xml`. 

   ```powershell
   $federationSettings = New-Object Microsoft.Open.AzureAD.Model.DomainFederationSettings
   $federationSettings.PassiveLogOnUri ="https://sts.totheclouddemo.com/adfs/ls/"
   $federationSettings.LogOffUri = $federationSettings.PassiveLogOnUri
   $federationSettings.IssuerUri = "http://sts.totheclouddemo.com/adfs/services/trust"
   $federationSettings.MetadataExchangeUri="https://sts.totheclouddemo.com/adfs/services/trust/mex"
   $federationSettings.SigningCertificate= <Replace with X509 signing cert’s public key>
   $federationSettings.PreferredAuthenticationProtocol="WsFed" OR "Samlp"
   $domainName = <Replace with domain name>
   New-AzureADExternalDomainFederation -ExternalDomainName $domainName  -FederationSettings $federationSettings
   ```

## <a name="step-3-test-direct-federation-in-azure-ad"></a>Schritt 3: Direkten Verbund in Azure AD konfigurieren
Jetzt testen Sie Ihr Setup des direkten Verbunds, indem Sie einen neuen B2B-Gastbenutzer einladen. Weitere Informationen finden Sie unter [Hinzufügen von Azure AD B2B-Collaboration-Benutzern im Azure-Portal](add-users-administrator.md).
 
## <a name="how-do-i-edit-a-direct-federation-relationship"></a>Wie bearbeite ich ein direkten Verbundbeziehung?

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com/). Wählen Sie im linken Bereich **Azure Active Directory** aus. 
2. Wählen Sie **Organisationsbeziehungen** aus.
3. Wählen Sie **Identitätsanbieter** aus
4. Wählen Sie unter **SAML/WS-Fed-Identitätsanbieter**, den Anbieter.
5. Aktualisieren Sie im Detailbereich des Identitätsanbieters die Werte.
6. Wählen Sie **Speichern** aus.


## <a name="how-do-i-remove-direct-federation"></a>Wie entferne ich einen direkten Verbund?
Sie können das Setup des direkten Verbunds entfernen. Wenn Sie dies tun, können sich direkte Gastbenutzer des Verbunds, die ihre Einladungen bereits eingelöst haben, nicht mehr anmelden. Aber Sie können ihnen wieder Zugriff auf Ihre Ressourcen gewähren, indem Sie sie aus dem Verzeichnis löschen und wieder einladen. So entfernen Sie einen direkten Verbund mit einem Identitätsanbieter in Azure AD-portal:

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com/). Wählen Sie im linken Bereich **Azure Active Directory** aus. 
2. Wählen Sie **Organisationsbeziehungen** aus.
3. Wählen Sie **Identitätsanbieter** aus.
4. Wählen Sie den Identitätsanbieter und dann **Benutzer löschen** aus. 
5. Wählen Sie **Ja** aus, um den Löschvorgang zu bestätigen. 

So entfernen Sie einen direkten Verbund mit einem Identitätsanbieter mit PowerShell:
1. Installieren Sie die neueste Version des Azure AD PowerShell für Graph-Moduls ([AzureADPreview](https://www.powershellgallery.com/packages/AzureADPreview)).
2. Führen Sie den folgenden Befehl aus: 
   ```powershell
   Connect-AzureAD
   ```
3. Melden Sie sich an der Anmeldeaufforderung mit dem verwalteten globalen Administratorkonto an. 
4. Geben Sie den folgenden Befehl ein:
   ```powershell
   Remove-AzureADExternalDomainFederation -ExternalDomainName  $domainName
   ```
