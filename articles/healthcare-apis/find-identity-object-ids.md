---
title: 'Ermitteln von Identitätsobjekt-IDs für die Authentifizierung: Azure API for FHIR'
description: In diesem Artikel wird erläutert, wie Sie die Identitätsobjekt-IDs ausfindig machen, die zum Konfigurieren der Authentifizierung für Azure API for FHIR erforderlich sind.
services: healthcare-apis
author: hansenms
ms.service: healthcare-apis
ms.topic: conceptual
ms.date: 02/07/2019
ms.author: mihansen
ms.openlocfilehash: b120b21a89a7105a25ba610402da99f9dce4b7e1
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "60194746"
---
# <a name="find-identity-object-ids-for-authentication-configuration"></a>Ermitteln von Identitätsobjekt-IDs für die Authentifizierungskonfiguration

In diesem Artikel erfahren Sie, wie Sie Identitätsobjekt-IDs finden, die zum Konfigurieren der Liste mit zulässigen Identitätsobjekt-IDs für Azure API for FHIR erforderlich sind.

Der vollständig verwaltete Dienst Azure API for FHIR&reg; ist so konfiguriert, dass der Zugriff nur für eine vordefinierte Liste mit Identitätsobjekt-IDs zugelassen wird. Wenn eine Anwendung oder ein Benutzer versucht auf die FHIR-API zuzugreifen, muss ein Bearertoken angegeben werden. Dieses Bearertoken enthält bestimmte Ansprüche (Felder). Damit der Zugriff auf die FHIR-API erteilt werden kann, muss das Token den richtigen Aussteller (`iss`), die richtige Zielgruppe (`aud`) und eine Objekt-ID (`oid`) aus einer Liste mit zulässigen Objekt-IDs enthalten. Eine Identitätsobjekt-ID ist entweder die Objekt-ID eines Benutzers oder ein Dienstprinzipal in Azure Active Directory.

## <a name="configure-list-of-allowed-object-ids"></a>Konfigurieren der Liste mit zulässigen Objekt-IDs

Beim Erstellen einer neuen Azure API for FHIR-Instanz können Sie eine Liste der zulässigen Objekt-IDs konfigurieren:

![Konfigurieren zulässiger Objekt-IDs](media/quickstart-paas-portal/configure-allowed-oids.png)

Diese Objekt-IDs können entweder IDs für bestimmte Benutzer oder Dienstprinzipale in Azure Active Directory sein.

## <a name="find-user-object-id-using-powershell"></a>Ermitteln der Benutzerobjekt-ID mithilfe von PowerShell

Ist ein Benutzer mit dem Benutzernamen `myuser@consoso.com` vorhanden, können Sie das `ObjectId`-Element des Benutzers mithilfe des folgenden PowerShell-Befehls ermitteln:

```azurepowershell-interactive
$(Get-AzureADUser -Filter "UserPrincipalName eq 'myuser@consoso.com'").ObjectId
```

Alternativ können Sie die Azure-Befehlszeilenschnittstelle verwenden:

```azurecli-interactive
az ad user show --upn-or-object-id myuser@consoso.com | jq -r .objectId
```

## <a name="find-service-principal-object-id-using-powershell"></a>Ermitteln einer Dienstprinzipalobjekt-ID mithilfe von Azure PowerShell

Wenn Sie eine [Dienstclient-App](register-service-azure-ad-client-app.md) registriert haben und diesem Dienstclient den Zugriff auf Azure API for FHIR erlauben möchten, können Sie die Objekt-ID für den Clientdienstprinzipal mit dem folgenden PowerShell-Befehl ermitteln:

```azurepowershell-interactive
$(Get-AzureADServicePrincipal -Filter "AppId eq 'XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX'").ObjectId
```

Dabei steht `XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX` für die Anwendungs-ID des Dienstclients. Alternativ können Sie das `DisplayName`-Element des Dienstclients verwenden:

```azurepowershell-interactive
$(Get-AzureADServicePrincipal -Filter "DisplayName eq 'testapp'").ObjectId
```

Bei Nutzung der Azure-Befehlszeilenschnittstelle können Sie Folgendes verwenden:

```azurecli-interactive
az ad sp show --id XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX | jq -r .objectId
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie gelernt, wie Sie die Identitätsobjekt-IDs ermitteln, die zum Konfigurieren der Identitäten erforderlich sind, die auf eine Azure API for FHIR-Instanz zugreifen dürfen. Als Nächstes stellen Sie eine vollständig verwaltete Azure API for FHIR-Instanz bereit:
 
>[!div class="nextstepaction"]
>[Bereitstellen von Open Source-FHIR-Servern](fhir-paas-portal-quickstart.md)