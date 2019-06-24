---
title: Allgemeine Parameter und Header
description: Die Parameter und Header, die für alle Vorgänge gelten, die Sie ggf. im Zusammenhang mit Key Vault-Ressourcen ausführen
services: key-vault
author: msmbaldwin
manager: barbkess
tags: azure-resource-manager
ms.service: key-vault
ms.topic: conceptual
ms.date: 01/07/2019
ms.author: mbaldwin
ms.openlocfilehash: 01ac02ca0e449dcac8fcd05450566fd8138acba3
ms.sourcegitcommit: 1289f956f897786090166982a8b66f708c9deea1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/17/2019
ms.locfileid: "64696688"
---
# <a name="common-parameters-and-headers"></a>Allgemeine Parameter und Header

Die folgenden Informationen gelten für alle Vorgänge, die Sie ggf. im Zusammenhang mit Key Vault-Ressourcen ausführen:

- Ersetzen Sie `{api-version}` durch die API-Version im URI.
- Ersetzen Sie `{subscription-id}` durch Ihren Abonnementbezeichner im URI.
- Ersetzen Sie `{resource-group-name}` durch die Ressourcengruppe. Weitere Informationen finden Sie unter „Verwenden von Ressourcengruppen zum Verwalten von Azure-Ressourcen“.
- Ersetzen Sie `{vault-name}` durch Ihren Schlüsseltresornamen im URI.
- Legen Sie den Inhaltstyp auf „application/json“ fest.
- Legen Sie den Autorisierungsheader auf ein JSON-Webtoken fest, das Sie aus Azure Active Directory (AAD) abgerufen haben. Weitere Informationen finden Sie unter [Authentifizieren von Anforderungen des Azure-Ressourcen-Managers](authentication-requests-and-responses.md).

## <a name="common-error-response"></a>Allgemeine Fehlerantwort
Der Dienst gibt anhand von HTTP-Statuscodes an, ob der Vorgang erfolgreich oder fehlerhaft war. Außerdem enthalten Fehler eine Antwort im folgenden Format:

```
   {  
     "error": {  
     "code": "BadRequest",  
     "message": "The key vault sku is invalid."  
     }  
   }  
```

|Elementname | Typ | BESCHREIBUNG |
|---|---|---|
| code | Zeichenfolge | Der Typ des aufgetretenen Fehlers.|
| message | Zeichenfolge | Eine Beschreibung der Ursache des Fehlers. |



## <a name="see-also"></a>Siehe auch
 [Referenz für die Azure Key Vault-REST-API](/rest/api/keyvault/)
