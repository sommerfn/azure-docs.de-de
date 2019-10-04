---
title: Problembehandlung von Geräten mit dem Befehl „dsregcmd“ – Azure Active Directory
description: Verwendung der Ausgabe von „dsregcmd“, um den Zustand von Geräten in Azure AD zu verstehen
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: troubleshooting
ms.date: 07/10/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: spunukol
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4aa8f9a7c6807a2f9505559ea13fb0b4f410346d
ms.sourcegitcommit: 5b76581fa8b5eaebcb06d7604a40672e7b557348
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2019
ms.locfileid: "68987168"
---
# <a name="troubleshooting-devices-using-the-dsregcmd-command"></a>Problembehandlung von Geräten mit dem Befehl „dsregcmd“

Das Hilfsprogramm „dsregcmd/status“ muss als Domänenbenutzerkonto ausgeführt werden.

## <a name="device-state"></a>Gerätestatus

In diesem Abschnitt sind die Statusparameter für das eingebundene Gerät aufgelistet. Die folgende Tabelle listet die Kriterien für das Gerät auf, nach denen sich das Gerät in verschiedenen Verbindungsstatus befindet.

| AzureAdJoined | EnterpriseJoined | DomainJoined | Gerätestatus |
| ---   | ---   | ---   | ---   |
| JA | NO | NO | Azure AD Joined |
| NO | NO | JA | Domain Joined |
| JA | NO | JA | Hybrid AD Joined |
| NO | JA | JA | On-premises DRS Joined |

> [!NOTE]
> Der Status „Workplace Join (Azure AD registered)“ wird im Abschnitt „Benutzerzustand“ angezeigt.

- **AzureAdJoined:** – Auf „YES“ festlegen, wenn das Gerät mit Azure AD verknüpft ist. Wählen Sie andernfalls „NO“.
- **EnterpriseJoined:** – Auf „YES“ festlegen, wenn das Gerät mit einem lokalen DRS verknüpft ist. Ein Gerät kann nicht gleichzeitig EnterpriseJoined und AzureAdJoined sein.
- **DomainJoined:** – Auf „YES“ festlegen, wenn das Gerät mit einer Domäne (AD) verknüpft ist.
- **DomainName:** – Auf den Namen der Domain festlegen, wenn das Gerät mit einer Domain verknüpft ist.

### <a name="sample-device-state-output"></a>Beispielausgabe für den Gerätestatus

```
+----------------------------------------------------------------------+
| Device State                                                         |
+----------------------------------------------------------------------+
             AzureAdJoined : YES
          EnterpriseJoined : NO
              DomainJoined : YES
                DomainName : HYBRIDADFS
+----------------------------------------------------------------------+
```

## <a name="device-details"></a>Gerätedetails

Wird nur angezeigt, wenn das Gerät mit Azure AD oder Hybrid-Azure AD verknüpft ist (nicht Azure AD registriert). In diesem Abschnitt werden die in der Cloud gespeicherten Details zur Identifizierung des Geräts aufgelistet.

- **DeviceId:** – Eindeutige ID des Geräts im Azure AD-Mandanten
- **Thumbprint:** – Fingerabdruck des Gerätezertifikats 
- **DeviceCertificateValidity:** – Gültigkeitsdauer des Gerätezertifikats
- **KeyContainerId:** – ContainerId des privaten Schlüssels des Geräts, der dem Gerätezertifikat zugeordnet ist
- **KeyProvider:** – KeyProvider (Hardware/Software) zum Speichern des privaten Schlüssels des Geräts.
- **TpmProtected:** – „YES“, wenn der private Schlüssel des Geräts in einem Hardware-TPM gespeichert ist.

### <a name="sample-device-details-output"></a>Beispielausgabe für Gerätedetails

```
+----------------------------------------------------------------------+
| Device Details                                                       |
+----------------------------------------------------------------------+

                  DeviceId : e92325d0-xxxx-xxxx-xxxx-94ae875dxxxx
                Thumbprint : D293213EF327483560EED8410CAE36BB67208179
 DeviceCertificateValidity : [ 2019-01-11 21:02:50.000 UTC -- 2029-01-11 21:32:50.000 UTC ]
            KeyContainerId : 13e68a58-xxxx-xxxx-xxxx-a20a2411xxxx
               KeyProvider : Microsoft Software Key Storage Provider
              TpmProtected : NO
+----------------------------------------------------------------------+
```

