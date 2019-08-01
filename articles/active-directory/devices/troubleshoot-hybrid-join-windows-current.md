---
title: Beheben von Problemen mit Geräten mit Hybrideinbindung in Azure Active Directory – Azure Active Directory
description: Beheben von Problemen mit Geräten unter Windows 10 und Windows Server 2016 mit Hybrideinbindung in Azure Active Directory
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: troubleshooting
ms.date: 06/28/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jairoc
ms.collection: M365-identity-device-management
ms.openlocfilehash: c4b0b5bd5972e544c4254ee0f425e27cc8c465f0
ms.sourcegitcommit: a8b638322d494739f7463db4f0ea465496c689c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/17/2019
ms.locfileid: "68297577"
---
# <a name="troubleshooting-hybrid-azure-active-directory-joined-devices"></a>Beheben von Problemen mit Geräten mit Hybrideinbindung in Azure Active Directory 

Der Inhalt dieses Artikels ist für Geräte mit Windows 10 oder Windows Server 2016.

Weitere Informationen zu anderen Windows-Clients finden Sie im Artikel [„Beheben von Problemen mit Geräten mit Hybrideinbindung in Azure Active Directory“](troubleshoot-hybrid-join-windows-legacy.md).

In diesem Artikel wird vorausgesetzt, dass Sie [Geräte mit Hybrideinbindung in Azure Active Directory konfiguriert haben](hybrid-azuread-join-plan.md), um die folgenden Szenarien zu unterstützen:

- Gerätebasierter bedingter Zugriff
- [Unternehmensroaming von Einstellungen](../active-directory-windows-enterprise-state-roaming-overview.md)
- [Windows Hello for Business](../active-directory-azureadjoin-passport-deployment.md)

Dieses Dokument enthält Anleitungen zur Problembehandlung zum Beheben potenzieller Probleme. 

Für Windows 10 und Windows Server 2016 unterstützt ein Azure Active Directory-Hybridbeitritt das Windows 10-Update vom November 2015 und höher.

## <a name="troubleshoot-join-failures"></a>Beheben von Verknüpfungsfehlern

### <a name="step-1-retrieve-the-join-status"></a>Schritt 1: Abrufen des Beitrittsstatus 

**So rufen Sie den Beitrittsstatus ab**

1. Öffnen Sie eine Eingabeaufforderung als Administrator
2. Geben Sie `dsregcmd /status` ein

```
+----------------------------------------------------------------------+
| Device State                                                         |
+----------------------------------------------------------------------+

    AzureAdJoined: YES
 EnterpriseJoined: NO
         DeviceId: 5820fbe9-60c8-43b0-bb11-44aee233e4e7
       Thumbprint: B753A6679CE720451921302CA873794D94C6204A
   KeyContainerId: bae6a60b-1d2f-4d2a-a298-33385f6d05e9
      KeyProvider: Microsoft Platform Crypto Provider
     TpmProtected: YES
     KeySignTest: : MUST Run elevated to test.
              Idp: login.windows.net
         TenantId: 72b988bf-xxxx-xxxx-xxxx-2d7cd011xxxx
       TenantName: Contoso
      AuthCodeUrl: https://login.microsoftonline.com/msitsupp.microsoft.com/oauth2/authorize
   AccessTokenUrl: https://login.microsoftonline.com/msitsupp.microsoft.com/oauth2/token
           MdmUrl: https://enrollment.manage-beta.microsoft.com/EnrollmentServer/Discovery.svc
        MdmTouUrl: https://portal.manage-beta.microsoft.com/TermsOfUse.aspx
  dmComplianceUrl: https://portal.manage-beta.microsoft.com/?portalAction=Compliance
      SettingsUrl: eyJVcmlzIjpbImh0dHBzOi8va2FpbGFuaS5vbmUubWljcm9zb2Z0LmNvbS8iLCJodHRwczovL2thaWxhbmkxLm9uZS5taWNyb3NvZnQuY29tLyJdfQ==
   JoinSrvVersion: 1.0
       JoinSrvUrl: https://enterpriseregistration.windows.net/EnrollmentServer/device/
        JoinSrvId: urn:ms-drs:enterpriseregistration.windows.net
    KeySrvVersion: 1.0
        KeySrvUrl: https://enterpriseregistration.windows.net/EnrollmentServer/key/
         KeySrvId: urn:ms-drs:enterpriseregistration.windows.net
     DomainJoined: YES
       DomainName: CONTOSO

+----------------------------------------------------------------------+
| User State                                                           |
+----------------------------------------------------------------------+

             NgcSet: YES
           NgcKeyId: {C7A9AEDC-780E-4FDA-B200-1AE15561A46B}
    WorkplaceJoined: NO
      WamDefaultSet: YES
WamDefaultAuthority: organizations
       WamDefaultId: https://login.microsoft.com
     WamDefaultGUID: {B16898C6-A148-4967-9171-64D755DA8520} (AzureAd)
         AzureAdPrt: YES
```

