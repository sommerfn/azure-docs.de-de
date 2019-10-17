---
title: Azure API Management-Authentifizierungsrichtlinien | Microsoft Docs
description: Erfahren Sie mehr über die Authentifizierungsrichtlinien, die für die Verwendung in Azure API Management verfügbar sind.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: 061702a7-3a78-472b-a54a-f3b1e332490d
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 11/27/2017
ms.author: apimpm
ms.openlocfilehash: 572d8c4b59622156e8b3aca4565bbc206367f6d4
ms.sourcegitcommit: 12de9c927bc63868168056c39ccaa16d44cdc646
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "72514854"
---
# <a name="api-management-authentication-policies"></a>API Management-Authentifizierungsrichtlinien
Dieses Thema enthält eine Referenz für die folgenden API Management-Richtlinien. Weitere Informationen zum Hinzufügen und Konfigurieren von Richtlinien finden Sie unter [Richtlinien in API Management](https://go.microsoft.com/fwlink/?LinkID=398186).

##  <a name="AuthenticationPolicies"></a> Authentifizierungsrichtlinien

-   [Standardauthentifizierung](api-management-authentication-policies.md#Basic) – Authentifizierung mit einem Back-End-Dienst unter Verwendung der Standardauthentifizierung.

-   [Authentifizierung mit Clientzertifikat](api-management-authentication-policies.md#ClientCertificate) – Authentifizierung mit einem Back-End-Dienst unter Verwendung von Clientzertifikaten.

-   [Authentifizierung mit verwalteter Identität](api-management-authentication-policies.md#ManagedIdentity) – Authentifizierung für den API Management-Dienst mit der [verwalteten Identität](../active-directory/managed-identities-azure-resources/overview.md).

##  <a name="Basic"></a> Standardauthentifizierung
 Verwenden Sie die Richtlinie `authentication-basic` für die Authentifizierung mit einem Back-End-Dienst unter Verwendung der Standardauthentifizierung. Diese Richtlinie legt letztlich den HTTP-Autorisierungsheader auf den Wert fest, der den Anmeldeinformationen in der Richtlinie entspricht.

### <a name="policy-statement"></a>Richtlinienanweisung

```xml
<authentication-basic username="username" password="password" />
```

### <a name="example"></a>Beispiel

```xml
<authentication-basic username="testuser" password="testpassword" />
```

### <a name="elements"></a>Elemente

|NAME|BESCHREIBUNG|Erforderlich|
|----------|-----------------|--------------|
|authentication-basic|Stammelement|Ja|

### <a name="attributes"></a>Attribute

|NAME|BESCHREIBUNG|Erforderlich|Standard|
|----------|-----------------|--------------|-------------|
|username|Gibt den Benutzernamen für die Standardanmeldeinformationen an.|Ja|–|
|password|Gibt das Kennwort für die Standardanmeldeinformationen an.|Ja|–|

### <a name="usage"></a>Verwendung
 Diese Richtlinie kann in den folgenden [Abschnitten](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) und [Bereichen](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) von Richtlinien verwendet werden.

-   **Richtlinienabschnitte**: inbound

-   **Richtlinienbereiche:** alle Bereiche

##  <a name="ClientCertificate"></a> Authentifizieren mit Clientzertifikat
 Verwenden Sie die Richtlinie `authentication-certificate` für die Authentifizierung mit einem Back-End-Dienst unter Verwendung eines Clientzertifikats. Das Zertifikat muss zuerst [in API Management installiert](https://go.microsoft.com/fwlink/?LinkID=511599) werden, es wird durch seinen Fingerabdruck identifiziert.

### <a name="policy-statement"></a>Richtlinienanweisung

```xml
<authentication-certificate thumbprint="thumbprint" certificate-id="resource name"/>
```

### <a name="examples"></a>Beispiele

In diesem Beispiel wird das Clientzertifikat durch seinen Fingerabdruck identifiziert.
```xml
<authentication-certificate thumbprint="CA06F56B258B7A0D4F2B05470939478651151984" />
```
In diesem Beispiel wird das Clientzertifikat durch seinen Ressourcennamen identifiziert.
```xml  
<authentication-certificate certificate-id="544fe9ddf3b8f30fb490d90f" />  
```  

### <a name="elements"></a>Elemente  
  
|NAME|BESCHREIBUNG|Erforderlich|  
|----------|-----------------|--------------|  
|authentication-certificate|Stammelement|Ja|  
  
### <a name="attributes"></a>Attribute  
  
|NAME|BESCHREIBUNG|Erforderlich|Standard|  
|----------|-----------------|--------------|-------------|  
|thumbprint|Der Fingerabdruck für das Clientzertifikat.|Entweder `thumbprint` oder `certificate-id` muss vorhanden sein.|–|  
|certificate-id|Der Zertifikatressourcenname.|Entweder `thumbprint` oder `certificate-id` muss vorhanden sein.|–|  
  
### <a name="usage"></a>Verwendung  
 Diese Richtlinie kann in den folgenden [Abschnitten](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) und [Bereichen](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) von Richtlinien verwendet werden.  
  
-   **Richtlinienabschnitte**: inbound  
  
-   **Richtlinienbereiche:** alle Bereiche  

##  <a name="ManagedIdentity"></a> Authentifizierung mit verwalteter Identität  
 Verwenden Sie die `authentication-managed-identity`-Richtlinie, um sich mit der verwalteten Identität des API Management-Diensts bei einem Back-End-Dienst zu authentifizieren. Diese Richtlinie verwendet im Grunde die verwaltete Identität, um aus Azure Active Directory ein Zugriffstoken für den Zugriff auf die angegebene Ressource abzurufen. Wenn das Token erfolgreich abgerufen wurde, legt die Richtlinie den Wert des Tokens unter Verwendung des Schemas `Bearer` im Header `Authorization` fest.
  
### <a name="policy-statement"></a>Richtlinienanweisung  
  
```xml  
<authentication-managed-identity resource="resource" output-token-variable-name="token-variable" ignore-error="true|false"/>  
```  
  
### <a name="example"></a>Beispiel  
#### <a name="use-managed-identity-to-authenticate-with-a-backend-service"></a>Verwenden der verwalteten Identität für die Authentifizierung bei einem Back-End-Dienst
```xml  
<authentication-managed-identity resource="https://graph.windows.net"/> 
```
```xml  
<authentication-managed-identity resource="https://management.azure.com/"/> <!--Azure Resource Manager-->
```
```xml  
<authentication-managed-identity resource="https://vault.azure.net"/> <!--Azure Key Vault-->
```
```xml  
<authentication-managed-identity resource="https://servicebus.azure.net/"/> <!--Azure Service Busr-->
```
```xml  
<authentication-managed-identity resource="https://storage.azure.com/"/> <!--Azure Blob Storage-->
```
```xml  
<authentication-managed-identity resource="https://database.windows.net/"/> <!--Azure SQL-->
```
  
#### <a name="use-managed-identity-in-send-request-policy"></a>Verwenden der verwalteten Identität in einer Richtlinie vom Typ „send-request“
```xml  
<send-request mode="new" timeout="20" ignore-error="false">
    <set-url>https://example.com/</set-url>
    <set-method>GET</set-method>
    <authentication-managed-identity resource="ResourceID"/>
</send-request>
```

### <a name="elements"></a>Elemente  
  
|NAME|BESCHREIBUNG|Erforderlich|  
|----------|-----------------|--------------|  
|authentication-managed-identity |Stammelement|Ja|  
  
### <a name="attributes"></a>Attribute  
  
|NAME|BESCHREIBUNG|Erforderlich|Standard|  
|----------|-----------------|--------------|-------------|  
|resource|Eine Zeichenfolge. Der App-ID-URI der Ziel-Web-API (geschützte Ressource) in Azure Active Directory.|Ja|–|  
|output-token-variable-name|Eine Zeichenfolge. Name der Kontextvariablen, die den Tokenwert als Objekttyp erhält `string`. |Nein|–|  
|ignore-error|Boolesch. Bei Festlegung auf `true` wird die Richtlinienpipeline auch dann weiter ausgeführt, wenn kein Zugriffstoken abgerufen wird.|Nein|false|  
  
### <a name="usage"></a>Verwendung  
 Diese Richtlinie kann in den folgenden [Abschnitten](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) und [Bereichen](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) von Richtlinien verwendet werden.  
  
-   **Richtlinienabschnitte**: inbound  
  
-   **Richtlinienbereiche:** alle Bereiche

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zur Verwendung von Richtlinien finden Sie unter:

+ [Richtlinien in Azure API Management](api-management-howto-policies.md)
+ [Transform and protect your API](transform-api.md) (Transformieren und Schützen von APIs)
+ Unter [Richtlinien für die API-Verwaltung](api-management-policy-reference.md) finden Sie eine komplette Liste der Richtlinienanweisungen und der zugehörigen Einstellungen.
+ [API Management policy samples](policy-samples.md) (API Management-Richtlinienbeispiele)