## <a name="tenant-details"></a>Mandantendetails

Wird nur angezeigt, wenn das Gerät mit Azure AD oder Hybrid-Azure AD verknüpft ist (nicht Azure AD registriert). In diesem Abschnitt werden die allgemeinen Mandantendaten aufgeführt, wenn ein Gerät mit Azure AD verknüpft ist.

> [!NOTE]
> Selbst wenn Sie MDM-URLs sehen, bedeutet dies nicht, dass das Gerät von einer MDM verwaltet wird. Die Informationen werden angezeigt, wenn der Mandant über eine MDM-Konfiguration für die automatische Registrierung verfügt, auch wenn das Gerät selbst nicht verwaltet wird. 

### <a name="sample-tenant-details-output"></a>Beispielsausgabe der Mandantendetails

```
+----------------------------------------------------------------------+
| Tenant Details                                                       |
+----------------------------------------------------------------------+

                TenantName : HybridADFS
                  TenantId : 96fa76d0-xxxx-xxxx-xxxx-eb60cc22xxxx
                       Idp : login.windows.net
               AuthCodeUrl : https://login.microsoftonline.com/96fa76d0-xxxx-xxxx-xxxx-eb60cc22xxxx/oauth2/authorize
            AccessTokenUrl : https://login.microsoftonline.com/96fa76d0-xxxx-xxxx-xxxx-eb60cc22xxxx/oauth2/token
                    MdmUrl : https://enrollment.manage-beta.microsoft.com/EnrollmentServer/Discovery.svc
                 MdmTouUrl : https://portal.manage-beta.microsoft.com/TermsOfUse.aspx
          MdmComplianceUrl : https://portal.manage-beta.microsoft.com/?portalAction=Compliance
               SettingsUrl : eyJVxxxxIjpbImh0dHBzOi8va2FpbGFuaS5vbmUubWljcm9zb2Z0LmNvbS8iLCJodHRwczovL2thaWxhbmkxLm9uZS5taWNyb3NvZnQuY29tLyxxxx==
            JoinSrvVersion : 1.0
                JoinSrvUrl : https://enterpriseregistration.windows.net/EnrollmentServer/device/
                 JoinSrvId : urn:ms-drs:enterpriseregistration.windows.net
             KeySrvVersion : 1.0
                 KeySrvUrl : https://enterpriseregistration.windows.net/EnrollmentServer/key/
                  KeySrvId : urn:ms-drs:enterpriseregistration.windows.net
        WebAuthNSrvVersion : 1.0
            WebAuthNSrvUrl : https://enterpriseregistration.windows.net/webauthn/96fa76d0-xxxx-xxxx-xxxx-eb60cc22xxxx/
             WebAuthNSrvId : urn:ms-drs:enterpriseregistration.windows.net
    DeviceManagementSrvVer : 1.0
    DeviceManagementSrvUrl : https://enterpriseregistration.windows.net/manage/96fa76d0-xxxx-xxxx-xxxx-eb60cc22xxxx/
     DeviceManagementSrvId : urn:ms-drs:enterpriseregistration.windows.net
+----------------------------------------------------------------------+
```

## <a name="user-state"></a>Benutzerzustand

In diesem Abschnitt wird der Status verschiedener Attribute für den Benutzer, der gerade am Gerät angemeldet ist, aufgelistet.

> [!NOTE]
> Der Befehl muss in einem Benutzerkontext ausgeführt werden, um einen gültigen Status abzurufen.

- **NgcSet:** – Auf „YES“ festlegen, wenn für den aktuell angemeldeten Benutzer ein Windows Hello-Schlüssel eingerichtet ist.
- **NgcKeyId:** – ID des Windows Hello-Schlüssels, wenn für den aktuell angemeldeten Benutzer einer eingerichtet ist.
- **CanReset:** – Gibt an, ob der Windows Hellp-Schlüssel vom Benutzer zurückgesetzt werden kann. 
- **Mögliche Werte:** – „DestructiveOnly“, „NonDestructiveOnly“, „DestructiveAndNonDestructive“ oder „Unknown“, wenn ein Fehler auftritt. 
- **WorkplaceJoined:** – Auf „YES“ festsetzen, wenn Konten, die bei Azure AD registriert sind, dem Gerät im aktuellen NTUSER-Kontext hinzugefügt wurden.
- **WamDefaultSet:** – Auf „YES“ festsetzen, wenn ein Standardwebkonto für den angemeldeten Benutzer beim WAM erstellt wird. Dieses Feld könnte einen Fehler anzeigen, wenn „dsreg /status“ im Admin-Kontext ausgeführt wird. 
- **WamDefaultAuthority:** – Für Azure AD auf „Organisation“ festsetzen.
- **WamDefaultId:** – Für Azure AD immer „https://login.microsoft.com“.
- **WamDefaultGUID:** – Die GUID des WAM-Anbieters (Azure AD/Microsoft-Konto) für das Standard-WAM-Webkonto. 

