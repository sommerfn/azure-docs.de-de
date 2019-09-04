---
title: Das Azure Active Directory-Anmeldeprotokollschema in Azure Monitor | Microsoft-Dokumentation
description: Informationen zum Azure AD-Anmeldeprotokollschema für die Verwendung in Azure Monitor
services: active-directory
documentationcenter: ''
author: cawrites
manager: daveba
editor: ''
ms.assetid: 4b18127b-d1d0-4bdc-8f9c-6a4c991c5f75
ms.service: active-directory
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 04/18/2019
ms.author: chadam
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4cc07ac2644ac9f97146e980a1961b9b84e7c561
ms.sourcegitcommit: 07700392dd52071f31f0571ec847925e467d6795
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2019
ms.locfileid: "70127044"
---
# <a name="interpret-the-azure-ad-sign-in-logs-schema-in-azure-monitor"></a>Interpretieren des Azure AD-Anmeldeprotokollschemas in Azure Monitor

Dieser Artikel beschreibt das Azure Active Directory-Anmeldeprotokollschema (Azure AD) in Azure Monitor. Die meisten Informationen zu Anmeldungen werden unter dem *Properties*-Attribut des `records`-Objekts bereitgestellt.


```json
{ 
    "time": "2019-03-12T16:02:15.5522137Z", 
    "resourceId": "/tenants/<TENANT ID>/providers/Microsoft.aadiam",
    "operationName": "Sign-in activity", 
    "operationVersion": "1.0", 
    "category": "SignInLogs", 
    "tenantId": "<TENANT ID>", 
    "resultType": "50140", 
    "resultSignature": "None", 
    "resultDescription": "This error occurred due to 'Keep me signed in' interrupt when the user was signing-in.", 
    "durationMs": 0, 
    "callerIpAddress": "<CALLER IP ADDRESS>", 
    "correlationId": "a75a10bd-c126-486b-9742-c03110d36262", 
    "identity": "Timothy Perkins", 
    "Level": 4, 
    "location": "US", 
    "properties": 
        {
            "id":"0231f922-93fa-4005-bb11-b344eca03c01",
            "createdDateTime":"2019-03-12T16:02:15.5522137+00:00",
            "userDisplayName":"Timothy Perkins",
            "userPrincipalName":"<USER PRINCIPAL NAME>",
            "userId":"<USER ID>",
            "appId":"<APPLICATION ID>",
            "appDisplayName":"Azure Portal",
            "ipAddress":"<IP ADDRESS>",
            "status":
            {
                "errorCode":50140,
                "failureReason":"This error occurred due to 'Keep me signed in' interrupt when the user was signing-in."
            },
            "clientAppUsed":"Browser",
            "deviceDetail":
            {
                "operatingSystem":"Windows 10",
                "browser":"Chrome 72.0.3626"
            },
            "location":
                {
                    "city":"Bellevue",
                    "state":"Washington",
                    "countryOrRegion":"US",
                    "geoCoordinates":
                    {
                        "latitude":45,
                        "longitude":122
                    }
                },
            "correlationId":"a75a10bd-c126-486b-9742-c03110d36262",
            "conditionalAccessStatus":"notApplied",
            "appliedConditionalAccessPolicies":
            [
                {
                    "id":"ae11ffaa-9879-44e0-972c-7538fd5c4d1a",
                    "displayName":"Hr app access policy",
                    "enforcedGrantControls":
                    [
                        "Mfa"
                    ],
                    "enforcedSessionControls":
                    [
                    ],
                    "result":"notApplied"
                },
                {
                    "id":"b915a70b-2eee-47b6-85b6-ff4f4a66256d",
                    "displayName":"MFA for all but global support access",
                    "enforcedGrantControls":[],
                    "enforcedSessionControls":[],
                    "result":"notEnabled"
                },
                {
                    "id":"830f27fa-67a8-461f-8791-635b7225caf1",
                    "displayName":"Header Based Application Control",
                    "enforcedGrantControls":["Mfa"],
                    "enforcedSessionControls":[],
                    "result":"notApplied"
                },
                {
                    "id":"8ed8d7f7-0a2e-437b-b512-9e47bed562e6",
                    "displayName":"MFA for everyones",
                    "enforcedGrantControls":[],
                    "enforcedSessionControls":[],
                    "result":"notEnabled"
                },
                {
                    "id":"52924e0f-798b-4afd-8c42-49055c7d6395",
                    "displayName":"Device compliant",
                    "enforcedGrantControls":[],
                    "enforcedSessionControls":[],
                    "result":"notEnabled"
                },
             ],
            "isInteractive":true,
            "tokenIssuerType":"AzureAD",
            "authenticationProcessingDetails":[],
            "networkLocationDetails":[],
            "processingTimeInMilliseconds":0,
            "riskDetail":"hidden",
            "riskLevelAggregated":"hidden",
            "riskLevelDuringSignIn":"hidden",
            "riskState":"none",
            "riskEventTypes":[],
            "resourceDisplayName":"windows azure service management api",
            "resourceId":"797f4846-ba00-4fd7-ba43-dac1f8f63013",
            "authenticationMethodsUsed":[]
        }
}
```


## <a name="field-descriptions"></a>Feldbeschreibungen