### <a name="step-2-evaluate-the-join-status"></a>Schritt 2: Bewerten des Beitrittsstatus 

Überprüfen Sie die folgenden Felder, und stellen Sie sicher, dass sie die erwarteten Werte aufweisen:

#### <a name="domainjoined--yes"></a>DomainJoined: JA  

Dieses Feld gibt an, ob das Gerät in ein lokales Active Directory eingebunden ist. Wenn der Wert **NO** lautet, kann das Gerät keinen Azure AD-Hybridbeitritt durchführen.  

#### <a name="workplacejoined--no"></a>WorkplaceJoined: NO  

Dieses Feld gibt an, ob das Gerät bei Azure AD als privates Gerät registriert ist (markiert als *Workplace Join*). Dieser Wert sollte für in eine Domäne eingebundene Computer mit Hybrideinbindung in Azure AD **NO** lauten. Wenn der Wert **YES** lautet, wurde vor Abschluss der Hybrideinbindung in Azure AD ein Geschäfts-, Schul- oder Unikonto hinzugefügt. In diesem Fall wird das Konto ignoriert, wenn eine Version von Windows 10 (1607) mit Anniversary Update verwendet wird.

#### <a name="azureadjoined--yes"></a>AzureAdJoined: JA  

Dieses Feld gibt an, ob das Gerät in Azure AD eingebunden ist. Wenn der Wert **NO** lautet, wurde der Azure AD-Beitritt noch nicht abgeschlossen. 

Fahren Sie mit den nächsten Schritten zur weiteren Problembehandlung fort.

### <a name="step-3-find-the-phase-in-which-join-failed-and-the-errorcode"></a>Schritt 3: Finden Sie die Phase, in der die Verknüpfung fehlgeschlagen ist, und den Fehlercode

#### <a name="windows-10-1803-and-above"></a>Windows 10 1803 und höher

Suchen Sie im Abschnitt „Diagnosedaten“ der Verknüpfungsstatusausgabe nach dem Unterabschnitt „Vorherige Registrierung“.
Das Feld „Fehlerphase“ bezeichnet die Phase des Verknüpfungsfehlers, während „Client-Fehlercode“ den Fehlercode des Verknüpfungsvorgangs bezeichnet.

```
+----------------------------------------------------------------------+
     Previous Registration : 2019-01-31 09:16:43.000 UTC
         Registration Type : sync
               Error Phase : join
          Client ErrorCode : 0x801c03f2
          Server ErrorCode : DirectoryError
            Server Message : The device object by the given id (e92325d0-xxxx-xxxx-xxxx-94ae875d5245) is not found.
              Https Status : 400
                Request Id : 6bff0bd9-820b-484b-ab20-2a4f7b76c58e
+----------------------------------------------------------------------+
```

#### <a name="older-windows-10-versions"></a>Ältere Windows 10-Versionen

Verwenden Sie die Protokolle der Ereignisanzeige, um die Phase und den Fehlercode für die Verknüpfungsfehler zu ermitteln.

1. Öffnen Sie die Ereignisprotokolle der **Benutzergeräteregistrierung** in der Ereignisanzeige. Sie finden sie unter **Anwendungs- und Dienstprotokolle** > **Microsoft** > **Windows** > **Benutzergeräteregistrierung**
2. Suchen Sie nach Ereignissen mit den folgenden eventIDs: 304, 305, 307.

![Fehlerhaftes Protokollereignis](./media/troubleshoot-hybrid-join-windows-current/1.png)

![Fehlerhaftes Protokollereignis](./media/troubleshoot-hybrid-join-windows-current/2.png)