### <a name="sample-user-state-output"></a>Beispielsausgabe des Benutzerstatus

```
+----------------------------------------------------------------------+
| User State                                                           |
+----------------------------------------------------------------------+

                    NgcSet : YES
                  NgcKeyId : {FA0DB076-A5D7-4844-82D8-50A2FB42EC7B}
                  CanReset : DestructiveAndNonDestructive
           WorkplaceJoined : NO
             WamDefaultSet : YES
       WamDefaultAuthority : organizations
              WamDefaultId : https://login.microsoft.com
            WamDefaultGUID : { B16898C6-A148-4967-9171-64D755DA8520 } (AzureAd)

+----------------------------------------------------------------------+
```

## <a name="sso-state"></a>SSO-Status

Dieser Abschnitt kann für Geräte, die bei Azure AD registriert sind, ignoriert werden.

> [!NOTE]
> Der Befehl muss in einem Benutzerkontext ausgeführt werden, um einen gültigen Status für den Benutzer abzurufen.

- **AzureAdPrt:** – Auf „YES“ festgesetzt, wenn sich auf dem Gerät ein PRT für den angemeldeten Benutzer befindet.
- **AzureAdPrtUpdateTime:** – Auf die Zeit (UTC) festgesetzt, zu der das PRT das letzte Mal aktualisiert wurde.
- **AzureAdPrtExpiryTime:** – Auf die Zeit (UTC) festgesetzt, wenn das PRT abläuft, wenn es nicht erneuert wird.
- **AzureAdPrtAuthority:** – Autoritäts-URL von Azure AD
- **EnterprisePrt:** – Auf „YES“ festgesetzt, wenn das Gerät über PRT von lokalen ADFS verfügt. Für in Azure AD eingebundene Hybridgeräte könnte das Gerät gleichzeitig über PRT von Azure AD und lokalem AD verfügen. Lokal verknüpfte Geräte verfügen nur über ein Enterprise-PRT.
- **EnterpriseAdPrtUpdateTime:** – Auf die Zeit (UTC) festgesetzt, zu der das Enterprise-PRT das letzte Mal aktualisiert wurde.
- **EnterpriseAdPrtExpiryTime:** – Auf die Zeit (UTC) festgesetzt, wenn das PRT abläuft, wenn es nicht erneuert wird.
- **EnterprisePrtAuthority:** – Autoritäts-URL von ADFS

### <a name="sample-sso-state-output"></a>Beispielsausgabe des SSO-Status

```
+----------------------------------------------------------------------+
| SSO State                                                            |
+----------------------------------------------------------------------+

                AzureAdPrt : YES
      AzureAdPrtUpdateTime : 2019-01-24 19:15:26.000 UTC
      AzureAdPrtExpiryTime : 2019-02-07 19:15:26.000 UTC
       AzureAdPrtAuthority : https://login.microsoftonline.com/96fa76d0-xxxx-xxxx-xxxx-eb60cc22xxxx
             EnterprisePrt : YES
   EnterprisePrtUpdateTime : 2019-01-24 19:15:33.000 UTC
   EnterprisePrtExpiryTime : 2019-02-07 19:15:33.000 UTC
    EnterprisePrtAuthority : https://fs.hybridadfs.nttest.microsoft.com:443/adfs

+----------------------------------------------------------------------+
```

## <a name="diagnostic-data"></a>Diagnosedaten

### <a name="pre-join-diagnostics"></a>Diagnose vor Verknüpfen

Dieser Abschnitt wird nur angezeigt, wenn das Gerät eine Domänenverknüpfung hat und nicht in der Lage ist, die Azure AD-Verbindung zu hybridisieren.