| Feldname | BESCHREIBUNG |
|------------|-------------|
| Time | Das Datum und die Uhrzeit in UTC. |
| resourceId | Dieser Wert wird nicht zugeordnet, sodass das Feld ignoriert werden kann.  |
| OperationName | Für Anmeldungen ist dieser Wert immer *Anmeldeaktivität*. |
| OperationVersion | Die vom Client angeforderte REST-API-Version. |
| Category (Kategorie) | Für Anmeldungen ist dieser Wert immer *SignIn*. | 
| TenantId | Die mit den Protokollen verknüpfte Mandanten-GUID. |
| ResultType | Das Ergebnis des Anmeldevorgangs ist *Erfolgreich* oder *Fehler*. | 
| ResultSignature | Gibt den Fehlercode (falls vorhanden) des Anmeldevorgangs an. |
| ResultDescription | Gibt die Fehlerbeschreibung des Anmeldevorgangs an. |
| riskDetail | riskDetail | Gibt den „Grund“ für einen bestimmten Zustand eines riskanten Benutzers, einer Anmeldung oder einer Risikoerkennung an. Die möglichen Werte sind: `none`, `adminGeneratedTemporaryPassword`, `userPerformedSecuredPasswordChange`, `userPerformedSecuredPasswordReset`, `adminConfirmedSigninSafe`, `aiConfirmedSigninSafe`, `userPassedMFADrivenByRiskBasedPolicy`, `adminDismissedAllRiskForUser`, `adminConfirmedSigninCompromised`, `unknownFutureValue`. Der Wert `none` bedeutet, dass für den Benutzer bzw. die Anmeldung bisher keine Aktion ausgeführt wurde. <br>**Hinweis:** Die Details für diese Eigenschaft erfordern eine Azure AD Premium P2-Lizenz. Andere Lizenzen geben den Wert `hidden` zurück. |
| riskEventTypes | riskEventTypes | Der der Anmeldung zugeordnete Risikoerkennungstyp. Die möglichen Werte sind: `unlikelyTravel`, `anonymizedIPAddress`, `maliciousIPAddress`, `unfamiliarFeatures`, `malwareInfectedIPAddress`, `suspiciousIPAddress`, `leakedCredentials`, `investigationsThreatIntelligence`, `generic` und `unknownFutureValue`. |
| riskLevelAggregated | riskLevel | Aggregierte Risikostufe. Die möglichen Werte sind: `none`, `low`, `medium`, `high`, `hidden` und `unknownFutureValue`. Der Wert `hidden` bedeutet, dass der Benutzer oder die Anmeldung nicht für Azure AD Identity Protection aktiviert war. **Hinweis:** Details für diese Eigenschaft sind nur für Azure AD Premium P2-Kunden verfügbar. Für alle anderen Kunden wird `hidden` zurückgegeben. |
| riskLevelDuringSignIn | riskLevel | Risikostufe während der Anmeldung. Die möglichen Werte sind: `none`, `low`, `medium`, `high`, `hidden` und `unknownFutureValue`. Der Wert `hidden` bedeutet, dass der Benutzer oder die Anmeldung nicht für Azure AD Identity Protection aktiviert war. **Hinweis:** Details für diese Eigenschaft sind nur für Azure AD Premium P2-Kunden verfügbar. Für alle anderen Kunden wird `hidden` zurückgegeben. |
| riskState | riskState | Meldet den Status des riskanten Benutzers, der Anmeldung oder einer Risikoerkennung. Die möglichen Werte sind: `none`, `confirmedSafe`, `remediated`, `dismissed`, `atRisk`, `confirmedCompromised`, `unknownFutureValue`. |
| DurationMs |  Dieser Wert wird nicht zugeordnet, sodass das Feld ignoriert werden kann. |
| CallerIpAddress | Die IP-Adresse des Clients, der die Anforderung gestellt hat. | 
| CorrelationId | Die optionale GUID, die vom Client übergeben wird. Dieser Wert kann dabei helfen, clientseitige Vorgänge mit serverseitigen Vorgängen zu korrelieren und Protokolle zu verfolgen, die sich über mehrere Dienste erstrecken. |
| Identity | Die Identität des Tokens, das beim Erstellen der Anforderung angegeben wurde. Dies kann ein Benutzerkonto, ein Systemkonto oder einen Dienstprinzipal sein. |
| Level | Gibt den Typ der Nachricht an. Bei Überwachungen ist es immer *Information*. |
| Location | Gibt den Standort der Anmeldeaktivität an. |
| Properties | Listet alle Eigenschaften auf, die mit Anmeldungen verknüpft sind. Weitere Informationen finden Sie in der [Microsoft Graph-API-Referenz](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/signin). Dieses Schema verwendet zur besseren Lesbarkeit die gleichen Attributnamen wie die Anmelderessource.

## <a name="next-steps"></a>Nächste Schritte

* [Interpret audit logs schema in Azure Monitor](reference-azure-monitor-audit-log-schema.md) (Interpretieren des Überwachungsprotokollschemas in Azure Monitor)
* [Informieren Sie sich ausführlicher über Azure-Diagnoseprotokolle](../../azure-monitor/platform/diagnostic-logs-overview.md)