### <a name="step-4-check-for-possible-causes-and-resolutions-from-the-lists-below"></a>Schritt 4: Überprüfen Sie die folgenden Listen auf mögliche Ursachen und Lösungen

#### <a name="pre-check-phase"></a>Vorüberprüfungsphase

Mögliche Ursachen für Fehler:

- Das Gerät hat keine „Sichtverbindung“ mit dem Domänencontroller.
   - Das Gerät muss sich im internen Unternehmensnetzwerk oder im VPN mit direktem Zugriff auf einen lokalen AD-Domänencontroller befinden.

#### <a name="discover-phase"></a>Ermittlungsphase

Mögliche Ursachen für Fehler:

- Falsch konfiguriertes Objekt im Dienstverbindungspunkt. Dienstverbindungspunktobjekt kann nicht aus DC gelesen werden.
   - Ein gültiges SCP-Objekt ist in der AD-Gesamtstruktur erforderlich, zu der das Gerät gehört, das auf einen verifizierten Domänennamen in Azure AD verweist.
   - Weitere Informationen finden Sie im Abschnitt [„Konfigurieren eines Dienstverbindungspunkts“](hybrid-azuread-join-federated-domains.md#configure-hybrid-azure-ad-join).
- Es ist nicht möglich, eine Verbindung herzustellen und die Ermittlungsmetadaten vom Ermittlungsendpunkt abzurufen.
   - Das Gerät sollte im SYSTEM-Kontext auf die `https://enterpriseregistration.windows.net` zugreifen können, um die Endpunkte für Registrierung und Autorisierung zu ermitteln. 
   - Wenn die lokale Umgebung einen Proxy für den ausgehenden Datenverkehr erfordert, muss der IT-Administrator sicherstellen, dass das Computerkonto des Geräts in der Lage ist, den Proxy zu ermitteln und sich im Hintergrund zu authentifizieren.
- Die Verbindung zum Endpunkt des Benutzerbereichs und die Durchführung der Bereichsermittlung sind fehlgeschlagen. (Nur Windows 10, Version 1809 und höher)
   - Das Gerät sollte im SYSTEM-Kontext auf `https://login.microsoftonline.com` zugreifen können, um eine Bereichsermittlung für die verifizierte Domäne durchzuführen und den Domänentyp (verwaltet/Verbund) zu bestimmen.
   - Wenn die lokale Umgebung einen Proxy für den ausgehenden Datenverkehr erfordert, muss der IT-Administrator sicherstellen, dass der SYSTEM-Kontext auf dem Gerät in der Lage ist, den Proxy zu ermitteln und im Hintergrund zu authentifizieren.

**Häufige Fehlermeldungen:**

- **DSREG_AUTOJOIN_ADCONFIG_READ_FAILED** (0x801c001d/-2145648611)
   - Ursache: Es ist nicht möglich, das SCP-Objekt zu lesen und die Azure AD-Mandanteninformationen abzurufen.
   - Lösung: Weitere Informationen finden Sie im Abschnitt [„Konfigurieren eines Dienstverbindungspunkts“](hybrid-azuread-join-federated-domains.md#configure-hybrid-azure-ad-join).
- **DSREG_AUTOJOIN_DISC_FAILED** (0x801c0021/-2145648607)
   - Ursache: Allgemeiner Ermittlungsfehler. Die Ermittlungsmetadaten konnten nicht von DRS abgerufen werden.
   - Lösung: Finden Sie den Unterfehler weiter unten, um weitere Untersuchungen durchzuführen.
- **DSREG_AUTOJOIN_DISC_WAIT_TIMEOUT**  (0x801c001f/-2145648609)
   - Ursache: Der Vorgang wurde während der Ermittlung zeitlich begrenzt.
   - Lösung: Stellen Sie sicher, dass im SYSTEM-Kontext auf `https://enterpriseregistration.windows.net` zugegriffen werden kann. Weitere Informationen finden Sie im Abschnitt [“Netzwerkverbindungsanforderungen“](hybrid-azuread-join-managed-domains.md#prerequisites).
- **DSREG_AUTOJOIN_USERREALM_DISCOVERY_FAILED** (0x801c0021/-2145648611)
   - Ursache: Allgemeiner Fehler bei der Bereichsermittlung. Der Domänentyp (verwaltet/Verbund) konnte nicht aus STS ermittelt werden. 
   - Lösung: Finden Sie den Unterfehler weiter unten, um weitere Untersuchungen durchzuführen.

**Häufige Unterfehlercodes:**

Um den Unterfehlercode für den Ermittlungsfehlercode zu finden, verwenden Sie eine der folgenden Methoden.

##### <a name="windows-10-1803-and-above"></a>Windows 10 1803 und höher

Suchen Sie im Abschnitt „Diagnosedaten“ der Verknüpfungsstatusausgabe nach dem Unterabschnitt „DRS-Ermittlungstest“.

```
+----------------------------------------------------------------------+
| Diagnostic Data                                                      |
+----------------------------------------------------------------------+

     Diagnostics Reference : www.microsoft.com/aadjerrors
              User Context : UN-ELEVATED User
               Client Time : 2019-06-05 08:25:29.000 UTC
      AD Connectivity Test : PASS
     AD Configuration Test : PASS
        DRS Discovery Test : FAIL [0x801c0021/0x80072ee2]
     DRS Connectivity Test : SKIPPED
    Token acquisition Test : SKIPPED
     Fallback to Sync-Join : ENABLED

+----------------------------------------------------------------------+
```

##### <a name="older-windows-10-versions"></a>Ältere Windows 10-Versionen

Verwenden Sie die Protokolle der Ereignisanzeige, um die Phase und den Fehlercode für die Verknüpfungsfehler zu ermitteln.

1. Öffnen Sie die Ereignisprotokolle der **Benutzergeräteregistrierung** in der Ereignisanzeige. Sie finden sie unter **Anwendungs- und Dienstprotokolle** > **Microsoft** > **Windows** > **Benutzergeräteregistrierung**
2. Suchen Sie nach Ereignissen mit den folgenden eventIDs: 201

![Fehlerhaftes Protokollereignis](./media/troubleshoot-hybrid-join-windows-current/5.png)

###### <a name="network-errors"></a>Netzwerkfehler

- **WININET_E_CANNOT_CONNECT** (0x80072efd/-2147012867)
   - Ursache: Verbindung mit dem Server konnte nicht hergestellt werden
   - Lösung: Stellen Sie die Netzwerkkonnektivität zu den erforderlichen Microsoft-Ressourcen sicher. Weitere Informationen finden Sie unter [„Netzwerkverbindungsanforderungen“](hybrid-azuread-join-managed-domains.md#prerequisites).
- **WININET_E_TIMEOUT** (0x80072ee2/-2147012894)
   - Ursache: Allgemeines Netzwerktimeout.
   - Lösung: Stellen Sie die Netzwerkkonnektivität zu den erforderlichen Microsoft-Ressourcen sicher. Weitere Informationen finden Sie unter [„Netzwerkverbindungsanforderungen“](hybrid-azuread-join-managed-domains.md#prerequisites).
- **WININET_E_DECODING_FAILED** (0x80072f8f/-2147012721)
   - Ursache: Der Netzwerkstapel konnte die Antwort des Servers nicht entschlüsseln.
   - Lösung: Stellen Sie sicher, dass der Netzwerk-Proxy die Serverantwort nicht beeinträchtigt und ändert.

###### <a name="http-errors"></a>HTTP-Fehler

- **DSREG_DISCOVERY_TENANT_NOT_FOUND** (0x801c003a/-2145648582)
   - Ursache: SCP-Objekt mit falscher Mandanten-ID konfiguriert. Oder es wurden keine aktiven Abonnements im Mandanten gefunden.
   - Lösung: Stellen Sie sicher, dass das SCP-Objekt mit der korrekten Azure AD-Mandanten-ID und aktiven Abonnements konfiguriert ist oder im Mandanten vorhanden ist.
- **DSREG_SERVER_BUSY** (0x801c0025/-2145648603)
   - Ursache: HTTP 503 vom DRS-Server.
   - Lösung: Der Server ist zurzeit nicht verfügbar. Zukünftige Verknüpfungsversuche werden wahrscheinlich erfolgreich sein, sobald der Server wieder online ist.

###### <a name="other-errors"></a>Andere Fehler

- **E_INVALIDDATA** (0x8007000d/-2147024883)
   - Ursache: Der JSON-Code der Serverantwort konnte nicht analysiert werden. Dies liegt wahrscheinlich daran, dass der Proxy HTTP 200 mit einer HTML-Authentifizierungsseite zurückgibt.
   - Lösung: Wenn die lokale Umgebung einen Proxy für den ausgehenden Datenverkehr erfordert, muss der IT-Administrator sicherstellen, dass der SYSTEM-Kontext auf dem Gerät in der Lage ist, den Proxy zu ermitteln und im Hintergrund zu authentifizieren.

#### <a name="authentication-phase"></a>Authentifizierungsphase

Gilt nur für Verbund-Domänenkonten.

Ursachen für Fehler:

- Es ist nicht möglich, einen Zugriffstoken für die DRS-Ressource im Hintergrund zu erhalten.
   - Windows 10-Geräte erhalten Authentifizierungstoken vom Verbunddienst über die integrierte Windows-Authentifizierung zu einem aktiven WS-Trust Endpunkt. Details: [Verbundserverkonfiguration](hybrid-azuread-join-manual.md##set-up-issuance-of-claims)

**Häufige Fehlermeldungen:**

Verwenden Sie Ereignisanzeigeprotokolle, um den Fehlercode, den Unterfehlercode, den Serverfehlercode und die Serverfehlermeldung zu finden.

1. Öffnen Sie die Ereignisprotokolle der **Benutzergeräteregistrierung** in der Ereignisanzeige. Sie finden sie unter **Anwendungs- und Dienstprotokolle** > **Microsoft** > **Windows** > **Benutzergeräteregistrierung**
2. Suchen Sie nach Ereignissen mit den folgenden eventIDs: 305

![Fehlerhaftes Protokollereignis](./media/troubleshoot-hybrid-join-windows-current/3.png)

##### <a name="configuration-errors"></a>Konfigurationsfehler

- **ERROR_ADAL_PROTOCOL_NOT_SUPPORTED** (0xcaa90017/-894894057)
   - Ursache: Das Authentifizierungsprotokoll ist nicht WS-Trust.
   - Lösung: Der lokale Identitätsanbieter muss WS-Trust unterstützen 
- **ERROR_ADAL_FAILED_TO_PARSE_XML** (0xcaa9002c/-894894036)
   - Ursache: Der lokale Verbunddienst hat keine XML-Antwort zurückgegeben.
   - Lösung: Stellen Sie sicher, dass der MEX-Endpunkt ein gültiges XML zurückgibt. Stellen Sie sicher, dass der Proxy nicht beeinträchtigt und Antworten zurückgibt, die nicht in XML sind.
- **ERROR_ADAL_COULDNOT_DISCOVER_USERNAME_PASSWORD_ENDPOINT** (0xcaa90023/-894894045)
   - Ursache: Der Endpunkt für die Authentifizierung von Benutzernamen und Passwörtern konnte nicht ermittelt werden.
   - Lösung: Überprüfen Sie die Einstellungen des lokalen Identitätsanbieters. Stellen Sie sicher, dass die WS-Trust-Endpunkte aktiviert sind und dass die MEX-Antwort diese korrekten Endpunkte enthält.

##### <a name="network-errors"></a>Netzwerkfehler

- **ERROR_ADAL_INTERNET_TIMEOUT** (0xcaa82ee2/-894947614)
   - Ursache: Allgemeines Netzwerktimeout.
   - Lösung: Stellen Sie sicher, dass im SYSTEM-Kontext auf `https://login.microsoftonline.com` zugegriffen werden kann. Stellen Sie sicher, dass im SYSTEM-Kontext auf den lokalen Identitätsanbieter zugegriffen werden kann. Weitere Informationen finden Sie unter [„Netzwerkverbindungsanforderungen“](hybrid-azuread-join-managed-domains.md#prerequisites).
- **ERROR_ADAL_INTERNET_CONNECTION_ABORTED** (0xcaa82efe/-894947586)
   - Ursache: Die Verbindung mit dem Authentifizierungsendpunkt wurde abgebrochen.
   - Lösung: Versuchen Sie es nach einiger Zeit erneut oder versuchen Sie, über einen alternativen stabilen Netzwerkstandort zu verbinden.
- **ERROR_ADAL_INTERNET_SECURE_FAILURE** (0xcaa82f8f/-894947441)
   - Ursache: Das vom Server gesendete Secure Sockets Layer (SSL)-Zertifikat konnte nicht überprüft werden.
   - Lösung: Überprüfen Sie die Zeitabweichung des Clients. Versuchen Sie es nach einiger Zeit erneut oder versuchen Sie, über einen alternativen stabilen Netzwerkstandort zu verbinden. 
- **ERROR_ADAL_INTERNET_CANNOT_CONNECT** (0xcaa82efd/-894947587)
   - Ursache: Der Versuch, eine Verbindung mit `https://login.microsoftonline.com` herzustellen, ist fehlgeschlagen.
   - Lösung: Überprüfen Sie die Netzwerkverbindung mit `https://login.microsoftonline.com`.

##### <a name="other-errors"></a>Andere Fehler

- **ERROR_ADAL_SERVER_ERROR_INVALID_GRANT** (0xcaa20003/-895352829)
   - Ursache: Das SAML-Token vom lokalen Identitätsanbieter wurde von Azure AD nicht akzeptiert.
   - Lösung: Überprüfen Sie die Einstellungen des Verbundservers. Suchen Sie nach dem Serverfehlercode in den Authentifizierungsprotokollen.
- **ERROR_ADAL_WSTRUST_REQUEST_SECURITYTOKEN_FAILED** (0xcaa90014/-894894060)
   - Ursache: Die Serverantwort von WS-Trust meldet Fehlerausnahme und konnte keine Bewertung erhalten
   - Lösung: Überprüfen Sie die Einstellungen des Verbundservers. Suchen Sie nach dem Serverfehlercode in den Authentifizierungsprotokollen.
- **ERROR_ADAL_WSTRUST_TOKEN_REQUEST_FAIL** (0xcaa90006/-894894074)
   - Ursache: Beim Versuch, Zugriffstoken vom Tokenendpunkt aus abzurufen, wurde ein Fehler gemeldet.
   - Lösung: Suchen Sie im ADAL-Protokoll nach dem zugrunde liegenden Fehler. 
- **ERROR_ADAL_OPERATION_PENDING** (0xcaa1002d/-895418323)
   - Ursache: Allgemeiner Adal-Fehler
   - Lösung: Suchen Sie in den Authentifizierungsprotokollen nach dem Unterfehlercode oder dem Serverfehlercode.
    
#### <a name="join-phase"></a>Verbindungsphase

Ursachen für Fehler:

Suchen Sie den Registrierungstyp und suchen Sie nach dem Fehlercode in der untenstehenden Liste.

#### <a name="windows-10-1803-and-above"></a>Windows 10 1803 und höher

Suchen Sie im Abschnitt „Diagnosedaten“ der Verknüpfungsstatusausgabe nach dem Unterabschnitt „Vorherige Registrierung“.
Das Feld „Registrierungstyp“ gibt die Art der durchgeführten Verknüpfung an.

```
+----------------------------------------------------------------------+
     Previous Registration : 2019-01-31 09:16:43.000 UTC
         Registration Type : sync
               Error Phase : join
          Client ErrorCode : 0x801c03f2
          Server ErrorCode : DirectoryError
            Server Message : The device object by the given id (e92325d0-7ac4-4714-88a1-94ae875d5245) is not found.
              Https Status : 400
                Request Id : 6bff0bd9-820b-484b-ab20-2a4f7b76c58e
+----------------------------------------------------------------------+
```

#### <a name="older-windows-10-versions"></a>Ältere Windows 10-Versionen

Verwenden Sie die Protokolle der Ereignisanzeige, um die Phase und den Fehlercode für die Verknüpfungsfehler zu ermitteln.

1. Öffnen Sie die Ereignisprotokolle der **Benutzergeräteregistrierung** in der Ereignisanzeige. Sie finden sie unter **Anwendungs- und Dienstprotokolle** > **Microsoft** > **Windows** > **Benutzergeräteregistrierung**
2. Suchen Sie nach Ereignissen mit den folgenden eventIDs: 204

![Fehlerhaftes Protokollereignis](./media/troubleshoot-hybrid-join-windows-current/4.png)

##### <a name="http-errors-returned-from-drs-server"></a>Vom DRS-Server zurückgegebene HTTP-Fehler

- **DSREG_E_DIRECTORY_FAILURE** (0x801c03f2/-2145647630)
   - Ursache: Fehlerantwort wurde vom DRS mit ErrorCode zurückgegeben: „DirectoryError“
   - Lösung: Weitere Informationen zu möglichen Gründen und Lösungen finden Sie im Server-Fehlercode.
- **DSREG_E_DEVICE_AUTHENTICATION_ERROR** (0x801c0002/-2145648638)
   - Ursache: Fehlerantwort wurde vom DRS mit ErrorCode zurückgegeben: „AuthenticationError“ und ErrorSubCode ist NICHT „DeviceNotFound“. 
   - Lösung: Weitere Informationen zu möglichen Gründen und Lösungen finden Sie im Server-Fehlercode.
- **DSREG_E_DEVICE_INTERNALSERVICE_ERROR** (0x801c0006/-2145648634)
   - Ursache: Fehlerantwort wurde vom DRS mit ErrorCode zurückgegeben: „DirectoryError“
   - Lösung: Weitere Informationen zu möglichen Gründen und Lösungen finden Sie im Server-Fehlercode.

##### <a name="tpm-errors"></a>TPM-Fehler

- **NTE_BAD_KEYSET** (0x80090016/-2146893802)
   - Ursache: TPM-Vorgang ist fehlgeschlagen oder ungültig
   - Lösung: Wahrscheinlich aufgrund eines ungültigen Sysrep-Images. Stellen Sie sicher, dass der Computer, von dem aus das Sysprep-Image erstellt wurde, nicht mit Azure AD oder Hybrid-Azure AD verknüpft oder bei Azure AD registriert ist.
- **TPM_E_PCP_INTERNAL_ERROR** (0x80290407/-2144795641)
   - Ursache: Allgemeiner TPM-Fehler. 
   - Lösung: Deaktivieren Sie TPM auf Geräten mit diesem Fehler. Windows 10 Version 1809 und höher erkennt automatisch TPM-Fehler und schließt die Hybrid-Azure AD-Einbindung ohne Verwendung des TPM ab.
- **TPM_E_NOTFIPS** (0x80280036/-2144862154)
   - Ursache: TPM im FIPS-Modus wird derzeit nicht unterstützt.
   - Lösung: Deaktivieren Sie TPM auf Geräten mit diesem Fehler. Windows 1809 erkennt automatisch TPM-Fehler und schließt die Hybrid-Azure AD-Einbindung ohne Verwendung des TPM ab.
- **NTE_AUTHENTICATION_IGNORED** (0x80090031/-2146893775)
   - Ursache: TPM wurde gesperrt.
   - Lösung: Transient error. (Vorübergehender Fehler.) Warten Sie die Abkühldauer ab. Der Verknüpfungsversuch sollte nach einiger Zeit erfolgreich sein. Weitere Informationen finden Sie im Artikel [TPM-Grundlagen](https://docs.microsoft.com/windows/security/information-protection/tpm/tpm-fundamentals#anti-hammering)

##### <a name="network-errors"></a>Netzwerkfehler

- **WININET_E_TIMEOUT** (0x80072ee2/-2147012894)
   - Ursache: Allgemeiner Netzwerk-Timeout beim Versuch, das Gerät bei DRS zu registrieren
   - Lösung: Überprüfen Sie die Netzwerkverbindung zu `https://enterpriseregistration.windows.net`.
- **WININET_E_NAME_NOT_RESOLVED** (0x80072ee7/-2147012889)
   - Ursache: Der Servername oder die Adresse konnte nicht aufgelöst werden.
   - Lösung: Überprüfen Sie die Netzwerkverbindung zu `https://enterpriseregistration.windows.net`. Stellen Sie sicher, dass die DNS-Auflösung für den Hostnamen in der NW und auf dem Gerät korrekt ist.
- **WININET_E_CONNECTION_ABORTED** (0x80072efe/-2147012866)
   - Ursache: Die Verbindung mit dem Server wurde vorzeitig beendet.
   - Lösung: Versuchen Sie es nach einiger Zeit erneut oder versuchen Sie, über einen alternativen stabilen Netzwerkstandort zu verbinden.

##### <a name="federated-join-server-errors"></a>Fehler beim verknüpften Verbundserver

| Serverfehlercode | Serverfehlermeldung | Mögliche Ursachen | Lösung |
| --- | --- | --- | --- |
| DirectoryError | Ihre Anforderung wird vorübergehend gedrosselt. Versuchen Sie es nach 300 Sekunden erneut. | Erwarteter Fehler. Möglicherweise aufgrund von mehreren Registrierungsanfragen in schneller Folge. | Wiederholen der Verknüpfung nach der Abkühlzeit |

##### <a name="sync-join-server-errors"></a>Fehler bei „sync join“

| Serverfehlercode | Serverfehlermeldung | Mögliche Ursachen | Lösung |
| --- | --- | --- | --- |
| DirectoryError | AADSTS90002: Mandant <UUID> wurde nicht gefunden. Dieser Fehler kann auftreten, wenn es für den Mandanten keine aktiven Abonnements gibt. Wenden Sie sich an Ihren Abonnement-Administrator. | Mandanten-ID im SCP-Objekt ist falsch | Stellen Sie sicher, dass das SCP-Objekt mit der korrekten Azure AD-Mandanten-ID und aktiven Abonnements konfiguriert ist und im Mandanten vorhanden ist. |
| DirectoryError | Das Geräteobjekt mit der angegebenen ID wird nicht gefunden. | Erwarteter Fehler bei „sync join“. Das Geräteobjekt wurde nicht von AD zu Azure AD synchronisiert | Warten Sie, bis die Azure AD Connect-Synchronisierung abgeschlossen ist und der nächste Verknüpfungsversuch nach Abschluss der Synchronisierung das Problem löst |
| AuthenticationError | Die Überprüfung der SID des Zielcomputers | Das Zertifikat auf dem Azure AD-Gerät stimmt nicht mit dem Zertifikat überein, mit dem der Blob während „sync join“ signiert wurde. Dieser Fehler bedeutet in der Regel, dass die Synchronisierung noch nicht abgeschlossen ist. |  Warten Sie, bis die Azure AD Connect-Synchronisierung abgeschlossen ist und der nächste Verknüpfungsversuch nach Abschluss der Synchronisierung das Problem löst |

### <a name="step-5-collect-logs-and-contact-microsoft-support"></a>Schritt 5: Sammeln Sie Protokolle und wenden Sie sich an den Microsoft-Support

Hier erhalten Sie öffentliche Skripts: [https://1drv.ms/u/s!AkyTjQ17vtfagYkZ6VJzPg78e3o7PQ]( https://1drv.ms/u/s!AkyTjQ17vtfagYkZ6VJzPg78e3o7PQ)

1. Öffnen Sie eine Admin-Eingabeaufforderung und führen Sie `start_ngc_tracing_public.cmd` aus.
2. Führen Sie die Schritte zum Reproduzieren des Problems aus.
3. Stoppen Sie die Ausführung des Protokollierungsskripts, indem Sie `stop_ngc_tracing_public.cmd` ausführen.
4. Zippen und senden Sie die Protokolle unter `%SYSTEMDRIVE%\TraceDJPP\*` zur Analyse ein.

## <a name="troubleshoot-post-join-issues"></a>Beheben von Problemen nach der Verknüpfung

### <a name="retrieve-the-join-status"></a>Abrufen des Beitrittsstatus 

#### <a name="wamdefaultset-yes-and-azureadprt-yes"></a>WamDefaultSet: YES und AzureADPrt: JA
  
Diese Felder geben an, ob der Benutzer sich bei der Anmeldung beim Gerät erfolgreich bei Azure AD authentifiziert hat. Wenn die Werte **NO** lauten, kann dies die folgenden Ursachen haben:

- Dem Gerät wurde bei der Registrierung ein ungültiger Speicherschlüssel im TPM zugewiesen (prüfen Sie den KeySignTest bei der Ausführung mit erhöhten Rechten).
- Alternative Anmelde-ID.
- HTTP-Proxy nicht gefunden.

## <a name="next-steps"></a>Nächste Schritte

Fahren Sie mit [„Problembehandlung von Geräten mit dem Befehl „dsregcmd““](troubleshoot-device-dsregcmd.md) fort

Bei Fragen lesen Sie unter [Häufig gestellte Fragen zur Geräteverwaltung](faq.md) nach.