In diesem Abschnitt werden verschiedene Tests durchgeführt, um die Diagnose von Verknüpfungsfehlern zu erleichtern. Dieser Abschnitt enthält auch die Details des vorherigen (?). Diese Informationen umfassen die Fehlerphase, den Fehlercode, die Serveranforderungs-ID, den Serverantwort-HTTP-Status und die Serverantwort-Fehlermeldung.

- **User Context:** – Der Kontext, in dem die Diagnose ausgeführt wird. Mögliche Werte: „SYSTEM“, „UN-ELEVATED“-Benutzer, „ELEVATED“-Benutzer. 

   > [!NOTE]
   > Da die eigentliche Verknüpfung im SYSTEM-Kontext durchgeführt wird, ist die Ausführung der Diagnose im SYSTEM-Kontext dem tatsächlichen Verknüpfungsszenario am nächsten. Um die Diagnose im SYSTEM-Kontext auszuführen, muss der Befehl „dsregcmd /status“ von einer Eingabeaufforderung mit erhöhten Rechten ausgeführt werden.

- **Client Time:** – Die Systemzeit in UTC.
- **AD Connectivity Test:** – Der Test prüft die Verbindung zum Domänencontroller. Ein Fehler in diesem Test wird wahrscheinlich zu Verknüpfungsfehlern in der Vorüberprüfungsphase führen.
- **AD Configuration Test:** – Der Test liest und überprüft, ob das SCP-Objekt in der lokalen AD-Gesamtstruktur korrekt konfiguriert ist. Fehler in diesem Test würden wahrscheinlich zu Verknüpfungsfehlern mit dem Fehlercode 0x801c001d in der Ermittlungsphase führen.
- **DRS Discovery Test:** – Der Test ruft die DRS-Endpunkte vom Endpunkt der Ermittlungsmetadaten ab und führt eine Benutzerbereichsanforderung durch. Fehler in diesem Test würden wahrscheinlich zu Verknüpfungsfehlern in der Ermittlungsphase führen.
- **DRS Connectivity Test:** – Der Test führt grundlegende Verbindungstests zum DRS-Endpunkt durch.
- **Token Acquisition Test:** – Der Test versucht, einen Azure AD-Authentifizierungstoken abzurufen, wenn der Benutzermandant zusammengefasst wird. Fehler in diesem Test würden wahrscheinlich zu Verknüpfungsfehlern in der Authentifizierungsphase führen. Wenn „auth“ fehlschlägt, wird „sync join“ als Fallback versucht, es sei denn, das Fallback wird explizit mit einem Registrierungsschlüssel deaktiviert.
- **Fallback to Sync-Join:** – Auf „Enabled“ festgesetzt, wenn der Registrierungsschlüssel NICHT vorhanden ist, um zu verhindern, dass der Fallback to „sync join“ mit Authentisierungsfehlern stattfindet. Diese Option ist ab Windows 10 1803 verfügbar.
- **Previous Registration:** – Zeitpunkt des letzten Verknüpfungsversuchs. Es werden nur fehlgeschlagene Verknüpfungsversuche protokolliert.
- **Error Phase:** – Die Verknüpfensphase, in der es zum Abbruch gekommen ist. Mögliche Werte sind „pre-check“, „discover“, „auth“ und „join“.
- **Client ErrorCode:** – Client-Fehlercode zurückgegeben (HRESULT).
- **Server ErrorCode:** – Serverfehlercode, wenn eine Anforderung an den Server gesendet wurde und der Server mit einem Fehlercode geantwortet hat. 
- **Server Message:** – Servernachricht, die zusammen mit dem Fehlercode zurückgegeben wird.
- **Https Status:** – Http-Status, der vom Server zurückgegeben wird.
- **Request ID:** – Die „requestId“ vom Client, die an den Server geschickt wird. Nützlich für das Korrelieren mit serverseitigen Protokollen.

### <a name="sample-pre-join-diagnostics-output"></a>Beispielsausgabe der Diagnose vor dem Verknüpfen

Das folgende Beispiel zeigt, wie der Diagnosetest mit einem Ermittlungsfehler fehlschlägt.

```
+----------------------------------------------------------------------+
| Diagnostic Data                                                      |
+----------------------------------------------------------------------+

     Diagnostics Reference : www.microsoft.com/aadjerrors
              User Context : SYSTEM
               Client Time : 2019-01-31 09:25:31.000 UTC
      AD Connectivity Test : PASS
     AD Configuration Test : PASS
        DRS Discovery Test : FAIL [0x801c0021/0x801c000c]
     DRS Connectivity Test : SKIPPED
    Token acquisition Test : SKIPPED
     Fallback to Sync-Join : ENABLED

     Previous Registration : 2019-01-31 09:23:30.000 UTC
               Error Phase : discover
          Client ErrorCode : 0x801c0021

+----------------------------------------------------------------------+
```

