---
title: Aufbau eines direkten Verbunds mit einem AD FS for B2B – Azure Active Directory | Microsoft Docs
description: Erfahren Sie, wie Sie AD FS als Identitätsanbieter für den direkten Verbund einrichten können, damit sich Gäste bei Ihren Azure AD Apps anmelden können
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 07/01/2019
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: a8f709186f0ef17e037c4203118be07ea2d4f511
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2019
ms.locfileid: "67544628"
---
# <a name="example-direct-federation-with-active-directory-federation-services-ad-fs-preview"></a>Beispiel: Direkter Verbund mit Active Directory Federation Services (AD FS) (Vorschau)
|     |
| --- |
| Ein direkter Verbund ist eine öffentliche Vorschaufunktion von Azure Active Directory. Weitere Informationen zu Vorschauversionen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).|
|     |

Dieser Artikel beschreibt, wie Sie einen [direkten Verbund](direct-federation.md) mit Active Directory Federation Services (AD FS) als SAML 2.0- oder WS-Fed-Identitätsanbieter einrichten. Um die direkte Föderation zu unterstützen, müssen bestimmte Attribute und Ansprüche beim Identitätsanbieter konfiguriert werden. Um zu veranschaulichen, wie Sie einen Identitätsanbieter für den direkten Verbund konfigurieren, verwenden wir als Beispiel Active Directory Federation Services (AD FS). Wir zeigen, wie man AD FS sowohl als SAML Identity Provider als auch als WS-Fed Identity Provider einrichtet.

> [!NOTE]
> Dieser Artikel beschreibt, wie Sie AD FS sowohl für SAML als auch für WS-Fed zur Veranschaulichung einrichten. Für direkte die Integration eines direkten Verbunds, bei dem der Identitätsanbieter AD FS ist, empfehlen wir die Verwendung von WS-Fed als Protokoll. 

## <a name="configure-ad-fs-for-saml-20-direct-federation"></a>AD FS für SAML 2.0 direkter Verbund konfigurieren
Azure AD B2B kann so konfiguriert werden, dass es mit Identitätsanbietern zusammenarbeitet, die das SAML-Protokoll mit den unten aufgeführten spezifischen Anforderungen verwenden. Um die SAML-Konfigurationsschritte zu veranschaulichen, wird in diesem Abschnitt Gewusst wie: Einrichten von AD FS für SAML 2.0. 