Das folgende Beispiel zeigt, dass Diagnosetests durchgeführt werden, aber der Registrierungsversuch mit einem Verzeichnisfehler fehlgeschlagen ist, der für „sync join“ erwartet wird. Sobald der Synchronisierungsauftrag von Azure AD Connect abgeschlossen ist, kann sich das Gerät verknüpfen.

```
+----------------------------------------------------------------------+
| Diagnostic Data                                                      |
+----------------------------------------------------------------------+

     Diagnostics Reference : www.microsoft.com/aadjerrors
              User Context : SYSTEM
               Client Time : 2019-01-31 09:16:50.000 UTC
      AD Connectivity Test : PASS
     AD Configuration Test : PASS
        DRS Discovery Test : PASS
     DRS Connectivity Test : PASS
    Token acquisition Test : PASS
     Fallback to Sync-Join : ENABLED

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

### <a name="post-join-diagnostics"></a>Diagnose nach der Verknüpfung

In diesem Abschnitt wird die Ausgabe von Integritätsprüfungen angezeigt, die an einem Gerät durchgeführt wurden, das mit der Cloud verbunden ist.

- **AadRecoveryEnabled:** – Wenn es „YES“ lautet, sind die im Gerät gespeicherten Schlüssel nicht verwendbar und das Gerät ist zur Wiederherstellung vorgemerkt. Die nächste Anmeldung löst die Wiederherstellung aus und führt zu einer erneuten Registrierung des Geräts.
- **KeySignTest:** – Wenn es „PASSED“ lautet, sind die Geräteschlüssel in gutem Zustand. Wenn „KeySignTest“ fehlschlägt, wird das Gerät in der Regel zur Wiederherstellung vorgemerkt. Die nächste Anmeldung löst die Wiederherstellung aus und führt zu einer erneuten Registrierung des Geräts. Bei Geräten, die mit Hybrid-Azure AD verbunden sind, erfolgt die Wiederherstellung unbeaufsichtigt. Geräte, die mit Azure AD verknüpft oder bei Azure AD registriert sind, zeigen eine Aufforderung zur Benutzerauthentifizierung an, um das Gerät wiederherzustellen und gegebenenfalls neu zu registrieren. **Für KeySignTest sind erhöhte Rechte erforderlich.**

#### <a name="sample-post-join-diagnostics-output"></a>Beispielsausgabe der Diagnose nach dem Verknüpfen

```
+----------------------------------------------------------------------+
| Diagnostic Data                                                      |
+----------------------------------------------------------------------+

         AadRecoveryEnabled: NO
               KeySignTest : PASSED
+----------------------------------------------------------------------+
```

## <a name="ngc-prerequisite-check"></a>Voraussetzungsprüfung für NGC

Dieser Abschnitt führt die erforderlichen Vorabüberprüfungen für die Bereitstellung eines NGC-Schlüssels durch. 

> [!NOTE]
> Sie sehen möglicherweise keine Details zur Überprüfung der NGC-Voraussetzung im Status „dsregcmd /“, wenn der Benutzer bereits erfolgreich NGC-Anmeldeinformationen konfiguriert hat.

### <a name="sample-ngc-prerequisite-check-output"></a>Beispielsausgabe der Überprüfung der NGC-Voraussetzung

```
+----------------------------------------------------------------------+
| Ngc Prerequisite Check                                               |
+----------------------------------------------------------------------+

            IsDeviceJoined : YES
             IsUserAzureAD : YES
             PolicyEnabled : YES
          PostLogonEnabled : YES
            DeviceEligible : YES
        SessionIsNotRemote : YES
            CertEnrollment : enrollment authority
          AdfsRefreshToken : YES
             AdfsRaIsReady : YES
    LogonCertTemplateReady : YES ( StateReady )
              PreReqResult : WillProvision
+----------------------------------------------------------------------+
```

## <a name="next-steps"></a>Nächste Schritte

Bei Fragen lesen Sie unter [Häufig gestellte Fragen zur Geräteverwaltung](faq.md) nach.