Um einen direkten Verbund einzurichten, müssen die folgenden Attribute in der SAML 2.0-Antwort vom Identitätsanbieter empfangen werden. Diese Attribute können durch Verlinkung mit der XML-Datei des Online-Sicherheitstoken-Service oder durch manuelle Eingabe konfiguriert werden. Schritt 12 in [Erstellen einer AD FS-Testinstanz](https://medium.com/in-the-weeds/create-a-test-active-directory-federation-services-3-0-instance-on-an-azure-virtual-machine-9071d978e8ed) beschreibt, wie Sie die AD FS-Endpunkte finden oder wie Sie Ihre Metadaten-URL generieren, z.B. `https://fs.iga.azure-test.net/federationmetadata/2007-06/federationmetadata.xml`. 

|Attribut  |Wert  |
|---------|---------|
|AssertionConsumerService     |`https://login.microsoftonline.com/login.srf`         |
|Zielgruppe     |`urn:federation:MicrosoftOnline`         |
|Issuer (Aussteller)     |Die Emittenten-URI des Partners IdP, z.B. `http://www.example.com/exk10l6w90DHM0yi...`         |

Die folgenden Ansprüche müssen in dem vom Identitätsanbieter ausgegebenen SAML 2.0-Token konfiguriert werden:


|Attribut  |Wert  |
|---------|---------|
|NameID-Format     |`urn:oasis:names:tc:SAML:2.0:nameid-format:persistent`         |
|emailaddress     |`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`         |


Im nächsten Abschnitt wird veranschaulicht, wie Sie die erforderlichen Attribute und Ansprüche am Beispiel von AD FS als Beispiel eines SAML 2.0 Identity Providers konfigurieren.

### <a name="before-you-begin"></a>Voraussetzungen

Ein AD FS-Server muss bereits eingerichtet und funktionsfähig sein, bevor Sie mit diesem Vorgang beginnen. Hilfe bei der Einrichtung eines AD FS-Servers finden Sie unter [Erstellen einer Test-AD FS 3.0-Instanz auf einer virtuellen Azure-Maschine](https://medium.com/in-the-weeds/create-a-test-active-directory-federation-services-3-0-instance-on-an-azure-virtual-machine-9071d978e8ed).

### <a name="add-the-claim-description"></a>Hinzufügen der Anspruchsbeschreibung

1. Wählen Sie auf dem AD FS-Server **Tools** > **AD FS-Verwaltung**.
2. Wählen Sie im Navigationsbereich **Service** > **Anspruchsbeschreibungen**.
3. Klicken Sie unter **Aktionen**auf **Anspruchsbeschreibung hinzufügen**.
4. Geben Sie im Fenster **Add a Claim Description** die folgenden Werte ein:

   - **Anzeigename**: Beständiger Bezeichner
   - **Anspruchsbezeichner**: `urn:oasis:names:tc:SAML:2.0:nameid-format:persistent` 
   - Aktivieren Sie das Kontrollkästchen für die **Veröffentlichung dieser Fallbeschreibung in Verbundsmetadaten als Fallart, die dieser Verbundsdienst akzeptieren kann**.
   - Aktivieren Sie das Kontrollkästchen für die **Veröffentlichung dieser Fallbeschreibung in Verbundsmetadaten als Fallart, die dieser Verbundsdienst akzeptieren kann**.

5. Klicken Sie auf **OK**.

### <a name="add-the-relying-party-trust-and-claim-rules"></a>Hinzufügen der Vertrauensperson und der Anspruchsregeln der vertrauenswürdigen Partei

1. Gehen Sie auf dem AD FS-Server zu **Tools** >  **AD FS Verwaltung**.
2. Wählen Sie im Navigationsbereich **Trust Relationships** >  **Relying Party Trusts**.
3. Wählen Sie unter **Aktionen** die Option **Add Relying Party Trust**. 
4. Verwenden Sie im Assistenten zum Hinzufügen von Vertrauenspersonen unter **Datenquelle auswählen** die Option **Daten über die Vertrauensperson importieren, die online oder in einem lokalen Netzwerk veröffentlicht wurde**. Geben Sie diese Verbundsmetadaten-URL an – https://nexus.microsoftonline-p.com/federationmetadata/saml20/federationmetadata.xml. Belassen Sie andere Standardauswahlen. Klicken Sie auf **Schließen**.
5. Der Assistent zum **Bearbeiten von Fallregeln** wird geöffnet.
6. Wählen Sie im Assistenten für die **Bearbeitung von Fallregeln** die Option **Regel hinzufügen**. Wählen Sie unter **Regeltyp auswählen** die Option **LDAP-Attribute als Anspruch senden**. Klicken Sie auf **Weiter**.
7. Geben Sie unter **Fallregel konfigurieren** die folgenden Werte an: 

   - **Name der Fallregel** : E-Mail-Anspruchsregel 
   - **Attribut Speicher**: Active Directory 
   - **LDAP-Attribut**: E-Mail-Adressen 
   - **Typ des ausgehenden Anspruchs**: E-Mail-Adresse

8. Wählen Sie **Fertig stellen** aus.
9. Die **Fallregeln bearbeiten** Fenster zeigt die neue Regel. Klicken Sie auf **Anwenden**. 
10. Klicken Sie auf **OK**.  

### <a name="create-an-email-transform-rule"></a>Erstellen einer E-Mail-Transformationsregel
1. Gehen Sie zu **Fallregeln bearbeiten** und klicken Sie auf **Regel hinzufügen**. Wählen Sie unter **Regeltyp auswählen** die Option **Einen eingehenden Anspruch transformieren** und klicken Sie auf **Weiter**. 
2. Geben Sie unter **Fallregel konfigurieren** die folgenden Werte an: 

   - **Name der Fallregel** : Erstellen einer E-Mail-Transformationsregel 
   - **Eingehnder Anspruchstyp**: E-Mail-Adresse 
   - **Ausgehender Anspruchstyp**: Namens-ID 
   - **Ausgehendes Namens-ID-Format**: Beständiger Bezeichner 
   - Wählen Sie **Durchlauf aller Anspruchswerte**.

3. Klicken Sie auf **Fertig stellen**. 
4. Die **Fallregeln bearbeiten** Fenster zeigt die neue Regel. Klicken Sie auf **Anwenden**. 
5. Klicken Sie auf **OK**. AD FS-Server ist jetzt für den direkten Verbund mit dem SAML 2.0-Protokoll konfiguriert.

## <a name="configure-ad-fs-for-ws-fed-direct-federation"></a>AD FS für SAML 2.0 direkten Verbund konfigurieren 
Azure AD B2B kann konfiguriert werden, um sich mit Identitätsanbietern zu verbinden, die das WS-Fed-Protokoll mit den unten aufgeführten spezifischen Anforderungen verwenden. Derzeit wurden die beiden WS-Fed-Anbieter auf Kompatibilität mit Azure AD getestet, darunter AD FS und Shibboleth. Hier werden wir Active Directory Federation Services (AD FS) als Beispiel für den WS-Fed Identitätsanbieter verwenden. Weitere Informationen zum Aufbau eines Vertrauens zwischen einem WS-Fed-kompatiblen Anbieter mit Azure AD finden Sie in den Kompatibilitätsdokumenten für Azure AD Identity Provider.

Um einen direkten Verbund einzurichten, müssen die folgenden Attribute in der WS-Fed-Nachricht vom Identitätsanbieter empfangen werden. Diese Attribute können durch Verlinkung mit der XML-Datei des Online-Sicherheitstoken-Service oder durch manuelle Eingabe konfiguriert werden. Schritt 12 in [Erstellen einer AD FS-Testinstanz](https://medium.com/in-the-weeds/create-a-test-active-directory-federation-services-3-0-instance-on-an-azure-virtual-machine-9071d978e8ed) beschreibt, wie Sie die AD FS-Endpunkte finden oder wie Sie Ihre Metadaten-URL generieren, z.B. `https://fs.iga.azure-test.net/federationmetadata/2007-06/federationmetadata.xml`.
 
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

Im nächsten Abschnitt wird veranschaulicht, wie Sie die erforderlichen Attribute und Ansprüche am Beispiel von AD FS als WS-Fed Identity Provider konfigurieren können.

### <a name="before-you-begin"></a>Voraussetzungen
Ein AD FS-Server muss bereits eingerichtet und funktionsfähig sein, bevor Sie mit diesem Vorgang beginnen. Hilfe bei der Einrichtung eines AD FS-Servers finden Sie unter [Erstellen einer Test-AD FS 3.0-Instanz auf einer virtuellen Azure-Maschine](https://medium.com/in-the-weeds/create-a-test-active-directory-federation-services-3-0-instance-on-an-azure-virtual-machine-9071d978e8ed).


### <a name="add-the-relying-party-trust-and-claim-rules"></a>Hinzufügen der Vertrauensperson und der Anspruchsregeln der vertrauenswürdigen Partei 
1. Gehen Sie auf dem AD FS-Server zu **Tools** >  **AD FS Verwaltung**. 
1. Wählen Sie im Navigationsbereich **Trust Relationships** >  **Relying Party Trusts**. 
1. Wählen Sie unter **Aktionen** die Option **Add Relying Party Trust**.  
1. Verwenden Sie im Assistenten zum Hinzufügen von Vertrauenspersonen unter **Datenquelle auswählen** die Option **Daten über die Vertrauensperson importieren, die online oder in einem lokalen Netzwerk veröffentlicht wurde**. Geben Sie diese Verbundsmetadaten-URL an -`https://nexus.microsoftonline-p.com/federationmetadata/2007-06/federationmetadata.xml`.  Belassen Sie andere Standardauswahlen. Klicken Sie auf **Schließen**.
1. Der Assistent zum **Bearbeiten von Fallregeln** wird geöffnet. 
1. Wählen Sie im Assistenten für die **Bearbeitung von Fallregeln** die Option **Regel hinzufügen**. Wählen Sie unter  **Regeltyp auswählen** die Option **Ansprüche mit einer benutzerdefinierten Regel senden**. Klicken Sie auf *Weiter*. 
1. Geben Sie unter **Fallregel konfigurieren** die folgenden Werte an:

   - **Name der Fallregel** : Unveränderliche Id herausgeben  
   - **Benutzerdefinierte Regel**: `c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"] => issue(store = "Active Directory", types = ("http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID"), query = "samAccountName={0};objectGUID;{1}", param = regexreplace(c.Value, "(?<domain>[^\\]+)\\(?<user>.+)", "${user}"), param = c.Value);`

1. Wählen Sie **Fertig stellen** aus. 
1. Die **Fallregeln bearbeiten** Fenster zeigt die neue Regel. Klicken Sie auf **Anwenden**.  
1. Wählen Sie im gleichen Assistenten für die **Bearbeitung von Fallregeln** die Option **Regel hinzufügen**. Wählen Sie unter **Cohose Rule Type** die Option **Send LDAP Attributes as Claims**. Klicken Sie auf **Weiter**.
1. Geben Sie unter **Fallregel konfigurieren** die folgenden Werte an: 

   - **Name der Fallregel** : E-Mail-Anspruchsregel  
   - **Attribut Speicher**: Active Directory  
   - **LDAP-Attribut**: E-Mail-Adressen  
   - **Typ des ausgehenden Anspruchs**: E-Mail-Adresse 

1.  Wählen Sie **Fertig stellen** aus. 
1.  Die **Fallregeln bearbeiten** Fenster zeigt die neue Regel. Klicken Sie auf **Anwenden**.  
1.  Klicken Sie auf **OK**. Der AD FS-Server ist nun für den direkten Verbund mit WS-Fed konfiguriert.

## <a name="next-steps"></a>Nächste Schritte
Als Nächstes [konfigurieren Sie den direkten Verbund in Azure AD](direct-federation.md#step-2-configure-direct-federation-in-azure-ad) entweder im Azure AD-Portal oder mit PowerShell. 
